# LLM Wiki — Esquema y reglas

Sistema de wiki personal estilo Karpathy. Las fuentes crudas entran a `raw/`;
el conocimiento compilado vive en `wiki/`.

## Estructura

```
raw/                 # fuentes crudas, inmutables (las mete el usuario)
wiki/                # conocimiento compilado (lo escribe y mantiene Claude)
  concepts/          # notas de conceptos (temas, ideas, métodos)
  projects/          # notas de proyectos (trabajo, materias, entregas)
  people/            # notas de personas (compañeros, jefes, profesores)
  index.md           # catálogo maestro con TLDRs
  log.md             # changelog append-only de ingestas/consultas
CLAUDE.md            # este archivo
```

## Reglas

1. **`raw/` es inmutable.** Nunca se modifica, renombra ni borra su contenido.
   Es la fuente de verdad original.
2. **Claude es dueño de `wiki/`.** Toda nota compilada vive aquí. En cada cambio
   se actualizan `index.md` (catálogo + TLDR) y `log.md` (entrada nueva al final).
3. **Wikilinks.** Usar `[[wikilink]]` para conectar notas y alimentar el graph
   view de Obsidian. Los nombres de nota son la clave del enlace.
4. **Contradicciones.** Si dos fuentes se contradicen, se anota explícitamente la
   discrepancia (citando ambas fuentes de `raw/`). Nunca se sobrescribe en
   silencio.

## Convenciones de nota

- Cada nota `wiki/` empieza con un TLDR de 1–2 frases.
- Cita la(s) fuente(s) de `raw/` que la respaldan. Excepción: las notas de
  `wiki/projects/` pueden citar repos/carpetas en disco como fuente (los repos
  son la fuente viva; no se copian a `raw/`).
- Concepto → `wiki/concepts/`. Proyecto → `wiki/projects/`. Persona → `wiki/people/`.

---

# Gab OS — capa de memoria

Este vault es la capa de memoria de **Gab OS** (ver `wiki/projects/gab-os.md`).
Las secciones anteriores siguen vigentes; lo de abajo las extiende.

## Quién es Gab

Gabriela Shaooli. Solo-founder y desarrolladora full-stack (Next.js/Supabase/
Python), cursando la maestría MIACD (ciencia de datos/IA). Trabaja en
Windows 11 + PowerShell.

## Entidades y clientes (mapa de negocio)

- **[[shift-software]]** — su consultora (shiftsoftware.com.mx), paraguas de
  los proyectos de cliente: [[vidarq]] (gestión de propiedades/condominios),
  [[sinai-pedidos]] (Sinaí: distribución retail de alimentos multi-sucursal)
  y [[tag]] (The Abadi Group: Muestras, Administración, Cocinas).
- **Productos propios:** [[revo]] (cashback MX) y [[bow-beauty]] (en pausa).
- **Personal:** [[maestria-miacd]] y [[ops-personales]].

Al registrar trabajo, SIEMPRE identificar a qué entidad pertenece (tag
`#revo #vidarq #sinai #tag #shift #bow #maestria #personal`).

## Proyectos activos

Una nota por dominio en `wiki/projects/`: `revo.md`, `vidarq.md`,
`bow-beauty.md`, `maestria-miacd.md`, `ops-personales.md`, `gab-os.md`.
Cada nota lleva rutas en disco, estado y decisiones que no se contradicen.

## Protocolo de sesión

**Al iniciar una sesión de trabajo:**
1. Lee las últimas 2–3 notas de `journal/` para saber dónde quedó todo.
2. Lee la nota de `wiki/projects/` del dominio en el que se va a trabajar.
3. No pidas a Gab que re-explique contexto que ya está en el vault.

**Al cerrar una sesión de trabajo:**
1. Escribe (o agrega a) la nota diaria `journal/YYYY-MM-DD.md`: qué se hizo,
   decisiones tomadas, siguiente paso concreto. Con wikilinks a los proyectos.
2. Si cambió el estado o hubo una decisión nueva, actualiza la nota del
   proyecto en `wiki/projects/`.
3. Actualiza `wiki/index.md` y agrega línea a `wiki/log.md` si tocaste `wiki/`.

## Memoria única

Los hechos de negocio, decisiones y estado de proyectos viven SOLO en este
vault. No guardarlos en la memoria interna de Claude Code — evita dos
versiones de la verdad. La memoria interna solo puede apuntar HACIA el vault.

## Prohibiciones

1. **Nunca tocar las BD de producción** de [[vidarq]] (Supabase, sin staging)
   ni de [[revo]] (Railway/Supabase) desde una sesión del vault. El trabajo de
   código se hace en sesiones abiertas en sus repos, con sus propias reglas.
2. **Nunca regenerar logos ni identidad** de Revo o Bow Beauty — los assets ya
   existen y son canónicos.
3. **Nunca contradecir decisiones registradas** en `wiki/` ni en los ADRs de
   los repos. Si una fuente nueva contradice algo, se anota la discrepancia
   (regla 4 de arriba), no se sobrescribe.
4. `raw/` sigue siendo inmutable.
5. Los proyectos abandonados (mi-gpt, netflix-recomendador, sinai, tickets,
   yam, ml-analyzer, entrevista_ai) no entran al OS ni al vault.

## Niveles de autonomía (por defecto: co-pilot)

- **Suggest** — solo recomendar, no ejecutar. Para tareas nuevas, críticas o
  de alto riesgo (equivale a plan mode de Claude Code).
- **Co-pilot** — ejecutar lo rutinario; pedir aprobación para todo lo de la
  lista de checkpoints de abajo. ESTE es el modo por defecto de Gab OS.
- **Autopilot** — solo para tareas repetitivas de bajo riesgo ya probadas
  (las rutinas programadas del OS operan aquí); pausar ante anomalías.

## Checkpoints humanos (aprobación explícita SIEMPRE, en cualquier modo)

1. Comunicaciones externas (emails, mensajes, posts públicos).
2. Transacciones financieras o cambios de precios/ofertas visibles a usuarios.
3. Modificar/eliminar datos de producción (Vidarq, Revo, Sinaí).
4. Acciones irreversibles sin undo fácil.
5. Deploys y cambios de infraestructura o configuración.

Nunca ocultar errores ni limitaciones; comunicar estado real
(éxito / parcial / fallo) y ofrecer camino de reversa cuando exista.

## Mantenimiento

Poda mensual (primer fin de semana del mes): borrar notas huérfanas, archivar
journal de +60 días si no aporta, revisar que `index.md` refleje la realidad,
y marcar skills de Gab OS sin uso para eliminarlas.
