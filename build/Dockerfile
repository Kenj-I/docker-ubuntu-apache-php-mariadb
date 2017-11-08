FROM php:7.1-apache
# FROM php:5.6-apache

RUN apt-get update

# タイムゾーンの変更
ENV TZ Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 言語設定
RUN apt-get install -y locales
RUN locale-gen ja_JP.UTF-8
RUN apt-get install -y task-japanese
RUN locale-gen en_US.UTF-8
ENV LANG=C.UTF-8
ENV LANGUAGE=en_US:en



# Manually set up the apache environment variables
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data

# Update the default apache site with the config we created.
COPY apache2.conf /etc/apache2/apache2.conf
COPY dir.conf /etc/apache2/mods-available/dir.conf
COPY vhost.conf /etc/apache2/sites-enabled/000-default.conf


RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get update \
	&& docker-php-ext-install mbstring pdo pdo_mysql mysqli bcmath posix bcmath posix zip

RUN apt-get update && apt-get install -y sendmail

# mod_rewrite on
RUN a2enmod rewrite

# Expose apache.
EXPOSE 80
