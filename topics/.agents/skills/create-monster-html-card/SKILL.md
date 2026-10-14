---
name: create-monster-html-card
description: "Use when creating or editing Rhodes Operation Handbook monster HTML stat cards, including normal+elite dual blocks, boss cards, CHM-safe structure, relative image paths, and GB2312-compatible templates."
---

<!-- Tip: Use /create-skill in chat to generate content with agent assistance -->

# Skill: 创建泰拉怪物 HTML 数据卡

本 Skill 用于在 Rhodes-Operation-Handbook 项目中创建或整理怪物数据卡 HTML 文件。

项目中的怪物数据卡主要分为两种：

1. 小怪页面：
   - 一个 HTML 文件内通常包含两个怪物数据块。
   - 第一个是较弱版本。
   - 第二个是更强版本。
   - 两个版本通常共用主题、机制、描述风格，但强版本拥有更高 CR、HP、伤害、技能或额外动作。

2. Boss 页面：
   - 一个 HTML 文件通常只包含一个 Boss 数据块。
   - Boss 页面可包含领袖图标、特殊描述、引言、传奇动作、神话特性、神话动作等。
   - Boss 通常使用更复杂的特质、动作、反应、传奇动作结构。

创建前必须优先参考项目中已有文件，例如：
- `深池逐火战士.html`
- `弧光锋卫.html`
- `爱布拉娜.html`

不要创造全新的 HTML 风格，除非用户明确要求。

---

## 一、通用 HTML 文件结构

新建怪物 HTML 文件时，优先使用以下项目风格：

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">

<html xmlns:o="urn:schemas-microsoft-com:office:office">
<head>
<meta content="text/html; charset=gb2312 http-equiv="Content-Type"/>
<meta content="IE=11" http-equiv="X-UA-Compatible">
<title>[页面标题]</title>
<link href="../../style.css" rel="stylesheet"/>
<link href="../../stat-block.css" rel="stylesheet"/>
</meta></head>
<body> 
<div id="content">

[页面内容]

<br><br>
</div>
</body>
</html>
```

注意：

1. 如果 HTML 文件位于 `第七章：生物图鉴/某阵营/` 下，CSS 路径通常是：
   ```html
   <link href="../../style.css" rel="stylesheet"/>
   <link href="../../stat-block.css" rel="stylesheet"/>
   ```

2. 如果 HTML 文件位于 `第七章：生物图鉴/` 直属目录下，CSS 路径通常是：
   ```html
   <link rel="stylesheet" href="../style.css">
   <link rel="stylesheet" href="../stat-block.css">
   ```

3. 不要随意更改项目现有的 HTML 4.01 Transitional 风格。
4. 编码声明通常使用 `gb2312`。
5. CHM 项目中应避免复杂脚本、动态路径或不稳定链接。
6. 图片路径必须使用项目内真实存在的相对路径。
7. 如果图片不确定，使用 `[待匹配图片]`，不要编造路径。

---

## 二、小怪页面结构

小怪页面通常包含两个级别：

1. 普通 / 弱版本
2. 精锐 / 强版本

两个怪物数据块放在同一个 HTML 文件中，中间使用：

```html
<br><hr><br>
```

分隔。

小怪页面整体结构如下：

```html
<div id="content">

[弱版本展示区]
<br>
[弱版本数据卡]

<br><hr><br>

[强版本展示区]
<br>
[强版本数据卡]

<br><br>
</div>
```

---

## 三、小怪展示区模板

每个小怪版本在数据卡前，需要有一个展示区。

模板如下：

```html
<h6>[中文名] [English Name]</h6>
<p class="grey">[攻击距离/伤害类型]；[敌人级别] [种族或标签]</p>

[可选：精英图标]

