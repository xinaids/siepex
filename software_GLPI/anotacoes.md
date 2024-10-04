# server ssh
sudo apt install openssh-server

# Atualiza Lista de Pacotes
apt update

# PACOTES MANIPULAÇÃO DE ARQUIVOS
apt install -y xz-utils bzip2 unzip curl git

Preparação do Servidor WEB
# Instalar dependências no sistema
apt install -y apache2 libapache2-mod-php php-soap php-cas php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,redis,bz2}

Resolvendo Problema de Acesso WEB ao Diretório
# Criar arquivo com conteúdo
cat > /etc/apache2/conf-available/siepex.conf << EOF
<Directory "/var/www/siepex/glpi/public/">
    AllowOverride All
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php [QSA,L]
    Options -Indexes
    Options -Includes -ExecCGI
    Require all granted
    <IfModule mod_php7.c>
        php_value max_execution_time 600
        php_value always_populate_raw_post_data -1
    </IfModule>
    <IfModule mod_php8.c>
        php_value max_execution_time 600
        php_value always_populate_raw_post_data -1
    </IfModule>
</Directory>
EOF

# Habilitar o módulo rewrite do apache
a2enmod rewrite

# Habilita a configuração criada
a2enconf siepex.conf

# Reinicia o servidor web considerando a nova configuração
systemctl restart apache2


Baixar e Instalar o GLPi
# Criar diretório onde o GLPi será instalado
mkdir /var/www/siepex

# Baixar o sistema GLPi
wget -O- https://github.com/glpi-project/glpi/releases/download/10.0.15/glpi-10.0.15.tgz | tar -zxv -C /var/www/siepex/

# Movendo diretórios "files" e "config" para fora do GLPi 
mv /var/www/siepex/glpi/files /var/www/siepex/
mv /var/www/siepex/glpi/config /var/www/siepex/

# Ajustando código do GLPi para o novo local dos diretórios
sed -i 's/\/config/\/..\/config/g' /var/www/siepex/glpi/inc/based_config.php
sed -i 's/\/files/\/..\/files/g' /var/www/siepex/glpi/inc/based_config.php


Ajustar Permissões de Arquivos
# Ajustar propriedade de arquivos da aplicação GLPi
chown root:root /var/www/siepex/glpi -Rf

# Ajustar propriedade de arquivos files, config e marketplace
chown www-data:www-data /var/www/siepex/files -Rf
chown www-data:www-data /var/www/siepex/config -Rf
chown www-data:www-data /var/www/siepex/glpi/marketplace -Rf

# Ajustar permissões gerais
find /var/www/siepex/ -type d -exec chmod 755 {} \;
find /var/www/siepex/ -type f -exec chmod 644 {} \;

# Criando link simbólico para o sistema GLPi dentro do diretório defalt do apache
ln -s /var/www/siepex/glpi /var/www/html/glpi

Preparando o Serviço SQL
# Instalando o Serviço MySQL
apt install -y mariadb-server

# Criando base de dados
mysql -e "create database siepex_glpi character set utf8"

# Criando usuário
mysql -e "create user 'bancodedados'@'localhost' identified by '123456'"

# Dando privilégios ao usuário
mysql -e "grant all privileges on siepex_glpi.* to 'bancodedados'@'localhost' with grant option";

# Habilitando suporte ao timezone no MySQL/Mariadb
mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql mysql

# Permitindo acesso do usuário ao TimeZone
mysql -e "GRANT SELECT ON mysql.time_zone_name TO 'bancodedados'@'localhost';"

# Forçando aplicação dos privilégios
mysql -e "FLUSH PRIVILEGES;"

# Ver IP
ifconfig

cd /etc/apache2/conf-available/
nano glpi.conf

<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/html/servicedeskbasil/glpi/public

    <Directory /var/www/html/servicedeskbrasil/glpi/public>
        AllowOverride All
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
sudo systemctl status apache2
</VirtualHost>

# Habilitação do módulo rewrite no apache
a2enmod rewrite

# Habilitação da configuração previamente criada
a2enconf glpi.conf

# Reinicialização do servidor web para buscar a nova configuração
service apache2 restart

cd /etc/

CONTINUAR PELA WEB!!!

/glpi