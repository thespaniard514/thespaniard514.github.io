# Cultural-Fit Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove the public hourly rate from the landing page and add a "Beyond the Code" section that supports cultural-fit conversations with hiring teams.

**Architecture:** Edit the single-page static site (`index.html` + `css/landing.css`). The existing Availability section is replaced by a new "Beyond the Code" section in the same slot. Hero eyebrow gains a location, nav gains an "About" link. No JS, no new assets, no framework.

**Tech Stack:** Static HTML/CSS with CSS custom properties (`--accent`, `--text`, `--muted`, `--border`, `--bg-soft`) defined in `css/landing.css`. Inter font via Google Fonts. Font Awesome icons (already loaded). Served by GitHub Pages.

**Spec:** [docs/superpowers/specs/2026-04-21-cultural-fit-refresh-design.md](../specs/2026-04-21-cultural-fit-refresh-design.md)

**Verification approach:** No unit tests exist for this static site. Verification uses the Playwright MCP browser tools to load the rendered page from disk and assert the key success criteria (rate removed, new section present, nav wired, layout correct at two viewports). Each task ends with a commit.

---

## File Structure

| File | Change | Responsibility |
| --- | --- | --- |
| `css/landing.css` | Append new rules | Styles for `.about-grid`, `.chips`, `.chip`, `.value-card`, `.value-card h4`, mobile stacking |
| `index.html` | Modify hero eyebrow, nav, and main content region | Remove `#availability`; add `#about` section; add nav link; update eyebrow |

No new files. No deletions (existing sections other than `#availability` untouched).

## Commit Strategy

Three commits in this order:

