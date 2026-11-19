# 站点视觉风格规范（AI 可执行版）

> 本文件是本站**视觉风格的唯一权威描述**。任何 AI 或人在修改样式前，先读第 5 节建立「这网站该长什么样」的基准，
> 再读第 2 节确认「改哪一层」，最后用第 6 节的「修改配方」定位到具体文件与键名。
> 不要凭截图猜样式，不要新增 CSS 框架，不要硬编码颜色。

> **风格识别卡**：冷调、低饱和、单一深海青蓝强调色的**学术现代主义**。
> 观感关键词：安静、克制、可信、条理清晰、纸卡感、工程气。
> 一句话：像一份排版精良、会呼吸的论文预印本，而不是温暖书斋、SaaS 落地页或炫技作品集。

- 站点：`Chiron`（Zhehao Jin），学术个人主页 / CV
- 模板：Hugo Blox `academic-cv`，Tailwind CSS v4
- 线上地址：`https://jzhchiron.github.io/`
- 部署：GitHub Actions → GitHub Pages（push `main` 自动构建）

---

## 1. 技术与构建基线

| 项目 | 值 | 位置 |
|---|---|---|
| 静态站生成器 | Hugo `0.152.2`（extended） | [hugoblox.yaml](hugoblox.yaml) |
| 主题模块 | `blox-tailwind`、`blox-plugin-netlify`、`blox-plugin-reveal` | [config/_default/module.yaml](config/_default/module.yaml) |
| CSS 方案 | Tailwind CSS `^4.1.12` + `@tailwindcss/typography` | [package.json](package.json) |
| 包管理器 | pnpm `10.14.0`，Node 20（CI） | [package.json](package.json)、[deploy.yml](.github/workflows/deploy.yml) |
| 本地开发 | `pnpm dev`（= `hugo server --disableFastRender`） | package.json |
| 构建 | `pnpm build`（= `hugo --minify`） | package.json |
| 内容语言 | 仅 `en`，`hasCJKLanguage: false` | [languages.yaml](config/_default/languages.yaml)、[hugo.yaml](config/_default/hugo.yaml) |

重要：主题模块（`blox-tailwind`）**不在本仓库内**，由 Hugo Modules 拉取。
→ 主题的 SCSS/Tailwind 源码、默认色板、默认间距刻度、各 `block` 模板都**不可直接编辑**，
只能通过下面第 2 节的 4 层变量去覆盖。

---

## 2. 样式控制链（修改前必读）

样式按以下 4 层生效，**层号越大优先级越高**：

| 层 | 作用范围 | 位置 | 典型内容 |
|---|---|---|---|
| L1 全局令牌 | 全站 | [config/_default/params.yaml](config/_default/params.yaml) → `hugoblox.theme` / `typography` / `layout` / `header` / `footer` / `copyright` | 明暗模式、色板、字体、圆角、间距密度、导航形态 |
| L2 页面级 | 单个页面 | 各 `content/**/*.md` front matter `design:` | 页面区块间距、页面整体背景 |
| L3 区块级 | 单个区块 | `sections[].design`（首页与 landing 页） | `view`、`columns`、`spacing`、`background`、`css_class` |
| L4 自定义 CSS | 全站 | [assets/css/custom.css](assets/css/custom.css) | 原始 CSS 覆盖（当前仅 3 条规则） |

判断规则：
- 颜色 / 字体 / 圆角 / 全局密度 → **只改 L1**，不要在 L4 写覆盖。
- 单页留白、区块排布 → **只改 L2/L3**。
- 主题没有暴露成 token 的细节（如正文宽度、基础字号） → 才写 L4，且必须是最小改动。
- **禁止**修改 `layouts/` 下的模块镜像去改样式；`layouts/_partials/hooks/head-end/github-button.html` 只负责注入 GitHub 按钮脚本。

---

## 3. 设计令牌现状（当前生效值）

### 3.1 品牌与身份

| 键 | 当前值 | 说明 |
|---|---|---|
| `hugoblox.identity.name` | `Chiron` | 导航栏、页脚、标题、版权名 |
| `hugoblox.identity.type` | `person` | Schema.org 结构化数据类型 |
| `hugoblox.identity.favicon` | `/favicon.ico` | 站点图标 |
| `hugoblox.identity.description` | `Chiron's Academic Homepage. Focusing on Number Theory, Model Theory, Artificial Intelligence, and Philosophy...` | SEO / 社交分享描述 |
| `hugoblox.identity.social.twitter` | `BuildLore` | Twitter Card |
| `hugo.title` | `Chiron` | 浏览器标题 |

### 3.2 颜色与明暗

| 键 | 当前值 | 可选值 / 效果 |
|---|---|---|
| `hugoblox.theme.mode` | `system` | `light` / `dark` / `system`（跟随系统） |
| `hugoblox.theme.pack` | `default` | 主题色包名；也可写成 `{ light: "x", dark: "y" }` |
| `hugoblox.theme.colors.primary` | `#1E4D6B`（深海青蓝，与两份 `viz2.html` 的 `--blue` 同值） | Tailwind 色名或 hex，如 `indigo` / `#1E4D6B` |
| `hugoblox.theme.colors.secondary` | `""` | 次色 |
| `hugoblox.theme.colors.neutral` | `""` | 控制 `text-gray-*` / `bg-gray-*` 系列 |
| `hugoblox.theme.colors_light` | `{}` | 仅浅色模式生效的覆盖 |
| `hugoblox.theme.colors_dark.primary` | `#5A8FAD`（同色相浅一档，保证暗色下对比度） | 仅暗色模式生效的覆盖 |
| `hugoblox.theme.surfaces.background` / `foreground` | `""` | 页面底色 / 前景色（保持主题默认白底） |
| `...surfaces.header.background` / `foreground` | `""` | 导航栏底色 / 文字色 |
| `...surfaces.footer.background` / `foreground` | `""` | 页脚底色 / 文字色 |

