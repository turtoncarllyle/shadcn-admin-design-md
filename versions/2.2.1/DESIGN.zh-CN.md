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
  base_size: "继承正文为 16px；text-sm 控件为 14px"
  mobile_form_size: "低于 768px 的原生 input/select/textarea 为 16px"
---

# shadcn-admin 2.2.1 设计系统

[English](DESIGN.md) | [简体中文](DESIGN.zh-CN.md)

<a id="scope"></a>
## 目的、范围与证据

本文是供 AI 编码 Agent 构建后台工作界面的独立设计契约，基于 **shadcn-admin 2.2.1**，不是通用 shadcn/ui 版本。覆盖视觉基础、已安装组件、页面组合、交互状态、响应式行为和演示功能边界，不提供完整组件 API、生产认证、权限、业务规则或后端契约。

事实基线为上游标签 `v2.2.1`、提交 [`0217f8cb73af66f3cbf4141d5e0d00e1c5c30434`][upstream]。2026-09-08 静态审计已将本地参考目录与上游全部 247 个文件 blob 比对一致。在线演示和最新版文档可能已经变化，不能作为该版本的事实依据。

| 标记 | 含义 | Agent 使用方式 |
| --- | --- | --- |
| **S** | 源码事实：固定快照中的配置、类、属性、路由或处理函数 | 复用对应组合，不推断未实现功能 |
| **D** | 依赖行为：固定依赖版本的默认能力，不是本项目自行实现 | 保留原语契约，在浏览器中验证实际组合 |
| **R** | 面向使用方产品的建议补充规则 | 按任务需要实现，不能宣称上游已经具备 |
| **V** | 尚未验证的运行行为或已识别的源码限制 | 保留为验收项，不能当作 UI 测试通过 |

除另有标记外，下文的组件类、尺寸和处理函数均为 **S**。记录源码限制不代表要求复制缺陷。建议依次阅读：本节与技术基线、令牌与壳层、相关组件及页面、状态与已知缺口、验收标准。修改现有产品时，仍须检查其已安装源码并保留数据和权限契约。

证据优先级：固定版本页面组合与局部覆盖 > 固定版本共享组件 > 本地主题和基础 CSS > 锁文件解析后的依赖默认值。`cn()` 使用 `clsx` 和 `tailwind-merge`，调用方类可以覆盖组件默认值；CSS 层叠、变体优先级及移动输入框 `!important` 规则同样影响结果。不能用当前注册表示例替代这一关系。

<a id="baseline"></a>
## 固定技术基线

精确版本来自 [pnpm-lock.yaml][lock]，不能只看 [package.json][package] 中的版本范围。[components.json][config] 配置为 `new-york`、`slate`、CSS 变量、`rsc: false` 和 `@/` 别名。项目是 Vite React 应用，不是 Next.js。

| 包 | 锁定版本 |
| --- | --- |
| `react`、`react-dom` | `19.2.0` |
| `vite` / `typescript` | `7.1.11` / `5.9.3` |
| `tailwindcss`、`@tailwindcss/vite` | `4.1.14` |
| `tw-animate-css` | `1.4.0` |
| `@tanstack/react-router` / `@tanstack/react-query` / `@tanstack/react-table` | `1.132.47` / `5.90.2` / `8.21.3` |
| `react-hook-form` / `@hookform/resolvers` / `zod` | `7.64.0` / `5.2.2` / `4.1.12` |
| `cmdk` / `input-otp` | `1.1.1` / `1.4.2` |
| `react-day-picker` / `date-fns` | `9.11.1` / `4.1.0` |
| `recharts` / `sonner` / `react-top-loading-bar` | `3.2.1` / `2.0.7` / `3.0.2` |
| `lucide-react` / `@radix-ui/react-icons` | `0.545.0` / `1.3.2` |
| `@clerk/clerk-react` / `zustand` / `axios` | `5.51.0` / `5.0.8` / `1.12.2` |
| `class-variance-authority` / `clsx` / `tailwind-merge` | `0.7.1` / `2.1.1` / `3.3.1` |

以下 Radix 后缀均指 `@radix-ui/react-*`，不能合并为一个统一的 Radix 版本。

| 包后缀 | 锁定版本 |
| --- | --- |
| `alert-dialog`、`dialog`、`popover` | `1.1.15` |
| `avatar` / `checkbox` / `collapsible` | `1.1.10` / `1.3.3` / `1.1.12` |
| `direction` / `dropdown-menu` / `label` | `1.1.1` / `2.1.16` / `2.1.7` |
| `radio-group` / `scroll-area` / `select` | `1.3.8` / `1.2.10` / `2.2.6` |
| `separator` / `slot` / `switch` | `1.1.7` / `1.2.3` / `1.2.6` |
| `tabs` / `tooltip` | `1.1.13` / `1.2.8` |

**R：** 优先复用项目已安装的本地组件。不能运行最新版 shadcn CLI 无条件覆盖，不能把新版 `Field`/`Empty`/`Spinner`/聊天原语当作已安装能力，也不能把应用本规范变成依赖升级。本快照使用 `FormField`/`FormItem`/`FormControl` 组合表单。

<a id="principles"></a>
## 设计原则

- 使用紧凑、便于扫描的工作界面、简短标题、语义令牌、细边框和克制层级。
- 组合已有原语与变体，保留本地 RTL 适配及页面特有的信息密度。
- 卡片用于重复指标、对象或确实需要边界的工具，不用于包裹所有区域或嵌套装饰。
- **R：** 新工作流应补足有效的禁用、校验、等待、空数据、失败和恢复状态；演示项目没有完整实现所有这些状态。
- 避免营销式超大标题、装饰渐变、随意增加品牌色及布局跳动。下文明确记录功能性的源码例外，不能将这些例外推广到所有页面。

<a id="colors"></a>
## 颜色系统

[theme.css][theme] 为每种模式定义 23 个直接颜色角色，并定义八个侧栏别名。`@theme inline` 将它们映射为语义工具类。Front matter 是精简摘要，以下表格才是完整颜色契约。源码没有单独的 `destructive-foreground` 或成功/警告令牌。

### 核心语义令牌

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

### 侧栏别名

这些别名声明于 `:root`，引用当前亮色或暗色模式的核心变量。

| 别名 | 两种模式中的值 |
| --- | --- |
| `sidebar` | `var(--background)` |
| `sidebar-foreground` | `var(--foreground)` |
| `sidebar-primary` | `var(--primary)` |
| `sidebar-primary-foreground` | `var(--primary-foreground)` |
| `sidebar-accent` | `var(--accent)` |
| `sidebar-accent-foreground` | `var(--accent-foreground)` |
| `sidebar-border` | `var(--border)` |
| `sidebar-ring` | `var(--ring)` |

### 源码例外与扩展规则

- 仪表盘实际使用 `primary` 和 `muted-foreground`，坐标轴为 `#888888`，不是 `chart-1..5`。五个图表变量确实存在，但两个仪表盘图表没有使用它们。
- [用户状态样式][user-data] 为 `active`：teal，`inactive`：neutral，`invited`：sky，`suspended`：destructive。对应实际类为 `bg-teal-100/30 text-teal-900 dark:text-teal-200 border-teal-200`；`bg-neutral-300/40 border-neutral-300`；`bg-sky-200/40 text-sky-900 dark:text-sky-100 border-sky-300`；`bg-destructive/10 dark:bg-destructive/50 text-destructive dark:text-primary border-destructive/10`。
- Apps 已连接按钮使用蓝色亮暗工具类，说明使用 `text-gray-500`。主题预览和品牌图像也包含原始颜色。Tooltip 使用 `primary`/`primary-foreground`，不是 Popover 颜色。破坏性 Button/Badge 使用白色文字，暗色背景包含 `bg-destructive/60`。
- **R：** 新增界面优先使用语义角色。保留或有意识地评估上述例外，不能声称源码已完全令牌化。新增多序列图表可稳定映射 `chart-1..5`，同时提供颜色之外的标签或数值；扩展状态色前应验证对比度。

