# ADR 002: Three.js for 3D Facial Consultation Visualization

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Aura Consultation Panel needs: interactive 3D facial model, treatment simulation (fillers, botox, surgery), photo overlay, real-time parameter adjustment.

## Decision
Three.js (React Three Fiber) with custom shaders for facial mesh deformation and material simulation.

## Rationale
- WebGL runs in browser (no install)
- React Three Fiber: React-native, performant, declarative
- Custom shaders: Realistic tissue simulation (subsurface scattering, elasticity)
- Photo overlay: Canvas texture mapping for before/after
- Exportable: Sessions recordable for consent documentation

## Consequences
- Bundle size (~200KB gzipped for Three.js core)
- WebGL requirement (fallback: static images)
- Mobile performance (mitigated: reduced quality, progressive enhancement)
- Shader maintenance complexity