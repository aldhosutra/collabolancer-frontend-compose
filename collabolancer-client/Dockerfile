FROM node:lts-alpine as build
RUN apk add --no-cache git libtool autoconf automake build-base python

WORKDIR /app
RUN git clone https://github.com/aldhosutra/collabolancer-client .
ENV PATH /app/node_modules/.bin:$PATH
RUN npm ci
RUN npm run build

FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
COPY --from=build /app/env.sh /usr/share/nginx/html/env.sh
COPY client.env /usr/share/nginx/html/client.env
RUN rm -rf /app
RUN chmod +x /usr/share/nginx/html/env.sh

WORKDIR /usr/share/nginx/html/
RUN apk add --no-cache bash

COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD /bin/bash /usr/share/nginx/html/env.sh && nginx -g "daemon off;"
