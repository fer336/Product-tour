# Comparación de librerías para Product Tour

## Driver.js ⭐ (recomendado por defecto)

**Mejor para:** cualquier proyecto, sin dependencias, muy fácil

- Peso: ~5kb gzip
- Sin dependencias
- Funciona con React, Vue, Angular, HTML vanilla
- API muy simple
- Spotlight con blur del fondo real
- NPM: `driver.js`

```bash
npm install driver.js
```

**Cuándo elegirlo:** siempre que no haya razón específica para otra librería.

---

## Shepherd.js

**Mejor para:** tours complejos, alto nivel de customización visual

- Peso: ~17kb gzip
- Sin dependencias
- Soporte para React via `react-shepherd`
- Tooltips muy customizables (Floating UI por debajo)
- Eventos ricos: beforeShow, afterShow, complete, cancel
- NPM: `shepherd.js`

```bash
npm install shepherd.js
# Para React:
npm install react-shepherd
```

**Cuándo elegirlo:** cuando Driver.js queda corto en customización o eventos.

---

## Intro.js

**Mejor para:** proyectos legacy, documentación clásica

- Peso: ~10kb gzip
- Muy maduro (desde 2013)
- API simple, parecida a Driver.js
- Licencia: **AGPL-3.0** (ojo: requiere licencia comercial para productos de pago)
- NPM: `intro.js`

**Cuándo elegirlo:** casi nunca hoy en día; Driver.js lo supera. Solo si el proyecto ya lo usa.

---

## Reactour

**Mejor para:** proyectos 100% React que quieren todo en JSX

- Peso: ~8kb gzip
- Solo React
- Tour definido como componente JSX
- Usa Framer Motion para animaciones

```bash
npm install @reactour/tour
```

**Cuándo elegirlo:** proyectos React donde el equipo prefiere todo en JSX.

---

## Custom CSS (sin librería)

**Mejor para:** necesidades muy específicas o proyectos donde no se puede agregar deps

Ver `references/custom-css.md` para implementación completa.

**Cuándo elegirlo:** cuando las librerías no se adaptan al diseño o al stack.
