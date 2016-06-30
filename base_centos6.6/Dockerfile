#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing CentOS6.6: Dockerfile for building CentOS images
#
FROM centos:6.6
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

ENV TERM xterm

RUN rm -rf /etc/yum.repos.d/*
COPY CentOS-Base.repo epel.repo /etc/yum.repos.d/

RUN yum -y install curl vim-enhanced rsync man tar bzip2 unzip && \
    yum -y install gcc gcc-c++ git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel && \
    yum -y install --enablerepo=epel python-pip pwgen && \
    yum clean all

COPY requirement.txt /tmp/requirement.txt
RUN pip install -r /tmp/requirement.txt
COPY supervisor.conf /etc/supervisor.conf
RUN mkdir -p /etc/supervisor.conf.d /var/log/supervisor

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENTRYPOINT ["/usr/bin/supervisord","-n","-c","/etc/supervisor.conf"]
