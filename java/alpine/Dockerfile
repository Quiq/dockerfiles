FROM adoptopenjdk/openjdk11:x86_64-alpine-jre-11.0.18_10

RUN set -ex && \
  apk add --no-cache ca-certificates dumb-init bash && \
  mkdir /service

# Add honest-profiler lib, s3://quiq-deployment/honest-profiler-12.07.2019.zip
ADD liblagent.so /service

WORKDIR /service

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