要点：
- 站点所有彩色元素（链接、按钮、标签、引文边线、技能条）都走 `primary-*` 语义色，
  因此 **改 `primary` 一处 = 全站换主色**。
- 主色为深海青蓝 `#1E4D6B`，已与两份 `viz2.html` 的 `--blue` 对齐；改主色时必须同步改这两份文件，
  暗色档 `#5A8FAD` 与其 `--blue2` / `--b2` 对齐。
- 正文灰阶走 `neutral`，改它会同时影响边框、次级文字、卡片描边。
- `primary` / `colors_dark.primary` 已显式指定 hex；`secondary`、`neutral` 仍由主题包内部定义，本仓库看不到。

### 3.3 字体与排版

| 键 | 当前值 | 可选值 |
|---|---|---|
| `hugoblox.typography.font` | `sans`（Inter） | `sans`（Inter）/ `serif`（Roboto Slab）/ `native`（系统字体） |
| `hugoblox.typography.size` | `md` | `sm` / `md` / `lg` / `xl` |
| `html { font-size }`（L4） | `15px` | 全站 rem 基准（浏览器默认 16px） |
| `.content` 宽度（L4） | `max-width: 680px` | 正文栏宽 |
| `.content` 字号（L4） | `1rem`（15px） | 正文大小（降低普通读者的长文阅读门槛） |
| `.content` 行高（L4） | `1.65` | 正文行高（窄栏小字需更舒展） |

派生影响（改 `html` 字号会等比缩放全站）：
- Tailwind 的 `text-sm` = 0.875rem → 13.1px，`text-base` = 1rem → 15px，`text-xl` = 1.25rem → 18.75px。
- L2/L3 的 `spacing: '3rem'` / `'5rem'` 同样按 15px 计算 → 45px / 75px。
- **当前站点比模板默认整体小约 6.25%**，这是刻意设置；改回 16px 会让所有页面变大变松。

### 3.4 形状与密度

| 键 | 当前值 | 可选值 | 视觉结果 |
|---|---|---|---|
| `hugoblox.layout.radius` | `sm` | `none` / `sm` / `md` / `lg` / `full` | 卡片、按钮、输入框圆角（当前为利落的微圆角） |
| `hugoblox.layout.spacing` | `comfortable` | `compact` / `comfortable` / `spacious` | 全站区块内边距与行距密度 |
| `hugoblox.layout.avatar_shape` | `circle` | `circle` / `square` / `rounded` | 作者头像形状（首页 `design.avatar.shape` 会覆盖单页） |

### 3.5 头部与导航

| 键 | 当前值 | 说明 |
|---|---|---|
| `header.enable` | `true` | 启用导航栏 |
| `header.style` | `navbar` | `navbar` / `navbar-simple` / `minimal` |
| `header.sticky` | `true` | 滚动吸顶 |
| `header.align` | `center` | `left` / `center` / `right`（菜单居中） |
| `header.search` | `true` | 搜索按钮 |
| `header.theme_toggle` | `true` | 明暗切换按钮 |
| `header.theme_picker` | `true` | 主题色选择器 |
| `header.language_switcher` | `true` | 语言切换（当前仅 en，实际不显示内容） |
| `header.cta.enable` | `false` | 导航右侧 CTA 按钮（关闭） |

导航项（顺序 = `weight`）见 [menus.yaml](config/_default/menus.yaml)：
`Agenda(/)` → `Projects(/projects/)` → `Papers(/publications/)` → `News(/#news)` → `Experience(/experience/)`

> 去简历化后只保留 5 项“工作目录”式入口；`Talks(/events/)`、`Courses(/courses/)` 页面仍然存在，但不再占用导航。
> `Papers` 指向真实存在的 `content/publications/`；不要使用无目录、无别名的 `/papers`。

### 3.6 页脚与版权

| 键 | 当前值 |
|---|---|
| `footer.style` | `minimal`（可选 `minimal` / `columns` / `centered`） |
| `footer.language_switcher` | `true` |
| `footer.text` | `""` |
| `copyright.notice` | `© {year} {name}. This work is licensed under {license}` |
| `copyright.license.type` | `cc`（`allow_derivatives: false`、`share_alike: true`、`allow_commercial: false`） |

### 3.7 内容功能开关（影响页面观感）

| 键 | 当前值 | 影响 |
|---|---|---|
| `content.math.enable` | `true` | 数学公式渲染 |
| `content.toc.enable` | `true` | 文章目录 |
| `content.reading_time.enable` | `true` | 阅读时长标签 |
| `content.citations.style` | `apa` | 文献引用格式（`apa`/`mla`/`chicago`/`ieee`） |
| `search.enable` | `true` | 全站搜索 |
| `comments.enable` | `false` | 评论关闭 |
| `analytics.*` | 全空 | 无统计脚本 |
| `pro.hide_attribution` | `false` | 页脚保留 HugoBlox 署名 |