<a id="typography"></a>
## 字体排版

[FontProvider][font-provider] 默认使用 `inter`，向 `html` 添加 `font-inter`、`font-manrope` 或 `font-system`。[index.html][html] 通过 Google Fonts 加载 Inter 和 Manrope，并设置 `display=swap`。主题中的声明精确为 `'Inter', 'sans-serif'` 和 `'Manrope', 'sans-serif'`。

**D/V：** 本地没有 `--font-system` 声明。选择 system 会去掉具名字体工具类，依赖 Tailwind/浏览器回退，不会加载另一款字体。Tailwind 4.1.14 默认 sans 栈以 `ui-sans-serif, system-ui, sans-serif` 开始，并包含平台表情字体。离线字体替换和非拉丁字形覆盖仍需运行验证。

| 角色 / 工具类 | 字号 / 行高 | 源码用途 |
| --- | --- | --- |
| `text-xs` | 12px / 16px | Badge、元数据、分组标签 |
| `text-sm` | 14px / 20px | 大多数控件、表格单元格、导航 |
| 继承正文 / `text-base` | 16px / 24px | 未单独设置的正文和说明 |
| `text-lg` | 18px / 28px | Dialog 标题字号；标题覆盖可能使用 `leading-none` |
| `text-xl` | 20px / 28px | 区域或认证标题 |
| `text-2xl` | 24px / 32px | 页面标题与指标值 |
| `text-3xl` | 30px / 36px | 较大页面标题，常从 `md` 生效 |
| 错误状态码 | `7rem`（112px）、`leading-tight` | 401/403/404/500/503，不用于普通页面标题 |

字号和默认行高为 **D**，来自 [Tailwind 4.1.14 theme.css][tw-theme]；用途为 **S**。CardTitle 继承字号，使用 semibold/`leading-none`；仪表盘卡片覆盖为紧凑的 `text-sm font-medium`。Label 通常为 14px medium，不是统一 12px。源码页面标题经常使用 `tracking-tight`，等宽数字不是全局应用规则。

[index.css][base-css] 在不超过 767px 时，用 `!important` 将**原生** `input, select, textarea` 强制为 16px。Radix SelectTrigger 是按钮，没有覆盖时仍为 `text-sm`；OTP 格子文字也不是原生 input。**R：** 忠实复现时保留源码标题类；新设计的数据列是否使用正常字距和等宽数字应明确选择，不能描述成继承的全局事实。

<a id="geometry"></a>
## 间距、形状、层级与动效

**D：** Tailwind 间距单位为 `0.25rem`，16px 根字号下等于 4px。源码常用 4/8/12/16/24/32px 间隔。[Main][main] 使用水平 16px、垂直 24px 内边距。设置表单字段组间距为 32px，不能与紧凑表格工具栏混用。

| 圆角 | 数值 | 已有用途 |
| --- | --- | --- |
| 根变量 `--radius` | `0.625rem` / 10px | 主题基础 |
| `radius-sm` | 根值 - 4px = 6px | 菜单项 |
| `radius-md` | 根值 - 2px = 8px | 按钮、输入、导航、Badge |
| `radius-lg` | 根值 = 10px | Dialog、Alert、TabsList、应用集成项 |
| `radius-xl` | 根值 + 4px = 14px | Card、inset 壳层、批量工具栏 |
| 局部例外 | 4px、全圆、16px 角 | Checkbox、Switch/Avatar、聊天气泡 |

边框通常为 1px，焦点环不代替边框。常见输入/按钮焦点是 3px `ring-ring/50` 加 ring 色边框；PasswordInput、侧栏、菜单和关闭按钮有各自焦点类，不能宣称全部统一。

**D：** 固定 Tailwind 主题中的精确默认阴影：

| 工具类 | 阴影 |
| --- | --- |
| `shadow-xs` | `0 1px 2px 0 rgb(0 0 0 / 0.05)` |
| `shadow-sm` | `0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)` |
| `shadow-lg` | `0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)` |

Card 使用 `shadow-sm`；输入/按钮常用 `shadow-xs`；Dialog/Sheet 为 `shadow-lg`；Popover/Select/下拉菜单为 `shadow-md`。批量工具栏使用 `shadow-xl`，选中的配置预览使用 `shadow-2xl`，这些较强阴影属于局部例外。

| 动效或层级 | 源码或默认行为 |
| --- | --- |
| 未单独限定的 transition | **D：** 150ms，不是统一 200ms |
| 侧栏宽度/位置、导航箭头、Dialog 内容 | 显式 200ms |
| `.CollapsibleContent` | 300ms ease-out 高度关键帧 |
| Sheet | 关闭 300ms、打开 500ms，支持方向化滑入 |
| 菜单/Popover/Tooltip | 使用 `tw-animate-css` 的淡入、缩放与方向过渡，保留本地类 |
| 侧栏 / 表格固定列 | `z-10` |
| 顶栏、模态表面、批量工具栏、移动聊天 | `z-50`；相同数值仍受层叠上下文影响 |
| 跳过导航链接 | `z-999` |
| 顶栏与批量工具栏 | 固定顶栏滚动后模糊；批量工具栏背景模糊、hover 缩放 1.05 |
| 功能性渐隐 | `faded-bottom` 在 `md` 添加 128px 渐变覆盖层，禁用指针事件 |

**V/R：** 没有找到明确的应用级 reduced-motion 媒体规则。尊重减弱动效、验证浮层层叠与背景滚动锁定、避免动态内容跳动，属于新增产品功能的验收要求，不是已证实的源码能力。

<a id="shell"></a>
## 后台壳层与导航

依据：[authenticated-layout][shell]、[Sidebar][sidebar]、[Header][header]、[Main][main]、[nav-group][nav] 和 [sidebar-data][nav-data]。

| 表面 | 源码契约 |
| --- | --- |
| 展开桌面侧栏 | `--sidebar-width: 16rem` / 256px |
| 图标宽度令牌 | `--sidebar-width-icon: 3rem` / 48px |
| 普通 sidebar 折叠 | 占位宽度 48px |
| inset/floating 折叠 | 占位宽度 = 48 + 16 = 64px；固定外容器 = 48 + 16 + 2 = 66px，包含 `p-2` 处理 |
| 移动侧栏 | 可折叠模式使用 18rem / 288px Sheet |
| Header | 4rem / 64px；内部 `p-4`，间隔 12px，从 `sm` 起为 16px |
| Main | `px-4 py-6`；非 fluid 内容只在 `@7xl/content` 起限制为 80rem / 1280px |
| fixed Main | `flex grow flex-col overflow-hidden`，后代区域负责滚动 |
| fixed 壳层 | `100svh`；inset 公式减去 `var(--spacing) * 4`（16px） |
| 桌面 inset 框架 | 8px 外边距、14px 圆角、小阴影 |

