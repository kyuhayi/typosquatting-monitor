# Project Context: Typosquatting Monitor

## 1. Project Overview
- **Goal:** Real-time phishing domain detection using Certstream and Kafka.
- **Tech Stack:** Java 17, Spring Boot, Kafka Streams, Podman.
- **Architecture:** Monorepo with 4 modules (common, ingestor, processor, alerter).

## 2. Agent Rules & Constraints
- **Language:** Always use Java 17+ features.
- **Containerization:** Use Podman (not Docker). Use `Containerfile` naming.
- **Style:** Follow Google Java Style Guide. Use Lombok for DTOs.
- **Kafka:** Use KRaft mode (no Zookeeper if possible).

## 3. Module Responsibilities
- `:common`: Shared POJOs and Kafka constants.
- `:producer-ingestor`: Connects to `wss://certstream.calidog.io`.
- `:streams-processor`: Business logic using `LevenshteinDistance`.
- `:consumer-alerter`: PostgreSQL persistence and Slack API integration.

## 4. Key Workflows
- Build: `./gradlew clean build`
- Run: `podman-compose up -d`
- Test: Focus on `streams-processor` unit tests for similarity logic.