# Driver.js — Guía de implementación

## Instalación

```bash
npm install driver.js
```

O via CDN (HTML vanilla):
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/driver.js@1.3.1/dist/driver.css"/>
<script src="https://cdn.jsdelivr.net/npm/driver.js@1.3.1/dist/driver.js.iife.js"></script>
```

---

## Uso básico — HTML/JS vanilla

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/driver.js@1.3.1/dist/driver.css"/>
</head>
<body>

  <!-- Elementos que el tour va a resaltar -->
  <nav id="menu-principal">...</nav>
  <button id="btn-nueva-orden">Nueva Orden</button>
  <div id="panel-reportes">...</div>

  <script src="https://cdn.jsdelivr.net/npm/driver.js@1.3.1/dist/driver.js.iife.js"></script>
  <script>
    const { driver } = window.driver.js;

    const tourGuiado = driver({
      // Textos de los botones de navegación
      nextBtnText: 'Siguiente →',
      prevBtnText: '← Anterior',
      doneBtnText: 'Entendido ✓',

      // Mostrar progreso (paso 1 de 3)
      showProgress: true,

      // Al completar el tour, guardarlo
      onDestroyStarted: () => {
        localStorage.setItem('tour_completado', 'true');
        tourGuiado.destroy();
      },

      // Los pasos del tour
      steps: [
        {
          element: '#menu-principal',
          popover: {
            title: 'Menú Principal',
            description: 'Desde aquí navegás a todas las secciones del sistema.',
            side: 'right',      // tooltip a la derecha del elemento
            align: 'start',
          }
        },
        {
          element: '#btn-nueva-orden',
          popover: {
            title: 'Crear Nueva Orden',
            description: 'Hacé click aquí para registrar una nueva orden de trabajo.',
            side: 'bottom',
            align: 'center',
          }
        },
        {
          element: '#panel-reportes',
          popover: {
            title: 'Reportes y Estadísticas',
            description: 'Acá podés ver el resumen de actividad del día.',
            side: 'top',
          }
        },
      ]
    });

    // Iniciar el tour solo si el usuario no lo vio antes
    const yaVio = localStorage.getItem('tour_completado');
    if (!yaVio) {
      tourGuiado.drive();
    }

    // Botón para relanzar el tour manualmente
    document.getElementById('btn-relanzar-tour')?.addEventListener('click', () => {
      tourGuiado.drive();
    });
  </script>
</body>
</html>
```

---

## Uso en React

```jsx
import { driver } from 'driver.js';
import 'driver.js/dist/driver.css';
import { useEffect } from 'react';

export function MiDashboard() {

  useEffect(() => {
    const yaVio = localStorage.getItem('tour_completado');
    if (yaVio) return;

    const tourGuiado = driver({
      nextBtnText: 'Siguiente →',
      prevBtnText: '← Anterior',
      doneBtnText: 'Listo ✓',
      showProgress: true,
      steps: [
        {
          element: '#menu-principal',
          popover: {
            title: 'Menú Principal',
            description: 'Navegá a todas las secciones desde aquí.',
            side: 'right',
          }
        },
        {
          element: '#btn-nueva-orden',
          popover: {
            title: 'Nueva Orden',
            description: 'Creá una nueva orden con este botón.',
            side: 'bottom',
          }
        },
      ],
      onDestroyStarted: () => {
        localStorage.setItem('tour_completado', 'true');
        tourGuiado.destroy();
      },
    });

    // Pequeño delay para que el DOM esté listo
    setTimeout(() => tourGuiado.drive(), 500);
  }, []);

  return (
    <div>
      <nav id="menu-principal">...</nav>
      <button id="btn-nueva-orden">Nueva Orden</button>
    </div>
  );
}
```

---

## Customización visual

Driver.js usa variables CSS. Pegar esto en tu CSS global:

```css
/* Cambiar colores del tour */
:root {
  --driver-popover-bg-color: #1e1e2e;        /* fondo del tooltip */
  --driver-popover-title-color: #cdd6f4;     /* color del título */
  --driver-popover-desc-color: #a6adc8;      /* color de la descripción */
  --driver-popover-border-color: #313244;    /* borde */
  --driver-btn-next-color: #1e1e2e;          /* texto botón siguiente */
  --driver-btn-next-bg-color: #cba6f7;       /* fondo botón siguiente */
  --driver-overlay-color: rgba(0, 0, 0, 0.75); /* color del overlay oscuro */
  --driver-stage-padding: 8px;               /* padding alrededor del elemento resaltado */
  --driver-popover-border-radius: 12px;      /* bordes redondeados del tooltip */
}
```

---

## Opciones de posición del tooltip

```js
side: 'top' | 'bottom' | 'left' | 'right'
align: 'start' | 'center' | 'end'
```

---

## Eventos disponibles

```js
driver({
  onHighlightStarted: (element, step) => { /* antes de resaltar */ },
  onHighlighted: (element, step) => { /* después de resaltar */ },
  onDestroyStarted: () => { /* antes de cerrar */ },
  onDestroyed: () => { /* después de cerrar */ },
  onNextClick: (element, step) => { /* click en siguiente */ },
  onPrevClick: (element, step) => { /* click en anterior */ },
})
```
