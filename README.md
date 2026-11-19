# five-expert

> 用五个专家视角研究任何主题，跑完后把结论压成可执行的学习 / 执行产物。

一套 11 个 [Hermes](https://hermes-agent.nousresearch.com/docs) 技能（10 个五视角研究与学习技能 + 1 个 `/using-five-expert` 路由入口）。核心主张是：**一个主题，五个互不兼容的立场**——目的不是给出一团和气的“综合观点”，而是逼出被单一视角系统性忽略的证据。

五个固定角色：

| # | 视角 | 他独有的盲区 / 洞见 |
|---|---|---|
| 1 | 实践者 | 每天真在干这行的人——知道学者不知道的现场妥协 |
| 2 | 学者 | 同行评审证据的真实强度，以及它在哪些地方和大众认知相反 |
| 3 | 怀疑者 | 最强反方论点，以及支持者刻意不提的证据 |
| 4 | 经济学家 | 谁在这个叙事里获利，利益如何污染结论 |
| 5 | 历史学家 | 结构相似的先例，以及它们最后怎么收场 |

---

## 技能组一览

共 11 个技能。表中「职责」一列直接取自各 `SKILL.md` 的 frontmatter `description`，「什么时候用」取自 `using-five-expert` 路由表的识别信号。

| 技能 | 职责 | 什么时候用 |
|---|---|---|
| `using-five-expert` | `/using-five-expert` 的入口，路由到十个技能之一 | 打了 `/using-five-expert`；或说“研究一下 X”“给我评审”“我想学 X”“测测我”“做个速查表”；不确定该用哪一棒时 |
| `five-expert-panel` | 研究主题时用五专家视角挖盲区与最强反方论点 | “研究 X”“多角度”“找盲区”“最强反方” |
| `five-expert-conflict-map` | 五视角跑完后画矛盾图：冲突、证据强弱、关键问题 | “矛盾图”“谁的证据强”“哪个问题关键” |
| `five-expert-brief` | 把五视角和矛盾图压成一页可执行的调研简报 | “总结”“简报送老板”“我该做什么” |
| `five-expert-peer-review` | 对五视角简报做对抗性同行评审、打分并找漏视视角 | “评审”“打分”“会不会被专家突” |
| `five-expert-curator` | 选五个资源、避坑、排成一周能走完的学习路径 | “资源太多”“该看什么”“一周学” |
| `five-expert-levels` | 把主题拆成五个难度级别，每级带里程碑与自测题 | “一步一步学”“从零到能用”“卡在哪一级” |
| `five-expert-sprint` | 把主题压成 20% 核心 + 10 节课的 2 小时速成计划 | “两小时”“明天就要用” |
| `five-expert-interview` | 一次一问的学习面试：递进出题、逐题打分并指出确切差距 | “测测我”“模拟面试”“主动回忆” |
| `five-expert-feynman` | 费曼法循环：十二岁讲解 → 你复述 → 只补漏洞 → 再讲，直到讲顺 | “费曼”“讲给 12 岁”“复述” |
| `five-expert-cheatsheet` | 做一页 5 分钟过完的速查表：定义、短条目、场景、清单 | “速查表”“上机前过一遍”“一页纸” |

---

## `/using-five-expert` 是入口

`using-five-expert` 是斜杠命令 `/using-five-expert` 的入口技能。它本身不新增能力，只防止一件事：**用户提了一个明显属于技能组的请求，你却不加载技能、直接自己写了一大篇**。

安装后在 Hermes 里这样触发：

```
/using-five-expert 帮我研究一下「远程办公对团队创新产出到底有没有影响」
```

入口会按三条问题做 triage（手上有没有已有产物？要产出还是要验证？手头有多少时间？），选定**一棒**，用 `skill_view(name=...)` 加载它、然后直接开始执行。它不会一次列十个技能让你选，也不建议把五棒全上——多数情况下只需要一棒。

如果你已经明确知道要哪一棒，也可以直接 `skill_view('five-expert-sprint')` 或直接描述任务让 Hermes 自动匹配。

---

## 安装

本仓库的 `skills/` 目录结构与 Hermes 的 skills 目录一一对应。复制到 `~/.hermes/skills/research/` 下即可：

```bash
git clone https://github.com/binbinao/five-expert.git
cd five-expert
cp -R skills/* ~/.hermes/skills/research/
```

重启 Hermes（或新开一个会话）后技能即被加载：

```bash
# 确认 11 个目录都在
ls ~/.hermes/skills/research/ | grep -E 'five-expert|using-five-expert'
```

然后在会话里用 `/using-five-expert` 触发，或直接 `skill_view('five-expert-panel')` 单独加载某一棒。

> 如果你用的是非默认 profile，目标目录是 `~/.hermes/profiles/<name>/skills/research/`。

---

## 典型工作流

流水线不是必须全跑。按技能实际职责，常见的三条链：

### 1. 快速摸清一个题（panel → conflict-map → brief）

| 步骤 | 技能 | 产出 |
|---|---|---|
| 摆全 | `five-expert-panel` | 五视角各答三问 + 冲突地图 + 共识与沉默 |
| 磨出结论 | `five-expert-conflict-map` | 五方底牌、冲突矩阵、直接冲突明细、证据强度排行、Keystone Question、全场共识 |
| 压成可用的 | `five-expert-brief` | 60 秒总结、五个发现（按确信度降序）、隐藏关联、行动建议、前沿问题 |

`brief` 的判据很硬：**读者能不能在读完的下一分钟做出一个不同的决定**。做不到就是写作失败。它的输入契约是五视角 + 矛盾图；只有五视角时，它会先内联做一遍精简版矛盾图（拆伪冲突 → 证据排序 → 挑 keystone），不会跳过证据排序直接写发现。

### 2. 要对外发布（上面 + peer-review）

加一棒 `five-expert-peer-review`：发现打分、最没把握的地方、视角占比、第六视角、模拟学术评审。用在发布前拆自己一次。

### 3. 从零学一个新东西（sprint / levels → interview → feynman → cheatsheet）

| 步骤 | 技能 | 产出 |
|---|---|---|
| 给骨架 | `five-expert-sprint`（2 小时）或 `five-expert-levels`（半年） | 那 20% + 10 次课 / 五级路径与里程碑 |
| 验收 | `five-expert-interview` | 一次一问、0–10 锚定打分、确切差距 |
| 补漏洞 | `five-expert-feynman` | 讲给 12 岁 → 诊断病灶（引原句）→ 只补这几处 → 再讲 |
| 上场前 | `five-expert-cheatsheet` | 一页：核心、三个场景、别踩这些、上场前、快问快答 |

`interview` 和 `feynman` 是**一问一答**的交互技能，不是一次性交付：一轮只输出一个提问或一段讲解，然后停下来等你。

---

## 目录结构

```
five-expert/
├── README.md
├── LICENSE
├── .gitignore
└── skills/
    ├── using-five-expert/SKILL.md
    ├── five-expert-panel/SKILL.md
    ├── five-expert-conflict-map/SKILL.md
    ├── five-expert-brief/SKILL.md
    ├── five-expert-peer-review/SKILL.md
    ├── five-expert-curator/SKILL.md
    ├── five-expert-levels/SKILL.md
    ├── five-expert-sprint/SKILL.md
    ├── five-expert-interview/SKILL.md
    ├── five-expert-feynman/SKILL.md
    └── five-expert-cheatsheet/SKILL.md
```

---

## License

[MIT](LICENSE) © 2026 binbinao
