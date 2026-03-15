# Zander Whitehurst — Extended Design Principles

Extended reference for the `zander-whitehurst` skill. The SKILL.md index covers the quick rules; this file holds the depth: system thinking, Memorisely patterns, and the rationale behind each principle.

---

## The Memorisely teaching approach

Memorisely teaches design through critique and iteration rather than rules. The method: show before/after, name the specific problem, apply one change at a time. The goal is training the eye to see what's wrong before knowing how to fix it.

This means design feedback should:

- **Name the problem first** — "the heading and body are the same visual weight" is more useful than "increase the font size."
- **Change one thing at a time** — multiple simultaneous changes make it impossible to know what worked.
- **Show the before/after** — without visual contrast, the improvement is invisible.
- **Anchor feedback to a principle** — "this violates hierarchy" is actionable; "this looks off" is not.

The Memorisely critique sequence: hierarchy → spacing → type → color → alignment. Fix in this order. A layout with broken hierarchy cannot be saved by good color choices — the hierarchy problem hides everything else.

---

## Spacing system

Establish a spacing scale based on a single base unit (4px is the most common). Every spacing value in the design must be a multiple of that unit:

```
4, 8, 12, 16, 24, 32, 48, 64, 96, 128
```

Never use arbitrary values (13px, 17px, 23px). Consistent spacing creates a visual rhythm that viewers feel even when they cannot name it.

**Rules for spacing application:**

- **Within a component** (between label and input, icon and text): 4–8px
- **Between related components** (form fields, list items): 12–16px
- **Between sections** (hero and features, heading and body): 48–96px
- **Section padding** (inside cards, panels): 24–32px

When in doubt, use more space than feels comfortable. Then add 25% more. Most amateur designs use half the spacing they need. Tight spacing signals anxiety; generous spacing signals confidence.

**Hierarchy through spacing:** Group related elements tightly. Separate unrelated elements with significantly more space. The ratio matters — 8px between related items and 48px between sections communicates structure. 8px and 16px communicates nothing.

---

## Typography patterns

Type is not decoration — it is structure. Get this right and the layout is already working.

**Type scale:** Use a ratio-based scale (1.25 "Major Third" or 1.333 "Perfect Fourth"). Avoid having more than 3–4 distinct sizes in active use at once. A typical set:

| Token | Size | Use |
|---|---|---|
| `text-xs` | 12px | Labels, captions, metadata |
| `text-sm` | 14px | Secondary body, supporting text |
| `text-base` | 16px | Primary body copy |
| `text-lg` | 20px | Subheadings, card titles |
| `text-xl` | 24px | Section headings |
| `text-2xl` | 32px | Page headings |
| `text-3xl` | 48px+ | Display / hero headings |

**Weight contrast:** Pair Regular (400) with Bold (700) or Semibold (600). Avoid Medium (500) as a primary contrast weight — it reads as indecisive. The contrast between 400 and 700 communicates hierarchy clearly. 400 and 500 does not.

**Line-height:**
- Body text: 1.5–1.6 (comfortable reading rhythm)
- Headings: 1.1–1.2 (tight, confident; never browser default on large text)
- Display text: 1.0–1.1 or even tighter

Never use browser default line-height on large headings. It creates visual awkwardness that viewers feel immediately.

**Letter-spacing:**
- Large display text (48px+): slightly tight (−0.02em to −0.04em)
- Body text: 0 or very slightly open (0 to +0.01em)
- All-caps labels: open tracking (+0.05em to +0.1em)

**Common typography mistakes:**
- More than 4 type sizes in active use → consolidate
- All text the same weight → add weight contrast at heading level
- Headings with 1.4+ line-height → tighten to 1.1–1.2
- Small text at 400 weight on light background → switch to medium or increase size
- Too many font families (anything over 2) → reduce to one family, vary weight instead

---

## Color application

Color is the most overused tool in amateur design. Use less.

**The 60-30-10 rule:**
- 60% neutral background (white, near-white, light gray)
- 30% secondary surfaces and text (mid-grays, dark text)
- 10% accent color (brand color, CTAs, key highlights only)

This ratio applies at page level and at component level. If a card is 60% white, 30% gray text, and 10% blue button — that is a coherent card.

**Specific rules:**

