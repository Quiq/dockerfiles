FROM ubuntu:18.04 as builder

WORKDIR /opt

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y install \
        build-essential \
        cmake \
        git \
        libglib2.0-dev \
        default-libmysqlclient-dev \
        libpcre3-dev \
        libssl-dev \
        zlib1g-dev

RUN git clone https://github.com/maxbube/mydumper.git && \
    cd mydumper && \
    cmake . && \
    make


FROM ubuntu:18.04

COPY --from=builder /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1 /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libglib-2.0.so.0 /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libgthread-2.0.so.0 /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libmysqlclient.so.20 /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libssl.so.1.1 /usr/lib/x86_64-linux-gnu/

COPY --from=builder /opt/mydumper/mydumper /usr/local/bin/
COPY --from=builder /opt/mydumper/myloader /usr/local/bin/
