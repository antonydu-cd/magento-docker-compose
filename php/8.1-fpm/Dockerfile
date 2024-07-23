FROM php:8.1-fpm-buster

LABEL Maintainer="antony@ebrook.com.tw" \
      Description="Magento ready image php:8.1-fpm-buster based"


ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y --no-install-recommends \
  apt-utils \
  sendmail-bin \
  sendmail \
  sudo \
  iproute2 \
  libbz2-dev \
  libjpeg62-turbo-dev \
  libpng-dev \
  libfreetype6-dev \
  libgeoip-dev \
  wget \
  libgmp-dev \
  libgpgme11-dev \
  libmagickwand-dev \
  libmagickcore-dev \
  libicu-dev \
  libldap2-dev \
  libpspell-dev \
  libtidy-dev \
  libxslt1-dev \
  libyaml-dev \
  libzip-dev \
  zip \
  webp \
  && rm -rf /var/lib/apt/lists/*

RUN apt-get update -y && apt-get upgrade -y && apt-get autoremove -y

# Configure the gd library
RUN docker-php-ext-configure \
  gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-configure \
  ldap --with-libdir=lib/x86_64-linux-gnu
RUN docker-php-ext-configure \
  opcache --enable-opcache

# Install extensions
# Install required PHP extensions
RUN docker-php-ext-install -j$(nproc) \
  bcmath \
  bz2 \
  calendar \
  exif \
  gd \
  gettext \
  gmp \
  intl \
  ldap \
  mysqli \
  opcache \
  pdo_mysql \
  pspell \
  shmop \
  soap \
  sockets \
  sysvmsg \
  sysvsem \
  sysvshm \
  tidy \
  xsl \
  zip \
  pcntl

# Install phpredis extension
RUN pecl install -o -f redis && docker-php-ext-enable redis
RUN echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for Magento application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www
RUN /etc/init.d/sendmail start

# Copy existing application directory permissions
COPY --chown=www:www . /var/www/html

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]

