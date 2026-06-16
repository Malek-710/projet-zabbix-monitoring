### *Commandes Ubuntu — Installation* *Zabbix*



#### Étape 1 — Mise à jour système:



usermod -aG sudo addali

sudo apt update



#### Étape 2 — Installation MariaDB:



sudo apt install mariadb-server -y



#### Étape 3 — Sécuriser MariaDB:



sudo apt install mariadb-server -y

#### 

#### Étape 4 — Créer la base de données Zabbix: 

#### 

sudo mysql

CREATE DATABASE ZABBIX1 CHARACTER SET utf8mb4 COLLATE utf8mb4\_bin;

CREATE USER 'ZABBIX1'@'localhost' IDENTIFIED BY 'zab123';

GRANT ALL PRIVILEGES ON ZABBIX1.\* TO 'ZABBIX1'@'localhost';

FLUSH PRIVILEGES;

EXIT; 

### 

### Étape 5 — Ajouter le dépôt Zabbix:



lsb\_release -rs

wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release\_6.0-1+ubuntu20.04\_all.deb

sudo dpkg -i zabbix-release\_6.0-1+ubuntu20.04\_all.deb

sudo apt update



### Étape 6 — Installer Zabbix:



sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent -y

sudo apt install zabbix-sql-scripts -y

dpkg -l | grep -i Zabbix



### Étape 7 — Importer le schéma SQL



zabbix\_server -V

sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uZABBIX1 -pzab123 ZABBIX1



### Étape 8 — Configurer Zabbix Server:



sudo nano /etc/zabbix/zabbix\_server.conf



### Étape 9 — Configurer PHP:



php --version

sudo nano /etc/zabbix/apache.conf

sudo nano /etc/php/7.4/apache2/conf.d/zabbix.ini



### Étape 10 — Fuseau horaire:

timedatectl

sudo timedatectl set-timezone Europe/Paris



### Étape 11 — Redémarrer les services:



ls /lib/systemd/system/ | grep zabbix

sudo systemctl enable zabbix-server zabbix-agent apache2 mariadb

sudo systemctl restart zabbix-server zabbix-agent apache2 mariadb

sudo systemctl status zabbix-server

sudo systemctl status zabbix-server zabbix-agent apache2 mariadb



### Étape 12 — Récupérer l'IP Ubuntu



hostname -I



### Vérifications réseau :



ping 192.168.56.1

sudo netstat -anp | grep :10050

sudo apt install zabbix-get

zabbix\_get -s 192.168.56.1 -p 10050 -k "system.hostname"







