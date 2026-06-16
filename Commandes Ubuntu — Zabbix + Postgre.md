# *Commandes Ubuntu — Zabbix + PostgreSQL*

### 

### Installation PostgreSQL :



sudo apt install postgresql postgresql-contrib

sudo systemctl status postgresql

sudo -u postgres psql



### Créer la base Zabbix (dans psql):



CREATE DATABASE zabbix;

CREATE USER zabbix WITH PASSWORD 'zab123p';

GRANT ALL PRIVILEGES ON DATABASE zabbix TO zabbix;



### Installer Zabbix (version PostgreSQL) :



sudo apt install zabbix-server-pgsql zabbix-frontend-php zabbix-apache-conf zabbix-agent -y

sudo apt install zabbix-sql-scripts -y

### 

### Configurer PostgreSQL :



sudo nano /etc/postgresql/12/main/pg\_hba.conf



### Importer le schéma SQL :



sudo -u postgres zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | psql -U zabbix -d zabbix

psql -U zabbix -d zabbix -h 127.0.0.1 -c "\\dt"



### Configurer Zabbix Server :



sudo nano /etc/zabbix/zabbix\_server.conf

\# Ajouter :

\# DBName=zabbix

\# DBUser=zabbix

\# DBPassword=zab123p

\# AllowUnsupportedDBVersions=1  <-- si PostgreSQL < 13



### Redémarrer les services :



sudo systemctl restart zabbix-server zabbix-agent apache2



### Vérifications réseau:



ping 192.168.56.1

sudo netstat -anp | grep :10050

sudo apt install zabbix-get

zabbix\_get -s 192.168.56.1 -p 10050 -k "system.hostname"