### 3.8 唯一全局 CSS 覆盖文件

[assets/css/custom.css](assets/css/custom.css) 当前全文（**完整就是这 4 段**）：

```css
html { font-size: 15px; }

.content {
  max-width: 680px;
  font-size: 1rem;         /* 15px：降低普通读者的长文阅读门槛 */
  line-height: 1.65;       /* 主题无对应 token：窄栏 + 小字需要更舒展的行距 */
}

/* 主题无 token：核心议程引文需要主色细边来承载庄重声明，并去掉斜体装饰 */
.content blockquote {
  border-left: 3px solid var(--color-primary-600, var(--color-primary-500, currentColor));
  font-style: normal;
}
```

规则：这里是"主题没给 token 的补丁层"。新增规则必须：
1. 选中已有的语义类（如 `.content`），不要用大范围元素选择器；
2. 颜色用 `var(--...)` 或 Tailwind 的 `theme()`，不写死 hex；
3. 每条规则加一行注释说明「为什么 token 做不到」。

---

## 4. 页面与区块结构

### 4.1 首页 [content/_index.md](content/_index.md)

`type: landing`，页面级 `design.spacing: '3rem'`（比子页更紧凑）。

| # | block | 关键 design | 视觉结果 |
|---|---|---|---|
| 1 | `resume-biography-3` | `background.gradient_mesh.enable: false`；`name.size: md`；`avatar.size: small`（150px）；`avatar.shape: circle` | 首屏：纯底色 + 小圆头像 + 姓名/职位/简介 + `Work Overview` 入口 |
| 2 | `markdown` | `columns: '1'` | 「Working Agenda」：指导思想引文（blockquote）+ 三条工作主线 |
| 3 | `collection`（id `projects`） | `view: article-grid`、`columns: 2`、`fill_image: false`、关闭日期/阅读时长/阅读更多 | 实干成果双列文字卡；区块文案内含独立报告 `/viz2.html` 入口 |
| 4 | `collection`（id `papers`） | `view: article-grid`、`columns: 2`、`filters.featured_only: true` | 精选论文双列卡片 |
| 5 | `collection`（Recent Publications） | `view: citation` | 文献列表式（APA 引用行） |
| 6 | `collection`（id `news`） | `page_type: blog`、`count: 10`、`order: desc`、`view: card`、`spacing.padding: [0,0,0,0]` | 新闻卡片列表，锚点 `#news` |

首页现为 **6 个区块**，顺序固定为「议程 → 成果 → 论文 → 动态」。`talks` 区块已移除，`/events/` 不再出现在首页与导航。
模板自带、带 `demo: true` 的「Build your own academic website」`cta-card` 已删除；
如未来需要开源工具入口，按 R12 新增自有区块，不恢复模板宣传语。

区块顺序 = 数组顺序；删除区块 = 删除对应 `- block:` 项；新增区块 = 往 `sections` 追加。

### 4.2 [content/experience.md](content/experience.md)

`type: landing`，`design.spacing: '5rem'`。区块：
`resume-experience`（`date_format: 'January 2006'`，`is_education_first: false`）
→ `resume-skills`（标题 `Technical Stack / Focus Areas`）
→ `resume-languages`（标题 `Languages`）

`resume-awards` 已移除（数据文件里的 `awards` 仍保持注释状态），不再展示空的奖项区块；
技能区标题从 `Skills & Hobbies` 改为客观的 `Technical Stack / Focus Areas`，避免“求职简历 + 个人生活秀”的观感。
数据全部来自 [data/authors/me.yaml](data/authors/me.yaml)。

### 4.3 [content/projects/_index.md](content/projects/_index.md)

`spacing: '5rem'`；`collection` + `view: article-grid` + `columns: 3`，
并关闭 `show_date` / `show_read_time` / `show_read_more`，`fill_image: false`
→ 三列纯文字卡网格，无元信息。区块文案使用中性工作档案措辞，并包含 `/viz2.html` 独立报告入口。

### 4.4 [content/courses/_index.md](content/courses/_index.md)

`type: landing` + `cascade` 把所有子页设为 `type: docs` 且显示面包屑；
`collection` + `view: article-grid` + `columns: 1` → 单列纵向课程卡。

### 4.5 列表页视图

| 页面 | front matter | 视觉 |
|---|---|---|
| [blog/_index.md](content/blog/_index.md) | `view: article-grid` | 博客索引卡片网格 |
| [publications/_index.md](content/publications/_index.md) | `view: citation` | APA 引用式列表 |
| [events/_index.md](content/events/_index.md) | `view: card` | 活动卡片 |

### 4.6 文章页封面样式（示例）

[content/blog/data-visualization/index.md](content/blog/data-visualization/index.md) 的 `cover`：

```yaml
cover:
  image: "https://images.unsplash.com/photo-1557682250-33bd709cbe85?q=80&w=2560"
  position: { x: 50, y: 40 }
  overlay: { enabled: true, type: "gradient", opacity: 0.4, gradient: "bottom" }
  fade:    { enabled: true, height: "80px" }
  icon:    { name: "✨" }
```

