---
name: website-bauen
description: Builds a complete, agency-quality website or landing page for a coach, consultant or small business — best model (Claude Fable), design system from the UI UX Pro Max skill, art direction from the Taste Skill, a hero section from 21st.dev, section animations with Framer Motion. Use when the user says "bau mir eine Website", "Website bauen", "Landingpage", "neue Webseite", "Website-Kit", "build me a website", or pastes their old website and wants a better one.
---

# Website bauen (Website-Kit)

You build a real, modern website the user can open in the browser, change and put online.
The user is usually a coach, consultant or expert, **not a developer**. Talk to them in their
language (default: German, informal "du"), short sentences, no jargon. You do the technical work.

The kit has five building blocks. Use all of them, in this order:

1. **Model:** Claude Fable.
2. **Design system:** skill `ui-ux-pro-max` (by nextlevelbuilder, MIT): colors, fonts, layout rules.
3. **Taste:** skill `design-taste-frontend` from the Taste Skill plugin (by Leonxlnx, MIT): art direction from the best company sites, anti-template rules, browser check at the end.
4. **Hero:** skill `hero-21st`: a hero section from 21st.dev.
5. **Motion:** skill `framer-motion-abschnitte`: section animations with Framer Motion (package `motion`).

If the Taste Skill is not installed (`design-taste-frontend` missing), tell the user once to run
`claude plugin install taste-skill@website-kit`, then continue without it.

## Step 0 · Model check (one line)

Tell the user once: "Tipp: Das Ergebnis wird am besten mit Claude Fable. Tipp `/model` und wähl Fable, falls es noch nicht eingestellt ist." Then continue. Don't block on it.

## Step 1 · Briefing (one message, max 6 questions)

Ask everything in ONE message, numbered, so they can answer in one go:

1. Name + what you offer (one sentence).
2. Who is it for (your ideal client)?
3. What should a visitor do? (e.g. book a call → link, join a waitlist, buy)
4. Do you have a current website or LinkedIn/Instagram profile? → link (you read it and take facts, offer, tone, testimonials from it)
5. Look & feel: colors / brand / a website you like? ("weiß ich nicht" is fine → you decide)
6. Photos/logo: drag them into the chat or name a folder (optional).

If they paste a URL, fetch and read it before you design. Never invent facts, numbers,
client names or testimonials. Where proof is missing, put a clearly marked placeholder
(`[Kundenstimme einfügen]`) and list all placeholders at the end.

## Step 2 · Design system (ui-ux-pro-max)

Invoke the `ui-ux-pro-max` skill and run its design-system search with the user's product + audience, e.g.

```bash
python3 "<ui-ux-pro-max skill dir>/scripts/search.py" "coaching landing page <niche> <audience>" --design-system --persist -p "<Brand>" --output-dir "<project dir>"
```

(Inside the plugin the skill dir is `${CLAUDE_PLUGIN_ROOT}/.claude/skills/ui-ux-pro-max`. If that variable is empty, find the folder with `find ~/.claude -type d -name ui-ux-pro-max`.)
If the user gave brand colors, they win over the suggested palette; keep the rest (type scale, spacing, anti-patterns).
Show the user the result in 3 lines (style, colors, fonts) and go on unless they object.

## Step 2b · Taste (Taste Skill)

Invoke `design-taste-frontend`. Apply its house rules and its layout and copy references on top of
the design system: one section per screen, equal edges, real photos and real product visuals instead of
stock icons, plain copy. The user's brand colors and the ui-ux-pro-max tokens still win on color and type.
It needs Node 22+ and Chrome/Edge/Chromium for its checker; if they are missing, use its rules without the checker.
Where the Taste Skill and this flow disagree on the stack, keep this flow's Next.js setup (Step 3) and
use the Taste Skill for direction, sections and the final gate.

## Step 3 · Project setup

Create the project in a new folder (default `~/Websites/<brand-slug>`; ask only if they care):

```bash
npx create-next-app@latest <brand-slug> --ts --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm --yes
cd <brand-slug>
npx shadcn@latest init -d
npm install motion lucide-react tw-animate-css
```

- Static site: **add** `output: "export"` and `images: { unoptimized: true }` to the existing `next.config.ts`. Keep every key that is already there (newer Next.js versions load Tailwind through a `turbopack` rule in that file; replacing the file breaks all styling). Remove only `cacheComponents` and `partialPrefetching` if present: they don't work with a static export ("PPR cannot be enabled in export mode"). Then `npm run build` gives a plain `out/` folder that runs on any host.
- If the build says `Can't resolve 'tw-animate-css'` or another CSS import, install that package and build again.
- Put the design tokens from Step 2 into `src/app/globals.css` (CSS variables, light + dark) and load the fonts with `next/font/google`.
- `lang="de"` on `<html>` for German sites. Real `<title>` and meta description.

## Step 4 · Hero (hero-21st)

Invoke `hero-21st`. It gives the user 3 hero options from 21st.dev and installs the chosen one.
If the user wants to skip, build a strong hero yourself with the same tokens.

## Step 5 · Sections

Follow the section pattern from the design system. A good default for coaches:

Hero → Problem (in the client's words) → Angebot / so arbeitest du → Ergebnisse / Stimmen → Über dich → FAQ (3–5) → Abschluss-CTA → Footer with Impressum + Datenschutz links.

Copy rules: concrete, the client's words, one CTA repeated, no AI filler ("nahtlos", "auf das nächste Level",
"entfesseln", "in der heutigen schnelllebigen Welt", em-dash chains). Short paragraphs. German sites
need `/impressum` and `/datenschutz` pages: create them with placeholders and tell the user to fill them.

## Step 6 · Motion (framer-motion-abschnitte)

Invoke `framer-motion-abschnitte` and animate sections the Apple way: subtle, on scroll, once.
Always respect `prefers-reduced-motion`.

## Step 7 · Check and show

1. `npm run dev`, open the page (use the browser/preview tool if you have one), check desktop and 375 px mobile.
2. Run the Taste Skill checker/gate if available, and the ui-ux-pro-max pre-delivery checklist (contrast 4.5:1, focus rings, no horizontal scroll, touch targets 44 px, alt texts).
3. `npm run build` must pass with zero errors.
4. Tell the user in 5 lines max: where the site is, how to open it (`npm run dev` → http://localhost:3000), the placeholder list, and the next step.

## Step 8 · Online (only when the user asks)

Explain the two easy ways, don't do it without their OK:
- **Netlify Drop / Vercel:** upload the `out/` folder (free tier), later connect your domain.
- **Own hosting:** upload the content of `out/` via FTP/SFTP into the web folder.

## Changes later

The user can just say "mach den Hero dunkler", "neuer Abschnitt mit Preisen", "Text kürzer". Edit, re-check mobile, rebuild.
