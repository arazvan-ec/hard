# AI Radar

Sistema para **clasificar y encontrar las noticias y prácticas más nuevas del desarrollo con IA**. No es una lista que se queda obsoleta: es un bucle que mejora solo cada vez que se ejecuta.

> Filosofía: **robar las técnicas, no instalar los sistemas.** El radar destila señal accionable, no acumula enlaces.

## El problema

El panorama (SDD, harness engineering, orquestadores, bucles autónomos, metodologías de proceso) se mueve semana a semana. Una búsqueda manual tiene dos fallos: sesgo de fuentes (te quedas en las *awesome-lists* y te pierdes a los practicantes que publican la técnica **antes** de que llegue a un repo) y no escala (cada barrido empieza de cero).

## La solución: un bucle que compone

```
        ┌──────────────────────────────────────────────┐
        │                                              │
        ▼                                              │
  [1] CURAR FUENTES ──► [2] ESCANEAR ──► [3] CLASIFICAR ──► [4] COMPOSTAR
   source-curation       radar-scan        taxonomy          findings/
   (descubre+vetea)    (búsqueda extensa) (familias+dims)   (digest+catálogo)
        ▲                                                       │
        └───────────────  realimenta sources.yaml ─────────────┘
```

- **[1] Curar fuentes** (`skills/source-curation`): descubre fuentes nuevas de alta señal y las añade a `sources.yaml`. **Esta es la pieza que hace que escale**: cuanto mejor es el registro, mejor es cada barrido.
- **[2] Escanear** (`skills/radar-scan`): recorre `sources.yaml` y hace una búsqueda extensa de novedades.
- **[3] Clasificar** (`taxonomy.yaml`): encaja cada hallazgo en una familia y lo puntúa.
- **[4] Compostar** (`findings/`): escribe un digest fechado y actualiza el catálogo, evitando duplicados. Lo aprendido realimenta el registro.

## Por qué escala

El activo no son los hallazgos: es **`sources.yaml`**. Cada ejecución de `source-curation` lo enriquece (más curadores, más idiomas, mejor señal), así que el siguiente barrido es más amplio y más certero sin más esfuerzo manual. Es el principio de *compounding*: cada pasada deja al sistema más listo que la anterior.

## Estructura

| Ruta | Qué es |
|------|--------|
| `sources.yaml` | Registro versionado de fuentes (awesome-lists, blogs, curadores X, blogs de ingeniería, agregadores). El activo central. |
| `taxonomy.yaml` | Familias de clasificación + dimensiones de puntuación + tipos de señal. |
| `skills/source-curation/SKILL.md` | Skill que descubre, vetea y añade fuentes nuevas. |
| `skills/radar-scan/SKILL.md` | Skill que ejecuta el barrido y produce el digest clasificado. |
| `findings/` | Digests fechados (`YYYY-MM-DD.md`) + `_TEMPLATE.md`. |

## Quickstart (con Claude Code)

```
# Barrido de novedades de la última semana
"Usa radar-scan para buscar novedades de la última semana en sources.yaml"

# Ampliar las fuentes (hacer que escale)
"Usa source-curation para descubrir 5 curadores o blogs nuevos de alta señal sobre harness engineering y añádelos a sources.yaml"
```

Las skills se disparan solas por su descripción; también puedes invocarlas por nombre.

## Convenciones

- **Commits atómicos:** un commit por cambio lógico, push tras cada uno. Mensajes: `sources:` (cambios en el registro), `findings:` (nuevos digests), `skill:` (cambios en skills), `docs:`.
- **Fuentes primarias > agregadores.** Ante dos enlaces que dicen lo mismo, gana el más primario y práctico.
- **Señal, no ruido.** Si un hallazgo no cambia una decisión, no entra.

## Roadmap (v0 → v1)

- [x] **v0** — registro sembrado + dos skills + plantilla de digest (prompt-driven).
- [ ] **v0.1** — script `validate_sources.py` (dedup, URLs vivas, campos obligatorios) → lógica determinista fuera del prompt.
- [ ] **v0.2** — `catalog.md` generado: tabla maestra de frameworks puntuada (importar del documento maestro existente).
- [ ] **v0.3** — backend de memoria (agentmemory/Beads) para dedupe entre barridos y novedad real.
- [ ] **v0.4** — fuentes multi-idioma (coreano/chino/japonés) y agregadores (HN/Reddit/X).
- [ ] **v1** — gate de calidad: un hallazgo solo entra en el catálogo si pasa validación de la taxonomía.

---

*Sembrado a partir de la investigación de junio 2026. Ver el documento maestro para el catálogo puntuado de partida.*
