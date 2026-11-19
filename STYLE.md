# 站点视觉风格规范（原生架构版）

> 本文件是主站视觉与内容架构的权威说明。修改前先确认改的是设计令牌、原生模板还是结构化内容；不要凭截图猜样式，不要重新引入主题模块或 CSS 框架。
>
> **风格识别卡：冷调学术现代主义。** 安静、克制、可信、条理清晰；像排版精良的论文预印本与工程目录，而不是求职简历、SaaS 落地页或炫技作品集。

- 站点：`Chiron`（Zhehao Jin / 金哲浩）
- 主站定位：思想档案与工程目录
- 独立专案：深度实证与公共传播，例如 `/viz2.html`
- 线上地址：`https://jzhchiron.github.io/`

---

## 1. 技术与构建基线

| 项目 | 当前基线 | 位置 |
|---|---|---|
| 静态站生成器 | 原生 Hugo `0.152.2` extended，无主题模块 | `config/_default/hugo.yaml` |
| CSS | Tailwind CSS v4 + `@tailwindcss/typography`，经 Hugo Pipes 构建 | `assets/css/main.css`、`layouts/_partials/head.html` |
| 包管理 | Node `20`、pnpm `10.14.0` | `package.json`、两套工作流 |
| 本地开发 | `pnpm dev` → `hugo server --disableFastRender` | `package.json` |
| 构建 | `pnpm build` 或 `hugo --minify` | `package.json` |
| 语言 | 默认英文 `en`，中文 `zh`；英文在根路径，中文在 `/zh/` | `config/_default/languages.yaml` |
| 双语内容 | `index.md` / `index.zh.md`，根级页面为 `page.md` / `page.zh.md` | `content/` |
| 分支校验 | push `refactor/**` 与所有 PR 执行 `hugo --minify`，不部署 | `.github/workflows/ci-build-check.yml` |
| 生产部署 | push `main` 或手动触发，构建后发布 GitHub Pages | `.github/workflows/deploy.yml` |

生产与校验工作流必须使用相同的 Node、pnpm、Hugo extended 版本、依赖安装命令和 `hugo --minify` 构建命令。生产环境只额外通过 `HUGO_BASEURL` 注入 Pages 地址。

---

## 2. 原生三层控制链

| 层 | 职责 | 修改位置 | 典型修改 |
|---|---|---|---|
| 1. 设计令牌与基础 CSS | 全站色彩、字体、字号、圆角、正文排版、明暗表面 | `assets/css/main.css` | `@theme`、`@theme inline`、`:root`、`.dark`、`.content-prose` |
| 2. 原生模板 | 页面结构、信息顺序、响应式布局、跨语言回退 | `layouts/` | 首页五区块、列表行、文章页、顶栏、页脚 |
| 3. 结构化内容 | 文案与条目的真实来源 | `content/` 的 Front Matter 与 Markdown | 首页 `pillars` / `dossiers`、栏目标题与导语、履历正文 |

辅助配置只负责站点行为，不承担视觉覆盖：

- `config/_default/params.yaml`：站点名、描述、数学渲染开关。
- `config/_default/menus.en.yaml` / `menus.zh.yaml`：双语导航。
- `i18n/en.yaml` / `i18n/zh.yaml`：界面词条，使用 `key: { other: "..." }` 的 YAML 展开形式。
- `data/authors/me.yaml`：作者事实与公开链接；页脚 GitHub 链接由此读取。

**禁止**在模板中写内联 `style=`、硬编码 hex、引入第二套 UI 框架，或复制第三方主题模板作为覆盖层。

---

## 3. 页面架构

### 3.1 首页：思想档案与工程目录

`layouts/index.html` 读取 `content/_index.md` 与 `_index.zh.md`，固定为五段：

1. **纲领与定位**：`identity_line`、庄重引文 `guiding_quote`、三主线 `pillars`。
2. **实证专案**：`dossiers` 细线行，包含 `/viz2.html` 直达入口。
3. **精选工程**：默认读取 `projects` 最新 4 项。
4. **精选研究**：默认读取 `publications` 最新 4 项。
5. **近期纪要**：读取 `blog` 最新 5 项；容器必须保留 `id="news"`，兼容 `/#news`。

