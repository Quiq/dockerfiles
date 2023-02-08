FROM alpine:3.16

RUN set -ex && \
    apk add --no-cache python3 py3-pip ca-certificates dumb-init patch && \
    wget -O - https://static.quiq-cdn.com/provision/client39.patch | patch -u /usr/lib/python3.10/http/client.py

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
