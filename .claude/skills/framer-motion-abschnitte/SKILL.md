---
name: framer-motion-abschnitte
description: Animates website sections with Framer Motion (npm package "motion", import from "motion/react") in a calm, Apple-like way — fade and rise on scroll, staggered cards, scroll-linked hero, sticky reveal. Use inside the website-bauen flow or when the user says "animier den Abschnitt", "Framer Motion", "wie bei Apple", "mehr Bewegung", "Scroll-Animation".
---

# Abschnitte animieren mit Framer Motion

Framer Motion is now published as `motion`. Install: `npm install motion`. Import: `import { motion } from "motion/react"`.
Every animated component is a client component (`"use client"` at the top).

## Rules (the Apple feel)

- Subtle: move 16–40 px, fade from 0 to 1, 0.5–0.8 s, ease `[0.22, 1, 0.36, 1]`.
- Animate once when the section enters the viewport (`viewport={{ once: true, amount: 0.3 }}`).
- Animate `opacity` and `transform` only (never width/height/top) to keep it smooth.
- One idea per section. Not every element moves.
- Always respect reduced motion: `useReducedMotion()` → skip the movement, show the content.

## Building blocks

Copy these into `src/components/motion/` and use them around sections.

### 1 · Reveal (fade + rise on scroll)

```tsx
"use client";
import { motion, useReducedMotion } from "motion/react";

export function Reveal({ children, delay = 0, className }: { children: React.ReactNode; delay?: number; className?: string }) {
  const reduce = useReducedMotion();
  return (
    <motion.div
      className={className}
      initial={reduce ? false : { opacity: 0, y: 32 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.3 }}
      transition={{ duration: 0.7, delay, ease: [0.22, 1, 0.36, 1] }}
    >
      {children}
    </motion.div>
  );
}
```

### 2 · Stagger (cards one after another)

```tsx
"use client";
import { motion, useReducedMotion, type Variants } from "motion/react";

const container: Variants = { hidden: {}, show: { transition: { staggerChildren: 0.12 } } };
const item: Variants = { hidden: { opacity: 0, y: 24 }, show: { opacity: 1, y: 0, transition: { duration: 0.6, ease: [0.22, 1, 0.36, 1] } } };

export function Stagger({ children, className }: { children: React.ReactNode; className?: string }) {
  const reduce = useReducedMotion();
  return (
    <motion.div className={className} variants={container} initial={reduce ? false : "hidden"} whileInView="show" viewport={{ once: true, amount: 0.2 }}>
      {children}
    </motion.div>
  );
}
export const StaggerItem = ({ children, className }: { children: React.ReactNode; className?: string }) => (
  <motion.div className={className} variants={item}>{children}</motion.div>
);
```

### 3 · Scroll-linked image (Apple product feel)

```tsx
"use client";
import { useRef } from "react";
import { motion, useScroll, useTransform, useReducedMotion } from "motion/react";

export function ScrollScale({ children }: { children: React.ReactNode }) {
  const ref = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();
  const { scrollYProgress } = useScroll({ target: ref, offset: ["start end", "end start"] });
  const scale = useTransform(scrollYProgress, [0, 0.5], [0.88, 1]);
  const opacity = useTransform(scrollYProgress, [0, 0.35], [0.4, 1]);
  return (
    <motion.div ref={ref} style={reduce ? undefined : { scale, opacity }}>
      {children}
    </motion.div>
  );
}
```

### 4 · Big statement, word by word

```tsx
"use client";
import { motion, useReducedMotion } from "motion/react";

export function WordReveal({ text, className }: { text: string; className?: string }) {
  const reduce = useReducedMotion();
  const words = text.split(" ");
  return (
    <p className={className} aria-label={text}>
      {words.map((w, i) => (
        <motion.span
          key={i}
          aria-hidden
          className="inline-block mr-[0.25em]"
          initial={reduce ? false : { opacity: 0.15 }}
          whileInView={{ opacity: 1 }}
          viewport={{ once: true, amount: 0.8 }}
          transition={{ duration: 0.4, delay: i * 0.06 }}
        >
          {w}
        </motion.span>
      ))}
    </p>
  );
}
```

## Where to use what

| Section | Block |
|---|---|
| Problem / statement | `WordReveal` for the one big sentence |
| Offer / steps / benefits | `Stagger` + `StaggerItem` per card |
| Screenshot, photo, mockup | `ScrollScale` |
| Everything else (headlines, testimonials, FAQ, CTA) | `Reveal` |

After adding motion: scroll the page once on desktop and mobile. If anything jumps, stutters or
hides content above the fold, tone it down. Content must be readable even if JavaScript is slow.
