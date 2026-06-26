# Protocolo de evaluación empírica (R5) — v0

> Cómo se hace un benchmark justo y comparable en el AI Radar. Este documento es el **método**; el meta-loop `eval-improve` lo mejora cuando una métrica no predice calidad real. Toda decisión aquí se justifica contra la **regla de decisión fundamental** (charter): lo mejor para el propósito, nunca lo más fácil.

## Propósito
Encontrar **qué proceso de desarrollo con IA produce el mejor resultado** para una clase de tarea — con evidencia ejecutada, no estimaciones.

## Diseño de un benchmark

### Tarea de referencia
- Una historia de usuario concreta + criterios de aceptación **objetivos** (tests que deben pasar, requisitos verificables).
- **Representativa del problema real**, no de juguete. (Regla: se elige por representatividad, no por comodidad de montaje.)
- **Idéntica para todos los candidatos.** Si cambia entre frameworks, el benchmark se invalida.
- Mismo entorno, mismo modelo base, mismo prompt/spec inicial. Cada framework se ejecuta aislado (worktree/branch propio).

### Métricas (ordenadas por peso para el propósito)
| Métrica | Qué mide | Peso | Cómo se obtiene |
|---|---|---|---|
| **Corrección** | ¿pasa los criterios de done / los tests? | **alto** | tests automáticos / checklist objetivo |
| **Calidad / mantenibilidad** | legibilidad, estructura, robustez del resultado | **alto** | rúbrica (1–5) revisada |
| **Iteraciones al done** | nº de vueltas hasta cumplir | medio | conteo |
| **Intervención humana** | cuánta ayuda hizo falta | medio | conteo/clasificación |
| **Dependencia de oráculo** | ¿el proceso genera sus criterios de corrección o necesita que se los den? | alto | análisis del proceso |
| **Coste (tokens)** | gasto | informativo | telemetría |
| **Tiempo** | duración | informativo | reloj |

> La métrica "dependencia de oráculo" se añadió en el benchmark `2026-06-26-parse-duration`:
> sin ella, dos procesos con 100% de corrección empatan y se oculta cuál es realmente el mejor
> proceso (el que produce el oráculo vs el que lo consume).

> Coste y tiempo **no deciden por sí solos**: son desempate o input para fasear. Un proceso que da mejor desarrollo pero es más caro **gana** y se fasea su adopción.

### Veredicto
Gana el **mejor resultado para el propósito**: corrección y calidad mandan; iteraciones/intervención afinan; coste/tiempo desempatan. El informe debe **justificar el veredicto contra la regla**.

## Salida
Un fichero `benchmarks/YYYY-MM-DD-<slug>.md` (ver `_TEMPLATE.md`) + realimentación a `frameworks/*.yaml` (scores, confidence) y a R4 (match-improve si el veredicto difiere de la recomendación).

## Estado de fases
- **F1 protocolo** — ✅ este documento (v0).
- **F2 benchmark manual** — ✅ primer caso: `2026-06-26-parse-duration` (process patterns).
- **F3 automatizado/repetible** — ⬜ destino (agentes/herramientas reales, casos ocultos, multi-agente).

## Mejoras del protocolo (las registra `eval-improve`)
- **2026-06-26** — añadida la métrica **"dependencia de oráculo"** (un empate en corrección pura
  ocultaba la diferencia que decide el mejor proceso). Registrada limitación de los benchmarks
  de un solo agente con casos conocidos → saldar en F3.
