# server ssh
sudo apt install openssh-server

# net tools 
apt install net-tools

### atualizar tudo e todos no sistema
apt update && apt upgrade

### instalacao de pacotes para glpi
apt install -y apache2 php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,redis,bz2} libapache2-mod-php php-soap php-cas
apt install -y mariadb-server

### instalcao do sql banco de dados
mysql_secure_installation
    unix socket = y
    change root password = y (siepexifrs123)
    anonymous = y
    remote login = y,y,y,y
    


### adicionar timezona (fuso horario)
mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql mysql

### criar usuario e database no banco de dados
mysql -uroot -pmysql
CREATE DATABASE glpi;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'yourstrongpassword';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
GRANT SELECT ON `mysql`.`time_zone_name` TO 'glpi'@'localhost';
FLUSH PRIVILEGES;

### instalacao do glpi
cd /var/www/html
wget https://github.com/glpi-project/glpi/releases/download/10.0.15/glpi-10.0.15.tgz
tar -xvzf glpi-10.0.15.tgz

### hierarquia dos dados
/etc/glpi : for the files of configuration of GLPI (config_db.php, config_db_slave.php) ;
/var/www/html/glpi : for the source code of GLPI (in reading only), served by Apache;
/var/lib/glpi : for the variable files of GLPI (session, uploaded documents, cache, cron, plugins, …);
/var/log/glpi : for the log files of GLPI.

### downstream, indicar onde o diretorio do glpi está instalado # criar pasta
nano /var/www/html/glpi/inc/downstream.php

### config file colar isso aqui
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
require_once GLPI_CONFIG_DIR . '/local_define.php';
}

### mover diretorios
mv /var/www/html/glpi/config /etc/glpi
mv /var/www/html/glpi/files /var/lib/glpi
mv /var/lib/glpi/_log /var/log/glpi

### definir arquivos locais
nano /etc/glpi/local_define.php

### colar isso
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi');
define('GLPI_DOC_DIR', GLPI_VAR_DIR);
define('GLPI_CRON_DIR', GLPI_VAR_DIR . '/_cron');
define('GLPI_DUMP_DIR', GLPI_VAR_DIR . '/_dumps');
define('GLPI_GRAPH_DIR', GLPI_VAR_DIR . '/_graphs');
define('GLPI_LOCK_DIR', GLPI_VAR_DIR . '/_lock');
define('GLPI_PICTURE_DIR', GLPI_VAR_DIR . '/_pictures');
define('GLPI_PLUGIN_DOC_DIR', GLPI_VAR_DIR . '/_plugins');
define('GLPI_RSS_DIR', GLPI_VAR_DIR . '/_rss');
define('GLPI_SESSION_DIR', GLPI_VAR_DIR . '/_sessions');
define('GLPI_TMP_DIR', GLPI_VAR_DIR . '/_tmp');
define('GLPI_UPLOAD_DIR', GLPI_VAR_DIR . '/_uploads');
define('GLPI_CACHE_DIR', GLPI_VAR_DIR . '/_cache');
define('GLPI_LOG_DIR', '/var/log/glpi');

### dar permissoes nas pastas e arquivos em geral

chown root:root /var/www/html/glpi/ -R
chown www-data:www-data /etc/glpi -R
chown www-data:www-data /var/lib/glpi -R
chown www-data:www-data /var/log/glpi -R
chown www-data:www-data /var/www/html/glpi/marketplace -Rf
find /var/www/html/glpi/ -type f -exec chmod 0644 {} \;
find /var/www/html/glpi/ -type d -exec chmod 0755 {} \;
find /etc/glpi -type f -exec chmod 0644 {} \;
find /etc/glpi -type d -exec chmod 0755 {} \;
find /var/lib/glpi -type f -exec chmod 0644 {} \;
find /var/lib/glpi -type d -exec chmod 0755 {} \;
find /var/log/glpi -type f -exec chmod 0644 {} \;
find /var/log/glpi -type d -exec chmod 0755 {} \;

chown root:root /var/www/html/glpi/ -R
# Muda o dono e o grupo de todos os arquivos e diretórios dentro de /var/www/html/glpi/ recursivamente (-R) para "root".

chown www-data:www-data /etc/glpi -R
# Muda o dono e o grupo de todos os arquivos e diretórios dentro de /etc/glpi/ recursivamente (-R) para "www-data".

chown www-data:www-data /var/lib/glpi -R
# Muda o dono e o grupo de todos os arquivos e diretórios dentro de /var/lib/glpi/ recursivamente (-R) para "www-data".

chown www-data:www-data /var/log/glpi -R
# Muda o dono e o grupo de todos os arquivos e diretórios dentro de /var/log/glpi/ recursivamente (-R) para "www-data".

chown www-data:www-data /var/www/html/glpi/marketplace -Rf
# Muda o dono e o grupo de todos os arquivos e diretórios dentro de /var/www/html/glpi/marketplace/ recursivamente (-R) para "www-data". O "-f" faz com que erros sejam suprimidos (silenciosamente).

