---
name: class-page-html-migration
description: 将“旧版职业页面 HTML”（custom-table / h4 特性标题 / p.light 战技卡写法）转换为“修改版”写法（table.basic / p+b.maroon 特性 / 精简战技卡）。适用于同一套 style.css 下的职业（先锋、近卫等）页面改版。
---

# 职业页面 HTML：旧写法 → 修改版写法

## 适用场景

用户给出一个按**旧写法**编写的职业页面（特征：`<div id="content">`、内联 `<STYLE>`、`table.custom-table`、`<h4>N级：…</h4>`、战技卡内 `<p class="light">`），要求改成**修改版写法**（特征：无容器 div、`table.basic`、`<p><b class="maroon">N级：…</b><br>…</p>`、战技卡裸文本）。

参考样本：`先锋.html → 先锋_修改版.html`、`近卫.html → 近卫_修改版.html`。

## 总原则

1. **只改结构与样式写法，不改规则文本。** 除第 6 步明确允许的简介段合并外，所有正文文字、数值、`<b class="green">` 关键字、`<A href>` 链接、图片路径必须逐字保留。
2. 不要“顺手修正”规则内容（参考样本里先锋的技力/兼职文字改动是内容修订，不属于本 skill；参考样本里的 `Trailulazer` 是拼写错误，不要复制）。
3. 输出整份完整 HTML，保持 UTF-8，保留原 `<!DOCTYPE>`、`<title>`、`<meta>`、`<link rel="stylesheet" href="../style.css">`。
4. 处理顺序按下文 1→11 步逐段进行，最后用检查清单核对。

---

## 转换步骤

### 1. `<head>`：删除内联样式

- **删除**整个 `<STYLE>…</STYLE>` 块（里面全是 `table.custom-table … nth-child` 对齐规则，新写法由 `style.css` 的 `table.basic` 接管）。
- 其余 head 内容原样保留。`<meta http-equiv="X-UA-Compatible"content="IE=11">` 可顺手补空格为 `… "X-UA-Compatible" content="IE=11"`，非必须。

```html
<!-- 旧 -->
<link rel="stylesheet" href="../style.css">
<STYLE>
table.custom-table td:nth-child(4) {text-align: left;}
…
</STYLE>
</head>

<!-- 新 -->
<link rel="stylesheet" href="../style.css">
</head>
```

### 2. `<body>`：去掉 `#content` 容器

- 删除 `<body>` 后紧跟的 `<div id="content">`，以及 `</body>` 前对应的 `</div>`。
- `<body> ` 后的尾随空格去掉。页面末尾保留 `<br><br>` 再接 `</body>`。

### 3. 标题与引言

- `<h1>` 行（含 `<IMG class="text-img">`）原样保留。
- 引言段：`<p class="grey">` → `<p class="sum">`（两行都改，内联 `style="text-align: right; margin-right: 100px;"` 保留，外层 `<ul>` 保留）。
- **删除**引言后面的 `<hr class="sub">`。

```html
<!-- 旧 -->
<ul>
  <p class="grey">“……”</p>
  <p class="grey" style="text-align: right; margin-right: 100px;">――某某</p>
</ul>
<hr class="sub">

<!-- 新 -->
<ul>
  <p class="sum">“……”</p>
  <p class="sum" style="text-align: right; margin-right: 100px;">――某某</p>
</ul>
```

### 4. side-box：内容包进 `<p>`

```html
<!-- 旧 -->
<div class="side-box">
  <b>近卫：</b>你擅长在战斗中对地面敌人造成伤害。
</div>

<!-- 新 -->
<div class="side-box">
  <p><b>近卫：</b>你擅长在战斗中对地面敌人造成伤害。</p>
</div>
```

side-box 后**紧接**核心特质（中间空一行即可），不再放简介段落——简介移到第 6 步。

### 5. 核心特质：`<p>` 列表 → `table.basic#render`

旧写法：

```html
<h2>先锋核心特质Core Vanguard Traits</h2>        <!-- 或 <h3> -->
<p>作为一名先锋，你具有以下职业核心特质：</p>
<p><b><i>主属性Primary Ability。</i></b>力量或敏捷</p>
<p><b><i>生命值骰Hit Point Die。</i></b>每先锋等级D8</p>
<p><b><i>技能熟练Skill Proficiencies。</i></b>选择3项：特技、隐匿、调查……。</p>
…
<br>
```

