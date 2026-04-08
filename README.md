# Particles

An interactive 3D particle system rendered in the browser using [Three.js](https://threejs.org/). 150,000 particles are distributed across a volumetric sphere, each individually sized and colored, and animated in real time via custom GLSL shaders.

## Features

- **Custom GLSL shaders** — vertex and fragment shaders handle per-particle sizing and soft radial gradient textures via additive blending, producing a luminous, spark-like appearance.
- **Full-spectrum color mapping** — particles are colored across the HSL spectrum, creating a smooth rainbow distribution across the system.
- **Animated pulsing** — each particle's size oscillates independently using a sine function offset by its index, giving the cloud an organic, breathing quality.
- **Continuous rotation** — the particle system rotates along the Z axis over time.
- **Responsive layout** — the canvas resizes correctly on window resize, maintaining the correct camera aspect ratio.

## Tech Stack

| Tool | Purpose |
|---|---|
| [Three.js](https://threejs.org/) `v0.170.0` | 3D rendering via WebGL |
| [Vite](https://vitejs.dev/) | Local development server |
| GLSL | Custom vertex and fragment shaders |

Three.js is loaded from a CDN via an [import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap) — no npm install required for the Three.js dependency itself.

## Prerequisites

- [Node.js](https://nodejs.org/) (v18 or later recommended)
- npm (included with Node.js)

## Getting Started

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd particles-project
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Start the development server**

   ```bash
   npm run dev
   ```

4. **Open in your browser**

   Navigate to `http://localhost:5173` (or whichever port Vite reports in the terminal).

## Project Structure

```
.
├── src/
│   └── index.html      # Entry point — markup, styles, shaders, and application logic
└── vite.config.js      # Vite configuration (sets src/ as the project root)
```

## How It Works

On load, `init()` sets up a Three.js scene with a perspective camera positioned 300 units back along the Z axis. A `BufferGeometry` is populated with 150,000 randomly positioned vertices within a 200-unit radius sphere. Each vertex is assigned:

- a position in 3D space
- an HSL color based on its index in the particle array
- an initial size attribute

A `ShaderMaterial` reads these per-vertex attributes and renders each particle as a soft, glowing point sprite using a procedurally generated radial gradient texture. Additive blending means overlapping particles brighten each other, enhancing the glow effect.

Each animation frame, the size buffer is rewritten with a sine wave function — `10 * (1 + sin(0.1 * i + time))` — producing the pulsing animation without any physics simulation.

## License

This project is provided as-is for personal and educational use.
