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
  base_size: "14px on desktop controls and operational content"
  mobile_form_size: "16px below 768px"
---

# shadcn-admin 2.2.1 Design System

## Purpose and Scope

Use this document to create or modify operational admin interfaces in the visual language of [satnaing/shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1). It translates the checked-in theme variables, Tailwind classes, Radix primitives, and page compositions into an AI-readable design contract.

The version in this document is the shadcn-admin application version. It is not a universal shadcn/ui version or a promise that current registry components have the same API. The source inside the consuming project wins whenever it differs from current shadcn/ui documentation.

This system is for dashboards, data management, settings, authentication, chat, and other repeated-work surfaces. It is not a marketing-site style. Preserve application behavior, routing, data contracts, permissions, and local component customizations while applying these visual rules.

## Design Principles

1. **Operational clarity.** Optimize for scanning, comparison, and repeated action. Headings orient the user; they do not dominate the viewport.
2. **Semantic theming.** Use named tokens such as `background`, `foreground`, `primary`, `muted`, `accent`, and `destructive`. Do not replace them with arbitrary palette utilities.
3. **Quiet structure.** Build hierarchy with spacing, thin borders, restrained shadows, and type weight. Avoid decorative containers and visual noise.
4. **Component composition.** Reuse the project's installed shadcn/Radix components and their variants. Compose pages from known primitives before creating custom surfaces.
5. **Complete states.** Every workflow accounts for hover, focus, active, selected, expanded, disabled, loading, empty, invalid, and destructive states where applicable.
6. **Responsive and bidirectional by design.** Mobile navigation, RTL direction, logical spacing, keyboard access, and text overflow are part of the component contract.

## Technology Baseline

The visual contract is pinned to the v2.2.1 source snapshot:

| Area | Source baseline |
| --- | --- |
| Application | Vite 7, React 19, TypeScript 5.9 |
| Styling | Tailwind CSS 4, CSS variables, `new-york` component style |
| Primitives | Radix UI with local shadcn component source |
| Navigation and data | TanStack Router, Query, and Table |
| Forms | React Hook Form, Zod, Radix controls |
| Icons | Lucide; Tabler is limited to brand icons in upstream documentation |
| Charts | Recharts with five semantic chart tokens |
| Feedback | Sonner, alerts, skeletons, navigation progress |

Do not use this table as permission to upgrade dependencies. Version upgrades require their own source review and a new design-document version.

## Color System

### Core Semantic Tokens

Use the CSS custom properties from `src/styles/theme.css`. Components consume them through semantic utilities such as `bg-background`, `text-foreground`, `border-border`, and `ring-ring`.

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

The sidebar tokens alias the core roles: `sidebar` to `background`, `sidebar-foreground` to `foreground`, `sidebar-primary` to `primary`, `sidebar-accent` to `accent`, plus their foreground, border, and ring counterparts. Keep these aliases so alternate sidebar treatments remain theme-aware.

Use chart colors in stable series order and include labels, legends, or direct values; color alone must not carry meaning. Destructive red is not a general accent. Do not introduce a second brand hue merely to make a page feel more colorful.

## Typography

The appearance settings expose `Inter`, `Manrope`, and the system stack. Inter is the default operational choice; Manrope is an available alternate, not a display-font license for oversized headings.

| Role | Size and weight | Usage |
| --- | --- | --- |
| Page title | approximately 24-30px, bold/semibold | One concise title per page |
| Section title | 18-20px, semibold | Major unframed sections or card titles |
| Card title | 16px, semibold | Compact surface heading |
| Body/control | 14px, regular or medium | Tables, buttons, navigation, forms |
| Supporting text | 14px, muted | Descriptions and secondary values |
| Caption/label | 12px, medium | Group labels, badges, metadata |
| Mobile form text | 16px minimum below 768px | Prevent browser focus zoom |

Use `font-medium` or `font-semibold` to create hierarchy before increasing size. Preserve normal letter spacing; only command shortcuts may use wider tracking. Use tabular numerals for counters and aligned metrics. Truncate long single-line navigation labels and provide access to the full value when it matters.

## Spacing, Shape, and Elevation

Tailwind's 4px spacing rhythm is the base. Common page spacing is `1rem` horizontal and `1.5rem` vertical. Compact component interiors commonly use 8-12px gaps; major content groups use 16-24px. Prefer `gap-*` in flex and grid layouts so direction and wrapping remain predictable.

