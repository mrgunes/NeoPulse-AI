# NeoPulse AI

Personal health and life data intelligence system. Ingests data from Apple HealthKit and Tesla, normalizes it through a reliable pipeline, and surfaces insights via reports and a dashboard.

---

## Structure

```
neopulse-v1/
│
├── docs/
│   ├── architecture.md      ← system architecture notes and layer breakdown
│   ├── system-design.md     ← full v1 system design document
│
├── diagrams/
│   ├── neopulse-v1.png      ← architecture diagram (Eraser export)
│   ├── neopulse-v1.eraser   ← Eraser.io source file
│
├── README.md
└── .gitignore
```

---

## Core Pipeline

```
[HealthKit / Tesla]
       ↓
  [Ingestors + Scheduler]
       ↓
  [Redis Queue]  →  [Raw Event Store]
       ↓
  [Normalization Worker]
       ↓
  [PostgreSQL (Canonical Store)]
       ↓
  [Report Service / REST API / Dashboard]
```

---

## v1 Constraints (Non-Negotiable)

- Redis is **transport only** — not storage
- Raw store is the **source of truth**
- All ingestion **must go through the queue**
- No direct writes from ingestors to canonical DB
- Normalization is **mandatory** before canonical storage

---

## Status

`v1 — In Design`
