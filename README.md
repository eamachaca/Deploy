# Commands that I find every deploy, every project, etc. Then, I want to save here for search it here.
## Generate differents ssh keys to each git server (4Ex)
`ssh-keygen -t rsa -b 4096 -f ~/.ssh/name`
## Change PHP versions
`sudo update-alternatives --config php`

It change the path of php's command (for example if i want to use php8 for one project modification I use it to change to php8, and if I need change something in php7.4 I can change with this command and later back to php8 (obviusly running newly this command)). It's a good functionally in my ubuntu desktop

sudo apt install -y php7.4-mbstring php7.4-xml php7.4-fpm php7.4-zip php7.4-common php7.4-fpm php7.4-cli php7.4-mysql php7.4-bcmath php7.4-curl unzip curl

sudo apt install -y php7.3-mbstring php7.3-xml php7.3-fpm php7.3-zip php7.3-common php7.3-fpm php7.3-cli php7.3-mysql php7.3-bcmath php7.3-curl unzip curl


## My Configurations to one SomeProject

### MYSQL
```
create database SomeProject;
CREATE USER 'SomeProject'@'localhost' IDENTIFIED WITH mysql_native_password BY 'some_password';
GRANT ALL PRIVILEGES ON hds.* TO 'SomeProject'@'localhost';
```
### NGINX
```
server {
    listen 80;
    server_name SomeProject.bolivia.bo;
    root /var/www/SomeProject/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
```
server {
    listen 80;
    server_name OtherProject.bolivia.bo;
    root /var/www/OtherProject/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
### Folder
```
sudo chmod -R 755 /var/www/SomeProject
sudo chown -R www-data:www-data /var/www/SomeProject
```