这是文章头图的**标准写法**：底部渐变遮罩 40% + 80px 渐隐 + emoji 图标，保证标题始终可读。
新增文章沿用同一组参数，不要每篇自定义遮罩强度。

### 4.7 独立页面 [static/viz2.html](static/viz2.html)（不受站点主题控制）

这是一份**独立的自包含 HTML 报告**（约 80KB，含自己的 `<style>`），完全不走 Tailwind 与 L1 令牌。
仓库里有两份内容相近的副本：[static/viz2.html](static/viz2.html) 与
[content/projects/pandas/viz2.html](content/projects/pandas/viz2.html)，后者变量用全名，前者用缩写（`--b2`、`--sur`、`--bdr`、`--txt`、`--mut`）。
它自带的视觉体系是「暖纸色学术风」（以下为全名版）：

```css
--bg:#F4F1EB;  --paper:#FDFBF7; --surface:#FFFFFF; --border:#DDD9CE;
--blue:#1E4D6B; --blue2:#5A8FAD; --orange:#C0592A; --green:#2E7D5E; --teal:#2A7D8E;
--text:#1A1917; --muted:#6E6A62;
--serif:'Noto Serif SC'; --sans:'Noto Sans SC'; --mono:'IBM Plex Mono';
--r:10px; --sh:0 2px 16px rgba(0,0,0,.07);
```

特征：15px 正文、1.75 行高、52px 高磨砂吸顶导航、`#1A3A54→#1E4D6B→#1E6B52` 135° hero 渐变、
10px 圆角卡片、左侧 3px 色条强调块。
它的主色 `--blue` / `--blue2` 已与站点 `primary` 对齐，但**它仍是独立文档**：
站点级改字体、圆角、底色对它无效，只能直接编辑文件；它的暖纸色底也不代表站点底色。

---

## 5. 视觉风格画像（"是什么风格 / 给人什么观感"的基准）

> 本节回答三件事：**它是什么风格**、**它给人什么观感**、**出现什么变化就不再是这个风格**。
> 修改前用它校准，修改后用它复核；§5.8 是最快的判定方法。

### 5.1 风格命名与族谱

**主名：冷调学术现代主义（Cool Academic Modernism）。**

它不是某个模板的默认长相，而是三股设计传统的交集：

| 来源 | 借来的东西 | 页面上的表现 |
|---|---|---|
| 瑞士国际主义排版 | 网格、无衬线、左对齐、去装饰、用留白建立层级 | 单栏正文、栅格卡片、强标题层级、无花纹无边框装饰 |
| 学术工作档案 / 论文排版 | 引用列表、目录、阅读时长、APA 引用、克制的信息密度 | `citation` 视图、`#news` 锚点、阅读时长、文献行（简历结构已弱化为工作记录） |
| 当代工程极简界面 | 低饱和单主色、微圆角、吸顶导航、明暗切换 | `primary-*` 单色体系、`radius: sm`、半透明吸顶 navbar |

**参照坐标：**

- 像：欧美高校研究者的工作档案页；排版良好的论文预印本；克制版的 Linear / Vercel 文档界面。
- 不像：求职简历模板；Notion 暖米色模板；作品集式的满屏大图与滚动动效；SaaS 营销页的彩色渐变与玻璃拟态；复古报刊的衬线大标题。

**双轨战略：两套并存的视觉体系（不要混用）：**

| | 站点本体（Hugo 主题） | 独立报告 `viz2.html` |
|---|---|---|
| 定位 | 技术底座与工作档案，面向技术界与专业评估者 | 深度实证 / 普惠解读 / 数据反制专案，面向更广泛受众 |
| 入口 | 主导航 + 首页 Selected Work 区块 | 主站项目区块内的明确链接，视觉隔离 |
| 底色 | 白 / 主题深色表面 | 暖纸色 `#F4F1EB` |
| 字体 | Inter 无衬线 | Noto Serif SC + Noto Sans SC + IBM Plex Mono |
| 圆角 | `sm` 微圆角 | 10px 卡片 |
| 气质 | 冷、干净、界面感、实干 | 暖、书卷、报告感、可传播 |

二者只共享同一种主色（`#1E4D6B`）。主站负责“可信的工程底座”，报告负责“可读的公共证据”；
站点永远保持白底冷调，不要把 `viz2.html` 的暖纸色、衬线标题、橙绿辅色带回站点。

### 5.2 观感分层：从第一眼到久读

**远看（0.1–3 秒）：一张被放大排版的论文首页。**
大面积白、极少蓝、没有装饰插画，也没有背景渐变或大图。首屏只有纯底色、一个小圆头像和几行文字，像一份刚打印出来的工作说明；先看到姓名，随即被下方的引文与三条工作主线接住。

**近看（3–30 秒）：一种颜色同时编码"重点"与"可操作"。**
链接、按钮、标签、技能条、引文边线全部来自同一个 `primary`。没有第二个彩色争夺注意力，视线不必在多种颜色之间切换，认知负担很低；"哪里能点、哪里是重点"由同一种蓝统一回答。

**久读（10 分钟以上）：像一本排版克制的学术小册子。**
15px 字号、1.65 行高、680px 栏宽，接近印刷品的正文密度。字号比常见网页小一档，让页面显得"资料化、专业化"，而不是"消费级营销"；舒展的行距又把小字带来的拥挤感抵消掉。

