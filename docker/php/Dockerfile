FROM php:8.2-fpm

# Install required system dependencies
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libicu-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libzip-dev \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) pdo_pgsql pgsql bcmath intl gd zip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy existing application files (optional)
COPY . /var/www/html

# Run Composer Install (Ensure dependencies are installed correctly)
# RUN composer install --no-interaction --optimize-autoloader

# Install Node.js and npm
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs

# Run npm install (adjust if you have a specific directory for frontend)
# RUN npm install --legacy-peer-deps

# Clean up unnecessary files
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Start PHP-FPM and NPM together
CMD ["sh", "-c", "php-fpm & npm install & npm run dev"]
