# Revo

**TLDR:** Fintech mexicana de cashback real (estilo ShopBack): el usuario compra online en marcas MX vía redes de afiliados (Awin, Impact, CJ, Admitad) y recibe 60% de la comisión en MXN retirable por SPEI. Proyecto más activo de Gab — trabajo diario, auditoría de seguridad en curso.

**Fuente:** repos en disco (no vive en `raw/`): `C:\Users\gabri\Documents\GitHub\revo-backend` y `revo-frontend`. Docs canónicos DENTRO del repo — leerlos antes de cambiar lógica: `BUSINESS.md`, `DOMAIN.md`, `RULES.md`, `SECURITY.md`, `ARCHITECTURE.md`, y la spec canónica `revo-frontend/revo-business/BUSINESS_LOGIC_SPEC.md` (BL-01..BL-99).

## Stack
- **Backend:** Python 3.11 + FastAPI + Supabase (sin ORM), deploy en Railway. Admin auth JWT+TOTP+AAL2.
- **Frontend:** Next.js 16 + React 19 + Supabase SSR + Sentry, deploy en Vercel.
- Escrituras críticas de dinero SOLO por RPCs Postgres (`acreditar_cashback`, `solicitar_retiro`).

## Estado (2026-07-06)
- **Auditoría pre-lanzamiento activa HOY.** Backend en rama `hardening/audit-fase2-quickwins` (sin mergear a main): cierre de fugas de dinero en el pipeline de postbacks, endurecimiento de arranque/CSRF/IP admin/logs, retiros C-2 resuelto con RPC atómico, fix crítico de retiros atorados (kyc_pendiente/revision).
- Se quitó la whitelist y se abrió el signup E-1 — cambio de hoy en ambos repos (ADR en backend; ADR-0002 + UI en frontend).
- Frontend en rama `fix/inicio-carousel`: UI con colores de marca (banner, carrusel, mosaicos de tiendas), headers COOP/CORP y CSP de admin endurecido (A-3). Ramas `deploy-branding` y `feat/store-logos-branded-tiles` también vivas — revisar cuáles ya se cherry-pickearon.
- Pendientes vivos grandes: HMAC obligatorio CJ/Admitad (TB1), redirect IDOR (M-9, plan en `SECURITY.md` §P1), 2FA usuario reactivo, payouts automáticos, y mergear las ramas de hardening a main.

## Decisiones clave (no contradecir)
- **Números de negocio (ventanas, montos, umbrales KYC): NO se copian aquí.** Regla anti-drift del repo: la fuente de verdad es `core/constants.py` + `RULES.md` — consultarlos siempre en vivo (los umbrales ya cambiaron una vez, 2026-05-25).
- Retiros manuales por admin; escrituras de dinero SOLO por RPCs Postgres.
- Monolito FastAPI decidido — no proponer microservicios.
- El repo ya tiene 12 subagentes propios + routing rules en su `CLAUDE.md` — al trabajar en Revo se usan ESOS, no los genéricos.

## Relaciones
- Mismo patrón de stack/documentación que [[vidarq]].
- Assets de marca ya definidos (`Downloads\REVO_BRAND_ASSETS.zip`) — nunca regenerar.
