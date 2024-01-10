FROM php:8.1-fpm

ARG DOCKER_GROUP_ID=1000
ARG DOCKER_GROUP_NAME='dockerGroup'

ARG DOCKER_UID=1000
ARG DOCKER_USERNAME='alisalehi'

ARG WORKDIR='/var/www'


# Copy composer.lock and composer.json
COPY composer.lock composer.json $WORKDIR

# Set working directory
WORKDIR $WORKDIR

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libonig-dev \
    libzip-dev \
    libgd-dev
# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
#Mine

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-external-gd
RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# =============== Add user ===============
RUN groupadd --force -g $DOCKER_GROUP_ID $DOCKER_GROUP_NAME
RUN useradd -u $DOCKER_GROUP_ID -ms /bin/bash --no-user-group -g $DOCKER_GROUP_NAME $DOCKER_USERNAME

# Copy existing application directory contents
COPY . $WORKDIR

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]