find /var/www/html/glpi/ -type f -exec chmod 0644 {} \;
# Encontra todos os arquivos (-type f) dentro de /var/www/html/glpi/ e executa o comando chmod 0644, que define as permissões para leitura e escrita para o proprietário, e leitura para o grupo e outros.

find /var/www/html/glpi/ -type d -exec chmod 0755 {} \;
# Encontra todos os diretórios (-type d) dentro de /var/www/html/glpi/ e executa o comando chmod 0755, que define permissões de leitura, escrita e execução para o proprietário, e leitura e execução para grupo e outros.

find /etc/glpi -type f -exec chmod 0644 {} \;
# Encontra todos os arquivos (-type f) dentro de /etc/glpi/ e executa o comando chmod 0644 para ajustar permissões.

find /etc/glpi -type d -exec chmod 0755 {} \;
# Encontra todos os diretórios (-type d) dentro de /etc/glpi/ e ajusta permissões para 0755.

find /var/lib/glpi -type f -exec chmod 0644 {} \;
# Encontra todos os arquivos (-type f) dentro de /var/lib/glpi/ e ajusta permissões para 0644.

find /var/lib/glpi -type d -exec chmod 0755 {} \;
# Encontra todos os diretórios (-type d) dentro de /var/lib/glpi/ e ajusta permissões para 0755.

find /var/log/glpi -type f -exec chmod 0644 {} \;
# Encontra todos os arquivos (-type f) dentro de /var/log/glpi/ e ajusta permissões para 0644.

find /var/log/glpi -type d -exec chmod 0755 {} \;
# Encontra todos os diretórios (-type d) dentro de /var/log/glpi/ e ajusta permissões para 0755.

Resumindo o 0644:

Proprietário: Leitura e escrita (rw-).
Grupo: Somente leitura (r--).
Outros: Somente leitura (r--).

Resumindo o 0755:

Proprietário: Leitura, escrita e execução (rwx).
Grupo: Leitura e execução (r-x).
Outros: Leitura e execução (r-x).

### web server, criar arquivo no seguinte diretorio
nano /etc/apache2/sites-available/glpi.conf


# colar isso
# Start of the VirtualHost configuration for port 80

<VirtualHost *:80>
    ServerName localhost
    # Specify the server's hostname
    DocumentRoot /var/www/html/glpi/public
    # The directory where the website's files are located
    # Start of a Directory directive for the website's directory
    <Directory /var/www/html/glpi/public>
        Require all granted
        # Allow all access to this directory
        RewriteEngine On
        # Enable the Apache rewrite engine
        # Ensure authorization headers are passed to PHP.
        # Some Apache configurations may filter them and break usage of API, CalDAV, ...
        RewriteCond %{HTTP:Authorization} ^(.+)$
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        # Redirect all requests to GLPI router, unless the file exists.
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
    # End of the Directory directive for /var/www/glpi/public
</VirtualHost>

# End of the VirtualHost configuration for port 80


ServerName if you have a public URL, you can type it here
DocumentRoot if you will store GLPI in a different page, change it too.
After the Virtual Host file is created you should disable the default apache site configuration, enable the rewrite module and reload the new vhost file.

### mais comandos

a2dissite 000-default.conf # Disable default apache site
a2enmod rewrite # enable the rewrite module
a2ensite glpi.conf # enable the new apache virtual host settings for your glpi instance
systemctl restart apache2

###

For GLPI to work properly it is recommended to change the following parameters on your php.ini file

Open the php.ini file

nano /etc/php/8.3/apache2/php.ini
Change the following parameters

upload_max_filesize = 20M Maximum size for uploaded files is set to 20 megabytes.
post_max_size = 20M Maximum size for POST data (e.g., form submissions) is also set to 20 megabytes.
max_execution_time = 60 Maximum execution time for a PHP script is set to 60 seconds.
max_input_vars = 5000 Maximum number of input variables (e.g., form fields) a script can accept is 5000.
memory_limit = 256M The maximum amount of memory a single PHP script can use is 256 megabytes.
session.cookie_httponly = On Sets the "HttpOnly" attribute for session cookies
date.timezone = America/Sao_Paulo Sets the default timezone for PHP to yours.
To add your timezone, please refer to the official list of supported timezones for PHP

Entrar no site pelo IP

localhost
usuario / senha

rm glpi/install/install.php


### INSTALACAO DE AGENTES

https://github.com/glpi-project/glpi-agent/releases/tag/1.7.1

http://127.0.0.1:62354/



REFERENCIAS

https://faq.teclib.com/03_knowledgebase/procedures/install_glpi/#3-preparing-files-to-install-glpi
https://www.youtube.com/watch?v=Dc0dy1Z6MyM]
https://glpi-install.readthedocs.io/en/latest/prerequisites.html
https://faq.teclib.com/03_knowledgebase/procedures/install_glpi/