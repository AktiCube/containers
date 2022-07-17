FROM --platform=linux/amd64 ghcr.io/graalvm/jdk:java17

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/containers/"

RUN apt-get update -y \
    && apt-get install -y lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 \
    && useradd -d /home/container -m container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]