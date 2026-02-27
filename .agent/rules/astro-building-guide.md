---
trigger: always_on
---

# 🧠 Antigravity Rules — Personal Portfolio
> Stack: **Astro** + **TailwindCSS** | Theme: **Minimalist Black & White** | Layout: **Single Page + Modal Popups**

---

## 📌 PROJECT OVERVIEW

You are helping me build a personal portfolio website with the following non-negotiable characteristics:

- **Framework:** Astro (latest stable)
- **Styling:** TailwindCSS v3+ — utility-first, no custom CSS unless Tailwind cannot handle it
- **Design Theme:** Strict minimalist — black and white only, clean typography, HR-optimized readability
- **Layout:** Single page, no body scrolling — everything fits in the viewport
- **Navigation:** Structured button grid on the main page; each button opens a modal popup with detailed sub-information
- **No external UI libraries** — all components built from scratch with Tailwind
- **No page routing** — one URL, one page, one viewport

---

## 🎨 DESIGN SYSTEM

### Color Palette
- Background: `#FFFFFF` / `#F9F9F9`
- Text: `#0A0A0A`
- Borders / Dividers: `#E0E0E0`
- Button: black bg + white text; hover → white bg + black text + black border
- Modal: white bg, `rgba(0,0,0,0.6)` overlay, `shadow-md` or `border border-gray-200`

> ⚠️ Never introduce colors outside this palette. No gradients, no accent colors.

### Typography
- Font: **Inter** (import via Google Fonts in `<head>`) — fallback `system-ui, sans-serif`
- Hero name: `text-4xl font-bold tracking-tight`
- Modal section titles: `text-xl font-semibold`
- Body: `text-base font-normal leading-relaxed text-gray-800`
- Labels/meta: `text-sm font-medium text-gray-500`
- Max 4 font sizes visible at any one time

### Spacing, Borders & Shadows
- Use Tailwind spacing scale consistently (`p-4`, `p-6`, `p-8`, `gap-4`, `gap-6`)
- Borders: `border border-gray-200` — use sparingly
- Shadows: `shadow-sm` or `shadow-md` only — nothing dramatic
- Border radius: `rounded-lg` across all components — consistent
- Button min size: `py-4 px-6`, `min-h-[48px]`

---

## 🏗️ FILE STRUCTURE

```
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── HeroSection.astro
│   │   ├── NavButton.astro
│   │   ├── Modal.astro
│   │   └── modals/
│   │       ├── ExperienceModal.astro
│   │       ├── ProjectsModal.astro
│   │       ├── SkillsModal.astro
│   │       ├── CertificatesModal.astro
│   │       ├── EducationModal.astro
│   │       └── ContactModal.astro
│   ├── data/
│   │   ├── experience.js
│   │   ├── projects.js
│   │   ├── skills.js
│   │   ├── certificates.js
│   │   └── profile.js
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   └── index.astro
│   └── styles/
│       └── global.css
├── astro.config.mjs
└── tailwind.config.cjs
```

**Rules:**
- All content data lives in `/src/data/` — never hardcode content inside component markup
- Components are `.astro` files; use vanilla JS `<script>` blocks for interactivity — no React/Vue/Svelte
- One component = one responsibility; keep files small and focused

---

## 🪟 MODAL SYSTEM

### Behavior
- All modals hidden by default (`hidden` class)
- Opens on button click; closes on: overlay click, `×` button click, or `Escape` key
- Only one modal open at a time — opening a new one closes any existing open modal
- Smooth transition: `transition-opacity duration-200`
- Centered via `fixed inset-0 flex items-center justify-center`
- Internal scroll for overflow: `overflow-y-auto max-h-[80vh]` on modal inner container

### HTML Structure Pattern
```html
<!-- Overlay -->
<div id="modal-overlay"
  class="fixed inset-0 bg-black/60 z-40 hidden"
  aria-hidden="true">
</div>

<!-- Modal -->
<div id="modal-{name}" role="dialog" aria-modal="true" aria-labelledby="modal-{name}-title"
  class="fixed inset-0 z-50 flex items-center justify-center hidden">
  <div class="bg-white rounded-lg shadow-md w-full max-w-2xl max-h-[80vh] overflow-y-auto p-8 relative">
    <button class="absolute top-4 right-4 text-gray-400 hover:text-black transition-colors">✕</button>
    <h2 id="modal-{name}-title" class="text-xl font-semibold mb-6">Section Title</h2>
    <!-- Content -->
  </div>
</div>
```

### JS Logic (inside `index.astro` `<script>`)
- Centralized `openModal(id)` and `closeModal()` functions
- Attach listeners to all nav buttons and close triggers
- `document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal() })`
- On open: move focus to modal container; on close: return focus to the triggering button
- No jQuery or external JS libraries

---

## 🏠 MAIN PAGE LAYOUT

### Hero Section
- Occupies max **35% of viewport height**
- Must contain: **Full Name** (large, bold), **Professional Title**, optional one-line tagline
- Optional: GitHub + LinkedIn as small text links (not buttons)
- No animations, no typing effects

