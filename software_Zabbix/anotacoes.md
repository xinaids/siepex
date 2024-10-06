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

### revisar a partir daqui

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
