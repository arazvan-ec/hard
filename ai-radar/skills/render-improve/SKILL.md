---
name: render-improve
description: Meta-loop de auto-mejora de la capacidad de presentación (dystopian-render) del AI Radar. Audita los renders HTML existentes y el lenguaje de diseño site/_DESIGN.md — ¿se entiende el dato? ¿es accesible? ¿la honestidad (estado/confianza) sobrevive? ¿el estilo es consistente entre páginas? — y mejora _DESIGN.md (no parches por página). Úsala cuando el usuario diga "esto no se entiende", "mejora el diseño", "hazlo más accesible/legible", tras varios renders, o periódicamente. Es la ley RT aplicada a la presentación. NO renderiza una página concreta (eso es dystopian-render): mejora el método y el lenguaje de diseño que comparten todos los renders.
---

# Render-Improve — meta-loop de presentación

Tu trabajo es que **todos los renders mejoren a la vez**, mejorando el lenguaje de diseño común, no
una página suelta. Auditas legibilidad, accesibilidad, honestidad de dato y consistencia.

> Regla de decisión fundamental (charter RT): mejoras el diseño por **lo que sirve al propósito**
> (comprensión y honestidad del dato), nunca por lo que es más vistoso o más fácil. La estética
> es medio, no fin.

## Contrato
- **Entrada:** `site/*.html` (renders existentes) + `site/_DESIGN.md` (lenguaje canónico).
- **Salida:** mejoras a `site/_DESIGN.md` (tokens, componentes, doctrina) + registro de la vuelta.

## Flujo
### 1. Audita los renders
Revisa los HTML por: ¿el dato clave se lee de un vistazo? ¿el estado/confianza es visible y
honesto? ¿hay scroll horizontal, contraste pobre, foco invisible, animación sin `reduced-motion`?
¿divergen estilos entre páginas (señal de que alguien maquetó a mano)?

### 2. Detecta el patrón
Si un problema se repite en varias páginas, es del **lenguaje**, no de la página. Eso es lo que
arreglas. Un fallo de una sola página se devuelve a `dystopian-render`.

### 3. Mejora _DESIGN.md (lo central)
Ajusta tokens, añade/aclara un componente, endurece una regla de la doctrina. Justifícalo contra
la regla: mejora comprensión/accesibilidad/honestidad, no solo "se ve mejor". Registra el cambio
en la sección "Registro de evolución" de `_DESIGN.md`.

### 4. Commitea
`feat: improve dystopian design language (render-improve)` + push. Los renders futuros heredan la
mejora; los existentes se re-renderizan cuando toque.

## Anti-patrones
- Parchear una página en vez de el lenguaje compartido.
- Añadir adorno que no mejora la comprensión (viola la regla).
- Romper la doctrina de autocontenido/accesibilidad por estética.
- Sacrificar la honestidad del dato (ocultar estado/confianza) por limpieza visual.
