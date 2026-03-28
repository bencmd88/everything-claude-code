---
name: ui-designer
description: UI design systems, tokens, component architecture, accessibility, typography, color, layout, and interaction patterns for building polished interfaces.
origin: ccplugins/awesome-claude-code-plugins
---

# UI Designer

Patterns and principles for building cohesive, accessible, and polished user interfaces. Covers design tokens, component architecture, layout systems, typography, color, animation, and accessibility.

## When to Activate

- Designing or auditing a component library or design system
- Implementing design tokens (colors, spacing, typography, shadows)
- Building responsive layouts with Grid or Flexbox
- Applying accessible color contrast or ARIA patterns
- Creating animation and micro-interaction patterns
- Reviewing visual hierarchy, spacing rhythm, and typographic scale
- Migrating from ad-hoc styles to a systematic design language

## Design Tokens

Design tokens are the single source of truth for all visual decisions.

### Token Structure

```ts
// tokens.ts
export const tokens = {
  color: {
    brand: {
      50:  '#eff6ff',
      100: '#dbeafe',
      500: '#3b82f6',
      900: '#1e3a8a',
    },
    neutral: {
      0:   '#ffffff',
      50:  '#f8fafc',
      100: '#f1f5f9',
      500: '#64748b',
      900: '#0f172a',
      1000: '#000000',
    },
    semantic: {
      success: '#22c55e',
      warning: '#f59e0b',
      error:   '#ef4444',
      info:    '#3b82f6',
    },
  },
  spacing: {
    px:  '1px',
    0:   '0',
    1:   '0.25rem',  // 4px
    2:   '0.5rem',   // 8px
    3:   '0.75rem',  // 12px
    4:   '1rem',     // 16px
    6:   '1.5rem',   // 24px
    8:   '2rem',     // 32px
    12:  '3rem',     // 48px
    16:  '4rem',     // 64px
  },
  radius: {
    none: '0',
    sm:   '0.25rem',
    md:   '0.5rem',
    lg:   '0.75rem',
    xl:   '1rem',
    full: '9999px',
  },
  shadow: {
    sm:  '0 1px 2px 0 rgb(0 0 0 / 0.05)',
    md:  '0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)',
    lg:  '0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)',
    xl:  '0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)',
  },
  duration: {
    fast:   '100ms',
    normal: '200ms',
    slow:   '300ms',
    slower: '500ms',
  },
  easing: {
    standard:    'cubic-bezier(0.4, 0, 0.2, 1)',
    decelerate:  'cubic-bezier(0, 0, 0.2, 1)',
    accelerate:  'cubic-bezier(0.4, 0, 1, 1)',
    spring:      'cubic-bezier(0.34, 1.56, 0.64, 1)',
  },
}
```

### CSS Custom Properties

```css
:root {
  /* Colors */
  --color-brand-500: #3b82f6;
  --color-surface: #ffffff;
  --color-surface-subtle: #f8fafc;
  --color-text: #0f172a;
  --color-text-muted: #64748b;
  --color-border: #e2e8f0;

  /* Spacing scale (4px base) */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-8: 2rem;

  /* Typography */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;

  /* Radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;

  /* Motion */
  --duration-normal: 200ms;
  --easing-standard: cubic-bezier(0.4, 0, 0.2, 1);
}
```

## Typography System

### Type Scale (Major Third — 1.25 ratio)

| Step | Size      | Usage                  |
|------|-----------|------------------------|
| xs   | 0.75rem   | Captions, labels       |
| sm   | 0.875rem  | Secondary text, meta   |
| base | 1rem      | Body copy              |
| lg   | 1.125rem  | Lead paragraph         |
| xl   | 1.25rem   | Card titles            |
| 2xl  | 1.5rem    | Section headings       |
| 3xl  | 1.875rem  | Page headings          |
| 4xl  | 2.25rem   | Hero headings          |

### Line-Height Pairings

```css
.text-body   { font-size: 1rem;    line-height: 1.5; }   /* reading */
.text-ui     { font-size: 0.875rem; line-height: 1.4; }  /* compact UI */
.text-heading { font-size: 1.5rem;  line-height: 1.2; }  /* headlines */
```

