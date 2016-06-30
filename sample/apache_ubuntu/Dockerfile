FROM ubuntu

#RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
#RUN sed -i 's@http://.*\.com/@http://mirrors.163.com/@' /etc/apt/sources.list
#RUN apt-get update && apt-get install -y --force-yes apache2
#EXPOSE 80 443
#ADD run.sh /
#RUN chmod +x /run.sh
#VOLUME ["/var/www","/var/log/apache2","/etc/apache2"]
#ENTRYPOINT ["/run.sh"]
RUN mkdir -p /app
VOLUME ["/app"]
