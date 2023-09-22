# Integrasi MongoDB dan Express

## Instalasi NodeJS
1. buka halaman https://nodejs.org/en/
2. Download dan jalankan node setup
3. Setelah instalasi selesai jalankan command `node -v` untuk memeriksa apakah NodeJS sudah terinstall <br>
![ss 1](../modul%203/ss%203/1.png)
## Inisiasi project Express dan pemasangan package
1. Lakukan pembuatan folder dengan nama express-mongodb dan masuk ke dalam folder tersebut lalu buka melalui text editor masing-masing
2. Lakukan npm init untuk mengenerate file package.json dengan menggunakan command `npm init -y` <br>
![ss 1.1](../modul%203/ss%203/1.1.png)
3. Lakukan instalasi express, mongoose, dan dotenv dengan menggunakan command `npm i express mongoose dotenv` <br>
![ss 1.2](../modul%203/ss%203/1.2.png)
## Koneksi Express ke MongoDB
1. Buatlah file index.js pada root folder dan masukkan kode di bawah ini
```
require('dotenv').config();
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
![ss 1.3](../modul%203/ss%203/1.3.png) <br>
Setelah itu coba jalankan aplikasi dengan command `node index.js`

```
⚠ Dikarenakan tidak menggunakan nodemon, 
maka setiap kali menyimpan perubahan file 
diharuskan untuk restart server node terlebih dahulu 
dengan menekan ctrl+c dan 
jalankan command node index.js lagi
```

2. Lakukan pembuatan file **.env** dan masukkan baris berikut <br>
```PORT=5000``` <br>
![ss 1.4](../modul%203/ss%203/1.4.png) <br>
Setelah itu ubahlah kode pada listening port menjadi berikut dan coba jalankan aplikasi kembali
```
...
const PORT = process.env.PORT || 8000;
app.listen(PORT, () => {
console.log(`Running on port ${PORT}`);
})
```
![ss 1.5](../modul%203/ss%203/1.5.png)
3. Copy connection string yang terdapat pada compas atau atlas dan paste kan pada **.env** seperti berikut <br>
```
MONGO_URI=<Connection string masing-masing>
```
![ss 1.6](../modul%203/ss%203/1.6.png)
4. Tambahkan baris kode berikut pada file **index.js** <br>
```
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');

mongoose.connect(process.env.MONGO_URI);
const db = mongoose.connection;
db.on('error', (error) => {
console.log(error);
});
db.once('connected', () => {
console.log('Mongo connected');
})
...
```
![ss 1.7](../modul%203/ss%203/1.7.png) <br>
Setelah itu coba jalankan aplikasi kembali <br>

## Pembuatan routing 
1. Lakukan pembuatan direktori routes di tingkat yang sama dengan index.js
![ss 2.1](../modul%203/ss%203/2.1.png)
2. Buatlah file book.route.js di dalamnya
![ss 2.2](../modul%203/ss%203/2.2.png)
3. Tambahkan baris kode berikut untuk fungsi getAllBooks <br>
```
const router = require('express').Router();
router.get('/', function getAllBooks(req, res) {
    res.status(200).json({
        message: 'mendapatkan semua buku'
        })
})
module.exports = router;
```
![ss 2.3](../modul%203/ss%203/2.3.png)
4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook <br>
```
const router = require('express').Router();
...
router.get('/:id', function getOneBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'mendapatkan satu buku',
        id,
    })
})
router.post('/', function createBook(req, res) {
    res.status(200).json({
        message: 'membuat buku baru'
    })
})
router.put('/:id', function updateBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'memperbaharui satu buku',
        id,
    })
})
router.delete('/:id', function deleteBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'menghapus satu buku',
        id,
    })
})
module.exports = router;
```
![ss 2.4](../modul%203/ss%203/2.4.png)
5. Lakukan import book.route.js pada file index.js dan tambahkan baris kode berikut <br>
```
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');

