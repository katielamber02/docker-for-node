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

3. USER before EXPOSE:

```
USER node
EXPOSE 3000
```

```
docker build -t usernode .

 ---> Running in 51372f5262b6
npm WARN checkPermissions Missing write access to /node
npm WARN user-node@1.0.0 No description
npm WARN user-node@1.0.0 No repository field.

npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /node
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, access '/node'
npm ERR!  { [Error: EACCES: permission denied, access '/node']
npm ERR!   stack: 'Error: EACCES: permission denied, access \'/node\'',
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/node' }
npm ERR!
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR!
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/node/.npm/_logs/2020-10-12T10_43_45_815Z-debug.log
The command '/bin/sh -c npm install && npm cache clean --force' returned a non-zero code: 243
```

WORKDIR /node creates directory as root
WORKDIR /node/app
so npm install will not work

4. Manually change permission:

```
FROM node:10-slim
EXPOSE 3000
WORKDIR /node
COPY package*.json ./
RUN mkdir app && chown -R node:node .
USER node
RUN npm install && npm cache clean --force
WORKDIR /node/app
COPY . .
CMD ["node", "app.js"]
```
