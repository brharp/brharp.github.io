---
title: Drupal Subdir
layout: post
---

## The Problem

How to run Drupal at a subpath of your domain?

## The Solution

```
<VirtualHost *:80>
  ServerName www.example.com
  Alias /path /srv/www/htdocs/www.example.com.path/docroot
  <Location /path>
    php_admin_value open_basedir "/srv/www/htdocs/www.example.com.path"
    <IfModule mod_rewrite.c>
      RewriteEngine on
      RewriteCond %{REQUEST_FILENAME} !-f
      RewriteCond %{REQUEST_FILENAME} !-d
      RewriteCond %{REQUEST_URI} !=/favicon.ico
      RewriteRule ^/srv/www/htdocs/www.example.com.path/docroot/(.*)$ /path/index.php?q=$1 [L,QSA]
    <IfModule>
  <Location>
<VirtualHost>
```