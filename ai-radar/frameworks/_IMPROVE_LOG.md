# Extract-Improve — registro de vueltas

> Memoria del meta-loop de R2. Cada entrada documenta una ejecución de `extract-improve`:
> qué perfiles mejoró, qué cambió, qué método se corrigió y qué queda abierto.
> Es lo que permite que el loop sepa qué hizo la vuelta anterior y no repita trabajo.

---

## Vuelta 1 — 2026-06-26

**Perfiles atacados:** beads, spec-kit, ralph (los 3 sembrados; todos en `confidence: medium`).

**Datos corregidos (verificados contra fuente primaria):**
- **beads** — ⚠️ dato obsoleto en el seed: `v0.59.0 → v1.0.2` (28-may-2026) y **cambio de backend JSONL → Dolt** (SQL versionado, merge a nivel de celda). `completeness 0.75→0.85`, `confidence medium→high`. Marcado `state_change` para findings.
- **spec-kit** — `stars 90k → ~115k` (jun-2026), 55+ releases desde feb-2026. `completeness 0.7→0.9`, `confidence medium→high`.
- **ralph** — autoría confirmada (Ryan Carson / snarktank), corre con Amp/Claude Code, memoria vía git+progress.txt+prd.json, `stars null → ~16k` (rango 14–19k). `completeness 0.6→0.8`, `confidence medium` (varianza de stars).

**open_questions:** resueltas 3, reformuladas/nuevas 3 (más precisas). Backlog estable, no crece.

**Mejora de MÉTODO (paso 5):**
- `_SCHEMA.yaml`: regla nueva — `maturity.stars`/`last_release` se verifican SIEMPRE en el repo primario, con rango anotado si las fuentes discrepan (motivado por el desfase spec-kit y beads).
- `_SCHEMA.yaml`: añadido campo opcional `meta.state_change` para que los cambios detectados aquí lleguen a `findings/`.

**Δ agregado:** completeness +0.5 (suma de 3), confidence +2 niveles. ✅ La vuelta deja los datos más fiables que antes.

**Pendiente para la próxima vuelta:**
- Confirmar backend Dolt vs JSONL de beads en docs primarias.
- Fijar versión/tag exacto de spec-kit y nº de stars exacto de ralph.
- `radar-scan` debería recoger el `state_change` de beads en el próximo digest.

---

## Vuelta 2 — 2026-06-26

**Contexto:** se importaron 25 perfiles nuevos desde `data/frameworks.json` (dossier
loop-engineering), todos en `confidence: low` con stars/versión sin verificar.

**Perfiles atacados (lote prioritario, OSS de alta palanca con stars en null):**
- **bmad** — stars `null → 49.000`, `v6.8 (may-2026)`, licencia MIT.
- **cline** — stars `null → ~61.200`, Apache-2.0, 5M+ instalaciones.
- **aider** — stars `null → ~44.000`, Apache-2.0.
- **mem0** — stars `null → ~48.000`, `v1.0.4`, Apache-2.0; URL corregida al repo `mem0ai/mem0`.

Los 4: `completeness 0.45→0.6`, `confidence low→medium`. Resuelta la open_question de
stars/versión; queda la de best_for/not_for + puntuación de dimensiones.

**Regla de decisión aplicada:** no se verificaron los 25 a ciegas — se eligió el lote por
**valor para el recomendador** (frameworks OSS comparables y citables), no por facilidad.

**Backlog (resto de importados, aún `confidence: low`):** verificar stars/versión de
bmalph, antfarm, loop-engineering-toolkit, gsd, kiro, tessl, gastown, conductor, claude-squad,
reverbcode, overstory, cosmos, openhands, 12-factor-agents; y datos de producto (no stars) de
kiro/cursor/codex/claude-code. Atacar por lotes en próximas vueltas, no de golpe.
