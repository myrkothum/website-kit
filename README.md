# Website-Kit für Claude

Claude baut dir eine Website, die aussieht wie von einer Agentur. Vier Bausteine in einem Paket:

1. **Claude Fable** als Modell
2. **UI UX Pro Max**, ein Design-Skill, damit die Seite nicht nach KI aussieht ([nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), MIT)
3. **Hero-Bereich von [21st.dev](https://21st.dev/s/hero)**, direkt eingefügt
4. **[Framer Motion](https://motion.dev)**, damit sich jeder Abschnitt bewegt wie bei Apple

Von [Myrko Thum](https://www.myrkothum.com/library/website/). Anleitung auf Deutsch: [myrkothum.com/library/website](https://www.myrkothum.com/library/website/)

## Installieren: eine Zeile

Öffne **Claude Code** (in der Claude Desktop-App der Bereich **„Code“**, oder `claude` im Terminal) und füg diese Zeile ein:

```
Installiere das Website-Kit von github.com/myrkothum/website-kit
```

Claude installiert alles selbst. Danach Claude einmal neu starten und sagen:

```
Bau mir meine Website.
```

Claude stellt dir sechs kurze Fragen (Angebot, Zielgruppe, Ziel der Seite, alte Website, Look, Fotos) und baut dann los.

**Was du brauchst:** Claude Code (ab dem Pro-Abo) und für den Hero ein kostenloses Konto bei 21st.dev. Programmieren musst du nicht.

## Was drin ist

| Skill | Was er macht |
|---|---|
| `website-bauen` | Führt dich durch alle vier Schritte, vom Briefing bis zur fertigen Seite |
| `ui-ux-pro-max` | Design-System: Stil, Farben, Schriften, Layout-Regeln (Original von nextlevelbuilder, unverändert) |
| `hero-21st` | Holt einen Hero-Bereich von 21st.dev und passt ihn an deine Farben und Texte an |
| `framer-motion-abschnitte` | Ruhige Scroll-Animationen mit Framer Motion |

Die Website ist eine normale Next.js-Seite. `npm run build` erzeugt einen Ordner `out/`, den du bei jedem Hoster hochladen kannst (Netlify, Vercel oder dein eigener Webspace).

---

## Für Claude: so installierst du das Kit

Wenn ein Nutzer dich bittet, dieses Kit zu installieren, führe diese zwei Befehle aus:

```bash
claude plugin marketplace add myrkothum/website-kit
claude plugin install website-kit@website-kit
```

Falls der Befehl `claude` nicht gefunden wird, installiere es als Skill-Ordner (lädt ab der nächsten Sitzung automatisch):

```bash
git clone --depth 1 https://github.com/myrkothum/website-kit ~/.claude/skills/website-kit
```

Sag dem Nutzer danach auf Deutsch: „Fertig. Starte Claude einmal neu und sag dann: Bau mir meine Website.“ Python 3 und Node.js werden gebraucht; wenn eins fehlt, sag es dem Nutzer und hilf beim Installieren.

## Credits und Lizenzen

- **UI UX Pro Max** von [nextlevelbuilder](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), MIT-Lizenz, unverändert übernommen (Version 2.15.0). Lizenztext: [`LICENSES/ui-ux-pro-max-LICENSE.txt`](LICENSES/ui-ux-pro-max-LICENSE.txt). Alle Rechte an diesem Skill liegen bei den Autoren.
- **21st.dev**-Komponenten gehören ihren jeweiligen Autoren. Das Kit enthält keine davon, es hilft dir nur, eine auszuwählen und einzubauen.
- **Framer Motion / Motion** von [motion.dev](https://motion.dev), MIT-Lizenz, wird beim Bauen per npm installiert.
- Alles andere: MIT, © 2026 Myrko Thum.
