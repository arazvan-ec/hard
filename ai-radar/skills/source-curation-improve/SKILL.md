---
name: source-curation-improve
description: Meta-loop de auto-mejora de la etapa R1 (buscar fuentes) del AI Radar. Audita la salud del registro sources.yaml — qué fuentes producen señal de verdad y cuáles son ruido, qué huecos se repiten — y mejora el MÉTODO de source-curation (sus heurísticas de descubrimiento y vetado) y los tiers, en base a resultados. Úsala tras varios barridos, cuando el usuario quiera "limpiar/auditar las fuentes", "por qué no encontramos X", "afinar el registro", o periódicamente. Es la implementación de la ley RT para R1. NO descubre fuentes nuevas (eso es source-curation): mejora cómo se descubren y se puntúan.
---

# Source-Curation-Improve — meta-loop de R1

Tu trabajo es que el **registro mejore como activo**: que cada barrido salga de fuentes más certeras y con menos ruido, y que `source-curation` aprenda de lo que funcionó. No añades fuentes (eso es R1): auditas y mejoras el método.

> Regla de decisión fundamental (charter RT): las mejoras se eligen por **lo que da mejor señal para el propósito**, nunca por lo fácil. No subas a tier-1 una fuente porque sea cómoda de leer; súbela si **produce hallazgos que cambian decisiones**.

## Contrato
- **Entrada:** `sources.yaml`, los `findings/*.md` (qué fuentes produjeron hallazgos), su sección `gaps`.
- **Salida:** ajustes de tier/poda en `sources.yaml`, mejoras al método en `skills/source-curation/SKILL.md`, entradas en un registro de vueltas.

## Flujo

### 1. Mide el rendimiento real de cada fuente
Cruza los `findings/*.md` con `sources.yaml`: ¿qué fuentes han producido hallazgos accionables y cuáles no aparecen nunca? La señal real, no el prestigio, manda.

### 2. Ajusta tiers y poda (decisión por señal)
- Sube de tier las que entregan; baja o marca para revisión las que solo hacen ruido.
- Propón retirar fuentes muertas (sin publicar) o redundantes.
- Toda subida/bajada se justifica con evidencia de `findings/`, no con opinión.

### 3. Detecta huecos recurrentes → mejora el MÉTODO
Si los barridos repiten "fuentes nuevas detectadas no registradas" de un mismo tipo (un idioma, un agregador, un tipo de autor), eso es un **fallo sistemático de descubrimiento**. Mejora `source-curation` (sus heurísticas, sus queries, su orden de búsqueda) para que deje de perderse esa clase de fuente. Corrige la fábrica, no solo el inventario.

### 4. Registra y commitea
- Anota la vuelta (qué medió, qué ajustó, qué método mejoró) en `sources_improve_log` (o el log acordado).
- Commits atómicos: `sources: retier/prune by yield` (datos), `skill: improve source-curation discovery` (método). Push tras cada uno.

## Anti-patrones
- Retiers por fama/comodidad en vez de por hallazgos producidos (viola la regla).
- Inflar el registro: eso es ruido, justo lo contrario de tu trabajo.
- Añadir fuentes nuevas tú mismo (eso es source-curation).
- Mejorar el inventario y nunca el método de descubrimiento.
