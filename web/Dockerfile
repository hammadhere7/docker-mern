FROM node:18-alpine

WORKDIR /web
ARG WEB_PORT

COPY package*.json .

RUN npm install

COPY . .

EXPOSE ${WEB_PORT}

ENV PORT=${WEB_PORT}

CMD ["npm","start"]