<table style="height: 10em;">
<tr><td>
<IMG style="width: auto; height: 10em; vertical-align: top;" src="[头像图片路径]" >
</td>
<td>
<p><b>[加粗简介首句]</b>[补充描述]</p>
</td></tr>
</table>
<br>
```

### 字段说明

- `[中文名]`：例如 `深池逐火战士`
- `[English Name]`：例如 `Dublinn Flamechaser Soldier`
- `[攻击距离/伤害类型]`：例如：
  - `近战 物理`
  - `近战 法术`
  - `近战 远程 物理 法术`
- `[敌人级别]`：例如：
  - `普通`
  - `精英`
  - `领袖`
- `[种族或标签]`：例如：
  - `任意`
  - `黎博利`
  - `类人（龙类；德拉克）`
- `[头像图片路径]`：必须来自项目图片资源目录。

### 精英图标

如果该小怪属于精英敌人，可在展示区加入：

```html
<div style="position: relative;">
<IMG style="left: 400px; top: -4em; position: absolute;" src="../../图片资源/图标_精英敌人.png" >
</div>
```

普通敌人通常不需要图标。

---

## 四、小怪数据卡模板

小怪数据卡使用：

```html
<div class="monster_container">
<h6>[中文名]丨[Short English Name]</h6>
<hr>

<p class="grey"><i>[体型][生物类型]，[阵营]</i></p>

<table class="stat">
<tr><td><b>AC</b> [AC]</td>
<td width="40%"><b>先攻</b> [先攻调整]（[先攻值]）</td></tr>
<tr><td colspan="2"><b>HP</b> [HP]（[生命骰]）</td></tr>
<tr><td colspan="2"><b>速度</b> [速度]</td></tr>
</table>

[六属性表]

<table class="stat">
[技能/抗性/免疫/装备/感官/语言/CR 等]
</table>

<br>
<h3>特质 Traits</h3>
<hr>

[特质列表]

<br>
<h3>动作 Actions</h3>
<hr>

[动作列表]

[可选：附赠动作]

[可选：反应]

</div>
```

---

## 五、六属性表模板

项目现有数据卡使用固定的六属性表：

```html
<table class="stat-abilities">
<tr><th></th><th></th>
<th>调整</th><th>豁免</th>
<td style="width: 5px;"></td>
<th></th><th></th>
<th>调整</th><th>豁免</th>
<td style="width: 5px;"></td>
<th></th><th></th>
<th>调整</th><th>豁免</th></tr>
<tr>
<td class="c1"><b>力量</b></td>
<td class="c1">[STR]</td>
<td class="c2">[STR调整]</td>
<td class="c2">[STR豁免]</td>
<td></td>
<td class="c1"><b>敏捷</b></td>
<td class="c1">[DEX]</td>
<td class="c2">[DEX调整]</td>
<td class="c2">[DEX豁免]</td>
<td></td>
<td class="c1"><b>体质</b></td>
<td class="c1">[CON]</td>
<td class="c2">[CON调整]</td>
<td class="c2">[CON豁免]</td>
<td></td>
</tr>
<tr><td class="c3"><b>智力</b></td>
<td class="c3">[INT]</td>
<td class="c4">[INT调整]</td>
<td class="c4">[INT豁免]</td>
<td></td>
<td class="c3"><b>感知</b></td>
<td class="c3">[WIS]</td>
<td class="c4">[WIS调整]</td>
<td class="c4">[WIS豁免]</td>
<td></td>
<td class="c3"><b>魅力</b></td>
<td class="c3">[CHA]</td>
<td class="c4">[CHA调整]</td>
<td class="c4">[CHA豁免]</td>
<td></td>
</tr>
</table>
```

### 属性调整规则

属性调整值应按 D&D 5e 规则计算：

```text
属性调整 = floor((属性值 - 10) / 2)
```

例如：

- 8-9：-1
- 10-11：+0
- 12-13：+1
- 14-15：+2
- 16-17：+3
- 18-19：+4
- 20-21：+5

豁免列默认等于属性调整。

如果该怪物具有某项豁免熟练，则：

```text
豁免 = 属性调整 + PB
```

不要把所有豁免都加熟练。普通小怪通常只有 0-2 项豁免熟练，强怪可略多，但仍应保守。

---

## 六、基础信息表字段顺序

项目中的基础信息表通常采用如下顺序：

```html
<table class="stat">
<tr><td><b>技能</b> [技能列表]</td></tr>
<tr><td><b>抗性</b> [伤害抗性]</td></tr>
<tr><td><b>免疫</b> [伤害免疫或状态免疫]</td></tr>
<tr><td><b>装备</b> [装备]</td></tr>
<tr><td><b>感官</b> [感官，被动察觉]</td></tr>
<tr><td><b>语言</b> [语言]</td></tr>
<tr><td><b>CR</b> [CR]（XP [XP]；PB +[PB]）</td></tr>
</table>
```

不是每个字段都必须出现。

可省略字段包括：

- 技能
- 抗性
- 免疫
- 装备

但以下字段通常应保留：

- 感官
- 语言
- CR

如果字段未知，使用占位符：

```text
[待补充]
[待确认]
[CR 待确认]
```

---

## 七、小怪弱版本与强版本设计规则

### 弱版本

弱版本通常具有：

- 较低 CR。
- 较少动作选项。
- 通常没有附赠动作或反应。
- 通常没有复杂控制。
- DPR 和 HP 应接近目标 CR 推荐值。
- 特质数量一般 1-2 个。
- 动作一般包括 1 个普通攻击，可能没有多重攻击。

示例结构：

```html
<h3>特质 Traits</h3>
<hr>

