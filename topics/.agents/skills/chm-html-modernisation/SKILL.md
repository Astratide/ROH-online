---
name: chm-html-modernisation
description: 《罗德岛行动手册》CHM 项目通用 HTML 改版基准。把旧写法页面（div#content、内联 STYLE、custom-table、h4/h5/h6 标题、p.light 卡片）改成项目现行的“修改版写法”。适用于本手册任意章节的页面，不限于职业页。
---

# CHM 项目 HTML 通用改版基准

## 适用范围

本手册（`Rhodes-Operation-Handbook/`）里所有页面共用 `style.css`。旧页面普遍具有这些特征：
`<div id="content">`、内联 `<STYLE>`、`table.custom-table`、`<h4>/<h5>/<h6>` 当小标题、卡片里 `<p class="light">`、每句一个 `<p>`、`<p class="grey">` 引言、到处用 `<hr>`/`<br>` 分隔。

目标是改成项目现行的**修改版写法**：结构扁平、正文按段用 `<br>` 断句、卡片内只有 `<br>`、表格统一 `class="basic"`、正文里不再出现 h2/h4/h5/h6。

**最高原则：只改标记与样式写法，不改规则文本。** 唯一允许动文字的情况：旧文件中被新结构取代的表格（如子职特性表整表删除）、以及用户确认过的错别字。数值、`<b class="green">` 关键词、`<A href>`、图片路径一律逐字保留。

## 页面骨架（新写法）

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">

<html><head><title>标题</title>
<meta content="text/html; charset=UTF-8" http-equiv="content-type">
<meta http-equiv="X-UA-Compatible" content="IE=11">
<link rel="stylesheet" href="../../style.css">
</head>
<body>
<h1 ...>…</h1>
<ul>
  <p class="sum">“引言”</p>
  <p class="sum" style="text-align: right; margin-right: 100px;">――出处</p>
</ul>
<div class="side-box">
  <p><b>标题：</b>一句话定位。</p>
</div>

<p>正文段落，段内用 <br> 断句。</p>

<h3 class="line">小节标题 Section Title</h3>

<p><b class="maroon">3级：特性名 Name</b><br>
第一行<br>
第二行</p>
<div class="syntax">
<table>…技能图标 + 卡名 + cost/trigger/time…</table>
卡片说明第一段<br>
说明第二段
</div>

<br><br>

