# Match-Improve — registro de vueltas

> Memoria del meta-loop de R4. Cada entrada documenta cómo una evidencia de R5 (benchmark)
> mejoró el MÉTODO de framework-fit (cómo puntúa el encaje), no un dato suelto.

---

## Vuelta 1 — 2026-06-26

**Evidencia:** benchmark `2026-06-26-parse-duration` (R5, F2).

**Aprendizaje (factor oculto):** en tareas de corrección crítica, dos procesos pueden empatar
en corrección (spec-first y Ralph, ambos 17/17) y aun así no ser igual de buenos: lo decide
**quién aporta el oráculo de corrección**. Spec-first lo genera; el bucle autónomo lo consume y
sin él se detiene en falso. R4 solo miraba solapamiento de `use_cases` y dimensiones — ignoraba
este factor.

**Cambio de método:** añadido a `skills/framework-fit/SKILL.md` (paso 3, puntuación del encaje)
el criterio "quién aporta el oráculo de corrección", **acotado a historias de corrección
crítica/especificable**, con preferencia por recomendar la combinación (spec-as-artifact +
autonomous-loop).

**Fuerza de la evidencia:** media (un solo benchmark, montaje de un agente con casos conocidos).
→ marcado como aprendizaje aplicado pero **a confirmar** con más benchmarks (idealmente F3 con
herramientas reales) antes de tratarlo como regla dura.

**Verificación (sin sobreajuste):** la recomendación `2026-06-26-greenfield-multiagente` ya
señalaba "falta una pieza de gates/oráculo" — el cambio la vuelve explícita y sistemática, sin
romper otras historias (el criterio no aplica fuera de corrección crítica).

**Pendiente próxima vuelta:**
- Confirmar el factor con un segundo benchmark de clase distinta.
- Cuando exista, contrastar contra una historia de corrección crítica real y ver si R4 ya
  propone la combinación spec-first + loop por sí solo.
