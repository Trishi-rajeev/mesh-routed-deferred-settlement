# UPI Offline Mesh-routed-deferred-settlement 🚀

> A secure offline UPI payment simulation using Bluetooth-style mesh networking, hybrid cryptography, and idempotent settlement.

## Overview

**UPI Offline Mesh** demonstrates how digital payments could be routed through nearby devices when internet connectivity is unavailable.

Imagine being in a basement, remote village, disaster zone, or underground transit system with no network access. A sender creates a payment request, which is encrypted and propagated through nearby devices using a mesh-style gossip protocol. Once any bridge device regains internet access, it uploads the packet to the backend where it is validated, decrypted, and settled exactly once.

This project showcases secure distributed systems concepts including:

* Offline payment routing
* Mesh networking simulation
* Hybrid RSA + AES encryption
* Tamper detection
* Replay protection
* Idempotent transaction processing
* Concurrent duplicate handling
* Transaction settlement

---

## Key Features

### Secure End-to-End Encryption

* RSA-2048 OAEP key wrapping
* AES-256-GCM authenticated encryption
* Tamper-proof transaction packets
* Only the backend can decrypt payment data

### Offline Mesh Propagation

* Simulated Bluetooth-style device-to-device packet forwarding
* TTL-based packet lifecycle
* Gossip protocol distribution

### Idempotent Settlement

* Duplicate packets are automatically detected
* Atomic hash claiming prevents double processing
* Concurrent bridge uploads settle exactly once

### Replay Attack Protection

* Timestamp freshness validation
* Unique transaction nonce
* Expired packets are rejected

### Interactive Dashboard

* Create payments
* Simulate gossip rounds
* Upload through bridge nodes
* Visualize balances and transaction history

---

## Tech Stack

| Layer       | Technology             |
| ----------- | ---------------------- |
| Backend     | Spring Boot 3          |
| Language    | Java 17                |
| Database    | H2 In-Memory Database  |
| Persistence | Spring Data JPA        |
| Security    | RSA-OAEP, AES-256-GCM  |
| Build Tool  | Maven                  |
| Testing     | JUnit 5                |
| Concurrency | Java ConcurrentHashMap |
| Frontend    | Thymeleaf + HTML       |

---

## System Architecture

```text
Sender Device (Offline)
        │
        ▼
Encrypt Payment
(RSA + AES-GCM)
        │
        ▼
Mesh Packet
        │
        ▼
Nearby Devices
(Gossip Protocol)
        │
        ▼
Bridge Device
(Regains Internet)
        │
        ▼
POST /api/bridge/ingest
        │
        ▼
Hash → Deduplicate → Decrypt
        │
        ▼
Freshness Validation
        │
        ▼
Transaction Settlement
        │
        ▼
Ledger Update
```

---

## Project Structure

```text
upi-offline-mesh/
│
├── src/main/java/com/demo/upimesh/
│   ├── config/
│   ├── controller/
│   ├── crypto/
│   ├── model/
│   ├── service/
│   └── UpiMeshApplication.java
│
├── src/main/resources/
│   ├── application.properties
│   └── templates/dashboard.html
│
├── src/test/
│   └── IdempotencyConcurrencyTest.java
│
├── pom.xml
└── README.md
```

---

## Running the Application

### Prerequisites

* Java 17+
* Git

Verify installation:

```bash
java -version
```

### Clone Repository

```bash
git clone https://github.com/your-username/upi-offline-mesh.git
cd upi-offline-mesh
```

### Windows

```bash
.\mvnw.cmd spring-boot:run
```

### Linux / macOS

```bash
./mvnw spring-boot:run
```

---

## Access Dashboard

After startup:

```text
http://localhost:8080
```

The dashboard allows you to:

* Create offline payments
* Simulate mesh propagation
* Upload through bridge nodes
* View balances
* Inspect transaction history

---

## Demo Workflow

### 1. Create Payment

