---
name: radar-scan
description: Ejecuta un barrido extenso de novedades sobre desarrollo con IA recorriendo el registro sources.yaml, clasifica cada hallazgo según taxonomy.yaml y produce un digest fechado en findings/. Úsala SIEMPRE que el usuario quiera buscar lo último/más nuevo en desarrollo con IA, frameworks, prácticas, técnicas, herramientas de agentes o "qué hay nuevo" — frases como "qué ha salido esta semana", "busca novedades", "barrido del radar", "ponme al día de X familia", "novedades en harness/orquestadores/SDD". También al revisar una familia o fuente concreta del registro.
---

# Radar Scan

Barrido extenso + clasificación. Convierte el registro de fuentes en un digest accionable, sin duplicar lo ya conocido.

## Cuándo dispararte

Cuando el usuario quiera lo más nuevo en desarrollo con IA (frameworks, técnicas, herramientas, prácticas), un barrido periódico, o ponerse al día de una familia.

## Antes de empezar
- Confirma el **alcance**: ventana temporal (ej. "última semana/mes"), y si es un barrido completo o de una familia/fuente concreta.
- Lee `sources.yaml` (qué recorrer) y `taxonomy.yaml` (cómo clasificar y el `novelty_gate`).
- Lee el último `findings/YYYY-MM-DD.md` para saber qué ya se reportó (evitar duplicados).

## Flujo

### 1. Selecciona fuentes
Filtra `sources.yaml` por alcance y por `cadence` (no re-escanees a diario lo que es mensual). Prioriza **tier 1** primero; los tier 3 sirven para descubrir, no como verdad.

### 2. Busca (extenso, no superficial)
- Una búsqueda por fuente o tema; no metas varias en una query.
- Incluye el año/fecha actual en las queries de novedad; "latest 2026" rinde mejor que "latest".
- Para una fuente concreta, busca su contenido reciente o haz fetch de su índice.
- Escala las llamadas a la complejidad: un barrido completo justifica muchas búsquedas. No te quedes corto.

### 3. Extrae hallazgos
Por cada cosa relevante, captura: **qué es**, **fuente/enlace**, **fecha**, y la **💡 técnica a robar** si la hay.
- Paráfrasis siempre; nada de copiar texto. Respeta copyright.
- Cita la fuente que sustenta cada afirmación.

### 4. Clasifica (taxonomy.yaml)
Por cada hallazgo:
- **Familia** dominante (una).
- **Tipos de señal** (uno o más).
- Si es framework/herramienta y hay datos, **puntúa** las 7 dimensiones (1–5).

### 5. Pasa el gate de novedad
Descarta lo que no supere `novelty_gate`: ya está en findings/catálogo, no aporta técnica nueva, no cambia el estado de nada. Lo que sobrevive es el digest.

### 6. Escribe el digest
Copia `findings/_TEMPLATE.md` a `findings/YYYY-MM-DD.md` y rellénalo. Ordena por relevancia (lo que cambia decisiones primero). Incluye una línea de "fuentes nuevas detectadas no registradas" → eso alimenta a `source-curation`.

### 7. Realimenta el sistema
- Actualiza `last_checked` de las fuentes barridas en `sources.yaml`.
- Si encontraste fuentes recurrentes no registradas, sugiere ejecutar `source-curation`.
- Si trabajas en git: commits atómicos `findings: add YYYY-MM-DD digest` y `sources: bump last_checked`, push tras cada uno.

## Calibración
- **Profundidad sobre amplitud falsa.** Mejor 8 hallazgos verificados y clasificados que 30 enlaces sin digerir.
- **Sé escéptico** con comparativas de vendors y con temas hype; contrasta con la fuente primaria.
- **Novedad real.** "Salió la v2" cuenta; "este blog re-explica Ralph" no.

## Anti-patrones
- Reportar lo ya reportado (revisa el último digest).
- Quedarte en tier-3 sin ir a la fuente primaria.
- Clasificar a ojo sin abrir `taxonomy.yaml`.
- Inflar el digest con ruido para parecer exhaustivo.
