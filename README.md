# shadcn-admin-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![shadcn-admin](https://img.shields.io/badge/shadcn--admin-2.2.1-0f172a)](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-64748b)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-0f172a)](LICENSE)

面向 AI 编码 Agent、按上游版本维护的 shadcn-admin 后台设计系统文档。

`DESIGN.md` 是 Google Stitch 提出的设计系统文档格式：用纯文本 Markdown 记录设计系统，让 AI 编码 Agent 能够生成风格一致的界面。它描述视觉结果、组件组合、交互状态和响应式规则，不替代 shadcn/ui 组件文档、业务逻辑或应用源码。

本仓库从 [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin) 官方版本源码提取真实的语义颜色、字体、尺寸、间距、圆角、阴影、布局和交互状态，并按 shadcn-admin 版本独立维护。这里的 `2.2.1` 指 shadcn-admin 项目版本，不是通用 shadcn/ui 版本。

## DESIGN.md 的目的与用途

这份规范把 shadcn-admin 2.2.1 中分散在 Tailwind 类、CSS 变量、Radix 组件和页面组合里的视觉决策，整理成 AI Agent 可以直接读取的上下文。它适合以下工作：

- 新建后台页面时复用同一套壳层、密度、令牌和组件组合
- 修改现有页面时避免引入另一套仪表盘视觉语言
- 评审亮色、暗色、RTL、移动端和交互状态的一致性
- 升级 shadcn-admin 时明确选择与源码版本匹配的规范

## 设计场景

本仓库服务后台 Admin 管理系统，而不是官网或营销落地页。规范重点覆盖：

- 仪表盘、指标、图表和近期活动
- 用户、任务、应用等数据列表、筛选、排序、分页和批量操作
- 登录、注册、设置、资料和权限相关表单
- 侧栏、顶栏、命令搜索、面包屑、标签页和响应式导航
- Dialog、Sheet、Dropdown、通知、加载、空态和错误页
- 亮色/暗色主题、字体切换、RTL 和布局配置

## 支持版本

| shadcn-admin 版本 | 英文标准版 | 简体中文版 | GitHub Release |
| --- | --- | --- | --- |
| `2.2.1` | [DESIGN.md](versions/2.2.1/DESIGN.md) | [DESIGN.zh-CN.md](versions/2.2.1/DESIGN.zh-CN.md) | [v2.2.1](https://github.com/turtoncarllyle/shadcn-admin-design-md/releases/tag/v2.2.1) |

英文 `DESIGN.md` 是生态兼容的标准版本；中文版本保持相同的章节、令牌、数值和规则。

## 使用方法

1. 选择与项目所用 shadcn-admin 版本一致的目录。
2. 将英文版或中文版下载到项目根目录并命名为 `DESIGN.md`。
3. 告诉 AI 编码 Agent 在生成或修改后台界面时严格遵循该文件。

Windows PowerShell 下载英文标准版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

下载简体中文版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

示例提示词：

```text
请读取项目根目录的 DESIGN.md，并严格按照 shadcn-admin 2.2.1 的语义令牌、
后台壳层、组件组合、信息密度、暗色模式和响应式规则实现这个页面。
不要引入另一套视觉语言，也不要用最新版 shadcn CLI 覆盖本地定制组件。
```

## 规范覆盖范围

- 亮色与暗色 OKLCH 语义令牌及图表色
- Inter、Manrope 和系统字体层级
- 后台壳层、三种侧栏样式、折叠模式和内容容器
- 间距、圆角、边框、阴影和动效
- 按钮、表单、卡片、图表、标签页、表格和分页
- 导航、命令菜单、Dialog、Sheet、Dropdown 和反馈状态
- 768px 移动边界、RTL、键盘导航和可访问性要求
- 面向 AI Agent 的 Do / Don't、提示词和验收清单

## 版本与来源

当前规范基于 [shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)，主要事实来源为该版本的 `components.json`、`src\styles\theme.css`、`src\styles\index.css`、`src\components\ui\` 和 `src\components\layout\`。

上游项目使用 Vite、React 19、Tailwind CSS 4、Radix UI、TanStack Router/Table、Lucide 和 shadcn/ui `new-york` 风格。上游明确说明它不是 starter template，并对部分组件做了 RTL 或其他定制；使用本规范时应以项目内源码为准。

本仓库是独立维护的设计系统文档，不隶属于 shadcn-admin 或 shadcn/ui，也不代表其官方认可。相关名称和标识归各自权利人所有。

## 许可证

本仓库原创文档使用 [MIT License](LICENSE)。上游 shadcn-admin 源码仍适用其自身的 [MIT License](https://github.com/satnaing/shadcn-admin/blob/v2.2.1/LICENSE)。
