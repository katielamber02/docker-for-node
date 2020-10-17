# MERN Stack

### Frontend

In `frontend-docker`.

1. npm install && npm start

2. Build and check url connecting string in App.js:

- when using proxy url: 'http://localhost/api'
- local dev url: 'http://localhost:4000/api'

- npm run build

### Backend

`backend-docker`.

1. npm install
   - rename .env.example into .env
   - change db url
   - npm start
   - npm build

## `docker-compose.yml`

1. To build images:

```
docker-compose build
```

2. Run the project

```
docker-compose up -d
docker-compose logs
docker-compose down
```

**Pay attention!**

Check healthchecks when running
