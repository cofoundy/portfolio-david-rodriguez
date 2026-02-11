# QA Report: David Rodriguez Taboada

**Date:** 2026-02-11
**URL:** https://cofoundy.github.io/portfolio-david-rodriguez/
**Status:** FAIL

## Data Validation
- [x] Name matches source
- [ ] Email discrepancy (see Issue #4)
- [x] Job title consistent with CV
- [x] Companies listed are from CV
- [x] Education institutions are from CV
- [x] Dates are from source
- [x] No hallucinated data

## Clean Deploy
- [x] No "Powered by" / "Made with" / "Built with"
- [x] No "View source" / "View on GitHub" / "Fork this"
- [x] No "Lorem ipsum" / placeholder text
- [x] No "undefined" or "null" visible
- [ ] Template artifact: programming symbols in hero background (see Issue #5)
- [ ] Escaped HTML visible to user (see Issue #1)

## Technical
- [x] Page loads (HTTP 200)
- [x] CSS loads (HTTP 200)
- [x] Favicon loads (HTTP 200)
- [x] astro.config.mjs has both site + base
- [ ] Console errors: could not verify (Chrome MCP unavailable)

## Issues Found

### CRITICAL

**Issue #1: Escaped HTML tags visible in Education degree**
The first education entry renders literal `<strong>Maestria</strong>` as visible text instead of bold formatting. Root cause: Education.astro line 28 uses `{edu.degree}` which escapes HTML. Config.ts line 100 contains `<strong>Maestria</strong>` in the degree field.
Fix: Change line 28 of Education.astro from `{edu.degree}` to `<Fragment set:html={edu.degree} />`.

**Issue #2: Footer renders dead social icons (LinkedIn, Twitter, GitHub)**
The footer unconditionally renders LinkedIn, Twitter, and GitHub icons even though config.ts only defines `email` in the social object. These icons have no `href` attribute, creating dead/broken link targets. Hero.astro correctly uses `siteConfig.social?.linkedin &&` guards, but Footer.astro does not.
Fix: Wrap each social link in Footer.astro with conditional guards matching the Hero pattern.

**Issue #3: No mobile navigation**
Header.astro uses `hidden md:block` which completely hides navigation on screens below 768px. There is no hamburger menu or any alternative mobile navigation. Mobile users can only scroll to find sections.
Fix: Add a hamburger menu toggle for mobile viewports.

### MODERATE

**Issue #4: Email mismatch between Sheet and config**
Google Sheet row lists `riatoviceirl@gmail.com` (company email matching RIATOVIC EIRL). Config.ts uses `david.rt85@gmail.com` (personal email). This may be intentional but should be verified with the client.
Action: Confirm with client which email they want displayed.

**Issue #5: Programming symbols in Hero background**
The hero section has a decorative SVG pattern with code symbols (`</>`, `=>`, `[]`, `()`, `::`, `==`, `++`, `;`). David is a Mining Engineer and Consultant, not a software developer. These symbols are a leftover developer-oriented template artifact.
Fix: Remove the `programming-symbols` pattern from Hero.astro or replace with neutral/mining-themed decorative elements.

### MINOR

**Issue #6: Missing Spanish accent marks in section headings**
"Educacion" should be "Educacion" (tilde on the o). "Sobre Mi" should be "Sobre Mi" (tilde on the i). These are misspellings in the component templates.
Fix: Update heading text in Education.astro, Header.astro, Footer.astro, and About.astro.

**Issue #7: No profile photo**
No profile image on the site. Client did not submit a photo in the Google Form. Acceptable for tier but worth noting.

## Evidence
- Screenshots could not be taken (Chrome MCP server unavailable)
- All findings verified via curl HTML inspection and source code review
