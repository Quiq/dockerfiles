FROM ubuntu:xenial

MAINTAINER kyle.mcintyre@centricient.com

RUN apt-get update && apt-get install -y wget software-properties-common python2.7 gcc
RUN wget https://bootstrap.pypa.io/get-pip.py && python2.7 get-pip.py && pip install requests && pip install dumb-init

RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

CMD bin/bash
