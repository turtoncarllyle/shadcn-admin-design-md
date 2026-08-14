---
version: "2.2.1"
name: "shadcn-admin"
description: "基于 satnaing/shadcn-admin v2.2.1、按版本固定的后台管理界面设计系统。"
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
  base_size: "桌面控件与工作内容默认使用 14px"
  mobile_form_size: "低于 768px 时表单文字使用 16px"
---

# shadcn-admin 2.2.1 设计系统

## 目的与范围

使用本文档创建或修改符合 [satnaing/shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1) 视觉语言的后台工作界面。它把源码中的主题变量、Tailwind 类、Radix 原语和页面组合转化为 AI 可读取的设计契约。

本文档中的版本号是 shadcn-admin 应用版本，不是通用 shadcn/ui 版本，也不表示当前注册表组件仍使用相同 API。项目内源码与最新版 shadcn/ui 文档不一致时，以项目内源码为准。

本系统适用于仪表盘、数据管理、设置、认证、聊天和其他高频工作界面，不是营销站风格。在应用这些视觉规则时，必须保留既有应用行为、路由、数据契约、权限和本地组件定制。

## 设计原则

1. **工作效率优先。** 面向扫描、比较和重复操作设计。标题负责定位，不应占据大部分视口。
2. **语义化主题。** 使用 `background`、`foreground`、`primary`、`muted`、`accent`、`destructive` 等命名令牌，不能随意替换成色板工具类。
3. **安静的结构。** 使用间距、细边框、克制阴影和字重建立层级，避免装饰容器和视觉噪声。
4. **组件组合。** 优先复用项目已安装的 shadcn/Radix 组件及其变体，先用已有原语组合页面，再创建自定义表面。
5. **状态完整。** 相关工作流必须覆盖 hover、focus、active、selected、expanded、disabled、loading、empty、invalid 和 destructive 状态。
6. **原生支持响应式与双向布局。** 移动导航、RTL、逻辑方向间距、键盘访问和文字溢出属于组件契约的一部分。

## 技术基线

视觉契约固定到 v2.2.1 源码快照：

| 范围 | 源码基线 |
| --- | --- |
| 应用 | Vite 7、React 19、TypeScript 5.9 |
| 样式 | Tailwind CSS 4、CSS 变量、`new-york` 组件风格 |
| 原语 | Radix UI 与项目内 shadcn 组件源码 |
| 导航与数据 | TanStack Router、Query 和 Table |
| 表单 | React Hook Form、Zod、Radix 控件 |
| 图标 | Lucide；上游文档仅将 Tabler 用于品牌图标 |
| 图表 | Recharts 与五个语义图表令牌 |
| 反馈 | Sonner、Alert、Skeleton、导航进度 |

此表不能作为升级依赖的授权。版本升级必须重新审查源码，并发布新的设计文档版本。

## 颜色系统

### 核心语义令牌

使用 `src/styles/theme.css` 中的 CSS 自定义属性。组件通过 `bg-background`、`text-foreground`、`border-border`、`ring-ring` 等语义工具类使用这些令牌。

