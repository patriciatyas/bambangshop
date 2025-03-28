# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia


Patricia Herningtyas - 2306152241
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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

    Di case BambangShop ini, kita tidak memerlukan interface atau trait untuk Subscriber karena hanya ada satu jenis observer yang terlibat yaitu Subscriber itu sendiri. Biasanya, interface atau trait digunakan saat kita ingin mendukung beberapa jenis observer yang berbeda sehingga dapat dikelola dalam kelas yang terpisah. Namun, dalam case ini, satu struct Model sudah cukup untuk menangani kebutuhan kita. Meskipun demikian, jika mengacu pada best practice, mendefinisikan Subscriber sebagai sebuah interface atau trait dapat memberikan fleksibilitas lebih di masa depan. Hal ini akan membuat kode lebih mudah dikembangkan serta memenuhi prinsip SOLID, khususnya prinsip Open/Closed. Dengan memakai interface atau trait, kita bisa menambahkan jenis subscriber baru tanpa harus mengubah implementasi yang sudah ada, sehingga sistem lebih modular dan mudah dipelihara.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
    
    Penggunaan DashMap dalam case ini lebih efektif dibandingkan dengan Vec, terutama karena setiap Subscriber memiliki id dan url yang unik. Jika menggunakan Vec, pencarian subscriber memerlukan iterasi satu per satu (O(n)), yang akan semakin lambat seiring bertambahnya jumlah subscriber. Sebaliknya, dengan DashMap, pencarian dapat dilakukan dalam kompleksitas waktu O(1), sehingga lebih efisien. Selain itu, DashMap juga lebih cocok untuk skenario di mana data perlu diakses atau diperbarui secara cepat. Oleh karena itu, penggunaan DashMap dalam case ini tidak hanya meningkatkan kinerja, tetapi juga membuat pengelolaan data subscriber lebih terstruktur dan skalabel.


