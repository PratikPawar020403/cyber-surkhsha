# Project: CyberSuraksha / NCRP 2.0 SPA Comprehensive UI/UX & A11y Audit

## Architecture
- Single Page Application (`index.html`) implementing the CyberSuraksha / NCRP 2.0 portal with 8 distinct views:
  1. Home View (Hero, Emergency CTA, Stats, Quick Links, Modals)
  2. 4-Step Incident Report Wizard (Step 1: Incident Category & Details, Step 2: Suspect Info & Identifiers, Step 3: Victim Details & Evidence Upload, Step 4: Preview, Review Sheet & Verification)
  3. Confirmation / Printable Slip View
  4. Diagnostic Triage & Self-Help Wizard
  5. Learning Corner / Cyber Hygiene Quiz & Guides
  6. Suspect Scanner / UPI / Account / Phone Verification
  7. Citizen Tracking Dashboard & Status Lookup
  8. Officials Queue / Law Enforcement Triage Portal

## Feature Inventory & Audit Domain Mapping
| # | Domain / Feature | Description | Milestone | Source |
|---|------------------|-------------|-----------|--------|
| 1 | R1: Visual Hierarchy & Styling | Typography scale, colors, spacing rhythms, contrast, button affordances across all 8 views | M1 | ORIGINAL_REQUEST §R1 |
| 2 | R2: Interactive Flows & State Machine | Step navigation, validation triggers, OTP box, suspect chips, location auto-detect, review sheet, printable slip, draft persistence | M2 | ORIGINAL_REQUEST §R2 |
| 3 | R3: A11y, WCAG 2.2, GIGW 3.0 & Bilingual | Keyboard navigation, focus rings, ARIA live regions, contrast, screen reader semantics, Hindi/Devanagari text clipping/alignment | M3 | ORIGINAL_REQUEST §R3 |
| 4 | R4: Mobile Usability & Responsiveness | Mobile (375px-414px), tablet (768px), desktop (1280px+), tap targets (>=44px), horizontal overflow, sticky bar overlaps | M4 | ORIGINAL_REQUEST §R4 |
| 5 | Synthesis & Top 5 Quick Wins | Consolidated defect log categorized with severity, line numbers, user impact, drop-in fixes, and top 5 quick wins | M5 | ORIGINAL_REQUEST §Acceptance Criteria |

## Milestones
| # | Name | Scope | Dependencies | Status |
|---|------|-------|-------------|--------|
| M1 | Visual Hierarchy & Typography Audit | Complete audit of styling consistency, typography, contrast, and visual rhythm across all 8 views | none | PLANNED |
| M2 | Interactive Flows & State UX Audit | Audit of forms, validation states, wizard navigation, OTP, dynamic elements, draft storage | none | PLANNED |
| M3 | Accessibility & Bilingual Polish Audit | WCAG 2.2 AA/AAA, GIGW 3.0, ARIA, keyboard navigation, Hindi Devanagari typography | none | PLANNED |
| M4 | Responsive & Mobile Usability Audit | Mobile/tablet/desktop layouts, touch targets, overflow, sticky elements, modals | none | PLANNED |
| M5 | Consolidation & Quick Wins Synthesis | Consolidated report with line numbers, severity, drop-in code fixes, and top 5 quick wins | M1, M2, M3, M4 | PLANNED |

## Code Layout
- Target application: `c:/Users/prati/OneDrive/Desktop/cc/index.html`
- Audit artifacts directory: `c:/Users/prati/OneDrive/Desktop/cc/.agents/`