| 角色 | 亮色 | 暗色 | 用途 |
| --- | --- | --- | --- |
| `background` | `oklch(1 0 0)` | `oklch(0.129 0.042 264.695)` | 应用画布 |
| `foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | 主要文字与图标 |
| `card` | `oklch(1 0 0)` | `oklch(0.14 0.04 259.21)` | 卡片表面 |
| `card-foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | 卡片内容 |
| `popover` | `oklch(1 0 0)` | `oklch(0.208 0.042 265.755)` | 菜单、Popover、命令表面 |
| `popover-foreground` | `oklch(0.129 0.042 264.695)` | `oklch(0.984 0.003 247.858)` | Popover 内容 |
| `primary` | `oklch(0.208 0.042 265.755)` | `oklch(0.929 0.013 255.508)` | 主要操作与强强调 |
| `primary-foreground` | `oklch(0.984 0.003 247.858)` | `oklch(0.208 0.042 265.755)` | Primary 上的内容 |
| `secondary` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | 次要控件与弱强调 |
| `secondary-foreground` | `oklch(0.208 0.042 265.755)` | `oklch(0.984 0.003 247.858)` | Secondary 上的内容 |
| `muted` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | 弱化表面与禁用上下文 |
| `muted-foreground` | `oklch(0.554 0.046 257.417)` | `oklch(0.704 0.04 256.788)` | 说明、元数据、占位文字 |
| `accent` | `oklch(0.968 0.007 247.896)` | `oklch(0.279 0.041 260.031)` | Hover、选中导航、柔和强调 |
| `accent-foreground` | `oklch(0.208 0.042 265.755)` | `oklch(0.984 0.003 247.858)` | Accent 上的内容 |
| `destructive` | `oklch(0.577 0.245 27.325)` | `oklch(0.704 0.191 22.216)` | 破坏性操作与无效状态 |
| `border` | `oklch(0.929 0.013 255.508)` | `oklch(1 0 0 / 10%)` | 分隔线与表面轮廓 |
| `input` | `oklch(0.929 0.013 255.508)` | `oklch(1 0 0 / 15%)` | 输入边框与暗色输入填充 |
| `ring` | `oklch(0.704 0.04 256.788)` | `oklch(0.551 0.027 264.364)` | 键盘焦点提示 |

### 图表与侧栏令牌

| 令牌 | 亮色 | 暗色 |
| --- | --- | --- |
| `chart-1` | `oklch(0.646 0.222 41.116)` | `oklch(0.488 0.243 264.376)` |
| `chart-2` | `oklch(0.6 0.118 184.704)` | `oklch(0.696 0.17 162.48)` |
| `chart-3` | `oklch(0.398 0.07 227.392)` | `oklch(0.769 0.188 70.08)` |
| `chart-4` | `oklch(0.828 0.189 84.429)` | `oklch(0.627 0.265 303.9)` |
| `chart-5` | `oklch(0.769 0.188 70.08)` | `oklch(0.645 0.246 16.439)` |

侧栏令牌引用核心角色：`sidebar` 引用 `background`，`sidebar-foreground` 引用 `foreground`，`sidebar-primary` 引用 `primary`，`sidebar-accent` 引用 `accent`，其前景、边框和焦点环角色采用相同方式。必须保留这些引用，使不同侧栏样式始终响应主题。

图表序列稳定使用图表令牌，并提供标签、图例或直接数值；不能只靠颜色传达含义。Destructive 红色不是通用强调色。不要为了让页面更“丰富”而添加第二套品牌色。

## 字体排版

外观设置提供 `Inter`、`Manrope` 和系统字体。Inter 是默认工作字体；Manrope 是可选字体，不代表可以在后台页面使用超大展示标题。

| 角色 | 字号与字重 | 用途 |
| --- | --- | --- |
| 页面标题 | 约 24-30px，bold/semibold | 每页一个简短标题 |
| 区域标题 | 18-20px，semibold | 主要无框区域或卡片标题 |
| 卡片标题 | 16px，semibold | 紧凑表面标题 |
| 正文/控件 | 14px，regular 或 medium | 表格、按钮、导航、表单 |
| 辅助文字 | 14px，muted | 描述和次要数值 |
| 说明/标签 | 12px，medium | 分组标签、Badge、元数据 |
| 移动表单文字 | 低于 768px 时至少 16px | 防止浏览器聚焦缩放 |

先用 `font-medium` 或 `font-semibold` 建立层级，再考虑增大字号。保持正常字间距；只有命令快捷键可以使用较宽 tracking。计数器和对齐指标使用等宽数字。单行导航长文本应截断，必要时仍能访问完整值。

## 间距、形状与层级

