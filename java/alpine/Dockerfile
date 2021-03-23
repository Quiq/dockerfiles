FROM adoptopenjdk/openjdk11:x86_64-alpine-jre-11.0.10_9

RUN set -ex && \
  sed -i 's/http:\/\/dl-cdn/https:\/\/dl-4/' /etc/apk/repositories && \
  apk add --no-cache ca-certificates dumb-init bash && \
  mkdir /service

# Add honest-profiler lib, s3://quiq-deployment/honest-profiler-12.07.2019.zip
ADD liblagent.so /service

WORKDIR /service

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
