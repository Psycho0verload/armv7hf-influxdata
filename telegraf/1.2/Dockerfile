FROM yangxuan8282/armhf-ubuntu-qemu

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

ENV TELEGRAF_VERSION 1.2.1
RUN wget -q https://dl.influxdata.com/telegraf/releases/telegraf_${TELEGRAF_VERSION}_armhf.deb.asc && \
    wget -q https://dl.influxdata.com/telegraf/releases/telegraf_${TELEGRAF_VERSION}_armhf.deb && \
    gpg --batch --verify telegraf_${TELEGRAF_VERSION}_armhf.deb.asc telegraf_${TELEGRAF_VERSION}_armhf.deb && \
    dpkg -i telegraf_${TELEGRAF_VERSION}_armhf.deb && \
    rm -f telegraf_${TELEGRAF_VERSION}_armhf.deb*
RUN [ "cross-build-end" ]
EXPOSE 8125/udp 8092/udp 8094

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["telegraf"]
