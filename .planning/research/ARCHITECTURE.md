# Architecture Patterns: GitHub Profile README

**Domain:** GitHub Profile README for Senior Software Engineer / Tech Lead
**Researched:** 2026-04-14
**Confidence:** HIGH (multiple authoritative sources, verified against github-readme-stats docs and GitHub rendering constraints)

---

## Recommended Architecture

A GitHub profile README is a single-file linear document rendered top-to-bottom by GitHub's markdown processor. "Architecture" means: section order, layout patterns within sections, and how sections relate visually and narratively.

The design challenge is twofold:
1. **Narrative flow** — each section should build on the last to tell a complete professional story
2. **Scannability** — visitors spend under 10 seconds deciding whether to engage; information must be extractable at a glance

---

## Section Ordering (Canonical for Senior Builder Brand)

### Rationale for this order

The sequence is designed around a "funnel of trust":
- Hook → Identity → Proof (deep) → Proof (social) → Call to action

Visitors skim top-to-bottom. The heaviest-signal content (who you are, what you build) must land before the fold. Stats and open source proof follow once identity is established. Social links close the loop.

```
┌─────────────────────────────────────┐
│  1. HOOK / IDENTITY HEADER          │  ← Above the fold: who + what in 1-2 lines
├─────────────────────────────────────┤
│  2. CURRENT STATUS / FOCUS LINE     │  ← One active signal ("building X at Y")
├─────────────────────────────────────┤
│  3. SOCIAL / CONTACT BAR            │  ← Badges: LinkedIn, X, Email — scannable
├─────────────────────────────────────┤
│  4. TECH STACK                      │  ← Visual badge grid: languages, cloud, AI
├─────────────────────────────────────┤
│  5. OPEN SOURCE HIGHLIGHTS          │  ← Specific PRs with links (proof of depth)
├─────────────────────────────────────┤
│  6. GITHUB STATS                    │  ← Stats card + streak + top langs (side by side)
└─────────────────────────────────────┘
```

### Section-by-Section Justification

| # | Section | Why Here | Depends On |
|---|---------|----------|------------|
| 1 | Hook / Identity | First thing rendered; must orient visitor instantly | Nothing |
| 2 | Current Focus | Adds temporal relevance; makes the hook actionable | Section 1 (extends identity) |
| 3 | Social Bar | Immediately after identity while engagement intent is highest | Section 1 |
| 4 | Tech Stack | Substantiates the identity claim with visual evidence | Section 1 (validates the "builder" framing) |
| 5 | Open Source | Differentiator — moves from "claims skills" to "demonstrated skills" | Section 4 (stack context makes OSS PRs more meaningful) |
| 6 | GitHub Stats | Quantitative confirmation of activity and range | Sections 4-5 (stats interpreted in context of stack/OSS) |

**Why stats are last, not first:** Stats without context (who you are, what you build) are noise. Placed after stack and OSS, they function as confirmation not introduction.

**Why social bar is near top:** Visitor engagement intent peaks immediately after reading the identity. Deferring social links to the bottom reduces click-through on first-time visitors.

---

## Markdown Layout Patterns

### Pattern 1: Centered Identity Header

Use `<div align="center">` for the opening block. GitHub strips CSS but honors the `align` attribute. This centers the name, tagline, and social badges symmetrically.

```html
<div align="center">
  <h1>Hi, I'm Niraj Kumar</h1>
  <p>Senior Software Engineer · Tech Lead · Builder</p>
</div>
```

**Why `<div>` not `<p align="center">`:** `<div>` creates a block-level container that consistently centers multi-line content across GitHub's renderer.

### Pattern 2: Badge-Based Social Bar

Inline badges using Shields.io or similar, wrapped in anchor tags. Group by type: contact links together, no mixing with stack badges.

```html
<div align="center">
  <a href="https://linkedin.com/in/nirajk77777"><img src="https://img.shields.io/badge/LinkedIn-nirajk77777-0A66C2?style=flat&logo=linkedin" /></a>
  <a href="https://twitter.com/nirajk777"><img src="https://img.shields.io/badge/X-nirajk777-000000?style=flat&logo=x" /></a>
  <a href="mailto:NIRAJK77777@GMAIL.COM"><img src="https://img.shields.io/badge/Email-contact-D14836?style=flat&logo=gmail" /></a>
</div>
```

