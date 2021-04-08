FROM centos:7

ARG SLARCHIVE_VERSION=slarchive-2.2
ARG SLARCHIVE_DOWNLOAD=http://ds.iris.edu/pub/programs/SeedLink/${SLARCHIVE_VERSION}.tar.gz

# Create the user which seiscomp3 will run as
RUN groupadd --gid 1000 intermagnet \
  && useradd --uid 1000 --gid intermagnet --shell /bin/bash --home /opt/slarchive slarchive

# Update the yum and install required packages
RUN set -ex \
    && yum update -y \
    && yum install -y curl gcc make tar

# Make data directory and change ownership
RUN mkdir -m 770 /data && chown slarchive:intermagnet /data

# Change the slarchive user
USER slarchive
WORKDIR /opt/slarchive

# Download slarchive and make data directory
RUN set -ex \
    && curl ${SLARCHIVE_DOWNLOAD} | tar xz

WORKDIR /opt/slarchive/${SLARCHIVE_VERSION}
RUN make

VOLUME [ "/data" ]

ENTRYPOINT [ "./slarchive", "-x", "slarchive.state", "-SDS", "/data", "-s", "?F?", "-v"]
