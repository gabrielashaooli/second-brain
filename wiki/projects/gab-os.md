# Gab OS

**TLDR:** El agentic OS de Gab sobre Claude Code: 4 capas (memoria → skills → automatizaciones → dashboard) construidas EXTENDIENDO este vault como memoria única. Instalación por fases con aprobación explícita entre cada una.

**Fuente:** decisión de diseño 2026-07-06 (sesión de Claude Code); patrón vault-as-memory adaptado a la realidad de los proyectos en disco.

## Arquitectura
1. **Memoria** = este vault (`Documents\second-brain`). Única fuente de verdad de hechos de negocio y decisiones. Notas por dominio en `wiki/projects/`: [[revo]], [[vidarq]], [[bow-beauty]], [[maestria-miacd]], [[ops-personales]].
2. **Skills** — pack inicial `dev` (máx. 3 skills), los demás packs (data, content, ops) diseñados pero no instalados hasta que las primeras se usen 5 veces con inputs reales.
3. **Automatizaciones** — solo con aprobación explícita; local (cron Claude Code) vs remota justificado por tarea.
4. **Dashboard** — al final, solo si las skills demostraron uso real. Botones `claude -p` (PowerShell) + observabilidad.

## Reglas
- Orden de construcción inviolable: memoria → skills → automatizaciones → dashboard.
- Proyectos abandonados del disco (mi-gpt, netflix-recomendador, sinai, tickets, yam, ml-analyzer, entrevista_ai) NO entran al OS.
- Mantenimiento: poda mensual del vault (notas huérfanas, logs viejos, skills sin uso).

