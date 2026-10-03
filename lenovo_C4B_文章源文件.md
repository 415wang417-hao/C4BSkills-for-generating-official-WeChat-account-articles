---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: 60ea5d0126731e33de81577b27285889_bd08dc20bf3911f1887c525400de85a5
    ReservedCode1: P5n68/Zz8Q8yctECEqvTbC56n91yKm/HyqPtbrgC+MT9tRDZutey1bu5+ldJPMTNBbEaymMhcRgOpIJykmwu6iKHlEmrfvzofVcwCUzO514j2XVs7uGupQ4Vk1WmGjTriXbcg633TFCvQmuJlmonB9iz9VeWW8C2o22Qdlg3CAX+QgM1qXMnyV6Wa4k=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: 60ea5d0126731e33de81577b27285889_bd08dc20bf3911f1887c525400de85a5
    ReservedCode2: P5n68/Zz8Q8yctECEqvTbC56n91yKm/HyqPtbrgC+MT9tRDZutey1bu5+ldJPMTNBbEaymMhcRgOpIJykmwu6iKHlEmrfvzofVcwCUzO514j2XVs7uGupQ4Vk1WmGjTriXbcg633TFCvQmuJlmonB9iz9VeWW8C2o22Qdlg3CAX+QgM1qXMnyV6Wa4k=
---



# 我把 Markdown 一键变公众号：从 starter kit 到内容发布流水线的完整复盘

> [!NOTE] 阅读收益
> 读完你会拿到一套可以直接复用的方法：不重写工具，而是在 starter kit 上做"增量改造"，把写文章到发布之间的 6 个手工步骤压缩成 1 条命令。

## 痛点：写作只花了 30%，排版吃掉了 70%

我每周都要在公众号发一篇技术复盘，但真正让我拖延的从来不是"写"，而是"排"。Markdown 里写得干干净净，粘进公众号编辑器就原形毕露：标题层级全乱、`<h1>` 被吞、代码块变成一坨灰、图片外链失效。最崩溃的一次，一篇文章我在编辑器里手工调了 40 分钟，最后发现**微信公众号只认 inline CSS**，我之前写的所有 class 全部作废。

更隐蔽的坑是：公众号会**静默删除违规标签**。你粘贴时看着好好的，点保存后 `<style>`、`<div>`、`<iframe>` 悄悄消失，排版瞬间崩掉——而你在手机上预览时才发现。

## 方案：不重造轮子，做"增量改造"

starter kit 已经给了我最难的部分：Markdown/Word → HTML 的读取管线、违规标签清洗、基础 inline CSS。它就像一台能跑的发动机，只是没有内饰。所以我的策略不是从零写一个转换器，而是**在 starter kit 上做增量改造**——保留它的读取与清洗内核，在"样式层、结构层、质检层"三个方向做加法。

这条路线的好处是**风险可控**：内核是被验证过的，我每加一个能力，都能立刻用同一份样例文章回归测试，不会出现"改一处崩三处"。

## 技能设计与新增能力

我把定制版命名为 `wechat-publisher-pro`，在 starter 的 5 项基础能力之上，新增了 9 项能力，全部用一条命令驱动：

| 新增能力 | 触发方式 | 解决的痛点 |
|---|---|---|
| 多主题切换 | `--theme default/tech/green/minimal` | 不同文章要有不同气质 |
| Callout 高亮框 | `> [!NOTE]` 类标记 | 知识/实践/注意/提示一眼区分 |
| 自动目录 | 按标题自动生成 | 长文可跳读 |
| 文章元数据 | front matter 或 CLI | 标题/作者/日期/摘要齐备 |
| 页脚版权声明 | `--footer` 或 front matter | 发布合规 |
| 中文排版优化 | 默认开启 | 中英文间距与标点更专业 |
| 本地图片 base64 | 自动识别本地路径 | 图片不再丢失 |
| 批量转换 | 传入目录即可 | 一次处理多篇 |
| 发布前自检 | `--check` | 发布前拦截违规标签 |

> [!PRACTICE] 主题是怎么实现的
> 4 套主题（`default` 静谧蓝、`tech` 极客紫、`green` 清新绿、`minimal` 极简黑白）共用一个 `THEMES` 字典，每个主题只定义 primary/heading/body/code 等 12 个色值，其余样式由 `build_styles()` 统一派生。想加第五套主题，只需往里加一个字典，零改动逻辑代码。