Tailwind 的 4px 间距节奏是基础。页面常用水平 `1rem`、垂直 `1.5rem` 内边距。紧凑组件内部通常使用 8-12px 间隔，主要内容组使用 16-24px。Flex 和 Grid 布局优先使用 `gap-*`，保证方向切换和换行稳定。

根圆角为 `0.625rem`（10px）：

| 令牌 | 计算圆角 | 常见用途 |
| --- | --- | --- |
| `radius-sm` | 6px | 小型命令项和紧凑内部控件 |
| `radius-md` | 8px | 按钮、输入框、导航行、Badge |
| `radius-lg` | 10px | Dialog、Alert、组合控件 |
| `radius-xl` | 14px | 卡片 |

使用细 `border-border` 轮廓和组件已有的 `shadow-xs`、`shadow-sm` 或 `shadow-lg`。卡片只用小阴影；Dropdown 和 Dialog 因为是真实分层，可以使用更强层级。普通页面区域不能做成漂浮表面。不要在同一表面同时叠加强边框、大阴影、有色填充和大圆角。

普通焦点使用 3px `ring/50` 光环和 ring 色边框。常规状态过渡为 200ms；Collapsible 内容使用 300ms ease-out；Sheet 关闭 300ms、打开 500ms。固定 Header 只有在滚动后才增加阴影和轻微背景模糊。遵循 reduced-motion 偏好，移除非必要动画但保留最终状态。

## 后台壳层与布局

### 壳层尺寸

- 桌面侧栏：`16rem`（256px）。
- 折叠图标侧栏：`3rem`（48px）。
- 移动 Sheet 侧栏：`18rem`（288px）。
- Header：`4rem`（64px）。
- Main 内容：水平 `1rem`、垂直 `1.5rem` 内边距。
- 非流式内容：达到 `@7xl/content` 容器阈值后居中，`max-w-7xl`（80rem / 1280px）。
- 完整应用高度：固定布局使用 `100svh`；inset 布局扣除外围间距。

默认布局是 `inset` 侧栏与 `icon` 折叠。侧栏样式支持 `inset`、`sidebar`、`floating`；折叠模式支持 `icon`、`offcanvas`、`none`。必须选用完整且一致的组合，不能拼接不同变体的视觉片段。

Header 包含侧栏触发器、Separator、上下文导航或搜索、主题/配置操作和用户菜单。Main 区域承载页面标题、主要操作、视图控件和内容。不要把整个 Main 包在装饰卡片中。

源码会持久化侧栏状态、布局样式、折叠模式、主题、字体和方向。路由切换不能重置这些偏好。固定布局必须使用自己的滚动区域，不能制造第二条页面滚动条。

## 组件语言

### 按钮与操作

按钮使用 14px medium 文字、图标文字间距和清晰焦点环。

| 尺寸 | 规格 |
| --- | --- |
| Default | 高 36px，水平内边距 16px |
| Small | 高 32px，水平内边距 12px |
| Large | 高 40px，水平内边距 24px |
| Icon | 36px 正方形 |

`default` 用于主要操作，`secondary` 用于次级操作，`outline` 用于中性工具，`ghost` 用于低装饰控件，`link` 用于行内导航，`destructive` 只用于不可逆或危险命令。一个页面通常只有一个最强操作。禁用控件使用 50% 透明度并停止指针事件。加载按钮保持禁用，同时保留标签或无障碍名称并显示进度。

图标操作使用熟悉的 Lucide 图标，由本地 Button 组件控制常规图标尺寸。含义不明显的纯图标控件必须提供无障碍名称和 Tooltip。

### 表单与选择控件

Input 高 36px、水平内边距 12px、8px 圆角、细 input 边框和小阴影。Textarea 使用同样的表面语言，最小高度 64px。宽度低于 768px 时，Input、Select 和 Textarea 字号强制为 16px，防止浏览器聚焦缩放。

