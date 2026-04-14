<!-- GSD:project-start source:PROJECT.md -->
## Project

**GitHub Profile README**

A GitHub profile README for Niraj Kumar (github.com/nkhunters) — a polished, balanced markdown file that showcases his identity as a builder and senior engineer. Displayed automatically on the GitHub profile page via the special `nkhunters/nkhunters` repository.

**Core Value:** Visitors instantly understand who Niraj is, what he builds, and how to connect with him — in under 10 seconds of scrolling.

### Constraints

- **Format**: Pure GitHub-flavored markdown — must render correctly on github.com
- **Content source**: cv.md in project root contains all factual information
- **Tone**: Builder brand — showcases expertise, not job-seeking
- **Visual balance**: Stats cards and badges yes, but clean and organized
<!-- GSD:project-end -->

<!-- GSD:stack-start source:research/STACK.md -->
## Technology Stack

## Recommended Stack
### Format Layer — The File Itself
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| GitHub Flavored Markdown (GFM) | Current | Primary file format | The only format GitHub's profile README renderer accepts. Pure `.md`, no build step. |
| HTML subset (inline) | N/A | Layout control (centering, alignment) | GFM allows a restricted HTML subset. `<p align="center">` and `<div align="center">` work. Inline `style=` CSS is stripped by GitHub's sanitizer — use deprecated HTML attributes instead (`align="center"`). |
### Stats Cards
| Tool | URL | Purpose | Why |
|------|-----|---------|-----|
| github-readme-stats (anuraghazra) | https://github.com/anuraghazra/github-readme-stats | Stats card, top-languages card | De facto standard. 70k+ stars. Supports themes, custom colors, layout options. |
| github-readme-streak-stats (DenverCoder1) | https://github.com/DenverCoder1/github-readme-streak-stats | Contribution streak counter | 6.8k stars, actively maintained (last release v1.6.0, February 2026). Runs on Heroku at `streak-stats.demolab.com`. |
### Tech Stack Badges
| Tool | URL | Purpose | Why |
|------|-----|---------|-----|
| Shields.io | https://shields.io | Static tech stack badges | Most widely used badge service. Stable, fast CDN, huge adoption. Pulls icons from Simple Icons. |
| Simple Icons | https://simpleicons.org | Icon source for shields.io | Provides SVG icons for 2000+ brands with official brand colors. Used as `logo=` param in shields.io URLs. |
| Skill Icons | https://skillicons.dev (https://github.com/tandpfun/skill-icons) | Grouped skill icon cards | Cloudflare Workers-backed. 300+ icons. Renders a compact icon grid from a comma-separated list — clean alternative to individual shields.io badges when showing many technologies at once. |
### Social / Contact Links
| Approach | Why |
|----------|-----|
| Inline image links using shields.io badges | Consistent with the rest of the badge style. Use `style=for-the-badge` with `logo=linkedin`, `logo=x`, `logo=gmail`. |
| Plain markdown links (no badge) | Fallback if badge loading fails. Include alongside badge for accessibility. |
### Visitor Counter (Optional)
| Tool | URL | Decision |
|------|-----|---------|
| Profile Views Counter | https://komarev.com/ghpvc/ | **Skip in v1.** PROJECT.md explicitly says "balanced, not overdone." A visitor counter reads as a vanity metric and adds noise. |
## What NOT to Use
| Tool/Approach | Why Not |
|---------------|---------|
| **WakaTime card** (from github-readme-stats) | Requires WakaTime IDE plugin to be installed and actively used. No data = empty/broken card. Not appropriate unless already using WakaTime. |
| **GitHub Trophies** (`ryo-ma/github-profile-trophy`) | Adds significant visual weight. Creates a "gamified" feel that conflicts with the "professional builder brand" goal. |
| **Animated GIFs / Lottie** | Explicitly out of scope in PROJECT.md. Overdone visual style. |
| **GitHub Actions for dynamic content generation** | Overkill for v1. Only needed if public stats endpoints prove unreliable. |
| **Custom SVG / canvas graphics** | Explicitly out of scope in PROJECT.md. Complex to maintain. |
| **CSS-only styling** | GitHub's sanitizer strips inline `style=` attributes. Any CSS styling will silently fail. Use deprecated HTML attributes (`align=`) instead. |
| **Typing SVG animators** (`readme-typing-svg`)| Adds animation that conflicts with the "balanced, not flashy" visual goal stated in PROJECT.md. |
| **Readme Activity Graph** | Heavy visual element. Adds a third stats widget on top of stats + streak, pushing past "balanced" into "overdone." |
## Alternatives Considered
| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Skill icons | Skill Icons (skillicons.dev) | devicons via CDN | Devicons requires hosting a font file or using a CDN with fragile HTML. Skill Icons is simpler — one URL, no font dependency. |
| Stats cards | anuraghazra/github-readme-stats | githubchart-api | github-readme-stats has more themes, more active maintenance, and vastly higher adoption (70k+ stars). |
| Streak stats | DenverCoder1/streak-stats | None | Only mature dedicated streak widget in the ecosystem. Actively maintained through 2026. |
| Badge style | for-the-badge | flat, plastic | for-the-badge is the dominant style in professional profile READMEs. More readable at small sizes than plastic. More visually substantial than flat. |
| Social links | Shields.io badges | Simple HTML `<a>` links | Badges are visually consistent with the rest of the profile. Plain links would look mismatched. |
## Dependency Map
## Sources
- anuraghazra/github-readme-stats official repo: https://github.com/anuraghazra/github-readme-stats
- Rate limit issue #4431: https://github.com/anuraghazra/github-readme-stats/issues/4431
- Vercel deprecation discussion #4748: https://github.com/anuraghazra/github-readme-stats/issues/4748
- DenverCoder1/github-readme-streak-stats: https://github.com/DenverCoder1/github-readme-streak-stats (v1.6.0, Feb 2026)
- tandpfun/skill-icons: https://github.com/tandpfun/skill-icons
- Shields.io official: https://shields.io
- Shields.io static badge docs: https://shields.io/badges
- Simple Icons: https://simpleicons.org
- Ileriayo/markdown-badges (badge reference): https://github.com/Ileriayo/markdown-badges
- GitHub community discussion on CSS/alignment in profile READMEs: https://github.com/orgs/community/discussions/183876
- awesome-github-profile-readme (curated examples): https://github.com/abhisheknaiidu/awesome-github-profile-readme
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found. Add skills to any of: `.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, or `.github/skills/` with a `SKILL.md` index file.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
