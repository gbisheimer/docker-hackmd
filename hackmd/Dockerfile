FROM node:boron

ADD . /hackmd
WORKDIR /hackmd

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
	wget -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add - && \
	apt-get update && \
	DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends postgresql-client-9.6 build-essential && \
	curl -L https://github.com/hackmdio/hackmd/archive/0.5.0.tar.gz | tar xzC /tmp && \
	cp -rfn /tmp/hackmd-0.5.0/* /hackmd && \
	export NODE_TLS_REJECT_UNAUTHORIZED=0 && \
	npm config set proxy http://172.17.0.1:8080 && \
    npm config set https-proxy http://172.17.0.1:8080 && \
    npm config set strict-ssl false && \
	npm install && \
	npm run build:prod && \
	npm prune --production && \
	apt-get purge -y --auto-remove build-essential && \
	apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 3000

CMD ["/bin/bash", "/hackmd/docker-entrypoint.sh"]
