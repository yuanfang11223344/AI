# AI 每周综述 2026-W25 (6/9 - 6/16)

> 生成时间：2026-06-16 | 基于本周搜索 + 日报汇总

---

## 本周最重要的 5-8 件事

### 1. G7 峰会：Altman、Amodei、Hassabis 历史性同台 🔥

Sam Altman (OpenAI)、Dario Amodei (Anthropic)、Demis Hassabis (Google DeepMind) 首次共同出席 G7 峰会（法国埃维昂，6/15-17）。议程涵盖 AI 治理、网络安全、青少年保护。马克龙亲自邀请 Altman，法国在 AI 地缘政治中的枢纽角色逐渐成型。

但峰会前的黑天鹅事件：Anthropic 被曝 Claude Fable 5 内置隐藏限制（检测"前沿 AI 研究"用途时自动降级），遭强烈批评后逆转。同时美国商务部 6/12 对其最新模型实施出口管制。

- [TNW: G7 峰会](https://thenextweb.com/news/g7-ai-summit-altman-amodei-hassabis)
- [Yahoo Tech: Anthropic 信任危机](https://tech.yahoo.com/ai/claude/articles/amicus-ill-pass-anthropic-policy-033022037.html)
- [东方财富: 三巨头 G7](https://fund.eastmoney.com/a/202606123770091471.html)

**为什么重要**：全球 AI 治理从行业自律走向地缘政治博弈。Anthropic 的信任危机可能影响其 IPO 估值和公众信任。三大 CEO 同时在 G7 亮相是 AI 产业政治成熟的标志。

### 2. NVIDIA 开源 5500 亿参数 Nemotron 3 Ultra 🔥

NVIDIA 发布 Nemotron 3 Ultra，550B MoE（Apache 2.0）。推理快 5x、成本低 30%。CrowdStrike 安全 agent、Palantir 自主运营已部署。OpenClaw / OpenHands / OpenCode 已适配。

- [IT之家](https://m.ithome.com/html/958090.htm)

**为什么重要**：NVIDIA 首次在 500B+ 参数级别与 OpenAI/Anthropic 直接竞争，Apache 2.0 开源将加速 Agent 生态。这是"主权 AI"运动的标志性事件。

### 3. Credo 首发 224G 三协议 AI Retimer 🔥

Credo Blue Heron：业界首个同时支持 UALink、ESUN、Ethernet on 224G 的 AI Scale-Up Retimer。3nm，30-Tap FFE+16 反射消除。Q3 量产。Scale-Up 网络市场 2030 年预期 >$400 亿。

- [Nasdaq](https://www.nasdaq.com/press-release/credo-introduces-industrys-first-224g-multiprotocol-ai-scale-retimer-supporting)

**为什么重要**：224G 物理层从"各家自有协议"走向"统一 Layer 1"。Credo 在 AI 互连基础设施的核心位置被确立。三协议兼容意味着芯片厂商不需要为 UALink/ESUN/Ethernet 分别设计物理层。

### 4. Cohere North Mini Code 开源：单 H100 可跑的 Agentic Coding Model

30B/3B MoE，128 专家，210 tokens/sec，Apache 2.0。单 H100 或 Mac Studio 即可运行。被定位为闭源模型的"主权 AI"替代方案。

- [Cohere 博客](https://cohere.com/blog/north-mini-code)
- [VentureBeat](https://venturebeat.com/technology/cohere-open-sources-a-coding-agent-that-runs-on-a-single-h100)

**为什么重要**："小模型也能做 Agent"的论据又多了一个重量级标杆。Cohere 的策略明确对标 Anthropic/OpenAI 的闭源路线，价格优势+本地运行可能改变企业 AI 部署格局。

### 5. Lightmatter + Synopsys：224G SerDes 与 CPO 的历史性对接

Synopsys 硅验证 224G SerDes + UCIe IP (3nm) 集成到 Lightmatter Passage 3D CPO 平台。电气→光学域桥接在 224G 速度下实现。AI-EDA 工具链用于光电联合设计。

- [Lightmatter 官方](https://lightmatter.co/press-release/lightmatter-collaborates-with-synopsys-to-integrate-advanced-interface-ip-with-its-passage-co-packaged-optics-platform/)

**为什么重要**：这是 CPO 从"可以做"到"怎么做"的关键节点。224G SerDes IP + UCIe + CPO 的三位一体方案正在成形。意味着未来 2-3 年内 CPO 将从小规模验证走向量产部署。

### 6. COMPUTEX 2026：黄仁勋定调"光学是唯一缩放路径"

"You scale up further with Optics, and you scale out with Optics, and you scale across with Optics." — 黄仁勋在 COMPUTEX 的主题演讲直接把光学定为 AI 缩放的核心路径。PCI-SIG 成立光学工作组，PCIe 7.0/8.0 将支持光学 PCIe。

### 7. 论文：推理能力越强，幻觉反而越多？

本周多篇论文 (2506.23840, 2506.12353, 2506.11274) 验证了同一个反直觉结论：thinking tokens 提高推理能力的同时也增加了工具调用幻觉率。另有 paper 证明了幻觉控制在数学上不可能（stat.ML）。

- [2506.23840: Do Thinking Tokens Help or Trap?](https://arxiv.org/abs/2506.23840)
- [2506.11425: Agent-RLVR](https://arxiv.org/abs/2506.11425)

**为什么重要**：这件事直接影响所有做大模型推理的公司的产品路线图。如果 thinking tokens 工具调用需要额外 safety layer，成本和延迟都会增加。

### 8. Anthropic + OpenAI 双双启动 IPO

Anthropic 估值 ~$9650 亿 ($65B 融资)；OpenAI 估值 >$1 万亿（高盛/摩根士丹利承销）。两者选择在 AI 监管最密集的时间窗口上市——G7 峰会提供了"负责任 AI"的背书。

---

## 本周必读论文（3 篇）

### 1. Agent-to-Agent Theory of Mind (2506.22957)

**问题**：LLM agent 能否理解其他 agent 的知识状态？

**发现**：当前最先进的 LLM 在 Agent-to-Agent 情境中的心智理论能力远低于人类基准。agent 倾向于过度自信地假设其他 agent 共享自己的知识。

**影响**：多 agent 系统设计需要显式的知识状态交换机制，不能依赖模型的隐含心智理论。

- [arXiv 2506.22957](https://arxiv.org/abs/2506.22957)

### 2. On the Fundamental Impossibility of Hallucination Control (stat.ML, 2026)

**问题**：能不能从根本上消除 LLM 的幻觉？

**结论**：不能。论文从数学上证明了任何足够强大的语言模型都存在不可消除的幻觉边界。

**影响**：改变了模型部署策略——与其追求零幻觉，不如在系统层面增加验证层。这篇 paper 可能成为今年引用最多的 AI 论文之一。

### 3. Agent-RLVR: Training Software Engineering Agents with RL (2506.11425)

**问题**：能不能用强化学习的环境奖励（而非人工标注）训练 coding agent？

**方法**：直接用编译/测试结果作为奖励信号，训练 agent 的代码编辑能力。

**影响**：RL+环境奖励可能是下一波 coding agent 训练的核心范式，降低了对高质量标注数据集的依赖。ACL 2025。

- [arXiv 2506.11425](https://arxiv.org/abs/2506.11425)

---

## 芯片与互连专项

### 224G SerDes 进入量产倒计时

| 厂商 | 产品 | 工艺 | 状态 |
|---|---|---|---|
| Credo | Blue Heron 224G Retimer | 3nm | 送样，Q3 2026 量产 |
| Broadcom | Tomahawk 6 (200G SerDes) | 3nm | 已发货 |
| Synopsys | 224G PAM4 SerDes IP | 3nm | 硅验证，可授权 |
| Cadence | 224G PAM4 SerDes PHY | 7nm-2nm+ | 硅验证，>40dB CEI-224G-LR |
| MediaTek | 224G SerDes (TPU v8e) | — | 流片验证 |

### CPO 三大里程碑本周达成

1. Lightmatter + Synopsys：224G SerDes IP 集成到 3D CPO 平台（首个量产级 CPO IP 方案）
2. COMPUTEX 黄仁勋："光学是 AI 缩放唯一路径"
3. PCI-SIG 成立光学工作组 → PCIe 7.0/8.0 支持光学

### 本周新增关注信号

- IEEE 802.3dj → 100G PCS lanes, OSFP-1600 标准化
- 1.6T Ethernet = 8×224G（已成为行业共识基线）
- CPO 测试从单通道→256 通道系统级验证

---

## 趋势观察

### 1. "小模型也能 Agent" 运动加速

本周 Cohere (30B)、JetBrains (12B)、Google (12B) 同时发布小参数量的 Agentic 模型。Apache 2.0 是默认协议。企业 AI 部署从"依赖一个超大闭源模型"转向"多个开源小模型各司其职"的趋势已经不可逆。

### 2. AI 产业的信任和透明度危机已到临界点

Anthropic Fable 5 隐藏限制事件 + 出口管制 + OpenAI 员工撤回法庭之友签名 + Oracle 用省下的工资买 GPU——本周出现了多起独立但同指向的事件：AI 产业的可信度正在被内外同时侵蚀。G7 峰会的"负责任 AI"口号和实际行动之间的差距正在拉大。

### 3. 224G PAM4 从论文变成产品

过去一年我们讨论 224G 时还是在说 ISSCC 论文和 Cadence IP 演示。这一周 Credo 宣布 Q3 量产、Lightmatter+Synopsys CPO 方案成形、NVIDIA 在 COMPUTEX 定调光学路径——224G 已经从"技术成熟度"问题变成"市场节奏"问题。

---

## 下周关注

| 事件 | 时间 | 关注点 |
|---|---|---|
| ICML 2026 继续 | 6/12-22 | ClinHallu, Persona-Pruner 等论文 |
| G7 峰会后续 | 6/17 闭幕 | 是否有 AI 治理联合声明 |
| Anthropic 出口管制谈判 | 本周 | 能否成功解除或放宽限制 |
| Credo TSMC Symposium | 持续 | 224G Retimer 客户反馈 |
| Nex-N2 开源生态 | 本周 | HuggingFace 社区采用情况 |

---

## 📱 分享卡片

> 🔥 **2026-W25 AI 每周综述**
>
> 1. G7 峰会：Altman + Amodei + Hassabis 首次同台，Anthropic 同时深陷信任危机 + 出口管制 + IPO 三线作战 |
> [TNW](https://thenextweb.com/news/g7-ai-summit-altman-amodei-hassabis)
>
> 2. NVIDIA 开源 5500 亿 Nemotron 3 Ultra (Apache 2.0)，推理快 5x |
> [IT之家](https://m.ithome.com/html/958090.htm)
>
> 3. 224G SerDes 量产倒计时：Credo Blue Heron Q3 量产，Lightmatter+Synopsys CPO 方案成形，黄仁勋定调"光学是唯一缩放路径" |
> [Nasdaq](https://www.nasdaq.com/press-release/credo-introduces-industrys-first-224g-multiprotocol-ai-scale-retimer-supporting)
>
> 4. "小模型 Agent"运动：Cohere/JetBrains/Google 同时发布 <30B 参数的 Agentic 开源模型 |
> 5. 论文证明"幻觉控制在数学上不可能"，同时验证"推理越强幻觉越多" |
> [arXiv 2506.23840](https://arxiv.org/abs/2506.23840)

---

> 📌 **本周一句话**：「G7 三巨头同台 + 224G SerDes 量产倒计时 + Anthropic 信任危机三重奏 + 小模型 Agent 运动进入快车道。」

> 本杂志基于 skills/调研/ 目录的 AI 资讯 skill 体系生成。本周报读取了 1 份日报 + 并行搜索 6 个信源 + 芯片专项搜索。
