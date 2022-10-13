# MariaDB
Install and start mariaDB

Reference link: https://mariadb.com/resources/blog/how-to-install-mariadb-on-rhel8-centos8/

1. To setup the mariadb repo:

wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
chmod +x mariadb_repo_setup
sudo ./mariadb_repo_setup

wget https://downloads.mariadb.com/MariaDB/mariadb-10.8.5/yum/rhel8-amd64/rpms/MariaDB-client-10.8.5-1.el8.x86_64.rpm
wget https://downloads.mariadb.com/MariaDB/mariadb-10.8.5/yum/rhel8-amd64/rpms/MariaDB-server-10.8.5-1.el8.x86_64.rpm
wget https://downloads.mariadb.com/MariaDB/mariadb-10.8.5/yum/rhel8-amd64/rpms/MariaDB-server-debuginfo-10.8.5-1.el8.x86_64.rpm
wget https://downloads.mariadb.com/MariaDB/mariadb-10.8.5/yum/rhel8-amd64/rpms/MariaDB-shared-10.8.5-1.el8.x86_64.rpm
wget https://downloads.mariadb.com/MariaDB/mariadb-10.8.5/yum/rhel8-amd64/rpms/MariaDB-common-10.8.5-1.el8.x86_64.rpm


2. To resolve dependencies:

sudo yum install perl-DBI libaio libsepol lsof boost-program-options

3. To install MariaDB for RHEL 8:


yum install MariaDB-client-10.8.5-1.el8.x86_64.rpm MariaDB-shared-10.8.5-1.el8.x86_64.rpm MariaDB-server-debuginfo-10.8.5-1.el8.x86_64.rpm MariaDB-server-10.8.5-1.el8.x86_64.rpm MariaDB-common-10.8.5-1.el8.x86_64.rpm


Try checking the status,
service mysql status 

If the service is not found, then try 
yum install MariaDB-server* 

4. Check the MariaDB service status and start:
service mysql status
service mysql start

5. Run upgrade command to check compaitability issues and same will be fixed
mysql_upgrade

6. Login to mysql and check for zabbix DB and its respective tables,
#mysql
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| zabbix             |
+--------------------+
5 rows in set (0.000 sec)

MariaDB [(none)]>
MariaDB [(none)]>
MariaDB [(none)]> use zabbix;
MariaDB [zabbix]> show tables;

Then execute below command:

MariaDB [zabbix]>alter table config row_format = dynamic;

7. Stop the service and update the configuration back to existing
service mysql status
service mysql stop
service mysql status

8. Renaming existing cnf and renaming the newly generated cnf file:
root@zbxdevprxy16:/tmp/pre_upgrade/etc
07:51:19 # cp -p /tmp/pre_upgrade/etc/my.cnf /etc/my_up.cnf

07:51:47 # ls -ltrh *.cnf
-rw-rw-r--. 1 root root 1.8K Feb 22  2019 my_up.cnf
-rw-r--r--. 1 root root 1.8K Sep 26 06:44 my.cnf

mv my.cnf my_new.cnf
mv my_up.cnf my.cnf

07:52:08 # ls -ltrh *.cnf
-rw-rw-r--. 1 root root 1.8K Feb 22  2019 my.cnf
-rw-r--r--. 1 root root  193 Sep 13 14:14 my_new.cnf


Start the service once configurations are updated,
service mysql status
service mysql start
