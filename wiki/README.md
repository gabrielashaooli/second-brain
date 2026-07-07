# wiki/ — Conocimiento compilado

Dominio de **Claude**. Aquí vive el conocimiento destilado a partir de `raw/`.

## Reglas

- Claude crea y mantiene estas páginas. Cada cambio actualiza `index.md`
  (catálogo + TLDR) y añade una entrada al final de `log.md`.
- Cada nota empieza con un TLDR de 1–2 frases y cita su(s) fuente(s) de `raw/`.
- Conectar notas con `[[wikilinks]]` para alimentar el graph view de Obsidian.
- Las contradicciones entre fuentes se anotan explícitamente, nunca se
  sobrescriben en silencio.

## Estructura

| Archivo / carpeta | Contenido                                          |
| ----------------- | -------------------------------------------------- |
| `index.md`        | Catálogo maestro de todas las páginas, con TLDRs.  |
| `log.md`          | Registro append-only de ingestas y consultas.      |
| `concepts/`       | Notas de conceptos y temas.                        |
| `projects/`       | Notas por proyecto.                                |
| `people/`         | Dossiers de personas.                              |
