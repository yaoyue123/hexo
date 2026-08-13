---
title: IEEE S&P 2025 录用论文深度解读（256 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, IEEE S&P, 2025]
---

IEEE S&P（Oakland）2025 于 2025 年 5 月在旧金山举行，共录用 256 篇论文。本文按研究方向分类，对每篇论文给出深度解读（含独立分析思考），有 arXiv 全文的基于精读，其余基于摘要，并附可获取的 PDF 链接。

## 全部 255 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（24 篇）

#### Secure Transfer Learning: Training Clean Model Against Backdoor in Pre-Trained Encoder and Downstream Dataset.
- **作者**: Yechao Zhang, Yuxuan Zhou, Tianyu Li, Minghui Li, Shengshan Hu, Wei Luo, Leo Yu Zhang
- **方向**: AI/ML安全
- **解读**:
基于预训练编码器的迁移学习已成为现代机器学习的核心范式，但预训练与下游微调的结合扩大了攻击面：后门既可能被植入预训练编码器，也可能出现在下游数据集中，而这一双重威胁常被以往研究忽视；此外资源受限用户难以承受端到端重训等通用防御的开销。

作者首先对现有防御策略进行穷尽式分析，发现许多方法遵循基于假设的反应式工作流，难以扩展到未知威胁、新型攻击或不同范式。据此提出主动式思维——先识别数据与模型中的"干净"成分，并设计了 Trusted Core (T-Core) Bootstrapping 框架，强调定位可信数据与神经元以提升安全性。

评估覆盖 5 种投毒攻击、14 个基准数据集、4 种场景与 3 类威胁，实验结果验证了 T-Core 相比基线防御的优越性。

"先识别可信核心、再安全引导训练"的主动防御视角区别于主流反应式防御，且面向资源受限的迁移场景，实用性强。局限在于其有效性依赖可信成分可被准确定位的前提，且摘要未给出量化数字。该工作为迁移学习场景的后门防御提供了新的范式参考。


#### Fuzz-Testing Meets LLM-Based Agents: An Automated and Efficient Framework for Jailbreaking Text-to-Image Generation Models.
- **作者**: Yingkai Dong, Xiangtao Meng, Ning Yu, Zheng Li, Shanqing Guo
- **方向**: AI/ML安全
- **解读**: T2I 生成模型面临越狱攻击风险，而现有攻击要么需白盒访问、要么提示词不自然易被困惑度类防御拦截、要么查询开销巨大。论文提出黑盒下自动生成自然、语义连贯越狱提示词的框架 JailFuzzer。

将模糊测试三要素（种子池、引导变异引擎、oracle 函数）与 LLM 智能体结合：变异智能体以 VLM（LLaVA/ShareGPT4V）为大脑，通过 ICL 记忆反思成功经验并实施语义变异；oracle 智能体以 LLM（Vicuna）模拟安全过滤器与语义评估器对候选排序，减少无效查询；多轮循环攻击流驱动种子池演化，以 CLIPScore≥δ=0.26 判定语义保持。

对 SD1.4/SDXL/SD3/DALL·E 3 与 6 类安全过滤器：多数过滤器一次性绕过率近 100%、平均仅 4.6 次查询；保守的文本分类器过滤器仍达 82.45%（平均 12.6 次查询）；DALL·E 3 绕过率 81.93%；对 UCE/POSI/SafeGen 绕过率 94%/81%/82%，对 PPL 防御 100% 绕过、SmoothLLM 88-94%；一次性/复用绕过率、FID、查询数四项指标全面优于 SneakyPrompt、DACA、Ring-A-Bell。

把模糊测试范式引入生成模型越狱并开源实现，在提示词自然度与查询效率间取得良好平衡，是主要贡献；但依赖开源 VLM/LLM 且需人工设计系统提示，对不同目标模型的通用性仍需验证。其"现有防御均可被自然提示绕过"的结论为 T2I 安全研究提供了新基准。
- **arXiv PDF**: [Fuzz-Testing Meets LLM-Based Agents: An Automated and Efficient Framework for Jailbreaking Text-to-Image Generation Models.](https://arxiv.org/pdf/2408.00523)

#### Harmonycloak: Making Music Unlearnable for Generative AI.

- **作者**: Syed Irfan Ali Meerza, Lichao Sun, Jian Liu

- **方向**: AI/ML安全

- **解读**: 生成式 AI 已扩展至音乐创作，模型可模仿艺术家风格生成新作品，威胁音乐作品的价值稀缺性与版权收益。现有"不可学习样本"防御主要面向图像，音乐领域缺乏保护机制。本文提出 Harmonycloak，首个专门针对器乐场景、防止生成模型未经授权学习音乐结构与风格的防御机制，回应 AIGC 时代版权保护的迫切需求。

方法上，Harmonycloak 采用误差最小化噪声思路：在音乐数据上叠加人耳难以察觉的扰动，使模型在扰动数据上的损失趋近于零，欺骗模型以为无可学内容，破坏其学习音乐结构的能力。这与图像领域的不可学习防御同源，但需适配音乐数据的时序与多轨特性。

作者在 MuseGAN、SymphonyNet、MusicLM 三个 SOTA 模型上开展实验，通过轨内/轨间客观指标与主观用户研究，在白盒与黑盒设置下验证防御的有效性与适用性；不可学习音频示例已公开可听。

我的思考：本文是把不可学习范式从图像迁移到音频/音乐的首批工作，多模型、多指标、主客观结合的综合评估较完整；黑盒下的有效性说明防御具备一定实用韧性。局限明显：误差最小化防御普遍可被对抗训练瓦解，且噪声强度与保护效果的权衡需仔细标定；对"风格模仿"而非"数据复制"的保护深度也未充分回答。相比图像领域文献，本文开辟了新模态场景，为音乐创作者提供了可操作方案，长期有效性仍有待更强攻击检验。


#### My Model is Malware to You: Transforming AI Models into Malware by Abusing TensorFlow APIs.

- **作者**: Ruofan Zhu, Ganhao Chen, Wenbo Shen, Xiaofei Xie, Rui Chang

- **方向**: AI/ML安全

- **解读**: 模型共享在降低成本的同时引入新风险：攻击者可在模型文件中嵌入恶意代码，使受害者在运行时触发难以察觉的攻击。尽管已有针对模型序列化格式（如 pickle）的攻击研究，TensorFlow 这一主流框架的 API 被滥用于构建恶意软件的风险仍未系统考察。本文对 TensorFlow API 安全风险进行系统分析，提出 TensorAbuse 攻击。

方法上，作者揭示 TensorFlow API 隐含的文件访问、网络通信等"隐藏能力"可被组合利用以构造隐蔽攻击。为此开发两项新技术：其一识别可持久存活的 API，保证恶意代码长期驻留；其二利用 LLM 自动分析分类 API 能力，将人工逆向自动化，可规模化发现可利用 API 面。

应用于 TensorFlow v2.15.0，识别出 1,083 项 API 能力，提取 5 个主要存活 API，利用 20 个开发攻击原语，构造数据泄露、IP 暴露、任意代码执行与 shell 访问四类攻击。Hugging Face Hub 与 ModelScan 均无法检测任何载荷，发现已报告 Google 与 ModelScan。

我的思考：本文的成色在于"框架能力滥用"视角的迁移——以往模型投毒聚焦权重与序列化格式，本文证明框架合法 API 本身即可成为恶意软件载体并绕过现有检测器；LLM 辅助 API 分析是值得借鉴的自动化方向。局限：攻击需受害者先加载恶意模型，属供应链投毒链条一环；检测绕过也仅针对摘要提及的工具。与 pickle 恶意模型研究相比，本文展示了更难防御的"合法 API 组合"路径，对模型生态的恶意筛查提出更高要求。


#### Preference Poisoning Attacks on Reward Model Learning.

- **作者**: Junlin Wu, Jiongxiao Wang, Chaowei Xiao, Chenguang Wang, Ning Zhang, Yevgeniy Vorobeychik

- **方向**: AI/ML安全

- **解读**: 从成对比较中学习奖励模型是自主控制、对话智能体、推荐系统等领域的基础组件，其目标是让自动化决策与用户偏好对齐。这类方法依赖收集用户偏好反馈，而反馈往往匿名提供；由于偏好本质主观、没有黄金标准可对照，恶意行为者可以操纵收集到的数据。本文系统研究"偏好投毒"攻击的脆弱性：攻击者只需翻转一小部分成对偏好，即可提升或压制特定目标结果。

方法上，作者提出两类攻击算法：基于梯度的框架及若干"按距离排序"（rank-by-distance）变体。攻击者以最小化目标结果被选中/落选概率为目标，选择最有利的少量偏好对进行翻转，并在推荐系统、文本提示-响应奖励模型学习等领域数据集上评估。

结果显示攻击高度有效：最极端情形下仅投毒 0.3% 的偏好即可达到 100% 的成功率；但最优攻击类型因领域而异。更简单、更具扩展性的方法在部分场景与复杂方法相当甚至更优；针对其他类型投毒的最先进防御在本场景下防护能力有限。

我的思考：本文揭示了奖励模型（RLHF 的核心组件）在数据层面对偏好操纵的深层脆弱性——由于偏好没有客观标准，投毒几乎无法被自动检测，0.3% 投毒率换来 100% 成功是最有冲击力的数字，对大模型对齐管线是直接警示。方法学上，"简单方法不弱于复杂方法"与安全领域普遍经验一致，提高了防御难度。局限：0.3% 是特定数据集上的极端情形。填补了"偏好学习投毒"的专门空白，是 AI 对齐安全的重要基线工作。


#### Query Provenance Analysis: Efficient and Robust Defense Against Query-Based Black-Box Attacks.
- **作者**: Shaofei Li, Ziqi Zhang, Haomin Jia, Yao Guo, Xiangqun Chen, Ding Li
- **方向**: AI/ML安全
- **解读**: 查询型黑盒攻击仅靠查询即可构造对抗样本,威胁MLaaS等系统。现有有状态防御模型(SDM,如BlackLight、PIHA)比较单个查询与历史记录的相似度,却可被OARS等自适应攻击借泄露的决策边界微调扰动而绕过,成功率可达99%以上。

QPA的关键洞察是放弃逐查询比较,改分析查询序列特征(provenance):攻击查询因迭代构造而呈现高度结构化的"扩散-前进"图模式,良性查询则随机分散、难以伪装。系统为新查询与最相似历史节点连边增量构建provenance图,先以PAS统计量加Grubbs检验(α=0.01)粗筛,再用线图+GCN分类器精判,并配图淘汰(内存保留top K)与24小时重置控制资源消耗。

在MNIST、CIFAR10、ImageNet、CelebaHQ四个数据集、6种攻击及其query-blinding/OARS自适应版本上,QPA平均检测覆盖率96%、精度98%、FPR 0.04%;OARS下平均ASR降至4.08%,约为BlackLight(77.63%)与PIHA(87.72%)的1/20;吞吐最高达基线7.67倍,延迟低至其1/11.09。NES-OARS在3,075/2,839次查询后逃过两基线,却在614次查询内被QPA拦截。

QPA把系统安全领域的provenance思想引入对抗防御,以序列结构特征对抗自适应攻击,思路新颖且工程化完整(动态管理使吞吐提升3.81倍)。局限:TTD平均约20.2次查询,高于基线;图分类器需按数据集离线训练;对手若在极少量查询内完成攻击(文中Square/QEBA在CIFAR10上ASR仍有20%/18%)仍有逃逸空间。

- **arXiv PDF**: [Query Provenance Analysis: Efficient and Robust Defense Against Query-Based Black-Box Attacks.](https://arxiv.org/pdf/2405.20641)

#### Architectural Neural Backdoors from First Principles.
- **作者**: Harry Langford, Ilia Shumailov, Yiren Zhao, Robert Mullins, Nicolas Papernot
- **方向**: AI/ML安全
- **解读**: 以往后门攻击修改模型参数,可被重新训练清除;近期工作发现更隐蔽的"架构后门":仅在网络定义中注入常见组件(激活、池化等),使后门在完全重训后依然存在。本文推广Bober-Irizar等的首个设计,构造任意触发模式检测器,并系统分类架构后门。

方法上用无参数的torch.sign与加减法搭出NAND门(4个算子可得1067种精确实现),借助NAND的完备性构造任意布尔触发器检测器,再按"检测-传播-注入"结构装配进网络;因不含学习参数,后门是确定性函数,任何训练都无法消除。作者沿检测方式(算子/常量)、传播路径(分离/共享/交错)、注入目标(定向/非定向)三个维度给出12类后门及实现。

12类后门的攻击成功率均100%、良性精度零损失,运行时开销中位数5.4%、代码改动中位数9行;ResNet18(基线92.96%)加载预训练权重后,完美触发器零损失,微弱检测最多损失2.2%。用户研究显示ML开发者仅在37%案例中识别出可疑组件,33%情况下反而偏好带后门模型,误报率74%;语言模型检测能力优于人类。

本文把架构后门从单一checkerboard PoC推进为任意触发器、可证明存活的攻击家族,证明"仅改架构定义即可永久劫持模型",对模型供应链构成实质警示。局限:用户研究仅11人、无法做统计推断(作者自认);防御仍以人工审查、权重沙箱、溯源为主,缺自动化检测工具;与NAS自动发现后门等工作的对抗关系尚待深入。

- **arXiv PDF**: [Architectural Neural Backdoors from First Principles.](https://arxiv.org/pdf/2402.06957)

#### Practical Poisoning Attacks with Limited Byzantine Clients in Clustered Federated Learning.

- **作者**: Viet Vo, Mengyao Ma, Guangdong Bai, Ryan K. L. Ko, Surya Nepal

- **方向**: AI/ML安全

- **解读**: 非独立同分布（non-IID）客户端数据是联邦学习（FL）实际部署的关键挑战，为此提出的聚类联邦学习（CFL）方案（如 FL+HC、PACFL）把 non-IID 客户端聚类为组内近似 IID 的群组，从而适用为 IID 场景设计的聚合技术。但 CFL 方案的鲁棒性此前鲜有研究，现有拜占庭鲁棒防御在 CFL 场景下被证明不够有效。本文提出新型 CFL 专用投毒攻击，展示其在极少量恶意客户端下的破坏力。

方法上，作者设计两种攻击 Cluster-U-M 与 Cluster-U-D，目标是在不针对特定防御的前提下显著降低良性参与客户端的模型效用（测试准确率）。攻击分两步利用聚类过程中的客户端漂移：前者诱导聚类产生不同训练分布的群组，后者放大每个客户端自身最优解与群组平均聚合之间的差异；攻击保持防御无关，无需对抗知识。

在 PACFL 上从小规模到大规模广泛评估：攻击可造成高达 54% 的最大准确率损失；即使仅 0.1% 客户端被攻陷（极小的实际成本），仍能使约 4% 的客户端受害。对 FLTrust 与 FLAME 等防御的测试显示，它们对抗 Cluster-U-D 时仍分别留下约 38% 与 18-38% 的损失。

我的思考：CFL 被视为 non-IID FL 的标准解法，本文证明其聚类机制本身可被操纵——攻击者不必对抗聚合算法，只需利用聚类过程把恶意客户端与目标受害者分到一组即可系统性破坏模型。0.1% 攻陷率影响约 4% 客户端揭示"极小投入即可造成不成比例损害"的不对称性，且现有鲁棒聚合防御几乎失效，说明为 IID 场景设计的防御不能直接迁移到 CFL。局限：评估主要基于 PACFL，其他 CFL 变体上的迁移性需验证。这是"攻击面随架构演化"的典型例证。


#### Verifiable Boosted Tree Ensembles.
- **作者**: Stefano Calzavara, Lorenzo Cazzaro, Claudio Lucchese, Giulio Ermanno Pibiri
- **方向**: AI/ML安全
- **解读**: 可验证学习主张训练易于高效安全验证的机器学习模型。先前研究表明，一类特定的决策树集成——称为"大间距"（large-spread）树——可以在多项式时间内针对任意基于范数的攻击者进行鲁棒性验证。本文将该研究从基于硬多数投票的基础集成方法扩展到最先进的提升集成（如 XGBoost、LightGBM 训练的模型）。

形式化结果表明：针对 L∞ 范数攻击者，提升树集成的鲁棒性验证可以在多项式时间内完成，但对其他范数攻击者仍是 NP-hard。作者进一步提出一个针对任意 p∈N∪{0} 的 Lp 范数的伪多项式验证算法，在实践中性能出色。

量化结果：L∞ 范数下多项式时间可验证，一般 Lp 范数下为 NP-hard，但伪多项式算法在实际场景中性能良好。

我认为该工作把可验证性从多数投票树扩展到实际应用最广泛的提升树，实用价值显著。L∞ 多项式可验证与一般 Lp NP-hard 的刻画为实践者提供了清晰的操作指引；伪多项式算法是否可用取决于实际维度与精度参数的大小；与鲁棒训练（如对抗训练提升树）的结合是值得期待的后续方向。


#### GRID: Protecting Training Graph from Link Stealing Attacks on GNN Models.
- **作者**: Jiadong Lou, Xu Yuan, Rui Zhang, Xingliang Yuan, Neil Zhenqiang Gong, Nian-Feng Tzeng
- **方向**: AI/ML安全
- **解读**: 图神经网络（GNN）在图结构数据的各类分类任务中表现出色，但面临链接窃取攻击的潜在威胁：攻击者可通过度量两个节点预测向量的相似性，推断节点之间是否存在链接。此类攻击对 GNN 模型所依赖的训练图构成严重的安全与隐私威胁。本文提出名为 GRID（Graph Link Disguise，链接伪装）的防御方案。

GRID 的核心思想是向训练图中添加精心构造的噪声，将相邻节点伪装成 n 跳间接邻居，从而干扰攻击者的相似性推断。同时，作者利用拓扑信息只扰动覆盖所有链接的子集（称为核心节点 core nodes），在提供形式化防御保证的前提下保留模型效用（准确率）。

摘要未给出量化结果，核心贡献是首个在形式化防御保证与模型效用之间取得平衡的链接窃取防御方案。

我认为 GRID 的亮点在于双目标设计：既有形式化防御保证，又通过核心节点选择最小化扰动代价，从而保留模型准确率。需要进一步验证的是防御强度与核心节点比例/噪声幅度之间的权衡曲线，以及对知道防御机制的自适应攻击者的稳健性；与基于差分隐私的图发布方法的关系也值得讨论。


#### TSQP: Safeguarding Real-Time Inference for Quantization Neural Networks on Edge Devices.
- **作者**: Yu Sun, Gaojian Xiong, Jianhua Liu, Zheng Liu, Jian Cui
- **方向**: AI/ML安全
- **解读**: 量化神经网络（QNN）凭借实时能力与低资源占用被广泛部署在资源受限的边缘设备上，但白盒可获取的已部署模型面临模型窃取威胁。TEE 保护的 Secure Inference 被引入作为高效解决方案，然而现有方法忽视了与 8 位量化计算的兼容性，导致推理过程中出现严重的整数溢出问题，可能把 QNN 性能摧毁到随机猜测水平，彻底破坏可用性。

此外，有限的数据表示空间也使模型的机密性与完整性面临重大威胁。本文提出 TSQP（TEE-Shielded QNN Partition），在 TEE 环境中保障 QNN 的准确推理。

摘要未给出量化结果，核心贡献是把 TEE 安全推理从浮点模型扩展到 8 位量化模型，并解决整数溢出导致的灾难性精度退化。

我认为该工作切中边缘部署的现实痛点——实际边缘模型几乎都是量化模型，而既有 TEE 推理方案大多面向浮点。对整数溢出问题的识别精准且有据可依；分区策略（哪些算子放在 TEE 内、哪些留在外部）与 TEE 内存限制、性能开销之间的权衡是核心工程挑战；与同类 TEE+量化推理方案的对比将决定其增量贡献的大小。


#### Fight Fire with Fire: Combating Adversarial Patch Attacks using Pattern-randomized Defensive Patches.
- **作者**: Jianan Feng, Jiachun Li, Changqing Miao, Jianjun Huang, Wei You, Wenchang Shi, Bin Liang
- **方向**: AI/ML安全
- **解读**: 目标检测易受对抗补丁攻击(隐藏/误分类),现有防御多需改造模型、重训或高开销,且难以抵抗知晓防御细节的自适应攻击。本文提出"以火攻火":向输入主动注入特制防御补丁,不改动模型即可检测攻击。

两类补丁:canary(脆弱探针对象)与woodpecker(修复语义的对象)。先从模型输出定位被抑制的候选框,在框附近放置补丁;若canary未被检测到,或woodpecker使原输入未出现的目标重现,则判定为对抗样本。补丁用成对良性与对抗样本联合优化生成(λc=2.0/λw=1.0),并借鉴ASLR思想对补丁内容与位置随机化(n位置×m对象,组合近乎无限)以对抗自适应攻击。

在4种攻击(AdvPatch、TC-EGA、Naturalistic、UPC)、5种检测器、8,760个对抗样本(含1,960个物理世界样本)上,Mode#1/#2/#3的F1为0.948/0.885/0.964,全面超过LGS(0.622)、UDF(0.749)、DetectorGuard(0.458)、ObjectSeeker(0.807)、SAC(0.445);误报仅4.9%/1.4%/5.5%,延迟约0.1秒/图。自适应攻击下,固定部署的Dep#1/#3分别被绕过338/228个(共488),随机化的Dep#2/#4零绕过,攻击者耗费37/18天仍无法生成有效补丁,而UDF被绕过73.2%。

本文把"主动注入探针+随机化"的系统安全范式移植到对抗补丁防御,无需改模型、开销小、对未知攻击与自适应攻击鲁棒,实验规模与基线对比完备。局限:依赖候选框定位,若补丁大到覆盖目标本身(面积比增至3.82倍)可规避,但现实中不可行;canary/woodpecker需按对象类与检测器训练,迁移性未充分验证。整体为物理世界防御提供了实用新思路。

- **arXiv PDF**: [Fight Fire with Fire: Combating Adversarial Patch Attacks using Pattern-randomized Defensive Patches.](https://arxiv.org/pdf/2311.06122)

#### On the Conflict Between Robustness and Learning in Collaborative Machine Learning.
- **作者**: Mathilde Raynal, Carmela Troncoso
- **方向**: AI/ML安全
- **解读**: 协作机器学习（CML）允许参与方在不共享训练数据的前提下联合训练模型，在健康等隐私敏感场景中被视为隐私问题的解决方案，但此类场景中安全性同样是首要关切。为确保在存在潜在不可信参与者的条件下，训练过程仍能产出正确可靠决策的模型，研究者提出使用鲁棒聚合器过滤恶意贡献。

本文证明，文献中两种流行的鲁棒聚合形式无法在不妨碍学习的情况下消除妥协风险：要从协作中学习，就必须始终接受可能遭受有害对抗操纵的可能性。因此，这些方法不适合健康等安全关键的应用场景。

摘要未给出量化结果，核心贡献是一个否定性（不可能性）理论结果，为 CML 的安全性边界划定了形式化界限。

我认为这是一项重要的不可能性结果，价值在于阻止社区在错误方向上继续投入——它系统性地纠正了"鲁棒聚合器可以挽救 CML"的普遍乐观预期，为安全关键场景下的 CML 适用性划出红线。局限在于结论针对的是所分析的聚合器类别，基于可信执行环境、密码学聚合等其他路径不在结论覆盖范围内，仍值得探索。


#### Rigging the Foundation: Manipulating Pre-training for Advanced Membership Inference Attacks.
- **作者**: Zihao Wang, Rui Zhu, Zhikun Zhang, Haixu Tang, XiaoFeng Wang
- **方向**: AI/ML安全
- **解读**: 训练大模型日益依赖迁移学习（预训练+微调），这也开辟了新的攻击面：攻击者若能在预训练阶段做手脚，可使下游针对用户私有数据微调的模型暴露于成员推理攻击（MIA），而这一隐私风险此前从未被研究。

方法上，作者提出"主动鲁棒过拟合"（ARO）技术：在预训练过程中主动诱发鲁棒性过拟合，在不牺牲准确率、保持隐蔽性的前提下，放大下游任务的成员信息泄露。

结果显示，在多种多样的 MIA 场景与数据集上的广泛评估表明，该方法在保持满意的测试性能的同时显著放大成员泄露，并有助于理解这一新型攻击的机理。

我认为该工作把隐私威胁的边界从"训练/微调数据"扩展到"供应链上游的预训练环节"，视角新颖且现实——预训练权重常来自第三方；ARO 以"鲁棒性过拟合"这一表面良性现象为杠杆，隐蔽性强。但摘要缺乏量化指标（如 MIA 准确率提升幅度、对微调数据量的依赖），且威胁模型现实性（攻击者能否控制预训练、微调方如何察觉）需进一步论证；对防御方而言，预训练权重的可信来源审计应成为新的防护方向。


#### Not All Edges are Equally Robust: Evaluating the Robustness of Ranking-Based Federated Learning.
- **作者**: Zirui Gong, Yanjun Zhang, Leo Yu Zhang, Zhaoxi Zhang, Yong Xiang, Shirui Pan
- **方向**: AI/ML安全
- **解读**: 联邦排序学习（FRL）以离散排名代替梯度更新、以服务端多数投票建立全局排名，被视为通信高效且抗投毒的联邦学习范式；但本文质疑其"天然鲁棒"的假设，认为并非所有边都同样健壮。

方法上，作者通过理论分析证明存在特别脆弱的边（层），并给出识别这些层及其脆弱性的下界与上界；据此提出针对 FRL 的新型本地模型攻击——脆弱边操纵（VEM）攻击，用优化方法扰动最脆弱的层以最大化攻击影响。

结果显示，在多个基准数据集上的实验表明，VEM 达到 53.23% 的整体攻击影响，比现有方法有效 3.7 倍，凸显排名式联邦系统的显著脆弱性，作者据此呼吁开发新的鲁棒框架。

我认为该工作的贡献在于用理论界（上下界定位脆弱层）支撑了"并非所有边都同样鲁棒"的反直觉论断，把攻击从全局扰动精细化到单层操纵，方法上有新意。53.23% 与 3.7 倍的量化指标清楚，但攻击影响的确切定义（如对全局排名的破坏程度）与部署约束（本地模型可修改性）需正文说明；多数投票机制被突破说明 FRL 的鲁棒性论证此前过于乐观，防御侧亟需针对"层级投毒"的新机制。


#### Edge Unlearning is Not "on Edge"! an Adaptive Exact Unlearning System on Resource-Constrained Devices.
- **作者**: Xiaoyu Xia, Ziqi Wang, Ruoxi Sun, Bowen Liu, Ibrahim Khalil, Minhui Xue
- **方向**: AI/ML安全
- **解读**: "被遗忘权"要求机器学习模型能够擦除数据所有者的信息；仅删除数据集不够，因为模型会记忆训练数据。近似遗忘的效果不被完全保证，精确遗忘（丢弃并从头重训）又成本高昂，而 IoT、移动与卫星等边缘设备资源受限，难以承载重训。

方法上，作者提出 CAUSE（面向边缘网络的约束感知自适应精确遗忘系统）：在设备上存储子模型以最小化重训开销，用基于斐波那契的替换策略在用户分区过程中自适应调整分片数量，并利用剪枝压缩节省内存，以极小精度损失换取资源节省。

结果显示，相比其他代表性系统，CAUSE 在速度、能耗与精度上分别实现 9.23%–80.86%、66.21%–83.46% 与 5.26%–194.13% 的改善。

我认为 CAUSE 把精确遗忘从数据中心下沉到资源受限设备，斐波那契式自适应分片与剪枝结合在工程上巧妙，"精确"保证了与重训等价的可验证遗忘，避免了近似方法的争议。但摘要中提升幅度跨度大（如精度 5.26%–194.13%），说明收益依赖具体模型与设备配置；分片存储带来的内存累积与长期行为（分片是否会无限增长）需要正文评估；与集中式精确遗忘的语义等价性也需严格论证。


#### Towards Reliable Verification of Unauthorized Data Usage in Personalized Text-to-Image Diffusion Models.
- **作者**: Boheng Li, Yanhao Wei, Yankai Fu, Zhenting Wang, Yiming Li, Jie Zhang, Run Wang, Tianwei Zhang
- **方向**: AI/ML安全
- **解读**: 个性化文生图扩散模型可能基于未授权数据训练，侵犯艺术家版权。现有数据溯源方案在水印/后门触发等"涂层"，但作者实验发现这些涂层在Stable Diffusion等大规模预训练模型上难以被个性化微调学习：训练500步后水印比特准确率仍约50%，DIAGNOSIS后门在改用BLIP生成的训练提示词后检测成功率骤降至4%-15%，验证不可靠。

SIREN的关键洞察是：涂层必须与个性化任务特征相关才会被模型学习。为此定义可学习性损失，优化涂层使其减小扩散模型文本-图像对的损失（即被模型视为任务相关特征）；配合CIEDE2000人类视觉感知约束保证隐蔽性、超球面分类提升对未见负样本的泛化、K-S假设检验给出可控误报的验证结论，并用Reptile元学习加速新数据适配。

实验在5个扩散模型、6个数据集、4种个性化方法上，SIREN在α=10^-9显著性水平下TPR近100%，而水印基线TPR不足40%；跨4种模型×3种描述生成器迁移率达100%，涂层仅占训练集1%时Pokemon上TPR仍达94.2%；在Replicate、Scenario两个真实个性化服务平台TPR=100%；PSNR达38-40、人类偏好研究显示画质影响很小。

我的思考：把涂层"对齐任务特征"的洞察是核心贡献，黑盒、免训练细节假设使方案实用；局限是依赖替代模型与类描述符、强净化攻击在足够大失真下仍可绕过（但会严重破坏画质，人工评分仅1.1-2.2），且基座模型已见过目标数据时效果下降。

- **arXiv PDF**: [Towards Reliable Verification of Unauthorized Data Usage in Personalized Text-to-Image Diffusion Models.](https://arxiv.org/pdf/2410.10437)

#### UnMarker: A Universal Attack on Defensive Image Watermarking.
- **作者**: Andre Kassis, Urs Hengartner
- **方向**: AI/ML安全
- **解读**: 防御性水印让GenAI提供商在生成图像中嵌入指纹用于深伪检测，但现有攻击要么需要检测器反馈、要么需要白盒知识或类似模型、要么依赖未必可用的高级去噪管线，实用条件苛刻。UnMarker提出首个实用、通用（黑盒、免查询、免数据）的防御性水印攻击。

其根基是"通用载体"分析：计算受限的检测器只能检查有限属性，而鲁棒水印必须把水印构造在频谱幅度上——非语义水印集中在高频（利用边缘掩蔽），语义水印集中在低频（改变纹理结构）。据此设计两种对抗优化：高频破坏用Direct Fourier Loss配合LPIPS感知约束直接优化像素；低频破坏学习逐像素可优化滤波器权重（FRL+MPL损失），配合10%中心裁剪即可系统性擦除水印。

实验击败全部7个SOTA方案（Yu1/Yu2/HiDDeN/PTW/StableSignature检测率降至0-31%），并首次实用攻破语义水印StegaStamp（降至43%）与TRW（降至40%），使其失去"安全优势"；输出LPIPS仅0.05-0.15，画质优于VAEAttack/DiffusionAttack，后两者对语义水印完全失效。

我的思考："空间鲁棒性与频谱脆弱性不可兼得"的权衡论证是亮点，结论激进但证据充分，直接质疑防御性水印作为深伪对策的可行性；局限是攻击依赖裁剪等基本操作且未覆盖全部生成场景，但其"频谱幅度"论断也为未来水印设计提供了反面方向。

- **arXiv PDF**: [UnMarker: A Universal Attack on Defensive Image Watermarking.](https://arxiv.org/pdf/2405.08363)

#### SoK: Watermarking for AI-Generated Content.
- **作者**: Xuandong Zhao, Sam Gunn, Miranda Christ, Jaiden Fairoze, Andrés Fábrega, Nicholas Carlini, Sanjam Garg, Sanghyun Hong, Milad Nasr, Florian Tramèr, Somesh Jha, Lei Li, Yu-Xiang Wang, Dawn Song
- **方向**: AI/ML安全
- **解读**: 生成式AI输出越来越难以与人类内容区分，事后检测（零样本统计或训练式分类器）误差率难低于10^-3且无理论保证。本文系统梳理水印这一主动方案：从历史（13世纪纸张水印）与监管视角（美国EO 14110、加州SB 942（2026年生效）、欧盟AI法案第50条、中国深度合成规定等）说明必要性，并梳理C2PA、SynthID等产业实践。

论文形式化零比特/多比特水印语法与期望性质：质量保证从经验验证、低失真、无失真到最强的计算不可检测性逐级递进；给出假阳性率、假阴性率与鲁棒性（须针对具体信道与内容熵定义）、不可伪造性（与鲁棒性本质冲突，须将检测与归因分离）、消息嵌入容量与计算效率的统一定义；并分析移除与伪造两类攻击目标、评估策略与开放问题。

基于对100余篇论文的综述，论文指出文本熵低、实际水印通常只能嵌入少量比特，而图像/音频/视频熵高更易嵌入消息；强调"水印不是银弹"，但可增强AI内容可信度。

我的思考：作为跨文本/图像/音频/视频的首个系统化梳理，形式化定义与威胁模型分层为研究者与政策制定者提供了共同语言；检测与归因分离、鲁棒性须按信道定义的澄清尤其有价值；全文框架完整，与UnMarker等攻击性研究形成攻防呼应的整体图景。

- **arXiv PDF**: [SoK: Watermarking for AI-Generated Content.](https://arxiv.org/pdf/2411.18479)

#### Supporting Human Raters with the Detection of Harmful Content Using Large Language Models.
- **作者**: Kurt Thomas, Patrick Gage Kelley, David Tao, Sarah Meiklejohn, Owen Vallis, Shunwen Tan, Blaz Bratanic, Felipe Tiengo Ferreira, Vijay Kumar Eranti, Elie Bursztein
- **方向**: AI/ML安全
- **解读**: 有害内容审核（仇恨言论、骚扰、暴力极端主义、选举虚假信息）依赖稀缺的人工审核员，面临规模、政策快速变化、区域知识与心理负担等瓶颈。本文用Google人工标注的5万条英文评论（4类违规各1万+1万非违规）系统探索LLM辅助审核的可行性，聚焦"协助人类"而非"替代人类"。

方法上做提示工程消融（零样本/少样本、手工/动态检索示例、关键词链式思考），提出五种人机协作模式：预过滤非违规内容、快速升级违规内容、自主评级、人工校验（分歧复核）与人工辅助（高亮关键上下文）；并用token概率作滑动决策阈值，使单个提示即可适配所有模式。

结果：text-unicorn+动态少样本最优，选举虚假信息98.7%、仇恨言论91.1%、暴力极端主义91.1%、骚扰90.1%准确率；单提示覆盖40条款会使上下文窗口末尾政策精度降13.7%；预过滤在99%召回下可移除54.5%-98.2%非违规内容；真实队列试点在95.1%召回下减少41.5%待审量，关键词辅助使人工审核精度+11%、召回+9%（绝对值）。独立复核还发现0-2%违规标注本身有误，LLM在部分分歧案例中比人工更准。

我的思考：生态效度极高，五种设计模式为LLM审核落地提供了清晰架构；提示工程细节（动态示例、阈值调优）有直接实用价值；风险在于自动化偏见与对LLM的过度信任，需谨慎配置阈值与复核流程，且精度上界受人工金标准质量限制。

- **arXiv PDF**: [Supporting Human Raters with the Detection of Harmful Content Using Large Language Models.](https://arxiv.org/pdf/2406.12800)

#### CipherSteal: Stealing Input Data from TEE-Shielded Neural Networks with Ciphertext Side Channels.
- **作者**: Yuanyuan Yuan, Zhibo Liu, Sen Deng, Yanzuo Chen, Shuai Wang, Yinqian Zhang, Zhendong Su
- **方向**: AI/ML安全
- **解读**: 
使用TEE在不可信主机上保护神经网络推理正被广泛采用；但本文指出，新披露的TEE密文侧信道会向恶意主机泄露TEE内神经网络的内存写模式，从而危害模型与用户数据的机密性。既有工作利用该信道恢复密钥比特，但其技术不适用于结构更复杂、仅泄露部分信息的神经网络输入。

本文提出自动化输入恢复框架CipherSteal，首次证明了对TEE保护下NN输入的严重威胁：将问题重构为"变换重建"两步方法，并提出优化以充分利用泄露信道；研究覆盖TensorFlow与PyTorch两大框架、TVM与Glow编译器生成的多种可执行文件，全面刻画攻击面。

评估在多种模型（含Transformer）与图像/视频输入上进行：在不同水平的攻击者先验知识下，均成功恢复出视觉上与原始输入相同的样本；此外，攻击者还能进一步窃取模型功能（训练替代模型），并利用恢复的输入生成"白盒"对抗样本，有效操纵模型预测。

该工作把密文侧信道威胁从密钥扩展到神经网络输入乃至模型本身，严重性显著升级；与提取密钥的工作相比，处理"部分信息+复杂语义"的输入恢复更具挑战性。对TEE+NN的隐私推理部署（如机密ML服务）提出了明确警示，需要配套防御措施。

#### EvilHarmony: Stealthy Adversarial Attacks Against Black-Box Speech Recognition Systems.

- **作者**: Xuejing Yuan, Jiangshan Zhang, Feng Guo, Kai Chen, Xiaofeng Wang, Shengzhi Zhang, Yuxuan Chen, Dun Liu, Pan Li, Zihao Wang, Runnan Zhu

- **方向**: AI/ML安全

- **解读**: 自动语音识别（ASR）系统易受对抗样本攻击：精心设计的小扰动即可让音频被误识别为目标命令。现有攻击通常以零向量或目标命令的 TTS 片段初始化——前者在扰动音频中累积目标命令特征，后者持续削弱命令特征，导致生成的对抗样本虽人耳难辨，却常呈现明显失真与不自然，暴露出"音频被篡改"的痕迹。

方法上，本文旨在只保留生成对抗样本所必需的特征，最小化多余要素，提升攻击音频的自然度、降低可检测性。研究揭示共振峰（formant）等语音特征对黑盒攻击的关键作用，据此设计更精炼的扰动生成策略。

结果：摘要截断，未给出量化数据；贡献集中于攻击质量（自然度、隐蔽性）与黑盒场景下的成功率提升（具体指标见论文全文）。

我的思考：对抗语音研究早期聚焦"成功率"，近年转向"隐蔽性"——因为语音助手的异常检测与人工审计会拦截过于失真的样本。EvilHarmony 抓住共振峰等语言学特征做减法，方向与"最小扰动"理论一致，但难点在于黑盒下无法访问梯度，如何高效定位关键特征需巧妙设计。与白盒方法及基于遗传算法的黑盒攻击相比，本文的语音学先验是其差异化优势；局限在于对抗样本对真实信道（压缩、噪声）的鲁棒性仍是语音攻击的通病。


#### Investigating Physical Latency Attacks Against Camera-Based Perception.

- **作者**: Raymond Muller, Ruoyu Song, Chenyi Wang, Yuxia Zhan, Jean-Philippe Monteuuis, Yanmao Man, Ming Li, Ryan M. Gerdes, Jonathan Petit, Z. Berkay Celik

- **方向**: AI/ML安全

- **解读**: 基于摄像头的感知是自动驾驶系统视觉的核心。近期研究考察了针对感知管线的延迟攻击，可导致系统拒绝服务（DoS）。但此类攻击缺乏真实世界可行性：要么依赖数字扰动，要么需要覆盖受害者视野的大尺寸、不可扩展且高度可见的补丁。

方法上，本文提出 Detstorm，一种物理可实现的延迟攻击：使用投影仪在环境中创建大量对抗性物体，通过拥挤检测区域造成感知管线延迟。这些物体针对四个目标优化，以规避多种非极大值抑制（NMS）过滤方案；为在动态物理环境中最大化效果，采用独特的贪心方法，对场景进行分段处理并逐步布置对抗物体。

结果：摘要未给出量化数字；研究以物理实验验证攻击的可行性与延迟放大效果（延迟增量、成功率等见论文全文）。

我的思考：物理对抗攻击研究的难点在"真实可部署性"，Detstorm 用投影替代实体补丁，兼具隐蔽性与可扩展性，且直击感知管线的时延脆弱性——不是让系统"看错"，而是让系统"来不及看"，威胁模型更贴合自动驾驶的实时约束。与以往聚焦误分类的攻击相比，延迟攻击更难防御（常规检测器难以识别"拥挤"）。局限在于投影在强光、雨天等环境下的有效性存疑，且贪心场景分割的优化质量需大规模实验支撑；总体是物理层 AI 安全的有力补充。

#### From One Stolen Utterance: Assessing the Risks of Voice Cloning in the AIGC Era.
- **作者**: Kun Wang, Meng Chen, Li Lu, Jingwen Feng, Qianniu Chen, Zhongjie Ba, Kui Ren, Chun Chen
- **方向**: AI/ML安全
- **解读**: 语音克隆从根本上威胁"语音作为唯一生物特征"的地位，已有多起利用其进行身份伪造的犯罪事件被报道。此前研究条件受限：受害者要么需出现在模型训练数据中，要么需向对手泄露数十分钟语音样本；而在 AIGC 时代这些约束大幅放松，SOTA 技术的真实风险被"浓雾"笼罩。

本文开展大规模真实场景研究：覆盖 5 个 SOTA 语音克隆工具（开源与商业）、8 个真实世界认证系统、30 名人类听众，使用 7000 余条语音样本（公开与自采），结合实验与理论分析揭示威胁全貌。

主要发现包括：1) 语音克隆对欺骗听众构成严重威胁；2) 年龄、性别等人口因素对攻击效果有微妙影响；3) 听众的主观意见与背景知识对易感性起重要作用；4) 现有检测方法仍无法按预期识别克隆语音。

我认为该工作的标题即点明核心结论——攻击门槛已降至"一句被窃取的语音"，将威胁评估从实验室推向了真实认证系统，其规模在语音克隆研究中罕见，对语音银行、语音助手等行业是及时警示。局限在于人口因素与主观性的影响偏定性描述，"检测方法为何失败"未在摘要中展开，且威胁水平高度依赖所选工具与系统的具体配置，泛化结论需谨慎外推。


### 大模型安全（15 篇）

#### On the (In)Security of LLM App Stores.
- **作者**: Xinyi Hou, Yanjie Zhao, Haoyu Wang
- **方向**: 大模型安全
- **解读**:
LLM 应用商店快速增长，大量自定义应用涌现，其安全风险随之上升：应用可能具有滥用潜力、恶意意图甚至内嵌后门，而商店审查机制的有效性未知。

作者提出三层关注框架识别风险应用（滥用潜力、恶意意图、后门），并在 5 个月内从六大商店（GPT Store、FlowGPT、Poe、Coze、Cici、Character.AI）收集了 786,036 个应用。研究整合静态与动态分析，结合自精炼的 LLM 毒性检测器与规则匹配互补检测有害内容，并构建了含 31,783+ 词条的大规模词典 ToxicDict。

结果显示：15,414 个应用存在误导性描述；1,366 个违反隐私政策收集敏感个人信息；15,996 个生成了仇恨言论、自残、极端主义等内容；评估发现 616 个可助恶意软件生成、钓鱼等活动。相关平台（包括 OpenAI、Quora）认可并调查了举报的应用，截至投稿已有 1,643 个被移除。

该工作以大规模实证揭示了 LLM 应用商店生态的失守程度，框架与 ToxicDict 可复用于持续监测。局限在于自动检测存在误报可能、商店政策差异影响结论外推。这是 LLM 应用供应链安全方向的基础性测量。


#### GPTracker: A Large-Scale Measurement of Misused GPTs.
- **作者**: Xinyue Shen, Yun Shen, Michael Backes, Yang Zhang
- **方向**: 大模型安全
- **解读**:
以 OpenAI GPT 为代表的 LLM 智能体重塑了 AI 的定制与使用方式，但 OpenAI GPT Store 中被滥用的 GPT（misused GPTs）成为关键而鲜少研究的问题。

作者提出 GPTRACKER 框架，持续从官方商店收集 GPT 并自动与之交互：8 个月内收集了 755,297 个 GPT 与 28,464 条对话流；结合 LLM 驱动的评分系统与人工审查，识别出 2,051 个跨 10 种禁用场景的滥用 GPT。通过静态与动态分析，论文刻画了这些 GPT 的趋势、构建者与运作机制。

研究发现构建者采用多种绕过审查的战术：集成外部 API、在描述中隐藏意图、URL 重定向等；启用外部 API 的 GPT 更可能对不当查询给出回答（Illegal Activity 场景下平均回答率提升 22.81%）。借助 VirusTotal，50 个恶意域名关联到 446 个 GPT（33 个标记为钓鱼、28 个恶意软件、2 个垃圾邮件）。作者于 2024 年 9 月 11 日与 11 月 12 日负责任披露，1,804 个报告中 1,316 个已被移除。

作为首个滥用 GPT 的大规模测量研究，其数据规模与披露闭环值得肯定。局限在于交互与检测覆盖面受商店 API 约束，结论随时间推移需更新。该工作为 LLM 应用市场监管提供了量化基线。


#### Modifier Unlocked: Jailbreaking Text-to-Image Models Through Prompts.
- **作者**: Shuofeng Liu, Mengyao Ma, Minhui Xue, Guangdong Bai
- **方向**: 大模型安全
- **解读**:
文生图模型的图像生成能力使其成为"双刃剑"：用户可凭简单提示词生成精美图像，攻击者也可借此生成不宜工作场合（NSFW）的内容，即越狱攻击；内置安全过滤器虽有缓解作用，但存在可利用的漏洞。

作者提出 MODX，首个基于修饰词（modifier）的文生图模型越狱攻击框架：利用启发式算法与两类函数（约束）识别可调整艺术风格的修饰词，微妙地引入不安全元素，将生成推向 NSFW 内容。该思路利用了模型在特定风格或形式下不太可能拒绝生成的特点。

作者通过理论分析与实验证据展示 MODX 的有效性：在四个 SOTA 文生图模型上成功越狱并超越现有方法；进一步在更多模型版本与额外类别上评估，显示其强可扩展性与泛化能力。

"通过风格修饰词迂回绕过过滤器"的视角新颖，理论分析+多模型实验的组合增强了说服力。局限在于 NSFW 判定存在主观性，且未给出防御建议的具体量化。该工作提醒安全过滤器的设计需考虑风格维度，而非仅基于内容语义。


#### On the Effectiveness of Prompt Stealing Attacks on In-the-Wild Prompts.

- **作者**: Yicong Tan, Xinyue Shen, Yun Shen, Michael Backes, Yang Zhang

- **方向**: 大模型安全

- **解读**: 高质量提示词已成为提示词市场上被交易的商品，攻击者可尝试从模型输出反推用户精心构造的提示词，威胁其知识产权与商业模式。此前研究多基于学术数据集评估提示窃取攻击，而真实用户提示词在长度、语义与主题上差异巨大，攻击在实战中是否真正构成威胁悬而未决。本文首次对"野外"真实提示词开展系统性研究，填补了这一评估空白。

方法上，作者先对比野外提示词与学术数据集的差异，揭示现有攻击在真实场景下表现不佳；随后提出基于文本梯度（Text Gradient）的迭代精化方法，用梯度信号逐步优化候选提示词以更好复现目标输出，把窃取从"一次性猜测"升级为可计算的优化搜索。

评估显示改进显著：METEOR 分数从 0.207 提升至 0.253，提示词恢复率从 0.323 提升至 0.440。但即便优化后恢复率仍不足一半，论文明确指出真实场景下的窃取远未解决，凸显了进一步评估实践有效性的必要。

我的思考：本文把提示窃取研究从理想化设定拉回真实世界，并证明优化式方法能实质提升攻击效果；可贵之处在于不夸大威胁——恢复率仍有限，提示词的信息冗余使完全窃取依然困难。局限是文本梯度方法的成本与对黑盒 API 的可迁移性存疑。与学术设定下的同类工作相比，本文为提示词保护提供了更准确的威胁模型参照，是提示词经济学与安全交叉领域的实证基石。


#### Fun-tuning: Characterizing the Vulnerability of Proprietary LLMs to Optimization-Based Prompt Injection Attacks via the Fine-Tuning Interface.
- **作者**: Andrey Labunets, Nishit V. Pandya, Ashish Hooda, Xiaohan Fu, Earlence Fernandes
- **方向**: 大模型安全
- **解读**: 闭源 LLM 不暴露梯度，基于优化的提示注入攻击难以计算。论文发现厂商微调接口返回的 loss 类信息可近似 logprobs，从而把灰盒离散优化攻击引入 Google Gemini 闭源模型族。

用小学习率（10^-13~10^-45，实验验证不显著改变模型）跑单轮微调，将训练 loss 作为代理目标；反推出 Gemini loss 与平均 logprobs 近似线性（TrainingLoss=K(X)+l·AvgLogprobs，长输出下 R² 趋近 1）；针对接口以固定种子置换 loss 的机制，用"逐步破坏目标串"构造数据集近似反置换（仅错 7-8% 位置、保留 >90% 序关系）；以贪婪坐标搜索优化 20 token 前缀/后缀包装 PurpleLlama 注入指令。

PPL40（40 个间接注入样本）上 Gemini 1.0 Pro 攻击成功率 82.0%、Gemini 1.5 Flash 65.3%（基线 42.5%/27.5%，随机替换消融 61.3%/43.8%）；每次攻击仅 90 次微调调用、总成本 <10 美元、单例耗时 15（Pro）/60（Flash）小时；候选集 1000（词表约 1%）即可；跨模型迁移良好（1.5 Flash 的攻击在 2.0 Flash 上 >80%）；口令钓鱼场景部分失败。

"微调接口＝实用与安全的根本张力"论证清晰，loss 代理与反置换两处工程细节扎实，且已负责任披露（Google 2025 年 4 月以限制学习率下限、最小批量 4 缓解）。攻击耗时长但可并行扩展，其揭示的"训练指标泄露"模式对其它厂商的微调接口同样适用，是闭源模型安全评估的重要新向量。
- **arXiv PDF**: [Fun-tuning: Characterizing the Vulnerability of Proprietary LLMs to Optimization-Based Prompt Injection Attacks via the Fine-Tuning Interface.](https://arxiv.org/pdf/2501.09798)

#### Codebreaker: Dynamic Extraction Attacks on Code Language Models.

- **作者**: Changzhou Han, Zehang Deng, Wanlun Ma, Xiaogang Zhu, Minhui Xue, Tianqing Zhu, Sheng Wen, Yang Xiang

- **方向**: 大模型安全

- **解读**: 基于 LLM 的代码助手迅速普及，针对其私有训练数据的提取攻击随之加剧——攻击者试图从代码大模型中抽取个人信息（PI）。现有方法多为手动或半自动，提取量有限，严重低估真实泄露程度。本文提出 Codebreaker，一个针对代码助手的自动化 PI 提取框架，系统放大并量化该风险。

方法上，框架含两个核心组件：(i) 语义熵——评估提示触发模型输出目标数据的可能性，为提示选择提供理论度量；(ii) 动态变异机制——在迭代中强化单条响应内不同元素间的互连，提升推理多样性、记忆唤起与提取性能，两者协同实现有导向的自动搜索。

作者在六个开源 CodeLLM 系列（CodeParrot、StarCoder2、Code Llama、CodeGemma、DeepSeek-Coder、DeepSeek-V3）与商业模型（CodeFuse、GPT）上验证：Codebreaker 全面超越 SOTA，提升 6.22%–44.9%（平均 21.79%）；当响应源自同一 GitHub 仓库时，利用多元素互连的优势达 3.88%–32.37%（平均 15.31%）。

我的思考：本文把提取攻击从人工工程推向自动化框架，语义熵为提示选择提供可计算依据，动态变异机制抓住了代码数据"同仓库多文件互相关联"的结构特征，是代码场景区别于文本场景的关键洞察。局限：提升幅度对模型与数据分布敏感，防御仅停留在呼吁层面。与文本 LLM 的提取研究相比，本文对代码模型的专项化填补了空白，警示代码助手供应商需从训练数据脱敏与去重层面加固。


#### Make a Feint to the East While Attacking in the West: Blinding LLM-Based Code Auditors with Flashboom Attacks.

- **作者**: Xiao Li, Yue Li, Hao Wu, Yue Zhang, Kaidi Xu, Xiuzhen Cheng, Sheng Zhong, Fengyuan Xu

- **方向**: 大模型安全

- **解读**: 基于 LLM 的漏洞审计器正在成为自动化代码安全分析的重要工具，其注意力机制本应聚焦潜在漏洞段。本文提出"声东击西"式攻击：用高注意力片段把审计器的注意力从真实漏洞引开致其漏检，对"LLM 审计器可信性"提出系统质疑。

方法上，作者将精心构造、能吸引模型注意力的代码片段注入待审计代码库，使模型注意力偏离真实漏洞。为规模化发动攻击，设计了 Crazy-Ivan 系统：自动识别可无缝融入代码库的高注意力片段，并采用函数级优先级排序与精化策略优化致盲效果，产出 Flashboom 攻击载荷。

评估显示攻击成功率在 CodeLlama 上高达 96.3%、Gemma 上达 83.05%，且具有跨模型迁移性与跨语言适用性。Copilot 案例研究中，攻击使其忽略一个关键区块链漏洞——直接证明注意力转移攻击对真实审计流程的威胁；相关发现已报告厂商并获确认修复中。

我的思考：本文把"注意力操纵"这一 LLM 对抗范式首次系统应用于代码审计，Crazy-Ivan 的自动化管线赋予其实用攻击力，高成功率与跨模型迁移性说明这是通病而非个例。局限：攻击需先向代码库注入片段（供应链场景成立），对真实流程的破坏程度需更大规模验证。与导致审计器误报的注入研究相比，本文瞄准更隐蔽的"漏报"方向，警示依赖 LLM 审计的 CI/CD 流程需要人工复核兜底。


#### BAIT: Large Language Model Backdoor Scanning by Inverting Attack Target.

- **作者**: Guangyu Shen, Siyuan Cheng, Zhuo Zhang, Guanhong Tao, Kaiyuan Zhang, Hanxi Guo, Lu Yan, Xiaolong Jin, Shengwei An, Shiqing Ma, Xiangyu Zhang

- **方向**: 大模型安全

- **解读**: 大语言模型（LLM）易受后门攻击：攻击者在训练提示中注入秘密触发 token 序列，并迫使模型在响应中包含特定目标序列。与输出空间有限的判别式 NLP 模型不同，生成式 LLM 的输出空间随响应长度指数增长，使基于触发反转的现有检测技术面临巨大困难。本文从理论分析 LLM 的学习过程出发，提出新的后门扫描范式 BAIT。

方法上，作者首先在假设下理论分析 LLM 的学习机制，揭示自回归范式下因果语言模型内在地在 token 与目标之间引入强关联。基于此，BAIT 反其道而行：不再反转后门触发器，而是确定模型是否针对某目标被植入后门，利用 token 间异常强的关联显著缩小搜索空间；其搜索式特性使其无需触发器或目标的先验知识，仅需黑盒访问即可扫描。

作者在 153 个模型、8 种架构、6 种不同攻击类型上评估，BAIT 优于 5 个基线方法；其卓越性能使其在 TrojAI 竞赛（多年多轮竞赛）排行榜上位列榜首。

我的思考：BAIT 的核心创新是"反转目标而非反转触发器"——针对生成模型输出空间爆炸的痛点，把检测问题从"搜索不可知的触发器"（难）转为"检验可疑目标是否存在异常关联"（易），是问题重述的典范，理论分析（自回归模型 token 间强关联）为这一转换提供了依据；153 个模型、6 类攻击的大规模评估与 TrojAI 榜首成绩提供了扎实实证，黑盒可用性更提高了实际价值。局限：检测能力可能依赖目标序列的可枚举性，对长目标或非固定目标的攻击效果需进一步考察。表明针对生成式模型的专用检测范式正在成型。


#### Prompt Inversion Attack Against Collaborative Inference of Large Language Models.
- **作者**: Wenjie Qu, Yuguang Zhou, Yongji Wu, Tingsong Xiao, Binhang Yuan, Yiming Li, Jiaheng Zhang
- **方向**: 大模型安全
- **解读**: 协同推理让用户各自托管LLM的部分层并传递中间激活,以低成本部署大模型,但恶意参与者可能从收到的激活中恢复输入提示,泄露敏感信息。因LLM深层强非线性产生大量无意义局部极小,已有嵌入反演攻击几乎失效(反演Llama-7B激活时token准确率近0),本文首次提出提示反演攻击(PIA)验证该隐私威胁。

白盒PIA分两阶段:约束优化阶段在激活匹配损失上加入由嵌入矩阵导出的正则项(拉格朗日乘子加逐维裁剪),迫使优化嵌入贴近合法词嵌入,避开无意义局部极小;自适应离散化阶段用激活校准(依掩码注意力性质贪心选取使激活距离最小的token,理论上可完美恢复,复杂度经候选集缩减)结合语义推测(oracle LLM的top-Y候选)枚举。灰盒场景下与LoRA参数交替优化。

在Skytrax上,Llama-65B、4参与者、末位攻击者(反演60层)时token准确率88.4%,最佳基线(Li et al.)仅22.8%,BLEU 0.8428、NERR 0.6605;各参与者位置设置下均高于87%。灰盒下Guanaco-65B达85.4%。防御实验:σ=1高斯噪声下仍有79.5%准确率,8-bit量化几乎无效,4-bit量化虽降准确率但文本质量严重受损(困惑度28.3)。

本文填补了协同推理LLM隐私风险研究空白,理论(梯度消失定理、贪心离散化最优性)与实验俱佳,88.4%的恢复率说明该场景隐私威胁真实紧迫。局限:攻击需白盒/灰盒条件(知晓前置层权重),只分析单恶意参与者场景;现有防御(噪声、量化、同态加密)均难兼顾效用与隐私,实用防护落地尚远。

- **arXiv PDF**: [Prompt Inversion Attack Against Collaborative Inference of Large Language Models.](https://arxiv.org/pdf/2503.09022)

#### PEFTGuard: Detecting Backdoor Attacks Against Parameter-Efficient Fine-Tuning.
- **作者**: Zhen Sun, Tianshuo Cong, Yule Liu, Chenhao Lin, Xinlei He, Rongmao Chen, Xingshuo Han, Xinyi Huang
- **方向**: 大模型安全
- **解读**: LoRA等PEFT适配器体积小、可即插即用,在开源平台大量共享,也成为后门注入的廉价载体(消费级GPU即可训练)。现有NLP后门检测多面向BERT式分类任务,对生成任务失效。本文构建PADBench基准并提出首个针对PEFT适配器的检测框架PEFTGuard。

PADBench含13,300个良性/带后门适配器,覆盖5个数据集(IMDB、AG News、SQuAD、两个toxic指令集)、4种文本后门攻击(InsertSent、RIPPLES、Syntactic、StyleBkd)、5种PEFT方法(LoRA、QLoRA、DoRA、LoRA+、AdaLoRA)与7种基础模型。PEFTGuard把适配器自注意力权重(Δq、Δv)通道级拼接后跨层连接成张量,送入"卷积+MLP"元分类器,无需输入数据或合并权重即可判别。

在生成任务上检测精度100%、AUC 1.0,分类任务99%/1.0,全面优于T-Miner(50%)、AttenTD(50%)、PICCOLO(76%)、MNTD(88%,IF任务仅51%)。零样本迁移:跨PEFT方法(除AdaLoRA外≥99%)、跨rank(≤256时≥98.67%)有效,跨攻击需对比学习+模型融合(未知攻击达90-95%)。5种自适应攻击下高斯噪声完全无效,强攻击虽可击穿检测但使模型CA/ASR大幅退化;Fine-mixing可将ASR从100%降至7.2%且保持CA 96.12%。

首个PEFT适配器后门检测框架与公开基准,工程价值高,"不合并、不推理、零样本"的设计贴合HuggingFace等平台审查场景。局限:元分类器依赖训练见过的攻击模式,跨攻击零样本能力有限;对AdaLoRA迁移失效提示动态rank会产生不同注入模式;强自适应攻击者可以性能为代价绕过检测。该工作与适配器投毒攻击构成攻防循环,通用特征仍需探索。

- **arXiv PDF**: [PEFTGuard: Detecting Backdoor Attacks Against Parameter-Efficient Fine-Tuning.](https://arxiv.org/pdf/2411.17453)

#### Alleviating the Fear of Losing Alignment in LLM Fine-tuning.
- **作者**: Kang Yang, Guanhong Tao, Xun Chen, Jun Xu
- **方向**: 大模型安全
- **解读**: 对齐是LLM安全的关键,但微调(即使使用干净数据)会意外破坏对齐:Mistral/Qwen微调后有害率从11.7%/2.4%升至21.3%/4.7%,注入有害样本后普遍超50%;即便用GPT-4o等审核过滤,残留392/1500条有害样本仍足以破坏对齐。本文提出微调后恢复对齐的方法。

洞察:对齐LLM内部存在"对齐方向"与"有害方向",把有害问题的特征推向对齐方向、拉离有害方向,可使有害率从4.57%升至80.42%。方法据此恢复被微调破坏的有害方向:用256条有害提示(14类,BeaverTails)在方向层(隐藏层2/3处)提取方向,以负余弦相似度为损失,用FGSM符号判定并按梯度幅值取top 0.2%权重,将微调模型的部分权重重置为对齐模型值,迭代20轮;并设rollback(回滚20%已恢复权重)与5%性能下降熔断,避免损害下游任务。

在125个微调模型(5模型×5数据集×5污染度,QLoRA)上,有害率从平均33.25%降至1.74%,任务性能平均仅下降2.93%(30/125例下降小于1%);恢复后模型与原模型有害方向余弦相似度0.99。优于SoftSFT(保性能但降有害率有限)与RESTA(有害率相近但任务性能损失巨大);对Llama3.1 8B、Qwen2.5 32B等新模型同样有效,且对恢复数据集规模/多样性/分布不敏感。

本文以"方向"视角为对齐损失提供了可操作且轻量的修复路径:只动极少数权重即可找回安全行为,权衡显著优于既有方法,工程上可即插即用。局限:恢复效果与原模型对齐强度相关(LLAMA2系列零有害率时只能近似恢复);对分类任务(TOXIC/CHEAT)不如生成任务理想;方向层选择与超参依赖经验;需访问原对齐模型权重与少量有害提示。总体是"微调后对齐修复"这一实用问题的有力进展。
- **arXiv PDF**: [Alleviating the Fear of Losing Alignment in LLM Fine-tuning.](https://arxiv.org/pdf/2504.09757)

#### DataSentinel: A Game-Theoretic Detection of Prompt Injection Attacks.
- **作者**: Yupei Liu, Yuqi Jia, Jinyuan Jia, Dawn Song, Neil Zhenqiang Gong
- **方向**: 大模型安全
- **解读**: LLM集成应用（AI搜索、GPT Store等）在整合不可信外部数据时极易遭受提示注入攻击。检测方法中效果最好的"已知答案检测"（secret key 探测）受限于标准LLM：误报率偏高，且未针对自适应攻击设计。本文提出博弈论检测框架 DataSentinel，核心洞察是把检测LLM"训练得更易受提示注入"，使其面对污染数据时更可能跟随注入指令而漏掉秘密密钥，从而将脆弱性转化为防御。

方法上，作者将检测LLM的微调形式化为极小极大优化问题：内层最大化模拟强自适应攻击（优化污染数据，同时欺骗检测LLM并误导后端LLM执行注入任务），外层最小化微调检测LLM以同时降低FPR与FNR；求解时交替进行内层GCG离散优化与外层梯度微调（QLoRA）。

实验在7个目标任务、9种现有攻击（6种启发式+3种优化式）与6个LLM上评估，FPR接近0、FNR最高仅0.07；相比6个基线优势显著，如已知答案检测FPR最高0.1、对优化式攻击FNR最高0.21。检测单次查询约1.6秒，仅为后端推理时间（15.3秒）的10%；微调约3小时（云端成本约0.9美元）。

我的思考：把"越脆弱越能检测"的思路反转巧妙，且对自适应攻击有效，是首个博弈论式提示注入检测方案；但作者也承认当注入任务与目标任务相同时会退化为传统对抗样本检测难题，效果下降；安全性依赖攻击者不知道秘密密钥这一密码学常规假设，且实验仅覆盖开源模型，其真实部署泛化性仍有待检验。

- **arXiv PDF**: [DataSentinel: A Game-Theoretic Detection of Prompt Injection Attacks.](https://arxiv.org/pdf/2504.11358)

#### Watermarking Language Models for Many Adaptive Users.
- **作者**: Aloni Cohen, Alexander Hoover, Gabe Schoenbach
- **方向**: 大模型安全
- **解读**: 已有LLM水印方案存在两大缺陷：一是对自适应提示（用户多次交互查询）没有任何鲁棒性保证，而现实用户必然多次查询；二是除个别例外都限于零比特水印，只能判断文本是否机器生成，无法提取额外信息，因而难以制止如聊天机器人诈骗等具体滥用。

本文提出多用户水印：可将模型生成文本追溯到具体用户或合谋用户群体，即使面对自适应提示。核心技术是首次给出从零比特水印到L比特水印的黑盒归约，为此提出统一抽象AEB-robustness（文本只要"近似足够多块"模型生成输出即可检出），并证明CGZ23的不可检测方案是自适应鲁棒的；同时给出反例说明非自适应鲁棒性并不蕴含自适应鲁棒性。

方案参数：无合谋时检测与追踪均O(log n)（信息论最优），c人合谋时追踪O(n log n)；L比特方案需k=O(Lλ)个块被近似，多用户方案在c合谋下需k=O(c²log²c·log n·λ)个块。短片段照常检测、长片段可追溯个人，两者保证兼得。

我的思考：这是水印方案间首个通用归约，统一了此前各方案各自为政的鲁棒性定义，理论贡献扎实；但要求底层方案具备密码学强度的不可检测性、可靠性与AEB鲁棒性，实际收益取决于CGZ23等具体实例化；对自适应鲁棒性的反例也提醒社区，真实交互场景的保证不能从单查询定义想当然推导。

- **arXiv PDF**: [Watermarking Language Models for Many Adaptive Users.](https://arxiv.org/pdf/2405.11109)

#### SV-TrustEval-C: Evaluating Structure and Semantic Reasoning in Large Language Models for Source Code Vulnerability Analysis.
- **作者**: Yansong Li, Paula Branco, Alexander M. Hoole, Manish Marwah, Hari Manassery Koduvely, Guy-Vincent Jourdan, Stephan Jou
- **方向**: 大模型安全
- **解读**: 现有漏洞评测基准（BigVul、CVEFixes、DiverseVul、SecLLMHolmes 等）主要考核识别/修复，无法区分模型是靠预训练模式匹配还是真正的逻辑推理完成分析，而这恰恰决定 LLM 漏洞分析的可靠性。本文提出 SV-TrustEval-C，从结构推理（数据流/控制流）与语义推理（反事实、目标驱动、预测）两维度评测 C 语言漏洞分析能力。

基于 Juliet 测试套件构建：377 个基础文件、覆盖 82 个 CWE，生成 1,297 个不安全与 1,286 个安全变体并构造 9,401 道选择题。核心是结构导向变体生成器：Flow Extractor 用 tree-sitter 解析 AST 构建数据流/控制流图，Behavior Simulator 把代码注入为安全/不安全/功能受损三类并叠加 Outer/Inner/Outer&Inner 掩码控制结构提升难度；做了标签掩蔽，11 个 LLM 在 zero-shot 与 ICL 下评测。

结构推理上 >15B 模型显著占优，Llama3.1-405B 平均 68.58%（zero-shot）/69.14%（ICL）超过 GPT-4；但语义推理普遍很差，zero-shot 平均低于 32%、ICL 低于 46%，目标驱动任务尤其薄弱；CodeLlama、CodeQwen、Gemma 等把安全代码误判为不安全的 FPR 接近 100%；一致性分析表明语义场景一致率平均低于 50%，部分模型目标驱动一致率为 0%，说明成功来自模式匹配而非理解。

我的思考：该基准首次把“推理 vs 模式匹配”作为评测对象，用图结构驱动变体与一致性指标给出量化证据，是对 SecLLMHolmes 仅 48 个样本、句子相似度评估的升级。局限：基于合成 Juliet 数据虽保证标签 100% 准确，却偏离真实漏洞形态；仅覆盖 C 语言；四选一形式存在猜中基线。“LLM 漏洞分析依赖模式匹配”的结论与 CSEBenchmark 等互相印证，提示安全场景需推理增强与专用微调，而非单纯堆参数。

- **arXiv PDF**: [SV-TrustEval-C: Evaluating Structure and Semantic Reasoning in Large Language Models for Source Code Vulnerability Analysis.](https://arxiv.org/pdf/2505.20630)

#### The Digital Cybersecurity Expert: How Far Have We Come?
- **作者**: Dawei Wang, Geng Zhou, Xianglong Li, Yu Bai, Li Chen, Ting Qin, Jian Sun, Dan Li
- **方向**: 大模型安全
- **解读**: LLM 正被部署到威胁情报、漏洞管理、安全运营等岗位（Copilot for Security、Gemini in Security），但现有评测粒度粗、无法定位模型的具体知识缺口，也缺少“网络安全专家应掌握什么”的完整框架。本文提出 CSEBenchmark：基于三份社区路线图构建 7 个子域共 345 个细粒度知识点的专家知识框架，按认知科学分为 121 个事实型、136 个概念型、88 个程序型。

针对三类知识选取不同材料（wiki/教材/官方文档），用 GPT-4-turbo 按“每主题 5 题”自适应生成，经语义去重（阈值 0.85）后人工校验修正（672 人时审阅+100 人时修正，$234.5），得到 11,050 道高质量单选题。评测 12 个主流 LLM（3B–671B），用 zero-shot/few-shot/CoT 取最高分，每问 5 次推理全对且答案轮换 4 位置全对才算掌握，用 xFinder 抽取答案。

GPT-4o 以 85.42% 居首，Deepseek-V3（84.92%）为最佳开源模型，Qwen-2.5-72B（84.40%）紧随；事实/概念知识中位数接近 92%，而程序性知识中位数仅 71.86%（最低 43.09%），缺口集中在专业工具与冷门命令（Metasploit、Burp、Wireshark 等）；69 个知识点存在缺口且模型间各异——Llama-3.1-70B 在 tcpdump 上反不如 8B 版本。用 RAG 注入缺口对应 Q&A 后，在三个任务基准上最多纠正 84% 的此前错误；角色对齐显示 GPT-4o 最适配 Google 高级情报分析师、Deepseek-V3 最适配 Amazon 隐私工程师。

我的思考：把“知识缺口可定位、可补救”做成闭环是该工作最大亮点——细粒度框架让 RAG 补强立竿见影，验证了评测结论可靠性；角色对齐评测为模型选型提供新维度。局限：MCQ 形式难覆盖开放式推理，GPT-4-turbo 既出题又参评存在循环偏差（作者已承认），程序性题目分布偏斜且仅英文。整体给出了“数字安全专家离我们多远”的量化答案：知识层接近、实操层尚远。

- **arXiv PDF**: [The Digital Cybersecurity Expert: How Far Have We Come?](https://arxiv.org/pdf/2504.11783)

### Web安全（7 篇）

#### Identifying Incoherent Search Sessions: Search Click Fraud Remediation Under Real-World Constraints.
- **作者**: Runze Zhang, Ranjita Pai Sridhar, Mingxuan Yao, Zheng Yang, David Oygenblik, Haichuan Xu, Vacha Dave, Cormac Herley, Paul England, Brendan Saltaformaggio
- **方向**: Web安全
- **解读**:
点击欺诈持续给搜索引擎和广告主造成巨额财务损失，且欺诈手段不断演进以绕过现有检测算法；传统方法多聚焦单个搜索请求，对会话层面行为的理解有限。

作者将分析单位从单次请求转向"会话"——同一用户的连续查询序列。他们发现良性用户在同一会话内呈现连贯行为（通常聚焦单一主题），而欺诈者与自动化机器人常表现出多样化、不合逻辑、不连贯的行为。据此提出 CoSeC 系统，量化会话的"不连贯指数"（incoherence index），综合字面、语义、时间与广告点击特征评估会话连贯性。

评估显示 CoSeC 识别欺诈会话的精确率达 95.79%、召回率 92.40%，展示了在真实约束下增强现有检测的潜力。

从"请求"到"会话"的视角转换是该工作的重要贡献，不连贯指数具有可解释性，便于部署诊断。局限在于特征可能随欺诈策略演化而衰减，且评估环境与真实流量存在差距。该工作表明行为级特征可为点击欺诈检测提供稳健补充信号。


#### MOCGuard: Automatically Detecting Missing-Owner-Check Vulnerabilities in Java Web Applications.
- **作者**: Fengyu Liu, Youkun Shi, Yuan Zhang, Guangliang Yang, Enhao Li, Min Yang
- **方向**: Web安全
- **解读**: Java Web 应用广泛承载高价值商业网站，但其复杂性使它们易受 Missing-Owner-Check（MOC）漏洞影响——未校验资源属主导致越权访问与数据泄露，而多年来的相关研究却十分有限。本文提出端到端漏洞分析方案 MOCGuard，可有效筛查此类问题。

方法上，MOCGuard 与既有技术不同，采用“数据库中心”的新视角：首先依据数据库结构推断用户表与用户自有数据，确定属主语义；随后跨 SQL 层与代码层检查不安全的校验逻辑，定位缺失属主检查的访问路径。

评估方面，作者与一家全球领先科技公司合作，在 30 个高知名度开源应用与 7 个工业应用上验证，证明其自动化与有效性：成功发现 161 个已确认的 0-day 漏洞，并获得 73 个 CVE 编号。

我认为“数据库中心”的分析视角巧妙绕开了难以建模的属主语义问题，161 个 0-day 与 73 个 CVE 的结果极具说服力。局限在于依赖数据库 schema 推断属主关系，对非关系型数据库或更复杂授权模型（如角色层级）的适用性尚未验证；与对象级授权（IDOR）检测类工作形成互补，但把二者统一起来的框架仍待探索。


#### Racedb: Detecting Request Race Vulnerabilities in Database-Backed Web Applications.
- **作者**: An Chen, Yonghwi Kwon, Kyu Hyung Lee
- **方向**: Web安全
- **解读**: 数据库支撑的 Web 应用中，请求竞争（request race）漏洞可导致数据不一致、非预期行为甚至越权访问，是重大安全威胁；但应用逻辑与数据库交互的复杂交织使现有自动检测技术力不从心。本文提出 Racedb 系统，通过两项关键创新应对挑战。

方法上，第一项是应用感知竞争检测（ARD）：对依赖关系做全面分析，不仅考虑查询依赖，还考虑代码依赖，从而识别出既有方法容易遗漏的细微竞争；第二项是基于重放执行的验证技术，高效地把真实竞争与误报区分开，并为已确认的漏洞自动生成确定性可利用的证明。

评估方面，Racedb 在 14 个真实 PHP 应用上测试：识别出 21 个已知漏洞并新发现 18 个漏洞，显著超越现有工具，同时保持更低的误报率；所有新发现漏洞均已负责任地报告给开发者，其中 7 个已获 CVE 编号。

我认为“依赖分析+重放验证”的两段式设计直击竞争检测高误报的痛点，重放生成确定性 exploit 的机制也提升了结果可信度。局限在于评估局限于 PHP 应用，重放验证在时序敏感的大规模并发场景下的可扩展性尚待验证；其与 TOCTOU 检测的整合是自然的后续方向。


#### PFortifier: Mitigating PHP Object Injection Through Automatic Patch Generation.
- **作者**: Bo Pang, Yiheng Zhang, Mingzhe Gao, Junzhe Zhang, Ligeng Chen, Mingxue Zhang, Gang Liang
- **方向**: Web安全
- **解读**: PHP 对象注入（POI）漏洞使应用中的类方法被意外执行，可引发各类严重攻击；而为 POI 设计有效补丁需要大量工程投入。现有研究集中于 gadget 链检测，自动补丁生成几乎未被探索。本文先经验性研究已知漏洞，发现攻击者通常通过分叉到开发者从未考虑过的路径来构造利用链，这些被意外跳入并执行的方法被称为 possible methods（PM）。

方法上，基于这一观察提出 PFortifier 框架，分两阶段工作：(i) 链分析阶段——模拟并检测攻击者可控对象是否到达危险 sink；(ii) 补丁生成阶段——自动生成限制第一阶段发现的 PM 跳转的补丁，阻断利用链。

评估方面，在 31 个应用与框架上实验：生成的补丁精确率为 52.53%，潜在修复率 45.45%，总体覆盖率高达 97.98%。

我认为以“PM 跳转”刻画 POI 根因、把补丁生成转化为可达性约束问题，思路新颖且能全自动执行，覆盖率近 98% 说明方案覆盖面扎实。局限在于精确率刚过半数，自动补丁可能误伤合法的反序列化功能，需要人审或更细粒度的跳转约束；与既有 gadget 挖掘工作结合可形成“检测-修复”闭环，是明确的前进方向。


#### Detecting Taint-Style Vulnerabilities in Microservice-Structured Web Applications.
- **作者**: Fengyu Liu, Yuan Zhang, Tian Chen, Youkun Shi, Guangliang Yang, Zihan Lin, Min Yang, Junyao He, Qi Li
- **方向**: Web安全
- **解读**: 微服务架构凭借可扩展、易维护的优势日益流行，其松耦合设计被认为通过服务间隔离增强了安全性；但本文研究表明，微服务 Web 应用仍深受污点类漏洞（最严重的漏洞类型之一）困扰。为此提出新型分析方案 MScan，可有效检测真实世界快速演进应用中的此类漏洞。

方法上，MScan 由三阶段组成：第一，采用网关中心分析识别外部恶意用户可达的入口点；第二，利用新数据结构“服务依赖图”桥接服务间通信，建模跨服务数据流；第三，采用距离引导策略做选择性上下文敏感污点分析，在精度与开销间取得平衡。

评估方面，在 25 个开源应用与 5 个工业应用（来自全球领先金融科技公司）上验证，发现 59 个高危 0-day 漏洞并完成负责任披露，截至目前已有 31 个获得 CVE 编号。

我认为“网关中心入口识别+跨服务依赖图”是针对微服务复杂性的务实设计，59 个 0-day 与 31 个 CVE 证明其真实效果。局限在于依赖统一网关入口的架构假设，对无网关或服务间直连的部署适用性存疑，上下文敏感分析在大规模集群上的成本控制细节也未充分展开；微服务安全分析整体仍是蓝海。


#### Follow My Flow: Unveiling Client-Side Prototype Pollution Gadgets from One Million Real-World Websites.
- **作者**: Zifeng Kang, Muxi Lyu, Zhengyu Liu, Jianjia Yu, Runqi Fan, Song Li, Yinzhi Cao
- **方向**: Web安全
- **解读**: 原型污染漏洞通常需借助 gadget（改变受害程序控制流或数据流的代码片段）才能升级为 XSS、cookie 操纵等实际危害。既有工作难以发现这类 gadget：有时需注入复杂属性值以替换既有未定义属性，此类场景约束求解器无法处理。本文设计动态分析框架 Gala，自动检测真实网站中的客户端原型污染 gadget。

方法上，Gala 的关键洞察是“借用”：从不受污染但属性未定义的网站中借用 gadget 定义，从而引导数据流向 sink，绕开复杂的属性值求解问题。

评估方面，Gala 对一百万个真实网站进行分析，揭示出 133 个既有工作未发现的 0-day gadget；例如一个来自 Meta 的软件、另一个来自 Vue 框架，Meta 已确认并修复，授予漏洞赏金并分配 CVE-2024-6783；另有 23 个此前未报道的漏洞，均已负责任地披露给开发者。

我认为“借用未定义属性 gadget”的洞察优雅地绕开了约束求解瓶颈，百万级规模的动态分析证据非常扎实。局限在于动态分析覆盖依赖实际触发路径，且在不同库间迁移 gadget 语义的泛化性值得深究；该工作标志着原型污染研究正从“能否污染”走向“能否利用”。


#### 403 Forbidden? Ethically Evaluating Broken Access Control in the Wild.
- **作者**: Saiid El Hajj Chehade; Florian Hantke; Ben Stock
- **方向**: Web安全
- **解读**: 越权访问（broken access control）是 OWASP Top Ten 中 Web 应用最普遍的漏洞类型。由于访问控制在服务端实现、研究者无法接触线上系统代码，对真实环境中越权问题的研究长期受限；此前工作多在研究者自控环境中分析开源应用，而针对线上系统的大规模研究因"可能泄露不知情用户数据"的伦理与法律顾虑而缺席。

本文提出 Variable Swapping Framework（VSF），首个在伦理上站得住脚、可扩展的黑盒框架，用于检测线上系统的越权模式。其设计基于深入的利益相关者分析，在风险最小化与漏洞检测收益最大化之间取得平衡；核心机制是每个站点使用两个账户，在两者之间交换资源标识符，将一方的资源暴露给另一方以探测越权。

在 100 个成功测试的应用上，VSF 发现共 584 个对访问控制敏感的 HTTP 端点，其中 19 个（分布在 7 个站点）为可利用的越权缺陷，并已负责任披露。

我的思考：该工作最大的贡献是把"线上越权检测"从伦理禁区变成了可操作的范式——以双账户标识符交换替代真实数据泄露，配合利益相关者分析，为同类研究提供了伦理模板。局限在于只覆盖交换型越权模式（如 IDOR），对水平/垂直越权之外的类别覆盖有限；19/584 的可利用率也说明黑盒检测仍存在大量误报空间。相比开源应用的静态/白盒研究，它更贴近真实攻击面，具有方法论示范价值。


### 恶意软件与二进制分析（5 篇）

#### TypeForge: Synthesizing and Selecting Best-Fit Composite Data Types for Stripped Binaries.
- **作者**: Yanzhong Wang, Ruigang Liang, Yilin Li, Peiwei Hu, Kai Chen, Bolun Zhang
- **方向**: 恶意软件与二进制分析
- **解读**:
剥离（stripped）二进制缺少类型信息，尤其复合数据类型（如结构体）的缺失，使静态分析器与逆向工程专家难以高效、准确分析闭源软件；现有方法在准确性上不足、可扩展性受限。恢复复合类型因此成为二进制分析中的关键问题。

受专家工作流启发，TypeForge 采用"合成—选择"两阶段策略自动化类型恢复。它设计了新的图结构 Type Flow Graph（TFG）表示二进制内部关系；第一阶段基于 TFG 的合成聚焦于高效准确地构建约束并合成候选类型声明；第二阶段提出 LLM 辅助的双淘汰框架，通过评估反编译代码的可读性从候选中选出最佳类型声明。

实验显示，TYPEFORGE 在复合数据识别上 F1 达 81.7%、布局恢复达 88.2%，显著优于现有方法；关系识别得分 72.1%，而此前方法难以处理该任务。此外其时间开销仅为最佳对比方法 OSPREY 的约 3.8%，效率优势明显。

将 LLM 用于类型候选筛选是本文的亮点，把"可读性"这类主观标准纳入自动化流程颇具新意。局限在于评估依赖反编译可读性、对未知编译风格泛化性未知。相比 OSPREY 等传统约束求解路线，TypeForge 在精度与效率间取得了更好平衡，为大规模逆向工程提供了实用工具。


#### PyLingual: Toward Perfect Decompilation of Evolving High-Level Languages.
- **作者**: Joshua Wiedemeier, Elliot Tarbet, Max Zheng, Sangsoo Ko, Jessica Ouyang, Sang Kil Cha, Kangkook Jee
- **方向**: 恶意软件与二进制分析
- **解读**: 
Python 同时被工业界开发者与恶意软件作者广泛使用，反编译器需求巨大；但 Python 版本演进激进、字节码规范不稳定（每年新增特性、代码生成大变、opcode 增删改），社区维护反编译工具的代价高昂。

方法上，将 NLP 技术与经典程序语言（PL）理论结合：PyLingual 以版本无关核心自动吸收编译器的表层变化（数据驱动的 NLP 组件），同时利用程序化的抽象控制流重建；并引入"完美反编译"这一严格正确性度量——静态可验证的细化语义等价。

结果显示，PyLingual 支持 Python 3.6 至 3.12，在四个数据集上完美反编译率平均比现有最佳工具提升 45%；提供大规模可验证的反编译评估，并上线为公开在线服务。

我的思考：NLP+PL 的混合路线从根本上缓解"版本漂移"维护难题，是反编译领域的范式创新；"完美反编译"可静态验证，度量令人信服。局限在于完美率之外的真实可读性仍需人工评估；对恶意 Python 样本分析而言，这是实用且及时的利器。


#### Disassembly as Weighted Interval Scheduling with Learned Weights.
- **作者**: Antonio Flores-Montoya, Junghee Lim, Adam Seitz, Akshay Sood, Edward Raff, James Holt
- **方向**: 恶意软件与二进制分析
- **解读**: 反汇编是逆向、二进制重写等二进制分析的第一步，难点是从候选指令/数据块中选出真正属于代码的指令。现有方法（D-ARM 等）把冲突消解建模为最大权独立集（MWIS），NP-hard 只能用贪心近似；启发式权重也依赖手工调参。本文提出覆盖 x86、x64、arm32、aarch64 的通用三段式反汇编算法。

候选生成扩展 Ddisasm 的前向/后向遍历，支持 arm32 双解码模式（ARM/Thumb）、跳转表与字面量池等数据块候选；分析阶段用 34 条多 ISA 启发式与 58 条 arm32 专用启发式赋权；冲突消解首次把问题归约为加权区间调度（WIS），用精确动态规划求解。权重学习方面，用带真值的二进制做符号化 WIS 推断，把“最优调度”转化为线性约束，再用带软约束的 LP（Pulp）求解，支持不完整标注。

在 SOK、Pangine、Jiang、Assemblage 四个数据集（含 GCC/Clang/ICC/MSVC、ELF/PE）上：1,895 个训练二进制产生 86,564 条约束，仅 301 条不可满足；学习权重把 95 条启发式中的 46 条置零（40%+ 可删），SOK arm32 验证集召回从 99.906% 升至 99.973%，完全正确反汇编的二进制比例从 35.77% 升至 44.06%；全部数据集上召回最高、精度距最优 0.01% 以内，且权重泛化无过拟合。

我的思考：把 NP-hard 冲突消解精确化为多项式可解的 WIS，并把“手工调权重”变成可学习的 LP 问题，思路干净、工程价值高；“学习把哪些启发式置零”还提供了可解释的重要性分析（x64 下跳转启发式权重为 0 而调用非 0，印证假跳转远多于假调用）。局限：候选生成不保证完备，x86/x64 完美反汇编比例较 Ddisasm-1.6 略有回退，对 DASSA 的指标口径存在争议。总体是反汇编精度与自动化方面扎实的推进。

- **arXiv PDF**: [Disassembly as Weighted Interval Scheduling with Learned Weights.](https://arxiv.org/pdf/2505.01536)

#### Inspecting Virtual Machine Diversification Inside Virtualization Obfuscation.
- **作者**: Naiqian Zhang, Dongpeng Xu, Jiang Ming, Jun Xu, Qiaoyan Yu
- **方向**: 恶意软件与二进制分析
- **解读**: 
虚拟化混淆器常被用于保护专有代码或阻碍恶意软件分析；尽管过去十年对抗研究不断，虚拟化仍是极为有效的混淆手段。现代虚拟机（VM）采用多种多样化技术复杂化内部结构，逆向一个 VM 耗时且无法复用于破解其他 VM，但缺乏对这些技术的系统研究。

方法上，首先从解释、字节码组织、处理器置换/重定位等视角系统分类并揭示 VM 内部的多样化技术；其次开发自动化工具识别 SOTA 混淆器实际采用的技术；最后利用新发现修补现有反混淆工具的弱点。

结果表明，该研究揭开了多样化技术在实际部署中的使用方式，修补后的反混淆工具克服了原有弱点，为下一代反混淆工具指明了方向。

我的思考：该工作填补了 VM 多样化技术系统研究的空白，分类法可成为后续研究的标准坐标系；对反混淆社区而言是"知己知彼"的基础性工作。局限是覆盖面受所选混淆器样本限制，新技术出现后分类法需持续扩充；总体为虚拟化混淆攻防提供了重要地图。


#### You Can't Judge a Binary by Its Header: Data-Code Separation for Non-Standard ARM Binaries Using Pseudo Labels.
- **作者**: Hadjer Benkraouda, Nirav Diwan, Gang Wang
- **方向**: 恶意软件与二进制分析
- **解读**: 
静态二进制分析是漏洞发现与恶意软件检测等安全任务的基础，但IoT与ICS厂商持续引入定制或非标准二进制格式，现有工具难以直接处理；人工逆向成本高昂，而"数据-代码分离"（区分字节是代码还是数据）是自动化分析的第一个关键步骤。

本文提出Loadstar，核心思路是利用标准二进制的大量标注数据训练分类器，再通过基于伪标签的域适应方法处理无标注的非标准二进制；同时采用知识启发的伪标签修正规则作为护栏，保证标签质量。该系统的一大优势是不需要对非标准二进制进行任何人工标注。

作者使用三个PLC数据集评估Loadstar，结果显示其在准确率与速度两方面均优于现有方法。工具将开源给社区使用。

"免标注的域适应"是务实且巧妙的技术路线，但伪标签噪声可能随迭代级联放大，修正规则的覆盖范围决定其上限；评估目前限于PLC与ARM生态，向更广非标准格式的泛化尚待验证。与符号执行、模式匹配等传统方法相比，学习式方法迁移成本更低，代表了该问题的前沿方向。


### 漏洞挖掘与利用（6 篇）

#### BridgeRouter: Automated Capability Upgrading of Out-Of-Bounds Write Vulnerabilities to Arbitrary Memory Write Primitives in the Linux Kernel.

- **作者**: Dongchen Xie, Dongnan He, Wei You, Jianjun Huang, Bin Liang, Shuitao Gan, Wenchang Shi

- **方向**: 漏洞挖掘与利用

- **解读**: 内存破坏漏洞对 Linux 内核构成重大威胁，越界写（OOB write）因普遍性备受关注。但现有内核 OOB 利用技术要么要求漏洞具备很强能力，要么要求漏洞对象与受害对象同处一个分配器缓存，要么依赖大规模页表操作——约束严重限制适用性并导致完整利用链成功率低。本文提出 BridgeRouter，仅凭有限能力即可实现任意内存写的实用方法。

方法上，作者利用两类特殊"桥"对象实现能力升级：先借不受控覆盖（uncontrolled overwrite）升级为受控覆盖（controlled overwrite），再达成任意写。为此开发自动化系统识别并利用这些对象，把"触发漏洞"到"达成任意写"的人工编排自动化，显著提高利用链构建效率与成功率。

作者在 14 个真实世界内核漏洞（含手工构造的漏洞）上评估，并与现有 SOTA 利用工作对比，验证广泛适用性——覆盖此前技术难以处理的漏洞类别，为内核利用研究提供新的能力升级路径。

我的思考：本文把内核 OOB 利用中"能力升级"这一关键步骤系统化、自动化：不再依赖手工堆风水与对象选择，用自动识别桥对象的方式把不受控写转化为任意写，直接回应"现有技术约束多、成功率低"的痛点。14 个真实漏洞的验证规模属扎实水平。局限：桥对象可用性依赖内核版本与对象布局，稳定性及对新加固机制的适应性未在摘要展开。与 SLUBStick 等知名内核利用技术相比，BridgeRouter 提供能力升级层面的通用方法论，对攻防双方均有参考价值。


#### Predator: Directed Web Application Fuzzing for Efficient Vulnerability Validation.
- **作者**: Chenlin Wang, Wei Meng, Changhua Luo, Penghui Li
- **方向**: 漏洞挖掘与利用
- **解读**: Web 应用漏洞仍是重大挑战：静态分析是主流手段但误报率高、依赖专家细粒度研判，而动态分析发展不足；现有模糊测试难以深入深层代码位置，最先进的灰盒模糊器也难以从用户界面捕获有效参数，无法高效探索输入空间。本文提出 Predator，一个带选择性插桩的定向模糊测试框架，用于高效的 Web 漏洞检测与验证。

方法上，Predator 让静态与动态分析互补：轻量级运行时为模糊器提供目标相关 URL，从而促进对静态报告的验证；针对 PHP 等解释型语言的特点，设计了运行时距离补充机制与定制化变异策略，引导模糊器逼近深层目标代码。

评估显示 Predator 能更有效地触发漏洞，在时间暴露（time-to-exposure）维度上最多比现有工具快 43.8 倍；在真实世界应用中检测出 26 个此前未知的漏洞，截至写作时有 7 个已获厂商确认并修复。

我认为本文的贡献是把“静态报告验证”与“定向模糊”无缝衔接，工程化价值显著，43.8 倍的提速也说明“验证”阶段是当前流水线的真正瓶颈。局限在于提速数字依赖特定目标集、URL 辅助机制依赖运行时可观测性，其对不同语言与框架的泛化能力仍需更多案例支撑。


#### RGFuzz: Rule-Guided Fuzzer for WebAssembly Runtimes.
- **作者**: Junyoung Park, Yunho Kim, Insu Yun
- **方向**: 漏洞挖掘与利用
- **解读**: WebAssembly 运行时内嵌编译器，把 Wasm 代码编译为机器码执行，其间依赖众多优化与降级（lowering）规则；但现有测试工具难以有效探索这些复杂规则，生成的测试用例多样性不足，导致大量 bug 未被发现。本文提出 RGFuzz，一个针对 WebAssembly 运行时的差分模糊器，通过两项新技术解决上述局限。

方法上，第一项是规则引导模糊：从 Wasmtime 运行时中提取编译器规则，用它们指导测试用例生成，从而深入探索复杂规则触发的代码路径；第二项是逆向基于栈的生成，提升测试用例的多样性。

结果方面，RGFuzz 在 Wasmer、WasmEdge、V8、SpiderMonkey、JavaScriptCore 等六个引擎上实现并评估，共发现 20 个新 bug，其中 1 个获得 CVE 编号；评估表明其利用提取的规则生成用例，表现优于现有模糊器。

我认为把“编译器内部规则”作为模糊引导信号是本文的差异化贡献，差分语义（跨引擎行为不一致）天然适配运行时验证。局限在于规则来源依赖 Wasmtime 单一体、规则集覆盖有限，随着已提取规则饱和，后续发现率可能下降，需持续补充规则来源。


#### SCAD: Towards a Universal and Automated Network Side-Channel Vulnerability Detection.
- **作者**: Keyu Man, Zhongjie Wang, Yu Hao, Shenghan Zheng, Xin'an Zhou, Yue Cao, Zhiyun Qian
- **方向**: 漏洞挖掘与利用
- **解读**: 网络侧信道攻击因后果严重且隐蔽性强而备受关注，例如 SADDNS 允许离路径攻击者利用网络侧信道实施缓存投毒。由于侧信道的本质十分微妙，针对此类漏洞的自动化发现技术很少，且现有工具通用性不足，限制了其长期价值。本文提出 SCAD，旨在填补这一空白，是首个面向该问题的通用自动化检测方案。

方法上，SCAD 的目标是识别非干涉性（non-interference）性质的违例——这被普遍认为是网络侧信道漏洞的根源。由于非干涉性属于超性质（hyperproperty），需要跨多条执行轨迹进行推理，作者据此采用基于欠约束动态符号执行（under-constrained dynamic symbolic execution）的技术路线实现自动化检测。

摘要未给出量化结果，核心贡献是首次将网络侧信道漏洞检测提升为通用、自动化的框架，并明确以非干涉性作为统一检测目标。

我认为该工作的理论定位清晰：把隐蔽的侧信道漏洞规约为可机械验证的非干涉性违例，欠约束符号执行则缓解了状态爆炸问题。其成色取决于能否发现全新的漏洞类别而非复现已知模式，以及符号执行对网络时序通道的建模精度；与 SADDNS 等个案式研究相比，通用性是最大增量。


#### Firmrca: Towards Post-Fuzzing Analysis on ARM Embedded Firmware with Efficient Event-Based Fault Localization.
- **作者**: Boyu Chang, Binbin Zhao, Qiao Zhang, Peiyu Liu, Yuan Tian, Raheem Beyah, Shouling Ji
- **方向**: 漏洞挖掘与利用
- **解读**: 固件 fuzzing 找到崩溃样本只是第一步，定位根因才是费时费力的后续工作；固件缺 sanitizer 等调试机制、剥离符号的裸二进制导致过度污染与噪声，自动根因分析基本空白。本文提出 FirmRCA，面向 32 位 ARM Cortex-M 的后验式故障定位框架，把崩溃归为非法内存访问与非法指令执行两类直接原因。

三部分：一是基于固件重托管（Unicorn）的事件足迹收集——记录每次内存读/写与指令执行（数据/动作事件）的具体地址与数据，用具体内存访问替代昂贵的别名推理，显著加速逆向执行；二是逆向执行——用 Capstone 构建 use-define 链，用数据事件恢复被不可逆指令遮蔽的历史值以解决别名问题；三是两阶段根因分析——后向污点分析 + 两个启发式排序：冗余循环污点抑制（降低循环重复指令分数）与历史写污点优先（提高写入崩溃相关地址的指令分数）。

用 Fuzzware 复现 41 个崩溃用例（17 个固件镜像）：92.7%（38/41）的根因进入前 10 条指令，深根因（距崩溃点超轨迹 50%）11 例成功 10 例；对比 POMP（7.3%）、POMP++（19.5%）成功率大幅领先，完整轨迹分析能力提升 27.8%，事件足迹收集平均仅 5.47s/23.16MB，别名解析平均 <0.01s（POMP 为 1,089.6s），整体为多项式级加速；历史写策略使 top-1 命中率从 39% 提到 73.2%。

我的思考：填补固件 fuzzing 后分析的关键空白，“用具体内存访问足迹直接消除别名不确定性”切中后验分析痛点，比 POMP 系列靠推断更准确高效，深根因能力令人信服。局限：只建模两类直接崩溃原因、仅支持 Cortex-M 与重托管环境、对比基线由作者自行移植到 ARM 公平性存疑；排名启发式在个别用例有轻微副作用。整体为固件漏洞闭环补上实用一环。
- **arXiv PDF**: [Firmrca: Towards Post-Fuzzing Analysis on ARM Embedded Firmware with Efficient Event-Based Fault Localization.](https://arxiv.org/pdf/2410.18483)

#### HouseFuzz: Service-Aware Grey-Box Fuzzing for Vulnerability Detection in Linux-Based Firmware.
- **作者**: Haoyu Xiao, Ziqi Wei, Jiarun Dai, Bowen Li, Yuan Zhang, Min Yang
- **方向**: 漏洞挖掘与利用
- **解读**: 
灰盒模糊测试已成为检测Linux固件漏洞的关键技术，但现有方法普遍忽视固件服务特性带来的三个障碍：仿真过程中多进程服务被过度简化，限制测试范围；固件常使用定制协议，富含严格语义约束，给输入生成带来独特挑战，二者严重制约漏洞发现的效率与效果。

本文提出服务感知工具HouseFuzz：仿真阶段仔细遍历系统初始化流程，识别网络守护进程；测试阶段提供框架支持通过多进程激活的服务进行全面检测；并利用离线与在线分析捕获token级约束，据此生成高质量测试用例。

与SOTA方法相比，HouseFuzz识别出多76%的网络服务，代码覆盖率提升24.8%，在同一数据集上检测到的0-day漏洞多出175%。

"服务感知"直击固件fuzzing的两大痛点（多进程与协议语义），token级约束生成显著提升输入质量；但该约束方法的通用性仍需在更多定制协议上验证。与FirmAFL等经典固件fuzzer相比，HouseFuzz更强调进程间交互与协议语义建模，二者可互补，其175%的0-day增益显示了服务感知路线的潜力。


### 网络与协议安全（20 篇）

#### Characterizing Robocalls with Multiple Vantage Points.
- **作者**: Sathvik Prasad, Aleksandr Nahapetyan, Bradley Reaves
- **方向**: 网络与协议安全
- **解读**: 电话骚扰多年高居美国网络安全关注榜首，行业与监管投入巨大（TRACED Act、STIR/SHAKEN），但既有研究多为单点、单视角，难以回答反骚扰是否奏效、研究刻画是否可靠。论文融合多个独立观测点回答这些问题。

融合 5 个数据源：Robocall Observatory 非交互蜜罐（1005.7 万通，2019-2024）、RRAPTOR 交互蜜罐（94.9 万通）、FTC DNC 投诉（1371 万条）、PPoNE 执法音频与 IRS APWG 数据；用 WavLM 768 维音频嵌入+HDBSCAN 聚类识别活动（聚类覆盖率 76.55%、簇完美度 93.79%），Whisper 转写约 300 万通电话，以 Jaccard/LCS 相似度跨源比对。

骚扰电话长期缓慢下降（斜率 -127 通/月，降幅约 25-50%），但投诉与损失额仍高；机器人已适应 STIR/SHAKEN——签名率从 2021 年 11 月约 60% 升至 2024 年 5 月约 80%，未签名占比从 38.5% 降至 20.7%；交互蜜罐多捕获 60% 音频、约 10 倍回拨号码；跨源黑名单拦截仅 9.8-21.1% 有效（同源 42.6-55.3%）；PPoNE 执法后约 99% 被查活动停止，但调查平均滞后 387 天。

多源交叉验证显著增强结论可信度，音频嵌入聚类方法可并行、可复现、优于指纹法，方法贡献突出；但蜜罐以北美英语样本为主，下降无法归因单一因素，PPoNE 仅覆盖约 5.5% 呼叫。对 STIR/SHAKEN 失效机理的剖析为监管提供了稀缺实证。
- **arXiv PDF**: [Characterizing Robocalls with Multiple Vantage Points.](https://arxiv.org/pdf/2410.17361)

#### Unveiling Security Vulnerabilities in Git Large File Storage Protocol.

- **作者**: Yuan Chen, Qinying Wang, Yong Yang, Yuanchao Chen, Yuwei Li, Shouling Ji

- **方向**: 网络与协议安全

- **解读**: Git LFS 是 Git 生态处理大文件与二进制内容的扩展协议，已被几乎所有主流代码托管平台采用，但其安全影响长期未被系统研究。本文对 LFS 协议进行首次全面分析，提炼出服务器必须满足的 11 项关键安全属性，为协议级漏洞研究奠定基础框架。

方法上，作者从属性出发推导违反属性的攻击路径，提出四类攻击向量：私有数据泄露、替换、基于配额的限制服务拒绝与配额逃逸。这些攻击利用实际服务器实现的弱点，可导致敏感文件未授权访问、恶意软件注入、公共仓库中断与资源滥用。为验证可利用性，作者开发了半自动黑盒测试工具。

工具被应用于 14 个主流平台，共发现 36 个此前未知漏洞，均已负责任披露并获积极反馈，以及超过 1800 美元漏洞赏金——侧面印证发现质量。这些漏洞中包括私有仓库泄露与恶意文件替换等高风险类别。

我的思考：本文首次把代码托管基础设施协议作为一等安全研究对象——LFS 处于供应链上游，其漏洞可被放大为大规模供应链投毒，影响面远超单个仓库。"属性清单→攻击向量→黑盒验证"的路径清晰可复用。局限：半自动测试覆盖度依赖平台行为差异，36 个漏洞的分布与严重度需查原文。与针对 Git 本体、容器镜像仓库的研究相比，本文补齐了 LFS 这一缺口，为托管平台加固提供了可操作的属性检查单。


#### Clubcards for the WebPKI: Smaller Certificate Revocation Tests in Theory and Practice.

- **作者**: John M. Schanck

- **方向**: 网络与协议安全

- **解读**: CRLite 是一种低带宽、低延迟、隐私保护的证书撤销分发机制：聚合器把撤销数据周期编码进紧凑静态哈希集，客户端下载后私有查询。尽管 CRLite 已优于传统 CRL/OCSP，其编码效率仍有优化空间。本文提出新型数据结构"clubcard"，以更小体积完成同样的撤销成员测试。

方法上，作者设计 clubcard 成员资格测试结构，用 Mozilla 基础设施数据评估编码效率。截至 2024 年 11 月，WebPKI 含超过 9 亿张有效证书与大量已撤销证书；给出的实例化仅 6.7 MB 即可承载状态查询——比 2017 年 IEEE S&P 的 CRLite 原论文小 54%，且比其声称的理论下界还低 21%。

针对动态数据集，作者把 clubcard 扩展到增量编码：基于 2024 年底数据，6 小时增量更新压缩后平均仅 26.8 kB，使接近实时的撤销分发真正可行。作者已扩展 Mozilla 基础设施生成 clubcard 并在 Firefox 增加客户端支持，目前在 Firefox Nightly 默认启用检查，同时报告性能并探讨进一步降低带宽的策略。

我的思考：这是"理论下界被实践反超"的罕见案例——clubcard 比 CRLite 还小 21%，说明原方案仍有结构冗余；26.8 kB 的 6 小时增量让客户端频繁同步成为可能，把撤销分发实用性推上新台阶。作为 Mozilla 内部人士的工作，Nightly 默认启用的落地路径使结果可信度高。局限：已撤销证书数量在摘要中缺字，影响压缩率解读；带宽优化可能以编码 CPU 为代价。与 CRLite 系列相比，本文给出更优实例，对 WebPKI 撤销延迟的改善是实打实的进展。


#### AccuRevoke: Enhancing Certificate Revocation with Distributed Cryptographic Accumulators.

- **作者**: Munshi Rejwan Ala Muid, Taejoong Chung, Thang Hoang

- **方向**: 网络与协议安全

- **解读**: 证书撤销对 PKI 安全至关重要，但传统机制存在系统性缺陷：CRL 可扩展性差、带宽高，OCSP 有隐私问题并依赖可能单点故障的集中式基础设施。本文提出 AccuRevoke，利用密码累加器与边缘计算的新撤销方案，同时缓解规模、带宽、隐私与集中化四方面问题。

方法上，AccuRevoke 使客户端无需每次验证都联系 CA：通过分布式门限密码学，即使撤销响应由第三方边缘计算提供商（ECP）生成，真实性与完整性仍可验证，既分散信任又保留可审计性。累加器提供极紧凑证明：成员证明约 21 字节、非成员约 61 字节，远小于 OCSP 响应。为优化见证生成性能，作者引入 GPU 加速显著缩短处理时间。

作者将 AccuRevoke 与现有机制全面对比，证明其在效率、可靠性、可审计性与隐私增强潜力上的优势；评估表明它是可扩展实用的撤销检查方案，能改善 TLS/PKI 部署。论文计划开源设计与实现以促进采用。

我的思考：本文把"密码累加器 + 边缘计算 + 门限信任"系统化套到证书撤销上：21/61 字节的证明尺寸是数量级改进，门限 ECP 把信任从单一 CA 分散化，兼顾可用性与审计；GPU 加速解决累加器更新瓶颈，说明作者关注部署的计算现实。局限：累加器更新传播与跨 ECP 一致性仍是实践难点，"隐私增强潜力"措辞表明尚未完全量化。与 CRLite/Clubcards 的"静态编码下载"路线相比，本文走"交互式证明"路线，两者取舍值得对照，开源承诺是可复现性加分项。


#### Invade the Walled Garden: Evaluating GTP Security in Cellular Networks.
- **作者**: Yiming Zhang, Tao Wan, Yaru Yang, Haixin Duan, Yichen Wang, Jianjun Chen, Zixiang Wei, Xiang Li
- **方向**: 网络与协议安全
- **解读**: 蜂窝回传与核心网长期被视为“围栏花园”，安全性由物理隔离保证，因此既有研究主要聚焦无线接入，对网络接口关注有限。GTP（GPRS 隧道协议）是 3G 到 5G 基站间用户流量管理的基础协议，通常被认为互联网不可达、不可利用；本文首次对真实世界 GTP 部署进行大规模实证评估。

方法上，作者借助半自动工具进行大规模测量，再对暴露的攻击面进行系统评估，并在隔离实验室环境中用开源 4G/5G 项目复现攻击可行性。

结果显示：测量发现约 749,000 个可通过公共互联网访问的有效主机，横跨 162 个国家、1,176 个服务提供商；攻击面评估表明多达 38 类 GTP 消息可被滥用，发动拒绝服务、会话劫持等攻击；实验确认可经蜂窝网络远程劫持，且用户设备还可被武器化为大规模反射式拒绝服务（RDoS）攻击。

我认为这项工作以令人信服的量级打破了“围栏花园”假设，749,000 个暴露主机的数据对运营商与监管者有直接冲击力。局限在于主机可达性不等于可利用性，部分攻击依赖特定配置与拓扑；其结论为运营商加固核心网（过滤、认证、GTP 防护）提供了紧迫依据，也把 5G 安全研究的视角从无线接口扩展到核心网接口。


#### Learning from Censored Experiences: Social Media Discussions around Censorship Circumvention Technologies.

- **作者**: Elham Pourabbas Vafa, Mohit Singhal, Poojitha Thota, Sayak Saha Roy

- **方向**: 网络与协议安全

- **解读**: 在严格互联网审查时期，维持信息可达至关重要，但用户往往需在复杂路径中摸索有效的审查规避技术（CCTs），如 VPN、代理与替代连接方案。现有研究多从技术层面分析规避工具，对真实用户如何学习、选择与讨论这些工具的社区动态缺乏理解。本文利用审查高峰期的社交媒体真实数据，研究 CCT 讨论对数字权利、隐私与治理的影响。

方法上，作者从 Twitter 与 Telegram 收集 2022 年 9 月 18 日至 2023 年 1 月 31 日期间超过 5,000 万条帖子，采用混合方法：先建立讨论编码手册，覆盖英语并首次纳入波斯语内容，归纳用户绕过限制时的主要问题；再开展为期 20 周的纵向研究追踪 VPN 偏好的演化。

结果显示，社区通过协作共享与讨论知识资源展现出显著的韧性与适应性；跨语种讨论存在共性关切，包括可追溯性、可识别性及意外使用恶意配置的风险。时间分析发现 VPN 偏好随审查策略变化而转移，更注重隐私与可访问性的功能带来更高采用率；还发现多个专门分享伪装文件与免费服务的流行频道。

我的思考：本文把审查规避研究从"工具能力"转向"用户实践"，首次为波斯语 CCT 讨论建立编码体系，填补非英语社区研究的空白；混合方法与 20 周纵向设计能捕捉审查博弈的动态性。局限：社交媒体样本存在自我选择偏差，平台数据政策限制可复现性。其政策启示是：审查会催生地下知识经济，纯粹封锁难以消灭信息流动，反而推动用户转向更隐蔽、更去中心化的工具与社群。


#### Transport Layer Obscurity: Circumventing SNI Censorship on the TLS-Layer.

- **作者**: Niklas Niere, Felix Lange, Robert Merget, Juraj Somorovsky

- **方向**: 网络与协议安全

- **解读**: HTTPS 占据互联网流量的大部分，长期是各国审查的对象。审查方通过分析 TLS 协议（尤其是明文 SNI 字段）识别并阻断加密流量，而此前的规避研究主要聚焦 TCP 等传输层协议。本文提出假设：TLS 协议本身蕴含此前未被发掘的规避机会，并系统探索在 TLS 层对抗 SNI 审查的技术空间。

方法上，作者构思并实现了多类作用于 TLS 协议的 SNI 规避技术，评估其在流行服务器上的接受度，随后在中国与伊朗的真实网络环境中验证规避效果，并对结果进行系统归纳。

结果显示这些技术确能在中国和伊朗成功规避基于 SNI 的审查。作者总结出 38 种部分符合标准（partially standard-compliant）的不同技术，归并为 11 个独特类别；证据显示至少存在三类不同的审查设备，作者推测中国的审查架构可能具有相似结构，呼吁学界预判审查设备的演化。

我的思考：本文把规避研究从 TCP 层拉回到协议语义更丰富的 TLS 层，展示"符合标准与否"并非二分——部分标准合规的技术恰好落在审查设备解析逻辑的盲区；38 种技术、11 个类别的系统归纳为后续工作提供了可复用图谱。局限：摘要未给出成功率与服务器接受度的具体比例，且"部分合规"式规避本质是猫鼠游戏，审查设备升级后可能失效。总体而言，这是"协议模糊性即审查盲区"的有力实证，也再次印证审查中间盒的实现质量往往低于其宣称的封锁能力。


#### A Wall Behind A Wall: Emerging Regional Censorship in China.

- **作者**: Mingshi Wu, Ali Zohaib, Zakir Durumeric, Amir Houmansadr, Eric Wustrow

- **方向**: 网络与协议安全

- **解读**: 中国的互联网审查长期由相对集中的政策与统一实施构成，即"国家防火墙"（GFW）。然而自 2023 年 8 月起有迹象表明，河南省部署了自己的省级区域审查系统。本文首次对省级审查进行系统性表征，并与国家层面 GFW 对比，考察中国审查体系从"一堵墙"向"墙中之墙"演化的新趋势。

方法上，作者对河南省级审查机制进行测量与逆向分析，确认其采用 TLS SNI 与 HTTP Host 字段检查，对离开该省的流量进行检测与阻断；同时与 GFW 的封锁行为做对照实验，并基于观察到的解析缺陷与注入行为设计简单的客户端侧绕过方法。

结果显示，河南省级审查在技术复杂度与鲁棒性上不如 GFW——对典型网络波动更敏感，但对二级域名的封锁更激进且波动更大，部分时点封锁的网站数量达到 GFW 的十倍。基于发现的解析缺陷与注入行为，作者提出的客户端侧方法能够绕过该审查。

我的思考：本文记录了一个令人警惕的信号——中国审查体系正从单一集中式架构向区域化、碎片化演化，省级实体独立部署审查能力的先例一旦确立，可能扩散至更多省份，形成层层叠加的审查纵深。区域系统通常比国家级更粗糙，蕴含更多可被利用的解析缺陷，本文的绕过方法正是受益于此。局限：单省案例能否外推尚需更多测量，绕过方法的长效性取决于审查方是否修补缺陷。


#### Is Nobody There? Good! Globally Measuring Connection Tampering Without Responsive Endhosts.

- **作者**: Sadia Nourin, Erik C. Rye, Kevin Bock, Nguyen Phong Hoang, Dave Levin

- **方向**: 网络与协议安全

- **解读**: 已有大量技术用于测量国家审查或企业防火墙对网络流量的干扰，但几乎所有方法都要求目标国家内有某种参与端点——运行软件的志愿者、VPN、云服务商等。而在真正实施干扰的国家，这类端点往往不可用，导致大量网络无法被测量。本文提出首个无需任何参与端点的全球性主动干扰测量系统。

方法上，作者扩展了两项近期研究的思路：利用测量者控制的外部数据包序列触发目标网络中的中间盒，使其误以为存在连接，从而暴露其干扰行为。在此基础上构建 Mint 系统，将该方法从仅适用于少数国家泛化并自动化到全球 IPv4 与 IPv6 互联网，无需目标国家内任何可响应端点即可进行全网扫描。

结果显示，Mint 能够测量此前方法无法触及的网络、自治系统乃至整个国家的干扰行为；作者还通过若干案例研究展示该工具如何支撑新型测量任务。

我的思考：本文解决了网络测量领域长期的覆盖盲区——"越是被审查的地方越测不到"，此前研究受端点可得性制约，结果天然偏向审查较温和的地区。Mint 的"无端点"设计在方法论上是突破：把中间盒当作可被外部触发与探测的被动对象。局限：基于诱骗中间盒的间接探测，推断的干扰规则可能与真实流量处理存在偏差；全网扫描他国基础设施的伦理与合规边界也值得关注。总体而言，这是审查测量"能力普惠化"的重要一步，与区域审查研究互补，共同完善了对全球审查格局的观测。


#### Asymmetric Mempool DoS Security: Formal Definitions and Provable Secure Designs.

- **作者**: Wanning Ding, Yuzhe Tang, Yibo Wang

- **方向**: 网络与协议安全

- **解读**: 内存池（mempool）是公共区块链中安全攸关的子系统，近期非对称 DoS 攻击已展示其能对以太坊网络造成严重损害：攻击者以极低成本锁定受害者交易、阻塞其打包，形成"攻击成本远低于防御成本"的不对称局面。此前缺乏原则性且非侵入式、带可证明安全性的 DoS 防御设计，本文正面处理这一开放问题。

方法上，作者首先给出基于内存池可观察条件的经济安全定义，为 DoS 防御提供形式化基准；随后提出 SAFERAD 框架，用于构造可证明安全的 DoS 防御设计。为同时防御"驱逐型"与"锁定型"两类攻击，SAFERAD 采用非平凡的设计：在更低成本的驱逐攻击下强制执行攻击成本的上界。

作者基于 Geth 实现原型，并用真实交易轨迹评估。结果显示 SAFERAD 对延迟与区块收入的开销很低，表明其非侵入性与实用性。

我的思考：区块链 DoS 研究中大量工作是经验性缓解，缺乏可证明的安全保证；本文把经济安全形式化（以内存池可观察条件定义安全）是该方向的稀缺贡献，让"防御是否有效"从定性讨论变成可验证属性。SAFERAD 同时覆盖驱逐与锁定两类攻击的机制设计，回应了攻击者会在两种策略间切换的现实。局限：形式化定义依赖对攻击者成本模型的假设，真实矿工/构建者行为（如 MEV 偏好）可能偏离模型；Geth 原型的评估规模与极端负载表现需进一步考察。总体是把"经济安全"引入区块链内存池防御的可证明安全框架。


#### Beyond the Horizon: Uncovering Hosts and Services Behind Misconfigured Firewalls.

- **作者**: Qing Deng, Juefei Pu, Zhaowei Tan, Zhiyun Qian, Srikanth V. Krishnamurthy

- **方向**: 网络与协议安全

- **解读**: 公网 IP 地址使设备与服务暴露于端口扫描与网络攻击之下，防火墙因此被广泛部署以执行安全策略、阻止未授权访问。然而防火墙配置漏洞可使其被绕过，保护形同虚设。本文首次全面研究一个此前被忽视的攻击面：防火墙误配置导致本应受保护的设备暴露于公网——具体而言，有缺陷的规则允许来自特定源端口（special source ports）的入站连接绕过防火墙。

方法上，作者扫描 IPv4 空间中 15 个常见的高风险 TCP/UDP 端口（含两个附加端口）以探测绕过现象的存在性与规模；随后分析受影响主机的暴露程度与安全状态，并通过蜜罐实验观察是否被真实利用。

测量揭示了该现象的广泛存在：识别出超过 200 万个本应不可达的暴露主机，分布于 15,837 个自治系统，使"可观察互联网"在多种协议上最多扩大 12.60%。更重要的是，受影响主机通常比公开可达主机更脆弱——运行过时软件版本、配置薄弱；尽管危害严重，蜜罐实验显示野外主动利用的证据很少。

我的思考：本文发现了"防火墙规则中源端口漏洞"这一隐蔽攻击面，其大规模测量（200 万+暴露主机、15,837 个 AS、可观察面扩大 12.6%）证明这不是理论问题而是普遍现实。最反直觉的发现是"被防火墙保护的主机反而更脆弱"——管理员以为受保护而疏于加固，形成安全假象下的更大风险敞口。局限：扫描推断的"本应不可达"需结合防火墙策略验证；蜜罐显示主动利用有限，该攻击面目前是"潜伏风险"而非"活跃灾难"。启示：防火墙规则应审查特殊源端口处理。

#### MANTIS: Detection of Zero-Day Malicious Domains Leveraging Low Reputed Hosting Infrastructure.
- **作者**: Fatih Deniz, Mohamed Nabeel, Ting Yu, Issa Khalil
- **方向**: 网络与协议安全
- **解读**: 攻击者大量使用短命的一次性域名,内容/日志类检测因cloaking、captcha及时间差常滞后于攻击。本文观察到攻击者为规模化和自动化而重用托管基础设施(某日托管恶意域名的IP超80%来自前7天;恶意IP邻域毒性0.063 vs 全网0.002),据此提出内容无关的MANTIS,在域名刚被托管时就检测零日恶意域名。

每天从VirusTotal feed提取首次出现的seed恶意域名,经被动DNS两级扩展(Domain-IP-Domain-IP,扩展率200、窗口7天)构造高毒性异构图(apex域名、FQDN、IP、/24子网、ASN),配词法+托管+IP特征,用半监督多关系GNN训练;诱导式按需分类由4个GNN编码器集成+Random Forest元学习器完成,支持图外域名的实时预测,并首创基于图结构的特征补全。

每日封禁清单精度99.7%、召回86.9%、FPR 0.1%,平均每天检测约19K新恶意域名,是VirusTotal日新增的5倍以上;按需分类器AUC 0.998。优于Nabeel等(F1 71.3/21.1% FPR)、Galloway(92.4/8.7%)、Ringer(94.0/3.8%)等SOTA。60天后82.3%预测域名被至少一个VT引擎标记,621/1000个由MANTIS首次提交给VT,可提前数天至数周于GSB预警;对MimicIP/MintA对抗扰动在15%扰动率下仍保持90%以上准确率。

MANTIS把恶意域名检测从"看内容"转向"看邻居",以攻击者基础设施复用这一经济学事实为锚点,工程完备(运行超一年、每日封禁清单开源),且刻意剔除流行度特征以抗操纵。局限:seed依赖VT扫描(固有盲区);区分攻击者创建/被攻陷域名虽提升精度(99.0% vs 93.69%)但缩小覆盖;对完全冷启动的新建独立基础设施仍力不从心。

- **arXiv PDF**: [MANTIS: Detection of Zero-Day Malicious Domains Leveraging Low Reputed Hosting Infrastructure.](https://arxiv.org/pdf/2502.09788)

#### Resolution Without Dissent: In-Path Per-Query Sanitization to Defeat Surreptitious Communication Over DNS.
- **作者**: Daiping Liu, Ruian Duan, Jun Wang
- **方向**: 网络与协议安全
- **解读**: DNS 是最基础的互联网组件之一，承载多种用途，流量模式多样，且极少被网络管理员封锁，因此成为攻击者建立隐蔽通信（DNS 隧道）的理想通道，被广泛滥用于命令控制（C2）与企业未批准的 VPN。现有方法完全依赖查询序列的统计特征来检测隧道，但这类方法无法保证零数据泄露，且当被窃取的数据分散到多个根域名传输时可以被绕过；因此现有技术更适合威胁调查与取证分析，而非隧道预防。

为填补这一防护空白，本文提出"路径内逐查询净化"（in-path per-query sanitization）方案：在 DNS 解析路径上对每个查询进行实时净化处理，从机制上阻断攻击者借助 DNS 隐蔽传输数据的可能，而非事后检测（摘要未给出净化处理的具体技术细节）。

摘要未提供量化结果，核心贡献在于论证从"检测"转向"预防/净化"的可行性，即让 DNS 通道对隐蔽通信失去可用性。

我认为这是 DNS 隧道对抗中一次重要的范式转变：统计检测永远存在漏报与绕过空间，而路径内净化从根源上收紧通道。落地难点在于净化逻辑必须与正常 DNS 功能高度兼容（不误伤合法查询），且部署位置（递归解析器、出口网关还是中间盒）决定了覆盖范围与性能开销。


#### SoK: Decoding the Enigma of Encrypted Network Traffic Classifiers.
- **作者**: Nimesha Wickramasinghe, Arash Shaghaghi, Gene Tsudik, Sanjay K. Jha
- **方向**: 网络与协议安全
- **解读**: TLS 1.3等现代加密协议使传统流量分类(NTC)失效,研究转向机器学习,但领域缺乏对方法论的批判审视。本文系统化2015年以来基于原始信息的ML-NTC研究,构建设计选择(粒度、提取策略、原始特征、SII)与基准数据集分类法,并实证检验三大疑点。

通过解析5个常用公开数据集,发现ISCXVPN2016、USTC-TFC2016、ISCXTor2016中未加密流量占比高达98.9%、94.7%、89.3%,且大量使用AES-CBC、3DES、RC4等已废弃密码套件,与现实中93%以上网页已加密(Google报告)严重脱节;据此发布CipherSpectrum(40类×3种TLS 1.3 AEAD套件×1000会话=12万会话),并在ET-BERT、YaTC上做348次特征遮挡实验。

结果证实:去除SII(IP/MAC/端口)使两模型精度平均下降0.36(0.96→0.51、0.90→0.62),显示对标识信息的依赖过拟合;仅加密负载(E1)时两模型精度跌至约0.11-0.14,掩码(E2)与随机化(E3)结果几乎不变,证明TLS 1.3 AEAD下密文唯一可学习特征只有长度,"密文含可学模式"的流行假设不成立;IP ID、校验和、TCP序号等会话性伪影与SNI泄漏显著影响分类。

该SoK的价值在于用大规模实证戳破NTC领域的系统性幻觉:大量"加密流量分类器"实际在学明文或标识符捷径;CipherSpectrum与"避免SII/SNI、警惕会话伪影"等准则可直接指导后续研究。局限:只覆盖原始信息类方法,排除侧信道与多模态;遮挡实验基于两个模型与10类,外推需谨慎;对"负载长度可用"的边界未深入探讨。总体是加密流量分类方法论的重要纠偏。

- **arXiv PDF**: [SoK: Decoding the Enigma of Encrypted Network Traffic Classifiers.](https://arxiv.org/pdf/2503.20093)

#### TrafficFormer: An Efficient Pre-trained Model for Traffic Data.
- **作者**: Guangmeng Zhou, Xiongwen Guo, Zhuotao Liu, Tong Li, Qi Li, Ke Xu
- **方向**: 网络与协议安全
- **解读**: 流量数据蕴含深层的领域特定知识，标注困难，缺乏标注数据严重损害基于学习的流量分析准确率。预训练技术在视觉与自然语言处理领域被广泛用于缓解数据不足问题，但在流量分析领域的探索仍然不足。本文提出高效预训练模型 TrafficFormer，以解决流量分析中标注稀缺这一核心瓶颈。

方法上，TrafficFormer 在预训练阶段引入细粒度多分类任务，以增强模型对流量数据的表征能力；在微调阶段提出利用随机初始化特征字段的增强方法，帮助模型聚焦关键信息、提升泛化。评估覆盖分类任务与协议理解任务两类场景。

实验结果表明 TrafficFormer 在分类与协议理解任务上均取得优越性能（摘要未给出具体数值），验证了预训练-微调范式在流量分析领域的有效性。

我认为该工作把成熟的预训练范式系统地引入流量分析，细粒度多分类预训练任务的设计是亮点，随机初始化字段的微调增强也直觉上类似 dropout 式的正则化，机制合理。需要进一步关注的是预训练数据与部署环境之间的领域漂移问题，以及"高效"声称对应的具体训练/推理开销量化。


#### SYN Proof-of- Work: Improving Volumetric DoS Resilience in TCP.
- **作者**: Samuel DeLaughter, Karen R. Sollins
- **方向**: 网络与协议安全
- **解读**: TCP 洪泛攻击是互联网上存在数十年的常见威胁，近年来在规模与频率上均大幅增加。当前广泛部署的 SYN Cookies 作为主要缓解手段，在面对大规模攻击时表现不佳，甚至可能损害正常性能。本文提出 SYN PoW，用微型工作量证明（proof-of-work）来缓解 TCP 洪泛攻击。

方法上，SYN PoW 与 SYN Cookies 扮演类似角色，但具备几个关键优势：(1) 不发送 SYN-ACK 响应而直接丢弃恶意包，从而保护带宽；(2) 支持网络内验证，使中间盒能在数据包到达目标之前检测并丢弃；(3) 将主要成本负担从受害者转移到攻击者自身；(4) 抗伪造，要求进行源地址验证。

摘要未给出具体实验数值，文章主要呈现并评估了 SYN PoW 的设计，论证其在洪泛场景下相对 SYN Cookies 的机制优势。

我认为把 DoS 防护从"状态分配"转向"计算证明"的思路优雅且符合攻击经济学——让攻击者付出计算成本、受害者零状态开销。落地挑战在于与现有网络中间设备、NAT 场景及高连接率合法客户端（如爬虫）的兼容性，以及工作量证明参数需随攻击规模自适应调整；与 SYN Cookies 的定量对比是评估其价值的关键。


#### Low-Cost and Robust Global Time Synchronization.
- **作者**: Marc Wyss, Marc Frei, Jonghoon Kwon, Adrian Perrig
- **方向**: 网络与协议安全
- **解读**: 众多关键应用依赖精确同步的时间，同步中断可能造成严重的运行安全与网络安全后果。然而，在地理分布广泛的设备之间建立低成本且健壮的同步非常困难。现有全球时间方案大多要求信任单一实体或系统，例如全球导航卫星系统（GNSS）或租用的基础设施提供商，这构成单点故障且成本高昂。

作为替代，一个低成本的方案是通过互联网运行时间同步，但面临三重挑战：(i) 实现高精度同步；(ii) 对故障、配置错误乃至被攻陷节点的健壮性；(iii) 应对拥塞相关问题，如容量型 DDoS 攻击。本文针对这些挑战提出低成本且健壮的全球时间同步方案（摘要未详述具体机制）。

摘要未给出量化结果，核心主张是摆脱对 GNSS 与单一提供商的依赖，同时兼顾精度、健壮性与抗 DDoS 能力。

我认为该工作延续了 Perrig 团队构建去中心化健壮网络基础设施的研究脉络，对关键基础设施的时间韧性意义重大。真正的难点在于：在不可信且易受容量攻击的公共互联网上达成高精度同步，需要在攻击模型、精度目标与部署假设之间取得平衡；其实测精度、抗攻击实验与部署成本是衡量成色的关键指标。


#### Chimera: Fuzzing P4 Network Infrastructure for Multi-Plane Bug Detection and Vulnerability Discovery.
- **作者**: Jiwon Kim, Dave Jing Tian, Benjamin E. Ujcich
- **方向**: 网络与协议安全
- **解读**: 
可编程网络数据平面（如 P4）带来定义转发行为的灵活性，但可编程性也引入新的攻击面；多数 P4 研究只关注数据平面，忽视其与控制平面的集成。作者调查开源实现的既有 bug 报告后观察到，许多漏洞源于两平面交互。

方法上，提出 Chimera 综合模糊器：针对需要多平面输入的缺陷，与现有分别模糊各平面的工具不同，Chimera 使用 concolic 执行捕获控制-数据平面交互，并提出两种利用跨平面依赖的新输入变异策略——解析器感知报文变异（PAPM）与头部引导规则生成（HGRG）。

结果显示，在 ONOS、Stratum、BMv2 上发现 7 个 bug，含 3 个安全关键漏洞、2 个由多平面输入触发；相比 SOTA 单平面模糊器，覆盖率提高 3.5 倍、检测率更高。

我的思考：首次系统处理 P4 基础设施的"跨平面"漏洞，把可编程网络安全研究从单平面扩展到系统级，两种变异策略针对性强、工程完整；局限是评估限于三套开源系统，跨平面模型向更多平台推广有待验证。对 5G、数据中心等可编程网络基础设施的安全保障有直接价值。


#### WireWatch: Measuring the Security of Proprietary Network Encryption in the Global Android Ecosystem.

- **作者**: Mona Wang, Jeffrey Knockel, Zoë Reichert, Prateek Mittal, Jonathan R. Mayer

- **方向**: 网络与协议安全

- **解读**: 应用内网络通信的加密质量直接决定用户数据安全，但大量应用依赖非标准的专有加密。本文提出 WireWatch，一个大规模测量管道，评估 Android 应用的网络安全状况——量化明文流量与非标准专有密码的使用。

方法上，WireWatch 对应用流量做协议识别与密码学审计，区分标准 TLS 与非标准加密，并对专有协议进行逆向与安全属性检验（可解密性、降级风险、证书校验、填充方案等）。

结果：小米应用商店头部应用中 47.6% 使用非标准加密而未加额外保护，而 Google Play 仅为 3.51%；作者分析了最流行的 18 个专有协议，分属 9 个协议家族，包括阿里、爱奇艺、快手、腾讯等设计的密码系统；其中 8 个家族发送的请求允许窃听者解密底层数据、浏览数据与设备元数据，还存在可被降级、不校验 TLS 证书、误用 RSA OAEP 等问题。这些漏洞影响了数据集中 26.9% 的应用。

我的思考：本文用确凿的大规模证据揭示了"生态分裂"的安全代价——第三方应用商店缺乏 Google Play 的安全审查，导致专有加密泛滥且质量堪忧。47.6% 对 3.51% 的悬殊对比极具政策意义，证明应用商店安全治理直接决定用户安全。与既有"TLS 误用测量"工作相比，本文把焦点转向更难处理的专有密码，逆向工作量系统化程度高；局限在于测量基于流量样本，实际可利用性与隐私影响需进一步评估。


#### Sniffing Location Privacy of Video Conference Users Using Free Audio Channels.
- **作者**: Long Huang, Chen Wang
- **方向**: 网络与协议安全
- **解读**: 疫情以来视频会议应用广泛普及，用户以为关闭麦克风、摄像头或使用虚拟背景即可掌控隐私，并默认三条常识：音频只涉及语义信息、麦克风只关乎声学隐私、参会者只能机会性利用意外泄露。本文证明这些假设全部不成立：任何参会者都能隐蔽地探测他人位置，即使摄像头关闭、虚拟背景隐藏了画面。

方法上，合法的双向语音信道被改造为远程传感通道——攻击者发送探测音并接收携带位置特征的回声信号；但会议系统的回声消除功能会阻止主动传感，为此作者开发基于 transformer 的算法，利用编码器与生成式 AI 从严重失真的声音中提取稳定嵌入；并提出两类攻击：信道内攻击（用精心构造的信号突破回声消除）与信道外攻击（利用第三方媒体声音如邮件通知音规避消除）。

实验在 Zoom、Teams、Skype 等商业应用上验证：仅用单个探测音，识别用户常去地点的准确率达 88.3%，识别新（未见、未标注）地点情境的准确率达 88.5%。

我认为这是"主动感知与回声消除"间的一场攻防博弈，信道内/信道外双路径设计说明作者深入考虑了现实部署约束，88%+ 的单音准确率意味着参会者位置几乎无法隐藏，对会议厂商是新的隐私设计挑战。局限在于精度依赖环境中回声特征的稳定性，新地点泛化能力来源于生成式 AI 嵌入的具体机制值得细究，且攻击需合法加入会议，威胁模型需与"参会者可信"的常见假设对照评估。


### 密码学与协议（56 篇）

#### Ringtail: Practical Two-Round Threshold Signatures from Learning with Errors.
- **作者**: Cecilia Boschini, Darya Kaviani, Russell W. F. Lai, Giulio Malavolta, Akira Takahashi, Mehdi Tibouchi
- **方向**: 密码学与协议
- **解读**:
阈值签名将签名密钥拆分给 ℓ 方，任意 t 方子集可联合对消息签名。NIST 近期征集后量子阈值签名，设计具体高效的后量子方案成为紧迫问题。

作者提出并实现了基于格的 Ringtail 方案，首次同时具备一组理想性质：(i) 协议仅两轮，且首轮与消息无关、可离线预处理；(ii) 可扩展到 t≤1024 方；(iii) 基于标准 LWE 假设（随机预言机模型）。在 128 位安全、t=1024 时，签名大小约 13.4 KB、在线通信约 10.5 KB，优于同效率下需三轮（Eurocrypt'24）或依赖新非标准假设（Crypto'24）的既有方案。

作者还进行了跨 8 国 5 大洲的广域网部署实验，发现绝大多数端到端延迟由网络延迟主导，凸显轮数优化对实际部署的关键意义。

两轮协议、大规模可扩展与标准假设三者的首次组合是核心贡献，部署实验体现工程务实性，广域网实测揭示的"延迟由网络主导"现象也为同类协议设计提供了实证指导。局限在于相比 ECDSA 阈值签名开销仍高，签名尺寸在带宽敏感场景是短板，且实现安全性（侧信道等）尚待检验。该工作为 NIST 后量子阈值签名候选提供了强有力参照。


#### Groundhog: A Restart-Based Systems Framework for Increasing Availability in Threshold Cryptosystems.
- **作者**: Ashish Kashinath, Disha Agarwala, Gabriel Kulp, Sourav Das, Sibin Mohan, Radha Venkatagiri
- **方向**: 密码学与协议
- **解读**:
阈值密码系统（TC）用于消除密钥管理服务、签名、加密存储乃至区块链应用中的单点故障，其安全依赖"敌手腐化节点数不超过固定上限"的假设；该假设一旦被打破，整个系统即可被攻陷，且移动敌手可随时间逐步腐化不同节点。

作者提出系统级重启框架 Groundhog，在阈值系统之上增加弹性层：通过设备重启驱逐恶意状态，即使面对最多腐化至只剩一台设备的恶意（移动）敌手，只要足够数量的诚实设备存在，系统可用性仍可保证。该框架基于容器、可组合推广到多类系统，作者将其与两个知名 TC 协议（DiSE 与 BLS 签名）及更简单的 PassAround 协议集成验证。

测量表明 Groundhog 能以低于 7% 的低开销保证高可用性，某些实例中甚至改善了方案性能（文中 §5 给出解释）。

用"重启洗牌"对抗移动敌手是把预防性安全（proactive security）思想系统化、工程化的尝试，且通用性好。局限在于重启时机与同步假设在真实分布式环境中实现复杂，敌手模型（剩一台设备）也较强。总体上是密码协议与系统设计结合的优秀范例。


#### Ring Referral: Efficient Publicly Verifiable Ad hoc Credential Scheme with Issuer and Strong User Anonymity for Decentralized Identity and More.
- **作者**: The-Anh Ta, Xiangyu Hui, Sid Chi-Kin Chau
- **方向**: 密码学与协议
- **解读**:
去中心化身份等场景中，用户常需向第三方签名者获取签名凭据，同时希望不泄露签发者身份。传统匿名凭据与环签名方案难以兼顾"向第三方获取签名"与"隐藏签发者"，现有 issuer-hiding 凭据方案功能也不完整。

作者提出环推荐（ring referral）方案：用户可公开证明自己掌握 ad hoc 授权签发者集合中某一签发者所签消息的合法签名，而不泄露具体签发者。方案支持多项特色性质：环上公开可验证性；即使签发者与验证者合谋也难以追踪用户的强匿名性；透明设置；消息隐藏；多消息下的对数级验证效率；以及需要多个签发者联合签名的阈值扩展。

作者完成了实现并进行了广泛的经验评估（摘要未给出具体性能数字）。

该方案将环签名的匿名思想与第三方签发的凭据模型结合，功能组合（公开可验证+强匿名+透明设置+阈值）在同类中较为全面，可支撑证书隐藏的身份、隐私增强联邦认证、匿名背书与隐私营销等应用。局限在于摘要缺乏量化效率数据，且强匿名性在合谋模型下的实际开销需细读原文评估。


#### Robust Threshold ECDSA with Online-Friendly Design in Three Rounds.
- **作者**: Guofeng Tang, Haiyang Xue
- **方向**: 密码学与协议
- **解读**:
阈值 ECDSA 通过消除单点故障增强密钥保护，其签名过程按是否依赖消息分为离线与在线阶段，"在线友好"指在线阶段成本低的方案；而鲁棒性要求即便存在作恶签名者，只要 t 个半诚实参与者在线，每轮执行仍能成功完成。现有最优在线友好方案（Doerner 等 S&P'24）仅三轮但无鲁棒性，WMY+23（NDSS'23）与 WMC24（NDSS'24）虽具备鲁棒性却需额外轮数（分别 7 轮与 4 轮）或依赖同态加密等高成本在线操作。

作者提出首个同时具备在线友好与鲁棒性的三轮阈值 ECDSA 方案：其在线阶段仅需若干椭圆曲线群运算，计算强度比基于同态加密的方案低 2–3 个数量级。

作者实现了协议并与 WMY+23、WMC24 全面对比，基准结果显示其快 2.5 倍至数百倍；并展示技术可扩展构造鲁棒的 BBS+ 签名。

在轮数与在线计算成本两个维度同时做到最优是核心贡献，工程实现与对比完整。局限在于安全性依赖半诚实参与者数量 t 的假设，且大范围参数下的性能需进一步验证。该工作为实际部署中的阈值 ECDSA 提供了兼具安全与效率的选择。


#### Phecda: Post-Quantum Transparent zkSNARKs from Improved Polynomial Commitment and VOLE-in-the-Head with Application in Publicly Verifiable AES.
- **作者**: Changchang Ding, Yan Huang
- **方向**: 密码学与协议
- **解读**:
随着量子计算威胁逼近，在随机预言机模型下构造抗量子、透明设置（无需可信初始化）的 zkSNARK 成为零知识证明领域的重要目标，这类方案需同时满足可验证性与实际性能。

作者提出 Phecda 框架，结合新型多线性多项式承诺方案与 VOLE-in-the-Head 零知识论证，可验证多种真实世界计算。特别地，作者设计了 AES 验证电路，将其与框架结合，可在单线程 Linux PC 上约 10ms 内验证 1024 个计数器模式 AES 数据块的加密计算。

透明设置、后量子安全与实际性能的结合是 Phecda 的核心卖点，AES 验证电路为常见密码运算提供了直接可用的公开可验证方案。

该工作延续并改进了 MPC-in-the-Head/VOLE-in-the-Head 路线，面向真实负载的性能数据（10ms 验证 1024 块）具有说服力，AES 验证电路可作为公开可验证密码计算的通用构件。局限在于摘要技术细节有限，与同类后量子 zkSNARK 的系统对比、证明大小与内存开销有待原文详读，抗量子性论证也需细看其随机预言机模型的设定。总体上为后量子零知识证明的实用化迈出了扎实一步。


#### Gold OPRF: Post-Quantum Oblivious Power-Residue PRF.
- **作者**: Yibin Yang, Fabrice Benhamouda, Shai Halevi, Hugo Krawczyk, Tal Rabin
- **方向**: 密码学与协议
- **解读**:
不经意伪随机函数（OPRF）是密码协议的重要构件，在密钥管理、隐私集合求交等场景广泛使用；量子威胁下设计可行且高效的后量子 OPRF 是开放问题。

作者基于 Power-Residue PRF（Damgård CRYPTO'88，Legendre PRF 的推广）提出候选后量子 OPRF：Gold_k(x) 将模素数 p=2^λ·g+1 的整数 x 映射到 (k+x)^g mod p（log g≈2λ）。核心是高效的两方计算求值方法 2PC-Gold，基于标准向量不经意线性求值（VOLE）相关，在 (V)OLE 混合模型下是信息论安全的常数轮协议：半诚实服务器+恶意客户端只需单个 VOLE 相关、3 个域元素通信（仅需均匀密钥时 2 个）；双方皆恶意时需 λ/4+O(1) 个 VOLE 相关。作者还用 Beullens 等（Eurocrypt'25）的方法扩展到通用可组合（UC）设置下的强 OPRF。

所有协议均已实现并基准测试：λ=128 时批量求值的半恶意与恶意变体每项分别约 100 字节与 1.9 KB 通信。

把经典 Legendre/Power-Residue PRF 与现代 VOLE 技术结合，构造出通信极紧凑的后量子 OPRF，是该文的主要创新。局限在于其"可行后量子"安全性依赖的新假设尚需密码学界进一步检验，且与基于格或同源的既有 PQ-OPRF 的全面对比有待展开。


#### Benchmarking Attacks on Learning with Errors.
- **作者**: Emily Wenger, Eshika Saxena, Mohamed Malhou, Ellie Thieu, Kristin E. Lauter
- **方向**: 密码学与协议
- **解读**: Kyber 已被 NIST 标准化、全同态加密（HE）库全部依赖 LWE，Signal 等已在部署，但 LWE 具体安全性多靠理论估计，唯一实证基准 Darmstadt Challenge 参数与标准化场景（小秘密、RLWE/MLWE）不匹配。论文提出首个面向标准化参数的 LWE 攻击基准挑战。

固定 n、q 与分布（Kyber：n=256、log2q=12/28/35、MLWE 秩 2/3、二项分布；HE：n=1024、log2q=26/29/50、三进制秘密、高斯误差 σ=3.19），以秘密汉明重量 h 为难度滑杆，统一硬件与归约库评估 uSVP、SALSA、Cool&Cruel、Dual Hybrid MitM 四种攻击；并扩展攻击：ML 的 slope distinguisher 恢复一般分布秘密、CC 的线性回归恢复 cool bits、MLWE cliff-splitting、修正 MitM（τ=50 条短向量足够、以中位数替代 Linf 判据）。

Cool&Cruel 恢复的权重最高且算力最少——Kyber(256,2,12) 下恢复 h=11（28.1 小时），uSVP 跑 1100+ 小时一无所获；MitM 一小时内在 h≤4 上解 Decision-LWE；估计器低估单次攻击时间（预测 0.9 小时 vs 实际 65 小时）却高估所需重复次数；用 C 库 LCG 生成 A 时 flatter 归约显著更强，ML 攻击可恢复 h 高达 90 的秘密。

首次以统一基准公平对比四类攻击并开源（facebookresearch/LWE-benchmarking），工程贡献扎实；但恢复的权重远低于 128 比特安全对应的 h，尚不构成对实际参数的直接威胁。坏 PRNG 弱化 LWE、BKZ 成本模型高维失真等"经验教训"对实现安全与理论估计均有警示。
- **arXiv PDF**: [Benchmarking Attacks on Learning with Errors.](https://arxiv.org/pdf/2408.00882)

#### Post-Quantum Cryptographic Analysis of SSH.

- **作者**: Benjamin Bencina, Benjamin Dowling, Varun Maram, Keita Xagawa

- **方向**: 密码学与协议

- **解读**: SSH 是首批主动升级以抵御未来量子计算机攻击的协议——OpenSSH 自 2022 年 4 月起默认采用"量子安全（否则经典安全）"的混合密钥交换。然而对该抗量子版本的系统安全分析长期缺失：相关工作要么孤立分析组件，要么使用不适合 SSH 的模型，尤其缺乏后量子设定。本文补上这一关键缺口。

方法上，作者采取自顶向下路径：先在更强的扩展 ACCE（认证保密信道建立）模型中证明协议安全，该模型显式刻画"先收割、后解密"威胁，具有独立价值；再基于协议级分析论证 SSH 底层原语在实际实例化下的性质。

分析覆盖实际部署的关键原语，如近期 TinySSH 使用的 Streamlined NTRU Prime 密钥封装机制（KEM），并借助随机预言机回答其文献中的开放问题。值得强调的是，证明仅依赖较弱的 IND-CPA 临时 KEM 假设，与此前依赖更强 IND-CCA 的工作形成对比。论文最后讨论用更简单更快的实现替换现有组件，并给出基准测试。

我的思考：本文的贡献是"模型工程"性质——为 SSH 的后量子混合升级提供首个匹配其安全目标（含收割-解密）的形式化证明，且把假设从 IND-CCA 松到 IND-CPA 是实质进步：既降低实现负担，也说明原设计存在安全余量。局限：证明依赖随机预言机与具体 KEM 实例，模型抽象与实现间仍有缝隙。与 SSH 经典 ACCE 分析相比，本文把 PQC 迁移同步到可证明安全层面，为 OpenSSH 后续演进提供理论依据。


#### SoK: Dlog-Based Distributed Key Generation.

- **作者**: Renas Bacho, Alireza Kavousi

- **方向**: 密码学与协议

- **解读**: 分布式密钥生成（DKG）协议是门限密码学的基石，使多方无需信任单点即可生成密钥，支撑加密、签名等操作，其中基于离散对数（dlog）的 DKG 应用最广。该领域文献体量庞大、设计各异，缺乏系统知识组织。本文以 SoK 形式对 dlog 类 DKG 全面梳理，目标是识别支撑安全协议设计的关键技术与设计原则。

方法上，作者采用模块化分类框架：按协议的底层网络假设（同步/异步、拜占庭容错模型）与所用工具（可验证秘密共享 VSS、共识协议）两个维度归类文献。这两个因素决定协议如何管理秘密共享与达成共识这两个核心构建块，把分散的工作纳入统一坐标系。

基于该框架，论文提炼不同设计选择背后的权衡（通信轮数、鲁棒性、恶意安全等级），归纳若干洞察与未来研究方向，为研究者选择或构造 DKG 协议提供结构化导引。

我的思考：SoK 的价值在于降维——按"网络假设 × 工具"双轴切分，让隐藏的设计谱系显性化，对设计空间爆炸、术语不统一的领域极具实用价值；把安全分析抽象为"秘密共享 + 共识"的共性骨架便于横向比较。局限：难覆盖最新进展与非常规构造，分类粒度可能牺牲细节差异。与门限签名 SoK 相比，本文聚焦密钥生成这一前置阶段，补齐生态位，相当于一张按需选型的决策地图。


#### Verifiable Secret Sharing Simplified.

- **作者**: Sourav Das, Zhuolun Xiang, Alin Tomescu, Alexander Spiegelman, Benny Pinkas, Ling Ren

- **方向**: 密码学与协议

- **解读**: 可验证秘密共享（VSS）是分布式密码学与门限协议（如 DKG、门限签名、MPC）的核心原语，允许分发者把秘密拆分为份额分发给参与者，并保证参与者可验证份额一致性、诚实者最终能恢复秘密。然而经典 VSS 协议（如 Feldman、Pedersen 方案）普遍复杂、开销高、证明繁琐，限制其在现代共识与链上场景的部署。本文（无摘要，据标题推断）以"简化"为目标提出更精简高效的构造。

方法上（据标题与作者背景推断），工作应致力于大幅简化 VSS 的协议流程与安全性论证——可能通过更紧凑的承诺、更少轮次或统一证明框架，在保持恶意安全的前提下降低开销，使 VSS 更易实现与形式化验证，并可直接嵌入 DKG 等上层协议。

作者阵容涵盖分布式系统与密码学知名学者（如 Ling Ren、Benny Pinkas），其历史工作强调可部署性与严谨性结合；据此推断，本工作预计提供可直接使用的简化构造——以更短更清晰的协议与证明达到同等或更强的安全性质，降低工程门槛并减少实现错误面（无摘要，据标题推断）。

我的思考：（基于标题推断，具体结果待原文核实）"简化"类工作看似低调实则价值重大：VSS 的复杂度是分布式系统落地的实际障碍，轮次更少、证明更短、实现更简单的构造能直接降低门槛。若简化不牺牲安全假设，影响力将辐射 DKG、随机信标、门限钱包等下游协议。潜在风险是可能依赖更强设置假设或受限对抗模型，需核对与经典方案的安全性等价性。与追求性能数字的工作相比，本文押注"简洁性即可用性"，值得精读验证。


#### PGUS: Pretty Good User Security for Thick MVNOs with a Novel Sanitizable Blind Signature.
- **作者**: Yang Yang, Quan Shi, Prosanta Gope, Behzad Abdolmaleki, Biplab Sikdar
- **方向**: 密码学与协议
- **解读**: 5G 的兴起凸显了 Thick 移动虚拟网络运营商（MVNO）在提供定制移动服务中的关键作用，但针对 MVNO 的特定安全与隐私挑战仍未得到充分解决。本文提出 PGUS（Pretty Good User Security）框架，为 MVNO 环境提供系统性安全保障。

方法上，PGUS 引入新的密码原语“可净化盲签名”（Sanitizable Blind Signature, SBS），并基于它设计新型认证密钥协商协议 PGUS-AKA；此外还开发了无缝切换协议 PGUS-HO，旨在保护 MVNO 环境内的全部通信；最后在通用可组合（UC）框架下进行严格的形式化分析以应对关键威胁。

结果方面，论文给出了 UC 框架下的安全性证明，并通过测试床上的评估展示了 PGUS 的实际有效性（摘要未提供具体量化数字）。

我认为盲签名与可净化性的结合精准契合 MVNO 场景中“运营商可监管、用户可匿名”的双向需求，UC 形式化证明提升了方案可信度，无缝切换协议也补齐了移动性缺口。局限在于摘要未披露性能与延迟的量化数据，SBS 原语的计算与通信开销在资源受限终端上的成本未知；与既有 5G-AKA 隐私扩展类工作的对比有待展开。


#### Provably Robust and Secure Steganography in Asymmetric Resource Scenario.
- **作者**: Minhao Bai, Jinshuai Yang, Kaiyi Pang, Xin Xu, Zhen Yang, Yongfeng Huang
- **方向**: 密码学与协议
- **解读**: 现有可证明安全隐写要求编码器与解码器运行同一模型、共享相同前缀以获得相同分布，硬件要求高且公开信道前缀易变，实际部署困难。论文提出"非对称资源"场景：仅发送端有强大生成模型，接收端只能读取隐写载体。

编码端用分布排列隐藏比特——按 token ID 二进制前缀构建完全二叉树，把多元分布分解为多层二元分布，由秘密比特决定采样排列；解码端仅凭载体文本与共享随机数构造采样函数 s（采样 a 取 f(r)、采样 b 取 f(1-r)），用 Hoeffding 不等式对 H∅/H0/H1 三假设做检验，比较样本均值与理论均值判定比特；解码完全不依赖模型与前缀，甚至可用 ASCII 替代 tokenizer。

安全性经 G0-G4 游戏序列证明与模型正常输出计算不可区分，载体困惑度/熵与正常文本接近；在 6 个开源 LLM 上容量随温度显著上升（Qwen2 从 T=1.0 的 44.4 bit/千 token 到 T=1.2 的 634.7），解码速率约 0.15 s/千 token，远快于编码；对替换鲁棒：理论保证至少 4.9%（K≈0.69），实验中错误概率 ≤7% 时正确率 99.9%，10% 时仍有 91.7%。

首次给出不依赖模型与前缀、可证明安全且抗替换的隐写构造，理论-实验闭环完整，是隐写领域少见的实用性突破；但容量总体偏低且与温度强耦合，鲁棒性上界随消息长度受限。对审查环境下"接收端零算力"的通信场景具有独特价值。
- **arXiv PDF**: [Provably Robust and Secure Steganography in Asymmetric Resource Scenario.](https://arxiv.org/pdf/2407.13499)

#### P2C2T: Preserving the Privacy of Cross-Chain Transfer.

- **作者**: Panpan Han, Zheng Yan, Laurence T. Yang, Elisa Bertino

- **方向**: 密码学与协议

- **解读**: 基于区块链的数字货币系统长期孤立运行，跨链资产转移仍是复杂挑战，现有方案在安全性、隐私性与实用性上各有短板。本文提出 P2C2T，一个隐私保护的跨链转账方案，据称是首个同时满足原子性、不可链接性、不可区分性与无抵押等要求、并适配异构区块链系统的方案。

方法上，P2C2T 建立在作者提出的阈值匿名原子锁（TA2L）之上——该原语保证跨链原子性并模糊用户间支付关系；结合可验证超时离散对数方案，使跨链交易与普通链内交易不可区分。方案消除了发送方的抵押需求，对底层区块链要求极低，仅需验证签名的能力；安全性通过"盲条件签名"概念及其证明论证。

作者将 P2C2T 与性质最接近的现有方案对比，结果显示其运行时间、通信成本与存储成本至少降低 85.488%；并在比特币测试网与莱特币上实际完成跨链转账，验证了隐私性与实用性。

我的思考：P2C2T 的价值在于把隐私跨链的多项性质首次纳入同一方案——原子性、不可链接性、不可区分性与无抵押在以往工作中往往顾此失彼，抵押要求更是多数跨链方案的实际负担；85.488% 的开销削减与真实测试网验证说明其工程可行性。局限：安全性依赖 TA2L 与盲条件签名等新原语，需更广泛社区审计；85.488% 是与特定基线对比的结果，不同对手方案下结论可能不同。以"交易不可区分性"为核心卖点直击链上可观察性的隐私痛点，是跨链隐私方向上有实质推进的工作。


#### Signature-Free Atomic Broadcast with Optimal $O(n^{2})$ Messages and $O(1)$ Expected Time.

- **作者**: Xiao Sui, Xin Wang, Sisi Duan

- **方向**: 密码学与协议

- **解读**: 拜占庭原子广播（ABC）是许可制区块链与众多多方计算协议的核心构件。ABC 协议通常依赖数字签名保证消息认证，但签名计算与验证带来显著开销；如何构造"免签名"且通信复杂度最优的 ABC 是长期悬而未决的开放问题。本文解决了该问题，给出首个免签名、异步、达到最优 $O(n^2)$ 消息复杂度与 $O(1)$ 期望时间的 ABC 协议，其中 $n$ 为副本总数。

方法上，协议采用新的设计范式：借助一个"出人意料地"被忽视的基础原语——多值协商（multivalued agreement, MBA）——进行规约，将 ABC 问题分解为可通过多值协商解决的子问题，从而在无需签名机制的情况下实现认证与一致性保证。

该协议是首个同时达到最优消息复杂度 $O(n^2)$ 与最优期望时间 $O(1)$ 的免签名异步 ABC 协议，在理论上关闭了该方向的一个长期缺口。

我的思考：异步拜占庭共识的理论下界研究近年进展迅速，本文的贡献在于把"免签名"与"最优复杂度"两个目标同时达成，并通过 MBA 规约而非全新设计实现，展示了经典原语在当代共识理论中的复用价值；免签名特性对许可链的工程意义明显，可省去签名验证的延迟与 CPU 开销。局限：$O(n^2)$ 与 $O(1)$ 是渐进最优，常数因子、实际容错能力（拜占庭副本上限、是否抵抗自适应对手）仍需结合全文评估，摘要也未给出与现有签名方案在真实部署中的性能对比。总体是共识理论领域的实质性进展。


#### Warning! The Timeout T Cannot Protect You From Losing Coins: PipeSwap: Forcing the Timely Release of a Secret for Atomic Cross-Chain Swaps.

- **作者**: Peifang Ni, Anqi Tian, Jing Xu

- **方向**: 密码学与协议

- **解读**: 原子跨链交换缓解了加密货币间的互操作难题，使互不信任的用户可以进行跨币种兑换。尽管大量基于哈希时间锁合约（HTLC）的协议已被部署，其对底层脚本语言的要求使其远离普适性。近期提出的 Universal Swaps 协议（IEEE S&P 2022）通过把功能委托给密码锁定机制（适配器签名与定时承诺）实现无脚本交换。本文发现这类设计存在新型攻击——"双重认领"（double-claiming），可利用时间锁机制以高概率破坏原子性。

方法上，作者先在现有无脚本设计与支付通道网络中演示该攻击，并基于最广泛使用的去中心化平台上的真实交换交易量化其严重性；随后提出 PipeSwap 方案：通过新颖的流水线化流程（pipelined flow）范式与两跳退款（two-hop refund）技术，保护被冻结的币不被双重认领，同时兼顾安全性与实践普适性。

作者在通用可组合（UC）框架下完成全面安全分析，并基于 Schnorr/ECDSA 签名实现概念验证；实验表明 PipeSwap 在普通机器上完成交换耗时小于 1.7 秒，通信开销约 7KB。

我的思考：本文展示了"协议设计缺陷在真实数据下有多严重"的完整案例——时间锁参数 T 看似保护双方，实则被双重认领攻击利用，且作者用真实链上交易数据量化了风险的现实性，说服力强。PipeSwap 的流水线化与两跳退款是对"如何强制按时释放秘密"这一难题的工程化回答，1.7 秒与 7KB 的开销表明其实用性。局限：攻击的高概率表述依赖具体链上参数分布，不同链结论可能不同；UC 框架下的证明强度需社区审计。兼具攻击发现与防御构造，是跨链安全研究中攻防闭环的范例。


#### Papercraft: Lattice-Based Verifiable Delay Function Implemented.

- **作者**: Michal Osadnik, Darya Kaviani, Valerio Cini, Russell W. F. Lai, Giulio Malavolta

- **方向**: 密码学与协议

- **解读**: 可验证延迟函数（VDF）要求计算需要指定数量的顺序步骤，但输出有效性可被远快于重算的方式验证；VDF 应用于区块链共识、抽奖与随机数生成等工业场景。然而，已知实用 VDF 构造无一例外都被量子算法攻破。本文研究实用化后量子安全 VDF 的可能性，提出 Papercraft——一个完全基于格（lattice）技术、因此具有后量子安全合理性的可工作实现。

方法上，Papercraft 结合了格基简洁论证系统（succinct argument systems）的新观察与底层优化，构建了首个在今天硬件上可实现的格基 VDF，在保证顺序性的同时压缩验证成本。

示例数据表明，Papercraft 能在约 7 秒内验证一段超过 6 分钟的计算输出。总体上，该工作证明格基 VDF 并非纯理论构造，为其实际部署铺平了道路。

我的思考：后量子安全是密码学迁移的主线，而 VDF 恰是"全被量子攻破"的重灾区——经典实用构造依赖不可分解的群运算，量子算法可显著加速。Papercraft 首次给出可实现的格基替代，验证端把 6 分钟计算压缩到 7 秒的对比直观展示了可验证性的价值。局限：格基 VDF 的"顺序性"论证通常比经典方案更微妙，需严格归约支撑；7 秒验证时间对区块链场景可能偏高，证明大小与生成端开销需结合全文评估。与基于类群的经典方案相比，抗量子性增益以效率损失为代价，实际部署取决于量子威胁的时间表。总体是后量子密码工程的前沿推进。


#### Constant Latency and Finality for Dynamically Available DAG.
- **作者**: Hans Schmiedel, Runchao Han, Qiang Tang, Ron Steinfeld, Jiangshan Yu
- **方向**: 密码学与协议
- **解读**: 基于有向无环图（DAG）的协议有望显著提升区块链性能，但 CAP 定理表明单一系统无法同时实现活性（即动态可用性）与网络分区下的安全性。本文系统探索两类 DAG 协议：优先活性（结构化传播）与优先安全（分级公共前缀 GCP, Graded Common Prefix）。

对于优先活性的协议，作者提出首个具有常数期望延迟的协议，在沉睡模型（sleepy model）下提供高吞吐与动态可用性，其延迟为 3Δ，且随参与规模线性扩展。作者通过在多台机器上运行原型验证了其相比现有 BFT 模型的改进；GCP 则作为在分区下提供安全性的原语，弱于标准共识但提供了有意义的安全保证。

量化结果包括：常数期望延迟 3Δ（Δ 为消息延迟上界），以及多机原型验证相对现有模型的延迟改进。

我认为"常数延迟 + 动态可用性"是 DAG 共识理论的重要推进，3Δ 的界在理论与实践之间架起桥梁；GCP 原语的定位（介于标准共识与无保证之间）为分区场景提供了实用折中。后续关注点在于原型验证的规模、真实网络条件下的延迟表现，以及两种协议之间的权衡边界如何随网络条件迁移。


#### Sailfish: Towards Improving the Latency of DAG-Based BFT.
- **作者**: Nibesh Shrestha, Rohan Shrothrium, Aniket Kate, Kartik Nayak
- **方向**: 密码学与协议
- **解读**: 基于 DAG 的 BFT 协议在多方之间平衡共识工作量，并在部分指定节点故障时保持高吞吐，但现有 DAG 型 BFT 提交决策的延迟较长，主要原因是每 2 轮或更多轮才有一个 leader。近期工作（如 Shoal, FC'23 与 Mysticeti）认为在每一轮支持 leader 顶点尤为困难甚至不可能；因此诚实 leader 下仍需额外的通信/复杂度来验证非 leader 顶点提交的提议。

本文提出 Sailfish，这是首个支持每一轮都有一个 leader 顶点的 DAG 型 BFT 协议。在诚实 leader 下，Sailfish 仅需一次可靠广播（RBC）加一次广播即可完成提交，延迟为一个 δ，其中 δ 为实际网络传输延迟。

量化结果：诚实 leader 场景下提交延迟降至单个网络延迟 δ，通信仅为一次 RBC 加一次广播，显著优于此前每两轮或更多轮才提交的方案。

我认为每轮 leader 顶点是 DAG BFT 延迟优化的关键突破，Sailfish 把每轮提交延迟压缩到单个网络往返，理论意义明确。与 Mysticeti、Shoal 的对比需要实现层面的公平评估，尤其要考察吞吐与延迟的权衡、拜占庭故障下的表现以及真实网络延迟波动对 δ 的影响。


#### Cauchyproofs: Batch-Updatable Vector Commitment with Easy Aggregation and Application to Stateless Blockchains.
- **作者**: Zhongtang Luo, Yanxue Jia, Alejandra Victoria Ospina Gracia, Aniket Kate
- **方向**: 密码学与协议
- **解读**: 无状态区块链设计旨在用简洁的全局状态应对状态库不断增长的挑战，此前的工作开发了支持证明更新与聚合的向量承诺以支撑该设计。然而，为多个用户维护证明仍然需要大量计算资源，尤其是每次交易都要更新证明。本文提出 Cauchyproofs：一种批量可更新的向量承诺，使证明服务节点能够在关于交易数的拟线性时间内高效地更新证明。

方法上，Cauchyproofs 利用优化的 KZG 方案实现批量更新，达到复杂度 o((|α|+|β|)·log²(|β|))，其中 |α| 为用户数、|β| 为交易数，对比此前方案 O(|α|·|β|) 的线性依赖（摘要截断）有实质改进，且易于证明聚合。

量化结果：批量更新复杂度为 o((|α|+|β|)log²(|β|))，将每笔交易更新从与用户数线性相关降低为拟线性，显著提升证明服务吞吐。

我认为把证明更新从 O(用户数×交易数) 降到拟线性是实质性的效率跃迁，直接改善无状态区块链证明服务端的可扩展性。基于 KZG 的优化实现需要验证常数因子与配对运算开销是否在实际部署中可控；批量更新的语义对交易排序、并发与跨 epoch 状态切换的适配是工程化的关键环节。


#### MicroNova: Folding-Based Arguments with Efficient (On-Chain) Verification.
- **作者**: Jiaxing Zhao, Srinath T. V. Setty, Weidong Cui, Greg Zaverucha
- **方向**: 密码学与协议
- **解读**: 本文描述 MicroNova 的设计与实现：一个基于折叠（folding）的递归论证，用于为增量计算 y=F^ℓ(x) 逐步生成证明，其中 F 是可能非确定性的计算（用 R1CS 等约束系统编码），x 为初始输入，y 为输出，ℓ>0。证明逐步生成，其大小与验证时间均不依赖步数 ℓ；在最终迭代对证明进行压缩，以在验证时间上进一步实现简洁性。

与先前的论证方案相比，MicroNova 的突出特点是验证器的具体效率——即使在以太坊区块链这类资源受限环境中也足够高效。具体而言，压缩后的证明由 O(log N) 个群元素组成，验证只需标量乘法与两次配对运算。

量化结果：压缩证明含 O(log N) 个群元素，链上验证成本仅为标量乘加两次配对，验证开销与计算步数无关。

我认为 MicroNova 把折叠论证的验证成本压到常数级配对运算，对链上验证场景极具实用价值，工程导向明确。需要进一步评估的是证明生成端的开销、与 Nova/SuperNova 家族方案的对比定位，以及 O(log N) 群元素的常数因子在实际 Gas 成本中的表现。


#### Permissionless Verifiable Information Dispersal (Data Availability for Bitcoin Rollups).
- **作者**: Ben Fisch, Arthur Lazzaretti, Zeyu Liu, Lei Yang
- **方向**: 密码学与协议
- **解读**: Rollup 是分布式状态机（即区块链）上的特殊应用：底层链只记录而不执行交易。Rollup 借助执行成本更高但吞吐更高的辅助网络来扩展规模；状态更新周期性地发布到链上，要么通过简洁密码学证明直接验证（zk rollup），要么在定义好的挑战期内由第三方以可验证方式质疑（optimistic rollup）。然而，一旦计算被简化，通信很快成为新的瓶颈。

除验证外，rollup 提供的关键服务是数据可用性：数据必须总能按请求恢复。广播要求每个参与者承担线性大小的通信量。本文提出无许可的可验证信息分散（VVID）方案，为比特币 rollup 提供数据可用性保障（摘要未给出具体机制与数字）。

摘要未提供量化结果，核心贡献是提出面向比特币生态的无许可 VVID 方案，解决 rollup 数据可用性这一通信瓶颈。

我认为比特币没有原生智能合约与数据可用性层，VVID 精准填补了这一空白。无许可（permissionless）意味着参与方不可预知，Sybil 抵抗与恶意参与方容忍是安全模型的核心难点；与以太坊 Danksharding 思路的对比、重建复杂度与诚实假设将决定该方案的实际可用性。


#### An Attack on TON's ADNL Secure Channel Protocol.
- **作者**: Aviv Frenkel, Dmitry Kogan
- **方向**: 密码学与协议
- **解读**: 本文对 The Open Network (TON) 中使用的抽象数据报网络层（ADNL）协议（TCP 变体）发起攻击，TON 在当时为市值第 10 大的区块链。ADNL 保护客户端与名为 liteserver 的专用节点之间的通信，后者提供数据访问服务。作者识别出该协议的两个密码学设计缺陷。

缺陷包括：一个允许会话密钥重放的握手机制，以及一个安全性关键依赖于消息机密性的非标准完整性机制。作者将这些漏洞转化为高效的明文恢复攻击，利用重放会话间的模式进行重排；随后为特定场景建立明文模型，构造出仅利用少量已知明文与数次重放即可恢复密钥流的算法，并完成了实现。

摘要未给出量化结果，核心贡献是对真实区块链协议实施完整的端到端密码分析，实现明文恢复。

我认为针对真实部署协议的密码分析价值很高——非标准原语的随意组合再次被证明是危险的。重放漏洞与"完整性依赖机密性"的设计缺陷揭示了自行设计密码机制的典型陷阱，与历史上同类协议攻击的教训一致。后续关注点包括披露流程、TON 的修复响应，以及该攻击对 ADNL UDP 变体等其他版本的影响范围。


#### Vitārit: Paying for Threshold Services on Bitcoin and Friends.
- **作者**: Sri Aravinda Krishnan Thyagarajan, Easwar Vivek Mangipudi, Lucjan Hanzlik, Aniket Kate, Pratyay Mukherjee
- **方向**: 密码学与协议
- **解读**: 区块链服务近年来快速兴起，许多服务采用门限敌手（threshold adversary）架构以实现去中心化，避免单点故障并缓解密钥托管问题。在支持智能合约的系统中，此类支付直截了当；但在比特币这类使用 UTXO 模型、脚本能力有限的系统中实现公平性颇具挑战——尤其是在没有可信第三方的情况下，用户只愿为 n 个服务器中所需的 t+1 个服务付费，同时任何服务器都不得重复索取付款。

本文提出 Vitārit（梵语"分布式"），一个为比特币量身定制的新颖解决方案，在保证健壮、可证明安全性的同时便于实际部署。

摘要未给出量化结果，核心贡献是在比特币有限的脚本能力约束下实现门限服务的公平付费机制。

我认为比特币脚本限制下的门限支付公平性是现实需求，直接服务于门限签名服务、预言机集群等去中心化服务收费场景。关键看点在于如何防止"服务方先收费不服务"与"用户不付款"的双向不公平，方案是否依赖复杂脚本或适配器签名等构造，以及与以太坊智能合约方案的成本对比；UTXO 模型的固有约束使这一设计充满张力。


#### "Check-Before-you-Solve": Verifiable Time-Lock Puzzles.
- **作者**: Jiajun Xin, Dimitrios Papadopoulos
- **方向**: 密码学与协议
- **解读**: 时间锁谜题是保证生成者的谜题在少于 T 步顺序计算内无法被解开的密码学原语，近期在公平合约签署、密封拍卖等领域获得广泛应用。然而，求解者在投入计算之前，无法预先知道将要揭示的解在其应用场景中是否"有用"。本文提出可验证时间锁谜题（VTLP）解决这一问题：生成者发布一个关于解满足某些性质的简洁证明，且不泄露任何其他信息，从而激励求解者"投入"资源求解。

VTLP 支持证明关于解满足任意 NP 关系 R 的性质。在技术层面，作者克服了"朴素方法"（即用同时检查关系 R 的 SNARK 直接构造）的性能瓶颈。

摘要未给出量化结果，核心贡献是首次形式化可验证时间锁谜题，使求解激励成为可能。

我认为 VTLP 精准解决了时间锁谜题从理论原语走向实用组件的"求解激励"痛点。关键评估维度是证明生成的额外开销相对于 T 步求解成本是否可忽略，以及能否与现有 VDF、时间锁构造自然结合；此外，其与"可验证延迟函数"等相近概念的边界与差异值得厘清，以避免命名与语义上的混淆。


#### Opera: Achieving Secure and High-Performance OLAP with Parallelized Homomorphic Comparisons.
- **作者**: Qi Hu, Wei Chen, Tianxiang Shen, Xin Yao, Nicholas Zhang, Heming Cui, Siu-Ming Yiu
- **方向**: 密码学与协议
- **解读**: 全同态加密（FHE）被用于在线分析处理（OLAP）系统以防数据泄露，但现有 FHE-OLAP 必须串行执行计算密集的同态比较，查询性能远低于传统明文系统，成为落地的主要障碍。

方法上，作者提出 Opera，首个基于 GPU 的高性能 FHE OLAP 系统：观察到"从零重算"的执行冗余后设计 Homcache，选择性缓存比较结果供后续查询复用，构建 GPU 加速的并行执行流程；针对密文缓存体积膨胀问题，提出面向密文的密度驱动缓存管理算法，取代朴素 LRU。

结果显示，相比运行于 CPU 的显著基线，Opera 将查询延迟最多降低 9612 倍，仅需 1.2GB 缓存存储，且不牺牲安全性；源码、完整基准与原始数据已在 GitHub 开源。

我认为 Opera 把"结果复用+密度感知缓存"的经典思想引入 FHE 查询执行，用工程化手段将性能差距缩小近四个数量级，实用意义突出。但 9612 倍是与 CPU 基线的对比，GPU/CPU 对比的公平性与缓存命中率对查询混合模式的敏感性值得细究；同态比较结果复用是否引入跨查询信息泄露也需形式化论证。总体而言，它代表了 FHE 系统从"能算"走向"算得快"的务实方向。


#### DataSeal: Ensuring the Verifiability of Private Computation on Encrypted Data.
- **作者**: Muhammad Husni Santriaji, Jiaqi Xue, Yancheng Zhang, Qian Lou, Yan Solihin
- **方向**: 密码学与协议
- **解读**: 全同态加密（FHE）保证了数据机密性，却不保证计算完整性与可验证性：恶意或受损的服务器可能篡改数据、计算与结果而不被发现。已有方案（同态MAC、零知识证明ZKP、可信执行环境TEE）要么开销巨大、要么不能独立提供可验证性，FHE可验证计算的低开销方案仍是开放问题。

DataSeal将算法级容错（ABFT）与FHE结合：用客户端私钥向量生成加权校验和与"golden input/output"作为冗余，无论矩阵多大都只增加两行，空间开销2/N渐近为零；校验密钥与随机标量由PRF每会话生成以防重放，加密后的哈希使攻击者无法伪造通过校验的篡改结果，golden output还堵住了整矩阵常数缩放攻击。客户端解密后仅需两次向量-矩阵乘法即可完成验证。

实验表明：矩阵乘法最终开销仅比纯FHE高2.99%，矩阵加法1.85%，卷积云端开销0.23%，平方激活在64×64时61%；相比之下FHE-MAC在AlexNet/CIFAR-10单次推理慢8.51倍，FHE-TEE因双重加密在加法上开销达59.18%，且这些方案开销随问题规模增大而上升，DataSeal则不断下降。

我的思考：以近乎可忽略的开销获得可验证性是亮点，安全性高于半诚实的朴素ABFT；但局限明显——仅支持矩阵类运算、每次只能验证单个操作（如A×B+C需重新编码）、多层网络需逐层验证，且安全证明依赖标准密码学假设，面向更复杂计算的可扩展性仍有待探索。

- **arXiv PDF**: [DataSeal: Ensuring the Verifiability of Private Computation on Encrypted Data.](https://arxiv.org/pdf/2410.15215)

#### CHLOE: Loop Transformation over Fully Homomorphic Encryption via Multi-Level Vectorization and Control-Path Reduction.
- **作者**: Song Bian, Zian Zhao, Ruiyu Shen, Zhou Zhang, Ran Mao, Dawei Li, Yizhong Liu, Masaki Waga, Kohei Suenaga, Zhenyu Guan, Jiafeng Hua, Yier Jin, Jianwei Liu
- **方向**: 密码学与协议
- **解读**: 当循环作用于密文时，控制流难以有效解释、循环体算子成本模型难以构建，导致多数 FHE 编译框架对含非平凡循环的程序支持不足，严重限制了 FHE 编程的表达力与通用性。

方法上，作者提出多层级编译框架 CHLOE：按循环条件是否加密，把 FHE 循环分为透明循环与不透明循环两类；对透明循环直接检查并应用细粒度的 FHE 特定分段与向量化，对不透明循环用静态技术推导闭式表达式以减少条件分支的潜在路径数，并设计新的控制流分析以优化复合重复结构。

结果显示，CHLOE 能够编译含复杂循环的可执行代码，性能相比最先进编译器提升 1.5 至 54 倍，含循环程序最高达 105 倍。

我认为 CHLOE 直击 FHE 编译器"循环支持缺失"的表达力短板，用向量化、路径缩减与闭式推导等编译变换在语言层解决性能问题，提升幅度可观。但其增益高度依赖循环结构类型，不透明循环的通用性仍受限于条件可否闭式化；端到端可用性（生成代码正确性验证、调试支持）摘要未涉及。总体而言，这是 FHE 系统软件栈走向"通用程序执行"的重要一步。


#### Improved Constructions for Distributed Multi-Point Functions.
- **作者**: Elette Boyle, Niv Gilboa, Matan Hamilis, Yuval Ishai, Yaxin Tu
- **方向**: 密码学与协议
- **解读**: 分布式点函数（DPF）用于在两方间压缩单位向量的加性秘密分享；许多应用需要压缩权重为 t 的稀疏向量，即分布式多点函数（DMPF），但此前多数场景的最佳实践仍是 t 个独立 DPF 的暴力组合，效率低下。

方法上，作者针对不同参数域提出新的 DMPF 构造并给出优化实现，提供相比现有方法显著的效率节省；并将这些工具集成到伪随机相关生成器（PCG）与两服务器隐私集合交集（PSI）等典型应用中。

结果显示，集成进最新的 PCG"静默"生成二元乘法三元组方案（FOLEAGE, Bombar 等, ePrint'24）后，吞吐提升 2.68 倍，种子规模仅膨胀 1.4 倍；在单核基准机上静默生成速度达每秒 2210 万个三元组，甚至超过非静默协议（Roy, CRYPTO'22）。

我认为该工作把 DMPF 从"理论上有动机、实践上仍是暴力组合"推进到可部署的效率水平，2.68 倍吞吐与 1.4 倍种子膨胀的权衡相当划算，静默生成反超非静默方案说明其综合优势。但改进幅度与参数域（t 的大小、域比特长度）强相关，实际收益需按场景评估；安全性依赖 DPF/DMPF 的底层假设与 PCG 的松弛正确性论证，工程实现的常数因子与内存占用还需细读正文验证。


#### Preprocessing for Life: Dishonest-Majority MPC with a Trusted or Untrusted Dealer.
- **作者**: Elette Boyle, Niv Gilboa, Matan Hamilis, Yuval Ishai, Ariel Nof
- **方向**: 密码学与协议
- **解读**: 安全多方计算（MPC）的预处理模型追求"一次性可行的设置支撑终身高效率在线计算"；最便宜的一类是三方抵御单个恶意方的 3PC，但现有方案在存储与通信上存在明显开销，阻碍范式落地。

方法上，作者提出新的预处理范式：一方只需发送电路规模次线性的数据、在发出初始消息后即可离线，形成"2+1"方结构，也可实例化为两方加一个（不可信）经销商。技术核心基于 Boyle 等人（CRYPTO 2021）的 FLIOP 协议，辅以大量算法与实现级优化，使复杂 FLIOP 相关性的分布式正确性证明与电路无关。

结果显示，与现有协议相比性能相当，但存储与经销商到方的通信需求改善超过 3 个数量级；端到端系统在 2+1 范式（含 SPDZ 布尔电路的经销商辅助变体）中完成基准测试，并将技术扩展到 (n+1) 方一般恶意多数 MPC，额外实现"可识别中止"。

我认为"预处理一次、受用终身"的提法颇具诱惑力，3 个数量级的通信/存储改善是把该范式推向实用的关键，FLIOP 证明与电路无关的设计是优雅的系统工程。但经销商模式下"不可信经销商"的安全假设细节（作恶检测、中止语义）决定真实保障，预处理长期有效性及相关性安全刷新机制摘要未展开；与诚实多数方案的取舍、在线阶段轮数等仍需正文细究。


#### MatriGear: Accelerating Authenticated Matrix Triple Generation with Scalable Prime Fields via Optimized HE Packing.
- **作者**: Hyunho Cha, Intak Hwang, Seonhong Min, Jinyeong Seo, Yongsoo Song
- **方向**: 密码学与协议
- **解读**: SPDZ 族协议是恶意多数设置下对抗主动敌手的流行 MPC 选择，其离线阶段生成认证三元组是性能瓶颈；机器学习中的矩阵运算与分布式 RSA 密钥生成等新需求，要求离线阶段支持矩阵三元组与更大的素域（最高 4096 位）。

方法上，作者提出 MatriGear，改进此前最先进构造 TopGear（Baum 等, SAC'19）及其变体（Chen, Asiacrypt'20）；为支撑更大素域，设计了针对该场景优化的 BFV 同态乘法算法与打包策略。

结果显示，生成 1024 位标量域三元组加速约 3.6 倍，128×128 矩阵三元组加速 34 倍；评估密钥从 27.4GB 降至 0.22GB，MAC 通信成本从 816MB 降至 16.6MB。

我认为 MatriGear 在吞吐（34 倍）与存储（评估密钥缩小两个数量级）两个维度都给出了硬指标，把 SPDZ 离线阶段推向矩阵运算与 RSA 生成的实用区间，方向对应用需求敏感。但标量 3.6 倍与矩阵 34 倍的差距说明加速主要来自矩阵打包，标量场景收益有限；4096 位大素域下的端到端实际开销、与 TopGear 的完整对比细节（通信轮数、对在线阶段的影响）摘要未完全展开，需结合实现验证。


#### SHARK: Actively Secure Inference Using Function Secret Sharing.
- **作者**: Kanav Gupta, Nishanth Chandran, Divya Gupta, Jonathan Katz, Rahul Sharma
- **方向**: 密码学与协议
- **解读**: 预处理模型下的主动安全两方机器学习推理，此前最优方案是 Escudero 等人（Crypto 2020）的工作，但其需要大量随机性、大量通信与多轮交互，导致性能很差，难以实用。

方法上，作者基于函数秘密共享（FSS）构造新的主动安全协议：首次在 FSS 协议中混合使用布尔值与算术值，并引入"交互式 FSS"这一 FSS 的推广作为关键构件，从而在随机性、轮数、通信与计算上全面占优。

结果显示，作者构建了首个此类推理系统 SHARK，在全部参数上优于 Escudero 等人的方案：随机性更少、轮数更少、通信与计算更低，性能提升最高达 2300 倍。

我认为 SHARK 的可贵之处在于理论构造与系统落地并重——2300 倍的跨越来自"混合布尔/算术值"与"交互式 FSS"两个机制性创新，而非单纯工程调优，并首次证明 FSS 路线在主动安全下可行。但 2300 倍对应的具体配置（网络、模型、安全参数）摘要未给出，增益可能对场景敏感；交互式 FSS 的安全证明与实现复杂度是落地门槛，与基于不经意传输或同态路线的系统化对比也待正文补齐。


#### Rushing at SPDZ: On the Practical Security of Malicious MPC Implementations.
- **作者**: Alexander Kyster, Frederik Huss Nielsen, Sabine Oechsner, Peter Scholl
- **方向**: 密码学与协议
- **解读**: MPC 虽有长足进展并吸引产业兴趣，但开源实现仍处于早期，缺少生产级代码，对其实际安全保证的理解不足；针对恶意敌手的 SPDZ 协议（Damgård 等, CRYPTO 2012/ESORICS 2013）的实现也不例外。

方法上，作者研究真实世界的现代实现（MP-SPDZ、SCALE-MAMBA、FRESCO），识别出 SPDZ 检查阶段一种新型 MAC 密钥泄露：该泄露可在并发、多线程环境下被利用，危及输出完整性，某些情况下还危及输入隐私。

结果显示，三个实现中有两个易受该攻击，同时还发现所有实现存在进一步的问题与漏洞；作者据此提出面向研究者、开发者与用户的缓解策略与建议，希望提高认识、避免未来重现。

我认为该工作的价值在于把"协议安全"与"实现安全"之间的鸿沟暴露出来——理论上可证明的恶意安全在并发工程实现中可能瓦解，对 MPC 产业化是及时警示。并发/多线程下的 MAC 密钥泄露表明攻击面来自实现细节而非协议本身；但摘要未给出攻击的完整利用链、影响规模与缓解策略的有效性验证。这类实现审计工作与形式化验证、安全编译相结合，才是 MPC 走向生产环境的基础。


#### Is MPC Secure? Leveraging Neural Network Classifiers to Detect Data Leakage Vulnerabilities in MPC Implementations.
- **作者**: Guopeng Lin, Xiaoning Du, Lushan Song, Weili Han, Jin Tan, Junming Ma, Wenjing Fang, Lei Wang
- **方向**: 密码学与协议
- **解读**: 
在 GDPR 等隐私法规推动下，多方计算（MPC）协议被企业机构广泛用于在保护隐私的前提下进行联合分析与机器学习，但协议实现常含数据泄漏漏洞，且现有安全性验证多依赖理论证明而忽视实现层面的漏洞检测。

方法上，提出实用框架 MPCGuard：建立漏洞标识符并配合两个神经网络分类器判断实现是否含漏洞，按 MPC 协议特征设计分类器结构以提升识别效果；识别出漏洞后采用 delta 方法辅助定位漏洞位置。

结果上，将 MPCGuard 应用于 CrypTen、TF-Encrypted、MP-SPDZ 三个主流框架的 29 个常见实现，发现其中 12 个存在可导致原始数据重构的漏洞，截至写作时均已分配 CVE 编号。

我的思考：这是首批用神经网络分类器系统检测 MPC 实现泄漏的工作，12/29 的高漏洞率令人警醒，有力说明"协议安全不等于实现安全"；局限在于依赖特定框架实现与分类器训练数据，通用性待验证。对隐私计算产业界是一次重要的安全预警，将推动实现层面安全测试的落地。


#### Comet: Accelerating Private Inference for Large Language Model by Predicting Activation Sparsity.
- **作者**: Guang Yan, Yuhui Zhang, Zimu Guo, Lutan Zhao, Xiaojun Chen, Chen Wang, Wenhao Wang, Dan Meng, Rui Hou
- **方向**: 密码学与协议
- **解读**: MPC私有推理中服务器间频繁通信是主要瓶颈：OPT-6.7B通信时间占85%以上，Llama2-7B生成16个token需60分钟以上，且此前优化多聚焦非线性层，线性层通信被忽视。Comet利用LLM普遍的激活稀疏性（OPT/Llama2的FFN中ReLU输出90%以上为零、MHA中半数以上注意力头未激活）跳过冗余计算与通信。

系统包含三部分：轻量低秩神经网络预测器预测稀疏分布（开销不超过端到端15%），配不经意洗牌使明文索引不泄露稀疏位置（在线仅1轮通信、成本2n，比传统安全索引降低约1000倍）；利用稀疏分布空间局部性的SOMM/SIMM稀疏矩阵乘协议，把同行列的点积分组为块矩阵乘，每行/列只掩蔽通信一次，经二部图连通分量划分证明通信与计算双最小；KV缓存管理器用合并缺失请求与成本收益预取缓解稀疏跳过破坏缓存连续性。

实验对比6个SOTA系统（Crypten、Iron、Bolt、MPCFormer、SecFormer、Puma）：端到端加速1.87-2.63倍、通信减少1.94-2.64倍；模型越大收益越大（Llama2-7B平均2.58倍），FFN各层加速6-10倍、通信最高减少约12倍；精度平均仅损失1.5%（预测器召回93%）。

我的思考：首个把稀疏预测引入私有LLM推理的系统，与协议级优化正交可叠加；威胁模型为半诚实且允许暴露稀疏度（位置由洗牌保护，稀疏度可用MPC-DP加噪缓解）；预测器开销与阈值权衡（阈值0.7时精度降至49.3%）提示精度-速度平衡需按应用谨慎设定。
- **arXiv PDF**: [Comet: Accelerating Private Inference for Large Language Model by Predicting Activation Sparsity.](https://arxiv.org/pdf/2505.07239)

#### Highly Efficient Actively Secure Two-Party Computation with One-Bit Advantage Bound.
- **作者**: Yi Liu, Junzuo Lai, Peng Yang, Qi Wang, Anjia Yang, Siu-Ming Yiu, Jian Weng
- **方向**: 密码学与协议
- **解读**: 
安全两方计算（2PC）允许双方在保护输入隐私的前提下联合计算，但主动安全与被动安全协议之间仍存在显著效率差距；Huang、Katz、Evans 在 S&P'12 提出"单比特泄漏"的主动安全概念以弥合差距，但其衍生协议在独立使用（而非作为大协议组件）时对诚实方的公平性存在弱点，作者提出的缓解方案昂贵且缺乏形式化保证。

方法上，本文正式定义增强概念"单比特优势界"（one-bit-advantage bound），将敌手优势严格限制为最多比其应得超出 1 比特；通过渐进揭示机制逐比特披露评估结果来强制执行该界，并利用带标签结构的混淆电路设计达到该界的常量轮协议。

结果显示，协议运行时与混淆电路基线几乎相同（LAN 上 SHA256 电路仅 1.033×），输出开销极低（每次发布仅 80 字节通信）。

我的思考：该工作以几乎可忽略的性能代价补上独立使用场景的公平性短板，形式化定义清晰（优势界+渐进揭示），实用价值高；局限在于"单比特泄漏"语义对某些应用仍需审慎评估其可接受性，且实验电路规模有限，更大规模应用的验证值得期待。


#### Hermes: Efficient and Secure Multi-Writer Encrypted Database.
- **作者**: Tung Le, Thang Hoang
- **方向**: 密码学与协议
- **解读**: 
可搜索加密（SE）支持在加密数据上进行隐私保护的关键词搜索，其中公钥 SE（PKSE）支持多用户搜索但公钥运算延迟高，对称 SE（SSE）亚线性高效但限于单用户；混合方案（HSE）结合两者优点，却仍易受字典攻击，访问控制验证需对所有授权关键词做昂贵的配对运算，且组件需周期性重建带来可观写者开销。

方法上，提出新方案 Hermes：开发身份基隐藏身份密钥聚合等新性质（可能具有独立研究价值），并设计新颖的分区与纪元编码技术，在保持最小复杂度和用户效率的同时提供前向隐私、抗字典攻击等安全保证。

实验表明，在商品硬件上与现有方案对比，Hermes 快一至两个数量级，同时提供更强的防注入等安全保证。

我的思考：该工作一次性回应 HSE 的字典攻击、配对开销与重建开销三大痛点，量级性能提升显著，且核心密码学构件可独立复用；局限是尚未在真实多写者部署场景中充分验证。对加密数据库的实际落地是重要进展，也为后续混合可搜索加密研究提供了新构件。


#### Towards Efficient and Practical Multi-party Computation under Inconsistent Trust in TEEs.
- **作者**: Xuanwei Hu, Rujia Li, Yi Liu, Qi Wang
- **方向**: 密码学与协议
- **解读**: 
安全多方计算（MPC）可在保证隐私与正确性的前提下对敏感数据联合计算，TEE 辅助的 MPC 协议能大幅降低昂贵密码学开销，但现有方案要么假设所有参与方对 TEE 一致信任，要么为特定应用专门设计，阻碍了实际部署。

方法上，提出不假设一致信任的通用协议，充分利用异构 TEE 提升效率；建立刻画各方不一致信任的安全模型，并在 UC 框架的简化变体（SUC 框架）下给出安全性证明；并以基于 SOTA 信息论方案 SwiftAgg+ 的安全聚合为实例进行实例化。

评估结果显示，在 64 方 Azure 虚拟机环境中，相比 SwiftAgg+ 运行时间降低 66%，通信量最多减少 91%。

我的思考："不一致信任"的建模更贴近真实部署（各方对 TEE 厂商的信任程度往往不同），通用协议设计填补了该场景空白，SUC 框架简化了证明负担；局限是评估规模为 64 方且仅一个实例化，更多应用场景的泛化能力有待验证。对联邦学习等大规模隐私聚合场景有直接应用前景。


#### Smaug: Modular Augmentation of LLVM for MPC.
- **作者**: Radhika Garg, Xiao Wang
- **方向**: 密码学与协议
- **解读**: 
MPC 是隐私保护计算的关键工具，但近年来协议优化使其日益复杂；MPC 编程工具本可让程序员无需精通密码学即可开发应用，然而多数现有工具因缺乏文档、维护不善、难以与遗留代码组合而难以吸引真实用户。

方法上，构建 Smaug——LLVM 的模块化扩展：为 MPC 程序员无缝带来 LLVM 生态支持，包括错误信息、代码优化、以及将多种语言前端编译为中间表示（IR）；可高效地将非 oblivious IR 转换为 oblivious 版本，并同时应用流行优化作为变换。

结果显示，在 C++/Rust 编写的基准上、以 Yao 与 GMW 协议为后端，Smaug 性能达到（部分情况下明显优于）使用类似后端的领域特定工具；并基于开源项目实现了 Minesweeper 与 Blackjack 两款可玩的双方游戏。

我的思考：站在 LLVM 生态肩膀上解决 MPC 编程的可用性与组合性问题，多语言前端与成熟优化管线是杀手锏，方向务实；关键挑战在于 oblivious 化转换的正确性，其安全保证应有形式化支撑。该工作为 MPC 工程化与普及铺平了道路。


#### Efficient Proofs of Possession for Legacy Signatures.
- **作者**: Anna P. Y. Woo; Alex Ozdemir; Chad Sharp; Thomas Pornin; Paul Grubbs
- **方向**: 密码学与协议
- **解读**: 数字签名支撑着现代系统的身份、真实性与信任。密码学研究早已证明可以"证明持有某公钥下合法消息-签名对"而不泄露消息或签名，但这类证明此前只适用于特制方案，对广泛部署的遗留方案（如 RSA、ECDSA、Ed25519）不可用，巨大的效率鸿沟阻碍了其实际应用。

设计策略是将签名验证算法编码为秩一约束系统（R1CS），再用 zkSNARK 证明其解的存在。为实现高效，作者：（1）分析并引入支持随机化计算的新型证明系统 Dorian；（2）提出编码哈希、椭圆曲线运算与模运算的多项技术；（3）给出把 ECDSA/Ed25519 验证中最昂贵的部分移出 R1CS 的方法；（4）生成新的高效约束表达。

效果上，R1CS 规模最高缩小 200 倍，证明者时间缩短超过 20 倍；为典型 TLS 证书（约 2 KB）规模的 RSA 签名生成 240 字节证明仅需 3 秒。

我的思考：把"签名持有证明"从学术玩具推进到可部署水平，核心创新在于对验证算法结构（如随机化、椭圆曲线点的部分公开验证）的精细利用，而非通用 SNARK 的简单套用。240 字节/3 秒的量级已接近实用门槛，为匿名凭证、密钥轮换审计等隐私增强应用铺平道路。局限在于仍需可信设置类 zkSNARK 基础设施，且遗留生态的密钥托管场景接受度待验证；与既有专用方案相比，其通用性-效率折中设计是主要亮点。


#### Volatile and Persistent Memory for zkSNARKs via Algebraic Interactive Proofs.
- **作者**: Alex Ozdemir; Evan Laufer; Dan Boneh
- **方向**: 密码学与协议
- **解读**: 在可验证外包场景中，不可信服务器执行昂贵计算并产出简洁证明（SNARK）。许多真实计算需要访问 RAM——要么是维持某承诺的持久 RAM，要么是初始为零的易失 RAM；但现有技术验证这类带内存访问的计算开销极高，限制了 SNARK 的实用性。

作者针对易失、持久与稀疏持久三种 RAM 模型开发新的证明方法，降低 SNARK 证明时间。过程中产出两个可独立使用的工具：其一，将任意代数交互证明（AIP，公开硬币、非简洁、验证者为算术电路）转化为 SNARK 的通用构造；其二，利用多项式上的 Bézout 恒等式构造唯一性与不相交性 AIP，用于证明不同地址的独立性。

结果同时包含渐进与具体改进——RAM 场景证明时间最高减少 51.3 倍。

我的思考：把"证明 RAM 访问"的开销砍掉一个数量级以上，对真实可验证计算（如虚拟机执行、数据库查询）意义重大，AIP→SNARK 的通用化转化与 Bézout 恒等式技巧具有独立理论价值。摘要未给出证明大小与验证开销的变化，折中情况不明；稀疏持久 RAM 贴近真实工作负载，是其相比纯理论工作的加分项。与同类内存 SNARK 相比，51.3× 的具体数字使其具备直接竞争力。


#### ZHE: Efficient Zero-Knowledge Proofs for HE Evaluations.
- **作者**: Zhelei Zhou; Yun Li; Yuchen Wang; Zhaomin Yang; Bingsheng Zhang; Cheng Hong; Tao Wei; Wenguang Chen
- **方向**: 密码学与协议
- **解读**: 同态加密（HE）允许在密文上直接计算，是隐私保护应用的主流选择，但它默认服务器是诚实但好奇的——一旦服务器恶意，计算结果正确性毫无保证。可验证 HE（vHE）用于检测恶意服务器行为，但现有 vHE 方案要么比底层 HE 运算慢四个数量级以上（Atapoor 等，CIC 2024），要么虽快却无法兼容服务器侧私密输入（Chatel，CCS 2024）。

ZHE 框架同时解决效率与私密输入兼容问题：先设计两个针对模（逆）数论变换（NTT）的高效零知识证明——NTT 是 HE 评估的基本构件；再构建定制化的 HE 评估 ZKP，可扩展且具备非交互式在线阶段，适用于所有 Ring-LWE 类方案（如 BGV、CKKS）。

作者在 CKKS 上实现并做了广泛实验：验证者开销仅为底层 HE 运算的约 27–36 倍，比现有最优 vHE 方案便宜约三个数量级。

我的思考：vHE 长期卡在"要么慢得不可用、要么不兼容私有输入"，ZHE 从 NTT 这一 HE 性能核心入手做定制 ZKP，方向精准，27–36 倍验证者开销在可接受工程范围。与 Chatel 等方案相比，它把私密输入兼容与效率同时拿下是实质性进步。局限：27–36 倍仍非零成本，TFHE 等非 Ring-LWE 方案未覆盖；两篇基准工作（CIC/CCS 2024）均为同代成果，后续性能对比需要更多独立复现。


#### CoBBL: Dynamic Constraint Generation for SNARKs.
- **作者**: Kunming Jiang; Fraser Brown; Riad S. Wahby
- **方向**: 密码学与协议
- **解读**: 通用概率证明系统要求把程序表达为算术约束——对开发者很不友好。把高级语言编译为约束的两条主流路线各有硬伤：直接翻译可产出高度优化的约束，但必须枚举程序所有可能路径，导致约束规模随运行时而非程序大小增长，且证明者要为未执行的路径也付出代价；CPU 模拟则相反，不产生路径爆炸，但无法做强大的程序特定优化，还继承了被模拟 CPU 低效的状态表示。

CoBBL 编译器兼取两者之长：利用程序结构做优化，却不为未执行或无谓的计算付费，实现动态约束生成。

性能上，CoBBL 相对最先进的直接翻译器 CirC 快 1–30 倍（部分场景 26–350 倍），相对模拟器 Jolt 在 Jolt 友好基准上快 1.1–1.8 倍、其他基准最高快 100 倍。

我的思考：把"模拟的执行效率"与"翻译的优化能力"统一到同一编译器里，是 SNARK 可用性研究的实在进步——证明者不再为未走路径买单是理论上限的突破。与 CirC/Jolt 的对照数字（尤其 26–350×）很有说服力。局限在于摘要未交代支持的语言特性范围与设置成本，动态约束生成对证明系统底层的适配性也需更多独立验证；该工作对 SNARK 落地（如 zkVM 与通用证明）有直接推动作用。


#### ALPACA: Anonymous Blocklisting with Constant-Sized Updatable Proofs.
- **作者**: Jiwon Kim; Abhiram Kothapalli; Orestis Chardouvelis; Riad S. Wahby; Paul Grubbs
- **方向**: 密码学与协议
- **解读**: 在线匿名日益重要，但匿名空间的治理是难题。密码学上的"屏蔽名单"方案允许用户匿名发帖同时仍可被治理：发帖时需附上"自己不在屏蔽名单上"的证明。现有方案对大规模名单远谈不上实用——用户需要在密码学上反复处理名单条目，验证时间与证明体积也偏高。

ALPACA 是首个满足"每个条目只需常数工作量"的系统，因此渐进最优；其证明大小与名单条目数无关。关键技术是专门为匿名性设计的新型增量可验证计算（IVC）变体，并给出了形式化定义与安全性证明。

在中端笔记本上，ALPACA 的证明生成耗时 6.15 秒、大小 25.6 KB；在服务器上仅 400 毫秒。

我的思考：把名单更新从"用户逐条重处理"变成"常数工作量"，同时保证匿名性不随折叠泄露身份，是 IVC 应用中的巧妙设计，渐近最优的表述也意味着理论无可再优化。25.6 KB/6.15 秒的端到端数据表明其已具备实际部署潜力。局限：匿名性约束使 IVC 构造显著复杂，安全性证明的审计负担重；常数工作量是否涵盖发帖端全流程、以及更新端到端延迟都需看全文。相比累加器类方案，它首次把常数更新与匿名性同时做到。


#### HyperPianist: Pianist with Linear-Time Prover and Logarithmic Communication Cost.
- **作者**: Chongrong Li; Pengfei Zhu; Yun Li; Cheng Hong; Wenjie Qu; Jiaheng Zhang
- **方向**: 密码学与协议
- **解读**: SNARK 证明紧凑、验证高效，但证明者开销高昂。Wu 等人（USENIX Security 2018）提出把证明任务分布到多台机器上并获得显著加速；然而现有分布式 ZKP 系统仍为拟线性成本，且通信量可能随电路规模线性增长。

HyperPianist 受 S&P 2024 的 Pianist 与 EUROCRYPT 2023 的 HyperPlonk（多元多项式证明）启发，设计了一个证明者线性时间、通信对数的多项式交互预言（PIOP），且应用于一般（非数据并行）电路时无额外开销。作者分别用 KZG 与 Dory 两类加法同态承诺实例化，得到 HyperPianistK 与 HyperPianistD 两个变体，并进一步提出集成基于 Lasso 优化查找论证的 HyperPianist+。

实验显示 HyperPianistD 在 32 台机器上实现 63.1× 与 40.2× 的加速；相比 Pianist，HyperPianistK 在 vanilla 与自定义门电路上分别快 2.9×–4.6×，HyperPianistD 快 2.4×–3.8×；分层电路上 HyperPianistK 最高 5.9×、HyperPianistD 达 4.7×。

我的思考：分布式 ZK 的核心矛盾是"并行加速"与"通信瓶颈"，HyperPianist 用线性时间证明者+对数通信同时解决两者，且不牺牲对一般电路的支持——这是相对 Pianist（面向数据并行电路）的实质扩展。两个承诺实例化（KZG/Dory）给部署方提供了设置与效率的选项。数字（63.1×/32 机）说明扩展性良好；Lasso 查找的集成进一步拓宽适用面。局限在于多层分布式网络下的同步开销与容错性尚待考察。


#### JesseQ: Efficient Zero-Knowledge Proofs for Circuits Over Any Field.
- **作者**: Mengling Liu; Yang Heng; Xingye Lu; Man Ho Au
- **方向**: 密码学与协议
- **解读**: VOLE（向量不经意线性求值）协议的进展催生了常数轮、快速、可扩展的指定验证者零知识证明，显著降低证明者计算成本。现有代表（QuickSilver, CCS'21；LPZKv2, CCS'22）对布尔电路每 AND 门需 4 次扩域乘法（其中一次为 O(k log k) 位运算，k=128 为安全参数），大域算术电路需 3–4 次乘法。

JesseQ 包含两个 VOLE 协议：JQv1 每门仅需 2 次标量乘法（O(κ) 位运算），通信每门仅 1 个域元素；JQv2 将通信再减半，代价是证明者计算翻倍。

实验中，算术电路在线阶段相对现有最优方案至少 3.9× 提升；布尔电路性能相当（1.3× 提升）。在最便宜的 AWS 实例上，每美元可证明 9.2 万亿门（或 5.8 万亿 61-bit 域运算）。JesseQ 在内积、矩阵乘法、格问题等应用上比 QuickSilver 提升 40%–200%，并可无缝集成 CCS'23 的 Batchman 框架，在批量析取语句上获得额外增益。

我的思考：VOLE-ZK 的赛道已相当拥挤，JesseQ 的增量在于把每门成本压到 2 次标量乘法与 1 个域元素通信，并在 JQv1/JQv2 之间给出通信-计算折中选项，对部署场景是实用设计。"每美元证明 9.2 万亿门"的定价视角也很有工程说服力。局限：指定验证者设定本身限制其通用性；40%–200% 的提升集中在特定运算模式。相比 QuickSilver 系，它把常数因子再砍一半，属于扎实的工程性推进。


#### HydraProofs: Optimally Computing All Proofs in a Vector Commitment (With Applications to Efficient zkSNARKs Over Data from Multiple Users).
- **作者**: Christodoulos Pappas; Dimitrios Papadopoulos; Charalampos Papamanthou
- **方向**: 密码学与协议
- **解读**: 向量承诺（VC）在密码学中广泛用于对数据向量的简洁绑定，但为多个元素（或连续子数组）逐一生成打开证明的开销是性能瓶颈：此前方案要么与以多元线性多项式编码输入的 zkSNARK 族不兼容（如 Merkle 树需在 SNARK 内重算哈希电路），要么需 O(N log N) 时间。

HydraProofs 是首个同时满足两个性质的 VC：其一，证明者可在大小的最优时间 O(N) 内为向量各元素（或连续子数组）生成全部打开证明；其二，可直接嵌入以多元线性多项式编码输入的 zkSNARK 家族，无需在 SNARK 内部"打开"整个预映像。作者将其与经典 GKR 协议结合，应用于多用户参与、由不可信服务器执行计算的场景，让每个用户验证自己的数据被正确纳入结果。

实验显示，对一般电路该方法比既有方案快 4–16 倍。具体应用上：可验证秘密共享中 Shamir 重构达到线性时间（低于委托方计算需求）；面向恶意聚合者的鲁棒聚合方案开销合理、对现有系统仅造成微小减速。

我的思考：O(N) 最优性与"即插即用"兼容性双管齐下，直接解决了 VC 在 zkSNARK 应用中"证明生成慢"与"电路重算哈希"两大痛点，4–16× 的实测加速有说服力。与 GKR 的组合逻辑自然，落地场景（秘密共享、鲁棒聚合）选择务实。局限在于其兼容性限定于多元线性编码的 SNARK 家族；不同承诺方案的信任模型差异也需在具体部署中权衡。整体属于理论干净、应用扎实的密码学系统工作。


#### Zero-Knowledge Location Privacy via Accurate Floating-Point SNARKs.
- **作者**: Jens Ernstberger; Chengru Zhang; Luca Ciprian; Philipp Jovanovic; Sebastian Steinhorst
- **方向**: 密码学与协议
- **解读**: 位置隐私场景常要求"证明我在某区域内"而不泄露精确位置。本文提出 ZKLP（零知识位置隐私），支持按用例定制不同粒度的位置证明，且第三方验证无需信任用户或服务商。

核心方法是构造完全符合 IEEE 754 标准的浮点运算零知识证明电路——浮点舍入误差正是此前位置类 ZK 证明易被攻击的薄弱点。结果表明其浮点电路摊销高效：单精度乘法平均每操作仅约 64 个约束；相比基线实现，单精度场景约束数减少 15.9 倍，双精度减少 12.2 倍。

作者进一步构建了隐私保护的端到端邻近测试协议：Alice 可在不泄露任何额外信息的情况下证明自己与 Bob 的距离远近，生成（非）邻近性证明仅需 0.26 秒，每秒可验证 470 个对等方的距离。

我的思考：把 IEEE 754 语义精确建模进 SNARK 电路，堵住了"舍入误差被利用伪造位置证明"这类漏洞，是 ZKLP 可信度的根基；15.9×/12.2× 的电路压缩说明设计上有实打实的优化。0.26 秒证明、470 对等方/秒验证的量级已支持真实 P2P 邻近服务。局限：位置区域的表示粒度与地理编码的精度边界仍需全文确认，GPS 误差与恶意声称的对抗模型也待考察。相比把坐标直接整数化的方案，其"精确浮点"路线更严谨但电路更复杂，属于正确的方向性选择。


#### FairZK: A Scalable System to Prove Machine Learning Fairness in Zero-Knowledge.
- **作者**: Tianyu Zhang; Shen Dong; Oyku Deniz Kose; Yanning Shen; Yupeng Zhang
- **方向**: 密码学与协议
- **解读**: 机器学习在关键决策场景普及后，算法公平性日益重要；但度量公平性通常需要完整访问模型参数，与模型所有者的保密需求冲突——监管者想验证公平，模型方不想开源模型。

FairZK 用零知识证明化解矛盾：模型所有者向公众证明模型是公平的，同时不泄露模型秘密。为绕过"朴素地在 ZK 内证明推理"的效率壁垒，其关键创新是：仅用模型参数与输入的某种聚合信息来度量公平性，而不依赖任何具体数据集；为此为逻辑回归与深度神经网络推导出比先前工作更紧、更贴合实际的公平性界，并开发了谱范数、最大值、绝对值、定点算术等公平性常用运算的高效证明协议。

系统已完整实现并证明零知识性：证明者时间按规模不同提升 3.1×–1789×；首次扩展到 4700 万（参数）规模并在 343 秒内生成证明；对数十万参数的小模型，估计比朴素方案快约 4 个数量级。

我的思考："用参数+聚合信息而非数据集度量公平"是概念上的关键突破——它既保护模型又保护数据，还避免了在 ZK 中证明海量推理；基于界（bound）的公平性度量虽比直接计算指标有松弛，但换来可证明性，是务实的工程取舍。3.1×–1789× 的提升幅度与 4700 万参数的规模都令人印象深刻。局限：界的形式限制了可证明的公平性指标类型，343 秒对在线监管场景仍偏重；与直接披露方案的信任模型差异需要在政策层面权衡。


#### TreeKEM: A Modular Machine-Checked Symbolic Security Analysis of Group Key Agreement in Messaging Layer Security.

- **作者**: Théophile Wallez, Jonathan Protzenko, Karthikeyan Bhargavan

- **方向**: 密码学与协议

- **解读**: 消息层安全（MLS）标准提出基于树的新型协议，为数千成员的大群组提供高效端到端加密消息。其功能分为三部分：TreeSync（组状态认证与同步）、TreeKEM（核心密钥协商）、TreeDEM（消息加密）。以往对 TreeKEM 的安全分析基于抽象模型，未覆盖标准中的精确底层细节。

方法上，本文给出 TreeKEM 的第一个机器验证证明：在符号化 Dolev-Yao 模型下，对位级精确、可执行、可互操作的规范进行形式化安全分析；且该定理可自然组合，为已发布的 MLS 标准提供模块化的强安全保证。

结果：摘要未给出量化数据；工作产出为机器检查的证明（基于 F* 等证明工具链），其可信度来自证明的自动化验证，属于协议形式化验证领域的高可信结果。

我的思考：MLS 是 E2EE 群聊的事实标准（已被 IETF 采纳并被主流应用采用），其核心组件 TreeKEM 的"位级精确"机器验证填补了抽象模型证明与真实实现之间的鸿沟——此前工作常因理想化假设而漏掉实现细节中的漏洞。作者（Bhargavan 团队）在机器验证上积累深厚，其结果可为实现者提供高置信度参考。局限在于符号模型（Dolev-Yao）不覆盖计算复杂性与侧信道，且验证对象是规范而非具体实现；与 MLS 的可执行实现（如 F* 实现）结合才构成完整保障链。


#### Impossibility Results for Post-Compromise Security in Real-World Communication Systems.

- **作者**: Cas Cremers, Niklas Medinger, Aurora Naska

- **方向**: 密码学与协议

- **解读**: iMessage、WhatsApp、Signal 等现代安全通信系统内置复杂机制，将新鲜秘密持续混入加密密钥材料，号称提供极强的安全属性——后妥协安全（PCS）：即使某方状态曾遭完全攻破，也能在未来恢复安全。然而近期研究指出这些证明无法直接传递到终端用户层面，可能源于可用性考量，引发疑问：终端用户究竟能否获得 PCS？在什么条件下可以？

方法上，本文给出形式化不可能性结果：证明现实通信系统若要实现 PCS，必须对某些类型的秘密丢失（实践中确实会发生）具备恢复能力，而这些条件与当前系统的设计与用户使用方式相冲突。

结果：摘要截断，未给出具体量化数据；贡献以形式化定理为主，刻画了 PCS 在真实系统中可实现的边界条件，属于理论性质研究。

我的思考：这是对"安全协议形式化证明"与"现实安全"之间鸿沟的精准打击——协议论文证明 PCS 成立，但端到端用户可能根本享受不到：密钥丢失、设备更换、备份恢复等现实操作会破坏证明所需的前提。Cremers 团队的"不可能性结果"系列一贯以严谨著称，这类负结果对标准制定者有实际价值：它划清了"可承诺"与"不可承诺"的界限，防止营销话术超出协议能力。局限在于不可能性基于特定威胁模型，放宽假设（如引入外部可信组件）后结论可能变化。


#### Extended Diffie-Hellman Encryption for Secure and Efficient Real-Time Beacon Notifications.

- **作者**: Liron David, Omer Berkman, Avinatan Hassidim, David Lazarov, Yossi Matias, Moti Yung

- **方向**: 密码学与协议

- **解读**: 每个计算范式都需要新的安全协议：互联网催生 TLS/SSL，移动计算催生端到端加密。本文面向新兴 IoT 场景——附着于物品的"信标"（beacon）——提出"信标通知问题"：如何让物品所有者安全高效地接收实时通知。

方法上，由于信标通知问题此前未被正式定义，作者先基于运行场景梳理自然需求，通过密码学博弈建立正确性、安全性与隐私的形式化定义；随后提出 XDHIES——对现有 DHIES（Diffie-Hellman 集成加密方案）的显著扩展，在保证安全的同时兼顾效率与实时性。

结果：摘要未给出量化数据；工作以形式化定义与新加密原语为核心，其安全性质通过密码学博弈论证（详细效率对比见论文全文）。

我的思考：本文的价值一半在"把新问题正式化"——IoT 信标通知（如物品防丢、货物追踪）此前缺乏统一的安全模型，伪造通知、窃听位置等攻击目标未被清晰刻画；一半在 XDHIES 这一工程化原语，背后是 Google 等业界作者对实际部署场景的考量。与通用加密方案相比，XDHIES 针对信标场景的轻量级、非交互特性做了定制。局限在于形式化定义覆盖的威胁模型是否涵盖现实攻击（如信标物理克隆、侧信道）需全文检验；与 DHIES 的安全性对比也需计算假设的细节支撑。


#### Peer2PIR: Private Queries for IPFS.
- **作者**: Miti Mazmudar, Shannon Veitch, Rasoul Akhavan Mahdavi
- **方向**: 密码学与协议
- **解读**: IPFS（19万+节点、152国）中任何查询都会向中间节点泄露查询内容：路由查询泄露目标peer ID，提供者广告与内容检索泄露CID，现有工作只解决单一环节。本文首次端到端地私有化IPFS三项核心功能，使网络内无人能得知客户端检索的内容。

方案要点：私有peer路由中提出路由表归一化算法（每桶恰k个最接近该桶索引目标的近邻，必要时随机补足），证明跳数仅增常数，再用PIR隐藏目标公共前缀长度；私有提供者广告将键值库按CID前缀分箱并固定B个bin，每条广告用KDF(CID)对称加密防止客户端多取；私有Bitswap分PRIVATEWANT HAVE与PRIVATEBLOCK两步。针对DHT小库场景提出PaiLLIER PIR（小公钥、低通信）与RLWEPIR/RLWEPIR2/3（RLWE、128位安全），并系统对比SealPIR、FastPIR、OnionPIR、Spiral、HintlessPIR、YPIR。

基于真实IPFS网络爬取（13,692个peer ID、5000次随机查询）验证归一化路由平均2跳（最多6跳）、与原始算法跳数完全一致；私有路由查询<100ms、提供者广告<1.5s，且不增加协议轮次；推荐RLWEPIR3作为通信-计算权衡最优选择。

我的思考：把PIR系统性地嵌入DHT三个功能并保持轮次结构，路由表归一化、CID即索引、加密bin等工程巧思通用性强，是分布式系统隐私化改造的范例。但诚实但好奇模型下安全，对可观察流量的网络级对手、CPIR计算开销引发的DoS放大等问题仍需速率限制等配套措施，实际部署还面临churn下的重计算成本。

- **arXiv PDF**: [Peer2PIR: Private Queries for IPFS.](https://arxiv.org/pdf/2405.17307)

#### TreePIR: Efficient Private Retrieval of Merkle Proofs via Tree Colorings with Fast Indexing and Zero Storage Overhead.
- **作者**: Son Hoang Dau, Quang Cao, Rinaldo Gagiano, Duy Huynh, Xun Yi, Phuc Lu Le, Quang-Hung Luu, Emanuele Viterbo, Yu-Chih Huang, Jingge Zhu, Mohammad M. Jalalzai, Chen Feng
- **方向**: 密码学与协议
- **解读**: 批量PIR（客户端私密检索Merkle证明即根到叶路径）在证书透明度（CT，如Google Xenon2024日志超10亿证书）、区块链无状态客户端（Bitcoin 1.7亿UTXO）等场景意义重大；现有方案基于概率批码（PBC），存储冗余达3N，且索引几乎与整树等大——PBC索引在2^20叶时达294MB、2^24叶时达4.7GB，实用上不可行。

TreePIR基于新概念"平衡祖先染色"：把(交换)Merkle树节点染成h种颜色，使每条根到叶路径恰好含每种颜色一个节点、各色类大小近乎相等，从而零存储冗余（总存储N）地分成h个子库，每库发一个PIR查询即可取回整条路径。作者给出h-可行色序列的充要条件（∑前ℓ个色尺寸≥∑2^i且总量=2^{h+1}-2），设计Θ(n log log n)的分治染色算法，并提出O(h³)、零通信的子索引算法（沿路径运行微型染色），可叠加VBPIR、PIRANA（计算降3×）、Lueks-Goldberg（计算降h^0.80735）。

与PBC相比：总存储3×低、通信1.5×低、计算1.5-2×低、设置时间8-60×低（h=24时2.7秒 vs 159.8秒）、索引19-160×快（0.46ms vs 74ms）；2^30叶设置约180秒、2^36叶索引仅0.7ms；与SealPIR/Spiral组合的客户端查询生成时间快约1.5×。

我的思考：利用"Merkle证明=根到叶路径"的特殊结构突破通用批码的冗余下界，是结构感知设计的典范；平衡染色+毫秒级索引让十亿级CT日志的私有审计成为现实。局限：理论针对完美二叉树，扩展到稀疏Merkle树与增长树（附录仅初步讨论）仍是开放问题，异质存储下的灵活性虽已支持但未充分验证。

- **arXiv PDF**: [TreePIR: Efficient Private Retrieval of Merkle Proofs via Tree Colorings with Fast Indexing and Zero Storage Overhead.](https://arxiv.org/pdf/2205.05211)

#### VerITAS: Verifying Image Transformations at Scale.
- **作者**: Trisha Datta, Binyi Chen, Dan Boneh
- **方向**: 密码学与协议
- **解读**: 图像来源（provenance）验证在新闻媒体领域日益重要，内容来源与真实性联盟（C2PA）制定的标准依赖相机生成的数字签名；但照片发表前通常会被编辑，仅凭已发布的图像无法直接验证签名原图，需要证明"某些编辑已被应用到签名照片上"。已有工作只能处理小尺寸图像，无法扩展到真实摄影场景。

方法上，VerITAS 用零知识证明（zk-SNARK）证明对已签名照片施加了特定编辑；其关键创新是设计一种新的证明结构，使证明能够验证"有效数量的 witness 数据"，从而首次支持 30 兆像素的真实大图——这正是该工作相对前人的数量级跃升所在。

实验结果比先前工作大一个数量级以上：计算能力较弱的签名者（如相机）生成 90 MB 证明仅需约 13 分钟，AWS 成本约 $0.54；使用更强大的机器成本约 $0.13，而无论哪种方式验证时间都小于 1 秒。作者指出该技术可广泛适用于任何需要证明"转换被正确且私有地执行"的场景。

我认为本文的价值在于把 C2PA 签名与 zk-SNARK 结合，弥合了"签名在编辑后失效"这一核心矛盾，直接服务于新闻真实性与深度伪造对抗。不过相机端 13 分钟、约 $0.54 的证明生成仍是签名侧瓶颈，且 zk 证明只能证明"声明的编辑确实发生"，无法排除未声明的其他编辑，安全边界依赖签名设备与电路正确性，落地时需权衡。


#### Trust Nobody: Privacy-Preserving Proofs for Edited Photos with Your Laptop.
- **作者**: Pierpaolo Della Monica, Ivan Visconti, Andrea Vitaletti, Marco Zecchini
- **方向**: 密码学与协议
- **解读**: 互联网上大量图像是对保密原图的变换（如缩放、模糊），在线售卖、打击虚假信息、检测深度伪造等场景都迫切需要一种机制：在不泄露原图的前提下，验证"当前图像是某变换作用于真实图像的结果"。此前方案要么需要昂贵计算资源，要么无法提供令人满意的机密性。

本文聚焦证明生成与验证的正确性，给出四项保证：1) 机密性（原图保持私密）；2) 证明生成高效，普通笔记本电脑即可完成，即使面对高分辨率图像；3) 真实性（只有声明的变换被应用过）；4) 欺诈证明可被快速检测。贡献包括：建模自适应对手的新安全定义、加速 ZK-snark 证明者的技术，以及依赖 ad-hoc 签名与哈希的构造——其使用的哈希比 C2PA 规范中嵌入的哈希更少。

实验结果确认了方法的可行性：证明可在普通计算机上计算，而先前工作要么需要昂贵的计算资源、要么机密性不尽如人意，本文在二者间取得了平衡。

我认为本文与 VerITAS 构成同期直接竞争：VerITAS 追求极端规模（30 兆像素、秒级验证），Trust Nobody 则把证明者成本压到"笔记本电脑"并显式建模自适应对手，工程落地门槛更低。但其依赖 ad-hoc 签名与哈希构造，安全性论证通常不如标准原语稳固，且摘要未披露证明尺寸与验证开销的量化指标，与 VerITAS 的对比还需细读正文评估。


#### Eva: Efficient Privacy-Preserving Proof of Authenticity for Lossily Encoded Videos.
- **作者**: Chengru Zhang, Xiao Yang, David F. Oswald, Mark Ryan, Philipp Jovanovic
- **方向**: 密码学与协议
- **解读**: 虚假视频在误导信息运动中愈演愈烈，证明一段被编辑视频的来源（provenance）——且不泄露原始素材——变得至关重要。此前方法面向图像认证，难以应对有损编码视频产生的复杂、海量编码数据，视频场景急需专门方案。

方法上，本文形式化定义了真实性证明的概念与安全模型，并给出首个支持有损编解码器与任意编辑的密码学认证协议 Eva，在公认假设下证明其安全。其两项理论进展颇具独立价值：将 lookup 论证整合进基于 folding 的增量可验证计算（IVC），并高效压缩 IVC；实现中集成到 Nova folding 方案（命名为 Loua），辅以定制电路设计与 GPU 加速。

结果方面，Eva 相比图像认证方法可处理的数据量大得多，同时达到线性证明者时间、恒定 RAM 占用、证明大小与视频尺寸无关——均为理论最优复杂度。具体而言，2 分钟 HD（1280×720）H.264、30fps 视频仅生成 448 B 证明，消费级硬件上约 2.4 小时（2.6 µs/像素），比最先进方案快一个数量级以上。

我认为本文的贡献在于把"图像→视频"的规模鸿沟用 IVC + folding 理论填平，常数大小证明与线性时间在复杂度上是终点级结果，对新闻取证与可信视频发布是重要基础设施。但 2.4 小时的单视频证明时间对时效性强的新闻场景仍偏长，GPU 依赖也限制了轻量部署；安全模型细节、448 B 证明的验证成本等指标摘要未展开，需结合正文评估其实用边界。


### 隐私保护（23 篇）

#### SoK: A Privacy Framework for Security Research Using Social Media Data.
- **作者**: Kyle Beadle, Kieron Ivy Turk, Aliai Eusebi, Mindy Tran, Marilyne Ordekian, Enrico Mariconti, Yixin Zou, Marie Vasek
- **方向**: 隐私保护
- **解读**: 社交媒体数据在研究中被广泛使用，横跨计算机科学、社会科学、人机交互、法律与犯罪学等领域，但这些数据常含个人敏感信息；既有关于数据使用伦理的讨论过于宽泛，难以揭示细粒度的隐私风险与可行的缓解措施。本文聚焦研究安全相关主题的论文，系统分析 16 年间 601 篇论文，覆盖广泛的学科领域。

方法上，作者运用 Solove 的隐私分类法对识别出的隐私风险场景进行归类，评估该分类法对现代社交媒体研究风险的覆盖能力，并为不同利益相关方提炼启示。

结果方面，研究发现报告透明度严重不足——仅 35% 的论文提及匿名化、可用性、存储等考量；Solove 分类法准确捕捉了聚合风险，但“体量+时效性+微观细节”与现代数据技术结合所产生的风险，已超出其 20 年前提出时的设想；作者为研究者、机构审查委员会（IRB）与出版方提出改进建议，并指出社区的一些小改变可能对用户隐私产生大影响。

我认为这是对“研究中的隐私实践”的一次有价值的系统自省，35% 透明度的量化数据很有冲击力。局限在于分类依赖人工标注、建议的落地效果未经验证；该工作与“研究伦理现代化”趋势相呼应，但如何把建议转化为 IRB 与出版方的可执行规范仍是开放问题。


#### A Low-Cost Privacy-Preserving Digital Wallet for Humanitarian Aid Distribution.
- **作者**: Eva Luvison, Sylvain Chatel, Justinas Sukaitis, Vincent Graf Narbel, Carmela Troncoso, Wouter Lueks
- **方向**: 隐私保护
- **解读**: 人道援助（如 ICRC）数字化能提升效率与公平，但现有方案要么缺乏隐私保护，要么（Wang et al.）仅支持领取固定物资包；而实际场景需要受援者把预算灵活花在不同商家，且硬件受限（无手机、卡间不能通信）。论文设计满足该场景的隐私保护数字钱包。

与 ICRC 合作将需求形式化为功能、部署、安全、隐私四类需求；方案基于智能卡+ORAM 隐藏访问模式：多张卡通过商家异步私有同步家庭共享余额（余额仅 3 字节），交易以 ECDSA 签名+Pedersen 同态承诺生成可聚合证明，商家按周期向结算站批量兑付，审计者只能看到总额。

对防超额消费、防超领、购买不可链接性、审计隐私给出形式化定义与安全证明；不可链接性在"商家不回滚数据库"假设下成立，卡内本地计数器可检测回滚、使恶意商家面临暴露风险；单笔交易仅需一次标准 ECDSA 签名（对比 Zcash 在 i7 上 2 分钟、Platypus 在 iPhone 上 0.8 秒，智能卡上不可行）；原型证明可支撑中等规模援助项目。

把"共享预算+无卡间通信+隐私"这一矛盾场景用 ORAM 与可聚合承诺优雅化解，需求提炼与 ICRC 场景高度贴合，工程取舍合理；但隐私仅对抗隐蔽（covert）敌手，回滚防护依赖商家对败露风险的权衡，实际激励还需试点验证。与 Wang et al. 的固定发放方案形成互补，扩展了数字援助的设计空间。
- **arXiv PDF**: [A Low-Cost Privacy-Preserving Digital Wallet for Humanitarian Aid Distribution.](https://arxiv.org/pdf/2410.15942)

#### Characterizing the Usability and Usefulness of U.S. Ad Transparency Systems.
- **作者**: Kevin Bryson, Arthur Borem, Phoebe Moh, Omer Akgul, Laura Edelson, Tobias Lauinger, Michelle L. Mazurek, Damon McCoy, Blase Ur
- **方向**: 隐私保护
- **解读**: 定向广告只向特定用户展示，引发大量隐私担忧，平台因此提供广告透明度系统（ATS）向用户说明这一实践。为理解当前 ATS 实际向用户传达了什么、以及如何传达，本文首先对 22 个最流行的英语网站（以美国视角呈现）的 ATS 设计与内容进行系统分类。

方法上，研究从透明度增强功能的普及度（如是否展示已推断出的用户兴趣）、信息呈现方式、术语使用、设置位置等维度刻画差异；随后开展在线用户研究，让 198 名参与者使用自己的账号在 8 个代表性平台之一上探索 ATS，检验这些设计差异如何影响用户理解。

结果方面，各平台在透明度增强功能、术语与设置位置等方面差异显著；但所有平台都存在一致的模糊性——用户难以弄清数据如何被用于定向、更改设置的实际影响是什么；用户完成探索后，原本希望解答的问题仍无答案，系统既复杂又缺乏关键细节；作者据此指出最能支持用户的决策点。

我认为这是把 ATS 研究从“有没有透明度”推进到“透明度是否可用”的系统性工作，分类+用户研究的组合提供了扎实证据。局限在于样本限于英语网站与美国用户，任务设计可能影响结论；其发现对 EU DSA 等透明度监管条款的落地具有直接参考价值。


#### Anix: Anonymous Blackout-Resistant Microblogging with Message Endorsing.

- **作者**: Sina Kamali, Diogo Barradas

- **方向**: 隐私保护

- **解读**: 高压政府日益倾向于在政治动荡期间关闭互联网以控制信息流动。作为回应，基于移动设备网状网络的即时消息应用成为公民与活动人士的重要通信工具，其中具微博客功能的应用尤其适合快速通知与动员。然而现有应用大多无法同时满足：维持用户匿名性，同时提供安全机制让用户信任网格中流动的他人消息。本文提出 Anix，一个抗断网的匿名微博客应用。

方法上，Anix 引入两项新颖特性：远程建立匿名连接，以及匿名消息背书（endorsing）机制，使用户能在不暴露身份的前提下对他人消息表达认可、建立信任；Anix 还利用一组身份撤销原语，对用户间关系进行细粒度管理，从而增强匿名性保障。

评估方面，作者进行了全面的微基准测试与仿真实验，结果显示 Anix 在断网场景下具有实用性，能够在基础设施中断时维持通信与信任构建能力。

我的思考：Anix 的定位精准地落在抗断网通信研究的空白处——多数网格消息应用要么匿名但缺乏信任机制，要么有信任机制却牺牲匿名性。将"匿名背书"与"身份可撤销"结合，本质是在匿名性与问责性之间做可配置权衡，对活动人士通信场景合理。局限：摘要未给出微基准与仿真的量化结果，真实部署中带宽、电池与设备异构性都可能影响表现，背书机制本身也可能成为 Sybil 攻击目标。尽管如此，它是少数同时考虑断网韧性、匿名与内容信任三要素的系统，为极端环境通信工具设计提供了参考。


#### Countmamba: A Generalized Website Fingerprinting Attack via Coarse-Grained Representation and Fine-Grained Prediction.

- **作者**: Xianwen Deng, Ruijie Zhao, Yanhao Wang, Mingwei Zhan, Zhi Xue, Yijun Wang

- **方向**: 隐私保护

- **解读**: Tor 是领先的低延迟匿名通信网络，但其流量仍易受网站指纹识别（WF）攻击——攻击者利用包大小、方向、包间时序等侧信道信息推断用户访问的网站。现有 WF 攻击在受控环境下成功率虽高，却依赖完整、未受扰动的流量，面对真实防御机制、多标签与早期识别等场景泛化能力差。本文提出 Countmamba，一个更鲁棒、可适应的 WF 攻击框架。

方法上，Countmamba 引入窗口流量计数矩阵（WTCM）生成抗扰动的粗粒度表示，将固定时间窗口内的事件聚合以抵御防御造成的中等扰动；同时采用面向状态空间（SSO）的分类器，在部分数据到达时增量生成细粒度预测，支持早期识别与多标签场景。与依赖完整捕获的既有方法不同，Countmamba 随新数据到达迭代更新预测，无需完整捕获即可在复杂环境下可靠推断。

在鲁棒性、早期识别等场景的广泛实验表明，Countmamba 优于现有最先进方法，展现出在真实、自适应网络分析中的适用性。源码与实验数据已公开。

我的思考：WF 攻击研究长期在"受控高精度"与"现实脆弱"之间摇摆，Countmamba 同时回应了防御扰动、多标签浏览、早期识别三个现实挑战，其"粗粒度表示 + 增量细粒度预测"的设计直击工程痛点，状态空间模型的引入避免了循环网络在长序列上的效率问题。局限：粗粒度窗口必然损失部分时序信息，面对强扰动防御（如自适应填充）的效果仍需检验；更强的 WF 攻击也意味着 Tor 用户去匿名化风险上升，需正视其隐私威胁属性。总体而言，这是把 WF 攻击推向"现实可用"的扎实工作。


#### Sparta: Practical Anonymity with Long-Term Resistance to Traffic Analysis.

- **作者**: Kyle Fredrickson, Ioannis Demertzis, James P. Hughes, Darrell D. E. Long

- **方向**: 隐私保护

- **解读**: 现有元数据私密消息系统要么不可扩展，要么易受长期流量分析攻击；缓解流量分析的方案往往依赖不现实、不可实现的假设，并对全网施加带宽限制，损害可用性与性能。本文提出通信系统新模型——延迟检索（deferred retrieval），在现实、可实现的用户假设下保证对长期流量分析的抵抗性，并给出实例化系统 Sparta。

方法上，Sparta 是可分发的延迟检索实用可扩展实例，支持高吞吐与多并发会话且不丢失消息。作者给出三种针对不同场景优化的构造：低延迟场景、共享内存环境（多线程）下的高吞吐场景，以及共享无（shared-nothing）分布式环境下的高吞吐场景。

性能结果显示，低延迟构造支持亚毫秒级（小于 1 毫秒）延迟，而高吞吐构造在单台 48 核服务器上每秒可投递超过 70 万条 100B 消息，展现了可扩展性。

我的思考：匿名通信的经典困境是强隐私保证与高吞吐、低延迟难以兼得——Mix 网络延迟高，DC-net 不可扩展，轻量方案又挡不住长期流量分析。Sparta 的延迟检索模型是有洞察力的折中：把读取行为与传输行为解耦，用户无需实时在线即可保证元数据隐私，对异步、间歇在线的现实用户行为更友好；70 万条/秒的吞吐说明工程实现达到实用水平。局限：摘要未说明长期流量分析抵抗性的形式化保证强度与对手模型细节，延迟检索对即时通信体验的影响、分布式构造在真实网络延迟下的表现有待验证。


#### Machine Learning with Privacy for Protected Attributes.
- **作者**: Saeed Mahloujifar, Chuan Guo, G. Edward Suh, Kamalika Chaudhuri
- **方向**: 隐私保护
- **解读**: 差分隐私（DP）把整个样本作为隐私单位，但当只需保护特定敏感属性（车牌、人脸、PII、标签）时，朴素DP会造成不必要的效用损失。本文提出特征差分隐私（FDP）：给定公开特征映射Ψ，只要求邻接样本在Ψ(x)=Ψ(x')时输出不可区分，从而允许模型记忆非敏感部分、严格保护敏感部分。

定义采用基于模拟器的插入/删除变体与f-DP权衡函数语言，支持自适应组合与后处理，并证明其约束属性推断攻击的成功率（adv ≤ 1-f(Ball)）；同时提出双批次DP-SGD变体：公开损失与私有损失分别采样两个批次，避免了直接套用导致子采样放大失效的问题（朴素方案中子采样随机币会泄露公开特征），噪声大小与公开损失的Lipschitz常数无关。

实验：Purchase100上精度提升10-20%；AFHQ扩散模型在ε=8时FID从DP的286.7降至FDP的101.9（非私有25.2、仅公开模糊特征149.1）；LSUN在ε=0.5时FDP达42.4/43.2，接近非私有26.5，而DP为114.2且严重失真；逻辑回归超额风险界O(d/ε²·|S|²·log(1/δ))优于Ghazi等人O(k/ε²·log(1/δ))，正面回答了标签DP能否利用子采样放大的开放问题。

我的思考：定义灵活、涵盖标签DP等特例且性质完备，双批次技巧是实用工程洞察；但公开特征本身可能被用于身份识别（文中Batmobile例子），对成员推断不提供上界，且公开/私有特征划分依赖人工判断，分类错误即隐私漏洞。

- **arXiv PDF**: [Machine Learning with Privacy for Protected Attributes.](https://arxiv.org/pdf/2506.19836)

#### Hash-Prune-Invert: Improved Differentially Private Heavy-Hitter Detection in the Two-Server Model.
- **作者**: Borja Balle, James Bell-Clark, Albert Cheu, Adrià Gascón, Jonathan Katz, Mariana Raykova, Phillipp Schoppmann, Thomas Steinke
- **方向**: 隐私保护
- **解读**: 
差分隐私（DP）重项（heavy-hitter）检测是数据分析的重要原语：给定阈值 t、规模 n、域大小 d 的数据集，算法忽略出现少于 t 次的项、识别超过 t+Δ 次的项（Δ 为误差界）。现有两服务器（或非共谋服务器）协议（如 S&P 2021 的 Poplar）在服务器效率与误差上依赖 log d，当域很大时（如 KB 级字符串，log d≈10^4）不可用。

方法上，提出 hash-prune-invert（HPI）技术：将任何带 log d 依赖的协议编译成计算、通信与轮复杂度（近似）依赖 log n 而非 log d、且误差界与 d 无关的新协议；在对抗性敌手最多腐化一个客户端的假设下保持隐私；同时给出改进版 Poplar，将误差改善约 √n 因子（与 d 无关）。

实验证实 HPI 在大 d 场景下全面改善协议表现。

我的思考：把依赖从 log d 压到 log n 且误差与域大小解耦，直击大规模域场景痛点，理论贡献扎实且变换具有通用性（可编译任意此类协议）；局限是依赖非共谋双服务器与至多一腐化客户端的假设。对联邦遥测、浏览器遥测等大规模数据分析有重要实用价值。


#### Click Without Compromise: Online Advertising Measurement via Per User Differential Privacy.
- **作者**: Yingtai Xiao, Jian Du, Shikun Zhang, Wanrong Zhang, Qian Yang, Danfeng Zhang, Daniel Kifer
- **方向**: 隐私保护
- **解读**: 在线广告测量（多触点归因 MTA）需跨平台收集用户行为并把转化数实时按天汇报给广告主，而 GDPR、苹果 ATT 等监管趋严，行业方案（Apple PCM、Google Attribution Reporting API）均无形式化隐私保证。本文首次形式化流式上报的广告测量差分隐私问题，并指出事件级 DP 不足以保护“用户是否参与过某广告活动”，需要更强的用户级 DP。

提出 AdsBPC：每天对数据加高斯噪声但尺度逐日不同（非同分布），把噪声标定建模为带隐私预算约束的方差最小化问题，可适配任意查询负载与目标函数；引入每日有界用户贡献（每天至多 r_i 次转化，超出裁剪）降低敏感度；前 l 天用差分隐私分位数估计 r_i，之后用稀疏向量技术（SVT）仅在显著变化时更新界；整体满足 zCDP（ρ=ρ1+l·ρ2+ρ3），并扩展到多发布者。

在 3 个合成数据集（各 100 万用户、1000 个发布者）和 Criteo、Facebook 真实数据上（n=31 天，ρ=1）：前缀和查询下加权 RMSE 比最优基线低 33%–63%，滑动窗口下最大方差低 66%–95%（合计 33%–95%）。基于全局敏感度的 IPA、BIN、UMM 因用户贡献长尾而过噪，Stream 则面向大规模释放场景。

我的思考：这是少见的工业界 DP 工作（作者多来自 TikTok），首次在流式 MTA 场景给出用户级 DP 形式保证，并以“独立噪声+每日裁剪”的极简设计胜过矩阵机制（用户级下矩阵机制 NP-hard，小 n 时最优分解退化为对角阵），可部署性强。局限：依赖归因权重归一化假设、预算分配需调参、未覆盖多设备归因；一般情形下噪声尺度的最优性仍是开放问题。

- **arXiv PDF**: [Click Without Compromise: Online Advertising Measurement via Per User Differential Privacy.](https://arxiv.org/pdf/2406.02463)

#### PAC-Private Algorithms.
- **作者**: Mayuri Sridhar, Hanshen Xiao, Srinivas Devadas
- **方向**: 隐私保护
- **解读**: 
可证明隐私通常需要复杂的分析且伴随不可接受的精度损失；成员推断攻击（MIA）、差分隐私审计（DPA）等经验验证或近似方法虽被广泛使用，却不提供严格保证。本文针对这一缺口，将新近提出的PAC（Probably Approximately Correct）隐私应用到一系列实用黑盒算法上。

作者为K-Means、SVM、PCA与随机森林等算法给出形式化、机械化、基于仿真的证明；提出新的仿真算法，高效确定达到任意给定隐私级别所需的各向异性噪声，并证明其相比各向同性噪声有实质收益。研究还表明稳定算法更易私有化，正则化引入可放大隐私；并提出将不可处理的几何稳定性转化为高效确定性验证的技术。

大量实验验证了方法在对抗性推断硬度上的可证明保证，并以较小精度损失获得有意义的隐私收益。

把PAC隐私从理论推向实用算法集是该工作的重要贡献，各向异性噪声的思想也可迁移到经典DP机制设计；但PAC保证是概率性而非最坏情况的，与经典DP的语义差异需要使用者充分理解，其应用范围目前限于所列算法族。


#### An Attack-Agnostic Defense Framework Against Manipulation Attacks Under Local Differential Privacy.
- **作者**: Puning Zhao, Zhikun Zhang, Jiawei Dong, Jiafei Wu, Zhe Liu, Shaowei Wang, Yunjun Gao
- **方向**: 隐私保护
- **解读**: 
保护LDP协议免受操纵攻击是重要而困难的问题，理想框架应不依赖攻击者的任何知识。早期工作通过把每个样本转换为二元信号来限制攻击者能力，但信号压缩导致严重信息损失，尤其在ε>1时造成不必要的效用牺牲。

本文提出通用估计框架RobustLDP：向所有用户发送精心构造的预定义信息，然后在服务器端聚合反馈，在保留效用与限制攻击者能力之间取得更优权衡；并在频率估计与均值估计（ℓ1与ℓ2支持）上实例化，作为更高级任务的基础构件，同时建立针对各类攻击的理论保证。

实验结果显示，在ε>1的场景下RobustLDP显著优于现有方法，多个真实数据集验证了其有效性。

攻击无关（不假设攻击者模型）使防御更加稳健，"预定义信息+反馈聚合"的设计简洁而优雅；但当前主要覆盖频率与均值类查询，更复杂任务需要更多构建块支撑。与Robust-RR等早期方案相比，该框架避免了二元压缩带来的效用损失，是LDP操纵攻击防御的重要进展。


#### From Randomized Response to Randomized Index: Answering Subset Counting Queries with Local Differential Privacy.
- **作者**: Qingqing Ye, Liantong Yu, Kai Huang, Xiaokui Xiao, Weiran Liu, Haibo Hu
- **方向**: 隐私保护
- **解读**: 
本地差分隐私（LDP）是保护个体数据隐私的主流模型，但现有扰动机制通常要求扰动原始值，不可避免地造成值扭曲与效用恶化。

本文提出替代路线——不再扰动值，而是对索引施加随机化，同时保持严格的LDP保证。受随机索引的可否认性启发，提出CRIAD用于集合值数据的子集计数查询；通过集成多重虚拟项（multi-dummy）、多样本（multi-sample）与多组（multi-group）策略，使其成为可扩展的解决方案，可在不同域规模的需求下灵活调整。

通过全面的理论分析与大量实验评估，CRIAD在查询准确度上优于现有全部方法，并在多种域规模下保持可扩展性。

从随机化应答到随机化索引是思路上的转变，巧妙借用了"可否认性"的概念，规避了值扰动固有的信息损失；多重虚拟项、多样本、多组策略背后的隐私-效用权衡值得深入推敲，其理论分析是否覆盖所有参数域也需细读。与本地哈希（OLH等）类索引机制的系统对比是其定位的关键，该方法为集合值数据LDP查询提供了新范式，具备清晰的实用前景。


#### Augmented Shuffle Protocols for Accurate and Robust Frequency Estimation Under Differential Privacy.
- **作者**: Takao Murakami, Yuichi Sei, Reo Eriguchi
- **方向**: 隐私保护
- **解读**: 
shuffle模型通过引入随机打乱器（shuffler）获得高效用，但近期研究表明现有协议存在两大缺陷：一是易受本地投毒攻击（攻击者发送构造数据操纵统计量，隐私预算ε小时尤甚）；二是收集器与打乱器共谋会使实际ε升高，削弱隐私保证。

本文系统探索增强shuffle模型（允许随机采样、添加虚拟值等额外操作）：提出通用框架，用户发送经过加密、不加噪声的值；并证明"若更简单的机制对二元DP稳健，则协议稳健"的判定定理。基于该框架给出三个具体协议：第一个以二项分布生成各值数量，精度高于多个SOTA协议；第二个引入新颖的非对称双边几何分布虚拟计数分布，显著改进；第三个为纯ε-DP的特例。

理论分析与全面实验验证了协议在投毒与共谋攻击下的鲁棒性与高精度。

把投毒鲁棒性纳入shuffle模型设计是重要推进，判定定理为协议分析提供了简洁工具；但增强模型对shuffler能力（随机采样、去重等）的假设更强，需在部署中权衡信任模型。几何分布虚拟计数的设计巧妙，可能启发后续协议创新。


#### Differentially Private Release of Israel's National Registry of Live Births.
- **作者**: Shlomi Hod, Ran Canetti
- **方向**: 隐私保护
- **解读**: 2024年2月以色列卫生部以差分隐私(DP)形式发布2014年单胎活产婴儿微观数据（165,915条记录、6个字段），这是据作者所知全球首个以纯DP保证发布的公共卫生微观数据。难点在于：利益相关方明确要求微观数据格式（拒绝查询答案列表）、要求记录级保真、禁止唯一行，传统匿名化又无法满足需求。

作者提出"通用方案"：均匀采样配置（模型族、超参数、数据分箱变换）→训练εx-DP生成模型→按合理性约束过滤采样→数据集投影（保证k=5、b=5的k-匿名、无唯一行）→用εq-DP接受标准（边际表、条件均值/中位数、线性回归、逐记录保真度）评估，不达标则重试；借助Liu-Talwar私有选择算法将多次尝试的预算消耗从随尝试次数线性累积降为固定2倍。另用美国NVSS公开出生数据在安全计算环境外预筛选配置。

预算分配为生成模型ε=4、接受标准评估ε=0.99、私有选择乘2，总预算ε=9.98，发布数据集最终通过全部接受标准，预算以历史真实DP发布为参照控制在ε<10。文中还披露了三个后期工程修正（线性回归预算节省、MAE全局灵敏度计算错误修正、投影算法简化）。

我的思考：这是DP从理论走向政府级真实部署的珍贵记录，格式、保真度、"面部隐私"等非传统需求催生了新概念，接受标准+私有选择是工程上稳健的组合。但ε=9.98理论上偏高，依赖"实际对手"假设；配置空间大、依赖公开数据代理，方案的普适性与可复制性仍有待后续发布验证。

- **arXiv PDF**: [Differentially Private Release of Israel's National Registry of Live Births.](https://arxiv.org/pdf/2405.00267)

#### Meeting Utility Constraints in Differential Privacy: A Privacy-Boosting Approach.
- **作者**: Bo Jiang, Wanrong Zhang, Donghang Lu, Jian Du, Sagar Sharma, Qiang Yan
- **方向**: 隐私保护
- **解读**: 传统DP机制"隐私优先"：按目标ε确定噪声尺度再检验效用，常与分析师的具体效用约束（如健康应用要求误差不超过真实值5%）脱节；已有的效用优先方法又局限于固定噪声形式。本文提出隐私提升（PB-DP）框架，把效用约束作为设计的一等公民。

PB-DP以任意加噪机制为核，按提升率q对其概率密度在偏好区域S(Q(X))内重加权，使输出落入偏好区域概率≥ρ，同时拉长分布尾部以降低整体隐私泄漏；q由核机制与效用约束决定。作者刻画了机制隐私损失分布（PLD），给出(ε,δ)-DP与RDP的隐私谱表达式、O(T²)的组合核算算法，并用三元搜索求最优核参数；覆盖数据相关（相对误差）、固定区域、数据无关（绝对误差）及本地随机应答四类场景。

实验中以高斯机制为核，在相同效用约束下PB-DP所需ε显著低于标准高斯机制，高敏感查询（ΔQ=4）与严格隐私参数（δ=10⁻⁷、大α）下差距更大；T=1000次组合下隐私损失仍显著更低；在Adult数据集（45,222条）上验证了PB-GRR频率估计的实用性。

我的思考：用"软边界提升+尾部加宽"替代硬截断，从机制设计层面同时服务效用与隐私，思路优雅且与多数加噪机制兼容，工程落地性强。局限在于最优参数搜索依赖对偏好区域与查询分布的刻画，且作者指出纯DP场景下L1/L2泄漏无法吸收，框架更适合近似DP；核机制选择与q的联合最优仍是开放问题。

- **arXiv PDF**: [Meeting Utility Constraints in Differential Privacy: A Privacy-Boosting Approach.](https://arxiv.org/pdf/2412.10612)

#### DPolicy: Managing Privacy Risks Across Multiple Releases with Differential Privacy.
- **作者**: Nicolas Küchler, Alexander Viand, Hidde Lycklama, Anwar Hithnawi
- **方向**: 隐私保护
- **解读**: 组织中DP发布通常各自为政，孤立保证的组合风险难以评估，而全局单一保证又因预算过大失去意义。作者指出多个发布可联合放大攻击：例如对两个模型的独立成员推断各得75%置信时，贝叶斯合并可升至90%；面向合成数据的属性级攻击也会因预算分配不均而集中泄露某一属性。

DPolicy引入高层策略语言：规则=(谓词作用域, 隐私单元, 预算)。基础策略（自定义、按属性、按类别+成员等级）生成中间规则，扩展策略按上下文（如标准vs黑盒ML、是否含超参调优）映射预算并自动组合成笛卡尔积规则集；支持多作用域与多隐私单元（时间单元经群隐私换算，如Wikimedia日级ρ=0.015按群隐私换算为月级ρ=14.415，而加入月贡献上限可降到0.735）。执行端用偏序与定理5.1剪枝非约束规则，结合块组合核算与时间步分段跟踪实现细粒度、有状态的策略执行。

作者实现了系统并开源，与预算分配系统Cohere集成，演示了如何阻止孤立管理下出现的属性预算集中与跨发布成员推断等隐私风险。

我的思考：把DP从"机制"提升为"组织级风险治理"，显式化作用域、上下文与隐私单元这些传统隐式假设，方向正确且填补了真实缺口。局限：有效性高度依赖标注质量与策略正确性，存在类似访问控制误配置的风险；群隐私换算只是上界，改进需要机制级多单元联合记账，本文仅给出初步框架。

- **arXiv PDF**: [DPolicy: Managing Privacy Risks Across Multiple Releases with Differential Privacy.](https://arxiv.org/pdf/2505.06747)

#### Differentially Private Selection Using Smooth Sensitivity.
- **作者**: Iago C. Chaves, Victor A. E. de Farias, Amanda Perez, Diego Mesquita, Javam C. Machado
- **方向**: 隐私保护
- **解读**: 私有选择任务（在敏感数据中选出最重要信息，如基因组GWAS中最显著SNP）现有机制（指数机制、permute-and-flip）均基于全局灵敏度加噪，考虑最坏情形导致噪声过大；而平滑灵敏度此前只用于数值查询，尚无选择机制。本文提出首个基于平滑灵敏度的选择机制Smooth Private Selection。

机制对每个候选的分数施加平滑灵敏度标定的噪声后取arg max。理论贡献包括：新定理允许把ε预算在α（滑动性质）与β（伸缩性质）间灵活分配（k∈(0,2)）；双侧噪声满足(k+|R|/2·l)ε-DP、单侧噪声满足(k+(|R|-1)/2·l)ε-DP；定理4/5给出小β下平滑灵敏度与平滑上界的有效计算方法（S(x)=GS·e^{-β·gd(x)}及带阈值T的变体）；定理6对密度h(z)∝1/(1+|z|^γ)给出比既有结果更紧的(α,β)=(ε/2·(γ-1)^{(γ-1)/γ}, ε/(2(γ-1)))。

在TDT基因组统计实验（N=150、m=5~20个SNP、ε∈[3,21]）中，定理5+单侧噪声（情形IV）精度全面最优；真实215家庭数据上ε=21时正确率92.5%（指数机制82.5%、permute-and-flip 80.0%），ε=3时32.5%对30.0%；γ=4最优；m=1000时运行时间约43秒，仍可接受。

我的思考：该工作把平滑灵敏度的理论优势首次带到选择任务，并补齐了噪声分布基础理论，具有奠基意义。但定理4适用的小β在现实中往往过小，需依赖定理5的阈值技巧，引入了对数据先验的依赖；一般情形下的误差分析与k、l的最优设置仍是开放问题，制约其直接落地。

- **arXiv PDF**: [Differentially Private Selection Using Smooth Sensitivity.](https://arxiv.org/pdf/2410.10187)

#### From Easy to Hard: Building a Shortcut for Differentially Private Image Synthesis.
- **作者**: Kecen Li, Chen Gong, Xiaochen Li, Yuzhong Zhao, Xinwen Hou, Tianhao Wang
- **方向**: 隐私保护
- **解读**: DP-SGD训练扩散模型生成DP合成图像收敛慢、复杂数据集上性能差；公共数据预训练虽能加速，但收益依赖公共与敏感数据的相似性，合适公共数据不可得时缺乏方案。本文受课程学习启发，提出两阶段框架DP-FETA（从易到难），在不使用任何公共数据的情况下改进DP图像合成。

阶段一"预热"：从敏感数据查询"中心图像"——均值图（对Poisson子采样图像裁剪后求均值，敏感度Cc/B*）与众数图（逐像素直方图加噪，整图敏感度√(WHCx)），以RDP记账隐私开销，配合数据增强（14种变换随机组合）防止在少量中心图像上过拟合，预热扩散模型学习轮廓、底色等简单特征；阶段二在原始敏感图像上用DP-SGD微调学习复杂内容，总预算为(α,γw+γf)-RDP。

在MNIST、F-MNIST、CelebA、Camelyon四个数据集上，平均FID比SOTA方法DPDM低33.1%、下游分类Acc高2.1%；ε=1时DP-FETAe的FID低37.4%、Acc高3.8%（如MNIST FID 8.5 vs 23.4，Acc 96.5% vs 93.4%）；相比无预热版本FID平均低32.8%、Acc高3.3%；达到相同FID只需18%的微调迭代，最优中心图像数量（50~1000）远小于敏感图像数量。

我的思考：用"中心图像"以极小隐私代价提供先验形状，缓解DP-SGD冷启动，是课程学习思想与DP结合的巧妙工程；均值图比众数图更稳定，但众数图可自然扩展至文本等离散数据。与公共数据结合（如PrivImagee）尚未充分探索，且隐私核算若改用更紧的PRV方法收益有限，提示该路线还有优化空间。

- **arXiv PDF**: [From Easy to Hard: Building a Shortcut for Differentially Private Image Synthesis.](https://arxiv.org/pdf/2504.01395)

#### The Inadequacy of Similarity-Based Privacy Metrics: Privacy Attacks Against "Truly Anonymous" Synthetic Datasets.
- **作者**: Georgi Ganev, Emiliano De Cristofaro
- **方向**: 隐私保护
- **解读**: 
生成模型产出的合成数据被宣传为隐私友好的数据发布方式，但其隐私保证只有在满足差分隐私时才被认为稳健；然而许多头部公司与研究论文并未采用DP，而是使用基于合成数据与真实数据统计相似度的ad-hoc指标来论证"匿名性"。

本文系统考察这些相似度指标在实际部署中的可靠性，并以多种方式证明其不可靠：首先构造反例——即使相似度测试全部通过，仍会发生严重隐私侵犯，攻击者可低成本实例化准确的成员推断与属性推断攻击；随后提出Recon-Syn重建攻击，仅以黑盒方式访问单个生成模型，生成多个数据集，恢复被声称"隐私保护"却实际泄露的独特个体记录。

结果显示，在相似度指标通过的情况下，Recon-Syn可恢复78-100%的训练离群记录；应用DP可以缓解攻击，但会打破端到端管线。

这是对"相似度即隐私"叙事的有力证伪，直接冲击产业实践；重建攻击比成员推断更能直观说明问题严重性。其教训明确：非DP合成数据不应宣称匿名，DP应成为底线而非可选项，为合成数据行业的合规与安全实践敲响警钟。


#### A Big Step Forward? A User-Centric Examination of iOS App Privacy Report and Enhancements.
- **作者**: Liu Wang, Dong Wang, Shidong Pan, Zheng Jiang, Haoyu Wang, Yi Wang
- **方向**: 隐私保护
- **解读**: iOS 15.2起Apple推出App Privacy Report，记录7天内各应用对敏感数据的访问与联网域名，被宣传为隐私透明的一大步，但其真实世界影响从未被检验。本文从用户视角对该功能做端到端评估并提出增强方案。

研究先对12名日常iOS用户做结构化焦点小组（隐私关注度均值6.83/10）：所有参与者此前均未听说过该功能，仅1人无意中开启过，说明其实际收益远低于预期；交互后确认两大可用性缺陷——数据访问"目的"不明（如"手电筒应用为何要定位"）与域名含义难懂。为此提出增强：(1)目的推断框架：Frida动态插桩敏感API并采集调用栈，LLM从应用描述与隐私政策（切片+<主体,对象,场景>三元组提取）整合上下文，链式思考推理数据访问目的；(2)域名澄清流水线：网络情报源构建白名单库+LLM识别+人工兜底。

评估中，42个可测应用上tracker捕获117/123项数据访问；LLM从隐私政策准确提取192/200条语句；目的推断经两位专家判定准确率91%和93%（一致率90%）；165个真实域名中LLM识别160个且人工核验100%准确；11人Think Aloud研究反馈积极，返回参与者确认原有顾虑被解决。

我的思考：该工作揭示"技术正确但用户无感"的隐私工程困境——功能默认关闭、缺乏教育，参与者竟无一人知晓；LLM+动态分析的组合轻量可行，是隐私工具可用性改进的示范性工作。局限：样本量小、推断质量依赖代码命名与政策表述的清晰度，规模化部署与隐私政策切片仍有工程挑战。

- **arXiv PDF**: [A Big Step Forward? A User-Centric Examination of iOS App Privacy Report and Enhancements.](https://arxiv.org/pdf/2511.00467)

#### Myco: Unlocking Polylogarithmic Accesses in Metadata-Private Messaging.

- **作者**: Darya Kaviani, Deevashwer Rathee, Bhargav Annem, Raluca Ada Popa

- **方向**: 隐私保护

- **解读**: 数十亿人依赖端到端加密消息，但通信时序、参与者关系等元数据仍在持续泄露并可用于去匿名化。具有强密码学保证的异步元数据隐藏方案历来受制于 O(N²) 的服务端计算（N 为用户数）——瓶颈在于对 PIR（私有信息检索）的依赖。

方法上，Myco 放弃 PIR，转而引入一种 oblivious 数据结构，发送者与接收者通过它私有通信：通过将读写操作去关联，在不泄露元数据的前提下实现高效消息投递。Myco 将系统实例化为非对称双服务器分布式信任模型，客户端写入消息，读取时亦保持不可链接。

结果：结果显示 Myco 在保持元数据隐私的同时将服务端计算复杂度从 O(N²) 降至 O(N log²N)，在用户规模扩大时具有显著效率优势（具体基准数据见论文全文）。

我的思考：元数据隐私是消息安全领域"最后一个硬骨头"，此前 Signal 等协议只解决内容加密。Myco 通过引入 oblivious 数据结构绕开 PIR 瓶颈，把计算复杂度压到近线性，是从理论走向实践的关键一步；双服务器模型把信任从单一运营商分散开，与现有"单一信任锚"方案相比部署弹性更好。局限在于双服务器假设要求两方不同谋，且 oblivious 数据结构的带宽与延迟开销仍需实证；与 Signal 的匿名凭据、PIR 类方案相比，Myco 扩展性更优，但生态迁移成本高。


#### Mixnets on a Tightrope: Quantifying the Leakage of Mix Networks Using a Provably Optimal Heuristic Adversary.

- **作者**: Sebastian Meiser, Debajyoti Das, Moritz Kirschte, Esfandiar Mohammadi, Aniket Kate

- **方向**: 隐私保护

- **解读**: 混合网络（mixnet）被广泛认为能隐藏通信元数据，但设计拓扑与路由策略时存在诸多陷阱，尤其是追求低延迟的 mixnet 会引入额外泄露。本文开发工具在经验上量化这类泄露，并精确估计接收方匿名性（误差仅来自采样）。

方法上，作者首先提出一种新的通用攻击策略，并证明其在破坏匿名性上的最优性；与以往工作不同，该攻击通过"隐私损失"度量每个观察的泄露严重程度并加权利用。其次，工具给出攻击者优势的下界：基于大量观察采样，若相当数量的观察具有高损失，则输出损失的尾部质量分布，从而对匿名性给出保守但有保证的估计。

结果：摘要未给出量化数字；贡献包括可证明最优的启发式攻击、泄露量化的形式化下界以及配套实证工具（具体实验数据见论文全文）。

我的思考：mixnet 研究长期在"理论匿名集大小"与"现实可观察攻击"之间脱节，本文的可证明最优攻击策略把"对手能做什么"推向理论极限，而隐私损失加权机制让防御者知道哪些观察最致命——这为 mixnet 设计（如拓扑选择、延迟预算分配）提供了可操作的量化依据。与既往"匿名集=安全"的乐观假设相比，本文更贴近现实威胁。局限在于攻击最优性在特定模型内成立，对自适应对手或大规模网络的实证仍需验证；该工作是"量化匿名"方向的重要推进。


#### SoK: Self-Generated Nudes over Private Chats: How can Technology Contribute to a Safer Sexting?

- **作者**: Joel Samper, Bernardo Ferreira

- **方向**: 隐私保护

- **解读**: 越来越多人通过移动应用建立亲密关系，有时会分享自拍裸照（self-generated nudes）。这既是性探索的途径，也带来隐私与安全担忧。本文系统梳理技术辅助的"许可式"方案与功能——那些在用户分享裸照时提供安全、隐私或问责益处的技术手段。

方法上，作者进行系统性文献梳理（筛选 10,026 条搜索交叉引用），并调研 52 款约会、消息与社交应用的 OS 级功能以识别真实世界的解决方案；进而系统化知识：定义 sexting 威胁模型，推导方案/功能分类学，讨论其缺陷，组织隐私相关概念，并给出可操作建议。

结果：摘要未给出量化数字；产出以分类学与威胁模型为核心，覆盖文献与真实应用两大来源，为研究者与实践者提供全景视图。

我的思考：SoK 的价值在于把分散在"家长控制、平台举报、端到端加密、一次性查看"等零散功能中的相关技术整合成统一框架，并用威胁模型锚定讨论——这有助于回答"技术能在多大程度上让 sexting 更安全"这一政策敏感问题。与纯技术 SoK 不同，本文还涉及问责与法律维度，视角更广。局限在于"许可式"定位本身有伦理争议（技术也可能被滥用于监控），且 52 款应用与文献样本的时效性会影响分类学的完备性。


### 系统与操作系统安全（14 篇）

#### What We Talk About When We Talk About Logs: Understanding the Effects of Dataset Quality on Endpoint Threat Detection Research.
- **作者**: Jason Liu, Muhammad Adil Inam, Akul Goyal, Andy Riddle, Kim Westfall, Adam Bates
- **方向**: 系统与操作系统安全
- **解读**:
端点威胁检测研究依赖高质量评估基准，但研究者对基准数据集内容的了解往往有限：通常只关注攻击行为是否真实（其仅占审计日志的一小部分），而数据的其他特征晦涩难知，可能悄悄扭曲实验结论。

作者提出一组审视数据集的新问题（如"数据集包含哪些活动？"），并引入新颖的可视化——通过出现-来源图邻域的时间序列刻画 100+ GB 日志的全貌；对训练与测试划分进行自相关分析，识别按可预测周期发生的合成背景进程行为；并量化"明显恶意"事件在已知与未见邻域中的占比。随后用这些画像验证其对 SOTA 入侵检测系统（R-CAID、FLASH、KAIROS、GNN）在 DARPA Transparent Computing、OpTC、ATLAS、ATLASv2 等公共数据集上分类性能的影响。

结果表明，数据集特性会大幅抬高真阴性率、人为提升真阳性率，使模型得分失真；显式控制这些因素后，才能获得更全面的分类器性能画像。

该文为"数据集卫生"提供了系统方法学，警示社区中隐性的评估失真问题，对提升端点检测实验严谨性有直接价值。局限在于聚焦端点日志域，方法迁移到其他域需再验证。它推动研究者把数据集刻画当作实验设计的一等公民。


#### Speedrunning the Maze: Meeting Regulatory Patching Deadlines in a Large Enterprise Environment.

- **作者**: Gerbrand ten Napel, Michel van Eeten, Simon Parkin

- **方向**: 系统与操作系统安全

- **解读**: 大量企业难以按时完成补丁部署，延迟源于技术依赖、资产清单过时与规模化难题。为此政府开始通过法规强制限期修补精选严重漏洞（如荷兰 BIO、美国 CISA KEV），但法规时限在企业环境中是否可行缺乏实证。本文与一家大型企业合作，对法规化补丁时限进行七年尺度的实证考察。

方法上，作者分析企业票务系统中七年内 81 份安全通告（覆盖 944 个 CVE）的补丁时间线，并访谈 9 名补丁管理专业人员，将量化数据与定性洞察结合，剖析延误原因与法规目标间的张力。

结果显示：40.2% 的通告需要实际修补，中位完成 13.2 天；不需修补的仅 1.4 天。48 小时完成率只有 16.2%；面对 BIO 一周期限仅 32.4% 达标；CISA KEV 时限表现较好——两周 56.8%、三周 62.2%。延误部分可由协调工作量（团队与人数）解释，优先级越高修补越快。

我的思考：这是补丁管理政策研究里少见的"七年真实数据 + 访谈"混合证据，直接回应法规可行性之争：48 小时与一周时限大面积落空，三周虽可行却留下更长的暴露窗口——监管者必须在现实性与暴露风险间取舍。局限：单家企业样本外推需谨慎。与既有补丁延迟研究相比，本文把讨论从"为何慢"推进到"法规目标如何设定才现实"，对政策制定有直接参考价值。


#### SwiftSweeper: Defeating Use-After-Free Bugs Using Memory Sweeper Without Stop-the-World.

- **作者**: Junho Ahn, Kanghyuk Lee, Chanyoung Park, Hyungon Moon, Youngjin Kwon

- **方向**: 系统与操作系统安全

- **解读**: Use-after-free（UAF）是 C/C++ 最严重的内存安全漏洞类型之一。此前受保守式 GC 启发的工作采用"内存清扫"缓解 UAF，但继承了 stop-the-world 暂停、可扩展性差、CPU 占用高等缺陷，不适合现代延迟敏感应用。本文提出 SwiftSweeper，一个面向未修改二进制、专为消除暂停与提升扩展性而重新设计的 UAF 防护分配器。

方法上，SwiftSweeper 重构清扫架构：用基于 eBPF 的 XMP（eXpress Memory Path）实现高效内核态数据通路，与用户态协同设计，使清扫、跟踪等操作在快速路径上低开销完成并避免全局暂停。"不修改二进制"的设计使既有程序无需重编译即可获得防护。

作者在 Linux 上实现，并在单线程与多线程应用、SPEC 与 WebServer 基准上评估：在提供 SOTA 级防护的同时实现高效能与极低延迟开销，消除停顿并显著提升可扩展性，满足高性能 C++ 负载要求。

我的思考：本文把清扫式 UAF 防护从概念验证推向生产可用：XMP/eBPF 内核数据通路 + 用户态协同的设计直击旧方案三大痛点（暂停、扩展性、CPU 开销），"不修改二进制"扫清部署障碍。若延迟开销在真实负载上确实可忽略，它有望成为生产环境 UAF 缓解的可选方案。局限：清扫本质是延迟重用检测，无法完全阻止所有 UAF；eBPF 路径在不同内核版本的可移植性待考察。与 MarkUs 等清扫系工作相比，SwiftSweeper 以"无暂停"差异化，在实时性敏感场景具有明确吸引力。


#### SoK: Challenges and Paths Toward Memory Safety for eBPF.
- **作者**: Kaiming Huang, Mathias Payer, Zhiyun Qian, Jack Sampson, Gang Tan, Trent Jaeger
- **方向**: 系统与操作系统安全
- **解读**: eBPF 允许不修改内核代码即扩展内核功能（网络、追踪、安全检测等），其校验器虽在运行前验证程序以阻止内存错误，但该子系统（包括校验器自身）仍被曝出大量漏洞，严重违背安全预期，引发对 eBPF 内存安全威胁的担忧。本文首次对 eBPF 生态固有风险做系统分析，聚焦既有挑战、局限与当前防御。

方法上，作者系统梳理并评估已提出的研究性缓解策略——包括隔离技术、运行时与静态验证——分析其贡献与差距，给出通向全面内存安全保证的可行路径。

量化结果是本文的亮点：对公开收集的 eBPF 程序分析发现，仅 1.62%–3.74%（37–85 个）的操作无法被全面证明安全，说明强内存安全保证与性能、兼容性之间的权衡空间比想象中更小，也为针对性加固提供了精确目标。

我认为这篇 SoK 的价值在于把“eBPF 是否安全”的争论转化为可操作的问题清单与评估框架，其“大部分操作可被证明安全”的结论直接支撑“可证明安全子集+隔离”的务实路线。局限在于样本基于公开程序，可能低估真实部署中更复杂程序的验证难度，且缓解策略的评价停留在定性层面。


#### IUBIK: Isolating User Bytes in Commodity Operating System Kernels via Memory Tagging Extensions.
- **作者**: Marius Momeu, Alexander J. Gaidis, Jasper v. d. Heidt, Vasileios P. Kemerlis
- **方向**: 系统与操作系统安全
- **解读**: 传统内核加固思路是保护“安全关键数据”免受破坏与泄露，但建立识别敏感对象的健全模型非常困难，由此衍生出可被攻击者滥用的新攻击向量。本文提出思路转变：通过 IUBIK 对内核内存做分区，把攻击者可控的数据（常被用于操纵内核数据）隔离到影子内存中，阻止其与受保护对象交互，从而从结构上抑制利用。

技术上，IUBIK 利用 ARM CPU 的 MTE（内存标签扩展）硬件特性，同时缓解基于空间与时序的内存错误；通过重写结构体定义，确保被隔离的数据不含指针等字段；并配套开发 profiling 框架深入探索代码库、记录各类分配点，以确定哪些数据需要隔离。

评估方面，profiler 在多样化工作负载中记录了 292 个特权与 212 个非特权分配点；Linux 内核实现经微基准与宏基准测试，在大多数测试中无运行时开销，额外资源消耗可忽略。

我认为本文的贡献在于把内核加固从“识别敏感对象”的脆弱范式转向“隔离攻击者数据”的稳健原则，且与 MTE 硬件趋势契合，实测零开销很有说服力。局限在于结构体重写对内核代码的侵入性、对非 ARM/无 MTE 平台的移植性，以及真实复杂负载下性能的全面验证仍需后续工作检验。


#### SoK: Software Compartmentalization.
- **作者**: Hugo Lefeuvre, Nathan Dautenhahn, David Chisnall, Pierre Olivier
- **方向**: 系统与操作系统安全
- **解读**: 最小权限原则下的软件隔离（compartmentalization）被公认能抑制漏洞利用影响，但历经数十年仍未成为主流工程实践，且术语与分析口径混乱、工作无法比较。本文提出统一模型：把隔离定义为“策略（划分保护域）+ 运行时强制”，并划分三个子问题——策略定义方法（P1）、隔离抽象（P2，CREATE/DESTROY/CALL/RETURN/ASSIGN 原语）、隔离机制（P3）。

基于该模型构建分类法，系统化梳理 2003–2023 年顶会 9,083 篇论文筛出的 211 项研究（106 项入表），并从 Debian 仓库 1,520 个安装量 >1K 的应用中人工筛选出 61 个主流隔离系统（56 个来自 Debian），双向对照研究界与工业界的差距，形成 14 条洞察。

主要发现：主流软件中隔离仍是少数派（<56/1,520），8/13 类程序由学术或安全专业人员开发；61 个系统的策略定义几乎全靠手工（Firefox RLBox 是唯一非全手动 PDM），机制上仅 V8（SFI）和 Firefox 不依赖页表/fork() 遗产机制；生产系统普遍追求可用性而研究界鲜有涉及；OpenSSH、Firefox、Nginx、Chrome 均被报出接口安全（confused deputy）漏洞。结论建议：整体化求解、简化策略定义、强化威胁模型、弥合研究-生产差距。

我的思考：这篇 SoK 的价值在于“统一词汇表”——P1/P2/P3 划分与多维分类让后续工作可比较可定位；研究-生产差距的量化（手工 vs 自动化、可用性缺口、页表依赖）比泛泛而谈更有说服力。局限：论文筛选带主观性、非穷尽，“主流”样本偏 Debian 生态。对领域的影响是纲领性的：把微内核、CHERI、PKU、SFI 等支线收拢到同一坐标轴上，直接指导了“机制无关抽象”“侧信道贯穿全栈”等研究议程。

- **arXiv PDF**: [SoK: Software Compartmentalization.](https://arxiv.org/pdf/2410.08434)

#### CoinDef: A Comprehensive Code Injection Defense for the Electron Framework.
- **作者**: Zheng Yang, Simon P. Chung, Jizhou Chen, Runze Zhang, Brendan Saltaformaggio, Wenke Lee
- **方向**: 系统与操作系统安全
- **解读**: 
Electron 等跨平台框架让开发者用熟悉的 Web 技术构建桌面应用，将 Web 与原生环境融合进单一可执行文件；但这一融合产生独特漏洞并显著扩大攻击面，传统防御无法同时跨两个上下文生效。

方法上，提出 CoinDef 集中式防御机制：在 JavaScript 引擎内部强制执行带执行上下文的抽象语法树（AST）结构完整性，提供快速、防篡改、全面的代码注入缓解；采用混合 profiling 收集 AST 画像以建立预期行为基线，并在运行时对解释执行的代码进行画像比对。

结果显示，在 20 个代表性真实 Electron 应用上的评估表明其能有效阻止利用，启动期运行时开销仅 3.96%，用户交互期开销可忽略；相比 SOTA 方案，能防护通过 DOM 操纵与动态执行的复杂攻击。

我的思考：在 JS 引擎内部做 AST 完整性检查是纵深防御的巧妙位置——攻击者难以在引擎之下绕过；3.96% 的开销可接受，覆盖 DOM 操纵与动态执行扩展了防护面。局限是需要建立基线画像（存在误报/漏报风险），框架版本迭代后的适配成本有待观察。


#### Efficient Storage Integrity in Adversarial Settings.
- **作者**: Quinn Burke, Ryan Sheatsley, Yohan Beugin, Eric Pauley, Owen Hines, Michael Swift, Patrick D. McDaniel
- **方向**: 系统与操作系统安全
- **解读**: 在不信任的存储（公有云、终端设备）上保证完整性（真实性、事务一致性、防回滚）通常依赖 Merkle 哈希树，但树遍历开销极大（大容量下可达 10×/3× 减速）；异步批处理方案（如 FastVer）虽提速，却让应用先拿到未校验数据，产生长达 800ms（10^5 批量）甚至 8 秒（10^6 批量）的漏洞窗口。本文提出混合方案 PAC。

PAC 的核心是“同步校验 + 异步更新”：读路径上，数据返回调用者前必须完成完整性校验（读从不投机，彻底关闭漏洞窗口）；写路径上，更新请求进入安全内存中的共享队列，由后台线程异步执行（支持覆盖合并与速率限制）；FSYNC 时主线程阻塞直到队列排空、Merkle 根密封完成，从而保证根哈希总反映最近一次 FSYNC。系统基于 Dynamic Merkle Tree 实现为 Linux 块设备驱动（BDUS，约 5k 行 C++），并给出读写两条保证的形式化证明。

实验表明：合理配置下 PAC 达到 AEAD（仅加密无完整性）基线吞吐的 85% 以上；相比同步 DMT 有 2.3–2.8× 吞吐提升、256KB I/O 下最高 5.5×、中位延迟改善 2.5×；校验约 2µs/块而更新约 25µs/块，证明“延迟校验”既不安全也无必要；缓存 0.1% 即达最优，队列内存开销仅 1.38×；性能在容量、缓存、偏斜度等维度保持稳定。

我的思考：用“并发而非延迟”化解存储完整性的安全-性能二难，把 FSYNC 周期重新定义为应用侧调优旋钮（>1000 次写入即可近零开销），比牺牲安全性的批处理方案更优雅，论证完整（形式化证明+参数扫描）。局限：fail-stop 崩溃模型下恢复仍是开放问题；重读场景性能收敛于基线 DMT；依赖可信 VM 内存（SEV-SNP 等 TEE）与密封计数器。作为通用块层抽象，它对数据库、启动镜像防降级等场景有直接价值。

- **arXiv PDF**: [Efficient Storage Integrity in Adversarial Settings.](https://arxiv.org/pdf/2504.07041)

#### SoK: Integrity, Attestation, and Auditing of Program Execution.
- **作者**: Mahmoud Ammar; Adam Caulfield; Ivan De Oliveira Nunes
- **方向**: 系统与操作系统安全
- **解读**: 程序执行完整性是运行时安全的基础，控制流完整性（CFI）与执行证明/认证（CFA）是两大主流机制，但二者目标、假设与关系长期缺乏系统梳理，导致防御设计碎片化，也难以判断它们能否在同一平台上共存互补。

作为 SoK 论文，本文系统化梳理 CFI 与 CFA 机制，考察其差异与联系，围绕目标、假设、特性与设计空间等关键问题展开，并对现有防御进行全景式综述，将 CFA 定位在更广阔的运行时防御图景中。

研究批判性评估了各类机制的强度、局限与权衡，结论强调需要进一步研究以弥合空白、推进运行时防御领域的发展。

我的思考：SoK 的价值在于为 CFI/CFA 这一快速膨胀但术语混乱的领域提供统一分类框架，特别是"同一平台共存"的问题切中实际部署痛点（如 TEE 环境中的组合防御）。作为综述，它没有量化实验，深度取决于对文献覆盖的完整性与分类视角的新颖度。CFA 相对 CFI 更年轻，该文将其系统化定位，有望成为该子领域后续工作的引用基点。


#### I know What You Sync: Covert and Side Channel Attacks on File Systems via syncfs.
- **作者**: Cheng Gu, Yicheng Zhang, Nael B. Abu-Ghazaleh
- **方向**: 系统与操作系统安全
- **解读**: 操作系统用进程、容器等抽象做逻辑隔离，但文件系统实现是共享的：syncfs 会冲刷整个文件系统（不仅调用者自己）的脏页、inode 与日志缓存。因此普通无特权进程反复调用 syncfs 并测量延迟，就能推断同盘其他隔离域的写 I/O 行为。与基于 fsync 争用的信道不同，该泄漏不要求受害者调用任何同步调用，只要写文件就会泄漏，信息量更丰富。

先刻画泄漏向量：write 使 syncfs 延迟从基线 2,509 周期升至 121,092，write(O_SYNC)/ftruncate/rename 分别为 41,406/61,315/66,774；并发写数量与延迟近线性（斜率 48,612）；写大小 <4KB 时延迟线性增长、>4KB 因 I/O 并行而放缓。据此构建 TSC 同步的隐蔽信道（高延迟=1、低延迟=0），以及基于 STFT+ResNet-152 的三类侧信道：网页指纹、视频指纹、Android 应用指纹，并扩展到容器间。

隐蔽信道在 XFS 达 4.98 Kbps（错误率 0.15%）、NTFS（WSL）达 7.61 Kbps（1.9%）、ext4 0.94 Kbps（0.01%）；网页指纹（Alexa Top100）闭集 F1 93.82%、开集 93.25%；视频指纹 YouTube 闭集 F1 95.69%（开集 92.74%）、Bilibili 89.68%（开集 87.03%）；Android 15 款应用指纹 F1 93.49%；跨容器隐蔽信道 0.23 Kbps（错误率 2.4%），并能通过 mount/unmount 的 s_umount 锁争用尖峰检测容器启停；5 个 I/O 噪声负载下 SNR 从 15.77 降到 4.75 仍可利用。

我的思考：揭示了“全局冲刷”这一性能优化带来的结构性侧信道，受害者无需执行任何同步调用即可被观测，比 fsync 争用类攻击适用范围广得多；Linux 与 Windows（NTFS）通吃且横跨进程/容器/移动端，评估完整。局限：隐蔽信道带宽仅 Kbps 级，网页/视频指纹在强噪声下 F1 会降到 75–80%，且依赖 CNN 训练；缓解困难——把冲刷限制到调用者域会破坏性能优化本质。作为“共享文件系统即共享计时器”的又一实例，它对容器与移动生态的隔离假设构成现实威胁。

- **arXiv PDF**: [I know What You Sync: Covert and Side Channel Attacks on File Systems via syncfs.](https://arxiv.org/pdf/2411.10883)

#### PEARTS: Provable Execution in Real-Time Embedded Systems.
- **作者**: Antonio Joia Neto, Norrathep Rattanavipanon, Ivan De Oliveira Nunes
- **方向**: 系统与操作系统安全
- **解读**: 
嵌入式设备日益普及并承担安全关键功能，但受成本与能耗约束，通常采用缺乏高级架构安全特性的MCU。此前的低开销架构能生成软件执行证明（PoX），确保传感器数据完整性，但要求被证明的执行原子完成（不可中断），这排除了分时系统与实时应用，与嵌入式系统的可用性需求直接冲突。

本文提出新目标Real-Time Proof（RT-PoX）：保留经典PoX保证的同时适用于分时/实时系统，其实现路径是放宽原子性、容忍同设备上其他恶意任务或操作系统的调度干扰。据此设计PEARTS架构，据作者所知是首个可直接与商品化FreeRTOS部署的PoX方案。

作者在单核ARM Cortex-M33处理器上构建开源原型并部署FreeRTOS，同时支持分时调度与实时应用，评估报告显示开销适中。

首个与真实RTOS集成的PoX是重要的工程突破；但"容忍恶意任务干扰"的信任模型需仔细审视——干扰者能否影响证明的完整性与时序语义。单核原型的结论向多核与更复杂调度场景扩展时仍需验证。与CEAL等超轻量TEE路线相比，PEARTS聚焦实时性这一独特维度，填补了关键空白。


#### Portal: Fast and Secure Device Access with Arm CCA for Modern Arm Mobile System-on-Chips (SoCs).
- **作者**: Fan Sang, Jaehyuk Lee, Xiaokuan Zhang, Taesoo Kim
- **方向**: 系统与操作系统安全
- **解读**: 
现代移动Arm SoC集成大量协处理器与外设，为安全高效的设备I/O带来挑战；现有依赖内存加密的方案带来显著性能与功耗开销，而实时数据处理与严格效率需求使问题加剧，阻碍了机密计算架构（CCA）在移动平台的广泛采用。

本文提出Portal，面向CCA SoC的I/O接口：通过隔离而非加密实现安全——利用CCA机制实施硬件级访问控制，确保只有指定的Realm虚拟机可以访问Portal保护的明文区域。该设计消除了加密相关开销，支持动态外设集成，并保持了可扩展性与效率。

评估结果显示，Portal仅引入最小的一次性9.8%开销，同时提升了可扩展性与效率，成为资源受限环境中推动CCA落地的关键方案。

"用硬件访问控制替代加密"切中了CCA在移动平台落地的核心痛点，设计务实；但明文区域的威胁面（物理攻击、固件漏洞）需要明确边界。与PENGLAI、内存加密等路线相比，Portal更适合资源受限的移动SoC，为机密计算的设备I/O提供了低开销新选择。


#### BadRAM: Practical Memory Aliasing Attacks on Trusted Execution Environments.
- **作者**: Jesse De Meulemeester, Luca Wilke, David F. Oswald, Thomas Eisenbarth, Ingrid Verbauwhede, Jo Van Bulck
- **方向**: 系统与操作系统安全
- **解读**: 
云计算的普及引发对信任与数据隐私的担忧，TEE（如AMD SEV-SNP、Intel TDX、Arm CCA）通过CPU内的透明内存加密与强访问控制保护完整虚拟机，成为规模化部署的信任基础。本文挑战这些大规模TEE的底层假设：攻击者仅需短暂物理接触SPD芯片，即可在地址空间中制造别名（aliasing），绕过CPU的防护机制。

作者设计了实用、低成本的装置，在DDR4与DDR5模块上制造内存别名，破坏SEV-SNP新引入的完整性保证：攻击者可操纵映射、破坏或重放密文，最终以端到端攻击攻破SEV-SNP的认证（attestation）功能。

研究还考察了该问题对其他TEE的影响：在可扩展SGX上发现无噪声的细粒度写模式泄漏，而Scalable SGX与TDX因采用专用别名检测，目前可抵御本攻击。结论动摇了SEV-SNP生态的安全根基，需要固件补丁，并对DRAM与可扩展TEE设计提出新的思考。

把物理攻击（SPD篡改）引入大规模TEE威胁模型意义重大，云厂商需更新信任根与固件；与Rowhammer等物理内存攻击相比，BadRAM篡改的是SPD配置而非内存单元，机制不同。该结果敦促DRAM供应链与TEE设计协同加固，是TEE安全研究的重要警钟。


#### INCOGNITOS: A Practical Unikernel Design for Full-System Obfuscation in Confidential Virtual Machines.

- **作者**: Kha Dinh Duy, Jaeyoon Kim, Hajeong Lim, Hojoon Lee

- **方向**: 系统与操作系统安全

- **解读**: 近年研究反复证明侧信道攻击可破坏 Intel SGX 等 TEE 的机密性保证；同时可信云正转向机密虚拟机（CVM）。不幸的是，许多针对 SGX 的侧信道攻击在 CVM 上依然可行，CVM 架构还引入了新的攻击面。既有防御大多面向保护用户态 enclave（如 SGX），CVM 场景下的混淆引擎设计空间基本空白。

方法上，本文提出 INCOGNITOS，一个面向 CVM 全系统工作负载混淆的 unikernel 设计：彻底拥抱"最小 TCB + 直接硬件访问"原则以使混淆可行，通过重构两个关键操作系统组件，让整个 CVM 的代码与数据形态在运行中持续变化，从而挫败侧信道探测。

结果：摘要在此截断，未给出量化评估数据；从公开内容看，其贡献集中于 unikernel 架构层面的设计决策与两个 OS 组件的重构方式，属于系统设计类工作。

我的思考：与 SGX 时代"应用级混淆/防护"相比，INCOGNITOS 把混淆下沉到整个 CVM 系统层，符合云环境"整机租用、租户不可信"的现实威胁模型，方向正确。但全系统混淆的运行时开销、与现有 CVM 生态（SEV-SNP、TDX）的兼容性、以及混淆对时序类侧信道的实际防御效果，都是需要全文数据支撑的疑点。作为该设计空间的早期探索，其价值更多在于验证"unikernel 是 CVM 混淆的合适载体"这一命题。


### 硬件与物理安全（22 篇）

#### Towards ML-KEM & ML-DSA on OpenTitan.
- **作者**: Amin Abdulrahman, Felix Oberhansl, Hoang Nguyen Hien Pham, Jade Philipoom, Peter Schwabe, Tobias Stelzer, Andreas Zankl
- **方向**: 硬件与物理安全
- **解读**:
OpenTitan 是开源硬件信任根，需要在其中支持高性能的格基密码，即 NIST 推荐标准化的 ML-KEM 与 ML-DSA；其 Big Number 加速器（OTBN）上的基线软件实现性能不足，制约了后量子密码在可信硬件中的落地。

作者先针对 OTBN 精细优化 ML-KEM 与 ML-DSA 软件实现，再基于剖析结果提出紧密的硬件集成方案：为 OTBN 新增与 OpenTitan Keccak 加速器（KMAC core）的接口，并在 ISA 中加入 256 位向量运算支持，从而在硬件层面加速哈希等瓶颈操作。

实现结果显示，ML-DSA 各参数集相对未修改 OTBN 的基线实现获得 6–9 倍加速；面积代价仅为单元数增加不到 17%，约占完整 Earl Grey 内核的 3%，代价可控。

硬件-软件协同设计思路清晰：先剖析软件瓶颈，再有针对性地扩展 ISA 与协处理器接口，加速比显著而面积开销小，论证了在低成本信任根中承载后量子算法的可行性。摘要未给出 ML-KEM 的具体加速数字，是信息上的留白，需以原文为准。总体而言，这项工作为 PQC 在嵌入式信任根中的高效部署提供了可复用的工程路径，对后量子迁移的现实落地有直接推动意义。


#### Security Attacks Abusing Pulse-level Quantum Circuits.
- **作者**: Chuanqi Xu, Jakub Szefer
- **方向**: 硬件与物理安全
- **解读**: 量子程序正从门级转向脉冲级定义以提升表达力与优化能力，但门级与脉冲级之间的接口缺乏安全检查，而脉冲配置是模拟量、随硬件校准频繁变化，几乎无法标准验证。论文首次系统探索该接口的攻击面。

利用自定义门（custom gate）门级描述与实际底层脉冲实现之间的不一致发起攻击：通道攻击（qubit plunder 掠夺、block 阻塞、reorder 重排，源于 SDK 未校验门量子比特与脉冲通道的对应关系，Qiskit 与 Amazon Braket 均受影响）与脉冲攻击（timing/frequency/phase/waveform 失配），并可组合成翻转、纠缠注入、木马等复合攻击；按受害者验证能力把其分为 5 级。

在 127 量子比特 ibm_osaka 真机与模拟器上演示：量子隐形传态中 Eve 可在门级电路完全相同的情况下秘密耦合/解耦量子比特、窃取传输态；Grover 搜索仅修改 4 个门（8 个脉冲）的相位即可把输出重定向到攻击目标态；量子神经网络（MNIST 验证精度 89.6%）参数平均改变 3% 即把精度降至 26.8%，平均 1.8% 改动即可植入后门。

类比 Spectre/Meltdown 的"指令-微架构不一致"，把攻击面定位于量子软件供应链，威胁模型与受害者分层清晰，是目前该方向最系统的探索；但演示规模有限、防御框架偏概念性，未给出可部署的验证工具。随云量子服务商业化，此类低层攻击值得重视。
- **arXiv PDF**: [Security Attacks Abusing Pulse-level Quantum Circuits.](https://arxiv.org/pdf/2406.05941)

#### Mon CHERI: Mitigating Uninitialized Memory Access with Conditional Capabilities.

- **作者**: Merve Gülmez, Håkan Englund, Jan Tobias Mühlberg, Thomas Nyman

- **方向**: 硬件与物理安全

- **解读**: 研究表明 C/C++ 中高达 10% 的内存安全漏洞源于未初始化变量访问，而针对这类未定义行为的软件缓解措施长期缺失。基于能力的寻址（如 CHERI）能缓解众多时空安全缺陷，却无法处理未初始化访问。本文提出 Mon CHERI，扩展 CHERI 能力模型引入"条件能力"，在硬件层面对未初始化访问实施防护。

方法上，作者为 CHERI 能力增加条件语义：基于先前操作历史实施访问策略，强制执行"无读取、除非先有写入"（Write-before-Read）——能力只有被写入后才能读取，未初始化即读在硬件层被拦截。工作包含架构扩展、编译器支持（自动附加条件状态）与完整评估：在 QEMU 全系统模拟器与 FPGA 的 CHERI-RISCV 软核上验证。

评估表明条件能力实用：检测准确率高，开销仅约 3.5%，与基线 CHERI 成本相当——以几乎可忽略的代价解决 10% 的漏洞类别，是硬件防护落地可行性的关键证据。

我的思考：本文把 CHERI 从空间/时间安全延伸到初始化状态安全，用极小硬件代价关闭未初始化访问这一被长期忽视的类别——10% 的占比使问题优先级被低估，3.5% 的开销让修复变得划算。设计上条件能力与既有模型正交，编译器自动插桩降低采用门槛。局限：QEMU/FPGA 评估未覆盖真实 SoC 全栈；对 DMA 等旁路访问可能失效；硬件方案部署周期长于软件方案。与 MSan 等软件方案相比，Mon CHERI 填补了 CHERI 未覆盖的空白，是能力安全架构演进的重要拼图。

#### SoK: Space Infrastructures Vulnerabilities, Attacks and Defenses.
- **作者**: Jose Luis Castanon Remy, Ekzhin Ear, Caleb Chang, Antonia Feffer, Shouhuai Xu
- **方向**: 硬件与物理安全
- **解读**: 空间基础设施对全球社会与经济日益关键，但其网络安全长期研究不足。本文基于新颖的五要素方法论（空间模型、任务、漏洞、攻击、防御）提出 SoK，借助受程序分析启发的“任务控制流”与“数据流”概念，建立空间基础设施的“解剖学”，系统组织该领域的既有知识。

方法上，作者把文献中研究的防御措施映射到五要素框架上，从而导出跨要素的结构化洞见，识别研究密集区与空白区。

关键发现包括：不当内存分配与缺乏认证是报道最多的两类被利用漏洞；全球导航卫星系统（GNSS）的安全研究主要聚焦物理层安全；有效的攻击路径常以地面段为跳板（segment pivot），攻击者通过地面段渗透整个空间系统。

我认为以程序分析视角系统化空间安全是一次高质量的知识整理，映射方法使研究空白一目了然，对后续研究者是实用的路线图。局限在于 SoK 结论依赖文献覆盖范围，“两类最常被利用漏洞”等量化论断的统计口径未充分透明；且空间系统与经典 IT 系统在威胁模型上的差异值得更深入的专门讨论。


#### Space RADSIM: Binary-Agnostic Fault Injection to Evaluate Cosmic Radiation Impact on Exploit Mitigation Techniques in Space.
- **作者**: Johannes Willbold, Tobias Cloosters, Simon Wörner, Felix Buchmann, Moritz Schloegel, Lucas Davi, Thorsten Holz
- **方向**: 硬件与物理安全
- **解读**: 低轨（LEO）卫星的普及使商用现货（COTS）硬件取代了专用抗辐射硬件，卫星成为关键基础设施的同时也暴露于网络攻击与空间特有威胁之下；然而星载固件中的密码保护与利用缓解措施仍然有限，且学术研究只关注缓解本身，鲜有人问：这些缓解策略是否适合太空环境、是否受宇宙辐射等因素影响。本文给出首个系统分析。

方法上，作者首先系统分析 381 个小型设计样本，梳理缓解方案在各平台与项目中的普及度及现成可用平台；随后提出 RADSIM——一个二进制无关的自动化工具，通过模拟单事件错误（bitflip）进行故障注入，对采用不同加固方式的固件二进制执行超 21 亿次故障模拟，评估其容错表现。

结果显示：部分缓解方案几乎不影响固件的容错性，而另一些方案会把错误概率提高最多 19%，揭示了安全有效性与辐射韧性之间的微妙权衡。

我认为把“辐射可靠性”引入“安全缓解”的评估是跨域新视角，21 亿次故障注入的规模提供了统计上扎实的证据，对空间系统开发者有直接指导价值。局限在于 bitflip 模型简化了真实辐射效应（多比特翻转、瞬态时序等），381 个样本的代表性也需审视；该工作为“太空安全+可靠性”交叉方向奠定了量化基础。


#### Slice+Slice Baby: Generating Last-Level Cache Eviction Sets in the Blink of an Eye.
- **作者**: Bradley Morgan, Gal Horowitz, Sioli O'Connell, Stephan van Schaik, Chitchanok Chuengsatiansup, Daniel Genkin, Olaf Maennel, Paul Montague, Eyal Ronen, Yuval Yarom
- **方向**: 硬件与物理安全
- **解读**: 缓存侧信道攻击（Prime+Probe 等）的前提是构造驱逐集。Intel LLC 采用分片设计，地址映射到哪个片由基于物理地址位的私有哈希决定，攻击者在虚拟地址下不知道高位物理地址位；且片数非 2 的幂时哈希非线性，无法像线性函数那样把驱逐集直接平移到其他页偏移。本文给出三项贡献解决该问题。

其一，提出 comparator gate——新的“怪诞门”（weird gate），利用乱序执行投机撤销留下微架构痕迹的特性让两个内存访问“赛跑”比较延迟，从而判定地址所属 LLC 片（RDTSCP 与固定延迟链在噪声下精度掉到 67%/32%，comparator gate 静/忙系统下保持 96%/93%，跨核平均 97%）；其二，利用哈希函数结构（线性每页 n 种映射、非线性每页 128 种），提出 closest match、贝叶斯推断、决策树三种页内映射传播，只需测量少量偏移即可推断整页映射并纠错；其三，首次对非线性哈希实现跨页偏移驱逐集传播，10 片处理器只需常规构造 15%、6 片只需 22% 的驱逐集，另加候选集片过滤与测试驱逐过滤（待测从 128/320 减到 2）。

在 8 款 Intel 处理器上验证：i7-9850H 生成整个 LLC 驱逐集仅需 0.7 秒、i9-10900K 上 1.6 秒，比 L2CS、Prune+PlumTree 等最新技术快 9×/10×；线性哈希处理器上也快 1.1–1.5×；全程仅需非特权 4KB 页面、无需大页、不关预取器。

我的思考：把“怪诞门”从纯研究推向实用攻击原语的精彩工程——用微架构竞速替代时钟测量抗噪，思路新颖、量化充分；三项优化环环相扣，把驱逐集生成从分钟级压到亚秒级，直接降低缓存侧信道门槛，也意味着以驱逐集难度为防御评估指标的设计需重新审视。局限：需离线逆向片函数、profile 依赖运行核心，12900KF/14900K 上决策树效果差需退回贝叶斯。

- **arXiv PDF**: [Slice+Slice Baby: Generating Last-Level Cache Eviction Sets in the Blink of an Eye.](https://arxiv.org/pdf/2504.11208)

#### Rapid Reversing of Non-Linear CPU Cache Slice Functions: Unlocking Physical Address Leakage.
- **作者**: Mikka Rainer; Lorenz Hetterich; Fabian Thomas; Tristan Hornetz; Leon Trampert; Lukas Gerlach; Michael Schwarz
- **方向**: 硬件与物理安全
- **解读**: 微架构攻击威胁日益增长，CPU 缓存是许多攻击的核心组件，理解其内部机制至关重要。尽管逆向技术不断进步，非线性缓存分片函数仍难以分析，尤其在 Intel 最新混合微架构上；这阻碍了分片测量与物理地址泄露类攻击的开展。

本文提出针对复杂非线性分片函数的新型逆向方法：通过理解哈希的具体结构，把逆向时间从数天缩短到数分钟，并能处理 512 GB 内存系统与多样化的分片配置。作者识别出 13 个新发现的寻址用分片函数，将支持范围扩展到更多 DRAM 与多代产品；并顺带获得一个非特权虚拟地址到物理地址的预言机。该方法在 Alder Lake 与 Meteor Lake 上有效——此前这些 CPU 上测量分片或泄露物理地址的手段均不可用。

三个案例研究验证了有效性：对非攻击者映射内存执行定向 Spectre 攻击、启用 DRAMA 攻击、构建缓存驱逐集；研究强调这些发现扩大了攻击面。

我的思考：从"天"到"分钟"的逆向提速是质变，让最新混合架构 CPU 也进入可攻击范围；13 个新分片函数与地址预言机为后续攻击提供公共基础设施。Alder Lake/Meteor Lake 上首次实现物理地址泄露，直接威胁依赖地址隐藏的防御（如非攻击者映射内存）。局限：结果高度依赖厂商哈希结构，Intel 更新分片函数即可失效；"分钟级"逆向仍需针对每款新 CPU 重做。相比此前仅能处理线性/简单哈希的工作，这是对非线性场景的系统性突破。


#### Breaking the Barrier: Post-Barrier Spectre Attacks.
- **作者**: Johannes Wikner; Kaveh Razavi
- **方向**: 硬件与物理安全
- **解读**: 瞬态执行防御的有效性依赖一些晦涩的模型特定操作，它们必须在微码中正确实现并被软件正确应用。IBPB（间接分支预测屏障）是抵御跨上下文、跨特权 Spectre 攻击的基石防御，本文发现其在实现与应用层面均存在新漏洞，并演示了 Intel 与 AMD CPU 上的两种屏障后推测性返回目标劫持。

其一，端到端跨进程攻击可从 suid 进程泄露 root 密码哈希——由于实现缺陷，该攻击在近几代 Intel CPU 上即使启用 IBPB 依然有效；其二，非特权攻击者可在 Zen 1(+)/2 上绕过已部署的缓解措施泄露特权内存，并展示了 Linux 内核中的利用链。

作者提出用"鸡尾位"（chicken bit）禁用受影响 CPU 上可利用的预测，并给出软件补丁方案供内核安全使用。

我的思考：IBPB 长期被视为跨域 Spectre 的标准防线，本文证明它既可能在微码实现层面失效（Intel），也可能在软件应用层面被绕过（AMD Zen），警示"屏障类防御的信任假设"不可靠。跨进程泄露 root 密码哈希是完整的现实攻击，严重性高。鸡尾位+内核补丁的修复路径务实，但这类逐代修补的军备竞赛模式代价高昂。与 BHI 等近期工作一起，表明分支预测屏障的根基需要重新审视，防御纵深而非单点屏障才是出路。


#### Peek-a-Walk: Leaking Secrets via Page Walk Side Channels.
- **作者**: Alan Wang; Boru Chen; Yingchen Wang; Christopher W. Fletcher; Daniel Genkin; David Kohlbrenner; Riccardo Paccagnella
- **方向**: 硬件与物理安全
- **解读**: 微架构侧信道是程序安全的重大威胁。新兴一类攻击构造"解引用数据内存内容"的 gadget——推测执行与数据内存预取（DMP）等优化会错误猜测并执行指针追逐。理论上这极具破坏性，但实践中大多数秘密不像合法指针，解引用往往失败，因此并不泄露信息。

本文提出页走查信道（PWSC）：即使解引用无效也能泄露信息。具体地，给定一个通过地址规范性检查的 64 位地址，PWSC 无需对秘密形态做任何假设即可泄露除低 6 位外的全部位——泄露发生在页表走查过程本身，而非数据缓存。

作者演示了 PWSC 在预取场景中如何放大泄露：结合 Intel 的 LAM 特性可达到近乎物理地址的泄露，并据此恢复 Dilithium 密钥；同时逆向出依赖数据的预取器（DMP）的语义，并展示 PWSC 能打破进程内沙箱的安全边界。

我的思考：把侧信道从"秘密必须长得像指针"的约束中解放出来，是概念层面的重要突破——PWSC 通过页表走查过程泄露地址内容，绕过了此前攻击的根本限制。与 Intel LAM 的组合导致物理地址级泄露并威胁 Dilithium 这类后量子签名，时机敏感；DMP 语义的逆向工作也独立有价值。局限：需通过规范性检查的地址前提、以及低 6 位无法泄露的残余面。相比纯缓存类侧信道，它打开了"无效解引用"这一新信道维度。


#### SLAP: Data Speculation Attacks via Load Address Prediction on Apple Silicon.
- **作者**: Jason Kim; Daniel Genkin; Yuval Yarom
- **方向**: 硬件与物理安全
- **解读**: 自 2018 年 Spectre 公开以来，硬件难以完全缓解推测执行攻击，导致六年里涌现大量变体与新攻击面；绝大多数变体沿袭原版思路——CPU 在缓解分支冒险时错误执行控制流，通过侧信道瞬态泄露秘密。本文超越（推测性）控制流影响，提出源于"为消除数据冒险而设计的微架构优化"的数据推测原语。

作者发现 Apple CPU 配备负载地址预测器（LAP）：LAP 监控同一 load 指令的历史地址并推测预测地址，该地址可能错误地指向任何位置（即使架构上从未被 CPU 读取）。秘密被取回后，足够大的推测窗口使攻击者能计算秘密并通过隐蔽信道泄露。LAP 存在于 M2、A15 及更新型号。

安全影响评估显示，该原语可实现越界读取、调用流氓函数、攻破 ASLR，并危及 Safari 浏览器——远程 Web 攻击者可借此窃取跨站敏感数据（如 Gmail 收件箱内容）。

我的思考：数据推测（而非控制流推测）是 Spectre 家族的新维度，LAP 这种"为性能而生"的组件成为攻击原语，再次印证优化与安全的结构性冲突。Safari 端到端攻击（跨站窃取 Gmail）把影响落到真实用户场景，严重性高；Apple 广泛部署的 M 系列芯片扩大了暴露面。局限：攻击依赖 LAP 预测命中率与窗口大小，跨进程场景的具体成功率需全文确认；且 Apple 生态的封闭性使修复与披露节奏更受关注。这是对"Apple Silicon 更安全"叙事的有力反驳。


#### PQ-Hammer: End-to-End Key Recovery Attacks on Post-Quantum Cryptography Using Rowhammer.
- **作者**: Samy Amer; Yingchen Wang; Hunter Kippen; Thinh Dang; Daniel Genkin; Andrew Kwong; Alexander Nelson; Arkady Yerukhimovich
- **方向**: 硬件与物理安全
- **解读**: 后量子密码（PQC）临近标准化与部署，理解入选方案实现的安全性至关重要。本文针对 NIST PQC 竞赛多个决赛方案展开调查，发现令人担忧的结果：Rowhammer 攻击可作用于 Kyber 与 BIKE 密钥封装机制（KEM）以及 Dilithium 数字签名方案，实现完整密钥恢复，且仅需中等程度的攻击努力——无需超级计算机或数月预计算。

作者将 Rowhammer 与性能降级、内存布局按摩（memory massaging）等技术组合，实验性地完成了攻击，证明其在现实中可行。

结果显示基于 Rowhammer 的侧信道攻击对新密码方案标准化、标准制定与实例部署都是必须正视的关键关切；论文最后给出用于加固实现、抵御 Rowhammer 攻击的建议。

我的思考：PQC 的安全性论证长期聚焦代数/格困难问题，而本文把注意力拉回实现层——Rowhammer 造成的位翻转可系统性破坏 Kyber/BIKE/Dilithium 的密钥材料，且"中等努力"即可完成，直接挑战"PQC 默认更安全"的直觉。在标准化进程中的时间点发表，影响面广。局限：攻击需要本地物理访问或特定多租户条件（Rowhammer 前置），云场景的实际可达性需评估；与纯粹数学攻击相比，其威胁模型更依赖部署环境。其加固建议对即将落地的 PQC 迁移具有直接指导价值。


#### Half Spectre, Full Exploit: Hardening Rowhammer Attacks with Half-Spectre Gadgets.
- **作者**: Andrea Di Dio; Mathé Hertogh; Cristiano Giuffrida
- **方向**: 硬件与物理安全
- **解读**: 近十年过去，业界与学界仍未找到针对 Spectre 与 Rowhammer 这类顽固硬件漏洞的全面高效对策。软件侧 Rowhammer 缓解尤其困难，因为攻击现实性常取决于（不）可行性：有些攻击完全非确定，靠随机位翻转赌运气破坏受害者数据，还冒着崩溃系统的风险；更可靠的方法依赖内存模板化与按摩，但面对复杂的内存管理抽象很难做到完全确定。

本文证明完全确定性可行：利用与 Rowhammer 的协同效应，聚焦所谓的"半 Spectre gadget"——这些 gadget 在上一代 CPU 上被认为因无法直接泄露秘密而不可利用，但作者发现它们能构建强大原语来加固其他攻击。具体地，用它们构建 Preload+Time：一种通用原语，无需与受害者共享内存即可在缓存行粒度监视受害者物理内存活动；进而构造 ProbeHammer——首个无需模板化或按摩的、无崩溃的端到端 Rowhammer 攻击。

细节上，攻击者喷射攻击者（即用户）页表并通过 Preload+Time 定位安全锤击模式，避免触发意外位翻转导致崩溃。评估确认 ProbeHammer 零误报（因而零崩溃），可在数小时内攻陷系统。

我的思考：把"不可利用的 gadget"重新定义为"可用原语"，思路巧妙——Preload+Time 提供确定性受害者内存活动反馈，把靠运气攻击变为可规划流程；"首个无崩溃端到端攻击"直接回应了"攻击不现实"的缓解论点。零误报+数小时的数据扎实。局限：需本地环境与精确页表喷射；对 ECC 内存等硬件防御的对抗性未涉及。该工作显著提高了 Rowhammer 威胁的现实权重。


#### Training Solo: On the Limitations of Domain Isolation Against Spectre-v2 Attacks.
- **作者**: Sander Wiebing; Cristiano Giuffrida
- **方向**: 硬件与物理安全
- **解读**: Spectre-v2 攻击日益强势，可发起强大的跨域瞬态执行攻击：攻击者在一个保护域（如用户进程）训练间接分支预测器，在受害域（如内核）中推测性劫持控制流并泄露数据。作为回应，厂商部署 eIBRS、IBPB 等强隔离技术防止跨域污染；BHI、Post-barrier Spectre 等近期攻击暴露的仅是（现已修补的）实现缺陷，业界普遍假设：排除实现问题后，隔离机制在实用场景中已关闭攻击面。

本文挑战这一假设：即使隔离完美也不足以防御。作者系统分析"自我训练"——训练与推测性控制流劫持发生在同一（受害）域内。这种场景此前被认为仅限域内攻击（攻击者需注入自己的泄露 gadget，如默认关闭的 eBPF），但作者的分析显示实践中存在多种变体。

具体地，作者描述了针对 Linux 内核的三类新攻击，并给出两个端到端 exploit，在 Intel CPU 上以最高 17 KB/s 的速度泄露内存；调查中还发现了完全破坏（用户、客户机、虚拟机监控器）隔离、重新启用经典攻击的问题。

我的思考：这是对"域隔离足以防御 Spectre-v2"共识的正面证伪——自我训练使攻击完全绕开 eIBRS/IBPB 的跨域假设，17 KB/s 的泄露速率也相当可观。三类攻击+两个端到端 exploit 的完整度高；附带发现的隔离破坏问题进一步抬高了严重性。局限：需域内任意代码执行（如受控 eBPF 或用户态注入）作为前置，且修复方向（预测器细粒度隔离）成本高昂。该工作推动防御思路从"隔离"转向"消除可训练共享状态"。


#### Scheduled Disclosure: Turning Power into Timing Without Frequency Scaling.
- **作者**: Inwhan Chun; Isabella Siu; Riccardo Paccagnella
- **方向**: 硬件与物理安全
- **解读**: 功耗侧信道攻击近期在安全研究中复兴，新兴一类攻击通过远程手段监视功耗——最典型的是观察功耗导致的 CPU 频率变化。但既有演示仅覆盖（较老的）x86 架构，那里频率缩放是（近乎唯一的）维持系统安全运行的手段；现代 CPU 配备了更复杂的机制，此类攻击是否仍然可行此前并不清楚。

本文证明：功耗侧信道在现代架构上不仅依然可行，而且即使在无频率缩放泄露的情况下也有效。攻击利用 Intel 的 Thread Director——一项根据处理器功耗提供调度"提示"以提升性能与能效的硬件优化：这些提示依赖处理器功耗，导致可观察的行为（如活动核心数量的变化），可纯粹通过软件远程时序分析捕获。

效果上，作者从常数时间密码学代码中泄露密钥（比此前在较老 CPU 上的攻击快 5 倍），并成功发起跨源像素窃取攻击。

我的思考：功耗侧信道通常被认为需要物理探针或高频测量，本文把"调度器提示"变成远程功耗观测窗口，扩展了攻击的现代适用面——即使 Intel 修复频率缩放泄露，Thread Director 依然泄露功耗信息，说明"功耗→可观测行为"的转换机制不止一种。常数时间代码被 5× 更快攻破，对侧信道防护实践有直接警示。局限：跨源像素窃取依赖浏览器渲染行为，防御者可通过关闭 Thread Director 提示或加噪缓解；但作为"新信道"发现，其方法论价值大于单一攻击。


#### Your Cable, My Antenna: Eavesdropping Serial Communication via Backscatter Signals.
- **作者**: Lina Pu, Yu Luo, Song Han, Junming Diao
- **方向**: 硬件与物理安全
- **解读**: 
串行通信广泛用于设备间数据交换，本文提出一种名为Backscattering Through Cable（BTC）的新型背散射侧信道攻击，实现低成本、有效的串行数据窃取。其物理原理是：端口传输不同比特（'0'与'1'）时阻抗发生变化，进而引起背散射信号幅度波动，线缆本身充当非故意天线，使敏感数据经侧信道泄露并可被远程截获。

BTC的突出特点是要求极低：无需修改目标设备的硬件或软件，也无需事先了解设备的通信配置。为加深机理理解并优化参数，作者还建立了全波模型，刻画设备长度、载波频率等参数对攻击效能的影响。

实验验证在视距（LOS）场景下最远14.5米、非视距（NLOS，含两堵墙）场景下4.5米可实现成功窃取；速率可达1 Mbps以上，并适用于多种线缆类型，线缆短至4厘米亦可工作。仿真结果还表明攻击在特定条件下可保持有效。

极低门槛（免植入、免配置知识）使该攻击对隔离网络构成现实威胁，但传输距离与信道环境限制明显，实际部署受背景噪声制约。与电磁辐射窃听相比，BTC利用的是反射信号而非辐射，机理不同，值得安全评估体系纳入此类物理侧信道。


#### EUCLEAK Side-Channel Attack on the YubiKey 5 Series (Revealing and Breaking Infineon ECDSA Implementation on the Way).
- **作者**: Thomas Roche
- **方向**: 硬件与物理安全
- **解读**: 
本文揭示Infineon Technologies（最重要的安全元件制造商之一）密码库中的侧信道漏洞：非恒定时间的模逆运算。该漏洞潜伏14年，约80次最高级别的Common Criteria（CC）认证评估均未发现，说明认证流程存在系统性盲区。

攻击仅需物理访问下的少量本地电磁采集（几分钟即可），即可提取ECDSA私钥；攻击者在YubiCo的FIDO硬件令牌上完成了演示，能够克隆设备。

Yubico已确认所有YubiKey 5系列（固件版本低于5.7）受影响；此外有强有力证据表明，更多运行易受攻击代码的Infineon安全微控制器（包括TPM）同样存在风险。

该工作揭示了安全认证评估在恒定时间性质覆盖上的系统性盲区——约80次最高级别CC认证均未发现该缺陷，影响面横跨大量认证级设备；与Minerva等ECDSA侧信道攻击相比，本文更突出"未公开即长期潜伏"的威胁特征，也说明侧信道评估需要更强的测试深度而非仅依赖形式化流程。修复依赖固件更新，存量设备风险将长期存在，对安全元件供应链的验证流程提出了深刻反思。


#### Tiktag: Breaking ARM's Memory Tagging Extension with Speculative Execution.
- **作者**: Juhee Kim, Jinbum Park, Sihyeon Roh, Jaeyoung Chung, Youngjoo Lee, Taesoo Kim, Byoungyoung Lee
- **方向**: 硬件与物理安全
- **解读**: ARM MTE（ARMv8.5-A）为每16字节内存分配4位随机标签，随机标签方案最大检测率15/16，被普遍视为C/C++内存安全最有希望的路径；若攻击者能获知目标地址的标签，概率性防御即被绕过，攻击成功率趋近100%。Google Project Zero此前认为投机执行下标签检查不产生可观测的缓存差异。

作者设计标签泄露模板（分支误预测+CHECK猜测指针触发标签检查+TEST探测指针更新缓存）与模板驱动模糊器，在Pixel 8/Pixel 8 Pro上发现两类gadget：TIKTAG-v1利用标签检查故障导致的投机执行收缩与数据预取抑制（需CHECK内≥2次加载、TEST距CHECK>10周期），TIKTAG-v2利用标签检查故障阻断store-to-load转发（store与load需在5条指令内），并发现链表遍历变体；消融实验确证根因后提出投机屏障、指令填充等软硬件缓解。

实验显示标签泄露成功率>95%、耗时<4秒；V8引擎中单次泄露100%（3.04秒），线性溢出98%、UAF 97-98%；Chromium应用中泄露95%（2.54秒）、溢出97%（16.11秒）、UAF 95%（21.90秒）；Linux内核（Android 14 v5.15）中定位到snd_timer_user_read内的TIKTAG-v1 gadget。已向ARM（2023年12月公开披露）、Chrome安全团队及Android安全团队（获赏金）报告。

我的思考：该工作推翻了"MTE标签在投机执行下不可观测"的假设，证明微架构优化（故障触发投机收缩）本身会引入侧信道，对硬件设计有警示意义。缓解策略（保持标签检查结果的行为不变性）成本与收益需权衡；Chrome默认未启用MTE使影响受限，但随着MTE普及，此类攻击的实用性将持续上升。

- **arXiv PDF**: [Tiktag: Breaking ARM's Memory Tagging Extension with Speculative Execution.](https://arxiv.org/pdf/2406.08719)

#### Ceviche: Capability-Enhanced Secure Virtualization of Caches.
- **作者**: Arnabjyoti Kalita, Yilong Yang, Alenkruth Krishnan Murali, Ashish Venkat
- **方向**: 硬件与物理安全
- **解读**: 
现代系统广泛使用资源虚拟化以提高硬件利用率、降低总拥有成本，但共享物理资源必然打开侧信道利用之门：即使应用不共享数据，共址攻击者仍可经由缓存容量等共享资源的竞争窥探受害者的行为或窃取私有信息，因为缓存查找是数据/地址相关的。

本文提出Ceviche，一种支持不同信任域线程间安全缓存分配的新策略：将"地址+域ID"对翻译为能力（capability）——编码缓存行访问权限与允许的操作集合；通过按能力约束缓存操作，实现行粒度的细粒度分区，在最大化利用率的同时强制机密性、可用性与公平性保证。

论文给出详细的设计、机制、策略与优化，并在现代多核缓存层级上评估可行性：相比不安全基线，全层级缓存保护平均性能损失仅2.4%，相比SOTA的Mirage/ScatterCache仅多1.8%。

把能力思想引入缓存虚拟化是优雅的设计，行粒度在安全与利用率之间取得良好平衡；但硬件改动成本与对现有缓存协议栈的兼容性仍需进一步评估。与ScatterCache的随机化混淆路线相比，Ceviche走确定性强制路线，二者可互补，为缓存侧信道防御提供了新范式。


#### Guardain: Protecting Emerging Generative AI Workloads on Heterogeneous NPU.

- **作者**: Aritra Dhar, Clément Thorens, Lara Magdalena Lazier, Lukas Cavigelli

- **方向**: 硬件与物理安全

- **解读**: LLM 驱动下生成式 AI 已成为现代云上的主导负载，GPU/NPU/TPU 等专用加速器凭借远超通用 CPU 的性能被广泛部署，但其中流转的数据往往来自互不信任的多方、高度敏感。现有工业级 CPU 侧 TEE（Intel SGX、AMD SEV）无法为加速器提供保护；设备级 TEE 如 Nvidia-CC 仅针对紧耦合的 CPU-GPU 系统，且是依赖宿主机侧 TEE 的专有方案；学术界的既有设计又只针对特定 CPU-TEE 平台。异构 NPU 上的机密计算缺少通用方案。

方法上，本文提出 Guardain，一个面向异构 NPU 的机密计算架构，把可信执行边界扩展到专用加速器：处理 CPU 与 NPU 之间的内存隔离、密钥管理与数据搬运路径上的信任问题，使异构加速集群上的生成式 AI 推理负载也能获得与 TEE 相当的保护。

结果：摘要文本在此处被截断（"confidential computing architecture discret…"），未提供可引用的量化结果；公开信息主要覆盖动机与设计目标层面，以架构设计与安全性论证为主。

我的思考：该工作瞄准了机密计算的真实缺口——生成式 AI 负载高度依赖加速器，而现有 TEE 栈的信任根仍停留在 CPU。与 Nvidia-CC 等厂商专有方案相比，面向异构 NPU 的通用架构更具生态意义，但其成色取决于后续对性能开销与安全保证的实证；摘要截断使读者暂无法评估其实现深度，需结合完整论文判断。


#### Eyes on your Typing: Snooping Finger Motions on Virtual Keyboards.

- **作者**: Sunwoo Lee, Wonsuk Choi

- **方向**: 硬件与物理安全

- **解读**: AR/VR 技术快速普及，头戴式显示器（HMD）通过头部与手部追踪让用户与虚拟对象交互，虚拟键盘正成为主要输入方式——用户直接用手"打字"，无需额外设备。但这种直接手打输入引入新的安全风险：打字时手指的细微运动可能无意间泄露隐私信息。

方法上，本文提出 SNOOPFINGER，一种新颖的侧信道攻击：利用 HMD 可获取的手部运动数据，从虚拟键盘输入过程中的手指运动模式推断用户输入内容，实现远程窃取密码等敏感信息。

结果：摘要截断，未给出量化结果；从公开内容看，攻击以运动传感器数据为信道，属于对 VR 输入法的被动侧信道窃听，无需物理接触目标设备。

我的思考：VR 输入安全是新兴且被低估的问题——HMD 为了交互体验必然采集高精度手部追踪数据，而应用层（甚至恶意 VR 应用或网页）可合法获取这些数据，形成天然的隐蔽信道。与智能手机触摸屏侧信道（键盘记录、加速度计推断输入）相比，VR 手部追踪数据的维度更高、可区分性更强，威胁更甚。本文的贡献在于证明该威胁的可行性；局限在于虚拟键盘布局的多样性、不同 HMD 追踪精度差异会影响攻击泛化性，其实际准确率需全文数据佐证。


#### BPSniff: Continuously Surveilling Private Blood Pressure Information in the Metaverse via Unrestricted Inbuilt Motion Sensors.

- **作者**: Zhengkun Ye, Ahmed Tanvir Mahdad, Yan Wang, Cong Shi, Yingying Chen, Nitesh Saxena

- **方向**: 硬件与物理安全

- **解读**: 血压（BP）是多种疾病最重要的生物标志物之一，被 HIPAA 视为受保护的健康信息，通常需要用户同意才能访问。本文揭示元宇宙使用中的一个隐蔽隐私泄露：攻击者可从未受限的 VR 头显运动传感器中秘密获取用户的血压。

方法上，核心洞察是：动脉中的血液波动会在身体主要动脉处引起细微振动，这些振动与用户的心跳周期和血压高度相关；VR 头显内置运动传感器能捕捉这些振动。攻击者可连续无感获取传感器数据，进而推导血压，再通过应用或网站加以利用，造成歧视、剥削、定向骚扰等更严重后果。作者实现了 BPSniff 攻击原型验证该泄露路径。

结果：摘要未给出量化数字；研究以可行性演示为主，展示从 VR 头显运动传感器到血压估计的完整攻击链（精度等评估见论文全文）。

我的思考：本文与 SNOOPFINGER 同属"VR 传感器滥用"研究浪潮，但威胁对象从输入升级为生理信号，社会危害更直接（健康歧视、保险拒保等）。其技术可行性建立在传感器灵敏度与信号处理之上，头显佩戴位置、个体差异都会影响血压估计精度，需谨慎看待准确率。这类工作最大的贡献是敲响平台治理警钟：VR 平台必须限制传感器数据的访问粒度。与既往基于手机加速度计的生理信号推断相比，VR 头显贴身佩戴使信号质量更高，攻击更隐蔽。


#### EveGuard: Defeating Vibration-based Side-Channel Eavesdropping with Audio Adversarial Perturbations.
- **作者**: Jung-Woo Chang, Ke Sun, David Xia, Xinyu Zhang, Farinaz Koushanfar
- **方向**: 硬件与物理安全
- **解读**: mmWave雷达、加速度计、光学传感器等振动类侧信道可隔墙窃听扬声器语音；现有防御（干扰、智能反射面、振动马达）昂贵、覆盖有限或造成不适。EveGuard提出纯软件防御：在播放前向音频注入对抗扰动，保护语音隐私而不影响人耳听感。

核心洞察是侧信道与麦克风/人耳的感知差异：侧信道只捕获低频振动（雷达在>1-2kHz时SNR趋近0dB、加速度计上限约250Hz），而人耳对<500Hz不敏感。据此设计两阶段扰动生成器PGM：FIR生成器（VAE-GAN在频域生成多样化扰动）与LFAP生成器（<500Hz低幅对抗扰动，SNR归一化限制幅度）；并用Eve-GAN（few-shot无配对音频→窃听信号翻译网络，含内容/域编码器与AdaIN解码器）作为可微层实现端到端训练，集成多个替代模型增强迁移性，以判别器+随机隐向量对抗自适应攻击。

对mmWave基线攻击：MCD从3.4升至13.6、WER从9.2%升至70.1%、数字分类成功率从96%降至2%，PESQ 3.42（WER上优于高斯噪声5.3×、VAP 4.4×）；在距离、角度、音量（85dB时WER 66.8%）、隔音材料、反射材料（WER最高85.5%）、60/77GHz频率、8/12kHz采样、天线配置等变化下保持稳健；光学传感器WER 73.2%、加速度计WER 88.6%（167-200Hz下>92%）；24人用户研究确认扰动不可感知。

我的思考："感知差异即攻击面"的洞察很优雅——在人耳听不见的低频注入正中侧信道要害的扰动，是少数纯软件、可扩展的侧信道防御。局限：只保护播放源，无法防御针对人体声带振动的窃听；few-shot翻译虽缓解数据采集开销，但72种场景×120样本的构建仍不轻量，且防御效果依赖对侧信道频率响应的先验假设。
- **arXiv PDF**: [EveGuard: Defeating Vibration-based Side-Channel Eavesdropping with Audio Adversarial Perturbations.](https://arxiv.org/pdf/2411.10034)

### 移动与物联网安全（7 篇）

#### BaseBridge: Bridging the Gap Between Over-the-Air and Emulation Testing for Cellular Baseband Firmware.
- **作者**: Daniel Klischies, Dyon Goos, David Hirsch, Alyssa Milburn, Marius Muench, Veelasha Moonsamy
- **方向**: 移动与物联网安全
- **解读**: 基带固件模拟因对现代基带复杂外设（DSP、SIM 卡、射频前端）支持不足而远逊于空口（OTA）测试；改善需要深度逆向工程、极其耗时，导致进展缓慢，模糊测试也只能发现较浅的 bug，因为无法触及大部分基带功能所需的状态。本文提出 BaseBridge，通过从真实设备内存转储恢复相关状态，实现远更全面的行为模拟。

方法上，原型实现对两家主流厂商（MediaTek 与 Samsung）的固件提供支持，这与现有最先进模拟器形成对比；状态恢复让模拟器能到达以往无法到达的深层代码路径。

结果显示：BaseBridge 对 97% 的测试 RRC/NAS 消息给出正确响应，覆盖率平均提升 2.41 倍（Samsung）与 5.54 倍（MediaTek），并通过多项 LTE 一致性测试；模糊测试整体效率提升 2.3–5 倍、定向功能探索提升 9.0–22.5 倍；并发现 5 个新漏洞，已向受影响厂商披露。

我认为“状态恢复”直击基带模拟真实度的瓶颈，97% 消息正确率与数倍覆盖率提升的工程成果扎实，对基带安全测试社区是即插即用的资产。局限在于依赖内存转储的可得性、仅覆盖两家厂商；与状态感知模糊类工作互补，指向“模拟+OTA 混合”的更完整测试路线。


#### Stateful Analysis and Fuzzing of Commercial Baseband Firmware.
- **作者**: Ali Ranjbar, Tianchang Yang, Kai Tu, Saaman Khalilollahi, Syed Rafiul Hussain
- **方向**: 移动与物联网安全
- **解读**: 基带固件在蜂窝通信中扮演关键角色，但其专有、闭源的特性和复杂有状态的处理逻辑使系统性安全测试极具挑战；现有方法常忽视基带任务间的相互依赖与输入逻辑的状态性，限制了测试的范围与效果。本文提出模糊框架 Loris，用于有效探索和分析基带实现。

方法上，Loris 采用迭代符号分析，逐步识别定义不同协议状态的状态变量及其上的谓词，同时缓解路径爆炸问题；这使 Loris 能对漏洞潜力高的程序区域进行定向探索与模糊测试，把精力集中在最可能出问题的状态转换上。

评估方面，Loris 在来自两家主流厂商的 5 台商用设备上验证，覆盖 4G LTE 与 5G NR，展示了广泛的适用性；共发现 7 个可由空口（over-the-air）攻击者利用的新漏洞，可能导致崩溃、远程代码执行或拒绝服务。

我认为把“协议状态”作为一等公民纳入基带模糊测试是该方向的重要推进，迭代符号分析在缓解爆炸的同时保持精度，7 个 OTA 可利用漏洞证明了实战价值。局限在于迭代符号分析在大规模固件上的扩展性、商用设备样本量有限；与 BaseBridge 的“状态恢复”思路互为补充，两者结合或能进一步提升基带测试的深度。


#### "We can't Change it Overnight": Understanding Industry Perspectives on IoT Product Security Compliance and Certification.
- **作者**: Prianka Mandal, Adwait Nadkarni
- **方向**: 移动与物联网安全
- **解读**: 监管机构与标准组织相继提出 IoT 产品安全合规与认证倡议，试图以认证带来安全保障；但即使是已认证的产品仍频繁暴露常见漏洞，说明认证生态系统存在潜在的深层障碍，而这些障碍此前未被系统研究。

方法上，本文开展首项针对 IoT 产品认证的质性访谈研究：访谈17位从业者，了解其对产品安全认证的行业视角与经验，并用反思性主题分析提炼阻碍认证有效执行的因素。

结果显示，研究提炼出16项关键发现，归纳为4大主题，代表要使 IoT 认证切实可行必须弥合的差距；这些主题揭示了认证执行与执法实践中的关键断层，说明"合规不等于安全"。

我认为该工作的价值在于首次以从业者视角系统揭示"认证失灵"的行业根因，为监管设计提供了接地气的证据。但17个样本覆盖的行业角色有限，且摘要未展开4大主题的具体内容与16项发现的分布，削弱了可操作性；未来可结合认证机构评估数据做三角验证，并针对各国不同认证框架（如欧盟网络韧性法案、美国标签计划）给出差异化建议。


#### Analyzing the iOS Local Network Permission from a Technical and User Perspective.

- **作者**: David Schmidt, Alexander Ponticello, Magdalena Steinböck, Katharina Krombholz, Martina Lindorfer

- **方向**: 移动与物联网安全

- **解读**: 过去恶意应用通过本地网络通信攻击路由器或定位用户位置。为缓解此类风险，Apple 自 iOS 14 引入本地网络权限。要真正有效，该权限既要能抵御技术威胁，也要让用户能做出知情决策——而"本地网络"这一概念的高度技术性很可能阻碍后者。

方法上，本文首次对 iOS 本地网络权限做四方面综合分析：系统性地研究其实现机制；对 10,862 款应用做大规模动态分析以考察权限的实际访问行为；剖析权限弹窗背后的概念构成（用户决策前获得的全部信息）；并基于这些概念开展 N=150 的在线调查，测量用户对该权限的理解程度。

结果：结果显示权限的"技术有效性"与"用户可理解性"之间存在明显落差：大规模动态分析揭示了应用对本地网络的访问模式，而用户调查表明多数用户难以正确理解"本地网络"所涉及的风险范围，难以据此做出知情同意（具体占比等数据见论文全文）。

我的思考：本文的价值在于把"权限设计"拆成技术面与人类因素面两条线同时检验，方法上（实现审计 + 万级应用动态分析 + 受控调查）相当完整。结论对平台权限设计有直接启示：仅靠授权弹窗无法解决用户不理解的问题，需要更直观的风险呈现。与既往 Android 权限研究相比，这是少有的对单一 iOS 权限的纵深剖析；局限在于调查样本规模（N=150）与自报理解可能存在偏差。


#### Born with a Silver Spoon: On the (In)Security of Native Granted App Privileges in Custom Android ROMs.

- **作者**: Chao Wang, Yanjie Zhao, Jiapeng Deng, Haoyu Wang

- **方向**: 移动与物联网安全

- **解读**: Android 生态的定制化与碎片化催生了大量厂商定制系统，OEM 为提升体验与性能对"超级应用"等常用应用授予原生特权已成为差异化策略。但这种授权往往源于对应用的信任，若缺乏对应用身份的校验，就会形成隐蔽的隐式漏洞——恶意应用可伪装成被授权应用，获取本不该拥有的系统权限。

方法上，本文对定制 Android ROM 中"原生授予应用特权"机制的安全性进行系统审计：梳理各 OEM 的授权机制实现，分析身份校验缺失导致的权限提升路径，并评估恶意应用实际利用这些漏洞获取特权的能力。

结果：摘要未给出量化数字；研究以漏洞发现与机理分析为主，揭示了定制 ROM 生态中普遍存在的"信任应用身份"这一脆弱假设（具体漏洞清单与受影响厂商分布见论文全文）。

我的思考：本文抓住了 Android 碎片化生态中一个被忽视的攻击面——厂商为体验而做的"特权预授权"。其深层问题是安全机制（权限模型）与商业诉求（体验优化）之间的冲突：OEM 以白名单方式绕过权限模型，却未配套身份认证，等于在权限模型上撕开一个口子。与 Google Play 生态的权限审计研究相比，定制 ROM 缺乏统一监管，问题更隐蔽。这类研究对 OEM 有直接整改价值；局限在于漏洞影响面取决于具体厂商实现，需在更大 ROM 集合上验证普遍性。


#### Code Speaks Louder: Exploring Security and Privacy Relevant Regional Variations in Mobile Applications.

- **作者**: Jiawei Guo, Yu Nong, Zhiqiang Lin, Haipeng Cai

- **方向**: 移动与物联网安全

- **解读**: 移动应用常针对不同地区分发不同版本以适应当地法规与市场偏好。以往研究只考察了权限、隐私政策等元数据层面的差异，缺乏对代码层面、可能影响安全的区域差异的系统调查。

方法上，本文首次在代码实现层面对 Android 应用的地理特征差异（GFD）做全面研究，开发了 Freelens 框架，解决混淆分析与可扩展性两大技术挑战，以识别和刻画安全相关的区域差异；研究在 10 个互联网自由度不同的国家、21,120 款应用上开展大规模分析。

结果：结果显示 GFD 广泛存在且影响显著：应用在不同地区的代码版本在安全与隐私相关的实现上存在系统性差异（具体差异类型与统计见论文全文），且不同互联网自由度地区之间的差异模式呈现一定规律性。

我的思考：本文把"区域差异"从元数据层推进到代码层，方法上需应对混淆与海量样本的规模化难题，Freelens 的工程价值不可小觑。其发现对监管者有重要含义：同一应用在不同地区可能提供不同的隐私保护水平，用户知情权与监管一致性因此受到挑战。与既有地区差异研究相比，代码级证据比元数据更有说服力；局限在于 21,120 款的样本虽大，但覆盖地区与商店生态有限，且"差异是否恶意"需要结合具体语义判断。


#### Analyzing Ad Prevalence, Characteristics, and Compliance in Alexa Skills.

- **作者**: Aafaq Sabir, Abhinaya S. B., Dilawer Ahmed, Anupam Das

- **方向**: 移动与物联网安全

- **解读**: 智能语音助手（如 Amazon Alexa）快速普及，且随 LLM 驱动助手的兴起与 Alexa 内"广告 ID"的引入，广告几乎必然在语音平台上泛滥。Alexa 平台虽允许第三方开发者（技能）内置广告、通过广告标识符实现定向投放，但政策明确限制技能响应、通知或提醒中的广告（除非符合限定情形）。问题在于：开发者是否都合规？是否有人绕过审核发布不合规广告？

方法上，本文对 Alexa 技能广告生态做首次大规模分析，从广告普遍性、特征刻画与政策合规性三个维度展开：大规模抓取技能并检测其中的广告行为，分析广告的呈现方式、触发条件与定向机制，并对照平台政策评估合规性。

结果：摘要未给出量化结果；研究以生态测量为主，揭示广告在技能中的流行程度、常见特征以及不合规广告的存在与规避审核的手法（具体统计见论文全文）。

我的思考：语音助手的"对话式广告"比 Web 广告更具侵入性——广告嵌在语音交互流程中，用户几乎无法屏蔽，且"广告 ID"定向机制涉及敏感语音数据的使用。本文作为该方向的首个大规模测量，为平台治理提供了基础事实。局限在于技能生态的动态性与区域差异可能影响测量样本的代表性；与 Web/移动广告测量相比，语音场景的检测方法（解析技能交互）需要更强的工程投入，这也是本文方法学上的看点。


### 云与供应链安全（2 篇）

#### Growlithe: A Developer-Centric Compliance Tool for Serverless Applications.
- **作者**: Praveen Gupta, Arshia Moghimi, Devam Sisodraker, Mohammad Shahrad, Aastha Mehta
- **方向**: 云与供应链安全
- **解读**: 
无服务器应用由异构语言编写的函数、多种数据存储与通信服务构成且快速演化，租户难以防止因 bug、配置错误、人为失误导致的无意数据泄漏；云安全工具（如 IAM）缺乏对租户应用的可观测性，而 SOTA 数据流追踪工具要么依赖云平台支持、要么带来显著运行时开销。

方法上，提出 Growlithe：与开发工具链集成，实现"策略即设计"的持续合规；允许以声明式方式在语言与平台无关的图抽象上指定访问流控制，并通过静态分析与执行强制的组合落地这些策略。

结果显示，Growlithe 支持用 Python、JavaScript 编写的函数，可部署于 AWS Lambda 与 Google Cloud Functions 等平台；实证表明其跨切面、可移植且高效，开发者能轻松适应不断演化的需求。

我的思考："合规左移"到开发工具链、声明式策略+平台无关图抽象解决了多云可移植性问题，静态+运行时混合强制在开销与覆盖间取得平衡；局限是当前覆盖语言与平台有限，策略表达能力与误报率需要更多真实应用实证。对无服务器租户数据治理是实用的补充。


#### The File That Contained the Keys Has Been Removed: An Empirical Analysis of Secret Leaks in Cloud Buckets and Responsible Disclosure Outcomes.
- **作者**: Soufian El Yadmani, Olga Gadyatskaya, Yury Zhauniarovich
- **方向**: 云与供应链安全
- **解读**: 
云对象存储（AWS S3、Google Cloud Storage、Azure Blob Storage）被广泛用于存放海量数据，包括软件开发中的敏感配置文件，其中常含 API 密钥与凭据等机密；配置错误的桶可能无意暴露这些机密，导致未授权访问与安全事件。

方法上，系统扫描大量公开可访问的桶，覆盖多种开发常用文件格式，聚焦不同类型泄漏的多样化影响及非侵入式验证的可行性；发现泄漏后向相关组织与服务提供商负责任披露，并测量披露流程的结果。

结果显示，识别出 215 例凭据暴露（涉及数据库、基础设施、第三方 API 等，风险各异）；披露努力促成 95 个问题的修复，其中 20 个组织直接反馈（及时处理），其余修复未向披露者直接反馈。

我的思考：大规模实证+完整披露闭环是该工作的亮点，"215 例泄漏、95 例修复"表明负责任的披露流程有效但未全覆盖，非侵入式验证方法值得借鉴；局限是扫描覆盖面与检测方法决定发现上限，现实泄漏规模可能远大于此（冰山一角）。对云租户的配置安全实践有警示意义。


### 认证与访问控制（9 篇）

#### RankGuess: Password Guessing Using Adversarial Ranking.

- **作者**: Tao Yang, Ding Wang

- **方向**: 认证与访问控制

- **解读**: 密码安全性的理解高度依赖对攻击者猜测行为的建模。最优攻击者按似然降序猜测，与推荐系统中学习排序的思路同构。本文提出 RankGuess，一个基于对抗排序的统一密码猜测框架，把密码生成形式化为顺序决策轨迹，引入强化学习式方法论。

方法上，RankGuess 将密码生成视为马尔可夫决策过程：当前状态为已生成前缀 token 序列，动作为生成下一 token，评估器（ranker）的评分作为奖励信号，训练智能体朝最大化排名评分的方向生成候选密码。框架高度通用，可适配拖网式猜测、基于 PII 的猜测与条件猜测（如已知部分掩码）。

作者用 12 个大规模密码数据集与 6 个 PII 数据集验证：(1) 全面超越 SOTA，比 GAN 类方法提升 26.29%–43.69%（平均 34.80%）；(2) 已知站点 A 的 PII 时，RankGuess-PII 在 10^12 次猜测内猜中 58.21%–91.95% 的跨站复用密码，优于最强对手 6.32%–17.09%；(3) 10^7 次猜测内，RankGuess-Mask 利用部分密码（如 d****1*02*）提升破解成功率 7.70%–14.85%（平均 8.21%）。

我的思考：本文把对抗排序/学习排序范式引入密码猜测，将生成建模为带奖励的序列决策问题，可端到端优化——相比 GAN，排序信号直接对应"按概率降序"的攻击目标函数，平均 34.80% 的领先证明了范式优势。局限：10^12 预算下的 PII 场景更接近理论威胁；ranker 选择对结果敏感。与 PCFG、Markov、神经网络与 GAN 系工作相比，RankGuess 提供新轴（对抗排序），其跨站复用与掩码猜测结果对密码策略制定有直接价值。


#### Security Analysis of Master-Password-Protected Password Management Protocols.

- **作者**: Yihe Duan, Ding Wang, Yanduo Fu

- **方向**: 认证与访问控制

- **解读**: 密码管理器帮助用户管理海量凭据，其中"主密码保护的密码管理协议"（M3PM）刻画客户端与 PM 服务器的交互：用户以主密码认证，服务器协助跨设备检索凭据。在数据泄露频发的背景下，确保服务器对凭据不可知至关重要，但 M3PM 缺乏系统性形式化分析。本文首次对 M3PM 协议开展大规模系统分析。

方法上，作者通过文档分析与流量逆向，从产业界与学术界的 43 个密码管理器中识别事实上的 M3PM 协议；在通用可组合（UC）框架内提出理想功能形式化所需安全性质；按攻击者知识把针对主密码的离线猜测攻击分为四种类型，形成系统威胁分类。

结果令人警醒：43 个方案中 38 个至少易受一种攻击，表明"单一保护机制"在多种攻击者知识下无法抵御离线猜测。案例分析显示：Passbolt 存在预言机攻击——被攻陷服务器可学习加密密钥；而 1Password 的双密钥机制提供较强保护，验证了纵深防御路线的有效性。

我的思考：本文的成色在于"规模 + 形式化"：43 个现实 PM 的协议逆向与 UC 框架内性质刻画，把散落的密码管理器讨论提升为可比较的体系化分析；四类离线猜测分类提供了通用威胁模型。核心启示是"单一保护必败"——只要本地派生密钥受主密码熵限制，服务器侧辅助数据就可能被离线穷举。局限：UC 理想功能与实现间的抽象差距、方案时效性需注意。与单点攻击论文相比，这是首个横向系统性比较，对 PM 行业安全设计（如双密钥）给出理论支撑。


#### Open Sesame! On the Security and Memorability of Verbal Passwords.

- **作者**: Eunsoo Kim, Kiho Lee, Doowon Kim, Hyoungshick Kim

- **方向**: 认证与访问控制

- **解读**: 文本密码研究已极深入，但"口头密码"（说出而非键入）的安全性与可记忆性几乎未被探索。口头密码在智能音箱、可穿戴设备、车载系统等无法方便键入的场景及运动障碍用户群体中价值显著。本文通过两项大规模用户研究，首次系统评估口头密码的可行性。

方法上，第一项研究（N=2,085）考察用户自由选择口头密码时的猜测空间；第二项（n=600）在创建环节施加"单词数量下限 + 屏蔽词表"策略，测量其对安全性（抗猜测）与可记忆性（长期回忆成功率）的影响，并与传统文本密码对照。

结果显示：自由选择的口头密码猜测空间有限，10^9 次猜测内即可破解 39.76%，安全性不足；施加策略后显著改善——"最小单词数 + 屏蔽词表"下 65.6% 用户在长期测试中成功回忆（对照组 54.11%），且受约束口令破解率更低（6.5% 对 10.3%）。口头密码由此成为"不可键入"场景中兼顾安全与记忆的实用替代。

我的思考：本文把密码研究从"键入"扩展到"语音"维度，两个大规模受控实验给出可靠结论：口头密码的天然弱点可被创建策略显著缓解，且策略还意外改善可记忆性（65.6% vs 54.11%）——约束反而帮助记忆，是对"强策略伤记忆"常识的有趣修正。局限：破解率评估依赖猜测模型对语音口令的适配，录音窃听等真实攻击未完全建模；中文等场景未覆盖。与文本密码策略研究相比，本文为语音交互时代的认证设计提供首批实证数据，值得智能音箱与车载厂商参考。


#### "Only as Strong as the Weakest Link": On the Security of Brokered Single Sign-On on the Web.
- **作者**: Tommaso Innocenti, Louis Jannett, Christian Mainka, Vladislav Mladenov, Engin Kirda
- **方向**: 认证与访问控制
- **解读**: 单点登录（SSO）让用户凭一组凭据访问多个服务，改善了体验，但给开发者带来安全实现复杂协议的挑战；外部服务（broker）简化了集成，却引入了一个几乎未被研究的新角色。本文聚焦新兴的 brokered SSO 生态，系统评估 broker 的安全性，揭示此前研究的重大盲区。

方法上，作者系统化测量 SSO 生态格局，统计 broker 的采用规模，并通过全面评估归纳出三类威胁：(1) 重定向链校验不足导致注入攻击；(2) 未授权数据访问与账户接管；(3) 违反现行最佳实践。

结果方面，研究揭示 25% 的网站通过 broker 集成认证——这一区域此前没有任何研究覆盖；评估中在超过 50 个目标上暴露漏洞，波及 2,000 余个网站，且作者强调这些发现只是严重局势的下界。

我认为该测量首次量化了 broker 角色的攻击面，“最弱一环”的比喻非常贴切——第三方抽象在简化集成的同时引入了信任错配与校验盲区。局限在于威胁归纳依赖具体集成测试的覆盖面，2,000 个网站的影响统计口径未详述；与 OIDC/OAuth 委托方实现审计类工作形成互补，但针对 broker 的自动化审计工具仍待发展。


#### A Composability Analysis Framework for Web3 Wallet Recovery Mechanisms.

- **作者**: Panagiotis Chatzigiannis, Ke Coby Wang, Sunpreet S. Arora, Mohsen Minaei

- **方向**: 认证与访问控制

- **解读**: 现代 Web3 钱包提供混合恢复方案，组合多种密钥管理方法以平衡安全、可用性与易用性，如私钥的秘密共享、加密云存储、基于智能合约的高级功能。然而组合方案可能引入独立方案中不存在的全新攻击向量。本文提出一个针对区块链/Web3 钱包恢复机制（含资产托管设计）的形式化安全分析框架，评估组合设计在何种条件下仍然安全。

方法上，该框架考虑多项因素：用户的可用性、响应性与恶意行为，外部方的共同托管，钱包管理的资产总价值，以及被选为支出辅助方的实体声誉等；分析通过概率模型检验进行，可识别出组合恢复机制保持安全的条件边界。

作者用两个受现有设计启发的复合机制示例验证了框架的有效性，展示了框架能对不同的组合方案给出可判定的安全结论。

我的思考：Web3 钱包恢复是安全与人因深度耦合的领域——私钥丢失即资产永久损失，多层恢复方案因此流行，但"组合引入新攻击面"长期缺乏系统性分析手段。本文的贡献是把形式化方法（概率模型检验）引入该工程问题：以资产价值、参与方行为与声誉等参数化条件，给出"何时安全"的可验证边界，对钱包设计者有实际指导意义。局限：概率模型检验需把现实行为抽象为参数，参数与真实威胁模型的差距会影响结论可靠性，框架对智能合约代码级漏洞覆盖有限。相比 MPC 钱包与社交恢复研究，本文提供了更严谨的组合分析视角。


#### EPScan: Automated Detection of Excessive RBAC Permissions in Kubernetes Applications.
- **作者**: Yue Gu, Xin Tan, Yuan Zhang, Siyan Gao, Min Yang
- **方向**: 认证与访问控制
- **解读**: 
Kubernetes 作为主导容器编排系统拥有庞大第三方应用生态，应用通过 RBAC 机制访问集群资源；已有研究揭示应用被授予过度权限并提出相关攻击，但其假设攻击者先通过容器逃逸攻陷节点，现实中难以实现。

方法上，提出条件更简单的攻击模型：攻击者攻陷一个 pod（无需攻陷节点）即可获得其他节点权限或破坏 pod 的数据机密性与可用性；并开发 EPScan 自动检测可利用的过度权限——采用 pod 导向的程序分析，准确识别每个 pod 中运行程序实际所需权限，与配置文件中请求的权限对比，报告可被滥用的权限。

结果显示，EPScan 应用于 108 个 CNCF 项目，发现 50 个项目中 106 个 pod 存在此前未知的过度权限，精确率 94.6%，并获得 9 个 CVE 编号。

我的思考：将攻击前提从"逃逸节点"降至"攻陷单 pod"更贴合现实威胁模型，务实且重要；pod 级程序分析+配置对比的自动化检测效果好（94.6% 精确率、9 个 CVE 佐证真实危害）。局限是依赖静态分析准确性，动态权限行为可能遗漏；对 K8s 生态安全基线建设有直接价值。

#### "It's almost like Frankenstein": Investigating the Complexities of Scientific Collaboration and Privilege Management within Research Computing Infrastructures.
- **作者**: Souradip Nath; Ananta Soneji; Jaejong Baek; Tiffany Bao; Adam Doupé; Carlos E. Rubio-Medrano; Gail-Joon Ahn
- **方向**: 认证与访问控制
- **解读**: 研究计算基础设施（RCI）整合了高性能计算、先进存储与复杂网络协议，支撑当今数据驱动的科研协作。访问控制对这类高度协作环境至关重要，但充分释放 RCI 潜力不仅需要技术探索，更需要对操作和使用这些系统的利益相关者做深入的人本理解，而这一维度此前缺乏实证研究。

本文是第一项针对 RCI 交互中人本维度的定性研究：对来自 12 个机构的 24 名关键利益相关者（包括研究人员与系统管理员）进行访谈，聚焦访问控制实践，考察其做法、挑战与需求。

研究发现揭示出项目特定、基于信任的资源共享动态，以及安全性与可用性之间的紧张关系；据此提出利益相关者驱动的、面向自适应与用户中心 RCI 的建议与需求清单，为改进 RCI 访问控制实践奠定基础。

我的思考：在访问控制研究被技术方案主导的背景下，这篇以"Frankenstein"隐喻 RCI 拼凑式基础设施的定性研究填补了用户侧空白。24 人/12 机构的样本量在定性研究中属中等水平，结论的普适性有待更大范围验证，但其价值在于为后续自适应访问控制设计提供了真实需求证据，与系统性技术研究形成互补，对科研机构权限治理有直接参考意义。


#### Token Weaver: Privacy Preserving and Post-Compromise Secure Attestation.

- **作者**: Cas Cremers, Gal Horowitz, Charlie Jacomme, Eyal Ronen

- **方向**: 认证与访问控制

- **解读**: 基于 TEE 的现代认证显著降低了秘密泄露风险，使用户能在安全关键服务上安全执行敏感计算（如认证用的密码协议）。但这也使 TEE 成为高价值攻击目标，引发"新型攻破与持续更新"之间的军备竞赛。理想目标是后妥协安全（PCS）：即使 TEE 曾遭攻破，更新后也能恢复安全状态；同时要保护用户隐私，防止 Intel、Google、Samsung 等服务提供商通过认证过程追踪用户。难点在于"不可链接性"与标准 PCS 修复机制看似互斥。

方法上，本文发展出 Token Weaver 方案，在 TEE 认证中同时实现隐私保护与后妥协安全：通过精心设计的令牌机制与密钥更新流程，使每次认证在服务商看来不可链接，同时允许 TEE 在遭受攻破并更新后自动恢复安全状态，弥合不可链接性与 PCS 修复之间的张力。

结果：摘要同样被截断，未给出可引用的量化数据；贡献以安全与隐私性质的形式化论证为主（作者团队在协议形式化验证领域积累深厚），公开摘要未展示具体证明细节与开销。

我的思考：本文切中 TEE 认证的真实痛点——远程认证往往依赖平台身份，天然与服务商追踪绑定；把 PCS 与不可链接性同时做到位在学术上极具挑战，作者团队的形式化功底为可信度背书。但"更新后恢复"的前提（TEE 固件更新本身可信）在现实供应链中未必成立，认证协议的实际部署开销也有待全文验证。


#### Lombard-VLD: Voice Liveness Detection Based on Human Auditory Feedback.

- **作者**: Hongcheng Zhu, Zongkun Sun, Yanzhen Ren, Kun He, Yongpeng Yan, Zixuan Wang, Wuyang Liu, Yuhong Yang, Weiping Tu

- **方向**: 认证与访问控制

- **解读**: 语音活体检测（VLD）用于保护说话人认证免受语音欺骗攻击，判断语音来自真人还是扬声器重放。以往方法主要利用信号层面的声学差异，在噪声环境等场景下区分度有限。

方法上，本文提出首个利用人类听觉反馈机制（Lombard 效应）的 VLD——Lombard-VLD。核心洞察：真人在嘈杂背景中会生理性地、不由自主地调整说话方式（Lombard 效应），而扬声器不会。方法上设计基于参考的双输入模式差分 SE-ResBlock 模型，刻画 Lombard 效应引起的声学变化，以此区分真人语音与重放。

结果：实验表明 Lombard-VLD 在两个数据集上分别达到 0% 与 0.24% 的等错误率（EER），优于现有最先进方法，且对多种环境因素（不同噪声水平等）具有鲁棒性。

我的思考：把 Lombard 效应（声学领域的经典现象）引入活体检测是聪明的跨域迁移——它利用了"生理不可控性"，正是攻击者最难伪造的特征。0% 的 EER 在单一数据集上很有说服力，但需警惕同分布评估带来的乐观估计，跨设备、跨语言与对抗性重放（如攻击者同时播放噪声）场景仍有待检验。与基于信号差异的活体检测相比，本文开辟了"生理反馈"新维度；但双输入模式要求受控的噪声环境，实际部署条件较苛刻。


### 数字取证（1 篇）

#### Connecting the Extra Dots (Contexts): Correlating External Information about Point of Interest for Attack Investigation.
- **作者**: Sareh Mohammadi, Hugo Kermabon-Bobinnec, Azadeh Tabiban, Lingyu Wang, Tomás Navarro Múnera, Yosr Jarraya
- **方向**: 数字取证
- **解读**:
来源图（provenance）分析是安全分析人员调查安全事件的主流手段，但图规模庞大，业界提出大量剪枝方法。这些方法多依赖图论特征或异常检测，将事件仅视为抽象起点，未能利用与事件相关的外部信息（如漏洞或利用知识），从而错失有效剪枝的机会。

基于此观察，作者提出 Contexts 方案：从外部来源提取情报，将其映射到来源图节点，并关联成与攻击相关子图，与现有剪枝方法互补。它既可作预处理器，也可独立作为调查解决方案。

基于真实攻击的实验显示：作为预处理器可将误报从 15 万以上降至 10 以下；独立使用时在 20 个攻击中的 19 个上达到 100% 真阳率（TPR），16 个攻击上假阳率（FPR）低于 0.6%；用户研究中 94.4% 的参与者认可其对攻击调查的实用性。

引入外部上下文与纯图特征互补的思路是该文最突出的创新点，量化结果扎实。局限在于效果依赖外部情报的质量与覆盖，且测试攻击集合规模有限。该工作展示了"情报增强型"取证分析的前景，为后续研究开辟了方向。


### 无线与通信安全（6 篇）

#### Mind the Location Leakage in LEO Direct-to-Cell Satellite Networks.
- **作者**: Weisen Liu, Zeqi Lai, Qian Wu, Hewu Li, Yuxuan Weng, Wei Liu, Qi Zhang, Jihao Li, Yuanjie Li, Jun Liu
- **方向**: 无线与通信安全
- **解读**: 利用低轨（LEO）直连手机（DTC）卫星直接为地面手机提供通信服务正快速普及，但无线介质的开放特性与 LEO 卫星的动态行为相结合，催生新的隐私泄露风险：窃听 DTC 广播的敌手可能窃取活跃用户的物理位置。本文系统研究新兴直连卫星网络（DCSN）的位置泄露技术。

方法上，提出分析器 DCATOR（DCSN 终端定位器）：持续监视广播信道上的信令消息，提取多种线索，并结合随时间变化的卫星轨迹推断用户位置；若敌手能持续监测，还可进一步推断同一覆盖区域内其他用户的存在。

评估方面，针对三个代表性 DCSN 展开：运营中的 Iridium、开发中的 Starlink DTC、以及基于最新 3GPP NTN 标准的系统；大量实验证明真实 DCSN 中确实存在位置泄露，最坏情况下可精确追踪至数百米量级；并据此提出面向 DCSN 的隐私增强对策。

我认为这是首个系统研究 DTC 网络位置泄露的工作，“信号线索+轨迹几何”联合推断的方法优雅且可复现，对即将规模部署的 Starlink DTC 具有预警价值。局限在于分析依赖广播信令的可观测假设，数百米精度相对地面定位技术的对比未展开；对策的有效性也需在真实部署中检验。


#### From Control to Chaos: A Comprehensive Formal Analysis of 5G's Access Control.
- **作者**: Mujtahid Akon, Md. Toufikuzzaman, Syed Rafiul Hussain
- **方向**: 无线与通信安全
- **解读**: 5G 核心网的访问控制机制复杂，而既有形式化分析多未考虑间接通信模式与漫游场景。本文开发 CoreScan，一个全面的形式化分析框架，用于分析 5G 核心网访问控制；为此构建了首个同时考虑间接通信模式与漫游的网络模型。

方法上，给定全局安全属性后，CoreScan 采用组合验证技术：利用 assume-guarantee 风格的推理把系统分解为多个不相交组件，应用 split assertion 原则识别局部假设与保证；模型的安全属性成立当且仅当所有派生保证在其各自组件中得到验证。框架还配备可配置敌手模型，支持在不同能力下评估属性。

结果方面，CoreScan 在 61 项测试中发现五类新的可利用权限提升漏洞；此外还发现大多数此前已知的过度授权（overprivilege）问题同样延伸至漫游设置中。

我认为组合验证是应对 5G 规模的关键工程决策，漫游与间接通信的建模补齐了既有工作的盲区，可配置敌手模型也增强了结论的稳健性。局限在于抽象模型与真实协议实现的差距、61 项测试的覆盖面未详述；与既有 5G 形式化工作相比，本文以“更完整模型+组合方法”实现递进，但对修复建议的落地验证仍待后续工作。


#### CamLopa: A Hidden Wireless Camera Localization Framework via Signal Propagation Path Analysis.
- **作者**: Xiang Zhang, Jie Zhang, Zehua Ma, Jinyang Huang, Meng Li, Huan Yan, Peng Zhao, Zijian Zhang, Bin Liu, Qing Guo, Tianwei Zhang, Nenghai Yu
- **方向**: 无线与通信安全
- **解读**: 隐藏无线摄像头（酒店、民宿偷拍）严重威胁隐私，但现有方案需空旷活动空间、昂贵设备或预收集训练数据，难以落地。本文提出 CamLopa：仅用低成本树莓派即可在 45 秒用户活动内完成检测与定位，无需训练。

分三阶段：检测（0–15s）按平均包长与包数阈值筛出可疑设备，再让用户离开房间，利用“摄像头编码流量随活动减少而下降”的因果关系判定是否偷拍；定位（15–35s）利用菲涅尔第一区（FFZ）衍射原理——人体穿过 FFZ 引起显著信号衰减、衰减时长与路径长度相关——让用户沿两条经过设备的正交路径行走，取两路径 FFZ 穿越时间之比（正交比）消去未知速度与体型影响，经传播路径分析建模用牛顿-拉夫森法解出方位角；最后（35–45s）第三次行走判定象限。

在三个真实房间、7 款商用摄像头（2.4GHz WiFi）上验证两个月：可疑设备识别率 84.35%（其中 360 摄像头因只发 ACK/RTS 帧而无法识别，其余 98.41%）；偷拍检测成功率 95.37%；平均方位角定位误差约 17.2°；跨设备、跨房间误差稳定，正交比理论最大误差 15° 优于单路径 L1 法（20°）。

我的思考：核心贡献是把 Fresnel 衍射物理模型转化为“正交比”这一免标定、免训练的定位原语，同时消除速度与体型两个未知量，比 LocCams 的 90° 粗定位精细得多，对空房间要求、设备成本、用户负担全面优于前人。局限明确：只适用 WiFi 直播摄像头、只能定位 0–180°、360 这类不传 QoS 数据帧的设备需靠 OUI 兜底、树莓派需 nexmon 改造。作为首个基于信号传播路径分析的隐藏摄像头定位框架，实用性突出，但 CSI 网卡门槛仍限制了普通用户直接使用。

- **arXiv PDF**: [CamLopa: A Hidden Wireless Camera Localization Framework via Signal Propagation Path Analysis.](https://arxiv.org/pdf/2409.15169)

#### Adversarial Robust ViT-Based Automatic Modulation Recognition in Practical Deep Learning-Based Wireless Systems.
- **作者**: Gen Li, ChunChih Lin, Xiaonan Zhang, Xiaolong Ma, Linke Guo
- **方向**: 无线与通信安全
- **解读**: 
深度学习的自动调制识别（AMR）被广泛用于频谱监控与管理，但实际环境中信号易受恶意噪声、有意干扰与对抗攻击影响：不可检测的微小扰动即可导致误分类，进而造成解码错误、吞吐下降与服务中断。现有防御工作存在局限，本文的目标正是构建鲁棒的AMR模型。

本文不直接套用视觉Transformer（ViT），而是首先创新性地设计面向射频（RF）信号的特征提取模块，联合利用时域特征，配合自适应位置编码以提升精度；为缓解通信噪声影响，进一步提出噪声自适应训练方案，使用白盒攻击者构造的对抗样本训练基于Transformer的模型。

作者在自采真实数据集上进行了全面评估，该数据集包含超3000万样本、21种调制方案，覆盖室内与室外场景。结果显示最大分类准确率达94.17%，在攻击下仍保持71.2%的准确率；同时还验证了面对真实攻击时的实时鲁棒性。数据集与代码已开源。

大规模真实数据与开源是该工作的重要加分项，工程完成度较高；但其对抗训练基于白盒攻击者假设，对黑盒或自适应攻击的鲁棒性仍存疑问。与基于CNN的AMR防御相比，Transformer在长时序依赖建模上具备天然优势，这一方向值得后续工作深入探索。


#### Saecred: A State-Aware, Over-the-Air Protocol Testing Approach for Discovering Parsing Bugs in SAE Handshake Implementations of COTS Wi-Fi Access Points.
- **作者**: Muhammad Daniyal Pirwani Dar, Robert Lorch, Aliakbar Sadeghi, Vincenzo Sorcigli, Héloïse Gollier, Cesare Tinelli, Mathy Vanhoef, Omar Chowdhury
- **方向**: 无线与通信安全
- **解读**: 
WPA3-Personal引入有状态的SAE握手协议以提供前向保密与抗口令猜测能力，但为缓解降级DoS攻击而增强的机制使报文格式变得结构可变、上下文敏感，COTS接入点（AP）在解析时极易出错，而错误解析会直接危害安全性。

Saecred将问题建模为"结构×状态"的二维搜索问题：结合迭代加深搜索（IDS）与基于语法的模糊测试，后者借助Syntax-Guided Synthesis（SyGuS）求解器生成语法引导的报文，实现对COTS AP的黑盒、空中（over-the-air）协议测试。

在6个COTS AP与广泛使用的开源hostapd上评估，发现若干实例、共4类bug，其中两类违反基本安全期望（构成DoS攻击）。发现已报告给相关厂商，并促成补丁与安全公告的发布。

状态感知是相对普通协议fuzzer的关键增量——把问题显式拆解为结构与状态两个维度，使搜索更有方向性，SyGuS求解器则让语法生成更具针对性；但黑盒设定下可达状态空间受限，测试深度仍有天花板，且评估仅覆盖6个AP，样本面有待扩大。与既有WPA3降级、DoS攻击研究相比，Saecred聚焦实现层的解析缺陷，二者互补，对Wi-Fi生态安全有直接价值。


#### Spoofing Eavesdroppers with Audio Misinformation.

- **作者**: Zhambyl Shaikhanov, Mahmoud Al-Madi, Hou-Tong Chen, Chun-Chieh Chang, Sadhvikas Addamane, Daniel M. Mittleman, Edward W. Knightly

- **方向**: 无线与通信安全

- **解读**: 对手机通话的无线窃听已成为重大安全关切，尤其随 5G 及更高频段在更高频率与更高分辨率上的发展。近期研究表明，攻击者可利用现成的毫米波雷达，远程检测手机听筒发出的微米级声学振动来窃听语音，受害者毫无察觉。

方法上，本文提出 MiSINFO 架构：不仅挫败此类攻击，还能反制攻击者——向其注入虚假信息（音频 misinformation）。其动机是：此类攻击针对的是物理介质信号，无法用数字加密保护，是通信链中最薄弱的一环，因此需要在物理层做系统化防御。MiSINFO 通过受控的声学调制，让雷达窃听者解调出的音频被替换为误导性内容。

结果：摘要未给出量化数据；工作以架构设计加实验验证为主，展示了对毫米波振动窃听的有效反制与欺骗能力（误码率/成功率等评估见论文全文）。

我的思考：物理层窃听是 E2EE 无法覆盖的盲区，本文的"以欺骗对窃听"思路颇具创造性——既然防不住探测，就让探测结果失去意义。与 EveGuard（抑制传感器信号）相比，MiSINFO 更进一步：主动投毒，把攻击者的窃听变成信息战。其可行性依赖对雷达解调机制的精确建模，环境反射与多径可能影响欺骗保真度；且"喂假信息"在伦理与法律上（如误导执法监听）需要仔细界定。总体而言，这是物理层安全对抗的新方向，与毫米波/超声感知研究形成攻防闭环。


### 数据安全（2 篇）

#### SoK: Dataset Copyright Auditing in Machine Learning Systems.
- **作者**: Linkang Du, Xuanru Zhou, Min Chen, Chusong Zhang, Zhou Su, Peng Cheng, Jiming Chen, Zhikun Zhang
- **方向**: 数据安全
- **解读**: 大规模训练数据被未授权使用（如 Clearview.AI 收集 30 亿张人脸图片）引发数据版权问题，但现有审计方案假设与能力各异、鲁棒性评估只覆盖流水线局部，难以横向比较。本文系统化梳理数据集版权审计研究，并给出统一评测。

按是否修改原始数据集，将方法分为侵入式审计（水印注入：目标/非目标、毒标签/干净标签的后门 T&P/T&C/U&P/U&C、放射性数据 RDA、风格变换 STA）与非侵入式审计（指纹提取：决策边界 DBA、模型行为 MBA），汇总 27 篇论文；并在 CIFAR-10/CIFAR-100/PubFig 上用 ResNet-18/VGG-19 统一构建理想、实用（数据增强+DP-SGD）与对抗（干净微调、Neural Cleanse、输出扰动）三类场景评测。

理想场景下侵入式审计精度最高达 96.9%，非侵入式仅 82.39%；BA1、BA3、RDA 在对抗场景最大仅衰减 11.30%，而 BA4 最大衰减 40.94%；实用场景中数据增强与 DP-SGD 对侵入式方法影响很小（审计精度最多降 5.6%，而模型精度降 32.86%）；STA 副作用最大（W-Acc 最高降 40.10%）。综合看目标后门类（BA）效果最优。

首次以部署视角统一基准并给出方法选择决策树，观察与开放问题提炼清晰，是该领域首个可直接指导实践的 SoK。但评测集中于图像分类，对文本、LLM 与多模态数据的版权审计覆盖不足，且以准确率为指标、缺少法律场景所需的形式化保证；"全流水线评估工具箱"仍是未来方向。
- **arXiv PDF**: [SoK: Dataset Copyright Auditing in Machine Learning Systems.](https://arxiv.org/pdf/2410.16618)

#### Hey, Your Secrets Leaked! Detecting and Characterizing Secret Leakage in the Wild.

- **作者**: Jiawei Zhou, Zidong Zhang, Lingyun Ying, Huajun Chai, Jiuxin Cao, Haixin Duan

- **方向**: 数据安全

- **解读**: API 密钥、密码等敏感凭据是应用安全的基础，开源生态的快速开发使泄露风险持续放大。现有检测工具多依赖正则与熵检查，对非结构化秘密召回率低，又常把非敏感数据误标为秘密。本文提出 Keysentinel，结合机器学习、语义分析与前缀匹配的自动化泄露检测工具，同时改善精度与召回。

方法上，Keysentinel 用 ML 模型学习秘密的结构与语义特征，配合前缀匹配确认真实凭据格式，从而区分真秘密与误报。为公平评估，作者构建首个跨平台基准：从 GitHub、PyPI、微信的 1,806,530 个文件中标注 11,826 个秘密，并与六个现有工具对比。

结果显示 Keysentinel 达到 SOTA：精确率 91.18%、召回率 81.71%、F1 0.86，显著超越业界工具并大幅降低误报；在与 GPT-4/o1 的对比中，准确性与成本效益均胜出。作者还分析了 80,330,098 个文件，发现高达 30% 存在泄露风险，并扫描一家 IT 公司代码库验证真实风险。

我的思考：本文把秘密检测从正则匹配推进到语义理解，首个跨平台大基准（含微信等中文生态）提供了可复现比较框架，并证实轻量 ML 方案性价比优于通用大模型，对工程落地有直接指导意义。局限：81.71% 召回率意味着近两成秘密漏检，对抗刻意混淆可能失效；30% 的比例受采样偏差影响。与 TruffleHog 等工具相比，Keysentinel 展示了机器学习路线的优势，为平台级秘密治理提供了实证依据。


### 社会工程与人类因素（25 篇）

#### Restricting the Link: Effects of Focused Attention and Time Delay on Phishing Warning Effectiveness.
- **作者**: Justin Petelka, Benjamin Berens, Carlo Sugatan, Melanie Volkamer, Florian Schaub
- **方向**: 社会工程与人类因素
- **解读**:
网络钓鱼警告研究提出了两类超链接限制手段以降低钓鱼点击率：聚焦注意（要求用户先点击警告内展开的链接才能继续访问可疑 URL）与时间延迟（短时间内禁用点击）。两者都旨在吸引用户注意并促使其仔细评估链接 URL，但此前从未被对比评估过。

作者开展了混合方法的在线实验（n=1,320），使用经过改造的邮件收件箱环境，让参与者评估含超链接的邮件，分别独立和组合检验两种措施的效果差异。

结果显示两种措施都能独立降低点击率，但效果强度显著不同——聚焦注意比时间延迟更有效；两者组合后点击率进一步下降。此外，看到警告的参与者更可能在点击前长时间悬停于超链接上。作者据此讨论了反钓鱼警告的设计启示。

这是首个对两种超链接限制进行对比的实证研究，样本量大、环境贴近真实邮箱场景。局限在于实验仍属受控在线环境，真实攻击情境下的表现可能不同。结论支持将聚焦注意与时间延迟结合使用，为浏览器与邮件客户端警告设计提供了直接依据。


#### Understanding the Efficacy of Phishing Training in Practice.
- **作者**: Grant Ho, Ariana Mirian, Elisa Luo, Khang Tong, Euyhyun Lee, Lin Liu, Christopher A. Longhurst, Christian Dameff, Stefan Savage, Geoffrey M. Voelker
- **方向**: 社会工程与人类因素
- **解读**:
企业普遍部署两类安全培训——年度网络安全意识培训与嵌入式反钓鱼演练，但其真实效果缺乏大规模、严谨的实证检验。这关系到企业安全预算的投入产出与整体防线有效性。

作者分析了一项为期 8 个月的随机对照实验，涉及大型医疗机构超过 19,500 名员工、10 次模拟钓鱼演练，以评估培训与演练对员工点击行为的影响。

结果对现行培训模式提出质疑：是否近期完成培训与演练失败可能性之间无显著关系；受训与未受训者在失败率上的绝对差异极低；多数员工仅花极少时间与培训材料交互；但对特定内容类型，完整接收并多次参与演练的少数人，其后续点击率确有提升。总体而言，现行形式的培训对降低风险的实用价值有限。

这是该领域少有的万级规模随机对照证据，结论对行业实践冲击直接。局限在于数据来自单一医疗机构，推广性需谨慎。研究提示应重新设计培训的内容形态与交互机制，而非仅关注"是否完成"这一指标，为未来培训有效性研究设立了标杆。


#### Understanding Users' Security and Privacy Concerns and Attitudes Towards Conversational AI Platforms.
- **作者**: Mutahar Ali, Arjun Arunasalam, Habiba Farrukh
- **方向**: 社会工程与人类因素
- **解读**: 对话式 AI 平台的普及带来新的安全与隐私风险，技术侧研究充分，但用户感知长期缺位。论文用大规模真实讨论数据回答：用户担忧什么、态度如何、担忧如何随事件演变。

抓取 r/ChatGPT 子社区（740 万成员）2022 年 12 月至 2024 年 7 月约 250 万条帖子；以 118 个关键词过滤+分层抽样 1200 条人工标注（κ=0.82）微调 RoBERTa 分类器（准确率 96%、F1 82%），筛出 30,240 条 S&P 帖子；对 440 条做主题饱和编码，用 GPT-4o 分层提示多分类（F1 91.2%）；对 38 个重大事件做中断时间序列回归。

担忧集中于数据收集（43.7%，其中个人数据 32.3%）、数据使用（22.5%：训练 11.5%、第三方共享 11.0%）、平台安全（28.9%）、留存（9.5%）、合规（9.6%）与透明控制（11.1%）；用户分为谨慎、好奇、漠视、认命四类；微软投资 OpenAI、意大利封禁 ChatGPT、聊天历史泄露 bug、三星泄密等事件显著驱动相应担忧峰值；部分担忧属误解（如未经授权的 GPS 访问）。

以亿级有机语料补足访谈/问卷的样本与时效局限，事件驱动的纵向分析是亮点；但 Reddit 用户偏技术、欧美为主，普适性有限，且依赖分类器与 LLM 标注。对"默认开启训练"等暗黑模式的证据化批评，以及按用户/平台/企业/政策制定者的分层建议，具有直接政策价值。
- **arXiv PDF**: [Understanding Users' Security and Privacy Concerns and Attitudes Towards Conversational AI Platforms.](https://arxiv.org/pdf/2504.06552)

#### A Deep Dive into How Open-Source Project Maintainers Review and Resolve Bug Bounty Reports.
- **作者**: Jessy Ayala, Steven Ngo, Joshua Garcia
- **方向**: 社会工程与人类因素
- **解读**: 漏洞赏金研究多聚焦平台、项目与猎人视角，而实际审阅报告的 OSS 维护者（通常无安全背景、无资金雇佣猎人）的视角长期被忽视。论文首次系统研究维护者审阅与解决赏金报告的体验。

采用三研究混合方法：列清单问卷（n1=51，开放式编码得到 40 项特征）、Likert 量表排序（n2=90，用 Log-Linear Bradley-Terry 模型计算偏好权重 π）、半结构化访谈（n3=17，主题分析）；招募对象为使用 huntr 平台并完成漏洞修复的 GitHub 项目维护者（覆盖 558-612 个项目）。

最重要收益是私有披露（π=0.155）与项目可见性（π=0.142）；最大挑战是猎人逐利（π=0.141）与审阅压力（π=0.113），低质量/垃圾报告是最常提及项（L=22，π=0.094）；最有用功能是激励性报告（π=0.167）与安全指标计算辅助（π=0.133），CVE 创建支持仅列帮助功能倒数第二（π=0.069）；最想要的是协作功能（π=0.163）与猎人信誉体系（π=0.145）。出人意料：沟通不畅是最不具挑战性的（π=0.038）。

首次给出维护者视角的量化重要性排序，Bradley-Terry 建模规避了 Likert 等距假设的统计缺陷，方法严谨；但样本以欧美男性为主、依赖自报数据，可推广性有限。研究发现对赏金平台设计（信誉体系、PoC 要求、LLM 辅助审阅）有直接指导意义。
- **arXiv PDF**: [A Deep Dive into How Open-Source Project Maintainers Review and Resolve Bug Bounty Reports.](https://arxiv.org/pdf/2409.07670)

#### Study Club, Labor Union or Start-Up? Characterizing Teams and Collaboration in the Bug Bounty Ecosystem.

- **作者**: Yangheran Piao, Temima Hrle, Daniel W. Woods, Ross Anderson

- **方向**: 社会工程与人类因素

- **解读**: 中国形成了独特的赏金生态：平台允许黑客组队注册并获取团队级奖励，但关于团队的规模分布、生产力与协作方式此前几乎无人研究。本文对中国赏金猎人团队现象进行首次混合方法刻画，回答"团队普遍吗、更高效吗、成员为何加入"等问题。

方法上，第一阶段自顶向下：收集 85 个平台排名数据，模糊匹配识别出 2.1k 个团队与 5.9k 名猎人；第二阶段自底向上：对 18 名成员半结构化访谈，揭示加入动机与内部协作机制，两阶段形成"结构—动机"完整图景。

量化结果显示：46% 用户注册为团队成员，团队成员产出是非团队成员的 2 倍以上；典型团队不足 10 人、只活跃于少数平台，但也存在参与 50+ 平台、数百成员的"巨型团队"。质性分析把团队概括为三重隐喻：学习俱乐部（知识交流）、工会（与厂商议价）、初创公司（收益分成与内部规则），帮助成员应对大公司与收入不确定性等挑战。

我的思考：本文聚焦中国特有的团队制赏金生态，用"俱乐部/工会/初创"三重隐喻概括团队的社会与经济功能，兼具宏观规模刻画与微观动机洞察。团队产出翻倍提供了组队效率的量化证据，但存在强队更易被排名捕捉的选择性偏差。局限：模糊匹配会遗漏未上榜团队，访谈样本有限。与欧美单人为主的赏金研究相比，本文揭示了平台规则如何塑造组织形态，为平台设计与研究者提供了中国场景的一手图景。


#### GDPR in the Small: A Field Study of Privacy and Security Challenges in Schools.
- **作者**: Francesco Ciclosi, Giovanna Varni, Fabio Massacci
- **方向**: 社会工程与人类因素
- **解读**: GDPR 本为约束互联网巨头而设，却同样施加于所有大小组织。本文报告一项针对意大利学校的多地点田野研究，考察在缺乏专职法律合规人员的情况下，学校在运行充满敏感问题的活动时面临的 GDPR 实施挑战。样本包括 1 所幼儿园、10 所小学与 2 所初中。

方法上，研究者深入学校现场观察与访谈，记录“照本宣科”的合规程序在真实资源约束下的执行情况，并收集关键事件及其对学童安全与隐私的潜在影响。

结果方面，研究并未发现“隐私悖论”（纸面制度完美、实践却疏忽）的证据；相反，当正规程序无法用学校实际拥有的资源落地时，教职工大多难以应对；作者还区分了可能造成实质伤害的关键事件与“不值得费心”的形式化合规，并讨论风险导向方法如何更好地解决这些问题。

我认为该研究以扎实的田野证据反驳了常见的“隐私悖论”叙事，把合规失败归因于资源约束而非态度问题，对政策制定有直接启示。局限在于单一国家样本、质性结论的推广需谨慎；其“风险导向替代方案”的提议为面向小微机构的 GDPR 工具与指南设计提供了方向。


#### "Sorry for Bugging you so much." Exploring Developers' Behavior Towards Privacy-Compliant Implementation.
- **作者**: Stefan Albert Horstmann, Sandy Hong, David Klein, Raphael Serafini, Martin Degeling, Martin Johns, Veelasha Moonsamy, Alena Naiakshina
- **方向**: 社会工程与人类因素
- **解读**: 保护用户数据至关重要，但开发者常常无法满足隐私要求；其原因是缺乏知识还是支持不足，此前并不清楚。本文开展 5 小时的质性编程研究：30 名专业开发者完成 3 个按 GDPR 合规要求设计的隐私敏感任务，参与者分为控制组、提示组与专家支持组，任务后进行跟踪访谈。

方法上，研究旨在观察开发者是否及如何实现隐私要求，并比较三种干预条件下的表现差异。

结果令人警醒：几乎全部提交均不合规（79/90）；控制组无人解决任何任务；隐私提示与专家支持仅略微改善表现（分别 6/30 与 8/30 的尝试成功）；目的限制、同意、数据最小化等常见要求在实现中问题严重。反直觉的是，尽管多数参与者对方案信心不足，却很少在线求助或联系专家（专家支持组仅 4/10 明确询问确认），反而依赖既有实现、优先考虑功能性与安全性。

我认为该实验设计（对照组+两种干预）严谨，且“开发者不求助”的反直觉发现对隐私工具与培训设计有直接启示——光提供支持不够，还需降低求助门槛。局限在于实验室任务规模与真实项目复杂度存在差距，GDPR 合规判定标准具有一定主观性；该工作为隐私工程工具研究提供了需求侧证据。


#### Teaching Data Science Students to Sketch Privacy Designs Through Heuristics.
- **作者**: Jinhe Wen, Yingxi Zhao, Wenqian Xu, Yaxing Yao, Haojian Jin
- **方向**: 社会工程与人类因素
- **解读**: 近期研究显示，有经验的数据从业者常借助草图围绕隐私设计概念进行沟通，但如何帮助新手学生培养这一技能，此前缺乏理解。本文探索降低数据科学学生创作高质量隐私设计草图门槛的方法。

方法上，作者首先进行需求发现研究（N=12），识别学生在绘制隐私设计草图时面临的障碍；随后采用以人为中心的方法指导方法开发，最终形成三条简单、基于文本的启发式规则。

结果方面，24 名参与者参与的用户研究表明：仅在任务开始时向参与者呈现这些启发式规则，就能提高草图中隐私相关决策的覆盖率、减少完成草图所需的认知负担，并改善最终草图的可读性。

我认为这是一项“轻量干预带来显著效果”的实证研究，三条文本规则的教学成本极低、易于推广，对隐私教育有直接价值。局限在于样本为学生群体、干预效果的长期保持性未测量，且草图质量评估标准的主观性需注意；该工作为隐私教育提供了可扩展的“脚手架”式工具，但其对专业实践中隐私沟通的影响仍待验证。


#### Supporting Family Discussions About Digital Privacy Through Perspective-Taking: An Empirical Investigation.
- **作者**: Zikai Wen, Lanjing Liu, Yaxing Yao
- **方向**: 社会工程与人类因素
- **解读**: 96% 的美国青少年每天上网，但大多数家庭在讨论隐私问题时面临困难：父母感到准备不足且犹豫沟通。本文探索以视角采择（perspective-taking）理论为基础的引导式家庭讨论，如何促进相互理解并提升数字素养。

方法上，研究者开展质性研究，招募 13 对亲子参与引导式讨论，观察沟通模式并提炼障碍与改进机制。

结果方面，研究识别出三类关键沟通挑战：关于隐私的讨论过于抽象、依赖绝对化表述、青少年参与度逐渐下降；这些障碍源于有限的数字素养与缺乏适应性沟通。作者的促进方法把传统“家长主导”的对话转变为协作式交流，通过反思性实践帮助家庭成员把隐私视为情境依赖的概念；并据此提出面向教育技术的设计启示，包括用界面与工具凸显非二元选择。

我认为把家庭作为隐私教育的基本单元、以视角采择化解代际沟通冲突，角度新颖且有实操价值。局限在于样本较小（13 对亲子）、质性结论的推广性有限；其设计启示对家长工具与学校课程开发有实用意义，但效果的长效性与规模化验证仍是后续工作。

#### The Importance of Being Earnest: Shedding Light on Johnny's (False) Sense of Privacy.

- **作者**: Wirawan Agahari, Alexandra Dirksen, Martin Johns, Mark de Reuver, Tobias Fiebig

- **方向**: 社会工程与人类因素

- **解读**: 隐私增强技术（PETs，如安全多方计算 MPC）常被组织用于提升用户对数据共享的信任，但此类技术对普通用户复杂且不透明。以往研究先向用户呈现技术的高层描述、再测量态度或行为变化，结论进而影响商业与监管决策。本文质疑这一方法论：技术名称与通用描述哪个真正塑造了用户的共享意愿，用户是否因此产生虚假安全感。

方法上，作者在数据市场场景中设计三组随机对照实验，共 1,457 名参与者：一组被告知使用 MPC，一组使用虚构的 PET，一组为无 PET 对照，以此分离"技术名称"与"技术描述"两种信息的效应，测量用户向服务商共享数据的意愿。

结果显示，披露 MPC 提升了用户的共享意愿，且起决定作用的是技术描述而非技术名称。作者由此得出结论：声称使用某技术并不能代表实际使用；在用户缺乏正确心理模型、无法核验声明的情况下，用户易被欺骗而陷入虚假安全感，暴露比原本意愿更多的隐私。

我的思考：本文是对"隐私态度研究"范式的方法论纠偏——用虚构技术对照干净地拆解了名称效应与描述效应，以往大量研究恰恰混淆了二者。局限在于摘要未给出效应量与显著性细节，单一数据市场场景的外部效度有限。但该发现对 PET 产品设计（强调机制而非标签）与实验设计（须含虚构技术对照）都有直接启示，也呼应"隐私剧场"批评：用户无法核验时，任何技术声称都应被审慎对待。


#### Ownership and Gatekeeping vs. Safeguarding and Consent: How Migrant Parents Navigate Child Data Management Complexities.
- **作者**: Rui Huan, Kopo M. Ramokapane, Awais Rashid
- **方向**: 社会工程与人类因素
- **解读**: 出国寻求机会的父母日益面临在新的文化与法律环境中养育子女的现实，这包括遵守不熟悉的法规并保护子女数据，任务往往复杂且充满挑战。本研究考察移民父母如何感知、管理并保护与子女相关的数据，聚焦这一被忽视人群的独特处境。

方法上，作者对英国 17 位监护人进行访谈，揭示了对数据所有权与管理细致且动态演变的观点。移民父母对与海外大家庭共享的数据失去控制表示严重担忧，担心数据被滥用，进而危害子女或危及移民身份；作者讨论其管理策略与应对不断变化的同意概念的方法，并呼吁提供面向文化敏感的支持。

量化信息：基于对英国 17 位监护人的访谈，属于定性研究。

我认为该研究把子女数据管理置于跨国家庭、移民身份风险与法律差异的交汇处，视角独特且填补了文献空白。17 个访谈样本保证了质性研究深度，但推广性有限；其对"同意"在家庭语境中动态理解的分析，挑战了以个人为中心的数据治理假设，对数据保护设计与监管实践具有启示意义。


#### "It's Time. Time for Digital Security.": An End User Study on Actionable Security and Privacy Advice.
- **作者**: Anna Lena Rotthaler, Harshini Sri Ramulu, Lucy Simko, Sascha Fahl, Yasemin Acar
- **方向**: 社会工程与人类因素
- **解读**: 数字安全建议是大量研究的焦点，但结果不尽如人意：终端用户不遵循专家建议，专家也难以对现有建议排序。多项研究表明用户被海量建议淹没，并提出了改进建议的途径；然而我们仍然不知道如何有效地向用户传递建议。受日常习惯类应用的启发，作者开发了 30 条简短可操作的安全与隐私建议。

方法上，作者将这些建议做成 Android 应用"Security App"提供给终端用户，以降低心智负担、建立安全习惯；随后开展为期 30 天的在线终端用户研究（N=74），评估用户是否有意义地采纳这些建议及其对意识与行为的影响。

量化结果：研究规模 N=74、周期 30 天、共 30 条建议；结果显示合适的工具确实帮助了用户——多数参与者认为大多数任务可理解、可操作、有用，且确实引入了新的安全行为。

我认为该研究的价值在于把关注点从"建议内容"转向"建议的传递机制"，借鉴习惯养成应用的设计是务实且可落地的方向。N=74 的 30 天在线研究存在自选样本与生态效度问题，且短期行为改变能否持久存疑；量化哪些建议产生持久行为改变、并进行长期随访，是后续研究最有价值的增量。


#### "Not the Right Question?" A Study on Attitudes Toward Client-Side Scanning with Security and Privacy Researchers and a U.S. Population Sample.
- **作者**: Lisa Geierhaas, Florin Martius, Arthi Arumugam, Matthew Smith
- **方向**: 社会工程与人类因素
- **解读**: 数十年来，执法部门与隐私倡导者围绕监控与隐私争论不休，形成所谓"加密战争"。2021 年苹果宣布计划实施客户端扫描（CSS）作为检测已知儿童性虐待材料（CSAM）的隐私保护折中方案，却遭到尤其是 IT 专家的强烈反对，数周内便放弃计划；然而 ECPAT 与 Geierhaas 等人的欧洲人口研究表明，多数人表示支持用于检测 CSAM 的 CSS，凸显了"多数人"与"专家"之间的潜在错位。

为考察不同群体的态度差异，本文从两方面扩展既有工作：首先开展定性访谈，随后对美国人口样本进行研究，比较安全与隐私研究者与普通美国民众对 CSS 的态度。

摘要未给出量化结果，核心贡献是提供美国人口样本与研究者群体的对比性实证数据。

我认为用实证方法检验"专家反对 vs 民众支持"的断层对政策讨论有直接价值，美国样本与欧洲研究的跨文化对比可揭示价值观差异，访谈与问卷的混合设计增强了结论的解释力。需要注意的局限是：问卷中如何呈现 CSS 的技术细节（误报率、可扩展性、执法影响）会显著影响受访者态度，问题框架本身即是研究的一部分。

#### "Why Would Money Protect me from Cyber Bullying?": A Mixed-Methods Study of Personal Cyber Insurance.
- **作者**: Rachiyta Jain, Temima Hrle, Margherita Marinetti, Adam D. G. Jenkins, Rainer Böhme, Daniel W. Woods
- **方向**: 社会工程与人类因素
- **解读**: 个人可能成为安全事件、隐私泄露、在线诈骗与社交媒体滥用的受害者；除预防外，用户还需为"不幸发生"准备应对策略。个人网络保险正是理解数字伤害与应对机制的重要窗口，但此前缺乏针对美英个人网络保险市场的系统研究，供需两侧的认知均属空白。

方法上，作者采用混合方法：供给侧对24份保单进行内容分析，需求侧面向584名受访者（美英各半）开展问卷调查，并引入"风险"与"不确定性"的概念框架分析购买决策中的认知障碍。

结果显示，保单主要补偿安全事件与欺诈，略过半数的保单覆盖网络欺凌，且与德国先前研究相比，英国的覆盖情况存在显著差异；需求侧仅有1.6%的受访者已购买、8.5%知晓该产品。供需认知差距在身份盗窃上最宽、在网络欺凌上最窄；受访者对各类事件频率的总体估计校准良好，金融事件被认为影响最大，网络欺凌的估计影响很低；购买顾虑集中于合同细节、报告要求、受害统计与解决方案获取的不确定性。

我认为该工作作为首个美英个人网络保险的混合方法研究，揭示了"低知晓率+供需认知错位"的市场症结，对保险产品设计与数字伤害治理有直接价值。但摘要缺乏显著性差异的具体量化数值，且市场仍处早期，结论外推需谨慎；"风险"与"不确定性"的区分虽有理论深度，二者在实际购买决策中的交互作用还需后续研究验证。


#### Security and Privacy Experiences of First- and Second-Generation Pakistani Immigrants to the US: Perceptions, Practices, Challenges, and Parent-Child Dynamics.
- **作者**: Warda Usman, John Sadik, Taha, Ran Elgedawy, Scott Ruoti, Daniel Zappala
- **方向**: 社会工程与人类因素
- **解读**: 少数族裔移民是安全隐私研究中的弱势人群，其在美国的数字安全实践与面临的独特威胁（歧视、监控、孤立）未被充分理解；父母与子女的代际动态如何影响安全习惯的习得与调适也缺少实证研究。

方法上，作者对25名在美巴基斯坦移民进行半结构化访谈，比较第一代与第二代移民在安全隐私感知、实践与挑战上的差异，并考察两代人在技术使用上的相互支持机制。

结果发现，第一代移民因其穆斯林身份感知到更高的歧视、监控与孤立风险，在线上自我表达与自我审查之间存在明显张力；第二代移民则较快适应美国生活，大多不面临这些挑战；两代人在应对感知威胁时相互支持、共同使用技术。作者据此呼吁针对高风险人群的定制化数字安全举措。

我认为该研究以"代际"为透镜切入移民数字安全，补充了以穆斯林移民为代表的高风险群体的质性证据，对设计包容性安全方案有现实意义。局限在于25人的样本以质性归纳为主，未量化不同威胁的实际发生率；"自我审查"究竟属于合理防御还是自由受限存在价值判断空间，需要更大规模、多族群对比研究支撑其外推结论。


#### Let's Get Visual - Testing Visual Analogies and Metaphors for Conveying Privacy Policies and Data Handling Information.
- **作者**: Verena Zimmermann, Adrienn Toth, Hannah Sievers, Linda Fanconi, Yanis Isenring, Mona Henz, Alina Stöver, Nina Gerber
- **方向**: 社会工程与人类因素
- **解读**: GDPR 等法规把知情同意的责任主要推给用户，但冗长的隐私政策与误导性的 cookie 提示很少促成真正的知情同意；图标化、信息结构化等既有改进手段效果参差，隐私信息呈现方式仍是可用隐私的核心难题。

方法上，作者采用被试间设计（N=379），检验将信息嵌入视觉隐喻与类比是否比纯文本更能支持用户的隐私决策，并考察动态反馈能否帮助用户理解其决策的后果。

结果显示，文本与可视化条件在决策一致性上未出现显著差异，但用户认为可视化比文本更合适、更美观；作者据此认为可视化在隐私增强技术与新兴助手等场景仍有使用价值，并建议未来考察实际部署效果与设计变体。

我认为该研究对"可视化必然更好"的直觉给出了严谨的证伪式检验——美观不等于更有效的知情决策，为可用隐私领域提供了重要校准。但"无显著差异"可能源于任务复杂度或被试同质性，结论的生态效度有限；动态反馈未起作用的发现也提示，单纯改变呈现方式难以弥补隐私政策本身的复杂性与用户动机不足等结构性问题。


#### "I'm Pretty Expert and I Still Screw It Up": Qualitative Insights into Experiences and Challenges of Designing and Implementing Cryptographic Library APIs.
- **作者**: Juliane Schmüser, Philip Klostermeyer, Kay Friedrich, Sascha Fahl
- **方向**: 社会工程与人类因素
- **解读**: 密码库是软件系统安全的关键组件，但其误用已引发多起安全事件；此前研究多关注开发者"使用"API 时的困难，而设计者与实现者如何决策、这些决策如何塑造密码库本身却几乎未知。

方法上，作者对21位经验丰富的密码库设计/实现者进行半结构化访谈，用主题分析归纳设计实现过程中跨抽象层级的挑战及其成因。

结果发现，挑战横跨多个抽象层级，并受标准、其他库、遗留代码与开发者直觉的强烈影响；开发者难以找到平衡安全、可用性与灵活性的最佳抽象层级，缺乏定义可用性并达成该平衡的系统性知识，只能依赖自测、个人经验与主观观点。据此作者提出将研究型指导纳入标准化流程、为 API 决策提供经验验证支持等建议。

我认为该研究把密码可用性研究从"下游用户"前移到"上游设计者"，揭示设计决策的高度个人化与缺乏实证支撑，直指密码误用问题的根源之一。局限在于质性方法难以量化不同决策模式的后果，设计者的自我报告与实际安全表现未必一致；"把研究建议写入标准"的呼吁面临标准制定周期长、参与者多元等现实阻力，落地路径仍需探索。


#### Transparency in Usable Privacy and Security Research: Scholars' Perspectives, Practices, and Recommendations.
- **作者**: Jan H. Klemmer, Juliane Schmüser, Byron M. Lowens, Fabian Fischer, Lea Schmüser, Florian Schaub, Sascha Fahl
- **方向**: 社会工程与人类因素
- **解读**: 透明报告是良好科研实践与可信科学的基础，有助于理解研究过程、评估贡献有效性并支持复现；但面对其他领域的可复现性危机，安全隐私（S&P）乃至可用隐私安全（UPS）社区仍缺乏清晰的透明报告标准。

方法上，作者对24位研究者进行半结构化访谈，了解 UPS 社区当前的透明度实践、相关挑战与障碍。

结果显示，研究者重视并已采用若干透明度实践；但一种"无激励、弊大于利"的隐性标准阻碍了透明度进一步推进。据此作者提出面向发表场所的建议：通过改造工件评估（如扩展到研究材料类工件）、放宽页面限制与附录约束等机制激励透明报告。

我认为该研究把可复现性危机引入 UPS 社区的自我审视，访谈揭示的"隐性标准+激励缺失"机制直指问题核心——不是研究者不愿透明，而是制度性激励失衡；所提建议务实可操作。局限在于24个样本以研究者自我报告为主，未覆盖审稿人与出版方视角，且"激励设计"的实际效果需试点验证；该工作与安全社区数据共享实践研究形成互补，共同推动 S&P 研究的可复现转型。


#### SoK: A Framework and Guide for Human-Centered Threat Modeling in Security and Privacy Research.
- **作者**: Warda Usman, Daniel Zappala
- **方向**: 社会工程与人类因素
- **解读**: 以人为中心的威胁建模用于识别普通人面临的安全隐私威胁及缓解方式，常是理解特定群体需求与体验的第一步；但相比成熟的系统威胁建模，该领域研究零散、实践碎片化，缺乏系统性梳理。

方法上，作者系统化梳理以人为中心的威胁建模：收集 78 篇相关论文作为语料，用质性分析理解研究者如何实施威胁建模，即如何引出并建立威胁模型。

结果显示，研究产出以最佳实践为基础的概念框架与操作指南，阐明其与系统威胁建模的差异，帮助新老研究者聚焦于"人的安全与实践"这一核心。

我认为该 SoK 的价值在于为长期"各自为政"的人本威胁建模提供统一词汇与流程参照，78 篇语料与质性编码保证了结论覆盖面，框架加指南的交付形态对入门者尤其友好。局限在于质性综述的结论粒度取决于语料质量，对"威胁模型有效性如何被验证"（是否有评估环节）的系统考察摘要未提及；与系统威胁建模的差异论述还需在正文中给出更可操作的迁移建议，未来可发展为带实证评估的方法论标准。


#### Prevalence Overshadows Concerns? Understanding Chinese Users' Privacy Awareness and Expectations Towards LLM-Based Healthcare Consultation.
- **作者**: Zhihuang Liu, Ling Hu, Tongqing Zhou, Yonghao Tang, Zhiping Cai
- **方向**: 社会工程与人类因素
- **解读**: 大语言模型（LLM）加速进入医疗领域，敏感健康信息面临被未经授权暴露与访问的风险；在中国这类隐私意识有限的地区风险尤甚，而用户对 LLM 医疗咨询的感知与期望此前未被系统研究。

方法上，作者开展首个面向中国用户（n=846）的 LLM 医疗咨询研究：部署真实聊天机器人考察实际使用，并用情境完整性（contextual integrity）的信息流框架测量用户的隐私期望。

结果显示，流行度通过激发好奇与使用意愿放大了使用行为，从而掩盖隐私担忧：77.3% 的参与者倾向于使用该服务，72.9% 表示会采纳 LLM 生成的建议；值得注意的是出现"矛盾错觉"——用户的知识与担忧与其实际期望相悖，导致更大程度的数据暴露。

我认为该研究以情境完整性为理论透镜，揭示"流行度压倒担忧"的心理机制与"知而不防"的矛盾错觉，对医疗 LLM 的部署方与监管者是重要警示：用户并非不知风险，而是动机与期望结构使其选择暴露；77.3% 与 72.9% 的高接受率量化了市场冲动。局限在于自报数据与实际行为可能偏离，样本代表性（平台、地域）影响外推；如何把担忧转化为防护行为是后续关键，也提示医疗 AI 的知情同意设计必须考虑中国用户特有的信任与期望结构。

#### Exploring Parent-Child Perceptions on Safety in Generative AI: Concerns, Mitigation Strategies, and Design Implications.
- **作者**: Yaman Yu, Tanusree Sharma, Melinda Hu, Justin Wang, Yang Wang
- **方向**: 社会工程与人类因素
- **解读**: 青少年广泛使用生成式AI（Ofcom统计79%的13-17岁英国青少年在线用户），带来沉迷、隐私与有害内容等安全风险，但家长管控面临严峻挑战。本文采用混合方法：Reddit内容分析（收集712帖、8533条评论，筛选181条有效分析）加上7名青少年、13名家长的半结构化访谈。

研究发现亲子间存在显著认知鸿沟：青少年把Character.ai当"免费心理治疗"倾诉情感、建立虚拟恋爱关系、在群聊中调用AI机器人社交，远超家长以为的"学习工具"用途；家长担心数据收集、错误信息与不当内容，青少年则更担心对虚拟关系上瘾、AI被用来在社交群中传播伤害性内容、个人信息被未经授权做成机器人/表情包；双方都有认知误区（误以为AI像搜索引擎，部分家长以为AI会做事实核查）。

家长管控层面，主流GAI平台缺乏家长控制功能，家长只能依赖系统自带工具、手工查看历史、共享账号与主动沟通，难以实时监控动态生成的内容，也缺乏可靠的风险教育资源；作者据此提出适龄内容审核、可定制家长控制工具与透明风险披露等设计建议。

我的思考：首次系统刻画亲子在GAI安全上的认知鸿沟，"信息源不对称"（孩子经TikTok、网红广告接触风险平台而家长不知）是关键发现；样本量小、依赖自报，但Reddit自然语料提供了生态效度，对平台责任与设计干预有直接参考价值。

- **arXiv PDF**: [Exploring Parent-Child Perceptions on Safety in Generative AI: Concerns, Mitigation Strategies, and Design Implications.](https://arxiv.org/pdf/2406.10461)

#### Security Perceptions of Users in Stablecoins: Advantages and Risks within the Cryptocurrency Ecosystem.
- **作者**: Maggie Yongqi Guan, Yaman Yu, Tanusree Sharma, Molly Zhuangtong Huang, Kaihua Qin, Yang Wang, Kanye Ye Wang
- **方向**: 社会工程与人类因素
- **解读**: 
稳定币（与资产锚定以维持价格稳定的加密货币）已成为生态重要组成部分，但既有研究多从技术与理论视角考察其安全性，缺乏对用户风险认知与实际行为的实证调查。

方法上，作者开展混合方法研究：基于文献构建交互框架以指导访谈协议设计，进行半结构化访谈（n=21），并分析 Reddit 数据（9,326 条帖子），考察用户在稳定币实践中的感知与行为。

研究发现参与者将监管合规视为稳定币相较其他加密货币的关键优势，但同时提出多重担忧：法币抵押型稳定币的中心化风险、加密抵押型稳定币因依赖全自动执行带来的挑战，以及用户对算法型稳定币复杂机制的困惑。据此提出改进用户教育、优化相关机制以促进更安全使用的建议。

我的思考：该工作补齐了稳定币安全研究中缺失的用户视角，访谈与社交大数据互为印证，方法扎实；局限是样本集中于特定社区，代表性有限。其政策含义明确——稳定币安全不仅是技术问题，用户认知、透明度与教育同样关键，对监管设计有参考意义。


#### "It's been Lovely Watching you": Institutional Decision-Making on Online Proctoring Software.
- **作者**: Elisa Shioji, Ani Meliksetyan, Lucy Simko, Ryan Watkins, Adam J. Aviv, Shaanan Cohney
- **方向**: 社会工程与人类因素
- **解读**: 
高校为维护在线考试诚信而广泛采用远程监考软件，但其引发了隐私（监控学生卧室）、数据处理、种族偏见监测及本地权限要求过高等安全与伦理问题；已有研究关注教育者认知，却缺乏对高层管理者制度级决策过程的理解。

方法上，作者访谈了 20 位来自美国与澳大利亚高校的管理者，探究其所在大学为何集中决定采用（或不采用）此类软件。

研究发现治理流程通常涵盖行政、法务、IT 团队；即便在 COVID-19 疫情期间仓促上线，学生有时仍被结构性排除在决策过程之外。学校在考试诚信需求与伦理担忧、长期运营成本等问题之间权衡，并尝试以隐私措施缓解各方关切。

我的思考：该工作首次系统揭示在线监考软件的"制度决策黑箱"，对教育技术采购治理有实践指导意义；局限是 20 人的访谈样本且集中于美澳高校。隐含风险值得警惕：决策中缺乏学生代表可能加剧隐私与伦理失衡，研究为混合教学常态化下的监考技术选择提供了决策框架。


#### "You Have to Ignore the Dangers": User Perceptions of the Security and Privacy Benefits of WhatsApp Mods.

- **作者**: Collins W. Munyendo, Kentrell Owens, Faith Strong, Shaoqi Wang, Adam J. Aviv, Tadayoshi Kohno, Franziska Roesner

- **方向**: 社会工程与人类因素

- **解读**: WhatsApp 是最流行的社交消息平台，其修改版（mod）日益流行，宣传额外功能与定制能力。但部分功能——如保留已删除消息与状态——使 mod 用户能侵犯他人隐私，并有严重的安全隐患。

方法上，本文通过访谈研究（n=20，对象为肯尼亚用户，该国 WhatsApp 使用率居全球前列）探索用户对 mod 的看法：许多人使用"高级"功能是为了自我保护（如用"anti-delete"规避法律责任），也有人承认用其隐藏行为或跟踪他人。为理解用户预期与应用实际行为之间的偏差，作者识别并分析了常见 mod（如 GB WhatsApp）中的 13 类功能实例，对照其宣称与实际行为。

结果：摘要截断，未给出量化结果；研究发现围绕动机（自我保护 vs 侵犯他人）与"宣称-实际"差距展开，揭示用户对 mod 安全与隐私后果的认知错位。

我的思考：本文是"非官方应用生态"用户研究的范例——mod 在发展中国家极其流行（绕过官方限制、节省流量、附加功能），但用户往往高估其保护价值、低估其恶意潜力（mod 常捆绑恶意代码或后门）。n=20 的深度访谈在质性研究上足够，但结论外推到其他地区需谨慎；13 类功能的"宣称-实际"对比是亮点，为官方治理策略提供了依据。与既往 mod 安全测量（如检测恶意性）相比，本文补充了用户侧动机与认知，两个视角互补。


#### (Blind) Users Really Do Heed Aural Telephone Scam Warnings.
- **作者**: Filipo Sharevski, Jennifer Vander Loop, Bill Evans, Alexander Ponticello
- **方向**: 社会工程与人类因素
- **解读**: 
电话诈骗因呼叫者ID可轻易伪造而泛滥（美国年损失以亿美元计），现有防护多为视觉告警，对法定盲人不友好，且STIR/SHAKEN的"Scam Likely"标记常被误标。作者提出听觉版诈骗告警，研究其在自然场景下对盲人（n=36）与视力正常者（n=36）的防护效果。

研究采用冷呼叫的真实环境：以2×3×2设计（盲人/视力正常两组×基线/短告警/情境告警三种条件×降息与社保诈骗两种场景），通过CallFire的IVR记录参与者行为，事后访谈并取得知情同意。告警文本由试点研究打磨，如"政府机构绝不会在电话中索要社保号"。

结果只有2名盲人按下诈骗要求的"1"键，且均在社保诈骗的情境告警条件下：一人因读屏器导航误触，另一人是有意拖延骗子时间以保护易受骗人群。两类用户都认为情境告警是强效可用线索，但视力正常者更偏好视觉版本；隐私顾虑集中于"缺乏控制与透明度"；还发现误标为Scam Likely时读屏器不播报真实号码，反而阻碍接听合法来电。

这是少有的在自然主义环境中检验无障碍安全告警的研究，把"可访问性"与"可用安全"结合，为Google等AI情境告警提供了实证支撑与设计建议（触觉反馈、语速语调、语言选择）。局限是样本量小、仅美国语境、轻微欺骗的伦理复杂性；与现有视觉告警的对比也非受控实验，推广需谨慎。
- **arXiv PDF**: [(Blind) Users Really Do Heed Aural Telephone Scam Warnings.](https://arxiv.org/pdf/2412.04014)

### 程序分析与形式化验证（6 篇）

#### CMASan: Custom Memory Allocator-aware Address Sanitizer.

- **作者**: Junwha Hong, Wonil Jang, Mijung Kim, Lei Yu, Yonghwi Kwon, Yuseok Jeon

- **方向**: 程序分析与形式化验证

- **解读**: 自定义内存分配器（CMA）常用于提升效率或安全性，但其分配的对象同样易受内存错误影响。遗憾的是，现有检测工具（包括 ASan）主要针对标准分配器设计，无法正确适配 CMA 内部逻辑，导致大量真实缺陷逃过检测。本文提出 CMASan，首个 CMA 感知的地址消毒器，在无需专家知识、无需改代码、无需改动 CMA 逻辑的前提下检测 ASan 遗漏的缺陷。

方法上，CMASan 需理解自定义分配器的元数据与对象布局，正确定位 CMA 管理的堆对象边界并实施访问消毒。设计强调自动化与透明——不侵入被测代码，不要求使用者理解 CMA 实现，与需要人工标注或改写分配器的方法形成对比，显著降低采用门槛。

评估显示：CMASan 成功识别 19 个此前未知、ASan 无法检测的缺陷，其中一些已潜伏长达 9 年——直观说明 CMA 区域检测盲区长期存在。而相对 ASan 仅增加 9.63% 开销，性能代价可控，具备日常集成可行性。

我的思考：本文精准定位消毒器生态的真实盲区：CMA 广泛存在于生产软件（jemalloc、tcmalloc 及安全加固分配器），ASan 的"标准分配器假设"使其在这些程序上形同虚设。以"无需专家介入"为目标直接服务大规模现实代码库，19 个潜伏缺陷提供有力动机证据，9.63% 开销回答实用性之问。局限：对 CMA 类型覆盖范围与误报率未展开。与标记/仿真适配方案相比，CMASan 的自动化为消毒器研究补上关键一环，对依赖自定义分配器的高性能系统安全检测有直接价值。


#### GoSonar: Detecting Logical Vulnerabilities in Memory Safe Language Using Inductive Constraint Reasoning.

- **作者**: Md Sakib Anwar, Carter Yagemann, Zhiqiang Lin

- **方向**: 程序分析与形式化验证

- **解读**: 全球推动采用内存安全语言，但随之而来的"逻辑漏洞"研究存在空白：没有缓冲区溢出等内存安全问题后，逻辑漏洞成为最严峻威胁。Go 作为云原生主流语言，资源可用性至关重要，尤其易受非终止、资源耗尽类漏洞影响。本文提出基于归纳约束推理的方法，专门检测 Go 程序中的非终止/无界递归缺陷。

方法上，GoSonar 采用二进制级欠约束符号执行收集多次递归迭代的约束，再施加一阶导数约束对函数建模与分类，判定子目标是否随迭代收敛，区分"正常终止"与"不受控递归"。该设计规避了 Go 源码级分析的挑战（goroutine、接口动态分派等），直接在二进制层面以符号化方式逼近终止性判定。

在标准化数据集上，GoSonar 优于当代同类工具，并在真实复杂 Go 程序中有效检测非终止问题；实际应用揭示出 Go 标准库中 5 个此前未知的不受控递归缺陷，证明方法能发现真实可部署的影响。

我的思考：本文把"终止性分析"这一验证难题工程化为可扩展的漏洞检测工具，瞄准 Go 这一云基础设施核心语言——无界递归可直接转化为 CPU/内存耗尽型 DoS，威胁模型成立且影响面大。"归纳约束 + 一阶导数约束"把跨多轮迭代的不变量刻画转化为可判定的约束求解，是方法上的巧思。局限：终止性判定本质不可判定，存在漏报/误报边界，二进制符号执行对大型程序的可扩展性是持续挑战。与源码级工具相比，GoSonar 走"二进制 + 约束推理"路线，为内存安全语言的逻辑漏洞检测提供新路径。


#### Evaluating the Effectiveness of Memory Safety Sanitizers.

- **作者**: Emanuel Q. Vintila, Philipp Zieris, Julian Horsch

- **方向**: 程序分析与形式化验证

- **解读**: C/C++ 以牺牲内存安全换取高性能，内存错误成为最严重漏洞的根源。各类消毒器能力差异巨大——覆盖不同区域、检测不同缺陷子类。然而既有研究多为概念分类，定量评估聚焦性能而非真实的缺陷检出能力，用户难以判断哪个消毒器在何种缺陷上真正有效。本文提出 MSET 并对主流消毒器开展大规模功能评估。

方法上，作者把内存缺陷系统解构为独立性质：缺陷区域、破坏方式、访问类型、目标缓冲区。基于该分解，MSET 自动生成由小型独特代码模板组合而成的测试用例，覆盖各种缓冲区溢出、下溢与 use-after-free，把"评估消毒器"转化为可控可复现的基准测试。

评估揭示：消毒器的标称能力与实际表现存在差异——目标相似的消毒器检出潜力各不相同；更值得注意的是，多个消毒器因实现不完整或有缺陷而漏检本应发现的问题，说明实现质量是决定工具有效性的关键变量。MSET 已开源，供研究者挖掘"丢失的潜力"、定位实现缺陷。

我的思考：本文把消毒器评估从性能竞赛转向能力体检，四维缺陷分解（区域/破坏方式/访问类型/目标缓冲区）提供可组合的测试空间生成器，比手工 PoC 更系统。"实现不完整导致漏检"的发现尤其重要，提醒社区理论模型与工程实现之间存在鸿沟。局限：模板覆盖面取决于对缺陷空间的枚举，未评估误报率影响。与 ASan/Valgrind 对比类工作相比，MSET 作为可扩展评估框架为消毒器领域的标准化体检奠定基础，对工具选型与实现修复均有价值。


#### Redefining Indirect Call Analysis with KallGraph.
- **作者**: Guoren Li, Manu Sridharan, Zhiyun Qian
- **方向**: 程序分析与形式化验证
- **解读**: 
调用图构造是大量静态分析应用的前提；SOTA 方法通过回退到所谓的"类型分析"来避免精确但昂贵的指针追踪，从而可扩展到大程序（如 Linux 内核），但其 ad hoc 设计存在缺陷。

方法上，作者对类型化间接调用分析进行深入评估，揭示两类新洞察：其一，近期多项工作的健全性声明在不少情况下不成立，导致遗漏间接调用目标；其二，部分分析在多个方面过度保守，导致大量误报。基于这些发现，将类型分析转化为统一传统方法的混合框架，开发出同时解决精度与扩展性问题的实用框架 KallGraph。

结果显示，KallGraph 在剪枝上最多削减 90% 的调用目标，同时消除成百上千的遗漏调用；完全并行化后可分析完整内核，耗时从数十分钟到数小时不等。

我的思考：对"类型分析是健全的"这一普遍假设的实证戳破很有价值，混合框架在精度与可扩展性间取得出色平衡；局限是评估集中于内核等 C 程序。对 Linux 内核污点分析、漏洞挖掘等安全应用有直接增益，是调用图分析的重要推进。


#### Empc: Effective Path Prioritization for Symbolic Execution with Path Cover.
- **作者**: Shuangjie Yao, Dongdong She
- **方向**: 程序分析与形式化验证
- **解读**: 
符号执行能形式化推理程序行为并检测软件缺陷，但受限于路径爆炸——路径数随程序规模指数增长，严重损害可扩展性；现有工作多用静态规则或高排名启发式对指数级路径排序，却难以跨程序泛化。

方法上，提出 Empc：核心洞察是并非所有代码区域都同等重要——采用最小路径覆盖（MPC）选取小规模代码区域子集以鼓励多样性，计算多个 MPC 并在这些区域内部引导搜索，而非按路径排序；基于 KLEE 实现。

结果显示，Empc 比 KLEE 的最佳策略多覆盖 19.6% 基本块，比 SOTA 工作 cgs 多覆盖 24.4% 行；发现 24 个安全违规；内存使用较 KLEE 最多降低 93.5%（状态数 88.6%）。

我的思考：从"按路径排序"转向"按覆盖区域引导"的思路简洁有效，在提升覆盖与漏洞发现的同时大幅降低内存，直击路径爆炸痛点；区域级引导比路径级更稳健、更易泛化。局限是依赖覆盖质量度量，每个程序的最优 MPC 数量与区域粒度仍需调参。


#### Faster Verification of Faster Implementations: Combining Deductive and Circuit-Based Reasoning in EasyCrypt.
- **作者**: José Bacelar Almeida, Gustavo Xavier Delerue Marinho Alves, Manuel Barbosa, Gilles Barthe, Luís Esquível, Vincent Hwang, Tiago Oliveira, Hugo Pacheco, Peter Schwabe, Pierre-Yves Strub
- **方向**: 程序分析与形式化验证
- **解读**: 
高度优化的密码学汇编代码大量使用架构特定指令，底层计算逻辑被复杂指令用法掩盖，使传统高层形式验证极其困难，形成"实现越快、越难验证"的困局。

本文提出混合形式验证方法，结合高层演绎推理与电路级推理，并沿两个互补方向扩展规模：其一，降低底层函数（计算逻辑被架构指令混淆）的证明负担；其二，通过等价性检查摊销证明成本，把对一个实现的验证传播到采用不同优化或面向不同架构的其他实现。方法在EasyCrypt证明助手的扩展中实现，并重新验证了Jasmin中形式化验证过的ML-KEM。

作为结果，作者获得了首个性能可与最快非验证x86-64实现相媲美、且经过正式验证的ML-KEM实现。

"证明摊销"是让形式验证跟上性能优化的关键思路，工程价值显著；电路级推理有效处理了架构特定指令带来的证明困难。与Fiat-Crypto、HACL*等路线相比，本文强调跨实现、跨架构的证明复用，为高性能密码学库的验证提供了可扩展的新范式。


### 其他（5 篇）

#### SoK: Digging into the Digital Underworld of Stolen Data Markets.
- **作者**: Tina Marjanov, Alice Hutchings
- **方向**: 其他
- **解读**:
被盗数据问题在过去数十年间从机会主义个人的滋扰演变为高度组织化、可盈利的产业，催生了大量试图记录和理解地下市场的研究。但这些研究分散且视角各异，缺乏对市场长期演化规律的系统梳理。

该 SoK 回顾了 15 年间关于被盗数据市场的研究，归纳研究者记录的市场模式与趋势：市场格局持续变化——流行的数据类型与承载市场的平台都在迁移；同时观察到市场生命周期在研究观测期内持续缩短。

论文还指出该领域研究存在若干不足，特别是对市场覆盖度低、涉及语言多样性不足；并提出未来研究方向，例如更准确地衡量数据泄露与市场出现之间的真实成本错配，以及追踪社区跨平台流动。

纵览 15 年文献的视角是该文的核心价值，生命周期缩短与平台迁移的观察对执法与防御有直接参考意义。局限在于结论依赖研究者已公开的文献，本身存在覆盖偏差，可能低估非英语市场。其"成本错配"研究议程为量化地下经济影响提供了清晰路线。


#### Liquefaction: Privately Liquefying Blockchain Assets.
- **作者**: James Austgen, Andrés Fábrega, Mahimna Kelkar, Dani Vilardell, Sarah Allen, Kushal Babel, Jay Yu, Ari Juels
- **方向**: 其他
- **解读**: 区块链安全模型普遍假设"私钥及资产由单一实体控制"(SEAO假设)。本文提出Liquefaction钱包平台,利用可信执行环境(TEE)实现密钥托管,为私钥附加丰富的多用户访问策略,使单地址的凭证与资产可被私下出租、共享、池化且链上无直接痕迹,系统性打破该假设,威胁锁定代币、DAO投票、空投、SBT、忠诚度积分与二次方投票等应用的安全经济模型。

设计核心是首个形式化的密钥托管策略模型:以资产-时间分割为原则,任意时刻每项资产仅由单一玩家独占控制;子策略(独立智能合约)构成委托树,只能新增或到期、不可撤销,策略更新函数β负责消除冲突。钱包仅签署Ethereum交易、ERC-191与EIP-712三类消息,并用nonce加链上包含证明防止子策略预签名双花,同时保证策略隐私(玩家只能经LWVerify了解他人权限)。

原型以Solidity部署于Oasis Sapphire(TEE区块链)。作者完整实现Dark DAO(链上投票贿赂)与Dark DAO Lite(基于DD代币的自治拍卖);测量交易包含周期在latest/justified/finalized三种预言机下平均49.0/648.7/1036.9秒,策略部署约7.78M gas(约$0.054),远低于以太坊交易成本;并枚举粉尘攻击缓解、隐私DAO等有益应用及Complete Knowledge(CK)证明这一现成对策。

本文把"SEAO假设可被打破"从理论设想推进为可运行系统,并给出密钥托管策略的通用模型,贡献系统而扎实。局限:安全模型理想化(排除TEE侧信道与活性失败,虽有fallback设计但复杂);"攻击还是建设"的边界主观(作者亦承认)。与CK结合后,它倒逼链上应用重新审视所有权执行方式,为后续钱包与协议设计划定新基线。

- **arXiv PDF**: [Liquefaction: Privately Liquefying Blockchain Assets.](https://arxiv.org/pdf/2412.02634)

#### Decentralization of Ethereum's Builder Market.
- **作者**: Sen Yang, Kartik Nayak, Fan Zhang
- **方向**: 其他
- **解读**: 区块链以去中心化为安全基石,但以太坊builder市场(MEV-Boost拍卖)高度集中——两家builder产出超85%区块。社区普遍相信"builder集中无碍",本文用迄今最大规模的拍卖数据挑战该信念,量化中心化给proposer造成的显著损失及其对PBS目标和MEV治理方案的危害。

作者自2022年起收集约120亿条partial bids,并获ultra sound relay的301,479场完整拍卖(2023年4-12月,含3.26亿条full bids);按拍卖理论将proposer损失分解为"非竞争损失"与"能力不平等损失",提出pivotal level度量提供方的持续影响力,并据理性竞争假设推断排他性私有订单流(整合)关系。

91.8%的拍卖是竞争的,非竞争损失仅0.5%-1.7%,但能力不平等损失达5.6%-11.5%,是不平等而非缺乏竞争主导;2024年每日估计proposer损失中位数升至21.2%(此前4.1%)。约80%的区块价值来自私有订单流;Banana Gun、jaredfromsubway.eth、Maestro三个关键提供方与Titan/Beaver独家整合,仅Banana Gun→Titan即贡献2024年研究期估计损失的32.6%。

本文首次以完整竞拍数据(而非链上数据)实证builder市场,识别出先前未知的整合模式,并论证ePBS、MEV burn等拟议方案因未处理提供方激励而失效,提炼出"订单流安全保证+去整合博弈保证"两条必要性质,为该领域经济学分析奠基。局限:真实价值TV为估计(链下返款不可见),2024年损失只是下界;对策层面仍无落地解法。

- **arXiv PDF**: [Decentralization of Ethereum's Builder Market.](https://arxiv.org/pdf/2405.01329)

#### Data to Infinity and Beyond: Examining Data Sharing and Reuse Practices in the Computer Security Community.
- **作者**: Anna Crowder, Allison Lu, Kevin Childs, Carson Stillman, Patrick Traynor, Kevin R. B. Butler
- **方向**: 其他
- **解读**: 共享高质量、可复用的研究数据能让社区避免重复采集、加速进展；但安全领域的工件讨论聚焦于可复现性与源代码可得性，数据本身的"可复用性"缺乏系统考察，难以给出改进依据。

方法上，作者考察测量类研究的数据共享实践：覆盖 2019–2023 五年间七个会议，识别出 948 篇以创建数据集为贡献之一的论文，分析其中 265 个可访问数据集，评估其可理解性与可复用程度。

结果显示，数据集在结构与文档上缺乏一致性，导致部分数据无法被有效共享；复用率整体偏低，在数据性质不适于复用的领域尤其如此；作者据此提出数据驱动的改进建议，并鼓励作者以明确目标为导向、使数据策略与目标对齐。

我认为这是少有的对安全社区"数据复用"而非"代码复现"的系统体检，948 篇到 265 个可访问数据集的漏斗本身就很说明问题（可得性约 28%），为开源数据治理提供了量化基线。局限在于测量类会议的子领域偏差（安全测量数据天然更难标准化），文档一致性与复用率的关系属描述性发现，缺乏干预实验验证；"按目标设计数据"的建议具体可行，但落地需要配套的激励与基础设施支撑。


#### "We can't Allow IoT Vendors to Pass off all Such Liability to the Consumer": Investigating the U.S. Legal Perspectives on Liability for IoT Product Security.
- **作者**: Prianka Mandal, Amit Seal Ami, Iria Giuffrida, Daniel Shin, Ella Sullivan, Adwait Nadkarni
- **方向**: 其他
- **解读**: 
随着IoT监管环境演变，厂商开始为产品做安全认证，但认证失败导致用户损害时责任如何归属尚无定论——即经认证的产品仍存在被利用的漏洞并造成损害时，谁应担责。这一问题对漏洞检测投入有重大影响，是一个基础且紧迫的法律-安全交叉问题。

本文通过定性分析20家IoT厂商的合同文件，考察厂商-用户条款中责任目前如何界定；再结合对18位法律专业人士的专家调查，交叉审视合同实践与法律专家观点之间的差异。

研究得出14项关键发现（F1-F14）：合同普遍以最大限度排除厂商责任（部分排除甚至不合法），而法律专家的观点与这些由律师起草的合同形成鲜明对比。作者将发现提炼为三大主题，呼吁建立稳健清晰的责任框架，以激励厂商达到应有的安全与隐私标准。

这是少见的法律实证与安全研究结合的成果，方法组合（合同定性分析+专家调查）严谨，但局限在于美国语境与有限样本规模。对政策制定者与安全社区均有价值：责任不清会直接削弱"漏洞发现-修复"的正向激励，该研究为立法提供了实证依据。