Label 简短且始终可见。Description 使用 `muted-foreground`。验证使用 `aria-invalid`、destructive 边框和 3px destructive 淡色焦点环；错误文字紧邻对应字段。禁用控件仍可读，但明确不可用。保留源码 React Hook Form 与 Zod 组合，不另造并行表单体系。

Checkbox、RadioGroup、Switch、Select、Calendar、InputOTP 和 PasswordInput 使用已安装的 Radix/shadcn 组件。必须实现 checked、unchecked、open、selected、invalid、disabled 状态。相关控件放在明确标签下，Placeholder 不能作为唯一标签。

### 卡片、指标与图表

Card 使用 `card`/`card-foreground`、边框、14px 圆角、24px 垂直内边距、24px 内部分组间隔和 `shadow-sm`。使用完整组合：Header 放标题/说明/操作，Content 放主要数值或可视化，Footer 放必要的辅助操作。

指标卡先显示标签和可选图标，再以最强层级显示数值，趋势或上下文位于其下。图表沿用卡片表面与语义图表色。坐标轴、Tooltip、图例、空态和加载态在两种主题下都必须清晰。

卡片只用于独立重复对象或有明确边界的工具。不要卡片嵌套卡片，也不要把所有页面区域都变成漂浮磁贴。次级层级通常使用边框、Separator 或无框内容组即可。

### 数据表格与分页

Table 使用全宽语义表面。表头高 40px、水平内边距 8px、medium 字重并禁止换行。单元格使用 8px 内边距和对齐的不换行工作内容。行使用底边框、accent hover 状态和清晰 selected 状态。

筛选、搜索、列可见性、主要操作和批量操作应靠近表格。排序和筛选控件必须显式展示状态，不能只改变图标颜色。Pagination 按源码能力包含页码、每页数量、当前范围和上一页/下一页。只有选中行后才显示批量操作；破坏性批量操作需要确认。

TanStack Table 状态可以同步到 URL 查询参数，必须保留此行为，使刷新、历史记录和共享链接有效。窄屏下优先保留重要列，提供行详情或受控表格视口，并阻止整个页面横向滚动。

### 导航与侧栏

侧栏分组使用 12px medium 标签和 muted foreground。标准菜单按钮高 32px、14px 文字、8px 内边距、8px 圆角和 16px 图标。Hover、active、open、selected 使用 sidebar accent 角色。子菜单使用逻辑方向边框和缩进，不创建新卡片。

展开侧栏显示标签、分组、Badge 和用户信息。Icon 变体折叠为 48px，并为隐藏标签提供 Tooltip。移动导航使用 Sheet，不压缩桌面轨道。`Ctrl/Cmd+B` 切换侧栏；在文字编辑流程中不能破坏预期输入行为。

顶部导航保持紧凑，可在移动端折叠为 Dropdown。长标题和用户值截断时不能推动相邻控件。使用逻辑方向的 `start`/`end`、`ms`/`me` 和 inline border 工具类支持 RTL。

### 标签页、Badge 与 Avatar

Tabs 使用高 36px 的 muted List 表面和 3px 内边距。Active Trigger 使用 background 表面和小阴影；Inactive 文字使用 muted。所有 Trigger 必须位于同一 TabsList 内，暂不可用视图显式 disabled。

Badge 是 12px 紧凑标签，8px 圆角，必要时使用细边框，支持 default、secondary、destructive、outline。Badge 用于状态或数量，不作为通用按钮。Avatar 始终提供首字母或短名称 fallback，并保持稳定尺寸。

### Dialog、Sheet、菜单与 Popover

Dialog Overlay 使用 50% 黑色。Dialog Content 居中、带边框、24px 内边距、10px 圆角和阴影；默认最大宽度为 `sm:max-w-lg`，视口边缘保留 1rem。Header 包含 Title 和可选 Description。Footer 在移动端反向堆叠，在大屏将操作对齐到末端。

