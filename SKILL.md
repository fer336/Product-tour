---
name: product-tour
description: >
  Implementa Product Tours, Onboarding Tours, Guided Walkthroughs o Interactive Tutorials en sistemas web.
  Un Product Tour es una guía interactiva que oscurece la pantalla y resalta elementos uno por uno con tooltips explicativos (técnica llamada spotlight/highlight overlay).
  Usar este skill siempre que el usuario mencione: "tour guiado", "onboarding", "tutorial interactivo", "guía paso a paso", "spotlight", "destacar elementos", "driver.js", "shepherd.js", "intro.js",
  o cuando quiera que su app enseñe al usuario cómo usarla la primera vez que entra.
  También aplica cuando el usuario quiere hacer una demo animada de su sistema como video con Remotion.
---

# Product Tour Skill

Un **Product Tour** (también llamado Onboarding Tour, Guided Walkthrough o Interactive Tutorial) es una secuencia de pasos que:
1. **Oscurece** toda la pantalla con un overlay semitransparente
2. **Resalta** un elemento específico de la UI (spotlight/highlight overlay)
3. **Muestra un tooltip** explicando qué hace ese elemento
4. **Avanza al siguiente paso** al hacer click en "Siguiente"

---

## Flujo de trabajo

### Paso 1 — Entender el contexto

Antes de escribir código, preguntar:
- ¿Qué framework usa el sistema? (React, Vue, HTML vanilla, etc.)
- ¿Tiene que ser interactivo (librería JS) o un video/demo animada (Remotion)?
- ¿Cuántos pasos tiene el tour?
- ¿Tiene que persistir si el usuario ya lo vio? (localStorage o backend)

### Paso 2 — Elegir la herramienta correcta

Consultar `references/libraries.md` para comparación detallada.

**Regla rápida:**
| Situación | Herramienta recomendada |
|---|---|
| App React moderna | **Driver.js** (más simple) o **Shepherd.js** (más customizable) |
| HTML vanilla / cualquier stack | **Driver.js** |
| Vue / Angular | **Shepherd.js** o **Intro.js** |
| Demo como video | **Remotion** |
| Necesita máximo control visual | Implementación custom con CSS |

### Paso 3 — Implementar

Consultar el archivo de referencia correspondiente en `references/`:
- `references/driverjs.md` — Guía completa Driver.js
- `references/shepherdjs.md` — Guía completa Shepherd.js
- `references/custom-css.md` — Implementación custom sin librerías
- `references/remotion.md` — Demo animada con Remotion

### Paso 4 — Persistencia (opcional)

Si el usuario quiere que el tour no se repita:
```js
// Verificar si ya lo vio
const yaVioElTour = localStorage.getItem('tour_completado');
if (!yaVioElTour) {
  iniciarTour();
}

// Al completar el tour
localStorage.setItem('tour_completado', 'true');
```

Para sistemas con login, guardar en el perfil del usuario en la base de datos.

---

## Output esperado

Siempre entregar:
1. **Código completo** listo para copiar y pegar
2. **Instrucciones de instalación** (npm install o CDN)
3. **Explicación de cada paso** del tour en el código
4. **Cómo agregar o quitar pasos** fácilmente