The root radius is `0.625rem` (10px):

| Token | Computed radius | Typical use |
| --- | --- | --- |
| `radius-sm` | 6px | Small command items and tight inner controls |
| `radius-md` | 8px | Buttons, inputs, navigation rows, badges |
| `radius-lg` | 10px | Dialogs, alerts, grouped controls |
| `radius-xl` | 14px | Cards |

Use thin `border-border` outlines and the component's existing `shadow-xs`, `shadow-sm`, or `shadow-lg`. Cards use a small shadow; dropdowns and dialogs can use stronger elevation because they are layered. Do not make normal page sections float. Do not combine a strong border, large shadow, tinted fill, and large radius on the same surface.

Normal focus uses a 3px `ring/50` halo plus the ring-colored border. Common state transitions are 200ms. Collapsible content uses 300ms ease-out; sheets use 300ms on close and 500ms on open. A fixed header adds a shadow and subtle background blur only after scrolling. Honor reduced-motion preferences by removing nonessential animation while preserving final states.

## Admin Shell and Layout

### Shell Dimensions

- Desktop sidebar: `16rem` (256px).
- Collapsed icon sidebar: `3rem` (48px).
- Mobile sheet sidebar: `18rem` (288px).
- Header: `4rem` (64px).
- Main content: `1rem` horizontal and `1.5rem` vertical padding.
- Non-fluid content: centered at the `@7xl/content` container threshold with `max-w-7xl` (80rem / 1280px).
- Full application height: `100svh` for fixed layouts; inset layouts subtract their surrounding spacing.

The default layout is an `inset` sidebar with `icon` collapse. Supported sidebar variants are `inset`, `sidebar`, and `floating`; supported collapse modes are `icon`, `offcanvas`, and `none`. Keep layout and collapse choices coherent rather than mixing visual fragments from different variants.

The header contains the sidebar trigger, separator, contextual navigation or search, theme/configuration actions, and user menu. The main area owns page title, primary action, view controls, and content. Avoid wrapping the entire main area in a decorative card.

Sidebar state, layout variant, collapse mode, theme, font, and direction are persistent user preferences in the source application. Do not reset them on route changes. Fixed layouts must contain their own scrolling regions and must not create a second page scrollbar.

## Component Language

### Buttons and Actions

Buttons are 14px medium text with icon/text gaps and a visible focus ring.

| Size | Dimensions |
| --- | --- |
| Default | 36px high, 16px horizontal padding |
| Small | 32px high, 12px horizontal padding |
| Large | 40px high, 24px horizontal padding |
| Icon | 36px square |

Use `default` for the principal action, `secondary` for an alternate, `outline` for neutral tools, `ghost` for low-chrome controls, `link` for inline navigation, and `destructive` only for irreversible or dangerous commands. A page normally has one strongest action. Disabled controls use 50% opacity and no pointer events. Loading buttons stay disabled and retain their label or accessible name while showing progress.

Use familiar Lucide symbols for icon actions. Let the local Button component control normal icon size. Supply accessible names and tooltips for icon-only controls whose meaning is not obvious.

### Forms and Selection Controls

Inputs are 36px high with 12px horizontal padding, an 8px radius, a thin input border, and a small shadow. Textareas use the same surface language with a 64px minimum height. At widths below 768px, input, select, and textarea text is forced to 16px to avoid browser focus zoom.

Labels are concise and visible. Descriptions use `muted-foreground`. Validation uses `aria-invalid`, a destructive border, and a 3px destructive-tinted ring; errors sit next to the field they describe. Disabled controls remain legible but visibly unavailable. Preserve the source React Hook Form and Zod composition rather than inventing a parallel form system.

Checkboxes, radio groups, switches, selects, calendars, OTP fields, and password inputs use the installed Radix/shadcn components. Keep checked, unchecked, open, selected, invalid, and disabled states. Group related controls under a clear label, and never use placeholder text as the only label.

### Cards, Metrics, and Charts

Cards use `card`/`card-foreground`, a border, 14px radius, 24px vertical padding, 24px internal group gap, and `shadow-sm`. Use full composition: header for title/description/action, content for the main value or visualization, and footer for supporting actions when needed.

Metric cards place the label and optional icon first, the value as the strongest element, and trend/context beneath it. Charts share the card surface and semantic chart palette. Axes, tooltips, legends, empty states, and loading states must remain readable in both themes.