Sheet 从指定边缘进入，侧面板宽 75%，最大为 `sm:max-w-sm`。移动侧栏和配置面板使用此模式。Dropdown、Select、Tooltip、Popover 使用 popover 令牌、克制层级、碰撞处理和 Radix 键盘导航。

每个 Dialog 和 Sheet 都需要无障碍 Title，即使在视觉上隐藏。破坏性确认明确写出受影响对象和后果。普通页面流中的内容不能滥用 Modal。

### 命令搜索

全局命令面板是包含已安装 `cmdk` 原语的 Dialog。Dialog 组合中的搜索区高 48px；结果列表最大高度 300px。Group 使用 12px muted 标题；Item 使用 14px 文字、紧凑内边距、语义 selected 状态、图标和可选快捷键。

支持 `Ctrl/Cmd+K` 打开、方向键移动、Enter 激活、Escape 关闭。无匹配结果时显示明确空态。搜索结果对应真实可导航目的地，并保留 active 与 nested 上下文。

### 反馈、加载、空态与错误态

瞬时通知使用 Sonner，页面内消息使用 Alert，结构加载使用 Skeleton，路由切换使用导航进度条。Alert 带有 `role="alert"`；自定义的普通非紧急状态更新应使用 polite live region。

加载态预留最终布局尺寸。空态说明缺少什么，并提供最相关的下一步操作。错误态区分可恢复验证、未授权、未找到和服务器故障。破坏性反馈使用 destructive 角色；成功状态不能在没有业务意义时发明永久绿色品牌体系。

认证与设置页面沿用主应用控件、令牌和间距。Chat 保持会话区域易读、输入区稳定，并让移动端边框和圆角响应布局变化。工具页面不能变成无关的营销页面。

## 完整源码覆盖矩阵

下列清单是本版本的规范性范围。具体实现只需使用工作流需要的组件，但只要对应源码模式适用，就不能换成另一套视觉语言。

### 已安装 UI 原语

| 源码组件 | 必须覆盖的视觉与状态 |
| --- | --- |
| `alert`、`alert-dialog` | 页面内状态与破坏性确认；Title、Description、default/destructive、open/closed、焦点锁定、取消/确认、disabled 和 pending |
| `avatar`、`badge` | 稳定身份 fallback 与紧凑状态/数量；图片失败、fallback、default/secondary/destructive/outline，以及可交互时的 focus |
| `button` | default、destructive、outline、secondary、ghost、link；default/small/large/icon；hover、focus、active、disabled、loading |
| `calendar`、`date-picker` | 月份导航、today、selected、range、outside、unavailable、disabled；Popover open/closed、键盘焦点和格式化值 |
| `card` | Header、Title、Description、Action、Content、Footer；default、loading、empty、error，且不嵌套卡片 |
| `checkbox`、`radio-group`、`switch` | checked/unchecked/支持时的 indeterminate、hover、focus、disabled、invalid、标签关联和 RTL 对齐 |
| `collapsible` | Trigger、open/closed、方向感知旋转指示器、300ms 内容过渡和 reduced-motion 最终状态 |
| `command` | Dialog、Input、List、Empty、Group、Item、Separator、Shortcut；query、selected、disabled、键盘导航和无结果 |
| `dialog`、`sheet` | Overlay、Content、Header、Title、Description、Footer、Close；open/closed、焦点管理、响应式操作和 pending submission |
| `dropdown-menu`、`popover`、`select`、`tooltip` | Trigger/Content、open/closed、highlighted、selected、checked、disabled、支持时的嵌套菜单、碰撞处理和键盘控制 |
| `form`、`label`、`input`、`textarea`、`input-otp` | default、hover、focus、filled、invalid、disabled、read-only、submitting；Description、Message、OTP 光标/完成和移动 16px 字号 |
| `scroll-area`、`separator` | 方向感知视口/滚动条和语义分隔；overflow、滚动条隐藏/显示、horizontal/vertical 和 RTL |
| `sidebar` | Provider、Rail、Inset、Header、Footer、Content、Group、Menu、Badge、Action、Submenu、Skeleton；expanded/collapsed/offcanvas/mobile、active、hover、focus、open、loading |
| `skeleton`、`sonner` | 保持形状的加载占位与瞬时通知；visible/dismissed、使用时的 success/error/info、Action 和时长语义 |
| `table` | Header、Body、Footer、Row、Head、Cell、Caption；hover、selected、排序上下文、empty、loading、overflow 和 RTL 对齐 |
| `tabs` | Root、List、Trigger、Content；active/inactive、hover、focus、disabled 和响应式溢出 |

