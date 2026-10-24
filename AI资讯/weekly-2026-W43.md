# AI 每周综述 2026-W43

> 2026-10-24 回溯补齐周一应生成的周报；信息截止 10-19。已读取本 ISO 周 10-19 日报，辅以近期公开来源；这是一份周初综述，不冒充完整周末回顾，不纳入 10-20 之后的发布。

## 本周最重要的 5-8 件事

1. **🔥 MAI-Base-2 预训练完成**：模型与算力协同成为竞争要点。为什么重要：80B 激活与 2.8T 总参数区分了计算和容量压力；训练阶段 BPB 优势尚需后训练任务验证。 [来源](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/)
2. **⭐ Muse Spark Open 开放权重**：增加企业自部署选择。为什么重要：816B 总参数意味着硬件容量仍是门槛，开放权重也不等于无商业限制。 [来源](https://research.meta.ai/blog/muse-spark-open)
3. **⭐ 224G 铜连接产品送样**：retimer 与 AEC 进入客户验证。为什么重要：板级与机架级链路要分别做信号完整性预算，不能只按总吞吐比较。 [来源](https://www.marvell.com/company/newsroom/marvell-connectivity-portfolio-3-2t-retimer-second-generation-1-6t-aec-dsp.html)
4. **⭐ CXL 扩展器预告**：容量池服务于推理。为什么重要：六通道设计和多主机共享改变容量利用方式，但 2027 Q1 送样时间限制近期可采购性。 [来源](https://www.marvell.com/company/newsroom/marvell-structera-x-cxl-3-2-memory-expanders-ai-inference.html)
5. **⭐ CPO 进入开放机架验证**：光引擎之外，机箱和系统接口也成为交付条件。为什么重要：Flex 验证系统与另一 XPU 演示系统说明生态集成进展，却尚不构成量产证据。 [来源](https://lightmatter.com/press-release/lightmatter-delivers-open-rack-cpo-switch-and-xpu-scale-up-server-through-independent-collaborations-with-flex-and-other-industry-partners/)

## 大模型与 LLM 技术解读

### ⭐ MAI-Base-2：稀疏模型的训练协同

- **背景**：10-15 公布预训练结果，本期回顾。
- **问题**：MoE 通信、数据重复与训练稳定性。
- **方法**：2.8T 总参数、80B 激活；使用 source scaling、改进 MoE/Muon 实现及激活梯度裁剪。
- **效果**：厂商报告 16K GB200 上训练 38 天；仍在后训练。BPB 比较不能替代应用评测，暂无独立评测。
- **来源**：[来源](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/)

### ⭐ Muse Spark Open：开放权重仍有许可与内存门槛

- **背景**：10-15 发布，近期跟踪。
- **问题**：自部署需兼顾总参数内存、算子和许可条件。
- **方法**：816B 总参数、42B 激活，1M 上下文；适配 MLX、SGLang 等引擎。
- **效果**：官方报告 MI350 端到端提升 3%；不把局部 kernel 加速等同整机收益。采用 Muse Model License，非 Apache；暂无独立评测。
- **来源**：[来源](https://research.meta.ai/blog/muse-spark-open)

### ⭐ 投机解码：接受量不是速度

- **背景**：10-16 提交，10-19 公开列表收录。
- **问题**：宽松验证可能让推理链变长，抵消每轮接受更多 token 的好处。
- **方法**：同提示比较 12 组 target/drafter，记录完成长度、验证轮次、实际耗时与准确率。
- **效果**：作者报告最宽松设置平均接受量 1.34 倍，而完成长度 1.54 倍；需要端到端核算。暂无独立评测。
- **来源**：[来源](https://arxiv.org/abs/2610.23818)


### ⭐ 蒸馏与行为耦合：训练信号的可迁移性

- **背景**：10-19 公开列表中的 ReDiST 与 Behavioral Couplings 分别研究监督有效性和行为副作用。
- **问题**：正确答案不保证有效教学，目标行为微调也不保证只改变目标行为。
- **方法**：前者按学生尝试修订并筛选轨迹；后者通过行为配对文档与后续微调研究耦合，两者是独立研究。
- **效果**：前者作者报告迁移收益，后者观察到耦合可组合；暂无独立评测，不据此推断部署模型发生概率。
- **来源**：[ReDiST](https://arxiv.org/abs/2610.23831)；[Behavioral Couplings](https://arxiv.org/abs/2610.23847)

## 本周必读论文

### 1. Acceptance Is Not Speed: Length Inflation in Training-Free Lossy Speculative Decoding
- 作者/时间：Chiatzen Wang 等，2026-10-16 提交、10-19 列表公开。
- 问题：接受率能否反映真实加速。
- 方法：对 12 组模型配对采用相同提示，比较宽松与无损验证。
- 影响：提示推理服务测试应同时报告长度、耗时与质量；结论限于论文实验。 [arXiv](https://arxiv.org/abs/2610.23818)

### 2. Beyond the Answer: Student-Conditioned Trace Construction for Reasoning Distillation
- 作者/时间：Shaoang Li 等，2026-10-16 提交、10-19 列表公开。
- 问题：答案正确的推理轨迹未必适合学生学习。
- 方法：ReDiST 修订学生尝试，由冻结的学生匹配 reader 筛选，再平衡轨迹和答案损失。
- 影响：训练数据筛选需评估迁移，而非只看源任务得分；作者报告跨基准收益，尚无独立复现。 [arXiv](https://arxiv.org/abs/2610.23831)

### 3. Behavioral Couplings: From Formation to Behavioral Emergence in Language Models
- 作者/时间：Dongyoon Hahm、Woogyeol Jin、Dennis Wei、Yi Zhou、Nathalie Baracaldo、Kimin Lee，2026-10-16 提交、10-19 列表公开。
- 问题：单一行为微调可能引出未目标化行为。
- 方法：通过配对行为文档训练再微调，观察耦合及多段组合。
- 影响：当前行为测试可能漏掉后续微调触发的效应；受控实验不能直接当成所有部署模型的风险概率。 [arXiv](https://arxiv.org/abs/2610.23847)

## 芯片与互连专项

### 1. 铜互连：先比较链路角色再比较带宽
3.2T retimer 与 1.6T AEC DSP 分别覆盖板内/背板和电缆链路。分析：同为 224G/lane，不代表均衡、功耗和距离预算可互换；选型应按通道损耗及端到端时延建立矩阵。发布已送样，实际板卡合规和误码率仍需验证。 [来源](https://www.marvell.com/company/newsroom/marvell-connectivity-portfolio-3-2t-retimer-second-generation-1-6t-aec-dsp.html)

### 2. CXL：容量受益不等于带宽替代
Structera X 将六个 DDR 通道与多主机共享组合。分析：对容量不足的 KV/embedding 工作负载具有意义，但跨 PCIe 链路带宽与延迟依然存在；应单独测 prefill 和 decode。与已送样铜 DSP 相比，其 2027 Q1 送样计划属于较晚交付阶段。 [来源](https://www.marvell.com/company/newsroom/marvell-structera-x-cxl-3-2-memory-expanders-ai-inference.html)

### 3. CPO：标准机架解决的是部分系统集成问题
Lightmatter 的交换系统验证与 XPU 演示是独立合作，不能拼接为同一商业部署。分析：ORv3/MHS 兼容有助机箱集成；激光可维护性、光纤布线、封装良率及热漂移仍需验证。CPO 与铜 AEC 服务的距离、集成位置不同，不能仅以带宽密度宣布相互替代。 [来源](https://lightmatter.com/press-release/lightmatter-delivers-open-rack-cpo-switch-and-xpu-scale-up-server-through-independent-collaborations-with-flex-and-other-industry-partners/)

## 趋势观察

- **模型评估正需要系统上下文**：MAI 的 BPB、Meta 的 kernel 加速与端到端加速是不同层级指标；它们共同提示采购评估要对齐任务和硬件。此为本文归纳。 [来源](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/)；[来源](https://research.meta.ai/blog/muse-spark-open)
- **效率指标必须保留质量约束**：投机解码的长度膨胀与 ReDiST 的源任务/迁移差异都说明单一代理指标会误导。此为跨论文归纳。 [论文一](https://arxiv.org/abs/2610.23818)；[论文二](https://arxiv.org/abs/2610.23831)

## 下周关注

周初补档口径：以下关注接下来一周，日期以官网为准。

| 日期（举办地） | 事件 | 关注点 | 官网 |
|---|---|---|---|
| 10-25—28 | Asilomar 2026 | 信号、系统与计算方法 | [官网](https://www.asilomarsscconf.org/) |
| 10-26—28 | Saudi HPC/AI 2026 | HPC 与 AI 基础设施 | [官网](https://saudihpcai.org/call-for-saudi-hpc-ai-2026/) |

## 📱 分享卡片

- 参数稀疏不等于内存稀疏：部署预算要区分激活参数与总参数。 [来源](https://research.meta.ai/blog/muse-spark-open)
- 接受更多 token 不一定更快：完成长度和质量也要测。 [论文](https://arxiv.org/abs/2610.23818)
- CPO 已到系统验证，验证系统仍不等于量产部署。 [来源](https://lightmatter.com/press-release/lightmatter-delivers-open-rack-cpo-switch-and-xpu-scale-up-server-through-independent-collaborations-with-flex-and-other-industry-partners/)
