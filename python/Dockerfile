FROM alpine:3.10

RUN set -ex && \
    sed -i 's/http:\/\/dl-cdn/https:\/\/dl-4/' /etc/apk/repositories && \
    apk add --no-cache python3 py3-pip ca-certificates dumb-init

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
