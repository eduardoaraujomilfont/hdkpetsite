FROM php:7.4-apache

RUN docker-php-ext-install mysqli pdo pdo_mysql

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs

RUN npm install --global gulp-cli

RUN apt-get install -y git

RUN apt-get install -y zip

#!/bin/sh

# add wget
RUN apt-get update -yqq && apt-get -f install -yyq wget

# download helper script
RUN wget -q -O /usr/local/bin/install-php-extensions https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions \
    || (echo "Failed while downloading php extension installer!"; exit 1)

# install all required extensions
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && install-php-extensions \
    gd \
;

RUN docker-php-ext-install exif
RUN docker-php-ext-configure exif \
            --enable-exif

# Install Composer
RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
&& curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
&& php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
&& php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
&& rm -f /tmp/composer-setup.*

# Set user and group
ARG user=hdkadmin
ARG group=hdkadmin
ARG uid=1000
ARG gid=1000
RUN groupadd -g ${gid} ${group}
RUN useradd -u ${uid} -g ${group} -s /bin/sh -m ${user}
RUN usermod -aG sudo hdkadmin
# WORKDIR /var/www/html
# COPY package.json /var/www/html
# RUN npm install
USER ${uid}:${gid}

# WORKDIR /var/www/html