新写法：

```html
<b>先锋核心特质Core Vanguard Traits</b>
<table class="basic" id="render">
<tr><td width="300"><b>主要属性Primary Ability</b><td>力量或敏捷</td></tr>
<tr><td width="300"><b>生命值骰Hit Point Die</b><td>每先锋等级D8</td></tr>
<tr><td width="300"><b>豁免熟练Saving Throw Proficiencies</b><td>敏捷与感知</td></tr>
<tr><td width="300"><b>技能熟练Skill Proficiencies</b><td>选择3项：<b class="green">特技</b>、<b class="green">隐匿</b>、<b class="green">调查</b>……。</td></tr>
<tr><td width="300"><b>武器熟练Weapon Proficiencies</b><td>简易武器、军用近战武器</td></tr>
<tr><td width="300"><b>工具熟练Tool Proficiencies</b><td>盗贼工具</td></tr>
<tr><td width="300"><b>护甲受训Armor Training</b><td>轻甲、中甲</td></tr>
<tr><td width="300"><b>起始装备Starting Equipment</b><td>你获得150GP，可以用来购买初始装备或作为起始财产。</td></tr>
</table>
```

规则：
- 标题从 `<h2>/<h3>` 变为**裸 `<b>`**，文字不变。
- 删除引导句“作为一名X，你具有以下职业核心特质：”。
- 每条 `<p><b><i>标签。</i></b>值</p>` → 一行 `<tr><td width="300"><b>标签</b><td>值</td></tr>`：
  - 去掉 `<i>`，去掉标签末尾的“。”。
  - “主属性” 改为 “主要属性”（仅此一处改字）。
  - 值前后的多余空格去掉（如 ` 盗贼工具`、` 你获得150GP`）。
  - 参考样本中第一个 `<td>` 没有闭合标签，浏览器会自动闭合；按样本原样写即可，写成 `</td><td>` 也可接受。
- **技能熟练**一行：把每个技能名分别包进 `<b class="green">…</b>`，顿号与句末“。”留在外面。
- 行的有无/顺序跟原文件走（如近卫没有“工具熟练”行就不要加）。
- 删除旧写法段落末尾的 `<br>`。

### 6. 简介段落：合并、移位（唯一允许动文字的步骤）

旧写法在 side-box 之后有 3 个 `<p>`：①职业概述、②干员风格描述、③`“面对魂灵昭示的诸多谜题，……”——论断：X`。

新写法：**只保留 1 个 `<p>`，放在核心特质表之后、`成为一名X` 之前**。

- 以②为主干，把①的内容接在后面，合成一段。
- ③“论断”引文整行删除；参考样本把引文意象（如“身先士卒突入其中”“挥舞武器将其剖开”）轻微改写嵌入②的句子中。若无把握，保守做法是：直接 `② + ①` 拼接，删除③，不自行改写。
- 不要新增任何原文没有的信息。

### 7. “成为一名X”：`<p>` → `<ul><li><div>`

```html
<!-- 旧 -->
<h3>成为一名先锋Becoming a Vanguard</h3>
<p><b>作为1级角色As a Level 1 Character</b></p>
<p>获得<b><font color="maroon">先锋核心特质表</font></b>里的全部能力。</p>
<p>获得先锋的1级特性（见先锋特性表 ）</p>
<p><b>作为兼职角色As a Multiclass Character</b> </p>
<p>获得<font color="maroon"><b>先锋核心特质表</b></font>里的以下能力：</p>
<p>生命骰、军用近战武器熟练和轻甲、中甲的护甲受训。</p>
<p>获得先锋的1级特性（见先锋特性表 ）。</p>
<br>

<!-- 新 -->
<h3>成为一名先锋Becoming a Vanguard</h3>
<p><b class="maroon">作为1级角色As a Level 1 Character</b></p>
<ul>
<li><div>获得<b class="maroon">先锋核心特质表</b>里的全部能力。</div></li>
<li><div>获得先锋的1级特性（见先锋特性表）</div></li>
</ul>
<p><b class="maroon">作为兼职角色As a Multiclass Character</b></p>
<ul>
<li><div>获得<b class="maroon">先锋核心特质表</b>里的以下能力：生命骰、军用近战武器熟练和轻甲、中甲的护甲受训。</div></li>
<li><div>获得先锋的1级特性（见先锋特性表）。</div></li>
</ul>
<br>
```

