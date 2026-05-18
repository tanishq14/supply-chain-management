# Enterprise Microservices-Based Supply Chain Management System

## Architectural Overview
This repository serves as the central orchestration and parent shell for a distributed, microservices-driven **Supply Chain Management System**. The application is architected to isolate core domains—including decentralized identity services, real-time inventory tracking, multi-state order pipelines, vendor logistics, and data telemetry processing—into decoupled, autonomous services.

The platform is built with a **Database-per-Service** design pattern to eliminate single points of failure, ensure strict database transactional boundaries, and allow independent scalability of individual components.

* **API Edge Routing:** Requests from the user client are processed and safely reversed-proxied via a unified API Gateway layer (`main`).
* **Stateless Identity Governance:** Centralized Role-Based Access Control (RBAC) and security handshake filters utilizing stateless JSON Web Tokens (JWT).
* **Decoupled Relational Boundaries:** Modular microservice storage zones leveraging dedicated relational schemas (`user_db`, `inventory_db`, `order_db`) to guarantee strict domain bounds.

---

## Distributed Project Ecosystem
This project leverages Git submodules to maintain atomic version control across independent codebase repositories:

1. **API Gateway Service Container (`main`)**
   * Central ingress point for traffic management, cross-origin request policies, and upstream routing mapping.
2. **User Authentication & Access Control Service (`user-authentication-access-control-service`)**
   * Identity provider responsible for secure user onboarding, password hashing, credential validation, and cryptographic token issuance.
3. **Inventory Management Service (`inventory-management-service`)**
   * Oversees storage levels, multi-warehouse component quantities, SKU catalogs, and stock mutation limits.
4. **Order Management Service (`order-management-service`)**
   * Manages transaction records, order generation, invoice state transitions, and fulfillment pipeline status.
5. **Supplier Management Service (`supplier-management-service`)**
   * Handles supplier catalogs, wholesale procurement records, vendor onboarding pipelines, and performance telemetry.
6. **Basic Data Insights Service (`basic-data-insights-service`)**
   * System telemetry engine processing platform events to produce analytical aggregations, key performance indicators, and reporting metrics.
7. **Client System Dashboard Interface (`supply-chain-management-system`)**
   * A modern React web dashboard built on Vite to provide administrative insight across the distributed network topology.

---

## Technical Prerequisites
Ensure your local development environment has the following software provisions available:
* **Java Development Kit (JDK) 17 or higher**
* **Node.js (v16.0.0+)** & npm package runner
* **MySQL Server** active and bound to local port `3306`

---

## Local Environment Provisioning

### 1. Unified Repository Initialization
To clone the entire ecosystem alongside its nested submodule directories in a single command, run:
```bash
git clone --recurse-submodules https://github.com/tanishq14/supply-chain-management.git
cd supply-chain-management
```

### 2. Relational Schema Provisioning
Establish the separate database backends inside your local MySQL instance to satisfy data decoupling isolation requirements:
```SQL
CREATE DATABASE user_db;
CREATE DATABASE inventory_db;
CREATE DATABASE order_db;
```

Update your explicit credentials (username and password) inside the respective src/main/resources/application.properties configuration files inside each microservice workspace folder:

```properties
# Example Database Configuration mapping (Adjust per service schema)
spring.datasource.url=jdbc:mysql://localhost:3306/user_db
spring.datasource.username=YOUR_LOCAL_DB_USER
spring.datasource.password=YOUR_LOCAL_DB_PASSWORD
```

### 3. Bootstrapping the Distributed Core
Open individual terminal sessions for each Java component module, navigate to its path, and execute the embedded Maven boot binary:
```bash
# Session 1: Secure Edge Gateway
cd main
./mvnw spring-boot:run

# Session 2: Identity Layer
cd ../user-authentication-access-control-service
./mvnw spring-boot:run

# Session 3: Core Inventory Control
cd ../inventory-management-service
./mvnw spring-boot:run

# Session 4: Order Lifecycles
cd ../order-management-service
./mvnw spring-boot:run

# Session 5: Vendor Ledger
cd ../supplier-management-service
./mvnw spring-boot:run

# Session 6: Analytics Engine
cd ../basic-data-insights-service
./mvnw spring-boot:run
```

## Frontend Prerequisites and Implementation
### 1. Compile Client Dependencies
Navigate to the frontend container folder and compile the node module dependencies:

```bash
cd supply-chain-management-system
npm install
```

### 2. Boot Local Web Server
Instantiate the local client development runtime server:

```bash
npm run dev
```

The interface layer will bind locally to your browser target at `http://localhost:5173`.
