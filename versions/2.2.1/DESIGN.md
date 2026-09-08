---
version: "2.2.1"
name: "shadcn-admin"
description: "Version-pinned design system for operational admin interfaces based on satnaing/shadcn-admin v2.2.1."
colors:
  format: "oklch"
  light:
    background: "oklch(1 0 0)"
    foreground: "oklch(0.129 0.042 264.695)"
    primary: "oklch(0.208 0.042 265.755)"
    accent: "oklch(0.968 0.007 247.896)"
    destructive: "oklch(0.577 0.245 27.325)"
  dark:
    background: "oklch(0.129 0.042 264.695)"
    foreground: "oklch(0.984 0.003 247.858)"
    primary: "oklch(0.929 0.013 255.508)"
    accent: "oklch(0.279 0.041 260.031)"
    destructive: "oklch(0.704 0.191 22.216)"
typography:
  families:
    - "Inter"
    - "Manrope"
    - "system-ui"
  base_size: "16px inherited body; 14px text-sm controls"
  mobile_form_size: "16px native input/select/textarea below 768px"
---

# shadcn-admin 2.2.1 Design System

[English](DESIGN.md) | [简体中文](DESIGN.zh-CN.md)

<a id="scope"></a>
## Purpose, Scope, and Evidence

This is an independent design contract for AI coding agents building operational admin interfaces from **shadcn-admin 2.2.1**, not a universal shadcn/ui version. It covers visual foundations, installed components, page composition, interaction states, responsive behavior, and the limits of the demo. It does not supply component APIs, production authentication, permissions, business rules, or backend contracts.

The source baseline is upstream tag `v2.2.1`, commit [`0217f8cb73af66f3cbf4141d5e0d00e1c5c30434`][upstream]. The 2026-09-08 static audit matched all 247 upstream file blobs to the local reference snapshot. The live demo and latest documentation may have moved on; they are not version evidence.

| Marker | Meaning | How an agent should use it |
| --- | --- | --- |
| **S** | Source fact: local configuration, classes, props, route or handler in the pinned snapshot | Reproduce the relevant composition; do not infer unimplemented functionality |
| **D** | Dependency behavior: pinned package defaults, not implemented by this app | Preserve the primitive contract; verify the composed workflow in a browser |
| **R** | Recommended completion rule for a consuming product | Implement when the task requires it; do not claim upstream already does it |
| **V** | Unverified runtime behavior or an identified source limitation | Keep visible in acceptance criteria; do not claim a passing UI test |

Unless marked otherwise, component classes, dimensions, and handlers below are **S**. Source limitations are not instructions to reproduce bugs. Read in this order: this scope and the baseline, tokens and shell, the relevant component/page section, states and known gaps, then acceptance. When modifying an existing product, inspect its installed source and preserve its data and permission contracts.

Evidence precedence: pinned page composition and local overrides > pinned shared component > local theme/base CSS > lockfile-resolved dependency defaults. `cn()` uses `clsx` and `tailwind-merge`; caller classes can override defaults. CSS cascade, variant specificity, and the mobile `!important` input rule also matter. Do not replace this hierarchy with current registry examples.

<a id="baseline"></a>
## Pinned Technology Baseline

Exact resolutions come from [pnpm-lock.yaml][lock], not only the ranges in [package.json][package]. [components.json][config] sets `new-york`, `slate`, CSS variables, `rsc: false`, and `@/` aliases. This is a Vite React application, not Next.js.

| Package | Resolved version |
| --- | --- |
| `react`, `react-dom` | `19.2.0` |
| `vite` / `typescript` | `7.1.11` / `5.9.3` |
| `tailwindcss`, `@tailwindcss/vite` | `4.1.14` |
| `tw-animate-css` | `1.4.0` |
| `@tanstack/react-router` / `@tanstack/react-query` / `@tanstack/react-table` | `1.132.47` / `5.90.2` / `8.21.3` |
| `react-hook-form` / `@hookform/resolvers` / `zod` | `7.64.0` / `5.2.2` / `4.1.12` |
| `cmdk` / `input-otp` | `1.1.1` / `1.4.2` |
| `react-day-picker` / `date-fns` | `9.11.1` / `4.1.0` |
| `recharts` / `sonner` / `react-top-loading-bar` | `3.2.1` / `2.0.7` / `3.0.2` |
| `lucide-react` / `@radix-ui/react-icons` | `0.545.0` / `1.3.2` |
| `@clerk/clerk-react` / `zustand` / `axios` | `5.51.0` / `5.0.8` / `1.12.2` |
| `class-variance-authority` / `clsx` / `tailwind-merge` | `0.7.1` / `2.1.1` / `3.3.1` |

Radix package suffixes below mean `@radix-ui/react-*`; there is no single shared Radix version.

| Package suffix | Resolved version |
| --- | --- |
| `alert-dialog`, `dialog`, `popover` | `1.1.15` |
| `avatar` / `checkbox` / `collapsible` | `1.1.10` / `1.3.3` / `1.1.12` |
| `direction` / `dropdown-menu` / `label` | `1.1.1` / `2.1.16` / `2.1.7` |
| `radio-group` / `scroll-area` / `select` | `1.3.8` / `1.2.10` / `2.2.6` |
| `separator` / `slot` / `switch` | `1.1.7` / `1.2.3` / `1.2.6` |
| `tabs` / `tooltip` | `1.1.13` / `1.2.8` |

**R:** Reuse installed local components. Do not run the latest shadcn CLI with overwrite, introduce newer `Field`/`Empty`/`Spinner`/chat primitives as if installed, or upgrade dependencies as part of applying this document. `FormField`/`FormItem`/`FormControl` is this snapshot's form composition.

<a id="principles"></a>
## Design Principles

- Use compact, scan-friendly work surfaces, concise titles, semantic tokens, thin borders, and restrained elevation.
- Compose existing primitives and their variants; preserve local RTL adaptations and page-specific density.
- Use cards for repeated metrics, objects, or genuinely bounded tools, not every section or nested decoration.
- **R:** New flows need meaningful disabled, validation, pending, empty, failure, and recovery states. The demo does not implement all of them.
- Avoid marketing-scale headlines, decorative gradients, arbitrary brand colors, and layout shifts. Functional source exceptions are explicitly recorded below, not generalized to every page.

<a id="colors"></a>
## Color System

[theme.css][theme] defines 23 direct color roles per mode and eight sidebar aliases. `@theme inline` maps them to semantic utilities. The front matter is a compact summary; these tables are the full color contract. There is no separate `destructive-foreground` or success/warning token.

### Core Semantic Tokens