</body>
</html>
```

## 通用规则（按处理顺序）

### 1. 编码与换行（每次保存都要保证）

- **UTF-8，BOM 必须在 byte 0**；换行 **CRLF**，不允许出现裸 LF/裸 CR。
- 用编辑工具覆写文件时很容易丢 BOM，保存后务必用字节检查确认 `EF BB BF` 在开头。
- 正文行不要留行尾空格（旧文件里常见 `…。</p> `）。

### 2. `<head>`

- 删除整块内联 `<STYLE>…</STYLE>`（里面的 `table.custom-table` 对齐规则由 `style.css` 接管）。
- `<!DOCTYPE>`、`<title>`、两个 `<meta>`、`<link rel="stylesheet">` 原样保留。
- `"X-UA-Compatible"content=` 可补一个空格（非必须，但同批保持统一）。

### 3. 容器与页面首尾

- 删除 `<body>` 后紧跟的 `<div id="content">`，**以及它配对的 `</div>`**：它可能就在 `</body>` 前，也可能出现在文件中部（收尾标签在卡片之前），要用嵌套深度定位，不能只看文件末尾。
- `<body> ` 后的尾随空格去掉。
- 页尾统一成：最后一段 → 空一行 → `<br><br>` → 空一行 → `</body>` → `</html>`；旧的多余 `<br>`/空行清掉。
- 引言段：`class="grey"` **原样保留**，不要再改成 `class="sum"`（外层 `<ul>` 与内联 `style="text-align: right; margin-right: 100px;"` 都保留）。
- 侧栏 `<div class="side-box">` 内的内容包进 `<p>`。
- 删除引言后的 `<hr class="sub">`。

### 4. 标题层级

- 正文里**只允许 `<h1>`（页面标题，1 个）和 `<h3>`**。
  - 例外：第二章“背景 / 阵营”页的页面标题沿用原样的 **`<h2 class="line">`**，不要改层级（如 `<h2 class="line">信使 Courier</h2>`、`<h2 class="line">罗德岛 Rhodes Island</h2>`）。
- 旧 `<h2>` → `<h3>`；大节标题保留/使用 `<h3 class="line">`（如 `X职业特性 X Traits`、`元素损伤选项`、`施法表` 等节标题）。
- `<h4>`/`<h5>` 当“特性/条目小标题”时：`<p><b class="maroon">标题原文</b><br>` 开头，后面接该条目的正文行，最后 `</p>`。
  - 标题内夹带的 `<br>`、首尾空格、`<h2>…</h4>` 这类**开闭标签不匹配**都要清干净。
- `<h6>` 当“战技卡名/物品卡名”时：`<b style="font-size: 120%;">卡名原文</b><br>`（去掉名称前后空格与内部 `<br>`；`丨`、`&nbsp;` 等分隔符保留）。
- 空的标题（`<h3><br>  </h3>`）直接删除。

### 5. 正文段落

- 旧写法“一句一个 `<p>`”，新写法“**一段一个 `<p>`，段内用 `<br>` 断句**”。
- 一条特性/条目 = **一个 `<p>`**：标题行 + 各行文本，行末 `<br>`，最后一行 `</p>`；条目之间用**空行**分隔（不要再放 `<br>`/`<hr>`）。
- **连续多行 `<p>…</p>`，只要每行开头都有一个 `<b>小标题。</b>` 或 `<b>小标题：</b>` 包裹的小标题，就必须合并成一个 `<p>`**（每行之间 `<br>` 断行，去掉各行原有的 `<p>`/`</p>`，只在最后补一个 `</p>`）。这条同时覆盖"标签：值"式行与"标签。说明"式行：

  ```html
  <!-- 旧 -->
  <p><b>属性值Ability Scores。</b>背景列出了角色的三项属性值。…</p>
  <p><b>专长Feat。</b>背景为你的角色提供一项特定的起源专长。</p>
  <p><b>技能熟练Skill Proficiencies。</b>…</p>

  <!-- 新 -->
  <p><b>属性值Ability Scores。</b>背景列出了角色的三项属性值。…<br>
  <b>专长Feat。</b>背景为你的角色提供一项特定的起源专长。<br>
  <b>技能熟练Skill Proficiencies。</b>…</p>
  ```

  注意：小标题后是普通段落、并非同类小标题行的（如两句独立说明文），不要合并。

  ```html
  <!-- 旧 -->
  <p><b>属性值提升：</b>体质、感知、魅力</p>
  <p><b>起源专长：</b>取决于身份</p>
  <p><b>技能熟练：</b>任选1项技能</p>

  <!-- 新 -->
  <p><b>属性值提升：</b>体质、感知、魅力<br>
  <b>起源专长：</b>取决于身份<br>
  <b>技能熟练：</b>任选1项技能</p>
  ```

- 分节引导句（如 `<p><b>阵营身份：</b>…</p>`）**前面不要放 `<br>` 分隔**，直接紧接上一段/上一组内容。
- 行内 `<b>小标题。</b>`、`<b class="green">`、`<A href>`、`<IMG>` 全部保留。
- **`<ul>` 与 `<p>`**：职业页特性里若含列表，列表前先闭合 `</p>`（`</ul>` 后按需要补 `<br>`）；**但第二章专长条目例外——整个条目（含 `<ul>`）包在同一个 `<p>` 里**，写法见下条。
- **第二章专长条目格式**（`专长` 目录下 `起源/通用/战斗风格` 等文件，用户认定格式）：

  ```html
  <p><b style="font-size: 120%;"; class="maroon">技艺研习 Originium Arts Study</b><br>
  通用专长（先决：等级4+，有施法或契约魔法特性）<br>
  在你不断的学习下，……你获得以下增益：<br>
  <ul>
  <li><b>属性值提升。</b>你的智力、感知或魅力提升1，最多为20。</li>
  <li><b>复选。</b>你可多次选择本专长，但每次必须选择不同的领域。</li>
  </ul>
  </p>
  ```

  - `<h5>名称 Name</h5>`（或 `<h4>`）→ `<p>` + `<b style="font-size: 120%;"; class="maroon">名称 Name</b><br>`；`<h5 id="…">` 的 id 移到该 `<p>` 上。
  - 条目里原本的 `<p class="grey">类别（先决：…）</p>` 与各正文 `<p>` 都变成同一 `<p>` 里的一行（去掉标签、行末 `<br>`）。
  - `<ul>…</ul>` 原样嵌在这个 `<p>` 内部，`</ul>` 之后才写 `</p>`（浏览器会自动处理，与用户样本一致）。
  - 条目之间原有的 `<br>` / 空行分隔保持原样。
- 删除 `<p></p>`、`<!---->` 之类的空壳。

### 6. 卡片：`<div class="syntax">`

**卡片内部只允许 `<br>`，不允许任何 `<p>`。** 具体：

```html
<div class="syntax">
<table>…</table>
<b style="font-size: 120%;">卡名丨English</b><br>
<div class="cost">附赠动作</div>
<div class="trigger">消耗 1 技力</div>
<div class="time">
<IMG class="clock" src="…图标时间.png">
立即</div>
说明第一段<br>
说明第二段
</div>
```

- `cost`/`trigger`/`time` 三个 div：去掉内部 `<p class="light">…</p>`，文字直接写进 div，写成单行；`<div class="cost" id="red">` 这类 **id 要保留**。
- `time` div 内 `<IMG class="clock">` 独占一行，时长文字在下一行。
- 卡片说明原本是若干 `<p>`：去掉标签，段与段用 `<br>` 连接，最后一段后直接 `</div>`。
- 左侧 `<div class="skill"><IMG class="icon" …></div>` 原样保留；一张卡一个 `<div class="syntax">`，多张卡连续排列、中间不加 `<br>`。
- **“条目卡”的卡名还要加 `class="maroon"`**：第二章的背景身份 / 阵营身份这类卡片（`<h4>1. 外勤干员 Field Operator</h4>`）→ `<b class="maroon" style="font-size: 120%;">1. 外勤干员 Field Operator</b><br>`。职业页的战技卡名仍是不带 maroon 的 `<b style="font-size: 120%;">`。
- **投稿/作者卡**（`<b>本章内容来源：</b>玩家投稿` + `<b>作者：</b>…（QQ）`）属于要保留的内容，同样按此规则处理：卡内只有 `<br>`，没有任何 `<p>`。

### 6b. 第三章「种族」页格式（参照 `种族-乌萨斯.html`）

这一类页面（`第三章：种族一览\种族-XX.html`）已由用户定稿一份参考（种族-乌萨斯），要点：

- 去掉 `#content`；`<h1>种族名 English</h1>` 保留；紧接的 `<ul>` + 两行 `<p class="grey">`（引言 + `style="text-align: right; max-width: 600px;"` 出处）**原样保留**。
- **小节标题保持 `<h2 class="line">中文 English</h2>`**（本章节允许 h2）；最末的「种族专长」用 `<h3 class="line">种族专长Race Feats</h3>`。
- 每个小节下的正文：**多段合并成一个 `<p>`**，段间 `<br>`，段间的 `<br>` 分隔行删除，小节之间空一行；小节内若含 `<b>小标题。</b>` 行（如推荐性格特征），按第 5 条规则一并并入同一个 `<p>`。
- **「种族特质」小节例外：去掉 `<p>` 包裹，直接写裸行**，每行以 `<br>` 结尾（如 `<b>类型：</b>你属于类人生物；你的种族是乌萨斯。<br>`），小节末尾 `<br>`。
- 本节的 h4/h5 是留给法术页与怪物页的：**本章其它位置的 `<h4>`/`<h5>`/`<h6>`（含跨行、带缩进、带尾随空标签的写法）一律转成 `<h3>`**，只有「种族专长」小节内的条目标题用上面的 `<b class="maroon" …>` 写法。
- 「种族专长」条目：`<h5>` → `<b class="maroon" style="font-size: 120%;">专长名 English</b><br>`；其先决行 `<p class="grey">…</p>` → `<span class="grey">…</span>`；正文行以 `<br>` 结尾；收益 `<ul>` 紧随其后，条目末尾不加 `</p>`。
- 间距收敛（第三章种族页实测要求）：
  - **`<h2 class="line">` 标题之前不要留 `<br>`**（否则两段之间空隙过大），上一行内容直接接标题。
  - 「种族专长」小节里，"作为XXX，你可以…获得。"那一行之后**不要 `<br>`**，紧接着就是第一个专长标题。
  - "XXX种族专长" 大标题之前**只保留两个 `<br>`**（上一行行尾的 `<br>` + 一个独立的 `<br>` 行）；多出来的 `<br>`/空行删掉。