Use cards for distinct repeatable objects or bounded tools. Do not place cards inside cards or convert every page section into a floating tile. A border, separator, or unframed content group is usually sufficient for secondary hierarchy.

### Data Tables and Pagination

Tables use a full-width semantic surface. Headers are 40px high with 8px horizontal padding, medium weight, and no wrapping. Cells use 8px padding and aligned, non-wrapping operational content. Rows have a bottom border, an accent hover state, and a distinct selected state.

Keep filters, search, column visibility, primary actions, and bulk actions close to the table. Sorting and filtering controls must expose state, not only icon color. Pagination includes page numbers, page size, current range, and previous/next controls as supported by the source. Bulk actions appear only when rows are selected and destructive bulk actions require confirmation.

TanStack Table state may synchronize with URL search parameters. Preserve that behavior so refresh, history, and shared links remain useful. On narrow screens, prioritize columns, offer row detail or a contained table viewport, and prevent the whole page from scrolling horizontally.

### Navigation and Sidebar

Sidebar groups use 12px medium labels with muted foreground. Standard menu buttons are 32px high, 14px text, 8px padding, 8px radius, and a 16px icon. Hover, active, open, and selected states use sidebar accent roles. Submenus use a logical-side border and indentation, not a new card.

The expanded sidebar shows labels, groups, badges, and user information. The icon variant collapses to 48px and uses tooltips for hidden labels. Mobile navigation is a Sheet, not a compressed desktop rail. The sidebar toggles with `Ctrl/Cmd+B`; do not intercept the shortcut inside text-editing workflows without preserving expected input behavior.

Top navigation remains compact and can collapse to a mobile dropdown. Long titles and user values truncate without moving adjacent controls. Use logical `start`/`end`, `ms`/`me`, and border-inline utilities so layout works in RTL.

### Tabs, Badges, and Avatars

Tabs use a 36px muted list surface with 3px inner padding. Active triggers use the background surface and a small shadow; inactive text is muted. Keep triggers inside one TabsList and disable unavailable views explicitly.

Badges are compact 12px labels with an 8px radius, thin border where applicable, and default, secondary, destructive, or outline variants. Use badges for status or count, not as general-purpose buttons. Avatars always include a fallback initial or short name and preserve a stable size.

### Dialogs, Sheets, Menus, and Popovers

Dialog overlays use 50% black. Dialog content is centered, bordered, padded 24px, rounded 10px, and shadowed; the default maximum width is `sm:max-w-lg` with 1rem viewport margins. Headers contain a title and optional description. Footers stack in reverse order on mobile and align actions to the end on larger screens.

Sheets slide from the requested edge, use 75% width for side panels, and cap at `sm:max-w-sm`. Mobile sidebars and configuration panels use this pattern. Dropdowns, selects, tooltips, and popovers use the popover tokens, restrained elevation, collision-aware placement, and keyboard navigation supplied by Radix.

Every Dialog and Sheet needs an accessible title, even when visually hidden. Destructive confirmations state the affected object and consequence. Do not use a modal for content that can remain in normal page flow.

### Command Search

The global command palette is a Dialog containing the installed `cmdk` primitives. Its search area is 48px high in the dialog composition; the result list has a 300px maximum height. Groups use 12px muted headings; items use 14px text, compact padding, semantic selected state, icons, and optional shortcuts.

Support `Ctrl/Cmd+K` to open, arrow keys to move, Enter to activate, and Escape to close. Display an explicit no-results state. Search results mirror navigable application destinations and preserve active/nested context.

### Feedback, Loading, Empty, and Error States

Use Sonner for transient notifications, Alert for in-flow messages, Skeleton for structural loading, and the navigation progress bar for route changes. Alerts carry `role="alert"`; routine nonurgent status updates should use polite live-region behavior when custom feedback is needed.

Loading states reserve final layout dimensions. Empty states state what is missing and offer the most relevant next action. Error states distinguish recoverable validation, authorization, not-found, and server failures. Destructive feedback uses the destructive role; success must not invent a permanent green brand system outside a purposeful status treatment.

Authentication and settings screens use the same controls, tokens, and spacing as the main application. Chat keeps the conversation region readable, the composer stable, and mobile borders/radii responsive. Do not make utility pages look like unrelated marketing pages.

