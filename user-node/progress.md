1. ls -la

```
total 96
drwxr-xr-x  9 beon  staff    288 Oct 12 13:15 .
drwxr-xr-x  7 beon  staff    224 Oct 12 13:14 ..
-rw-r--r--  1 beon  staff     20 Oct 12 13:14 .dockerignore
-rw-r--r--  1 beon  staff    166 Oct 12 13:14 Dockerfile
-rw-r--r--  1 beon  staff   1125 Oct 12 13:14 app.js
-rw-r--r--  1 beon  staff    155 Oct 12 13:14 docker-compose.yml
-rw-r--r--  1 beon  staff  24579 Oct 12 13:14 package-lock.json
-rw-r--r--  1 beon  staff    313 Oct 12 13:15 package.json
-rw-r--r--  1 beon  staff      0 Oct 12 13:15 progress.md
```

2. All commands (RUN, Entry Point, CMD) after "USER node" will be using node user indtead of the root user. But everything else will still be acting as root

```
COPY . .

USER node

CMD [ "node", "./bin/www" ]
```

```
docker build -t usernode .
Successfully built 473fcc0f86e7
Successfully tagged usernode:latest
```

3. Check permissions:

```
docker run -it usernode bash
root@adb1634c7cac:/node/app# ls -la
total 60
drwxr-xr-x 1 root root  4096 Oct 12 10:30 .
drwxr-xr-x 1 root root  4096 Oct 12 10:30 ..
-rw-r--r-- 1 root root    20 Oct 12 10:14 .dockerignore
-rw-r--r-- 1 root root   166 Oct 12 10:14 Dockerfile
-rw-r--r-- 1 root root  1125 Oct 12 10:14 app.js
-rw-r--r-- 1 root root   155 Oct 12 10:14 docker-compose.yml
-rw-r--r-- 1 root root 24579 Oct 12 10:14 package-lock.json
-rw-r--r-- 1 root root   313 Oct 12 10:15 package.json
-rw-r--r-- 1 root root   699 Oct 12 10:28 progress.md


root@adb1634c7cac:/node/app# cd ..

root@adb1634c7cac:/node# ls -la
total 48
drwxr-xr-x  1 root root  4096 Oct 12 10:30 .
drwxr-xr-x  1 root root  4096 Oct 12 10:32 ..
drwxr-xr-x  1 root root  4096 Oct 12 10:30 app
drwxr-xr-x 41 root root  4096 Oct 12 10:30 node_modules
-rw-r--r--  1 root root 24579 Oct 12 10:14 package-lock.json
-rw-r--r--  1 root root   313 Oct 12 10:15 package.json


```
