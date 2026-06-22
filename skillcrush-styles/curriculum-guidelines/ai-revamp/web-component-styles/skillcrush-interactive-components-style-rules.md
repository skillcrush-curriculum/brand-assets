---
name: skillcrush-interactive-components-style-rules
description: Use this skill whenever building interactive web components for the Skillcrush web development bootcamp. Triggers include: any request for a lesson component, interactive exercise, step-by-step UI, quiz, checklist, or any web component that will be embedded in the learning dashboard iFrame. Provides exact colors, fonts, layout patterns, and component recipes for bootcamp-compliant interactive lessons.
---

# Skillcrush Interactive Web Components — Design System

Reference for all bootcamp lesson components. These components render inside an **800px-wide iFrame** embedded in the existing learning dashboard. The dashboard already provides navigation chrome — do not add top-level headings, outer navigation, or curriculum menus to components.

---

## Color Palette

### Backgrounds
| Name | Value | Usage |
|------|-------|-------|
| **White** | `#ffffff` | Default component background |
| **Hero / Headline Wash** | `#effafa` | Top hero area behind the main lesson headline |
| **Active / Selected Step** | `rgb(254 242 241)` | Background of the currently active step card or row |
| **Tips Box** | `rgb(240 249 254)` | Background of tip/info callout boxes |

### Interactive States
| Name | Value | Usage |
|------|-------|-------|
| **Primary Button** | `rgba(241, 96, 89)` | Default CTA and action buttons |
| **Button Hover / Disabled** | `rgb(250 166 158)` | Hover state AND deactivated/disabled button state |
| **Completed Indicator Fill** | `rgb(48 192 161)` | Circle background behind white checkmark for completed steps |

### Text & Icons
| Name | Value | Usage |
|------|-------|-------|
| **All Text** | `#000000` | Body copy, labels, headings — always black |
| **Link Color** | `rgba(241, 96, 89)` | Inline links — underlined in red (primary button color) |

---

## Typography

### Font Families
| Font | Source | Usage |
|------|--------|-------|
| **Montserrat** | Google Fonts | Headlines, buttons, feature/callout text |
| **Open Sans** | Google Fonts | All body copy, descriptions, step content |

### Rules
- **Buttons and feature text: ALL CAPS** (CSS: `text-transform: uppercase`)
- **Button text: bold** (`font-weight: 700`)
- **Links: underlined in red** — `text-decoration: underline; color: rgba(241,96,89)`
- No italic, no decorative weights unless specifically requested

### Recommended Scale (px)
| Element | Font | Size | Weight |
|---------|------|------|--------|
| Hero headline | Montserrat | 24–28px | 700 |
| Section label / feature text | Montserrat | 13–14px | 700, ALL CAPS |
| Button label | Montserrat | 13px | 700, ALL CAPS |
| Body / step content | Open Sans | 15–16px | 400 |
| Tips / callout text | Open Sans | 14px | 400 |
| Step counter / meta | Open Sans | 13px | 400 |

---

## Layout & Spacing

### Viewport
- **Target width: 800px** — components are embedded in an iFrame at this width
- Use `max-width: 800px; margin: 0 auto` to center in wider containers
- Do **not** rely on viewport units (`vw`, `vh`) for core layout

### Core Rules
- **No border radius** — all corners are sharp (square)
- **Generous white space** — padding inside cards: `24px`; section gaps: `32–40px`
- **No outer component heading** — the lesson title lives in the dashboard, not the component
- **No animations or CSS transitions** — all state changes are instant, triggered by user button clicks
- **No delayed states** — never use `setTimeout` or CSS `transition` to reveal content

### Grid / Structure
```
[Hero band — #effafa, full width]
  Lesson headline (Montserrat 700, 24–28px)
  Optional subtext (Open Sans, 15px)

[Content area — white background]
  Step list or main interactive area
  Tips / callout boxes (inline, not modal)

[Navigation bar — white, bottom of component]
  < PREV STEP        NEXT STEP >
```

---

## Component Patterns

### Step Bullets / Progress Indicators

Use **solid-color icons** (not outlines) as step bullets — emoji or simple SVG icons work well.

