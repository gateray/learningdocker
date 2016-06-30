#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing CentOS6.6: Dockerfile for building building nginx1.8 
#
FROM gateray/centos:6.6
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

ENV PREFIX /usr/local
ENV SRC_DIR /usr/local/src
ENV NGINX_USER www


#install nginx requirement pkg: jemalloc
RUN yum -y install jemalloc jemalloc-devel

#install nginx requirement pkg: zlib
ADD zlib-1.2.8.tar.gz ${SRC_DIR}/
RUN cd ${SRC_DIR}/zlib-1.2.8 && (make clean ||/bin/true) && \
    ./configure --prefix=${PREFIX}/zlib && make -j && make install

#install nginx requirement pkg: openssl
ADD openssl-1.0.2g.tar.gz ${SRC_DIR}/
RUN cd ${SRC_DIR}/openssl-1.0.2g && (make clean ||/bin/true) && \
    ./config --openssldir=${PREFIX}/openssl && make -j && make install

#install nginx requirement pkg: pcre
ADD pcre-8.38.tar.gz ${SRC_DIR}/
RUN cd ${SRC_DIR}/pcre-8.38 && (make clean ||/bin/true) && \
    ./configure --prefix=${PREFIX}/pcre && make -j && make install    
    
#install nginx
RUN useradd -r -s /sbin/nologin -M ${NGINX_USER}
ADD nginx-1.8.1.tar.gz ${SRC_DIR}/
RUN cd ${SRC_DIR}/nginx-1.8.1 && (make clean ||/bin/true) && \
    ./configure --prefix=${PREFIX}/nginx \
    --user=${NGINX_USER} --group=${NGINX_USER} \
    --with-ld-opt='-ljemalloc' \
    --with-http_ssl_module \
    --with-openssl=${SRC_DIR}/openssl-1.0.2g \
    --with-pcre=${SRC_DIR}/pcre-8.38 \
    --with-zlib=${SRC_DIR}/zlib-1.2.8 \
    --with-http_stub_status_module && make -j `grep 'processor' /proc/cpuinfo | wc -l` && make install

#config nginx
RUN mkdir -p ${PREFIX}/nginx/conf/vhosts
COPY nginx_nginx.conf ${PREFIX}/nginx/conf/nginx.conf
COPY nginx_proxy.conf ${PREFIX}/nginx/conf/proxy.conf
COPY nginx_upstream.conf ${PREFIX}/nginx/conf/upstream.conf
COPY nginx_vhosts ${PREFIX}/nginx/conf/vhosts/
EXPOSE 80 443

#add to supervisor
COPY supervisor_nginx.conf /etc/supervisor.conf.d/nginx.conf

#clean source code dir
RUN rm -rf ${SRC_DIR}/*
