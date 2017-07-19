FROM debian:stretch-slim
MAINTAINER ryan.huddleston@centricient.com

ENV JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=131 \
    JAVA_VERSION_BUILD=11 \
    JAVA_PACKAGE=server-jre \
    JAVA_JCE=unlimited \
    JAVA_HOME=/opt/jdk \
    PATH=${PATH}:/opt/jdk/bin \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8 \
    DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1
#    LD_PRELOAD=/usr/lib64/libksm_preload.so

#ADD libksm_preload.so /usr/lib64/

RUN set -ex && \
    echo 'APT::Install-Recommends "0";' > /etc/apt/apt.conf.d/01norecommend && \
    echo 'APT::Install-Suggests "0";' >> /etc/apt/apt.conf.d/01norecommend && \
    apt-get update && \
    LANG= LANGUAGE= LC_ALL= DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install locales && \
    locale-gen en_US.UTF-8 && \
    sed -i 's/^# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    LANG= LANGUAGE= LC_ALL= DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales && \
    DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install curl ca-certificates unzip python3-minimal python3-requests && \
    echo "export LANG=en_US.UTF-8" > /etc/profile.d/locale.sh && \
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/java.tar.gz \
      https://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/d54c1d3a095b4ff2b6607d096fa80163/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz && \
    echo "a80634d17896fe26e432f6c2b589ef6485685b2e717c82cd36f8f747d40ec84b  /tmp/java.tar.gz" | sha256sum -c --status -  && \
    tar -C /opt -xf /tmp/java.tar.gz && \
    ln -s /opt/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} /opt/jdk && \
    find /opt/jdk/ -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
    cd /opt/jdk/ && ln -s ./jre/bin ./bin && \
    if [ "${JAVA_JCE}" = "unlimited" ]; then echo "Installing Unlimited JCE policy" && \
      curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip \
        https://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION_MAJOR}/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
      echo "f3020a3922efd6626c2fff45695d527f34a8020e938a49292561f18ad1320b59  /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip" | sha256sum -c --status -  && \
      cd /tmp && unzip /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
      cp -v /tmp/UnlimitedJCEPolicyJDK8/*.jar /opt/jdk/jre/lib/security/; \
    fi && \
    sed -i s/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/ $JAVA_HOME/jre/lib/security/java.security && \
    curl -L -o dumb-init_1.2.0_amd64  https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 && \
    echo "81231da1cd074fdc81af62789fead8641ef3f24b6b07366a1c34e5b059faf363  dumb-init_1.2.0_amd64" | sha256sum -c --status -  && \
    mv dumb-init_1.2.0_amd64 /usr/bin/dumb-init && \
    chmod 755 /usr/bin/dumb-init && \
    cd /tmp && \
    curl -Lo honest-profiler.zip http://insightfullogic.com/honest-profiler.zip && \
    echo "378d5506d76d205f8c846e7dfdc701072ecb361a787365899f5a0637717db43f  honest-profiler.zip" | sha256sum -c --status -  && \
    unzip honest-profiler.zip && \
    mkdir /service && \
    cp liblagent.so /service/ && \
    apt-get remove --purge -y curl unzip && \
    apt-get autoremove -y && \
    apt-get -y clean all && \
    dpkg --list |awk '$1 ~ /^rc$/ {print $2}' | xargs dpkg --purge && \
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
           /tmp/* /var/lib/apt/lists/*

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
