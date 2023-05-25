# Install and Create a vue project 
### Step 1: Update System Packages
```
sudo apt update
sudo apt upgrade
sudo apt install build-essential curl
```

### Step 2: Install Node js and NPM
```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install nodejs
sudo npm install npm@latest -g
npm -v
node -v
```
### Step 3: Install Vue.js on Ubuntu 20.04
```
npm install vue@next
sudo npm install -g @vue/cli
```

### Step 4: Create Vue.js Application
```
vue create tuts-project
cd tuts-project
npm run serve
```

# NGINX Configuration
   First you need to build your apps which will create a folder name dist
```
# goto you project folder and run this command
npm run build
```

```
sudo apt update
sudo apt install nginx
```
```
sudo nano /etc/nginx/sites-available/example.com
```
```
upstream vue {
     server 127.0.0.1:8069;
}
server {
    listen      80;
    server_name vue.schoolofthought.tech www.vue.schoolofthought.tech;
    charset utf-8;
    root    /home/ubuntu/tuts-project/dist;
    index   index.html;
    proxy_read_timeout 720s;
    proxy_connect_timeout 720s;
    proxy_send_timeout 720s;
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Real-IP $remote_addr;
    #Always serve index.html for any request
    location / {
        root /home/ubuntu/tuts-project/dist;
        index    index.html index.htm;
        include  /etc/nginx/mime.types;
        #try_files $uri /index.html;
        try_files $uri $uri/ /index.html;
    }
    error_log  /var/log/nginx/vue-app-error.log;
    access_log /var/log/nginx/vue-app-access.log;
    gzip_types text/css text/less text/plain text/xml application/xml application/json application/javagzip on;
}


```

