---
name: dystopian-render
description: Renderiza cualquier dato o informe del AI Radar (findings, recommendations, frameworks, benchmarks, JSON) en una página HTML autocontenida con el lenguaje de diseño distópico canónico de site/_DESIGN.md. Úsala SIEMPRE que el usuario quiera "verlo en HTML", "renderiza/visualiza esto", "una página distópica", "muéstrame el informe", "haz un dossier", o cuando un stage produzca una salida que merezca presentación visual. Es la capacidad de presentación del sistema — transversal a las 5 etapas. NO genera datos (eso es radar-scan/framework-fit/benchmark): los presenta. Produce HTML servible y versionado, y opcionalmente un Artifact online.
---

# Dystopian-Render — presentación (capacidad transversal)

Conviertes datos del AI Radar en una página HTML autocontenida y distópica. Eres presentación, no
generación: tomas lo que ya existe (un `.json`, un digest, una recomendación, un benchmark) y lo
haces legible y compartible online, siguiendo un lenguaje de diseño único y mejorable.

> Regla de decisión fundamental (charter RT): el render se diseña por **lo que mejor sirve al
> propósito** (que el dato se entienda y decida), no por lo más rápido de maquetar. La estética
> sirve a la legibilidad y la honestidad del dato, nunca al revés.

## Contrato
- **Entrada:** un dato/informe de origen (ruta a `.json`, `findings/`, `recommendations/`,
  `benchmarks/`, `frameworks/`) + alcance.
- **Salida:** `site/<slug>.html` autocontenido (datos embebidos), enlazado desde `site/index.html`;
  opcionalmente publicado como Artifact.

## Flujo

### 1. Carga el lenguaje de diseño
Lee `site/_DESIGN.md` (tokens, doctrina, componentes). **No inventes estilo**: usa el canónico.
`site/research.html` es la implementación de referencia.

### 2. Estructura el dato de origen
Si el dato ya es JSON, úsalo. Si es Markdown (un digest, una recomendación), extrae a una
estructura mínima primero (o genera un `.json` paralelo) — el render se hace **desde datos**, no
parafraseando prosa. Respeta la honestidad: estado, confianza y fuente deben sobrevivir al render.

### 3. Renderiza por inyección en plantilla
Parte de una plantilla con marcador `__DATA__` y **inyecta el JSON con un script**, no a mano. Así
la copia embebida es idéntica al fichero de datos (doctrina de `_DESIGN.md`). Inlinea todo el
CSS/JS. Aplica: scanlines (reduced-motion off), título glitch, eyebrow numerado, badges de estado,
cards, filtros, sin scroll horizontal.

### 4. Enlaza al proceso
Añade una entrada en `site/index.html` apuntando a la nueva página (queda en el catálogo).

### 5. Verifica
Valida que el HTML no deja marcadores sin sustituir, que el JSON embebido parsea, y que no hay
recursos externos (romperían la CSP).

### 6. Guarda, publica y commitea
- `site/<slug>.html` + enlace en el índice.
- Opcional: publícalo como Artifact para verlo online al instante.
- Commit `feat: render <slug> (dystopian-render)` + push.

## Relación con la ley RT
Tu meta-loop es `render-improve`: cuando un render confunde, no es accesible o el lenguaje se queda
corto, mejora `site/_DESIGN.md` (no parches por página). Cada mejora del diseño beneficia a todos
los renders futuros.

## Anti-patrones
- CSS/fuentes/fetch externos (la CSP los mata → render roto).
- Maquetar a mano divergiendo de `_DESIGN.md` (rompe la consistencia y el single-source).
- Embeber datos parafraseados que no coinciden con el fichero de origen.
- Ocultar estado/baja confianza para que "quede bonito" (viola la honestidad de dato).
- Scroll horizontal; animación sin respetar `prefers-reduced-motion`.