### 公共、布局与数据组件

| 源码范围 | 组件与要求 |
| --- | --- |
| 应用壳层 | `app-sidebar`、`app-title`、`authenticated-layout`、`header`、`main`、`nav-group`、`nav-user`、`team-switcher`、`top-nav`；覆盖三种侧栏、全部折叠模式、状态持久化、移动 Sheet、active route、nested route、账号菜单和长标签 |
| 数据表格系统 | `bulk-actions`、`column-header`、`faceted-filter`、`pagination`、`toolbar`、`view-options`；覆盖搜索、重置、排序方向、筛选选择、列可见性、行选择、每页数量、页码、上一页/下一页、loading、empty 和破坏性批量确认 |
| 全局工具 | `command-menu`、`config-drawer`、`confirm-dialog`、`date-picker`、`navigation-progress`、`search`、`select-dropdown`、`sign-out-dialog`、`theme-switch`；覆盖 trigger/open/close、键盘访问、主题/方向/布局预览、pending navigation 和破坏性退出 |
| 内容工具 | `coming-soon`、`learn-more`、`long-text`、`password-input`、`profile-dropdown`、`skip-to-main`；覆盖不可用功能提示、外部/帮助操作、截断、密码显示、Avatar fallback、账号操作和键盘跳过导航 |

### 功能与页面族

| 功能/页面族 | 必须覆盖 |
| --- | --- |
| Dashboard | Overview/Analytics Tabs、指标卡、图表卡、Recent Sales、下载操作、图表 Tooltip/Legend、disabled Tabs、loading、no-data |
| Tasks | 搜索、状态/优先级筛选、可排序/隐藏列、选择、分页、导入/创建/编辑/删除 Dialog、行/批量操作、验证、submitting、空结果 |
| Users | 搜索与筛选、表格选择、邀请/创建/编辑/删除、行/批量操作、状态/角色 Badge、验证、pending request、空结果 |
| Apps | 搜索/筛选结果网格、集成卡、connected/not-connected、空结果、长描述和响应式卡片布局 |
| Chats | 会话列表、搜索、selected conversation、unread/count、消息记录、Composer、New Chat Dialog、滚动、空会话和移动分栏行为 |
| Authentication | Sign in、Alternate Sign-in、Sign up、Forgot Password、OTP；default、focus、invalid credentials、validation、密码显示、submitting、success、recovery |
| Clerk 集成 | Clerk Sign-in、Sign-up、认证边界和 User Management 保持视觉整合，不能超出其主题钩子支持范围改写第三方行为 |
| Settings | Profile、Account、Appearance、Notifications、Display 表单；dirty/pristine、validation、saving、saved feedback、主题、字体、方向和显示偏好 |
| Error Handling | 认证区错误路由以及 401、403、404、500、503；明确状态、简短解释、安全的主要恢复操作和可选次级导航 |
| Help 与未开放路由 | Help Center 和 Coming Soon 使用正常壳层、明确下一步，不使用营销式 Hero |

即使上游演示使用模拟数据、没有在屏幕上触发每个状态，每个页面族仍必须实现相关的响应式、暗色、RTL、loading、empty、error、disabled 和键盘状态。

## 交互状态矩阵