const bookRoutes = require('./routes/book.route'); //
...
app.get('/', (req, res) => {
    res.status(200).json({
        message: '<nama>,<nim>'
    })
})
app.use('/books', bookRoutes); //

const PORT = process.env.PORT || 8000;
app.listen(PORT, () => {
    console.log(`Running on port ${PORT}`);
})
```
![ss 2.5](../modul%203/ss%203/2.5.png)
5. Uji salah satu endpoint dengan Postman
![ss 2.7](../modul%203/ss%203/2.7.png)

## Pembuatan controller
1. Lakukan pembuatan direktori controllers di tingkat yang sama dengan index.js
![ss 3.1](../modul%203/ss%203/3.1.png)
2. Buatlah file book.controller.js di dalamnya
![ss 3.2](../modul%203/ss%203/3.2.png)
3. Salin baris kode dari routes untuk fungsi getAllBooks <br>
```
function getAllBooks(req, res) {
    res.status(200).json({
        message: 'mendapatkan semua buku'
    })
};
module.exports = {
    getAllBooks,
}
```
![ss 3.3](../modul%203/ss%203/3.3.png)
4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook <br>
```
...
function getOneBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'mendapatkan satu buku',
        id,
    })
}
function createBook(req, res) {
    res.status(200).json({
        message: 'membuat buku baru'
    })
}
function updateBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'memperbaharui satu buku',
        id,
    })
}
function deleteBook(req, res) {
    const id = req.params.id;
    res.status(200).json({
        message: 'menghapus satu buku',
        id,
    })
}
module.exports = {
    getAllBooks,
    getOneBook, //
    createBook, //
    updateBook, //
    deleteBook //
}
```
![ss 3.4](../modul%203/ss%203/3.4.png)
5. Lakukan import book.controller.js pada file book.route.js <br>
```
const router = require('express').Router();
const book = require('../controllers/book.controller'); //
...
module.exports = router;
```
![ss 3.5](../modul%203/ss%203/3.5.png)
6. Lakukan perubahan pada fungsi agar dapat memanggil fungsi dari book.controller.js <br>
```
const router = require('express').Router();
const book = require('../controllers/book.controller');

