RECOMMENDATIONS
     
    KERNEL: 4.18.0-147.5.1.el8_1.x86_64 or later.
    DISTRO: "Red Hat Enterprise Linux" or "CentOS".
    VERSION: "8.x" 
    ZABBIX_VERSION: 4.0.18 # latest LTS realese.
    DATABASE_VERSION: Ver 15.1 Distrib 10.3.17-MariaDB


MANUAL CONFIGURATION 

---

### Zabbix Setup




 - Update all your packages.
   
       sudo yum update


--- 

 - Reboot to make sure that any kernel fix or update will be used.
  
       sudo reboot

---


- Make sure that selinux is disabled.
          
      sudo setenforce 0 

---

 - Now add zabbix LTS repository.
    
       sudo rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/8/x86_64/zabbix-release-4.0-2.el8.noarch.rpm
 
       sudo yum clean all

---

 - Install all packages that will be necessary.
 
       sudo yum install -y zabbix-server-mysql zabbix-web-mysql zabbix-agent mariadb-server zabbix-get

---

 - Start mariadb service.
 
       sudo systemctl start mariadb

---

 - Open mysql shell.
 
       sudo mysql
 
     - Create zabbix database and user.
       
            mysql > create database zabbix character set utf8 collate utf8_bin;
  
            mysql > grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
  
            myql > quit; 
            
     - You should change <'zabbix'> for a much more secure password.
     
     - Write down your database password for future use.

---

 - Now open the fallowing file.

       sudo vi /etc/my.cnf.d/mariadb-server.cnf 
  
     - Under [mysqld] append this lines.
 
           default_storage_engine=MyISAM
 
           innodb_strict_mode=0
           
           
---

 - Now import zabbix schemma.
 
       sudo systemctl restart mariadb
       
       sudo zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | sudo mysql  zabbix 
       
       

---

 - Echo your database password  to zabbix_sever.conf.
 
       sudo echo 'DBPassword=zabbix' | sudo tee -a  /etc/zabbix/zabbix_server.conf
       
      - Replace '...=zabbix' and add your database passord.

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
 
       sudo systemctl start zabbix-agent

---

 - Enable services during boot time.
 
       sudo systemctl enable httpd zabbix-server zabbix-agent mariadb

---

 - Go to the web interface. 
 
       firefox http://your-zabbix-server-ip/zabbix

     - Fallow this guide.
 
               https://www.zabbix.com/documentation/4.0/manual/installation/install#installing_frontend

---

 - First Login, Zabbix default user and password.
   
       - Admin
       - zabbix

---
 
 - After first login. 
    
        - Go to Administration.
        - Then click on Users.
        - Click on the Admin user.
        - Go on Passoword and click - Change Password.
        - Write your new password down, for future use.

---

 #### Important !!

- We must make sure that selinux is disable on boot time.
    
      sudo sed -i 's,SELINUX=enforcing,SELINUX=permissive,' /etc/selinux/config

- Now reboot the machine and make sure that everything is running before we start setting up grafana.


****


### Grafana Setup



 -  Go to:
 
        https://grafana.com/docs/grafana/latest/installation/rpm/
 
 
 ---
 
 -  Copy the fallowing lines.

        [grafana]  
        name=grafana  
        baseurl=https://packages.grafana.com/oss/rpm  
        repo_gpgcheck=1  
        enabled=1  
        gpgcheck=1  
        gpgkey=https://packages.grafana.com/gpg.key  
        sslverify=1  
        sslcacert=/etc/pki/tls/certs/ca-bundle.crt  

---

 -  Paste on.

        /etc/yum.repos.d/grafana.repo

---
 
 -  Install grafana.
    
        sudo yum install -y grafana

---
  
 -  Start grafana-server.
 
        sudo systemctl start grafana-server
        

--- 

 -  Log into the web console.
    
        http://your-zabbix-server-ip:3000
        
        -   User: admin
        -   Password : admin
        
        -  Change your password. 
 
 
---

 -  Install all essential plugins. 
    
        sudo grafana-cli plugins install alexanderzobnin-zabbix-app

        sudo grafana-cli plugins install grafana-clock-panel
    
        sudo grafana-cli plugins install grafana-piechart-panel


---
 
 
 -  Restart grafana-server.
  
        sudo systemctl restart grafana-server
        
---
    
    
 -  Activate zabbix plugin.
 
        http://your-zabbix-server-ip:3000/plugins

        - click on the zabbbix plugin.
        
        - click on the enable button.
        
        
---

 - Import data source.
     
       http://your-zabbix-server-ip:3000/datasources/edit/1/?gettingstarted
       
      - api url.
         
               http://localhost/zabbix/api_jsonrpc.php
 
---
 
 -  Import  dashboard.
   
        http://your-zabbix-server-ip:3000/dashboard/import
        
        - Click on the  [ Upload .json file ] button. 
        
        - select:
        
                zafrana/basic_linux_monitor.json

--- 

- Now you should be able to see data of your own server, provides you have zabbix-agent running.



