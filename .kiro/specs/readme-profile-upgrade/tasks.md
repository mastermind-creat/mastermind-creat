# Implementation Plan: README Profile Upgrade

## Overview

Created At: 2026-08-17T09:43:18Z
Completed At: 2026-08-17T15:10:00Z

Sequential edits to `README.md` — each task is a discrete, targeted change to the file. Tasks build on each other and culminate in a fully upgraded profile page.

## Tasks

- [x] 1. Upgrade header capsule-render gradient palette
  - Replace `customColorList=12,16,20` with `customColorList=0:8B5CF6,50:06B6D4,100:10B981` (or nearest capsule-render slots 6,11,20) in the top `<img src="https://capsule-render...section=header...">` tag
  - _Requirements: 3.1_

- [x] 2. Update hero Readme_Typing_SVG lines
  - In the `<h1>` hero `<img src="https://readme-typing-svg...">`, update the `lines` parameter to include at minimum:
    - `💻 WAMBIA KENNEDY`
    - `🚀 Digital Systems Architect`
    - `🤖 AI Systems Builder`
    - `🏪 Creator of Sokomtaa`
    - `📦 Architect of Scalable Products`
  - Confirm `font=Fira+Code`, `weight=800`, `size=40`, `color=10B981`
  - _Requirements: 3.2, 5.4_

- [x] 3. Update subtitle Readme_Typing_SVG lines
  - In the `<h3>` subtitle `<img src="https://readme-typing-svg...">`, update `lines` to include:
    - `Building Intelligent, Scalable & Impact-First Digital Ecosystems`
    - `Powering Sokomtaa — AI Marketplace for Neighbourhoods`
    - `Where Engineering Excellence Meets Strategic Vision`
    - `Architecting Tomorrow's Systems Today`
  - Confirm `font=Fira+Code`, `weight=500`, `size=20`, `color=8B5CF6`
  - _Requirements: 3.2, 5.4_

- [x] 4. Add section dividers between all major sections
  - Identify every `---` horizontal rule and every pair of consecutive `##`-level headings that currently lack a divider between them
  - Replace bare `---` rules and insert `<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" alt="divider" width="100%"/>` between each major section pair
  - Sections requiring a divider: Header→Philosophy, Philosophy→Featured Projects, Featured Projects→Other Systems, Other Systems→Tech Radar, Tech Radar→GitHub Analytics, GitHub Analytics→Roadmap, Roadmap→Priorities, Priorities→Accomplishments, Accomplishments→Ethos, Ethos→Testimonials, Testimonials→Status, Status→Contact, Contact→Visitor Footprint, Visitor Footprint→Gallery
  - _Requirements: 3.3_

  - [ ]* 4.1 Write property test for section dividers (Property 5)
    - **Property 5: Section dividers appear between all major sections**
    - For every consecutive `##`-heading pair extracted from README.md, assert a rainbow-line or capsule-render `<img>` appears in the source between them
    - **Validates: Requirements 3.3**

- [x] 5. Replace static tech-radar.svg with quadrant layout
  - Remove the `<img src="./images/tech-radar.svg" .../>` line entirely
  - Insert a new centered `<div>` block containing four quadrant subsections. Each subsection must have:
    - A Shields.io label badge (e.g., `![ADOPT](https://img.shields.io/badge/🟢_ADOPT-Production_Ready-10B981?style=for-the-badge)`)
    - A `skillicons.dev` icon row for the technologies in that quadrant
  - Quadrant → technologies mapping:
    - **Adopt**: `nextjs,react,nodejs,laravel,python,ts,js,postgresql,mysql,redis,docker,linux`
    - **Trial**: `graphql,prisma,supabase,mongodb,prometheus,grafana`
    - **Assess**: `figma,vite,firebase,vercel`
    - **On Hold / Legacy**: `nginx,git,github,php,postman`
  - Add Readme_Typing_SVG subtitle (font=Fira+Code) with lines: `⚡ Adopt: Production-Ready Stack|🔬 Trial: Actively Evaluating|🔭 Assess: On the Horizon`
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 5.4_

  - [ ]* 5.1 Write property test for Tech Radar quadrants present (Property 1)
    - **Property 1: All quadrant confidence badges are present**
    - For each of the four quadrant subsections parsed from the Tech Radar block, assert at least one `shields.io` badge URL with a confidence label is present
    - **Validates: Requirements 1.3**

  - [ ]* 5.2 Write property test for required technologies in radar (Property 2)
    - **Property 2: All required technologies appear in the Tech Radar**
    - For each key in `[mysql, postgresql, redis, docker, linux, git, github, nginx, prometheus, grafana]`, assert it appears inside a `skillicons.dev` URL within the Tech Radar section
    - **Validates: Requirements 1.4**

  - [ ]* 5.3 Write property test for no bare-text technology entries (Property 3)
    - **Property 3: All technology entries use consistent icon-based visual style**
    - For every technology name listed in the radar section, assert it only appears inside a Skillicons or Shields.io URL — not as bare plain text
    - **Validates: Requirements 1.5**