应用 LayoutProvider 默认 `inset` + `icon`，单独的 Sidebar 原语默认则是 `sidebar` + `offcanvas`。样式支持 `inset/sidebar/floating`，折叠支持 `offcanvas/icon/none`。**注意：** `none` 在移动分支之前返回静态 16rem 侧栏，不会变成移动 Sheet；它是组件能力，但未在 ConfigDrawer 中提供。

只有 `<Header fixed>` 是 sticky，文档滚动超过 10px 后才显示阴影和半透明模糊，普通 Header 不固定。Apps、Chats、Settings 使用 `<Main fixed>` 和内部滚动；Tasks、Users 使用 `<Header fixed>` 加普通 Main；Dashboard 使用普通 Header/Main。不能混淆固定顶栏和固定高度内容。

侧栏菜单行默认高 32px、14px 文字、8px 内边距/圆角、16px 图标；small/large 变体分别高 28/48px，分组标签为 12px。`data-active` 使用侧栏 accent 和 medium 字重。展开的嵌套菜单为 Collapsible 加逻辑侧边框缩进；桌面折叠组变成下拉菜单；叶子点击关闭移动 Sheet。分组展开使用 `defaultOpen`，没有保证路由变化后同步展开的 effect。

导航组为 General、Pages、Other。Chats 的 `3` 徽标是静态演示数据。TeamSwitcher 只改变本地显示团队，不改变租户或权限。NavUser/ProfileDropdown 提供菜单组合与 SignOutDialog，显示资料为演示数据。TopNav 有小屏下拉和桌面链接，不是面包屑或持久路由标签系统。

**S/V：** Ctrl/Cmd+B 切换侧栏，Ctrl/Cmd+K 切换全局搜索；两者均为全局监听，没有 input/contenteditable 排除条件。**R：** 保留名称、焦点和逻辑方向，加入编辑器时评估快捷键冲突。不能把面包屑、混合导航布局或任意深度树虚构成已有功能。

<a id="components"></a>
## 已安装组件契约

[UI 目录][ui] 共 30 个文件，页面覆盖优先。本清单记录真实原语；loading/empty/error 由对应调用方负责，不能仅根据组件名称推断。

### 按钮与 Badge

[button.tsx][button] 提供 `default/destructive/outline/secondary/ghost/link`、`asChild` 和四种尺寸。

| 尺寸 | 高度 | 水平内边距 | 存在直接 SVG 子节点时 |
| --- | --- | --- | --- |
| `default` | 36px | 16px | 12px |
| `sm` | 32px | 12px | 10px |
| `lg` | 40px | 24px | 16px |
| `icon` | 36px 方形 | 组件布局决定 | 默认图标 16px |

默认 hover 为 `primary/90`，secondary 为 `secondary/80`，outline 为 accent（暗色 input/50），ghost 为 accent（暗色 accent/50），link 显示下划线。Disabled 禁用指针事件并降低为 50% 不透明度。没有内置 `loading`/`isLoading` 属性，也没有统一 active 缩放规则。**R：** 真正的异步操作应组合进度图标与 `disabled`，保持尺寸稳定。

`badge.tsx` 提供 default/secondary/destructive/outline，12px medium 文字、水平 8px/垂直 2px 内边距、8px 圆角和 12px 图标。链接 hover 只在渲染为 anchor 时生效；导航计数徽标覆盖为全圆。Badge 不是通用模式按钮。

### 表单、输入与选择

[form.tsx][form] 组合 RHF FormProvider、Controller 驱动的 FormField、FormItem、FormLabel、FormControl、FormDescription 和 FormMessage。FormControl 连接 `id`、`aria-describedby` 与 `aria-invalid`，FormMessage 输出校验消息。**V：** 调用方未使用 FormControl 时，不会自动获得这些关联。

| 源码组件 | 尺寸与状态契约 |
| --- | --- |
| `input.tsx` | 高 36px、`min-w-0`、水平内边距 12px、圆角 8px，透明/暗色 input/30，muted 占位文字；3px focus ring；invalid 为 destructive 边框及 /20 或暗色 /40 环；disabled 50% |
| `textarea.tsx` | 最小 64px、`field-sizing-content`、12px/8px 内边距；相同的 focus、invalid、暗色和 disabled 处理 |
| `label.tsx` | 14px medium，peer/group 禁用样式；保留标签关联，不能只有占位文字 |
| `checkbox.tsx` | 16px 方形、显式 4px 圆角、primary 选中、Check 图标、invalid/focus/disabled 样式；Radix 支持 indeterminate，但本封装没有单独的横杠图标 |
| `radio-group.tsx` | 分组 Grid 间隔 12px；16px 圆形控件和 8px primary 圆点，focus/invalid/disabled 样式 |
| `switch.tsx` | 宽 32px、高 1.15rem（18.4px），滑块 16px；选中 primary、未选中 input，RTL 镜像滑块；focus/disabled 样式，没有本地专用 invalid 样式 |
| `select.tsx` | 按钮触发器高 36px 或 32px、`w-fit`、14px 文字；popper 菜单最小 128px、限制可用高度；选中勾选、focus accent、disabled 50%、invalid 触发器 |
| `input-otp.tsx` | 36px 格子、active 3px 环、光标动画、分组边缘圆角；OTP 页面为六格、2+2+2 分组 |
| 共享 `password-input.tsx` | 原生输入高 36px、1px 焦点环；逻辑末端 24px 显隐按钮；没有本地 destructive invalid 类，也没有显隐按钮可访问名称 |
| 共享 `select-dropdown.tsx` | 封装选项、值与展开；可选 disabled `loading` 项高 56px，内含 20px 旋转 Loader；本身不获取数据 |

**D：** Radix 处理 checkbox/radio/select 状态和键盘操作，RHF/Zod 处理传入的 schema。**R：** 保留可见标签、字段关联错误、明确的提交按钮和防重复提交逻辑。不能声称所有演示表单都阻止重复请求、提示未保存离开或实现服务端校验。密码显隐名称及自定义封装的标签/invalid 关联需要检查。

### Calendar 与 DatePicker

[calendar.tsx][calendar] 封装 DayPicker 9.11.1：12px 内边距、32px 单元格令牌，月份先纵排、从 `md` 横排，默认显示相邻月份日期。包含 today、单选、区间起止/中间、outside、disabled、hidden、focused 日期样式，并将日期焦点转交给按钮。支持区间样式不代表每个 DatePicker 都是区间选择器。

[date-picker.tsx][date-picker] 是共享单日期 Popover，触发器为宽 240px 的 outline Button，格式为 `MMM d, yyyy`，标题为下拉模式，禁用今天之后或 `1900-01-01` 之前日期。空值显示 muted 占位文字；没有自由文本解析、清空、时间选择、上传或选中后自动关闭处理函数。**D/V：** 日期键盘与选择语义来自固定版本 DayPicker；本地化、RTL 区间圆角和缩放需要运行验证。

### Card、Avatar 与 Tabs

`card.tsx` 使用 card 角色、边框、14px 圆角、`py-6`、24px 组间距和 `shadow-sm`。Header/Content/Footer 的水平内边距为 24px，CardAction 位于头部操作列；Card 本身没有 loading/empty/failure 状态。

`avatar.tsx` 默认 32px 方形、全圆，含图片与 muted fallback；调用方可覆盖尺寸，例如聊天中的 36/44px。图片加载失败时应保留回退；远程 URL 不代表资源一定可用。