<p><b>[特质名称 English Name]。</b>[规则描述]</p>

<br>
<h3>动作 Actions</h3>
<hr>

<p><b>[武器或能力名称 English Name]。</b><i>近战攻击检定：</i>+[AB]，触及[距离]尺。<i>命中：</i>[平均伤害]（[伤害骰]）[伤害类型]。</p>
```

### 强版本

强版本通常在弱版本基础上增强：

- CR 提高。
- HP 增加。
- 可能拥有更高属性或技能。
- 通常获得多重攻击。
- 攻击伤害提高。
- 可能获得附赠动作、反应或额外控制。
- 可继承弱版本的核心特质，但应调整描述名称一致性。

示例结构：

```html
<h3>动作 Actions</h3>
<hr>

<p><b>多重攻击 Multiattack。</b>[怪物名]发动两次[攻击名称]攻击。</p>
<p><b>[攻击名称 English Name]。</b><i>近战攻击检定：</i>+[AB]，触及[距离]尺。<i>命中：</i>[平均伤害]（[伤害骰]）[伤害类型]，[附加效果]。</p>

<br>
<h3>附赠动作 Bonus Actions</h3>
<hr>

<p><b>[附赠动作名称 English Name]。</b>[规则描述]</p>
```

强版本不要只是单纯堆高数值。优先通过一个清晰机制表现其“更强”。

---

## 八、Boss 页面结构

Boss 页面通常只包含一个数据卡，但展示区更复杂。

Boss 页面整体结构：

```html
<div id="content">

<h6>[Boss 标题或称号]</h6>
<p class="grey">[攻击距离/伤害类型]；领袖 [种族或标签]</p>

<div style="position: relative;">
<IMG style="left: 400px; top: -4em; position: absolute;" src="../../图片资源/图标_领袖敌人.png" > 
</div>

<table style="height: 10em;">
<tr><td>
<IMG style="width: auto; height: 10em; vertical-align: top;" src="[Boss头像路径]" >
</td>
<td>
<p><b>[简介首句]</b>[描述]</p>
<p><b>[机制或称号]</b>[额外描述]</p>
</td></tr>
</table>

[可选引言]

<br>
<div class="monster_container">
[Boss数据卡]
</div>

<br><br>
</div>
```

### Boss 引言格式

可选引言通常使用：

```html
<ul>
<p class="grey">“[Boss 引言或台词]”</p>
</ul>
<br>
```

如果用户没有提供台词，不要编造。使用：

```text
[待补充引言]
```

---

## 九、Boss 数据卡模板

Boss 数据卡一般使用：

```html
<div class="monster_container">
<h6>[Boss数据卡名称]丨[English Name]</h6>
<hr>

<p class="grey"><i>[体型][生物类型]，[阵营]</i></p>

<table class="stat">
<tr><td><b>AC</b> [AC]（[来源，可选]）</td>
<td width="40%"><b>先攻</b> [先攻调整]（[先攻值]）</td></tr>
<tr><td colspan="2"><b>HP</b> [HP]（[生命骰]）</td></tr>
<tr><td colspan="2"><b>速度</b> [速度]</td></tr>
</table>

[六属性表]

<table class="stat">
<tr><td><b>技能</b> [技能列表]</td></tr>
<tr><td><b>抗性</b> [伤害抗性]</td></tr>
<tr><td><b>免疫</b> [状态免疫或伤害免疫]</td></tr>
<tr><td><b>感官</b> [感官，被动察觉]</td></tr>
<tr><td><b>语言</b> [语言]</td></tr>
<tr><td><b>CR</b> [CR]（XP [XP]，或神话形态 [神话XP]；PB+[PB]）</td></tr>
</table>

