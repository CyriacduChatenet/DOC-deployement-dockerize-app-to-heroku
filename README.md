# deployement-dockerize-app-to-heroku
<img src="https://miro.medium.com/max/1838/0*3xwm0Jdbyn7geaMK.png" alt="docker heroku">
How to deploy dockerize app to heroku ?

## Example project structure
```bash
- client
|-- build
|-- src
|-- Dockerfile
- dist
- src
- Dockerfile
- docker-compose.yml
- heroku.yml
- Procfile
```
## Docker Config files
- client Dockerfile
```bash
FROM node:16.15 AS build-stage

WORKDIR /app

COPY package*.json /app/

RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine

COPY --from=build-stage /app/build/ /usr/share/nginx/html
```

- server Dockerfile
```bash
FROM node:16.15

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 4000

CMD ["npm", "start"]
```

- docker-compose.yml
```bash
version: "3"
services:
  api:
    image: api_server
    build:
      context: ./
      dockerfile: ./Dockerfile
    ports: 
      - 4000:4000
    networks: 
      - mern-stack-net
  client:
    image: react_app
    build:
      context: ./client
      dockerfile: ./Dockerfile
    ports:
      - 3000:3000
    networks: 
      - mern-stack-net
networks:
  mern-stack-net:
    driver: bridge
```

## Heroku Config files
! You need to have Dockerfile at root path of your app.

### 1. at root of your project create file name "heroku.yml" and copy :
```bash
build:
  docker:
    web: ./Dockerfile
    web: ./client/Dockerfile
```

! Here the goal is to only have one heroku dyno, it's normal if there's two have a same name.

### 2. at root of your project create file name "Procfile" and copy :
```bash
web:    lein run -m my-heroku-app-name.web
worker: lein run -m my-heroku-app-name.worker
```

## Commands
! use [heroku cli](https://devcenter.heroku.com/articles/heroku-cli) is required
- Login to Heroku 
```bash
heroku login
```

- Login to Heroku container
```bash
heroku container:login
```

- Create heroku app at root of your project 
```bash
heroku create my-app
```

- Set the stack of your app to container
```bash
heroku stack:set container
```

- If you need to add heroku worker in your project
```bash
heroku ps:scale web+worker-number
```

- Commit your project
```bash
git commit -m "message"
```

- push your project on heroku
```bash
git push heroku master
```

- deploy docker containers on heroku
```bash
heroku container:push web 
```

- release docker containers on heroku
```bash
heroku container:release web
```

### Docs 
- [Heroku docker doc](https://devcenter.heroku.com/articles/build-docker-images-heroku-yml)
