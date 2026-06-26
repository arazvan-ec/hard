# Recomendaciones — 2026-06-26 · Simulaciones (framework-fit sobre 28 perfiles)

> Casos sintéticos para demostrar el recomendador R4 sobre el registro completo. Cada uno
> aplica el método de `framework-fit`, incluido el factor "quién aporta el oráculo de corrección"
> aprendido del benchmark F2 (vía match-improve). Confianza de datos anotada.

---

## SIM 1 — Migración con tests ya existentes (oráculo presente)
**Historia:** *"Migrar un microservicio de Express a Fastify sin cambiar el comportamiento. Tenemos buena cobertura de tests."*

- **Requisitos:** `porting` + `refactoring`, corrección crítica, **el oráculo YA existe** (los tests).
- **Candidatos:** ralph, bmalph (loop), aider (refactor git-native), spec-kit/openspec (anclar).

| Framework | Encaje | Por qué | Confianza dato |
|---|---|---|---|
| **Ralph / bmalph** | **Alto** | `porting` + `autonomous-long-run`: itera hasta que los tests pasen. El oráculo ya está → su debilidad (no genera criterios) no aplica aquí. | media |
| Aider | Medio-alto | refactor git-native, cada cambio un commit; bueno si se prefiere control fino | media |
| Spec Kit | Bajo | no hace falta crear spec: el comportamiento ya está fijado por los tests | alta |

> **Recomendación:** **Ralph (o bmalph) como motor del porting**, sobre Claude Code/Codex, usando la suite de tests como condición de terminación. Aider como alternativa si se quiere revisar diff a diff.
> **Caveat:** sin gates extra; fía la corrección 100% a la cobertura de tests existente. *(Factor oráculo: como YA existe, el loop autónomo es ideal — contraste deliberado con SIM 2.)*

---

## SIM 2 — Librería nueva, muy correcta, sin tests aún (oráculo ausente)
**Historia:** *"Construir desde cero una librería de validación de IBAN/SWIFT con muchos edge cases; tiene que ser muy correcta."*

- **Requisitos:** `greenfield`, corrección crítica, especificable, **no hay oráculo todavía**.
- **Candidatos:** superpowers, spec-kit, openspec, + un loop (ralph).

| Framework | Encaje | Por qué | Confianza dato |
|---|---|---|---|
| **Superpowers** | **Alto** | flujo de 7 fases con **TDD obligatorio**: crea el oráculo (tests) como parte del proceso | media (★224k verificado, best_for no) |
| **Spec Kit / OpenSpec** | **Alto** | enumeran casos como spec antes de codificar → generan el oráculo | alta / alta |
| Ralph | Medio | excelente convergiendo, pero **necesita el oráculo que aquí no existe** | media |

> **Recomendación:** **combinación.** Spec-as-artifact (Spec Kit u OpenSpec) o Superpowers para **enumerar edge-cases y crear el oráculo** → luego un loop para converger. No empezar por Ralph solo: sin oráculo se detendría en falso.
> **Caveat:** OpenSpec aporta además gate de estado (rigor). *(Factor oráculo aplicado: aquí manda quien lo crea — justo el aprendizaje del F2.)*

---

## SIM 3 — Equipo grande, multi-agente, proyecto de meses
**Historia:** *"Equipo de 5, proyecto grande de varios meses, queremos varios agentes en paralelo sin pisarse y no perder el plan."*

- **Requisitos:** `multi-agent-orchestration` + `long-horizon-memory` + fuente de verdad.
- **Candidatos:** gastown, reverbcode, conductor (orquestar) · beads (memoria) · spec-kit (verdad).

| Framework | Rol | Por qué | Confianza dato |
|---|---|---|---|
| **Gastown / ReverbCode** | orquestador | flota de agentes en worktrees + enrutado de CI/review/conflictos | baja (stars null) |
| **Beads** | memoria | grafo de plan/dependencias en git, merge a nivel de celda multi-agente | alta |
| **Spec Kit** | fuente de verdad | la spec versionada alinea a humanos y agentes | alta |

> **Recomendación:** **stack de tres piezas** — un orquestador con worktrees (ReverbCode si se quiere enrutado automático de backpressure; Gastown para flota grande con roles) + **Beads** (memoria/plan) + **Spec Kit** (verdad compartida).
> **Caveat:** los orquestadores están en `confidence: low` (stars sin verificar) → conviene un `extract-improve` sobre ellos antes de comprometerse. Falta un full-flow con gates duros en el registro para el rigor de proceso.

---

## SIM 4 — Dev solo, rápido, sin lock-in, poco montaje
**Historia:** *"Dev en solitario, quiero ser productivo ya, sin atarme a un vendor ni montar mucha infraestructura."*

- **Requisitos:** ligereza (LIG alta), portabilidad/BYOK, bajo overhead.
- **Candidatos:** cline, aider (agentes BYOK) · gsd, kiro (proceso ligero).

| Framework | Encaje | Por qué | Confianza dato |
|---|---|---|---|
| **Cline** | **Alto** | Apache-2.0, **BYOK total** (Anthropic, OpenAI, Gemini, OpenRouter…), sidebar VS Code | media (★61k) |
| **Aider** | **Alto** | git-native, CLI, multi-modelo, cero ceremonia | media (★44k) |
| GSD | Medio | proceso ligero ('Lean Orchestrator', 15% de contexto) si quiere algo de estructura | baja |
| Kiro | Bajo-medio | el más ligero (3 .md) pero IDE propio = algo de lock-in de entorno | baja |

> **Recomendación:** **Cline o Aider** como agente (independencia de vendor + arranque inmediato). Si quiere un mínimo de proceso sin peso, **GSD** encima.
> **Caveat:** GSD/Kiro en `confidence: low`; Cline/Aider verificados (vuelta 2 de extract-improve).

---

## Lectura transversal
- El sistema **no da siempre un único ganador**: en 2 de 4 casos la mejor respuesta es una **combinación**.
- El factor **oráculo de corrección** (del F2) cambia la recomendación entre SIM 1 (oráculo existe → loop) y SIM 2 (no existe → spec-first primero). El aprendizaje se está aplicando de verdad.
- Las recomendaciones **exponen su propia incertidumbre**: donde el dato es `low`, lo dice y pide `extract-improve`. Eso es honestidad de sistema, no marketing.

*Generado por framework-fit (R4). Casos sintéticos de demostración.*
