FROM resin/armv7hf-debian-qemu

RUN [ "cross-build-start" ]
RUN apt-get update
RUN apt-get -y install apt-utils
RUN	apt-get -y install wget

RUN set -ex && \
    for key in \
        05CE15085FC09D18E99EFB22684A14CF2582E0C5 ; \
    do \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key" || \
        gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
        gpg --keyserver keyserver.pgp.com --recv-keys "$key" ; \
    done

ENV INFLUXDB_VERSION 1.1.5
RUN wget -q https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_armhf.deb.asc && \
    wget -q https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    gpg --batch --verify influxdb_${INFLUXDB_VERSION}_armhf.deb.asc influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    dpkg -i influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    rm -f influxdb_${INFLUXDB_VERSION}_armhf.deb*
COPY influxdb.conf /etc/influxdb/influxdb.conf
RUN [ "cross-build-end" ]
EXPOSE 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
