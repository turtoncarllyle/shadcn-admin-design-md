# shadcn-admin-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![shadcn-admin](https://img.shields.io/badge/shadcn--admin-2.2.1-0f172a)](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-64748b)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-0f172a)](LICENSE)

面向 AI 编码 Agent 的 shadcn-admin 版本化 DESIGN.md。
Versioned shadcn-admin DESIGN.md specifications for AI coding agents.

## 目的与适用场景

`DESIGN.md` 用 Markdown 将设计系统整理为 AI 可读取的上下文。本仓库从 [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin) 指定版本源码提取视觉令牌、布局、组件组合、交互和响应式规则，适合新建后台页面、修改现有界面和评审视觉一致性，不是营销落地页模板。

目标是让 Agent 在读取指定版本规范后理解该后台的实际设计语言，同时明确哪些只是演示、哪些仍需要产品实现。规范不替代组件 API、应用源码、业务逻辑、认证权限、后端数据契约或上游 shadcn/ui 文档。

`2.2.1` 始终指 **shadcn-admin 应用版本**，不是通用 shadcn/ui 版本，也不是本文档的修订编号。

## 支持版本与读取入口

| 上游版本 | 当前英文标准版 | 当前简体中文版 | 首次发布快照 |
| --- | --- | --- | --- |
| `2.2.1` | [DESIGN.md](versions/2.2.1/DESIGN.md) | [DESIGN.zh-CN.md](versions/2.2.1/DESIGN.zh-CN.md) | [v2.2.1 Release](https://github.com/turtoncarllyle/shadcn-admin-design-md/releases/tag/v2.2.1) |

英文版是生态兼容标准版，中文版保持等价的章节、事实、令牌和规则。单独下载任一 DESIGN 文件即可作为 Agent 上下文，README 不替代完整规范。

推荐阅读顺序：[范围与证据](versions/2.2.1/DESIGN.zh-CN.md#scope) → [令牌](versions/2.2.1/DESIGN.zh-CN.md#colors)与[壳层](versions/2.2.1/DESIGN.zh-CN.md#shell) → [组件](versions/2.2.1/DESIGN.zh-CN.md#components)和[对应页面](versions/2.2.1/DESIGN.zh-CN.md#pages) → [状态](versions/2.2.1/DESIGN.zh-CN.md#states)、[已知缺口](versions/2.2.1/DESIGN.zh-CN.md#known-gaps)与[验收](versions/2.2.1/DESIGN.zh-CN.md#acceptance)。

## 使用方法

1. 确认实际项目使用的上游版本，选择匹配目录，不要将最新版依赖行为混入旧规范。
2. 下载英文或中文规范到目标项目的 `DESIGN.md`。以下命令会覆盖同名文件，执行前确认目标。
3. 让 Agent 先阅读规范，再检查项目中已有的组件、路由、schema 和业务契约。
4. 区分规范中的 **S 源码事实、D 依赖行为、R 建议补充、V 待验证**；不能把模拟 Toast 当作真实提交成功。
5. 完成后分别报告静态核对和实际浏览器验证，不将文档检查当作 UI 验收。

Windows PowerShell 下载当前英文标准版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

下载当前简体中文版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/shadcn-admin-design-md/main/versions/2.2.1/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

如果 raw 下载域名超时，可使用 GitHub Contents API 获取相同文件，不需要安装依赖：

```powershell
Invoke-WebRequest `
  -Uri "https://api.github.com/repos/turtoncarllyle/shadcn-admin-design-md/contents/versions/2.2.1/DESIGN.md?ref=main" `
  -Headers @{ Accept = "application/vnd.github.raw+json" } `
  -OutFile ".\DESIGN.md"
```

获取中文时将 URL 文件名改为 `DESIGN.zh-CN.md`；固定内容时将 `ref=main` 改为所选完整提交 SHA。公开 API 受 GitHub 请求频率限制。

示例提示词：

```text
请先读取 DESIGN.md，按 shadcn-admin 2.2.1 的语义 OKLCH 令牌、壳层尺寸、
本地组件、页面密度和真实响应式规则实现本次需求。
区分源码已有事实、依赖默认行为、建议补充与待验证事项。
保留现有路由、schema、权限和数据接口，不用演示 Toast 替代业务请求，
不从最新版 shadcn 注册表覆盖定制组件，不虚构不存在的功能。
明确相关 hover/focus/invalid/disabled/selected/loading/empty/error 状态，
报告修改内容、源码依据和已实际执行的验证。
```

## 覆盖与审计结果

当前 2.2.1 规范覆盖：

- 两种模式各 23 个直接颜色角色、八个侧栏别名、Inter/Manrope/system 字体与回退边界
- 0.625rem 基础圆角、间距、阴影、层叠、默认动效和局部样式例外
- 三种侧栏样式及折叠组合、固定内容区、命令搜索和偏好持久化
- 30 个已安装 UI 原语及共享控件，包括按钮、表单、选择、日期、卡片、Tabs、Dialog、Sheet 和反馈
- 表格筛选、排序、分页、固定列、URL/本地状态和批量确认范围
- Dashboard、Tasks、Users、Apps、Chats、五个认证页面、五个设置页面、Clerk、错误和帮助占位
- 640/768/1024px 等视口规则，以及与之不同的具名容器断点
- 交互状态矩阵、源码限制、资源/业务边界、Agent 提示词和分层验收

[完整覆盖矩阵与审计记录](versions/2.2.1/DESIGN.zh-CN.md#coverage) 说明了问题证据、影响、优先级、改进及验证方式。此次重点修正了任务编辑 Sheet、图表实际配色与缺失 Tooltip/Legend、模拟导入/聊天/CRUD、设置持久化、移动原生输入与 Radix Select 的区别，以及侧栏和分页断点。

仍有限制：该演示没有完整后端、真实集成/消息送达或统一异步状态；Skip to Main 目标、部分可访问名称、系统主题上下文同步、历史筛选同步等有明确源码缺口。规范已记录这些问题，**没有修改上游应用，也不宣称它们已修复**。主题/RTL/Clerk、对比度、焦点与窄屏实际表现仍需运行验证。

## 同版本维护与下载策略

本次继续维护 `versions\2.2.1`，front matter 仍为 `version: "2.2.1"`，不新增修订版本号、不升级上游依赖。

| 入口 | 含义 | 是否随文档维护变化 |
| --- | --- | --- |
| `main/versions/2.2.1/` | 当前完善后的 2.2.1 规范，以上下载命令使用此入口 | 是 |
| 完整 Git 提交 SHA 下的文件 | 某次维护结果，适合可复现的 Agent 任务 | 否 |
| 标签 `v2.2.1` / 原 Release 附件 | 首次发布的历史快照，不包含后续 main 完善 | 否，保留原标签和附件 |

需要固定内容时，从 [提交历史](https://github.com/turtoncarllyle/shadcn-admin-design-md/commits/main/versions/2.2.1/) 选择提交，并将下载 URL 中的 `main` 替换为完整 SHA。不要把原 Release 附件当作当前最新版规范。只有切换到另一个上游应用版本时，才新增版本目录；同版本纠错通过普通 Git 提交追溯。

## 来源与验证边界

固定源码为 [satnaing/shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)，提交 [0217f8cb73af66f3cbf4141d5e0d00e1c5c30434](https://github.com/satnaing/shadcn-admin/tree/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434)。规范内链接均优先固定到该提交，精确依赖版本来自锁文件。

2026-09-08 静态审计将参考快照全部 247 个文件 blob 与上游比对一致，并据此核对主题、组件、路由、处理函数和配置。本轮验证不安装依赖、不构建或运行应用、不制作预览站点；静态核对不等于浏览器、读屏或 UI 验收。

本地 `shadcn-admin-2.2.1\` 仅供分析，由 `.gitignore` 排除。仓库仅跟踪两份 README、两份 DESIGN、LICENSE 和 .gitignore，不发布上游应用源码。

上游明确说明不是 starter template，并对部分 shadcn 组件做了 RTL 或其他修改。当前 [在线演示](https://shadcn-admin.netlify.app/) 和 [shadcn/ui 文档](https://ui.shadcn.com/) 只作背景参考，不能替代固定版本源码。

## 许可与独立声明

本仓库原创文档使用 [MIT License](LICENSE)，Copyright (c) 2026 turtoncarllyle。上游代码适用其自身 [MIT License](https://github.com/satnaing/shadcn-admin/blob/0217f8cb73af66f3cbf4141d5e0d00e1c5c30434/LICENSE)。字体、头像和品牌标识可能有独立条款，本规范不授予第三方资源或商标的额外使用权。

本仓库是独立整理成果，不隶属于 shadcn-admin 或 shadcn/ui，也不代表其官方认可。
