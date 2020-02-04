FROM php:7.3-fpm

RUN apt-get update && \
  apt-get install -y git \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev && \
  docker-php-ext-install gd pdo_mysql mysqli mbstring bcmath

ENV LD_LIBRARY_PATH /usr/local/instantclient/
ENV TNS_ADMIN /usr/local/instantclient/

COPY instantclient-basic-linux.x64-12.2.0.1.0.zip /tmp
COPY instantclient-sdk-linux.x64-12.2.0.1.0.zip /tmp

RUN apt-get update && \
    apt-get install -y zip unzip libaio1 wget && \
    unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/ && \
    unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/ && \
    ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
    ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so && \
    export LD_LIBRARY_PATH=/usr/local/instantclient && \
    docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient && \
	docker-php-ext-install oci8 && \ 
	docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/usr/local/instantclient,12.2 && \
	docker-php-ext-install pdo_oci && \
    rm -rf /usr/local/*.zip

RUN apt-get install -y \
    curl \
    gnupg
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y nodejs
RUN npm install npm@latest -g

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
