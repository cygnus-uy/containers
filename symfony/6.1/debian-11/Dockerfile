FROM php:8.1-apache-bullseye

RUN apt-get update -y \
	&& apt-get install -y software-properties-common apt-transport-https curl wget gpg lsb-release ca-certificates \ 
	# && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/sury-php.list \
	# && wget -qO - https://packages.sury.org/php/apt.gpg | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/debian-php-8.gpg --import \
	# && chmod 644 /etc/apt/trusted.gpg.d/debian-php-8.gpg \
	&& apt-get update -y \
	&& apt-get install -y build-essential git zip
# && apt-get install -y php-mbstring php-zip php-gd php-json php-curl php-xml php-intl php-xdebug

RUN git config --global user.email "samuel@cygnus.com.uy" \ 
	&& git config --global user.name "Samuel"

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt-get install -y symfony-cli

RUN symfony new /var/www/html --version="6.1.*" --webapp
RUN	yes | composer require symfony/apache-pack
# RUN mkdir /var/www/html/public

COPY ./apache2/dir.conf /etc/apache2/mods-enabled/dir.conf 
COPY ./apache2/vhost-prod-symfony.conf /etc/apache2/sites-available/vhost-prod-symfony.conf
COPY ./apache2/ports.conf /etc/apache2/ports.conf
RUN a2dissite 000-default \ 
	&& a2ensite vhost-prod-symfony \
	&& a2enmod rewrite \
	&& service apache2 restart

WORKDIR /var/www/html