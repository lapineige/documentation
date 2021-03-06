# Server requirements

FreshRSS is a web application. This means you'll need a web server to run it. FreshRSS requirements are really low, so it could run on most shared host servers.

You need to verify that your server can run FreshRSS before installing it. If your server has the proper requirements and FreshRSS does not work, please contact us to find a solution.

| Software    | Recommended      | Works also with               |
| ----------- | ---------------- | ----------------------------- |
| Web server  | **Apache 2**     | Nginx                         |
| PHP         | **PHP 5.3.7+**   | PHP 5.2+                      |
| PHP modules | Required: libxml, cURL, PDO_MySQL, PCRE and ctype. \\ Required (32-bit only): GMP \\Recommanded: JSON, Zlib, mbstring, iconv, ZipArchive | |
| Database    | **MySQL 5.0.3+** | SQLite 3.7.4+                 |
| Browser     | **Firefox**      | Chrome, Opera, Safari or IE9+ |

### Important notice

FreshRSS **CAN** work with PHP 5.3.3. To do so, we are using specific functions available in the ''password_compat'' library for the form authentication. This library is compatible with PHP >= 5.3.7 but some older version include the patch.
It all depends on the distribution:

* CentOS and RHEL 6.5 are supported.
* On the other hand, **Debian with PHP 5.3.3 is not**! ([For more information](https://github.com/ircmaxell/password_compat#requirements))

# Getting the appropriate version of FreshRSS

FreshRSS has three different releases or branches. Each branch has its own release frequency. So it is better if you spend some time to understand the purpose of each release.

## Stable release

[Download](https://github.com/FreshRSS/FreshRSS/archive/master.zip)

This release is done when we consider that our goal concerning the new features and the stability is reached. It could happen that we make two releases in a really short time if we have a really good coding pace. In reality, we are all working on our spare time, so we release every few months. But this version is really stable, tested thoroughly and you should not face any major bugs.

## Development release

[Download](https://github.com/FreshRSS/FreshRSS/archive/dev.zip)

As its name suggests, it is the working release for developers. **This release is unstable!** If you want to keep track of enhancements on a daily basis, you can use it. But keep in mind that you need to follow the branch activity on Github (via [the branch RSS feed](https://github.com/FreshRSS/FreshRSS/commits/dev.atom) for instance). Some say that the main developers use it on a daily basis without problem. They may know what they are doing…

# Apache installation

**TODO**

# Nginx installation

This is an example nginx configuration file. It covers http, https and php-fpm configuration.

_You can find simpler config file but they may be incompatible with FreshRSS API._

```
server {
  listen 80; # http on port 80
  listen 443 ssl; # https on port 443

  # https configuration 
  ssl on;
  ssl_certificate      /etc/nginx/server.crt;
  ssl_certificate_key  /etc/nginx/server.key;

  # your server's url(s)
  server_name example.com rss.example.com;

  # the folder p of your FreshRSS installation
  root /srv/FreshRSS/p/;

  index index.php index.html index.htm;

  # nginx log files
  access_log /var/log/nginx/rss.access.log;
  error_log /var/log/nginx/rss.error.log;

  # php files handling
  # this regex is mandatory because of the API
  location ~ ^.+?\.php(/.*)?$ {
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_split_path_info ^(.+\.php)(/.*)$;
    # By default, the variable PATH_INFO is not set under PHP-FPM
    # But FreshRSS API greader.php need it. If you have a "Bad Request" error, double check this var !
    fastcgi_param PATH_INFO $fastcgi_path_info;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  }
  
  location / {
    try_files $uri $uri/ index.php;
  }
}
```



# Security

**TODO**
