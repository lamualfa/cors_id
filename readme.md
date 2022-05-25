# Panduan singkat mengatasi CORS di berbagai macam aplikasi

![CORS](https://user-images.githubusercontent.com/39755201/164412362-7117f181-df69-409d-95b7-eb46fe5e54d9.png)

Mayoritas [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) terjadi di browser dan disebabkan oleh _server/backend/api_ yang kamu gunakan di website tersebut. Untuk mengatasi nya, silahkan ikuti panduan singkat berikut.

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## FAQ

- Saya menggunakan _server/backend/api_ eksternal? **[Gunakan cara cepat](#cara-cepat)**.
- Saya tidak tahu teknologi apa yang _server/backend/api_ eksternal saya gunakan? **[Gunakan cara cepat](#cara-cepat)**.
- Teknologi yang saya gunakan belum ada? **[Buat issue](https://github.com/lamualfa/cors_id/issues/new)** atau **[buat pull request](https://github.com/lamualfa/cors_id/pulls)** jika berkenan.

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## Cara membaca error CORS

![image](https://user-images.githubusercontent.com/39755201/170202328-dc47a4b7-394f-43c5-89a6-a442ddc270e0.png)

Pada contoh diatas berarti server yang bermasalah adalah server dengan domain `http://localhost:5000`. Server dengan domain tersebut yang harus diperbaiki menggunakan solusi-solusi yang saya sediakan dibawah.

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## Informasi

Domain `http://domain-website-kamu.com` adalah domain dari server frontend yang kamu gunakan. Bukan domain dari _server/backend/api_ yang bermasalah tadi.

###### Development

Ganti `http://domain-website-kamu.com` dengan `http://localhost:[PORT_WEBSITE_KAMU]`.

###### Production

Ganti `http://domain-website-kamu.com` dengan domain dari website kamu.

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## Pintasan

###### Pilih berdasarkan teknologi yang _server/backend/api_ kamu gunakan.

- [Node.js](https://github.com/lamualfa/cors_id#nodejs)
  - [HTTP](https://github.com/lamualfa/cors_id#http)
  - [Express](https://github.com/lamualfa/cors_id#express)
  - [Fastify](https://github.com/lamualfa/cors_id#fastify)
  - [Next.js](https://github.com/lamualfa/cors_id#nextjs-api)
- [PHP](https://github.com/lamualfa/cors_id#php)
  - [Laravel](https://github.com/lamualfa/cors_id#laravel)
- [Cara cepat](https://github.com/lamualfa/cors_id#cara-cepat)
  - [Ekstensi browser](https://github.com/lamualfa/cors_id#ekstensi-browser)

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## Node.js

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

### Next.js API

https://github.com/yonycalsin/nextjs-cors

```js
import NextCors from "nextjs-cors"

async function handler(req, res) {
  await NextCors(req, res, {
    // Ganti domain ini
    origin: "http://domain-website-kamu.com",
    methods: ["GET", "HEAD", "PUT", "PATCH", "POST", "DELETE"],
  })

  //  letakkan kode anda disini
}
```

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## PHP

### Laravel

#### Versi 8, 7, 6 dan kebawah

https://github.com/fruitcake/laravel-cors

##### Install `laravel-cors`

```bash
composer require fruitcake/laravel-cors
```

##### Tambahkan middleware

Buka file `app/Http/Kernel.php`, dan tambahkan middleware `\Fruitcake\Cors\HandleCors::class` di bagian paling atas `$middleware`.

###### contoh

```php
protected $middleware = [
  \Fruitcake\Cors\HandleCors::class,
    // ...
];
```

##### Buat file konfigurasi

```bash
php artisan vendor:publish --tag="cors"
```

##### Konfigurasi CORS

Buka file `config/cors.php` dan tambahkan konfigurasi berikut:

```php
'allowed_origins' => ['http://domain-website-kamu.com']
```

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)


## Go

### net/http

#### Tanpa third-party package

```go
func CORS(next http.HandlerFunc) http.HandlerFunc {
  return func(w http.ResponseWriter, r *http.Request) {
    w.Header().Add("Access-Control-Allow-Origin", "http://domain-website-kamu.com")
    w.Header().Add("Access-Control-Allow-Credentials", "true")
    w.Header().Add("Access-Control-Allow-Methods", "POST, GET, OPTIONS, PUT, DELETE")

    if r.Method == "OPTIONS" {
        http.Error(w, "No Content", http.StatusNoContent)
        return
    }

    next(w, r)
  }
}

func main() {
  router := http.NewServeMux()

  // Pasang routing kamu disini

  http.ListenAndServe(":8080", CORS(router))
}
```

#### Dengan third-party package

Menggunakan [rs/cors](https://github.com/rs/cors)

```go
package main

import (
    "net/http"

    "github.com/rs/cors"
)

func main() {
    router := http.NewServeMux()
    // Pasang routing kamu disini

    corsMiddleware := cors.New(cors.Options{
        AllowedOrigins: []string{"http://domain-website-kamu.com"},
        AllowCredentials: true,
        AllowedMethods: []string{"GET", "POST", "OPTIONS", "PUT", "DELETE"},
        // Enable Debugging for testing, consider disabling in production
        Debug: true,
    })

    http.ListenAndServe(":8080", corsMiddleware(router))
}
```

### Gin

Dengan [gin-contrib/cors](https://github.com/gin-contrib/cors)

```go
package main

import (
  "time"

  "github.com/gin-contrib/cors"
  "github.com/gin-gonic/gin"
)

func main() {
  router := gin.Default()

  router.Use(cors.New(cors.Config{
    AllowOrigins:     []string{"http://domain-website-kamu.com"},
    AllowMethods:     []string{"GET", "POST", "OPTIONS", "PUT", "DELETE"},
    AllowCredentials: true,
    MaxAge: 12 * time.Hour,
  }))

  // Routing kamu disini

  router.Run()
}
```

### Chi

Dengan [go-chi/cors](https://github.com/go-chi/cors)

```go
package main

import (
  "net/http"

  "github.com/go-chi/chi/v5"
  "github.com/go-chi/cors"
)

func main() {
  r := chi.NewRouter()
  
  r.Use(cors.Handler(cors.Options{
    AllowedOrigins:   []string{"http://domain-website-kamu.com"},
    AllowedMethods:   []string{"GET", "POST", "OPTIONS", "PUT", "DELETE"},
    AllowCredentials: true,
    MaxAge:           2592000, 
  }))

  // Routing kamu disini

  http.ListenAndServe(":8080", r)
}
```

## Cara cepat

###### Production

Jangan gunakan cara ini di mode production.

### Ekstensi browser

Install ekstensi berikut untuk menyelesaikan masalah CORS dengan instan.

##### Chrome atau Brave

https://chrome.google.com/webstore/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf

##### Firefox

https://addons.mozilla.org/id/firefox/addon/cors-everywhere/

##### Microsoft Edge

https://microsoftedge.microsoft.com/addons/detail/allow-cors-accesscontro/bhjepjpgngghppolkjdhckmnfphffdag

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

#### Brought to you by [Teknologi Umum](https://github.com/teknologi-umum)