`tabs.tsx`：根间隔 8px，列表高 36px、muted 背景、10px 圆角、3px 内边距。触发器 14px medium、nowrap、8px 圆角。Active 使用 background 和小阴影，但暗色 active 为 input/30 加 input 边框；未激活文字亮色为 foreground、暗色为 muted。Disabled 阻止指针事件、50% 不透明度。**D：** Radix 提供激活和键盘行为，调用方负责窄屏溢出。

### Dialog、AlertDialog、Sheet 与浮层

| 组件 | 表面与组合 |
| --- | --- |
| `dialog.tsx` | 黑色 /50 遮罩；居中 background、24px 内边距、16px 间隔、10px 圆角、`shadow-lg`、宽度为视口减 32px，`sm:max-w-lg`（512px）；页脚先移动反向纵排，再末端对齐横排；可选内置关闭按钮 |
| `alert-dialog.tsx` | 独立的 Radix 确认原语；相似居中表面，组合 cancel/action，不是提示型 Toast |
| `sheet.tsx` | Background、进入边缘边框、`shadow-lg`；左右面板宽 75%、`sm:max-w-sm`（384px）；上下 auto 高度；Header/Footer 内边距 16px，Footer 靠末端 |
| `dropdown-menu.tsx` | Popover 表面，紧凑内边距，selected/checkbox/radio/submenu/destructive/disabled 样式与快捷键 |
| `popover.tsx` | 默认宽 288px、内边距 16px、8px 圆角、边框、`shadow-md`，调用方常覆盖为 auto 或 200px |
| `tooltip.tsx` | Primary 背景和前景，12px 文字、12px/6px 内边距、8px 圆角、箭头；不是 Popover 表面 |

[confirm-dialog.tsx][confirm] 封装 AlertDialog。`isLoading` 禁用取消和确认，`disabled` 可独立限制确认。确认操作是普通 Button，不会自动出现加载图标或执行数据变更，完成和关闭由调用方负责。

**D：** 固定版本 Radix 原语提供 Portal 放置、模态焦点处理、关闭/焦点返回和菜单导航。Dialog/Sheet 与 AlertDialog 的外部交互契约不同，不能假定相同关闭行为。**V：** 自定义受控打开、无触发器组合，以及全局 `body[data-scroll-locked] { overflow: unset !important; }` 都可能影响结果，应验证实际焦点返回和背景滚动。每个模态需要有效标题与描述关联，不能仅凭导入 DialogTitle 就判定通过。

### 命令搜索与布局工具

[CommandMenu][command-menu] 从 sidebar-data 生成目标，并附加亮色/暗色/system 命令；先关闭再导航或设置主题。CommandDialog 输入区域高 48px，列表最大 300px，实际命令面板内嵌高 288px 的 ScrollArea。分组标题 12px muted，项目 14px、accent 选中、disabled 50%；Dialog 组合将项目垂直内边距覆盖为 12px。空态文案为 `No results found.`，这是本地导航/主题搜索，不是服务端搜索或按权限过滤的索引。

**D：** cmdk 提供查询过滤、方向键移动和 Enter 选择，Radix 处理外层模态。**V：** [ui/command.tsx][command] 中隐藏的 DialogHeader/Title 位于 DialogContent 外部，应检查可访问名称和模态隐藏行为，不能默认通过。

`collapsible.tsx` 导出 Radix Root/Trigger/Content，300ms 应用动画需要主动添加 `.CollapsibleContent`。`scroll-area.tsx` 添加 `orientation` 属性（默认 vertical）、对应的 10px 滚动条，并在横向 viewport 使用 `overflow-x-auto!`，没有 `viewportClassName` 属性。`separator.tsx` 默认 horizontal、`decorative: true`，厚 1px，纵向高度由调用方提供；这些都不是卡片。

### 反馈、加载与空内容

`alert.tsx` 使用 `role="alert"`、带边框的 10px 圆角 card 表面、标题/说明/图标布局以及 default/destructive 变体。`skeleton.tsx` 为 `bg-accent animate-pulse rounded-md`，尺寸由调用方提供。SidebarMenuSkeleton 可用，不代表每个页面都异步获取数据。

[sonner.tsx][sonner] 传入主题，并用 popover 角色覆盖普通通知背景/文字/边框。[根路由][root-route] 设置持续时间 5000ms，模拟认证/批量流程使用 promise toast。[NavigationProgress][progress] 将路由 pending/complete 状态映射为高 2px、muted-foreground 的进度条，它不是后端任务进度。

没有共享 `Empty`、`Spinner` 或通用 `Progress` 组件。表格以高 96px 单元格显示 `No results.`；命令列表有自己的空态；聊天有未选择会话占位；Apps 没有专用零结果文案。**R：** 新异步视图应保持尺寸稳定、区分首次加载和刷新，提供适用的空态/重置/重试，并播报成功/失败，不能虚构源码行为。

### 共享壳层与内容组合

依据：[共享组件][shared]和[布局组件][layout-components]中的下列文件。

| 组件 | 实际组合与边界 |
| --- | --- |
| `app-sidebar.tsx`、`app-title.tsx` | AppSidebar 挂载 TeamSwitcher、NavGroup、NavUser、SidebarRail；AppTitle 是可选的首页链接/开关替代，默认组合中被注释 |
| `search.tsx` | 高 32px 的 outline Button，打开 CommandMenu，不是行内搜索输入；宽度从弹性扩展为 sm/lg/xl 的 160/208/256px，快捷键徽标从 sm 显示 |
| `top-nav.tsx` | lg 以下非模态下拉，lg 起行内链接；active/disabled 由调用方传入，没有路由标签历史 |
| `nav-user.tsx`、`profile-dropdown.tsx` | 头像/资料菜单、设置目标及 SignOutDialog；Billing 指向设置，Upgrade/New Team 为占位，展示快捷键文字不会注册处理函数 |
| `sign-out-dialog.tsx` | 384px 确认框；重置本地认证，携带当前位置跳转登录，不撤销服务端会话 |
| `theme-switch.tsx` | 非模态 light/dark/system 菜单、选中勾选、太阳/月亮动画；只有显式 dark 才将 theme-color 设为 `#020817`，否则为 `#fff` |
| `learn-more.tsx` | 20px 有名称的图标触发器，top/start Popover，14px muted 内容；不是 Dialog，也不一定是外部链接 |
| `coming-soon.tsx` | 全高居中占位，72px 图标、36px 标题，没有可工作的帮助操作 |
| `long-text.tsx`、`skip-to-main.tsx` | 长文本披露与跳过导航意图，具体键盘/目标缺口见下文 |

<a id="tables"></a>
## 数据表格、筛选与批量操作

依据：[data-table 组件][data-table]、[Tasks 表格][tasks-table]、[Users 表格][users-table]、[URL 状态 Hook][url-state]。

