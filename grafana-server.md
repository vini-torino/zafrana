## Grafana Install 




 -  Go to:
 
        https://grafana.com/docs/grafana/latest/installation/rpm/
 
 
 ---
 
 -  Copy the fallowing lines: 

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

 -  Paste on:

        /etc/yum.repos.d/grafana.repo

---
 
 -  Install grafana:
    
        sudo yum install grafana

---
  
 -  Start grafana-server: 
 
        sudo systemctl start grafana-server
        

--- 

 -  Log into the web console:
    
        http://your-server-ip:3000
        
        -   User: admin
        -   Password : admin
        
        -  Change your password. 
 
 
---

 -  Install all essential plugings by typing this on the terminal: 
    
        sudo grafana-cli plugins install alexanderzobnin-zabbix-app

        sudo grafana-cli plugins install grafana-clock-panel
    
        sudo grafana-cli plugins install grafana-piechart-panel


---
 
 
 -  Restart grafana-server:
  
        sudo systemctl restart grafana-server
        
---
    
    
 -  Activate zabbix plugin:
 
        http://your-server-ip:3000/plugins

        - click on the zabbbix plugin.
        
        - click on the enable button.
        
        
---

 
 -  Import 

