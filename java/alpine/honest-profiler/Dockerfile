FROM ubuntu:18.04

ENV JAVA_HOME='/opt/java/openjdk'

COPY jdk-8u211-linux-x64.tar.gz /tmp/java.tar.gz

RUN apt-get update -y \
    && apt-get install -y cmake libunittest++-dev maven pkg-config git curl unzip

RUN git clone https://github.com/jvm-profiling-tools/honest-profiler.git \
    && cd honest-profiler \
    && cmake CMakeLists.txt \
    && export LC_ALL=C \
    && mvn clean package -DskipTests \
    && ls -al
