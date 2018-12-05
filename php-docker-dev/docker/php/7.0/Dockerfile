FROM daocloud.io/library/php:7.0.13-fpm


RUN sed -i "s/archive.ubuntu./mirrors.aliyun./g" /etc/apt/sources.list 
RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list 

RUN apt-get clean -y
RUN apt update
RUN apt upgrade -y
RUN apt-get install -y \
    libpng-dev \
    libpq-dev \
    g++ \
    libicu-dev \
    libxml2-dev \
    htop \
    git \
    vim \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    zlib1g-dev \
    librabbitmq-dev
    
RUN apt install -y curl
RUN apt install -y libcurl3
RUN apt install -y libcurl3-dev

RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install opcache \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install posix \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install sysvmsg 

    
RUN curl -fsSL 'http://192.168.9.24:807/docker_files/amqp-1.9.3.tgz' -o amqp.tar.gz \
    && mkdir -p amqp \
    && tar -xf amqp.tar.gz -C amqp --strip-components=1 \
    && rm amqp.tar.gz \
    && ( \
    cd amqp \
    && phpize \
    && ./configure --enable-amqp \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r amqp \
    && docker-php-ext-enable amqp 

RUN curl -fsSL 'http://192.168.9.24:807/docker_files/swoole-1.10.0.tgz' -o swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
    cd swoole \
    && phpize \
    && ./configure --enable-swoole \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole 

RUN curl -fsSL 'http://192.168.9.24:807/docker_files/redis-3.1.6.tgz' -o redis.tar.gz \
    && mkdir -p redis \
    && tar -xf redis.tar.gz -C redis --strip-components=1 \
    && rm redis.tar.gz \
    && ( \
    cd redis \
    && phpize \
    && ./configure --enable-redis \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r redis \
    && docker-php-ext-enable redis 

RUN curl -fsSL 'http://192.168.9.24:807/docker_files/ImageMagick-7.0.7-26.tar.gz' -o ImageMagick.tar.gz \
    && mkdir -p ImageMagick \
    && tar -xf ImageMagick.tar.gz -C ImageMagick --strip-components=1 \
    && rm ImageMagick.tar.gz \
    && ( \
    cd ImageMagick \
    && ./configure \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r ImageMagick
    
RUN curl -fsSL 'http://192.168.9.24:807/docker_files/imagick-3.4.3.tgz' -o imagick.tar.gz \
    && mkdir -p imagick \
    && tar -xf imagick.tar.gz -C imagick --strip-components=1 \
    && rm imagick.tar.gz \
    && ( \
    cd imagick \
    && phpize \
    && ./configure --enable-imagick \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r imagick \
    && docker-php-ext-enable imagick 

# RUN curl -fsSL 'http://192.168.9.24:807/docker_files/molten-v0.1.1.tar.gz' -o molten.tar.gz \
#     && mkdir -p molten \
#     && tar -xf molten.tar.gz -C molten --strip-components=1 \
#     && rm molten.tar.gz \
#     && ( \
#     cd molten \
#     && phpize \
#     && ./configure \
#     && make \
#     && make install \
#     ) \
#     && rm -r molten \
#     && docker-php-ext-enable molten  

ENV PHALCON_VERSION=3.2.4
RUN curl -sSL "http://192.168.9.24:807/docker_files/phalcon-v3.2.4.tar.gz" -o phalcon.tar.gz \
    && mkdir -p phalcon \
    && tar -xf phalcon.tar.gz -C phalcon --strip-components=1 \
    && rm phalcon.tar.gz \
    && cd phalcon/build \
    && ./install \
    && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    && cd ../../ \
    && rm -r phalcon 
    

#xss安全提醒插件
# RUN curl -sSL "http://192.168.9.24:807/docker_files/taint-2.0.4.tar.gz"  -o taint.tar.gz \
#     && mkdir -p taint \
#     && tar -xf taint.tar.gz -C taint --strip-components=1 \
#     && rm taint.tar.gz \
#     && ( \
#     cd taint \
#     && phpize \
#     && ./configure \
#     && make \
#     && make install \
#     ) \
#     && rm -r taint \
#     && docker-php-ext-enable taint  

#rpc yar
# RUN curl -fsSL "http://192.168.9.24:807/docker_files/yar-2.0.4.tar.gz"   -o yar.tar.gz \
#     && mkdir -p yar \
#     && tar -xf yar.tar.gz -C yar --strip-components=1 \
#     && rm yar.tar.gz \
#     && ( \
#     cd yar \
#     && phpize \
#     && ./configure \
#     && make \
#     && make install \
#     ) \
#     && rm -r yar \
#     && docker-php-ext-enable yar  




#composer安装
RUN echo "memory_limit=-1" > "$PHP_INI_DIR/conf.d/memory-limit.ini" \
    && echo "date.timezone=${PHP_TIMEZONE:-UTC}" > "$PHP_INI_DIR/conf.d/date_timezone.ini"

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp
ENV COMPOSER_VERSION 1.6.5

RUN curl -sSL "http://192.168.9.24:807/docker_files/composer.phar" -o composer.phar \
    && mv composer.phar /usr/bin/composer \
    && chmod u+x /usr/bin/composer


RUN curl -fsSL 'http://192.168.9.24:807/docker_files/mongodb-1.3.4.tgz' -o mongodb.tar.gz \
    && mkdir -p mongodb \
    && tar -xf mongodb.tar.gz -C mongodb --strip-components=1 \
    && rm mongodb.tar.gz \
    && ( \
    cd mongodb \
    && phpize \
    && ./configure --enable-mongodb \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r mongodb \
    && docker-php-ext-enable mongodb 





ADD gitconfig /etc/
ADD php.ini /usr/local/etc/php/