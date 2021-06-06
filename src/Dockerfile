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
    ca-certificates \
    tzdata && \
    apk add --no-cache --virtual build-dependencies \
    gcc \
    g++ \
    make \
    autoconf \
    libffi-dev \
    libc-dev \
    libressl-dev \
    curl-dev \
    bzip2-dev \
    libzip-dev \
    libsodium \
    libsodium-dev \
    postgresql-dev \
    libjpeg-turbo \
    freetype \
    freetype-dev \
    libpng \
    libpng-dev \
    icu \
    icu-dev \
    oniguruma-dev \
    gettext-dev \
    libxml2-dev \
    libedit-dev \
    libxslt-dev \
    imap-dev \
    krb5-dev \
    tidyhtml-dev \
    libmemcached-libs \
    imagemagick \
    imagemagick-libs \
    imagemagick-dev \
    libjpeg-turbo-dev && \
    rm -rf /var/cache/apk/* && \
    ln -s /usr/include/tidybuffio.h /usr/include/buffio.h

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
    #docker-php-ext-configure pdo_dblib --with-libdir=/lib/x86_64-linux-gnu && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    pecl install imagick redis && \
    docker-php-ext-install \
        calendar \
        ctype \
        curl \
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
        zip \
        xsl \
        bcmath \
        imap \
        soap \
        pgsql \
        pdo_pgsql \
        pdo_mysql \
        #pdo_dblib \
        pcntl \
        tidy && \
        docker-php-ext-enable \
        imagick \
        redis \
        #memcached \
        opcache \
        imap \
        zip && \
        pecl install pdo_dblib || echo "Unsupported php version for pdo_dblib" && \
        pecl install swoole || echo "Swoole not supported" && \
        docker-php-ext-enable swoole || echo "Swoole missing" && \
        #apk del build-dependencies && \
        rm -rf /var/cache/apk/*

SHELL ["/bin/bash", "-c"]

# Add and setup entrypoint
COPY resources/bin/entrypoint.sh /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/bin/bash", "entrypoint.sh"]

#USER ${SINDRIA_USER}