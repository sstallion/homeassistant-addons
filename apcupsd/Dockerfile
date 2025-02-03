ARG BUILD_FROM

FROM alpine AS apcupsd

RUN apk add \
    alpine-conf \
    alpine-sdk \
    sudo

RUN git clone --depth 1 https://gitlab.alpinelinux.org/alpine/aports.git

RUN --mount=target=/aports/main/apcuspd,source=./apcupsd \
    abuild-keygen -a -i -n \
    && abuild -Fr -C /aports/main/apcupsd -P /packages

FROM $BUILD_FROM

# Install requirements
RUN --mount=target=/mnt,source=/packages,from=apcupsd \
    apk add --allow-untrusted --no-cache /mnt/main/*/apcupsd-3.14.14*.apk

# /dev/log is symlinked to /run/systemd/journal/dev-log by the container host;
# the parent path must exist for syslogd to create the log socket at startup.
RUN mkdir -p /run/systemd/journal

# Copy root filesystem
COPY rootfs /

HEALTHCHECK CMD netstat -ltn | grep -qF 0.0.0.0:3551
