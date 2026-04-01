# Remotion — Product Tour como video animado

Remotion **no es interactivo** — genera videos MP4 programáticamente con React.
Usar cuando se quiere una **demo en video** del sistema (para landing page, YouTube, onboarding pasivo).

## Instalación

```bash
npm create video@latest
# Elegir "blank" template
```

## Estructura del componente

```tsx
// src/ProductTour.tsx
import {
  Sequence,
  useCurrentFrame,
  interpolate,
  spring,
  useVideoConfig,
  AbsoluteFill,
} from 'remotion';

// Datos de cada paso del tour
const PASOS = [
  {
    selector: { x: 0, y: 0, width: 200, height: 600 },  // coordenadas del elemento
    titulo: 'Menú Principal',
    descripcion: 'Navegá a todas las secciones desde aquí.',
    tooltipLado: 'derecha' as const,
  },
  {
    selector: { x: 220, y: 80, width: 160, height: 44 },
    titulo: 'Nueva Orden',
    descripcion: 'Creá una nueva orden de trabajo.',
    tooltipLado: 'abajo' as const,
  },
];

const FRAMES_POR_PASO = 90; // 3 segundos a 30fps

export const ProductTour = () => {
  return (
    <AbsoluteFill>
      {/* Screenshot de tu UI de fondo */}
      <img
        src="./public/screenshot-dashboard.png"
        style={{ width: '100%', height: '100%', objectFit: 'cover' }}
      />

      {/* Renderizar cada paso en su Sequence */}
      {PASOS.map((paso, i) => (
        <Sequence
          key={i}
          from={i * FRAMES_POR_PASO}
          durationInFrames={FRAMES_POR_PASO}
        >
          <PasoTour paso={paso} />
        </Sequence>
      ))}
    </AbsoluteFill>
  );
};

// Componente de un paso individual
const PasoTour = ({ paso }: { paso: typeof PASOS[0] }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Animación de entrada suave
  const opacidad = spring({ frame, fps, config: { damping: 20 } });

  const { x, y, width, height } = paso.selector;

  // Posición del tooltip según el lado
  const tooltipStyle = paso.tooltipLado === 'derecha'
    ? { left: x + width + 20, top: y }
    : { left: x, top: y + height + 20 };

  return (
    <AbsoluteFill style={{ opacity: opacidad }}>
      {/* Overlay oscuro con "agujero" usando box-shadow */}
      <div style={{
        position: 'absolute',
        left: x - 8,
        top: y - 8,
        width: width + 16,
        height: height + 16,
        borderRadius: 8,
        boxShadow: '0 0 0 9999px rgba(0,0,0,0.65)',
        outline: '3px solid #6366f1',
      }} />

      {/* Tooltip */}
      <div style={{
        position: 'absolute',
        ...tooltipStyle,
        background: 'white',
        borderRadius: 12,
        padding: '16px 20px',
        maxWidth: 260,
        boxShadow: '0 8px 32px rgba(0,0,0,0.3)',
      }}>
        <div style={{ fontWeight: 700, fontSize: 16, marginBottom: 8 }}>
          {paso.titulo}
        </div>
        <div style={{ fontSize: 14, color: '#555' }}>
          {paso.descripcion}
        </div>
      </div>
    </AbsoluteFill>
  );
};
```

```tsx
// src/Root.tsx
import { Composition } from 'remotion';
import { ProductTour } from './ProductTour';

const TOTAL_PASOS = 2;
const FRAMES_POR_PASO = 90;

export const RemotionRoot = () => (
  <Composition
    id="ProductTour"
    component={ProductTour}
    durationInFrames={TOTAL_PASOS * FRAMES_POR_PASO}
    fps={30}
    width={1280}
    height={720}
  />
);
```

## Renderizar el video

```bash
# Preview en el browser
npm run dev

# Exportar como MP4
npx remotion render ProductTour output/tour.mp4
```

## Tips para sacar el screenshot de la UI

```bash
# Con Playwright (automatizado)
npx playwright screenshot http://localhost:3000 screenshot-dashboard.png --full-page

# Copiar a la carpeta public de Remotion
cp screenshot-dashboard.png mi-remotion-project/public/
```
