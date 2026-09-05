# VeriTrace Platform
> **Enterprise Multi-Tenant Supply Chain Management & GS1-Compliant Traceability Platform Powered by Real-Time Telemetry and Decentralized Verification.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Go Version](https://img.shields.io/badge/Go-1.22+-00ADD8?logo=go)](https://golang.org)
[![Next.js Version](https://img.shields.io/badge/Next.js-14_App_Router-black?logo=next.js)](https://nextjs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16_RLS-316192?logo=postgresql)](https://www.postgresql.org)
[![Kafka](https://img.shields.io/badge/Apache_Kafka-KRaft_Mode-231F20?logo=apachekafka)](https://kafka.apache.org)
[![Polygon](https://img.shields.io/badge/Polygon-Amoy_Testnet-8247E5?logo=polygon)](https://polygon.technology)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)](https://www.docker.com)

---

## 📌 Overview

**VeriTrace** is an enterprise multi-tenant supply chain execution and traceability platform designed to eliminate data silos, safeguard cold-chain integrity, and ensure end-to-end provenance across untrusted logistics stakeholders.

The platform bridges physical logistics operations (WMS/TMS) with cryptographic data integrity through three technical pillars:
1. **Standardized Identity & Strict Isolation:** Full compliance with global GS1 EPCIS 2.0 standards (GLN, GTIN, SSCC), protected by dynamic Attribute-Based Access Control (ABAC) and database-level PostgreSQL Row-Level Security (RLS).
2. **High-Throughput Telemetry Ingestion:** Real-time cold chain monitoring via Mosquitto MQTT, streamed through Apache Kafka (KRaft mode) into TimescaleDB, with sub-second WebSocket breach dispatching.
3. **Cryptographic Integrity & Gasless Verification:** Client-side Envelope Encryption (AES-256-GCM) with IPFS storage for confidential compliance documents, anchored to a Polygon Layer-2 network using Merkle Tree state batching and an automated Go Relayer engine.

---

## 🏛️ System Architecture

```
[ CLIENT LAYER ]
  ├── enterprise-dashboard       (Next.js 14 Web WMS/TMS Management)
  ├── driver-mobile-pwa          (Next.js PWA - Hardware Barcode/QR Scanning)
  └── public-trace-portal        (Next.js 14 ISR - Edge-Cached Public Audit)
                                 │
                                 ▼ (HTTPS / WSS)
[ API GATEWAY & SECURITY LAYER ]
  └── Nginx Reverse Proxy (SSL Termination + JWT/ABAC Context Injection)
                                 │
                                 ▼
[ GOLANG MICROSERVICES ]
  ├── core-business-service      ──► PostgreSQL 16 (Tenant RLS Partitioning)
  ├── telemetry-stream-service   ──► Mosquitto MQTT ──► Kafka KRaft ──► TimescaleDB
  └── blockchain-relayer-service ─► Redis 7 Queue ──► Master Relayer Signer
                                 │
                 ┌───────────────┴───────────────┐
                 ▼ (Decentralized Storage)       ▼ (Telemetry Ingestion)
     [ Pinata IPFS Cluster ]             [ Python IoT Simulator Engine ]
     (AES-256 Encrypted Documents)       (MQTT Stream & Anomaly Triggering)
                 │
                 ▼ (On-Chain Settlement)
     [ Polygon Layer-2 Network ]
     (SupplyChainTraceability.sol - Merkle State Commitment Store)
```

---

## ⚙️ Core Technical Capabilities

* **GS1 EPCIS 2.0 Compliance:** Native validation and assignment of Global Location Numbers (GLN-13), Global Trade Item Numbers (GTIN-14), and Serial Shipping Container Codes (SSCC-18) using automated Modulo 10 check-digit algorithms.
* **Database-Enforced Multi-Tenancy:** Multi-organization data isolation achieved via PostgreSQL Row-Level Security (RLS) policies driven by runtime JWT session variables (`app.current_tenant_id`).
* **Real-Time Cold Chain Pipeline:** Scalable ingestion architecture built on Mosquitto MQTT Broker and Apache Kafka, storing high-frequency sensor readings in TimescaleDB hypertables while detecting temperature breaches within 30 seconds.
* **Confidential Decentralized Storage:** Envelope Encryption scheme combining AES-256-GCM symmetric encryption with IPFS content-addressed storage to guarantee commercial secrecy for compliance certificates (CO/CQ).
* **Gasless Web3 Relayer Engine:** Decouples enterprise users from crypto wallet management. An asynchronous Go worker handles Redis queues, locks atomic transaction nonces, batches Merkle roots, and sponsors gas fees on Polygon L2.
* **Anti-Counterfeiting Public Portal:** GS1 Digital Link QR resolution integrated with cryptographic HMAC signatures and geo-frequency anomaly detection to prevent QR duplication and physical label tampering.
* **Unified Observability:** Full-stack operational visibility combining Prometheus performance metrics and Grafana Loki structured log streams into a single dashboard.

---

## 📦 Repository Ecosystem

The platform is partitioned into autonomous repositories organized by architectural domain:

| Repository | Layer | Core Tech Stack | Primary Responsibilities |
| :--- | :--- | :--- | :--- |
| [`core-business-service`](https://github.com/veritrace-platform/core-business-service) | Core Backend | Go, Gin, GORM, PostgreSQL 16 | Tenant lifecycle, ABAC authorization, GS1 catalog (GLN, GTIN, SSCC), shipment state machine, emergency recall orchestration. |
| [`telemetry-stream-service`](https://github.com/veritrace-platform/telemetry-stream-service) | Telemetry Backend | Go, Mosquitto, Kafka KRaft, TimescaleDB | MQTT telemetry ingestion, Kafka stream processing, TimescaleDB hypertable persistence, WebSocket broadcast hub. |
| [`blockchain-relayer-service`](https://github.com/veritrace-platform/blockchain-relayer-service) | Settlement Backend | Go, `go-ethereum`, Redis 7 | Off-chain Merkle root computation, Redis transaction queue management, nonce synchronization, Polygon L2 gasless execution. |
| [`smart-contracts`](https://github.com/veritrace-platform/smart-contracts) | Blockchain | Solidity 0.8.20, Foundry, OpenZeppelin | `SupplyChainTraceability.sol` state commitment contract, role-based relayer access controls, automated test suites. |
| [`enterprise-dashboard`](https://github.com/veritrace-platform/enterprise-dashboard) | Web Application | Next.js 14, TypeScript, Tailwind, Shadcn | Enterprise WMS/TMS desktop interface, dynamic GS1 barcode generation, live telemetry charts, emergency recall triggers. |
| [`driver-mobile-pwa`](https://github.com/veritrace-platform/driver-mobile-pwa) | Edge / Mobile | Next.js 14 PWA, Web Camera APIs | Driver touch-first mobile PWA, hardware-accelerated barcode scanning, offline-capable 3-way handover protocols with dynamic OTP. |
| [`public-trace-portal`](https://github.com/veritrace-platform/public-trace-portal) | Public Web | Next.js 14 ISR, Edge Runtime, ECharts | Public consumer provenance resolver, GS1 Digital Link validation, HMAC anti-tampering verification, on-chain proof exploration. |
| [`platform-infrastructure`](https://github.com/veritrace-platform/platform-infrastructure) | Infrastructure | Docker Compose, Nginx, Prometheus, Loki | Multi-container local orchestration, SQL migration baselines, Nginx routing configs, simulated IoT fleet generator. |

---

## 📄 License
This project is licensed under the [MIT License](LICENSE).
