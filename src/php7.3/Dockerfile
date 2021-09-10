FROM php:7.3-alpine

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
RUN apk add --no-cache --virtual build-dependencies \
    gcc \
    g++ \
    make \
    autoconf \
    pkgconf \
    imagemagick \
    imagemagick-libs \
    imagemagick-dev && \
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
RUN apk add \
    php7-calendar \
    php7-ctype \
    php7-curl \
    php7-exif \
    php7-fileinfo \
    php7-ftp \
    php7-gd \
    php7-gettext \
    php7-iconv \
    php7-intl \
    php7-json \
    php7-mbstring \
    php7-mysqli \
    php7-pdo \
    php7-phar \
    php7-posix \
    #php7-readline \
    php7-shmop \
    php7-sockets \
    php7-sysvmsg \
    php7-tokenizer \
    php7-session \
    php7-openssl \
    php7-xml \
    php7-zip \
    php7-xsl \
    php7-bcmath \
    php7-imap \
    php7-pgsql \
    php7-pdo_pgsql \
    php7-pdo_mysql \
    php7-pdo_dblib \
    php7-pcntl \
    php7-tidy && \
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