| 状态 | 必须处理 |
| --- | --- |
| Hover | 语义 accent 或受控透明度变化，不产生布局偏移 |
| Focus visible | Ring 色边框加 3px 焦点环，键盘可见且不被遮挡 |
| Active/pressed | 即时反馈，不改变控件尺寸 |
| Selected | Accent 表面与前景，并在需要时提供状态语义 |
| Expanded/open | 清晰 Trigger 状态与方向正确的 Chevron |
| Checked/indeterminate | 控件标记、语义状态、关联 Label 和键盘焦点 |
| Current route | Active 导航表面以及 `aria-current` 或等价语义 |
| Sorted/filtered | 可见排序方向或筛选摘要，并提供明确重置路径 |
| Disabled | 不可交互、50% 透明度、Label 仍可读 |
| Loading | 尺寸稳定、显示进度、防止重复操作 |
| Submitting/saving | 字段仍可理解、阻止重复提交、宣布成功或失败 |
| Empty | 明确信息和符合上下文的下一步操作 |
| Invalid | `aria-invalid`、destructive 边框/焦点环、相邻说明 |
| Destructive | Destructive 令牌、明确后果、不可逆时确认 |

不能只用颜色表达状态；按场景同时使用文字、图标、ARIA 状态或结构提示。

## 响应式行为

主要移动边界是 768px：`useIsMobile()` 将低于 768px 的宽度视为移动端。

### 低于 768px

- 桌面侧栏替换为 18rem Sheet，并在导航后关闭。
- Header 操作保持紧凑，低优先级导航移入菜单。
- 表单文字使用 16px，需要时控件全宽。
- Dialog Footer、卡片 Grid、筛选和表单列按任务顺序堆叠。
- 主要操作保持可触达，不能与标题或搜索重叠。
- 优先保留重要表格列并提供详情入口，禁止页面整体横向滚动。

### 768px 及以上

- 使用所选桌面侧栏样式和折叠模式。
- 保持 64px Header，空间允许时将上下文工具稳定对齐在一行。
- Dashboard 和表单使用响应式 Grid，不能固定像素列。
- 大容器宽度下居中非流式内容，并限制在 1280px。

认证后的 Inset 使用 `@container/content` 标记内容区，应使用容器查询实现依赖内容宽度的布局。测试长标签、本地化文字、RTL、缩放和窄窗口。固定控件和 Grid 尺寸，防止 loading 或 hover 状态推动布局。

## 暗色模式与个性化

暗色模式是在 `.dark` 下完整替换语义令牌，不是为每个组件硬编码覆盖色。组件通过相同令牌名自动适配。Card 与暗色 Background 略有区分；Popover 层级更高；Border 和 Input 使用半透明白色。

源码支持 light、dark、system 主题，以及 Inter/Manrope/system 字体、LTR/RTL、侧栏样式和折叠模式。配置控件可以预览选择，但不能改变应用基本层级。持久化用户选择，并避免初始化时闪现错误主题。

不能因为一个硬编码颜色在亮色下看起来合适就认为它安全。必须同时验证两种主题中的文字、图标、边框、焦点、图表、Overlay 和 Disabled 状态。

## RTL 与可访问性

全局使用逻辑方向定位和间距。Chevron 方向、Sheet 边缘、子菜单边框、表格对齐、Switch、Dialog、Select 和导航跟随文档方向。v2.2.1 源码明确修改了 `scroll-area`、`sonner`、`separator`，并为 `alert-dialog`、`calendar`、`command`、`dialog`、`dropdown-menu`、`select`、`table`、`sheet`、`sidebar`、`switch` 增加 RTL 更新。升级时必须保留这些文件并手工合并。

保留源码的 `Skip to Main`、Landmark 结构、Label、无障碍名称、Radix 键盘行为、可见焦点、Dialog Title 和 Avatar Fallback。纯图标按钮需要名称；陌生图标需要 Tooltip。缩放和长翻译文字不能重叠或裁剪控件。

