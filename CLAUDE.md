# CLAUDE.md — AI Radar

Este repo es **AI Radar**: un sistema para encontrar, estructurar y comparar lo más nuevo del
desarrollo con IA, y **recomendar el mejor framework/proceso ante una historia de usuario** —
con la propiedad de que **mejora solo en cada ejecución**. Todo vive bajo `ai-radar/`.

> **Fuente de verdad del propósito:** `ai-radar/RESPONSABILIDADES.md` (el charter). Léelo antes
> de cambios estructurales. Cualquier cambio debe poder mapearse a una responsabilidad (R1–R5) o
> a la ley transversal (RT); si no encaja, actualiza el charter **primero**.

## Regla de decisión fundamental (gobierna TODO, incluidas tus recomendaciones al usuario)
> Toda elección —qué mejorar, construir, priorizar o recomendar— se decide por **lo mejor para
> el propósito del proceso**, nunca por su facilidad, rapidez o coste. La facilidad solo
> desempata. Si lo mejor es costoso, se elige igual y se **fasea**. Justifica cada decisión
> contra esta regla.

## Las 5 etapas (stage) + su auto-mejora (meta-loop)

| Resp. | Hace | Skill (stage) | Meta-loop (RT) |
|---|---|---|---|
| R1 | busca y vetea fuentes | `source-curation` | `source-curation-improve` |
| R2 | extrae a datos estructurados (perfiles) | `radar-scan` | `extract-improve` |
| R3 | compara y rankea | `taxonomy.yaml` | `ranking-improve` |
| R4 | recomienda ante una historia | `framework-fit` | `match-improve` |
| R5 | evalúa empíricamente (ejecuta y mide) | `benchmark` | `eval-improve` (interno) |

Los **stages** hacen el trabajo; los **meta-loops** mejoran el *método* de su etapa (no solo el
dato) y dejan registro. Cada vuelta debe dejar al sistema mejor que antes.

## Capacidades transversales (cruzan las 5 etapas)
| Capacidad | Skill | Meta-loop |
|---|---|---|
| presentar datos en HTML distópico | `dystopian-render` | `render-improve` (evoluciona `site/_DESIGN.md`) |

## Principio de crecimiento del sistema (ley)
> Siempre que aparezca un **pensamiento o divergencia recurrente** —una capacidad nueva que el
> proceso necesita (presentar, validar, exportar, lo que sea)— se convierte en un **skill unitario
> y auto-evolutivo** con su propio meta-loop, no en código ad-hoc. Cada capacidad cumple los
> requisitos de la feature principal y crece sola dentro del proceso completo. El sistema se
> extiende creando loops, no parches.

## Cómo interactuar (puertas de entrada)
- **Recomendar:** "Tengo esta historia de usuario: '…'. ¿Qué framework me conviene?" → `framework-fit`
- **Novedades:** "Haz un barrido de la última semana" → `radar-scan`
- **Comparar de verdad:** "Compara spec-first vs Ralph en esta tarea con tests" → `benchmark`
- **Ampliar fuentes:** "Encuentra curadores nuevos de harness engineering" → `source-curation`
- **Afinar el sistema:** "Mejora los perfiles" / "audita las fuentes" / "afina la puntuación" /
  "que las recomendaciones aprendan del último benchmark" → los `*-improve`
- **Ver en HTML:** "Renderiza esto / muéstramelo en una página distópica" → `dystopian-render`

Las skills están en `ai-radar/skills/` y enlazadas en `.claude/skills/` para que se descubran
solas (no edites los symlinks; edita el original en `ai-radar/skills/`).

## Dónde está cada cosa
```
ai-radar/
  RESPONSABILIDADES.md   charter (propósito + regla + responsabilidades)
  sources.yaml           registro de fuentes (R1)
  taxonomy.yaml          familias, señales, dimensiones con anclas 1/3/5 (R3)
  frameworks/            perfiles estructurados (R2) + _SCHEMA.yaml + _IMPROVE_LOG.md
  findings/              digests de barridos (R2/R3)
  recommendations/       salidas de framework-fit (R4) + _IMPROVE_LOG.md
  benchmarks/            evaluaciones empíricas (R5) + _PROTOCOL.md
  skills/                las 8 skills (5 stages + meta-loops)
  site/                  visor HTML de los digests
```

## Convenciones
- **Commits atómicos**, uno por cambio lógico, con prefijo: `sources:`, `findings:`,
  `frameworks:`, `recommendations:`, `benchmark:`, `skill:`, `docs:`, `feat:`, `fix:`.
- **Fuentes primarias > agregadores.** Nada de datos sin verificar; si no lo verificas, no entra.
- **Señal, no ruido.** Si un hallazgo no cambia una decisión, no entra.
- Los ficheros `_SCHEMA`, `_PROTOCOL`, `_TEMPLATE`, `_IMPROVE_LOG` son método/memoria del sistema:
  respétalos y actualízalos cuando un meta-loop mejore una etapa.
