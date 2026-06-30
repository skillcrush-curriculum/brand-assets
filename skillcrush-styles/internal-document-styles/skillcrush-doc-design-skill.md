# Skillcrush Brand Skill

Use this skill whenever creating or editing a **Skillcrush-branded document** — Word docs (.docx), lesson outlines, teaching references, or any formatted deliverable. It defines typography, color, table structure, dividers, callout boxes, document structure rules, and header/footer behavior. Always load `skillcrush-doc-design-tokens.json` alongside this file for exact values.

---

## Font Families

| Role     | Font       | Availability  |
|----------|------------|---------------|
| Headings | Montserrat | Google Fonts  |
| Body     | Open Sans  | Google Fonts  |

In `docx-js`, set `font: 'Montserrat'` or `font: 'Open Sans'` on each `TextRun`.

---

## Typography Scale

All `size_pt` values are document-safe point sizes confirmed for readable rendering in .docx and Google Docs.

| Level | Font       | size_pt | size (halfpt) | Weight  | Color              | Use                              |
|-------|------------|---------|---------------|---------|--------------------|---------------------------------|
| H1    | Montserrat | 19pt    | 38            | Bold    | `#f86d58` (coral)  | Document title only              |
| H2    | Montserrat | 16pt    | 32            | Bold    | `#743ffd` (purple) | Primary sections + step labels   |
| H3    | Montserrat | 12pt    | 24            | Bold    | `#666666` (grey)   | Sub-sections / step titles       |
| H4    | Montserrat | 10pt    | 20            | Bold    | `#666666` (grey)   | Smallest header level            |
| Body  | Open Sans  | 10pt    | 20            | Regular | `#333333`          | All body copy                    |
| H/F   | Open Sans  | 9pt     | 18            | Regular | `#666666`          | Page header and footer text      |

In `docx-js`, font sizes are in **half-points**: `size_pt * 2`. So 19pt = `size: 38`, 10pt = `size: 20`. Never use `size_pt * 20` (that is the DXA conversion for margins, not fonts).

### Heading Spacing (DXA)

| Level | before | after |
|-------|--------|-------|
| H1    | 0      | 160   |
| H2    | 280    | 100   |
| H3    | 200    | 80    |
| H4    | 160    | 60    |

**Exception — Step labels:** H2 used as step labels (READ STEP, CHALLENGE STEP, etc.) get `before: 320, after: 40` to create extra visual breathing room above a new step.

---

## Color Palette

| Token            | Hex        | Use                                                  |
|------------------|------------|------------------------------------------------------|
| `coral`          | `#f86d58`  | H1 text                                              |
| `purple`         | `#743ffd`  | H2 text                                              |
| `teal`           | `#23a898`  | Data table header backgrounds                        |
| `grey_text`      | `#666666`  | H3, H4 text; header/footer text                      |
| `body_text`      | `#333333`  | Body copy, table body rows, callout body text        |
| `table_alt_row`  | `#eefaf8`  | Alternating row tint in all tables                   |
| `callout_bg`     | `#f0f9fe`  | Callout box background                               |
| `callout_border` | `#000000`  | Callout box border                                   |
| `divider`        | `#cccccc`  | Section horizontal rules                             |
| `white`          | `#ffffff`  | Table cell backgrounds (non-alternating rows)        |

---

## Document Structure

### Heading hierarchy
- **One H1 per document.** H1 is the document title only — never used for body section headings.
- All subsequent headings use H2 → H3 → H4 in order. Do not skip levels.
- Use `heading: HeadingLevel.HEADING_1` through `HeadingLevel.HEADING_4` in docx-js — not styled `TextRun` paragraphs. This is required for Word and Google Docs to generate a real document outline.
- Register each heading style with the correct `outlineLevel` in `paragraphStyles`:
  - H1 → `outlineLevel: 0`
  - H2 → `outlineLevel: 1`
  - H3 → `outlineLevel: 2`
  - H4 → `outlineLevel: 3`

### Document title
- The document title is the only H1. It appears at the top of the first page.
- The document type (e.g. "Lesson Outline") belongs in the **page header only** — not in the body.

---

## Header and Footer

### Header
- Content: document type label only (e.g. `Lesson Outline`, `Teaching Reference`).
- Font: Open Sans, 9pt, `#666666`, left-aligned.
- **No Skillcrush wordmark.** No decorative border or rule.

### Footer
- Content: page number only, right-aligned (e.g. `Page 1`).
- Font: Open Sans, 9pt, `#666666`.
- **No decorative border or rule.**

---

## Section Dividers

Separate major sections with a paragraph bottom border:

- Color: `#cccccc`, `sz: 4` (0.5pt)
- In docx-js: apply a bottom border to an empty paragraph:
  ```javascript
  border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: 'CCCCCC', space: 1 } }
  ```
- Spacing: `before: 120, after: 120`
- **Never use a table as a divider.**

---

## Tables

Two table patterns are used. Both share the same cell margins, body text style, black borders, and alternating row fill (`#eefaf8`).

