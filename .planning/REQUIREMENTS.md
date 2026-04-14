# Requirements: GitHub Profile README

**Defined:** 2026-04-14
**Core Value:** Visitors instantly understand who Niraj is, what he builds, and how to connect with him — in under 10 seconds of scrolling.

## v1 Requirements

### Identity

- [ ] **IDEN-01**: Professional headline with name, current role (Sr. Software Engineer / Tech Lead at Emirates NBD), and builder-focused one-liner
- [ ] **IDEN-02**: Social links bar with LinkedIn, X/Twitter, and email as Shields.io badge links

### Tech Stack

- [ ] **TECH-01**: Tech stack badges using Shields.io grouped by category (Languages, Frontend, Backend, Cloud/Infra, AI/ML)
- [ ] **TECH-02**: Badges use `for-the-badge` style with appropriate brand colors from Simple Icons

### Open Source

- [ ] **OSS-01**: Open source contributions section with direct links to merged PRs (LangChain.js, LangChain Docs, react-chrono)
- [ ] **OSS-02**: Each contribution includes repo name, brief description of the PR, and clickable PR link

### GitHub Stats

- [ ] **STAT-01**: GitHub stats card showing contributions, stars, PRs via github-readme-stats
- [ ] **STAT-02**: Top languages card showing most-used languages
- [ ] **STAT-03**: GitHub streak stats card via github-readme-streak-stats
- [ ] **STAT-04**: All stats cards support dark/light mode via `<picture>` element

### Layout & Quality

- [ ] **LAYT-01**: Clean section ordering: Identity → Social → Tech Stack → OSS Contributions → Stats
- [ ] **LAYT-02**: Layout uses HTML `align` attribute (not CSS) for centering — compatible with GitHub's sanitizer
- [ ] **LAYT-03**: Mobile-friendly layout that stacks gracefully on narrow viewports
- [ ] **LAYT-04**: All badge URLs use correct encoding (double-dash for literal hyphens)

## v2 Requirements

### Identity

- **IDEN-03**: Animated typing SVG header with rotating titles
- **IDEN-04**: Current focus/status line showing active project

### Stats

- **STAT-05**: Self-hosted github-readme-stats on private Vercel instance with PAT for reliability

### Content

- **CONT-01**: Featured projects section with descriptions and links
- **CONT-02**: Certifications section (AWS Solutions Architect Professional)
- **CONT-03**: Key achievements section (Leap Framework, 10M+ downloads, 900K DKK savings)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Visitor counter badge | Vanity metric, unprofessional for builder brand |
| Snake contribution animation | Gimmicky, requires GitHub Actions overhead |
| Blog posts auto-update | Operational complexity for marginal value |
| Portfolio website | Separate project, not part of README scope |
| Pinned repo configuration | GitHub UI setting, not README content |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| IDEN-01 | Phase 1 | Pending |
| IDEN-02 | Phase 1 | Pending |
| TECH-01 | Phase 2 | Pending |
| TECH-02 | Phase 2 | Pending |
| OSS-01 | Phase 2 | Pending |
| OSS-02 | Phase 2 | Pending |
| STAT-01 | Phase 3 | Pending |
| STAT-02 | Phase 3 | Pending |
| STAT-03 | Phase 3 | Pending |
| STAT-04 | Phase 3 | Pending |
| LAYT-01 | Phase 3 | Pending |
| LAYT-02 | Phase 3 | Pending |
| LAYT-03 | Phase 3 | Pending |
| LAYT-04 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 14 total
- Mapped to phases: 14
- Unmapped: 0 ✓

---
*Requirements defined: 2026-04-14*
*Last updated: 2026-04-14 after roadmap creation*
