# MERN App Docker Boilerplate

This is a boilerplate to kickstart your mern stack application with docker. Rather than setting up all the services from scratch and running the containers I have setup the apps from scratch and you can start working in them right away. Use it to develop any MERN stack project and replace
your web and backend projects and build with `docker-compose`

### Initializing the Project

Clone the GitHub link to a local folder in your computer. Open the folder using [VSCode ](https://code.visualstudio.com/download)or any text editor of your choice.


### React App

The react application is created using `create-react-app` by default it runs on port `3000` which can be changed in environment file to listen on different port. This app is named with folder `web` in the project contains following `Dockerfile` to build it's image: 

```yml
FROM node:18-alpine

WORKDIR /web
ARG WEB_PORT
COPY package*.json .

RUN npm install
COPY . .

EXPOSE ${WEB_PORT}
ENV PORT=${WEB_PORT}
CMD ["npm","start"]

```

### Express App

A fresh express app is included in the folder named `api` where you can start working on express backend project. The express app runs on port `3000` by default which can be changed in environment variable to listen on different port. This project is contained in the directory `api` and contains following `Dockerfile` to build the image:

```yml
FROM node:18-alpine

WORKDIR /api
ARG API_PORT

COPY package.json .

RUN npm install

COPY . .

EXPOSE ${API_PORT}

ENV PORT=${API_PORT}

CMD ["npm","start"]
```

### Mongo Service

Mongo DB service is instantiated in `docker-compose.yaml` file. If you need to seed the database with pre-existing data then open an issue and I will help you with that.

### Configuration

Rename `.env-sample` to `.env` and set following ports to your desired system ports: 

<code>
WEB_PORT=3000 <br/>
API_PORT=5000 <br/>
MONGO_PORT=27017 <br/>
</code>

You can set the above ports to any different ports that you would like to:

### Build

After changing the ports to your desired ports simply run following command to build and run the services in their respective containers

`docker-compose up --build`

Once you are done developing then you can run following command to bring down the contains: 

`docker-compose down`

### Adding additional environment variables

Often you need to set additional environment variables for either react app or express app. In order to add additional environment variables follow below mentioned steps: 

- Add additional environment variable `.env` file
- Now reference the same variable in `docker-compose.yaml` file in respective service's build section under `args` key and set the a new variable there. 
- Reference the newly set variable from step two in respective service `Dockerfile` with `ARG` keyword. 
- Before CMD command use `ENV` keyword to setup new environment variable and that environment variable will be available in `process.env` object. 

#### Example

Let's say you want to add new environment variable named `STRIPE_SECRET_KEY` with the value `sk_Dlakfsdfklasklladslkflkds_dfskdka` into express app. You need to follow these steps: 

- Open .env file and define new environment variable as `STRIPE_SECRET_KEY=sk_Dlakfsdfklasklladslkflkds_dfskdka`
- Open `docker-compose.yaml` file and set new argument in `args` section under build property of `api` service. Like this:

```yaml
      args:
        - STRIPE_SECRET_KEY=${STRIPE_SECRET_KEY} #STRIPE_SECRET_KEY is automatically loaded from .env file
```

- Open `Dockerfile` in `api` folder add reference the above argument like this: 

```yaml
ARG STRIPE_KEY
```

- Now set this arg as environment variable to express app with following line: 

```yaml
ENV STRIPE_KEY=STRIPE_KEY
```