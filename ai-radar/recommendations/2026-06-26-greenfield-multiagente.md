# Recomendación — 2026-06-26 · Arranque greenfield multi-agente con fuente de verdad

## Historia de usuario
> Como equipo pequeño (3 devs), vamos a arrancar un proyecto **nuevo desde cero** con varios agentes de Claude Code trabajando **en paralelo**. Queremos **una única fuente de verdad** de qué construir y que los agentes **no pierdan el plan entre sesiones**.

**Requisitos extraídos:**
- Tipo de tarea (use_cases): `greenfield`, `spec-authoring`, `long-horizon-memory`, `multi-agent-orchestration`
- Lenguaje/stack: cualquiera (no especificado)
- Restricciones duras: fuente de verdad compartida · memoria persistente entre sesiones · varios agentes a la vez sin pisarse
- Prioridades: alineamiento del equipo > velocidad; control del plan

## Candidatos considerados
- **spec-kit** ✅ entra — `use_cases` incluye `spec-authoring`/`greenfield`; la spec versionada es literalmente "una única fuente de verdad".
- **beads** ✅ entra — `use_cases` incluye `long-horizon-memory`/`multi-agent-orchestration`; merge a nivel de celda pensado para varios agentes.
- **ralph** ❌ descartado — orientado a un loop autónomo sobre un PRD; `not_for` avisa "sin gates de calidad" y no resuelve colaboración multi-dev. Encaje bajo para esta historia.

## Comparativa
| Framework | Fuente de verdad | Memoria entre sesiones | Multi-agente en paralelo | Madurez | Confianza dato |
|---|---|---|---|---|---|
| **spec-kit** | ✅ la spec versionada | parcial (la spec, no el progreso) | vía 30+ integraciones, no nativo | ~115k★ | high |
| **beads** | ❌ (no es su rol) | ✅ grafo de plan/dependencias en git | ✅ merge a nivel de celda | ~17k★ · v1.0.2 | high |
| ralph | ❌ | git+progress.txt (un solo loop) | ❌ | ~16k★ | medium |

## Recomendación
**Combinación: Spec Kit + Beads.** No es un "o uno u otro" — resuelven mitades distintas de la historia:
- **Spec Kit** como **fuente de verdad de qué construir**: la spec versionada es el artefacto compartido del que derivan plan→tareas→código. Cubre `greenfield` + alineamiento del equipo.
- **Beads** como **memoria del plan entre sesiones y entre agentes**: grafo de dependencias en git con merge a nivel de celda, justo para varios agentes en paralelo sin perder estado. Cubre `long-horizon-memory` + `multi-agent-orchestration`.

Flujo sugerido: la spec (Spec Kit) define *qué*; Beads trackea *en qué punto está cada agente* y las dependencias, persistido en git.

**Caveats:**
- Ninguno de los dos aporta **gates de calidad fuertes** (state machine/hooks que bloqueen). Si el equipo quiere rigor de proceso, falta esa pieza.
- El backend de Beads (JSONL→Dolt) está pendiente de confirmar en docs primarias (`open_question` viva en su perfil).

**Si falta cobertura:** sí. No tenemos en el registro un framework **full-flow con gates** (tipo Agent OS / Superpowers / BMAD) que sería el tercer ingrediente ideal para esta historia. **Recomendación de sistema:** ejecutar `radar-scan` sobre la familia `full-flow` y `source-curation` para sembrar 1–2 candidatos con gates antes de cerrar la decisión.

---
*Generado por framework-fit (R4) sobre 3 perfiles. Contrastable contra R5 (evaluación empírica) cuando exista.*
