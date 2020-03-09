### Zabbix Setup



---

- Make sure that selinux is disabled.
          
      sudo setenforce 0 

---


 - Update all your packages.
   
       sudo yum check-update
 
       sudo yum update


--- 

 - Reboot to make sure that any kernel fix or update will be used.
  
       sudo reboot

---

 - Now add zabbix LTS repository.
    
       sudo rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/8/x86_64/zabbix-release-4.0-2.el8.noarch.rpm
 
       sudo yum clean all

---

 - Install all packages that will be necessary.
 
       sudo yum install zabbix-server-mysql zabbix-web-mysql zabbix-agent mariadb-server zabbix-get

---

 - Start mariadb service.
 
       sudo systemctl start mariadb

---

 - Open mysql shell.
 
       sudo mysql

---
 
 - Create zabbix database and user.
       
       mysql > create database zabbix character set utf8 collate utf8_bin;
  
       mysql > grant all privileges on zabbix.* to zabbix@localhost identified by 'password';
  
       myql > quit; 

---

 - Now open the fallowing file.

       sudo vi /etc/my.cnf.d/mariadb-server.cnf 
  
     - Under [mysqld] paste 2 lines.
 
           default_storage_engine=My_ISAM
 
           innodb_strict_mode=0
           
           
---

 - Now import zabbix schemma.
 
       sudo zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | sudo mysql  zabbix 
       
       sudo systemctl restart mariadb

---

 - Echo your database password  to zabbix_sever.conf.
 
       sudo echo 'DBPassword=password' | sudo tee -a  /etc/zabbix/zabbix_server.conf

---

 - Start zabbix-server. 
 
       sudo systemctl start zabbix-server

---

 - Echo your timezone to  the front-end config file.
 
       sudo echo 'php_value[date.timezone] = America/Sao_Paulo' | sudo tee -a   /etc/php-fpm.d/zabbix.conf

---

 - Start the httpd server. 
 
       sudo systemctl start httpd 

---

 - Also start our local zabbix-agent. 
 
       sudo systemctl start zabbbix-agent

---

 - Enable services during boot time.
 
       sudo systemctl enable httpd zabbix-server zabbix-agent mariadb

---

 - Go to the web interface. 
 
       firefox http://your-zabbix-server-ip/zabbix

---

 - Fallow this guide.
 
       https://www.zabbix.com/documentation/4.0/manual/installation/install#installing_frontend

---

 - First Login, Zabbix default user and password.
   
       - Admin
       - zabbix

---
 
 - After first login. 
    
        - Go to Administration
        - Then click on Users
        - Click on the Admin user
        - Go on Passoword and click - Change Password
        - Write your new password down, for future use

---

 #### Important !!

- We must make sure that selinux is disable on boot time.
    
      sudo sed -i 's,SELINUX=enforcing,SELINUX=permissive,' /etc/selinux/config

- Now reboot the machine and make sure that everything is running before we start setting up grafana.

