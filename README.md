# BananaPI-M3-CentOS-plus-ownCloud
This repo describes how to install PHP 7.0 and ownCloud on BananaPI M3

## Installing **Apache HTTP Server**

    # yum -y install httpd links
    # systemctl enable httpd.service
    # systemctl restart httpd.service

Open HTTP port:

    # firewall-cmd --add-service http --permanent
    # firewall-cmd --reload

## Installing **MySQL**

    # yum -y install mariadb-server mariadb
    # systemctl start mariadb.service
    # systemctl enable mariadb.service
    # systemctl restart httpd.service
    # mysql_secure_installation

Log into MySQL with the administrative account:

    # mysql -u root -p

    mysql> CREATE DATABASE owncloud;
    mysql> GRANT ALL ON owncloud.* to 'owncloud'@'localhost' IDENTIFIED BY 'my_strong_password:)';
    mysql> FLUSH PRIVILEGES;
    mysql> exit

## Installing PHP 7.0 (Or any other version)

Install dependencies:

    # yum install -y wget gcc gcc-c++ libicu libicu-devel libxml2 libxml2-devel pkgconfig openssl-devel zip bzip2 bzip2-devel bzip2-libs libpng-devel libpng-devel libjpeg-devel libXpm-devel freetype-devel gmp-devel libmcrypt-devel mariadb-devel aspell-devel recode-devel httpd-devel autoconf curl curl-devel unixODBC unixODBC-devel

Download [php](http://php.net/downloads.php). For example:

    # wget http://de2.php.net/get/php-7.0.29.tar.bz2/from/this/mirror -O /opt/php-7.0.29.tar.bz2
    # cd /opt/

Extract the archive and compile:

    # tar -xvjf php-7.0.29.tar.bz2
    # cd /opt/php-7.0.29

    # ./configure --prefix=$HOME/php7/usr \
        --with-config-file-path=$HOME/php7/usr/etc \
        --enable-mbstring \
        --enable-zip \
        --with-ldap \
        --enable-soap \
        --enable-bcmath \
        --enable-pcntl \
        --enable-ftp \
        --enable-exif \
        --enable-calendar \
        --enable-sysvmsg \
        --enable-sysvsem \
        --enable-sysvshm \
        --enable-wddx \
        --with-curl \
        --with-mcrypt \
        --enable-intl \
        --with-iconv \
        --with-gmp \
        --with-pspell \
        --with-gd \
        --with-jpeg-dir=/usr \
        --with-png-dir=/usr \
        --with-zlib-dir=/usr \
        --with-xpm-dir=/usr \
        --with-freetype-dir=/usr \
        --enable-gd-native-ttf \
        --enable-gd-jis-conv \
        --with-openssl \
        --with-pdo-mysql=/usr \
        --with-gettext=/usr \
        --with-zlib=/usr \
        --with-bz2=/usr \
        --with-recode=/usr \
        --with-mysqli=/usr/bin/mysql_config \
        --with-apxs2 \
        --with-mhash \
        --with-pdo-odbc=unixODBC,/usr/ \
        --with-pdo-mysql \
        --with-mcrypt

    # make
    # make install
    # cp /opt/php-7.0.29/php.ini-development /usr/local/lib

Edit the file ```/etc/httpd/conf/httpd.conf``` and push the following text to the end of the file:

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

    # systemctl restart httpd.service
    
## Installing **ownCloud**

    # if [ ! -d /var/www/html ]; then mkdir /var/www/html; fi;
    # cd /var/www/html
    # wget https://dl-oo.owncloud.org/download/community/setup-owncloud.php -O setup-owncloud.php
    # php setup-owncloud.php
    # chmod -R 777 /var/www/html/owncloud
    
Then go to ```http://bananapi_ip/setup-owncloud.php``` and finish setup.
