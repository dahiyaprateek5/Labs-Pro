<div align="center">

<img src="https://img.shields.io/badge/Status-Live-1D9E75?style=for-the-badge" />
<img src="https://img.shields.io/badge/Labs%20Live-28-378ADD?style=for-the-badge" />
<img src="https://img.shields.io/badge/Revenue-Active%20Monthly-534AB7?style=for-the-badge" />

<br/><br/>

# LabsPro
### Multi-Tenant Diagnostic Lab Management SaaS

*Built by [Prateek Dahiya](https://github.com/dahiyaprateek5)*

</div>

---

## The Problem

Small pathology labs in India run on **paper registers**, **Word templates**, and **Tally** — all open simultaneously, all manually updated, all prone to errors.

No unified system. Patient records get duplicated. Reports get lost. Billing is slow. No audit trail.

**LabsPro replaces all of it with one calm dashboard.**

---

## What It Does

> Register a patient → enter results → print a report → raise a GST invoice → take payment — **in under 90 seconds.**

<table>
<tr>
<td width="50%" valign="top">

### Patient Management
- **Returning patient detection** — 10-digit mobile triggers instant lookup, no duplicate records
- **Visits timeline** — every visit, report, and invoice linked to one Patient ID
- **Edit & propagate** — updating details updates across all visits simultaneously

</td>
<td width="50%" valign="top">

### Report Engine
- **Formula auto-calc** — LDL, eGFR, A:G ratio auto-fill from entered values (safe recursive-descent parser, no `eval()`)
- **Critical value alerts** — red CRIT badges + banner + WhatsApp escalation to doctor
- **Text sections** — interpretation blocks, method notes on printed reports

</td>
</tr>
<tr>
<td width="50%" valign="top">

### Print & Verification
- Multi-page letterhead-aware printing
- Footer (QR + signature + disclaimer) pinned to every page bottom
- Live margin adjuster — top/bottom/side mm, saved as lab defaults
- **QR-verifiable reports** — public verification page, no login required

</td>
<td width="50%" valign="top">

### Billing & Reconciliation
- GST-optional invoices with per-invoice override
- Payments by mode: Cash / UPI / Card / Bank / Cheque
- **Day Close** — billed, collected, outstanding, by-mode breakdown
- Printable reconciliation receipt with signature lines

</td>
</tr>
<tr>
<td width="50%" valign="top">

### Operations
- **Cmd+K command palette** — search patients, invoices, doctors, tests
- **Activity log** — every action logged with user + timestamp
- **Lab Branches** — multi-branch support, patients tagged to branch
- Recent Patients sidebar (localStorage, per-lab, cross-tab synced)

</td>
<td width="50%" valign="top">

### Platform Admin Console
- Invite-code gating — labs sign up only with platform-admin codes
- Cross-lab oversight, bulk suspend/reactivate, CSV export
- Secure password override via bcrypt DB RPC
- No plaintext passwords ever stored or transmitted

</td>
</tr>
</table>

---

## Tech Stack

<div align="center">

![React](https://img.shields.io/badge/React%2018-61DAFB?style=flat-square&logo=react&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI%2FCD-2088FF?style=flat-square&logo=github-actions&logoColor=white)

</div>

<br/>

| Layer | Tools |
|---|---|
| Frontend | React 18, Vite, React Router 6, Tailwind CSS |
| Backend / DB | Supabase — PostgreSQL 17, Auth, Storage, Row-Level Security |
| Security | RLS on every lab-scoped table; `current_lab_id()` isolates tenants at DB layer |
| Barcodes | bwip-js — Code 128 linear barcode on sample tube labels |
| Monitoring | Sentry (frontend + backend), Vercel Analytics, Vercel Speed Insights |
| Frontend Hosting | Vercel — auto-deploy on push to `main` |
| Backend Hosting | Railway — Dockerised Express API, Node 22-slim, healthcheck-gated |
| CI/CD | GitHub Actions — frontend build + backend syntax check on every push and PR |

---

## Architecture

```
React 18 SPA (Vercel)
        │
        │  Supabase JS client (anon key only — service role never exposed)
        ▼
Supabase
  PostgreSQL 17 + Row-Level Security
  Auth (email + Google OAuth)
  Storage (letterheads · logos · signatures · test templates)
        │
        ▼  platform-admin RPCs only
Express API (Railway · Docker · Node 22)
```

**Tenant isolation:** Every lab-scoped table has an RLS policy enforcing `lab_id = current_lab_id()`.
Lab A cannot read Lab B's data — not via frontend bugs, not via direct API calls.

---

## Scale & Impact

<div align="center">

| Metric | Value |
|:---:|:---:|
| Labs live in India | **28** |
| Revenue | Active monthly (SaaS subscriptions) |
| Frontend routes | 30+ |
| Database tables | 15+ with full RLS coverage |
| Deployment | Auto-deploy on push — Vercel + Railway + Supabase |

</div>

---

## Local Setup

```bash
cd frontend
cp .env.example .env        # add VITE_SUPABASE_URL + VITE_SUPABASE_ANON_KEY
npm install
npm run dev                  # http://localhost:5173
```

**Supabase setup**
1. Create project at supabase.com
2. SQL Editor → paste `backend/supabase/schema.sql` → Run
3. Storage → create `letterheads`, `logos`, `signatures`, `test-templates` buckets (public)
4. Auth → enable Google + email, add localhost + production URLs

---

## Roadmap

- [ ] Repeat Last Visit — one-click pre-select last visit's tests
- [ ] Doctor commission tracking — % per referring doctor, monthly statement
- [ ] Bulk WhatsApp dues reminder — multi-select unpaid invoices
- [ ] Reagent inventory lite — stock tracking with low-threshold alerts
- [ ] Patient self-portal — mobile + OTP, own report download

---

<div align="center">

*Built, deployed, and maintained end-to-end by Prateek Dahiya*

[![Email](https://img.shields.io/badge/dahiyaprateek5%40gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:dahiyaprateek5@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dahiyaprateek5/)

</div>