规则：
- `<h3>` 保留。
- 两个小标题 `<b>` → `<b class="maroon">`，去掉 `</b>` 后的尾随空格。
- 其后的每个 `<p>` → `<li><div>…</div></li>`，分别包在两个 `<ul>` 中。
- 兼职部分的“获得…以下能力：”和“生命骰、…”两句**合并为一个 `<li>`**。
- `<font color="maroon">` / `<b><font color="maroon">…</font></b>` / `<font color="maroon"><b>…</b></font>` 一律改为 `<b class="maroon">…</b>`。不得保留 `<font>` 标签。
- “（见X特性表 ）”里的多余空格去掉 → “（见X特性表）”。
- 第二个 `</ul>` 后保留一个 `<br>` 再接特性表标题。

### 8. 特性表

```html
<!-- 旧 -->
<h4>先锋特性表</h4>                                   <!-- 近卫是 <h3> -->
<table class="custom-table" style="max-width: 520px;">
  <colgroup>
  <col style="width: 40px;">
  …
  <tr>
    <td>1st</td>
    …
    <td>2nd</td>
    …

<!-- 新 -->
<h3>先锋特性表 Vanguard Traits</h3>
<table class="basic" id="render" style="max-width: 520px;">
  <colgroup>
  <col style="width: 50px;">
  …
  <tr>
    <td>1</td>
    …
    <td>2</td>
    …
```

规则：
- 标题统一 `<h3>`，并在中文后补上英文 `X Traits`（与 `<h1>` 里的英文职业名一致，如 `Vanguard Traits`、`Guard Traits`）。
- `class="custom-table"` → `class="basic"`。
- 两个参考样本的 id 不一致：先锋用 `id="left"`（且去掉了 max-width），近卫用 `id="render"`（保留 max-width）。**默认采用近卫写法** `class="basic" id="render" style="max-width: 520px;"`；若项目中已有其他改好的页面，以它们为准，同一批文件保持一致。
- 第一列 `<col style="width: 40px;">` → `50px`，其他列宽不变。
- 等级单元格 `1st / 2nd / 3rd / 4th … 20th` → 纯数字 `1 / 2 / 3 / 4 … 20`。
- 表头、其余行内容、`<tbody>` 结构不变。表后保留 `<br>`。

### 9. 子职表：去掉 class 和 id

```html
<!-- 旧 -->
<table class="custom-table" id="left">

<!-- 新 -->
<table>
```

其余（colgroup、`<th>`、`colSpan="2"` 行、每个子职行）原样保留。表格结束后不要再放 `<br>`/`<hr>`，直接空两行接下一节。

### 10. 职业特性大标题

```html
<!-- 旧 -->
<br>
<h1>先锋职业特性 Vanguard Traits</h1>
<hr class="sub">
<br>

<!-- 新 -->
<h3 class="line">先锋职业特性 Vanguard Traits</h3>
```

### 11. 每条职业特性：`h4 + 多个 p + br` → 单个 `p`

```html
<!-- 旧 -->
<h4>1级：开路先锋 Trailblazer</h4>
<p>你懂得如何应对陌生的战场环境，为队友扫除障碍。你获得下述增益：</p>
<p><b>技能专精。</b>选择一项你熟练但不具备专精的技能，你获得其<b class="green">专精</b>。</p>
<p><b>工具熟练。</b>选择一项你不具备熟练的工具，你获得其熟练。</p>
<br>

<!-- 新 -->
<p><b class="maroon">1级：开路先锋 Trailblazer</b><br>
你懂得如何应对陌生的战场环境，为队友扫除障碍。你获得下述增益：<br>
<b>技能专精。</b>选择一项你熟练但不具备专精的技能，你获得其<b class="green">专精</b>。<br>
<b>工具熟练。</b>选择一项你不具备熟练的工具，你获得其熟练。</p>
```

