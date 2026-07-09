# VIDARQ — grafo de módulos (para orquestar trabajo en paralelo)

**TLDR:** El mapa de módulos de [[vidarq]] con sus dependencias y estado, para saber
qué se puede despachar en paralelo (fan-out con `dispatchMany` del kernel) y qué no.
**Vidarq ya está ~90% construido** (su roadmap vive en el repo: `PLAN.md` F0–F9), así
que este doc sirve para (1) orquestar lo que FALTA y (2) ser la plantilla para arrancar
sistemas nuevos desde cero, donde el paralelo brilla más.

**Fuente:** repo `Desktop\vidarq` — `PLAN.md`, `RESUMEN-AVANCE.md`, `app/(dashboard)/`,
`app/api/`, `CLAUDE.md` (leídos 2026-07-08). No inventar; el repo manda.

## El grafo (capas por dependencia)

```
FUNDACIÓN  ─────────────────────────────────────────────  [HECHO]
  schema Prisma (25 modelos) · Auth (NextAuth v5 + lockout) ·
  seguridad (validación IDs, AuditLog, CSP, invalidación de sesión) ·
  navegación + obras (10 pestañas por obra)
        │  (todo lo de abajo depende de esto)
        ▼
NÚCLEO TRANSACCIONAL  ────────────────────────────────────  [HECHO]
  movimientos (177K filas) ← autorizaciones (F5, egreso atómico + póliza)
                           ← cobranzas (F6-A, ingreso atómico + comprobante)
        │
CATÁLOGOS (dependen de fundación, casi independientes entre sí)  [HECHO]
  clientes · proveedores · categorías · chequeras · contratos ·
  unidades/inventario (F6-C) · inversionistas · presupuestos
        │
DERIVADOS (dependen de núcleo + catálogos)  ──────────────  [HECHO]
  reportes (Estado de Fondos, Egresos con drill-downs, export Excel/PDF) ·
  conciliación bancaria v1 · admin · seguridad · audit-log
```

Regla de oro del paralelo: **B no puede empezar antes que A si B depende de A.** Por eso
la fundación y el núcleo se hicieron interactivos (dirigidos por Gab); lo independiente
es lo que se despacha en paralelo.

## Lo que FALTA — y qué es paralelizable

### Paralelizable YA (independientes entre sí → un worktree cada uno)
Se pueden despachar juntos con `dispatchMany` porque no se tocan:
- **Sentry** (monitoring) — infra, aislado.
- **Pruebas E2E** — necesita BD de pruebas; aislado del resto.
- **Cloudflare WAF + dominio + backups** — infra, aislado.
- **Conciliación v2** (partidas: comisiones/intereses + cerrar mes) — cuelga de
  conciliación v1 (hecho), no de lo demás.

### Bloqueado por decisión de Cintia (NO paralelizar sobre incógnitas)
Estas esperan respuesta de negocio; despacharlas ahora = construir sobre suposiciones:
- Reporte oficial (¿solo banco o + histórico? formato exacto).
- Fórmula del "saldo en libros" (Conciliación).
- Permisos: quién otorga / súper-admin.
- Inventario: ¿precargar ventas históricas?
- Renta vencida: regla + correos de clientes (bloquea **F6-B alertas por correo/Resend**).
- Torres A/B + montos para doble autorización.

### Secuencial / de entrega (no paralelo por naturaleza)
- Contraseñas fuertes + activar 2FA (día de entrega).
- F8: limpiar data sucia, merge de duplicados, histórico bloqueado, torres.
- Capacitación / handoff al equipo.

## Cómo se orquesta esto con el kernel (el mecanismo, aplicado a Vidarq hoy)

1. **Fundación + núcleo:** ya hechos (fueron interactivos).
2. **Fan-out de lo independiente-no-bloqueado:** ej. una sola corrida despacha en
   paralelo `Sentry`, `scaffold de E2E` y `doc de infra Cloudflare` → 3 worktrees →
   3 diffs → code review → merge por módulo. Cada uno con su spec `.md`.
3. **Lo bloqueado por Cintia** espera decisión; no se paraleliza sobre incógnitas.
4. **Merge a main = humano** (Vidarq es producción). El kernel prepara el diff; Gab aprueba.

## Nota

Para sistemas **nuevos** (features grandes de [[sinai-pedidos]], el de [[tag]]) este
mismo patrón rinde mucho más: al construir de cero hay muchos módulos independientes
que caen en paralelo desde el inicio. Vidarq es el caso "maduro"; Sinaí/TAG son los
"greenfield" donde el fan-out se nota.
