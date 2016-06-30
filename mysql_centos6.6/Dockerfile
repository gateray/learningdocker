#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing MySQL5.6.29: Dockerfile for building MySQL5.6.29
#
FROM gateray/centos:6.6
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

#install mysql
COPY mysql56-5.6.29-1.el6.x86_64.rpm /tmp/
RUN  yum -y localinstall /tmp/mysql56-5.6.29-1.el6.x86_64.rpm
EXPOSE 3306
VOLUME /usr/local/mysql/data

#add to supervisor
RUN chkconfig mysqld off && \
    service mysqld stop && \
    chkconfig --del mysqld
COPY supervisor_mysql.conf /etc/supervisor.conf.d/mysql.conf

#clean source code dir
RUN rm -f /tmp/mysql56-5.6.29-1.el6.x86_64.rpm && \
    yum clean all
