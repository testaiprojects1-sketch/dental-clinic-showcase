# ADR 005: Audit Log Design — Healthcare Data Access

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Healthcare regulations require audit trails for all PHI access (HIPAA, GDPR Art. 30, Chilean Law 19.628).

## Decision
Structured audit logs for every patient data access. Event types: patient_data_access, appointment_change, consent_change, photo_access, treatment_update. Stored in PostgreSQL (RLS) + Loki.

## Rationale
- Granular event types enable compliance reporting
- RLS on audit tables: providers see only their patients' audit trails
- Loki: Fast querying for compliance dashboards
- Immutable: Append-only; deletion requires admin + audit

## Consequences
- Volume: High (every API call logs)
- Storage: Tiered (Loki hot 30d, PG cold 7y, S3 raw 90d)
- Query complexity: Compliance reports need specialized dashboards
- PII in audit logs: Minimized (patient_id only, no clinical data)