- [x] 6. Checkpoint — Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [x] 7. Add Sokomtaa project card to Featured Architecture Projects
  - In the Featured Architecture Projects `<table>`, insert a new `<tr>` with a single full-width `<td>` (or extend to a new row pairing) for Sokomtaa
  - Card must follow the same template as existing cards:
    - `valign="top"` on `<td>`
    - Image: `./images/sokomtaa.png` linked to `https://sokomtaa.co.ke`
    - Badge: `![SOKOMTAA](https://img.shields.io/badge/🏪_SOKOMTAA-8B5CF6?style=for-the-badge)`
    - Architecture highlights: AI-powered recommendations, neighbourhood-scoped marketplace, real-time listings
    - Skillicons row: `nextjs,nodejs,postgresql`
    - View Project badge linking to `https://sokomtaa.co.ke` in color `8B5CF6`
  - _Requirements: 5.2, 5.3_

  - [ ]* 7.1 Write property test for Sokomtaa card top-alignment (Property 8)
    - **Property 8: All project card table cells are top-aligned**
    - For every `<td>` element inside the Featured Architecture Projects table, assert `valign="top"` attribute is present
    - **Validates: Requirements 5.3**

  - [ ]* 7.2 Write property test for image/badge rows center-aligned (Property 9)
    - **Property 9: All image and badge rows are center-aligned**
    - For every `<div>` wrapping `<img>` or badge `<a>` elements in the README, assert `align="center"` is set
    - **Validates: Requirements 5.2**

- [x] 8. Add Gameverse row to Other Impactful Systems table
  - Inside the `<details>` block, append a new row to the Markdown table:
    ```
    | Gameverse | Gamers arena — browser-based multi-game collection & gaming ecosystem | JavaScript, React, Canvas API | Live gaming platform at [epldls.vercel.app](https://epldls.vercel.app) |
    ```
  - Image reference for visual context: `./images/gameverse.png` (add as optional inline image in the Impact Metric cell or as a note below the table if desired)
  - Verify the row has exactly four pipe-delimited columns matching the header
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_

  - [ ]* 8.1 Write property test for table column count consistency (Property 11)
    - **Property 11: Table rows have consistent column count**
    - For every row (including the new Gameverse row) in the Other Impactful Systems table, assert the number of `|`-delimited columns equals the header column count (4)
    - **Validates: Requirements 2.5**

- [x] 9. Verify all Readme_Typing_SVG calls use Fira Code font
  - Grep the README for every `readme-typing-svg.herokuapp.com` URL
  - For any URL missing `font=Fira+Code`, update it
  - Includes: hero typing SVG, subtitle typing SVG, Tech Radar subtitle SVG, ethos/philosophy typing SVG
  - _Requirements: 5.4_

  - [ ]* 9.1 Write property test for Fira Code font on all typing SVGs (Property 6)
    - **Property 6: All Readme_Typing_SVG calls use Fira Code font**
    - Extract all `readme-typing-svg.herokuapp.com` URLs from README source; for each, parse the `font` query param and assert it equals `Fira+Code`
    - **Validates: Requirements 5.4**

  - [ ]* 9.2 Write property test for hero typing SVG line count (Property 4)
    - **Property 4: Hero Typing SVG contains at least three rotating lines**
    - For the hero `<h1>` typing SVG URL, parse the `lines` param and assert pipe-separated count ≥ 3
    - **Validates: Requirements 3.2**

- [x] 10. Verify color palette consistency across new badges and banners
  - Review every new Shields.io badge URL and capsule-render URL added in tasks 1–9
  - Assert hex color values used are only: `8B5CF6`, `10B981`, `F59E0B`, `0EA5E9`, or brand-locked social colors (`0A66C2`, `EA4335`, `1DA1F2`, `E4405F`, `FF0000`, `1877F2`, `25D366`)
  - Fix any out-of-palette color found in newly added content
  - _Requirements: 5.1_

  - [ ]* 10.1 Write property test for color palette restriction (Property 7)
    - **Property 7: Color palette is restricted to the defined set**
    - For all hex color values in new Shields.io badge and capsule-render URLs added by this upgrade, assert each value is in the allowed palette set
    - **Validates: Requirements 5.1**

- [x] 11. Upgrade footer capsule-render gradient palette
  - In the closing `<img src="https://capsule-render...section=footer...">` tag, replace `customColorList=12` with `customColorList=0:8B5CF6,50:06B6D4,100:10B981` (or slots 6,11,20) to match the header
  - _Requirements: 3.5_

- [x] 12. Verify all original external service URLs are preserved
  - Check that the following services still have at least one URL present: `capsule-render.vercel.app`, `readme-typing-svg.herokuapp.com`, `komarev.com/ghpvc`, `github-readme-stats.vercel.app`, `streak-stats.demolab.com`, `github-profile-trophy.vercel.app`, `github-profile-summary-cards.vercel.app`, snake SVG from `raw.githubusercontent.com`
  - Confirm snake `<picture>` element still has both dark and light `srcset` values
  - Confirm streak card URL still contains `ring=10B981&fire=F59E0B`
  - Confirm trophy card URL still contains `theme=algolia`
  - Confirm summary card URL still contains `theme=tokyonight`
  - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 5.6_

  - [ ]* 12.1 Write property test for preservation of original service URLs (Property 10)
    - **Property 10: All existing external service URLs are preserved**
    - For each expected service hostname, assert at least one URL pointing to that service exists in the upgraded README
    - **Validates: Requirements 5.6**

- [x] 13. Final checkpoint — Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Tasks execute sequentially against a single file: `README.md`
- Each task references specific requirements for traceability
- Property tests are written in TypeScript using fast-check, parsing README.md as a string
- All Readme_Typing_SVG lines should stay under 55 characters to avoid overflow
