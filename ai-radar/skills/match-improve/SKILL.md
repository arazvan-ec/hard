---
name: match-improve
description: Meta-loop de auto-mejora de la etapa R4 (consultar y recomendar) del AI Radar. Compara las recomendaciones pasadas de framework-fit contra los veredictos empíricos de R5 (benchmarks/), detecta divergencias o factores que R4 ignoraba, y mejora el MÉTODO de framework-fit (cómo puntúa el encaje) para que las próximas recomendaciones sean mejores. Úsala tras un benchmark de R5, cuando el usuario quiera "que las recomendaciones aprendan de la evidencia", "afinar el recomendador", "por qué R4 no acertó", o periódicamente. Es la implementación de la ley RT para R4. NO recomienda (eso es framework-fit) ni evalúa (eso es benchmark): cierra el lazo evidencia→recomendación.
---

# Match-Improve — meta-loop de R4

Tu trabajo es que el recomendador **aprenda de la realidad**. Cada vez que R5 ejecuta un benchmark, comparas su veredicto con lo que R4 habría recomendado y, si hay algo que aprender, **reescribes el método de `framework-fit`** (sus criterios de puntuación), no un dato suelto.

> Regla de decisión fundamental (charter RT): los cambios al método se eligen por **lo que hace mejores las recomendaciones para su propósito**, nunca por lo fácil de codificar. No añadas un factor porque sea medible; añádelo porque **predice el mejor resultado de desarrollo**.

## Contrato
- **Entrada:** `recommendations/*.md` (lo que R4 dijo), `benchmarks/*.md` (lo que R5 demostró), `frameworks/*.yaml`.
- **Salida:** mejoras a `skills/framework-fit/SKILL.md` (método) y/o a la rúbrica de puntuación; entradas en `recommendations/_IMPROVE_LOG.md`.

## Flujo

### 1. Empareja recomendación ↔ evidencia
Por cada benchmark de R5, localiza la recomendación (o la clase de historia) comparable. ¿Qué dijo R4? ¿Qué demostró R5?

### 2. Detecta el aprendizaje
Busca una de tres cosas:
- **Divergencia:** R5 coronó a un proceso que R4 no recomendó → ¿qué factor lo decidió que R4 no pesó?
- **Factor oculto:** R5 reveló una dimensión que decide el resultado y que el método de R4 ignora (ej. F2: "quién aporta el oráculo de corrección").
- **Confirmación útil:** R4 acertó, pero por un motivo que conviene volver explícito en el método.

### 3. Mejora el MÉTODO de framework-fit (lo central)
Traduce el aprendizaje a un cambio en `skills/framework-fit/SKILL.md`:
- Añade/ajusta un criterio en el paso de puntuación del encaje.
- Acótalo a la **clase de historia** donde aplica (no globalices un aprendizaje de un solo caso).
- Justifícalo contra la regla: por qué hace mejores las recomendaciones, no por qué es fácil.

### 4. Verifica (sin sobreajustar)
Re-corre mentalmente la recomendación afectada con el método nuevo: ¿mejora sin romper otras? Cuidado con **sobreajustar a un único benchmark**: un aprendizaje necesita o evidencia fuerte o repetirse antes de volverse regla dura. Si es débil, déjalo como "heurística tentativa" y márcalo para confirmar con más benchmarks.

### 5. Registra y commitea
- Entrada en `recommendations/_IMPROVE_LOG.md`: qué benchmark, qué aprendizaje, qué cambió en el método, fuerza de la evidencia.
- Commit `skill: improve framework-fit — <aprendizaje>` + push.

## Anti-patrones
- Sobreajustar el recomendador a un solo benchmark (regla dura sobre evidencia de un caso).
- Cambiar un dato del perfil en vez del método (eso es extract-improve; aquí se mejora *cómo se recomienda*).
- Añadir un factor por ser medible aunque no prediga mejor desarrollo (viola la regla).
- Globalizar un aprendizaje que solo aplica a una clase de historia.
