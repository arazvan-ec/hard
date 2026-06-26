---
name: extract-improve
description: Meta-loop de auto-mejora de la etapa R2 (extracción a datos estructurados) del AI Radar. Audita los perfiles frameworks/*.yaml, resuelve sus open_questions contra fuentes primarias, sube completeness/confidence, detecta cambios de estado (nuevas versiones, deprecations) y —cuando un hueco se repite— mejora el propio método (el esquema o la skill de extracción). Úsala SIEMPRE que el usuario quiera "mejorar los datos", "actualizar/verificar los perfiles", "cerrar huecos", "una vuelta de auto-mejora", "refrescar frameworks", o tras un barrido para dejar los perfiles más fiables. Es la implementación de la ley RT (auto-mejora unitaria) para R2: cada ejecución debe dejar al menos un perfil más completo o más fiable que antes.
---

# Extract-Improve — meta-loop de R2

Eres el bucle de auto-mejora de la etapa de extracción. Tu propósito NO es descubrir frameworks nuevos (eso es `radar-scan`) ni recomendar (eso es `framework-fit`): es **subir la calidad y la fiabilidad de los datos ya capturados** en `frameworks/*.yaml`, y mejorar el método que los produce.

> Ley RT: cada vuelta deja al menos un perfil más completo o más fiable que la anterior. El sistema compone porque el dato mejora solo.

## Contrato

- **Entrada:** `frameworks/*.yaml` (perfiles existentes) + `frameworks/_SCHEMA.yaml` (contrato de R2).
- **Salida:** los mismos perfiles, mejorados; entradas en `frameworks/_IMPROVE_LOG.md`; y, si procede, mejoras al esquema o a `radar-scan`.

## Flujo

### 1. Prioriza qué mejorar
Lee todos los `frameworks/*.yaml`. Ordena por necesidad de atención:
1. `meta.confidence: low` antes que `medium` antes que `high`.
2. `meta.completeness` más baja primero.
3. `meta.last_verified` más antiguo primero (los datos caducan: stars, versiones, estado).
Coge los 1–3 perfiles con más necesidad (o los que pida el usuario). No intentes todos a la vez: profundidad sobre amplitud.

### 2. Resuelve open_questions contra la fuente primaria
Por cada `open_question` del perfil:
- Busca/fetch la **fuente primaria** (el repo, el release, el blog del autor), no un agregador.
- Si la resuelves, actualiza el campo correspondiente y **elimina** la pregunta.
- Si surge una duda nueva más precisa, **añádela** (el backlog evoluciona, no se vacía sin más).
- Si no puedes verificarla, déjala y anota por qué (no inventes datos).

### 3. Detecta cambios de estado
Compara lo verificado con lo que decía el perfil. Si hay **nueva versión mayor, cambio de backend/arquitectura, deprecation o pivote**:
- Actualiza `maturity` / `one_liner` / `steal` según corresponda.
- Marca el cambio para que `radar-scan` lo recoja en el próximo digest (sección "Cambios de estado"). Anótalo en `_IMPROVE_LOG.md`.

### 4. Recalcula meta
- `completeness`: fracción de campos relevantes rellenos y verificados (0.0–1.0).
- `confidence`: `high` solo si los datos clave vienen de fuente primaria y son recientes; `medium` si hay agregadores o varianza entre fuentes (anótala); `low` si quedan supuestos sin verificar.
- `last_verified`: fecha de hoy para lo tocado.

### 5. Mejora el MÉTODO (auto-mejora unitaria, no solo el dato)
Si un mismo hueco aparece en varios perfiles (p. ej. "siempre falta la licencia", "el flujo de gates nunca se captura"):
- Mejora `frameworks/_SCHEMA.yaml` (añade/aclara un campo) **o** la skill `radar-scan` (añade el dato a su paso de extracción), para que el hueco no se repita en futuras extracciones.
- Esto es lo que distingue a un meta-loop de un simple refresco: corrige la fábrica, no solo el producto.

### 6. Registra y commitea
- Añade una entrada a `frameworks/_IMPROVE_LOG.md`: fecha, qué perfiles tocaste, qué cambió, qué método mejoraste, qué queda abierto. Este log es la **memoria del loop** (sabe qué hizo la vuelta pasada).
- Commits atómicos: `frameworks: improve <id> profile` (datos), `feat: refine framework schema` o `skill: improve radar-scan extraction` (método). Push tras cada uno.

## Métrica de éxito (de la propia etapa)
- Δ `completeness` y Δ `confidence` agregados > 0 en cada vuelta.
- Nº de `open_questions` resueltas ≥ nº de nuevas (el backlog no crece sin control).
- Al menos cada N vueltas, una mejora de método (paso 5), no solo de dato.

## Anti-patrones
- Inventar stars/versiones que no verificaste contra fuente primaria.
- Vaciar `open_questions` marcándolas resueltas sin evidencia.
- Descubrir frameworks nuevos (eso es trabajo de `radar-scan`, no tuyo).
- Tocar los 20 perfiles por encima en vez de mejorar 2 a fondo.
- Mejorar el dato y nunca el método: entonces no es auto-mejora, es mantenimiento.
