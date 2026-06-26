# Responsabilidades del repo — AI Radar

> Documento-charter. Define **qué tiene que hacer** este repo y **bajo qué ley arquitectónica** lo hace. Es la fuente de verdad sobre el propósito; todo lo demás (skills, taxonomía, registro, digests) son implementaciones de estas responsabilidades y se juzgan por cuánto las cumplen.

## Propósito

Encontrar, clasificar y destilar **lo más nuevo del desarrollo con IA** (repos, técnicas, frameworks, prácticas), convirtiéndolo en señal accionable — y hacerlo mediante un sistema que **mejora solo en cada ejecución**.

> Principio rector heredado: **robar las técnicas, no instalar los sistemas.** Destilamos señal, no acumulamos enlaces.

---

## Las cuatro responsabilidades

Tres son **operativas** (etapas del flujo). La cuarta es **transversal** (una ley que gobierna a las otras tres y a sí misma).

### R1 — Buscar fuentes
**Qué:** descubrir de dónde sacar repos e información sobre nuevos desarrollos con IA, vetearlos por señal y mantener un registro vivo.
- **Entrada:** huecos conocidos, fuentes existentes, señales de barridos previos.
- **Salida:** entradas nuevas/mejoradas en `sources.yaml` con metadatos honestos.
- **Implementación actual:** skill `source-curation` + `sources.yaml`.
- **Métrica de éxito:** % de hallazgos valiosos que vinieron de fuentes del registro (cobertura) y ratio señal/ruido del registro (precisión).

### R2 — Analizar y rankear
**Qué:** clasificar cada hallazgo y producir un **ranking** comparable y justificado, no una lista plana.
- **Entrada:** hallazgos en bruto extraídos por R3.
- **Salida:** cada hallazgo con familia + tipo(s) de señal + puntuación por dimensiones (Σ), y un orden por relevancia.
- **Implementación actual:** `taxonomy.yaml` (familias, señales, dimensiones, gate de novedad) aplicado en el digest.
- **Métrica de éxito:** consistencia del ranking (¿dos barridos puntúan igual lo equivalente?) y poder de decisión (¿el top del ranking cambia lo que hacemos?).

### R3 — Extraer la mayor cantidad de información
**Qué:** de cada fuente/hallazgo, sacar el máximo de información útil — qué es, fecha, fuente primaria, datos, y sobre todo la **técnica reutilizable** ("idea a robar").
- **Entrada:** fuentes seleccionadas de `sources.yaml` para un alcance dado.
- **Salida:** hallazgos ricos y trazables (cada afirmación con su fuente), parafraseados.
- **Implementación actual:** skill `radar-scan` (pasos de búsqueda y extracción).
- **Métrica de éxito:** profundidad por hallazgo (campos rellenos, fuente primaria alcanzada) sin sacrificar veracidad ni copiar texto.

### R4 — Cada etapa es un proceso abstracto y auto-mejorable *(ley transversal)*
**Qué:** toda etapa del sistema (incluida esta) debe ser un **proceso abstracto** con tres propiedades:

1. **Contrato explícito.** Define sus entradas y salidas como una interfaz estable. Otra etapa solo depende del contrato, nunca de cómo está hecho por dentro.
2. **Evolución independiente.** Se puede mejorar una etapa sin tocar las demás. Cambiar cómo rankeamos (R2) no debe obligar a cambiar cómo buscamos (R1).
3. **Auto-mejora unitaria.** Cada flujo **sabe mejorarse a sí mismo**: tiene un bucle propio que observa sus resultados, detecta dónde falló respecto a su propósito, y **reescribe su propio método** (su SKILL.md, sus criterios, sus queries) para la próxima vez.

Esta responsabilidad es la que hace que el sistema *componga*: el activo no son los hallazgos, es la **capacidad creciente de cada etapa**. Cada ejecución debe dejar al menos una etapa mejor que antes.

---

## La ley R4, en concreto

Para que R4 no sea retórica, cada etapa cumple este patrón:

```
                 ┌─────────────────────────────────────┐
   contrato ───► │  ETAPA (proceso abstracto)          │ ───► contrato
   (entrada)     │                                     │      (salida)
                 │   ejecuta su propósito              │
                 │            │                        │
                 │            ▼                        │
                 │   [meta-loop] observa su resultado  │
                 │   vs su propósito → propone mejora  │
                 │   a su propio método ───────────────┼──► se reescribe
                 └─────────────────────────────────────┘
```

- El **contrato** de cada etapa se documenta (entradas/salidas) y se versiona.
- El **meta-loop** de cada etapa es, idealmente, otra skill hermana (`<etapa>-improve`) cuyo único trabajo es auditar la etapa y proponer/aplicar una mejora a su definición — con evidencia del barrido que la motiva.
- Las mejoras se **commitean atómicamente** (`skill:` para cambios de método, `sources:`/`findings:` para datos), de modo que la evolución de cada etapa queda versionada y es auditable.

---

## Mapa: responsabilidad → artefacto → estado

| Resp. | Etapa | Artefacto hoy | Contrato definido | Meta-loop (auto-mejora) |
|-------|-------|---------------|-------------------|--------------------------|
| R1 | Buscar fuentes | `skills/source-curation` + `sources.yaml` | parcial | ❌ pendiente |
| R2 | Analizar/rankear | `taxonomy.yaml` (+ en `radar-scan`) | parcial | ❌ pendiente |
| R3 | Extraer info | `skills/radar-scan` | parcial | ❌ pendiente |
| R4 | Abstracción + auto-mejora | *(esta ley)* | este documento | ❌ pendiente (es lo siguiente a construir) |

**Lectura del estado:** las tres etapas operativas existen como skills/config, pero ninguna tiene aún (a) un contrato formal escrito ni (b) su meta-loop de auto-mejora. Cumplir R4 es el siguiente trabajo: dar a cada etapa su contrato explícito y su `*-improve`.

---

## Cómo se usa este documento

- Cualquier cambio en el repo debe poder mapearse a una de R1–R4. Si no encaja en ninguna, o sobra o falta una responsabilidad (y entonces se discute y se actualiza aquí primero).
- Este documento se revisa cuando cambie el propósito, no en cada barrido. Es estable a propósito.

---

*Charter v0 — 2026-06-26. Define responsabilidades; la implementación de los contratos y meta-loops (R4) viene a continuación.*