router.get('/', book.getAllBooks);
router.get('/:id', book.getOneBook);
router.post('/', book.createBook);
router.put('/:id', book.updateBook);
router.delete('/:id', book.deleteBook);
module.exports = router;
```
![ss 3.6](../modul%203/ss%203/3.6.png)
7. Lakukan pengujian kembali, pastikan response tetap sama
![ss 3.7](../modul%203/ss%203/3.7.png)
## Pembuatan model
Berikut adalah gambaran bentuk data dari modul sebelumnya <br>

|----   | ---   |
| title | string  |
| author| string |
| year    | number |
| pages| number |
| summary| string |
| publisher| string |
| --- | --- |

1. Lakukan pembuatan direktori models di tingkat yang sama dengan index.js
![ss 4.1](../modul%203/ss%203/4.1.png)
2. Buatlah file book.model.js di dalamnya
![ss 4.2](../modul%203/ss%203/4.2.png)
3. Tambahkan baris kode berikut sesuai dengan tabel di atas <br>
```
const mongoose = require('mongoose');
const bookSchema = new mongoose.Schema({    
    title: {
        type: String 
        }, 
        author: {
        type: String
        },
        year: {
        type: Number
        },
        pages: {
        type: Number
        },
        summary: {
        type: String
        },
        publisher: {
        type: String
    }
})
module.exports = mongoose.model('book', bookSchema);
```
![ss 4.3](../modul%203/ss%203/4.3.png)
## Operasi CRUD
1. Hapus semua data pada collection books
![ss 5.1](../modul%203/ss%203/5.1.png)
![ss 5.2](../modul%203/ss%203/5.2.png)
2. Lakukan import book.model.js pada file book.controller.js <br>
```
const Book = require('../models/book.model');
...
```
![ss 5.3](../modul%203/ss%203/5.3.png)
3. Lakukan perubahan pada fungsi createBook <br>
```
const Book = require('../models/book.model');
...
async function createBook(req, res) {
    const book = new Book({
    title: req.body.title,
    author: req.body.author,
    year: req.body.year,
    pages: req.body.pages,
    summary: req.body.summary,
    publisher: req.body.publisher,
    })
try {
    const savedBook = await book.save();
    res.status(200).json({
        message: 'membuat buku baru',
        book: savedBook,
        })
} catch (error) {
    res.status(500).json({
        message: 'kesalahan pada server',
        error: error.message,
        })
    }       
}
...
```
![ss 5.4](../modul%203/ss%203/5.4.png)
4. Buatlah dua buah buku dengan data di bawah ini dengan Postman <br>
```
{
    "title": "Dilan 1990",
    "author": "Pidi Baiq",
    "year": 2014,
    "pages": 332,
    "summary": "Mirea, anata wa utsukushī",
    "publisher": "Pastel Books"
}
```
![ss 5.5](../modul%203/ss%203/5.5.png)
```
{
    "title": "Dilan 1991",
    "author": "Pidi Baiq",
    "year": 2015,
    "pages": 344,
    "summary": "Watashi ga kare o aishite iru to ittara",
    "publisher": "Pastel Books"
}
```
![ss 5.6](../modul%203/ss%203/5.6.png)
Hasil setelah diubah
![ss 5.7](../modul%203/ss%203/5.7.png)
5. Lakukan perubahan pada fungsi getAllBooks <br>
```
const Book = require('../models/book.model');
async function getAllBooks(req, res) {
    try {
        const books = await Book.find();
        res.status(200).json({
        message: 'mendapatkan semua buku',
        books,
    })
} catch (error) {
    res.status(500).json({
        message: 'kesalahan pada server',
        error: error.message,
        })
    }
}
...
```
![ss 5.8](../modul%203/ss%203/5.8.png)
6. Lakukan perubahan pada fungsi getOneBook <br>
```
const Book = require('../models/book.model');
...
async function getOneBook(req, res) {
    const id = req.params.id;
try {
    const book = await Book.findById(id);
    res.status(200).json({
        message: 'mendapatkan satu buku',
        book,
    })
} catch (error) {
    res.status(500).json({
        message: 'kesalahan pada server',
        error: error.message,
        })
    }
}
...
```
![ss 5.9](../modul%203/ss%203/5.9.png)
7. Tampilkan semua buku dengan Postman
![ss 5.10](../modul%203/ss%203/5.10.png)
8. Tampilkan buku Dilan 1990 dengan Postman
![ss 5.11](../modul%203/ss%203/5.11.png)
9. Lakukan perubahan pada fungsi updateBook <br>
```
const Book = require('../models/book.model');
...
async function updateBook(req, res) {
    const id = req.params.id;
try {
    const book = await Book.findByIdAndUpdate(
        id, req.body, { new: true }
    )
    res.status(200).json({
        message: 'memperbaharui satu buku',
        book,
    })
} catch (error) {
    res.status(500).json({
        message: 'kesalahan pada server',
        error: error.message,
        })
    }
}
...
```
![ss 5.12](../modul%203/ss%203/5.12.png)
10. Ubah judul buku Dilan 1990 menjadi “(NAMA PANGGILAN) 1991” dengan Postman
![ss 5.13](../modul%203/ss%203/5.13.png)
11. Lakukan perubahan pada fungsi deleteBook <br>
```
const Book = require('../models/book.model');
...
async function deleteBook(req, res) {
    const id = req.params.id;
    try {
        const book = await Book.findByIdAndDelete(id);
        res.status(200).json({
            message: 'menghapus satu buku',
            book,
        })
    } catch (error) {
        res.status(500).json({
            message: 'kesalahan pada server',
            error: error.message,
        })
    }
}
...
```
![ss 5.14](../modul%203/ss%203/5.14.png)
12. Hapus buku Dilan 1990 dengan Postman
![ss 5.15](../modul%203/ss%203/5.15.png)