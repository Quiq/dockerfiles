FROM alpine:3.6
MAINTAINER ryan.huddleston@centricient.com

ENV JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=144 \
    JAVA_VERSION_BUILD=01 \
    JAVA_PACKAGE=server-jre \
    JAVA_JCE=unlimited \
    JAVA_HOME=/opt/jdk \
    PATH=${PATH}:/opt/jdk/bin \
    GLIBC_VERSION=2.25-r0 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8 \
    PYTHONUNBUFFERED=1

RUN set -ex && \
    sed -i 's/http:/https:/' /etc/apk/repositories && \
    apk upgrade --update && \
    apk add --update --no-cache libstdc++ curl ca-certificates bash && \
    curl -Lo /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub && \
    for pkg in glibc-${GLIBC_VERSION} glibc-bin-${GLIBC_VERSION} glibc-i18n-${GLIBC_VERSION}; do curl -sSL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/${pkg}.apk -o /tmp/${pkg}.apk; done && \
    echo "b1747912145766364458f67aa6d9b81a3d7f62d7207962d972a56ca935461e50  /tmp/glibc-${GLIBC_VERSION}.apk" | sha256sum -c -s -  && \
    echo "0999174703974a4f2fafbfcc69bd1b80cc99cfadaed72466485b2b24ceb14ba3  /tmp/glibc-bin-${GLIBC_VERSION}.apk" | sha256sum -c -s -  && \
    echo "3dccbac67760693b50bf01bfb94e292b570ba24e7fbba2cf98417f06ce852dc4  /tmp/glibc-i18n-${GLIBC_VERSION}.apk" | sha256sum -c -s -  && \
    apk add /tmp/*.apk && \
    rm -v /tmp/*.apk && \
    ( /usr/glibc-compat/bin/localedef --force --inputfile en_US --charmap UTF-8 en_US.UTF-8 || true ) && \
    echo "export LANG=en_US.UTF-8" > /etc/profile.d/locale.sh && \
    /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc-compat/lib && \
    mkdir /opt && \
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/java.tar.gz \
      https://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/090f390dda5b47b9b721c7dfaa008135/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz && \
    echo "8ba6f1c692518beb0c727c6e1fb8c30a5dfcc38f8ef9f4f7c7c114c01c747ebc  /tmp/java.tar.gz" | sha256sum -c -s -  && \
    tar -C /opt -xf /tmp/java.tar.gz && \
    ln -s /opt/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} /opt/jdk && \
    find /opt/jdk/ -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
    cd /opt/jdk/ && ln -s ./jre/bin ./bin && \
    if [ "${JAVA_JCE}" = "unlimited" ]; then echo "Installing Unlimited JCE policy" && \
      curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip \
        https://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION_MAJOR}/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
      echo "f3020a3922efd6626c2fff45695d527f34a8020e938a49292561f18ad1320b59  /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip" | sha256sum -c -s -  && \
      cd /tmp && unzip /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
      cp -v /tmp/UnlimitedJCEPolicyJDK8/*.jar /opt/jdk/jre/lib/security/; \
    fi && \
    sed -i s/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/ $JAVA_HOME/jre/lib/security/java.security && \
    curl -Lo dumb-init_1.2.0_amd64 https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 && \
    echo "81231da1cd074fdc81af62789fead8641ef3f24b6b07366a1c34e5b059faf363  dumb-init_1.2.0_amd64" | sha256sum -c -s -  && \
    mv dumb-init_1.2.0_amd64 /usr/bin/dumb-init && \
    chmod 755 /usr/bin/dumb-init && \
    cd /tmp && \
    curl -Lo honest-profiler.zip http://insightfullogic.com/honest-profiler.zip && \
    echo "378d5506d76d205f8c846e7dfdc701072ecb361a787365899f5a0637717db43f  honest-profiler.zip" | sha256sum -c -s -  && \
    unzip honest-profiler.zip && \
    mkdir /service && \
    cp liblagent.so /service/ && \
    apk del curl glibc-i18n && \
    rm -rf /opt/jdk/jre/plugin \
           /opt/jdk/jre/bin/javaws \
           /opt/jdk/jre/bin/jjs \
           /opt/jdk/jre/bin/orbd \
           /opt/jdk/jre/bin/pack200 \
           /opt/jdk/jre/bin/policytool \
           /opt/jdk/jre/bin/rmid \
           /opt/jdk/jre/bin/rmiregistry \
           /opt/jdk/jre/bin/servertool \
           /opt/jdk/jre/bin/tnameserv \
           /opt/jdk/jre/bin/unpack200 \
           /opt/jdk/jre/lib/javaws.jar \
           /opt/jdk/jre/lib/deploy* \
           /opt/jdk/jre/lib/desktop \
           /opt/jdk/jre/lib/*javafx* \
           /opt/jdk/jre/lib/*jfx* \
           /opt/jdk/jre/lib/amd64/libdecora_sse.so \
           /opt/jdk/jre/lib/amd64/libprism_*.so \
           /opt/jdk/jre/lib/amd64/libfxplugins.so \
           /opt/jdk/jre/lib/amd64/libglass.so \
           /opt/jdk/jre/lib/amd64/libgstreamer-lite.so \
           /opt/jdk/jre/lib/amd64/libjavafx*.so \
           /opt/jdk/jre/lib/amd64/libjfx*.so \
           /opt/jdk/jre/lib/ext/jfxrt.jar \
           /opt/jdk/jre/lib/ext/nashorn.jar \
           /opt/jdk/jre/lib/oblique-fonts \
           /opt/jdk/jre/lib/plugin.jar \
           /tmp/* /var/cache/apk/*

WORKDIR /service

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