源码以可访问性为目标，但本文档不提供合规认证。消费项目需要正式合规时，应使用键盘导航、屏幕阅读器语义、对比度和响应式缩放测试真实流程，并在不抹除源码视觉语言的前提下改进。

## 应做与禁止

### 应做

- 创建自定义 UI 前先使用项目已安装组件及现有 Variant。
- 所有主题相关颜色都使用语义 CSS 变量和工具类。
- 条件类组合使用 `cn()`，方向布局使用逻辑属性。
- 后台页面保持紧凑、易扫描、可预测。
- 筛选和操作靠近它们影响的数据。
- 保留 URL 表格状态、加载态、确认流程和键盘快捷键。
- 应用上游更新时保留所有本地 RTL/定制组件修改。
- 使用当前 shadcn/ui 示例前先检查本地组件源码。

### 禁止

- 不要把界面做成重渐变、超大字号、漂浮卡片的 SaaS 营销页。
- 有语义令牌时，不要硬编码 slate、white、black 或 destructive 颜色。
- 不要嵌套卡片，也不要为每个区域添加装饰容器。
- 后台工作页面不要使用巨型标题或过量留白。
- 不要只用颜色隐藏状态、验证或破坏性后果。
- 移动端不要永久压缩桌面侧栏。
- 不要让最新版 shadcn CLI 覆盖 v2.2.1 的定制组件。
- 不要把应用 Release 版本号当作 shadcn/ui Registry 版本。

## Agent 提示词指南

委派 UI 工作时使用以下紧凑指令：

```text
使用 shadcn-admin 2.2.1 视觉语言构建这个后台工作界面。使用项目内已安装的
shadcn/Radix 组件和语义 OKLCH 令牌。保持 14px 工作密度、10px 基础圆角体系、
细边框、克制阴影、64px Header、响应式侧栏、暗色模式、RTL、可见焦点，以及
完整的 loading/empty/invalid/disabled 状态。不要添加营销式构图、原始主题颜色、
嵌套卡片，也不要用最新版 shadcn Registry 覆盖定制组件。
```

完成前按以下清单检查：

1. 页面是否像 v2.2.1 应用，而不是通用仪表盘？
2. 两种主题是否始终使用语义令牌？
3. 侧栏、Header、内容宽度、密度、圆角、边框和阴影是否忠于源码？
4. 组件组合和每个相关交互状态是否完整？
5. 低于 768px、RTL、缩放和长文本下是否仍可用？
6. 是否存在键盘焦点、无障碍名称、Dialog/Sheet Title 和错误语义？
7. 本地组件定制是否得到保留？

## 已知边界与源码范围

- shadcn-admin 明确将自身描述为可复用 Dashboard 集合，而不是 starter project。本文档不会把它变成应用模板。
- 本规范固定到 shadcn-admin v2.2.1。当前 shadcn/ui Registry 的 API、Preset、Token 或组件结构可能不同。
- 部分组件为 RTL 或项目行为做了有意修改。没有逐文件 Diff 和手工合并时，自动覆盖不安全。
- 本文档描述视觉与交互预期，不定义路由、认证、授权、API 契约、数据模型或产品验证规则。
- 其他上游页面和选项可能包含局部例外。修改组件 Markup 或行为前查阅项目内 v2.2.1 源码。
- 可访问性要求可能需要产品级额外测试和改进；改进不能抹除源码视觉语言。
- 这是独立源码分析，不是 shadcn-admin 或 shadcn/ui 官方规范或认可。

## 来源

- [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin)
- [shadcn-admin v2.2.1](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1)
- [shadcn-admin 在线演示](https://shadcn-admin.netlify.app/)
- [shadcn/ui 文档](https://ui.shadcn.com/)
- [Google Stitch DESIGN.md 概览](https://stitch.withgoogle.com/docs/design-md/overview/)

本文档数值主要取自 `components.json`、`src/styles/theme.css`、`src/styles/index.css`、`src/components/ui/`、`src/components/layout/` 和 v2.2.1 Changelog。
