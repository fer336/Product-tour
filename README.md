# Product Tour Skill

Repositorio con una skill y material de referencia para implementar **product tours**, **onboarding guiado**, **tutoriales interactivos** y **demos en video** dentro de aplicaciones web.

El contenido principal está en `SKILL.md` y en la carpeta `references/`, donde hay guías prácticas para trabajar con alternativas como **Driver.js**, **CSS custom** y **Remotion**.

## Prerrequisitos

- [Git](https://git-scm.com/)
- [Node.js 18 o superior](https://nodejs.org/)
- npm (incluido con Node.js)

## Instalación desde GitHub

1. Cloná el repositorio:

   ```bash
   git clone https://github.com/fer336/Product-tour.git
   ```

2. Entrá a la carpeta del proyecto:

   ```bash
   cd Product-tour
   ```

3. Instalá las dependencias del proyecto:

   ```bash
   npm install
   ```

> Este repositorio no define dependencias de runtime por defecto. El `package.json` existe para que el proyecto pueda instalarse correctamente con npm al clonarlo y para dejar una base estándar si querés ampliarlo después.

## Uso básico

### 1. Revisar la guía principal

- `SKILL.md`: explica cuándo usar esta skill y cómo elegir la herramienta correcta según el contexto.

### 2. Consultar las referencias disponibles

- `references/libraries.md`: comparación de librerías para product tours.
- `references/driverjs.md`: implementación con Driver.js.
- `references/custom-css.md`: implementación sin librerías.
- `references/remotion.md`: demo animada como video con Remotion.

### 3. Tomar una base según tu caso de uso

- **App web interactiva**: empezá por `references/driverjs.md`.
- **Máximo control visual sin dependencias**: usá `references/custom-css.md`.
- **Demo o onboarding en video**: revisá `references/remotion.md`.

## Scripts disponibles

Actualmente el repositorio expone un único script mínimo:

```bash
npm test
```

Ese comando informa que **no hay tests automatizados configurados** en este repositorio. Se dejó como script conservador para mantener una estructura npm válida sin inventar procesos que hoy no existen.

## Estructura del proyecto

```text
.
├── SKILL.md
├── package.json
└── references/
    ├── custom-css.md
    ├── driverjs.md
    ├── libraries.md
    └── remotion.md
```

## Cómo contribuir

1. Hacé un fork del repositorio o creá una rama nueva.
2. Mantené el contenido en español y con ejemplos claros.
3. Si agregás una nueva referencia, enlazala desde `SKILL.md` o desde `references/libraries.md` cuando corresponda.
4. Verificá que los comandos y snippets documentados existan de verdad antes de proponerlos.
5. Abrí un Pull Request explicando qué problema resuelve tu aporte.

## Notas

- Este repositorio hoy funciona principalmente como **documentación + skill reusable**, no como librería de Node.js lista para importar con `require()` o `import`.
- Si más adelante querés convertirlo en un paquete ejecutable o publicable, conviene definir un entrypoint real, tests y scripts de validación.
