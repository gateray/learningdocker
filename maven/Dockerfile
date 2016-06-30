#
# MAINTAINER        Gateray.Zhou <gateray1@gmail.com>
# DOCKER-VERSION    1.10.1
#
# Dockerizing Maven3: Dockerfile for building maven images
#
FROM java
MAINTAINER Gateray.Zhou <gateray1@gmail.com>

ENV MAVEN_VERSION 3.3.9
ENV MAVEN_HOME /usr/local/maven

RUN mkdir -p ${MAVEN_HOME} && \
    curl -fsSL http://mirrors.cnnic.cn/apache/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz | \
    tar -xzC ${MAVEN_HOME} --strip-components=1 && \
    ln -s ${MAVEN_HOME}/bin/mvn /usr/bin/mvn 

VOLUME /root/.m2

CMD ["mvn"]
