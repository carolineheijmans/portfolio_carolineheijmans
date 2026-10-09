Portfolio Build Spec — Desktop Metaphor

1. Project overview

A personal portfolio website structured as a desktop environment. The main page displays folder icons on a desktop background; each folder represents one dimension of the user's career. Clicking a folder opens its contents — projects, case studies, or work samples.

Goals:

• Simple, clean, fast — no frameworks, no build step.
• Hosted on GitHub Pages.

2. Tech constraints

HTML/CSS/JS	Plain, no frameworks, no build step
Dependencies	None (vanilla JS)
Responsive	Must work on mobile and desktop
Browser support	Modern evergreen browsers
Hosting	GitHub Pages (static)

3. Files

```
index.html             home page
project.html           projects list (squared blue cards → project pages)
spoons.html            project info page (sample; one per project)
logos.html             logos page
prints.html            prints page
designs.html           designs page
styles.css             shared stylesheet — reuse on every page
logos/ prints/ designs/  image assets per section
Boldfont-Regular.ttf   asset font ("BoldRegular")
assets/background.jpg  desktop wallpaper (extracted from Welcome.pdf)
Welcome.pdf            source doc (wallpaper origin)
```

4. Reuse rules

- Every page uses `styles.css`. Never inline page styles; add a component to `styles.css` instead.
- Keep the shell exactly: `.screen > .topbar + main + .footer`. `.screen` is the full-bleed wallpaper.
- Reusable classes: `.topbar`/`.topbar__slot`, `.desktop` (centered hero area), `.hero__title`, `.menu`+`.btn` (red pills), `.footer`+`.footer__links`, `.link`.
- Content pages (`logos.html`, `prints.html`, `designs.html`) add `.gallery`/`.gallery__item`/`.gallery__img`/`.gallery__caption` inside `.desktop`; assets live in `logos/`, `prints/`, `designs/`.
- Projects: `project.html` lists projects as `.projects`/`.project-card` (`__name` + `__desc`), each card links to a project page. `spoons.html` is the sample project page — uses `.screen--solid` (flat `--blue-bg`, dark text, no wallpaper) + `.project-info`/`__name`/`__desc`/`__links`, blue pills `.btn--blue`, and `.back-tab` (fixed bottom-left) back to `project.html`.
- Tokens in `:root` — colors (`--red`, `--red-edge`, `--blue-soft`, `--blue-bg`, `--blue`, `--ink`, `--paper`), fonts (`--font-body`, `--font-asset`, `--font-ui`), spacing (`--gutter`, `--bar-size`, `--title-size`). Change values here, not per-component.
- Fonts: body = Fragment Mono (Google Fonts `<link>` in `<head>`); display/assets = `--font-asset` = local `BoldRegular`; bold project titles = `--font-ui` (system sans 700).
- New section pages: copy the `index.html` shell, change `<title>`/content, keep `.screen`. Home menu links point at `logos.html`, `prints.html`, `designs.html`, `project.html`.
- Backgrounds live on `.screen` as `background: var(--bg-image) center/cover, linear-gradient(...)` — the gradient is the fallback; swap `--bg-image` per page to change wallpaper.

5. Local preview / verify

```sh
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless \
  --disable-gpu --hide-scrollbars --window-size=1440,810 \
  --screenshot=/tmp/render.png "file://$PWD/index.html"
# then view /tmp/render.png
```

Or serve statically: `python3 -m http.server`.

6. Rules

- No frameworks, no build step, no npm. Vanilla HTML/CSS/JS only.
- Match the reference desktop look (green hills wallpaper, red `Desktop` title, red pill buttons, mono white chrome). Preserve responsive + a11y (labels, focus states, `prefers-reduced-motion`).