首页无 Hero、无大头像、无简历下载按钮、无宣传卡片。模块之间只使用垂直留白与 `1px` 细线，不使用背景色块、渐变网格或卡片阴影。

### 3.2 栏目与文章

- `layouts/_default/list.html`：标题、简短导语、按日期倒序的细线索引行。
- `layouts/_default/single.html`：680px 文章栏、元信息、可折叠目录、正文、上下篇导航。
- `layouts/_partials/page-links.html`：论文与工程链接的细边微标签。
- `content/experience.md` / `experience.zh.md`：客观的研究、工程、教育、技术方向与公开联系方式，不做求职简历。

### 3.3 双语回退

`layouts/_partials/get-section.html` 是首页和栏目列表共用的查询入口：

1. 先读取当前语言站点的对应 `Section`。
2. 当前语言没有任何条目时，回退到 `site.Sites.Default` 的同栏目条目。
3. 回退结果由模板在标题旁显示灰阶 `[EN]`。
4. 默认语言也没有条目时，显示 `i18n "no_content"`，不报错。

该机制是“整栏目为空时回退”，不是逐篇混合两种语言。新增中文栏目入口使用 `_index.zh.md`；新增中文文章使用与英文页面对应的 `.zh.md` 文件。

---

## 4. 冷调学术现代主义：量化指纹

| 维度 | 主站标准 |
|---|---|
| 强调色数量 | **1**，只允许深海青蓝及其 hover / dark 档 |
| 浅色主色 | `#1E4D6B`；hover `#163A52` |
| 暗色主色 | `#5A8FAD`；hover `#76A8C4` |
| 浅色表面 | 页面 `#FFFFFF`；次级 `#F8FAFC`；细线 `#E2E8F0` |
| 暗色表面 | 页面 `#0F141A`；次级 `#161D26`；细线 `#24303F` |
| 浅色文字 | 主文字 `#1E293B`；次级 `#64748B` |
| 暗色文字 | 主文字 `#E2E8F0`；次级 `#94A3B8` |
| 字体 | Inter + 中文系统无衬线回退；等宽字体只用于编号、日期与代码 |
| 基准字号 | `html { font-size: 15px; }` |
| 正文 | `1rem`、`line-height: 1.65`、`max-width: 680px` |
| 页面容器 | `max-w-3xl` 居中，左右 `px-5`；首页与内页一致 |
| 圆角 | `sm = 2px`；其余常用圆角令牌统一收敛到 `4px` |
| 分隔 | `1px solid var(--border-hairline)`，由 `border-hairline` 等工具类使用 |
| 庄重引文 | 非斜体、无装饰引号、左侧 `3px` 主色竖线、次级灰阶底色 |
| 链接 | 主色 + 下划线，去色后仍可辨识 |
| 明暗模式 | `<html>` 上的 `.dark`；首次绘制前初始化，跟随本地选择或系统偏好 |
| 动效 | **零滚动动画、零视差、零轮播**；只保留必要的状态反馈 |

### 反风格清单：出现即回退

1. 第二个彩色强调体系、彩虹标签、彩色渐变或发光边框。
2. 主站使用暖纸色、米黄底或纯黑 `#000`；暗色页面不得浅于 `#0F141A` 的最深表面约定。
3. 大 Hero、大头像、装饰插画、玻璃拟态、厚阴影、大圆角或胶囊按钮。
4. 衬线正文、装饰性手写字体、夸张标题字号或靠字重制造营销感。
5. 滚动动画、视差、自动轮播、打字机等持续运动。
6. `Download CV`、奖项墙、`Skills & Hobbies`、生活化技能条或求职 CTA。
7. 把独立专案的暖纸色、衬线标题、数据辅助色直接带回主站。
8. 在模板里写 `style=`、任意 hex，或绕过令牌创建平行色彩系统。

### 快速复核

- **去色测试**：转为灰度后，标题、日期、摘要、链接与分区层级仍清楚。
- **三色测试**：除主色与中性灰阶外，不应出现另一种彩色 UI。
- **盲测**：遮住站点名，仍应像研究者工作档案与工程目录，而非简历或营销页。

---

## 5. 双轨制与去简历化原则

