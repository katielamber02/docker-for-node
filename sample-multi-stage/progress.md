1. docker build -t multistage --target prod .

```
   Successfully built 39b8efa7c990
   Successfully tagged multistage:latest
```

2. docker build -t multistage --target prod . && docker run multistage

3. docker build -t multistage:dev --target dev . && docker run --init -p 3000:3000 multistage:dev

```
Successfully built ce95667ff9c9
Successfully tagged multistage:dev
[nodemon] 1.18.9
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `node ./bin/www --inspect=0.0.0.0:9229`
```

4. docker build -t multistage:test --target test . && docker run --init multistage:test

```
Successfully built 3968dee371a2
Successfully tagged multistage:test

> sample-multi-stage@0.0.0 test /app
> mocha --timeout 10000 --exit



  smoke test
    ✓ checks equality


  1 passing (10ms)
```
