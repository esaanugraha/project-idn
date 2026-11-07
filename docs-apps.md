# Apps (ExpressJS)

## Prerequisite
### Download source code
You can fork on my github [here](https://github.com/IDN-Training/simple-apps)

and you download source code to local (development) and server (deployment)
```
git clone https://github.com/idndevops/simple-apps.git
```
### Download package nodejs
Install nodejs versi 18.x
```
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && sudo apt-get install -y nodejs
```

## Database Installation
### Install package 
Install mariadb server
```
apt install mariadb-server
```
### Listen Server Database
Set Network Database
```
vim /etc/mysql/mariadb.conf.d/50-server.cnf
*di ubah ke network server masing masing
systemctl restart mariadb
```

### Create database, user, and password
Create with mariadb syntax
```
mysql
create database training;
grant all privileges on training.* to 'peserta'@'localhost' identified by 'password';
grant all privileges on training.* to 'peserta'@'%' identified by 'password';
flush privileges;
quit;
```
### Import table to database
Import table to database from backup file
```
mysql -u peserta -p training < database/training.sql
mysql -u peserta -p training -e "show tables;"
```
### Create .env file
Import table to database from backup file
```
DB_NAME = training
DB_HOST = 10.23.0.113
DB_USER = peserta
DB_PASS = password
APP_PORT = 3000
```

## Running Application
Install dependecies apps
```
npm install
```

Running apps
```
npm start
```

## Access Application
You can access on endpoint url
```
http://ip_server:3000
```