| 轨道 | 主站 | 独立 HTML 专案 |
|---|---|---|
| 职责 | 战略叙事、思想档案、工程与研究索引 | 深度实证、交互呈现、公共议题传播 |
| 受众 | 专业界、技术评估者、长期读者 | 需要完整证据与可读解释的广泛受众 |
| 技术 | Hugo + Tailwind + 双语结构化内容 | 自包含 HTML / CSS / JS，独立维护 |
| 视觉 | 冷调、单主色、细线、微圆角、零动画 | 可拥有适合报告自身的排版与数据语义色 |
| 入口 | 首页实证专案、工程栏目导语 | 通过明确链接进入，如 `/viz2.html` |

边界规则：

- 主站样式不会传入独立专案；不要为了“统一”而重写专案的阅读风格或数据色彩。
- 主站只提供准确、克制的专案标题、摘要与入口，不嵌入大型交互 iframe。
- 新专案优先追加到首页 `dossiers`，并在工程栏目保留清晰入口。
- `static/viz2.html` 与 `static/uploads/` 是受保护资产；任何修改都需独立、明确的任务授权。
- 履历页记录事实与公开成果，不把首页或栏目重新包装成求职材料。

---

## 6. 文件地图

| 需求 | 文件 |
|---|---|
| 全站颜色、字体、圆角、正文、表格、代码、引文 | `assets/css/main.css` |
| CSS 构建、主题初始化、字体、KaTeX | `layouts/_partials/head.html` |
| 页面外壳与容器 | `layouts/_default/baseof.html` |
| 顶栏、移动导航、语言切换、明暗切换 | `layouts/_partials/header.html` |
| 页脚、RSS、GitHub | `layouts/_partials/footer.html` |
| 首页五区块 | `layouts/index.html` |
| 首页双语文案、主线、专案 | `content/_index.md`、`content/_index.zh.md` |
| 栏目索引与 `[EN]` 回退 | `layouts/_default/list.html` |
| 单页与文章 | `layouts/_default/single.html` |
| 共用栏目查询与默认语言回退 | `layouts/_partials/get-section.html` |
| 工程 / 论文 / 动态链接 | `layouts/_partials/page-links.html` |
| 双语导航 | `config/_default/menus.en.yaml`、`menus.zh.yaml` |
| 双语界面词条 | `i18n/en.yaml`、`i18n/zh.yaml` |
| 中文工程、研究、纪要入口 | 对应栏目的 `_index.zh.md` |
| 双语履历 | `content/experience.md`、`content/experience.zh.md` |
| 独立实证专案 | `static/viz2.html` |
| CI 与生产部署 | `.github/workflows/ci-build-check.yml`、`deploy.yml` |

---

## 7. AI 修改速查

1. **换主色**：只改 `main.css` 的 `@theme` 与 `.dark` 主色 / hover；同时检查 light、dark 和链接去色可辨识度。
2. **改正文宽度、字号、行高**：改 `.content-prose`；改全站 rem 基准才动 `html`，并复核导航与移动端。
3. **改首页文案**：改双语 `_index` Front Matter，不把文案硬编码进模板。
4. **新增独立专案**：在双语 `dossiers` 数组追加 `year / title / summary / url / tag`，保留真实静态文件入口。
5. **改首页结构或条目数量**：改 `layouts/index.html`，保留五段顺序与 `id="news"`。
6. **改栏目列表样式**：改 `list.html`；查询与回退统一走 `get-section.html`，不要复制另一套回退逻辑。
7. **新增中文栏目 / 文章**：添加 `_index.zh.md` 或对应 `.zh.md`；无中文条目时依靠自动 `[EN]` 回退。
8. **改导航**：分别修改 `menus.en.yaml` 与 `menus.zh.yaml`，检查桌面、移动和 `/zh/` 路径。
9. **改界面词语**：在双语 i18n 文件同时新增 `other` 映射，不在模板写死可翻译标签。
10. **改构建**：两套工作流同步修改 Node / pnpm / Hugo / install / build；生产只保留 Pages 必需的权限、baseURL 与部署步骤。
11. **完成修改后**：检查模板分隔符、空列表保护、双语路由、`/#news`、`/viz2.html`、light / dark、375px / 768px / 1440px，并运行 `hugo --minify`；本地无二进制时明确说明仅完成静态校验，以 CI 为准。
