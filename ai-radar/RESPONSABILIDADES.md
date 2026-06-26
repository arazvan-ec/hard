# Responsabilidades del repo — AI Radar

> Documento-charter. Define **qué tiene que hacer** este repo y **bajo qué ley arquitectónica** lo hace. Es la fuente de verdad sobre el propósito; todo lo demás (skills, taxonomía, registro, datos, digests) son implementaciones de estas responsabilidades y se juzgan por cuánto las cumplen.

## Propósito

Encontrar, estructurar y comparar **lo más nuevo del desarrollo con IA** (repos, técnicas, frameworks, prácticas) para poder **recomendar, ante una historia de usuario concreta, el mejor framework o proceso** — y, en su grado máximo, **demostrarlo ejecutando y midiendo resultados reales**. Todo ello mediante un sistema que **mejora solo en cada ejecución**.

> Principio rector heredado: **robar las técnicas, no instalar los sistemas.** Destilamos señal accionable, no acumulamos enlaces.

El norte no es un boletín de novedades: es un **motor de comparación y recomendación**. Le pasas una historia de usuario y te dice qué te conviene, con comparativa, justificación y —cuando esté maduro— evidencia empírica.

---

## Mapa de responsabilidades

Cinco son **operativas** (etapas del flujo, R1→R5). Una es **transversal** (RT): una ley que gobierna a todas las demás y a sí misma.

```
   R1 ──► R2 ──► R3 ──► R4 ──► R5
 fuentes  datos  comparar consulta  evaluar
          estruct. /rank  /recomendar empírico
     ▲                                   │
     └──────── realimenta el registro ───┘

   RT · auto-mejora (ley): cada etapa tiene su propio meta-loop que la reescribe
```

### R1 — Buscar fuentes
**Qué:** descubrir de dónde sacar repos e información sobre nuevos desarrollos con IA, vetearlos por señal y mantener un registro vivo.
- **Contrato — entrada:** huecos conocidos, fuentes existentes, señales de barridos previos. **Salida:** entradas nuevas/mejoradas en `sources.yaml` con metadatos honestos.
- **Implementación:** skill `source-curation` + `sources.yaml`.
- **Métrica:** cobertura (% de hallazgos valiosos que vinieron del registro) y ratio señal/ruido del registro.
- **Meta-loop:** `source-curation-improve`.

### R2 — Extraer a datos estructurados
**Qué:** de cada fuente/hallazgo, sacar el máximo de información útil **en forma estructurada y comparable** — un perfil por framework, no prosa. Incluye la **técnica reutilizable** ("idea a robar").
- **Contrato — entrada:** fuentes seleccionadas de `sources.yaml` para un alcance dado. **Salida:** perfiles `frameworks/<id>.yaml` con atributos comparables (familia, casos de uso, gates, preserva-tests, lock-in, madurez, dimensiones de puntuación…) + hallazgos trazables (cada afirmación con su fuente, parafraseado).
- **Implementación:** skill `radar-scan` (extracción) → emite a un esquema de perfil (pendiente de definir).
- **Métrica:** profundidad y completitud del perfil (campos rellenos, fuente primaria alcanzada) sin sacrificar veracidad.
- **Meta-loop:** `extract-improve`.

### R3 — Comparar y rankear
**Qué:** sobre los datos estructurados, construir **filtros y comparativas** y un ranking comparable y justificado. El ranking es **consultable**, no una lista plana.
- **Contrato — entrada:** perfiles `frameworks/<id>.yaml`. **Salida:** comparativas (tablas por dimensión/atributo) y un catálogo maestro rankeado.
- **Implementación:** `taxonomy.yaml` (familias, señales, dimensiones, gate de novedad) + catálogo (pendiente).
- **Métrica:** consistencia del ranking entre barridos y poder de decisión (¿el top cambia lo que hacemos?).
- **Meta-loop:** `ranking-improve`.

### R4 — Consultar y recomendar
**Qué:** dada una **historia de usuario**, filtrar los frameworks por sus atributos, producir una comparativa de los candidatos y **recomendar el mejor** con justificación y avisos.
- **Contrato — entrada:** una historia de usuario + restricciones (lenguaje, stack, gates exigidos…). **Salida:** shortlist + comparativa + recomendación razonada con caveats.
- **Implementación:** skill `framework-fit` / `radar-match` (pendiente).
- **Métrica:** acierto de la recomendación (validada contra resultado real cuando R5 exista) y trazabilidad (la recomendación cita los atributos/datos que la sostienen).
- **Meta-loop:** `match-improve`.
- *Ejemplo:* "Como dev, quiero portar un servicio de Python a Go conservando los tests" → filtra por porting / loop autónomo / spec-anclada / preserva-tests → shortlist {Ralph, Spec Kit…} → comparativa + recomendación.

### R5 — Evaluar empíricamente *(norte; por fases)*
**Qué:** **ejecutar de verdad** varios frameworks sobre la misma historia de usuario, medir los resultados y compararlos para **encontrar el mejor proceso** — no solo el mejor sobre el papel.
- **Contrato — entrada:** una historia de usuario + el shortlist de R4. **Salida:** resultados ejecutados + métricas comparables (calidad, nº de iteraciones, coste, tiempo) + veredicto.
- **Implementación:** harness de benchmarking (pendiente; la pieza más pesada).
- **Métrica:** reproducibilidad del benchmark y correlación entre la recomendación de R4 y el ganador empírico.
- **Meta-loop:** `eval-improve`.
- **Fases:** F1 datos-only (R1–R4 ya recomiendan); F2 benchmark manual de 2–3 frameworks sobre una historia; F3 benchmark automatizado y repetible.

### RT — Auto-mejora *(ley transversal)*
**Qué:** toda etapa (incluida esta) es un **proceso abstracto** con tres propiedades:

1. **Contrato explícito.** Entradas y salidas como interfaz estable. Otra etapa depende del contrato, nunca del interior.
2. **Evolución independiente.** Se mejora una etapa sin tocar las demás.
3. **Auto-mejora unitaria.** Cada flujo **sabe mejorarse a sí mismo**: su meta-loop (`<etapa>-improve`) observa sus resultados, detecta dónde falló respecto a su propósito y **reescribe su propio método** (su SKILL.md, criterios, queries o esquema), con evidencia del barrido que lo motiva.

**Regla de decisión fundamental — gobierna a TODO meta-loop y a cualquier recomendación del sistema (incluidas las del agente al usuario):**
> Toda elección —qué mejorar, qué construir, qué priorizar, qué recomendar— se decide por **lo que es mejor para el propósito del proceso**, nunca por su facilidad, rapidez o coste de implementación.
>
> - La facilidad/rapidez solo vale como **desempate** entre opciones igualmente buenas para el propósito; jamás como criterio primario.
> - Si la mejor opción para el propósito es costosa, **esa se elige** — y, si hace falta, se **fasea** (no se cambia por una peor que es más fácil).
> - Todo meta-loop debe poder **justificar cada decisión contra esta regla**; si no puede, la decisión está mal tomada.

Esta regla es la primera comprobación de cualquier paso de priorización o recomendación en R1–R5 y en sus `*-improve`.

**Decisión de modelo:** RT se implementa como **ley transversal, de forma incremental** — un único patrón de auto-mejora reutilizable, instanciado por etapa, empezando por la de más palanca y replicando cuando haya datos. No es una etapa centralizada de reflexión. "Empezar por la de más palanca" se interpreta bajo la regla de decisión: más palanca = más valor para el propósito, no menos esfuerzo.

**Principio de crecimiento (cómo se extiende el sistema):**
> Siempre que aparezca un **pensamiento o divergencia recurrente** —una capacidad que el proceso
> necesita y que hoy se resuelve ad-hoc— se convierte en un **skill unitario con su propio
> meta-loop** (`<cap>` + `<cap>-improve`), no en código suelto. Toda capacidad nace cumpliendo los
> requisitos de la feature principal y con la propiedad de **auto-evolucionar** dentro del proceso
> completo. El sistema se extiende **creando loops, no parches**.

**Capacidades transversales** (cruzan R1–R5, nacidas de este principio):
| Capacidad | Skill | Meta-loop |
|---|---|---|
| Presentación (HTML distópico) | `dystopian-render` | `render-improve` (evoluciona `site/_DESIGN.md`) |

```
                 ┌─────────────────────────────────────┐
   contrato ───► │  ETAPA (proceso abstracto)          │ ───► contrato
   (entrada)     │   ejecuta su propósito              │      (salida)
                 │            │                        │
                 │            ▼                        │
                 │   [meta-loop] observa resultado     │
                 │   vs propósito → mejora su método ──┼──► se reescribe
                 └─────────────────────────────────────┘
```

---

## Estado actual (honesto)

| Resp. | Etapa | Artefacto hoy | Contrato | Meta-loop (RT) |
|-------|-------|---------------|----------|-----------|
| R1 | Buscar fuentes | `skills/source-curation` + `sources.yaml` (+ señal `yield`) | ✅ | ✅ `source-curation-improve` |
| R2 | Extraer a datos estructurados | `skills/radar-scan` + `frameworks/_SCHEMA.yaml` + perfiles | ✅ | ✅ `extract-improve` (vuelta 1 dada) |
| R3 | Comparar y rankear | `taxonomy.yaml` (anclas 1/3/5 completas) | ✅ | ✅ `ranking-improve` (vuelta 1 dada) |
| R4 | Consultar y recomendar | `skills/framework-fit` + `recommendations/` | ✅ | ✅ `match-improve` (vuelta 1 dada) |
| R5 | Evaluar empíricamente | `skills/benchmark` + `benchmarks/` (F2 hecho) | ✅ | ✅ auto-mejora interna (`eval-improve`) |
| RT | Auto-mejora (ley) + regla de decisión | este documento | este documento | ✅ ley grabada, 5/5 etapas cubiertas |

**Hito:** las 5 etapas operativas tienen contrato y meta-loop; la ley RT y la regla de decisión
fundamental gobiernan todo el sistema. La arquitectura del charter está **completa**.

**Lo que falta es profundidad, no arquitectura:**
- Más **perfiles** (`frameworks/`) para que las comparativas de R4 tengan masa.
- Más **evidencia** (benchmarks R5) — F3 con herramientas/agentes reales y casos ocultos.
- Las primeras vueltas de los meta-loops son de **evidencia media**; se endurecen con más barridos.

---

## Orden de construcción — completado

1. ✅ Esquema de perfil de framework (`frameworks/_SCHEMA.yaml`).
2. ✅ Perfiles sembrados (ralph, spec-kit, beads).
3. ✅ R4 `framework-fit` + recomendación demostrada.
4. ✅ RT — meta-loops en las 5 etapas + regla de decisión fundamental.
5. ✅ R5 `benchmark` — F1 protocolo + F2 manual ejecutado. Pendiente: **F3 automatizado/real**.

---

## Cómo se usa este documento

- Cualquier cambio en el repo debe poder mapearse a una de R1–R5 o a RT. Si no encaja, o sobra o falta una responsabilidad — y entonces se discute y se actualiza aquí **primero**.
- Se revisa cuando cambie el propósito, no en cada barrido. Es estable a propósito.

---

*Charter v1 — 2026-06-26. Decisiones fijadas: RT = ley transversal incremental; alcance = datos + benchmark empírico (R5 como norte por fases). Siguiente: esquema de perfil de framework (R2).*
