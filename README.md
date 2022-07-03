# deployement-dockerize-app-to-heroku
How to deploy dockerize app to heroku ?

## Example project structure

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

## Config files
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

### Docs 
- [Heroku docker doc](https://devcenter.heroku.com/articles/build-docker-images-heroku-yml)