- 页尾 `<br><br>` 后接 `</body></html>`；`class="grey"` 一律保留。

### 6c. 第四章「冒险装备 / 消耗品 / 工具」条目格式

旧写法（`<h5>物品名</h5>` + 价格行 + 说明段）：

```html
<h5>手表 Wristwatch</h5>
<p class="grey">价格：1 GP | 重量：―</p>
<p>现代世设下的手表……</p>
```

新写法（整条一个 `<p>`，`<p>` 单独占一行，标题行/价格行/说明依次排开）：

```html
<p>
<b class="maroon" style="font-size:120%">手表 Wristwatch</b><br>
<span class="grey">价格：1 GP | 重量：―</span><br>
现代世设下的手表……</p>
```

- 名称：`<h5>` → `<b class="maroon" style="font-size:120%">名称 Name</b><br>`（注意本章写法是 `font-size:120%`，无空格无分号；`class` 在前）。
- 价格/重量行：`<p class="grey">…</p>` → `<span class="grey">…</span><br>`；没有价格行的条目就省略这一行。
- 说明：原来的一条或多条 `<p>` 去掉标签，多段用 `<br>` 连接，最后以 `</p>` 收尾。
- 条目之间原有的 `<br id="锚点">` 行**原样保留**（是跳转锚点）。
- 去 `#content`；页首 `<h1 class="line">` 与列表说明段保留。

