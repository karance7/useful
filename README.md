## Create Multiple Virtual Hosts (Websites)

***Install Apache in Server***
```bash
sudo apt-get update
sudo apt-get install apache2
```
***Create Folder in Server***
```bash
sudo mkdir -p /var/www/example.com/public_html
sudo mkdir -p /var/www/test.com/public_html
```
***Set Files Permissions***
```bash
sudo chown -R $USER:$USER /var/www/example.com/public_html
sudo chown -R $USER:$USER /var/www/test.com/public_html
```
***Modify WWW folder Permissions*** 
```bash
sudo chmod -R 755 /var/www
```
***Create Demo Page***
```bash
sudo nano /var/www/example.com/public_html/index.html
```
```html
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success!  The example.com virtual host is working!</h1>
  </body>
</html>
```

*Repeat for Another Domain*

***Create Virtual Host File***
```bash
sudo nano /etc/apache2/sites-available/example.com.conf
```
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

> Replace example.com with your domain.

***Enable Website***
```bash
sudo a2ensite example.com.conf
```
***Restart Server***
```bash
sudo systemctl restart apache2
```
```bash
sudo service apache2 restart
```

## Setup SSL

***Copy Cert and Key to Server***

```bash
sudo mkdir -p /etc/cloudflare/
```
```bash
sudo nano /etc/cloudflare/example.com.pem
```
```bash
sudo nano /etc/cloudflare/example.com.key
```

***Configure Apache***

```bash
sudo a2enmod ssl
```

```bash
sudo nano /etc/apache2/sites-available/example.com.conf
```
```
<VirtualHost *:80>

    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    RewriteEngine on
    RewriteCond %{SERVER_NAME} =example.com [OR]
    RewriteCond %{SERVER_NAME} =www.example.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]

</VirtualHost>

<VirtualHost *:443>

    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    SSLEngine on
    SSLCertificateFile /etc/cloudflare/example.com.pem
    SSLCertificateKeyFile /etc/cloudflare/example.com.key

</VirtualHost>
```

> *Check for Error*

```bash
apachectl configtest
```
```bash
sudo systemctl restart apache2
```
