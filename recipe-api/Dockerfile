#!/bin/bash
FROM node:18.14.1-alpine3.16 AS deps

WORKDIR /srv
COPY package*.json ./
RUN npm ci --only=production



FROM alpine:3.16 AS release

ENV V 18.14.1
ENV FILE node-v$V-darwin-arm64.tar.gz

RUN apk add libstdc++ 
RUN apk add --virtual .deps curl 
RUN curl -fsSLO --compressed \
    "https://nodejs.org/dist/v$V/$FILE" 
RUN tar -xf $FILE -C /usr/local --strip-components=1 
RUN rm -f $FILE /usr/local/bin/npm /usr/local/bin/npx 
RUN rm -rf /usr/local/lib/node_modules 
RUN apk del .deps

WORKDIR /srv
COPY --from=deps /srv/node_modules ./node_modules
COPY . .

EXPOSE 1337
ENV HOST 0.0.0.0
ENV PORT 1337
CMD [ "node", "producer-http-basic.js" ]