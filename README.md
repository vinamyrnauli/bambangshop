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
###### 1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?
* *Observer design patterns* memungkinkan objek untuk diberitahu tentang perubahan pada objek lain Biasanya, ini menggunakan *trait* pada Rust atau *interface* untuk mendefinisikan metode-metode yang ingin digunakan.
* Penggunaan *interface* atau *trait* pada Bambangshop tidak sepenuhnya diperlukan karena tidak ada variasi dalam behaviour* yang diharapkan dari *observer*.
* Untuk alternatifnya, penggunaan satu struktur Model yang mencakup data mengenai *observer* dan *behaviour* yang diperlukan untuk memberi tahu bahwa sudah memadai. Jadi, penggunaan satu struktur Model untuk *observer* sudah memadai.

###### 2. **id** in **Program** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?
* Apabila hanya ingin menyimpan ID atau URL dari *customer*, penggunaan Vec (list) sudah memadai. Akan tetapi, DashMap (*map/dictionary*) lebih disarankan karena memberikan keunggulan dalam kecepatan akses dan manipulasi data.
* Dengan menggunakan DashMap, pengecekan terhadap ID atau URL yang sudah ada akan menjadi lebih sederhana. Selain itu, DashMap juga menyediakan akses dan pembaruan entri yang sudah ada dengan kecepatan yang lebih tinggi. Hal ini dapat meningkatkan efisiensi dan kinerja aplikasi.

###### 3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (**SUBSCRIBERS**) static variable, we used the **DashMap** external library for **thread safe HashMap**. Explain based on your understanding of design patterns, do we still need **DashMap** or we can implement Singleton pattern instead?
* *Singleton pattern* menjamin bahwa sebuah *class* hanya memiliki satu *instance* di seluruh aplikasi.
* Penggunaan DashMap untuk *static variable* SUBSCRIBERS merupakan pilihan terbaik karena memungkinkan akses ke struktur data yang sama dari mana pun dalam aplikasi serta menjaga keamanan konkurensi. Jadi, DashMap memberikan solusi yang efektif untuk mengelola keamanan konkurensi dalam *multi-threaded environment* yang penting dalam pengembangan program Rust.
* Walaupun pola Singleton dapat diimplementasikan dalam Rust tetapi penggunaannya tidak terlalu perlu pada kasus ini karena DashMap sudah menyediakan fungsionalitas yang diperlukan. Jadi, penggunaan DashMap adalah opsi yang tepat untuk kebutuhan *static variable* SUBSCRIBERS dalam Bambangshop.

#### Reflection Publisher-2
###### 1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
* Prinsip SRP (*Single Responsibility Principle*) menjelaskan bahwa penting untuk *class* yang hanya fokus pada satu tanggung jawab. Maka dari itu, perlunya pemisahan Service dan Repository dari Model sehingga Model hanya berkonsentrasi pada definisi struktur data.
* *Design principle* menekankan mengenai pemisahan komponen berbeda agar setiap bagian bertanggung jawab atas tugasnya sendiri. Dengan memisahkan *business logic* ke Service dan operasi penyimpanan data ke Repository, Model hanya berfungsi sebagai representasi struktur data.
* Pendekatan ini memungkinkan Model untuk lebih fleksibel dan mudah menyesuaikan perubahan dalam struktur data atau penyimpanan data tanpa harus memikirkan metode yang tidak diperlukan.

###### 2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (**Program, Subscriber, Notification**) affect the code complexity for each model?
* Jika kita hanya mengandalkan Model tanpa membagi tanggung jawab menjadi Service dan Repository akan menyebabkan model bertanggung jawab atas representasi data dan *business logic*. Hal ini dapat menghasilkan *class-class* dengan tanggung jawab yang berlebihan yang tidak sesuai dengan prinsip SRP.
* Dikarenakan kesalahan ini, dapat mengakibatkan peningkatan kompleksitas kode, kesulitan dalam *maintain* dan memperluas kode, interaksi antar-model akan lebih rumit dan terikat satu sama lain, serta ketergantungan yang kuat antar-kode.
* Misalnya, jika suatu Product harus memberi tahu Subscriber mengenai perubahan, bisa saja langsung berinteraksi dengan objek Subscriber untuk mengirimkan Notification. Hal ini tidak ideal karena melanggar prinsip enkapsulasi dan sulit dipahami kodenya.
* Jadi, tanpa membagi tanggung jawab dengan benar, kompleksitas kode untuk setiap Model akan meningkat. Hal ini dapat membuat kode menjadi sulit dipahami, diuji, dan dipelihara.

###### 3. Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
* Postman dapat membantu dalam *testing* program dengan mengirimkan respons menuju *endpoint* API. Hal ini dapat memungkinkan simulasi, tanpa perlu HTML.
* Postman dapat mengirim permintaan, yaitu memfasilitasi pengiriman permintaan HTTP menuju *endpoint* API dengan parameter yang dapat disesuaikan, seperti *header, query* parameter, dan *request body*.
* Postman memiliki *testing* otomatis yang memungkinkan pembuatan dan eksekusi serangkaian *testing* otomatis untuk memverifikasi *behaviour endpoint* API, termasuk status kode respons, isi *body* respons, dan lainnya.
* Postman memiliki fitur *environment variable* yang dapat mempermudah beralih antara *environment* yang berbeda, seperti *development, staging, production* saat menguji API.

#### Reflection Publisher-3
###### 1. Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and **Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
* Pada *module* ini, kita menggunakan variasi model Push dari *Observer Pattern* yang dapat dilihat dari penggunaan *request* HTTP POST untuk mengirimkan notifikasi kepada *customer* yang telah berlangganan.
* Notifikasi ini dipicu oleh *create, promote,* atau *delete* produk.

###### 2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
* Kelebihan: mengurangi penggunaan sumber daya jaringan dan komputasi karena Subscriber hanya mengambil data saat diperlukan. Selain itu, Subscriber memiliki kendali penuh atas *timing* pengambilan data yang dapat memungkinkan mereka menghindari pengambilan data yang tidak diperlukan.
* Kekurangan: *customer* harus secara aktif meminta pembaruan yang menyebabkan keterlambatan dalam mendapatkan informasi terbaru. Hal ini tidak efisien di mana pembaruan harus segera diterima. Selain itu, implementasi Model Pull bisa saja membutuhkan penambahan *logic* di sisi *customer* untuk mengelola *request* dan pembaruan data secara efisien.

###### 3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
* Proses notifikasi akan dilakukan secara berurutan dengan setiap notifikasi diproses satu per-satu. Lalu, kita menunggu hingga notifikasi sebelumnya selesai sebelum memproses yang berikutnya.
* Apabila terdapat banyak notifikasi yang harus dikirim, proses dapat menjadi lambat dan menyebabkan *delay* dalam memberikan respons.
* Apabila kita menggunakan *multi-threading*, notifikasi dapat diproses secara paralel sehingga dapat mempercepat proses serta meningkatkan responsivitas aplikasi. Selain itu, penggunaan ini lebih efisien ketika ingin mengirim banyak notifikasi. 