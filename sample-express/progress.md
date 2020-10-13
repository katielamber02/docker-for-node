1. cat docker-compose.yml

```
version: '2.4'

services:
  express:
    build: .
    ports:
      - 3000:3000
    volumes:
      - .:/app
    environment:
      - DEBUG=sample-express:*
```

2. host tries to bind into the container app  
   volumes: - .:/app

```
docker-compose up

Attaching to sample-express_express_1
express_1  | internal/modules/cjs/loader.js:584
express_1  |     throw err;
express_1  |     ^
express_1  |
express_1  | Error: Cannot find module 'http-errors'
sample-express_express_1 exited with code 1

```

3. The host doesn't have node_modules:

```
ls
Dockerfile		bin			package-lock.json	progress.md		routes
app.js			docker-compose.yml	package.json		public			views
```

4. docker-compose run express npm install
   It will be imstalled on the host:

```
ls
Dockerfile		docker-compose.yml	package.json		routes
app.js			node_modules		progress.md		views
bin			package-lock.json	public
```

5. docker-compose up

```
Starting sample-express_express_1 ... done
Attaching to sample-express_express_1
express_1  | Tue, 13 Oct 2020 14:41:24 GMT sample-express:server Listening on port 3000
```

5. cat docker-compose.yml

````
    volumes:
      - .:/node/app
      - /node/app/node_modules

      ```
````

6. cat Dockerfile

```
FROM node:10.15-slim

ENV NODE_ENV=production

WORKDIR /node

COPY package.json package-lock*.json ./

RUN npm install && npm cache clean --force

WORKDIR /node/app

COPY . .

CMD ["node", "./bin/www"]
```

7. npm install

```
ls
Dockerfile		docker-compose.yml	package.json		routes
app.js			node_modules		progress.md		views
```

8. docker-compose build
   docker-compose up -d

```
Recreating sample-express_express_1 ... done

docker-compose ps
          Name                Command       State           Ports
--------------------------------------------------------------------------
sample-express_express_1   node ./bin/www   Up      0.0.0.0:3000->3000/tcp
```

9. docker-compose exec express bash
   root@8ab4eb97438f:/node/app# ls

   ```
   ls
   Dockerfile  app.js  bin  docker-compose.yml  node_modules  package-lock.json  package.json  progress.md  public  routes  views

   ```

10. root@8ab4eb97438f:/node/app# ls node_modules
