Containers|master ⇒ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
dca0416d5ef9   bridge    bridge    local
43176bf189d6   host      host      local
511c0bb0f25c   none      null      local

- Dont use the default bridge network
- Create new bridge to enable two microservices containers talk to each other
- Docker networking from scratch
- docker run -d --network=app-net -p 27017:27017 --name=db --rm mongo:3
- docker run -it --network=app-net --rm mongo:3 mongo --host db
- mkdir networking
- cd networking
- npm init -y
- npm i mongodb @hapi/hapi hapi-pino
- npm i mongodb@3.3 @hapi/hapi hapi-pino

- docker build --tag=my-app-with-mongo .
- docker run -p 3000:3000 --network=app-net --env MONGO_CONNECTION_STRING=mongodb://db:27017 my-app-with-mongo
