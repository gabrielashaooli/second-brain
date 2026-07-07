# Sinai-pedidos

**TLDR:** Sistema de pedidos en producción para **Sinaí**, cliente de [[shift-software]] dedicado a distribución retail de alimentos con tiendas multi-sucursal. Genera PDF/JPG de pedidos por proveedor con Puppeteer y los guarda en Supabase Storage. Cuando Gab dice "el repo de sinai" se refiere a ESTE — no confundir con el `sinai` abandonado del Desktop (recetario Streamlit).

**Fuente:** repo en disco (no vive en `raw/`): `C:\Users\gabri\Documents\GitHub\Sinai-pedidos`. Remoto: github.com/Sofhus/Sinai-pedidos.

## Stack
Next.js + Supabase (bucket `archivos-generados`) + Puppeteer. Deploy: push a main → Vercel.

## Estado (2026-07-06)
En producción. El fix del InvalidKey de Supabase Storage (ñ/acentos en `nombreArchivo`, `app/api/generar/route.ts` — normalizar NFD + strip de diacríticos) quedó commiteado y rebaseado sobre origin/main hoy, así que viaja a Vercel con el push a main. Commit previo (2026-06-23): retry ante errores 529 de la API de Claude.

## Decisiones clave (no contradecir)
- Claves de Supabase Storage siempre ASCII (normalizar NFD + quitar diacríticos antes de subir).
- Deploy es directo a producción (push a main) — no hay staging.

## Casos de uso futuros del OS para Sinaí (del blueprint de Gab, 2026-07-06)
Inspecciones de mantenimiento multi-sucursal, reportes automáticos, predicción de demanda por tienda, optimización de inventarios.

## Relaciones
- Cliente de [[shift-software]]. Mismo patrón Next.js + Supabase que [[revo]] y [[vidarq]].