| State | Visual |
|-------|--------|
| **Not started** | Numbered circle (outline or neutral fill) |
| **Active / current** | Row/card background: `rgb(254 242 241)`; icon highlighted |
| **Completed** | `rgb(48 192 161)` filled circle + white checkmark `✓` |

```css
.step.active   { background: rgb(254 242 241); }
.step-icon.done {
  background: rgb(48 192 161);
  color: #fff;
  border-radius: 50%; /* circles only on step indicators */
}
```

### Buttons

```css
/* Primary */
.btn-primary {
  background: rgba(241, 96, 89, 1);
  color: #ffffff;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 13px;
  text-transform: uppercase;
  border: none;
  padding: 12px 24px;
  cursor: pointer;
  border-radius: 0; /* sharp corners always */
}

/* Hover + Disabled */
.btn-primary:hover,
.btn-primary:disabled {
  background: rgb(250, 166, 158);
  cursor: default;
}
```

### Prev / Next Navigation

Always formatted as text-style buttons at the bottom of the component:

```
< PREV STEP                    NEXT STEP >
```

- Left-aligned `< PREV STEP`, right-aligned `NEXT STEP >`
- Font: Montserrat, bold, ALL CAPS, primary red color
- No button box/border — plain text with arrow characters
- Disable (mute to `rgb(250 166 158)`) when at first or last step

```html
<nav class="step-nav">
  <button class="nav-btn prev" onclick="prevStep()">&#8249; PREV STEP</button>
  <button class="nav-btn next" onclick="nextStep()">NEXT STEP &#8250;</button>
</nav>
```

```css
.step-nav {
  display: flex;
  justify-content: space-between;
  padding: 24px 0 8px;
  border-top: 1px solid #e5e7eb;
  margin-top: 32px;
}
.nav-btn {
  background: none;
  border: none;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 13px;
  text-transform: uppercase;
  color: rgba(241, 96, 89, 1);
  cursor: pointer;
  padding: 0;
}
.nav-btn:disabled { color: rgb(250, 166, 158); cursor: default; }
```

### Tips / Info Callout Box

```
┌─────────────────────────────────────────┐  ← 1px border, #e5e7eb or tip-blue tint
│  ⓘ  TIP LABEL (Montserrat, caps)       │
│     Tip body text in Open Sans.         │
└─────────────────────────────────────────┘
```

- Background: `rgb(240 249 254)`
- Border: `1px solid #bde3f7` (or similar light blue)
- Icon: white `i` letter on a solid circular background (`rgb(48 192 161)` or the primary red)
- No border radius on the box itself
- Label in Montserrat ALL CAPS; body in Open Sans

```html
<div class="tip-box">
  <span class="tip-icon">i</span>
  <div>
    <strong class="tip-label">Pro Tip</strong>
    <p>Your tip text here.</p>
  </div>
</div>
```

```css
.tip-box {
  background: rgb(240, 249, 254);
  border: 1px solid #bde3f7;
  padding: 16px 20px;
  display: flex;
  gap: 14px;
  align-items: flex-start;
  border-radius: 0;
}
.tip-icon {
  background: rgb(48, 192, 161);
  color: #fff;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 13px;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.tip-label {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 12px;
  text-transform: uppercase;
  display: block;
  margin-bottom: 4px;
}
```

---

## Google Fonts Import

Always include this at the top of the `<head>` or in the CSS:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=Open+Sans:wght@400;600&display=swap" rel="stylesheet">
```

---

## iFrame / Embedding Notes

- Component is a **standalone HTML file** — no external JS framework required
- All state managed with plain JS variables + direct DOM manipulation
- `document.getElementById` / `classList.add` preferred over framework reactivity
- No `localStorage` or `sessionStorage` — state resets per session (expected behavior)
- Component should look correct at exactly **800px wide** with no horizontal scroll

---

## What NOT to Do

| ❌ Don't | ✅ Do instead |
|---------|--------------|
| Add a component title/heading at top | Start with hero band or directly with content |
| Use border-radius on cards/buttons | Keep all corners sharp (`border-radius: 0`) |
| Animate step transitions | Instant show/hide with JS `display` toggle |
| Add a sidebar or left nav | Dashboard provides all navigation chrome |
| Use `vh`/`vw` for layout | Use `px` or `%` within the 800px container |
| Auto-advance steps on timer | Require explicit button click to proceed |
