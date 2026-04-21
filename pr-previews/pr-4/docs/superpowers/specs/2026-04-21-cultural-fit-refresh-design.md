# Cultural-Fit Refresh: Remove Rate, Add "Beyond the Code"

**Date:** 2026-04-21
**Status:** Draft
**Scope:** `index.html`, `css/landing.css`

## Goal

Rework the landing page so it still signals availability for contract work and engineering competence, but:

1. Removes the public hourly rate.
2. Adds a humanizing "Beyond the Code" section that supports cultural-fit conversations with potential hiring teams.

The personal content is weighted ~70% personality/interests, with lighter touches of working style and engineering philosophy.

## Change Summary

| Location | Change |
| --- | --- |
| Hero eyebrow | Prepend location: `Based in Durango, CO · Available for fractional roles and projects` |
| Nav bar | Add `About` link between `Work` and `Contact` |
| `#availability` section | **Remove entirely** (heading, availability paragraph, rates block) |
| New `#about` section | **Add** in the slot vacated by `#availability`, between Experience and Contact |
| `css/landing.css` | Add rules for `.about-grid`, `.chips`, `.chip`, `.value-card`, `.value-card h4` |

No changes to: Technical Expertise, How I Can Help, Selected Work & Results, Contact, Footer, JS, images, fonts, dependencies.

## Page Structure After Change

```
Hero (eyebrow updated)
├── Technical Expertise     [unchanged]
├── How I Can Help          [unchanged]
├── Selected Work & Results [unchanged]
├── Beyond the Code         [NEW — replaces Availability]
└── Contact                 [unchanged]
```

## Detailed Changes

### 1. Hero eyebrow (`index.html`)

Current:
```html
<p class="eyebrow">Available for fractional roles and projects</p>
```

New:
```html
<p class="eyebrow">Based in Durango, CO · Available for fractional roles and projects</p>
```

### 2. Nav bar (`index.html`)

Add a link between `Work` and `Contact`:

```html
<a class="nav-link" href="#about">About</a>
```

### 3. Remove Availability section (`index.html`)

Delete the entire `<section class="section" id="availability">...</section>` block, including the `.rates` div that contains the hourly rate.

### 4. Add "Beyond the Code" section (`index.html`)

Insert in the slot previously occupied by the Availability section:

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

### 5. CSS additions (`css/landing.css`)

Append to the file, following existing formatting conventions (4-space indent, variable-driven colors):

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

Existing classes (`.section`, `.section-header`) remain authoritative for outer spacing and heading styles — the new rules only handle content inside the section.

## Copy Decisions

- `Renaissance man` tagline is deliberately first-person and slightly self-aware. It matches the user's voice.
- `When I'm not shipping code...` chooses action verbs over a static list, which reads as a person rather than a resume.
- `Low ego, high curiosity` is terse by design. It's the line a hiring manager will remember.
- The optimization belief is a paraphrase of the user's exact words ("certain patterns will express themselves when the time for optimization is right"), tightened for the page.

## What Stays Off the Page

Per the user: no mention of family. The page will not reference partners, kids, or pets even though the domain handle (`thespaniard514`) may hint at one.

## Success Criteria

- No hourly rate appears anywhere in rendered HTML.
- The new section is reachable from the nav.
- Section renders correctly at desktop (>= 1024px), tablet (~720px), and mobile (< 720px) widths.
- Fonts, colors, and spacing visually match the rest of the page (no off-theme styling).
- All existing sections unchanged.

## Out of Scope

- Rewriting existing sections (Technical Expertise, How I Can Help, Selected Work).
- Adding new assets (photos, icons beyond Font Awesome already loaded).
- SEO meta updates beyond what's already in place.
- Contact form or any JS behavior.
- Accessibility audit beyond reusing the existing pattern (semantic section + heading).
