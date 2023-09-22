# Integrasi MongoDB dan Express

## Langkah Percobaan
### Percobaan instalasi NodeJS
1. buka halaman https://nodejs.org/en/
2. Download dan jalankan node setup
3. Setelah instalasi selesai jalankan command `node -v` untuk memeriksa apakah NodeJS sudah terinstall
### Inisiasi project Express dan pemasangan package
1. Lakukan pembuatan folder dengan nama express-mongodb dan masuk ke dalam folder tersebut lalu buka melalui text editor masing-masing
2. Lakukan npm init untuk mengenerate file package.json dengan menggunakan command `npm init -y` 
3. Lakukan instalasi express, mongoose, dan dotenv dengan menggunakan command `npm i express mongoose dotenv`
### Koneksi Express ke MongoDB
1. Buatlah file index.js pada root folder dan masukkan kode di bawah ini

```require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const app = express();
app.use(express.json());
app.get('/', (req, res) => {
res.status(200).json({
message: '<nama>,<nim>'
})
})
const PORT = 8000;
app.listen(PORT, () => {
console.log(`Running on port ${PORT}`);
})
```

Setelah itu coba jalankan aplikasi dengan command `node index.js`

```⚠ Dikarenakan tidak menggunakan nodemon, maka setiap kali menyimpan perubahan file diharuskan untuk restart server node terlebih dahulu dengan menekan ctrl+c dan jalankan command node index.js lagi```

2. Lakukan pembuatan file *.env* dan masukkan baris berikut
```PORT=5000```