**Constraints:**
- Keep badge count to 3-5 in the social bar (clarity degrades beyond that)
- All badges should link somewhere; decorative-only badges waste real estate
- Use consistent `style=flat` or `style=for-the-badge` throughout — mixing creates visual noise

### Pattern 3: Tech Stack Badge Grid

No headings inside the grid; section heading above suffices. Badges grouped by category via line breaks, not sub-headers (sub-headers break the compact visual rhythm).

```markdown
## Tech Stack

**Languages:** ![TypeScript](badge) ![Python](badge) ![Go](badge)

**AI/ML:** ![LangChain](badge) ![PyTorch](badge) ![Triton](badge)

**Cloud:** ![AWS](badge) ![Docker](badge)

**Frontend:** ![React](badge) ![React Native](badge)
```

**Why prose-style grouping:** GitHub renders badge images inline. Line-break groupings with bold category labels give visual structure without the weight of `###` sub-headings, which add too much vertical space in a badge-dense section.

### Pattern 4: Side-by-Side Stats Cards

The canonical pattern from github-readme-stats documentation uses adjacent `<a>` + `<img>` tags with `align="center"` and explicit `height` values. GitHub does not auto-arrange images side by side — explicit HTML is required.

```html
<div align="center">
  <a href="https://github.com/nkhunters">
    <img height="180" src="https://github-readme-stats.vercel.app/api?username=nkhunters&show_icons=true&theme=default&hide_border=true" />
  </a>
  <a href="https://github.com/nkhunters">
    <img height="180" src="https://github-readme-stats.vercel.app/api/top-langs?username=nkhunters&layout=compact&langs_count=8&card_width=320&hide_border=true" />
  </a>
</div>
```

**Critical constraint:** Set equal `height` on both cards. Mismatched heights cause vertical misalignment. Set `card_width` on the languages card to prevent overlap with the stats card.

**Mobile behavior:** On narrow viewports (< 768px), adjacent inline images stack vertically automatically. No extra code required — this is GitHub's default behavior for images that exceed viewport width.

### Pattern 5: Open Source Highlights as Structured List

Avoid tables for OSS contributions — tables render poorly on mobile and look rigid for narrative content. Use a compact unordered list with bolded repo names and inline PR links.

```markdown
## Open Source Contributions

- **LangChain.js** — [PR #XXXX: Description](link)
- **LangChain Docs** — [PR #XXXX: Description](link)
- **react-chrono** — [PR #XXXX: Description](link)
```

**Why not a table:** Tables require fixed column widths that break on narrow screens. Lists with bold + inline links scan as fast as tables and remain readable at any viewport width.

### Pattern 6: Section Separators

Use `---` (horizontal rule) between major sections only if sections would otherwise blend together visually. Between badge-dense sections (social → stack) a separator helps. Between narrative sections (identity → status) no separator needed — the whitespace is sufficient.

---

## Component Boundaries

| Component | Responsibility | Should NOT Contain |
|-----------|---------------|-------------------|
| Identity Header | Name, role, one-liner tagline | Stats, badges, long prose |
| Status Line | Single current-focus statement | Multiple items, past history |
| Social Bar | Contact/profile links only | Tech stack badges |
| Tech Stack | Technology categories with badges | Project descriptions, links to repos |
| OSS Highlights | Specific PRs with links and short descriptions | Generic "I contribute to OSS" statements |
| GitHub Stats | Auto-generated cards only | Manual stats or manually listed numbers |

**Separation rationale:** Mixing contact links with tech badges (common mistake) makes both harder to scan. Keeping components single-responsibility allows a visitor to extract exactly what they need without parsing unrelated content.

---

## Responsive Design Considerations

GitHub renders the profile README in a fixed-width column (~880px on desktop, full viewport width on mobile). Key constraints:

| Concern | Desktop (>768px) | Mobile (<=768px) |
|---------|-----------------|-----------------|
| Side-by-side stats cards | Render inline as intended | Stack vertically (automatic) |
| Badge grids | Full width inline badges | Wrap naturally onto new lines |
| HTML tables | Render with scrollbar if too wide | Horizontal scroll — avoid for key content |
| Markdown tables | Fixed column sizing | Often unusable — prefer lists |
| `align` attribute | Respected | Respected |
| CSS (`style=`) | Stripped by GitHub sanitizer | Stripped by GitHub sanitizer |

