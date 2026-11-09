### **install and Config Sonarqube**
=====================================
### **Install Java 21**
```
apt install openjdk-21-jdk openjdk-21-jre -y
sudo update-alternatives --config java
```
### **Install Postgresql**
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
```
### **Config Database**
```
sudo -u postgres psql
CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
ALTER USER sonar WITH SUPERUSER;
\q
```

====================
### **install SonarQube**
sonarqube-installation
```
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-26.10.0.133078.zip
sudo apt install unzip -y 
sudo unzip sonarqube-26.10.0.133078.zip
sudo mv sonarqube-26.10.0.133078 sonarqube

nano /opt/sonarqube/conf/sonar.properties
```
di edit config 
```
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
```

### **Systemd Configuration**
Systemd config
```
sudo useradd -r -s /bin/false sonar
sudo chown -R sonar:sonar /opt/sonarqube

sudo nano /etc/systemd/system/sonarqube.service
```
edit seperti ini
```
===============
[Unit]
Description=SonarQube service
After=network.target postgresql.service

[Service]
Type=forking
User=sonar
Group=sonar
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
Restart=always

[Install]
WantedBy=multi-user.target
```
### reload systemctl
```
======================

sudo systemctl daemon-reload
sudo systemctl enable sonarqube
sudo systemctl start sonarqube
sudo systemctl status sonarqube
```
### cek port 

```
ss -tulpn
* pastikan port 9000 running/listen

```

===================================================
### **install sonar scanner**

```
cd /opt/
sudo wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-8.3.1.16491-linux-x64.zip
rm -R sonarscanner
unzip sonar-scanner-cli-8.3.1.16491-linux-x64.zip
sudo mv sonar-scanner-8.3.1.16491-linux-x64/  /opt/sonarscanner
sudo chmod +x /opt/sonarscanner/bin/sonar-scanner
export PATH=$PATH:$(pwd)/sonarscanner/bin
echo 'export SONAR_SCANNER_HOME=/opt/sonarscanner' | sudo tee /etc/profile.d/sonarscanner.sh
echo 'export PATH=$PATH:$SONAR_SCANNER_HOME/bin' | sudo tee -a /etc/profile.d/sonarscanner.sh
source /etc/profile.d/sonarscanner.sh
which sonar-scanner
sonar-scanner -v
cd
cd Belajar-Devops/simple-apps/
```



