1. docker build -t ultimatenode:dev --target dev .
2. docker build -t ultimatenode:test --target test .
3. docker build -t ultimatenode:prod --target prod .
4. docker image ls

```

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              ad408015c610        10 seconds ago      277MB
ultimatenode        prod                720e190e86b1        10 seconds ago      153MB
ultimatenode        test                5b02a32fde0d        16 seconds ago      277MB
ultimatenode        dev                 e1d3abb914f9        3 minutes ago       277MB

```

5. docker run ultimatenode:test

```
[nodemon] 2.0.5
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node server.js`
Mon, 19 Oct 2020 14:55:59 GMT body-parser deprecated bodyParser: use individual json/urlencoded middlewares at server.js:75:9
Mon, 19 Oct 2020 14:55:59 GMT body-parser deprecated undefined extended: provide extended option at node_modules/body-parser/index.js:105:29
App running on port 8080
Waiting for db
Waiting for db
CTRL-C
```

6. docker run ultimatenode:prod

```
Mon, 19 Oct 2020 14:56:58 GMT body-parser deprecated bodyParser: use individual json/urlencoded middlewares at server.js:75:9
Mon, 19 Oct 2020 14:56:58 GMT body-parser deprecated undefined extended: provide extended option at node_modules/body-parser/index.js:105:29
App running on port 8080
Waiting for db
```

7. docker-compose up

```
result_1  | App running on port 8080
result_1  | Connected to db
vote_1    | [2020-10-19 15:02:43 +0000] [11] [INFO] Received vote for a
worker_1  | Processing vote for 'a' by '35e8a02d0241c22b'
```
