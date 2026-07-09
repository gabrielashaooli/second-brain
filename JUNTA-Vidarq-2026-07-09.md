# Junta Vidarq — 2026-07-09

> Brief para exponer. Fuentes: repo `Desktop\vidarq` verificado EN VIVO 2026-07-08
> (git: prod=`personal/main`@`55af1ea`, trabajo=`fix/integridad-entrega`@`9ccee71`) +
> `CRONOGRAMA-ENTREGA.md`, `PLAN.md`. Nota de proyecto: [[vidarq]].

---

## ⚠️ Estado de deploy (leer antes de exponer)

**Producción corre el código del 29-jun (`55af1ea`).** Las **43 commits** de la racha de
integridad/seguridad (2FA, contraseñas fuertes, reverso de movimientos, fix de la race de IDs,
tests del flujo de dinero, Sentry, chequeras, multi-categoría, cierre de periodo, captura
manual, drill-down de Estado de Fondos) están **construidas y probadas en la rama
`fix/integridad-entrega` pero NO deployadas.** El merge/deploy quedó **agendado para
DESPUÉS de la junta** (acción de producción, schema aditivo a BD sin staging → se hace con
calma, no la noche antes). Al exponer, distingue "vivo" de "listo para deploy" — sección 2.

---

## 1. En una frase
El sistema **ya está en producción y en uso** con datos reales (177,642 movimientos
migrados del Access legacy). El núcleo de dinero está en vivo; el **endurecimiento contable
(seguridad, 2FA, tests, reverso, fix de race) está terminado y probado, listo para deployar
esta semana.** Lo que resta depende **de las respuestas de Cintia** y de la limpieza final
de datos. Objetivo de entrega: **fin de semana 4** si Cintia responde a tiempo.

---

## 2. Lo que llevamos (para exponer)

### 2A · Vivo en producción HOY (código del 29-jun, `55af1ea`)
- Sistema **en producción y en uso** con 177,642 movimientos migrados del Access legacy.
- Autenticación real (NextAuth v5) + protección de rutas + invalidación de sesión al cambiar permisos.
- Guardas contables base: **`parseMonto`** (dinero — cerrado el truncamiento de `"10,000"`) y
  **`parseId`** (IDs — cerrado el bug `parseInt("5abc")`).
- Navegación por obra (10 pestañas) + vistas generales + bandejas globales de Cobranza y Autorizaciones.
- **Estado de Fondos** (replica el reporte impreso) + export Excel/PDF.
- **Egresos completo:** reporte + filtros + buscador + export + gráfica + drill-downs (categoría→proveedores→movimientos).
- **Cobranza (F6-A)** e **Inventario de unidades (F6-C)** base.
- Catálogos: obras, clientes, proveedores; filtros de fecha en todo.
- Póliza PDF (Cheque Póliza) con firmas, folio y CFDI.
- Deploy en vivo (repo personal + Vercel Hobby, sin pagar Pro); dato de prueba limpiado
  sin tocar datos reales; fixes post-deploy resueltos (sesión JWT, lentitud, clientes general).

### 2B · Construido, probado y listo — PENDIENTE de deploy (rama `fix/integridad-entrega`, 43 commits)
> ⚠️ Existe y funciona en la rama, **NO está vivo aún.** Merge/deploy agendado post-junta.

**Endurecimiento de seguridad e integridad (núcleo de confianza para ~$500M MXN/obra):**
- **2FA TOTP** obligatorio, con step-up en pagos y reset por admin (`lib/totp.ts`, `lib/step-up.ts`).
- Contraseñas fuertes cifradas (bcrypt) + cambio forzado de clave temporal.
- **Fix de la race condition de IDs de Movimiento** (advisory lock por obra, `lib/locks.ts`) + **tests del flujo de dinero**.
- **Reverso de movimientos** (corregir sin editar/borrar registros verificados).
- Audit log completo + separación de funciones (quien captura ≠ quien verifica; no autorizas tu propia solicitud).
- Permisos por rol global + visibilidad por secciones.
- Sentry (monitoreo), backups verificados (Supabase diario 7d + PITR + pg_dump semanal) + RUNBOOK, CI en GitHub Actions.

**Funcionalidad adicional:**
- Captura manual de movimientos (pedido de Jaime, junta 4).
- **Estado de Fondos dinámico** + drill-down "¿en qué se gastó / de dónde entró?" (pedido de Cintia, junta 5).
- **Conciliación bancaria v1** (banco vs. sistema, captura manual por mes).
- Chequeras, proveedores multi-categoría, cierre de periodo contable, contratos in-app,
  inventario con precioVenta/enganche.

---

## 3. Lo que falta (roadmap hasta la entrega)

**Bloqueado por respuestas de Cintia** → ver sección 5.

**Pasada de seguridad — día de entrega** (ya construido, falta activarlo con el cliente):
- Contraseñas fuertes definitivas para admin y cada usuario (hoy hay de prueba).
- Activar 2FA a fondo con cada usuario real.

**Roadmap pendiente:**
- **F6-B — Alertas de renta vencida** por correo (necesita los correos de clientes + regla de "vencido" + Resend).
- **F8 — Limpieza de datos** (sesión supervisada): ~2,800 orphans, merge de proveedores/cuentas
  duplicados, histórico bloqueado, torres A/B si aplican.
