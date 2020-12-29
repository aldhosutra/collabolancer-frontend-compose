FROM nginx:stable-alpine
RUN apk add --no-cache python3 python3-dev py3-pip build-base libressl-dev musl-dev libffi-dev bash
RUN pip3 install pip --upgrade
RUN pip3 install certbot-nginx
RUN mkdir /etc/letsencrypt

COPY /build /usr/share/nginx/html
COPY client.env /usr/share/nginx/html/client.env
RUN chmod +x /usr/share/nginx/html/env.sh

COPY crontab.txt /crontab.txt
RUN /usr/bin/crontab /crontab.txt

WORKDIR /usr/share/nginx/html/

EXPOSE 80
CMD /usr/sbin/crond && /bin/bash /usr/share/nginx/html/env.sh && nginx -g "daemon off;"
