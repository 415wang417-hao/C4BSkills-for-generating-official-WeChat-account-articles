---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: 60ea5d0126731e33de81577b27285889_be46c71ebf3911f1887c525400de85a5
    ReservedCode1: kYqltJU7NHbPMY17yDo/Ks5l0KFPNVcTaeaklCGC0BzrkO403AFHgQ1bNBOM2plitD4k3XJU9V2kbKXeihuSVnEkjE8c2Z3kympmdgxlYKl9bZsob5UbNltXJG6VAo0Ivjact2Gagy7JtTSoB2n8Rm5nTpOXjvbJDySadG992kx35R4/kXhnJQjYuBo=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: 60ea5d0126731e33de81577b27285889_be46c71ebf3911f1887c525400de85a5
    ReservedCode2: kYqltJU7NHbPMY17yDo/Ks5l0KFPNVcTaeaklCGC0BzrkO403AFHgQ1bNBOM2plitD4k3XJU9V2kbKXeihuSVnEkjE8c2Z3kympmdgxlYKl9bZsob5UbNltXJG6VAo0Ivjact2Gagy7JtTSoB2n8Rm5nTpOXjvbJDySadG992kx35R4/kXhnJQjYuBo=
---

# 微信公众号文章生成技能 — 教学说明

> 技能名称：`wechat-publisher-pro` · C4B 定制版 · 作者：lenovo

本文档面向零基础用户，手把手教你安装并运行「公众号文章生成技能」，从安装 Python 依赖到发布一篇完整的公众号文章。

---

## 一、前置条件

