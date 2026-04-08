# Luminos

An interactive 3D particle system rendered in the browser using [Three.js](https://threejs.org/). 150,000 particles are distributed across a volumetric sphere, each individually sized and colored, and animated in real time via custom GLSL shaders.

## Controls

| Input | Action |
|---|---|
| Left-drag | Orbit the camera |
| Right-drag | Pan |
| Scroll | Zoom in / out |
| `Space` | Explode / implode particles |
| `1` | Cloud formation |
| `2` | Sphere formation |
| `3` | Torus formation |
| `4` | Helix formation |

Switching formations or imploding resets the camera to the default view.

## Features

- **Custom GLSL shaders** — vertex and fragment shaders handle per-particle sizing and soft radial gradient textures via additive blending, producing a luminous, spark-like appearance.
- **Full-spectrum color mapping** — particles are colored across the HSL spectrum, creating a smooth rainbow distribution across the system.
- **Animated pulsing** — each particle's size oscillates independently using a sine function offset by its index, giving the cloud an organic, breathing quality.
- **Turbulence** — each particle has a randomised per-axis phase offset, producing independent organic drift across all formations.
- **Formation morphing** — particles smoothly interpolate between four 3D shapes (cloud, sphere, torus, double helix) using smooth-step easing.
- **Explode / implode** — particles scale outward to 5× their positions and spring back, composited on top of any active morph or turbulence.
- **Responsive layout** — the canvas resizes correctly on window resize, maintaining the correct camera aspect ratio.

## Tech Stack

| Tool | Purpose |
|---|---|
| [Three.js](https://threejs.org/) `v0.170.0` | 3D rendering via WebGL |
| [Vite](https://vitejs.dev/) | Local development server |
| GLSL | Custom vertex and fragment shaders |

Three.js is loaded from a CDN via an [import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap) — no build step required.

## Project Structure

```
.
├── src/
│   ├── index.html    # Entry point — markup, shaders, and application logic
│   └── style.css     # Styles
└── vercel.json       # Vercel deployment config
```

## How It Works

On load, `init()` sets up a Three.js scene with a perspective camera positioned 300 units back along the Z axis. A `BufferGeometry` is populated with 150,000 randomly positioned vertices within a 200-unit radius sphere. Each vertex is assigned:

- a position in 3D space
- an HSL color based on its index in the particle array
- an initial size attribute

A `ShaderMaterial` reads these per-vertex attributes and renders each particle as a soft, glowing point sprite using a procedurally generated radial gradient texture. Additive blending means overlapping particles brighten each other, enhancing the glow effect.

Each animation frame, the position buffer is rewritten by combining three layers: a smooth-step morph between the current and target formation, per-particle sine-based turbulence (independent phase per axis), and an optional explode scale. The size buffer is driven by a separate sine wave per particle, producing the pulsing effect.

## License

This project is provided as-is for personal and educational use.