## Skills instaladas (en `~\.claude\skills\`)
- `project-status` — escanea git de Revo/Vidarq/bow-scraper/factura-bot/Sinai-pedidos y actualiza las notas de proyecto. Formato 📊 con semáforo por entidad (spec de Gab 2026-07-07).
- `session-log` — cierre de sesión: journal del día + nota de proyecto si cambió estado. Formato estructurado cliente/status/lecciones.
- `maestria-digest` — compila `raw/` de la maestría a `wiki/concepts/` + Mermaid + preguntas abiertas.
- `doc-generator` (2026-07-07, spec de Gab) — documentación técnica de módulos/repos, entregable para clientes Shift.
- `system-architect` (2026-07-07, spec de Gab) — diseño de arquitectura de sistemas nuevos → nota en el vault.
- `deploy-validator` (2026-07-07, spec de Gab) — pre-flight de deploy por proyecto (Vidarq dual-remoto, Revo main limpio para [[sofia]], Sinaí push=prod). Reporta semáforo; el push lo ejecuta Gab (checkpoint 5).
- `env-var-validator` (2026-07-07, spec de Gab) — .env vs .env.example, placeholders, secrets expuestos (sin imprimir valores).
- `prisma-lock-fixer` (2026-07-07, spec de Gab) — **vive en el repo de Vidarq** (`Desktop\vidarq\.claude\skills\`), no global: fix del EPERM de Prisma en Windows.

**Comandos de review — NO son skills nuevas, usan motores existentes (decisión 2026-07-07):**
- "revisa este PR / este código" → skill integrada `/code-review` de Claude Code; en Revo/Vidarq, preferir sus agentes propios (routing rules del repo).
- "escanea vulnerabilidades" → skill integrada `/security-review`; en Revo, `api-security-auditor`/`webhook-security-auditor` del repo.
- En ambos casos el resultado se registra al vault cerrando con `session-log`.

Regla: cada skill debe usarse **5 veces con inputs reales** antes de instalar el siguiente pack. Contadores:
- project-status: 1/5 (2026-07-06) · session-log: 0/5 · maestria-digest: 0/5 · doc-generator: 0/5 · system-architect: 0/5 · deploy-validator: 1/5 (2026-07-07) · env-var-validator: 0/5 · prisma-lock-fixer: 0/5

## Packs diseñados (NO instalados aún — enriquecidos con el blueprint de Gab 2026-07-06)
- **data** — análisis de CSVs/dumps (Mixup/Fincado, TAG inventarios): skill `data-brief` (resumen ejecutivo de un dataset) y `query-fincado` (consultas sobre los dumps).
- **content** — Bow Beauty: skill `bow-brief` (contenido/research de marca con Higgsfield MCP, sin tocar identidad) y `bow-market-scan` (research de mercado beauty MX).
- **ops** — skill `factura-run` (correr factura-bot sobre tickets nuevos) y `inbox-brief` (resumen accionable de Gmail+Calendar). Nota: menús ya cubierto por la skill existente `planificador-menus`.
- **revo-growth** (nuevo, del blueprint) — `competencia-scan` (rates/beneficios de cashbacks MX: Beruby, Letyshops, super-apps) y `oferta-optimizer` (variaciones de oferta evaluadas contra datos históricos; SIEMPRE con approval gate antes de tocar producción — checkpoint 2).
- **shift-clientes** (nuevo, del blueprint) — `pr-review-cliente` (revisión de PRs con reporte para cliente), `sinai-inspeccion` (reportes multi-sucursal Sinaí), `tag-inventario` (cuando TAG tenga datos en disco).

## Arquitectura de 5 capas (mapeo 2026-07-07 — qué cubre cada capa HOY)

Blueprint canónico de Gab → implementación real en Gab OS. Regla: no instalar
una pieza nueva si algo existente ya cubre la capa mejor.

**1. Modelos (el cerebro)**
- LLM base: Claude Code con **Opus** como default de sesión (política cero-Fable-5 del 2026-07-07 — ver sección Política de modelos); Sonnet en agentes de revisión/rutinas, Opus en guardianes de dinero.
- Especializados: **Higgsfield MCP** (imagen/video/audio/3D) y skill `nano-banana` (imágenes Gemini) para todo lo visual/branding.
- Embeddings/RAG: **NO instalado a propósito** — la investigación original de Gab documentó que el vault plano le ganó al pipeline RAG. Camino futuro si el vault crece a miles de notas: pgvector en Supabase (ya está en el stack).

**2. Datos y memoria (el contexto)**
- Corto plazo: el contexto de sesión de Claude Code (+ protocolo de sesión: leer journal + nota de proyecto al abrir).
- Largo plazo: **este vault** — única fuente de verdad (memoria interna de Claude Code solo apunta aquí). Preferencias y lecciones → secciones Decisiones/Lecciones del journal y las notas.
- Base vectorial: no aplica hoy (ver capa 1). Búsqueda = Obsidian search + grep, que a esta escala es instantáneo.

**3. Herramientas (la acción)**
- Conectores SaaS: MCP de **Gmail/Calendar/Drive** (cuenta sacaljose), **GitHub/Supabase/Sentry/Postgres** (en los `.mcp.json` de Revo), **Higgsfield**.
- Código: PowerShell/Bash + Python locales vía Claude Code (el "sandbox" es la propia máquina con permission modes).
- Navegación web: **Claude in Chrome** (browser autónomo) + WebSearch/WebFetch.

**4. Orquestación (el flujo)**
- Agentes especializados: **9 subagentes de Revo en disco** (fraude, webhooks, SQL, deploy… — su CLAUDE.md lista 12: drift pendiente de limpiar en sesión de Revo), **3 de Vidarq** (data-integrity, prisma-expert, security-reviewer), + agentes generales de Claude Code.
- Multi-agent router: las **routing rules del CLAUDE.md de Revo** (tabla de delegación + desempates) + las descripciones de skills — el dispatcher es Claude Code mismo. Para fan-outs grandes existe el modo workflow multi-agente.
- HITL: **niveles de autonomía** (suggest/co-pilot/autopilot, default co-pilot) + **5 checkpoints humanos** (sección arriba en el CLAUDE.md del vault).

**5. Gobernanza y operaciones (la seguridad)**
- Guardrails: prohibiciones del vault (BDs de producción, logos, raw/ inmutable) + permission modes de Claude Code + reglas por skill (secrets nunca impresos, deploy-validator nunca deploya, poda solo propone).
- Telemetría y auditoría: `wiki/log.md` (append-only, cada cambio), `journal/` (cada sesión con cliente/status/decisiones), `scheduler.log` (corridas headless con timestamps y exit codes), contadores de uso por skill.
- PII: Revo ya scrubbea PII hacia Sentry (`before_send`). Costo por token: no aplica (suscripción) — el proxy de costo es el contador de usos.

**Huecos honestos (roadmap, no deuda oculta):** base vectorial (si el vault escala), telemetría de tokens (si se migra a API), knowledge graph de código tipo Graphify (primer candidato post-gate de 5 usos).

## Adaptación del blueprint "Agentic AI System" (2026-07-06)
Gab trajo un blueprint completo (autonomía por niveles, guardrails, observabilidad, log estructurado). Qué se adoptó y qué no:
- **Adoptado:** mapa de entidades (Shift/TAG/Sinaí) en el CLAUDE.md del vault; niveles de autonomía suggest/co-pilot/autopilot (default co-pilot); 5 checkpoints humanos obligatorios; formato de log estructurado con cliente/status/lecciones en la skill session-log; casos de uso por cliente sembrados en los packs.
- **NO adoptado (deliberado):** Langfuse, Stripe API, Graphify MCP, Tremor, React Flow, budgets por token y rate-limiting — infraestructura que no existe hoy; construirla antes de que las skills se usen sería repetir el error dashboard-first. El vault + wikilinks ES el knowledge graph ligero; los contadores de costo no aplican con suscripción de Claude Code.
- **Roadmap Dashboard v1** (cuando se desbloquee la Fase 4 completa): activity feed de corridas, approval gates con preview e impacto, autonomy slider visible, undo por acción.

## Aprobaciones que NUNCA se saltan (resumen — detalle en CLAUDE.md del vault)
Comunicaciones externas · dinero/ofertas visibles a usuarios · datos de producción · irreversibles · deploys.

## Política de modelos (Gab 2026-07-07: "cero Fable 5 — el modelo justo por tarea")
Fable 5 queda FUERA de todo el sistema (cambio de acceso previsto 2026-07-08).
- **Opus** — (a) default de sesiones interactivas (`model: opus` + fallback sonnet en `~\.claude\settings.json`; antes decía `fable` y de ahí heredaba todo); (b) guardianes de dinero real: `fraud-analyst` y `revo-cashback-auditor` (Revo), `data-integrity` (Vidarq).
- **Sonnet** — el resto de agentes de Revo/Vidarq (explícito en frontmatter, nadie hereda sin decidirlo), las 2 rutinas headless (`--model sonnet` en los .cmd), el chat del dashboard y el advisor.
- **No-Claude** — nano-banana → Gemini; Higgsfield → modelos propios.
- Drift detectado: el CLAUDE.md de revo-backend lista 12 agentes pero en disco hay 9 — pendiente de limpiar en una sesión de Revo.

## Automatizaciones instaladas (Programador de tareas de Windows)
- **GabOS - Estado semanal** — lunes 08:03, corre `~\.claude\gab-os\estado-semanal.cmd` (`claude -p` headless con project-status). Log: `~\.claude\gab-os\scheduler.log`.
- **GabOS - Poda mensual** — día 1 de cada mes 09:07, `poda-mensual.cmd`. Solo PROPONE candidatos en `journal/poda-YYYY-MM.md`; nunca borra.
- Rechazadas/pospuestas: briefing Gmail+Calendar (frágil en headless por auth interactiva de conectores).

## Estado
- 2026-07-06 — **Fase 1 (memoria) instalada**: notas de los dominios + protocolo de sesión y prohibiciones en `CLAUDE.md`.
- 2026-07-06 — **Fase 2 (skills) instalada**: pack dev (3 skills) + packs restantes diseñados. Agregada nota [[sinai-pedidos]] (producción).
- 2026-07-06 — **Fase 3 (automatizaciones) instalada**: 2 tareas programadas aprobadas por Gab. `GAB-OS-README.md` escrito en la raíz del vault.
- 2026-07-06 — **Fase 4 parcial — Dashboard v0 (solo observabilidad)**: servidor Node sin dependencias en `~\.claude\gab-os\dashboard\server.js` (http://127.0.0.1:4747), lanzador `Gab OS.cmd` en el Escritorio. Muestra proyectos/journal/corridas/contadores en vivo desde el vault; los **botones de acción están bloqueados** hasta 5/5 usos por skill (visible en la UI). Verificado por HTTP (200, 7 proyectos); Chrome vía extensión Claude requiere permiso de sitio para 127.0.0.1.
