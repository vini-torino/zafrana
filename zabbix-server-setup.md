## Setting up zabbix-server



### Become root
 sudo su



### Make sure that selinux is disabled
 setenforce 0 



### Update all your packages
 dnf check-update
 
 dnf update



### Reboot to make sure that any kernel fix or update will be used
 reboot



### Now add zabbix LTS repository
 rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/8/x86_64/zabbix-release-4.0-2.el8.noarch.rpm
 
 dnf clean all



### Install all packages that will be necessary 
 yum install zabbix-server-mysql zabbix-web-mysql zabbix-agent mariadb-server zabbix-get



### Start mariadb service
 systemctl start mariadb



### Open mysql shell
 mysql



### Create zabbix database and user
  mysql > create database zabbix character set utf8 collate utf8_bin;
  
  mysql > grant all privileges on zabbix.* to zabbix@localhost identified by 'password';
  
  myql > quit; 



### Now open the fallowing file
 vi /etc/my.cnf.d/mariadb-server.cnf 



### Under [mysqld] add the 2 lines
 default_storage_engine=My_ISAM
 
 innodb_strict_mode=0



### Now import zabbix schemma
 zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql  zabbix 
 systemctl restart mariadb



### Echo your database password  to zabbix_sever.conf 
 echo 'DBPassword=password' >> /etc/zabbix/zabbix_server.conf


### Start zabbix-server 
 systemctl start zabbix-server



### Echo your timezone to  the front-end config file
 echo 'php_value[date.timezone] = America/Sao_Paulo' >>  /etc/php-fpm.d/zabbix.conf



### Start the httpd server 
 systemctl start httpd 



### Also start our local zabbix-agent 
 systemctl start zabbbix-agent



### Enable our 4 main services during boot time
 systemctl enable httpd zabbix-server zabbix-agent mariadb



### Go to the web interface 
 firefox http://your_zabbix_server_ip/zabbix



### Fallow this guide
 https://www.zabbix.com/documentation/4.0/manual/installation/install#installing_frontend



### First login
Zabbix default user and password
- Admin
- zabbix

### After first login 
 - Go to Administration
 - Then click on Users
 - Click on the Admin user
 - Go on Passoword and click - Change Password
 - Write your new password down, for future use



## Important !!
- We must make sure that selinux is disable on boot time

  sed -i 's,SELINUX=enforcing,SELINUX=permissive,' /etc/selinux/config

Now reboot the machine and make sure that everything is running before we start setting up grafana 

