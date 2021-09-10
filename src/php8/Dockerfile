FROM php:8-alpine

ARG TAG_SUFFIX
ARG TAG_VERSION
ARG HOST_USER_UID
ARG TIMEZONE

LABEL \
	name="php" \
	image="sindriainc/php" \
	tag="${TAG_VERSION}-${TAG_SUFFIX}" \
	vendor="sindria"

ENV PATH "/root/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/sindria/.local/bin"
ENV TZ=${TIMEZONE}
ENV SINDRIA_USER="sindria"
ENV SINDRIA_USER_HOME="/home/sindria"
ENV PHP_VERSION=${TAG_SUFFIX}

# Update and install packages
RUN apk update && \
    apk add \
    shadow \
    bash \
    curl \
    wget \
    git \
    rsync \
    openssl \
    ca-certificates \
    tzdata && \
    rm -rf /var/cache/apk/*

## Install dependencies
RUN apk add --no-cache --virtual build-dependencies gcc g++ make autoconf libffi-dev libc-dev libressl-dev curl-dev bzip2-dev libzip-dev libsodium libsodium-dev postgresql-dev libjpeg-turbo freetype freetype-dev libpng libpng-dev icu icu-dev oniguruma-dev gettext-dev libxml2-dev libedit-dev libxslt-dev imap-dev krb5-dev tidyhtml-dev libmemcached-libs imagemagick imagemagick-libs imagemagick-dev libjpeg-turbo-dev && \
    rm -rf /var/cache/apk/*

# Setting app user, timezone and permissions
RUN addgroup -S ${SINDRIA_USER} -g ${HOST_USER_UID} && adduser -s /bin/bash -S ${SINDRIA_USER} -u ${HOST_USER_UID} -G ${SINDRIA_USER} -h ${SINDRIA_USER_HOME} && \
    mkdir -p /var/www && \
    mkdir -p /var/www/app && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    chmod -R 770 /var/www/app && \
    chown -R ${SINDRIA_USER}:${SINDRIA_USER} /var/www/app
COPY resources/.bashrc ${SINDRIA_USER_HOME}

WORKDIR /var/www/app

# Install php extensions
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ || docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install calendar ctype curl exif fileinfo ftp gd gettext iconv intl json mbstring mysqli pdo phar posix readline shmop sockets sysvmsg sysvshm xml zip xsl bcmath imap soap pgsql pdo_pgsql pdo_mysql pcntl tidy sodium && \
    docker-php-ext-enable imagick redis opcache imap zip && \
    apk add \
    php${PHP_VERSION}-calendar \
    php${PHP_VERSION}-ctype \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-exif \
    php${PHP_VERSION}-fileinfo \
    php${PHP_VERSION}-ftp \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-gettext \
    php${PHP_VERSION}-iconv \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-json \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-mysqli \
    php${PHP_VERSION}-pdo \
    php${PHP_VERSION}-phar \
    php${PHP_VERSION}-posix \
    #php${PHP_VERSION}-readline \
    php${PHP_VERSION}-shmop \
    php${PHP_VERSION}-sockets \
    php${PHP_VERSION}-sysvmsg \
    php${PHP_VERSION}-tokenizer \
    php${PHP_VERSION}-session \
    php${PHP_VERSION}-openssl \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-xsl \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-imap \
    php${PHP_VERSION}-pgsql \
    php${PHP_VERSION}-pdo_pgsql \
    php${PHP_VERSION}-pdo_mysql \
    php${PHP_VERSION}-pdo_dblib \
    php${PHP_VERSION}-pcntl \
    php${PHP_VERSION}-tidy && \
    #pecl install pdo_dblib && \
    pecl install imagick redis && \
    pecl install swoole && \
    docker-php-ext-enable swoole && \
    docker-php-ext-enable \
    imagick \
    redis && \
    apk del build-dependencies && \
    rm -rf /var/cache/apk/*

SHELL ["/bin/bash", "-c"]

# Add and setup entrypoint
COPY resources/bin/entrypoint.sh /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/bin/bash", "entrypoint.sh"]

#USER ${SINDRIA_USER}