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

RUN wget -q https://dl.influxdata.com/influxdb/nightlies/influxdb_nightly_armhf.deb.asc && \
    wget -q https://dl.influxdata.com/influxdb/nightlies/influxdb_nightly_armhf.deb && \
    gpg --batch --verify influxdb_nightly_armhf.deb.asc influxdb_nightly_armhf.deb && \
    dpkg -i influxdb_nightly_armhf.deb && \
    rm -f influxdb_nightly_armhf.deb*
COPY influxdb.conf /etc/influxdb/influxdb.conf
RUN [ "cross-build-end" ]
EXPOSE 8083 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
