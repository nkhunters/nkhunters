# Domain Pitfalls: GitHub Profile README

**Domain:** GitHub Profile README (github.com/nkhunters)
**Researched:** 2026-04-14
**Confidence:** HIGH — sourced from official GitHub docs, anuraghazra/github-readme-stats issues, shields/shields issues, and community discussions

---

## Critical Pitfalls

Mistakes that cause the README to silently fail, render broken, or not appear at all.

---

### Pitfall 1: Repository Name Does Not Exactly Match Username

**What goes wrong:** The profile README never appears on the GitHub profile page. GitHub silently ignores the README.md file. No error is shown.

**Why it happens:** GitHub only activates the special profile display behavior when the repository name is a byte-for-byte match with the account username — including case. A repo named `NkHunters` when the username is `nkhunters` will not trigger it.

**Consequences:** The entire README is invisible to visitors. Nothing breaks loudly — it just never shows.

**Prevention:**
- Repository must be named exactly `nkhunters` (lowercase matching the GitHub username).
- Repository must be public.
- `README.md` must be in the root of that repository.
- Verify at `github.com/nkhunters` — GitHub shows a green "secret" banner in the repo when the special repo is detected.

**Detection:** Visit `github.com/nkhunters` while logged in as that account. If no profile README section appears above the pinned repos, the repo name or visibility is wrong.

**Phase:** Affects initial setup (Phase 1 / scaffolding).

---

### Pitfall 2: Stats Cards Fail to Load Due to Public API Rate Limits

**What goes wrong:** `github-readme-stats` cards (`github-readme-stats.vercel.app`) appear as broken images or show "Max retries exceeded" or simply render nothing. This happens intermittently because the public Vercel instance is shared across millions of profiles and regularly hits GitHub API rate limits (5,000 requests/hour).

**Why it happens:** The public instance at `https://github-readme-stats.vercel.app` is a shared service. When rate-limited, card SVGs return error responses instead of valid images, which GitHub's CDN may cache for hours.

**Consequences:** Visitors see broken image placeholders where stats cards should be. The profile looks abandoned or technically broken. This is not deterministic — it may look fine locally and broken to visitors.

**Prevention:**
- Deploy a private fork of `anuraghazra/github-readme-stats` to your own Vercel account with a PAT (`PAT_1` env var).
- Use your own instance URL instead of the public one.
- Free Vercel tier works — set `maxDuration: 10` in `vercel.json`.
- Alternatively, use `github-readme-streak-stats` (DenB10) for streak, which has its own separate instance.

**Detection:** Reload the profile page multiple times across different times of day. Check the image URL directly in the browser — if it returns an error JSON or empty SVG, the rate limit is hit.

**Phase:** Affects stats card integration phase. Must be addressed before publishing.

---

### Pitfall 3: Stats Cards Display Broken or Invisible in Dark/Light Mode

**What goes wrong:** A stats card with a hardcoded light theme (`theme=default`) looks fine when the viewer uses GitHub Light mode, but appears with a jarring white box on a dark background when the viewer uses GitHub Dark mode. The reverse is also common: a dark-themed card with no background looks invisible in light mode.

**Why it happens:** GitHub renders READMEs with the user's chosen theme. Stats card SVGs have their own background color baked in. There is no automatic theme-switching unless explicitly implemented.

**Consequences:** The profile looks visually broken for approximately half of visitors (dark mode is the majority among developers).

**Prevention:**
- Use the `<picture>` element with `prefers-color-scheme` media queries to serve two separate card URLs — one with a light theme, one with a dark theme.
- Or use `bg_color=00000000` (transparent) and `hide_border=true` to let the card float on any background.
- Example pattern:
  ```html
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="...&theme=dark">
    <source media="(prefers-color-scheme: light)" srcset="...&theme=default">
    <img src="...&theme=default" />
  </picture>
  ```
- Note: The `<picture>` approach requires the viewer to reload the page when switching GitHub themes — this is a known GitHub limitation, not fixable without JavaScript.

**Detection:** Switch GitHub's appearance between Light and Dark in Settings and reload the profile page. Verify cards do not show a clashing background box.

