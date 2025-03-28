# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X] Commit: `Implement add function in Notification repository.`
    -   [X] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Commit: `Implement receive_notification function in Notification service.`
    -   [X] Commit: `Implement receive function in Notification controller.`
    -   [X] Commit: `Implement list_messages function in Notification service.`
    -   [X] Commit: `Implement list function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. RwLock<> digunakan untuk menyinkronkan akses ke Vec dari Notifikasi karena tipe ini memungkinkan beberapa thread untuk membaca data secara bersamaan, sementara hanya satu thread yang diizinkan untuk menulis pada satu waktu. Pendekatan ini lebih efisien apabila operasi baca lebih sering terjadi dibandingkan operasi tulis. Jika menggunakan Mutex<>, semua akses, baik untuk membaca maupun menulis, memerlukan penguncian data, yang akan membuat thread lain menunggu, bahkan untuk sekadar membaca. Oleh karena itu, penggunaan RwLock<> lebih tepat dalam kasus ini.

2. Rust tidak mengizinkan kita untuk memodifikasi konten variabel static secara langsung seperti di Java karena Rust mengutamakan keamanan memori dan thread safety. Di Java, meskipun kita bisa mengubah nilai variabel static melalui fungsi static, hal ini berisiko menimbulkan masalah seperti race condition ketika banyak thread mengakses variabel tersebut secara bersamaan. Sementara itu, Rust memaksa penggunaan mekanisme seperti lazy_static untuk inisialisasi variabel static secara aman, serta tipe seperti RwLock<> atau Mutex<> untuk memastikan bahwa akses ke data tersebut aman dalam konteks multithreading. 
#### Reflection Subscriber-2
1. Saya belum mengeksplorasi bagian lain di luar langkah-langkah tutorial, seperti src/lib.rs, karena saya belum belajar Rust lebih dalam. Fokus saya saat ini adalah memahami konsep dasar rust dan penerapannya dalam tutorial. Kedepannya saya berencana untuk memperdalam pemahaman tentang Rust dan mulai mengeksplorasi bagian lain dari kode yang lebih kompleks, seperti src/lib.rs, untuk melihat bagaimana semuanya terhubung dalam keseluruhan aplikasi.

2. Observer pattern sangat memudahkan dalam menambahkan lebih banyak subscriber. Dengan Observer, ketika ada penambahan subscriber baru, subscriber tersebut akan langsung terdaftar dan mulai menerima notifikasi tanpa perlu mengubah logika yang ada. Hal ini memungkinkan penambahan subscriber baru menjadi sangat mudah. Jika kita menambahkan lebih dari satu instance dari aplikasi Main, sistem tetap fleksibel karena setiap instance hanya perlu mendaftar sebagai subscriber pada sistem, dan sistem notifikasi akan mengurus distribusi pesan ke semua instance yang terdaftar.

3. Saya belum mencoba membuat test sendiri. Saya hanya melakukan pengujian menggunakan Postman collection yang ada di tutorial. Membuat test sendiri sangat berguna karena memungkinkan kita untuk menguji fungsionalitas aplikasi secara lebih mendalam dan spesifik. Membuat test sendiri juga memberikan kita kontrol lebih besar terhadap skenario yang ingin kita uji.  Penambahan dokumentasi yang lebih rinci tentang setiap endpoint dan contoh request/response bisa membantu anggota kelompok di group project dalam menguji API dan membantu pemahaman cara kerja API. 