## Complete Source Coverage Matrix

The following inventory is normative for this version. An implementation may use only the components needed by its workflow, but it must not substitute a different visual language when one of these source patterns applies.

### Installed UI Primitives

| Source components | Required visual and state coverage |
| --- | --- |
| `alert`, `alert-dialog` | In-flow status and destructive confirmation; title, description, default/destructive roles, open/closed, focus trap, cancel/confirm, disabled and pending actions |
| `avatar`, `badge` | Stable identity fallback and compact status/count; image failure, fallback, default/secondary/destructive/outline and interactive focus where linked |
| `button` | Default, destructive, outline, secondary, ghost and link variants; default/small/large/icon sizes; hover, focus, active, disabled and loading |
| `calendar`, `date-picker` | Month navigation, today, selected, range, outside, unavailable and disabled dates; popover open/closed, keyboard focus and formatted value |
| `card` | Header, title, description, action, content and footer composition; default, loading, empty and error content without nested cards |
| `checkbox`, `radio-group`, `switch` | Checked/unchecked/indeterminate where supported, hover, focus, disabled, invalid, label association and RTL alignment |
| `collapsible` | Trigger, open/closed state, rotating direction-aware indicator, 300ms content transition and reduced-motion result |
| `command` | Dialog, input, list, empty, group, item, separator and shortcut; query, selected, disabled, keyboard navigation and no results |
| `dialog`, `sheet` | Overlay, content, header, title, description, footer and close; open/closed, focus management, responsive actions and pending submission |
| `dropdown-menu`, `popover`, `select`, `tooltip` | Trigger/content composition, open/closed, highlighted, selected, checked, disabled, nested/submenu where provided, collision handling and keyboard control |
| `form`, `label`, `input`, `textarea`, `input-otp` | Default, hover, focus, filled, invalid, disabled, read-only and submitting; descriptions, messages, OTP caret/completion and mobile 16px text |
| `scroll-area`, `separator` | Direction-aware viewport/scrollbar and semantic visual division; overflow, hidden/visible scrollbar, horizontal/vertical orientation and RTL |
| `sidebar` | Provider, rail, inset, header, footer, content, group, menu, badge, action, submenu and skeleton; expanded/collapsed/offcanvas/mobile, active, hover, focus, open and loading |
| `skeleton`, `sonner` | Shape-preserving loading placeholders and transient notification variants; visible/dismissed, success/error/info where used, action and duration semantics |
| `table` | Header, body, footer, row, head, cell and caption; hover, selected, sorted context, empty, loading, overflow and RTL alignment |
| `tabs` | Root, list, trigger and content; active/inactive, hover, focus, disabled and responsive overflow |

### Shared, Layout, and Data Components

| Source area | Components and expectations |
| --- | --- |
| Application shell | `app-sidebar`, `app-title`, `authenticated-layout`, `header`, `main`, `nav-group`, `nav-user`, `team-switcher`, `top-nav`; cover all three sidebar variants, all collapse modes, persistent state, mobile Sheet, active route, nested route, account menu and long labels |
| Data table system | `bulk-actions`, `column-header`, `faceted-filter`, `pagination`, `toolbar`, `view-options`; cover search, reset, sort direction, filter selection, column visibility, row selection, page size, page numbers, previous/next, loading, empty and destructive bulk confirmation |
| Global utilities | `command-menu`, `config-drawer`, `confirm-dialog`, `date-picker`, `navigation-progress`, `search`, `select-dropdown`, `sign-out-dialog`, `theme-switch`; cover trigger/open/close, keyboard access, theme/direction/layout preview, pending navigation and destructive sign-out |
| Content utilities | `coming-soon`, `learn-more`, `long-text`, `password-input`, `profile-dropdown`, `skip-to-main`; cover unavailable feature messaging, external/help action, truncation, password reveal, avatar fallback, account actions and keyboard bypass navigation |

### Feature and Page Families

