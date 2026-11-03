# ADR 004: Consent Management — Versioned, Revocable, Audited

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Health data processing requires explicit, informed, revocable consent (GDPR Art. 7, Chilean Law 19.628). Must track: what consented to, when, version, revocation.

## Decision
Consent as first-class entity: versioned documents, digital signatures, revocation workflow, audit trail on every consent action.

## Rationale
- Legal compliance: Meets GDPR/Chilean requirements for granular consent
- Versioning: Consent form changes create new version; patients re-consent
- Revocation: Patient can withdraw; system enforces (blocks processing)
- Audit: Every consent action logged with trace_id, patient_id, version, action

## Consequences
- Schema complexity: consent_versions, patient_consents, consent_audit tables
- Application logic: Every data access checks current consent status
- UX: Consent dashboard for patients; provider alerts for expired consent
- Storage: Signed documents in Supabase Storage (encrypted)