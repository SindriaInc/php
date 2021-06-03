ARG TAG_SUFFIX
FROM php:${TAG_SUFFIX}-alpine

ARG TAG_VERSION
ARG HOST_USER_UID
ARG TIMEZONE

LABEL \
	name="php" \
	image="sindriainc/php" \
	tag="${TAG_VERSION}-${TAG_SUFFIX}" \
	vendor="sindria"

ENV TZ=${TIMEZONE}
ENV SINDRIA_USER="sindria"
ENV SINDRIA_USER_HOME="/home/sindria"
ENV PHP_VERSION=${TAG_SUFFIX}

# Update and install packages
RUN apk update && \
    apk add \
    gcc \
    g++ \
    libffi-dev \
    libc-dev \
    libressl-dev \
    bash \
    curl \
    wget \
    git \
    rsync \
    ca-certificates \
    libzip-dev \
    libsodium \
    libffi-dev \
    postgresql-dev \
    libjpeg-turbo \
    freetype \
    libpng \
    icu \
    tzdata && \
    apk add --no-cache --virtual build-dependencies icu-dev libxml2-dev freetype-dev libpng-dev libjpeg-turbo-dev g++ make autoconf libsodium-dev && \
    rm -rf /var/cache/apk/*

# Setting app user, timezone and permissions
RUN addgroup -S ${SINDRIA_USER} -g ${HOST_USER_UID} && adduser -s /bin/bash -S ${SINDRIA_USER} -u ${HOST_USER_UID} -G ${SINDRIA_USER} -h ${SINDRIA_USER_HOME} && \
    mkdir -p /var/www && \
    mkdir -p /var/www/app && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    chmod -R 770 /var/www/app && \
    chown -R ${SINDRIA_USER}:${SINDRIA_USER} /var/www/app

WORKDIR /var/www/app

# Install php extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install \
        calendar \
        ctype \
        dom \
        exif \
        fileinfo \
        ftp \
        gd \
        gettext \
        iconv \
        intl \
        json \
        mbstring \
        mysqli \
        mysqlnd \
        pdo \
        phar \
        posix \
        readline \
        shmop \
        sockets \
        sysvmsg \
        sysvshm \
        tokenizer \
        xml \
        xmlreader \
        xmlwriter \
        zip \
        xsl \
        bcmath \
        imap \
        soap \
        pgsql \
        pdo_pgsql \
        pdo_mysql \
        pdo_dblib \
        pcntl \
        tidy

SHELL ["/bin/bash", "-c"]