# AI 每周综述 2026-W28

> 生成时间：2026-07-06 Asia/Shanghai；周一按规则与当日日报同时生成。

## 本周最重要的 5-8 件事
1. 🔥 **Sonnet 5 进入生产验证**。要点：以 Sonnet 成本承接更强 Agent。**为什么重要**：企业竞争指标从单轮能力转向任务成功成本，但早期反馈分化。[Axios](https://www.axios.com/2026/06/30/anthropic-sonnet-5-agents-mythos-fable)
2. 🔥 **Fable 5 恢复访问**。要点：高能力模型重新覆盖 Claude 多入口。**为什么重要**：短期恢复不消除政策供给风险，多 provider fallback 已是工程要求。[来源](https://www.kubiosec.tech/blog/2026-07-03-AgenticUpdates)
3. 🔥 **ICML 2026 开幕**。要点：训练、推理、agents 和 evaluation 成为本周研究焦点。**为什么重要**：新方法需要从预印本进入同行讨论和复现阶段。[ICML](https://icml.cc/)
4. 🔥 **Jalapeño 推动 model-system-silicon co-design**。**为什么重要**：frontier lab 的成本优势将越来越来自 chip、rack 和 fabric 协同。[OpenAI](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)
5. ⭐ **CPO 进入 AI 平台交付**。**为什么重要**：NVIDIA 的 200Gb/s SerDes CPO 与 Marvell 448G 路线共同显示 electrical reach 正逼近功耗边界。[NVIDIA](https://nvidianews.nvidia.com/news/vera-rubin-full-production-agentic-ai-factory)
6. ⭐ **MCP RC 推进无状态核心**。**为什么重要**：Agent 的扩缩容、恢复、授权和长任务治理开始获得协议层支撑。[MCP](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)

## 大模型与 LLM 技术解读
### Sonnet 5
- **背景**：高频 Agent 对成本和延迟敏感。**问题**：token 单价不能代表任务总成本。**方法**：以 Sonnet 档承接 browser、planning、coding。**效果**：官方称接近 Opus 4.8，独立评价仍分化。[Axios](https://www.axios.com/2026/06/30/anthropic-sonnet-5-agents-mythos-fable)
### Inference-scaled evaluation
- **背景**：模型越来越依赖测试时计算。**问题**：固定预算扭曲排名。**方法**：调整 token budget、compaction 和重复尝试。**效果**：论文显示评分显著依赖 protocol。[arXiv](https://arxiv.org/abs/2606.17930)
### RiVER
- **背景**：RLVR 难覆盖无唯一解任务。**问题**：缺少二值 correctness reward。**方法**：以执行分数构造候选排序。**效果**：作者报告优化收益，仍需独立复现。[arXiv](https://arxiv.org/abs/2606.27369)
### MCP runtime
- **背景**：Agent server 进入多租户生产。**问题**：session 状态妨碍弹性与恢复。**方法**：stateless core 加 Tasks/Apps 扩展。**效果**：架构更清晰，兼容性待验证。[MCP](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)

## 本周必读论文（3 篇）
1. **How Inference Compute Shapes Frontier LLM Evaluation**：问题是固定预算误判能力；方法是跨任务调整推理预算；影响是 benchmark 必须披露 compute protocol。[arXiv:2606.17930](https://arxiv.org/abs/2606.17930)
2. **Reinforcement Learning without Ground-Truth Solutions can Improve LLMs**：以 ranking reward 扩展 RLVR 到连续优化任务。[arXiv:2606.27369](https://arxiv.org/abs/2606.27369)
3. **When are likely answers right?**：系统分析 likelihood 与 correctness 的条件关系，影响 best-of-N 和 verifier 设计。[arXiv:2606.27359](https://arxiv.org/abs/2606.27359)

## 芯片与互连专项
### 1. 224G 到 448G 不只是 baud rate 升级
更高 SerDes 速率会增加 equalization、retimer 和 channel loss 压力，迫使 optical engine 更靠近 switch/accelerator package；Marvell 同时布局 CPC/NPO/CPO 正反映这种分层。[Marvell](https://www.marvell.com/blogs/scale-up-network-solutions-for-ai-infrastructure.html)
### 2. CPO 的平台优势与维护代价并存
NVIDIA 控制 GPU、switch、network software 和 system，可吸收 CPO 的协同复杂度；开放生态则仍需解决 laser、fiber attach、热与现场维护。[NVIDIA](https://nvidianews.nvidia.com/news/vera-rubin-full-production-agentic-ai-factory)｜[Siemens](https://blogs.sw.siemens.com/semiconductor-packaging/2026/02/05/five-key-trends-of-co-packaged-optics-cpo-in-2026/)
### 3. Custom inference silicon 正重塑供应链
Jalapeño 的价值不是单颗 ASIC，而是模型、kernel、HBM、rack 和 network 的共同定义；对传统 merchant silicon 的压力来自系统优化能力。[OpenAI](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)

## 趋势观察
1. **模型评价从 token price 转向 task economics**：Sonnet 5 的官方定位与社区争议说明成功率、轨迹长度和重试成本必须一起测。[Axios](https://www.axios.com/2026/06/30/anthropic-sonnet-5-agents-mythos-fable)
2. **模型供给成为架构变量**：Fable 5 的限制与恢复证明 provider availability 会被政策改变。[来源](https://www.kubiosec.tech/blog/2026-07-03-AgenticUpdates)
3. **互连瓶颈推动 optical I/O 前移**：200G CPO、224G→448G SerDes 和 OCI chiplet 分别从系统、lane 与 package 层逼近同一问题。[Intel](https://www.intel.com/content/www/us/en/products/details/network-io/silicon-photonics.html)

## 下周关注
| 事件 | 日期 | 关注点 | 官方链接 |
|---|---|---|---|
| ICML 2026 | 07-06 至 07-11 | LLM、Agent、evaluation、AI systems | [ICML](https://icml.cc/) |
| MCP 新规范迁移 | 持续 | stateless server、Tasks、authorization | [MCP RC](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC) |

## 📱 分享卡片
1. Sonnet 5 把竞争焦点拉到“完成一次 Agent 任务的总成本”。[Axios](https://www.axios.com/2026/06/30/anthropic-sonnet-5-agents-mythos-fable)
2. 推理预算会改变模型排名，benchmark 不披露 compute protocol 就不完整。[arXiv](https://arxiv.org/abs/2606.17930)
3. Fable 5 恢复访问，但单 provider 架构的供应风险已被验证。[来源](https://www.kubiosec.tech/blog/2026-07-03-AgenticUpdates)
4. 224G 到 448G 正推动 optical engine 向 package 靠近。[Marvell](https://www.marvell.com/blogs/scale-up-network-solutions-for-ai-infrastructure.html)
5. MCP stateless core 是 Agent 从 demo 走向生产 runtime 的重要一步。[MCP](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)

## 执行报告
- 周报；基于本周日报并补充 6 个方向；重要事件 6 件、LLM 深读 4 条、论文 3 篇、芯片专项 3 组、趋势 3 条、分享卡片 5 条。
