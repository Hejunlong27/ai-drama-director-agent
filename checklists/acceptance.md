# 验收清单（Acceptance Checklist）

> 本文件把工程包的两条铁律 + 各模块自检 + 整链 AC-1..8 落成可勾选的清单。
> ⚠️ 任何 FAIL 项都不得放行；FAIL 指向的回退目标见清单说明。
> 通用三条：语言一致性 ｜ 外观描述跨段逐字一致 ｜ 场景未单独定义即 FAIL。

---

## A. 全局门禁（两条铁律 + 复用锚点）

| # | 检查项 | 判定 | 注 |
|---|--------|------|----|
| A1 | 人物镜头无远景/全景（人物为视觉主体） | 遍历分镜全部镜号：出现则 FAIL | 判据 references/iron_rules.md §1.1 |
| A2 | 交代环境使用空镜远景（人物无/极小<10%） | 环境镜头符合则 PASS | 场口/转场/定场 |
| A3 | 台词 100% 逐字等同 `script.json`（含省略号/破折号/叹号/问号） | 逐字 diff：任一差异 FAIL | 唯一权威 = script.json |
| A4 | 新内容可溯源（能指认剧本原文或前序字段） | 任一新元素无溯源 → FAIL | 铁律② |
| A5 | 无改写/补造/润色台词 | 只允许"语气/表演"补充，词面不变 | FAIL 则回滚 |
| A6 | **单集全局 subject_definitions**：人物、场景都指定 `<Subject N>`；全段共用一套，全局唯一、外观逐字一致 | 出现未定义/重编/外观漂移 → FAIL | 铁律①b |
| A7 | 跨剧集风格沿用：非首集已沿用既有导演风格 | 无沿用声明或自创冲突 → FAIL | M02 步骤0 |

## B. 逐模块自检（按新顺序 M1→M2→M3→M4→M5→M6→M8→M9→M7）

| 模块 | 检查项 | 交付物应含 |
|---|---|---|
| M1 | 剧种/情绪曲线/节奏图谱/触发点，均带证据引用 | `genre / emotionCurve / rhythmProfile / audienceTriggers` |
| M2 | 风格命名+视觉基调+镜头偏好(合法景别)+叙事+表演；含 boomBaseline；**有既有风格则沿用** | `style` + `boomBaseline` + 沿用注记 |
| M3 | 每场 3-8 镜；每镜字段齐全；台词带溯源定位；景别合专断表；空镜远景用于环境；**编列本集全局 subject 清单** | `globalSubjects` + `storyboard.json` |
| M4 | 每镜 SFX/BGM/Set/Lighting，全部有 source 标注；无分镜外新元素 | `soundboard.json` |
| M5 | 转场/节奏/运镜轨迹/视觉母题；台词时序不受转场干涉 | `language.json` |
| M6 | H3 严格六字段 + 官方字面语法；台词逐字；景别合法；屏幕文字脱敏；**人物/场景均以 Subject 指定并引注全局套** | 初版 H3 块 |
| M8 | 逐镜情绪类型+强度+触发溯源；镜头语言优先承担情绪；**按红果/TikTok情绪节奏判达标，不达标退回 M2/M3** | `emotionCurve` + `rework` |
| M9 | ≥3 个风格版本；版本差异在风格/镜头/节奏/音效；**共用单集全局 subject_definitions**；台词逐字一致校验 | `versions[]` + globalSubjectRef |
| M7 | **终审批量质检**：按红果/TikTok判据 P1-P9 逐条，达标才交最终交付；不达标退回 M2/M3/M6 | `质检报告` + `deliverables` |

## C. 整链 AC 映射（dev-grill-docs spec）

| AC | 判据 | 落点 | 判定 |
|----|------|------|------|
| AC-1 | 工程含 M1–M9 九个模块文档，各含输入/输出 schema | `modules/M01..M09` | — |
| AC-2 | 全文显式含人物远景禁令 + 环境空镜远景，且可被检测模块引用 | `iron_rules.md` + M03/M07/M08 | — |
| AC-2b | 全文显式含"全局 subject_definitions + 出现(人物/场景)即指定 Subject" | `iron_rules.md §2b` + M03/M06/M09 | — |
| AC-3 | 测试示例台词与 `EP001-script.json` 逐字一致 | `examples/EP001_跑通示例.md` | — |
| AC-4 | 测试示例 H3 用官方六字段，无自造字段 | `examples/EP001_跑通示例.md` | — |
| AC-5 | ≥3 个导演风格 H3 版本，台词逐字一致 | `M09` + 示例 | — |
| AC-6 | 三场景模板在 `references/h3_official.md`；示例至少体现两处应用 | `references/h3_official.md` + 示例 | — |
| AC-7 | 明确 `script.json` 为台词唯一权威 + 各生成模块含溯源约束 | `iron_rules.md` + M01/M03/M04 | — |
| AC-8 | 质检按红果/TikTok 爆款逻辑；不达标退回导演/分镜 | `M07` + `M08` | — |

## D. 最终放行判据

- A1–A7 全 PASS，B 表每模块 ≥ 该列交付物完整，C 表 AC-1..8 全勾选。
- 若有 FAIL：按"点位"回退（M7 景别/台词/官方 → 相应前置；M7 节奏 → M2/M3；M8 情绪节奏 → M2/M3），复检后再放行。