### Responsive Type with clamp()

```css
h1 {
  /* Scales from 1.875rem at 320px to 3rem at 1280px */
  font-size: clamp(1.875rem, 1.5rem + 2vw, 3rem);
}
```

## Color System

### Semantic Color Roles

```ts
const semanticColors = {
  // Surfaces
  surface:         tokens.color.neutral[0],
  surfaceSubtle:   tokens.color.neutral[50],
  surfaceInverse:  tokens.color.neutral[900],

  // Text
  textPrimary:     tokens.color.neutral[900],
  textSecondary:   tokens.color.neutral[500],
  textDisabled:    tokens.color.neutral[300],
  textInverse:     tokens.color.neutral[0],
  textBrand:       tokens.color.brand[500],

  // Interactive
  interactive:     tokens.color.brand[500],
  interactiveHover: tokens.color.brand[600],
  interactiveFocus: tokens.color.brand[700],

  // Borders
  border:          tokens.color.neutral[200],
  borderStrong:    tokens.color.neutral[400],
  borderFocus:     tokens.color.brand[500],
}
```

### WCAG Contrast Requirements

| Text Type        | Min Ratio | Target Ratio |
|------------------|-----------|--------------|
| Normal text      | 4.5:1     | 7:1          |
| Large text (18px+) | 3:1     | 4.5:1        |
| UI components    | 3:1       | 4.5:1        |
| Decorative       | none      | —            |

```ts
// Check contrast programmatically
function getContrastRatio(fg: string, bg: string): number {
  const l1 = getRelativeLuminance(fg)
  const l2 = getRelativeLuminance(bg)
  const lighter = Math.max(l1, l2)
  const darker  = Math.min(l1, l2)
  return (lighter + 0.05) / (darker + 0.05)
}
```

## Layout System

### Spacing Rhythm

Use the 4px base unit consistently — avoid arbitrary values.

```css
/* ✅ On the grid */
.card { padding: 1rem; gap: 0.75rem; }

/* ❌ Off the grid */
.card { padding: 14px; gap: 11px; }
```

### Responsive Grid

```css
.grid-layout {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(280px, 100%), 1fr));
  gap: var(--space-4);
}
```

### Content Width Constraints

```css
.container { max-width: 1280px; margin-inline: auto; padding-inline: 1rem; }
.container-prose { max-width: 65ch; }   /* Ideal reading width */
.container-narrow { max-width: 480px; } /* Forms, dialogs */
```

### Flexbox Patterns

```css
/* Center anything */
.center { display: flex; align-items: center; justify-content: center; }

/* Space between with alignment */
.row { display: flex; align-items: center; gap: var(--space-2); }

/* Stack with consistent spacing */
.stack { display: flex; flex-direction: column; gap: var(--space-4); }
```

## Component Patterns

### Button Variants

```tsx
type ButtonVariant = 'primary' | 'secondary' | 'ghost' | 'danger'
type ButtonSize    = 'sm' | 'md' | 'lg'

const buttonStyles: Record<ButtonVariant, string> = {
  primary:   'bg-brand-500 text-white hover:bg-brand-600 focus-visible:ring-brand-500',
  secondary: 'bg-surface border border-border text-text hover:bg-surface-subtle',
  ghost:     'bg-transparent text-text-muted hover:bg-surface-subtle hover:text-text',
  danger:    'bg-error text-white hover:bg-red-600 focus-visible:ring-red-500',
}

const buttonSizes: Record<ButtonSize, string> = {
  sm: 'h-8  px-3 text-sm  rounded-md',
  md: 'h-10 px-4 text-base rounded-md',
  lg: 'h-12 px-6 text-lg  rounded-lg',
}
```

### Focus Ring (Accessible)

```css
/* Apply to ALL interactive elements */
:focus-visible {
  outline: 2px solid var(--color-brand-500);
  outline-offset: 2px;
}

/* Remove default outline — only after adding :focus-visible */
:focus:not(:focus-visible) {
  outline: none;
}
```

