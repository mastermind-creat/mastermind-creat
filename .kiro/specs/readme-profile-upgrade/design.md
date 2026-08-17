# Design Document: README Profile Upgrade

## Overview

This document covers the technical design for upgrading the `README.md` of the `mastermind-creat` GitHub profile. The work is purely presentational — no application code, no backend, no database. The output is a single Markdown file (`README.md`) that renders correctly inside GitHub's sanitised HTML pipeline.

GitHub's README renderer supports a strict subset of HTML: `<div>`, `<p>`, `<table>`, `<tr>`, `<td>`, `<img>`, `<a>`, `<details>`, `<summary>`, `<br>`, `<sub>`, `<sup>`, and `<picture>`. It strips `<style>`, `<script>`, `class`, `id`, and inline `style` attributes from most elements (except a small allowlist on `<img>` and tables). True CSS animations, JavaScript, and parallax effects are therefore not achievable inside a README. All motion must be embedded in external SVG/GIF image assets served over HTTPS from trusted providers.

The five upgrade areas are:

1. **Technology Radar redesign** — replace `./images/tech-radar.svg` with a quadrant-based, Markdown-native layout built from Skillicons icon grids and Shields.io badges, plus an animated Readme_Typing_SVG subtitle.
2. **Other Impactful Systems table** — add rows for Sokomtaa and Gameverse.
3. **Animated header upgrade** — update Capsule_Render gradient palette and Readme_Typing_SVG hero strings to include Sokomtaa and AI-systems references.
4. **Section dividers** — insert consistent capsule-render soft dividers or andreasbm rainbow lines between every major section.
5. **Stats section polish and visual consistency** — enforce tokyonight/algolia theme across all stats widgets, centre all badge rows, and use Fira Code on all typing SVGs.

---

## Architecture

The README is a static document compiled once and served by GitHub. There is no runtime logic. The "architecture" is a content pipeline:

```
Author edits README.md
       │
       ▼
GitHub Markdown renderer (cmark-gfm + HTML sanitiser)
       │
       ▼
Browser renders sanitised HTML
       │
       ├─► External image requests (capsule-render, skillicons, shields.io,
       │    readme-typing-svg, github-readme-stats, streak-stats,
       │    github-profile-trophy, github-profile-summary-cards,
       │    raw.githubusercontent.com snake SVG)
       │
       └─► Visitor sees finished profile page
```

All dynamic content (animated SVGs, stat cards) is rendered server-side by the respective third-party service and delivered as an image. The README itself contains only static Markdown/HTML.

### Rendering Constraints

| Constraint | Impact |
|---|---|
| `style` attribute stripped on most elements | No inline color/font on divs; colour must come from image URLs |
| `<script>` stripped | No JavaScript; no parallax |
| `<style>` stripped | No CSS animations in HTML |
| External images must be HTTPS | All service URLs are HTTPS-only |
| `align` attribute allowed on `<div>`, `<td>`, `<p>` | Used for centering |
| `valign` allowed on `<td>` | Used for top-aligning project cards |
| Animated GIFs render inline | Used for motion accents in headings |
| SVG `<animate>` tags from trusted services render | Typing SVGs and snake SVG work |

---

## Components and Interfaces

Each upgrade area maps to a discrete block in the README. The blocks are composed of HTML + Markdown mixed syntax, consistent with the existing file's style.

### Component 1: Animated Header Banner

**Service:** `capsule-render.vercel.app`

**Interface (URL parameters):**

| Parameter | Current value | Upgraded value |
|---|---|---|
| `type` | `waving` | `waving` |
| `color` | `gradient` + `customColorList=12,16,20` | `gradient` + `customColorList=6,11,20` |
| `customColorList` | `12,16,20` | encodes purple→cyan→emerald sweep |
| `text` | `WAMBIA KENNEDY` | unchanged |
| `desc` | current subtitle | unchanged |
| `animation` | `twinkling` | `twinkling` (kept) |

The `customColorList` values 6, 11, 20 map to capsule-render's pre-defined palette slots that approximate the `#8B5CF6` / `#06B6D4` / `#10B981` target colours. Exact hex is not directly settable; the closest slot mapping will be used and documented in the README source comment.

### Component 2: Hero Typing SVG

**Service:** `readme-typing-svg.herokuapp.com`

Updated `lines` parameter (URL-encoded):

```
💻 WAMBIA KENNEDY
🚀 Digital Systems Architect
🤖 AI Systems Builder
🏪 Creator of Sokomtaa
📦 Architect of Scalable Products
```

Font: `Fira+Code`, weight: `800`, size: `40`, color: `10B981`.

### Component 3: Subtitle Typing SVG

