# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam kasus ini, kita tidak membutuhkan interface atau trait karena kita hanya memiliki satu observer, yaitu Subscriber. Biasanya, interface atau trait digunakan ketika kita bekerja dengan berbagai jenis observer yang dikelompokkan dalam kelas yang berbeda, namun di sini, satu kelas sudah cukup untuk memenuhi kebutuhan.
2. Dashmap sangat mempermudah pengelolaan data produk beserta subscriber-nya. Jika kita hanya menggunakan vektor, kita akan membutuhkan dua vektor terpisah, yang tentu saja membuat pencarian data menjadi lebih rumit dan kurang efisien.
3. Dashmap adalah built-in data structure yang mendukung multithreading. Dalam aplikasi Bambangshop yang menggunakan multithreading, Map Subscriber akan diakses oleh banyak thread. Singleton memastikan bahwa hanya ada satu instance objek yang berjalan selama program, yang menjaga agar daftar subscriber terhadap produk tetap berada dalam satu Dashmap.

#### Reflection Publisher-2
1. Dengan memisahkan Service dan Repository, kita mengadopsi prinsip SRP. Service bertanggung jawab untuk mengambil dan mengolah data, sedangkan Repository menangani akses dan penyimpanan data ke dalam tempat penyimpanan seperti database. Pemisahan ini menciptakan kode yang lebih bersih, dengan setiap komponen memiliki peran yang jelas. Ini membuat kode lebih mudah dipahami, dimodifikasi, dan dipelihara, karena perubahan di satu bagian tidak akan mengganggu bagian lainnya.
2. Tanpa pembagian seperti Service dan Repository, kita akan menghadapi coupling yang tinggi untuk antara komponen. Setiap perubahan pada satu bagian bisa berisiko menyebabkan perubahan besar di bagian lain, yang tentu akan mempersulit maintainablity dan development lebih lanjut.
3. Postman adalah alat yang sangat berguna untuk menguji endpoint aplikasi kita. Dengan Postman, kita bisa mengeksplorasi bagaimana aplikasi merespons berbagai request, apakah sesuai dengan yang kita harapkan atau tidak.

#### Reflection Publisher-3
1. Di sini kita menerapkan Push Model, di mana setiap kali terjadi perubahan data, baik itu create, update, atau delete, akan memicu pemanggilan NotificationService. Service ini yang akan mengiterasi semua subscriber dan memberikan update terbaru.
2. Pull Model memiliki keuntungan dalam hal efisiensi. Program hanya akan mengirimkan data ketika subscriber memerlukannya, yang berarti resource akan lebih hemat, tidak hanya itu observer dapat memilih apakah perubahan data tersebut relevan atau tidak. Namun, tantangannya adalah observer harus mengetahui struktur data sumbernya agar bisa melakukan request yang tepat.
3. Tanpa menggunakan multithreading, kita akan menghadapi masalah antrian panjang karena NotificationService harus memberi tahu tiap subscriber satu per satu, yang bisa menyebabkan bottleneck dan memperlambat pengiriman notifikasi.