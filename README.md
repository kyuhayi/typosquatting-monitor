# Typosquatting Monitor

**Real-time Phishing & Brand Impersonation Detection 🛡️**

This project is a high-performance, event-driven security tool designed to detect typosquatted domains (e.g., `g00gle.com` instead of `google.com`) in real-time. By tapping into the global Certstream (SSL/TLS certificate heartbeat), it processes thousands of new domains per second using Apache Kafka and Java.

## 🚀 Key Features

*   **Real-time Ingestion:** Connects to Certstream WebSockets to capture every new SSL certificate issued globally.
*   **Stream Processing:** Uses Kafka Streams for low-latency similarity analysis.
*   **Fuzzy Matching:** Implements the Levenshtein Distance algorithm to calculate "edit distance" between target brands and new domains.
*   **Automated Alerts:** Triggers instant notifications via Slack Webhooks and archives threats to PostgreSQL.
*   **Rootless Deployment:** Built specifically for Podman, ensuring a secure, daemonless container environment.

## 🏗️ Architecture

The system follows a decoupled, reactive microservices pattern within a Gradle monorepo:

*   **Ingestor (Producer):** A Spring Boot app that acts as a bridge between WebSockets and Kafka.
*   **Kafka Cluster:** The central nervous system, buffering raw domain events.
*   **Analysis Engine (Processor):** A Kafka Streams app that performs the heavy lifting of string similarity calculations.
*   **Alerter (Consumer):** A final sink that handles notifications and persistent storage.

## 🛠️ Tech Stack

*   **Language:** Java 17+ (Spring Boot 3.x)
*   **Messaging:** Apache Kafka (KRaft mode)
*   **Stream Processing:** Kafka Streams API
*   **Library:** Apache Commons Text (Levenshtein)
*   **Containerization:** Podman & podman-compose
*   **Database:** PostgreSQL 15

## 📁 Project Structure (Monorepo)

```plaintext
.
├── common/              # Shared DTOs and Kafka configuration
├── producer-ingestor/   # Certstream WebSocket Client
├── streams-processor/   # Core analysis logic (The "Brain")
├── consumer-alerter/    # Slack alerts & Database persistence
├── Containerfile        # Multi-stage build for Podman
└── docker-compose.yaml  # Full stack orchestration
```

## 🚦 Getting Started

### 1. Prerequisites

Ensure you have Podman and `podman-compose` installed.

### 2. Build the Project

From the root directory, compile all modules:

```bash
./gradlew clean build
```

### 3. Spin up the Stack

Launch Kafka, the database, and the Java services:

```bash
podman-compose up -d --build
```

### 4. Monitor Logs

Watch the threat detection in real-time:

```bash
podman logs -f streams-processor
```

> **Note:** For optimal performance in a production environment, ensure the `raw-domains` topic has at least 3 partitions to allow the streams-processor to scale horizontally.