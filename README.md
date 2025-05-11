# Frontend App with Apache on Linux

# Update the System
sudo apt update && sudo apt upgrade -y

# Apache Web Server:
sudo apt install apache2 -y

# Node.js and npm 
sudo apt install nodejs npm -y

# unzip (to extract zipped builds)
sudo apt install unzip -y

# Move and Unzip the Build Folder
sudo mv ~/build.zip /var/www/html/
cd /var/www/html/
sudo unzip build.zip
sudo rm build.zip

# Set Permissions
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/

# Configure Apache Virtual Host
sudo nano /etc/apache2/sites-available/example.com.conf

<VirtualHost *:80>
    ServerName example.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/client_error.log
    CustomLog ${APACHE_LOG_DIR}/client_access.log combined
</VirtualHost>

# Enable Virtual Host & Rewrite Module
sudo a2ensite example.com.conf
sudo a2enmod rewrite
sudo systemctl reload apache2

# Create .htaccess File for Client-Side Routing (React/Vue/etc.)

sudo nano /var/www/html/.htaccess

<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>

# Restart Apache

sudo systemctl restart apache2




