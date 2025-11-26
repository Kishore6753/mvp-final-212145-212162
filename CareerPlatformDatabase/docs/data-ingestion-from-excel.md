# Data Ingestion from Excel

## Overview
The database ingestion pipeline transforms Excel sources into normalized tables with complete audit and traceability. This document details the mappings and validation rules.

## Competency_mapping.xlsx → role_competency_map
### Columns
- “Competency” + multiple role headers (e.g., CA, CTO, BU CIO, …).
- Values: F, P, A, Au, P–A, A–Au.

### Transform
- For each (competency, roleHeader, levelToken):
  - Parse token into (required_min_level, required_max_level).
  - Upsert into role_competency_map(role_id, competency_id, required_min_level, required_max_level).
- Persist traceability(entity_type="role_competency_map", entity_id=role_id, source_documents=[file path]).

### Validation
- Role header must map to a known role id.
- Level token must be in {F,P,A,Au,P–A,A–Au}.
- Competency name must be unique or deterministically merged.

## CA_Role_Adjacency.xlsx → role_adjacency_edges
### Rows (excerpt)
- Target role, Competency, Gap (levels).
- Overlap method: per-competency min/max; overall overlap = mean across mutual competencies.

### Transform
- Compute weight (0..1) using overlap formula; insert role_adjacency_edges(from_role="ca", to_role=target, weight).
- Optional rationale is derived from largest deltas (“top gaps”).

## Role_Navigator_Worksheet.xlsx → user preferences (optional)
### Columns
- Interest (1–5), Overlap (%), Sponsorship (0–5), Runway (months), Risk Fit (0–5), Market Pull (0–5), Scope Fit (0–5), Notes/Evidence.

### Transform
- Store per user and per target role as a preference document or extension record, with references to the latest gap_result.

## Audit, Batches, and File Records
- import_batches captures each run; file_ingestions captures each file with checksum and errors JSON.
- audit_logs capture the initiating actor (service identity) and summarized changes (inserted/updated/failed).

## Example Errors
```json
[
  { "sheet": "Competency_mapping", "row": 14, "column": "CTO", "error": "Invalid token 'AAu'" },
  { "sheet": "Competency_mapping", "row": 27, "column": "CIO", "error": "Unknown role header" }
]
```
