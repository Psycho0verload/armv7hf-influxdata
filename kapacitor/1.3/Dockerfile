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

ENV KAPACITOR_VERSION 1.3.1
RUN wget -q https://dl.influxdata.com/kapacitor/releases/kapacitor_${KAPACITOR_VERSION}_armhf.deb.asc && \
    wget -q https://dl.influxdata.com/kapacitor/releases/kapacitor_${KAPACITOR_VERSION}_armhf.deb && \
    gpg --batch --verify kapacitor_${KAPACITOR_VERSION}_armhf.deb.asc kapacitor_${KAPACITOR_VERSION}_armhf.deb && \
    dpkg -i kapacitor_${KAPACITOR_VERSION}_armhf.deb && \
    rm -f kapacitor_${KAPACITOR_VERSION}_armhf.deb*
COPY kapacitor.conf /etc/kapacitor/kapacitor.conf
RUN [ "cross-build-end" ]
EXPOSE 9092

VOLUME /var/lib/kapacitor

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["kapacitord"]
