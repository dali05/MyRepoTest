# Wall-E Project

This **Spring Boot** project connects to multiple services (MinIO, PostgreSQL, Vault, Kafka, Schema Registry, and AKHQ).  
This document explains how to start and properly configure the development environment.

---

## 🚀 Prerequisites

- **kubectl** configured and connected to your Kubernetes cluster
- **Docker** and **Docker Compose**
- **Java 17+** (or required version for the project)
- **Maven** or **Gradle** (depending on the build tool used)

---

## ⚙️ Exposing Kubernetes Services

Before running the application, expose the required services locally with `kubectl port-forward`:

```bash
# MinIO
kubectl -n ns-minio port-forward svc/minio 9000:9000 9001:9001

# PostgreSQL
kubectl -n ns-postgresql port-forward svc/postgresql 5432:5432

# Vault
kubectl -n ns-vault port-forward svc/vault 8200:8200
```

---

## 🐳 Launching Services via Docker Compose

From the project root, run:

```bash
docker compose up -d
```

This starts:

- **Kafka** (port `9092`)
- **Schema Registry** (port `8081`)
- **AKHQ** (Kafka administration UI)

---

## 🔐 Certificate Configuration

The application requires certificates.  
Add the following variables when starting the application:

```bash
-DTRUST_STORE_LOCATION="$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-truststore.jks"
-DKEYSTORE_LOCATION="$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-keystore.jks"
-DTRUST_STORE_PASSWORD="cetelem"
-DKEYSTORE_PASSWORD="cetelem"
```

---

## ▶️ Running the Application

Run the Spring Boot application (example with Maven):

```bash
mvn spring-boot:run   -Dspring-boot.run.jvmArguments="   -DTRUST_STORE_LOCATION=$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-truststore.jks    -DKEYSTORE_LOCATION=$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-keystore.jks    -DTRUST_STORE_PASSWORD=cetelem    -DKEYSTORE_PASSWORD=cetelem"
```

---

## 🌐 Service Endpoints

- **Wall-E Application** → [http://localhost:8082](http://localhost:8082)
- **Schema Registry** → [http://localhost:8081](http://localhost:8081)
- **Kafka Broker** → `localhost:9092`
- **MinIO Console** → [http://localhost:9001](http://localhost:9001)
- **Vault** → [http://localhost:8200](http://localhost:8200)
- **AKHQ** → (check the exposed port in `docker-compose.yml`)

---

## 📂 Project Structure

```
src/
 └── test/
      └── resources/
           └── certs/
                └── dev/
                     ├── AP26136-PF-benchmark-customer-springboot-dev-truststore.jks
                     └── AP26136-PF-benchmark-customer-springboot-dev-keystore.jks
```

---

## ✅ Notes

- Ensure all `port-forward` commands stay active while the application is running.
- Double-check that certificates and passwords are correct.
- Use `docker compose logs -f` to debug local services.

---

👨‍💻 Developed for the **Wall-E Spring Boot** environment.
