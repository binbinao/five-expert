---
name: using-five-expert
description: Use when /using-five-expert runs. 路由到十技能之一。
version: 1.0.0
author: binbinao
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [five-expert, routing, skills, triage, learning]
    related_skills: [five-expert-panel, five-expert-conflict-map, five-expert-brief, five-expert-peer-review, five-expert-curator, five-expert-levels, five-expert-sprint, five-expert-interview, five-expert-feynman, five-expert-cheatsheet]
---

# Using Five Expert

Activated by the slash command `/using-five-expert`。它是 `five-expert-panel` 技能组（十个技能）的入口：用户只描述要干什么，由你选对那一棒。

这个命令不新增能力，它只防止一件事：**用户提了一个明显属于技能组的请求，你却抡起袖子自己写了一大篇**。

## When to Use

- 用户打了 `/using-five-expert`（主要激活方式）
- 用户说“研究一下 X”“给我评审”“我想学 X”“测测我”“做个速查表”
- 不确定该用哪一棒的时候

激活后仅对**属于技能组职责范围**的请求做路由；闲聊、元任务、配置管理照常处理，不要硬往技能上套。

## 预检

十个技能：`five-expert-panel` / `conflict-map` / `brief` / `peer-review` / `curator` / `levels` / `sprint` / `interview` / `feynman` / `cheatsheet`。

选中某个后，用 `skill_view(name=...)` 加载它并按它的流程执行。如果加载失败，直说缺哪个，不要凭空提造一个“类似”的流程——十个技能的行为差异恰好在于细节纪律（比如一次只问一题、不许评分通胀）。

## 路由表

| 用户想干什么 | 用哪个 | 识别信号 |
|---|---|---|
| 看清一个主题的多面 | `five-expert-panel` | “研究 X”“多角度”“找盲区”“最强反方” |
| 已有五视角，想知道谁对 | `five-expert-conflict-map` | “矛盾图”“谁的证据强”“哪个问题关键” |
| 要一页能用的结论 | `five-expert-brief` | “总结”“简报送老板”“我该做什么” |
| 发布前拆自己一次 | `five-expert-peer-review` | “评审”“打分”“会不会被专家突” |
| 一周找资源、避坑 | `five-expert-curator` | “资源太多”“该看什么”“一周学” |
| 系统学一门东西 | `five-expert-levels` | “一步一步学”“从零到能用”“卡在哪一级” |
| 手头只有两小时 | `five-expert-sprint` | “两小时”“明天就要用” |
| 学完不确定会多少 | `five-expert-interview` | “测测我”“模拟面试”“主动回忆” |
| 好像懂了但说不清 | `five-expert-feynman` | “费曼”“讲给 12 岁”“复述” |
| 上场前 5 分钟 | `five-expert-cheatsheet` | “速查表”“上机前过一遍”“一页纸” |

## 决策顺序（三条问题定棒）

1. **用户手上有没有已有产物？**
   - 只有主题 → 从 `panel` 起
   - 已有五视角输出 → `conflict-map`
   - 已有矛盾图/简报 → `brief` 或 `peer-review`
2. **他要产出还是要验证？**
   - 要产出（分析/简报/计划/材料）→ 前四项 或 `curator`/`levels`/`sprint`/`cheatsheet`
   - 要验证（“我到底会没会”）→ `interview` / `feynman`
3. **他手头有多少时间？**
   - 2 小时 → `sprint`；一周 → `curator`；半年 → `levels`；5 分钟 → `cheatsheet`

三个问题答完还定不下来，就是他还没想清楚想要什么——**直接问一句**，不要跑全套。

## 常用流水线

| 场景 | 链条 |
|---|---|
| 快速摸清一个题 | `panel` → `conflict-map` → `brief` |
| 要对外发布 | 上面 + `peer-review` |
| 从零学一个新东西 | `sprint`（或 `levels`）→ `interview` → `feynman` → `cheatsheet` |
| 长期自学 | `levels` 为主，每级用 `interview` 验收 |
| 写深度文章前 | `panel` → `conflict-map`（keystone 当文章主线）→ `brief`（隱藏关联当立意） |

流水线不是必须全跑。**多数情况下只需要一棒**；把五棒全上只会把上下文灌满、把用户等烦。

## 交互式技能的纪律

`interview` 和 `feynman` 是**一问一答**的对话，不是一次性交付：

- 一次只输出一个提问 / 一段讲解，然后停下来等用户
- 绝对不要一次把十道题、或整轮诊断全部吐完
- 这两个技能被加载后，本轮的输出应当短（≤一屏），否则用户会脑补完答案然后跳步

加载后如果发现自己的输出要写超过两屏，停下来想：是不是把交互式技能当成了一次性报告。

## 反模式

- ❌ 用户说“研究一下 X”，自己不加载技能、直接开写——那就白建了这十个技能
- ❌ 一次加载五个技能堆到上下文里。只加当前那一棒
- ❌ 用户只要一句话结论，却把 `panel` → `conflict-map` → `brief` 全跑一遍
- ❌ 把 `interview`/`feynman` 当成一次性报告输出
- ❌ 技能加载失败时用“类似的方法”蒙混，而不告诉用户缺哪个
- ❌ 把不确定的数据写成确定的事实——十个技能都有“未核实要标注”的红线，入口这一棒不例外

## 激活后的回复形状

定下技能后，一句话说清选了什么、为什么，然后直接开始执行：

```
用 `five-expert-sprint`：你只有两小时且要明天用上。开始——
```

没定下来（用户没给任务或需求模糊）就直接问：

```
你想做哪件：看清一个主题 / 评审已有结论 / 规划学习 / 测自己会多少？
```

不要一次性列出十个技能让用户选。
