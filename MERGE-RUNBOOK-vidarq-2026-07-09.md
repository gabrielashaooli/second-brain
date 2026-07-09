# Runbook — merge a `main` + limpieza de branches + deploy · Vidarq

> **APROBADO por Gab** (respuesta "ok", 2026-07-08) — pero es acción de PRODUCCIÓN
> (checkpoints 3 y 5: datos de prod + deploy). **NO se ejecuta desde el vault.**
> Se corre en una **sesión abierta en `C:\Users\gabri\Desktop\vidarq`**, con Gab
> presente, idealmente **hoy en la noche** para llegar a la junta con todo en `main`.
> Este archivo es la guía copy-paste; cada bloque tiene su gate. Si un gate falla, **para**.

---

## Gate 0 — sanidad del working tree
```powershell
cd C:\Users\gabri\Desktop\vidarq
git status -sb
git branch -vv
```
- [ ] Confirmar rama actual y que no haya cambios sin commitear que no quieras. Si hay
      cambios sueltos (p.ej. en `.claude/`), commitéalos o descártalos antes de seguir.
- [ ] **Verificar `.gitignore` cubre `.env.test` y `backups/`** (que NO entren al commit ni al remoto):
```powershell
git check-ignore .env.test backups/   # debe imprimir ambas rutas = están ignoradas
```
  Si NO las imprime → **PARA**, agrégalas a `.gitignore` antes de cualquier `git add`.

## Gate 1 — resolver la discrepancia de commits (14 vs ~40) y ver qué entra
```powershell
git fetch --all --prune
git rev-list --count main..fix/integridad-entrega    # nº real de commits que entran
git log --oneline main..fix/integridad-entrega        # el contenido exacto
git diff --stat main..fix/integridad-entrega          # archivos tocados
```
- [ ] Cuadrar el número real (esto cierra la discrepancia del vault) y revisar que el
      diff sea SOLO la racha de integridad/seguridad esperada (2FA, Sentry, audit log,
      CI, contraseñas fuertes, permisos, catálogos, drill-downs, captura manual).
- [ ] ¿Hay cambios de **schema Prisma**? Míralo explícito:
```powershell
git diff --stat main..fix/integridad-entrega -- prisma/
```
  Si hay → habrá `prisma db push` contra prod (Gate 4). Si NO → te saltas ese paso.

## Gate 2 — pre-flight verde (build + tests + validador)
```powershell
npm ci
npm run build
npm test        # o el script de tests del flujo de dinero que use el repo
```
- [ ] Luego corre el pre-flight de deploy del OS: **skill `deploy-validator`** (Vidarq).
      Debe quedar en **verde** (env vars, build, reglas de push del proyecto).
- [ ] Si algo sale rojo → **PARA** y arréglalo antes de mergear. No se deploya en rojo.

## Gate 3 — BACKUP FRESCO antes de tocar prod (irreversible sin esto)
- [ ] Backup verificado de la BD de producción **ahora** (Supabase / `pg_dump` semanal +
      confirmar PITR activo — sigue el RUNBOOK del repo). Anota timestamp del backup.
      **Sin backup fresco confirmado, no pases de aquí.**

## Gate 4 — merge + push al remoto que Vercel observa
```powershell
git checkout main
git pull --ff-only
git merge --no-ff fix/integridad-entrega -m "Merge integridad/seguridad para entrega (junta 2026-07-09)"
# push al remoto PERSONAL (el que Vercel deploya) — nombre real del remoto según el repo:
git push personal main
# y al remoto de la org si aplica:
git push origin main
```
- [ ] ⚠️ El push a `personal/main` **dispara el deploy en Vercel automáticamente.**
      Verifica el nombre real del remoto personal con `git remote -v` antes.

## Gate 5 — migración de schema a prod (SOLO si Gate 1 mostró cambios en `prisma/`)
```powershell
npx prisma db push        # con el backup del Gate 3 ya hecho
```
- [ ] Confirmar que aplicó sin drift ni pérdida. Si el diff de schema es riesgoso
      (drops de columnas/tablas), **revísalo con Gab** antes de correrlo.
- [ ] Confirmar env vars de Sentry / prod en Vercel si la racha las agregó.

## Gate 6 — verificar el deploy vivo
- [ ] Abrir la URL de producción, login, y humo del flujo de dinero (una autorización,
      Estado de Fondos, un drill-down). Revisar Sentry por errores nuevos post-deploy.
- [ ] Si algo se rompe: **reversa** → `git revert` del merge + re-deploy, o rollback de
      deployment en Vercel. (Por eso el backup del Gate 3.)

## Gate 7 — limpieza de branches (solo tras deploy sano)
```powershell
git branch --merged main          # ver qué ya está mergeado y es seguro borrar
git branch -d fix/integridad-entrega
git push personal --delete fix/integridad-entrega   # y origin si aplica
git fetch --all --prune
git branch -a                     # confirmar el estado final limpio
```
- [ ] Borrar SOLO ramas ya mergeadas a `main`. No borres ramas con trabajo no integrado.

---

## Al cerrar (de vuelta, puede ser desde el vault)
- [ ] Actualizar nota `wiki/projects/vidarq.md`: estado → "racha de integridad **mergeada
      a main y en producción** al 2026-07-09; ramas limpias". Quitar el "aún sin mergear".
- [ ] Journal `journal/2026-07-09.md`: session-log del merge (nº real de commits, si hubo
      `db push`, resultado del humo). Cerrar la discrepancia 14-vs-40.
- [ ] Marcar en `JUNTA-Vidarq-2026-07-09.md` §6 que el pre-flight ya se ejecutó.

*Preparado por Gab Agentic OS · cliente=vidarq · handoff de ejecución para sesión del repo.*