<br>
<h3 class="line">特质 Traits</h3>

[Boss特质]

<br>
<h3 class="line">动作 Actions</h3>

[Boss动作]

[可选：附赠动作]

[可选：反应]

[可选：传奇动作]

[可选：神话动作]

</div>
```

Boss 页面中的分节横线优先使用：

```html
```

小怪页面通常使用：

```html
<hr>
```

---

## 十、Boss 常见模块

Boss 可能包含以下模块：

### 1. 特质 Traits

```html
<h3 class="line">特质 Traits</h3>

<p><b>[特质名称 English Name]。</b>[规则描述]</p>
```

Boss 特质可以包含：

- 魔法抗性
- 领袖抗性
- 神话特性
- 特殊光环
- 召唤强化
- 形态转换
- 与核心机制相关的特殊规则

### 2. 动作 Actions

```html
<h3 class="line">动作 Actions</h3>

<p><b>多重攻击 Multiattack。</b>[Boss]发动若干次攻击，或将其中一次攻击替换为[其他动作]。</p>
<p><b>[攻击名称 English Name]。</b><i>近战攻击检定：</i>+[AB]，触及[距离]尺。<i>命中：</i>[平均伤害]（[伤害骰]）[伤害类型]，[附加效果]。</p>
<p><b>[特殊动作 English Name]（[使用次数]）。</b>[规则描述]</p>
```

### 3. 施法 Artcasting

项目中通常使用“施法 Artcasting”而不是标准 “Spellcasting”。

格式示例：

```html
<p><b>施法 Artcasting。</b>[Boss]作为[等级]级施法者施展一道以下法术，使用[属性]作为施法属性（法术豁免 DC [DC]，法术攻击命中+[AB]）。</p>
<p><i>随意：</i><b class="spell">[法术列表]</b></p>
<p><i>每项3/日：</i><b class="spell">[法术列表]</b></p>
<p><i>每项2/日：</i><b class="spell">[法术列表]</b></p>
<p><i>每项1/日：</i><b class="spell">[法术列表]</b></p>
```

### 4. 反应 Reactions

```html
<br>
<h3 class="line">反应 Reactions</h3>

<p><b>[反应名称 English Name]。</b><i>触发：</i>[触发条件]。<i>响应：</i>[效果]。</p>
```

### 5. 传奇动作 Legendary Actions

```html
<br>
<h3 class="line">传奇动作 Legendary Actions</h3>

<p class="grey">传奇动作次数：[次数]。 [Boss]可以在另一生物的回合后立即消耗一次传奇动作来执行以下一道动作。[Boss]在其回合开始时回复所有已消耗的传奇动作次数。</p>
<p><b>[传奇动作名称 English Name]。</b>[效果]</p>
```

一般规则：

- 普通 Boss：传奇动作次数 3。
- 巢穴或高阶 Boss 可写 `3（巢穴内4）`。
- 传奇动作造成伤害时，估算 CR 时默认使用其中两次可造成伤害的传奇动作计入 DPR。

### 6. 神话动作 Mystic Actions

如果 Boss 拥有神话特性，可加入：

```html
<br>
<h3 class="line">神话动作 Mystic Actions</h3>