Updated `lines` parameter:

```
Building Intelligent, Scalable & Impact-First Digital Ecosystems
Powering Sokomtaa — AI Marketplace for Neighbourhoods
Where Engineering Excellence Meets Strategic Vision
Architecting Tomorrow's Systems Today
```

Font: `Fira+Code`, weight: `500`, size: `20`, color: `8B5CF6`.

### Component 4: Technology Radar (Redesigned)

**Replaces:** `<img src="./images/tech-radar.svg" .../>`

**Structure:** Four visually distinct subsections inside a centred `<div>`, each prefixed by a coloured Shields.io badge label and followed by a Skillicons icon row.

```
┌─────────────────────────────────────────────────────────┐
│  [Animated subtitle — Readme_Typing_SVG]                │
│                                                         │
│  🟢 ADOPT — Production Ready                            │
│  [skillicons row: core production stack]                │
│  [shields.io badges: confidence indicators]             │
│                                                         │
│  🔵 TRIAL — Actively Evaluating                         │
│  [skillicons row]  [shields badges]                     │
│                                                         │
│  🟡 ASSESS — Researching / Prototyping                  │
│  [skillicons row]  [shields badges]                     │
│                                                         │
│  ⚫ ON HOLD / LEGACY                                     │
│  [skillicons row]  [shields badges]                     │
└─────────────────────────────────────────────────────────┘
```

**Skillicons interface:** `https://skillicons.dev/icons?i={icon1},{icon2},...&theme=dark`

Available icon keys relevant to this profile: `nextjs`, `react`, `nodejs`, `laravel`, `python`, `ts`, `js`, `postgresql`, `mysql`, `redis`, `docker`, `linux`, `git`, `github`, `nginx`, `tailwind`, `php`, `prisma`, `graphql`, `figma`, `vite`, `postman`, `prometheus`, `grafana`, `mongodb`, `supabase`, `firebase`, `vercel`.

**Shields.io quadrant badge interface:**
```
https://img.shields.io/badge/{label}-{status}-{color}?style=for-the-badge
```

**Radar section subtitle (Readme_Typing_SVG):**
```
lines=⚡ Adopt: Production-Ready Stack | 🔬 Trial: Actively Evaluating | 🔭 Assess: On the Horizon
```

### Component 5a: Sokomtaa — Featured Architecture Project Card

Sokomtaa is the flagship AI-powered product and is added as a new project card in the **Featured Architecture Projects** two-column grid table (alongside Elimu Tech, SnapAura, PageForge, LaunchVerse). It gets its own card cell following the same template pattern as the existing cards:

- Badge color: `#8B5CF6` (purple) — flagship AI product receives the primary brand color
- Image: `./images/sokomtaa.png`
- Live link badge: `https://sokomtaa.co.ke`
- Skillicons row: `nextjs,nodejs,postgresql,openai` (representative AI/full-stack stack)
- Architecture highlights: AI-powered recommendations, neighborhood-scoped marketplace, real-time listings

This adds a third row to the Featured Architecture Projects table (row 3 = Sokomtaa + one partner card or standalone full-width), or inserts as the 5th card extending the existing 2×2 grid to 2×3.

### Component 5b: Other Impactful Systems Table — New Rows

Two new rows appended to the existing Markdown table inside the `<details>` block:

| Column | Gameverse value |
|---|---|
| Project | Gameverse |
| Purpose | Gamers arena — browser-based multi-game collection & gaming ecosystem |
| Tech Stack | JavaScript, React, Canvas API |
| Impact Metric | Live gaming platform at epldls.vercel.app |

Sokomtaa is intentionally placed in the **Featured Architecture Projects** section (Component 5a) rather than this table, as it is a flagship product warranting full card treatment.

### Component 6: Section Dividers

Two divider styles available:

- **andreasbm rainbow line:** `https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png` — already used in the existing file; will be inserted between every major section that currently uses `---` or lacks a divider.
- **Capsule-render soft divider (alternative):** a shorter `height=4` capsule-render gradient slice can be used where a thinner rule is preferred.

Decision: use the rainbow line consistently to match the existing instance already in the file.

### Component 7: GitHub Stats Section

All existing cards are retained. The polish changes:

| Card | Theme parameter | Change |
|---|---|---|
| Profile summary card | `theme=tokyonight` | already correct — keep |
| Stats card | `bg_color=0d1117`, `title_color=8B5CF6`, `icon_color=10B981` | keep existing |
| Top languages card | same as stats card | keep existing |
| Streak card | `theme=tokyonight-duo` | already correct — keep; verify ring/fire/label colours match palette |
| Trophy card | `theme=algolia` | already correct — keep |

