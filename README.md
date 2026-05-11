# CloudShare ☁️

A production-inspired cloud storage and file sharing platform built using Spring Boot. CloudShare enables secure file uploads, public/private sharing, JWT-based authentication, payment-based credit management, and observability using Prometheus and Grafana.

---

# 🚀 Features

## 🔐 Authentication & Security

* Clerk OAuth authentication
* JWT-based stateless authentication
* Secure protected endpoints
* Role-based access control
* Public/private file access management
* CORS configuration for frontend integration

## 📁 File Management

* Multipart file upload
* Secure file download
* Public file sharing
* File visibility toggling
* Metadata management
* Binary storage handling

## 💳 Credit & Payment System

* Upload credit tracking
* Razorpay payment integration
* Payment verification using HMAC-SHA256
* Credit purchase plans
* Transaction history tracking

## 📊 Monitoring & Observability

* Spring Boot Actuator metrics
* Prometheus metrics scraping
* Grafana dashboard visualization
* Health monitoring
* Request and JVM metrics

## 🐳 Deployment

* Dockerized backend deployment
* Docker Compose support
* Environment-based configuration

---

# 🏗️ High-Level Architecture

```text
                     ┌────────────────────┐
                     │     Frontend       │
                     │ React / Next.js UI │
                     └─────────┬──────────┘
                               │ HTTP Requests
                               ▼
                 ┌──────────────────────────┐
                 │   Spring Boot Backend    │
                 │      (Monolithic)        │
                 └─────────┬────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
 ┌─────────────┐   ┌──────────────┐   ┌──────────────┐
 │ Auth Module │   │ File Module  │   │ Payment Module│
 └─────────────┘   └──────────────┘   └──────────────┘
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ▼
                 ┌─────────────────┐
                 │    MongoDB      │
                 │ Metadata Storage│
                 └─────────────────┘
                           │
                           ▼
                 ┌─────────────────┐
                 │ Prometheus      │
                 │ Metrics Scraper │
                 └─────────────────┘
                           │
                           ▼
                 ┌─────────────────┐
                 │ Grafana         │
                 │ Dashboards      │
                 └─────────────────┘
```

---

# 🔄 File Upload Flow

```text
User Uploads File
        │
        ▼
Frontend Sends multipart/form-data Request
        │
        ▼
FileController
        │
        ▼
FileService
        │
        ├── Validate File
        ├── Check User Credits
        ├── Generate Metadata
        └── Store File
        │
        ▼
MongoDB Stores Metadata + Binary Data
        │
        ▼
Response Returned to Client
```

---

# 🔐 Authentication Flow

```text
User Logs In via Clerk
        │
        ▼
JWT Token Issued
        │
        ▼
Frontend Sends JWT in Authorization Header
        │
        ▼
ClerkJwtAuthFilter Intercepts Request
        │
        ├── Validate JWT Signature
        ├── Fetch Public Keys via JWKS
        └── Extract User Identity
        │
        ▼
Authenticated User Stored in SecurityContext
        │
        ▼
Protected Endpoint Access Granted
```

---

# 📊 Monitoring Architecture

```text
Spring Boot Application
        │
        ▼
Spring Boot Actuator
        │
        ▼
Prometheus Scrapes Metrics
        │
        ▼
Grafana Visualizes Metrics

Metrics Monitored:
- API Request Count
- JVM Memory Usage
- CPU Usage
- HTTP Response Time
- Active Sessions
- Application Health
```

---

# 🛠️ Tech Stack

| Category          | Technology            |
| ----------------- | --------------------- |
| Language          | Java 21               |
| Backend Framework | Spring Boot 3         |
| Database          | MongoDB               |
| Security          | Spring Security + JWT |
| Authentication    | Clerk OAuth           |
| Monitoring        | Prometheus + Grafana  |
| Build Tool        | Maven                 |
| Payment Gateway   | Razorpay              |
| Deployment        | Docker                |

---

# 📁 Project Structure

```text
cloudshare/
├── src/main/java/in/drive/cloudshare/
│   ├── Controller/
│   ├── Service/
│   ├── Repository/
│   ├── Security/
│   ├── Configuration/
│   ├── Document/
│   ├── Dto/
│   └── Exception/
│
├── src/main/resources/
│   └── application.properties
│
├── docker-compose.yml
├── Dockerfile
├── pom.xml
└── README.md
```

---

# 🗄️ Core Database Models

## FileMetadataDocument

```java
- id
- clerkId
- name
- size
- mimeType
- uploadedAt
- isPublic
- fileData
```

## UserCredits

```java
- clerkId
- credits
- totalCreditsAdded
- totalCreditsUsed
```

## PaymentTransaction

```java
- orderId
- paymentId
- amount
- status
- creditsAdded
```

---

# 🔌 Important API Endpoints

## File Upload

```http
POST /api/v1.0/files/upload
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

## Download File

```http
GET /api/v1.0/files/download/{fileId}
```

## Public File Access

```http
GET /api/v1.0/files/public/{fileId}
```

## Toggle Public/Private Access

```http
PATCH /api/v1.0/files/{fileId}/toggle-public
```

## User Credits

```http
GET /api/v1.0/users/credits
```

---

# 🐳 Docker Setup

## Build Docker Image

```bash
docker build -t cloudshare .
```

## Run Container

```bash
docker run -p 9090:9090 cloudshare
```

## Run Using Docker Compose

```bash
docker-compose up --build
```

---

# 📈 Prometheus & Grafana Setup

## Prometheus

Prometheus scrapes metrics exposed through Spring Boot Actuator.

Example metrics endpoint:

```http
/actuator/prometheus
```

## Grafana Dashboards

Dashboards visualize:

* API latency
* JVM memory usage
* CPU usage
* Request throughput
* Health metrics

---

# ⚙️ Configuration

## application.properties

```properties
server.port=9090
server.servlet.context-path=/api/v1.0

spring.servlet.multipart.max-file-size=5MB
spring.servlet.multipart.max-request-size=25MB

management.endpoints.web.exposure.include=*
management.endpoint.prometheus.enabled=true
management.prometheus.metrics.export.enabled=true
```

---

# 🔐 Security Design Decisions

* JWT-based stateless authentication was chosen to simplify scalable request validation.
* User-level isolation ensures users can only access their own private files.
* Public sharing is controlled through explicit visibility toggles.
* HMAC verification prevents payment tampering.

---

# 🧠 Engineering Concepts Demonstrated

* REST API design
* Multipart file handling
* Layered backend architecture
* JWT authentication flow
* Payment gateway integration
* Observability & monitoring
* Docker containerization
* Modular monolithic architecture

---

# 🚀 Future Improvements

* AWS S3 / MinIO integration
* Redis caching layer
* Signed URL-based secure sharing
* File chunking support
* Video streaming optimization
* Rate limiting
* CI/CD pipeline integration

---

# 📷 Screenshots

## Suggested Screenshots To Add

* Grafana Dashboard
* Upload Interface
* File Listing UI
* Architecture Diagram
* Docker Containers Running

---

# 📦 Running Locally

## Prerequisites

* Java 21+
* Maven
* MongoDB
* Docker

## Steps

```bash
# Clone Repository

git clone <repository-url>

# Build Application
./mvnw clean install

# Run Application
./mvnw spring-boot:run
```

Application runs on:

```text
http://localhost:9090/api/v1.0
```

---

# 📄 License

This project is built for educational and backend engineering learning purposes.

---

# 👨‍💻 Author
AGAMPAL SINGH.
