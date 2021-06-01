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
    tzdata && \
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
RUN apk add \
    php${PHP_VERSION} \
    php${PHP_VERSION}-calendar \ 
    php${PHP_VERSION}-ctype \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-dom \
    php${PHP_VERSION}-exif \
    php${PHP_VERSION}-fileinfo \
    php${PHP_VERSION}-ftp \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-gettext \ 
    php${PHP_VERSION}-iconv \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-json \
    php${PHP_VERSION}-mbstring \ 
    php${PHP_VERSION}-mcrypt \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-mysqli \
    php${PHP_VERSION}-mysqlnd \
    php${PHP_VERSION}-PDO \
    php${PHP_VERSION}-Phar \
    php${PHP_VERSION}-posix \
    php${PHP_VERSION}-readline \ 
    php${PHP_VERSION}-shmop \
    php${PHP_VERSION}-SimpleXML \ 
    php${PHP_VERSION}-sockets \ 
    php${PHP_VERSION}-sysvmsg \ 
    php${PHP_VERSION}-sysvsem \ 
    php${PHP_VERSION}-sysvshm \
    php${PHP_VERSION}-tokenizer \ 
    php${PHP_VERSION}-wddx \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-xmlreader \
    php${PHP_VERSION}-xmlwriter \
    php${PHP_VERSION}-xsl \
    php${PHP_VERSION}-opcache \ 
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-imap \
    php${PHP_VERSION}-soap && \
    rm -rf /var/cache/apk/*

SHELL ["/bin/bash", "-c"]