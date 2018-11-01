FROM php:7.2.11-fpm

# updates
RUN apt-get update && apt-get install -y \
    git \
    libxml2-dev \
    libssl-dev \
    libpq-dev \
    libbz2-dev \
    libpng-dev

# extensions
RUN docker-php-ext-install opcache && \
    docker-php-ext-install zip && \
    docker-php-ext-install bz2 && \
    docker-php-ext-install iconv && \
    docker-php-ext-install calendar && \
    docker-php-ext-install dom && \
    docker-php-ext-install gd && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install pdo pdo_mysql

# xdebug
ARG XDEBUG_ON=false
ARG XDEBUG_REMOTE_HOST=127.0.0.1
RUN if [ ${XDEBUG_ON} = true ]; then \
    pecl install xdebug && \
    docker-php-ext-enable xdebug && \
    echo "xdebug.default_enable = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_autostart = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_connect_back = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_enable = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_host = ${XDEBUG_REMOTE_HOST}" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_port = 9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
;fi

# Install Tideways
RUN if [ ${XHPROF} = true ]; then \
    git clone https://github.com/tideways/php-profiler-extension.git && \
    cd php-profiler-extension && \
    phpize7.2 && \
    ./configure && \
    make && \
    make install && \
    echo "extension=tideways.so" >> /usr/local/etc/php/php.ini && \
    echo "tideways.auto_prepend_library=0" >> /usr/local/etc/php/php.ini \
;fi

# mark volume/s as externally mounted
VOLUME ["/var/www"]

# starting point
WORKDIR /var/www

# make web server connect to this port
EXPOSE 9000

CMD ["php-fpm"]
