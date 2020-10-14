1. change docker-compose.yml:

```
version: "2.4"

services:
  traefik:
    image: traefik:1.7-alpine
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "80:80"
    command:
      - --docker
      - --docker.domain=traefik
      - --docker.watch
      - --api
      - --defaultentrypoints=http,https
    labels:
      - traefik.port=80
      - traefik.frontend.rule=Host:traefik.localhost
    networks:
      - frontend
      - backend

  redis:
    image: redis:alpine
    networks:
      - frontend
    healthcheck:
      test: redis-cli ping

  db:
    image: postgres:9.6
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    healthcheck:
      test: pg_isready -U postgres -h 127.0.0.1

  vote:
    image: bretfisher/examplevotingapp_vote
    ports:
      - "5000:80"
    networks:
      - frontend
    depends_on:
      redis:
        condition: service_healthy
    labels:
      - traefik.port=80
      - traefik.frontend.rule=Host:vote.localhost

  result:
    build:
      context: result
    command: nodemon --inspect=0.0.0.0:9229 server.js
    environment:
      - NODE_ENV=development
    ports:
      - "5001:80"
      - "9229:9229"
    volumes:
      - ./result:/app
    networks:
      - backend
    depends_on:
      db:
        condition: service_healthy
    labels:
      - traefik.port=80
      - traefik.frontend.rule=Host:result.localhost

  worker:
    image: bretfisher/examplevotingapp_worker:java
    networks:
      - frontend
      - backend
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy

networks:
  frontend:
  backend:

volumes:
  db-data:
```

2. docker-compose up

```
Creating assignment-sweet-compose_redis_1   ... done
Creating assignment-sweet-compose_traefik_1 ... done
Creating assignment-sweet-compose_db_1      ... done
Creating assignment-sweet-compose_result_1  ... done
Creating assignment-sweet-compose_worker_1  ... done
Creating assignment-sweet-compose_vote_1    ... done
Attaching to assignment-sweet-compose_db_1, assignment-sweet-compose_redis_1, assignment-sweet-compose_traefik_1, assignment-sweet-compose_result_1, assignment-sweet-compose_worker_1, assignment-sweet-compose_vote_1
redis_1    | 1:C 14 Oct 2020 13:33:35.916 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1    | 1:C 14 Oct 2020 13:33:35.916 # Redis version=6.0.8, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1    | 1:C 14 Oct 2020 13:33:35.916 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1    | 1:M 14 Oct 2020 13:33:35.917 * Running mode=standalone, port=6379.
redis_1    | 1:M 14 Oct 2020 13:33:35.918 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1    | 1:M 14 Oct 2020 13:33:35.918 # Server initialized
redis_1    | 1:M 14 Oct 2020 13:33:35.918 * Ready to accept connections
db_1       |
db_1       | PostgreSQL Database directory appears to contain a database; Skipping initialization
db_1       |
db_1       | LOG:  database system was shut down at 2020-10-14 13:03:03 UTC
db_1       | LOG:  MultiXact member wraparound protections are now enabled
db_1       | LOG:  database system is ready to accept connections
db_1       | LOG:  autovacuum launcher started
result_1   | [nodemon] 2.0.5
result_1   | [nodemon] to restart at any time, enter `rs`
result_1   | [nodemon] watching path(s): *.*
result_1   | [nodemon] watching extensions: js,mjs,json
result_1   | [nodemon] starting `node --inspect=0.0.0.0:9229 server.js`
result_1   | Debugger listening on ws://0.0.0.0:9229/8f0aa94d-2773-4e1c-8176-69a87f4c96fd
result_1   | For help see https://nodejs.org/en/docs/inspector
vote_1     | [2020-10-14 13:34:07 +0000] [1] [INFO] Starting gunicorn 19.10.0
vote_1     | [2020-10-14 13:34:07 +0000] [1] [INFO] Listening at: http://0.0.0.0:80 (1)
vote_1     | [2020-10-14 13:34:07 +0000] [1] [INFO] Using worker: sync
vote_1     | [2020-10-14 13:34:07 +0000] [9] [INFO] Booting worker with pid: 9
vote_1     | [2020-10-14 13:34:07 +0000] [10] [INFO] Booting worker with pid: 10
vote_1     | [2020-10-14 13:34:07 +0000] [11] [INFO] Booting worker with pid: 11
vote_1     | [2020-10-14 13:34:07 +0000] [12] [INFO] Booting worker with pid: 12
worker_1   | Connected to redis
result_1   | App running on port 80
result_1   | Connected to db
worker_1   | Connected to db
worker_1   | Watching vote queue
^[vote_1     | 172.24.0.3 - - [14/Oct/2020:13:34:39 +0000] "GET / HTTP/1.1" 200 1285 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"
vote_1     | 172.24.0.3 - - [14/Oct/2020:13:34:39 +0000] "GET /static/stylesheets/style.css HTTP/1.1" 200 0 "http://vote.localhost/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"
vote_1     | 172.24.0.3 - - [14/Oct/2020:13:34:40 +0000] "GET /favicon.ico HTTP/1.1" 404 232 "http://vote.localhost/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"
vote_1     | [2020-10-14 13:34:43,090] INFO in app: Received vote for a
vote_1     | [2020-10-14 13:34:43 +0000] [9] [INFO] Received vote for a
worker_1   | Processing vote for 'a' by 'ea8a4711075334a0'
vote_1     | 172.24.0.3 - - [14/Oct/2020:13:34:43 +0000] "POST / HTTP/1.1" 200 1688 "http://vote.localhost/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"

result_1   | [nodemon] restarting due to changes...
result_1   | [nodemon] starting `node --inspect=0.0.0.0:9229 server.js`
```

3. Make sure you have no Watching for db as a healthcheck sign
4. open http://vote.localhost/ and http://result.localhost/

```
vote_1     | 172.24.0.3 - - [14/Oct/2020:13:47:26 +0000] "GET / HTTP/1.1" 200 1285 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"
vote_1     | 172.24.0.3 - - [14/Oct/2020:13:47:40 +0000] "GET / HTTP/1.1" 200 1285 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36"
```

5. open chrome://inspect
