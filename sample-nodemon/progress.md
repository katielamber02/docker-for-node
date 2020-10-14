1. docker-compose run express npm install nodemon --save-dev
2. add to docker-compose.yml

```
version: "2.4"

services:
  express:
    build: .
    command: /app/node_modules/.bin/nodemon ./bin/www
    ports:
      - 3000:3000
    volumes:
      - .:/app
    environment:
      - DEBUG=sample-express:*
      - NODE_ENV=development
```

3. add to Dockerfile:

```
FROM node:10.15-slim

ENV NODE_ENV=production

WORKDIR /app

COPY package.json package-lock*.json ./

RUN npm install && npm cache clean --force

ENV PATH app/node_modules/.bin/:$PATH

COPY . .

CMD ["node", "./bin/www"]
```

4. docker-compose build

```
 ---> 77f23d15c367

Successfully built 77f23d15c367
Successfully tagged sample-nodemon_express:latest
```

5. docker-compose up

```
Recreating sample-nodemon_express_1 ... done
Attaching to sample-nodemon_express_1
express_1  | [nodemon] 2.0.5
express_1  | [nodemon] to restart at any time, enter `rs`
express_1  | [nodemon] watching path(s): *.*
express_1  | [nodemon] watching extensions: js,mjs,json
express_1  | [nodemon] starting `node ./bin/www`
express_1  | Wed, 14 Oct 2020 10:15:23 GMT sample-express:server Listening on port 3000
express_1  | GET / 304 3826.776 ms - -
express_1  | GET /stylesheets/style.css 304 15.367 ms - -
```
