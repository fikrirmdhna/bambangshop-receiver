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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Dalam kasus ini, RwLock<Vec<Notification>> digunakan untuk menyinkronkan akses ke vektor notifikasi karena memungkinkan beberapa reader atau satu writer pada satu waktu. Ini diperlukan karena metode add mungkin dipanggil secara bersamaan dari beberapa thread, dan tanpa sinkronisasi, bisa terjadi ras data yang mengarah ke perilaku yang tidak terdefinisi. Menggunakan RwLock memastikan bahwa beberapa thread dapat membaca vektor secara bersamaan tanpa saling memblokir, tetapi ketika sebuah thread ingin memodifikasi vektor (seperti menambahkan notifikasi baru), itu akan memperoleh kunci eksklusif, memastikan bahwa hanya satu thread yang dapat memodifikasi vektor pada satu waktu untuk mencegah korupsi data.
Kita tidak menggunakan Mutex di sini karena Mutex hanya memungkinkan satu thread pada satu waktu untuk mengakses data yang dijaganya, baik untuk membaca atau menulis. Karena kami memiliki skenario di mana beberapa thread mungkin ingin membaca notifikasi secara bersamaan (seperti saat mencantumkan semua notifikasi), menggunakan RwLock memberikan konkurensi yang lebih baik dengan memungkinkan beberapa reader.

2. Dalam Rust, mengubah konten dari variabel statik umumnya tidak diperbolehkan karena fokus bahasa pada keamanan dan mencegah perilaku yang tidak terdefinisi. Aturan ownership dan borrowing Rust memastikan keamanan memori pada waktu kompilasi, dan mengizinkan mutasi variabel statik melalui fungsi statik bisa menyebabkan berbagai masalah, termasuk ras data, ketidakamanan memori, dan kesulitan dalam penalaran tentang kode.
Dengan tidak mengizinkan mutasi variabel statik, Rust memastikan bahwa variabel tersebut diakses dengan cara yang terkendali dan dapat diprediksi, mencegah kelemahan umum yang terkait dengan status bersama yang dapat dimutasi. Sebagai gantinya, Rust mendorong penggunaan primitif konkurensi seperti kunci (Mutex, RwLock), saluran, atau mekanisme sinkronisasi lainnya untuk memodifikasi status bersama secara aman di seluruh thread. 

#### Reflection Subscriber-2
