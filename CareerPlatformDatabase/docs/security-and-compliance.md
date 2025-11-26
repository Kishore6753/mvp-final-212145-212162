# Security and Compliance

## Overview
The database container exposes an internal API only to the backend via mTLS and a service token. Storage design, ingestion, and operational scripts follow least-privilege and auditability principles.

## Access Control
- No direct frontend access to the database.
- Internal API requires mTLS and X-Internal-Token; only the backend service account is authorized.

## Data Handling
- Canonical data includes roles, competencies, mappings, assessments, gaps, plans, adjacency edges, and templates.
- Traceability preserves links to source Excel files and import batches.

## Audit and Evidence
- Every write produces an audit_logs row including entityType, entityId, action, timestamp, and details JSON.
- Ingestions also populate import_batches and file_ingestions with validation outcomes.

## Backup and Restore
- backup_db.sh and restore_db.sh provide non-interactive, environment-aware operations with status output.
- Backups must be encrypted at rest and access controlled; operational logs should avoid printing secrets.

## Compliance Posture (MVP)
- Encryption in transit: mTLS for internal API; HTTPS enforced for any exposed endpoints.
- Encryption at rest: rely on platform defaults (documented per environment), with backups encrypted.
- Retention: navigator inputs and similar preference data should be retained only as long as necessary for the journey.

## Logging
- Structured, minimal logs (no secrets); errors include stable messages and identifiers for correlation.
