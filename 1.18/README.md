# Nginx Docker Container Image

- [Docker images](#docker-images)
- [Environment variables](#environment-variables)
- [Default behaviour](#default-behavior)    
- [Customization](#customization)      
- [Virtual hosts presets](#virtual-hosts-presets)
    - [HTML](#html)
    - [HTTP proxy (application server)](#http-proxy-application-server)
    - [Django](#django)
    - [PHP-based (FastCGI)](#PHP-based-fastcgi)
        - [PHP](#php)
        - [WordPress](#wordpress)
        - [Drupal](#drupal)
    - [Custom preset](#custom-preset)
    - [No preset](#no-preset)
- [Orchestration actions](#orchestration-actions)

## Environment Variables

| Variable                                             | Default Value               | Description                         |
| -----------------------------------------            | --------------------------- | -----------                         |
| `NGINX_ALLOW_ACCESS_HIDDEN_FILES`                    |                             |                                     |
| `NGINX_BACKEND_FAIL_TIMEOUT`                         | `0`                         |                                     |
| `NGINX_BACKEND_HOST`                                 | Varies with a preset        |                                     |
| `NGINX_BACKEND_PORT`                                 | Varies with a preset        |                                     |
| `NGINX_BROTLI`                                       | `on`                        |                                     |
| `NGINX_BROTLI_STATIC`                                | `on`                        |                                     |
| `NGINX_BROTLI_COMP_LEVEL`                            | `1`                         |                                     |
| `NGINX_CLIENT_BODY_BUFFER_SIZE`                      | `16k`                       |                                     |
| `NGINX_CLIENT_BODY_TIMEOUT`                          | `60s`                       |                                     |
| `NGINX_CLIENT_HEADER_BUFFER_SIZE`                    | `4k`                        |                                     |
| `NGINX_CLIENT_HEADER_TIMEOUT`                        | `60s`                       |                                     |
| `NGINX_CLIENT_MAX_BODY_SIZE`                         | `32m`                       |                                     |
| `NGINX_CONF_INCLUDE`                                 | `conf.d/*.conf`             |                                     |
| `NGINX_DISABLE_CACHING`                              |                             |                                     |
| `NGINX_DJANGO_MEDIA_ROOT`                            | `/var/www/html/media/`      |                                     |
| `NGINX_DJANGO_MEDIA_URL`                             | `/media/`                   |                                     |
| `NGINX_DJANGO_STATIC_ROOT`                           | `/var/www/html/static/`     |                                     |
| `NGINX_DJANGO_STATIC_URL`                            | `/static/`                  |                                     |
| `NGINX_DRUPAL_ALLOW_XML_ENDPOINTS`                   |                             |                                     |
| `NGINX_DRUPAL_FILE_PROXY_URL`                        |                             | e.g. `http://dev.example.com`       |
| `NGINX_DRUPAL_HIDE_HEADERS`                          |                             |                                     |
| `NGINX_DRUPAL_XMLRPC_SERVER_NAME`                    |                             | Drupal 6/7 only                     |
| `NGINX_ERROR_403_URI`                                |                             |                                     |
| `NGINX_ERROR_404_URI`                                |                             |                                     |
| `NGINX_ERROR_LOG_LEVEL`                              | `error`                     |                                     |
| `NGINX_ERROR_MESSAGE_50x`                            |                             |                                     |
| `NGINX_FASTCGI_BUFFER_SIZE`                          | `32k`                       | For PHP-based presets only          |
| `NGINX_FASTCGI_BUFFERS`                              | `16 32k`                    | For PHP-based presets only          |
| `NGINX_FASTCGI_INDEX`                                | `index.php`                 | For PHP-based presets only          |
| `NGINX_FASTCGI_INTERCEPT_ERRORS`                     | `on`                        | For PHP-based presets only          |
| `NGINX_FASTCGI_READ_TIMEOUT`                         | `900`                       | For PHP-based presets only          |
| `NGINX_GZIP_BUFFERS`                                 | `16 8k`                     |                                     |
| `NGINX_GZIP_COMP_LEVEL`                              | `1`                         |                                     |
| `NGINX_GZIP_DISABLE`                                 | `msie6`                     |                                     |
| `NGINX_GZIP_HTTP_VERSION`                            | `1.1`                       |                                     |
| `NGINX_GZIP_MIN_LENGTH`                              | `20`                        |                                     |
| `NGINX_GZIP_PROXIED`                                 | `any`                       |                                     |
| `NGINX_GZIP_VARY`                                    | `on`                        |                                     |
| `NGINX_GZIP`                                         | `on`                        |                                     |
| `NGINX_HIDE_50x_ERRORS`                              |                             |                                     |
| `NGINX_HTTP2`                                        |                             |                                     |
| `NGINX_INDEX_FILE`                                   | Varies with a preset        | Hard-coded for Drupal and WP        |
| `NGINX_KEEPALIVE_REQUESTS`                           | `100`                       |                                     |
| `NGINX_KEEPALIVE_TIMEOUT`                            | `75s`                       |                                     |
| `NGINX_LARGE_CLIENT_HEADER_BUFFERS`                  | `8 16k`                     |                                     |
| `NGINX_LOG_FORMAT_OVERRIDE`                          |                             |                                     |
| `NGINX_MODSECURITY_ENABLED`                          |                             | See [ModSecurity]                   |
| `NGINX_MODSECURITY_INBOUND_ANOMALY_SCORE_THRESHOLD`  | `7`                         |                                     |
| `NGINX_MODSECURITY_OUTBOUND_ANOMALY_SCORE_THRESHOLD` | `7`                         |                                     |
| `NGINX_MODSECURITY_POST_CORE_RULES`                  |                             | Location to rules loaded after CRS  |
| `NGINX_MODSECURITY_PRE_CORE_RULES`                   |                             | Location to rules loaded before CRS |
| `NGINX_MODSECURITY_USE_OWASP_CRS`                    |                             | See [ModSecurity]                   |
| `NGINX_MULTI_ACCEPT`                                 | `on`                        |                                     |
| `NGINX_NO_DEFAULT_HEADERS`                           |                             |                                     |
| `NGINX_PAGESPEED_ENABLE_FILTERS`                     |                             |                                     |
| `NGINX_PAGESPEED_ENABLED`                            |                             | See [PageSpeed]                     |
| `NGINX_PAGESPEED_FILE_CACHE_PATH`                    | `/var/cache/ngx_pagespeed/` |                                     |
| `NGINX_PAGESPEED_PRESERVE_URL_RELATIVITY`            | `on`                        |                                     |
| `NGINX_PAGESPEED_RESPECT_X_FORWARDED_PROTO`          | `on`                        |                                     |
| `NGINX_PAGESPEED_REWRITE_LEVEL`                      | `CoreFilters`               |                                     |
| `NGINX_PAGESPEED_STATIC_ASSET_PREFIX`                | `/pagespeed_static`         |                                     |
| `NGINX_PAGESPEED`                                    | `on`                        | See [PageSpeed]                     |
| `NGINX_REAL_IP_HEADER`                               | `X-Real-IP`                 |                                     |
| `NGINX_REAL_IP_RECURSIVE`                            | `off`                       |                                     |
| `NGINX_RESET_TIMEDOUT_CONNECTION`                    | `off`                       |                                     |
| `NGINX_SEND_TIMEOUT`                                 | `60s`                       |                                     |
| `NGINX_SENDFILE`                                     | `on`                        |                                     |
| `NGINX_SERVER_EXTRA_CONF_FILEPATH`                   |                             |                                     |
| `NGINX_SERVER_NAME`                                  | `default`                   |                                     |
| `NGINX_SERVER_ROOT`                                  | `/var/www/html`             |                                     |
| `NGINX_SERVER_TOKENS`                                | `off`                       |                                     |
| `NGINX_SET_REAL_IP_FROM`                             |                             |                                     |
| `NGINX_STATIC_404_TRY_INDEX`                         |                             |                                     |
| `NGINX_STATIC_ACCESS_LOG`                            | `off`                       |                                     |
| `NGINX_STATIC_EXPIRES`                               | `1y`                        |                                     |
| `NGINX_STATIC_MP4_BUFFER_SIZE`                       | `1M`                        |                                     |
| `NGINX_STATIC_MP4_MAX_BUFFER_SIZE`                   | `5M`                        |                                     |
| `NGINX_STATIC_OPEN_FILE_CACHE_ERRORS`                | `on`                        |                                     |
| `NGINX_STATIC_OPEN_FILE_CACHE_MIN_USES`              | `2`                         |                                     |
| `NGINX_STATIC_OPEN_FILE_CACHE_VALID`                 | `30s`                       |                                     |
| `NGINX_STATIC_OPEN_FILE_CACHE`                       | `max=1000 inactive=30s`     |                                     |
| `NGINX_TCP_NODELAY`                                  | `on`                        |                                     |
| `NGINX_TCP_NOPUSH`                                   | `on`                        |                                     |
| `NGINX_TRACK_UPLOADS`                                | `uploads 60s`               |                                     |
| `NGINX_UNDERSCORES_IN_HEADERS`                       | `off`                       |                                     |
| `NGINX_UPLOAD_PROGRESS`                              | `uploads 1m`                |                                     |
| `NGINX_USER`                                         | `nginx`                     |                                     |
| `NGINX_VHOST_NO_DEFAULTS`                            |                             |                                     |
| `NGINX_VHOST_PRESET`                                 | `html`                      |                                     |
| `NGINX_WORKER_CONNECTIONS`                           | `1024`                      |                                     |
| `NGINX_WORKER_PROCESSES`                             | `auto`                      |                                     |
| `NGINX_WP_FILE_PROXY_URL`                            |                             | e.g. `http://dev.example.com`       |
| `NGINX_WP_GOOGLE_XML_SITEMAP`                        |                             | See [WordPress]                     |
| `NGINX_WP_YOAST_XML_SITEMAP`                         |                             | See [WordPress]                     |

Static files extension defined via the regex and can be overridden via the env var `NGINX_STATIC_EXT_REGEX`, default:
```
css|cur|js|jpe?g|gif|htc|ico|png|xml|otf|ttf|eot|woff|woff2|svg|mp4|svgz|ogg|ogv|pdf|pptx?|zip|tgz|gz|rar|bz2|doc|xls|exe|tar|mid|midi|wav|bmp|rtf|txt|map
```

## Default behavior

Applied to all presets by default, can be disabled via `$NGINX_VHOST_NO_DEFAULTS`:

- `/.well-known/` location supported
- `/ads.txt` allowed
- `/robots.txt` allowed
- `/humans.txt` allowed
- `/favicon.ico` allowed
- `.flv`, `.m4a`, `.mp4`, `.mov` locations supported and handled with appropriate modules
-  `/.healthz` location supported, requests not shown in access log

## Customization

- Pass real IP from a reverse proxy via `$NGINX_SET_REAL_IP_FROM`, e.g. `172.17.0.0/16` for docker network 
- Customize the header which value will be used to replace the client address via `$NGINX_REAL_IP_HEADER`
- Default recommended headers can be disabled via `$NGINX_NO_DEFAULT_HEADERS` (defined in `nginx.conf`)
- Error page file can be customized for HTTP errors `403` (`$NGINX_ERROR_403_URI`) and `404` (`$NGINX_ERROR_404_URI`)
- Default error page for HTTP errors `500`, `502`, `503`, `504` can be disabled via `$NGINX_HIDE_50x_ERRORS`
- Access to hidden files (starting with `.`) can be allowed via `$NGINX_ALLOW_ACCESS_HIDDEN_FILES`
- Caching can be disabled via `$NGINX_DISABLE_CACHING`
- Add extra locations via `$NGINX_SERVER_EXTRA_CONF_FILEPATH=/filepath/to/nginx-locations.conf`, the file will be included at the end of default rules (`server` context)
- Completely override include of the virtual host config by overriding `NGINX_CONF_INCLUDE`, it will be included in `nginx.conf`
- Define [custom preset](#custom-preset)

## Virtual hosts presets

By default will be used `html` virtual host preset, you can change it via env var `$NGINX_VHOST_PRESET`. The list of available presets:

### HTML

- [Preset template](https://github.com/wodby/nginx/blob/master/templates/presets/html.conf.tmpl)
- Usage: this preset selected by default

Overridden default values:

| Variable             | Default Value |
| -------------------- | ------------- |
| `NGINX_INDEX_FILE`   | `index.html`  |

### HTTP proxy (application server)

- [Preset template](https://github.com/wodby/nginx/blob/master/templates/presets/http-proxy.conf.tmpl)
- Usage: add `NGINX_VHOST_PRESET=http-proxy` and `NGINX_BACKEND_HOST=[HOST]` 

Overridden default values:

| Variable             | Default Value |
| -------------------- | ------------- |
| `NGINX_BACKEND_HOST` |               |
| `NGINX_BACKEND_PORT` | `8080`        |

### Django

Same as HTTP proxy but with additional media/static locations for Django.

- [Preset template](https://github.com/wodby/nginx/blob/master/templates/presets/django.conf.tmpl)
- Usage: add `NGINX_VHOST_PRESET=django` 

Overridden default values:

| Variable             | Default Value |
| -------------------- | ------------- |
| `NGINX_BACKEND_HOST` | `python`      |
| `NGINX_BACKEND_PORT` | `8080`        |

### PHP-based (FastCGI)

Overridden default values:

| Variable             | Default Value |
| -------------------- | ------------- |
| `NGINX_BACKEND_HOST` | `php`         |
| `NGINX_BACKEND_PORT` | `9000`        |

#### PHP

* [Preset template](https://github.com/wodby/nginx/blob/master/templates/presets/php.conf.tmpl)
* Usage: add `NGINX_VHOST_PRESET=php`, optionally modify `NGINX_BACKEND_HOST`

Overridden default values:

| Variable           | Default Value          |
| ------------------ | -------------          |
| `NGINX_INDEX_FILE` | `index.php index.html` |

#### WordPress

- [Preset template](https://github.com/wodby/nginx/blob/master/templates/presets/wordpress.conf.tmpl)
- Usage: add `NGINX_VHOST_PRESET=wordpress`, optionally modify `NGINX_BACKEND_HOST`  
- Access to `*.txt` files allowed only if they are located in uploads directory
- Access to `/wp-content/uploads/woocommerce_uploads` disallowed
- There are 2 ways to add sitemap.xml locations:
    - For plugin [Google XML Sitemap](https://wordpress.org/plugins/google-sitemap-generator/) add `$NGINX_WP_GOOGLE_XML_SITEMAP=1`
    - For plugin [Yoast SEO](https://kb.yoast.com/kb/xml-sitemaps-nginx/) add `$NGINX_WP_YOAST_XML_SITEMAP=1`

#### Drupal

- Preset templates: [Drupal 8], [Drupal 7], [Drupal 6]
- Usage: add `NGINX_VHOST_PRESET=` with the value of `drupal8`, `drupal7` or `drupal6`. Optionally modify `NGINX_BACKEND_HOST`
- If you want to use [stage_file_proxy](https://www.drupal.org/project/stage_file_proxy) module, set `$NGINX_STATIC_404_TRY_INDEX=1` to redirect 404 static files requests to Drupal
- Access to `*.txt` files allowed only if they are located in files directory 

[Drupal 8]: https://github.com/wodby/nginx/blob/master/templates/presets/drupal8.conf.tmpl
[Drupal 7]: https://github.com/wodby/nginx/blob/master/templates/presets/drupal7.conf.tmpl
[Drupal 6]: https://github.com/wodby/nginx/blob/master/templates/presets/drupal6.conf.tmpl

#### Custom preset

You can use a custom by preset by mounting your preset to `/etc/gotpl/presets/[my-preset-name].conf.tmpl` and setting `$NGINX_VHOST_PRESET=[my-preset-name]`.

#### No preset

To disable presets set `$NGINX_VHOST_PRESET=""`