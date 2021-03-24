ARG BUILD_FROM=ghcr.io/hassio-addons/base/amd64:9.1.5
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        build-base=0.5-r2 \
        git=2.30.2-r0 \
        go=1.15.10-r0 \
    \
    && git clone --depth=1 \
        "https://github.com/jhead/phantom.git" /tmp/phantom \
    \
    && cd /tmp/phantom/cmd \
    && go build -o /usr/bin/phantom phantom.go \
    && chmod +x /usr/bin/phantom \
    \
    && rm -f -r /tmp/* \
    && apk del --no-cache --purge \
        .build-dependencies

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH=amd64
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Marvin Blakley <marv@blakley.dev>"