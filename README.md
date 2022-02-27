# Nginx setup in Odoo

## Step 1: Install Nginx
```
sudo apt update
sudo apt upgrade
sudo apt install nginx
```
## Step 2: Remove default Nginx configurations
```
sudo rm /etc/nginx/sites-enabled/default
sudo rm /etc/nginx/sites-available/default
```
## Step 3: Create a new Nginx configuration for Odoo in the sites-available directory.
```
sudo nano /etc/nginx/sites-available/odoo15.conf
```
## Step 4: Copy and paste the following configuration, ensure that you change the server_name directory to match your domain name.
```
upstream odoo15 {
     server 127.0.0.1:8069;
}

server {
     listen 80;
     server_name domainname.com;

     access_log /var/log/nginx/access.log;
     error_log /var/log/nginx/error.log;

     proxy_read_timeout 720s;
     proxy_connect_timeout 720s;
     proxy_send_timeout 720s;
     proxy_set_header X-Forwarded-Host $host;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Real-IP $remote_addr;

     location / {
        proxy_redirect off;
        proxy_pass http://odoo15;
     }

     location ~* /web/static/ {
         proxy_cache_valid 200 90m;
         proxy_buffering on;
         expires 864000;
         proxy_pass http://odoo15;
     }

     gzip_types text/css text/less text/plain text/xml application/xml application/json application/javascript;
     gzip on;
 }
```
Hit Ctrl+X followed by Y and Enter to save the file and exit.

## Step 5: To enable this newly created website configuration, symlink the file that you just created into the sites-enabled directory.

```
sudo ln -s /etc/nginx/sites-available/odoo15.conf /etc/nginx/sites-enabled/odoo15.conf
```
## Step 6: Check your configuration and restart Nginx for the changes to take effect.

```
sudo nginx -t
sudo service nginx restart
```