- **Never full black (#000) for text** — use a very dark neutral (#111, #1a1a2e, #0f0f0f). Pure black text on white creates too much contrast and feels harsh.
- **Never pure white (#fff) backgrounds without relief** — use #fafafa, #f8f9fa, or #f5f5f5. Pure white feels clinical and harsh under harsh lighting.
- **Limit brand color to interactive elements and key highlights** — not decorative borders, not section backgrounds, not hover states on non-interactive elements.
- **Build a 9-step neutral scale** (50 through 900) and stick to it. Ad hoc grays (#ddd, #ccc, #bbb) cause visual drift and inconsistency. Define: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900.

**When a design uses too many colors:**
1. Identify how many hues are in use
2. Strip everything to neutral + one accent
3. Only reintroduce a second hue if it serves a semantic purpose (error red, success green)
4. Decorative color is almost never justified

**Dark mode:** Don't use #000 backgrounds — use #0f0f0f, #111, or #1a1a1a. Text on dark: #e5e5e5 or #f0f0f0 rather than white. Pure black backgrounds with pure white text are as harsh as pure white with pure black.

---

## Layout and composition

**Alignment:** Left-align body content almost always. Centered text works for: short display headings (1–3 words), CTAs in isolation, hero taglines. Centered body paragraphs reduce readability and feel amateur.

**Max-width for readability:**
- Body content: 65–75ch (characters). Beyond 75ch, line length becomes tiring to read.
- Dashboard / data-heavy layouts: wider (up to 1280px container)
- Marketing sections: 720–800px for text blocks within wider containers

**Grid system:**
- Use a 12-column grid for complex layouts (dashboards, multi-column content)
- 4-column for mobile
- Never invent ad hoc column counts — always derive from the 12-column base
- Column gaps: 16px (compact) or 24px (comfortable) or 32px (spacious)

**Cards:**
- Consistent internal padding: 24px or 32px (not both — pick one per design system)
- Consistent border-radius: pick one radius value and apply it everywhere
- Border or shadow for elevation — not both. A subtle border (1px, neutral-200) or a soft shadow (0 1px 3px rgba(0,0,0,0.08)) — not both simultaneously.
- Card backgrounds: slightly lighter or darker than page background, not pure white on pure white

**Visual weight distribution:**
- Every layout should be readable if you squint at it from a distance. If it turns into a uniform gray blob when squinted — there is no hierarchy.
- The dominant element (headline, hero image, primary CTA) should be identifiable at a glance.

---

## Before/after: common Memorisely critique patterns

| Design problem | Root cause | Fix |
|---|---|---|
| Button looks weak | Same font weight as body text | Increase to Bold; add more horizontal padding (1.5–2× current) |
| Page looks flat | All elements same visual weight | Increase heading size by at least 2 scale steps above current |
| Content feels cramped | Section padding too tight | Double the vertical padding between sections |
| Sidebar feels disconnected from main content | No shared spacing unit | Apply the same base spacing unit to both columns |
| Form looks intimidating | Input fields too close together | 16px gap between fields; 24px gap before submit button |
| Colors clash | Too many hues in use | Strip to neutral + one accent; reintroduce second hue only if semantic |
| Hero section feels generic | Stock photography + no type hierarchy | Strong typographic statement reduces dependency on image quality |
| CTA is buried | Same visual weight as surrounding text | Increase padding, weight, and optionally background fill |
| Navigation feels heavy | Too many links at same weight | Limit primary nav to 5–7 items; visually separate active state |
| Section feels disconnected from next | No visual transition or shared spacing | Use consistent vertical rhythm (multiples of base unit) between sections |

---

## The critique loop

When reviewing a design, assess and fix in this order:

**1. Hierarchy** — Can you identify the single most important element? If not, establish one. Size, weight, and position are the primary tools.

**2. Spacing** — Does the layout breathe? Are related items grouped with tight spacing and unrelated items separated with generous spacing? Is the ratio large enough to communicate structure?

**3. Typography** — Does the type scale communicate content structure? Is weight contrast clear between heading and body? Is line-height comfortable for body and tight for headings?

**4. Color** — Is color earning its place? Could any color be removed or replaced with a neutral without losing meaning? Is the accent color being used sparingly?

**5. Alignment** — Does everything align to an implied grid? Are there any elements positioned "approximately" rather than exactly?

This sequence mirrors the order in which a viewer's eye processes a design. A viewer notices hierarchy immediately (or its absence). Spacing is felt next. Type reads next. Color is noticed after structure. Misalignment is noticed last — but it creates unease throughout.

Fix in this order. There is no benefit to refining color when hierarchy is broken. There is no benefit to tightening alignment when spacing makes the layout breathe poorly.

---

## Quick-start: applying this skill

When asked to critique or improve a design:

1. Run the critique loop (hierarchy → spacing → type → color → alignment)
2. Name problems before suggesting fixes — "the heading has no visual dominance" before "increase to 48px"
3. Suggest one change at a time
4. Bias toward removal — every suggestion to add must be weighed against a suggestion to remove
5. Ground all feedback in one of the 7 core principles from SKILL.md

When generating design specs or guidelines:

1. Start with type scale and spacing system — the rest builds on these
2. Define the neutral scale and one accent before any secondary colors
3. Establish max-widths and grid structure before component-level details
4. Every value should be derivable from the system — no arbitrary numbers
