ARG BUILD_FROM
FROM $BUILD_FROM

# Install requirements
RUN apk add --no-cache apcupsd

# /dev/log is symlinked to /run/systemd/journal/dev-log by the container host;
# the parent path must exist for syslogd to create the log socket at startup.
RUN mkdir -p /run/systemd/journal

# Copy root filesystem
COPY rootfs /

HEALTHCHECK CMD netstat -ltn | grep -qF 0.0.0.0:3551