- `ui/table.tsx` 在语义表格外封装 `overflow-x-auto`。表头高 40px、水平内边距 8px，单元格内边距 8px、14px 文字和 nowrap；行使用 `hover:bg-muted/50`、`data-[state=selected]:bg-muted`，**不是 accent**。
- TableFooter 使用 muted/50 背景、顶边框和 medium 字重；TableCaption 位于底部，14px muted、上间距 16px。即使 Tasks/Users 没有渲染，这些原语仍存在。
- 工具栏搜索高 32px、宽 150px，从 `lg` 起宽 250px；筛选和 Reset 紧邻数据。Column View 在 `lg` 以下隐藏，通过非模态 checkbox 下拉切换可隐藏 accessor 列。
- ColumnHeader 按能力提供升序、降序和隐藏。FacetedFilter 使用 Popover + Command，包含多选、每项计数、已选徽标/数量和清除。
- 表头复选框切换**当前页全部行**，不是整个数据集；批量操作读取**筛选后的已选行**。排序、列显隐和选择保留在本地，不能暗示全部序列化到 URL。
- Users 窄布局将选择和用户名列固定在逻辑起点附近，单元格背景保持 hover/selected；`@4xl/content` 阴影处理取决于容器宽度。不能改写成“自动变成卡片布局”。

| 页面 | URL 状态 | 数据边界 |
| --- | --- | --- |
| Tasks | `filter`、`status`、`priority`、`page`、`pageSize` | 在客户端过滤模拟数据；全局搜索 task id/title |
| Users | `username`、`status`、`role`、`page`、`pageSize` | 列筛选，禁用全局搜索 |
| 两者 | 默认 page 1、pageSize 10；Table pageIndex 从 0 开始 | 筛选变化重置页码，排序/列显隐/选择不是 URL 参数 |

**V：** Hook 从 search 初始化本地筛选，但没有 effect 在浏览器历史变化后同步筛选；分页从当前 search 派生。`ensurePageInRange` 仅在 pageCount > 0 时修正超界页码。支持刷新/分享不等于已验证完整的前进后退或空数据分页。

分页使用 32px 控件、10/20/30/40/50 每页选项、当前 `Page X of Y`、上一页/下一页和空间允许时的首末页。`getPageNumbers` 在不超过五页时全部显示，否则显示边界及省略号；**没有当前数据范围**，例如“1-10 of 100”。低于 `@2xl/content`（672px）时反向堆叠，低于 `@md/content`（448px）隐藏首末页；两处页码文案的可见性规则在 672-767px 容器宽度留下静态缺口，需运行验证，不能与 768px 视口规则混淆。

[BulkActions][bulk] 固定于视口底部 24px、居中、z-50、14px 圆角，带模糊、`shadow-xl` 和 hover 1.05 缩放。有选中行才出现，礼貌播报计数，并提供方向键/Home/End 导航；Escape 根据事件目标或活动元素是否属于下拉 trigger/content 决定是否跳过清空，不是简单检查菜单是否打开，其他情况则清空选择。**R：** 适配时验证裁剪、缩放、移动可达性及模态层叠。

<a id="pages"></a>
## 页面模式与实际行为

### Dashboard

[Dashboard][dashboard] 提供 Overview 与 Analytics，Reports 和 Notifications 禁用。Overview 指标为单列，`sm` 两列，`lg` 四列；下方在 `lg` 使用七轨 Grid，按 4/3 分配。RecentSales 组合 Avatar fallback、姓名/邮箱和右对齐金额。

[Overview 图表][overview] 是高 350px 的 Recharts BarChart，使用月份模拟值、primary 柱、顶部 4px 圆角、12px `#888888` 坐标轴，无刻度线和轴线，Y 轴加美元符号。[Analytics 图表][analytics-chart] 为高 300px 的 AreaChart，primary clicks（填充透明度 .15）、muted-foreground uniques（.1）、星期数据和相同轴样式；Analytics 还使用条高 10px、全圆、12px 标签及等宽数值的 SimpleBarList，不是通用 Progress 原语。

两个图表都没有 Tooltip/Legend 组件、图表令牌序列映射或 loading/empty/error 分支。Download 没有处理函数，Dashboard TopNav 示例路径未注册成业务目标。**R：** 实际分析功能需补充可访问的序列/数值说明、加载、空态、重试和真正导出，不能声称演示已提供。

### Tasks

[Tasks][tasks] 是客户端数据表格，包含 label、title、status、priority 模式。状态包含 backlog/todo/in progress/done/canceled，优先级为 low/medium/high 加图标；保留 schema 值和显示标签，不要翻译枚举标识符。

创建/编辑使用 [TasksMutateDrawer][task-sheet]，实际是 **Sheet**，不是 Dialog。字段 title/status/label/priority 必填，包含 SelectDropdown、单选和页脚提交；提交仅展示数据、重置并关闭。单条删除使用 ConfirmDialog；Make a copy 和 Favorite 是禁用演示操作。

[导入][task-import] 使用 `sm:max-w-sm` Dialog 与文件输入；校验非空 FileList 和首个文件 MIME `text/csv`，处理函数仅展示 name/size/type。不解析 CSV、不上传、不新增行，也没有进度、大小限制或拖放。**R：** 这些需要明确的产品实现和服务端校验，不能由视觉规范推断。

批量状态/优先级/导出/删除通过两秒 promise 模拟反馈。批量删除要求去除两端空格后精确输入 `DELETE`，然后清空选择并通知，不移除数据；源码 pending 行为不等于统一防重复提交能力。

### Users

[Users][users] 组合 username/status/role 筛选、固定列表格、行菜单、邀请/创建/编辑/删除浮层和批量操作。角色值为 `superadmin/admin/manager/cashier`，状态颜色见前文；角色标签不是权限实现。

[创建/编辑][user-dialog] 使用 `sm:max-w-lg` Dialog，内部表单滚动区高 26.25rem（420px），六列中标签/控件占 2/4 列，间隔 16px，窄屏**不会**自动上下堆叠。包含姓名、用户名、邮箱、电话、角色、密码和确认；新密码至少八位，含小写字母和数字，编辑时可留空；密码字段 dirty 后才启用确认密码。保留真实 Zod schema，不能套用认证页较宽松的密码规则。

[邀请][user-invite] 使用 `sm:max-w-md`（448px）Dialog，邮箱和角色必填，说明可选；提交展示数据、重置和关闭，不发送邀请邮件。单条删除要求去除两端空格后精确输入用户名，批量删除要求 `DELETE`、模拟两秒并清空选择，两者都不持久删除。**R：** 接入真实请求后应限制重复提交、让失败表单可恢复，并确认影响范围。

### Apps

[Apps][apps] 是可滚动的集成**列表/Grid**，不是数据表格。URL 参数为 `filter/type/sort`，type 为 `all/connected/notConnected`，sort 为 `asc/desc`；客户端按名称筛选/排序，从 search 初始化本地状态，与表格筛选一样不保证后续浏览器历史同步。

Grid 为单列、`md` 两列、`lg` 三列，间隔 16px。项目使用 `rounded-lg border p-4 hover:shadow-md`（10px 圆角）、40px 品牌图标区、两行说明与功能性 `faded-bottom`。已连接按钮使用蓝色例外；Connect 无操作，没有零结果分支或真实集成授权。

### Chats

[Chats][chats] 读取静态 `convo.json`，搜索去除两端空格后匹配全名，选择用户改变本地选中状态。列表先全宽，`sm` 为 224px、`lg` 为 288px、`2xl` 为 320px。低于 **640px** 时，选中会话以 z-50 覆盖主区域，Back 清除移动选择；从 `sm` 起分栏，与侧栏的 768px 边界不同。

历史按 `d MMM, yyyy` 分组，时间为 `h:mm a`，反向纵排滚动区保持示例顺序。气泡最大宽 288px、12px/8px 内边距、长词换行和局部 16px 角；发出消息用 primary/90 加带透明度前景，收到消息用 muted。桌面未选会话占位提供 "Send message"。

