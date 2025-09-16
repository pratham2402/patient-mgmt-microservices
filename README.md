# Patient Management System – Microservices Architecture

A production-ready, cloud-native Patient Management System built with Java, Spring Boot, Docker, and AWS. This project demonstrates a robust microservices architecture with secure authentication, real-time and event-driven communication, and infrastructure as code for seamless deployment.

---

## Table of Contents
- [Architecture Overview](#architecture-overview)
- [Tech Stack](#tech-stack)
- [Microservices](#microservices)
- [API Gateway & Load Balancing](#api-gateway--load-balancing)
- [Event-Driven & Real-Time Communication](#event-driven--real-time-communication)
- [Authentication & Security](#authentication--security)
- [Local Development & Setup](#local-development--setup)
- [Testing](#testing)
- [Deployment (AWS & LocalStack)](#deployment-aws--localstack)
- [Sample API Usage](#sample-api-usage)
- [Contributing](#contributing)
- [License](#license)

---

## Architecture Overview

- **Microservices**: Each domain (auth, patient, billing, analytics) is a separate Spring Boot service, containerized with Docker.
- **API Gateway**: Central entry point for all client requests, with load balancing and routing.
- **Databases**: Each service uses its own Postgres database for data isolation and scalability.
- **Event-Driven**: Kafka is used for asynchronous communication and event propagation between services.
- **Real-Time**: REST and gRPC are used for synchronous and high-performance inter-service communication.
- **Security**: JWT Bearer tokens secure APIs and user authentication.
- **Infrastructure as Code**: AWS resources are provisioned using CDK and tested locally with LocalStack.

---

## Tech Stack
- **Languages**: Java 17+
- **Frameworks**: Spring Boot, Spring Security, Spring Data JPA
- **Databases**: PostgreSQL
- **Messaging**: Apache Kafka
- **API**: REST (Spring MVC), gRPC (protobuf)
- **Containerization**: Docker
- **API Gateway**: Spring Cloud Gateway
- **Testing**: JUnit, Integration Tests
- **Infrastructure**: AWS CDK, LocalStack

---

## Microservices

- **auth-service**: Handles user registration, login, JWT issuance, and validation.
- **patient-service**: Manages patient records (CRUD), emits events on changes.
- **billing-service**: Manages billing accounts, listens to patient events, and processes billing.
- **analytics-service**: Consumes events for analytics and reporting.
- **api-gateway**: Routes and secures all external API traffic.

Each service has its own Dockerfile and can be built and run independently.

---

## API Gateway & Load Balancing
- All client requests go through the API Gateway.
- The gateway handles routing, load balancing, and authentication (JWT validation).
- Configuration is in `api-gateway/src/main/resources/application.yml`.

---

## Event-Driven & Real-Time Communication
- **Kafka**: Used for publishing and subscribing to domain events (e.g., patient created/updated).
- **gRPC**: Used for high-performance, real-time service-to-service calls (see proto files in each service).
- **REST**: Standard synchronous communication for most APIs.

---

## Authentication & Security
- **JWT Bearer Tokens**: Secure all APIs. Obtain a token via the auth-service and include it in the `Authorization: Bearer <token>` header.
- **Spring Security**: Used in all services to enforce authentication and authorization.

---

## Local Development & Setup

### Prerequisites
- Java 17+
- Docker & Docker Compose
- Maven

### Build & Run All Services
```sh
# Build all services
mvn clean package -DskipTests

# Build Docker images for each service
cd auth-service && docker build -t auth-service .
cd ../patient-service && docker build -t patient-service .
cd ../billing-service && docker build -t billing-service .
cd ../analytics-service && docker build -t analytics-service .
cd ../api-gateway && docker build -t api-gateway .

# (Optional) Use docker-compose.yml if provided
# docker-compose up --build
```

### Running Infrastructure Locally
- Use [LocalStack](https://github.com/localstack/localstack) to emulate AWS services locally.
- Deploy infrastructure with:
```sh
cd infrastructure
./localstack-deploy.sh
```

---

## Testing
- **Unit & Integration Tests**: Run with Maven:
```sh
mvn test
```
- **Integration tests** are in the `integration-tests/` module.
- **Sample HTTP/gRPC requests**: See `api-requests/` and `grpc-requests/` folders for ready-to-use request samples.

---

## Deployment (AWS & LocalStack)
- Infrastructure as code is managed in the `infrastructure/` directory using AWS CDK.
- To deploy to AWS, configure your AWS credentials and run the CDK deployment scripts.
- For local testing, use LocalStack as described above.

---

## Sample API Usage

### Authenticate & Get JWT
```http
POST /auth/login HTTP/1.1
Content-Type: application/json
{
  "username": "user",
  "password": "pass"
}
```

### Create Patient (with JWT)
```http
POST /patients HTTP/1.1
Authorization: Bearer <token>
Content-Type: application/json
{
  "name": "John Doe",
  "dob": "1990-01-01",
  ...
}
```

### gRPC Example
- See proto files in each service's `src/main/proto/` directory.
- Use the provided `.proto` files and `grpc-requests/` samples.

---

## Contributing
Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

---

## License
This project is licensed under the MIT License.