- **F9 — Cierre:** pruebas E2E del flujo de dinero, Cloudflare WAF + dominio (opcional).
- **Conciliación v2:** partidas (comisiones/intereses) + cierre de mes.
- **Capacitación / handoff** al equipo (David, Jaime, Cintia).

---

## 4. Lo que TÚ (Gab) tienes que preguntar / decidir en la junta

**Internas de Gab (decisiones tuyas antes de la entrega):**
- **Timing del merge a producción:** ¿mergeamos la racha de integridad/seguridad a `main`
  y deployamos ya, o después de la junta? (Ver pre-flight en sección 6 — es acción de repo, no del vault.)
- **Dominio + Cloudflare:** ¿se lanza con la URL `*.vercel.app` gratis o se compra dominio?
- **Fecha objetivo de handoff:** confirmar semana 4 con el cliente en la junta.

**Las 6 preguntas para Cintia (mandar/confirmar YA — cada día que tarda corre el deadline):**
1. **Reporte oficial (Estado de Fondos):** ¿solo la cuenta bancaria o también el histórico? ¿Formato exacto?
2. **Conciliación:** ¿cómo se calcula el "saldo en libros"? (hoy: todo menos histórico).
3. **Permisos:** ¿quién puede dar/quitar accesos? ¿hace falta un súper-admin único global?
4. **Inventario de ventas:** ¿precargamos departamentos ya vendidos (histórico) o solo de aquí en adelante?
5. **Alertas de renta:** ¿a partir de qué día se considera "vencido"? ¿con qué correos de clientes?
6. **Torres A/B y montos altos:** ¿qué obras se dividen en torres? ¿desde qué monto se pide doble autorización?

---

## 5. Pendientes que tienen ELLOS (el cliente)

Para no frenar la entrega, el cliente debe:
- [ ] **Responder las 6 preguntas** de arriba (bloquean reporte oficial, conciliación, permisos, alertas y torres).
- [ ] **Entregar los correos de los clientes** (sin ellos F6-B alertas de renta no se puede entregar; se puede entregar sin alertas y sumarlas después).
- [ ] **Definir el formato exacto del reporte oficial** (título, columnas, banco vs. histórico, PDF).
- [ ] **Agendar la sesión supervisada de limpieza de datos** (F8: orphans + merge de duplicados — no es automática, requiere a alguien del cliente presente).
- [ ] **Confirmar asistentes a la capacitación / handoff** (David, Jaime, Cintia).

---

## 6. Pre-flight del merge/deploy — VERIFICADO EN VIVO 2026-07-08 · POST-JUNTA

> **Decisión Gab (2026-07-08):** el merge/deploy se hace **DESPUÉS de la junta**, con calma.
> Hoy NO se toca producción. Runbook copy-paste: **`MERGE-RUNBOOK-vidarq-2026-07-09.md`**
> (actualizar sus números con los de abajo).

**Estado real de git (contado en vivo, resuelve la discrepancia "14 vs 40"):**
- **Producción = `personal/main` @ `55af1ea` (29-jun).** Es literalmente el merge-base:
  **ninguna** de las 43 commits está viva.
- `fix/integridad-entrega` @ `9ccee71` (3-jul): **43 commits adelante** de `main`, **1 commit atrás**
  (solo el doc de CLAUDE.md de hoy). Número real: **43 vs 1**, no "14 vs 40".
- `origin/fix/integridad-entrega` está **desactualizada** (@ `9c51f36`, 30-jun) — la copia buena
  es la local = `personal/fix`. Empujar la local antes de mergear.
- Ramas viejas ya mergeadas en `main` (borrar tras el merge): `feat/egresos-drilldown`,
  `feat/nav-obra-bandejas-seguridad`, `fix/preview-bugs`.

**Riesgos verificados:**
- ✅ **Schema aditivo:** diff `main..fix` en `schema.prisma` = **51 inserciones, 0 borrados**
  (columnas nullable + tabla `ProveedorCategoria`). El `db push` *debería* ser seguro, pero se
  confirma en vivo con `prisma validate` + leer el output del push (regla 10 — parar si avisa data loss).
- 🔴 **`.env.test` NO está en `.gitignore`** (solo `backups/` lo está). Un `git add -A` lo
  commitearía con secretos → **agregarlo a `.gitignore` ANTES de cualquier `git add`.**

**Checklist (en sesión del repo, con tu OK, post-junta):**
1. Agregar `.env.test` a `.gitignore`; confirmar working tree limpio.
2. `git push personal fix/integridad-entrega` (sincronizar la copia buena).
3. Backup fresco de producción ANTES de tocar nada.
4. `npm run build` + tests + `/deploy-validator` → semáforo verde.
5. Merge `fix/integridad-entrega` → `main` (`--no-ff`) → push a `origin` y `personal`.
6. `prisma validate` → `prisma db push` (aditivo) contra prod → verificar deploy en Vercel con reversa.
7. Borrar las 4 ramas ya mergeadas.

---

*Preparado por Gab Agentic OS · cliente=vidarq · para junta 2026-07-09.*
