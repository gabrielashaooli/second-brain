# TAG (The Abadi Group)

**TLDR:** Cliente de [[shift-software]]. **The Abadi Group** es una firma de **diseño de interiores de lujo y construcción** que opera entre **Ciudad de México y Nueva York**. [[sofia]] y Gab le construyen un **sistema de rastreo de muestras** (muestras de material que viajan de bodegas en México a arquitectos/clientes en NY). Pidieron tres sistemas; el de muestras es el urgente y ya tiene **v1 desplegada**.

**Fuente:** `raw/claude-exports/TAG.md` (nota provista por Gab el 2026-07-07 desde su second brain de la cuenta de trabajo; ahí las fuentes originales son sesiones de Claude y archivos de export del trabajo). Corrige la nota-semilla anterior.

> **Corrección de una hipótesis vieja:** la versión previa de esta nota especulaba que TAG podía ligarse a los datos Mixup/Fincado o a los ERP de Tecuán. **Falso** — TAG es una firma de interiorismo de lujo, sin relación con eso. La hipótesis queda descartada.

## Alcance: tres sistemas (en el orden de prioridad de TAG)

1. **Rastreo de muestras (URGENTE)** — el que se construye primero.
2. **Administración y finanzas (en paralelo)** — migrar los Excel de hojas de proyecto / flujo de caja a una base de datos; es el de menor tolerancia al error.
3. **Cotización de cocinas / carpintería (fase 3)** — IA que lee PDFs arquitectónicos y saca takeoff + precio; el más complejo, diferido.

## El proyecto de muestras (Shift)

- Reemplaza un flujo manual fragmentado (Smartsheet + WhatsApp + etiquetas cortadas a mano). Dolor central: en la recepción de EE.UU. no saben qué llegó ni a dónde mandarlo.
- **Contrato a precio fijo ~$75,000 MXN**, 4 fases, 10–12 semanas, hitos de pago 30/30/40. Stack: Supabase/PostgreSQL + React (Claude Code).
- **Modelo de datos:** ID ancla inmutable concatenado (ej. `100-WD-01-HW-64 Fulton` = serie que reinicia en 100 por proyecto + código de material + nombre de proyecto). Las muestras **SPEC** y **ALTERNATIVE** son registros separados que preservan historia.
- **Decisiones de producto confirmadas:** dashboard de pendientes **pull** por usuario (Elías rechazó notificaciones push); **dos fotos obligatorias** por muestra (etiqueta del proveedor + reetiquetada); generación automática de etiqueta + integración con **impresora de etiquetas real**; **dashboard QR de solo lectura** para el cliente, acotado a un proyecto; números de tracking capturados a mano. Fuera de alcance: APIs de WhatsApp/paqueterías.
- **Modelo refinado con sus archivos reales:** los envíos son **consolidados** (un tracking# → muchas muestras) → se modela como entidad `Envío ↔ N muestras`, no 5 columnas fijas de tracking; el estatus general es **derivado** de una bitácora de eventos inmutable (se desincroniza si se lleva a mano); SPEC/ALT son una **cadena versionada** (.1/.2 al reenviar); las fotos tienen 3 capas (registro crudo / presentable / embellecida por IA).
- **Estado (2026-06-29):** **v1 completa y desplegada** (7 pantallas, Supabase ref `emsgftprytzvowigkhlf`); el cliente vio la demo y le gustó; hay un **brief de v2 aprobado y arrancando** ("iterar, no reconstruir": UI en inglés, motor de pasos de ruta libre, dashboard-galería para el cliente, contadores de días).
- Lección reusable del build de la demo (Claude Code reproducía el *slideshow* de Claude Design en vez de una app funcional; se arregló con prompting orientado a comportamiento + rutas reales de React Router): [[demo-vs-functional-prototype]].

## Personas (lado TAG)

Roster confirmado por el seed data del sistema (`scripts/seed.ts`):

- **David Rophie** ("David") — contacto principal, conocido personalmente por [[sofia]]; primo de Elías.
- **Elías Abadi** ("Elías") — **PM / decisor de producto** (definió las decisiones de v2). Login activo.
- **Abraham (TAG)** — lidera **operaciones México / equipo de muestras**; en v2, el **único rol que puede editar/corregir nombres de proveedor** (editor del catálogo). Login activo. *(Ojo: hay un Abraham distinto en [[vidarq]]; no confundir.)*
- **Kitzia** — back-office México, login activo. **Valeria, Fabián** — back-office México (sin login en el seed).
- **Elizabeth** — back-office México (sin login en el seed); más adelante será dueña del **diseño visual de la etiqueta**.
- **Viviana** — **bodega / recepción en Nueva York**: entra por su dashboard de pendientes, recibe, imprime y pega la etiqueta, sube las 2 fotos (frente/reverso), define la siguiente acción; no aprueba nada. Login activo.

## Relaciones

- Cliente de [[shift-software]]. Lo lleva [[sofia]] junto con Gab.
- Build detallado del sistema: [[tag-sample-tracking-system]] (nota por escribir).
- Lección de proceso: [[demo-vs-functional-prototype]] (nota por escribir).