3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?


    Pola Singleton yang memastikan hanya ada satu instance objek yang berjalan tidak cukup untuk menangani masalah thread safety dalam lingkungan multithreading. Pola Singleton memang memastikan bahwa hanya ada satu instance dari hash map yang digunakan di seluruh sistem, tetapi tidak menjamin bahwa akses ke dalamnya aman jika dilakukan oleh banyak thread secara bersamaan. Ini bisa menyebabkan race condition dan inkonsistensi data. Oleh karena itu, penggunaan DashMap tetap diperluka karena selain mendukung akses bersamaan, DashMap juga dirancang untuk menangani concurrent write tanpa menyebabkan deadlock atau kondisi tak terduga lainnya. Dengan demikian, pola Singleton dan DashMap dapat digunakan secara bersamaan: Singleton memastikan hanya ada satu instance dari DashMap, sementara DashMap sendiri memastikan akses data tetap aman di lingkungan yang bersifat paralel.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    Dalam pola Model-View-Controller (MVC), pemisahan Service dan Repository dari Model sangat penting untuk menerapkan Single Responsibility Principle (SRP). Model sebaiknya hanya berfungsi sebagai representasi data, sementara Repository menangani interaksi dengan penyimpanan data, dan Service mengelola logikanya.

    Dengan memisahkan ketiga komponen ini, kode menjadi lebih modular, mudah dipahami, dan lebih terstruktur. Selain itu, pemisahan ini memungkinkan kita melakukan pengujian unit secara independen pada setiap bagian, meningkatkan fleksibilitas serta skalabilitas sistem tanpa mengganggu komponen lain. Perubahan dalam satu bagian, seperti logika yang ada di NotificationService, tidak akan langsung memengaruhi struktur data dalam Model Notification atau proses penyimpanan dalam SubscriberRepository sehingga pemeliharaan kode menjadi lebih efisien.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
    Jika seluruh fungsionalitas ditangani dalam Model tanpa pemisahan Service dan Repository, akan ada ketergantungan antar komponen yang tinggi. Model tidak hanya akan mengelola data, tetapi juga logika dan interaksi dengan penyimpanan, yang melanggar prinsip SRP.

    Misalnya, dalam aplikasi yang menangani Program, Subscriber, dan Notification, jika seluruh operasi dilakukan dalam Model, maka perubahan kecil dalam satu Model bisa berdampak luas pada bagian lain. Kompleksitas kode meningkat karena satu Model harus menangani banyak tanggung jawab, seperti yang terlihat pada fungsi subscribe dan unsubscribe di NotificationService. Tanpa pemisahan ini, setiap perubahan dalam sistem akan lebih sulit dikelola dan bisa memperlambat proses development.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
    Saya telah mengeksplorasi Postman sebagai alat uji API yang sangat membantu dalam menguji endpoint aplikasi web. Dengan Postman, kita bisa melakukan pengujian HTTP request tanpa harus menulis kode tambahan, sehingga mempercepat validasi fungsionalitas aplikasi. 

    Beberapa fitur yang saya anggap sangat berguna dalam Postman antara lain adalah kemampuan Postman untuk menyimpan request dan response, yang memudahkan uji coba berulang tanpa harus mengatur ulang konfigurasi setiap kali pengujian dilakukan. Selain itu, Postman memungkinkan kontrol penuh terhadap header dan body request, sehingga sangat membantu dalam menguji berbagai skenario API. Manajemen cookie juga menjadi fitur penting karena mempermudah pengujian autentikasi dan sesi pengguna. Terakhir, fitur dokumentasi API otomatis memudahkan kolaborasi dalam tim, terutama saat berbagi dokumentasi API dengan anggota lain, seluruh tim dapat memahami bagaimana endpoint bekerja dengan lebih efisien.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
    Dalam tutorial ini, kita menerapkan Observer Pattern dengan pendekatan Push. Dalam model ini, setiap kali terjadi perubahan data seperti create, update, delete sehingga NotificationService akan langsung mengirimkan informasi tersebut ke seluruh subscriber. Service ini akan menelusuri daftar subscriber dan mengirimkan notifikasi terbaru tanpa menunggu permintaan dari subscriber. Dengan pendekatan ini, publisher secara proaktif memberitahukan perubahan kepada subscriber melalui implementasi fungsi notify dalam NotificationService.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
    Jika kita menerapkan model Pull dalam kasus ini, ada beberapa aspek yang perlu diperhatikan. Salah satu keuntungannya adalah efisiensi penggunaan sumber daya karena data hanya diambil saat subscriber benar-benar membutuhkannya. Subscriber juga memiliki kendali penuh dalam menentukan kapan dan data mana yang ingin mereka ambil sehingga mengurangi jumlah informasi yang tidak diperlukan. Selain itu, kompleksitas dalam bagian publisher berkurang karena tidak perlu menangani pengiriman notifikasi secara langsung.

    Namun, kekurangannya adalah subscriber harus secara berkala melakukan pengecekan terhadap perubahan data di publisher. Hal ini dapat menyebabkan peningkatan beban pada sistem, terutama jika pengecekan dilakukan dalam interval yang sering. Selain itu, ada risiko peningkatan latensi karena subscriber mungkin tidak segera mendapatkan informasi perubahan, bergantung pada seberapa sering mereka menarik data dari publisher.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
    Jika proses notifikasi dilakukan tanpa menggunakan multithreading, program akan mengalami kendala dalam efisiensi dan skalabilitas. NotificationService harus mengirimkan notifikasi ke setiap subscriber secara berurutan, yang dapat menyebabkan penumpukan antrean dan memperlambat distribusi informasi. Setiap subscriber harus menunggu proses pengiriman ke subscriber sebelumnya selesai sebelum menerima notifikasi, sehingga menciptakan bottleneck.

    Masalah ini akan semakin terlihat ketika jumlah subscriber meningkat karena proses pemberitahuan akan semakin memakan waktu. Dengan menggunakan multithreading, seperti dalam implementasi `thread::spawn(move || subscriber_clone.update(payload_clone));`, kita dapat mengirimkan notifikasi ke beberapa subscriber secara bersamaan, meningkatkan kecepatan dan efisiensi proses distribusi notifikasi.