## 真实使用教程

整个流程只需要三步，真正的"一条命令可跑"：

```bash
# 第一步：安装依赖（首次运行会自动安装）
pip install markdown beautifulsoup4 python-docx

# 第二步：转换文章，指定主题与自检
python scripts/convert_to_wechat.py lenovo_C4B_文章源文件.md lenovo_C4B_output.html --theme tech --check

# 第三步：批量转换整个目录
python scripts/convert_to_wechat.py ./articles/ ./out/ --theme green
```

转换完成后，打开 `output.html`，`Ctrl+A` 全选、`Ctrl+C` 复制，直接粘贴进公众号编辑器即可。因为所有样式都是 inline CSS，粘贴后不会掉格式。

> [!TIP] 用 callout 提升可读性
> 在 Markdown 里写 `> [!WARNING] 注意 xxx`，转换后会自动变成带图标和底色的高亮框。四类标记分别是：`[!NOTE]` 📘知识、`[!PRACTICE]` 🔧实践、`[!WARNING]` ⚠️注意、`[!TIP]` 💡提示，同时兼容中文别名。

## 踩坑与失败经验

### 坑一：blockquote 被"吃掉"了

我最初的 callout 实现是在 markdown 转换**之后**去正则匹配 `<blockquote>`。结果发现：因为开启了 `nl2br` 扩展，`[!NOTE]` 后面会插入 `<br>`，导致标记和正文粘连，正则匹配到一半就崩。**修复方案**：把 callout 抽取提前到 markdown 转换**之前**，用占位符 token 替换，转换后再回填。这个改动让 callout 识别率从"经常失败"变成"100% 稳定"。

### 坑二：中文标点被"误伤"

第一版中文排版优化我写得太激进：把所有的英文逗号都转成中文逗号。结果代码块里的 `for i, j in ...` 全被改成了 `for i，j`，语法直接错了。**修复方案**：排版优化只作用于文本节点，并且**跳过 `<code>`/`<pre>` 内部**；标点转换也加了前置条件——只有当逗号前是汉字时才转换。这个教训让我记住：**任何"全量替换"都是危险的**。

### 坑三：自检报告一开始全是误报

最初我的 `--check` 把 `<html>`、`<body>` 也判成违规标签，报告一片飘红，根本没法用。后来想明白了：**自检应该只检"会被粘贴进公众号的那部分内容"**，也就是 body 内部的正文章节，预览用的外壳（html/head/body）是给浏览器看的，不该纳入检查范围。

## 效果对比

| 指标 | 手工排版 | 定制技能 |
|---|---|---|
| 单篇排版耗时 | 约 30-40 分钟 | 小于 10 秒 |
| 格式保持率 | 经常出现标题/代码丢失 | inline CSS，粘贴即还原 |
| 发布前风险 | 保存后才发现标签被删 | `--check` 提前拦截 |
| 风格一致性 | 每次都不一样 | 主题固定，篇篇统一 |
| 多篇处理 | 一篇篇手工来 | 一条命令批量 |

## 反思与复用建议

**第一，改造要选对"接缝"。** starter kit 的读取和清洗内核我没有动，因为那是它最成熟的部分；我在样式层和质检层做加法，所以每一步都稳。**在别人的基础上做增量，比从零重写更接近专业开发者的工作方式。**

**第二，AI 负责产出，人负责"下判断"。** 4 套主题的色值、callout 的语义分类、自检的规则边界，这些都不是 AI 能替我拍板的——AI 给了我 10 个能跑的版本，但选择哪一个、边界划在哪里，是我的判断。

**第三，能力要能被别人直接用。** 我特意把主题、callout、目录都做成了"配置驱动"而非"写死"，这样群里同学拿到这个技能，不改一行代码就能生成自己风格的公众号文章。

> [!WARNING] 一个诚实的提醒
> 这个技能解决的是"排版自动化"，不是"内容生产"。它能让你的文章更专业，但写什么、观点是什么，仍然只能靠你自己。工具放大的是你的思考，不是替代它。

如果你也在被排版折磨，建议立刻做一件事：找到你重复做过三次以上的手工步骤，问自己"能不能变成一条命令"。然后，去改造一个现成的 starter kit——**先跑通，再优化，比追求完美更重要。**
*（内容由AI生成，仅供参考）*