**触感：平、薄、纸卡感。**
`radius: sm` 的微圆角，配合克制的边线与阴影，让卡片和按钮像裁切整齐的纸卡或金属薄片，而不是气泡、胶囊或塑料。没有厚投影、没有玻璃拟态，界面是"平"的。

**光感：冷白光。**
白底 + 冷蓝，整体像 6500K 的屏幕白或实验室照明，不是米黄纸张的暖光。唯一偏暖的东西来自内容图片本身。

**暗色模式：夜海，不是霓虹。**
主色提亮到 `#5A8FAD`（同色相、进一步降饱和），表面与文字交给主题深色灰阶；蓝色仍是画面里唯一的彩色。明暗切换应像"关掉顶灯"，而不是"打开霓虹"。

### 5.3 情绪坐标（风格雷达）

0–10 打分，**10 表示更靠近左侧端点**：

| 维度 | 读数 | 解读 |
|---|---|---|
| 冷 ↔ 暖 | 8.5 冷 | 深海青蓝 + 冷白，几乎无暖色 |
| 静 ↔ 动 | 8.5 静 | 不做滚动叙事、视差、自动轮播 |
| 克制 ↔ 张扬 | 9 克制 | 无大色块、无插画、无夸张字重 |
| 简 ↔ 繁 | 7.5 简 | 单一强调色 + 单一字体家族 + 少量形状 |
| 硬 ↔ 软 | 6 偏硬 | 微圆角、细边，利落但不锋利 |
| 疏 ↔ 密 | 6 偏舒 | 区块留白舒适，正文信息密度中上 |
| 学术 ↔ 商业 | 8.5 学术 | 引用列表、阅读时长、CV 结构 |
| 权威 ↔ 亲和 | 6.5 偏权威 | 冷蓝 + 小字 + 结构感带来可信，而非亲切 |

读法：这些分数是改稿的报警线。把任一维度向反方向推 3 分以上（例如改成暖色、加大圆角、加入动效），页面就会滑向另一种风格。

### 5.4 量化指纹（可测量，不靠感觉）

| 维度 | 当前值 | 换算 / 说明 | 对观感的贡献 |
|---|---|---|---|
| 强调色数量 | **1** | 仅 `primary-*`，无第二彩色 | 冷静、统一、低噪音 |
| 主色 | `#1E4D6B` | ≈ `hsl(203 56% 27%)` | 深海青蓝：可信、理性、微技术感 |
| 暗色主色 | `#5A8FAD` | ≈ `hsl(202 34% 52%)` | 同色相提亮，保持身份连续 |
| 饱和度上限 | 56%（亮）/ 34%（暗） | 低饱和 | 不喧闹、不"互联网感" |
| 中性色 | 白 + 主题灰阶 | `neutral` 未覆盖 | 页面像白纸/白墙，不偏黄 |
| 字体 | Inter（`sans`） | 单一家族 | 界面感、文档感、中性 |
| 基准字号 | 15px | 0.9375 × 浏览器默认 | 比常见网页小约 6.25%，更"资料化" |
| 正文字号 | 1rem = 15px | `custom.css` | 与基准一致，不额外放大 |
| 正文栏宽 | 680px | ≈ 45em ≈ 75ch | 长文可读区间的上沿 |
| 正文行高 | 1.65 | 窄栏小字需更舒展 | 久读不累 |
| 正文字号跨度 | 13.1–18.75px | `text-sm` → `text-xl`（不含标题层级） | 元信息与正文差距小，层级不靠夸张字号 |
| 圆角 | `sm` | 主题微圆角档 | 利落、纸卡感，不成胶囊 |
| 区块节奏 | 首页 45px / 子页 75px | `3rem` / `5rem` @15px | 首屏重、内页松 |
| 首屏背景 | 纯色，无装饰 | `gradient_mesh` 关闭 | 文档感、实干感，不做展示型 hero |
| 导航 | 5 项、居中、吸顶、半透明 | `navbar` | 稳定、可预期、不抢内容 |
| 页脚 | `minimal` | 无多列站点地图 | 收尾轻，不拖沓 |
| 文章封面遮罩 | 底部 40% 渐变 + 80px 渐隐 | 标准写法见 §4.6 | 图片上的标题始终可读 |
| 动效 | 仅必要状态反馈 | 不做滚动动画/视差/轮播 | 安静，注意力留给内容 |

### 5.5 七个要素如何共同产生这种观感

| 要素 | 做法 | 传递的信息 |
|---|---|---|
| 色彩 | 单一低饱和冷蓝 + 白/灰 | "这是机构与研究者，不是品牌营销" |
| 字体 | Inter 无衬线、单一家族、偏小字号 | "这是文档与工具，不是杂志" |
| 网格 | 居中窄栏 + 1/2/3 列卡片 | "信息有结构，可扫描" |
| 形状 | 微圆角、细边、轻层次 | "现代，但不轻浮" |
| 空间 | 首页紧、子页松、区块留白 | "有呼吸感，但不散" |
| 图像 | 圆形头像；封面统一遮罩；emoji 只作轻标记 | "人 + 内容"，不让图片变成主角 |
| 动效 | 只有必要的状态反馈 | "安静，别表演" |

### 5.6 风格关键词（检索、复现、给 AI 下指令时用）