### 7. 表格

- `class="custom-table"` → `class="basic"`；`id` 与 `style`（`max-width` 等）按原样保留。
- 子职页的**子职特性表整表删除**（连同它的标题、表后 `<br>`/`<hr>`），因为新写法把特性直接写成 `<p><b class="maroon">`。
- 其它表格（法术表、装置表、事件表、子职表…）**内容一字不改**，只换 class。
- **数据块整体原样保留**：`<table class="stat">`、`<table class="stat-abilities">`、`<div class="monster_container">`、`<div class="stat-block">` 是手册通用的怪物/召唤物数据块写法，内部的 `<h5>/<h6>/<hr>/<p class="grey">` 属于该格式的一部分，**不要改**（全手册有 150+ 处同类页面）。判断方法：卡表也常用 `class="stat"`，但它内部一定有 `<h6>` 或 `class="cost"`，那种按卡片规则处理；真正的数据块没有这些。
- 正文中提及表格的文字（如“X特性表列出了…”）保持原样。

### 8. 废弃标签与统一写法

- `<hr>`（含 `<hr class="sub">`）**全部删除**（数据块内部除外），章节之间改用空行。
- `<strong>` → `<b>`。
- `<font color="maroon">` / `<b><font color="maroon">…</font></b>` → `<b class="maroon">…</b>`；不得残留 `<font>`。
- `class="grey"` 一律 → `class="sum"`（引言）或被并入正文行。
- 不再使用 `class="light"`（卡片里已扁平化）。

### 9. 文字层面的处理

- **只修正明显错误**：叠字（`你你懂得` → `你懂得`）、拼错的英文词（`Superme` → `Supreme`、`Shinning` → `Shining`）、标题里用错职业的英文名（`Barb Traits` 出现在吟游诗人页 → `Bard Traits`）、连续多余空格（`Puppet  Show` → `Puppet Show`）。
- **不要动**：特性名与卡片战技名不一致（正常现象）；图标文件名与卡名不一致但路径有效（视为正确）；不确定的疑似笔误——**列清单交给用户**处理。
- 不改数值、不改规则措辞、不新增原文没有的信息。
- 职业页的简介段落合并（②+①、删③论断引文）是历史上唯一允许的文字重排，仍以参考样本为准；其它页面不要自行合并段落文字。

## 自检清单

改完每份文件后核对（可用 grep/正则计数）：

- [ ] 不出现：`<STYLE>`（用户自己新增的除外）、`id="content"`、`custom-table`、`<hr`、`<font`、`<strong`、`class="light"`、`<h4`、`<h5`、`<h6`（数据块内除外）；`<h2` 只允许出现在第二章背景/阵营类页面的页面标题上。
- [ ] **任何 `<div class="syntax">` 卡片块内部都不得出现 `<p>`**（战技特效卡、身份卡、投稿卡、战技卡一律如此）。
- [ ] `<h1>` 只出现 1 次（页面标题）；正文小标题只用 `<h3>`。
- [ ] 每条特性都是 `<p><b class="maroon">…</b><br>` 开头、`</p>` 结尾（含 `<ul>` 的情况除外）。
- [ ] 每个 `.syntax` 卡片内部 **0 个 `<p>`**（除数据块）。
- [ ] 没有嵌套 `<p>`；`<div>` 与 `</div>` 数量配平。
- [ ] 文本 diff：**只有该删的内容被删，没有任何新增**（用“去标签后的逐行文本”比对）。
- [ ] BOM 在 byte 0；无裸 LF；行尾无空格。
- [ ] 所有 `<b class="green">`、`<A href>`、`<IMG>` 数量与旧文件一致。

## 推荐工作流

1. 备份原件（放到工作目录的 `backup-*/`），先**只产出 `.new` 干跑**，不要直接覆盖。
2. 用脚本批量转换；每条替换都断言“命中次数符合预期”，并加**结构守卫**：战技卡数（`div.syntax`）、图标数、`cost/trigger/time` 数、表格数只允许“特性表 -1”，其它一律不许变。
3. 转换后跑文本 diff 与上面清单；确认无异常再提交（`Move .new → 原名`）。
4. 找不到对应规则的怪异结构（标签错配、残缺容器、畸形卡片）**宁可先修那一处再跑**，不要让通用规则去猜。
5. 遇到疑似笔误但不确定 → 汇总成清单让用户定夺。
