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
We can use to ssh terminal as dasith user , in one terminal we will run program and in one terminal we wi

  
