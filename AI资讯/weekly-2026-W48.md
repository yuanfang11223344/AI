# AI 每周综述 2026-W48

> 覆盖 2026-11-16 至 2026-11-23（含周一日报）。素材先读取本周日报，并用官方发布、论文与厂商资料复核；“厂商称”不表示独立验证。

## 本周最重要的 5–8 件事

1. 🔥 **GPT-6.2 Astra / Sol 进入 API、ChatGPT Work 与 Codex。** 官方将 Astra 用于复杂专业推理/coding、Sol 用于更快更低成本的多步骤工作；这使模型选择、background completion 与 tool-calling 路由成为 Agent 系统的显式工程决策。重要性在于接口与定价窗口已影响线上架构，而不只是一次模型命名更新。[OpenAI release notes](https://help.openai.com/en/articles/9624314-model-release-notes) · [API changelog](https://developers.openai.com/api/docs/changelog)
2. 🔥 **前沿模型发布把 preparedness controls 一并产品化。** GPT-6.2 system card 说明高风险 cyber 辅助收紧默认限制且更宽松访问需 vetting。重要性在于模型安全评估开始决定具体 API 访问路径，企业的权限设计、日志和人工复核不能后置。[system card](https://deploymentsafety.openai.com/gpt-6-2-astra-sol/internal-deployment-of-gpt-6.2-astra-and-gpt-6.2-sol)
3. 🔥 **Microsoft 在 Ignite 走向“自研模型 + Foundry 分发 + 受控权重”。** 演讲稿称 GPT-6.2 加入 Foundry 并提及 MAI 权重向可信伙伴提供。重要性在于模型采购不再是单一 API 比价，数据面、部署面和权重准入会共同影响企业锁定与合规。[Ignite transcript](https://news.microsoft.com/events/wp-content/uploads/2026/11/Nadella_TRANSCRIPT_Ignite-Keynote_Nov-2026.pdf)
4. ⭐ **Anthropic 的拟议 IPO 将模型公司算力承诺带到公开披露框架。** S-1 及公司公告可帮助观察基础设施承诺和商业风险。重要性在于资本成本会反向限制训练/推理扩张，但估值、融资和模型实测必须分开解读。[Anthropic newsroom](https://www.anthropic.com/news)
5. ⭐ **CPO 的讨论从“能否达到更高速率”转向 200G/lane 封装协同。** Marvell 公开 102.4Tbps、200G/lane SiPho CPO 演示；Lightmatter/Synopsys 则将 224G SerDes、UCIe 和 3D CPO 连接起来。重要性在于交换 ASIC、chiplet 与光引擎的联合验证决定能否量产，而非单项 demo。[Marvell](https://www.marvell.com/company/newsroom/marvell-industry-first-2nm-optical-technology-ai-data-center-infrastructure-ecoc-2026.html) · [Lightmatter](https://lightmatter.co/press-release/lightmatter-collaborates-with-synopsys-to-integrate-advanced-interface-ip-with-its-passage-co-packaged-optics-platform/)
6. ⭐ **开源 Agent 基础设施继续聚焦协议、浏览器与状态流。** agent-browser、AG-UI 与 MCP 分别覆盖浏览器操控、UI 事件和工具协议。重要性在于它们把“工具可调用”扩展成可观察、可授权、可追溯的系统边界。[agent-browser](https://github.com/vercel-labs/agent-browser) · [AG-UI](https://github.com/ag-ui-protocol/ag-ui) · [MCP](https://modelcontextprotocol.io/specification/2025-06-18)

## 🧠 大模型与 LLM 技术解读

### 1. GPT-6.2 的“双层产品化”

- **背景**：Agent 应用同时面对复杂推理的准确性和批量流程的单位成本。
- **问题**：同一模型档位难以兼顾长任务吞吐、交互体验、工具调用和高难度判断。
- **方法**：Astra/Sol 以不同定位接入 Responses/Chat Completions；tool calling 经 Responses，background completion 提供长时运行与相对折扣。
- **效果**：官方称新模型在高难任务上表现更强、输出 token 更少；没有统一第三方真实任务集，不能将厂商 benchmark 外推为所有业务 KPI。[release notes](https://help.openai.com/en/articles/9624314-model-release-notes)

### 2. 安全卡开始影响 Agent 的访问拓扑

- **背景**：模型的 coding、browser 和 computer-use 能力扩张后，风险不只来自输出内容，也来自可执行工具链。
- **问题**：模型在高风险 cyber 场景中可能将知识转化为可操作流程，通用 policy 无法代替分级准入。
- **方法**：官方将模型评估结果映射到更严格的默认限制和额外 vetting；企业仍需在外部实施 tool allowlist、审批和审计。
- **效果**：这是厂商自身部署说明，暂无独立安全认证；但对安全架构的启示明确：把模型选择、用户身份和工具权限一同做策略决策。[system card](https://deploymentsafety.openai.com/gpt-6-2-astra-sol/internal-deployment-of-gpt-6.2-astra-and-gpt-6.2-sol)

### 3. TideCache：Agent serving 的 cache 分层/流式化

- **背景**：hybrid attention/状态空间模型使各层 cache 的大小、重用和可迁移性不同。
- **问题**：长上下文 Agent 的 HBM 压力会推低并发，同时迁移 host/device cache 又损害首 token 延迟。
- **方法**：TideCache 使用透明 cache pool、按层 streaming/prefetch 及请求调度，在不改变后端 cache 接口的前提下分配物理页。
- **效果**：作者在 GB300/GH200 上报告最多 2.04× Agent turns 和 1.40–3.05× P50 TTFT 改善；数字为论文作者结果，需在目标模型、batch 与硬件配置上复现。[论文](https://arxiv.org/abs/2611.26619)

### 4. AutoMaster：将 post-training 从“轨迹收集”推进到“过程行为设计”

- **背景**：成功答案的生成轨迹并不天然是高质量训练数据。
- **问题**：未经筛选的 teacher rollout 会包含重复、无根据假设或表面验证，而单一 success label 难以定位它们。
- **方法**：Skill-FT 用自然语言 skill 约束 rollout；AutoMaster 用 Agent 做轨迹分析、skill 设计、样本选择与反馈迭代。
- **效果**：论文给出其设置内的改进结论；仍应单独核查成本、跨模型泛化、reward hacking 与人工评审开销。[论文](https://arxiv.org/abs/2611.26510)

## 本周必读论文

1. **TideCache: Efficient Hybrid LLM Serving with Transparent Cache Management and Layer-Wise Streaming** — Jinbin Luo 等，2026-11-19。**问题**：hybrid LLM 的异构 cache 让 HBM/延迟权衡恶化。**方法**：透明页池、layer streaming 与调度器。**影响**：为 GB300/GH200 上的 Agent-serving cache 设计给出可检验的系统路径。[arXiv](https://arxiv.org/abs/2611.26619)
2. **AutoMaster: Autonomous Post-Training by Making the Same Model a Better Teacher** — Guoxin Chen 等，2026-11-20。**问题**：成功 teacher trajectory 仍夹杂坏习惯。**方法**：用 Agent 设计自然语言 skills 与训练干预。**影响**：将 post-training 的数据治理和过程质量变为可自动迭代对象。[arXiv](https://arxiv.org/abs/2611.26510)
3. **MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management** — Xuyang Liu 等，2026-11-20。**问题**：长期 Agent 的记录数和取回上下文同时膨胀。**方法**：结构指导的 memory folding。**影响**：将记忆管理从“删/不删”扩展为检索、prefill 与存储的联合优化。[arXiv](https://arxiv.org/abs/2611.26427)

## 🖥️ 芯片与互连专项

1. **200G/lane CPO 是封装—光子—交换芯片的共同项目。** Marvell 的 102.4Tbps CPO 演示把 optics 贴近 accelerator/switch silicon，主要价值是说明铜互连 reach/power 限制正迫使光电边界向 package 内移动；演示不等同量产。设计评审应逐项问清 thermal path、laser/serviceability、BER 与测试覆盖。[Marvell](https://www.marvell.com/company/newsroom/marvell-industry-first-2nm-optical-technology-ai-data-center-infrastructure-ecoc-2026.html)
2. **224G SerDes、UCIe 与 CPO 不可拆开选型。** Lightmatter/Synopsys 的合作将 3nm 224G SerDes/UCIe IP 接入 3D CPO，意味着 die-to-die protocol、package channel、E/O interface 与 bring-up test 必须共用预算。对 ASIC 团队，先定义 reach/功耗/可测性，再选 PHY/光引擎，比从产品名倒推架构更稳妥。[Lightmatter](https://lightmatter.co/press-release/lightmatter-collaborates-with-synopsys-to-integrate-advanced-interface-ip-with-its-passage-co-packaged-optics-platform/)
3. **224G 到 448G 的路线需要标准和系统协同。** OIF 的 CEI-448G 工作提示，下一代 lane rate 尚在接口/互操作成熟过程中。短 reach 可能继续用优化铜互连，中距离/跨机架需评估 LPO/NPO/CPO/coherent-lite；每一段的 PCB/封装损耗、FEC/DSP、维修模型不同。[OIF](https://www.oiforum.com/wp-content/uploads/OIF_CEI-448G_Framework-01.0.pdf)

## 趋势观察

1. **“模型能力”正被拆成可组合的部署要素。** GPT-6.2 的模型档位、长后台窗口、tool-calling 路径和安全准入说明，生产系统比较的对象是“模型 + 调度 + 权限”，不是 leaderboard 单点分数。[OpenAI changelog](https://developers.openai.com/api/docs/changelog)
2. **Agent 成本优化的中心在 memory/cache，而非仅 token 单价。** TideCache 与 MemFolder 分别从 serving cache、长期记忆处理 HBM/上下文增长，提示推理集群设计需联合算 prefill、KV、检索和并发。[TideCache](https://arxiv.org/abs/2611.26619) · [MemFolder](https://arxiv.org/abs/2611.26427)
3. **AI 集群瓶颈正在向互连与封装迁移。** 200G/lane CPO、224G SerDes/UCIe 协同与 448G 标准化并列出现，说明扩展性由电/光边界和制造/运维共同约束，而非只由 accelerator FLOPS 决定。[Marvell](https://www.marvell.com/company/newsroom/marvell-industry-first-2nm-optical-technology-ai-data-center-infrastructure-ecoc-2026.html)

## 下周关注

| 事件 | 日期 | 关注点 | 官方链接 |
|---|---|---|---|
| NeurIPS 2026 | 11 月 24–26 日 | LLM、Agent、系统与硬件相关论文/活动 | [NeurIPS](https://neurips.cc/) |
| SC26 后续发布 | 本周 | 网络、CPO、AEC、HPC/AI 部署证据 | [SC](https://sc26.supercomputing.org/) |
| OpenAI/Anthropic/Microsoft release notes | 持续 | 版本、地区、权限和价格变化 | [OpenAI](https://help.openai.com/en/articles/9624314-model-release-notes) · [Anthropic](https://www.anthropic.com/news) |

## 📱 分享卡片

- GPT-6.2 的工程意义：模型选择、长后台任务、工具调用与安全准入被拆为可配置层。[链接](https://developers.openai.com/api/docs/changelog)
- TideCache 把 hybrid LLM 的 KV cache 变成分层/流式调度问题，作者报出最高 2.04× Agent turns，仍待复现。[链接](https://arxiv.org/abs/2611.26619)
- CPO 的门槛已不止 200G/lane：热、封装良率、测试与维修模型决定能否从 demo 走向部署。[链接](https://www.marvell.com/company/newsroom/marvell-industry-first-2nm-optical-technology-ai-data-center-infrastructure-ecoc-2026.html)
- 连接 MCP、浏览器工具与 UI 协议时，最重要的是把权限和可观察性设计进系统边界。[链接](https://modelcontextprotocol.io/specification/2025-06-18)
