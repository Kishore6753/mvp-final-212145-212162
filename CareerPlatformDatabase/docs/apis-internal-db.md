# Internal Database API

## Overview
This internal API mediates all access to the PostgreSQL database for the MVP Career Platform. Only the backend service is authorized to call it. The API provides generic CRUD for canonical entities, audit-log retrieval, and traceability resolution back to source documents.

## Security
- Mutual TLS (mTLS) is required; client certificates identify the backend.
- Include X-Internal-Token header with a valid service token.
- No public access; non-backend calls are denied.

## Base URL
- https://career-platform-internal-api.local

## Schemas (Selected)
- Entity: { id: string, entityType: string, data: object }
- AuditLog: { timestamp: date-time, userId: string, action: string, entityType: string, entityId: string, details?: object }
- Traceability: { entityType: string, entityId: string, sourceDocuments: string[] }
- ErrorResponse: { status: string, error: string, details?: object }

## Endpoints
- GET /entities/{entityType} → Entity[]
- POST /entities/{entityType} (body: Entity) → 201 Entity
- GET /entities/{entityType}/{id} → 200 Entity or 404
- PUT /entities/{entityType}/{id} (body: Entity) → 200 Entity or 404
- DELETE /entities/{entityType}/{id} → 204 or 404
- GET /audit/logs → AuditLog[]
- GET /traceability/{entityType}/{entityId} → Traceability

## Canonical Entity Types
users, profiles, roles, competencies, role_competency_map, assessments, gap_results, development_plans, role_adjacency_edges, templates, audit_logs, traceability, file_ingestions, import_batches

## cURL Examples
```bash
# List roles (with mTLS and internal token)
curl --cert client.pem --key client.key \
  -H "X-Internal-Token: $INTERNAL_TOKEN" \
  https://career-platform-internal-api.local/entities/roles
```

```bash
# Traceability for CTO role mapping
curl --cert client.pem --key client.key \
  -H "X-Internal-Token: $INTERNAL_TOKEN" \
  https://career-platform-internal-api.local/traceability/role_competency_map/cto
```

## Notes
- Every write operation must append an audit log with the backend’s service identity and the initiating user context when applicable.
- Indexing guidance in the schema document should be applied to ensure predictable query performance.
