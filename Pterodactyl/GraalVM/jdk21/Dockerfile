FROM --platform=$TARGETOS/$TARGETARCH amd64/debian

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/AktiCube/containers/"
LABEL org.opencontainers.image.description="A Docker image to run GraalVM JDK 21 on Pterodactyl" 

# Linux things
ARG DEBIAN_FRONTEND=noninteractive

# Installing GraalVM JDK 21
ARG GRAALVM_VERSION=21.0.0
ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/jdk-${GRAALVM_VERSION}/graalvm-community-jdk-${GRAALVM_VERSION}_linux-x64_bin.tar.gz
ARG GRAALVM_FOLDER_NAME=graalvm-community-openjdk-21+35.1

RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 locales locales-all font-manager

# Setting the language
RUN echo "Setting the language..."
ENV LANG='en_US.UTF-8'
ENV LANGUAGE='en_US.UTF-8'
ENV LC_ALL='en_US.UTF-8'
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

RUN echo ${GRAALVM_PKG}
RUN cd /usr/local/ \
    && echo "Downloading GraalVM..." && curl --retry 3 -Lfso /tmp/graalvm.tar.gz ${GRAALVM_PKG} \
    && echo "Extracting GraalVM..." && tar -xzf /tmp/graalvm.tar.gz \
    && echo "Delete temp file..." && rm /tmp/graalvm.tar.gz

RUN echo "Mooving GraalVM..." && mkdir /usr/local/graalvm && mv /usr/local/${GRAALVM_FOLDER_NAME}/* /usr/local/graalvm && rm -rf /usr/local/${GRAALVM_FOLDER_NAME}

RUN echo "Setting GraalVM environment variables..."
ENV JAVA_HOME=/usr/local/graalvm
ENV PATH=/usr/local/graalvm/bin:${PATH}

# Check if the installation was successful
RUN java -version 

# Pterodactyl things
RUN useradd -d /home/container -m container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]