规则：
- `<h4>N级：名称 English</h4>` → `<p><b class="maroon">N级：名称 English</b><br>`。标题文字逐字保留；删除 `<h4>` 内部夹带的 `<br>`/空格（如近卫 13 级 `究明攻击 Studied Attacks<br>  `）。
- 该特性下的每个 `<p>` 变成一行文本，行与行之间用 `<br>` 连接（`<br>` 放在上一行末尾）。最后一行以 `</p>` 收尾。
- 删除特性之间的 `<br>` 分隔；改为特性之间**空一行**。
- 去掉行末尾随空格（旧文件里不少 `…。 </p>`）。
- 行内 `<b>小标题。</b>`、`<b class="green">`、`<A href>` 全部保留。`<b> 技力。</b>` 这类标签内多余空格可去掉。

#### 11a. 特性内含 `<ul>` 列表时

`<ul>` 不能放在 `<p>` 内部。参考样本的写法是在 `<ul>` 前不写 `</p>`（靠浏览器自动闭合），`</ul>` 后跟一个 `<br>`：

```html
<p><b class="maroon">2级：部署策略 Deploy Strategy</b><br>
作为先锋，……<br>
<b>部署策略。</b>你最初习得下述部署策略：<br>
<ul>
<li><b>攻击：</b>……</li>
<li><b>加速：</b>……</li>
</ul>
<br>
```

推荐写法（渲染一致且 HTML 合法）：在 `<ul>` 前补 `</p>`，其余相同。`<li>` 内容原样保留。

#### 11b. 特性内含战技卡（`<div class="syntax">`）时

先按 11 把战技卡之前的说明文字写成一个 `<p>…</p>`（以 `</p>` 闭合），然后紧接战技卡。战技卡本身按下面改写：

```html
<!-- 旧 -->
<div class="syntax">
<table>
  <tr>
    <td>
      <div class="skill">
      <IMG class="icon" src="../图片资源/技能图标/先锋/技能_冲锋号令.png" >
      </div>
    </td>
    <td>
      <h6> 冲锋号令丨Order of Charge</h6>
      <div class="cost">
        <p class="light">附赠动作</p></div>
      <div class="trigger">
        <p class="light">消耗 1 技力</p></div>
      <div class="time">
        <p class="light">
          <IMG class="clock" src="../图片资源/技能图标/图标时间.png">
          立即</p></div>
    </td>
  </tr>
</table>
<p>你可以使用附赠动作并消耗1点技力来发动这个战技。……</p>
<p><b>部署策略：</b>此战技每消耗1点技力，……</p>
</div>

<!-- 新 -->
<div class="syntax">
<table>
  <tr>
    <td>
      <div class="skill">
      <IMG class="icon" src="../图片资源/技能图标/先锋/技能_冲锋号令.png" >
      </div>
    </td>
    <td>
      <b style="font-size: 120%;">冲锋号令丨Order of Charge</b><br>
      <div class="cost">附赠动作</div>
      <div class="trigger">消耗 1 技力</div>
      <div class="time">
      <IMG class="clock" src="../图片资源/技能图标/图标时间.png">
      立即</div>
    </td>
  </tr>
</table>
你可以使用附赠动作并消耗1点技力来发动这个战技。……<br>
<b>部署策略：</b>此战技每消耗1点技力，……
</div>
```

规则：
- `<h6>名称丨English</h6>` → `<b style="font-size: 120%;">名称丨English</b><br>`；去掉名称前的空格。分隔符“丨”保留。
- `cost / trigger / time` 三个 div：去掉内部的 `<p class="light">…</p>`，文字直接放在 div 里，写成单行。`<div class="cost" id="red">` 这类 id 要保留。
- `time` div：`<IMG class="clock" …>` 和时长文字直接放在 div 内。
- 表格之后的描述段落：去掉 `<p>`，文字直接作为 `.syntax` div 的内容；多段之间用 `<br>` 连接；最后一段后直接 `</div>`。
- 左侧图标 `<div class="skill"><IMG class="icon" …></div>` 原样保留。
- 一个特性下有多个战技卡（如近卫 2 级三张卡）时，每张卡各自一个 `<div class="syntax">`，连续排列，中间不加 `<br>`。
- 最后一张战技卡 `</div>` 后空一行，再接下一条特性的 `<p><b class="maroon">…`。

### 12. 收尾

```html
<p><b class="maroon">20级：陷阵旌旗 Unto The Breach</b><br>
……</p>
<br><br>

</body>
</html>
```

去掉旧的 `</div>`（对应 `#content`）。

---

## 页面骨架模板（新写法）

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">

