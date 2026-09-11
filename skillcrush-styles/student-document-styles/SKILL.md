---
name: skillcrush-doc-design
description: "Design system for Skillcrush by PowerToFly student-facing Word documents — student agreements, scholarship letters, program handbooks, syllabi, enrollment and policy docs. Use this skill whenever creating or restyling a .docx for Skillcrush that is intended so be student-facing, a Skillcrush/PowerToFly program or scholarship, or when the user asks for a document that should match the Student Agreement, the Break Into Tech docs, or 'our program doc template.' Also use it when the user asks for exact heading sizes, brand colors, the purple header band, callout boxes, or table fills for these documents. This is the DOCUMENT system — for slide decks use the powertofly-brand skill instead."
---

# Skillcrush by PowerToFly — Document Design System

Two things drive the whole look: a saturated purple header band at the top of page 1, and purple Montserrat headings over Open Sans body copy. Restraint is the point — purple is used for structure and emphasis only, never for body text.

Call out boxes pop out with the mint color

> This is a **different palette from the PTF deck system** (`powertofly-brand`, which uses PTF Purple `743FFD` and PTF Mint `03FFB2`). Don't mix them. Skillcrush documents use the purple below.

---

## Color palette

| Name | Hex | Usage |
|---|---|---|
| **Skillcrush Purple** | `4933D1` | Header band fill, all H2/H3 headings, section rules, callout headings, hyperlinks, table header fills |
| **White** | `FFFFFF` | Page background; all type inside the purple band |
| **Body Ink** | `1A1A1A` | Default text color (set in `docDefaults`) |
| **Muted Grey** | `5A5A5A` | Signature-line captions, small labels, captions |
| **Hairline Grey** | `999999` | Signature rules |
| **Mint Wash** | `E5FFF6` | Callout box fill — action items, deadlines, good news |
| **Purple Wash** | `EDEAFA` | 10% purple tint —  table zebra emphasis |
| **Purple Whisper** | `F8F7FD` | 4% purple tint — alternating table rows |
| **Table Rule** | `D2CCF3` | 25% purple tint — table gridlines |

Word takes hex without a `#`, lowercase or uppercase (the source file uses lowercase, e.g. `w:fill="4933d1"`).

---

## Typography

Two families, both **embedded in the source .docx** (`word/fonts/*.ttf`, wired through `fontTable.xml.rels`). Embed them in anything new too — otherwise the doc silently falls back to Calibri on machines without Montserrat, and the whole design collapses.

- **Montserrat** — headings, header band, callout headings. Bold only.
- **Open Sans** — body, lists, micro-headings, captions. Set as the document default.

`w:sz` is in half-points: `sz=22` is 11pt. Sizes below give both.

| Role | Font | Size | Color | Spacing | Notes |
|---|---|---|---|---|---|
| **Header subheader** | Montserrat Bold | 10pt (`sz=20`) | `FFFFFF` | after 60 (3pt) | Left column, top. Program/initiative name. Placeholder: *Break Into Tech* |
| **Header H1** | Montserrat Bold | 22pt (`sz=44`) | `FFFFFF` | before 40 (2pt) | Left column, below subheader. Document title |
| **Header descriptive text** | Montserrat Regular | 10pt (`sz=20`) | `FFFFFF` | before 80 (4pt) | Right column, below logo, right-aligned. Placeholder: *Partnership or Initiative* |
| **H2 — section** | Montserrat Bold | 15pt (`sz=30`) | `4933D1` | before 420 (21pt), after 200 (10pt) | Numbered sections. Carries the bottom rule. |
| **H3 — subsection** | Montserrat Bold | 12pt (`sz=24`) | `4933D1` | before 260 (13pt), after 120 (6pt) | No rule |
| **H4 — micro-heading** | Open Sans **Bold Italic** | 11pt (`sz=22`) | `1A1A1A` | before 240, after 240 (12pt) | Inline topic label; deliberately not purple |
| **Body** | Open Sans | 11pt (`sz=22`) | `1A1A1A` | after 160 (8pt) | `w:lineRule="auto"` |
| **List item** | Open Sans | 11pt (`sz=22`) | `1A1A1A` | after 100 (5pt) | See Lists |
| **Signature caption** | Open Sans | 9pt (`sz=18`) | `5A5A5A` | after 120 (6pt) | Sits under the rule |
| **Hyperlink** | Open Sans | 11pt (`sz=22`) | `4933D1` + underline | — | Never blue |

**Never use Heading1.** In the source file `Heading1` is still Word's factory style — 16pt, `2E74B5` blue — and using it drops an off-brand blue heading into the document. Section headings start at **H2**. If you need a level above H2, the header band is that level.

