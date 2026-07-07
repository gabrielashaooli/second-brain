# Gab OS — cómo operar este sistema

## Cómo alimentar tu second brain (lo único que tienes que hacer)

1. **Suelta material crudo en `raw/`** — apuntes, PDFs, exports de chats,
   transcripciones. En la subcarpeta que corresponda, sin formato especial.
   `raw/` nunca se toca después: es tu fuente original.
2. **Pide la digestión** — en el dashboard (Tareas → "Digiere mi maestría" o
   una tarea equivalente) o dile a Claude "digiere lo nuevo de raw/". El
   agente compila el material a notas conectadas en `wiki/`.
3. **Trabaja normal y cierra sesiones** — "cierra la sesión" guarda al
   journal lo que hiciste. Las corridas del Kanban se registran solas.
4. **No organices a mano** — el agente mantiene `wiki/index.md`, el log y los
   wikilinks. Tu trabajo es soltar material y decidir; el suyo es ordenar.

El resto (backup diario a GitHub, poda mensual, estado semanal) corre solo.

Sistema agéntico de Gab sobre Claude Code. Este vault (`second-brain`) es su
memoria única. Cualquier persona con Claude Code instalado puede operarlo.

## Qué es cada cosa

| Capa | Dónde vive | Qué hace |
|---|---|---|
| Memoria | Este vault (`Documents\second-brain`) | Notas por proyecto en `wiki/projects/`, journal diario, reglas en `CLAUDE.md` |
| Skills | `C:\Users\gabri\.claude\skills\` | `project-status`, `session-log`, `maestria-digest` |
| Automatizaciones | Programador de tareas de Windows + `C:\Users\gabri\.claude\gab-os\*.cmd` | Estado semanal (lunes 08:03) y propuesta de poda (día 1, 09:07) |
| Dashboard | `Gab OS.cmd` en el Escritorio → http://127.0.0.1:4747 | v0 observabilidad: proyectos, journal, corridas y contadores en vivo. Botones de acción se desbloquean a 5 usos reales por skill |

## Operación diaria

1. **Abrir sesión:** en una terminal, `cd` al vault (o al repo del proyecto) y
   abre `claude`. El protocolo del `CLAUDE.md` hace que lea el journal
   reciente y la nota del proyecto — no re-expliques contexto.
2. **Trabajar normal.**
3. **Cerrar sesión:** di "cierra la sesión" → corre la skill `session-log` y
   deja registrado qué se hizo, decisiones y siguiente paso.

## Comandos útiles

- "¿Cómo van mis proyectos?" → skill `project-status` (solo lectura de repos).
- "Digiere mis apuntes de la maestría" → skill `maestria-digest`
  (raw/ → wiki/concepts/).
- Ver corridas automáticas: `C:\Users\gabri\.claude\gab-os\scheduler.log`.
- Correr el estado semanal a mano: `schtasks /run /tn "GabOS - Estado semanal"`.

## Reglas que nunca se rompen

Ver sección Prohibiciones del `CLAUDE.md`: no tocar BDs de producción
(Vidarq/Revo), no regenerar logos (Revo/Bow), no contradecir decisiones
registradas, `raw/` inmutable. La poda mensual solo PROPONE borrados
(`journal/poda-YYYY-MM.md`); borrar es decisión humana.

## Mantenimiento

- El día 1 de cada mes aparece `journal/poda-YYYY-MM.md` con candidatos a
  limpiar — revisarlo y ejecutar los borrados a mano.
- Contador de uso de skills en `wiki/projects/gab-os.md`: al llegar a 5 usos
  reales por skill, se habilita instalar el siguiente pack (data, content
  u ops — ya diseñados ahí mismo).
