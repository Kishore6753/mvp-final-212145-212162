# Appendices

## Data Dictionary (Selected)
- Proficiency: ENUM(F,P,A,Au)
- Range Tokens: “P–A”, “A–Au” → stored as required_min_level and required_max_level
- role_adjacency_edges.weight: float(0..1), computed via overlap formula
- traceability.source_documents: array of file paths or URIs
- file_ingestions.status: enum(pending, success, failed)

## Excel-to-DB Mapping Summary
- Competency_mapping.xlsx → role_competency_map
- CA_Role_Adjacency.xlsx → role_adjacency_edges (+ rationale)
- Role_Navigator_Worksheet.xlsx → user preferences attached to gap/plan context

## Index Suggestions
- role_competency_map(role_id, competency_id)
- role_adjacency_edges(weight)
- gap_results(user_id, target_role_id, created_at DESC)
- audit_logs(timestamp DESC, entity_type, entity_id)

## Example Entities
```json
{
  "entityType": "roles",
  "id": "cto",
  "data": {
    "name": "Chief Technology Officer",
    "description": "Owns technology strategy and platform outcomes",
    "version": "2025-11-24",
    "source": "attachments/role_cards/"
  }
}
```
