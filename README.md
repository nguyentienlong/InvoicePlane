app url 'http://dev.invoiceplane/'

copy ~/invoiceplane/application/config/database_sample.php and rename to database.php

in ~/invoiceplane/application/config/database.php
```
$db['default']['hostname'] = '127.0.0.1';
$db['default']['username'] = 'root';
$db['default']['password'] = '';
$db['default']['database'] = 'invoiceplane';
$db['default']['dbdriver'] = 'mysqli';
```
nginx configuration

```
server {
    listen   80;
    server_name    dev.invoiceplane;
    #access_log    /var/log/nginx/dev.invoiceplance.access.log;
    error_log    /var/log/nginx/dev.invoiceplane.error.log;
    root    /path/to/invoiceplane;
    index index.html index.php index.htm;

    # set expiration of assets to MAX for caching
    location ~* \.(ico|css|js|gif|jpe?g|png)(\?[0-9]+)?$ {
        expires max;
        log_not_found off;
    }


    location / {
        index       index.php;
        try_files   $uri $uri/ /index.php;    
    }

    location ~* \.php(/|$) {
	fastcgi_pass php-fpm;
        #fastcgi_split_path_info ^(.+\.php)(/.+)$;
	#fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;    
     }

     # deny access to .htaccess files
    location ~ /\.ht {
        deny        all;
    }

}
```
add `dev.invoiceplan 127.0.0.1` into /etc/hosts
run http://dev.invoiceplane/setup
Initial commit for the customized CRM of Duy VO