**Phase:** Affects stats card integration phase.

---

### Pitfall 4: Shields.io Badge URL Encoding Errors (Dashes and Special Characters)

**What goes wrong:** Badge labels or messages containing literal hyphens render incorrectly — either split at the wrong position or producing a broken badge image. For example, a badge meant to read "AWS-SAP" may render as two separate segments or show nothing.

**Why it happens:** Shields.io static badge URLs use `-` as a delimiter between `label`, `message`, and `color` segments. A literal dash inside a label or message is ambiguous. The fix is to double the dash (`--` encodes a single `-`), use `_` for spaces (displayed as space), and URL-encode any other special characters with `%20` etc.

**Consequences:** Badge shows wrong text, splits incorrectly, or does not render — making the tech stack section look broken.

**Prevention:**
- Escape literal dashes with double-dash: `AWS--SAP` in the URL to render "AWS-SAP".
- Use underscores for spaces in badge text: `Node.js` becomes `Node.js`, spaces become `%20` or `_`.
- Validate every badge URL individually in the browser before embedding in the README.
- Use the Shields.io badge builder at `https://shields.io/badges` to auto-generate correct URLs.

**Detection:** Preview the rendered README in the GitHub UI (not just a local Markdown previewer) and visually inspect every badge.

**Phase:** Affects badge/stack section authoring.

---

## Moderate Pitfalls

Mistakes that degrade quality, visual polish, or accuracy without causing total failure.

---

### Pitfall 5: Relative Image Paths That Break Outside the Repository

**What goes wrong:** Images referenced with relative paths (e.g., `./assets/banner.png`) work in the repo file browser but break on the profile page, in forks, or when the branch changes.

**Why it happens:** The profile README is served from the root of the `nkhunters/nkhunters` repo. Relative paths require the image to be in that repo. If images are hosted elsewhere or on a different branch, relative paths resolve incorrectly.

**Prevention:**
- Use absolute URLs for any externally hosted images.
- For images committed to the same repo, use the raw GitHub content URL format: `https://raw.githubusercontent.com/nkhunters/nkhunters/main/assets/image.png`.
- Alternatively, use a CDN or image host (Imgur, GitHub releases) and reference the absolute URL.

**Detection:** View the profile page at `github.com/nkhunters` (not the repository view). Any broken image icon indicates a path problem.

**Phase:** Affects any phase that adds images or banners.

---

### Pitfall 6: CSS Styles Are Stripped — Layout Tricks Fail Silently

**What goes wrong:** Inline CSS styles added to HTML elements in the README (`style="text-align:center"`, `style="margin:10px"`, `class="foo"`) are completely stripped by GitHub's HTML sanitizer. The element renders but without the intended styling, often breaking the intended layout.

**Why it happens:** GitHub's GFM renderer aggressively sanitizes HTML to prevent XSS. All `style=`, `class=`, and `id=` attributes are removed.

**Consequences:** Centered sections appear left-aligned; badge rows with intended padding render flush; custom font-size or color attempts do nothing.

**Prevention:**
- Use only the deprecated `align="center"` attribute on `<div>`, `<p>`, or `<img>` tags — this survives sanitization.
- Use `<table>` with `align` or `cellpadding` for layout spacing — HTML table attributes (not CSS) survive.
- Never use `style=` or `class=` — they will always be stripped.
- Test the actual GitHub render, not a local Markdown preview (local previewers often do not strip CSS).

**Detection:** The gap between local preview and the GitHub-rendered README is the warning sign. Always compare against the live GitHub render.

**Phase:** Affects layout and visual design phase.

---

### Pitfall 7: Badge Visual Overload Obscures Signal

**What goes wrong:** Stacking 30+ tech badges in a single block creates visual noise. Visitors cannot quickly extract which technologies are primary strengths versus incidental knowledge. The section reads as padding rather than substance.

**Why it happens:** Badge generators and community templates encourage adding every technology ever touched. The "more is more" instinct conflicts with the scannability requirement.

**Consequences:** Visitors skip the tech section. The profile fails its core value: "understand who Niraj is in under 10 seconds."

