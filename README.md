# Wall-E Project

Ce projet **Spring Boot** se connecte à plusieurs services (MinIO, PostgreSQL, Vault, Kafka, Schema Registry et AKHQ).  
Ce document explique comment démarrer et configurer correctement l’environnement de développement.

---

##  Prérequis

- **kubectl** configuré et connecté à votre cluster Kubernetes
- **Docker** et **Docker Compose**
- **Java 17+** (ou version requise par le projet)
- **Maven** ou **Gradle** (selon le build tool utilisé)

---

## ⚙️ Exposition des services Kubernetes

Avant de lancer l’application, exposez les services nécessaires en local avec `kubectl port-forward` :

```bash
# MinIO
kubectl -n ns-minio port-forward svc/minio 9000:9000 9001:9001

# PostgreSQL
kubectl -n ns-postgresql port-forward svc/postgresql 5432:5432

# Vault
kubectl -n ns-vault port-forward svc/vault 8200:8200
```

---

## Lancer les services via Docker Compose

Depuis la racine du projet, exécutez :

```bash
docker compose up -d
```

Cela démarre :

- **Kafka** (port `9092`)
- **Schema Registry** (port `8081`)
- **AKHQ** (UI d’administration Kafka)

---

## Configuration des certificats

L’application requiert des certificats.  
Ajoutez les variables suivantes lors du démarrage de l’application :

```bash
-DTRUST_STORE_LOCATION="$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-truststore.jks"
-DKEYSTORE_LOCATION="$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-keystore.jks"
-DTRUST_STORE_PASSWORD="cetelem"
-DKEYSTORE_PASSWORD="cetelem"
```

---

## Démarrage de l’application

Exécutez l’application Spring Boot (par exemple avec Maven) :

```bash
mvn spring-boot:run   -Dspring-boot.run.jvmArguments="   -DTRUST_STORE_LOCATION=$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-truststore.jks    -DKEYSTORE_LOCATION=$(pwd)/src/test/resources/certs/dev/AP26136-PF-benchmark-customer-springboot-dev-keystore.jks    -DTRUST_STORE_PASSWORD=cetelem    -DKEYSTORE_PASSWORD=cetelem"
```

---

##  Accès aux services

- **Application Wall-E** → [http://localhost:8082](http://localhost:8082)
- **Schema Registry** → [http://localhost:8081](http://localhost:8081)
- **Kafka Broker** → `localhost:9092`
- **MinIO Console** → [http://localhost:9001](http://localhost:9001)
- **Vault** → [http://localhost:8200](http://localhost:8200)
- **AKHQ** → (vérifiez le port exposé dans `docker-compose.yml`)

---

##  Structure utile du projet

```
src/
 └── test/
      └── resources/
           └── certs/
                └── dev/
                     ├── AP26136-PF-benchmark-customer-springboot-dev-truststore.jks
                     └── AP26136-PF-benchmark-customer-springboot-dev-keystore.jks
```
