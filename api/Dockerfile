FROM node:18-alpine

WORKDIR /api
ARG API_PORT

COPY package.json .

RUN npm install

COPY . .

EXPOSE ${API_PORT}

ENV PORT=${API_PORT}

CMD ["npm","start"]
