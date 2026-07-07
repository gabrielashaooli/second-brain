# Vidarq

**TLDR:** ERP de administración de condominios/juntas EN PRODUCCIÓN con cliente real (Cintia). Migró 177,642 movimientos desde un Access legacy de 2004. ⚠️ La BD Supabase es producción y no hay staging — máximo cuidado.

**Fuente:** repo en disco (no vive en `raw/`): `C:\Users\gabri\Desktop\vidarq`. Fuentes de verdad dentro del repo: `CLAUDE.md`, `PLAN.md`, `ARCHITECTURE.md`, `SECURITY.md`, `CRONOGRAMA-ENTREGA.md`. Ignorar `C:\Users\gabri\vidarq` (carpeta creada por error).

## Stack
Next.js 16 App Router + TypeScript + Tailwind v4 + shadcn · Prisma + Supabase · NextAuth v5 + 2FA TOTP · Vercel + Cloudflare + Resend + Sentry. 25 modelos, 9 enums.

## Estado (2026-07-06)
- EN VIVO. Racha 2026-06-29→07-03 en la rama `fix/integridad-entrega` (~40 commits, **aún sin mergear a main**): integridad del flujo de dinero + tests + CI (GitHub Actions), 2FA TOTP completo (obligatorio, step-up en pagos, reset por admin), contraseñas fuertes + cambio forzado, Sentry, RUNBOOK + backup verificado, permisos por rol global y visibilidad por secciones, catálogo completo (obras, clientes, proveedores multi-categoría N:N, chequeras), captura manual de movimientos (pedido de Jaime, junta 4), drill-downs de Estado de Fondos (pedido de Cintia, junta 5), y las decisiones de la clienta 2026-07-02 traducidas a build (Q5 scan de contratos, Q8 corrección por reverso).
- Sin actividad 07-04→07-06. Siguiente: verificar y mergear la rama a main y deployar (`prisma db push` + envs de Sentry, ver nota de deploy en el repo), limpiar datos de prueba, E2E, alertas F6-B (Resend).

## Decisiones clave (no contradecir)
- Tablas de reporte NO migradas — se generan en vivo (ADR-004). Historico unificado en `Inmobiliaria.estado=CERRADA` (ADR-005). Conciliación: 7 tablas legacy → 2 modelos (ADR-007).
- Reglas de oro del repo: `parseMonto`/`parseId` obligatorios, locks, `proxy.ts` NO se renombra a `middleware.ts`.
- Lista Movimientos muestra TODAS las fuentes; Estado de Fondos solo `fuente='movimiento'` — es diseño, no bug.
- El repo tiene subagentes propios (`data-integrity`, `prisma-expert`, `security-reviewer`).

## Relaciones
- Proyecto de cliente bajo [[shift-software]] (repo en la org GitHub `shiftsoftwaremx`). Mismo patrón de stack que [[revo]]. Deploy: dos remotos, Vercel deploya desde personal/main.