### Skeleton Loading

```css
@keyframes shimmer {
  from { background-position: -200% 0; }
  to   { background-position:  200% 0; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-neutral-100) 25%,
    var(--color-neutral-200) 37%,
    var(--color-neutral-100) 63%
  );
  background-size: 400% 100%;
  animation: shimmer 1.4s ease infinite;
  border-radius: var(--radius-sm);
}
```

## Animation & Motion

### Transition Defaults

```css
/* All interactive state changes */
.btn, .input, .card {
  transition:
    background-color var(--duration-normal) var(--easing-standard),
    border-color     var(--duration-normal) var(--easing-standard),
    box-shadow       var(--duration-normal) var(--easing-standard),
    color            var(--duration-normal) var(--easing-standard);
}
```

### Entrance Animations

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes scale-in {
  from { opacity: 0; transform: scale(0.95); }
  to   { opacity: 1; transform: scale(1); }
}

.animate-in    { animation: fade-in  var(--duration-slow) var(--easing-decelerate) both; }
.scale-in      { animation: scale-in var(--duration-normal) var(--easing-spring) both; }
```

### Respect Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration:   0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration:  0.01ms !important;
  }
}
```

## Accessibility Patterns

### ARIA Landmark Roles

```html
<header role="banner">…</header>
<nav    role="navigation" aria-label="Main">…</nav>
<main   role="main">…</main>
<aside  role="complementary">…</aside>
<footer role="contentinfo">…</footer>
```

### Icon Buttons

```tsx
// ✅ Accessible icon button
<button aria-label="Close dialog" type="button">
  <XIcon aria-hidden="true" />
</button>
```

### Live Regions

```tsx
// Announce dynamic changes to screen readers
<div role="status" aria-live="polite" aria-atomic="true">
  {statusMessage}
</div>

<div role="alert" aria-live="assertive">
  {errorMessage}
</div>
```

### Skip Navigation

```html
<!-- First element in <body> -->
<a href="#main-content" class="sr-only focus:not-sr-only">
  Skip to main content
</a>
```

### Screen-Reader Only Utility

```css
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

## Dark Mode

### CSS Variables Approach

```css
:root {
  --color-surface: #ffffff;
  --color-text:    #0f172a;
  --color-border:  #e2e8f0;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-surface: #0f172a;
    --color-text:    #f8fafc;
    --color-border:  #334155;
  }
}

[data-theme='dark'] {
  --color-surface: #0f172a;
  --color-text:    #f8fafc;
  --color-border:  #334155;
}
```

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| 4px spacing base | Aligns with most design tools (Figma 8pt grid = 2 steps) |
| Semantic color roles | Decouple brand colors from usage — enables theming |
| `clamp()` for type | Fluid scaling without breakpoint-specific overrides |
| CSS custom properties | Runtime theming, dark mode without JS |
| `prefers-reduced-motion` | Motion that harms vestibular users must be skippable |
| `:focus-visible` | Keyboard users get focus rings; mouse users don't |

## Best Practices

- **Use the spacing scale exclusively** — never hardcode arbitrary px values
- **Name colors semantically** (`--color-text-muted`, not `--color-gray-400`)
- **Test at 200% zoom** — WCAG 1.4.4 requires usable layout up to 200% zoom
- **Use `rem` for font sizes** — respects user browser font size preferences
- **Provide text alternatives** for all non-decorative images and icons
- **Ensure 44×44px minimum touch targets** for mobile interactive elements
- **Test with keyboard only** — tab through every interactive element

## Anti-Patterns to Avoid

- Hardcoding hex colors outside the token system
- Using `outline: none` without a `:focus-visible` replacement
- Relying on color alone to convey meaning (use icons or text too)
- Disabling zoom (`user-scalable=no`) in viewport meta
- Triggering animations on every page load without `prefers-reduced-motion` guard
- Using `div` or `span` as interactive elements without ARIA roles
- Typography below 12px for body text
- Touch targets smaller than 44×44px
