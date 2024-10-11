# E-Commerce Review Service with Spring Boot, Wiremock, Debezium, Kafka, SQL DB, and Redis

## Project Overview

This project implements a **Review Service** as part of an e-commerce platform. The service allows users to submit, view, delete and list reviews for products(s), with admins able to moderate them. The project integrates **Spring Boot**, **Debezium**, **Kafka**, **SQL Database**, and **Redis**, with **Wiremock** for simulating dependent services. The following sections outline the key steps involved in building the project.

## Steps

### Step 1: Setting up the Spring Boot Service
The core of this system is built using **Spring Boot**, which simplifies the creation of RESTful web services. In this step:
- We set up the Spring boot Application for the endpoints catering to Review and Moderation service endpoints.
- Configure initial dependencies for **Spring Data JPA** (for database interactions) and **Spring Web** (for exposing REST APIs) and other dependencies as required.
- The service includes endpoints for creating, reading, listing and deleting reviews, which can be moderated by admins.
- The embedded Tomcat server allows the service to be run locally for easier development.

### Step 2: Integrating Kafka for Asynchronous Communication
**Apache Kafka** is used for asynchronous communication between the Review Service and a **Moderation Service**:
- **Kafka** serves as a message broker to ensure that user reviews are submitted to the Moderation Service for approval .
- This step involves configuring Kafka topics for publishing new reviews and consuming moderation results (details will be later in Design Doc).
- The Review Service acts as a producer, publishing review events, while the Moderation Service acts as a consumer.


### Step 3: Setting Up SQL Database for Persistent Data Storage possibly through Master - Slave formats
The **SQL Database** stores review and moderation data persistently:
- Reviews are stored with fields like `review_id`, `product_id`, `user_id`, `rating`, and `content`.
- **Spring Data JPA** simplifies the interaction with the database by abstracting CRUD operations.
- Database migrations are managed using tools like **Flyway** to handle schema changes.

### Step 4: Implementing Debezium for Data Change Capture
To maintain data consistency across services, we integrate **Debezium**:
- **Debezium** captures row-level changes in the SQL database and streams them to Kafka topics.
- Changes made by the Moderation Service, such as approving or rejecting reviews, are automatically reflected in the Review Service’s database.
- This step involves setting up Debezium connectors for change data capture and ensuring data synchronization without direct service-to-service communication.

### Step 5: Introducing Redis for Caching
**Redis** is introduced as a caching layer to reduce latency and improve performance:
- Redis stores frequently accessed data (such as recent reviews) in memory.
- The Review Service first checks Redis before querying the SQL database, improving response times.
- Redis helps to manage temporary data such as in-progress moderation results or cached review information.

### Step 6: Local Testing and Service Simulation with Wiremock
For local testing, **Wiremock** is used to mock the services like User, Product:
- Wiremock allows the Review Service to be tested independently by simulating the responses of the mocked services.
- Mock endpoints return predefined responses, enabling local development and testing without full service integration.
- This step allows the testing of different scenarios, such as varying response times or network failures.

### Step 7: Unit Testing
Testing is a crucial aspect of ensuring the reliability of the Review Service. **JUnit** and **Mockito** are used to create unit tests that verify the functionality of individual components. This step focuses on testing the business logic, service layer, and interactions with external systems.

- **JUnit** is used to write test cases for the core logic, including review creation, retrieval, and deletion. Each method is tested for expected outcomes under normal and edge cases.
- **Mockito** is employed to mock dependencies such as the database, Kafka producer, and Redis cache. This ensures that the unit tests remain focused on the service logic and do not require actual external resources like databases or message brokers.
- Additionally, integration tests using @SpringBootTest can be configured to test the interaction between the Review Service and the Moderation Service through Kafka, verifying that events are published and consumed correctly.

Unit testing is automated through **GitHub Actions** or other CI/CD tools to ensure code quality and functionality are maintained with each update. This step ensures that the service remains reliable and scalable as it evolves.

## Conclusion
This project combines several technologies—**Spring Boot**, **Kafka**, **Debezium**, **SQL**, and **Redis**—to create a scalable and resilient Review Service. It leverages **Wiremock** for local testing, allowing the service to be developed and tested in isolation. Future improvements could include adding security, optimizing caching, and scaling the Kafka infrastructure.

### References:
To aid in understanding the technologies and setting up the environment using Docker, here are essential references to official documentation, tutorials, and guides:

- **Spring Boot**: 
  - [Official Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
  - [Spring Boot Guide](https://spring.io/guides/gs/spring-boot/)
  
- **Kafka**:
  - [Kafka Official Documentation](https://kafka.apache.org/documentation/)
  - [Kafka Docker Setup Guide](https://developer.confluent.io/quickstart/kafka-docker/)
  
- **Debezium**: 
  - [Debezium Documentation](https://debezium.io/documentation/reference/stable/)
  - [Debezium Docker Setup Guide](https://debezium.io/documentation/reference/stable/tutorial.html#setting-up-debezium-with-docker)
  - https://www.youtube.com/watch?v=R873BlNVUB4&list=PLHQXtr5rNVQStxWhF3sD_rGl-R_PGFzwB 
  
- **SQL Databases**: 
  - [Spring Data JPA Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
  - [MySQL Docker Setup](https://hub.docker.com/_/mysql)
  - https://www.youtube.com/watch?v=kOrGN36ViaU 

- **Redis**: 
  - [Redis Documentation](https://redis.io/documentation)
  - [Redis Docker Setup Guide](https://hub.docker.com/_/redis)

- **Wiremock**: 
  - [Wiremock Documentation](http://wiremock.org/docs/)
  - [Wiremock Docker Setup Guide](https://hub.docker.com/r/wiremock/wiremock)
  - https://www.youtube.com/watch?v=jxviJMeqESg 

- **JUnit**:
  - [JUnit 5 Documentation](https://junit.org/junit5/docs/current/user-guide/)
  - [JUnit 5 Testing Guide](https://www.baeldung.com/junit-5)

- **Mockito**:
  - [Mockito Documentation](https://site.mockito.org/)
  - [Mockito Guide for Unit Testing](https://www.baeldung.com/mockito-series)

- **GitHub Actions**:
  - [GitHub Actions Documentation](https://docs.github.com/en/actions)
  - [Automating CI/CD with GitHub Actions](https://docs.github.com/en/actions/automating-builds-and-tests/about-continuous-integration)

- **Docker Setup for the Project**:
  - [Debezium + Kafka + MySQL Docker Setup](https://debezium.io/documentation/reference/stable/tutorial.html#setting-up-debezium-with-docker)
  - [MySQL Docker Hub Setup](https://hub.docker.com/_/mysql)
  - [Kafka Docker Setup with Confluent](https://docs.confluent.io/platform/current/quickstart/ce-docker-quickstart.html)
  - [Redis Docker Hub Setup](https://hub.docker.com/_/redis)
  - [Wiremock Docker Hub Setup](https://hub.docker.com/r/wiremock/wiremock)