- 中文：冷调、低饱和、深海青蓝、学术现代主义、单主色、无衬线、窄栏长文、微圆角、纸卡感、克制、可信、安静的界面、结构化、去装饰、留白。
- 英文：cool-toned, low-saturation, deep-sea teal-blue, academic modernism, single accent, Inter, narrow measure, micro-radius, flat surfaces, quiet UI, structured, restrained, trustworthy, editorial-technical.

可直接粘贴给其他 AI 的风格约束：

```text
保持冷调学术现代主义：白底或主题深色表面，唯一强调色是深海青蓝 #1E4D6B
（暗色 #5A8FAD）；正文 Inter 15px、680px 窄栏、1.65 行高；卡片微圆角、
细边、轻层次；导航居中吸顶、半透明；首屏为纯底色，不放渐变网格/大图/装饰。
禁止：第二强调色、暖纸色底、衬线正文、大圆角/胶囊、玻璃拟态、厚投影、
滚动动画/视差、彩色渐变、字体混搭、Download CV 式求职 CTA。
```

### 5.7 反风格：出现即跑偏

以下任意一条成立，就说明已经离开本站风格，应回退：

1. UI 里出现第二个彩色（红/绿/橙/紫的语义色块、彩色标签、彩虹渐变）。
2. 站点底色变成米黄、暖灰或纯黑；或把 `viz2.html` 的暖纸色带进站点。
3. 正文改用衬线字体，或标题使用装饰性 / 手写字体。
4. 主要卡片圆角超过 8px、出现胶囊按钮或大圆角"气泡"。
5. 正文栏宽超过 720px 或字号小于 14px，破坏久读体验。
6. 加入玻璃拟态、彩色投影、发光边框、3D 倾斜等炫技效果。
7. 加入滚动动画、视差、自动轮播、打字机等持续运动。
8. 用满屏大图 / 插画替代文字层级，或让 emoji 变成主要视觉语言。
9. 重新出现求职型元素：`Download CV`、`Skills & Hobbies`、奖项墙、把 Talks 或社交置于 Projects 之前。

### 5.8 三步风格距离测试

1. **去色测试**：截图转灰度，信息层级仍然清楚 → 结构优先，符合；若必须靠颜色才分得清主次，说明跑偏。
2. **三色测试**：除主色与灰阶外，UI 中还能找到第三种彩色吗？能 → 跑偏。
3. **盲测**：遮住姓名与 logo，页面看起来像"研究者工作档案 / 学术文档"，而不是"求职简历 / SaaS 落地页 / 设计作品集" → 符合。

### 5.9 一句话自述（供快速引用）

> 白底冷白、单一深海青蓝；首屏纯底色 + 小圆头像，像一份刚打印的工作说明；Inter 15px 窄栏、引文一条主色细边；微圆角纸卡、居中吸顶的五项导航；克制到几乎看不见设计，但议程、成果、论文、动态的次序一眼可读。

### 5.10 去简历化与双轨战略（结构性约束）

**目标不是"换文案"，而是改变页面在替谁说话。** 页面首先替"事"说话——科技底层工程、公共普惠实践、规则与技术反制——而不是替"求职者"说话。

执行原则：

1. **弱化个人推销**：首屏不用装饰性渐变网格；头像降为 `small`；不出现 `Download CV`；不放奖项墙或 `Skills & Hobbies`。
2. **议程优先**：首屏之后第一块必须是 `Working Agenda`（指导思想引文 + 三条工作主线），再由 `Projects` 承接落地成果，最后才是 `Papers` 与 `News`。
3. **工作目录式导航**：`Agenda / Projects / Papers / News / Experience` 五项；`Talks`、`Courses` 保留页面但不进导航。
4. **成果先于履历**：首页 `Projects` 先于 `Publications`；`/events/` 不再占据首屏。
5. **双轨不混色**：主站保持冷调、白底、Inter、单主色；`viz2.html` 体系承担"深度实证 / 普惠解读 / 数据反制"的公共传播，只通过主站项目区块的明确链接进入，不把暖纸色、衬线标题、橙绿辅色带回主站。

判定：修改后若首页重新出现"Download CV / 奖项墙 / 个人生活展示 / 会议社交优先 / 装饰性 hero"，即视为回退到 CV 风格。

---

## 6. 修改配方（文件 → 键 → 示例 → 副作用）

### R1 换主色
文件：`config/_default/params.yaml`
```yaml
hugoblox:
  theme:
    colors:
      primary: "#1E4D6B"    # 当前值（深海青蓝）；换色示例："indigo" / "#1F6F5C"
    colors_dark:
      primary: "#5A8FAD"    # 同步换同色相的浅一档
```
副作用：链接、按钮、标签、技能条、`gradient_mesh` 同时变化；暗色档由 `colors_dark.primary` 决定。
同时必须同步 `static/viz2.html` / `content/projects/pandas/viz2.html` 的 `--blue`、`--blue2`（见 R14）。
**不要**同时去改 `custom.css` 里的颜色。

### R2 切换明暗默认值
```yaml
hugoblox:
  theme:
    mode: "light"   # system → light / dark
```
若想让站点固定浅色且隐藏切换按钮，同时把 `header.theme_toggle` 设为 `false`。