| Role | Light | Dark | Intended use |
| --- | --- | --- | --- |
| `background` | `oklch(1 0 0)` | `oklch(0.129 0.042 264.695)` | Application canvas |
| `foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | Primary text and icons |
| `card` | `oklch(1 0 0)` | `oklch(0.14 0.04 259.21)` | Card surface |
| `card-foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | Card content |
| `popover` | `oklch(1 0 0)` | `oklch(0.208 0.042 265.755)` | Menus, popovers, command surfaces |
| `popover-foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | Popover content |
| `primary` | `oklch(0.208 0.042 265.755)` | `oklch(0.929 0.013 255.508)` | Primary action and strong emphasis |
| `primary-foreground` | `oklch(0.984 0.003 247.858)` | `oklch(0.208 0.042 265.755)` | Content on primary |
| `secondary` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | Secondary controls and low emphasis |
| `secondary-foreground` | `oklch(0.208 0.042 265.755)` | `oklch(0.984 0.003 247.858)` | Content on secondary |
| `muted` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | Muted surfaces and disabled context |
| `muted-foreground` | `oklch(0.554 0.046 257.417)` | `oklch(0.704 0.04 256.788)` | Supporting copy, metadata, placeholders |
| `accent` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | Hover, selected navigation, soft emphasis |
| `accent-foreground` | `oklch(0.208 0.042 265.755)` | `oklch(0.984 0.003 247.858)` | Content on accent |
| `destructive` | `oklch(0.577 0.245 27.325)` | `oklch(0.704 0.191 22.216)` | Destructive actions and invalid states |
| `border` | `oklch(0.929 0.013 255.508)` | `oklch(1 0 0 / 10%)` | Dividers and surface outlines |
| `input` | `oklch(0.929 0.013 255.508)` | `oklch(1 0 0 / 15%)` | Input borders and dark input fill |
| `ring` | `oklch(0.704 0.04 256.788)` | `oklch(0.551 0.027 264.364)` | Keyboard focus indication |

### Chart and Sidebar Tokens

| Token | Light | Dark |
| --- | --- | --- |
| `chart-1` | `oklch(0.646 0.222 41.116)` | `oklch(0.488 0.243 264.376)` |
| `chart-2` | `oklch(0.6 0.118 184.704)` | `oklch(0.696 0.17 162.48)` |
| `chart-3` | `oklch(0.398 0.07 227.392)` | `oklch(0.769 0.188 70.08)` |
| `chart-4` | `oklch(0.828 0.189 84.429)` | `oklch(0.627 0.265 303.9)` |
| `chart-5` | `oklch(0.769 0.188 70.08)` | `oklch(0.645 0.246 16.439)` |

### Sidebar Aliases

These aliases are declared in `:root`; they resolve against the active light/dark core variables.

| Alias | Value in both modes |
| --- | --- |
| `sidebar` | `var(--background)` |
| `sidebar-foreground` | `var(--foreground)` |
| `sidebar-primary` | `var(--primary)` |
| `sidebar-primary-foreground` | `var(--primary-foreground)` |
| `sidebar-accent` | `var(--accent)` |
| `sidebar-accent-foreground` | `var(--accent-foreground)` |
| `sidebar-border` | `var(--border)` |
| `sidebar-ring` | `var(--ring)` |

### Source Exceptions and Extension Rules

- The dashboard uses `primary` and `muted-foreground`, with `#888888` axes, not `chart-1..5`. Those five variables exist but are not consumed by the two dashboard charts.
- [Users status styles][user-data] are `active`: teal, `inactive`: neutral, `invited`: sky, `suspended`: destructive. Exact existing classes: `bg-teal-100/30 text-teal-900 dark:text-teal-200 border-teal-200`; `bg-neutral-300/40 border-neutral-300`; `bg-sky-200/40 text-sky-900 dark:text-sky-100 border-sky-300`; `bg-destructive/10 dark:bg-destructive/50 text-destructive dark:text-primary border-destructive/10`.
- Apps' connected buttons use blue light/dark utilities; descriptions use `text-gray-500`. Theme previews and brand artwork also contain raw colors. Tooltip uses `primary`/`primary-foreground`, not popover colors. Destructive Button/Badge use white text, including dark `bg-destructive/60`.
- **R:** Prefer semantic roles for new work. Preserve or deliberately review the exceptions above; do not claim the source is entirely tokenized. New multiseries charts may use `chart-1..5` in a stable mapping, with labels or values beyond color. Verify contrast before extending status palettes.

<a id="typography"></a>
## Typography

[FontProvider][font-provider] defaults to `inter` and adds `font-inter`, `font-manrope`, or `font-system` to `html`. [index.html][html] loads Inter and Manrope through Google Fonts with `display=swap`. Theme declarations are exactly `'Inter', 'sans-serif'` and `'Manrope', 'sans-serif'`.

**D/V:** There is no local `--font-system` declaration. Selecting system removes the named font utility and relies on Tailwind/browser fallback; it does not load another font. Tailwind 4.1.14's default sans stack begins `ui-sans-serif, system-ui, sans-serif` and includes platform emoji fonts. Offline font substitution and non-Latin glyph coverage require runtime checks.

| Role / utility | Size / line height | Source use |
| --- | --- | --- |
| `text-xs` | 12px / 16px | Badges, metadata, group labels |
| `text-sm` | 14px / 20px | Most controls, table cells, navigation |
| Inherited body / `text-base` | 16px / 24px | Unqualified body text and descriptions |
| `text-lg` | 18px / 28px | Dialog title size; title overrides may use `leading-none` |
| `text-xl` | 20px / 28px | Section/auth titles |
| `text-2xl` | 24px / 32px | Page headings and metric values |
| `text-3xl` | 30px / 36px | Larger page headings, often at `md` |
| Error status | `7rem` (112px), `leading-tight` | 401/403/404/500/503, not normal page titles |

Sizes and default line heights are **D**, from [Tailwind 4.1.14 theme.css][tw-theme]; role assignments are **S**. CardTitle inherits size and uses semibold/`leading-none`; dashboard cards override it to compact `text-sm font-medium`. Labels are generally 14px medium, not uniformly 12px. Source page headings often use `tracking-tight`; tabular numerals are not an application-wide rule.

[index.css][base-css] forces **native** `input, select, textarea` to 16px with `!important` through 767px. Radix SelectTrigger is a button and remains `text-sm` unless overridden. OTP slot text is also not a native input. **R:** Preserve source heading classes for faithful reproduction; normal tracking and tabular figures in newly designed data columns are deliberate choices, not inherited global facts.

<a id="geometry"></a>
## Spacing, Shape, Elevation, and Motion

**D:** Tailwind spacing unit is `0.25rem` (4px with a 16px root). Common source gaps are 4/8/12/16/24/32px. [Main][main] uses horizontal 16px and vertical 24px padding. Settings forms use 32px field-group spacing; this is not interchangeable with compact table toolbars.

| Radius | Value | Existing examples |
| --- | --- | --- |
| Root `--radius` | `0.625rem` / 10px | Theme foundation |
| `radius-sm` | root - 4px = 6px | Menu items |
| `radius-md` | root - 2px = 8px | Buttons, inputs, navigation, Badge |
| `radius-lg` | root = 10px | Dialog, Alert, TabsList, app integration item |
| `radius-xl` | root + 4px = 14px | Card, inset shell, bulk toolbar |
| Local exceptions | 4px, full, 16px corners | Checkbox, Switch/Avatar, chat bubbles |

Borders are generally 1px; focus is not a substitute for the border. Common input/button focus is a 3px `ring-ring/50` with ring-colored border. PasswordInput and sidebar/menu/close controls have their own focus classes, so do not normalize every focus style by assertion.

**D:** Exact default shadows from the pinned Tailwind theme:

| Utility | Shadow |
| --- | --- |
| `shadow-xs` | `0 1px 2px 0 rgb(0 0 0 / 0.05)` |
| `shadow-sm` | `0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)` |
| `shadow-lg` | `0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)` |

Card uses `shadow-sm`; input/button often `shadow-xs`; Dialog/Sheet `shadow-lg`; Popover/Select/dropdown `shadow-md`. Bulk toolbar uses `shadow-xl`, and selected configuration previews use `shadow-2xl`. Those stronger shadows are local exceptions.