**Prevention:**
- Group badges by category (Languages, Frameworks, Cloud, AI/ML) with clear section headers.
- Limit each category to technologies with genuine depth, not passing familiarity.
- Prefer `for-the-badge` style for primary skills; use `flat` for supporting tools — visual hierarchy reinforces signal.
- For Niraj's profile: Triton, LangChain/LangGraph, Node.js, React/RN, AWS are the core signal. Supporting tools should be visually subordinate.

**Detection:** Time how long it takes to identify the top 3 skills from the badge section. More than 5 seconds means too much clutter.

**Phase:** Affects tech stack section authoring.

---

### Pitfall 8: Stats Cards Show Misleading Language Distribution

**What goes wrong:** The "Top Languages" card (from `github-readme-stats`) counts lines across all public repositories, not skill weighting or project significance. A config-heavy repo (lots of JSON/YAML) can make JSON appear as a top language. Forked repos skew results.

**Why it happens:** The card does a naive line-count across public repos. It does not account for repo quality, size of contributions, or relevance.

**Consequences:** Visitors see a language distribution that misrepresents Niraj's actual skill profile — potentially surfacing YAML or HTML above TypeScript or Python.

**Prevention:**
- Use `hide=html,css,yaml,json` query params to exclude non-programming languages.
- Use `langs_count=6` to limit output.
- Use `layout=compact` to keep it scannable.
- Example: `&hide=html,css,yaml,json,dockerfile&langs_count=6&layout=compact`

**Detection:** Inspect the default card output before embedding. Check whether the top results reflect actual coding languages or markup/config noise.

**Phase:** Affects stats card integration phase.

---

### Pitfall 9: Hardcoded Employer / Role Information Becomes Stale

**What goes wrong:** A job title, company name, or project description hardcoded in the README becomes outdated when roles change, making the profile look abandoned or inaccurate.

**Why it happens:** Profile READMEs have no dynamic content mechanism. Static text requires manual updates.

**Consequences:** Visitors see outdated information. Trust in the profile's accuracy decreases. For a senior engineer, a stale profile signals disengagement.

**Prevention:**
- Design content to be role-agnostic where possible (focus on domain expertise, not specific job title).
- Keep the employer/title line short and easy to update — a single line, not embedded in complex markup.
- Note in a comment (`<!-- Update: role/company -->`) the lines that need updating when roles change.

**Detection:** Audit current-status lines against the CV before publishing. Schedule a quarterly review.

**Phase:** Affects content authoring. Flag for ongoing maintenance.

---

## Minor Pitfalls

Common aesthetic or quality issues that reduce polish without breaking functionality.

---

### Pitfall 10: Mobile Rendering — Content Collapses to Unreadable Column

**What goes wrong:** Side-by-side stats cards arranged with HTML tables or `<img align="left">` tricks look fine on desktop but collapse to a single column on mobile, often with oversized cards that require horizontal scrolling.

**Why it happens:** GitHub's mobile web view has a narrow viewport. HTML table layouts do not reflow responsively. The mobile app notes "View all of README.md" and hides most content behind a tap.

**Prevention:**
- Keep stats cards stacked vertically (not side-by-side in tables) or use small enough cards that they work in a single column.
- Use `width` attributes on `<img>` tags to cap card width at ~495px — the width that fits a standard GitHub profile column.
- Test by narrowing the browser window to ~400px or by visiting on a mobile device.

**Detection:** Open the profile page on a phone or use browser DevTools responsive mode at 390px width.

**Phase:** Affects layout and visual design phase.

---

### Pitfall 11: Open Source Contribution Links Pointing to the Wrong Level

**What goes wrong:** Linking to a repository homepage (e.g., `github.com/langchain-ai/langchainjs`) instead of the specific merged PR leaves visitors unable to verify the contribution without digging through PR history.

**Why it happens:** It is easier to link to the repo than to find and bookmark individual PR URLs.

**Consequences:** The open source section looks vague and unverifiable — reducing its credibility as social proof.

