# Dental Clinic Platform — Showcase
**Full-stack patient management + consultation panel for dental/facial aesthetics clinic.**  
*Public architecture showcase — implementation is private.*

[![Status](https://img.shields.io/badge/Status-Production%20Ready-00C9A7)]()
[![Governance](https://img.shields.io/badge/Governance-By%20Design-0B2D4A)]()
[![License](https://img.shields.io/badge/License-Private-E85D4D)]()

---

## 🎯 Problem & Outcome

**Problem:** Dental/facial aesthetics clinics need: patient CRM, treatment planning, consultation visualization, appointment scheduling, and compliance (health data privacy) — but existing solutions are either generic (not dental-specific) or legacy (poor UX, no modern stack).

**Outcome:**
- `Patient management`: Complete CRM with treatment history, photos, consent forms
- `Consultation panel`: Interactive 3D facial visualization for treatment planning (Three.js)
- `Appointment scheduling`: Real-time calendar with provider availability
- `Compliance`: GDPR/Chilean health data ready — audit logs, encryption, access controls
- `Deployed`: Live at dra-fontecilla site (public demo)

**Domain:** Healthtech / Medical Aesthetics · **Regulatory touchpoints:** GDPR, Chilean Law 19.628 (health data), HIPAA-aligned

---

## 🎥 Demo

| Format | Link |
|--------|------|
| **Full walkthrough (5-8 min)** | [🎥 YouTube / Loom / Vimeo](https://...) |
| **Quick demo (60 sec GIF)** | ![Demo](assets/demo.gif) |
| **Live site** | [dra-fontecilla](https://github.com/testaiprojects1-sketch/dra-fontecilla) |
| **Consultation panel** | [aura-panel-consulta](https://github.com/testaiprojects1-sketch/aura-panel-consulta) |

---

## 📐 Architecture

### System Overview
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {
  'primaryColor': '#0B2D4A',
  'primaryTextColor': '#fff',
  'primaryBorderColor': '#00C9A7',
  'lineColor': '#888',
  'secondaryColor': '#1A1A2E',
  'tertiaryColor': '#E85D4D',
  'background': 'transparent',
  'mainBkg': 'transparent',
  'secondBkg': 'transparent',
  'tertiaryBkg': 'transparent'
}}}%%
flowchart LR
    subgraph Patient[Patient Portal]
        Portal[Next.js Patient Portal]
        Appt[Appointment Booking]
        History[Treatment History]
    end
    subgraph Clinic[Clinic Dashboard]
        Dashboard[Provider Dashboard]
        Consult[Aura Consultation Panel]
        Calendar[Schedule Management]
    end
    subgraph Backend[Backend (Next.js API + Supabase)]
        Auth[Auth + RLS]
        API[REST API Routes]
        PG[(PostgreSQL + RLS)]
        Storage[Supabase Storage]
    end
    subgraph Viz[Visualization]
        ThreeJS[Three.js Facial Viz]
        Photo[Photo Management]
    end
    subgraph Governance[Governance]
        Audit[Audit Logger]
        Consent[Consent Management]
        Retention[Data Retention]
    end

    Portal --> Auth
    Appt --> API
    History --> API
    Dashboard --> Auth
    Consult --> ThreeJS
    Calendar --> API
    API --> PG
    API --> Storage
    ThreeJS --> Photo
    API --> Audit
    Audit --> Consent
    Consent --> Retention

    style Patient fill:#0B2D4A,color:#fff
    style Clinic fill:#1A1A2E,color:#fff
    style Backend fill:#1A1A2E,color:#fff
    style Viz fill:#00C9A7,color:#0B2D4A
    style Governance fill:#E85D4D,color:#fff
```

### High-Level Data Flow (Excalidraw)
![Architecture Overview](diagrams/system-overview.svg)  
*Hand-drawn style for architectural thinking — [editable Excalidraw file](diagrams/system-overview.excalidraw)*

### Key Components

| Component | Responsibility | Tech | Governance Hook |
|-----------|----------------|------|-----------------|
| `Patient Portal` | Self-service booking, history, consent forms | Next.js 14, React 18, TypeScript, Tailwind | Audit log on every patient action |
| `Aura Consultation Panel` | 3D facial visualization, treatment simulation | Three.js, React Three Fiber, custom shaders | Session recorded for consent |
| `Provider Dashboard` | Patient list, treatment plans, schedule | Next.js, React, Supabase Realtime | Role-based access (RLS) |
| `Appointment Engine` | Real-time scheduling, conflicts, reminders | Supabase Realtime, PostgreSQL functions | Audit trail; patient notifications |
| `Photo Management` | Before/after photos, consent-tagged, encrypted | Supabase Storage, client-side encryption | GDPR Art. 17 (erasure) compliant |
| `Consent Management` | Digital signatures, versioned, revocable | Custom + Supabase | Audit log; legal compliance |

---

## ⚙️ Tech Stack (What I Built vs. Leveraged)

| Layer | Built by Me | Leveraged / Configured |
|-------|-------------|------------------------|
| **Frontend** | Patient portal, consultation panel (3D), provider dashboard, appointment UI | Next.js 14, React 18, TypeScript, Tailwind, Three.js, React Three Fiber, Framer Motion |
| **Backend** | API routes, RLS policies, PostgreSQL functions, realtime subscriptions | Supabase (PostgreSQL + Auth + Realtime + Storage) |
| **Visualization** | 3D facial model, treatment simulation, photo overlay | Three.js, custom shaders, React Three Fiber |
| **Data** | Patient schema, treatment plans, consent forms, audit logs | Supabase, PostgreSQL, RLS, pgcrypto |
| **Governance** | **Audit logger, consent management, data retention, access controls** | (Private — not in showcase) |
| **Infra & Deploy** | Vercel config, Supabase migrations, GitHub Actions CI | Vercel, Supabase CLI, GitHub Actions |

> **Governance-by-design note:** Every patient data access emits audit entry. Consent is versioned and revocable. RLS enforces provider-only access to assigned patients. Data retention policies automated.

---

## 🛡️ Governance & Compliance Artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| **Model Card** | `docs/model-card.md` | Intended use, limitations, eval results, data lineage |
| **Risk Register** | `docs/risk-register.csv` | Identified risks, likelihood, impact, mitigations, owner |
| **Eval Harness** | `evals/` | Regression tests, security tests, threshold gates |
| **Audit Log Schema** | `docs/audit-log-schema.json` | Structured schema for every data access |
| **ADRs** | `docs/adr/` | Architecture Decision Records (numbered, dated) |

---

## 🚀 How to Run (High-Level)

> **Implementation is private.** This section describes the *operational model* for reference.

```bash
# 1. Infrastructure
supabase db push
supabase storage setup

# 2. Frontend
cd apps/web && npm install && npm run dev

# 3. Eval harness (local)
cd evals && python -m pytest --tb=short
```

**Environment variables required** (see `.env.example` in private repo):
- `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `ENCRYPTION_KEY`, `AUDIT_LOG_LEVEL`

---

## 📁 Repository Structure

```
dental-clinic-showcase/
├── README.md                    ← This file
├── ARCHITECTURE.md              ← Deep-dive: decisions, trade-offs, failure modes
├── diagrams/
│   ├── system-overview.mermaid  ← Mermaid source for system diagram
│   ├── system-overview.svg      ← Excalidraw export (architecture)
│   ├── data-flow.mermaid
│   ├── data-flow.svg
│   ├── sequence-booking.mermaid
│   ├── sequence-consultation.mermaid
│   └── sequence-consent.mermaid
├── assets/
│   ├── demo.gif                 ← 60-sec demo GIF (<2MB, 720p)
│   ├── screenshot-portal.png    ← Patient portal (1440px)
│   ├── screenshot-consult.png   ← Aura consultation panel
│   └── screenshot-dashboard.png ← Provider dashboard
├── docs/
│   ├── model-card.md
│   ├── risk-register.csv
│   ├── audit-log-schema.json
│   └── adr/
│       ├── 001-nextjs-supabase.md
│       ├── 002-threejs-consultation.md
│       ├── 003-rls-patient-data.md
│       ├── 004-consent-management.md
│       └── 005-audit-log-design.md
└── evals/
    ├── test_regression.py
    ├── test_security.py
    └── thresholds.yaml
```

---

## 🔗 Private Repo Access

**Full implementation, CI/CD, infra-as-code, and eval runs** are in a private repository.

**Access process:**  
1. Initial screening call (30 min)  
2. Mutual NDA (template provided)  
3. Temporary read access to private repo + 1-hour walkthrough with me  
4. Access revoked after evaluation period

**Contact:** `jdriveros232@gmail.com` — subject: `Dental Clinic Private Repo Access`

---

## 📌 Related Showcases

- [Novaxis OS / Atlas](https://github.com/testaiprojects1-sketch/novaxis-showcase)
- [MAVIS — Personal AI](https://github.com/testaiprojects1-sketch/mavis-showcase)
- [Mission Control](https://github.com/testaiprojects1-sketch/mission-control-showcase)
- [Tattoo Artist Platform](https://github.com/testaiprojects1-sketch/tattoo-platform-showcase)
- [All showcases](https://github.com/testaiprojects1-sketch?tab=repositories&q=showcase)

---

*Part of the **testaiprojects1-sketch** AI Systems Engineering portfolio — Governance by Design.*