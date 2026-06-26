---
name: source-curation
description: Descubre, vetea y añade fuentes nuevas de alta señal al registro sources.yaml del AI Radar. Úsala SIEMPRE que el usuario quiera ampliar, mejorar o auditar las fuentes de búsqueda sobre desarrollo con IA — frases como "encuentra más curadores", "mejora las fuentes", "qué blogs/cuentas nuevas seguir", "amplía el radar", "tapa los huecos de sources.yaml", o cuando un barrido (radar-scan) detecte una fuente recurrente que aún no está registrada. Es la pieza que hace que el radar escale: cada ejecución deja mejor el registro.
---

# Source Curation

Enriquece `sources.yaml`. El registro es el activo que hace que cada barrido sea más amplio y certero sin más esfuerzo manual. Tu trabajo: encontrar fuentes que aporten **señal primaria** y añadirlas con metadatos honestos, evitando ruido y duplicados.

## Cuándo dispararte

Cuando el usuario quiera ampliar/mejorar fuentes, tapar huecos, o cuando un barrido encuentre una fuente fuerte no registrada.

## Flujo

### 1. Lee el estado actual
- Abre `sources.yaml` y la sección `gaps`. Abre `taxonomy.yaml` para conocer las familias.
- Identifica qué falta: ¿una familia con pocas fuentes tier-1? ¿un idioma ausente? ¿un hueco listado?

### 2. Descubre candidatos (en este orden de valor)
La señal nace en los practicantes y baja hacia las listas. Busca de arriba hacia abajo:
1. **Originadores** — autores citados repetidamente por una técnica concreta (no por opinar). Si tres fuentes atribuyen una técnica a una persona, esa persona es tier-1.
2. **Curadores** — blogs/feeds que destilan y enlazan a primarios.
3. **Listas curadas nuevas** (`awesome-*`) — minan el long-tail; revisa también sus "related lists".
4. **Agregadores** (HN/Reddit/X trending) — para lo emergente que aún no está en ninguna lista.
5. **Multi-idioma** — repite las queries clave en coreano/chino/japonés; ahí salen frameworks que el mundo anglófono ignora (MoAI-ADK fue uno).

Técnica de minería de curadores: a partir de una fuente buena, mira a quién cita y enlaza; sigue ese hilo una o dos saltos.

### 3. Vetea cada candidato (gate de calidad)
Acéptalo solo si cumple la mayoría:
- **Primario > agregador.** ¿Publica técnica propia o solo reempaqueta? Ante dos que dicen lo mismo, gana el más primario y práctico.
- **Cadencia real.** ¿Publica con regularidad o está muerto? (mira la fecha del último post).
- **Señal/ruido.** ¿Cambia decisiones o es hype? Si no aportaría a un digest, no entra.
- **No duplica.** Compara `url` y `name` contra `sources.yaml`. Si ya existe, no lo añadas; como mucho mejora sus `topics`/`tier`.

Asigna `tier`: 1 = originador/primario, 2 = metodología/síntesis de calidad, 3 = agregador útil-pero-ruidoso.

### 4. Añade al registro
Por cada fuente aceptada, añade una entrada bajo `sources:` con TODOS los campos (`id, name, type, url, topics, tier, lang, cadence, last_checked, notes`). El `id` es un slug único. `notes` debe decir **por qué** vale (qué técnica/cobertura aporta) — eso es lo que el yo-futuro necesita.

> Señal de rendimiento (`yield`, añadida por source-curation-improve 2026-06-26): el tier debe poder revisarse por **evidencia**, no por prestigio. `radar-scan` incrementa el `yield` de una fuente cuando produce un hallazgo accionable; `source-curation-improve` lo usa para retiers/poda. Si una tier-1 nunca produce, baja; si una tier-3 entrega, sube.

Actualiza la sección `gaps`: quita lo que acabas de cubrir, añade huecos nuevos que hayas detectado.

### 5. Cierra
- Resume en 2-3 frases: qué añadiste, por qué, qué hueco sigue abierto.
- Si trabajas en git: commit atómico `sources: add <N> <tema> sources` y push.

## Anti-patrones (no hagas esto)
- Añadir cuentas/blogs solo por ser famosos si no publican técnica de desarrollo con IA.
- Inflar el registro con tier-3: el ruido degrada los barridos.
- Inventar URLs o handles. Si no lo verificas, no entra.
- Duplicar una fuente que ya está con otro nombre.

## Recordatorio
Calidad del registro > cantidad. Una fuente tier-1 nueva vale más que diez tier-3.
