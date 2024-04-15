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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?

    *In the Observer design pattern, an interface (or trait in Rust) is typically used to define the methods or services that observers will implement. However, in the case of BambangShop, the use of an Interface or Trait may not be necessary as there isn't much variation in the expected behavior of observers. Instead, a single Model struct that encompasses data about observers and the necessary behavior to notify them would suffice. Therefore, in this case, using a single Model struct for observers is adequate.*

2. **id** in **Program** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?

    *While using a Vec (list) may be sufficient if we only need to store unique IDs or URLs of subscribers, employing DashMap (map/dictionary) as currently utilized is recommended. DashMap offers advantages in terms of speed of access and data manipulation. With DashMap, we can easily check the existence of IDs or URLs, and swiftly access or update existing entries. This can lead to more efficiency and better performance as the application scales.*

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (**SUBSCRIBERS**) static variable, we used the **DashMap** external library for **thread safe HashMap**. Explain based on your understanding of design patterns, do we still need **DashMap** or we can implement Singleton pattern instead?

    *In the Singleton Pattern, a class has only one instance throughout the application. In this scenario, using DashMap for the SUBSCRIBERS static variable is the preferable choice as it allows access to the same data structure from anywhere in the application, ensuring concurrent safety. DashMap provides a robust solution for concurrency safety in a multithreaded environment, which is crucial in Rust programming. Although Singleton pattern can be implemented in Rust, it might not be necessary in this case as DashMap effectively provides the required functionality while maintaining shared safety. Therefore, the use of DashMap is the appropriate choice for the SUBSCRIBERS static variable in the BambangShop application.*


#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model? 

    *The separation of "Service" and "Repository" from a Model is necessary due to the Single Responsibility Principle (SRP) from the SOLID principles of software design. SRP dictates that a class should have only one reason to change. Thus, we need to separate concerns to maintain code cohesion and manageability. In the context of MVC, the Model traditionally encompasses both data storage and business logic. However, by separating business logic into Services and data access into Repositories, we adhere more closely to SRP. This separation allows the Model to focus solely on representing data, making it easier to understand, maintain, and extend. Additionally, it facilitates flexibility, as the Model can seamlessly transition between different data storage implementations without affecting its core responsibilities.*

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (**Program, Subscriber, Notification**) affect the code complexity for each model?

    *Solely relying on the Model without separating concerns into Service and Repository components can lead to increased code complexity and violation of SRP. Each Model (Program, Subscriber, Notification) would be burdened with both data representation and business logic responsibilities, resulting in classes with multiple reasons to change. This can lead to tightly coupled interactions between models, complicating maintenance and expansion efforts. For instance, interactions between different models may become convoluted, with direct dependencies leading to code that is harder to comprehend and maintain. Overall, without proper separation of concerns, code complexity for each model would escalate significantly, impeding understandability, testing, and maintenance efforts.*

3. Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    *Postman serves as a valuable tool for testing and interacting with APIs, aiding in the development and validation of software projects. It allows me to simulate API responses and test endpoints without the need for frontend HTML. Key features of Postman that I find useful for group projects and future software engineering endeavors include:*
    - *Sending Requests: Easily sending HTTP requests to API endpoints with customizable parameters such as headers, query parameters, and request bodies.*
    - *Automated Testing: Creating and executing automated test suites within Postman to verify the behavior of API endpoints. This includes tests for response status codes, response body content, and more.*
    - *Environment Variables: Defining and utilizing environment variables to streamline testing across different environments (e.g., development, staging, production) when testing APIs. This feature simplifies the process of switching between different environments during testing and development.*

#### Reflection Publisher-3
1. Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and **Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    *In this tutorial case, we are utilizing the Push model variation of the Observer Pattern. This can be inferred from the description that notifications are sent to subscribers who subscribe via HTTP POST requests triggered by product creation, promotions, or deletion.*

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    *Advantages:*
    - *On-Demand Data Retrieval: With the Pull model, subscribers fetch data only when needed, reducing network and computational resource usage.*
    - *Subscriber Control: Subscribers have full autonomy over when to retrieve data, enabling them to avoid unnecessary data requests.*

    *Disadvantages:*

    - *Delayed Updates: In the Pull model, subscribers must actively request updates, potentially leading to delays in receiving new information. This could be inefficient in scenarios where immediate updates are crucial.*
    - *Increased Logic Complexity: Implementing the Pull model may necessitate additional logic on the subscriber side to manage data requests and updates efficiently. This could introduce complexity to the codebase and require more effort to maintain.*

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    *If we decide not to use multi-threading in the notification process, notifications will be processed sequentially and in a serial manner. This means each notification will be processed one by one, waiting for the previous notification to finish before processing the next one. As a result, if there are many notifications that need to be sent, this process can become slow and cause delays in providing responses to users. Without multi-threading, the application may feel sluggish and less responsive, especially when sending a large number of notifications. By using multi-threading, we can process notifications in parallel, speeding up the process and enhancing the responsiveness of the application.*

