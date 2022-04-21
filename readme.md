# Panduan singkat mengatasi CORS di berbagai macam aplikasi

Mayoritas CORS terjadi di browser dan disebabkan oleh _server/backend/api_ yang kamu gunakan di website tersebut. Untuk mengatasi nya, silahkan ikuti panduan singkat berikut. Pilih berdasarkan teknologi _server/backend/api_ gunakan.

### FAQ

- Saya menggunakan _server/backend/api_ eksternal? **[Gunakan cara cepat](#cara-cepat)**.
- Saya tidak tahu teknologi apa yang _server/backend/api_ eksternal saya gunakan? **[Gunakan cara cepat](#cara-cepat)**.
- Teknologi yang saya gunakan belum ada? **[Buat issue](https://github.com/lamualfa/cors_id/issues/new)** atau **[buat pull request](https://github.com/lamualfa/cors_id/pulls)** jika berkenan.

### Pintasan

- [Node.js](https://github.com/lamualfa/cors_id#nodejs)
  - [HTTP](https://github.com/lamualfa/cors_id#http)
  - [Express](https://github.com/lamualfa/cors_id#express)
  - [Fastify](https://github.com/lamualfa/cors_id#fastify)
- [PHP](https://github.com/lamualfa/cors_id#php)
  - [Laravel](https://github.com/lamualfa/cors_id#laravel)
- [Cara cepat](https://github.com/lamualfa/cors_id#cara-cepat)
  - [Ekstensi browser](https://github.com/lamualfa/cors_id#ekstensi-browser)

![hr](https://user-images.githubusercontent.com/39755201/159233055-3bd55a37-7284-46ad-b759-5ab0c13b3828.png)

## Node.js

###### Development

Ganti `http://domain-website-kamu.com` dengan `http://localhost:[PORT_WEBSITE_KAMU]`.

###### Production

Ganti `http://domain-website-kamu.com` dengan domain dari website kamu.

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
