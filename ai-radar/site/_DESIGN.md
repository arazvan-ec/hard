# _DESIGN.md — Lenguaje de diseño distópico (canónico)

> Fuente única de verdad del estilo de los renders del AI Radar. El skill `dystopian-render`
> lo consume e **inlinea** (la CSP de los Artifacts prohíbe CSS/fuentes/fetch externos). El
> meta-loop `render-improve` evoluciona este documento. No dupliques estilos en cada página:
> cámbialos aquí.

## Doctrina (reglas duras — violarlas rompe el render)
- **Autocontenido.** Todo CSS/JS inline; datos embebidos en `<script type="application/json">`.
  Nada de CDN, fuentes remotas ni `fetch` (la CSP los bloquea → fallo silencioso).
- **Datos = fichero.** El render embebe una copia EXACTA del `.json`/dato de origen; se genera
  por inyección en plantilla, nunca a mano, para que vista y dato no diverjan.
- **Sin scroll horizontal.** Contenido ancho (tablas) en contenedor con `overflow-x:auto`.
- **Accesible.** `prefers-reduced-motion` desactiva flicker/glitch; foco visible; contraste alto.
- **Honestidad de dato.** Estado y confianza visibles; lo `deprecated` se estampa, no se oculta;
  lo de baja confianza se marca, no se disfraza.

## Tokens (paleta)
```
--void:#08080a  fondo vacío        --ash:#cfc9bd  texto bone/ash
--panel:#101014 panel              --dim:#76736a  texto secundario
--panel2:#16161c panel elevado     --faint:#4a4842 texto terciario
--line:#26262e  bordes
--hazard:#e8b21f  ámbar señal (acento principal)
--danger:#d23b1e  rojo alerta (deprecated / clasificado)
--cyan:#45c0cf    cian terminal (enlaces / rol, uso escaso)
--ok:#8fb04a      verde activo (badges ACTIVE)
```
Tipo: **monoespaciada** en todo (`"SF Mono",ui-monospace,"JetBrains Mono",Menlo,Consolas`).
El carácter lo dan escala, peso (800 en títulos) y tracking, no una webfont.

## Componentes del lenguaje
- **hazard-bar**: franja diagonal ámbar/negro arriba.
- **banner clasificado**: cabecera `CLASSIFIED // …` + `● REC` parpadeante (rojo).
- **título glitch**: dos pseudo-elementos desplazados (rojo/cian) con clip animado.
- **eyebrow numerado**: `01 — Sección` con regla roja.
- **badge de estado**: `ACTIVE` (verde) / `DECOMMISSIONED` (rojo, con sello rotado).
- **subject card**: índice `#NN`, categoría (ámbar), one-liner, bloque `▸ TÉCNICA A ROBAR`
  (borde ámbar), `↻ rol` (cian), pie con stars/versión/fuente.
- **filtros**: botones por categoría, `aria-pressed` para el activo (ámbar sólido).
- **scanlines + vignette**: overlays `position:fixed` con `flick` animation (reduced-motion off).

## Implementación de referencia
`site/research.html` es la implementación canónica viva de este lenguaje. Para un render nuevo,
parte de su `<style>` + el patrón de plantilla `__DATA__` inyectada por script.

## Registro de evolución (lo mantiene render-improve)
- **2026-06-26** — v0: extraído del dossier loop-engineering. Paleta hazard+danger, scanlines CRT,
  badges ACTIVE/DECOMMISSIONED, filtros por categoría.