The H2 rule (its defining feature):

```xml
<w:pBdr><w:bottom w:val="single" w:sz="12" w:space="4" w:color="4933d1"/></w:pBdr>
```

`sz=12` is 1.5pt, `space=4` is 4pt of air between text and rule. Sections 4–9 of the source doc carry it; sections 1–3 don't, because the rule lives in the `Heading2` *style* and those three paragraphs override `pPr`. **Put it on every H2** — the inconsistency in the source is a bug, not a design choice.

---

## Page setup
 US Letter, portrait.

| Property | DXA | Inches |
|---|---|---|
| Page size | 12240 × 15840 | 8.5 × 11 |
| Top margin | 900 | 0.625 |
| Left / right margin | 1080 | 0.75 |
| Bottom margin | 1080 | 0.75 |
| Header / footer | 720 | 0.5 |
| **Usable content width** | **10080** | **7.0** |

Full-bleed-to-margin elements (callouts, data tables) are `10080` DXA wide. The source file uses `10095` — 15 DXA of overhang past the right margin. Use `10080`. The header band has its own fixed column widths (below) that total slightly more than this — that's expected, see the note there.

---

## The header band — reproduce exactly

**[verified, updated spec]** A two-column, single-row borderless table with a solid `4933D1` fill across both cells. (This is Skillcrush Purple, not literally light blue — see the color palette above; use the hex regardless of what it's called.) Not a Word header (`header1.xml` doesn't exist) — it's the first content on page 1, so body text flows normally beneath it and it does not repeat on later pages. That's intentional.

This replaces the single-cell-plus-floating-logo construction from the original source file: the logo is now an **inline image inside the second column**, not a page-anchored object layered on top. Inline is more robust — it moves with the row instead of needing re-checked coordinates whenever the band's height changes.

### Table and column setup

| Property | Value |
|---|---|
| Rows / columns | 1 row, 2 columns |
| Fill (both cells) | `4933D1`, `<w:shd w:val="clear"/>` |
| Borders | `nil` on all sides, both cells |
| Cell margins | 320 DXA (0.222″) all four sides, both cells |
| Vertical alignment | `center`, both cells |
| Table layout | `fixed` |

| Column | Width (in) | Width (DXA) | Content alignment |
|---|---|---|---|
| 1 | 5.42 | 7831 | Left |
| 2 | 1.58 | 2280 | Right |
| **Total** | **7.021** | **10111** | |

Conversion is `inches × 1440`. The total is 31 DXA (≈0.02″) past the standard `10080` content width used elsewhere in this system — that's consistent with the source document's own header table, which likewise overhung the margin slightly (15 DXA). Harmless at this scale; don't stretch the columns to force an exact fit, since that changes the ratio the widths were specified with.

### Column 1 (left-aligned)

Two paragraphs, in order:

1. **Subheader** — Montserrat Bold, 10pt (`sz=20`), white, `spacing after="60"`. If the document being styled already states a program or initiative name, use that; otherwise use the placeholder **"Break Into Tech."**
2. **H1** — Montserrat Bold, 22pt (`sz=44`), white, `spacing before="40"`. The document's title.

### Column 2 (right-aligned)

Two paragraphs, in order:

1. **Logo** — inline image, `jc="right"`. Source: `https://raw.githubusercontent.com/skillcrush-curriculum/brand-assets/fe018016ec9fee1389b35c2bfdc74b638d9fefa5/logo/skillcrush-by-ptf-white-2x.png` (white Skillcrush by PowerToFly lockup, verified 266 × 100px, transparent PNG, 2.66:1 aspect ratio). Fetch this URL fresh when building a new document so you always get the current logo; `assets/skillcrush-ptf-logo-white.png` in this skill is the same file, bundled as an offline fallback if the network isn't reachable.

   Recommended render size: **1.05″ × 0.39″** (960,120 × 356,616 EMU). That fits inside column 2's usable width (1.583″ − 0.444″ padding = 1.139″) with a little breathing room, and preserves the 2.66:1 ratio. Don't stretch it to fill the column — the white logo needs a margin of purple around it to read cleanly.

