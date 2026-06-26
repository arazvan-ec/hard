---
name: ranking-improve
description: Meta-loop de auto-mejora de la etapa R3 (comparar y rankear) del AI Radar. Audita la consistencia del ranking y el poder de decisión de la taxonomía — ¿se puntúa igual lo equivalente entre barridos? ¿las dimensiones discriminan? ¿la evidencia de R5 contradice un score estimado? — y mejora el MÉTODO de clasificación (taxonomy.yaml: anclas de puntuación, dimensiones, gate de novedad). Úsala tras varios digests/benchmarks, cuando el usuario quiera "el ranking no es consistente", "afinar la puntuación", "revisar la taxonomía". Es la implementación de la ley RT para R3. NO clasifica hallazgos (eso es radar-scan): mejora cómo se clasifica y puntúa.
---

# Ranking-Improve — meta-loop de R3

Tu trabajo es que el ranking sea **consistente y decisivo**: que dos cosas equivalentes reciban la misma nota en barridos distintos, y que las dimensiones de verdad separen lo bueno de lo malo. Mejoras la taxonomía y la rúbrica, no clasificas hallazgos.

> Regla de decisión fundamental (charter RT): las mejoras se eligen por **lo que hace el ranking más fiel al propósito** (predecir el mejor desarrollo), nunca por lo fácil de puntuar. Una dimensión vale si **discrimina y predice**, no si es cómoda de rellenar.

## Contrato
- **Entrada:** `taxonomy.yaml`, los `frameworks/*.yaml` (scores + `meta.evidence`), los `benchmarks/*.md`.
- **Salida:** mejoras a `taxonomy.yaml` (anclas, dimensiones, gate), entradas en un registro de vueltas.

## Flujo

### 1. Audita consistencia
Revisa si cosas equivalentes tienen scores dispares entre perfiles/digests sin razón. La causa más común: **anclas de puntuación incompletas** (se define qué es un 5 pero no un 1/3), así que cada quien puntúa a ojo.

### 2. Contrasta score vs evidencia (R5)
Donde haya `meta.evidence` de un benchmark, comprueba si el score estimado concuerda con lo medido. Si la evidencia contradice la estimación, el problema puede ser la **definición de la dimensión**, no solo el dato.

### 3. Mejora el MÉTODO (taxonomy.yaml)
- Completa **anclas** de las dimensiones (qué es 1, 3, 5) para que la puntuación sea reproducible.
- Si una dimensión no discrimina (todo el mundo saca lo mismo) o no predice resultado, redefínela o fúndela.
- Afina el `novelty_gate` si deja pasar ruido o frena señal.
- Justifica cada cambio contra la regla: mejora la fidelidad del ranking, no la comodidad.

### 4. Registra y commitea
- Anota la vuelta (qué inconsistencia, qué ancla/dimensión cambió, con qué evidencia).
- Commit `feat: refine taxonomy scoring anchors` (método). Push.

## Anti-patrones
- Retocar scores de perfiles concretos (eso es extract-improve/R5, no aquí: tú mejoras la rúbrica).
- Añadir dimensiones que no discriminan, por parecer completo.
- Cambiar anclas sin evidencia de inconsistencia real.
- Optimizar por facilidad de puntuación en vez de por fidelidad al propósito.
