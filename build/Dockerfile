FROM php:8.1-fpm-alpine

EXPOSE 9000

RUN apk add --no-cache rsync freetype-dev libpng-dev jpeg-dev unzip sudo bash gettext libzip-dev zip \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd opcache mysqli pdo pdo_mysql \
    && docker-php-ext-install zip \
	&& rm -rf /usr/src/php/

ARG MODX_VERSION=3.0.1
ENV MODX_VERSION ${MODX_VERSION}

# upstream tarballs include ./modx-${MODX_VERSION}/
RUN cd / \
  && echo ${MODX_VERSION} \
  && curl -o modx.zip -SL https://modx.com/download/direct/modx-${MODX_VERSION}-pl.zip \
	&& unzip -q modx.zip -d /usr/src \
	&& rm -rf /modx \
	&& mkdir -p /modx /usr/src/modx \
  && mv /usr/src/modx-${MODX_VERSION}-pl/ /modx/public \
	&& find /modx -name 'ht.access' -exec bash -c 'rm $0' {} \; \
	&& mv /modx/public/core /modx/ \
	&& cp -r /modx/public/setup /usr/src/modx \
  && rm modx.zip \
	&& chown -R www-data:www-data /modx

COPY --chown=www-data files/docker-entrypoint.sh /entrypoint.sh
COPY --chown=www-data:www-data files/docker-entrypoint /docker-entrypoint
COPY files/php-config/ /usr/local/etc/php/conf.d/
COPY --chown=www-data:www-data files/config.core.php /modx/config.core.php.tmpl

RUN chmod +x /entrypoint.sh \
	&& mkdir -p /modx/static/manager /modx/core/config /modx/core/packages /modx/core/components /modx/public/assets

VOLUME /modx/static/manager
VOLUME /modx/core/config
VOLUME /modx/core/packages
VOLUME /modx/core/components
VOLUME /modx/public/assets

WORKDIR /modx/public
ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]