All cards wrapped in `<div align="center">`. `<br/>` spacers between card rows.

### Component 8: Footer Banner

Retain existing capsule-render waving footer. Update `customColorList` to match the header upgrade (`6,11,20`).

---

## Data Models

There is no runtime data model. The "data" in this context is the structured content embedded in the README. The relevant schema is the shape of each reusable block.

### Typing SVG URL Schema

```
https://readme-typing-svg.herokuapp.com
  ?font={font_family}          // e.g. Fira+Code
  &weight={font_weight}        // e.g. 800
  &size={font_size_px}         // e.g. 40
  &duration={ms_per_line}      // e.g. 4000
  &pause={ms_between}          // e.g. 500
  &color={hex_no_hash}         // e.g. 10B981
  &center=true
  &vCenter=true
  &width={px}
  &height={px}
  &lines={url_encoded_lines}   // pipe-separated: line1|line2|line3
```

### Skillicons URL Schema

```
https://skillicons.dev/icons
  ?i={comma_separated_icon_keys}
  &theme=dark                  // dark | light
  &perline={n}                 // optional: icons per row
```

Max ~30 icons per request before the image becomes unwieldy. Split into multiple rows if needed.

### Shields.io Badge Schema

```
https://img.shields.io/badge/{left_label}-{right_label}-{color_hex}
  ?style=for-the-badge
  &logo={simple_icons_slug}    // optional
  &logoColor=white             // optional
```

### Capsule-Render URL Schema

```
https://capsule-render.vercel.app/api
  ?type=waving                 // waving | soft | rect | shark | egg | ...
  &color=gradient
  &customColorList={slots}     // comma-separated palette slot numbers
  &height={px}
  &section=header|footer
  &text={url_encoded_text}     // header only
  &fontSize={px}               // header only
  &fontAlignY={0-100}          // header only
  &desc={url_encoded_desc}     // header only
  &descAlignY={0-100}          // header only
  &descSize={px}               // header only
  &animation=twinkling|fadeIn|blinking|scaleIn
```

### Project Table Row Schema

Each row in the Other Impactful Systems table follows this Markdown table format:

```markdown
| {Project Name} | {Purpose description} | {Tech Stack, comma-separated} | {Impact metric or live URL} |
```

---

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system — essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

The properties below are derived from the acceptance criteria. Because the output artifact is a Markdown file, "execution" means "the README.md source is parsed and its structure is verified". Properties are universally quantified over the elements of the document — for instance, "for all Readme_Typing_SVG URLs" means every such URL that appears anywhere in the file must satisfy the stated condition.

---

### Property 1: All quadrant confidence badges are present

*For any* Tech Radar section in the README, each of the four quadrants (Adopt, Trial, Assess, On Hold/Legacy) must contain at least one Shields.io badge conveying a proficiency or confidence label (e.g., "Production Ready", "Actively Evaluating", "Researching").

**Validates: Requirements 1.3**

---

### Property 2: All required technologies appear in the Tech Radar

*For any* version of the upgraded README, every technology key that was present in the original Tech Radar row (mysql, postgresql, redis, docker, linux, git, github, nginx, prometheus, grafana) must appear within a `skillicons.dev` URL inside the Technology Radar section.

**Validates: Requirements 1.4**

---

### Property 3: All technology entries use consistent icon-based visual style

*For any* technology listed in the Tech Radar quadrants, its representation must use either a Skillicons icon URL or a Shields.io badge URL — no technology name may appear as bare plain text without an accompanying icon or badge in the same section.

**Validates: Requirements 1.5**

---

### Property 4: Hero Typing SVG contains at least three rotating lines

*For any* Readme_Typing_SVG URL used as the hero title element, the `lines` URL parameter must contain at least three pipe-separated entries, ensuring at least three strings rotate in the animation.

**Validates: Requirements 3.2**

---

### Property 5: Section dividers appear between all major sections

*For any* pair of consecutive major sections in the README (identified by `##`-level headings), at least one Section_Divider element (rainbow line `<img>` or capsule-render divider `<img>`) must appear in the source between the two sections.

**Validates: Requirements 3.3**

---

### Property 6: All Readme_Typing_SVG calls use Fira Code font

*For any* `readme-typing-svg.herokuapp.com` URL that appears in the README, the `font` parameter must be set to `Fira+Code`.

**Validates: Requirements 5.4**

---

### Property 7: Color palette is restricted to the defined set