<p class="grey">如果[Boss]的<b>[神话特性名称]</b>特性被激活，它可以选择以下选项作为传奇动作。</p>
<p><b>[神话动作名称 English Name]。</b>[效果]</p>
```

注意：

- 神话形态是可选遭遇难度。
- 计算纸面 CR 时通常忽略神话特性和神话动作。
- 若神话形态被激活，实际遭遇难度约提高 25%，XP 通常加倍。

---

## 十一、项目内常用 HTML class

创建数据卡时应优先使用以下 class：

```text
monster_container
stat
stat-abilities
grey
para
green
red
spell
syntax
custom-table
```

常见用法：

```html
<b class="green">优势</b>
<b class="green">劣势</b>
<b class="green">燃烧</b>
<b class="green">失能</b>
<b class="green">隐形</b>
<b class="red">深池逐火精锐战士</b>
<b class="spell">法师护甲</b>
```

不要随意新增 CSS class，除非用户明确要求。

---

## 十二、怪物规则文本风格

规则文本应采用项目现有风格。

攻击格式：

```html
<p><b>[攻击名称 English Name]。</b><i>近战攻击检定：</i>+[AB]，触及[距离]尺。<i>命中：</i>[平均伤害]（[伤害骰]）[伤害类型]。</p>
```

远程攻击格式：

```html
<p><b>[攻击名称 English Name]。</b><i>远程攻击检定：</i>+[AB]，射程[距离]尺。<i>命中：</i>[平均伤害]（[伤害骰]）[伤害类型]。</p>
```

豁免格式：

```html
<p><b>[能力名称 English Name]。</b><i>[属性]豁免检定：</i>DC[DC]，[目标范围]。<i>失败：</i>[失败效果]。<i>成功：</i>[成功效果]。</p>
```

触发反应格式：

```html
<p><b>[反应名称 English Name]。</b><i>触发：</i>[触发条件]。<i>响应：</i>[响应效果]。</p>
```

使用次数格式：

```html
<b>[能力名称 English Name]（1次/日）。</b>
<b>[能力名称 English Name]（3次/日）。</b>
<b>[能力名称 English Name]（充能5-6）。</b>
```

---

## 十三、占位符规则

如果信息不足，必须使用占位符，而不是编造。

推荐占位符：

```text
[待补充描述]
[待补充引言]
[待匹配图片]
[待补充来源]
[CR 待确认]
[数值待确认]
[规则效果待确认]
[英文名待确认]
[中文名待确认]
[阵营待确认]
[生物类型待确认]
```

不要为了让页面完整而擅自扩写官方设定、台词、背景故事或复杂能力。

---

## 十四、图片匹配规则

图片路径必须来自项目内实际存在的图片文件。

优先查找：

```text
../../图片资源/敌人头像/[阵营]/
```

常见命名风格：

```text
头像_敌人_[中文名].png
头像_敌人_[中文名弱版本].png
头像_敌人_[中文名强版本].png
头像_敌人_“领袖”.png
```

敌人图标常见路径：

```text
../../图片资源/图标_精英敌人.png
../../图片资源/图标_领袖敌人.png
```

如果有多个候选图片，不要擅自选择，应列出候选让用户确认。

如果没有找到图片，使用：

```html
src="[待匹配图片]"
```

---

## 十五、创建小怪页面时的工作流程

当用户要求创建小怪页面时，按以下流程：

1. 确认页面标题、阵营文件夹、文件名。
2. 确认弱版本和强版本名称。
3. 确认两个版本的 CR。
4. 确认头像图片路径。
5. 查找已有同类文件，优先参考其结构。
6. 创建一个 HTML 文件，包含两个数据块。
7. 弱版本先写，强版本后写。
8. 两个版本之间使用：
   ```html
   <br><hr><br>
   ```
9. 保证两个版本的机制有继承关系。
10. 完成后总结：
    - 新建文件
    - 弱版本 CR
    - 强版本 CR
    - 使用图片
    - 占位符
    - 需要用户确认的数值

---

## 十六、创建 Boss 页面时的工作流程

当用户要求创建 Boss 页面时，按以下流程：

1. 确认 Boss 称号、数据卡名称、英文名。
2. 确认目标 CR。
3. 确认是否为神话 Boss。
4. 确认头像、领袖图标、阵营文件夹。
5. 查找已有 Boss 页面，优先参考 `爱布拉娜.html`。
6. 创建一个 HTML 文件，只包含一个 Boss 数据块。
7. 根据需要加入：
   - 领袖图标
   - 引言
   - 领袖抗性
   - 施法
   - 反应
   - 传奇动作
   - 神话特性
   - 神话动作
8. 计算 CR 时默认忽略神话动作。
9. 如果启用神话形态，在 CR 说明中标记实际遭遇难度提高。
10. 完成后总结：
    - 新建文件
    - Boss CR
    - 是否神话形态
    - 使用图片
    - 传奇动作数量
    - 神话动作情况
    - 待确认字段

---

## 十七、禁止事项

执行本 Skill 时禁止：

1. 不要覆盖已有文件，除非用户明确允许。
2. 不要修改无关文件。
3. 不要编造不存在的图片路径。
4. 不要把不确定内容写成确定事实。
5. 不要大段复制未授权网页内容。
6. 不要创造新的页面结构替代项目现有结构。
7. 不要让低 CR 小怪拥有过多强控制、传奇动作或神话能力。
8. 不要让 Boss 数据只有数值堆叠而没有清晰机制。
9. 不要随意更新索引页，除非用户明确要求。
