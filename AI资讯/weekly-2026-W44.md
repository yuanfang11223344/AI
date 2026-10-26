# AI 每周综述 2026-W44

> 周初综述，统计截止 2026-10-26（Asia/Shanghai）。本 ISO 周为 10-26—11-01，目前只有 10-26 日报，已先生成并读取；因此以该日报的重点及前一周延续进展为基础，不是完整七天回顾。另补检模型技术、治理/出口管制、芯片、开源、会议、人才资本六方向；缺乏可靠新增证据的方向不凑数。

## 本周最重要的 5-8 件事

### ⭐ 原生 PyTorch 扩展硬件选择

TorchNeuron 进入公测。 **为什么重要**：eager、torch.compile 和 NKI 的同栈支持让已有训练代码有更直接的迁移入口；目前仍需验证兼容与成本。 [来源](https://aws.amazon.com/about-aws/whats-new/2026/10/aws-announce-native-pytorch-support-trainium/)

### ⭐ Agent Skills 进入基金会治理

10-22 公布标准移交。 **为什么重要**：名称、组织与域名一起转移，比单个厂商添加 loader 更影响标准长期归属；路线图仍需社区落实。 [来源](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation)

### ⭐ 企业模型采购进一步多云化

OpenAI 计划进入 Google Cloud Model Garden。 **为什么重要**：可使用既有 GCP 消费承诺改变采购路径，但 GA 尚未落地，不把合作声明当作部署业绩。 [来源](https://openai.com/index/openai-models-google-cloud/)

### ⭐ 长上下文训练尝试减少外部数据依赖

LongZero 自生成多文档任务。 **为什么重要**：将问答证据和长文本扩写拆开，使任务构造更易验证；两个模型上的结果仍需扩大验证。 [来源](https://arxiv.org/abs/2610.30132)

### ⭐ Agent 缓存从文本问题变为状态问题

AKV 处理 cache 操作历史依赖。 **为什么重要**：相同 token 前缀可能对应不同 KV 状态，传统 prefix matching 不再总是足够，直接影响 serving 正确性。 [来源](https://arxiv.org/abs/2610.28577)

### ⭐ 高速 I/O 需要分清标准与系统

PCIe 7.0 与 CPO 提供不同层级的带宽能力。 **为什么重要**：PCIe 128 GT/s 是链路规格，不能替代交换网络、光引擎和应用层端到端验证。 [来源](https://pcisig.com/faq?field_category_value%5B%5D=pci_express_7.0)

## 大模型与 LLM 技术解读

### ⭐ Haiku 5.5：小模型承担高频子任务（10-07，跟踪）

- **背景**：本月小模型更新聚焦摘要、分类及 subagent 工作。
- **问题**：高频任务对成本与延迟敏感，复杂编码仍需更强模型。
- **方法**：厂商提供小模型与 Opus/Sonnet 分工；公开页没有披露参数量、层数和完整训练配方，不能推断采用了何种蒸馏架构。
- **效果**：厂商称平均运行成本较 Haiku 4.5 降低约 75%，包含 tokenizer 与请求分布影响，不能当成每次调用的固定折扣。本次暂无独立评测。
- **来源**：[来源](https://www.anthropic.com/claude-haiku-5-5)

### ⭐ LongZero 与 TeachBack：合成数据质量比答案正确更重要（10-21）

- **背景**：两篇预印本探索自训练，分别处理长上下文任务和 on-policy self-distillation。
- **问题**：外部长文本训练数据昂贵；有参考答案的 Teacher 可能省略 Student 必须学习的推导。
- **方法**：LongZero 先生成紧凑问答与证据，再扩展多文档，结合检索任务进行 GRPO；TeachBack 调整参考推理后缀与提示，用双向偏好目标同时改进 Student 和 Teacher 监督。
- **效果**：LongZero 作者在两个模型、五个长上下文基准上报告优于所比较基线；TeachBack 报告数学和逻辑任务改善。均为作者实验，暂无独立评测，不能推断可无限自我提升。
- **来源**：[来源](https://arxiv.org/abs/2610.30132)；[来源](https://arxiv.org/abs/2610.29167)

### ⭐ 推理内存分三层看：KV、权重表与每 token 能耗（10-20—22）

- **背景**：长 Agent 轨迹与 n-gram 参数表都可能增加存储需求。
- **问题**：摘要重写会使缓存失效；降低某一内存项并不自动降低整机能耗。
- **方法**：Cache Engineering 用 drop/repos 操作 KV 并配套 AKV 状态复用；低比特 n-gram 工作量化表；Token-Energy Law 将共享权重字节、私有 KV 字节与实际 decode 时间分开建模。
- **效果**：分别属于缓存系统实验、表内存压缩实验和 GPU 板级能耗拟合。能耗论文在三个模型的 BF16、Hopper/Blackwell 设置中报告留出条件均值误差最高 3.73%；不能外推到全部 MoE、ASIC 或机房能耗。暂无独立评测。
- **来源**：[来源](https://arxiv.org/abs/2610.28577)；[来源](https://arxiv.org/abs/2610.31646)；[来源](https://arxiv.org/abs/2610.28576)

### ⭐ 原生 PyTorch 后端：Trainium 与 TPU 的迁移入口（10-20 起）

- **背景**：AWS 公布 TorchNeuron public beta；Google 的 TorchTPU 仓库公开提供 TPU backend。
- **问题**：算子、编译器和运行时迁移成本会限制芯片选择。
- **方法**：TorchNeuron 注册 neuron device，支持 eager、torch.compile 与 NKI 自定义 kernel；TorchTPU 作为 PyTorch backend 提供 TPU 算子运行入口。
- **效果**：代码可用和接口兼容不等于任意模型零修改或性能持平；需检验数值、动态形状、分布式通信及吞吐。暂无统一独立对比评测。
- **来源**：[来源](https://aws.amazon.com/about-aws/whats-new/2026/10/aws-announce-native-pytorch-support-trainium/)

官方代码：[TorchTPU](https://github.com/google-pytorch/torch_tpu/blob/main/README.md)；[TorchNeuron](https://github.com/aws-neuron/torch-neuronx)。

### ⭐ Agent 集成：标准治理与云端分发

- **背景**：10-22 Agent Skills 移交 AAIF；10-21 OpenAI 宣布 Google Cloud 合作。
- **问题**：流程可移植性和企业部署治理是两个不同瓶颈。
- **方法**：前者以 SKILL.md 和配套资源统一流程描述，后者计划经 Model Garden 分发模型与 Codex，并沿用云端权限控制。
- **效果**：标准治理已有公告，云端 GA 仍计划未来几周；两者都没有提供统一任务成功率实测，暂无独立评测。
- **来源**：[Anthropic](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation)；[OpenAI](https://openai.com/index/openai-models-google-cloud/)

## 本周必读论文

以下从周一日报中选择，均是近期预印本，并非 W44 已录用论文。

### 1. LongZero: Enhancing Long-Context Reasoning in LLMs from Zero Data

- **作者 / 时间**：Zhennan Wan、Jianfei Chen；2026-10-21。
- **问题**：长上下文 RL 的可验证数据供给不足。
- **方法**：先生成问答和支持证据，再扩成长文档，联合多跳与多轮检索任务进行 GRPO。
- **影响**：把数据生成过程拆开，可检验是否真正学习跨文档推理；论文覆盖两个模型、五个基准，不能泛化为所有数据均可自造。
- **来源**：[arXiv](https://arxiv.org/abs/2610.30132)

### 2. TeachBack: Learning to Teach via Bidirectional Preference Optimization for On-Policy Self-Distillation

- **作者 / 时间**：Linfei Ji、Jiayun Li、Haozhao Wang 等；2026-10-21。
- **问题**：Teacher 知道正确答案，可能遗漏 Student 所需的探索与自我纠错过程。
- **方法**：控制参考推理后缀、改变提示，并以双向偏好优化让 Student 行为反馈到 Teacher 监督。
- **影响**：提示评测蒸馏数据不能只看答案准确率，还需看是否适合无特权信息的学生；结果来自作者数学和逻辑任务实验。
- **来源**：[arXiv](https://arxiv.org/abs/2610.29167)

### 3. The Token-Energy Law of LLM Decode: Active Bytes and Realized Time Account for Joules per Token

- **作者 / 时间**：Chenxu Niu、Batuhan Sencer、Wei Zhang、Yong Chen；2026-10-20。
- **问题**：单一 tokens/s 无法解释 batch 和上下文对能耗的不同作用。
- **方法**：分别建模共享权重字节、私有 KV 历史字节和实际 decode 时间，按模型与硬件/运行时校准。
- **影响**：可帮助设计 HBM/KV 实验变量，但已知 decode 时间的拟合精度不等于对未知系统的预测精度；仅对应论文 GPU 板级边界。
- **来源**：[arXiv](https://arxiv.org/abs/2610.28576)

## 芯片与互连专项

### 1. 224G：器件可选性与整条链路预算

Semtech 的线性 TIA/driver 支持多种光调制器，为 NPO/CPO 设计提供接口选择。工程推论：不能仅比较每 lane 标称速率，应同时检查电摆幅、带宽、线性度、光预算与温漂。选择不同调制器意味着不同封装及驱动约束；本次公告没有给出足以公平比较整机 pJ/bit 的独立材料，不能得出某一种材料全面胜出的结论。[来源](https://investors.semtech.com/news/semtech-sets-bar-for-224g-optical-connectivity/1fdb3563-ce31-49f2-81e4-464ce69a3ec6)

### 2. CPO：交换容量与可维护性分开验收

BCM78919 产品简报给出 102.4 Tb/s 和 212.5 Gb/s PAM4 收发器规格。与分立光模块相比，CPO 缩短电通道的价值需在系统中验证；工程评估还应包含光纤接入、散热、激光源与维修边界。标称容量不足以回答单流尾延迟和故障后的可用带宽。这里的可维护性清单是分析建议，不是宣称厂商已有某项实测结果。[来源](https://docs.broadcom.com/doc/BCM78919-PB)

### 3. PCIe 与光网络：不同层级不能直接替换

PCIe 7.0 的 128 GT/s、x16 双向合计 512 GB/s 与 CPO 交换芯片的 Tb/s 总容量口径不同。系统预算应先明确主机到加速器、加速器内部通信还是机架网络，再比较单方向有效载荷、协议开销和拓扑。由此推论，采购阶段不能用总带宽数字直接排名；也不能将标准发布误认为服务器与板卡已经全量支持。[来源](https://pcisig.com/faq?field_category_value%5B%5D=pci_express_7.0)

## 趋势观察

1. **软件入口正在影响芯片竞争**：TorchNeuron 提供原生 PyTorch 接口，TorchTPU 也开放 backend。推论是试验成本有望下降，是否改变 TCO 仍取决于实际模型及集群效率。[来源](https://aws.amazon.com/about-aws/whats-new/2026/10/aws-announce-native-pytorch-support-trainium/)；[TorchTPU](https://github.com/google-pytorch/torch_tpu/blob/main/README.md)。
2. **训练数据从数量转向监督过程设计**：LongZero 控制证据生成顺序，TeachBack 控制教师推导方式。两者支持一个研究方向判断：应分析任务可验证性与监督可学习性，而不是只统计合成 token。[来源](https://arxiv.org/abs/2610.30132)；[来源](https://arxiv.org/abs/2610.29167)
3. **Agent 成本需要系统边界**：缓存工作关注活动 KV，能耗工作区分字节需求与耗时。推论是 API 账单、GPU 板能耗和整机能耗应分别报告，不能将一种成本降幅直接套到另一种成本。[来源](https://arxiv.org/abs/2610.28577)；[来源](https://arxiv.org/abs/2610.28576)

## 下周关注

下一个 ISO 周为 11-02—08，本次未核实到足以列入的确定发布日程；下表是跟踪计划，不冒充官方活动。

| 事件 / 跟踪对象 | 日期 | 关注点 | 官方链接 |
|---|---|---|---|
| OpenAI on Google Cloud 可用性 | 11-02—08 跟踪；GA 日未定 | 是否公布区域、配额及实际可用性 | [公告](https://openai.com/index/openai-models-google-cloud/) |
| TorchNeuron beta 后续 | 11-02—08 跟踪；无预告发布日期 | 算子兼容、分布式训练与修复记录 | [官方 Releases](https://github.com/aws-neuron/aws-neuron-sdk/releases) |

本周可参加活动（10-26—11-01）：

| 日期（举办地） | 事件 | 关注点 | 官网 |
|---|---|---|---|
| 10-26—29（会期 24—29） | EMNLP 2026 | 长上下文、训练与评测论文 | [官网](https://2026.emnlp.org/) |
| 10-26—28 | Saudi HPC/AI 2026 | HPC/AI 系统与科研应用 | [官网](https://saudihpcai.org/call-for-saudi-hpc-ai-2026/) |
| 10-28—30 | TEDAI Vienna | AI 应用、安全和产业观点 | [官网](https://tedai-vienna.ted.com/) |

## 📱 分享卡片

- PyTorch 原生后端降低试用门槛，性能仍需按模型实测。[来源](https://aws.amazon.com/about-aws/whats-new/2026/10/aws-announce-native-pytorch-support-trainium/)
- KV cache 应作为可管理的运行时状态，而非只靠摘要裁剪。[来源](https://arxiv.org/abs/2610.28577)
- PCIe x16 的双向合计带宽不能直接当成单向业务吞吐。[来源](https://pcisig.com/faq?field_category_value%5B%5D=pci_express_7.0)