*For any* hex color value that appears inside a Shields.io badge URL or a Capsule_Render URL in the README, that value must be one of the four palette colors: `8B5CF6` (purple), `10B981` (emerald), `F59E0B` (amber), `0EA5E9` / `06B6D4` (cyan variants), or `0A66C2` / `EA4335` / `1DA1F2` / `E4405F` / `FF0000` / `1877F2` (brand-locked social badge colors that are fixed by the external service and exempt from the palette rule). New badges added during this upgrade must use only the four primary palette colors.

**Validates: Requirements 5.1**

---

### Property 8: All project card table cells are top-aligned

*For any* `<td>` element inside a project-card table (the Featured Architecture Projects table and any similar card grid), the `valign` attribute must be set to `"top"`.

**Validates: Requirements 5.3**

---

### Property 9: All image and badge rows are center-aligned

*For any* `<div>` element that wraps one or more `<img>` tags or badge `<a>` elements, the `align` attribute must be set to `"center"`.

**Validates: Requirements 5.2**

---

### Property 10: All existing external service URLs are preserved

*For any* external service URL present in the original `README.md` (capsule-render banners, readme-typing-svg, komarev profile views, github-readme-stats, streak-stats, github-profile-trophy, github-profile-summary-cards, snake SVG, shields.io social badges), the upgraded README must still contain a URL pointing to the same service with the same primary parameters (username, theme, card type). URLs may be updated for parameter improvements but must not be deleted.

**Validates: Requirements 5.6**

---

### Property 11: Table rows have consistent column count

*For any* row in the Other Impactful Systems Markdown table (including the newly added Gameverse row and the Sokomtaa row), the number of pipe-delimited columns must equal the number of columns in the header row.

**Validates: Requirements 2.5**

---

## Error Handling

Because the output is a static Markdown file (not executable code), "errors" are authoring mistakes that cause visual degradation on the GitHub profile page. The following categories of failure and their mitigations are relevant:

### Broken External Image URLs

**Risk:** A third-party service (capsule-render, skillicons, shields.io, readme-typing-svg) changes its URL schema or goes offline. The image renders as a broken icon on GitHub.

**Mitigation:**
- All service URLs are well-established and actively maintained as of 2024–2025.
- `capsule-render.vercel.app`, `skillicons.dev`, `shields.io`, and `readme-typing-svg.herokuapp.com` are among the most widely used README services with strong uptime records.
- For the snake animation, a `<picture>` element with fallback `src` is already in place.
- If a service goes down temporarily, the broken image is cosmetic only — no data loss, no functional regression.
- Periodic manual review of the profile page is the recommended monitoring approach.

### GitHub Sanitiser Stripping Attributes

**Risk:** GitHub's HTML sanitiser strips an attribute used for layout (e.g., `align`, `valign`, `width` on `<td>`). The layout degrades to left-aligned or full-width.

**Mitigation:**
- All HTML attributes used (`align`, `valign`, `width`, `height`, `src`, `alt`) are on GitHub's allowlist for README rendering.
- Avoid `style`, `class`, or `id` attributes — these are stripped and are not used in this design.
- Use only `<table>`, `<tr>`, `<td>`, `<div>`, `<p>`, `<img>`, `<a>`, `<details>`, `<summary>`, `<picture>`, `<source>`, `<br>`, `<sub>`, `<sup>` — all confirmed supported tags.

### Overly Long Skillicons Requests

**Risk:** A single `skillicons.dev` request with too many icon keys (>30) may produce an oversized or malformed image.

**Mitigation:**
- Split large icon groups across two separate `<img>` rows within the same quadrant section.
- Each Adopt quadrant row: max 12 icons per request.
- Keep each URL under 200 characters for URL-length safety.

### Typing SVG Lines Too Long

**Risk:** Very long strings in the `lines` parameter of `readme-typing-svg` overflow the specified `width` and truncate or wrap awkwardly.

**Mitigation:**
- Keep each line under 55 characters.
- Use `width=800` for subtitle SVGs and `width=600` for title SVGs.
- Test URL visually by loading the direct image URL before committing.

### Markdown Table Formatting Errors

**Risk:** An incorrectly formatted Markdown table row (missing pipes, mismatched column count) breaks the entire table rendering.

**Mitigation:**
- Each new row must be validated against Property 11 (column count consistency).
- Use a consistent pipe-column structure: `| Project | Purpose | Tech Stack | Impact Metric |` — four columns, same as existing rows.

### Capsule-Render Color Slot Mismatch

**Risk:** The `customColorList` slot numbers do not produce the intended purple/cyan/emerald gradient.

**Mitigation:**
- Document the exact slot numbers chosen and their visual approximation in a comment in the README source.
- Slots `6`, `11`, `20` are used; these can be adjusted if the visual output is unsatisfactory. A `color=0:8B5CF6,50:06B6D4,100:10B981` gradient string can be used as an alternative if capsule-render supports direct hex gradients (supported via the `gradient` type with `customColorList` as hex values in newer API versions).

