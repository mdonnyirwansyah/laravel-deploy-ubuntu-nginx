1. **Setup Ubuntu firewall:**
   ```bash
   sudo ufw enable
   ```
   ```bash
   sudo ufw default deny incoming
   ```
   ```bash
   sudo ufw default allow outgoing
   ```
   ```bash
   sudo ufw app list
   ```
   ```bash
   sudo ufw allow OpenSSH
   ```
   ```bash
   sudo ufw allow ssh
   ```
   ```bash
   sudo ufw allow http
   ```
   ```bash
   sudo ufw allow https
   ```
   ```bash
   sudo ufw status
   ```
2. **PHP & Apache2 Config:**
   ```bash
   sudo apt install php8.2 php8.2-gd php8.2-intl php8.2-mbstring php8.2-fpm php8.2-curl php8.2-mysql php8.2-xml php8.2-bcmath php8.2-zip
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
   sudo a2enmod proxy_fcgi setenvif
   ```
   ```bash
   sudo a2disconf php8.1-fpm
   ```
   ```bash
   sudo a2enconf php8.2-fpm
   ```
   ```bash
   sudo a2enmod rewrite
   ```
   ```bash
   sudo a2dissite 000-default.conf
   ```
3. **Setup SSL:**
   ```bash
   openssl genrsa -out hms.key 2048
   ```
   ```bash
   openssl req -new -key hms.key -out hms.csr
   ```
   ```bash
   openssl x509 -req -days 365 -in hms.csr -signkey hms.key -out hms.crt
   ```
4. **Setup Database:**
   ```bash
   sudo mysql
   ```
   ```bash
   CREATE DATABASE simrs_db;
   ```
   ```bash
   CREATE USER 'simrs_admin'@'%' IDENTIFIED WITH mysql_native_password BY '';
   ```
   ```bash
   GRANT ALL ON simrs_db.* TO 'simrs_admin'@'%';
   ```
   ```bash
   exit
   ```
   ```bash
   sed -i 's/DEFINER=[^*]*\*/\*/g' db-dumps/mysql-simrs_db.sql
   ```
   ```bash
   mysql -u simrs_admin -p simrs_db < db-dumps/mysql-simrs_db.sql
   ```
5. **SIMRS Installation:**
   ```bash
   sudo mkdir /var/www/simrs
   ```
   ```bash
   cd /var/www/simrs
   ```
   ```bash
   sudo usermod -a -G www-data $USER
   ```
   ```bash
   sudo chown -R $USER:www-data application/logs
   ```
   ```bash
   sudo chown -R $USER:www-data public
   ```
   ```bash
   sudo chmod -R ug+rw application/logs
   ```
   ```bash
   sudo chmod g+s application/logs
   ```
   ```bash
   composer install --optimize-autoloader --no-dev
   ```
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
   sudo apache2ctl configtest
   ```
   ```bash
   sudo a2ensite simrs.conf
   ```
   ```bash
   sudo systemctl restart apache2
   ```
6. **API-SIMRS Installation:**
   ```bash
   sudo mkdir /var/www/api-simrs
   ```
   ```bash
   cd /var/www/api-simrs
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
   sudo chmod g+s storage/logs
   ```
   ```bash
   wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
   ```
   ```sh
   export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
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
   cp .env.example .env
   ```
   ```bash
   php artisan key:generate
   ```
   ```bash
   php artisan storage:link
   ```
   ```bash
   sudo ufw status
   ```
   ```bash
   sudo ufw allow 81
   ```
   ```bash
   sudo ufw status
   ```
   ```bash
   sudo nano /etc/apache2/ports.conf
   ```
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
   sudo apache2ctl configtest
   ```
   ```bash
   sudo a2ensite api-simrs.conf
   ```
   ```bash
   sudo systemctl restart apache2
   ```
7. **Satset-Client Installation:**
   ```bash
   sudo mkdir /var/www/satset-client
   ```
   ```bash
   cd /var/www/satset-client
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
   sudo chmod -R ug+rwsx storage bootstrap/cache
   ```
   ```bash
   sudo chmod g+s storage/logs
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
   cp .env.example .env
   ```
   ```bash
   php artisan key:generate
   ```
   ```bash
   sudo ufw status
   ```
   ```bash
   sudo ufw allow 8083
   ```
   ```bash
   sudo ufw status
   ```
   ```bash
   sudo nano /etc/apache2/ports.conf
   ```
   ```bash
   sudo nano /etc/apache2/sites-available/satset-client.conf
   ```
   ```bash
   sudo nano /etc/apache2/ports.conf
   ```
   ```bash
   <VirtualHost *:8083>
      ServerName localhost
       DocumentRoot /var/www/satset-client/public

       <Directory /var/www/satset-client/public>
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
   ```bash
   sudo apt-get install supervisor
   ```
   ```bash
   sudo systemctl status supervisor
   ```
   ```bash
   sudo nano /etc/supervisor/conf.d/satset-client-worker.conf
   ```
   ```bash
   [program:satset-client-worker]
   process_name=%(program_name)s_%(process_num)02d
   command=php /var/www/satset-client/artisan queue:work --queue=patients,outpatient,default --timeout=300
   autostart=true
   autorestart=true
   stopasgroup=true
   killasgroup=true
   user=www-data
   numprocs=1
   redirect_stderr=true
   stdout_logfile= /var/www/satset-client/storage/logs/satset-client-worker.log
   stopwaitsecs=3600
   ```
   ```bash
   sudo supervisorctl reread
   ```
   ```bash
   sudo supervisorctl update
   ```
   ```bash
   sudo supervisorctl start satset-client-worker:*
   ```
   ```bash
   sudo nano /etc/supervisor/conf.d/laravel-pulse-worker.conf
   ```
   ```bash
   [program:laravel-pulse-worker]
   process_name=%(program_name)s
   command=php /var/www/satset-client/artisan pulse:check
   autostart=true
   autorestart=true
   user=www-data
   redirect_stderr=true
   stdout_logfile=/var/www/satset-client/storage/logs/laravel-pulse-worker.log
   stopwaitsecs=3600
   ```
   ```bash
   sudo supervisorctl reread
   ```
   ```bash
   sudo supervisorctl update
   ```
   ```bash
   sudo supervisorctl start laravel-pulse-worker:*
   ```
   ```bash
   sudo supervisorctl
   ```
