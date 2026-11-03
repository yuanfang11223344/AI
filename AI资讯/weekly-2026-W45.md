# AI 每周综述 2026-W45

> 周初综述·历史补档。2026-11-03 回溯整理，以本周唯一已补齐日报 2026-11-02 为基础，材料截止原始发布日期 11-02；不是完整一周总结，也不伪装成周一 10:00 快照。补充搜索覆盖 LLM、治理、芯片互连、开源、会议论文趋势、人才资本六方向；未核实的政策和融资数字不收录。

## 本周最重要的 5-8 件事

### 1. ⭐ Marvell–Lumentum 拟整合

- 要点：电连接与光器件进入同一拟议交易。
- 为什么重要：双方的互补技术让系统协同设计成为后续看点，但收购协议不能直接证明封装良率或客户收益。
- 来源：[原始来源](https://investor.marvell.com/news-events/press-releases/detail/1040/marvell-and-lumentum-to-combine-creating-a-category-leader-in-ai-connectivity)

### 2. ⭐ Grid Protectors

- 要点：算力规划把灵活用电纳入承诺。
- 为什么重要：Georgia 园区尚在开发，说明调度能力必须与电网协议一并设计，不能只按 GPU 数量规划容量。
- 来源：[原始来源](https://openai.com/index/grid-protectors-program/)

### 3. ⭐ Pando

- 要点：Agent 并发工作推动源代码服务重构。
- 为什么重要：按分支独立协调和 packfile 复用分别对应写入竞争与读取开销，瓶颈已延伸到模型之外。
- 来源：[原始来源](https://openai.com/index/scaling-git-at-openai/)

### 4. ⭐ Flide

- 要点：决策树分支共享状态可减少重复推理。
- 为什么重要：论文明确覆盖标准注意力和递归层，因此不能只按传统 KV cache 前缀共享理解。
- 来源：[原始来源](https://arxiv.org/abs/2610.42148)

### 5. ⭐ ZHA

- 要点：稀疏注意力连索引也需要降复杂度。
- 为什么重要：分层 key 摘要树将路由纳入复杂度分析，比只统计最终 attention 的稀疏度更有解释力。
- 来源：[原始来源](https://arxiv.org/abs/2610.42112)

### 6. ⭐ Anthropic S-1 跟踪

- 要点：模型公司的资本路径进入公开申报阶段。
- 为什么重要：公司公告仍未确定发行数量和价格区间，估值传闻不应替代正式文件。
- 来源：[原始来源](https://www.anthropic.com/news/s1-sec)

## 大模型与 LLM 技术解读

### 1. ⭐ Flide：决策分支复用

- **背景**：Agent 的分类决策常共享长前缀。
- **问题**：逐分支执行会重复计算，近似复用又可能改变概率。
- **方法**：将逻辑树展开为 token 流，通过 GPU 索引和 ForkAttention 管理标准注意力及 Gated DeltaNet 的分支状态。
- **效果**：作者报告最高 9.5 倍查询加速；精确算术等价不代表浮点逐位一致，也不是所有生成任务的加速比。暂无独立评测。
- 来源：[原始来源](https://arxiv.org/abs/2610.42148)

### 2. ⭐ DecisionRank：排序不必逐 token 生成

- **背景**：检索重排需要模型判断多个候选文档。
- **问题**：生成文档 ID 序列带来解码延迟，教师序列与学生分数空间不一致。
- **方法**：item-space on-policy distillation 将学生分数排序成序列，再由生成式教师提供条件反馈，并结合离线监督。
- **效果**：作者报告 8B 版本在 TREC DL 和 BRIGHT 分别较其生成式对照快 24.5 倍、16.7 倍；仅适用于论文设定。暂无独立评测。
- 来源：[原始来源](https://arxiv.org/abs/2610.42128)

### 3. ⭐ ZHA：把稀疏索引本身也做成次二次

- **背景**：长上下文不仅受 KV 存储约束，也受索引计算约束。
- **问题**：若稀疏注意力索引仍是二次复杂度，减少最终 attention 不能根治增长。
- **方法**：递归压缩 key 摘要形成树，query 自顶向下选择 key；用短程 KL 蒸馏适配预训练模型。
- **效果**：作者报告保持多种任务表现；百万以上上下文优势包含性能建模，亿级 token 是潜力判断，不能写成已部署能力。暂无独立评测。
- 来源：[原始来源](https://arxiv.org/abs/2610.42112)

## 本周必读论文

### 1. Flide: Accelerating Decision Inference with Exact State Forking

- 作者 / 时间：Bobo Li 等；2026-10-30 提交，11-02 列表可见。
- 问题：逐分支执行会重复计算，近似复用又可能改变概率。
- 方法：用 ForkAttention 复用分支状态，减少决策推理的重复计算。
- 影响：将执行状态正确性和吞吐联合讨论；下一步应复现不同分支深度下的显存与延迟。作者结果尚无独立评测。
- [arXiv](https://arxiv.org/abs/2610.42148)

### 2. DecisionRank: On-Policy Distillation for Decision-Model Rerankers

- 作者 / 时间：Yanran Wu 等；2026-10-30 提交，11-02 列表可见。
- 问题：生成文档 ID 序列带来解码延迟，教师序列与学生分数空间不一致。
- 方法：将生成式教师的排序反馈蒸馏给直接打分的决策模型。
- 影响：为检索系统提供直接打分路线；需要在相同硬件、批量与质量约束下比较生成式重排。作者结果尚无独立评测。
- [arXiv](https://arxiv.org/abs/2610.42128)

### 3. Subquadratic Attention with a Hierarchical Tree-Structured Indexer

- 作者 / 时间：Leon Lufkin 等；2026-10-30 提交，11-02 列表可见。
- 问题：若稀疏注意力索引仍是二次复杂度，减少最终 attention 不能根治增长。
- 方法：用 KV 摘要树进行稀疏检索，使路由与注意力总成本实现次二次增长。
- 影响：提醒长上下文系统同时计入索引开销；亿级 token 潜力仍需真实硬件验证。作者结果尚无独立评测。
- [arXiv](https://arxiv.org/abs/2610.42112)

## 芯片与互连专项

### 1. 电—光垂直整合：系统验证责任如何变化

Marvell–Lumentum 的拟议组合覆盖电与光技术，分析上可能减少不同供应商之间的接口协调，但也会增加客户对完整平台的依赖。选型应继续要求激光寿命、热漂移、光纤装配、可维修性和封装测试数据，不能只比较 SerDes 标称速率。交易是否完成与 CPO 产品是否通过客户认证是两个独立里程碑。

[原始来源](https://investor.marvell.com/news-events/press-releases/detail/1040/marvell-and-lumentum-to-combine-creating-a-category-leader-in-ai-connectivity)

### 2. 224G 铜链路与集成光学：边界由距离和功耗共同决定

Marvell 的 3.2T retimer 与 1.6T AEC DSP 已送样，面向板级、背板和机架铜连接；Broadcom 的集成光学文章讨论 OBO/NPO/CPO 路线。这两类方案不能仅按单 lane 数字互相替代：短距铜连接的布线与维护便利，需要同光学的距离、密度优势一起评估。工程比较应统一链路距离、FEC、线缆、接头与功耗边界；这里是路线分析，不提供未经测量的胜负结论。

[原始来源](https://www.marvell.com/company/newsroom/marvell-connectivity-portfolio-3-2t-retimer-second-generation-1-6t-aec-dsp.html)；[原始来源](https://www.broadcom.com/company/news/articles/optics/scaling-ai-infrastructure-with-integrated-optics)

### 3. CXL 容量池与长上下文：瓶颈不会只剩容量

六通道 CXL 3.2 扩展器增加可连接的内存资源，但 ZHA 所讨论的索引与访问模式仍决定计算和搬运成本。分析上，KV 分层放置需要将访问局部性、链路拥塞和尾延迟纳入实验；仅增加远端容量并不保证长上下文吞吐提升。两份资料没有提供联合系统测试，不能把推论写成已实现方案。

[原始来源](https://www.marvell.com/company/newsroom/marvell-structera-x-cxl-3-2-memory-expanders-ai-inference.html)；[原始来源](https://arxiv.org/abs/2610.42112)

## 趋势观察

1. **优化对象从 token 扩展到工作流基础设施。** Flide 处理决策分支，Pando 处理源代码并发，说明 Agent 的吞吐瓶颈横跨推理与开发基础设施。这是两项材料的综合观察，并非全行业统计结论。[原始来源](https://arxiv.org/abs/2610.42148)；[原始来源](https://openai.com/index/scaling-git-at-openai/)

2. **资本与用电约束共同影响扩张节奏。** S-1 申报和 Grid Protectors 分别提供融资与需求响应层面的公开证据；不能据此推算未来盈利或电力节省。[原始来源](https://www.anthropic.com/news/s1-sec)；[原始来源](https://openai.com/index/grid-protectors-program/)

3. **使用规则要沿服务链一致落实。** OpenAI 10-21 公告说明同类使用规则适用于直接、托管与转售访问，并宣布 Usage Policies 于 11-20 更新；这属于厂商规则，不等于新的国家法律或出口许可结论。[原始来源](https://openai.com/index/broad-access-through-consistent-standards/)

## 下周关注

周初报告优先列出接下来一周的已确认活动；ICCAD 延续至下一 ISO 周，不将尚未核实日期的发布会列为确定事件。

| 事件 | 日期（当地） | 关注点 | 官方链接 |
|---|---|---|---|
| ICCAD 2026 | 11-08—11-12 | EDA、验证、AI 芯片设计；11-09 起进入 W46 | [原始来源](https://ieee-ceda.org/event/conference/2026-international-conference-computer-aided-design) |

## 📱 分享卡片

1. **互连关注**：Marvell–Lumentum 是拟议交易，跟踪电—光协同与后续批准。 [原始来源](https://investor.marvell.com/news-events/press-releases/detail/1040/marvell-and-lumentum-to-combine-creating-a-category-leader-in-ai-connectivity)

2. **推理关注**：Flide 将分支依赖显式放入 GPU 元数据；作者加速结果需结合负载看。 [原始来源](https://arxiv.org/abs/2610.42148)

3. **活动关注**：ICCAD 11 月 8–12 日举行，适合跟踪 EDA、验证与 AI 硬件设计。 [原始来源](https://ieee-ceda.org/event/conference/2026-international-conference-computer-aided-design)
