#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing CentOS14.04: Dockerfile for building Ubuntu images
#
FROM ubuntu
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

ENV TERM xterm

RUN  sed -i 's@http://archive.ubuntu.com/ubuntu/@http://mirrors.aliyun.com/ubuntu/@' /etc/apt/sources.list && apt-get update

RUN  apt-get -y install vim build-essential zlib1g-dev libyaml-dev libssl-dev libgdbm-dev libreadline-dev libncurses5-dev libffi-dev curl openssh-server checkinstall libxml2-dev libxslt-dev libcurl4-openssl-dev libicu-dev logrotate python-docutils pkg-config cmake nodejs git git-core python-mysqldb python-pip python-dev

RUN  update-alternatives --set editor /usr/bin/vim.basic

RUN  pip install supervisor
COPY supervisor.conf /etc/supervisor.conf
RUN  mkdir -p /etc/supervisor.conf.d /var/log/supervisor

RUN  cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

RUN  apt-get clean && \
     apt-get autoclean && \ 
     apt-get autoremove
ENTRYPOINT ["/usr/local/bin/supervisord","-n","-c","/etc/supervisor.conf"]