| Motion or layer | Source/default behavior |
| --- | --- |
| Unqualified transition | **D:** 150ms, not universally 200ms |
| Sidebar width/position, nav chevron, Dialog content | Explicit 200ms |
| `.CollapsibleContent` | 300ms ease-out height keyframes |
| Sheet | Close 300ms, open 500ms; direction-aware sliding |
| Menus/Popover/Tooltip | Fade/zoom/side entry from `tw-animate-css`; keep the installed classes |
| Sidebar / table sticky cells | `z-10` |
| Header, modal surfaces, bulk toolbar, mobile chat | `z-50`; equal values still depend on stacking context |
| Skip link | `z-999` |
| Header and bulk toolbar | Scrolled fixed header blur; bulk backdrop blur and hover scale 1.05 |
| Functional fading | `faded-bottom` adds a 128px gradient overlay at `md`, with pointer events disabled |

**V/R:** No explicit app-wide reduced-motion media rule was found. Respecting reduced motion, testing overlay stacking and background scroll locking, and preventing dynamic-content shifts are acceptance requirements for new product work, not certified source behavior.

<a id="shell"></a>
## Admin Shell and Navigation

Source: [authenticated-layout][shell], [Sidebar][sidebar], [Header][header], [Main][main], [nav-group][nav], and [sidebar-data][nav-data].

| Surface | Source contract |
| --- | --- |
| Expanded desktop sidebar | `--sidebar-width: 16rem` / 256px |
| Icon token | `--sidebar-width-icon: 3rem` / 48px |
| Plain sidebar collapsed | 48px allocation |
| Inset/floating collapsed | Gap = 48 + 16 = 64px; fixed outer container = 48 + 16 + 2 = 66px, including `p-2` treatment |
| Mobile sidebar | 18rem / 288px Sheet for collapsible modes |
| Header | 4rem / 64px; inner `p-4`, gap 12px then 16px at `sm` |
| Main | `px-4 py-6`; non-fluid width capped at 80rem / 1280px only at `@7xl/content` |
| Fixed Main | `flex grow flex-col overflow-hidden`; descendants own scrolling |
| Fixed shell | `100svh`; inset formula subtracts `var(--spacing) * 4` (16px) |
| Inset desktop frame | 8px margins, 14px radius, small shadow |

The app's LayoutProvider defaults to `inset` + `icon`; the primitive Sidebar alone defaults to `sidebar` + `offcanvas`. Variants: `inset/sidebar/floating`; collapsibility: `offcanvas/icon/none`. **Important:** `none` returns a static 16rem sidebar before the mobile branch; it does not become a mobile Sheet. It is supported by the component but is not offered in ConfigDrawer.

Only `<Header fixed>` is sticky. Its shadow and translucent blur appear when document scroll exceeds 10px; normal Header is not fixed. Apps, Chats and Settings use `<Main fixed>` with internal scrolling. Tasks and Users use `<Header fixed>` with a normal Main; Dashboard uses normal Header/Main. Do not conflate a sticky header with fixed-height content.

Sidebar menu rows default to 32px height, 14px text, 8px padding/radius, 16px icons; small/large variants are 28/48px. Group labels are 12px. `data-active` applies sidebar accent plus medium weight. Expanded nested menus use Collapsible and an indented logical-side border; collapsed desktop groups become dropdowns. Leaf clicks close the mobile Sheet. Open groups use `defaultOpen`, not a guaranteed route-synchronized expansion effect.

Groups are General, Pages, and Other. Chats badge `3` is static demo data. TeamSwitcher changes local displayed team, not tenancy or permissions. NavUser/ProfileDropdown provide menu compositions and SignOutDialog; displayed profile data is demo data. TopNav has a small-screen dropdown and desktop links, not a breadcrumb trail or persistent route-tab system.

**S/V:** Ctrl/Cmd+B toggles the sidebar; Ctrl/Cmd+K toggles global search. Both handlers are global and lack an input/contenteditable guard. **R:** Preserve names, focus, and logical directions, but review shortcut conflicts when adding editors. Do not invent breadcrumbs, mixed-navigation layout, or arbitrary-depth trees as existing features.

<a id="components"></a>
## Installed Component Contracts

The [UI directory][ui] contains exactly 30 files. Page overrides take precedence. This inventory documents actual primitives; loading/empty/error behavior must come from the relevant caller, not from a component name alone.

### Buttons and Badges

[button.tsx][button] exposes `default/destructive/outline/secondary/ghost/link`, `asChild`, and four sizes.

| Size | Height | Horizontal padding | With direct SVG child |
| --- | --- | --- | --- |
| `default` | 36px | 16px | 12px |
| `sm` | 32px | 12px | 10px |
| `lg` | 40px | 24px | 16px |
| `icon` | 36px square | Component layout | 16px default icon |

Default hover uses `primary/90`; secondary `secondary/80`; outline accent (dark input/50); ghost accent (dark accent/50); link underline. Disabled removes pointer events and halves opacity. There is no built-in `loading`/`isLoading` prop or universal active-scale rule. **R:** Compose a progress icon and `disabled` for real async actions without changing dimensions.

`badge.tsx` has default/secondary/destructive/outline variants, 12px medium text, 8px horizontal and 2px vertical padding, 8px radius, and 12px icons. Link hover is conditional on rendering as an anchor. Navigation count badges override to full rounding. A Badge is not a generic mode button.

### Forms, Inputs, and Selection

[form.tsx][form] composes RHF FormProvider, Controller-backed FormField, FormItem, FormLabel, FormControl, FormDescription, and FormMessage. FormControl wires `id`, `aria-describedby`, and `aria-invalid`; FormMessage renders the validation message. **V:** A caller bypassing FormControl does not automatically receive those associations.

| Source component | Geometry and state contract |
| --- | --- |
| `input.tsx` | 36px high, `min-w-0`, 12px horizontal padding, 8px radius, transparent / dark input/30, placeholder muted; focus ring 3px; invalid destructive border and ring/20 or dark /40; disabled 50% |
| `textarea.tsx` | Minimum 64px, `field-sizing-content`, 12px/8px padding; same focus, invalid, dark and disabled treatment |
| `label.tsx` | 14px medium, peer/group disabled styling; preserve association, not placeholder-only labels |
| `checkbox.tsx` | 16px square, explicit 4px radius, primary checked state, Check icon, invalid/focus/disabled styles; Radix supports indeterminate, but this wrapper does not supply a separate dash icon |
| `radio-group.tsx` | Group grid gap 12px; 16px round item and 8px primary dot, focus/invalid/disabled styles |
| `switch.tsx` | 32px by 1.15rem (18.4px), 16px thumb; primary checked/input unchecked, mirrored RTL thumb, focus/disabled styles; no dedicated local invalid style |
| `select.tsx` | Button trigger 36px or 32px, `w-fit`, 14px text; popper menu minimum 128px, available-height cap; selected check, focus accent, disabled 50%, invalid trigger |
| `input-otp.tsx` | 36px slots, active 3px ring, caret animation, grouped edge radii; actual OTP page uses six positions grouped 2+2+2 |
| Shared `password-input.tsx` | Native input 36px, 1px focus ring; 24px reveal button at logical end. No local destructive invalid classes or accessible reveal label |
| Shared `select-dropdown.tsx` | Items/value/open composition; optional disabled `loading` item is 56px high with a 20px spinning Loader; this does not fetch data |

**D:** Radix handles checkbox/radio/select state and keyboard interaction; RHF/Zod handles the passed schema. **R:** Keep visible labels, field-linked errors, explicit submit buttons, and repeat-submit guards. Do not claim every demo form blocks duplicate requests, warns on dirty navigation, or has server validation. Password reveal naming and wrapper label/invalid wiring require review.

### Calendar and DatePicker