编辑器 form **没有 submit 处理函数或消息变更逻辑**，Send 可能触发原生表单提交；附件、通话/视频和更多按钮未实现，没有真实未读跟踪、流式响应、送达、上传或持久化。[NewChat][new-chat] 是宽 600px 的 Dialog，含 Command 搜索和可移除选择徽标，无选择时 Chat 禁用，否则只展示提交数据，关闭时重置选择。**R：** 消息送达、图标可访问名称、错误/重试和附件处理属于独立产品实现。

### 认证

[认证页面][auth] 包含 `/sign-in`、`/sign-in-2`、`/sign-up`、`/forgot-password`、`/otp`。共享认证布局居中，SignIn2 从 `lg` 起双栏，并在该断点以下隐藏亮暗仪表盘截图；不能将此图片组合推广到普通后台页面。

登录校验邮箱/密码（至少七位），模拟两秒 promise 时禁用提交，存储模拟用户/token，并跳转 redirect 或首页。注册校验两次密码一致并模拟 loading；忘记密码显示两秒 promise 后跳转 OTP。OTP 要求恰好六个字符，未完成或 loading 时 Verify 禁用，展示数据后等待一秒跳回首页。这不代表服务端凭证认证、实际发信或 OTP 校验成功。社交按钮及 `/terms`/`/privacy` 链接在本快照中属于未实现操作/目标。

**R：** 接入生产认证时不能复制演示处理函数去记录或 Toast 展示真实密码/token。实际凭证、请求错误、重发/限流及账户恢复必须依照使用方契约实现。

### 设置

[设置布局][settings] 包含五个路由。`md` 以下导航为高 48px Select，`md` 起为横向 ScrollArea，`lg` 起为宽 20% 的纵向侧栏、列间距 48px。内容区域从 `lg` 起内部最大宽 576px，自有滚动和 `faded-bottom`；字段组间距 32px。

| 路由 | 源码表单行为 | 边界 |
| --- | --- | --- |
| `/settings`（Profile） | 用户名 2-30 字符、邮箱选择、bio 4-160 字符、useFieldArray URL 列表及追加按钮 | 提交只展示数据，没有资料服务 |
| `/settings/account` | 姓名 2-30 字符、受限 DatePicker、可搜索语言 Popover/Command | 提交只展示数据，语言选择不翻译应用 |
| `/settings/appearance` | 原生字体 select、light/dark 预览单选；提交调用 setFont/setTheme 并展示数据 | Schema 不接受 `system`，但 ThemeProvider 可默认 system |
| `/settings/notifications` | all/mentions/none 单选、邮件开关、安全邮件已开且 disabled/`aria-readonly`、移动 checkbox | 没有通知服务或服务端偏好持久化 |
| `/settings/display` | 多项 checkbox，要求至少选择一项 | 提交只展示数据，不改变侧栏显隐 |

**V：** [AppearanceForm][appearance] 将初始主题强制断言为 light/dark，但没有转换 `system`，默认 system 时可能需要选中有效主题才能通过提交校验。这些表单没有统一的未保存离开保护、保存 spinner、服务端失败或保存状态持久化。

### 可选 Clerk 集成

[Clerk 路由][clerk] 需要 `VITE_CLERK_PUBLISHABLE_KEY`。缺少时显示配置说明 Alert；存在时由 ClerkProvider 渲染第三方 SignIn/SignUp 路由，这不是模拟登录流程。

`/clerk/user-management` 仍渲染模拟 UsersTable 数据，组件检查 Clerk loading/登录状态，显示 spinner；说明用的 LearnMore Popover 关闭后启动未授权五秒跳转倒计时，可取消跳转。`_authenticated` 布局名称本身不是权限守卫。根 ClerkProvider 未配置 appearance/暗色/RTL 适配。**V：** 本次源码审计没有验证真实 Clerk 会话、跳转、主题或远程数据。

### 错误页、帮助与请求反馈

[错误页族][errors] 包含独立 `/401 /403 /404 /500 /503` 和壳层内 `/errors/$error`，分别映射 unauthorized/forbidden/not-found/internal-server-error/maintenance-error，未知值使用 NotFound。多数页面包含 112px 状态码、简短说明、历史 Back 和 Home；401 不直接提供 Sign In 按钮，503 的 Learn more 没有处理函数，GeneralError 的 `minimal` 省略状态码和操作。Help Center 为 ComingSoon，不是知识库或可工作的客服流程。

[main.tsx][entry] 配置 Query 请求反馈：401 重置认证、Toast 并携带 redirect 参数跳转登录；500 Toast 且只在生产导航至 /500；403 导航被注释。Query 对 Axios 401/403 不重试，开发/生产采用不同重试规则，stale time 十秒，生产支持窗口聚焦刷新；Mutation 错误调用 handleServerError。共享配置不能让静态 Tasks/Users/Apps 自动成为 API 页面。

<a id="states"></a>
## 交互状态矩阵

| 状态 | 已有源码处理 | 补充规则或限制 |
| --- | --- | --- |
| Default / filled | 组件表面、占位/值和标签 | 内容变化时保留宽度和密度 |
| Hover | 按钮变体、表格 muted/50、导航 sidebar-accent、应用项阴影 | 不强制所有控件同一种 accent/缩放 |
| Focus / focus-visible | 常见 3px 环，密码/侧栏/关闭控件有例外 | **R：** 验证真实组合的可见性、名称、焦点顺序和返回 |
| Active / pressed | 原生/Radix 状态，特定导航激活表面 | 没有统一自定义按压动画 |
| Selected / checked | 表格 muted、active Tabs、primary 标记、导航 data-active | 不能仅靠颜色；indeterminate 没有专用横杠图标 |
| Open / expanded | Radix 状态、Collapsible 箭头与动画 | 分组 defaultOpen 不保证路由更新时同步 |
| Disabled | 通常 50% 不透明度，指针/光标规则因组件不同 | 必须真正不可交互；已开启安全设置仍可见 |
| Loading / submitting | 认证禁用按钮、promise toast、导航进度、SelectDropdown 项、Skeleton 原语 | 演示 CRUD 没有统一 loading 或防重复提交 |
| Empty | Table/Command 文案、初始聊天占位 | Apps 和图表缺少专用空分支 |
| Invalid | RHF 消息和 aria 关联，常见 destructive 输入边框/环 | PasswordInput 和绕过 FormControl 的调用方需要补充 |
| Request failure | Query/Mutation 共享反馈、独立错误页 | 模拟处理函数不覆盖服务端拒绝/重试 |
| Success | 演示数据/promise toast，Appearance 应用本地偏好 | 成功 Toast 不代表持久化变更 |
| Destructive | ConfirmDialog，指定流程要求用户名或 DELETE | 确认影响范围；没有后端删除 |
| Recovery | 重置筛选、分页、取消对话框、错误页 Back/Home | **R：** 只在业务支持时增加重试/重发/撤销 |

<a id="responsive"></a>
## 响应式行为

不能把视口和具名容器断点合并为一个“移动端规则”。[use-mobile.tsx][mobile] 判断 width < 768。**D：** 下列值来自 Tailwind 4.1.14。

