1. docker build -t assignment1 .

```
 ---> 272b306bfc09
Successfully built 272b306bfc09
Successfully tagged assignment1:latest
```

2. docker run -p 80:3000 assignment1

```
[1602581664234] INFO  (6 on d3cb98c45ca5): server started
    created: 1602581664179
    started: 1602581664225
    host: "0.0.0.0"
    port: 3000
    protocol: "http"
    id: "d3cb98c45ca5:6:kg7rqu2b"
    uri: "http://0.0.0.0:3000"
    address: "0.0.0.0"
Server running at: http://0.0.0.0:3000

[1602581695218] INFO  (6 on d3cb98c45ca5): In handler /
    req: {
      "id": "1602581695205:d3cb98c45ca5:6:kg7rqu2b:10000",
      "method": "get",
      "url": "http://localhost/",
      "headers": {
        "host": "localhost",
        "connection": "keep-alive",
        "upgrade-insecure-requests": "1",
        "user-agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36",
```

3. docker run -d -p 80:3000 assignment1

```
0885fdcb747ef91735fc24f9f5febc163b2c8d35fef503f6ff218dd888f09899

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
0885fdcb747e        assignment1         "/sbin/tini -- node …"   5 seconds ago       Up 4 seconds        0.0.0.0:80->3000/tcp   funny_hodgkin

docker stop 08
08
```

4. docker build -t assignment1:without_tini .

```
Successfully built 00aec114324a
Successfully tagged assignment1:without_tini

docker run -d -p 80:3000 assignment1:without_tini
c49524da5e4ee6035d217de38a1b3665dc7ca99ed7fd1ab326b64d156c49718e

ctrl-c doesn't work
docker ps
CONTAINER ID        IMAGE                      COMMAND             CREATED              STATUS              PORTS                  NAMES
c49524da5e4e        assignment1:without_tini   "node app.js"       About a minute ago   Up About a minute   0.0.0.0:80->3000/tcp   gracious_blackburn

docker stop c4
c4
stops after 10 sec

docker top d3ad90ad3161a7941c1e8a49cd0982d98a92060aa069af7acdfb5e8d2f396cab
PID                 USER                TIME                COMMAND
2598                root                0:00                node app.js


docker run -d -p 8080:3000 assignment1
7623e9a9cadc9c5e6dfd56185535b5de19104f68a1b30993d8401d4586c5f5f4

docker ps
CONTAINER ID        IMAGE                      COMMAND                  CREATED              STATUS              PORTS                    NAMES
7623e9a9cadc        assignment1                "/sbin/tini -- node …"   5 seconds ago        Up 5 seconds        0.0.0.0:8080->3000/tcp   wizardly_hypatia
d3ad90ad3161        assignment1:without_tini   "node app.js"            About a minute ago   Up About a minute   0.0.0.0:80->3000/tcp     suspicious_knuth


docker top assignment1
PID                 USER                TIME                COMMAND
2704                root                0:00                /sbin/tini -- node app.js
2740                root                0:00                node app.js

docker top  assignment1:without_tini
PID                 USER                TIME                COMMAND
2598                root                0:00                node app.js


docker stop 7623e9a9cadc d3ad90ad3161
7623e9a9cadc immediately stops
d3ad90ad3161 after 10 sec
```

5.  docker run -d --init -p 80:3000 assignment1
    c92689f422eb3061238cb2a661057a161386a0990533c47ec18a535786b5ed4b

```
docker run -d --init -p 80:3000 00aec114324a
dbd5cb22115241ca09d15da01900837827ca369756e8b62dded8afade61c64c4

 docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
dbd5cb221152        00aec114324a        "node app.js"       4 seconds ago       Up 2 seconds        0.0.0.0:80->3000/tcp   zealous_volhard

 docker top dbd5cb221152
PID                 USER                TIME                COMMAND
3011                root                0:00                /sbin/docker-init -- node app.js
3047                root                0:00                node app.js
```