[calendar.tsx][calendar] wraps DayPicker 9.11.1: 12px padding, 32px cell token, months stacked then horizontal at `md`, outside days shown by default. It styles today, single selection, range endpoints/middle, outside, disabled, hidden, and focused days; day focus is forwarded to the button. Range styling support does not mean every DatePicker is a range picker.

[date-picker.tsx][date-picker] is a shared single-date Popover with a 240px outline Button, `MMM d, yyyy` formatting, dropdown caption, and dates disabled after today or before `1900-01-01`. Empty displays a muted placeholder. There is no free-text parser, clear action, time picker, upload, or automatic close-on-select handler here. **D/V:** Calendar keyboard/date semantics come from the pinned DayPicker; localization, RTL range corner shapes, and zoom need runtime verification.

### Cards, Avatars, and Tabs

`card.tsx` uses card roles, border, 14px radius, `py-6`, group gap 24px, and `shadow-sm`. Header/Content/Footer use horizontal 24px padding; CardAction occupies the header action column. Card itself does not implement loading, empty, or failure states.

`avatar.tsx` defaults to 32px square with full rounding, image and muted fallback. Callers override size (for example 36/44px in chat). Retain fallback when images fail; a remote URL is not an availability guarantee.

`tabs.tsx`: root gap 8px; list 36px, muted surface, 10px radius, 3px padding. Triggers are 14px medium and nowrap, with 8px radius. Active is background + small shadow, but dark active uses input/30 and input border. Inactive text is foreground in light and muted in dark. Disabled blocks pointer events with 50% opacity. **D:** Radix supplies activation/keyboard behavior; callers must handle narrow overflow.

### Dialog, AlertDialog, Sheet, and Overlays

| Component | Surface and composition |
| --- | --- |
| `dialog.tsx` | Black/50 overlay; centered background, 24px padding, 16px gap, 10px radius, `shadow-lg`, viewport width minus 32px, `sm:max-w-lg` (512px). Footer is mobile reverse-column then end-aligned row. Optional built-in close button |
| `alert-dialog.tsx` | Separate Radix confirmation primitive; similar centered surface with cancel/action composition, not an informational toast |
| `sheet.tsx` | Background, border at entering edge, `shadow-lg`; left/right panels 75% width and `sm:max-w-sm` (384px); top/bottom auto height. Header/Footer padding 16px; footer at end |
| `dropdown-menu.tsx` | Popover surface, compact padded items, selected/checkbox/radio/submenu/destructive/disabled styles and shortcuts |
| `popover.tsx` | Default width 288px, 16px padding, 8px radius, border, `shadow-md`; callers frequently override to auto or 200px |
| `tooltip.tsx` | Primary background and foreground, 12px text, 12px/6px padding, 8px radius, arrow; not the popover surface |

[confirm-dialog.tsx][confirm] wraps AlertDialog. Its `isLoading` disables cancel and confirm; `disabled` can gate confirm separately. The confirm is a regular Button, not an automatic spinner or mutation. Callers own completion and closing.

**D:** Pinned Radix primitives supply portal placement, modal focus handling, dismiss/focus-return behavior, and menu navigation. Dialog/Sheet and AlertDialog have different outside-interaction contracts; do not assume identical dismissal. **V:** Custom controlled opens, triggerless compositions, and the global `body[data-scroll-locked] { overflow: unset !important; }` can affect outcomes. Test actual focus return and background scrolling. Every modal needs a valid title and description association; do not infer compliance from importing DialogTitle.

### Command Search and Layout Utilities

[CommandMenu][command-menu] derives destinations from sidebar-data and adds light/dark/system commands. It closes before navigating or setting a theme. CommandDialog input area is 48px, list max-height 300px, and the actual palette nests a 288px ScrollArea. Group headings are 12px muted; items 14px, accent-selected, disabled 50%; the dialog overrides item vertical padding to 12px. Empty text is `No results found.` This is local navigation/theme search, not server search or a permission-filtered index.

**D:** cmdk supplies query filtering, arrow navigation and Enter selection; Radix handles the enclosing modal. **V:** In [ui/command.tsx][command], the hidden DialogHeader/Title is outside DialogContent; verify accessible naming and modal hiding behavior rather than assuming it passes.

`collapsible.tsx` exposes Radix Root/Trigger/Content; the 300ms app animation is opt-in via `.CollapsibleContent`. `scroll-area.tsx` adds an `orientation` prop (vertical by default), a matching 10px scrollbar, and `overflow-x-auto!` on the horizontal viewport; there is no `viewportClassName` prop. `separator.tsx` defaults to horizontal and `decorative: true`, with 1px thickness; callers supply vertical height. These are not cards.

### Feedback, Loading, and Empty Content

`alert.tsx` uses `role="alert"`, a bordered 10px-radius card surface, title/description/icon layout, and default/destructive variants. `skeleton.tsx` is `bg-accent animate-pulse rounded-md`; callers supply size. SidebarMenuSkeleton is available, not evidence that every page fetches asynchronously.

[sonner.tsx][sonner] passes theme and overrides normal background/text/border with popover roles. [Root route][root-route] sets duration to 5000ms. Promise toasts appear in simulated auth/bulk flows. [NavigationProgress][progress] binds pending/complete router status to a 2px muted-foreground loading bar; it is not backend task progress.

There is no shared `Empty`, `Spinner`, or generic `Progress` component. Tables render a 96px `No results.` cell; command lists have their own empty text; chat has an initial conversation placeholder; Apps has no dedicated zero-result message. **R:** New async views should keep dimensions stable, distinguish initial loading from refresh, provide relevant empty/reset/retry actions, and announce success/failure without inventing source behavior.

### Shared Shell and Content Compositions

Sources: the named files in [shared components][shared] and [layout components][layout-components].

| Component | Actual composition and boundary |
| --- | --- |
| `app-sidebar.tsx`, `app-title.tsx` | AppSidebar mounts TeamSwitcher, NavGroup, NavUser and SidebarRail. AppTitle is an available home-link/toggle alternative, commented out in the default composition |
| `search.tsx` | A 32px outline Button opening CommandMenu, not an inline search input; width grows from flexible to 160/208/256px at sm/lg/xl; shortcut badge appears at sm |
| `top-nav.tsx` | Nonmodal dropdown below lg, inline links from lg; caller-provided active/disabled values, no route-tab history |
| `nav-user.tsx`, `profile-dropdown.tsx` | Avatar/profile menu, settings destinations and SignOutDialog; Billing leads to Settings, Upgrade/New Team are placeholders, displayed shortcut labels do not register handlers |
| `sign-out-dialog.tsx` | 384px confirmation; resets local auth and redirects to sign-in with the current location, not server-session revocation |
| `theme-switch.tsx` | Nonmodal light/dark/system menu, selected check, animated sun/moon icons; updates theme-color to `#020817` only for explicit dark, otherwise `#fff` |
| `learn-more.tsx` | 20px named icon trigger with a top/start Popover and 14px muted content, not a Dialog or necessarily an external link |
| `coming-soon.tsx` | Full-height centered placeholder, 72px icon and 36px title; no working help action |
| `long-text.tsx`, `skip-to-main.tsx` | Truncation disclosure and bypass intent; concrete keyboard/target gaps are documented below |

<a id="tables"></a>
## Data Tables, Filtering, and Bulk Actions

Sources: [data-table components][data-table], [Tasks table][tasks-table], [Users table][users-table], [URL state hook][url-state].

