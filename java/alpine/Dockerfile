FROM adoptopenjdk/openjdk11:x86_64-alpine-jre-11.0.4_11

COPY honest-profiler-12.07.2019.zip /tmp/honest-profiler.zip

RUN set -ex && \
  sed -i 's/http:\/\/dl-cdn/https:\/\/dl-4/' /etc/apk/repositories && \
  apk --no-cache upgrade && \
  apk add --no-cache ca-certificates dumb-init curl bash && \
  mkdir /service && \
  cd /tmp && \
  echo "f54185d12c29ce493b825a416df91ce2c47610db2563d4799fa92b1ce4ae700d  honest-profiler.zip" | sha256sum -c - && \
  unzip honest-profiler.zip && \
  cp liblagent.so /service/ && \
  rm -rf /tmp/*

WORKDIR /service

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