| 视口断点 | 源码用途 |
| --- | --- |
| `sm` 640px | 聊天分栏、两列仪表盘指标、LongText 从移动 Popover 变为 Tooltip、Dialog 宽度/页脚变体 |
| `md` 768px | 桌面侧栏、Apps 两列、横向设置导航、结束原生输入 16px 覆盖 |
| `lg` 1024px | 四列仪表盘指标、Apps 三列、纵向设置导航、Column View、双栏认证 |
| `xl` 1280px / `2xl` 1536px | 可用视口断点；聊天列表在 2xl 加宽 |

| `@container/content` 断点 | 数值 | 实际用途 |
| --- | --- | --- |
| `@md/content` | 448px | 分页首末页控件显隐 |
| `@2xl/content` | 672px | 分页堆叠/第一处页码文案 |
| `@3xl/content` | 768px | 第二处页码文案 |
| `@4xl/content` | 896px | Users 固定列阴影 |
| `@7xl/content` | 1280px | 非 fluid Main 最大宽度 |

**R：** 横向溢出限制在表格内部，固定布局的纵向滚动由内部区域负责。不能声称所有表单在 768px 堆叠；Users 的 2/4 列 Grid、240px DatePicker、200px 语言/字体控件需评估窄屏。应检查长本地化标签、空/大量数据、缩放、横竖屏、主题与 RTL。`html overflow-x-hidden` 可能掩盖裁剪，不能作为响应式通过的证据。

<a id="personalization"></a>
## 主题、方向与持久化

| 偏好 | 机制 / 默认值 | 持久化 |
| --- | --- | --- |
| 主题 | [ThemeProvider][theme-provider]，system/light/dark，默认 system，html 类 | Cookie `vite-ui-theme`，365 天 |
| 字体 | [FontProvider][font-provider]，inter/manrope/system，默认 inter | Cookie `font`，365 天 |
| 方向 | [DirectionProvider][direction]，html dir + Radix DirectionProvider，默认 ltr | Cookie `dir`，365 天 |
| 侧栏样式 | [LayoutProvider][layout-provider]，inset/sidebar/floating，默认 inset | Cookie `layout_variant`，7 天 |
| 折叠模式 | icon/offcanvas/none，应用默认 icon | Cookie `layout_collapsible`，7 天 |
| 桌面展开状态 | AuthenticatedLayout 读取，除 cookie 为 false 外默认展开 | Cookie `sidebar_state`，7 天 |
| 移动展开状态 | SidebarProvider 本地状态 | 与持久化桌面展开偏好不同 |

[ConfigDrawer][config-drawer] 立即应用主题/方向/布局，侧栏样式与布局区域在 `md` 以下隐藏。布局名称映射为 Default = 展开，Compact = 关闭/icon，Full = 关闭/offcanvas。Reset 重新展开侧栏，重置主题、方向和布局，**不重置字体**。Appearance 只在提交时改变主题/字体。

**V：** 主题类在 effect 中设置，源码不保证初始化无闪烁。系统主题变化会更新根类，但在 theme 改变前不会更新 memoized `resolvedTheme` 上下文，因此即使 CSS 已更新，读取该值的消费者仍可能过时。根 Sonner 接收主题偏好，Clerk 没有应用专用 appearance 映射。

**R：** 保留所有语义角色、局部 dark 例外、Portal 祖先关系和 RTL 修改。验证首屏、浮层打开期间的系统变化、原生控件、第三方组件和从设置返回；不能声称每个第三方表面都会自动继承所有偏好。

<a id="accessibility"></a>
## 可访问性与内容规则

上游 README 标明 ScrollArea、Sonner、Separator 的本地修改，以及 AlertDialog、Calendar、Command、Dialog、DropdownMenu、Select、Table、Sheet、Sidebar、Switch 的 RTL 适配。后续合并应保留这些文件，当前注册表代码不能直接替换。

**S/V 源码发现：**

- [SkipToMain][skip] 指向 `#content`，但本快照 Main/路由没有匹配目标。应保留跳过导航意图；**R：** 在产品实现中连接真正可聚焦的主内容目标。
- 密码显隐、部分聊天操作及徽标移除按钮没有可访问名称。不是所有自定义字段都使用 FormControl，标签/错误不保证全部关联。
- CommandDialog 标题位置需要真实可访问树验证；受控或无触发器 Dialog 需明确验证焦点返回。
- [LongText][long-text] 在 ref 绑定时检查溢出，`sm` 起 Tooltip、以下 Popover，但 div 触发器不可键盘聚焦，也没有 ResizeObserver；不能保证键盘或尺寸变化后可查看完整值。
- 逻辑方向间距和方向化原语与部分物理图表/圆角/定位类并存，没有找到全局减弱动效规则或实测对比度报告。
- HTML 初始 `lang="en"`，Account 语言字段仅提交演示数据；RTL 不是翻译系统。

**R 扩展内容契约：** 状态/图标/颜色旁保留可读标签，每个图标操作有名称，身份有回退，关键长值不能只有截断。DatePicker 为 `MMM d, yyyy`，聊天为 `d MMM, yyyy` 和 `h:mm a`，仪表盘金额使用美元示例；locale、币种、时区、数值精度、复数和翻译消息都必须由产品明确选择，不能静默更改枚举或假定演示已本地化。

键盘验收包括 Tab/Shift+Tab、Enter/Space、菜单/日历导航、Escape、初始焦点、模态焦点限制和返回触发控件。**D/V：** 原语提供默认能力；实际组合、对比度、缩放和读屏行为在测试前仍为未验证。

<a id="resources"></a>
## 资源、依赖与业务边界

[index.html][html] 加载远程 Google Fonts、按系统主题选择的 favicon，以及初始白色 theme-color。挂载的 ThemeSwitch 会针对显式 dark/light 更新该 meta 值，但 system-dark 仍走白色分支；favicon 不一定跟随手动应用主题。[Assets][assets] 包含本地 Logo/Clerk 标识、品牌图标组件和配置预览；认证使用本地亮暗仪表盘 PNG，public/images 包含 favicon 变体和仪表盘图片。聊天/头像数据引用远程图片和 `/avatars/shadcn.jpg` 等路径，并非全部已打包；NewChat 回退 `/placeholder.svg` 也不是已有 public 文件。

通用图标主要使用 Lucide，表格和设置还使用 Radix Icons。上游将品牌图标归因于 Tabler，但它们是本地源码组件，不是已安装的 `@tabler/icons-react` 依赖。保留源码图像与回退语义，不能假定 URL 始终可用。

本仓库原创文档使用 MIT，Copyright (c) 2026 turtoncarllyle。[上游 LICENSE][upstream-license] 为上游代码的 MIT。品牌标识、外部头像和托管字体可能适用独立条款，本次审计不认证全部资源/字体许可，也不授予商标权。**R：** 重新分发或自托管前逐项核对实际资源/字体许可。本规范仓库不重新分发上游应用源码或图片。

`_authenticated` 路由是壳层，本身不是安全边界。模拟认证和角色/状态显示不实现生产访问控制。必须保留使用方应用的路由、search schema、行标识、RHF/Zod 模型、Query 错误处理和认证/授权契约，不能靠删除校验或把请求改成演示 Toast 来“修复”界面。

<a id="coverage"></a>
## 覆盖矩阵与审计记录

下表评估**静态审计后的文档覆盖**，不是业务功能完整度或浏览器认证。“完整”表示相关源码模式及其限制已说明，不代表可直接用于生产。