| Feature/page family | Required coverage |
| --- | --- |
| Dashboard | Overview and analytics tabs, metric cards, chart cards, recent sales, download action, chart tooltip/legend, disabled tabs, loading and no-data states |
| Tasks | Search, status/priority filters, sortable and hideable columns, selection, pagination, import/create/edit/delete dialogs, row and bulk actions, validation, submitting and empty results |
| Users | Search and filters, table selection, invite/create/edit/delete flows, row and bulk actions, status/role badges, validation, pending requests and empty results |
| Apps | Search/filter result grid, integration cards, connected/not-connected state, empty results, long descriptions and responsive card layout |
| Chats | Conversation list, search, selected conversation, unread/count state, message history, composer, new-chat dialog, scrolling, empty conversation and mobile split-view behavior |
| Authentication | Sign in, alternate sign-in layout, sign up, forgot password and OTP; default, focus, invalid credentials, validation, password reveal, submitting, success and recovery paths |
| Clerk integration | Clerk sign-in, sign-up, authenticated boundary and user management remain visually integrated without restyling third-party behavior beyond supported theming hooks |
| Settings | Profile, account, appearance, notifications and display forms; dirty/pristine, validation, saving, saved feedback, theme, font, direction and display preferences |
| Error handling | Authenticated error route plus 401, 403, 404, 500 and 503 pages; clear status, concise explanation, safe primary recovery action and optional secondary navigation |
| Help and unavailable routes | Help center and coming-soon surfaces use the normal shell, clear next steps and no marketing-style hero composition |

Every page family must include its relevant responsive, dark, RTL, loading, empty, error, disabled, and keyboard states even when the upstream demo uses mock data and does not visibly exercise every state.

## Interaction State Matrix

| State | Required treatment |
| --- | --- |
| Hover | Semantic accent or controlled opacity change; no layout shift |
| Focus visible | Ring-colored border plus 3px ring; keyboard-visible and unobscured |
| Active/pressed | Immediate feedback without changing control dimensions |
| Selected | Accent surface and foreground, plus state semantics where needed |
| Expanded/open | Clear trigger state and correctly oriented chevron |
| Checked/indeterminate | Control mark, semantic state, associated label, and keyboard focus |
| Current route | Active navigation surface plus `aria-current` or equivalent semantics |
| Sorted/filtered | Visible direction or filter summary with a clear reset path |
| Disabled | No interaction, 50% opacity, preserved readable label |
| Loading | Stable dimensions, progress indication, duplicate action prevented |
| Submitting/saving | Inputs remain understandable, submit repeats are blocked, completion or failure is announced |
| Empty | Clear message and context-appropriate next action |
| Invalid | `aria-invalid`, destructive border/ring, adjacent explanation |
| Destructive | Destructive token, explicit consequence, confirmation when irreversible |

Do not communicate a state by color alone. Preserve text, icon, ARIA state, or structural cues as appropriate.

## Responsive Behavior

The primary mobile boundary is 768px: `useIsMobile()` treats widths below 768px as mobile.

### Below 768px

- Replace the desktop sidebar with an 18rem Sheet and close it after navigation.
- Keep header actions compact; move lower-priority navigation into menus.
- Use 16px form text and full-width controls where needed.
- Stack dialog footers, card grids, filters, and form columns in task order.
- Keep primary actions reachable without overlapping titles or search.
- Prioritize table columns and provide detail access; never force page-wide horizontal scrolling.

### At and Above 768px

- Use the selected desktop sidebar variant and collapse mode.
- Keep the 64px header and align contextual tools on one stable row when space permits.
- Build dashboard and form layouts with responsive grids, not fixed pixel columns.
- Center non-fluid content at large container widths and cap it at 1280px.

Use container queries for content-dependent layout, as the authenticated inset marks the content region with `@container/content`. Test long labels, localized text, RTL, zoom, and narrow windows. Stable control and grid dimensions must prevent loading or hover states from shifting the interface.

## Dark Mode and Personalization

Dark mode is a full semantic token swap under `.dark`, not a collection of per-component hard-coded overrides. Components should automatically adapt through the same token names. Cards are slightly separated from the dark background; popovers are more elevated; borders and inputs use translucent white.

The source supports light, dark, and system theme behavior, plus Inter/Manrope/system fonts, LTR/RTL direction, sidebar variant, and collapse mode. Configuration controls preview these choices without changing the application's basic hierarchy. Persist user choices and avoid flashes of the wrong theme during initialization.

Do not infer that a custom raw color is safe because it looks acceptable in light mode. Verify text, icons, borders, focus, charts, overlays, and disabled states in both themes.

## RTL and Accessibility

