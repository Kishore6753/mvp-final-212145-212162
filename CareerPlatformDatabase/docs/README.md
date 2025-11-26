# Career Platform Database – Documentation

## Overview
This documentation set covers the MVP Career Platform’s PostgreSQL database container, including schema proposals, internal API security, ingestion from Excel, audit/traceability, and operations.

## Contents
- architecture.md — Role within overall architecture, access boundaries
- apis-internal-db.md — Internal data access API (secure CRUD, audit, traceability)
- modules-and-schema.md — ERD-level schema, relationships, indexing
- data-ingestion-from-excel.md — Ingestion plan, mappings, versioning, validation
- security-and-compliance.md — mTLS, internal token, encryption-in-transit, audit
- testing-strategy.md — DB fixtures and verification
- operations-and-env.md — Startup, healthchecks, environment variables, runbook
- appendices.md — Data dictionary and field mappings

## Related Containers
- CareerPlatformBackendAPI (FastAPI): sole consumer of DB/internal API
- CareerPlatformWebFrontend (React): interacts via backend only (no direct DB access)

## Source References
- startup.sh, backup_db.sh, restore_db.sh
- db_visualizer/server.js, postgres.env