| 要求 | 说明 |
|------|------|
| Python 3.7+ | 本技能需要 Python 运行环境。如未安装，访问 [python.org](https://www.python.org/downloads/) 下载 Windows 安装包 |
| pip | Python 包管理器，通常随 Python 安装自带。在终端输入 `python -m pip --version` 验证 |
| 一个 Markdown 源文件 | 你要发布的文章内容，格式为 `.md`（如 `my_article.md`） |
| （可选）一篇 Word 文档 | 格式为 `.docx`，也可作为输入 |

> 技能内置自动安装依赖机制——首次运行时会检测并自动安装 `markdown`、`beautifulsoup4`、`python-docx`、`lxml` 四个包，无需手动准备。

---

## 二、安装技能

### 1. 解压技能包

将技能包 `lenovo_C4B_wechat-publisher.skill`（ZIP 格式）解压到一个你喜欢的目录，例如：

```
C:\Users\lenovo\wechat-publisher-pro\
```

解压后目录结构：

```
wechat-publisher-pro/
├── SKILL.md          ← 技能指令（了解即可）
├── README.md         ← 本文件（详细使用说明）
├── scripts/
│   └── convert_to_wechat.py   ← 核心转换脚本（你主要跟它打交道）
├── references/
│   ├── wechat_styles.md       ← 样式参考
│   └── wechat_restrictions.md ← 公众号限制规则
└── examples/
    ├── sample_article.md      ← 示例输入
    └── sample_output.html     ← 示例输出
```

### 2. 进入项目目录

```powershell
cd C:\Users\lenovo\wechat-publisher-pro
```

---

## 三、一键转换（单文件）

这是最常用的操作——把一篇 Markdown 文章转换成公众号 HTML。

### 基本命令

```powershell
python scripts/convert_to_wechat.py 文章.md 输出.html
```

示例：

```powershell
python scripts/convert_to_wechat.py my_article.md wechat_article.html
```

### 加上主题

```powershell
python scripts/convert_to_wechat.py 文章.md 输出.html --theme tech
```

可选主题：

| 主题名 | 风格 | 主色 |
|--------|------|------|
| `default` | 静谧蓝 | `#1a73e8` |
| `tech` | 极客紫 | `#7c3aed` |
| `green` | 清新绿 | `#0f9d58` |
| `minimal` | 极简黑白 | `#111111` |

### 发布前自检

```powershell
python scripts/convert_to_wechat.py 文章.md 输出.html --theme tech --check
```

`--check` 会在生成 HTML 的同时输出一份 dry-run 报告，扫描违规标签、违规 CSS、外链图片等风险项。错误数 = 0 表示完全通过，可以直接粘贴到公众号编辑器。

---

## 四、高级用法

### 1. 使用 YAML Front Matter 设置元数据

在 Markdown 文件顶部加一段 YAML 元数据：

```markdown
---
title: 我的文章标题
author: lenovo
date: 2026-10-03
summary: 一句话摘要，会渲染成开头的摘要卡片
footer: © 2026 lenovo · 保留所有权利
theme: tech
---

正文从这里开始...
```

### 2. Callout 高亮框

在正文中使用 Callout 语法：

```markdown
> [!NOTE] 阅读收益
> 读完本文你将掌握公众号排版的完整流程。

> [!WARNING] 注意
> 公众号会静默删除 `<style>` 标签，所有样式必须 inline。

> [!TIP] 小技巧
> 用 `--check` 参数做发布前自检。

> [!PRACTICE] 动手试试
> 试试把 `my_article.md` 转换成四个主题看看效果。
```

支持四种标记：

| 标记 | 图标 | 语义 |
|------|------|------|
| `[!NOTE]` | 📘 | 知识 / 概念 |
| `[!PRACTICE]` | 🔧 | 实践 / 操作 |
| `[!WARNING]` | ⚠️ | 注意 / 风险 |
| `[!TIP]` | 💡 | 提示 / 技巧 |

同时支持中文别名：`[!知识]`、`[!实践]`、`[!注意]`、`[!提示]`。

### 3. 自动目录

默认开启。在 Markdown 正文任意位置写 `<!-- toc -->` 即可插入目录锚点；或使用 `--toc` 强制开启、`--no-toc` 关闭。

### 4. 本地图片 Base64 内嵌

技能会自动将同目录下的本地图片（PNG/JPG）转换为 base64 内嵌，粘贴到公众号编辑器时不会丢失。

如需关闭此功能：`--no-base64`

### 5. 发布前自检详细报告

```powershell
python scripts/convert_to_wechat.py 文章.md 输出.html --check --report report.json
```

`--report report.json` 会导出结构化 JSON 报告，适合在 CI 流水线或批量处理中自动化验收。

---

## 五、批量转换

### 目录批量转换

```powershell
python scripts/convert_to_wechat.py ./articles ./output --theme green --check --report report.json
```

- `./articles`：包含多个 `.md` 文件的源目录
- `./output`：输出目录，脚本会自动在输出目录中创建对应的子目录结构

### 文件列表批量

在批量模式下，传入多个文件：

```powershell
python scripts/convert_to_wechat.py file1.md file2.md file3.md
```

---

## 六、完整发布流程

### 步骤总览

```
写文章 → 转换 HTML → 预览 → 粘贴 → 发布
  ↓        ↓         ↓       ↓       ↓
 Markdown  命令转换  浏览器   公众号编辑器  正式推送
```

### 详细步骤

**步骤 1：准备文章**

写好 Markdown 源文件 `my_article.md`，包含 front matter 元数据和正文。

**步骤 2：转换**

```powershell
python scripts/convert_to_wechat.py my_article.md wechat_output.html --theme tech --check
```

确认输出为「通过 ✅」，错误数 = 0。

**步骤 3：预览**

```powershell
start wechat_output.html
```

在浏览器中预览效果。按 `Ctrl + A` 全选，`Ctrl + C` 复制。

**步骤 4：粘贴到公众号**

1. 打开 [mp.weixin.qq.com](https://mp.weixin.qq.com) → 登录 → 新建图文
2. 在编辑器中按 `Ctrl + V` 粘贴
3. 上传图片素材（通过公众号素材库，不是粘贴本地图片）
4. 手机端预览 → 确认无误

**步骤 5：发布**

点击「发表」按钮，文章正式对外发布。

---

## 七、常见问题（FAQ）

### Q1：提示 "ModuleNotFoundError: No module named 'markdown'"

**答**：技能会自动安装依赖。如果安装失败，手动运行：

```powershell
python -m pip install markdown beautifulsoup4 python-docx lxml --break-system-packages -q
```

### Q2：转换后中文排版不美观

**答**：技能默认开启中文排版优化（中英文加空格、标点规范化）。如需调整，可编辑 `references/wechat_styles.md` 中的样式参数，或使用 `--no-typography` 关闭优化。

### Q3：图片粘贴到公众号后丢失

**答**：有两种情况：
- 外链图片：技能会保留外链并输出告警，建议通过公众号素材库上传后使用 CDN 链接
- 本地图片：技能默认自动转 base64 内嵌。如需手动处理，可用 `--no-base64` 关闭自动内嵌

### Q4：SVG 图片粘贴后消失

**答**：公众号编辑器保存时会删除 SVG。请使用 PNG 或 JPG 格式的 base64 图片。技能在自检时会对此发出告警。

### Q5：H1 标签在公众号里看不到

**答**：技能会自动将 `<h1>` 降级为 `<h2>`，因为公众号保留 `h1` 作为文章标题（在元数据中）。

### Q6：`--check` 报告里有"风险 CSS"警告，需要处理吗？

**答**：风险 CSS（如 `width`、`display`、`box-shadow`）不是硬性违规，公众号可能静默忽略它们。如果想追求完美，可以修改 `references/wechat_styles.md` 移除这些属性。

### Q7：转换后页面太宽/太窄

**答**：输出 HTML 默认设置 `max-width: 600px`，适配微信公众号的常见显示宽度。如需调整，可修改 `convert_to_wechat.py` 中的 `max-width` 值。

### Q8：如何把 Word 文档也转成公众号？

**答**：把 `.md` 换成 `.docx` 即可：

```powershell
python scripts/convert_to_wechat.py my_report.docx wechat_report.html --theme default
```

---

## 八、命令速查表

| 用途 | 命令 |
|------|------|
| 单文件转换 | `python scripts/convert_to_wechat.py a.md out.html` |
| 单文件 + 主题 | `python scripts/convert_to_wechat.py a.md out.html --theme tech` |
| 单文件 + 自检 | `python scripts/convert_to_wechat.py a.md out.html --check` |
| 只看报告不写文件 | `python scripts/convert_to_wechat.py a.md out.html --dry-run` |
| 批量目录转换 | `python scripts/convert_to_wechat.py ./src ./dst --theme green --check` |
| 导出 JSON 报告 | `python scripts/convert_to_wechat.py a.md out.html --check --report r.json` |
| 查看主题列表 | `python scripts/convert_to_wechat.py --list-themes` |
| 查看帮助 | `python scripts/convert_to_wechat.py --help` |

---

## 九、依赖项一览

| 包名 | 用途 | 来源 |
|------|------|------|
| `markdown` | Markdown 转 HTML | PyPI |
| `beautifulsoup4` | HTML 解析与修改 | PyPI |
| `python-docx` | Word (.docx) 读取 | PyPI |
| `lxml` | XML/HTML 解析后端 | PyPI |

首次运行 `convert_to_wechat.py` 时会自动检测并安装缺失的依赖。

---

## 十、技术支持

如遇到问题，请按以下顺序排查：

1. **运行 `--check`** 获取 dry-run 报告，定位具体错误
2. **查看示例**：`examples/` 目录下有完整示例文件，可直接运行
3. **检查依赖**：`python -m pip list | Select-String "markdown|bs4|docx|lxml"`
4. **联系作者**：本技能由 lenovo 基于 C4B starter kit 定制

---

> 本技能以 `c4b-wechat-publisher-starter` 为基座，在 starter kit 的输入读取、标签清洗、inline CSS 三条内核管线之上，新增了 9 项能力。详见《拿来说明》文档。
*（内容由AI生成，仅供参考）*
