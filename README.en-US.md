# shadcn-admin-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![shadcn-admin](https://img.shields.io/badge/shadcn--admin-2.2.1-0f172a)](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-64748b)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-0f172a)](LICENSE)

面向 AI 编码 Agent 的 shadcn-admin 版本化 DESIGN.md。
Versioned shadcn-admin DESIGN.md specifications for AI coding agents.

## Purpose and Use Cases

`DESIGN.md` records a design system as Markdown context for AI agents. This repository extracts visual tokens, layouts, component compositions, interactions and responsive rules from a specified release of [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin). Use it to create admin pages, change existing interfaces and review visual consistency, not as a marketing-page template.

The goal is to let an agent understand the actual design language from the versioned specification while knowing which behaviors are demos or still require product implementation. It does not replace component APIs, application source, business logic, authentication/authorization, backend contracts or upstream shadcn/ui documentation.

`2.2.1` always means the **shadcn-admin application version**, not a universal shadcn/ui version or a document revision number.

## Versions and Reading Entry Points

| Upstream version | Current canonical English | Current Simplified Chinese | Initial publication snapshot |
| --- | --- | --- | --- |
| `2.2.1` | [DESIGN.md](versions/2.2.1/DESIGN.md) | [DESIGN.zh-CN.md](versions/2.2.1/DESIGN.zh-CN.md) | [v2.2.1 Release](https://github.com/turtoncarllyle/shadcn-admin-design-md/releases/tag/v2.2.1) |

English is the canonical ecosystem-compatible edition; Chinese keeps equivalent sections, facts, tokens and rules. Either DESIGN file is usable on its own as agent context. README is not a substitute for the complete specification.

Read [scope and evidence](versions/2.2.1/DESIGN.md#scope), [tokens](versions/2.2.1/DESIGN.md#colors) and [shell](versions/2.2.1/DESIGN.md#shell), then [components](versions/2.2.1/DESIGN.md#components) and the [relevant page](versions/2.2.1/DESIGN.md#pages), followed by [states](versions/2.2.1/DESIGN.md#states), [known gaps](versions/2.2.1/DESIGN.md#known-gaps) and [acceptance](versions/2.2.1/DESIGN.md#acceptance).

## Usage

1. Identify the application's actual upstream version and select the matching directory. Do not import latest dependency behavior into an old baseline.
2. Download either edition as `DESIGN.md` in the target project. The commands below overwrite that filename; confirm the target first.
3. Ask the agent to read the specification, then inspect the project's existing components, routes, schemas and business contracts.
4. Distinguish **S source facts, D dependency behavior, R recommendations and V unverified items**. A demo toast is not a real successful mutation.
5. Report static checks separately from actual browser validation; document checks are not UI acceptance.

Download the current canonical English edition with Windows PowerShell:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

Download the current Simplified Chinese edition:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

Example prompt:

```text
Read DESIGN.md first. Implement this task with shadcn-admin 2.2.1 semantic
OKLCH tokens, shell dimensions, local components, page density and actual
responsive rules. Separate source facts, dependency defaults, recommendations
and unverified items. Preserve routes, schemas, permissions and data APIs.
Do not replace business requests with demo toasts, overwrite customized
components from the latest shadcn registry, or invent missing functionality.
Identify relevant hover/focus/invalid/disabled/selected/loading/empty/error
states, and report the changes, source evidence and checks actually performed.
```

## Coverage and Audit Results

The maintained 2.2.1 specification covers:

- 23 direct color roles per mode, eight sidebar aliases, Inter/Manrope/system typography and fallback limits
- The 0.625rem base radius, spacing, shadows, stacking, default motion and local style exceptions
- Three sidebar variants and collapse combinations, fixed content, command search and preference persistence
- 30 installed UI primitives and shared controls, including buttons, forms, selection, dates, cards, Tabs, Dialog, Sheet and feedback
- Table filtering, sorting, pagination, sticky columns, URL/local state and bulk confirmation scope
- Dashboard, Tasks, Users, Apps, Chats, five auth pages, five settings pages, Clerk, errors and help placeholders
- Viewport rules including 640/768/1024px and separate named-container thresholds
- A state matrix, source limitations, asset/business boundaries, agent prompts and layered acceptance

The [coverage matrix and audit record](versions/2.2.1/DESIGN.md#coverage) records evidence, impact, priority, changes and verification. Corrections include the task-edit Sheet, real chart colors and absent Tooltip/Legend, mock import/chat/CRUD, settings persistence, native mobile inputs versus Radix Select, and sidebar/pagination breakpoints.

Limits remain: the demo lacks a complete backend, real integrations/message delivery and uniform async states. Skip-to-main targeting, some accessible names, system-theme context synchronization and history/filter synchronization have concrete source gaps. The specification now records them; **the upstream app was not changed and those defects are not claimed fixed**. Theme/RTL/Clerk, contrast, focus and actual narrow-screen rendering still need runtime validation.

## Same-Version Maintenance and Downloads

This work continues in `versions\2.2.1` with front matter `version: "2.2.1"`. It adds no numbered revision and upgrades no upstream dependency.

| Entry point | Meaning | Changes with document maintenance? |
| --- | --- | --- |
| `main/versions/2.2.1/` | Current enriched 2.2.1 specification; used by the commands above | Yes |
| File under a full Git commit SHA | A specific maintenance result for reproducible agent tasks | No |
| Tag `v2.2.1` / original Release assets | Initial publication snapshot, without subsequent main improvements | No; existing tag and assets are preserved |

To pin content, choose a commit from [history](https://github.com/turtoncarllyle/shadcn-admin-design-md/commits/main/versions/2.2.1/) and replace `main` in the download URL with its full SHA. Do not treat the original Release assets as the current maintained specification. Create a new version directory only for a different upstream application version; track same-version corrections through normal Git commits.

## Sources and Verification Limits

The fixed baseline is [satnaing/shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1), commit [0217f8cb73af66f3cbf4141d5e0d00e1c5c30434](https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434). Source links in the specification prefer that immutable commit; exact dependency versions come from its lockfile.

The 2026-09-08 static audit matched all 247 reference file blobs to upstream, then checked theme, components, routes, handlers and configuration. This update does not install dependencies, build/run the app or create a preview site. Static verification is not browser, screen-reader or UI acceptance.

The local `shadcn-admin-2.2.1\` snapshot is analysis-only and excluded by `.gitignore`. This repository tracks only two READMEs, two DESIGN files, LICENSE and .gitignore, not upstream application source.

Upstream explicitly says this is not a starter template and modifies some shadcn components for RTL or other behavior. The current [live demo](https://shadcn-admin.netlify.app/) and [shadcn/ui documentation](https://ui.shadcn.com/) are contextual references, not replacements for the pinned source.

## License and Independence

Original documentation is under the [MIT License](LICENSE), Copyright (c) 2026 turtoncarllyle. Upstream code retains [its own MIT License](https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/LICENSE). Fonts, avatars and brand marks may have separate terms; this specification grants no additional third-party asset or trademark rights.

This is independent work, not affiliated with or endorsed by shadcn-admin or shadcn/ui.
