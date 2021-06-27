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

# Install parallel
RUN pecl install parallel && \
    echo "extension=parallel.so" > /usr/local/etc/php/conf.d/parallel.ini

#USER ${SINDRIA_USER}
