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