**Prevention:**
- Link directly to each merged PR URL (e.g., `github.com/langchain-ai/langchainjs/pull/XXXX`).
- Include a brief one-line description of what each PR changed.
- For Niraj's profile: LangChain.js, LangChain Docs, react-chrono PRs should all link to their specific PR pages.

**Detection:** Click every link in the contributions section — each should land on a specific PR or commit.

**Phase:** Affects open source section authoring.

---

### Pitfall 12: Emoji Overuse Degrades Senior/Professional Signal

**What goes wrong:** An excessive number of emojis (every section header, every list item) reads as immature or template-generated. For a senior tech lead, this undercuts the "builder brand" tone.

**Why it happens:** Many GitHub profile README templates use emojis as visual separators because they render identically across platforms. Community examples normalize heavy emoji use.

**Prevention:**
- Limit emojis to 0-2 per major section as accent, not decoration.
- Avoid emojis in the bio or current role lines — these should read as plain professional text.
- Use horizontal rules (`---`) or bold headers for section separation instead.

**Detection:** Count total emojis. If the number exceeds roughly 10 for the entire README, audit for pruning.

**Phase:** Affects content tone review.

---

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|----------------|------------|
| Repo creation / scaffolding | Repo name case mismatch (Pitfall 1) | Confirm repo name is exactly `nkhunters` before any other work |
| Stats card integration | Public API rate limits (Pitfall 2) | Deploy private Vercel instance with PAT before embedding URLs |
| Stats card integration | Dark/light mode clash (Pitfall 3) | Use `<picture>` element or transparent background from the start |
| Tech stack badges | Dash encoding in badge URLs (Pitfall 4) | Validate each badge URL in browser individually |
| Tech stack badges | Badge overload (Pitfall 7) | Cap at ~5-6 badges per category, group by domain |
| Stats card integration | Language card noise (Pitfall 8) | Add `hide=` param before publishing |
| Layout / visual design | CSS stripping (Pitfall 6) | Never use `style=` — use deprecated `align=` only |
| Layout / visual design | Mobile collapse (Pitfall 10) | Test at 390px viewport width before publishing |
| Open source section | Vague repo links (Pitfall 11) | Direct PR URLs only |
| Content authoring | Stale role information (Pitfall 9) | One-line role format, easy to update |
| Content tone | Emoji overuse (Pitfall 12) | Audit total emoji count |

---

## Sources

- [Managing your profile README — GitHub Docs](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme)
- [anuraghazra/github-readme-stats — Issue: Stats not showing on first load](https://github.com/anuraghazra/github-readme-stats/issues/2446)
- [anuraghazra/github-readme-stats — Issue: Rate limits on public instance](https://github.com/anuraghazra/github-readme-stats/issues/4658)
- [anuraghazra/github-readme-stats — Discussion: Dark/light mode auto theme](https://github.com/anuraghazra/github-readme-stats/discussions/1684)
- [How to make your images adjust for dark/light mode — GitHub Blog](https://github.blog/developer-skills/github/how-to-make-your-images-in-markdown-on-github-adjust-for-dark-mode-and-light-mode/)
- [Shields.io — Static Badge URL encoding](https://shields.io/badges)
- [Bug: Unable to use hyphen in project name — badges/shields Issue #8043](https://github.com/badges/shields/issues/8043)
- [GitHub Flavored Markdown does not render CSS styles — Community Discussion #22728](https://github.com/orgs/community/discussions/22728)
- [Guide to aligning images in GitHub README — DavidWells Gist](https://gist.github.com/DavidWells/7d2e0e1bc78f4ac59a123ddf8b74932d)
- [README Badges Best Practices — daily.dev](https://daily.dev/blog/readme-badges-github-best-practices)
- [7 GitHub Profile Mistakes That Cost You Job Offers — Medium](https://medium.com/@kantmusk/7-github-profile-mistakes-that-cost-you-job-offers-e6b37ea92238)
- [Why does my GitHub profile README not display correctly? — Community Discussion #174117](https://github.com/orgs/community/discussions/174117)
- [PNG images broken in readme — Community Discussion #56416](https://github.com/orgs/community/discussions/56416)
- [Mobile README rendering — Community Discussion #177702](https://github.com/orgs/community/discussions/177702)
