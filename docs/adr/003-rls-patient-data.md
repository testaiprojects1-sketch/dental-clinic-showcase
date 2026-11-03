# ADR 003: Row-Level Security for Patient Data Isolation

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Multi-provider clinic: each provider sees only their patients. Admin sees all. Patients see only their own data. Must be enforced at database level.

## Decision
PostgreSQL Row-Level Security (RLS) on all patient-facing tables. Policies enforced by Supabase Auth JWT claims.

## Rationale
- Database-level enforcement (cannot be bypassed by application bugs)
- Supabase Auth integrates natively (JWT claims → RLS policies)
- Audit trail: every query logged with user context
- Compliance: Demonstrates "privacy by design" (GDPR Art. 25)

## Consequences
- Policy maintenance: every table + role needs policies
- Performance: RLS adds query overhead (mitigated: indexes, policy simplicity)
- Testing complexity: must test every role/permission combination
- Migration risk: policy changes affect live data access