### R3 换字体家族
```yaml
hugoblox:
  typography:
    font: "sans"    # 当前值；可选 serif(Roboto Slab) / native(系统字体)
```
副作用：只换家族，不改字号；`custom.css` 的 15px 基准继续生效。

### R4 调整全站字号
两个层级，先改令牌，不够再用 L4：
```yaml
hugoblox:
  typography:
    size: "lg"      # sm / md / lg / xl
```
仍不够时改 `assets/css/custom.css` 的 `html { font-size }`。
副作用：所有 rem 值（间距、圆角、字号）等比变化；改完必须检查首页 hero 与三列项目网格是否换行错位。

### R5 改圆角风格
```yaml
hugoblox:
  layout:
    radius: "sm"    # 当前值（利落微圆角）；更亲和用 md，直角用 none
```
注意 `full` 会把卡片变成胶囊，与当前"清爽实干"的卡片风格冲突，不建议。

### R6 改整体疏密
```yaml
hugoblox:
  layout:
    spacing: "spacious"   # compact / comfortable / spacious
```
单页留白另由 L2 `design.spacing` 控制（首页 `3rem`、子页 `5rem`），两者会叠加。

### R7 改首页首屏背景
文件：`content/_index.md` 第 1 个区块的 `design.background`
```yaml
design:
  background:
    gradient_mesh:
      enable: true            # 关闭纯色：false
    image:
      filename: hero-background.jpg   # 改用图片（已存在于 assets/images/）
      filters:
        brightness: 0.6
```
可选项：`gradient_mesh`（当前关闭，去简历化后首屏保持纯色）、`image`（配 `filters` 调亮度/模糊）、纯色。
三者**只启用一个**。

### R8 改头像
```yaml
# content/_index.md → 区块 1
design:
  avatar:
    size: large      # small(150) / medium(200) / large(320) / xl(400) / xxl(500)
    shape: circle    # circle / square / rounded
```
头像文件：`assets/media/authors/me.png`（替换时保持同名、正方形、≥500px）。
全局默认形状仍在 `hugoblox.layout.avatar_shape`。

### R9 改导航项 / 顺序
文件：`config/_default/menus.yaml`，`weight` 决定顺序（10、11、12…）。
```yaml
main:
  - name: 论文
    url: /publications/
    weight: 12
```
同时检查 `header.align`（当前居中）与移动端折叠后项数是否过多（当前 5 项）。

### R10 改正文栏宽 / 正文排版
文件：`assets/css/custom.css`
```css
.content {
  max-width: 680px;   /* 当前值；加宽示例：720px */
  font-size: 1rem;    /* 当前值；缩小示例：0.95rem */
  line-height: 1.65;  /* 当前值；更松散示例：1.75 */
}
```
仅影响 `.content` 容器（文章正文、markdown 区块），不影响列表页栅格。

### R11 加新的全局样式补丁
只允许追加到 `assets/css/custom.css` 末尾，禁止另建 CSS 文件（Tailwind 构建不会自动包含）。
```css
/* 主题无 token：首页论文卡标题与正文间距偏紧 */
.article-grid .article-title { margin-bottom: 0.4rem; }
```
新增前先确认 L1 是否已有对应键（颜色/字体/圆角/间距四类基本都有）。

### R12 新增首页 CTA 卡片（可选）
模板自带的 `demo: true` 示例区块已删除。确需开源工具入口时，在 `sections` 末尾新增自有区块，
不要恢复模板宣传语，也不要添加 `demo: true`：
```yaml
- block: cta-card
  content:
    title: 标题
    text: 正文
    button:
      text: 按钮
      url: /projects/
  design:
    card:
      css_class: "bg-primary-100 dark:bg-primary-900"
      css_style: ""
```
类名必须是 Tailwind 的 `primary`/`gray` 语义色，禁止 `bg-[#123456]` 任意值（会绕过主题换色）。

### R13 新增 landing 页面
复制 `content/projects/_index.md` 的骨架：
```yaml
type: landing
design:
  spacing: '5rem'
sections:
  - block: markdown
    content: { title: '标题', text: '正文' }
```
`view` 可选：`article-grid`（卡片网格）/ `card`（图卡）/ `citation`（文献行）/ `list`。
栅格宽度用 `columns: 1|2|3`，不要用自定义 `css_style` 调宽度。

### R14 让 `viz2.html` 与站点观感统一（已对齐，改色时需同步）
它不继承站点令牌，两处色彩是手动对齐的：
- 站点 `theme.colors.primary: #1E4D6B` ↔ 两份报告里的 `--blue`
- 站点 `theme.colors_dark.primary: #5A8FAD` ↔ `content/projects/pandas/viz2.html` 的 `--blue2` / `static/viz2.html` 的 `--b2`
- 浅色底纹 `#E8EFF4`（导航激活、标签、强调块）是该主色的固定淡色，换主色时一并替换

改站点主色时必须同步改这两份文件的 `:root`；两者变量命名不同（长名 / 缩写），按各自文件里已有的名字改，不要重命名。
它们的暖纸色底（`--bg: #F4F1EB`）是报告自身的阅读风格，**不随站点白底调整**；也没有暗色模式。

---

## 7. 修改约束（硬规则）