**Critical rule:** Do not use CSS `style` attributes for layout — GitHub's Content Security Policy strips them. Use only the `align` HTML attribute and structural elements (`<div>`, `<p>`, `<table>`).

**The 768px breakpoint** is the primary threshold for GitHub profile layout. Designs tested at 880px (desktop) and 375px (mobile) cover the realistic viewport range.

---

## Anti-Patterns to Avoid

### Anti-Pattern 1: Stats Above Identity
**What:** Placing GitHub stats cards at the top before any context
**Why bad:** Stats without identity context are meaningless. "1,200 commits" means nothing until the visitor knows who you are and what you build.
**Instead:** Stats section last, as confirmation not introduction.

### Anti-Pattern 2: Mixing Badge Types in One Block
**What:** Social links, tech stack badges, and "visitor count" badges all in one visual cluster
**Why bad:** Forces the visitor to parse badge semantics instead of scanning quickly. Destroys the scannability advantage badges provide.
**Instead:** One block per badge category, each with a clear section label.

### Anti-Pattern 3: Markdown Tables for OSS Contributions
**What:** Using a 3-column table (Repo | PR | Description) for open source work
**Why bad:** Tables render poorly on mobile and feel bureaucratic for narrative content that benefits from human voice.
**Instead:** Unordered list with bold repo names + inline PR links.

### Anti-Pattern 4: CSS Style Attributes
**What:** `<img style="margin: 10px" />` or `<div style="display: flex">`
**Why bad:** GitHub sanitizes these — they silently do nothing, creating false confidence in layout behavior.
**Instead:** Use `align` attributes and structural HTML only.

### Anti-Pattern 5: Generic Greeting Heading
**What:** `# Hi there, I'm a developer 👋`
**Why bad:** Wastes the highest-attention real estate on a phrase anyone could write.
**Instead:** Name + specific role + differentiated tagline. "Senior Software Engineer · AI Infrastructure · LangChain contributor" communicates more in the same line count.

---

## Build Order for Implementation

The sections have a natural build dependency — earlier sections are self-contained and validate immediately; later sections require external service integration.

```
Phase 1 (Standalone content — no external deps)
  └─ Identity Header
  └─ Current Focus Line
  └─ Social Bar (Shields.io badges — static)

Phase 2 (Tech badges — Shields.io, self-contained)
  └─ Tech Stack section

Phase 3 (Narrative content — requires CV/PR links)
  └─ Open Source Highlights

Phase 4 (External service integration — requires API tokens/deployment)
  └─ GitHub Stats cards (github-readme-stats)
  └─ Streak counter (github-readme-streak-stats)
```

**Why this order:**
- Phases 1-2 can be drafted, previewed, and iterated without any external dependencies
- Phase 3 requires accurate PR URLs from the CV — verify links before writing
- Phase 4 depends on the github-readme-stats service being reachable; test last to avoid blocked iterations on the rest of the content

**Single-pass build is possible** (it's one file), but this phase ordering minimizes rework if stats cards need theme or sizing adjustments.

---

## Sources

- [github-readme-stats documentation — side-by-side layout](https://github.com/anuraghazra/github-readme-stats)
- [Responsive GitHub README techniques — SVG foreignObject, picture element, 768px breakpoint](https://dev.to/grahamthedev/take-your-github-readme-to-the-next-level-responsive-and-light-and-dark-modes--3kpc)
- [Badge best practices — positioning, grouping, quantity](https://daily.dev/blog/best-practices-for-github-markdown-badges)
- [10 Standout GitHub Profile READMEs — structural patterns analysis](https://dev.to/github/10-standout-github-profile-readmes-h2o)
- [Complete guide to GitHub profile README structure](https://dev.to/farhadrahimiklie/how-to-create-the-perfect-github-profile-readme-complete-guide-for-developers-jmf)
- [Two-column image layout in GitHub markdown](https://medium.com/@ManoelFreitas/github-using-images-in-two-columns-in-the-readme-responsive-b616651d0e74)
- [GitHub community — vertical-align constraints in profile READMEs](https://github.com/orgs/community/discussions/183876)