2. **Descriptive text** — Montserrat Regular, 10pt (`sz=20`), white, `jc="right"`, `spacing before="80"`. Placeholder: **"Partnership or Initiative."** (This is the general case of the source file's "In partnership with Zero Irving" line — use that specific phrasing when the document actually names a partner, otherwise fall back to the placeholder.)

### XML skeleton

```xml
<w:tbl>
  <w:tblPr>
    <w:tblW w:w="10111" w:type="dxa"/><w:jc w:val="left"/>
    <w:tblLayout w:type="fixed"/><w:tblLook w:val="0000"/>
  </w:tblPr>
  <w:tblGrid>
    <w:gridCol w:w="7831"/>
    <w:gridCol w:w="2280"/>
  </w:tblGrid>
  <w:tr>
    <w:tc>
      <w:tcPr>
        <w:tcW w:w="7831" w:type="dxa"/>
        <w:tcBorders><w:top w:val="nil"/><w:left w:val="nil"/><w:bottom w:val="nil"/><w:right w:val="nil"/></w:tcBorders>
        <w:shd w:fill="4933d1" w:val="clear"/>
        <w:tcMar>
          <w:top w:w="320" w:type="dxa"/><w:left w:w="320" w:type="dxa"/>
          <w:bottom w:w="320" w:type="dxa"/><w:right w:w="320" w:type="dxa"/>
        </w:tcMar>
        <w:vAlign w:val="center"/>
      </w:tcPr>
      <w:p><w:pPr><w:jc w:val="left"/><w:spacing w:after="60"/></w:pPr>
        <w:r><w:rPr><w:rFonts w:ascii="Montserrat" w:hAnsi="Montserrat"/><w:b/><w:color w:val="ffffff"/><w:sz w:val="20"/></w:rPr>
          <w:t>Break Into Tech</w:t></w:r></w:p>
      <w:p><w:pPr><w:jc w:val="left"/><w:spacing w:before="40"/></w:pPr>
        <w:r><w:rPr><w:rFonts w:ascii="Montserrat" w:hAnsi="Montserrat"/><w:b/><w:color w:val="ffffff"/><w:sz w:val="44"/></w:rPr>
          <w:t>Document Title Here</w:t></w:r></w:p>
    </w:tc>
    <w:tc>
      <w:tcPr>
        <w:tcW w:w="2280" w:type="dxa"/>
        <w:tcBorders><w:top w:val="nil"/><w:left w:val="nil"/><w:bottom w:val="nil"/><w:right w:val="nil"/></w:tcBorders>
        <w:shd w:fill="4933d1" w:val="clear"/>
        <w:tcMar>
          <w:top w:w="320" w:type="dxa"/><w:left w:w="320" w:type="dxa"/>
          <w:bottom w:w="320" w:type="dxa"/><w:right w:w="320" w:type="dxa"/>
        </w:tcMar>
        <w:vAlign w:val="center"/>
      </w:tcPr>
      <w:p><w:pPr><w:jc w:val="right"/></w:pPr>
        <w:r><!-- inline w:drawing, logo, ~1.05in x 0.39in --></w:r></w:p>
      <w:p><w:pPr><w:jc w:val="right"/><w:spacing w:before="80"/></w:pPr>
        <w:r><w:rPr><w:rFonts w:ascii="Montserrat" w:hAnsi="Montserrat"/><w:color w:val="ffffff"/><w:sz w:val="20"/></w:rPr>
          <w:t>Partnership or Initiative</w:t></w:r></w:p>
    </w:tc>
  </w:tr>
</w:tbl>
```

With docx-js: a `Table` with `columnWidths: [7831, 2280]`, each `TableCell` given a matching `width: { size: ..., type: WidthType.DXA }`, `shading: { type: ShadingType.CLEAR, fill: "4933D1" }` (never `SOLID` — that renders black), `margins: { top: 320, left: 320, bottom: 320, right: 320 }`, and `verticalAlign: VerticalAlign.CENTER`. The logo is an `ImageRun` inside a right-aligned `Paragraph` in the second cell, `type: "png"`, sized to the dimensions above.

---

## Callout boxes

Same construction as the header band, at lower saturation: one-cell borderless table, `tblW = 10080`, **`tcMar` 160 DXA top/bottom and 220 DXA left/right** — tighter than the band. Two paragraphs inside:

- **Heading** — Montserrat **Bold 11pt** in the variant's accent color, `spacing after="60"`. Ends with a colon in the source (`Action needed by August 31st:`).
- **Body** — default Open Sans 11pt `1A1A1A`, no extra spacing. Put body text inline to the heading text.

Follow the box with a normal body paragraph; don't stack two callouts back to back.

| Fill | Heading color | Use for |
|---|---|---|---|
| `E5FFF6` | `4933D1` | Deadlines, "you must do X by Y", acceptance steps |

```xml
<w:shd w:fill="e5fff6" w:val="clear"/>
<w:tcMar>
  <w:top w:w="160" w:type="dxa"/><w:left w:w="220" w:type="dxa"/>
  <w:bottom w:w="160" w:type="dxa"/><w:right w:w="220" w:type="dxa"/>
</w:tcMar>
```

Aim for **at most one callout per section**, and no more than three or four in a document. In the source there's exactly one, on page 1, and it lands hard because of that. Every extra box costs the others their weight.

---

## Data tables
 — the source document contains no data tables, so this section extends the palette rather than reproducing it. Keep it consistent with what's above: purple carries structure, fills stay pale enough to read 11pt body copy over.

| Element | Fill | Type |
|---|---|---|
| Header row | `4933D1` | Montserrat Bold 10pt, `FFFFFF`, cells vertically centered |
| Body rows (odd) | `FFFFFF` | Open Sans 11pt, `1A1A1A` |
| Body rows (even, zebra) | `F8F7FD` | same |
| Emphasis / total row | `E5FFF6` | Open Sans **Bold** 11pt |
| Section-divider row | `EDEAFA` | Montserrat Bold 10pt, `4933D1` |
| Gridlines | `D2CCF3`, `sz=4` (0.5pt) `single`, inside only | outer borders `nil` |
| Cell margins | 100 DXA top/bottom, 140 DXA left/right | |

Zebra striping and gridlines are alternatives, not partners — pick one. Zebra suits five-plus rows; gridlines suit dense numeric tables. Use both and the table starts fighting the callouts for attention.

```xml
<!-- header cell -->
<w:tcPr>
  <w:shd w:fill="4933d1" w:val="clear"/>
  <w:vAlign w:val="center"/>
  <w:tcMar>
    <w:top w:w="100" w:type="dxa"/><w:left w:w="140" w:type="dxa"/>
    <w:bottom w:w="100" w:type="dxa"/><w:right w:w="140" w:type="dxa"/>
  </w:tcMar>
</w:tcPr>
```

Set `<w:tblHeader/>` in `trPr` on the header row so it repeats across page breaks. Column widths must sum to `10080`, and in docx-js every cell needs its own `width` in `WidthType.DXA` alongside the table's `columnWidths` — percentages break in Google Docs, which matters here since these documents get shared as Google Docs.

---

## Lists
 Standard bullet: `•`, `ind left="504" hanging="288"`, `spacing after="100"`. Use this everywhere.

The source also contains a second convention — `●` (a heavier glyph) at `left="720" hanging="360"` with `spacing after="0"` inside the group — used in sections 4, 5, and 7. It's a paste artifact from another document and it looks noticeably different. **Standardize on `•` at 504/288.**

Bolded lead-ins are a signature move and worth keeping: `**Ownership and transfer:** The laptop is provided…`. Bold run, colon inside the bold, then a normal run.

---

## Signature block
 Four line/caption pairs (Student Signature, Print Name, Date, Program Manager Signature). Each pair is two paragraphs — no table, no underscores.

1. **Rule** — empty paragraph, `spacing before="200" after="40"`, with a bottom border:
   ```xml
   <w:pBdr><w:bottom w:val="single" w:sz="6" w:space="1" w:color="999999"/></w:pBdr>
   ```
2. **Caption** — Open Sans 9pt `5A5A5A`, `spacing after="120"`.

Never use a one-row table as a horizontal rule, and never type `_______` — both break when the document is converted or edited.

---

## Document skeleton

```
[empty anchor paragraph + logo]
[purple header band: eyebrow / title / partner line]
Opening paragraphs — warm, direct, second person
[Callout: the single most important thing]
More opening context
H2  1. First Section          ← with purple bottom rule
    body / bullets
H2  2. Second Section
  H3  Subsection
      body
  H4  Micro-heading
      body
…
H2  9. Acknowledgment
    bullets summarizing what signing confirms
[signature block ×4]
```

---

## Voice note

The visual system assumes the copy that goes with it: second person, contractions, plain language, no legalese theater. If the writing turns stiff, the design reads cold rather than warm. For PTF and Skillcrush copy, use the `ptf-writing-skill`.

---

## Before shipping

Render and look at it — colors and spacing that read fine in XML often don't survive contact with a page:

```bash
python /mnt/skills/public/docx/scripts/office/soffice.py --headless --convert-to pdf out.docx
pdftoppm -jpeg -r 100 out.pdf page
```

Then check:

- [ ] Header band is two columns (5.438″ / 1.583″), both cells filled `4933D1`, logo inline in column 2 and not clipped or stretched
- [ ] All heading text is bold, including header subheader and header descriptive text
- [ ] Montserrat and Open Sans embedded (`word/fonts/`) — headings are not falling back to Calibri
- [ ] Every H2 carries the 1.5pt purple rule; no Heading1 (blue) anywhere
- [ ] One bullet convention throughout
- [ ] At most one callout per section
- [ ] Table header rows repeat across page breaks; column widths sum to 10080
- [ ] Signature rules are paragraph borders, not tables or underscores
