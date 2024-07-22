1. **PHP Dependencies:**
   ```bash
   sudo apt install openssl php8.2 php8.2-cli php8.2-common php8.2-mysql php8.2-zip php8.2-curl php8.2-gd php8.2-mbstring php8.2-xml php8.2-bcmath php8.2-fpm php8.2-phpdbg php8.2-cgi libphp8.2-embed libapache2-mod-php8.2 php8.2-json php8.2-tokenizer
   ```
2. **SIMRS Config Apache2:**
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
   cd /var/www
   ```
   ```bash
   mkdir simrs
   ```
3. **API-SIMRS Config Apache2:**
   ```bash
   sudo nano /etc/apache2/sites-available/api-simrs.conf
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
   cd /var/www
   ```
   ```bash
   mkdir api-simrs
   ```
