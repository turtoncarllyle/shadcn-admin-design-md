# shadcn-admin-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![shadcn-admin](https://img.shields.io/badge/shadcn--admin-2.2.1-0f172a)](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-64748b)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-0f172a)](LICENSE)

A versioned shadcn-admin design-system document for AI coding agents.

`DESIGN.md` is a design-system document format introduced by Google Stitch. It records a design system in plain-text Markdown so AI coding agents can generate visually consistent interfaces. It defines visual results, component composition, interaction states, and responsive rules; it does not replace shadcn/ui component documentation, business logic, or application source code.

This repository extracts real semantic colors, typography, dimensions, spacing, radii, shadows, layouts, and interaction states from official [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin) releases and maintains them separately by shadcn-admin version. `2.2.1` here is the shadcn-admin project version, not a general shadcn/ui version.

## Purpose and Usage

This specification turns the visual decisions distributed across Tailwind classes, CSS variables, Radix components, and page compositions in shadcn-admin 2.2.1 into context that an AI agent can read directly. Use it to:

- Reuse the same shell, density, tokens, and component compositions on new admin pages
- Keep modifications from introducing a second dashboard visual language
- Review consistency across light, dark, RTL, mobile, and interaction states
- Select a specification that matches the source version when upgrading shadcn-admin

## Design Context

This repository targets operational admin systems rather than marketing websites. The specification focuses on:

- Dashboards, metrics, charts, and recent activity
- User, task, and application tables with filters, sorting, pagination, and bulk actions
- Sign-in, sign-up, settings, profile, and access-related forms
- Sidebars, headers, command search, breadcrumbs, tabs, and responsive navigation
- Dialogs, sheets, dropdowns, notifications, loading, empty, and error states
- Light/dark themes, font selection, RTL, and layout configuration

## Supported Versions

| shadcn-admin version | Canonical English | Simplified Chinese | GitHub Release |
| --- | --- | --- | --- |
| `2.2.1` | [DESIGN.md](versions/2.2.1/DESIGN.md) | [DESIGN.zh-CN.md](versions/2.2.1/DESIGN.zh-CN.md) | [v2.2.1](https://github.com/turtoncarllyle/shadcn-admin-design-md/releases/tag/v2.2.1) |

The English `DESIGN.md` is the canonical ecosystem-compatible edition. The Chinese edition preserves the same sections, tokens, values, and rules.

## Usage

1. Select the directory that matches the shadcn-admin version used by your project.
2. Download either language edition to your project root and name it `DESIGN.md`.
3. Tell your AI coding agent to follow the file whenever it generates or modifies admin UI.

Download the canonical English edition with Windows PowerShell:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

Download the Simplified Chinese edition:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

Example prompt:

```text
Read DESIGN.md in the project root. Implement this page using the semantic
tokens, admin shell, component compositions, information density, dark mode,
and responsive rules of shadcn-admin 2.2.1. Do not introduce another visual
language or overwrite local customized components with the latest shadcn CLI.
```

## Coverage

- Light and dark OKLCH semantic tokens and chart colors
- Inter, Manrope, and system font hierarchy
- Admin shell, three sidebar variants, collapse modes, and content container
- Spacing, radii, borders, shadows, and motion
- Buttons, forms, cards, charts, tabs, tables, and pagination
- Navigation, command menu, dialogs, sheets, dropdowns, and feedback states
- The 768px mobile boundary, RTL, keyboard navigation, and accessibility
- Agent-facing Do / Don't rules, prompt guidance, and acceptance checklist

## Version and Source

The current specification is based on [shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1). Its primary evidence is that release's `components.json`, `src\styles\theme.css`, `src\styles\index.css`, `src\components\ui\`, and `src\components\layout\`.

The upstream project uses Vite, React 19, Tailwind CSS 4, Radix UI, TanStack Router/Table, Lucide, and the shadcn/ui `new-york` style. Upstream explicitly states that it is not a starter template and that some components contain RTL or other customizations. Treat the checked-in project source as authoritative when using this specification.

This is an independently maintained design-system document. It is not affiliated with or endorsed by shadcn-admin or shadcn/ui. Related names and marks belong to their respective owners.

## License

Original documentation in this repository is released under the [MIT License](LICENSE). Upstream shadcn-admin source remains subject to [its own MIT License](https://github.com/satnaing/shadcn-admin/blob/v2.2.1/LICENSE).
