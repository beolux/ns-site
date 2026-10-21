# CLAUDE.md — beolux/ns-site (nickseverson.com)

Read this before every task. If a task conflicts with it, stop and ask Nick.

## Stack
- Plain static site: hand-written HTML pages plus one shared `style.css`. No framework, no bundler, no package.json, no build step, no tests, no CI.
- Pages at the repo root: `index.html`, `calculator.html`, `glossary.html`, `open-enrollment.html`, `contact.html`, `contact-success.html`, `family.html`. There is also `Nick-Severson-Resume.pdf`.
- JavaScript is inline `<script>` in each page: the scroll-reveal observer, the mobile menu toggle, and the calculator logic in `calculator.html`. There are no external JS files.
- Fonts come from Google Fonts (Bricolage Grotesque, Newsreader, Spline Sans Mono). The favicon is an inline SVG data URI.
- Hosting: GitHub Pages, deployed from the root of `main`. `CNAME` = `nickseverson.com`, and `.nojekyll` is present. **Merging to `main` deploys to production.**
- Note: `README.md` is out of date. It still says "single index.html (Tailwind via CDN)". The site no longer uses Tailwind. Don't add Tailwind back.

## Preview and checks
- Preview locally: `python3 -m http.server 8000` from the repo root, then open http://localhost:8000/.
- There are no automated checks. Before you open a PR:
  - Load every page you touched at desktop width and at about 375px wide. Check the mobile menu, the side padding, and that nothing scrolls sideways.
  - Make sure the browser console shows no errors.
  - Click through the internal links (nav and footer) on every page you touched.
  - For calculator changes, try a few inputs by hand and put the results in the PR description.
- Don't submit the live contact form while testing. It sends real email to Nick.

## Conventions
- **Header, nav, and footer are copied into every page.** There are no includes. If you change one, change it the same way in every page that has it (`index`, `calculator`, `glossary`, `open-enrollment`, `contact`, `family`). `contact-success.html` has no header or nav, on purpose.
- Main nav order: Focus · Experience · Writing · Calculator · Glossary · Open enrollment · Resume · Get in touch (CTA). On `index.html` the section links are `#focus` etc. and the CTA is `#contact`. On subpages they are `index.html#focus` etc. and the CTA is `contact.html`.
- Footer soft links (`.footer-links`): Calculator · Glossary · Open enrollment.
- **The Family Portal is out of public navigation.** No page links to `family.html`, and it has `<meta name="robots" content="noindex">`. Keep it that way, and don't add links to it.
- Styling: use the CSS variables in `:root` of `style.css` (`--paper`, `--ink`, `--pine`, `--brass`, `--pad`, `--max`, etc.). Reuse existing classes (`.wrap`, `.card`, `.btn btn-primary`/`btn-ghost`, `.reveal`, `.calc-*`, `.glossary-*`, `.check-*`) before writing new ones. Keep inline styles to a minimum.
- **Mobile/iOS padding:** `.wrap` sets left/right padding as `max(var(--pad), env(safe-area-inset-*))`. Never zero out or override `.wrap` horizontal padding. Past bugs came from section rules that clobbered it (see commits 71ad314, 7213f56, c8e4992). The breakpoint is `@media (max-width:720px)`.
- Accessibility: keep `aria-label`/`aria-expanded` on the nav, keep `:focus-visible` outlines, and keep honoring `prefers-reduced-motion` (the reveal script already does).
- External links use `target="_blank" rel="noopener"`.
- The benefits tools (calculator, glossary, OE checklist) each end with a `.calc-disclaimer` block ("Educational … only. Not insurance, tax, or legal advice …"). Keep those disclaimers word for word. New tools of this kind need the same style of disclaimer.

## Do not touch (ask Nick first)
- **Playbook section: PARKED.** It is James P. Bennett & Company IP. Don't add, restore, or expand any playbook content or pages, and no JPB material at all. (The LinkedIn post card titled "...employer playbook" in the Writing section of `index.html` is an external link to Nick's own post. Leave it as it is.)
- **`family.html`:** don't edit it without asking.
- **Contact form / FormSubmit (`contact.html`):** don't change the hashed `formsubmit.co` action URL or the hardening fields (`_captcha=true`, the `_honey` honeypot, `_next` → `https://nickseverson.com/contact-success.html`, `_subject`, `_template`). Don't swap in a different form provider.
- `CNAME`, `.nojekyll`, and `Nick-Severson-Resume.pdf`: don't touch these unless the task says to.
- Stats and claims in `index.html` (for example the savings figures) are Nick's own numbers. Don't change them.

## Hard rules
- **Never push to `main`**, never force-push, and never merge. Work only on a `claude/<issue#>-<slug>` branch and open a PR that references the issue. Only Nick merges.
- **Never commit secrets**: no keys, tokens, passwords, `.env` files, or credentials of any kind, in code, comments, or commit messages.
- Keep copy changes small and limited to what the issue asks for. Don't invent benefits, plan, tax, or legal claims, statistics, testimonials, or client names. If the copy needs new facts, leave a `TODO` and flag it in the PR.
- Don't add dependencies, frameworks, build tools, analytics, trackers, or third-party scripts unless the issue asks for them.
- Keep Claude Code's permission prompts on.