- `ui/table.tsx` wraps a semantic table in `overflow-x-auto`. Headers are 40px high with 8px horizontal padding; cells have 8px padding, 14px text and nowrap. Rows use `hover:bg-muted/50` and `data-[state=selected]:bg-muted`, **not accent**.
- TableFooter has muted/50 fill, top border and medium weight; TableCaption is bottom-positioned, 14px muted with 16px top margin. These primitives exist even though Tasks/Users do not render them.
- Toolbar search is 32px high, 150px wide then 250px at `lg`; filter controls and Reset stay near the data. Column View is hidden below `lg`, with a nonmodal checkbox dropdown for hideable accessor columns.
- ColumnHeader offers ascending, descending and hiding where permitted. Faceted filters use Popover + Command, multi-selection, per-option counts, selected badges/counts and a clear action.
- Header checkbox toggles **all rows on the current page**, not the entire dataset. Bulk operations read **filtered selected rows**. Sorting, visibility and selection stay local; do not imply they all serialize to URL.
- Users pins the selection and username columns near logical start on narrow layouts; cell backgrounds preserve hover/selected state. Its `@4xl/content` shadow treatment is container-dependent. Do not replace this with a claim that columns automatically turn into cards.

| Page | URL-backed state | Data boundary |
| --- | --- | --- |
| Tasks | `filter`, `status`, `priority`, `page`, `pageSize` | Client filtering of mock data; global filter targets task id/title |
| Users | `username`, `status`, `role`, `page`, `pageSize` | Column filters; global filtering disabled |
| Both | Default page 1, pageSize 10; Table pageIndex starts at 0 | Filter changes reset page; sorting/visibility/selection are not URL keys |

**V:** The hook initializes local filters from search but does not synchronize them through an effect when browser history changes. Pagination is derived from current search. `ensurePageInRange` only corrects out-of-range pages when pageCount > 0. Refresh/share support is not proof of full back/forward or empty-pagination correctness.

Pagination uses 32px controls, page sizes 10/20/30/40/50, current `Page X of Y`, previous/next, and first/last where space permits. `getPageNumbers` shows all pages up to five, otherwise boundaries and ellipses. There is **no current item range** such as "1-10 of 100". Under `@2xl/content` (672px) it stacks in reverse; first/last hide below `@md/content` (448px). The two page-label visibility rules leave a static 672-767px container gap to verify; this is not the 768px viewport mobile rule.

[BulkActions][bulk] is viewport-fixed at bottom 24px, centered, z-50, with a 14px radius, blur, `shadow-xl` and hover scale 1.05. It appears for selection, announces counts politely, and has Arrow/Home/End navigation. Escape skips clearing when the event target or active element is a dropdown trigger/content; it does not simply test whether a menu is open. Otherwise Escape clears selection. **R:** Verify clipping, zoom, mobile reachability and modal stacking when adapting it.

<a id="pages"></a>
## Page Patterns and Actual Behavior

### Dashboard

[Dashboard][dashboard] has Overview and Analytics tabs; Reports and Notifications are disabled. Overview metrics use a one-column grid, two at `sm`, four at `lg`; lower panels form a seven-track `lg` grid split 4/3. RecentSales combines avatar fallback, name/email and right-aligned amount.

[Overview chart][overview] is a 350px Recharts BarChart, monthly mock values, primary bars with 4px top corners, 12px `#888888` axes, no tick or axis lines, and dollar-formatted Y ticks. [Analytics chart][analytics-chart] is a 300px AreaChart with primary clicks (fill opacity .15) and muted-foreground uniques (.1), weekday data and the same axis treatment. Analytics also uses SimpleBarList with 10px-high rounded bars, 12px labels and tabular values; it is not a generic Progress primitive.

There are no Tooltip/Legend components in these charts, no chart-token series mapping, and no chart loading/empty/error branches. Download has no handler; dashboard top-nav example paths are not registered business destinations. **R:** For live analytics add accessible series/value descriptions, loading, empty and retry states, and a real export handler without claiming the demo provides them.

### Tasks

[Tasks][tasks] is a client-data table with label, title, status and priority patterns. Status values include backlog/todo/in progress/done/canceled; priority uses low/medium/high with icons. Preserve schema values and displayed labels rather than translating enum identifiers.

Create/update is [TasksMutateDrawer][task-sheet], implemented with **Sheet**, not Dialog. It has required title/status/label/priority, SelectDropdowns and radio choices, and a footer submit action; submit displays data, resets and closes. Single delete uses ConfirmDialog; Make a copy and Favorite are disabled demo row actions.

[Import][task-import] uses a `sm:max-w-sm` Dialog and file input. Validation requires a nonempty FileList and first-file MIME `text/csv`; the handler only displays name/size/type. It does not parse CSV, upload, create rows, or provide progress, size limits or drag-and-drop. **R:** Those require explicit product work and server validation, not a visual specification assumption.

Bulk status/priority/export/delete actions simulate feedback via a two-second promise. Bulk delete requires trimmed exact `DELETE`, then clears selection and toasts; it does not remove data. Source pending behavior is not universal repeat-submit protection.

### Users

[Users][users] combines username/status/role filters, sticky-column table, row menus, invite/create/edit/delete overlays and bulk actions. Role values are `superadmin/admin/manager/cashier`; status colors are recorded above. Role labels are not an authorization implementation.

[Create/edit][user-dialog] uses a `sm:max-w-lg` Dialog with a 26.25rem (420px) scrolling form region, a six-column label/control layout (2/4) and 16px gaps. It does **not** automatically stack labels on narrow screens. Fields include first/last name, username, email, phone, role, password and confirmation. New password validation requires at least eight characters, lowercase and a digit; edit can leave password empty. Confirmation is disabled until the password field is dirty. Preserve the actual Zod schema, not the looser auth-page password rule.

[Invite][user-invite] uses a `sm:max-w-md` (448px) Dialog with required email/role and optional description. Submit displays data, resets and closes; no invitation email is sent. Single delete requires the trimmed exact username; bulk delete requires `DELETE`, simulates two seconds and clears selection. Neither is a persistent deletion. **R:** When connecting real requests, gate repeat submits, keep failed forms recoverable, and confirm the affected scope.

### Apps

[Apps][apps] is a scrollable integration **list/grid**, not a data table. URL keys are `filter/type/sort`; type is `all/connected/notConnected`, sort `asc/desc`, with client name filtering/sorting and local state initialized from search. Like table filters, subsequent browser-history synchronization is not guaranteed.

Grid is one column, two at `md`, three at `lg`, with 16px gaps. Items use `rounded-lg border p-4 hover:shadow-md` (10px radius), 40px brand icon areas, two-line descriptions, and the functional `faded-bottom` overlay. Connected buttons use blue exceptions. Connect buttons have no action; no zero-result branch or actual integration authorization is present.

### Chats

[Chats][chats] reads static `convo.json`. Search trims and matches full names; choosing a person changes local selection. The list is full width then 224px at `sm`, 288px at `lg`, 320px at `2xl`. Below **640px**, the selected conversation overlays the main region at z-50, with a Back button clearing mobile selection; from `sm` it is a split view. This differs from the 768px sidebar boundary.

History is grouped by `d MMM, yyyy`, timestamps use `h:mm a`, and a reverse-column scroll area keeps the sample order. Bubbles max at 288px, use 12px/8px padding, wrap words, and have local 16px corner shapes; outgoing is primary/90 with tinted foreground, incoming muted. The initial unselected desktop panel offers "Send message".

Composer form has **no submit handler or message mutation**. Send can trigger native form submission; attachments, phone/video and more controls have no implemented action. There is no real unread tracking, streaming, delivery, upload or persistence. [NewChat][new-chat] is a 600px Dialog with Command search and removable selection badges; Chat is disabled with no selection and otherwise only shows submitted data. Close resets selections. **R:** Treat message delivery, accessible icon labels, error/retry and attachment handling as separate implementation work.

