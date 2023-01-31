# MEMBUAT BACKEND RESTFUL API DENGAN MENGGUNAKAN EXPRESS JS WITH ORM SEQUELIZE

## Component

## Cara penggunaan :

- Buka terminal, ketik npm init -y, untuk menginisialisasi npm pada project kita.

      $ npm i express mysql2 sequelize sequelize-cli –-save

fungsi dari sequelize-cli di sini adalah untuk mempermudah membuat perintah di
sequelize

Jika Error bisa menggunakan perintah ini :

      $ npm i --save express mysql2
      $ npm i sequalize-cli --save

Atau bisa juga menggunakan perintah :

      $ npm i express mysql2 sequelize sequelize-cli

- Install Nodemon di project dengan perintah ini:

   $ sudo npm install -g nodemon
   (Opsional biasanya ada yang sudah terinstall otomatis bebarengan dengan penginstalan node js).

- Buat file .sequelizerc dengan menggunakan perintah ini :

   $ touch .sequelizerc

dan isi code nya dengan menggunakan code di bawah :

      const path = require('path');
      module.exports = {
      'config': path.resolve('src/config', 'config.js'),
      'models-path': path.resolve('src', 'models'),
      'seeders-path': path.resolve('src/database', 'seeders'),
      'migrations-path': path.resolve('src/database', 'migrations')
      };

- Buat file index.js dan isikan code seperti ini :

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

      - Kemudian masuk package.json, pada baris di bawah ini :

      "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
      },

      Tambahkan codenya menjadi seperti ini :
      "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "start" :"nodemon index.js",
      "server" : "nodemon index.js"
      },

- Buat folder src kemudian ketik di terminal seperti di bawah ini :

      $ cd src
      $ npx sequelize init

- Buat folder bernama Controllers dan Routes (penamaan folder bebas)

- Jalankan perintah di terminalnya dengan perintah berikut untuk membuat databasenya :

      $ npx sequelize db:create

bisa di cek respon dari terminalnya dan bisa juga cek di localhost/phpmyadmin apakah sudah terbuat databasenya secara otomatis.

- Buat model databasenya terlebih dahulu seperti contoh ini :

      $ npx sequelize model:create --name menu --attributes nama:string,harga:integer,deskripsi:string,kategori:string

Keterangan :

menu = nama tabelnya
nama, harga deskripsi dan kategori = kolom /attributenya.

- cek pada model dan database/migration

- lalu jalankan npx sequelize db:migrate cek pada database maka akan ada tabel baru

- Buat file menuController.js (penamaan bebas), pada folder controller kemudian tambahkan code get all seperti ini :

      // File /controller/menuController.js
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

- Buat folder menuRoutes.js (penamaan bebas), pada folder routes kemudian tambahkan code seperti ini :

      // FILE menuRoutes.js
      const menuRoutes = require('express').Router();
      const menuControllers = require('../controllers/menuController');

      menuRoutes.get('/', menuControllers.getAll)
      menuRoutes.post('/tambahData', menuControllers.postData)

      module.exports = menuRoutes;

disini yang kita pakai hanya getall nya terlebih dahulu.

- Kemudian buat file baru di folder routes lagi bernama index.js, yang berisi code di bawah ini :

      // File /routes/index.js
      const mainRouts = require ('express').Router();
      const menuRoutes = require ('./menuRoutes');

      mainRouts.use('/menu', menuRoutes);

      module.exports = mainRouts;

- coba cek di browser dengan ketik localhost:5000 (portnya)

- tambahkan methode post data untuk menambahkan data, caranya tambahkan code di menuControllers.js sehingga code keseluruhannya seperti ini :

      // GET DAN POST DATA
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

- Kemudian untuk menambahkan data, bisa pakai ekstensi dari vscode yaitu thunder client atau bisa juga menggunakan postman, disini kita pakai thunder client, install dulu ekstensinya lalu buka.

- ubah methode pada thunder cliennya menjadi POST, lalu ubah url thunder cliennya menjadi localhost:5000/menu/tambahData

  Keterangan :

  - localhost = server localhost
  - 5000 = port server node js yang sedang kita jalankan
  - menu = nama model tabelnya, waktu kita jalankan :
    $ npx sequelize model:create --name menu --attributes nama:string,harga:integer,deskripsi:string,kategori:string
  - tambahData = mengikuti path yang ada di file menuRoutes.js

- masukan datanya, bisa klik menu body => form-encode => kemudian isikan datanya sesuai kolomnya, contoh :

  - nama = Mie Ayam
  - harga = 7000
  - deskripsi = ini mie ayam
  - kategori = makananan

- sebelum post, update file index.js yang ada di luar srcnya menjadi seperti ini :

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
