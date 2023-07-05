FROM --platform=$TARGETOS/$TARGETARCH amd64/debian

LABEL author="Louis Ravignot Dos Santos" maintainer="louis.rds@akticube.fr"
LABEL org.opencontainers.image.source="https://github.com/AktiCube/containers/"
LABEL org.opencontainers.image.description="A Docker image to run NodeJS 18 and Java 11 on Pterodactyl" 

# Linux things
ARG DEBIAN_FRONTEND=noninteractive

# Installing NodeJS 18 and Java 11
RUN apt-get update -y \
    && apt-get install -y lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 locales locales-all font-manager default-jdk \
    && curl -sL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs

# Setting the language
RUN echo "Setting the language..."
ENV LANG='en_US.UTF-8'
ENV LANGUAGE='en_US.UTF-8'
ENV LC_ALL='en_US.UTF-8'
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

# Check if the installation was successful
RUN java -version
RUN node -v

# Pterodactyl things
RUN useradd -d /home/container -m container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./../entrypoint.sh /entrypoint.sh
CMD [ "/bin/bash", "/entrypoint.sh" ]
