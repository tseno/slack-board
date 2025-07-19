FROM php:8.2-fpm-alpine

# Install system dependencies and PHP extensions in one layer
RUN apk add --no-cache \
    # System packages
    curl wget unzip git vim bash shadow sudo nodejs npm \
    # PostgreSQL client
    postgresql-client \
    # PHP extension dependencies
    postgresql-dev libpng-dev libjpeg-turbo-dev freetype-dev \
    libzip-dev icu-dev oniguruma-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
        pdo_pgsql pgsql mbstring bcmath gd zip intl \
    # Clean up build dependencies to reduce image size
    && apk del postgresql-dev libpng-dev libjpeg-turbo-dev freetype-dev \
        libzip-dev icu-dev oniguruma-dev

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Set up non-root user
RUN adduser -D -s /bin/bash vscode && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Switch to vscode user
USER vscode

# Create workspace directory
WORKDIR /workspace