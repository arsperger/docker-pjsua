FROM debian:stretch-slim

LABEL maintainer="Arsen Semenov <arsperger@gmail.com>" \
      org.label-schema.docker.dockerfile="/Dockerfile" \
      org.label-schema.license="MIT" \
      org.label-schema.name="Node / Debian" \
      org.label-schema.vcs-type="Git" \
      org.label-schema.vcs-url="https://github.com/arsperger/docker-pjsua"

ENV PJPROJECT_VERSION=2.10

WORKDIR /usr/local/src

RUN echo 'APT::Install-Recommends 0;' >> /etc/apt/apt.conf.d/01norecommends \
	&& echo 'APT::Install-Suggests 0;' >> /etc/apt/apt.conf.d/01norecommends \
	&& apt-get update \
	&& DEBIAN_FRONTEND=noninteractive \ 
	&& apt-get install -y curl bzip2 build-essential libasound2-dev ca-certificates \
	&& curl -sfSL https://github.com/pjsip/pjproject/archive/${PJPROJECT_VERSION}.tar.gz | tar -xzC /usr/local/src \
	&& cd pjproject-${PJPROJECT_VERSION} \
	&& ./configure \ 
	&& make dep \
	&& make \
	&& cp pjsip-apps/bin/pjsua-*gnu /usr/bin/pjsua \
	&& apt-get purge --force-yes build-essential \
	&& rm -rf /var/lib/apt/lists/* \

COPY . /opt/
ENV PATH="/opt:${PATH}"

ENTRYPOINT ["docker-entrypoint.sh"]
