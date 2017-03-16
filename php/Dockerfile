# https://github.com/docker-library/wordpress/blob/3a1ca61731e6070764d5b7235e3b6617798b8af8/php7.1/fpm-alpine/Dockerfile
# 7.1.2 is buggy. Mediawiki throws an exception: Could not gather sufficient random data in /var/www/html/includes/NoLocalSettings.php:44
FROM php:7.1.1-fpm-alpine

RUN set -ex; \
	apk add --no-cache --virtual .build-deps \
		libjpeg-turbo-dev \
		libpng-dev \
	; \
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install gd mysqli opcache; \
	runDeps="$( \
		scanelf --needed --nobanner --recursive \
			/usr/local/lib/php/extensions \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)"; \
	apk add --virtual .phpexts-rundeps $runDeps; \
	apk del .build-deps; \
    { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

VOLUME /var/www/html

ENTRYPOINT ["php-fpm"]