---

## Testing Strategy

The output artifact is a Markdown file. There is no application runtime to unit-test in the traditional sense. Testing is verification of the document's structural and content correctness.

### Dual Testing Approach

**Manual visual review** (analogous to integration tests): load the profile page after committing and visually verify each section renders as designed.

**Automated structural tests** (unit + property tests): parse the `README.md` source and assert structural invariants programmatically.

### Unit Tests

Unit tests verify concrete, specific facts about the README source:

- The first element of the file is a capsule-render waving header with `section=header`.
- The last non-whitespace element is a capsule-render waving footer with `section=footer`.
- The snake animation `<picture>` element exists with both dark and light `srcset` values.
- The streak-stats card URL contains `ring=10B981&fire=F59E0B`.
- The github-profile-trophy URL contains `theme=algolia`.
- The github-profile-summary-cards URL contains `theme=tokyonight`.
- The Gameverse row appears inside the `<details>` block.
- The Sokomtaa project card appears in the Featured Architecture Projects table.
- A `readme-typing-svg` URL exists within the Technology Radar section.
- The static `./images/tech-radar.svg` reference no longer exists in the file.

### Property-Based Tests

Each property from the Correctness Properties section maps to one property-based test. Given the document nature of the artifact, property tests are implemented as parameterized structural checks over the document's parsed elements.

**Library:** [fast-check](https://github.com/dubzzz/fast-check) (JavaScript/TypeScript) is the recommended PBT library, used against a simple README parser that extracts sections, URLs, table rows, and HTML elements.

**Minimum iterations per property:** 100 (fast-check default; for document structure tests the "random input" is generated variations of the README content, not just fixed parsing).

**Tag format for each test:**
```
// Feature: readme-profile-upgrade, Property {N}: {property_text}
```

| Property | Test description | PBT pattern |
|---|---|---|
| Property 1 | For all quadrant subsections extracted from Tech Radar, each contains a shields.io badge with a confidence label | Invariant |
| Property 2 | For all required technology keys, each appears in at least one skillicons URL within the Tech Radar section | Invariant |
| Property 3 | For all technology entries in the radar, none are plain text without an accompanying icon/badge URL | Invariant |
| Property 4 | For the hero typing SVG URL, lines count ≥ 3 | Invariant |
| Property 5 | For all consecutive section heading pairs, a divider element exists between them | Invariant |
| Property 6 | For all readme-typing-svg URLs, font=Fira+Code | Invariant |
| Property 7 | For all new Shields.io/capsule-render color hex values, value ∈ allowed palette set | Invariant |
| Property 8 | For all `<td>` in project card tables, valign="top" | Invariant |
| Property 9 | For all `<div>` wrapping images/badges, align="center" | Invariant |
| Property 10 | For all original service URLs, a URL to the same service still exists in the upgraded file | Round-trip / preservation |
| Property 11 | For all table rows, column count equals header column count | Invariant |

**Property Test Configuration:**

```typescript
// Feature: readme-profile-upgrade, Property 6: All Readme_Typing_SVG calls use Fira Code font
fc.assert(
  fc.property(
    fc.constantFrom(...extractTypingSvgUrls(readmeSource)),
    (url) => {
      const params = new URLSearchParams(new URL(url).search);
      return params.get('font') === 'Fira+Code';
    }
  ),
  { numRuns: 100 }
);
```

```typescript
// Feature: readme-profile-upgrade, Property 11: Table rows have consistent column count
fc.assert(
  fc.property(
    fc.constantFrom(...extractOtherSystemsTableRows(readmeSource)),
    (row) => countColumns(row) === EXPECTED_COLUMN_COUNT
  ),
  { numRuns: 100 }
);
```

### Manual Visual Review Checklist

After committing the upgraded README, verify the following visually on `github.com/mastermind-creat`:

1. Header banner displays purple→cyan→emerald gradient wave.
2. Hero typing SVG animates through all lines correctly.
3. Tech Radar section shows four labelled quadrant blocks with Skillicons rows and confidence badges.
4. Tech Radar animated subtitle typewriter is visible.
5. Other Impactful Systems table expands to show Gameverse and Sokomtaa rows.
6. Sokomtaa project card appears in Featured Architecture Projects alongside existing cards.
7. All section dividers (rainbow lines) appear between each major section.
8. Snake animation renders in both light/dark mode.
9. Streak card, stats cards, trophy cards, and summary card all render without broken images.
10. Footer banner matches header gradient style.

