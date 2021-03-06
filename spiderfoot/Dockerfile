FROM ubuntu:16.04

LABEL maintainer="warhorse@thedarkcloud.net"

# Install pre-requisites.
RUN apt-get update && apt-get install -y \
  build-essential \
  curl \
  git \ 
  libssl-dev \
  libxml2-dev \
  libxslt1-dev \
  python-pip  \
  python-dev \
  python-setuptools \
  python-lxml \
  python-m2crypto \
  python-bs4 \
  python-requests \
  swig \
  --no-install-recommends

RUN rm -rf /var/lib/apt/lists/* \
  && cd /usr/include/openssl/ \
  && ln -s ../x86_64-linux-gnu/openssl/opensslconf.h . \
  && pip install cherrypy lxml mako netaddr

# Create a dedicated/non-privileged user to run the app.
RUN addgroup spiderfoot && \
    useradd -r -g spiderfoot -d /home/spiderfoot -s /sbin/nologin -c "SpiderFoot User" spiderfoot

ENV SPIDERFOOT_VERSION 2.12.0

# Download the specified release.
WORKDIR /home
RUN curl -sSL https://github.com/smicallef/spiderfoot/archive/v$SPIDERFOOT_VERSION-final.tar.gz \
  | tar -v -C /home -xz \
  && mv /home/spiderfoot-$SPIDERFOOT_VERSION-final /home/spiderfoot
COPY ./docker-entrypoint.sh /home/spiderfoot
RUN chown -R spiderfoot:spiderfoot /home/spiderfoot
RUN chmod +x /home/spiderfoot/docker-entrypoint.sh

USER spiderfoot
WORKDIR /home/spiderfoot

EXPOSE 8080

# Run the application.
ENTRYPOINT ["/home/spiderfoot/docker-entrypoint.sh"]