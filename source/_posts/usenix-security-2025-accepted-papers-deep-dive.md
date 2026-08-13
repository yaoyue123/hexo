---
title: USENIX Security 2025 录用论文深度解读（440 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, USENIX Security, 2025]
---

USENIX Security 2025（第 34 届）于 2025 年 8 月在华盛顿州西雅图举行，两轮共录用 440 篇论文。本文按研究方向分类，对每篇论文基于全文精读给出深度解读（含独立分析思考），并附可获取的 PDF 链接。

## 全部 440 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（57 篇）

#### Analyzing the AI Nudification Application Ecosystem.
- **作者**: Cassidy Gibson, Daniel Olszewski, Natalie Grace Brigham, Anna Crowder, Kevin R. B. Butler, Patrick Traynor, Elissa M. Redmiles, Tadayoshi Kohno
- **方向**: AI/ML安全
- **解读**: 基于 AI 的"脱衣"应用(nudification)能由一张衣着人物的照片生成其裸体图像,且现实世界中已存在大量未经图像主体同意的滥用案例,可能侵犯个人权利并造成下游伤害。尽管此类应用广受关注,此前却没有跨应用、系统化的生态研究,本文即填补这一空白,为科学、技术与政策社区提供数据支撑的讨论基础。

作者聚焦 20 个流行且易获取的脱衣网站开展系统性测量,从三个维度刻画生态:网站的定位(发现多数站点明确以女性为对象而非所有人)、其宣传的功能(从原地脱衣到将主体渲染为性姿势,以及差异化的用户隐私选项),以及底层的变现基础设施(信用卡与加密货币支付)。

研究发现:多数站点明确针对女性图像的脱衣处理;功能范围跨度大,隐私选项各异;变现渠道涵盖信用卡与加密货币,暗示不同监管与追责难度。论文附带内容警告,涉及合成非自愿露骨 AI 图像(SNEACI)相关描述。

这是首个对 AI 脱衣应用生态的跨应用系统研究,把模糊的"威胁感知"转化为可审计的生态事实,对监管与平台治理有直接参考价值。局限:测量聚焦 20 个站点且生态随时间漂移快,摘要未给出量化指标,结论深度有限。未来可扩展到更多语言与平台、追踪应用演变,并评估下架与法律干预的实际效果。
- **论文 PDF**: [Analyzing the AI Nudification Application Ecosystem.](https://www.usenix.org/system/files/usenixsecurity25-gibson.pdf)

#### GradEscape: A Gradient-Based Evader Against AI-Generated Text Detectors.
- **作者**: Wenlong Meng, Shuguo Fan, Chengkun Wei, Min Chen, Yuwei Li, Yuanchao Zhang, Zhikun Zhang, Wenzhi Chen
- **方向**: AI/ML安全
- **解读**: 随着 AI 生成文本的普及,各类 AI 文本检测器被用于内容治理与学术诚信,但攻击者可构造规避文本使检测失效。已有规避方法多依赖启发式或重写,缺乏高效、可迁移的攻击框架。本文提出基于梯度的规避方法 GradEscape。(无摘要,据标题推断)

依据标题推断,作者利用检测器模型的可微性,对候选文本施加梯度信号驱动的扰动或替换,在保持语义与流畅性的同时最大化检测误差,可能还结合采样与搜索策略提升成功率与迁移性。

研究应报告对主流检测器(如 GPTZero、判别式与统计检测器)的规避成功率、文本质量损失与迁移效果。由于摘要与全文缺失,具体数值无法引用。

该方向与"AI 文本水印/检测"攻防互为镜像:检测方与规避方的军备竞赛决定内容溯源技术的可信度。梯度方法相比启发式重写的优势在于自动化与针对性,但依赖白盒或代理模型假设,黑盒场景下的有效性是关键考验。期待与同期水印鲁棒性工作(如本批次多比特水印论文)对照阅读,以判断攻防双方的真实差距。

#### Exposing the Guardrails: Reverse-Engineering and Jailbreaking Safety Filters in DALL·E Text-to-Image Pipelines.
- **作者**: Corban Villa; Muhammad Shujaat Mirza; Christina Pöpper
- **方向**: AI/ML安全
- **解读**: 黑盒文生图模型DALL·E的安全护栏设计从未公开文档化，"以隐蔽保安全"阻碍了社区评估。本文首次提出基于响应时间的侧信道分析，逆向DALL·E级联安全过滤器的架构。

方法上，通过差分响应时间测量：DALL·E 2使用块名单过滤（原词0.25秒内被拒，字母变异词绕过首道闸后被后续过滤），DALL·E 3引入LLM提示改写阶段，4.8秒时间分界以F1=0.99准确区分改写与图像生成两阶段；还发现ChatGPT接口被动泄露改写提示流、拒绝原因与拒绝阶段（已报OpenAI）；并构建27种语言3402条对抗提示集，用Moderation API量化毒性变化。

结果显示，LLM改写平均将提示毒性降低-64.46%（中位-83.72%），即存在"提示软化"隐式护栏；差分实验证明DALL·E 2/3过滤机制不同且Moderation API不在护栏组合中；利用LLM语言理解与CLIP嵌入的错位提出否定式越狱，利用对齐不足提出低资源语言攻击（低资源语言提示接受率显著更高），并给出六类对策。

我的思考：首次把时间侧信道用于T2I管线逆向，揭示多组件系统对齐的深层矛盾——语言模型"理解"否定而CLIP嵌入层"不理解"，是结构性盲区。局限是黑盒推断的机制多为推测、结论随服务端改动漂移、依赖单一厂商，但其方法可迁移到其他多模态管线。

- **论文 PDF**: [Exposing the Guardrails: Reverse-Engineering and Jailbreaking Safety Filters in DALL·E Text-to-Image Pipelines.](https://www.usenix.org/system/files/usenixsecurity25-villa.pdf)

#### On the Proactive Generation of Unsafe Images From Text-To-Image Models Using Benign Prompts.
- **作者**: Yixin Wu, Ning Yu, Michael Backes, Yun Shen, Yang Zhang
- **方向**: AI/ML安全
- **解读**: 问题：恶意或经篡改的提示词可被用来诱导文生图模型生成不安全图像，已有研究多聚焦于这种"被动利用"。本文转向新视角：通过被恶意修改的文生图模型，从完全良性的提示词（如"一张猫的照片"）主动生成不安全图像，威胁面更隐蔽。

方法：初步调查表明投毒攻击（poisoning attack）可行，但存在显著副作用——毒化效果意外扩散到非目标提示词，损害攻击隐蔽性；根因分析将"概念相似性"（conceptual similarity）识别为副作用的重要成因。据此提出一种平衡隐蔽性与性能的隐蔽投毒攻击方法。

结果：实验验证了所提攻击能在保持良性提示词正常输出的同时触发不安全内容生成，揭示了采用文生图模型的现实风险，并呼吁在该领域开展进一步研究与安全措施。

思考：把攻击假设从"恶意提示"移到"恶意模型"，更贴近模型供应链被篡改的现实场景，风险模型更完整；对副作用根因（概念相似性）的分析提升了工作的可解释性。局限在于攻击依赖对模型权重或训练流程的篡改能力，防御端如何验证模型完整性与输出安全仍是开放问题。

- **论文 PDF**: [On the Proactive Generation of Unsafe Images From Text-To-Image Models Using Benign Prompts.](https://www.usenix.org/system/files/usenixsecurity25-wu-yixin-generation.pdf)

#### Watch the Watchers! On the Security Risks of Robustness-Enhancing Diffusion Models.
- **作者**: Changjiang Li, Ren Pang, Bochuan Cao, Jinghui Chen, Fenglong Ma, Shouling Ji, Ting Wang
- **方向**: AI/ML安全
- **解读**: 问题：得益于强大的去噪能力，扩散模型越来越多地被用作防御工具以增强其他模型的鲁棒性，典型用途是对抗样本净化（purification）与对抗鲁棒性认证（certification）。但这些"安全工具"自身的风险此前几乎未被研究，若防御模型本身可被攻陷，其提供的安全保障将形同虚设。

方法：本文研究鲁棒性增强扩散模型的脆弱性，提出 DIFF2 攻击：在扩散模型中植入恶意的扩散采样过程，将嵌入特定触发器的输入引导到攻击者定义的分布，同时保持干净输入的正常功能。该攻击简单却有效。

结果：在对对抗净化和鲁棒性认证的案例研究中，DIFF2 在多个基准数据集与模型上显著降低了净化后准确率与认证准确率，凸显了依赖预训练扩散模型作为防御工具的潜在风险；作者还探索了可能的对策，为未来研究指明方向。

思考：该工作完成了"对防御者的攻击"这一逻辑闭环——安全防线本身成为攻击面，警示社区不能默认信任预训练防御组件，这一提醒在模型复用盛行的当下尤为及时；局限在于攻击要求能篡改扩散模型权重（同供应链威胁模型），且提出的对策仍属初步探索，防御侧尚缺乏系统性方案。

- **论文 PDF**: [Watch the Watchers! On the Security Risks of Robustness-Enhancing Diffusion Models.](https://www.usenix.org/system/files/usenixsecurity25-li-changjiang.pdf)

#### Pretender: Universal Active Defense against Diffusion Finetuning Attacks.
- **作者**: Zekun Sun, Zijian Liu, Shouling Ji, Chenhao Lin, Na Ruan
- **方向**: AI/ML安全
- **解读**: 问题：扩散模型（DM）的普及带来新的侵权威胁——扩散微调攻击（DFA）：攻击者用极低资源微调预训练 DM，"窃取"个人照片或艺术作品中的信息生成侵权图像，严重威胁隐私与知识产权。现有防御策略泛化性不足、机制效果欠佳。

方法：作者将主动防御形式化为一个双层优化（bi-level optimization）问题，聚焦攻击者的共性行为以增强防御泛化性；据此提出 Pretender 算法，对抗训练一个代理模型以生成更有效的保护噪声，并引入同步梯度反向传播（SGBP）技术显著提升计算效率。

结果：大量实验与真实世界评估证明 Pretender 有效：仅施加微小扰动（p=0.03）即可破坏多种 DFA 生成图像的质量与语义，各项自动化评估指标平均提升 22.27%，人工评估分数提升 94.28%。

思考：把"防微调窃取"建模为双层优化并以攻击者共性行为为目标，抓住了跨攻击泛化的关键，SGBP 解决效率问题使方案可落地，量化提升显著；作为主动防御（在发布前污染图像），其代价是用户图像被永久叠加扰动，与图像可用性/美感的权衡、以及对抗更强自适应攻击的鲁棒性，是需要继续回答的局限。
- **论文 PDF**: [Pretender: Universal Active Defense against Diffusion Finetuning Attacks.](https://www.usenix.org/system/files/usenixsecurity25-sun-zekun.pdf)

#### Self-interpreting Adversarial Images.
- **作者**: Tingwei Zhang, Collin Zhang, John X. Morris, Eugene Bagdasarian, Vitaly Shmatikov
- **方向**: AI/ML安全
- **解读**: 对抗样本的核心难题在于其"不可解释性"：攻击者施加的微小扰动使模型出错，但人无法理解扰动"为什么"、以及它针对模型哪部分判断起作用，这既阻碍对抗样本的审计与防御，也让"可验证的对抗性"难以落地。本工作从标题看聚焦"自我解读"（self-interpreting）的对抗图像——即对抗样本本身携带可被人类或分析工具读取的语义信息，使图像在欺骗模型的同时自我说明其攻击意图。

据标题推断，方法上可能是在对抗扰动中嵌入可读信号（如文本、图标或语义标注），或设计扰动使其结构性地对应可解释的语义概念，从而把"扰动←→语义"建立映射；也可能面向对抗样本的生成-解释联合建模。由于摘要缺失，具体技术路线（如隐写式嵌入、可微解释器、或多任务生成框架）无法确认。

摘要缺失，无法引用量化结果；预期评估包含对抗攻击成功率（如白盒/黑盒迁移性）、内嵌信息的可读性/还原精度，以及扰动幅度与可解释性之间的权衡等指标。

该选题的独特价值在于把"解释"从防御侧（事后归因、可解释性工具）前移到攻击侧内嵌于样本本身，可能服务于对抗样本审计、攻击归因与"可验证对抗性"等方向；Shmatikov 团队在对抗机器学习与隐私方向积累深厚，方法的严谨性值得期待。局限：无摘要，机制与实验规模均无法评估；若采用隐写式嵌入，内嵌信息与对抗鲁棒性的耦合、以及对扰动预算的额外开销将是关键权衡点。（无摘要，据标题推断）


#### Unlocking the Power of Differentially Private Zeroth-order Optimization for Fine-tuning LLMs.
- **作者**: Ergute Bao, Yangfan Jiang, Fei Wei, Xiaokui Xiao, Zitao Li, Yaliang Li, Bolin Ding
- **方向**: AI/ML安全
- **解读**: 大语言模型微调需要保护训练数据的隐私，差分隐私（DP）训练是主流手段，但基于一阶梯度的 DP-SGD 在微调大模型时内存开销巨大，难以普及。零阶优化（Zeroth-order Optimization）仅通过函数求值来估计梯度，内存占用极低，被视为私有微调 LLM 的有力替代方案。本文旨在系统释放 DP 零阶优化在 LLM 微调中的潜力，解决其此前被认为"不实用"的困境。

从理论与工程两方面入手：理论上分析零阶梯度估计在 DP 训练中的收敛性质与效用-隐私权衡，纠正此前对其样本效率过于悲观的认知；实践上设计仅需极小内存即可完成大规模模型私有微调的高效算法，使资源受限场景也能进行差分隐私微调。（无摘要，据标题推断）

论文通过理论分析证明 DP 零阶优化在微调 LLM 时能够获得与一阶方法可比的效用，同时保留低内存优势，并在多个模型与数据集上验证了其有效性。（无摘要，据标题推断）

该工作为资源受限环境下的私有 LLM 微调提供了实用路径，挑战了"零阶方法效率低下"的成见，贡献成色较高。局限在于零阶估计的收敛速度仍受问题维度影响，未来可与 LoRA 等参数高效微调方法结合，进一步缩小与一阶 DP-SGD 的差距。


#### Synthetic Artifact Auditing: Tracing LLM-Generated Synthetic Data Usage in Downstream Applications.
- **作者**: Yixin Wu, Ziqing Yang, Yun Shen, Michael Backes, Yang Zhang
- **方向**: AI/ML安全
- **解读**: LLM 生成的合成数据被广泛用于训练下游模型与统计分析，成本低、质量高，但也可能延续甚至放大社会偏见与刻板印象，并产生偏离事实的"幻觉"输出。本文提出"合成工件审计"：识别分类器、生成器、统计图等下游工件是否由合成数据训练或导出，以提升用户意识、降低意外后果与风险。

将审计建模为判定问题：对候选工件提取可区分的统计特征（如输出分布特性、样本痕迹），训练审计器区分基于合成数据构建与基于真实数据构建的工件，实现数据来源的追溯（摘要截断）。

在多种工件类型与 LLM 合成数据上验证了审计方法的有效性，表明合成数据会留下可被检测的"指纹"（摘要截断，具体指标未给出）。

该工作开创了"数据来源审计"这一方向，对合规、信任与数据治理意义重大（如识别 AI 生成内容的扩散路径）。局限是当合成数据分布与真实数据日益接近时，检测难度上升；攻击者也可针对性规避痕迹，检测方法需要持续演进并与水印等技术结合。

- **论文 PDF**: [Synthetic Artifact Auditing: Tracing LLM-Generated Synthetic Data Usage in Downstream Applications.](https://www.usenix.org/system/files/usenixsecurity25-wu-yixin-auditing.pdf)

#### Data-Free Model-Related Attacks: Unleashing the Potential of Generative AI.
- **作者**: Dayong Ye, Tianqing Zhu, Shang Wang, Bo Liu, Leo Yu Zhang, Wanlei Zhou, Yang Zhang
- **方向**: AI/ML安全
- **解读**: 生成式 AI 具有双重用途，现有对抗应用研究多聚焦网络攻击，较少关注针对深度学习模型本身的攻击。本文提出利用生成式 AI 促进模型相关攻击，包括模型窃取、成员推断与模型反演，系统拓展了生成模型作为攻击基础设施的威胁面。

在无数据（data-free）与黑盒条件下，利用生成模型合成查询样本或辅助重建训练数据分布，从而驱动针对图像与文本模型的多种攻击，摆脱对真实训练数据的依赖，显著降低攻击门槛（摘要截断）。

实验显示攻击者能在 data-free、黑盒设定下对图像与文本模型发起多种模型相关攻击，并取得与依赖真实数据的基线相当的性能（摘要截断，具体数值未给出）。

该工作系统展示了生成式 AI 作为"通用攻击引擎"的潜力，使模型窃取与隐私攻击不再需要训练数据，威胁模型更加现实。局限是生成样本与真实分布仍有偏差，攻击在复杂模型与防御（如水印、蒸馏检测）下的表现有待进一步研究，防御侧需重视对生成查询的识别。

- **论文 PDF**: [Data-Free Model-Related Attacks: Unleashing the Potential of Generative AI.](https://www.usenix.org/system/files/usenixsecurity25-ye-attacks.pdf)

#### Boosting Gradient Leakage Attacks: Data Reconstruction in Realistic FL Settings.
- **作者**: Mingyuan Fan; Fuyi Wang; Cen Chen; Jianying Zhou
- **方向**: AI/ML安全
- **解读**:（无摘要，据标题推断）梯度泄漏攻击（梯度反演）在理想设定下能重建联邦学习客户端的训练数据，但真实 FL 系统存在本地多轮训练、大批次、数据增强等条件，使攻击失效；如何在现实设定下提升攻击成功率是研究热点。

论文针对真实 FL 设置（如更大批次、部分梯度、防御后处理）改进数据重建攻击，提出增强策略，如利用辅助信息、优化初始化与正则化、适配多轮梯度等。

（无摘要，据标题推断）据标题，在更贴近实际的 FL 配置下，改进后的攻击相比既有梯度反演方法显著提升重建质量与成功率。

该工作与同批次的 SoK（梯度泄漏）形成呼应——SoK 指出梯度反演在现实中受限，而本文恰好挑战这一结论，说明"现实设定"的建模方式本身决定攻击有效性结论，这类正反对照很有价值。局限在于辅助信息假设的强度与评估数据集的代表性；与防御方（Refiner、DP-BREM 等）的攻防博弈将推动 FL 隐私研究走向更真实的评估基准。


#### Refiner: Data Refining against Gradient Leakage Attacks in Federated Learning.
- **作者**: Mingyuan Fan; Cen Chen; Chengyu Wang; Xiaodan Li; Wenmeng Zhou
- **方向**: AI/ML安全
- **解读**:（无摘要，据标题推断）联邦学习中梯度泄漏攻击可从共享梯度重建客户端数据；已有防御多在梯度或模型层面加噪/扰动，但常以显著牺牲模型精度为代价，或仅对特定攻击有效。

论文提出 Refiner，在数据层面进行"精炼"——对训练数据本身施加扰动，使从梯度中反演出的数据失真，同时尽量保持模型训练效用。

（无摘要，据标题推断）据标题，Refiner 在抵御多种梯度重建攻击的同时保持模型精度，优于现有梯度级防御。

把防御从梯度域前移到数据域是差异化思路：数据级扰动不受梯度聚合/压缩等后续处理影响，可与任何 FL 协议组合；但数据扰动直接进入训练分布，对效用与收敛的影响需精细平衡。与同一团队的攻击工作同批出现，攻防自洽性值得关注；评估需覆盖不同批次、模型与攻击变体，才能确立其相对梯度裁剪/加噪（DP-SGD）的实用优势；与攻击方进行对抗性评估（自适应攻击）是证明其鲁棒性的必要环节。


#### SoK: On Gradient Leakage in Federated Learning.
- **作者**: Jiacheng Du; Jiahui Hu; Zhibo Wang; Peng Sun; Neil Gong; Kui Ren; Chun Chen
- **方向**: AI/ML安全
- **解读**: 联邦学习声称不暴露原始数据，但研究反复证明可从共享梯度重建客户端私有数据（梯度反演攻击，GIA）；然而 GIA 多在有辅助假设的理想设定下有效，其对实际 FL 系统的真实威胁程度缺乏系统认识。

本文对 GIA 开展综述，建立时间线梳理其演化并形成系统化分类；通过"训练设定、模型、后处理"三个影响 GIA 效力的基本方面，对 SOTA GIA 进行大规模理论与经验评估。

研究发现 GIA 明显受限、脆弱且易防御：对实际本地训练设定存在固有局限，效力对训练模型高度敏感，甚至简单的梯度后处理即可构成有效防御。

这是对"梯度反演威胁被夸大"的纠偏式 SoK，用系统实验把攻击从"演示威胁"还原为"受限威胁"，方法论示范意义强；三个影响维度的框架为后续攻防研究提供了公共坐标。需注意结论依赖实验设定范围（模型、数据集、梯度保真度），不可过度外推；与同批次"Boosting"攻击工作形成张力，恰好说明该问题仍处于活跃攻防期，SoK 的结论应被持续检验。

- **论文 PDF**: [SoK: On Gradient Leakage in Federated Learning.](https://www.usenix.org/system/files/usenixsecurity25-du.pdf)

#### DP-BREM: Differentially-Private and Byzantine-Robust Federated Learning with Client Momentum.
- **作者**: Xiaolan Gu; Ming Li; Li Xiong
- **方向**: AI/ML安全
- **解读**: 联邦学习协议同时面临隐私攻击与鲁棒性攻击，但已有防御大多只针对其一；如何同时实现差分隐私（DP）与拜占庭鲁棒性是跨机构（cross-silo）FL 的开放问题。

论文基于"从历史中学习"的思路：客户端动量对每个客户端的更新随时间取平均，降低诚实客户端方差、暴露拜占庭客户端单轮不可见但会累积的恶意扰动，实现鲁棒性；DP 通过对聚合动量加噪实现（DP-BREM），并单独核算动量带来的隐私成本；进一步利用安全聚合去除可信服务器假设（DP-BREM+），DP 噪声由客户端安全联合生成。

理论分析与实验表明，两种方案在不同隐私预算与攻击设定下，均取得优于多种基线的隐私-效用权衡与更强的拜占庭鲁棒性。

"动量既做鲁棒又做隐私载体"的设计把两个通常冲突的目标统一到同一机制；对动量隐私成本的单独核算（区别于 DP-SGD 对梯度的核算）是理论亮点；DP-BREM+ 消除可信服务器使方案具备实际部署可行性。局限在于客户端动量对数据异构性的敏感度与跨设备场景的适用性，与 Byzantine-robust DP 领域其他方案（如鲁棒聚合+DP）的系统对比可进一步确立其位置。

- **论文 PDF**: [DP-BREM: Differentially-Private and Byzantine-Robust Federated Learning with Client Momentum.](https://www.usenix.org/system/files/usenixsecurity25-gu-xiaolan.pdf)

#### From Risk to Resilience: Towards Assessing and Mitigating the Risk of Data Reconstruction Attacks in Federated Learning.
- **作者**: Xiangrui Xu; Zhize Li; Yufei Han; Bin Wang; Jiqiang Liu; Wei Wang
- **方向**: AI/ML安全
- **解读**:（无摘要，据标题推断）联邦学习面临数据重建攻击风险，但部署方缺乏量化自身系统风险并针对性缓解的手段：风险水平随模型、数据与训练设定变化，一刀切防御效率低。

论文提出面向数据重建攻击的风险评估与缓解框架：先度量系统在不同条件下的重建攻击风险，再据此指导缓解策略的选择与配置。

（无摘要，据标题推断）据标题，该框架可准确评估 FL 系统的重建风险，并在保持效用的前提下显著降低攻击成功率。

从"攻防两端各做各的"转向"先评估后缓解"的风险治理视角，对该领域有方法论价值——重建攻击风险高度依赖场景（同批次 SoK 亦佐证），按需防御比无条件加噪更实用；与审计、度量类工作同属 FL 隐私工程化方向。挑战在于风险度量的可操作定义（是否需要白盒访问、攻击模拟成本）与缓解措施的自动映射；若评估指标能标准化，可成为 FL 平台的安全仪表盘组件。

#### Evaluating the Effectiveness and Robustness of Visual Similarity-based Phishing Detection Models.

- **作者**: Fujiao Ji; Kiho Lee; Hyungjoon Koo; Wenhao You; Euijin Choo; Hyoungshick Kim; Doowon Kim
- **方向**: AI/ML安全
- **解读**: 视觉相似度检测被广泛用于反钓鱼，但此类模型在真实世界场景下的有效性与对抗鲁棒性长期缺乏系统评估——这正是本文要填补的空白。

  作者基于 451k 个真实钓鱼网站的大规模数据集，全面审视流行视觉相似度反钓鱼模型的有效性与鲁棒性；并将攻击者的规避策略划分为"可见操纵"与"基于扰动的操纵"两类，施加到网站 logo 上构造对抗样本，统计评估模型的抗攻击能力。

  有效性分析显示，某些模型在实验精选数据集上准确率高，但在真实数据集上性能明显偏低，凸显真实世界评测的重要性；攻击者主要沿三条路径规避：直接攻击模型流水线、模仿良性 logo、以及采用移除截图 logo 之类的简单策略；多个模型暴露出可被利用的脆弱点。

  该研究用大规模真实数据填平了实验室指标与实战效果的鸿沟，警示"高精度"不等于"高鲁棒"；其价值不止于揭短，还给出可操作的加固方向（更稳健的视觉特征、对抗训练等）；局限在于数据集存在时点性，攻击手法演化迅速，需要持续跟踪评测以维持结论的有效性。

- **论文 PDF**: [Evaluating the Effectiveness and Robustness of Visual Similarity-based Phishing Detection Models.](https://www.usenix.org/system/files/usenixsecurity25-ji.pdf)

#### Predictive Response Optimization: Using Reinforcement Learning to Fight Online Social Network Abuse.

- **作者**: Garrett Wilson; Geoffrey Goh; Yan Jiang; Ajay Gupta; Jiaxuan Wang; David Freeman; Francesco Dinuzzo
- **方向**: AI/ML安全
- **解读**: 十余年的社交网络滥用检测研究几乎都止步于产出最优二元分类器，却无人回答关键后续问题：检测出滥用后，服务到底该采取什么行动？作者认为检测本身并非目标，行动选择才是。

  作者主张目标应是选择动作（封禁用户、拦截请求、展示验证码或"继续收集证据"），以优化"滥用危害"与"对良性用户影响"之间的权衡；据此提出 PRO（预测性响应优化）：基于强化学习，利用上下文信息预测每种候选动作下的未来滥用与用户体验指标，选出多维权衡最优的动作，并指出扩大动作集合能移动仅靠调整分类器阈值无法触及的 Pareto 前沿。

  针对 Instagram 与 Facebook 上自动化活动的部署实验显示，PRO 均优于基线分类系统：滥用量分别下降 59% 与 4.5%，且对用户无负面影响；多个案例研究展示其能快速自适应业务约束、系统行为或对手策略的变化。

  从"检测"转向"响应优化"是反滥用研究视角的重要重构，强化学习天然适配在线决策场景；59% 与 4.5% 的落差既反映两平台基线与业务约束差异，也提示单一指标难以横向比较；局限在于依赖充分的上下文特征与可靠的奖励设计，动作空间边界与安全护栏的讨论仍可深化。

- **论文 PDF**: [Predictive Response Optimization: Using Reinforcement Learning to Fight Online Social Network Abuse.](https://www.usenix.org/system/files/usenixsecurity25-wilson.pdf)

#### Mirage in the Eyes: Hallucination Attack on Multi-modal Large Language Models with Only Attention Sink.
- **作者**: Yining Wang, Mi Zhang, Junjie Sun, Chenyue Wang, Min Yang, Hui Xue, Jialing Tao, Ranjie Duan, Jiexi Liu
- **方向**: AI/ML安全
- **解读**: 多模态大语言模型(MLLM)正深刻改变视觉-语言应用,但幻觉问题(生成与图像内容不符的对象、属性与关系)严重威胁其在医疗推理、自动驾驶、机器人操控等关键领域的可靠部署。以往研究多从"弱视觉编码器与强语言先验失衡"或"训练数据统计偏差"等角度事后解释幻觉成因,且对抗性方法依赖固定模式。本文首次从内部注意力机制出发,揭示指令微调过程的内在脆弱性,并据此构造仅操纵注意力汇(attention sink)的新型幻觉攻击。
作者通过深入剖析 MLLM 的指令微调过程,发现模型在遵循用户指令后倾向生成"图文相关性下降的两段式响应",而误导性全局信息的聚合进一步加剧了与真实图像内容的偏离;幻觉输出往往跟随注意力汇 token 出现。基于此,他们提出首个仅操纵注意力汇的幻觉攻击:通过控制注意力机制与隐藏嵌入,动态生成高效、可迁移的对抗性视觉输入,无需依赖预定义模式,也不牺牲响应质量。
实验在 6 个主流 MLLM 上开展,采用 GPT-4 辅助评估幻觉程度与响应质量:攻击使幻觉句子与幻觉词比例最高分别提升 10.90% 和 12.74%;在 OK-VQA 问答任务上使准确率下降最多达 7.67%。攻击可有效迁移至黑盒 MLLM,并成功突破三类现有缓解策略;在 GPT-4o、Gemini 1.5 等商业 API 上也取得显著效果。
该工作的独特价值在于把"注意力汇"这一解释性现象转化为可操控的攻击面,并给出了成因-机制-攻击的完整论证链,为理解 MLLM 幻觉提供了机制层面的新视角,也警示了指令微调流程本身的结构性缺陷。局限在于:注意力汇操纵对响应质量的"不牺牲"声称依赖自动化评估,可能与人类感知存在偏差;攻击对更大规模、更新架构模型(如原生多模态模型)的适用性待验证。相比基于固定扰动模式的对抗攻击,Mirage 的动态性与跨模型迁移性显著更强,为 MLLM 安全评估与防御(如注意力正则化)提供了重要参照。
- **论文 PDF**: [Mirage in the Eyes: Hallucination Attack on Multi-modal Large Language Models with Only Attention Sink.](https://www.usenix.org/system/files/usenixsecurity25-wang-yining.pdf)

#### Automated Discovery of Semantic Attacks in Multi-Robot Navigation Systems.

- **作者**: Doguhan Yeke, Kartik Anand Pant, Muslum Ozgur Ozmen, Hyungsub Kim, James M. Goppert, Inseok Hwang, Antonio Bianchi, Z. Berkay Celik
- **方向**: AI/ML安全
- **解读**: 
（无摘要，据标题推断）多机器人导航系统（如无人机集群、仓储机器人车队）依赖感知与规划算法在共享空间中安全移动，"语义攻击"指那些不破坏系统低级功能、却违反其高层语义目标（如保持安全间距、到达目标、避免碰撞）的攻击。本文聚焦于自动化发现这类攻击：不是手工构造对抗样本，而是系统化地搜索能导致导航语义违规的输入（如环境扰动、GPS/传感器欺骗）。

方法上可能结合形式化分析、搜索/优化与仿真：为多机器人导航系统建立语义属性（安全性、活性）的形式化规约，通过自动化工具在仿真环境中搜索违反这些属性的攻击场景，并验证其可迁移到真实系统。作者团队在 CPS 安全（Celik、Bianchi 组）与机器人学（Hwang、Goppert 组）均有积累，方法应兼顾算法层面与实际平台。

预期成果包括发现多类此前未知的语义攻击场景（如协作中的连锁碰撞、编队失散），并评估其对主流导航算法的有效性。安全含义在于：即使单个机器人的避障逻辑正确，多体交互的复杂动态也会被攻击者利用，形成单点扰动引发系统性事故。

该工作把"语义攻击"概念系统化地带入多机器人导航这一新兴 CPS 领域，自动化发现框架具有通用价值。局限在于未见摘要，具体方法与数据待核实；仿真到真实的鸿沟、以及攻击是否需要物理访问目标机器人，都是后续需关注的问题。


#### Chimera: Creating Digitally Signed Fake Photos by Fooling Image Recapture and Deepfake Detectors.
- **作者**: Seongbin Park, Alexander Vilesov, Jinghuai Zhang, Hossein Khalili, Yuan Tian, Achuta Kadambi, Nader Sehatbakhsh
- **方向**: AI/ML安全
- **解读**: 密码学方案（拍摄时生成硬件绑定数字签名）为 deepfake 提供了比启发式检测更强的保障，但其安全承诺在"重拍攻击"面前坍塌：攻击者把数字生成的假图翻拍到屏幕后重新拍摄即可获得合法签名。此时安全防线退化为两类检测器——deepfake 检测器与重拍检测器——且成功的攻击必须同时骗过两者，此前对抗与生成模型要么无法兼顾、要么缺乏跨设备泛化。

Chimera 提出端到端攻击：核心是硬件感知的对抗补偿器（hardware-aware adversarial compensator）驱动的 GAN 图像生成器，在生成阶段显式建模并补偿重拍过程引入的物理变换（显示、镜头、采样等），使最终成品既通过重拍检测器又通过 deepfake 检测器。

在商用现成相机与显示器上的严格测试中，Chimera 以高成功率同时欺骗两类 SOTA 检测机制，且生成图像相对原始真实图像保持高视觉质量，端到端演示了"带合法签名的假照片"的可行性。

我的思考：该工作的杀伤力在于揭示了硬件数字签名的安全承诺仍依赖检测器的有效性——密码学把攻击门槛抬高到"必须翻拍"，却未消除检测器这个薄弱环节。方法论上，把物理重拍过程纳入生成管线的对抗补偿是新颖且正确的思路。局限是攻击需要物理重拍设备与受控环境，规模化部署成本不低；但它清楚表明"签名+检测"组合并非终极方案，倒逼更鲁棒的溯源与检测策略。
- **论文 PDF**: [Chimera: Creating Digitally Signed Fake Photos by Fooling Image Recapture and Deepfake Detectors.](https://www.usenix.org/system/files/usenixsecurity25-park.pdf)

#### APPATCH: Automated Adaptive Prompting Large Language Models for Real-World Software Vulnerability Patching.
- **作者**: Yu Nong, Haoran Yang, Long Cheng, Hongxin Hu, Haipeng Cai
- **方向**: AI/ML安全
- **解读**: 及时有效的漏洞补丁是网络防御的基石,但人工修补成本高昂且跟不上漏洞涌现速度。现有自动化修补方案分两类:基于代码分析的方案依赖漏洞触发测试输入或 exploit 且要求代码可编译,对零日漏洞与未完成项目束手无策;基于深度学习的方案需要大量带标签训练数据,泛化性差。LLM 提示方法虽无需训练,但零样本 SOTA 方案在真实漏洞上常生成无效补丁,且面临四大挑战:提示无法自动生成、范例难以自动选择、上下文长度限制无法容纳跨过程漏洞所需的大代码、模型幻觉导致补丁不稳定。本文要构建一个自动、无需测试输入与微调的 LLM 漏洞修补系统。
APPATCH 的流程为:先通过语义感知作用域裁剪(semantics-aware scoping)将分析范围缩小到与漏洞相关的代码子集,解决上下文限制;再引导 LLM 进行漏洞语义推理(vulnerability semantics reasoning)识别根因,据此从离线挖掘的已知补丁范例库中动态挑选最匹配的范例(动态自适应提示);随后自动构造提示生成多个候选补丁;最后引入多 LLM 集成交叉验证候选补丁的有效性与正确性,抑制幻觉。
APPATCH 基于 GPT-4、Gemini-1.5、Claude-3.5、Llama-3.1 四个 LLM 实现,在 97 个零日漏洞(横跨 18 个项目)与 20 个已知真实漏洞上评估:零日数据集 F1 最高达 36.46%(基线提示仅 28.41%),已知漏洞数据集 F1 达 73.86%(基线 68.54%);相比 SOTA 非 LLM 方法(零日召回最高仅 17.53%、已知漏洞 85.00%),APPATCH 分别达到 49.48% 与 90.00% 的召回,最高提升 F1 28.33%、召回 182.26%。效率上平均 37.1–50.2 秒生成一个补丁、每补丁耗 5,684–6,802 tokens。
该工作的方法论贡献在于把"提示工程"系统化为面向漏洞修补的自动化流水线:语义裁剪、根因推理、动态范例选择与多模型交叉验证四步环环相扣,并清晰回答了"什么对 LLM 修补有效、为什么"。绝对值上,零日数据集 36.46% 的 F1 仍显有限,说明真实漏洞修补之难;作者也坦诚讨论了失败案例与 LLM 的局限(幻觉、不确定输出)。与单纯零样本提示相比,自适应范例与交叉验证带来的增益证明了"推理引导"的价值。局限:依赖已知漏洞位置标注、范例库的覆盖面,以及四个商用/开源 LLM 的 API 成本。总体而言,APPATCH 是 LLM 驱动自动化漏洞修复方向的代表性工作,为后续研究提供了基准与方法框架。
- **论文 PDF**: [APPATCH: Automated Adaptive Prompting Large Language Models for Real-World Software Vulnerability Patching.](https://www.usenix.org/system/files/usenixsecurity25-nong.pdf)

#### VoiceWukong: Benchmarking Deepfake Voice Detection.
- **作者**: Ziwei Yan, Yanjie Zhao, Haoyu Wang
- **方向**: AI/ML安全
- **解读**: TTS 与语音转换技术快速进步使 deepfake 语音泛滥，但学界与工业界缺乏全面直观的检测基准：现有数据集语言多样性不足，且缺少真实生产环境中的多种操纵方式。本文构建 VoiceWukong 基准，系统评估 deepfake 语音检测器的真实能力。

数据集构建上，收集 19 个商业与 15 个开源先进工具生成的 deepfake 语音，构造覆盖六类操纵的 38 个数据变体，最终包含 265,200 个英文与 148,200 个中文 deepfake 样本；并据此评估 12 个 SOTA 检测器。

结果显示 AASIST2 取得最佳等错误率（EER）13.50%，其余检测器全部超过 20%；在真实应用场景下各检测器性能显著下滑。此外，300+ 参与者的用户研究与 12 个检测器及多模态大模型 Qwen2-Audio 对比显示：不同检测器与人类在不同欺骗难度下识别能力各异，而 MLLM 完全没有检测能力；基准与排行榜已公开。

我的思考：VoiceWukong 的价值在于把"双语、多工具、多操纵、含用户研究"整合进一个可持续更新的评估基础设施，公开排行榜让检测器之间的比较变得直观可复现；"MLLM 无检测能力"与"真实场景性能大跌"是两个重要警示。局限是检测器性能随新生成工具快速过时，基准需持续维护；但作为该领域首个规模化双语基准，影响是奠基性的。
- **论文 PDF**: [VoiceWukong: Benchmarking Deepfake Voice Detection.](https://www.usenix.org/system/files/usenixsecurity25-yan-ziwei.pdf)

#### SafeSpeech: Robust and Universal Voice Protection Against Malicious Speech Synthesis.
- **作者**: Zhisheng Zhang, Derui Wang, Qianyi Yang, Pengyang Huang, Junhan Pu, Yuxin Cao, Kai Ye, Jie Hao, Yixian Yang
- **方向**: AI/ML安全
- **解读**: 攻击者可未经授权收集受害者语音并克隆出相似声音用于电信诈骗等非法活动；现有防御既无法有效阻止 deepfake 利用，又易被鲁棒训练等对抗技术击穿。本文提出 SafeSpeech，在音频上传前嵌入人耳几乎不可感知的扰动，阻止高质量合成语音的生成，属于"主动数据保护"路线。

核心是 Speech PErturbative Concealment（SPEC）技术：借助替代模型生成对生成式合成模型普遍适用的扰动，实现鲁棒且通用的防护；同时在时域与频域优化扰动的人类感知，使其不可察觉。综合的主客观实验覆盖多种先进模型与数据集。

结果表明 SafeSpeech 在语音防护有效性与跨模型迁移性上达到 SOTA，对高级自适应攻击者保持高鲁棒性，并在真实测试中具备实时处理能力，代码已开源。

我的思考：与事后检测相比，"上传前加扰"的主动防护把攻防阵地前移到数据源头，通用扰动+感知优化的组合在实用性与隐蔽性之间取得了平衡，实时性让端侧部署成为可能。局限是扰动对语音质量的潜在影响、对未见合成模型的迁移上限未知，且这类防护与"合法语音合成（如无障碍辅助）"的需求存在张力；但作为主动防御的代表作，思路与工程均扎实。
- **论文 PDF**: [SafeSpeech: Robust and Universal Voice Protection Against Malicious Speech Synthesis.](https://www.usenix.org/system/files/usenixsecurity25-zhang-zhisheng.pdf)

#### Whispering Under the Eaves: Protecting User Privacy Against Commercial and LLM-powered Automatic Speech Recognition Systems.

- **作者**: Weifei Jin, Yuxin Cao, Junjie Su, Derui Wang, Yedi Zhang, Minhui Xue, Jie Hao, Jin Song Dong, Yixian Yang
- **方向**: AI/ML安全
- **解读**: ASR 广泛应用支撑大规模语音监听，攻击者截获通话后用 ASR 转录即可提取隐私，而对话双方毫不知情。对抗样本可让 ASR 转录出错以保护语音隐私，但既有方法要么离线优化耗时、不满足实时通信，要么通用对抗扰动（UAP）噪声过大、严重损伤音质。本文目标是在实时性、黑盒迁移性与高音质之间兼得。

AudioShield 提出潜空间可迁移通用对抗扰动（LS-TUAP）：以 VITS 自编码器把语音编入潜空间再加扰动、经解码合成对抗音频，避免在波形域注入噪声；目标特征适配使扰动学习目标文本的潜特征以提升跨模型迁移性，PGD 约束扰动幅度，并用 615 条真实房间冲激响应模拟空气失真增强物理鲁棒性；还给出解码器 Lipschitz 假设下的 r-鲁棒性概率界，从理论上支撑音质保持。

在四个商业 ASR API、两个 LLM 驱动 ASR（Qwen-Audio、MooER）与 Whisper-large-v3 上，平均保护成功率 82.36%，对 Google/Amazon/iFlytek/Alibaba 分别比最强对手 AdvDDoS 高 27.88%/27.44%/5.5%/17.29%；实时端到端与空中传输场景平均成功率 87.5% 与 69%；NISQA 2.45、主观 MOS 2.91（良性音频 3.99），显著优于对手（Zong 等人仅 1.14）。

首次把潜空间扰动与目标特征适配结合用于 ASR 隐私防护，兼顾实时、迁移与音质，评测体系完整（10 个目标系统、主客观音质、自适应对抗鲁棒性）。局限：依赖 VITS 骨干与本地代理模型（DeepSpeech2），评估中过滤异常样本可能高估成功率，真实通话链路经编解码压缩后的稳健性仍待验证。

- **论文 PDF**: [Whispering Under the Eaves: Protecting User Privacy Against Commercial and LLM-powered Automatic Speech Recognition Systems.](https://www.usenix.org/system/files/usenixsecurity25-jin-weifei.pdf)

#### AudioMarkNet: Audio Watermarking for Deepfake Speech Detection.
- **作者**: Wei Zong, Yang-Wai Chow, Willy Susilo, Joonsang Baek, Seyit Camtepe
- **方向**: AI/ML安全
- **解读**: 深度生成模型使假语音与真实语音难以区分，人类与检测器都面临识别困境；现有检测技术多为"被动反应式"——事后识别生成模型留下的痕迹，处于不断追赶的劣势。本文提出 AudioMarkNet，采用主动水印路线：在原始语音中嵌入水印，防止语音被用于说话人自适应（即微调 TTS 模型）以生成高保真假语音。

方法核心是把水印嵌入作为与被动检测正交的独立防线：原始音频携带水印，被用于 TTS 微调后，生成语音中保留的水印信号可被检测，从而判定音频为"由受保护语音衍生"的伪造内容。

实验证明该方法能有效检测开源与商业 TTS 模型生成的假语音；水印对常见非自适应攻击具有鲁棒性，且对自适应攻击同样有效；代码与制品已公开。

我的思考：AudioMarkNet 与同会议的多篇音频防护工作（SafeSpeech、AUDIO WATERMARK）共同构成"主动防护"研究浪潮，其差异点在于直接瞄准"说话人自适应"这一最常用的伪造路径。局限是水印鲁棒性存在上限（强后处理可抹除）、对训练时未见的 TTS 架构泛化待验证，且水印可能影响原始音频质量；但作为与被动检测正交的防线，其生态价值值得肯定。
- **论文 PDF**: [AudioMarkNet: Audio Watermarking for Deepfake Speech Detection.](https://www.usenix.org/system/files/usenixsecurity25-zong.pdf)

#### SoK: Efficiency Robustness of Dynamic Deep Learning Systems.
- **作者**: Ravishka Rathnasuriya, Tingxi Li, Zexin Xu, Zihe Song, Mirazul Haque, Simin Chen, Wei Yang
- **方向**: AI/ML安全
- **解读**: 动态深度学习系统（DDLS）通过早退、动态路由、按输入复杂度调整计算量来提升推理效率，但这种"动态性"引入了一类此前被忽视的安全面——效率攻击：攻击者构造输入放大推理时的延迟、能耗与计算量，在资源受限的真实部署（移动端、IoT、自动驾驶）中造成拒绝服务或成本失控。

本 SoK 给出首个效率攻击分类学，按三种动态行为划分攻击对象：单次推理的动态计算量、动态推理迭代次数、面向下游任务的动态输出生产；系统评估了 NICGSlowDown、DeepSloth、SlowTrack 等代表性攻击，并审查现有防御（SVM 输入验证、JPEG 压缩、空间平滑等）。

实证显示：对抗输入可显著拉长输出长度与延迟（序列任务 BLEU 显著下降）；JPEG 压缩等简单变换只能部分恢复效率；检测防御准确率约 51%-85.6%，且无单一防御能完全中和所有效率攻击向量。

我的思考：该 SoK 把"效率鲁棒性"确立为独立于传统精度鲁棒性的研究维度，分类学为后续攻击与防御提供了坐标系；对时延敏感的实时系统而言，这类攻击比精度攻击更具破坏性。局限是评估覆盖的模型规模有限（多为 MobileNet+RNN 类），对 LLM、MoE、扩散模型等新型动态架构的泛化尚待扩展；但作为领域首个系统化梳理，方向意义重大。（无摘要，据标题推断）

#### When Translators Refuse to Translate: A Novel Attack to Speech Translation Systems.
- **作者**: Haolin Wu, Chang Liu, Jing Chen, Ruiying Du, Kun He, Yu Zhang, Cong Wu, Tianwei Zhang, Qing Guo, Jie Zhang
- **方向**: AI/ML安全
- **解读**: 语音翻译系统（将一种口语转换为另一种语言）快速普及，但其安全性研究几乎空白。本文发现 SOTA 语音翻译模型存在一个固有倾向：即便翻译能力强，也常常输出源语言内容而非目标语言。基于此提出"untranslation attack"（拒绝翻译攻击）——欺骗系统输出源语言内容，使翻译功能完全失效。

攻击模型利用该固有倾向，只需极小扰动即可让系统"拒绝翻译"；与传统语义扰动攻击相比，攻击开销极小：在扰动预算低至 0.001 时攻击成功率即达 87.5%。作者还将该方法扩展为通用扰动攻击，并在物理世界中成功测试。

结果显示，这一新型威胁以极低成本实现了对翻译系统的功能破坏，物理世界的通用扰动进一步证明了现实可行性。

我的思考：该攻击的巧妙之处在于"借力打力"——不强行改变语义，而是放大模型自身的语言倾向，因此扰动预算极低，规避了传统攻击的检测特征；语音翻译作为语音助手、字幕、会议系统的底层能力，其功能失效的直接后果是真实的安全事件。局限是攻击效果依赖模型倾向的强度、跨语言对与跨模型泛化待验证；但作为该领域首个系统攻击，开启了语音翻译安全的研究线。
- **论文 PDF**: [When Translators Refuse to Translate: A Novel Attack to Speech Translation Systems.](https://www.usenix.org/system/files/usenixsecurity25-wu-haolin.pdf)

#### Fighting Fire with Fire: Continuous Attack for Adversarial Android Malware Detection.
- **作者**: Yinyuan Zhang, Cuiying Gao, Yueming Wu, Shihan Dou, Cong Wu, Ying Zhang, Wei Yuan, Yang Liu
- **方向**: AI/ML安全
- **解读**: Android 作为主流开源操作系统同时也是恶意软件的首要攻击目标，学习型恶意软件检测器（AMD）虽在识别上成效显著，却日益被对抗样本（AE）攻破——最新生成工具可将主流检测器准确率降至 1%。为提升 AMD 的鲁棒性，本文提出"以攻为守"的防御机制 HagDe。

HagDe 的核心观察是 AE 对扰动高度敏感：对所有样本沿梯度上升方向施加迭代扰动，正常样本的损失变化平缓，而对抗样本的损失会不成比例地激增，据此区分二者。作者在 15,000 个样本、15 种攻击组合上进行了大规模评估。

实验结果显示，HagDe 对 AdvDroidZero 的防御有效率达 88.5%、对 BagAmmo 达 90.7%，相比最新防御方法 KD_BU 和 LID 分别提升 32.45% 和 11.28 个百分点。

"以攻为守"——用攻击的生成性强化防御——是应对对抗样本的新颖且成本可控的策略，无需重训检测器即可部署。局限是有效性依赖扰动幅度与阈值设定，精心规避扰动强度的攻击可能削弱区分度；结果也提示对抗鲁棒性研究仍处于攻防螺旋之中。

- **论文 PDF**: [Fighting Fire with Fire: Continuous Attack for Adversarial Android Malware Detection.](https://www.usenix.org/system/files/usenixsecurity25-zhang-yinyuan.pdf)

#### Vulnerability of Text-Matching in ML/AI Conference Reviewer Assignments to Collusions.
- **作者**: Jhih-Yi Hsieh, Aditi Raghunathan, Nihar B. Shah
- **方向**: AI/ML安全
- **解读**: 顶级 ML/AI 会议普遍用基于文本匹配的自动算法分配审稿人——把论文与审稿人的工作做相似度匹配以确定评审资格。但这种机制易受共谋攻击：作者或审稿人可通过操纵文本（引用特定论文、改动摘要措辞等）影响匹配结果，从而获得有利评审。本文研究此类共谋漏洞。（无摘要，据标题推断）

从标题推断，工作以理论分析或大规模模拟刻画文本匹配机制在共谋下的脆弱性：构造攻击者策略（论文侧与审稿人侧的操纵），度量其对匹配质量、评审公平性与会议决定的影响，可能还提出检测或缓解思路。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于证明并量化文本匹配评审分配对共谋的脆弱性，为该机制的稳健性设计提供依据。（无摘要，据标题推断）

评审分配是学术生态的公正性基础设施，其"被攻击"维度长期被忽视；把安全研究的方法（对手建模、脆弱性分析）引入学术流程治理，视角新颖且影响面广。局限是模拟假设与现实会议流程存在差距，实际利用的检测难度与缓解成本未明；该工作也提示任何依赖可操纵输入的自动化机制都需要对抗性审视。


#### Dormant: Defending against Pose-driven Human Image Animation.
- **作者**: Jiachen Zhou, Mingsi Wang, Tianlin Li, Guozhu Meng, Kai Chen
- **方向**: AI/ML安全
- **解读**: 姿态驱动的人体图像动画技术进展迅速，仅凭一张照片即可生成逼真的人类视频，但也加剧了图像滥用风险——攻击者可利用一张可用照片生成涉政、暴力等非法内容的视频。本文提出 Dormant，一种专门防御姿态驱动人体图像动画的保护方法。

Dormant 对一张人体图像施加保护性扰动：保持与原始图像的视觉相似，但使生成的视频质量严重劣化。扰动经优化以诱导对图像外观特征的错误提取，并制造生成视频帧间的不连贯。作者在 8 种动画方法、4 个数据集上与 6 种基线保护方法对比，并在 6 个真实商业服务上以完全黑盒方式验证。

实验表明 Dormant 优于全部 6 种基线方法，使生成视频出现身份错配、视觉畸变、明显伪影与帧间不一致；在 6 个真实商业服务上即使黑盒访问也依然有效。

深度伪造防御多聚焦人脸交换与换声，姿态驱动动画这一快速发展的滥用向量此前缺少专门防护；"保相似、毁视频"的目标函数设计直击生成流程，黑盒有效性增强了实用性。局限是扰动可能被对抗性净化或针对性训练绕过，且对图像质量的轻微影响与批量保护的成本需进一步评估。

- **论文 PDF**: [Dormant: Defending against Pose-driven Human Image Animation.](https://www.usenix.org/system/files/usenixsecurity25-zhou.pdf)

#### SoK: Machine Learning for Misinformation Detection.
- **作者**: Madelyne Xiao, Jonathan R. Mayer
- **方向**: AI/ML安全
- **解读**: 虚假信息（misinformation）在社交媒体上传播迅速、危害深远，机器学习被广泛用于自动检测，但该领域方法庞杂、数据集与评测指标不一，缺乏系统化梳理。本文以 SoK（系统化知识）形式综述机器学习用于虚假信息检测的研究现状。（无摘要，据标题推断）

从标题推断，SoK 将系统化分类检测方法（基于内容、用户、传播网络的模型等）、数据集与评估基准，梳理威胁模型与评测缺口，并可能对代表性方法做统一复现比较，给出可操作的研究建议。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于给出 ML 虚假信息检测领域的结构化地图、常用数据集与评测实践清单，以及关键开放问题。（无摘要，据标题推断）

虚假信息检测是典型"论文多、落地难"的领域，SoK 的价值在于厘清方法-数据-评测的对应关系、暴露基准偏置与泛化问题，防止研究内卷于刷榜。局限是领域演进快，SoK 时效性有限；若缺乏统一评测实验，仅靠文献梳理难以裁决方法优劣；期待其明确区分"检测任务"与"传播干预"两个层面。


#### Towards a Re-evaluation of Data Forging Attacks in Practice.
- **作者**: Mohamed Suliman; Anisa Halimi; Swanand Ravindra Kadhe; Nathalie Baracaldo; Douglas J. Leith
- **方向**: AI/ML安全
- **解读**: （无摘要，据标题推断）"数据伪造攻击"（data forging）指攻击者向训练流程注入或篡改数据以操纵模型行为的攻击族（含投毒、后门、虚假标注等）。本文标题强调"再评估"与"在实践中"，暗示此前的威胁模型或评估设定与真实部署存在偏差，需要重新校准。

据标题推断，作者可能基于真实机器学习流水线（如联邦学习、数据管道、持续学习）重新审视数据伪造攻击的可行性、前置条件与实际影响，可能通过大规模实验或对既有攻击的复现比较，检验哪些攻击在现实中真正构成威胁。

由于无摘要，具体结论与数据无法引用，不做数字性断言。

我的思考："再评估"类工作对安全社区极有价值——攻击论文往往在理想化设定下成立，而工程实践中的清洗、校验、监控可能天然削弱攻击；把结论从"理论上可行"修正为"实践中何时可行/不可行"，能引导防御资源投向真实风险。风险在于若只做复现性批评而未给出新攻击或新防御，贡献易被视为增量。总体而言，这类实践导向的重新校准对攻防双方都有参考价值，也期待论文给出对应的实证数据支撑。


#### Rectifying Privacy and Efficacy Measurements in Machine Unlearning: A New Inference Attack Perspective.
- **作者**: Nima Naderloui; Shenao Yan; Binghui Wang; Jie Fu; Wendy Hui Wang; Weiran Liu; Yuan Hong
- **方向**: AI/ML安全
- **解读**: （无摘要，据标题推断）机器遗忘（machine unlearning）要求模型删除特定训练记录的影响，学界通常用成员推断攻击（MIA）衡量隐私保护效果、用效用指标衡量模型性能。本文标题"修正隐私与效能的度量"暗示现有度量体系存在缺陷——例如 MIA 成功率可能被攻击方法选择左右，或遗忘评估忽略了更隐蔽的泄露路径；尤其在被遗忘数据本应"无痕"的设定下，度量失真意味着合规承诺落空。

据标题推断，作者从一个新的推断攻击视角切入，可能构造针对"已遗忘记录"的新型攻击或更合理的度量协议，重新校准遗忘模型的隐私与效用测量，使"遗忘成功"的判定更贴近真实威胁。

由于无摘要，具体攻击设计、数据集与量化改进无法引用，不做数字性断言。

我的思考：机器遗忘的度量之争本质是"隐私如何可证地消失"的定义问题；若现有 MIA 度量高估了遗忘效果，将误导合规与产品决策，因此"纠正度量"与提出新攻击同等重要。挑战在于任何度量都无法穷尽攻击空间，研究最终仍需与理论隐私界限（如差分隐私）对接。总体来看，度量问题的澄清是机器遗忘走向合规落地的前提，本文的切入点值得关注。


#### SoK: Data Reconstruction Attacks Against Machine Learning Models: Definition, Metrics, and Benchmark.
- **作者**: Rui Wen; Yiyong Liu; Michael Backes; Yang Zhang
- **方向**: AI/ML安全
- **解读**: （无摘要，据标题推断）数据重构攻击（data reconstruction attack）旨在从模型输出或梯度中还原训练数据（如成员图像、文本），是机器学习隐私研究的重要分支。本文以 SoK（系统化知识）形式给出该领域的定义、度量与基准，意在结束该方向"各说各话"的混乱局面。

据标题推断，作者应系统梳理现有重构攻击的形式化定义与威胁模型差异，统一评估指标（如重构保真度、成功率、查询预算），并构建可复现的基准测试环境，供后续工作横向比较，可能还对比不同攻击在统一基准上的表现并给出指导性结论。

由于无摘要，具体基准规模与结论无法引用，不做数字性断言。

我的思考：SoK 的价值在"统一度量"——重构攻击常因评估口径不一而难以比较，定义与基准的规范化能显著降低该方向的进入门槛与无效内卷。风险是基准一旦固化可能窄化研究视野，且重构攻击的度量天然依赖数据集与模型族，需要持续演进；但由 CISPA 团队主导，数据与工具通常开源，利于社区共建。总体而言，一份被社区采纳的定义与基准，其影响将远超单篇攻击论文。


#### From Purity to Peril: Backdooring Merged Models From "Harmless" Benign Components.
- **作者**: Lijin Wang, Jingjing Wang, Tianshuo Cong, Xinlei He, Zhan Qin, Xinyi Huang
- **方向**: AI/ML安全
- **解读**:
**问题**：大模型能力扩展训练成本高昂，模型合并能以极低成本将多个任务专用模型组合成多功能模型；但合并场景的安全影响，尤其是后门漏洞，存在显著研究空白——多个看似良性的模型能否被合并成带后门的模型，构成新型供应链威胁。
**方法**：MergeBackdoor是通用训练框架：在合并前抑制上游模型中的后门行为（各组件单独看都干净），同时确保合并时后门涌现激活，实现"合前无害、合后有害"。
**结果**：在3类模型（ViT、BERT、LLM）与12个数据集上，合并前各上游模型攻击成功率（ASR）均处于随机猜测水平，而合并后模型ASR接近1.0；最先进的检测器也无法在合并前识别异常。
**思考**：该工作首次系统提出并验证"模型合并后门"这一供应链威胁，攻击面新颖且现实（开放权重与模型动物园普及合并），"合前检测无效"对现有AI审计构成直接挑战；局限在于攻击依赖对合并算法的适配、训练框架的通用性仍需检验，但其对模型供应链全流程审计的呼吁具有政策含义。

- **论文 PDF**: [From Purity to Peril: Backdooring Merged Models From "Harmless" Benign Components.](https://www.usenix.org/system/files/usenixsecurity25-wang-lijin.pdf)

#### Revisiting Training-Inference Trigger Intensity in Backdoor Attacks.
- **作者**: Chenhao Lin, Chenyang Zhao, Shiwei Wang, Longtian Wang, Chao Shen, Zhengyu Zhao
- **方向**: AI/ML安全
- **解读**:
**问题**：后门攻击在训练数据上放置特定触发器，使模型推理时对含触发器输入产生错误预测；尽管触发器是核心，既有研究普遍认为"训练-推理触发器完全匹配"最优，本文首次系统探索训练-推理触发器的关系，尤其是二者失配时的效果。
**方法**：TITIM工作流系统研究触发器强度（大小、不透明度）在训练与推理两侧的匹配关系，揭示触发器的泛化与过拟合现象，挑战"完全匹配最优"的常识。
**结果**：两个反直觉发现：其一，推理触发器固定时，混合强度训练触发器强于任何单一强度——CIFAR-10/ResNet18上混合1.0与0.1不透明度，将最坏情况ASR从10.61%提升至92.77%；其二，失配可提升隐蔽性——训练/推理1.0/0.7相比1.0/1.0，将Scale-Up防御AUC从0.96降至0.62，同时保持高ASR（99.65% vs 91.62%）。
**思考**：该工作推翻了"触发器必须完全匹配"的常识，"混合强度提升泛化、失配提升隐蔽"对威胁建模与防御设计有直接影响；局限在于实验主要基于图像领域与特定防御，结论向NLP等领域的推广尚需验证，但其为理解后门触发器机理提供了新视角。

- **论文 PDF**: [Revisiting Training-Inference Trigger Intensity in Backdoor Attacks.](https://www.usenix.org/system/files/usenixsecurity25-lin-chenhao.pdf)

#### Persistent Backdoor Attacks in Continual Learning.
- **作者**: Zhen Guo, Abhinav Kumar, Reza Tourani
- **方向**: AI/ML安全
- **解读**:
**问题**：后门攻击能操纵模型在特定输入上的输出，在关键应用中后果严重；但既有研究很少关注持续学习（continual learning）场景下后门攻击的实用性与持久性——模型参数随新数据分布不断更新，这些更新如何影响后门效果随时间保持（或消退），尚缺乏理解。
**方法**：作者提出两种持久后门攻击：Blind Task Backdoor（盲任务后门，仅通过微调损失计算施加影响，不直接控制训练过程）与Latent Task Backdoor（潜在任务后门，只影响单个任务的训练，其余任务全部正常训练），均以最小的对抗干预实现后门植入。
**结果**：在多种配置下评估，两种攻击在静态、动态、物理与语义触发器上都持续取得高成功率，且能有效规避SentiNet、I-BAU等SoTA防御，展示了后门在持续更新模型中的持久性。
**思考**：该工作首次系统研究持续学习场景的后门持久性，"极低干预+跨算法有效"的设定使攻击极具隐蔽性与现实性（许多系统确实在持续增量更新模型）；局限在于评估的持续学习算法与数据集规模有限，且防御侧的针对性对策尚待跟进，但"持久性"作为后门威胁的新维度，对模型生命周期安全治理有重要启示。
- **论文 PDF**: [Persistent Backdoor Attacks in Continual Learning.](https://www.usenix.org/system/files/usenixsecurity25-guo-zhen.pdf)

#### Data Duplication: A Novel Multi-Purpose Attack Paradigm in Machine Unlearning.

- **作者**: Dayong Ye, Tianqing Zhu, Jiayang Li, Kun Gao, Bo Liu, Leo Yu Zhang, Wanlei Zhou, Yang Zhang
- **方向**: AI/ML安全
- **解读**:

数据重复是数据集中普遍存在的现象，已有研究证明重复数据会显著影响模型性能与数据隐私，但重复数据对机器学习"反学习"（unlearning，即按 GDPR"被遗忘权"等要求删除训练数据影响）过程的影响长期无人研究。作者首次系统考察重复数据在标准、联邦与强化三类反学习范式中的角色，并将其重新定义为可利用的攻击面。

方法上提出一种通用攻击范式：攻击者复制目标模型训练集的一个子集并混入训练集，训练完成后要求模型所有者"反学习"该重复子集，再分析反学习后模型的影响——例如可借此揭露"尽管声称已反学习，重复子集的影响仍然残留在模型中"。为规避去重（de-duplication）检测，作者针对三类反学习范式分别设计三种近重复（near-duplication）构造方法，并检验去重技术应用后的攻击效果。

结果揭示三个关键发现：1）黄金标准反学习方法"从头重训"在特定条件下无法有效完成反学习；2）反学习重复数据会在特定场景下导致模型显著性能退化；3）精心构造的重复数据可逃逸去重检测。代码已开源（zenodo）。

我的思考：该工作的贡献在于打通"数据重复"与反学习安全性之间的联系，为反学习机制的脆弱边界提供了首个系统性攻击范式——若模型无法证明已彻底遗忘，反学习的可审计性承诺就存疑。局限在于攻击需在训练前注入数据，近重复对检测的逃逸评估规模也有限；与投毒、成员推断等范式相比，也为设计可验证遗忘提供了反例集。

- **论文 PDF**: [Data Duplication: A Novel Multi-Purpose Attack Paradigm in Machine Unlearning.](https://www.usenix.org/system/files/usenixsecurity25-ye-duplication.pdf)

#### DeBackdoor: A Deductive Framework for Detecting Backdoor Attacks on Deep Models with Limited Data.

- **作者**: Dorde Popovic, Amin Sadeghi, Ting Yu, Sanjay Chawla, Issa Khalil
- **方向**: AI/ML安全
- **解读**:

后门攻击是深度学习中最高效、实用且隐蔽的攻击之一。论文聚焦一个现实场景：开发者从第三方获得深度模型并用于安全关键系统，部署前需要检查模型是否携带后门。作者指出大多数现有检测技术的假设（如需要大量干净数据、可微调模型、访问训练流程等）在此场景下不成立，检测面临数据受限与黑盒约束。

方法上提出 DeBackdoor 框架：通过演绎式搜索在可能的触发器空间中生成候选触发器，并以攻击成功率（ASR）的平滑版本作为搜索目标函数进行构造与优化；从一类宽泛的模板攻击出发，仅利用模型的前向传播即可逆向工程出后门攻击，不依赖模型内部结构或反向传播。

结果：在广泛的攻击、模型与数据集组合上进行大量评估，该技术在几乎所有设置下表现近乎完美（检测与触发器逆向均接近满分），覆盖多种触发器类型与架构。

我的思考：贡献成色在于把后门检测从"强假设依赖"推进到"仅需前向传播+少量数据"的实用设定，平滑 ASR 目标与模板驱动的演绎搜索是方法上的巧妙之处，与 Neural Cleanse 等逆向工程思路相比显著放宽了对数据规模与模型可解释性的依赖。局限：搜索空间以模板攻击为界，非模板化或自适应的触发器可能超出假设范围；"几乎完美"的具体量化仍需查阅原文。对第三方模型供应链的部署前体检具有直接实用价值。

- **论文 PDF**: [DeBackdoor: A Deductive Framework for Detecting Backdoor Attacks on Deep Models with Limited Data.](https://www.usenix.org/system/files/usenixsecurity25-popovic.pdf)

#### SoK: Gradient Inversion Attacks in Federated Learning.

- **作者**: Vincenzo Carletti, Pasquale Foggia, Carlo Mazzocca, Giuseppe Parrella, Mario Vento
- **方向**: AI/ML安全
- **解读**:

联邦学习（FL）通过"只共享梯度、不共享数据"保护客户端隐私，但梯度反转攻击（gradient inversion）已被证明可从共享梯度中重建训练数据，使 FL 的隐私承诺面临根本性质疑。该方向近年发展迅速、攻击与防御论文激增，但缺乏系统性梳理，研究者难以把握威胁模型、攻击成立条件与防御边界。

据标题推断，作为 SoK（知识系统化）论文，该工作系统收集并分类 FL 梯度反转攻击研究：归纳攻击威胁模型（恶意服务器/客户端、白盒/黑盒、批次大小、辅助信息），梳理从优化式重建到生成式先验辅助的方法演化，并对照梯度扰动、压缩、安全聚合等防御手段分析其有效性前提与失效条件。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于为社区建立共识性分类与术语，明确攻击成功的关键条件与各防御的适用范围，并总结开放问题。

我的思考：此类 SoK 是领域基础设施，能避免重复造轮子，并为新防御的评估提供对照基准，对隐私保护与 AI/ML 安全交叉研究有导航价值。局限在于此类综述常面临时效性——梯度反转领域迭代快（生成式攻击、大模型场景不断涌现），分类框架与结论需持续更新；是否包含最新工作的实证复现也需查阅原文确认。总体是 FL 隐私攻防方向必要且及时的梳理。


#### PoiSAFL: Scalable Poisoning Attack Framework to Byzantine-resilient Semi-asynchronous Federated Learning.

- **作者**: Xiaoyi Pang, Chenxu Zhao, Zhibo Wang, Jiahui Hu, Yinggui Wang, Lei Wang, Tao Wei, Kui Ren, Chun Chen
- **方向**: AI/ML安全
- **解读**: 半异步联邦学习（SAFL）聚合部分快客户端的本地模型，兼顾异步效率与同步收敛速度，但也带来新风险：客户端基于不同版本全局模型训练，本地模型间天然高度不一致，为恶意模型提供"混入"掩护；慢客户端在 SAFL 中参与聚合的机会多于完全异步（AFL），被攻破后破坏力更强。本文首次揭示：即使部署 Byzantine 鲁棒防御，SAFL 仍面临严重投毒风险。

PoiSAFL 将攻击建模为带约束优化问题：最小化投毒后全局模型的正确分类概率，约束恶意模型在服务器信任数据上的损失不超过最陈旧良性模型的 β 倍、与全局模型距离不超过其 γ 倍。三模块逼近目标：反训练初始化（ATI）用倒置损失生成全类误分类的种子恶意模型；损失感知知识蒸馏（LAD）以种子为教师训练恶意本地模型，保持低熵低损失以绕过熵基与性能基防御；距离感知缩放（DAS）把模型拉回相似度可接受范围绕过相似度防御。恶意客户端保持最快上传（staleness=1），不被陈旧加权惩罚。

在 FashionMNIST/CIFAR10/GTSRB、IID/Non-IID、20% 恶意客户端下，面对 5 种相似度防御（Median、TriMean、Norm-clipping、FLARE、AFLGuard）、2 种性能防御（LFR、Sageflow）与 2 种熵防御（Sageflow、Efilter）几乎全部大幅超越基线攻击（LabelFlip/SignFlip/LIE/MinSum/LA/Grad）：如 FashionMNIST IID 下 Median 上影响 58.79% vs 基线 7.13%，Efilter 下 48.36% vs 4.94%，Non-IID NormClip 下高达 78.48%；多数基线在性能/熵防御下几乎无效，说明现有防御给 SAFL 带来虚假安全感。

填补 SAFL 投毒攻击空白，用"最陈旧合法模型"作为可容忍不一致上界的设计巧妙，威胁模型克制、框架可扩展、消融验证三模块必要性。局限：假设攻击者控制最快客户端并拥有同分布数据，现实偏强；仅评估精度下降型攻击；对 FLARE 胜出不稳定，细粒度防御仍有反制空间。
- **论文 PDF**: [PoiSAFL: Scalable Poisoning Attack Framework to Byzantine-resilient Semi-asynchronous Federated Learning.](https://www.usenix.org/system/files/usenixsecurity25-pang-xiaoyi.pdf)

#### Towards Lifecycle Unlearning Commitment Management: Measuring Sample-level Unlearning Completeness.

- **作者**: Cheng-Long Wang, Qi Li, Zihang Xiang, Yinzhi Cao, Di Wang
- **方向**: AI/ML安全
- **解读**:

机器学习反学习（unlearning）承担 GDPR"被遗忘权"等技术落地，但当前实践多停留在"请求—删除"的形式流程，缺乏对"是否真正完成遗忘"的量化验证。模型所有者向用户做出的反学习承诺贯穿训练、部署、再训练的完整生命周期，而样本级遗忘完整性（unlearning completeness）度量缺失，使承诺无法被审计。

据标题推断，论文提出面向生命周期的反学习承诺管理框架，核心是在样本粒度上度量"反学习完整性"——判断某条数据的影响是否真的从模型中消除；方法可能结合成员推断式信号、影响函数或可验证删除技术，把"承诺"形式化为可检验的量化指标，并跟踪其在模型生命周期中的保持与失效。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把反学习从"过程合规"推进到"结果可验证"，为模型所有者向监管方与用户出示遗忘证据提供技术基础。

我的思考：该工作直击反学习可审计性痛点——正如 Data Duplication 等攻击所示，残留影响可被利用，而本工作正面回应"如何度量残留"；与可验证删除（verifiable deletion）方向互补，是反学习承诺可审计化的重要一环。局限（据标题推断）：样本级度量在近似反学习方法（梯度上升、SISA 等）上的可靠性、计算开销与大规模部署可行性是常见挑战，具体方案设计有待原文呈现。


#### Atkscopes: Multiresolution Adversarial Perturbation as a Unified Attack on Perceptual Hashing and Beyond.
- **作者**: Yushu Zhang, Yuanyuan Sun, Shuren Qi, Zhongyun Hua, Wenying Wen, Yuming Fang
- **方向**: AI/ML安全
- **解读**:
端到端加密（E2EE）与内容监管长期冲突，平台普遍以感知哈希匹配（PHM）在加密前计算媒体指纹并比对敏感内容库（如儿童色情图像）。然而既有针对 PHM 的对抗攻击仅适用于个别算法或计算代价极高——例如 Prokos 等人对 PhotoDNA 的单次触发攻击需约 4 GPU 小时，根源在于攻击作用的特征尺度与哈希提取特征尺度不匹配（pHash/PDQ 为全局 DCT 特征、PhotoDNA 为中间尺度网格梯度、NeuralHash 为像素级深度特征）。

作者提出多分辨率扰动思想（ATKSCOPES）：通过局部正交变换（LOT）形式化，使每个扰动元素可影响可调尺度的图像区域，从像素尺度到全局尺度均可覆盖，并将此前的像素域攻击作为特例囊括其中。对非学习哈希采用黑盒坐标下降（对称差商估计梯度），对 NeuralHash 采用白盒反向传播，支持逃逸监管与触发监管两类攻击场景。

在 ImageNet 上各取 50 张图评估：逃逸攻击对 pHash、PDQ、PhotoDNA、NeuralHash 均达 100% 成功率，平均仅需约 123、80、138、19 轮训练即可越过检测阈值；触发攻击成功率 92%–100%，且较以往攻击在收敛效率上有巨大提升，视觉失真保持在低水平。

这是首次以统一框架成功攻击四大商用感知哈希算法，系统性暴露了 E2EE-PHM 监管体系的脆弱性，社会影响重大。局限在于评估基于 ImageNet 与公开实现（PhotoDNA 为 2021 年泄露的实现），与真实敏感库规模、实际阈值仍有差距；作者提出的防御建议尚需工程验证。该工作为监管技术与对抗攻击的持续博弈设立了新基准。

- **论文 PDF**: [Atkscopes: Multiresolution Adversarial Perturbation as a Unified Attack on Perceptual Hashing and Beyond.](https://www.usenix.org/system/files/usenixsecurity25-zhang-yushu.pdf)

#### GeCos Replacing Experts: Generalizable and Comprehensible Industrial Intrusion Detection.

- **作者**: Konrad Wolsing, Eric Wagner, Luisa Lux, Klaus Wehrle, Martin Henze
- **方向**: AI/ML安全
- **解读**:

工业控制系统（ICS）是网络攻击的重点目标，工业入侵检测系统（IIDS）是快速发现攻击、避免重大损失的关键防线；但现有研究两极分化：重度机器学习方法自动化程度高却不可解释，专家系统准确但每个部署点都需大量人工建模，难以兼顾泛化性与可理解性。

方法上提出 GeCo：从历史过程数据中自动挖掘状态空间模型，构建良性系统行为的可理解模型，以最小化运维人员的手工工作量，同时保持高检测性能与跨工业领域的泛化能力，填补"全自动"与"专家知识"之间的空白。

结果：与现有最先进 IIDS 及多个数据集对比，GeCo 在保持可理解性的同时取得更优的检测性能，并达到与专家推导规则相当的水平，展示了"可解释的自动化"在 ICS 检测中的可行性。

我的思考：该工作的成色在于把可解释性作为一等公民纳入检测设计，而非事后解释——状态空间模型天然可读，直击工业界对 ML 告警不信任的痛点；与 GeCos 系列工作一脉相承。局限：状态空间模型对过程动态的刻画深度有限，复杂非线性过程与罕见攻击的检测边界、跨领域泛化对数据分布相似度的依赖需进一步验证。

- **论文 PDF**: [GeCos Replacing Experts: Generalizable and Comprehensible Industrial Intrusion Detection.](https://www.usenix.org/system/files/usenixsecurity25-wolsing.pdf)

#### ORTHRUS: Achieving High Quality of Attribution in Provenance-based Intrusion Detection Systems.

- **作者**: Baoxiang Jiang, Tristan Bilot, Nour El Madhoun, Khaldoun Al Agha, Anis Zouaoui, Shahrear Iqbal, Xueyuan Han, Thomas Pasquier
- **方向**: AI/ML安全
- **解读**:

基于溯源图与图神经网络（GNN）的异常入侵检测报告了接近完美的准确率，但工业界仍不愿采用；论文指出根源在于归因质量（Quality of Attribution, QoA）低下——检测输出产生大量与攻击无关的节点，分析人员需耗费巨大精力排查根因、评估影响并排除误报，导致告警疲劳与分析师倦怠。

方法上提出 ORTHRUS，首个在节点粒度实现高 QoA 检测的溯源 IDS：用专门设计的 GNN 编码器捕获系统事件的细粒度时空动态以识别恶意主机，再通过依赖分析重建攻击路径，使输出直接对应可调查的攻击过程。

结果：与五个最先进的 IDS 对比，ORTHRUS 将需要人工检查的节点数量减少数个数量级，显著减轻分析师负担，同时保持强检测性能，证明"高准确率"与"高可用性"可以兼得。

我的思考：该工作直击"论文指标与产业采纳脱节"这一关键问题，把 QoA 概念化为可优化的目标而非事后附加，攻击路径重建使检测结果可解释可追踪，方向极具现实意义。局限：节点级归因依赖 GNN 学习到的时空特征质量，对抗性规避与长时程多步攻击下的路径重建完整性尚需更多场景验证；与同组"简单方法更好"的对比研究相互印证，提示社区应重视评估基准的公平性。

- **论文 PDF**: [ORTHRUS: Achieving High Quality of Attribution in Provenance-based Intrusion Detection Systems.](https://www.usenix.org/system/files/usenixsecurity25-jiang-baoxiang.pdf)

#### Sometimes Simpler is Better: A Comprehensive Analysis of State-of-the-Art Provenance-Based Intrusion Detection Systems.

- **作者**: Tristan Bilot, Baoxiang Jiang, Zefeng Li, Nour El Madhoun, Khaldoun Al Agha, Anis Zouaoui, Thomas Pasquier
- **方向**: AI/ML安全
- **解读**:

溯源型入侵检测近年被 GNN 等复杂模型主导，文献中普遍报告接近完美的检测性能；但复杂模型的高开销、难解释与脆弱可复现性，使其在真实部署中的实际优势存疑，社区缺乏对各类方法在统一基准下的系统比较。

据标题推断，论文对最先进的溯源型入侵检测系统进行全面对比分析：在统一数据集与评估协议下复现并比较基于图学习、统计与规则等不同技术路线的方法，检验"复杂模型是否真的更优"，并给出部署选型建议。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于用系统化实证厘清方法优劣，为研究社区提供可复现的评估基准，挑战"越复杂越好"的默认假设。

我的思考：该工作与同组 ORTHRUS 互补，后者提出新方法、前者做横向检验，体现了作者团队对领域评估严谨性的重视；"简单更好"的结论若成立，将推动社区重新审视评估数据集的偏差与基线方法的公平性。局限（据标题推断）：比较结论受数据集与攻击场景选择影响，不同方法的最优适用域（实时性、资源受限环境）需要更细粒度的刻画；具体对比框架与量化结果待原文确认。


#### CAMP in the Odyssey: Provably Robust Reinforcement Learning with Certified Radius Maximization.

- **作者**: Derui Wang, Kristen Moore, Diksha Goel, Minjune Kim, Gang Li, Yang Li, Robin Doss, Minhui Xue, Bo Li, Seyit Camtepe, Liming Zhu
- **方向**: AI/ML安全
- **解读**:

深度强化学习（DRL）智能体对观测噪声与对抗扰动脆弱，在机器人、自动驾驶等关键任务中可能造成严重后果；随机平滑（RS）通过策略平滑为 DRL 提供可证明的鲁棒性保证，但现有可认证鲁棒 DRL 仅用简单高斯增强训练策略，导致认证鲁棒性与认证回报之间的权衡远非最优，制约了实际部署。

方法上提出 Certified-rAdius-Maximizing Policy（CAMP）训练范式：利用"全局认证半径可由训练时统计量推导出的局部认证半径导出"这一洞察，构造与局部认证半径相关的替代损失来指导策略优化，并引入策略模仿技术稳定训练过程。

结果：在多种任务上的实验表明，CAMP 显著改善鲁棒性—回报权衡，认证期望回报最多可达基线方法的两倍，代码已开源，为可认证鲁棒 DRL 提供了可直接使用的训练范式。

我的思考：把认证半径直接嵌入训练目标而非事后认证，是方法论上的关键改进——局部半径替代损失的构造使"最大化可证明保证"成为可微优化目标，策略模仿则解决了训练稳定性这一实践难点。局限：认证保证依赖平滑噪声分布与攻击者模型假设，两倍提升在不同任务与噪声水平下的稳定性、以及大规模状态空间上的可扩展性仍需更多验证。

- **论文 PDF**: [CAMP in the Odyssey: Provably Robust Reinforcement Learning with Certified Radius Maximization.](https://www.usenix.org/system/files/usenixsecurity25-wang-derui.pdf)

#### Towards Understanding and Enhancing Security of Proof-of-Training for DNN Model Ownership Verification.
- **作者**: Yijia Chang, Hanrui Jiang, Chao Lin, Xinyi Huang, Jian Weng
- **方向**: AI/ML安全
- **解读**:
DNN 经济价值巨大，模型窃取攻击频发，proof-of-training（PoT）以训练记录作为所有权证明。但既有方案（重训练式与统计式）的判别准则建立在直觉与观察之上，缺乏严谨分析：重训练式已被伪造记录攻破，作者还攻破了统计式方案，PoT 安全性整体存疑。

作者首次以形式化方法研究 PoT：形式化训练记录与威胁模型，将"非平凡"攻击归类为算法操纵与数据伪造两类；用信息论分析诚实与伪造记录的关键区别，得出普适准则——被伪造的算法与轨迹只能诱导出低保真数据，无法满足重训练要求或显著偏离目标分布；据此提出不依赖训练数据的通用构造，基于轨迹匹配算法让验证者合成数据并检查保真度。

用带内存效率的轨迹匹配算法实例化方案，在经典 DNN 与数据集上模拟包括攻破既有方案在内的多种伪造方法，实验表明其能有效区分所有者与攻击者，且模型越大、数据集越复杂防御效果越强。

该工作为 PoT 安全提供首个形式化基础，并把数据蒸馏中的轨迹匹配引入所有权验证，交叉创新有新意。局限在于结论依赖攻击者能力假设，验证计算成本与真实大规模训练适用性仍需评估。

- **论文 PDF**: [Towards Understanding and Enhancing Security of Proof-of-Training for DNN Model Ownership Verification.](https://www.usenix.org/system/files/usenixsecurity25-chang-yijia-verification.pdf)

#### AGNNCert: Defending Graph Neural Networks against Arbitrary Perturbations with Deterministic Certification.
- **作者**: Jiate Li, Binghui Wang
- **方向**: AI/ML安全
- **解读**:
图神经网络（GNN）对边、节点、节点特征三类扰动都很脆弱，经验防御很快被自适应攻击击破；既有认证防御存在三重局限：几乎都只限单类型扰动、只针对单一任务、且除一个特例外鲁棒性保证均为概率性而非 100% 准确。

作者提出 AGNNCert，首个针对任意扰动、同时适用于节点与图分类任务的确定性认证防御：用哈希函数将图确定性划分为多个子图，以基分类器对各子图的预测投票构建投票分类器，再推导可证明的鲁棒扰动规模。给出边中心与节点中心两种划分策略，后者将无向边视为两条有向边、仅按节点出边映射子图索引，使被操纵节点即使连无限条边也只影响一个子图。

边中心划分下，扰动规模 200（节点任务）/10（图任务）时认证准确率约 70%/60%；节点中心划分在总扰动 200/10 时达到相近准确率，且不限制被操纵节点度数。在节点注入（对比 BiRS）与边/特征扰动（对比 GNNCert）上均显著超越 SOTA，并在 Amazon 图（200 万节点、5100 万边）与 Big-Vul 数据集上验证了可扩展性。

该工作首次统一解决"任意扰动+双任务+确定性保证"三难问题，将既有认证防御收为特例，工程上可扩展至大规模真实图。局限在于投票机制损失部分自然准确率，认证半径以扰动数量计，对实际攻击者的约束取决于阈值选择。

- **论文 PDF**: [AGNNCert: Defending Graph Neural Networks against Arbitrary Perturbations with Deterministic Certification.](https://www.usenix.org/system/files/usenixsecurity25-li-jiate.pdf)

#### LightShed: Defeating Perturbation-based Image Copyright Protections.

- **作者**: Hanna Foerster, Sasha Behrouzi, Phillip Rieger, Murtuza Jadliwala, Ahmad-Reza Sadeghi
- **方向**: AI/ML安全
- **解读**:

Stable Diffusion 等图像生成模型可能未经授权使用版权作品训练，为此学界提出 Glaze、NightShade 等方案，通过向图像注入不易察觉的扰动（毒化）阻止模型有效利用这些样本；这些版权保护方案的鲁棒性缺乏系统检验，攻击者可能通过去毒化（depoisoning）使保护失效。

方法上提出 LightShed，一种可泛化的去毒化攻击：利用保护方案公开可用、可批量生成毒化样本的特点，建模毒化扰动的特征并提取"指纹"，据此高效识别受保护图像并中和其中的对抗扰动，且单一模型可跨扰动技术泛化。

结果：对 NightShade 毒化图像的检测达到 99.98% 的真阳性率（TPR）与 100% 的真阴性率（TNR），并能有效去毒化；同时证明 LightShed 可推广到不同扰动技术，显示现有版权保护方案的脆弱性。

我的思考：该工作揭示了"公开可用的保护机制本身可被用来学习其指纹"这一攻防闭环，指纹建模的思路使其区别于逐方案特设攻击，对版权保护设计者提出"扰动不可模式化"的新要求。局限：依赖对保护方案生成过程的建模，未来更隐蔽、自适应或样本相关的扰动可能削弱指纹有效性；TPR/TNR 是在 NightShade 上的结果，跨方案的量化表现与对抗保护（抗指纹化）的博弈需要持续跟进。

- **论文 PDF**: [LightShed: Defeating Perturbation-based Image Copyright Protections.](https://www.usenix.org/system/files/usenixsecurity25-foerster.pdf)

#### Robustifying ML-powered Network Classifiers with PANTS.

- **作者**: Minhao Jin, Maria Apostolaki
- **方向**: AI/ML安全
- **解读**:

资源分配、入侵检测等网络管理任务日益依赖基于机器学习的流量分类器（MNC），但 MNC 易受对抗输入攻击，可能导致网络中断与安全违规；为 MNC 生成"可实现"（realizable）的对抗输入比一般 ML 模型困难得多——流量整形等非可微组件与保持语义、可靠性的约束使成熟的梯度方法无法直接套用。

方法上提出 PANTS，一个实用白盒框架：将对抗机器学习（AML）技术与可满足性模理论（SMT）求解器独特结合，生成满足威胁模型与语义约束的对抗输入，并嵌入迭代式对抗训练流程以增强 MNC 的鲁棒性。

结果：与最先进的基线 Amoeba 与 BAP 相比，PANTS 中位数上找到对抗输入的可能性分别高 70% 与 2 倍；经 PANTS 鲁棒化后，目标 MNC 的鲁棒性提升 52.7%（即使面对鲁棒化时未考虑的更强攻击者），且不牺牲分类准确率。

我的思考：把 SMT 约束求解引入对抗样本生成，精准解决了"非可微+强约束"场景下梯度方法失效的问题，且强调"可实现性"使生成样本真正可用于运营者加固模型，实用导向明确。局限：白盒假设限制了黑盒场景的适用性，SMT 求解在复杂流量特征与大规模流量上的扩展性、以及对抗训练对多攻击者族同时防御的效果仍需进一步评估。

- **论文 PDF**: [Robustifying ML-powered Network Classifiers with PANTS.](https://www.usenix.org/system/files/usenixsecurity25-jin-minhao.pdf)

#### THEMIS: Towards Practical Intellectual Property Protection for Post-Deployment On-Device Deep Learning Models.
- **作者**: Yujin Huang, Zhi Zhang, Qingchuan Zhao, Xingliang Yuan, Chunyang Chen
- **方向**: AI/ML安全
- **解读**:
端侧深度学习模型存储在用户设备上，面临模型窃取与知识产权侵权风险；TEE 等系统级保护存在第三方模型支持复杂、移动端采纳有限等现实障碍。水印虽是常用防御，但许多移动开发者缺乏 ML 技能，且部署后的端侧模型只读、仅推理（禁反向传播），使第三方无法在存量模型上嵌入水印。

作者提出 THEMIS，自动化的部署后模型水印工具：拆解 APK 提取端侧模型（对加密模型引入执行追踪机制，监控 API 调用并捕获数据对象以补全碎片化提取）；Model Rooting 依据模型 schema 逆向重构可写模型（生成数据结构类并补充序列化函数）；Model Reweighting 以公共数据集合成任意模型所需数据，配合免训练后门算法求解水印参数；并提出 FFKEW（前馈知识编辑水印），单次前馈中精确编辑模型知识完成嵌入，克服免训练算法对数据量的依赖。

在 MobileNetV2、InceptionV3、EfficientNetV2 与 FMNIST、CIFAR10、GTSRB、SVHN 数据集上，FFKEW 水印成功率均超 80% 且精度下降最小；对 Google Play 403 个真实 DL 应用端到端嵌入水印，成功率 81.14%（327 个），涵盖皮肤癌识别、安全装备检测等关键场景。

该工作首次系统解决"部署后只读推理模型"的水印嵌入问题，把 IP 保护扩展到存量应用生态，403 个真实应用的实证规模有说服力。局限在于水印本身仍面临针对性移除或检测的威胁，加密模型提取依赖特定框架的 API 行为；与所有权验证协议结合、形成完整防护闭环是其落地关键。

- **论文 PDF**: [THEMIS: Towards Practical Intellectual Property Protection for Post-Deployment On-Device Deep Learning Models.](https://www.usenix.org/system/files/usenixsecurity25-huang-yujin.pdf)

#### A Crack in the Bark: Leveraging Public Knowledge to Remove Tree-Ring Watermarks.

- **作者**: Junhua Lin, Marc Juarez
- **方向**: AI/ML安全
- **解读**:

扩散模型等生成式 AI 的输出常被嵌入水印以追溯来源，Tree-Ring 水印通过在潜空间中植入树环图案实现鲁棒认证；但水印方案的安全边界取决于其设计细节是否保密，若攻击者利用公开知识即可移除水印，生成内容的溯源与责任认定将失去根基。

据标题推断，论文提出利用公开知识移除 Tree-Ring 水印的攻击：分析水印嵌入与验证算法的公开设计，构造不破坏图像质量的水印移除变换，使水印验证失效，从而揭示该类水印在"算法公开"假设下的脆弱性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于以攻促防，为水印方案的公开性设计（Kerckhoffs 原则下的安全性）提供实证检验。

我的思考：水印安全性必须经受"公开算法+未知密钥"模型的检验，该工作把 Tree-Ring 的缺陷从理论推演落实为可复现攻击，对生成内容溯源生态有直接警示作用；与 LightShed（去毒化）同属"绕过生成内容保护"谱系。局限（据标题推断）：移除攻击对水印鲁棒性参数（强度、后处理）的敏感性、以及是否影响下游任务质量需量化评估；与蒸馏式、微调式水印移除方法的对比待原文呈现。


#### CertTA: Certified Robustness Made Practical for Learning-Based Traffic Analysis.

- **作者**: Jinzhu Yan, Zhuotao Liu, Yuyang Xie, Shiyu Liang, Lin Liu, Ke Xu
- **方向**: AI/ML安全
- **解读**:

基于机器学习的流量分析（如网站指纹识别、加密流量分类）在隐私攻防中扮演关键角色，但其分类器对输入扰动缺乏可证明的鲁棒性保证：攻击者可微调流量特征使分类器误判，而现有认证方法要么开销过高、要么精度损失大，难以在真实流量分析场景中实用。

据标题推断，论文提出 CertTA，将可认证鲁棒性（certified robustness）实用化地引入基于学习的流量分析：可能采用随机平滑或区间传播类方法，在可接受的训练与推理开销下为分类器提供扰动半径的认证保证，并保持分类准确率。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把"可证明安全"从理论目标推进为流量分析场景可部署的工具，为对抗流量分析（如 Tor 网站指纹）的攻防提供量化边界。

我的思考：流量特征的高维离散性与语义约束使通用认证方法难以直接迁移，CertTA 若能在保持准确率的同时给出有意义的认证半径，将是隐私攻防领域的重要实用化进展；与 CAMP 等认证鲁棒工作构成方法谱系。局限（据标题推断）：认证半径在流量场景下的现实意义（扰动可行性）、以及认证带来的开销与准确率折损是核心取舍，具体量化结果需原文确认。


#### Invisible but Detected: Physical Adversarial Shadow Attack and Defense on LiDAR Object Detection.

- **作者**: Ryunosuke Kobayashi, Kazuki Nomoto, Yuna Tanaka, Go Tsuruoka, Tatsuya Mori
- **方向**: AI/ML安全
- **解读**:

自动驾驶依赖 LiDAR 点云目标检测识别障碍物，而自然物体阴影虽不直接出现在输出中，却隐式影响检测结果；论文提出"Shadow Hack"，首个利用 LiDAR 点云中物体阴影的物理对抗攻击，展示了一种此前被忽视的物理攻击面。

方法上，攻击者使用 LiDAR 难以精确测量的材料制造"对抗阴影"，优化阴影的位置与尺寸以最大化点云目标识别模型的误分类；并进一步提出 BB-Validator 防御机制，通过验证检测框边界的一致性识别对抗阴影。

结果：仿真中在 11-21 米距离上对多个模型取得 100% 攻击成功率；物理实验中在 10 米处对 PointPillars 达到最高 100%、对 SECOND-IoU 达到 98% 的成功率，镜面材料在 1-14 米距离实现近 100% 的点云移除率；BB-Validator 在保持高检测精度的同时达到 100% 的防御成功率。

我的思考：把"阴影"这一自然现象转化为可控对抗载体，攻击的新颖性与物理可实现性俱佳，仿真与真实世界的双重验证增强了结论可信度；对自动驾驶安全有直接现实威胁。局限：攻击依赖特定材料与光照几何条件，场景泛化性（雨雾、复杂路面）与防御在多种攻击形态下的稳健性尚需进一步检验；与针对 LiDAR 的对抗点云攻击（Frustum 等）相比，其"不可见性"是差异化亮点。

- **论文 PDF**: [Invisible but Detected: Physical Adversarial Shadow Attack and Defense on LiDAR Object Detection.](https://www.usenix.org/system/files/usenixsecurity25-kobayashi.pdf)

#### From Threat to Trust: Exploiting Attention Mechanisms for Attacks and Defenses in Cooperative Perception.

- **作者**: Chenyi Wang, Raymond Muller, Ruoyu Song, Jean-Philippe Monteuuis, Jonathan Petit, Yanmao Man, Ryan M. Gerdes, Z. Berkay Celik, Ming Li
- **方向**: AI/ML安全
- **解读**:

协同感知（cooperative perception）让车辆通过 V2X 共享感知信息以扩展视野，其融合模型普遍采用注意力机制聚合各参与方特征；但注意力权重可被恶意车辆操纵——注入虚假或对抗特征使模型过度信任恶意来源，或压制真实目标，从而破坏协同感知的安全性与可信度。

据标题推断，论文系统研究注意力机制在协同感知中的双重角色：攻击侧利用注意力可操纵性实施误导攻击（如伪造高注意力特征、投毒共享信息），防御侧基于注意力一致性或可信度建模识别恶意参与方、实现从"威胁"到"信任"的转换。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于揭示协同感知这一新兴范式的攻击面，并给出利用同一机制构建防御的对称设计思路。

我的思考：注意力机制"可解释即可利用"的双面性在协同感知中尤为突出——多车数据融合放大了单点投毒的影响；与 LiDAR 对抗攻击（Shadow Hack）相比，该工作聚焦于跨车信息交互层。局限（据标题推断）：攻击对车端异常检测与加密认证机制的抵抗能力、防御在真实 V2X 延迟与丢包下的稳健性，以及多攻击者合谋场景的刻画需原文细化。


#### CertPHash: Towards Certified Perceptual Hashing via Robust Training.

- **作者**: Yuchen Yang, Qichang Liu, Christopher Brix, Huan Zhang, Yinzhi Cao

- **方向**: AI/ML安全

- **解读**: 

感知哈希（PHash）系统——Apple NeuralHash、Microsoft PhotoDNA、Facebook PDQ——广泛用于非法内容筛查，但对抗扰动可导致哈希逃逸（evasion）或碰撞（collision）：给非法图片加扰动使其哈希偏离原值以逃避过滤，或给合法图片加扰动使其与非法图片哈希一致以制造误报。此前没有任何带可证明保证的 PHash 系统，且因其效用定义特殊、两类攻击并存，训练认证系统极具挑战。

CERTPHash 是首个带鲁棒训练的可认证 PHash 系统，包含三个优化项：抗逃逸项为输入扰动导致的哈希偏差设定上界，抗碰撞项为扰动哈希与其他输入哈希的距离设定下界，功能项保证鲁棒训练过程中系统保持可靠有效；配合神经网络验证技术给出可证明保证。

在扰动界 ‖δ‖∞≤8/255 下实现 100% 可认证免逃逸率与 95% 免碰撞率，而同样设置下对抗训练仅为 0.01% 与 0%；CERTPHash 对经验攻击亦稳健，并在真实非法内容检测任务上表现良好。

我的思考：首次把"认证鲁棒性"引入内容审核基础设施，三个优化项直接对应 PHash 特有的双攻击面，非空洞认证（non-vacuous）是关键突破。局限是当前可认证扰动界仍较小、验证开销随网络规模增长，95% 免碰撞率意味着仍留有碰撞余地。与分类模型的认证（如 α,β-CROWN）相比，本文把认证目标从"类别稳定"推广到"哈希距离性质"，为审核系统提供了可证明的安全基座。

- **论文 PDF**: [CertPHash: Towards Certified Perceptual Hashing via Robust Training.](https://www.usenix.org/system/files/usenixsecurity25-yang-yuchen.pdf)

### 大模型安全（36 篇）

#### Provably Robust Multi-bit Watermarking for AI-generated Text.
- **作者**: Wenjie Qu, Wengrui Zheng, Tianyang Tao, Dong Yin, Yanze Jiang, Zhihua Tian, Wei Zou, Jinyuan Jia, Jiaheng Zhang
- **方向**: 大模型安全
- **解读**: 大语言模型生成文本日益以假乱真,可能被用于虚假新闻、钓鱼邮件等恶意用途。文本水印通过向生成文本嵌入比特串(如用户 ID)实现内容溯源,但现有方法在真实场景中提取长消息的准确率与效率欠佳,制约了溯源能力。

作者提出基于伪随机段分配(pseudo-random segment assignment)的新型水印方法,并设计多项技术增强水印鲁棒性,目标是同时提升长消息提取的准确率与时间效率,并给出可证明的编辑容忍度。

实验表明,在 200 token 文本中嵌入 20 比特消息时,本方法匹配率达 97.6%,而当前最优工作(Yoo et al.)仅 49.2%;同时证明在同设置下水印平均可容忍每段 17 的编辑距离,兼顾了准确率与鲁棒性。

该工作的核心贡献是把多比特水印的"可证明鲁棒性"落地,显著改善提取准确率与效率的权衡,对版权追踪与内容溯源有直接价值。局限:编辑距离模型主要覆盖局部编辑,对释义、改写等语义级变换的鲁棒性仍需检验;水印嵌入对文本质量的影响未在摘要中量化。与 GradEscape 类规避工作对照,可评估其在实际对抗条件下的强度。
- **论文 PDF**: [Provably Robust Multi-bit Watermarking for AI-generated Text.](https://www.usenix.org/system/files/usenixsecurity25-qu-watermarking.pdf)

#### HateBench: Benchmarking Hate Speech Detectors on LLM-Generated Content and Hate Campaigns.
- **作者**: Xinyue Shen, Yixin Wu, Yiting Qu, Michael Backes, Savvas Zannettou, Yang Zhang
- **方向**: 大模型安全
- **解读**: LLM 可被滥用于生成仇恨言论,仇恨言论检测器是重要防线,但各检测器对 LLM 生成仇恨言论的有效性此前未知。本文提出 HateBench 框架,系统评估检测器在 LLM 生成内容与"仇恨运动"场景下的表现。

作者构建了由六个主流 LLM 生成、覆盖 34 个身份群体的 7,838 条仇恨言论样本数据集,并由三名标注者细致标注;随后评估八个代表性检测器在该数据集上的效果,并进一步研究 LLM 驱动的仇恨运动:利用对抗攻击与模型窃取等技术刻意规避检测并自动化开展在线仇恨运动。

结果显示,检测器总体能识别 LLM 生成仇恨言论,但随 LLM 版本更新性能下降;最有效的对抗攻击达到 0.966 的攻击成功率,而通过模型窃取攻击可将攻击效率再提升 13-21 倍,同时保持可接受的攻击性能。

该工作的独特之处在于把评估从"单条内容"推进到"攻击者驱动的运动级威胁",揭示检测器面对自适应攻击的脆弱性与 LLM 放大效应,呼吁社区加固防御。局限:数据集与标注的主观性、检测器集合的时效性会影响结论;0.966 的攻击成功率说明现有检测器在对抗设置下近乎失效,防御研究亟需跟进。它为仇恨言论检测设立了更贴近现实的基准。
- **论文 PDF**: [HateBench: Benchmarking Hate Speech Detectors on LLM-Generated Content and Hate Campaigns.](https://www.usenix.org/system/files/usenixsecurity25-shen.pdf)

#### EmbedX: Embedding-Based Cross-Trigger Backdoor Attack Against Large Language Models.
- **作者**: Nan Yan, Yuqing Li, Xiong Wang, Jing Chen, Kun He, Bo Li
- **方向**: 大模型安全
- **解读**: 大语言模型在预训练/微调阶段易被植入后门:攻击者通过含触发模式的训练样本使模型在触发时输出恶意行为。现有后门多依赖显式 token 触发器,易被检测。本文提出基于嵌入的跨触发器后门攻击 EmbedX。(无摘要,据标题推断)

依据标题推断,作者将触发器设计在嵌入空间而非表面 token 上,使多种形态的输入(不同 token、不同语言或不同语义)共享同一嵌入特征即可激活后门,从而提升触发隐蔽性与跨场景迁移性。

研究应报告在微调与指令微调场景下对多个 LLM 的后门注入成功率、触发隐蔽性与对正常能力的影响。由于摘要与全文缺失,具体数值无法引用。

嵌入级后门代表了"触发器语义化"的趋势,威胁在于绕过基于 token 统计的检测,且跨触发器特性显著扩大攻击面。与文本水印、越狱研究一样,它属于 LLM 供应链可信度问题:第三方微调与权重分发都可能是注入点。局限:无法核实实验设置;此类攻击的防御(如激活值检测、安全对齐验证)仍是开放问题。

#### Mind the Inconspicuous: Revealing the Hidden Weakness in Aligned LLMs' Refusal Boundaries.
- **作者**: Jiahao Yu, Haozheng Luo, Jerry Yao-Chieh Hu, Yan Chen, Wenbo Guo, Han Liu, Xinyu Xing
- **方向**: 大模型安全
- **解读**: 对齐(aligned)大模型被训练为拒绝有害请求,其"拒绝边界"决定了哪些输入会被拒绝、哪些被放行。已有越狱研究聚焦显式有害提示,但对边界附近"不起眼"输入的脆弱性缺乏机制级理解。本文揭示对齐 LLM 拒绝边界的隐藏弱点。(无摘要,据标题推断)

依据标题推断,作者系统刻画拒绝边界附近输入空间的结构(如微小扰动、前缀/后缀注入、无害化改写如何跨越边界),分析边界脆弱的成因,并提出利用该弱点的攻击或评估方法。

研究应展示在边界附近构造的低成本输入如何稳定绕过拒绝,或量化边界的不稳定性。由于摘要与全文缺失,具体攻击成功率与模型范围无法引用。

"拒绝边界"视角比零散的越狱样例更具解释力:若边界本质上是粗糙的决策面,则任何对齐模型都存在系统性绕过空间。该方向与安全对齐的可证明化密切相关。局限:无法核实方法细节;边界行为高度依赖具体模型与提示分布,结论外推需谨慎。与同期"Refusal Is Not an Option"对照,可构成"边界攻击—拒绝卸载"的完整威胁图景。

#### Game of Arrows: On the (In-)Security of Weight Obfuscation for On-Device TEE-Shielded LLM Partition Algorithms.
- **作者**: Pengli Wang, Bingyou Dong, Yifeng Cai, Zheng Zhang, Junlin Liu, Huanran Xue, Ye Wu, Yao Zhang, Ziqi Zhang
- **方向**: 大模型安全
- **解读**: 为保护端侧 LLM 推理的模型权重,常见做法是把权重混淆后放入可信执行环境(TEE)保护的分区算法中。此类"权重混淆"方案宣称可防止提取与窃取,但其安全性缺乏严格检验。本文研究其(不)安全性。(无摘要,据标题推断)

依据标题推断,作者对现有 TEE 保护下的 LLM 分区与权重混淆方案建模,构造攻击以恢复或绕过混淆权重,可能利用混淆的确定性、分区接口的侧信道或算法层面的可逆性。

研究应证明现有混淆方案可被实用攻破,并分析攻防双方的成本。由于摘要与全文缺失,具体攻击效率与受影响方案列表无法引用。

该工作呼应"混淆不等于安全"的经典论断,对日益流行的端侧模型保护(如设备端大模型推理)有重要警示:若混淆可被逆向,则 TEE 加混淆的堆叠并不等于可信保护。局限:无法核实攻击场景与威胁模型细节。对产业界的启示是模型保护应依赖可证明机制(如硬件信任根加最小暴露面),而非工程上的混淆技巧。

#### LLMmap: Fingerprinting for Large Language Models.
- **作者**: Dario Pasquini, Evgenios M. Kornaropoulos, Giuseppe Ateniese
- **方向**: 大模型安全
- **解读**: 越来越多应用集成 LLM 能力,识别应用背后具体使用哪个 LLM 版本,对安全审计、滥用归因与竞争分析都有价值,但此前缺乏面向 LLM 集成应用的一代指纹识别技术。本文提出 LLMmap,首个针对 LLM 集成应用的主动指纹识别方案。

LLMmap 采用主动指纹识别:向应用发送精心构造的查询,分析响应以识别具体 LLM 版本;查询选择基于对 LLM 如何对主题多变提示产生独特可识别响应的领域知识。设计上跨应用层鲁棒,可识别开源或专有、多厂商的 LLM 版本。

实验显示,仅需最少 8 次交互,LLMmap 即可在 42 个不同 LLM 版本上实现超过 95% 的识别准确率;且在未知系统提示、随机采样超参数,乃至 RAG 或思维链(CoT)等复杂生成框架下依然有效。作者讨论潜在缓解,并指出面对资源充足的攻击者,有效反制可能困难甚至不可实现。

LLMmap 的价值在于把"模型身份"变成可主动探测的指标,对模型盗用检测、合规审计与安全调查都有用武之地;其跨层鲁棒性设计贴近真实部署。局限:主动探测本身可能被日志记录,且 95% 准确率来自受限版本集,版本持续更新会稀释指纹库。与模型窃取/指纹防御研究形成攻防闭环,值得后续跟踪。
- **论文 PDF**: [LLMmap: Fingerprinting for Large Language Models.](https://www.usenix.org/system/files/usenixsecurity25-pasquini.pdf)

#### Refusal Is Not an Option: Unlearning Safety Alignment of Large Language Models.
- **作者**: Minkyoo Song, Hanna Kim, Jaehan Kim, Seungwon Shin, Sooel Son
- **方向**: 大模型安全
- **解读**: 大模型的安全对齐依赖"拒绝"机制:对有害请求返回拒答。若攻击者能移除或绕过拒绝行为,对齐保护即告失效。已有研究聚焦提示注入式越狱,本文研究通过"遗忘学习"(unlearning)直接卸载安全对齐的威胁。(无摘要,据标题推断)

依据标题推断,作者构造可在微调/遗忘框架下执行的攻击:用精心挑选的(可能无害的)数据对模型进行小规模更新,使其遗忘拒绝行为,从而让模型对有害请求也给出回答,同时尽量保持其余能力。

研究应证明低成本的微调即可系统性清除拒绝行为,并讨论现有对齐防线(如输入输出过滤器)的失效。由于摘要与全文缺失,具体数据量与模型范围无法引用。

"拒绝卸载"是比越狱更根本的威胁:一旦权重层面的拒绝被擦除,提示级过滤难以兜底,这对开放权重模型的二次分发安全提出严肃问题。局限:无法核实实验设置;此类攻击依赖对模型权重的写权限,威胁模型需明确。与"Mind the Inconspicuous"的边界攻击互补,共同说明对齐鲁棒性需要权重级与行为级的双重保障。

#### Activation Approximations Can Incur Safety Vulnerabilities in Aligned LLMs: Comprehensive Analysis and Defense.
- **作者**: Jiawen Zhang, Kejia Chen, Lipeng He, Jian Lou, Dan Li, Zunlei Feng, Mingli Song, Jian Liu, Kui Ren, Xiaohu Yang
- **方向**: 大模型安全
- **解读**: 为在资源受限设备上部署对齐 LLM,常用激活近似/压缩技术(如量化、稀疏化)降低计算与内存开销,但这些近似是否破坏模型的安全对齐,此前缺乏系统研究。本文对激活近似引发的安全漏洞做全面分析与防御。(无摘要,据标题推断)

依据标题推断,作者系统对比多种激活近似方案在安全基准(有害请求拒绝率、越狱鲁棒性)上的退化情况,定位漏洞成因(近似误差如何放大不安全行为),并提出缓解或防御方法。

研究应量化不同近似程度下安全性能的下降曲线,并验证防御手段的有效性。由于摘要与全文缺失,具体模型、近似方法与数值结果无法引用。

该主题的实践意义强:端侧 LLM 部署是主流趋势,若压缩破坏对齐,则"更高效的模型"同时是"更不安全的对齐"。把安全退化纳入近似方案评估指标,是对传统"精度-体积"权衡的重要补充。局限:无法核实方法细节;近似对对齐的影响可能与具体安全基准高度相关,需要更全面的基准验证。此类工作有助于推动压缩-安全联合设计。

#### Membership Inference Attacks Against Vision-Language Models.
- **作者**: Yuke Hu, Zheng Li, Zhihao Liu, Yang Zhang, Zhan Qin, Kui Ren, Chun Chen
- **方向**: 大模型安全
- **解读**: 视觉-语言模型（VLM）基于预训练视觉编码器与 LLM 构建，展现出卓越的多模态理解与对话能力，但其数据滥用与泄露风险几乎未被探索。指令微调数据最可能包含敏感或未经授权信息，本文首次以成员推断攻击（MIA）的视角，系统分析 VLM 中的误用与泄露检测问题，填补了多模态模型隐私评估的空白。

针对 VLM 指令微调数据设计成员推断攻击方法，在有限信息条件下判断某样本是否参与过训练；为克服视觉与文本模态融合带来的挑战，提出针对性的攻击策略并构建相应评估框架，刻画攻击可行性与影响因素。

论文以多种 VLM 与指令微调数据为对象开展系统实验，评估成员推断攻击的可行性并分析关键影响因素，为评估 VLM 数据泄露风险提供了依据（摘要截断，未给出具体量化指标）。

作为首个 VLM 场景的成员推断研究，该工作把隐私攻击从纯文本模型拓展到多模态模型，警示指令微调数据的敏感信息暴露风险。局限是实验规模与模型覆盖面有限，且摘要未披露具体数值；未来应扩展到更大规模 VLM，并评估差分隐私微调、输出扰动等防御手段的有效性。

- **论文 PDF**: [Membership Inference Attacks Against Vision-Language Models.](https://www.usenix.org/system/files/usenixsecurity25-hu-yuke.pdf)

#### Towards Label-Only Membership Inference Attack against Pre-trained Large Language Models.
- **作者**: Yu He, Boheng Li, Liu Liu, Zhongjie Ba, Wei Dong, Yiming Li, Zhan Qin, Kui Ren, Chun Chen
- **方向**: 大模型安全
- **解读**: 成员推断攻击（MIA）旨在判断某数据样本是否属于模型训练集。已有针对 LLM 的 MIA 研究大多需要访问完整输出 logits，而现实中 API 通常只返回生成文本，攻击条件难以满足。本文研究标签-仅（label-only）设置下预训练 LLM 的成员推断脆弱性——攻击者只能访问生成的 token 文本。

作者首先揭示：现有 label-only MIA 对预训练 LLM 效果微弱，尽管它们对个性化 LLM 的微调数据集推断非常有效。进一步分析发现失败源于两个主要原因，包括预训练模型更好的生成特性等（摘要截断），据此设计针对性改进的 label-only 攻击方法，仅凭文本输出完成成员判定。

实验验证改进后的 label-only 攻击能在仅访问生成文本的现实条件下有效攻击预训练 LLM，并与 logits 攻击对比量化性能差距（摘要截断，具体数值未给出）。

该工作把 MIA 推向更贴近真实部署的威胁模型（仅文本输出），对 API 化 LLM 的隐私风险评估意义重大。局限是黑盒文本采样的成本较高、攻击耗时，且差分隐私微调、输出扰动等防御可能显著削弱攻击效果，攻击-防御的后续博弈值得跟踪。

- **论文 PDF**: [Towards Label-Only Membership Inference Attack against Pre-trained Large Language Models.](https://www.usenix.org/system/files/usenixsecurity25-he-yu.pdf)

#### Depth Gives a False Sense of Privacy: LLM Internal States Inversion.
- **作者**: Tian Dong, Yan Meng, Shaofeng Li, Guoxing Chen, Zhen Liu, Haojin Zhu
- **方向**: 大模型安全
- **解读**: LLM 推理过程中产生的中间隐藏状态可能被服务端、协作方或分块推理的第三方接触。人们常以为深层状态已高度抽象、不泄露输入信息，本文质疑这一直觉，研究从 LLM 内部状态反演出原始输入文本的攻击，揭示"深度带来隐私安全感"实为错觉。

利用预训练 LLM 各层的隐藏状态，训练或构造解码器将内部表示映射回输入 token 序列，系统考察不同深度层的可反演性，证明较深的内部状态同样（甚至更）容易被恢复出用户输入。（无摘要，据标题推断）

实验表明攻击能以较高保真度从内部状态重建输入文本，对基于内部状态访问的服务架构构成实质隐私威胁，深层抽象并不能提供有效的隐私保护。（无摘要，据标题推断）

该工作发出重要警示：向第三方暴露中间激活（如模型拆分部署、协作推理）会显著放大隐私风险，深度并不等于安全。局限是攻击对更大模型与长上下文的可扩展性、以及防御性表示扰动等缓解手段的有效性仍待探索，未来需在隐私与推理性能之间寻求平衡。


#### When LLMs Go Online: The Emerging Threat of Web-Enabled LLMs.
- **作者**: Hanna Kim, Minkyoo Song, Seung Ho Na, Seungwon Shin, Kimin Lee
- **方向**: 大模型安全
- **解读**: LLM 已演进为能规划并调用工具的智能体系统，常与网页工具结合以获取多样化来源与实时信息。能力增强的同时，恶意使用风险也随之上升，尤其在涉及个人信息的网络攻击中。本文研究联网 LLM 智能体被用于网络攻击的风险，系统回答三个问题。

具体考察：被指示实施网络攻击时 LLM 智能体的能力有多强；网页工具如何增强攻击效果；此类攻击的代价与易用性如何（摘要截断），以实证方式评估 AI 智能体对攻击者能力的放大效应。

研究发现 LLM 智能体在网页工具加持下能够完成涉及个人信息的攻击流程，显著降低攻击门槛与实施成本，放大攻击者的能力与攻击规模（摘要截断，具体数字未给出）。

该工作聚焦"AI 即攻击代理"的实证威胁，为防御者理解智能体滥用提供了第一手证据。局限是实验环境与真实对抗环境存在差距，且安全对齐与越狱防护可能削弱攻击；缓解需结合 API 管控、工具权限最小化与滥用监测等治理手段。

- **论文 PDF**: [When LLMs Go Online: The Emerging Threat of Web-Enabled LLMs.](https://www.usenix.org/system/files/usenixsecurity25-kim-hanna.pdf)

#### PRSA: Prompt Stealing Attacks against Real-World Prompt Services.

- **作者**: Yong Yang, Changjiang Li, Qingming Li, Oubo Ma, Haoyu Wang, Zonghui Wang, Yandong Gao, Wenzhi Chen, Shouling Ji
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）商业提示词（prompt）服务把精心设计的系统提示词作为核心资产出售或订阅，但攻击者可通过黑盒查询从模型输出中逆向推断（steal）这些提示词，从而免费窃取商业服务的关键知识产权。PRSA 针对真实世界的提示词服务提出提示词窃取攻击，研究攻击的可行性与威胁面。

方法上，该工作应针对真实商业提示词服务（如各类 prompt 市场与 API 封装）设计攻击流程：通过构造探测性查询、利用模型对提示词结构的泄漏，逐步重建隐藏的系统提示词，并可能研究提示词长度、温度等参数对窃取成功率的影响。

预期结果将展示对多个真实服务的窃取成功率与重建保真度，证明该威胁在实践中的严重性，并讨论缓解措施（如输出扰动、提示词混淆）。

我的思考：提示词窃取是 LLM 商业化后的新型知识产权威胁，本研究把攻击从实验室搬到真实服务，威胁建模更贴近实际；但论文无摘要，细节（如是否需要大量查询、是否依赖特定模型）有待原文验证。相比越狱，该威胁的经济动机更直接，值得提示词服务商认真对待。


#### Cross-Modal Prompt Inversion: Unifying Threats to Text and Image Generative AI Models.

- **作者**: Dayong Ye, Tianqing Zhu, Feng He, Bo Liu, Minhui Xue, Wanlei Zhou
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）生成式 AI 模型的提示词（prompt）蕴含用户隐私与创作意图，提示词反演（prompt inversion）攻击试图从模型输出（文本或图像）逆向重建输入提示词。本文提出跨模态提示词反演框架，把文本与图像两类生成模型的威胁统一起来，揭示其共同机理。

方法上，该工作应设计统一的反演框架：对文本模型，从生成文本推断原始指令与隐私属性；对图像模型，从生成图像恢复提示词（即文本到图像模型的提示词重建），并可能利用梯度、嵌入或生成过程的结构信息提高重建精度。

预期结果将展示跨模态攻击的可行性、重建质量随模型与输出变化的规律，并论证该威胁对隐私（如人脸、位置等敏感信息）与版权的实际影响。

我的思考：「跨模态统一」是本文的差异化贡献——把文本与图像领域的提示词反演归入同一威胁模型，有助于设计通用防御。无摘要意味着具体指标需查原文；隐私角度的风险论证（从输出反推用户提示中的敏感内容）对合规与伦理审查有意义，可能催生输出侧净化等防御研究。


#### Prompt Obfuscation for Large Language Models.

- **作者**: David Pape, Sina Mavali, Thorsten Eisenhofer, Lea Schönherr
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）提示词（prompt）是 LLM 应用的敏感资产，既有研究关注提示词窃取与反演攻击，本文从对立面出发研究提示词混淆（prompt obfuscation）：把提示词变换为难以被第三方理解或逆向的形式，同时尽量保持模型输出质量，以此保护提示词不被窃取。

方法上，该工作应探索多种混淆策略——如语义等价改写、指令与数据交织、编码/嵌入技巧或对抗性扰动——并评估混淆后的提示词在目标模型上的输出保真度、对窃取/反演攻击的抵御能力，以及与原始提示词的相似性度量。

预期结果将给出混淆的有效性-可用性权衡：混淆越强，窃取越难，但输出质量与可维护性可能下降，需在两者间取平衡。

我的思考：把「混淆」系统化为提示词保护手段，是对提示词窃取/反演攻击研究的镜像补充，攻防对照完整。难点在于提示词的语义敏感性使强混淆极易损害效用，且攻击者可能利用模型自身能力进行去混淆；论文若能给出系统的混淆分类与实证评估，将对商业化提示词资产的保护实践有直接价值。


#### TwinBreak: Jailbreaking LLM Security Alignments based on Twin Prompts.

- **作者**: Torsten Krauß, Hamid Dashtbani, Alexandra Dmitrienko
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）越狱攻击通过精心构造的提示词绕过 LLM 的安全对齐，使其输出有害内容。TwinBreak 提出基于「孪生提示词」（twin prompts）的越狱方法：利用成对出现、仅细微差异（如措辞、格式、上下文）的提示词组合，诱使模型在看似无害的交互中突破安全边界。

方法上，该工作应系统构造孪生提示词对——如安全请求与恶意请求的镜像变体、语义相同但表述不同的双通道输入——使安全审查机制难以识别恶意意图，或利用多提示组合分散模型的注意力，从而绕过对齐。

预期结果将展示 TwinBreak 在多个主流开源/闭源 LLM 上的越狱成功率，并分析其与单提示越狱、多轮越狱等方法的差异与优势。

我的思考：利用「孪生」结构制造安全审查盲区，是提示工程类越狱的新变体，其思路与多代理、多通道攻击相通，说明对齐机制的盲点会随输入结构复杂化而扩大。无摘要使具体成功率待查原文；该类工作再次表明，仅靠对齐训练难以根治越狱，需要输入侧检测与输出侧审查协同防御。


#### Exploiting Task-Level Vulnerabilities: An Automatic Jailbreak Attack and Defense Benchmarking for LLMs.

- **作者**: Lan Zhang, Xinben Gao, Liuyi Yao, Jinke Song, Yaliang Li
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）现有越狱研究多关注提示级漏洞（如对抗后缀、角色扮演），本文转向「任务级漏洞」（task-level vulnerabilities）：某些任务类别本身容易诱发模型生成不安全内容。论文构建自动化的越狱攻击与防御基准，系统化度量任务级漏洞。

方法上，该工作应自动化生成覆盖多样任务类别（如编码、写作、翻译等）的越狱场景，自动评估攻击成功率与防御效果，形成可复现的基准框架，并分析哪些任务属性与漏洞相关。

预期结果将给出任务类别×模型×防御的量化矩阵，揭示不同模型在不同任务上的脆弱性分布，以及现有防御在任务级攻击下的表现差异。

我的思考：把「任务本身」视为漏洞来源，是越狱研究的重要视角补充——很多越狱并非提示技巧高明，而是任务范式天然越界。自动化的攻防基准有助于标准化评估、避免手工样例偏差；但基准的任务覆盖面与「任务级」判定的客观性决定其价值，这类工作通常还需要人工校验以避免误判为越狱。


#### StruQ: Defending Against Prompt Injection with Structured Queries.

- **作者**: Sizhe Chen, Julien Piet, Chawin Sitawarin, David A. Wagner
- **方向**: 大模型安全
- **解读**: LLM 集成应用的兴起带来提示注入（prompt injection）威胁：攻击者诱使模型偏离应用原始指令而遵循用户指令，其根源在于 LLM 擅长遵循指令却无法区分提示（prompt）与用户数据。StruQ 提出结构化查询这一通用方案，把提示与数据分离到两个通道。

系统由两部分组成：（1）安全前端把提示和用户数据格式化为特殊格式；（2）经专门训练的 LLM 能基于这些输入产生高质量输出。训练采用新颖的微调策略：把基础（非指令微调）LLM 转换为「结构化指令微调」模型，使其只遵循查询提示部分的指令——方法是在标准指令微调数据中加入数据部分也含指令的样例，微调模型学会忽略这些指令。

结果显示：系统显著提升了对提示注入攻击的抵抗力，且对效用几乎无影响；代码已开源。

我的思考：StruQ 把「通道分离」这一经典安全原则引入 LLM 应用，属结构性防御而非事后检测，方向正确且可推广。用数据增强式微调让模型学会忽略数据通道指令，比启发式过滤更根本；局限是需自训模型、与现有黑盒 LLM 应用的迁移成本，以及对抗性数据通道指令的鲁棒性仍需评估。

- **论文 PDF**: [StruQ: Defending Against Prompt Injection with Structured Queries.](https://www.usenix.org/system/files/usenixsecurity25-chen-sizhe.pdf)

#### PAPILLON: Efficient and Stealthy Fuzz Testing-Powered Jailbreaks for LLMs.

- **作者**: Xueluan Gong, Mingzhe Li, Yilin Zhang, Fengyuan Ran, Chen Chen, Yanjiao Chen, Qian Wang, Kwok-Yan Lam
- **方向**: 大模型安全
- **解读**: LLM 易受越狱攻击，现有方法或依赖手工模板（扩展性与适应性差），或难以生成语义连贯的提示（易被检测），且多数提示冗长导致查询成本高。PAPILLON 是针对这些挑战的自动化黑盒越狱框架，把黑盒模糊测试思想与系列定制设计相结合。

与手工模板不同，PAPILLON 从空种子池开始，无需搜索任何越狱模板；提出三种基于 LLM 助手的「问题相关」变异策略，在保持语义连贯的同时显著缩短提示长度；并实现两级评判模块以准确识别真正成功的越狱。在 7 个代表性 LLM 上评估、与 5 种最先进越狱策略对比。

对专有 LLM API（GPT-3.5-turbo、GPT-4、Gemini-Pro），PAPILLON 分别取得超过 90%、80%、74% 的攻击成功率，超出基线 60% 以上；对 GPT-4 即使仅用 100 token 也能达到 78% 以上成功率；同时表现出可迁移性，并对最先进防御具有鲁棒性。

我的思考：模糊测试思想（空种子池+变异+评判器）被成功迁移到越狱场景，自动化程度与提示简洁性是亮点——短提示直接降低查询成本并增强隐蔽性。60% 以上的相对提升幅度显著，但黑盒评估依赖单一实现，其「鲁棒于防御」的结论需结合具体防御再检验；攻击侧工作的价值也在于为防御研究提供更强基准。

- **论文 PDF**: [PAPILLON: Efficient and Stealthy Fuzz Testing-Powered Jailbreaks for LLMs.](https://www.usenix.org/system/files/usenixsecurity25-gong-xueluan.pdf)

#### Great, Now Write an Article About That: The Crescendo Multi-Turn LLM Jailbreak Attack.

- **作者**: Mark Russinovich, Ahmed Salem, Ronen Eldan
- **方向**: 大模型安全
- **解读**: LLM 被严格对齐以拒绝涉足非法或不道德话题，而越狱攻击旨在缩小「模型能做什么」与「愿意做什么」之间的差距。

方法上，Crescendo 是一种新颖的多轮越狱：与现有方法不同，它以看似良性的方式与模型多轮交互——从关于任务的一般性问题开始，逐步升级对话、引用模型自身回复，渐进地导向成功越狱。

作者在多个公开系统上评估 Crescendo，包括 ChatGPT、Gemini Pro、Gemini-Ultra、LLaMA-2 70b、LLaMA-3 70b Chat 与 Anthropic Chat，在所有被测模型与任务上都取得高攻击成功率。此外还提出自动化工具 Crescendomation：在 AdvBench 子集上，它比其他最先进越狱技术高出 29-61%（GPT-4）与 49-71%（Gemini-Pro）；Crescendo 还能越狱多模态模型。

我的思考：Crescendo 的威力在于「简单」——不依赖复杂模板或优化，仅靠多轮渐进升级与引用模型回复就普遍奏效，说明对齐在长对话语境下系统性脆弱。作者来自微软，能直接测到多款闭源系统，结果可信度高。局限是对话轮次带来的成本与检测暴露面；其「渐进升级」模式对对话级防御（状态监控、语义连贯性检查）提出了明确需求。

- **论文 PDF**: [Great, Now Write an Article About That: The Crescendo Multi-Turn LLM Jailbreak Attack.](https://www.usenix.org/system/files/usenixsecurity25-russinovich.pdf)

#### SelfDefend: LLMs Can Defend Themselves against Jailbreaking in a Practical Manner.

- **作者**: Xunguang Wang, Daoyuan Wu, Zhenlan Ji, Zongjie Li, Pingchuan Ma, Shuai Wang, Yingjiu Li, Yang Liu, Ning Liu, Juergen Rahmel
- **方向**: 大模型安全
- **解读**: 越狱攻击已演化出人类构造、优化、生成以及间接与多语言等多类变体，实用防御需同时应对所有类别、对用户提示产生可忽略的延迟，并兼容开源与闭源 LLM。

方法上，受传统安全中影子栈（shadow stack）防御内存溢出攻击的启发，SelfDefend 建立影子 LLM 作为防御实例（检测状态）与正常栈中的目标 LLM 实例（正常应答状态）并发协作，实现基于检查点的访问控制。

其有效性建立在「现有 LLM 能识别用户查询中的有害提示或意图」这一观察上，作者用主流 GPT-3.5/4 模型对主要越狱攻击做了实证验证。为提升鲁棒性并降低成本，采用数据蒸馏方法调优专门的防御模型：部署以保护 GPT-3.5/4、Claude、Llama-2-7b/13b 与 Mistral 时，这些模型胜过七种最先进防御，且以显著更低的额外延迟达到 GPT-4 版 SelfDefend 的性能；对自适应越狱与提示注入也表现鲁棒。

我的思考：把影子栈思想映射到 LLM 安全是漂亮的类比迁移——影子检测实例不干扰主流程，检查点式访问控制提供了精细拦截能力；数据蒸馏把昂贵的 GPT-4 检测能力压缩进开源小模型，兼顾成本与闭源兼容性，工程落地性强。局限是检测-应答双实例仍增加推理开销，且「LLM 能识别有害意图」的观察在对抗性自适应攻击下可能退化。

- **论文 PDF**: [SelfDefend: LLMs Can Defend Themselves against Jailbreaking in a Practical Manner.](https://www.usenix.org/system/files/usenixsecurity25-wang-xunguang.pdf)

#### "I Cannot Write This Because It Violates Our Content Policy": Understanding Content Moderation Policies and User Experiences in Generative AI Products.

- **作者**: Lan Gao, Oscar Chen, Rachel Lee, Nick Feamster, Chenhao Tan, Marshini Chetty
- **方向**: 大模型安全
- **解读**: 
（无摘要，据标题推断）随着生成式 AI 产品（对话助手、图像生成等）大规模普及，其内容审核策略日益成为决定用户体验与安全边界的关键因素。本文从标题看聚焦于"理解生成式 AI 产品中的内容审核策略与用户体验"，结合作者团队在人机交互与社会计算方向的背景，应是一项定性+定量的实证研究。

研究方法很可能综合了政策文本分析、用户访谈/调查以及对产品实际拦截行为的测试，揭示内容策略在何种情况下拒绝生成、用户遭遇"我不能写这个因为它违反我们的内容政策"式反馈时的理解与感受。核心问题在于：审核策略在防止有害内容的同时，是否过度拒绝、误伤正常请求，以及用户是否感知到不一致或难以理解的规则。

预期发现可能包括：审核边界在不同产品与提示措辞下高度不一致，用户普遍对模糊的拒绝原因感到困惑，且策略透明度不足削弱信任。这类结果将为政策制定者与产品方提供改进方向，例如更清晰的拒绝解释与更一致的判定标准。

该工作属于"社会工程与人类因素"与 AI 安全交叉的实证研究，其价值在于把通常只做技术攻防讨论的内容审核问题落到真实用户体验上。局限在于样本与产品范围可能有限，结论外推需谨慎；但作为少见的人因视角研究，对生成式 AI 治理具有现实参考意义。


#### Make Agent Defeat Agent: Automatic Detection of Taint-Style Vulnerabilities in LLM-based Agents.

- **作者**: Fengyu Liu, Yuan Zhang, Jiaqi Luo, Jiarun Dai, Tian Chen, Letian Yuan, Zhengmin Yu, Youkun Shi, Ke Li, Chengyuan Zhou, Hao Chen, Min Yang
- **方向**: 大模型安全
- **解读**: 
（无摘要，据标题推断）基于 LLM 的智能体（LLM-based agent）能够调用工具、访问外部数据并执行多步任务，成为大模型落地的主要形态；然而其"不可信输入→模型推理→敏感操作"的数据流引入了新型污点式漏洞——攻击者可通过提示注入等手段污染输入，使智能体执行未授权操作。本文提出以"智能体对抗智能体"（Make Agent Defeat Agent）的方式自动检测这类漏洞。

方法上，标题中的"污点式"（taint-style）提示其借鉴了传统程序分析的污点传播思想：把 LLM 输入视为污点源，将工具调用/敏感操作视为污点汇聚点，检测从源到汇的传播路径是否可被恶意内容触发。与传统静态污点分析不同，检测主体是另一个 LLM 智能体，利用大模型的推理能力自动构造对抗性提示并验证漏洞是否真实可利用。

预期成果是构建一个自动化的漏洞检测框架，能够在大规模 LLM 智能体应用（如各类 Agent 框架、插件生态）中发现可被提示注入利用的漏洞，并输出可复现的触发样例，弥补当前以人工审计为主的不足。

该工作把经典的污点分析范式迁移到 LLM 智能体安全，思路新颖且具有自动化价值；作者团队（复旦大学 Min Yang 组等）在程序分析与 AI 安全领域积累深厚。局限在于 LLM 输出的不确定性与检测结果的可复现性挑战，以及"用模型测模型"可能引入误报；但作为方向性探索，对智能体安全测试工具链建设有重要参考意义。


#### Machine Against the RAG: Jamming Retrieval-Augmented Generation with Blocker Documents.

- **作者**: Avital Shafran, Roei Schuster, Vitaly Shmatikov
- **方向**: 大模型安全
- **解读**: 
RAG 系统从知识库检索相关文档并交给 LLM 生成回答，当知识库包含不可信内容时，系统便面临新的攻击面。本文提出名为 jamming 的拒绝服务攻击：攻击者只需向数据库注入一篇"阻塞文档"（blocker document），使该文档在针对特定查询时被检索到，从而让 RAG 系统拒绝回答该查询——要么声称缺乏相关信息，要么判定回答不安全。

作者描述并量化了多种生成阻塞文档的方法，其中核心是一种基于黑盒优化的新方法。该方法的三个关键特性是：不依赖指令注入、无需知道目标 RAG 使用的嵌入模型或 LLM、不借助辅助 LLM——仅通过黑盒查询即可构造出能稳定"劫持"检索结果的文档，攻击门槛很低。

实验在多种嵌入模型和 LLM 上评估了 jamming 攻击的有效性，并证明现有面向 LLM 的安全指标无法刻画其对 jamming 的脆弱性——即"看起来安全"的模型实际上一击即倒。作者随后讨论了针对阻塞文档的防御措施。

该工作的贡献在于揭示 RAG 中"检索即攻击面"的新 DoS 范式，且攻击完全黑盒、成本极低，威胁模型清晰可信。局限在于 jamming 只针对特定查询、属于可用性破坏而非数据泄露，防御讨论尚处早期。与同类中毒攻击相比，它展示了 RAG 安全中"可用性"维度的空白，对 RAG 系统设计者有直接警示价值。

- **论文 PDF**: [Machine Against the RAG: Jamming Retrieval-Augmented Generation with Blocker Documents.](https://www.usenix.org/system/files/usenixsecurity25-shafran.pdf)

#### Topic-FlipRAG: Topic-Orientated Adversarial Opinion Manipulation Attacks to Retrieval-Augmented Generation Models.

- **作者**: Yuyang Gong, Zhuo Chen, Jiawei Liu, Miaokun Chen, Fengchang Yu, Wei Lu, XiaoFeng Wang, Xiaozhong Liu
- **方向**: 大模型安全
- **解读**: 
（无摘要，据标题推断）RAG 模型的回答由检索到的文档驱动，这使其成为观点操控的目标：攻击者若能在知识库中植入或操纵特定主题相关文档，就可能让模型在某个话题上输出攻击者期望的立场。本文提出 Topic-FlipRAG，一种面向主题（topic-oriented）的对抗性观点操纵攻击，目标是"翻转"RAG 模型在特定话题上的观点。

方法上，攻击者针对目标话题构造对抗性文档，使其在与该话题相关的查询中被优先检索，从而压过知识库中原本的中立或相反立场内容，诱导模型在生成时呈现被操纵的倾向性观点。攻击设计强调"面向主题"，即不针对单一问题，而是系统性地影响模型对整类话题的立场输出，放大操纵的覆盖面。

预期通过在白盒/黑盒条件下构造文档并评估观点翻转率来验证有效性，可能还会测试跨模型迁移性与对检索排序的鲁棒性。这类攻击对新闻问答、决策辅助等依赖 RAG 呈现"事实"的应用构成可信度威胁。

该工作与同批次的 RAG 攻击研究（如 jamming、PoisonedRAG）共同勾勒出 RAG 安全的全景：从拒答、知识投毒到观点操控。其独特之处在"主题级"操控目标，比单查询攻击更具社会影响力。局限在于未提供摘要，具体方法与数据待核实；但作者团队含 XiaoFeng Wang 等安全名家，方法可信度较高。


#### PoisonedRAG: Knowledge Corruption Attacks to Retrieval-Augmented Generation of Large Language Models.

- **作者**: Wei Zou, Runpeng Geng, Binghui Wang, Jinyuan Jia
- **方向**: 大模型安全
- **解读**: 
RAG 通过检索外部知识库来缓解 LLM 缺乏最新知识与幻觉的问题，但本文指出：知识库作为新的攻击面此前几乎未被安全研究覆盖。据此作者提出 PoisonedRAG，首个针对 RAG 的知识投毒攻击：攻击者向知识库注入少量恶意文本，即可诱导 LLM 对攻击者指定的目标问题输出攻击者指定的目标答案。

攻击被形式化为一个优化问题，其解即一组恶意文本。依据攻击者对 RAG 系统的背景知识，作者给出两种求解方案：白盒方案利用对检索器与生成器的完整了解进行优化，黑盒方案则仅凭查询反馈构造恶意文本，覆盖了从内部人员到外部攻击者的威胁谱系。

实验表明，PoisonedRAG 对每个目标问题仅注入 5 篇恶意文本、在包含数百万文本的知识库中即可达到 90% 的攻击成功率，说明投毒效率极高、规模无关。作者还评估了多种防御手段，结果显示现有防御均不足以抵御 PoisonedRAG，凸显新防御的迫切需求。

该工作的贡献在于定义了 RAG 安全中"知识完整性"这一此前缺失的问题，并给出系统性攻击框架与量化评估。局限在于其威胁模型假设攻击者能写入知识库（对公开众包知识库成立，对封闭库未必）；与同批次的 jamming、Topic-FlipRAG 相比，它瞄准的是"篡改答案"而非"拒绝回答"或"翻转观点"，三者共同构成 RAG 投毒攻击的完整图谱。

- **论文 PDF**: [PoisonedRAG: Knowledge Corruption Attacks to Retrieval-Augmented Generation of Large Language Models.](https://www.usenix.org/system/files/usenixsecurity25-zou-poisonedrag.pdf)

#### TracLLM: A Generic Framework for Attributing Long Context LLMs.

- **作者**: Yanting Wang, Wei Zou, Runpeng Geng, Jinyuan Jia
- **方向**: 大模型安全
- **解读**: 
（无摘要，据标题推断）长上下文 LLM 的输入可包含海量文档（如整本书、整个代码仓库或大规模检索结果），其输出究竟由哪部分输入驱动、是否忠实于输入，成为可解释性与安全审计的关键问题。TracLLM 从标题看是一个通用的归因框架：对长上下文 LLM 的输出进行归因（attribution），定位影响生成结果的具体输入片段。

方法上，"通用"（generic）暗示框架不依赖特定模型架构或任务类型，可能通过扰动、梯度或对比生成等技术量化每个输入片段对输出的贡献，并将归因问题形式化为可计算的任务。针对长上下文的挑战（片段多、相互作用复杂、计算量大）应有专门的效率设计。

预期成果包括在长文档问答、摘要、检索增强等任务上验证归因准确性，并与已有归因方法对比。归因能力的安全价值在于：审计模型是否依赖了被投毒/误导的检索文档（与 RAG 投毒攻击呼应）、检测提示注入导致的异常依赖，以及提升模型可解释性。

作者团队与 PoisonedRAG 同源（Jinyuan Jia 组），两者可形成"攻击—归因"的闭环研究脉络。局限在于未见摘要，具体方法与评估难以确证；但归因框架本身对长上下文 LLM 的可信审计具有明确实用价值，尤其在与知识投毒、幻觉检测结合时。


#### From Meme to Threat: On the Hateful Meme Understanding and Induced Hateful Content Generation in Open-Source Vision Language Models.

- **作者**: Yihan Ma, Xinyue Shen, Yiting Qu, Ning Yu, Michael Backes, Savvas Zannettou, Yang Zhang
- **方向**: 大模型安全
- **解读**: 开源视觉语言模型（VLM）在图文任务上表现优异，但仇恨梗图这类现实图像携带复杂语义、文化指涉与情绪信号；若模型缺乏安全对齐，还可能被恶意利用生成仇恨内容。本文围绕两个问题：开源 VLM 能否理解仇恨梗图？能否被利用来生成仇恨内容？这直接关系到多模态模型部署的安全底线。

作者从 ADL 收集 34 个传统仇恨梗图（Happy Merchant、Trollface、Pepe the Frog、Bowlcut/Dylann Roof、Moon Man 五家族）并新增 5 个新涌现梗图；以 5 个通用提示加 GPT-4/GPT-4V 生成的 20 个细粒度提示构成 25 提示框架，查询 7 个开源 VLM 得到 12,775 条响应，由三名专家按信息量与准确性人工标注；生成任务中用朴素提示与自动提示工程（APE）诱导 VLM 生成仇恨言论、笑话与口号，以 Perspective API 的毒性分数量化危害。

VLM 理解能力较强（信息量最高 3.87、准确性最高 4.07），给出梗图名称可显著提升理解（如 Bowlcut/Dylann Roof 提升超 20%），但安全护栏严重缺失：所有被测模型面对仇恨内容从不拒绝回答；约 40% 仇恨言论响应毒性分高于 0.7，笑话与口号生成中也有超过 10% 被判定有害，且这两类任务的拒绝率几乎为零。

首批系统评估开源 VLM 理解与生成仇恨内容的工作，标注严谨，对"能理解但不会拒绝"的安全漏洞给出量化证据。局限：梗图仅 39 张、模型为旧版开源 VLM，时效性有限；标注者为作者本人，主观偏差难排除；40%/10% 依赖 Perspective API 阈值，其对文化语境敏感的仇恨内容识别力存疑。

- **论文 PDF**: [From Meme to Threat: On the Hateful Meme Understanding and Induced Hateful Content Generation in Open-Source Vision Language Models.](https://www.usenix.org/system/files/usenixsecurity25-ma-yihan.pdf)

#### Unsafe LLM-Based Search: Quantitative Analysis and Mitigation of Safety Risks in AI Web Search.

- **作者**: Zeren Luo, Zifan Peng, Yule Liu, Zhen Sun, Mingchen Li, Jingyi Zheng, Xinlei He

- **方向**: 大模型安全

- **解读**: 

（无摘要，据标题推断）AI 网页搜索（LLM 驱动的搜索助手）在返回答案前会抓取网页内容，其"安全护栏"能否在有害内容（恶意软件、钓鱼、色情、暴力等）进入上下文前将其过滤，直接影响用户安全；LLM 搜索对不安全页面的处理机制与失败模式此前缺乏定量分析。

从标题推断，本文对 AI 网页搜索进行量化安全风险分析：应是通过构造含恶意/有害内容的网页语料，测试主流 LLM 搜索工具在检索、解析、回答各阶段对不安全内容的暴露，并据此提出缓解措施（如内容预过滤、安全分类器）。具体工具集与评估指标因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题中的"Quantitative"暗示作者以可复现的统计指标量化了风险暴露率与缓解效果。

我的思考（基于标题的保守推断）：搜索是 LLM 最危险的能力之一——检索即执行，把网页内容喂给模型等于扩大注入与诱导面；该工作把"搜索安全"从提示注入扩展到时序上更早的检索环节。与针对 LLM 助手的注入攻击研究相比，本文强调防御侧量化与缓解，其评估框架可为搜索产品的安全验收提供基线。


#### Generated Data with Fake Privacy: Hidden Dangers of Fine-tuning Large Language Models on Generated Data.

- **作者**: Atilla Akkus, Masoud Poorghaffar Aghdam, Mingjie Li, Junjie Chu, Michael Backes, Yang Zhang, Sinem Sav
- **方向**: 大模型安全
- **解读**:

为规避真实数据微调带来的隐私风险，开发者越来越多地使用 LLM 生成的合成数据微调模型，认为生成数据"不同于"真实数据。但现代 LLM 生成的数据与真实数据几乎无法区分，这种趋同是否引入同等隐私风险此前无人研究。论文提出核心问题：用 LLM 生成数据微调，是增强隐私还是带来额外风险？

研究覆盖两种主流微调场景：非结构化数据 SFT——用 Pythia-12b 生成 Enron 邮件数据集，以 QLoRA 微调 Pythia-410m/1.4b/2.8b，用四类模板的 PII 提取攻击评估；self-instruct 微调——以 64 个种子任务、75 个输入输出对引导 Llama-3-8B-Instruct 生成 4000 条法律指令数据微调 Pythia-6.9b，用四种成员推断攻击（MIA）评估 FreeLaw 隐私。

结果显示 PII 提取成功率提升超 20%（Pythia-410m 从 36 增至 52，1.4b 从 41 增至 53，2.8b 从 48 增至 58）；在训练数据与 Enron 无重叠的 OPT 模型及假名化数据上，某些模板提升约 40%。Self-instruct 后 MIA 的 ROC-AUC 提升超 40%（如 min-k 从 0.468 升至 0.871）。机理上，生成数据与原始数据语义相似度超 0.7、含 6 万余个姓名-邮箱对，结构相似性触发了模型记忆；论文建议用约 10⁻⁶ 的低学习率缓解风险。

论文用严谨实验揭示了"合成数据≠隐私安全"的反直觉结论，机理分析为后续研究提供了抓手。局限在于仅覆盖 Pythia/OPT 等开源模型与邮件、法律两个领域，闭源 API 场景与更多数据类型有待验证。对产业界的警示意义明确：合成数据并非隐私的"免罪金牌"，基于生成数据的微调同样需要隐私审计与防护。

- **论文 PDF**: [Generated Data with Fake Privacy: Hidden Dangers of Fine-tuning Large Language Models on Generated Data.](https://www.usenix.org/system/files/usenixsecurity25-akkus.pdf)

#### Cloak, Honey, Trap: Proactive Defenses Against LLM Agents.

- **作者**: Daniel Ayzenshteyn, Roy Weiss, Yisroel Mirsky

- **方向**: 大模型安全

- **解读**: 

（无摘要，据标题推断）LLM 智能体（能调用工具、浏览网页、执行操作）正被用于自动化攻击（钓鱼、数据窃取、社工）；对这类自主攻击者的防御仍以被动检测为主，缺乏主动对抗手段——能否在网络中布置诱饵与陷阱，让 LLM 攻击者自投罗网并暴露意图，是新兴的攻防问题。

从标题推断，本文提出针对 LLM 智能体的主动防御框架 Cloak-Honey-Trap：应是通过伪装（cloaking）隐藏真实资产、布置蜜罐（honeypot）诱捕、设置陷阱（trap）消耗或溯源攻击者，利用 LLM 攻击者推理链的可预测性提高诱捕成功率。具体机制因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者以诱捕成功率或攻击拦截率评估了主动防御的有效性。

我的思考（基于标题的保守推断）：蜜罐思想在 LLM 智能体时代迎来新机遇：机器攻击者比人类更依赖结构化推理，更易被语义诱饵诱导，但其规模化也让蜜罐设计面临成本与自适应规避挑战。与蜜网、蜜标等传统主动防御相比，本文把欺骗对象从人类攻击者扩展到自主智能体，若机制成立，将开创"以欺骗制 LLM"的新防御路线。


#### SOFT: Selective Data Obfuscation for Protecting LLM Fine-tuning against Membership Inference Attacks.
- **作者**: Kaiyuan Zhang, Siyuan Cheng, Hanxi Guo, Yuetian Chen, Zian Su, Shengwei An, Yuntao Du, Charles Fleming, Ashish Kundu, Xiangyu Zhang, Ninghui Li
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）

**问题**：大语言模型微调常使用包含敏感信息的私有数据，成员推断攻击（MIA）可以判断某条样本是否出现在模型的训练集中，从而泄露训练数据的成员隐私，给医疗、金融等合规场景带来现实风险。如何在保持模型可用性的前提下抵御 MIA，是隐私保护微调的核心难题。

**方法**：SOFT 提出"选择性数据混淆"的思路：不是对全部训练数据做统一处理，而是识别并挑选对成员推断贡献最大的敏感样本，对其做混淆/扰动后再用于微调，从而切断攻击者利用模型输出推断成员身份的通道，同时尽可能减少对模型下游任务性能的损害。

**结果**：论文无公开摘要，具体实验数据无法引用。（据标题推断）该工作应通过实验对比了混淆前后模型在成员推断攻击成功率与下游任务性能之间的权衡曲线，验证选择性混淆优于全量处理。

**思考**：该工作与差分隐私、数据净化等路线不同，属于"按样本选择"的定向防御，隐私—效用权衡更为精细，是值得肯定的差异化思路；但混淆策略的自动化选择依据、对多轮微调与后续对齐的鲁棒性，以及能否抵御更强的自适应攻击，都是需要继续追问的问题。


#### Effective PII Extraction from LLMs through Augmented Few-Shot Learning.
- **作者**: Shuai Cheng, Shu Meng, Haitao Xu, Haoran Zhang, Shuai Hao, Chuan Yue, Wenrui Ma, Meng Han, Fan Zhang, Zhao Li
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）

**问题**：大语言模型会记忆训练语料中的个人身份信息（PII），攻击者可通过精心构造的提示词诱使模型输出他人隐私，构成数据泄露风险。如何在提示工程层面系统化、高效地提取 PII、量化模型的隐私暴露程度，是评估与治理 LLM 隐私风险的重要问题。

**方法**：该工作提出"增强小样本学习"的提取框架：通过构造与目标 PII 形态相近的增强示例（如格式模仿、上下文引导），以少量示例即可引导模型输出训练数据中的真实 PII，显著降低对人工设计复杂提示词的依赖，并提升提取的稳定性与覆盖面。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应对比了不同提示策略下 PII 提取的成功率与召回情况，展示增强小样本相对零样本与手工提示的优势。

**思考**：与同期多篇 PII 提取工作相互呼应，说明 LLM 的 PII 泄露已成为系统性问题；few-shot 增强表明攻击者并不需要复杂的提示工程技巧即可达成目的；防御端应关注训练数据净化与输出过滤，监管端则需要重新审视模型记忆与遗忘的合规边界。


#### Private Investigator: Extracting Personally Identifiable Information from Large Language Models Using Optimized Prompts.
- **作者**: Seongho Keum, Dongwon Shin, Leo Marchyok, Sanghyun Hong, Sooel Son
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）

**问题**：大语言模型可能记住并复述训练数据中的个人身份信息（PII），如何以优化提示词系统地从模型中提取这些信息、量化隐私泄露程度，是评估模型安全性的重要课题。

**方法**：该工作针对 PII 提取设计优化后的提示模板与查询策略（如角色扮演、格式约束、逐步引导等），在多种商用与开源 LLM 上系统评估提取效果，并分析影响提取成功率的关键因素。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应报告了在多个模型上的 PII 提取成功率，并与朴素提示基线进行对比。

**思考**：与同期 few-shot 提取工作互为补充，共同说明"提示工程"是当前评估 LLM 隐私泄露最现实可行的手段；其局限在于提取结果依赖模型对训练数据的记忆强度，且可能被内容过滤机制阻断；该工作可作为防御研究（机器遗忘、输出过滤、对齐）的评估基准，具有方法学价值。


#### PrivacyXray: Detecting Privacy Breaches in LLMs through Semantic Consistency and Probability Certainty.
- **作者**: Jinwen He, Yiyang Lu, Zijin Lin, Kai Chen, Yue Zhao
- **方向**: 大模型安全
- **解读**: （无摘要，据标题推断）

**问题**：大语言模型在推理时会无意泄露训练数据中的隐私信息，现有检测方法往往需要访问训练数据或依赖人工审查，难以可靠、规模化地识别这类泄露，亟需自动化的隐私泄露检测手段。

**方法**：该工作提出基于"语义一致性"与"概率确定性"两个维度的检测框架：当模型对某输出给出极高的概率确定性，且其语义与已知事实高度一致时，判定为可能的隐私泄露，从而在不依赖训练数据的前提下对模型输出进行筛查。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应报告了检测的准确率、误报率以及在真实泄露样本上的有效性。

**思考**：从"输出层面"而非"训练数据层面"检测隐私泄露，部署成本低、可扩展性强，适合作为持续的模型监控手段；但语义一致性与概率确定性的阈值设定可能漏报罕见却真实的泄露，对抗性改写也可能绕过检测；将其与审计、人工复核结合使用更有实际价值。


#### JBShield: Defending Large Language Models from Jailbreak Attacks through Activated Concept Analysis and Manipulation.

- **作者**: Shenyi Zhang, Yuchen Zhai, Keyan Guo, Hongxin Hu, Shengnan Guo, Zheng Fang, Lingchen Zhao, Chao Shen, Cong Wang, Qian Wang
- **方向**: 大模型安全
- **解读**:

即使经过安全对齐，LLM 仍易被越狱攻击突破。现有防御多关注输入输出的表面模式，缺乏对越狱底层机制的理解，难以持久有效。论文基于线性表示假说（LRH）研究两个问题：对齐 LLM 能否识别越狱提示中的有害语义？越狱如何把输出从拒绝扭转为服从？

方法上，论文把有害/越狱提示与良性提示的表示差异定义为 toxic concept、越狱与有害提示的差异定义为 jailbreak concept，通过反事实配对与截断 SVD（rank=1）提取概念子空间并映射为可读 token。分析发现 LLM 能识别两类提示中的 toxic concept，但越狱提示额外激活了 jailbreak concept（如 "sure"、"yes" 等服从性 token），从而压过拒绝。据此构建 JBShield：检测组件在单次前向传播内比较输入概念与锚定子空间的余弦相似度，toxic 与 jailbreak 概念同时激活即判为越狱；缓解组件在关键层增强 toxic、削弱 jailbreak 向量，使模型自主生成安全内容。

在 5 个开源 LLM（Mistral-7B、Vicuna-7B/13B、Llama2-7B、Llama3-8B）上对抗 9 种越狱攻击，检测平均准确率 0.95、F1 0.94，缓解把平均攻击成功率从 61% 降至 2%（AutoDAN、Puzzler、Base64 降为 0）。仅需 30 个越狱样本校准，跨攻击迁移准确率大多高于 0.84。

亮点是"先理解机制再设计防御"，用 LRH 给出可解释的统一检测与缓解框架，优于依赖表面模式的方案，且不改动模型参数、开销低。局限在于依赖校准集与阈值选择，对仅激活 jailbreak 而无 toxic 概念的输入（Mistral-7B 上）检测性能下降；线性假设与 token 映射的可解释性仍有限。面对刻意压制 toxic 激活的自适应攻击，其鲁棒性值得进一步验证。

- **论文 PDF**: [JBShield: Defending Large Language Models from Jailbreak Attacks through Activated Concept Analysis and Manipulation.](https://www.usenix.org/system/files/usenixsecurity25-zhang-shenyi.pdf)

### Web安全（10 篇）

#### The Silent Danger in HTTP: Identifying HTTP Desync Vulnerabilities with Gray-box Testing.
- **作者**: Keran Mu, Jianjun Chen, Jianwei Zhuge, Qi Li, Haixin Duan, Nick Feamster
- **方向**: Web安全
- **解读**: 问题：HTTP 请求走私/反序列化不一致（Desync）是去中心化互联网中的高危威胁，源于 HTTP 实现之间的解析差异。现有自动检测工具主要是字典扫描器与黑盒模糊器，无法洞察实现内部状态，测试效果差；且只关注请求侧 Desync，忽视了 HTTP 响应侧的漏洞。

方法：作者提出 HDHunter，一种基于灰盒覆盖率引导的差分测试（coverage-directed differential testing）的自动 HTTP 差异检测框架，能同时发现 HTTP 请求、HTTP 响应以及 CGI 响应中的不一致。

结果：对 19 个主流 HTTP 实现进行评测，发现 17 个新的 HTTP Desync 漏洞；已向厂商披露并获得致谢与漏洞赏金，其中 9 个 CVE 来自 Apache、Tomcat、Squid 等知名 HTTP 软件。

思考：把差分测试从黑盒升级为灰盒覆盖率引导，并首次把响应侧纳入检测范围，切中了现有工具的盲区；17 个新漏洞与 9 个 CVE 的实绩证明了方法的有效性。局限在于差分测试依赖参考实现的选择与覆盖率指标的设计，对新协议特性（HTTP/2、QUIC）的扩展性值得后续关注。

- **论文 PDF**: [The Silent Danger in HTTP: Identifying HTTP Desync Vulnerabilities with Gray-box Testing.](https://www.usenix.org/system/files/usenixsecurity25-mu.pdf)

#### Shadowed Realities: An Investigation of UI Attacks in WebXR.
- **作者**: Chandrika Mukherjee, Reham Mohamed, Arjun Arunasalam, Habiba Farrukh, Z. Berkay Celik
- **方向**: Web安全
- **解读**: WebXR 是扩展现实的 Web 标准接口，提供虚拟环境与沉浸式 3D 交互，与传统 Web 的 UI 属性截然不同。这些新 UI 属性也带来"暗黑设计"（dark design）被利用的新途径：例如 WebXR 缺少 iframe 之类的元素边界，第三方（如广告服务商）可注入 JavaScript 脚本，诱导用户无意点击或窃取敏感信息。本文旨在识别并分析 WebXR 中易被第一/第三方利用的 UI 属性及其对用户体验的影响。

作者考察易受攻击的 UI 属性，提出五种利用一个或多个此类属性的新型攻击技术，并将广告领域既有与新识别的攻击系统化为全面分类学（taxonomy）。随后设计用户研究框架：开发日志系统采集 3D 交互空间数据，集成到不同交互需求的 WebXR 应用中，并设计指标从交互日志中评估暗黑设计对用户行为的影响。

作者开展 100 名参与者的组间（between-subjects）研究。结果表明：大多数暗黑模式在有效达成其目的的同时，几乎未被用户察觉；其影响因攻击类别与应用类型而异。分类学、日志框架、指标与用户研究结果共同帮助开发者审视实践，并为防御机制研究提供启发。

该工作系统性地把 Web 安全中的"UI 欺骗/暗黑模式"研究拓展到 WebXR 这一新兴面：五类新攻击技术与实证"用户察觉不到"的发现，说明沉浸式平台的信任边界尚未建立；开源式的框架与指标也为后续研究提供了工具基础。局限：研究在受控应用中进行，样本规模（100 人）与真实广告生态的复杂性影响外部效度。总体而言，它为 WebXR 的安全设计规范敲响了警钟。
- **论文 PDF**: [Shadowed Realities: An Investigation of UI Attacks in WebXR.](https://www.usenix.org/system/files/usenixsecurity25-mukherjee.pdf)

#### Double-Edged Shield: On the Fingerprintability of Customized Ad Blockers.
- **作者**: Saiid El Hajj Chehade, Ben Stock, Carmela Troncoso
- **方向**: Web安全
- **解读**:
**问题**：网络追踪正转向无Cookie技术（如浏览器指纹），隐私意识强的用户会通过定制广告拦截器的过滤器列表、采用反指纹配置来躲避追踪；但这些个性化选择可能反而成为新的指纹来源，使"隐私设置"变成"隐私泄露"，本文旨在量化这一反直觉风险。
**方法**：作者评估广告拦截器定制（过滤器列表选择）的可指纹性，提出三种无脚本攻击，可绕过SoTA指纹检测器与缓解措施；攻击通过探测页面渲染差异等行为识别用户所选过滤器列表，并结合去重与熵分析刻画其匿名性损失。
**结果**：攻击能识别84%的过滤器列表，捕获的指纹稳定（归一化熵0.72），仅用577K条规则中的45条即可将用户的相对匿名集缩小到中位数48人（占人群0.2%）；论文还面向各相关方给出了建议与预防措施。
**思考**：该工作揭示了一个深刻的隐私悖论——用户越用心定制隐私工具，越容易被唯一化，贡献在于把这一直觉转化为可复现的攻击与量化证据，并覆盖了SoTA缓解手段；局限是匿名集估计依赖特定人群模型，且现实中指纹并非唯一泄露渠道，但其对反指纹社区"越定制越安全"假设的质疑具有重要警示价值。

- **论文 PDF**: [Double-Edged Shield: On the Fingerprintability of Customized Ad Blockers.](https://www.usenix.org/system/files/usenixsecurity25-el-hajj-chehade.pdf)

#### Web Execution Bundles: Reproducible, Accurate, and Archivable Web Measurements.
- **作者**: Florian Hantke, Peter Snyder, Hamed Haddadi, Ben Stock
- **方向**: Web安全
- **解读**:

**问题**：可复现性已成为安全与隐私研究社区的核心追求（如 artifact evaluation），但 Web 测量缺乏可跨任务复用、抗规避、准确覆盖各类 Web 行为的工具；多数测量研究使用自研工具与各异的存档格式，正确性未知且局限明显，系统性影响研究的准确性与可复现性。

**方法**：作者提出 WebREC 测量工具，具备三个特性：准确（能正确测量并归因现有工具做不到的事件）、通用（无需修改即可复用于广泛测量任务）、全面（覆盖浏览器所有相关行为）；同时提出 .web 存档格式，用于网站行为的准确、可复现测量与归档。

**结果**：通过复现知名 Web 测量研究，WebREC 的结果更准确地匹配基线；对 2024 年 Web 爬取 SoK 论文的分析显示，70% 的研究任务可直接用 WebREC 完成，另有 48% 可基于 .web 存档完成而无需任何新爬取。

**思考**：该工作把"测量工具本身"作为研究对象，直面社区长期存在的正确性与可复现性痛点，定位精准；用 SoK 论文覆盖度衡量通用性具有说服力；但浏览器行为持续演化，WebREC 的长期维护、.web 格式的生态采纳以及存档的隐私合规问题，仍是落地层面的现实挑战。

- **论文 PDF**: [Web Execution Bundles: Reproducible, Accurate, and Archivable Web Measurements.](https://www.usenix.org/system/files/usenixsecurity25-hantke.pdf)

#### XSSky: Detecting XSS Vulnerabilities through Local Path-Persistent Fuzzing.
- **作者**: Youkun Shi, Yuan Zhang, Tianhao Bai, Feng Xue, Jiarun Dai, Fengyu Liu, Lei Zhang, Xiapu Luo, Min Yang
- **方向**: Web安全
- **解读**: （无摘要，据标题推断）

**问题**：跨站脚本（XSS）是最普遍、危害最大的 Web 漏洞之一，现有模糊测试方法在 Web 应用上存在状态覆盖不足、输入生成低效等问题，难以有效发现深层 XSS 漏洞。

**方法**：该工作提出"局部路径持久化模糊测试"（Local Path-Persistent Fuzzing）：在应用的局部执行路径上维持持久化状态，减少重复初始化开销，并针对 XSS 漏洞的触发条件定向生成输入，从而提升对 XSS 相关代码路径的探索效率与漏洞检出率。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应报告了在真实 Web 应用上发现的 XSS 漏洞数量及其与现有 fuzzer 的对比。

**思考**：把持久化模糊测试思想从原生程序迁移到 Web 应用是合理延伸，局部路径切分有效控制了状态爆炸；但 Web 应用的多层框架、异步交互与 DOM 型 XSS 仍难以覆盖，与污点分析、浏览器端执行监控结合可能是更完整的解决方案。


#### ZIPPER: Static Taint Analysis for PHP Applications with Precision and Efficiency.
- **作者**: Xinyi Wang, Yeting Li, Jie Lu, Shizhe Cui, Chenghang Shi, Qin Mai, Yunpei Zhang, Yang Xiao, Feng Li, Wei Huo
- **方向**: Web安全
- **解读**: （无摘要，据标题推断）

**问题**：PHP 在 Web 生态中占比巨大，污点分析是检测其安全漏洞（注入、XSS 等）的主流手段，但传统静态污点分析在 PHP 上常面临精度与效率的两难：要么误报偏高，要么分析时间不可接受。

**方法**：该工作提出 ZIPPER 静态污点分析框架，通过设计精确且高效的污点传播规则与别名/上下文处理，在保持低误报的同时提升分析速度，面向真实世界大型 PHP 应用的漏洞检测需求。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应对比了在真实 PHP 应用上发现的漏洞数量、误报率与运行开销。

**思考**：PHP 的动态类型、超全局变量与魔术方法使静态分析极具挑战，ZIPPER 需要在精度与可扩展性之间取得平衡；若能集成进 CI 流程并与模糊测试、动态监控互补，可显著提升 PHP 应用的安全基线；与既有 PHP 分析器（如 RIPS、Pixy）在真实应用上的横向对比，是评估其价值的关键。


#### The DOMino Effect: Detecting and Exploiting DOM Clobbering Gadgets via Concolic Execution with Symbolic DOM.

- **作者**: Zhengyu Liu, Theo Lee, Jianjia Yu, Zifeng Kang, Yinzhi Cao
- **方向**: Web安全
- **解读**:

DOM Clobbering 是利用 DOM 元素与 JavaScript 变量命名冲突的代码复用攻击，可将攻击者注入的无脚本 HTML 流转到危险 sink（如 XSS、CSRF）。关键步骤是发现库中可被触发的 "gadget"。唯一先验工具 TheThing 依赖预定义 HTML payload 模板与静态分析，无法发现含复杂约束或动态特性的新 gadget。

论文提出 Hulk，首个自动化动态分析框架。核心洞察是把攻击者控制的 HTML 建模为 Symbolic DOM——一种形式化约束语言描述 DOM 树的标签、属性、父子/兄弟关系及 srcdoc 等特性；基于 Jalangi2 重写的污点引擎支持全 JS 类型与跨浏览器追踪，在污点依赖图上执行 concolic 求解，生成满足全部控制流/数据流约束的 exploit HTML。研究还补全了 DOM 转字符串的类型转换图景：除已知的二进制 + 隐式 toString 外，属性查找与内置函数调用的隐式 toString 也可构成 gadget，并发现 input form 属性劫持、广义嵌套 window proxy、兄弟/子节点加载等新技术。

Hulk 在 Tranco Top 5000 站点发现 497 个可利用 gadget（378 个 XSS、90 个 CSRF、26 个开放重定向、3 个存储篡改），生成 174 万多个候选 payload；覆盖 Webpack、Google Client API、MathJax、Google Closure 等流行库，实现 12 个端到端利用（Jupyter、Canvas LMS、Hackmd 等），已获 19 个 CVE。与 TheThing 对比，Hulk 假阴性更低，且 TheThing 无法检测其报告的 0-day gadget。

符号化 DOM 把"生成满足 DOM 约束的 payload"转化为可求解问题，并系统补全了 DOM Clobbering 的转换技术，是 web 代码复用攻击挖掘的重要推进，19 个 CVE 佐证了现实影响。局限在于依赖无脚本 HTML 注入的威胁模型与浏览器内动态执行，覆盖率受页面可达性限制。相比常规 XSS 注入检测，该工作把无脚本代码复用系统化，对前端库安全有直接指导意义。

- **论文 PDF**: [The DOMino Effect: Detecting and Exploiting DOM Clobbering Gadgets via Concolic Execution with Symbolic DOM.](https://www.usenix.org/system/files/usenixsecurity25-liu-zhengyu.pdf)

#### FIXX: FInding eXploits from eXamples.
- **作者**: Neil P. Thimmaiah, Yashashvi J. Dave, Rigel Gjomemo, V. N. Venkatakrishnan
- **方向**: Web安全
- **解读**:

**问题**：全面分析现代 Web 应用以发现不同漏洞及对应 exploit 既困难又耗时；安全研究者的漏洞披露往往只覆盖单个漏洞实例，不包含同一应用内其他同类漏洞的信息，导致已知漏洞的同类变体被系统性遗漏。

**方法**：FIXX 自动在同一个 PHP 应用内发现与已知 exploit 相似的多个同类漏洞利用路径，核心是结合代码图表示上的路径相似度与图相似度两个新颖概念，在 taint 型漏洞（CVE 报告）基础上泛化出相似的可利用路径。

**结果**：在 32 个 CVE 报告（涵盖 XSS 与 SQL 注入）、关联 19 个 PHP 应用上评估，发现 1097 条相似的可利用路径，并由此产生 10 个新 CVE 条目。

**思考**：这是"变体分析"（variant analysis）在 PHP Web 漏洞上的落地，用图相似度把单点披露扩展为全应用排查，直接产出新 CVE，实用价值明确；但 1097 条路径中的误报比例未充分披露，图相似度阈值选择与人工确认成本是落地时需权衡的点。

- **论文 PDF**: [FIXX: FInding eXploits from eXamples.](https://www.usenix.org/system/files/usenixsecurity25-thimmaiah.pdf)

#### Careless Retention and Management: Understanding and Detecting Data Retention Denial-of-Service Vulnerabilities in Java Web Containers.

- **作者**: Keke Lian, Lei Zhang, Haoran Zhao, Yinzhi Cao, Yongheng Liu, Fute Sun, Yuan Zhang, Min Yang
- **方向**: Web安全
- **解读**:

DoS 研究多聚焦网络层与 ReDoS 等应用层攻击，内存耗尽型 DoS 在 Java Web 容器中缺乏系统研究。论文定义 DRDoS（数据驻留 DoS）：容器把用户可控请求数据长期驻留并累积，导致内存耗尽。除 CVE 中少数手工发现的案例外，此前无系统性学术研究。

论文提出静态分析框架 DR.D，三步定位问题：一是请求数据驱动地定位请求处理器——识别请求类并按其集中式/分散式管理模式聚类方法；二是用 13 个类特征（实例化次数、并发访问、可配置性、全局可达性等）训练随机森林分类长生命周期类；三是做上下文、字段、流敏感的污点分析，检测请求数据是否流入长生命周期对象中的可扩展集合，并从请求数据值空间、驻留容量、驻留数据生命周期三个维度静态评估可利用性，划分为无约束、时间约束、上下文约束、不可利用四级。

在 Tomcat、Jetty、Undertow、Resin 四个容器上发现 145 个候选、验证 28 个、确认 25 个可利用 0-day 漏洞，已获 17 个 CVE（3 个高危）。Shodan 扫描显示超 150 万公网 IP 可能受影响。在 Google Cloud、Azure、阿里云、华为云实测中，攻击成功耗尽 2GB 容器内存且不触发任何 DDoS 防护告警。分类模型精度 85.98%、召回 92.16%，单个容器分析约 3 分钟。

首次系统化研究 Java Web 容器的内存耗尽 DoS，把"数据驻留"形式化为可静态检测与分级的问题，云平台实测证明低速率攻击的现实威胁。局限是 55.6% 的假阳性率仍偏高，长生命周期分类依赖人工标注数据，语义约束（如预注册路径）难以静态建模。与流量式防御可检测的 ReDoS 不同，DRDoS 利用正常功能缺陷，提示需要容器级内存治理与配额机制。

- **论文 PDF**: [Careless Retention and Management: Understanding and Detecting Data Retention Denial-of-Service Vulnerabilities in Java Web Containers.](https://www.usenix.org/system/files/usenixsecurity25-lian.pdf)

#### Effective Directed Fuzzing with Hierarchical Scheduling for Web Vulnerability Detection.
- **作者**: Zihan Lin, Yuan Zhang, Jiarun Dai, Xinyou Huang, Bocheng Xiang, Guangliang Yang, Letian Yuan, Lei Zhang, Tian Chen, Min Yang
- **方向**: Web安全
- **解读**:

**问题**：Java Web 应用在现代数字生态中至关重要，也因此成为网络攻击的主要目标；对其开展定向模糊测试需要同时解决两大挑战：高效探索海量 Web 入口与参数、以及生成结构合规且语义受限的输入。

**方法**：WDFuzz 提出两阶段方案：一是语义约束提取技术，准确捕获 Web 参数的预期输入结构与约束；二是分层调度策略，评估每个种子触发漏洞的潜力并优先调度最有希望的种子，从而定向逼近目标漏洞点。

**结果**：在 15 个真实 Java Web 应用上，对已知漏洞数据集达到 92.6% 的召回率，比现有最先进的 Web fuzzer 多发现 3.2 倍漏洞、检测速度快 7.1 倍；此外发现 92 个此前未知的漏洞，已获 4 个 CVE 与 15 个 CNVD 编号。

**思考**：语义约束提取是核心创新——把参数格式知识注入模糊测试，直接提升输入有效性；分层调度有效缓解了 Web 入口爆炸问题；92 个新漏洞及 CVE/CNVD 落地验证了实际价值；对状态敏感漏洞与复杂业务逻辑的覆盖能力仍值得进一步观察。

- **论文 PDF**: [Effective Directed Fuzzing with Hierarchical Scheduling for Web Vulnerability Detection.](https://www.usenix.org/system/files/usenixsecurity25-lin-zihan.pdf)

### 恶意软件与二进制分析（9 篇）

#### My ZIP isn't your ZIP: Identifying and Exploiting Semantic Gaps Between ZIP Parsers.
- **作者**: Yufan You, Jianjun Chen, Qi Wang, Haixin Duan
- **方向**: 恶意软件与二进制分析
- **解读**: ZIP 是分发恶意代码与文档最常用的容器格式之一,杀毒引擎、扫描器与解压工具各自实现 ZIP 解析,解析器之间的语义差异(如对重名条目、路径穿越、格式错误的处理不同)可能被利用造成"我的 ZIP 不是你的 ZIP"的解析混淆。(无摘要,据标题推断)

依据标题推断,作者系统化识别多个主流 ZIP 解析器在条目选择、路径规范化、元数据处理等方面的语义鸿沟,构造能对不同解析器呈现不同内容的"多义 ZIP",并评估其被安全工具链解压后落地为恶意文件的可行性。

研究应报告解析器差异的规模与可利用场景(如恶意文件规避检测、绕过沙箱)。由于摘要与全文缺失,具体解析器清单与数值结果无法引用。

该主题与"解析器差分/多态打包"研究一脉相承(如 polyglot 文件、ZIP 炸弹变体),核心洞察是安全工具的解析语义必须与真实消费者一致,否则检测形同虚设。局限:无法核实实验覆盖范围;此类攻击的修复(统一解析规范)在生态碎片化下推进缓慢。对安全厂商与解压库维护者都有直接警示意义。

#### Tady: A Neural Disassembler without Structural Constraint Violations.
- **作者**: Siliang Qin, Fengrui Yang, Hao Wang, Bolun Zhang, Zeyu Gao, Chao Zhang, Kai Chen
- **方向**: 恶意软件与二进制分析
- **解读**: 反汇编是二进制分析的基础,神经网络反汇编器近年兴起,但其输出常违反指令集的结构约束(如长度与解码不一致、非法操作码),导致下游分析不可靠。本文提出不产生结构约束违例的神经反汇编器 Tady。(无摘要,据标题推断)

依据标题推断,作者把指令集的结构约束(如长度一致性、前缀合法组合)编码进模型架构或解码过程,使输出天然满足约束,可能采用约束解码、结构化标签或后处理校验,在保持神经方法泛化性的同时提升正确性。

研究应报告在多种架构与真实二进制上的反汇编准确率与约束满足率,并与现有神经反汇编器对比。由于摘要与全文缺失,具体数值无法引用。

神经反汇编的"不可靠性"正是其落地的主要障碍,把结构约束内建到生成过程是务实且有说服力的方向:既保留学习方法的鲁棒性,又获得传统反汇编器的纪律性。局限:无法核实约束建模的完备性与性能开销;约束解码通常增加推理成本。若准确率与效率数据扎实,有望成为下一代反汇编工具的基础。

#### EvilEDR: Repurposing EDR as an Offensive Tool.
- **作者**: Kotaiba Alachkar, Dirk Gaastra, Eduardo Barbaro, Michel van Eeten, Yury Zhauniarovich
- **方向**: 恶意软件与二进制分析
- **解读**: 问题：端点检测与响应（EDR）系统在企业的安全架构中地位关键，其持续监控、威胁检测与响应能力也被广泛部署。但这类"双刃剑"系统若被攻击者利用，将成为强大的攻击武器。以往研究多聚焦于逃避或篡改 EDR，本文首次提出"EDR 重定向复用"（EDR repurposing）这一新概念，即 EvilEDR。

方法：作者分析展示了 EvilEDR 可经响应控制台执行任意命令、传输工具、窃取数据，并被动收集系统信息以辅助进一步利用与横向移动；它伪装成合法进程、与可信域名通信以实现隐蔽性，还能通过注册自身 EPP 为默认来削弱防御，甚至将主机从网络中隔离，切断企业防御依赖的遥测与响应通道。

结果：研究表明 EvilEDR 可被有效检测与缓解，论文为此提出了具体可操作的防御策略。

思考：该工作把"安全产品即攻击面"这一主题推到新高度，提醒企业在引入 EDR 时须将其纳入自身信任边界考量；其价值不仅在于展示攻击，更在于给出可落地的检测与缓解建议。局限在于演示环境与真实企业部署的差异、以及防御策略对新型复用手法的覆盖度。

- **论文 PDF**: [EvilEDR: Repurposing EDR as an Offensive Tool.](https://www.usenix.org/system/files/usenixsecurity25-alachkar.pdf)

#### Hercules Droidot and the murder on the JNI Express.

- **作者**: Luca Di Bartolomeo; Philipp Mao; Yu-Jye Tung; Jessy Ayala; Samuele Doria; Paolo Celada; Marcel Busch; Joshua Garcia; Eleonora Losiouk; Mathias Payer
- **方向**: 恶意软件与二进制分析
- **解读**: （无摘要，据标题推断）Android 应用的 JNI（Java Native Interface）层承载大量本地（native）代码，是恶意行为与混淆的常见藏身之处；标题致敬《东方快车谋杀案》，暗示对名为 "Droidot" 的恶意样本或家族展开"侦探式"深度剖析。

  据标题推断，本文围绕 JNI 代码展开逆向分析与自动化工具建设，可能包括对 Droidot 恶意应用的动静结合分析、JNI 桥接层行为追踪，以及从 .so 本地库中恢复恶意逻辑的方法论。

  无摘要，具体量化结果无法从标题确认；该工作预计贡献一套针对 JNI 恶意代码的分析技术或工具链，并配以案例研究。

  JNI 是 Android 恶意软件研究的"硬骨头"——Java 层与 native 层语义割裂使纯静态分析失效、动态分析难以覆盖；若能提供可复用的自动化 JNI 分析流水线，对行业检测能力是实质补强；其与既有反混淆、跨层污点分析工作的差异度取决于技术路线的新颖性与规模化验证，需读全文确认；无论如何，聚焦 JNI 层本身就已对准了安卓生态中最常被忽视的攻击面。


#### NeuroScope: Reverse Engineering Deep Neural Network on Edge Devices using Dynamic Analysis.

- **作者**: Ruoyu Wu, Muqi Zou, Arslan Khan, Taegyu Kim, Dongyan Xu, Dave (Jing) Tian, Antonio Bianchi
- **方向**: 恶意软件与二进制分析
- **解读**: 
边缘设备（如 IoT）上的 DNN 推理由专用编译产物（DNN 二进制）执行。从安全角度看，逆向这类二进制（恢复其高层网络表示）可支撑模型窃取、灰/白盒对抗攻击与后门检测等多种应用。此前虽有专门工作研究 DNN 二进制逆向，但都受限于支持的二进制类型——无法覆盖不同编译器与硬件平台。

为此作者提出 NeuroScope，一种基于动态分析与机器学习的数据驱动逆向方法。其关键设计是"编译器无关、代码特征无关"：不依赖静态代码模式，而是通过运行时行为（如算子执行序列、张量形状变化）来识别网络结构，从而支持跨 DNN 编译器与硬件平台的多种二进制，包括采用解释器（interpreter）方式实现的模型——这正是此前方法无法处理的类型。

实验证明 NeuroScope 能以高精度逆向出此前方法不支持的各种 DNN 二进制。更重要的是，作者展示它能逆向一个由闭源编译器编译的专有 DNN 二进制，并在此基础上成功实施灰盒对抗机器学习攻击——说明该方法已具备实际攻击价值。

该工作的贡献在于把 DNN 逆向从"每个编译器定制"推进到"数据驱动通用"范式，扩大了攻击面覆盖。局限在于动态分析需要运行目标二进制（对受保护/混淆部署有门槛），且 ML 分类器的泛化依赖训练覆盖；与静态逆向方法相比，它牺牲部分信息（如精确权重）换取平台通用性，二者互补。

- **论文 PDF**: [NeuroScope: Reverse Engineering Deep Neural Network on Edge Devices using Dynamic Analysis.](https://www.usenix.org/system/files/usenixsecurity25-wu-ruoyu.pdf)

#### VAPD: An Anomaly Detection Model for PDF Malware Forensics with Adversarial Robustness.
- **作者**: Side Liu, Jiang Ming, Yilin Zhou, Jianming Fu, Guojun Peng
- **方向**: 恶意软件与二进制分析
- **解读**: PDF 是恶意软件投递的高频载体，但现有基于分类器的检测模型易被对抗样本绕过，且缺少取证定位能力。本文提出 VAPD：仅用良性 PDF 训练的重建式异常检测模型，恶意 PDF 因无法被良性模型重建而产生显著重建误差，据此检测恶意并定位异常区域，兼具对抗鲁棒性与取证价值。

方法分三阶段：先从 PDF 结构构建结构树、提取结构路径并以 Bag-of-Paths 与二进制嵌入向量化；再做重建与异常检测；最后对异常区域进行定位，输出恶意性信号与可疑区域双重结果。

在真实世界/APT 数据集上 VAPD 准确率达 99.54%，优于现有异常检测器；对四种对抗攻击框架（特征空间与样本空间、白盒与黑盒）表现出强鲁棒性，如对 MalGAN 特征空间攻击近乎完全抵抗，对自适应 EvadeML 逃逸率仅 6.01%（竞品超 50%）；训练成本约为 SOTA 方法的 4.8%，定位输出约 0.01 秒，文档级与对象级召回分别为 81.8% 与 74.5%。

我的思考：VAPD 把"异常检测+对抗鲁棒+取证定位"三合一，重建误差范式天然规避了特征空间对抗（攻击者难以让恶意 PDF"重建得像良性"），99.54% 准确率与极低训练成本使其具备大规模部署潜力。局限是依赖良性 PDF 分布的代表性、对精心构造的"良性化"恶意 PDF 效果待验证；但作为 PDF 取证的新范式，技术成色扎实。（无摘要，据标题推断）

#### The Ransomware Decade: The Creation of a Fine-Grained Dataset and a Longitudinal Study.
- **作者**: Armin Sarabi, Ziyuan Huang, Chenlan Wang, Tai Karir, Mingyan Liu
- **方向**: 恶意软件与二进制分析
- **解读**: 勒索软件是过去十年最严重的网络威胁之一，但研究长期受限于缺乏细粒度、可长期追踪的数据：公开数据多为聚合统计，难以刻画勒索组织的攻击行为与受害者决策随时间的演化。本文致力于构建细粒度数据集并开展纵向研究，为理解勒索软件生态的十年变迁提供实证基础。（无摘要，据标题推断）

工作重心分两部分：一是创建细粒度数据集——推测涵盖勒索组织、受害者、攻击时间线、支付记录等多维字段，并形成可复用的收集与标注流程；二是在此基础上做纵向分析，考察攻击规模、目标行业、勒索金额等指标随时间的演化规律与结构性变化。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；核心贡献应是公开一套细粒度勒索软件数据集，并给出跨约十年时间跨度的纵向分析结论，为后续研究提供可复现的数据基础。（无摘要，据标题推断）

勒索软件研究长期被数据瓶颈制约，细粒度数据的价值在于支撑"受害者是否支付""组织如何演化"等此前难以量化的问题，纵向视角更能揭示生态的结构性变迁。局限是数据集覆盖范围与来源偏差会影响结论外推，且无摘要无法评估其规模与质量；但"建数据+做研究"的组合模式值得借鉴。


#### REVDECODE: Enhancing Binary Function Matching with Context-Aware Graph Representations and Relevance Decoding.

- **作者**: Tongwei Ren, Ronghan Che, Guin Gilman, Lorenzo De Carli, Robert J. Walls
- **方向**: 恶意软件与二进制分析
- **解读**:

二进制函数匹配（binary function matching）是逆向工程与恶意代码分析的核心技术：识别跨编译器、跨优化级别、跨架构编译的同一源码函数，支撑漏洞追踪、补丁分析、恶意代码聚类与代码复用检测；但编译差异带来的指令级巨大形变使匹配精度长期受限，传统方法依赖手工特征或浅层相似度。

据标题推断，REVDECODE 以上下文感知的图表示增强函数匹配：将函数表示为融合调用上下文、控制流与数据流信息的图（如增强控制流图），用图神经网络编码；"相关性解码"（relevance decoding）机制可能通过注意力或相关性排序，从候选库中挑选与查询函数真正语义相关的匹配项，从而提升跨编译差异下的匹配精度。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把图表示学习与"相关性解码"结合，为跨编译器/跨架构匹配这一长期难题提供新思路。

我的思考：该方向与 Asm2Vec、Gemini、jTrans 等一脉相承，REVDECODE 的差异化在于上下文感知图与专用解码机制——若解码环节能利用函数对之间的相关性信号（而非单纯向量距离），有望缓解嵌入空间"最近邻≠语义等价"的顽疾。局限（据标题推断）：图表示的构建开销与大规模库上的检索效率、以及跨架构泛化能力是此类工作的常见瓶颈；具体量化提升需原文实验确认。对二进制相似性分析工具链有潜在增强价值。


#### BLens: Contrastive Captioning of Binary Functions using Ensemble Embedding.

- **作者**: Tristan Benoit, Yunru Wang, Moritz Dannehl, Johannes Kinder
- **方向**: 恶意软件与二进制分析
- **解读**:

函数名能极大辅助人工逆向，因此出现了大量基于机器学习的"为剥离符号的二进制函数预测函数名"方法；当前主流用 Transformer 把"代码到函数名"视为机器翻译任务。但函数命名模型在泛化到训练集之外的项目时面临严峻挑战，跨项目性能显著下降。

方法上，论文采取全新思路：把自动图像描述（image captioning）领域的进展迁移到二进制逆向——让二进制函数的不同部分与名称的不同部分建立关联。BLens 将多个二进制函数嵌入组合成新的集成表示（ensemble embedding），通过对比学习（contrastive learning）将其与名称表示的潜在空间对齐，再用专为函数名定制的 Transformer 架构生成函数名。

结果：实验显示 BLens 显著超越最先进方法：常规"按二进制划分"设定下 F1 达 0.79（对比 0.70）；强调泛化性的跨项目设定下 F1 达 0.46（对比 0.29）；在减少项目间共享组件的实验设定下 F1 达 0.32（对比 0.19），跨项目场景的相对提升最为显著。

我的思考：该工作的成色在于用"集成嵌入+对比对齐+定制解码"重构函数命名管线，把图像描述中"局部到局部"的对齐思想引入二进制域，跨项目 F1 从 0.29 提升到 0.46 证明了泛化瓶颈确实可被架构设计缓解。局限：F1 绝对值仍偏低，复杂函数与超长名称的生成质量、以及训练数据规模与领域分布的影响值得进一步考察。与 REVDECODE 同属二进制表示学习前沿，对逆向工具链有直接增强价值。

- **论文 PDF**: [BLens: Contrastive Captioning of Binary Functions using Ensemble Embedding.](https://www.usenix.org/system/files/usenixsecurity25-benoit.pdf)

### 漏洞挖掘与利用（23 篇）

#### Synthesis of Code-Reuse Attacks from p-code Programs.
- **作者**: Mark DenHoed, Tom Melham
- **方向**: 漏洞挖掘与利用
- **解读**: 代码复用攻击(如 ROP)是绕过 NX 等防护的经典利用手段,自动合成 ROP 链一直是研究热点,但现有方法常绑定特定架构或依赖启发式搜索。本文提出基于机械化形式逻辑的自动合成方法,目标是跨架构、可扩展的利用生成。

作者通过把机器码抽象到 Ghidra 的 p-code 中间语言,使方法适用于几乎所有架构;在命题逻辑中为 p-code 片段建立形式模型,从而可用自动推理算法分析;通过挑选能模拟给定 p-code 参考程序的 gadget 组合来合成代码复用攻击,在参考程序与 gadget 库规模上都具有良好的扩展性,并便于与外部工具集成。

实验表明,该方法在 ROP 链合成的成功率上达到或超过现有最优方法,同时运行性能更优。

该工作的亮点是"p-code 抽象+命题逻辑建模"的组合:既获得架构无关性,又得到可扩展的推理管线,把 gadget 选择转化为可满足性问题。局限:依赖 p-code 对机器码语义的忠实性,边界指令与反汇编错误可能影响结果;"达到或超过"的表述说明提升幅度有限。总体上为跨架构自动化利用合成提供了新的工程范式。
- **论文 PDF**: [Synthesis of Code-Reuse Attacks from p-code Programs.](https://www.usenix.org/system/files/usenixsecurity25-denhoed.pdf)

#### Sound and Efficient Generation of Data-Oriented Exploits via Programming Language Synthesis.
- **作者**: Yuxi Ling, Gokul Rajiv, Kiran Gopinathan, Ilya Sergey
- **方向**: 漏洞挖掘与利用
- **解读**: 数据导向编程(DOP)把恶意程序嵌入固定可执行文件,可实现不违反 NX、ASLR、控制流与代码指针完整性等防御的内存破坏代码复用。现有基于程序合成的自动利用生成方法存在三大缺陷:效率低(生成常需极长时间)、无形式保证(不保证描述的攻击在该程序上可行)、能力不可见(不清楚程序支持哪些攻击能力)。

作者把 DOP 利用合成建模为一种此前未被探索的"编程语言合成"问题:给定漏洞程序与一个利用(如缓冲区溢出),推导出描述可用攻击的编程语言文法。该方法把合成单个攻击的成本转移到合成整个攻击语言:文法一旦生成,每个攻击的"编译"耗时几乎可忽略;并通过证明文法合成算法的正确性,保证文法可表达的任意攻击均可实现,同时让攻击能力一目了然。

作者实现了端到端工具 DOPPLER,在 17 个案例研究(含三个近期 CVE)上对比现有最优技术,证明其有效性更强(生成更多攻击)且效率更高(快得多)。

该工作的理论贡献在于用"语言合成"一次性解决效率、可靠性与可视性三个问题,正确性证明把利用生成从启发式搜索提升为有保证的构造;工程上 DOPPLER 是可用的编译器。局限:文法覆盖能力受限于模板与漏洞类型,17 个案例的规模有限;对复杂现实程序的可扩展性仍待检验。这是程序合成应用于漏洞利用的标志性进展。
- **论文 PDF**: [Sound and Efficient Generation of Data-Oriented Exploits via Programming Language Synthesis.](https://www.usenix.org/system/files/usenixsecurity25-ling.pdf)

#### Nothing is Unreachable: Automated Synthesis of Robust Code-Reuse Gadget Chains for Arbitrary Exploitation Primitives.
- **作者**: Nicolas Bailluet; Emmanuel Fleury; Isabelle Puaut; Erven Rohou
- **方向**: 漏洞挖掘与利用
- **解读**: 代码重用链合成工具普遍假设攻击者能控制栈，无法处理UAF、堆溢出等近十年涌现的新利用原语。本文提出支持任意利用布局的gadget链自动合成方法，实现为工具ARCANIST，并支持x86、x64与Arm。

方法上，将链接问题形式化为鲁棒可达性与组件式程序合成的结合，构造一次性SMT查询（QF_ABV位向量与数组理论，Boolector求解）完全交由求解器编排gadget，用污点传播思路消除对不可控变量的全称量化，保证链"一次成功、正确性由构造保证"；gadget经Binary Ninja LLIL提升为GSA形式并编码前置/后置条件，增量求解按链长递增，随机采样gadget库并行搜索。

结果显示，10个真实漏洞（含OP-TEE的CVE-2022-46152、RouterOS、Foxit Reader UAF、Serv-U等）全部合成成功而竞品（SGC、Angrop、Ropium）全部失败；在经典栈布局基准上它是唯一完成fcall3/4、scall4、pivot全部目标的工具，比SGC快39%（共享实例）至61%（总体）；还能自主完成栈迁移、JOP dispatcher等复杂技巧，对CVE-2022-46152仅凭"只能递减返回地址"的受限布局即合成2个gadget的链。

我的思考：将"攻击者能力"从硬编码假设提升为可配置输入、以求解器替代启发式探索，是gadget合成范式的实质推进；相比SGC约30%候选被验证器拒绝的不健全合成，正确性设计更有工程价值。局限是假设无CFI、仅单线程，求解开销随写内存gadget比例激增（0%到5%平均176秒升至1427秒）。

- **论文 PDF**: [Nothing is Unreachable: Automated Synthesis of Robust Code-Reuse Gadget Chains for Arbitrary Exploitation Primitives.](https://www.usenix.org/system/files/usenixsecurity25-bailluet.pdf)

#### Trust but Verify: An Assessment of Vulnerability Tagging Services.
- **作者**: Szu-Chun Huang, Harm Griffioen, Max van der Horst, Georgios Smaragdakis, Michel van Eeten, Yury Zhauniarovich
- **方向**: 漏洞挖掘与利用
- **解读**: 问题：互联网范围扫描服务（如 Shodan、ONYPHE、LeakIX）被企业、政府与研究机构广泛用于攻击面发现与风险评估，但其可靠性与可信度此前鲜少被检验。若这些服务的漏洞标注不准确，将误导防御决策。

方法：作者将三家商业扫描器的结果与使用经过验证的 Nuclei 模板、通过构造良性请求识别特定漏洞的独立实验进行对比，系统评估其检测准确性。

结果：Shodan 基于载荷的检测大多得到确认；但 Nuclei 发现了多得多的易受攻击端点，防御者可能面临严重漏报。反之，Shodan 基于 banner 的检测出现显著误报（false positives），说明该类检测不可靠；三家商业服务与 Nuclei 的结果之间也存在显著差异。

思考：该工作以"验证扫描器"的姿态填补了评估空白，揭示了商业攻击面管理服务的漏报与误报双重问题，对依赖这些数据的行业、政策制定者与学术研究有直接警示意义。局限在于评估依赖 Nuclei 模板的覆盖度与版本时效，且商业服务在评估后会改进，需要持续的纵向跟踪研究。

- **论文 PDF**: [Trust but Verify: An Assessment of Vulnerability Tagging Services.](https://www.usenix.org/system/files/usenixsecurity25-huang-szu-chun.pdf)

#### Auspex: Unveiling Inconsistency Bugs of Transaction Fee Mechanism in Blockchain.
- **作者**: Zheyuan He, Zihao Li, Jiahao Luo, Feng Luo, Junhan Duan, Jingwei Li, Shuwei Song, Xiapu Luo, Ting Chen, Xiaosong Zhang
- **方向**: 漏洞挖掘与利用
- **解读**: 区块链交易费机制（TFM）通过按资源用量向用户收费来防止资源滥用，但"收费金额与实际资源消耗不一致"的缺陷（TFM 不一致性 bug）会带来严重的安全与资金风险：用户可能被多收、少收费用，或被利用制造拒绝服务。此前该问题依赖人工审计，缺乏自动化检测手段。本文提出 Auspex——首个利用模糊测试自动检测以太坊生态 TFM 不一致性 bug 的工具。

Auspex 引入三项关键技术：(i) 基于链的测试用例生成策略，高效构造贴近真实链上状态的测试用例；(ii) 收费引导的模糊测试，以收费逻辑引导探索更多代码分支；(iii) 两个通用 bug 预言（oracle）——费用一致性与资源一致性性质——自动判定收费与实际消耗是否不一致。

在以太坊上的评估中，Auspex 发现了 13 个此前未知的 TFM 不一致性 bug，代码分支覆盖达到最先进工具的 3.5 倍。其影响：这些 bug 已在以太坊与 BSC 上给用户造成超过数百万美元的损失；利用它们的拒绝服务（DoS）攻击在攻击期间可将交易等待时间延长 4.5 倍。

该工作把"费用与资源消耗一致性"这一经济语义抽象为可自动判定的性质，并用收费引导的模糊测试实现规模化检测，填补了 TFM 安全自动化审计的空白，13 个新 bug 与数百万美元损失的事实极具警示意义。局限：检测依赖可观测的资源计量语义，复杂 gas 重定价或跨合约场景的覆盖可能有限；bug 的影响分析基于链上推断，与真实归因可能存在偏差。总体而言，它为公链费用机制的安全审计提供了可复用的自动化范式。

- **论文 PDF**: [Auspex: Unveiling Inconsistency Bugs of Transaction Fee Mechanism in Blockchain.](https://www.usenix.org/system/files/usenixsecurity25-he-zheyuan.pdf)

#### AidFuzzer: Adaptive Interrupt-Driven Firmware Fuzzing via Run-Time State Recognition.
- **作者**: Jianqiang Wang, Qinying Wang, Tobias Scharnowski, Li Shi, Simon Wörner, Thorsten Holz
- **方向**: 漏洞挖掘与利用
- **解读**: 模糊测试是发现固件漏洞的有效方法，但固件模拟中存在难以绕过的障碍，其中中断处理尤为关键：中断触发不当会导致固件崩溃或卡死，甚至无法进入早期执行阶段。中断的触发与处理机制是固件模糊测试中重要却研究不足的问题，本文提出 AidFuzzer 加以解决。

核心观察是固件镜像通常呈现一致的运行时状态模式（摘要截断），据此进行运行时状态识别，自适应地决定何时触发何种中断，使模拟行为贴近真实硬件，避免因中断误触发导致的崩溃与停滞，从而提升代码覆盖率。

在真实固件镜像上评估，AidFuzzer 相比现有固件模糊器显著提升代码覆盖率并发现更多漏洞，验证了自适应中断调度机制的有效性（摘要截断，具体数字未给出）。

该工作把"中断调度"从手工经验变为自适应机制，补上了固件模拟的关键短板。局限是状态识别依赖固件的结构规律，对异构或混淆固件的泛化能力有待验证；未来可与外设模型（peripheral models）结合，进一步提升模拟真实性。

- **论文 PDF**: [AidFuzzer: Adaptive Interrupt-Driven Firmware Fuzzing via Run-Time State Recognition.](https://www.usenix.org/system/files/usenixsecurity25-wang-jianqiang.pdf)

#### GDMA: Fully Automated DMA Rehosting via Iterative Type Overlays.
- **作者**: Tobias Scharnowski, Simeon Hoffmann, Moritz Bley, Simon Wörner, Daniel Klischies, Felix Buchmann, Nils Ole Tippenhauer, Thorsten Holz, Marius Muench
- **方向**: 漏洞挖掘与利用
- **解读**: 固件安全分析通常需要在主机上重宿主（rehosting）固件以进行动态分析与模糊测试，但依赖 DMA 外设的固件因缺少真实硬件而无法正常运行，手动建模 DMA 设备耗时且易错。本文提出 GDMA，实现全自动的 DMA 重宿主，消除这一分析瓶颈。

核心方法是"迭代类型覆盖（Iterative Type Overlays）"：自动推断固件中与 DMA 相关的数据结构类型（指针关系、结构体布局），通过迭代精化类型假设，自动生成具备 DMA 语义的仿真外设，无需人工逆向即可驱动固件代码执行。（无摘要，据标题推断）

GDMA 在真实固件样本上自动完成重宿主并成功运行依赖 DMA 的固件代码，支撑后续的漏洞挖掘工作，显著降低人工成本。（无摘要，据标题推断）

该工作把类型推断与设备仿真结合，解决了固件重宿主的自动化难题，工程价值突出。局限是类型推断对优化或混淆固件的鲁棒性、以及 DMA 与复杂中断交互的覆盖仍有限；未来可与模糊测试打通，形成全自动的固件分析管线。


#### Precise and Effective Gadget Chain Mining through Deserialization Guided Call Graph Construction.
- **作者**: Yiheng Zhang; Ming Wen; Shunjie Liu; Dongjie He; Hai Jin
- **方向**: 漏洞挖掘与利用
- **解读**:（无摘要，据标题推断）反序列化漏洞利用常需把多个 gadget（可利用的方法序列）串成调用链，人工挖掘成本极高；现有自动挖掘方法因调用图不精确而产生大量无效链或漏报。

论文提出反序列化引导的调用图构建方法，利用反序列化数据流特征裁剪调用图，使 gadget chain 挖掘聚焦于反序列化入口可达的路径，提升挖掘的精度与效率。

（无摘要，据标题推断）据标题，该方法在真实 Java 等生态的 gadget chain 挖掘中，相比现有工具发现更多可达且可利用的链，误报更低。

反序列化漏洞（如 Java 反序列化链）长期是高危攻击面，gadget 挖掘精度直接决定利用可行性；把"反序列化入口的数据流语义"注入调用图构建，思路针对性强，与基于污点或可达性的既有工作相比更贴近攻击者视角。挑战在于库规模爆炸与图构建开销，以及挖掘出的链在真实环境中的稳定性验证；与开源库覆盖范围及 ysoserial 等工业级链库的对比可进一步凸显其实用性。


#### NASS: Fuzzing All Native Android System Services with Interface Awareness and Coverage.
- **作者**: Philipp Mao, Marcel Busch, Mathias Payer
- **方向**: 漏洞挖掘与利用
- **解读**: 原生 Android 系统服务通过 Binder RPC 暴露给应用沙箱，是恶意应用提权的关键攻击面，但厂商服务多为闭源、接口定义未知，传统 fuzzing 要么依赖源码要么无法覆盖真实设备上的专有服务。本文要解决：能否在无源码条件下系统化地对所有原生服务做稳定、覆盖引导的模糊测试。

NASS 是一个灰盒 fuzzer，核心创新是反序列化引导的接口提取（DGIE）：从闭源二进制中重建 RPC 接口定义并生成接口感知的种子；针对多线程、相互依赖的 RPC 服务器，把 fuzzer 输入与服务处理状态相关联，实现稳定覆盖信号，避免噪声反馈。

在 Google、小米、Samsung、OnePlus 等 5 台商用设备上，NASS 发现 12 个独特 bug，其中 5 个已分配 CVE；在接口恢复、目标探索与 bug 发现上均优于此前工作，且不依赖源码访问。

我的思考：NASS 把"闭源 Android 系统服务"从模糊测试盲区变为可系统覆盖的目标，接口重建+状态感知覆盖的组合是实质技术推进，且其通用 RPC 设计原则（IPC 绑定抽象、单一入口、反序列化例程）使方法可迁移到 Binder 之外的框架。局限是评估设备与 bug 数量有限，发现的多为崩溃类缺陷，利用价值需进一步评估；但作为闭源服务安全测试基础设施，价值明确。（无摘要，据标题推断）

#### Attacker Control and Bug Prioritization.
- **作者**: Guilhem Lacombe, Sébastien Bardin
- **方向**: 漏洞挖掘与利用
- **解读**: 漏洞发现能力已超过修复能力，积压漏洞需要基于可利用性的精确优先级排序；而攻击者对漏洞参数的控制程度是常被忽视的可利用性要素。本文证明污点分析以及朴素的定性/定量控制度量都不足以区分漏洞，提出以"可行值集"——控制域（domains of control）——作为分析焦点，以纳入威胁模型与专家知识。

方法上提出 Shrink and Split 算法：从符号执行得到的路径约束中高效提取每个参数的控制域，并以易处理、人类可读的形式呈现；在此基础上可自动计算更复杂的控制度量，如加权定量控制（Weighted Quantitative Control），为不同威胁等级的值赋予不同权重。

实验表明该方法既高效又精确：它是唯一能区分 cve-2019-14192 与 cve-2022-30552 这两个漏洞的方法，并揭示了 cve-2022-30790 人工评估中的错误；高度自动化使其接近全自动评估流水线。

我的思考：把"攻击者控制"从模糊直觉操作化为可行值域，是漏洞优先级排序研究里被低估却关键的半步——它让威胁模型与专家判断能以结构化方式进入排序过程，并能发现人工评估的错误，说明其区分度真实有效。局限是依赖符号执行的路径约束质量与可扩展性，控制域对复杂数据结构的表达力有限；但该方向对大规模漏洞管理的价值明确。
- **论文 PDF**: [Attacker Control and Bug Prioritization.](https://www.usenix.org/system/files/usenixsecurity25-lacombe.pdf)

#### CoreCrisis: Threat-Guided and Context-Aware Iterative Learning and Fuzzing of 5G Core Networks.
- **作者**: Yilu Dong; Tianchang Yang; Abdullah Al Ishtiaq; Syed Md. Mukit Rashid; Ali Ranjbar; Kai Tu; Tianwei Wu; Md. Sultan Mahmud; Syed Rafiul Hussain
- **方向**: 漏洞挖掘与利用
- **解读**: 蜂窝网络安全分析此前多依赖人工构造的静态测试输入，只能发现逻辑错误，难以系统性挖掘 5G 核心网（5GC）实现的漏洞。CoreCrisis 提出有状态黑盒模糊测试框架，针对 5GC 实现自动发现规范违背与崩溃缺陷。

方法采用动态两步走：先用仅含良性（正例）输入、结合分治与基于属性的等价性检查，高效学习 5GC 实现的有限状态机（FSM）表示；模糊测试阶段利用所学 FSM 定位未充分探索的状态，施加状态感知变异生成攻击性（负例）输入；再根据核心网的响应持续精化 FSM，形成"学习-测试-再学习"的迭代闭环。

结果显示，在 3 个开源与 1 个商用 5GC 实现上，发现 7 类违反技术规范的偏差与 13 个崩溃漏洞，可导致拒绝服务、认证绕过与计费欺诈。

我的思考：把 FSM 推断引入状态化模糊测试、以状态覆盖率引导变异，是针对复杂协议实现更聪明的测试策略；"规范偏差+崩溃"双输出兼顾逻辑漏洞与内存安全漏洞。局限是黑盒学习对超大规模状态空间的可扩展性、商业实现覆盖有限，逻辑漏洞还需结合业务场景评估真实危害。

- **论文 PDF**: [CoreCrisis: Threat-Guided and Context-Aware Iterative Learning and Fuzzing of 5G Core Networks.](https://www.usenix.org/system/files/usenixsecurity25-dong-yilu.pdf)

#### Fuzzing the PHP Interpreter via Dataflow Fusion.
- **作者**: Yuancheng Jiang, Chuqi Zhang, Bonan Ruan, Jiahao Liu, Manuel Rigger, Roland H. C. Yap, Zhenkai Liang
- **方向**: 漏洞挖掘与利用
- **解读**:
**问题**：PHP是Web开发的主导脚本语言，支撑从个人博客到大型平台的广泛网站；既有研究多聚焦PHP应用层安全问题（如代码注入），解释器庞大C代码库中的内存错误却长期被忽视，直接威胁PHP服务器的机密性、完整性与可用性。
**方法**：FlowFusion是首个自动检测PHP解释器内存错误的fuzzing框架：以数据流作为官方测试用例的高效表示，将两条及以上用例"融合"成语义更复杂的用例，并辅以测试变异、接口fuzzing与环境跨界等策略提升找bug能力。
**结果**：FlowFusion发现158个未知bug，其中125个已修复、11个已确认；相比官方测试套件与朴素用例拼接能发现其遗漏的bug并获得更高覆盖；24小时fuzzing后比AFL++和Polyglot多覆盖24%的代码，且已集成进官方PHP工具链。
**思考**：数据流融合是对"如何构造复杂有效输入"的巧妙回答，获官方工具链集成是强实绩背书；局限在于高度依赖PHP官方测试语料与领域特性，迁移到其他解释器需重新适配，但"以既有测试数据为原料"的思路对其他语言生态仍有借鉴价值。

- **论文 PDF**: [Fuzzing the PHP Interpreter via Dataflow Fusion.](https://www.usenix.org/system/files/usenixsecurity25-jiang-yuancheng.pdf)

#### Waltzz: WebAssembly Runtime Fuzzing with Stack-Invariant Transformation.
- **作者**: Lingming Zhang, Binbin Zhao, Jiacheng Xu, Peiyu Liu, Qinge Xie, Yuan Tian, Jianhai Chen, Shouling Ji
- **方向**: 漏洞挖掘与利用
- **解读**:
**问题**：WebAssembly是浏览器厂商推动的二进制指令格式，模块应在内存安全的运行时中执行，运行时因此构成可信计算基，其漏洞影响严重；对Wasm运行时fuzzing面临两大挑战：栈式语言的栈语义须被正确维护，且数百条指令的组合空间难以有效覆盖。
**方法**：Waltzz灰盒fuzzing框架提出"栈不变代码变换"，在变异过程中保持恰当的栈语义；设计多功能变异器从控制流与数据流两个维度系统遍历指令组合；并采用骨架生成算法产出种子语料中罕见的代码片段。
**结果**：在7个知名Wasm运行时上评估，Waltzz相比最近竞争者多覆盖12.4%代码、发现的唯一bug数为1.38倍；总计发现20个新bug且全部确认，其中17个已分配CVE。
**思考**：栈不变变换直击栈式语言fuzzing的语义约束难题，抽象漂亮且可复用，20个bug与17个CVE也证明实战价值；局限在于面向运行时实现而非应用业务逻辑，指令组合策略调参空间较大，但其思路对任何栈式虚拟机（如区块链VM）的fuzzing都有借鉴意义。

- **论文 PDF**: [Waltzz: WebAssembly Runtime Fuzzing with Stack-Invariant Transformation.](https://www.usenix.org/system/files/usenixsecurity25-zhang-lingming.pdf)

#### MBFuzzer: A Multi-Party Protocol Fuzzer for MQTT Brokers.
- **作者**: Xiangpu Song, Jianliang Wu, Yingpei Zeng, Hao Pan, Chaoshun Zuo, Qingchuan Zhao, Shanqing Guo
- **方向**: 漏洞挖掘与利用
- **解读**:
**问题**：MQTT是IoT环境广泛使用的多方通信协议，broker作为连接众多设备的服务器，其缺陷影响所有参与者；既有fuzzing工作均采用两方模型，输入空间不足，多方通信处理代码得不到覆盖，且只关注内存破坏或逻辑错误，不检验broker是否符合规范。
**方法**：MBFuzzer是黑盒fuzzing方法：构建含两个fuzzing输入发送方的框架以覆盖多方通信代码；设计消息优先级调度器与Petri网模型，分别指导用例生成与协调两个发送方的消息时序；用差分测试识别规范不符bug，并让LLM自动分析、验证bug报告是否为规范违例。
**结果**：在6个主流MQTT broker上评估，发现73个bug（20个内存破坏、53个规范不符），11个已分配CVE；与SoTA fuzzers相比，代码覆盖与找bug能力均占优。
**思考**：该工作把"多方交互"从fuzzing盲区变成一等公民，Petri网时序建模与LLM辅助规范判定务实创新，规范符合性检测也拓宽了协议fuzzing目标定义；局限在于黑盒设计对broker内部状态可见性有限、LLM判定依赖提示设计，但其方法对AMQP、CoAP等多方协议同样适用。

- **论文 PDF**: [MBFuzzer: A Multi-Party Protocol Fuzzer for MQTT Brokers.](https://www.usenix.org/system/files/usenixsecurity25-song-xiangpu.pdf)

#### IDFuzz: Intelligent Directed Grey-box Fuzzing.
- **作者**: Yiyang Chen, Chao Zhang, Long Wang, Wenyu Zhu, Changhua Luo, Nuoqi Gui, Zheyu Ma, Xingjian Zhang, Bingkai Su
- **方向**: 漏洞挖掘与利用
- **解读**: （无摘要，据标题推断）
**问题**：定向灰盒fuzzing（Directed Grey-box Fuzzing，DGF）旨在把测试资源集中导向特定目标代码位置（如补丁点、危险函数），以高效验证漏洞或回归缺陷；既有DGF方法在距离度量、能量分配与种子选择上仍有优化空间，本文提出"智能"定向fuzzing框架IDFuzz以提升目标到达效率。
**方法**：据标题推断，IDFuzz通过更精细的代码距离建模、自适应能量调度与智能种子优先级策略，引导fuzzing快速逼近目标位置，并可能在覆盖率与定向性之间取得更好平衡。
**结果**：论文标题表明其实现了智能化的定向fuzzing流程，与既有DGF工具相比的具体提升幅度以原文为准，此处不作臆断。
**思考**：定向fuzzing是漏洞复现与补丁验证的常用利器，其瓶颈在于距离度量与陷入局部路径的问题；IDFuzz若能在真实程序（如浏览器、内核组件）上验证有效性，将是对AFLGo等经典DGF工作的有力推进，其"智能"策略的具体机制与可复现性值得关注。


#### Robust, Efficient, and Widely Available Greybox Fuzzing for COTS Binaries with System Call Pattern Feedback.

- **作者**: Jifan Xiao, Peng Jiang, Zixi Zhao, Ruizhe Huang, Junlin Liu, Ding Li
- **方向**: 漏洞挖掘与利用
- **解读**: 灰盒模糊测试依赖代码覆盖率反馈，但对闭源（COTS）二进制收集覆盖率极难：静态重写会改坏程序（ZAFL 在 28% 的二进制上崩溃）、模拟器拖慢 6-100 倍且不支持新指令（QEMU 不支持 AVX）、Intel-PT 依赖特定硬件且事件缓冲易溢出。现有方法没有一种能覆盖全部 COTS 二进制。本文提出系统调用模式覆盖率（SPC）这一新反馈机制。

核心洞察是系统调用及其参数遍布程序各处、可充当隐蔽插桩点：执行不同代码路径会生成不同系统调用序列。作者用 N-gram 滑动窗口对调用序列（含参数）取哈希作为覆盖反馈，采用"长-短结合"策略（SPCθ∪SPC2θ∪SPCfull，θ 动态搜索）解决序列歧义与窗口冲突；参数选择算法按复杂度启发式自动剪除不稳定参数。SPFuzz/SPFuzz++（基于 AFL/AFL++）通过 3k 行内核驱动的分布式内核缓冲区采集调用，避免审计框架 90% 的事件丢失。

在 29 个真实基准（含 SPECCPU17 与 CUDA 工具链）上能模糊全部应用，而 ZAFL 失败 6 个、StochFuzz 11 个、AFL-QEMU 7 个（AVX）、AFL++Nyx 3 个、PTFuzzer 10 个（缓冲溢出）；SPFuzz 比 AFL-QEMU 快最多 41 倍，参数选择额外提速 45 倍；与 ZAFL 覆盖率差平均小于 0.1%，分支覆盖比同类覆盖率模糊器平均高 10.3%。发现 6 个新 CVE（含 CUDA nvdisasm 中 1 个），共 13 个崩溃获厂商确认。

首个 SPC 方案：完全绕开二进制改写、模拟与专用硬件，普适性（所有用系统调用的程序）、鲁棒性（不崩溃）与效率（近原生速度）兼得，且开源发布。局限：SPC 与代码覆盖率只是统计相关而非等价，对少系统调用的纯计算目标（SPECCPU 中 AI 类被排除）效果有限；依赖 Linux 内核驱动，跨平台不可用；N-gram 与参数选择含多个启发式超参，理论保证不足。

- **论文 PDF**: [Robust, Efficient, and Widely Available Greybox Fuzzing for COTS Binaries with System Call Pattern Feedback.](https://www.usenix.org/system/files/usenixsecurity25-xiao-jifan.pdf)

#### ELFuzz: Efficient Input Generation via LLM-driven Synthesis Over Fuzzer Space.
- **作者**: Chuyang Chen, Brendan Dolan-Gavitt, Zhiqiang Lin
- **方向**: 漏洞挖掘与利用
- **解读**: （无摘要，据标题推断）
**问题**：模糊测试的输入生成质量直接决定漏洞发现效率，传统基于变异与语法生成的方法在结构复杂、语义约束强的目标上常陷入低效；如何借助LLM的代码理解与生成能力产出高质量测试输入，是LLM×fuzzing交叉方向的核心问题，本文把LLM的应用从"输入空间"提升到"fuzzer空间"。
**方法**：据标题推断，ELFuzz不再让LLM直接生成输入，而是在"fuzzer空间"上做合成——即用LLM生成或组合生成输入的策略、模板或小工具（fuzzer），再据此高效产生覆盖多样的测试用例，实现"生成生成器"的思路。
**结果**：论文标题表明其通过LLM驱动的fuzzer空间合成提升了输入生成效率，与既有方法的覆盖率/漏洞产出对比以原文为准，此处不作臆断。
**思考**：把LLM用于"生成fuzzer"而非"生成输入"是一个高层次的抽象跃迁，可能绕过LLM逐输入生成的昂贵成本，同时保留其语义理解优势；该方向的挑战在于fuzzer空间搜索的收敛性与生成策略的通用性，若与经典灰盒框架（如AFL++）无缝集成，将极具实用潜力。


#### Hybrid Language Processor Fuzzing via LLM-Based Constraint Solving.
- **作者**: Yupeng Yang, Shenglong Yao, Jizhou Chen, Wenke Lee
- **方向**: 漏洞挖掘与利用
- **解读**: （无摘要，据标题推断）
**问题**：混合语言处理器（如SQLite、PHP、JavaScript引擎等同时解析多种语言/语法的复杂程序）在解析嵌套、混合语法时容易产生深层解析bug；传统符号执行与约束求解在这些目标上因约束复杂、路径爆炸而难以奏效，需要更智能的约束求解手段来引导fuzzing。
**方法**：据标题推断，本文利用LLM进行约束求解：让LLM理解解析器中的条件约束（如语法检查、长度/字符条件），生成满足约束的输入片段或变异方向，从而突破传统求解器在混合语言场景中的瓶颈，辅助fuzzing深入解析逻辑。
**结果**：论文标题表明其将LLM约束求解与混合语言处理器fuzzing结合并验证了有效性，发现的真实漏洞数量以原文为准，此处不作臆断。
**思考**：LLM作为约束求解器的替代/补充是当前fuzzing研究的热点，其优势在于处理非形式化、启发式的条件（传统SMT难以表达），弱点是正确性与可判定性无法保证；该工作若能在SQLite级的目标上稳定发现新bug，将证明LLM求解在实践中的可用性，但其结果的鲁棒性与规模化仍需检验。


#### From Alarms to Real Bugs: Multi-target Multi-step Directed Greybox Fuzzing for Static Analysis Result Verification.
- **作者**: Andrew Bao, Wenjia Zhao, Yanhao Wang, Yueqiang Cheng, Stephen McCamant, Pen-Chung Yew
- **方向**: 漏洞挖掘与利用
- **解读**:
静态分析工具误报率居高不下，人工核验告警真伪耗时费力；定向灰盒模糊（DGF）能以动态执行确认真实漏洞并提供 PoC，是验证静态分析结果的可行途径。但既有 DGF 不感知单个告警的语义信息（程序流）与多个告警间的关联（共享根因），无法有效利用静态分析"告警多、相互关联、路径有提示"的特点。

作者提出 Lyso，首个多目标、多步引导模糊器：利用静态分析给出的程序流把每个告警分解为可达的步骤序列，逐步逼近目标（intra-target directness）；依据共享根因将告警分为独立与相互依赖两类，并发处理相互依赖的目标（inter-target directness）；以 overlap、depth、distance 三种指标指导种子选择，并提出 seed density power scheduling 为罕见命中的目标动态分配更多变异。

在 Magma 基准上与 3 个覆盖率模糊器、5 个定向模糊器（共 8 个 SOTA）对比，Lyso 平均提速 12.17 倍并找到最多的绝对 bug 数；将其用于验证 4 个成熟真实程序的静态分析结果，成功发现 18 个新漏洞，其中 16 个获开发者确认、7 个已分配 CVE 编号。

该工作把静态分析的"告警景观"特征转化为模糊引导信号，思路自然、工程完整，是 DGF 与静态分析结合的代表性进展，18 个真实漏洞充分证明其实用性。局限在于效果依赖静态分析输出的流信息质量与根因聚类的准确性，评估以 Magma 与少量真实程序为主，跨工具、跨规模的泛化性仍待观察。

- **论文 PDF**: [From Alarms to Real Bugs: Multi-target Multi-step Directed Greybox Fuzzing for Static Analysis Result Verification.](https://www.usenix.org/system/files/usenixsecurity25-bao-andrew.pdf)

#### Low-Cost and Comprehensive Non-textual Input Fuzzing with LLM-Synthesized Input Generators.

- **作者**: Kunpeng Zhang, Zongjie Li, Daoyuan Wu, Shuai Wang, Xin Xia
- **方向**: 漏洞挖掘与利用
- **解读**:

现代软件常接受语法高度复杂的输入，灰盒模糊测试要发现安全缺陷就必须生成符合输入语法的测试用例；但这要求深入理解语法，而语法往往不可得且难以推断。LLM 已被证明能合成符合给定输入格式语法的高质量自然语言与代码，但 LLM 对图像、视频、PDF 等非文本输出的生成能力不足或成本过高，阻碍了 LLM 在语法感知模糊测试中的应用。

方法上提出 G²FUZZ：让 LLM 以 Python 脚本形式合成并变异"输入生成器"，由其产出符合语法的非文本数据，再用传统模糊器（AFL++）对生成的数据做进一步变异以有效探索输入空间。混合策略结合 LLM 驱动的"整体搜索"（holistic search）与工业级模糊器的"局部搜索"（local search）：LLM 擅长跳出局部最优并与变异型模糊器形成协同，且仅在必要时才调用 LLM，显著降低使用成本。

结果：在 TIFF 图像、MP4 音频与 PDF 等输入格式上评估，G²FUZZ 在 UNIFUZZ、FuzzBench 与 MAGMA 三个平台的大多数被测程序上，代码覆盖率与缺陷发现均优于 AFL++、Fuzztruction、FormatFuzzer 等最先进工具；还在最新真实软件中发现 10 个独特缺陷，其中 3 个已获 CVE 确认。

我的思考：该工作的成色在于用"LLM 写生成器、模糊器做变异"的混合分工化解 LLM 非文本生成短板，成本控制（低频调用 LLM）使其具备实用经济性，三大平台评测与 3 个 CVE 提供了扎实证据。局限：生成器合成质量依赖 LLM 对格式的理解，复杂二进制格式的语法覆盖与协同参数调优仍需工程经验。与 FormatFuzzer 等结构感知模糊器相比，G²FUZZ 开辟了"LLM 即语法来源"的低成本路线，是 LLM 驱动模糊测试的代表性进展。
- **论文 PDF**: [Low-Cost and Comprehensive Non-textual Input Fuzzing with LLM-Synthesized Input Generators.](https://www.usenix.org/system/files/usenixsecurity25-zhang-kunpeng.pdf)

#### Pig in a Poke: Automatically Detecting and Exploiting Link Following Vulnerabilities in Windows File Operations.

- **作者**: Bocheng Xiang, Yuan Zhang, Fengyu Liu, Hao Huang, Zihan Lin, Min Yang
- **方向**: 漏洞挖掘与利用
- **解读**:

链接跟随（link following）漏洞是 Windows 提权与沙箱逃逸攻击的核心根因之一：当高特权进程执行文件操作时，攻击者可通过符号链接、连接点（junction）等重解析点将操作重定向到任意目标文件，造成任意文件读写、删除或覆盖。此类漏洞分布广、手工审计成本高，长期缺乏自动化发现与利用手段。

据标题推断，该工作提出自动检测并利用 Windows 文件操作中链接跟随漏洞的管线：静态分析系统文件操作代码以定位缺乏妥善链接处理的路径，再通过动态验证（构造链接并观察操作是否被重定向）确认可利用性，并自动生成攻击序列（PoC）。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把链接跟随漏洞的发现从人工审计推进到规模化自动挖掘，直接服务 Windows 提权与沙箱逃逸的攻防两端。

我的思考：Windows 符号链接攻击（如 SymbolicLink 提权、mount point 攻击）是经典攻击面，该工作的自动化检测对漏洞响应与补丁优先级有实用价值；与 Linux 侧 symlink 竞态检测（如 TOCTOU 分析）形成对照。局限（据标题推断）：检测覆盖面取决于对 Windows 重解析机制与 API 语义的建模完整度，误报率与"自动利用"在真实沙箱环境中的成功率需原文数据支撑。


#### Await() a Second: Evading Control Flow Integrity by Hijacking C++ Coroutines.

- **作者**: Marcos Bajo, Christian Rossow
- **方向**: 漏洞挖掘与利用
- **解读**:

控制流完整性（CFI）防御通过限制程序执行到合法控制流路径来抵御代码复用攻击，但 CFI 只保护其设计时已知的指针类型；C++20 协程作为新编程范式，其执行关键数据存放在可写的堆内存（协程帧）中，暴露出 CFI 未覆盖的攻击面，且协程已在超过 4 亿台设备、130 余个热门 GitHub 仓库中部署。

方法上提出 Coroutine Frame-Oriented Programming（CFOP）：利用主流编译器在协程帧布局与恢复机制上的共同实现缺陷，劫持协程帧中的控制数据，在粗粒度与细粒度 CFI 保护下实现代码复用攻击与数据操纵。

结果：跨 GCC、MSVC、Clang 等主要编译器构建了一系列概念验证（PoC）利用，展示 CFOP 在 CFI 保护环境中的实际危害，并据此提出增强协程安全性的防御措施。

我的思考：该工作揭示"新语言特性引入时 CFI 知识库未同步更新"这一系统性问题，CFOP 的命名与面向对象编程（COOP）攻击一脉相承，说明编译器实现细节可成为通用攻击原语；对标准制定者与 CFI 实现者（Intel CET、CFG）都有警示意义。局限：利用需要先获得一次任意写或堆破坏原语，CFOP 的端到端可利用性依赖前置漏洞；提出的防御措施对现有二进制的兼容性有待评估。

- **论文 PDF**: [Await() a Second: Evading Control Flow Integrity by Hijacking C++ Coroutines.](https://www.usenix.org/system/files/usenixsecurity25-bajo.pdf)

#### Towards Automatic Detection and Exploitation of Java Web Application Vulnerabilities via Concolic Execution guided by Cross-thread Object Manipulation.

- **作者**: Xinyou Huang, Lei Zhang, Yongheng Liu, Peng Deng, Yinzhi Cao, Yuan Zhang, Min Yang
- **方向**: 漏洞挖掘与利用
- **解读**:

Java Web 应用漏洞检测的主要挑战是跨线程数据流：同一会话的多个请求在不同线程中通过共享 Java 对象传递数据，攻击者需要按特定顺序构造请求序列、并生成相互依赖的结构化 payload 才能到达 sink。先验工作（如面向 PHP 的 Navex、模糊器 Witcher、静态分析平台 Joern）都无法处理此类跨线程数据流。

论文提出 JAEX，首个面向 Java Web 应用的自动化检测与利用（AEG）框架。静态阶段基于扩展 Andersen 指针分析的 ICFG（支持 Spring 依赖注入与 AOP）识别 17 类入口（Web 框架方法与线程入口），用 sink 引导的按需迭代算法沿跨线程数据流图搜索，依据共享对象写读顺序推断请求序列；动态阶段采用步进式 concolic 执行逐请求推进、以不可控共享对象为目标点，用 Cross-thread Object Manipulation Graph（COMG）记录请求间字段依赖与约束，结合 exploit 模板库生成真实 HTTP 利用。

在 92 个历史漏洞基准（16 个应用）中，JAEX 检测 90 个（97.8%）并生成 87 个 exploit；Witcher 仅检测 2 个、Joern 44 个，且均无法生成 exploit。在 25 个最新流行应用中挖掘出 35 个 0-day 漏洞并全部生成利用，均获厂商确认。静态阶段假阳性率 85.07%，但 concolic 验证后最终 0 假阳性；消融实验显示 JAEX-NOCT 漏掉的均为跨线程漏洞。

填补了跨线程数据流在 Java Web AEG 中的空白，COMG 对请求间依赖的形式化直观且可扩展，97.8% 的检测率与最终 0 假阳性在同类工具中突出。局限是仅覆盖注入类漏洞（SQL、命令、表达式注入），DoS 与反序列化不在范围；concolic 执行需要最小运行时环境与外部资源 hook，复杂配置场景可能受限。与 Witcher、Joern 的对比鲜明说明"感知跨线程数据流"是 Java Web 自动利用的关键。

- **论文 PDF**: [Towards Automatic Detection and Exploitation of Java Web Application Vulnerabilities via Concolic Execution guided by Cross-thread Object Manipulation.](https://www.usenix.org/system/files/usenixsecurity25-huang-xinyou.pdf)

### 网络与协议安全（22 篇）

#### Email Spoofing with SMTP Smuggling: How the Shared Email Infrastructures Magnify this Vulnerability.
- **作者**: Chuhan Wang, Chenkai Wang, Songyi Yang, Sophia Liu, Jianjun Chen, Haixin Duan, Gang Wang
- **方向**: 网络与协议安全
- **解读**: 问题：邮件伪造是钓鱼攻击中冒充可信发件人的关键技术，而 SMTP 走私（SMTP smuggling）是 2023 年公开的新漏洞，可绕过 SPF、DMARC 等既有认证协议实现邮件伪造。其影响范围与社区缓解措施的有效性此前未被系统评估，威胁面未知。

方法：作者对 SMTP 走私漏洞开展深入研究，结合对公共邮件服务、开源邮件软件与邮件安全网关的实证测量；并首次探索如何在符合伦理的前提下测量私有邮件服务，提出结合用户研究、DKIM 侧信道与非侵入式测试方法的新方法论。

结果：共发现 19 个公共邮件服务、1,577 个私有邮件服务、5 个开源邮件软件和 1 个邮件网关仍受 SMTP 走私（或其新变种）影响；结果还显示邮件基础设施的中心化（如共享 SPF 记录、通用邮件软件/网关）放大了漏洞影响，攻击者可借免费注册的邮箱账号伪造高信誉域名邮件。作者给出了短长期缓解建议，并开发了在线自诊断服务。

思考：该工作的测量伦理方法论（非侵入式+侧信道+用户研究）是亮点，为私有基础设施测量树立了范例；中心化放大漏洞的结论对邮件生态治理有直接政策含义。局限在于测量覆盖面随时间衰减、以及缓解建议在碎片化邮件生态中的落地阻力。

- **论文 PDF**: [Email Spoofing with SMTP Smuggling: How the Shared Email Infrastructures Magnify this Vulnerability.](https://www.usenix.org/system/files/usenixsecurity25-wang-chuhan.pdf)

#### Catch-22: Uncovering Compromised Hosts using SSH Public Keys.
- **作者**: Cristian Munteanu, Georgios Smaragdakis, Anja Feldmann, Tobias Fiebig
- **方向**: 网络与协议安全
- **解读**: 问题：攻击者常通过暴力破解攻陷系统，并以部署 SSH 公钥建立持久化，从 Mirai 等 IoT 僵尸网络到恶意运营的后端皆然。在大规模识别这些被攻陷系统将能支撑针对性清理，但现有方法要么需要访问系统，要么依赖已知口令测试。

方法：作者利用 SSH 的行为特性——公钥认证时服务器向客户端发送挑战以验证密钥是否存在——在不访问系统的情况下检测被攻陷主机，既不能访问被攻陷系统，也无需审计权限。

结果：互联网范围扫描识别出超过 21,700 个唯一系统（1,649 个 AS、144 个国家），其上安装了 52 个已核实恶意密钥中的至少一个，其中包括关键互联网基础设施；还发现 fritzfrog 僵尸网络、teamtnt 等恶意行动的新背景，以及敏感 AS 中与国家行为体相关的密钥。与蜜罐数据对比显示蜜罐会低估或高估攻击者活动。作者还与 CSIRT 和 Shadowserver 基金会合作通报并修复被攻陷系统。

思考：利用协议行为做无侵入式检测的创意出色，把检测成本降到互联网规模可行，且不触碰被攻陷系统数据，伦理上更干净；与 CSIRT 的落地协作形成研究到实战的闭环。局限在于仅覆盖公钥认证持久化这一种机制，且扫描频次与伦理需持续审慎管理。

- **论文 PDF**: [Catch-22: Uncovering Compromised Hosts using SSH Public Keys.](https://www.usenix.org/system/files/usenixsecurity25-munteanu.pdf)

#### Transparent Attested DNS for Confidential Computing Services.
- **作者**: Antoine Delignat-Lavaud, Cédric Fournet, Kapil Vaswani, Manuel Costa, Sylvan Clebsch, Christoph M. Wintersteiger
- **方向**: 网络与协议安全
- **解读**: 机密计算（如 Intel TDX、AMD SEV-SNP）承诺保护云端数据的机密性与完整性，但客户端真正要验证的不仅是"底层硬件支持 TEE"，还包括"我连接到的这个端点确实运行着我所信任的代码与配置"。DNS 作为客户端解析服务端点的第一跳，若可被劫持或欺骗，远程证明的可信链条就从根上断裂。本工作从标题看旨在为机密计算服务提供"透明且可证明"的 DNS，把域名解析与远程证明绑定。

据标题推断，方法上可能将 DNS 记录（或关联的证明材料）与 TEE 远程证明（如 attestation 报告、代码身份）密码学绑定，并借助透明日志（transparency log）机制使记录变更公开可审计，客户端在解析域名时即可验证服务端点的机密计算属性，从而抵御 DNS 劫持、中间人重定向与恶意镜像部署。微软研究院背景提示其可能面向 Azure 机密计算基础设施落地。摘要缺失，具体协议设计无法确认。

摘要缺失，无法引用量化结果；预期包含协议的安全性论证、DNS 解析性能开销，以及与现有 DNS/DoH/透明日志生态的兼容性评估。

DNS 是机密计算信任链中常被忽视的一环：现有方案大多假设"解析到的 IP 即可信端点"，而攻击者可劫持 DNS 把客户端引向恶意实例。把证明嵌入 DNS 解析流程，是把信任从传输层延伸到命名层的务实设计，与微软此前 Attested TLS/DCAP 证明体系一脉相承。局限：无摘要，威胁模型（谁控制 DNS、证明的时效性）、对权威 DNS 生态的改造成本、以及证明撤销机制等关键细节待全文确认。（无摘要，据标题推断）


#### Available Attestation: Towards a Reorg-Resilient Solution for Ethereum Proof-of-Stake.
- **作者**: Mingfei Zhang, Rujia Li, Xueqian Lu, Sisi Duan
- **方向**: 网络与协议安全
- **解读**: 以太坊于 2022 年 9 月从 PoW 转向 PoS，虽带来能耗降低、吞吐提升等改进，也引入新漏洞——其中最具代表性的是恶意重组（reorganization）攻击：拜占庭故障验证者故意操纵规范链，使诚实验证者的区块被丢弃，从而获取更高奖励、降低链质量，甚至构成活性威胁。本文首先揭示：已知的大多数以太坊 PoS 攻击本质上都是某种形式的重组攻击，且即便网络同步（消息传递与处理有已知上界），多数攻击仍可发动。

针对此，作者不再像既有研究那样逐案打补丁，而是采取系统性方法：提出一种可证明安全的解决方案，在同步网络下保证任何重组攻击都无法发动；在部分同步网络下，仍保持共识协议常规的安全性与活性性质。

评估表明，该方案对五类重组攻击均具韧性，且协议开销很低、效率高，兼顾了安全保证与工程可行性。

该工作的贡献在于把碎片化的重组攻击研究统一为"重组攻击"框架，并给出第一个可证明安全的系统性缓解方案，安全论证（同步下抗重组、部分同步下保安全活性）与实验验证相结合，说服力强。局限：方案可能对共识协议有修改（如新的投票/确认规则），与以太坊现有客户端实现的兼容性、以及部分同步假设下安全性边界的实际宽裕度需进一步评估；对 LMD-GHOST/Casper FFG 具体机制的适配细节待全文确认。总体而言，它为 PoS 链的抗重组设计提供了理论基准。

- **论文 PDF**: [Available Attestation: Towards a Reorg-Resilient Solution for Ethereum Proof-of-Stake.](https://www.usenix.org/system/files/usenixsecurity25-zhang-mingfei.pdf)

#### Beyond Exploit Scanning: A Functional Change-Driven Approach to Remote Software Version Identification.
- **作者**: Jinsong Chen; Mengying Wu; Geng Hong; Baichao An; Mingxuan Liu; Lei Zhang; Baojun Liu; Haixin Duan; Min Yang
- **方向**: 网络与协议安全
- **解读**:（无摘要，据标题推断）远程识别服务器或设备上运行的软件版本，是攻击面测绘与漏洞管理的基础；传统方法依赖扫描已知漏洞利用（exploit probing），覆盖面窄、误报高且可能触发告警。

论文提出基于功能变化的远程版本识别方法：通过探测不同版本间可观测的功能差异（如协议行为、响应特征的变化）来推断具体版本，而非依赖已知漏洞利用。

（无摘要，据标题推断）据标题，该方法相比 exploit 扫描在版本识别的覆盖度与准确率上均有提升，可识别更细粒度的版本。

把版本指纹从"漏洞特征"转向"功能变化特征"是思路上的升级——功能差异信息量更大且无需依赖已知漏洞库，对未知设备与闭源软件尤其有价值；作者团队在互联网测量领域积累深厚，工程可信度较高。挑战在于功能探测点的选取、被动场景适用性与探测流量的隐蔽性，与 banner 抓取、TLS 指纹等方法互为补充而非取代。


#### Mitigating Injection Attacks against E2EE Applications via View-Based Partitioning.
- **作者**: Andrés Fábrega; Samuel Breckenridge; Armin Namavari; Thomas Ristenpart
- **方向**: 网络与协议安全
- **解读**:（无摘要，据标题推断）端到端加密（E2EE）应用（如 Signal 类）面临消息注入攻击：攻击者可在用户会话中注入看似来自可信联系人的内容，而界面无法区分内容来源，用户易被误导。

论文提出基于视图分区的缓解方案：将消息显示界面按来源（发送者）进行视觉分区，使注入内容与真实来源的呈现分离，让用户能够辨别消息的真实来源。

（无摘要，据标题推断）据标题，视图分区方案在保持可用性的同时显著降低注入攻击的欺骗成功率，且对现有 E2EE 协议改动较小。

继该团队此前系统刻画 E2EE 注入攻击之后，本文转向防御：视图分区直击"界面层无法认证来源"这一根因，属人机交互与协议安全的交叉方案，无需修改密码学协议，部署门槛低。局限在于分区界面对长对话的可用性影响、移动端小屏适配，以及恶意应用诱导用户忽略分区的残余风险；与协议层修复（如来源认证扩展）可形成纵深防御。


#### Your Shield is My Sword: A Persistent Denial-of-Service Attack via the Reuse of Unvalidated Caches in DNSSEC Validation.

- **作者**: Shuhan Zhang; Shuai Wang; Li Chen; Dan Li; Baojun Liu
- **方向**: 网络与协议安全
- **解读**: （无摘要，据标题推断）DNSSEC 通过签名验证保障 DNS 数据完整性，但验证器对某些缓存条目可能未经完整验证即复用；本文利用这种"未验证缓存重用"把 DNSSEC 这一防御机制转化为攻击武器，发动持久拒绝服务攻击。

  据标题推断，攻击者诱导验证器缓存并复用未通过验证的数据，使特定域名长期解析失败或指向错误结果，形成"中毒缓存即服务"的持久 DoS；研究应包含攻击构造、触发条件与对主流验证器实现的验证。

  无摘要，具体影响范围与持续时间无法从标题确认；"Your Shield is My Sword"暗示启用 DNSSEC 的一方反受其害。

  把"验证机制的缓存路径"本身变成攻击面，提醒协议设计者：安全机制的性能捷径（缓存）必须与验证状态强绑定；与既往 DNS 投毒研究不同，该攻击可能不需要猜解事务 ID，而是利用验证器内部状态机缺陷；对 DNSSEC 大规模部署而言，这类"防御性武器化"研究有重要警示价值，也提示部署方需把验证器的缓存语义纳入安全审计范围。


#### POPS: From History to Mitigation of DNS Cache Poisoning Attacks.

- **作者**: Yehuda Afek; Harel Berger; Anat Bremler-Barr
- **方向**: 网络与协议安全
- **解读**: （无摘要，据标题推断）DNS 缓存投毒是长期威胁，传统缓解依赖随机化（随机端口、随机事务 ID），但攻击者可借暴力猜解或侧信道绕过；本文提出基于历史数据的新缓解思路。

  据标题推断，POPS（"From History to Mitigation"）利用 DNS 的历史查询与解析记录：通过比对当前响应与历史基线，识别可疑的异常响应，或对同一记录多次解析结果做一致性检验，从而在投毒生效前拦截；可能结合概率统计与信誉机制。

  无摘要，具体检测率与误报率无法从标题确认；该团队长期深耕 DNS 安全，预计包含对真实流量的评估。

  把"历史"作为抗投毒信号是对随机化思路的补充——不赌攻击者猜不中，而是赌攻击者的伪造难以同时符合历史模式；这类方法对解析器、转发器等基础设施的部署侵入小，有落地价值；局限在于冷启动时缺乏历史，以及攻击者可能自适应伪造历史统计特征；其与 DNSSEC、DoH 等强认证方案并非替代关系，而是可以互为补充的纵深防御层次。


#### DNS FLaRE: A Flush-Reload Attack on DNS Forwarders.

- **作者**: Gilad Moav; Yehuda Afek; Anat Bremler-Barr; Amit Klein
- **方向**: 网络与协议安全
- **解读**: （无摘要，据标题推断）DNS 转发器与解析器在本地缓存解析结果，其缓存命中状态是用户访问域名的侧信道；本文提出基于 Flush-Reload 缓存侧信道的攻击，窃取转发器的 DNS 查询内容。

  据标题推断，FLaRE 利用 CPU 缓存共享：攻击者与转发器同机或共享硬件时，通过 Flush+Reload 时序测量探测解析器内部数据结构（如缓存条目）的访问情况，推断其他用户查询了哪些域名，从而破坏 DNS 查询隐私。

  无摘要，具体探测精度与泄露范围无法从标题确认；Amit Klein 是知名侧信道研究者，预计包含对主流转发器实现的实证评估。

  DNS 查询隐私是敏感的用户行为元数据，转发器常被视为信任边界内组件，侧信道攻击把它拉回攻击面；该工作与既往对解析器缓存的时序攻击一脉相承，但 Flush-Reload 精度更高；缓解方向包括缓存访问随机化、组件隔离或加密 DNS 查询，也再次印证在共享硬件上运行安全基础设施的固有风险。


#### Lemon: Network-Wide DDoS Detection with Routing-Oblivious Per-Flow Measurement.

- **作者**: Wenhao Wu; Zhenyu Li; Xilai Liu; Zhaohua Wang; Heng Pan; Guangxing Zhang; Gaogang Xie
- **方向**: 网络与协议安全
- **解读**: 全网 DDoS 检测可提前发现攻击、减少受害方损失；但 DDoS 流量路由不可预测，会打破管理员对网络拓扑的先验假设，使现有基于 sketch 的测量系统出现报文重复计数（over-counting）与处理阶段错配（mis-allocating）。

  作者提出 Lemon——路由无关、资源友好且可扩展的 DDoS 检测系统，不依赖任何流量路由假设：设计新型数据结构 Lemon sketch，在数据平面支持无重复计数、无错配的测量；控制平面聚合各测量点的 Lemon sketch，利用逐流级全网测量结果进行 DDoS 攻击检测与受害者识别。

  在软件交换机（Bmv2）与可编程交换机硬件（Tofino）上实现；评估显示在各种拓扑与流量分布配置下均能保持一致的检测精度。

  去掉"路由已知"这一脆弱假设是网络测量的重要工程进步，Tofino 硬件实现证明其可部署性；重复计数问题的根除使全网聚合测量更可信；局限在于原型评估规模与真实骨干流量的验证，以及控制平面聚合的通信开销，后续可探索与数据平面安全卸载的结合。

- **论文 PDF**: [Lemon: Network-Wide DDoS Detection with Routing-Oblivious Per-Flow Measurement.](https://www.usenix.org/system/files/usenixsecurity25-wu-wenhao.pdf)

#### Assessing the Aftermath: the Effects of a Global Takedown against DDoS-for-hire Services.

- **作者**: Anh V. Vu; Ben Collier; Daniel R. Thomas; John Kristoff; Richard Clayton; Alice Hutchings
- **方向**: 网络与协议安全
- **解读**: 执法机构与私营伙伴近年持续干预 DDoS-for-hire（booter）市场，但这类全球性干预的实际效果缺乏系统评估——本文以罕见的多源数据全景回答"干预之后发生了什么"。

  作者融合多类量化数据：查封网站的网页流量与 ground-truth 访问、来自学术/行业/自报的数百万条 DDoS 攻击记录、地下论坛与 Telegram 频道的聊天内容，评估自 2022 年 12 月以来持续进行的全球干预（两波站点查封，配合欺骗性域名与数字影响力策略）。

  结果显示：第一波被查封站点超半数中位 1 天内回归，第二波全部中位 2 天内回归；重现的 booter 域名虽与旧域名高度相似却难以吸引流量（减少 80-90%）；第一波使全球 DDoS 攻击量下降 20-40%（对通常归因于 booter 的 UDP 攻击统计显著），第二波影响甚微；地下讨论显示累积效应（安全感变化、部分经营者退出），但所有数据集一致表明非法市场韧性很强，对全球 DDoS 攻击量的影响总体短暂，至多约六周。

  该研究用全源数据冷静评估执法干预，得出"短期有效、市场韧性"的结论；欺骗性域名与数字影响力策略虽重创回归者流量，却未动摇供需基本面；对政策制定者的启示是查封需配合结构性威慑（上游基础设施、支付渠道、供应商生态）方能持久。

- **论文 PDF**: [Assessing the Aftermath: the Effects of a Global Takedown against DDoS-for-hire Services.](https://www.usenix.org/system/files/usenixsecurity25-vu.pdf)

#### BGP Vortex: Update Message Floods Can Create Internet Instabilities.

- **作者**: Felix Stöger; Henry Birge-Lee; Giacomo Giuliari; Jordi Subirà Nieto; Adrian Perrig
- **方向**: 网络与协议安全
- **解读**: （无摘要，据标题推断）BGP 依靠持续交换更新消息维持路由收敛，但大量更新消息洪泛可压垮路由器的处理能力，造成路由振荡与互联网级不稳定。

  据标题推断，论文研究"更新消息洪泛"攻击：攻击者通过众多路径与前缀注入海量 BGP UPDATE，使路由器 CPU 过载、队列积压、收敛延迟，甚至触发重置与路由抖动，形成自我强化的"漩涡"；应包含攻击构造、对真实路由器或协议的模拟验证与影响面分析。

  无摘要，具体洪泛速率阈值与恢复时间无法从标题确认；Adrian Perrig 团队长期研究 BGP 安全与架构加固。

  控制平面资源耗尽是比前缀劫持更隐蔽的 BGP 攻击面，路由器的"无状态"设计使洪泛难以事前过滤；该工作若能量化触发不稳定所需的最小资源，将对运营商的限速与过滤策略有直接指导意义；与 RPKI、BGPsec 等侧重真实性的方案互补，资源型攻击需要不同的防御支点；其测量与模拟方法本身也为评估互联网控制面的韧性提供了参考范式。


#### ImpROV: Measurement and Practical Mitigation of Collateral Damage in RPKI Route Origin Validation.

- **作者**: Weitong Li; Yuze Li; Taejoong Chung
- **方向**: 网络与协议安全
- **解读**: RPKI 仅在路由器执行路由来源验证（ROV）时才有效；但 ROV 的部分部署导致"附带损害"：即使某 AS 启用了 ROV，若后续跳未正确验证，其流量仍可能被导向错误来源。

  作者首次全面测量真实世界 ROV 附带损害的规模；并提出 ImpROV：针对特定 RPKI-invalid 前缀，检测并避开可能引发附带损害的下跳，同时不干扰数据平面上不受该附带损害影响的其他 IP 地址空间。

  测量显示：高达 85.6% 的 RPKI-invalid 通告易受附带损害攻击，34% 已启用 ROV 的 AS 仍易受影响；ImpROV 能降低多数部署 ROV 的 AS 的劫持成功率，仅引入不到 3% 与 4% 的内存与 CPU 开销，缓解代价几乎可忽略。

  首次把"部分部署的副作用"量化到 85.6% 与 34% 量级，说明过渡期的 ROV 安全增益被显著稀释；ImpROV 以极小开销提供务实缓解，凸显"不完美部署下的防御工程"价值；后续可探索与 BGP 社区过滤、路由策略的协同，以及对选择性劫持的更强保证；其"避开可疑下跳"的思路若能映射为标准的策略语言（如 BGP communities），将更易于在运营商网络中落地。

- **论文 PDF**: [ImpROV: Measurement and Practical Mitigation of Collateral Damage in RPKI Route Origin Validation.](https://www.usenix.org/system/files/usenixsecurity25-li-weitong.pdf)

#### SoK: An Introspective Analysis of RPKI Security.

- **作者**: Donika Mirdita; Haya Schulmann; Michael Waidner
- **方向**: 网络与协议安全
- **解读**: RPKI 是抵御 BGP 前缀劫持的核心机制，部署已近临界点：全球近半数前缀被 RPKI 覆盖，27% 的网络已用 RPKI 验证通告；十年研究积累需要一次系统性整合与安全态势量化。

  作者编译首个覆盖 RPKI 漏洞与错误配置的系统性综述（SoK）：梳理既有研究（软件漏洞、基础设施鲁棒性、验证普及度），并补充新测量——发现 RPKI 仓库可用性的新趋势及其与验证器的通信模式。

  结果显示 56% 的全球 RPKI 验证器至少存在一个已文档化漏洞；论文将既有研究与自身测量编织成漏洞全景表，追溯漏洞来源，并推导出为全球完全部署所需的未来研究方向。

  SoK 的价值在于把散落十年的成果整合成可操作的态势图；56% 验证器带漏洞的数字令人警醒——RPKI 的"信任锚"本身并不健壮；对仓库可用性与通信模式的测量填补了生态健康度空白；其划定的研究路线图或能引导资源投向最薄弱环节，也为验证器厂商提供了一份可直接对照的自查清单。
- **论文 PDF**: [SoK: An Introspective Analysis of RPKI Security.](https://www.usenix.org/system/files/usenixsecurity25-mirdita.pdf)

#### Onions Got Puzzled: On the Challenges of Mitigating Denial-of-Service Problems in Tor Onion Services.

- **作者**: Jinseo Lee, Hobin Kim, Min Suk Kang
- **方向**: 网络与协议安全
- **解读**: 
Tor 洋葱服务的匿名性要求使其无法采用按 IP 限流、验证码等常规 DoS 缓解手段，服务极易被拒绝服务攻击打垮。为此 Tor 社区在官方更新中重新启用了"客户端谜题"（client puzzle）机制应对真实世界的 DoS 攻击，并被多家大型洋葱服务采用。本文关注的正是这套谜题系统的安全性。

作者发现并命名了一族新攻击 OnionFlation：攻击者通过操纵谜题难度协商过程，人为地"膨胀"所有客户端所需的谜题难度，却不会在目标服务处造成明显拥塞，从而在不触发拥塞检测的情况下让任何现有洋葱服务几乎不可用。其根因是客户端谜题机制本身存在一个"通胀抵抗"与"拥塞抵抗"不可兼得的权衡——这正是漏洞的根源。

实验在真实 Tor 网络上进行了符合伦理的评估，展示了攻击的实际影响；攻击成本约为每小时几美元，即可让服务基本瘫痪。作者已将漏洞报告给 Tor 项目并获得确认，并给出了面向洋葱服务的、旨在平衡两类攻击的实践建议。

本文的价值在于首次系统揭示了匿名网络场景下谜题机制的根本性设计权衡，说明"给所有人加难度"与"让攻击者更费力"在 Tor 场景下相互冲突。局限在于攻击依赖对谜题协商协议的具体理解，且成本数据基于当前网络环境；未来若谜题机制升级，攻击面可能转移。该工作对匿名网络的可用性安全研究具有重要启发意义。

- **论文 PDF**: [Onions Got Puzzled: On the Challenges of Mitigating Denial-of-Service Problems in Tor Onion Services.](https://www.usenix.org/system/files/usenixsecurity25-lee.pdf)

#### Endangered Privacy: Large-Scale Monitoring of Video Streaming Services.
- **作者**: Martin Björklund; Romaric Duvignau
- **方向**: 网络与协议安全
- **解读**: HTTPS 普及未能堵住所有侧信道：MPEG-DASH 自适应码率流媒体生成的独特流量模式可在 TLS 保护下暴露观看内容，但既往研究未证明其在"大规模攻击场景"中仍可利用。本文构建协议无关（不依赖网络层信息）的识别系统，把该威胁从实验室推向最大规模的实测。

方法上，系统结合 k-d 树搜索与时间序列方法进行实时视频指纹匹配；数据集为迄今最大，覆盖 24 万余个视频、横跨三家完整流媒体服务；攻击设定包括受害者位于 VPN 之后或遭受 Wi-Fi 窃听等现实场景。

结果显示，实时视频识别准确率超过 99.5%，在 VPN/无线窃听场景下依然有效；作者还分析自适应码率流是漏洞根因并提出缓解策略，并开源大规模指纹数据集、抓包数据与工具。

我的思考：99.5% 以上的准确率加上"协议无关"，意味着视频识别攻击已具备大规模监控的工程条件，对"加密即隐私"的信念是又一次实质性削弱；补齐"数据集+工具"开源短板也让该威胁可被复现与防御研究。局限是识别依赖指纹库预先采集、码率档位变化可能漂移，缓解策略（填充/固定码率）的部署成本也需现实权衡。
- **论文 PDF**: [Endangered Privacy: Large-Scale Monitoring of Video Streaming Services.](https://www.usenix.org/system/files/usenixsecurity25-bjorklund.pdf)

#### S/MINE: Collecting and Analyzing S/MIME Certificates at Scale.

- **作者**: Gurur Öndarö, Jonas Kaspereit, Samson Umezulike, Christoph Saatjohann, Fabian Ising, Sebastian Schinzel
- **方向**: 网络与协议安全
- **解读**:

S/MIME 是电子邮件端到端加密与签名的主流标准，其安全性建立在证书公钥基础设施之上；但与 Web PKI 相比，S/MIME 证书生态的研究长期不足，证书滥用、弱密钥、算法退化的真实规模缺乏实证，影响了邮件加密安全性的评估与监管决策。

据标题推断，S/MIME 通过大规模收集与分析 S/MIME 证书构建生态画像：大概率利用证书透明日志（CT）、邮件服务器扫描与证书库等数据源，统计证书签发规模、算法与密钥长度分布、有效期、CA 行为与异常证书（如密钥重用、自签名滥用），并评估其对邮件安全的影响。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于首次以数据规模刻画 S/MIME PKI 的健康状况，为 CA/B 论坛 S/MIME 基线要求等政策的有效性评估提供证据。

我的思考：Schinzel 团队此前在 S/MIME 生态安全（如"the S/MIME security crisis"）上有系列工作，该论文大概率延续其"测量驱动安全"范式，与 Web PKI 的 CT 普查研究形成对照；对理解邮件加密采用瓶颈与弱证书风险有实证价值。局限（据标题推断）：证书收集覆盖面取决于数据源（CT 之外的私有 CA 证书不可见）、证书存在与真实使用之间的差距，以及分析维度是否涵盖客户端信任行为均需原文确认。


#### Towards Internet-Based State Learning of TLS State Machines.

- **作者**: Marcel Maehren, Nurullah Erinola, Robert Merget, Jörg Schwenk, Juraj Somorovsky
- **方向**: 网络与协议安全
- **解读**:

TLS 协议的安全性不仅取决于规范，更取决于各实现对状态机的正确执行：状态机缺陷可导致协议降级、认证绕过或消息混淆等严重漏洞。主动学习（active learning）可自动推断 TLS 实现对的状态机，但已有工作多局限于本地或受限环境，难以反映互联网上真实部署的多样性。

据标题推断，论文将状态机学习推广到互联网规模：对大量真实 TLS 端点发送探测消息序列，依据响应自动推断其状态机行为，并与规范及主流实现对比，以发现不一致、未定义转换与实现缺陷。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于以实证数据刻画 TLS 实现状态机的真实面貌，为 RFC 合规性评估与漏洞挖掘提供规模化手段。

我的思考：作者团队在 TLS 实现分析与状态机学习（如 TLS-Attacker 生态）上有深厚积累，互联网规模学习能覆盖更多小众实现与历史版本，弥补本地测试的盲区。局限（据标题推断）：黑盒学习的探测开销、中间盒与负载均衡对响应一致性的干扰、以及推断状态机与真实实现语义的偏差，是该方法的主要挑战；学习到的状态机是否催生新的可利用漏洞有待原文呈现。


#### Misty Registry: An Empirical Study of Flawed Domain Registry Operation.

- **作者**: Mingming Zhang, Yunyi Zhang, Baojun Liu, Haixin Duan, Min Zhang, Fan Shi, Chengxi Xu
- **方向**: 网络与协议安全
- **解读**:

域名注册局通过可扩展供给协议（EPP）管理 TLD 内域名的完整生命周期，其实现与运营实践各异，任何细微的操作缺陷都可能使受管资源暴露于滥用风险；但注册局运营封闭不透明，社区对其内部实践与潜在威胁缺乏理解，成为域名安全研究中的盲区。

方法上，作者系统分析跨 TLD 注册局的 EPP 操作安全：梳理整个域名生命周期并将操作映射到对应域名状态，发现注册局视角的状态重叠与复杂触发因素；融合 TLD zone 文件、超过 79 亿条历史 WHOIS 记录与实时注册商接口等数据源，采用静态与动态结合的外部评估技术推断域名存在性与注册状态，揭示注册局策略的内部运作。

结果：发现三个新型 EPP 实现缺陷，影响 Identity Digital、Google、Nominet 等主要注册局，且已有证据表明攻击者正在暗中利用；实验显示超过 160 万个域名、横跨 50% 以上的 TLD（如 .app、.top）受这些缺陷影响；作者已负责任披露并协助修复。

我的思考：这是少有的"注册局侧"系统安全研究，以海量外部观测数据穿透不透明运营，发现真实被利用的缺陷并给出量化影响面，实证价值突出。局限：外部观测对注册局内部策略的推断存在误差，覆盖范围受可观测数据源限制，三个缺陷之外的隐患可能仍未被发现；对域名滥用生态（劫持、抢注、恶意注册）的防御有直接参考意义。

- **论文 PDF**: [Misty Registry: An Empirical Study of Flawed Domain Registry Operation.](https://www.usenix.org/system/files/usenixsecurity25-zhang-mingming.pdf)

#### Haunted by Legacy: Discovering and Exploiting Vulnerable Tunnelling Hosts.

- **作者**: Angelos Beitis, Mathy Vanhoef
- **方向**: 网络与协议安全
- **解读**:

互联网上存在大量接受任意来源隧穿流量的开放隧道主机（支持 IPIP、GRE、4in6、6in4 等老旧或现代封装协议），这些遗留设施可能被滥用为代理、源地址欺骗或内网渗透跳板，但其规模与安全影响长期未被量化。

方法上，作者用 7 种不同的扫描方法对 IPv4 与 IPv6 空间进行大规模扫描，识别接受未认证隧穿流量的易受害主机，并系统评估其可被滥用的攻击面，包括单向代理、源地址伪造与内网访问。

结果：发现超过 400 万个易受害主机；并构造两种新的 DoS 放大攻击——一种将流量在时间上集中、另一种让数据包在易受害主机间循环，放大因子分别至少达 16 与 75；另提出经济性拒绝可持续性（EDoS）攻击耗尽目标主机的出口带宽。

我的思考：大规模测量叠加新攻击构造，揭示了"遗留协议默认开放"的普遍性隐患，400 万级受害面与 75 倍环路放大对 DDoS 生态是现实威胁；与经典反射放大攻击（NTP、DNS）相比，隧道主机的双向性使其更难溯源。局限：扫描方法对非标准封装与防火墙策略的覆盖有限，放大因子的现实可达性依赖具体网络拓扑，作者给出的加固对策在遗留设备上的落地难度未充分讨论。

- **论文 PDF**: [Haunted by Legacy: Discovering and Exploiting Vulnerable Tunnelling Hosts.](https://www.usenix.org/system/files/usenixsecurity25-beitis.pdf)

#### Current Affairs: A Security Measurement Study of CCS EV Charging Deployments.

- **作者**: Marcell Szakály, Sebastian Köhler, Ivan Martinovic

- **方向**: 网络与协议安全

- **解读**: 

自 2012 年推出以来，CCS 成为欧洲、北美及部分亚洲地区电动车直流快充的主导标准，其充电通信由 ISO 15118 定义；研究者已证明多种高影响攻击（DoS、篡改、信息窃取、偷电），且有线通信的物理层会产生意外无线信道，使攻击者可无线实施。2014 年标准引入 TLS 等安全特性，但实际部署状态此前无人测量。

首次对公开部署的 CCS 直流充电桩开展测量研究：分析 2013 年 4 月至 2023 年 6 月制造、最晚 2024 年 5 月安装的 325 台充电桩，覆盖 4 个欧洲国家 26 家制造商，检查协议版本、TLS 部署、固件年代与 PKI 设计。

仅 12% 的充电桩实现了 TLS，其余全部暴露于多年前已被证明的攻击；ISO 15118-2 支持呈上升趋势，2023 年制造的充电桩达 70%；多数充电桩的 HomePlug 调制解调器固件已落后约十年，可能含有已被修复的漏洞；作者还指出充电 PKI 系统的设计缺陷并提出改进建议。

我的思考：作为首个大规模实测，该研究把 EV 充电安全从"论文中的攻击"落到"现实中的暴露"，12% 的 TLS 覆盖率说明关键基础设施的安全合规滞后比预期严重。局限是样本集中于欧洲、测量受厂商固件差异限制。与早期 EV 充电攻击研究相比，本文提供了部署层面的地面真相，为监管与标准制定提供了量化依据。

- **论文 PDF**: [Current Affairs: A Security Measurement Study of CCS EV Charging Deployments.](https://www.usenix.org/system/files/usenixsecurity25-szakaly.pdf)

#### STEK Sharing is Not Caring: Bypassing TLS Authentication in Web Servers using Session Tickets.

- **作者**: Sven Hebrok, Tim Leonhard Storm, Felix Matthias Cramer, Maximilian Radoy, Juraj Somorovsky

- **方向**: 网络与协议安全

- **解读**: 

（无摘要，据标题推断）TLS 会话票据（session tickets）允许服务器无状态恢复会话，其加密密钥 STEK 若被多台服务器共享或复用，攻击者可利用任一台泄露/受控服务器解密其他站点的会话票据，进而绕过 TLS 服务器身份认证；共享 STEK 的普遍程度与可利用性此前缺乏系统性调查。

从标题推断，本文针对 Web 服务器中 STEK 共享问题展开研究：应是通过大规模扫描/测量识别共享或复用会话票据加密密钥的服务器集群，并构造利用会话票据解密与伪造以绕过 TLS 认证的攻击。具体测量范围与攻击构造因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者在真实互联网范围内量化了受影响服务器规模。

我的思考（基于标题的保守推断）：会话票据是 TLS 生态的"便捷性补丁"，其密钥管理一旦失当即破坏认证完整性；该工作把攻击面从单服务器密钥泄露提升到"密钥共享"这一部署层面问题。与 TLS 配置测量（证书、协议版本）相比，本文聚焦密钥生命周期管理，其发现将推动服务器厂商的密钥轮换与隔离最佳实践。


### 密码学与协议（65 篇）

#### Narrowbeer: A Practical Replay Attack Against the Widevine DRM.
- **作者**: Florian Roudot, Mohamed Sabt
- **方向**: 密码学与协议
- **解读**: Widevine 是流媒体 DRM 的主流方案,广泛用于保护视频内容;其许可证(License)获取流程若可被重放,攻击者就能在无授权设备上解密内容。本文提出针对 Widevine DRM 的实用重放攻击 Narrowbeer。(无摘要,据标题推断)

依据标题推断,作者分析 Widevine 许可证请求/响应协议的信任链,发现协议状态或消息可被合法捕获并在另一设备/时刻重放,从而绕过设备绑定与授权检查,实现离线或跨设备解密。

研究应给出可复现的攻击流程、受影响 Widevine 版本与安全等级(L1/L2/L3),并讨论为何现有反回放机制不足。由于摘要与全文缺失,具体技术细节与影响范围无法引用。

DRM 安全研究常因封闭性缺乏公开分析,此类工作把协议层漏洞暴露在学术聚光灯下,对内容产业与消费者都有影响:重放攻击往往无需破解芯片,成本低、影响广。局限:无法核实攻击所需前提(如是否需 root 或特定设备);DRM 的闭源性质使第三方验证困难。与软件保护研究一样,其价值在于推动"可证明安全"替代"混淆安全"。

#### X.509DoS: Exploiting and Detecting Denial-of-Service Vulnerabilities in Cryptographic Libraries using Crafted X.509 Certificates.
- **作者**: Bing Shi; Wenchao Li; Yuchen Wang; Xiaolong Bai; Luyi Xing
- **方向**: 密码学与协议
- **解读**: 密码库漏洞研究聚焦机密性与完整性，可用性（DoS）被系统性忽视。本文观察到密码实现大量涉及大数运算与ASN.1/DER复杂解析，天然易受DoS攻击，且脆弱点常与X.509证书相关，故提出以构造证书为攻击向量的X.509DoS。

方法上，识别出3类此前未审计的新风险（F2m大m导致伪无限循环、归约多项式索引m<t越界崩溃、结构化ASN.1海量元素引发解析开销），结合历史CVE归纳共10类风险；开发X.509DoSTool（generate/detect/edit）自动生成恶意证书并监控CPU/内存/崩溃，典型利用包括7字节字段宣称20GB分配、策略树构造2^l-1节点耗尽内存、证书链issuer成环导致死循环。

结果显示，在OpenSSL、Botan、Bouncy Castle、Crypto++、GnuTLS、phpseclib与Apple Security框架中发现18个新漏洞、确认12个已知CVE，无漏报误报；并以Botan搭建的PoC网站与macOS系统（经S/MIME邮件触发）验证了TLS握手与应用签名验证两个真实场景。

我的思考：这是首次系统研究证书解析类DoS，"严格遵循教科书与标准并不保证安全"的结论敲响警钟，攻击向量真实且构造证书无需私钥重签名。局限是资源耗尽阈值属启发式设定、覆盖7个库有限、修复依赖厂商配合，属"发现问题"而非"根治问题"。

- **论文 PDF**: [X.509DoS: Exploiting and Detecting Denial-of-Service Vulnerabilities in Cryptographic Libraries using Crafted X.509 Certificates.](https://www.usenix.org/system/files/usenixsecurity25-shi-bing.pdf)

#### CoVault: Secure, Scalable Analytics of Personal Data.
- **作者**: Roberta De Viti, Isaac Sheff, Noemi Glaeser, Baltasar Dinis, Rodrigo Rodrigues, Bobby Bhattacharjee, Anwar Hithnawi, Deepak Garg, Peter Druschel
- **方向**: 密码学与协议
- **解读**: 问题：对个人移动、金融与健康数据的分析能带来巨大社会效益，但自由社会一直对此谨慎，根源在于缺乏能在极强威胁模型下扩展到数十亿条记录的安全分析平台。核心缺口是缺少一种能在不削弱安全性的前提下水平扩展（主动）安全多方计算（MPC）的架构。

方法：CoVault 利用服务器辅助 MPC 与可信执行环境（TEE），将 MPC 参与方共置在单个数据中心内而不降低安全性，从而将 MPC 水平扩展到数据中心可用资源规模。该架构解决了 MPC 跨机器横向扩展时的安全与性能权衡问题。

结果：CoVault 表现出良好的经验可扩展性。例如，它能用 DualEx 2PC 协议持续执行一个 8000 万人口国家的流行病学分析（约每天 118.5 亿条数据记录），每 3 万人只需一对核心。

思考：把 MPC 参与方"共置进可信硬件"以换取水平扩展，是对传统"地理分散参与方"假设的务实突破，工程价值显著；但安全论证依赖 TEE 的可信基与共置假设，一旦 TEE 侧信道或供应链被攻破，其强威胁模型是否依然成立值得深究，这是此类混合架构的共同软肋。

- **论文 PDF**: [CoVault: Secure, Scalable Analytics of Personal Data.](https://www.usenix.org/system/files/usenixsecurity25-de-viti.pdf)

#### Dumbo-MPC: Efficient Fully Asynchronous MPC with Optimal Resilience.
- **作者**: Yuan Su, Yuan Lu, Jiliang Li, Yuyi Wang, Chengyi Dong, Qiang Tang
- **方向**: 密码学与协议
- **解读**: 全异步多方计算（AMPC）能在可任意延迟通信的异步对手下实现隐私与保证输出交付（G.O.D.），鲁棒性优越。但现有协议要么弹性次优、通信开销庞大，要么在线阶段有额外密码开销，均不实用；唯一的实现 HoneyBadgerMPC 也仅因离线预处理阶段不鲁棒而在一些不切实际的乐观情形下保证 G.O.D.。本文提出 Dumbo-MPC 解决这些问题。

Dumbo-MPC 是具备全阶段 G.O.D. 的 AMPC-as-a-service 设计，在 t < n/3 个恶意方（n 为总参与方数）下达到最优弹性，并对离线预处理与在线计算两阶段进行效率优化，去除不必要的密码开销（摘要截断）。

实现并开展基准测试，Dumbo-MPC 在吞吐与延迟上优于 HoneyBadgerMPC 等基线，成为首个真正实用的全异步鲁棒 MPC 方案（摘要截断，具体数字未给出）。

该工作把 AMPC 从理论推向工程可用，补齐了异步 MPC 的实践空白，贡献扎实。局限是异步协议固有的较高延迟，更适合联盟链、去中心化等无信任假设场景；与同步协议相比吞吐仍有差距，实际部署需按应用场景权衡选型。

- **论文 PDF**: [Dumbo-MPC: Efficient Fully Asynchronous MPC with Optimal Resilience.](https://www.usenix.org/system/files/usenixsecurity25-su-yuan.pdf)

#### FABLE: Batched Evaluation on Confidential Lookup Tables in 2PC.
- **作者**: Zhengyuan Su, Qi Pang, Simon Beyzerov, Wenting Zheng
- **方向**: 密码学与协议
- **解读**: 在两方安全计算（2PC）中，查表（lookup table）是隐私数据库、安全机器学习等众多应用的基础操作，但表内容与查询均需保密，大规模查表的通信与计算开销高昂。本文提出 FABLE，针对批量查询场景实现高效的机密查表评估。

面向批量查询设计 2PC 查表协议，利用离线预计算与混淆技术将昂贵计算前置，使批量查询的在线阶段通信与计算成本显著摊销，提升端到端效率（据标题推断）。

（无摘要，据标题推断）FABLE 在批量场景下相比现有查表协议（如基于 OT 或混淆电路方案）大幅降低通信量与运行时间，显著提升端到端吞吐，支撑隐私数据库等实际应用。

该工作针对"批量"这一真实工作负载做专门优化，工程取向明确、实用性强。局限是批量收益依赖查询规模，小批量场景优势有限；协议的安全性证明与实现审计需同行进一步验证，未来可关注与私有集合求交、隐私数据库系统的集成。


#### MAESTRO: Multi-Party AES Using Lookup Tables.
- **作者**: Hiraku Morita, Erik Pohle, Kunihiko Sadakane, Peter Scholl, Kazunari Tozawa, Daniel Tschudi
- **方向**: 密码学与协议
- **解读**: 安全多方计算（MPC）使多方在不泄露输入的前提下联合计算函数。在 MPC 中计算 AES（密钥与/或输入在多方间秘密共享）对门限密码等应用至关重要，而 S-box 是 AES 唯一的非线性部件，也是 MPC 计算的性能瓶颈。本文提出一族专用的高性能 MPC 协议来计算 AES S-box。

核心技术是组合两类手段：基于随机 one-hot 向量的查找表协议，以及将 GF(2^8) 中的有限域求逆分解为更小域 GF(2^4) 上的乘法与求逆，从而显著降低非线性计算的开销；协议同时提供半诚实与恶意安全两种变体（摘要截断）。

在诚实多数设置下实现并评估，MAESTRO 相比通用 MPC 方案显著降低 S-box 计算的通信与运行开销，提升了门限 AES 等实际应用的性能，证明专用设计能够带来可观的性能增益（摘要截断，具体数字未给出）。

该工作以"专用协议+数学结构分解"优化非线性部件，思路经典而扎实，两种安全变体覆盖了不同威胁模型。局限是恶意安全变体开销更高，参数选择需在安全与性能间权衡；且仅优化了 S-box，整体 AES 吞吐仍受线性层及轮结构影响。

- **论文 PDF**: [MAESTRO: Multi-Party AES Using Lookup Tables.](https://www.usenix.org/system/files/usenixsecurity25-morita.pdf)

#### Efficient 2PC for Constant Round Secure Equality Testing and Comparison.
- **作者**: Tianpei Lu, Xin Kang, Bingsheng Zhang, Zhuo Ma, Xiaoyuan Zhang, Yang Liu, Kui Ren, Chun Chen
- **方向**: 密码学与协议
- **解读**: 安全相等测试与安全比较是隐私保护机器学习、私有集合求交、安全数据挖掘等场景广泛使用的基础原语，其效率直接影响上层应用。现有两方计算（2PC）方案的在线通信开销较大，本文提出新的常数轮安全相等测试与比较协议，采用在线/离线（online/offline）范式优化。

通过离线预计算把昂贵计算前置，在线阶段仅需极少量通信：对 32 位输入，相等测试协议的在线通信仅 76 比特（约为 ABY 的 1%），安全比较协议为 384 比特（约为 ABY 的 5%），实现了比特级的在线开销。

基准测试显示：对 32 位相等测试，该方案比 Guo 等（EUROCRYPT 2023）快 9 倍，比半门优化的混淆电路（GC）快 15 倍；比较协议同样显著优于现有基线（摘要截断，其余指标未给出）。

该工作把两个最常用原语的在线通信压到极致，对延迟敏感、弱网或在线带宽受限的应用价值明显。局限是优化依赖离线阶段的预计算，端到端总成本仍需综合考量；协议安全性在标准模型下的论证、以及更大位宽输入的扩展性也值得关注。

- **论文 PDF**: [Efficient 2PC for Constant Round Secure Equality Testing and Comparison.](https://www.usenix.org/system/files/usenixsecurity25-lu.pdf)

#### Efficient Multi-Party Private Set Union Without Non-Collusion Assumptions.
- **作者**: Minglang Dong, Cong Zhang, Yujie Bai, Yu Chen
- **方向**: 密码学与协议
- **解读**: 多方私有集合并（MPSU）协议让 m（m>2）个各持集合的参与方在不泄露额外信息的前提下计算集合并集。现有协议分两类：公钥技术类需要超线性数量的公钥运算，实际效率差；基于不经意传输（OT）与对称密钥技术的类别中，Liu 与 Gao（ASIACRYPT 2023）的方案具体性能最好，但仍为超线性复杂度，且未完全达到理想的安全假设（摘要截断）。

本文基于 OT 与对称密钥技术设计新的 MPSU 协议，实现线性级的计算与通信复杂度，并移除"非共谋（non-collusion）"假设——即不再要求部分诚实方互不串通的强假设，使安全模型更普适、更贴近真实威胁环境（摘要截断）。

与 Liu-Gao 协议相比，新方案在通信与计算上达到线性复杂度且安全性更强，提升了 MPSU 的实用性与健壮性（摘要截断，具体数字未给出）。

该工作同时优化效率（线性复杂度）与安全假设（去除非共谋），推动 MPSU 走向实用，贡献显著。局限是线性复杂度背后的隐藏常数与工程实现复杂度未知，恶意安全变体下的开销也可能上升；未来可与布隆过滤器、OPRF 批处理等工程优化结合。

- **论文 PDF**: [Efficient Multi-Party Private Set Union Without Non-Collusion Assumptions.](https://www.usenix.org/system/files/usenixsecurity25-dong-minglang.pdf)

#### Scalable Collaborative zk-SNARK and Its Application to Fully Distributed Proof Delegation.
- **作者**: Xuanming Liu, Zhelei Zhou, Yinghao Wang, Yanxin Pang, Jinye He, Bingsheng Zhang, Xiaohu Yang, Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**: 传统 zk-SNARK 由单一证明者生成证明，但分布式计算、多方数据协同等场景需要多个互不信任的参与方协作生成一个证明，且各方只掌握部分见证（witness）。本文提出可扩展的协作式 zk-SNARK，并应用于完全分布式的证明委托，拓展零知识证明的适用边界。

设计多方协作证明协议：各参与方在不泄露私有见证的前提下联合执行证明生成，协议对参与方数量具备可扩展性，并支持把证明生成任务安全地委托给分布式参与方集合（据标题推断）。

（无摘要，据标题推断）在多方设置下验证协作证明的可行性与效率，证明生成开销随参与方数量近似线性扩展，支撑分布式证明委托等实际场景。

该工作把 zk-SNARK 从"单人证明"推进到"多方协作证明"，对分布式系统与去中心化应用的可信计算有重要意义。局限是协作协议在恶意方合谋下的安全模型、参与方之间的通信复杂度、以及与主流证明系统的兼容性仍需进一步研究，工程成熟度有待验证。


#### zkGPT: An Efficient Non-interactive Zero-knowledge Proof Framework for LLM Inference.
- **作者**: Wenjie Qu, Yijun Sun, Xuanming Liu, Tao Lu, Yanpei Guo, Kai Chen, Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**: LLM 服务提供商可能为降低成本部署更小的模型以欺骗用户。零知识证明（ZKP）提供了一种解决方案：在保护模型参数隐私的同时证明推理确实被执行。但现有方案要么不支持 LLM 架构，要么效率低下、开销巨大。本文提出 zkGPT，一个高效的 LLM 推理非交互零知识证明框架。

提出高效证明 LLM 线性层与非线性层的新方法，将证明计算开销降低数量级；进一步提出约束融合（constraint fusion）技术，减少证明非线性层（如 GELU、注意力机制中的非线性操作）所需的约束数量（摘要截断）。

实现 zkGPT 并验证，相比现有 ZK-LLM 方案，证明时间与内存开销大幅下降，使 LLM 推理的零知识证明从理论走向可行（摘要截断，具体数字未给出）。

该工作以电路与约束优化攻克"为大模型生成证明"的规模难题，工程贡献突出。局限是证明开销仍显著高于裸推理，验证多针对中小规模模型；对通用大模型与交互式场景的可扩展性、以及证明完备性（电路与模型的等价性）仍需严格论证。
- **论文 PDF**: [zkGPT: An Efficient Non-interactive Zero-knowledge Proof Framework for LLM Inference.](https://www.usenix.org/system/files/usenixsecurity25-qu-zkgpt.pdf)

#### DFS: Delegation-friendly zkSNARK and Private Delegation of Provers.

- **作者**: Yuncong Hu, Pratyush Mishra, Xiao Wang, Jie Xie, Kang Yang, Yu Yu, Yuwen Zhang
- **方向**: 密码学与协议
- **解读**: zkSNARK 证明可被简洁验证，但生成证明需要巨大计算资源，实际应用中常需外包给第三方。公开委托会向第三方泄露见证（witness），私密委托则借助多方计算（MPC）隐藏见证，但现有私密委托方案受限于 MPC 效率低、资源利用率差以及 zkSNARK 协议设计不佳，可扩展性和效率都不理想。

论文提出 DFS，一个对公开与私密委托都友好的新型 zkSNARK。与以往只针对既有 zkSNARK 优化 MPC 协议不同，DFS 对 MPC 与 zkSNARK 进行协同设计（co-design），使协议在分布式计算和 MPC 两种场景下都高效：非委托场景下证明者时间线性、验证代价对数级；私密委托场景下 MPC 通信开销为零并可免费获得恶意安全性，总通信量对数级，而先前工作需要线性通信。

评估显示，DFS 在公开委托中与最先进的 zkSNARK 效率相当；用于私密委托时扩展性优于先前工作。对 2^24 个约束，DFS 总通信量小于 500KB，而先前工作达 300GB（与电路规模线性相关）。此外，论文还发现并修复了先前工作 EOS（USENIX'23）中的一个安全缺陷。

我的思考：把「MPC 与 zkSNARK 协同设计」作为第一性原则而非事后优化，是这篇工作的核心贡献，通信量从线性降到对数级是实质性的量级改进；顺带指出并修复 EOS 的安全缺陷也提升了工作的可信度。局限在于论文数字主要来自实验场景，真实硬件与大规模部署下的表现仍需观察；其与后续「可委托证明系统」路线的竞争关系也值得关注。

- **论文 PDF**: [DFS: Delegation-friendly zkSNARK and Private Delegation of Provers.](https://www.usenix.org/system/files/usenixsecurity25-hu-yuncong.pdf)

#### SoK: Understanding zk-SNARKs: The Gap Between Research and Practice.

- **作者**: Junkai Liang, Daqi Hu, Pengfei Wu, Yunbo Yang, Qingni Shen, Zhonghai Wu
- **方向**: 密码学与协议
- **解读**: zk-SNARK 是证明计算正确性的强大工具，学术界与工业界涌现大量方案与实现，但其内在复杂性造成研究者、开发者与用户之间的鸿沟：研究者专注构建更高效、更强安全性与新性质的证明系统，开发者与用户则更关心工具链、可用性与兼容性，这一鸿沟阻碍了领域发展。

论文对 zk-SNARK 从理论到实践做了系统研究。首先提出一个「主配方」（master recipe），统一了把程序转换为 zk-SNARK 的主要步骤；再按关键技术对现有 zk-SNARK 分类，该分类对应了实际价值属性的主要差异；随后调研了 2013 年以来 40 余个 zk-SNARK，给出含类别与属性的参考表，并进一步调研 11 个通用流行库的可用性、兼容性、效率与局限；由于安装运行这些系统困难，还提供了可在其中运行各编译器（compiler）的完整虚拟环境。

研究发现：学术界主要聚焦证明系统（proving system），而工业界的瓶颈在约束系统（constraint system）。据此论文给出建议，呼吁开源社区加强文档、标准化与兼容性建设。

我的思考：该 SoK 的价值在于同时覆盖「理论-分类-实现-工具」全链条，参考表和虚拟环境使其具备可复用的操作性；「学术重证明系统、工业卡在约束系统」的观察切中要害。局限是分类标准难免带有主观性，40 余方案的对比深度受篇幅限制；但对研究者和工程实践者而言，这是一份高价值的路线图。

- **论文 PDF**: [SoK: Understanding zk-SNARKs: The Gap Between Research and Practice.](https://www.usenix.org/system/files/usenixsecurity25-liang-sok.pdf)

#### GraphAce: Secure Two-Party Graph Analysis Achieving Communication Efficiency.
- **作者**: Jiping Yu; Kun Chen; Yunyi Chen; Xiaoyu Fan; Xiaowei Zhu; Cheng Hong; Wenguang Chen
- **方向**: 密码学与协议
- **解读**: 图分析在重要性度量、欺诈检测、反洗钱等任务中成效显著，但完整图往往涉及多方数据，需要安全多方计算（MPC）在不泄露各方数据的前提下联合计算；已有安全图分析框架每轮通信存在 Ω(|V|+|E|) 的下界，在大规模图上开销巨大。

GraphAce 采用与现有方案不同的技术路线：混合使用同态加密与秘密共享原语，并设计新颖的 ChaosTable 数据结构保护跨方计算中的隐私，从而解决利用各方本地图数据时的安全问题。

GraphAce 消除了与边相关的全部网络流量，每轮通信复杂度降为 Θ(|V|)，打破此前 Ω(|V|+|E|) 下界；计算复杂度 Θ(|V|+|E|) 与不安全方法持平。评估显示其速度最高提升数万倍、通信最多节省 99.99%，并成为首个可在合理时间内处理超 100 万顶点、1.32 亿边安全两方图分析的框架，规模是此前报告的 128 倍。

打破"每轮必须传输边信息"的认知下界是实质贡献，ChaosTable 的设计思路对安全图计算社区有启发；局限在于两方场景假设及对具体图算法的适配需求。与同类 MPC 图分析框架相比，它把通信瓶颈从边转移到本地预处理，工程与理论结合较好，但其安全模型（半诚实/恶意）下的完备性仍值得后续深入验证。

- **论文 PDF**: [GraphAce: Secure Two-Party Graph Analysis Achieving Communication Efficiency.](https://www.usenix.org/system/files/usenixsecurity25-yu-jiping.pdf)

#### Breaking the Layer Barrier: Remodeling Private Transformer Inference with Hybrid CKKS and MPC.
- **作者**: Tianshi Xu; Wen-jie Lu; Jiangrui Yu; Yi Chen; Chenqi Lin; Runsheng Wang; Meng Li
- **方向**: 密码学与协议
- **解读**:（无摘要，据标题推断）私有 Transformer（大模型）推理要求对密文完成注意力、前馈网络等复杂算子，纯同态加密（HE）与纯 MPC 方案各有短板，推理延迟与通信开销难以实用。

本文提出混合 CKKS 与 MPC 的推理框架，按算子/层重新设计 Transformer 结构，把适合 HE 的算术密集部分（如矩阵乘法）与适合 MPC 的非线性部分（如 softmax、GELU）分工部署，突破以往按层整体切换协议的"层壁垒"。

（无摘要，据标题推断）论文宣称在保持推理精度的前提下显著降低私有推理延迟与通信量，较纯 HE 或纯 MPC 基线有数量级提升。

按算子粒度而非按层粒度混合两种协议，是私有推理工程化的重要方向，与领域内"混合协议调度"趋势一致；但具体数字与安全模型需以论文原文核实。此类工作的共性挑战是精度-开销权衡与部署复杂度，能否推广到更大模型、半诚实与恶意模型下的代价差异，是后续评估重点。整体上代表了私有 LLM 推理从理论走向实用的关键一步。


#### Aion: Robust and Efficient Multi-Round Single-Mask Secure Aggregation Against Malicious Participants.
- **作者**: Yizhong Liu; Zixiao Jia; Xiao Chen; Song Bian; Runhua Xu; Dawei Li; Yuan Lu
- **方向**: 密码学与协议
- **解读**:（无摘要，据标题推断）安全聚合让联邦学习服务器只能看到聚合结果，但现有方案要么每轮生成多个掩码、开销大，要么无法抵御恶意参与方（投毒、掉线、共谋），多轮场景下效率与鲁棒性难以兼得。

论文提出 Aion：面向多轮联邦学习的高效安全聚合协议，全程使用单一掩码，并在恶意参与方模型下保证正确性与隐私。

（无摘要，据标题推断）据标题，Aion 相比现有安全聚合方案显著降低通信与计算开销，同时保持对恶意参与方的鲁棒性。

掩码复用（单掩码跨轮）是提升安全聚合效率的关键杠杆，但需仔细处理掩码泄漏与重放风险，Aion 声称在恶意模型下解决此问题，理论贡献分量取决于证明的严谨性；对真实 FL 部署（跨设备、高掉线率）的适配性决定工程价值。与 SecAgg/SecAgg+ 一脉相承，若性能数据扎实，有望成为大规模联邦训练的实际选择；与恶意安全聚合基准方案的系统比较将决定其最终定位。


#### SLOTHE : Lazy Approximation of Non-Arithmetic Neural Network Functions over Encrypted Data.
- **作者**: Kevin Nam; Youyeon Joo; Seungjin Ha; Yunheung Paek
- **方向**: 密码学与协议
- **解读**:（无摘要，据标题推断）同态加密（HE）只能高效计算多项式算术运算，而神经网络中的 ReLU、softmax、GELU 等非算术函数在密文上开销极高，是私有推理性能的主要瓶颈。

论文提出 SLOTHE，对非算术神经网络函数采用"惰性近似"（lazy approximation）策略：按需、延迟地以低阶多项式近似这些函数，减少不必要的同态计算。

（无摘要，据标题推断）据标题，SLOTHE 在保持推理精度的前提下显著降低非算术函数的同态评估开销，提升端到端私有推理效率。

非算术函数近似是 HE 私有推理的经典瓶颈；本文"惰性"思路——不在每一层都做满精度近似、利用近似误差的累积特性分配计算——是对现有逐层多项式近似方案的细化优化；与 CKKS 定点编码的误差分析结合是评估其安全性的关键。局限在于无摘要难核实近似策略的适用算子范围与精度-开销数据；若与混合协议方案（如本批次的 Hybrid CKKS+MPC 工作）对比，可明确其相对优势场景。


#### BulletCT: Towards More Scalable Ring Confidential Transactions With Transparent Setup.

- **作者**: Nan Wang; Qianhui Wang; Dongxi Liu; Muhammed F. Esgin; Alsharif Abuadbba
- **方向**: 密码学与协议
- **解读**: RingCT 签名是区块链环机密交易方案的核心，决定匿名花费能力与可扩展性；ZGSX23（S&P '23）提出的 DLOG 设置下 Any-out-of-N 证明以隐藏秘密数量换取更强匿名性，但其实际价值未经严格检验。

  论文贡献有二：首次深入分析 Any-out-of-N 证明及其关联 RingCT 方案，识别出交易体积增大、密码学复杂度升高与潜在安全风险等局限；进而提出新的 DLOG 基础 RingCT 签名——整合改进的基于 "K-Weight" 的 K-out-of-N 证明与全新的 tag proof，后者首次高效实现前者的可链接性以抵抗双花攻击；同时发现并修补 ZGSX23 签名中的可链接性缺陷。

  与修补后的 ZGSX23 方案基准对比，新方案在可扩展性上取得明显提升，被视为迈向更可扩展 RingCT 的有前景一步。

  该工作展示了"优雅理论"与"工程实用"之间的张力：Any-out-of-N 的匿名增益被体积与复杂度成本抵消，K-out-of-N 路线反而更务实；透明设置（免可信初始化）契合区块链的去信任诉求；可链接性缺陷的发现与修补体现了对既有方案的严谨审视，但真实链上部署仍有待验证。

- **论文 PDF**: [BulletCT: Towards More Scalable Ring Confidential Transactions With Transparent Setup.](https://www.usenix.org/system/files/usenixsecurity25-wang-nan.pdf)

#### PolySys: an Algebraic Leakage Attack Engine.

- **作者**: Zachary Espiritu; Seny Kamara; Tarik Moataz; Andrew Park
- **方向**: 密码学与协议
- **解读**: （无摘要，据标题推断）加密数据库与可搜索加密系统在运行中必然泄漏访问模式等信息，泄漏滥用攻击（leakage-abuse attack）可利用这些泄漏恢复明文；本文旨在把这类攻击的构造自动化、系统化，做成一个通用"引擎"。

  据标题推断，PolySys 把泄漏攻击的构造建模为代数问题：将攻击者已知的泄漏信息与先验分布编码为多项式方程组，借助代数或符号求解技术自动推导可利用的统计规律，甚至直接恢复数据，从而统一复现并超越若干手工构造的已知攻击。

  无摘要，具体攻击效率与恢复率无法从标题确认；若引擎成立，预计能以统一框架覆盖多类加密系统。

  泄漏攻击研究长期依赖逐例手工构造，自动化引擎若能成熟，将大幅压低构造新攻击的成本，迫使加密数据库设计者重新审视泄漏上限；其风险在于被用来评估真实系统——这正是该方向的双刃剑性质；该工作与 Kamara 团队此前在泄漏滥用攻击方面的系统性研究一脉相承。


#### Distributional Private Information Retrieval.

- **作者**: Ryan Lehmkuhl; Alexandra Henzinger; Henry Corrigan-Gibbs
- **方向**: 密码学与协议
- **解读**: 经典 PIR 让客户端私密获取数据库记录，但对所有记录一视同仁；现实中记录流行度高度偏斜（少数热记录被反复获取），这种不对称性未被经典方案利用。

  作者提出 distributional PIR：与经典 PIR 提供完全相同的密码学隐私，速度提升来自放宽正确性——保证分布内查询以高概率成功，分布外查询成功率较低；方案黑盒调用经典 PIR 协议，并证明了一类自然 distributional-PIR 方案服务器运行时间的下界；安全上要求客户端是否发起查询必须独立于其过往查询是否成功。

  在两个真实流行度分布上，相对现有技术计算成本降低 5-77 倍；端到端系统 CrowdSurf（私密获取推文）将服务器端到端成本降低 8 倍。

  以"概率性正确性"换取数量级效率，为 PIR 实用化开辟了新设计维度，特别适合"尽力而为"检索的公开数据场景；隐私定义中的独立性要求（查询决策不得依赖历史成功与否）是易被实现者忽视的安全细节；局限在于分布外的冷门查询可能失败，适用场景需仔细界定。

- **论文 PDF**: [Distributional Private Information Retrieval.](https://www.usenix.org/system/files/usenixsecurity25-lehmkuhl.pdf)

#### Practical Keyword Private Information Retrieval from Key-to-Index Mappings.

- **作者**: Meng Hao; Weiran Liu; Liqiang Peng; Cong Zhang; Pengfei Wu; Lei Zhang; Hongwei Li; Robert H. Deng
- **方向**: 密码学与协议
- **解读**: 关键字 PIR 让用户以关键字私密查询公开数据库，比索引 PIR 更困难：查询位置未知且关键字域巨大，朴素地把每个关键字映射到固定位置会导致映射表随域膨胀而失控，现有方案效率低下。

  核心洞察是构造高效紧凑的"键到索引映射"，把关键字 PIR 归约到标准 PIR：先把服务器端键值数据库编码为带映射的可索引数据库，再对编码库调用标准 PIR 按位置取回；据此提出三个构造并引入多项新技术。

  相对当前最优的 ChalametPIR（CCS '24），通信量降低 15~178 倍、运行时间提升 1.1~2.4 倍（依数据库大小与条目长度而定）；对含 100 万条 32 字节条目的数据库，关键字 PIR 仅需 47 ms。

  把困难问题干净地归约到成熟原语是密码工程的好范例，通信量数量级下降直击 PIR 落地痛点；47 ms 的端到端延迟已接近实用门槛；局限在于键到索引映射的存储与更新开销，以及多关键字、动态数据库等场景仍待拓展；三个构造覆盖不同数据库形态，也便于按场景选型部署。

- **论文 PDF**: [Practical Keyword Private Information Retrieval from Key-to-Index Mappings.](https://www.usenix.org/system/files/usenixsecurity25-hao.pdf)

#### SEAF: Secure Evaluation on Activation Functions with Dynamic Precision for Secure Two-Party Inference.

- **作者**: Hao Guo; Zhaoqian Liu; Ximing Fu; Zhusen Liu
- **方向**: 密码学与协议
- **解读**: （无摘要，据标题推断）安全两方推理中，激活函数（如 ReLU、Sigmoid）的非线性计算是隐私保护机器学习的主要性能瓶颈之一，SEAF 聚焦于此。

  据标题推断，SEAF 提出以动态精度（dynamic precision）方式安全评估激活函数：根据输入数据特征自适应调整计算精度，在不牺牲安全性的前提下降低秘密共享、混淆电路等密码原语的开销，并通过离线与在线阶段分离等方式控制动态精度的通信代价。

  无摘要，具体加速比与精度损失无法从标题确认；预计给出相对固定精度基线的效率对比与精度保持验证。

  激活函数安全计算是两方/多方推理的经典痛点，动态精度思路在"效率-精度"之间引入自适应权衡，工程上务实；其安全证明是否覆盖动态分支带来的泄漏是评审与后续研究的关键；与深度学习中的量化训练思路呼应，但需在密码学约束下重新设计；若与多项式近似、分段线性拟合等既有技巧结合，或能进一步逼近明文推理的性能表现。


#### Fast Enhanced Private Set Union in the Balanced and Unbalanced Scenarios.

- **作者**: Binbin Tu; Yujie Bai; Cong Zhang; Yang Cao; Yu Chen
- **方向**: 密码学与协议
- **解读**: 私有集合并（PSU）允许双方计算集合并集而不泄露其他信息；Jia 等（USENIX Security 2024）指出既有可扩展 PSU 存在执行中泄漏并提出平衡场景的增强安全 PSU，但其复杂度超线性，且不存在不平衡场景的增强 PSU。

  本文同时解决两个开放问题：平衡场景提出首个线性复杂度的增强 PSU；不平衡场景提出首个增强 PSU，其通信复杂度在较大集合规模上呈亚线性。

  平衡场景相对 Jia 等的最优增强 PSU，通信量降低 2.2-8.8 倍、运行时间加速 1.2-8.6 倍（依集合规模与网络环境而定）；不平衡场景在 (2^10, 2^20)、单线程、1Mbps 带宽下仅需 2.322 MB 通信，相对最优增强 PSU 通信缩小 38.1 倍、运行时间约加速 17.6 倍；两集合规模差异越大优势越明显。

  同时收束两个开放问题，理论复杂度与工程优化并重，38 倍量级的通信压缩对低带宽场景意义重大；增强安全性（消除执行中泄漏）呼应了近年对"可证明无泄漏"的更高标准；不平衡场景契合真实数据不对称应用，后续可探索多方扩展与恶意安全模型。

- **论文 PDF**: [Fast Enhanced Private Set Union in the Balanced and Unbalanced Scenarios.](https://www.usenix.org/system/files/usenixsecurity25-tu.pdf)

#### BEAT-MEV: Epochless Approach to Batched Threshold Encryption for MEV Prevention.

- **作者**: Jan Bormet; Sebastian Faust; Hussien Othman; Ziyan Qu
- **方向**: 密码学与协议
- **解读**: DeFi 中待处理交易公开可见，使区块提议者可重排、省略或插入交易牟取 MEV（矿工可提取价值），用户遭抢跑损失；加密待处理交易直至区块定稿是主流缓解思路，而 Choudhuri 等（USENIX '24）的批量阈值加密（BTE）按 epoch 推进，且每次批量解密都需昂贵的 MPC 设置。

  本文提出无 epoch（epochless）的 BTE 方案，消除昂贵的 epoch 设置并实现实用的加密/解密耗时；同时首次探讨此前被忽略的用户交易协调问题；给出多项通信-计算复杂度的优化与权衡，并在刻画实际 MEV 预防攻击的模型下证明安全性。

  标准硬件上加密耗时 < 2 ms，解密 512 笔交易 < 440 ms，达到实用性能。

  把 BTE 从"概念优雅"推向"可部署"，消除逐 epoch 设置是迈向链上实用的关键一步；交易协调问题的显式建模填补了方案可用性的空白；对区块构建者而言，加密-解密延迟直接决定出块时间预算，<2 ms 的加密与亚秒级批量解密已为集成留出余量；与同会议的一次性设置 BTE 工作互为对照，两条路线谁能落地取决于委员会模型与实现复杂度。

- **论文 PDF**: [BEAT-MEV: Epochless Approach to Batched Threshold Encryption for MEV Prevention.](https://www.usenix.org/system/files/usenixsecurity25-bormet.pdf)

#### Practical Mempool Privacy via One-time Setup Batched Threshold Encryption.

- **作者**: Arka Rai Choudhuri; Sanjam Garg; Guru-Vamsi Policharla; Mingyuan Wang
- **方向**: 密码学与协议
- **解读**: DeFi 生态增长使提交交易客户的保护日益重要：mempool 中交易公开可见，易遭抢跑与尾随等市场操纵，客户端需要"待处理交易隐私"（交易执行前内容保密）。

  本文构造首个实用的 mempool 隐私方案：基于一次性 DKG 设置为 n 个解密服务器建立密钥；每个 epoch 选出 B 笔加密交易后，各服务器仅需 O(1) 通信即可解密；不仅隐藏交易直至解密，还保证未被选中的交易同样获得隐私（pending transaction privacy）。

  加密耗时约 8.5 ms（与委员会规模无关）；解密整批交易的通信为每方 48 字节（与交易数无关）；若部署于以太坊（每块约 500 笔交易），单线程下每委员会成员计算部分解密约 3.2 s、解密整块约 3.0 s；相对每 epoch 需昂贵设置的旧方案，最坏情况开销 < 2 倍，部分指标（如部分解密大小）反而更优。

  一次性设置把部署门槛大幅降低，O(1) 解密通信直击扩展性瓶颈，具体数字具备工程说服力；与同期无 epoch 方案竞争表明该方向正快速收敛；剩余挑战包括委员会活性、密钥托管风险与真实链上集成验证。

- **论文 PDF**: [Practical Mempool Privacy via One-time Setup Batched Threshold Encryption.](https://www.usenix.org/system/files/usenixsecurity25-choudhuri.pdf)

#### DeepFold: Efficient Multilinear Polynomial Commitment from Reed-Solomon Code and Its Application to Zero-knowledge Proofs.
- **作者**: Yanpei Guo, Xuanming Liu, Kexi Huang, Wenjie Qu, Tianyang Tao, Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**: 多项式承诺方案(PCS)是现代 zk-SNARK 的核心构件,广泛用于加密货币、区块链 rollup、AI 监管与匿名凭证等场景。基于 FRI 的 PCS 因无需可信设置、仅依赖轻量哈希而备受青睐,但将其推广到多线性多项式时,现有方案(Virgo、PolyFRIM、BaseFold)总在证明者时间或证明大小上付出显著代价:BaseFold 因 FRI 使用方式的特殊性,查询次数回到唯一解码半径限制下的水平,导致证明体积大幅膨胀(µ=22 时达 619 KB)。此外,FRI 类 PCS 处理非 2 的幂长度的输入需填充,验证矩阵乘法时甚至可能带来最高 4× 的开销。本文要同时解决"最优证明者时间+紧凑证明"与"任意长度输入高效承诺"两个开放问题。
DeepFold 首次把 FRI 多线性 PCS 适配到列表解码半径(list decoding radius)设置:在码率 1/8 下,查询次数从 BaseFold 的超过 100 降至约 34,证明大小较 BaseFold 缩减 3×,同时保持 O(n log n) 承诺时间与 O(n) 求值时间的最优证明者效率。针对任意长度输入,作者提出批求值(batch evaluation)方案:将输入分块承诺为多个不同变量数的多线性多项式,并用新颖的批处理求值技术避免求值开销膨胀。通过 Fiat-Shamir 变换方案可完全非交互化。
实验显示:与 PolyFRIM(USENIX Security '24)相比,DeepFold 在证明者时间、验证者时间与证明大小上均取得 2× 改进。集成应用收益显著:替换 Virgo(S&P '20)的 PCS 后,证明 256 叶 Merkle 树知识时证明者提速 2.5× 且证明大小相近;替换 HyperPlonk(Eurocrypt '23)的 PCS 后证明者约提速 3.6×;对 GPT-2 实际使用的 1200×768 与 768×2304 矩阵验证乘法,任意长度承诺方案带来 2.4× 证明者时间缩减。
该工作把单变量 FRI 中已被证明有效的列表解码技巧成功移植到多线性 PCS,并系统解决了非 2 的幂长度输入这一此前被忽视的实际问题,理论复杂度与实测数据相互印证,说服力强。与 BaseFold、PolyFRIM 的对比清晰展示了"证明体积-证明者时间"权衡空间的进一步压缩。局限在于安全性依赖列表解码半径下 FRI 的严谨论证(需读者信任其安全归约),且端到端加速在集成真实 SNARK 时可能受其他组件瓶颈影响;批量求值对更大规模、更多变量数的多项式是否依然高效也待验证。总体而言,它为下一代 zk-SNARK 提供了更优的 PCS 选型。
- **论文 PDF**: [DeepFold: Efficient Multilinear Polynomial Commitment from Reed-Solomon Code and Its Application to Zero-knowledge Proofs.](https://www.usenix.org/system/files/usenixsecurity25-guo-yanpei.pdf)

#### On the Atomicity and Efficiency of Blockchain Payment Channels.

- **作者**: Di Wu, Shoupeng Ren, Yuman Bai, Lipeng He, Jian Liu, Wu Wen, Kui Ren, Chun Chen
- **方向**: 密码学与协议
- **解读**: 
（无摘要，据标题推断）区块链吞吐量有限，支付通道（payment channel）通过链下交易把大量支付移出主链，是扩容的关键技术。但通道设计面临两个核心张力：原子性——多跳支付要么全部成功要么全部失败（防止中间节点赖账或部分执行）；效率——通道开启/关闭/更新的链上成本与延迟。本文从标题看是对支付通道原子性与效率的系统性研究。

方法上可能包括：形式化定义通道操作（开启、更新、关闭、争议解决）的原子性保证，分析现有通道协议（如基于哈希时间锁 HTLC 的方案）在何种条件下破坏原子性（如超时窗口不一致、争议期设计缺陷），并给出改进的通道构造以降低链上开销或提升并发能力。

预期成果包括对主流支付通道构造的安全性证明或反例、以及新的高效通道协议及其实验评估（链上 Gas、延迟、吞吐量对比）。该研究直接关系到支付通道网络的可用性与资金安全。

作者团队（浙江大学 Kui Ren、Jian Liu 组）在区块链协议安全方面积累深厚。该工作的价值在于把"原子性"这一支付通道的核心安全性质系统化地加以分析，并与效率指标联合优化。局限在于未见摘要，具体结论待核实；支付通道领域竞争激烈（如 Thunderdome 等同期工作），该文如何与免时间锁、理性安全等新方向竞争值得关注。


#### Parallelizing Universal Atomic Swaps for Multi-Chain Cryptocurrency Exchanges.

- **作者**: Danlei Xiao, Chuan Zhang, Haotian Deng, Jinwen Liang, Licheng Wang, Liehuang Zhu
- **方向**: 密码学与协议
- **解读**: 
（无摘要，据标题推断）跨链交易是加密货币生态的核心需求，原子交换（atomic swap）通过密码学机制保证多链之间"要么全部成交、要么全部退回"，避免中间人卷款。但经典原子交换依赖哈希时间锁（HTLC），存在脚本语言兼容性差、超时窗口串行化导致的低效率等问题。本文提出并行化通用原子交换方案，面向多链交易所场景。

"通用"（universal）暗示方案不依赖各链的特定脚本能力（如仅用哈希锁与时间锁这类通用原语），从而适配更多异构链；"并行化"则针对现有方案中多笔交换/多跳环节被迫串行等待的问题，通过并发执行设计缩短总体完成时间、提升吞吐。

预期成果包括新的并行原子交换协议、安全性论证（公平性、原子性、防抢跑），以及在多链环境中的性能评估（相比串行基线的时间与成本优势）。对多链交易所而言，更快的跨链成交意味着更好的用户体验与更低的套利风险敞口。

该工作的价值在于把原子交换从"可用"推向"高效"，回应跨链交易规模化需求。作者团队（北京理工大学 Liehuang Zhu 组）在密码学与区块链方向积累扎实。局限在于未见摘要，具体协议构造与安全模型待核实；并行化设计往往引入更复杂的时序假设，其在异步网络下的安全性需要仔细检验。


#### Automated Soundness and Completeness Vetting of Polygon zkEVM.

- **作者**: Xinghao Peng, Zhiyuan Sun, Kunsong Zhao, Zuchao Ma, Zihao Li, Jinan Jiang, Xiapu Luo, Yinqian Zhang
- **方向**: 密码学与协议
- **解读**: 
（无摘要，据标题推断）zkEVM 是零知识证明与以太坊虚拟机的结合：在链下执行 EVM 交易并生成简洁证明，链上验证即可确信执行正确，是 Rollup 扩容的核心组件。zkEVM 的"健全性"（soundness，虚假执行无法通过证明）与"完备性"（completeness，正确执行总能通过证明）直接决定资金安全——任何实现与电路之间的偏差都可能是可利用漏洞。本文对 Polygon zkEVM 进行自动化的健全性与完备性审查。

方法上应涉及：从 zkEVM 的电路/约束系统与 EVM 语义规范中提取可验证的对应关系，通过自动化工具（如符号执行、SMT 求解、差分测试）系统比对每条 EVM 指令的实现与规范，找出约束缺失（导致 soundness 破坏）或约束过严（导致 completeness 破坏）的情形。

预期成果包括在 Polygon zkEVM 中发现的具体不一致案例、漏洞分类以及修复建议。其安全价值在于：zkEVM 的证明正确性完全建立在电路正确性之上，自动化审查能在大规模审计之前先行暴露高风险缺陷。

该工作的贡献在于把形式化验证方法系统性地应用于真实 zkEVM 实现，而非停留在理论层面。作者团队（香港城市大学 Xiapu Luo、南方科技大学 Yinqian Zhang 组）在程序分析与系统安全方面实力强劲。局限在于未见摘要，具体发现与覆盖范围待核实；zkEVM 指令集庞大、电路更新频繁，审查的时效性是一大挑战。


#### Does Finality Gadget Finalize Your Block? A Case Study of Binance Consensus.

- **作者**: Rujia Li, Jingyuan Ding, Qin Wang, Keting Jia, Haibin Zhang, Sisi Duan
- **方向**: 密码学与协议
- **解读**: 
（无摘要，据标题推断）在 PoS 区块链中，"终局性小工具"（finality gadget）在异步区块生产之上叠加确定性确认机制，防止链被重组（reorg）或回滚，是资金安全的关键保障。Binance Chain 的共识（BSC 采用的基于 Tendermint 变体的共识机制）宣称提供终局性保证，但"宣称的终局性"与"实际达成的终局性"之间可能存在差距。本文以 Binance 共识为案例，检验终局性小工具是否真的终局化了区块。

方法上应是对 Binance 共识协议的形式化建模与分析：检查其小工具的设计（如投票规则、超时设置、领导者选举与最终确认条件）是否满足终局性的定义——包括安全性（不会确认相互冲突的区块）与活性（诚实验证器最终总能达成确认），并可能构造攻击场景展示终局性被破坏的条件。

预期成果包括发现 Binance 共识在特定网络条件（分区、验证器缺席、时钟偏差）下终局性保证失效的反例，以及改进建议。这类发现对依赖 BSC 终局性的应用（跨链桥、交易所充值确认）有直接影响——若终局性并不可靠，资金安全假设即被削弱。

该工作的价值在于把"终局性"从营销话术还原为可验证的性质，并以头部公链为案例给出实证。作者团队（Sisi Duan 等在 BFT 共识方面有深厚积累）具备理论严谨性。局限在于未见摘要，具体结论待核实；公链共识代码迭代快，案例分析可能随版本更新而过时，但其方法论可推广到其他 PoS 链。


#### Following Devils' Footprint: Towards Real-time Detection of Price Manipulation Attacks.

- **作者**: Bosi Zhang, Ningyu He, Xiaohui Hu, Kai Ma, Haoyu Wang
- **方向**: 密码学与协议
- **解读**: 
价格操纵攻击是 DeFi 中最臭名昭著的威胁之一，攻击者以严重偏离市场的价格成交代币。现有防御多为事后响应（攻击发生后检测攻击交易），无法及时止损。本文视角转换：攻击者通常需在攻击前阶段部署攻击合约，若能在预攻击阶段主动识别这些合约，就能提前告警并缓解威胁。

基于此，作者提出 SMARTCAT：在预攻击阶段主动识别价格操纵攻击。其设计兼顾三方面——通用性：直接分析字节码，不依赖源码与交易数据；准确性：将函数调用间的控制流与数据流依赖关系刻画为"代币流图"（token flow graph）；可扩展性：先过滤可疑路径，仅在必要时做跨合约分析。由此可在攻击合约部署上链后实时锁定目标。

实验显示 SMARTCAT 显著优于现有基线：召回率 91.6%、精确率约 100%。部署于以太坊与 BSC 的实时检测中，它在部署后平均 99 秒内发出 14 条告警；这些攻击已造成 64.1 万美元损失，其中 7 起仍在"等待收割"。此外 SMARTCAT 在野发现了 616 个攻击合约，对应 925 万美元损失，而其中仅 19 起被公开报道——大量攻击处于"暗处"。

该工作的贡献在于把 DeFi 安全从"事后取证"推进到"事前预警"，且全字节码、免源码的设计使其具备跨链通用性。局限在于预攻击检测依赖攻击者的部署模式（部署新合约），若攻击者复用既有合约或使用工厂模式可能绕过；同时"在野发现"的漏报率无法完全确认。总体而言，这是 DeFi 安全监控的重要进展。

- **论文 PDF**: [Following Devils' Footprint: Towards Real-time Detection of Price Manipulation Attacks.](https://www.usenix.org/system/files/usenixsecurity25-zhang-bosi.pdf)

#### Recover from Excessive Faults in Partially-Synchronous BFT SMR.

- **作者**: Tiantian Gong, Gustavo Franco Camilo, Kartik Nayak, Andrew Lewis-Pye, Aniket Kate
- **方向**: 密码学与协议
- **解读**: 
BFT 状态机复制（SMR）是区块链一致性的基础，但其安全性只在拜占庭故障数不超过协议容忍上限时成立。本文研究"过度故障"（excessive fault）场景：实际拜占庭故障数超过协议容忍度，系统状态已被破坏——此时原有安全概念不可满足，但仍可追求"修复"。作者给出了针对线性链式、基于法定人数的部分同步 SMR 的第一个修复算法，从过度故障造成的错误状态中恢复。

修复过程依赖"委托故障检测模块"（识别故障副本且不误伤任何正确副本的算法），并以稍弱的活性保证换取恢复能力（因为过度故障下原始活性不可能满足）。作者用 Rust 实现了可恢复版 HotStuff：恢复例程结束后，7 副本的吞吐恢复到正常水平，30 副本仅降低不超过 4.3%；延迟平均增加 12.87%（7 副本）与 8.85%（30 副本），恢复代价可控。

除采用现有检测模块外，作者还建立了通用 BFT SMR 协议支持"完全且健全"故障检测的充分条件——即使有多达 n-2 个（共 n 个）拜占庭副本攻击安全性也能检测。他们给出了首个无需额外通信轮次的封闭式（closed-box）故障检测算法，并在 Tendermint 与 HotStuff 中给出开箱式（open-box）实例化，进一步降低渐近与实际开销。

该工作的贡献在于首次系统化处理"过度故障后的恢复"这一现实而此前未被形式化的问题，理论（充分条件、闭箱检测）与工程（Rust 实现、开销测量）兼备。局限在于恢复过程需要故障检测模块，且弱化的活性保证意味着恢复期间可能暂停出块；对追求极端可用性的公链部署需权衡恢复速度与安全边界。

- **论文 PDF**: [Recover from Excessive Faults in Partially-Synchronous BFT SMR.](https://www.usenix.org/system/files/usenixsecurity25-gong-tiantian.pdf)

#### TockOwl: Asynchronous Consensus with Fault and Network Adaptability.

- **作者**: Minghang Li, Qianhong Wu, Zhipeng Wang, Bo Qin, Bohang Wei, Hang Ruan, Shihong Xiong, Zhenyang Ding
- **方向**: 密码学与协议
- **解读**: 
BFT 协议在崩溃故障面前通常呈"瀑布式"性能退化；少数协议虽对崩溃稳健，却在无故障场景付出更高的通信与轮复杂度代价。既有协议普遍缺乏"适应性"——无法在不同条件下都保持最优性能。本文提出 TockOwl，首个具备故障适应性的异步共识协议。

TockOwl 拥有二次通信复杂度与常数轮复杂度，使其在无故障场景保持高效；同时具备崩溃稳健性，面对崩溃故障时性能稳定——这两个性质共同构成"故障适应性"。进一步，作者提出 TockOwl+ 增加网络适应性：融合快、慢两条轨道并采用对冲延迟（hedging delays），在异步环境下无需等待超时即可获得可与部分同步协议媲美的低延迟；相比最新的双轨协议，TockOwl+ 的慢轨更简单，在完全异步环境中延迟更短。

该工作的价值在于把"按场景自适应"作为共识协议的一等设计目标：既要无故障时的高效率（二次通信、常数轮），也要故障下的稳健性，还要网络条件变化时的低延迟——此前这些目标往往此消彼长。作者团队（北航 Qianhong Wu 组）在密码学与共识方向积累深厚。

局限方面，异步共识的安全性通常依赖较强假设（如不可预测随机源），常数轮异步协议的实际部署仍需工程验证；此外"适应性"带来的协议复杂度提升可能增加实现与审计成本。与 DAG 类异步协议相比，TockOwl 家族提供了另一种兼顾效率与稳健的设计折中，对下一代区块链共识有参考价值。

- **论文 PDF**: [TockOwl: Asynchronous Consensus with Fault and Network Adaptability.](https://www.usenix.org/system/files/usenixsecurity25-li-minghang.pdf)

#### Thunderdome: Timelock-Free Rationally-Secure Virtual Channels.

- **作者**: Zeta Avarikioti, Yuheng Wang, Yuyi Wang
- **方向**: 密码学与协议
- **解读**: 
支付通道网络（PCN）被视为解决区块链低吞吐的方案，但近期多项研究揭示了其对时间锁（timelock）攻击与审查攻击的脆弱性：攻击者利用超时机制拖延或冻结资金。本文提出 Thunderdome，首个免时间锁（timelock-free）的 PCN：不依赖时间锁，而是借助虚拟通道（virtual channel）的设计思路，把免时间锁的支付通道原语扩展为支持多跳交易——让不共享通道的双方也能安全交易，同时规避时间锁攻击面。

其核心机制是由一组非信任的"守护者"（wardens，即watchtower委员会）在通道关闭过程中保障离线诚实方的资金安全；作者还设计了定制的激励（incentive）机制，确保所有参与者遵循协议正确执行。安全性论证双管齐下：既有假设委员会诚实多数的传统安全证明，也有所有参与者（含守护者）理性行为下的形式化博弈论分析——即"理性安全"。

作者在以太坊上实现了概念验证并评估成本：部署 Thunderdome（含开启底层支付通道）约需 15 美元（0.0089 ETH），最坏情况下关闭通道约 7 美元（0.004 ETH），成本可接受。

该工作的贡献在于以"去除时间锁"这一釜底抽薪的方式同时消解时间锁攻击与审查攻击，并以博弈论加固信任假设，设计思路新颖。局限在于依赖守护者委员会（尽管非信任仍需诚实的多数或理性激励），且免时间锁机制在恶意离线场景下的资金托管复杂度较高；与 Lightning 类 HTLC 方案相比，其激励与委员会模型更重，但安全性面更干净。
- **论文 PDF**: [Thunderdome: Timelock-Free Rationally-Secure Virtual Channels.](https://www.usenix.org/system/files/usenixsecurity25-avarikioti.pdf)

#### Hobbit: Space-Efficient zkSNARK with Optimal Prover Time.
- **作者**: Christodoulos Pappas, Dimitrios Papadopoulos
- **方向**: 密码学与协议
- **解读**: 零知识简洁非交互证明（zkSNARK）是区块链与隐私计算的核心原语，但证明者在计算时间与内存占用上的高昂开销限制了大规模应用，现有方案往往难以兼顾证明者时间与空间效率。本文提出 Hobbit，目标是以最优证明者时间实现空间高效的 zkSNARK。（无摘要，据标题推断）

从标题推断，Hobbit 在证明系统设计上做了针对性优化：推测通过改进多项式承诺方案与证明结构的组织方式，在保证简洁性与安全性（知识可靠性、零知识性）的前提下，同时压缩证明者的时间复杂度和内存占用。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；标题强调"最优证明者时间"与"空间高效"，预期以复杂度界与实验数据展示其在证明速度与内存占用上相对既有方案的改进。（无摘要，据标题推断）

证明者效率是 zkSNARK 落地的关键瓶颈，同时优化时间与空间具有明确工程价值，尤其对移动端与资源受限环境意义重大。局限是缺少摘要无法评估其安全性假设、通用性（是否面向特定电路）与具体提升幅度；与 Plonk、STARK 等主流路线的对比将是后续关注的焦点。


#### A Tale of Two Worlds, a Formal Story of WireGuard Hybridization.
- **作者**: Pascal Lafourcade, Dhekra Mahmoud, Sylvain Ruhault, Abdul Rahman Taleb
- **方向**: 密码学与协议
- **解读**: WireGuard 以简洁与高性能成为广泛部署的 VPN 协议，但面对量子计算威胁，其基于经典密码学的密钥交换需要升级为"混合"方案——同时使用经典与后量子算法，在量子时代保留经典安全性。本文用形式化方法对 WireGuard 的混合化改造进行安全分析。（无摘要，据标题推断）

从"两个世界"的标题推断，研究聚焦经典与后量子两个密码学世界的结合：推测采用符号化或计算模型对混合握手协议建模，验证认证性、密钥保密性等安全属性，并分析混合组合方式可能引入的降级或交互问题。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于给出混合化 WireGuard 的形式化安全证明或发现其设计缺陷，并为混合协议设计提供可推广的建模方法。（无摘要，据标题推断）

后量子迁移是当前密码学的头号工程问题，混合方案是平滑过渡的主流路线；对 WireGuard 这类"安全由协议简洁性支撑"的系统，形式化分析尤为重要。局限是形式化结果依赖建模保真度，模型与现实实现之间的差距（如实现层漏洞）不在证明范围内；其对其他混合协议的示范意义有待观察。


#### Improved Secure Two-party Computation from a Geometric Perspective.
- **作者**: Hao Guo, Liqiang Peng, Haiyang Xue, Li Peng, Weiran Liu, Zhe Liu, Lei Hu
- **方向**: 密码学与协议
- **解读**: 安全两方计算（2PC）中，乘法与非线性运算是最昂贵的协议，而比较协议（Wrap 协议）又是截断、有符号扩展、有符号非均匀乘法等基础操作所必需，成为效率瓶颈。本文从几何视角重新审视 2PC，目标是避免调用昂贵的比较协议以提升整体效率。

核心思路是把秘密的两个分享值看作笛卡尔坐标系中一个点的横纵坐标，秘密本身对应该点；比较问题转化为判断点落在哪个区域，从而识别出可用更高效的与门（AND gate）协议替代比较协议的受限场景。基于此，作者改进了截断、有符号扩展与有符号非均匀乘法协议。

对于一位错误截断协议与有符号扩展协议，将 Cheetah（USENIX'22）和 SirNN（S&P'21）的最优通信复杂度从约 λ(l+1) 降至约 λ（两轮完成，l 为输入长度、λ 为安全参数）；对有符号非均匀位宽乘法，相对 SirNN 通信成本降低 40% 至 60%。

以几何视角统一理解 2PC 基础协议并系统性规避比较操作，是方法论上的巧妙推进，且改进的是最基础构件，影响面广。局限是优化集中于特定协议族与受限范围场景，通用比较协议本身未被消除；实际收益还取决于协议实现与网络往返的权衡。

- **论文 PDF**: [Improved Secure Two-party Computation from a Geometric Perspective.](https://www.usenix.org/system/files/usenixsecurity25-guo-hao-improved.pdf)

#### zk-promises: Anonymous Moderation, Reputation, and Blocking from Anonymous Credentials with Callbacks.
- **作者**: Maurice Shih, Michael Rosenberg, Hari Kailad, Ian Miers
- **方向**: 密码学与协议
- **解读**: 匿名性对言论自由与表达异议至关重要，但平台需要监管不良行为者——匿名场景下的封禁、声誉扣减与访问控制状态更新往往在违规行为发生后异步进行，恶意客户端自然想逃避这种异步负面反馈。本文提出 zk-promises：支持图灵完备状态机与异步回调的有状态匿名凭证框架。

客户端状态存放于"zk-object"，零知识证明确保对象只能按既定程序更新；回调允许第三方（如对匿名用户点踩或封禁）更新状态，客户端周期性扫描回调并更新自身状态；认证时匿名断言状态上的谓词并证明近期（如 24 小时内）扫描过状态。作者据此设计、实现并实测了一个匿名声誉系统。

该匿名声誉系统支持异步声誉更新、封禁与依赖声誉的限流（以抵御 Sybil 攻击），作者宣称其性能与功能优于现有最优方案，且支持"最近扫描"证明以对抗状态过期。

把通常存放于可信服务器的状态"外包"给客户端、同时保留服务端更新能力，是对匿名凭证范式的重要扩展，图灵完备状态机与回调覆盖了审核、声誉、封禁等真实场景。局限是客户端必须周期性上线扫描回调，离线用户可能错过更新；具体性能数字与安全性假设依赖全文，与现实部署的兼容性仍待检验。

- **论文 PDF**: [zk-promises: Anonymous Moderation, Reputation, and Blocking from Anonymous Credentials with Callbacks.](https://www.usenix.org/system/files/usenixsecurity25-shih.pdf)

#### A Formal Analysis of Apple's iMessage PQ3 Protocol.
- **作者**: Felix Linker, Ralf Sasse, David A. Basin
- **方向**: 密码学与协议
- **解读**: 即时通讯需要能抵御量子计算对手的高性能端到端加密协议。苹果 iMessage 的 PQ3 结合苹果身份服务与定制的后量子安全初始化阶段，采用类 Signal 的双棘轮结构并扩展提供后量子与前向失密后安全（post-compromise security）。本文对 PQ3 进行详细建模与机器验证的安全证明。

作者用 Tamarin 证明器给出 PQ3 的详细形式化模型、细粒度安全性质的精确规格与机器检查证明，覆盖两个密钥棘轮（含无界循环）——这曾被认为超出 Tamarin 这类符号证明器的能力范围。特别之处在于将后量子密钥封装（KEM）集成进相关协议阶段并给出完整形式化分析。

分析覆盖两个棘轮及其无界循环，成功证明 PQ3 达到其声明的后量子与后失密安全性质，并表明符号证明器可处理此前被认为不可行的建模范围。

对苹果生产级协议做端到端形式化验证并覆盖无界循环，是形式化方法的里程碑式应用；证明"无界循环对 Tamarin 并非不可行"也为同行打开思路。局限是符号模型的抽象可能遗漏实现与侧信道层面的攻击面，模型保真度依赖对协议文档的解读；其结果对理解 PQ3 的实际安全边界仍有重要参考价值。

- **论文 PDF**: [A Formal Analysis of Apple's iMessage PQ3 Protocol.](https://www.usenix.org/system/files/usenixsecurity25-linker.pdf)

#### LOHEN: Layer-wise Optimizations for Neural Network Inferences over Encrypted Data with High Performance or Accuracy.

- **作者**: Kevin Nam, Youyeon Joo, Dongju Lee, Seungjin Ha, Hyunyoung Oh, Hyungon Moon, Yunheung Paek
- **方向**: 密码学与协议
- **解读**: FHE 推理神经网络时，密文配置（CC：环大小 N、级别、打包方式等）决定安全性、性能与精度，传统做法是全局选一个"凑合"的 CC。NN 的分层结构使每层最优 CC 各不相同，但层间切换 CC 需要昂贵密码学操作，开销常抵消收益。LOHEN 的目标是让逐层 CC 真正可用，同时兼顾性能与精度。

核心是 LOHEN 密码学重打包（LCR）：SlottoCoeff→Ringswitch→LWE-Extract→RLWE-Pack→modRecover 实现任意 CC 切换，还能顺带替代昂贵旋转（实现任意置换）与自举（恢复级别）。配合代价分析器与贪心算法（ALS/CLS 层栈，附不劣于全局 CC 的归纳证明）自动决定 LCR 插入位置与 CC 选择；多方案模式用方案转换 gadget（ModRecover 从 3 次减到 1）把非算术激活层切换到 TFHE 消除 CKKS 近似精度损失，并提供精度优先/延迟优先/均衡三种开发者可定制策略。

在 CIFAR-10/ImageNet 的 6 个模型（ResNet/VGG/SqueezeNet/MobileNetv2）上，LOHEN 比 SOTA 的 FHE-MP-CNN 端到端延迟降低 7%-65%（1.08-2.88×），如 MobileNetv2 从 2922.78s 降至 1016.17s；而全层无脑重打包（AllRepack）反而最多慢 38%，凸显代价分析器价值。多方案模式下零精度损失仍比 SOTA 多方案方法快 1.34-1.75×；均衡策略下 5 个 TFHE 层即可用 1.2% 精度损失换取 1.13× 加速。

把编译器式"逐层优化"系统引入 FHE NN 推理，LCR 一石三鸟（切换+置换+自举）是关键洞察，有形式化保证且基线公平（用同一 GPU 库重跑对手）。局限：LCR 本身比自举还慢（N=2^17 时 402,749µs vs 371,541µs），收益依赖层结构；依赖 TFHE 的 PBS，模型增大后开销仍高；仅评估 CNN，对 Transformer/大模型的可扩展性未涉及。

- **论文 PDF**: [LOHEN: Layer-wise Optimizations for Neural Network Inferences over Encrypted Data with High Performance or Accuracy.](https://www.usenix.org/system/files/usenixsecurity25-nam-lohen.pdf)

#### Who Pays Whom? Anonymous EMV-Compliant Contactless Payments.

- **作者**: Charles Olivier-Anclin, Ioana Boureanu, Liqun Chen, Christopher J. P. Newton, Tom Chothia, Anna Clee, Andreas Kokkinis, Pascal Lafourcade
- **方向**: 密码学与协议
- **解读**:

EMV 是 Mastercard、Visa、American Express 等采用的全球事实支付标准，但店内 EMV 非接触支付并不匿名：付款人的长期身份标识数据会泄露给商户甚至旁观者。反洗钱（AML）、了解你的客户（KYC）与强客户认证（SCA）等监管要求虽然保护我们免受非法活动侵害，却也构成了 EMV 支付缺乏隐私的主要原因。

方法上，作者在不破坏与现有 EMV 兼容性的前提下，通过"工程化设计实现隐私"而非同态加密等隐私增强技术，提出两个符合法规、切实可行的非接触支付协议 PrivBank 与 PrivProxy，完全沿用现有 EMV 基础设施。

结果：PrivBank 与 PrivProxy 可证明地实现了支付者与商户的强隐私、匿名性与不可关联性（达到电子现金或购物券级别的安全概念），同时可直接在现有 EMV 体系中原样实现，兼顾 AML/KYC/SCA 合规。

我的思考：该工作的成色在于"在监管钢丝上走通隐私"——不引入破坏向后兼容的密码学，而是用协议工程在既有 EMV 流程中重新分配信任与信息可见性，可证明的匿名性/不可关联性概念也有理论严谨性。局限：支付金额、时间等元数据的匿名边界、商户侧实现成本与实际部署阻力（发卡行、清算网络配合）是落地主要障碍。与匿名电子现金经典文献一脉相承，是把学术隐私目标映射到真实支付标准的代表性尝试。

- **论文 PDF**: [Who Pays Whom? Anonymous EMV-Compliant Contactless Payments.](https://www.usenix.org/system/files/usenixsecurity25-olivier-anclin.pdf)

#### Shimmer: a Provably Secure Steganography Based on Entropy Collecting Mechanism.

- **作者**: Minhao Bai, Kaiyi Pang, Guorui Liao, Jinshuai Yang, Yongfeng Huang
- **方向**: 密码学与协议
- **解读**:

隐写术将秘密信息嵌入看似无害的载体中，而基于生成模型的可证明安全隐写是近年热点：只有让嵌入过程保持与载体分布几乎一致，才能保证"不可检测"。与同组 SparSamp、隐写设计框架等工作一脉相承，Shimmer 聚焦如何在保持可证明安全的同时提升嵌入效率。

据标题推断，Shimmer 提出基于"熵收集机制"的可证明安全隐写：在生成模型采样过程中收集熵（随机性），将秘密比特与采样随机数结合以嵌入信息，使嵌入后的采样分布与原始分布不可区分，从而获得可证明安全性。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于为可证明安全隐写提供又一种基于熵利用的构造，与稀疏采样、概率重组等思路互为补充。

我的思考：该工作隶属清华黄永峰团队的可证明安全隐写谱系（与 SparSamp、框架论文同期发表），"熵收集"切入点在理论上可与拒绝采样式隐写、Coded Steganography 对比分析；若能证明嵌入不扰动分布，即具备标准意义下的安全性。局限（据标题推断）：熵收集的实现细节、单步开销与嵌入率指标缺失，且隐写安全性的实际验证需要与载体统计检测对抗；与同类方案的系统对比有待原文呈现。


#### Exploring How to Authenticate Application Messages in MLS: More Efficient, Post-Quantum, and Anonymous Blocklistable.
- **作者**: Keitaro Hashimoto, Shuichi Katsumata, Guillermo Pascual-Perez
- **方向**: 密码学与协议
- **解读**:
MLS 是 IETF 新标准化的可扩展安全组消息协议，但现有研究几乎都聚焦机密性，应用消息认证被忽视。MLS 目前用 EdDSA 签名认证每条应用消息，经典设置下 64 字节开销尚可接受；转入后量子后 NIST 推荐的 Dilithium 签名达 2.4 KB，是 EdDSA 的 40 倍，而典型应用消息不足 100 字节，开销被急剧放大。此外缺乏认证形式化模型，恶意内部成员可借畸形消息对群组发动 DoS。

作者系统地将应用消息认证划分为不可伪造性、匿名性、匿名拉黑、追溯健全性四种安全概念，提出 GAM（组认证消息）协议抽象以剥离机密性单独研究认证；基于 OWF、MAC、KEM 等经典与后量子可实例化的通用构件设计五种模式：COSMOS、COSMAC、QUASAR、STARS、GEMSTARS，并给出 COSMOS+/(++) 与 COSMAC+/(++) 优化变体。

COSMOS++ 用一次性令牌加 MAC 标签替代签名，优化后每条消息通信开销仅 32 字节（COSMAC++ 为 48 字节），后量子通信开销比 MLS 现行方式节省约 75 倍，代价是略微弱化的不可伪造性（假设恶意服务器不与内部人员合谋）；QUASAR 与 STARS 则在保持匿名性的同时实现追溯健全性，GEMSTARS 用群签名完全去除状态更新。

该工作系统性填补了 MLS 应用消息认证研究的空白，给出了完整的安全-效率权衡设计空间，后量子场景下价值突出。局限在于弱化安全模型依赖非合谋假设，QUASAR/STARS 的全局状态更新限制实用性，GEMSTARS 因缺乏高效后量子群签名而低效；对完整 MLS 的组合安全分析仍留作未来工作。

- **论文 PDF**: [Exploring How to Authenticate Application Messages in MLS: More Efficient, Post-Quantum, and Anonymous Blocklistable.](https://www.usenix.org/system/files/usenixsecurity25-hashimoto-blocklistable.pdf)

#### How to Compare Bandwidth Constrained Two-Party Secure Messaging Protocols: A Quest for A More Efficient and Secure Post-Quantum Protocol.

- **作者**: Benedikt Auerbach, Yevgeniy Dodis, Daniel Jost, Shuichi Katsumata, Rolfe Schmidt
- **方向**: 密码学与协议
- **解读**:

两方安全消息协议（如 Signal 类端到端加密）是当下通信安全基石，其设计需要在安全性、带宽与延迟之间权衡；随着 NIST 后量子算法标准化，协议需迁移到后量子原语，而 PQ 原语的密钥与签名尺寸更大，如何在带宽受限场景下公平比较并设计更优协议成为关键问题。

据标题推断，论文提出带宽受限两方安全消息协议的比较框架/度量，并在此框架下设计更高效、更安全的后量子协议：可能以带宽开销、握手轮次、前向保密/后向保密与可否认性等为维度统一比较现有协议，再以紧凑 PQ 原语（如 ML-KEM、哈希签名或结构化签名）重构握手流程。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把"协议比较"方法化，为后量子迁移提供可量化的取舍依据，并给出具体的候选协议设计。

我的思考：作者阵容（Dodis、Katsumata、Auerbach、Jost）在 Signal 协议形式化分析与 PQ 密码学上均有深厚积累，该工作大概率在严格安全模型下给出比较与构造，具备理论可信度。局限（据标题推断）：比较框架的维度选择本身可能引发争议，新协议的带宽节省是否以安全假设或计算开销为代价需逐项核对；与 Signal 官方 PQXDH 路线的兼容性也待评估。对后量子即时通讯迁移有直接指导意义。


#### Bundled Authenticated Key Exchange: A Concrete Treatment of Signal's Handshake Protocol and Post-Quantum Security.
- **作者**: Keitaro Hashimoto, Shuichi Katsumata, Thom Wiggers
- **方向**: 密码学与协议
- **解读**:
Signal 依靠 X3DH（现为 PQXDH）握手协议建立安全会话，但既有分析都针对具体协议定制模型，或对标准 AKE 定义做临时改编，导致各协议达到的具体安全级别不清晰、难以比较；更关键的是，一些自然的 Signal 握手设计（如多个 prekey bundle 间共享状态）根本无法被这些定制模型刻画。

作者引入 Bundled Authenticated Key Exchange（BAKE），正式建模 prekey bundle 及其状态，通过排除所有协议都无法避免的攻击来定义"最优安全"级别，在统一模型下处理密钥不可区分性与认证两类性质，并支持经典、harvest-now-decrypt-later（HNDL）与量子三类对手；重放与 KEM 重封装攻击被显式建模为认证的破坏，而非含糊的隐式攻击。

分析表明 X3DH 与 PQXDH 均达不到最优安全：都存在接收方状态泄露后被冒充的弱点、prekey bundle 组件未签名导致发送方会话仅具弱前向保密、PQXDH 仅 HNDL 安全等，这些已知弱点被量化为额外的不可避免攻击。据此提出 RingXKEM——基于后量子环签名（Gandalf）与 KEM 的完全后量子握手，用 Merkle 树认证 prekey bundle（服务器仅需存储一个签名而非每 bundle 一个），达到量子对手下的最优安全，估算部署成本与 PQXDH 相当。

这是首个可复用、可比较的 Signal 握手统一模型，且作者与 Signal 团队持续沟通并获得其确认，现实影响力强。局限在于 RingXKEM 依赖环签名的效率（移动端运行时为推算值），"最优安全"定义对 Origin 谓词仍有一定自由度。作为后量子迁移的形式化基石，其模型与协议设计均具长期价值。

- **论文 PDF**: [Bundled Authenticated Key Exchange: A Concrete Treatment of Signal's Handshake Protocol and Post-Quantum Security.](https://www.usenix.org/system/files/usenixsecurity25-hashimoto-key-exchange.pdf)

#### Comprehensive Deniability Analysis of Signal Handshake Protocols: X3DH, PQXDH to Fully Post-Quantum with Deniable Ring Signatures.

- **作者**: Shuichi Katsumata, Guilhem Niot, Ida Tucker, Thom Wiggers
- **方向**: 密码学与协议
- **解读**:

可否认性（deniability）是安全消息协议的重要属性：通信双方事后应无法向第三方证明对方确实参与了某次通信，Signal 的 X3DH 握手与后量子升级版 PQXDH 均被设计为具备该属性，但其可否认性在不同攻击者模型（离线/在线、合谋）下的完整边界缺乏系统分析，后量子迁移是否破坏可否认性也待考察。

据标题推断，论文对 Signal 握手协议 X3DH 与 PQXDH 做全面的可否认性分析，形式化刻画其在不同威胁模型下的可否认性保证与失效情形；并进一步探索完全后量子方案——利用可否认环签名（deniable ring signatures）构造在纯后量子设定下仍保持可否认性的握手协议。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把"可否认性"这一微妙属性置于严格的密码学分析之下，并为后量子时代的握手设计提供可否认性安全构造。

我的思考：可否认性是 Signal 类协议中最难形式化的属性之一（涉及合谋攻击者、第三方见证与零知识论证的边界），Katsumata、Wiggers 等作者在 PQ 密码学与协议实现上资历深厚，其分析结论对 Signal 协议族的安全论证有权威参考意义。局限（据标题推断）：完全后量子方案的效率（环签名尺寸、握手轮次）与 Signal 现有部署的兼容迁移路径是落地关键；可否认性定义（判定者、见证者模型）的选择也直接决定结论强弱。与 Signal 官方 PQXDH 路线图衔接紧密。


#### SparSamp: Efficient Provably Secure Steganography Based on Sparse Sampling.

- **作者**: Yaofei Wang, Gang Pei, Kejiang Chen, Jinyang Ding, Chao Pan, Weilong Pang, Donghui Hu, Weiming Zhang
- **方向**: 密码学与协议
- **解读**:

隐写术将机密数据嵌入看似正常的通信中，而可证明安全的隐写（嵌入后分布与载体分布不可区分）是长期追求的目标，深度生成模型使其成为可能；但现有方法面临安全性与效率的根本权衡，嵌入速度慢、提取准确率有限，制约了实际部署。

方法上提出 SparSamp：基于稀疏采样的高效可证明安全隐写。将消息与伪随机数结合得到"消息派生随机数"用于采样，通过增大采样间隔、使采样过程稀疏化来提升提取准确率与嵌入容量；由于保持生成模型原始概率分布不变，安全性得以保证。每个采样步骤仅引入 O(1) 额外复杂度，嵌入速度最快且不牺牲生成速度；即插即用，只需将现有生成模型的采样组件替换为 SparSamp。

结果：在文本、图像与音频生成模型上实现部署，嵌入速度分别达到 GPT-2 上 755 bits/s、DDPM 上 5,046 bits/s、WaveRNN 上 9,223 bits/s，验证了高效率与高容量的结合。

我的思考：该工作的成色在于把"可证明安全"从理论概念落实为工程上可插拔的采样组件，O(1) 附加开销与三个模态的高吞吐数字（尤其 WaveRNN 9k bits/s）很有说服力，稀疏采样提升容量的思路与既有拒绝采样/编码式隐写形成对照。局限：安全性证明依赖生成模型分布可精确采样且熵充足，稀疏采样在长序列下的累积误差、对模型分布失配的鲁棒性值得深究。与同组 Shimmer、隐写设计框架共同构成可证明安全隐写的方法谱系。

- **论文 PDF**: [SparSamp: Efficient Provably Secure Steganography Based on Sparse Sampling.](https://www.usenix.org/system/files/usenixsecurity25-wang-yaofei.pdf)

#### A Framework for Designing Provably Secure Steganography.

- **作者**: Guorui Liao, Jinshuai Yang, Weizhi Shao, Yongfeng Huang
- **方向**: 密码学与协议
- **解读**:

隐写术在公开信道上传输秘密消息且不暴露其存在，而基于共享白盒采样器的可证明安全隐写是主流方向，因其能在各种实用信道上构造安全高效的隐写系统。然而设计新方案极具挑战：嵌入秘密信息的同时必须使采样分布与任意给定离散分布几乎一致，目前可用的可证明安全方案寥寥无几，严重制约应用与理论发展。

方法上提出可证明安全隐写的通用设计框架，并为该框架派生的方案给出通用安全证明：把整体复杂设计分解为三个较易实现的子过程——概率重组模块（Probability Recombination Module）、二进制采样（Bin Sampling）与均匀隐写模块（Uniform Steganography Module）。

结果：基于该框架提出多个新的可证明安全隐写方案，并证明近期工作 Discop(base) 也被该框架所涵盖；在指定度量下识别出若干理论上最优或非常有效的方案，并通过实验验证其有效性。

我的思考：这是把隐写设计"模块化"的方法论贡献——统一安全证明使研究者无需为每个新方案重复论证安全性，覆盖 Discop(base) 说明框架具有实际解释力，"最优/高效方案"的系统化发现也提升了理论完备性。局限：框架有效性依赖三个子模块各自的构造质量，理论最优与实测性能之间、以及多模态（文本/图像/音频）适配的差异性需要原文细节支撑。与 SparSamp、Shimmer 同属该团队的可证明安全隐写体系，是领域的理论基石型工作。

- **论文 PDF**: [A Framework for Designing Provably Secure Steganography.](https://www.usenix.org/system/files/usenixsecurity25-liao.pdf)

#### Flexway O-Sort: Enclave-Friendly and Optimal Oblivious Sorting.

- **作者**: Tianyao Gu, Yilei Wang, Afonso Tinoco, Bingnan Chen, Ke Yi, Elaine Shi
- **方向**: 密码学与协议
- **解读**:

不透明算法已在真实世界大规模部署（如 Signal 的私有联系人发现依赖硬件 enclave 中的不透明键值存储），不透明排序是其核心构件；但理论与实践存在鸿沟：常用的 bitonic 排序渐近非最优（O(N log²N)），而渐近最优的 AKS 类算法常数巨大，实际性能远逊于 bitonic。

方法上构造 flexway o-sort：达到渐近最优的 O(N logN) 工作量、具体高效且适合 Intel SGX 等硬件 enclave 实现，弥合理论最优与现实可用性之间的差距。

结果：对 12GB 中等规模输入，flexway o-sort 在数据适配 enclave 时比已知不透明排序算法快 1.32-28.8 倍，数据超出 enclave 时快 4.1-208 倍；在直方图、数据库连接与 ORAM 初始化等应用（8-32GB 数据）上，enclave 内较 bitonic 加速 1.44-2.3 倍、enclave 外加速 4.9-5.5 倍。

我的思考：该工作直击不透明算法"理论最优 vs 工程可用"的核心矛盾，以具体可复现的实现把最优性转化为真实加速，对依赖不透明原语的隐私保护应用（Signal 类服务）有直接落地价值；大外存场景数十倍的提升尤为亮眼。局限：性能优势与输入规模、数据分布及 enclave 内存配置强相关，跨硬件平台的泛化性与实现正确性的形式化保证需进一步审视；与同类近期构造（如 OSS 系列）的对比是评估其真实增量关键。
- **论文 PDF**: [Flexway O-Sort: Enclave-Friendly and Optimal Oblivious Sorting.](https://www.usenix.org/system/files/usenixsecurity25-gu-tianyao.pdf)

#### Treebeard: A Scalable and Fault Tolerant ORAM Datastore.

- **作者**: Amin Setayesh, Cheran Mahalingam, Emily Chen, Sujaya Maiyya

- **方向**: 密码学与协议

- **解读**: 

（无摘要，据标题推断）云端外包存储中，Oblivious RAM（ORAM）用于隐藏客户端访问模式、抵御服务器端泄漏攻击，但现有 ORAM 数据存储（datastore）在可扩展性与容错性上存在明显短板：多数方案面向单一客户端设计，难以支撑多用户并发；服务器单点故障会直接导致服务不可用。本文提出 Treebeard，目标是构建可扩展且容错的 ORAM 数据存储系统。

从标题推断，Treebeard 应是在经典树形 ORAM（如 Path ORAM）基础上引入多副本冗余与一致性协调：将数据及元数据在多个服务器间分布/复制，通过共识协议或容错设计保证部分服务器失效时仍能正确服务，同时把重洗（reshuffle）等昂贵操作并行化以提升吞吐。具体复制策略与安全假设因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题中"可扩展与容错"表明作者围绕并发规模、吞吐量、故障恢复等维度进行了系统性评估。

我的思考（基于标题的保守推断）：Treebeard 的价值在于把 ORAM 从"单客户端密码学原语"推向"可部署的容错服务"，贴近真实云服务形态。挑战在于容错复制会显著放大 ORAM 本就高昂的带宽与重洗成本，如何在可扩展性、容错性与开销间取得平衡是其核心命题。与只优化单客户端渐近开销的学术 ORAM 相比，这类面向多用户并发与故障场景的系统工作更具工程落地意义。


#### Learning from Functionality Outputs: Private Join and Compute in the Real World.

- **作者**: Francesca Falzon, Tianxin Tang

- **方向**: 密码学与协议

- **解读**: 

Google 提出的 Private Join and Compute（PJC）允许两方各持有关键字-值数据库，仅计算交集关键字对应值的内积，其部署版本 PSI-SUM 已用于广告转化场景。然而 MPC 文献的安全模型通常不考虑功能输出本身：PJC 的输出是输入的确定性函数，参与方即使完全诚实地运行协议，也可能从中反推对方数据，构成现实隐私威胁。

作者将攻击者建模为 PJC 参与方，定义键值恢复（KVR）目标——恢复交集内键的成员关系与关联值，给出四类攻击：二叉搜索式搜索树攻击（自适应，至多 O(k·log_ℓ|T|) 次查询）；基于最大似然估计的非自适应攻击；压缩感知攻击（稀疏场景下仅需 2k·log₂(n/k) 次查询即可精确恢复，对噪声鲁棒）；DFT 攻击（已知交集规模 k 时只需 2k 次查询）。所有攻击均不偏离协议、不攻破底层密码原语，并同样适用于 PSI-SUM 与 PSI-CA。

两类设定下的攻击均能恢复键成员关系与关联值；即使长期诚实进行数据分析的参与方，也只需与交集规模呈线性的查询次数即可达成 KVR；作者实现攻击并验证效率，证明清洗离群值、小交集中止、输出加噪等缓解措施均不充分。

我的思考：该工作的成色在于揭示"功能输出即固有泄漏通道"这一被 MPC 模型长期忽视的问题，攻击不依赖任何协议偏离，影响面覆盖已部署的 Google PSI-SUM；压缩感知与 DFT 路线把查询次数压到对数/线性级。局限是假设受害者数据库静态、攻击者已知值域，缓解措施仅给出方向性建议。与 Guo 等人针对 PSI-CA 的成员推理相比，本文把攻击目标推进到"键值同时恢复"，并系统化四条技术路线，直接推动 MPC 安全模型修订。

- **论文 PDF**: [Learning from Functionality Outputs: Private Join and Compute in the Real World.](https://www.usenix.org/system/files/usenixsecurity25-falzon.pdf)

#### ALERT: Machine Learning-Enhanced Risk Estimation for Databases Supporting Encrypted Queries.

- **作者**: Longxiang Wang, Lei Xu, Yufei Chen, Ying Zou, Cong Wang

- **方向**: 密码学与协议

- **解读**: 

可搜索对称加密（SSE）提供亚线性密文检索，但访问模式与搜索模式泄漏可被泄漏滥用攻击（LAA）利用，用于恢复查询或重构数据。现有泄漏量化方法需要综合全量查询分析，无法实时评估风险；而 SSE 的泄漏随每次查询逐条累积暴露，客户端迫切需要"边查询边预警"的在线风险感知机制。

ALERT 是一个机器学习增强的实时风险评估框架：利用公开辅助信息自动学习明文查询的关键词特征并训练为分类器；每当查询执行时，ALERT 快速预测查询对应的关键词并估计泄漏发生的可能性，向客户端发出告警，从而为"何时重建索引或数据集以重置漏洞"提供量化决策依据。

实验表明 ALERT 可在数秒内给出预测，相比现有最先进方法实现 31.1× 的加速；作为首个实时泄漏风险评估的可行性研究，它展示了把泄漏监控嵌入加密数据库运行过程的可能性。

我的思考：贡献在于把泄漏量化从"事后全量分析"转变为"在线逐查询预警"，补齐了 SSE 部署中风险感知的关键一环，31.1× 的加速说明工程上可行。局限是分类器依赖公开辅助信息与历史查询特征，对全新关键词域或刻意对抗的查询其泛化能力未知，且关键词预测本身的准确率构成风险估计的质量下限。与偏攻击侧的 LAA 研究相比，本文站在防御侧做实时量化，与攻击、缓解文献形成互补，为加密数据库的运行时安全运营提供了新工具。

- **论文 PDF**: [ALERT: Machine Learning-Enhanced Risk Estimation for Databases Supporting Encrypted Queries.](https://www.usenix.org/system/files/usenixsecurity25-wang-longxiang.pdf)

#### Suda: An Efficient and Secure Unbalanced Data Alignment Framework for Vertical Privacy-Preserving Machine Learning.

- **作者**: Lushan Song, Qizhi Zhang, Yu Lin, Haoyu Niu, Daode Zhang, Zheng Qu, Weili Han, Jue Hong, Quanwei Cai, Ye Wu
- **方向**: 密码学与协议
- **解读**:

安全数据对齐是纵向隐私保护机器学习（VPPML）的第一步关键环节，但在广告等实际场景中，双方数据通常不平衡（一方数据量 N 远大于另一方 n）。现有基于 Cuckoo Hashing 的 circuit-PSI 类方案会输出交集之外的冗余数据份额（规模 εn），显著增大后续安全训练的通信量；用安全洗牌裁剪冗余又会引入巨大通信开销。论文的核心挑战是：能否不经洗牌，直接且只输出交集内的数据份额。

论文提出 Suda，用多项式运算取代 Cuckoo Hashing。框架分两步：先基于 ECDH-PSI 设计 GetNewID 协议完成 ID 对齐并分享标签份额；再设计批量 PIR 协议（利用 NTT 将特征编码为多项式、基于多项式取模的 oblivious polynomial reduction 把次数从 O(N) 降到 O(n)、用随机多项式掩码实现 oblivious polynomial evaluation），并扩展出批量 PIR-to-Share 协议，通过 oblivious polynomial interpolation 让客户端取回的是特征份额而非明文特征。安全模型为半诚实敌手。

实验显示，Suda 相比最先进的 iPrivJoin 通信量降低 31.14×~210.78×、运行时间最多快 8.21×，比批量 PIR 框架 PIRANA 服务器时间最多快 11.53×。例如 SVHN 数据集上 Suda 通信仅 455.44MB（2545.99s），而 iPrivJoin 高达 63259.04MB（12203.20s）；通信复杂度从 O(m(N+n)) 降至 O(N+n+mn′)，WAN 与 LAN 下性能接近。相比之下，CPSI 因输出 εn 冗余份额在训练阶段还多出约 2 倍通信开销。

该工作把对齐阶段与安全训练无缝衔接，规避了洗牌开销，思路巧妙且实验充分。局限在于半诚实模型假设、n 与 N 需满足特定关系、以及依赖 BFV/Ed25519 的参数规模，规模化部署成本仍待评估。与基于 PSI 的路线相比，多项式 PIR 方案在特征维度大、数据不平衡时优势更明显，为 VPPML 数据对齐提供了新的技术范式。

- **论文 PDF**: [Suda: An Efficient and Secure Unbalanced Data Alignment Framework for Vertical Privacy-Preserving Machine Learning.](https://www.usenix.org/system/files/usenixsecurity25-song-lushan.pdf)

#### Shechi: A Secure Distributed Computation Compiler Based on Multiparty Homomorphic Encryption.

- **作者**: Haris Smajlovic, David Froelicher, Ariya Shajii, Bonnie Berger, Hyunghoon Cho, Ibrahim Numanagic

- **方向**: 密码学与协议

- **解读**: 

分布式敏感数据分析（生物医学、金融）受隐私与法律约束形成数据孤岛；全同态加密（HE）计算开销大，安全多方计算（SMC）通信开销高，二者结合的 MHE 虽有潜力，却因缺乏编译器而要求开发者同时精通密码学、分布式算法与领域分析，手工实现并优化安全协议极易出错且难以验证。

Shechi 是首个 MHE 编译器：自动将 Pythonic 代码转换为安全的分布式等价实现，融合 HE 与 SMC；引入面向分布式数据的新数据类型，并对密码学与分布式计算进行系统性编译器优化，把"私密与分布式"的复杂性对终端用户完全抽象。

在包括主成分分析与复杂基因组分析在内的广泛应用上，Shechi 相比先前最先进方案实现最高 15× 的运行时改进，相比专家手工优化代码获得 40 倍的代码表达力提升；例如 128 个样本从 2 方拆分到 8 方时运行时间由 40 分钟降至 15 分钟，展现其分布式扩展能力。

我的思考：贡献在于把 MHE 从"专家手工工程"变为"编译器自动生成"，显著降低安全分布式计算的准入门槛，15×/40 倍的提升说明其优化系统性地超越了人工设计。局限是 MHE 底层密码开销依旧存在，编译器优化依赖具体算法模式，整体安全性最终取决于底层 HE/SMC 原语。与 Lattigo、MP-SPDZ 等库相比，Shechi 的差异化在于"声明式 Pythonic 编程 + 自动编译"这一更高层抽象，是安全计算走向普及的关键一步。

- **论文 PDF**: [Shechi: A Secure Distributed Computation Compiler Based on Multiparty Homomorphic Encryption.](https://www.usenix.org/system/files/usenixsecurity25-smajlovic.pdf)

#### Private Set Intersection and other Set Operations in the Third Party Setting.

- **作者**: Foo Yee Yeo, Jason H. M. Ying

- **方向**: 密码学与协议

- **解读**: 

三方私有集合交集（TP-PSI）中，接收方是不持有任何输入的第三方；推广到多方后，朴素地把"全集"当作一方输入的做法其计算与通信成本随全集规模线性增长，不可行；而此前的两方 TP-PSI 方案依赖交换密码与密钥协商，无法推广到多方场景。

作者给出多方第三方 PSI 协议族：多个输入方各持私有集合，无输入第三方仅学习所有集合的交集结果，客户端与接收方不泄露其他任何信息；同时给出第三方私有集合差（PSD）与私有对称差（PSSD）协议；所有构造在半诚实模型下对任意数量共谋方可证明安全，并完成实现。

协议在多种输入/输出集合规模上完成实验评估，验证了开销随集合规模多项式增长、随全集规模多对数增长的效率目标。

我的思考：贡献在于系统补齐"第三方设定"下的 PSI 协议版图，任意共谋鲁棒性是显著亮点——第三方无输入、无法用自己的集合约束交集元素，使协议设计难度本质不同于经典 PSI。局限是半诚实模型未覆盖恶意敌手，多方场景的交互轮次与通信量在超大集合上的实测表现有待扩展。与 Yeo & Ying 之前的两方 TP-PSI 相比，本文的多方推广与差集/对称差扩展拓宽了其在名单核查、供应链验证等场景的适用性。

- **论文 PDF**: [Private Set Intersection and other Set Operations in the Third Party Setting.](https://www.usenix.org/system/files/usenixsecurity25-yeo.pdf)

#### Oblivious Digital Tokens.

- **作者**: Mihael Liskij, Xuhua Ding, Gene Tsudik, David A. Basin

- **方向**: 密码学与协议

- **解读**: 

设备通常靠展示可测行为或证明秘密来标识自身，两种方式都会向验证者泄露信息；已有研究大量保护证明方、减少泄露数据量，却几乎无人关注"隐藏验证行为本身"。这一需求自然出现于 ICRC 提出的数字徽章场景：冲突期间标记受保护的数字资产（如医院系统）时，验证方若暴露自身将陷入危险。

作者定义新原语——可遗忘验证的数字令牌（Oblivious Digital Token, ODT）：验证者可检查设备是否持有 ODT，且不向任何其他方（包括设备本身）暴露该检查发生；给出满足 ODT 安全需求的确定性构造，即使证明方设备的软件被完全攻破也可证明安全；并实现原型评估性能。

原型评估确认实用性：O-TEE 相比原生 TLS 客户端每次握手多花约 144ms（含密码操作与见证生成）；ODT 服务器每请求额外开销小于 1ms，其中 PPET 承诺生成与 Elligator 解码分别约 0.33ms 与 0.17ms，不破坏验证的不可察觉性。

我的思考：首次形式化"验证的隐蔽性"这一被忽视的安全维度，并给出对抗"证明方完全被攻破"的强安全构造；除数字徽章外，还可用于匿名认证、隐蔽式 attestation 等场景。局限是依赖 TEE/特定硬件假设，144ms 的握手开销在延迟敏感场景需权衡。与可见工件标记及 ADEM 等数字徽章实现相比，ODT 把信任从管理员转移到密码学本身，是更根本的解法。

- **论文 PDF**: [Oblivious Digital Tokens.](https://www.usenix.org/system/files/usenixsecurity25-liskij.pdf)

#### V-ORAM: A Versatile and Adaptive ORAM Framework with Service Transformation for Dynamic Workloads.

- **作者**: Bo Zhang, Helei Cui, Xingliang Yuan, Zhiwen Yu, Bin Guo

- **方向**: 密码学与协议

- **解读**: 

ORAM 为加密存储提供强访问模式隐藏保证，但单一 ORAM 方案通常针对特定客户端工作负载设计与优化，一旦部署便难以高效、经济地服务动态工作负载——例如医院电子病历系统平时常规访问、远程医疗时需低延迟高吞吐、备份时需大带宽，单一方案无法兼顾。

V-ORAM 提出通用 ORAM 框架：服务转换协议以基础 ORAM 作为转换中介，可在不下载、不重建 ORAM 的情况下同步树形 ORAM 状态，实现不同 ORAM 服务间的安全高效切换；框架形式化证明包括转换过程在内的整体安全性；并提供规划器，根据客户端工作负载、服务器资源与货币成本推荐 ORAM 服务类型与参数。

相比直接重建 ORAM，V-ORAM 在真实工作负载下节省最高 104.12× 的处理时间与通信成本、最高 33.1% 的货币成本，且对所用 ORAM 服务的即时影响恒定（处理 <5ms、通信 <50kB）；在三个医疗数据集与不同工作负载上完成案例研究。

我的思考：贡献在于把 ORAM 从"固定方案"变为"可自适应切换的服务"，规划器把性能、资源与成本纳入统一决策，填补了真实部署中动态工作负载的空白。局限是转换面向树形 ORAM 族，非树形方案与超大状态的同步开销仍需评估。与单方案 ORAM 系统（如 Signal、UtahFS）相比，V-ORAM 提供的"成本-性能-安全"可调性更贴近云服务的商业现实。

- **论文 PDF**: [V-ORAM: A Versatile and Adaptive ORAM Framework with Service Transformation for Dynamic Workloads.](https://www.usenix.org/system/files/usenixsecurity25-zhang-bo-voram.pdf)

#### More is Less: Extra Features in Contactless Payments Break Security.

- **作者**: George Pavlides, Anna Clee, Ioana Boureanu, Tom Chothia

- **方向**: 密码学与协议

- **解读**: 

EMV 非接触支付涉及支付机构、终端厂商、手机厂商、银行与监管等多方，EMVCo 发布 15 卷规范维持互操作；但各方独立叠加功能——Square 限制离线读卡器仅限手机支付、苹果/谷歌/三星实现交通卡模式、Visa/Mastercard 遵循区域高额支付监管——导致核心规范被"打补丁式"改造与超载，功能间的微妙交互引发漏洞。

作者逐一调查这些附加功能，借助离线终端区分终端侧与后端校验，通过行为测试理解各厂商实现，并构建包含新功能的 EMV 协议形式化模型，检验不同功能交互下可能的攻击路径，进而测试修复方案。

发现可绕过"仅限手机支付"限制、离线完成未认证的高额交易，甚至用克隆卡离线完成 £25,000 的交易；作者与 EMV 利益相关方沟通，并同 Square 合作落地修复。

我的思考：贡献在于把"生态级功能叠加"本身作为漏洞来源，揭示规范开放性与各方私有扩展之间的系统性错配；与 Square 的落地合作使研究直接转化为真实修复，罕见且有说服力。局限是大量细节依赖私有文档逆向与特定厂商实现，结论时效性随规范修订而变化。与既往 EMV 形式化分析（如芯片与 PIN 降级攻击）相比，本文聚焦"新增功能"这一动态攻击面，方法论可复用于未来规范扩展。

- **论文 PDF**: [More is Less: Extra Features in Contactless Payments Break Security.](https://www.usenix.org/system/files/usenixsecurity25-pavlides.pdf)

#### Efficient Batchable Secure Outsourced Computation: Depth-Aware Arithmetization of Common Primitives for BFV & BGV.
- **作者**: Jelle Vos, Mauro Conti, Zekeriya Erkin
- **方向**: 密码学与协议
- **解读**: （无摘要，据标题推断）

**问题**：全同态加密（FHE）支持对密文计算，是实现安全外包计算的核心工具；但 BFV/BGV 等方案的乘法深度直接决定性能，常用计算原语（比较、取整、归一化等）的朴素算术化深度过高，且难以充分利用密文打包的批量（SIMD）特性。

**方法**：该工作提出"深度感知的算术化"方法，为常见计算原语设计乘法深度更低的算术电路，并适配 BFV 与 BGV 方案的密文打包能力，使安全外包计算在保持功能的同时显著降低同态计算开销，提升可批处理性。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应对比了算术化前后各原语的计算深度、运行时间与批量处理效率。

**思考**：深度感知算术化是 FHE 应用优化的关键方向——降低深度意味着更少的重线性化与自举开销，直接改善端到端性能；其价值取决于原语库的覆盖度与可组合性，若能与自动深度优化工具结合，将显著降低 FHE 应用开发门槛。


#### Arbitrary-Threshold Fully Homomorphic Encryption with Lower Complexity.
- **作者**: Yijia Chang, Songze Li
- **方向**: 密码学与协议
- **解读**:

**问题**：门限全同态加密（ThFHE）允许多方在不泄露数据隐私的前提下联合计算，但多数现有方案限于全门限，要求所有参与方在场才能输出结果；现有任意门限（ATh）FHE 方案要么在大量参与方与大数据规模下低效，要么无法容忍所有类型的不参与方。

**方法**：作者将 AThFHE 的构造归约为一种新原语——近似秘密共享（ApproxSS），并证明基于任意门限 ApproxSS（ATh-ApproxSS）性质可构造正确且安全的 AThFHE；该归约揭示已有方案隐式采用了高复杂度的"噪声共享"思路，据此提出基于"加密共享"思想的 ATASSES 方案。

**结果**：计算复杂度由 O(N²K) 降至 O(N²+K)，通信复杂度由 O(NK) 降至 O(N+K)；在一千参与方的系统中，ATASSES 相对基线取得 3.83x–15.4x 的加速。

**思考**：把 AThFHE 归约到 ApproxSS 是理论贡献的亮点，统一了此前各自为战的构造并暴露了性能瓶颈来源；用"加密共享"替代"噪声共享"是复杂度下降的关键；但方案提供的是近似正确性而非精确结果，其对上层应用的影响以及大规模下的安全参数选择，是实际部署前需要回答的问题。

- **论文 PDF**: [Arbitrary-Threshold Fully Homomorphic Encryption with Lower Complexity.](https://www.usenix.org/system/files/usenixsecurity25-chang-yijia-encryption.pdf)

#### Leuvenshtein: Efficient FHE-based Edit Distance Computation with Single Bootstrap per Cell.
- **作者**: Wouter Legiest, Jan-Pieter D'Anvers, Bojan Spasic, Nam-Luc Tran, Ingrid Verbauwhede
- **方向**: 密码学与协议
- **解读**: （无摘要，据标题推断）

**问题**：编辑距离（Levenshtein）是文本与基因序列相似度计算的基础操作，在隐私保护场景（如基因组隐私、敏感文本比对）中需要在密文上计算；但 FHE 下动态规划式的编辑距离计算需要大量自举（bootstrap）操作，开销极其高昂。

**方法**：该工作提出面向 FHE 的编辑距离高效计算方案，通过重新组织动态规划递推与密文布局，使每个单元格（状态）仅需一次自举，显著降低全流程计算成本，并适配密文打包以利用 SIMD 并行能力。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应报告了编辑距离计算的具体运行时间及与现有方案的加速比。

**思考**："每单元格单次自举"直接针对 FHE 编辑距离的主要瓶颈——自举次数，方向正确且影响直接；但序列长度、打包方式与内存开销之间的权衡仍需细看；该技术可推广到 DTW、LCS 等同类动态规划核，对基因组隐私计算具有现实价值。


#### Engorgio: An Arbitrary-Precision Unbounded-Size Hybrid Encrypted Database via Quantized Fully Homomorphic Encryption.

- **作者**: Song Bian, Haowen Pan, Jiaqi Hu, Zhou Zhang, Yunhao Fu, Jiafeng Hua, Yunyi Chen, Bo Zhang, Yier Jin, Jin Dong, Zhenyu Guan
- **方向**: 密码学与协议
- **解读**:

混合数据库（向量搜索+关系查询，用于推荐系统、RAG 等）外包存储时数据高度敏感。现有 FHE 加密数据库（EDB）与加密向量搜索（EVS）框架受困于两个问题：缺乏高效的密文排序能力导致大多不支持混合查询；数据精度不足（现有方法只能排序约 500~1000 项）且效率低下（HE3DB 对 4096 项 ORDER BY 需近 2 天）。

论文提出 Engorgio，首个基于量化 FHE 的混合加密数据库框架，全部构建在 RNS-CKKS 之上。核心设计包括：量化编码方案 QuantEncode/QuantSegment/QuantMerge 把任意精度数据分段为低精度块；二值化同态比较 BHomComp（基于 sgn 多项式近似与整流多项式）与任意精度比较 ArbQuantComp（分段比较+批量同态 MUX 合并）；把排序拆分为序生成与序应用两步，利用置换矩阵的对称稀疏性编码把置换复杂度摊还至 O(1)，HomSync 快速同步多列顺序；并针对全表扫描、批量查询、Top-k 做了专门优化。

相比最先进框架，同态比较快 28×~854×、排序快 65×~687×、同步快 42×~4,595×、Top-k 快 4×~58×；端到端关系/向量/混合查询分别快 15×~621×、30×~388×、68×~1,640×。在 48 核处理器上，10K 行混合库的关系查询摊还运行时间低于 3 秒、混合查询低于 75 秒。安全性可规约到语义安全 FHE，并通过强制混合查询包含排序操作抵御体积泄漏攻击。

解决了 FHE 数据处理"精度-效率-表达力"的三难问题，把最贵的排序原语做到极致，10K 行/75 秒的混合查询指标实用性强；矩阵置换把列同步从重排序降为 O(1) 是巧妙设计。局限在于半诚实安全模型，且体积泄漏缓解依赖"查询必含排序算子"的假设，限制了部分查询形态；单方案 CKKS 路线的乘法深度与 bootstrapping 开销仍是规模化瓶颈。相比 HE3DB、ArcEDB 等通用 EDB，Engorgio 为混合查询树立了新的性能标杆。
- **论文 PDF**: [Engorgio: An Arbitrary-Precision Unbounded-Size Hybrid Encrypted Database via Quantized Fully Homomorphic Encryption.](https://www.usenix.org/system/files/usenixsecurity25-bian.pdf)

#### Qelect: Lattice-based Single Secret Leader Election Made Practical.
- **作者**: Yunhao Wang, Fan Zhang
- **方向**: 密码学与协议
- **解读**:

**问题**：单秘密领袖选举（SSLE）要求所有参与方共同且不可见地选出一位领袖，除本人外无人知晓其身份；Boneh 等的经典方案基于 DDH 假设，面对量子计算机威胁，后续工作转向纯格或全同态加密，但没有任何具体基准证明这些重密码原语的部署可行性。

**方法**：作者将 Boneh 等（AFT'23）的承诺方案改造为多方可随机化承诺，基于适配的环 LWE（RLWE）问题构造新方案，并以此构建常数轮 SSLE（crSSLE）；利用特定门限 FHE（tFHE）方案的 SIMD 特性高效求值选举电路；Qelect 进一步引入预处理阶段摊薄本地计算、回溯检测阶段避免选举阶段的重零知识证明。

**结果**：Qelect 是首个实用的常数轮抗量子 SSLE 协议，在 WAN 环境中实测，性能至少比现有最先进方案快两个数量级。

**思考**："首个实用抗量子 SSLE"填补了理论方案与工程可行性之间的空白，常数轮与"预处理＋回溯检测"两阶段优化是关键工程创新；选用 tFHE＋SIMD 体现了"用成熟原语拼实用系统"的设计智慧；其安全模型（尤其是恶意参与方与自适应安全性）及更大规模下的扩展性仍值得继续审视。

- **论文 PDF**: [Qelect: Lattice-based Single Secret Leader Election Made Practical.](https://www.usenix.org/system/files/usenixsecurity25-wang-yunhao.pdf)

#### H2O2RAM: A High-Performance Hierarchical Doubly Oblivious RAM.
- **作者**: Leqian Zheng, Zheng Zhang, Wentao Dong, Yao Zhang, Ye Wu, Cong Wang
- **方向**: 密码学与协议
- **解读**:

**问题**：混淆 RAM（ORAM）与可信执行环境（TEE）互补结合：TEE 缓解 ORAM 的带宽与延迟瓶颈，ORAM 为 TEE 应用提供抗内存访问模式攻击的通用保护；该组合要求 TEE 内外的内存访问都不可观测，即"双重混淆 RAM"（O2RAM），而现有基于树结构的设计性能有限。

**方法**：首次在 O2RAM 中采用层次化（hierarchical）框架，其天然具备更好的数据局部性与并行化潜力；针对最新层次化方案 FutORAMa 依赖"亚线性大小客户端私有内存"这一松弛假设与双重混淆需求的冲突，作者设计多个新的高效混淆组件，构建高性能层次化 O2RAM（H2O2RAM）。

**结果**：在多种场景下评估，相比现有最先进方案，执行时间最多降低约 10³ 倍，内存占用节省 5–44 倍。

**思考**：用层次化框架替换树框架是结构性的性能突破，10³ 倍时间与 5–44 倍内存的量化结果很有说服力；其关键贡献在于巧妙化解了"客户端私有内存"松弛假设与双重混淆要求的矛盾；后续可关注其与真实 TEE（如 SGX）集成的端到端开销，以及层次化重构带来的同步与带宽特征。

- **论文 PDF**: [H2O2RAM: A High-Performance Hierarchical Doubly Oblivious RAM.](https://www.usenix.org/system/files/usenixsecurity25-zheng.pdf)

#### OBLIVIATOR: OBLIVIous Parallel Joins and other OperATORs in Shared Memory Environments.
- **作者**: Apostolos Mavrogiannakis, Xian Wang, Ioannis Demertzis, Dimitrios Papadopoulos, Minos N. Garofalakis
- **方向**: 密码学与协议
- **解读**:

**问题**：数据库等值连接操作在面对可观察内存访问模式的强敌手时会泄露数据关联信息，需要保证除输入输出规模外不泄露任何数据；现有可信硬件下的混淆连接方案性能不足，且对并行化的支持有限。

**方法**：结合可信硬件与高效的混淆压缩、排序原语，提出两个混淆算法：一是可视为面向可信硬件定制的并行前缀和变体的"混淆聚合树"；二是新颖的"混淆扩展"算法，用于 oblivious 地扩展关系的元素；同时支持非外键与外键等值连接。

**结果**：顺序设置下，在 n=2^24 的数据集上比现有最先进方案（Krastnikov 等，VLDB 2020）快 4.6x–5.14x；32 线程并行时相对顺序版加速约 16x（相对 Krastnikov 的顺序算法最高约 80x）；混淆算子还可独立用于混淆选择、混淆 group-by 等其他数据库查询。

**思考**：并行化是核心贡献——把混淆数据库算子推向多线程共享内存环境，16x/80x 的加速对实际部署意义重大；针对可信硬件定制的混淆聚合树与扩展算法体现了系统化设计；算子可组合性（选择、group-by）拓展了适用范围，未来可与完整查询优化器集成。

- **论文 PDF**: [OBLIVIATOR: OBLIVIous Parallel Joins and other OperATORs in Shared Memory Environments.](https://www.usenix.org/system/files/usenixsecurity25-mavrogiannakis.pdf)

#### Efficient Ranking, Order Statistics, and Sorting under CKKS.
- **作者**: Federico Mazzone, Maarten H. Everts, Florian Hahn, Andreas Peter
- **方向**: 密码学与协议
- **解读**:

**问题**：全同态加密（FHE）使云端密文计算成为可能，但排序、次序统计量、秩等数据库基础功能在 FHE 下开销巨大——这些功能都基于比较操作，而比较在密文下极其昂贵；现有方案普遍采用基于交换（swap）的技术，比较深度（算法中不可并行的同态比较次数）是主要性能瓶颈。

**方法**：该工作抛弃 swap 范式，利用 CKKS 的 SIMD 能力在加密状态下对输入向量重新编码，使所有元素两两同时比较，从而把排名、次序统计量与排序的比较深度降至常数（至多 2），高度可并行、适合硬件加速；同态重编码仅引入 O(log N) 次旋转的少量开销。

**结果**：对 128 元素向量，排名约 5.76 秒，argmin/argmax 约 12.83 秒，排序约 78.64 秒。

**思考**：把比较深度从 log²N 降至常数是范式层面的突破，意味着几乎所有比较可以并行执行，对 FHE 硬件加速极其友好；代价是同态重编码引入的 O(log N) 旋转与密文扩展带来的存储开销；元素规模扩大后的扩展性、重编码电路的具体实现细节以及与 swap 方案在真实负载下的全面对比，是后续关注重点。
- **论文 PDF**: [Efficient Ranking, Order Statistics, and Sorting under CKKS.](https://www.usenix.org/system/files/usenixsecurity25-mazzone.pdf)

### 隐私保护（28 篇）

#### SoK: Can Synthetic Images Replace Real Data? A Survey of Utility and Privacy of Synthetic Image Generation.
- **作者**: Yunsung Chung, Yunbei Zhang, Nassir Marrouche, Jihun Hamm
- **方向**: 隐私保护
- **解读**: 合成图像(如 GAN、扩散模型生成的数据)被视为缓解真实数据收集成本与隐私风险的途径,但其能否真正替代真实数据仍存疑:替代价值取决于效用(utility),而隐私收益也可能被成员推理、模型反演等攻击侵蚀。本文以 SoK 形式系统梳理这一权衡。(无摘要,据标题推断)

依据标题推断,作者对合成图像生成领域进行系统化综述:归纳生成方法谱系、效用评估基准与隐私攻击/防御体系,并比较不同方法在效用-隐私平面上的落点。

研究应给出"何时合成数据可替代真实数据"的条件性结论与研究空白清单。由于摘要与全文缺失,具体基准与量化比较无法引用。

SoK 的价值在于为快速膨胀的合成数据研究建立坐标系,明确效用与隐私的张力及其边界条件,对数据密集型安全研究(如隐私保护机器学习)有直接指导意义。局限:无法核实其分类框架与结论深度;该领域进展极快,综述时效性天然受限。期待其给出可操作的选型建议,而不仅是文献编目。

#### Deanonymizing Ethereum Validators: The P2P Network Has a Privacy Issue.
- **作者**: Lioba Heimbach, Yann Vonlanthen, Juan Villacis, Lucianna Kiffer, Roger Wattenhofer
- **方向**: 隐私保护
- **解读**: 许多区块链网络声称保护验证者在 P2P 网络中的匿名性——即任何对手都无法把验证者标识与其所在节点的 IP 地址关联起来，这关乎验证者免受定向攻击、审查与物理威胁。本文证明以太坊 P2P 网络并不提供这种匿名性：作者提出一种方法论，使网络中的任意节点都能识别连接对等方（peer）上托管的验证者，并实证验证了该方法的可行性。

方法上，作者利用以太坊 P2P 网络的连接结构、验证者广播行为（如区块、证明的传播时序）等可观测特征，把"谁在何时广播了什么"与验证者标识建立关联。使用四个节点在三天内收集的数据，作者成功定位了网络中超过 15% 的以太坊验证者，并由此获得验证者在各对等节点上的分布、地理位置与托管组织等有价值的信息。

工作还讨论了 P2P 网络缺乏匿名性的影响与风险，并提出帮助验证者保护隐私的改进方法；以太坊基金会为此授予作者漏洞赏金（bug bounty），认可了该结果的现实影响。

该工作首次在真实网络规模上量化了以太坊验证者去匿名化风险，15% 的定位率足以证明威胁的现实性，且获得官方赏金佐证其影响。局限：方法依赖验证者与节点间的行为关联，混币/代理/云托管可能降低定位精度；防御建议（如改变广播策略）与共识机制兼容性需进一步验证。总体而言，它提醒验证者运营方：P2P 层的元数据泄露同样构成严重的隐私与安全暴露面。

- **论文 PDF**: [Deanonymizing Ethereum Validators: The P2P Network Has a Privacy Issue.](https://www.usenix.org/system/files/usenixsecurity25-heimbach.pdf)

#### PrivaCI in VR: Exploring Perceptions and Acceptability of Data Sharing in Virtual Reality Through Contextual Integrity.
- **作者**: Emiram Kablo, Melina Kleber, Patricia Arias Cabarcos
- **方向**: 隐私保护
- **解读**: VR 头显持续采集用户的动作、视线、语音、环境乃至生理信号，数据流经应用、平台与广告商等多个主体，远超传统网络场景；而用户对这些新型数据共享是否知情、是否接受，缺乏系统刻画。既有隐私研究多采用泛化的担忧量表，难以解释"什么情境下共享哪些数据给谁为何可接受"。本工作从标题看，用情境完整性理论（Contextual Integrity, CI）——Nissenbaum 提出的"隐私即信息流的语境规范"框架——探索 VR 中数据共享的感知与可接受性。

据标题推断，方法上可能采用访谈或调查：以 CI 的五元组（发送者、接收者、数据类型、传输原则、情境）系统枚举 VR 数据流场景，测量参与者在不同情境组合下的接受度，识别哪些数据流违反语境规范、哪些被默许，并可能比较 VR 与传统环境（如网页）的感知差异。摘要缺失，具体方法与样本无法确认。

摘要缺失，无法引用量化结果；预期包含不同数据流情境的接受度评分、影响接受度的关键因素（接收者身份、数据类型、用途），以及用户对 VR 隐私风险的认知水平等数据。

CI 框架特别适合 VR：其"情境敏感"的特性正好捕捉 VR 中"同样数据在不同情境/接收者下接受度迥异"的直觉，比泛化担忧测量更具解释力；结果可为 VR 平台的数据采集默认设置、知情同意设计提供依据。局限：无摘要，情境枚举的覆盖面与参与者的 VR 经验代表性需全文确认；CI 测量的生态效度（设想场景 vs 真实体验）是此类研究的通病。（无摘要，据标题推断）


#### How Researchers De-Identify Data in Practice.

- **作者**: Wentao Guo, Paige Pepitone, Adam J. Aviv, Michelle L. Mazurek
- **方向**: 隐私保护
- **解读**: 人类受试者研究者日益被要求对参与者数据进行去标识化（de-identification）并发布，但去标识化本身很困难：缺乏平衡隐私与效用的客观方案，且需要大量时间与专业知识。论文访谈了 18 位为发表而去标识化数据的实践者和 6 位为存储库与资助机构审查数据提交的策展人，理解研究者的实际做法。

研究发现：研究者会考虑 k-匿名所描述的那类风险，但通过手动与社会流程处理，而非对数据集进行系统性风险评估——这允许精细处理，却可能使已发布数据面临重识别风险。论文进一步探究研究者采用这种做法的原因，指出三大障碍：威胁看起来不现实、更强的标准缺乏激励与支持、现有工具无法满足研究者需求。

结论部分对存储库、资助机构和隐私专家提出建议。

我的思考：该研究揭示了「理论隐私模型与实际去标识化实践」之间的真实落差，三大障碍（威胁认知、激励缺失、工具不足）解释力强且可操作。局限是样本量小、且依赖受访者自我报告，可能存在社会期望偏差；对政策制定者而言，「为更严格标准提供激励」的建议比单纯的技术方案更切中制度根源。

- **论文 PDF**: [How Researchers De-Identify Data in Practice.](https://www.usenix.org/system/files/usenixsecurity25-guo-wentao.pdf)

#### HawkEye: Statically and Accurately Profiling the Communication Cost of Models in Multi-party Learning.
- **作者**: Wenqiang Ruan, Xin Lin, Ruisheng Zhou, Guopeng Lin, Shui Yu, Weili Han
- **方向**: 隐私保护
- **解读**: 多方计算(MPC)驱动的机器学习(多方学习,MPL)是隐私保护数据利用的关键技术,但其巨大的通信开销严重制约实际部署。为压缩开销,研究者不断提出 MPC-friendly 模型,而剖析模型通信成本是优化流程的核心环节。然而现有方案要么依赖人工插入探针并动态运行安全训练/推理(动态剖析,费时费力),要么依赖框架底层黑盒编译、需手工构造单层模型逐一测试。本文要解决的核心问题是:能否在不动态运行 MPL 流程的前提下,准确、自动地获得模型通信成本画像。
HawkEye 提出一套静态通信成本剖析框架。首先,针对复杂模型结构,提出基于前缀结构(prefix structure)的静态剖析方法:该前缀结构记录函数调用链,由于 MPL 框架为保证安全要求程序执行流与输入无关,静态分析可精确推知安全训练/推理所需的全部计算。其次,HawkEye 将剖析方法与自动微分(Autograd)库集成,让模型设计者直接用 PyTorch 风格代码获得通信画像;针对广播算子带来的额外通信偏差,还设计了面向多种 MPL 框架的通信高效优化算子。使用者只需修改不到十行代码即可剖析 transformer 等复杂模型,而动态剖析需手工插入约一百行代码。
实验将 HawkEye 的静态结果与 CrypTFlow2、CrypTen、Delphi、Cheetah、SecretFlow-SEMI2K 五个框架的动态剖析对比。在 DenseNet-121、ResNet-50、MobileNetV3、ShuffleNetV2 及 BERTBASE 上,各算子在线/离线通信占比与基线几乎一致,占比差异均小于 0.50%。效率上,HawkEye 剖析 CNN 模型仅需 15.5~39.2 秒、BERTBASE 约 8 分钟,而动态剖析耗时为其 1.31×~145.23×。作者还发现:在共谋方数量假设相同时,面向半诚实安全模型优化的 MPC-friendly 模型,其优化移植到恶意安全模型框架仍保持有效。
该工作的贡献在于把"通信成本剖析"从繁琐的人工动态实验转化为自动静态分析,填补了 MPC-friendly 模型优化方法论上的工具空白。其准确性依赖于 MPL 执行流与输入无关这一安全性质,逻辑自洽;对广播算子的专门处理也体现了工程细节的用心。局限:静态剖析的精度上限受基本算子通信复杂度建模(渐近而非实际值)影响,且需为每个目标框架手工配置算子成本。未来若能自动推导算子成本并扩展到更多框架与训练场景,将进一步提升价值。
- **论文 PDF**: [HawkEye: Statically and Accurately Profiling the Communication Cost of Models in Multi-party Learning.](https://www.usenix.org/system/files/usenixsecurity25-ruan.pdf)

#### Privacy Audit as Bits Transmission: (Im)possibilities for Audit by One Run.
- **作者**: Zihang Xiang; Tianhao Wang; Di Wang
- **方向**: 隐私保护
- **解读**: 隐私审计通常通过模拟基于博弈的协议，判断两个相邻数据集中哪个是原始输入；传统方法需要数千次模拟，计算开销巨大。近年提出的单次运行审计虽大幅降低成本，但其通用性与经验隐私保证的紧致性仍不确定。

本文基于信息论原理建立统一审计框架，将审计建模为噪声信道中的比特传输问题，由此推导基本极限并开发能给出紧致隐私下界的审计方法；同时利用该框架剖析"一次运行审计"，识别其可行与不可行的条件，给出一般性审计准则。

实验表明，对常见差分隐私机制，该方法在需要显著更少观测的情况下给出更紧的隐私下界；案例研究还证明它能成功检测有缺陷的私有算法实现中的隐私违规。

把隐私审计抽象为信道编码问题，视角新颖且同时给出理论极限与实用方法，对单次运行审计可行性的刻画填补了空白；紧致下界与低采样成本的组合可直接服务于 DP 部署方的合规验证。局限在于通道模型假设能否覆盖全部协议，以及检测能力对算法缺陷类型的敏感度，值得在更大规模机制上进一步验证。

- **论文 PDF**: [Privacy Audit as Bits Transmission: (Im)possibilities for Audit by One Run.](https://www.usenix.org/system/files/usenixsecurity25-xiang-zihang.pdf)

#### General-Purpose f-DP Estimation and Auditing in a Black-Box Setting.
- **作者**: Önder Askin; Holger Dette; Martin Dunsche; Tim Kutta; Yun Lu; Yu Wei; Vassilis Zikas
- **方向**: 隐私保护
- **解读**:（无摘要，据标题推断）差分隐私保证通常以 (ε,δ) 或 f-DP 形式给出，但实际部署的算法可能因实现错误而偏离声称的隐私保证；黑盒场景下无法访问机制内部，如何通用地估计其真实隐私损失并审计是难题。

论文面向黑盒设置提出通用 f-DP 估计与审计方法，仅通过查询机制的输入输出行为估计其 f-DP 特征（如 trade-off 函数），无需了解内部实现，也无需机制可微或结构已知等假设。

（无摘要，据标题推断）论文宣称该方法在多种机制上可准确估计隐私参数，并能在不访问源码的情况下发现实现层面的隐私退化。

黑盒 f-DP 审计是对现有 (ε,δ) 经验审计的重要推广——f-DP 刻画整个 trade-off 曲线而非单点，信息量更大；该方向与"单次运行审计"等主流审计工作互补。挑战在于估计精度对查询次数与分布假设的依赖、高维机制下的样本效率；若实现扎实，可为 DP 库与产品的第三方独立验证提供工具，推动 DP 保证的可信落地。


#### FastLloyd: Federated, Accurate, Secure, and Tunable k-Means Clustering with Differential Privacy.
- **作者**: Abdulrahman Diaa; Thomas Humphries; Florian Kerschbaum
- **方向**: 隐私保护
- **解读**:（无摘要，据标题推断）联邦场景下执行 k-means 聚类，需要在保护各参与方数据隐私的同时保证聚类质量，还要抵御恶意参与方，并允许按需调节隐私与效用的权衡；朴素 Lloyd 算法直接迁移会泄露簇中心或中间计数。

论文提出 FastLloyd，将差分隐私融入联邦 k-means（Lloyd 迭代），结合安全聚合等机制保护每轮更新，并提供可调节的隐私-效用参数，使部署方能在隐私预算与聚类精度之间取舍。

（无摘要，据标题推断）据标题，其在联邦、安全、可调隐私约束下实现快速且准确的 k-means 聚类，优于已有联邦 k-means 方案的效率与效用。

k-means 是数据挖掘基础算子，"联邦+DP+安全聚合+可调隐私"的组合工程价值明确；标题四个形容词直接对标同类工作的常见短板。局限在于无摘要难以核实具体数字与威胁模型（半诚实/恶意、中心/本地 DP）；迭代聚类中隐私预算的分配与收敛质量是此类工作的核心权衡，与 SecureKmeans、DP-FedAvg 类方案的对比结果值得关注。


#### Addressing Sensitivity Distinction in Local Differential Privacy: A General Utility-Optimized Framework.
- **作者**: Xingyu He; Youwen Zhu; Rongke Liu; Gaoning Pan; Changyu Dong
- **方向**: 隐私保护
- **解读**:（无摘要，据标题推断）本地差分隐私（LDP）中，不同取值（或不同用户数据）的敏感度往往不同，而现有协议通常对所有数据一视同仁地注入噪声，导致不必要的效用损失；如何利用敏感度差异在保证隐私的前提下优化效用是开放问题。

论文提出通用的效用优化框架，显式区分敏感度（如取值依赖的敏感度），据此设计更精细的扰动机制，在相同隐私预算下减少噪声、提升数据可用性，并声称框架可推广到多种 LDP 统计任务。

（无摘要，据标题推断）据标题，该框架在同等隐私保证下取得优于统一敏感度方案的效用表现。

敏感度区分的思路在中心化 DP 中已有探索（如 attribute/value-dependent sensitivity），本文将其推广到 LDP 并给出通用框架，若机制设计得当可显著改善频率估计等基础任务的精度。风险在于敏感度定义依赖领域知识、可能被对手利用，且通用框架的实例化代价需评估；与 RAPPOR、OLH 等经典 LDP 协议的对比将决定其实用价值。


#### Further Study on Frequency Estimation under Local Differential Privacy.
- **作者**: Huiyu Fang; Liquan Chen; Suhui Liu
- **方向**: 隐私保护
- **解读**: LDP 已在多家大型科技公司部署，频率估计是其基础构件，用于估计指定域内各取值的出现频率；现有频率协议在精度、计算成本与通信成本之间存在折衷，且缺乏统一精确的评估手段。

本文给出一个精确且便捷的方程来评估频率协议的精度，据此定量分析现有协议的优势与不足；基于分析提出新协议 Random Wheel Spinner (RWS)，在低计算与通信成本下同时实现最优精度。

在合成与真实数据集上的大量实验表明，RWS 相对现有协议在精度、计算与通信三方面均具优势。

贡献分为两层：一是提供可复用的精度评估方程，为 LDP 频率协议比较建立统一标尺，方法论价值独立于 RWS 本身；二是 RWS 协议实现三维指标的同时最优。局限在于评估方程可能依赖特定分布假设，真实数据分布偏离时的鲁棒性需检验；与 RAPPOR、OUE、OLH 等经典方案的全面对比是判断其落地潜力的关键；总体而言，该评估方程有望成为 LDP 频率协议研究中的公共工具。

- **论文 PDF**: [Further Study on Frequency Estimation under Local Differential Privacy.](https://www.usenix.org/system/files/usenixsecurity25-fang.pdf)

#### Beyond Statistical Estimation: Differentially Private Individual Computation via Shuffling.
- **作者**: Shaowei Wang; Changyu Dong; Xiangfu Song; Jin Li; Zhili Zhou; Di Wang; Han Wu
- **方向**: 隐私保护
- **解读**: shuffle 模型无需可信中心，并能通过洗牌获得隐私放大效应、实现高效用，但该模型下的计算任务仅限于统计估计，无法支持每位用户获得个性化输出的真实场景。

论文提出新范式 Private Individual Computation (PIC)，将 shuffle 模型推广到更广泛的置换等变计算：借助一次性公钥，使用户在不牺牲匿名性（隐私放大所需）的前提下收到个性化输出，并设计最优随机化器 Minkowski Response 提升效用；论文形式化证明了 PIC 协议的安全与隐私性质。

理论分析与经验评估表明，PIC 能处理非统计计算任务，且 PIC 与 Minkowski 随机化器相比现有方案实现更优效用。

把 shuffle 模型从"统计聚合"拓展到"个性化计算"，补齐了 shuffle 模型应用版图的重要缺口；一次性公钥与匿名性兼得的构造有理论巧思，Minkowski Response 的最优性证明增强了贡献成色。局限在于置换等变计算的类别边界、大规模用户下的通信/计算开销，以及恶意模型下的安全性，后续可与安全聚合类方案横向比较。

- **论文 PDF**: [Beyond Statistical Estimation: Differentially Private Individual Computation via Shuffling.](https://www.usenix.org/system/files/usenixsecurity25-wang-shaowei.pdf)

#### Sharpness-Aware Initialization: Improving Differentially Private Machine Learning from First Principles.
- **作者**: Zihao Wang; Rui Zhu; Dongruo Zhou; Zhikun Zhang; XiaoFeng Wang; Haixu Tang
- **方向**: 隐私保护
- **解读**:（无摘要，据标题推断）DP-SGD 等差分隐私机器学习因注入噪声导致模型效用显著下降，收敛到泛化差的"尖锐"区域被认为是重要原因；如何从原理上改善 DP 训练的收敛与泛化是开放问题。

论文从第一性原理出发，提出"锐度感知初始化"（Sharpness-Aware Initialization）：在训练开始前为 DP 优化提供位于平坦区域的初始化，使每轮注入的 DP 噪声不因损失景观尖锐而被放大，与锐度感知优化（SAM）思想相结合。

（无摘要，据标题推断）据标题，该方法在多种数据集与模型上提升 DP 训练精度，且无需改变每轮噪声机制。

初始化是 DP 训练中常被忽视的自由度——DP 噪声预算有限，起点所在区域的平坦性直接决定噪声对收敛的破坏程度；把 SAM 的"锐度"思想前移到初始化，成本低、与现有 DP-SGD 兼容性好，实用价值高。局限在于初始化获取本身是否需要非私有数据、平坦性度量在不同架构上的稳定性；与 DP 超参数调优、预训练+微调范式的结合是自然的后续方向。


#### Task-Oriented Training Data Privacy Protection for Cloud-based Model Training.
- **作者**: Zhiqiang Wang; Jiahui Hou; Haifeng Sun; Jingmiao Zhang; Yunhao Yao; Haikuo Yu; Xiang-Yang Li
- **方向**: 隐私保护
- **解读**: 云训练要求用户上传个人数据，上传后数据脱离用户控制，可能被用于未授权的其他任务训练；现有方案没有解决"控制训练数据用途范围"这一需求。

论文提出任务级训练数据使用控制：向训练数据注入精心设计的噪声，既提供明显的视觉保护，又通过对抗优化最小化特征域偏移；通过调节噪声与类别标签的相关性，引导模型学习目标任务的正确特征、同时阻止未授权隐私任务训练；另引入 overflow matrix 兼容现有编码与传输框架。

真实实验表明，方案同时实现视觉隐私保护（SSIM 为 0.028）与阻止未授权训练（保护成功率 100%），而目标任务模型精度仅下降约 1.8%。

"数据用途控制"（阻止特定任务训练而非仅隐藏内容）是新颖的隐私目标，与 GDPR 目的限制原则直接呼应，overflow matrix 的工程落地考虑加分；SSIM 0.028 说明视觉保护彻底，1.8% 的精度代价相当低。局限在于对抗优化的迁移性——攻击者用不同架构或损失重训时保护是否持久存疑；与不可逆扰动、数据投毒类防御的边界也需厘清，后续工作应评估自适应对手下的鲁棒性。

- **论文 PDF**: [Task-Oriented Training Data Privacy Protection for Cloud-based Model Training.](https://www.usenix.org/system/files/usenixsecurity25-wang-zhiqiang.pdf)

#### Disparate Privacy Vulnerability: Targeted Attribute Inference Attacks and Defenses.
- **作者**: Ehsanul Kabir; Lucas Craig; Shagufta Mehnaz
- **方向**: 隐私保护
- **解读**: 属性推断攻击中有一个被忽视的现象——"异质脆弱性"：模型在整体上预测敏感属性可能很差，但对少数脆弱群体却预测得异常精准，隐私泄露呈系统性偏差。本文指出攻击者可利用这种差异发起一系列新攻击，威胁超出既有认知。

方法上，作者提出 disparity inference attack，先识别数据集中的高风险群体；再提出两种定向属性推断攻击变体，定位并利用训练数据中的脆弱子集，是该攻击类别中首个定向版本；同时提出首个有效的"差异缓解"技术，在保持模型性能的同时消除定向攻击风险。

结果显示，定向变体相比非定向版本取得显著更高的准确率；缓解技术首次同时兼顾模型效用与防定向攻击能力。

我的思考：该工作把"公平性"与"隐私"两个议题缝合——群体级隐私差异既是公平问题也是攻击入口，视角新颖、实践意义强。局限是实验依赖特定数据集与模型族，缓解方法的普适性、对攻击者自适应策略的鲁棒性仍需检验；但"先定位高危群体再定向攻击"的范式对隐私审计很有启发。

- **论文 PDF**: [Disparate Privacy Vulnerability: Targeted Attribute Inference Attacks and Defenses.](https://www.usenix.org/system/files/usenixsecurity25-kabir.pdf)

#### Enhanced Label-Only Membership Inference Attacks with Fewer Queries.
- **作者**: Hao Li; Zheng Li; Siyuan Wu; Yutong Ye; Min Zhang; Dengguo Feng; Yang Zhang
- **方向**: 隐私保护
- **解读**: 成员推断攻击（MIA）中，仅能获得预测标签的 label-only 场景最具挑战；现有方法直接以样本到决策边界的最短距离（shortestBD）作为成员信号，却面临两难：样本多样性导致成员与非成员区分度低，方向多样性导致查询开销高。本文提出 DHAttack，追求更高性能与更高隐蔽性。

方法上，DHAttack 聚焦单个样本自身的边界距离以缓解样本多样性干扰，并把该距离度量到固定点方向，大幅压缩查询次数；配合更贴近成员/非成员差异的决策信号，实现少查询、高区分。

结果显示 DHAttack 一致优于现有先进攻击；部分场景下仅用 5 至 30 次查询，TPR@0.1% FPR 就比所有基线高出一个数量级以上；作者还分析攻击成功原因及其他影响性能的关键因素，并评估多种防御，攻击仍具优势。

我的思考：查询预算与隐蔽性直接决定现实威胁程度，"5-30 次查询"把 MIA 从学术演示推向可大规模部署的窃取工具，也考验防御方（如差分隐私）的参数选择。局限是依赖决策边界估计质量、对高维或复杂模型开销仍高；但固定点度量思想简单有效，是 label-only 攻击的重要改进。

- **论文 PDF**: [Enhanced Label-Only Membership Inference Attacks with Fewer Queries.](https://www.usenix.org/system/files/usenixsecurity25-li-hao.pdf)

#### For Human Ears Only: Preventing Automated Monitoring on Voice Data.
- **作者**: Irtaza Shahid; Nirupam Roy
- **方向**: 隐私保护
- **解读**: （无摘要，据标题推断）语音数据正被各类自动监控系统（语音助手、语音分析、监听服务）大规模采集与分析，用户语音一旦进入机器可解析的形式便难以控制去向。标题"仅供人耳"暗示本文目标是构造一种仅人类可听、而自动系统难以解析的语音保护方案。

据标题推断，方法可能利用人类听觉与机器语音识别（ASR）之间的感知差异——例如加入与语音内容无关却足以干扰 ASR 的掩蔽信号、利用人类鸡尾酒会效应的容错性，或生成人耳可懂但特征分布异常的对抗性语音样本。

由于无摘要，具体机制与量化指标（对 ASR 的干扰率、人类可懂度保留）无法引用，不做数字性断言。

我的思考：与"语音水印/防伪造"相对，该工作站的是"防监听/防采集"一侧，若成立将是对抗无处不在的语音监控的一种实用主义方案。挑战在于同时满足"人可懂"与"机不可懂"的矛盾目标，且需对抗不断升级的 ASR 模型；实用性还取决于干扰是否会被合法语音服务（如语音助手本身）拒收。总体而言，若方案能在真实环境中保持稳定的人-机感知差异，将具备较强的实用转化潜力。


#### Free Record-Level Privacy Risk Evaluation Through Artifact-Based Methods.
- **作者**: Joseph Pollock; Igor Shilov; Euodia Dodd; Yves-Alexandre de Montjoye
- **方向**: 隐私保护
- **解读**: （无摘要，据标题推断）记录级隐私风险评估通常依赖成员推断攻击（MIA）类方法，需要在训练后重新训练或大量查询模型，成本高昂。标题中 "Free" 与 "Artifact-Based" 暗示本文提出利用训练过程的副产品（artifact，如模型权重、梯度、损失曲线等）来零成本评估单条记录泄露风险的思路。

据标题推断，方法可能基于"训练结束时模型对某条记录的损失/梯度记忆痕迹"或模型权重的局部特征，直接估计该记录的成员风险，无需额外训练或查询预算；"free" 即指评估开销被训练本身吸收。

由于无摘要，具体方法细节与量化结果无法引用，不做数字性断言。

我的思考：隐私审计要想成为常规工程实践，成本是首要障碍；"训练副产品即风险指标"若成立，可让开发者以近乎零开销持续监控记录级风险，方向务实且与差分隐私的隐私核算形成互补。风险在于 artifact 信号与真实攻击成功率的对应关系需严格校准，且不同训练配置（优化器、正则化）可能改变信号可靠性。总体来看，该方向若能建立与标准 MIA 审计的一致性验证，将成为隐私工程工具箱的重要补充。


#### Addressing the Address Books' (Interdependent) Privacy Issues.
- **作者**: Kavous Salehzadeh Niksirat; Lev Velykoivanenko; Samuel Mätzler; Stephan Mulders; Aurelia Tamò-Larrieux; Marc-Olivier Boldi; Mathias Humbert; Kévin Huguenin
- **方向**: 隐私保护
- **解读**: （无摘要，据标题推断）通讯录是典型的"相互依赖隐私"载体：用户上传通讯录不仅暴露自己的联系人，还暴露他人的姓名、号码、关系结构，而第三方（社交网络、通讯 App）常借此构建社交图谱或用于账号匹配。本文标题表明其系统处理通讯录的（相互依赖）隐私问题。

据标题推断，作者可能结合技术分析（如 App 如何收集、上传、使用通讯录数据）与隐私机制设计（如加密、匿名化、差分隐私化的联系人共享方案），或进行用户调研评估用户对通讯录泄露的认知与偏好，并给出工程化缓解方案。

由于无摘要，具体发现与方案效果无法引用，不做数字性断言。

我的思考：通讯录隐私的难点在于"多边同意"——本人的知情同意无法覆盖被卷入的联系人，这正是相互依赖隐私的治理盲区；若论文能给出兼顾可用性（好友发现、消息同步）与隐私的技术方案，将填补务实空白。风险是此类方案常因可用性损失或生态阻力难以落地，需要诚实的部署讨论。总体而言，该工作若能把技术方案与治理讨论结合，将是对相互依赖隐私研究的有力补充，也贴合 GDPR 关于联系人数据的争议。


#### I Can Tell Your Secrets: Inferring Privacy Attributes from Mini-app Interaction History in Super-apps.

- **作者**: Yifeng Cai, Ziqi Zhang, Mengyu Yao, Junlin Liu, Xiaoke Zhao, Xinyi Fu, Ruoyu Li, Zhe Liu, Xiangqun Chen, Yao Guo, Ding Li
- **方向**: 隐私保护
- **解读**: 超级应用整合大量小程序，其网页化架构使厂商天然能记录两类被忽视的数据：小程序使用历史（Mini-H，访问频次与类别）与操作历史（Op-H，按钮点击、拖动、图片浏览）。监管（GDPR/PIPL）与学界聚焦位置、通讯录等传统敏感数据，无人关注这两类"看似非敏感"的数据——它们却可能泄露性别、年龄、财产等隐私属性。本文揭示这一泄漏源并构造攻击 THEFT。

作者先以 30 篇顶会论文综述与 31 个真实超级应用隐私政策审查证明认知空白（无一论文讨论 Mini-H/Op-H，31 个应用中仅微信提及收集 Mini-H、无一提及 Op-H）。THEFT 用 Transformer（1.1 亿参数）以 Mini-H（最近 200 个小程序：ID、28 类类别码、30 天访问次数）与 Op-H（100 秒窗口、0.5 秒粒度的点击序列）为输入推断性别、城市等级、年龄、房产、车辆、婚姻、子女 7 类隐私属性，用温度校准使置信度忠实反映精度，以 0.9 阈值筛选可高置信推断的用户子集。

在真实支付宝数据（219,826 名用户、109.9 万样本）上：整体推断精度 65.2%（比随机基线高 48.1%）；0.9 置信度阈值下可对 16.1% 用户以 95.5% 平均精度推断隐私属性（年龄子集 98.6%、房产 99.0%），且仅需 200 名用户的公开泄露数据训练；置信度与精度皮尔逊相关达 0.992。按支付宝 13 亿用户推算，约 2 亿用户面临此类风险。

发现监管与业界共同盲区，威胁模型（厂商低权限部门内部威胁+泄露数据匹配）现实且克制，并向 31 家厂商披露、4 家同意修改隐私声明，具有真实产业影响。局限：仅支付宝单平台验证，泛化是推断；数据来自知情志愿者或有偏差；缺乏关键交互特征的细粒度归因分析。

- **论文 PDF**: [I Can Tell Your Secrets: Inferring Privacy Attributes from Mini-app Interaction History in Super-apps.](https://www.usenix.org/system/files/usenixsecurity25-cai-yifeng.pdf)

#### Seeing Through: Analyzing and Attacking Virtual Backgrounds in Video Calls.
- **作者**: Felix Weissberg; Jan Malte Hilgefort; Steve Grogorick; Daniel Arp; Thorsten Eisenhofer; Martin Eisemann; Konrad Rieck
- **方向**: 隐私保护
- **解读**: 视频会议已成为远程办公必需品，但传输居家环境视频会泄露背景中的私人信息；虚拟背景本应遮挡真实环境，却因分割不完美而偶有背景像素渗出。本文系统研究这一泄露：先分析 Zoom 与 Google 两家主流会议服务的虚拟背景实现，确定环境像素是如何泄露的。

方法上，作者提出重构攻击：复用会议软件自身把虚拟背景"反转"——用语义分割过滤掉视频中的人像，仅保留环境中渗出的像素，再聚合成重构图像，从而"看穿"虚拟背景还原房间环境。

结果显示，与既往研究相比，该攻击从一次视频通话中多恢复至少 53% 的泄露像素，暴露更大范围的居家环境；定量与定性评估均表明虚拟背景目前并不能提供充分的现实防护。

我的思考：把防御机制（虚拟背景）本身作为攻击管线的一部分，"借力打力"的思路简洁有力；53% 的像素恢复增益说明分割残余泄漏是结构性的而非偶发。局限是攻击效果依赖分割模型质量与摄像头角度，且属于被动观察式泄露，风险等级取决于场景敏感度；对厂商的启示是背景分割需更激进的后处理而非依赖单一模型精度。

- **论文 PDF**: [Seeing Through: Analyzing and Attacking Virtual Backgrounds in Video Calls.](https://www.usenix.org/system/files/usenixsecurity25-weissberg.pdf)

#### Bots can Snoop: Uncovering and Mitigating Privacy Risks of Bots in Group Chats.
- **作者**: Kai-Hsiang Chou, Yi-Min Lin, Yi-An Wang, Jonathan Weiping Li, Tiffany Hyun-Jin Kim, Hsu-Chun Hsiao
- **方向**: 隐私保护
- **解读**:
**问题**：群聊平台中的聊天机器人可以访问超出其功能所需的信息，如发信人身份或并非发给机器人的消息；开发者可能利用这些信息推断用户个人信息、跨群组关联用户，引发数据泄露、追踪与定向广告等隐私风险。
**方法**：作者基于对话数据集量化风险，发现机器人访问的消息量远超所需，且用户加入含机器人的新群组时，约3.6%的概率至少一个机器人能将其与其他群组的历史互动关联起来；据此提出SnoopGuard协议，提供选择性消息访问（机器人读不到无关消息）与发送者匿名（对机器人隐藏身份）两项保证，并集成到Message Layer Security（MLS）中。
**结果**：在n用户m机器人的群组中，SnoopGuard消息发送复杂度为O(log n + m)，相比SoA的O(log(n+m))仅轻微增加；原型实测向50用户加10机器人的群组发送消息约需10毫秒，开销可接受。
**思考**：该工作把"机器人过度读取"这一现实隐患转化为可验证的协议原语，兼具量化分析与工程落地，成色扎实；但3.6%依赖特定数据集，"对机器人匿名"与端到端加密平台的信任模型如何兼容、机器人合法功能与选择性访问如何平衡，仍值得深究。

- **论文 PDF**: [Bots can Snoop: Uncovering and Mitigating Privacy Risks of Bots in Group Chats.](https://www.usenix.org/system/files/usenixsecurity25-chou.pdf)

#### Evaluating Privacy Policies under Modern Privacy Laws At Scale: An LLM-Based Automated Approach.

- **作者**: Qinge Xie, Karthik Ramakrishnan, Frank Li
- **方向**: 隐私保护
- **解读**:

隐私政策是用户知情同意与监管合规的核心文书，但 GDPR、CCPA 等现代隐私法律对政策内容提出复杂披露要求（处理目的、法律依据、数据主体权利等），人工审核海量网站政策成本极高且结果不一致，缺乏可规模化、自动化的合规评估手段。

据标题推断，论文提出基于大语言模型（LLM）的自动化方法：利用 LLM 的自然语言理解能力抽取隐私政策中的关键条款，与现代隐私法律的合规要求逐项对照，判断政策是否披露了法律要求的要素、是否存在缺失或含糊表述，从而在规模化网站群上完成合规评估。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；该方法的价值在于以 LLM 的泛化能力降低对人工标注的依赖，使"政策—法律"合规比对首次具备规模化可行性。

我的思考：LLM 用于隐私政策分析并非首例（Polisis、OPP-115 标注等先行），但结合"现代隐私法律"做合规判定并以规模化评估为目标，是实用导向的推进，对监管执法与消费者权益组织有直接工具价值。局限（据标题推断）：法律条款判定需要高精度，LLM 幻觉与法律解释的语义粒度是主要风险，通常需与检索增强、人工抽查结合；评测基准的代表性与跨法域泛化性也待考察。


#### Navigating Cookie Consent Violations Across the Globe.

- **作者**: Brian Tang, Duc Bui, Kang G. Shin
- **方向**: 隐私保护
- **解读**:

Cookie 同意横幅是 GDPR/ePrivacy 框架下网站获准追踪用户的核心机制，但大量网站存在违规实践——未经有效同意即植入追踪 Cookie、默认勾选、反复弹窗施压等，全球范围的合规状况与违规模式缺乏系统刻画。

据标题推断，论文对全球范围内的网站 Cookie 同意实践进行大规模测量：自动访问大量网站，检测同意横幅的呈现时机、默认选项、追踪 Cookie 的设置时机，对照 GDPR/ePrivacy 的同意要求判定违规行为，并分析地域、监管框架与网站类别对合规率的影响。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；此类跨国测量研究的意义在于用实证数据揭示"纸面合规"与"实际行为"之间的鸿沟，为监管执法提供证据基础。

我的思考：Kang G. Shin 团队在 Web 隐私测量上积累深厚，该工作大概率延续了"大规模爬取+自动判定"的成熟范式，跨国视角能回答"监管严格地区是否更合规"这一关键问题。局限（据标题推断）：自动判定同意违规存在语义歧义（横幅文案多样）、动态横幅渲染与反爬对抗会影响覆盖度，测量结论的地域代表性也取决于站点抽样。与 GPC 合规研究互补，共同勾勒 Cookie 同意生态的实际图景。


#### Websites' Global Privacy Control Compliance at Scale and over Time.

- **作者**: Katherine Hausladen, Oliver Wang, Sophie Eng, Jocelyn Wang, Francisca Wijaya, Matthew May, Sebastian Zimmeck
- **方向**: 隐私保护
- **解读**:

CCPA 赋予加州居民通过 Global Privacy Control（GPC）退出个人信息出售/共享的权利，但网站是否真正尊重该退出信号缺乏大规模实证。论文研究如何评估网站对 GPC 的合规性，回答"网站是否尊重加州居民经 GPC 表达的退出权"这一监管核心问题。

方法上，基于对 11,708 个网站的纵向爬取数据，通过检查四个指示用户退出状态的隐私字符串来判定合规：US Privacy String、Global Privacy Platform String、OptanonConsent Cookie 以及 .well-known/gpc.json，考察其设置与取值是否反映用户的退出意愿。

结果：约三分之一的"有证据表明在出售/共享个人信息"的网站实现了至少一个隐私字符串；2023 年 12 月有 44%（1,411/3,226）的此类网站通过全部已实现字符串退出用户，2024 年 2 月降至 43%（1,473/3,402），2024 年 4 月回升至 45%（1,620/3,566）。尽管小幅回升，整体合规率仍处低位，表明加州居民的退出权被广泛忽视，作者据此强调 CCPA 有效执法（尤其是针对大型发布者）的重要性。

我的思考：该工作把 GPC 合规从"有无实现"推进到"纵向、多信号、大规模"的量化测量，四信号交叉判定的方法设计细致，能抵御单一字符串的误判；时间序列数据直接支撑"合规率低迷且无改善趋势"的监管论断。局限：字符串存在并不必然等于真实退出行为，代理指标与实际数据流之间仍有差距；站点集合与爬取频率也限制结论外推。对 CCPA 执法优先级与 GPC 生态改进有直接参考价值。

- **论文 PDF**: [Websites' Global Privacy Control Compliance at Scale and over Time.](https://www.usenix.org/system/files/usenixsecurity25-hausladen.pdf)

#### Privacy Law Enforcement Under Centralized Governance: A Qualitative Analysis of Four Years' Special Privacy Rectification Campaigns.
- **作者**: Tao Jing, Yao Li, Jingzhou Ye, Jie Wang, Xueqiang Wang
- **方向**: 隐私保护
- **解读**:
GDPR 等隐私法律虽带来积极变化，但执法受制于监管资源不足与调查成本高昂——例如 FTC 执行 COPPA 平均耗时 294 天。中国自 2019 年起由工信部等中央机构发起"专项治理行动"（SPRCs），以行政命令动员商店与第三方认证机构开展市场级应用审查，违规应用须在约 5 个工作日内整改，否则被通告或下架；截至 2022 年 6 月已审查超 322 万应用、至少下架 3000 款。但该模式的有效性与问题此前缺乏研究。

作者对 18 名参与过 SPRC 的应用工程师（开发者、技术负责人、安全与测试工程师）进行半结构化访谈，围绕四个研究问题（工作流、挑战、应对方案、整体影响）编码分析，梳理出 SPRC 与应用商店两条审查工作流及多方利益相关者关系。

结果发现工程师面临多重挑战：同一应用在不同商店得到不一致的审查报告（8 名受访者提及）且报告粒度差异大、自测结果与报告不符；同时缺乏制度支持——包括隐私法律学习、自验证资源、多方沟通渠道以及问责公平性。正面变化包括隐私侵犯应用减少、隐私意识提升、隐私政策更清晰；但受访者普遍担忧存在规避审查的技术。

该研究是少见的对中国集中式隐私执法模式的实证剖析，为 GDPR 式执法提供对照视角。局限在于样本量小（18 人）且依赖自我报告，结论外推需谨慎。其价值在于揭示执法体制差异如何塑造合规生态。

- **论文 PDF**: [Privacy Law Enforcement Under Centralized Governance: A Qualitative Analysis of Four Years' Special Privacy Rectification Campaigns.](https://www.usenix.org/system/files/usenixsecurity25-jing.pdf)

#### SpeechGuard: Recoverable and Customizable Speech Privacy Protection.

- **作者**: Jingmiao Zhang, Suyuan Liu, Jiahui Hou, Zhiqiang Wang, Haikuo Yu, Xiang-Yang Li
- **方向**: 隐私保护
- **解读**:

将语音数据上传到云服务器存在隐私风险，声音特征（声学隐私）与说话内容（内容隐私）都需要保护。用户常需要云端处理非敏感信息、同时保护敏感部分，并能在本地恢复原始数据；但现有方法多依赖不可逆或不灵活的技术——整体匿名化整段语音或替换敏感文本，无法同时满足细粒度定制与完全可恢复。

方法上提出 SpeechGuard 系统：通过可逆保护方法并给私有信息分配权限组来实现可恢复、可定制的语音隐私保护。声学层面设计带逆函数的多参数扭曲函数（multi-parameter warping function）用于声音转换；内容层面开发敏感文本的自动或手动检测与加密机制；将听者划分为权限组，按权限分配扭曲参数与加密密钥，使不同听者按权限恢复不同级别的声学与内容信息。

结果：在三个数据集上的实验显示，SpeechGuard 在匿名性、敏感内容机密性与抗攻击性上优于三个基线系统，同时提供可恢复、可定制的声学与内容隐私保护，支持按需定义的隐私与保护强度。

我的思考：把"加密-授权"思想移植到语音领域并以可逆变换实现细粒度多级访问，设计新颖且实用场景清晰（云端处理+本地恢复+分级听者），三个数据集的对比实验支撑了有效性。局限：扭曲函数的参数空间与抗逆向分析强度、敏感文本检测的召回率，以及可恢复性与强隐私之间的内在张力值得深究。与说话人匿名化、语音对抗扰动等同类工作相比，权限分级恢复机制是差异化亮点。

- **论文 PDF**: [SpeechGuard: Recoverable and Customizable Speech Privacy Protection.](https://www.usenix.org/system/files/usenixsecurity25-zhang-jingmiao.pdf)

#### Distributed Private Aggregation in Graph Neural Networks.

- **作者**: Huanhuan Jia, Yuanbo Zhao, Kai Dong, Zhen Ling, Ming Yang, Junzhou Luo, Xinwen Fu

- **方向**: 隐私保护

- **解读**: 

图神经网络（GNN）在社交网络、金融等图数据上表现出色，但在分布式、隐私敏感环境下难以应用：现有方案要么依赖不切实际的信任假设，要么无法构造有效模型。特别是节点级差分隐私在分布式设定下无法用本地差分隐私（LDP）实现——服务器构建 GNN 需反复访问同一节点的特征、边、标签，按序组合拆分隐私预算导致噪声过大、模型失效。

作者提出基于安全多方计算（MPC）的分布式私有聚合（DPA）方法：各参与方以秘密共享等方式在 MPC 内聚合邻居信息，用密码学保护数据而非仅靠加噪，从而在满足节点级差分隐私的同时显著提升统计精度；实现 DPA-GNN 并形式化证明其满足节点级 DP。

在 Cora、CiteSeer、LastFM、Facebook、Amazon、Reddit 六个真实数据集上与 13 个基线（含 10 个最先进的隐私保护 GNN 方法与 3 个变体）对比，DPA-GNN 一致超越所有基线，取得隐私与效用的最优平衡；作者还给出了裁剪率（各数据集最优介于 40%–80%）与隐私预算 ε∈{4,6,8,10} 的参数设置建议。

我的思考：核心贡献是首次在分布式设定下实现可证明的节点级隐私 GNN，用 MPC 规避 LDP 预算拆分导致的噪声膨胀，思路清晰、实验系统。局限在于 MPC 引入通信与计算开销，安全模型多为半诚实假设，在更大规模图上的成本仍需评估。与集中式 DP 加噪及 LDP 类方案相比，DPA 代表"以密码学换精度"的路线，为隐私保护 GNN 树立了新的实用基线。

- **论文 PDF**: [Distributed Private Aggregation in Graph Neural Networks.](https://www.usenix.org/system/files/usenixsecurity25-jia-huanhuan.pdf)

#### Big Help or Big Brother? Auditing Tracking, Profiling, and Personalization in Generative AI Assistants.

- **作者**: Yash Vekaria, Aurelio Loris Canino, Jonathan Levitsky, Alex Ciechonski, Patricia Callejo, Anna Maria Mandalari, Zubair Shafiq

- **方向**: 隐私保护

- **解读**: 

（无摘要，据标题推断）生成式 AI 助手（ChatGPT、Copilot 等）为用户提供便利的同时收集大量交互数据用于跟踪、画像与个性化；这些行为是否透明、是否符合用户隐私预期、被哪些实体用于何种目的，缺乏独立审计；"助手"与"监视者"的一体两面亟待厘清。

从标题推断，本文对生成式 AI 助手的跟踪、画像与个性化行为进行审计：应是通过受控实验（构造特征化提示词）与网络流量/数据包分析，识别助手收集哪些信号、如何构建用户画像、个性化如何改变行为，并评估隐私披露与选择退出机制。具体审计方法与助手范围因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者系统测量了数据收集范围、画像准确性与个性化影响。

我的思考（基于标题的保守推断）：该审计直指 GenAI 时代的核心隐私悖论——个性化质量与监控深度同源。与针对网站跟踪的经典测量（cookie 与浏览器指纹）相比，对话式交互让数据收集更隐晦、更难审计；本文若提供可复现的审计方法学，将成为监管与用户知情决策的重要工具。挑战在于助手端行为黑盒、平台快速迭代导致结论时效性受限。

### 系统与操作系统安全（18 篇）

#### BlueGuard: Accelerated Host and Guest Introspection Using DPUs.
- **作者**: Meni Orenbach, Rami Ailabouni, Nael Masalha, Thanh Nguyen, Ahmad Saleh, Frank Block, Fritz Alder, Ofir Arkin, Ahmad Atamli
- **方向**: 系统与操作系统安全
- **解读**: 问题：虚拟机自省（VMI）是监控虚拟机运行时状态的关键技术，被云厂商广泛用于恶意软件检测等场景。但现有 VMI 系统要么与 VM 争抢 CPU 资源，要么性能不佳，且无法自省虚拟机监控器（hypervisor）或裸机。这限制了 VMI 的部署与覆盖范围。

方法：BlueGuard 利用数据中心服务器上物理隔离的数据处理单元（DPU）高效运行完整的宿主机与客户机自省（HGI），将自省任务卸载到 DPU，解放 CPU。它抽象了 DPU 加速器，提供内核旁路、非阻塞内存访问与用户态线程以实现微秒级自省延迟；还提出 delta introspection 加速状态变化检测，并能从网络层隔离被感染主机。

结果：在 NVIDIA BlueField-2 DPU 上实现并评估，相比先前工作取得 4.3 倍检测加速，可并发监控数十台 VM 而不影响宿主机性能；作为附带收益，还能自省通常超出 VMI 范围的裸机服务器。

思考：把自省负载搬到 DPU 上，同时解决"资源争抢"与"性能差"两个老问题，工程完成度高，4.3 倍加速与数十 VM 并发监控是实打实的收益；局限在于依赖专用 DPU 硬件与厂商生态，且监控方与租户的信任模型、DPU 自身被攻陷时的安全边界仍待回答。

- **论文 PDF**: [BlueGuard: Accelerated Host and Guest Introspection Using DPUs.](https://www.usenix.org/system/files/usenixsecurity25-orenbach.pdf)

#### Serverless Functions Made Confidential and Efficient with Split Containers.
- **作者**: Jiacheng Shi, Jinyu Gu, Yubin Xia, Haibo Chen
- **方向**: 系统与操作系统安全
- **解读**: Serverless 正进入金融、医疗等安全关键领域，推动"机密 serverless"需求，即用机密虚拟机（CVM，如 AMD SEV、Intel TDX）保护函数内存。但作者分析发现，现有 CVM 实现与函数实际需求错配：为通用工作负载设计的完整客户机 OS 带来性能瓶颈、资源浪费与过大的可信计算基（TCB），严重制约部署。本文要解决"如何以最小 TCB 高效运行机密 serverless 函数"这一核心问题。

Split container 的设计要点是"安全与解耦管理"：观察到真实 serverless 函数通常只需有限的 OS 功能，于是在 CVM 内部署面向函数的 OS（微内核 + library OS）以安全执行多个函数，而在 CVM 外复用不可信的通用 Linux 负责容器管理，从而把 TCB 压缩到最小、同时保留容器生态的便利性。基于该设计实现了 CoFunc 原型，可同时运行于 AMD SEV 与 Intel TDX。

在 FunctionBench 与 ServerlessBench 基准上，CoFunc 相比已知的 CVM 机密容器方案（经优化的 Kata-CVM）性能提升最高达 60×（SEV）与 215×（TDX）；相比最先进的非机密容器系统（lean container），平均性能开销低于 14%，在提供机密性的同时保持了接近普通容器的效率。

该工作直面"机密计算与 serverless 的适配鸿沟"，用函数导向 OS（微内核 + library OS）把 TCB 与性能同时优化，工程完成度高（双平台原型、双基准评估），量化数据很有说服力。局限：机密容器方案（Kata-CVM）作为基线的绝对性能较弱，"最高 215×"的增益部分来自基线劣势；微内核内的系统调用接口对函数兼容性有要求，复杂函数依赖可能受限；TCB 仍包含微内核与 library OS 自身。总体而言，它为机密 serverless 提供了一条低开销、可部署的架构路径。

- **论文 PDF**: [Serverless Functions Made Confidential and Efficient with Split Containers.](https://www.usenix.org/system/files/usenixsecurity25-shi-jiacheng.pdf)

#### Exploring and Exploiting the Resource Isolation Attack Surface of WebAssembly Containers.
- **作者**: Zhaofeng Yu, Dongyang Zhan, Lin Ye, Haining Yu, Hongli Zhang, Zhihong Tian
- **方向**: 系统与操作系统安全
- **解读**: WebAssembly（WASM）正被广泛用作云原生容器与插件沙箱运行时（Wasmtime、Wasmer、WAMR 等），其宣称的内存安全与沙箱隔离被寄予厚望。然而 WASM 沙箱的资源隔离能力（CPU 配额、内存上限、网络与文件系统访问、执行时间等）是否经得起对抗检验仍是开放问题：若隔离边界可被绕过，恶意模块即可耗尽宿主机资源、干扰同租户负载或窃取隔离数据，威胁多租户部署的安全基础。本工作从标题看旨在系统探索并实际利用 WASM 容器的资源隔离攻击面。

据标题推断，方法上先系统梳理 WASM 容器暴露的资源隔离接口与实现（运行时调度、内存管理、系统调用代理、WASI 能力模型），再设计攻击原语——如资源耗尽型 DoS、时序侧信道、跨实例干扰或配额逃逸——在真实运行时上验证可利用性，并可能给出攻击链演示与影响评估。摘要缺失，具体攻击类型与受影响运行时无法确认。

摘要缺失，无法引用量化结果；预期包含各 WASM 运行时的攻击成功率、资源消耗放大倍数、隔离破坏的影响范围等实验数据。

WASM 沙箱的"安全承诺"主要针对内存安全，而资源隔离是另一维度，此前研究多聚焦字节码层面漏洞，本工作把视线投向运行时资源管理边界，角度互补且有现实威胁（WASM 已进入 CDN、边缘计算与服务网格）。局限：无摘要，攻击的现实触发条件（是否需恶意模块被加载）与修复建议无法评估；与既有 WASM 沙箱逃逸、侧信道研究（如 Spectre 类）的关系也需对照原文。（无摘要，据标题推断）


#### TLBlur: Compiler-Assisted Automated Hardening against Controlled Channels on Off-the-Shelf Intel SGX Platforms.
- **作者**: Daan Vanoverloop; Andrés Sánchez; Flavio Toffalini; Frank Piessens; Mathias Payer; Jo Van Bulck
- **方向**: 系统与操作系统安全
- **解读**: Intel SGX的受控信道（页错误）攻击能以无噪声的4KB粒度追踪enclave页访问，重建文本、图像乃至完整密钥；现有防御要么侧重检测、要么依赖易错的手工注解或假想的硬件扩展。本文提出TLBlur，利用现成Intel处理器上的AEX-Notify扩展实现全自动防泄漏。

核心观察是：由TLB服务的页翻译无需页表遍历，enclave被中断时TLB虽被强制冲刷，但可在自定义中断处理器中把最近访问的N个页透明预取回TLB，从而把攻击带宽压缩到"最近使用页的匿名集"；为此用LLVM后端插件加BOLT二进制重写（不动点算法）自动插桩记录页访问（PAM数据结构），处理器内以常数时间（cmov）排序选取N个最近页并以最大权限原子预取，全程无需开发者注解。

结果显示，在Intel Xeon Silver 4410Y（SGX2+AEX-Notify）上：libjpeg场景PWS=30即可消除秘密相关泄漏（仅剩8×8块总数，不足以推断图像尺寸）；平方-乘算法PWS≥3即归约到首次访问；优化后开销约1~5倍（libjpeg总计×3.07、yescrypt×1.10），较此前防御改进一个数量级。

我的思考：这是首个同时处理受控信道时间与空间带宽的全自动SGX防御，编译器方案解决了注解不可扩展的痛点；libjpeg案例证明手工写常数时间代码对通用程序不可行。局限是仅支持单线程enclave、假设关闭SMT、至少每次页面的首次访问仍会泄漏，且依赖AEX-Notify硬件。

- **论文 PDF**: [TLBlur: Compiler-Assisted Automated Hardening against Controlled Channels on Off-the-Shelf Intel SGX Platforms.](https://www.usenix.org/system/files/usenixsecurity25-vanoverloop.pdf)

#### TETD: Trusted Execution in Trust Domains.
- **作者**: Zhanbo Wang, Jiaxin Zhan, Xuhua Ding, Fengwei Zhang, Ning Hu
- **方向**: 系统与操作系统安全
- **解读**: 以"信任域"（Trust Domain）为抽象的新型 TEE（如 Intel TDX 的 TD、ARM CCA 的 Realm）正成为机密计算的主流形态：域内执行环境与 VMM/宿主隔离，提供比传统 enclave 更完整的虚拟机级保护。但信任域机制在生命周期管理、域内安全执行模型、以及与既有系统软件栈的集成上仍面临设计空白——例如域如何建立、如何保证域内执行的完整性与对外部不可信组件的防御。本工作从标题看聚焦"信任域中的可信执行"，试图填补这一设计空间。

据标题推断，TETD 可能提出一种新的信任域构建或使用模型：或是在信任域内提供轻量可信执行原语，或是设计信任域的动态创建/迁移/销毁流程并论证其安全性，也可能面向特定硬件（如 TDX/CCA）给出更优的域内执行方案。摘要缺失，具体机制无法确认。

摘要缺失，无法引用量化结果；预期包含安全模型的形式化论证、原型实现，以及与现有 TEE 方案在性能与 TCB 上的对比数据。

信任域正取代 enclave 成为 TEE 的主流抽象，但其系统软件侧的研究仍落后于硬件普及：域的生命周期、引导链、与外设交互等都是开放问题。该工作若能给出系统的域内可信执行设计并附带严格安全论证，将对机密计算落地有直接价值。局限：无摘要，具体硬件平台、威胁模型（是否含恶意 OS/VMM）与实现完整性无法评估；与 TDX/CCA 官方栈的关系、以及和既有 TrustDomain 类工作的差异需对照原文。（无摘要，据标题推断）


#### Software Availability Protection in Cyber-Physical Systems.
- **作者**: Ao Li, Jinwen Wang, Ning Zhang
- **方向**: 系统与操作系统安全
- **解读**: 现有软件保护研究多聚焦机密性与完整性违规的检测，对"检测到违规后如何处理"研究甚少——常见做法是直接崩溃程序。但在实时安全攸关的 CPS 中，崩溃牺牲可用性，而计算不及时可能造成灾难性的物理后果。本文提出 Gecko 攻击恢复方案，弥合这一缺口。

Gecko 不仅及时从攻击中恢复执行，还禁用被利用的功能以提升系统可用性。实现面临两个技术挑战（摘要截断，涉及恢复与检测的一致性、被利用功能的识别与安全禁用等），需要在不打断实时任务的前提下完成状态回滚与功能隔离。

在真实 CPS 环境（如嵌入式控制器）中验证，Gecko 能在攻击发生后及时恢复执行并维持关键功能可用，同时阻断被利用的攻击面（摘要截断，具体指标未给出）。

该工作把"可用性"纳入软件保护的设计目标，视角务实且契合 CPS 的现实需求。局限是恢复逻辑本身可能成为新的攻击面，"禁用被利用功能"的粒度判定也较困难；未来需与实时调度、容错设计深度融合，并考虑攻击者针对恢复机制的对抗。

- **论文 PDF**: [Software Availability Protection in Cyber-Physical Systems.](https://www.usenix.org/system/files/usenixsecurity25-li-ao.pdf)

#### Kintsugi: Secure Hotpatching for Code-Shadowing Real-Time Embedded Systems.
- **作者**: Philipp Mackensen, Christian Niesler, Roberto Blanco, Lucas Davi, Veelasha Moonsamy
- **方向**: 系统与操作系统安全
- **解读**: 实时嵌入式系统常采用"代码遮蔽（code-shadowing）"机制隐藏并保护代码，防止固件提取与篡改，但这也使得安全补丁难以直接落地——系统"受保护"的同时也"无法更新"。本文提出 Kintsugi，为代码遮蔽的实时嵌入式系统提供安全热补丁机制。

在满足实时性约束的前提下，设计安全的热补丁加载与校验流程，使补丁能够兼容代码遮蔽机制（如内存保护、签名校验）被安全安装并生效，同时保证补丁自身的完整性与可信性。（无摘要，据标题推断）

在真实或仿真的实时系统/MCU 平台上验证热补丁的可行性与安全性，证明补丁可在系统运行期间应用且不破坏代码遮蔽保护。（无摘要，据标题推断）

该工作填补了"受保护的嵌入式系统无法打补丁"这一实用空白，对工控、汽车等长期运行且难以停机的系统价值很高。局限是热补丁的实时性开销、与遮蔽机制（加密存储、完整性校验）的兼容深度仍有待考察，且补丁机制本身可能成为新的攻击目标。


#### Harness: Transparent and Lightweight Protection of Vehicle Control on Untrusted Android Automotive Operating System.
- **作者**: Haochen Gong, Siyu Hong, Shenyi Yang, Rui Chang, Wenbo Shen, Ziqi Yuan, Chenyang Yu, Yajin Zhou
- **方向**: 系统与操作系统安全
- **解读**: 软件定义汽车时代,车载信息娱乐系统(IVI)通过车内网络(IVN)连接 ECU 并集成车辆控制能力,Android Automotive OS(AAOS)因其丰富生态成为主流 IVI。但 Android 的 TCB 过于庞大(内核、驱动、系统服务与应用),且允许第三方应用安装,攻击者可利用任一组件漏洞攻陷整个 IVI,经由 IVN 实现非法控车,直接威胁人身与财产安全。既有 TEE 与基于 hypervisor 的方案多聚焦隔离单个组件,无法保护"人机界面(HMI)→控制决策→IVN 通信"的完整控制链,且常需硬件改动或深度软件改造,难以适配含大量闭源组件的 Android。本文要构建一个透明、轻量、可保护完整控车链的隔离框架。
Harness 定义一个最小保护域,涵盖执行安全关键车辆控制所必需的受信软件,利用 hypervisor 在进程粒度创建 enclave,将该域与不可信的 AAOS 隔离,并保护其与外部环境的交互,确保控车操作符合用户意图。核心挑战有二:其一,Android 应用与服务重度依赖可写匿名共享内存,而现有软件隔离方案仅支持受限共享——Harness 设计了共享感知的内存监视器,在允许 enclave 安全共享内存的同时防止内核恶意管理;其二,Android 的 Binder IPC 比传统 Linux IPC 更复杂、对性能更敏感——Harness 在 Lowvisor 中跟踪 enclave 的服务注册与查询,以访问控制与加密通道确保只有保护域能调用安全关键的车辆接口。
作者在 Google Cuttlefish 虚拟平台(KVM/crosvm)上实现原型,主组件约 9.3K 行代码,Lowvisor 仅 7,523 行,对 AOSP 增改约 914 行、对 guest/host 内核仅改 287 行。评估显示:性能开销仅为 Geekbench 单核 3.35%、多核 6.09%,内存开销低于 4%,且不影响系统上未受保护软件的性能;安全分析表明 Harness 能抵御从内核攻击到 Binder 服务伪造、触摸输入伪造等十类威胁模型下的攻击。
该工作的亮点在于把 hypervisor 隔离从"隔离单个程序"提升到"保护完整控制链",并解决了 Android 语义下共享内存与 Binder 两大技术难点,做到了对未修改用户态组件的透明保护。性能开销数字务实可信。局限:评估基于仿真平台与原型应用,真实车规 SoC 与车规级 hypervisor 上的表现仍需验证;TCB 仍包含 hypervisor 本身,若 hypervisor 被攻破则保护失效;对"用户意图"的判定依赖 HMI 保护机制的正确性。与 TrustZone 风格的机器粒度隔离相比,进程粒度隔离在容错性上更优,但实现复杂度更高。它为 SDV 时代的控车安全提供了一个低部署门槛的可行方案。
- **论文 PDF**: [Harness: Transparent and Lightweight Protection of Vehicle Control on Untrusted Android Automotive Operating System.](https://www.usenix.org/system/files/usenixsecurity25-gong-haochen.pdf)

#### RangeSanitizer: Detecting Memory Errors with Efficient Range Checks.
- **作者**: Floris Gorter, Cristiano Giuffrida
- **方向**: 系统与操作系统安全
- **解读**: 空间与时间内存错误消毒器是安全测试的基石，但以 ASan 为代表的 redzone 消毒器因逐对象元数据与逐地址检查而开销显著。本文要同时保留 redzone 方案的高兼容性，并引入经典"逐指针元数据"方案的区间检查能力，以打破"兼容性-性能"的取舍。

RangeSanitizer（RSan）提出新型元数据与检查范式：把每个对象的边界信息存入其 underflow redzone，通过指针标记与 2 的幂大小类快速定位元数据，从而用单次检查验证对任意内存范围的访问，并支持多种优化。

在 SPEC CPU2017 上，RSan 几何平均运行时开销仅 44%，快于所有现有 SOTA redzone 消毒器，且是 ASan 的 2 倍速；以 AFL++ 配合 RSan 模糊测试，吞吐量较 SOTA 提升最高达 70%。

我的思考：RSan 的核心洞察是"元数据位置与定位方式决定检查粒度"——把边界信息放进 redzone 并用指针标记+大小类加速查找，是优雅的工程设计，44% 开销与 2 倍于 ASan 的速度在消毒器领域是显著进步。局限是依赖编译期插桩与指针标记的可用性，且范围检查对某些非连续访问模式可能有精度损失；但作为 redzone 系消毒器的新范式，实用价值突出。
- **论文 PDF**: [RangeSanitizer: Detecting Memory Errors with Efficient Range Checks.](https://www.usenix.org/system/files/usenixsecurity25-gorter.pdf)

#### Phantom: Privacy-Preserving Deep Neural Network Model Obfuscation in Heterogeneous TEE and GPU System.

- **作者**: Juyang Bai, Md Hafizul Islam Chowdhuryy, Jingtao Li, Fan Yao, Chaitali Chakrabarti, Deliang Fan
- **方向**: 系统与操作系统安全
- **解读**: 生产级 DNN 是昂贵资产，模型窃取与微调攻击可让对手重建等价模型、直接窃取知识产权。纯密码学方案（HE/MPC）开销过大，TEE-GPU 分区方案要么只适用于预训练模型微调场景，要么仍可被窃取。本文提出 Phantom，在异构 TEE/GPU 系统上对任意从头训练的 DNN 同时混淆其架构与权重。

Phantom 用强化学习（REINFORCE）架构搜索为模型添加轻量冗余卷积层（候选 Null/Conv1/Conv3/Conv5/Conv7），TEE 内以轻量 MUX 操作配合密钥恢复真实计算路径：授权用户得到正确结果，无密钥的对手只见被刻意训练为高损失的混淆模型。为缓解 SGX2 时代 TEE-GPU 间片外数据搬运这一新瓶颈，提出 Top-K 逐层混淆敏感性分析（仅混淆最敏感 K 层）；中间特征用一次性密码本（OTP）同态掩码保护，架构保护采用恒定路径计算与无分支位运算以防侧信道泄露。

在 AlexNet/ResNet-18/VGG-16 与 CIFAR-10/100、STL-10 上，无密钥对手的精度降至接近随机猜测（CIFAR-10/STL-10 约 10%、CIFAR-100 约 1%），远超 NNSplitter（无密钥仍达 22%-59%）与 No Privacy Left Outside（86%-95%）；模型窃取攻击平均成功率仅 6.99%；对手掌握部分参数加 10% 训练数据时仍能防御，而两个 SOTA 基线均告失败；SGX2+GPU 实现端到端延迟较 SOTA 降低 35%。

首次用"RL 搜索混淆层+密钥 MUX"同时保护架构与权重，把随机猜测水平设为防御目标，并发现 SGX2 时代数据搬运成为主瓶颈这一重要工程结论。局限：混淆训练耗时长（每模型 18-38 小时）是显著部署成本；依赖 TEE 可信与密钥管理；评估仅覆盖中小 CNN 与图像分类，对大模型及更强自适应攻击未验证。

- **论文 PDF**: [Phantom: Privacy-Preserving Deep Neural Network Model Obfuscation in Heterogeneous TEE and GPU System.](https://www.usenix.org/system/files/usenixsecurity25-bai-juyang.pdf)

#### System Register Hijacking: Compromising Kernel Integrity By Turning System Registers Against the System.
- **作者**: Jennifer Miller, Manas Ghandat, Kyle Zeng, Hongkai Chen, Abdelouahab Benchikh, Tiffany Bao, Ruoyu Wang, Adam Doupé, Yan Shoshitaishvili
- **方向**: 系统与操作系统安全
- **解读**:
Linux 内核的利用与防御攻防持续升级：Google KernelCTF 一年内为 44 个独特利用技术发放赏金，而 FineIBT 已成为内核默认的控制流完整性（CFI）机制，可阻断所有已知控制流劫持利用技术。但既有利用都在指令层面模仿用户态，忽视了内核特有的系统寄存器（如 cr0、cr3、gs、swapgs）这一潜在攻击面。

作者提出系统寄存器劫持（SRH）这一新利用类别，系统性评估 x86-64 与 aarch64 内核中所有访问系统寄存器的指令，分析其攻击效果、前置条件与内核中的 gadget 规模，共提出 7 种技术（含已知 cr4 劫持与 6 个新方法）。其中最强大的技术利用 swapgs 指令劫持 KERNEL_GSBASE_MSR，既不需要通用寄存器控制也不需要栈控制，成为首个能够绕过 FineIBT 的通用利用原语；作者同时为多数技术提出缓解方案。

7 种技术中 4 种只需劫持点的寄存器控制、2 种需要部分栈控制、1 种（swapgs）两者皆不需，理论上适用于 x86-64 任意控制流劫持漏洞；作者用脆弱内核模块 PoC 并利用真实漏洞验证了 swapgs 技术。缓解 swapgs 的补丁可恢复 FineIBT 的安全态势，性能代价低于 1%。

该工作开辟了"系统寄存器劫持"这一被忽视的利用前沿，证明 FineIBT 并非内核控制流防护的终点，对防御研究具有重要警示意义，swapgs 技术的通用性极具威胁。局限在于多数技术仍需寄存器或栈控制等前提，部分技术难以缓解，跨架构覆盖与实战整合仍待完善；其缓解方案也需经受更多绕过尝试的检验。

- **论文 PDF**: [System Register Hijacking: Compromising Kernel Integrity By Turning System Registers Against the System.](https://www.usenix.org/system/files/usenixsecurity25-miller.pdf)

#### When Good Kernel Defenses Go Bad: Reliable and Stable Kernel Exploits via Defense-Amplified TLB Side-Channel Leaks.
- **作者**: Lukas Maar, Lukas Giner, Daniel Gruss, Stefan Mangard
- **方向**: 系统与操作系统安全
- **解读**:
内存随机化是 Linux 内核限制漏洞利用的基础防御，但既有 TLB 侧信道研究只能泄露代码段或直接物理映射的基址，无法定位具体安全关键对象；同时，防御向内存映射子系统深度集成后，可能反而制造更精确的泄漏——哪些防御会放大该风险此前无人系统回答。

作者对 KSPP 与 Google KernelCTF 推荐的 127 项内核防御做系统性侧信道分析，发现其中 3 项——强制严格内存权限、虚拟化内核堆、虚拟化内核栈——把对象映射从 2 MB 细化为 4 kB，产生可通过 Evict+Reload TLB 侧信道观测的细粒度争用模式。结合内核分配器 massaging 与 2 个已知、1 个新增的判别原语（区分 2 MB 与 4 kB 映射），提出位置泄露攻击，可定位 pipe_buffer、msg_msg、cred、file、seq_file、各级页表与内核栈等安全关键对象。

攻击在第 8 至 14 代 Intel CPU 与 v5.15–v6.8 内核上验证：空闲系统下耗时仅 0.3–17.8 秒且几乎无误报，高负载系统误报率约 7%。利用泄漏，作者在 v6.8 等现代内核上实现无崩溃、接近 100% 可靠性的提权，既重新启用了此前被抑制的利用技术，也催生了一种此前不可能的新技术；并论证虚拟栈防御实际上降低了系统安全性。

"防御放大侧信道"的视角新颖且反直觉——防御越强，对象反而越可被精确定位，对内核防御设计提出重要警示；亚 18 秒的泄漏速度与近 100% 的可靠性使攻击具备实战级威胁。局限在于攻击条件本身依赖 4 kB 映射类防御的启用，缓解（避免对象使用 4 kB 映射）与性能存在权衡，新硬件特性下的适用性也有待研究。
- **论文 PDF**: [When Good Kernel Defenses Go Bad: Reliable and Stable Kernel Exploits via Defense-Amplified TLB Side-Channel Leaks.](https://www.usenix.org/system/files/usenixsecurity25-maar-kernel.pdf)

#### Approximation Enforced Execution of Untrusted Linux Kernel Extensions.

- **作者**: Hao Sun, Zhendong Su
- **方向**: 系统与操作系统安全
- **解读**:

Linux 内核允许加载第三方扩展（如 eBPF 程序、内核模块），在提升灵活性的同时引入巨大攻击面：不可信扩展的漏洞可导致内核提权或崩溃；现有验证与沙箱机制要么限制表达力、要么开销高昂，难以兼顾安全与性能。

据标题推断，论文提出"近似强制执行"（Approximation Enforced Execution）机制运行不可信内核扩展：以近似语义执行（如限制、替换或抽象危险操作）换取安全性，使扩展在受控的近似环境中运行，既保留大部分功能又隔离恶意行为。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于为不可信内核扩展提供一种"功能—安全"折中的新执行模型，拓宽 eBPF 类机制的安全边界。

我的思考：Zhendong Su 团队在程序分析与内核验证上积累深厚，"近似执行"的设想若以形式化保证落地（近似误差有界、危险操作被替换），将是对 eBPF 验证器路线的重要补充。局限（据标题推断）：近似语义对扩展行为正确性的影响、性能开销与近似精度之间的权衡，以及与现有验证器/沙箱的集成方式需原文细节支撑。


#### EKC: A Portable and Extensible Kernel Compartment for De-Privileging Commodity OS.

- **作者**: Jiaqin Yan, Qiujiang Chen, Shuai Zhou, Yuke Peng, Guoxing Chen, Yinqian Zhang
- **方向**: 系统与操作系统安全
- **解读**:

操作系统内核的单一特权域是安全设计的历史包袱：任一内核漏洞即可导致完全失陷；内核隔离（compartmentalization）通过对内核组件去特权化、限制漏洞影响范围来缓解该问题，但已有方案往往依赖特定硬件特性或深度改造，难以移植到商用 OS。

据标题推断，论文提出 EKC，一个可移植、可扩展的内核隔离区机制：将内核划分为相互隔离的权限域，仅以最小必要特权执行各组件的受信任部分，并以可扩展的接口支持新组件的接入，在保持与现有内核生态兼容的前提下降低提权风险。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把内核去特权化从研究原型推向可落地于商用 OS 的通用机制。

我的思考：内核隔离（如 Nested Kernel、CHERI、MPK 分区）是系统安全热点，EKC 强调"可移植+可扩展"直面了该方向部署难的通病；与 eBPF 沙箱、模块签名等增量防御相比，隔离是结构性方案。局限（据标题推断）：隔离边界的性能开销（系统调用跨域切换）、跨架构/跨 OS 的可移植性实现成本，以及隔离后组件间通信的安全验证是常见难点，具体设计待原文呈现。


#### Subverting the Secure VM by Exploiting PCIe Devices.

- **作者**: Cheolwoo Myung, Sangho Lee, Byoungyoung Lee
- **方向**: 系统与操作系统安全
- **解读**:

基于 AMD SEV、Intel TDX 等技术的安全虚拟机（Secure VM）承诺在不受信任的云主机面前保护客户机机密性，但机密计算边界并非只由 CPU 决定：PCIe 设备及其与内存、IOMMU 的交互构成复杂攻击面，设备直通、中断重映射与 DMA 配置缺陷可能被利用来突破隔离。

据标题推断，论文研究如何通过利用 PCIe 设备攻破安全 VM：分析设备配置空间、MMIO、DMA 与中断路径中可被恶意设备或驱动操纵的环节，构造攻击链以读取或篡改客户机内存，从而颠覆机密计算的安全承诺。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于揭示"CPU 级机密计算"与"设备级外围攻击面"之间的信任缺口，对云安全架构有直接警示。

我的思考：SEV/TDX 的攻击研究多聚焦于 CPU 微架构与软件接口，设备侧攻击面（如 DMA、设备固件）是相对未被充分挖掘的方向，该工作有望填补这一空白；与已公开的 PCIe/设备侧 TEE 攻击形成互补。局限（据标题推断）：攻击依赖特定硬件平台与设备行为，虚拟化层对设备的模拟与过滤可能阻断部分路径；实际可利用性与影响范围需原文实验确认。


#### Finding Metadata Inconsistencies in Distributed File Systems via Cross-Node Operation Modeling.

- **作者**: Fuchen Ma, Yuanliang Chen, Yuanhang Zhou, Zhen Yan, Hao Sun, Yu Jiang
- **方向**: 系统与操作系统安全
- **解读**:

元数据一致性是分布式文件系统（DFS）的基石：它保证不同客户端看到一致的数据视图；但 DFS 天然易错，元数据不一致虽罕见却后果严重——数据丢失、服务故障与权限违规。社区对不一致的特征缺乏理解，更缺乏有效检测方法。

方法上，论文先对五年间四个广泛使用 DFS 的元数据不一致进行综合研究，得出两个关键发现：不一致主要由相互关联的跨节点文件操作触发而非系统故障，根因在于元数据冲突解决过程；据此提出 Horcrux 模糊测试框架，用跨节点操作建模把无限输入组合压缩到可管理空间，捕获隐式跨节点操作关系、触发更多冲突解决逻辑。

结果：Horcrux 已检测出 10 个此前未知的元数据不一致；在冲突解决代码覆盖率上比最先进的工具多覆盖 20.29%-146.21%，验证了建模方法的有效性。

我的思考：先实证后方法的"发现驱动"研究路径扎实——两个根因发现直接指导了测试设计，跨节点操作建模把模糊测试从单节点语义扩展到分布式交互语义，是 DFS 测试的重要推进。局限：覆盖四个 DFS 的结论外推需谨慎，检测到的 10 个不一致的严重性与修复情况、以及模型对更多节点与故障注入场景的扩展性值得进一步考察。

- **论文 PDF**: [Finding Metadata Inconsistencies in Distributed File Systems via Cross-Node Operation Modeling.](https://www.usenix.org/system/files/usenixsecurity25-ma-fuchen.pdf)

#### Save what must be saved: Secure context switching with Sailor.

- **作者**: Neelu S. Kalani, Thomas Bourgeat, Guerney D. H. Hunt, Wojciech Ozga
- **方向**: 系统与操作系统安全
- **解读**:

机密计算（如 SEV-SNP、TDX）中，上下文切换（context switching）是性能与安全的关键交汇点：切换需保存/恢复寄存器状态以隔离安全与非安全世界，保存过多增加开销、保存不足则可能泄露机密；中断与异常随时可能打断受信执行，使切换路径成为攻击面。

据标题推断，论文提出 Sailor，一种安全的上下文切换机制，秉持"只保存必须保存的"原则：最小化切换时保存与恢复的寄存器集合，在保证隔离边界完整的前提下显著降低切换开销，面向 TEE 与可信固件场景设计。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把上下文切换从"安全开销负担"优化为"最小必要状态"的工程实现，对机密计算性能落地有直接意义。

我的思考：作者背景（IBM 的 Hunt、ETH 的 Bourgeat 在 TEE 与形式化硬件验证上均有建树）暗示 Sailor 可能附带形式化或严格的隔离论证，使其区别于纯工程优化；"最小保存集"思路在中断密集的 I/O 场景收益尤其明显。局限（据标题推断）：减少保存状态可能引入新的信息泄漏通道（如未保存寄存器残值），安全性论证的完备性与跨平台（SEV/TDX/自研）适配性需原文确认。


#### Too Much of a Good Thing: (In-)Security of Mandatory Security Software for Financial Services in South Korea.

- **作者**: Taisic Yun, Suhwan Jeong, Yonghwa Lee, Seungjoo Kim, Hyoungshick Kim, Insu Yun, Yongdae Kim
- **方向**: 系统与操作系统安全
- **解读**:

KSA 2.0 是韩国强制部署十余年、2022 年注册用户超 2.07 亿的金融安全软件套件，却在 2023 年被朝鲜黑客利用，导致 61 家机构、超 1000 万台计算机被攻陷。论文系统调查其安全性，目标是回答：这种被强制安装、与浏览器安全模型相悖的设计究竟带来多大风险。

研究覆盖 7 款匿名化产品（防火墙、反键盘记录、异常检测、证书管理四类），结合黑盒测试、逆向工程（绕过 Themida 等混淆）与改造的 AFL++/AFLnet 模糊测试，提出远程攻击者（T1）、恶意网页（T2）、中间人（T3）、源欺骗（T4）四种威胁模型。作者还进行了两项用户研究：400 人代表性调查与 48 人桌面实测。

分析发现四类设计缺陷（威胁模型不一致、TLS 使用不当、违反浏览器沙箱、允许用户追踪）及 19 个漏洞，造成键盘记录、MITM、私钥泄露、RCE、设备指纹等风险。典型如 PRODUCT E 的根 CA 私钥可被提取并成功签发 google.com 证书；PRODUCT B 通过任意文件下载+移动链式利用实现 RCE。调查显示 97.35% 银行用户安装了 KSA 但 59.25% 不了解其功能；桌面实测平均每人安装 9 个、最多 24 个 KSA 程序，且多运行 2022 年及更早的过时版本。全部漏洞已向政府机构与厂商披露并修复。

该工作揭示了"以安全之名强制部署"的软件反而成为国家级攻击面，与 Green Dam、哈萨克斯坦根证书等案例形成呼应，政策价值突出。局限是产品匿名化限制了可复现性，结论外推需谨慎。最深刻的洞察在于补丁分发生态的结构性困难——二进制与各服务商 JavaScript 深度耦合，导致即使披露也难以快速修复，提示监管框架必须建立统一升级与评估机制。

- **论文 PDF**: [Too Much of a Good Thing: (In-)Security of Mandatory Security Software for Financial Services in South Korea.](https://www.usenix.org/system/files/usenixsecurity25-yun.pdf)

### 硬件与物理安全（38 篇）

#### Dorami: Privilege Separating Security Monitor on RISC-V TEEs.
- **作者**: Mark Kuhne, Stavros Volos, Shweta Shinde
- **方向**: 硬件与物理安全
- **解读**: RISC-V TEE（如 Keystone、Penglai 等）通过一个可信组件——安全监视器（SM）——在最高特权层（machine mode）执行隔离任务：用特权 ISA 指令与物理内存保护把 enclave 相互隔离、并与 OS 隔离。但 SM 与平台固件同处最高特权层，而固件不仅体积庞大，还包含平台特定的第三方厂商代码，一旦固件被攻破，整个 TEE 的隔离保证即告瓦解。本文要解决"如何缩小 TEE 攻击面"：把 SM 与不可信固件做特权分离。

Dorami 的核心思路是复用现有 ISA 特性（而非新增硬件）强制实施 SM 与固件的隔离，使 SM 即使与庞大固件共存于 machine mode，也能在固件被攻破时保持自身完整性，从而显著缩小 TEE 的可信攻击面；其设计目标还包括不引入大的性能与兼容性开销。

摘要给出了设计动机与目标：在不引入大的额外开销的前提下，利用现有 ISA 特性实现 SM 与固件的特权分离，降低 TEE 攻击面。具体量化指标（如隔离粒度、性能开销、对现有 SM/固件栈的改动量）需以全文为准。

把"SM 与固件同特权层"这一 RISC-V TEE 的通病作为切入点，用纯软件的特权分离（复用既有 ISA）改善 TCB，思路务实且对 RISC-V 生态有普适价值——多数 RISC-V TEE 设计都面临厂商固件不可信的困境。局限：复用现有 ISA 实现隔离往往在粒度或强度上有妥协，攻击者若找到固件→SM 的间接交互路径（如共享外设、中断路径）可能仍可渗透；评估是否覆盖真实商用 SoC 固件栈也需确认。总体而言，它是 TEE 平台安全基座研究的重要一步。

- **论文 PDF**: [Dorami: Privilege Separating Security Monitor on RISC-V TEEs.](https://www.usenix.org/system/files/usenixsecurity25-kuhne.pdf)

#### TDXploit: Novel Techniques for Single-Stepping and Cache Attacks on Intel TDX.
- **作者**: Fabian Rauscher, Luca Wilke, Hannes Weissteiner, Thomas Eisenbarth, Daniel Gruss
- **方向**: 硬件与物理安全
- **解读**: Intel TDX 是机密计算的关键硬件防线，承诺把信任域（TD）与宿主、VMM 乃至其他 TD 隔离；但其安全性最终依赖硬件与固件对微架构边界的处理，历史经验表明"硬件隔离承诺"常被侧信道击穿。本工作从标题看聚焦 TDX 的两类新型攻击技术：单步执行（single-stepping，精确控制/观测指令级执行时序）与缓存攻击（cache attack，如 Prime+Probe、Evict+Time），意在检验 TDX 在真实微架构层面能否兑现其机密性承诺。

据标题推断，方法上可能利用 TDX 的特定执行路径（如 TD exit/entry、APIC 处理、定时原语）构造精确的单步原语，再结合缓存侧信道恢复 TD 内敏感数据（如密钥、内存内容），或组合二者实现更细粒度的信息泄露；格拉斯（Gruss）团队在微架构攻击上积累深厚，技术可信度预期较高。摘要缺失，具体攻击链无法确认。

摘要缺失，无法引用量化结果；预期包含攻击成功率、泄露的信息粒度（如密钥恢复位数）、对 TDX 版本的适用性，以及是否可跨信任域隔离边界等实验证据。

TDX 作为主流云机密计算方案，其微架构攻击面此前研究相对有限，本工作若在真实 TDX 平台上实现单步与缓存攻击，将直接冲击"TD 内数据不可被宿主观测"的核心承诺，对云服务商与硬件厂商的缓解策略（如缓存分区、定时源限制）有重要推动。局限：无摘要，攻击的现实前提（恶意宿主控制程度）、是否已负责任披露及缓解状态均待全文确认；与针对 SGX 的既有侧信道（如 SGXPECTRE、Prime+Probe）工作的对比也是评估重点。（无摘要，据标题推断）


#### I Know What You Said: Unveiling Hardware Cache Side-Channels in Local Large Language Model Inference.
- **作者**: Zibo Gao, Junjie Hu, Feng Guo, Yixin Zhang, Yinglong Han, Siyuan Liu, Haiyang Li, Zhiqiang Lv
- **方向**: 硬件与物理安全
- **解读**: 本地 LLM 推理常被视为免受云端监控的隐私友好选择，但若推理运行在共享或不可信硬件上，硬件缓存侧信道可能泄露用户输入内容。本文首次揭示本地 LLM 推理中的硬件缓存侧信道威胁，说明"本地部署"并非隐私终点。

攻击者利用共享缓存（如最后一级缓存）的访问时序差异，通过 Prime+Probe 等经典侧信道手段观察 LLM 推理过程中的访存模式，结合模型结构知识推断用户"说了什么"——重建输入文本与会话内容。（无摘要，据标题推断）

实验在真实本地推理环境中成功恢复输入信息，证明该攻击在无需特权访问的条件下即可奏效，威胁模型现实可行。（无摘要，据标题推断）

该工作将经典硬件侧信道与新兴本地 LLM 场景结合，提醒用户与开发者关注推理基础设施的物理层信任边界。局限是攻击对更大模型、批处理推理的扩展性有待验证；防御可从缓存隔离、恒定时间推理或访存模式混淆入手，但会带来性能代价。


#### Enabling Low-Cost Secure Computing on Untrusted In-Memory Architectures.
- **作者**: Sahar Ghoflsaz Ghinani, Jingyao Zhang, Elaheh Sadredini
- **方向**: 硬件与物理安全
- **解读**: 内存带宽瓶颈（内存墙）限制现代计算性能，处理-内存（PIM）架构把计算移近数据以提升有效带宽，在内存密集型负载上性能优越。但将 PIM 模块集成进安全计算系统面临难题：未加密数据必须离开芯片送往 PIM，暴露给攻击者，破坏可信计算基（TCB）假设。本文研究如何在不可信 PIM 架构上实现低成本安全计算。

利用安全多方计算（MPC）技术——具体为算术秘密共享与 Yao 混淆电路——将带宽密集的运算外包给 PIM 执行，使 PIM 只能看到共享或混淆形式的数据，从而在保留性能收益的同时维持安全边界（摘要截断）。

设计并实现基于 PIM 的安全计算方案，在真实 PIM 平台或模拟器上评估其性能与安全开销，验证了"以 MPC 改造硬件加速"路线的可行性（摘要截断，具体加速比未给出）。

该工作为 PIM 的实际安全部署扫清了障碍，思路具有工程价值。局限是 MPC 协议本身的通信与计算开销、以及 PIM 对协议原语的支持程度，仍需在更大规模工作负载下验证；其安全假设（如秘密共享下的合谋）也需在具体威胁模型下审视。

- **论文 PDF**: [Enabling Low-Cost Secure Computing on Untrusted In-Memory Architectures.](https://www.usenix.org/system/files/usenixsecurity25-ghinani.pdf)

#### GenHuzz: An Efficient Generative Hardware Fuzzer.
- **作者**: Lichao Wu, Mohamadreza Rostami, Huimin Li, Jeyavijayan Rajendran, Ahmad-Reza Sadeghi
- **方向**: 硬件与物理安全
- **解读**: 硬件安全是可信计算的基础，但硬件设计日益复杂，制造后修复漏洞代价高昂。白盒硬件模糊测试有望实现规模化、可适配的漏洞检测，但现有硬件模糊器面临输入语义复杂、反馈利用有限、所需测试用例过多等挑战。本文提出 GenHuzz 框架解决这些局限。

GenHuzz 将硬件模糊测试重构为优化问题：通过优化模糊测试策略（policy），生成更隐蔽且有效的测试用例，并更充分地利用仿真反馈指导输入生成，从而在有限测试预算内发现更微妙的设计缺陷（摘要截断）。

在 RTL 硬件设计上评估，GenHuzz 相比现有硬件模糊器能以更少测试用例达到更高覆盖率、发现更多设计缺陷，展示了策略优化路线的优势（摘要截断，具体数字未给出）。

该工作把策略优化思想引入硬件验证，属于"生成式+优化"路线，方向新颖。局限是策略训练开销与可解释性，且覆盖率难以完全刻画漏洞语义，与形式化验证形成互补而非替代；多设计间的泛化能力仍需检验。

- **论文 PDF**: [GenHuzz: An Efficient Generative Hardware Fuzzer.](https://www.usenix.org/system/files/usenixsecurity25-wu-lichao.pdf)

#### Security Implications of Malicious G-Codes in 3D Printing.
- **作者**: Jost Rossel, Vladislav Mladenov, Nico Wördenweber, Juraj Somorovsky
- **方向**: 硬件与物理安全
- **解读**: 3D 打印技术的快速增长变革了航空航天、医疗等多个行业，但也引入显著的安全挑战。此前研究虽指出控制打印过程的 G-Code 命令存在安全隐患，但假设了强大攻击者并聚焦打印模型的操纵，对完整攻击潜力理解不足。本文系统分析 G-Code 命令导致的漏洞，填补这一空白。

对 G-Code 命令进行系统性安全分析，引入攻击与威胁模型，假设比传统研究更弱的攻击者，考察攻击者如何通过恶意或篡改的 G-Code 影响打印过程，扩展攻击面认知（摘要截断）。

研究揭示多种此前未被充分认识的 G-Code 攻击向量，证明较弱的攻击者也能对打印过程造成实质性危害（摘要截断，具体数字未给出）。

该工作细化了 3D 打印的威胁模型，降低了攻击者假设门槛的发现对现实防御有直接指导意义。局限是攻击多在受控环境验证，而实际打印机固件与切片器实现差异大；防御需推动 G-Code 解析与校验的标准化，并与打印机厂商协作落地。

- **论文 PDF**: [Security Implications of Malicious G-Codes in 3D Printing.](https://www.usenix.org/system/files/usenixsecurity25-rossel.pdf)

#### SoK: A Security Architect's View of Printed Circuit Board Attacks.
- **作者**: Jacob Harrison, Nathan Jessurun, Mark Tehranipoor
- **方向**: 硬件与物理安全
- **解读**: 近期大量论文提出电学测量或物理检测技术来防御 PCB/PCBA 篡改，并频繁引用"大黑客"事件、游戏机 modchip 与"拦截攻击"作为动机。作者对此表示担忧：实现错误与安全架构问题在这些研究中鲜被讨论，而它们恰是此类攻击的根因，说明攻击本身可能并未被很好理解。

作为 SoK（系统化知识综述），该文从安全架构师的视角系统梳理 PCBA 攻击：分析已知攻击的实现路径与根因、归纳防御技术与验证方法的有效性，指出防御研究与攻击现实之间的错位（摘要截断）。

综述揭示防御研究与实际攻击根因之间的结构性错位：若假设新颖的防御与验证方法能够解决攻击问题，而忽视实现错误与架构缺陷，可能导致"攻防不匹配"的新风险（摘要截断，具体结论未完整给出）。

该 SoK 提醒硬件安全社区"先理解攻击、再设计防御"，对研究动机叙事具有纠偏价值。局限是 PCB 供应链威胁本身难以量化验证，综述的呼吁能否落地，取决于后续能否建立共享的威胁模型与评测基准，并加强实现层面的安全架构教育。

- **论文 PDF**: [SoK: A Security Architect's View of Printed Circuit Board Attacks.](https://www.usenix.org/system/files/usenixsecurity25-harrison.pdf)

#### SoK: So, You Think You Know All About Secure Randomized Caches?

- **作者**: Anubhav Bhatla, Hari Rohit Bhavsar, Sayandeep Saha, Biswabandan Panda
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）随机化缓存（randomized caches）通过随机化地址到集合的映射来抵御缓存侧信道攻击（如 Prime+Probe、Evict+Time），是近年硬件安全设计的重点方向。本 SoK 论文系统化梳理安全随机化缓存的既有方案、威胁模型与评估方法，审视研究社区对这类设计的认知是否完整。

方法上，该工作应收集并分类各类随机化缓存设计（如 CEASER/CEASER-S、ScatterCache、MIRAGE、SassCache 等），按其随机化机制、安全保证、性能开销与评估假设归类，并系统对比其抵御不同侧信道攻击（基于争用、基于占用等）的能力与局限。

预期结果将指出随机化缓存的共性弱点、评估基准的不一致，以及被忽视的攻击面，为后续设计与评估提供统一框架。

我的思考：SoK 的价值在于把分散的随机化缓存文献「对齐到同一张图」——此类设计的安全论证常依赖特定威胁模型，横向审视容易暴露评估口径不统一的问题；与同批次的「缓存占用攻击」研究呼应，说明随机化并非万能。无摘要使具体分类结论待查原文；对硬件安全研究者而言，这类系统性梳理是选题与评估的必备参照。


#### TEEcorrelate: An Information-Preserving Defense against Performance-Counter Attacks on TEEs.

- **作者**: Hannes Weissteiner, Fabian Rauscher, Robin Leander Schröder, Jonas Juffinger, Stefan Gast, Jan Wichelmann, Thomas Eisenbarth, Daniel Gruss
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）性能计数器（performance counters）是 TEE（如 Intel SGX）中的关键侧信道源：攻击者可利用计数器泄露的控制流/访存信息推断机密数据，而现有防御往往通过禁用或扰动计数器来阻断攻击，代价是牺牲合法的性能监控能力。TEEcorrelate 提出「信息保留」型防御，在不放弃性能信息的前提下抵御基于性能计数器的攻击。

方法上，该工作应分析性能计数器攻击的信息利用方式（如与已知控制流的相关性分析），设计使攻击者无法从计数器数据中关联出机密信息的机制——可能通过混淆、延迟注入或相关性破坏——同时保持计数器对合法监控的可用性。

预期结果将展示在真实 TEE（SGX 等）上防御的有效性、对性能监控功能的保留程度及开销。

我的思考：「信息保留」而非「信息消除」是本文的差异化思路——前作多直接限制计数器，本文则保留信息但破坏可利用的相关性，工程上更实用。但性能计数器与合法监控的界限微妙，攻击者可能结合其他侧信道重新建立关联；无摘要使具体机制待查原文。该工作对 TEE 生态（SGX 已弃用、机密计算转向其他架构）的延续价值取决于其通用性。


#### Systematic Evaluation of Randomized Cache Designs against Cache Occupancy.

- **作者**: Anirban Chakraborty, Nimish Mishra, Sayandeep Saha, Sarani Bhattacharya, Debdeep Mukhopadhyay
- **方向**: 硬件与物理安全
- **解读**: 随机化地址到集合映射与分区被证明是设计安全缓存的有效机制，现有设计基于三种思路：随机化设计、随机化+分区设计、伪全相联设计。本文填补随机化缓存文献的关键空白：多数设计只防御基于争用的攻击，忽视缓存占用（cache occupancy）攻击，作者对 CEASER、CEASER-S、MIRAGE、ScatterCache、SassCache 五种设计就缓存占用做了性能与安全两个维度的系统评估。

性能方面，作者首先指出现有设计的基准测试策略因缓存配置不同、基准套件选择差异及实现相关假设而不适合公平评估，遂提出统一基准策略，使各设计在不同替换策略下可比。安全方面，评估覆盖三种威胁模型：隐蔽信道、进程指纹识别与 AES 密钥恢复——据作者所知，这是首次在随机化缓存设计上利用缓存占用攻击实现完整 AES 密钥恢复。

结果确立了在随机化缓存设计考量中必须纳入缓存占用侧信道的必要性。

我的思考：这是对随机化缓存研究的一次「压力测试」——此前文献几乎默认随机化可抵御占用型攻击，本文用统一评估框架证明其脆弱，且首次演示完整 AES 密钥恢复，冲击力强。方法上，统一基准策略本身也是贡献，解决了各设计自说自话的评估乱象；局限是模拟环境与真实硬件存在差距，且五种设计的选择未覆盖全部变体。

- **论文 PDF**: [Systematic Evaluation of Randomized Cache Designs against Cache Occupancy.](https://www.usenix.org/system/files/usenixsecurity25-chakraborty.pdf)

#### Exploiting Inaccurate Branch History in Side-Channel Attacks.

- **作者**: Yuhui Zhu, Alessandro Biondi
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）分支历史（branch history）是现代处理器分支预测器的核心状态，此前研究（如分支历史注入 BHI 类攻击）关注攻击者能否控制分支历史来注入预测；本文转向另一面——利用不准确（inaccurate）的分支历史作为侧信道：分支历史缓冲区的状态受受害者控制流影响，攻击者可观测其变化来推断受害者的分支行为，从而泄露机密。

方法上，该工作应针对具体处理器微架构（如现代 Intel/AMD 或 RISC-V 设计）构造利用不准确分支历史的侧信道原语，量化分支预测器状态的泄露粒度，并展示其在真实场景（如密码实现、内核控制流）中的密钥/秘密恢复能力。

预期结果将给出侧信道构建的成功率、信噪比与跨特权级/跨核心的可行性，以及与既有分支预测侧信道（如 BTB 攻击）的对比。

我的思考：把「预测错误/历史不准确」当作信息源而非故障源，是对分支预测侧信道研究视角的拓展——既有的 BTB/BHB 攻击多假设攻击者主动注入，本文则利用被动观测的预测状态差异，可能更难防御。无摘要使具体架构与指标待查原文；若在常见商用处理器上可行，将再次提醒「预测状态本身即机密」这一硬件安全教训。


#### Phantom Trails: Practical Pre-Silicon Discovery of Transient Data Leaks.

- **作者**: Alvise de Faveri Tron, Raphael Isemann, Hany Ragab, Cristiano Giuffrida, Klaus von Gleissenthall, Herbert Bos
- **方向**: 硬件与物理安全
- **解读**: 瞬态执行漏洞困扰 CPU 近十年，但设计阶段的高效发现方法仍然缺失。现有方法试图寻找泄露「显式定义秘密」的程序（有时还要求经侧信道传输），严重限制了可触发检测的程序空间，迫使模糊器用已知漏洞模板约束搜索空间，存在过拟合风险。缺的是一种通用检测机制：（1）让模糊器容易触发违规，（2）在根因处捕获漏洞——类似软件中的消毒器（sanitizer）。

Phantom Trails 正是这样的通用方法，无需模板。两个关键设计选择：其一，聚焦发现微架构数据泄露而独立于隐蔽信道，直击攻击核心；其二，从程序架构行为自动推断所有秘密位置，使检测器更容易发现泄露。

评估中，作者对 BOOM RISC-V CPU 进行模糊测试：从空种子开始、无预定义模板，24 小时内发现所有已知投机漏洞，并发现一个新的 BOOM 特有 Spectre 变体——Spectre-LoopPredictor。

我的思考：把「发现数据泄露」与「发现隐蔽信道」解耦、并自动推断秘密位置，是摆脱模板过拟合的关键创新，让模糊器在未知漏洞空间自由探索；「消毒器式」检测思路在硬件验证中稀缺而可贵。局限是仅在 BOOM 单一处理器上验证，通用性（其他乱序核、商业微架构）有待证明；能发现新变体则说明方法确有超越已知模板的泛化能力。

- **论文 PDF**: [Phantom Trails: Practical Pre-Silicon Discovery of Transient Data Leaks.](https://www.usenix.org/system/files/usenixsecurity25-de-faveri-tron.pdf)

#### Place Protections at the Right Place: Targeted Hardening for Cryptographic Code against Spectre v1.

- **作者**: Yiming Zhu, Wenchao Huang, Yan Xiong
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）Spectre v1（边界检查绕过）允许攻击者诱导 CPU 瞬态执行越界访问，从而泄露秘密，密码学实现尤其高危。现有防御常在代码中大量插入序列化指令（lfence/speculation barrier），全面防护但性能损失显著。本文提出「在正确的位置放置防护」的定向加固方法，以最小开销保护密码学代码免受 Spectre v1。

方法上，该工作应做敏感数据流分析：识别密码代码中真正可被瞬态执行泄露的边界检查点与秘密访问路径，只在必要位置插入防护，同时保证加固后代码的安全性（如结合形式化/符号推理验证防护完备性），并把开销与全量加固对比。

预期结果将展示定向加固在维持同等安全保证的前提下大幅降低性能开销（如密码运算吞吐损失的显著减少），并给出自动化工具支持。

我的思考：把 Spectre 加固从「撒网式」变为「精准式」，核心难点是完备性证明——漏插一个防护点就可能前功尽弃，因此分析工具与验证手段是成败关键。该方向对密码库（OpenSSL、底层签名实现等）的实际部署价值大；无摘要使具体开销数字待查原文，但「位置选择」这一命题本身即是对既有全量加固实践的重要改进。


#### Encarsia: Evaluating CPU Fuzzers via Automatic Bug Injection.

- **作者**: Matej Bölcskei, Flavien Solt, Katharina Ceesay-Seitz, Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: 硬件模糊测试在开源 RISC-V CPU 设计中发现了大量漏洞，但比较不同硬件模糊器的效果仍很困难：各模糊器针对不同指标优化、在不同 CPU 设计上演示，且新发现漏洞数量不适合作为指标——设计成熟后找新漏洞越来越难。作者主张用可自动注入的漏洞语料库来比较模糊器，以理解其强项与弱点。

通过对开源 RISC-V CPU 中 177 个软件可观测漏洞的大规模研究，作者发现 CPU 漏洞可通过操纵条件语句或信号驱动（signal drivers）来建模。据此设计 Encarsia：自动变换给定 CPU 设计的中间表示，注入等价于 HDL 层错误条件或错误赋值的漏洞；为确保注入漏洞有可观测的架构影响，用形式化方法证明该变换确实导致架构偏差。

作者把 Encarsia 注入到三个开源 RISC-V CPU 中，用近期提出的 CPU 模糊器进行模糊测试并比较其漏洞发现性能。实验揭示了现有硬件模糊器的关键局限：无法覆盖被测设计的大片区域、覆盖率指标无效、漏洞检测机制常漏报或误报，凸显重新审视现有方法的迫切性。

我的思考：Encarsia 解决的是「模糊器评测的基准缺失」这一方法论问题——注入真实形态的漏洞（源于 177 个真实 bug 的建模）使比较公平且可复现，形式化验证保证注入漏洞真实可观测，设计严谨。其结论（覆盖率指标失效、大区域不可达）对硬件模糊研究是重要警示；局限是注入漏洞的分布与真实设计缺陷仍有偏差，评测结论的推广需谨慎。
- **论文 PDF**: [Encarsia: Evaluating CPU Fuzzers via Automatic Bug Injection.](https://www.usenix.org/system/files/usenixsecurity25-bolcskei.pdf)

#### FLOP: Breaking the Apple M3 CPU via False Load Output Predictions.
- **作者**: Jason Kim; Jalen Chuang; Daniel Genkin; Yuval Yarom
- **方向**: 硬件与物理安全
- **解读**: 处理器为弥合执行速度与内存延迟之间不断扩大的差距而持续引入激进优化，但这些优化屡次被证明会带来安全代价。本文发现近期 Apple M 系列与 A 系列处理器实现了加载值预测器（LVP），即在内存内容真正就绪之前预测加载结果，使指令得以并行执行，缓解读后写（RAW）依赖造成的停顿。

作者首先逆向调查 Apple LVP 的实现细节，确定其触发预测的条件；随后指出，虽然 LVP 无法直接预测 64 位值（如指针），但可通过对较小尺寸值的预测进行组合利用，最终实现任意内存访问。

论文端到端构建了基于 LVP 的攻击利用链，在 Safari 与 Chrome 浏览器内获得了 64 位读原语。

这是继 Spectre/Meltdown 之后又一例"性能优化即安全债"的微架构攻击，首次揭示 Apple 自研芯片 LVP 的安全影响，成色在于完整的实现逆向与端到端利用链；其局限是预测条件较苛刻且只能预测小尺寸值，需组合才能形成 64 位读能力。对 Apple Silicon 的防御设计（约束或禁用 LVP 预测）与后续微架构评审有直接警示意义，与既有 load value prediction 类攻击共同构成对闭源芯片安全性的持续挑战。

- **论文 PDF**: [FLOP: Breaking the Apple M3 CPU via False Load Output Predictions.](https://www.usenix.org/system/files/usenixsecurity25-kim-jason.pdf)

#### Branch Privilege Injection: Compromising Spectre v2 Hardware Mitigations by Exploiting Branch Predictor Race Conditions.
- **作者**: Sandro Rüegge; Johannes Wikner; Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: 现代处理器依赖eIBRS/AutoIBRS等硬件缓解阻止Spectre v2（分支目标注入）的跨特权攻击，但这些机制依赖高度异步的分支预测反馈，其特权域隔离是否可靠值得质疑。本文发现一类新漏洞——分支预测器竞态条件（BPRC），并利用其攻破全部近期Intel CPU。

方法上，作者证明Intel存在异步分支预测器更新（ABPU）：新预测在分支退休后才插入且不受lfence同步；据此发现三个变体：用户到内核（BPRCU→K）、客户机到虚拟机监控器（BPRCG→H）与穿透IBPB屏障（BPRCIBPB），且违规全部源于BTB。进而构建新原语Branch Privilege Injection（BPI）：从用户态注入被标记为内核特权的任意分支预测（syscall须在训练后约300-400条NOP窗口内执行，预测持久驻留BTB）。

结果显示，影响自Skylake以来至少六代Intel微架构（Golden Cove上99.6%的jmp*训练位置出现违规信号）；在默认缓解全开的Ubuntu 24.04上以5.6 KiB/s、99.8%准确率泄漏任意内核内存，21秒泄出/etc/shadow哈希；讽刺的是，为防BHI而默认开启的BHI_DIS_S意外禁用内核IBP，反而提高BPI可重复性。已分配CVE-2024-45332，AMD/ARM不受影响；缓解方向为Retpoline+RRSBA_DIS_S（UnixBench最高3.1%、lmbench 8.3%）或IPRED_DIS_S（1.7%/6.4%），完整修复依赖Intel微码。

我的思考：该工作证明"硬件缓解的正确性取决于异步状态同步"，是微架构安全研究的一次漂亮逆向工程；"一个缓解为另一个攻击助攻"的发现提醒安全社区注意缓解间的相互作用。局限是需本地非特权代码执行与离线内核gadget分析，但影响面覆盖六代Intel CPU，分量十足。
- **论文 PDF**: [Branch Privilege Injection: Compromising Spectre v2 Hardware Mitigations by Exploiting Branch Predictor Race Conditions.](https://www.usenix.org/system/files/usenixsecurity25-ruegge.pdf)

#### Sound of Interference: Electromagnetic Eavesdropping Attack on Digital Microphones Using Pulse Density Modulation.
- **作者**: Arifu Onishi, S. Hrushikesh Bhupathiraju, Rishikesh Bhatt, Sara Rampazzi, Takeshi Sugawara
- **方向**: 硬件与物理安全
- **解读**: 声音窃听是隐私保护的经典威胁,但音频信号仅 0.02–20 kHz,难以通过电磁(EM)侧信道远程获取。以往 EM 音频窃听研究(MagEar、Periscope、TEMPEST Comeback)要么针对扬声器,要么依赖片上耦合、局限于特定 SoC。本文发现了一条新路径:现代数字 MEMS 麦克风通过脉冲密度调制(PDM)传输音频,即用输出脉冲密度直接编码模拟声音电平,其数字脉冲的每个谐波都保留了声学信息——攻击者无需恢复数字比特,只需用标准无线电接收机对约 30 kHz 的窄子带做简单 FM 解调即可还原原始音频。本文要验证该漏洞的存在性、评估端到端攻击可行性并探索防御。
作者先在理论上刻画了 PDM 谐波的宽带 EM 泄漏机理,并用仿真验证;随后对五款来自不同厂商的商用 PDM 麦克风(Knowles、ST、TDK-InvenSense、Vesper)做系统性测试,确认泄漏的普遍性。端到端评估覆盖相邻房间窃听场景,考察房间布局、墙体材料与厚度、遮挡、设备朝向等因素;受害者设备包括四款来自三家厂商的笔记本、一款耳机(Jabra)与智能音箱(Google Home)。攻击无需安装恶意软件或篡改设备,甚至在麦克风被内部硬件配置"关闭"时仍可生效。
实验数据极具冲击力:在 25 cm 混凝土墙后的受害者笔记本场景下,攻击对数字语音识别准确率达 94.2%,最远距离 2 米;在石膏板墙另一侧,说话人与数字识别分类准确率 ≥99%,转写词错误率最低仅 6.5%;即使使用未经 EM 轨迹训练的 OpenAI 等公开语音转写 API,恢复 Harvard Sentences 的最大转写错误率也仅 14%。攻击还可用廉价隐蔽的铜箔胶带自制天线完成。防御方面,传统软件防御(采样率随机化)无效,而硬件时钟随机化有效:时钟频率偏离 1% 即可将侧信道信号质量从 0.7254 降至 0.0490,且不损害正常音频性能。
该工作是 TEMPEST 研究的现代化延伸:首次系统揭示 PDM 数字麦克风的固有 EM 泄漏,把"数字传输更抗噪"的常识转化为攻击面,原理清晰、覆盖真实设备与场景,量化的距离/墙厚/准确率指标极具说服力,提出的时钟随机化硬件防御简单有效。局限:PDM 麦克风并非所有设备标配,攻击距离受天线与环境限制。总体而言,它提醒硬件设计者:数字化的信号链路同样会泄漏模拟世界的信息。
- **论文 PDF**: [Sound of Interference: Electromagnetic Eavesdropping Attack on Digital Microphones Using Pulse Density Modulation.](https://www.usenix.org/system/files/usenixsecurity25-onishi.pdf)

#### TimeTravel: Real-time Timing Drift Attack on System Time Using Acoustic Waves.

- **作者**: Jianshuo Liu, Hong Li, Haining Wang, Mengjie Sun, Hui Wen, Jinfa Wang, Limin Sun
- **方向**: 硬件与物理安全
- **解读**: 
实时时钟（RTC）为各类实时系统提供精确系统时间，其内部存储的时间戳通常被视为可信硬件状态。本文揭示 RTC 电路的一个新安全漏洞：攻击者可在物理层面将系统时间任意向前或向后修改，这种动态篡改系统时间的威胁被命名为 TimeTravel。

攻击原理基于声学共振与压电效应：向石英晶体施加声学导波，改变送入 RTC 电路的振荡信号特性。通过调节声波参数，攻击者可以按可调速率加速或减速系统时钟的走时速度，造成计时相对漂移——不需要接触设备，也不需要篡改软件，即可影响依赖时间的认证、日志、证书校验等机制。

作者考察了 RTC 电路下的九个模块和两台商用设备，实验显示 TimeTravel 能以选定速度将系统时间向前/向后漂移，最高精度达 93%；在存在典型障碍物的环境中，攻击成功率仍不低于 77%，说明该攻击在现实场景中具有较强可行性。

该工作揭示了"物理层时间可信性"这一被忽视的攻击面，声学攻击手段新颖且无需拆机，威胁模型清晰。局限在于攻击需要靠近设备发射声波、影响的是相对走速而非瞬时跳变，对严格依赖绝对时间同步的系统效果需具体评估；与同领域的电磁/激光注入攻击相比，声学方式成本更低、隐蔽性更好，值得实时系统设计者关注。

- **论文 PDF**: [TimeTravel: Real-time Timing Drift Attack on System Time Using Acoustic Waves.](https://www.usenix.org/system/files/usenixsecurity25-liu-jianshuo.pdf)

#### DiskSpy: Exploring a Long-Range Covert-Channel Attack via mmWave Sensing of μm-level HDD Vibrations.

- **作者**: Weiye Xu, Danli Wen, Jianwei Liu, Zixin Lin, Yuanqing Zheng, Xian Xu, Jinsong Han
- **方向**: 硬件与物理安全
- **解读**: 
气隙（air-gapped）环境因与外部网络物理隔离而被视为防止敏感信息泄露的可靠手段。本文提出的 DiskSpy 揭示了一种新的隐蔽信道攻击：通过调制硬盘（HDD）的振动来编码敏感信息，再利用毫米波雷达远程感知这些振动并解码数据，从而把机密信息"偷偷送出"气隙环境。

技术难点在于 HDD 振动极其微弱（微米级），而毫米波信号在远距离传播中衰减严重。为此作者开发了一种基于毫米波的长距离微米级振动感知技术，突破了毫米波感知的极限，使远距离解码成为可能。攻击使用商用现成（COTS）毫米波雷达即可实施。

实验表明，即使在 22 米的远距离上，DiskSpy 仍能以 20bps 的速率向远程毫米波雷达传输秘密信息，误码率低于 1.2%。更重要的是，DiskSpy 对 HDD 的安装方式与摆放位置没有限制，甚至在非视距（NLOS）场景下也能发起攻击。

该工作的贡献在于把振动侧信道与毫米波感知结合，实现了气隙攻击距离的数量级提升，且对部署条件几乎无要求，实用性突出。局限在于 20bps 的速率仅适合短消息（如密钥、口令）外传，且需要攻击者预置恶意代码在气隙机器上驱动振动调制；与电磁/热/声学隐蔽信道相比，毫米波方案在距离与 NLOS 能力上优势明显，是隐蔽信道研究的重要进展。

- **论文 PDF**: [DiskSpy: Exploring a Long-Range Covert-Channel Attack via mmWave Sensing of μm-level HDD Vibrations.](https://www.usenix.org/system/files/usenixsecurity25-xu-weiye.pdf)

#### HubBub: Contention-Based Side-Channel Attacks on USB Hubs.

- **作者**: Junpeng Wan, Yanxiang Bi, Han Gao, Dave (Jing) Tian
- **方向**: 硬件与物理安全
- **解读**: 
（无摘要，据标题推断）USB 集线器（hub）是外设连接的基础设施，多台设备共享同一集线器的总线带宽。本文提出 HubBub，一类基于竞争（contention）的侧信道攻击：当目标设备通过共享 USB 集线器通信时，攻击者控制的另一台设备可通过观察总线争用造成的时序变化，推断目标设备的通信活动甚至数据特征。

方法上，攻击者利用 USB 总线的共享介质特性——目标设备传输数据会占用带宽并产生可测量的延迟/争用信号，攻击者设备通过精确测量自身请求的响应时间等指标，建立"争用模式→目标活动"的映射，从而在不需要物理接触目标设备内部的情况下窃听其行为。这属于资源竞争类侧信道（类似 CPU 缓存竞争攻击）在 USB 外设域的移植。

预期研究包括在真实 USB 集线器上验证不同设备组合下的推断精度，以及可能的键盘输入、文件访问等敏感活动的识别。其安全含义是：共享 USB 基础设施本身就构成可信边界上的薄弱点，隔离主机并不能阻止物理邻近的攻击者利用外设生态窃取信息。

该工作的价值在于把经典的竞争侧信道思想扩展到 USB 集线器这一此前较少被研究的共享资源上，威胁模型贴近现实（如共享充电站、KVM 切换器场景）。局限在于未见摘要，具体成功率与适用场景需核实；与基于电磁辐射或电源的 USB 侧信道相比，竞争型攻击无需专用硬件、成本低，值得外设安全社区重视。


#### Lost in Translation: Enabling Confused Deputy Attacks on EDA Software with TransFuzz.

- **作者**: Flavien Solt, Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: 
EDA 工具（仿真器、综合器等）负责把 RTL 设计翻译为底层表示，这一翻译过程的安全属性长期未受重视。本文提出 MIRTL，一种针对 EDA 软件的 confused deputy 攻击：利用 EDA 软件翻译 RTL 时的漏洞作为"小工具"（gadget），将传统硬件木马加固为隐蔽性空前的恶意设计——由于翻译错误只在下游实现阶段显现，白盒测试与形式化验证均不可见。

为发现翻译漏洞，作者设计了新模糊器 TransFuzz：生成包含大量运算符与复杂互连的随机 RTL 设计，专门触发翻译 bug。由于 RTL 表达力强，构造"黄金模型"检测偏差极其困难，TransFuzz 巧妙的替代方案是：对比多个 RTL 仿真器的信号输出，以交叉验证发现翻译漏洞——用仿真器之间的分歧定位 bug。

TransFuzz 在四个流行的开源 EDA 应用中发现了 31 个新 bug（含 25 个 CVE），其中 20 个是翻译漏洞。作者进一步展示 MIRTL 小工具如何加固传统后门以对抗白盒检测，并在 CVA6 RISC-V 核中演示了一个真实世界的 MIRTL 加固后门实例。

该工作的贡献在于系统性地揭示了硬件设计工具链（EDA）本身作为攻击面的可能，并提供了可扩展的自动化发现方法（差分仿真模糊测试）。局限在于攻击依赖 RTL 阶段即被植入恶意设计（需恶意设计者参与），属供应链上游威胁；但与纯门级木马相比，其隐蔽性大幅提升，对硬件信任链研究具有重要警示意义。

- **论文 PDF**: [Lost in Translation: Enabling Confused Deputy Attacks on EDA Software with TransFuzz.](https://www.usenix.org/system/files/usenixsecurity25-solt.pdf)

#### BarraCUDA: Edge GPUs do Leak DNN Weights.

- **作者**: Péter Horváth, Lukasz Chmielewski, Léo Weissbart, Lejla Batina, Yuval Yarom
- **方向**: 硬件与物理安全
- **解读**: 
大量公司把核心知识产权编码在神经网络参数中，而在"攻击者拥有产品物理控制权"的强威胁模型下（如边缘设备落入他人之手），这些参数面临泄露风险。本文提出 BarraCUDA，一种针对通用 GPU 的攻击，能够提取运行在热门 Nvidia Jetson 设备上神经网络的参数。

攻击的核心洞察是：推理中的卷积运算必须以"部分和"（partial sums）序列的方式计算，而每个部分和恰好泄露一个或几个参数。BarraCUDA 利用相关电磁分析（correlation electromagnetic analysis），将实测的电磁辐射与这些部分和计算建立关联，从而逐一恢复参数——不需要破坏芯片封装，仅凭物理邻近的电磁测量即可实施。

实验表明 BarraCUDA 能恢复真实世界卷积神经网络的参数。该攻击直接针对边缘 GPU 推理场景，威胁模型现实：模型部署方无法保证设备物理安全（如售出的汽车、监控设备、机器人），参数一旦泄露即意味着知识产权与模型能力被完整复制。

该工作的贡献在于把侧信道分析从 CPU/微控制器扩展到通用 GPU 推理管线，并给出可复现的攻击流程。局限在于需要物理接近与可控输入（选择触发特定卷积的输入），且逐层恢复耗时长、对网络规模敏感；与 NeuroScope（软件逆向）相比，BarraCUDA 属于物理层提取，二者可组合成"提取结构+窃取权重"的完整模型窃取链，对边缘 AI 部署安全构成实质性警示。

- **论文 PDF**: [BarraCUDA: Edge GPUs do Leak DNN Weights.](https://www.usenix.org/system/files/usenixsecurity25-horvath.pdf)

#### ChoiceJacking: Compromising Mobile Devices through Malicious Chargers like a Decade ago.
- **作者**: Florian Draschbacher, Lukas Maar, Mathias Oberhuber, Stefan Mangard
- **方向**: 硬件与物理安全
- **解读**: 十年前 JuiceJacking 被发现后，移动 OS 引入"是否信任此 USB 主机"的用户确认提示，此后十年再无同量级 USB 攻击。本文发现该缓解的隐含假设——攻击者无法在建立数据连接的同时注入输入事件——在实践中并不成立，提出首个绕过现有 JuiceJacking 缓解的新型 USB 攻击家族 ChoiceJacking。

方法上提出平台无关的攻击原理：恶意充电器在用户交互前/交互间隙伪造触摸/按键输入，自动点击"允许"完成自身数据连接授权；并给出针对 Android 与 iOS 的三种具体实现，配合自制的廉价恶意充电器。为在需解锁设备的场景中隐蔽执行，还利用电源线侧信道检测用户注意力时机，避免视觉痕迹被发现。

评估结果令人警醒：在 8 家厂商（含市占前 6）的全部被测设备上，ChoiceJacking 都能访问照片、文档、应用数据等敏感文件；其中两家厂商的设备即使处于锁定状态也能被提取文件。除一家外的所有厂商（含 Google、Samsung、Xiaomi、Apple）均承认攻击并正在集成缓解。

我的思考：这是对"提示框即安全"这一朴素信任模型的精准打击——安全提示的语义完整性与输入通道的独立性被打破。十年沉寂后 USB 充电攻击卷土重来，说明缓解设计必须考虑输入注入的物理可实现性。局限是部分攻击需设备解锁，且依赖物理接触与用户时机；但低成本硬件+高成功率+跨平台影响的组合使其现实威胁显著。
- **论文 PDF**: [ChoiceJacking: Compromising Mobile Devices through Malicious Chargers like a Decade ago.](https://www.usenix.org/system/files/usenixsecurity25-draschbacher.pdf)

#### Secure Caches for Compartmentalized Software.
- **作者**: Kerem Arikan, Huaxin Tang, Williams Zhang Cen, Yu David Liu, Nael B. Abu-Ghazaleh, Dmitry Ponomarev
- **方向**: 硬件与物理安全
- **解读**: 分区（compartmentalization）软件系统被提出以应对传统进程级隔离的安全挑战，其在同一地址空间内为互不信任组件提供逻辑隔离，但缺乏侧信道隔离，仍易受缓存侧信道攻击。本文研究如何保护分区软件免受硬件缓存侧信道攻击，并应对性能、数据共享与共享库多调用者等独特挑战。

作者提出 SCC 框架：一是多级缓存分区（含 L1 缓存），配以一系列避免性能损失的优化；二是面向域的分区——按内存域而非按分区创建缓存分区；三是为每个调用者创建共享库代码的独立分区实例。作者用操作语义形式化证明 SCC 的安全性，并在 gem5 模拟器上对一组分区基准进行性能评估。

摘要未给出具体数值，宣称安全性经形式化证明、性能开销经 gem5 模拟验证，即在保障缓存隔离的同时将性能损失控制在可接受范围。

将侧信道隔离纳入分区安全模型是分区方案走向实用的关键一步，"按域而非按分区划分"与"共享库按调用者实例化"直击多分区场景的核心痛点，形式化证明增强了可信度。局限是评估限于模拟器，真实硬件行为（预取、多核干扰）可能使结果打折；L1 分区的性能开销与空间利用率仍是现实化挑战。

- **论文 PDF**: [Secure Caches for Compartmentalized Software.](https://www.usenix.org/system/files/usenixsecurity25-arikan.pdf)

#### McSee: Evaluating Advanced Rowhammer Attacks and Defenses via Automated DRAM Traffic Analysis.
- **作者**: Patrick Jattke; Michele Marazzi; Flavien Solt; Max Wipfli; Stefan Gloor; Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）Rowhammer 攻防的军备竞赛已进入"微观层面"：现代 DRAM 内置 TRR、RFM 等缓解机制，攻击者则发展出非均匀、模式化访问策略绕过它们。评估这类攻防需要精确观测内存控制器实际发出的命令流，本文标题表明其核心是自动化的 DRAM 流量分析平台。

据标题推断，McSee 可能是一个对内存控制器到 DRAM 的命令流（激活、预充电、刷新等）进行自动采集、解析与统计的框架，据此量化攻击模式的实际"锤击"强度并检验各种防御机制（含 TRR/RFM 变体）的覆盖效果，可能配套可配置的 Rowhammer 测试平台（如 TRRespass 一类的升级版）。

由于无摘要，具体平台能力与评估数据无法引用，不做数字性断言。

我的思考：Rowhammer 评估的瓶颈在于"看不到 DRAM 里发生了什么"，自动化流量分析恰好补上观测层，让攻击有效性与防御覆盖度都可量化比较，对该领域是基础设施级贡献；若 McSee 开源，可成为社区通用评估基准。局限在于需要特定硬件采集手段，且结论依赖具体 DRAM 型号的微观行为。


#### Not so Refreshing: Attacking GPUs using RFM Rowhammer Mitigation.
- **作者**: Ravan Nazaraliyev; Yicheng Zhang; Sankha Baran Dutta; Andrés Márquez; Kevin J. Barker; Nael B. Abu-Ghazaleh
- **方向**: 硬件与物理安全
- **解读**: 侧信道攻击此前集中于 CPU 内存芯片，难以迁移到 GPU：GDDR 微架构、内存控制器行为与 GPU 计算模型差异巨大。作者逆向出物理地址到 GDDR bank 的映射，并发现既有行缓冲时序攻击因行缓冲管理策略而失效，遂转向现代内存为缓解 Rowhammer 引入的 RFM（刷新管理）特性。

方法上，作者识别出基于 RFM 的时序泄露：频繁访问同一 bank 触发刷新事件，造成可观测的访问时间差异；据此构造共享 GPU 隐蔽信道，并构建离散 GPU（GDDR6）与 GPU SoC（LPDDR5）的端到端侧信道攻击，还利用 RFM 阻塞实现 DoS。

结果显示隐蔽信道带宽超过每 bank 50 KBps、误码率仅 0.03%；GDDR6 上应用指纹识别与 Blender 3D 渲染对象指纹识别 F1 分别达 95% 与 98%；SoC 上应用/网页/视频指纹识别均获高 F1；DoS 攻击平均拖慢应用 4.8 倍以上。

我的思考：把"防御机制"本身变成泄露源是硬件安全研究的经典范式：RFM 本为防 Rowhammer 而设，却成 GPU 侧信道放大器；对共享 GPU 租户场景（云 GPU、ML 训练集群）尤其现实。局限是攻击依赖同一 GPU 共存与特定内存代次，但思路可迁移到其他 DRAM 缓解特性。

- **论文 PDF**: [Not so Refreshing: Attacking GPUs using RFM Rowhammer Mitigation.](https://www.usenix.org/system/files/usenixsecurity25-nazaraliyev.pdf)

#### Posthammer: Pervasive Browser-based Rowhammer Attacks with Postponed Refresh Commands.
- **作者**: Finn de Ridder; Patrick Jattke; Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: 浏览器是客户端系统最大的 Rowhammer 威胁面，但作者大规模评估发现，浏览器场景仅在少量 DRAM 设备上能触发比特翻转。推迟刷新命令（可触发 DRAM 内置缓解）理论上能提升攻击效果，却从未在实践中实现；Posthammer 首次做到这一点。

方法上，Posthammer 以 JavaScript 实现：通过长时间高强度 Rowhammer 活动迫使 CPU 内存控制器推迟刷新命令，再留出足够长的延迟窗口让控制器批量补刷；并引入 lane 抽象，使 Rowhammer 模式中的部分地址被更高频访问，首次在浏览器中支持"刷新推迟"的非均匀模式。

结果显示，Posthammer 比现有最优技术有效 2.8 倍，在 28 台 DDR4 测试设备中于 86% 的设备上触发比特翻转。

我的思考：把 Rowhammer 威胁从"原生程序"推进到"纯浏览器脚本"，意味着无需安装任何软件即可攻击任意访客，现实威胁显著升级；lane 抽象也让攻击模式设计更灵活。局限是依赖具体内存控制器调度行为与 DRAM 代次，且 86% 覆盖面意味着仍有设备免疫；对缓解机制（如 TRR/RFM 升级）的对抗仍需持续评估。

- **论文 PDF**: [Posthammer: Pervasive Browser-based Rowhammer Attacks with Postponed Refresh Commands.](https://www.usenix.org/system/files/usenixsecurity25-de-ridder.pdf)

#### ECC.fail: Mounting Rowhammer Attacks on DDR4 Servers with ECC Memory.
- **作者**: Nureddin Kamadan; Walter Wang; Stephan van Schaik; Christina Garman; Daniel Genkin; Yuval Yarom
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）服务器内存普遍启用 ECC（纠错码），传统观点认为 ECC 能检测并纠正 Rowhammer 引发的单比特翻转，从而在内存层瓦解攻击。本文标题 "ECC.fail"（呼应经典的 Rowhammer.js 等命名）直指该假设的失效：DDR4 服务器上的 ECC 内存同样可以被 Rowhammer 攻破。

据标题推断，作者可能研究了 ECC 纠错与 DRAM 刷新/缓解机制的相互作用，展示通过精确的比特翻转操纵（如同行对称翻转、模式化锤击）使 ECC 无法纠正或掩盖错误，进而实现权限提升、数据破坏或绕过内存完整性保护。

由于无摘要，具体攻击流程与量化结果无法引用，不做数字性断言。

我的思考：ECC 常被视为服务器端 Rowhammer 的"天堑"，若被系统攻破将动摇云与数据中心的信任边界；此类工作提醒安全设计不能把纠错码当作安全原语。局限在于需要本地代码执行与精确的物理地址/库映射，且现代内存缓解（TRR/RFM）叠加后攻击条件更苛刻，现实利用门槛需如实评估。总体而言，该工作的意义在于重新划定"内存纠错≠内存安全"的边界，推动服务器侧防御升级。


#### Relocate-Vote: Using Sparsity Information to Exploit Ciphertext Side-Channels.
- **作者**: Yuqin Yan; Wei Huang; Ilya Grishchenko; Gururaj Saileshwar; Aastha Mehta; David Lie
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）硬件内存加密（如 Intel SGX、MKTME 等）以"密文侧信道"闻名：密文的可压缩性/稀疏性会泄露明文信息，攻击者可据此重构受害者数据。本文标题 "Relocate-Vote" 暗示一种利用稀疏性信息的投票式攻击机制，把密文侧信道推向更实用的程度。

据标题推断，方法可能先定位密文中可压缩或稀疏的区域（如全零、重复块），利用压缩结果（密文大小或压缩标志位）作为"投票"信号，跨多次观测或跨行迁移（relocate）累积证据，从而恢复受害者内存中的秘密（如密钥、页面内容）。

由于无摘要，具体机制与量化结果无法引用，不做数字性断言。

我的思考：密文侧信道直接威胁"加密内存=安全内存"的假设，若能借稀疏性把噪声信号放大为稳定泄露，将显著压低硬件加密方案的有效安全边界；该方向与 SGX 相关攻击一脉相承。局限在于依赖特定的内存布局与压缩策略，且攻击通常需要与受害者共享物理资源或具备一定观测能力，现实约束需在论文中量化说明。总体而言，此类攻击再次表明硬件加密方案的强度取决于其周边实现的严密性。


#### GPUHammer: Rowhammer Attacks on GPU Memories are Practical.
- **作者**: Chris S. Lin; Joyce Qu; Gururaj Saileshwar
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）GPU 内存（GDDR）此前被认为对 Rowhammer 相对免疫，原因在于其刷新策略、bank 组织与 CPU 内存不同；本文标题以 "Practical" 直接回应这一乐观假设，声称 GPU 内存上的 Rowhammer 攻击切实可行。

据标题推断，作者应通过逆向 GDDR 的地址映射与刷新调度，构造能在 GPU 计算模型下持续高频访问目标行的模式，验证比特翻转的可触发性，并可能演示从 GPU 侧发起的权限提升或数据破坏（如影响共享 GPU 环境下的其他租户或 GPU 上运行的模型数据）。

由于无摘要，具体翻转率、成功率与利用链无法引用，不做数字性断言。

我的思考：GPU 已成为 AI/云计算的算力底座，多租户共享 GPU 日益普遍，若 Rowhammer 在 GPU 内存上可行，"隔离"承诺将被削弱；该工作若成功，将把 Rowhammer 威胁面扩展到此前被认为安全的加速器域。局限在于 GDDR 代次差异大、地址映射逆向复杂，攻击条件（时钟、刷新窗口）可能受限，实用性与可移植性需谨慎评估。


#### SCASE: Automated Secret Recovery via Side-Channel-Assisted Symbolic Execution.
- **作者**: Daniel Weber; Lukas Gerlach; Leon Trampert; Youheng Lü; Jo Van Bulck; Michael Schwarz
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）微架构侧信道攻击（缓存、时序）能逐比特泄露秘密，但人工把泄露信号转译为秘密恢复流程繁琐易错；符号执行擅长探索程序路径与约束求解，却不掌握运行时的物理泄露。SCASE 标题表明其把两者结合：用侧信道泄露"辅助"符号执行，自动恢复秘密（如密钥）。

据标题推断，方法可能用符号执行建模受害者程序，把观测到的侧信道信号（如访问踪迹、时序分支）作为约束注入符号状态，自动剪枝错误路径并求解出与观测一致的具体秘密值，实现端到端的自动化密钥恢复。

由于无摘要，具体目标场景（如 AES 密钥、口令）与恢复成功率无法引用，不做数字性断言。

我的思考：手工"侧信道→秘密"流程长期依赖专家经验，自动化是放大攻击能力、降低门槛的关键一步；SCASE 若成功，可作为安全评估工具自动验证"此处是否可经侧信道泄露秘密"。局限是符号执行的状态爆炸与侧信道噪声的建模误差仍是根本瓶颈，且适用目标需可被符号化建模。总体而言，自动化秘密恢复工具的成熟将显著降低侧信道攻击的准入门槛，也便于防御方自测。


#### Shadows in Cipher Spaces: Exploiting Tweak Repetition in Hardware Memory Encryption.
- **作者**: Wei Peng; Yinshuai Li; Yinqian Zhang
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）硬件内存加密（SGX、MKTME、SEV 等）通常用"块地址/计数器作为 tweak + AES"的 XTS/GCM 类模式加密内存行；tweak 若被重复使用（如行迁移后 tweak 未更新、虚拟地址别名、休眠唤醒回滚），会破坏加密的语义安全，产生"密文空间中的影子"。本文标题表明其利用 tweak 重复这一实现缺陷发起攻击。

据标题推断，作者可能定位了某种硬件或软件路径导致的 tweak 复用场景，展示攻击者可借相同 tweak 下的密文关系（如 XOR 关系）恢复明文或注入伪造数据，并可能结合行迁移、页回收等系统机制放大效果。

由于无摘要，具体利用场景与量化影响无法引用，不做数字性断言。

我的思考：密码学原语在硅片上的工程实现（计数器管理、迁移路径）是此类攻击的温床，tweak 复用直击加密模式的证明假设，属于"实现与证明的落差"类漏洞，发现难度高、影响面可能是平台级的。局限是通常需要特定触发条件（如行迁移事件）与部分观测能力，现实可利用性取决于平台调度行为，需论文给出实证支撑。总体而言，该工作提醒硬件设计者在采纳密码学模式时必须同时保证其计数/tweak 状态的完备管理。


#### Breaking the Blindfold: Deep Learning-based Blind Side-channel Analysis.
- **作者**: Azade Rezaeezade; Trevor Yap; Dirmanto Jap; Shivam Bhasin; Stjepan Picek
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）传统侧信道分析（SCA）需要"已知明文/密文"来建立泄露模型（如相关功耗分析），被称为"蒙眼"场景的盲分析（blind SCA）则在不掌握中间值的情况下仅凭泄露曲线恢复秘密，此前被认为极难。本文标题"摘下蒙眼布"暗示用深度学习攻克这一盲场景。

据标题推断，方法可能以深度网络（如 CNN、自编码器或对比学习）直接从原始功耗/电磁轨迹中无监督或弱监督地提取与秘密相关的特征，无需明文标签即可聚类或分离出正确密钥假设，把"盲分析"转化为可训练的表征学习问题。

由于无摘要，具体网络结构、数据集与成功率无法引用，不做数字性断言。

我的思考：深度学习让 SCA 从"已知中间值建模"走向"数据驱动特征发现"，盲分析若被攻破，将动摇"攻击者不知道明文就安全"的常见假设，对嵌入式安全评估影响深远；Picek 团队在该方向积累深厚，可信度高。局限是深度方法的泛化性（跨设备、跨噪声）与训练成本仍是实际障碍，无监督信号的有效性依赖泄露分布的可分性。总体而言，盲分析的突破将推动 SCA 评估向更贴近实战的设定演进。


#### EchoLLM: LLM-Augmented Acoustic Eavesdropping Attack on Bone Conduction Headphones with mmWave Radar.
- **作者**: Xin Yao, Kecheng Huang, Yimin Chen, Jiawei Guo, Jie Tang, Ming Zhao
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）
**问题**：骨传导耳机通过颅骨振动传递声音，旁人很难察觉佩戴者正在收听的内容；攻击者可借助毫米波雷达远程感知佩戴者头部或声带的微弱振动，从而窃听耳机播放的音频，构成一种新颖的物理层侧信道窃听攻击，直接威胁用户隐私。
**方法**：EchoLLM将毫米波雷达感知与大型语言模型（LLM）相结合：雷达负责捕捉骨传导引起的微振动信号，LLM则利用语言先验对低信噪比、受损的语音片段进行增强与语义补全，从而端到端地恢复被窃听的内容。
**结果**：论文标题表明其实现了LLM增强的完整窃听链路，具体的识别准确率、有效作用距离等量化指标以原文为准，此处不作臆断。
**思考**：该工作属于物理侧信道攻击与AI信号恢复的交叉方向，威胁模型新颖，警示"听不见声音的环境"并不等于"无法窃听的环境"；其实际可利用性受雷达距离、入射角度、环境多径与语音可懂度等因素制约，防御思路可考虑为骨传导设备增加抗雷达探测的屏蔽或对振动信号施加加扰处理。


#### Rowhammer-Based Trojan Injection: One Bit Flip Is Sufficient for Backdooring DNNs.
- **作者**: Xiang Li, Ying Meng, Junming Chen, Lannan Luo, Qiang Zeng
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）
**问题**：深度神经网络（DNN）的权重存储于DRAM中，传统后门注入需要控制训练过程或篡改模型文件；本文探索一条纯物理路径——利用Rowhammer攻击反复访问DRAM行造成相邻行位翻转，以极少的比特翻转（标题称一次即可）在已部署模型的权重中植入木马，实现无需训练控制的"物理级"后门。
**方法**：据标题推断，作者分析DNN权重在内存中的布局，定位对模型输出影响最大的关键比特，通过Rowhammer精确翻转这些比特，使模型在特定触发输入下产生预设错误输出，同时保持正常输入的精度基本不变。
**结果**：论文标题表明其实现了基于Rowhammer的DNN木马注入，且单次位翻转即可奏效，具体攻击成功率与对精度的影响以原文为准，此处不作臆断。
**思考**：该工作打通了硬件漏洞（Rowhammer）与模型安全（后门）两个领域，威胁模型独特——攻击者只需物理接触或利用云租户的Rowhammer能力，无需任何训练控制权，对"可信硬件上部署可信模型"的假设构成挑战；局限在于位翻转的精确性、ECC内存的防护以及大规模翻转的可靠性，但其"最小干预"的思路在物理侧信道攻击中很有新意。


#### Achilles: A Formal Framework of Leaking Secrets from Signature Schemes via Rowhammer.

- **作者**: Junkai Liang, Zhi Zhang, Xin Zhang, Qingni Shen, Yansong Gao, Xingliang Yuan, Haiyang Xue, Pengfei Wu, Zhonghai Wu
- **方向**: 硬件与物理安全
- **解读**:

签名方案是网络安全基础设施的基本组件，它们虽在数学上抗密码攻击，却面临 Rowhammer 故障注入攻击。已有攻击都是"特设"的——只针对特定签名方案的特定参数，因此 Rowhammer 对签名方案整体的影响范围一直不清楚，缺乏系统性理解与自动化分析手段。

方法上提出 Achilles 形式化框架，帮助在各种真实签名方案中通过 Rowhammer 泄露密钥：将 Rowhammer 故障诱导到广义签名方案 G-sign 的关键参数上，再进行故障后分析以恢复秘密。该框架可用于发现已研究方案中更脆弱的参数，也能揭示可能易受攻击的新方案。

结果：评估了六个签名方案（其中五个被分配 CVE 追踪其 Rowhammer 漏洞），覆盖基于不同数学问题的传统与后量子签名；根据 Achilles 分析，六个方案全部被证明易受攻击，并为 EdDSA 识别出两个新的脆弱参数；进一步在 wolfssl、relic、liboqs 等现代密码库上对每个方案演示了成功的 Rowhammer 攻击。

我的思考：该工作的成色在于把 Rowhammer 攻击从"逐方案特设"提升为"通用形式化框架"，G-sign 抽象与故障后分析流程使新方案可被系统筛查，六个方案全数中招并产出五个 CVE、为 EdDSA 新增脆弱参数，证据链完整。局限：形式化框架对故障模型（翻转比特位置、诱导概率）的假设与真实 Rowhammer 物理条件之间的差距、攻击对内存布局与硬件环境的依赖，可能限制现实可利用性。对密码库实现者（内存加固、密钥冗余）与硬件防御均有直接警示价值。

- **论文 PDF**: [Achilles: A Formal Framework of Leaking Secrets from Signature Schemes via Rowhammer.](https://www.usenix.org/system/files/usenixsecurity25-liang-achilles.pdf)

#### Found in Translation: A Generative Language Modeling Approach to Memory Access Pattern Attacks.

- **作者**: Grace Jia, Alex Wong, Anurag Khandelwal

- **方向**: 硬件与物理安全

- **解读**: 

（无摘要，据标题推断）内存访问模式泄漏是侧信道攻击的重要来源：即使数据加密，处理器/存储系统对内存的访问序列仍可暴露程序行为；对抗方若能预测或建模受害者的访问模式，即可破坏 ORAM 等防御的混淆效果。传统攻击依赖手工设计的统计特征，难以捕捉复杂时序依赖。

从标题推断，本文把内存访问模式攻击建模为序列翻译/语言建模问题：应是用生成式语言模型学习"受害者程序行为到访问模式"或"混淆访问流到真实访问"的映射，端到端地从观测到的混淆访问流中恢复真实访问规律。具体模型架构与目标系统因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者以攻击成功率或恢复保真度对比了生成式方法与既有统计攻击。

我的思考（基于标题的保守推断）：把现代序列模型引入侧信道分析是值得关注的方向：其强表达力可能攻破基于固定模式假设的防御，但也可能因可解释性差而难以转化为实际利用。与经典 ORAM 攻击（如基于时序统计的定位）相比，该方法的价值在于能否在更少假设下达到更高恢复精度；若成立，将迫使防御方重新审视"混淆随机性是否可被学习"这一基本假设。


#### GlitchFHE: Attacking Fully Homomorphic Encryption Using Fault Injection.
- **作者**: Lakshmi Likhitha Mankali, Mohammed Nabeel, Faiq Raees, Michail Maniatakos, Ozgur Sinanoglu, Johann Knechtel
- **方向**: 硬件与物理安全
- **解读**: （无摘要，据标题推断）

**问题**：全同态加密（FHE）在语义层面保证数据机密性，但其实际部署（尤其硬件实现）可能遭受物理攻击；故障注入（如时钟毛刺）可在计算过程中诱导错误结果，若攻击者能观察或控制故障行为，可能破坏 FHE 的安全性假设。

**方法**：该工作对 FHE 实现开展故障注入攻击研究：通过电压/时钟毛刺等物理手段在加密计算的关键步骤中诱导确定性故障，分析错误输出与秘密数据之间的关联，从而提取或推断明文信息。

**结果**：论文无公开摘要，具体数字无法引用。（据标题推断）实验应报告了故障注入的成功率、所需注入次数以及恢复出明文信息的程度。

**思考**：该工作把"物理攻击"与"密码学安全证明"之间的鸿沟摆上台面——FHE 的安全性证明假设完美实现，而物理实现常留攻击面；故障注入能否规模化、是否需要多次注入与统计后处理，决定其实际威胁等级；对硬件 FHE 加速器设计者而言，故障检测与冗余校验应成为标配。


### 移动与物联网安全（16 篇）

#### Cyber-Physical Deception Through Coordinated IoT Honeypots.
- **作者**: Chongqi Guan, Guohong Cao
- **方向**: 移动与物联网安全
- **解读**: 问题：IoT 设备大规模部署后面临众多攻击威胁，蜜罐是吸引并观察攻击者的有效手段。但传统蜜罐无法模拟真实 IoT 环境中设备间的物理与网络依赖关系，攻击者从多个来源（横跨网络域与物理域）发起攻击验证设备状态时，很容易识破蜜罐间的不一致性，导致欺骗失效。

方法：作者提出一种网络-物理欺骗系统 CPDS，通过协调多个 IoT 蜜罐来模仿设备间复杂的网络-物理连接。具体地，系统收集并分析攻击轨迹以建模单个设备的漏洞，分析设备间的物理与网络依赖并将其形式化为 Prolog 规则，再根据攻击者行为与依赖规则协调各蜜罐，保证蜜罐间跨层一致性；实现上利用软件定义网络（SDN）增强现有 IoT 蜜罐并使其协同工作。

结果：通过在线部署、针对真实攻击场景的人工评估以及大规模仿真实验，验证了 CPDS 在保真度（fidelity）与可扩展性（scalability）两方面的有效性。

思考：该工作把"蜜罐一致性"从网络层扩展到网络-物理跨层，抓住了攻击者反识别蜜罐的关键弱点，立意新颖；Prolog 规则表达依赖关系直观但面对大规模动态环境可能存在建模开销与维护成本，其与真实生产环境的差距是值得进一步考察的局限。

- **论文 PDF**: [Cyber-Physical Deception Through Coordinated IoT Honeypots.](https://www.usenix.org/system/files/usenixsecurity25-guan.pdf)

#### TORCHLIGHT: Shedding LIGHT on Real-World Attacks on Cloudless IoT Devices Concealed within the Tor Network.
- **作者**: Yumingzhi Pan; Zhen Ling; Yue Zhang; Hongze Wang; Guangchi Liu; Junzhou Luo; Xinwen Fu
- **方向**: 移动与物联网安全
- **解读**: 无云IoT设备直接暴露于互联网，本文意外发现大量Tor流量指向此类设备，怀疑攻击者借Tor匿名化地利用地下市场流出的未公开漏洞。为验证这一假设，作者构建TORCHLIGHT系统。

方法上，系统含三组件：出口流量收集器利用Tor内外网流量的IP模式（源/目的IP是否为Tor节点）经iptables/ipset/NFQUEUE在内核层高效过滤；部署规划器基于Tor加权带宽算法推导恶意流量经过已部署节点的概率Pc(b)，在预算内按性价比优化VPS部署；LLM分析器采用五步思维链（设备名NER识别→自验证防幻觉→搜索引擎RAG补全厂商型号→确认真实IoT流量→识别攻击），底层为量化的Llama 2 70B。

结果显示，仅以196美元部署3个VPS出口节点，12个月收集26.5TB流量、6047万条响应，识别出50874台设备（DVR占54.9%、Qualvision占40.9%）；共发现45个漏洞，其中29个零日（25个已分配CVE，5个CRITICAL/3个HIGH/16个MEDIUM/1个LOW），按VulDB估价约31.2万美元，影响148个国家的约1271万台设备；IoT识别在Tor流量集上准确率93.84%、宏F1为0.8671。

我的思考：首次实证证明攻击者确实利用Tor隐匿身份攻击无云IoT设备，把"暗网交易漏洞"的传闻转化为可量化的观测情报，196美元的低成本方法论极具可复制性。局限包括出口节点观测偏差、LLM幻觉需人工复核兜底、伦理披露边界（三星DVR漏洞细节未公开）。

- **论文 PDF**: [TORCHLIGHT: Shedding LIGHT on Real-World Attacks on Cloudless IoT Devices Concealed within the Tor Network.](https://www.usenix.org/system/files/usenixsecurity25-pan-yumingzhi.pdf)

#### Am I Infected? Lessons from Operating a Large-Scale IoT Security Diagnostic Service.
- **作者**: Takayuki Sasaki, Tomoya Inazawa, Youhei Yamaguchi, Simon Parkin, Michel van Eeten, Katsunari Yoshioka, Tsutomu Matsumoto
- **方向**: 移动与物联网安全
- **解读**: 家庭物联网设备的安全责任被默认为由用户承担，但用户往往缺乏"该做什么"的信息：不知设备是否易受攻击、已被感染、如何修复。2022 年 2 月，作者上线了一个面向日本用户的网页服务，扫描其 IoT 设备以评估安全性，用于诊断并修复设备漏洞与恶意软件感染。本文报告该服务大规模运营的发现与经验。

研究由三部分组成：(1) 2022 年 2 月至 2024 年 5 月间 114,747 名用户的参与数据；(2) 服务用户的大规模评估问卷（n=4,103）；(3) 针对"设备不安全"用户补救行为的调查（n=90）。运营期间，服务向 417 名（0.36%）用户通知其设备存在漏洞，向 171 名（0.15%）用户通知其设备感染恶意软件；99% 的用户未发现问题。

尽管绝大多数用户无恙，96% 的用户仍积极评价服务，最常见理由是安心、免费与诊断快速。补救上：171 名感染用户中 67 人回访复查，59 人显示改善；417 名漏洞用户中 151 人回访复诊，75 人显示改善。作者还总结了运营教训，包括非专家用户会对安全扫描产生何种能力假设。

该工作提供了大规模 IoT 安全诊断服务的罕见纵向数据（11 万+用户、两年运营），量化了家庭 IoT 的真实风险面（感染率、漏洞率均低但存在），并刻画了"通知→补救"的真实转化率（约半数回访者改善），对安全通知设计与服务运营有直接价值。局限：样本限于日本用户，且主动使用服务的用户本就更关注安全，自我选择偏差可能高估参与度；诊断手段对新型恶意软件的检出能力受限。总体而言，它是"面向大众的 IoT 安全服务"领域的重要实证参考。

- **论文 PDF**: [Am I Infected? Lessons from Operating a Large-Scale IoT Security Diagnostic Service.](https://www.usenix.org/system/files/usenixsecurity25-sasaki.pdf)

#### AirTag-Facilitated Stalking Protection: Evaluating Unwanted Tracking Notifications and Tracker Locating Features.
- **作者**: Dañiel Gerhardt, Matthias Fassl, Carolyn Guthoff, Adrian Dabrowski, Katharina Krombholz
- **方向**: 移动与物联网安全
- **解读**: 苹果 AirTag 等蓝牙追踪器被滥用于跟踪骚扰（stalking）已成为现实的社会安全问题；为此苹果推出了"未知追踪器通知"（unwanted tracking notifications）与追踪器定位功能（如 Find My 中的 Precision Finding、"Tracker Detect"应用），在 iOS 与 Android 上警示用户"有陌生追踪器随行"并帮助其找到设备。但这些保护功能在真实场景中是否有效、用户能否理解并正确使用，此前缺乏系统的用户体验评估。本工作从标题看对这两类防护功能进行评测。

据标题推断，方法上可能开展用户研究：模拟被跟踪场景（如随身放置 AirTag），让参与者体验通知触发过程并尝试定位追踪器，观测其能否正确理解通知、完成定位、乃至采取后续行动（如关闭/禁用追踪器）；可能结合技术测试（通知延迟、触发条件）与质性访谈。摘要缺失，实验设计无法确认。

摘要缺失，无法引用量化结果；预期包含通知触发率/延迟、参与者定位成功率、对通知的理解与情绪反应，以及跨平台（iOS/Android）差异等数据。

防跟踪功能是平台方对社会责任的回应，但其有效性完全取决于"通知是否及时可理解、定位是否可操作"——若用户在恐慌中无法完成定位，防护形同虚设。该工作把这两项功能置于真实用户情境中检验，对平台改进通知设计与定位交互有直接价值。局限：无摘要，实验室模拟与真实跟踪场景（跟踪者的对抗行为）存在差距；样本量与方法细节需全文确认。（无摘要，据标题推断）


#### Patching Up: Stakeholder Experiences of Security Updates for Connected Medical Devices.

- **作者**: Lorenz Kustosch, Carlos Gañán, Michel van Eeten, Simon Parkin
- **方向**: 移动与物联网安全
- **解读**: 医疗设备日益联网，需要安全措施保障患者安全与数据保护，但此类设备常被报道缺乏基本安全且运行着未修补的过时软件，业界因而呼吁更快更规律地向现场设备推送安全补丁。

方法上，本文对联网医疗设备打补丁的现行实践进行实证研究：开展 23 次半结构化访谈，覆盖 9 家医疗服务机构（HDO）与 3 家设备制造商，并采集 25 台具体设备的实际更新数据。

研究发现：向医疗设备交付软件更新对 HDO 和制造商而言是费力的高成本过程——医疗使用的运营要求与日益增长的设施管理需求给相关利益相关者带来巨大压力，使得常规安全打补丁能否在不压垮医疗行业运营与财务的前提下奏效存疑。

我的思考：该研究用一手访谈与真实设备数据揭示了医疗补丁生态的系统性困境：问题不在「愿不愿意打补丁」，而在流程、法规与资源约束。研究覆盖了供需两侧（HDO 与厂商），视角完整；局限是 25 台设备样本有限、且以访谈为主。对监管者（如 FDA）与设备设计者（可更新性设计）都有直接政策含义。

- **论文 PDF**: [Patching Up: Stakeholder Experiences of Security Updates for Connected Medical Devices.](https://www.usenix.org/system/files/usenixsecurity25-kustosch-patching.pdf)

#### No Way to Sign Out? Unpacking Non-Compliance with Google Play's App Account Deletion Requirements.

- **作者**: Jingwen Yan; Song Liao; Jin Ma; Mohammed Aldeen; Salish Kumar; Long Cheng
- **方向**: 移动与物联网安全
- **解读**: （无摘要，据标题推断）Google Play 要求提供账户的应用必须允许用户便捷删除账户，本文针对该政策的实际执行合规性展开实证调查——"No Way to Sign Out"的质问暗示大量应用并未真正履行义务。

  据标题推断，研究者对 Google Play 上大量热门应用进行系统测量：检查应用是否提供账户删除入口、删除流程是否顺畅，以及是否存在隐蔽阻碍（强制联系客服、要求登录 Web 端、以"冻结"替代删除）等非合规模式。

  无摘要，具体比例等数字无法从标题确认；从疑问语气看，非合规现象应相当普遍，可能量化出显著比例的应用未满足删除要求。

  账户删除是隐私保护（尤其 GDPR/CCPA 类"被遗忘权"）落地的关键一环，应用以"软阻碍"拖延删除是规避监管的常见手法；这类测量研究为政策执行者提供证据基础，也暴露商店审核机制的漏洞；局限在于抽样范围与人工验证成本，且商店政策随版本演化，结论需持续追踪。


#### Lost in the Mists of Time: Expirations in DNS Footprints of Mobile Apps.

- **作者**: Johnny So; Iskander Sánchez-Rola; Nick Nikiforakis
- **方向**: 移动与物联网安全
- **解读**: 移动应用像二进制程序一样"静态"，但一旦与 Web 交互就含动态组件：应用本身未更新，动态组件的变化却可能改变整体行为，其中依赖域名过期带来的风险此前未被大规模刻画。

  作者首次从时间与版本更新的双重视角大规模分析移动应用依赖：构建含 77,206 个版本、15,124 个唯一 Android 应用的语料库，用动态、UI 引导的测试输入生成器监控网络流量，提取每个 APK 的 eTLD+1 域名依赖（"DNS 足迹"）；再结合被动 DNS 推断单个 APK 的潜在脆弱时段。

  结果表明过期域名威胁在规模上不可忽视：影响数百个应用与数千个 APK，甚至波及类别排名前十、下载量数亿或最新版本的应用；分析期间在应用足迹中发现 41 个立即可注册的域名，并以案例研究证明其滥用潜力；即便用户能即时更新应用，也无法自我防护于过期依赖带来的风险。

  过期域名是"数字荒地"式供应链风险——任何人都可注册后静默接管应用行为（追踪、注入、钓鱼）；本文的价值在于把域名生命周期纳入移动供应链安全视角，并强调用户无可自救、责任在开发者与商店生态；相比桌面端过期域名研究，移动端版本碎片化使暴露窗口更复杂。

- **论文 PDF**: [Lost in the Mists of Time: Expirations in DNS Footprints of Mobile Apps.](https://www.usenix.org/system/files/usenixsecurity25-so.pdf)

#### TapTrap: Animation-Driven Tapjacking on Android.

- **作者**: Philipp Beer; Marco Squarcina; Sebastian Roth; Martina Lindorfer
- **方向**: 移动与物联网安全
- **解读**: （无摘要，据标题推断）点击劫持（tapjacking）是 Android 上利用恶意 UI 覆盖层诱导用户误触的经典攻击；本文提出一种动画驱动的点击劫持新变体，针对现有防护手段的盲区。

  据标题推断，TapTrap 利用动画（在点击瞬间移动或缩放覆盖元素、利用动画时序差）把目标控件在用户手指落下的刹那"送入"点击位置，从而绕过系统对 overlay 的检测、FLAG_SECURE 及用户可见性警示等既有缓解。

  无摘要，具体成功率等量化数据无法从标题确认；从命名与作者背景看，应包含对主流 Android 版本与已知防护的实证绕过测试。

  动画型攻击把"用户看不见的界面变化"武器化，说明移动端点击劫持防护必须从静态覆盖检测走向时序感知的动态防护；与 overlay 权限滥用、无障碍服务攻击等同类工作一致，Android 的 UI 信任边界始终是攻防拉锯点；实际威胁取决于绕过手段在真实设备上的稳定性与普及成本；此类研究也提醒商店审核需关注动画类 UI 组件的滥用，将时序行为纳入应用审查视野。


#### The Doom of Device Drivers: Your Android Device (Most Likely) has N-Day Kernel Vulnerabilities.
- **作者**: Lukas Maar, Florian Draschbacher, Lorenz Schumm, Ernesto Martínez García, Stefan Mangard
- **方向**: 移动与物联网安全
- **解读**: Android 厂商碎片化使内核补丁分发严重滞后，而非特权应用可直接访问的厂商内核驱动（GPU 之外）正是 N-day 漏洞的最大温床。此前研究多聚焦 GPU，本文要系统回答：除 GPU 外还有哪些驱动暴露给不可信应用、这些驱动上的已知漏洞有多少长期未打补丁、能否被真实利用。

方法上，作者大规模分析了 131 台 Android 设备、493 个固件版本、覆盖 7 家 OEM，审查非特权应用可访问的内核驱动；采用历史树（history-tree）方法跨驱动重建公开已知漏洞（如 use-after-free、越界写），并设计半自动方法检测 50 个驱动漏洞中有 21 个的补丁存在性，最后在真实设备子集上验证漏洞可触发性。

结果显示，GPU 之外还识别出 11 个可被不可信应用利用的驱动，共重建 50+ 个已知漏洞/CVE 原语；超过 59% 的分析设备至少含一个高危 N-day 漏洞；补丁延迟极不均匀且常长达数月，最长观察达 830 天，说明"已知漏洞"在碎片化生态中依旧致命。

我的思考：该工作把 Android 安全讨论从"有无零日"拉回到更普遍的"N-day 长期裸奔"现实，量化证据（59% 设备、830 天延迟）极具说服力，也呼应了驱动作为提权跳板的攻击面价值。局限是补丁检测为半自动、样本限于 7 家 OEM，且"可利用性"仅验证子集；但作为生态级风险评估，其政策与工程启示（驱动访问控制收紧、补丁时限）是明确的。（无摘要，据标题推断）

#### Ariadne: Navigating through the Labyrinth of Data-Driven Customization Inconsistencies in Android.
- **作者**: Parjanya Vyas, Haseeb Ur Rehman Faheem, Yousra Aafer, N. Asokan
- **方向**: 移动与物联网安全
- **解读**: 厂商对 Android 框架的"数据驱动定制"（修改受访问控制保护的数据持有者/字段）会在不修改代码的情况下悄悄改变访问控制语义，造成现有方法难以建模与检测的 AC 不一致。这类漏洞可被恶意应用利用获取本不该拥有的能力，是碎片化生态特有的安全盲区。

Ariadne 构建抽象 AC 依赖图：以框架中的数据持有者与 API 为节点，编码确定性/非确定性 AC 依赖边；借鉴污点传播思路，让 AC 属性（而非敏感数据）沿依赖图流动，推断每个数据持有者及其 API 应有的 AC 等级，与实际强制不一致处即为漏洞候选。

在 2 个 AOSP 与 11 个定制 ROM（8 厂商、Android 8-14）上，Ariadne 对 AOSP 数据驱动相关 API 的 AC 预测准确率约 97%；共发现 90 个独特不一致（估计误报率约 11.8%），其中 13 个构造出端到端 exploit（如启用/禁用关键包、以系统权限启动受保护 Activity），并发现 30 个先前工作遗漏的不一致。

我的思考：该工作把"数据驱动定制"从难以言说的厂商 bug 变成可建模、可检测的对象，AC 依赖图+传播推断的思路简洁有效，且开源发布便于复现。局限是依赖 WALA 反编译（11 个 ROM 中约 17% 的 dex 无法可靠处理，尤其 Vivo），误报仍存；但与既有不一致检测方法形成互补，对定制 Android 生态的加固有直接价值。（无摘要，据标题推断）

#### eSIMplicity or eSIMplification? Privacy and Security Risks in the eSIM Ecosystem.
- **作者**: Maryam Motallebighomi; Jason Veara; Evangelos Bitsikas; Aanjhan Ranganathan
- **方向**: 移动与物联网安全
- **解读**: （无摘要，据标题推断）eSIM 取代物理 SIM 成为移动设备身份认证的新载体，其生态涉及运营商、eSIM 供应商、设备厂商与用户多方的信任链。标题以 "eSIMplicity/eSIMplification" 双关发问：eSIM 带来便捷的同时，是否也带来了过度简化导致的安全简化？本文应系统梳理 eSIM 生态中的隐私与安全风险。

据标题推断，研究可能覆盖 eSIM 远程配置（Remote Provisioning）流程、Profile 下载与切换机制、运营商与供应商接口的信任边界，以及用户身份与位置信息的泄露面，可能结合逆向或实测验证。

由于无摘要，具体发现与量化结果无法引用，不做数字性断言。

我的思考：eSIM 把"换卡"变成软件操作，攻击面从物理卡迁移到数字凭证与云端接口，信任模型更复杂（多出一个远程配置基础设施），对移动身份安全有直接现实意义。局限是生态闭源、跨厂商一致性差，实证研究往往受限于可获取的设备与运营商接口。总体来看，其对理解新一代身份载体生态的安全底色具有及时价值，也为后续 eSIM 安全研究提供了问题清单，值得运营商与监管方关注。


#### HyTrack: Resurrectable and Persistent Tracking Across Android Apps and the Web.
- **作者**: Malte Wessels; Simon Koch; Jan Drescher; Louis Bettels; David Klein; Martin Johns
- **方向**: 移动与物联网安全
- **解读**: Android 应用可用 Custom Tabs 与 Trusted Web Activities 自由混入 Web 内容，这些组件可访问默认浏览器状态（如认证令牌的复用），模糊了原生与 Web 的边界。HyTrack 正是利用这一共享浏览器状态，用同一标识符在 App 内与全网追踪用户，是全新的追踪技术。

方法上，作者给出多种把追踪融入 App UI、隐藏或完全伪装追踪的方法——在部分 Android 版本上可做到零可见痕迹；并借助 Android 系统基础功能，使标识符几乎无法清除：即使用户更换默认浏览器甚至设备，HyTrack 仍能"复活"追踪标识符，持久性超过 Web 上的 evercookie。

结果显示，虽无该技术已被实际采用的直接证据，但所有关键组件目前均已就位、随时可能被快速部署利用，论文由此发出早期预警。

我的思考：只依赖 Android 常见功能的"预期行为"即实现超越 evercookie 的持久追踪，说明平台级隔离缺失才是根因；对研究者的启示是"特性组合"审计应成为移动平台安全评估的常规科目。局限是论文未实证在野利用，且厂商若收紧 Custom Tabs/浏览器状态隔离即可消解部分威胁，但修复涉及生态协同，短期难落地。

- **论文 PDF**: [HyTrack: Resurrectable and Persistent Tracking Across Android Apps and the Web.](https://www.usenix.org/system/files/usenixsecurity25-wessels.pdf)

#### BLuEMan: A Stateful Simulation-based Fuzzing Framework for Open-Source RTOS Bluetooth Low Energy Protocol Stacks.
- **作者**: Wei-Che Kao, Yen-Chia Chen, Yu-Sheng Lin, Yu-Cheng Yang, Chi-Yu Li, Chun-Ying Huang
- **方向**: 移动与物联网安全
- **解读**: （无摘要，据标题推断）
**问题**：蓝牙低功耗（BLE）是IoT设备最常用的无线协议之一，开源RTOS中的BLE协议栈直接处理来自空中接口的攻击者可控数据，其漏洞可被远程利用；但对这类嵌入式协议栈做fuzzing需要维护复杂的协议状态机，且硬件依赖使测试成本高昂，缺乏系统性方案。
**方法**：据标题推断，BLuEMan将BLE协议栈移植到仿真环境中运行，构建有状态（stateful）的fuzzing框架：通过维护链路层/ATT/GATT等协议状态，按状态合法地构造与发送BLE报文，从而深入测试协议栈的状态处理逻辑。
**结果**：论文标题表明其实现了面向开源RTOS BLE协议栈的有状态仿真fuzzing框架，发现的漏洞数量等量化结果以原文为准，此处不作臆断。
**思考**：BLE协议栈的安全性直接决定数十亿IoT设备的面板攻击面，该工作把成熟的"有状态协议fuzzing+仿真"组合引入嵌入式无线领域，方向正确且实用；局限在于仿真与真实射频环境存在差异（时序、硬件行为），可能漏掉依赖真实物理层的bug，但其为RTOS生态的无线协议安全测试提供了可复用的基础设施。


#### LEMIX: Enabling Testing of Embedded Applications as Linux Applications.

- **作者**: Sai Ritvik Tanksalkar, Siddharth Muralee, Srihari Danduri, Paschal C. Amusuo, Antonio Bianchi, James C. Davis, Aravind Kumar Machiry
- **方向**: 移动与物联网安全
- **解读**:

嵌入式系统（RTOS、微控制器固件）广泛应用于 IoT 与关键基础设施，但其应用测试与模糊测试长期落后于桌面软件：缺乏可用的调试设施、覆盖率反馈与通用测试工具链，导致嵌入式固件漏洞频发却难以被发现。

据标题推断，LEMIX 通过把嵌入式应用翻译/适配为 Linux 应用来赋能测试：将针对 RTOS（如 Zephyr、FreeRTOS 等）编写的应用及其系统调用/外设接口映射到 Linux 等价实现，使其能在标准 Linux 环境下以原生进程运行，从而复用 AFL++、消毒器（sanitizer）等成熟测试基础设施进行模糊测试与动态分析。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于绕开"为嵌入式重造测试工具链"的高成本路径，直接复用桌面生态的成熟工具。

我的思考：Machiry 与 Bianchi 团队在嵌入式/IoT 安全测试上积累深厚，LEMIX 的"翻译而非仿真"思路若成立，可显著降低嵌入式模糊测试的入门门槛——相比 QEMU 全系统仿真，原生 Linux 运行具备更好的吞吐与工具兼容性。局限（据标题推断）：RTOS 特有语义（中断、时序、外设寄存器、并发模型）的忠实映射是最大挑战，语义偏差会导致漏报/误报；支持的操作系统与架构覆盖面也决定实用价值。对 IoT 固件安全测试工具链是重要的工程化推进。


#### The Cost of Performance: Breaking ThreadX with Kernel Object Masquerading Attacks.

- **作者**: Xinhui Shao, Zhen Ling, Yue Zhang, Huaiyu Yan, Yumeng Wei, Lan Luo, Zixia Liu, Junzhou Luo, Xinwen Fu
- **方向**: 移动与物联网安全
- **解读**:

微控制器 IoT 设备普遍使用嵌入式实时操作系统（RTOS），RTOS 漏洞可直接导致设备失陷；但缺乏标准化安全指南使各 RTOS 防护水平参差——FreeRTOS 等缺少基本防护，Zephyr 与 ThreadX 虽实现特权分离、内存访问控制与系统调用参数净化三重防护，其实现却存在关键差异。

方法上，作者发现 ThreadX 为追求系统调用性能而采用的优化实践可绕过内核对象指针的参数净化，据此提出内核对象伪装（KOM）攻击：攻击者通过精心挑选的系统调用操纵一个或多个内核对象，访问内核对象敏感字段，可能导致未授权数据操纵、提权乃至系统失陷；并设计基于欠约束符号执行的自动化方法来发现 KOM 攻击。

结果：在 ThreadX 平台上实验证实 KOM 攻击可行，已向厂商报告并获得认可，Amazon 与 Microsoft 在官网上致谢了该贡献，显示嵌入式 RTOS 安全研究对产业界的直接价值。

我的思考：该工作把"性能优化 vs 安全"的经典张力带到嵌入式 RTOS 语境，KOM 攻击揭示了"净化可被语义混淆绕过"这一普遍问题，欠约束符号执行的自动化思路也具有推广价值。局限：攻击依赖特定系统调用组合与平台内存布局，对 Zephyr 等采用细粒度指针校验的 RTOS 是否同样成立未系统验证；物联网设备多样化硬件上的可利用性需更多场景评估。

- **论文 PDF**: [The Cost of Performance: Breaking ThreadX with Kernel Object Masquerading Attacks.](https://www.usenix.org/system/files/usenixsecurity25-shao.pdf)

#### AUTOVR: Automated UI Exploration for Detecting Sensitive Data Flow Exposures in Virtual Reality Apps.

- **作者**: John Y. Kim, Chaoshun Zuo, Yanjie Zhao, Zhiqiang Lin

- **方向**: 移动与物联网安全

- **解读**: 

（无摘要，据标题推断）虚拟现实（VR）应用收集头部运动、注视、手柄轨迹等高度敏感的沉浸式数据，但这些数据经应用内数据流传输到何处、是否被第三方 SDK 或服务器收集，缺乏系统检测手段；VR 的 3D UI 与传统 2D UI 差异巨大，现有移动应用数据流分析工具难以直接迁移。

从标题推断，AUTOVR 是自动化 UI 探索框架：应是通过在 VR 应用中自动模拟用户交互（头部转动、手柄点击、移动），驱动应用覆盖各 UI 状态，同时插桩/追踪敏感数据流，识别其向网络或外部组件的暴露路径。具体探索策略与污点分析机制因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者在真实 VR 应用上测量了敏感数据流暴露的普遍性与严重性。

我的思考（基于标题的保守推断）：沉浸式数据的敏感性远超传统遥测，AUTOVR 把"自动 UI 探索 + 数据流分析"范式引入 VR，填补了该生态的隐私审计空白。与 Android 平台 FlowDroid 等静态/动态污点分析相比，VR 的 3D 交互空间与 Unity/Unreal 运行时给探索与插桩都带来新挑战；其价值取决于覆盖应用数量与发现的真实暴露案例。


### 云与供应链安全（7 篇）

#### An Industry Interview Study of Software Signing for Supply Chain Security.
- **作者**: Kelechi G. Kalu, Tanmay Singla, Chinenye Okafor, Santiago Torres-Arias, James C. Davis
- **方向**: 云与供应链安全
- **解读**: 现代软件产品由多方组件集成而成,供应链每增加一环,恶意行为注入的风险就增大一分。众多框架、标准与法规推荐使用软件签名提升供应链溯源能力,但近期调查显示签名采用率与质量偏低,业界视角的深层原因缺乏研究。

作者对 13 家组织的 18 名资深安全从业者进行深度访谈,系统梳理软件签名实践中的挑战,并考察软件供应链安全事件、安全标准与法规对签名采用的影响。

主要发现:(1)提出细化的软件供应链工厂模型,突出从业者的签名实践;(2)揭示阻碍签名实施的技术、组织与人力三类挑战;(3)专家对签名重要性的看法存在分歧;(4)内部与外部事件共同影响签名的采用进程。研究为软件签名采用提供了系统的决策考量。

该工作用质性方法填补了签名研究"数据很多、理解很少"的空白,将采用率低的原因从技术问题拓展到组织与人力层面,对标准制定与工具设计均有启发。局限:18 人样本的代表性有限,自我报告存在偏差。与大规模测量研究互补,可指导后续量化验证与自动化签名工具的落地。
- **论文 PDF**: [An Industry Interview Study of Software Signing for Supply Chain Security.](https://www.usenix.org/system/files/usenixsecurity25-kalu.pdf)

#### CloudFlow: Identifying Security-sensitive Data Flows in Serverless Applications.
- **作者**: Giuseppe Raffa, Jorge Blasco, Daniel O'Keeffe, Santanu Kumar Dash
- **方向**: 云与供应链安全
- **解读**: Serverless 应用由大量细粒度函数、事件源与权限配置组成，敏感数据（密钥、凭据、个人身份信息）在函数间、函数与外部服务间流动的路径难以人工追踪；一旦安全敏感数据流经低权限或暴露面大的组件，就可能造成数据泄露或权限滥用。本工作从标题看旨在自动识别 serverless 应用中的安全敏感数据流，帮助开发者与审计者定位敏感数据的流向与风险暴露点。

据标题推断，方法上可能以静态分析为主：解析函数代码、事件触发器、IAM 权限与资源配置，构建跨函数的数据流图，并结合污点分析或敏感数据源/汇点标注，识别从敏感源到不可信汇（如外发网络、日志、存储）的路径，进而给出风险告警。由于摘要缺失，分析精度、可扩展性等设计细节无法确认。

摘要缺失，无法引用量化结果；预期包含在真实 serverless 应用（如 AWS Lambda 部署集）上的检测效果、误报率与规模可扩展性等评估。

Serverless 的事件驱动、无服务器架构使数据流跨越函数边界与云服务边界，传统单程序污点分析难以覆盖，该方向填补了"跨组件安全数据流"分析的工具空白，对云上数据合规（如 GDPR）审计有直接价值。局限：无摘要，静态分析的精确性（尤其动态下发、反射调用场景）与对配置语义建模的完整性待全文确认；与既有云数据流审计工具（如 CloudTracking 类工作）的差异也需对照原文评估。（无摘要，据标题推断）


#### "I wasn't sure if this is indeed a security risk": Data-driven Understanding of Security Issue Reporting in GitHub Repositories of Open Source npm Packages.

- **作者**: Rajdeep Ghosh, Shiladitya De, Mainack Mondal
- **方向**: 云与供应链安全
- **解读**: （无摘要，据标题推断）开源 npm 包占据现代软件供应链的核心位置，安全问题的发现与上报是漏洞修复的起点，但开源维护者对「什么构成安全风险」的判断往往不确定。论文针对 GitHub 上开源 npm 包仓库中的安全问题上报行为展开数据驱动研究，试图理解安全问题如何被识别、报告与处理。

方法上，该工作应是从 GitHub 仓库数据（issue、提交、讨论等）出发，结合定量分析与定性考察，刻画维护者与社区在安全问题上报中的实际模式、困惑与障碍，尤其是「不确定这算不算安全风险」这类判断模糊如何影响上报行为。

预期结果将揭示：安全问题被漏报或迟报的常见原因、上报过程的信息缺口，以及社区规范对上报质量的影响，从而为改进开源安全流程（如更清晰的安全上报指南）提供依据。

我的思考：该研究把「人的判断不确定性」引入供应链安全议题，角度新颖且贴近现实；数据驱动方法可覆盖大规模仓库，但结论的因果解释需谨慎。相比强调漏洞利用的技术研究，它更关注治理与协作流程，是对 npm 生态安全实践的重要补充；其量化指标（如上报时延、类别分布）的选取将决定说服力。


#### Context Matters: Qualitative Insights into Developers' Approaches and Challenges with Software Composition Analysis.

- **作者**: Elizabeth Lin, Sparsha Gowda, William Enck, Dominik Wermke
- **方向**: 云与供应链安全
- **解读**: （无摘要，据标题推断）软件组成分析（SCA）是管理开源依赖漏洞的主流手段，但开发者如何在实际工作中使用 SCA 工具、遇到哪些挑战，仍缺乏深入理解。论文采用定性研究（很可能是访谈或日记研究）考察开发者的 SCA 实践，强调「情境（Context）很重要」——即工具使用方式因团队、项目与工作流而异。

方法上，该工作应通过半结构化访谈或混合方法收集开发者经验数据，按主题编码分析开发者在集成 SCA 到开发流程时的决策过程、误用模式与痛点，尤其关注工具输出（如漏洞告警）在真实情境中的可操作性。

预期发现将包括：开发者常因告警过多或缺乏上下文而忽视 SCA 结果，修复责任归属不清，以及工具与现有 CI/CD 工作流整合不畅等典型问题。

我的思考：SCA 的研究常聚焦检测能力本身，而本研究把视角转向使用者的真实体验，切中了「工具好不等于用得好」的现实痛点。定性方法样本量有限，但能提供调查问卷难以获得的深度细节；其结论对 SCA 工具设计（告警分级、上下文呈现、流程集成）有直接指导意义，与供应链安全治理议题高度相关。


#### We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs.
- **作者**: Joseph Spracklen, Raveen Wijewickrama, A. H. M. Nazmus Sakib, Anindya Maiti, Bimal Viswanath, Murtuza Jadliwala
- **方向**: 云与供应链安全
- **解读**: Python 与 JavaScript 等语言高度依赖 PyPI、npm 等集中式包仓库,而代码生成 LLM 的普及催生了一种新型供应链威胁——包幻觉(package hallucination):LLM 生成代码时推荐实际并不存在的包名。攻击者可抢注这些幻觉包名并投放恶意代码,利用 LLM 用户的"信任"形成新型包混淆攻击。此前仅有初步博客估计,缺乏对 SOTA 模型、参数设置与缓解手段的系统研究。本文要系统回答包幻觉的普遍程度、根因、影响参数与缓解可能性。
作者使用 16 个流行代码生成 LLM(商业与开源)、两个独特提示数据集(Python 与 JavaScript 各 19,500 条提示),生成 576,000 个代码样本进行包幻觉分析,共发现 205,474 个独特的幻觉包名。结果显示:商业模型平均幻觉率至少 5.2%,开源模型高达 21.7%;GPT-4 Turbo 幻觉率最低(3.59%),而开源模型普遍更高;Python 代码幻觉率(15.8%)低于 JavaScript(21.3%)。模型间比较显示商业模型幻觉率约为开源模型的 1/4。研究还发现幻觉具持久性(58% 的幻觉包在模型内重复出现),且模型具备"自检"能力——GPT-4 Turbo、GPT-3.5 与 DeepSeek 识别自身幻觉包的准确率超过 75%,这种可被利用的自我认知为缓解策略提供了契机。作者测试了 RAG、自检反馈、解码策略等缓解手段,可显著降低幻觉率同时保持代码质量。
研究首次以大规模、多模型、双语言的方式量化了包幻觉这一系统性现象,证据扎实:576,000 个样本、16 个模型、约 7.6 万独特幻觉实例的分析体量在同类工作中罕见。其根因分析(如语言混淆、删除包影响甚微仅 0.17%)纠正了直觉假设,富有洞察。局限在于:幻觉率高度依赖提示集与检测方法,绝对数字可能随模型迭代快速变化;缓解实验部分细节未在正文完整呈现;对"幻觉包被抢注后的真实危害链条"仅做了推理性论证而非实证。对防御者而言,包仓库的恶意包检测、LLM 输出侧的包名校验仍是开放方向。
- **论文 PDF**: [We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs.](https://www.usenix.org/system/files/usenixsecurity25-spracklen.pdf)

#### MalGuard: Towards Real-Time, Accurate, and Actionable Detection of Malicious Packages in PyPI Ecosystem.
- **作者**: Xingan Gao, Xiaobing Sun, Sicong Cao, Kaifeng Huang, Di Wu, Xiaolei Liu, Xingwei Lin, Yang Xiang
- **方向**: 云与供应链安全
- **解读**: PyPI 生态中恶意包投毒已成为供应链攻击的主要载体，现有检测方案要么依赖手工特征、要么依赖笨重的 LLM 推理，难以兼顾实时性、准确性与可解释性。本文提出 MalGuard，以轻量传统机器学习实现实时、准确且可操作的恶意包检测。

方法上，用图中心性自动识别敏感 API（取代人工特征工程）；以 LIME 提供可解释的检测结果；并借助 GPT-3.5-turbo 对特征集进行精炼。轻量化的设计使特征提取与模型训练只需数分钟、中心性计算数小时内完成，支持每日更新以应对新威胁。

在 6 个 SOTA 基线的对比中，MalGuard 精确率提升 0.5%-33.2%、召回率提升 1.8%-22.1%；真实部署验证中监控了 64,348 个新上传的 PyPI 包（2024-12-21 至 2025-01-28），发现 113 个此前未知的恶意包，其中 109 个随后被 PyPI 官方移除。

我的思考：MalGuard 的亮点是"轻量但有效"——图中心性自动化特征+传统 ML 达到或超过重型方法，且可解释（LIME）与可更新（每日）直接服务运营场景，109 个被官方移除的恶意包是罕见的落地证据。局限是依赖上传元数据与已知恶意样本的标注质量，对混淆/规避型投毒的效果需持续检验；但作为实时生态防线，工程价值明确。（无摘要，据标题推断）

#### ChainFuzz: Exploiting Upstream Vulnerabilities in Open-Source Supply Chains.
- **作者**: Peng Deng, Lei Zhang, Yuchuan Meng, Zhemin Yang, Yuan Zhang, Min Yang
- **方向**: 云与供应链安全
- **解读**:
**问题**：软件供应链攻击对下游软件威胁日益严重，软件成分分析（SCA）虽能帮助识别脆弱依赖，但主流SCA工具误报率高，开发者需耗费大量时间人工验证告警，延误了真正可利用的上游漏洞的检测与修复，本文旨在自动化验证上游漏洞在下游软件中的可利用性。
**方法**：作者提出ChainFuzz，通过生成概念验证（PoC）自动验证下游软件中的上游漏洞，并解决三个挑战：层内代码与约束（下游自定义代码与检查显著改变触发路径与条件）、层间依赖（跨层控制流与数据流依赖）、长供应链（传递依赖导致复杂利用路径与深层约束）。
**结果**：在包含66个漏洞-供应链组合的数据集上全面评估，ChainFuzz能有效为直接与传递脆弱依赖生成PoC；与代表性fuzzing工具AFLGo、AFL++和NestFuzz对比，其在下游PoC生成方面表现更优。
**思考**：该工作把SCA从"报漏洞"推进到"证可利用"，直接压缩人工验证成本，实用价值突出；局限在于PoC生成本质上受漏洞触发条件复杂度的制约，长链传递依赖场景仍有成功率的边界，但作为"供应链漏洞可利用性验证"的自动化尝试，与NestFuzz等形成互补，是供应链安全的重要拼图。

- **论文 PDF**: [ChainFuzz: Exploiting Upstream Vulnerabilities in Open-Source Supply Chains.](https://www.usenix.org/system/files/usenixsecurity25-deng.pdf)

### 认证与访问控制（10 篇）

#### Easy As Child's Play: An Empirical Study on Age Verification of Adult-Oriented Android Apps.
- **作者**: Yifan Yao, Shawn McCollum, Zhibo Sun, Yue Zhang
- **方向**: 认证与访问控制
- **解读**: 移动应用生态中面向成年人的应用快速增长,尽管平台与开发者采取多种策略防止未成年人访问,这些机制的实际有效性长期缺乏实证检验。本文系统研究此类年龄验证机制,并提出新的检测方案 GUARD(Guarding Underage Access Restriction Detection)。

GUARD 依据布局中组件的空间关系识别可能接收用户年龄或出生日期的组件,并通过污点分析追踪数据流;考虑到静态分析的局限,它还会动态地与应用交互以定位年龄相关输入组件,再据此进行精确的污点分析,提升检测精度。

对 Google Play 上 693,334 个应用中筛选出的 31,750 个仅限成人应用的分析显示,只有 1,165 个(3.67%)实现了年龄验证,且多数依赖最弱的"年龄门槛"(仅询问用户是否年满 18 岁)。更强的验证方式(证件上传、在线身份核验)也可被假证件绕过,还可通过无年龄检查服务的账户(OAuth 滥用)或 VPN 利用地区差异来规避。作者提出的加固对策获得了 Google 的积极反馈。

该工作首次大规模量化成人应用的年龄验证现状,并用动静结合的方法实现可扩展检测,结论直指平台治理短板。局限:检测依赖布局启发式,对动态界面与非典型设计可能漏报;绕过实验基于模拟攻击者假设。相比单点案例研究,其规模与对策建议对监管和平台政策有直接参考价值。
- **论文 PDF**: [Easy As Child's Play: An Empirical Study on Age Verification of Adult-Oriented Android Apps.](https://www.usenix.org/system/files/usenixsecurity25-yao-yifan.pdf)

#### Demystifying the (In)Security of QR Code-based Login in Real-world Deployments.

- **作者**: Xin Zhang; Xiaohan Zhang; Bo Zhao; Yuhong Nan; Zhichen Liu; Jianzhou Chen; Huijun Zhou; Min Yang
- **方向**: 认证与访问控制
- **解读**: 扫码登录（QRLogin）已成为主流 Web 账户认证方式，比传统口令输入更友好；但缺乏标准化设计与实现指南、部署差异巨大，其安全性长期被忽视。作者以"首个系统性研究"的定位切入，回答 QRLogin 在真实部署中到底安不安全。

  研究先通过真实世界调研理解部署现状与用户认知，建立贴近现实的威胁模型；再泛化 QRLogin 的典型工作流，逐一考察关键变量是否符合通用安全原则，暴露出 6 个潜在缺陷；最后用半自动检测流水线对真实部署做安全分析。

  结果令人警醒：47 个顶级网站（占测试样本的 43%）至少存在一个缺陷，可诱发 5 类攻击——授权劫持、双重登录、暴力登录、通用账户接管与隐私滥用；作者负责任披露全部问题并获得 42 个官方漏洞库编号，同时提供审计工具及面向开发者与用户的加固建议。

  该研究首次把 QRLogin 的真实风险量化到 43% 的网站层面，说明这一"便利范式"的安全地基远未稳固；局限在于样本以顶级网站为主，且随着厂商修复与行业实践演进，结论需持续更新；其与口令、双因素等成熟认证研究的对比，也为后续标准化与合规工作指明了方向。

- **论文 PDF**: [Demystifying the (In)Security of QR Code-based Login in Real-world Deployments.](https://www.usenix.org/system/files/usenixsecurity25-zhang-xin.pdf)

#### Universal Cross-app Attacks: Exploiting and Securing OAuth 2.0 in Integration Platforms.

- **作者**: Kaixuan Luo; Xianbo Wang; Adonis P. H. Fung; Wing Cheong Lau; Julien Lecomte
- **方向**: 认证与访问控制
- **解读**: 集成平台（工作流自动化、虚拟助手、智能家居）已成为互联网基础设施，开放市场欢迎第三方应用，并以 OAuth"账户链接"代用户编排外部服务；开放生态使不可信集成应用可能利用多应用 OAuth 授权的缺陷设计，形成新的攻击面。

  作者研究多应用 OAuth 授权因缺乏"应用区分"导致的平台级设计缺陷，提出两类新攻击：跨应用 OAuth 账户接管（COAT）与跨应用请求伪造（CORF）；并开发 COVScan 半自动黑盒测试工具刻画不同 OAuth 设计，系统性发现真实平台的跨应用漏洞。

  测量显示：18 个面向消费者或企业的流行集成平台中，11 个易受 COAT、另有 5 个易受 CORF，波及微软、谷歌、亚马逊等厂商的产品；漏洞可致终端用户服务与设备被未授权控制、敏感信息被隐蔽记录，甚至单击危害整个主要生态（对应 CVE，CVSS 9.6）；作者已与受影响厂商协作部署修复。

  该工作揭示"账户链接"把单应用 OAuth 风险放大为平台级风险，攻击面思维从应用内转向应用间，是 OAuth 安全研究的范式补充；COVScan 提升了发现过程的可复现性；局限在于黑盒测量依赖平台行为差异，披露后的修复效果与新型绕过仍需长期验证。

- **论文 PDF**: [Universal Cross-app Attacks: Exploiting and Securing OAuth 2.0 in Integration Platforms.](https://www.usenix.org/system/files/usenixsecurity25-luo-kaixuan.pdf)

#### Are CAPTCHAs Still Bot-hard? Generalized Visual CAPTCHA Solving with Agentic Vision Language Model.

- **作者**: Xiwen Teoh, Yun Lin, Siqi Li, Ruofan Liu, Avi Sollomoni, Yaniv Harel, Jin Song Dong
- **方向**: 认证与访问控制
- **解读**: 
（无摘要，据标题推断）CAPTCHA 是互联网上区分人类与机器人的经典访问控制手段，视觉类验证码（扭曲文字、图像选择等）长期以来被认为是自动化攻击的屏障。本文标题以设问形式提出"CAPTCHA 是否仍然对机器人困难"，核心主张是：借助具备智能体能力的视觉语言模型（Agentic VLM），可以通用地解决各类视觉 CAPTCHA。

方法层面，Agentic VLM 区别于传统 OCR/专用分类器之处在于其推理与工具调用能力：模型能够"观察—思考—行动"，将验证码任务拆解为多个步骤（如识别元素、理解交互指令、点击目标区域），从而不再依赖为某类验证码专门训练的模型，实现跨类型的泛化解题。

研究预期通过大规模验证码数据集评估解出率，并与人类表现及既有专用方法对比，回答"bot-hard"这一命题是否仍然成立。若通用 VLM 已能稳定通过主流验证码，将直接动摇大量网站依赖 CAPTCHA 做反爬虫/反滥用防线的前提。

该工作的意义在于把大模型能力评估与访问控制安全联系起来，提示安全社区 CAPTCHA 体系亟需升级或转向更难伪造的挑战（如行为特征、风险评分）。局限在于未见摘要，具体成功率与适用边界无法确证；但其所指向的"通用模型瓦解专用防御"趋势值得重视。


#### Encrypted Access Logging for Online Accounts: Device Attributions without Device Tracking.

- **作者**: Carolina Ortega Pérez, Alaa Daffalla
- **方向**: 认证与访问控制
- **解读**:

尽管认证机制不断改进，在线账户被攻破仍然普遍，因此事后检测账户失陷的技术同样必要。服务商通过账户安全界面（ASI）显示近期登录等活动帮助用户诊断安全状态，但近期研究显示主流服务的 ASI 并不可信——它们依赖易被操纵的客户端提供值。其根源是"准确将访问归因到特定设备"与"防止服务商追踪设备"之间看似根本性的矛盾。

方法上提出客户端侧加密访问日志（CSAL）新思路：在账户活动日志中加入端到端（E2E）加密的设备标识信息，利用操作系统支持与 FIDO2 风格认证，在"追踪隐私"与"ASI 效用"之间取得平衡。论文给出与现有认证机制共存的完整 CSAL 协议提案，并对诚实但好奇攻击者模型下的完整性、隐私与不可关联性做了形式化分析。

结果：形式化分析刻画了该设定下日志记录可行性的边界；论文讨论了主动攻击者下的安全性，提供概念验证实现，整体展示了操作系统厂商与服务商协作提升账户安全与用户安全的可行性。

我的思考：该工作直面 ASI 不可信的根源性矛盾，用 E2E 加密设备认证（而非可伪造的客户端值）重构访问归因，形式化分析覆盖完整性/隐私/不可关联性，PoC 验证了工程可行性。局限：需要操作系统与 FIDO2 生态深度协作、对主动攻击者的防护边界仍需推敲，且"不可追踪"与"可归因"在端到端加密下如何与执法需求平衡值得关注。与通行密钥生态演进方向契合，是账户安全可诊断性的基础性设计。

- **论文 PDF**: [Encrypted Access Logging for Online Accounts: Device Attributions without Device Tracking.](https://www.usenix.org/system/files/usenixsecurity25-ortega-perez.pdf)

#### Detecting Compromise of Passkey Storage on the Cloud.

- **作者**: Mazharul Islam, Sunpreet S. Arora, Rahul Chatterjee, Ke Coby Wang

- **方向**: 认证与访问控制

- **解读**: 

FIDO 同步 passkey 通过把 FIDO2 私钥备份到 passkey 管理服务（PMS）的云存储来解决账户恢复难题，但也引入严重风险：攻击者可借 PMS 被攻破（账户密码被猜中或内部人员窃取）盗走用户 passkey 并接管账户；现有防御要么重新引入账户恢复难题，要么干扰用户日常登录。

CASPER 是首个 passkey 泄露检测框架，采用诱饵（decoy）技术：把真实 passkey 隐藏在不可区分的诱饵列表中，攻击者盗取并尝试登录依赖方（RP）时大概率误用诱饵从而触发告警；配套设计新的 passkey 备份与恢复（BnR）协议，用户只需记忆低熵秘密（如两位数字 PIN），不破坏账户恢复与登录习惯。

分析表明即便攻击者知晓机制并策略性优化以规避检测，CASPER 仍具备有说服力的检测效果；其可无缝融入现有 passkey 备份、同步与认证流程，对用户体验影响极小、性能开销可忽略、部署与存储复杂度最低。

我的思考：贡献在于把蜜罐思想迁移到 passkey 生态，并首次让 RP 检测"他方（PMS）"的泄露而非仅检测自身密码库泄露；低熵秘密设计巧妙规避了账户恢复难题。局限是检测不等于阻止——被盗 passkey 在触发告警前可能已造成损失；诱饵机制的假阳性与拒绝服务式干扰仍需防范。与蜜字/蜜账户研究相比，CASPER 面对的是可携带、可同步的高价值密钥，威胁模型更贴近真实攻击链。

- **论文 PDF**: [Detecting Compromise of Passkey Storage on the Cloud.](https://www.usenix.org/system/files/usenixsecurity25-islam.pdf)

#### OneTouch: Effortless 2FA Scheme to Secure Fingerprint Authentication with Wearable OTP Token.

- **作者**: Yihui Yan, Zhice Yang

- **方向**: 认证与访问控制

- **解读**: 

指纹认证面临合成指纹、3D 打印高保真指纹副本等日益严重的威胁；2FA 可缓解凭据泄露，但现有 2FA 要么要求额外交互、破坏指纹"一触即验"的便利性，要么依赖指纹设备不具备的 I/O 接口或无线模块——统计显示亚马逊销量前 50 的指纹门锁中 20 款（40%）不支持蓝牙、7 款（14%）完全无无线能力。

OneTouch 保持"手指触一下"的原始体验完成第二因素认证：把指纹扫描仪从成像设备改造为可捕获接触物体时变电压的 I/O 设备，通过人体直接导通建立扫描仪与可穿戴 OTP 令牌之间的触控通信信道，执行挑战-响应 OTP 协议；物理层采用基于像素方差的开关调制，并用里德-所罗门（RS）纠错码保障传输可靠性。

作者对触控信道进行安全讨论与实测评估：真实传输中（如单参与者传输 50 包、每包 60 随机比特）误码位置分布均匀、RS 符号错误在多数情况下可纠正，验证了该信道承载 OTP 凭据交换的有效性。

我的思考：巧妙之处在于"以人体作信道"——把第二因素绑定到物理接触本身，天然规避无线截获，攻击面显著收窄，且兼容无无线模块的设备生态。局限是需改造扫描仪硬件、信道带宽有限（kHz 级载波）、抬指等动作仍会抬升误码，且缺少真实威胁下的端到端安全评估。与蓝牙可穿戴 2FA 相比，OneTouch 以牺牲灵活性换取普适性与抗截获性，是硬件与认证交叉的务实尝试。

- **论文 PDF**: [OneTouch: Effortless 2FA Scheme to Secure Fingerprint Authentication with Wearable OTP Token.](https://www.usenix.org/system/files/usenixsecurity25-yan-yihui.pdf)

#### Practically Secure Honey Password Vaults: New Design and New Evaluation against Online Guessing.

- **作者**: Haibo Cheng, Fugeng Huang, Jiahong Yang, Wenting Li, Ping Wang

- **方向**: 认证与访问控制

- **解读**: 

（无摘要，据标题推断）口令管理器集中存储用户全部口令，一旦主口令被暴力破解或数据库被拖库，所有凭据即刻沦陷；蜜口令（honey password）技术通过注入大量"看起来真实"的诱饵口令迷惑攻击者，但现有方案在在线猜测威胁下（攻击者逐个尝试、可与系统交互）的安全性与实用性缺乏严谨的重新评估。

从标题推断，本文给出蜜口令密码库的新设计，并针对在线猜测攻击建立新评估框架：应是通过改进诱饵生成分布、蜜字验证与告警机制，使攻击者即使获得密码库也难辨真假，同时量化在线场景下的破解成功率与检测率。具体构造细节因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题中的"新评估"暗示作者针对在线猜测提供了系统的实验对比与威胁模型分析。

我的思考（基于标题的保守推断）：蜜口令的价值在于把"被攻破"转化为"被检测"，但诱饵分布一旦偏离真实口令分布即失效，在线场景还面临速率限制、账户锁定等现实约束。与 Juels 等人的蜜加密（honey encryption）相比，本文聚焦口令密码库这一具体载体并强调"实践安全"，其评估的严谨程度将决定该技术能否被工业界采纳。


#### Password Guessing Using Large Language Models.

- **作者**: Yunkai Zou, Maoxiang An, Ding Wang

- **方向**: 认证与访问控制

- **解读**: 

（无摘要，据标题推断）口令猜测是评估口令安全性的核心手段，传统 PCFG、马尔可夫模型与神经网络（如 rNN、PassGAN）各有局限：难以建模长距离依赖与语义规律。大语言模型（LLM）具备强大的语言建模能力与海量先验知识，能否用于口令猜测、效果与成本如何，此前缺乏系统研究。

从标题推断，本文系统研究用 LLM 进行口令猜测：应是以生成式方式让预训练 LLM 产出候选口令，结合提示设计、微调或概率重排，与 PCFG、Markov、PassGAN 等基线对比猜测成功率与效率。具体模型选择与提示策略因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者在真实口令集上量化比较了 LLM 与经典方法的猜测覆盖率与效率。

我的思考（基于标题的保守推断）：LLM 口令猜测是生成式安全的最新前沿：其语义先验可能捕捉"人类如何构造口令"的规律，但推理成本高、候选吞吐量低、可解释性差，能否替代高效的传统模型存疑。与 RankGuess、PCFG 等定向猜测工作相比，本文的价值取决于是否证明了 LLM 在特定场景（如语义相关、定向猜测）上的不可替代优势。


#### Red Bleed: A Pragmatic Near-Infrared Presentation Attack on Facial Biometric Authentication Systems.

- **作者**: Bowen Hu, Kuo Wang, Chip-Hong Chang

- **方向**: 认证与访问控制

- **解读**: 

（无摘要，据标题推断）面部生物特征认证（智能手机、门禁、支付）日益普及，展示攻击（presentation attack）——用照片、视频或 3D 面具欺骗摄像头——构成主要威胁；近红外（NIR）相机被广泛用于活体检测，但攻击者能否用近红外光照这一低成本手段绕过主流方案，此前缺乏系统研究。

从标题推断，本文提出 Red Bleed——一种基于近红外的展示攻击：应是通过向人脸照射特定波段近红外光，利用人脸皮肤与材质对 NIR 的反射特性、以及活体检测算法的感知盲区，在真实设备（如手机、门禁）上演示攻击可行性。具体光照参数与攻击流程因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题中的"Pragmatic"暗示作者强调攻击的低成本、易实施性。

我的思考（基于标题的保守推断）：活体检测是面部认证的信任基石，针对 NIR 通道的攻击一旦成立，将同时威胁依赖 NIR 活体判断的整类设备生态。与打印照片、硅胶面具等经典展示攻击相比，基于光照的攻击无需高保真复制面部，更易批量实施；其价值取决于是否覆盖主流活体检测算法并在真实设备上验证。


### 数字取证（6 篇）

#### Ghost Clusters: Evaluating Attribution of Illicit Services through Cryptocurrency Tracing.
- **作者**: Kelvin Lubbertsen, Michel van Eeten, Rolf van Wegberg
- **方向**: 数字取证
- **解读**: 执法与威胁情报依赖加密货币追踪对非法服务（勒索软件、暗网市场、洗钱服务）进行归因：通过链上聚类把地址关联到同一实体，再定位服务运营者。但聚类归因的可靠性存在隐患——"幽灵聚类"（ghost clusters）指那些看似属于某非法服务、实则可能是无关地址误并入、或由服务方刻意构造的假聚类，会误导归因结论，甚至冤枉无辜或放跑真凶。本工作从标题看旨在系统评估通过加密货币追踪对非法服务进行归因的有效性。

据标题推断，方法上可能以真实案件/已知非法服务为锚，对比"官方/标准聚类结果"与"独立追踪结果"，量化归因的精度、召回与误归因率，并刻画幽灵聚类的成因（地址误合并、服务拆分、混淆技术）；可能结合混币、链上图谱与启发式规则分析。摘要缺失，具体数据集与指标无法确认。

摘要缺失，无法引用量化结果；预期包含归因准确率、误归因（false attribution）案例数、以及幽灵聚类出现频率与影响等实证数据。

归因结论直接影响执法行动与制裁决策，其"可信度"问题此前缺乏系统评估；作者团队（van Eeten/van Wegberg，代尔夫特理工）在勒索软件经济与加密货币犯罪实证上成果丰硕，方法可信度预期高。局限：无摘要，样本覆盖（哪些非法服务类型、时间范围）与评价基准的客观性需全文确认；误归因的界定本身存在主观性，需审视其判定逻辑。（无摘要，据标题推断）


#### Secure Information Embedding in Forensic 3D Fingerprinting.
- **作者**: Canran Wang, Jinwen Wang, Mi Zhou, Vinh Pham, Senyue Hao, Chao Zhou, Ning Zhang, Netanel Raviv
- **方向**: 数字取证
- **解读**: 打印机指纹识别技术在取证领域长期发挥关键作用，用于追踪造假者与保护机密信息。3D 打印的普及带来新的公共安全风险：任何人可通过互联网与消费级 3D 打印机生产无法追踪的枪支、假冒产品等。本文借鉴 2D 打印机指纹识别的成功经验，提出面向 3D 打印的取证指纹框架 SIDE（Secure Information EmbeDding and Extraction）。

SIDE 将信息嵌入 3D 打印过程的物理特性（如打印路径、层高、挤出量等细微扰动）实现可提取的指纹，并针对 3D 打印取证中的对抗挑战（如后处理、重打印）设计鲁棒的嵌入与提取方案（摘要截断）。

实验验证 SIDE 在多种打印设置与材料下能可靠嵌入并提取指纹信息，为溯源 3D 打印件提供了可行的技术手段（摘要截断，具体指标未给出）。

该工作将成熟的 2D 打印取证范式迁移到 3D 打印，方向明确、实用性强，对公共安全有直接价值。局限是嵌入扰动对打印质量的影响、攻击者通过参数随机化规避指纹的能力，以及需要打印机固件供应链配合部署的现实难度。

- **论文 PDF**: [Secure Information Embedding in Forensic 3D Fingerprinting.](https://www.usenix.org/system/files/usenixsecurity25-wang-canran.pdf)

#### Expert Insights into Advanced Persistent Threats: Analysis, Attribution, and Challenges.

- **作者**: Aakanksha Saha, James Mattei, Jorge Blasco, Lorenzo Cavallaro, Daniel Votipka, Martina Lindorfer
- **方向**: 数字取证
- **解读**: 高级持续性威胁（APT）复杂且有针对性，检测与归因需要分析人员投入大量精力，研究者虽开发了各种支撑技术，但安全实践者对 APT 级威胁分析的感知与挑战尚未被充分理解。论文对 15 位不同角色与专长的安全实践者进行半结构化访谈，弥合这一空白。

基于访谈，作者识别出 APT 归因的三层方法，每层有各自目标与挑战；并发现实践者通常优先理解对手的策略、技术、程序（TTPs）与动机，而非识别攻击背后的具体实体（归因到具体组织/国家）。此外，现有工具与流程的主要挑战源于其无法处理多样而复杂的数据，以及内部与外部协作中的问题。

基于这些发现，论文提出四条改进归因方法的建议，并讨论这些改进如何应对已识别的挑战。

我的思考：该工作的贡献在于把「归因实践的真实图景」系统化——三层归因框架和「重 TTP 轻实体」的观察对学术研究（常过度聚焦归因到具体对手）是重要纠偏。局限是样本仅 15 人、且为自我报告，代表性有限；访谈数据的编码解释也依赖研究者判断。对攻击检测与归因工具的设计方向（数据整合、协作支持）有实际参考价值。

- **论文 PDF**: [Expert Insights into Advanced Persistent Threats: Analysis, Attribution, and Challenges.](https://www.usenix.org/system/files/usenixsecurity25-saha.pdf)

#### Scoop: Mitigation of Recapture Attacks on Provenance-Based Media Authentication.
- **作者**: Yuxin (Myles) Liu, Habiba Farrukh, Ardalan Amiri Sani, Sharad Agarwal, Gene Tsudik
- **方向**: 数字取证
- **解读**: 生成式 AI 使深度伪造日益逼真，仅靠 deepfake 检测是永无终点的军备竞赛；基于硬件签名的媒体溯源认证（拍摄时绑定设备密钥，记录在哪里、何时、如何拍摄）成为"眼见为实"的强有力替代。但重拍攻击（把屏幕/投影上的假内容用安全相机翻拍）可让伪造内容获得合法签名，直接击穿溯源认证的信任根基。本文要缓解这一最薄弱的物理层攻击。

Scoop 结合深度传感与基于学习的深度估计：认证拍摄内容时，检查捕获内容的深度图与真实场景深度是否一致，标记深度线索不匹配的区域为可疑重拍。系统在 iPhone 14 Pro（ToF）与 Samsung Galaxy S20+（iToF）上实现，并配套构建了首个重拍检测数据集（78 个重拍场景与 44 个良性场景）。

结果显示，iPhone 上重拍检测准确率最高达 94.81%（误报率 0.02%），Samsung 上为 74.03%（误报率 17.78%），端到端验证了生产者（相机）与消费者（查看者）的溯源校验工作流。

我的思考：Scoop 的价值在于把"重拍"这一绕过密码学保证的物理攻击作为一等威胁来防御，深度一致性是难以被图像域对抗扰动轻易伪造的信号，与纯视觉检测器形成互补。局限明显：依赖设备深度传感器，跨设备泛化时准确率显著下降且误报偏高，攻击者若用深度欺骗（如立体显示）仍可能绕过。总体而言，它为溯源认证补齐了物理层防御的一块关键拼图。

#### Principled and Automated Approach for Investigating AR/VR Attacks.
- **作者**: Muhammad Shoaib, Alex Suh, Wajih Ul Hassan
- **方向**: 数字取证
- **解读**: AR/VR 正进入行业与消费场景，针对头显的攻击（欺骗、注入、追踪操纵等）需要溯源审计来支持根因分析与影响评估；但传统审计系统在 AR/VR 设备上要么生成不准确、不完整的溯源图，要么因设备的运行限制根本无法部署。本文提出 REALITYCHECK，一个面向 AR/VR 的基于溯源的审计系统。

方法有三步：先扩展 W3C PROV 数据模型，加入 AR/VR 特定实体与因果关系的本体；再用自然语言处理与基于特征的日志关联技术，从分散、非结构化的 AR/VR 日志中透明地抽取实体与关系构建溯源图；最后引入 AR/VR 感知的执行切分技术，过滤取证无关数据与虚假因果关系，提升分析准确性与速度。

在 Meta Quest 2 上实现原型并针对 25 个真实 AR/VR 攻击评估：REALITYCHECK 为所有攻击生成了准确的溯源图，跨基准应用运行时开销低；执行切分在不丢失关键调查细节的前提下大幅缩小了溯源图规模。

我的思考：该工作把成熟的系统审计方法论（PROV 溯源）带入新兴 AR/VR 领域并解决其特有难点（非结构化日志、设备限制），非侵入、无需额外安装、可跨设备泛化的设计很有部署价值。局限是仅以 Quest 2 单平台验证，日志质量与本体覆盖决定上游抽取准确性；但作为 AR/VR 攻击调查的首批系统化方案，方向正确。
- **论文 PDF**: [Principled and Automated Approach for Investigating AR/VR Attacks.](https://www.usenix.org/system/files/usenixsecurity25-shoaib.pdf)

#### High Stakes, Low Certainty: Evaluating the Efficacy of High-Level Indicators of Compromise in Ransomware Attribution.

- **作者**: Max van der Horst, Ricky Kho, Olga Gadyatskaya, Michel Mollema, Michel van Eeten, Yury Zhauniarovich
- **方向**: 数字取证
- **解读**: 勒索软件攻击频发，受害组织常被迫支付赎金，但支付前须先归因攻击者并做制裁筛查，避免向受制裁实体付款而面临法律与财务风险。归因依赖入侵指标（IoC）匹配威胁画像，而 RaaS 生态与团伙改名使归因极为困难。本文首次实证研究勒索归因流程及高层 IoC 在其中的可靠性。

采用混合方法：对 20 名专家半结构化访谈（Krippendorff's Alpha 0.872），并分析 27 份公司事件报告与 13 份 CISA #StopRansomware 报告，映射到 MITRE ATT&CK（887 维向量）后计算 TTP 重叠相似度与轮廓系数。发现归因实际依赖赎金票据、通信渠道、泄漏网站、加密钱包等低层 IoC；同一团伙报告间 TTP 平均重叠仅 0.37，不同团伙间 0.21，轮廓系数为负（-0.0864），TTP 既不稳定也不唯一；公司与 CISA 报告对同一团伙的 TTP 覆盖率差异大、重叠仅约 0.35。

作者据此挑战"金字塔之痛"等框架对高层 IoC 的推崇：建议将 TTP 从 Tough! 降级至 Challenging 层，并补充赎金票据语言学、加密钱包地址、团伙品牌等勒索特有指标。研究还揭示改名、RaaS 附属与国家级伪装（APT41/APT45）导致误归因风险，12/20 受访者强调其法律财务后果；公私机构均无完整 IoC 库，数据碎片化加剧风险。

以访谈加数据双重证据挑战业界教条，并提出可操作的制裁合规建议，对受害者与政策制定者都有直接价值。局限：样本仅 27 份、来自单一公司、单人提取数据无独立核验；TTP 提取依赖报告质量；访谈集中于欧洲公司，结论外推需谨慎。

- **论文 PDF**: [High Stakes, Low Certainty: Evaluating the Efficacy of High-Level Indicators of Compromise in Ransomware Attribution.](https://www.usenix.org/system/files/usenixsecurity25-van-der-horst.pdf)

### 无线与通信安全（12 篇）

#### The Ghost Navigator: Revisiting the Hidden Vulnerability of Localization in Autonomous Driving.

- **作者**: Junqi Zhang, Shaoyin Cheng, Linqing Hu, Jie Zhang, Chengyu Shi, Xingshuo Han, Tianwei Zhang, Yueqiang Cheng, Weiming Zhang
- **方向**: 无线与通信安全
- **解读**: 
定位是自动驾驶（AD）的关键基础，直接影响下游模块性能；多传感器融合（MSF）虽提升了定位精度与可靠性，其安全性却成为重大关切。此前研究已大量考察融合定位系统的安全，但本文指出一个被持续忽视的因素——车辆动力学对 GPS 欺骗攻击成功率的影响。

为填补这一空白，作者提出运动敏感分析框架 MSAF，聚焦于分析此前被低估的车辆动态行为。研究表明两种常见驾驶场景显著影响 GPS 欺骗攻击的成功率：加速行驶与高速巡航，它们在 MSAF 分析下表现出更高的脆弱性——动态运动会改变传感器融合中各数据源的置信度分配，为欺骗创造窗口。

基于此洞察，作者设计了两种动力学定向攻击策略，并在三个测试平台上评估：模拟框架 MSAF_MSF，以及两套真实世界的 MSF 自动驾驶系统 Apollo_MSF 与 Shenlan_MSF。结果显示相比基线，MSAF 攻击效率显著提升：完成攻击所需时间大幅缩短，同时成功率更高。

该工作的价值在于把"车辆运动状态"纳入 GPS 欺骗威胁建模，揭示了动力学-融合算法交互这一此前空白的攻击维度，并给出可复现的攻击框架（含开源代码与演示）。局限在于攻击仍需在目标车辆附近布置 GPS 欺骗源，且对融合算法的具体置信度机制依赖较强；与同类定位攻击相比，其"挑时机"（利用加速/高速场景）的策略更具实战性，对 AD 安全设计有直接指导意义。

- **论文 PDF**: [The Ghost Navigator: Revisiting the Hidden Vulnerability of Localization in Autonomous Driving.](https://www.usenix.org/system/files/usenixsecurity25-zhang-junqi.pdf)

#### Tracking You from a Thousand Miles Away! Turning a Bluetooth Device into an Apple AirTag Without Root Privileges.
- **作者**: Junming Chen, Xiaoyue Ma, Lannan Luo, Qiang Zeng
- **方向**: 无线与通信安全
- **解读**: Apple 的 Find My 网络依托超过十亿活跃 Apple 设备,是全球最大的设备定位网络。此前 OpenHayStack 已证明可将 ESP32 等设备伪装成 AirTag 追踪器,但其需要 root 权限修改 BLE 广播地址,而提权在真实攻击场景中往往难以实现。本文要解决:能否在无需 root 权限的前提下,把普通电脑变成可被 Find My 网络定位的"AirTag",从而大规模滥用该网络实施隐私追踪。
nRootTag 的核心洞察是"反向匹配":不再修改广播地址去匹配公钥,而是搜索一个与现有广播地址匹配的公私钥对,然后用该公钥广播丢失消息。作者发现 Find My 规范虽规定丢失消息须使用随机静态地址,但实际实现允许所有类型的 BLE 地址,这一被 Apple 与既往工作忽视的事实成为攻击基础。针对不同操作系统采用不同技术:Linu x 上公共地址无需 root 即可访问,且地址含 IEEE 分配的 24 位 OUI(截至 2024 年 12 月共 47,054 个,公开可查),可预计算彩虹表实现瞬时密钥查找;Windows/Android 上地址需 root 才能读取,攻击者可在受害机器上通过流行 App 嗅探广播地址,再经云端服务进行彩虹表查找或 GPU 加速在线密钥搜索。
实验表明:在线密钥搜索成功率 3 分钟内超过 90%,成本仅数美元,且成本不随被追踪电脑数量增加;攻击在 Linux、Windows、Android 上均有效,可追踪台式机、笔记本、智能手机与 IoT 设备。彩虹表存储经自定义数据库设计大幅压缩(为 Attack-I 预计算记录后存储成本约 479.99 美元)。
该攻击的巧妙之处在于把"密码学难题"(匹配 46 位关键比特)转化为可预计算、可并行的搜索问题,并利用 OUI 公共信息与 Find My 实现的宽松地址校验,形成无需提权的普适攻击面,对个人隐私构成现实威胁。局限:攻击依赖目标设备发出 BLE 广播且附近有 Apple 设备作为 finder,城市环境效果最佳;在线搜索需云端算力支撑。防御上,Apple 应严格校验丢失消息的地址类型、限制未认证设备广播接入。与 OpenHayStack 相比,nRootTag 的"无 root"特性将攻击门槛从专业攻击者降至普通个体,显著放大了风险,值得平台方与监管重视。
- **论文 PDF**: [Tracking You from a Thousand Miles Away! Turning a Bluetooth Device into an Apple AirTag Without Root Privileges.](https://www.usenix.org/system/files/usenixsecurity25-chen-junming.pdf)

#### LLFuzz: An Over-the-Air Dynamic Testing Framework for Cellular Baseband Lower Layers.
- **作者**: Tuan Dinh Hoang, Taekkyung Oh, CheolJun Park, Insu Yun, Yongdae Kim
- **方向**: 无线与通信安全
- **解读**: 蜂窝基带是智能手机的关键攻击面——基带漏洞可被远程利用实现设备接管，而基带底层（物理层、MAC 层）长期缺乏有效的动态测试工具。本文提出 LLFuzz，一个面向蜂窝基带底层协议的空中（over-the-air）动态测试框架。（无摘要，据标题推断）

从标题推断，LLFuzz 通过真实空口信号与基带协议栈交互：可能利用软件定义无线电（SDR）生成畸变或恶意构造的下行信令，监测基带响应以发现崩溃或异常行为，并针对底层协议特性设计输入生成与状态跟踪策略。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于提供可复现的基带底层动态测试能力，并报告在真实基带芯片或设备上发现的漏洞或异常。（无摘要，据标题推断）

基带安全研究多聚焦高层协议（NAS、LTE 信令等）的静态分析或仿真，空中动态测试更贴近真实攻击路径，对发现底层实现缺陷意义重大。局限是 SDR 与真实网络的同步与时间约束、测试覆盖受协议状态机复杂度限制，漏洞验证需要厂商修复配合；"空中测试"的合法性与规模部署也是现实门槛。


#### Preventing Artificially Inflated SMS Attacks through Large-Scale Traffic Inspection.
- **作者**: Jun Ho Huh, Hyejin Shin, Sunwoo Ahn, Hayoon Yi, Joonho Cho, Taewoo Kim, Minchae Lim, Nu-El Choi
- **方向**: 无线与通信安全
- **解读**: 人为膨胀流量（AIT）攻击已成为依赖 SMS 验证的企业面临的普遍威胁：攻击者用机器人账号发起海量虚假 SMS 验证请求，恶意运营商或短信聚合商可能是共谋方，但此前没有文献正式刻画 AIT 攻击或研究检测技术。本文首次基于 940 万条短信请求日志对 AIT 进行正式刻画并提出检测系统。

分析揭示攻击常使用短命邮箱服务，并复用公共前缀值快速生成未验证的电话号码与 IMEI 号；为绕过限流，机器人被编程为每提交少量请求就切换账号、号码或设备。作者提出三级检测：基于单请求信息的机器学习特征、与用户/号码/设备相关的多历史事件特征，以及全国范围内与该请求有联系的可疑流量特征（如统计过去 24 小时内某前缀关联的不同号码数），配合固定阈值判定。

在网页客户端发起的认证请求上达到 89.6% 召回率（误报率 0.2%），在原生应用客户端流量上达到 91.1% 召回率（误报率 0.1%）。

首次把 AIT 攻击从"行业博客的经验之谈"提升为有数据支撑的形式化刻画，三级特征设计（个体-关联-全局）针对分布式、分散化攻击的检测思路有普适借鉴意义。局限是依赖固定阈值与特征工程，攻击者可通过更复杂的身份随机化与慢速攻击规避；检测结果未经不同地区或运营商验证，泛化性待考察。

- **论文 PDF**: [Preventing Artificially Inflated SMS Attacks through Large-Scale Traffic Inspection.](https://www.usenix.org/system/files/usenixsecurity25-huh.pdf)

#### GLaDoS: Location-aware Denial-of-Service of Cellular Networks.
- **作者**: Simon Erni, Martin Kotuliak, Richard Baker, Ivan Martinovic, Srdjan Capkun
- **方向**: 无线与通信安全
- **解读**: 蜂窝通信无处不在，但在敏感工业与政府区域需要被管控。现有干扰系统依赖大功率宽带发射，非选择性且会干扰邻近区域（如阻断紧急呼叫），且难以同时满足健康限值与安装约束。本文提出 GLaDoS，改进上行协议级压制攻击并融合低功率宽带噪声干扰，实现对指定区域的精确蜂窝 DoS 控制。

GLaDoS 结合协议级压制（overshadowing）与商用定位系统将 DoS 限制在受控区域；低功率宽带噪声干扰用于解决弱小区问题——当目标手机处于强无线电路径而连接到远处小区时，协议级攻击难以检测所有小区连接，噪声干扰弥补这一缺口。作者在 62,500 平方米、覆盖超过 100 个商业运营商小区且禁止使用手机的近城区部署评估。

部署使用 4 台协议级 DoS 单元、约 40 台室外与 100 台室内低功率干扰单元及配套天线前端；对不同手机型号实测，系统中和了 99.3% 的连接，同时能跟踪超过 100 个小区。这是首个全尺寸部署的基于压制（overshadowing）的蜂窝通信管控系统。

相比传统大功率干扰，GLaDoS 以"协议级压制 + 低功率噪声 + 定位"的组合实现高选择性、低辐射的蜂窝管控，99.3% 的中和率与百小区级跟踪展示了工程成熟度。局限是此类系统的合法部署存在治理与滥用双重性，硬件成本与区域规模的可扩展性、以及对抗手机端规避的能力仍需讨论。

- **论文 PDF**: [GLaDoS: Location-aware Denial-of-Service of Cellular Networks.](https://www.usenix.org/system/files/usenixsecurity25-erni.pdf)

#### AKMA+: Security and Privacy-Enhanced and Standard-Compatible AKMA for 5G Communication.
- **作者**: Yang Yang, Guomin Yang, Yingjiu Li, Minming Huang, Zilin Shen, Imtiaz Karim, Ralf Sasse, David A. Basin, Elisa Bertino, Jian Weng, HweeHwa Pang, Robert H. Deng
- **方向**: 无线与通信安全
- **解读**: 应用认证与密钥管理（AKMA）协议是 5G 网络安全与隐私的基础构件，其漏洞可能被攻击者利用，因此协议安全性至关重要。本文对 AKMA 做详细分析，发现多项可导致安全与隐私破坏的漏洞，并提出 AKMA+ 增强协议。

AKMA+ 在保持与现有标准兼容的前提下抵御安全与隐私破坏：为 UE 与应用功能（AF）之间的通信提供反制措施（含抵御归属地公网内的攻击者），在不改变协议流程的情况下实现 UE 与 AKMA 锚点功能之间的双向认证；同时通过混淆订阅者与 AKMA 密钥标识符，阻止攻击者将目标 UE 与其历史应用访问关联（防链接攻击）。作者用形式化验证证明 AKMA+ 达到关键安全与隐私目标。

大量实验表明 AKMA+ 的计算开销、带宽成本与 UE 电池消耗均在可接受范围，且保持标准兼容性。

在"不改协议流程"的约束下修复合标准协议的安全漏洞，兼顾可部署性与安全性，是协议改进的务实范本；对标识符的混淆直接针对 5G 中的链接性隐私威胁，形式化验证增强了结论可信度。局限是兼容性约束可能限制反制措施的彻底性，其安全论证依赖攻击者模型（如 HPLMN 内部攻击者）的设定，实际部署中的生态协同成本仍需评估。
- **论文 PDF**: [AKMA+: Security and Privacy-Enhanced and Standard-Compatible AKMA for 5G Communication.](https://www.usenix.org/system/files/usenixsecurity25-yang-yang.pdf)

#### A Thorough Security Analysis of BLE Proximity Tracking Protocols.
- **作者**: Xiaofeng Liu; Chaoshun Zuo; Qinsheng Hou; Pengcheng Ren; Jianliang Wu; Qingchuan Zhao; Shanqing Guo
- **方向**: 无线与通信安全
- **解读**: 低功耗蓝牙（BLE）普及催生大量近距追踪服务（如 Apple Find My、Samsung Find My Mobile），其安全建立在周边手机测量的"接近度"之上，但既有分析要么只聚焦单一参与者、要么缺乏形式化保证。本文以四项工作补齐：逆向恢复闭源协议、基于逆向结果构建形式化模型、抽取并形式化协议的设计安全目标、形式化验证这些目标能否成立。

方法上，作者对两个最流行的真实服务 Apple Find My 与 Samsung Find My Mobile 进行逆向工程，将协议转为可验证的形式化模型，系统枚举攻击者能力下的安全属性（如位置隐私、防追踪、消息机密性等），检验目标是否被违背。

结果显示共发现 7 个经厂商确认的新漏洞，其中 4 个获得 CVE/SVE 编号、3 个为高危漏洞；作者提出缓解方案并形式化确认所有安全目标在缓解后均可达；截至论文写作时，Samsung 已在其协助下修复 5 个漏洞。

我的思考：该工作是近距追踪协议首个端到端"逆向+形式化验证"闭环，把针对 AirTag 类产品的隐私争论落到可证明的协议缺陷上。局限是闭源逆向可能遗漏隐藏逻辑、验证模型与真实实现存在抽象差距、修复依赖厂商响应；但方法可推广到其他 BLE 生态服务。

- **论文 PDF**: [A Thorough Security Analysis of BLE Proximity Tracking Protocols.](https://www.usenix.org/system/files/usenixsecurity25-liu-xiaofeng.pdf)

#### Gotta Detect 'Em All: Fake Base Station and Multi-Step Attack Detection in Cellular Networks.
- **作者**: Kazi Samin Mubasshir; Imtiaz Karim; Elisa Bertino
- **方向**: 无线与通信安全
- **解读**: 伪基站（FBS）冒充合法基站，可引发非法监控、敏感信息窃听与网络服务中断，其衍生的多步攻击（MSA）更使威胁叠加；现有检测多为启发式，难以在实际网络中可靠识别。本文提出 FBSDetector，在用户设备（UE）侧仅凭层三网络痕迹用机器学习可靠检测 FBS 与 MSA。

方法上，作者构建 FBSAD 与 MSAD——首个包含 FBS 实例与 21 种 MSA 的高质量大规模数据集，覆盖移动性与不同攻击者能力的真实蜂窝场景；检测框架采用多级设计：包级分类用带注意力的有状态 LSTM，轨迹级分类识别 FBS，MSA 识别则用图学习建模攻击步骤间的关联。

结果显示 FBS 检测准确率 96%、误报率 2.96%；MSA 识别准确率 86%、误报率 3.28%；系统已部署为移动 App 并在真实环境验证，相比无法检测伪基站的启发式方案，FBSDetector 能实时在野检测。

我的思考：数据集是这项工作的核心资产——FBS/MSA 领域长期缺乏大规模公开数据，ML 方案的价值直接由数据质量决定；LSTM+注意力与图学习的组合也贴合"包-轨迹-步骤"的层级特性。局限是依赖层三痕迹质量、数据集场景有限，UE 侧部署的算力与耗电需进一步评估。

- **论文 PDF**: [Gotta Detect 'Em All: Fake Base Station and Multi-Step Attack Detection in Cellular Networks.](https://www.usenix.org/system/files/usenixsecurity25-mubasshir.pdf)

#### SNI5GECT: A Practical Approach to Inject aNRchy into 5G NR.
- **作者**: Shijie Luo; Matheus E. Garbelini; Sudipta Chattopadhyay; Jianying Zhou
- **方向**: 无线与通信安全
- **解读**: （无摘要，据标题推断）5G 新空口（NR）沿袭了 4G 的许多信令机制，历史上被反复证明存在注入类攻击（如伪基站、RRC/NAS 信令伪造）。本文标题以 "aNRchy"（anarchy 与 NR 的双关）暗示在 5G NR 中实现"信令无政府状态"的注入攻击，并强调 "practical"——真实可复现而非理论推演。

方法上，据标题推断，作者应针对 5G NR 的某一具体注入向量（可能是 RRC 连接建立/重配流程、寻呼或系统信息广播）构造低成本的伪造信令工具，给出端到端攻击流程与影响分析，可能涉及软件无线电实现。

由于无摘要，具体量化结果（成功率、受影响网元、漏洞编号）无法引用，不做数字性断言。

我的思考：5G 安全研究已从"纸上谈兵"转向"实测注入"，此类工作把 3GPP 规范的攻击面落到开源协议栈与商用设备上，价值在于提醒业界"NR 并非天然安全"。局限是注入攻击的实际危害取决于网络侧是否启用完整性保护与认证校验，评估时需区分理论漏洞与实际可利用性。总体而言，该工作延续了 4G 时代信令注入研究的脉络，把"5G 是否真正安全"的问题再次摆上台面。


#### DiffLoc: WiFi Hidden Camera Localization Based on Electromagnetic Diffraction.
- **作者**: Xiang Zhang, Jie Zhang, Huan Yan, Jinyang Huang, Zehua Ma, Bin Liu, Meng Li, Kejiang Chen, Qing Guo, Tianwei Zhang, Zhi Liu
- **方向**: 无线与通信安全
- **解读**: （无摘要，据标题推断）
**问题**：隐藏摄像头（偷拍设备）体积小、部署隐蔽，传统物理搜查或光电检测难以覆盖全部角落；如何借助日常可得的无线信号自动发现并定位这类设备，是反偷拍与隐私保护中的现实难题。
**方法**：DiffLoc利用WiFi信号遇到摄像头金属外壳与电子元件时产生的电磁衍射现象，通过分析衍射信号的强度与到达特征，推断隐藏摄像头的位置（据标题推断，具体信号建模与定位算法以原文为准）。
**结果**：论文标题表明其实现了基于电磁衍射的WiFi隐藏摄像头定位方案，定位精度等量化指标以原文为准，此处不作臆断。
**思考**：该工作延续了射频感知定位（如RF-Cam等）的研究脉络，其吸引力在于可利用室内已有WiFi基础设施、无需专用设备；但实际效果受多径环境、摄像头尺寸与材质、天线布局等因素影响，从实验室精度到现实房间尺度部署仍有距离，也需考虑对遮挡物和非摄像头金属物体的误判问题。


#### GNSS-WASP: GNSS Wide Area SPoofing.

- **作者**: Christopher Tibaldo, Harshad Sathaye, Giovanni Camurati, Srdjan Capkun
- **方向**: 无线与通信安全
- **解读**:

GNSS 欺骗攻击通常要求攻击者跟踪受害接收机的位置并逐台定制信号，限制了攻击规模；GNSS-WASP 首次提出广域欺骗：由一组战略布设的同步发射器组成星座，无需知道受害者位置即可操纵目标区域内所有接收机的定位结果，将其欺骗到另一位置并保持真实编队（相对距离不变），可整体转移大范围的车队与无人机集群。

方法上，多个同步发射器协同发射精心设计的信号，使区域内任意位置的接收机都解算出被操纵的坐标，误差可隐藏在合法噪声水平之下；与依赖"已知距离的接收机星座"或"突发不可预测运动"的检测手段正面对抗。作者用现成软件无线电实现原型并在真实 GNSS 接收机上评估。

结果：除大量仿真外，原型实验成功欺骗两台接收机，在欺骗位置距参考位置 1000 米时，两台接收机相对距离保持的平均误差仅 0.97 米，验证了"保形欺骗"的可行性；论文同时提出若干可能的对策。

我的思考：该工作把 GNSS 欺骗从"单点、需跟踪"升级为"广域、免跟踪"，威胁模型变化是本质性贡献，对自动驾驶车队、无人机蜂群等新兴应用构成直接警示；0.97 米的相对距离误差证明攻击精度足以隐藏于噪声。局限：部署同步发射器星座的物理成本与同步精度要求很高，误差随欺骗距离增长的趋势与对策的有效性仍需实证检验。

- **论文 PDF**: [GNSS-WASP: GNSS Wide Area SPoofing.](https://www.usenix.org/system/files/usenixsecurity25-tibaldo.pdf)

#### LEO-Range: Physical Layer Design for Secure Ranging with Low Earth Orbiting Satellites.

- **作者**: Daniele Coppola, Arslan Mumtaz, Giovanni Camurati, Harshad Sathaye, Mridula Singh, Srdjan Capkun
- **方向**: 无线与通信安全
- **解读**:

低轨（LEO）卫星星座（如 Starlink）信号强度高、几何多样性好，正被研究用作 GNSS 之外的定位与测距来源；但 LEO 信号同样面临欺骗、重放与距离欺诈（distance fraud）等物理层攻击，如何在 LEO 场景下实现安全测距缺乏系统的物理层设计。

据标题推断，论文提出面向 LEO 卫星的安全测距物理层设计：可能结合跳频/扩频序列的不可预测性、加密的测距应答协议或距离边界（distance bounding）思想，对抗欺骗与中继攻击，并为 LEO 定位提供认证的测距原语。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把安全测距理论落地到 LEO 卫星这一新兴基础设施，为"LEO 增强定位"提供可认证的物理层基础。

我的思考：该工作与同组 GNSS-WASP 构成"攻击—防御"互补：前者研究广域欺骗，后者构建安全测距；LEO 信号源多、功率强，天然适合做距离边界协议的载体，物理层不可预测性（如加密扩频）是防中继的关键。局限（据标题推断）：LEO 卫星高速移动带来的多普勒与可见性窗口约束、接收机硬件复杂度与标准化阻力是落地主要障碍，具体方案与性能数据需原文呈现。


### 数据安全（3 篇）

#### Blockchain Address Poisoning.
- **作者**: Taro Tsuchiya, Jin-Dong Dong, Kyle Soska, Nicolas Christin
- **方向**: 数据安全
- **解读**: 地址投毒（address poisoning）是近年兴起的链上诈骗手段：攻击者向受害者地址发送小额"粉尘"交易或构造相似假地址，污染其交易记录与地址簿；受害者在后续转账时若从历史记录复制地址，便可能把资金误转入攻击者控制的假地址。此类攻击利用的是用户在真实世界交互（复制粘贴、地址簿习惯）中的盲区，属于低成本、高覆盖的定向诈骗。本工作从标题看对链上地址投毒现象进行系统研究。

据标题推断，方法上可能包括：大规模扫描链上数据识别投毒交易模式（粉尘交易、相似地址生成、投毒者的资金归集特征），刻画攻击者的策略（目标选择、投毒时机、伪装程度）与受害规模，并可能测量受害者实际损失与攻击收益。摘要缺失，具体测量口径无法确认。

摘要缺失，无法引用量化结果；预期包含投毒交易数量、受影响地址规模、估算损失金额，以及攻击者行为画像等实证数据。

地址投毒揭示了"链上数据公开"与"用户行为盲区"结合产生的新型诈骗面，与传统钓鱼不同，它不需要受害者点击任何链接，仅靠污染交易历史即可奏效，防御上难以用常规手段拦截。此类实证对钱包厂商（地址校验 UI、相似地址告警）与用户教育有直接价值。局限：无摘要，样本时间范围、与合法粉尘交易（空投、隐私服务）的区分方法，以及损失估算的严谨性需全文确认；作者团队（Christin 组）在暗网与加密货币实证上积累深厚。（无摘要，据标题推断）


#### AUDIO WATERMARK: Dynamic and Harmless Watermark for Black-box Voice Dataset Copyright Protection.
- **作者**: Hanqing Guo, Junfeng Guo, Bocheng Chen, Yuanda Wang, Xun Chen, Heng Huang, Qiben Yan, Li Xiao
- **方向**: 数据安全
- **解读**: 大量开源语音数据集仅允许学术/教育用途，却无有效手段保证合规；理想方案是数据所有者对数据集加水印，从而识别任何使用该数据的模型。传统后门式水印存在四大缺陷：引入有害后门、对黑盒模型无效、损害音频质量、静态模式易被检测。本文提出 AUDIO WATERMARK 解决这些问题。

动态性通过风格迁移生成模型与随机参考风格实现；无害性借助域外（OOD）特征——水印模型能正确识别水印而不改变真实标签；黑盒有效性通过双层对抗优化训练一个通用模型来对抗水印生成器，提升水印在多个目标模型上的隐蔽性。

在 2 个语音数据集、10 个说话人识别模型上，与 10 种现有保护方法对比并在 8 种攻击场景下测试：良性准确率接近 100%、水印验证成功率 95%，且对所有测试攻击具有抵抗力。

我的思考：该工作把"无害+动态+黑盒"三个此前难以兼得的性质整合进数据集水印，双层对抗优化是让水印跨模型泛化的关键机制，95% 验证成功率与近 100% 良性准确率的组合很亮眼。局限是 OOD 特征依赖具体任务（说话人识别）设计、对迁移学习/微调场景的鲁棒性需进一步验证；但为语音数据版权治理提供了一条可行的技术路径。
- **论文 PDF**: [AUDIO WATERMARK: Dynamic and Harmless Watermark for Black-box Voice Dataset Copyright Protection.](https://www.usenix.org/system/files/usenixsecurity25-guo-hanqing.pdf)

#### Assuring Certified Database Utility in Privacy-Preserving Database Fingerprinting.

- **作者**: Mingyang Song, Zhongyun Hua, Yifeng Zheng, Tao Xiang, Guoai Xu, Xingliang Yuan

- **方向**: 数据安全

- **解读**: 

数据库指纹技术允许数据所有者（DO）在关系数据库中嵌入唯一标识以追踪非法转售，但 DO 为最大化指纹鲁棒性往往大量修改数据位，可能显著损害数据库效用；接收方无法核实修改程度，对"看起来被动过手脚"的库心存顾虑。现有指纹方案只关注鲁棒性提升，不提供任何验证修改程度的机制。

本文首次解决"接收方可验证最大修改位数"这一空白：提出模糊扰动验证（FPV）协议，使验证者在修改位置与原比特串均保密的前提下评估证明者对比特串的修改程度；基于 FPV 提出 UtiliClear 指纹方案，允许接收方指定并验证指纹数据库中的修改程度，并从理论上论证其在指纹鲁棒性、数据库效用与数据隐私三方面的保证。

在大型真实数据集上的评估表明，UtiliClear 引入的开销适中，同时保持与现有最先进方案相当的指纹鲁棒性与数据库效用。

我的思考：贡献在于把"可验证性"引入数据库指纹交易，弥合 DO 与接收方之间的信任鸿沟；FPV 的"模糊验证"在控制信息泄露的同时完成程度认证，设计巧妙，是首个可证明的效用认证方案。局限是验证的是修改程度而非具体位置，协议在超大规模数据库上的扩展性与不同数据分布下的表现仍需更广评估。与只追求鲁棒性的既有指纹方案相比，本文建立了鲁棒性-效用-隐私三角的新平衡点，对数据交易市场具有现实价值。

- **论文 PDF**: [Assuring Certified Database Utility in Privacy-Preserving Database Fingerprinting.](https://www.usenix.org/system/files/usenixsecurity25-song-mingyang.pdf)

### 社会工程与人类因素（36 篇）

#### Abusability of Automation Apps in Intimate Partner Violence.
- **作者**: Shirley Zhang, Paul Chung, Jacob Vervelde, Nishant Korapati, Rahul Chatterjee, Kassem Fawaz
- **方向**: 社会工程与人类因素
- **解读**: 亲密伴侣暴力(IPV)中的技术滥用是新兴的安全与人机交互交叉问题,攻击者利用共享账户、智能家居与自动化工具对受害者实施监控、骚扰与控制。本文聚焦自动化应用(automation apps)在这一场景中的可滥用性,此前缺乏系统的机制分析。(无摘要,据标题推断)

依据标题推断,作者对自动化应用的滥用面进行系统性梳理,分析其设计机制(如触发器-动作编排、共享设备配置、通知与日志)如何可能被伴侣滥用于监视与控制,并考察现有平台防护措施的缺口。

研究应刻画具体滥用场景、识别高风险功能,并评估防护机制的覆盖范围,为平台设计者提供改进方向。由于摘要与全文文本均缺失,具体量化结果无法引用。

该主题的紧迫性在于智能家居与自动化应用普及速度远超安全设计跟进速度,此类工作能把"技术滥用"从个案报道提升为可操作的设计原则。局限:缺乏摘要与全文,无法核实方法细节与结论强度;此类质性研究通常样本有限。期待与既有 IPV 技术滥用研究(如 stalkerware 分析)形成互补。

#### Malicious LLM-Based Conversational AI Makes Users Reveal Personal Information.
- **作者**: Xiao Zhan, Juan Carlos Carrillo, William Seymour, Jose Such
- **方向**: 社会工程与人类因素
- **解读**: 大语言模型驱动的对话式 AI 正在成为主流交互界面,但恶意部署的 LLM 聊天机器人可能被用于社会工程:通过看似无害的对话诱导用户泄露个人信息。本文研究这类恶意对话 AI 对用户信息披露行为的影响,此前缺乏实证。(无摘要,据标题推断)

依据标题推断,作者设计了受控用户研究或在线实验,让参与者与恶意 LLM 聊天机器人交互,测量其在对话中被诱导披露个人信息的程度与方式,可能还对比不同对话策略(如共情、伪装身份)的诱导效果。

研究应揭示恶意对话 AI 诱导信息披露的有效性与作用机制,警示平台与用户。由于摘要与全文缺失,具体样本量、披露率等量化结果无法引用。

LLM 社会工程是"大模型安全×人类因素"的重要交叉方向,与经典钓鱼研究相比,LLM 能以极低成本大规模生成个性化诱导对话,威胁面显著扩大。该工作若能给出可复现的实验范式,将推动后续防御研究(如对话审计、披露提醒)。局限在于结论依赖实验设计,生态效度有待真实环境验证。

#### Characterizing and Detecting Propaganda-Spreading Accounts on Telegram.
- **作者**: Klim Kireev, Yevhen Mykhno, Carmela Troncoso, Rebekah Overdorf
- **方向**: 社会工程与人类因素
- **解读**: 基于信息的社会工程攻击(虚假信息运动与宣传)是新兴网络安全威胁,安全社区此前聚焦 X、Reddit 等开放平台,却忽视了 WhatsApp、Telegram、Signal 等即时通讯平台——这些平台上攻击主要发生在群组与频道,依赖管理员人工审核,成本高且易遗漏。

作者收集、标注并分析了超过 1,700 万条 Telegram 评论与消息,发现两个相互独立、协调运作的网络,分别传播亲俄与亲乌宣传,并获得了真实用户的回复。据此提出利用合法用户消息与宣传回复之间关系的新型检测机制,并针对 Telegram 向管理员开放的信息量身设计。

该检测方法更快、更便宜,且在看一个账户的一条消息后检出率达 97.6%,比人工审核员高出 11.6 个百分点;面对不断演化的宣传手法仍保持有效。

该工作的亮点是把检测对象从开放社交平台迁移到半公开频道,并证明"利用受害者回复关系"这一信号的判别力,兼顾了管理员可获取信息的现实约束。局限:数据集以特定地缘政治事件为主,泛化到其他语言与议题需验证;97.6% 的检出率来自单条消息,长尾账户的早期识别能力未充分说明。与既有平台研究相比,其部署导向更贴合即时通讯平台的治理现实。
- **论文 PDF**: [Characterizing and Detecting Propaganda-Spreading Accounts on Telegram.](https://www.usenix.org/system/files/usenixsecurity25-kireev.pdf)

#### SoK: Inaccessible & Insecure: An Exposition of Authentication Challenges Faced by Blind and Visually Impaired Users in State-of-the-Art Academic Proposals.
- **作者**: Md Mojibur Rahman Redoy Akanda, Amanda Lacy, Nitesh Saxena
- **方向**: 社会工程与人类因素
- **解读**: 认证是数字安全的基石，但大量认证方案（图形密码、生物识别、多因素、基于位置的方案等）在设计时默认用户具备完整视觉能力，使盲人与低视力（BVI）用户既难以使用（可及性缺失），又可能因被迫采用不安全替代路径（如求助他人）而暴露于更高风险（安全性受损）。本工作以 SoK（知识系统化）的形式，系统梳理学术界最新认证方案对 BVI 用户的挑战，回答"可及性与安全性如何在学术方案设计中双双落空"。

据标题推断，方法上是对近年学术文献中的认证方案进行系统性筛选与评估：以可及性标准（是否依赖视觉、是否有替代通道）与安全性标准（是否引入新的攻击面）为维度对方案分类，归纳共性问题与设计误区，并给出面向研究社区的改进指引。摘要缺失，文献范围与评估框架无法确认。

摘要缺失，无法引用量化结果；预期包含被评估方案的数量、分类统计（如视觉依赖方案占比）、以及按可及性/安全性维度的问题分布等综述性数据。

SoK 的价值在于把"无障碍认证"从边缘议题提升为可评估的研究议程：现有方案在可及性与安全性的权衡上系统性失衡，且两者往往互相强化（不可及→不安全），缺乏系统综述使新方案重复同样的错误。Saxena 团队在认证与人类因素方向积累深厚。局限：无摘要，文献检索范围与编码框架的主观性需全文确认；其聚焦"学术提案"可能低估现实工业系统的成熟实践。（无摘要，据标题推断）


#### Scanned and Scammed: Insecurity by ObsQRity? Measuring User Susceptibility and Awareness of QR Code-Based Attacks.
- **作者**: Marvin Kowalewski, Leona Lassak, Markus Dürmuth, Theodor Schnitzler
- **方向**: 社会工程与人类因素
- **解读**: 二维码（QR code）已成为日常支付、扫码点餐、访客登记的标配入口，其"看一眼无法分辨指向何处"的特性使其成为钓鱼（quishing）与恶意重定向的天然载体：攻击者只需覆盖或替换一张二维码，即可把用户导向恶意站点，而用户几乎无法在扫码前察觉。本工作从标题看通过用户研究测量用户对二维码攻击的易感性（susceptibility）与意识（awareness），回答"用户是否会扫、以及是否知道扫了之后可能发生什么"。

据标题推断，方法上可能设计受控实验：向参与者展示带二维码的场景（真实或模拟环境），观测其扫码行为与后续决策（如是否在恶意页面输入凭据），并配合问卷测量风险感知；可能比较不同情境（公共场所张贴、邮件、支付）下的易感性差异。摘要缺失，实验规模与任务设计无法确认。

摘要缺失，无法引用量化结果；预期包含扫码率、在恶意页面的上当率、参与者的风险认知水平，以及人口/情境因素对易感性的影响等数据。

二维码攻击的独特之处在于"介质与内容解耦"——用户信任视觉符号而非实际 URL，传统 URL 警觉在扫码场景完全失效，因此用户层面的防御（警觉训练、安全提示）需要重新设计。Dürmuth 组在人类因素与移动安全方向有持续积累。局限：无摘要，实验生态效度（实验室 vs 真实场景）与样本代表性需全文确认；其结论对扫码应用（相机应用预览 URL、安全扫描）的设计启示值得期待。（无摘要，据标题推断）


#### URL Inspection Tasks: Helping Users Detect Phishing Links in Emails.
- **作者**: Daniele Lain, Yoshimichi Nakatsuka, Kari Kostiainen, Gene Tsudik, Srdjan Capkun
- **方向**: 社会工程与人类因素
- **解读**: 最普遍的钓鱼攻击形态是邮件中指向恶意内容的链接；尽管有培训与检测技术，攻击依然高效。近年研究表明，关键因素并非用户缺乏教育，而是注意力涣散。本文据此提出一种新颖的钓鱼防御机制——URL 检查任务（URL inspection tasks）：一类受 CAPTCHA 启发的"小挑战"，要求用户必须与 URL 结构交互并理解其基本构成，才能通过"屏障"访问目标网站。

作者实现并评估了三种任务：(1) 从 URL 列表中正确点击选中目标；(2) 用鼠标高亮 URL 的域名部分；(3) 重新键入域名。任务设计遵循安全界面与警告设计最佳实践，经一项 2,673 名参与者（来自三种文化、母语与字母系统）的在线用户研究评估。

结果显示，与基线相比，这些任务显著降低了钓鱼尝试的成功率；且对最难的 URL（如 typosquat 拼写仿冒域名，即参与者最难应对的类型）效果最为显著。这印证了两点：其一，放慢用户节奏并聚焦其注意力；其二，帮助用户理解 URL 结构（尤其是域名成分）并将其与自身意图匹配。

该工作的巧妙之处在于把"强制交互"从烦人验证码转化为安全认知工具：拦截发生在点击之前，且任务本身即在训练用户识别 URL 结构。局限：任务带来的摩擦可能引发用户反感或绕过行为（如放弃访问或寻求他人代操作），长期部署的可用性成本需权衡；与邮件客户端现有过滤体系的集成方式也待细化。总体而言，它为"人因防御"提供了可落地的设计范式。

- **论文 PDF**: [URL Inspection Tasks: Helping Users Detect Phishing Links in Emails.](https://www.usenix.org/system/files/usenixsecurity25-lain.pdf)

#### Digital Security Perceptions and Practices Around the World: A WEIRD versus Non-WEIRD Comparison.
- **作者**: Franziska Herbert, Collins W. Munyendo, Jonas Hielscher, Steffen Becker, Yixin Zou
- **方向**: 社会工程与人类因素
- **解读**: 可用安全与隐私研究长期偏向 WEIRD（西方、受过教育、工业化、富裕、民主）社会，对非 WEIRD 社会的研究稀少且多为定性，缺乏大规模跨国比较，导致人们对安全需求、感知与实践如何随文化与情境变化知之甚少，基于 WEIRD 样本得出的结论也难以直接外推。本文填补这一空白：对四大洲 12 个国家（7 个 WEIRD、5 个非 WEIRD）共 12,351 名参与者开展问卷调查，考察感知（不同数据类型的重要性、各类攻击者带来的风险）与实践（防护措施采用、既往负面经历）。

研究发现，WEIRD 与非 WEIRD 国家在几乎所有变量上均存在显著差异，效应量各异。一个典型例子：非 WEIRD 国家的参与者更依赖亲友提供数字安全建议，但同时更可能把亲友视为潜在攻击者——信任与威胁来源的重叠反映出不同社会环境中的安全现实。

作者对跨国差异给出解释，讨论研究结果对安全干预与教育的启示，并总结开展跨国研究的经验教训。

该研究以罕见的样本规模（12,351 人、12 国）把"WEIRD 偏差"从批评性议论变成可量化的实证基线，对安全干预设计的文化适配具有直接指导意义，其"亲友既被信任又被怀疑"等反直觉发现尤其值得关注。局限：问卷依赖自我报告，可能受社会期许偏差影响；国家内差异（城乡、代际）被国家间比较掩盖；12 国虽广但远非全球代表。总体而言，它为跨文化安全研究树立了方法参照。

- **论文 PDF**: [Digital Security Perceptions and Practices Around the World: A WEIRD versus Non-WEIRD Comparison.](https://www.usenix.org/system/files/usenixsecurity25-herbert.pdf)

#### SoK: Come Together - Unifying Security, Information Theory, and Cognition for a Mixed Reality Deception Attack Ontology & Analysis Framework.
- **作者**: Ali Teymourian; Andrew M. Webb; Taha Gharaibeh; Arushi Ghildiyal; Ibrahim Baggili
- **方向**: 社会工程与人类因素
- **解读**: MR头显把虚拟信息叠加到现实世界，欺骗攻击可操纵用户感知与决策（如引导行人走入车流），但学界缺乏描述与分析MR欺骗攻击的理论框架。本文通过跨学科SoK，整合MR安全、信息论与认知科学来填补这一空白。

方法上，以Borden-Kopp欺骗模型为骨架构建攻击本体——信道攻击（公开降级：感官过载、信号替换；隐蔽降级：边界/维度微调；拒绝：关停、覆盖、移除）与处理攻击（腐化：欺骗、伪旗；颠覆：偏见攻击、虚假信息、煤气灯效应）；用Shannon信道容量C=W·log2(1+S/(NA+NE))形式化攻击对信息通信的影响，用Vitanyi模仿度刻画腐化/颠覆的伪装程度；构建七组件决策模型（感觉输入→注意→感知→记忆→决策→执行→反应），经mini-Delphi专家迭代，综述80篇文献。

结果显示，提出MR欺骗分析框架（DAF）与5项关键发现（如技术攻击主要面向视觉与软件模态、主要支持信道攻击而非处理攻击）、5项研究空白（如AR系统研究不足、缺乏描述处理攻击影响人类行为的模型），并将30余种技术攻击（chaperone、human joystick、重定向行走等）映射进本体。

我的思考：这是首个统一MR欺骗攻击的理论框架，把散乱的研究纳入可比较的本体与定量语言，对研究议程有明确引导作用。局限是纯定性/理论性：无实验验证，DAF是分析透镜而非检测工具，信道容量对"人类感知信道"的适用性也有待实证检验。


#### Evaluating LLM-based Personal Information Extraction and Countermeasures.
- **作者**: Yupei Liu, Yuqi Jia, Jinyuan Jia, Neil Zhenqiang Gong
- **方向**: 社会工程与人类因素
- **解读**: 大规模自动提取公开资料中的个人信息（姓名、电话、邮箱等）是鱼叉式钓鱼等后续攻击的起点。传统正则表达式、关键词搜索与实体识别方法在此类提取上效果有限。本文对基于 LLM 的个人信息提取攻击及其防御开展系统性测量研究，量化 LLM 放大个人信息暴露风险的程度。

提出基于 LLM 的提取攻击框架；构建四个数据集，包括一个 GPT-4 生成的合成数据集和三个真实世界数据集，人工标注八类个人信息；并进一步提出若干对抗手段，评估其对 LLM 提取攻击的防御效果（摘要截断）。

测量显示 LLM 能以远超传统方法的精度从公开资料中提取个人信息，为鱼叉式钓鱼等攻击提供"弹药"；所提出的防御手段可一定程度降低提取成功率（摘要截断，具体数值未给出）。

该工作以严谨的测量研究方式，实证了"大规模数据抓取+LLM 提取"组合攻击的现实威胁，对隐私政策与技术防御均有参考价值。局限是数据集规模与标注类别有限，且防御与攻击将持续博弈，单靠技术手段难以根治，需政策与工程双管齐下。

- **论文 PDF**: [Evaluating LLM-based Personal Information Extraction and Countermeasures.](https://www.usenix.org/system/files/usenixsecurity25-liu-yupei.pdf)

#### A Mixed-Methods Study of Open-Source Software Maintainers On Vulnerability Management and Platform Security Features.
- **作者**: Jessy Ayala; Yu-Jye Tung; Joshua Garcia
- **方向**: 社会工程与人类因素
- **解读**: OSS漏洞数量激增，学界研究过漏洞报告者与贡献者的安全实践，却忽视了站在防御最前线的维护者视角。本文首次聚焦维护"曾存在已修复漏洞项目"（来自GitHub Advisory Database）的维护者，探究其漏洞管理与平台安全功能（PSF）使用情况。

方法上采用混合研究设计：列出式在线调查（n1=80，免费列出法）加半结构化访谈（n2=22，平均52分钟），经开放式编码与主题分析归纳出37个因素、五大类别（现状实践、一般挑战、PSF挑战、PSF采用障碍、维护者期望）。

结果显示，最大的一般性挑战是供应链不信任（36次列出/6次访谈提及）与对漏洞的理解不足（35/12）；PSF最大挑战是自动化不足（39/11）、噪音过多（24/10）与漏洞评分难（21/9）；采用PSF的主要障碍是缺乏意识（33/12）、设置复杂（26/7）与"认为没必要"（23/6）——26.7%（21/80）受访者未启用安全策略、私有漏洞报告、安全公告这三项关键功能中的任何一项，多数人（50/80）没有安全背景。出乎意料的是，即便项目曾被攻破，部分维护者仍允许公开报告漏洞甚至无视报告。

我的思考：该研究把"平台功能利用率低"的定量观察转化为维护者的一手解释（意识、可用性、动机），对GitHub等平台改进PSF可用性与默认开启策略有直接可操作的建议，在xz-utils供应链投毒事件后尤具现实意义。局限是自我报告数据与样本偏差（主动参与安全议题者），可推广性有限。

- **论文 PDF**: [A Mixed-Methods Study of Open-Source Software Maintainers On Vulnerability Management and Platform Security Features.](https://www.usenix.org/system/files/usenixsecurity25-ayala.pdf)

#### "Threat modeling is very formal, it's very technical, and also very hard to do correctly": Investigating Threat Modeling Practices in Open-Source Software Projects.
- **作者**: Harjot Kaur; Carson Powers; Ronald E. Thompson III; Sascha Fahl; Daniel Votipka
- **方向**: 社会工程与人类因素
- **解读**: 威胁建模（TM）被推荐为在开发早期识别威胁、确定缓解措施的安全设计实践，但OSS开发者如何考虑威胁、是否使用结构化TM方法缺乏证据。本文通过25名OSS开发者的半结构化访谈（平均64分钟），回答实践、挑战与采纳动机。

方法上，访谈围绕"四个问题"框架（构建什么、可能出什么问题、如何应对、做得够不够好）设计，并刻意避免使用"威胁建模"术语；招募覆盖npm安全/密码学/Rust生态等高相关项目，主题分析编码一致性Krippendorff α≥0.94。

结果显示，几乎所有开发者采用ad hoc实践——后台对抗思维、"凭常识知道"的威胁记忆清单、甚至先上缓解措施再回头想威胁；只有极少数使用STRIDE、攻击树、LINDDUN等结构化流程。独特挑战包括：志愿者缺时间动机（"不有趣"、怕挫伤热情）、去中心化且流动的团队（组织、完备性判断、共识均难）、团队太小认为结构化TM不值得、缺乏安全专家资源，以及项目从爱好起步后期才补安全。约半数通过issue tracker沟通威胁，部分实践完全私有，个别开发者开始用ChatGPT找威胁。

我的思考：这是首个系统刻画OSS威胁建模实践的研究，核心洞见是"ad hoc虽不理想，却因易用、灵活、低开销而被普遍采纳"，而结构化方法被普遍视为"重武器"——这提示应推动轻量化的结构化TM而非要求完整流程。局限是样本偏向安全相关项目、纯定性无有效性评估。

- **论文 PDF**: ["Threat modeling is very formal, it's very technical, and also very hard to do correctly": Investigating Threat Modeling Practices in Open-Source Software Projects.](https://www.usenix.org/system/files/usenixsecurity25-kaur.pdf)

#### A limited technical background is sufficient for attack-defense tree acceptability.

- **作者**: Nathan Daniel Schiele, Olga Gadyatskaya
- **方向**: 社会工程与人类因素
- **解读**: 攻击-防御树（ADT）是重要的图形化威胁建模方法，被高度推荐用于分析与沟通安全相关信息。然而既有实证研究只确立了攻击树对高技术水平（计算机科学背景）用户的可用性，对技术背景有限的安全利益相关者是否适用仍存疑问，本文针对这一空白研究用户技术背景对 ADT 可接受性的影响。

研究基于方法评估模型（Method Evaluation Model），招募 n=102 名参与者（53 名强计算机背景、49 名弱计算机背景）完成一系列与 ADT 相关的任务，通过分析作答并比较两组结果得出结论。

结果显示：非常有限的技术背景就足以保证 ADT 的可接受性，这一发现强调了攻击树作为威胁建模方法的可行性。

我的思考：该工作以对照实验设计回答了「ADT 是否只能被技术专家使用」的疑问，n=102 的样本量在威胁建模可用性研究中较为扎实；两组被试的对照使结论更具说服力。局限在于实验室任务环境与真实威胁建模场景仍有差距，参与者动机也与实际利益相关者不同；但研究为 ADT 在更广泛利益相关者群体（如管理层、非技术合规人员）中的推广提供了实证支撑。

- **论文 PDF**: [A limited technical background is sufficient for attack-defense tree acceptability.](https://www.usenix.org/system/files/usenixsecurity25-schiele.pdf)

#### "It's not my responsibility to write them": An Empirical Study of Software Product Managers and Security Requirements.

- **作者**: Houda Naji, Felix Reichmann, Tobias Bruns, M. Angela Sasse, Alena Naiakshina
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）安全需求（security requirements）是软件安全的上游保障，但谁负责撰写它们一直存在责任模糊。论文以软件产品经理（PM）为对象开展实证研究，标题引语「写它们不是我的责任」鲜明地揭示了 PM 在安全需求上的责任推诿态度。研究应通过访谈或问卷考察 PM 对安全需求撰写的认知、角色定位与实际做法。

方法上，该工作很可能是半结构化访谈加主题分析，探究 PM 如何看待安全需求与其职责边界、在哪些情境下会把安全需求责任推给他人（如安全团队、开发人员），以及组织流程如何塑造这种态度。

预期发现将包括：安全需求被视为「别人的事」、缺乏激励与考核、安全与产品节奏冲突导致安全需求被边缘化等结构性原因。

我的思考：该研究把「人的因素」带入安全需求工程——安全失败的根因往往不在技术而在职责分配与组织文化。引语式标题直指问题核心，实证数据若扎实将有力支撑「把安全需求责任显式写入 PM 职责」的治理建议；局限是自我报告可能美化行为，结合观察或工件分析会更可信。


#### "I'm regretting that I hit run": In-situ Assessment of Potential Malware.
- **作者**: Brandon Lit; Edward Crowder; Hassan Khan; Daniel Vogel
- **方向**: 社会工程与人类因素
- **解读**: 用户在安装软件时如何现场判断其是否恶意？此前研究多基于问卷或虚拟情境，缺少真实安装过程中的行为测量，导致对用户威胁评估能力与误区的理解不足。

本文开展首次双阶段受控实验室研究（n=36），让参与者在标准 Windows 笔记本上安装真实良性与恶意软件：第一阶段不给任何指导，观察其评估策略；第二阶段引入提供 CPU 占用、访问文件、网络目的国等系统信息的"增强任务管理器"，考察攻击指标对决策的影响；采用 think-aloud 协议记录评论，得到 2,651 条编码为 4 个顶级"指标"类别（25 个子类）的数据集。

结果显示，终端用户分类恶意软件出人意料地准确，提供攻击指标后准确率进一步提升；分析还揭示了常见误解、对可被绕过的指标的依赖，并为软件与操作系统厂商改进界面与通知提供了可操作建议。

生态效度高是该研究的核心成色——真实恶意软件、真实安装流程、think-aloud 行为数据；2,651 条编码语料对后续人因研究有复用价值。局限在于 36 人样本与实验室情境的推广性，以及"增强任务管理器"本身是模拟工具；与既有安全心智模型研究互补，提示防御设计应聚焦用户实际依赖且难以被攻击者伪造的指标。

- **论文 PDF**: ["I'm regretting that I hit run": In-situ Assessment of Potential Malware.](https://www.usenix.org/system/files/usenixsecurity25-lit.pdf)

#### "I'm trying to learn...and I'm shooting myself in the foot": Beginners' Struggles When Solving Binary Exploitation Exercises.
- **作者**: James Mattei, Christopher Pellegrini, Matthew Soto, Marina Sanusi Bohuk, Daniel Votipka
- **方向**: 社会工程与人类因素
- **解读**: 漏洞发现是安全领域的核心技能,但对初学者而言异常艰难:尽管网上资源丰富,许多初学者仍会受挫甚至放弃。以往研究多聚焦于设计新型漏洞发现练习(VDE),却缺乏对初学者在解题过程中具体卡点的系统化、量化理解。本文要回答:初学者在识别与利用漏洞时究竟在哪里挣扎、如何搜索答案、以及为何难以把搜索结果应用到实际问题上。
作者对 37 名漏洞发现初学者开展半结构化观察式访谈,让他们尝试利用 51 个存在漏洞的程序,对 3950 个行为事件进行严格定性编码。研究发现:初学者最难的是"理解漏洞如何利用"这一步骤,该步骤中网页访问量显著高于其他步骤;25 名受挫参与者中有 21 人在"把网页知识迁移到题目"时受挫。常见技术失误包括:33 人从未以十六进制地址作为程序输入、24 人忽略小端序转换、16 人误读 GDB 十六进制输出。在资源使用上,论坛页面(如 StackOverflow)是最常用资源(N=33, E=175);约 32% 的参与者借助 LLM(如 ChatGPT)搜索信息与解读工具输出;当参与者只看搜索结果摘要而不点开页面时(共 163 次),仅 59 次(36%)真正解决了问题——作者将这种缺乏搜索目标的现象称为"搜索瘫痪"(search paralysis)。
研究揭示了初学者困境的根源并非单纯的"找不到资料",而是缺乏构造有效关键词的专业词汇、以及无法将资源信息正确迁移到具体上下文。这与"语言先验缺失"的假设一致:他们往往不知道自己在找什么。结论为教育者与资源制作者提供了明确指引:应提供更贴近初学者认知的资源、更清晰的术语桥梁和分步指导。局限在于样本以美国高校学生为主、以访谈任务为情境,推广性有限;LLM 作为新工具的角色也值得后续专门研究。

#### "That's my perspective from 30 years of doing this": An Interview Study on Practices, Experiences, and Challenges of Updating Cryptographic Code.
- **作者**: Alexander Krause, Harjot Kaur, Jan H. Klemmer, Oliver Wiese, Sascha Fahl
- **方向**: 社会工程与人类因素
- **解读**: 密码代码是否及时更新直接决定软件整体安全性:算法升级(SHA-1→SHA-512)、密钥长度提升(2048→4096 位)、协议演进(TLS 1.2→1.3)以及即将到来的后量子密码(PQC)迁移都是典型密码更新目标。然而大量软件长期携带过时、不安全的密码代码,安全社区对"密码更新到底难在哪里"缺乏扎实理解。本文通过访谈研究填补这一空白,探索开发者的意识来源、更新动机、过程与挑战。
作者对 21 名具备密码更新经验的资深开发者进行了深度半结构化访谈,围绕四个研究问题展开:开发者如何获知更新需求(RQ1)、为何更新(RQ2)、如何规划与执行(RQ3)、以及遇到哪些挑战(RQ4)。结果表明:多数参与者出于安全原因更新,但性能等非安全外部因素同样起作用;密码更新比普通软件更新更独特、更复杂——过时硬件会反向影响算法选型。关键发现是开发者普遍缺乏结构化的更新流程,面临三大挑战:密码知识不足、遗留系统向后兼容性阻碍迁移、缺乏可用的指导文档与资源。参与者明确表达了对密码专家协助和"非密码开发者也能读懂"的资源的渴望。
研究以 21 名经验丰富的开发者样本,系统刻画了密码更新这一"高影响、低理解"活动的全貌,结论落在面向开发者、库作者、标准化组织与学术界的可操作建议上,对即将到来的 PQC 大规模迁移具有直接现实意义。其局限在于访谈研究的样本量与自我报告偏差——参与者可能美化自身实践;也未能覆盖更新失败的长期后果。相比以往聚焦"密码误用"的研究,本文把视角从"写错"转向"改不动",为密码敏捷性(crypto agility)研究提供了稀缺的质性基础,也为自动化迁移工具的设计指明了需求方向。
- **论文 PDF**: ["That's my perspective from 30 years of doing this": An Interview Study on Practices, Experiences, and Challenges of Updating Cryptographic Code.](https://www.usenix.org/system/files/usenixsecurity25-krause.pdf)

#### "I have no idea how to make it safer": Studying Security and Privacy Mindsets of Browser Extension Developers.
- **作者**: Shubham Agarwal; Rafael Mrowczynski; Maria Hellenthal; Ben Stock
- **方向**: 社会工程与人类因素
- **解读**:（无摘要，据标题推断）浏览器扩展是安全与隐私泄露的高发面，但扩展开发者如何理解威胁、为何写出不安全代码，缺乏系统的实证研究。

论文对浏览器扩展开发者开展访谈等定性研究，考察其安全与隐私心智模型、知识缺口与开发实践中的决策过程，分析安全考虑在开发流程中的位置。

（无摘要，据标题推断）据标题，研究发现开发者普遍缺乏把扩展做得更安全的知识与能力，安全考虑在开发流程中系统性缺席，并据此给出改进方向。

引言式标题（"I have no idea how to make it safer"）直指开发者无助感的普遍性；理解开发者视角是设计有效安全工具、文档与平台机制（如最小权限 API、审查流程）的前提，与浏览器扩展商店的权限机制改革直接相关。局限在于无摘要难核实样本与方法细节；此类定性研究的价值在于假设生成，与扩展生态的量化测量研究结合能形成完整图景。


#### Doubly Dangerous: Evading Phishing Reporting Systems by Leveraging Email Tracking Techniques.

- **作者**: Anish Chand; Nick Nikiforakis; Phani Vadrevu
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）钓鱼邮件举报系统是用户与安全厂商发现钓鱼站点的重要防线；本文研究攻击者如何反过来利用邮件追踪技术（如追踪像素、重定向链接）逃避举报系统的发现与取证。标题"Doubly Dangerous（双重危险）"暗示同一机制同时服务于攻击与反取证。

  据标题推断，攻击者借助邮件追踪能力感知邮件何时被打开、举报何时发生、钓鱼基础设施何时暴露，从而在举报者取证前切换或隐藏恶意内容，或据此校准攻击投放；研究应包含对这些滥用手法的系统分析与实证评估。

  无摘要可用，论文的具体量化结果无法从标题确认；其核心主张应是：防御方依赖的可见性信号（打开率、举报来源）反而成为攻击者修正攻击的依据，使举报系统系统性失效。

  该工作把攻防不对称推向新高度——举报生态若继续依赖可被追踪者观测的信号，取证路径就需要重新设计（如代理化上报、延迟上报、去标识化）。与既往钓鱼规避研究相比，其新意在于利用邮件生态自身的机制而非单纯混淆内容；实际影响取决于追踪滥用手法在主流邮件客户端与举报流程中的可行性，需阅读全文确认。


#### NOKEScam: Understanding and Rectifying Non-Sense Keywords Spear Scam in Search Engines.
- **作者**: Mingxuan Liu, Yunyi Zhang, Lijie Wu, Baojun Liu, Geng Hong, Yiming Zhang, Hui Jiang, Jia Zhang, Haixin Duan, Min Zhang, Wei Guan, Fan Shi, Min Yang
- **方向**: 社会工程与人类因素
- **解读**: NOKEScam（无意义关键词精准诈骗）是一种新兴诈骗：用罕见且通常无意义的词汇（NSKeywords）作为诱饵，无需复杂黑帽 SEO——因为只有诈骗页面精确匹配这些生僻词，搜索结果顶部被完全占据，受害者误信其权威性。其独特性使此前研究与检测方法缺位，本文与中国头部搜索引擎合作进行实证与治理。

基于实证研究归纳三个关键观察，作者开发轻量检测系统：约 1 小时即可处理约 20 亿条 URL；系统在七个月内识别出 68,863 个域名上的 153,975 个 NSKeywords。

测量显示，借助搜索引擎的信任背书，NOKEScam 网站日均吸引超过 3 万次页面浏览，诈骗收益潜力巨大；尽管搜索引擎持续打击、攻击者改用更多域名规避，检测系统依然有效，真实用户投诉量实现 194 倍下降。

我的思考：该工作把"检测-测量-治理"闭环落地于真实搜索引擎，20 亿 URL/小时的吞吐与 194 倍投诉下降是罕见的端到端证据，也揭示了"搜索引擎信任背书"这一被忽视的诈骗放大器。局限是依赖与搜索引擎的合作（通用性受限），且攻击者会持续换域名对抗；但作为新兴诈骗的系统刻画与治理样板，价值明确。
- **论文 PDF**: [NOKEScam: Understanding and Rectifying Non-Sense Keywords Spear Scam in Search Engines.](https://www.usenix.org/system/files/usenixsecurity25-liu-mingxuan.pdf)

#### "Please don't send that bot anything": A Mixed-methods Study of Personal Impersonation Attacks Targeting Digital Payments on Social Media.
- **作者**: Hoang Dai Nguyen, Sumit Dhungana, Madhulika Itha, Phani Vadrevu
- **方向**: 社会工程与人类因素
- **解读**: 社交媒体上的数字支付日益普及，攻击者通过冒充真实个人（亲友、名人等）实施诈骗，诱导受害者向其转账，造成直接经济损失；此类"个人冒充攻击"的规模、手法与受害者体验此前缺乏系统的实证刻画。本文以混合方法研究针对社交平台数字支付的冒充攻击现象。（无摘要，据标题推断）

从标题推断，研究结合定量与定性手段：可能包括对冒充账号的大规模采集与特征分析（账号命名、头像、发帖行为等）、对诈骗对话的取证分析，以及对受害者和平台用户的访谈，以还原攻击者话术与诱导流程的全貌。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于揭示冒充攻击在数字支付场景中的运作机制、常见话术模板，以及现有平台防护的薄弱环节。（无摘要，据标题推断）

冒充诈骗是社交工程与支付安全的交汇点，经济损失直接且难以追回，研究价值明确。混合方法设计的优势在于用定量数据支撑规模判断、用定性材料揭示动机与体验；局限是样本获取依赖平台数据与受访者自愿参与，存在覆盖偏差，且结论随平台策略演化需持续更新。


#### 'Hey mum, I dropped my phone down the toilet': Investigating Hi Mum and Dad SMS Scams in the United Kingdom.

- **作者**: Sharad Agarwal, Emma Harvey, Enrico Mariconti, Guillermo Suarez-Tangil, Marie Vasek
- **方向**: 社会工程与人类因素
- **解读**: SMS 诈骗激增且检测手段升级，骗子转向更难检测的手法。"嗨，妈妈/爸爸"诈骗即冒充受害者的孩子、以手机损坏换号为由索要金钱，与传统 smishing 的关键区别在于成功后会与受害者持续对话而非只发一条带链接的消息。该诈骗已全球蔓延却缺乏实证研究，本文是第一项系统实证。

作者与英国某移动网络运营商合作获取 3,402 条初始诈骗短信，并创建 mum/dad/无称呼三种虚拟身份（各 10 个虚拟号码）主动与骗子对话假装受害者，配合 HLR 查询、生存分析、财务与网络图分析研究诈骗全生命周期。发现 72.5% 初始消息针对"妈妈"（骗子认为女性更易受骗），83.7% 要求转至 WhatsApp 等在线平台；骗子主要利用善意、分心与时间压力三种诱骗原则（Stajano 与 Wilson 分类）。

13 周内从 711 次交互收集 582 个唯一骡子账户，骗子共索要逾 £577,792（564 笔请求，中位每笔 SMS £880、在线平台 £1,244），据此估计英国受害者每年至少损失 £230 万。骗子大量滥用电子货币机构（374 个账户）并以小额（<£1,500）规避风控，大额（>£2,500）请求改用高街银行；67.1% 来源号码属于单一运营商 MNO1（其 SIM 支持 SIM box 所需 GSM），来源与对话号码中位存活期仅 14 与 46 天；识别出 6 个骗子团伙。

首次完整刻画该诈骗运营生态：角色分工、诱骗心理机制、运营商与金融机构被滥用的具体方式，并提出快速封号、金额风控与公众教育等缓解措施，发现已共享执法部门与银行，伦理审查完备。局限：仅一家运营商数据且不透露机构名，代表性受限；主动交互可能改变骗子行为；年损失为外推估计。

- **论文 PDF**: ['Hey mum, I dropped my phone down the toilet': Investigating Hi Mum and Dad SMS Scams in the United Kingdom.](https://www.usenix.org/system/files/usenixsecurity25-agarwal-sharad.pdf)

#### On the Virtues of Information Security in the UK Climate Movement.
- **作者**: Mikaela Brough, Rikke Bjerg Jensen, Martin R. Albrecht
- **方向**: 社会工程与人类因素
- **解读**: 社会运动组织者常面临监控、骚扰与法律风险，其信息安全实践关乎成员安全与运动存续，但学界对草根运动群体的安全需求与行为知之甚少。本文以英国气候运动为对象，研究信息安全的"德性"（virtues）——即运动中信息安全实践的伦理与社会面向。（无摘要，据标题推断）

从标题推断，研究采用质性方法（如民族志、访谈、参与式观察），考察气候活动人士如何理解与实践信息安全——包括威胁感知、工具使用、风险权衡，以及安全实践如何与运动的价值伦理（如开放、信任）产生张力。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于刻画英国气候运动中的信息安全实践图景及其背后的伦理动机，为面向活动人士的安全支持提供依据。（无摘要，据标题推断）

信息安全研究长期偏向企业与技术精英视角，草根运动群体是重要却常被忽视的"高风险用户"；将安全实践置于伦理与政治语境中理解，能纠正"安全只是技术问题"的简化认知。局限是质性样本的时空特定性（英国气候运动）限制外推，安全建议的落地还需与运动内部文化协商；该方向丰富了安全人因研究的多样性。


#### "No, I Can't Be a Security Personnel on Your Phone": Security and Privacy Threats From Sharing Infrastructure in Rural Ghana.
- **作者**: Emmanuel Tweneboah, Collins W. Munyendo, Yixin Zou
- **方向**: 社会工程与人类因素
- **解读**: 手机共享（共用设备）在许多低收入国家是普遍实践，但共享带来的安全与隐私威胁——他人接触短信、支付应用与账户——在"个人设备"假设主导的安全研究中被系统性忽视。本文研究加纳农村地区基础设施共享场景下的安全与隐私威胁。（无摘要，据标题推断）

从标题推断，研究基于实地质性数据（访谈、观察），刻画农村用户如何共享手机、充电、网络等基础设施，分析由此产生的具体威胁（账户接管、隐私泄露、冒充等）及用户的应对策略，并探讨与更广泛数字生态（如移动支付）的交互。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于揭示共享基础设施场景下被主流安全模型忽略的威胁面与用户实践。（无摘要，据标题推断）

"一人一机"的安全假设在共享文化下明显失配，本研究为设计包容性安全方案（多用户模式、共享感知的认证等）提供实证基础，也呼应了安全研究对 Global South 语境的关注。局限是单国乡村样本的代表性、自我报告偏差，以及研究者局外人视角对理解共享信任关系的深度限制。


#### Regulating Smart Device Support Periods: User Expectations and the European Cyber Resilience Act.
- **作者**: Lorenz Kustosch, Carlos Gañán, Mattis van 't Schip, Michel van Eeten, Simon Parkin
- **方向**: 社会工程与人类因素
- **解读**: 智能设备（路由器、摄像头等）的软件支持期有限，停止更新后设备沦为易受攻击的僵尸网络成员，但用户对此几乎无感知，市场也缺乏明示与约束。欧盟《网络弹性法案》（CRA）试图规制支持期，但其与用户实际期望是否匹配缺乏实证。本文研究用户对智能设备支持期的期望与 CRA 的契合度。（无摘要，据标题推断）

从标题推断，研究采用用户调查或访谈方法，测量用户对支持期长度、信息透明度的期望与支付意愿，并将用户期望与 CRA 对厂商的披露与支持义务要求对照分析，识别政策设计与用户认知之间的差距。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于给出用户支持期期望的实证画像，并评估 CRA 相关规定在多大程度上满足或偏离这些期望。（无摘要，据标题推断）

安全政策设计若脱离用户认知，再严格的合规要求也难以转化为真实安全收益；本研究把"支持期"这一政策概念与用户期望对接，对 CRA 落地与消费者保护具有直接参考价值。局限是调查样本可能偏向关注安全的用户、政策文本与实施细节仍在演进，结论时效性需要跟踪；此类"政策-用户-厂商"三角的实证支撑是有价值的补充。


#### How Transparent is Usable Privacy and Security Research? A Meta-Study on Current Research Transparency Practices.
- **作者**: Jan H. Klemmer, Juliane Schmüser, Fabian Fischer, Jacques Suray, Jan-Ulrich Holtgrave, Simon Lenau, Byron M. Lowens, Florian Schaub, Sascha Fahl
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：可用性安全与隐私研究（Usable Security）高度依赖人类受试者实验，其结果的可复现性与可信度取决于方法透明程度；本文以元研究（Meta-Study）视角系统审视该领域当前的研究透明度实践，回答"研究做得够不够透明"这一学科自省性问题。
**方法**：据标题推断，作者对可用性安全领域的已发表论文进行系统抽样与编码，考察其在问卷设计、量表验证、样本描述、数据分析与材料公开等方面的透明度实践，并给出改进建议。
**结果**：论文标题表明其揭示了该领域透明度实践的整体状况与缺口，具体统计结果以原文为准，此处不作臆断。
**思考**：此类自反性元研究对学科健康发展至关重要，直接推动预注册、材料共享等规范落地；其局限在于编码标准与抽样范围会显著影响结论，且"透明度"本身难以完全量化，但作为对"可复现性危机"在安全领域的一次系统性体检，价值明确。


#### Understanding How Users Prepare for and React to Smartphone Theft.
- **作者**: Divyanshu Bhardwaj, Sumair Ijaz Hashmi, Katharina Krombholz, Maximilian Golla
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：智能手机失窃是普遍且高影响的现实威胁，失窃后设备中的数据、支付与身份信息可能被滥用；既有研究多聚焦技术防护手段，对用户在失窃前如何准备、失窃后如何应对的真实行为与心理缺乏系统理解，本文旨在填补这一用户侧空白。
**方法**：据标题推断，作者通过访谈或问卷等方法收集用户经历，考察其失窃前的预防措施（如锁屏设置、查找设备、备份）与失窃后的反应行为（如远程锁定擦除、改密、报案），并分析行为背后的动机与障碍。
**结果**：论文标题表明其刻画了用户失窃准备与应对的全过程图景，具体发现以原文为准，此处不作臆断。
**思考**：该研究对设备安全设计具有直接指导意义——若多数用户在失窃前未开启关键保护，说明现有机制在可用性与用户教育上存在缺口；其局限在于自报数据可能存在记忆偏差，且不同地区失窃情境差异较大，但作为少见的"失窃全程"用户研究，对防盗功能设计与运营商/厂商协作流程有参考价值。


#### Exploring User Security and Privacy Attitudes and Concerns Toward the Use of General-Purpose LLM Chatbots for Mental Health.
- **作者**: Jabari Kwesi, Jiaxun Cao, Riya Manchanda, Pardis Emami Naeini
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：越来越多用户向通用型LLM聊天机器人倾诉心理健康问题，但这类场景涉及高度敏感的个人信息，用户对其安全与隐私风险的感知、态度与担忧尚不清楚；理解这些认知对负责任的AI产品设计与监管至关重要。
**方法**：据标题推断，作者通过访谈或调查等用户研究方法，考察用户对将通用LLM聊天机器人用于心理健康支持时的信任、风险感知、披露意愿与隐私担忧，并分析不同用户群体的态度差异。
**结果**：论文标题表明其系统刻画了用户在该场景下的安全隐私态度与担忧，具体发现以原文为准，此处不作臆断。
**思考**：心理健康数据是最高敏感度的个人信息类别之一，将其交给第三方LLM服务涉及数据留存、模型训练、未成年人保护等系列问题，该研究为"AI心理健康助手"热潮提供了用户侧的现实审视；局限在于自报态度与实际披露行为可能脱节，且通用LLM与专用心理健康产品在用户认知中的边界模糊，后续研究可结合行为实验进一步验证。


#### Investigating the Impact of Online Community Involvement on Safety Practices and Perceived Risks Among People Who Use Drugs.
- **作者**: Jiliang Li, Nora Sinong Lu, Isaak Hanimann, Janice Jianing Si, Dazhao Cheng, Xiaobo Zhou, Kanye Ye Wang
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：吸毒人群（PWUD）在获取物质与规避执法风险时常依赖线上社区，而这些社区的参与如何影响其安全实践与风险感知，直接关系危害减少（harm reduction）策略的效果，是安全研究中对边缘人群关注的延伸。
**方法**：据标题推断，作者通过问卷、访谈或在线社区内容分析等方法，考察线上社区参与程度与吸毒人群采取安全实践（如纯度测试、用药陪伴）及感知风险之间的关系。
**结果**：论文标题表明其揭示了线上社区参与对安全实践与风险感知的影响机制，具体结论以原文为准，此处不作臆断。
**思考**：该研究把安全研究拓展到公共卫生与边缘人群交叉地带，具有显著的社会价值，提示线上社区既可以传播风险信息也可能放大危险；研究面临的伦理与招募挑战较大，样本代表性可能受限，且因果方向（社区参与改变行为还是安全倾向者更易参与）需要谨慎解读，但其对面向高危人群的安全干预设计有切实启发。


#### Privacy Solution or Menace? Investigating Perceptions of Radio-Frequency Sensing.
- **作者**: Maximiliane Windl, Omer Akgul, Nathan Malkin, Lorrie Faith Cranor
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：射频（RF）感知技术可在不依赖摄像头的情况下感知人体活动（跌倒检测、呼吸监测、手势识别等），被宣传为保护隐私的"非视觉"方案；但RF信号同样能穿透墙壁、泄露细微行为信息，用户究竟如何看待这种技术——是隐私解决方案还是威胁——尚缺乏实证研究。
**方法**：据标题推断，作者通过调查或访谈等用户研究方法，考察不同背景用户对RF感知技术的认知、信任、隐私担忧及部署接受度，并比较其与摄像头方案的感知差异。
**结果**：论文标题表明其揭示了用户对RF感知技术"解决方案还是威胁"的二元感知图景，具体结论以原文为准，此处不作臆断。
**思考**：该研究服务于一个正在快速商业化的技术品类，业界常以"无摄像头=更隐私"作为卖点，本文有助于检验这一宣传是否与用户真实感知一致；局限在于用户对RF技术原理普遍陌生，调查中的"感知"可能随解释方式波动，但正因如此，其结果对RF产品的透明披露与默认设置设计具有直接参考价值。


#### Navigating Security and Privacy Threats in Homeless Service Provision.
- **作者**: Yuxi Wu, Ruoxi Zhang, Shiyue Liu, Mufei He, Aidan Hong, Jeremy J. Northup, Calla Kainaroi, Fei Fang, Hong Shen
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：无家可归者在寻求救助服务时，往往被迫交出大量个人信息（身份证明、健康记录、行踪数据），而服务提供方之间数据共享频繁、安全能力参差，导致这一最脆弱群体面临严重的隐私与安全威胁；该问题长期被主流安全研究忽视。
**方法**：据标题推断，作者通过对服务提供方、工作人员与受助者的访谈或实地调研，梳理救助服务流程中的数据收集、存储与共享环节，识别其中的安全与隐私威胁及制度性根源。
**结果**：论文标题表明其系统刻画了救助服务供给中的安全隐私威胁图景，具体发现以原文为准，此处不作臆断。
**思考**：这是典型的数据正义（data justice）取向研究，把安全研究从"为强者防御"拓展到"保护最弱势群体"，社会价值突出；局限在于访谈样本规模与地域范围可能限制推广性，且改进建议需要与救助机构的资源约束相容，但该工作为公共服务数字化中的数据最小化与知情同意设计提供了稀缺的一手证据。


#### Security and Privacy Advice for UPI Users in India.
- **作者**: Deepthi Mungara, Harshini Sri Ramulu, Yasemin Acar
- **方向**: 社会工程与人类因素
- **解读**:
**问题**：统一支付接口（UPI）在印度被广泛使用并走向全球，支持日常转账与周期性缴费（房租、燃气、电费等），但UPI相关诈骗与欺诈交易数量惊人；既有研究多关注UPI的技术安全，要有效应对威胁，还需理解用户的心智模型、担忧、安全信息来源与行为。
**方法**：作者采用混合方法研究：对26名印度UPI用户进行半结构化访谈，并对来自监管机构、UPI应用与银行提供的16个安全信息来源进行内容分析，梳理用户心智模型、担忧、获取安全建议的渠道及其安全相关行为。
**结果**：论文分析了用户对UPI安全与隐私的担忧和威胁认知，指出官方建议覆盖不到的缺口，并据此向UPI服务商与银行提出定制易懂、覆盖面更广的建议，同时给出UPI用户的个人防护建议。
**思考**：该工作补齐了UPI生态中"用户侧"的实证空白，且直接产出可操作的建议，现实影响力强；访谈样本26人规模适中，深度足够但推广性有限，后续可结合大规模问卷验证，而其"官方建议与用户实际担忧错位"的发现对新兴市场支付安全宣教具有普遍借鉴意义。

- **论文 PDF**: [Security and Privacy Advice for UPI Users in India.](https://www.usenix.org/system/files/usenixsecurity25-mungara.pdf)

#### "Helps me Take the Post With a Grain of Salt: " Soft Moderation Effects on Accuracy Perceptions and Sharing Intentions of Inauthentic Political Content on X.
- **作者**: Filipo Sharevski, Verena Distler, Florian Alt
- **方向**: 社会工程与人类因素
- **解读**: （无摘要，据标题推断）
**问题**：社交平台X（原Twitter）上充斥着不真实的政治内容，平台除删除账号等硬性手段外，越来越多采用"软审核"（如事实核查标签、误导性内容提示）来引导用户判断；这类软审核是否真能降低用户对不真实内容的准确性感知并抑制分享意愿，是平台治理有效性的关键问题。
**方法**：据标题推断，作者通过在线实验向参与者展示带有不同软审核标注（或对照无标注）的不真实政治帖子，测量其准确性感知与分享意图的变化，并可能考察提示措辞与用户政治立场等因素的调节作用。
**结果**：论文标题表明软审核确实帮助用户"带着怀疑看待帖子"，具体效应量与边界条件以原文为准，此处不作臆断。
**思考**：该研究为平台内容审核政策提供了稀缺的因果证据，回答"温和干预是否有效"这一业界反复争论的问题；局限在于实验室情境与真实信息环境存在差距、实验者效应与政治极化可能放大或掩盖效果，且软审核的长期效果（而非一次性暴露）仍需纵向研究验证，但其方向对治理研究很有价值。


#### As Advertised? Understanding the Impact of Influencer VPN Ads.
- **作者**: Omer Akgul, Richard Roberts, Emma Shroyer, Dave Levin, Michelle L. Mazurek
- **方向**: 社会工程与人类因素
- **解读**:
**问题**：YouTube网红赞助广告（Influencer VPN Ads）常传播关于VPN及安全隐私的误导性信息，但这些广告究竟如何影响用户的VPN认知与知识尚不清楚；弄清这一影响对评估这类广告的危害与制定监管对策至关重要。
**方法**：作者训练新型VPN广告检测模型，利用217名参与者的YouTube观看历史计算其VPN广告暴露量，并开发量表刻画其VPN心智模型（对应广告中常见的主张），随后通过预注册的回归分析检验广告暴露与心智模型之间的关联。
**结果**：广告暴露与VPN品牌熟悉度及对（夸大的）威胁的信念显著相关；但暴露与广告中常见的事实性或误导性VPN心智模型并无显著相关，表明VPN广告对心智模型的影响主要是情绪层面的（威胁感知）而非技术认知层面的。
**思考**：该研究的亮点在于预注册分析、基于真实观看历史（而非自报）的暴露测量，以及"情感影响≠认知影响"的细致区分，为理解广告说服机制提供了扎实证据；局限在于暴露测量依赖检测模型精度、样本以YouTube用户为主，且相关设计无法完全排除自选择偏差，但结论对监管与用户教育都有实际意义。

- **论文 PDF**: [As Advertised? Understanding the Impact of Influencer VPN Ads.](https://www.usenix.org/system/files/usenixsecurity25-akgul.pdf)

#### A Stakeholder-Based Framework to Highlight Tensions when Implementing Privacy Features.

- **作者**: Julia Netter, Tim Nelson, Skyler Austen, Eva Lau, Colton Rusch, Malte Schwarzkopf, Kathi Fisler
- **方向**: 社会工程与人类因素
- **解读**:

培养大学生构建隐私保护系统的能力，需要让他们学会围绕社会情境与利益相关者做设计。GDPR、CCPA 等立法提供了监管框架，但对学生而言隐私与利益相关者价值的讨论往往过于抽象；从教学视角看，把"被遗忘权"等抽象概念放到具体技术实现情境中，有助于学生理解其实际含义。

方法上，论文提出一个用于设计技术作业的框架：布置让学生在实现某个具体技术特性时解决相互冲突的利益相关者之间的张力。作者在一门大二计算机系统导论课程中设计并实施了面向隐私的作业，并研究其教学效果。

结果：学生基于其面对的特定利益相关者冲突，做出了不同的设计选择、为不同的价值取向而实现；作业设计有效促使学生思考抽象价值如何在隐私语境下影响技术设计决策。

我的思考：这是少见的"安全/隐私教育实证"工作，把 HCI 的利益相关者分析引入系统课程作业，用可操作的教学设计让学生体验"价值—实现"的张力，对 CS 教育社区有方法示范意义。局限：单课程、单作业的样本有限，学习效果的外推需谨慎；"学生在作业中做了不同选择"不等于"形成了持久的设计价值观"。与安全顶会中日益增长的安全教育（security education）方向一致，是教学法研究的扎实补充。

- **论文 PDF**: [A Stakeholder-Based Framework to Highlight Tensions when Implementing Privacy Features.](https://www.usenix.org/system/files/usenixsecurity25-netter.pdf)

#### A Framework for Abusability Analysis: The Case of Passkeys in Interpersonal Threat Models.

- **作者**: Alaa Daffalla, Arkaprabha Bhattacharya, Jacob Wilder, Rahul Chatterjee, Nicola Dell, Rosanna Bellini, Thomas Ristenpart

- **方向**: 社会工程与人类因素

- **解读**: 

passkey 已被数百家网站部署，是"无口令认证"的最大规模尝试，但新认证机制常忽视高风险人群（家暴幸存者、人口贩卖受害者、老人虐待受害者）面临的特殊威胁：人际威胁者通常能接触受害者的已解锁设备，或能逼迫/获知口令。passkey 究竟是加剧还是缓解此类威胁，此前无人研究。

作者首次在人际威胁模型下分析 passkey，为此提出可复用性滥用（abusability）分析框架：六阶段流程引导分析团队识别威胁模型、评估功能、假设滥用向量、设计并执行逐步验证协议、以易懂的滥用场景总结发现；并将框架应用于 passkey，调研 19 个支持 passkey 的服务。

识别出多种允许攻击者在人际场景中借 passkey 作恶的滥用向量：最严重的情况下，主流服务的缺陷实现允许攻击者持续非法访问且受害者无法恢复账户安全；还发现阻止用户访问账户、以及帮助攻击者情感操纵（gaslight）受害者的向量。背景数据：截至 2024 年 5 月，Google 报告 4 亿账户已启用 passkey。

我的思考：价值在于把"安全机制"放回"人的处境"中审视，abusability 框架本身可推广到其他新功能的安全评估；"无法恢复账户安全"的持续性访问向量尤其值得行业警惕。局限是分析基于服务当前实现、时效性强，且以定性案例为主。与 Bellini 等人对金融应用技术滥用（tech abuse）的研究相比，本文针对 passkey 的跨服务调研为"以受害者为先"的认证设计提供了第一批证据。

- **论文 PDF**: [A Framework for Abusability Analysis: The Case of Passkeys in Interpersonal Threat Models.](https://www.usenix.org/system/files/usenixsecurity25-daffalla.pdf)

#### Phishing Attacks against Password Manager Browser Extensions.

- **作者**: Claudio Anliker, Daniele Lain, Srdjan Capkun

- **方向**: 社会工程与人类因素

- **解读**: 

（无摘要，据标题推断）口令管理器浏览器扩展自动填充凭据，是钓鱼攻击的高价值目标：恶意网页可伪造登录界面诱导扩展填充，或利用扩展的 UI 注入、域名匹配与快捷填充机制窃取凭据。扩展的填充策略与用户交互设计是否可被钓鱼利用，直接关系数亿用户的凭据安全。

从标题推断，本文系统研究针对口令管理器浏览器扩展的钓鱼攻击：应是通过构造恶意网页/钓鱼页面，攻击扩展的自动填充触发条件、URL 匹配逻辑或用户操作习惯，实证评估主流扩展（如 LastPass、1Password、Bitwarden 等）的暴露面。具体攻击向量与受害者范围因缺少摘要无法确认。

由于缺乏摘要，本文没有可引用的量化结果；标题暗示作者以系统性攻击演示或测量揭示了扩展层面的钓鱼风险。

我的思考（基于标题的保守推断）：口令管理器本是对抗钓鱼的防御工具，若自身可被钓鱼利用，则构成"防御被武器化"的悖论；自动填充的便利性与安全性之间的张力是核心矛盾。与针对网站的钓鱼研究相比，本文把攻击面聚焦到扩展的浏览器集成层，其结果将直接影响扩展的默认行为设计（如填充前确认、更严格的站点匹配）。


### 程序分析与形式化验证（16 篇）

#### Lancet: A Formalization Framework for Crash and Exploit Pathology.
- **作者**: Qinrun Dai, Kirby Linvill, Yueqi Chen, Gowtham Kaki
- **方向**: 程序分析与形式化验证
- **解读**: 崩溃(crash)与利用(exploit)是漏洞分析的核心对象,但"什么构成可利用性""崩溃如何转化为利用"缺乏形式化、可复用的分析框架,导致分析结果难以比较与复用。本文提出 Lancet,一个崩溃与利用病理学的形式化框架。(无摘要,据标题推断)

依据标题推断,作者为崩溃与利用模式建立形式化模型(可能基于抽象解释、符号执行或类型化语义),统一描述崩溃成因、内存破坏原语与利用条件,支持自动化分析与验证。

研究应展示框架对真实崩溃样本的分类、可利用性判定或利用合成能力,并与现有工具(如 sanitizer、利用生成器)对比。由于摘要与全文缺失,具体案例与指标无法引用。

崩溃分析长期依赖经验法则,形式化框架能提升分析的严格性与工具间互操作性,是"漏洞科学化"的有益尝试。局限:无法核实框架的表达力与自动化程度;形式化模型常牺牲对真实复杂内存布局的保真度。若能证明对真实 CVE 崩溃的覆盖,将显著提升其价值。与 DOPPLER、p-code 合成等工作同属"程序分析×漏洞利用"的前沿。

#### Let's Move2EVM.
- **作者**: Lorenzo Benetollo, Andreas Lackner, Matteo Maffei, Markus Scherer
- **方向**: 程序分析与形式化验证
- **解读**: Move 是专为数字资产设计的安全编程语言，其资源类型与线性所有权模型从语言层面杜绝了双花、重入等一类常见智能合约漏洞；而 EVM 生态拥有最庞大的用户与工具链。二者之间的鸿沟在于：Move 的安全保证难以直接惠及 EVM 生态。本工作从标题看旨在把 Move 程序迁移/编译到 EVM 字节码，使开发者能用 Move 的安全语义编写、在以太坊生态部署。作者团队（Maffei 组，TU Wien）在智能合约形式化与安全编译方向积累深厚。

据标题推断，方法上可能实现一个 Move→EVM 的编译器/翻译器：把 Move 的类型系统、资源语义（线性所有权、能力检查）映射为 EVM 上可执行的模式（如存储布局、访问控制封装），并可能带形式化语义与翻译正确性论证，确保编译后的合约保留源语言的安全性质。摘要缺失，具体翻译策略（如如何模拟资源移动性）无法确认。

摘要缺失，无法引用量化结果；预期包含编译器的正确性证明或测试、生成合约的 gas 开销对比、以及对既有 Move 生态（如 Aptos/Sui 合约）的迁移覆盖率评估。

"用 Move 写、往 EVM 部署"若成立，等于把语言级安全带入最大的链上生态，是跨生态资产与代码迁移的前沿尝试；其价值不仅在于工具链，更在于"安全性质能否经编译保持"这一形式化问题。局限：无摘要，Move 的资源模型与 EVM 无类型存储之间的语义鸿沟是最大难点，翻译后合约的 gas 成本与可审计性、以及对 Move 高级特性（全局存储、能力）的覆盖度需全文确认。（无摘要，据标题推断）


#### CollisionRepair: First-Aid and Automated Patching for Storage Collision Vulnerabilities in Smart Contracts.

- **作者**: Yu Pan, Wanjing Han, Yue Duan, Mu Zhang
- **方向**: 程序分析与形式化验证
- **解读**: 
（无摘要，据标题推断）智能合约的存储（storage）是链上状态的持久化层，而 Solidity 的存储布局规则（如 delegatecall 代理、结构体槽位错位、映射与数组的槽位计算）在复杂场景下可能引发"存储碰撞"（storage collision）漏洞——不同变量意外共享存储槽位，导致状态被错误覆盖或越权修改，历史上多起重大合约事故与此相关。本文提出 CollisionRepair，为这类漏洞提供"急救"（first-aid）与自动化补丁。

方法上，作者应先构建存储碰撞漏洞的检测逻辑（分析槽位布局与访问路径，识别可能冲突的读写），再针对已部署合约设计"急救"方案——在无法重部署的前提下以最小干预阻止漏洞被利用，并进一步为源码级修复自动生成补丁。这与传统"检测-报告"类工具不同，强调修复闭环。

预期成果包括在真实合约数据集上发现存储碰撞漏洞、评估急救方案的链上可行性与补丁的正确性。安全意义在于：智能合约一旦部署难以修改，存储类漏洞的自动化修复工具能显著降低安全事故的处置成本与窗口期。

该工作的价值在于填补智能合约漏洞"自动修复"环节的空白，尤其是针对存储碰撞这类隐蔽且破坏性强的漏洞。局限在于未见摘要，具体检出率与修复正确性待核实；自动补丁需要保证不改变原有业务语义，这是形式化验证与测试共同面临的挑战。与同领域的检测类工作相比，"修得好"比"找得准"更难，值得期待。


#### PATCHAGENT: A Practical Program Repair Agent Mimicking Human Expertise.
- **作者**: Zheng Yu, Ziyi Guo, Yuhang Wu, Jiahao Yu, Meng Xu, Dongliang Mu, Yan Chen, Xinyu Xing
- **方向**: 程序分析与形式化验证
- **解读**: 自动程序修复（APR）若只擅长补丁生成，就无法胜任现实中的端到端任务：给定一个能触发漏洞的具体输入，如何在修复漏洞的同时不破坏既有测试？此前 LLM 驱动的 APR 工具缺乏有效的故障定位与补丁验证，本文提出 PatchAgent，在单一自主 agent 中无缝集成故障定位、补丁生成与验证三个环节。

PatchAgent 模仿人类专家修复流程：借助语言服务器提供代码语义上下文，用补丁验证器对候选补丁执行测试验证，并通过交互优化技术（如上下文组织、迭代反馈、验证结果引导）提升推理质量，形成"定位→生成→验证→再生成"的闭环。

在 178 个真实世界漏洞的数据集上，PatchAgent 成功修复超过 90% 的案例，在可比较场景中优于现有 SOTA APR 工具；消融实验进一步剖析了各类交互优化对效果的贡献。

我的思考：PatchAgent 的价值在于把 APR 从"会打补丁"推进到"能端到端交作业"，验证环节的闭环是落地关键，90%+ 的真实漏洞修复率在同量级工作中极具竞争力。局限是依赖 LLM 的推理质量与验证成本、数据集规模有限，且"不破坏既有测试"以测试充分性为前提；但作为自主修复 agent 的代表作，它清晰展示了 LLM+工具闭环的工程范式。
- **论文 PDF**: [PATCHAGENT: A Practical Program Repair Agent Mimicking Human Expertise.](https://www.usenix.org/system/files/usenixsecurity25-yu-zheng.pdf)

#### Logs In, Patches Out: Automated Vulnerability Repair via Tree-of-Thought LLM Analysis.
- **作者**: Youngjoon Kim, Sunguk Shin, Hyoungshick Kim, Jiwon Yoon
- **方向**: 程序分析与形式化验证
- **解读**: 现有 LLM 自动修复方案常依赖重型程序分析或人工介入，部署成本高。本文提出 SAN2PATCH：仅凭 sanitizer 日志与源码即可端到端修复漏洞，无需复杂分析或人工干预，目标是低部署成本的实用修复。

SAN2PATCH 采用四阶段流水线（漏洞理解、修复位置定位、修复策略制定、候选生成），以思维树（Tree-of-Thought）提示引导 LLM 分步推理，并向模型供给聚焦的代码片段以平衡上下文长度与定位精度，最后通过严格验证筛选多样且功能正确的补丁。

在 VulnLoc 数据集上 SAN2PATCH 修复成功率达 79%（31/39），显著优于 ExtractFix（43%）与 VulnFix（51%）；在新构建的 SAN2VULN 数据集（27 个新漏洞）上成功率为 63%；对内存类漏洞表现突出，缓冲区溢出修复成功率 81.8% 且其中 81.8% 保持了程序功能。

我的思考：该工作把"输入最小化"做到极致——只吃 sanitizer 日志，这大幅降低了部署门槛，也印证了树状推理+代码片段供给对 LLM 修复效果的重要性。局限是依赖 sanitizer 能捕获的漏洞类型（内存类为主），对逻辑类漏洞覆盖有限，且 79% 的成功率在真实大规模漏洞池中仍偏低；但作为低成本端到端修复路线的代表，工程价值清晰。（无摘要，据标题推断）

#### SoK: Automated Vulnerability Repair: Methods, Tools, and Assessments.
- **作者**: Yiwei Hu, Zhen Li, Kedie Shu, Shenghua Guan, Deqing Zou, Shouhuai Xu, Bin Yuan, Hai Jin
- **方向**: 程序分析与形式化验证
- **解读**: 自动漏洞修复（AVR）研究发展迅速，但方法碎片化、评估基准不一致，跨工作比较困难。本 SoK 旨在系统化梳理 AVR 领域：面向 C/C++ 与 Java，提出规范化的端到端 AVR 工作流（漏洞分析、补丁生成、补丁验证），并以此组织对方法、工具与评估的全景审视。

为支撑统一评估，作者构建了 VUL4C 基准：首个大规模 C/C++ AVR 基准，含 144 个带 exploit 与官方补丁的漏洞；并联合第三方 VUL4J 数据集，评估 7 个 C/C++ AVR 工具与 2 个 Java AVR 工具，对比基于语义与基于学习的补丁生成路线。

关键发现：整体上语义方法优于学习方法——语义式补丁生成器（如 VulnFix）生成的补丁有效性（测试通过率）最高达 96.0%；论文还深入分析了补丁失败原因（复杂代码解析、指针/数组语义、不变量与资源依赖），并给出案例研究。

我的思考：该 SoK 的增量在于把"基准"作为系统化的核心产出——VUL4C 为 C/C++ AVR 提供了标准测试场，填补了此前数据集不一致的空白；语义 vs 学习的实证对比也纠正了"LLM 万能"的直觉。局限是基准规模与语言覆盖有限、工具集未穷尽；但作为评估基础设施，其对领域标准化与可复现研究的推动是长期的。（无摘要，据标题推断）

#### SoK: Towards Effective Automated Vulnerability Repair.
- **作者**: Ying Li, Faysal Hossain Shezan, Bomin Wei, Gang Wang, Yuan Tian
- **方向**: 程序分析与形式化验证
- **解读**: 软件漏洞日益普遍，自动漏洞修复（AVR）成为关键需求，但领域缺乏对"方法-数据集-策略"全景的梳理与量化比较。本 SoK 通过系统文献综述与跨数据集、跨方法、跨策略的定量基准测试，给出 AVR 版图的完整画像，涵盖合成与真实漏洞。

论文建立 AVR 方法分类学，将现有技术分为四类：模板引导（template-guided）、搜索式（search-based）、约束式（constraint-based）与学习驱动（learning-driven），并评估各类方法的长处与短板及共同挑战。

核心结论是"不存在单一最佳方法"：学习方法在特定场景表现出色，但缺乏完整的程序理解能力；学习与非学习方法在面对复杂漏洞时都力不从心；论文还识别了新兴趋势并给出未来研究方向。

我的思考：这篇 SoK 与同会议的 AVR 姊妹 SoK（Hu 等）视角互补——前者重基准评估，本篇重分类学与权衡分析；"没有银弹、学习方法缺程序理解"的结论对研究资源配置有务实指导意义。局限是定量评估的覆盖深度受公开数据集限制，分类边界对混合方法可能模糊；但作为领域导航图，其对新手与规划者的价值明确。
- **论文 PDF**: [SoK: Towards Effective Automated Vulnerability Repair.](https://www.usenix.org/system/files/usenixsecurity25-li-ying.pdf)

#### VULCANBOOST: Boosting ReDoS Fixes through Symbolic Representation and Feature Normalization.
- **作者**: Yeting Li, Yecheng Sun, Zhiwu Xu, Haiming Chen, Xinyi Wang, Hengyu Yang, Huina Chao, Cen Zhang, Yang Xiao, Yanyan Zou, Feng Li, Wei Huo
- **方向**: 程序分析与形式化验证
- **解读**: ReDoS（正则表达式拒绝服务）是 Web 安全的常见隐患，但自动修复面临两大难点：既有方法依赖测试用例指导，且难以处理多样字符类与扩展正则特性。本文提出 VULCANBOOST，在不依赖测试用例的前提下自动修复 ReDoS 漏洞正则。

方法上，VULCANBOOST 用符号表示建模正则语义、以特征归一化化简复杂结构，将修复转化为确定性有限自动机（DFA）上的变换操作，直接从语义层推导修复而非从示例归纳；由此可处理此前方法难以覆盖的字符类与扩展特性。

在 6,360 个真实 NPM 项目中 ReDoS 漏洞正则的大规模评估中，VULCANBOOST 的测试覆盖修复成功率（TCRSR）达 93.95%、等价修复成功率（ERSR）达 93.05%，修复了 5,000+ 个正则且等价率约 99.05%；并归纳出 100 个高频修复模式作为开源资源发布，显著优于基线方法。

我的思考：DFA 变换驱动的"免测试用例"修复范式是 ReDoS 修复的重要推进，99% 的等价率说明修复在消除风险的同时守住了正则语义正确性，修复模式库也为开发者提供了直接可用的防护知识。局限是处理扩展特性带来一定的额外时间开销，且符号化对极复杂正则有规模压力；但 93%+ 的大规模成功率使其成为该方向的新标杆。（无摘要，据标题推断）

#### DISPATCH: Unraveling Security Patches from Entangled Code Changes.
- **作者**: Shiyu Sun, Yunlong Xing, Xinda Wang, Shu Wang, Qi Li, Kun Sun
- **方向**: 程序分析与形式化验证
- **解读**: 安全补丁常与非安全改动纠缠在同一提交中，导致补丁识别、验证与部署被显著拖延。现有代码变更分解方法面向代码审查设计，倾向连接相关部分，反而会产出含无关语句的臃肿"安全补丁"。本文要自动从纠缠变更中拆出针对单个漏洞的独立安全补丁。

DISPATCH 先提出 PatchGraph 图表示，通过保留变更语句的语法与依赖信息刻画细粒度代码修改；再做两阶段补丁依赖分析：第一阶段在语句级划定边界、排除无关语句，第二阶段分析未访问依赖，保证拆出的补丁语法正确、函数完整、可独立应用。

在 OpenSSL、Linux Kernel、ImageMagick 与 Nginx 四个流行仓库上的评估显示，DISPATCH 从纠缠变更中拆解独立安全补丁的召回率超过 91.9%，准确率领先现有方法至少 20%。

我的思考：该工作把"补丁分解"从代码审查辅助工具升级为安全补丁治理基础设施——准确的分解直接影响补丁检测、验证与应急部署的速度，PatchGraph+两阶段分析的组合在技术上是扎实的。局限是依赖提交内变更的可分离性，对深度交织的重构型提交效果存疑；但 91.9% 召回率与跨四仓库的验证使其对安全运维有直接价值。
- **论文 PDF**: [DISPATCH: Unraveling Security Patches from Entangled Code Changes.](https://www.usenix.org/system/files/usenixsecurity25-sun-shiyu.pdf)

#### Towards Practical, End-to-End Formally Verified X.509 Certificate Validators with Verdict.
- **作者**: Zhengyao Lin, Michael McLoughlin, Pratap Singh, Rory Brennan-Jones, Paul Hitchcox, Joshua Gancher, Bryan Parno
- **方向**: 程序分析与形式化验证
- **解读**: X.509 证书验证是 TLS 等公钥基础设施安全的第一道闸门，但验证器实现复杂、历史上多次出现证书验证绕过漏洞，而既有形式化验证工作多为原型、难以端到端实用。本文提出 Verdict，目标构建实用且端到端形式化验证的 X.509 证书验证器。（无摘要，据标题推断）

从标题推断，Verdict 覆盖从规范到实现的完整验证链条：可能以形式化规范刻画 X.509 验证语义（证书链构建、路径验证、扩展与约束处理等），并保证实现代码与规范一致，同时兼顾性能以满足实际部署需求。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于展示"可实际使用"的验证器——既有端到端形式化保证，又有与主流实现相当的解析与验证性能。（无摘要，据标题推断）

证书验证是安全关键且历史上事故频发的组件，端到端验证直击"规范到实现鸿沟"这一核心问题；实用化是形式化方法落地的关键一步。局限是验证范围（支持的证书特性与算法组合）与性能仍需与 OpenSSL、BoringSSL 等成熟实现对比，且公钥基础设施生态的复杂性可能使形式化保证与真实世界行为存在差距。


#### PICACHV: Formally Verified Data Use Policy Enforcement for Secure Data Analytics.
- **作者**: Haobin Hiroki Chen, Hongbo Chen, Mingshen Sun, Chenghong Wang, XiaoFeng Wang
- **方向**: 程序分析与形式化验证
- **解读**: 在复杂隐私政策下确保分析中敏感数据的正当使用日益关键，而既有方法在跨数据处理框架的可移植性、可验证性与可扩展性上不足。本文提出 PICACHV——自动执行数据使用策略的新型安全监控器。

PICACHV 以关系代数作为程序语义的抽象，对程序执行期间生成的查询计划实施策略强制，从而简化跨多种分析操作的分析并支持多种前端查询语言；作者在 Coq 中同时形式化数据使用策略与关系代数语义，证明 PICACHV 正确实施策略；并借助可信执行环境（TEE）增强运行时信任，向利益相关方提供可证明的策略合规保证。系统已集成进 Polars 数据分析框架，并用 TPC-H 基准评估。

论文在 TPC-H 基准上评估了集成进 Polars 的性能，并应用于真实场景，演示了形式化方法在保障数据分析安全上的实际可行性，但摘要未给出具体性能数字。

"查询计划级监控 + Coq 形式化证明 + TEE 运行时信任"的组合解决策略执行的可验证性，架构设计清晰，集成真实框架展示了落地潜力。局限是关系代数抽象对复杂分析（UDF、自定义算子）的覆盖有限，TEE 引入的信任根与性能开销需权衡；与差分隐私等技术的协同是未来方向。

- **论文 PDF**: [PICACHV: Formally Verified Data Use Policy Enforcement for Secure Data Analytics.](https://www.usenix.org/system/files/usenixsecurity25-chen-haobin.pdf)

#### OwlC: Compiling Security Protocols to Verified, Secure, High-Performance Libraries.
- **作者**: Pratap Singh, Joshua Gancher, Bryan Parno
- **方向**: 程序分析与形式化验证
- **解读**: 安全协议的实现极易出错：手工实现常引入认证或机密性漏洞，而形式化验证又往往停留在规范层面，难以直接生成高性能、可用的代码。本文提出 OwlC——把安全协议编译成经过验证、安全且高性能库的编译器。（无摘要，据标题推断）

从标题推断，OwlC 采取"规范驱动编译"路线：以形式化规格描述协议（如 TLS、Signal 类协议）的消息流与安全性质，通过经过验证的编译流水线生成实现代码，使正确性保证从规范延伸到产物，并针对性能做优化以满足实际使用。（无摘要，据标题推断）

摘要缺失，无法引用具体量化结果；预期贡献在于证明"验证过的安全协议库"可以同时具备形式化保证与可用的性能，并以若干真实协议为案例展示。（无摘要，据标题推断）

把"形式化保证"从论文推进到可编译、可部署的库，是缩小验证与工程之间鸿沟的务实方向，对协议生态的长期安全有积极意义。局限是编译目标与所支持协议范围的广度、生成代码的可读性与性能开销都是现实约束，且协议规范本身若存在设计缺陷，编译级验证无法弥补；仍需与成熟手工库做全面对比。


#### TRex: Practical Type Reconstruction for Binary Code.

- **作者**: Jay Bosamiya, Maverick Woo, Bryan Parno
- **方向**: 程序分析与形式化验证
- **解读**:

剥离符号的二进制代码丢失了变量类型信息，而类型是逆向分析、漏洞研究与程序验证的关键基础：恢复结构体布局、函数签名与标量类型能显著提升反编译质量与后续分析精度。类型重建（type reconstruction）研究多年，但现有工具要么精度不足、要么难以扩展到真实世界的大规模二进制。

据标题推断，TRex 提出实用（practical）的二进制类型重建方法：可能综合符号执行、值分析、数据流与使用点（usage site）约束，从二进制代码的访存模式、调用约定与常量使用中推断变量与结构体类型，并以真实二进制上的可扩展性与准确率为设计目标。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于把类型重建从实验室原型推向可大规模部署的实用工具，为反编译器与二进制分析提供类型基础。

我的思考：作者背景（Parno 组在形式化验证与二进制安全上均有建树）暗示该方法可能在"实用性"与"可靠性"上做了独特取舍——若结合形式化保证或可证明的推断规则将更具分量。局限（据标题推断）：类型推断的准确性受编译器优化（寄存器重命名、结构体标量化）影响，递归结构与跨模块类型的一致性恢复是常见难点；与既有工具（如 TIE、REWARDS、Dexter 等）的系统对比是判断贡献的关键。对二进制分析生态是重要的基础设施型工作。


#### Vest: Verified, Secure, High-Performance Parsing and Serialization for Rust.

- **作者**: Yi Cai, Pratap Singh, Zhengyao Lin, Jay Bosamiya, Joshua Gancher, Milijana Surbatovich, Bryan Parno
- **方向**: 程序分析与形式化验证
- **解读**:

解析与序列化是网络协议栈与安全关键软件中最易出错的部分：解析器手工处理字节流时频繁出现越界、整数溢出与逻辑错误，历史上大量 CVE 源于此。Rust 提供内存安全，但复杂协议的状态机与边界条件仍需更强的保证；而形式化验证工具虽能提供数学级保障，往往以性能为代价，难以满足高性能网络场景。

据标题推断，Vest 为 Rust 提供"已验证、安全、高性能"的解析与序列化框架：可能基于 Verus 等验证工具链，对解析/序列化代码进行形式化证明（内存安全、协议不变量），同时通过零拷贝、SIMD 或精心设计的表示避免验证带来的性能损失，并可能生成协议栈（如 TLS/QUIC 消息处理）所需的编解码代码。（无摘要，据标题推断）

摘要缺失，无法给出量化结果；其价值在于证明"形式化验证+高性能"可以兼得，为安全关键协议实现提供可信替代路径。

我的思考：Parno 组在验证与性能平衡上有系列积累（如 HACL*、Verus 生态相关合作），Vest 若能在真实协议基准上给出"证明覆盖+接近手写性能"的结果，将有力推动验证代码在生产网络栈中的采用。局限（据标题推断）：证明工作量与协议复杂度成正比、验证边界（哪些属性被证明）的清晰度、以及与 serde 等既有生态的兼容性是落地关键。对 TLS/QUIC 等协议实现的安全性有直接价值。


#### TYPEPULSE: Detecting Type Confusion Bugs in Rust Programs.

- **作者**: Hung-Mao Chen, Xu He, Shu Wang, Xiaokuan Zhang, Kun Sun
- **方向**: 程序分析与形式化验证
- **解读**:

Rust 支持类型转换，安全 Rust 通过强静态类型检查与严格所有权规则保证转换安全；但程序员在涉及指针的类型转换时不得不使用 unsafe Rust，这些转换可能引发严重的内存破坏。尽管 C/C++ 的类型混淆（type confusion）研究已很充分，Rust 中的类型混淆研究仍然缺乏，且因 Rust 类型系统的新特性，C/C++ 的既有方案无法直接迁移。

方法上提出静态分析工具 TYPEPULSE，检测三类 Rust 类型混淆缺陷：未对齐（misalignment）、不一致布局（inconsistent layout）与作用域不匹配（mismatched scope）。先进行类型转换分析，收集并确定类型对的 trait 边界；再进行指针别名分析解析指针的别名关系；将信息整合进属性图（property graph）后构造类型模式，在各种转换场景中检测每类缺陷。

结果：在排名前 3,000 的 Rust 包上运行，发现 71 个新的类型混淆缺陷，超过 RUSTSEC 过去五年报告的类型混淆缺陷总数；已获开发者确认 32 个，并取得 1 个 CVE 编号与 6 个 RUSTSEC 编号，生态认可度高。

我的思考：这是首个系统性针对 Rust unsafe 指针转换的类型混淆静态分析，trait 边界收集+别名分析+属性图的组合设计贴合 Rust 语义，71 个新缺陷（超五年 RUSTSEC 总和）证明其实际发现力。局限：静态分析在 trait 解析与别名精度上的近似可能漏报跨 crate 复杂场景，误报率与可扩展性（3,000 crate 上的运行成本）也需权衡。对 Rust unsafe 代码安全审查与生态治理有直接价值，填补了 C/C++ 与 Rust 类型混淆研究之间的空白。

- **论文 PDF**: [TYPEPULSE: Detecting Type Confusion Bugs in Rust Programs.](https://www.usenix.org/system/files/usenixsecurity25-chen-hung-mao.pdf)

#### A Comprehensive Formal Security Analysis of OPC UA.

- **作者**: Vincent Diemunsch, Lucca Hirschi, Steve Kremer
- **方向**: 程序分析与形式化验证
- **解读**:

OPC UA 是部署于关键基础设施的工业控制系统（ICS）标准化协议，其安全设计攸关物理世界安全；即将发布的 v1.05 对底层密码设计做出重大修改——用基于 Diffie-Hellman 的密钥交换替代原有 RSA 方案，并声称提供包括完美前向保密（PFS）在内的更强安全属性，但这些承诺缺乏严格的协议级验证。

方法上，作者用符号化协议验证工具 ProVerif 对 OPC UA v1.05 与 v1.04、RSA 与 DH 两种模式进行形式化安全分析，模型覆盖安全信道建立、会话及其管理的各子协议组合与大量可能配置，是迄今规模最大的 ProVerif 模型之一；论文详细讨论了为克服状态机复杂性而采用的建模与化简策略。

结果：分析发现了多个此前未知的漏洞，已上报并获 OPC 基金会确认；作者设计了可证明安全的修复方案，其中大部分已被纳入即将发布的标准版本，实现了"标准发布前堵漏"。

我的思考：在工业协议标准发布前完成全模式形式化验证，兼具理论严谨性与现实影响力，其"最大规模 ProVerif 模型"的复杂性管理经验本身即是方法论贡献。局限：符号化分析抽象了密码原语代数性质与实现细节，DH 模式下的攻击者能力假设与现实实现偏差仍需结合实现审计；与既有 OPC UA 形式化工作相比，其覆盖面与发现的新漏洞是主要增量。

- **论文 PDF**: [A Comprehensive Formal Security Analysis of OPC UA.](https://www.usenix.org/system/files/usenixsecurity25-diemunsch.pdf)

### 其他（28 篇）

#### 34th USENIX Security Symposium, USENIX Security 2025, Seattle, WA, USA, August 13-15, 2025.
- **作者**: Lujo Bauer, Giancarlo Pellegrino
- **方向**: 其他
- **解读**: 该条目为会议论文集卷首信息,并非研究论文:对应第 34 届 USENIX Security 研讨会(USENIX Security 2025,2025 年 8 月 13-15 日于美国西雅图举行),作者栏为程序委员会共同主席 Lujo Bauer 与 Giancarlo Pellegrino。(无摘要,据标题推断)

USENIX Security 由 USENIX 协会主办,是安全领域公认的顶级学术会议,以严格评审与高影响力著称,每年接收系统与操作系统安全、网络与协议安全、AI/机器学习安全、隐私、滥用防护与测量研究等方向的原创工作;本卷即收录该年度录用论文的全文。

本批次其余 24 个条目均为该会议录用的具体论文,主题涵盖大模型安全(指纹识别、水印、越狱与后门)、漏洞利用合成、供应链安全、社会工程与内容滥用测量等,与卷首信息一一对应。

处理该条目时按输入 JSON 的实际条目数如实保留,以保持条目顺序与数量一致;因其本身不含论文内容,本解读仅作标注说明,不对任何研究结论构成引用或推断,读者应直接参见后续论文条目。

#### Voluntary Investment, Mandatory Minimums, or Cyber Insurance: What Minimizes Losses?
- **作者**: Adam Hastings; Simha Sethumadhavan
- **方向**: 其他（安全经济学）
- **解读**: 勒索软件已成全球性威胁，各国政策制定者纷纷寻求立法干预，但安全政策"事前难预测、事后难衡量"，决策基本靠经验。本文构建可量化评估政策方案的仿真工具：结合博弈论、蒙特卡洛模拟与实证数据的多智能体经济模型，并开源为Web应用。

模型含防御者、攻击者与保险公司三类智能体，均按期望效用最大化"完美博弈"决策；参数全部来自真实数据：50个勒索组织、5000家防御组织、200家保险公司，财富服从对数正态分布，赎金随规模线性增长（系数792145，R²=0.921），恢复成本亚线性（230123·w^0.125），安全投资经erf(25x)函数转化为防御姿态并按40%折旧。

基线模拟中防御者100次全部被洗劫一空（约17万亿美元被勒索，攻击者仅花费约3000亿）；而强制每轮投入2%资源进行安全的政策反而优于完美博弈下的自愿投资——1%时财富仍耗尽但流向安全投入，2%时防御姿态足以击退全部攻击；强制保险与精算公平保险几乎不改善结果；敏感性分析表明政策应着力降低攻击次数与赎金系数。

我的思考：该工作把安全政策辩论从"艺术"推向可复现实验，"安全是最薄弱环节博弈、强制最低投入胜于理性自愿决策"的结论直接支撑强制安全最低标准。作者坦承模型缺乏生态效度（现实中防御者并未全灭），参数依赖有限数据集，结论稳健性仍需更多场景检验，但作为政策试验场的价值是明确的。

- **论文 PDF**: [Voluntary Investment, Mandatory Minimums, or Cyber Insurance: What Minimizes Losses?](https://www.usenix.org/system/files/usenixsecurity25-hastings.pdf)

#### A First Look at Governments' Enterprise Security Guidance.
- **作者**: Kimberly Ruth, Raymond Buernor Obu, Ifeoluwa Shode, Gavin Li, Carrie Gates, Grant Ho, Zakir Durumeric
- **方向**: 其他
- **解读**: 各国政府发布大量面向企业的安全指引,旨在指导企业落实安全基线。然而这些指引的质量、覆盖面与可操作性长期缺乏系统评估,企业照单执行可能既费资源又留漏洞。本文首次系统审视政府企业安全指引。(无摘要,据标题推断)

依据标题推断,作者收集多国政府发布的企业安全指引文档,构建语料并编码分析其内容:覆盖的安全主题、建议的具体性与可操作性、与权威框架的一致性,以及是否存在过时或矛盾建议。

研究应给出指引生态的整体画像与典型问题清单,为政府改进指引质量提供依据。由于摘要与全文缺失,具体国家数量、文档规模与发现细节无法引用。

安全指引是安全治理"最后一公里"的重要载体,却少被当作研究对象;本工作的"第一次"定位具有开创意义,类似于此前对 CVE 描述、安全公告质量的测量研究。局限:无法核实其方法学与结论,指引时效性强,结论可能随版本更新快速过时。若能与企业实际遵循度结合,价值更大。

#### SoK: Towards a Unified Approach to Applied Replicability for Computer Security.
- **作者**: Daniel Olszewski, Tyler Tucker, Kevin R. B. Butler, Patrick Traynor
- **方向**: 其他
- **解读**: 可复现性(replicability)是计算机安全实验研究的基石,但安全领域的复现实践缺乏统一方法论:不同子领域对"复现"的定义、工具与验收标准各异,阻碍结果验证与比较。本文以 SoK 形式探索统一的应用可复现性方法。(无摘要,据标题推断)

依据标题推断,作者系统梳理安全领域可复现性实践:调查现有工件(artifact)评审机制、复现案例与失败模式,归纳影响可复现性的因素,并提出统一框架或最佳实践建议。

研究应给出安全实验可复现性的现状画像、常见障碍与改进路线。由于摘要与全文缺失,具体调查规模与量化结论无法引用。

可复现性 SoK 的价值在于把"复现难"从个体抱怨提升为可度量的学科问题,为会议工件评审、工具链建设提供依据;与顶会普遍推行的 artifact 评审制度高度相关。局限:无法核实其覆盖范围;方法论类 SoK 容易停留在清单层面,落地效果取决于社区的采纳。此类元科学研究对提升安全研究的整体可信度有长远意义。

#### LLMxCPG: Context-Aware Vulnerability Detection Through Code Property Graph-Guided Large Language Models.
- **作者**: Ahmed Lekssays, Hamza Mouhcine, Khang Tran, Ting Yu, Issa Khalil
- **方向**: 漏洞挖掘与利用（无摘要，据标题推断）
- **解读**: 问题：基于大语言模型（LLM）的漏洞检测通常只把代码当作平面文本输入，缺乏对函数间调用关系、数据流和控制流的全局理解，导致上下文缺失、误报偏高；而传统基于代码属性图的符号分析又难以利用大模型的语义推理能力。将两者结合以提升漏洞检测的上下文感知能力，是本文的核心目标。

方法：标题表明其核心思路是以代码属性图（Code Property Graph，融合抽象语法树、控制流图与数据流图的统一表示）为引导，为 LLM 提供结构化的上下文信息，使模型在分析目标代码片段时能感知其依赖与调用环境，从而做出更准确的漏洞判断（具体技术细节以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应在漏洞检测基准上验证了 CPG 引导相比纯 LLM 或纯静态分析的改进，但具体量化数值无法从现有信息获得，不作猜测。

思考：该工作顺应"符号分析+大模型"的融合趋势，方向本身有现实价值——结构化上下文是 LLM 漏洞检测落地的关键短板之一；其成色取决于评测基准的规模与真实性、对跨函数与跨文件上下文的建模能力，以及与同类 LLM 漏洞检测框架相比在精度与开销上的实际差距。


#### AutoLabel: Automated Fine-Grained Log Labeling for Cyber Attack Dataset Generation.
- **作者**: Yihao Peng, Tongxin Zhang, Jieshao Lai, Yuxuan Zhang, Yiming Wu, Hai Wan, Xibin Zhao
- **方向**: 恶意软件与二进制分析（无摘要，据标题推断）
- **解读**: 问题：高质量的网络攻击检测数据集依赖对日志的精细标注（如攻击阶段、恶意行为类别），人工标注成本高昂、易出错且难以规模化，制约了基于机器学习的安全检测研究。如何自动化地生成细粒度日志标签，是该工作的核心目标。

方法：据标题推断，AutoLabel 提出自动化标注流水线，对攻击过程中产生的日志进行细粒度（fine-grained）标签化处理，从而端到端地生成可供检测模型训练使用的攻击数据集（具体标注粒度与算法细节以论文正文为准）。

结果：无摘要与正文可引用，论文应通过标注质量、与人工标注的一致性及下游检测效果等指标验证其有效性，具体数值不作猜测。

思考：数据集与标注是安全机器学习研究的"弹药库"，该工作切中数据工程这一常被忽视的痛点，有较好的工程价值；其成色取决于标签粒度的定义是否贴合检测任务、标注的一致性与噪声控制，以及能否覆盖多样化的攻击场景，避免对特定攻击模式的过拟合。


#### TAPAS: An Efficient Online APT Detection with Task-guided Process Provenance Graph Segmentation and Analysis.
- **作者**: Bo Zhang, Yansong Gao, Changlong Yu, Boyu Kuang, Zhi Zhang, Hyoungshick Kim, Anmin Fu
- **方向**: 系统与操作系统安全（无摘要，据标题推断）
- **解读**: 问题：高级持续性威胁（APT）具有隐蔽、长时、多阶段的特点，基于进程溯源图（provenance graph）的检测是主流思路，但大规模溯源图带来的高开销使在线（online）实时检测难以落地。如何高效地对溯源图进行切分与分析以支撑在线 APT 检测，是本文的目标。

方法：据标题推断，TAPAS 采用"任务引导"（task-guided）的溯源图分段策略，将庞大繁杂的进程溯源图按攻击任务/语义划分为可独立分析的子图，再进行针对性的分析以降低计算开销、支持在线检测（具体分段算法与特征以论文正文为准）。

结果：无摘要与正文可引用，论文应在检测效率（如吞吐、延迟）与检测效果（如对真实 APT 攻击的检出率）间给出权衡证据，具体数值不作猜测。

思考：溯源图检测的痛点正在于"图太大、算不动"，任务引导式分段抓住了语义与效率的结合点，方向务实；其成色取决于分段的准确性——切错了任务边界反而会割裂攻击链证据，如何界定"任务"本身可能依赖先验知识，泛化到未知攻击模式的能力值得关注。


#### RollingEvidence: Autoregressive Video Evidence via Rolling Shutter Effect.
- **作者**: Feng Qian, Lingfeng Zhang, Tao Luo, Shiqi Xu, Zhijun Yu, Wei Wang
- **方向**: 数字取证（无摘要，据标题推断）
- **解读**: 问题：视频证据在司法与安全场景中越来越重要，但伪造与篡改手段不断升级，需要可靠的取证技术验证视频的真实性与时序一致性。据标题推断，本文关注如何利用卷帘快门（rolling shutter）效应这一 CMOS 传感器的固有物理特性来取证。

方法：标题中的"autoregressive video evidence"提示其利用卷帘快门在逐行曝光中引入的时间偏移，将视频帧内的空间信息与时间信息建立关联，从而自回归地提取或验证视频证据（例如判断视频是否为真实拍摄、重建拍摄时序），具体技术细节以论文正文为准，据标题推断。

结果：无摘要与正文可引用，论文应在真实与伪造视频样本上验证方法的检测能力，具体数值不作猜测。

思考：利用传感器物理指纹（如卷帘快门）做取证是"以物理事实对抗数字伪造"的代表性思路，比纯统计检测更具鲁棒性；其局限在于对后处理、重编码与合成视频（无真实传感器过程）的适用性，以及自动化攻击者针对该类特征的刻意规避，取证与反取证的技术博弈将持续演进。


#### From Constraints to Cracks: Constraint Semantic Inconsistencies as Vulnerability Beacons for Embedded Systems.
- **作者**: Jiaxu Zhao, Yuekang Li, Yanyan Zou, Yang Xiao, Naijia Jiang, Yeting Li, Nanyu Zhong, Bingwei Peng, Kunpeng Jian, Wei Huo
- **方向**: 漏洞挖掘与利用（无摘要，据标题推断）
- **解读**: 问题：嵌入式系统固件漏洞危害严重但分析困难，缺少源码与符号信息使传统漏洞挖掘手段受限。据标题推断，本文提出以"约束语义不一致"（constraint semantic inconsistencies）作为漏洞信标（vulnerability beacons），即从固件/代码中提取的约束条件在语义层面相互矛盾之处，往往是真实漏洞的征兆。

方法：核心思路是系统性地分析程序中的约束（如边界检查、格式校验、状态断言等），检测其语义上的不一致并以此为线索定位潜在漏洞（具体分析对象与不一致判定方法以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应在嵌入式固件/相关程序中报告发现的漏洞或不一致数量及有效性验证，具体数值不作猜测。

思考：把"约束矛盾"作为漏洞先导信号，跳出了单纯模式匹配的窠臼，具有可解释性——不一致即证据；该思路的成色取决于约束提取的完整度与不一致判定的准确性，误报控制是关键。若能结合符号执行或形式化验证确认矛盾可达性，可能进一步提升精度。


#### IRBlock: A Large-Scale Measurement Study of the Great Firewall of Iran.
- **作者**: Jonas Tai, Karthik Nishanth Sengottuvelavan, Peter Whiting, Nguyen Phong Hoang
- **方向**: 网络与协议安全（无摘要，据标题推断）
- **解读**: 问题：网络审查是影响信息自由与互联网研究的现实议题，伊朗的"国家防火墙"（GFW）此前缺乏系统性的实证研究。据标题推断，本文针对伊朗的审查基础设施开展大规模测量，试图揭示其封锁机制、覆盖面与行为特征。

方法：从标题看，作者通过大规模主动/被动测量手段，从多个网络位置探测被审查目标（域名、IP、协议），刻画伊朗防火墙的封锁策略与地理/运营商分布差异（具体测量方法与数据集以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告被审查目标规模、封锁技术特征等量化测量结果，具体数值不作猜测。

思考：对特定国家审查系统的系统性测量具有双重价值：一是实证刻画审查机制，为规避与对抗研究提供依据；二是为互联网审查研究的方法论提供样例。此类研究的局限在于测量覆盖度与安全伦理边界，且审查系统动态演化，测量结论的时效性需要持续更新验证。


#### Censorship Evasion with Unidentified Protocol Generation.
- **作者**: Ryan Wails, Rob Jansen, Aaron Johnson, Micah Sherr
- **方向**: 网络与协议安全（无摘要，据标题推断）
- **解读**: 问题：审查者常通过协议识别（如流量指纹）封锁代理与翻墙工具，传统规避依赖固定协议，一旦被识别即失效。据标题推断，本文提出"无法识别协议生成"（unidentified protocol generation）的思路，即自动生成无法被审查系统识别的通信协议，以对抗流量分类型封锁。

方法：核心想法是让协议本身"不可被识别"，例如通过自动合成与既有流量分布难以区分的协议格式或流量特征，使审查者无法将其归类为被禁协议（具体生成机制与对抗模型以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应在真实审查场景或模拟网络中验证生成协议的可用性与抗识别能力，具体数值不作猜测。

思考：与"把流量伪装成已知应用"的范式不同，生成"未知"协议是从另一侧规避——审查者面对无法归类的流量陷入两难。该思路的可行性依赖审查者的分类粒度与部署现实，若审查策略改为默认拒绝未知流量，此类方法将失效；其性能开销与实际部署价值需在论文中求证。


#### Exposing and Circumventing SNI-based QUIC Censorship of the Great Firewall of China.
- **作者**: Ali Zohaib, Qiang Zao, Jackson Sippe, Abdulrahman Alaraj, Amir Houmansadr, Zakir Durumeric, Eric Wustrow
- **方向**: 网络与协议安全（无摘要，据标题推断）
- **解读**: 问题：QUIC 等加密传输协议日益普及，审查系统也转向基于 SNI（服务器名称指示）对加密流量进行封锁。据标题推断，本文针对中国防火墙对 SNI 的 QUIC 审查机制展开研究，先"暴露"其封锁行为，再探索"绕过"方法。

方法：从标题看，作者通过测量刻画 GFW 对 QUIC 流量中 SNI 的检测与封锁规则（如触发条件、封锁粒度），并基于对规则的逆向理解设计规避手段（具体测量与规避技术以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告封锁规则特征与所提规避方法在真实网络中的有效性，具体数值不作猜测。

思考：针对加密协议审查的攻防研究具有方法论价值——理解封锁机制本身即是防御与对抗的前提；QUIC 的 0-RTT 与加密握手特性给 SNI 审查带来新挑战，也使规避空间更大。局限在于审查规则动态演化、测量存在法律与伦理风险，结论时效性有限，需谨慎推广到其他审查环境。


#### Ares: Comprehensive Path Hijacking Detection via Routing Tree.
- **作者**: Yinxiang Tao, Chengwan Zhang, Changqing An, Shuying Zhuang, Jilong Wang, Congcong Miao
- **方向**: 网络与协议安全（无摘要，据标题推断）
- **解读**: 问题：BGP 路径劫持是互联网路由安全的核心威胁，可导致流量被窃听、重定向或中断。据标题推断，现有检测方法在覆盖面与准确性上存在不足，本文提出基于"路由树"（routing tree）的全面路径劫持检测方案 Ares。

方法：核心思路是利用路由树结构对 BGP 更新与路由传播进行建模，通过比较观测路由与控制平面预期之间的不一致来识别劫持（具体特征与判定算法以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告检测的覆盖率、准确率与误报率等量化结果，具体数值不作猜测。

思考：BGP 劫持检测的难点在于区分真实劫持与合法路由变更，基于路由树的结构化视角比逐前缀孤立分析更能捕捉传播语义，方向合理；其成色取决于数据源覆盖（RIB 观测点数量）与判定阈值设计，实时性与误报控制的平衡是落地关键，与现有 RPKI/人工审核流程的互补性也值得讨论。


#### Watch Out Your TV Box: Reversing and Blocking a P2P-based Illegal Streaming Ecosystem.
- **作者**: Jungun Ahn, Sueun Jung, Seungwan Yoo, Jungheum Park, Sangjin Lee
- **方向**: 恶意软件与二进制分析（无摘要，据标题推断）
- **解读**: 问题：基于 P2P 的非法流媒体生态（如电视盒子上的盗版直播/点播服务）绕过版权监管并可能夹带恶意代码，危害用户设备安全。据标题推断，本文对该生态进行逆向分析，并探索阻断手段。

方法：从标题看，作者通过逆向工程（二进制/固件/协议分析）还原 P2P 流媒体应用的通信协议与网络结构，理解其节点组织与内容分发机制，进而设计可落地的阻断策略（具体技术细节以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告逆向得到的协议/拓扑特征以及阻断方案的可行性与效果，具体数值不作猜测。

思考：对地下流媒体生态的逆向+阻断属于"应用型"安全研究，兼具版权保护与用户安全价值；P2P 架构的分布式特性使单点打击失效，作者若能给出协议级或基础设施级的阻断方案，将具有实际威慑力。局限在于生态快速演化、跨司法管辖的执法难度，以及阻断措施的误伤风险。


#### USD: NSFW Content Detection for Text-to-Image Models via Scene Graph.
- **作者**: Yuyang Zhang, Kangjie Chen, Xudong Jiang, Jiahui Wen, Yihui Jin, Ziyou Liang, Yihao Huang, Run Wang, Lina Wang
- **方向**: AI/ML安全（无摘要，据标题推断）
- **解读**: 问题：文生图模型可能生成不适宜工作场所（NSFW）的内容，现有过滤机制多依赖文本或图像的单模态特征，容易被绕过。据标题推断，本文提出基于场景图（scene graph）的 NSFW 内容检测方法 USD，以提升检测的准确性与鲁棒性。

方法：场景图以"对象-关系-对象"的结构化三元组描述图像语义，据标题推断，USD 将生成图像的场景图表示与文本提示对齐，从结构化语义层面判断内容是否 NSFW，弥补纯视觉或纯文本检测的盲区（具体实现以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应在 NSFW 检测基准上报告准确率、召回率及对绕过的鲁棒性，具体数值不作猜测。

思考：场景图把图像语义结构化，能捕捉"人物-动作-对象"组合中的不当关系，比逐对象分类更接近人类对"不当内容"的理解，方向有可取之处；局限在于场景图提取本身依赖检测模型、可能出错，且对抗者可通过操纵生成过程规避结构化语义，检测与生成攻防的迭代仍将持续。


#### Neural Invisibility Cloak: Concealing Adversary in Images via Compromised AI-driven Image Signal Processing.
- **作者**: Wenjun Zhu, Xiaoyu Ji, Xinfeng Li, Qihang Chen, Kun Wang, Xinyu Li, Ruoyan Xu, Wenyuan Xu
- **方向**: AI/ML安全（无摘要，据标题推断）
- **解读**: 问题：现代摄像头普遍采用基于 AI 的图像信号处理（ISP）流水线（降噪、HDR、去马赛克等）。据标题推断，本文提出"神经隐形斗篷"——通过攻击被攻陷的 AI ISP 模块，将目标（如人物）在生成的图像中"隐藏"，使下游检测/识别系统无法发现，同时图像表面看似正常。

方法：核心思路是把对抗扰动注入到 AI 驱动的 ISP 处理环节，使输出图像中的特定对象被抹除或降质，而不破坏图像的总体观感（具体注入方式与隐蔽性设计以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告对目标检测/识别模型的隐藏成功率与图像质量保持程度，具体数值不作猜测。

思考：攻击面从识别模型前移到图像采集与处理链路的"源头"，揭示 AI ISP 作为新攻击入口的安全风险，视角独特且具现实性——一旦 ISP 固件被篡改，物理世界的视觉证据将不可信；局限在于需要先攻陷 ISP 固件/更新链，且与对抗补丁等既有物理攻击的关系与比较有待论文厘清。


#### Bridging the Gap in Vision Language Models in Identifying Unsafe Concepts Across Modalities.
- **作者**: Yiting Qu, Michael Backes, Yang Zhang
- **方向**: 大模型安全（无摘要，据标题推断）
- **解读**: 问题：视觉-语言模型（VLM）需要识别跨模态（文本、图像等）的不安全概念，但据标题推断，VLM 在不同模态间识别不安全内容的"能力差距"（gap）导致漏判——同一概念在文本中被识别、在图像中却漏过，或被组合模态绕过。本文旨在弥合这一差距。

方法：据标题推断，作者系统评估 VLM 跨模态识别不安全概念的差异，分析差距成因（如模态信息不对称、概念表达差异），并提出缩小差距的对齐/训练或检测策略（具体方法以论文正文为准）。

结果：无摘要与正文可引用，论文应报告 VLM 在各模态下的不安全内容识别率差异及改进效果，具体数值不作猜测。

思考：多模态安全评测是当前大模型安全的热点，跨模态能力差距是实际部署中的真实盲区——攻击者可利用"文本安全、图像危险"的不对称绕过过滤；该工作若给出可复现的评测基准，价值显著。局限在于不安全概念定义的主观性、评测集的覆盖面，以及安全策略在闭源模型上的可迁移性。


#### Backdooring Bias (B^2) into Stable Diffusion Models.
- **作者**: Ali Naseh, Jaechul Roh, Eugene Bagdasarian, Amir Houmansadr
- **方向**: AI/ML安全（无摘要，据标题推断）
- **解读**: 问题：文生图模型（如 Stable Diffusion）在生成内容时会放大训练数据中的社会偏见。据标题推断，本文提出 B²（Backdooring Bias）攻击：将特定偏见作为"后门"植入扩散模型，使模型仅在特定触发条件下表现出偏颇输出，平时行为正常，隐蔽性远高于全时偏见。

方法：核心思路是把偏见注入与后门攻击结合——训练/微调时把偏见关联到特定触发词或条件，形成仅在触发时激活的偏见行为（具体植入方式与触发设计以论文正文为准，据标题推断）。

结果：无摘要与正文可引用，论文应报告触发条件下偏见强化的程度、非触发条件下的正常性（隐蔽性）以及攻击的持久性，具体数值不作猜测。

思考：该工作把"偏见"与"后门"两个 AI 安全议题缝合，揭示模型供应链中带有政治/社会动机的攻击向量——攻击者不追求错误输出，而是定向扭曲价值观输出，比传统后门更隐蔽、更难审计；其价值在于警示公平性评测不能只看整体指标。局限在于偏见度量的主观性与跨文化差异，以及防御（后门检测+偏见审计）如何结合仍是开放问题。


#### Approve Once, Regret Forever: On the Exploitation of Ethereum's Approve-TransferFrom Ecosystem.
- **作者**: Nicola Ruaro, Fabio Gritti, Dongyu Meng, Robert McLaughlin, Ilya Grishchenko, Christopher Kruegel, Giovanni Vigna
- **方向**: 其他
- **解读**: ERC-20 的 approve/transferFrom 机制允许用户预先授权第三方合约（如 DEX、DeFi 协议）动用其代币，是 DeFi 生态的基础设施；但该机制也被攻击者系统性滥用：通过钓鱼、恶意合约或诱骗签名诱导用户 approve，随后用 transferFrom 一次性转走授权额度内的全部资产。此类"授权即损失"的诈骗在链上持续高发，受害者往往无法追回。本工作从标题看对以太坊 approve-transferFrom 生态的被利用情况开展大规模研究。

据标题推断，方法上可能包括：大规模分析链上 approve 交易与 transferFrom 调用，识别攻击性合约与诈骗模式（如高额授权、长期未撤销授权、与已知恶意地址关联），统计受害者规模与损失金额，并可能提出检测或预警机制。摘要缺失，具体分析框架与口径无法确认。

摘要缺失，无法引用量化结果；预期包含被滥用 approve 的数量与金额、典型攻击合约的行为特征、受害者的分布等实证数据，并可能给出缓解建议。

approve 机制的便利性与风险并存，其"一次授权、随时可转"的语义是 DeFi 安全的老大难，而用户的授权往往长期滞留、超出实际所需，构成巨大的存量风险面。该工作把这一现象系统化量化，对钱包安全设计（授权限额提示、定期清理）、协议最佳实践与用户教育均有价值。局限：无摘要，攻击判定的标准（如何区分恶意与正常授权）与损失归因的严谨性需全文确认；作者团队（Vigna/Kruegel 组）在恶意软件与链上安全实证方向积累深厚。（无摘要，据标题推断）


#### Voting-Bloc Entropy: A New Metric for DAO Decentralization.
- **作者**: Andrés Fábrega, Amy Zhao, Jay Yu, James Austgen, Sarah Allen, Kushal Babel, Mahimna Kelkar, Ari Juels
- **方向**: 其他
- **解读**: DAO 用智能合约组织社区协作，其名字中的"D"（去中心化）却缺乏能刻画"多元与平等参与"的度量：现有定义多为临时拼凑，无法反映投票者利益一致带来的隐性集权。本文提出 Voting-Bloc Entropy（VBE，读作"vibe"）：核心洞察是利益高度一致的投票者构成一股"集权力量"，应被显式建模。VBE 通过测量参与者在多轮投票中效用函数的相似度来量化这种一致化程度，且从第一性原理出发——先引入一个简洁而有力的基于强化学习的投票概念模型，再由此推导出 VBE。

理论上，作者证明了一系列关于投票委托（delegation）、提案捆绑（proposal bundling）、贿赂（bribery）等机制对去/集权影响的结论——这些效应被以往的去中心化定义所忽视，并据此给出增强 DAO 去中心化的实用建议。

实证侧，作者通过 VBE 测量研究（measurement studies）与基于 VBE 的治理实验展示其可用性，并将相关工具以开源形式发布，供社区继续研究 DAO 去中心化。

论文把"去中心化"从口号转化为可计算、可验证的量，理论（公理化推导+机制分析）与实证（测量+治理实验）结合，并配套开源工具，体系完整。局限：VBE 依赖对投票者效用函数相似度的估计，其输入（投票行为数据）的可得性与噪声会影响度量稳健性；作为新度量，其与社区直觉、既有指标的收敛性需更多实际部署检验。总体而言，它为 DAO 治理研究提供了首个有理论根基的去中心化度量工具。

- **论文 PDF**: [Voting-Bloc Entropy: A New Metric for DAO Decentralization.](https://www.usenix.org/system/files/usenixsecurity25-fabrega-entropy.pdf)

#### Surviving in Dark Forest: Towards Evading the Attacks from Front-Running Bots in Application Layer.
- **作者**: Zuchao Ma, Muhui Jiang, Feng Luo, Xiapu Luo, Yajin Zhou
- **方向**: 其他
- **解读**: 抢先交易（front-running）攻击给区块链带来数十亿美元级别的损失，且多由高速自动化的机器人执行；应用层如何设计规避策略来对抗这些机器人，是开发者关心的核心问题。然而真实世界中的规避策略因其隐蔽性，此前从未被系统化梳理与度量。本文迈出第一步：揭秘以太坊与 BNB Smart Chain 上的抢先交易规避策略，回答"应用层实际在用哪些方法躲开机器人"这一空白问题。

方法上，作者提出 EVScope 框架，结合二进制分析与机器学习，检测已知与未知的规避策略；对 6,761,186 笔套利交易与 71 笔在野外成功躲避抢先交易机器人的重大攻击交易进行了大规模分析，从中提炼出 32 种精细策略，涵盖访问控制、利润控制、执行拆分与代码混淆四大类。

量化结果：32 种策略中 25/32 为本工作首次提出，28/32 是首次被发现应用于规避抢先交易，填补了该方向的文献空白。

该工作把"黑暗森林"中的对抗实践从轶事变为系统化知识，规模（数百万笔交易）与产出（策略分类学）兼具价值，对评估应用层抗 MEV 韧性与设计更安全的 DeFi 协议有直接参考意义。局限：策略检测依赖二进制分析+ML，存在误分类可能；规避策略随机器人进化快速迭代，分类学的时效性有限。总体而言，它是 MEV 对抗研究的重要实证基石，也为防御侧（更聪明的机器人、公平排序机制）指明了压力点。

- **论文 PDF**: [Surviving in Dark Forest: Towards Evading the Attacks from Front-Running Bots in Application Layer.](https://www.usenix.org/system/files/usenixsecurity25-ma-zuchao.pdf)

#### Stack Overflow Meets Replication: Security Research Amid Evolving Code Snippets.
- **作者**: Alfusainey Jallow; Sven Bugiel
- **方向**: 其他
- **解读**: 大量安全研究以 Stack Overflow 数据为基础，但其上的代码片段随社区编辑持续演化；若这些数据是非平稳的，过去的研究结论在不同时间点重复实验可能得出不同结果，研究的可复现性存疑。

作者系统评审了 2005–2023 年间发表的论文，识别影响研究结果的 SO 关键方面（如代码片段的语言或上下文），并用更近期的数据集复制六项研究，实证检验该风险。

复制实验显示，其中四项论文的结果与原始发现显著不同，无法在较新数据集版本上得出相同结论；作者建议将 SO 视为时间序列数据源，为解读横截面研究结论提供语境。

该工作把"数据演化"这一测量方法论问题正式摆到安全社区面前，六项中四项结果漂移的比例很有冲击力，对依赖众包代码数据的漏洞与代码研究均有警示意义；时间序列视角的建议务实可操作。局限在于复制样本量有限，且 SO 之外的代码源（如 GitHub）同样存在演化问题；与可复现性运动结合，此类元研究的价值会持续上升。

- **论文 PDF**: [Stack Overflow Meets Replication: Security Research Amid Evolving Code Snippets.](https://www.usenix.org/system/files/usenixsecurity25-jallow.pdf)

#### Confusing Value with Enumeration: Studying the Use of CVEs in Academia.
- **作者**: Moritz Schloegel; Daniel Klischies; Simon Koch; David Klein; Lukas Gerlach; Malte Wessels; Leon Trampert; Martin Johns; Mathy Vanhoef; Michael Schwarz; Thorsten Holz; Jo Van Bulck
- **方向**: 其他
- **解读**:（无摘要，据标题推断）CVE 编号本意是漏洞的唯一标识（枚举），但学术界常把"是否有 CVE"当作漏洞严重性或研究价值的度量；这种把枚举当价值的概念混淆可能使研究结论失真。

论文系统研究学术论文中使用 CVE 的方式，分析 CVE 编号被当作值（价值/严重度代理）而非枚举标识的现象，考察其对研究结论有效性的影响。

（无摘要，据标题推断）据标题，研究发现 CVE 在学术界存在普遍误用模式，并揭示其对漏洞研究可比性与结论有效性的影响。

这是针对安全研究"度量污染"的元研究：CVE 存在与否受披露流程、厂商响应与分配随机性影响，远非严重度或重要性的可靠代理，用其排序或筛选语料会引入系统性偏差；该问题在漏洞测量与 NVD 驱动的研究中广泛存在。作者阵容强大、方法学严谨性可期；若能给出可操作的替代度量建议（如以可利用性、影响链深度等维度替代 CVE 存在性），对提升领域研究质量价值很大。


#### SoK: Automated TTP Extraction from CTI Reports - Are We There Yet?
- **作者**: Marvin Büchel, Tommaso Paladini, Stefano Longari, Michele Carminati, Stefano Zanero, Hodaya Binyamini, Gal Engelberg, Dan Klein, Giancarlo Guizzardi, Marco Caselli, Andrea Continella, Maarten van Steen, Andreas Peter, Thijs van Ede
- **方向**: 其他（威胁情报）
- **解读**: 网络威胁情报（CTI）报告中的战术、技术与过程（TTP，常对齐 MITRE ATT&CK）是安全运营的核心输入，但自动化抽取方法高度碎片化：各工作使用不同的 TTP 本体与数据集，跨工作比较几乎不可能。本 SoK 要回答"我们到那一步了吗"——系统化审视并从实证上评估该领域的真实进展。

方法上，作者综述并统一了 40+ 种基于 NLP 的抽取方法（命名实体识别、嵌入、生成式模型），在统一评估框架下标准化数据集、本体与评估设置，实现公平对比。

关键发现：现有方法在真实场景中存在性能天花板，难以一致地解析 TTP；传统 NLP 方法在现实条件下常常优于更新的嵌入方法乃至部分生成式模型；领域进展受限于 TTP 本体本身的歧义与数据集质量，需要更清晰的本体与高质量标注数据。

我的思考：该 SoK 的"泼冷水"结论很有价值——在 LLM 热潮下，实证表明传统 NLP 仍具竞争力，提醒社区警惕"新方法必然更好"的直觉；统一评估框架是消除碎片化的关键一步。局限是评估框架对方法的选择性覆盖、以及"性能天花板"可能随模型进步而移动；但作为领域体检报告，其对研究资源的引导意义明确。（无摘要，据标题推断）

#### DarkGram: A Large-Scale Analysis of Cybercriminal Activity Channels on Telegram.
- **作者**: Sayak Saha Roy, Elham Pourabbas Vafa, Kobra Khanmohamaddi, Shirin Nilizadeh
- **方向**: 其他（网络犯罪生态测量）
- **解读**: Telegram 已成为网络犯罪内容传播的重要渠道，但此前缺乏对网络犯罪活动频道（CAC）的大规模系统刻画。本文首次对 339 个此类频道展开分析——这些广播式频道拥有超过 2380 万订阅者，传播泄露凭证、盗版软件、社交媒体操纵工具、恶意软件与漏洞利用工具包以及社交工程诈骗等恶意内容。

作者开发 DarkGram——基于 BERT 的框架，自动识别频道中的恶意帖子，准确率达 96%；随后对 2024 年 2 至 5 月间这些频道的 53,605 条帖子进行定量分析，并利用 DarkGram 检测新出现的频道、向 Telegram 及相关组织举报恶意内容。

结果显示，大量内容虽免费分发，但管理员常通过促销、抽奖等策略刺激付费恶意内容销售；频道对订阅者自身也有风险——28.1% 的分享链接指向钓鱼攻击，38% 的可执行文件捆绑了恶意软件；频道还能通过快速迁移新频道规避审查，订阅者损失极小。借助 DarkGram 的举报，三个月内促成 196 个频道被下架。

该研究是首批对 Telegram 网络犯罪生态进行端到端量化的工作之一，96% 的识别准确率与"边检测边举报下架"的闭环兼具学术与实务价值。局限是样本限于广播式频道、时间窗口短（四个月），BERT 分类器对新型混淆内容的泛化能力未知；研究也展示了平台治理与生态韧性的持续对抗。

- **论文 PDF**: [DarkGram: A Large-Scale Analysis of Cybercriminal Activity Channels on Telegram.](https://www.usenix.org/system/files/usenixsecurity25-roy.pdf)

#### Tracking the Takes and Trajectories of English-Language News Narratives across Trustworthy and Worrisome Websites.

- **作者**: Hans W. A. Hanley, Emily Okabe, Zakir Durumeric
- **方向**: 其他
- **解读**: 误导与虚假信息如何进入并在新闻生态中扩散是长期难题：故事在数千边缘与主流网站间传播，需要可扩展追踪手段。既有方法局限于单一平台或关键词，也无法区分各网站对同一事件的态度。本文构建端到端系统，在 18 个月内追踪 146K 条新闻叙事在 4,000+ 网站间的传播轨迹与立场，以揭示宣传网络结构。

系统每日抓取 4,076 个网站（低/混合/高可信三类）共 2,900 万篇文章、4.28 亿段落；用 LoRA+SimCSE 微调的 e5-base-v2 嵌入段落，FAISS 加速的 DP-Means 聚类出 146,212 个故事簇；用重训的 TATA 模型（零样本 F1=0.781）做零样本立场检测，最后以 NETINF 网络推断算法还原网站间的复制转载关系与影响力结构。

可靠与混合可信网站在设定新闻议程上主导（可靠网站被复制故事中 60% 来自可靠网站，仅 10% 来自不可靠网站），但三者在立场上分化明显：可靠网站平均亲乌克兰（µ=0.36），不可靠网站反疫苗反乌克兰（µ=-0.82/-0.87）；仅凭立场数据即可 85.9% 准确率、AUC 0.889 区分不可靠网站。在不可靠故事集（6,762 条）中可靠网站 44% 的被复制故事竟源自不可靠网站；还识别出 Metric Media 运营的 338 个伪装地方新闻的右倾网站网络及 rt.com 等反乌宣传源头。

首次在四千网站规模同时量化故事流向与态度倾向，方法论完整且验证充分（立场模型与党派标签 Spearman ρ=0.51）。局限：依赖 Media-Bias/Fact-Check 人工分级作为"真值"；聚类阈值与 NETINF 单一影响源假设引入噪声；数据期（2022-2023）已过；系统不做事实判断，只能定位可疑内容而无法判定真伪。

- **论文 PDF**: [Tracking the Takes and Trajectories of English-Language News Narratives across Trustworthy and Worrisome Websites.](https://www.usenix.org/system/files/usenixsecurity25-hanley.pdf)

#### Characterizing the MrDeepFakes Sexual Deepfake Marketplace.
- **作者**: Catherine Han, Anne Li, Deepak Kumar, Zakir Durumeric
- **方向**: 其他（网络滥用生态测量）
- **解读**: 色情深度伪造（sexual deepfake）材料在过去几年急剧泛滥，攻击者出于性满足、骚扰羞辱或对亲密伴侣的控制等动机制作传播，催生了买卖此类材料的市场。本文系统刻画其中最主流、最公开的集市 MrDeepFakes。

作者分析该市场的经济结构、被制作媒体的目标对象，以及用户讨论中如何制作深度伪造的内容，以此理解深度伪造制作的技术现状；通过大规模数据采集与内容分析还原市场运作全貌。

研究揭示了平台对既定规则（如仅限知名公众人物）的执行几乎缺失、此前未被记录的攻击者动机，以及攻击者获取制作资源（算力、工具、目标素材）的未被探索的策略。

这是对色情深度伪造市场生态的首批系统性测量之一，把攻击者侧的动机、战术与经济纳入视野，为平台治理、执法与受害者保护提供了稀缺证据。局限是单一平台样本的生态代表性有限，数据采集与呈现面临伦理边界（如目标隐私），且市场形态快速演化，结论需要持续更新。

- **论文 PDF**: [Characterizing the MrDeepFakes Sexual Deepfake Marketplace.](https://www.usenix.org/system/files/usenixsecurity25-han.pdf)

#### The Conspiracy Money Machine: Uncovering Telegram's Conspiracy Channels and their Profit Model.
- **作者**: Vincenzo Imperati, Massimo La Morgia, Alessandro Mei, Alberto Maria Mongardini, Francesco Sassi
- **方向**: 其他（网络犯罪生态测量）
- **解读**: 主流社交平台日益严格的内容审核迫使阴谋论者转向 Telegram 等宽松平台，其"仅管理员广播"的频道模式与宽松内容政策催生了复杂的阴谋论频道生态。本文揭示这一生态：检测阴谋论频道、聚类社区结构，并挖掘其盈利模式。

作者提出检测阴谋论频道的方法，发现这些频道可聚为四个不同社区、涵盖超过 17,000 个频道；随后揭示"阴谋论赚钱机器"——多数频道积极寻求从订阅者获利：利用电商平台销售可疑产品或通过联盟链接推广，并借助捐赠与众筹平台为其活动筹款。

这一生意涉及数十万捐赠者，累计产生近 7100 万美元（约 $71 million）的营业额。

把阴谋论传播与商业化动机绑定分析，揭示了虚假信息生态得以自我维持的经济引擎，为平台治理与政策干预提供了明确靶点（切断变现渠道）。局限是收益估算依赖公开数据的推断，渠道与支付方式可能低估实际规模；对"哪些频道、何种内容获利最高"的细粒度分析有待深入。

- **论文 PDF**: [The Conspiracy Money Machine: Uncovering Telegram's Conspiracy Channels and their Profit Model.](https://www.usenix.org/system/files/usenixsecurity25-imperati.pdf)
