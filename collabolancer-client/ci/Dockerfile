FROM node:lts-alpine as build
RUN apk add --no-cache git libtool autoconf automake build-base python

WORKDIR /app
RUN git clone https://github.com/aldhosutra/collabolancer-client .
ENV PATH /app/node_modules/.bin:$PATH
RUN npm ci
RUN npm run build
RUN rm -rf /app/node_modules

FROM nginx:stable-alpine
RUN apk add --no-cache python3 python3-dev py3-pip build-base libressl-dev musl-dev libffi-dev bash
RUN pip3 install pip --upgrade
RUN pip3 install certbot-nginx
RUN mkdir /etc/letsencrypt

COPY --from=build /app/build /usr/share/nginx/html
COPY --from=build /app/env.sh /usr/share/nginx/html/env.sh
COPY client.env /usr/share/nginx/html/client.env
RUN chmod +x /usr/share/nginx/html/env.sh

COPY crontab.txt /crontab.txt
RUN /usr/bin/crontab /crontab.txt

WORKDIR /usr/share/nginx/html/

EXPOSE 80
CMD /usr/sbin/crond && /bin/bash /usr/share/nginx/html/env.sh && nginx -g "daemon off;"
