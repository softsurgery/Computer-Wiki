his always works for me:

```php
ini_set('display_errors', '1');
ini_set('display_startup_errors', '1');
error_reporting(E_ALL);
```

However, this doesn't make PHP to show parse errors occurred in the same file. Also, these settings can be overridden by PHP. In these cases the only way to show those errors is to modify your php.ini (or php-fpm.conf) with this line:

```php
display_errors = on
```

(if you don't have access to `php.ini`, then putting this line in `.htaccess` might work too):

```php
php_flag display_errors 1
```

### PROD environment

Note that above recommendation is only suitable for the DEV environment. On a live site it must be

```php
display_errors = off
log_errors = on
```

And then you'll be able to see all errors in the error log. See [Where to find PHP error log](https://stackoverflow.com/q/5127838/285587)

### AJAX calls

In case of AJAX call, on a DEV server, open DevTools (F12), then Network tab. Then initiate the request which result you want to see, and it will appear in the Network tab. Click on it and then the Response tab. There you will see the exact output.

While on a live server just check the error log all the same.