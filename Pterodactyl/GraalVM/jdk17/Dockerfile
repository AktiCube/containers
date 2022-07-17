FROM --platform=$TARGETOS/$TARGETARCH ubuntu:20.04

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/containers/"

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

ARG GRAALVM_VERSION=22.1.0
ARG JAVA_VERSION=java17
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-${JAVA_VERSION}-GRAALVM_ARCH-${GRAALVM_VERSION}.tar.gz

RUN mkdir /usr/lib/jvm/
RUN cd /usr/lib/jvm/
RUN wget $GRAALVM_PKG
RUN tar -xzf graalvm-ce-${JAVA_VERSION}-GRAALVM_ARCH-${GRAALVM_VERSION}.tar.gz
RUN rm graalvm-ce-${JAVA_VERSION}-GRAALVM_ARCH-${GRAALVM_VERSION}.tar.gz

ENV JAVA_HOME=/usr/lib/jvm/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}} \
    PATH="/usr/lib/jvm/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}}/bin:$PATH"

RUN apt-get update -y \
    && apt-get install -y lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 \
    && useradd -d /home/container -m container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]