# 🚀 BotLedger: Event-Driven WhatsApp Billing Engine

### 📸 LOGO
![BotLedger logo](screenshots/sign-in.png)

BotLedger is an enterprise-grade backend microservice built to bridge the Meta WhatsApp Cloud API with the Paystack payment gateway. 

It acts as an event-driven fulfillment broker that guarantees exactly-once transaction processing (zero double-billing) and handles heavy background tasks asynchronously (like PDF invoice generation) using a message broker.

## 🏗 Tech Stack

*   **Language:** Java 21 (LTS)
*   **Framework:** Spring Boot 3.2+
*   **Database:** PostgreSQL 15
*   **Message Broker:** RabbitMQ
*   **Database Migrations:** Flyway
*   **Document Generation:** Apache PDFBox
*   **Infrastructure:** Docker & Docker Compose

## 🧠 Core Architecture Problems Solved

1.  **Network Unreliability & Double-Billing:** Implements strict atomic database locks (Idempotency) via PostgreSQL `INSERT ... ON CONFLICT` to secure payment webhooks.
2.  **CPU Bottlenecks:** Offloads CPU-intensive tasks (like generating PDF invoices or certificates) to a decoupled RabbitMQ asynchronous worker, keeping the main API thread lightning fast.

## 🚦 Getting Started (Local Development)

### Prerequisites
*   [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or Docker Engine + Docker Compose)
*   Java 21 JDK
*   Maven 3.8+

### 1. Spin up the Infrastructure
The project relies on PostgreSQL and RabbitMQ. Start them locally using the provided Docker Compose file:

```bash
docker-compose up -d
```

*   **PostgreSQL:** Runs on `localhost:5432`
*   **RabbitMQ AMQP:** Runs on `localhost:5672`
*   **RabbitMQ Management UI:** Accessible at `http://localhost:15672` (Login: `botledger` / `secretpassword`)

### 2. Run the Application
Start the Spring Boot application. Flyway will automatically detect the empty database and run the initial schema migrations on startup.

```bash
./mvnw spring-boot:run
```

### 3. Verify Database Creation
Once the app is running, you can connect to PostgreSQL using DBeaver, pgAdmin, or the CLI to verify the tables were created:
*   **DB URL:** `jdbc:postgresql://localhost:5432/botledger_db`
*   **Username:** `botledger`
*   **Password:** `secretpassword`

You should see the `orders`, `processed_webhooks`, and `admin_profiles` tables successfully migrated.

## 🗺️ Roadmap & Build Order

- [x] **Step 1:** Skeleton, Infrastructure, and Flyway Migrations.
- [ ] **Step 2:** Orders Domain (Entities and Status Flow).
- [ ] **Step 3:** Paystack Webhook Handler (HMAC-SHA512 Security & Idempotency).
- [ ] **Step 4:** RabbitMQ Queue and Async PDFBox Worker.
- [ ] **Step 5:** Secured Admin API (JWT & Pagination).
- [ ] **Step 6:** Meta WhatsApp Webhook & Outbound Client.
