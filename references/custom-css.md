# Custom CSS — Product Tour sin librerías

Útil cuando no se pueden agregar dependencias o se necesita control total del diseño.

## Técnica del box-shadow gigante

El truco del spotlight es un `div` posicionado **exactamente sobre el elemento** con un `box-shadow` de radio enorme que "tiñe" todo el resto de la pantalla.

```css
.spotlight {
  position: fixed;
  z-index: 9998;
  border-radius: 8px;
  /* El box-shadow gigante hace el overlay oscuro */
  box-shadow: 0 0 0 9999px rgba(0, 0, 0, 0.65);
  transition: all 0.3s ease;
  pointer-events: none; /* el usuario puede hacer click en el elemento resaltado */
}

.tour-tooltip {
  position: fixed;
  z-index: 9999;
  background: #fff;
  border-radius: 12px;
  padding: 20px 24px;
  max-width: 280px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
  transition: all 0.3s ease;
}

.tour-tooltip h3 {
  margin: 0 0 8px;
  font-size: 16px;
  font-weight: 600;
}

.tour-tooltip p {
  margin: 0 0 16px;
  font-size: 14px;
  color: #555;
}

.tour-tooltip .tour-buttons {
  display: flex;
  gap: 8px;
  justify-content: flex-end;
}

.tour-btn {
  padding: 8px 16px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.tour-btn-next {
  background: #6366f1;
  color: white;
}

.tour-btn-skip {
  background: transparent;
  color: #888;
}
```

```js
class ProductTour {
  constructor(pasos) {
    this.pasos = pasos;
    this.pasoActual = 0;
    this.spotlight = null;
    this.tooltip = null;
  }

  iniciar() {
    // Crear el spotlight
    this.spotlight = document.createElement('div');
    this.spotlight.className = 'spotlight';
    document.body.appendChild(this.spotlight);

    // Crear el tooltip
    this.tooltip = document.createElement('div');
    this.tooltip.className = 'tour-tooltip';
    document.body.appendChild(this.tooltip);

    this.mostrarPaso(0);
  }

  mostrarPaso(index) {
    const paso = this.pasos[index];
    const elemento = document.querySelector(paso.selector);
    if (!elemento) return;

    // Posicionar el spotlight sobre el elemento
    const rect = elemento.getBoundingClientRect();
    this.spotlight.style.cssText = `
      position: fixed;
      top: ${rect.top - 8}px;
      left: ${rect.left - 8}px;
      width: ${rect.width + 16}px;
      height: ${rect.height + 16}px;
    `;

    // Posicionar el tooltip (abajo del elemento por defecto)
    const tooltipTop = rect.bottom + 16;
    const tooltipLeft = Math.max(16, Math.min(rect.left, window.innerWidth - 312));

    const esUltimo = index === this.pasos.length - 1;

    this.tooltip.style.cssText = `
      top: ${tooltipTop}px;
      left: ${tooltipLeft}px;
    `;

    this.tooltip.innerHTML = `
      <h3>${paso.titulo}</h3>
      <p>${paso.descripcion}</p>
      <div class="tour-buttons">
        <button class="tour-btn tour-btn-skip" onclick="window._tour.terminar()">
          Saltar tour
        </button>
        <button class="tour-btn tour-btn-next" onclick="window._tour.siguiente()">
          ${esUltimo ? 'Entendido ✓' : 'Siguiente →'}
        </button>
      </div>
    `;
  }

  siguiente() {
    if (this.pasoActual < this.pasos.length - 1) {
      this.pasoActual++;
      this.mostrarPaso(this.pasoActual);
    } else {
      this.terminar();
    }
  }

  terminar() {
    this.spotlight?.remove();
    this.tooltip?.remove();
    localStorage.setItem('tour_completado', 'true');
  }
}

// Uso
window._tour = new ProductTour([
  {
    selector: '#menu-principal',
    titulo: 'Menú Principal',
    descripcion: 'Desde aquí navegás a todas las secciones del sistema.',
  },
  {
    selector: '#btn-nueva-orden',
    titulo: 'Nueva Orden',
    descripcion: 'Hacé click aquí para crear una nueva orden de trabajo.',
  },
  {
    selector: '#panel-reportes',
    titulo: 'Reportes',
    descripcion: 'Acá ves el resumen de actividad.',
  },
]);

// Iniciar si no lo vio antes
if (!localStorage.getItem('tour_completado')) {
  window._tour.iniciar();
}
```
