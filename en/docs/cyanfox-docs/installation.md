---
title: CyanFox-Docs Installation
permalink: /docs/cyanfox-docs/installation
next: /docs/cyanfox-docs/usage
next_title: Usage
---

# Installation

- <a href="/docs/cyanfox-docs/" wire:navigate>Introduction</a>
- <a href="/docs/cyanfox-docs/installation" wire:navigate>Installation</a>
- <a href="/docs/cyanfox-docs/usage" wire:navigate>Usage</a>

To install the CyanFox-Docs,
you first need to install the dependencies with Composer and NPM.


```bash
composer install && npm install
```

After that, you need to copy the `.env.example` file to `.env` and generate a new key.<br>
**Note:** You can change all the settings like urls, etc. in the `.env` file.

```bash
cp .env.example .env
php artisan key:generate
```

## Development Environment
Now you can start the development server with the following command.

```bash
php artisan serve
```
and in another terminal

```bash
npm run dev
```

or with Laravel Sail

```bash
./vendor/bin/sail up -d && ./vendor/bin/sail npm run dev
```

Now you can visit the Laravel-Template in your browser at [http://localhost:8000](http://localhost:8000).
Or with Laravel Sail at [http://localhost](http://localhost).

## Production
To build the assets for production, you can run the following command.

```bash
npm run build
```

Migrate the database with the following command.

```bash
php artisan migrate
# or with Laravel Sail
./vendor/bin/sail artisan migrate
```

For Webserver-Configuration, you can use the following example for Nginx or Apache.

**Note:** We recommend to add a SSL-Certificate to your Webserver-Configuration.

```apache
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/CyanFox-Poll/public

    <Directory /var/www/CyanFox-Poll/public>
        AllowOverride All
        Require all granted
    </Directory>

</VirtualHost>
```

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /var/www/CyanFox-Poll/public;

    add_header X-Frame-Options "SAMEORIGIN";
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
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }

}
```
