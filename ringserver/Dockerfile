FROM centos:7

ARG RINGSERVER_VERSION=2020.075
ARG RINGSERVER_DOWNLOAD=https://codeload.github.com/iris-edu/ringserver/tar.gz/refs/tags/v${RINGSERVER_VERSION}

# Create the user which ringserver will run as
RUN groupadd --gid 1000 intermagnet \
  && useradd --uid 1000 --gid intermagnet --shell /bin/bash --home /opt/ringserver ringserver

# Update the yum and install required packages
RUN set -ex \
    && yum update -y \
    && yum install -y curl gcc make tar

# Make data directory and change ownership
RUN mkdir -m 770 /data && chown ringserver:intermagnet /data

# Change the ringserver user
USER ringserver
WORKDIR /opt/ringserver

# Download ringserver and make data directory as specified in ring.conf
RUN set -ex \
    && mkdir -p /data/miniseed && mkdir -p /data/ring \
    && curl ${RINGSERVER_DOWNLOAD} | tar xz

WORKDIR /opt/ringserver/ringserver-${RINGSERVER_VERSION}
RUN make

# Copy sample configuration
COPY ring.conf .

VOLUME [ "/data/miniseed" ]
EXPOSE 18000 

ENTRYPOINT ["./ringserver", "ring.conf"]