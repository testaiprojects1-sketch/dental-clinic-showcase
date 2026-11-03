# ADR 001: Next.js 14 + Supabase for Healthcare Platform

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Need patient portal, provider dashboard, 3D consultation panel, auth, database, file storage, realtime — HIPAA/GDPR aligned.

## Decision
Next.js 14 + Supabase (PostgreSQL + Auth + Realtime + Storage) + Vercel.

## Rationale
- Supabase: Managed PostgreSQL with RLS (critical for patient data isolation), Auth (MFA, providers), Realtime (appointment sync), Storage (encrypted photos)
- Next.js: React Server Components for performance, API routes for backend logic
- Vercel: HIPAA-eligible infrastructure, preview deployments
- Single vendor reduces compliance surface area

## Consequences
- Vendor lock-in (Supabase/Vercel)
- RLS policy complexity (every table needs policies)
- Supabase Storage encryption at rest (managed)
- Cost at scale