| 模块 | 原文档 | 当前覆盖 | 依据 / 剩余限制 |
| --- | --- | --- | --- |
| 定位、提示词、版本选择 | 完整 | 完整 | 范围、基线、README，现明确当前文档与原始快照 |
| 语义色与别名 | 基本完整 | 完整 | 保留 theme.css 角色到模式的映射，补充明确例外 |
| 排版、动效、阴影 | 部分 | 完整 | 正文字号、控件例外、固定默认 150ms 和局部层级 |
| 壳层与导航 | 部分 | 完整 | 48/64/66px 实际折叠宽度、none/移动例外、菜单 |
| 30 个 UI 原语及共享控件 | 部分 | 完整 | 组件契约与 UI 源码，没有虚构已安装原语 |
| 表格、筛选、分页、批量 | 部分 / 不准确 | 完整 | URL/本地状态区分、内部溢出、确认范围 |
| 仪表盘与图表 | 部分 / 不准确 | 完整 | 真实序列、高度，无 Tooltip/Legend/导出实现 |
| Tasks 与 Users | 部分 / 不准确 | 完整 | Sheet/Dialog 区分、校验、导入/模拟变更边界 |
| Apps 与 Chats | 部分 | 完整 | Grid/聊天断点，集成/发送处理函数缺失 |
| 认证、五个设置页、错误/帮助 | 部分 | 完整 | 实际处理函数、持久化、占位和恢复 |
| 主题/RTL 与第三方组件 | 部分 | 部分 | 源码规则已说明，首屏、Clerk 和动态主题需运行 |
| 可访问性/内容 | 部分 | 部分 | 明确源码缺口，没有 WCAG 认证 |
| 真实请求、上传、送达、服务端权限 | 不适用 | 不适用 | 演示未提供，只作为扩展要求 |
| 通用树、面包屑、路由标签、详情页、Drawer、Dropzone、Chart 封装 | 不适用 | 不适用 | 不存在；Tasks “drawer” 是 Sheet，Account combobox 为组合 |
| 双语一致/链接/来源 | 部分 | 完整 | 相同章节、事实和数值，固定来源链接 |
| 浏览器渲染 / 辅助技术 | 待验证 | 待验证 | 本次文档更新未运行应用、截图或执行运行验收 |

按优先级记录问题及本规范已应用的改进：

| 优先级 | 原问题 / 证据 | 影响与修正 | 验证方式 |
| --- | --- | --- | --- |
| P1 | 原覆盖矩阵将图表 tooltip/legend、任务 Dialog、CRUD/导入/聊天和设置持久化写成已实现，见前文页面处理函数 | 可能生成错误流程；改为精确组合和演示边界 | 阅读真实 JSX 与 submit/click 函数，不只匹配关键词 |
| P1 | 原可访问性/主题章节暗示跳过导航、第三方继承主题和防闪烁可用，见 SkipToMain、ThemeProvider、Clerk 路由 | 可能隐藏可用性缺口；分离 S/D/R/V，补充针对性运行标准 | 记录静态证据，浏览器证明仍为 V |
| P2 | 原统一 14px 正文、16px Select、focus/200ms、accent 表格行和当前数据范围 | 可能生成错误密度/状态；按基础 CSS、原语与固定 Tailwind 默认值修正 | 精确令牌映射、类/元素类型和断点核对 |
| P2 | 原将 48px 折叠宽度用于所有样式，只强调 768px | 可能错误布局 inset、聊天、分页；补充变体公式及视口/容器断点表 | Sidebar 公式、聊天 sm、分页容器类 |
| P2 | 依赖主要只有大版本，可能混入最新版组件行为 | 补充锁定版本、本地覆盖、源码链接和缺失组件边界 | 比对 package/importer 和已安装源码清单 |
| P3 | 原始 Release 快照与同版本持续维护文档容易混淆 | main 继续维护 2.2.1，保留历史标签/附件，用提交固定具体内容 | 检查 README URL、远端 main 及原标签/附件 |

<a id="acceptance"></a>
## 验收与同版本维护

本次是 **2.2.1** 的纠错与补充，不升级上游、不创建新的修订版本号。`versions/2.2.1` 和 YAML `version: "2.2.1"` 保持不变，当前文档在 main 维护，由 Git 提交记录变化。已有 `v2.2.1` 和 Release 原附件继续作为首次发布快照，不自动等同 main 的最新内容；固定某次维护结果时使用完整提交 SHA。

本次文档检查：

1. 严格 UTF-8 解码，front matter 合法且包含 version/name/description/colors/typography；双语章节锚点、语义规则、尺寸和令牌映射一致。
2. 将两种模式各 23 个直接颜色和八个别名逐项与 theme.css 比对，不能只统计 OKLCH 出现次数；核对依赖版本及所有来源路径与固定源码树。
3. 核对组件/页面/状态证据、相对链接、固定上游链接、版本索引、当前下载/历史快照说明及许可归属。
4. 确认仍只有约定六个文件被跟踪，本地上游快照保持忽略；标签/附件不变，并核对推送后的 main 内容。

独立的产品运行验收，**本次文档更新未执行**：

1. 在窄屏/移动/桌面及 640/768/1024 视口、448/672/768/896/1280 内容断点渲染目标页，检查溢出、表单 Grid、表格固定列和分页文案。
2. 验证亮色/暗色/system、浮层打开时的系统主题切换、字体回退、RTL、缩放、减弱动效、Dialog/Sheet/Command 焦点和背景滚动。
3. 验证键盘跳过导航、名称/错误关联、完整长文本访问和读屏播报，测量令牌、状态与图表对比度。
4. 对真正集成使用受控数据验证凭证/权限失败、等待/重复提交、空数据、重试、文件校验、批量范围和恢复；Clerk 需配置凭证及服务访问。

本次静态文档审计不包含依赖安装、应用构建、启动服务、截图或 UI 符合性认证；未测试的运行项目继续标为 V。

<a id="agent-guide"></a>
## Agent 提示词

```text
修改界面前先读取 shadcn-admin 2.2.1 的 DESIGN.md。
将 S 视为固定源码事实，D 视为固定依赖行为，R 视为补充规则，V 视为待验证。
复用本地组件、语义 OKLCH 令牌、源码密度、10px 基础圆角及真实页面/视口/容器规则，
保留路由、schema、权限和本地 RTL 定制。不要虚构缺失 API、把演示 Toast 当成数据变更，
或从当前 shadcn 注册表覆盖组件。先确定适用状态与源码例外，只实现所请求的产品行为，
并将静态检查结果与浏览器/运行验证分开报告。
```

<a id="known-gaps"></a>
## 已知缺口与非目标

上游明确表示它不是 starter template。本规范可为忠实源码的页面样式/组合提供上下文，但不替代本地组件 API、shadcn/ui 文档、应用源码或产品/后端设计。模拟数据边界、源码缺陷、未验证的第三方/可访问性行为仍如前文所述；未来上游版本需要独立证据，不能静默替换 2.2.1 基线。

本仓库是独立整理成果，不是 shadcn-admin 或 shadcn/ui 官方规范，也不代表官方认可。

<a id="sources"></a>
## 资料来源

以下所有 `src` 引用均固定到同一上游提交，不使用移动的默认分支。[上游 Release](https://github.com/satnaing/shadcn-admin/releases/tag/v2.2.1) 标识应用版本；[在线演示](https://shadcn-admin.netlify.app/)、[shadcn/ui 文档](https://ui.shadcn.com/)、[DESIGN.md 概览](https://stitch.withgoogle.com/docs/design-md/overview/) 仅作背景参考。

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
