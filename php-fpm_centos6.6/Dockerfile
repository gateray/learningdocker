#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing CentOS6.6: Dockerfile for building php-fpm images
#
FROM gateray/centos:6.6
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

ENV APP_DIR /app
ENV SRC_DIR /usr/local/src
ENV PREFIX /usr/local
ENV FPM_LOG_PATH ${PREFIX}/php/var/log
ENV FPM_PID_PATH ${PREFIX}/php/var/run
ENV FPM_CONF_PATH ${PREFIX}/php/etc
ENV PHP_SESS_PATH ${PREFIX}/php/session

#install and config nginx
COPY nginx.repo /etc/yum.repos.d/
RUN yum -y install nginx
COPY nginx_nginx.conf /etc/nginx/nginx.conf
COPY nginx_default.conf /etc/nginx/conf.d/default.conf

#install php and its requirement
ADD php-5.5.33.tar.bz2 ${SRC_DIR}/
RUN yum -y install libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel freetype freetype-devel libmcrypt libmcrypt-devel 
RUN cd ${SRC_DIR}/php-5.5.33 && (make clean ||/bin/true) && \
    ./configure --prefix=/usr/local/php \
                --with-mysql \
                --with-openssl \
                --enable-fpm \
                --enable-sockets \
                --enable-sysvshm \
                --enable-mbstring \
                --with-freetype-dir \
                --with-jpeg-dir \
                --with-png-dir \
                --with-zlib-dir \
                --with-libxml-dir \
                --enable-xml \
                --with-mhash \
                --with-mcrypt \
                --with-config-file-path=/etc \
                --with-config-file-scan-dir=/etc/php.d \
                --with-bz2 \
                --with-curl \
                --enable-opcache \
                --with-mysqli \
                --with-pdo-mysql \
                --enable-mysqlnd \
                --enable-zip \
                --with-gd \
                --enable-bcmath \
                --enable-soap \
                --enable-shmop && make -j `grep 'processor' /proc/cpuinfo | wc -l` && make install
RUN mkdir -p /etc/php.d && cp ${SRC_DIR}/php-5.5.33/php.ini-production /etc/php.ini.default && mkdir -p ${PHP_SESS_PATH} && mkdir -p ${APP_DIR}
COPY php-fpm.conf ${FPM_CONF_PATH}/ 
COPY php.ini /etc/php.ini

EXPOSE	80 443

COPY supervisor_nginx.conf /etc/supervisor.conf.d/nginx.conf
COPY supervisor_php-fpm.conf /etc/supervisor.conf.d/php-fpm.conf

RUN rm -rf ${SRC_DIR}/* &&  yum clean all

ONBUILD COPY . ${APP_DIR}/
ONBUILD RUN chown -R nginx:nginx ${APP_DIR}
