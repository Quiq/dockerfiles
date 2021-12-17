FROM alpine:3.15

RUN set -ex && \
    apk add --no-cache python3 py3-pip ca-certificates dumb-init

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
