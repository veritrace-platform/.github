# VeriTrace Platform

> **Multi-tenant supply chain execution and GS1 traceability, with real-time cold-chain monitoring and
> blockchain-anchored verification.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Go](https://img.shields.io/badge/Go-1.27-00ADD8?logo=go)](https://go.dev)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-18_+_TimescaleDB-316192?logo=postgresql)](https://www.postgresql.org)
[![Kafka](https://img.shields.io/badge/Apache_Kafka-KRaft-231F20?logo=apachekafka)](https://kafka.apache.org)
[![Polygon](https://img.shields.io/badge/Polygon-Amoy-8247E5?logo=polygon)](https://polygon.technology)

---

> 👉 **Start here:** [`veritrace`](https://github.com/veritrace-platform/veritrace) is the project home, with
> documentation, roadmap, architecture decisions, and a one-command workspace setup for all repositories.

## Overview

VeriTrace gives companies that do not fully trust each other one shared, tamper-evident record of the
goods they move. Those companies include brand owners, carriers, distributors, and retailers.

- **GS1 identification:** locations (GLN), products (GTIN), lots, and logistic units (SSCC) are validated
  with Modulo 10 check digits and bound to each company's GS1 prefix.
- **Multi-party isolation:** every company's data is isolated by PostgreSQL row-level security. Shipments
  are shared only with their participants: the owner, the carrier, and the consignee.
- **Custody handover:** the origin issues a one-time pickup code; the driver scans the SSCC inside the
  origin's geo-fence and enters the code; the destination confirms delivery inside its own geo-fence.
- **Emergency recall:** one action locks a lot across every company that holds it and alerts all of them
  in real time.
- **Cold-chain monitoring:** sensor readings travel MQTT → Kafka → TimescaleDB. Sustained excursions of
  30 seconds or more raise incidents that reach dashboards and drivers within a second.
- **Tamper evidence:** every shipment event is canonicalized, hashed, and chained. Batches of hashes are
  committed as Merkle roots on Polygon by a gasless relayer, so users never touch a wallet.
- **Public verification:** signed GS1 Digital Link labels let consumers verify provenance, cold-chain
  history, and on-chain proofs, and let the platform detect cloned labels.

## Architecture

```
 enterprise-dashboard   driver-mobile-pwa   public-trace-portal
            └───────────────┬──────────────────────┘
                     Gateway (REST + WebSocket)
        ┌──────────────────┼────────────────────────┐
 core-business-service  telemetry-stream-service  blockchain-relayer-service
   PostgreSQL (RLS)       TimescaleDB · MQTT         Redis · Polygon · IPFS
        └──────────── Kafka (shipment.events, telemetry.incidents) ───────┘
```

## Repositories

| Repository | Purpose | Stack |
| --- | --- | --- |
| [`veritrace`](https://github.com/veritrace-platform/veritrace) | **Project home**: documentation, roadmap, decisions, workspace tooling | Markdown, Bash, Python |
| [`platform-infrastructure`](https://github.com/veritrace-platform/platform-infrastructure) | Local environment, bootstrap, gateway, IoT simulator | Docker Compose, Caddy, Python |
| [`core-business-service`](https://github.com/veritrace-platform/core-business-service) | Tenants, identity, GS1 catalog, lots, inventory, shipments, handover, recall, document vault, public trace API | Go, PostgreSQL |
| [`telemetry-stream-service`](https://github.com/veritrace-platform/telemetry-stream-service) | Telemetry ingestion, breach detection, real-time notifications | Go, MQTT, Kafka, TimescaleDB |
| [`blockchain-relayer-service`](https://github.com/veritrace-platform/blockchain-relayer-service) | Merkle batching, gasless commits, chain indexing, proofs | Go, Redis, go-ethereum |
| [`smart-contracts`](https://github.com/veritrace-platform/smart-contracts) | On-chain commitment contract | Solidity, Foundry, OpenZeppelin |
| [`enterprise-dashboard`](https://github.com/veritrace-platform/enterprise-dashboard) | Management web application | Web |
| [`driver-mobile-pwa`](https://github.com/veritrace-platform/driver-mobile-pwa) | Driver app: scanning, handover, alerts | PWA |
| [`public-trace-portal`](https://github.com/veritrace-platform/public-trace-portal) | Consumer verification portal | Web |

## Documentation

Architecture, domain rules, API and messaging contracts, decision records, and the roadmap are in
[`veritrace/docs`](https://github.com/veritrace-platform/veritrace/tree/main/docs).

## License

[MIT](https://github.com/veritrace-platform/.github/blob/main/LICENSE)
