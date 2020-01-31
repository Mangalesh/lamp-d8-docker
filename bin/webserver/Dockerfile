FROM php:7.1.20-apache

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

# Install useful tools
RUN apt-get -y install apt-utils vim wget

# Install important libraries
RUN apt-get -y install --fix-missing apt-utils build-essential git curl libcurl3 libcurl3-dev zip libxml2-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev

# Other PHP7 Extensions
RUN apt-get -y install libmcrypt-dev
RUN docker-php-ext-install mcrypt

RUN apt-get -y install mysql-client
RUN docker-php-ext-install pdo_mysql 
RUN docker-php-ext-install mysqli

RUN docker-php-ext-install curl
RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json
RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

RUN docker-php-ext-install -j$(nproc) bcmath
RUN docker-php-ext-install -j$(nproc) session
RUN docker-php-ext-install -j$(nproc) opcache

RUN docker-php-ext-install -j$(nproc) mbstring && docker-php-ext-enable mbstring

RUN docker-php-ext-install -j$(nproc) xml
RUN docker-php-ext-install -j$(nproc) simplexml
RUN docker-php-ext-install -j$(nproc) mcrypt
RUN docker-php-ext-install -j$(nproc) dom
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 
RUN docker-php-ext-install -j$(nproc) gd

ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf

RUN a2enmod rewrite

# Install Composer
# RUN curl -sS https://getcomposer.org/installer | php && \
#    mv composer.phar /usr/local/bin/composer && \
#    ln -s /root/.composer/vendor/bin/drush /usr/local/bin/drush


# Install Drush 8.
# RUN composer global require drush/drush
# ENV PATH="~/.composer/vendor/bin:${PATH}"
# RUN drush --version
# RUN composer global update


# Unfortunately, adding the composer vendor dir to the PATH doesn't seem to work. So:
# RUN ln -s /root/.composer/vendor/bin/drush /usr/local/bin/drush

# Install Drupal Console. There are no stable releases yet, so set the minimum 
# stability to dev.
# RUN curl https://drupalconsole.com/installer -L -o drupal.phar && \
#	mv drupal.phar /usr/local/bin/drupal && \
#	chmod +x /usr/local/bin/drupal
# RUN drupal init


# Clean repository
RUN apt-get clean

# Enable apache modules
RUN a2enmod headers

# Drupal installation ::

# WORKDIR /var/www/html
# RUN curl http://drupalconsole.com/installer -L -o drupal.phar
# RUN mv drupal.phar /usr/local/bin/drupal && chmod +x /usr/local/bin/drupal
# RUN drupal init

RUN wget https://ftp.drupal.org/files/projects/drupal-8.8.1.zip
RUN mv /var/lib/docker/tmp/docker-builder467111395/drupal-8.8.1.zip /home/ubuntu/d8-docker-git/www/web/
RUN unzip drupal-8.8.1.zip
RUN mv /var/lib/docker/tmp/docker-builder467111395/drupal-8.8.1 /home/ubuntu/d8-docker-git/www/web/drupal8
RUN chown -R www-data:www-data /var/www/html
RUN chmod +x /var/www/html

EXPOSE 80
EXPOSE 3306
