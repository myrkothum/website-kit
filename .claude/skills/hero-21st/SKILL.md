---
name: hero-21st
description: Adds a hero section from 21st.dev (community React + Tailwind + shadcn components) to a website project and adapts it to the project's design tokens and copy. Use inside the website-bauen flow, or when the user says "Hero von 21st.dev", "neuer Hero", "21st", or pastes a 21st.dev component prompt or code.
---

# Hero von 21st.dev

21st.dev is a free library of React components (Tailwind + shadcn/ui) made by the community.
The hero is the first screen of the site, so it carries most of the "agency look".

Talk to the user in their language (default German, "du"). They are not developers.

## 1 · Let the user pick (2 minutes)

Send this, adapted:

> Such dir einen Hero aus: https://21st.dev/s/hero
> Klick auf einen, der dir gefällt, und dann auf **„Copy prompt“** (oder kopier den Code). Dafür brauchst du ein kostenloses Konto bei 21st.dev.
> Füg das Kopierte einfach hier in den Chat ein. Wenn du keine Lust hast zu suchen, sag „such du aus“.

If they say "such du aus": pick a hero that fits the design system (calm and premium for coaches:
centered headline + subline + one CTA, subtle background; avoid heavy WebGL/3D unless they want it).
Show them the link of your pick and ask for the copied prompt/code, or build it yourself in that style.

Optional (for users with a 21st.dev account who are fine with the terminal):
`npx @21st-dev/cli@latest login` then `npx @21st-dev/cli@latest add <author>/<component>` installs it straight into the project.
Free accounts have a small daily limit; if the CLI says so, fall back to copy & paste.

## 2 · Install what the user pasted

The pasted prompt/code usually contains: the component file, a demo file, dependencies (npm packages
and shadcn components) and sometimes extra CSS/Tailwind keyframes.

1. Put the component into `src/components/ui/<name>.tsx` (shadcn convention, `@/` alias).
2. Install every listed npm dependency (`npm install …`) and shadcn component (`npx shadcn@latest add …`).
3. Add extra CSS/keyframes to `globals.css` (Tailwind v4: `@theme`/`@keyframes` there, no tailwind.config needed).
4. Keep `"use client"` at the top when the component uses hooks or motion.
5. Use it in the page as `<HeroSection />` (or its export name). Remove the demo file.

## 3 · Make it theirs

- Replace all demo text, logos and images with the user's copy and assets. No placeholder brands left.
- Swap hard-coded colors for the design tokens (`bg-primary`, `text-foreground`, CSS variables).
- One clear CTA (their booking/opt-in link), optional second link. Headline = the result their client wants.
- Check mobile (375 px): headline readable, CTA visible without scrolling, no horizontal scroll.
- Respect `prefers-reduced-motion` if the hero animates.

## Credit

Components on 21st.dev belong to their authors. Keep any license header in the file.
