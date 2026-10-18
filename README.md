# tanzeemsakib.github.io

Personal portfolio of **Tanzeem Ahmed**, a product designer and visual artist based in Dhaka, Bangladesh.

**Live site:** https://tanzeemsakib.github.io

The whole site is one hand-written `index.html` (HTML, CSS and JavaScript in a single file) plus a folder of images. There is no framework, no bundler and no build step.

---

## Contents

- [Features](#features)
- [Tech stack](#tech-stack)
- [Project structure](#project-structure)
- [Page sections](#page-sections)
- [Running it locally](#running-it-locally)
- [Deploying](#deploying)
- [Editing guide](#editing-guide)
- [How the interactions work](#how-the-interactions-work)
- [Fallbacks and accessibility](#fallbacks-and-accessibility)
- [SEO and link previews](#seo-and-link-previews)
- [Responsive behavior](#responsive-behavior)
- [Performance notes](#performance-notes)
- [Browser support](#browser-support)
- [Credits](#credits)
- [License and contact](#license-and-contact)

---

## Features

- Single-page layout: Hero, Work, Illustration, Games, About and Contact
- Editorial blue-and-white design with a **Night / Day** toggle (the choice is remembered, and the first visit follows the system setting)
- Line-grid background that **bends away from the cursor** and sends a **ripple** through the grid on click
- Floating navigation "island" that expands into a menu, with a character-by-character name reveal
- Smooth scrolling and scroll-triggered reveals
- WebGL **bulge hover** on the illustration grid
- Cursor-following labels on case studies and games ("View Case Study", "Play Game")
- Lazy-loaded images, responsive grids, and full reduced-motion support
- SEO and social-sharing metadata, including Person structured data

---

## Tech stack

Everything is vanilla. The only dependencies are small libraries loaded from CDNs.

| Library | Version | Loaded from | Used for |
| --- | --- | --- | --- |
| [Lenis](https://github.com/darkroomengineering/lenis) | 1.1.14 | cdnjs | Smooth scrolling |
| [GSAP](https://gsap.com) | 3.15 | jsDelivr | Navigation island and menu timeline |
| GSAP SplitText | 3.15 | jsDelivr | Rolling-character name reveal in the island |
| [anime.js](https://animejs.com) | 4 | jsDelivr (ES module, dynamic import) | Fade-in of the Work rows on scroll |
| [OGL](https://github.com/oframe/ogl) | 1 | jsDelivr (ES module) | WebGL bulge effect on Illustration |
| Canvas 2D API | native | n/a | Interactive line-grid background |
| IntersectionObserver | native | n/a | Scroll reveals and lazy preparation of WebGL canvases |

**Fonts** (Google Fonts): DM Serif Display (headings and names) and DM Sans (body, weights 300, 400 and 500).

**Hosting:** GitHub Pages.

---

## Project structure

```
.
├── index.html              # the entire site: markup, styles and scripts
├── README.md
├── favicon.ico
├── favicon-32x32.png
├── favicon-16x16.png
├── apple-touch-icon.png
├── site.webmanifest
└── assets/
    ├── og-preview.png                        # social share image
    ├── resume.pdf
    ├── shwo-logo-branding.svg                # experience logos
    ├── bjit-logo-branding.svg
    │
    ├── ozmo-consult-mockup.png               # Work thumbnails
    ├── bangladesh-national-museum-ui-ux.png
    ├── incognito.png
    ├── archive-and-resist-conclave.gif
    ├── smol-moodboard.gif
    ├── social-media.png
    │
    ├── bloodborne-fan-art.png                # Illustration grid
    ├── illus_reanimal.png
    ├── illus_lovecraft.png
    ├── illus_re4.png
    │
    ├── heated-rivalry-thumb.gif              # Games cards
    ├── maya-thumb.gif
    ├── kitchen-keep-thumb.gif
    └── flip-n-frolic-thumb.gif
```

> **Note:** `index.html` references images by their full URL (`https://tanzeemsakib.github.io/assets/...`), so when you open the page locally the images come from the live site. A new image will not show up locally until it has been pushed. To work fully offline, change the paths to relative ones such as `assets/incognito.png`.

Inside `index.html` the code is laid out in this order:

1. `<head>`: metadata, canonical link, favicons, fonts, deferred CDN scripts, Person JSON-LD
2. `<style>`: all CSS (design tokens, layout, components, breakpoints)
3. `<body>`: early theme script, grid canvas, navigation island and menu, then the sections
4. Scripts at the end of the body: Lenis, theme toggle, scroll reveal, cursor label, Work row animation, WebGL bulge, menu timeline, grid canvas

---

## Page sections

| Section | id | What it contains |
| --- | --- | --- |
| Hero | `#hero` | Name, tagline, scroll cue |
| Work | `#work` | Six case-study rows with thumbnails linking to Figma or Behance |
| Illustration | `#illustration` | Four-image grid with the WebGL hover and a Behance link |
| Games | `#games` | Four game-jam cards linking to itch.io |
| About | `#about` | Bio, experience, awards, disciplines, tools, social links, resume |
| Contact | `#contact` | Headline and email link |
| Footer | `footer` | Availability note and copyright |

The navigation menu links to About, Behance, itch.io and Contact.

---

## Running it locally

There is nothing to install. Serve the folder with any static server so that the ES-module scripts behave exactly as they do online:

```bash
# from the repo root
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly from disk also works in most browsers, but a local server is the safer option.

An internet connection is needed for the fonts, the CDN libraries and (unless you switch to relative paths) the images.

---

## Deploying

This is a GitHub user site (`<username>.github.io`), so it is served straight from the repository.

1. Commit and push to the branch that GitHub Pages is configured to publish (Settings, then Pages).
2. Wait a minute or so for Pages to rebuild.
3. Hard-refresh the live site to bypass cached copies.

---

## Editing guide

### Add or change a Work item

Work items are `<li>` blocks inside `.work-list`. Copy an existing one and edit the title, description, tags, link and thumbnail:

```html
<li>
  <a class="work-row" href="https://example.com/case-study" target="_blank" rel="noopener">
    <div class="work-row-left">
      <span class="work-row-title">Project name <span class="arr">→</span></span>
      <p class="work-row-desc">One or two sentences about the project.</p>
      <span class="work-row-tags">UI/UX · Client Work</span>
    </div>
    <div class="work-thumb">
      <img src="https://tanzeemsakib.github.io/assets/your-image.png" alt="Short description"
           width="960" height="720" loading="lazy" decoding="async"/>
    </div>
  </a>
</li>
```

Use `&amp;` instead of a bare `&` in text and attributes.

### Add an illustration

Add another `.illus-cell` inside `.illus-grid`. The grid is four columns on desktop, so keep the count a multiple of four (or change `repeat(4, 1fr)`). Cells use a 4:5 ratio and the image is cropped to fit. The WebGL hover is attached automatically to every `.illus-cell`.

### Add a game

Copy a `.games-card` inside `.games-grid`. The card background is an image with the text overlaid, so use a darker or busy image that keeps white text readable. Cards are 3:4. The image uses `alt=""` because the card already shows the game name as text.

### Update experience, awards or tools

These live in the About section: `.exp-item` for each job, `.award-item` for awards, and the two `.about-col-items` lists for disciplines and tools. Logos go in `assets/` as SVG.

### Change the colors

The palette comes from a few CSS variables at the top of the stylesheet:

```css
:root     { --blue: #1a2fff; --blue-rule: #1a2fff; --bg: #ffffff; }
body.dark { --bg: #1a2fff;   --blue-rule: #ffffff; --blue: #ffffff; }
```

Night mode simply swaps the background and ink colors. The grid lines are hard-coded as `rgba(26,47,255,...)` in a few places (the grid canvas script, `.menu-grid`, `.menu-link` borders), so update those too if you change the blue.

### Tune the grid animation

The constants at the top of the **BACKGROUND** script control the feel:

| Constant | Default | Effect |
| --- | --- | --- |
| `PUSH` | `0.75` | How hard lines are shoved away. Keep it below 1 so lines never fold over themselves |
| `HOVER_SIGMA` | `55` | Width of the bend around the cursor, in px |
| `FLING` | `0.2` | How much fast mouse movement drags lines along. `0` turns it off |
| `SPRING` | `0.06` | How strongly lines return to rest |
| `FRICTION` | `0.7` | Lower settles faster, higher feels floatier |
| `STEP` | `8` | Sample spacing along each line. Smaller is smoother but costs more CPU |
| `SPEED`, `RING`, `AMP`, `LIFE` | `0.8`, `55`, `12`, `1400` | Click ripple travel speed, thickness, strength and duration |
| `GAP` | `50` | Grid cell size. Must match `background-size: 50px` in the CSS fallback grid |

### Change the menu

Menu links are the `.menu-link` anchors inside `#menu-overlay`. The expand and collapse animation is the GSAP timeline in the **MENU** script.

---

## How the interactions work

**Theme (Night / Day).** An inline script at the top of `<body>` applies the saved theme before first paint, so there is no flash. The choice is stored in `localStorage` under the key `theme` (`dark` or `light`). With no saved choice the site follows the operating-system setting and updates live if it changes. The browser `theme-color` meta tag switches with the theme.

**Line-grid background.** A fixed full-screen `<canvas>` draws the entire grid. Every point on every line is a small spring that chases a target set by the cursor, a smooth bump with no jump directly under the pointer, so lines bend as continuous curves. Lines near the cursor also brighten. A click starts a ring that travels outward and fades. The loop goes to sleep when nothing is moving. While the canvas is running it hides the static CSS grid (`.grid-canvas body::before`).

**Navigation island.** A small fixed bar with the Night/Day toggle and a menu button. Opening it expands the bar, rolls in the name with GSAP SplitText, morphs the burger into a close icon and fades in the menu panel. Everything runs on one reversible GSAP timeline.

**Scroll reveals.** Elements with `.reveal` fade up when they enter the viewport, using IntersectionObserver and a CSS transition. The Work rows use anime.js instead.

**Illustration bulge.** Each illustration cell gets a small WebGL canvas (OGL) that magnifies the image around the pointer. The canvases are created shortly before the section scrolls into view, and the effect only runs on devices with a fine, hover-capable pointer.

**Cursor labels.** Work rows and game cards show a label that follows the pointer. It is hidden on small screens.

---

## Fallbacks and accessibility

Resilience was a design goal: if something fails to load, the page should still read and work.

- **No JavaScript:** content stays visible. The reveal-hiding styles only apply once a `.js` class has been added to `<html>`.
- **CDN failures:** Lenis failing means native scrolling. GSAP or SplitText failing means the menu opens and closes without animation (`.no-gsap`). anime.js failing means the Work rows simply appear. An OGL or WebGL failure leaves the normal image with a CSS zoom on hover. A canvas failure leaves the static CSS grid.
- **Reduced motion** (`prefers-reduced-motion`): the grid animation and the WebGL effect are switched off, menu animation runs faster, the Work rows appear without animation and the game images skip their zoom.
- **Touch devices:** no hover effects. A tap still triggers the grid ripple.
- **Keyboard:** visible focus styles on interactive elements, `Escape` closes the menu, and focus is trapped inside the menu while it is open. Menu links are not focusable while the menu is closed.
- **Screen readers:** section titles are real `<h2>` headings, the menu has dialog and `aria-expanded` semantics, the theme button reports `aria-pressed`, and decorative images and canvases are hidden from assistive technology.

---

## SEO and link previews

- Title, description, Open Graph and Twitter Card tags, with `assets/og-preview.png` as the share image (1200 x 630 px works best)
- `canonical` link and `theme-color` meta tag
- **Person** structured data (JSON-LD) in the `<head>`: name, job title, location, employer and profile links
- Favicons in several sizes, an Apple touch icon and a web manifest

After changing the share image or title, link-preview caches on social platforms may take a while to refresh. Their debugger tools can force it.

---

## Responsive behavior

| Width | Layout |
| --- | --- |
| above 1000px | Four-column Illustration and Games grids, two-column About |
| 761px to 1000px | Games drop to two columns so the card text has room; Illustration stays at four |
| 760px and below | Everything is a single column, one item per scroll. The About section stacks, work thumbnails go full width, the cursor label is hidden |

---

## Performance notes

- Every image uses `loading="lazy"` and `decoding="async"`. The `width` and `height` attributes are aspect-ratio hints; the CSS reserves the space, so the layout does not jump while images load.
- Lenis, GSAP and SplitText load with `defer`, and the scripts that depend on them wait for `DOMContentLoaded`.
- The grid loop sleeps when idle, caps the device pixel ratio at 2, and the WebGL canvases are only built just before they are needed.
- **The biggest remaining cost is the GIFs** (six of them). Converting them to muted, looping `<video>` (MP4 plus WebM) or animated WebP usually cuts their size by 50 to 90 percent. Static PNGs can also be converted to WebP or AVIF at about 960px wide.

Example conversions:

```bash
# GIF to MP4
ffmpeg -i in.gif -movflags +faststart -pix_fmt yuv420p \
  -vf "scale='min(960,iw)':-2" -c:v libx264 -crf 26 -an out.mp4

# GIF to animated WebP
gif2webp -q 70 -m 6 in.gif -o out.webp
```

---

## Browser support

Current versions of Chrome, Edge, Firefox and Safari (desktop and mobile). The layout uses `100svh`, CSS `aspect-ratio`, `inset`, `backdrop-filter` and ES modules, so very old browsers are not supported, though the content remains readable there.

---

## Credits

- [Lenis](https://github.com/darkroomengineering/lenis) by Darkroom Engineering
- [GSAP](https://gsap.com) and SplitText by GreenSock
- [anime.js](https://animejs.com) by Julian Garnier
- [OGL](https://github.com/oframe/ogl) by Nathan Gordon
- Bulge hover effect adapted from the Codrops tutorial by Robin Payot
- Fonts by Google Fonts: DM Serif Display and DM Sans

---

## License and contact

© 2026 Tanzeem Ahmed. All rights reserved. The artwork, case studies and written content are the author's own work. If you want to open-source the code, add a `LICENSE` file to say so.

- Email: tasakib69@gmail.com
- Behance: https://www.behance.net/tanzeemsakib
- LinkedIn: https://www.linkedin.com/in/tanzeemsakib
- itch.io: https://tanzeem-ahmed-sakib.itch.io/
- Instagram: https://instagram.com/tanzeemsakib
