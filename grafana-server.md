## Grafana Install 


#### Go to: 
 -  https://grafana.com/docs/grafana/latest/installation/rpm/
 
 
 -  Copy the fallowing lines: 
 
 ---
 
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

- Paste on:

       /etc/yum.repos.d/grafana.repo
