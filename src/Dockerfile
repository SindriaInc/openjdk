ARG TAG_SUFFIX
FROM openjdk:${TAG_SUFFIX}-alpine

ARG TAG_VERSION
ARG HOST_USER_UID
ARG TIMEZONE

LABEL \
	name="openjdk" \
	image="sindriainc/openjdk" \
	tag="${TAG_VERSION}-${TAG_SUFFIX}" \
	vendor="sindria"

ENV TZ=${TIMEZONE}
ENV SINDRIA_USER="sindria"
ENV SINDRIA_USER_HOME="/home/sindria"

RUN apk update

# Install packages and setting
RUN apk add \
    bash \
    curl \
    mingw-w64-binutils \
    screen \
    vim \
    wget \
    git \
    rsync \
    ca-certificates \
    tzdata && \
    rm -rf /var/cache/apk/*

# Install Maven
RUN apk add maven && \ 
    rm -rf /var/cache/apk/*

# Add custom maven settings
COPY resources/maven/settings.xml /usr/share/java/maven-3/conf

# Setting app user, timezone and permissions
RUN addgroup -S ${SINDRIA_USER} -g ${HOST_USER_UID} && adduser -s /bin/bash -S ${SINDRIA_USER} -u ${HOST_USER_UID} -G ${SINDRIA_USER} -h ${SINDRIA_USER_HOME} && \
    mkdir -p /var/www && \
    mkdir -p /var/www/app && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    chmod -R 770 /var/www/app && \
    chown -R ${SINDRIA_USER}:${SINDRIA_USER} /var/www/app

WORKDIR /var/www/app

SHELL ["/bin/bash", "-c"]
