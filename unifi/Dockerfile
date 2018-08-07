FROM ubuntu:xenial

LABEL MAINTAINER "Coder <code@brosy.com>"

ENV DEBIAN_FRONTEND=noninteractive
ENV UNIFI_VERSION=5.8.24

ENV BASEDIR=/usr/lib/unifi \
    DATADIR=/var/lib/unifi \
    LOGDIR=/var/log/unifi \
    RUNDIR=/var/run/unifi

RUN set -ex \
    && fetchDeps=' \
        ca-certificates \
        wget \
    ' \
    && apt-get update \
    && apt-get -y install binutils libcap2 curl openjdk-8-jre-headless jsvc gnupg ca-certificates

RUN echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" > /etc/apt/sources.list.d/mongodb.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv 0C49F3730359A14518585931BC711F9BA15703C6
RUN apt-get update && \
    apt-get -y install mongodb-org-server && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN curl -o unifi_sysvinit_all.deb http://dl.ubnt.com/unifi/${UNIFI_VERSION}/unifi_sysvinit_all.deb && \
    dpkg -i unifi_sysvinit_all.deb && \
    rm unifi_sysvinit_all.deb

RUN ln -s ${DATADIR} ${BASEDIR}/data
RUN ln -s ${RUNDIR} ${BASEDIR}/run
RUN ln -s ${LOGDIR} ${BASEDIR}/logs

EXPOSE 8080:8080/tcp \
       8443:8443/tcp \
       8843:8843/tcp \
       8880:8880/tcp \
       3478:3478/udp

WORKDIR /usr/lib/unifi

ENTRYPOINT ["/usr/bin/java", "-Xmx1024M", "-jar", "/usr/lib/unifi/lib/ace.jar"]

CMD ["start"]
