# Error Logging Service Architecture

## Major Components of the Architecture

### 1. Client SDK
A lightweight TypeScript SDK will be provided for integration with third-party applications.
- **TypeScript + ESLint** allows for many errors to be caught early in development.
- Runtime errors (`try/catch`, `window.onerror`, `unhandledrejection`) are intercepted by the SDK and forwarded to the backend securely.
- Logs can be enriched with additional metadata (application version, user, environment).

---

### 2. API Backend
The backend will be built using **NestJS (Node.js, TypeScript)**.
- NestJS provides good modularity and built-in validation features.
- The API is responsible for receiving the logs, processing them, and storing them.
- **Rate limiting** is implemented to prevent abuse.
- Supports **REST** and **WebSocket** for real-time delivery of the logs.

---

### 3. Data Storage
A hybrid storage plan is recommended:
- **PostgreSQL** → structured data (users, projects, metadata).
- **AWS S3** → archival storage with retention policies (e.g., 30 days hot storage, then archive).

---

### 4. Analytics & Dashboard
The web dashboard will be constructed using **Next.js (React, TypeScript)**.
- Provides a fast and responsive UI to developers.
- Supports filtering, searching, and browsing logs.
- **Grafana** is used for visualization of error trends and performance metrics.
- **AWS CloudWatch** provides system-level monitoring (CPU, memory, log metrics).

---

### 5. Alerts & Notifications
- **AWS SNS/SES** for email alerts and Slack.
- Configurable alerting rules, e.g., *"trigger an alert if more than 100 errors per minute"*.  
- Also triggered at **Docker-level monitoring**, with logs retained between restarts until they are reviewed.

---

### 6. DevOps & Reliability
The infrastructure will be on **AWS**, with the following tools and practices:
- **CloudWatch** → monitoring service & log aggregation.
- **Grafana** → centralized metric and alert visualization.
- **CI/CD** → automated deploys using GitHub Actions or Bitbucket Pipelines.
- **Docker monitoring** → container logs retained until approved for not losing critical errors.

**cron jobs** may also be configured to send automatic periodic reports (e.g., daily or weekly summary) at set times.

---

## Code Quality & Error Prevention
- **TypeScript + ESLint** automatically catch a lot of common errors.
- NestJS and Next.js built-in validation and exception handling.
- Global **try/catch** blocking in main services.
- Automated testing with **Jest** (unit and integration tests).

---

## Questions to the Client
Before implementation, the following is required:

1. How much volume of logs should be desired?
2. What is the preferable retention period (30, 90, 365 days)?
3. Will the system be integrated with external services (Slack, Jira)?
4. Are there any budget restrictions on AWS or are there other alternatives to the clouds (GCP, Azure)?

 

 

⚠️ This is **one viable solution** to error logging. The actual implementation may vary depending on the client's infrastructure, budget, and project requirements.
