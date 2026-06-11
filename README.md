# Velo Backend (TeamZeus)

**Velo** is a smart event engagement platform that turns passive event attendance into an interactive, personalised experience.

### Key Features
- **For Attendees:** Discover and register for events (similar to Luma or Tix), then participate live through polls, Q&A, quizzes, reactions, and gamified activities.
- **For Organisers:** Create and run events, manage live engagement, and access real-time and post-event analytics to measure success and improve future events.

This repository contains the containerized Spring Boot backend service for **Velo**, built with Java 21 and Maven.

---

## 🛠️ Tech Stack & Requirements

- **Java Development Kit (JDK):** Version 21
- **Framework:** Spring Boot 4.0.6
- **Build Tool:** Maven (wrapper included)
- **Database:** PostgreSQL (running in Docker)
- **Cache / Key-Value Store:** Redis (running in Docker)
- **Containerization:** Docker & Docker Compose

---

## Project Structure

```text
teamzeu-backend/
├── .env                  # Local environment overrides (git-ignored)
├── .env.example          # Sample environment template
├── docker-compose.yml    # Docker services config (PostgreSQL & Redis)
├── pom.xml               # Maven dependencies and build configuration
├── mvnw / mvnw.cmd       # Maven wrapper scripts
└── src/
    ├── main/
    │   ├── java/         # Application source code
    │   │   └── com/teamzeu/velo/VeloApplication.java   # Spring Boot Main class
    │   └── resources/
    │       └── application.yml   # Spring Boot application configuration
    └── test/
        └── java/         # JUnit unit and integration tests
```

---

## Environment Variables

The application utilizes a `.env` file at the root of the project to manage secrets and environment-specific variables. Spring Boot imports these properties automatically using the configuration below in [application.yml](file:///Users/uniway/IdeaProjects/teamzeu-backend/src/main/resources/application.yml):
```yaml
spring:
  config:
    import: "optional:file:.env[.properties]"
```

### Configuration Options

| Variable | Example Value | Description |
| :--- | :--- | :--- |
| `DB_HOST` | `localhost` | Host address of the PostgreSQL database |
| `DB_PORT` | `5430` | Port of the PostgreSQL database |
| `DB_USER` | `your_db_user` | PostgreSQL username |
| `DB_PASSWORD` | `your_db_password` | PostgreSQL password |
| `DB_NAME` | `your_db_name` | Name of the PostgreSQL database |
| `REDIS_HOST` | `localhost` | Host address of the Redis server |
| `REDIS_PORT` | `6370` | Port of the Redis server |

---

## Getting Started

Follow these steps to run the application locally:

### 1. Configure the Environment
Copy the example environment file to create your local configurations:
```bash
cp .env.example .env
```
*(Optional)* Adjust variables inside `.env` if you want to use custom database credentials.

### 2. Start External Services
Spin up the PostgreSQL database and Redis store using Docker Compose:
```bash
docker compose up -d
```
This launches:
- **PostgreSQL** on port `5430` (mapped from `5432` internally)
- **Redis** on port `6370` (mapped from `6379` internally)

### 3. Run the Spring Boot App
Start the development server using the Maven wrapper:
```bash
./mvnw spring-boot:run
```
The server will boot up and automatically connect to your local PostgreSQL instance using the credentials defined in `.env`.

### 4. Run Tests
Verify everything is working correctly by executing the test suite:
```bash
./mvnw test
```

---

## Docker Services Reference

The services are defined in [docker-compose.yml](file:///Users/uniway/IdeaProjects/teamzeu-backend/docker-compose.yml):

- **PostgreSQL:**
  - Container Name: `velo-postgres`
  - Host Port: `5430`
  - Database volumes are persisted in a Docker volume named `postgres-data`.
- **Redis:**
  - Container Name: `velo-redis`
  - Host Port: `6370`
  - Redis data is persisted in a Docker volume named `redis-data`.