### Metadata table
Used for lesson/document details at the top of a document (Placement, Format, Deliverables, etc.).

- **No header row.** Rows alternate white → `#eefaf8` → white → `#eefaf8`.
- Body text: Open Sans, 10pt, `#333333`.
- Borders: `#000000`, `sz: 4`, `single`.

### Data table
Used for reference or content tables (coding examples, curriculum tables, step-type tables, etc.).

- **Teal header row:** Open Sans, 9pt, Bold, background `#23a898`, text `#ffffff`.
- Body rows alternate white → `#eefaf8`.
- Borders: `#000000`, `sz: 4`, `single`.

### docx-js implementation notes
- Always set `width: { size: N, type: WidthType.DXA }` on both the table and each cell.
- Use `ShadingType.CLEAR` (never `SOLID`) for cell backgrounds.
- Cell margins: `{ top: 80, bottom: 80, left: 140, right: 140 }`.
- Table width for US Letter with 1" margins = 9360 DXA. Column widths must sum exactly to the table width.
- Borders: `{ style: BorderStyle.SINGLE, size: 4, color: '000000' }` on all sides including `insideH` and `insideV`.

---

## Callout / Note Box

Used for tips, notes, warnings, or highlighted information.

| Property      | Value                  |
|---------------|------------------------|
| Background    | `#f0f9fe`              |
| Border color  | `#000000`              |
| Border sz     | 4                      |
| Label         | Montserrat, Bold, inline at start of paragraph |
| Label color   | `#000000`              |
| Body font     | Open Sans, 10pt, Regular, `#333333` |
| Padding (DXA) | 160 on all sides       |

**Key pattern:** The label (`NOTE:`, `TIP:`, etc.) and the body text are **in the same paragraph** — the label is a bold Montserrat `TextRun`, followed immediately by a regular Open Sans `TextRun` with the body copy. There is no separate label paragraph.

**Common labels:** `NOTE:`, `TIP:`, `IMPORTANT:`, `REMINDER:`

**docx-js implementation:** Single-cell table with `shading: { fill: 'F0F9FE', type: ShadingType.CLEAR }` and all borders set to `#000000`. Single paragraph in the cell with two `TextRun` children: bold label + regular body.

---

## Lesson Outline Document — Step Label Pattern

Lesson outlines use a consistent two-level pattern for each step:

1. **Step label** (H2, ALL CAPS) — identifies the step type. Uses `spacing before: 320, after: 40` to separate it clearly from the preceding section.
   - Examples: `READ STEP 1`, `READ STEP 2`, `DOWNLOAD STEP`, `CHALLENGE STEP 1`, `CHALLENGE STEP 2`
2. **Step title** (H3) — the readable name of the step, immediately following the label.
   - Examples: `What Makes a Good Prompt?`, `Prompt Writing Cheat Sheet`
3. **Step summary** (body paragraph) — one or two sentences describing what the step covers.
4. **Sub-sections** (H4) — used for the internal structure of a step (cover sections, specific topics).

**All step labels use H2**, including Challenge steps. Do not use H3 for step labels.

---

## Page Setup (docx-js)

```javascript
sections: [{
  properties: {
    page: {
      size: { width: 12240, height: 15840 }, // US Letter
      margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } // 1 inch = 1440 DXA
    }
  }
}]
```

---

## Implementation Checklist

Before outputting any Skillcrush document, verify:

- [ ] All headings use **Montserrat**; all body text uses **Open Sans**
- [ ] H1 is coral `#f86d58` at 19pt (sz: 38) — one per document, title only
- [ ] H2 is purple `#743ffd` at 16pt (sz: 32); H3 is grey `#666666` at 12pt (sz: 24); H4 is grey `#666666` at 10pt (sz: 20)
- [ ] Body text is Open Sans 10pt (sz: 20) `#333333`
- [ ] Font sizes use `size_pt * 2` in docx-js (half-points), never `size_pt * 20`
- [ ] All headings use `heading: HeadingLevel.HEADING_N` for real document outline
- [ ] `paragraphStyles` includes `outlineLevel` 0–3 for H1–H4
- [ ] Page header: document type label only, Open Sans 9pt `#666666`, no wordmark, no border
- [ ] Page footer: page number only, right-aligned, Open Sans 9pt `#666666`, no border
- [ ] Major sections separated by `#cccccc` paragraph bottom border
- [ ] Metadata tables (lesson details): no header row, alternating white/`#eefaf8`, black borders
- [ ] Data tables: teal `#23a898` header row with white bold text, alternating white/`#eefaf8` body, black borders
- [ ] Table + each cell have explicit DXA widths; column widths sum to table width (9360)
- [ ] Callout boxes: `#f0f9fe` background, black border, inline bold label + body in single paragraph
- [ ] Lesson outlines: step labels (READ STEP, CHALLENGE STEP, etc.) use H2 with `before: 320, after: 40`

---

## Related Files

- `skillcrush-doc-design-tokens.json` — machine-readable design tokens (single source of truth for all values)
