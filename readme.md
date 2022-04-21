# Panduan singkat mengatasi CORS di berbagai macam aplikasi

Mayoritas CORS terjadi di browser dan disebabkan oleh server yang kamu gunakan di website tersebut. Untuk mengatasi nya, silahkan ikuti panduan singkat berikut. Pilih teknologi apa yang kamu gunakan di server/backend/api kamu.

## Node.js

- Saat development, ganti `http://domain-website-kamu.com` dengan `http://localhost:[PORT_WEBSITE_KAMU]`.
- Saat production, ganti `http://domain-website-kamu.com` dengan domain dari website kamu.

### HTTP

```js
const http = require("http")
const port = 8080

http
  .createServer((req, res) => {
    const headers = {
      // Ganti domain ini
      "Access-Control-Allow-Origin": "http://domain-website-kamu.com",
      "Access-Control-Allow-Methods": "OPTIONS, POST, GET",
      "Access-Control-Max-Age": 2592000,
    }

    if (req.method === "OPTIONS") {
      res.writeHead(204, headers)
      res.end()
      return
    }

    // letakkan router kamu disini

    res.writeHead(405, headers)
    res.end(`${req.method} is not allowed for the request.`)
  })
  .listen(port)
```

### Express

https://expressjs.com/en/resources/middleware/cors.html

```js
var express = require("express")
var cors = require("cors")
var app = express()

var corsOptions = {
  // Ganti domain ini
  origin: "http://domain-website-kamu.com",
}

app.use(cors(corsOptions))

app.listen(80)
```

### Fastify

https://github.com/fastify/fastify-cors

```js
const fastify = require("fastify")()

fastify.register(require("fastify-cors"), {
  // Ganti domain ini
  origin: "http://domain-website-kamu.com",
})
```
