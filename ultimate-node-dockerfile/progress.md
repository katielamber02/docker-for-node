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

8. run -it ultimatenode:prod bash

```
node@7b937489a80f:/app$ ls
README.md  node_modules  package-lock.json  package-original.json  package.json  progress.md  server.js  views
```

9. no nodemon in prod version:

```
node@7b937489a80f:/app$ ls node_modules
accepts             callsite             engine.io         inherits           packet-reader         postgres-date      socket.io-parser
after               component-bind       engine.io-client  ipaddr.js          parseqs               postgres-interval  split
array-flatten       component-emitter    engine.io-parser  isarray            parseuri              proxy-addr         statuses
arraybuffer.slice   component-inherit    escape-html       media-typer        parseurl              qs                 through
async               content-disposition  etag              merge-descriptors  path-to-regexp        range-parser       to-array
async-limiter       content-type         express           method-override    pg                    raw-body           toidentifier
backo2              cookie               finalhandler      methods            pg-connection-string  safe-buffer        type-is
base64-arraybuffer  cookie-parser        forwarded         mime               pg-int8               safer-buffer       unpipe
base64id            cookie-signature     fresh             mime-db            pg-pool               send               utils-merge
better-assert       debug                has-binary2       mime-types         pg-protocol           serve-static       vary
blob                depd                 has-cors          ms                 pg-types              setprototypeof     ws
body-parser         destroy              http-errors       negotiator         pgpass                socket.io          xmlhttprequest-ssl
buffer-writer       ee-first             iconv-lite        object-component   postgres-array        socket.io-adapter  xtend
bytes               encodeurl            indexof           on-finished        postgres-bytea        socket.io-client   yeast
```

10. docker-compose up -d
11. docker-compose exec result ./tests/tests.sh

```
e ------------
-e Tests passed
-e ------------

```

12. docker build --build-arg=MICROSCANNER -t ultimatenode:test --target test .

```
 ---> Running in 3333bd26f780
   ___                 ____          __  ____              ____
  / _ |___ ___ _____ _/ __/__ ____  /  |/  (_)__________  / __/______ ____  ___  ___ ____
 / __ / _ `/ // / _ `/\ \/ -_) __/ / /|_/ / / __/ __/ _ \_\ \/ __/ _ `/ _ \/ _ \/ -_) __/
/_/ |_\_, /\_,_/\_,_/___/\__/\__/ /_/  /_/_/\__/_/  \___/___/\__/\_,_/_//_/_//_/\__/_/
       /_/
Aqua Security MicroScanner, version 3.5.0
Community Edition

.
.
.

Removing intermediate container 3333bd26f780
 ---> a3b64c7103c8
[Warning] One or more build-args [MICROSCANNER] were not consumed
Successfully built a3b64c7103c8
Successfully tagged ultimatenode:test
```

13. DOCKER_BUILDKIT=1 docker build --build-arg=MICROSCANNER -t ultimatenode:test --target test .

```
[+] Building 44.8s (22/22) FINISHED
 .
 .
 .
 => => writing image sha256:c14145824ca000da64e48a1d15cce887284313f4ae6b8d500a80a24ef99a4f7a                                                        0.0s
 => => naming to docker.io/library/ultimatenode:test
```

14. DOCKER_BUILDKIT=1 docker build --build-arg=MICROSCANNER -t ultimatenode:prod --target prod .

```
[+] Building 9.3s (24/24) FINISHED
 => => writing image sha256:07eeaa3eed619d987d5b3967f2f493f5579f5ae5ef807b8375f803b4689be043                                                        0.0s
 => => naming to docker.io/library/ultimatenode:prod
```

15. docker run ultimatenode:prod

```
App running on port 8080
Waiting for db
```

16. # USER root
    DOCKER_BUILDKIT=1 docker build --build-arg=MICROSCANNER -t ultimatenode:prod --target prod .

```
 > [test 4/5] RUN chmod +x /microscanner:
#20 0.213 chmod: changing permissions of '/microscanner': Operation not permitted
```

uncomment user 17. change port from 80 to 8080 in EXPOSE and in server.js:
from var port = process.env.PORT || 80;
to var port = process.env.PORT || 8080;
