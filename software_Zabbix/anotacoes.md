###

https://www.zabbix.com/br/download

# server ssh
sudo apt install openssh-server

# net tools 
apt install net-tools

wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu24.04_all.deb

dpkg -i zabbix-release_7.0-2+ubuntu24.04_all.deb

apt update


### INSTALANDO ZABBIX SERVER, FRONTEND E AGENT
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mysql-server

mysql -u root -p (mateus123)
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'suasenha';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit

### 

zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

mysql -uroot -p
set global log_bin_trust_function_creators = 0;
quit;

CONFIGURANDO O BANCO DE DADOS
Edite o arquivo: /etc/zabbix/zabbix_server.conf
DBPassword=suasenha

INICIAR OS SERVIÇOS E HABILITAR INICIALIZAÇÃO AUTOMATICA
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

https://ipdeconfig/zabbix

usuario/senha
Admin
zabbix


### INSTALANDO UM AGENTE

https://relatosti.com.br/artigo/instalando-o-zabbix-agent-no-linux-ubuntu-debian-e-centos&id=200


sudo su
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb
dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb
apt-get update
apt-get upgrade
apt-get install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent

nano /etc/zabbix/zabbix_agentd.conf
Server=IP address of Zabbix Server
Hostname=Hostname of client PC

systemctl restart zabbix-agent
systemctl enable zabbix-agent
systemctl status zabbix-agent

### adicionar agente

Monitoring -> Hosts -> Create Host
