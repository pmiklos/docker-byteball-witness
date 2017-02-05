FROM node:7.4-alpine

RUN	deluser --remove-home node \
	&& adduser -D -u 1000 byteball \
	&& apk add --no-cache --virtual .build-deps git \
	&& npm install -g https://github.com/byteball/byteball-witness.git \ 
	&& apk del .build-deps \
	&& mkdir /byteball /home/byteball/.config \
	&& chown byteball:byteball /byteball /home/byteball/.config \
	&& ln -s /byteball /home/byteball/.config/byteball-witness

VOLUME /byteball

USER byteball
WORKDIR /home/byteball

EXPOSE 6611

CMD [ "/bin/sh", "-c", "node /usr/local/lib/node_modules/byteball-witness/start.js 2>> /byteball/error.log" ]