### Authentication

[Auth pages][auth] include `/sign-in`, `/sign-in-2`, `/sign-up`, `/forgot-password`, and `/otp`. Shared auth layout is centered; SignIn2 uses two columns at `lg` and hides its light/dark dashboard screenshot below that breakpoint. Do not apply its image composition to ordinary admin pages.

Sign-in validates email/password (minimum seven characters), disables submission while simulating a two-second promise, stores a mock user/token, and follows the redirect or home route. Sign-up validates matching passwords and simulates loading; forgot-password shows a two-second promise and navigates to OTP. OTP requires exactly six characters, disables Verify until complete/loading, then shows data and navigates home after one second. It is not evidence of verified server credentials, sent email, or validated OTP codes. Social buttons and `/terms`/`/privacy` links are unimplemented destinations/actions in this snapshot.

**R:** Do not log/toast real passwords or tokens by copying demo handlers. Production credentials, request errors, resend/rate limits and account recovery must be implemented against the consuming product's contracts.

### Settings

[Settings layout][settings] has five routes. Below `md`, navigation is a 48px Select; at `md` it is horizontal ScrollArea navigation; at `lg` a 20%-width vertical sidebar with 48px inter-column gap. The content section has a 576px maximum inner width from `lg`, its own scrolling and `faded-bottom`; fields use 32px group spacing.

| Route | Source form behavior | Boundary |
| --- | --- | --- |
| `/settings` (Profile) | Username 2-30 chars, email selection, bio 4-160 chars, URL list via useFieldArray; append URL action | Submit only shows data; no profile service |
| `/settings/account` | Name 2-30 chars, constrained DatePicker, searchable language Popover/Command | Submit only shows data; language choice does not translate the app |
| `/settings/appearance` | Native font select, light/dark preview radio options; submit calls setFont/setTheme and shows data | Schema omits `system` although ThemeProvider can start in system |
| `/settings/notifications` | all/mentions/none radio; email switches; security email on and disabled/`aria-readonly`; mobile checkbox | No notification service or saved server preferences |
| `/settings/display` | Multiple item checkboxes with a nonempty selection rule | Submit only shows data; does not alter sidebar visibility |

**V:** [AppearanceForm][appearance] casts initial theme to light/dark without converting `system`; a system-default submission can fail schema validation until a valid theme is selected. There is no universal dirty-state navigation guard, saving spinner, server failure or saved-state persistence across these forms.

### Optional Clerk Integration

[Clerk route][clerk] requires `VITE_CLERK_PUBLISHABLE_KEY`. Without it, an instructional Alert page is shown. With it, ClerkProvider renders hosted-library SignIn/SignUp routes. This is third-party behavior, not the mock sign-in flow.

`/clerk/user-management` still renders the mock UsersTable dataset. Its component checks Clerk loading/sign-in state, shows a spinner, and starts an unauthorized five-second redirect countdown after the explanatory LearnMore Popover closes; the redirect can be cancelled. The `_authenticated` layout name alone is not an authorization guard. The root Clerk provider does not configure an appearance/dark/RTL adapter. **V:** Real Clerk sessions, redirects, theming and remote data are not verified by this source audit.

### Errors, Help, and Request Feedback

[Error family][errors] covers standalone `/401 /403 /404 /500 /503` and shell-contained `/errors/$error` mapping unauthorized/forbidden/not-found/internal-server-error/maintenance-error; unknown values use NotFound. Most show the 112px code, short explanation, history Back and Home. 401 does not directly provide a Sign In button; 503's Learn more has no handler. GeneralError `minimal` omits code/actions. Help Center is ComingSoon, not a knowledge base or working support workflow.

[main.tsx][entry] configures Query request feedback: 401 resets auth, toasts and redirects to sign-in with a redirect search value; 500 toasts and navigates to /500 only in production; 403 navigation is commented out. Queries skip retries for Axios 401/403, use development/production-specific retries, 10-second stale time and production focus refetch. Mutation errors call handleServerError. These shared hooks do not turn static Tasks/Users/Apps into API-backed pages.

<a id="states"></a>
## Interaction State Matrix

| State | Existing source treatment | Completion rule / limitation |
| --- | --- | --- |
| Default / filled | Component surface, placeholder/value and labels | Preserve width and density as content changes |
| Hover | Button variants, table muted/50, nav sidebar-accent, app item shadow | Do not force one accent/scale rule on all controls |
| Focus / focus-visible | Common 3px ring; password/sidebar/close-control exceptions | **R:** Test visibility, names, focus order and return in real compositions |
| Active / pressed | Native/Radix state; specific nav active surface | No universal custom pressed animation |
| Selected / checked | Table muted, active Tabs, primary control mark, nav data-active | State must not depend on color alone; indeterminate visual is not a dedicated dash |
| Open / expanded | Radix state, Collapsible chevron and animation | Group defaultOpen is not a guaranteed route-update effect |
| Disabled | Typically 50% opacity; pointer/cursor rules vary by component | Must be truly unavailable; checked security setting remains visible |
| Loading / submitting | Auth disabled buttons, promise toasts, navigation bar, SelectDropdown item, Skeleton primitives | Demo CRUD does not provide uniform loading or duplicate prevention |
| Empty | Table/Command messages, initial chat placeholder | Apps and charts lack dedicated empty branches |
| Invalid | RHF message and aria wiring; common destructive input ring/border | PasswordInput and callers bypassing FormControl need additional wiring |
| Request failure | Shared Query/Mutation feedback, standalone errors | Mock page handlers do not exercise server rejection/retry |
| Success | Demo data/promise toasts, Appearance applies local preferences | Toast success is not proof of a persistent mutation |
| Destructive | ConfirmDialog, username or DELETE gate for specified flows | Confirm affected scope; actual backend deletion is absent |
| Recovery | Reset filters, page navigation, cancel dialogs, error Back/Home | **R:** Add retry/resend/undo only where supported by business logic |

<a id="responsive"></a>
## Responsive Behavior

Do not collapse viewport and named-container breakpoints into one "mobile" rule. [use-mobile.tsx][mobile] compares width < 768. **D:** Values below come from Tailwind 4.1.14.

| Viewport threshold | Source examples |
| --- | --- |
| `sm` 640px | Chat split view; dashboard two metric columns; LongText Tooltip rather than mobile Popover; Dialog width/footer variants |
| `md` 768px | Desktop sidebar, Apps two columns, horizontal settings navigation, end of native input 16px override |
| `lg` 1024px | Four dashboard metrics; Apps three columns; vertical settings nav; column View control; two-column auth |
| `xl` 1280px / `2xl` 1536px | Available viewport thresholds; chat list widens at 2xl |

| `@container/content` threshold | Value | Actual use |
| --- | --- | --- |
| `@md/content` | 448px | Pagination first/last control visibility |
| `@2xl/content` | 672px | Pagination stacking / first page label |
| `@3xl/content` | 768px | Second page label visibility |
| `@4xl/content` | 896px | Users sticky-column shadow treatment |
| `@7xl/content` | 1280px | Non-fluid Main max width |

**R:** Keep horizontal overflow within tables and vertical scroll inside fixed layouts. Do not claim all forms stack at 768px: Users' 2/4 grid, 240px DatePicker and 200px language/font controls need narrow-screen review. Test long localized labels, empty and large datasets, zoom, portrait/landscape, theme and RTL. `html overflow-x-hidden` can conceal clipping; it is not proof of a responsive pass.

