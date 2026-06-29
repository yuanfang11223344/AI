# AI 每周综述 2026-W27

> 生成时间：2026-06-29 Asia/Shanghai；覆盖截至本周一可验证的公开信息。

## 本周最重要的 5-8 件事

1. 🔥 **frontier model 发布开始接受政府级安全审核**。OpenAI 与 Anthropic 的新模型均出现受限访问。**为什么重要**：模型可用性不再只由厂商 SLA 决定，企业必须设计 provider fallback、地域路由和能力降级。[AP](https://apnews.com/article/065d5398baac7f16c8265c2cb8ba2baa)
2. 🔥 **GPT-5.6 采用 Sol/Terra/Luna 分层路线**。**为什么重要**：能力、价格和 latency 分档会直接影响 agent router 与 workload placement；但正式参数和独立评测仍不足。[Axios](https://www.axios.com/2026/06/25/trump-administration-openai-gpt-model-release)
3. 🔥 **OpenAI/Broadcom 发布 Jalapeño 路线**。**为什么重要**：模型公司正在把竞争延伸到 inference silicon、rack 和 fabric，通用 accelerator 的价值链面临更强垂直整合。[OpenAI](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)
4. 🔥 **NVIDIA 把 CPO 纳入 Vera Rubin 平台**。**为什么重要**：200Gb/s SerDes 和 Ethernet Photonics 被放进量产系统叙事，光互连开始直接服务 million-GPU scale-out。[NVIDIA](https://nvidianews.nvidia.com/news/vera-rubin-full-production-agentic-ai-factory)
5. ⭐ **MCP 2026-07-28 RC 推进 stateless core**。**为什么重要**：生产 Agent 的瓶颈已从工具调用转向扩缩容、授权、任务恢复和互操作。[MCP](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)
6. ⭐ **RiVER 扩展可验证 RL 的任务边界**。**为什么重要**：它尝试让没有唯一答案、但有执行分数的工程优化任务进入 LLM RL 范围。[arXiv](https://arxiv.org/abs/2606.27369)

## 大模型与 LLM 技术解读

### GPT-5.6：模型分层和发布治理同时到来
- **背景**：agent 工作负载对能力、吞吐和成本的需求分化。
- **问题**：旗舰推理成本过高，同时高 cyber 能力引发发布风险。
- **方法**：Sol/Terra/Luna 分层并采用有限客户预览。
- **效果**：产品路线清晰，但正式 model card、价格和独立综合评测未齐。[AP](https://apnews.com/article/065d5398baac7f16c8265c2cb8ba2baa)

### Fable 5：能力与 safeguard routing 绑定
- **背景**：Claude 继续强化 coding 与长程 agent 能力。
- **问题**：统一拒答会损害正常请求，完全开放又增加高风险滥用。
- **方法**：触发风险时路由到较低能力模型，并配合访问审批。
- **效果**：官方称平均触发低于 5%；主要依据厂商材料，暂无独立安全评测。[Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5)

### RiVER：从 correctness reward 转向 ranking reward
- **背景**：RLVR 在数学、代码上有效，但覆盖不了无唯一解任务。
- **问题**：连续优化任务缺少二值 ground truth。
- **方法**：把 deterministic score 转成候选排序并做 group-relative RL。
- **效果**：作者报告优化任务收益，抗 reward hacking 与跨域泛化仍需复现。[arXiv](https://arxiv.org/abs/2606.27369)

### inference-scaled evaluation
- **背景**：模型排名常忽略测试时计算量。
- **问题**：固定 token budget 会混淆模型能力与资源限制。
- **方法**：跨任务改变预算、compaction 和重复尝试。
- **效果**：分数与排序会随 protocol 改变，评测必须披露 compute budget。[arXiv](https://arxiv.org/abs/2606.17930)

## 本周必读论文（3 篇）

1. **Reinforcement Learning without Ground-Truth Solutions can Improve LLMs**：以执行反馈排序替代唯一答案，为工程优化任务构造 RL 信号；影响在于扩展 RLVR 的适用范围。[arXiv:2606.27369](https://arxiv.org/abs/2606.27369)
2. **When are likely answers right?**：系统分析 sequence probability 与 correctness 的关系；影响在于提醒 best-of-N 和 verifier 不能盲目信任 likelihood。[arXiv:2606.27359](https://arxiv.org/abs/2606.27359)
3. **How Inference Compute Shapes Frontier LLM Evaluation**：改变 token budget、compaction 和重复提交；影响在于使 benchmark 结果与推理成本共同可比。[arXiv:2606.17930](https://arxiv.org/abs/2606.17930)

## 芯片与互连专项

### 1. Custom silicon 的重点从 die 转向 rack
Jalapeño 的关键信号是 model/kernel/serving 与 chip/board/network 共同定义。它可能提高特定负载效率，但也增加模型路线与硬件生命周期绑定；目前缺少 HBM、SerDes、功耗和良率数据。[OpenAI](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)

### 2. CPO 已进入平台竞争，但不会立即替代所有 pluggable
NVIDIA 将 200Gb/s SerDes CPO 放入 Vera Rubin；Coherent 展示 32×200G socketed CPO。前者强调系统控制，后者强调可维护光引擎。机架距离、热设计、laser serviceability 与良率决定 CPO、NPO、LPO 的实际边界。[NVIDIA](https://nvidianews.nvidia.com/news/vera-rubin-full-production-agentic-ai-factory) [Coherent](https://www.coherent.com/news/press-releases/coherent-co-packaged-optics-cpo-technologies-ofc-2026)

### 3. 224G 到 448G 是 electrical reach 与 optical placement 的共同问题
Marvell 同时布局 224G/448G SerDes 和 CPC/NPO/CPO，说明下一代互连不是只提高 baud rate，而是重新选择 copper reach、retimer、optical engine 与 package 边界。[Marvell](https://www.marvell.com/blogs/scale-up-network-solutions-for-ai-infrastructure.html)

## 趋势观察

1. **模型能力与供应连续性同等重要**：GPT-5.6、Fable 5 的受限发布说明合规与审核已进入模型架构选型。[AP](https://apnews.com/article/065d5398baac7f16c8265c2cb8ba2baa)
2. **Agent 基础设施正在协议化**：MCP stateless core 与 Tasks/Apps 分层，表明长任务、授权和恢复成为下一阶段工程主线。[MCP](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)
3. **AI scale-out 推动 optical I/O 前移**：NVIDIA CPO、Coherent socketed CPO 和 Intel OCI 分别从系统、光引擎、chiplet 三条路径逼近 compute package。[Intel](https://www.intel.com/content/www/us/en/products/details/network-io/silicon-photonics.html)

## 下周关注

| 事件 | 日期 | 关注点 | 官方链接 |
|---|---|---|---|
| AI Engineer World's Fair | 06-30 至 07-02 | Agent runtime、MCP、inference、evaluation | [官网](https://www.ai.engineer/worldsfair)
| ICML 2026 | 07-06 至 07-11 | LLM training、agents、AI systems | [官网](https://icml.cc/)
| MCP 新规范迁移准备 | 持续 | stateless server、Tasks、authorization | [RC](https://github.com/modelcontextprotocol/modelcontextprotocol/releases/tag/2026-07-28-RC)

## 📱 分享卡片

1. frontier model 已进入“能力发布 + 政府审核”双轨时代，企业需要多模型 fallback。[AP](https://apnews.com/article/065d5398baac7f16c8265c2cb8ba2baa)
2. GPT-5.6 用 Sol/Terra/Luna 分层承接能力、成本和吞吐，但独立评测仍待补齐。[Axios](https://www.axios.com/2026/06/25/trump-administration-openai-gpt-model-release)
3. Jalapeño 把 LLM 竞争推向 model-kernel-silicon-rack 全栈协同。[OpenAI](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)
4. CPO 正进入 AI platform，但可维护性、良率与 laser 供应仍决定规模化速度。[NVIDIA](https://nvidianews.nvidia.com/news/vera-rubin-full-production-agentic-ai-factory)
5. RiVER 让没有唯一答案的优化任务也能获得相对可验证 RL 信号。[arXiv](https://arxiv.org/abs/2606.27369)

## 执行报告

- 模式：周报；基于 28、29 日补生成日报并补充 6 类信源。
- 内容：重要事件 6 件、LLM 深读 4 条、论文 3 篇、芯片专项 3 组、趋势 3 条、下周关注 3 项、分享卡片 5 条。
- 保存：Obsidian、本地运行副本、AI.git 三处；Git commit/push 与通知结果在同步后核验。
