# H3 官方提示词框架 · 参考模板（三应用场景）

> 本文件是「短剧AI导演Agent」的 H3 输出唯一格式基准。一切 H3 结构以本文件为准，禁止自造字段。
> 画面锚点取自现场已跑通的真实合并稿 `年报之前_分镜/EP001_多参H3/EP001_H3_Ref2VA_合并稿.md`，是官方框架在该工程的落地实例。
>
> ⚠️ 标注：字段骨架与字面语法以官方 H3 提示词框架为准；本文件以现场实例为画面匹配的字母锚点。若官方文档最终口径与本文有出入，**以官方文档最终口径为准**（见 ADR / spec Open questions）。

---

## 0. 六字段骨架（所有应用场景共用）

每个 H3 提示词块由六个字段组成，顺序固定、缺一不可：

```
subject_definitions:      # 主体定义：人物/场景的外观与身份锁定
summary:                  # 段落概述：时长、画幅、整段叙事落点
retention_analysis:       # 一致性保持分析：角色/场景的持续保持声明
detailed_description:     # 分镜级细腻描述：[Shot k] 逐镜 + 台词 + 切点时间
overall_soundscape:       # 整体音效氛围（环境音/动作音）
non_diegetic_music:       # 非叙事音乐（BGM；本示例基调下多为 N/A）
```

## 1. 字面语法（字母锚点，取自真实合并稿）

| 语法元素 | 写法 | 实例 |
|---|---|---|
| 应用场景标识 | `summary` 首词 `[reference generation]` / `[first-and-last-frame]` / `[text-to-video]` | `[reference generation] 9 秒、9:16 深夜文戏定场片段：…` |
| 画幅与时长的信息载体 | 在 `summary` 首句 | `9 秒、9:16 …片段` |
| 分镜切换 | `detailed_description` 内 `[Shot k]`，第一镜不写时间，后续镜写 `At 00:00.000` | `[Shot 1] <Subject 1> … 固定镜头…`；`[Shot 2] At 00:02.000, 切她面部近景…` |
| 台词 | `<d>[Chinese] 逐字台词。</d>`，嵌套在该镜描述内、动作/神情之后 | `压低声音低语，<d>[Chinese] 等一下……这个数对不上。</d>` |
| 人物引用 | `<Subject N>`（全局说话人 ID，跨段一致） | `<Subject 1> (S1)` |
| 参考图 | `<Picture N>`（多参时才用，标记人物/场景外观来源） | `<Picture 1> 定义人物 <Subject 1>（林晚星）外观来源。` |
| 一致性声明 | `retention_analysis` 内 `<Subject N> (appears in [Shot k]): fully_preserved - 描述` | `<Subject 1> (appears in [Shot 1]–[Shot 4]): fully_preserved - 齐下巴中分短发…` |
| 屏幕文字脱敏 | 所有可见屏幕/单据文字一律呈现为不可辨认图案（正向写进 retention） | `屏幕上表格保持为不可辨认的模糊图案，不出现任何可读文字。` |
| 风格打底 | `detailed_description` 首行英文风格句 | `Realistic, moderately warm short-drama style, vertical 9:16, night-lit indoor office.` |

---

## 2. 统一版式（块级模板）

```text
## {段名} · {内容一句话} · {时长}s

subject_definitions:
<Subject N> is <Picture N> 中的<外形描述>；锁定脸形五官、发型与服装主色。<声线描述>
<Picture N> 定义人物 <Subject N>（<角色名>）外观来源。
<Picture M> 定义场景：<环境描述>。

summary:
[{应用场景标识}] {秒数} 秒、9:16 {温度/质感}片段：{段落叙事一句话 + 结尾落点}。场景由 <Picture M> 锁定。

retention_analysis:
<Subject N> (appears in [Shot k1], [Shot k2]): fully_preserved - <保持要点>。
<Picture M> (贯穿某镜): fully_preserved - <布局与光线方向不变>。
<屏幕脱敏声明按需>。

detailed_description:
<风格打底句>
[Shot 1] <景别>；<画面/动作/神态>。固定镜头/运镜，<光线>。
[Shot 2] At 00:00.000, 切<景别>；<画面/动作/神态>，<动作><d>[Chinese] 台词。</d> <运镜补充>。

overall_soundscape:
<环境底噪>、<动作音>、<人声处理>。

non_diegetic_music:
N/A（或三段式：情绪建立→推向落点→收声）
```