<a id="personalization"></a>
## Theme, Direction, and Persistence

| Preference | Mechanism / default | Persistence |
| --- | --- | --- |
| Theme | [ThemeProvider][theme-provider], system/light/dark; default system; html class | Cookie `vite-ui-theme`, 365 days |
| Font | [FontProvider][font-provider], inter/manrope/system; default inter | Cookie `font`, 365 days |
| Direction | [DirectionProvider][direction], html dir + Radix DirectionProvider; default ltr | Cookie `dir`, 365 days |
| Sidebar style | [LayoutProvider][layout-provider], inset/sidebar/floating; default inset | Cookie `layout_variant`, 7 days |
| Collapse mode | icon/offcanvas/none; app default icon | Cookie `layout_collapsible`, 7 days |
| Desktop open state | Read by AuthenticatedLayout; expanded unless cookie is false | Cookie `sidebar_state`, 7 days |
| Mobile open state | SidebarProvider local state | Not the persisted desktop open preference |

[ConfigDrawer][config-drawer] applies theme/direction/layout immediately. Sidebar style and layout sections are hidden below `md`. Layout labels map to Default = open, Compact = closed/icon, Full = closed/offcanvas. Reset reopens the sidebar and resets theme, direction and layout **but not font**. Appearance changes theme/font only on submit.

**V:** Theme classes are applied in an effect; the source does not guarantee flash-free initialization. OS theme changes update root classes but do not update the memoized `resolvedTheme` context until theme changes. Consumers of that value may become stale even when CSS colors update. Root Sonner receives the theme preference, while Clerk has no app-specific appearance mapping.

**R:** Preserve all semantic roles, local dark exceptions, portal ancestry and RTL changes. Test initial paint, OS changes while open, native controls, third-party widgets, and return from settings. Do not state that every third-party surface automatically inherits every preference.

<a id="accessibility"></a>
## Accessibility and Content Rules

The upstream README identifies local modifications to ScrollArea, Sonner and Separator and RTL adaptations to AlertDialog, Calendar, Command, Dialog, DropdownMenu, Select, Table, Sheet, Sidebar and Switch. Preserve these files during any later merge; current registry code is not a drop-in replacement.

**S/V source findings:**

- [SkipToMain][skip] links to `#content`, but no matching target exists in this snapshot's Main/routes. Keep the bypass intent; **R:** wire a real focusable main destination in a product implementation.
- Password reveal, some chat actions and badge-removal buttons have no accessible name. FormControl is not used around every custom field, so labels/errors are not universally associated.
- CommandDialog title placement needs a real accessibility-tree check. Controlled/triggerless dialogs require explicit focus-return testing.
- [LongText][long-text] checks overflow when its ref attaches, uses Tooltip at `sm` and Popover below, but its div triggers are not keyboard-focusable and it has no ResizeObserver. Complete-value access on keyboard/resize is not guaranteed.
- Logical spacing and direction-aware primitives coexist with physical chart/corner/position classes. No app-wide reduced-motion rule or measured contrast report was found.
- HTML starts `lang="en"`. The Account language field only submits demo data; RTL is not a translation system.

**R content contract for extensions:** Keep human-readable labels alongside status/icon/color, expose a name for every icon action, retain fallback identities, and make full critical values available beyond truncation. DatePicker uses `MMM d, yyyy`; chat uses `d MMM, yyyy` and `h:mm a`; dashboard amounts use dollar examples. Locale, currency, timezone, number precision, pluralization and translated messages must be explicit product decisions. Do not silently change enum values or assume the demo is localized.

Keyboard acceptance must include Tab/Shift+Tab, Enter/Space, menu/calendar navigation, Escape, initial focus, focus trap where modal, and return to the invoking control. **D/V:** Primitives supply defaults; actual compositions, contrast, zoom and screen-reader behavior remain unverified until tested.

<a id="resources"></a>
## Assets, Dependencies, and Business Boundaries

[index.html][html] loads remote Google Fonts, OS-theme-selected favicons, and an initial white theme-color. Mounted ThemeSwitch later updates that meta value for explicit dark/light, but system-dark still takes its white branch; favicon choice does not necessarily track manual app theme. [Assets][assets] contains local Logo/Clerk logos, brand icon components and configuration previews. Auth uses checked-in light/dark dashboard PNGs; public/images contains favicon variants and a dashboard image. Chat/avatar data includes remote images and paths such as `/avatars/shadcn.jpg`; these are not all bundled. NewChat's `/placeholder.svg` fallback is not a supplied public file.

Lucide is the primary general icon set; Radix Icons is also used in tables/settings. Upstream credits Tabler for brand icons, which are local source components, not an installed `@tabler/icons-react` dependency. Preserve source artwork and fallback semantics without assuming URLs are always available.

The repository's original documentation is MIT, Copyright (c) 2026 turtoncarllyle. [Upstream LICENSE][upstream-license] is MIT for upstream code. Brand marks, external avatars and hosted fonts may have separate terms; this audit does not certify all asset/font licensing or grant trademark rights. **R:** Check each actual asset/font license before redistribution or self-hosting. No upstream application source or image is redistributed by this documentation repository.

The `_authenticated` route is a shell, not by itself a security boundary. Mock auth and role/status displays do not implement production access control. Preserve the consuming application's router, search schema, row identifiers, RHF/Zod models, Query error behavior, authentication and authorization contracts. Do not "fix" a UI by dropping validation or replacing request handlers with demo toasts.

<a id="coverage"></a>
## Coverage and Audit Record

This matrix evaluates **documentation coverage after the static audit**, not working product features or browser certification. Complete means the relevant source pattern and its limits are described; it never implies production readiness.

| Module | Prior documentation | Current coverage | Evidence / remaining limit |
| --- | --- | --- | --- |
| Purpose, prompts, version selection | Complete | Complete | Scope, baseline, README; current vs original snapshot now explicit |
| Semantic colors and aliases | Mostly complete | Complete | theme.css role-to-mode values retained; explicit exceptions added |
| Typography, motion, elevation | Partial | Complete | Body size, control exceptions, pinned default 150ms and local layers |
| Shell and navigation | Partial | Complete | Actual 48/64/66px collapse geometry, none/mobile exception, menus |
| All 30 UI primitives and shared controls | Partial | Complete | Component contracts + UI source; no invented installed primitives |
| Tables, filters, pagination, bulk | Partial / inaccurate | Complete | URL/local state separation, contained overflow, confirmation scope |
| Dashboard and charts | Partial / inaccurate | Complete | Real series, heights, no Tooltip/Legend/export implementation |
| Tasks and Users | Partial / inaccurate | Complete | Sheet vs Dialog, validation, import/demo mutation boundaries |
| Apps and Chats | Partial | Complete | Grid/chat breakpoint, absent integration/send handlers |
| Auth, five settings pages, errors/help | Partial | Complete | Actual handlers, persistence, placeholders and recovery |
| Themes/RTL and third-party widgets | Partial | Partial | Source rules complete; initial paint, Clerk and dynamic theme need runtime |
| Accessibility/content | Partial | Partial | Concrete source gaps documented; no WCAG certification |
| Real requests, uploads, delivery, server permissions | Not applicable | Not applicable | Not supplied by this demo; extension requirements only |
| Generic tree, breadcrumb, route tabs, detail page, Drawer, Dropzone, Chart wrapper | Not applicable | Not applicable | Absent; tasks "drawer" is Sheet, account combobox is composition |
| Bilingual parity / links / provenance | Partial | Complete | Same sections, facts and values; immutable source references |
| Browser rendering / assistive technology | Unverified | Unverified | No app run, screenshots or runtime acceptance in this documentation update |

