# Pipeline de entrevistas con AI (trabajo pasado)

**TLDR:** Pipeline en Python que procesa entrevistas en video/audio: descarga el archivo (S3), separa hablantes (diarización) y transcribe con Whisper, y analiza el contenido con Gemini. De un trabajo anterior. Incluye una app web (`studio`) para el flujo.

## Flujo

1. Recibe la URL del video/audio (bucket S3).
2. Extrae audio y hace diarización + transcripción (Whisper, pyannote).
3. Procesa/analiza el texto con Gemini (extractor de insights).
4. Guarda resultados (Firebase/Firestore + Storage).

## Stack

Python, Whisper, pyannote (diarización), Gemini API, Firebase (Firestore + Storage), FFmpeg, AWS S3. Estructura por módulos orquestados desde `main.py`. Front/app en Next.js (`studio`, Firebase Studio).

## Contexto

Empleo anterior. Buen ejemplo de experiencia en pipelines de IA (audio → transcripción → análisis). Referencia para futuros proyectos con Whisper/diarización.

## Preguntas abiertas

- ¿Reutilizable para algún venture propio?

## Fuentes

- Código completo: `C:\Users\gabri\Documents\entrevista_ai` (`main.py`, módulos: whisper_diarizer, pyannote_diarizer, whisper_transcriber, insight_extractor, upload_to_firebase).
- Repo relacionado: `GitHub/pipline` (versión inicial). App: `GitHub/studio` (Next.js).

Relacionadas: [[proyectos-moc]], [[python-para-ciencia-de-datos-fundamentos]]