Generate a payment packet by selecting:

* Sender
* Receiver
* Amount
* PIN

The backend encrypts the payment and injects it into the virtual mesh.

---

### 2. Run Gossip

Simulate Bluetooth packet propagation between devices.

Each device forwards packets to nearby devices until TTL expires.

---

### 3. Bridge Upload

Bridge devices with internet connectivity upload packets to:

```http
POST /api/bridge/ingest
```

---

### 4. Settlement

Backend processing pipeline:

```text
Receive Packet
      │
      ▼
SHA-256 Hash
      │
      ▼
Idempotency Check
      │
      ▼
Decrypt Payload
      │
      ▼
Freshness Validation
      │
      ▼
Debit/Credit Accounts
      │
      ▼
Write Ledger Entry
```

---

## API Endpoints

| Method | Endpoint           | Description                |
| ------ | ------------------ | -------------------------- |
| GET    | /                  | Dashboard                  |
| GET    | /api/accounts      | Account balances           |
| GET    | /api/transactions  | Transaction ledger         |
| GET    | /api/mesh/state    | Device states              |
| POST   | /api/demo/send     | Create payment             |
| POST   | /api/mesh/gossip   | Run gossip round           |
| POST   | /api/mesh/flush    | Upload bridge packets      |
| POST   | /api/mesh/reset    | Reset mesh                 |
| POST   | /api/bridge/ingest | Production ingest endpoint |
| GET    | /h2-console        | Database console           |

---

## Security Design

### Hybrid Encryption

Each payment:

1. Generates a random AES-256 key
2. Encrypts payment JSON using AES-GCM
3. Encrypts AES key using RSA-OAEP
4. Combines encrypted key + IV + ciphertext

Benefits:

* Fast encryption
* Confidentiality
* Integrity verification
* Tamper resistance

---

### Idempotency Protection

Duplicate uploads are prevented using:

```java
seen.putIfAbsent(packetHash, timestamp);
```

Only the first bridge node can claim a packet hash.

All subsequent duplicates are discarded.

---

### Replay Protection

Every payment contains:

* Timestamp
* Unique nonce

The backend rejects:

* Expired packets
* Previously processed packets

---

## Testing

Run all tests:

```bash
.\mvnw.cmd test
```

or

```bash
./mvnw test
```

### Included Tests

* Encrypt/Decrypt Round Trip
* Tampered Ciphertext Rejection
* Concurrent Duplicate Delivery
* Idempotent Settlement Verification

---

## Production Improvements

The current implementation is a demonstration system.

Potential upgrades:

| Demo Version       | Production Version        |
| ------------------ | ------------------------- |
| H2 Database        | PostgreSQL                |
| ConcurrentHashMap  | Redis SETNX               |
| Simulated Mesh     | BLE / Wi-Fi Direct        |
| Generated RSA Keys | HSM / AWS KMS             |
| No Authentication  | mTLS                      |
| Demo Accounts      | Real Banking Integration  |
| Console Logging    | Centralized Observability |

---

## Limitations

This project intentionally focuses on cryptography and settlement mechanics.

Current limitations include:

* No offline balance verification
* Possible offline double-spend attempts
* Simulated mesh networking
* No real NPCI integration
* No mobile client implementation

For this reason, the system should be described as:

> **"Mesh-Routed Deferred Settlement for Offline Payments"**

rather than a fully functional real-time offline UPI solution.

---

## Learning Outcomes

This project demonstrates practical implementation of:

* Distributed Systems
* Cryptography
* Concurrent Programming
* Transaction Processing
* Idempotent APIs
* Secure Backend Design
* Spring Boot Architecture
* Eventual Consistency Concepts

---

## Author

**Trishi Rajeev**

Computer Science Engineer | Java Developer | Backend & Distributed Systems Enthusiast

---

## License

This project is intended for educational, research, and portfolio purposes.

Feel free to use, modify, and learn from it.
