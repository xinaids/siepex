###

https://www.zabbix.com/br/download

# server ssh
sudo apt install openssh-server<br>

# net tools 
apt install net-tools<br>

wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu24.04_all.deb<br>

dpkg -i zabbix-release_7.0-2+ubuntu24.04_all.deb<br>

apt update<br>


### INSTALANDO ZABBIX SERVER, FRONTEND E AGENT
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mysql-server<br>

mysql -u root -p (mateus123)<br>
create database zabbix character set utf8mb4 collate utf8mb4_bin;<br>
create user zabbix@localhost identified by 'suasenha';<br>
grant all privileges on zabbix.* to zabbix@localhost;<br>
set global log_bin_trust_function_creators = 1;<br>
quit

### 

zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix<br>

mysql -uroot -p<br>
set global log_bin_trust_function_creators = 0;<br>
quit;<br>

### CONFIGURANDO O BANCO DE DADOS
Edite o arquivo: /etc/zabbix/zabbix_server.conf<br>
DBPassword=suasenha<br>

### INICIAR OS SERVIÇOS E HABILITAR INICIALIZAÇÃO AUTOMATICA
systemctl restart zabbix-server zabbix-agent apache2<br>
systemctl enable zabbix-server zabbix-agent apache2<br>

https://ipdeconfig/zabbix <br>

usuario/senha <br>
Admin <br>
zabbix <br>


### INSTALANDO UM AGENTE

https://relatosti.com.br/artigo/instalando-o-zabbix-agent-no-linux-ubuntu-debian-e-centos&id=200 <br>

 
sudo su <br>
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb <br>
dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb <br> 
apt-get update<br>
apt-get upgrade <br>
apt-get install zabbix-agent <br>
systemctl restart zabbix-agent<br>
systemctl enable zabbix-agent<br>

nano /etc/zabbix/zabbix_agentd.conf<br>
Server=IP address of Zabbix Server<br>
Hostname=Hostname of client PC<br>

systemctl restart zabbix-agent<br>
systemctl enable zabbix-agent<br>
systemctl status zabbix-agent<br>

### adicionar agente

Monitoring -> Hosts -> Create Host<br>
