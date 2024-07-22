1. **PHP:**
   ```bash
   sudo apt install openssl php8.2 php8.2-cli php8.2-common php8.2-mysql php8.2-zip php8.2-curl php8.2-gd php8.2-mbstring php8.2-xml php8.2-bcmath php8.2-fpm php8.2-phpdbg php8.2-cgi libphp8.2-embed libapache2-mod-php8.2 php8.2-json php8.2-tokenizer
   ```
   ```bash
   sudo a2dismod php8.1
   ```
   ```bash
   sudo a2enmod php8.2
   ```
   ```bash
   sudo update-alternatives --config php
   ```
   ```bash
   sudo update-alternatives --set php /usr/bin/php8.2
   ```
   ```bash
   sudo a2disconf php8.1-fpm
   ```
   ```bash
   sudo a2enconf php8.2-fpm
   ```
2. **SIMRS Config:**
   ```bash
   sudo nano /etc/apache2/sites-available/simrs.conf
   ```
   ```bash
   <VirtualHost *:80>
      ServerName localhost
       DocumentRoot /var/www/simrs

       <Directory /var/www/simrs>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```
   ```bash
   sudo mkdir /var/www/simrs
   ```
   ```bash
   cd /var/www/simrs
   ```
   ```bash
   
   ```
3. **API-SIMRS Config:**
   ```bash
   sudo mkdir /var/www/api-simrs
   ```
   ```bash
   cd /var/www/api-simrs
   ```
   ```bash
   sudo usermod -a -G www-data $USER
   ```
   ```bash
   sudo chown -R $USER:www-data storage bootstrap/cache
   ```
   ```bash
   sudo chgrp -R www-data storage bootstrap/cache
   ```
   ```bash
   sudo chmod -R ug+rwx storage bootstrap/cache
   ```
   ```bash
   wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
   ```
   ```bash
   nvm install node
   ```
   ```bash
   npm install
   ```
   ```bash
   npm run build
   ```
   ```bash
   composer install --optimize-autoloader --no-dev
   ```
   ```bash
   cp .example.env .env
   ```
   ```bash
   php artisan key:generate
   ```
   ```bash
   sudo nano /etc/apache2/sites-available/api-simrs.conf
   ```
   ```bash
   sudo nano /etc/apache2/ports.conf
   ```
   ```bash
   <VirtualHost *:81>
      ServerName localhost
       DocumentRoot /var/www/api-simrs/public

       <Directory /var/www/api-simrs/public>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```
   ```bash
   sudo apache2ctl configtest
   ```
   ```bash
   sudo a2ensite satset-client.conf
   ```
   ```bash
   sudo systemctl restart apache2
   ```
   
