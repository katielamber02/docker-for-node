1. Two ways to run various tools inside the conatainer:

- docker-compose run: starts a new container and run command/shell
- docker-compose exec: run additional command/shell in currently running container (usually after docker-compose up)

2. docker-compose run api npm install

```
Creating network "sample-strapi_default" with the default driver
Creating volume "sample-strapi_mongo" with default driver
Building api

Step 7/10 : RUN npm install && npm cache clean --force
 ---> Running in 246bf4f1be62

> sqlite3@5.0.0 install /app/node_modules/sqlite3
> node-pre-gyp install --fallback-to-build

Successfully built cd21c2a1d845
Successfully tagged sample-strapi_api:latest

added 1367 packages from 2008 contributors and audited 1440 packages in 189.088s
```

3. ls

```
Dockerfile		config			favicon.ico		package.json
README.md		docker-compose.yml	node_modules		progress.md
api			extensions		package-lock.json	public
```

4. docker-compose up

```
api_1    | [2020-10-14T09:17:13.475Z] info ⏳ Opening the admin panel...
```

5. in another tab:
   docker-compose exec api strapi --help
   docker-compose exec bash
   root@90c273009c47:/app#
