FROM alpine:3.16
LABEL Maintainer="Reishou <reishou90@gmail.com>" \
      Description="Lightweight container with Nginx & PHP-FPM 8 based on Alpine Linux."

# Install packages
RUN apk --no-cache add php81 php81-fpm php81-bcmath php81-ctype php81-json php81-fileinfo \
    php81-mbstring php81-openssl php81-pdo_pgsql php81-curl php81-pdo php81-tokenizer php81-xml \
    php81-opcache nginx curl runit openrc php81-simplexml php81-dom php81-xmlwriter \
    php81-sockets php81-phar git php81-gd php81-iconv php81-xmlreader php81-zip php81-session php81-sodium php81-pdo_sqlite nano

RUN apk add --no-cache -X http://dl-cdn.alpinelinux.org/alpine/edge/testing \
  php81-pecl-grpc

# fix work iconv library with alphine
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ --allow-untrusted gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

RUN cp /usr/bin/php81 /usr/bin/php

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Configure nginx
COPY nginx.conf /etc/nginx/nginx.conf
#COPY sites/*.conf /etc/nginx/http.d/
## Remove default server definition
RUN ls /etc/nginx/http.d
RUN rm /etc/nginx/http.d/default.conf

# Configure PHP-FPM
COPY fpm-pool.conf /etc/php81/php-fpm.d/www.conf
COPY php.ini /etc/php81/conf.d/custom.ini

# Configure runit boot script
COPY boot.sh /sbin/boot.sh

RUN adduser -D -u 1000 -g 1000 -s /bin/sh www && \
    mkdir -p /var/www/html && \
    mkdir -p /var/cache/nginx && \
    chown -R www:www /var/www/html && \
    chown -R www:www /run && \
    chown -R www:www /var/lib/nginx && \
    chown -R www:www /var/log/nginx

COPY nginx.run /etc/service/nginx/run
COPY php.run /etc/service/php/run

RUN chmod +x /etc/service/nginx/run \
    && chmod +x /etc/service/php/run \
    && ls -al /var/www/html/

WORKDIR /var/www/html

# Expose the port nginx is reachable on
EXPOSE 80

# Let boot start nginx & php-fpm
CMD ["sh", "/sbin/boot.sh"]

