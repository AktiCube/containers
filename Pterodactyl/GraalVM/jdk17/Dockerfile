FROM --platform=$TARGETOS/$TARGETARCH ubuntu:20.04

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/AktiCube/containers/"

# Linux things

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'
ARG DEBIAN_FRONTEND=noninteractive

# Installing GraalVM JDK 17

ARG GRAALVM_VERSION=22.1.0
ARG JAVA_VERSION=java17
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-${JAVA_VERSION}-linux-amd64-${GRAALVM_VERSION}.tar.gz

RUN apt-get update -y \
    && apt-get install -y lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6

RUN echo ${GRAALVM_PKG}
RUN cd /usr/local/ \
    && echo "Downloading GraalVM..." && curl --retry 3 -Lfso /tmp/graalvm.tar.gz ${GRAALVM_PKG} \
    && echo "Extracting GraalVM..." && tar -xzf /tmp/graalvm.tar.gz \
    && echo "Delete temp file..." && rm /tmp/graalvm.tar.gz && ls

RUN echo "Set as default JVM..."
ENV JAVA_HOME=/usr/local/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}}
ENV PATH=/usr/local/graalvm-ce-${JAVA_VERSION}-${GRAALVM_VERSION}}/bin:${PATH}
RUN java -version 

# Pterodactyl things

RUN useradd -d /home/container -m container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]