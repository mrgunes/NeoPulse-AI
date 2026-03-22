# NeoPulse v1 — Architecture

## Overview

_This file documents the high-level architecture of NeoPulse v1. Diagrams live in `/diagrams/`._

---

## Architecture Diagram

![NeoPulse v1 Architecture](../diagrams/neopulse-v1.png)

> Source file: `diagrams/neopulse-v1.eraser`

---

## System Layers

### 1. Ingestion Layer
- `healthkit-ingestor` — polls Apple HealthKit API on schedule
- `tesla-ingestor` — polls Tesla Fleet API on schedule
- `scheduler` — orchestrates polling intervals
- `token-manager` — manages OAuth tokens for external APIs

### 2. Transport Layer
- **Redis Queue** — event bus between ingestors and normalization worker
- Transport only, no persistence

### 3. Storage Layer
- **Raw Event Store** — append-only, source of truth for all raw events
- **PostgreSQL (Canonical Store)** — normalized, queryable data

### 4. Processing Layer
- **Normalization Worker** — consumes from Redis, writes to Raw Store → normalizes → writes to PostgreSQL
- **Dead Letter Queue (DLQ)** — captures failed/unprocessable events

### 5. Output Layer
- **Report Service** — generates scheduled and on-demand reports
- **REST API** — exposes canonical data to consumers
- **Dashboard** — UI for monitoring and visualization

### 6. Observability Layer
- Metrics, logging, and error tracking across all components

---

## Key Design Decisions

| Decision | Rationale |
|---|---|
| Redis as transport only | Avoid Redis as a database; keep it ephemeral |
| Raw store before normalization | Replayability — raw events are never lost |
| Queue-gated ingestion | All writes go through the queue, no shortcuts |
| No direct ingestor → DB writes | Enforces normalization pipeline integrity |

---

## Notes

_Add architectural notes, decision records, and open questions here._
