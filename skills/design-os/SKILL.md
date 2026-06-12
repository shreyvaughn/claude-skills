---
name: design-os
description: Build clean, production-grade UI with shadcn/ui and Tailwind, using design-system thinking to avoid generic AI-slop layouts.
---

# Design OS

A capability skill for building interfaces that look intentional, not generated. The default stack is shadcn/ui on top of Tailwind CSS, but the thinking here is framework-agnostic. The goal is production-grade UI: consistent, accessible, responsive, and free of the tells that mark a layout as machine-made.

## When to use this

Use this whenever you are asked to build, restyle, or review a web interface: a page, a component, a dashboard, a marketing section, a form. Reach for it before writing any markup. The point is to decide on a system first, then fill it in, rather than styling element by element.

## Core principle: tokens before pixels

Never reach for an arbitrary value when a scale value exists. Every spacing, size, color, and radius decision should come from a small, named set. This is what separates a designed interface from a styled one. If you find yourself typing `mt-[13px]` or `#3a7bd5`, stop and pick the nearest token.

### Spacing scale

Use a 4px base step. Tailwind's default scale already encodes this: `1` = 4px, `2` = 8px, `4` = 16px, `6` = 24px, `8` = 32px, `12` = 48px, `16` = 64px. Pick a small working set per project (for example 2, 4, 6, 8, 12, 16, 24) and stick to it. Consistent rhythm comes from reusing the same handful of gaps everywhere, not from fine-tuning each one.

### Type scale

Pick a modular scale and assign roles, do not size text ad hoc.

- Display / hero: `text-4xl` to `text-6xl`, tight tracking, tight leading
- Section heading: `text-2xl` to `text-3xl`
- Card / subheading: `text-lg` to `text-xl`
- Body: `text-base` (16px), leading-relaxed for long form
- Secondary / meta: `text-sm`, muted color
- Caption / label: `text-xs`, often uppercase with wider tracking

Use weight and color to build hierarchy before you reach for size. Two sizes plus a weight contrast often reads cleaner than four sizes.

### Color tokens

Define semantic tokens, not raw hexes scattered through markup. shadcn/ui ships a sensible set as CSS variables: `background`, `foreground`, `card`, `popover`, `primary`, `secondary`, `muted`, `accent`, `destructive`, `border`, `input`, `ring`. Style against these names so dark mode and rebrands are one-file changes.

- Pick ONE primary brand color and one neutral ramp. Resist adding a second accent until a real need appears.
- Most of a good UI is neutrals. Color is for emphasis and state, not decoration.
- Keep a clear `muted-foreground` for secondary text so hierarchy survives without extra size.

### Radius and elevation

- Choose one base radius (commonly `0.5rem`) and derive the rest. Mixing sharp and very round corners in one view looks accidental.
- Treat elevation as a small set of shadow tiers (flat, raised, overlay). Reserve heavy shadows for things that actually float (menus, dialogs, toasts). Cards usually want a subtle border or a faint shadow, not both at full strength.

## Component composition

shadcn/ui components are copied into your repo, not imported from a black box, so compose and extend them directly.

- Build screens from small primitives (`Button`, `Card`, `Input`, `Badge`, `Dialog`) rather than bespoke one-off blocks.
- Keep a `cn()` helper (clsx + tailwind-merge) so variant classes merge predictably and last-write-wins on conflicts.
- Express component variants with `cva` (class-variance-authority): define `variant` and `size` once, then use them. This keeps a button library consistent instead of 12 slightly different buttons.
- Compose, do not fork. Wrap a primitive to add behavior; avoid copying its internals into a new component.

## Layout patterns that read as designed

- Lead with a layout primitive: a max-width container (`max-w-5xl mx-auto px-4`), then a stack or grid inside. Decide the page skeleton before any content.
- Prefer `flex` and `grid` with `gap-*` over margins between siblings. Gap-based spacing stays consistent when items wrap or reorder.
- Give content room. Generous whitespace and a constrained measure (around 60 to 75 characters per line for body text via `max-w-prose`) signal care.
- Align to a grid. Things that line up vertically and share a left edge feel composed.
- Establish one clear focal point per view. Not everything can be bold, large, and colored at once.

## Accessibility (non-negotiable)

- Use semantic elements: real `button`, `a`, `nav`, `main`, `label`. Do not paint a `div` to look clickable.
- Every interactive element needs a visible focus state. Keep (or restyle) `focus-visible:ring-*`; never strip focus outlines.
- Associate every input with a `label` (wrap or `htmlFor`). Add `aria-*` only where semantics fall short.
- Maintain WCAG AA contrast: 4.5:1 for body text, 3:1 for large text and meaningful UI borders. Muted gray-on-white is a common failure.
- Respect `prefers-reduced-motion`: gate non-essential animation behind it.
- Make sure the whole flow works by keyboard: tab order is logical, dialogs trap and restore focus, Escape closes overlays.

## Responsive patterns

- Design mobile-first: write the base styles for small screens, then layer `sm:`, `md:`, `lg:` overrides. Do not design desktop-down.
- Reflow, do not just shrink. A three-column grid should become one column on mobile, not three squished columns.
- Use fluid type sparingly (`clamp()`) for hero text; keep body text at a fixed comfortable size.
- Make tap targets at least 44px on touch. Spacing that looks fine with a mouse is often too tight on a phone.
- Test the awkward middle widths (around 768px and 1024px), not just the extremes.

## How to avoid generic AI-slop layouts

These are the tells. Audit your output against them.

- Centered everything with the same shadow on every card. Vary alignment and elevation by role.
- The default purple-to-blue gradient hero. If you want a gradient, derive it from the brand and keep it subtle.
- Three feature cards with a generic icon, a two-word title, and a sentence of filler. If the content is real, it rarely fits that mold exactly.
- Equal visual weight on every element, so nothing leads. Designed layouts have a clear hierarchy and obvious primary action.
- Inconsistent spacing because each section was styled in isolation. Reuse the same vertical rhythm between sections.
- Emoji used as iconography. Use a real icon set (such as lucide-react) for a coherent look.
- Lorem-flavored copy left in. Push for real text early; copy length drives layout.
- Over-rounded, candy-colored buttons. Match the radius and weight to the rest of the system.

## Recommended setup

1. Initialize Tailwind, then run the shadcn/ui init to generate `components.json` and the CSS variable theme.
2. Set your design tokens in the theme layer first (colors, radius, fonts). Do this before building screens.
3. Add only the components you need with the CLI as you go.
4. Keep a `cn()` util and define shared variants with `cva`.
5. Pick a single icon set and a single font pairing (one display, one text) and commit to them.

## Build checklist

Before calling a UI done, confirm:

- Spacing comes from the scale, no arbitrary one-off values
- Type roles are consistent across the page
- Color is mostly neutral with intentional accents and AA contrast
- One base radius, a small set of elevation tiers
- Every interactive element is semantic, focusable, and labeled
- Layout reflows cleanly from mobile to desktop
- There is one clear focal point and primary action per view
- None of the AI-slop tells above are present
