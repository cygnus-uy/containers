FROM php:8.1-apache-bullseye

RUN apt-get update -y \
	&& apt-get install -y software-properties-common apt-transport-https curl wget gpg lsb-release ca-certificates \ 
	# && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/sury-php.list \
	# && wget -qO - https://packages.sury.org/php/apt.gpg | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/debian-php-8.gpg --import \
	# && chmod 644 /etc/apt/trusted.gpg.d/debian-php-8.gpg \
	&& apt-get update -y \
	&& apt-get install -y build-essential git zip
	# && apt-get install -y php-mbstring php-zip php-gd php-json php-curl php-xml php-intl php-xdebug

# RUN apt-get update && apt-get install -y \
# 	gnupg \
# 	g++ \
# 	procps \
# 	openssl \
# 	git \
# 	unzip \
# 	zlib1g-dev \
# 	libzip-dev \
# 	libfreetype6-dev \
# 	libpng-dev \
# 	libjpeg-dev \
# 	libicu-dev  \
# 	libonig-dev \
# 	libxslt1-dev \
# 	acl \
# 	&& echo 'alias sf="php bin/console"' >> ~/.bashrc

# RUN docker-php-ext-configure gd --with-jpeg --with-freetype 

# RUN docker-php-ext-install \
#     pdo pdo_mysql zip xsl gd intl opcache exif mbstring

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt-get install -y symfony-cli

COPY ./config-files/dir.conf /etc/apache2/mods-enabled/dir.conf 
COPY ./config-files/vhost-prod-symfony.conf /etc/apache2/sites-available/vhost-prod-symfony.conf
COPY ./config-files/ports.conf /etc/apache2/ports.conf

RUN a2enmod proxy_fcgi setenvif
RUN a2enmod rewrite

RUN git config --global user.email "samuel@cygnus.com.uy" \ 
	&& git config --global user.name "Samuel"

WORKDIR /var/www/html