ARG TAG_SUFFIX
FROM sindriainc/php:zts-${TAG_SUFFIX}

ARG TAG_SUFFIX
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

#USER root

# Install pthreads
RUN cd /tmp && \
    git clone https://github.com/krakjoe/pthreads.git && \
    cd pthreads && \
    phpize && \
    ./configure --with-php-config=/usr/local/bin/php-config && \
    make && \
    cp modules/pthreads.so /usr/lib64/php-zts/modules/ && \
    echo "extension=pthreads.so" >> /etc/php-zts.d/pthreads.ini

#USER ${SINDRIA_USER}
