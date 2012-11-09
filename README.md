codeigniter-cli-bootstrap
=========================

Formerly: **Cron Bootstrapper**

A simple bootstrap file that you can use to directly run your CodeIgniter controllers from the command line and redirect the output to a log file.

Installation
------------

1) Copy the code at the bottom of this code and save it in a file called **cron.php** anywhere on your server (but not in the document root!).

2) Set the CRON_CI_INDEX constant to the full absolute file/path of your CodeIgniter index.php file

3) Make this file directly executable:
```php
chmod a+x cron.php
```

Usage
-----

You can use this file to call any controller function:
```php
./cron.php --run=/controller/method [--show-output] [--log-file=logfile] [--time-limit=N] [--server=http_server_name]
```

Options
-------

`--run=/controller/method`
(Required) The controller and method you want to run.

`--show-output`
(Optional) Display CodeIgniter's output on the console (default: don't display)

`--log-file=logfile`
(Optional) Log the date/time this was run, along with CodeIgniter's output

`--time-limit=N`
(Optional) Stop running after N seconds (default=0, no time limit)

`--server=http_server_name`
(Optional) Set the $_SERVER['SERVER_NAME'] system variable (useful if your application needs to know what the server name is)

Troubleshooting
---------------

**`Invalid interpreter: /usr/bin/php^M` error when running ./cron.php:**

This is caused by not having UNIX-style line breaks, which usually happens by copying files created
on other operating systems. Use this command to convert the line breaks to UNIX format:

```sh
mv cron.php cron.old
tr -d '\15\32' < cron.old > cron.php
rm cron.old
```

**`cron.php` bombs around line 111:**
```php
require(CRON_CI_INDEX);           // Main CI index.php file
```

Check that you have correctly defined the path to your application's main **index.php** defined correctly:

```php
define('CRON_CI_INDEX', '/var/www/vhosts/intranet/index.php');   // Your CodeIgniter main index.php file
```

**No errors or output and the script doesn't seem to run.**

Apparently some authentication libraries, such as Michael Wales' Erkana authentication library,
cause this sort of behaviour (it's possible that any code that depends on sessions could cause this).
Just use some conditional logic testing to see if the CRON constant is defined before auto-loading
any authentication libraries.

