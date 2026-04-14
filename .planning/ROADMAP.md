# Roadmap: GitHub Profile README

## Overview

Build a polished GitHub profile README for Niraj Kumar (nkhunters) in three natural passes: first establish identity and social presence, then populate the technical substance (tech stack and open source contributions), and finally wire up the GitHub stats cards with a full layout and quality pass. The result is a single markdown file that lets any visitor understand who Niraj is, what he builds, and how to reach him in under 10 seconds.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Identity & Social** - Name, role, one-liner, and social links bar
- [ ] **Phase 2: Tech Stack & Open Source** - Categorized tech badges and OSS contribution highlights
- [ ] **Phase 3: Stats, Layout & Polish** - GitHub stats cards, section ordering, mobile-friendliness, and URL correctness

## Phase Details

### Phase 1: Identity & Social
**Goal**: Visitors can instantly see who Niraj is and reach him via any channel
**Depends on**: Nothing (first phase)
**Requirements**: IDEN-01, IDEN-02
**Success Criteria** (what must be TRUE):
  1. The README opens with a professional headline showing Niraj's name, current role (Sr. Software Engineer / Tech Lead at Emirates NBD), and a builder-focused one-liner
  2. A social links bar is visible with clickable Shields.io badge links for LinkedIn, X/Twitter, and email
  3. The tone reads as "builder brand," not "hire me"
**Plans**: TBD
**UI hint**: yes

### Phase 2: Tech Stack & Open Source
**Goal**: Visitors can see what Niraj builds and his open source track record
**Depends on**: Phase 1
**Requirements**: TECH-01, TECH-02, OSS-01, OSS-02
**Success Criteria** (what must be TRUE):
  1. A tech stack section shows badges grouped by category: Languages, Frontend, Backend, Cloud/Infra, AI/ML
  2. All tech badges use `for-the-badge` style with appropriate brand colors from Simple Icons
  3. An open source contributions section lists LangChain.js, LangChain Docs, and react-chrono PRs with clickable links
  4. Each OSS entry includes the repo name, a brief description of the PR, and a direct link to the merged PR
**Plans**: TBD
**UI hint**: yes

### Phase 3: Stats, Layout & Polish
**Goal**: The README renders correctly, looks balanced, and works on all screen sizes
**Depends on**: Phase 2
**Requirements**: STAT-01, STAT-02, STAT-03, STAT-04, LAYT-01, LAYT-02, LAYT-03, LAYT-04
**Success Criteria** (what must be TRUE):
  1. GitHub stats card (contributions, stars, PRs), top languages card, and streak stats card are all visible and loading
  2. All three stats cards switch correctly between dark and light appearance using a `<picture>` element
  3. Sections appear in order: Identity → Social → Tech Stack → OSS Contributions → Stats
  4. Layout uses HTML `align` attributes (not CSS) for centering and renders without broken elements on GitHub
  5. Viewing the README on a narrow viewport (mobile) stacks sections gracefully without horizontal overflow
**Plans**: TBD
**UI hint**: yes

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Identity & Social | 0/TBD | Not started | - |
| 2. Tech Stack & Open Source | 0/TBD | Not started | - |
| 3. Stats, Layout & Polish | 0/TBD | Not started | - |
