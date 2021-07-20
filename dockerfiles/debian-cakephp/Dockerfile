# build arguments
ARG COMPOSER_VERSION="latest"



#===========================================================
# PHP Composer stage
#===========================================================
FROM composer:${COMPOSER_VERSION} AS my-composer



#===========================================================
# main stage
#===========================================================
FROM debian:buster-slim

SHELL [ "/bin/bash", "-l", "-c" ]

ENV APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_PID_FILE=/var/run/apache2/apache2.pid \
    APACHE_RUN_DIR=/var/run/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_LOG_DIR=/var/log/apache2 \
    LANG=ja_JP.UTF-8

RUN apt update && \
    apt install -y \
        apache2 \
        openssl \
        curl \
        wget \
        git \
        zip \
        unzip \
        vim && \
    a2ensite default-ssl && \
    a2enmod ssl

# prepare installing PHP 7 versions
RUN apt install -y \
        gnupg2 \
        ca-certificates \
        lsb-release \
        apt-transport-https && \
        wget https://packages.sury.org/php/apt.gpg && \
        apt-key add apt.gpg && \
        echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php7.list

# install specified PHP version
ARG PHP7_VERSION=7.4
ENV PHP7_VERSION=${PHP7_VERSION}
RUN apt update && \
    apt install -y \
        php${PHP7_VERSION} \
        php${PHP7_VERSION}-cli \
        php${PHP7_VERSION}-common \
        libapache2-mod-php${PHP7_VERSION} && \
    update-alternatives --set php /usr/bin/php${PHP7_VERSION}

# PHP extensions
RUN apt install -y \
    # *** for Composer ***
        php${PHP7_VERSION}-curl \
        # json            # already installed \
        php${PHP7_VERSION}-zip \
    # *** for CakePHP ***
        php${PHP7_VERSION}-intl \
        php${PHP7_VERSION}-mbstring \
        php${PHP7_VERSION}-xml \
        # openssl         # already installed \
        # pdo             # already installed \
    # *** for DebugKit ***
        php${PHP7_VERSION}-sqlite3 \
    # *** for PHPUnit ***
        # dom             # include in xml \
        # xmlwriter       # include in xml \
    # *** for data access ***
        php${PHP7_VERSION}-mysql \
    #     php${PHP7_VERSION}-pgsql \
     && \
    apt -y clean && \
    rm -rf /var/lib/apt/lists/*

# install PHP Composer
COPY --from=my-composer /usr/bin/composer /usr/bin/composer
ARG COMPOSER_HOME="/.composer"
ENV COMPOSER_HOME=${COMPOSER_HOME} \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PATH="$PATH:${COMPOSER_HOME}/vendor/bin"

EXPOSE 80 443 8765
ENTRYPOINT [ "apachectl", "-D", "FOREGROUND" ]