---

## 3. 三应用场景差异对照

| 维度 | 多参（Ref2VA）图生视频 | 首尾帧图生视频 | 文生视频（T2V） |
|---|---|---|---|
| summary 标识 | `[reference generation]` | `[first-and-last-frame generation]` | `[text-to-video]` |
| 参考图来源 | 多张 `<Picture N>` 锁定人物/场景外观（subject_definitions 逐参定义） | 首帧 + 尾帧两张 `<Picture first/last>`（或对应首尾帧编号）锁定起点与落点关键形态；人物主体外观以首帧为准，尾帧锁"落点姿态/景别不动" | 无参考图，纯文字；subject_definitions 用纯文字把人物/场景外观写死（含服装、发型、脸型五官、场景布局光线） |
| subject_definitions | 每个 `<Subject>` 都要有 `<Picture N> 定义…外观来源` 行 | 开头列出首/尾帧参考图定义首尾锁点；人物 `<Picture first>` 锁定 | 全部纯文字锁定（"20代年轻女会计师：齐下巴黑色短发…藏青西裤"），无 Picture 引用行 |
| retention_analysis | 侧重"人物与场景全程 fully_preserved" | 侧重"从首帧起点到尾帧落点的一致性迁移"，写清楚中间变化 | 侧重"纯文字定义的外观/服装/场景布局全程一致" |
| 复现细节 | 多点参考图允许"跨段沿用全局 subject_definitions" | 首尾帧强调"话语生视频的时间连续性" | 强依赖文字密度、台词 `<d>` 与镜头切点完整 |
| 使用场景 | 需要跨段复用已定妆容/资产人物（本项目主用） | 需要严格锁定某一段的起止形态（如"进入/离场"、"开合"强锁定） | 无现成参考图、快速概念验证 |

### 3.1 多参注意

- 全局一致约束可写在每段开头（说话人全局 ID、角色参考图排列、服装昼夜换装锚点）。
- 跨段沿用同一 subject_definitions → 每段 `《<Picture N> 定义人物 <Subject N>》` 行仍须保留，保证参考图声明不丢。

### 3.2 首尾帧注意

- 首帧、尾帧各自用一条参考图声明行；`retention_analysis` 需声明"从首帧到尾帧的核心外观不变"。
- 若段内有状态变化（如文件被合上、人物由蹲到站），在 `[Shot k]` 的 `At` 切点处写清变化，并被 retention 承接。

### 3.3 文生注意

- 没有任何 `<Picture>` 可复用 → subject_definitions 的每个 `<Subject>` 用纯文字把外观写满（脸型/发型/服装主色/配饰/声线）。
- 场景也用纯文字（环境、布光、色温）。

---

## 4. 入场声明（每段 H3 之前的元信息，非六字段的一部分）

（供段落组织，不作为 H3 字段输出）

```text
> 依据：<script.json 集号> + <参考图来源说明>
> 模式：Ref2VA / 首尾帧 / 文生；竖屏 9:16。
> 台词铁律：以下台词全部逐字抄录自 script.json，一字不改。
> 全局一致约束：说话人 ID / 参考图排列 / 服装换装锚点 / 屏幕文字脱敏。
```

---

## 5. 常用细节句

### 景别合法写法（铁律①约束）
- 近景/特写/大特写/中景/中近景/过肩 → 人物可用。
- 空镜远景 → 仅用于环境定场/转场（人物不出现）。

### 运镜写法
- 固定镜头、缓慢推近、缓慢右摇、跟拍、从精细到全局拉出……
- 运镜必须服务于情绪/信息，不与台词抢时序。

### 光线写法
- 深夜：冷白荧光灯混桌台灯暖光；屏幕光上面光。
- 清晨：百叶窗晨光铺地条纹。
- 午后：暖光透百叶窗。
- 白天资料室：整面落地窗日光直射。

### 音效写法
- 环境底噪（空调/打印机）+ 关键动作音（鼠标/荧光笔/文件夹搭扣/档案落地）+ 人声气口处理。
- 非叙事音乐大部分场景 N/A（写实文戏）；爽点/泪点可转暖/收敛。
