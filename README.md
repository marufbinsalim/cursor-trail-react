# cursor-trail-react

[![npm version](https://img.shields.io/npm/v/cursor-trail-react.svg)](https://www.npmjs.com/package/cursor-trail-react)
[![license](https://img.shields.io/npm/l/cursor-trail-react.svg)](./LICENSE)

Animated cursor trail components and hooks for React, powered by [Framer Motion](https://www.framer.com/motion/). Build custom cursors, hover effects, and interactive trail zones with spring physics — all in a composable, type-safe API.

---

## Features

- 🌀 Spring-physics cursor tracking (`snappy`, `smooth`, `lazy` presets)
- 🎯 Zone-based trail activation via `CursorTrailArea`
- 🔌 Plug-and-play `CursorCircularTrail` component
- 🛠 `createCursorTrail` factory for fully custom trails
- 🪝 `useCursorTrail` hook for direct position/state access
- 🔒 Zero layout impact — all overlays are `pointer-events: none`
- 💡 Full TypeScript support

---

## Installation

```bash
npm install cursor-trail-react framer-motion
# or
yarn add cursor-trail-react framer-motion
# or
pnpm add cursor-trail-react framer-motion
```

> **Peer dependencies:** `react >= 17`, `react-dom >= 17`, `framer-motion >= 10`

---

## Quick Start

Wrap your app in `CursorTrailProvider`, then drop in a trail component:

```tsx
import {
  CursorTrailProvider,
  CursorCircularTrail,
} from 'cursor-trail-react';

export default function App() {
  return (
    <CursorTrailProvider>
      <CursorCircularTrail color="#6366f1" size={24} activeSize={48} />
      <main>Your app content</main>
    </CursorTrailProvider>
  );
}
```

---

## API

### `<CursorTrailProvider>`

The required context provider. Wrap your entire app (or the section where cursor tracking is needed).

```tsx
<CursorTrailProvider>
  {children}
</CursorTrailProvider>
```

---

### `<CursorCircularTrail>`

A ready-to-use circular cursor that grows when hovering interactive elements.

```tsx
<CursorCircularTrail
  size={20}           // default size in px
  activeSize={40}     // size when hovering a button/link
  color="#000"        // border color
  borderWidth={1.5}   // border thickness in px
  backgroundColor="transparent"
  blendMode="normal"  // CSS mix-blend-mode
  springPreset="snappy" // 'snappy' | 'smooth' | 'lazy'
/>
```

---

### `<CursorTrailArea>`

Activates a custom trail node when the cursor enters a zone. Supports all common HTML elements via dot notation.

```tsx
import { CursorTrailArea } from 'cursor-trail-react';

const HoverLabel = () => (
  <div style={{ position: 'fixed', /* position via useCursorTrail */ }}>
    👋 Hello!
  </div>
);

<CursorTrailArea.div trail={<HoverLabel />}>
  Hover over me
</CursorTrailArea.div>
```

**Available tags:** `div`, `section`, `article`, `aside`, `header`, `footer`, `main`, `nav`, `span`, `li`, `ul`, `a`, `button`

---

### `createCursorTrail(render, springPreset?)`

Factory for building fully custom trail components. Provides spring-tracked `x`/`y` and `isHoveringInteractive` out of the box.

```tsx
import { createCursorTrail } from 'cursor-trail-react';
import { motion } from 'framer-motion';

const EmojiTrail = createCursorTrail(({ springX, springY }) => (
  <motion.div
    style={{
      position: 'fixed',
      top: 0,
      left: 0,
      x: springX,
      y: springY,
      translateX: '-50%',
      translateY: '-50%',
      fontSize: 24,
      pointerEvents: 'none',
      zIndex: 999999,
    }}
  >
    ✨
  </motion.div>
), 'smooth'); // spring preset: 'snappy' | 'smooth' | 'lazy'
```

---

### `useCursorTrail()`

Low-level hook for accessing cursor state directly.

```tsx
import { useCursorTrail } from 'cursor-trail-react';

function MyCursor() {
  const {
    x,                    // raw cursor X
    y,                    // raw cursor Y
    springX,              // spring-animated X (MotionValue)
    springY,              // spring-animated Y (MotionValue)
    isVisible,            // is cursor inside the document?
    isHoveringInteractive,// is cursor over a link/button?
    active,               // current active trail node (ReactNode)
  } = useCursorTrail();

  // ...
}
```

Must be used inside `<CursorTrailProvider>`.

---

### Spring Presets

| Preset    | Feel                         |
|-----------|------------------------------|
| `snappy`  | Fast, tight response         |
| `smooth`  | Balanced, natural            |
| `lazy`    | Slow, floaty                 |

```tsx
import { SPRING_SNAPPY, SPRING_SMOOTH, SPRING_LAZY, resolveSpring } from 'cursor-trail-react';
```

---

## Advanced Example

```tsx
import {
  CursorTrailProvider,
  CursorTrailArea,
  createCursorTrail,
  useCursorTrail,
} from 'cursor-trail-react';
import { motion } from 'framer-motion';

const MagneticDot = createCursorTrail(({ springX, springY, isHoveringInteractive }) => (
  <motion.div
    style={{
      position: 'fixed',
      top: 0,
      left: 0,
      x: springX,
      y: springY,
      translateX: '-50%',
      translateY: '-50%',
      width: isHoveringInteractive ? 48 : 12,
      height: isHoveringInteractive ? 48 : 12,
      borderRadius: '50%',
      background: '#7c3aed',
      pointerEvents: 'none',
      zIndex: 999999,
      transition: 'width 0.2s, height 0.2s',
    }}
  />
), 'snappy');

export default function App() {
  return (
    <CursorTrailProvider>
      <MagneticDot />
      <CursorTrailArea.section trail={<div>🔥 Hot zone!</div>}>
        <h1>Hover here for a special trail</h1>
      </CursorTrailArea.section>
    </CursorTrailProvider>
  );
}
```

---

## License

MIT
# cursor-trail-react
