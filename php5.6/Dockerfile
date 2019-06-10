FROM php:5.6-apache
MAINTAINER antlg

# Install any custom system requirements here
# Needed for the php extensions we enable below
RUN apt-get update \
    && apt-get install -y -q --no-install-recommends \
        libfreetype6 \
        libjpeg62-turbo \
        libxpm4 \
        libpng16-16 \
        libxslt1.1 \
        libmemcachedutil2 \
        imagemagick \
		libfreetype6-dev \
        libjpeg62-turbo-dev \
        libxpm-dev \
        libpng-dev \
        libicu-dev \
        libxslt1-dev \
        libmemcached-dev \
        libxml2-dev \
	    libicu-dev \
        unzip \
		wget \
        git \
		iputils-ping \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ --with-xpm-dir=/usr/include/ --enable-gd-jis-conv

RUN docker-php-ext-install \
        bcmath \
#        bz2 \
#        calendar \
        dba \
#        exif \
        gd \
#        gettext \
        intl \
#        ldap \
        mysqli \
        opcache \
        pdo_mysql \
#        soap \
#        xmlrpc \
#        xsl \
        zip \
    ; 

# Install and configure xdebug +apcu
RUN pecl install xdebug-2.5.5 \
 && docker-php-ext-enable xdebug \
 && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini \
 && pecl install apcu-4.0.11 \
 && docker-php-ext-enable apcu

# Delete source & builds deps so it does not hang around in layers taking up space
RUN pecl clear-cache \
 && rm -rf /tmp/pear /usr/share/doc /usr/share/man /var/lib/apt/lists/* \
 && docker-php-source delete \
 && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $buildDeps

# Set timezone Europe/Paris
RUN rm /etc/localtime \
 && ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime \
 && "date"
ENV TZ=Europe/Paris

# Composer
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer

RUN chown -R www-data:www-data /var/www/

RUN mkdir -p $COMPOSER_HOME/cache

USER www-data

EXPOSE 9000
