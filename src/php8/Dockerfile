FROM alpine:3.13

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

# dependencies required for running "phpize"
# these get automatically installed and removed by "docker-php-ext-*" (unless they're already installed)
ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c

COPY resources/bin/ /usr/local/bin/

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

# Setting app user, timezone and permissions
RUN addgroup -S ${SINDRIA_USER} -g ${HOST_USER_UID} && adduser -s /bin/bash -S ${SINDRIA_USER} -u ${HOST_USER_UID} -G ${SINDRIA_USER} -h ${SINDRIA_USER_HOME} && \
    mkdir -p /var/www && \
    mkdir -p /var/www/app && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    chmod -R 770 /var/www/app && \
    chown -R ${SINDRIA_USER}:${SINDRIA_USER} /var/www/app
COPY resources/.bashrc ${SINDRIA_USER_HOME}

WORKDIR /var/www/app

# Installing PHP with extensions
RUN apk add --no-cache php8 \
    php8-common \
    php8-cli \
    php8-opcache \
    php8-calendar \
    php8-ctype \
    php8-curl \
    php8-exif \
    php8-fileinfo \
    php8-ftp \
    php8-gd \
    php8-gettext \
    php8-iconv \
    php8-intl \
    php8-json \
    php8-mbstring \
    php8-mysqli \
    php8-pdo \
    php8-phar \
    php8-posix \
    php8-dom \
    #php8-readline \
    php8-shmop \
    php8-sockets \
    php8-sysvmsg \
    php8-tokenizer \
    php8-session \
    php8-openssl \
    php8-xml \
    php8-xmlwriter \
    php8-simplexml \
    php8-zip \
    php8-xsl \
    php8-bcmath \
    php8-imap \
    php8-pgsql \
    php8-pdo_pgsql \
    php8-pdo_mysql \
    php8-pdo_sqlite \
    php8-pdo_dblib \
    php8-pecl-redis \
    php8-pecl-imagick \
    php8-sodium \
    #php8-pecl-swoole \
    php8-pcntl \
    php8-tidy && \
    #pecl install pdo_dblib && \
    #pecl install imagick redis && \
    #pecl install swoole && \
    #docker-php-ext-enable swoole && \
    #docker-php-ext-enable \
    #imagick \
    #redis && \
    rm -rf /var/cache/apk/*

SHELL ["/bin/bash", "-c"]

# Add and setup entrypoint
COPY resources/bin/entrypoint.sh /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/bin/bash", "entrypoint.sh"]

#USER ${SINDRIA_USER}