1. **CSS first.** Safe to land alone — no HTML references the new classes yet, so nothing changes visually.
2. **HTML next.** Single atomic change: eyebrow + nav + section swap. Once the CSS is in place, the new markup renders styled correctly.
3. **Verification evidence.** Screenshots captured, no file changes — no commit. (Or optional commit if screenshots are saved to the repo; default: don't save.)

---

## Task 1: Add CSS rules for the new section

**Files:**
- Modify: `css/landing.css` (append to end of file)

- [ ] **Step 1: Open the file and locate the append point**

Run: `wc -l css/landing.css`
Note the final line number. Append the new rules after the last existing rule. Do not modify any existing rule.

- [ ] **Step 2: Append the new CSS rules**

Append this block to the end of `css/landing.css`, preserving existing 4-space indentation and variable-based coloring:

```css

.about-grid {
    max-width: 1100px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1.2fr 1fr;
    gap: 40px;
    align-items: start;
}

.about-narrative p {
    color: var(--text);
    margin: 0 0 20px;
}

.chips {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

.chip {
    background: var(--bg-soft);
    border: 1px solid var(--border);
    border-radius: 999px;
    padding: 6px 14px;
    font-size: 14px;
    color: var(--muted);
    font-weight: 500;
}

.about-values {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.value-card {
    background: var(--bg-soft);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 24px;
}

.value-card h4 {
    margin: 0 0 10px;
    font-size: 13px;
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 0.8px;
    font-weight: 600;
}

.value-card p {
    margin: 0;
    color: var(--text);
}

@media (max-width: 720px) {
    .about-grid {
        grid-template-columns: 1fr;
        gap: 28px;
    }
}
```

Note the leading blank line — it separates this block from the existing last rule. The `@media (max-width: 720px)` breakpoint matches the existing responsive breakpoint at line 350 of this file, keeping behavior consistent.

- [ ] **Step 3: Verify the file parses (no broken braces)**

Run: `node -e "const fs=require('fs'); const css=fs.readFileSync('css/landing.css','utf8'); const open=(css.match(/\{/g)||[]).length; const close=(css.match(/\}/g)||[]).length; console.log('open:', open, 'close:', close); if (open!==close) process.exit(1);"`

Expected output: `open: N close: N` with matching counts. Non-zero exit means unbalanced braces — re-check the paste.

- [ ] **Step 4: Commit**

```bash
git add css/landing.css
git commit -m "$(cat <<'EOF'
style: add CSS rules for Beyond the Code section

Introduces .about-grid, .chips, .chip, .value-card, and a 720px
breakpoint matching the existing responsive breakpoint. No HTML
references these classes yet; landing on CSS first keeps the HTML
change in its own atomic commit.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 2: Update HTML — eyebrow, nav, section swap

**Files:**
- Modify: `index.html` (hero eyebrow line, nav-actions block, `#availability` section → `#about` section)

- [ ] **Step 1: Update the hero eyebrow**

Locate in `index.html`:

```html
<p class="eyebrow">Available for fractional roles and projects</p>
```

Replace with:

```html
<p class="eyebrow">Based in Durango, CO · Available for fractional roles and projects</p>
```

Note: the separator is a middle-dot character (U+00B7, `·`), not a hyphen or pipe. Paste the exact character as shown.

- [ ] **Step 2: Add "About" link to the nav**

Locate in `index.html`:

```html
<a class="nav-link" href="#services">Services</a>
<a class="nav-link" href="#experience">Work</a>
<a class="nav-link" href="#contact">Contact</a>
```

Replace with (adds an `About` link between Work and Contact):

```html
<a class="nav-link" href="#services">Services</a>
<a class="nav-link" href="#experience">Work</a>
<a class="nav-link" href="#about">About</a>
<a class="nav-link" href="#contact">Contact</a>
```

- [ ] **Step 3: Remove the Availability section entirely**

Locate in `index.html` — delete this entire block including the opening `<section>` and closing `</section>`:

```html
<section class="section" id="availability">
    <div class="section-header">
        <h2>Availability &amp; Engagements</h2>
        <p>Flexible options to match your goals and timelines.</p>
    </div>
    <div class="availability">
        <p>I'm available immediately for fractional roles (10–20 hours/week), project-based backend or
            full-stack development, API design, and architecture consulting.</p>
        <div class="rates">
            <div><strong>Rates:</strong> $120–$150/hr depending on scope.</div>
            <div>Project-based pricing available.</div>
        </div>
    </div>
</section>
```

This removes the only occurrence of the rate string from the rendered HTML.

- [ ] **Step 4: Insert the new "Beyond the Code" section in the same slot**

In the exact position vacated by Step 3 (between the `#experience` section and the `#contact` section), paste:

```html
<section class="section" id="about">
    <div class="section-header">
        <h2>Beyond the Code</h2>
        <p>A renaissance man based in Durango, Colorado.</p>
    </div>
    <div class="about-grid">
        <div class="about-narrative">
            <p>When I'm not shipping code, you'll find me in the mountains around Durango — mountain biking, skiing, or snowshoeing — or at home playing guitar, cooking, tinkering with 3D prints, or teaching myself to sew. I like being a jack of all trades; "renaissance man" is a compliment I'll happily accept.</p>
            <div class="chips">
                <span class="chip">Guitar</span>
                <span class="chip">Singing</span>
                <span class="chip">Mountain biking</span>
                <span class="chip">Skiing</span>
                <span class="chip">Snowshoeing</span>
                <span class="chip">Golf</span>
                <span class="chip">Cooking</span>
                <span class="chip">3D printing</span>
                <span class="chip">Sewing</span>
            </div>
        </div>
        <div class="about-values">
            <div class="value-card">
                <h4>How I work</h4>
                <p>I build connection and trust before consensus. Low ego, high curiosity — I'd rather learn your codebase's patterns than impose mine.</p>
            </div>
            <div class="value-card">
                <h4>What I believe</h4>
                <p>There are many right ways to program something. The right optimization reveals itself when the time comes — until then, write code people can understand.</p>
            </div>
        </div>
    </div>
</section>
```

Notes on special characters:
- The em-dashes in the narrative (`Durango —`, `curiosity —`, `time comes —`) are U+2014 (`—`), not two hyphens.
- `"renaissance man"` uses straight double quotes (U+0022), matching the rest of the file.
- The range `10–20 hours/week` used to appear in Availability — it is no longer needed because the section is removed.

- [ ] **Step 5: Sanity-check the file was edited correctly**

Run these greps — each should produce the expected output:

```bash
grep -c 'id="availability"' index.html
# Expected: 0

grep -c 'id="about"' index.html
# Expected: 1

grep -c '\$120' index.html
# Expected: 0

grep -c 'Beyond the Code' index.html
# Expected: 1

grep -c 'href="#about"' index.html
# Expected: 1

grep -c 'Based in Durango' index.html
# Expected: 1
```

If any count is off, re-open the file and reconcile before committing.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
feat: replace Availability (with rate) with Beyond the Code section

- Remove public hourly rate from the rendered page
- Add a personal "Beyond the Code" section covering interests,
  working style, and engineering philosophy for cultural-fit signal
- Add "Based in Durango, CO" to the hero eyebrow
- Add "About" link to the nav pointing at the new section

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## Task 3: Visual verification via Playwright MCP

**Files:** None modified. This task produces evidence, not code.

The Playwright MCP browser tools are available in this environment. Use them to load the page from disk and confirm the success criteria visually and programmatically.

- [ ] **Step 1: Resolve the absolute path for the file:// URL**

Run: `pwd && echo "file://$(pwd)/index.html"`

Expected output ends with `/index.html`. Copy the `file://...` URL for the next step.

- [ ] **Step 2: Navigate Playwright to the page at desktop width**

Use the Playwright MCP browser tools (schemas are available via ToolSearch with `select:mcp__plugin_playwright_playwright__browser_navigate,mcp__plugin_playwright_playwright__browser_resize,mcp__plugin_playwright_playwright__browser_take_screenshot,mcp__plugin_playwright_playwright__browser_evaluate,mcp__plugin_playwright_playwright__browser_close`):

1. `browser_resize` → width 1280, height 900
2. `browser_navigate` → the `file://.../index.html` URL from Step 1
3. `browser_take_screenshot` → saves a reference screenshot

- [ ] **Step 3: Assert success criteria programmatically**

Use `browser_evaluate` with this function body:

```javascript
() => {
  const html = document.documentElement.outerHTML;
  return {
    rateGone: !/\$120/.test(html) && !/\$150/.test(html),
    aboutSectionPresent: !!document.getElementById('about'),
    availabilitySectionGone: !document.getElementById('availability'),
    navAboutLinkPresent: !!document.querySelector('a.nav-link[href="#about"]'),
    durangoInEyebrow: (document.querySelector('.eyebrow')?.textContent || '').includes('Durango'),
    chipCount: document.querySelectorAll('.chip').length,
    valueCardCount: document.querySelectorAll('.value-card').length,
  };
}
```

Expected result object:

```json
{
  "rateGone": true,
  "aboutSectionPresent": true,
  "availabilitySectionGone": true,
  "navAboutLinkPresent": true,
  "durangoInEyebrow": true,
  "chipCount": 9,
  "valueCardCount": 2
}
```

Any `false` or mismatched count means a regression — stop and fix before proceeding.

- [ ] **Step 4: Verify mobile layout**

1. `browser_resize` → width 375, height 800
2. `browser_take_screenshot` → mobile screenshot
3. `browser_evaluate` with:

```javascript
() => {
  const grid = document.querySelector('.about-grid');
  const style = grid ? getComputedStyle(grid) : null;
  return {
    gridTemplateColumns: style?.gridTemplateColumns,
  };
}
```

Expected: `gridTemplateColumns` is a single track (one value, not two). On a 375px viewport inside a 1fr grid, it typically returns something like `"339px"` — the point is that there is exactly one column, confirming the `@media (max-width: 720px)` rule fired.

- [ ] **Step 5: Scroll through the page at desktop width and eyeball it**

1. `browser_resize` → width 1280, height 900
2. `browser_navigate` → the `file://.../index.html` URL again (fresh load)
3. `browser_take_screenshot` with `fullPage: true` → capture the whole page top-to-bottom

Review the full-page screenshot for:
- Hero eyebrow shows `Based in Durango, CO · Available for fractional roles and projects`
- Nav shows `Services`, `Work`, `About`, `Contact` in that order, followed by the social icons and `Hire Me` button
- The new "Beyond the Code" section sits between "Selected Work & Results" and "Let's Build Something Great"
- Chips render as pills with the soft-bg color, wrapping naturally
- Value cards have accent-colored uppercase labels (`HOW I WORK`, `WHAT I BELIEVE`)
- No `$120–$150/hr` or `Availability & Engagements` anywhere on the page

- [ ] **Step 6: Close the browser**

`browser_close` → releases the page.

- [ ] **Step 7: Report results to the user**

Summarize in plain English: screenshots taken, assertion result object, any visual notes. No commit needed for this task unless the user asks to save screenshots.

---

## Self-Review Checklist (for the plan author)

**Spec coverage:**
- ✅ Hero eyebrow location prefix → Task 2 Step 1
- ✅ Nav `About` link → Task 2 Step 2
- ✅ Remove `#availability` section including rate → Task 2 Step 3, verified in Task 3 Step 3
- ✅ Add `#about` section with narrative, chips, two value cards → Task 2 Step 4
- ✅ CSS for `.about-grid`, `.chips`, `.chip`, `.value-card`, mobile breakpoint → Task 1 Step 2
- ✅ Final copy (narrative, How I work, What I believe) → Task 2 Step 4, matches spec verbatim
- ✅ 720px responsive breakpoint matches existing file → Task 1 Step 2 note
- ✅ Files changed are only `index.html` and `css/landing.css` → Task 1 and Task 2
- ✅ No family references → confirmed, none of the copy touches family
- ✅ Success criteria (rate gone, section reachable, responsive, theme-consistent, existing sections unchanged) → Task 3 asserts rate gone, section present, nav wired, mobile single-column; remaining criteria covered by the full-page eyeball review in Task 3 Step 5

**Placeholder scan:** No TBDs, no "add appropriate error handling," no "similar to Task N" references. Every step has the exact content to paste.

**Type/name consistency:** Section id `about` matches the nav link `href="#about"` matches the CSS-targeted IDs in verification. Class names `about-grid`, `about-narrative`, `about-values`, `chips`, `chip`, `value-card` used in HTML (Task 2) match exactly what CSS defines (Task 1).
