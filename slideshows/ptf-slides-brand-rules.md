# Skillcrush by PTF — Slide Design Rules
_Companion to `ptf-slides-brand.json`. Read both files before building any deck._

---

## Background Color Hierarchy

- **White (`#FFFFFF`) is the default background** for the majority of slides in any deck.
- **Off-white (`#FAFAF6`)** can be used as a subtle alternating background for variety — never two consecutive off-white slides.
- **Black (`#0A0A0A`) backgrounds are used sparingly** — typically one per deck for emphasis (e.g. a closing slide, a bold statement slide, or a section divider). When a black background is used, all text becomes white and buttons switch to the on-dark variant (white background, black text).
- Never use purple or green as a full-slide background.

---

## Color Role Hierarchy

- **Greens are the primary brand expression.** Default to green (`#0A8C66` or `#4FE8A9`) for accents, highlights, headline underlines, iconography, and decorative elements.
- **Purple (`#5B4FCF`) is used occasionally** — no more than 1–2 slides per deck — typically for a feature callout, a section intro, or a button variant that needs contrast from green.
- **Iridescent accents (cyan, green, lime)** are used for decorative flourishes, gradient accents, or background texture — not for large fills or primary UI elements.
- Never use purple and green as competing accents on the same slide. Pick one per slide.

---

## Typography Hierarchy

- All type is set in **Arimo** (Google Fonts). The font name must be written exactly as `"Arimo"` in PPTX generation.
- Use **weight progression** to establish hierarchy: display/headline 700–800, subheadings 600, body 400–500, captions/labels 400.
- Headline text on white/off-white backgrounds defaults to ink primary (`#0A0A0A`).
- Headline text on black backgrounds is white (`#FFFFFF`).
- Body copy uses ink secondary (`#2A2A2A`) on light backgrounds.

---

## Logo

- The Skillcrush by PTF logo appears in the **top-right corner of every slide** without exception, including title slides, divider slides, and black-background slides.
- Maintain the **8:3 aspect ratio** at all times. Never stretch or crop.

---

## Slide Structure Norms

- **Title slide**: Usually white or off-white background, large display headline, logo top-right, optional iridescent accent shape.
- **Section dividers**: Can use black background for emphasis. One per major section.
- **Content slides**: White background, green accents, consistent logo placement.
- **Closing slide**: Often black background or a bold green treatment with a CTA button.

---

## Spacing & Density

- Prefer generous whitespace over dense layouts. If a slide feels crowded, split it into two.
- Card elements use the defined shadow token and `md` or `lg` border radius.
- Consistent internal padding on cards and content blocks: at minimum 16px equivalent.

---

## What NOT to do

- Do not use purple and green together on the same slide as competing elements.
- Do not put slide numbers on slides. Slide counters (e.g. "3 / 28") are never displayed on the slide canvas.
- Do not use black backgrounds on more than ~2 slides in a standard deck.
- Do not use iridescent colors as primary text or button fills.
- Do not omit the logo on any slide.
- Do not use font names other than exactly `"Arimo"` — no fallbacks written into the file.
