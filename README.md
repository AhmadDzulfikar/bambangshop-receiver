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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X ] Commit: `Create Notification model struct.`
    -   [X ] Commit: `Create SubscriberRequest model struct.`
    -   [ X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X ] Commit: `Implement add function in Notification repository.`
    -   [X ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X ] Commit: `Create Notification service struct skeleton.`
    -   [ X] Commit: `Implement subscribe function in Notification service.`
    -   [ X] Commit: `Implement subscribe function in Notification controller.`
    -   [X ] Commit: `Implement unsubscribe function in Notification service.`
    -   [X ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X ] Commit: `Implement receive_notification function in Notification service.`
    -   [ X] Commit: `Implement receive function in Notification controller.`
    -   [X ] Commit: `Implement list_messages function in Notification service.`
    -   [X ] Commit: `Implement list function in Notification controller.`
    -   [ X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
> In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Penggunaan RwLock<> lebih tepat dalam kasus ini karena memungkinkan beberapa thread untuk membaca data secara bersamaan tanpa harus menunggu thread lain menyelesaikan tugasnya. Ini sangat berguna jika operasi baca lebih sering terjadi dibanding operasi tulis, karena tidak akan terjadi blocking kecuali saat ada operasi tulis.

Sebaliknya, jika kita menggunakan Mutex<>, hanya satu thread yang dapat mengakses data pada satu waktu, baik untuk membaca maupun menulis. Hal ini kurang efisien jika ada banyak operasi baca karena thread lain harus menunggu akses, meskipun mereka hanya ingin membaca data. Oleh karena itu, RwLock<> dipilih untuk meningkatkan performa dan efisiensi dalam skenario ini.

> In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Rust tidak mengizinkan mutasi langsung pada variabel static untuk menjamin memory safety dan concurrency safety. Dalam Java, mutasi langsung pada variabel static bisa dilakukan karena Java memiliki garbage collector dan runtime safety checks yang menangani masalah race conditions secara otomatis.

Namun, Rust tidak memiliki garbage collector, sehingga tanggung jawab untuk memastikan keamanan data dalam lingkungan multi-threading ada pada pengembang. Oleh karena itu, Rust mewajibkan penggunaan mekanisme seperti lazy_static! dengan RwLock<> atau Mutex<> untuk memberikan kontrol eksplisit terhadap bagaimana data diakses dan dimodifikasi, memastikan program tetap aman dan bebas dari race condition.
#### Reflection Subscriber-2
> Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Ya, saya sempat mengeksplorasi kode di lib.rs, yang merupakan bagian dari aplikasi Rocket (framework web di Rust). File ini mengatur konfigurasi aplikasi, menyediakan klien HTTP global menggunakan Reqwest, serta mendefinisikan struktur untuk menangani error. Selain itu, saya juga melihat main.rs, yang berfungsi sebagai entry point dari aplikasi berbasis Rocket. Di dalamnya, aplikasi mengelola instance global reqwest::Client untuk melakukan HTTP request serta menangani pendaftaran rute API.

> Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Observer Pattern memudahkan penambahan subscriber karena subscriber hanya perlu mendaftarkan dirinya ke publisher, tanpa perlu mengubah logika utama. Ketika ada event baru, semua subscriber yang telah terdaftar akan otomatis menerima notifikasi, sehingga sistem tetap modular dan fleksibel.

Namun, jika kita menambahkan lebih dari satu instance Main app, kompleksitas bisa meningkat. Hal ini disebabkan oleh perlunya sinkronisasi data antar publisher dan subscriber, terutama jika aplikasi berjalan dalam beberapa instance yang berbeda. Untuk mengatasi hal ini, kita mungkin perlu menambahkan mekanisme tambahan, seperti database terpusat atau message queue, guna memastikan bahwa setiap instance memiliki data subscriber yang konsisten.

> Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Belum sempat, tetapi saya menyadari bahwa membuat unit test akan sangat berguna, terutama dalam proyek kelompok. Dengan adanya unit tests, kita bisa:

- Memverifikasi respons API secara otomatis tanpa harus mengetes secara manual.
- Memastikan status kode dan data yang dikembalikan sesuai ekspektasi, sehingga debugging menjadi lebih cepat.
- Meningkatkan keandalan sistem, karena kita bisa melakukan regression testing dengan lebih mudah jika ada perubahan dalam kode.
