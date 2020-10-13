1. docker build -t mta .

```
 ---> bf6710e5bbb5
Successfully built bf6710e5bbb5
Successfully tagged mta:latest
```

2. docker run mta

```
info: about to start
info: about to scan ./in
error: undefined {"errno":-2,"code":"ENOENT","syscall":"scandir","path":"./in"}
```

Volumes should be determined:

3. Volumes: -v or --mount
   docker run -v $(pwd)/in:/app/in -v $(pwd)/out:/app/out mta

```
info: about to start
info: about to scan ./in
info: found file ./in/picard5.gif
info: working on it, I'm old and slow
info: found file ./in/picard7.gif
info: working on it, I'm old and slow
info: image picard5.gif resized
info: image picard7.gif resized
```

As a result two pictures appeared in out folder yet they are ignored

4. docker run -v $(pwd)/in:/app/in -v $(pwd)/out:/app/out --env CHARCOAL_FACTOR=10 mta

5. docker ps -l

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                          PORTS               NAMES
61d50ee91c10        mta                 "docker-entrypoint.s…"   About a minute ago   Exited (0) About a minute ago                       hungry_elion
```

7. docker logs 61d

```
info: about to start
info: about to scan ./in
info: found file ./in/picard5.gif
info: working on it, I'm old and slow
info: found file ./in/picard7.gif
info: working on it, I'm old and slow
info: image picard5.gif resized
info: image picard7.gif resized
```
