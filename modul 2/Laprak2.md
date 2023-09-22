# CRUD MongoDB Compass dan Shell

## - MongoDB Compass
MongoDB Compass adalah tool berbasis Graphical User Interface (GUI) yang digunakan untuk berinteraksi dengan MongoDB yang terpasang secara on-premise dan MongoDB Atlas yang berbasis cloud. Tool ini dapat melakukan aktivitas dasar seperti CREATE, READ, UPDATE, dan DELETE (CRUD) tanpa berhadapan dengan baris perintah (command line)

### Langkah Percobaan
1. Lakukan koneksi ke MongoDB menggunakan connection string <br>
![ss 1.1](../modul%202/ss%202/1.1.png)
2. Buat database dengan melakukan klik “Create Database” <br>
![ss 1.2](../modul%202/ss%202/1.2.png)
3. Lakukan insert buku pertama dengan melakukan klik “Add Data”, pilih “Insert Document”, isi dengan data yang diinginkan dan klik “Insert” <br>
![ss 1.3](../modul%202/ss%202/1.3.png) <br>
![ss 1.4](../modul%202/ss%202/1.4.png)
4. Lakukan insert buku kedua dengan cara yang sama. <br>
![ss 1.5](../modul%202/ss%202/1.5.png)
5. Lakukan pencarian buku dengan author “Osamu Dazai” dengan mengisi filter yang diinginkan dan klik “Find” <br>
![ss 1.6](../modul%202/ss%202/1.6.png) <br>
![ss 1.7](../modul%202/ss%202/1.7.png)
6. Lakukan perubahan summary pada buku “No Longer Human” menjadi “Buku yang bagus (NAMA,NIM) dengan melakukan klik “Edit Document” (berlambang pensil), mengisi nilai summary yang baru, dan melakukan klik “Update” <br>
![ss 1.8](../modul%202/ss%202/1.8.png) <br>
![ss 1.9](../modul%202/ss%202/1.9.png)
7. Lakukan penghapusan pada buku “I Am a Cat” dengan melakukan klik “Remove Document” (berlambang tong sampah) dan melakukan klik “Delete” <br>
![ss 1.10](../modul%202/ss%202/1.10.png) <br>
![ss 1.11](../modul%202/ss%202/1.11.png)

## - MongoDB Shell
Walaupun dapat melakukan operasi seperti MongoDB Compass, interaksi yang dilakukan MongoDB Shell berbasis Command Line Interface (CLI) sehingga diperlukan baris perintah untuk melakukan aktivitas dasar. MongoDB Shell dapat diakses langsung dari MongoDB Compass atau menggunakan mongosh pada Command Prompt

### Langkah Percobaan
1. Lakukan koneksi ke MongoDB Server dengan menjalankan command mongosh bagi yang menggunakan terminal build in OS sehingga tampilan terminal kalian akan menjadi seperti berikut <br>
![ss 2.1](../modul%202/ss%202/2.1.png)
2. Mencoba melihat list database yang ada di server dengan menjalankan command ```show dbs``` <br>
![ss 2.2](../modul%202/ss%202/2.2.png)
Untuk berpindah ke database ```bookstore``` gunakan command use bookstore , kalian dapat memastikan telah berpindah ke database yang benar dengan melihat tulisan sebelum tanda ```“>”``` <br>
![ss 2.2](../modul%202/ss%202/2.2.png)
Cobalah untuk melihat collection yang ada pada database tersebut dengan menggunakan command  ```show collections``` <br>
![ss 2.4](../modul%202/ss%202/2.4.png)
3. Lakukan insert buku “Overlord I” dengan menggunakan command ```db.books.insertOne(data kalian)``` , setelah insert buku berhasil maka MongoDB akan mengembalikan pesan sebagai berikut. <br>
![ss 2.5](../modul%202/ss%202/2.5.png)
4. Lakukan insert buku “The Setting Sun” dan “Hujan” dengan insert many dengan menggunakan command ```db.books.insertMany(data kalian)``` , dan akan mengembalikan pesan sebagai berikut. <br>
![ss 2.6](../modul%202/ss%202/2.6.png)
5. Lakukan pencarian buku dengan menggunakan command ```db.books.find()``` untuk melakukan pencarian semua buku.<br>
![ss 2.7](../modul%202/ss%202/2.7.png)
6. Tampilkan seluruh buku dengan author “Osamu Dazai” dengan mengisi argument pada ```find()``` dengan menggunakan command ```db.books.find({filter yang ingin diisi})``` <br>
![ss 2.8](../modul%202/ss%202/2.8.png)
7. Lakukan perubahan summary pada buku “Hujan” menjadi “Buku yang bagus (NAMA,NIM) dengan mengunakan command ```db.books.updateOne({filter}, {$set: {data yang akan di update}})``` sehingga output yang dihasilkan oleh MongoDB akan menjadi seperti berikut <br>
![ss 2.9](../modul%202/ss%202/2.9.png)
8. Lakukan perubahan publisher menjadi “Yen Press” pada semua buku “Osamu Dazai” dengan menggunakan command ```db.books.updateMany({filter}, {$set: {data yang akan di update}})``` <br>
![ss 2.10](../modul%202/ss%202/2.10.png)
9. Lakukan penghapusan pada buku “Overlord I” dengan menggunakan command ```db.books.deleteOne({argument})``` <br>
![ss 2.11](../modul%202/ss%202/2.11.png)
10. Lakukan penghapusan pada semua buku “Osamu Dazai dengan menggunakan command ```db.books.deleteMany({argument})```
![ss 2.12](../modul%202/ss%202/2.12.png)