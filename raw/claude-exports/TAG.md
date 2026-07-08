---
title: TAG (The Abadi Group)
type: entity
origen: second brain de la cuenta de trabajo de Gab (claude.ai work). Provisto por Gab el 2026-07-07 y guardado aquí como fuente. Las rutas en `sources:` apuntan al OTRO vault, no a esta máquina.
sources:
  - raw/claude-exports-work/account-memory.md
  - raw/claude-exports-work/projects/TAG.md
  - raw/claude-exports-work/Organizar ideas de junta con cliente TAG.md
  - raw/claude-exports-work/Validación de modelo con datos de Abadi Group.md
  - raw/claude-exports-work/Master prompt para proyecto de muestras en Claude.md
  - raw/claude-exports-work/Descripción de proyecto.md
  - raw/claude-sessions/2026-06-26-tag-project-explore-find.md
  - raw/claude-sessions/2026-06-29-tag-muestras-local-command.md
related:
  - "[[shift]]"
  - "[[gabriela]]"
  - "[[demo-vs-functional-prototype]]"
  - "[[tag-sample-tracking-system]]"
created: 2026-06-22
last-updated: 2026-06-30
---

**TAG = The Abadi Group**, a **luxury interior-design & construction firm** operating between **Mexico City and New York**. A client of [[shift]], for which Sofia and Gabriela are building a custom **sample-tracking system**. (`raw/claude-exports-work/account-memory.md`)

## Scope: three systems wanted (TAG's own priority order)

TAG asked for three systems, not one (`raw/.../Organizar ideas de junta con cliente TAG.md`):
1. **Samples tracking (URGENT)** — the one being built first (below).
2. **Administration & finance (parallel)** — migrate project-sheet / cash-flow Excels to a database; lowest error tolerance.
3. **Kitchen-cabinetry quoting (phase 3)** — AI reads architectural PDFs → takeoff + price; most complex, deferred.

## The Shift project for TAG (samples)

- Replaces a fragmented manual workflow (Smartsheet + WhatsApp + hand-cut labels) for tracking material **samples** (muestras) through warehouses (Mexico → New York) to architects/clients. The pain: the US receiver can't tell what arrived or where to send it. (source)
- **Fixed-price contract ~$75,000 MXN**, 4 phases, 10–12 weeks, 30/30/40 payment milestones. Built with Supabase/PostgreSQL + React (Claude Code). (source)
- Core data model: an immutable concatenated anchor ID (e.g. `100-WD-01-HW-64 Fulton` = serial resets at 100 per project + material code + project name); SPEC vs ALTERNATIVE samples are separate records preserving history. (source)
- Confirmed product decisions: **pull-based pending-tasks dashboard** per user (Elías rejected push notifications); **two mandatory photos** per sample (supplier label + relabeled); auto label generation + **real label-printer** integration; read-only **QR client dashboard** scoped to one project; tracking numbers entered manually. Out of scope: WhatsApp/carrier APIs. (source, `raw/.../projects/TAG.md`)
- A demo was presented in a meeting (Jun 11, 2026; the "64 Fulton" project used as seed data). (`raw/.../projects/TAG.md`)
- **Refined model from their files:** shipments are **consolidated** (one tracking# → many samples) → modeled as a `Shipment ↔ N samples` entity, not 5 fixed tracking columns; general status is **derived** from an immutable event log (it drifts when hand-kept); SPEC/ALT are a **versioned chain** (.1/.2 on resubmit); photos have 3 layers (raw record / presentable / AI-beautified). (`raw/.../Validación de modelo con datos de Abadi Group.md`)
- **Demo build was painful** — Claude Code kept reproducing the Claude Design *slideshow* instead of a functional app; fixed by behavior-first prompting + real React Router routes, ultimately via a Lovable-style prompt. The reusable lesson: [[demo-vs-functional-prototype]]. (source)
- **The actual build → its own page: [[tag-sample-tracking-system]].** As of 2026-06-29: **v1 is complete and deployed** (7 screens, Supabase ref `emsgftprytzvowigkhlf`); the client saw the demo and liked it; a detailed **v2 brief** ("iterate, don't rebuild" — English UI, free-route step engine, gallery client dashboard, day counters) is approved and starting. (`raw/claude-sessions/2026-06-26-...md`, `raw/claude-sessions/2026-06-29-...md`)

## People (TAG side)

The system's **seed data** (`scripts/seed.ts`) gives the confirmed team roster + roles (`.test` demo emails): (`raw/claude-sessions/2026-06-26-...md`)

- **David Rophie** ("David") — primary contact, known personally by Sofia; Elías's cousin. (`raw/.../projects/TAG.md`)
- **Elías Abadi** ("Elías") — **PM / product decision-maker** (made the v2 calls: pull-based dashboard, Discontinued as Declined subcategory). Has an active login. (source, `raw/claude-sessions/...`)
- **Abraham** — leads **Mexico operations / samples team**; in v2, the **only role allowed to edit/correct supplier names** (the catalog editor). Active login. (`raw/claude-sessions/2026-06-26-...md`) *(Per `CLAUDE.md`, a different Abraham works at [[vidarq]]; only the TAG Abraham appears here.)*
- **Kitzia** — Mexico back-office, active login (used in demos to create samples). **Valeria, Fabián** — Mexico back-office (no active login in the seed). (`raw/claude-sessions/2026-06-26-...md`)
- **Elizabeth** — Mexico back-office (no login in seed); will own the **label visual design** later. (`raw/claude-sessions/2026-06-26-...md`)
- **Viviana** — **New York warehouse / reception**: enters via her pending dashboard, receives, prints/sticks the label, uploads the 2 (Front/Back) photos, sets next action; approves nothing. Active login. (`raw/claude-sessions/2026-06-26-...md`)

> **Resolved (Viviana's location):** the seed data sets her role to **"NY (recepción)"**, team **Nueva York** — confirming **New York**. This closes the earlier Mexico-vs-NY contradiction; the one source that grouped her with Mexico back-office is outweighed.

> Matches `CLAUDE.md`: "TAG (David and Elias are the chiefs, with a different Abraham and one other on the team building their system)."
