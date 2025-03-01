# Pterodactyl Panel සඳහා Docker Image එක
FROM ubuntu:20.04

# Update & Install Dependencies
RUN apt update && apt upgrade -y && \
    apt install -y curl sudo zip unzip tar git redis-server mariadb-server \
                   php php-cli php-mbstring php-xml php-bcmath php-curl php-zip \
                   php-tokenizer php-json php-common php-gd php-pear php-dev \
                   php-mysql nginx composer

# Dockerized Environment Setup
WORKDIR /var/www/pterodactyl

# Pterodactyl Panel Download කරමු
RUN curl -Lo panel.tar.gz https://github.com/pterodactyl/panel/releases/latest/download/panel.tar.gz && \
    tar -xzvf panel.tar.gz && \
    chmod -R 755 storage/* bootstrap/cache/

# PHP Dependencies Install
RUN composer install --no-dev --optimize-autoloader

# Permissions Set
RUN chown -R www-data:www-data /var/www/pterodactyl

# Expose Panel Port
EXPOSE 80

# Start Services
CMD service nginx start && php artisan p:environment:setup && php artisan migrate --seed && php artisan queue:restart
