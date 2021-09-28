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
## seeing contents of run.yml reveals that `copy_links=yes` is there which means that when content is copied from `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` , it also copies files/directories  that are linked with these directories like if `/home/luis/.ssh` is linked with  `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`etc.
seeing contents of run.yml reveals that `copy_links=yes` is there which means that when content is copied from `/var/lib/tomcat9/webapps/ROOT/admin/dashboard` , it also copies files/directories  that are linked with these directories like if `/home/luis/.ssh` is linked with  `/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads`etc.