Use logical positioning and spacing throughout. Chevron direction, sheet sides, submenu borders, table alignment, switches, dialogs, selects, and navigation must follow document direction. The v2.2.1 source specifically customizes `scroll-area`, `sonner`, and `separator`, and includes RTL updates for `alert-dialog`, `calendar`, `command`, `dialog`, `dropdown-menu`, `select`, `table`, `sheet`, `sidebar`, and `switch`. Preserve and manually merge those files during upgrades.

Keep the source `Skip to Main` link, landmark structure, labels, accessible names, Radix keyboard behavior, visible focus, dialog titles, and avatar fallbacks. Icon-only buttons need names; unfamiliar icons need tooltips. Ensure zoom and long translated content do not overlap or clip controls.

The source is designed with accessibility in mind, but this document does not certify conformance. Validate real workflows with keyboard navigation, screen-reader semantics, contrast checks, and responsive zoom when the consuming product requires formal compliance.

## Do's and Don'ts

### Do

- Use installed project components and their existing variants before creating custom UI.
- Use semantic CSS variables and utilities for every theme-sensitive color.
- Use `cn()` for conditional class composition and logical properties for direction.
- Keep admin pages compact, scan-friendly, and predictable.
- Keep filters and actions close to the data they affect.
- Preserve URL-backed table state, loading states, confirmation flows, and keyboard shortcuts.
- Preserve all local RTL/custom component changes when applying upstream updates.
- Check the local component source before following current shadcn/ui examples.

### Don't

- Do not turn the interface into a gradient-heavy, oversized, floating-card SaaS landing page.
- Do not hard-code slate, white, black, or destructive colors where a semantic token exists.
- Do not nest cards or wrap every section in a decorative container.
- Do not use giant headings or excessive empty space in operational screens.
- Do not hide state, validation, or destructive consequences behind color alone.
- Do not replace mobile navigation with a permanently squeezed desktop sidebar.
- Do not run the latest shadcn CLI with overwrite against customized v2.2.1 components.
- Do not treat this application release number as a shadcn/ui registry version.

## Agent Prompt Guide

Use this compact instruction when delegating UI work:

```text
Build this operational interface in the shadcn-admin 2.2.1 visual language.
Use the installed local shadcn/Radix components and semantic OKLCH tokens. Keep
the 14px work-focused density, 10px base radius system, thin borders, restrained
shadows, 64px header, responsive sidebar, dark mode, RTL, visible focus, and all
loading/empty/invalid/disabled states. Do not add marketing composition, raw
theme colors, nested cards, or overwrite customized components from the latest
shadcn registry.
```

Before finishing, compare the result against this checklist:

1. Does the page look like the v2.2.1 application rather than a generic dashboard?
2. Are semantic tokens used consistently in both light and dark modes?
3. Are sidebar, header, content width, density, radii, borders, and shadows source-faithful?
4. Are component compositions and every relevant interaction state complete?
5. Does the page remain usable below 768px, in RTL, at zoom, and with long text?
6. Are keyboard focus, accessible names, Dialog/Sheet titles, and error semantics present?
7. Were local component customizations preserved?

## Known Gaps and Source Boundaries

- shadcn-admin explicitly describes itself as a reusable dashboard collection, not a starter project. This document does not turn it into an application template.
- The specification is pinned to shadcn-admin v2.2.1. Current shadcn/ui registry APIs, presets, tokens, or component structure may differ.
- Some components are intentionally modified for RTL or project-specific behavior. Automated overwrite is unsafe without a per-file diff and manual merge.
- The document describes visual and interaction expectations; it does not define routing, authentication, authorization, API contracts, data models, or product-specific validation.
- Additional upstream pages and options may contain localized exceptions. Consult the checked-in v2.2.1 source before changing component markup or behavior.
- Accessibility requirements may require further product-specific testing and improvements. Make those improvements without erasing the source visual language.
- This is an independent source analysis, not an official shadcn-admin or shadcn/ui specification or endorsement.

## Sources

- [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin)
- [shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
- [shadcn-admin live demo](https://shadcn-admin.netlify.app/)
- [shadcn/ui documentation](https://ui.shadcn.com/)
- [Google Stitch DESIGN.md overview](https://stitch.withgoogle.com/docs/design-md/overview/)

Source values in this document were derived primarily from `components.json`, `src/styles/theme.css`, `src/styles/index.css`, `src/components/ui/`, `src/components/layout/`, and the v2.2.1 changelog.
