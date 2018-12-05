FROM daocloud.io/library/php:7.1-fpm-alpine


ENV TIMEZONE Asia/Shanghai

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories && \
  apk update && \
  apk add --no-cache tzdata curl && \
  cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
  echo "${TIMEZONE}" > /etc/timezone && \
  apk --update --repository=http://dl-4.alpinelinux.org/alpine/edge/main add --no-cache \
  #--virtual .php-deps $PHPIZE_DEPS \
  #libmcrypt-dev \
  pcre-dev \
  hiredis-dev \
  libpng-dev \
  freetype-dev \
  libjpeg-turbo-dev \
  rabbitmq-c-dev \
  tzdata


ENV SWOOLE_VERSION 4.0.4
ENV REDIS_VERSION 3.1.6
ENV MONGODB_VERSION 1.4.2
ENV PHALCON_VERSION 3.2.1

 RUN curl -fsSL http://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz -o swoole.tar.gz \
    && ( \
    mkdir /tmp/swoole \
    && tar -xf swoole.tar.gz -C /tmp/swoole --strip-components=1 \
    && rm -f swoole.tar.gz \
    && docker-php-ext-configure /tmp/swoole --enable-async-redis \
    ) \
    && curl -fsSL http://pecl.php.net/get/redis-${REDIS_VERSION}.tgz -o redis.tar.gz \
    && ( \ 
    mkdir /tmp/redis \
    && tar -xf redis.tar.gz -C /tmp/redis --strip-components=1 \
    && rm -f redis.tar.gz \
    ) \
    && curl -fsSL http://pecl.php.net/get/amqp-1.9.3.tgz -o amqp.tar.gz \
    && ( \ 
    mkdir /tmp/amqp \
    && tar -xf amqp.tar.gz -C /tmp/amqp --strip-components=1 \
    && rm -f amqp.tar.gz \
    ) \
    && curl -fsSL http://pecl.php.net/get/mongodb-${MONGODB_VERSION}.tgz -o mongodb.tar.gz \
    && ( \
    mkdir /tmp/mongodb \
    && tar -xf mongodb.tar.gz -C /tmp/mongodb --strip-components=1 \
    && rm -f mongodb.tar.gz \
    && docker-php-ext-configure /tmp/mongodb --with-mongodb-ssl=libressl \
    ) \
    && docker-php-ext-configure gd --with-png-dir --with-freetype-dir --with-jpeg-dir \
    && curl -fsSL https://codeload.github.com/phalcon/cphalcon/tar.gz/v$PHALCON_VERSION -o phalcon.tar.gz \
    && ( \ 
    mkdir /tmp/phalcon \
    && tar -xf phalcon.tar.gz -C /tmp/phalcon --strip-components=1 \
    && rm -f phalcon.tar.gz \
    && cd /tmp/phalcon/build \
    && sh install \
    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/50-phalcon.ini \
    ) \
    && docker-php-ext-install mysqli pdo pdo_mysql bcmath pcntl mbstring gd /tmp/redis /tmp/swoole /tmp/amqp /tmp/mongodb

ENV COMPOSER_VERSION 1.6.5
RUN curl -s -f -L -o /tmp/installer.php https://raw.githubusercontent.com/composer/getcomposer.org/b107d959a5924af895807021fcef4ffec5a76aa9/web/installer \
    && php -r " \ 
    \$signature = '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061'; \
    \$hash = hash('SHA384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
    unlink('/tmp/installer.php'); \
    echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
    exit(1); \
    }" \
    && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} \
    && composer --ansi --version --no-interaction 


 RUN rm -rf /var/cache/apk/*  \
    && rm -rf /tmp/*

ADD php.ini /usr/local/etc/php/