### Button Grid
- Layout: `grid grid-cols-2 md:grid-cols-3 gap-4`
- Each button: filled black bg + white text; hover → invert with black border
- Style: `py-4 px-6 text-sm font-medium tracking-widest uppercase rounded-lg transition-all duration-200`
- Each button has a short subtitle below the label in `text-xs text-gray-400` (e.g., "3 roles · 4 years")
- `cursor-pointer` always set on interactive elements

### Viewport Constraint
- Root container: `h-screen overflow-hidden`, distributed via `flex flex-col`
- Must fit on `1366×768` laptop without scrolling
- Adjust font sizes responsively using Tailwind `sm:` / `md:` prefixes as needed

---

## 📋 SECTION CONTENT RULES

| Section | Key Fields |
|---|---|
| **Experience** | Company, Role, Duration, 2–3 line description, optional tech tags |
| **Projects** | Name, 1–2 line description, tech tags, optional GitHub/Live links |
| **Skills** | Grouped by category (Languages, Frameworks, Tools, Soft Skills) — tags/pills only |
| **Certificates** | Name, Issuer, Date, optional Verify link |
| **Education** | Institution, Degree & Major, Year range, optional GPA |
| **Contact** | Email, LinkedIn, GitHub — as labeled links, no contact form |

**Shared rules:**
- Sort experience and projects reverse-chronologically (most recent first)
- Tech tags: `text-xs px-2 py-0.5 border border-gray-300 rounded-full`
- **No skill progress bars or percentages** — subjective and unprofessional
- All external links: `target="_blank" rel="noopener noreferrer"`
- Links use inline SVG or Unicode for icons — no icon libraries

---

## ⚙️ CONFIGURATION

### astro.config.mjs
```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
  output: 'static',
});
```

### tailwind.config.cjs
```js
module.exports = {
  content: ['./src/**/*.{astro,html,js,ts}'],
  theme: {
    extend: {
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
    },
  },
  plugins: [],
};
```

### BaseLayout.astro Requirements
- `<html lang="id">` (adjust as needed)
- `<body class="bg-white text-black antialiased overflow-hidden">`
- Meta tags: `charset`, `viewport`, `description`, `og:title`, `og:description`
- Inter font import from Google Fonts in `<head>`

---

## 🔒 CODE QUALITY

- Indentation: 2 spaces, consistent throughout
- Use `const` over `let`; never `var`
- Max HTML nesting: 3 levels deep
- No `!important`, no CSS `var()` unless Tailwind cannot cover it, no `@apply` unless building a shared component style used 3+ times
- Avoid arbitrary Tailwind values (e.g., `w-[347px]`) — use the standard scale; comment if unavoidable
- Add brief comments above major component blocks explaining purpose
- Descriptive variable/function names — no `x`, `temp`, `data1`

### Accessibility
- All interactive elements keyboard-accessible
- Semantic HTML: `<header>`, `<main>`, `<section>`, `<button>`, `<a>` used appropriately
- All images: descriptive `alt` attributes
- WCAG AA contrast maintained (black on white passes by default)

---

## 🚫 NEVER DO

- ❌ Add scrolling to the main page body
- ❌ Use colors outside the defined palette (no blues, purples, gradients)
- ❌ Use external UI libraries (DaisyUI, Shadcn, Flowbite, MUI, etc.)
- ❌ Use icon libraries (FontAwesome, Lucide, Heroicons) — inline SVG or Unicode only
- ❌ Add skill progress bars or percentage indicators
- ❌ Add dark mode, cursor effects, parallax, particle backgrounds, or typing animations
- ❌ Use `Lorem ipsum` — ask me for real content if needed
- ❌ Install packages without explaining what they do and why they're needed
- ❌ Hardcode content inside component markup — all data goes in `/src/data/`

## ✅ ALWAYS DO

- ✅ Ask before assuming anything about my personal data
- ✅ Show the **file path** above every code block
- ✅ Show **all affected files** in one response when a task touches multiple files
- ✅ Use `transition-all duration-200 ease-in-out` for all hover/focus states
- ✅ Use `rel="noopener noreferrer"` on all `target="_blank"` links
- ✅ Verify layout fits `1366×768` before presenting a solution
- ✅ Separate data from markup at all times

---

## 🗂️ MODAL SECTIONS REFERENCE

| Button Label | Opens |
|---|---|
| Experience | Work history modal |
| Projects | Projects portfolio modal |
| Skills | Skills & tools modal |
| Certificates | Certifications modal |
| Education | Academic background modal |
| Contact | Contact links modal |

Ask before adding or removing sections.

---

## 📐 DEVELOPMENT ORDER

1. `BaseLayout.astro` + `index.astro` shell + modal JS system
2. Data files in `/src/data/` with placeholder structure (not real content)
3. Hero section + button grid layout
4. Modals one at a time — fully styled before moving to the next
5. Responsive pass — verify mobile at every step

---

## 💬 COMMUNICATION PROTOCOL

- **"build the modal"** → generate the full component file, not a snippet
- **"update the design"** → modify styling only, not data or structure
- **"refactor"** → improve code quality, no visual changes
- **"add section X"** → create the button AND the modal together
- If my instruction is ambiguous → ask **one clarifying question** before proceeding

*These rules apply for the entire duration of this project. Do not deviate unless I explicitly override a rule.*