# Project: Course Slides

Educational course slides for advanced algorithms, hosted as a static HTML site on Netlify.

## Tech Stack

- **Presentation framework:** Reveal.js v4.5.0 (loaded via CDN)
- **Math rendering:** KaTeX v0.16.9 with auto-render (loaded via CDN)
- **Landing page template:** "Split Screen" by Pixelarity, with SCSS source in `assets/sass/`
- **Fonts:** Heliotrope (custom, `assets/fonts/`), EB Garamond and Overlock (Google Fonts)
- **No build system.** Pure static HTML/CSS/JS. Styles are inlined in each slide deck's `<style>` block. The SCSS pipeline only applies to the landing page (`assets/css/main.css`).
- **Hosting:** Netlify (see `netlify.toml`)

## Repository Structure

```
index.html                  Landing page
advalgo/
  greedy/index.html         Greedy algorithms slide deck (~3,000 lines)
  reductions/index.html     Reductions slide deck (~6,000 lines)
assets/
  css/main.css              Compiled landing page styles
  sass/                     SCSS source for landing page
  fonts/                    Heliotrope woff2 files
  js/main.js                Landing page JS (carousel, preload)
```

Each slide deck is a self-contained HTML file with all styles and scripts inline.

## Design Aesthetic

The visual style is **minimalist and academic**, inspired by gwern.net. Key principles:

- **White slides with a light gray (`#f5f5f5`) viewport background.** Slides have a `3px double #cccccc` border.
- **Left-aligned text.** Never centered body text. `text-align: left` on `.reveal .slides`.
- **Serif typography.** Body text in Heliotrope; headings in EB Garamond (weight 700); UI labels in Overlock. Monospace (Consolas/Monaco) for algorithm pseudocode.
- **Pastel accent palette** used for semantic color-coding, never for decoration:
  - `--pastel-blue: #a8d4f0` (definitions)
  - `--pastel-green: #b8e6c1` (theorems, success)
  - `--pastel-coral: #f5c4c0`
  - `--pastel-lavender: #d4c4e8` (algorithms)
  - `--pastel-gold: #f5e6a3` (warnings)
  - `--pastel-mint: #a8e6d5` (outcomes)
  - `--pastel-peach: #f5d4b8`
- **Dark text only.** Primary `#222222`, secondary `#666666`. No colored text. White text only on dark-background buttons and table headers.
- **No slide transitions.** Reveal.js `transition: 'none'`. Content appears instantly.
- **Generous whitespace.** 55px top padding, 35px side padding on slides. 2em element margins.

## Content Boxes

Semantic content is wrapped in colored-left-border boxes. Every new slide deck must use these consistently:

| Class             | Left border color                | Use for                        |
|-------------------|----------------------------------|--------------------------------|
| `.definition-box` | `--pastel-blue`                  | Definitions, problem statements|
| `.theorem-box`    | `--pastel-green`                 | Theorems, lemmas, proofs       |
| `.algorithm-box`  | `#9b8bb8` (darker lavender)      | Pseudocode (monospace, `#fafafa` bg) |
| `.success-box`    | `--pastel-mint`                  | Correct outcomes, results      |
| `.goal-box`       | Purple variant                   | Goals, objectives              |

## Interactive Elements

Interactivity is central to the pedagogical approach. Slides are not passive; they include hands-on demos where learners manipulate algorithm inputs directly. Key patterns:

### Demo Container Structure

Every interactive demo follows this DOM pattern:

```html
<div class="demo-container">
  <div style="display:flex; ...">
    <div class="demo-controls">
      <!-- number inputs, buttons -->
    </div>
  </div>
  <div class="demo-canvas" id="PREFIX-canvas">
    <!-- SVG or dynamically rendered visualization -->
  </div>
  <div class="demo-output" id="PREFIX-output">
    <!-- text feedback -->
  </div>
</div>
```

- **`demo-controls`**: Compact row of labeled `<input type="number">` fields (50px wide) and action buttons.
- **`demo-canvas`**: Visual area (min-height 280px) for SVG-based algorithm visualizations. Supports mouse-based interaction (click to select/delete, drag to move, drag edges to resize).
- **`demo-output`**: Text feedback area (`#fafafa` background) that shows results, hints, and success/error messages.

### Interactive Patterns Used

1. **Counterexample builders** -- Learner constructs an input set that breaks a wrong greedy strategy. The algorithm runs and reveals whether the strategy fails.
2. **Warm-up arenas** -- Learner solves a generated problem instance by clicking to select items, then checks their solution against the optimal.
3. **Graph/matroid arenas** -- Canvas-based interactive graph editors for constructing matroids and exploring properties.
4. **Step-through visualizations** -- Algorithms run step-by-step with visual highlights.
5. **Approach cards** -- Clickable cards (`<a class="approach-card">`) that link to specific slide columns. Correct answers get a `.correct` class (green border, light green background).

### Buttons

```
.btn            -- default (white, bordered)
.btn-primary    -- dark background (#222222), white text
.btn-success    -- pastel-green background
.btn-warning    -- pastel-gold background
.btn-secondary  -- light gray (#eeeeee)
```

All buttons use `transition: background 0.2s`.

### Feedback Messages

```
.feedback-success  -- #f0fff0 background (light green)
.feedback-error    -- #fff5f5 background (light red)
.feedback-info     -- #f5faff background (light blue)
```

## Reveal.js Configuration

All slide decks use this configuration:

```javascript
Reveal.initialize({
    hash: true,
    slideNumber: 'c/t',
    progress: true,
    center: false,
    transition: 'none',
    width: 1100,
    height: 700,
    margin: 0.04
});
```

- **Horizontal sections** = top-level topics (e.g., "Problem", "Wrong Approaches", "Correct Approach", "Matroids")
- **Vertical slides within sections** = subtopics and progressive content
- **Fragments** (`.fragment` with optional `data-fragment-index`) for progressive reveal within a slide
- **Navigation buttons** at slide bottom (`<div class="bottom-nav">`) with `← Back to ...` links

## Math Typesetting

- Inline math: `\( ... \)` delimiters
- Display math: `$$ ... $$` delimiters
- KaTeX auto-render processes the entire document after DOMContentLoaded
- Common notation: set theory, function definitions, inequalities, complexity classes

## When Creating New Slide Decks

1. Copy the `<head>` block (font faces, Reveal.js CDN, KaTeX CDN) and the full `<style>` block from an existing deck. Do not create a separate CSS file for slides.
2. Keep all JavaScript inline at the bottom of the HTML file.
3. Use the established content box classes for semantic structure.
4. Prefer interactive demos over static diagrams. If a concept can be explored hands-on, build a demo-container for it.
5. Use SVG for visualizations (not canvas or images). SVGs are generated and manipulated via inline JavaScript.
6. Keep the Reveal.js config identical across decks.
7. Slide dimensions are fixed at 1100x700. Design content to fit without scrolling.
