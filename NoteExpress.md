# MEMBUAT BACKEND RESTFUL API DENGAN MENGGUNAKAN EXPRESS Js, ORM SEQUELIZE

- Component

- Cara penggunaan :

1. buka terminal, ketik npm init -y, untuk menginisialisasi npm pada project kita.

2. npm i express mysql2 sequelize sequelize-cli –-save, fungsi dari sequelize-cli di sini adalah untuk mempermudah membuat perintah di
   sequelize/.

Jika Error bisa menggunakan perintah ini :
$ npm i --save express mysql2
$ npm i sequalize-cli --save

Atau bisa juga menggunakan perintah :
$ npm i express mysql2 sequelize sequelize-cli

3. Install Nodemon di project dengan perintah ini:
   $ sudo npm install -g nodemon
   (Opsional biasanya ada yang sudah terinstall otomatis bebarengan dengan penginstalan node js).

4. Buat file .sequelizerc dengan menggunakan perintah ini :
   $ touch .sequelizerc

dan isi code nya dengan menggunakan code di bawah :
const path = require('path');
module.exports = {
'config': path.resolve('src/config', 'config.js'),
'models-path': path.resolve('src', 'models'),
'seeders-path': path.resolve('src/database', 'seeders'),
'migrations-path': path.resolve('src/database', 'migrations')
};

5. Buat file index.js dan isikan code seperti ini :

// require ("dotenv").config({})
const express = require ('express');
const app = express();

// inisialisasi port yang di jalankan (bebas pilih port brapa saja)
const port = process.env.PORT || 5858;
//const mainRouts = require('./src/routes');

app.use(express.urlencoded({extended:false}));
//app.use('/', mainRouts)

app.listen (port, ()=>{
console.log("server run in port " + port)
});

6. Kemudian masuk package.json, pada baris di bawah ini :

"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},

Tambahkan codenya menjadi seperti ini :
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"start" :"nodemon index.js",
"server" : "nodemon index.js"
},

7. Buat folder src kemudian ketik di terminal seperti di bawah ini :

$ cd src
$ npx sequelize init

8. Buat folder bernama Controllers dan Routes (penamaan folder bebas)

9. Jalankan perintah di terminalnya dengan perintah berikut untuk membuat databasenya :

$ npx sequelize db:create
bisa di cek respon dari terminalnya dan bisa juga cek di localhost/phpmyadmin apakah sudah terbuat databasenya secara otomatis.

9.Buat model databasenya terlebih dahulu seperti contoh ini :

$ npx sequelize model:create --name menu --attributes nama:string,harga:integer,deskripsi:string,kategori:string

menu = nama tabelnya
nama, harga deskripsi dan kategori = kolom /attributenya.

10. cek pada model dan database/migration

11. lalu jalankan npx sequelize db:migrate cek pada database maka akan ada tabel baru

12. Buat file menuController.js (penamaan bebas), kemudian tambahkan code get all seperti ini :

const { menu } = require("../models");
module.exports = {
  getAll: (req, res) => {
    menu.findAll()
      .then((data) => {
        res.send({
          msg: "succes get all data ",
          status: 200,
          data,
        });
      })
      .catch((err) => {
        res.send({
          msg: "eror get all data ",
          status: 500,
          err,
        });
      });
  }
  };


13. Buat folder menuRoutes (penamaan bebas), kemudian tambahkan code seperti ini :

const menuRoutes = require('express').Router();
const menuControllers = require('../controllers/menuController');

menuRoutes.get('/', menuControllers.getAll)
menuRoutes.post('/tambahData', menuControllers.postData)


module.exports = menuRoutes;

disini yang kita pakai hanya getall nya terlebih dahulu.

14. Kemudian buat file baru di folder routes lagi bernama index.js, yang berisi code di bawah ini :

const mainRouts = require ('express').Router();
const menuRoutes = require ('./menuRoutes');

mainRouts.use('/menu', menuRoutes);

module.exports = mainRouts;

15. coba cek di browser dengan ketik localhost:5000 (portnya)

16. tambahkan methode post data untuk menambahkan data, caranya tambahkan code di menuControllers.js sehingga code keseluruhannya seperti ini :

const { menu } = require("../models");
module.exports = {
  getAll: (req, res) => {
    menu.findAll()
      .then((data) => {
        res.send({
          msg: "succes get all data ",
          status: 200,
          data,
        });
      })
      .catch((err) => {
        res.send({
          msg: "eror get all data ",
          status: 500,
          err,
        });
      });
  },
  postData: (req, res) => {
    let { body } = req;

    // const newData = {
    //   ...body,
    //   image: req.image.url,
    // };
    menu.create(req)
      .then((data) => {
        res.send({
          msg: "succes get all data ",
          status: 200,
          data,
        });
      })
      .catch((err) => {
        res.send({
          msg: "eror get all data ",
          status: 500,
          err,
        });
      });
  }
  };

17. Kemudian untuk menambahkan data, bisa pakai ekstensi dari vscode yaitu thunder client atau bisa juga menggunakan postman, disini kita pakai thunder client, install dulu ekstensinya lalu buka.

18. ubah methode pada thunder cliennya menjadi POST, lalu ubah url thunder cliennya menjadi localhost:5000/menu/tambahData

Keterangan :

- localhost = server localhost
- 5000 = port server node js yang sedang kita jalankan
- menu = nama model tabelnya, waktu kita jalankan :
    $ npx sequelize model:create --name menu --attributes nama:string,harga:integer,deskripsi:string,kategori:string
- tambahData = mengikuti path yang ada di file menuRoutes.js

19. masukan datanya, bisa klik menu body => form-encode => kemudian isikan datanya sesuai kolomnya, contoh :

nama = Mie Ayam
harga = 7000
deskripsi = ini mie ayam
kategori = makananan

20. sebelum post, update file index.js yang ada di luar srcnya menjadi seperti ini :

// require ("dotenv").config({})
const express = require ('express');
const app = express();
const port = process.env.PORT || 5151;
const mainRouts = require('./src/routes');

app.use(express.urlencoded({extended:false}));
app.use('/', mainRouts)

app.listen(port, () => {
  console.log("server run in port " + port);
});



