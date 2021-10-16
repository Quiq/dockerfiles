FROM alpine:3.14

RUN set -ex && \
    apk add --no-cache python3 py3-pip ca-certificates dumb-init

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
