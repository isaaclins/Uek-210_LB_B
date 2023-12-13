FROM node:19-alpine

RUN apk add --no-cache dos2unix python3 make g++

ENV npm_config_cache ./.npm-cache

WORKDIR /srv/app

COPY package.json package-lock.json ./

RUN npm ci && \
    npm install bcrypt@5.0.1 

COPY . .

RUN dos2unix entrypoints/*

USER node

EXPOSE 8080

ENTRYPOINT ["entrypoints/production.sh"]

CMD ["npm", "start"]
