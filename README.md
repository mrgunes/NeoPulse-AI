# NeoPulse v1

AI-powered personal data ingestion and reporting system.

## Architecture
See:
- /docs/system-design.md
- /diagrams/neopulse-v1.png

## Core Pipeline
Ingestors → Redis → Raw Store → Normalization → PostgreSQL → Reports

## Status
Architecture locked. Implementation in progress.
