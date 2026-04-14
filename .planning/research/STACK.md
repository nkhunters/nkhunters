# Technology Stack

**Project:** GitHub Profile README — nkhunters
**Researched:** 2026-04-14
**Overall confidence:** HIGH (all core tools verified against official repos and current docs)

---

## Recommended Stack

### Format Layer — The File Itself

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| GitHub Flavored Markdown (GFM) | Current | Primary file format | The only format GitHub's profile README renderer accepts. Pure `.md`, no build step. |
| HTML subset (inline) | N/A | Layout control (centering, alignment) | GFM allows a restricted HTML subset. `<p align="center">` and `<div align="center">` work. Inline `style=` CSS is stripped by GitHub's sanitizer — use deprecated HTML attributes instead (`align="center"`). |

**Confidence: HIGH** — GitHub's rendering pipeline is well-documented and behavior is verifiable by inspection.

---

### Stats Cards

| Tool | URL | Purpose | Why |
|------|-----|---------|-----|
| github-readme-stats (anuraghazra) | https://github.com/anuraghazra/github-readme-stats | Stats card, top-languages card | De facto standard. 70k+ stars. Supports themes, custom colors, layout options. |
| github-readme-streak-stats (DenverCoder1) | https://github.com/DenverCoder1/github-readme-streak-stats | Contribution streak counter | 6.8k stars, actively maintained (last release v1.6.0, February 2026). Runs on Heroku at `streak-stats.demolab.com`. |

**Embed endpoints to use:**

```markdown
<!-- Stats card -->
![Stats](https://github-readme-stats.vercel.app/api?username=nkhunters&show_icons=true&theme=default)

<!-- Top Languages -->
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=nkhunters&layout=compact)

<!-- Streak -->
[![Streak](https://streak-stats.demolab.com/?user=nkhunters)](https://git.io/streak-stats)
```

**RELIABILITY WARNING (MEDIUM confidence):** The public Vercel instance of github-readme-stats (`github-readme-stats.vercel.app`) has documented rate-limit failures. The project maintainers have opened issue #4748 discussing deprecating Vercel hosting entirely, and issue #4431 confirms public rate limit errors are recurring. For a personal README that renders for potentially thousands of visitors, the public instance is acceptable as a starting point but may show broken images intermittently. If reliability matters, the GitHub Actions deployment option (generates static SVGs on a schedule) is the safe path.

**Action:** Use the public Vercel instance in v1. Document self-hosting via GitHub Actions as a known follow-up if breakage is observed.

---

### Tech Stack Badges

| Tool | URL | Purpose | Why |
|------|-----|---------|-----|
| Shields.io | https://shields.io | Static tech stack badges | Most widely used badge service. Stable, fast CDN, huge adoption. Pulls icons from Simple Icons. |
| Simple Icons | https://simpleicons.org | Icon source for shields.io | Provides SVG icons for 2000+ brands with official brand colors. Used as `logo=` param in shields.io URLs. |
| Skill Icons | https://skillicons.dev (https://github.com/tandpfun/skill-icons) | Grouped skill icon cards | Cloudflare Workers-backed. 300+ icons. Renders a compact icon grid from a comma-separated list — clean alternative to individual shields.io badges when showing many technologies at once. |

**Shields.io badge URL pattern:**

```
https://img.shields.io/badge/<LABEL>-<COLOR>?style=for-the-badge&logo=<SIMPLE_ICONS_SLUG>&logoColor=white
```

Example for Node.js:
```markdown
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
```

**Style choice:** Use `for-the-badge` style. It renders taller, more legible badges that look professional at GitHub's default README width. The `flat` style is smaller and better suited for repo-level CI badges, not profile showcases.

**Confidence: HIGH** — Shields.io and Simple Icons are among the most starred badge projects on GitHub. URL format verified via official docs.

---

### Social / Contact Links

| Approach | Why |
|----------|-----|
| Inline image links using shields.io badges | Consistent with the rest of the badge style. Use `style=for-the-badge` with `logo=linkedin`, `logo=x`, `logo=gmail`. |
| Plain markdown links (no badge) | Fallback if badge loading fails. Include alongside badge for accessibility. |

Example patterns:
```markdown
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/nirajk77777)
[![X](https://img.shields.io/badge/X-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/nirajk777)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:NIRAJK77777@GMAIL.COM)
```

---

### Visitor Counter (Optional)

| Tool | URL | Decision |
|------|-----|---------|
| Profile Views Counter | https://komarev.com/ghpvc/ | **Skip in v1.** PROJECT.md explicitly says "balanced, not overdone." A visitor counter reads as a vanity metric and adds noise. |

---

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

---

## Alternatives Considered

| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Skill icons | Skill Icons (skillicons.dev) | devicons via CDN | Devicons requires hosting a font file or using a CDN with fragile HTML. Skill Icons is simpler — one URL, no font dependency. |
| Stats cards | anuraghazra/github-readme-stats | githubchart-api | github-readme-stats has more themes, more active maintenance, and vastly higher adoption (70k+ stars). |
| Streak stats | DenverCoder1/streak-stats | None | Only mature dedicated streak widget in the ecosystem. Actively maintained through 2026. |
| Badge style | for-the-badge | flat, plastic | for-the-badge is the dominant style in professional profile READMEs. More readable at small sizes than plastic. More visually substantial than flat. |
| Social links | Shields.io badges | Simple HTML `<a>` links | Badges are visually consistent with the rest of the profile. Plain links would look mismatched. |

---

## Dependency Map

```
Profile README (README.md)
├── Stats cards ──────────────── github-readme-stats (public Vercel OR self-hosted)
├── Streak card ──────────────── streak-stats.demolab.com
├── Tech stack badges ────────── shields.io → Simple Icons (logos)
├── Social badges ────────────── shields.io → Simple Icons (logos)
└── Skill icons (optional) ───── skillicons.dev → Cloudflare Workers
```

All dependencies are external services embedded as image URLs. There is no local build step. The README is a single flat markdown file.

---

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
