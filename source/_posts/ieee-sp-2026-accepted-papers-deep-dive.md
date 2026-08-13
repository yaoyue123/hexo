---
title: IEEE S&P 2026 录用论文深度解读（252 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, IEEE S&P, 2026]
---

IEEE S&P（Oakland）2026 于 2026 年 5 月在旧金山举行，共录用 252 篇论文。本文按研究方向分类，对每篇论文给出深度解读（含独立分析思考），有 arXiv 全文的基于精读，其余基于摘要，并附可获取的 PDF 链接。

## 全部 252 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（20 篇）

#### SoK: Critical Evaluation of Quantum Machine Learning for Adversarial Robustness.
- **作者**: Saeefa Rubaiyet Nowmi, Jesus Rafael Lopez, Md Mahmudul Alam Imon, Shahrooz Pouryousef, Mohammad Saidur Rahman
- **方向**: AI/ML安全
- **解读**: 

**问题**：量子机器学习（QML）将量子计算原理融入学习算法，有望带来更强的表示能力与计算效率，但其在对抗条件下的安全与鲁棒性研究严重不足。本文提出首个QML对抗鲁棒性系统化（SoK）研究，将概念分类与实证评估相结合，覆盖黑盒、灰盒与白盒三类威胁模型，为安全部署QML奠定基础。

**方法**：作者构建统一威胁模型分类体系，实现五类代表性攻击：黑盒标签翻转投毒、灰盒编码器级无差别投毒（QUID）与代理模型干净标签后门、白盒电路级后门（QTrojan）及FGSM/PGD梯度逃逸。实验采用量子多层感知机（QMLP），在MNIST与恶意软件数据集AZ-Class上考察电路深度（2/5/10/50层）与角度/振幅两种编码的影响，并用去极化噪声（p=0.01）模拟NISQ环境。

**结果**：振幅编码干净准确率最高（MNIST 92.6%、AZ-Class 67%），但在对抗扰动与噪声下崩溃，浅层角度编码更稳定；QUID无噪声下有效但被噪声削弱，代理模型后门除非电路被噪声淹没否则持续存在；电路级后门在多分类设定下失效，存在扩展性瓶颈。QMLP在标签翻转下比经典MLP鲁棒（相对准确率约0.9对0.5），但对梯度逃逸显著更脆弱。

**我的思考**：该工作揭示了"精度-鲁棒性"权衡与噪声作为被动防御的不对称性，价值在于统一碎片化的量子安全研究并指出量子专用防御的必要性。局限是QMLP与CMLP的差距主要源于NISQ强制的PCA降维而非量子学习本身；多分类后门失效说明攻击研究仍处早期。评估基于单架构与模拟器，结论外推需真实硬件验证，其威胁感知与噪声鲁棒的设计框架尚待落地检验。

- **arXiv PDF**: [SoK: Critical Evaluation of Quantum Machine Learning for Adversarial Robustness.](https://arxiv.org/pdf/2511.14989)

#### Parasites in the Toolchain: A Large-Scale Analysis of Attacks on the MCP Ecosystem.
- **作者**: Shuli Zhao, Qinsheng Hou, Zihan Zhan, Yanhao Wang, Yuchong Xie, Yu Guo, Libo Chen, Shenghong Li, Zhi Xue
- **方向**: AI/ML安全
- **解读**: LLM 通过模型上下文协议（MCP）与外部系统深度集成，从被动信息处理器转变为任务型工具链的自主编排者，攻击面随之剧增、攻击目标从操纵单个输出升级为劫持整个执行流。论文识别并刻画了一种系统性隐私泄露模式——"寄生工具链攻击"（Parasitic Toolchain Attacks），实例化为 MCP 非预期隐私泄露（MCP-UPD）。

此类攻击无需受害者直接交互：对手把恶意指令嵌入 LLM 在合法任务中访问的数据源。与提示注入、数据投毒不同，它攻击互连的工具链本身，编排多个工具构成协同工作流以达成目标。攻击逻辑分三阶段展开：寄生摄取（Parasitic Ingestion）、隐私收集（Privacy Collection）与泄露（Disclosure），最终静默外传私密数据。根因分析表明 MCP 缺少上下文-工具隔离与最小权限强制，使敏感调用不受约束地传播。

为评估严重性，作者构建 MCP-SEC 对生态进行首次大规模普查，分析 1,360 台服务器上的 12,230 个条目；结果显示生态中遍布真实可利用的泄露 gadget，手段多样，凸显 LLM 集成环境的系统性风险与部署防御机制的紧迫性。

首次把"工具链编排"本身系统化为攻击面，三阶段模型清晰、普查数据支撑充分；与传统提示注入相比，攻击目标从输出提升到执行流，威胁模型更贴近真实 agent 应用。局限：普查基于可公开发现的服务器，未覆盖暗处部署；上下文-工具隔离与最小权限是明确的工程修复方向，但落地受生态碎片化制约。对 MCP 等 agent 协议的标准化与安全设计有直接政策含义。

- **arXiv PDF**: [Parasites in the Toolchain: A Large-Scale Analysis of Attacks on the MCP Ecosystem.](https://arxiv.org/pdf/2509.06572)

#### Ensemble Conformal Predictor (EnCP): A New Conformal Predictor with Robustness Guarantees Against Data Poisoning Attacks.
- **作者**: Yuxin Yang, Qiang Li, Runyang Feng, Liren Shan, Binghui Wang
- **方向**: AI/ML安全
- **解读**: 共形预测（conformal prediction）提供分布无关的预测集覆盖保证，正被用于医疗、自动驾驶等风险敏感决策；但训练数据投毒可破坏其覆盖率保证，现有共形预测器普遍缺乏对抗投毒下的鲁棒性论证。论文提出集成共形预测器 EnCP，为数据投毒攻击提供鲁棒性保证。

据标题推断，EnCP 组合多个共形预测器（如在不同数据子集或不同评分函数上训练的成员），通过精心设计的集成聚合规则（如投票或最坏情况边界），使即便部分训练数据被投毒，整体预测集仍保持覆盖率与效率的统计保证，并给出可证明的理论界限。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以不同投毒预算下覆盖率保持与预测集大小的理论保证及实验验证作为证据。

共形预测的"保证"在投毒下失效是实际关切，EnCP 提供可证明的鲁棒性是对该方向的必要补强；理论界限的给出使其区别于经验性防御。局限：鲁棒保证通常以效率（更大的预测集）为代价，投毒预算假设决定保证强度；与随机化平滑等既有鲁棒方法的比较值得展开。总体是理论意义明确、契合风险敏感应用需求的工作。


#### Exploiting Leaderboards for Large-Scale Distribution of Malicious Models.
- **作者**: Anshuman Suri, Harsh Chaudhari, Yuefeng Peng, Ali Naseh, Alina Oprea, Amir Houmansadr
- **方向**: AI/ML安全
- **解读**: 针对机器学习模型的投毒攻击已被广泛研究，但对手如何大规模分发投毒模型这一环节却几乎无人探索。论文揭示模型排行榜——用于模型发现与评估的排名平台——可作为投毒模型隐秘、大规模分发的强大渠道，填补攻击链上的关键空白。

作者提出 TrojanClimb 通用框架：在注入恶意行为的同时维持具有竞争力的排行榜表现。框架在四种异构模态上验证有效性——文本嵌入、文本生成、文本转语音与文生图——成功获得高排名，同时嵌入从后门到偏见注入等任意有害功能。

跨四模态的实验证明框架的通用性，表明模型生态存在显著脆弱性：排行榜作为信任锚点却可被武器化，亟需重新设计检测与过滤（如投毒模型筛查）机制；同时警示社区采用未验证来源模型的风险。

排行榜是模型生态的信任基础设施，将其变成攻击分发通道属于供应链思维的高阶攻击；跨模态的通用性说明这不是个别平台的偶然漏洞而是范式级问题，与"模型即代码"的加载侧风险（如并行工作）形成完整攻击链。局限：隐蔽性受评测基准质量影响，防御上应引入基准污染检测与模型来源审计；对 HuggingFace 等平台的治理有直接提醒价值。

- **arXiv PDF**: [Exploiting Leaderboards for Large-Scale Distribution of Malicious Models.](https://arxiv.org/pdf/2507.08983)

#### GraphRAG Under Fire.
- **作者**: Jiacheng Liang, Yuhui Wang, Changjiang Li, Tanqiu Jiang, Rongyi Zhu, Neil Gong, Ting Wang
- **方向**: AI/ML安全
- **解读**: GraphRAG 通过把外部知识组织为多尺度图结构来增强检索增强生成（RAG），使语言模型同时整合宏观语境与细粒度细节，已在多领域取得成功；但其安全影响此前几乎未被探索。论文研究 GraphRAG 对投毒攻击的脆弱性，并发现一个耐人寻味的悖论。

悖论在于：由于图索引与检索特性，现有 RAG 攻击在 GraphRAG 上反而效果更差；但同一特性也创造了新的攻击面。作者提出 GragPoison，利用图底层共享的关系构造能同时危害多个查询的文本，采用三种关键策略：关系注入（引入虚假知识）、增强（放大攻击影响力）、叙事（把恶意内容嵌入连贯文本中）。

在多样数据集上的实验表明，GragPoison 在有效性（攻击率最高达 98%）与可扩展性（仅用 68% 的文本量）上显著优于现有攻击，且适用于多种 GraphRAG 变体；论文同时探讨了防御措施与局限，指出有前景的研究方向。

揭示"图结构既是缓解也是新攻击面"的悖论颇具洞察力；一次污染同时危害多个查询的效率优势，把 RAG 投毒的威胁模型从单点提升到批量层面，对以图为基础的企业知识库是真实风险。局限：攻击率依赖图构建方式与查询模式，防御评估仍属初步；对图索引与检索结果的可信度分级是值得跟进的方向。

- **arXiv PDF**: [GraphRAG Under Fire.](https://arxiv.org/pdf/2501.14050)

#### AI Wrote My Paper and All I Got was This False Negative:* Measuring the Efficacy of Commercial AI Text Detectors.
- **作者**: Seth Layton, Bernardo B. P. Medeiros, Kevin R. B. Butler, Patrick Traynor
- **方向**: AI/ML安全
- **解读**: 商用 AI 文本检测器被广泛应用于学术诚信、招聘筛选与内容审核，其裁决可能带来处分、拒聘等实质后果，但检测器的真实效能缺乏独立、系统的实证测量。标题中的"False Negative"直指检测器漏判 AI 生成文本这一关键失败模式。论文测量商用 AI 文本检测器的效能。

据标题推断，作者用真实 AI 生成文本（覆盖不同模型、提示方式与改写策略）与人类写作文本构成测试语料，对多种商用检测器进行黑盒评估，测量漏检率（假阴性）与误判率（假阳性），并分析其与文本长度、语言、改写程度等因素的关系。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以各检测器的漏检/误检率矩阵与失败模式分析作为核心证据。

AI 文本检测器在现实中被赋予超出其能力的裁决权重，独立测量其效能是重要的公共安全议题；假阴性（漏判）与假阳性（冤枉人类作者）构成双重风险。局限：检测器与生成模型迭代极快，评估结论时效性有限；若证实改写、翻译可轻易规避检测，将动摇"检测即治理"的思路，提示应转向模型水印、内容来源认证等源头方案；该实证对高校与招聘平台的工具选型具有直接参考意义。


#### Your Compiler is Backdooring Your Model: Understanding and Exploiting Compilation Inconsistency Vulnerabilities in Deep Learning Compilers.
- **作者**: Simin Chen, Jinjun Peng, Yixin He, Junfeng Yang, Baishakhi Ray
- **方向**: AI/ML安全
- **解读**: 深度学习编译器是现代 DL 系统的核心基础设施，提供超越厂商专库的灵活性与可扩展性。论文揭示其设计层面的根本漏洞：一个官方、未经修改的编译器能否在编译过程中改变模型语义并植入隐藏后门？作者从对抗与自然两种场景系统研究该问题。

对抗场景中，作者构造良性模型：触发器在编译前完全无效，编译后却成为有效后门；在 6 个模型、3 个商业编译器、2 个硬件平台上，攻击在触发输入上实现 100% 成功率，同时保持正常精度且不被 SOTA 检测器发现。自然场景中，分析 HuggingFace 下载量前 100 的模型（含一个下载量超 2.2 亿的模型），发现其中 31 个存在此类问题——即使没有恶意操纵，编译本身也会引入风险。

攻击跨硬件平台与浮点设置泛化；该工作首次系统暴露 DL 编译器"编译可静默改变模型语义"这一被忽视的威胁，开辟安全可信 ML 的新研究方向。

把经典"可信编译器"问题引入 DL 生态：模型作者信任编译器，而编译器可能成为后门注入点，对"下载即用"的开源模型生态是真实风险。局限：自然场景中 31/100 的发现需区分无害数值差异与可利用后门；防御上需要编译语义等价性验证工具。对模型供应链审计具有开创性意义。

- **arXiv PDF**: [Your Compiler is Backdooring Your Model: Understanding and Exploiting Compilation Inconsistency Vulnerabilities in Deep Learning Compilers.](https://arxiv.org/pdf/2509.11173)

#### DREAM: Scalable Red Teaming for Text-to-Image Generative Systems via Distribution Modeling.
- **作者**: Boheng Li, Junjie Wang, Yiming Li, Zhiyang Hu, Leyi Qi, Jianshuo Dong, Run Wang, Han Qiu, Zhan Qin, Tianwei Zhang
- **方向**: AI/ML安全
- **解读**: 尽管集成了安全对齐与外部过滤器，文生图（T2I）系统仍可能生成性、暴力等有害内容，引发暴露与滥用担忧。红队测试——主动发现能诱发不安全输出的多样提示——是上线前评估与改进的关键手段，但现有自动化方法把提示发现当作孤立的逐提示优化任务，可扩展性、多样性与有效性受限。

DREAM 直接建模目标系统提示的概率分布，实现对有效性的显式控制，并支持训练完成后的大规模高效采样；无需代表性训练样本，借鉴能量模型把目标重写为简单可解的形式；提出 GC-SPSA 算法，在长且可能不可微的管线中提供稳定的梯度估计；推理阶段采用多样性感知策略增强提示的丰富度。

大量实验证明 DREAM 在多种过滤器设置下均达到 SOTA 性能，成功率和多样性全面领先；代码已开源。

从"逐提示优化"转向"分布建模"是红队方法论的重要升级：一次训练换取大规模采样，能量模型加零训练样本的设计实用且优雅。局限：分布建模质量依赖代理得分器，GC-SPSA 在超长管线上的稳定性边界待查；"有害性"标注与人类判断的对齐仍是瓶颈。对生成模型安全评测的工业化有直接推动。

- **arXiv PDF**: [DREAM: Scalable Red Teaming for Text-to-Image Generative Systems via Distribution Modeling.](https://arxiv.org/pdf/2507.16329)

#### On the (In)Security of Loading Machine Learning Models.
- **作者**: Gabriele Digregorio, Marco Di Gennaro, Stefano Zanero, Stefano Longari, Michele Carminati
- **方向**: AI/ML安全
- **解读**: 
模型共享让机器学习变得触手可及，但加载共享模型的安全风险长期被低估，安全叙事与实现之间明显错位。本文以任意代码执行为核心威胁，提出三个问题：主流框架与中心的安全态势如何、宣称安全且支持完整模型共享的机制是否真安全、安全叙事是否误导用户感知。

方法上，作者系统分析 TensorFlow、Keras、PyTorch、scikit-learn、XGBoost 五大框架及 Hugging Face、Kaggle 等五大中心，将格式划分为自包含/非自包含、代码型/数据型两维；随后手工逆向 Keras safe_mode 与 Skops 的"secure persistence"，发现 6 个 0-day 漏洞（均获 CVE），并将 PoC 原样上传 Hugging Face 检验扫描器；最后对 62 名从业者做匿名调查。

结果显示：Keras 侧包括恶意 config.json 模块解析（CVE-2025-1550，CVSS 7.3）、经 Lambda 层代码复用禁用 safe_mode（CVE-2025-8747/9906，CVSS 8.6）、HDF5 旧格式下 safe_mode 被静默忽略（CVE-2025-9905，CVSS 7.3）；Skops 侧包括 MethodNode 对象图遍历（CVE-2025-54413，CVSS 8.7）、OperatorFuncNode 校验绕过（CVE-2025-54412，CVSS 8.7）与模型卡片静默回退 joblib（CVE-2025-54886，CVSS 8.4），全部可致任意代码执行。Hugging Face 扫描评估中 picklescan 全漏报，SV.1/SV.2 的 .skops PoC 被标"Safe"。调查显示 safe_mode=True 使舒适度从 4.5/10 升至 7.8/10（Wilcoxon p=3.14×10⁻⁹，|r|=0.87），担心任意代码执行者从 83% 降至 15.1%。

我的思考：本文有力证伪了"JSON 等数据型格式天然安全"的假设——正如 Google 安全团队所言"模型就是代码"，安全取决于 JSON 到代码对象转换中的校验强度，允许列表/黑名单机制可被代码复用绕过并需反复打补丁。SV.1/SV.2 被标"Safe"与 KV.3 的"Won't Fix"凸显生态碎片化与标签误导问题。局限是调查样本小、漏洞集中于两个框架。对社区的启示是：把模型扫描当传统恶意软件检测看待，仅作第一道防线。

- **arXiv PDF**: [On the (In)Security of Loading Machine Learning Models.](https://arxiv.org/pdf/2509.06703)

#### Adversarial Hubness in Multi-Modal Retrieval.
- **作者**: Tingwei Zhang, Fnu Suya, Rishi D. Jha, Collin Zhang, Vitaly Shmatikov
- **方向**: AI/ML安全
- **解读**: Hubness 是高维向量空间中的已知现象：某些天然分布中的点异常接近大量其他点，导致检索中部分条目意外（且错误地）对大量查询显得相关。论文研究攻击者如何利用 hubness，把多模态系统中的任意图像或音频输入变成"对抗 hub"（adversarial hub）。

对抗 hub 可用来注入通用内容（如垃圾信息，使其被数千个不同查询检索返回），也可对与特定攻击者选定概念相关的查询实施定向攻击。作者提出构造方法并建立用于评估的基准数据集，以 Pinecone 向量数据库实现的图-图检索为例验证：在文本-字幕-图像检索中，用 100 张随机图生成的单个 hub 在 25,000 个测试查询中的 21,000 多个上成为 top-1 结果（普通 hub 仅命中 102 个查询），展示出极强泛化能力。

研究还检验现有缓解 hubness 的技术能否抵御对抗 hub，结论是它们对定向对抗 hub 并不有效，凸显多模态检索系统的防御缺口。

把检索系统的"固有噪声"转化为可控攻击原语，21,000+/25,000 的 top-1 占比说明对抗 hub 威力远超自然 hub；对向量数据库驱动的 RAG 与搜索产品是真实威胁。局限：定量实验以图-图与字幕-图检索为主，跨模态（如音频到图）的定量结果待展开；缓解技术失效提示需要专门的对抗防御而非通用去 hub 化。对多模态检索生态的安全设计有直接影响。

#### WRATH: Turning Watermark Robustness Against Itself via a Watermark-Agnostic Black-Box Invalidation Attack.

- **作者**: Nan Jiang, Juan Hu, Bangjie Sun, Terence Sim, Jun Han

- **方向**: AI/ML安全

- **解读**: 内容水印依赖"鲁棒性"抵抗编辑、压缩等处理，版权保护与内容溯源以此为前提。然而鲁棒性也可被反向利用：若攻击者能在黑盒条件下（不掌握水印算法细节）使水印失效，水印体系的信任基础即被瓦解，AI 生成内容的可溯源承诺也将落空。（无摘要，据标题推断）

WRATH 提出与水印算法无关（watermark-agnostic）的黑盒失效攻击：把水印的鲁棒性本身当作攻击工具——通过精心构造的扰动或变换，在保持内容可用的同时使水印检测失效；攻击无需访问水印解码器，也无需了解具体水印方案。（无摘要，据标题推断）

论文展示了该攻击对现有鲁棒水印方案的失效效果，说明"鲁棒性"与"可移除性"之间存在可被利用的张力；具体量化结果（成功率、保真度代价等）需查阅原文。（无摘要，据标题推断）

我的思考：思路的巧妙之处在于"以其人之道还治其人之身"——鲁棒性常被视为水印的优势，本文将其重述为攻击面，对水印方案设计（鲁棒性与不可移除性的权衡）有方法论启示。局限在于黑盒约束下攻击的通用性与内容保真度代价未知，与已有白盒/灰盒移除攻击相比，其核心增益应是无需水印知识；对 C2PA 类生成内容溯源标准的防御设计有直接参考价值。


#### Are LLM-Enhanced Graph Neural Networks Robust Against Poisoning Attacks?
- **作者**: Yuhang Ma, Jie Wang, Zheng Yan
- **方向**: AI/ML安全
- **解读**: 
LLM 增强的图神经网络通过语义化节点表示显著提升文本属性图（TAG）上的性能，但其面对训练期投毒攻击（同时操纵图结构与文本属性）的鲁棒性尚无系统研究。既有工作存在覆盖不全、缺乏 LLM 出现后发布的数据集（预训练泄漏会高估鲁棒性）、缺少攻防方向讨论三类局限，本文提出系统化鲁棒性评估框架填补空白。

方法上，框架将 8 个特征增强器（TAPE、KEA；LLaMA-2、text-embedding-3-large、Linq-Embed-Mistral；SimTeG、E5-Large、ModernBert）与 3 个骨干（GCN、GAT、GraphSAGE）组合成 24 个受害模型；攻击集含 6 个结构投毒（Nettack、SGA、NAG-R；Mettack、DICE、PGA）与 3 个文本投毒（DeepWordBug、Bert-Attack、MAYA）；在 Cora、Pubmed、Ogbn-products 子集及 LLM 出现后发布的 Tape-arxiv23 子集上，以灰盒对手评测精度与 RDA，并做嵌入质量分析，另提出组合攻击与图净化防御。

结果显示：干净图上 LLM 增强模型全面优于浅层嵌入基线（Tape-arxiv23 上 GCN 最大提升 13.55%）；Nettack 高扰动下浅层 GCN 精度仅 58.15%–75.12%，最佳 LLM 增强模型达 67.56%–85.84%；Mettack 高扰动下提升 8.04%–39.78%。TAPE 与 E5-Large 最鲁棒，GraphSAGE 骨干最强；文本攻击远弱于结构攻击（基线最大 RDA 7.93% vs Mettack 的 15.49%），部分模型甚至出现负 RDA。0%–100% 重叠率实验证明鲁棒性增益主要源于 LLM/LM 架构集成而非预训练数据泄漏。

我的思考：这是迄今覆盖最广（24 模型×9 攻击×4 数据集）的 LLM-enhanced GNN 投毒鲁棒性研究，其"反直觉"结论——语义丰富嵌入因编码了标签与结构信息反而更抗投毒——为 GNN 安全设计提供了实证支撑。局限：后 LLM 数据集仅 Tape-arxiv23 一个、灰盒假设偏强、未直接投毒 LLM 本身。组合攻击与图净化防御使攻防共演研究有了起点。它提示 LLM 特征增强不仅提精度，还可能天然提升鲁棒性。

- **arXiv PDF**: [Are LLM-Enhanced Graph Neural Networks Robust Against Poisoning Attacks?](https://arxiv.org/pdf/2603.26105)

#### Breaking Free from Ivory Tower: Evaluating and Enhancing Real-world Chinese Underground Adversarial Jargon Detection.

- **作者**: Zhifan Jiang, Mingxuan Liu, Yue Qin, Baojun Liu

- **方向**: AI/ML安全

- **解读**: 中文地下网络（诈骗、赌博、黑产）使用对抗性隐语（黑话）规避内容审核：人工构造的扰动破坏可读性与分词，比算法攻击更难检测。学术数据集与真实网络环境的差距使检测模型陷入"象牙塔"，亟需基于真实世界数据的大规模评测与增强方法。

作者与行业合作构建大规模真实中文对抗性隐语数据集，刻画其扰动强度与形式多样性；提出基于扰动模式分类学的 LLM 检测/还原框架 JADE，采用现实对抗学习课程（外部知识检索 + 内部自适应），覆盖隐语检测、还原（restoration）与非法内容识别三类任务。

评测显示即便 GPT-4o 等顶级模型也仅达到约 88.16% 的隐语检测准确率、62.74% 的还原准确率与 76.05% 的非法内容检测；JADE 分别提升至 98.59%、95.91% 与 97.99%，优于商业与微调开源 LLM，并在毒性内容、诈骗检测等跨域基准上表现出良好泛化。

我的思考：核心贡献是"走出象牙塔"：真实数据 + 扰动分类学 + 知识增强 LLM 的组合直接对接平台内容安全运营，落地价值高。局限在于数据来自单一合作渠道，隐语类型与平台分布的覆盖可能偏差；LLM 方案的推理成本与延迟在超大规模审核流中的可行性待评估；对抗性隐语动态演化，需持续更新。与基于规则与小模型的检测相比，LLM 的理解力显著提升但开销更高。


#### Beyond Indistinguishability: Measuring Extraction Risk in LLM APIs.
- **作者**: Ruixuan Liu, David Evans, Li Xiong
- **方向**: AI/ML安全
- **解读**: 

**问题**：不可区分性（如差分隐私界或低成员推断风险）被广泛用作模型免受记忆化风险（数据提取）的代理指标。论文证明这种代理关系不成立：可区分性与可提取性是两类独立隐私博弈，上界约束区分能力并不能上界约束提取能力（定理2、3），用DP/MIA审计可能产生虚假安全感。

**方法**：提出(l,b)-不可提取性定义——任何黑盒攻击者需至少2^b次期望查询才能诱导LLM API输出受保护的l-gram子串；基于最坏情形提取博弈推导基于秩（rank）的提取风险上界1-(1-∏1/rt)^n，其中rt是ground-truth token的概率排名；该估计只需一次teacher-forcing前向传播（滑动窗口扫描），并扩展覆盖非定向与近似提取，统一了贪心与概率式测量的估计。

**结果**：经验评估（GPT-2/Enron微调、LLaMA-3.1-8B/Pile预训练）显示：MIA信号与提取成本的相关性取决于信号是否校准（未校准的Loss/MinK相关更强）；低复杂度/低多样性文本与高唯一性文本是两类高危提取模式，激进去重反而可能提升最坏情形可提取性；超过99%的受保护子串提取成本低于"重复我"上下文基线，说明确属训练数据记忆；更大模型提取成本更低。

**我的思考**：该工作把提取风险从"结果二分"升级为"成本度量"，与密码学安全级别（2^b）类比，为LLM API部署提供训练、访问控制与解码配置层面的可操作缓解指南，理论分离证明（DPD不蕴含DE）澄清了长期混淆。局限是上界假设攻击者已知完整文本（discoverable设定）、按teacher-forcing固定秩，对自适应查询的真实风险仍是保守估计；实验基于开源小模型，真实API的top-m概率访问限制（m不超过20）下秩估计的精度与代价权衡值得进一步研究。
- **arXiv PDF**: [Beyond Indistinguishability: Measuring Extraction Risk in LLM APIs.](https://arxiv.org/pdf/2604.18697)

#### ARES: Scalable and Practical Gradient Inversion Attack in Federated Learning Through Activation Recovery.

- **作者**: Zirui Gong, Leo Yu Zhang, Yanjun Zhang, Viet Vo, Tianqing Zhu, Shirui Pan, Cong Wang

- **方向**: AI/ML安全

- **解读**: 联邦学习通过共享梯度而非原始数据保护隐私，但梯度反演攻击（GIA）可从中重建训练样本；其中主动型 GIA 在大批量下也能高保真重建，却普遍要求修改模型架构，严重限制实际可行性。需要一种在真实联邦设置、大批量下无需架构修改即可扩展的实用攻击。

ARES 把重建问题建模为含噪稀疏恢复任务，用广义 Lasso 求解；为扩展到多样本重建，引入 imprint 方法解耦各样本的激活，实现逐样本可扩展恢复；并给出期望恢复率与重建误差上界的理论保证，使攻击具有可量化的理论支撑。

在 CNN/MLP 与多数据集上，ARES 大幅领先基线：如 CIFAR-10 在 batch 32/64/256 下 PSNR 达 92.27/90.47/37.08，而多数基线在大批量下完全失效（batch 256 时失败或仅约 12-15）；安全聚合防御下，全局 batch 320/640 平均 PSNR 达 53.7/51.9，比 LOKI 提升 7.3×/7.1×；即便先验少于需要训练解码器的 Scale-MIA，平均 PSNR 仍高出 2.71×。

我的思考：该工作揭示中间激活是被严重低估的联邦学习隐私风险，且"无需架构修改"使威胁模型更贴近现实攻击者，理论界与恢复率分析提升了贡献成色。局限：重建质量随 batch 增大明显下滑（256 时 PSNR 降至 33-42），主要针对全连接层相关场景；对梯度裁剪、强 DP 等防御的鲁棒性仍有限。它为联邦学习防御设计提供了明确的紧迫依据。

- **arXiv PDF**: [ARES: Scalable and Practical Gradient Inversion Attack in Federated Learning Through Activation Recovery.](https://arxiv.org/pdf/2603.17623)

#### On the Detectability of Active Gradient Inversion Attacks in Federated Learning.

- **作者**: Vincenzo Carletti, Pasquale Foggia, Carlo Mazzocca, Giuseppe Parrella, Mario Vento
- **方向**: AI/ML安全
- **解读**:
联邦学习（FL）的核心优势是客户端数据留在本地、协同训练模型，但这可能造成虚假的安全感：此前研究表明，训练中交换的梯度仍易受梯度反演攻击（GIA）威胁，可重构本地数据，打破 FL 的隐私承诺。

GIA 可由被动服务器或主动恶意服务器发起：后者操纵全局模型以促进重构。早期主动攻击已被证明可被客户端检测，限制了现实可用性；近期出现自称更隐蔽的新型攻击。论文首次全面分析这些声明，研究四种最先进的 GIA，并剖析其可检测性。

作者提出轻量级客户端侧检测技术，基于统计上不可能的权重结构与异常的损失梯度动态；跨多种配置的广泛评估表明，客户端无需修改协议即可有效检测这些攻击。

该工作把“可检测性”作为主动反演攻击的评估维度，系统检验“更隐蔽”声明的成色；统计异常检测切入、无需协议改动即可部署，实用性强。局限在于检测阈值与误报率的调优，以及攻击者自适应规避的潜在可能；对 FL 生态的隐私-安全平衡具有直接指导意义。


#### Towards Automating Data Access Permissions in AI Agents.
- **作者**: Yuhao Wu, Ke Yang, Franziska Roesner, Tadayoshi Kohno, Ning Zhang, Umar Iqbal
- **方向**: AI/ML安全
- **解读**: AI 智能体代表用户自主执行任务时，用户需要对其数据访问保持透明与控制。作者论证基于权限的访问控制不可或缺，但传统权限模型（静态、一次性授权）无法适应智能体连续、情境化的自主执行范式，因此提出由"管理智能体"（management agents）辅助的权限自动化思路。

方法分两步：先开展用户研究，识别影响用户数据访问授权决策的因素（如通信情境、任务上下文）；再将这些因素编码为基于机器学习的助手，用于预测用户在未来类似情境下会做出的授权决定。研究发现参与者的授权决策受通信情境影响，且个人偏好在同一情境内保持稳定、并与其他参与者趋于一致。

基于这些洞察构建的预测模型整体准确率达 85.1%，高置信预测达 94.4%；即使不使用历史记录，准确率仍有 66.9%，而仅 1–4 个训练样本即可带来 10.8% 的显著提升。

我的思考：把"授权决策可预测性"作为自动化权限管理的基础，是智能体权限问题上一个务实且有证据支撑的切入点——若用户授权行为在情境内高度一致，AI 助手就能低风险地代为决策。局限在于 85.1% 的准确率意味着仍有约 15% 的误授权风险，且"情境一致性"结论能否推广到更复杂、更敏感的授权场景存疑；与"运行时权限询问"类方案的对比（何时该问、何时自动放行）值得进一步研究。


#### MUSICSHIELD: Protection for Musicians in the Era of Generative AI.
- **作者**: Syed Irfan Ali Meerza, Jian Liu
- **方向**: AI/ML安全
- **解读**: 生成式 AI 可轻易克隆歌手音色、模仿演奏风格并生成仿冒作品，使音乐人的声音与作品被未经授权使用，带来身份冒用、侵权与生计威胁；现有版权与技术手段难以系统保护音乐人。（无摘要，据标题推断）

据标题推断，MUSICSHIELD 为音乐人提供防护机制：可能结合声音水印、对抗扰动（破坏克隆模型对音色的学习）与内容指纹，在发布音频时嵌入防护信号，使 AI 克隆工具难以生成可信仿冒或使仿冒可被溯源。

摘要缺失，无法引用量化结果；预期包含对克隆攻击防护成功率、音频质量影响与绕过鲁棒性的评估。

我的思考：生成式 AI 对创作者权益的冲击是当下最现实的社会技术矛盾之一，从技术侧为音乐人提供"盾牌"是对版权制度滞后性的补充。与图像域的"对抗样本保护艺术家"（如 Glaze、Nightshade）一脉相承，音频域的挑战在于防护信号需在人耳不可感知的同时保持对多种克隆管线的有效性；无摘要使具体机制与抗绕过能力待正文确认。


#### Promoguardian: Detecting Promotion Abuse Fraud with Multi-Relation Fused Graph Neural Networks.

- **作者**: Shaofei Li, Xiao Han, Ziqi Zhang, Minyao Hua, Shuli Gao, Zhenkai Liang, Yao Guo, Xiangqun Chen, Ding Li

- **方向**: AI/ML安全

- **解读**: 促销滥用（薅羊毛）是电商增长最快的欺诈类型之一：用户利用促销规则套取平台资金；与刷单等传统欺诈不同，它常由进行正常交易的普通用户参与，与合法交易深度交织，难以检测。

作者首次对美团（MEITUAN）促销滥用开展系统性研究，发现团购类活动中的两种滥用模式（囤货返利滥用与现金返还滥用）；提出 PROMOGUARDIAN——多关系融合图神经网络，将交易数据融合进同质图，并利用空间与时间维度信息检测欺诈。

在美团真实数据上的大量实验表明，模型优于现有最先进方法：检测精度达 93.15%，识别的欺诈者数量为基线方法的 2.1–5.0 倍，在生产环境中挽回的损失为对比方法的 1.5–8.8 倍。

我的思考：把"合法用户 + 合法交易"交织场景建模为多关系图并融合时空信息，抓住了促销滥用的本质，工业落地数据有说服力；局限在于对平台特定活动的依赖、新活动模式的迁移性，以及 GNN 在超大规模图上的工程成本与可解释性。


#### Weaponizing Reflectivity for Pointcloud Deception with Forged Invisible Geometries.

- **作者**: Hengwei Chen, Menglan Hu, Tianyue Zheng
- **方向**: AI/ML安全
- **解读**:
自动驾驶依赖 LiDAR 点云感知进行目标检测与追踪，既有针对点云感知的对抗攻击要么在物理世界部署实体物体（成本高、可发现），要么直接向传感器注入激光（需贴近目标）。高反射材料（镜面、反光板、幕墙）为攻击提供了第三条路径：通过反射机制在点云中产生"伪造的不可见几何体"——攻击者无需在场景中放置真实物体，即可注入幽灵形状欺骗感知模型。（无摘要，据标题推断）

据标题推断，论文系统研究反射特性如何被武器化：刻画镜面/多次反射下 LiDAR 点云的畸变规律（虚假点的位置、密度与分布），构造可稳定欺骗目标检测/追踪的伪造几何体，并在真实或高保真仿真道路场景中验证攻击对感知模型的影响。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文报告点云欺骗的成功率、对主流检测模型的攻击效果，以及与实体物体攻击、激光注入攻击的对比。（无摘要，据标题推断）

我的思考：反射欺骗利用的是 LiDAR 的物理原理而非模型漏洞，因此可能跨传感器型号与检测架构通用，且攻击者可在远处布置反射面、隐蔽性更强，对自动驾驶安全验证是重要补充。局限在于反射面的部署可行性、天气与距离下的鲁棒性、以及多传感器融合（相机/雷达）对欺骗的抑制效果，均需全文确认。（无摘要，据标题推断）


### 大模型安全（17 篇）

#### LLM Unlearning Should Be Form-Independent.
- **作者**: Xiaotian Ye, Mengqi Zhang, Shu Wu
- **方向**: 大模型安全
- **解读**: 大语言模型遗忘（unlearning）旨在擦除或压制模型中的有害、私密知识，但近期研究显示其在现实场景中有效性有限，阻碍实际采用。本文识别出众多下游失败的普遍症结：现有方法的有效性严重依赖训练样本的表达形式，难以泛化到同一知识的其他表述，称为"形式依赖偏差"（Form-Dependent Bias）。

作者形式化定义形式依赖偏差，系统考察其在多种任务中的具体表现模式；为量化普遍性并支撑后续研究，提出 ORT 基准，评测遗忘方法对知识表述变化的鲁棒性。针对该问题提出免训练的 Rank-one Concept Redirection（ROCR）：以任务不变式（被激活的危险概念）为靶点做秩一重定向，在数秒内修改模型参数，把模型对目标概念的感知重定向至另一个无害概念。

评测结果显示形式依赖偏差在现有遗忘技术中既普遍又严重；ROCR 相比传统方法显著提升遗忘在不同表述下的泛化效果，同时生成输出高度自然。

点破"遗忘只对训练时形式有效"这一要害，形式化定义并配套 ORT 基准，为领域提供可复用的测量工具；ROCR 免训练、秒级完成，实用性强。局限：秩一重定向对复杂、多概念纠缠知识的遗忘边界待考察，"重定向至无害概念"在个别场景可能引入新行为；但把遗忘目标从"样本"提升到"知识形式不变式"，方向值得肯定。


#### Sok: Evaluating Jailbreak Guardrails for Large Language Models.
- **作者**: Xunguang Wang, Zhenlan Ji, Wenxuan Wang, Zongjie Li, Daoyuan Wu, Shuai Wang
- **方向**: 大模型安全
- **解读**: 大语言模型（LLM）部署暴露了易受越狱攻击的脆弱性，guardrails——监控并控制 LLM 交互的外部防御机制——成为有前景的解决方案。然而当前 guardrail 研究碎片化：缺乏统一分类法与综合评测框架，实践者难以比较和选型。该 SoK 论文给出 LLM 越狱防御机制的首次整体分析。

论文提出新颖的多维分类法，沿六个关键维度对现有 guardrail 归类；引入 Security-Efficiency-Utility（安全-效率-效用）框架评估实际有效性；通过大量实验识别各类方法的优点与局限，为机制优化提供洞见，并考察其在不同攻击类型间的普适性。代码已开源。

实验系统性地揭示了现有 guardrail 方法的能力边界与失效模式，为后续优化指明方向；六维分类与三维评测框架构成结构化基础，可支撑领域内研究的有序推进。

SoK 填补了 guardrail 领域"无统一坐标系"的空白，其分类与评测框架有望成为后续工作的共同基线。局限：guardrail 技术演化极快，评测语料与攻击集合的时效性决定结论半衰期；安全、效率、效用三者的权衡如何量化是难点。总体而言，这是实践者选型与研究定位的高价值地图；后续工作可针对短板维度做定向突破，并随攻击演化持续更新评测基准。

- **arXiv PDF**: [Sok: Evaluating Jailbreak Guardrails for Large Language Models.](https://arxiv.org/pdf/2506.10597)

#### URLcoat: Exploiting Web Search Capability to Jailbreak Large Language Models.
- **作者**: Yiheng Sun, Linkang Du, Zhou Su, Yuntao Wang, Han Liu
- **方向**: 大模型安全
- **解读**: 联网大语言模型（具备网页搜索能力）正成为主流，但搜索能力同时开辟了新的越狱面：模型会检索并采纳网页内容，攻击者可利用这一机制诱导模型输出有害内容。URLcoat 研究如何利用 LLM 的网页搜索能力实施越狱攻击。

据标题推断，攻击者构造或托管恶意网页，或利用搜索结果中的对抗性内容，使 LLM 在回答用户问题时检索并引用这些内容，从而绕过安全对齐输出有害信息；论文进一步分析“何时模型会采纳检索内容”的触发条件，并跨多个模型与搜索配置评估攻击有效性，探究 URL 构造、内容布局等影响因素。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以越狱成功率以及"模型何时会采纳检索内容"的触发条件分析作为证据。

联网搜索把外部不可信内容引入推理上下文，等于系统性扩大注入面；此类攻击与提示注入同源，却以检索为入口，难以被输入过滤拦截，对搜索增强型 LLM 是结构性威胁。局限：攻击可控性依赖模型对检索结果的信任度与搜索排名；防御上应引入检索内容可信分级与来源验证。该方向值得持续关注。


#### MetaBreak: Jailbreaking Online LLM Services via Special Token Manipulation.
- **作者**: Wentian Zhu, Zhen Xiang, Wei Niu, Le Guan
- **方向**: 大模型安全
- **解读**: 特殊 token（special tokens）是 LLM 微调中人为创建、用于标注结构化对话的元数据，在指导模型生成连贯、上下文感知的响应中起关键作用。论文证明特殊 token 可被利用构造四种攻击原语，使恶意用户可靠地同时绕过在线 LLM 服务的内部安全对齐与 SOTA 外部内容审核系统。

四种攻击原语基于特殊 token 的元数据语义构造。防御困难是重要发现：学界建议的激进输入清洗（如直接删除全部特殊 token）效果远不及预期，因为被移除的 token 可被替换为 tokenizer 嵌入空间中语义高度相似的 token 来规避过滤。作者将该方法命名为 MetaBreak，并在实验室环境与商业平台上系统评估。

无审核时，MetaBreak 的越狱率与 SOTA 提示工程方案相当；部署外部审核时，其越狱率分别比 PAP 高 11.6%、比 GPTFuzzer 高 34.8%。由于与提示工程采用根本不同的策略，二者可协同工作：结合 MetaBreak 后，两者成功率分别再提升 24.3% 与 20.2%。

揭示"元数据 token 即攻击面"这一新颖向量，四种原语加嵌入空间替换的绕过分析完整，且与提示工程攻击的协同性证明组合威胁真实存在。局限：商业平台覆盖面与跨模型泛化性待更多披露；对嵌入空间相似 token 的检测等防御尚待研究。该工作说明输入清洗类防御存在结构性盲区。

- **arXiv PDF**: [MetaBreak: Jailbreaking Online LLM Services via Special Token Manipulation.](https://arxiv.org/pdf/2510.10271)

#### SoK: Robustness in Large Language Models against Jailbreak Attacks.
- **作者**: Feiyue Xu, Hongsheng Hu, Chaoxiang He, Sheng Hang, Hanqing Hu, Xiuming Liu, Yubo Zhao, Zhengyan Zhou, Bin Benjamin Zhu, Shifeng Sun, Dawu Gu, Shuo Wang
- **方向**: 大模型安全
- **解读**: 

**问题**：大语言模型（LLM）易受越狱（jailbreak）攻击，对抗性提示可诱导模型输出有害或违反政策的内容，在关键应用中威胁安全、信任与合规。然而现有评估实践碎片化，往往只用攻击成功率（ASR）单一指标，无法刻画LLM安全的多维特性，甚至产生误导性结论。

**方法**：论文提出系统化的越狱攻击与防御分类体系，并引入Security Cube多维评估框架，整合7项攻击指标（成功率、稳定性、可迁移性、集中度、破坏深度、开销等）、3项防御指标与裁判（judge）评估指标，其中攻击稳定性、CIPA集中度指数与破坏深度为首次提出的正式度量。基于该框架对13种代表性攻击、5种防御和4种自动裁判进行基准评测。

**结果**：评估显示2025年后模型鲁棒性大幅跃升——o1-mini与Claude-3.7-Sonnet平均ASR约17%，而GPT-3.5-Turbo、Mistral等旧模型在自适应攻击下ASR超90%；ReNeLLM（66.6%）、ActorBreaker（61.35%）、LLM-Adaptive（57.65%）为最有效攻击；Hidden State Guard可将9/11种攻击的成功率降至近零；多维评估结论常与仅看ASR相矛盾。

**我的思考**：该SoK的成色在于将稳定性、可迁移性、集中度、破坏深度等维度形式化，纠正了ASR单一指标的认知偏差，其"攻击有效性、泛化性与开销不可兼得"及"生成先验固有脆弱性"的观察具有前瞻性。局限是评测集中于提示级攻击，对多模态、推理型模型覆盖有限；指标多基于特定数据集与模拟，自动裁判与人类判断的一致性也需更大规模验证。

- **arXiv PDF**: [SoK: Robustness in Large Language Models against Jailbreak Attacks.](https://arxiv.org/pdf/2605.05058)

#### Evaluating Concept Filtering Defenses against Child Sexual Abuse Material Generation by Text-to-Image Models.
- **作者**: Ana-Maria Cretu, Klim Kireev, Amro Abdalla, Wisdom Obinna, Raphael Meier, Sarah Adel Bargal, Elissa M. Redmiles, Carmela Troncoso
- **方向**: 大模型安全
- **解读**: 
AI 生成的儿童性虐待材料（AIG-CSAM）是文本到图像（T2I）模型的严重滥用问题，"从训练数据过滤儿童图像"被视为"黄金标准"防御。本文首次评估其有效性：以博弈式安全定义刻画生成难度，基准测试 20 余种儿童检测方法，并用伦理代理（戴眼镜的儿童）测量过滤后的对抗难度。

方法上，作者形式化概念过滤，定义安全博弈与查询数度量 Q_α。检测基准覆盖图像模态（MiVOLO、Amazon Rekognition 等年龄估计器、LLaVA-7B）与文本模态（同义词匹配、DeepSeek-V3）；随后在 CC3M 与 LAION-Face 上从零训练 Stable Diffusion 1.x 的过滤/未过滤模型，对启发式提示（900 条）、LLM 优化对抗提示、LoRA 微调（1000 张儿童图）与 DreamBooth 个性化（8 张图）做人类评估。

结果显示：最佳检测 TPR 仅 93.9%（FPR 35.0%），十亿级数据集仍残留数百万儿童图像；过滤后直接误用仅需 Q=12（CC3M）/9（LAION-Face）次查询，对抗提示降至 Q=9/7；过滤模型生成的儿童显著更老（+7.89 岁与 +7.26 岁），但微调后年龄差缩至 3 年内，个性化不受影响；即使完美过滤，LoRA 微调 200 张图即可重新引入概念（Sprigatito 实验 87/100、85/100 成功）。过滤还损害模型通用性（playground、woman 等提示生成受阻）。

我的思考：本文方法论贡献突出——博弈定义、伦理代理与人类评估的结合为后续评测树立可复现范式。结论对产业是沉重打击：过滤只对封闭权重模型提供有限保护、对开放权重模型无保护，且完美检测不现实（十亿数据集顺序标注需 10-476 年）。局限是 CWG 与 CSAM 语义强度可能有差、训练规模远小于产业模型。真正值得追问的是：若数据过滤注定失效，架构级防护与平台治理的组合才更现实。

- **arXiv PDF**: [Evaluating Concept Filtering Defenses against Child Sexual Abuse Material Generation by Text-to-Image Models.](https://arxiv.org/pdf/2512.05707)

#### Hijacking Large Audio-Language Models via Context-Agnostic and Imperceptible Auditory Prompt Injection.
- **作者**: Meng Chen, Kun Wang, Li Lu, Jiaheng Zhang, Tianwei Zhang
- **方向**: 大模型安全
- **解读**: 

**问题**：大型音频语言模型（LALM）紧密整合音频与文本，支撑智能语音交互，但连续高维音频通道引入了超出文本的攻击面。此前研究聚焦音频越狱，而第三方恶意音频注入（间接提示注入）的威胁被忽视——攻击者只能接触音频数据、且需强感知隐蔽。论文提出AudioHijack框架，系统化探索这一威胁。

**方法**：AudioHijack采用输出级注入策略，将恶意指令嵌入对抗音频以劫持模型行为。针对离散音频token化不可微导致的梯度阻断，用Gumbel-Softmax采样估计梯度实现端到端优化；针对未知用户上下文，通过注意力监督与多上下文训练把模型注意力引向对抗音频；并设计卷积扰动混合，用可学习类混响核将扰动能量重分布到时频域，使对抗音频如同自然混响。

**结果**：在13个SOTA LALM（离散/连续/混合三种架构）上，6类错误行为（听觉失明、拒绝、虚假信息、钓鱼、人格控制、工具滥用）在未见上下文上平均PISR 0.89-0.95、BMSR 0.79-0.96，仅SpeechGPT因音频token词表仅1000而受限（0.23-0.57）；卷积混合比加法扰动SNR提升2.9-7.1dB、PESQ提升0.9。真实世界中Mistral AI与Microsoft Azure商用语音代理可被诱导执行未授权操作，如发送敏感搜索、下载恶意文件、邮件外泄用户信息。

**我的思考**：该工作首次系统验证了LALM间接提示注入威胁，输出级注入绕开模态鸿沟，卷积混响思路在隐蔽性与有效性间取得较好平衡，是高质量攻击研究。局限是依赖白盒访问（以开源模型代理攻击商业API），跨模型迁移（VS-latest）时成功率明显下降；防御部分仅给出初步检测思路，如何在不损害语音交互体验的前提下检测此类注入仍是开放问题。

- **arXiv PDF**: [Hijacking Large Audio-Language Models via Context-Agnostic and Imperceptible Auditory Prompt Injection.](https://arxiv.org/pdf/2604.14604)

#### LLMThief: Evaluating Configuration Leaking Risks in Commercial LLM App Stores.
- **作者**: Pinji Chen, Jinlong Jiang, Jianjun Chen, Feiran Qin, Minghao Zhang, Jiahe Zhang, Haixin Duan, Kaiwen Shen, Hui Jiang
- **方向**: 大模型安全
- **解读**: 

**问题**：LLM 应用商店（如 GPT Store、字节 Coze、百度文心等）承载大量第三方 LLM 应用，其敏感配置——系统提示词、外部 API、知识文件——可通过提示注入/越狱等被窃取，威胁开发者知识产权、用户隐私与企业机密。此前缺乏从商店层面系统评估配置泄露风险的工作。

**方法**：LLMThief 是首个端到端配置泄露评估框架，分三阶段：①从商店级特性提取基础信息，构建攻击提示种子池；②用影子 LLM 应用与遗传算法探索自适应的提示变异策略；③用洞察性提示对目标应用模糊测试，并以微调模型判定是否发生泄露。

**结果**：在六个主流 LLM 应用商店（OpenAI GPT Store、字节 Coze、百度文心等）上完成 ground-truth 测试，有效泄露机密配置且优于基线；大规模分析 4,164 个真实 LLM 应用，发现系统提示词泄露、外部 API 暴露、知识文件泄露等关键风险；向受影响厂商负责任披露，获百度、字节、阿里巴巴等厂商的漏洞奖励。

**我的思考**：首次从商店层面系统化刻画配置泄露面，遗传算法驱动的自适应变异使攻击提示能跟上应用迭代，4,164 个应用的大规模测量使结论有代表性。局限在于泄露判定模型本身可能误报/漏报、商店防御升级后方法需持续适配。该工作凸显 LLM 应用商店作为新兴供应链节点的治理需求，对平台准入审查与开发者配置保护有直接参考价值。


#### Toward Efficient Membership Inference Attacks Against Federated Large Language Models: A Projection Residual Approach.

- **作者**: Guilin Deng, Silong Chen, Yuchuan Luo, Yi Liu, Songlei Wang, Zhiping Cai, Lin Liu, Xiaohua Jia, Shaojing Fu

- **方向**: 大模型安全

- **解读**: 联邦大语言模型（FedLLM）让多方在不共享原始数据的情况下协作微调 LLM，但其共享梯度仍可能泄露敏感信息——成员推理攻击（MIA）可判定某样本是否在训练集中。然而 FedLLM 参数规模巨大、收敛迅速、梯度稀疏且非正交，导致既有 MIA 失效，成员隐私暴露缺乏有效评估手段。

论文提出 ProjRes，首个面向 FedLLM 的基于投影残差的被动 MIA：以隐藏嵌入向量作为样本表示，分析其在梯度子空间上的投影残差，揭示梯度与输入之间的内在关联；攻击不需要影子模型、辅助分类器或历史更新，保证了效率与鲁棒性。

在四个基准与四个 LLM 上，ProjRes 达到接近 100% 的攻击准确率，比既有方法最多高出 75.75 个百分点；即使在强差分隐私防御下仍保持有效。

我的思考：该工作揭示了一个此前被忽视的 FedLLM 隐私漏洞，结果有力，且"无影子模型、无历史更新"的被动设计使攻击轻量实用，对安全假设提出了严肃质疑。局限：攻击依赖恶意服务器或梯度拦截以获取嵌入与梯度，威胁模型需明确；DP 下仍有效说明梯度中残余的成员信息很强，也为更强防御的设计提出挑战。对联邦微调生态的安全重审具有直接推动作用。

- **arXiv PDF**: [Toward Efficient Membership Inference Attacks Against Federated Large Language Models: A Projection Residual Approach.](https://arxiv.org/pdf/2604.21197)

#### ENCHTABLE: Unified Safety Alignment Transfer in Fine-Tuned Large Language Models.
- **作者**: Jialin Wu, Kecen Li, Zhicong Huang, Xinfeng Li, XiaoFeng Wang, Cheng Hong
- **方向**: 大模型安全
- **解读**: 大模型常被微调到代码生成、生物医学、数学等垂直领域，但微调过程会系统性削弱模型的安全对齐，增加有害输出风险；如何在"不为每个下游模型重新训练"的前提下把对齐能力迁移并保持到微调模型上，是安全与实用的双重挑战。

方法上，EnchTable 提出统一的安全对齐迁移框架：采用基于神经正切核（NTK）的向量蒸馏，把安全约束与任务特定推理解耦，从而兼容不同架构与规模的模型；再以干扰感知的合并（interference-aware merging）技术平衡各任务域效用，原型在三种不同 LLM 架构上实现。

实验覆盖十一个数据集并纳入供应商模型对比，验证其泛化能力：对静态与动态越狱攻击表现出稳健抵抗，优于厂商发布的缓解措施；与六种参数修改方法及两种推理时基线相比，不安全率更低、得分更高，且可无缝接入部署管线、开销小。

我的思考：该工作的成色在于"零重训迁移对齐"——用蒸馏加合并把对齐从安全模型"拷贝"到微调模型，工程价值直接。但摘要文字缺损，具体数字（不安全率、得分、开销）无法在此引用；NTK 蒸馏的理论依据与对超大规模模型的适用性、合并对任务效用的长期影响仍需正文验证，与 LoRA 微调后对齐修复路线的对比也值得期待。


#### Who Taught the Lie? Responsibility Attribution for Poisoned Knowledge in Retrieval-Augmented Generation.

- **作者**: Baolei Zhang, Haoran Xin, Yuxi Chen, Zhuqing Liu, Biao Yi, Tong Li, Lihai Nie, Zheli Liu, Minghong Fang

- **方向**: 大模型安全

- **解读**: 检索增强生成（RAG）将外部知识库接入大模型以提升回答质量，但知识库投毒（插入恶意文本）可操纵模型输出；已有防御常被更强攻击绕过，且缺乏定位"哪段文本导致错误输出"的手段，事后追责与清理困难。

本文提出 RAGOrigin——黑盒责任归属框架：为每个错误生成事件构造聚焦范围（focused scope），从检索排名、语义相关性、对生成结果的影响三个维度为候选文本打分，再用无监督聚类隔离被投毒文本。

在 7 个数据集上评估，涵盖 15 种设置（含新设计的攻击策略与多攻击者场景），方法优于现有基线，且在动态、带噪声条件下保持鲁棒；代码已开源。

我的思考：把 RAG 安全从"检测攻击"推进到"定位毒源"，支撑事后取证、知识库清洗与责任划分，实用价值明确；黑盒设计使其不依赖模型内部结构；局限在于评分维度的完备性、多跳污染的归因精度，以及聚类在极端噪声下的稳定性。


#### When AI Meets the Web: Prompt Injection Risks in Third-Party AI Chatbot Plugins.
- **作者**: Yigitcan Kaya, Anton Landerer, Stijn Pletinckx, Michelle Zimmermann, Christopher Kruegel, Giovanni Vigna
- **方向**: 大模型安全
- **解读**: 提示注入研究多聚焦个人 copilot 等前沿应用，但遍布网络的简单 LLM 应用——由第三方插件驱动的客服聊天机器人——安全态势不明；这些插件作为网站与商业 LLM API 之间的中介，被非专业建站者大量采用（2025 年生态扩张近 50%）。本文首次大规模研究 17 个插件与超 10,000 个网站。

方法上，作者从插件与配置两个层面刻画：发现 8 个插件（服务于 8,000 个网站）不校验浏览器到 LLM 请求中对话历史的完整性，攻击者可伪造含 system 消息在内的历史；15 个插件提供网页抓取工具构建 RAG 知识库，却不区分可信与不可信（第三方）内容；再测量真实 system prompt 与已激活工具，并将观测实践转为受控实验量化攻击成功率。

结果显示，伪造对话历史使直接注入诱导非预期行为的能力提升 3–8 倍；约 13% 的电商网站已把聊天机器人暴露给第三方内容，构成间接注入风险；角色边界被破坏时，注入触发未授权任务的成功率达 25–100%，正确隔离时仅 0–25%；加固的 system prompt 能阻止任务劫持，但对工具参数劫持防护有限。披露后最广泛使用的插件已修复，多数仍脆弱。

我的思考：本文首次把提示注入研究从"前沿应用"延伸到"长尾生态"，核心发现是插件层系统性破坏了 LLM 提供方内置的指令层级防御，测量数据（3–8 倍、25–100%）均来自受控实验，扎实可信。局限在于研究为 2025 年生态快照，插件更新频繁使时效性有限，也未覆盖恶意插件本身的供应链风险。与指令层级研究相互印证：再强的模型级防御也挡不住应用层的角色边界破坏。

- **arXiv PDF**: [When AI Meets the Web: Prompt Injection Risks in Third-Party AI Chatbot Plugins.](https://arxiv.org/pdf/2511.05797)

#### PromptLocate: Localizing Prompt Injection Attacks.

- **作者**: Yuqi Jia, Yupei Liu, Zedian Shao, Jinyuan Jia, Neil Zhenqiang Gong

- **方向**: 大模型安全

- **解读**: 提示注入攻击通过污染输入数据（含指令与数据）让 LLM 执行攻击者指定任务；事后取证与系统恢复需要从被污染输入中定位注入的提示，但该问题此前几乎未被研究。

PromptLocate 是首个提示定位方法，包含三步：(1) 将输入切分为语义连贯的片段；(2) 识别其中指令类片段；(3) 精确定位注入的提示；整体流程完全黑盒，便于在真实系统部署。

实验表明，在 8 种现有自适应攻击上均能准确定位注入提示，验证了方法的通用性与鲁棒性。

我的思考：定位是"检测→防御"之外的新能力维度，支撑事后取证、日志审计与数据清理；"识别指令意图"的思路本身可复用为注入检测特征，其分段与指令识别思想也可迁移到输入清理与内容过滤等防御环节；局限在于对无指令痕迹的数据型注入、多轮对话与复杂提示工程的鲁棒性仍需检验。


#### PromptCOS: Towards Content-Only System Prompt Copyright Auditing for LLMs.
- **作者**: Yuchen Yang, Yiming Li, Hongwei Yao, Enhao Huang, Shuo Shao, Yuyi Wang, Zhibo Wang, Dacheng Tao, Zhan Qin
- **方向**: 大模型安全
- **解读**: 

**问题**：系统提示词决定 LLM 应用行为与质量，已成为高价值知识产权，但窃取泄露事件频发（Bing、GPT-5 均被报道，OWASP 列为十大风险）。现有水印依赖验证 logit 分布偏移，在真实"仅内容"场景下不可行：防御者只能拿到生成文本，且随机采样使内容级生成不稳定、加强指令又损害保真度。

**方法**：PromptCOS 是首个基于内容级输出相似度的提示词版权审计方法，三个核心设计：循环输出信号（"[标记][pad]"循环序列，稳定生成）；辅助 token（仅注入约 5 个编码水印，主提示词不动）；封面 token（优化原提示词关键 token，使删除辅助 token 时性能严重下降）。用交替优化与贪心梯度搜索联合优化提示词、验证查询与信号标记，验证时以滑动窗口字符相似度判定侵权。

**结果**：在 Llama/Gemma/Deepseek 五模型、三任务上，PromptCOS 平均水印相似度 99.3%，区分度（MDWS 均值 0.74）比最优基线高 60.8%，甚至超过需 logits 的 PromptCare；精度下降不超 0.58%（常为负值，水印反略提升性能）；对冗余注入、约束、重优化、辅助 token 删除四类攻击均稳健；嵌入成本最高节省 98.1%。

**我的思考**：贡献在于把版权审计从"可访问 logits"推进到"只见文本"的现实场景，循环信号与封面 token 组合设计巧妙且可落地。局限：假设防御者可访问目标 LLM 参数（仅开源模型成立），验证仍具概率性，面对彻底重写或蒸馏等强对抗手段存疑；水印不可读正契合自动生成提示词非人类可读的特点。相比 PromptCare，内容级框架更贴近部署现实。

- **arXiv PDF**: [PromptCOS: Towards Content-Only System Prompt Copyright Auditing for LLMs.](https://arxiv.org/pdf/2509.03117)

#### AttnTrace: Contextual Attribution of Prompt Injection and Knowledge Corruption.
- **作者**: Yanting Wang, Runpeng Geng, Ying Chen, Jinyuan Jia
- **方向**: 大模型安全
- **解读**: 

**问题**：长上下文 LLM 支撑的 RAG 与智能体易受提示注入与知识投毒攻击，恶意文本混入上下文即可操控输出。事后取证需回答：哪些文本导致恶意输出？现有方法（Shapley、TracLLM 等）要么精度不足，要么每样本数百秒。Nikkei 调查显示 14 所大学的研究者将隐藏提示注入论文以操纵 LLM 评审，凸显现实紧迫性。

**方法**：AttnTrace 直接利用 LLM 产生的注意力权重衡量各文本对输出的贡献，两项关键技术：top-K token 平均——只对文本中注意力最高的 K 个 token（默认 5）取均值，过滤注意力汇聚（attention sink）噪声；上下文子采样——随机抽取部分文本多次（采样率 0.4、30 次）求平均，缓解多恶意文本并存时的注意力分散，并给出理论分析支撑。

**结果**：HotpotQA 知识投毒下精确率/召回率 0.95/0.95，TracLLM 仅 0.80/0.80；MuSiQue 提示注入下 21.7 秒取得 0.99/0.81，TracLLM 需 403.7 秒，快约 15–20 倍。对 15 种攻击均有效；"先归因后检测"将 DataSentinel 的 FPR 从 1.0 降至 0.06、AttentionTracker 的 AUC 从 0.70 提升至 0.95；真实案例 36.2 秒从 18350 词论文中定位注入指令，并能抵御 nano-GCG 适应性攻击。

**我的思考**：以注意力权重替代扰动式归因，兼顾精度与效率且具理论支撑，归因-检测联动范式有推广价值。但注意力反映相关性而非因果性，闭源模型需开源代理归因，存在忠实度缺口；对注意力的对抗操纵仍值得警惕。相比 TracLLM 的迭代搜索，AttnTrace 更轻量且支持多文本协同场景，为 RAG/智能体取证提供实用工具，但不同 LLM 下召回波动提示稳健性仍可提升。

- **arXiv PDF**: [AttnTrace: Contextual Attribution of Prompt Injection and Knowledge Corruption.](https://arxiv.org/pdf/2508.03793)

#### Hollow-LLM Attack: Computationally Trivial Weights in Zero-Knowledge Verification of LLM Inference.

- **作者**: Chen Gong, Beijie Liu, Mengyuan Li

- **方向**: 大模型安全

- **解读**: 远程托管的 LLM 推理需要可验证性——证明服务商确实执行了所宣称模型的计算而非偷工减料；零知识（ZK）LLM 证明承诺公开可验证与逐实例的等式正确性，但不约束"为产生输出付出的计算量"。

作者形式化这一被忽视的缺口，提出 Hollow-LLM 攻击：不诚实的证明者保留声明的参数数量，但嵌入"幽灵权重"（ghost weights）——其代数结构使有效计算坍缩为远小于声明规模的模型；这些 witness 仍满足验证电路、生成有效证明，使证明者以小型模型的计算成本交付"可证明正确"的输出，形成有利的欺诈均衡；并刻画可用标准 transformer 块构造的具体空心部署族。

结果显示，Hollow 部署可在不损失精度的前提下大幅降低服务端计算量（质量无损、同一电路），证明"大模型可验证推理"还需额外的可验证工作（verifiable work）类保护。

我的思考：该攻击击中 ZK-ML 的信任盲区——"正确性 ≠ 工作量"，把算力审计问题重新摆上台面，对按算力/规模计价的推理市场构成根本性质疑；应对方向（可验证工作、计算锚点）仍需在可接受性能下落地；局限在于攻击对特定电路结构与证明系统实际开销的依赖。


#### Investigating the Impact of Dark Patterns on LLM-Based Web Agents.

- **作者**: Devin Ersoy, Brandon Lee, Ananth Shreekumar, Arjun Arunasalam, Muhammad Ibrahim, Antonio Bianchi, Z. Berkay Celik
- **方向**: 大模型安全
- **解读**:
深色模式（dark patterns，欺骗性界面设计）长期主要针对人类用户，通过诱导其做出非预期决定来牟利。随着用户越来越多地让 LLM 驱动的 Web 智能体代为执行在线任务，一个关键问题浮现：这些针对人的欺骗设计是否同样能操纵自动化智能体？此前无人系统研究，本文是首个考察深色模式对 LLM 智能体决策过程影响的工作。

方法上，作者构建两个组件：LiteAgent 轻量框架，自动提示智能体执行任务并记录全面的交互日志与屏幕录像；TrickyArena 受控环境，包含电子商务、流媒体、新闻平台等领域应用，内置多样化、可选择性启用的真实深色模式。基于 LiteAgent，在六个流行智能体与三个 LLM 上评估单个及组合深色模式对智能体行为的影响。

结果显示，环境中存在单个深色模式时，智能体平均 41% 的执行时间会受影响；通过视觉设计变化与 HTML 代码调整修改 UI 属性、以及同时引入多个模式，都会显著影响易感性，说明欺骗效果取决于模式的呈现方式与组合。

我的思考：该工作把深色模式研究从人因扩展到 AI 智能体这一新主体，揭示针对人的欺骗设计同样能欺骗自动化代理，41% 的平均受陷率说明问题严重。LiteAgent + TrickyArena 提供了可复现的测试框架，为后续研究奠定基础设施。局限在于受控环境与现实网站的差距、智能体版本与提示差异的波动，以及"整体性防御"（智能体特定保护 + 更广泛安全措施）尚待验证；与提示注入、UI 操纵类攻击的关系也值得厘清。


### Web安全（10 篇）

#### WebCloak: Characterizing and Mitigating Threats From LLM-Driven Web Agents as Intelligent Scrapers.

- **作者**: Xinfeng Li, Tianze Qiu, Yingbin Jin, Lixu Wang, Hanqing Guo, Xiaojun Jia, Xiaofeng Wang, Wei Dong

- **方向**: Web安全

- **解读**: LLM 驱动的网页智能体可充当"智能爬虫"，以远低于传统爬虫的门槛大规模抓取站点内容（图像、文本、音频）并规避反爬机制；现有反爬方案难以区分合法浏览与智能体抓取，威胁内容型网站的流量与商业利益，构成"智能体即攻击者"的新型 Web 威胁。

作者构建 LLMCrawlBench 基准（237 个真实网页、10,895 张图片、50 个高流量站点），评估 32 种抓取实现（L2S、LNC、LWA）；据此提出 WebCloak 双层防御：动态结构混淆（Dynamic Structural Obfuscation）打乱解析-理解流程并在客户端恢复视觉呈现，优化语义迷宫（Optimized Semantic Labyrinth）注入无害误导线索干扰 LLM 的语义解释，同时保持正常用户体验。

结果显示高级 LLM 框架显著降低抓取门槛并呈现不同的性能/成本权衡；WebCloak 将主流 LLM 抓取器的抓取召回率从 88.7% 降至 0%，且不损害正常用户体验，防护同样覆盖文本与音频资产；绕过该防御需对抓取器核心解析能力进行大规模重工程。

我的思考：这是"智能体即攻击者"范式下的代表性防御，面向解析-理解链路而非单一反爬特征，方法系统、评测完整，为网站运营者提供了可落地的自防护方案。局限在于防御依赖对 LLM 解析模式的先验建模，更强的多模态/交互式智能体可能部分绕过；0% 召回率基于特定抓取器集合，泛化性需持续检验；结构混淆对 SEO 与无障碍访问的潜在副作用值得关注。


#### LEAKYLINKS: Measuring the Security and Privacy Risks of URL Scanning Services.
- **作者**: Ali Mustafa, Jannis Rautenstrauch, Florian Hantke, Shubham Agarwal, Stefano Calzavara, Ben Stock
- **方向**: Web安全
- **解读**: 

**问题**：URL 扫描服务（用于检测钓鱼、恶意软件）会公开索引所扫描的 URL 以共享威胁情报，但 URL 中往往内嵌敏感个人信息（SPI），如会话令牌、凭证等；这些服务可能在"保护用户"的同时无意中泄露敏感数据，形成安全服务自身成为风险源的问题。

**方法**：构建自动化测量管线：从六个主流 URL 扫描服务抓取公开 URL 列表，经 URL 过滤与 LLM 驱动的语义分类器检测其中的 SPI；提出 "state drop" 方法——对同一 URL 先正常访问、再丢弃会话状态后访问，对比前后页面差异以识别会话状态泄露；并用蜜罐页面观察外部实体是否主动访问泄露 URL。

**结果**：三周内访问约 33.2 万条 URL，识别出 4,417 条暴露 SPI 的 URL，精度 97%（为压低误报属保守下界）；蜜罐实验显示包括可疑 IP 在内的外部实体确实主动访问这些泄露 URL，表明野外存在实际利用风险。风险源于扫描服务与站点交互的系统性耦合，而非孤立事件。

**我的思考**：揭示"安全基础设施反而放大泄露"的反讽性风险，方法（state drop + LLM 分类 + 蜜罐验证）完整且可复现，97% 精度说明设计克制。缓解建议（缩短链接有效期、敏感链接默认不公开、增加二次确认）务实可行，但作者也承认即便缓解后大部分扫描 URL 仍公开，说明问题具有系统性。伦理处理（内部模型、安全服务器）值得借鉴。与既有 URL 泄露测量工作相比，首次聚焦扫描服务这一具体生态位。


#### Practical Covert Channel Across Isolated Browser Instances via GPU Command Queue Contention.
- **作者**: Jinhong Liu, Zifeng Kang, Song Li, Yinzhi Cao
- **方向**: Web安全
- **解读**: 
浏览器将不同站点的页面置于隔离进程中，防止恶意网页窃取其他站点数据；但GPU等共享硬件资源为跨隔离边界的通信提供了潜在通道。若攻击者能在两个"相互隔离"的浏览器实例间建立隐蔽信道，即可绕过隔离机制传递敏感信息，威胁用户隐私与数据安全。

据标题推断，论文利用GPU命令队列竞争（command queue contention）构建跨隔离浏览器实例的实用隐蔽信道：通过测量GPU队列的竞争延迟编码与解码比特，使两个无直接通信途径的浏览器实例以隐蔽方式交换数据，并研究信道的容量、可靠性与检测对抗能力。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告信道容量（比特率）、误码率与稳定性等指标，具体数据以全文为准。

GPU已成为浏览器渲染的标准加速器，将其作为隐蔽信道载体具有高隐蔽性与低检测成本，是"硬件资源竞争型"跨域信道的代表性研究。Yinzhi Cao团队在Web安全与隐蔽信道方向积累深厚。局限在于信道速率通常较低、依赖GPU调度细节，实际威胁取决于能否与恶意数据窃取场景结合；对浏览器厂商而言，提示了GPU资源隔离与调度随机化的重要性。


#### State of Browser Process-Isolation: The Same-Site Weakness.
- **作者**: Fabian Kilger, Hannah Fischer, Adrian Staeves, Robin Marchart, Josef Schönberger, Fabian Franzen
- **方向**: Web安全
- **解读**: 
现代浏览器以进程隔离作为安全基石的"最后一公里"：不同站点页面运行在不同进程，使恶意页面难以直接读取其他站点数据。但进程分配策略若存在缺陷——例如同站（same-site）页面被分配到同一进程——将削弱隔离强度，为跨站数据泄露提供可乘之机，系统评估隔离策略的现状十分必要。

据标题推断，论文系统评估浏览器进程隔离的现状，聚焦"同站弱点"：研究进程分配策略在何种条件下把本应隔离的页面放入同一进程，分析由此产生的安全后果（如同一进程内的侧信道与内存泄露面），并评估主流浏览器（Chrome、Firefox、Safari等）的暴露程度。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应给出主流浏览器隔离策略的系统测试结果与弱点分类，具体数据以全文为准。

浏览器安全研究常聚焦于渲染器漏洞本身，而"进程分配策略"这一层较少被系统审视；该工作把隔离策略本身作为攻击面，视角独特且实用。同站进程共享意味着攻击者只需在目标同站注册内容即可靠近受害者，威胁模型现实。局限在于其结论随浏览器版本快速过时，需要持续跟踪；对浏览器厂商的进程分配策略改进具有直接指导意义。


#### KeyChaser: Unveiling API Keys in Browser Extensions.
- **作者**: Shijin Chen, Willy Susilo, Yudi Zhang, Fuchun Guo
- **方向**: Web安全
- **解读**: 
浏览器扩展拥有强大的权限与网络访问能力，开发者常在其中硬编码API密钥（云服务、支付、第三方平台密钥）以便调用服务，这些密钥一旦泄露可被恶意方滥用并造成经济损失；扩展商店的海量规模使密钥泄露问题难以人工核查，自动化检测势在必行。

据标题推断，KeyChaser面向浏览器扩展自动化挖掘API密钥：结合静态分析与动态验证（如向对应API发起探测请求确认密钥有效性），从扩展源码、配置与网络流量中定位真实可用的密钥，并给出密钥类型与归属的判定，形成可规模化的检测管线。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告在扩展商店大规模扫描中发现的密钥数量、类型分布与真实可用率，具体数据以全文为准。

扩展生态的密钥泄露此前多作为个案报道，KeyChaser将其系统化为可规模化的检测管线，对扩展商店审核与开发者安全实践都有警示价值。局限在于"验证密钥有效"可能涉及对第三方API的主动请求，需谨慎处理伦理边界；静态规则也难以覆盖混淆与动态加载的密钥。与GitHub密钥泄露扫描相比，扩展生态的检测面更聚焦、自动化程度要求更高。


#### Site Isolation is Dead: How Site Isolation is Broken in Agentic Browsers and Extensions.
- **作者**: Suyoung Lee, Seongho Keum, Changoo Lee, Dongwon Shin, Sanghyun Hong, Byoungyoung Lee, Sooel Son
- **方向**: Web安全
- **解读**: 
Site Isolation是Chrome等浏览器防御跨站数据泄露的终极防线，将不同站点隔离到独立进程。但新一代"智能体浏览器"（agentic browsers）与浏览器扩展赋予页面和脚本跨站点操作与读取权限，可能系统性绕过Site Isolation的隔离承诺，使该防御形同虚设，对自动化代理场景下的数据安全构成新威胁。

据标题推断，论文系统展示智能体浏览器与扩展如何打破Site Isolation：通过分析代理框架的权限模型与扩展API，识别允许跨站数据访问的路径，构造端到端攻击场景证明隔离失效，并对主流智能体浏览器与扩展进行实证评估，量化其暴露面。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告受影响的浏览器与扩展清单及攻击场景的可行性评估，具体数据以全文为准。

智能体浏览器是LLM时代的核心入口，其"代为操作多个站点"的设计目标与Site Isolation的"严格隔离"天然冲突，该工作敏锐地指出这一结构性矛盾，具有很强的时效性与警示意义。作者团队（Sooel Son、Byoungyoung Lee等）在浏览器安全方向成果卓著。局限在于智能体浏览器生态尚在快速演变，结论需随版本更新；对代理框架设计者而言，如何在"可跨站操作"与"不泄露数据"之间取得平衡是亟待解决的问题。


#### APIECHO: Training-Less Anomaly Detection via Intra-API Behavioral Comparison for Web Applications.

- **作者**: Yihao Peng, Yiming Wu, Du Wu, Shouling Ji, Hai Wan, Xibin Zhao

- **方向**: Web安全

- **解读**: Web 应用 API 异常检测通常依赖训练数据构造正常行为模型，但训练成本高、对新接口与新部署不适应；需要无需训练的异常检测方案以快速部署。

据标题推断，APIECHO 提出"API 内行为比较"：对同一 API 的不同调用进行行为对比，利用响应一致性或语义差异识别异常，无需离线训练，即插即用。

无摘要，无法引用具体量化结果；核心是验证训练无关的 API 行为比较可有效检测异常，其设计无需标注数据，适合冷启动场景。（无摘要，据标题推断）

我的思考：免训练特性使其易于迁移到新系统，intra-API 内部对比避免了跨接口基线漂移问题；若能覆盖参数级行为差异，有望在无需样本标注的条件下实现细粒度防护；局限在于"异常"定义依赖比较维度选择，对需要全局上下文（跨接口业务链）的攻击可能漏报；与基于语义规范的传统 WAF 的互补性值得探讨。（无摘要，据标题推断）


#### Web Application Vulnerability Repair Via Context-Aware Fault Localization and Directed Differential Fuzzing.

- **作者**: Chenlin Wang, Wei Meng

- **方向**: Web安全

- **解读**: Web 应用漏洞（注入、XSS 等）修复高度依赖人工，自动修复需先精确定位缺陷代码再验证补丁有效性；现有方法定位粗糙，或补丁易引入回归。

据标题推断，该工作提出上下文感知的缺陷定位：结合漏洞类型与数据流上下文缩小可疑代码范围；再用定向差分 fuzzing 验证修复——比较修复前后行为差异，确认漏洞被消除且不破坏正常功能。

无摘要，无法引用具体量化结果；核心是"定位 + 差分验证"闭环提升自动修复的精度与安全性。

我的思考：把差分 fuzzing 引入补丁验证可缓解"修了漏洞却破坏功能"的经典问题，上下文感知定位减少无效补丁，实用性强；自动修复若能端到端落地，可显著降低 Web 应用的安全维护成本；局限在于定位依赖污点/数据流分析的精度、差分 fuzzing 的测试充分性，以及生成补丁的可读性与部署成本。（无摘要，据标题推断）


#### Papers, Please: A First Look at Age Verification on the Web.

- **作者**: Shreyas Minocha, Isaac Sheridan, Harry Oppenheimer, Paul Pearce, Michael A. Specter

- **方向**: Web安全

- **解读**: 全球多地立法要求在线平台实施年龄验证（age verification），但该技术的部署方式、隐私影响与规避现状缺乏系统性测量；需要首次大规模刻画 Web 上的年龄验证生态。

据标题推断，作者对 Web 上的年龄验证进行首次测量：识别部署年龄验证的网站与第三方服务商，分析其技术方案（证件扫描、面部年龄估计、付款方式等）、数据收集与隐私风险，并评估绕过手段的可行性。

无摘要，无法引用具体量化结果；核心是刻画年龄验证的部署现状、服务商格局与隐私/安全影响。

我的思考：年龄验证在隐私与合规之间张力明显——过度采集生物特征或证件信息反而制造新风险，测量研究对监管落地与隐私保护设计有参考价值；局限在于样本覆盖与时间窗口，法规与服务商动态演进快，结论时效性需注意。（无摘要，据标题推断）


#### Poisoned by the Host: Large-Scale Measurement of Host Name Poisoning in Web Applications.

- **作者**: Rui Yang, Haoyu Wang, Zhicheng Sun, Zhengyu Liu, Yinzhi Cao
- **方向**: Web安全
- **解读**:
Web 应用普遍信任请求中的 Host 头，并将其用于生成链接、路由决策与安全逻辑（密码重置邮件中的链接、缓存键、反向代理后端选择）。攻击者通过构造恶意 Host 头即可"毒化"应用行为——Host header poisoning——实现密码重置劫持、缓存污染、SSRF 与请求走私等后果；该问题在大规模 Web 生态中的普遍性此前缺乏系统测量。（无摘要，据标题推断）

据标题推断，论文对大量真实 Web 应用开展 Host 头毒化的大规模测量：主动向站点发送构造的 Host 头变体，检测应用是否将其反射进响应/链接/重定向、是否影响缓存与安全功能，量化受影响站点比例、漏洞类型分布与可利用场景，并可能结合被动流量佐证。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出受影响站点规模、漏洞类型分类与代表性利用场景的实证数据。（无摘要，据标题推断）

我的思考：Host header 攻击是经典 Web 漏洞，但"经典"不等同于"已解决"，大规模实证测量能揭示其真实流行度，为漏洞治理优先级与框架默认安全（如 Host 校验）提供依据。局限在于主动测量对目标系统的影响边界、站点样本的代表性，以及测量结论与真实攻击利用之间的距离，需全文确认；与被动数据结合的交叉验证会增强结论可信度。（无摘要，据标题推断）


### 恶意软件与二进制分析（3 篇）

#### Oxidizer: Toward Concise and High-fidelity Rust Decompilation.
- **作者**: Yibo Liu, Zion Leonahenahe Basque, Arvind S. Raj, Chavin Udomwongsa, Chang Zhu, Jie Hu, Changyu Zhao, Fangzhou Dong, Adam Doupé, Tiffany Bao, Yan Shoshitaishvili, Ruoyu Wang
- **方向**: 恶意软件与二进制分析
- **解读**: Rust 程序反编译长期困难：其所有权/借用模型、枚举与模式匹配、迭代器等高级语义在编译后信息大量丢失，通用反编译器输出的伪 C 冗长且失真，严重拖慢恶意样本分析与闭源 Rust 软件审计。（无摘要，据标题推断）

据标题推断，Oxidizer 提出面向 Rust 的高保真反编译方案：利用 Rust 二进制的类型元数据（如 vtable、枚举判别、字符串与 panic 信息）重建类型与所有权结构，并以控制流与调用约定证据推断迭代器/闭包等惯用模式，输出简洁、贴近源码语义的 Rust 伪代码。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在真实 Rust 二进制语料上报告反编译输出的简洁度与语义保真度，并与既有反编译器对比。作者团队为 ASU 逆向团队，具备扎实的二进制分析工程能力。（无摘要，据标题推断）

我的思考：Rust 恶意软件与闭源 Rust 组件日益增多，反编译缺口是分析者的真实痛点；"简洁+高保真"双目标切中要害，若类型重建扎实将显著提升分析效率。局限：无摘要，对优化等级、剥离（stripped）二进制的鲁棒性以及大型程序的扩展性需正文确认；与 Ghidra/IDA 插件路线的差异也值得对照。（无摘要，据标题推断）


#### Catch Me If You Can: Detector-Resistant Evasion via Semantics-Preserving Command Re-Realization.
- **作者**: Muhammad Shoaib, Hare Sudhan Muthusamy, Tareq Alkhatib, Wajih Ul Hassan
- **方向**: 恶意软件与二进制分析
- **解读**: 基于命令（shell 命令、脚本）的检测器是端点防护与攻击检测的核心，攻击者可通过改写命令的写法规避基于签名或规则的检测；如何在不改变命令语义的前提下系统化生成"检测器看不见"的变体，威胁评估与防御设计都缺乏充分理解。（无摘要，据标题推断）

据标题推断，本文提出"语义保持的命令再实现"（command re-realization）：对一条命令自动枚举等价的替代实现，利用 shell 语法特性、环境变量、内建命令、编码与子命令等价变换等，在保持执行语义的同时躲避检测器的特征匹配，形成系统化的规避原语与评估框架。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告该技术对主流检测器的规避成功率、语义保持的验证手段与对抗检测器的改进建议。（无摘要，据标题推断）

我的思考：把"命令改写"从手工技巧升级为可枚举、可验证的系统方法，能同时服务红队评估与检测器加固（通过对抗训练或语义归一化）；作者（Ul Hassan 组）在攻击检测方向有积累，方法论成色可期。局限：无摘要，语义等价的验证边界（环境差异导致的语义漂移）与规模化改写成本需正文确认。（无摘要，据标题推断）


#### CiRCLE: Recovering Complex Data Structures in Binaries Beyond Fragmentation.
- **作者**: Zeyu Gao, Junlin Zhou, Songtao Yang, Chao Zhang
- **方向**: 恶意软件与二进制分析
- **解读**: 从二进制中恢复高级数据结构（链表、树、哈希表等）是逆向工程与漏洞分析的基础，但真实程序中的数据结构常被编译优化切碎——成员分散、内联、融合，传统基于模式或布局的方法只能恢复"碎片"，难以还原完整结构。（无摘要，据标题推断）

据标题推断，CiRCLE 突破"碎片化"限制恢复复杂数据结构：融合多种证据（内存访问模式、分配站点、类型信息、控制流关联）把分散的字段与节点重新聚合成连贯的结构，甚至推断结构间的引用关系与容器语义，输出高保真的数据结构视图。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在真实二进制（含优化与混淆样本）上报告结构恢复的精确率与召回率对比。（无摘要，据标题推断）

我的思考：数据结构恢复是二进制分析的"上层建筑"——结构清晰度直接决定后续反编译与漏洞定位的质量；超越布局匹配、以行为证据聚类的思路切中编译优化现实。作者含 Chao Zhang（清华，二进制安全知名团队），工程可信。局限：无摘要，对高度内联与虚函数多态的恢复能力、以及规模化开销需正文确认。（无摘要，据标题推断）


### 漏洞挖掘与利用（19 篇）

#### StepStone: LLM-Based GPU Kernel Driver Fuzzing via User-Space Libraries.
- **作者**: Xiaochen Zou, Juefei Pu, Arrdya Srivastav, Jonathan Cox, Zhengchuan Liang, Yuan Tan, Xingyu Li, Yilin Zhu, Zhiyun Qian
- **方向**: 漏洞挖掘与利用
- **解读**: GPU 内核驱动是现代系统攻击面最大的部分之一：其 ioctl 接口直接暴露给用户态，但内核模糊测试难以有效覆盖驱动代码，因为需要构造合法请求与正确的设备状态。传统驱动 fuzzing 依赖硬件与手工 harness，覆盖率低、门槛高。StepStone 提出以 LLM 结合用户态库实现 GPU 内核驱动模糊测试，试图在不依赖昂贵硬件与人工逆向的前提下规模化发现驱动缺陷。

据标题推断，StepStone 利用 LLM 分析 GPU 驱动的用户态库（如主流厂商的 userspace 库）中的调用序列，自动生成能触达内核驱动的输入与 harness：借用户态库完成协议与结构编排，在不依赖专有硬件细节的前提下系统化 fuzz 内核驱动代码。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以发现的真实驱动漏洞、崩溃数量及覆盖率相对传统方案的对比作为主要证据。

GPU 驱动 fuzzing 的难点在于"如何构造合法深层次调用"，借用户态库作桥梁是聪明的解耦，LLM 生成 harness 则大幅降低人工成本，代表 LLM 驱动安全测试的实用化方向。局限：LLM 生成质量与覆盖率上限、对驱动符号信息的依赖需实证检验；作者团队（Zhiyun Qian 组）在驱动与内核安全上积累深厚，漏洞发现的可信度较高。


#### Fuzzing the Physical Space: Physics-Aware Testing of Black-Box Industrial Control Systems.
- **作者**: Burak Sahin, David Oygenblik, Mingxuan Yao, Yizhi Huang, Brendan Saltaformaggio, Saman A. Zonouz
- **方向**: 漏洞挖掘与利用
- **解读**: 工业控制系统（ICS）运行真实物理过程，传统模糊测试只关注软件状态，产生大量物理上不可能的输入，且黑盒场景下缺乏内部反馈信号，导致测试效率低下、误报率高，也难以发现“软件正常但物理过程失控”这类 ICS 特有缺陷。论文提出物理感知（physics-aware）的黑盒 ICS 模糊测试方法。

据标题推断，作者将物理过程模型（如传感器读数约束、执行器语义、工艺安全界限）融入测试的输入生成与异常判定：只生成物理可行的测试序列，并以物理层面的异常响应（而非仅软件崩溃）作为漏洞信号，从而在黑盒条件下高效探测 ICS 的逻辑缺陷。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以真实或仿真 ICS 环境（PLC、SCADA 协议）中发现的异常与物理约束违反作为证据。

ICS 模糊测试的"物理语义鸿沟"是实际部署的最大障碍，物理感知是把 fuzzing 从玩具示例带到真实工艺的关键一步；作者（Saltaformaggio、Zonouz 等）在 ICS 取证与攻防方向积累深厚。局限：物理模型获取成本高、跨工艺可迁移性弱，异常判定依赖仿真保真度；但该方法学对关基设施安全测试具有明确指导意义。


#### Agentic Concolic Execution.
- **作者**: Zhengxiong Luo, Huan Zhao, Dylan Wolff, Cristian Cadar, Abhik Roychoudhury
- **方向**: 漏洞挖掘与利用
- **解读**: 
符号执行与混合执行（concolic execution）是漏洞发现的核心技术，但路径爆炸与约束求解瓶颈长期制约其规模化应用；传统工具依赖启发式选择探索方向，难以像人类分析师那样利用对程序结构的整体理解做全局决策，探索效率低是符号执行走向实用的最大障碍。

据标题推断，Agentic Concolic Execution将大语言模型（LLM）智能体引入混合执行循环：由LLM智能体根据当前路径、程序语义与历史探索信息动态决策下一步探索方向、约束简化与输入生成策略，把程序理解能力注入执行引擎，从而减少无谓路径探索、提高漏洞发现效率。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应对比该方案与主流符号执行工具在真实程序上的路径覆盖率与漏洞发现数量，具体数据以全文为准。

该团队（Cadar为KLEE作者，Roychoudhury为LLM安全与漏洞修复领域权威）把LLM作为concolic执行的"策略层"，是LLM×程序分析融合的代表性工作；相比纯提示词式漏洞挖掘，智能体与执行引擎的闭环设计更系统。局限在于LLM决策的延迟与成本可能拖慢探索节奏，简单程序上收益有限；若能在主流基准上证明显著提升，将推动符号执行工具进入"智能体辅助"时代。


#### Batch Me If You Can: Coverage-Guided RPKI Fuzzing at Scale.

- **作者**: Haya Schulmann, Niklas Vogel

- **方向**: 漏洞挖掘与利用

- **解读**: 资源公钥基础设施（RPKI）已成为保护域间路由（BGP 防劫持）的关键设施，美国联邦机构正推动其广泛部署，但 RPKI 验证软件除浅层解析外几乎没有被充分测试。AFL++、libFuzzer 等经典模糊器假设"一次执行一个自包含输入"，而 RPKI 仓库含数百个密码学互链对象，无法在多对象仓库中精确归因覆盖率，导致反馈式探索失效、最严重的漏洞被漏检。

论文提出 CAT 模糊器：连续采样（continuous sampling）技术配合"以函数为侧信道"实现大输入仓库中每对象级别的覆盖率归因；将输入解析为带标签树，可在保持密码学有效性的同时执行结构化与语义突变；模板无关的 ASN.1 突变引擎结合非顺序模糊，摆脱单输入假设。

CAT 相对顺序模糊实现 66 倍吞吐提升，比 libFuzzer 与先前工作多探索 24%–47% 的唯一代码路径；对 RPKI 验证器的评估发现 21 个先前未知漏洞，其中 8 个已获 CVE（CVSS 7.5–9.8），涵盖缓冲区溢出、拒绝服务与可利用的仓库投毒逻辑缺陷；工具已开源。

我的思考：这是首个面向"多对象、密码学互链"复杂输入的覆盖率引导模糊框架，填补了关键互联网基础设施的测试空白；CVSS 最高 9.8 说明漏洞严重性极高，方法还可推广到 DNSSEC、TLS 等同类协议。局限：覆盖率归因精度与仓库规模的可扩展性仍有待更大规模验证。对 RPKI 生态安全具有直接且紧迫的贡献。

- **arXiv PDF**: [Batch Me If You Can: Coverage-Guided RPKI Fuzzing at Scale.](https://arxiv.org/pdf/2605.26651)

#### Camveil: Unveiling Security Camera Vulnerabilities Through Multi-Protocol Coordinated Fuzzing.
- **作者**: Fuchen Ma, Yuqiao Yang, Yuanliang Chen, Yanyang Zhao, Ting Chen, Yu Jiang
- **方向**: 漏洞挖掘与利用
- **解读**: 网络摄像头暴露在互联网上且常运行多个协议栈（RTSP、ONVIF、HTTP/HTTPS、专有协议），单一协议模糊测试难以覆盖其复杂的跨协议状态交互，而跨协议组合漏洞（如先 ONVIF 配置再 RTSP 触发的越权）是现实攻击链的高发点。（无摘要，据标题推断）

据标题推断，Camveil 实现多协议协调模糊测试：在多个协议会话间同步状态并联合变异输入，使模糊测试能探索协议交织导致的深层状态与安全属性破坏，可能配合协议状态机建模与覆盖率反馈。

摘要缺失，无法引用量化结果；预期包含在真实摄像头固件上发现的漏洞数量、跨协议触发的漏洞类型（认证绕过、命令注入、内存破坏）与 CVE 情况。

我的思考：摄像头固件漏洞频发且补丁滞后，多协议协调思路抓住了"协议交织是攻击面"这一现实——单协议测试发现的漏洞往往只是冰山一角。Fuchen Ma、Yu Jiang 团队在固件与嵌入式模糊测试上有系列工作，方法可信。挑战在于协议状态机建模的完整性与跨厂商协议差异；无摘要使漏洞规模与协调机制的具体设计待正文确认。


#### Fizzle: A Framework for Deterministic and Reproducible Network Fuzzing.
- **作者**: Nathaniel Bennett, Tyler Tucker, Carson Stillman, William Enck, Patrick Traynor, Kevin R. B. Butler
- **方向**: 漏洞挖掘与利用
- **解读**: 网络协议模糊测试常因时序、随机性与环境差异而难以复现失败用例，给漏洞确认、回归测试与团队协作带来困难；可复现性已成为网络模糊测试工具链的重要工程缺口。（无摘要，据标题推断）

据标题推断，Fizzle 提供确定性与可复现的网络模糊测试框架：通过固定随机种子、控制时序与状态、记录完整执行轨迹与输入，使同一测试在不同环境/时间重复产生相同结果，并可能提供编排多个测试目标（客户端/服务端/中间件）的能力。

摘要缺失，无法引用量化结果；预期包含对框架复现性保证的验证及在真实协议实现上的漏洞发现或回归测试效果。

我的思考：可复现性是模糊测试从"研究工具"走向"工程实践"的关键——无法复现的崩溃对开发者毫无说服力。Enck、Traynor、Butler 团队在网络与移动安全方向积累深，该框架若设计得当可成为协议测试基础设施。价值在于确定性保证的粒度（是否覆盖调度、超时、网络重放）与易用性；无摘要使具体机制与评估待确认。


#### zkFuzz: Foundation and Framework for Effective Fuzzing of Zero-Knowledge Circuits.
- **作者**: Hideaki Takahashi, Jihwan Kim, Suman Jana, Junfeng Yang
- **方向**: 漏洞挖掘与利用
- **解读**: 零知识（ZK）电路是隐私计算与许多密码协议的核心，Circom 等系统把 witness 计算与电路约束合并在一个程序中，简化开发的同时引入安全风险：欠约束（under-constrained）电路会接受非法 witness，过约束（over-constrained）电路会拒绝合法 witness。静态分析误报高、形式化工具难扩展，现有方法还忽略中间程序中止等关键行为。

理论贡献是 Trace-Constraint Consistency Test（TCCT）：一种基础性、语言无关的缺陷形式化，其统一语义涵盖此前定义并同时捕获欠约束与过约束两类漏洞，暴露此前工具因形式化局限而遗漏的完整缺陷空间。

实现为 zkFuzz：基于变异的模糊测试框架，系统性变异 ZK 电路的计算逻辑，用定制启发式注入精心构造的输入触发 TCCT 违规。在 452 个 Circom 程序上发现 85 个缺陷，其中 59 个为零日、39 个经开发者确认并修复（此前工具因形式化局限无法检测），并获得数千美元漏洞赏金；在 Noir 上的初步研究也表明可支持多种 DSL。

我的思考：TCCT 把"计算与约束不一致"上升为语言无关的数学定义，是 ZK 电路正确性研究的奠基性抽象——此前工具各自为政的缺陷定义被统一归约，这解释了为何 zkFuzz 能发现他人遗漏的缺陷。39 个被确认修复的零日与赏金收入是强有力的实证。局限：模糊测试的覆盖率依赖变异策略，452 个程序的规模虽可观但主要集中于 Circom 生态；TCCT 对 witness 生成缺失（而非不一致）类缺陷的覆盖需进一步考察。


#### SpecAuditor: Generating Audit Specifications for LLM-Driven Bug Detection.
- **作者**: Miaoqian Lin, Hao Chen
- **方向**: 漏洞挖掘与利用
- **解读**: 大模型正被用于自动化漏洞检测，但其效果高度依赖"审计规范"——告诉模型要查什么、代码应满足什么契约；人工撰写规范成本高、覆盖面窄，规范缺失时 LLM 检测往往流于泛泛，误报与漏报并存。（无摘要，据标题推断）

据标题推断，SpecAuditor 自动为代码生成审计规范：从被测代码及其 API 契约、注释、历史漏洞中提炼结构化的规范描述，再将其注入 LLM 驱动其定向寻找违规实现；核心是把"规范工程"从人工转为自动化，以提升 LLM 检测的覆盖与精度。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告生成的规范质量与下游漏洞检出率/误报率相对基线（人工规范、无规范）的提升。（无摘要，据标题推断）

我的思考："先规范、后检测"是把 LLM 从"随机审查员"变成"按单核查员"的关键一步，方向正确；规范自动生成的质量直接决定检测上限，需评估其与静态分析（自动契约推断）的结合。局限：无摘要，规范表示形式、评测基准（是否含已知 CVE）与成本需正文确认；LLM 检测的可复现性本身仍是开放问题。（无摘要，据标题推断）


#### The First Large-Scale Systematic Study of Python Class Pollution Vulnerability.
- **作者**: Zhengyu Liu, Jiacheng Zhong, Jianjia Yu, Muxi Lyu, Zifeng Kang, Yinzhi Cao
- **方向**: 漏洞挖掘与利用
- **解读**: Python 生态中大量框架（Web、ORM、配置解析）存在"类污染"（class pollution）——攻击者借由递归合并、属性设置等操作污染类对象的全局属性，进而引发远程代码执行、拒绝服务或逻辑绕过；该漏洞类在 JS 原型污染之后浮现，但其在 Python 中的普遍性、危害与可利用性此前缺乏系统刻画。（无摘要，据标题推断）

据标题推断，本文开展首个大规模系统研究：扫描海量 PyPI 包与真实应用，检测可被污染的可达点（递归更新、键路径写入等模式），结合污点或符号分析评估可达性与可利用性，并分类归纳触发原语与攻击链模板，形成 Python 类污染的完整图谱。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告受影响包/项目的数量级、代表性 CVE 案例与可被利用为远程代码执行的比例。（无摘要，据标题推断）

我的思考：把 JS 原型污染的教训前移到 Python 是及时且高价值的——Python 在数据与 AI 栈中的地位使类污染影响面更广；系统性测量能倒逼框架层加固（如限制递归合并、属性白名单）。作者 Cao 组在 Web/数据安全上有积累，方法可信。局限：无摘要，检测的误报率与"可利用性"判定的严格性需正文确认。（无摘要，据标题推断）


#### Cottontail: Large Language Model-Driven Concolic Execution for Highly Structured Test Input Generation.
- **作者**: Haoxin Tu, Seongmin Lee, Yuxian Li, Peng Chen, Lingxiao Jiang, Marcel Böhme
- **方向**: 漏洞挖掘与利用
- **解读**: 解析类程序（XML、SQL、JavaScript、JSON 等）的测试需要"高度结构化"的合法输入，而传统符号执行/混合执行存在三重局限：路径约束选择不感知输入结构、约束求解能力有限导致大量语法非法输入、依赖人工种子导致测试不连续。

方法上，Cottontail 构建"表达性结构覆盖树"（ESCT）以选择结构感知的约束；设计基于"求解-补全"（Solve-Complete）范式的 LLM 驱动求解器，使生成的输入不仅可满足约束且语法合法；并以历史引导的方式在测试开始前或覆盖饱和后自动获取种子，实现连续测试。系统基于 SymCC 实现。

在八个经过充分测试的开源库、四种格式（XML、SQL、JavaScript、JSON）上，Cottontail 平均超越基线 30.73%–41.32%（行/分支覆盖率）；并发现六个此前未知的漏洞（已分配六个 CVE），均已报告开发者，其中部分已修复。

我的思考：该工作把 LLM 嵌入求解环节而非仅仅做种子生成，直击"约束满足+语法合法"的矛盾，覆盖率与真实 CVE 双指标支撑其有效性，选题与结果都扎实。局限：LLM 求解的延迟与成本、对超长输入的扩展性、以及"求解-补全"在复杂约束下的失败模式需正文细究；六个 CVE 的具体严重性与触发条件也值得对照原文评估。


#### Jazzer: Coverage-Guided Fuzzing for Semantic Vulnerabilities in the Java Ecosystem.

- **作者**: Sergej Dechand, Tobias Wienand, Fabian Meumertzheim, Peter Samarin, Simon Resch, Khaled Yakdan, Thorsten Holz, Flavio Toffalini

- **方向**: 漏洞挖掘与利用

- **解读**: Java 生态的典型漏洞多为语义类（SQL 注入、反序列化、XXE 等），传统以内存破坏为目标、面向 C/C++ 的 fuzzing 思路无法直接迁移；JVM 的托管内存与运行时特性也使覆盖率引导与插桩面临挑战，业界缺少成熟的 Java 模糊测试方案。

据标题推断，Jazzer 将覆盖率引导机制引入 JVM：通过字节码插桩收集覆盖率，在 Java 侧实现自定义 sanitizer 检测语义漏洞，并借助种子生成与语料库进化驱动深度测试，支持与现有测试框架集成。

无摘要，无法引用具体量化结果；论文聚焦方法论与工具落地，展示对真实 Java 项目语义漏洞的发现能力。

我的思考：Jazzer 已成为 Java 模糊测试领域广泛使用的工具，"覆盖率引导 + 语义 sanitizer"的思路把 fuzzing 价值从内存安全扩展到业务逻辑安全；其与 OSS-Fuzz 生态的结合也大幅降低了 Java 项目接入模糊测试的门槛；局限在于复杂框架依赖与状态模拟仍需人工构造 harness。（无摘要，据标题推断）


#### Specializing Language Models for Textual Fuzzing via Reinforcement Learning.
- **作者**: Jiayi Lin, Liangcai Su, Junzhe Li, Chenxiong Qian
- **方向**: 漏洞挖掘与利用
- **解读**: 编译器、解释器、数据库引擎等复杂文本目标的模糊测试，要求输入满足精细语法语义约束；LLM 虽有潜力，但面临三大挑战：真实代码库深层逻辑探索不足、大模型调用成本高、奖励信号不精确且稀疏。传统方法依赖手工语法或规范，目标演进即失效。

方法上，R1-Fuzz 用覆盖切片式问题构造（沿执行路径切片代码、针对未覆盖分支生成问题）和基于距离的奖励计算（输入执行路径与目标分支的距离，借鉴定向模糊思想），通过 GRPO 强化学习对 Qwen2.5-7B 后训练得到 R1-Fuzz-7B；三段式流程为数据集构建（共 16,338 个问题）、RL 后训练、LLM 驱动的覆盖引导模糊循环。

在 1,640 题静态测试集上，R1-Fuzz-7B 的 Pass@1 达 50%，远超基础模型（8%）、DeepSeek-V3（23%）与 GPT-o4mini（35%）；模糊循环中平均覆盖比 AFL++、libFuzzer、Nautilus、Gramatron、Polyglot 等 SOTA 模糊器高约 75%，并发现 29 个此前未知漏洞（24 个已获确认或修复），单模型 24 小时发现 23 个 bug（GPT-o4mini 15 个、DeepSeek-V3 9 个），且本地运行、无 API 成本（闭源模型每目标 24 小时约 40 美元）。

我的思考：首次把 RL 后训练引入模糊输入生成，走"专才小模型"路线，直接挑战模糊领域"越大越好"的惯例，成本优势显著，消融显示问题格式与调度策略均有效。局限在于依赖静态切片与种子语料、训练目标族有限，泛化到全新语言与协议仍需验证。与 LLM 直接生成输入的 PromptFuzz 类工作相比，其闭环自适应的设计更系统化。

- **arXiv PDF**: [Specializing Language Models for Textual Fuzzing via Reinforcement Learning.](https://arxiv.org/pdf/2509.20384)

#### PILOT: Command-Line Interface Fuzzing Via Path-Guided, Iterative Large Language Model Prompting.
- **作者**: Momoko Shiraishi, Yinzhi Cao, Takahiro Shinagawa
- **方向**: 漏洞挖掘与利用
- **解读**: CLI 模糊测试需同时变异选项字符串与输入文件，深层漏洞只在特定选项-输入组合下暴露；现有方法（CarpetFuzz、ProphetFuzz、ZigZagFuzz、SelectFuzz）只能从 man page 等获取有限选项知识，输入文件多依赖人工准备，导致深层函数难以触及。作者以 FFmpeg RTP 模块中因负 size 参数引发巨量 memcpy 的零日漏洞为例说明问题。

方法上，PILOT 的核心洞察是向 LLM 提供目标函数的潜在调用路径作为上下文：先用静态分析（libclang 调用图加中心性分析）识别候选路径，LLM 据此生成语义丰富的选项串与输入文件（必要时自动编排原生命令行工具生成格式合法的输入文件）；若未达目标，则反馈已覆盖函数并迭代重询。默认 LLM 为 Claude Sonnet 4.0，并与 GPT-4.1 对比。

在 43 个广泛使用的 C 程序上，PILOT 发现 51 个零日漏洞（分布在 24 个程序），其中 41 个获开发者确认、33 个已修复、3 个获得 CVE 编号；覆盖率比 CarpetFuzz 高 16.0%、比 ProphetFuzz 高 36.2%、比 ZigZagFuzz 高 21.8%、比 SelectFuzz 高 57.9%。消融显示去掉路径引导平均下降 19.8%；Claude 版平均覆盖 506.5 个函数/4,232 分支，优于 GPT-4.1 版的 361.3/1,907。

我的思考：把"调用路径"作为 LLM 的语义上下文，既弥补了 man page 知识不足，又自动化了输入文件生成，是区别于既有 CLI 模糊工作的关键创新，51 个零日与 3 个 CVE 证明了实战价值。局限在于依赖静态调用图质量、LLM 推理成本较高、以 C 语言程序为主。与 R1-Fuzz 等面向文本输入的 LLM 模糊框架相比，PILOT 聚焦选项-输入组合空间，二者互补。

- **arXiv PDF**: [PILOT: Command-Line Interface Fuzzing Via Path-Guided, Iterative Large Language Model Prompting.](https://arxiv.org/pdf/2511.20555)

#### Contextualizing Sink Knowledge for Java Vulnerability Discovery.
- **作者**: Fabian Fleischer, Cen Zhang, Joonun Jang, Jeongin Cho, Meng Xu, Taesoo Kim
- **方向**: 漏洞挖掘与利用
- **解读**: Java 漏洞多源于安全敏感 API（sink）的不安全使用，如路径遍历、反序列化导致远程代码执行；sink 蕴含可达性与可利用性两层关键知识，但覆盖引导模糊器（如 Jazzer）对所有路径一视同仁。作者大规模实测发现，Jazzer 在 63,600 CPU 小时（超 7.2 CPU 年）预算下仅利用 14.8% 的已知漏洞，46.3% 的漏洞根本未被触达。

方法上，GONDAR 把 sink 发现分解为两个子任务并部署两个 LLM 代理：sink 检测组件先用 CodeQL 的 CWE 库提取候选，经有效性、可达性过滤与 LLM 可利用性评估精选；探索代理沿入口到 sink 的调用路径迭代求解路径约束生成输入；利用代理基于模糊器反馈的到达输入合成 PoC，满足漏洞触发条件；代理与模糊器双向交换种子与运行时反馈，形成协作闭环。

在含 54 个漏洞、22 个项目、12 类 CWE 的基准上，GONDAR 利用了 41 个漏洞（Jazzer 仅 8 个，4 倍提升），触达 46 个（对 26 个，提升 77%）；消融显示禁用探索使触达数降 31%、禁用利用使利用数降 51%，而两者与模糊器的协同额外发现 7 个单一方找不到的漏洞，且成本（3,051 美元）低于大规模模糊（3,264 美元）。

我的思考：以"sink 为中心"重构 Java 漏洞发现，把 LLM 语义推理、程序分析结构与模糊动态探索三者拧成协作回路，消融与成本数据都很扎实。早期版本助力 Team Atlanta 夺得 DARPA AIxCC 第一名（发现 7 个合成与 3 个零日漏洞），并已集成进 OpenSSF 的 OSS-CRS 项目、发现零日漏洞，实战背书充分。局限在于依赖 CodeQL sink 库覆盖、LLM 成本仍高，与 Jazzer 的通用覆盖路线形成互补而非替代。

- **arXiv PDF**: [Contextualizing Sink Knowledge for Java Vulnerability Discovery.](https://arxiv.org/pdf/2604.01645)

#### deepSURF: Detecting Memory Safety Vulnerabilities in Rust Through Fuzzing LLM-Augmented Harnesses.

- **作者**: Georgios C. Androutsopoulos, Antonio Bianchi

- **方向**: 漏洞挖掘与利用

- **解读**: Rust 默认保证内存安全，但 unsafe 代码若误用仍会引入漏洞；现有 Rust 漏洞检测工具能力有限、对 Rust 特有类型（泛型、trait）支持不足，或严重依赖人工编写 harness，自动化程度低。

deepSURF 将静态分析与 LLM 引导的 fuzzing harness 生成相结合：针对泛型提出新方法——用自定义类型替换泛型参数并为所需 trait 生成定制实现，使 fuzzer 能模拟库内用户自定义行为；同时用 LLM 动态扩充 harness，探索复杂 API 交互，显著提高暴露漏洞的概率。

在 63 个真实世界 crate 上评估，成功重新发现 30 个已知漏洞，并发现 12 个先前未知漏洞（其中 11 个已分配 RustSec ID、3 个已修复），优于现有最先进工具。

我的思考：用 LLM 缓解 Rust 类型系统造成的 fuzz 接入困难，harness 生成与扩充自动化程度高，成果已被 RustSec 采纳，实用价值明确；局限在于 LLM 生成代码的正确性波动、对 unsafe 语义的建模深度，以及泛型/ trait 替换策略在大型库上的可扩展性。


#### Stop Starving or Stuffing Me: Boosting Firmware Fuzzing Efficiency with On-Demand Input Delivery.
- **作者**: Shandian Shen, Wei Zhou, Keming Zhao, Peng Liu, Chung Hwan Kim, Le Guan
- **方向**: 漏洞挖掘与利用
- **解读**: MCU 固件输入经外设异步到达，时间与数量不确定，与通用软件同步读取的模型截然不同；现有固件模糊器按通用方式投递输入，要么过量淹没处理函数（stuffing），要么投递不足无法触发输入处理（starving），严重削弱模糊能力。Fuzzware、MULTIFUZZ 采用固定周期投递，SEmu 需人工指定投递点且不处理缓冲区容量问题。

方法上，作者利用"固件使用数据前必先检查可用性"这一事实，用静态与动态分析把每条输入处理路径映射为 CRP 三阶段（可用性检查、数据获取、处理），在检查点按预期长度范围精准投递；"水位线"（watermark）技术通过逐步加长输入确定固件可处理长度的上下限；并设计多路径调度算法在各输入路径间公平分配测试用例。FIDO 以插件形式集成进 Fuzzware、SEmu、MULTIFUZZ。

在 28 个单元测试与 25 个真实固件镜像上，FIDO 准确识别出全部投递策略；五组 24 小时模糊中，中位覆盖率比 Fuzzware 周期模式最高提升 115%、比 MULTIFUZZ 提升 54%、比 Fuzzware 的 fuzzed 模式最高提升 106%、比需人工的 SEmu 仍提升 19%；崩溃触发次数提升 1–100 倍，发现已知 bug 快数百倍，并新发现 5 个 bug，其中 1 个获 CVE 编号。

我的思考：本文揭示"输入投递"是与多流输入（MULTIFUZZ、Hoedur）正交的固件模糊独立瓶颈，CRP 语义建模比 AidFuzzer 的等待状态启发式更精确，免人工投递点标注相对 SEmu 是显著进步。局限在于依赖静态分析恢复 I/O 语义，MCU 外设的多样性可能产生未建模路径，与真实硬件时序仍有差距。该工作为固件模糊的效率问题提供了系统化解法，工程价值突出。

- **arXiv PDF**: [Stop Starving or Stuffing Me: Boosting Firmware Fuzzing Efficiency with On-Demand Input Delivery.](https://arxiv.org/pdf/2605.16798)

#### PufferDoS: Efficient and Effective Attack String Generation for Regular Expression Denial of Service Vulnerabilities.
- **作者**: Shangzhi Xu, Ziqi Ding, Xiao Cheng, Yuekang Li, Nan Sun, Benjamin Turnbull, Shuangxiang Kan, Siqi Ma
- **方向**: 漏洞挖掘与利用
- **解读**: ReDoS 通过让回溯型正则引擎探索指数级匹配路径耗尽资源，是 npm 生态第四常见、PyPI 第六常见的漏洞类型，2016 年以来已有 350 多个 CVE，曾造成 Stack Overflow 34 分钟与 Cloudflare 27 分钟宕机。现有攻击串生成方法要么假设不切实际的长输入预算、要么不做程序级验证，导致生成的攻击串在真实程序中无法触发漏洞。

方法上，作者先形式化证明"选择特定字符并排序可放大匹配成本"的效应，据此定义三种正则脆弱模式并设计对应的攻击串合成规则；随后用 ReDoS 专用的组合式 concolic 执行（配合函数摘要）细化并验证候选攻击串，保证在真实程序的数据流约束下可利用，且可作插件接入任何 ReDoS 检测器。

在 17,962 个真实脆弱正则上，达到相同匹配时间阈值（0.1 秒、1 秒、10 秒）时，PufferDoS 生成的攻击串比 SOTA 工具 RENGAR 短 97.2 倍到 3,872.4 倍；成功复现 96.8% 的已利用 ReDoS CVE（RENGAR 仅 77.4%）；在 12 个月下载量超一万的开源项目中新发现 59 个可利用 ReDoS 漏洞（比 RENGAR 多 25 个），投稿时已有 6 个项目开发者确认其中 21 个。

我的思考：把"攻击串长度"与"程序级可利用性"同时作为一等公民，形式化证明加组合 concolic 验证使结果可复现、可证明，97.2 倍以上的长度压缩直接回应"开发者因 ReDoS 攻击不实际而拒绝修复"的现实痛点。局限在于模型针对回溯型引擎，对 RE2 等非确定性/线性引擎不适用，组合 concolic 也存在开销。与静态模式检测类工作互补，是打通"检测到利用"闭环的关键一环。

- **arXiv PDF**: [PufferDoS: Efficient and Effective Attack String Generation for Regular Expression Denial of Service Vulnerabilities.](https://arxiv.org/pdf/2606.19654)

#### HEAP LOCALIZATION: Cache Side-Channel Based Linux Kernel Heap Exploit Techniques.

- **作者**: Yoochan Lee, Sihyun Roh, Hyuk Kwon, Byoungyoung Lee, Thorsten Holz

- **方向**: 漏洞挖掘与利用

- **解读**: Linux 内核堆利用常需精确定位堆对象（堆风水），但内核地址随机化与堆布局不确定性使对象定位困难；现有技术依赖大量尝试或内核信息泄露，利用可靠性低。

据标题推断，该工作利用缓存侧信道（如 Flush+Reload / Prime+Probe 或访问延迟差异）探测内核堆对象的缓存状态，从而定位特定堆对象的位置与生命周期，为后续利用（UAF、越界写等）提供精准目标。

无摘要，无法引用具体量化结果；核心是把缓存侧信道引入内核堆布局感知，提升利用的可靠性与成功率。

我的思考：把侧信道从"泄露秘密"扩展到"泄露堆布局"是攻击技术的实用演进，可能使内核利用从"运气/暴力尝试"走向"可测量、可规划"；对防御方的启示是堆缓存行为本身即是泄露源；局限在于侧信道测量对 CPU/调度噪声敏感，新硬件上的可靠性需验证。（无摘要，据标题推断）


#### TRIGFUZZ: Triggering Conditions Guided Directed Fuzzing.

- **作者**: Yiyang Chen, Nuoqi Gui, Long Wang, Longfei Chen, Xuanqing Shi, Xi Cao, Chao Zhang

- **方向**: 漏洞挖掘与利用

- **解读**: 定向 fuzzing（directed fuzzing）旨在快速到达并触发指定目标（补丁后代码、漏洞点），但现有方法常忽略目标代码的触发条件（前置状态、参数约束），导致"到达目标却无法触发 bug"。

据标题推断，TRIGFUZZ 以触发条件为引导：分析目标代码的执行条件与约束，在距离度量之外引入条件满足度，引导 fuzzer 生成真正能触发目标的输入。

无摘要，无法引用具体量化结果；核心是证明触发条件引导比单纯距离引导更快、更准地触发目标漏洞。

我的思考：定向 fuzzing 在补丁回归测试与漏洞复现中价值显著，"到达 ≠ 触发"的痛点真实存在，把条件感知纳入引导是自然且有效的改进；触发条件信息还可用于生成更高质量的种子与回归用例；局限在于条件提取的准确性、跨函数复杂条件的可扩展性；与 AFLGo、Beacon 等工作的对比评估是关键看点。（无摘要，据标题推断）


### 网络与协议安全（19 篇）

#### Descriptors of Exposure: Undermining Tor Anonymity Through Exploiting Descriptor Flood.
- **作者**: Chunmian Wang, Junzhou Luo, Zhen Ling, Yue Zhang, Shan Wang, Ming Yang, Guangchi Liu, Xinwen Fu
- **方向**: 网络与协议安全
- **解读**: Tor 匿名网络依赖分布式哈希表（DHT）分发隐藏服务的描述符，描述符携带服务公钥、引入点等敏感元数据，是匿名链中的关键薄弱点。若攻击者能大规模伪造或淹没描述符（descriptor flood），即可观测、关联甚至劫持描述符的获取过程，从而破坏隐藏服务的匿名性。论文研究利用描述符洪泛攻击削弱 Tor 匿名的机制。

据标题推断，攻击者在 Tor 的 DHT 中大规模注入并淹没描述符，占据或污染负责存储与响应的位置，使受害服务的真实描述符被截获、替换或延迟，进而关联隐藏服务与其真实 IP，或对其施加拒绝服务。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重点在于证明洪泛原语在真实 Tor 网络中破坏匿名性的可行性与影响面。

描述符分发层是 Tor 匿名性研究中相对薄弱的环节，洪泛类攻击成本低、难溯源，若被证实有效将构成对隐藏服务生态的真实威胁；与目录污染、Sybil 攻击等既有 Tor 威胁模型结合，还可能进一步放大攻击能力。作者团队长期深耕 Tor 安全（Ling、Fu 等），攻击场景设计应贴近真实部署。局限：大规模洪泛的可检测性、对现代 Tor 版本 DHT 加固的有效性均需实证检验；该工作对匿名系统的抗洪泛设计具有明确警示价值。


#### Revisiting PQ Wireguard: A Comprehensive Security Analysis with a New Design Using Reinforced KEMs.
- **作者**: Keitaro Hashimoto, Shuichi Katsumata, Guilhem Niot, Thom Wiggers
- **方向**: 网络与协议安全
- **解读**: 

**问题**：WireGuard 需要后量子迁移，Hülsing 等人（S&P 2021）提出的 PQ WireGuard 用 KEM 取代 DH 握手，但服务器需存储庞大的 Classic McEliece 公钥，内存开销成为部署瓶颈；且 Cremers 等人（CCS 2024）揭示的 PQ KEM 绑定（binding）问题在原设计中未被处理，安全性存疑。

**方法**：提出新的计算安全模型，系统刻画 PQ KEM 绑定问题并修复此前设计缺陷；引入"强化 KEM"（Reinforced KEM，RKEM）作为自然构建块——其密钥可"细化/压缩"，使 PQ WireGuard 服务器无需存储大 McEliece 公钥；并给出名为 Rebar 的具体 RKEM 构造，压缩两个 ML-KEM 类密文，可独立用于其他协议。

**结果**：公钥内存开销降低约 190–390 倍，在保持单 UDP 报文握手约束的同时显著提升部署友好性；在新计算模型下完成全面安全分析。Rebar 的密文压缩技术具有独立价值，可推广到更广的 PQ 密钥交换场景。

**我的思考**：同时解决"内存瓶颈"与"绑定安全"两个此前 PQ WireGuard 的痛点，RKEM 抽象抓住了"服务器持有大量静态密钥"这一内核部署场景的本质。局限在于 RKEM/Rebar 为新构造，需更多密码分析检验；压缩可能引入额外计算。与直接混用 ML-KEM/X25519 的混合方案相比，该设计在带宽与内存间取得更好平衡，为 VPN 后量子迁移提供了务实路径。


#### The Pipes Model for Latency and Throughput Analysis.
- **作者**: Andrew Lewis-Pye, Kartik Nayak, Nibesh Shrestha
- **方向**: 网络与协议安全
- **解读**: 
区块链共识协议的性能（延迟与吞吐）是设计者与研究者最关心的指标，但精确建模困难：延迟受网络、批处理与流水线阶段影响，吞吐受带宽与确认规则制约，缺乏统一的解析模型使得协议比较与参数选择高度依赖仿真，结论难以复现与推广。

据标题推断，论文提出"Pipes模型"，将共识协议的数据流抽象为管道（pipes）系统：每个阶段（打包、广播、排序、确认）视为一段管道，用解析公式刻画延迟与吞吐的权衡，从而统一分析不同DAG或链式共识协议的性能，并给出与代表性协议（如Narwhal等）的对比结论与设计启示。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应给出模型的解析表达式、关键参数的敏感性分析及对代表性协议的拟合结果，具体数据以全文为准。

Lewis-Pye与Nayak是共识协议理论的活跃研究者，Pipes模型若成立，将把共识性能分析从"仿真驱动"提升到"理论驱动"，帮助研究者与工程团队在设计早期即评估延迟—吞吐权衡。局限在于模型抽象必然省略实现细节（网络拓扑、CPU开销），对真实系统的预测精度需要校准；该工作对共识协议的系统化比较具有方法论价值。


#### STIR/SHAKEN: A Cocktail of Cryptographic Clumsiness.

- **作者**: Joshua Brown, Paul Grubbs, Matthew Hardeman
- **方向**: 网络与协议安全
- **解读**:
来电显示欺骗催生了大量电话诈骗；STIR/SHAKEN 是美国及多国部署的基于签名的呼叫认证体系，用密码学手段为呼叫来源背书，是治理机器人电话（robocall）的核心基础设施，但其安全保证与实际效果长期受到质疑。

据标题推断，论文从密码学工程视角剖析 STIR/SHAKEN：分析其证书体系、签名与验证流程、信任根与中介链中的设计缺陷与实现“笨拙”（clumsiness）之处，指出协议宣称的安全属性与现实部署之间的落差。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重心是对协议设计缺陷的系统性剖析与攻击面梳理。

对已上线运营的密码学协议做工程化批评极具现实价值——STIR/SHAKEN 的信任根与回退策略一直是研究者关注的焦点，作者在密码学协议分析上积累深厚；局限在于无摘要难窥具体发现，此类批评要转化为标准修订与监管行动才真正落地。，为监管者评估该体系的真实安全性提供依据。


#### A Liveness Attack to Ethereum PoS with No Additional Cost.

- **作者**: Mingfei Zhang, Rujia Li, Xueqian Lu, Sisi Duan
- **方向**: 网络与协议安全
- **解读**:
以太坊转 PoS（权益证明）后，共识安全研究多聚焦活性（liveness）与最终性（finality）；通常认为破坏协议需要持有大量质押或付出高昂代价，活性攻击被视为门槛较高的威胁。

据标题推断，论文发现一种“零额外成本”的活性攻击：攻击者在既有权益与参与策略下，不增加任何额外质押，即可利用共识协议特定缺陷（如提议者调度、验证者消息时机）破坏链的活性或拖延最终性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；关键主张是攻击成本为零，使威胁模型的现实性显著提升。

“无额外成本”的活性攻击对 PoS 协议设计极具警示意义——若无需额外资源即可破坏活性，说明协议激励机制存在结构性缺陷；与既有最终性攻击（如 balancing attack）的关系需厘清。局限是可行性依赖具体网络参数与客户端实现，测试网/主网层面的实证验证是关键，防御对策（如提议者轮换、消息时序约束）的可行性也值得进一步研究。


#### Lost in Translation: Text Message Spoofing via Email.

- **作者**: Sumanth Rao, Ye Shu, Stefan Savage, Aaron Schulman, Geoffrey M. Voelker, Enze Liu
- **方向**: 网络与协议安全
- **解读**:
短信（SMS）被广泛用于身份验证（如一次性验证码）与通知，其真实性依赖发送方身份；若攻击者可伪造任意号码发送短信，将直接威胁验证码安全与反钓鱼防线，造成账号接管与诈骗风险。

据标题推断，论文研究通过电子邮件到短信（email-to-SMS）网关进行短信伪造：剖析运营商网关的地址解析与转发逻辑，展示攻击者如何借邮件通道冒充任意短信发送方，并测量该路径的可滥用范围与规模。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；价值在于把“邮件→短信”这条隐蔽的伪造路径系统化地暴露给业界与监管者。

短信伪造是电信诈骗与账号接管的重要助推器，而 email-to-SMS 网关常被安全研究忽视；作者团队在测量型安全研究上经验丰富。局限在于无摘要难辨实测规模与受影响运营商范围；若广泛成立，则需推动网关对发送方身份的强校验与监管介入。

#### SaTor: Exploring Satellite Routing in Tor to Reduce Latency.
- **作者**: Haozhi Li, Tariq Elahi
- **方向**: 网络与协议安全
- **解读**: 高延迟是 Tor 网络的关键短板，根源之一是电路常横跨地理上相距遥远的区域，产生大量传输时延。常见解法是修改电路构建过程以避开长距离链路，但这会削弱路由随机性、增加去匿名化风险。如何在降低延迟的同时最小化安全性损失，是核心挑战。

SaTor 提出卫星辅助方案：为部分中继配备卫星接入能力，让慢速链路的数据经卫星传输加速，而不改变既有的路径选择过程——即在不偏置路由随机性的前提下提升性能。评估基于自建模拟器与真实世界测量。

长期运行模拟显示：仅约 100 个高优先级装备卫星的中继，即可为 40% 的电路带来平均 21.8 ms 的期望加速。

我的思考：卫星中继绕开地理距离瓶颈的思路直击 Tor 延迟的物理根源，且"不动路径选择"的设计避免了安全-性能权衡中安全一侧的牺牲，是巧妙之处。卫星链路本身引入新的信任与审查问题（运营商可见流量元数据、成本与可用性），21.8 ms 对 Web 浏览的实际体感提升也有限，更像对"低轨卫星能否救 Tor"的可行性探索；作为未来网络架构的预研参考有价值。


#### A Context Is Worth a Thousand Lies: Evading Intrusion Detectors via Intelligent Context Distortion.
- **作者**: Magdy Nasr, Vansh Rastogi, Azadeh TabibanB
- **方向**: 网络与协议安全
- **解读**: 基于机器学习的入侵检测系统依赖网络流量的上下文特征区分正常与恶意行为，攻击者若能"智能扭曲"上下文（伪装流量特征、注入误导性背景信息），就可能在保持攻击效果的同时绕过检测。（无摘要，据标题推断）

据标题推断，方法核心是学习型上下文扭曲：攻击者建模检测器的决策边界，优化生成与正常流量上下文高度相似的干扰，使恶意流量在特征空间中被误分类为正常，并可能评估不同扭曲策略在不同检测器上的迁移性。

摘要缺失，无法引用量化结果；预期包含对抗攻击下的检测器逃逸率、扭曲开销与跨检测器迁移效果等评估。

我的思考：标题化用"一图胜千言"，暗示少量精心构造的上下文即可骗过检测器，凸显 ML 检测器对特征分布的脆弱依赖，对评估流量侧检测的现实威胁有直接价值。与同类对抗逃逸工作相比，其差异化在"上下文"这一维度——不仅改载荷，还扭曲流量背景；无摘要使攻击假设的现实性（对检测器的知情程度）与防御对策无法评估，需以正文为准。


#### MadeYouReset: Exploiting HTTP/2 Server-Side Resets for Large-Scale DoS.
- **作者**: Gal Bar Nahum, Anat Bremler Barr, Yaniv Harel
- **方向**: 网络与协议安全
- **解读**: HTTP/2 的多路复用与流控制带来效率，也引入新的滥用面：RST_STREAM 帧允许任一方快速终止流，但服务器对大量请求不断处理并发送重置（reset）时，其开销可被攻击者放大为拒绝服务。（无摘要，据标题推断）

据标题推断，MadeYouReset 研究"服务器侧重置"被攻击者利用的机制：攻击者以极低成本诱发服务器生成大量重置（如请求不存在的资源、触发协议错误路径），使服务器 CPU/内存被重置处理流程耗尽，从而实现大规模 DoS，且难以与传统 DDoS 区分。

摘要缺失，无法引用量化结果；预期包含对主流 Web 服务器/代理重置处理路径的开销测量、放大倍数与攻击规模评估。

我的思考：HTTP/2 的重置语义本为优雅取消设计，其"廉价触发、昂贵处理"的不对称性正是 DoS 的理想放大器，标题"MadeYouReset"点出攻击者借服务器之手完成攻击。与 HTTP/2 慢速攻击、流复用滥用等同类工作相比，其价值在于揭示重置路径这一相对少被研究的原语；实际危害取决于重置处理成本的放大比与 CDN/代理层的缓解能力，细节待正文确认。


#### The Threat Landscape of IP Leasing in the RPKI Era.
- **作者**: Weitong Li, Yongzhe Xu, Taejoong Chung
- **方向**: 网络与协议安全
- **解读**: IP 地址租赁市场快速扩张，租户获得地址使用权的合法性依赖资源持有者的授权，而 RPKI（资源公钥基础设施）为地址授权提供可验证的加密背书。租赁关系与 RPKI 授权之间的错位（如租约到期未撤销 ROA、租户冒用）可能形成新的路由劫持与欺诈面。（无摘要，据标题推断）

据标题推断，该研究对 RPKI 时代 IP 租赁的威胁格局进行实证测量：收集租赁市场数据与 RPKI 数据（ROA、AS 授权），交叉分析租赁地址的 RPKI 覆盖、授权链完整性与被滥用（劫持、垃圾邮件、恶意托管）情况。

摘要缺失，无法引用量化结果；预期产出包括租赁地址的 RPKI 合规率、授权错位案例与真实滥用事件关联的统计刻画。

我的思考：地址租赁的"所有权与使用权分离"天然挑战 RPKI 的"持有者即授权者"模型，Chung 团队在网络测量与路由安全上积累深厚，此类测量能揭示合规性与现实威胁的鸿沟。若发现大量租赁地址缺乏或错误配置 RPKI 授权，将直接影响运营商的过滤策略与市场治理；测量结论的时效性（数据窗口）与滥用归因的准确性是评价其价值的关键。


#### The Fault in Our Drafts: Vulnerabilities in RPKI Specification and Software.
- **作者**: Oliver Jacobsen, Tobias Kirsch, Haya Schulmann, Niklas Vogel, Michael Waidner
- **方向**: 网络与协议安全
- **解读**: RPKI 是路由安全的关键基础设施，其正确性依赖规范（RFC/草案）与开源实现（如 rpki-client、Routinator 等）的严谨性；规范表述模糊、实现偏差或解析漏洞都可能被攻击者利用，破坏路由授权验证的可信度。（无摘要，据标题推断）

据标题推断，该工作对 RPKI 规范与软件进行系统性漏洞审计：审查相关草案与标准文本中的歧义与缺口，并对主流开源 RPKI 软件做代码审计或模糊测试，识别可导致错误验证结果或拒绝服务的缺陷。

摘要缺失，无法引用量化结果；预期产出包括发现的漏洞清单、受影响实现与版本、以及规范修订建议。

我的思考：Schulmann 团队是 RPKI 与 BGP 安全领域的活跃研究者，此前在 RPKI 漏洞与路由安全测量上多有产出。规范与实现双重审计能同时推动标准修订与软件加固，对依赖 RPKI 做前缀过滤的运营商有直接意义。此类研究的关键在于漏洞的现实可利用性（能否实际造成劫持或错误宣告）与披露协调；无摘要使漏洞数量与严重性分布待确认。


#### Designing Transport-Level Encryption for Datacenter Networks.
- **作者**: Tianyi Gao, Xinshu Ma, Suhas Narreddy, Eugenio Luo, Steven W. D. Chien, Michio Honda
- **方向**: 网络与协议安全
- **解读**: 数据中心正转向 NDP、Homa 等新型传输协议以获得低延迟与高带宽，但这些协议原生不支持加密，而数据中心内部流量同样面临窃听与中间人威胁。如何在新型传输上原生集成数据加密，是设计层面的开放问题。

论文提出 SDP 协议设计：为 NDP/Homa 等新兴数据中心传输集成数据加密，支持 NIC 卸载，按 TLS-over-TCP 的思路设计，可原生地与 TCP、UDP 共存，并提供基于消息的抽象，支持低延迟 RPC 与细粒度并行。

摘要信息有限，未给出量化性能数据；核心贡献在于传输级加密的协议设计与 NIC 卸载可行性论证。

我的思考：传统观点认为数据中心内部网络可信，但侧信道、租户间攻击与硬件供应链威胁使内部加密日益必要；现有方案多把 TLS 硬套在数据中心传输上，开销与延迟不匹配。SDP 把加密"沉入"传输层并与 NIC 卸载结合，思路契合 Homa/NDP 的硬件加速路线。挑战在于加密与多路径、乱序交付、在网计算的兼容，以及部署改造代价；量化吞吐/延迟对比待正文补充。


#### CenRL: A Framework for Performing Intelligent Censorship Measurements.

- **作者**: Hieu Le, Armin Huremagic, Kevin Wang, Roya Ensafi, Ram Sundara Raman

- **方向**: 网络与协议安全

- **解读**: 网络审查测量需大规模、持续探测网络异常，但传统方法依赖固定探测策略，难以自适应不同地区、不同封锁机制与对抗性部署的变化，测量效率与覆盖不足。

据标题推断，CenRL 提出"智能审查测量"框架：将测量策略建模为可学习的决策过程，根据历史探测结果动态选择探测目标与探测方式，从而更高效地刻画审查行为。

无摘要，无法引用具体量化结果；核心是提供自适应测量框架以提升审查探测的效率与覆盖面。

我的思考：审查机制不断演化（主动探测、绕过流量识别），自适应测量是合理方向；与 OONI、Censored Planet 等既有测量基础设施的关系及真实部署效果值得关注，测量伦理（避免触发更严封锁）也需纳入设计；此外，如何把测量结果转化为可验证的封锁证据、支持跨地区对比，也是框架设计需回答的问题。（无摘要，据标题推断）


#### CenAlert: Amplifying User Voices to Rally Censorship Investigation.

- **作者**: Aaron Ortwein, Anna Ablove, Armin Huremagic, Luqin Chang, Vinicius Fortuna, Roya Ensafi

- **方向**: 网络与协议安全

- **解读**: 网络审查研究依赖受控测量与专家分析，普通用户的真实访问遭遇难以大规模汇聚；如何把分散的用户声音转化为可指导审查调查的证据，是测量研究中的空白。

据标题推断，CenAlert 设计用户参与机制：从用户端收集访问异常与阻断报告（如浏览器扩展或客户端遥测），聚合与验证后向研究者输出警报，放大用户声音以触发针对性的审查调查。

无摘要，无法引用具体量化结果；核心是众包式审查警报系统，把"用户报告"转化为"可调查线索"。

我的思考：用户侧数据噪声大、真实性参差，聚合与去噪设计是关键；与 Censored Planet 等主动测量互补性强——主动探测回答"哪里被封锁"，CenAlert 回答"谁在经历封锁"；隐私保护与对抗审查者滥用报告机制是需要关注的风险，如何平衡众包数据的隐私与可用性、避免恶意灌入虚假报告，是工程落地的关键。（无摘要，据标题推断）


#### NetPanic: the Attack Surface You Can't Syscall.

- **作者**: Tianshuo Han, Zong Cao, Zhen Dong, Xiapu Luo, Zhenyu Song, Jian Liu

- **方向**: 网络与协议安全

- **解读**: 网络数据路径（协议栈、eBPF/XDP 快速路径、网卡驱动）是外部可达的攻击面，但其中大量代码路径不经系统调用即可由网络报文触发，传统以 syscall 为中心的分析、监控与沙箱难以覆盖这一隐藏攻击面。

据标题推断，NetPanic 系统刻画并利用"无法通过 syscall 触达"的网络攻击面：分析报文驱动的内核执行路径，构造仅凭网络输入即可触发的攻击或崩溃场景，验证其可被远程利用。

无摘要，无法引用具体量化结果；核心是刻画非 syscall 网络攻击面并验证其可利用性。

我的思考：现代内核把越来越多逻辑下沉到快速路径（eBPF、XDP、网卡卸载），这些路径绕过传统审计边界，"You Can't Syscall"的视角提醒防护体系不能只盯系统调用；其发现的攻击模式可为网络侧入侵检测与内核硬化提供新方向；局限在于攻击场景的可达性与稳定性，以及 eBPF 验证器等缓解措施的持续演进。（无摘要，据标题推断）


#### Incalmo: an Autonomous Llm-Assisted System for Red Teaming Multi-Host Networks.
- **作者**: Brian Singer, Keane Lucas, Lakshmi Adiga, Meghna Jain, Lujo Bauer, Vyas Sekar
- **方向**: 网络与协议安全
- **解读**: 

**问题**：红队演练是企业发现防御缺口的关键手段，但真实企业攻击常横跨多个"跳板"主机，成本高、极度依赖专家。LLM 在 CTF 中已展现攻击潜力，能否自主完成多主机红队演练仍是空白。作者构建 MHBench 基准（40 个仿真网络，22–50 台主机）评估现有系统，发现 PentestGPT、CyberSecEval3、CAI 等 SOTA 系统几乎全部失败，明确了研究缺口。

**方法**：日志分析归纳出四大失败模式：执行无关任务（47–90% 命令无关）、命令实现错误（相关任务 6–41% 出错）、脆弱的利用后控制、上下文膨胀（一次演练膨胀至 5.4 万 token）。据此提出 Incalmo，将规划与执行解耦：LLM 只按网络杀伤链输出高层声明式任务（扫描、横向移动、提权、信息搜集、数据外渗），由确定性领域代理执行，并引入环境状态、攻击图、C2 三个辅助服务管理知识与资产。

**结果**：Incalmo（Sonnet 4）在 40 个环境中成功获取关键资产 37 个，最优基线 ExpertPromptShell 仅 3 个；28 个环境实现 5/5 次全成功，21 个环境获取超 50% 资产，9 个环境拿到全部资产（基线最高 24%）。成功攻击耗时 12–54 分钟、花费不超 15 美元；换 10 种 LLM 仍成功 6–9/10 环境，小模型（Haiku 3.5）胜过大型模型基线，证明抽象设计比模型规模更关键。

**我的思考**：核心成色在于用数据证明"提升抽象层次"而非"改进提示词"是 LLM 胜任长时程攻击的关键，规划/执行分离与 RAG 式知识服务对智能体领域有普适借鉴，且全开源。局限：不考虑防御方检测、仅限已知漏洞、仿真环境离真实对抗有距离，双用途伦理需警惕。相比规则系统 Caldera，其展示了 LLM 知识与专业工具结合的范式价值，0day 扩展性尚待验证。

- **arXiv PDF**: [Incalmo: an Autonomous Llm-Assisted System for Red Teaming Multi-Host Networks.](https://arxiv.org/pdf/2501.16466)

#### One Char to Rule Them All: Systematically Exploring and Exploiting DNS Silent Vulnerabilities in Domain Name Resolution.

- **作者**: Fasheng Miao, Xiang Li, Changqing An, Wenbin Xu, Jilong Wang
- **方向**: 网络与协议安全
- **解读**:
DNS 解析依赖精确匹配与多层容错机制，历史包袱（通配符、宽松比较、大小写与编码归一化、缓存键差异）使解析器之间存在大量"静默漏洞"：输入上的细微差异（如单字符、大小写、编码变体）可能导致不同解析器对同一域名给出不同结果，且失败方式隐蔽——不报错、不告警，仅返回错误或异样的解析，从而被攻击者利用实现劫持、污染或绕过。这类漏洞成本极低，却可能命中关键域名。（无摘要，据标题推断）

据标题推断，论文对域名解析中的静默漏洞做系统性探索与利用：构造单字符级与编码级的输入变体集合，测量权威服务器与递归解析器对变体输入的响应差异，归纳不一致模式，识别可被利用的解析路径，并评估其安全影响（域名劫持、缓存污染、恶意过滤绕过）。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出解析不一致的测量规模、漏洞分类以及代表性场景下的可利用性演示。（无摘要，据标题推断）

我的思考：DNS 生态的宽松实现是长期存在的攻击面，单字符攻击成本极低、覆盖面广，系统性枚举"静默失败"路径比单点漏洞报告更具价值，对解析器厂商与域名运营者有直接警示。局限在于漏洞的实际触发条件（缓存、DNSSEC、解析器版本差异）与测量方法对真实解析器部署的代表性，需全文确认。（无摘要，据标题推断）


#### Resolve the Unresolved: Systematic Work Profiling for DNS Resolvers.

- **作者**: Liwen Xu, Huayi Duan, Zechao Cai, Adrian Perrig
- **方向**: 网络与协议安全
- **解读**:
DNS 解析器是互联网基础设施的枢纽，但其工作负载（查询类型分布、缓存命中率、上游依赖、失败与重试模式）长期缺乏系统性画像，导致容量规划、配置调优与故障诊断多依赖经验而非数据。理解"解析器实际在做什么、为什么做、何时失败"，是提升解析服务可靠性与安全性的前提。（无摘要，据标题推断）

据标题推断，论文对 DNS 解析器开展系统性的工作画像（work profiling）：从真实流量或日志中提取查询特征，对解析行为与失败模式进行分类，刻画不同类型解析器（递归、权威、转发型）的工作负载画像，并可能构建可复用的画像方法、数据集或工具。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出解析器工作负载的分布特征、异常模式与失败归因的实证发现。（无摘要，据标题推断）

我的思考：测量型工作为解析器的缓存策略、预取机制与抗放大攻击配置提供实证基线，Adrian Perrig 团队在网络架构与 DNS 方向积累深厚，方法可信度较高。局限在于数据来源的代表性与隐私约束下的日志处理、画像结论向不同规模解析器的推广性，以及工作负载随时间/地域的漂移，均需全文确认。（无摘要，据标题推断）


#### Knocking on the Front Door: An LLM-Guided Systematic Analysis of DNS Query Processing Vulnerabilities.

- **作者**: Yuqi Qiu, Xiang Li, Zheli Liu
- **方向**: 网络与协议安全
- **解读**:
DNS 查询处理（解析器与权威服务器对报文的解析、校验与响应逻辑）存在大量边界错误、解析不一致与静默失败，但传统模糊测试难以系统覆盖其协议语义约束（如压缩指针、EDNS、大小写保留、异常类型码组合），漏洞发现依赖人工或随机变异、效率有限。如何利用 LLM 的协议理解能力生成高语义质量的测试输入，系统性挖掘 DNS 查询处理漏洞，是新的研究问题。（无摘要，据标题推断）

据标题推断，论文以 LLM 引导的方式对 DNS 查询处理实现做系统分析：利用 LLM 生成边界用例、协议变异与差分测试输入，驱动被测解析器执行并检测崩溃/行为不一致，可能结合差分测试（多实现对比）与覆盖率反馈迭代，形成半自动化的漏洞发现流程。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文报告发现漏洞的类型与数量、相对传统模糊测试的覆盖/效率提升，以及漏洞的严重性与可利用性分析。（无摘要，据标题推断）

我的思考：LLM 辅助漏洞挖掘是新兴方向，DNS 报文是结构化协议、语义约束强，恰好适合 LLM 生成"语义有效但异常"的输入，与随机变异相比可显著提高深度逻辑的触达率。局限在于 LLM 生成质量的去重与筛选成本、发现的漏洞的实际可利用性，以及与既有模糊测试（如 AFL、定制解析器 fuzzer）的增量对比，需全文确认；与同组"One Char to Rule Them All"（DNS 静默漏洞）可对照阅读。（无摘要，据标题推断）


### 密码学与协议（54 篇）

#### VerfCNN, Optimal Complexity zkSNARK for Convolutional Neural Networks.
- **作者**: Wenjie Qu, Yanpei Guo, Yue Ying, Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**: 模型推理验证——证明某 CNN 推理结果确实来自给定模型与输入——需要高效的 zkSNARK，但 CNN 包含卷积、激活、池化等异构算子，通用电路编译器生成的证明开销巨大。论文针对 CNN 构造"最优复杂度"的 zkSNARK，目标是把推理上链与可验证外包（如模型服务商证明推理正确、审计方完成验证）推向实用，为安全可信的 AI 提供密码学基础。

据标题推断，作者针对 CNN 各层算子设计定制化的代数表示与证明系统（如利用卷积的矩阵/多项式结构、ReLU 的分段性质），使证明者复杂度达到与 CNN 计算规模匹配的最优或近最优水平，消除通用电路方案的冗余编码。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以证明生成时间、证明大小与验证时间相对通用方案的阶别改进作为核心数据。

若"最优复杂度"成立，将显著降低 CNN 推理可验证外包的代价；该团队（Jiaheng Zhang 等）在 zkSNARK 理论上有扎实成果，可信度较高。局限：最优复杂度通常指渐进意义，常数因子、内存开销与对多样激活函数的支持需实证检验；但面向特定模型结构做定制证明系统是正确且务实的方向。


#### Blinding Post-Quantum Hash-and-Sign Signatures.
- **作者**: Charles Bouillaguet, Thibauld Feneuil, Jules Maire, Matthieu Rivain, Julia Sauvage, Damien Vergnaud
- **方向**: 密码学与协议
- **解读**: 盲签名是电子现金、匿名凭证等隐私场景的核心构件，现有后量子盲签名多基于 Fiat-Shamir 变换或结构化格，而 hash-and-sign 类方案（如 SPHINCS+、Merkle 树签名）结构特殊，其安全根基（哈希函数）最为保守，却缺乏高效的盲化构造，使隐私场景无法直接受益。论文研究对后量子 hash-and-sign 签名进行盲化，目标是在最保守的假设下获得盲签名能力。

据标题推断，作者基于 hash-and-sign 范式（哈希域上的 FDH 型或哈希树签名）设计盲化协议：用户以盲化方式提交消息哈希，签名者在不知晓消息内容的前提下完成签名，利用哈希函数的结构性质保证盲性与不可伪造性，并给出完整的安全性归约。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以盲化协议在签名/验证时间、通信量与签名大小上相对明文签名的增量开销作为证据。

hash-and-sign 方案基于最保守的抗量子假设，若盲化构造成立，将为后量子电子现金与匿名凭证提供假设最稳健的构件；作者（Bouillaguet、Vergnaud 等）是公钥密码理论强队。局限：盲化通常需要交互与重随机化性质，盲性与可链接性模型的选择决定实用边界；但补齐 hash-and-sign 盲化空白本身具有明确价值。


#### AESpoly: Symmetric-Key Cryptographic Designs Using Instruction-Level Parallelism Between AES and Polynomial Hash.

- **作者**: Yukihito Hiraga, Yusuke Naito, Yu Sasaki, Takeshi Sugawara

- **方向**: 密码学与协议

- **解读**: 现代对称密码（AEAD、可调分组密码等）中 AES 与多项式哈希（如 GHASH 类）常交替执行形成依赖链，限制吞吐；而现代 CPU 的 AES 指令与无进位乘（PCLMUL/VPCLMUL）可在指令级并行执行，现有方案未充分利用这一并行潜力，性能优化空间可观。

作者提出 AESpoly 设计族：利用 AES 轮与多项式哈希之间的指令级并行（ILP），把两类计算交错重叠执行，压缩关键路径上的串行依赖；给出 AESpolyW 与 AESpolyM 两个具体方案，分别面向不同的安全与性能权衡，并与 HCTR2、EME、GMAC、PMAC 等既有方案对比；开源实现与基准测试代码。

基准测试覆盖多种分组密码原语，AESpoly 方案在支持的平台上展现出与既有方案可比的正确性与更优的并行潜力；具体吞吐数据以原文实验为准。（无摘要，据公开资料推断）

我的思考：这是面向 AES-NI/VAES 时代的高效认证加密设计：与其追求单条指令更快，不如重新组织数据流吃满执行单元的并行度，思路契合现代超标量处理器特性。局限在于方案依赖 AES 与乘法的 ILP 特性，无专用指令的低端嵌入式平台收益有限；安全性证明与标准化采用路径未展开；与 GHASH 类方案的安全边界（如跨消息注入）需形式化论证。对高性能认证加密的工程实现有直接参考价值。


#### XDup: Privacy-Preserving Deduplication for Humanitarian Organizations Using Fuzzy PSI.
- **作者**: Tim Rausch, Sylvain Chatel, Wouter Lueks
- **方向**: 密码学与协议
- **解读**: 

**问题**：人道主义组织中多个机构在同一地区运作，需跨组织去重以避免同一受助人重复注册（重复注册率估计高达15%），但现有基于共享哈希或明文匹配的去重方案会泄露弱势受助人的隐私数据。论文从NGO需求出发，构建满足隐私、低误报、可扩展与离线/在线双模式的去重系统xDup。

**方法**：xDup先用LSH嵌入把传记数据（姓名、出生日期等）映射到汉明空间（511位、阈值132），使相似记录汉明距离小；再将去重归约为汉明空间模糊隐私集合交集（FPSI），并设计otFPSI协议——基于SHADE构造用不经意传输（OT）安全计算汉明距离与阈值比较，无输入假设、精确结果、复杂度与维度准线性。为适配田野团队资源受限与离线需求，计算外包给两个不共谋的总部计算节点（秘密共享数据库），支持在线单条查询与离线批量模式。

**结果**：端到端去重仅需3小时，比现有方法快84倍（MainSEL的SMC需约10天）；嵌入+精确FPSI仅漏0.6%重复（召回99.4%），误报率0.098%满足需求；otFPSI在多数参数下优于全部现有FPSI协议——比Approx-PSI快7.8倍（4096规模）、比PE-FPSI快1530倍（256规模、τ=16）且通信减少44倍。

**我的思考**：该工作的成色在于系统化梳理真实NGO需求并以具体效率数字落地，otFPSI"无输入假设+精确结果+高维可扩展"的组合确实填补了现有FPSI协议的空白，外包双节点架构贴合人道场景的非共谋现实。局限是仅在合成数据上评估（出于伦理未用真实数据），真实转录误差分布的模拟是否准确存疑；otFPSI通信开销较高、二次复杂度在更大规模下可能成为瓶颈；部署依赖两个总部节点长期在线与非共谋假设。

- **arXiv PDF**: [XDup: Privacy-Preserving Deduplication for Humanitarian Organizations Using Fuzzy PSI.](https://arxiv.org/pdf/2604.08019)

#### Concretely-Efficient Multi-Key Homomorphic Secret Sharing and Applications.
- **作者**: Kaiwen He, Sacha Servan-Schreiber, Geoffroy Couteau, Srinivas Devadas
- **方向**: 密码学与协议
- **解读**: 

**问题**：同态秘密共享（HSS）允许在份额上直接做安全计算，多密钥 HSS（MKHSS）更进一步支持各参与方用独立密钥编码输入、无需协调联合公钥或交互式设置；但此前 Couteau 等人（Eurocrypt 2025）的 MKHSS 构造受限于最大模数高达 N^4，具体效率低下，难以实用。

**方法**：在可复用公共参考串（CRS）与离散承诺重随机预言（DCR）假设下优化 MKHSS：将最大模数从 N^4 降至 N^2（与 Paillier 模数同级，为 RSA 模数一半），并配合算法级优化；各参与方仅需独立输入密钥即可，编码与 CRS 可跨多方、跨轮次复用。

**结果**：单次同态乘法约 5.0 毫秒，而此前构造为 224.6 毫秒（约 45 倍加速）。应用于属性基非交互密钥交换（ANIKE），首次给出两类谓词的实用实例化：基于地理位置密钥交换在单核上 1 秒内完成，8 词口令的模糊 PAKE 数秒完成，整体较此前 MKHSS 方案快约 30 倍。

**我的思考**：把 MKHSS 从理论可行推进到具体可用，模数降阶是关键的工程洞察；ANIKE 把 PAKE 推广到任意属性策略，开辟了地理位置、口令模糊匹配等现实应用。局限在于基于 DCR/格上假设、单次乘法毫秒级延迟仍高于经典密码，且 CRS 可复用性依赖生成方的可信设置。与格基 HSS 方案相比，该路线避免了多密钥协调的交互开销，是隐私密钥管理方向的实质进展。


#### No Honor Among Crooks: Non-Transferable Anonymous Tokens from Betrayability.
- **作者**: David Kretzler, Yong Li
- **方向**: 密码学与协议
- **解读**: 

**问题**：匿名令牌（如隐私通行证、验证码豁免凭证）支持匿名验证，但可被持有者转卖/共享，破坏其经济模型。Durak 等人（CCS 2024）的非转移匿名令牌（NTAT）将令牌绑定到"保险秘密"以抑制转让，但在"供应商协助兑换"场景下失效——攻击者可借助供应商完成兑换而无需持有保险秘密。

**方法**：提出"可背叛性"（betrayability）概念：令牌购买者要么证明自己知道保险秘密，要么在供应商协助兑换时通过嵌入的陷阱门提取供应商的秘密（背叛供应商），使"协助兑换"在功能上等价于"转移保险秘密"，从而在供应商参与兑换时仍保持转让威慑。形式化匿名可背叛令牌（ATB）安全定义，给出基于 BBS 签名的实例化并证明安全性。

**结果**：形式化并实现了支持供应商协助兑换下不可转让性的匿名令牌协议，提供完整安全证明与实现/实验评估，为 NTAT 补上了"供应商合谋/协助"这一关键漏洞场景。

**我的思考**：用博弈论式激励机制（背叛威慑）而非纯密码学约束解决转让问题，思路巧妙；"背叛供应商"的陷阱门设计使供应商没有动机协助兑换，机制自洽。局限在于依赖保险秘密的实际价值与攻击者风险偏好假设，且 BBS 实例化的部署成本与生态接受度待检验。与硬件绑定、生物识别等非转移手段相比，纯密码学+激励方案更易部署，是匿名凭证实用化的重要拼图。


#### Starfighters-On the General Applicability of X-Wing.
- **作者**: Deirdre Connolly, Kathrin Hövelmanns, Andreas Hülsing, Stavros Kousidis, Matthias Meijers
- **方向**: 密码学与协议
- **解读**: 

**问题**：X-Wing 是基于 ML-KEM-768 与 X25519 的具体混合 KEM，其安全性依赖 KEM 组合器 QSF；但 QSF 是否适用于更广泛的标准化后量子 KEM 与 ECDH 变体、能否作为通用混合 KEM 范式，此前未被系统回答。

**方法**：系统分析 QSF 组合器，通过密文第二原像抗性（C2PRI）等条件为多种 FO 变换变体建立与 QSF 的兼容性判据；并引入 QSI——QSF 的 KEM-only 变体，直接哈希两个 KEM 的共享密钥，证明只要其中一个 KEM 满足 IND-CCA、另一个满足 C2PRI，组合即 IND-CCA 安全；同时分析 QSF/QSI 对 KEM 绑定性质（HON-BIND、LEAK-BIND、MAL-BIND）的保持性。

**结果**：QSF 兼容几乎所有已标准化或正在标准化的后量子 KEM——ML-KEM、eFrodoKEM、HQC、Classic McEliece 及多种 NTRU 变体（如 ML-KEM-1024、sntrup），使 PQ/T 混合 KEM 构造得以通用化；QSI 与 QSF 具备相同兼容性，且无需群元素，更简洁。

**我的思考**：把 X-Wing 从"单一具体方案"升华为"通用混合范式"，C2PRI 判据让实践者能自行验证任意 KEM 组合的适用性；QSI 的简洁性对 TLS 等协议集成尤其友好。绑定性质分析回应了 KEM 滥用（如混淆身份密钥）的担忧，是协议安全视角的加分项。局限在于 C2PRI 需逐 KEM 验证、部分 KEM 仍需个案分析。该工作为后量子迁移中的混合密码选型提供了权威依据。


#### A Leakage-Free Framework for Private Set Operations.
- **作者**: Wenhao Wu, Yuyue Chen, Bowen Shen, Peng Yang, Ximing Fu, Zoe Lin Jiang, Junbin Fang
- **方向**: 密码学与协议
- **解读**: 

**问题**：私有集合运算（PSO，如 PSI、PSU、交集基数等）在安全多方计算中广泛应用，但多数协议在运行中泄露额外信息（如哈希分桶导致的成员关系泄露）；既有无泄露方案要么只支持单一运算、要么效率低下，缺乏支持广泛 PSO 功能集的高效统一框架。

**方法**：提出两个新原语：pSEQT（置换安全相等测试，由置换相关生成器与安全相等测试构成）与 pbssPMT（置换批量秘密共享私有成员测试，由 pSEQT 结合不经意伪随机函数 OPRF 与不经意键值存储 OKVS 构造）；将 pbssPMT 与布谷鸟哈希、不经意传输结合，构建支持交集、并集及关联值运算等多种 PSO 功能、且无运行期泄露的统一框架。

**结果**：实验支持百万级元素数据集，规模化良好；pbssPMT 版 PSO 较 Chen 等人（PKC 2024）的无泄露通用 PSO 框架在线阶段最高提速 3.43 倍；pbssPMT 版 PSU 较 Tu 等人（USENIX Security 2025）领先的无泄露 PSU 在线阶段最高提速 3.97 倍，同时支持更广的 PSO 功能。

**我的思考**：在"消除运行期泄露"与"广泛功能支持"之间同时取得进展，pSEQT/pbssPMT 的原语分解干净、复用 OPRF+OKVS 成熟构件，工程可信。局限在于离线/预处理成本未纳入对比口径，且安全模型为半诚实或需进一步确认恶意安全性。与电路化 PSI 路线相比，该框架在不牺牲在线效率的前提下拓宽了无泄露 PSO 的适用范围，对隐私计算产业有直接价值。


#### A Maliciously-Secure Post-Quantum OPRF from Crypto Dark Matter.
- **作者**: Diego F. Aranha, Aron van Baarsen, Adam Blatchley Hansen, Kent Nielsen, Peter Scholl
- **方向**: 密码学与协议
- **解读**: 

**问题**：不经意伪随机函数（OPRF）是 PSI、口令硬化等隐私协议的核心构件；基于"交替模数假设"（Crypto Dark Matter 范式，Boneh 等，TCC 2016）的 OPRF 具有后量子友好特性，但此前仅在半诚实敌手模型下安全，无法抵御恶意行为方，限制实际部署。

**方法**：利用新的 cut-and-choose 技术为向量线性求值（VOLE）生成相关随机性，实现零知识证明与两方计算之间的高效转换，从而将 CDM 范式 OPRF 提升到恶意安全（malicious security）级别；并支持秘密共享输出、可扩展至秘密共享输入。

**结果**：与最先进的 GOLD OPRF（Yang，S&P 2025）相比，在线阶段在所有设置下均更快，小批量设置下整体效率更优；秘密共享输出与输入支持开启了 PSI 变体与数据库运算等新应用。

**我的思考**：把 CDM 路线从半诚实推进到恶意安全且不牺牲效率，VOLE 化 cut-and-choose 是核心技术贡献；对依赖 OPRF 的后量子隐私协议（PQ-PSI 等）是重要基础设施。局限在于交替模数假设较新、密码分析积累少于格假设；与格基 OPRF 的全面性能对比仍有待更广设置验证。该工作证明"结构化假设+恶意安全"可以兼得效率，是后量子隐私计算的关键拼图。


#### Scalable Accountable Byzantine Agreement and Beyond.
- **作者**: Pierre Civit, Daniel Collins, Vincent Gramoli, Rachid Guerraoui, Jovan Komatovic, Manuel Vidigueira, Pouriya Zarbafian
- **方向**: 密码学与协议
- **解读**: 

**问题**：在非同步网络中，当故障进程数 f ≥ n−2t 时，传统 t 容错拜占庭协议（BA/可靠广播）无法保证一致性；引入可问责性（accountability）与取证支持可在安全违规后追责，但既有 ABC 类变换通信开销达二次方，无法支撑大规模系统。

**方法**：提出 ABC++ 通用变换：以 ratifier 与 propagator 两个原语顺序组合任意封闭式（closed-box）BA 或可靠广播协议，将其提升为可问责、带取证支持的版本；提供 1-delayed 自适应安全的问责保证，通信附加开销为次二次方（subquadratic）。

**结果**：决策与取证支持仅增加 1 轮开销，安全违规检测再增 2 轮（优化通信后 O(log n)）；这是首个在 1-delayed 自适应敌手下达到次二次方通信的可问责 BA/可靠广播；优于 Sheng 等人（FC 2023）的次二次方取证方案与二次方 ABC（IPDPS 2022/JPDC 2023），并可接入 τscr 变换（ICDCS 2022）得到保持次二次方乘法开销的 τscr++。

**我的思考**：通用变换让任意现成 BA 协议"一键获得"可问责性，次二次方开销使其真正可扩展到大型系统；自适应敌手下的安全证明是理论深度所在。局限在于封闭式（非黑盒可替换）假设与检测轮次延迟——违规后的问责仍需数轮。对区块链与状态机复制的问责机制设计有直接价值，是"安全违规可追责"方向的重要工程化成果。


#### Practical Asynchronous Distributed Key Reconfiguration and Its Applications.
- **作者**: Hanwen Feng, Yingzi Gao, Yuan Lu, Qiang Tang, Jing Xu
- **方向**: 密码学与协议
- **解读**: 

**问题**：容错门限密码系统需随参与方集合变化而重构密钥（成员加入/退出），异步环境（无时序假设）下尤为困难；此前 ADKR/ADKG 类协议总通信开销达 O(n^3)，无法扩展到大规模异步网络，成为门限密码系统长期运行的实际瓶颈。

**方法**：提出"份额分发-共识-重构"（share-dispersal-then-agree-and-recast）新范式：用 O(n) 次公开笔录分发取代 O(n) 次异步可验证秘密共享（AVSS）步骤，仅随机选取小的 κ 子集做验证；并以线性通信的交互式 PVSS 笔录生成与分布式 PVSS 验证削减冗余计算。

**结果**：总开销从 O(n^3) 降至 O(κn^2)，κ 为约 30 以内的小常数；PVSS 验证成本削减约三分之一。应用成果：首个带 coin 辅助随机性的二次方通信 ADKG、二次方通信且自适应安全的异步动态主动秘密共享（ADPSS）；在 256 台 AWS 服务器的全球网络上，重配置场景延迟较简化版最先进 ADKG 最高降低 40%。

**我的思考**：以"公开笔录+抽查验证"换掉昂贵的逐对 AVSS，复杂度降阶思路简洁而有效；256 节点全球实验证实了实用性，对区块链密钥管理与长期运行的门限系统意义重大。局限在于 κ 子集抽查带来概率性保证、实验对比对象为"简化版"最先进方案。该工作让异步密钥重构首次具备大规模部署可行性，是分布式密钥管理的基础设施级贡献。


#### Lattice-Based Threshold Blind Signatures.
- **作者**: Sebastian Faller, Guilhem Niot, Michael Reichle
- **方向**: 密码学与协议
- **解读**: 

**问题**：盲签名是隐私支付、匿名凭证的核心原语，但单一签名者的私钥一旦泄露即全面失守；门限化（多方共同签名）可分散风险，而后量子迁移又要求格基实现。此前尚不存在格基门限盲签名方案，且朴素组合（盲签名+门限签名）被证明不安全。

**方法**：以 AKSY 式格基盲签名为骨干，融合 Plover（隐私保护格基签名）与 DKM 式门限签名技术，精心设计交互协议使"门限性"与"盲性"同时成立；安全性在交互式 SIS 变体假设下证明，包括 Agrawal 等人（CCS 2022）的 One-More ISIS 及其弱化变体。

**结果**：给出首个格基门限盲签名方案：N 方门限签名、签名者视图仍保持盲性；在自然启发式假设下，签名大小较非门限格基盲签名仅增大 1.4–2.5 倍，开销温和；提供分布式部署实现并公开代码。

**我的思考**：填补格基门限盲签名的空白，且正视"朴素组合不安全"并给出整合式构造，证明扎实；对隐私现金、匿名凭证的后量子化有直接价值。局限在于交互式 SIS 假设属非标准假设、鲁棒性与 DoS 问题仅作讨论未完全解决。与经典 BBS 类门限盲签名相比，格基路线牺牲部分效率换取量子抗性，是后量子隐私密码的关键一步。


#### Mad-Dag: Protecting Blockchain Consensus From MEV.
- **作者**: Roi Bar Zur, Ittay Eyal, Aviv Tamar
- **方向**: 密码学与协议
- **解读**: 

**问题**：自私挖矿威胁区块链共识收入公平性，且在被 rushing（传播优势）、MEV（区块奖励随内容变化）与 petty-compliant（接受贿赂的矿工）等条件叠加后更为严峻；最先进的自私挖矿抵抗协议 Colordag 未处理这些条件，且仅在延迟高到不切实际时才被证明安全。

**方法**：提出 MAD-DAG（Mutually-Assured-Destruction DAG，相互确保毁灭式有向无环图）——首个在这些条件下实用的自私挖矿抵抗协议：设计新型账本函数，丢弃竞争成为最长链的等长链的内容，使偏离协议的收益归零；并在 Colordag 与 MAD-DAG 两种建模下用马尔可夫决策过程（MDP）分析理性矿工，通过保守奖励规则获得可解模型并给出利润上界。

**结果**：显著降低"为获利挖矿所需的最小算力份额"阈值，在比特币会失效的条件下依然安全，其余安全性与其他方案相当；例如高 MEV 波动下 MAD-DAG 阈值在 11%–31% 之间，而 Colordag 在所有水平下阈值均为 0%（即完全失效）。

**我的思考**：以"相互确保毁灭"的激励设计从共识层抵御 MEV 操纵，账本函数丢弃等长竞争链内容的思路新颖；MDP 建模给出可证利润上界，分析严谨。局限在于 DAG 共识的延迟假设与复杂实现、阈值区间随 MEV 波动变化需运营者关注。与 MEV 拍卖/排序器等应用层方案相比，共识层内建抵抗更根本，是 MEV 防护研究的重要方向。


#### Robot: Robust Threshold BBS+ in Two Rounds.
- **作者**: Guofeng Tang, Tian Qiu, Bowen Jiang, Haiyang Xue, Guomin Yang, Man Ho Au, Robert H. Deng, Kwok-Yan Lam
- **方向**: 密码学与协议
- **解读**: 

**问题**：BBS+ 匿名凭证广泛用于选择性披露的分散身份，但凭证签发集中于单一实体造成单点故障；门限 BBS+ 可分散签发权，但既有方案轮数偏多（Wong 等，NDSS 2024 为 4 轮）或缺乏鲁棒性（Tang & Xue，S&P 2025 为 3 轮），需要轮数最少且鲁棒的构造。

**方法**：提出 Robot——首个两轮鲁棒门限 BBS+ 方案：第 1 轮用基于 DDH 的门限可验证随机函数（TVRF）单轮生成公开随机数 nonce；第 2 轮用门限 Castagnos-Laguillaumie 与门限 ElGamal 同态加密处理剩余非线性计算；只要 t+1 个诚实方在线签名即可成功，每方上传通信为常数、计算随签名者数线性增长。

**结果**：与 Wong 等人（NDSS 2024，4 轮）相比，常数通信更小（2.02 KB vs 3.23 KB）且运行更快；与 Tang & Xue（S&P 2025，3 轮）相比，签名者数 ≥5 时通信与计算均更优，规模化表现良好。

**我的思考**：同时达到"最少轮数（2 轮）"与"鲁棒性"，DDH 型 TVRF 单轮生成 nonce 是关键技术巧思，规避了多轮同步问题；通信常数级、计算线性可扩展。局限在于依赖 Castagnos-Laguillaumie 类群与 ElGamal 同态的具体假设、门限安全模型下的合谋阈值需明确。对分散身份、可验证凭证的分布式签发是实用价值很高的进展。


#### New Constructions of Functional Adaptor Signatures: Broader Functions and Improved Efficiency.
- **作者**: Nikhil Vanjani, Garrett Greiner, Sri Aravinda Krishnan Thyagarajan, Pratik Soni
- **方向**: 密码学与协议
- **解读**: 

**问题**：功能适配器签名（FAS，CCS 2024）支持隐私保护、无需信任的原子化数据-支付交换（买家付费后才学到函数值），但此前仅支持线性函数 f(x)，无法覆盖数据分析、机器学习推理等非线性场景，应用面受限。

**方法**：给出基于同态加密的新 FAS 构造，支持次数 ≥2 的更高阶函数并提升效率；标准区块链交易仅需基础签名验证脚本即可集成，兼容主流链系统；概念层面提出通用的基于区块链的功能公平交换（FFE）协议范式，形式化买家与卖家公平性定义，并证明 FAS 蕴含 FFE 目标。

**结果**：即使在线性函数下也比 CCS 2024 构造快一个数量级；支持数据分析与 ML 推理所需的更高阶函数；但论文发布后注记指出 FHE 实例化的安全证明存在一个 bug，细节见论文。

**我的思考**：把 FAS 从线性推广到高阶函数使"数据市场"类应用成为可能，FFE 范式为公平交换提供了统一框架，概念贡献扎实；效率提升显著。但安全证明 bug 的自我披露值得警惕——FHE 实例化的可靠性需修复后再定论。与智能合约方案相比，FAS 路线避免链上计算成本，兼容性更好。该工作是区块链数据交易密码学的重要推进，但其可信度以证明修复为前提。


#### Practical Multi-Party Private Set Intersection with Reducible Zero-Sharing.
- **作者**: Yewei Guan, Hua Guo, Man Ho Au, Jiarong Huo, Jin Tan, Zhenyu Guan
- **方向**: 密码学与协议
- **解读**: 

**问题**：多方私有集合交集（mPSI，n≥3 方各持规模 m 的集合）是联邦分析、威胁情报共享的核心原语，但多方场景下计算与通信成本高、合谋抵抗难；既有 mPSI 在线阶段开销大，缺乏在任意合谋容忍下仍高效实用的方案。

**方法**：提出"可约化零共享"（reducible zero-sharing）新原语：允许 t（t<n）方对交集元素获得零份额，同时抵抗多达 t−1 方的合谋；基于同态 OKVS（不经意键值存储）构建离线/在线框架，在线阶段仅使用对称运算，实现 O((n+t)·m) 的总计算与通信复杂度。

**结果**：在线阶段总复杂度 O((n+t)·m) 且只用对称密码操作，半诚实模型下支持任意合谋子集抵抗（t−1 合谋安全）；LAN 与 WAN 环境下在线时间更快、通信更低，离线+在线总计较此前工作最高提速约 18.7 倍，同时安全性强于部分以效率见长的基线。

**我的思考**：可约化零共享把"零共享"从全方推广到任意子集，在线阶段纯对称运算+线性复杂度极具工程吸引力；18.7 倍的总提速说明并非牺牲离线成本换在线。局限在于半诚实模型、离线阶段仍依赖同态 OKVS 的预处理。与电路化 mPSI 相比，该方案在多方合谋抵抗与实用效率间取得更优平衡，对多方隐私计算落地有直接价值。


#### Single-Server Stateful PIR with Verifiability and Balanced Efficiency.
- **作者**: Pranav Shriram Arunachalaramanan, Ling Ren
- **方向**: 密码学与协议
- **解读**: 

**问题**：有状态单服务器 PIR 在客户端存储、计算与通信之间存在强权衡：客户端存储越少则计算越重，反之亦然；此前方案（如 Ren 等的 WR、SimplePIR 家族）偏向极端，缺乏兼顾三方资源的平衡设计，且可验证 PIR 的客户端存储开销巨大。

**方法**：提出 BALANCED-PIR 家族：以客户端存储与计算的平衡为目标设计状态化 PIR；进一步给出 Verifiable BALANCED-PIR，在保持亚线性摊销计算与小客户端存储的同时加入可验证性（检测恶意服务器并安全中止），成为首个亚线性摊销计算的可验证 PIR。

**结果**：对 100 万条 8 字节记录：客户端存储约 0.2 MB、摊销计算约 0.2 ms、摊销通信约 11.14 KB；摊销计算较同类存储方案快约 9 倍、初始离线计算改进约 40 倍，较 WR 方案摊销计算提升约 390 倍（同等存储下 50–60 倍）。可验证版本较 de Castro & Lee 方案客户端存储小约 100 倍（117 倍）、计算快 15–55 倍、通信优约 2.9 倍。

**我的思考**：系统性地把"平衡"确立为设计目标并给出量化权衡图谱，可验证版本以亚线性摊销成本获得可验证性，突破此前可验证 PIR 的存储瓶颈。局限在于单服务器模型下无验证时安全性仍依赖计算假设、有状态要求客户端维护状态。该工作填补了有状态 PIR 设计空间的关键空白，对实际部署（轻客户端）很有吸引力。


#### ZELDA: Efficient Multi-Server Preprocessing PIR With Unconditional Security.
- **作者**: Ashrujit Ghoshal, Mingxun Zhou, Bo Peng, Elaine Shi
- **方向**: 密码学与协议
- **解读**: 

**问题**：多服务器 PIR 无预处理时每次查询需线性工作量；引入预处理可降低在线开销，但此前预处理 PIR 要么依赖密码学假设、要么造成服务器存储膨胀；信息论安全（无条件安全）与高效预处理难以兼得。

**方法**：提出 Zelda（ZEro-Leakage Data Access）——客户端专属预处理的信息论多服务器 PIR：给出统一的参数化构造框架，在同一设置下渐近优于 Singh 等人（TCC 2024）与 Ishai 等人（CRYPTO 2024）；方案自包含，不依赖底层 PIR 构件，概念简单并开源实现。

**结果**：每次查询计算在服务器数 S 与数据库规模 n 上均为亚线性；完全无条件安全、无任何密码学假设；与 QuarterPIR（Eurocrypt 2024）相比在线响应更快、客户端空间更小，代价是离线维护成本更高；实验证明在线检索的实际收益。

**我的思考**：以"客户端专属预处理"同时获得信息论安全与亚线性查询，统一框架覆盖并超越两条先前工作线，理论贡献扎实；开源实现让结论可复现。局限在于多服务器非合谋假设与较高的离线维护开销——适合查询密集而预处理可摊销的场景。该工作为"无需假设的实用 PIR"提供了可行路线，是 PIR 理论与实践的桥梁。


#### Verifiable PIR with Small Client Storage.
- **作者**: Mayank Rathee, Keewoo Lee, Raluca Ada Popa
- **方向**: 密码学与协议
- **解读**: 

**问题**：可验证 PIR（vPIR）让客户端确信检索结果正确，但 VeriSimplePIR 等既有方案客户端需存储超过 1 GiB 的状态（针对 8 GiB 数据库），成为实际部署（浏览器、轻量钱包）的硬性障碍；如何在标准假设下大幅压缩客户端存储成为关键问题。

**方法**：提出 VeriSimplePIR++：基于新的可验证同态加密（vLHE）方案，仅依赖标准格假设（R-LWE 与 SIS）、无需随机预言机；在线状态与 LWE 参数 n 无关；引入查询级预处理以加速在线阶段并压缩每查询状态；并定义 covert vPIR（cvPIR）安全松弛——有状态客户端保持完整 vPIR 安全，无状态客户端也可获得 covert 安全（恶意服务器下）。

**结果**：8 GiB 数据库下客户端持久状态降至 512 KiB（较 VeriSimplePIR 改善约 2000 倍），在线短期状态低于 11 MiB；服务器吞吐较 VeriSimplePIR 慢约 2.5 倍，但查询级预处理使在线延迟相当；优化后 8 GiB 数据库在线延迟最高改善 8.5 倍；4 GiB 存储客户端可支持 300+ 个 PIR 应用（原仅数个）。

**我的思考**：把 vPIR 客户端存储从 GiB 级压到 KiB 级是量级突破，标准假设+免随机预言机的安全底座更稳健；cvPIR 对无状态客户端的务实松弛扩大了适用场景。局限在于服务器吞吐开销与 LWE 参数规模。该工作扫清了可验证 PIR 落地的最大障碍，对证书透明度、钱包隐私查询等场景有直接价值。


#### Euston: Efficient and User-Friendly Secure Transformer Inference with Non-Interactivity.
- **作者**: Xinwen Gao, Shaojing Fu, Lin Liu, Zhuotao Liu, Yuchuan Luo, Yongjun Wang
- **方向**: 密码学与协议
- **解读**: 

**问题**：基于全同态加密（FHE）的安全 Transformer 推理（STFI）可实现完全非交互式私有推理，但密文体积大、旋转操作多、乘法深度受限，用户端预处理与系统级推理开销巨大（如 NEXUS，NDSS 2025），离实用很远。

**方法**：基于 RNS-CKKS 设计 Euston：线性运算用奇异值分解（SVD）与批量同态矩阵乘法（HMM）压缩密文规模、减少旋转；非线性运算采用列（对角）打包密文格式避免昂贵旋转并降低深度，实现非交互式非线性求值（HNE）；提出四种 HMM 原语适配不同矩阵打包、离线处理掩码矩阵、深度调控策略抑制噪声增长以支持自举。

**结果**：相较 NEXUS（NDSS 2025），用户端预处理成本最高降低 3100 倍，系统级推理性能最高提升 8.8 倍，其中 HMM 提速约 90 倍、HNE 提速约 165.7 倍；提供 CPU（SEAL）与 GPU（PhantomFHE）双平台实现。

**我的思考**：同时攻克"非交互性"与"效率"两个 STFI 痛点，SVD 压缩线性层、打包格式免旋转的思路工程价值突出，3100 倍预处理降幅对用户体验至关重要。局限在于 SVD 截断引入近似误差、FHE 深度仍限制模型规模与序列长度。与交互式 MPC 方案相比，非交互性换来了部署简单性与可扩展性，是私有云推理的实用化里程碑。


#### InsPIRe: Communication-Efficient PIR with Server-Side Preprocessing.
- **作者**: Rasoul Akhavan Mahdavi, Sarvar Patel, Joon Young Seo, Kevin Yeo
- **方向**: 密码学与协议
- **解读**: 

**问题**：单服务器 PIR 的在线通信与吞吐是落地瓶颈：SimplePIR/DoublePIR 需客户端持有大体积离线提示，Piano 需流式传输整个数据库；如何在服务器侧完成预处理、把客户端在线成本降到最低是核心挑战。

**方法**：提出 InsPIRe：服务器侧"静默"预处理（客户端零离线通信），在线仅需低通信查询与高吞吐响应；核心技术 InspiRING——将 LWE 密文打包为 RLWE 密文的环打包算法，仅需 2 个密钥切换矩阵（此前方案需对数级），噪声增长更小、打包更快；二级 PIR 采用同态多项式求值，客户端仅多传一个密文。

**结果**：相较 YPIR，密码学密钥小约 5 倍、在线查询通信最高减少 50%、吞吐最高提升 25%；无需 SimplePIR/DoublePIR 的大离线提示，也无需 Piano 的全库流式传输；在多种数据库形态与应用（IPFS、私有设备注册）中验证，Rust 实现可支撑约 73 GB/24 亿条目的以太坊状态规模。

**我的思考**：把预处理完全移到服务器侧使客户端体验接近理想（零离线通信），InspiRING 的 2 个密钥切换矩阵是简洁而关键的理论改进。局限在于服务器预处理/存储成本、密钥切换材料的生成开销。与 SimplePIR 家族相比，该方案以服务器侧成本换客户端轻量化，方向契合"胖服务器、瘦客户端"的现实部署趋势，是通信最优 PIR 的代表性工作。

#### LatORAM: ORAMs from Lateral Stashes and Delayed Shuffling.
- **作者**: Sarvar Patel, Giuseppe Persiano, Joon Young Seo, Kevin Yeo
- **方向**: 密码学与协议
- **解读**: 
不经意随机访问机（ORAM）是隐藏程序内存访问模式的基础原语，广泛服务于安全处理器、外包数据库与隐私计算场景。传统ORAM依靠频繁洗牌（shuffle）与分层结构掩盖访问模式，但洗牌操作开销大、常数因子高，实际部署成本昂贵；如何在保持安全性的前提下降低ORAM的摊销带宽与常数开销是长期未解的难题。

据标题推断，LatORAM提出基于"侧向暂存区（lateral stashes）"与"延迟洗牌（delayed shuffling）"的新型ORAM构造：将待洗牌数据暂时存放于侧向暂存区，推迟并合并洗牌时机，从而摊薄洗牌成本、降低每次访问的摊销开销，并配套相应的安全性分析以证明其不泄露访问模式。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文的贡献应集中于在既定ORAM安全定义下优化常数因子与带宽效率，具体性能提升幅度需以全文实验数据为准。

Patel等作者在ORAM与PIR领域积累深厚，该工作延续其追求实用常数开销的研究路线，把最重的洗牌操作的调度方式作为切入点，思路新颖。局限在于暂存区与延迟洗牌会引入额外状态与实现复杂度，收益可能依赖特定访问模式；与同期分层ORAM方案相比，其优势应体现在更低的摊销带宽与更小的客户端存储上，值得以全文数据验证。


#### VIA: Communication-Efficient Single-Server Private Information Retrieval.
- **作者**: Chenyang Liu, Xukun Wang, Zhifang Zhang
- **方向**: 密码学与协议
- **解读**: 
私密信息检索（PIR）允许用户从服务器数据库获取记录而不泄露查询内容，是隐私保护数据访问的基础工具。单服务器PIR安全假设最干净、不依赖多方不共谋假设，但通常依赖全同态加密等重密码工具，通信与计算开销大，成为其实际部署的最大障碍；降低通信复杂度是单服务器PIR研究的核心目标。

据标题推断，VIA提出通信高效的单服务器PIR方案，核心思路应是在查询编码与响应压缩层面进行优化，例如采用更紧凑的查询表示、向量化/批量处理技术或改进的同态打包方式，使传输字节数相比现有方案显著下降，同时尽量保持可接受的计算开销与标准安全假设。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其宣称的"通信高效"应体现在更低的查询与响应大小上，具体数值需以论文实验为准。

单服务器PIR的通信量直接影响其在真实数据库场景（证书吊销、广告定向、隐私检索）中的可用性，VIA若能在不牺牲计算效率的前提下压缩通信，是扎实的增量贡献。作者具有密码理论背景，方案安全性应建立在标准困难假设之上；与多服务器PIR相比，单服务器路线免去共谋假设，更贴近云服务现实。后续值得关注其与批量PIR、PIR-PSI等应用的结合效果。


#### Automated Formal Analysis of Signal's Double Ratchet: Attacks, Fixes and Security Proofs.
- **作者**: Vincent Cheval, Charlie Jacomme, Jessica Richards
- **方向**: 密码学与协议
- **解读**: 
Signal协议是端到端加密通信的事实标准，其核心构件Double Ratchet（双棘轮）机制在前向安全与后向安全之间取得平衡，被WhatsApp、Signal等数十亿用户使用。该机制状态机复杂、密钥派生链条交错，安全性长期依赖手工论证，任何被忽视的边界情形都可能转化为真实攻击，自动化检验其安全性意义重大。

据标题推断，论文利用自动化形式化分析方法对Signal Double Ratchet的完整状态机进行系统检验，在分析中发现可被利用的攻击（attacks），据此提出修复方案（fixes），并对修复后的协议给出安全证明（security proofs），形成"攻击—修复—证明"的完整闭环。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文的产出应包括具体攻击场景、相应修复以及自动化/机械化验证的安全性质，细节以全文为准。

该工作体现形式化验证在真实世界密码协议上的价值：人工设计的高复杂度状态机正是自动化分析最能发挥作用的领域。Cheval与Jacomme在协议验证（TLS、Signal等）方向有系列成果，其攻击发现的可靠性值得信赖。局限在于自动化模型对攻击者能力的抽象可能遗漏现实实现层面的问题；若能结合实现代码验证将更有说服力，但"证明驱动修复"的示范意义已经明确。


#### Towards Practical Zero-Knowledge Proof for PSPACE.

- **作者**: Ashwin Karthikeyan, Hengyu Liu, Kuldeep S. Meel, Ning Luo

- **方向**: 密码学与协议

- **解读**: 高效零知识证明（ZKP）此前几乎全部局限于 NP 陈述，而 PSPACE 类中存在大量实际相关的计算问题（如博弈、规划、量化推理），却长期缺乏实用协议。ZKP 是隐私与可验证性的基础工具，把高效证明能力扩展到 PSPACE 类陈述具有重要理论意义与应用价值。

论文提出首个面向 PSPACE 完全问题的实用 ZK 协议，通过对 QBF（量化布尔公式）求值进行零知识证明实现。核心思想是在 ZK 中验证量化消解证明（Q-Res）：作者设计了 Q-Res 证明的多项式级编码，使证明验证归结为低开销的算术检查；此外还设计了证明"掌握与 QBF 相关的必胜策略"的 ZK 协议，这在实践中往往同样关键。

在 QBFEVAL 基准上，对可得证明或策略的实例，协议可在 100 秒内验证 72% 的 QBF 求值（经 Q-Res 证明）以及 82% 实例的必胜策略；QBFEVAL'07 中来自 PEC、C-Plan、BBC 的实例分别约在 300、1,200、200 秒内完成验证。

我的思考：这是 ZKP 从 NP 迈向 PSPACE 的里程碑式一步，"按子句大小分组的分层编码+量词编码"是让证明验证可行的关键工程技巧，必胜策略证明也直击博弈类应用痛点。局限在于 Q-Res 证明理论上可指数增长，方案效率依赖实际实例存在短证明；策略可表示性也限制其适用范围。它为博弈、规划等 PSPACE 场景的隐私可验证计算开辟了道路。

- **arXiv PDF**: [Towards Practical Zero-Knowledge Proof for PSPACE.](https://arxiv.org/pdf/2511.15071)

#### Coral: Fast Succinct Non-Interactive Zero-Knowledge CFG Proofs.
- **作者**: Sebastian Angel, Sofía Celi, Elizabeth Margolin, Pratyush Mishra, Martin Sander, Jess Woods
- **方向**: 密码学与协议
- **解读**: 
零知识证明（ZK）让证明者在不泄露秘密的前提下证明语句成立，但传统ZK电路面向逐字节计算，难以高效表达程序的控制流（分支、循环等）。若能把控制流图（CFG）本身纳入证明，将大幅提升程序级ZK应用的效率；但CFG证明的简洁性、非交互性与生成速度长期难以兼顾。

据标题推断，Coral构造快速、简洁、非交互的零知识CFG证明：将程序的控制流结构直接编码进证明系统，使证明规模与验证时间随电路规模而非执行轨迹膨胀，并针对CFG特性设计高效的证明协议，在保持零知识性的同时实现快速的证明生成与验证。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告证明大小、证明与验证时间及与同类系统的对比，具体数据以全文为准。

Angel与Mishra均为ZK领域知名研究者（Mishra参与Spartan等方案），该工作直击ZK在程序语义表达上的结构性短板；若CFG证明的简洁性成立，可显著改善ZKP在智能合约审计、可验证计算等场景的可用性。局限在于非交互与简洁性通常伴随较重的信任假设或预计算成本，需仔细权衡；与电路式ZK相比，CFG式证明的通用性与工程成熟度有待检验。


#### Dory: Streaming PCG with Small Memory.
- **作者**: Xiaojie Guo, Hanlin Liu, Zhicong Huang, Hongrui Cui, Wenhao Zhang, Cheng Hong, Xiao Wang, Kang Yang, Yu Yu
- **方向**: 密码学与协议
- **解读**: 
伪随机相关生成器（PCG）让多方在不交互的前提下预生成大量随机相关（如乘法三元组、OT扩展），是高效安全多方计算（MPC）的基石；但现有PCG要么内存占用巨大（一次性生成并存储全部相关），要么交互复杂，制约了其在资源受限或超大规模场景下的应用。

据标题推断，Dory提出流式（streaming）PCG方案：以恒定小内存边生成边消费随机相关，避免大规模存储开销；核心思路应是将相关生成过程分解为可增量计算的流，配合高效的种子扩展与验证机制，在保持安全性的同时把内存需求压至极小，并给出相应的安全性证明。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告内存占用、生成与消费速率及端到端MPC开销的对比数据，具体数据以全文为准。

该团队汇聚MPC与PCG领域的核心研究者（Xiao Wang、Kang Yang、Yu Yu等），PCG的小内存化直接拓宽其应用边界——从服务器集群到嵌入式设备都能受益，实用价值显著。流式化在密码学方案中常带来安全证明与协议设计的双重挑战，若能同时解决，贡献扎实。局限在于流式生成可能与某些需要随机重排的MPC协议不完全兼容；值得关注其与具体MPC框架的集成效果。


#### CAVERN: Efficient Honest-Majority Maliciously Secure (2+1)-PC for $\mathbb{Z}_{2^{n}}$ via DPF.
- **作者**: Yang Liu, Liang Feng Zhang
- **方向**: 密码学与协议
- **解读**: 
三方安全计算（3PC）在诚实多数假设下兼具效率与安全，是实际部署MPC的热门选择；但多数3PC方案面向布尔域或素数域，对$\mathbb{Z}_{2^n}$（模2的幂整数环，贴近真实计算机算术）的支持不足，且恶意安全（malicious security）版本往往以牺牲效率为代价，如何高效实现恶意安全的$\mathbb{Z}_{2^n}$三方计算是开放问题。

据标题推断，CAVERN利用分布式点函数（DPF）构建诚实多数下恶意安全的(2+1)方计算方案：以DPF作为核心原语实现秘密分享、乘法与验证的高效组合，使恶意安全版本的通信与计算开销逼近半诚实基线，并针对$\mathbb{Z}_{2^n}$算术进行专门优化，同时给出严格的安全性证明。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告恶意安全(2+1)-PC在$\mathbb{Z}_{2^n}$上的通信与计算开销及其与半诚实基线和现有恶意安全方案的对比，具体数据以全文为准。

环$\mathbb{Z}_{2^n}$上的MPC贴近真实程序语义（整数运算、无溢出处理），是连接密码协议与实际应用的桥梁；DPF此前多用于ORAM与PIR，将其作为3PC的核心原语思路新颖。局限在于DPF的生成与求值开销可能成为瓶颈，且诚实多数假设限制了其对抗模型；该工作为"以现代原语重铸经典MPC"提供了范例，工程落地价值可期。


#### Sort, Sweep, Mirror: Batch Private Interval Lookup with Logarithmic Cost.
- **作者**: Andes Y. L. Kei, Lucien K. L. Ng, Jack P. K. Ma, Sherman S. M. Chow
- **方向**: 密码学与协议
- **解读**: 
私有区间查找（private interval lookup）允许一方查询落在另一方位点或区间集合内的数据而不泄露查询与数据本身，是隐私保护的定位服务、订阅管理、数据匹配等应用的基础构件；批量场景下其开销通常随数据规模线性增长，通信与计算成本高昂，亟需更高效的构造。

据标题推断，论文提出"Sort, Sweep, Mirror"三阶段方法实现批量私有区间查找：通过排序建立有序结构、扫描生成匹配关系、镜像变换进行对称化处理，将批量查找的成本压缩到对数级（logarithmic），并给出相应的安全性证明与实现方案。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告批量区间查找的通信与计算复杂度对比及具体实验数据，具体数据以全文为准。

把区间查找从线性成本降到对数成本是明显的复杂度改进，对大规模隐私数据匹配场景意义重大；作者团队（Sherman Chow）在隐私保护协议方向积累深厚。局限在于对数成本可能以更复杂的预处理或更强的假设为代价，且常数因子与并行化能力需实测验证；该工作为隐私数据匹配族提供了新的效率标尺，值得关注其后续扩展与工程化。


#### Vega: Low-Latency Zero-Knowledge Proofs over Existing Credentials.
- **作者**: Darya Kaviani, Srinath Setty
- **方向**: 密码学与协议
- **解读**: 
数字凭证（如OIDC ID令牌、JWT、通行密钥）是网络身份认证的基础，用户常需向服务方证明"我持有满足条件的凭证"；传统做法直接提交凭证，泄露不必要信息。用零知识证明隐藏凭证细节（如只证明年龄达标而不泄露生日）能增强隐私，但现有证明方案对凭证格式适配差、延迟高，难以满足认证场景的低延迟要求。

据标题推断，Vega面向现有凭证体系设计低延迟零知识证明：直接对现实凭证（JWT、OIDC等）的签名验证与声明检查构建证明电路，优化证明生成与验证路径，使客户端在低延迟内完成证明、服务端快速验证，实现"凭证可验证但内容不泄露"，并给出与现有方案的对比分析。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告端到端证明延迟、证明大小及对主流凭证类型的支持情况，具体数据以全文为准。

Setty是ZK领域权威（Spartan等方案作者），Kaviani长期研究凭证场景的ZK应用；"对现有凭证开箱即用"是该工作的差异化价值——多数ZK方案需要专门设计的数据格式，难以融入存量认证体系，低延迟目标直击认证场景的实时性约束。局限在于凭证签名的验证电路可能较大，且需信任凭证颁发者的公钥基础设施；该工作对隐私身份认证的落地具有直接推动意义。


#### Practical Anonymous Two-Party Gradient Boosting Decision Tree.

- **作者**: Chenyu Huang, Fan Zhang, Minxin Du, Sherman S. M. Chow, Huangxun Chen, Huaming Rao, Danqing Huang, Bo Qian, Peng Chen
- **方向**: 密码学与协议
- **解读**:
梯度提升决策树（GBDT）擅长处理结构化数据，在金融、医疗等场景广受欢迎，通常需要在互不信任的双方之间、基于垂直切分的特征训练模型。安全计算为此带来独特挑战，需要记录对齐与比较；现有方案普遍依赖私有集合交集（PSI），但其所谓的安全措施反而会暴露双方数据集共享的标识符（ID）。

作者发起“匿名 GBDT 训练”研究：双方交替作为接收方，在本地特征上执行 pick-then-sum 操作；借助不经意可编程伪随机函数（OPPRF）在多次运行间传播输出与状态；避免通用对齐，解决了“隐藏 ID 的开销随域大小增长”这一难题；并将基于（环）LWE 的单指令多数据（SIMD）同态加密的密文打包开销减半（改进自 USENIX Security '23 相关工作）。

对比实验表明，协议在效率上仍与有泄露（leaky）的方案相当，同时实现了 ID 隐藏聚合；所提技术还可扩展到其他安全分析任务。摘要未给出具体量化数值。

该工作直击 MPC 实践中“安全对齐反而泄露身份关联”的矛盾，把“匿名训练”确立为可研究的性质，是隐私计算的务实推进；OPPRF 加 SIMD 打包优化体现了扎实的工程技巧。局限在于仅覆盖两方、半诚实模型，多方扩展性与域规模开销仍需观察；与基于通用 MPC 的训练方案的系统对比有待展开。


#### Private Data Imputation.

- **作者**: Addelkarim Kati, Florian Kerschbaum, Marina Blanton
- **方向**: 密码学与协议
- **解读**:
数据插补（data imputation）是重要的数据准备任务：分析师用估计值替换缺失或错误值，以提高下游分析的期望精度。当分析跨分布式数据库、需要保护隐私时，现有方法会破坏隐私，使隐私保护形同虚设；因此插补必须在私有计算的前提下完成。

论文给出首批面向水平切分与垂直切分数据集的优化私有插补协议。优化目标是削减计算量最大的集合交集（或至少是不经意可编程伪随机函数 OPPRF）部分——后者可以被非常高效地计算。

在所有评估数据集上，平均优于本地插补约 20%（部分设置约 5%）；最差情形仍显著更优（提及最高 32.7 倍、3.4 倍等质量差异数字）；在 10 Gbps 网络下处理 10 万条记录仅需 2.4–8.4 秒。摘要文本残缺，部分数字的具体含义需以全文为准。

该工作把“插补必须先于隐私分析”这一常被忽视的前置环节拉回研究视野，是首批系统优化私有插补的协议工作；复用 OPPRF/PSI 的成熟底座控制开销，务实可行。局限在于摘要残缺、具体安全假设（双方、半诚实）与扩展性需全文确认；对多方与恶意安全模型仍有拓展空间。


#### Fast Deterministically Safe Proof-of-Work Consensus.

- **作者**: Ali Farahbakhsh, Giuliano Losa, Youer Pu, Lorenzo Alvisi

- **方向**: 密码学与协议

- **解读**: 无许可区块链共识面临范式性两难：PoS 协议易受长程攻击（廉价购买旧密钥伪造历史），只能依赖社交共识等外部机制；PoW 协议要么只提供概率性安全（如比特币的运气分叉），要么速度过慢。能否同时获得确定性安全、常数期望时延且不依赖外部机制，此前悬而未决。

论文将 PoS 协议 MMR 移植到 PoW 设置，得到 Sieve-MMR：继承 MMR 的常数期望时延与确定性安全，PoW 天然抵御长程攻击。移植的核心障碍是作者提出的"时间旅行攻击"——攻击者用远古生成的 PoW 夸大当前算力；为此提出 Sieve 算法实现新广播原语 TTRB（抗时间旅行广播），其基于黑盒、确定性的 PoW 原语 DPoW（固定次数哈希计算，类似无需串行执行的 VDF），作为 MMR 的消息层，包含处理在线节点的 Online-Sieve 与处理新激活节点的 Bootstrap-Sieve。

这是首个完全无许可、确定性安全、常数期望时延且不依赖外部机制的 PoW 总序广播协议；作者提供 PlusCal/TLA+ 规范与完整的正确性证明，安全假设为攻击者控制的算力严格少于一半。属于理论协议设计，论文以形式化证明而非实测吞吐作为验证。

我的思考：突出贡献是"把共识逻辑与无许可参与的副作用控制解耦"的设计原则，使既有低时延共识协议可安全部署到无许可环境，思路简洁深刻。局限：依赖确定性 PoW 原语与算力多数假设，实用化还需工程实现；缺乏实测性能数据，常数期望时延的实际常数有待验证。该原则可能启发其他更强模型下协议的"无许可化"移植。

- **arXiv PDF**: [Fast Deterministically Safe Proof-of-Work Consensus.](https://arxiv.org/pdf/2512.19968)

#### Shared Spotlight Meridian: Distributed Sparse Pseudorandom Functions for Scalable Federated Learning.

- **作者**: Youlong Ding, Peihua Mai, Jingqi Zhang, Sherman S. M. Chow, Minxin Du, Yan Pang
- **方向**: 密码学与协议
- **解读**:
联邦学习（FL）规模化面临两难：安全聚合可保护梯度隐私，但通信与计算开销大、对参与方掉线敏感；而高效可扩展的聚合需要更轻量的密码学原语，现有原语在带宽与容错上难以兼得。

据标题推断，论文提出分布式稀疏伪随机函数（distributed sparse PRF）：多服务器协同生成稀疏（仅少数位置非零）的伪随机输出，用于 FL 中的掩码与份额生成，在保持安全性的同时显著降低通信与计算负担，支撑可扩展的联邦学习聚合。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重心是把稀疏性引入分布式 PRF 以换取 FL 的可扩展性，并论证其安全性。

稀疏 PRF 的设计同时削减带宽并保留伪随机性，方向切中 FL 聚合瓶颈；与 secure aggregation、分布式点函数（DPF）等原语的对比是评估贡献的关键。局限在于多服务器信任假设，以及稀疏掩码对隐私保证（如对抗合谋）的影响需要严谨论证与实验支撑。，为大规模 FL 系统提供可落地的密码学底座。


#### Jigsaw: Doubly Private Smart Contracts.

- **作者**: Sanjam Garg, Aarushi Goel, Dimitris Kolonelos, Rohit Sinha
- **方向**: 密码学与协议
- **解读**:
区块链智能合约的公开可验证性常以牺牲隐私为代价：交易输入与合约状态对所有人可见，阻碍了拍卖、金融、身份等敏感应用上链；已有隐私方案多只隐藏输入，状态仍持续泄露。

据标题推断，Jigsaw 实现“双重隐私”（doubly private）智能合约：同时隐藏交易输入（输入隐私）与合约状态（状态隐私），支持以私有状态执行的通用合约逻辑，并在此之上维持可验证性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其目标是把隐私保护从“单笔交易”扩展到“连续执行的合约状态”。

“双重隐私”直击此前方案只保护输入、状态仍泄露的缺口，作者团队在 MPC/零知识方向积累深厚，构造可信度较高；通用私有状态的性能代价与合约开发复杂度是现实障碍，落地性取决于与 ZK-Rollup 系、私有状态链等专用方案的效率对比，其安全模型（如对恶意合约、重放攻击的防护）也需在全文给出严谨论证。


#### CHORUS: Secret Recovery with Ephemeral Client Committees.

- **作者**: Deevashwer Rathee, Emma Dauterman, Allison Li, Raluca Ada Popa
- **方向**: 密码学与协议
- **解读**:
秘密恢复（如密码管理器主密码恢复、密钥托管）通常依赖集中式服务器或固定阈值机构，一旦被攻破或合谋即威胁全部秘密；同时用户设备丢失时又必须存在可信的恢复途径，两者难以兼得。

据标题推断，CHORUS 提出用“临时客户端委员会”（ephemeral client committees）实现秘密恢复：由一组短暂存在的客户端共同持有并轮换秘密份额，摆脱对单一可信服务器或固定委员会的依赖，新成员加入后旧份额即失效，兼顾可用性与前向安全。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其创新在于把恢复流程的信任模型从静态机构转向动态、短命的客户端集合。

把轮换（proactive）与去中心化思路引入恢复场景，能显著降低长期托管的单点风险，与 Popa 组此前在备份/恢复方向的工作一脉相承；局限在于临时委员会对设备在线率、通信同步的现实约束，以及恢复流程可用性与安全性的平衡仍需系统实证。


#### Efficient Fuzzy Private Set Intersection from Secret-Shared OPRF.

- **作者**: Xinpeng Yang, Meng Hao, Chenkai Weng, Robert H. Deng, Yonggang Wen, Tianwei Zhang

- **方向**: 密码学与协议

- **解读**: 私有集合交集（PSI）让双方在不泄露非交集元素的前提下计算交集；模糊 PSI（FPSI）进一步允许接收方得知发送方集合中距离不超过阈值的元素，适用于近邻匹配、基因组相似度等场景。既有面向 L_p 距离的 FPSI 要么重度依赖昂贵的同态加密，要么复杂度随元素维数指数增长，实用性差。

论文提出面向 L_{p∈[1,∞]} 距离的高效 FPSI 协议，主要依赖更廉价的对称密钥操作：核心构件是带秘密共享输出的不经意可编程 PRF（OPRF），可独立复用；并引入前缀（prefix）技术，把对距离阈值 δ 的依赖从线性降到对数，特别适合大 δ 场景。

协议在集合规模 m、n、维数 d 与阈值 δ 上均达到线性通信与计算复杂度（前缀技术后 δ 项为对数）。实现对比显示：比 Gao 等（ASIACRYPT'24）快 12–145 倍、通信减少 3–8 倍；比 Dang 等（CCS'25）快 9–80 倍、通信减少 5–19 倍，在所有设置下均一致显著优于现有工作。

我的思考：用对称密钥路线整体取代同态加密路线并获得数量级加速，方向性正确且工程完整；可独立使用的秘密共享输出 OPRF 构件具有独立价值，前缀技巧也优雅地化解了大阈值难题。局限：协议效率与距离度量、参数选择耦合较紧，真实网络环境下的端到端验证仍需补充。该工作显著推进了 FPSI 的实用化进程。

- **arXiv PDF**: [Efficient Fuzzy Private Set Intersection from Secret-Shared OPRF.](https://arxiv.org/pdf/2604.14909)

#### Weighted Batched Threshold Encryption With Applications to Mempool Privacy.

- **作者**: Amit Agarwal, Kushal Babel, Sourav Das, Babak Poorebrahim Gilkalaye, Arup Mondal, Benny Pinkas, Peter Rindal, Aayush Yadav
- **方向**: 密码学与协议
- **解读**:
区块链交易的 mempool（交易池）公开可见，攻击者可据此抢跑（front-running）、三明治攻击或窥探交易意图；隐藏 mempool 需要支持“延迟解密”的加密方案——交易先入池、到点后再解密执行，对解密效率与公平性要求很高。

据标题推断，论文提出加权批量阈值加密（weighted batched threshold encryption）：在阈值解密框架中引入权益权重与批量处理，使不同参与者按权益加权参与解密、多笔交易批量解密，提升效率与公平性，适配 PoS 公链的 mempool 隐私场景。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其贡献在密码学原语的效率与表达力（权重、批量），而非单纯的理论构造。

mempool 隐私是 MEV（最大可提取价值）问题的核心对策之一，阈值加密（TARDIS 系）已成为主流技术路线；权重化与批量化贴近 PoS 权益结构，工程价值明确。局限在于解密延迟与用户体验、密钥分发可用性；与可验证延迟函数（VDF）等替代路线的取舍值得比较。，为公链部署提供更贴近权益结构的加密基础设施。


#### GoSSamer: Lightweight and Linear-Communication Asynchronous (Dynamic Proactive) Secret Sharing and the Applications.

- **作者**: Xinxin Xing, Yizhong Liu, Boyang Liao, Jianwei Liu, Bin Hu, Xun Lin, Yuan Lu, Tianwei Zhang
- **方向**: 密码学与协议
- **解读**:
异步环境下的秘密共享（如异步 MPC、去中心化存储）面临成员动态变化与长期秘密泄露风险；主动安全（proactive）要求周期性轮换份额，但已有方案通信开销大或依赖同步假设，难以在完全异步网络中高效落地。

据标题推断，GoSSamer 提供轻量级、线性通信的异步（动态主动）秘密共享：在完全异步网络中支持成员动态加入/退出与份额刷新，通信复杂度线性于参与方数量，兼顾动态性与前向安全性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；核心卖点是“轻量+线性通信+异步动态”三者的组合。

异步主动秘密共享是去中心化系统（如 DKG、异步 MPC）的重要底层原语，线性通信对可扩展性至关重要；作者团队在异步协议领域有持续积累。局限在于线性通信的常数因子、并发重共享的正确性证明复杂度，以及与 HoneyBadgerMPC、ACS 等系统的集成效果待观察。，为其在去中心化基础设施中的部署创造条件。


#### UltraProofs: Scalable Reed-Solomon Code Commitment.

- **作者**: Yanpei Guo, Alex Luoyuan Xiong, Wenjie Qu, Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**:
多项式与编码承诺是 SNARK 与可验证存储的核心原语；Reed-Solomon（RS）码承诺具备抗擦除、线性编码等优良性质，但通用方案的承诺与证明开销随数据规模增长显著，制约其在大规模场景的实际部署。

据标题推断，UltraProofs 提出可扩展的 RS 码承诺方案：优化承诺生成与求值证明的复杂度（如利用 FRI 类技术或新的编码结构），使大尺寸数据上的承诺与证明更高效，支撑大规模可验证计算与存储证明。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其目标是以更低的渐近与常数开销支撑 RS 承诺的规模化使用。

RS 承诺是 FRI、STIR 等前沿证明系统的基础，其效率直接影响 SNARK 性能；若承诺端与证明端同时改善，对零知识证明与存储证明（如 Filecoin 系）领域都有价值。局限在于无摘要难辨具体构造与安全假设，与 Orbit、BaseFold 等工作的对比是评估其贡献的关键，实际部署中的常数因子与并行化能力同样决定其竞争力。


#### Scalable Registration-Based Encryption from Lattices.

- **作者**: Michael Klooß, Russell W. F. Lai, Jan Niklas Siemer, Monisha Swarnakar
- **方向**: 密码学与协议
- **解读**:
基于注册的加密（RBE，registration-based encryption）试图去除公钥基础设施与群签名中的可信设置：用户仅通过向注册表登记公钥即可获得加密能力，无需全局主密钥；但已有构造多基于配对，效率有限、参数规模随注册人数增长不理想。

据标题推断，论文给出基于格（lattice）的可扩展 RBE：利用格上工具（如函数加密、同态累加、紧凑证明）构造注册表规模增长下仍高效的密文与密钥，实现可扩展性并天然具备抗量子安全性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；贡献在于把 RBE 从配对假设推进到格假设，并改善其扩展性。

RBE 是“无可信设置”密码学的重要一步，格基构造同时带来抗量子属性，符合后量子时代需求；可扩展性（注册表增长下的开销）是 RBE 落地的关键指标。局限是格上方案效率通常低于配对方案，常数因子与安全参数的实际表现需要基准测试佐证。，为后量子时代无可信设置的加密体系铺路。


#### From Perfect to Approximate Hints: Efficient LWE Secret Recovery Leveraging Low Hamming Weight.

- **作者**: Minki Hhan, Ga Hee Hong, Jiseung Kim, Changmin Lee, JeongHwan Lee
- **方向**: 密码学与协议
- **解读**:
LWE 假设是后量子密码（如 Kyber、Dilithium）的基石，其安全性依赖错误分布与秘密结构；当秘密具有低汉明重量（少量非零比特）时，攻击者可能获得结构性优势，威胁具体方案的安全裕度。

据标题推断，论文研究“提示”（hints）下的 LWE 秘密恢复：把攻击从完美提示推广到更贴近现实的近似提示，利用低汉明重量秘密的结构，显著提升求解效率（如通过格基约简、组合/碰撞类攻击），并评估此类结构化秘密对后量子方案的影响。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；核心是量化“近似提示+低重量”对 LWE 求解的加速效果。

该工作属于密码分析侧研究：评估结构化秘密与旁路信息（提示）对 NIST 后量子标准方案安全参数的影响，有直接参考价值；“完美→近似”的推广使攻击模型更贴近真实泄露场景。局限在于无摘要难辨攻击适用的参数域与复杂度结论；若影响有限，则属负面结果型贡献，同样有学术意义。


#### Single-Server Private Outsourcing of zk-SNARKs.
- **作者**: Kasra Abbaszadeh, Hossein Hafezi, Jonathan Katz, Sarah Meiklejohn
- **方向**: 密码学与协议
- **解读**: zk-SNARK 证明生成计算开销大，资源受限的证明者希望把证明外包给服务器，但服务器需访问 witness——这会泄露隐私。如何把证明任务外包给单个（不可信）服务器而不泄露 witness 与输入，是"私有外包"的核心问题。（无摘要，据标题推断）

据标题推断，方案结合同态加密/安全计算技术与 SNARK 结构：证明者以加密形式提供 witness，服务器在密文上执行证明生成所需的部分计算，或通过盲化/可验证委托协议使服务器在不知晓 witness 的情况下协同完成证明。

摘要缺失，无法引用量化结果；预期包含协议的安全性证明（恶意/半诚实模型）与相对本地证明的计算开销对比。

我的思考：Katz 与 Meiklejohn 都是密码学领域权威，该问题设定切中 zk 应用的现实瓶颈——移动端/轻客户端无法承担证明成本，"单服务器"设定比多服务器 MPC 更易部署，但需用更精巧的密码工具弥补信任缺口。关键看点是安全模型（服务器是否恶意、能否学习 witness 的部分信息）与端到端开销；与"分布式证明""证明委托"等既有路线的关系待正文阐明。无摘要使具体构造与效率数字待确认。


#### APEX: Accurate Parallel Expressive Homomorphic Execution for Encrypted Databases.
- **作者**: Wei Chen, Qi Hu, Siu-Ming Yiu, Heming Cui
- **方向**: 密码学与协议
- **解读**: 加密数据库把查询执行下沉到同态加密（HE）之上以保护数据机密性，但 HE 运算昂贵、支持的操作表达力有限（难以表达复杂 SQL 语义），且并行化困难，三者共同制约了 HE 数据库的实用化。（无摘要，据标题推断）

据标题推断，APEX 面向加密数据库提供"准确、并行、表达力强"的同态执行引擎：设计高表达力的密文运算原语以覆盖更多 SQL 操作（比较、聚合、连接等），通过数据/任务级并行与 SIMD 化利用多核硬件，并保证计算准确性（如正确处理舍入与溢出）。

摘要缺失，无法引用量化结果；预期包含在标准数据库基准（如 TPC-C/TPC-H 类查询）上相对既有 HE 数据库系统在延迟、吞吐与准确率上的对比。

我的思考：HE 数据库的"三角困境"（准确-并行-表达力）是领域公认瓶颈，APEX 若能在三端同时推进，将显著提升同态查询的实用性。Heming Cui 团队在系统软件与并行计算上积累深厚，工程实现质量值得期待。关键评价点在于：支持的查询类别是否覆盖真实负载、并行加速的扩展性（多核/多机），以及其安全参数下的实际开销；无摘要使这些数字待正文确认。

#### Secure Lookup Tables: Faster, Leaner, and More General.
- **作者**: Chongrong Li, Pengfei Zhu, Yun Li, Zhanpeng Guo, Jingyu Li, Yuncong Hu, Zhicong Huang, Cheng Hong
- **方向**: 密码学与协议
- **解读**: 安全查找表（secure lookup）是隐私计算的基础构件——在多方之间以秘密分享或密文形式执行"按键取表"，广泛服务于隐私集合运算（PSI）、私有信息检索与联邦统计等场景；但现有实现往往在通信、存储或通用性上各有短板，难以同时满足高效、紧凑与场景普适的要求。（无摘要，据标题推断）

据标题推断，本文对安全查找表协议做系统改进，以"更快、更省、更通用"为目标：核心思路应是利用函数秘密共享（FSS）/分布式点函数等现代密码原语压缩查找表表达，或优化底层不经意传输与预计算流程，在吞吐、通信与离线存储上同时取得收益，并把构造推广到更宽泛的表结构与查询模式。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；按标题推断，论文应报告相对既有最优方案在查找延迟/吞吐、通信量与状态规模上的提升，并给出多种参数域下的对比。（无摘要，据标题推断）

我的思考：查找表是众多隐私协议共用的"积木"，其效率改进具有乘法式放大效应；作者团队（Ant Group 等）有工业落地背景，方向直指实际瓶颈。局限在于无摘要，具体机制与收益需以正文为准；"更通用"的宣称也需检验是否以牺牲最坏情况开销为代价。（无摘要，据标题推断）


#### Efficient Arithmetic-and-Comparison Homomorphic Encryption with Space Switching.

- **作者**: Erwin Eko Wahyudi, Yan Solihin, Qian Lou

- **方向**: 密码学与协议

- **解读**: 全同态加密（FHE）可在不解密的情况下计算加密数据，是隐私保护计算的核心，但没有任何现有方案能在统一框架内高效支持算术与比较两类运算：词级方案（BFV/BGV/CKKS）擅长算术，位级方案（FHEW/TFHE）擅长比较；方案切换（scheme switching）对大型输入开销过高，多项式近似则在临界点附近引入误差，难以用于精度敏感任务。

论文提出空间切换（space switching）方法，在 FV 风格（BGV）方案内无缝整合两类运算：核心观察是两类运算需要不同的明文空间——数空间 Z_{p^r} 与数字空间 Z_p；据此设计 reduction 步骤（从数空间转换到数字空间）执行比较，再用 modulus raising 步骤把结果映射回数空间，从而在同一方案内连续求值算术与比较。

在数据库工作负载上，该方法比方案切换快至 17 倍、比直接比较快至 15 倍，展示了对真实隐私数据库查询（比较与算术混合）的实用性；代码与工件已开源（Universal-BGV）。

我的思考：在单一方案内消除切换开销与近似误差，思路简洁且针对混合负载痛点直接；数据库场景的 17×/15× 加速具有工程说服力。局限：明文空间转换会限制可表示范围（Z_{p^r}→Z_p），性能依赖具体参数选取；与 CKKS/TFHE 混合编译路线的综合优劣尚需更系统的比较。该工作为 FHE 编译与算子设计提供了新的优化维度。

- **arXiv PDF**: [Efficient Arithmetic-and-Comparison Homomorphic Encryption with Space Switching.](https://arxiv.org/pdf/2604.19890)

#### Code-Based Scalable Collaborative SNARKs.
- **作者**: Christodoulos Pappas, Dimitrios Papadopoulos, Charalampos Papamanthou
- **方向**: 密码学与协议
- **解读**: 可扩展协作式 SNARK 面向"多方各持一部分计算或数据、共同生成一个证明"的场景，是区块链与去中心化计算中验证聚合、跨机构审计的关键需求；但现有协作证明方案要么对参与方数目敏感、要么需要较强信任假设，规模化能力受限。（无摘要，据标题推断）

据标题推断，本文以线性码（linear code）为底层工具构造协作式 SNARK——延续基于纠错码的证明系统路线，利用码的代数结构把"多证明者协作生成证明"组织为可扩展、可并行且抗合谋的流程，从而在证明者数量增长时保持通信与计算开销可控。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出新的协作证明协议及其在参与方规模、证明大小、验证开销上的实证对比。（无摘要，据标题推断）

我的思考：协作证明的价值在于把"证明生成"从单点算力依赖中解放出来，契合去中心化验证的叙事；基于码的路线通常以更大的证明为代价换取无可信设置与结构简单性，与基于折叠/多项式承诺的方案形成互补。局限：无摘要，安全性模型（合谋容忍度、公共参考串假设）与常数因子需正文确认。（无摘要，据标题推断）


#### Optimistic Asynchronous Dynamic-Committee Proactive Secret Sharing.
- **作者**: Bin Hu, Jianwei Liu, Zhenliang Lu, Qiang Tang, Zhuolun Xiang, Zongyang Zhang
- **方向**: 密码学与协议
- **解读**: 主动秘密共享（PSS）通过周期性刷新份额抵御长期妥协，是门限签名、密钥托管与区块链验证者安全的基石；而动态委员会（成员随时间变化）与异步网络（消息任意延迟）是真实分布式系统的常态，二者叠加使份额刷新与轮换的正确性和活性都变得困难。（无摘要，据标题推断）

据标题推断，本文提出异步动态委员会下的乐观式 PSS 协议："乐观"意味着在正常情形下走快速路径（高效完成刷新与移交），仅在异常时才回退到更保守的通用流程；技术细节可能涉及异步可验证秘密共享、委员会重配置与份额一致性验证的组合，在活性与效率间取得平衡。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出协议在轮数、通信复杂度与容错参数上的理论分析及可能的实现评估。（无摘要，据标题推断）

我的思考：动态委员会的 PSS 直接服务 PoS 区块链（验证者集不断轮换）的密钥管理，选题贴近系统现实；乐观式设计是典型的工程智慧——把昂贵路径留给罕见异常。作者阵容含 Lu、Xiang 等分布式密码学活跃学者，理论严谨性可期；局限是无摘要，容错阈值与异步假设细节需正文确认。（无摘要，据标题推断）


#### Nebula: Proving Machine Executions via Folding Schemes.
- **作者**: Arasu Arun, Srinath T. V. Setty
- **方向**: 密码学与协议
- **解读**: 可验证计算（zkVM）让一方证明"某段程序确实如此执行"，是区块链扩容、可信审计与 AI 推理验证的基础设施；但通用机器执行（任意指令、任意状态）的证明生成通常串行、开销大，难以同时做到高效、可并行与模块化。（无摘要，据标题推断）

据标题推断，Nebula 基于折叠方案（folding schemes）构造 zkVM：把程序执行切分为可折叠的增量证明步骤，使证明者能以近线性时间生成证明并利用并行性加速；"模块化保持"意味着证明结构可复用、便于组合，降低工程复杂度。作者 Arun 与 Setty 是折叠/SNARK 方向的核心贡献者，方案应为该路线的系统化落地。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告相对既有 zkVM 在证明生成时间、内存占用与可扩展性上的对比数据。（无摘要，据标题推断）

我的思考：折叠路线（如 Nova 系）近年成为 zkVM 主流，Nebula 的价值在于把折叠推广到完整机器语义并保持并行与模块化，理论扎实，工程成色取决于实现；局限：无摘要，具体指令集、折叠方案选择与基准数字需以正文为准，其与既有 RISC-V zkVM 工作的差异也值得对照细读。（无摘要，据标题推断）


#### Breaking the Barrier for Asynchronous MPC with a Friend.
- **作者**: Banashri Karmakar, Aniket Kate, Shravani Patil, Arpita Patra, Sikhar Patranabis, Protik Paul, Divya Ravi
- **方向**: 密码学与协议
- **解读**: 异步安全多方计算（MPC）在消息任意延迟的网络上运行，经典结果显示其最多容忍 t<n/3 的恶意腐败，这一容错"天花板"长期限制了异步协议的部署价值；能否借助额外假设突破该界限，是分布式密码学的开放问题。（无摘要，据标题推断）

据标题推断，本文引入"一个朋友"（a friend）——一个始终诚实或半诚实的辅助参与方——以突破 t<n/3 的经典屏障：朋友承担少量关键计算或验证职责，使协议在更高腐败阈值（如 t<n/2）下仍保活性与安全；作者团队在异步与轮次优化 MPC 上积累深厚，方案应是异步轮次协议与辅助方假设的系统化结合。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出新容错阈值下的协议构造、复杂度分析与可能的实现基准。（无摘要，据标题推断）

我的思考："以额外假设换取容错上限"是密码学的常见杠杆，关键是朋友的假设强度是否可接受——若朋友可被现实化为可信硬件或诚实多数子网，方案落地空间可观；局限：无摘要，朋友的确切角色、最终性与活性保证及与同步-异步混合协议的关系需正文厘清。（无摘要，据标题推断）


#### Decor: Delegated Computation on Randomness for Secure Evaluation of Nonlinear Functions.
- **作者**: Haris Smajlovic, Kyle Sheng, Timos Antonopoulos, Ruzica Piskac, Hyunghoon Cho
- **方向**: 密码学与协议
- **解读**: 安全多方计算/两方计算中，非线性函数（如 sigmoid、softmax、指数、对数）的密文求值远比线性运算昂贵，成为隐私机器学习落地的主要开销来源；如何让非线性求值"变便宜"是隐私计算领域的焦点问题。（无摘要，据标题推断）

据标题推断，Decor 采用"随机性上的委托计算"（delegated computation on randomness）：把与具体输入无关的随机相关值（如预计算元组、掩码随机性）委托给更高效的第三方或服务器提前生成，在线阶段借助这些预计算以低轮数、低通信完成非线性函数求值，从而实现安全性与效率的分离。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出非线性函数求值的端到端延迟/吞吐改进及其与既有预计算方案的对比。（无摘要，据标题推断）

我的思考：把"输入无关的随机工作"外包是预计算模型的自然延伸，若安全模型允许半可信委托方，收益可观；该团队兼具密码学与系统实现背景，方案可能包含可运行原型。局限：无摘要，委托方的信任假设与泄露边界需正文界定，非线性近似的精度与安全权衡也值得细究。（无摘要，据标题推断）


#### Parasol Compiler: Pushing the Boundaries of FHE Program Efficiency.
- **作者**: Rick Weber, Ryan Orendorff, Ghada Almashaqbeh, Ravital Solomon
- **方向**: 密码学与协议
- **解读**: 全同态加密（FHE）允许在密文上直接计算，但程序被编译成同态电路后开销巨大，编译器优化水平直接决定实用性能；现有 FHE 编译器在并行化、密钥切换与重线性化调度上仍有大量浪费，程序效率远未达到理论可能。（无摘要，据标题推断）

据标题推断，Parasol 是面向 FHE 程序的编译器，以"推动效率边界"为目标：可能在程序级挖掘并行性、智能调度引导自举与重线性化、或对计算图做同态友好的变换，把高层程序高效映射为密文运算序列，兼顾安全参数与噪声预算。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告编译后程序相对既有编译器（如 EVA、Cingulata、HEIR）在运行时间上的加速比。（无摘要，据标题推断）

我的思考：FHE 的"最后一公里"是软件栈——编译器优化对实际部署的杠杆巨大；把并行化纳入编译决策是正确方向，作者含 Almashaqbeh 等密码系统研究者，方案应有实现支撑。局限：无摘要，支持的语言子集、安全模型假设与基准程序集需正文确认；优化收益是否随程序规模保持也待验证。（无摘要，据标题推断）


#### Prrr: Personal Random Rewards for Blockchain Reporting.

- **作者**: Hongyin Chen, Yubin Ke, Xiaotie Deng, Ittay Eyal

- **方向**: 密码学与协议

- **解读**: 区块链智能合约常依赖链上报告（发布者被激励上报数据），现有方案在"少数可信发布者（中心化风险）"与"开放发布（报告泛滥）"之间存在安全-性能权衡，二者难以兼得。

作者指出权衡根源在于对称奖励设计——对所有发布者一视同仁，并证明不存在能克服该权衡的对称奖励机制；进而提出 Prrr（Personal Random Rewards），为不同发布者分配随机异质奖励，提出机制设计新概念"事前合成不对称"（Ex-Ante Synthetic Asymmetry），并采用二价（second-price）式结算分配奖励。

文中证明该协议构成子博弈完美纳什均衡（SPNE），对共谋与 Sybil 攻击具有鲁棒性，同时实现激励相容并兼顾安全与效率；适用于众多依赖报告的智能合约场景。

我的思考：以不可行性证明指出"不对称性是打破权衡的必要条件"，再以随机异质奖励 + 二价结算落地，理论贡献扎实、思想简洁；局限在于随机奖励的公平感知、链上随机性的安全生成，以及多轮动态博弈下的长期均衡分析。


#### Leafblower: a Leakage Attack Against Tee-Based Encrypted Databases.

- **作者**: Zachary Espiritu, Seny Kamara, Tarik Moataz, Valentin Ogier

- **方向**: 密码学与协议

- **解读**: TEE 支撑的加密数据库（如基于 SGX 的方案）承诺在硬件可信区内解密处理查询，但查询执行过程仍可能泄露信息（访问模式、数据分布等）；其实际隐私保证需要攻击视角的检验。

据标题推断，Leafblower 针对基于 TEE 的加密数据库提出泄露攻击：利用可观察的查询副作用（页面访问、时序、内存访问模式等）重建敏感数据或恢复查询内容，绕过加密层的语义保护。

无摘要，无法引用具体量化结果；核心是证明 TEE 加密数据库仍存在可利用的泄露通道。

我的思考：加密数据库的安全论证常聚焦加密方案本身，而 TEE 引入的执行侧泄露（缺乏 oblivious 机制）是"实现层漏洞"；该工作与 access-pattern 攻击一脉相承，提醒工程系统须把端到端隐私建模纳入威胁模型；局限在于攻击效率与真实工作负载下的信噪比。（无摘要，据标题推断）


### 隐私保护（21 篇）

#### The Person Behind the Sound: Demystifying Audio Private Attribute Profiling Via Multimodal Large Language Models.
- **作者**: Lixu Wang, Kaixiang Yao, Xinfeng Li, Dong Yang, Haoyao Li, Xiaofeng Wang, Wei Dong
- **方向**: 隐私保护
- **解读**: 语音中蕴含年龄、性别、健康状态、情绪、身份等大量隐私属性；多模态大语言模型（MLLM）听觉理解能力大幅跃升后，可能被用于对陌生人的音频自动进行隐私画像，构成新型隐私泄露风险。论文旨在"去神秘化"地研究基于 MLLM 的音频隐私属性画像的原理与威胁面。

据标题推断，作者利用多模态大模型对音频（人声）进行零样本或少样本的隐私属性推断，系统评估模型能从声音中推断出哪些个人信息（如年龄、性别、地区口音、健康状态），并考察提示工程等手段对画像能力的放大作用。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重心在于量化 MLLM 音频隐私画像的能力边界与触发条件，而非提出攻击工具本身。

该工作把生成式模型的隐私威胁从文本与图像延伸到语音模态，呼应"声音即生物特征"的隐私共识；若 MLLM 的推断精度足够高，将冲击匿名语音数据分享（客服录音、语音社交）的既有安全假设，也为“脱敏后语音仍可被画像”提供实证。局限：真实音频数据集的规模与代表性、跨库泛化能力需实证；防御侧（音频脱敏、属性不可区分）也应同步跟进。


#### Breaking the Illusion: Automated Reasoning of GDPR Consent Violations.
- **作者**: Ying Li, Wenjun Qiu, Faysal Hossain Shezan, Kunlin Cai, Michelangelo van Dam, Lisa M. Austin, David Lie, Yuan Tian
- **方向**: 隐私保护
- **解读**: 

**问题**：GDPR等法规要求用户同意必须知情、自由、特定且明确，但大量网站不合规（截至2025年1月累计罚款约58.8亿欧元，30%来自同意违规）。现有研究聚焦cookie横幅，而功能网页中嵌入的同意表单——结构多样、法律依据各异、难以定位——是尚未被系统审计的合规盲区。

**方法**：论文提出Cosmic自动化框架，含三大创新：(1) 基于LLM与RAG从隐私政策提取需同意的处理目的，并用多模态web agent动态定位相关表单；(2) 设计领域特定语言（DSL）统一描述异构表单结构；(3) 与隐私法律专家协作，将GDPR自然人语言要求翻译为机器可解释的Datalog规则，用Soufflé引擎进行形式化推理验证，生成定位到具体元素的违规报告。

**结果**：在5,823个网站、3,598个表单上检测出3,384项违规，94.1%的同意表单存在违规，覆盖自由同意、目的披露、撤回选项等原则；违规检测TPR 98.6%、TNR 99.1%（以150个表单的人工标注为基准，Cohen's Kappa 0.861）；缺失撤回信息是最普遍违规（89.6%），成人网站违规率最高（36.1%），55.3%的表单同时存在多项违规。

**我的思考**：该工作把"法律文本→形式逻辑→自动验证"的链条完整落地，DSL+Datalog使推理阶段完全确定（错误全部来自LLM文本处理环节），这一分工很聪明，也为其他合规场景提供可复用范式。局限是LLM环节仍会引入误报漏报（如对newsletter多重描述过度解析、未识别CSS类控制的预选状态）；约15天分析5,823个网站的规模化成本高；仅覆盖英语网站且按英国GDPR语境评估，跨辖区扩展仍需工作。

- **arXiv PDF**: [Breaking the Illusion: Automated Reasoning of GDPR Consent Violations.](https://arxiv.org/pdf/2512.22789)

#### Understanding and Analyzing Privacy Risks in Mobile Consent-Management Platforms.

- **作者**: Jingzhou Ye, Fares Alharbi, Luyi Xing, Xueqiang Wang

- **方向**: 隐私保护

- **解读**: 移动应用通过同意管理平台（CMP）统一向多个第三方 SDK 征求用户同意，但 CMP 的存在不等于有效、可问责的同意：GUI 披露不全、撤回困难、强制同意等问题可能使 GDPR 等合规要求形同虚设，移动端 CMP 的隐私风险缺乏大规模实证。

作者提出 DIULENS 框架：用 LLM 辅助的 GUI 分析定位 CMP 界面，并将 CMP 的"设计-实现-使用"（DIU）与真实 SDK 使用行为比对，检测三类违规；在 9,332 个 Android 与 3,496 个 iOS 应用上大规模测试，并比较 Android/iOS 平台差异（如 Apple ATT 框架的影响）。

发现 CMP GUI 常未清晰披露全部第三方 SDK 或用户操作的准确同意后果，用户难以撤回同意、可能遭遇强制同意；至少一个 DIU 违规出现在 397 个应用中，占集成 CMP 应用的 60.5%（其中 Android 371 个、iOS 26 个）；DIU 风险同时源于开发者配置/实现与 CMP 自身实现。

我的思考：本文把网页端 CMP 审计方法推进到移动生态，DIU 分类学 + LLM GUI 分析 + 大规模测量的方法论组合完整，60.5% 的违规比例凸显移动 CMP 的治理缺口。局限在于 LLM GUI 分析依赖界面可达性，动态/原生渲染的 CMP 可能漏检；DIU 判定基于语义比对，误报率需人工核验；iOS 样本较小，平台差异结论的稳健性有限。对 CMP 厂商与监管机构均有直接参考价值。


#### Generate-then-Verify: Reconstructing Data from Limited Published Statistics.
- **作者**: Terrance Liu, Eileen Xiao, Adam D. Smith, Pratiksha Thaker, Zhiwei Steven Wu
- **方向**: 隐私保护
- **解读**: 
政府与机构常发布聚合统计（人口普查、经济指标等）供公众使用，但聚合数据并非天然安全——攻击者可从有限统计中重建个体记录，威胁数据主体隐私；差分隐私等机制可降低风险，但"统计发布到底泄露多少"仍缺乏系统理解，重建攻击的研究对隐私政策制定至关重要。

据标题推断，论文提出"生成—验证"（Generate-then-Verify）框架研究从有限发布统计中重建数据：先利用先验知识生成候选数据集，再以发布统计为约束进行验证与筛选，迭代逼近真实数据；框架应结合优化与统计推断方法，并评估不同统计量与发布机制下的重建成功率与精度。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告在不同统计量数量、类型与隐私机制下的重建精度对比，具体数据以全文为准。

该团队（Adam Smith为差分隐私奠基人之一，Zhiwei Steven Wu为隐私与机器学习安全权威）从理论视角系统化重建攻击，对差分隐私的隐私损失度量提出实证拷问——有限统计的发布也可能被逆向利用，为隐私参数选择提供依据。局限在于重建成功率依赖先验与统计量的信息量，极端情况下可能高估威胁；该工作对隐私发布实践与监管具有重要参考价值。


#### Setting the Course, but Forgetting to Steer: Analyzing Compliance with GDPR's Right of Access to Data by Instagram, TikTok, and Youtube.
- **作者**: Sai Keerthana Karnam, Abhisek Dash, Antariksh Das, Sepehr Mousavi, Stefan Bechtold, Krishna P. Gummadi, Animesh Mukherjee, Ingmar Weber, Savvas Zannettou
- **方向**: 隐私保护
- **解读**: 
GDPR赋予用户"数据访问权"（Right of Access），要求平台应请求提供用户数据副本；该权利是用户理解与控制自身数据的基石。但大型社交平台的合规实践是否真正满足GDPR要求（如提供完整、可理解、可机读的数据）缺乏系统评估，合规流于形式将架空用户的数据权利。

据标题推断，论文对Instagram、TikTok与YouTube三大平台的数据访问请求流程进行合规性实证分析：向平台发起真实的访问请求，系统比较返回数据的完整性、格式、时间范围与可操作性，评估其与GDPR要求的差距，并从用户视角讨论实际体验与改进空间。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告三大平台在响应时间、数据完整性与格式等方面的合规差距对比，具体数据以全文为准。

该团队跨领域（Gummadi为社交网络隐私研究权威，Zannettou为平台审计专家）把GDPR合规从法律条文拉到实证检验，方法可复制、结论可问责，对监管机构与平台都具有参考价值。"设定了方向却忘记掌舵"的标题暗示平台在形式上响应、实质上缺位，这一批评具有公共政策意义。局限在于单次请求的样本与平台更新可能导致结论时效性受限；该工作为"平台合规审计"提供了方法论范本。

#### Consumer Beware! Exploring Data Brokers' CCPA Compliance.

- **作者**: Elina van Kempen, Isita Bagayatkar, Pavel Frolikov, Chloe Georgiou, Gene Tsudik
- **方向**: 隐私保护
- **解读**:
数据经纪人（data brokers）在消费者不知情或未同意的情况下，收集并出售数百万人的个人信息。加州消费者隐私法案（CCPA）赋予消费者请求访问、删除自身数据的合法权利，并设有官方注册机制，但各实体实际合规程度并不清楚。

论文开展了首次大规模系统性研究：向全部 543 家官方注册数据经纪人逐一人工提交数据请求，随后深入分析各家的响应（或无响应），从合规执行层面检验 CCPA 落地情况。

超过 40% 的经纪人完全没有响应，明显违反 CCPA；部分响应的经纪人在身份验证环节要求用户提供其先前并未收集的细节信息，形成悖论——行使隐私权反而引入新的风险。研究发现不合规与流程不标准化现象普遍，凸显更强执法、更清晰指引与标准化定期核查的紧迫性。

该研究以“合规实测”填补了法规文本与现实执行之间的空白，543 家全量覆盖使结论颇具说服力；“验证即泄露”的悖论发现尤有价值。局限在于单一时点的快照式测量，难以刻画合规随时间的变化；若能与消费者投诉数据、司法执法记录交叉印证，结论将更完整。


#### Understanding Data Collection, Brokerage, and Spam in the Lead Marketing Ecosystem.

- **作者**: Yash Vekaria, Nurullah Demir, Konrad Kollnig, Zubair Shafiq

- **方向**: 隐私保护

- **解读**: 线索营销（lead marketing）生态通过网页表单收集个人数据，再出售给商家以促成保险等高价值垂直领域的报价，市场规模预计从 2021 年的 30 亿美元增至 2028 年近 100 亿美元。尽管数据高度敏感、规模巨大，该生态此前几乎没有被研究界系统考察，其隐私与垃圾营销风险处于黑箱状态。

作者构建了端到端测量框架，追踪从数据收集到消费者被联系的完整数据流：对 100 多个健康类线索生成网站插桩，监控 200 个受控电话号码与邮箱；从三家主要线索平台购买自身及他人产生的线索，研究下游经纪行为；并分析 7,432 条 BBB 投诉与评价作为消费者侧佐证。

研究发现敏感健康信息被分享给 70 多个独立第三方；经纪环节存在欺骗性做法——数据被出售给未审核买家，且常被增补甚至编造健康状况、体重等属性。作者共收到 8,000 余通电话营销、600 条短信与 200 封邮件，表单提交后数秒内即来电；大量活动使用 VoIP 邻居号码欺骗与高频拨号，有时使手机无法使用。电话退订相对最有效，但所有退订都无法完全停止营销联系。

我的思考：这是首个系统性刻画线索营销隐私与垃圾信息风险的实证工作，数据流追踪方法可复用于其他垂直领域，对监管与合规有直接参考价值。局限：聚焦健康类网站与三家平台，样本代表性有限；测量中的退订实验涉及真实营销者，伦理边界需谨慎界定。整体揭示了一个高度互联且不合规的生态在激进变现敏感数据，值得政策层面跟进。

- **arXiv PDF**: [Understanding Data Collection, Brokerage, and Spam in the Lead Marketing Ecosystem.](https://arxiv.org/pdf/2604.06759)

#### CBUE: Conclusion Based Utility Evaluation for Differentially Private Categorical Data.

- **作者**: Furkan Sarikaya, Shaohua Lu, Johes Bater, Mark Hempstead
- **方向**: 隐私保护
- **解读**:
差分隐私（DP）机制在类别型（categorical）数据上的实用性评估，长期依赖单一效用指标，难以反映机制对下游“结论”的影响；机制选型不当可能让统计结论失真，决策者缺乏面向结论的评估工具。

据标题推断，CBUE 提出基于结论（conclusion-based）的效用评估框架：以最终分析结论（如假设检验结果、推断正确性）为度量标准，评估不同 DP 机制在类别型数据上的表现，从而把效用评估从“误差大小”转向“结论有效性”。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于为 DP 机制选型提供以任务结论为导向的判据。

该工作延续“以任务为中心的 DP 效用评估”趋势，直击传统误差度量与真实分析目标脱节的问题，对从业者选型有直接参考价值；与下游统计任务绑定也是其局限——结论度量依赖具体分析任务，通用化与跨机制比较仍需更多实证支撑，未来可与结论有效性校准（conclusion-calibration）类方法结合，形成更系统的机制评估流程。


#### Hadal: Centralized Label DP without a Trusted Party.

- **作者**: James Choncholas, Stanislav Peceny, Amit Agarwal, Mariana Raykova, Baiyu Li, Karn Seth
- **方向**: 隐私保护
- **解读**:
集中式标签差分隐私（label DP）为机器学习训练提供集中式隐私保证，但通常依赖可信第三方完成加噪或聚合；现实部署中缺乏这样的可信方，成为该范式落地的关键障碍。

据标题推断，Hadal 在无可信第三方的前提下实现集中式标签 DP：借助多方安全计算等密码学手段，让参与方在本地数据上协同完成标签聚合与加噪，使任何单一实体都无法看到原始标签，同时保持集中式 DP 的隐私预算语义。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；核心主张是把信任假设从“可信聚合者”转移到密码学协议，换取集中式 DP 的效用优势。

该工作直接回应标签 DP 落地中的信任痛点，与 secure aggregation 思路一脉相承，作者团队兼具工业界与学术界背景，实用取向明显。局限在于 MPC 的通信与计算开销对大规模训练的制约，以及与本地 DP 方案在效用-成本曲线上需要严谨的实证比较。，并给出端到端训练的可行性与成本分析。


#### Privacy-Conscious Algorithm Design Via PAC Privacy.

- **作者**: Mayuri Sridhar, Xiaochen Zhu, Srinivas Devadas
- **方向**: 隐私保护
- **解读**:
差分隐私是主流隐私定义，但其严格的最坏情况保证使许多算法难以直接满足；设计者在隐私与效用之间缺少更灵活的“设计期”隐私语言，往往只能事后检验而非事前指导算法设计。

据标题推断，论文以 PAC 隐私（基于 PAC 学习理论的隐私概念）作为算法设计工具：在设计阶段用 PAC 隐私刻画算法的隐私-效用权衡，使算法在保持可证明隐私界的同时获得更宽松、更实用的保证，并给出从“隐私意识设计”到具体算法实例的转化路径。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重心是建立一套以 PAC 隐私为语言的自顶向下算法设计框架。

PAC 隐私把概率近似正确视角引入隐私分析，与纯差分隐私的最坏情况视角形成互补，理论上可显著改善效用；但其“多数情形”语义能否被安全社区接受是推广的关键难点。与 PAC-Bayes、Bayes 隐私等相邻概念的关系需进一步厘清，实证效用增益也待展示。


#### Decomposition-Based Optimal Bounds for Privacy Amplification via Shuffling.

- **作者**: Pengcheng Su, Haibo Cheng, Ping Wang
- **方向**: 隐私保护
- **解读**:
洗牌（shuffling）已被证明能放大差分隐私保证，带来更优的隐私-效用权衡。刻画该放大的两个基本分析框架是 Balle 等人（CRYPTO 2019）的 privacy blanket 与 Feldman 的 clone（FOCS 2021、SODA 2023，含标准与更强变体），二者共享同一基础：把局部随机化器分解为结构化组件。

论文提出统一框架——general clone 范式，包含所有可能的分解方式，blanket 分解只是其特例；在此框架内识别出最优分解，恰是 blanket 所用的那一种；并给出基于快速傅里叶变换（FFT）的简单高效算法来计算放大界。

实验表明，计算出的上界几乎与下界吻合，验证了方法的紧致性；基于该方法还推导出洗牌模型下 LDP 机制的联合（joint）与并行（parallel）组合界。

把两个看似不同的经典框架统一进一个范式并证明其最优性，属于把已知结果打磨成一般理论的扎实理论工作；FFT 算法让理论界真正可计算。局限是纯理论性质，对具体洗牌协议（如安全聚合、洋葱路由）的落地指导尚需进一步转化。，其紧致性结论也为此前分散的放大结果提供了统一参照。


#### Making Privacy Public: Toward a Differential Privacy Deployment Registry.

- **作者**: Priyanka Nanayakkara, Elena Ghazi, Salil P. Vadhan
- **方向**: 隐私保护
- **解读**:
差分隐私的承诺与实际部署之间存在信息鸿沟：公众无法系统了解哪些机构、在哪些系统、以何种参数真正部署了 DP，难以独立核验隐私声明，学术界的实证研究也缺乏统一的部署数据来源。

据标题推断，论文倡导并构建一个差分隐私部署注册表（deployment registry）：系统化登记全球真实 DP 部署的机制、参数、数据范围与透明度信息，把“我们用了 DP”的模糊声明转化为可查证、可比较的公共记录，并讨论注册表的设计、数据模型与治理机制。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；工作重心在注册表的框架设计与社区可行性论证。

该工作把“透明性”本身当作隐私工程的研究对象，呼应 DP 审计与“隐私声明可验证”的呼声，注册表可为实证研究与公众监督提供基础设施；但自愿登记会导致覆盖偏差，公开参数也可能引发放大攻击面等顾虑，与独立审计机制结合才更有说服力。


#### Auditing Apple's DifferentialPrivacy.framework: Implementation Bugs, Misconfigurations, and Practical Risks.

- **作者**: Rishav Chourasia, Ergute Bao, Uzair Javaid, Xiaokui Xiao
- **方向**: 隐私保护
- **解读**:
自 2016 年起，Apple 声称用于改进用户体验的设备分析受差分隐私（DP）保护，其框架部署于各操作系统，处理 Safari 域名、键盘事件、照片属性、健康报告等敏感信号；由于私有化算法未开源，这些声明难以被独立核验。

论文对 macOS Sonoma 14.2 与 Sequoia 15.6 上的 Apple DP 开展客户端审计：逆向工程随附二进制、恢复 Objective-C 接口、构建运行时测试台执行各机制，检验输出是否符合宣称的保证；覆盖几乎所有活跃机制，含 Count Median Sketch、Hadamard-CMS、随机响应与 Prio 风格安全聚合。

发现多个实现缺陷与错误配置：每个被审计机制都依赖浮点噪声，因采样器存在已知漏洞，无法满足其宣称的（零知识证明级）保证；安全聚合配置被本地禁用，使聚合前记录暴露给任何能访问日志的一方。共 9 个机制中 5 个违规，影响 87% 的数据收集（Sequoia 上 68%）；公开泄露的 iPhone 日志可解码出域名、emoji 等隐私信息。

这是对闭源商用 DP 系统罕见的客户端侧实证审计，把“声称的隐私”与“可观测的实现”的差距量化呈现，方法论可复用于其他供应商；5/9 机制违规、87% 受影响等数字极具冲击力。局限在于依赖逆向工程与特定系统版本，结论随版本演进可能变化；也印证开源与第三方审计应是隐私声明的必备环节。


#### Sparse Estimation Under Local Differential Privacy at All Privacy Levels.

- **作者**: Puning Zhao, Qingqing Ye, Shaowei Wang, Jun Feng, Sheng Yue, Zhen Chen, Xiaochun Cao
- **方向**: 隐私保护
- **解读**:
局部差分隐私（LDP）下的稀疏估计（稀疏分布、稀疏协方差等）是隐私统计的核心问题；此前方法往往只在特定隐私预算区间内最优，无法在所有隐私水平（ε）上统一达到最优，限制了实际部署中预算灵活设定的需求。

据标题推断，论文研究在全部隐私水平下 LDP 的稀疏估计：通过自适应机制选择与平滑的估计策略，使估计量在任意 ε 下都达到（近乎）最优的统计效率，而非只在一个区间成立，从而统一不同隐私水平下的最优性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；理论贡献在于填补不同隐私水平之间方法论空白，给出全域最优估计策略。

把“分区间最优”整合为“全水平最优”是隐私统计的重要推进，对预算需随数据敏感度动态调整的场景尤有意义；局限在于稀疏假设的适用面、高维下的计算成本，以及与压缩感知、minimax 最优估计文献的联系有待作者在全文中的系统阐述。


#### EyeSpy: Inferring Eye Gaze via Side-Channel Attacks Against Foveated Rendering.
- **作者**: Paul Maynard, Harris Amjad, Camila Molinares, Bo Ji, Brendan David-John
- **方向**: 隐私保护
- **解读**: 眼动追踪为 VR 带来注视交互与动态注视点渲染（DFR），但眼动数据可泄露敏感用户信息。现有保护（权限提示、数据门禁）在 DFR 系统上失效，因为眼动数据在内部驱动渲染，无法简单封禁；DFR 下中央凹（fovea）内的物体比周边承担更高 GPU 负载，形成注视相关的侧信道。

攻击设计：在用户视野中扫过不可感知的高成本物体（HCOs），记录游戏引擎通常暴露的性能指标（帧率、帧时间），利用 HCO 与中央凹重叠引起的性能波动与已知 HCO 位置的对应关系，直接推断注视坐标，全程无需调用眼动 API。

实验显示在 Meta Quest Pro、Varjo XR-4 与桌面平台上，注视预测平均误差仅 1.1–4.4 度，与典型眼动追踪器精度相当，且能泛化到多种硬件平台、引擎与渲染管线；作者还提出基于监督/无监督检测器的防御，可在短时间窗口内可靠标记攻击（F1 0.99）。

我的思考：这是"渲染优化反而成为隐私通道"的典型案例——DFR 本为节省算力，其负载差异却被用作注视推断信号，且攻击面是引擎天然暴露的性能日志，几乎无法在不破坏功能的前提下关闭。1.1–4.4 度的误差对推断"用户在注视什么"已足够危险。局限在于攻击需要控制场景内的物体放置（游戏内可行，一般应用受限）；防御 F1 0.99，但其成本与对合法用户体验的影响需进一步评估。


#### Consistent Estimation of Numerical Distributions Under Local Differential Privacy by Wavelet Expansion.

- **作者**: Puning Zhao, Zhikun Zhang, Bo Sun, Li Shen, Liang Zhang, Shaowei Wang, Zhe Liu

- **方向**: 隐私保护

- **解读**: 本地差分隐私（LDP）下分布估计是基础任务，被 Apple、Google、Microsoft 等用于真实产品；分类数据上已有大量进展，但数值数据的评估指标不同——Wasserstein 与 KS 距离要求"概率质量不能放错位置"，而分类方法（如 ℓ1/KL 指标下的方法）会把质量挪到远离真值的地方，导致范围查询等下游任务精度受损。

论文提出用小波展开表示样本分布：各小波系数在 LDP 下估计，并优先估计低阶系数以保证宏观层面的准确性，从而防止概率质量大幅偏移；作者给出估计误差的理论保证，并推导出最优展开阶数 J 的选择规则（基于理论界）。

在 Beta(5,2)（10 万样本）、Retirement（682,410 样本）、Income、纽约出租车（8,760,687 样本）四个数据集上，方法在 Wasserstein 与 KS 距离上一致优于分箱 OUE/kRR、层级直方图（HH）、卷积类（DW/IIW）与 SW+EM 等基线，对含尖峰的非平滑分布（Retirement、Income）优势尤其明显——小波能保留尖峰而不依赖分布平滑性；经验最优 J 与理论预测一致。

我的思考：首次系统地把小波展开用于 LDP 数值分布估计，理论分析（含最优参数规则）与实验闭环完整，"不依赖平滑性假设"是相对既有方法的关键优势。局限：实验集中在 1 维分布，高维数值数据的扩展尚待验证；与小 ε、小样本场景下的基线差距也需更多数据支撑。该工作为数值型 LDP 分布估计提供了新的理论框架与实用工具。
- **arXiv PDF**: [Consistent Estimation of Numerical Distributions Under Local Differential Privacy by Wavelet Expansion.](https://arxiv.org/pdf/2509.19661)

#### Hidden Secrets in the arXiv: Discovering, Analyzing, and Preventing Unintentional Information Disclosure in Source Files of Scientific Preprints.

- **作者**: Jan Pennekamp, Johannes Lohmöller, David Schütte, Joscha Loos, Martin Henze

- **方向**: 隐私保护

- **解读**: 科研预印本（arXiv）的源文件常包含 LaTeX 源码、图片等，可能无意中泄露敏感信息（未公开数据、个人隐私、审稿痕迹等）；大规模发现、分析并防止这类泄露尚缺乏系统研究。

据标题推断，作者对 arXiv 预印本源文件进行大规模扫描：自动检测其中隐藏的敏感信息（本地路径、环境变量、个人数据等），分析泄露的类别与规模，并提出预防措施（发布前检查工具与平台侧过滤）。

无摘要，无法引用具体量化结果；核心是刻画 arXiv 源文件信息泄露的现状并提出缓解方案。

我的思考：预印本已成为学术交流主流，源文件泄露是真实且被忽视的风险，对匿名评审（double-blind）尤其致命——元数据可能直接暴露作者身份；局限在于检测能力依赖敏感信息规则库，误报/漏报平衡与平台侧落地可行性值得关注。（无摘要，据标题推断）


#### Revelio: Blurred Images Can Still Disclose Your Identity.

- **作者**: Haoyu Zhai, Shuo Wang, Pirouz Naghavi, Qingying Hao, Gang Wang

- **方向**: 隐私保护

- **解读**: 社交平台与新闻媒体常用模糊（打码）处理人脸以保护隐私，但研究表明模糊图像仍可能泄露身份；需要系统评估模糊的隐私保护效果及其失效条件，回应"模糊即匿名"的普遍假设。

据标题推断，Revelio 利用深度学习模型在模糊人脸图像上识别身份，系统评估不同模糊强度、区域与算法下的身份可识别性，揭示模糊处理的隐私保护边界。

无摘要，无法引用具体量化结果；核心是证明模糊图像仍可披露身份，并刻画影响识别率的因素。

我的思考：与去匿名化、人脸识别对抗研究一脉相承，提醒媒体与平台模糊处理不足以保证匿名，对 GDPR 等法规的"匿名化"标准有直接启示；研究结论对执法与媒体处理人脸素材的规范也有借鉴意义；局限在于依赖训练数据分布，对强模糊与新型匿名化（如扩散模型重绘）的鲁棒性未知。（无摘要，据标题推断）


#### When Designers Meet GenAI: Understanding the Role of Prompt-to-Design Generators in Privacy Dark Patterns.

- **作者**: Jingzhou Ye, Zhaojie Hu, Yao Li, Xueqiang Wang
- **方向**: 隐私保护
- **解读**:
生成式 AI 的"提示到设计"工具（prompt-to-design generators，据文字描述直接生成 UI/网页设计）正被设计师广泛采用，但这些工具的训练数据中天然包含大量既有网站的深色模式（dark patterns）——诱导过度授权、误导性同意、隐藏式选项等隐私侵害设计。若生成工具在输出中复制甚至放大这些模式，AI 将成为隐私深色模式的"工业化生产者"，此前缺乏对这一链条的理解性研究。（无摘要，据标题推断）

据标题推断，论文采用以人为中心的研究方法：让设计师使用 GenAI 设计工具完成真实设计任务，结合生成结果分析、日志与访谈，考察输出中隐私深色模式的出现频率、类型，设计师对其的觉察程度与采纳行为，并可能对比多款主流工具，理解工具特性如何影响深色模式的产生。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文以质性或混合方法给出 GenAI 设计工具助长隐私深色模式的证据、发生机制与设计师视角的洞察。（无摘要，据标题推断）

我的思考：既有深色模式研究聚焦"人设计欺骗"，本文把生成式 AI 纳入设计生产链，指出模型会从训练分布中习得并复制既有欺骗模式，属于 AI 供应链伦理与隐私治理的交汇问题，选题新颖且具前瞻性。局限在于样本规模与工具覆盖的代表性、深色模式判定的主观性，以及缓解建议（提示约束、输出过滤、标注）的有效性验证，均需以全文为准。（无摘要，据标题推断）


#### SoK: After Decades of Web Tracker Detection, What's Next?
- **作者**: Wolf Rieder, Philip Raschke, Thomas Cory, Christian René Sechting, Aditya Kumar, Axel Küpper
- **方向**: 隐私保护
- **解读**: 

**问题**：网络跟踪无处不在，社区过滤列表存在人工维护成本高、易出错等缺陷，研究者转而开发检测器，但三十年来无人对检测器研究做过系统综述与分类——已有综述要么聚焦领域整体、要么聚焦跟踪技术，且存在采样偏倚，缺少对方法、证据质量与可复现性的全景审视。

**方法**：首个元科学系统综述：遵循 PRISMA，从 5 个数码图书馆加顶会检索并辅以引用检索，从 832 篇论文筛出 59 篇主要与 16 篇补充研究（1990–2025）；双人独立筛选加第三人仲裁（Krippendorff α=0.801）；构建覆盖 10 类、29 维、96 特征的分类体系；用 GRADE 评估证据质量，并开展受限可复现性实验。

**结果**：威胁建模 2022 年后才出现且多数研究仍缺失；检测普遍为有监督二分类、以随机森林等传统 ML 为主；图方法（AST 自 2012 年）日益增多但特征复用极少；简单与复杂检测器都稳定达到 80–90 分位性能，"检测本身已不是问题"；但评估停留于静态快照、依赖阈值型指标，缺统一基线与基准；可复现性上 7 个可运行工件全部复现出相近结果，但多数论文未提供完整工件。

**我的思考**：首次以元科学视角为检测器研究建立统一分类学并量化方法论缺陷，"性能已饱和、未来应转向鲁棒性/部署/破坏性评估"的判断为领域指明研究议程（标准化评估框架、对抗测试平台、公开基准）。局限：按设计排除过滤列表与拦截器、存在语言与发表偏倚，可复现样本有限。相比偏重跟踪技术的综述，它把焦点从"检测准不准"转向"证据可不可信、能不能部署"，具有纲领性意义。
- **arXiv PDF**: [SoK: After Decades of Web Tracker Detection, What's Next?](https://arxiv.org/pdf/2605.02982)

#### Your Eyes Won't Lie: Snooping Online Voting Privacy from User Webcam.

- **作者**: Zeyu Deng, Jingwei Zhang, Chen Wang
- **方向**: 隐私保护
- **解读**:
在线投票、问卷与电子政务中，用户的选项选择是高度敏感的隐私数据。现实威胁在于：恶意软件或已被授权但滥用的应用往往拥有摄像头访问权限，而用户在投票界面上的注视轨迹与其选择存在强相关——眼睛"不会说谎"。攻击者仅凭网络摄像头画面即可推断用户投给了谁/选了哪项，使在线投票的隐私保证在侧信道层面失效。（无摘要，据标题推断）

据标题推断，论文研究从用户网络摄像头视频中恢复注视方向/落点的方法，将其与屏幕布局（投票界面）对齐，从而推断用户的选择；可能涵盖视线校准、头部姿态补偿、屏幕内容重建等环节，并在真实的投票/问卷场景与多类界面布局下评估。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出注视推断在投票场景下的选择恢复准确率，以及对不同界面布局、设备与光照条件的敏感性分析。（无摘要，据标题推断）

我的思考：视线追踪是成熟技术，但把它系统性地引入"投票隐私"这一敏感场景并明确威胁模型（摄像头被劫持）是新的攻击面刻画，对电子投票与隐私敏感表单的设计者有直接警示。局限在于攻击前提（摄像头权限、屏幕内容可见）、多显示器与移动端适用性、以及真实投票场景中用户视线行为的多样性，需全文确认；相应的防御（界面防注视设计、摄像头权限最小化、投票时提示风险）也值得讨论。（无摘要，据标题推断）


### 系统与操作系统安全（12 篇）

#### SoK: All You Ever Wanted to Know About Bootloader Security but Were Afraid to Ask.
- **作者**: Connor Glosner, Aravind Machiry
- **方向**: 系统与操作系统安全
- **解读**: 引导加载程序（bootloader）是平台信任链的起点，一旦失陷，Secure Boot、TEE 等上层保障全部失效；但相关研究分散、术语混乱、缺乏系统梳理，从业者难以建立完整威胁图景；同时 bootloader 常与固件供应链、硬件信任根交织，边界不清。该 SoK 论文对 bootloader 安全领域进行系统化整理。

据标题推断，论文对主流 bootloader（UEFI/BIOS 级固件、ARM 可信固件等）做系统调研，建立威胁模型与分类法，归纳攻击面（配置、更新、验证链、硬件接口）与已知漏洞模式，并评估现有缓解措施的覆盖范围与缺口。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；SoK 的价值在于提供全景式的威胁建模、统一分类与开放问题清单。

作者 Machiry 长期从事固件与 bootloader 安全研究（如对 UEFI 攻击面的实证工作），本 SoK 有望为领域提供亟需的共同语言。bootloader 安全常被"买硬件即安全"的叙事掩盖，系统化梳理有助于把防御资源引导到真实缺口。局限：结论依赖覆盖范围，对新兴 RISC-V 平台、后量子验证链与可信启动交互的覆盖程度待查；面向防御实践（测量根、回滚保护、更新通道硬化）的落地建议是核心看点，其完整性能否兑现决定该 SoK 的长期引用价值。


#### GHost in the Shell: A GPU-to-Host Memory Attack and its Mitigation.

- **作者**: Sihyun Roh, Woohyuk Choi, Jaeyoung Chung, Yoochan Lee, Suhwan Song, Byoungyoung Lee

- **方向**: 系统与操作系统安全

- **解读**: 异构内存管理（HMM）统一 CPU 与 GPU 地址空间，使 GPU 可访问主机进程内存，但这一便利也打破了 GPU 与 CPU 之间的内存隔离边界：GPU 侧的内存安全缺陷或恶意内核可借此攻陷主机进程，威胁模型亟待重新审视。

作者提出 GHOST-ATTACK——首个源自 GPU 的主机内存利用技术：利用 GPU 内核中的内存安全缺陷或攻击者提供的恶意内核，滥用 HMM 机制读写主机进程内存，绕过 ASLR 并劫持控制流；同时设计 SHELL（Secure HMM Enforcement with LLVM）防御，静态识别共享内存区域，通过 GPU 驱动的缺页机制在运行时实施细粒度访问控制，恢复隔离。

GHOST-ATTACK 可在常见应用（如 PyTorch、Chrome）中实现主机进程的 ASLR 绕过与控制流劫持；SHELL 通过修改 Clang/LLVM 与开源 NVIDIA 驱动实现，能阻断所有 GHOST-ATTACK 变体且性能影响可忽略，同时保留 HMM 的收益。

我的思考：本文系统揭示 HMM 引入的新型攻击面，是 GPU-主机隔离研究的重要工作：攻击端展示了"GPU 漏洞→主机沦陷"的完整链路，防御端给出可落地的 LLVM+驱动协同方案，攻防闭环完整。局限在于 SHELL 依赖开源驱动的改动与静态分析精度，闭源驱动平台与动态共享场景的覆盖有限；HMM 相关硬件路径的其他隐蔽副作用仍待探索。对 GPU 虚拟化与异构计算的安全设计有直接指导意义。


#### Demystifying and Exploiting ASLR on NVIDIA GPUs.

- **作者**: Ruofan Zhu, Ganhao Chen, Wenbo Shen, Lyuye Zhang, Dakun Shen, Rui Chang, Yanan Guo

- **方向**: 系统与操作系统安全

- **解读**: ASLR 是抵御内存破坏攻击利用的关键缓解机制，但 GPU 侧的 ASLR 实现从未被系统审视。若 GPU 地址空间随机化存在盲区（如未随机化的堆）或与 CPU 的 ASLR 存在关联，将同时削弱 GPU 与 CPU 的内存隔离，威胁 GPU 上运行的敏感负载。

作者首次对 NVIDIA GPU 的 ASLR 进行系统安全分析：逆向 GPU 虚拟地址空间，发展两项新技术——检查 GPU 内存映射（提取页表结构）与缩放随机化地址，据此构建细粒度 GPU 内存图，并以熵度量量化 ASLR 强度；发现与 NVIDIA 共同验证，并提出实用缓解建议。

研究发现存在未随机化的 GPU 堆，且 GPU 与 CPU 的 ASLR 偏移之间存在相关性——可从 GPU 侧推断 CPU 的 ASLR 偏移，从而破坏整体内存隔离；论文提出的缓解措施可增强 GPU ASLR 安全性（本工作获 IEEE S&P 2026 杰出论文奖）。

我的思考：这是首次把"ASLR 有效性"的系统化评估方法论引入 GPU 域，发现"GPU/CPU 随机化相关"这一反直觉且影响深远的结果，对硬件厂商的加固有直接价值。局限在于研究聚焦 NVIDIA 生态，AMD/Intel GPU 的 ASLR 实现未覆盖；熵度量与真实利用成功率之间的映射仍需更多实证；缓解方案的实际部署进度取决于驱动与硬件更新。


#### VMSCAPE: Exposing and Exploiting Incomplete Branch Predictor Isolation in Cloud Environments.
- **作者**: Jean-Claude Graf, Sandro Rüegge, Ali Hajiabadi, Kaveh Razavi
- **方向**: 系统与操作系统安全
- **解读**: 
云环境通过虚拟化隔离不同租户，但微架构资源共享（如分支预测器）可能破坏隔离边界。若分支预测器在虚拟机之间未完全隔离，攻击者可利用其状态残留实施跨租户侧信道或瞬态执行攻击，泄露他人数据；评估"不完整隔离"在实际云处理器上的可利用性是云安全的关键问题。

据标题推断，VMSCAPE系统化暴露并利用云环境中分支预测器隔离的不完备性：通过精细的微架构测量确认跨虚拟机（VM）的分支预测状态泄露路径，进而构建端到端攻击（如恢复密钥或窃取跨租户数据），展示隔离缺陷的现实危害，并讨论可行的缓解措施。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告在真实云处理器上的泄露确认与攻击成功率，具体数据以全文为准。

Razavi团队在微架构安全（Rowhammer、Spectre类）方向成果卓著，VMSCAPE延续其"把硬件隔离缺陷变成现实攻击"的研究路线。分支预测器隔离此前多停留在理论担忧，该工作以实证方式确认并利用，对云厂商的安全声明构成直接挑战。局限在于攻击稳定性依赖具体微架构与工作负载，跨代处理器的泛化性需验证；其价值在于推动硬件供应商落实更彻底的隔离设计。


#### It's a Feature, Not a Bug: Secure and Auditable State Rollback for Confidential Cloud Applications.

- **作者**: Quinn Burke, Anjo Vahldiek-Oberwagner, Michael Swift, Patrick D. McDaniel
- **方向**: 系统与操作系统安全
- **解读**:
重放与回滚攻击威胁云应用完整性：攻击者通过不可信存储接口重新引入“真实但过期”的数据，破坏应用决策。以往安全框架用硬件支撑的“仅前进状态转换”（状态连续性）加以缓解，但把所有回滚一律视为恶意，从而排除了运维恢复（如修复损坏或配置错误）所需的合法回滚。

Rebound 是通用框架：在保持防护的同时，允许策略授权的回滚（二进制、配置、数据）。核心是引用监视器（reference monitor），它中介所有状态转换、执行授权策略、保证更新与回滚的原子性，并发出防篡改日志，为应用与审计者提供透明性。

论文分析了 Rebound 的安全属性，并以 GitLab CI 中的软件部署工作流作为案例：它实现对二进制与原始版本的稳健控制，且端到端开销很低。摘要未给出具体数值。

把“回滚”从单纯的攻击向量重新定义为“受策略约束的能力”，是对状态连续性研究的一次务实修正；引用监视器加防篡改日志的设计经典可靠。局限在于授权策略的表达能力、对硬件信任基（TEE）的依赖，以及审计日志在大规模部署下的可扩展性有待验证。


#### QuickSafe: Targeted Hardening Against Memory Corruption.
- **作者**: Johannes Blaser, Floris Gorter, Klaus von Gleissenthall, Herbert Bos
- **方向**: 系统与操作系统安全
- **解读**: 内存安全方案（全面消毒、重编译加固等）因高开销、部署复杂或覆盖狭窄而难以普及；对"已知但尚未修复"的真实漏洞，开发者需要一种立即可用的止损补丁，在等待修复期间以最小代价阻断利用。

方法上，QuickSafe 不做运行时约束检查，而是把与漏洞相关的对象从程序其余部分"隔离"：传统架构上为易危对象分配专属页并配以不可访问的守卫页，支持内存标签扩展（MTE）的平台则强制不相交的标签域；并扩展 AddressSanitizer 提出 TagASan，用带标签的指针把故障访问追溯到原始分配点，以可靠定位待加固的错误。

评估上，作者构造含 223 个真实世界漏洞（横跨十个知名项目）的数据集，并在该基准、Juliet 测试套件与 SPEC CPU2006/2017 上评测：守卫页后端保护了全部评估漏洞，几何平均开销 2.46%–2.67%，绝大多数应用仅放缓约 1%；MTE 策略在代表性子集上开销约 0.12%。

我的思考：定位"已确认未修复漏洞"的定向加固非常务实——把资源花在最确定的风险上，全保护覆盖加 2.46% 量级开销的组合对真实部署有吸引力，MTE 近乎零开销更突出。局限：守卫页依赖页粒度分配，可能放大内存占用；数据集覆盖面与"已知漏洞"这一前提限制了适用范围，与系统化内存安全方案是互补而非替代关系。


#### NanoTag: Systems Support for Efficient Byte-Granular Overflow Detection on ARM MTE.
- **作者**: Mingkai Li, Hang Ye, Joseph Devietti, Suman Jana, Tanvir Ahmed Khan
- **方向**: 系统与操作系统安全
- **解读**: 内存安全漏洞（缓冲区溢出、释放后使用）占微软 70%、Android 51% 的漏洞；ASAN 等软件方案精度高但开销大（约 2 倍性能），无法在模糊测试中持续运行。ARM MTE 硬件检测开销低，但作者对首个生产实现（Google Pixel 8）的研究发现其精度粗：16 字节标签粒度导致粒内溢出（intra-granule overflow）漏检——Juliet 测试集中 MTE 漏检 24.32% 的堆上溢出（ASAN 检测 98.66%），SPEC CPU 2017 中 86.57% 的内存分配存在粒内溢出风险，并关联真实严重漏洞 CVE-2024-12084。

方法上，NANOTAG 在分配时识别可能发生粒内溢出的"短粒"（short granule），对其概率性安装绊线（tripwire）标签：硬件标签失配触发软件处理器，精确判断访问是否越界；元数据存放在粒内未用 padding 字节中，利用 MTE SYNC 模式的异常指令与故障地址推断访问偏移，避免影子内存开销，并以可调采样旋钮显式权衡检测能力与性能。

评估显示，NANOTAG 在 Juliet 上检测出 97.57% 的堆上溢出，接近 ASAN（98.66%），远超 MTE SYNC（75.68%）与 ASYNC（75.60%）；SPECrate Integer 几何平均开销 12.50%，与 Scudo MTE SYNC（11.98%）相当，仅为 ASAN（95.11%）的约七分之一；Geekbench 6 上 4.99%，Memcached/LevelDB/RocksDB 上最高 12.35%，Magma 模糊吞吐仅慢 15.86%（ASAN 为 111.20%）。

我的思考：这是首个在真实 MTE 硬件上实现字节粒度概率检测的系统，核心巧思是让硬件覆盖绝大多数访问、软件只查少量短粒，绕开了影子内存与插桩的双重开销，工程落地性强。局限在于检测是概率性的、主要针对溢出而非任意读写，且依赖 MTE SYNC 模式信息与 Scudo 分配器实现。与纯软件 sanitizer 相比，它代表"够用且便宜"的实用路线，对厂商推进更细粒度硬件标签有明确启示。

- **arXiv PDF**: [NanoTag: Systems Support for Efficient Byte-Granular Overflow Detection on ARM MTE.](https://arxiv.org/pdf/2509.22027)

#### SmuFuzz: Enable Deep System Management Mode Fuzzing in Fully Featured UEFI Runtime Environment.

- **作者**: Jianqiang Wang, Yi Xiang, Meng Wang, Qinying Wang, Ali Abbasi, Thorsten Holz

- **方向**: 系统与操作系统安全

- **解读**: 系统管理模式（SMM）是 x86 上特权级最高的固件执行环境，SMI 处理程序一旦存在漏洞即可被用于植入 rootkit 或绕过安全启动；但 SMM 代码依赖真实固件与硬件状态运行，传统 fuzzing 难以触达，缺乏在完整 UEFI 运行环境下对 SMM 深度测试的手段。

据标题推断，SmuFuzz 构建"全功能 UEFI 运行时环境"，让被测固件在接近真实的环境中启动运行，并针对 SMI 处理路径与 SMM 入口机制设计模糊测试方案，实现对该高特权代码面的深度覆盖与漏洞挖掘。

无摘要，无法引用具体量化结果；核心是提供可复现的 SMM fuzz 基础设施，扩大固件安全测试的覆盖面。

我的思考：SMM 漏洞利用链（SMM 提权、安全启动绕过）后果严重但测试门槛高，把 fuzzing 推进到固件运行时环境是有价值的工程方向；局限在于需真实硬件/固件配合，可扩展性与跨平台性有待观察。（无摘要，据标题推断）


#### Beyond Nodes vs. Edges: A Multi-View Fusion Framework for Provenance-Based Intrusion Detection.
- **作者**: Fan Yang, Binyan Xu, Di Tang, Kehuan Zhang
- **方向**: 系统与操作系统安全
- **解读**: 溯源图入侵检测（PIDS）面临固有粒度局限：节点中心检测器对良性新奇行为（软件更新、配置变更）敏感，复现中 MAGIC、NodLink 在 THEIA-E3 上产生超 9 万误报；边中心检测器缺乏实体上下文，攻击者用高频正常操作即可混过检测。作者通过评分可视化证明两类方法捕捉的是异常的不同侧面，任何单一视角都不完整。

方法上，PROVFUSION 在分数与决策两个阶段融合：分数阶段从属性、结构、因果三个视图出发，对每个系统实体按七个异常维度评分，经单调融合方案整合异构信号；决策阶段引入投票式自适应检测机制，由多维度投票结果做出最终异常判定，兼顾稳定性与上下文感知。

在九个广泛使用的数据集（DARPA TC 的 E3/E5 六个数据集加 OpTC 三个主机子集）上，PROVFUSION 在 TC 上平均 24.8 个真阳/6.2 个假阳，而节点级次优方法为 7.2/136.5；攻击级召回 13/14 对 12/14；OpTC 上平均 7 真阳/13 假阳，节点中心基线产生 10^4–10^5 量级假阳、边中心基线漏到不足 3 个真阳。消融实验证实多视图融合优于任一单视图变体。

我的思考：本文把"节点还是边"的争论转化为多视图融合问题，七维评分加投票机制设计完整，且开源并附带人工精修的 ground truth，对社区基准建设价值大。局限在于仍基于 GNN 局部推理、依赖良性训练数据分布，对抗性模仿攻击的评估深度有限。与 CAPTAIN 等侧重可解释性的 PIDS 相比，本文走的是纯精度路线，二者可互为补充。

- **arXiv PDF**: [Beyond Nodes vs. Edges: A Multi-View Fusion Framework for Provenance-Based Intrusion Detection.](https://arxiv.org/pdf/2604.14685)

#### Fine-Grained Kernel Auditing Using Augmented Syscall Reference Behavior Analysis and Virtualized Selective Tracing.

- **作者**: Chuqi Zhang, Spencer Faith, Feras Al-Qassas, Theodorus Februanto, Zhenkai Liang, Adil Ahmad

- **方向**: 系统与操作系统安全

- **解读**: 内核审计是检测恶意活动与合规审计的重要手段，但现有方案要么粒度粗（记录全部系统调用，噪声大、开销高），要么依赖脆弱的内核插桩；需要既细粒度又可扩展的审计方案。

据标题推断，该工作结合"增强的系统调用参考行为分析"与"虚拟化选择性追踪"：先为系统调用建立参考行为模型（正常行为基线），再借助虚拟化技术在相关执行路径上做选择性、细粒度追踪，只对异常或关键行为记录详细证据。

无摘要，无法引用具体量化结果；核心是以参考行为分析降低噪声、以虚拟化追踪降低开销的内核审计框架。

我的思考：用虚拟化技术做审计可降低对内核内部的侵入性，参考行为模型则缓解海量日志问题，思路务实；局限在于行为模型覆盖有限、选择性追踪的时机判定准确性，以及虚拟化层的性能开销与部署要求。（无摘要，据标题推断）


#### PLaTypus: Restricting Cross-Module Transitions to Mitigate Code-Reuse Attacks.

- **作者**: Apostolos Chatzianagnostou, Marcos Bajo, Christian Rossow
- **方向**: 系统与操作系统安全
- **解读**:
代码复用攻击（ROP/JOP）通过拼接合法代码片段实现任意计算，现有缓解（CFI、Intel CET、shadow stack）的强度取决于控制流策略的粒度：模块间（库、插件、动态加载代码）调用频繁且目标集合庞大，为兼容性常被放宽，恰好给攻击者留下绕过空间。如何在不破坏模块化软件性能与兼容性的前提下，对跨模块控制流转移实施严格约束，是代码复用防御的未解问题。（无摘要，据标题推断）

据标题推断，PLaTypus 提出限制跨模块转换（cross-module transitions）的缓解机制：对模块之间的跳转/调用实施细粒度目标约束（如仅允许跳转到显式导出的合法入口、按调用者-被调用者模块对进行白名单校验），可能结合编译期模块接口信息与现有硬件特性（如 CET 的 IBT）实现，把策略从"任意合法目标"收紧为"模块间许可转换"。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文报告该机制对真实代码复用攻击的阻断效果，以及相对基线缓解方案的性能开销与兼容性数据。（无摘要，据标题推断）

我的思考：模块边界是 CFI 的经典薄弱点，把控制流策略收紧到"模块间许可转换"既保留了模块化软件的灵活性，又大幅压缩攻击者可拼接的代码面，是务实且可部署的思路。局限在于需要可靠的模块接口信息、对动态加载与插件生态的兼容性，以及与主流缓解（CET、CFI-ratchet）的叠加成本，需全文确认；作者团队（Rossow 组）在利用与缓解对抗方向积累深厚，方法可信度较高。（无摘要，据标题推断）


#### Crashing Through Defenses: Exploiting Segfaults and Chaining Around Intel CET.

- **作者**: Marcos Bajo, Ritvik Goyal, Apostolos Chatzianagnostou, Christian Rossow
- **方向**: 系统与操作系统安全
- **解读**:
Intel CET（影子栈 + IBT）正被广泛部署以阻断代码复用攻击，其安全性依赖所有执行路径都正确维护影子栈与间接跳转约束。但程序崩溃（segfault）触发的信号处理路径——异常处理、信号处理器切换、返回时的栈与影子栈恢复——语义复杂且与正常调用路径不同，若存在缺口，攻击者可利用崩溃机制"绕过"CET 的检查，或把多次崩溃链式组合成可利用的原语。（无摘要，据标题推断）

据标题推断，论文系统研究 CET 启用环境下 segfault/信号处理场景的控制流行为，寻找影子栈与异常返回路径之间的语义不一致（如信号处理器对影子栈的维护差异、返回地址恢复时机），构造链式利用（chaining）技术，在启用 CET 的现代系统上实现绕过其防御的实际利用。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出针对真实应用与真实 CET 配置的绕过演示，以及利用链的可靠性评估。（无摘要，据标题推断）

我的思考：缓解方案的安全性取决于所有执行路径的一致性，异常/信号路径是典型的"边缘路径"，常在设计时被简化处理，本文延续"攻击主流缓解"的研究线，对 CET 的实际强度提出实证质疑。局限在于绕过是否依赖特定 CET 实现/硬件版本、是否需要额外漏洞原语配合，以及 Intel 缓解更新后的有效性，均需全文确认；与同团队的 PLaTypus（防御侧）对照阅读，可看到对 CET 攻防两端的完整视角。（无摘要，据标题推断）


### 硬件与物理安全（26 篇）

#### Keytar: Practical Keystroke Timing Attacks and Input Reconstruction.
- **作者**: Mufan Qiu, Lihsuan Chuang, Dohhyun Kim, Huaizhi Qu, Tianlong Chen, Andrew Kwong
- **方向**: 硬件与物理安全
- **解读**: 击键时序侧信道能够绕过系统安全机制窃取用户输入（口令、聊天内容等），但多数既有方案依赖高精度观测条件或特权环境，实用性受限。Keytar 研究实用化的击键时序攻击，并进一步从时序信息重建完整输入内容，直接威胁键盘输入的机密性——从“泄露节奏”到“还原明文”是威胁等级的本质跃升。

据标题推断，Keytar 通过低成本、易获得的观测信道采集按键时序信号，利用击键间隔（可能经键盘扫描、网络或音频信道）推断打字节奏，再结合语言模型对候选输入进行重建，把粗粒度的时序信息转化为可读文本。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；标题强调"实用"（practical），预期在普通设备、非特权攻击者的真实场景下验证攻击可行性与输入重建准确率。

击键攻击的价值取决于信道的可达性与重建精度：既有工作多证明"能泄露时序"，而"重建输入"才是把威胁落到实处的关键一步；作者 Kwong 曾以音频击键侧信道研究闻名，本工作延续其低成本高影响的风格。局限：时序重建受语言模型先验影响，对随机口令效果存疑；随机键盘布局与输入扰动能部分缓解此类威胁。


#### Phoenix: Rowhammer Attacks on DDR5 with Self-Correcting Synchronization.

- **作者**: Diego Meyer, Patrick Jattke, Michele Marazzi, Salman Qazi, Daniel Moghimi, Kaveh Razavi

- **方向**: 硬件与物理安全

- **解读**: DDR5 引入更强的 Rowhammer 缓解（更先进的 TRR 目标行刷新、片上 ECC 等），一度被认为基本遏制了该攻击；但 TRR 只能降低而非消除风险，且攻击与刷新的同步是成功率关键——漏掉同步会使攻击失效，DDR5 是否仍可被可靠攻击是悬而未决的问题。

作者提出自校正同步（self-correcting synchronization）：持续跟踪成千上万个刷新周期，一旦检测到漏掉的刷新即自动重新同步，从而可靠地对准 TRR 的时序窗口；配合对 DDR5 时序的逆向，在默认配置下稳定诱发位翻转，并构建端到端页表利用链。

在 15/15 条（2021–2024 年）DDR5 UDIMM 上全部成功诱发位翻转；端到端页表利用可在低至 109 秒内于商用系统获得 root 权限；将刷新率提高 3 倍可缓解 Phoenix，但带来约 8.4% 的 SPEC2017 性能开销。

我的思考：Phoenix 证明 DDR5 的"硬件缓解神话"不成立，是 Rowhammer 攻防的最新一轮升级，同步机制的工程细节扎实，对 TRR 时序的逆向方法具有借鉴价值。局限在于攻击需要持续大量访问（耗损 TRR 计数器），部分平台可能被检测；3× 刷新率缓解的高昂开销表明仅调刷新参数不够，作者主张按行激活计数器等硬件防御，方向正确但实现成本需产业评估。


#### GPUBreach: Privilege Escalation Attacks on GPUs Using Rowhammer.
- **作者**: Chris S. Lin, Yuqin Yan, Guozhen Ding, Joyce Qu, Joseph Zhu, David Lie, Gururaj Saileshwar
- **方向**: 硬件与物理安全
- **解读**: 
CPU 上的 Rowhammer 已被证明可实现提权，而 GPU（GDDR 显存）的 Rowhammer 此前仅能注入非定向位翻转破坏 ML 模型权重精度。本文提出首个 GPU 侧提权攻击 GPUBreach：通过 Rowhammer 位翻转篡改 GPU 页表，使非特权 CUDA 内核获得整块显存的任意读写，进而泄露密钥、篡改模型代码，甚至突破 IOMMU 实现 CPU 侧 root 提权。

方法上，攻击者修补开源 NVIDIA 驱动转储页表，发现页表位于连续 2MB"PT region"，首区耗尽后新区域与用户数据共用内存池。针对三个挑战给出：UVM 的 2MB+4KB 分配技巧迫使分配器产出 4KB 页框（1GB 显存即可填满 2MB 页表区，朴素方法需 256GB）；CPU 侧访问把 2MB 页劈成 64KB 片实现高密度填充（31/32 PTE 有效）；利用 UVM 换出延迟尖峰作侧信道探测新页表区分配时刻（每 508 次 4KB 分配一个尖峰），再以"逐出-重分配"循环把新页表区推入可翻转位的内存位置。端到端攻击约 17–20 秒。

结果在 RTX A6000（48GB GDDR6、ECC 关闭）上验证：6 个 bank 共 34 个位翻转，9 个适合页表提权（PFN 跳跃 16MB–32GB）。获得任意读写后，从 cuPQC 的 MLKEM512 密钥交换中在 1000 次运行里 44 次（4.4%）完整恢复私钥；篡改 cuBLAS 单个分支使 5 个模型精度从 57%–80% 降至约 0.1%（延迟变化 <6.62%）；最严重的是篡改 GSP 消息队列 elemCount（置 17）触发内核越界写覆盖 pMetaData，获得任意 4 字节内核写，改写 cred 的 euid=0 拿到 root shell，即使 IOMMU 开启。

我的思考：本文首次把 GPU Rowhammer 提升到与 CPU 同级，跨过设备-宿主边界，直接挑战"GPU 隔离由驱动与 GMMU 保证"的设计前提，页表按摩三项技术堪称系统性逆向成果。局限：依赖 ECC 关闭（两家云厂商默认如此）、需共驻与相邻行布局、画像每可用位翻转约 3 小时。代码篡改比权重篡改隐蔽得多，威胁 ML 推理供应链，也再次说明"信任设备数据"的边界需硬件级重审。
- **arXiv PDF**: [GPUBreach: Privilege Escalation Attacks on GPUs Using Rowhammer.](https://arxiv.org/pdf/2605.03812)

#### GDDRHammer: Greatly Disturbing DRAM Rows - Cross-Component Rowhammer Attacks From Modern GPUs.

- **作者**: Yichang Hu, Noah Brown, Yuhang Chen, Joshua Bakita, Tianlong Chen, Daniel Genkin, Andrew Kwong

- **方向**: 硬件与物理安全

- **解读**: 此前 GPU 上的 Rowhammer 研究仅观察到有限位翻转与有限影响，GPU 内存（GDDR6）的脆弱性程度及其能否跨组件影响主机内存缺乏系统研究；若 GPU 可作为大规模 Rowhammer 攻击面，将威胁 GPU 与主机之间的隔离。

作者对 GDDR6 实施系统性研究：逆向 GPU 的逻辑/物理内存映射与 warp 调度器行为，提出同步多 warp 锤击模式与几何感知的双面锤击（double-sided）策略，规避 TRR 并大幅提升位翻转数量；在跨 Ampere 与 Ada 两代的 25 块高端 NVIDIA GPU 上进行大规模实测。

平均每 DRAM bank 诱发约 129 次位翻转，约为先前工作的 64 倍；多数 RTX A6000 在现实配置下仍易受攻击；并演示端到端 GPU→CPU 跨组件利用：翻转 GPU 内存中的位即可经页表/PTE 重定向获得对全部主机 CPU 内存的读写访问。

我的思考：本文把 GPU Rowhammer 从"玩具级扰动"推进到"系统级跨组件利用"，量化揭示了 GDDR6 的脆弱性规模，是 GPU 硬件安全的重要实证贡献。局限在于位翻转成功率与内存型号、温度等因素相关，25 块 GPU 的样本虽广但特定型号的普遍性仍有限；GPU→CPU 利用依赖页表放置等条件，多租户场景的具体威胁需进一步评估。与 GeForge（页表伪造提权）互为补充，共同刻画 GPU Rowhammer 攻击全景。


#### GeForge: Hammering GDDR Memory to Forge GPU Page Tables for Fun and Profit.

- **作者**: Junpeng Wan, Yanan Guo, Zhi Zhang, Zhuo Li, Dave Jing Tian, Zhenkai Zhang

- **方向**: 硬件与物理安全

- **解读**: Rowhammer 已被广泛用于攻击系统主内存，近年研究显示 GPU 内存同样易受位翻转影响，但其安全影响此前探索不足。若攻击者能利用 GDDR 位翻转破坏 GPU 页表、夺取地址翻译控制，即可实现系统级攻陷，GPU 内存隔离形同虚设。

GeForge 提出端到端攻击：利用 GDDR 位翻转伪造 GPU 页表（主要瞄准 PD0），夺取地址翻译，实现对整个 GPU 内存的任意访问；并利用 GPU 页表的特殊映射特性把攻击范围从 GPU 内存延伸到主机内存。技术上发展出三项关键技术：将离线探测的物理映射对齐到运行时分配、借助厂商驱动分配器把页表结构"搬运"到易受攻击位置（memory massaging）、改进锤击模式触发更多位翻转，消除了先前工作的限制性假设。

在消费级与工作站级 NVIDIA GPU 上、默认配置下即可生效：可跨 GPU 上下文任意读写数据，并进一步提权至系统 root；相比先前工作，不再依赖特殊内存布局或非默认配置，实用性显著提升。

我的思考：GeForge 是 GPU Rowhammer 从"翻转演示"到"系统攻陷"的端到端证明，攻击链完整、工程性强，对 GPU 厂商（页表加固、分配器随机化、ECC）有直接警示。局限在于依赖特定页表结构与驱动分配行为，不同架构/驱动版本的可移植性有限；对主机内存的延伸受特殊映射特性约束；真实场景的稳定复现成本（锤击时间、成功率）仍需谨慎评估。与 GDDRHammer 的"跨组件扰动"互补，本文聚焦页表伪造的提权路径。


#### Defeating Transient Execution Attacks by Limiting Secret Reachability Through Register Hiding and ShadowCFI.

- **作者**: Daniël Trujillo, Jagadish Kotra, David Kaplan, Mengjia Yan

- **方向**: 硬件与物理安全

- **解读**: 瞬态执行攻击（Spectre/Mimus 类）利用投机路径泄露秘密，现有防御（序列化指令、加载-存储抑制等）要么开销巨大，要么覆盖不全。一个根本思路是限制秘密在微架构状态中的"可达性"（reachability）——让秘密根本不进入可被瞬态访问的路径，从源头压缩泄露面。

作者（MIT 与 AMD 合作）提出寄存器隐藏（register hiding）与影子 CFI（ShadowCFI）两项机制：寄存器隐藏约束敏感数据在寄存器堆中的移动与暴露路径，ShadowCFI 在影子状态中维护控制流完整性，防止瞬态执行期间通过控制流操纵泄露秘密；二者组合将秘密的可达性压缩到最小，并配套 gadget 分析工具。

论文展示该组合防御能阻断多类瞬态执行泄露向量，同时避免全量序列化带来的巨大开销；具体覆盖的攻击面与性能开销量化以原文实验为准。（无摘要，据公开摘要介绍推断）

我的思考：与"检测+清除"式防御不同，本文走"减少暴露面"路线，且与 AMD 硬件协同结合紧密，落地潜力大。局限在于组合防御的效果取决于秘密识别精度与影子状态完整性，复杂控制流与间接跳转场景仍需验证；对 LVI 类"秘密在内存而非寄存器"的变种效果需原文数据；硬件改动需求使其短期仅适用于新平台。这类思想与 ProSpeCT、ConTExT 等软硬协同方案互补，代表瞬态执行防御的下一阶段。


#### Transient Architectural Execution: From Weird Gates to Weird Programs.

- **作者**: Ping-Lun Wang, Fraser Brown, Riccardo Paccagnella, Eyal Ronen, Riad S. Wahby, Yuval Yarom

- **方向**: 硬件与物理安全

- **解读**: "怪异门"（weird gates）证明微架构状态（缓存、分支预测器）可作为计算媒介，但仅支持布尔电路级的小规模计算，表达力与效率受限；若能利用处理器的完整 ISA 把瞬态执行提升为通用"怪异程序"，微架构计算将具备实际利用价值（侧信道放大、恶意软件混淆、隐蔽计算）。

作者提出瞬态架构执行（TAE）原语，三步范式：将微架构状态复制进瞬态寄存器、用 ISA 指令在其上计算、将结果写回微架构状态；利用分支预测器/BTB 状态模拟宽变量（最高 16 位），借助条件执行、索引访存与丰富数据类型克服怪异门的布尔电路局限，把"怪异门"推广为"怪异程序"。

基于 TAE 的"怪异程序"比怪异门快两个数量级以上，并能计算此前方法难以实现的函数；计算在瞬态执行结束后对架构状态不可见，为不可见计算提供新载体；工作统一并推广了怪异门与瞬态执行攻击的概念。

我的思考：该工作把微架构隐蔽计算从"玩具电路"推向"可编程机器"，概念贡献突出，对侧信道放大与恶意代码混淆有直接安全影响。局限在于 TAE 可用性依赖特定微架构特性（BTB 状态可观测性），跨厂商/代际可移植性存疑；16 位宽度仍限制计算规模；防御方如何检测这类"无架构痕迹"的计算是开放问题。作为微架构"怪异机器"研究的里程碑，为隐蔽计算与侧信道研究提供了新范式。


#### SeqAss: Using SeqUential Associative Caches to Mitigate Conflict-Based Cache Attacks with Reduced Cache Misses and Performance Overhead.

- **作者**: Wei Song, Zhidong Wang, Jinchi Han, Da Xie, Hao Ma, Peng Liu

- **方向**: 硬件与物理安全

- **解读**: 基于冲突的缓存侧信道攻击（如 Prime+Probe、Evict+Time）利用缓存集冲突泄露访存模式，威胁密码实现与隔离边界；现有防御（随机化映射、偏斜缓存、缓存分区）通常以显著增加缓存缺失率与性能开销为代价，实用化受限，安全与性能难以兼得。

作者提出顺序关联缓存（SeqUential Associative Cache, SeqAss）：重组缓存行的映射与访问方式，使冲突行为对攻击者难以预测或利用，从而缓解冲突型侧信道泄露，同时避免传统随机化方案在缺失率与开销上的损失；面向最后一级缓存（LLC）给出具体微架构设计。

论文表明 SeqAss 在缓解冲突型缓存攻击的同时，相比现有方案降低缓存缺失率与性能开销；具体的攻击成功率下降幅度与性能量化数据以原文实验为准。（无摘要，据公开摘要介绍推断）

我的思考：思路是以"顺序化映射"换取攻击者不可预测性，在安全性与性能之间寻找比随机化更优的权衡，与作者此前随机化集合关联缓存的工作形成延续。局限在于顺序化映射可能引入新的时序/能耗副作用；与非冲突型侧信道（如时间攻击）的对抗效果待验证；硬件改动需要产业采纳。若性能数据扎实，SeqAss 有望成为 LLC 抗冲突攻击设计的实用候选方案。


#### Trevex: A Black-Box Detection Framework for Data-Flow Transient Execution Vulnerabilities.

- **作者**: Daniel Weber, Fabian Thomas, Leon Trampert, Ruiyi Zhang, Michael Schwarz

- **方向**: 硬件与物理安全

- **解读**: 数据流型瞬态执行漏洞（Meltdown 类）持续被发现，但厂商文档滞后、RTL 不可得，芯片量产后的黑盒环境中如何自动发现未知漏洞是现实难题；现有检测多依赖已知瞬态窗口或具体泄露模式，覆盖面有限。

Trevex 提出三项创新：数据流发散测试（data-flow divergence testing）——检测非预期的瞬态数据流而非仅探测已知窗口；指令影子化（instruction shadowing）——对任意微架构元素打 taint，突破内存子系统的限制；跨特权数据依赖检测——推断泄露是否源自受害者的特权域；以 CPU 模糊测试方式在 20 种微架构（Intel、AMD、兆芯）上运行。

发现 AMD Zen/Zen+ 上的 FP-DSS（浮点除法器状态采样）：可跨域泄露数据（包括来自 Linux 内核的数据），并以 JavaScript PoC 演示从浏览器泄露原生进程数据；发现无需 denormal 输入的新 FPVI 变体；发现三例 Zero-at-ret（LVI-NULL 变体），其中部分为公开文档未记载的 CPU 影响。

我的思考：Trevex 展示了"无 RTL 黑盒发现未知瞬态漏洞"的可行范式，FP-DSS 等新漏洞说明厂商验证仍不充分，工具化方法对回归测试与安全审计有直接价值。局限在于黑盒方法依赖测试程序生成与 taint 推理的完备性，可能漏掉依赖特定时序的漏洞；对纯时序型侧信道的覆盖有限；fuzz 成本较高，覆盖与效率之间需权衡。与硬件设计验证工具形成互补，是后硅验证的重要补充。


#### Crucible: Retrofitting Commodity CPUs with Vulnerabilities via Transparent Software Emulation.

- **作者**: Tristan Hornetz, Lukas Gerlach, Michael Schwarz

- **方向**: 硬件与物理安全

- **解读**: 瞬态执行漏洞（Meltdown、Foreshadow、MDS）研究依赖易受影响的特定硬件，硬件稀缺、不可复现、难以评测检测工具与教学；随着受影响芯片逐渐退出市场，如何在不依赖脆弱硬件的情况下可重复地"拥有"这些漏洞成为紧迫问题。

Crucible 是纯软件框架：在独立进程中影子化（shadow）指令流，透明模拟 Meltdown 类瞬态执行漏洞的微架构效应（缓存泄露、瞬态窗口、fence 行为），无需修改攻击者代码；可选内核模块用于需要内核/物理地址访问的漏洞配置；支持信号处理、TSX 与自定义性能计数器（PMC）模型，可同时配置多个含合成的漏洞场景。

未受影响的 x86 CPU 上即可运行未修改的公开 PoC 与完整利用（如演示从 VeraCrypt 提取密钥）；支持仅二进制的应用；可与模糊测试器集成以检测被模拟的漏洞；支撑检测工具基准测试与教学。

我的思考：Crucible 把"硬件漏洞"变成"可移植软件资产"，解决瞬态执行研究的可复现性危机，对检测工具评测、教学与历史漏洞归档（Meltdown 等终将退出硬件）意义重大。局限在于模拟保真度取决于微架构效应建模精度，深层时序行为可能与真实硬件有偏差；性能开销限制大规模 fuzz；目前聚焦 x86 生态，RISC-V/ARM 场景未覆盖。这是"以软件模拟补齐硬件退场"的务实基础设施。


#### RISCy Cache Coherence: Timer-Free Architectural Cache Attacks via Instruction/Data Cache Incoherence.

- **作者**: Fabian Thomas, Michael Schwarz

- **方向**: 硬件与物理安全

- **解读**: 缓存侧信道攻击通常依赖高精度计时器（如 Flush+Reload），在计时器受限或被扰动的环境下失效；若能以"架构级可观察结果"替代计时，攻击将更隐蔽、更稳健，且可绕过以计时器防御为核心的缓解体系。

作者利用 RISC 处理器指令/数据缓存不一致（I/D incoherence）：自修改代码场景下，数据存储与指令取指对同一地址的视图不同，形成架构级（而非时序级）泄露。构建 I²SC 的两个积木——B1 架构级预言机（检测目标行是否驻留 I-cache）与 B2 传输 gadget（基于瞬态执行把 D-cache 驻留状态复制进攻击者控制的 I-cache 行），从而得到无需计时器的通用缓存预言机；在 RISC-V、ARM、LoongArch 等 18 种微架构上评估。

12 种微架构受该不一致性影响；演示了无计时器 AES 密钥恢复、跨三种架构的类 Spectre 架构级泄露，以及针对 Android 共享库恢复细粒度触摸事件时序的经典侧信道攻击；完整防御需硬件改动，软件缓解只能部分奏效。

我的思考：本文找到一条绕过"计时器防御"的通用路径——用架构状态编码微架构状态，把时序信道转为架构信道，显著提升侧信道攻击的稳健性与隐蔽性，是可作 Flush+Reload 替代的通用原语。局限在于 B2 依赖瞬态执行机制，其可用性受 Spectre 缓解影响；12/18 的受影响比例说明并非全架构通用；根治仍需硬件一致性保证。对"计时器防御是否值得"的讨论有重要启示。


#### Hardware Trojans from Invisible Inversions: On the Trojanizability of Standard Cell Libraries.
- **作者**: Kolja Dorschel, René Walendy, Lukas Plätz, Thorben Moos, Christof Paar, Steffen Becker
- **方向**: 硬件与物理安全
- **解读**: 

**问题**：硬件木马检测研究依赖 SEM 图像数据集；Puschner 等人（S&P 2023）公开了 4 颗不同制程节点的数字 IC 的完整背面 SEM 图像数据集，并初步观察到特征尺寸影响检测性能，但无法区分"插入策略/图像质量退化"与"标准单元库固有属性"两类成因。区分二者对判断"更好的成像设备能否消除偏差"、木马易植入性是否内生于库，至关重要。

**方法**：深入分析该 S&P 2023 数据集，设计替代性度量指标以更有意义地评估和比较不同标准单元库的"可木马化性"（Trojanizability）；并利用背面 SEM 下视觉不可区分的单元，实际构造隐蔽的基于标准单元的硬件木马。

**结果**：发现不同制程节点之间存在清晰差异，但在所有评估案例中，实现不同逻辑功能的单元在背面 SEM 图像下视觉上不可区分；据此成功构造隐蔽木马，并给出具体案例研究——在 Ibex RISC-V 核中植入权限提升后门。结论表明"可木马化性"应作为库的固有属性被评估，并据此推荐实用防御措施。

**我的思考**：将问题从"检测木马"转向"评估库的可木马化性"是视角升级——成像分辨率提升无法消除不可区分单元这一结构性盲区；Ibex 后门案例证明威胁真实可行。局限在于仅覆盖 4 颗 IC 与背面成像视角，正面/多层成像与更先进节点下的结论待扩展。对代工厂信任、晶圆厂检查标准与单元库设计（如等功耗/等形单元）均有直接启示。


#### Banshee: Target Switch Attacks on Gimbal-Stabilized Visual Tracking Systems via Acoustic Injection.
- **作者**: Jiarui Li, Joseph Brewington, Qingzhao Zhang, Z. Morley Mao
- **方向**: 硬件与物理安全
- **解读**: 
云台式视觉跟踪系统广泛应用于安防监控、无人机与机器人，依靠云台转动持续锁定目标。若攻击者能通过物理信号干扰其跟踪逻辑，使云台转向错误目标或丢失锁定，将直接破坏监控与安防效能；声学注入作为无需接触的物理攻击向量，其现实威胁值得警惕。

据标题推断，Banshee利用声学注入（acoustic injection）干扰云台稳定系统的传感器或控制回路，诱导视觉跟踪系统发生"目标切换"（target switch），把跟踪对象从真实目标错误地切换到攻击者指定的其他目标，从而规避监控或制造混乱；攻击应针对云台控制链中的物理信号处理环节实施。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应展示攻击在不同云台系统、距离与声学参数下的有效性，并讨论可能的防御对策，具体数据以全文为准。

物理层攻击研究（声学、激光、电磁干扰）近年受到安全顶会高度关注，Banshee把目标设定为"让跟踪系统锁定错误对象"，比单纯使系统失效更具威胁性与研究新意。局限在于声学注入的可行性依赖具体硬件特性（传感器共振频率等），泛化性需要验证；若能同时给出可落地的检测或抗干扰机制，对安防与机器人系统的物理安全设计具有直接参考价值。


#### Chypnosis: Undervolting-based Static Side-channel Attacks.
- **作者**: Kyle Mitard, Saleh Khalaj Monfared, Fatemeh Khojasteh Dana, Robert Dumitru, Yuval Yarom, Shahin Tajik
- **方向**: 硬件与物理安全
- **解读**: 
侧信道攻击通过功耗、电磁等物理可观测量恢复秘密信息，传统攻击多为"动态"式，即在目标运行加密操作时采集轨迹。若能在设备"静态"（空闲或未执行目标操作）的状态下实施攻击，将大幅放宽攻击条件并规避现有对策；电压调控（undervolting）引发的受控故障为这类静态攻击提供了新途径。

据标题推断，Chypnosis利用欠压（undervolting）手段使目标设备（推测为FPGA或嵌入式系统）进入受控故障状态，在静态条件下诱发并观测与秘密相关的物理特征，从而实施侧信道攻击；核心思路是把电压调控作为攻击入口、把设备状态作为信息泄露的载体。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告攻击在不同电压水平、设备与密钥条件下的成功率与可重复性，具体数据以全文为准。

该工作由Yarom（Spectre共同作者）与Tajik（硬件安全专家）等参与，把欠压从"可靠性故障"重新定义为"攻击工具"，视角新颖。静态攻击若成立，将绕过许多依赖运行期采样的对策，威胁模型更隐蔽。局限在于欠压控制的精度与设备特异性可能限制通用性，且往往需要物理接触或调试接口；与动态功耗分析相比，其适用范围与信噪比有待全文数据支撑。对硬件设计者而言，电压管理的安全影响应纳入设计考量。


#### SoK: Systematizing a Decade of Architectural Rowhammer Defenses Through the Lens of Streaming Algorithms.
- **作者**: Michael Jaemin Kim, Seungmin Baek, Jumin Kim, Hwayong Nam, Nam Sung Kim, Jung Ho Ahn
- **方向**: 硬件与物理安全
- **解读**: 
Rowhammer自2014年被发现以来，已从DRAM单元故障演变为真实可利用的提权与逃逸攻击，十年间学界提出大量架构级防御方案（计数、随机化、刷新调度等），但方案林立、术语不一，缺乏统一比较框架，研究者与工业界难以判断各方案的真实防护能力与开销权衡。

据标题推断，该SoK论文引入流式算法（streaming algorithms）视角系统化审视架构级Rowhammer防御：将各类防御抽象为对流式访问序列的统计或跟踪问题，用统一的复杂度与精度框架刻画检测能力、误报率与硬件开销，从而对十年间的防御方案进行归类、比较并提炼设计空间。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文的产出应包括系统的防御分类学、统一比较指标与未来研究方向建议，具体数据以全文为准。

SoK的价值在于建立共同语言：流式算法理论为Rowhammer防御提供了天然的精度—开销权衡框架（如计数器位数与漏检率的关系），这一类比颇具洞见，可解释为何许多方案在资源受限下难以两全。作者来自存储与架构方向（首尔大学等），对DRAM细节把握可靠。局限在于流式视角可能弱化具体电路实现差异；该框架有望成为后续Rowhammer防御研究的参照系。


#### RadKey: An LLM-Guided RF Backscatter System for Through-Wall Keystroke Inference.
- **作者**: Qijun Wang, Chunqi Qian, Huacheng Zeng
- **方向**: 硬件与物理安全
- **解读**: 
键盘输入泄露是物理侧信道的重要主题：若能隔着墙壁感知用户的击键，攻击者即可窃取口令、密码等敏感信息。现有穿墙击键感知依赖高精度射频设备与复杂信号处理，成本高且难以自动化；如何以低成本设备实现高精度的穿墙击键推断是实际挑战。

据标题推断，RadKey利用RF反向散射（backscatter）技术感知击键引起的身体与手指运动，并引入大语言模型（LLM）引导推断流程，例如由LLM结合上下文与按键规律辅助候选序列的生成与校正，降低信号模糊性带来的错误率，形成"低成本射频感知+智能推断"的击键窃取系统。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告穿墙场景下的击键识别准确率与词级恢复效果，具体数据以全文为准。

将LLM引入物理侧信道推断是该工作的最大亮点：物理信号常含歧义，语言先验能显著提升按键序列恢复的准确性，这一"信号+语言先验"的组合思路可能启发其他传感类攻击。反向散射方案成本低、隐蔽性强，威胁模型贴近现实。局限在于击键推断准确率高度依赖目标打字习惯与距离，LLM先验也可能对非常规输入产生偏差；总体而言是低成本物理攻击与AI结合的有趣探索。


#### INSIGHT: Automatic Generation of Explanations for Efficient Identification of Hardware Bugs and Underspecifications.
- **作者**: Vincent Quentin Ulitzsch, Alessandro Bertani, Peter W. Deutsch, David Langus Rodriguez, Kelly Xu, Aarti Gupta, Sharad Malik, Mengjia Yan
- **方向**: 硬件与物理安全
- **解读**: 处理器等硬件设计中的安全缺陷与"欠规格"（underspecification，行为未被规格覆盖）极难发现：形式化验证能报告违反，但海量反例与抽象的报错信息让设计者难以定位根因，调试成为硬件安全的瓶颈。（无摘要，据标题推断）

据标题推断，INSIGHT 自动生成"解释"以辅助硬件调试：对验证失败或语义空缺，自动提炼最小化、可理解的解释（如差异输入、关键信号路径、反例聚类），把验证结果转化为设计者可直接行动的信息，从而加速缺陷定位与规格补全；方法可能结合模型检查、反例最小化与可解释性技术。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在真实处理器或安全设计案例上报告解释的质量与定位效率的提升。（无摘要，据标题推断）

我的思考："能报错"到"能解释"是从验证走向调试的关键一跃，对硬件安全设计（如 Spectre 类问题的验证）价值直接；作者含 Malik 与 Yan（硬件验证与微架构安全权威），方法可信。局限：无摘要，解释生成的可扩展性与客观评价指标（是否人工评测）需正文确认；解释的正确性保证（不会误导）也是严肃问题。（无摘要，据标题推断）


#### Interplay of Efficient Model Checking and Secure Processor Design: A Case Study on Secure Speculation.
- **作者**: Tingzhen Dong, Qinhan Tan, Kunpeng Wang, Thomas Bourgeat, Yuheng Yang, Sharad Malik, Yu-Wei Fan, Mengjia Yan
- **方向**: 硬件与物理安全
- **解读**: 安全处理器设计（如安全推测执行机制）在加入防御后，正确性验证成为难题：机制与微架构交互复杂，手工推理易漏，而通用模型检查在处理器规模上往往不可扩展；安全机制的设计与形式化验证需要协同演进。（无摘要，据标题推断）

据标题推断，本文以安全推测执行为案例研究"高效模型检查与安全处理器设计"的交互：把设计问题编码为可高效求解的验证问题，或将验证约束纳入设计空间探索，使模型检查在真实设计流程中可落地，并以此发现或避免安全机制的缺陷。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告模型检查在案例上的可扩展性、发现的问题及对设计流程的改进。（无摘要，据标题推断）

我的思考：安全机制"加了不等于对了"——防御本身引入的新漏洞需要形式化保障；把验证前移到设计期能显著降低成本，选题示范意义强。作者含 Malik、Bourgeat（安全微架构）与 Yan，阵容权威。局限：无摘要，案例规模能否推广到完整处理器、验证抽象与真实时序语义的差距需正文确认。（无摘要，据标题推断）


#### Fractal: An Operating System Designed for Microarchitecture Reverse Engineering.

- **作者**: Joseph Ravichandran, Mengjia Yan

- **方向**: 硬件与物理安全

- **解读**: 微架构逆向工程（发现新侧信道、瞬态执行漏洞）需要精确控制与观测微架构状态，但通用操作系统（调度、中断、虚拟内存）引入大量干扰，研究者只能靠内联汇编与计时库拼凑测量环境，效率与精度受限。

据标题推断，Fractal 是首个专为微架构逆向工程设计的操作系统：提供精确的时序测量原语、确定性执行环境与细粒度硬件状态控制（缓存、TLB、预取器行为观测），把微架构探测从"在通用 OS 上挣扎"变为"专用 OS 原生支持"。

无摘要，无法引用具体量化结果；核心是提供可复现、低噪声的微架构测量平台，支撑侧信道与瞬态执行研究。

我的思考：该团队曾提出 WeSee、Pathfinder 等瞬态执行攻击，此工作把攻击者常用的微架构探测能力"产品化"为研究基础设施，可能显著降低测量门槛、加速新侧信道发现；局限在于专用 OS 与真实运行环境的差异，以及硬件代际演进的维护成本。（无摘要，据标题推断）


#### AEX-NStep: Probabilistic Interrupt Counting Attacks on Intel SGX.

- **作者**: Nicolas Dutly, Friederike Groschupp, Ivan Puddu, Kari Kostiainen, Srdjan Capkun

- **方向**: 硬件与物理安全

- **解读**: Intel 为 SGX 引入 AEX-Notify 扩展以缓解基于中断的单步攻击（如 SGX-Step），其目标之一是混淆前向进度、阻止确定性单步执行；需检验该缓解是否真正杜绝中断类攻击。

作者提出 AEX-NStep——首个针对启用 AEX-Notify 的 enclave 的中断计数攻击：证明单步并非攻击实用性的必要条件，指出 AEX-Notify 的安全保证"混淆前向进度"不成立，构造两种新的概率性技术，并据此实现对启用 AEX-Notify 的 enclave 的 ECDSA 密钥泄露。

实验表明 AEX-Notify 未能完全阻止此类攻击；结果扩展了原始分析，为未来缓解设计提供依据。

我的思考：这是对 AEX-Notify 安全声明的系统性再检验，说明"防单步"不等于"防中断计数"，攻击面分析细致；概率性攻击对噪声与硬件行为的建模是关键，也为依赖中断进行侧信道观测的既有攻击（如 SGX-Step 系列）提供了新的继承路径；局限在于攻击成功率及与特定 enclave/算法的适配性；对 SGX 后继平台（TDX）的防御设计有借鉴意义。


#### Battering RAM: Low-Cost Interposer Attacks on Confidential Computing via Dynamic Memory Aliasing.

- **作者**: Jesse De Meulemeester, David F. Oswald, Ingrid Verbauwhede, Jo Van Bulck

- **方向**: 硬件与物理安全

- **解读**: 机密计算 TEE（SGX/TDX、SEV-SNP）依赖硬件级内存加密对抗特权软件与物理攻击（总线嗅探、冷启动）；新扩展加入访问控制检查以防御基于软件的密文操作与别名攻击，但物理攻击者的能力边界未被充分检验。

作者构建低成本（远低于商用设备价格）的 DDR4 内存内插器（interposer），动态篡改地址线制造"动态内存别名"，绕过现有 TEE 防护；其运行时动态特性可规避为早期攻击引入的启动期固件缓解。

首次对 Scalable SGX 的单密钥域发起攻击，实现任意明文读/写并提取平台供给密钥（platform provisioning key），瓦解远程证明的信任基础；还可在最新的 SEV-SNP 平台上重新启用完整的证明破坏，绕过近期针对静态别名的防御；设备成本比商用 DRAM 内插器低数个数量级。

我的思考：把"物理攻击者"重新拉回机密计算的威胁模型，证明仅靠内存加密与访问控制不足以对抗低成本硬件篡改，对可信硬件供应链与 TEE 设计（地址线完整性校验）提出直接挑战；局限在于需物理接触与专用硬件搭建，攻击场景受限；与 VoltPillager 等内插器工作共同扩展了硬件攻击工具箱。

#### TEE.Fail: Breaking Trusted Execution Environments via DDR5 Memory Bus Interposition.

- **作者**: Jalen Chuang, Alexander Seto, Nicolás Berrios, Stephan van Schaik, Christina Garman, Daniel Genkin
- **方向**: 硬件与物理安全
- **解读**:
TEE（如 Intel SGX、AMD SEV、Intel TDX）把机密性承诺建立在内存加密之上，但 CPU 与 DRAM 之间的内存总线承载着加密数据与关键控制信号，其物理防护假设值得审视：攻击者若能以物理方式对 DDR5 总线实施插接（interposition），旁路探测或注入，理论上即可绕过 TEE 的内存保护边界。DDR5 总线速率更高、信号组织更复杂（命令/地址/数据线分离、片上 ECC），为这类攻击提供了新的作用面。（无摘要，据标题推断）

据标题推断，方法核心是构造 DDR5 内存总线插接装置（转接/探测板或直接接入总线走线），在真实 TEE 平台上对运行期内存流量实施窃听与故障注入：利用可观测的命令/地址线同步数据线信号，结合时序与功耗信息对齐采样窗口，读取加密内存背后的明文或破坏内容完整性。作者团队（Genkin 组）在物理侧信道与内存总线攻击方向有系列积累。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文以真实 DDR5 平台实验演示对 TEE 的破坏，可能涵盖窃听（机密性）与故障注入（完整性）两类能力及适用性。（无摘要，据标题推断）

我的思考：物理攻击门槛高（拆机、专用设备、信号工程），但对机密计算场景属于必须纳入威胁模型的现实风险。该工作与针对 SGX 的 DRAM 总线插接攻击一脉相承，把物理内存总线攻击推向 DDR5 与更新的 TEE 生态，倒逼厂商重估内存加密密钥管理与总线信号防护的设计假设。局限在于物理接触前提、实现成本，以及厂商缓解下攻击的对抗强度，需阅读全文确认。（无摘要，据标题推断）


#### PrintSpy: Pixel-Level Eavesdropping on Commodity Laser Printers via Electromagnetic Side Channels.

- **作者**: Wenhao Li, Jiarong Yang, Mingda Han, Xiuzhen Cheng, Pengfei Hu, Cong Wang
- **方向**: 硬件与物理安全
- **解读**:
激光打印机在打印过程中，扫描单元电机、高压部件等机电组件会辐射电磁信号；若这些电磁泄漏与逐行打印的内容存在统计关联，旁路攻击者即可在物理隔离或受限环境下重建被打印文档的文本，直接威胁打印文档的机密性。此前打印机电磁侧信道研究多停留在"能否恢复"层面，缺乏对恢复精度的系统性推进。（无摘要，据标题推断）

据标题推断，PrintSpy 对商用激光打印机工作时的电磁辐射进行采集，建立扫描电机驱动信号与打印内容的时序对应关系，通过去噪、行同步与逐行对齐等信号处理手段，把电磁信号映射回像素级文本；标题强调"像素级"（Pixel-Level），意味着其目标是把重建精度从字符/词级别提升到可辨认细节的程度。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文报告了在多种打印机、字体与排版条件下文本重建的准确率评估，以支撑"像素级窃听"的主张。（无摘要，据标题推断）

我的思考：把电磁侧信道的文本重建精度推向像素级，是从"泄露存在性"迈向"实际可用窃听"的关键一步，对政府、律所、企业等打印敏感文档的场景有直接警示意义。局限在于攻击距离与是否需要贴近设备、字体/格式变化下的鲁棒性、以及与其他泄露路径（声学、功耗）的组合潜力，均需全文确认；此类工作也应与厂商的电磁屏蔽与随机化缓解措施对照评估。（无摘要，据标题推断）


#### Goldilocks and the Three P-States: Mitigating Hertzbleed with Formal Leakage Guarantees.

- **作者**: Inwhan Chun, Christine Guo, Riccardo Paccagnella
- **方向**: 硬件与物理安全
- **解读**:
Hertzbleed 揭示了一个普遍存在的频率侧信道：CPU 睿频/P 状态调节使执行时间随数据依赖的功耗变化，攻击者仅凭计时即可恢复密码学运算中的秘密。彻底的缓解（禁用睿频）性能代价高昂，放任不管则持续泄露，如何在两者之间找到"刚刚好"（Goldilocks）的操作点是该领域的关键难题。（无摘要，据标题推断）

据标题推断，论文形式化刻画 P-state 配置与信息泄漏量之间的关系，为不同配置给出形式化的泄漏保证（如每操作可泄露比特数的上界），并据此系统评估三态（如最高性能、折中、保守）配置，找到既保留大部分性能又具有可证明泄漏上界的操作点；方法可能基于公开的功耗-频率模型与静态分析，而非仅依赖经验测量。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文以形式化界加实验验证的方式，展示在可证明的泄漏保证下相对"完全禁用睿频"的性能保留幅度，以及该保证对真实密码实现的适用性。（无摘要，据标题推断）

我的思考：Hertzbleed 类缓解的难点在于性能与安全是连续权衡而非二选一，把泄漏量形式化并给出可证明上界，使缓解从"启发式关功能"升级为"可论证的工程设计"，这是本文的核心价值。局限在于 P-state 粒度与平台相关性、功耗-频率模型假设的准确性，以及多核/虚拟化场景下频率竞争的建模难度，需全文确认；与常量时间实现、掩码等经典对策的组合效果也值得关注。（无摘要，据标题推断）


#### TDXRay: Microarchitectural Side-Channel Analysis of Intel TDX for Real-World Workloads.

- **作者**: Tristan Hornetz, Hosein Yavarzadeh, Albert Cheu, Adrià Gascón, Lukas Gerlach, Daniel Moghimi, Phillipp Schoppmann, Michael Schwarz, Ruiyi Zhang
- **方向**: 硬件与物理安全
- **解读**:
Intel TDX 通过内存加密与缓存划分（TDX partitioning）等机制承诺缓解微架构侧信道，但机密计算的安全边界最终取决于真实工作负载下这些机制的实际强度：共享的页表、翻译缓存、填充缓冲等微架构资源是否仍能在可信域（TD）与 VMM/其他 TD 之间传递信息，尚无系统的实证刻画。（无摘要，据标题推断）

据标题推断，TDXRay 对 Intel TDX 开展系统的微架构侧信道分析：构建能够跨越 TDX 信任边界传递信息的侧信道原语（缓存、页表/翻译、时序类信道），并在贴近真实部署的工作负载（而非玩具程序）场景下测量信道可达性、带宽与噪音特征，可能包括与无 TDX 基线及软件分区方案的对比。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出真实工作负载下的侧信道可达性与泄露速率评估，以及各缓解机制在实际使用中的残余风险。（无摘要，据标题推断）

我的思考：TDX 的安全承诺需要独立实证而非厂商声明，"真实工作负载"的定位使结论贴近部署现实，作者阵容（Schwarz、Moghimi 等微架构安全一线团队）也预示方法的严谨性。局限在于结论对 TDX 版本、平台与缓存划分配置的强依赖性，以及微架构侧信道分析固有的环境噪音问题；该工作与 SGX 时代的侧信道研究一脉相承，共同收紧了机密计算的可信边界。（无摘要，据标题推断）


#### BreakFAST: Confused Deputy Attack on Infinity Fabric to Break AMD SEV-SNP.

- **作者**: Philipp Giersfeld, Benedict Schlüter, Shweta Shinde
- **方向**: 硬件与物理安全
- **解读**:
AMD SEV-SNP 承诺在不可信 hypervisor 下保护虚拟机内存的机密性与完整性，其安全边界建立在 RMP（反向映射表）与内存加密之上。但平台中还存在大量有能力访问内存的"代理"——IO 设备、南桥、Infinity Fabric 上的各类组件；若攻击者能以"混淆代理"（confused deputy）方式诱导某个高权限代理代为访问受保护内存，即可绕过 SNP 的完整性校验与机密性保护，使承诺失效。（无摘要，据标题推断）

据标题推断，论文分析 AMD Infinity Fabric（片内互连）上可被攻击者操纵的组件与协议行为，构造混淆代理攻击链：让某个本应可信/受限的代理在攻击者诱导下执行对 SEV-SNP 保护内存的读写或校验操作，从而绕过加密与 RMP 约束，并在真实 AMD 平台上验证攻击。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文以真实 AMD 平台实验演示对 SEV-SNP 的破坏，说明攻击所需的权限前提与影响范围。（无摘要，据标题推断）

我的思考：SEV-SNP 的威胁模型隐含"平台组件可信"的假设，Infinity Fabric 生态引入的代理面（IO、固件、安全处理器）常被低估，本文与同期的 SEV 系列攻击共同收紧了机密计算的可信边界，对云厂商评估 SEV-SNP 部署风险有直接价值。局限在于攻击前提（所需权限、物理访问、固件版本）与 AMD 缓解（固件/微码更新）下的对抗强度，需全文确认。（无摘要，据标题推断）


### 移动与物联网安全（7 篇）

#### Convenience at a Cost: the Security Risks of Template-Based Development in the App-in-App Ecosystem.

- **作者**: Yizhe Shi, Zhemin Yang, Yifan Yang, Yunteng Yang, Min Yang

- **方向**: 移动与物联网安全

- **解读**: 小程序（app-in-app）生态中，模板化开发极大降低开发门槛，但也可能系统性引入安全缺陷：模板携带的通用漏洞、过度权限与不当配置随模板复用扩散到大量小程序，形成供应链式风险；"便利"背后的安全代价缺乏系统量化。（无摘要，据标题推断）

作者系统分析小程序模板化开发的安全影响：考察模板如何引入通用漏洞、权限滥用与配置问题，结合静态分析与生态测量刻画模板复用带来的风险传播路径，并评估平台侧审核与缓解机制的覆盖情况。（无摘要，据标题推断）

论文揭示模板化开发与安全缺陷之间的关联模式，说明模板复用使漏洞"一次编写、多处受害"；具体受影响范围与量化数据以原文为准。（无摘要，据标题推断）

我的思考：该工作把"模板即供应链"视角带入小程序安全，与作者团队此前的凭证泄露等小程序安全研究一脉相承，对超应用平台的开发者工具、模板审核与沙箱机制有直接改进价值。局限在于模板生态多样（官方/第三方、低代码/代码生成），覆盖范围需审视；模板与漏洞的因果性（区别于开发者自身写坏代码）需要严谨对照；缓解措施的实际落地效果待评估。对国内小程序生态治理具有现实参考意义。

#### When VR Meets BCI: (Un)Observable Brainwave-Aware Privacy Reconstruction in the Metaverse via Unrestricted Inbuilt Motion Sensors.
- **作者**: Tao Ni, Zehua Sun, Qingchuan Zhao, Wei-Bin Lee, Cong Wang
- **方向**: 移动与物联网安全
- **解读**: 

**问题**：VR头显等元宇宙设备的运动传感器（加速度计等）访问不受限制（约42%的Meta Quest商店应用采集运动传感器数据且无权限请求），此前研究仅揭示可观测行为（如用户在虚拟场景中看到什么）的泄露。论文提出假设：头显内置运动传感器可捕捉瞳孔反应引发的细微面部振动，其与大脑EEG信号高度相关，从而越出"可观测"范畴，重建"不可观测"的脑电关联表示。

**方法**：实现BRAVESPY攻击系统——先对加速度数据进行坐标校准、身体运动抑制与眨眼检测，用STFT提取时频特征，再以pix2pix为骨干训练cGAN把加速度频谱图重建为EEG频谱图（逆变换得EEG信号），最后用基于ViT的识别模型推断UI级（网站/应用/视频指纹）与用户级（身份、注视、击键）隐私，并用重建EEG微调DreamDiffusion（Stable Diffusion）解码脑内感知图像。

**结果**：在Quest 2、Quest、PICO 4、HTC Vive Pro四款头显上，脑级感知图像类准确率52.0%-67.2%，首次在元宇宙中揭示不可观测隐私；UI级网站指纹89.3%、应用85.8%、Netflix视频80.0%；用户去匿名化98.4%、注视识别97.6%；消融显示EEG重建在脑级提升26.2%，会话分离协议下性能仅小幅下降，排除仅靠头部运动线索的假说。

**我的思考**：该工作把"瞳孔反应→面部微振动→运动传感器→EEG重建→脑感知解码"的链条打通，首次把VR隐私攻击从可观测行为推进到脑级感知，选题新颖、方法完整，跨设备鲁棒性实验较扎实。局限是脑级感知仅按类别（40类ImageNet）判定而非像素级重建，Cat类仅54%；需25人配对EEG数据训练，跨设备迁移（PICO 4/HTC Vive降至52%）明显；实际威胁依赖恶意应用长期后台采集，且需结合注视先验才能有效击键推断（top-1仅10.4%）。

- **arXiv PDF**: [When VR Meets BCI: (Un)Observable Brainwave-Aware Privacy Reconstruction in the Metaverse via Unrestricted Inbuilt Motion Sensors.](https://arxiv.org/pdf/2606.10502)

#### Secret State Leakage Attacks and Their Impacts on EMV Contactless Payment Apps.
- **作者**: Jesse Chen, Rubin Yuchan Yang, Ahmad Musa, Syed Rafiul Hussain, Omar Chowdhury, Sazzadur Rahaman
- **方向**: 移动与物联网安全
- **解读**: 

**问题**：EMV 非接触式移动支付应用（ECM App）承载真实资金与敏感协议状态，但其秘密协议状态的保护此前未被系统研究。本文提出 Secret State Leakage（SecStLeak）攻击：将应用合法用户视为本地特权敌手（可 root、可逆向、可修改应用），从 ECM App 内部提取秘密 EMV 协议状态字段，进而实施双花等破坏安全保证的攻击，威胁支付安全与用户隐私。

**方法**：将 SecStLeak 形式化为"最小满足割集（minimal-satisfying cut-set）识别"问题，提出元层协议分析方法——先确定"哪些秘密状态字段集合必须被保护"才能阻断各类攻击场景，再据此对真实 Android ECM App 做逆向分析，检查这些字段是否可被提取，并在受控测试环境中验证端到端攻击。

**结果**：识别出 ECM App 必须保护的 4 个最小秘密状态集；分析 136 个 Android ECM App，在 24 个 App 中成功识别出其中 2 个集合的全部秘密字段，另有 3 个 App 可泄露第 3 个集合；在受控环境中对真实 ECM App 演示了破坏核心保证的端到端攻击。24 个受影响 App 合计约 8200 万下载量，多个来自发展中国家或没有 Google Wallet 的地区；多数 App 依赖软件级防御，既次优又可被绕过，与 EMV 标准指引相悖。

**我的思考**：该工作首次把"应用侧秘密状态保护"纳入 EMV 安全分析，割集形式化使"必须保护什么"有了清晰答案，136 个 App 的实证规模也扎实。局限在于特权本地敌手模型较强（需 root），但正符合现实中的设备失控与欺诈场景；对防御的启示明确——应以硬件（安全元素/TEE）承载关键状态而非可逆的软件混淆。与既有 EMV 中继、双花类攻击互补，为支付 App 安全评估提供了新维度。


#### Navigating Developers' Quagmire: LLM-Enabled Privacy Compliance Analysis for SDK Integrations.
- **作者**: Zhaojie Hu, Xueqiang Wang
- **方向**: 移动与物联网安全
- **解读**: 

**问题**：第三方 SDK 是移动应用隐私不合规的主要来源，而开发者在集成 SDK 时面临复杂的隐私合规"泥潭"：难以判断 SDK 的 API 调用在具体集成语境下是否合规。既有隐私不合规集成（PIN）检测依赖预定义的、外部可观察的模式，通用性有限，也无法为开发者提供可操作的指导。

**方法**：提出隐私-语境一致性分析（privacy-contextual consistency analysis）新范式：为"API 的隐私含义"与"集成上下文（隐私语境）"分别建模，并定义通用一致性规则；实现自动框架 PINFINDER，将应用分析（提取 SDK API 调用与隐私语境）与大语言模型推理结合，判断二者是否不一致，并加入提升 LLM 分析可行性的增强手段。

**结果**：在 4,683 个真实 Android 应用上评估，验证了方法的有效性并覆盖更广的 PIN 类型：揭示了 PIN 的普遍性，发现了此前未知的新 PIN 类型，并定位了根本原因——SDK API 设计缺乏标准化、隐私含义不透明，为 SDK 治理与设计改进提供了依据。

**我的思考**：从"匹配已知模式"转向"检查语境一致性"是检测范式的升级，天然具备泛化到未知 PIN 类型的能力；LLM 承担语义推理、规则保证可解释性，分工合理。局限在于依赖 API 建模与隐私语境提取的准确率，LLM 误判可能带来误报/漏报，且静态分析难以覆盖全部动态行为。对 SDK 生态治理（标准化、透明化）的启示具有政策价值，是移动隐私合规自动化的重要进展。


#### Bridge: High-Order Taint Vulnerabilities Detection in Linux-Based IoT Firmware.
- **作者**: Jiaqian Peng, Puzhuo Liu, Yicheng Zeng, Kai Cheng, Yongji Liu, Yun Yang, Hongsong Zhu
- **方向**: 移动与物联网安全
- **解读**: Linux 内核的物联网固件数量庞大且普遍缺乏安全加固，污点分析是发现其中漏洞的主要手段；但传统污点分析多只追踪单跳数据流，而真实漏洞常是"高阶污点"——敏感数据经文件、数据库、配置项等中间存储落地后再次进入危险函数，跨过程、跨状态的传播使其极易被漏报。（无摘要，据标题推断）

据标题推断，Bridge 面向 IoT 固件构建高阶污点检测框架：识别固件中可持久化的中间介质（文件读写、数据库、配置文件）作为"桥"，把污点跨介质的写入-读取对衔接起来，从而把二阶乃至多阶传播路径补全为完整污点流，配合可达性分析降低误报。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在真实固件语料上报告检测到的漏洞数量、与既有污点工具的对比及误报率。（无摘要，据标题推断）

我的思考："高阶/二阶污点"是静态分析长期忽视却真实高频的漏洞形态（如存储型注入类问题），把焦点放在固件场景切中现实；以"桥"统一刻画中间介质传播是清晰抽象。局限：无摘要，介质建模的完备性（何种写入-读取可视为同一桥）与固件语料规模需正文确认，跨介质别名分析的精确度决定误报水平。（无摘要，据标题推断）


#### BACHunter: Detecting Broken Access Control Vulnerabilities in Intelligent Connected Vehicles.
- **作者**: Yanbang Sun, Xiaohong Li, Quanzhou Wang, Hebo Leng, Guangzheng Yao, Zhihua Xie, Qiang Hu, Junjie Wang
- **方向**: 移动与物联网安全
- **解读**: 智能网联汽车（ICV）把车端、手机 App 与云端平台互联，越权访问（BAC）漏洞——如普通用户调用他人车辆的远程控制接口——可直接转化为人身安全威胁；车云链路接口众多、权限模型复杂，自动化检测越权是车联网安全的迫切需求。（无摘要，据标题推断）

据标题推断，BACHunter 面向 ICV 的 App-云-车链路检测越权漏洞：通过逆向 App 与云端 API 的交互，建模对象与操作权限矩阵，比较不同身份/角色下的实际访问结果与应有授权，从而发现水平/垂直越权；可能结合动态执行与语义分析提高准确率。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在真实 ICV 平台或 App 语料上报告发现的越权漏洞数量、类型与误报情况。（无摘要，据标题推断）

我的思考：车联网越权直接关联人身安全与法律风险，选题价值高；把 Web 越权检测经验迁移到"车-云"异构链路是合理路径，难点在于身份建模与多端状态同步。局限：无摘要，车辆/App 样本来源与检测的端到端验证（能否真控制目标车辆）需正文说明，后者直接决定威胁等级。（无摘要，据标题推断）


#### TÄMU: Emulating Trusted Applications at the (GlobalPlatform)-API Layer.
- **作者**: Philipp Mao, Li Shi, Marcel Busch, Mathias Payer
- **方向**: 移动与物联网安全
- **解读**: 

**问题**：移动设备 TEE 中的可信应用（TA）承载指纹、支付、DRM 等关键功能，漏洞可致整机沦陷，但 TEE 闭源且碎片化（各家 TOS 与系统调用截然不同），设备上只能运行签名代码，动态分析几乎不可能。既有方案（如三星 PartEmu 全系统模拟）未开源难复现，或以临时方式逐 TEE 适配。

**方法**：TÄMU 在 API 层拦截实现 TA 重托管：TA 在仿真核心中运行，调用 GP/libc/TEE 私有 API 时被钩子重定向到虚拟 TEE 实现，一套框架横跨多 TEE。关键是"贪婪高层仿真"：用静态 ICFG 计算实现某组 API 后可到达的基本块数，按覆盖增益贪心排序，指导私有 API 的逆向投入；并集成 ASAN、AFL++ 模糊测试、gdb 调试与共享内存模型（复现 TOCTTOU 竞态）。

**结果**：API 研究覆盖 7 个 TEE 的 337 个 TA，GP 采纳率从 2015 年 19% 升至 2025 年超 55%，但 94% 的 TA 仍使用至少一个私有 API（共 389 个）；仅 GP+libc 可到达 39% 代码（TEEGris 仅 9%、MiTEE 达 98%），每 TEE 再实现 1–8 个私有 API 即覆盖 90%，70% 高影响私有 API 可由 GP/libc 等价替代。TÄMU 运行 4 个 TEE 的 67 个 TA，复现 6 个 n-day；对 30 个 TA 模糊测试发现 17 个 0day（11 个真机复现），崩溃中 72% 为真实缺陷，速度约 100 次/秒，比 PartEmu 快 5–10 倍。

**我的思考**：将 HAL 仿真困境与 TEE 标准化趋势结合，用数据论证 GP 统一 API 的规模经济，greedy HLE 把人工逆向转为数据驱动决策，且全开源、漏洞已负责任披露。局限：仅覆盖单输入缓冲区的 30/67 个 TA，无状态模糊未应对多命令与状态化 TA；5 个漏洞因测试机不支持无法真机验证；私有 API 仍需大量人工逆向。相比未开源的 PartEmu，TÄMU 提供了可扩展的公共基础设施。

- **arXiv PDF**: [TÄMU: Emulating Trusted Applications at the (GlobalPlatform)-API Layer.](https://arxiv.org/pdf/2601.20507)

### 云与供应链安全（5 篇）

#### Responsible Disclosure is a Two-Way Street: Empirically Measuring the Responsible Disclosure Contract in the Firmware Ecosystem.
- **作者**: Hui Jun Tay, Souradip Nath, Arvind S. Raj, Abhay Bhat, Ishan Bansal, Audrey Dutcher, Moritz Schloegel, Adam Doupé, Tiffany Bao, Yan Shoshitaishvili, Ruoyu Wang
- **方向**: 云与供应链安全
- **解读**: 负责任披露（responsible disclosure）是安全研究生态的基石契约：研究者报告漏洞，厂商负责修复并给予回应。但固件生态中这一"双向契约"是否被切实履行——厂商响应速度如何、修复是否发布、披露协议是否被遵守、研究者承担何种代价——缺乏系统实证，尤其固件厂商生命周期短、补丁碎片化，问题更为突出。论文以固件生态为对象，实证测量这一契约。

据标题推断，作者大规模收集固件漏洞披露数据（协调披露记录、厂商公告、CVE 时间线等），并结合对研究者的调查或访谈，量化披露周期、厂商响应率、修复发布情况与研究者侧成本，描绘契约失衡的具体环节。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；预期以披露到修复全周期的统计数据与案例，揭示披露契约中权利义务失衡的具体表现。

披露契约的失衡——厂商不响应、不修复却要求保密——会直接抑制研究者的报告意愿，是供应链安全的隐性放大器；来自 SEFCOM 团队的实证数据可信度较高。局限：样本偏公开记录，私下协调的披露难以覆盖；"双向"视角（研究者成本与厂商义务并重）的量化方法是本工作的看点，对政策制定与厂商流程改进有直接参考价值。


#### Rain: Transiently Leaking Data from Public Clouds Using Old Vulnerabilities.
- **作者**: Mathé Hertogh, Dave Quakkelaar, Thijs Raymakers, Mahesh Hari Sarma, Marius Muench, Herbert Bos, Erik van der Kouwe
- **方向**: 云与供应链安全
- **解读**: 
公共云的多租户架构将不同用户的虚拟机置于共享基础设施上，理论上隔离机制应阻止跨租户数据访问；但历史漏洞（尤其是已修复的旧漏洞）若能在云环境中以"瞬时"方式被重新激活或利用，可能造成短暂但严重的数据泄露。系统化评估旧漏洞如何造成云上瞬时泄露，有助于理解云安全的残余风险。

据标题推断，Rain研究如何利用旧漏洞在公有云中瞬时泄露数据：可能通过触发已修复漏洞的旧版本组件、利用内核或固件遗留缺陷，或在补丁部署前的竞争窗口内完成攻击，从而在受限时间窗口内读取其他租户的敏感数据，并评估其现实可行性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应包含针对主流公有云的实证测试与泄露数据规模的评估，具体数字以全文为准。

该工作的价值在于把"漏洞年龄"与"云租户隔离"联系起来：企业普遍假设"打了补丁就安全"，Rain揭示补丁覆盖的盲区与瞬时攻击窗口，提醒云厂商与租户重新评估残余风险。作者团队（VUSec）在底层系统安全方向声誉卓著。局限在于瞬时泄露的利用窗口窄、可重复性存疑，实际危害取决于泄露内容的敏感度；该研究对云安全审计与补丁管理策略具有警示意义。


#### C-Verifier: Understanding and Formally Verifying Cross-Service Flaws in AWS Cognito.
- **作者**: Zhen Chen, Ze Jin, Le Gong, Kexin Chen, Xiangyi Zeng, Qixu Liu
- **方向**: 云与供应链安全
- **解读**: 
现代云应用普遍采用身份与访问管理服务（如AWS Cognito）组合多个云服务完成认证与授权，服务间的交互流程常常引入单点服务审计无法发现的逻辑缺陷，导致越权、身份混淆等严重漏洞；云身份服务的事实标准地位使这类跨服务缺陷的影响面巨大，理解并验证其缺陷具有现实紧迫性。

据标题推断，C-Verifier系统性地理解并形式化验证AWS Cognito相关的跨服务缺陷：先对Cognito与周边服务（如API Gateway、Lambda、IAM）的交互流程进行建模，再以形式化方法检验其认证与授权语义是否可被绕过，从而自动发现跨服务逻辑漏洞并给出可复现的验证结果。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告发现的跨服务缺陷类型、数量与真实影响（如是否可导致越权），具体数据以全文为准。

云身份服务的安全分析是近年热点（此前已有针对OIDC、Cognito配置的研究），C-Verifier的贡献在于把"跨服务"作为分析单元，弥补单服务审计的盲区，方法论上具备迁移到其他云服务商的潜力。局限在于对AWS生态建模的完备性决定结论强度，且云服务API频繁演进，模型需持续更新；对云厂商与安全审计工具而言，此类工作具有直接的实用价值。


#### COSSETER: GitHub Actions Permission Reduction Using Demand-Driven Static Analysis.
- **作者**: Greg Tystahl, Jonah Ghebremichael, Siddharth Muralee, Sourag Cherupattamoolayil, Antonio Bianchi, Aravind Machiry, Alexandros Kapravelos, William Enck
- **方向**: 云与供应链安全
- **解读**: GitHub Actions 已成为软件供应链攻击的高发面，其工作流常被赋予远超实际需要的权限（如 GITHUB_TOKEN 的仓库读写、action 的可信输入），一旦被恶意 PR 或第三方 action 利用，会造成严重供应链危害。最小权限是核心缓解手段，但手工审计大规模工作流不现实。（无摘要，据标题推断）

据标题推断，COSSETER 采用需求驱动（demand-driven）的静态分析：从工作流中实际执行的命令与 API 调用出发，反向推导其真正需要的数据与权限，自动识别可安全移除的多余权限，生成最小权限建议并可能自动改写工作流。

摘要缺失，无法引用量化结果；预期贡献包括对真实 GitHub 仓库工作流权限的测量（多少权限可被削减）以及工具在规模与准确率上的评估。

我的思考：供应链安全社区已多次指出 Actions 过度授权问题，自动化的权限收窄工具直接回应这一痛点，需求驱动分析比传统保守分析更能给出可落地的精简建议。同类工作多停留在规则或 lint 层面，COSSETER 的静态分析路线若精确度高，可作为 CI 加固工具链的一环；无摘要使误报率与可扩展性等关键指标待正文确认。


#### Death Is Not the End: a Longitudinal Study on the Impact of Automatic Updates on Container Vulnerability Lifespans.

- **作者**: Simge Tekin, Octavian Suciu, Sungsu Kwag, Yonghwi Kwon, Tudor Dumitras

- **方向**: 云与供应链安全

- **解读**: 容器镜像的漏洞生命周期不同于传统软件：即使上游已修复（"死亡"），已构建或运行中的镜像仍长期携带漏洞；自动更新（如基础镜像自动重建）能否真正缩短漏洞寿命，缺乏长期实证。

据标题推断，作者开展纵向研究：长期跟踪大量容器镜像与自动更新机制，对比有无自动更新时漏洞的引入、修复与存续时间，量化自动更新对漏洞寿命的影响。

无摘要，无法引用具体量化结果；核心是揭示容器漏洞"修复后仍存活"的现象及其与自动更新的关系。

我的思考：容器供应链安全的关键洞见是"镜像不可变 + 未重建 = 漏洞永续"，自动更新看似缓解，实际可能因重建滞后或依赖锁定而失效；对镜像扫描策略与供应链治理（基础镜像固定、重建策略）有直接指导；局限在于样本代表性（镜像仓库分布）与观测周期。（无摘要，据标题推断）


### 认证与访问控制（8 篇）

#### A Full Threshold NIST PQC-Compliant Framework for Distributed Trust in Federal Public Key Infrastructure.
- **作者**: Kiarash Sedghighadikolaei, Changqi Sun, Thang Hoang, Bechir Hamdaoui, Attila A. Yavuz
- **方向**: 认证与访问控制
- **解读**: 

**问题**：美国联邦 PKI（FPKI）依赖集中式 CA 签名，单一密钥被攻破即威胁整个信任根；同时量子威胁使经典签名面临淘汰，而既有 ML-DSA 门限方案大多不满足完整的 NIST/FIPS-204 合规要求，阻碍联邦机构采用。

**方法**：提出 SHIELD——全门限、NIST PQC 合规的签名框架：在算法层面完全满足 FIPS-204，且无需改动签名算法即可与标准 ML-DSA 互操作；采用安全多方计算（MPC）而非全同态加密实现分布式签名，在计算与通信成本间取得平衡；提供开源实现。

**结果**：给出首个开源 FIPS-204 合规门限签名实现；在不同网络延迟条件下完成实证评估，结果表明其满足 FPKI 部署的实际性能要求；以分布式签名替代集中式证书签发，缓解单密钥沦陷风险并具备量子抗性。

**我的思考**：以"合规优先"切入联邦 PKI 迁移是务实之举——不改变签名算法的互操作设计极大降低落地阻力；全门限（而非 t-out-of-n 的部分容错）与 FPKI 结构要求的对齐值得注意。局限在于 MPC 门限签名的通信/延迟开销与密钥管理复杂度仍需实际运营检验。该工作是后量子 PKI 治理的示范性工程，对政府与企业 CA 迁移均有参考价值。


#### RIS-CLA: Reviving CSI-Based Continuous Location Authentication With Reconfigurable Intelligent Surfaces.
- **作者**: Yan Zhang, Jiawei Li, Yizhou Wang, Dianqi Han, Yanchao Zhang, Aditya Shekhawat, Georgios Trichopoulos
- **方向**: 认证与访问控制
- **解读**: 基于信道状态信息（CSI）的连续位置认证（CLA）利用无线信道特征确认用户在合法位置的持续在场，但环境动态与用户移动常使 CSI 特征漂移、认证性能衰减，导致该路线此前实用性受限。（无摘要，据标题推断）

据标题推断，RIS-CLA 用可重构智能超表面（RIS）"复活"CSI 连续位置认证：RIS 可主动调制无线信道、增强位置相关特征的可区分性，使 CSI 指纹对位置更敏感、对干扰更鲁棒，从而提升连续认证的准确率与稳定性。

摘要缺失，无法引用量化结果；预期包含在真实室内环境对认证准确率、误报/漏报与抗攻击（重放、模拟攻击）能力的评估。

我的思考：把 RIS 的"信道塑形"能力用于认证，是 RIS 安全应用的正面例子——主动制造位置可分性，而非被动接受信道随机性。Yanchao Zhang 团队在无线安全（尤其位置认证）上积累深厚，方法可信。核心问题是 RIS 部署成本与覆盖范围能否支撑现实应用，以及攻击者同样部署 RIS 时的对抗情形；无摘要使实验规模与指标待确认。


#### MAYA: Addressing Inconsistencies in Generative Password Guessing Through a Unified Benchmark.
- **作者**: William Corrias, Fabio De Gaspari, Dorjan Hitaj, Luigi V. Mancini
- **方向**: 认证与访问控制
- **解读**: 生成模型被越来越多地用于密码猜测（trawling 攻击），但此前研究评估方法不一致，导致不同工作难以横向比较、社区缺乏对生成式猜测能力的公正理解。统一的基准框架是填补这一缺口的必要基础设施。

MAYA 是一个统一、可定制、即插即用的基准框架：将六个最新的生成式密码猜测方法重新实现并适配到同一评估协议下，在八个真实世界数据集上覆盖一套全面的高级测试场景，总计算量超过 15,000 机时。

发现：这些方法各自捕获了人类密码分布的不同方面且泛化能力强，但在长密码、复杂密码上的有效性差异显著；顺序（sequential）模型一致优于其他架构与传统工具，展现出生成精确猜测的独特能力；此外，各模型学到的分布具有多样性，多模型组合攻击优于最佳单模型。框架已开源（github.com/williamcorrias/MAYA-Password-Benchmarking）。

我的思考：该工作解决的是密码猜测研究长期被忽视的"比较公平性"问题——方法论不统一会让结论失真，15,000 机时的系统评估为社区提供了可信基线。核心发现（顺序模型占优、多模型集成更强）对攻击者与防御者都有指导意义。局限在于基准仍以猜测率为主视角，且重新实现的六个模型与原始版本的一致性需谨慎对待；作为基础设施型贡献，其长期价值在于被社区采用。


#### Credential Extraction Attacks Against Compromised Credential Checking Services of Password Managers.
- **作者**: Yihe Duan, Ding Wang, Yutong Li
- **方向**: 认证与访问控制
- **解读**: 密码管理器通过"泄露凭证检查"服务（如 HIBP 类 API）在本地比对用户口令是否已被泄露，其安全性依赖客户端与服务端的交互协议设计；若协议存在缺陷（查询泄露、响应可枚举、分区攻击），攻击者可能借服务端响应反向提取或关联用户的真实口令。（无摘要，据标题推断）

据标题推断，该研究系统分析主流密码管理器泄露检查服务的协议实现，构造凭证提取攻击：利用协议中的可观察差异（响应时间、k-anonymity 分区、范围查询语义）逐步缩小并恢复用户口令的泄露状态乃至口令本身。

摘要缺失，无法引用量化结果；预期包含受影响的服务与客户端、攻击成功率与提取成本、以及修复建议。

我的思考：Ding Wang 团队是口令安全领域的权威，此类"对防御机制本身的攻击"研究价值在于揭示安全服务的实现细节如何破坏其设计假设——泄露检查本为保护用户，协议缺陷却可能反向泄露口令信息。与既有 HIBP 隐私研究相比，其增量应在攻击的深度（提取真实凭证）与覆盖的服务范围；无摘要使具体攻击路径与可行性待正文确认。


#### Can Foundation LLMs Accurately Estimate Password Strength and Provide Appropriate Password Feedback?
- **作者**: Madison Pickering, Garrison Hinson-Hasty, Luca Dovichi, Helena Williams, Nathaniel Kim, Aybala Esmer, Blase Ur
- **方向**: 认证与访问控制
- **解读**: 密码强度估计与反馈是密码设置体验的核心环节，传统强度计（如 zxcvbn）基于规则与统计，而基础 LLM 能否准确评估口令强度并给出恰当、可执行的改进建议，是"AI 时代的密码评估"这一开放问题。（无摘要，据标题推断）

据标题推断，研究以用户研究或系统评估方式考察多种基础 LLM 在密码强度评分、猜测抵抗评估与反馈生成上的表现，并与 zxcvbn 等基线对比，分析其在强弱口令判别、反馈合理性（如拒绝误导性建议）与安全-可用性平衡上的能力。

摘要缺失，无法引用量化结果；预期包含 LLM 与基线的一致性、准确率及反馈质量的编码评估。

我的思考：Blase Ur 团队深耕密码可用性与强度评估，其问题设定直击现实：若用户把 LLM 当作密码顾问，其建议的安全性与可用性就具有实际后果。风险在于 LLM 可能被表面复杂度或熵模型误导而高估/低估口令强度，或给出降低安全性的建议；与 zxcvbn 类确定性工具的互补关系是核心看点。无摘要使模型集合、评估口径与结论待正文确认。


#### MoPE: A Mixture of Password Experts for Improving Password Guessing.
- **作者**: Mingjian Duan, Ming Xu, Shenghao Zhang, Weili Han
- **方向**: 认证与访问控制
- **解读**: 文本口令仍是 Web 主要认证手段，数据驱动的口令强度评估模型通常把口令"一视同仁"，忽视结构差异，导致训练偏向高频口令模式。作者主张口令作为短文本应被结构化处理：先识别模式，再路由到专门模型。

MoPE（Mixture of Password Experts）的关键观察是结构相似的口令（如定长数字串）在潜空间中聚成高密度区域。据此引入：① 基于结构生成专家模型的构造方法；② 轻量级门控（gate）选择合适专家输出可靠猜测，契合高计算频率的猜测任务。

评估显示 MoPE 在离线和在线场景均显著优于最先进基线，破解率最高提升分别达 38.80% 与 9.27%；并实现基于 MoPE 的实时口令强度计（PSM），以毫秒级响应辅助用户选择更强口令。

我的思考："按结构分诊"是口令建模中朴素却有效的洞见——把同质口令交给专门模型，能减少模式间干扰、提升猜测精度。与通用生成模型相比，MoPE 的专家路由在可解释性与效率上有优势；38.80% 的离线增益说明结构先验的实战价值。局限在于专家划分粒度与门控的泛化性（新结构口令的路由质量），以及在线场景增益较小（9.27%）的现实意义；作为口令猜测工具箱的补充，其与 MAYA 类统一基准的兼容值得关注。


#### 2FiA: Towards WiFi Sensing-Based Authentication with Unique Biometrics.
- **作者**: Bofan Li, Zhankai Ye, Weikuan Yu, Yongning Tang, Liu Xiu
- **方向**: 认证与访问控制
- **解读**: 传统生物认证（指纹、人脸）依赖专用传感器且存在隐私与伪造风险；WiFi 感知（CSI 信号）能以无设备方式捕捉人体运动与生理特征，为"随处可用的生物认证"提供可能，但其个体唯一性（区分不同用户）此前缺乏严格验证。（无摘要，据标题推断）

据标题推断，2FiA 提出基于 WiFi 感知的唯一性生物认证：从 CSI 中提取与个体强相关的生物特征（如步态、体态、运动模式），设计特征选择与分类或度量学习框架，使不同用户间特征可分、同一用户跨时间稳定，并结合防重放与持续认证机制提升安全性。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告认证准确率、误接受率与误拒绝率及在多人、多场景下的区分度。（无摘要，据标题推断）

我的思考："WiFi 即传感器"让认证无需专用硬件，也规避了生物特征采集的敏感性问题，设想吸引人；核心难点是 CSI 特征的环境敏感性与个体唯一性的严格验证（能否支撑大规模用户区分）。局限：无摘要，实验规模（人数、环境数、时间跨度）与对环境扰动的鲁棒性需正文确认，对模仿/重放攻击的安全分析深度决定其可信度。（无摘要，据标题推断）

#### Demystifying the (In)Security of Oauth-Based Account Linking in Connector Ecosystems.

- **作者**: Kaixuan Luo, Xianbo Wang, Adonis P. H. Fung, Wing Cheong Lau
- **方向**: 认证与访问控制
- **解读**:
连接器生态（connector ecosystems，如各类自动化/集成平台把多个第三方服务绑定起来）普遍使用 OAuth 完成"账户关联"（account linking），把用户在不同服务的身份绑定为一条可跨服务调用的链路。账户关联组合了多个 OAuth 流程环节（授权回调、状态校验、令牌绑定），任一环节的设计缺陷（如令牌混淆、状态参数缺失、回调校验不严）都可能被攻击者利用，实现账户劫持与跨服务权限提升，其安全性长期缺乏系统梳理。（无摘要，据标题推断）

据标题推断，论文旨在"去神秘化"连接器生态中 OAuth 账户关联的安全状况：梳理主流连接器平台与 OAuth 库的关联实现，建立威胁模型与漏洞分类（如重定向 URI 混淆、CSRF、令牌泄露/重放），并通过实证测量或自动化检测识别不安全实现及其可利用路径。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出不安全关联模式的分类体系、真实生态中的流行度测量以及代表性漏洞的端到端可利用性演示。（无摘要，据标题推断）

我的思考：账户关联是 OAuth 生态中被忽视的"组合环节"，其安全属性无法由单个授权流程保证，跨服务身份绑定还会放大单点沦陷的影响，系统化的实证研究对平台与开发者都有直接指导价值。局限在于调研覆盖面（平台与库的样本数量）、测量方法的主动/被动边界，以及结论随时间演化的时效性；与 RFC 6749/8252 及重定向混淆、CSRF 等经典 OAuth 漏洞的关联是评判其贡献的重要参照。（无摘要，据标题推断）


### 数字取证（3 篇）

#### Recovering and Rehosting Mobile Local LLM Conversations and Contexts via Memory Forensics.

- **作者**: Haichuan Xu, David Oygenblik, Runze Zhang, Mingxuan Yao, Muhammad Ibrahim, Brendan Saltaformaggio

- **方向**: 数字取证

- **解读**: 移动端本地大模型（local LLM）应用把对话与上下文状态保存在设备内存中，涉及高度敏感的个人信息；即便应用声称清除数据，内存镜像中仍可能残留完整对话记录。传统内存取证工具面向普通应用的数据结构，无法理解 LLM 运行时特有的布局（分词器状态、会话上下文、KV 缓存等），数字取证需要新的方法论。

作者（论文项目名 ORISA）逆向分析主流移动端本地 LLM 应用的运行时内存布局，定位对话历史、系统提示与上下文对象在堆中的结构；设计内存结构指纹与语义恢复技术，从原始内存转储中识别并重建对话记录，再将恢复的上下文重放（rehost）到独立环境，以验证恢复内容的完整性与可用性。

实验表明即使应用尝试清除或混淆数据，内存取证仍可完整重建对话并重放，重放后模型可基于恢复的上下文继续生成连贯回复；研究证明移动端本地 LLM 存在系统性的敏感数据留存攻击面，为取证调查与泄露事件响应提供可行路径（该工作发表于 S&P 2026）。

我的思考：这是把内存取证方法论拓展到端侧大模型这一新兴场景的开创性工作，开辟"AI 时代的数字取证"方向，对执法调查与数据泄露响应有直接价值。局限在于恢复依赖特定运行时布局，模型与框架版本更迭可能使指纹失效；重放保真度与取证对象的隐私伦理边界也值得讨论。与针对传统 App 的取证研究相比，本文首次系统处理 LLM 特有的对话与上下文状态，填补了空白。


#### Sealing the Window: Efficient Tamper Protection for Provenance Logs.

- **作者**: Sagar Mishra, R. Sekar
- **方向**: 数字取证
- **解读**:
溯源日志（provenance logs）记录系统实体与事件的因果关系，是入侵检测与数字取证的关键证据；但日志在被攻陷后的“窗口期”内可能被攻击者篡改，破坏证据完整性，削弱整个溯源系统的可信度。

据标题推断，论文提出高效的防篡改机制：缩小并封堵溯源日志被篡改的时间窗口，以较低开销持续为日志条目提供完整性绑定（如链式哈希、聚合签名等），使攻击者无法在事后无痕修改历史记录。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；设计目标是在可接受的性能开销下实现接近实时的完整性保护。

“篡改窗口”确实是溯源系统信任链的软肋，该工作把完整性保护从“事后审计”推进到“持续密封”，方向务实；与 W3C PROV、透明日志、BAF 等既有方案的关系是评估贡献的关键。局限在于密封服务/密钥管理的信任假设，以及跨主机溯源场景下的开销放大。，为取证证据链的完整性提供工程化保障。


#### APT to Disagree: A Comparative Analysis of Attribution in Commercial TI.

- **作者**: Aksel Ethembabaoglu, Rolf van Wegberg, Yury Zhauniarovich, Michel van Eeten
- **方向**: 数字取证
- **解读**:
商业威胁情报（TI）厂商发布的 APT 归因（attribution）——把攻击事件归属到特定组织甚至国家——直接塑造企业安全运营的优先级与响应决策，但归因本质上是基于有限证据的高不确定性推断，不同厂商在方法论、命名体系与证据标准上的差异可能导致对同一事件给出相互矛盾或重叠的归因，误导防御资源配置。这一问题此前缺乏跨厂商的系统比较。（无摘要，据标题推断）

据标题推断，论文对多家商业 TI 厂商的 APT 归因数据进行系统收集与对齐，把来自不同厂商的报告映射到同一批攻击事件/活动簇，量化归因的一致性与分歧程度，分析分歧来源（归因粒度、命名规范、时间跨度、证据链标准），并可能考察归因与厂商商业因素的关系。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；据标题推断，论文给出跨厂商归因一致性的量化结果、分歧典型案例与驱动分歧的因素分析。（无摘要，据标题推断）

我的思考：归因是威胁情报中最受关注也最不透明的环节，实证比较能让消费者理解 TI 数据的可信边界、推动归因方法的透明化，对安全运营决策有直接实用价值。方法论上存在固有边界：由于缺乏"归因真相"地面真值，研究只能比较一致性而非正确性，样本可得性（商业数据授权）也限制覆盖，这些需全文说明；与学术归因方法（如基于基础设施指纹的技术归因）的对比可作为延伸视角。（无摘要，据标题推断）

### 无线与通信安全（5 篇）

#### One Tap to Hijack Them All: A Security Analysis of the Google Fast Pair Protocol.
- **作者**: Sayon Duttagupta, Seppe Wyns, Nikola Antonijevic, Dave Singelée, Bart Preneel
- **方向**: 无线与通信安全
- **解读**: Google Fast Pair 服务（GFPS）以"一键配对"与账户同步扩展蓝牙配对，覆盖数亿用户，但此前从未有过系统性安全分析。论文给出 GFPS 的首次全面安全分析，揭示协议设计层面的系统性缺陷及其跨服务级联放大效应。

作者检查 16 家厂商、17 种芯片组的 25 款商业配件，发现规范核心要求在实现、验证与认证环节存在系统性执行失败；进一步展示已识别缺陷可在协议内部级联，GFPS 与 Find Hub 网络看似独立却互相造成严重后果。据此提出 WhisperPair 攻击族：未授权配对、静默劫持音频设备、把受害者配件秘密绑定到攻击者账户，从而经 Find Hub 实现持续位置跟踪与骚扰。

实验表明这些漏洞并非孤立事件，而是实现、验证、认证链条上的生态级缺口；根因是 GFPS 依赖可失败的"应用层状态检查"而非密码学强制，使缺陷得以传播到终端用户。修复方案 IntentPair 以轻量修改把用户意图密码学地绑定进密钥调度，从设计上消除漏洞，影响面覆盖数亿用户。

首个对 GFPS 的完整安全分析，攻击族设计系统、根因分析到位（密码学强制对应用层状态），并给出可落地修复，是典型的"测量+攻击+修复"闭环。局限：25 款配件相对生态仍是抽样，WhisperPair 对未测试芯片组的普遍性待验证；"秘密绑定+位置跟踪"的组合展示了跨服务级联的严重性，提示厂商应将配对意图纳入密码学协议而非信任应用层状态。


#### Nonlocalizable Jamming with Curving Beams.

- **作者**: Caroline Jane Spindel, Edward W. Knightly

- **方向**: 无线与通信安全

- **解读**: 毫米波通信依赖波束成形与到达方向（DoA）估计来定位并置零干扰源。若攻击者使用可弯曲波束（curving beams）——波束沿弯曲路径传播、到达角与真实发射位置解耦——干扰源将无法被定位，现有 DoA 估计与置零防御随之失效，威胁毫米波链路的可用性。

作者理论建模弯曲波束的传播与到达特性，分析其对 DoA 估计和接收波束成形的欺骗效果；在毫米波测试平台上进行实验验证，考察弯曲波束对直接相位测量与接收端波束成形的实际干扰能力，并演示其伪造源位置、把干扰送入受害者主瓣（main lobe）的机制。

实验结果显示，弯曲波束干扰使误码率（BER）提升约 1000 倍（~1000x），同时影响直接相位测量与接收波束成形，证实了"不可定位干扰"（nonlocalizable jamming）在毫米波频段的可行性与威胁程度。

我的思考：该工作把光学/声学领域的弯曲波束概念引入无线安全，开辟了"干扰源不可定位"这一新的对抗维度，对毫米波与太赫兹网络的抗干扰设计构成新挑战。局限在于实验场景规模有限，真实多径环境下的可靠性、以及对抗方重新定位干扰源的反制手段仍需研究；与压制式、欺骗式干扰的对比度量也待补充。对 6G 频谱共享与定向通信的防御设计具有启示意义。


#### Guardians of the Air: In-Device Detection of 5G Control-Plane Threats.
- **作者**: Tianwei Wu, Abdullah Al Ishtiaq, Tianchang Yang, Yilu Dong, Kai Tu, Zeyu Song, Ridwanul Hasan Tanvir, Md. Toufikuzzaman, Shagufta Mehnaz, Syed Rafiul Hussain
- **方向**: 无线与通信安全
- **解读**: 5G 控制面（信令面）是网络信任的根基，却面临伪基站（false base station）、IMSI 捕获、信令风暴与协议滥用等威胁，且许多攻击在设备端即可观测。现有防护多依赖网络侧或专用硬件，缺乏在用户设备内实时、轻量检测控制面威胁的方案。（无摘要，据标题推断）

据标题推断，Guardians of the Air 在设备端实现控制面威胁检测：通过监测 UE 与网络交互的信令消息、定时关系与状态转换，提取攻击特征，用规则引擎或机器学习分类器在手机内实时识别伪基站与异常信令行为。

摘要缺失，无法引用量化结果；预期包含在真实或仿真 5G 测试平台上对各类控制面攻击的检测率、误报率与设备端开销评估。

我的思考：控制面攻击（尤其伪基站）长期依赖昂贵的外置检测设备，设备端检测能把防护下沉到每一部手机，覆盖面与可部署性显著提升。Hussain 团队在 5G 协议安全（fuzzing、漏洞挖掘）上积累深厚，方法可信度高。挑战在于设备端算力/电量约束、厂商基带固件的可编程性，以及合法网络行为多变带来的误报控制；无摘要使检测范围与实测数据待确认。


#### The Battle of Metasurfaces: Understanding Security in Smart Radio Environments.
- **作者**: Paul Staat, Christof Paar, Swarun Kumar
- **方向**: 无线与通信安全
- **解读**: 可重构智能超表面（RIS）能把传统被动的无线电环境变为可编程介质，但其可编程性也开辟了新安全前沿：攻击者与防御者都能利用超表面按自身意图改变电磁波传播。此前研究多为单边应用（只赋能一方），本文首次系统研究攻防双方都具备相当能力的对称场景。

方法结合理论建模与真实实验，分析双方超表面在不同目标（信号功率、感知、观感欺骗）下的竞争交互，并在 Wi-Fi 环境中构造干扰、信道混淆、通信欺骗、伪造（spoofing）等多个案例研究。

结果揭示"超表面之战"的胜负取决于时序、部署位置、算法策略与硬件规模的相互作用：对立的超表面可以大幅甚至完全抵消彼此的效果，从而破坏此前提出的隐私保护方案；同时为在智能环境中设计鲁棒、高保证的物理层系统打开新机会。

我的思考：把 RIS 研究从"单边能力展示"推进到"攻防对称博弈"，是该领域安全视角的重要补全——先前的隐私/安全方案多假设只有防御者使用超表面，本文证明该假设不成立。实验验证"对抗可互相抵消"是强结论，意味着任何依赖 RIS 的安全方案都必须考虑对手拥有同等硬件的情形。局限在于实验环境规模与信道模型的一般性，以及博弈均衡结论向大规模部署的推广。


#### SatBleed: Security of Commoditized Communication Modules in Satellites.
- **作者**: Ulysse Planta, Julian Rederlechner, Martin Strohmeier, Mathias Fischer, Ali Abbasi
- **方向**: 无线与通信安全
- **解读**: 小卫星（CubeSat 等）大量采用商用现货（COTS）通信模块，其固件与协议实现的安全性远低于航天级组件，却承担卫星的遥测、指令与载荷数据传输——攻击者若能攻破这些模块，即可劫持卫星通信或植入恶意载荷。（无摘要，据标题推断）

据标题推断，SatBleed 对这些商用通信模块进行安全分析：通过固件逆向、协议审计与漏洞挖掘，识别可被利用的内存破坏、认证缺陷或后门类问题，并评估其在地面测试环境或仿真中的可利用性。

摘要缺失，无法引用量化结果；预期产出包括受影响模块与芯片、漏洞类型与严重性、以及供应链层面的影响评估。

我的思考："商业化航天"的兴起使卫星不再昂贵且不可触碰，COTS 组件的安全短板成为航天供应链的系统性风险。Abbasi 与 Strohmeier 团队在无线与嵌入式安全上有积累，此类研究对航天安全社区是及时提醒。关键评价点在于漏洞是否可被远程（经无线链路）利用而非仅本地调试接口，以及披露与修复路径；无摘要使这些细节待正文确认。


### 数据安全（1 篇）

#### Can I Get More? An Incremental Inference Attack on Encrypted SQL.
- **作者**: Xiaoqian Sun, Ruiqi He, Yang Zhang, Siyi Lv, Guiyun Qin, Fangzhou Yi, Zheli Liu, Xiaofeng Chen
- **方向**: 数据安全
- **解读**: 

**问题**：加密数据库（encrypted SQL）承诺数据机密性，但查询执行过程不可避免地泄露访问模式、顺序、频率等侧信息；既有推理攻击多为"一次性"利用，而真实数据库工作负载是长期连续查询，攻击者随查询累积逐步增强推理能力的"增量式"威胁此前缺乏系统研究。

**方法**：据标题与会议摘要推断，本文提出增量推理攻击框架：攻击者观察并适配一系列加密 SQL 查询及其结果，将多次查询暴露的侧信息逐步累积、交叉关联，渐进恢复明文属性或缩小候选空间——攻击者的分析随观测到的查询数增长而不断"获得更多"，突破单次攻击的信息上限。（无摘要，据标题推断）

**结果**：摘要缺失，无法给出量化结果；论文由南开大学与西安电子科技大学团队完成，聚焦于揭示加密 SQL 在长期查询负载下的渐进式机密性损失，相关工作（如针对 TEE 加密数据库的 Leafblower 等）表明该威胁面真实存在。

**我的思考**：增量视角贴合真实数据库运维场景，比单轮攻击更贴近实际敌手能力，对加密数据库（含 OPE/DTE 类保序/保属性方案）的安全性论证是重要补充——"每次泄露一点点、累积成完整恢复"正是此类系统最难防御的攻击形态。局限在于无摘要可核对具体量化指标，且增量攻击的有效性高度依赖查询分布与侧信道类型。该方向警示：加密数据库部署者应评估长期运行下的累积泄露，而非仅看单查询安全证明。


### 社会工程与人类因素（14 篇）

#### "I Wonder if These Warnings are Accurate": Security and Privacy Advice in Nine Majority World Countries.

- **作者**: Collins W. Munyendo, Veronica A. Rivera, Jackie Hu, Emmanuel Tweneboah, Amna Shahnawaz, Karen Sowon, Dilara Keküllüoglu, Marcos Silva, Yue Deng, Mercy Omeiza, Gayatri Priyadarsini Kancherla, Marianne Batista Diniz Da Silva, Abhishek Bichhawat, Maryam Mustafa, Francisco J. Marmolejo Cossío, Elissa M. Redmiles, Yixin Zou

- **方向**: 社会工程与人类因素

- **解读**: 安全与隐私建议多由西方语境产出，其在"多数世界"（覆盖非洲、亚洲、拉美与中东的九个国家）的准确性、可获得性与可执行性从未被系统检验；脱离本地现实（经济、语言、社会规范）的建议可能误导资源受限用户，甚至适得其反。

作者在九国开展 70 次半结构化访谈，受访者为当地用户信赖的技术支持角色（网吧经营者、维修技师、社区人物等），考察安全建议的生产、传播与采纳全链路，并分析性别、宗教、集体主义规范等社会文化因素的影响。

本地建议大体与西方一致，但经济约束、语言障碍与社会摩擦显著阻碍执行（如付费工具、隐私功能、安全更新）；女性更倾向向女性求助，共享设备与集体规范影响风险感知与行为；密码管理器等西方推荐工具与本地现实错位，需建议优先化与工具本地化。

我的思考：这是"多数世界安全"研究的重要实证补充，以本土信息提供者为访谈对象的取样策略有效且具说服力，对安全沟通与产品本地化有直接指导意义。局限在于九国样本以城市与技术中介视角为主，农村与低连接人群覆盖有限；"准确性"的判定依赖研究者框架，本地人的风险优先级可能与研究者不同；建议落地效果的纵向测量缺失。与单一文化研究相比，跨九国的比较设计显著增强了结论的稳健性。


#### Privacy Perspectives and Practices of Chinese Smart Home Product Teams.

- **作者**: Shijing He, Yaxiong Lei, Xiao Zhan, Chi Zhang, Juan Ye, Ruba Abu-Salma, Jose Such

- **方向**: 社会工程与人类因素

- **解读**: 智能家居隐私研究多聚焦设备所有者、主要用户与旁观者群体，对产品团队的视角与实践知之甚少，尤其缺乏非西方语境的研究；产品团队的决定塑造了设备的默认隐私行为，其认知与动机是理解智能家居隐私生态的关键但缺失的一环。

作者对 27 名中国智能家居产品团队成员进行半结构化访谈，覆盖产品/项目经理、软硬件工程师、UX 设计师、法律/隐私专家、市场/运营人员；分析其隐私视角、实践与风险缓解策略，并结合中国数据法规与文化语境解读。

受访者普遍强调对数据法规的合规遵从，且往往将国家/集体权益置于个人权利之上；中国特有的文化、社会与法律因素塑造了伦理考量与便利性-隐私权衡态度；作者据此提出面向多用户家庭中弱势群体的社会技术干预与改进建议。

我的思考：本文填补了"产品团队视角 × 非西方语境"的研究空白，揭示了合规中心主义（compliance-centric）如何替代以用户为中心的隐私设计，对监管与产品实践均有启示。局限在于 27 名受访者多来自头部厂商，小团队与外包开发者的实践可能不同；访谈自我陈述与实际产品行为之间存在落差；"国家权益优先"的表述需谨慎解读，避免简单归因于文化本质主义。与欧美产品团队研究形成有价值的对照。


#### Searching for a Farang: Collective Security Among Women in Pattaya, Thailand.

- **作者**: Taylor Robinson, Rikke Bjerg Jensen

- **方向**: 社会工程与人类因素

- **解读**: 边缘化群体（性工作/按摩行业女性）的数字安全需求常被主流安全研究忽视；她们在泰国芭堤雅面临诈骗、虐待等在线风险，且生存策略与数字实践深度交织，需要在地的、民族志式的理解才能设计有效的安全干预。

作者在芭堤雅一家妇女中心开展两个月的民族志田野调查，访谈 76 名参与者；聚焦数字安全相关实践：与"farang"（外国男性）的关系、线上自我展示、技术挪用与集体知识共享机制。

五项核心发现：（i）女性常把与 farang 的关系视为获得安全的最佳甚至唯一途径；（ii）线上展示策略模仿酒吧老板吸引顾客的广告方式；（iii）挪用被视为"西方理想"标签的技术为自己谋利；（iv）在诈骗与虐待等在线风险中摸索，这一过程塑造其对 farang 的搜寻；（v）通过知识共享形成保护彼此的集体安全机制。

我的思考：研究把"集体安全"从理论概念落地为可观察实践，揭示安全不是个人行为而是关系性、社群性的产物，对以社区为中心的安全干预设计有重要启发。局限在于单一地点与群体限制了普适性；研究者在敏感行业的身份与伦理边界使部分数据难以三角验证；从田野发现到可操作的安全产品/服务之间仍有距离。与主流"个人防护建议"范式形成对照，提示安全研究需纳入结构性不平等。


#### Toward Inclusive Security and Privacy for Deaf and Hard-of-Hearing People: A Community-Based Interview Study.

- **作者**: Mindy Tran, Xinru Tang, Adryana Hutchinson, Adam J. Aviv, Yixin Zou

- **方向**: 社会工程与人类因素

- **解读**: 聋人与听障（DHH）群体在使用辅助技术与在线服务时面临独特的安全隐私挑战：听力状况与手语内容可能泄露、安全资源不可访问，但安全与隐私研究几乎未覆盖该群体，包容性设计缺乏实证依据。

作者在德国开展基于社区的访谈研究：24 次半结构化访谈（17 名 DHH 参与者 + 7 名支持者），围绕辅助技术中的安全隐私风险感知、听力状况与手语内容披露顾虑、建议来源与障碍展开。

DHH 参与者对助听辅助设备的安全隐私风险认知普遍较低；对听力状况与手语视频的主动、被动或中介式披露存在顾虑；安全隐私资源不可访问、支持者相关知识有限，阻碍信息有效传递；社区依赖信任网络，翻译与社会动态构成额外障碍；研究提出参与式设计与多模态无障碍资源等建议。

我的思考：本文以社区参与方法填补了无障碍安全研究的空白，揭示"不可访问"本身即安全漏洞——用户被迫转向不可靠的信息渠道。局限在于样本集中于德国，手语方言、教育水平与数字素养差异未充分展开；24 名受访者规模有限；从访谈发现到无障碍 MFA、可访问安全提示等产品改造的落地路径仍需工程化验证。对无障碍安全研究具有奠基价值，为后续参与式设计提供了基线。


#### Human-Centered Threat Modeling in Practice: Lessons, Challenges, and Paths Forward.
- **作者**: Warda Usman, Yixin Zou, Daniel Zappala
- **方向**: 社会工程与人类因素
- **解读**: 

**问题**：以人为中心的威胁建模（HCTM）是安全与隐私研究的新兴方向，关注人们如何在各种社会、文化与技术情境中定义和应对威胁。虽已有SoK梳理文献，但对研究者实际如何准备与开展HCTM、面临哪些挑战所知甚少。论文通过对研究者的访谈揭示HCTM的幕后实践。

**方法**：作者对23位活跃于HCTM的研究者进行半结构化访谈（美国、英国、德国、加拿大，覆盖资深/中期/初级研究者），采用演绎与归纳混合的主题分析编码，并进行了成员核验（member checking）。访谈覆盖动机与价值观、groundwork（基础工作）、威胁引出（elicitation）、挑战、长期目标与建议等主题。

**结果**：HCTM并非规定性流程，而是由研究者与参与者的关系、学科背景与制度结构塑造的演化实践；研究者以持续groundwork与以参与者为中心的探询来建模，关怀、正义、自主等价值观驱动选题；同时面临情感负担、伦理困境（如是否告知参与者未意识到的威胁）与结构性障碍——发表压力、研究者-实践者鸿沟、成果转化困难；受访者呼吁共享基础设施、承认多元贡献、加强研究向政策与社会的转化。

**我的思考**：作为元研究，该工作为新手提供了难得的"幕后地图"，把HCTM从方法手册还原为关系性、情境化的实践，对价值负载科学的讨论也呼应了安全领域的范式之争。局限是样本以北美与欧洲为主（作者自述），基于自我报告且规模较小；"如何量化影响力"与"从洞察到可行动设计"的鸿沟，论文只给出方向性建议，尚未形成可操作的评估框架。

- **arXiv PDF**: [Human-Centered Threat Modeling in Practice: Lessons, Challenges, and Paths Forward.](https://arxiv.org/pdf/2511.13781)

#### LISA: A Scale-Optimized and Psychometrically-Validated Instrument for the Lightweight Assessment of Organizational Information Security Awareness in Heterogeneous Organizations.

- **作者**: David Langer, Jan Tolsdorf, Luigi Lo Iacono

- **方向**: 社会工程与人类因素

- **解读**: 组织信息安全意识（ISA）测量对安全行为研究与安全实践至关重要，但现有量表要么缺乏理论基础与心理测量学验证，要么过于冗长而难以在异构组织（如医院）中低成本施测，跨语言效度也常缺失，制约了研究与实践。

作者基于理论开发 LISA（轻量级信息安全意识量表），在尺度优化（精简题项）与心理测量学严谨性之间取得平衡；通过两个样本验证：1,182 名调查对象与一所大型德国大学医院的 579 名员工；检验内部一致性、结构效度、生态效度以及英语/德语跨语言测量不变性。

LISA 表现出高内部一致性、跨语言（英语/德语）测量不变性与良好的结构/生态效度；能够区分异构医院员工群体间的 ISA 水平，并与 11 个组织信息安全的使能/障碍因素相关；量表轻量易用，无需心理测量学专业背景即可部署。

我的思考：该工作为 ISA 研究提供"轻量而严谨"的标准工具，兼具理论建模与实操测量价值，双语不变性尤其适合跨国组织比较。局限在于样本集中于德语国家背景（调查面板 + 一家医院），行业多样性有限；自陈量表固有的社会期望偏差；"意识"与真实安全行为之间的鸿沟仍需纵向研究弥合。对安全行为干预的效果评估提供了可靠的度量基础，具有方法论示范意义。


#### Perceived Privacy Risk and Mitigation Post-Roe.

- **作者**: Alan F. Luo, Phoebe Moh, Cora Sula, Michelle L. Mazurek, Nora McDonald

- **方向**: 社会工程与人类因素

- **解读**: 美国推翻 Roe v. Wade 后，生育相关数据（位置、健康记录、经期追踪应用数据）的隐私风险剧增：执法部门与民间诉讼可依法索取数据，用户对"数据何时会被用于对自己不利"的担忧成为紧迫且广泛的社会问题，但缺乏系统的实证刻画。（无摘要，据标题推断）

作者研究用户在后 Roe 时代的隐私风险感知及其缓解行为：考察不同人群（尤其可能涉及生育决策者）如何重新评估位置、健康等敏感数据的风险，采取何种缓解措施（删除应用、限制权限、改变线下行为），并分析感知与行为之间的落差及其成因。（无摘要，据标题推断）

论文报告了感知风险与缓解行为的模式：特定群体显著提高风险意识并采取相应行动，但缓解行为的可行性与代价因资源、地域而异；具体发现与量化数据以原文为准。（无摘要，据标题推断）

我的思考：该研究把法律环境剧变作为自然实验，考察"制度性隐私风险"如何重塑用户感知与行为，对隐私政策与产品设计（尤其经期追踪类应用）有直接警示。局限在于自报感知可能受议题政治化影响；样本对跨州法律差异的覆盖需审视；研究时点距判决的时间距离影响行为稳定性，长期追踪缺失。与"威胁模型应包含国家行为体"的隐私研究转向一致，提示设计者需把法律风险纳入威胁模型。


#### International Students and Scams: At Risk Abroad.

- **作者**: Katherine Zhang, Arjun Arunasalam, Pubali Datta, Z. Berkay Celik
- **方向**: 社会工程与人类因素
- **解读**:
在美留学国际学生（IntlS）持学生签证求学，面临适应新国家与文化、远程租房、筹措学费与生活费等挑战；叠加近期签证吊销等事件，使其更易成为网络诈骗的目标与受害者，但此前研究几乎未专门考察国际学生受诈骗影响的独特方式。

论文开展两阶段用户研究：问卷调查（n=48）与半结构化访谈（n=9），考察国际学生接触诈骗的经历、接触后的行为与举报情况、对现有预防资源的看法，以及遵循防骗建议的障碍。

研究发现学生频繁遭遇诈骗（如攻击者冒充政府官员），因害怕法律后果或被驱逐出境，常带着紧迫感延长与诈骗者的互动；同时普遍缺乏获取可靠信息的渠道与举报指引。独特之处在于：学生必须证明自己有能力留美，这反而促使他们遵循防骗建议、避免财务损失。研究产出为利益相关方（学校、政府、平台）提供针对性指南。

该工作把“签证身份”纳入诈骗脆弱性分析，揭示法律恐惧如何被诈骗者武器化，填补了用户研究视角的空白；48+9 的样本规模适合质性深描但推广性有限。未来可扩展至其他签证类别与多国对比，并评估指南干预的实际效果。


#### From "Be Careful" to "Here's Why": Investigating User Reasoning with Context-Specific SMS Scam Warnings.
- **作者**: Elijah Robert Bouma-Sims, Enze Liu, Alexandra Xinran Li, Lorrie Faith Cranor
- **方向**: 社会工程与人类因素
- **解读**: SMS 诈骗每年造成巨额经济损失，而手机内置短信警告（如"小心诈骗"）往往笼统模糊，用户难以据此判断具体消息是否可信。该研究转向"情境化警告"（context-specific warnings）——不仅提示风险，还解释"为什么"（Here's Why）这条消息可疑，考察警告内容差异如何影响用户的推理过程与防骗效果。（无摘要，据标题推断）

方法上，据标题推断应是以用户为中心的行为实验：对照"Be Careful"式泛化警告与"Here's Why"式情境化警告，通过访谈、出声思考等方式观察用户如何解读警告、是否据此重新评估短信真实性，并分析其推理路径（对措辞、链接、身份线索等的注意）。

摘要缺失，无法引用量化结果；预期产出是警告设计对用户防骗推理影响的实证证据，以及情境化内容在什么条件下更有效、何时可能适得其反的结论。

我的思考：Cranor 团队在可用安全与警告设计上积累深厚，该选题切中"警告无效"这一现实痛点——泛化警告常被忽略，而"为什么"式解释可能提升说服力与信任度。潜在风险是情境化警告的编写成本与规模化问题，以及攻击者反向利用警告措辞规避用户警觉；无摘要使样本量、效果量与生态效度无法评估，需以正文为准。


#### No Password, No Problem? A Large-Scale Field Study of Passkey Adoption and Usage.
- **作者**: Tobias Reittinger, Günther Pernul
- **方向**: 社会工程与人类因素
- **解读**: 密码已成为认证安全的短板，FIDO2/passkey 被视为"无密码"的未来，行业与标准机构大力推动；但真实用户如何采用、在哪些场景使用、遇到什么障碍，此前缺乏大规模实地证据，部署方与研究者都缺少可靠的采纳画像。（无摘要，据标题推断）

据标题推断，本文开展大规模实地研究：通过遥测或日志收集真实 passkey 使用数据，分析注册率、登录成功率、跨设备同步行为、回退到密码的频率与场景，并结合用户侧信息刻画采纳障碍，形成实证的采纳与使用图谱。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告 passkey 的注册/使用比例、关键流失点与回退模式等量化发现。（无摘要，据标题推断）

我的思考："无密码"承诺与实际采纳之间的落差是认证研究的核心问题，大规模实地数据能纠正实验室结论与厂商宣传的偏差，对标准与产品设计有直接价值。局限：无摘要，数据来源的覆盖与隐私合规（遥测数据如何匿名化）需正文说明，结论受限于合作平台与用户群体的代表性。（无摘要，据标题推断）


#### Usable Anonymity in Reproductive Health Privacy.
- **作者**: Qiurong Song, Yanlai Wu, Rie Helene Hernandez, Yao Li, Yubo Kou, Xinning Gui
- **方向**: 社会工程与人类因素
- **解读**: 生殖健康信息高度敏感，相关搜索与记录一旦泄露可能带来污名化、社会关系受损甚至法律风险；传统隐私方案强调技术匿名性，却忽视用户在真实情境中的匿名需求、风险感知与可用性，导致保护措施与用户实际行为脱节。（无摘要，据标题推断）

据标题推断，本文从可用性角度研究生殖健康场景的匿名需求：通过访谈或调查理解用户（尤其弱势群体）如何理解与寻求匿名、现有工具（匿名浏览、加密、去标识化）为何未被采用，并分析情境性因素（共享设备、家庭成员、法律环境）对匿名实践的影响。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出用户匿名行为模式、工具使用障碍与设计启示等质性或量化发现。（无摘要，据标题推断）

我的思考：把"匿名"从密码学概念还原为用户的真实需求与行为，是对隐私研究的重要校准——技术再强，用户不敢用、不会用则等于零；该团队在人机交互与健康隐私方向有积累，质性深度可期。局限：无摘要，样本规模与群体代表性（地域、文化背景）影响外推，敏感话题访谈的伦理处理也需关注。（无摘要，据标题推断）


#### The Passkey Promise: A Comparative Usability Study of MFA Methods.
- **作者**: Erwin Kupris, Thomas Schreck
- **方向**: 社会工程与人类因素
- **解读**: 多因素认证（MFA）能显著提升账户安全，但不同方法的可用性差异直接决定用户接受度与部署率；passkey 被寄望取代既有 MFA（如 TOTP、推送、短信），其与成熟方案的可用性对比缺乏受控的实证证据。（无摘要，据标题推断）

据标题推断，本文开展受控比较可用性研究：在同一任务集（注册、登录、跨设备、恢复）上对比 passkey 与多种主流 MFA 方法，测量完成时间、成功率、错误率与主观体验（感知易用、偏好），并考察用户对 passkey 概念的信任与理解。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应给出各方法在任务成功率、耗时与主观评分上的对比数据及显著性检验。（无摘要，据标题推断）

我的思考："承诺"与"现实"的对照正是认证研究需要的证据——若 passkey 在受控测试中并不优于成熟 MFA，厂商的迁移叙事就需重新审视；比较方法（被试内/间设计、样本量、任务设计）决定结论可信度。局限：无摘要，样本规模与招募渠道、设备生态（Apple/Google/Windows）的覆盖需正文确认，实验室结果与实地采纳仍有距离。（无摘要，据标题推断）


#### Behind the Curtain: How Shared Hosting Providers Respond to Vulnerability Notifications.

- **作者**: Giada Stivala, Rafael Mrowczynski, Maria Hellenthal, Giancarlo Pellegrino

- **方向**: 社会工程与人类因素

- **解读**: 大规模漏洞通知（VN）旨在帮助托管服务商（HPO）修复被攻击者利用的漏洞，但既往研究只关注报告方可控因素（发件人声誉、邮件格式、渠道），修复率仍持续偏低，对服务商内部处理流程缺乏理解。

本文从服务商视角出发，直接访谈 24 家共享托管/Web 开发服务提供商（覆盖不同规模与角色），调查其内部通知处理流程、责任边界、可触达性、服务模型与感知风险等组织与运营因素。

结果显示：部分服务商因基础设施复杂难以触达，但多数会例行处理 VN；修复率低常源于严格的责任边界——应用层问题被视为客户职责；低收费与每日大量被入侵站点进一步削弱修复意愿；服务商归咎于疏忽的网站所有者，而既往研究显示所有者低估站点价值且缺乏技能，责任错位使现有通知模式的有效性存疑。

我的思考：这是首个系统刻画 VN 失败根因的组织层面研究，把"通知有效性"从报告方技术参数转向服务商运营现实，对通知策略设计有直接指导意义；局限在于 24 家访谈样本的覆盖面与自述偏差，改进建议（责任划分、激励设计）的落地效果仍需实证。


#### LLMs in the SOC: An Empirical Study of Human-AI Collaboration in Security Operations Centres.
- **作者**: Ronal Singh, Shahroz Tariq, Fatemeh Jalalvand, Mohan Baruwal Chhetri, Surya Nepal, Cécile Paris, Martin Lochner
- **方向**: 社会工程与人类因素
- **解读**: LLM 进入安全运营中心（SOC）被视为减轻分析师负担的变革机会，但真实世界的应用情况缺乏实证：现有研究多为实验室环境或系统级性能评估（如 Copilot Guided Response），并不了解分析师在日常工作中实际如何使用 LLM。本文填补这一空白。

方法上，作者对 eSentire 公司 45 名 SOC 分析师在 10 个月内（2023 年 5 月至 2024 年 3 月）提交给 GPT-4 的 3,090 条查询做了纵向研究，并设计了结构化编码框架，从查询级（内容、措辞与 NICE 框架对齐）、会话级（多轮交互结构与推理模式）、分析师级（个体使用频率与方式）三个层面进行主题分析。

结果显示，分析师把 LLM 当作按需认知辅助，用于意义建构与上下文构建，而非做出高利害判定，保留最终决策权；多数查询是解读低级遥测（如命令）与润色技术沟通，交互通常很短（1–3 轮）；93% 的查询对齐 NICE 框架定义的网络安全能力，表明 LLM 使用与 SOC 核心任务高度相关；使用趋势从偶发探索转向日常例行整合，部分分析师形成持续使用习惯。

我的思考：这是首个真实 SOC 环境中人-AI 协作的纵向实证，"辅助而非替代、保留决策权"的结论与 93% 的 NICE 对齐数据为安全 AI 产品设计提供了直接依据，三条设计建议（微任务内嵌辅助、证据优先于建议、上下文自适应解释）具体可落地。局限在于单一企业、单一 LLM（GPT-4）与纯文本交互，结论外推到不同 SOC 文化与多模态场景需谨慎。与 CGR 等系统级评估互补：前者回答"系统能做什么"，本文回答"人实际怎么用"。
- **arXiv PDF**: [LLMs in the SOC: An Empirical Study of Human-AI Collaboration in Security Operations Centres.](https://arxiv.org/pdf/2508.18947)

### 程序分析与形式化验证（8 篇）

#### DY* Unchained: Now with Composable Security Proofs and Precise Compromise Scenarios.
- **作者**: Théophile Wallez
- **方向**: 程序分析与形式化验证
- **解读**: 密码协议实现的形式化验证长期受困于可组合性难题：单个协议证明正确并不保证组合系统依然安全；同时既有模型多采用"全有或全无"的密钥泄露假设，难以刻画现实中部分组件失陷的场景。DY* 是构建在 F* 之上的符号验证框架（基于 Dolev-Yao 模型），本文（标题即续作）为其补上"可组合安全证明"与"精确妥协场景"两大能力。

据标题推断，作者为 DY* 引入可组合的证明机制：定义组合安全性质与对应的组合定理，使独立验证过的协议能够安全拼接而不必重新证明；同时把"妥协场景"建模为精确的攻击者能力集合，允许逐组件、逐密钥地刻画失陷状态，取代以往笼统的整体沦陷假设。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；标题表明核心交付是框架层面的理论扩展——可组合证明规则与精细妥协场景的刻画，并配套 DY* 工具链的机器检查证明。

可组合安全与渐进式妥协是协议验证走向实用必须跨越的门槛，若该扩展以 F* 机器证明形式落地，将显著提升 DY* 对 TLS、信号协议等现实协议栈的分析可信度。局限在于内容系据标题推断，且组合定理往往需要手工构造组合不变式，自动化程度有待观察；但方向本身无可争议。


#### Detecting Privilege Escalation in Polyglot Microservices via Agentic Program Analysis.

- **作者**: Penghui Li, Hong Yau Chong, Yinzhi Cao, Junfeng Yang

- **方向**: 程序分析与形式化验证

- **解读**: 微服务是现代云系统的主导架构，但权限与许可控制被分散到大量跨服务、多语言的组件中，特权提升漏洞（攻击者借越权访问资源或操作）难以检测：跨服务调用链复杂、代码库异构、特权操作与权限检查形态多样。此类漏洞一旦被利用即可造成横向越权，风险极高，因此自动化检测具有重要价值。

Neo 将大语言模型（LLM）与经典程序分析结合：LLM 智能体动态生成分析计划、自适应调整代码搜索策略并对语义进行验证；作者开发了跨服务、跨语言的代码搜索原语，使 Neo 能以可扩展且灵活的方式在服务间探索代码，兼顾结构精确性与跨服务规模。

在 25 个开源微服务应用（7 种编程语言、约 620 万行代码）上，Neo 发现 24 个零日特权提升漏洞；在含 20 个已确认漏洞的 ground-truth 数据集上达到 81.0% 精确率与 85.0% 召回率（17 真阳、4 假阳、3 漏报）。与既有程序分析及 agentic 方案相比，Neo 在检测精度与可扩展性上均有显著提升；将其扩展到其他应用领域与漏洞类型后，又发现 18 个零日漏洞。

我的思考：核心贡献是把"LLM 智能体+经典分析"做成可落地的跨服务漏洞检测系统，为多语言微服务特权分析提供了新范式；扩展性展示（额外 18 个零日）说明方法可迁移。局限在于 ground-truth 规模偏小（20 个），对复杂权限语义的理解仍依赖 LLM 能力，误报与漏报的边界有待更大规模评估；与人工审计的成本对比也尚缺数据。整体上展示了 LLM 与程序分析互补的实用方向。

- **arXiv PDF**: [Detecting Privilege Escalation in Polyglot Microservices via Agentic Program Analysis.](https://arxiv.org/pdf/2605.15569)

#### The Secrets Must Not Flow: Scaling Security Verification to Large Codebases.
- **作者**: Linard Arquint, Samarth Kishor, Jason R. Koenig, Joey Dodds, Daniel Kroening, Peter Müller
- **方向**: 程序分析与形式化验证
- **解读**: 
形式化验证能对软件安全属性给出数学级保证，但传统验证工具难以扩展到真实的大规模代码库——复杂的推理、巨大的状态空间与人工标注负担使"全库验证"不切实际。如何让安全验证（例如秘密信息不得流向未授权位置）在大型代码库上落地，是形式化方法走向工业应用的核心瓶颈。

据标题推断，论文提出可扩展至大型代码库的安全验证方法：通过模块化推理、自动化的规范推断与注解生成、增量验证等技术，将流安全（信息流控制）属性的验证规模化，并配合高效的求解与抽象策略降低验证成本，使"秘密不泄露"的保证覆盖真实规模的软件。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文应报告在大型真实代码库上的验证规模（代码行数）、验证开销与发现的漏洞或证明结果，具体数据以全文为准。

该工作汇聚了形式化验证领域的顶尖力量（Müller为Viper/Prusti作者，Kroening为CBMC作者），"规模化"是其关键词——验证工具真正进入工程实践的障碍正在于此，流安全是安全验证中最具实用价值的属性之一。局限在于自动化程度与精度难以兼得，注解负担仍是推广障碍；若能显著降低人工成本，将推动形式化验证在安全敏感软件中的实际采用。


#### Mechanized Safety and Liveness Proofs for the Mysticeti Consensus Protocol Under the LiDO-DAG Framework.
- **作者**: Longfei Qiu, Jingqi Xiao, Zhong Shao
- **方向**: 程序分析与形式化验证
- **解读**: 
Mysticeti是Sui区块链采用的高性能DAG共识协议，以低延迟与高吞吐著称；共识协议的正确性直接关系资产安全，其安全性与活性（liveness）性质必须严格成立。然而DAG共识协议状态复杂、论证繁琐，人工证明易出错，机械化证明是确保其正确性的可靠途径，也是形式化方法最具价值的应用场景。

据标题推断，论文在LiDO-DAG框架下对Mysticeti进行机械化验证：将协议建模为DAG共识的形式化框架，用证明助手（如Coq）分别证明其安全性（一致性、无分叉）与活性（最终性、进展性）性质，形成可机器检查的完整正确性论证，并分析证明工作的结构。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；论文的产出应是覆盖安全性与活性性质的机械化证明及其工作量或代码量统计，具体数据以全文为准。

区块链共识协议的正确性证明是形式化方法最富价值也最具难度的应用场景之一（此前已有对HotStuff、Narwhal等的验证），Mysticeti作为部署中的高性能协议，其机械化验证具有直接的实际意义。Zhong Shao团队（VST、CompCert相关）在机械化验证领域权威。局限在于协议抽象与实现之间的鸿沟仍需桥接，且验证工作量大；该工作为DAG共识族提供了可复用的证明框架，示范意义明确。


#### Language-Agnostic Detection of Computation-Constraint Inconsistencies in ZKP Programs Via Value Inference.
- **作者**: Arman Kolozyan, Bram Vandenbogaerde, Janwillem Swalens, Lode Hoste, Stefanos Chaliasos, Coen De Roover
- **方向**: 程序分析与形式化验证
- **解读**: ZKP 程序（Circom、Noir 等）中"计算逻辑"与"电路约束"的不一致是欠约束/过约束漏洞的根源，但现有检测工具多绑定单一 DSL，难以跨语言复用。（无摘要，据标题推断）

据标题推断，该方法通过值推断（value inference）跨语言检测计算-约束不一致：静态分析 witness 计算中的变量取值域，与约束系统允许的取值域做对比，找出约束未覆盖或过度限制的路径，且不依赖特定语言的语法细节，从而泛化到多种 ZKP 语言。

摘要缺失，无法引用量化结果；预期包含在多个语言（Circom、Noir 等）程序集上的检出率、误报率及与 zkFuzz 等工具的比较。

我的思考：与 zkFuzz 的模糊测试路线互补，这是"静态+形式化"路线——值推断天然可扩展且无运行开销，语言无关设计直击 ZK 生态多 DSL 并存的现实。Chaliasos 等作者在 ZK 程序分析（如 Circom 审计工具）上有积累，可信度高。挑战在于值推断的精度（复杂计算路径下域分析的松紧）与对 witness 生成器行为的建模；无摘要使覆盖面与精度指标待正文确认。


#### Best of Both Worlds: Effective Foreign Bridge Identification in V8 Embedders for Security Analysis.
- **作者**: Georgios Alexopoulos, Thodoris Sotiropoulos, Zhendong Su, Dimitris Mitropoulos
- **方向**: 程序分析与形式化验证
- **解读**: 现代 JS 运行时（V8）常被嵌入 Node.js、Electron 等宿主，宿主通过"外来桥"（foreign bridge）把原生 C++ 函数暴露给 JS；这些桥既是攻击面（内存错误、类型混淆的高发区），也是安全分析的入口，但桥的数量庞大、边界模糊，缺乏自动化识别手段。（无摘要，据标题推断）

据标题推断，本文提出高效的外来桥识别方法，在 V8 嵌入层同时利用静态与动态分析的优势（"两全其美"）：以静态分析定位跨语言调用的候选点，以动态证据或语义分析过滤确认，从而构建完整的 JS 与原生边界图，为后续漏洞挖掘、污点分析等安全任务提供可靠基础。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应报告桥识别在多个真实嵌入器（如 Node.js、Electron）上的覆盖率、精确率与漏报情况。（无摘要，据标题推断）

我的思考：嵌入器边界是供应链安全与漏洞研究的富矿——大量真实漏洞发生在"JS 调 C++"的转换层；自动、可信的桥识别能把分析范围从手工列举解放出来。作者含 Su 与 Mitropoulos，分别在程序分析与 JS 生态安全上有建树，方法可信度较高；局限是无摘要，具体机制与评估规模需正文确认。（无摘要，据标题推断）


#### SFA-Miner: Mining Path-Sensitive API Usage Patterns Via Symbolic Finite Automata.
- **作者**: Jiasheng Jiang, Mingwei Zheng, Qingkai Shi, Xiangyu Zhang
- **方向**: 程序分析与形式化验证
- **解读**: API 误用是软件缺陷与漏洞的重要来源，从代码库中挖掘"正确/违规的 API 使用模式"是检测误用的常见思路；但 API 使用常依赖具体路径条件（分支、状态），现有模式挖掘多为路径不敏感，模式粒度粗、误报高。（无摘要，据标题推断）

据标题推断，SFA-Miner 用符号有限自动机（SFA）刻画 API 调用序列：把路径条件编码为符号转移上的谓词，在过程间、路径敏感的意义上归纳高频且一致的调用模式，再以模式为规范检测偏离行为；作者团队（Purdue，Xiangyu Zhang 组）在程序分析与漏洞检测上有系统积累。（无摘要，据标题推断）

摘要缺失，无法引用量化结果；按标题推断，论文应在大型真实项目上报告挖掘模式的规模、质量（精确率）与下游误用检测效果。（无摘要，据标题推断）

我的思考：路径敏感让"模式"从语法序列升级为"带条件的协议"，理论上能显著压低误报、提高可用性；SFA 提供优雅的代数框架（可判定、可最小化），工程上关键看可扩展性。局限：无摘要，挖掘的可扩展性与模式评价基准需正文确认；与既有 API 模式挖掘工作（基于频繁序列或图的方法）的定量对比是判断增量成色的关键。（无摘要，据标题推断）


#### PortGPT: Towards Automated Backporting Using Large Language Models.
- **作者**: Zhaoyang Li, Zheng Yu, Jingyi Song, Meng Xu, Yuxuan Luo, Dongliang Mu
- **方向**: 程序分析与形式化验证
- **解读**: 补丁 backport（把主线安全补丁迁移到 stable/LTS 分支）是 Linux 内核等大型开源项目维护的刚需，但人工执行劳动密集；现有自动化工具（FIXMORPH、TSBPORT）依赖预定义语法或语义规则，遇到函数改名、代码移位、栈上改为堆上声明等变化便失效，且绑定特定语言。

方法上，PORTGPT 以"模拟人类专家"为设计哲学，为 LLM（GPT-4o）配备工具集：按需定位符号与访问代码、总结 Git 历史、依据编译器和测试反馈自主修订补丁（反例引导式精化），将定位（localization）与变换（transformation）两个阶段都交由智能体推理完成，形成端到端自动化流水线。

在既有数据集 1,815 例（TSBPORT 的 1,465 个 Linux 内核 CVE 加 FIXMORPH 的 350 个 bug）上，PORTGPT 成功率达 89.15%；在自建的 146 例复杂用例（34 个程序、覆盖 C/C++/Go）上成功率为 62.33%，分别比 FIXMORPH 与 TSBPORT 高 56.53 与 26.09 个百分点；在 Linux 6.1-stable 分支上成功 backport 18 例中的 9 例且全部被内核社区合并，Ubuntu 上 16 组补丁对成功 10 组（对应 10 个 CVE）。

我的思考：把 backport 从"规则模板匹配"升级为"智能体推理+工具调用+反馈回路"，89.15% 的高成功率和 9 个真实合并补丁证明了实战价值，语言无关性也优于既有工具。局限在于依赖 GPT-4o 的推理成本、复杂用例成功率仍只有六成、验证主要靠编译与测试反馈（测试缺失时风险上升），且 LLM 幻觉仍需人工把关。与 Mystique、PPathF 等仅做补丁变换的上下文学习方法相比，这是完整的端到端系统。

- **arXiv PDF**: [PortGPT: Towards Automated Backporting Using Large Language Models.](https://arxiv.org/pdf/2510.22396)
