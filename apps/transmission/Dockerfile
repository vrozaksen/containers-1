# NOTE: When updating the alpine version, remember to update metadata.yaml
# file with the new alpine version number in the renovate annotation.
FROM docker.io/library/python:3.13-alpine3.21

ARG TARGETPLATFORM
ARG VERSION
ARG CHANNEL

ENV HOME=/config \
    XDG_CONFIG_HOME=/config \
    XDG_DATA_HOME=/config \
    TRANSMISSION__RPC_PORT=9091 \
    TRANSMISSION_WEB_HOME=/usr/share/transmission/public_html

#hadolint ignore=DL3018
RUN \
    apk add --no-cache \
        bash \
        ca-certificates \
        catatonit \
        coreutils \
        curl \
        jo \
        jq \
        nano \
        p7zip \
        transmission-daemon="${VERSION}" \
        transmission-cli \
        transmission-extra \
        transmission-remote \
        trurl \
        tzdata \
    && \
    case "${TARGETPLATFORM}" in \
        'linux/amd64') export ARCH="x86_64" ;; \
        'linux/arm64') export ARCH="aarch64" ;; \
    esac \
    && \
    curl -fsSL https://github.com/mitsuhiko/minijinja/releases/download/2.8.0/minijinja-cli-${ARCH}-unknown-linux-musl.tar.xz \
        | tar xJf - -C /usr/local/bin/ --strip-components=1 \
    && \
    rm -rf /tmp/*

COPY . /

COPY --from=ghcr.io/linuxserver/unrar:latest /usr/bin/unrar-alpine /usr/bin/unrar

USER nobody:nogroup
WORKDIR /config
VOLUME ["/config"]

ENTRYPOINT ["/usr/bin/catatonit", "--", "/entrypoint.sh"]
