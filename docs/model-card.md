# Dental Clinic Platform — Model Card

## Model Details
- **Model name:** Dental Clinic Platform (Patient Portal + Aura Consultation)
- **Version:** 1.0.0
- **Date:** November 2025
- **Owner:** Jane Riveros

## Intended Use
Patient management and consultation visualization for dental/facial aesthetics clinics. Patient self-service + provider dashboard.

## Limitations
- Single-clinic (multi-tenant not implemented)
- Spanish/English only
- No insurance billing integration
- 3D visualization requires WebGL-compatible browser

## Evaluation Results
| Suite | Pass Rate | Threshold | Status |
|-------|-----------|-----------|--------|
| Regression | 97% | 95% | ✅ Pass |
| Security | 0 critical | 0 critical | ✅ Pass |
| Access control | 100% | 100% | ✅ Pass |
| Data retention | Compliant | Compliant | ✅ Pass |

## Data Lineage
- Patient-provided: Demographics, medical history, consent, photos
- Provider-generated: Treatment plans, clinical notes, prescriptions
- System-generated: Audit logs, appointments, reminders

## Governance
- Audit logging: Every data access traced
- RLS: Row-level security on all patient tables
- Consent: Versioned, revocable, audit-trailed
- Retention: Automated per Chilean Law 19.628 / GDPR
- Model card: Updated per release