Priority findings and the changes applied to this specification:

| Priority | Former problem / evidence | Impact and correction | Verification |
| --- | --- | --- | --- |
| P1 | Old coverage matrix treated chart tooltip/legend, task dialogs, CRUD/import/chat and settings persistence as implemented; see page handlers linked above | Could generate incorrect flows. Replaced generic promises with exact composition and demo boundaries | Read actual JSX and submit/click handlers, not keywords alone |
| P1 | Old accessibility/theme sections implied working bypass, inherited third-party themes and flash prevention; see SkipToMain, ThemeProvider, Clerk route | Could hide known usability gaps. Separated S/D/R/V and added focused runtime criteria | Static checks recorded; browser proof remains V |
| P2 | Old 14px body, universal 16px Select, uniform focus/200ms, accent table rows and current item range | Could produce wrong density/states. Corrected against base CSS, primitives and pinned Tailwind defaults | Exact token mapping, class/element and breakpoint comparison |
| P2 | Old 48px collapsed width applied to every variant; only 768px boundary emphasized | Could mis-size inset and chat/pagination. Added variant geometry and distinct viewport/container tables | Sidebar formulas, chat sm and pagination container classes |
| P2 | Dependencies recorded mainly as major versions; latest component behavior could leak in | Added exact lock resolutions, local overrides, source links, absent-component boundary | Match package/importer and installed source inventory |
| P3 | Original Release snapshot and maintained same-version docs could be confused | Same 2.2.1 maintained on main; keep historical tag/assets and use commit pins for reproducibility | Check README URLs, remote main and original tag/assets |

<a id="acceptance"></a>
## Acceptance and Same-Version Maintenance

This is a correction/enrichment of **2.2.1**, not an upstream upgrade or a new numbered revision. `versions/2.2.1` and YAML `version: "2.2.1"` stay unchanged. Current documents are maintained on main; Git commits record changes. Existing `v2.2.1` and its original Release assets remain the initial publication snapshot, not an automatically updated copy of main. Use a full commit SHA to pin a particular maintained document.

Document checks for this update:

1. Strict UTF-8 decoding; valid front matter with version/name/description/colors/typography; same bilingual section anchors, semantic rules, dimensions and token mappings.
2. Compare all 23 direct colors per mode and eight aliases to theme.css, not just occurrences of OKLCH strings. Verify dependency resolutions and all referenced source paths against the pinned tree.
3. Check component/page/state evidence, relative links, immutable upstream links, version index, current-download vs snapshot wording and license attribution.
4. Confirm only the six repository files are tracked and the local upstream snapshot remains ignored; keep tag/assets unchanged and verify pushed main content.

Separate product/runtime acceptance, **not performed by this documentation update**:

1. Render target pages at narrow/mobile/desktop widths, plus 640/768/1024 viewport and 448/672/768/896/1280 content thresholds; inspect overflow, form grid, table sticky columns and pagination labels.
2. Test light/dark/system, OS changes while open, font fallback, RTL, zoom, reduced motion, Dialog/Sheet/Command focus and background scrolling.
3. Exercise keyboard bypass, names/error associations, full long-text access and screen-reader announcements; measure contrast for tokens, statuses and charts.
4. For real integrations, test credential/permission failures, pending/duplicate submits, empty data, retries, file validation, batch scope and recovery with controlled data. Clerk needs configured credentials and service access.

No dependency installation, application build, server run, screenshots or UI conformance claim is part of this static documentation audit. Any untested runtime item remains V.

<a id="agent-guide"></a>
## Agent Prompt Guide

```text
Read DESIGN.md for shadcn-admin 2.2.1 before changing this interface.
Treat S as pinned source facts, D as pinned dependency behavior, R as completion
rules, and V as items requiring verification. Reuse local components, semantic
OKLCH tokens, source-specific density, the 10px base radius system and actual
page/viewport/container rules. Preserve routing, schemas, permissions and local
RTL customizations. Do not invent missing APIs, treat demo toasts as mutations,
or overwrite components from the current shadcn registry. Identify applicable
states and source exceptions, implement only the requested product behavior,
and report static checks separately from browser/runtime validation.
```

<a id="known-gaps"></a>
## Known Gaps and Non-Goals

Upstream explicitly says it is not a starter template. This specification is sufficient context for source-faithful page styling/composition, but not a replacement for local component APIs, shadcn/ui documentation, application code, or product/backend design. Mock-data boundaries, source defects and unverified third-party/accessibility behavior remain as described above. Future upstream versions need separate evidence; they must not silently replace the 2.2.1 baseline.

This independent work is not an official shadcn-admin or shadcn/ui specification or endorsement.

<a id="sources"></a>
## Sources

All `src` references below resolve to the same immutable upstream commit, not the moving default branch. The [upstream release](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1) identifies the application version. The [live demo](https://shadcn-admin.netlify.app/), [shadcn/ui docs](https://ui.shadcn.com/), and [DESIGN.md overview](https://stitch.withgoogle.com/docs/design-md/overview/) are contextual references only.

[upstream]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434
[lock]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/pnpm-lock.yaml
[package]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/package.json
[config]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/components.json
[theme]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/styles/theme.css
[base-css]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/styles/index.css
[html]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/index.html
[font-provider]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/context/font-provider.tsx
[main]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout/main.tsx
[shell]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout/authenticated-layout.tsx
[sidebar]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/sidebar.tsx
[header]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout/header.tsx
[nav]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout/nav-group.tsx
[nav-data]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout/data/sidebar-data.ts
[ui]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui
[button]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/button.tsx
[form]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/form.tsx
[calendar]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/calendar.tsx
[date-picker]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/date-picker.tsx
[confirm]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/confirm-dialog.tsx
[command-menu]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/command-menu.tsx
[command]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/command.tsx
[sonner]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/ui/sonner.tsx
[root-route]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/routes/__root.tsx
[progress]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/navigation-progress.tsx
[data-table]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/data-table
[tasks-table]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/tasks/components/tasks-table.tsx
[users-table]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/users/components/users-table.tsx
[url-state]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/hooks/use-table-url-state.ts
[bulk]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/data-table/bulk-actions.tsx
[dashboard]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/dashboard/index.tsx
[overview]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/dashboard/components/overview.tsx
[analytics-chart]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/dashboard/components/analytics-chart.tsx
[tasks]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/tasks
[task-sheet]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/tasks/components/tasks-mutate-drawer.tsx
[task-import]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/tasks/components/tasks-import-dialog.tsx
[users]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/users
[user-data]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/users/data/data.ts
[user-dialog]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/users/components/users-action-dialog.tsx
[user-invite]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/users/components/users-invite-dialog.tsx
[apps]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/apps/index.tsx
[chats]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/chats/index.tsx
[new-chat]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/chats/components/new-chat.tsx
[auth]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/auth
[settings]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/settings
[appearance]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/settings/appearance/appearance-form.tsx
[clerk]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/routes/clerk/route.tsx
[errors]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/features/errors
[entry]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/main.tsx
[mobile]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/hooks/use-mobile.tsx
[theme-provider]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/context/theme-provider.tsx
[direction]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/context/direction-provider.tsx
[layout-provider]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/context/layout-provider.tsx
[config-drawer]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/config-drawer.tsx
[skip]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/skip-to-main.tsx
[long-text]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/long-text.tsx
[assets]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/assets
[upstream-license]: https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/LICENSE
[tw-theme]: https://unpkg.com/tailwindcss@4.1.14/theme.css
[shared]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components
[layout-components]: https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/src/components/layout
