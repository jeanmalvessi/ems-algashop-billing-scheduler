# billing-scheduler

Short-lived scheduled microservice that performs background tasks for the billing service in the [AlgaShop](https://github.com/jeanmalvessi/ems-algashop-meta) platform.

Designed to run as a scheduled job, execute its tasks, and exit — rather than running as a long-lived HTTP server.

## Responsibilities

- Periodically queries the billing database for expired invoices
- Cancels expired invoices by calling the FastPay payment API
- Runs on a configurable schedule via Spring's task scheduling

## Architecture

Minimal architecture focused on task execution:

- **Application Layer:** `CancelExpiredInvoicesApplicationService` (interface + JDBC implementation)
- **Infrastructure Layer:** `CancelExpiredInvoicesRunner` (scheduled task), `FastpayPaymentAPIClient` (REST client), `AlgaShopPaymentProperties` (config binding)

Uses **Spring JDBC** instead of JPA for lightweight, direct database access.

## Tech Stack

- **Java 25**, Spring Boot 4.0.1
- **Spring JDBC** + PostgreSQL 17 (direct SQL queries, no ORM overhead)
- **Spring REST Client** (FastPay API for invoice cancellation)
- **Spring Web MVC** (without embedded Tomcat — lightweight setup)
- **Spring Validation**
- **Lombok**

## Running

```bash
./gradlew bootRun
```

Database: PostgreSQL `billing` database on `localhost:5432` (shared with the billing service)
Payment gateway: FastPay on `localhost:9995` (mock)
