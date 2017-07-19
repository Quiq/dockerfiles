FROM alpine:3.6
MAINTAINER talon.daniels@centricient.com

RUN set -ex && \
    sed -i 's/http:/https:/' /etc/apk/repositories && \
    apk upgrade --update && \
    apk add --update --no-cache python py-pip ca-certificates curl && \
    curl -Lo dumb-init_1.2.0_amd64 https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 && \
    echo "81231da1cd074fdc81af62789fead8641ef3f24b6b07366a1c34e5b059faf363  dumb-init_1.2.0_amd64" | sha256sum -c -s - && \
    mv dumb-init_1.2.0_amd64 /usr/bin/dumb-init && \
    chmod 755 /usr/bin/dumb-init && \
    apk del curl

ENTRYPOINT ["/usr/bin/dumb-init", "--"]