<html><head><title>职业名</title>
<meta content="text/html; charset=UTF-8" http-equiv="content-type">
<meta http-equiv="X-UA-Compatible" content="IE=11">
<link rel="stylesheet" href="../style.css">
</head>
<body>

<h1>职业名 English<IMG class="text-img" alt ="职业名标志" src="../图片资源/职业图标/大图/icon_xxx.png" ></h1>
<ul>
  <p class="sum">“引言”</p>
  <p class="sum" style="text-align: right; margin-right: 100px;">――出处</p>
</ul>
<div class="side-box">
  <p><b>职业名：</b>一句话定位。</p>
</div>

<b>职业名核心特质Core Xxx Traits</b>
<table class="basic" id="render">
<tr><td width="300"><b>主要属性Primary Ability</b><td>…</td></tr>
…
</table>

<p>合并后的简介段落。</p>

<h3>成为一名职业名Becoming a Xxx</h3>
<p><b class="maroon">作为1级角色As a Level 1 Character</b></p>
<ul>
<li><div>…</div></li>
<li><div>…</div></li>
</ul>
<p><b class="maroon">作为兼职角色As a Multiclass Character</b></p>
<ul>
<li><div>…</div></li>
<li><div>…</div></li>
</ul>
<h3>职业名特性表 Xxx Traits</h3>
<table class="basic" id="render" style="max-width: 520px;">
  …等级列为纯数字…
</tbody></table>
<br>
<table>
  …子职表…
</tbody></table>


<h3 class="line">职业名职业特性 Xxx Traits</h3>

<p><b class="maroon">1级：特性名 English</b><br>
正文行 1<br>
正文行 2</p>

<p><b class="maroon">2级：战技 Battleskill</b><br>
…说明…</p>
<div class="syntax">
<table>…</table>
描述<br>
描述
</div>

<p><b class="maroon">20级：特性名 English</b><br>
正文</p>
<br><br>

</body>
</html>
```

---

## 完成后检查清单

- [ ] 文件中不再出现：`<STYLE>`、`id="content"`、`class="grey"`、`class="custom-table"`、`<hr class="sub">`、`<font`、`<h4>`、`<h6>`、`<h2>`、`class="light"`、`<h1>` 中的“职业特性”标题。
- [ ] `<h1>` 仅出现 1 次（页面标题）。`<h3>` 出现 3 次：成为一名X / X特性表 / `class="line"` 的职业特性。
- [ ] 核心特质表每行格式为 `<tr><td width="300"><b>…</b><td>…</td></tr>`，技能名全部带 `<b class="green">`。
- [ ] 特性表等级列无 `st/nd/rd/th` 后缀；首列宽 50px。
- [ ] 每条特性都是 `<p><b class="maroon">N级：…</b><br>` 开头、`</p>` 结尾（含 `<ul>` 的特殊情况除外）。
- [ ] 每张战技卡：`<b style="font-size: 120%;">` 标题、三个 div 单行、描述无 `<p>`。
- [ ] 旧文件中所有 `<b class="green">`、`<A href>`、`<IMG>` 数量与新文件一致（可用 grep 计数核对）。
- [ ] 规则文本逐字一致（除第 6 步简介合并）。
- [ ] 保存为 UTF-8，无 BOM 要求但不要改变原编码。

## 可用正则（辅助，不替代人工核对）

| 目的 | 查找 | 替换 |
|---|---|---|
| 引言 class | `<p class="grey"` | `<p class="sum"` |
| 特性标题 | `<h4>(.+?)</h4>\s*<p>` | `<p><b class="maroon">$1</b><br>\n` |
| 战技名 | `<h6>\s*(.+?)</h6>` | `<b style="font-size: 120%;">$1</b><br>` |
| cost/trigger | `<div class="(cost|trigger)"( id="\w+")?>\s*<p class="light">(.+?)</p></div>` | `<div class="$1"$2>$3</div>` |
| 等级序数 | `<td>(\d+)(st|nd|rd|th)</td>` | `<td>$1</td>` |
| maroon | `<b><font color="maroon">(.+?)</font></b>` 及 `<font color="maroon"><b>(.+?)</b></font>` | `<b class="maroon">$1</b>` |

特性正文中的 `</p>\s*<p>` → `<br>\n` 只能在单条特性范围内做，不要全局替换（会误伤其他段落）。
