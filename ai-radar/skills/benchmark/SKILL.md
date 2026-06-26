---
name: benchmark
description: Etapa R5 del AI Radar — evaluación empírica. Ejecuta varios frameworks sobre la MISMA historia de usuario/tarea de referencia, mide los resultados con métricas comparables y dictamina cuál es el mejor PROCESO de desarrollo para esa clase de tarea (no el mejor sobre el papel). Úsala cuando el usuario quiera "comparar de verdad", "benchmark", "probar frameworks en una tarea real", "validar la recomendación", "qué proceso da mejor resultado". Es un proceso-loop auto-mejorable que obedece la regla de decisión fundamental del charter. NO recomienda sobre el papel (eso es framework-fit/R4): aporta evidencia ejecutada que valida o corrige a R4 y sube la confianza de los perfiles.
---

# Benchmark — evaluación empírica (R5)

Ejecutas frameworks sobre la **misma tarea** y comparas **resultados reales**. Tu salida es evidencia, no opinión: valida o corrige la recomendación de R4 y sube la confianza de los perfiles con datos medidos.

## Regla de decisión fundamental (del charter — obligatoria)
> Toda elección se decide por **lo mejor para el propósito**, nunca por facilidad/rapidez/coste. El propósito de R5 es **encontrar el proceso que produce el mejor desarrollo**, no el más fácil de medir.

Esto te obliga a, en cada paso:
- Elegir la **tarea de referencia** porque es representativa del problema real, no porque sea pequeña/cómoda.
- Elegir las **métricas** porque predicen calidad real de desarrollo (corrección, mantenibilidad, robustez), no porque sean fáciles de contar (líneas, velocidad bruta). La facilidad de medición es desempate, nunca criterio.
- Dictar el **veredicto** por el mejor resultado para el propósito, aunque el ganador sea el más caro/lento de operar. Si es costoso, se **fasea**, no se descarta.
- Justificar **cada** una de estas decisiones contra la regla en el informe.

## Contrato
- **Entrada:** una historia de usuario + el shortlist de R4 (los frameworks a comparar) + criterios de "done".
- **Salida:** resultados ejecutados + métricas comparables + veredicto, en `benchmarks/YYYY-MM-DD-<slug>.md`; realimentación a los perfiles (`scores`, `confidence`) y a R4.

## Fases (de la regla: se construye lo mejor, por fases, no una versión peor más fácil)
- **F1 — Protocolo.** Define la tarea de referencia, los criterios de done y las métricas en `benchmarks/_PROTOCOL.md`. Sin protocolo no hay benchmark comparable. *(empezamos aquí)*
- **F2 — Benchmark manual.** Ejecuta 2–3 frameworks sobre la tarea a mano, registra resultados con el template. Suficiente para un primer veredicto con evidencia.
- **F3 — Automatizado y repetible.** Harness que ejecuta y mide solo, reproducible. El destino.

## Flujo (un benchmark)

### 1. Fija la tarea de referencia y el done
Una historia de usuario concreta + criterios de aceptación objetivos (tests que deben pasar, requisitos verificables). Misma tarea para todos los candidatos: si la tarea cambia entre frameworks, el benchmark no vale.

### 2. Define las métricas (bajo la regla)
Métricas que reflejen el propósito. Mínimo recomendado:
- **Corrección** — ¿cumple los criterios de done / pasan los tests? (la que más pesa)
- **Calidad/mantenibilidad** — del resultado, evaluada contra rúbrica.
- **Iteraciones** hasta el done.
- **Intervención humana** requerida.
- **Coste** (tokens) y **tiempo** — informativos, NO deciden por sí solos.
Documenta por qué cada métrica sirve al propósito (justificación contra la regla).

### 3. Ejecuta cada candidato igual
Mismas condiciones, mismo prompt inicial/spec, mismo entorno. Aísla (worktree/branch por framework) para no contaminar. Registra todo lo que pasó, no solo el número final.

### 4. Compara y dictamina
Tabla candidatos × métricas. Veredicto por **mejor resultado para el propósito** (corrección y calidad mandan; coste/tiempo desempatan o se fasean). Explica el porqué.

### 5. Realimenta el sistema
- Sube/ajusta `scores` y `meta.confidence` de los perfiles con la evidencia medida (un dato empírico vale más que una estimación).
- Compara el veredicto con lo que había recomendado R4 → si no coincide, es señal para `match-improve` (mejorar cómo R4 puntúa el encaje).
- Anota cambios de estado si los hubo.

### 6. Auto-mejora del método (meta-loop `eval-improve`)
Tras el benchmark, pregunta: **¿alguna métrica no predijo la calidad real?** ¿La tarea de referencia era representativa? Si una métrica engañó (p. ej. el más rápido dio el peor código), **corrige el protocolo** (`_PROTOCOL.md`) para la próxima vez — bajo la regla, no para simplificar, sino para medir mejor el propósito. Registra la mejora.

### 7. Guarda y commitea
- `benchmarks/YYYY-MM-DD-<slug>.md` con el informe completo (incluida la justificación de cada decisión contra la regla).
- Commits atómicos: `benchmark: add <slug>`, y aparte `frameworks: update scores from benchmark <slug>` y `skill: improve benchmark protocol` si tocaste el método. Push tras cada uno.

## Anti-patrones (todos violan la regla de decisión)
- Elegir una tarea de juguete porque es fácil de montar.
- Premiar velocidad/coste por encima de corrección y calidad.
- Cambiar la tarea entre frameworks para que "quepa" en alguno.
- Declarar ganador al más fácil de operar cuando otro da mejor desarrollo.
- Medir solo lo fácil de contar y llamarlo evaluación.
