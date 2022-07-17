FROM --platform=$TARGETOS/$TARGETARCH ubuntu:20.04

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/AktiCube/containers/"

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'
ARG DEBIAN_FRONTEND=noninteractive

ARG GRAALVM_VERSION=22.1.0
ARG JAVA_VERSION=java17
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-${JAVA_VERSION}-linux-amd64-${GRAALVM_VERSION}.tar.gz

RUN apt-get update -y \
    && apt-get install -y lsof curl wget ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 \
    && useradd -d /home/container -m container

RUN echo ${GRAALVM_PKG}
RUN mkdir /usr/lib/jvm/
RUN cd /usr/lib/jvm/ \
    && echo "Downloading GraalVM..." && wget ${GRAALVM_PKG} \
    && echo "Extracting GraalVM..." && tar -xzf graalvm-ce-${JAVA_VERSION}-linux-amd64-${GRAALVM_VERSION}.tar.gz \
    && echo "Delete temp file..." && rm graalvm-ce-${JAVA_VERSION}-linux-amd64-${GRAALVM_VERSION}.tar.gz

RUN echo "Set as default JVM..."
ENV JAVA_HOME=/usr/lib/jvm/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}} \
    PATH="/usr/lib/jvm/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}}/bin/java:$PATH"

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]