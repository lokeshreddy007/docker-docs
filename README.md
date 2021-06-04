# docker-docs

## What is Docker
 A platform for building, running and shipping applications 

Sometimes application works in your local machine but it is not working in server or other system, below list might me the cause

1. One or more file missing
2. Software version mismatch
3. Different configuration setting

this were docker comes it into picture, with docker we can easily package out application everthing it need and run it anywhereon machine which has docker 

## What is Container

An isolated environment for running an application
Are lighweight
Use Os of the host
Start quickly
Nedd less hardware Resoures

## Commands

## TODO Make it has a table
docker pull ubuntu  -- to pull the image from the docker hub
docker images ls  -- to show list of images
docker run ubuntu  -- to run the appllication
docker ps  - to see the list rununing process
docker ps -a  -- to see all the stoped container as well
docker run -it ubuntu  -- to run container in interactive mode
docker start -i docker-id  -- to start container

docker build -t react-app . 
docker build -t react-app sh alpine has only shell not bash by default

docker image prune
docker container prune
docker image rm name 
dokcer image rm id
docker image tag react-app:lastest react-app:1
docker image save -o react-app.tar react-app
docker image load -i react-app.tar
docker run -d react-app 
docker run -d --name bluestar react-app
docker logs id
docker logs --help
docker run -d -p 3000:3000 --name bluestar react-app
docker exec  -- we can execute command on a running container
docker exec bluestar ls 
docker exec -it bluestar sh 
docker stop bluestar
docker remove bluestar
docker ps -a | grep bluestar
docker volume creeate app-data
docker run -d -p 3000:3000 -v app-data:/app/data
docker rm -f id
docker cp id :/app/log.txt  . 
docker cp secret.txt id:/app

docker run -d -p 3000:3000 -v $(pwd):app -- sharing source code with container
docker container rm -f $(docker container ls -aq) --Remove all container

docker image rm -f $(docker image ls -q) --  Remove all images

docker-compose build
docker-compose build --no-cache
docker-compose up
docker-compose up -d
docker-compose down
docker network ls 
docker-compose logs
### Docker File 

```Dockerfile
FROM node:14.16.0-alpine3.13

RUN addgroup app && adduser -S -G app app
USER app

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . . 

EXPOSE 3001 

CMD ["npm", "start"]
```
#### Keywords in Docker file

1. FROM
2. WORKDIR
3. COPY
4. ADD
5. RUN
6. ENV
7. EXPOSE
8. USER
9. CMD
10. ENTRYPOINT

### Docker Compose Yml File demo

``` yml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/app
  web-tests:
    image: demo_web
    volumes:
      - ./frontend:/app
    command: npm test
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/demo
    volumes:
      - ./backend:/app
    command: ./docker-entrypoint.sh
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - demo:/data/db

volumes:
  demo:
```
