---
name: framework-fit
description: Responde, ante una historia de usuario concreta, qué framework(s) de desarrollo con IA conviene usar, comparando los perfiles de frameworks/*.yaml y recomendando con justificación y avisos. Úsala SIEMPRE que el usuario pregunte "¿me ayuda X o Y para esto?", "¿qué framework uso para…?", "compara frameworks para esta tarea", "recomiéndame para esta historia de usuario", o pase una historia/tarea de desarrollo y quiera saber qué herramienta encaja. Es la etapa R4 (consultar y recomendar) del AI Radar. NO descubre frameworks (eso es radar-scan) ni mejora los datos (eso es extract-improve): consume los perfiles para dar una recomendación trazable.
---

# Framework-Fit — recomendador (R4)

Dada una **historia de usuario**, filtras los perfiles `frameworks/*.yaml`, comparas los candidatos y recomiendas el/los que mejor encajan, con justificación trazable y avisos honestos. La recomendación se guarda versionada.

> Una recomendación solo vale lo que valen los datos que la sostienen. Si te apoyas en un perfil de `confidence: low`, dilo. Si no hay buen candidato, dilo y pide ampliar el registro.

## Contrato
- **Entrada:** una historia de usuario + restricciones (lenguaje/stack, gates exigidos, multi-agente, presupuesto/tiempo, etc.).
- **Salida:** shortlist + comparativa + recomendación razonada con caveats, guardada en `recommendations/YYYY-MM-DD-<slug>.md`.

## Flujo

### 1. Descompón la historia en requisitos
De la historia, extrae:
- **Tipo de tarea** → mapea a `use_case_vocab` del esquema (greenfield, porting, refactoring, spec-authoring, long-horizon-memory, multi-agent-orchestration, ci-autofix, autonomous-long-run, context-management, evaluation).
- **Lenguaje/stack**, si lo hay.
- **Restricciones duras** (necesita gates de calidad, debe ser portable, memoria entre sesiones, varios agentes…).
- **Prioridades** (rigor vs ligereza, velocidad vs control).
Si la historia es ambigua en algo que cambia la recomendación, pregunta antes de recomendar.

### 2. Filtra candidatos
Recorre `frameworks/*.yaml`. Descarta los que no cumplen una restricción dura (`requires`, `languages`, `not_for`). Quédate con los que tienen `use_cases` que solapan con los requisitos.

### 3. Puntúa el encaje (no el framework en abstracto)
Para cada candidato, evalúa el encaje **con ESTA historia**, no su Σ global:
- Solapamiento de `use_cases` con los requisitos.
- `best_for` / `not_for` aplicados al caso.
- Las **dimensiones que importan para esta historia** (p. ej. si pide rigor → pesa RIG; si pide memoria larga → pesa CTX/MA).
- **Confianza del dato** (`meta.confidence`): un encaje aparente sobre datos `low` vale menos.

### 4. Comparativa
Tabla de los candidatos × las dimensiones/atributos relevantes para esta historia (no las 7 siempre: solo las que deciden). Marca claramente el porqué de cada celda.

### 5. Recomienda
- Di **qué usar** y por qué, en términos de la historia. Puede ser **una combinación** (p. ej. un spec-as-artifact para fijar la spec + una memoria para el horizonte largo).
- Incluye **caveats**: qué `not_for` aplica, qué dato es flojo, qué falta verificar.
- Si **ningún candidato encaja bien** o hay pocos perfiles, dilo y recomienda ejecutar `radar-scan`/`source-curation` para ampliar candidatos antes de decidir.

### 6. Guarda y commitea
- Copia `recommendations/_TEMPLATE.md` a `recommendations/YYYY-MM-DD-<slug>.md` y rellénalo.
- Commit `recommendations: add <slug>` + push. Así cada consulta queda versionada y se puede contrastar luego contra R5 (evaluación empírica).

## Relación con la ley RT
Tu meta-loop es `match-improve`: cuando R5 (evaluación empírica) exista, comparará tu recomendación contra el ganador real y ajustará cómo puntúas el encaje. Hasta entonces, deja la recomendación trazable (qué atributos la sostienen) para que sea auditable.

## Anti-patrones
- Recomendar un framework por su fama o su Σ global ignorando que `not_for` aplica a esta historia.
- Ocultar que la recomendación se apoya en datos de baja confianza.
- Inventar encaje para frameworks que no tienen el `use_case` (mejor decir "no hay buen candidato, amplía el registro").
- Recomendar uno solo cuando una combinación resuelve mejor la historia.
