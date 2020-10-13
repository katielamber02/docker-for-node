FROM node:10.15-alpine

EXPOSE 3000

# https://github.com/krallin/tini -> Alpine Linux Package
RUN apk add --no-cache tini

WORKDIR /myapp

COPY package.json package-lock.json* ./

RUN npm install && npm cache clean --force

COPY . .

# https://github.com/krallin/tini -> Alpine Linux Package
# ENTRYPOINT ["/sbin/tini", "--"]

CMD ["node", "app.js"]