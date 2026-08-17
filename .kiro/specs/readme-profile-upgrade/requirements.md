# Requirements Document

## Introduction

Upgrade the GitHub profile README (`README.md`) for mastermind-creat with three focused improvements:

1. **Technology Radar section upgrade** — Replace the static SVG image with a rich, interactive/animated representation of the tech radar using GitHub-compatible Markdown, shields, and inline SVG techniques.
2. **Gameverse entry in "Other Impactful Systems"** — Add a new project row for "Gameverse" inside the collapsible project portfolio table.
3. **Animations, parallax effects, and UI polish** — Leverage GitHub Markdown-supported animation techniques (animated GIFs, capsule-render banners, readme-typing-svg, animated badges, and CSS-style HTML tricks) to elevate the overall visual quality and flow of the README.

> Note: GitHub README files render a limited subset of HTML/CSS. True parallax (JS-based scroll effects) is not supported. The requirements below operate within GitHub's rendering constraints.

---

## Glossary

- **README**: The `README.md` file rendered on the GitHub profile page at `github.com/mastermind-creat`.
- **Tech_Radar**: The section titled "🛠️ Technology Radar" that visualises the developer's curated technology stack.
- **Other_Systems_Table**: The collapsible `<details>` table under "🔗 Other Impactful Systems".
- **Gameverse**: A new project entry to be added to the Other_Systems_Table.
- **Capsule_Render**: The `capsule-render.vercel.app` service used for animated/gradient header and footer banners.
- **Readme_Typing_SVG**: The `readme-typing-svg.herokuapp.com` service used for animated typewriter text.
- **Shields_IO**: The `shields.io` badge service used for skill, status, and label badges.
- **Skillicons**: The `skillicons.dev` icon grid service used for technology icons.
- **GitHub_Stats**: The `github-readme-stats.vercel.app` service for statistics cards.
- **Snake_Animation**: The GitHub Actions–generated contribution snake SVG.
- **Section_Divider**: Decorative horizontal rule images (e.g., rainbow line from `andreasbm/readme`) used between sections.

---

## Requirements

### Requirement 1: Technology Radar Section Upgrade

**User Story:** As a profile visitor, I want to see a visually rich and informative technology radar section, so that I can quickly understand the developer's stack depth, categorisation, and current focus areas at a glance.

#### Acceptance Criteria

1. THE Tech_Radar SHALL display technologies grouped into at least four named quadrants or categories: **Adopt** (production-ready, daily use), **Trial** (actively evaluating), **Assess** (researching), and **On Hold / Legacy**.
2. WHEN the Tech_Radar section is rendered on GitHub, THE Tech_Radar SHALL use Skillicons icon grids, Shields_IO badges, or inline SVG to present each technology with a visible logo and label — replacing the static `./images/tech-radar.svg` image reference.
3. THE Tech_Radar SHALL include a proficiency or confidence indicator for each category group (e.g., a coloured badge showing "Production Ready", "Actively Learning", "Evaluating").
4. THE Tech_Radar SHALL maintain all technologies already present in the existing radar image and badge row (MySQL, PostgreSQL, Redis, Docker, Linux, Git, GitHub, Nginx, Prometheus, Grafana) within the appropriate quadrant.
5. WHEN new technologies are added to the Adopt or Trial categories, THE Tech_Radar SHALL present them using the same visual style as existing entries (Skillicons + Shields_IO).
6. THE Tech_Radar SHALL include a section subtitle using Readme_Typing_SVG with rotating category names to create an animated teaser effect.

---

### Requirement 2: Add Gameverse to Other Impactful Systems

**User Story:** As a profile visitor, I want to see Gameverse listed in the project portfolio table, so that I have a complete picture of the developer's work including gaming-related projects.

#### Acceptance Criteria

1. THE Other_Systems_Table SHALL contain a new row for **Gameverse** with the four required columns: Project, Purpose, Tech Stack, and Impact Metric.
2. THE Gameverse row SHALL describe its Purpose as a multi-game interactive web platform (browser-based game collection / gaming ecosystem).
3. THE Gameverse row SHALL list a relevant Tech Stack (e.g., JavaScript, React, Canvas API, or equivalent stack used).
4. THE Gameverse row SHALL include a meaningful Impact Metric (e.g., number of games, active users, or sessions).
5. WHEN the `<details>` block is expanded by a visitor, THE Gameverse row SHALL be visible and consistently formatted with all other rows in the Other_Systems_Table.

---

### Requirement 3: Animated Header and Section Banners

**User Story:** As a profile visitor, I want the README to have eye-catching animated banners and visual dividers, so that the page feels dynamic and professional from the first scroll.

#### Acceptance Criteria

1. THE README SHALL use a Capsule_Render waving banner as the very first element, consistent with the current header style but with an upgraded gradient palette aligned to the site's color scheme (purple `#8B5CF6`, cyan `#06B6D4`, emerald `#10B981`).
2. WHEN a visitor loads the profile page, THE README SHALL present the animated Readme_Typing_SVG hero title with at least three rotating strings reflecting identity and role.
3. THE README SHALL include Section_Dividers between major sections using either the rainbow line asset or an equivalent animated/gradient capsule-render divider.
4. IF a section contains a major project or achievement, THEN THE README SHALL use an animated GIF or badge accent (from Shields_IO or giphy) within that section to maintain visual momentum.
5. THE README SHALL end with a Capsule_Render waving footer banner, consistent with the existing footer style.

---

### Requirement 4: Animated Stats and Activity Indicators

**User Story:** As a profile visitor, I want to see live GitHub stats, streaks, and activity visuals with consistent theming, so that I can assess the developer's coding activity quickly.

#### Acceptance Criteria

1. THE README SHALL display GitHub_Stats cards (stats card + top languages card) side-by-side using a `<div align="center">` layout with matching `tokyonight`-derived theme colors.
2. THE README SHALL include the Snake_Animation contribution graph with both dark and light scheme `<picture>` sources.
3. THE README SHALL display the GitHub streak card using a consistent color scheme (ring, fire, and label colors matching the primary palette).
4. WHEN the stats section is viewed, THE README SHALL also include GitHub trophy cards rendered with the `algolia` theme at full width.
5. THE README SHALL include the GitHub profile summary card from `github-profile-summary-cards.vercel.app` using the `tokyonight` theme.

---

### Requirement 5: Consistent Visual Language and Polish

**User Story:** As a profile visitor, I want all sections to follow a cohesive visual language, so that the README feels intentionally designed rather than assembled from parts.

#### Acceptance Criteria

1. THE README SHALL use a consistent color palette across all badge, banner, and icon elements: primary purple (`#8B5CF6`), accent emerald (`#10B981`), highlight amber (`#F59E0B`), and info cyan (`#0EA5E9`).
2. THE README SHALL use `align="center"` on all image and badge rows to maintain horizontal centering throughout.
3. WHEN a section uses a table for project cards, THE README SHALL use `valign="top"` on table cells so content is top-aligned.
4. THE README SHALL use the `Fira Code` font parameter on all Readme_Typing_SVG calls for typographic consistency with the portfolio site.
5. IF a section heading uses an emoji, THEN THE README SHALL also include a matching animated GIF or icon badge inline within the heading or directly below it, to add motion accents consistently across sections.
6. THE README SHALL keep all existing external service URLs functional and not remove any currently working badge, stat card, or animation that is already rendering correctly.
