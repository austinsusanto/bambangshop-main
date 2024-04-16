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
    | variable | type | description |
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

1. If there will be only one type of subscriber, then an interface is not needed and a single model struct will be enough. This is because there will be no other kind of subscriber (receiver) model that will be used in the application which is very relevant to the case in Bambang Shop. Although this approach is easier and shorter codewise, the option to add a new type of receiver will be lost. We will need to make a new trait and change written codes to make this option available again.

2. Using a Vec to store id of Product and url of Subscriber is possible and sufficient. But it makes the program slower because a linear search will have to be done to add or delete a Product or a Subscriber. Using a Vec for this case also makes room for error due to the elements non-unique behavior inside a Vec.

3. The use of a DashMap is not mandatory and it can be replaced by the Singleton Pattern implementation. But a DashMap helps a lot by providing a thread-safety HashMap for concurrent access. Even though this is also achievable using the Singleton Pattern, it is harder to make a fully thread-safe implementation and potentially creates new problems in the program.

#### Reflection Publisher-2

1. The separation of "Service" and "Repository" from "Model" has a lot of important reasons behind it. First, it fulfills the design principle of Single Responsibility (SRP) where a file/class should only be responsible of one thing. This helps in maintaining the project from being too stuffed into one place and helps with error handling. Secondly, this implementation enforces the concept of Separation of Concerns (SoC) where the data manipulation and proccesses (Service and Repository) is separated from the more sensitive and crucial part of the program (Model).

2. If we only use the Model, the code will most probably be more complex. This applies to all the models in the tutorial (Product, Subscriber, and Notification). Eventhough some parts might be relatively less complex like imports and the mod (because no connections between Model, Repository, and Service aren't needed), but most of the other code will be much more complex due to the lack of SRP and SoC implementation.

3. Postman is a great tool on testing the functionality of our program. It provides complete access to data and statistics of our program (Response, Cookies, Headers, Response Time, Data Usage, etc.). I'm also interested in trying other request type like PUT, PATCH, DELETE, OPTIONS because we haven't learnt much of it in the courses.

#### Reflection Publisher-3

1. In this tutorial, we use the Push model where the publisher pushes data to the subscriber which happens every time and update happened so the subscriber doesn't have to pull data manually from the publisher.

2. If we used the Pull model, the subscriber will only get the update/notification when they make a new request manually which makes the application less interactive. But, the Pull model is more simple and efficient because the subscriber can control over their data access and not mandated to take the new data. It also helps in reducing coupling between the subscriber and publisher because the requests made by the subscriber doesn't access the publisher.

3. If we didn't use multi-threading in the notification proccess, the subscriber will get a late notification. This can happen because the program will wait for one notification to be sent to one subscriber before sending it to the next one. The latency will get worse if a product has many subscriber.