1. **不得**编辑 Hugo Modules 中的主题源码，也不得把主题文件复制进 `layouts/` 改样式。
2. **不得**在内容页写内联 `<style>` 或任意值 Tailwind 类（`bg-[#...]`、`w-[713px]`）来"解决"样式问题。
3. **不得**新增第二个强调色；所有彩色必须来自 `primary-*`。
4. **不得**为单页单独引入字体或 CSS 框架。
5. `assets/css/custom.css` 只做 token 做不到的事，且每次修改保持"最小、可解释、带注释"。
6. 修改 `html { font-size }`、`max-width`、`typography.size` 这类**全局放大缩小**的键，必须连带检查：首页 hero、三列项目网格、导航在 375px 宽的折叠形态。
7. 涉及颜色/主题包的修改，**必须同时检查 light 与 dark**（`mode: system`）。
8. 首页模板示例 CTA 已删除；未来新增 `cta-card` 必须使用真实站点内容，不得恢复 `demo: true` 或模板宣传语。

---

## 8. 验证清单（改完必过）

- [ ] `pnpm build` 无报错（本地无 Hugo 时以 CI `Deploy website to GitHub Pages` 通过为准；本地需 Hugo extended 0.152.2）。
- [ ] `/` 首屏：纯底色 + `small` 圆头像 + `Working Agenda`，无渐变网格 / `Download CV`，hero 未溢出。
- [ ] `/` 区块顺序为 Agenda → Selected Work → Featured Publications → Recent Publications → Recent News。
- [ ] `Selected Work` 区块文案中的 `/viz2.html` 链接可点，且报告保持暖纸色、与主站视觉隔离。
- [ ] `/#news` 锚点能跳到新闻列表。
- [ ] `/experience/`、`/projects/`、`/courses/` 三页栅格列数正确（1 / 3 / 1）；`/experience/` 无 Awards 区块，技能标题为 `Technical Stack / Focus Areas`。
- [ ] 任意文章页：`.content` 宽度与字号符合预期，代码块、表格、公式不溢出；`blockquote` 左侧 3px 主色细边、非斜体。
- [ ] light / dark 两套模式逐页目测一次。
- [ ] 375px / 768px / 1440px 三个宽度下无横向滚动、无文字截断。
- [ ] 去色测试：首页截图转灰度后，议程、成果、论文、动态的层级仍清楚。
- [ ] `assets/css/custom.css` 仍只包含必要规则，没有与 L1 令牌重复的定义。

---

## 9. 文件地图（关键词 → 位置）

| 想改的东西 | 文件 | 键 / 位置 |
|---|---|---|
| 主色、次色、灰阶 | `config/_default/params.yaml` | `hugoblox.theme.colors.*` |
| 明暗模式、主题包 | 同上 | `hugoblox.theme.mode` / `pack` |
| 背景/导航/页脚底色 | 同上 | `hugoblox.theme.surfaces.*` |
| 字体家族、字号档 | 同上 | `hugoblox.typography.*` |
| 圆角、疏密、头像形状 | 同上 | `hugoblox.layout.*` |
| 导航形态、吸顶、CTA | 同上 | `hugoblox.header.*` |
| 页脚样式、版权、许可 | 同上 | `hugoblox.footer.*` / `copyright.*` |
| 站点名、描述、favicon | 同上 | `hugoblox.identity.*` |
| 导航项与顺序 | `config/_default/menus.yaml` | `main[]` |
| 页面区块顺序/视图/列数 | `content/_index.md`、`content/*/_index.md`、`content/experience.md` | `sections[].design` |
| 页面留白 | 同上 | `design.spacing` |
| 首页 hero 背景、姓名字号、头像 | `content/_index.md` | 区块 1 `design.*` |
| 文章封面遮罩 | `content/**/index.md` | `cover.overlay` / `fade` / `icon` |
| 正文宽度、基础字号、正文行高 | `assets/css/custom.css` | `html`、`.content` |
| 作者信息、头像、技能、学历、链接 | `data/authors/me.yaml` | `me.yaml` 各字段 |
| 头像图片 | `assets/media/authors/me.png` | 同名替换 |
| favicon / 站点图标 | `static/`（`favicon.ico`、`favicon.svg`、`web-app-manifest-*.png`、`site.webmanifest`） | 同名替换（`assets/media/` 有并行副本） |
| CV 下载文件 | `static/uploads/resume.pdf` | 同名替换 |
| 独立可视化报告样式 | `static/viz2.html`、`content/projects/pandas/viz2.html` | 文件内 `:root` 与 `<style>` |

---

## 10. AI 执行速查

1. 用户说"换风格/换色/换字体" → 只动 L1（`params.yaml`）。
2. 用户说"某页太挤/太空" → 先 L2 `design.spacing`，再 L1 `layout.spacing`。
3. 用户说"某区块排布不对" → L3 `sections[].design`（`view` / `columns`）。
4. 用户说"正文太宽/字太小" → `assets/css/custom.css` 的 `.content`。
5. 用户说"某个元素长得不对" → 先确认主题是否有对应 token；没有才写 L4，且必须加注释。
6. 用户说"太像简历 / 去简历化" → 按 §5.10：先改首页结构与区块顺序（§4.1），再改导航与 Experience（§3.5 / §4.2），不要只换文案。
7. 任何修改完成后，执行第 8 节验证清单，并在回复中说明改了哪个文件、哪个键、影响了哪些页面。
