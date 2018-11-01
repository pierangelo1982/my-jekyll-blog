---
layout: post
title: "nodejs vs golang server web"
date: 2018-11-01 10:58:38 +0200
categories: nodejs
---

After the comparison [between NodeJS and GO about how to use MongoDB](http://www.web-dev.info/docker/2018/10/25/golang-with-mongodb.html), now will see how programming a very simple and basic web server with this two technologies.

### Golang:
file main.go
```
package main

import "net/http"

func homePage(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Welcome to my Home Page"))
}
func main() {
	http.HandleFunc("/", homePage)
	if err := http.ListenAndServe(":8080", nil); err != nil {
		panic(err)
	}
}
```

Run server:
```
go run main.go

```
Should be run on:
http://127.0.0.1:8080

### NodeJS
file app.js
```
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Welcome to my Home Page');
});

server.listen(port, hostname, () =>  {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```

run server:
```
node app.js
```

Should be run on:
http://127.0.0.1:3000

So as you see, with both you can get a running web server with few lines of code.

Have a Nice Day!

### Respository:
* NodeJS [https://github.com/pierangelo1982/nodejs-experiment/blob/master/01%20-%20first%20app/app.js](https://github.com/pierangelo1982/nodejs-experiment/blob/master/01%20-%20first%20app/app.js)
* Golang [https://github.com/pierangelo1982/go-experiment/tree/master/server-web/02](https://github.com/pierangelo1982/go-experiment/tree/master/server-web/02)

### Video Tutorial:
<iframe width="560" height="315" src="https://www.youtube.com/embed/oZUTGOSTQVE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
