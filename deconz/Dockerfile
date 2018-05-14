ARG BUILD_FROM=hassioaddons/debian-base-armhf:0.2.2
FROM ${BUILD_FROM}

# Setup base system
RUN \
    apt-get update \
    && apt-get -y --no-install-recommends install wget curl libqt5core5a libqt5network5 libqt5widgets5 libqt5gui5 libqt5serialport5 libqt5websockets5 libqt5sql5 libcap2-bin sqlite3 \
    && apt-get -y autoremove && apt-get clean
RUN \
    cd /tmp && wget -q http://www.dresden-elektronik.de/rpi/deconz/beta/deconz-2.05.26-qt5.deb \
    && dpkg -i --ignore-depends=wiringpi /tmp/deconz-2.05.26-qt5.deb
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN ln -s /config/deconz/local /root/.local && ln -s /config/deconz/otau /root/otau

# deconz will not start in docker with capabilities
RUN \
    setcap -r /usr/bin/deCONZ


# Copy root filesystem
COPY rootfs /

# Build arugments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="deconz" \
    io.hass.description="deCONZ REST plugin provides a REST API to access ZigBee" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="David Kreitschmann <d@o7o.de>" \
    org.label-schema.description="deCONZ REST plugin provides a REST API to access ZigBee" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="deconz" \
    org.label-schema.schema-version="1.0" \
#    org.label-schema.url="https://community.home-assistant.io/t/community-hass-io-add-on-airsonos/36796?u=frenck" \
#    org.label-schema.usage="https://github.com/hassio-addons/addon-airsonos/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
#    org.label-schema.vcs-url="https://github.com/hassio-addons/addon-airsonos" \
#    org.label-schema.vendor="Community Hass.io Add-ons"
