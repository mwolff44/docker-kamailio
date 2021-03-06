FROM debian:9

MAINTAINER Mathias WOLFF <mathias@celea.org>

# Based on the work of Victor Selva
# https://github.com/kamailio/kamailio-docker/blob/c08dc2f896a7351fef469e31c6fa8c08624b2441/stretch/Dockerfile

# Important! Update this no-op ENV variable when this Dockerfile
# is updated with the current date. It will force refresh of all
# of the base images and things like 'apt-get update' won't be using
# old cached versions when the Dockerfile is built.
ENV REFRESHED_AT 2019-07-17

# avoid httpredir errors
RUN sed -i 's/httpredir/deb/g' /etc/apt/sources.list

RUN rm -rf /var/lib/apt/lists/* && apt-get update &&   apt-get install --assume-yes gnupg wget

RUN echo "deb http://deb.kamailio.org/kamailio52 stretch main" > /etc/apt/sources.list.d/kamailio.list
RUN wget -O- http://deb.kamailio.org/kamailiodebkey.gpg | apt-key add -

RUN apt-get update && apt-get install -y kamailio kamailio-extra-modules kamailio-outbound-modules kamailio-postgres-modules kamailio-tls-modules kamailio-redis-modules kamailio-xml-modules curl tcpdump

#setup dumb-init
RUN curl -k -L https://github.com/Yelp/dumb-init/releases/download/v1.2.1/dumb-init_1.2.1_amd64 > /usr/bin/dumb-init
RUN chmod 755 /usr/bin/dumb-init

# Config files.
VOLUME /etc/kamailio

# clean
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["kamailio", "-DD", "-E"]
