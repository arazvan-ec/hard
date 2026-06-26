# Benchmark — 2026-06-26 · parse_duration (process patterns)

## Tarea de referencia
> Implementar `parse_duration(text) -> int` (segundos) soportando d/h/m/s, varias unidades,
> espacios, mayús/minús, decimales, orden y repetición arbitrarios; y **rechazar** entradas
> inválidas (vacío, número sin unidad, unidad desconocida, basura, signo) con `ValueError`.
> **Done = oráculo objetivo** de 17 casos (12 válidos + 5 inválidos), idéntico para todos.

- Procesos comparados: **baseline ad-hoc** · **spec-first (patrón Spec Kit)** · **bucle autónomo (patrón Ralph)**.
- Entorno: mismo modelo base, misma tarea, mismo oráculo. Cada proceso en su fichero aislado.
- Fase: **F2 (manual)**.

> ⚠️ **Qué es y qué NO es esto.** Es un benchmark de **patrones de proceso** con el modelo
> mantenido constante — NO ejecuta las máquinas empaquetadas (CLI de Spec Kit, BD de Beads,
> bucle de shell de Ralph). Además, un único agente corrió los tres procesos y conocía los
> casos (el baseline se escribió de buena fe en estilo naíf). Por tanto: **evidencia
> direccional sobre el proceso, no un leaderboard de herramientas.** Es lo ejecutable con
> fidelidad en esta sesión; F3 (harness real, multi-agente) sigue siendo el destino.

## Métricas usadas (y por qué sirven al propósito)
- **Corrección** (casos del oráculo que pasan) — la que más pesa: el propósito es el mejor desarrollo, y un parser que acepta basura es incorrecto. No es "fácil de contar elegida por fácil": es la definición de done.
- **Iteraciones al done** — coste real del proceso para llegar a correcto.
- **Calidad/robustez** (rúbrica 1–5) — ¿valida o solo suma lo que matchea?
- **Dependencia de oráculo** (añadida por eval-improve, ver abajo) — ¿el proceso genera sus propios criterios de corrección o necesita que se los den?
- LOC/tiempo: informativos, no deciden.

## Resultados (ejecutados)
| Proceso | Corrección | Calidad (1–5) | Iteraciones | LOC | Dependencia de oráculo |
|---|---|---|---|---|---|
| Baseline ad-hoc | 10/17 (59%) | 2 | 1 | 7 | — (no sabe que falla) |
| **Spec-first** | **17/17 (100%)** | **5** | 1 (0 correcciones) | 19 | **genera el oráculo** |
| Ralph-loop | 17/17 (100%) | 4 | 3 | 17 | **lo consume** (lo necesita) |

Notas cualitativas:
- **Baseline** suma lo que matchea con un regex; rápido pero frágil y, lo peor, **ciego**: devuelve `0` para `"abc"` o `"100"` sin enterarse de que está mal. Sin oráculo, "parece" terminado.
- **Spec-first** enumeró las clases de caso en `SPEC_specfirst.md` antes de programar y acertó 17/17 a la primera; el trabajo previo **produjo los criterios de corrección** como subproducto.
- **Ralph** partió del naíf (10/17) y convergió a 17/17 en 3 vueltas — pero **solo porque existía el oráculo** contra el que iterar. Sin él se habría parado en la iteración 1 creyéndose correcto.

## Veredicto (justificado contra la regla de decisión)
**Para esta clase de tarea (corrección crítica y especificable), gana spec-first.** No por ser
el más fácil — de hecho fue el de **más trabajo previo** (escribir la spec, 19 LOC) — sino por
ser **el más correcto y autosuficiente**: alcanza 100% sin depender de un oráculo externo, y lo
genera. Elegirlo es aplicar la regla (mejor para el propósito sobre facilidad): lo fácil habría
sido el baseline.

**Hallazgo de proceso (lo valioso):** spec-first y Ralph **no compiten, se encadenan**. Ralph
es excelente convergiendo, pero **necesita un oráculo que alguien debe crear** — y eso es lo que
hace spec-first. El proceso óptimo para tarea grande sería: **spec-first define los criterios →
Ralph itera hasta cumplirlos.** El baseline solo gana en velocidad, perdiendo lo único que
importa: saber si está bien.

## Realimentación al sistema
- **Perfiles:** adjuntada evidencia F2 a `spec-kit` (RIG/corrección respaldados empíricamente) y `ralph` (confirma RIG 2: sin gates, depende de oráculo externo). Campo `meta.evidence`.
- **¿Coincide con R4?** Sí y lo afina: la recomendación greenfield ya marcaba "falta una pieza de gates/oráculo". Nota para `match-improve`: en historias de **corrección crítica**, R4 debe **pesar quién aporta el oráculo de corrección**, no solo el solapamiento de use_cases.
- **Cambios de estado:** ninguno.

## Auto-mejora del método (eval-improve)
- Añadida la métrica **"dependencia de oráculo"** al protocolo: una métrica de corrección pura
  habría empatado a spec-first y Ralph (ambos 17/17) y ocultado la diferencia que de verdad
  decide el mejor proceso. Medir solo lo obvio habría engañado → corregido `_PROTOCOL.md`.
- Registrada la **limitación del montaje** (modelo único, agente conoce los casos) como deuda a
  saldar en F3 con agentes/herramientas reales y casos ocultos.

---
*Generado por benchmark (R5), fase F2. Evidencia ejecutada; manda sobre las estimaciones de los perfiles, dentro de los límites declarados.*
