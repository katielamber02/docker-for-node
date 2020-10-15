1. docker build -t testnode --target=test .

```
Step 21/23 : RUN eslint .
 ---> Running in 2a48e8c68693
Removing intermediate container 2a48e8c68693
 ---> 543fc9c4452f
Step 22/23 : RUN npm test
 ---> Running in 0102ea51c146

> multi-stage-test@0.0.0 test /opt/app
> mocha --timeout 10000 --exit



  smoke test
    ✓ checks equality


  1 passing (19ms)

Removing intermediate container 0102ea51c146
 ---> ba2935825530
Step 23/23 : CMD ["npm", "run", "int-test"]
 ---> Running in 14af78f21a0f
Removing intermediate container 14af78f21a0f
 ---> c18c84121a2b
Successfully built c18c84121a2b
Successfully tagged testnode:latest
```

2. to rebuild: docker build -t testnode --target=test . --no-cache
