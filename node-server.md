
### Install Node.js

Update your server's packages and install  **curl** with the following commands:

```mixed
sudo apt-get update
sudo apt-get install curl
```

Download the Node.js PPA:

```mixed
curl -sL https://deb.nodesource.com/setup_6.x -o nodesource_setup.sh
```

Run the  **nodesource_setup.sh**  command to add the PPA to your server's package cache:

```mixed
sudo bash nodesource_setup.sh
```

Note: This script will update the server automatically. There is no need to run  `apt-get update`  a second time.

Install Node.js:

```mixed
sudo apt-get install nodejs
```

This will automatically install  **npm** as well.

Finally, install the  **build-essential** package for npm:

```mixed
sudo apt-get install build-essential
```

### Create a Sample Node.js Application

For this example we will begin by creating a separate directory in your website's document root for housing Node.js applications:

```mixed
sudo mkdir /var/www/html/nodejs
```

Create the file  **hello.js**  in this directory:

```mixed
sudo nano /var/www/html/nodejs/hello.js
```

Put the following content into the file:

```mixed
#!/usr/bin/env nodejs
var http = require('http');
http.createServer(function (request, response) {
   response.writeHead(200, {'Content-Type': 'text/plain'});
   response.end('Hello World! Node.js is working correctly.\n');
}).listen(8080);
console.log('Server running at http://127.0.0.1:8080/');
```

Save and exit the file.

Make the file executable:

```mixed
sudo chmod 755 hello.js
```

### Install PM2

Use  **npm** to install PM2 with the command:

```mixed
sudo npm install -g pm2
```

Start the  **hello.js** example script with the command:

```mixed
sudo pm2 start hello.js
```

**As root**  add PM2 to the startup scripts, so that it will automatically restart if the server is rebooted:

```mixed
sudo pm2 startup systemd
```

### Configure Apache

To access the Node.js script from the web, install the Apache modules  **proxy** and  **proxy_http** with the commands:

```mixed
sudo a2enmod proxy
sudo a2enmod proxy_http
```

Once the installation is complete, restart Apache for the changes to take effect:

```mixed
sudo service apache2 restart
```

Next, you will need to add the Apache proxy configurations. These directives need to be inserted into the  **VirtualHost** command block in the site's main Apache configuration file.

By common convention, this Apache configuration file is usually **/etc/apache2/sites-available/example.com.conf**  on Ubuntu.

Note: The location and filename of a site's Apache configuration file can vary.

Edit this file with your editor of choice, for example with the command:

```mixed
sudo nano /etc/apache2/sites-available/example.com.conf
```

Scroll through the file until you find the  **VirtualHost** command block, which will look like:

```mixed
<VirtualHost *:80>
ServerName example.com
    <Directory "/var/www/example.com/html">
    AllowOverride All
    </Directory>
</VirtualHost>
```

Add the following to  **VirtualHost** command block:

```mixed
ProxyRequests Off
   ProxyPreserveHost On
   ProxyVia Full
   <Proxy *>
      Require all granted
   </Proxy>

   <Location /nodejs>
      ProxyPass http://127.0.0.1:8080
      ProxyPassReverse http://127.0.0.1:8080
   </Location>
```

Be sure to put these lines outside any  **Directory** command blocks. For example:

```mixed
<VirtualHost *:80>
ServerName example.com

   ProxyRequests Off
   ProxyPreserveHost On
   ProxyVia Full
   <Proxy *>
      Require all granted
   </Proxy>

   <Location /nodejs>
      ProxyPass http://127.0.0.1:8080
      ProxyPassReverse http://127.0.0.1:8080
   </Location>

    <Directory "/var/www/example.com/html">
    AllowOverride All
    </Directory>
</VirtualHost>
```

Save and exit the file, then restart Apache for the changes to take effect:

```mixed
sudo services apache2 restart`
```

After Apache restarts, you can test the application by viewing it in a browser. You should see the message, "Hello World! Node.js is working correctly."
