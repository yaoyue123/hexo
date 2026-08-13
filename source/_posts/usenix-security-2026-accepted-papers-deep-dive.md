---
title: USENIX Security 2026 录用论文深度解读（165 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, USENIX Security, 2026]
---

USENIX Security 2026（第 35 届）于 2026 年 8 月在波士顿举行，第一轮录用 165 篇。本文按研究方向分类，对每篇论文基于全文精读给出深度解读（含独立分析思考），并附论文 PDF 链接。

## 全部 165 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（17 篇）

#### Imitative Membership Inference Attack
- **作者**: Yuntao Du and Yuetian Chen; Hanshen Xiao; Bruno Ribeiro and Ninghui Li
- **方向**: AI/ML安全
- **解读**: 成员推断攻击（MIA）是衡量模型隐私泄漏的基准工具，但 SOTA 攻击（LiRA、PMIA）需训练数百个影子模型：LiRA 在 CIFAR-10 上用官方实现约需 6 天（单块 A100），令隐私审计与复现难以负担。作者指出根源在于影子训练"目标无关"——只学到成员/非成员的一般模式，对难攻击样本方差大，故需海量模型平均。

IMIA 提出"模仿训练"：用加权 logits 匹配损失（对真实类与次高置信类加权）训练模仿目标模型输出的 imitative_out 模型，再用专门挑选的同类 pivot 数据以交叉熵继续微调得到 imitative_in 模型，显式编码成员信号；推断用非参数得分——观测置信分数到 in/out 分布均值平方距离的差，避免高斯参数估计；非自适应设置下用同类样本代理近似 in 行为。仅需 10 个模仿模型，对比 LiRA/PMIA 的 256 个。

非自适应设置全面超越 PMIA：FMNIST 上 TPR@0%FPR 2.52% vs 0.25%（约 9 倍+），MNIST 1.01% vs 0.30%；自适应设置接近或超过 LiRA（CIFAR-10 的 TPR@0%FPR 8.52% vs 5.41%）；计算量不足 SOTA 的 5%（CIFAR-10 从约 6 天降至 <7 小时，缩减超 95%）；预算充足时改用高斯似然比的 IMIAGaussian 变体表现更强；相比蒸馏类攻击（Attack-D、SeqMIA、GLiRA）优势明显。

我的思考："目标知情"替代"目标无关"是干净且可迁移的洞察，攻击效果与计算成本双赢，对需要反复审计的 DP 合规场景尤其实用；非参数得分规避分布假设是加分项。局限：需要黑盒访问目标模型输出、pivot 同类代理是近似、评估限于标准图像与非图像基准。IMIA 有望成为隐私审计的低成本默认工具，其模仿训练范式也可能启发防御研究。

- **论文 PDF**: [Imitative Membership Inference Attack](https://www.usenix.org/system/files/usenixsecurity26-du.pdf)

#### The Prompt Stealing Fallacy: Rethinking Metrics, Attacks, and Defenses
- **作者**: Zehang Deng, Haoyang Li, Wanlun Ma, Ruoxi Sun and Derui Wang, Minhui Xue, Haibo Hu, Sheng Wen and Yang Xiang
- **方向**: AI/ML安全
- **解读**: 文生图（T2I）模型的提示词已成为可交易的智力资产（PromptBase 均价约 $3.99），提示词窃取攻击（PSA）试图从生成图像逆向还原提示词。本文指出现有评估体系存在根本缺陷：广泛使用的 SITS 指标与攻击真实效果相关性极低，无法反映修饰词等高贡献 token 的恢复质量，导致已有攻击的有效性被系统性高估。
方法上，作者提出两个新指标：风格相似度（SS，基于 CSD 模型）和提示词显著性（PS = STS × 归一化 SIIS），并据此重估 PEZ、PH2P、Textual Inversion（白盒）与 PSteal、DI-FT、GPT-4o 等（黑盒）方法，指出白盒方法存在离散优化目标错配、黑盒方法存在与目标模型解耦的信息损失。随后提出黑盒攻击 PromptThief：先 SFT 预热策略模型，再用 GRPO 强化学习（奖励为 STS 与 Jaccard 混合）引导高贡献 token 恢复，最后用 SS 分数进行 30–67 次查询的候选搜索。防御侧提出 TokenGuard，含基于 token 贡献的对抗样本主动防御与特征级提示词水印被动方案。
实验表明 PromptThief 在分布内（SD1.4/Lexica）达 SIIS 0.89、STS 0.69、PS 0.53、SS 0.63，跨模型 OOD（DALL·E）为 0.89/0.71/0.61/0.70，较基线平均提升 14% 以上（SS/PS 提升 16% 以上）；特征级水印在各种图像变换下保持稳定检测且误报有界，而主动防御在自适应攻击下仅提供有限鲁棒性。
本文最大贡献是度量体系的纠偏——"PSA 是否有效"被重新定义，对高贡献 token 的重视直接服务商业化场景（可复用性 O2 与查询预算 O3）。PromptThief 将 RL 引入黑盒提示词恢复是合理路径，但水印方案的检测可信度依赖密钥管理与验证权威，落地仍有距离；主动防御失效的结论也提示该方向需要全新范式。

- **论文 PDF**: [The Prompt Stealing Fallacy: Rethinking Metrics, Attacks, and Defenses](https://www.usenix.org/system/files/usenixsecurity26-deng.pdf)

#### VSG-Safe: Spotting NSFW Video through Cross-Frame Evidence
- **作者**: Yuyang Zhang, Yihao Huang, Run Wang
- **方向**: AI/ML安全
- **解读**:
文生视频（T2V）模型（Sora、Hailuo，甚至 Grok "Spicy" 模式）被滥用于生成色情、暴力内容。这类内容的关键语义常分散在多帧之间（如持刀威胁需跨三帧推断人-刀-人关系），而现有审核要么逐帧检测、要么压成全局表征，只能捕获低层线索，在连续/不连续镜头上频繁漏检（动机实验：准确率从 camera 的 83.42% 跌至 62.41%/57.52%）。

提出以视频场景图为中间语义表征的 VSG-Safe。场景图生成三步：实体分割（重训 PVSG 模块+UniTrack 生成跨帧一致 track_ID）、掩码引导 VLM（BLIP）开放词汇属性提取、跨帧关系推断（配对模块+Transformer 编码器）。分类端为双通道 GNN（Dual-GNN）：属性通道与关系通道（两层 GATv2）经门控融合，协同实现跨帧推理与帧内属性识别。自建 Unsafe-VidGraph 数据集（340 视频、51k 帧、7 类 NSFW、3 种镜头模式）。

Unsafe-VidGraph 上 F1 达 97.62%，平均领先 7 个基线 42.72%（最佳基线 Qwen-VL 81.44%）；UGVD 上 F1 78.45%。分镜头 F1：camera 100%、continuous 97.44%、discontinuous 96.23%；分场景：实体 100%、显式关系 98.06%、隐式关系 96.06%；跨帧场景 96.84%（提升 54.66%）。基线在跨帧场景崩塌（MHSC 仅 8.97% F1）。

首次把场景图作为视频 NSFW 审核的语义表征，针对 AIGC 视频跨帧推理给出系统方案，数据集构建本身也是贡献；双通道解耦与门控融合设计合理。局限：管线依赖 PVSG/BLIP/UniTrack 组合，域外泛化未验证；7 类界定是任务化的，回避文化主观性；未考虑对抗性规避。

- **论文 PDF**: [VSG-Safe: Spotting NSFW Video through Cross-Frame Evidence](https://www.usenix.org/system/files/usenixsecurity26-zhang-yuyang.pdf)

#### VIPER Strike: Defeating Visual Reasoning CAPTCHAs via Structured Vision–Language Inference
- **作者**: Minfeng Qi and Dongyang He, Qin Wang, Lefeng Zhang
- **方向**: AI/ML安全
- **解读**: 

**问题**：视觉推理验证码（VRC）将视觉场景与自然语言指令结合，要求对物体、属性、空间关系做组合推理，被腾讯防水墙、极验等大规模部署，是当前最难攻克的验证码防线。现有解法分两派：视觉中心派（Holistic、GraphNet）依赖模板化检测器，新布局即失效；推理中心派（Oedipus）依赖 LLM，但细粒度视觉感知差，平均成功率仅 63.5%，远低于人类。两派都缺乏跨异构 VRC 部署的通用性。

**方法**：VIPER 是模块化攻击框架：多目标检测器（1200 张标注场景训练）结构化盘点物体（形状、颜色、朝向、坐标）；查询属性解析器（QIE）把指令解析为属性槽；Integrator 对齐剪枝；相对位置推断引擎（RPIE）用几何投影与点包含测试解析空间指代；最后由 Policymaker 按推理类型构造任务条件化提示词，让 LLM 只输出一个像素坐标，无需 chain-of-thought。

**结果**：在 VTT、Geetest、NetEase、Dingxiang、Shumei、Xiaodun 六个平台（每平台 1000 题 ground-truth）上，GPT-4o 后端准确率达 86.5%–98.5%：VTT 97.33% 超过人类基线 87.60%；全面超越 GraphNet（89.47%）、Holistic（88.12%）、Oedipus（70.23%）。四种 LLM 后端下完整版均达 81.9% 以上；消融显示纯 LLM 仅 7.1%–31.4%，加检测器后超 60%。端到端平均 8.3 秒，接近人类 7.0 秒。防御侧提出模板空间随机化（TSR），在 600 个定制问题上显著降低攻击者成功率。

**我的思考**：贡献扎实：首个跨平台统一 VRC 基准与"结构化感知+符号对齐+LLM 推理"解耦架构，准确率超人类说明 VRC 已不再可靠。局限：5–11 秒/题的延迟在批量注册等场景仍是瓶颈；TSR 仅初步验证，可能被对抗改写绕过；作为攻击研究需负责任披露。领域影响：为验证码设计者指出"人可解而机器难"的具体方向，也提醒业界视觉推理验证码并不安全。

- **论文 PDF**: [VIPER Strike: Defeating Visual Reasoning CAPTCHAs via Structured Vision–Language Inference](https://www.usenix.org/system/files/usenixsecurity26-qi-minfeng.pdf)

#### SoK: PHILTER: Uncovering Security and Functional Gaps in AI-based Phishing Website Detection Literature via an LLM-based Reasoning Framework
- **作者**: Mahbub Alam, Muhammad Lutfor Rahman, Sonjoy Kumar Paul
- **方向**: AI/ML安全
- **解读**: 

**问题**：钓鱼网站仍是网络犯罪主要入口（2024 年 45% 的钓鱼邮件含钓鱼网站链接，67.4% 的攻击引入 AI），学术界提出大量 AI 检测方法并常启发 Google Safe Browsing 等工业系统，但多数论文只报高精度，能否满足真实部署需求——对演化战术的韧性、良性页面鲁棒性、可解释性、隐私保护——并不清楚，且缺乏统一评估框架。

**方法**：PHILTER 提出结构化框架：4 项功能指标（F1 战术多样性、F2 良性页面多样性、F3 可解释性、F4 评估透明性）和 3 项安全指标（S1 概念漂移适应、S2 主动攻击抵抗、S3 隐私保护），配合代码簿驱动的两阶段流水线：o4-mini 与 gemini-2.5-pro 两个 LLM 提取证据并生成初步评估（要求引用原文以减少幻觉），不一致时仲裁，专家复核定稿；并提出特征/相似/身份/混合四类分类法解释设计权衡。

**结果**：对 55 篇论文的评估显示系统性缺口：无一满足全部 7 项要求，仅 18 篇在 1 个以上指标达 High。F1 无一篇达 High；F2 最弱（总体 75% Low）；S1 概念漂移 69% Low；S2 主动攻击抵抗（特征型 74% Low）；S3 隐私两极（特征型客户端 43% High，身份型 89% Low）。客户端部署隐私显著更好（61% High vs 服务端 12%）但其他指标更弱；满足安全要求的方法往往精度下降。

**我的思考**：这篇 SoK 把"精度叙事"戳穿为盲点掩护，给出可复现的评估框架、代码簿与建议清单（战术标注基准、良性页面控制、时间感知评估等），LLM+专家复核的可扩展方法论本身也是贡献。局限：High/Medium/Low 定性评估依赖专家判断与 LLM 证据提取质量；只评估学术文献，工业系统未同等纳入；分类粒度较粗。领域影响：为后续钓鱼检测研究提供明确验收标准，与 Arp 等关于 ML 安全方法学陷阱的研究相互印证。
- **论文 PDF**: [SoK: PHILTER: Uncovering Security and Functional Gaps in AI-based Phishing Website Detection Literature via an LLM-based Reasoning Framework](https://www.usenix.org/system/files/usenixsecurity26-alam.pdf)

#### On Evaluating the Robustness of Large Vision-Language Models via Untargeted Modality Alignment Breaking Adversarial Attack

- **作者**: Zhichao Li, Yaopeng Wang, Kui Ren and Chun Chen

- **方向**: AI/ML安全

- **解读**: 大视觉语言模型（LVLM）通过对齐视觉编码器与 LLM 表示空间实现多模态理解，也继承了视觉模态的对抗脆弱性（对抗样本可使 LVLM 自动驾驶路由完成率下降 61%）。既有黑盒评估多用目标攻击，只扰动视觉编码阶段，对 LVLM"对齐-推理"机制利用不足。本文提出 MABA（模态对齐破坏攻击），面向 LVLM 的迁移型非目标黑盒攻击。

方法上，MABA 同时攻击两个关键阶段。视觉编码阶段引入判别特征抑制模块：提取视觉表示中携带判别信息的高频分量，经 SVD 识别最判别特征并抑制，使扰动偏离原始语义、提升迁移性。模态对齐阶段设计互信息感知投影器：以最大化视觉与文本嵌入互信息为目标训练，捕获跨模态统计依赖、模拟受害者 LVLM 的适配器——首次在黑盒下构造通用替代适配器。

在图像描述任务上，MABA 使语义指标平均下降 58.37%，VQA 准确率下降 31.63%，达当时最优攻击性能。作者还在多个 LVLM 家族消融，剖析模型规模、可训练模块与模型能力对鲁棒性的影响。

我的思考：本文把攻击面从"视觉编码器"扩展到"模态对齐"这一被忽视的组件，并给出可迁移的替代适配器构造，直击 LVLM 架构咽喉；"压制判别性表征是迁移攻击核心"的观察具机理解释力。局限：替代适配器泛化依赖训练数据覆盖，对训练范式差异大的新模型需单独验证。相比仅扰动编码器的既有工作，MABA 展示了攻击整条多模态流水线的范式价值。
- **论文 PDF**: [On Evaluating the Robustness of Large Vision-Language Models via Untargeted Modality Alignment Breaking Adversarial Attack](https://www.usenix.org/system/files/usenixsecurity26-li-zhichao.pdf)

#### Attacks on Approximate Caches in Text-to-Image Diffusion Models
- **作者**: Desen Sun
- **方向**: AI/ML安全
- **解读**: 文生图扩散模型计算昂贵，近似缓存（如 Adobe 的 NIRVANA）复用相似提示的中间去噪状态可跳过最多 50% 步骤，但缓存被所有用户共享，打破了用户间隔离并扩大攻击面；此前攻击都针对模型本身或训练数据，服务系统缓存的安全性无人研究。

方法上，作者复现 NIRVANA（CLIP 余弦相似度>0.65 判命中，每 10% 步骤一档缓存至 50%），部署 FLUX 与 SD3 及 DiffusionDB/Lexica 真实提示数据集，识别两个攻击原语：①命中/未命中延迟可区分（H100 上跳过 10% 步骤 FLUX 省 0.92s、SD3 省 0.48s）；②命中同一缓存提示的输出结构更相似（SSIM 更高）。据此构造三个远程攻击：异步隐信道（古词关键词+标记物，延迟分类器+内容检测两级判定）、CacheExposer 提示词窃取（探测提示聚类→嵌入预测→GPT-2 恢复）、CachePoison（把攻击者 logo 嵌入窃取的提示词，命中用户的输出被污染）。

隐信道在 FLUX 上准确率 97.8%（仅延迟 94.6%），注入条目在缓存中存活超 44 小时（LCBFU 策略）；CacheExposer 窃取的提示词平均余弦相似度 0.75–0.81（基线 0.67），恢复图像 PSNR 最高 25.62，分类器距理想仅差 3.01%，但单次恢复中位数需 4385 次探测；CachePoison 无需修改模型或训练数据即可让命中请求渲染出 prompt 中没有的 logo。

我的思考：首个系统评估扩散模型近似缓存安全性的工作，展示了“性能优化组件沦为跨用户信息通道”的新攻击面，隐信道异步、可持续数天，隐蔽性显著高于同步信道，且攻击均由普通用户权限远程完成。局限：CacheExposer 探测开销大、依赖服务商部署近似缓存与固定相似度阈值、CachePoison 需先完成提示词窃取。防御可考虑缓存访问审计、相似度阈值加噪或按租户隔离缓存，对采用语义缓存的生成服务是及时警告。

- **论文 PDF**: [Attacks on Approximate Caches in Text-to-Image Diffusion Models](https://www.usenix.org/system/files/usenixsecurity26-sun-desen.pdf)

#### Identifying Provenance of Generative Text-to-Image Models
- **作者**: Anna Yoo Jeong Ha
- **方向**: AI/ML安全
- **解读**: 微调可用极低成本"克隆"出与从零训练难以区分的文生图模型，被厂商宣传为"新基座模型"，既抑制竞争又掩盖训练数据伦理问题；现有水印需训练时嵌入（对已发布模型不可行），分类器指纹方法不适用于高维随机输出的扩散模型，而现实约束是只能黑盒查询 API。

核心观察是：从零训练构建的高维特征空间是个性化"指纹"，微调只做局部调整、保留大部分特征几何。据此设计：用详细自然提示词（GPT-4 从高频物体名扩写）查询目标模型与基座池各 k=100 次，S2-CLIP 提取视觉特征、Isomap 降到 d=10 维，计算 Jensen-Shannon 散度距离；定理证明提示词条件化下输出分布距离等于输入分布距离，z 检验判断目标是否在池外（NULL），再按最近距离归属父模型。

在 8 个基座模型（SD2.1、SDXL、FLUX、DeepFloyd、Kolors、PixArt-Σ、Hunyuan、Sana）与多种微调变体上，微调模型到父模型距离 <0.5，到其他基座 ≥3.18，30 次重复 100% 准确、标准差为 0；对权重加噪（σ<0.016 再微调）、图像后处理等对抗条件仍稳健；5 个不同物体提示词结论一致；并对 6 个在线模型完成真实溯源。计算开销低（A100 上 100 张图 5–45 分钟）。

我的思考：首个黑盒生成模型溯源系统，理论直觉（两个定理）+统计检验落地清晰，监管场景（验证"伦理数据训练"声明）直接可用。局限：需要已知且去相关的基座池、k=100 查询成本高、对抗者若投入接近从零训练的微调预算仍可逃避、提示词池理论上可被针对。相比模型完整性验证类工作（只能判断"偏离"），本方法能可靠归属父模型，是实质性进步。
- **论文 PDF**: [Identifying Provenance of Generative Text-to-Image Models](https://www.usenix.org/system/files/usenixsecurity26-ha.pdf)

#### BADControl: Backdoor Attacks Against Control Systems
- **作者**: Luis Burbano, Hampei Sasahara, Ruoyu Song and Z. Berkay Celik, Alvaro A. Cardenas
- **方向**: AI/ML安全
- **解读**: PID 与 LQR 是汽车、航空、工业自动化中几乎无处不在的低层反馈控制器，其参数由运行数据调优而来，因此易受数据投毒；但现有后门攻击都面向 DL/RL（操纵高维输入或奖励函数），低层控制器既无准确率/奖励这类攻击目标，参数又少、改动易被发现。

BADControl 以闭环系统频率响应（H∞ 范数）为目标：离线阶段用投影梯度上升投毒运行数据，使系统在目标频率处的频率响应最大化，从而嵌入一个可由外部物理信号（如前车加减速机动、路面贴片）触发的后门，同时约束扰动小、只毒化部分数据保持隐蔽；在线用物理机动或贴片激活后门。防御侧提出两种训练时方案：PID 的正则化与 LQR 的鲁棒优化——用新定理把无限投毒场景化为单个可解优化，约束所有扰动下的闭环 H∞ 范数以限制触发信号放大。

在自适应巡航控制（ACC）中攻击成功率 100%（无后门时无法制造碰撞），车道保持控制（LKC）中 62% 的测试使车辆侵入对向车道（无后门 0%）；而 SOTA 自动驾驶 falsification 框架在 30 次试验中仅识别 1 次碰撞实例，凸显隐蔽性。模拟与物理实验覆盖 PID 与 LQR 两类控制器；在防御存在时，多次仿真中投毒攻击者无法再造成碰撞。

我的思考：开辟"低层控制器后门"全新攻击面，将控制理论（H∞）与对抗投毒结合自然——攻击目标不是性能指标而是系统动态特性，故正常工况表现如常、隐蔽性好。防御把无限约束化为有限优化，理论上有价值。局限：需数据投毒访问、物理触发可检测性讨论有限、场景限于 SAE L1/L2 的 ACC/LKC。对数据驱动调参提出安全审查警示。
- **论文 PDF**: [BADControl: Backdoor Attacks Against Control Systems](https://www.usenix.org/system/files/usenixsecurity26-burbano.pdf)

#### CompLeak: Deep Learning Model Compression Exacerbates Privacy Leakage
- **作者**: Na Li, Yansong Gao, Hongsheng Hu, Boyu Kuang, Anmin Fu
- **方向**: AI/ML安全
- **解读**: 剪枝、量化、权重聚类等模型压缩技术在移动与 IoT 部署中广泛使用，用户可付费访问不同压缩版本，但压缩对训练数据隐私的影响此前被忽视——尤其当多个压缩版本同时发布时。既有单模型成员推断攻击（MIA）视角无法回答：压缩是否以及多大程度放大隐私泄露？

CompLeak 提出首个系统评估框架，含三个变体：CompLeakNR 直接对每个压缩模型独立实施现有 MIA；CompLeakSR 将原模型与一个压缩模型配对，拼接二者后验构造元数据训练元分类器；CompLeakMR 利用多个压缩版本，拼接各版本损失序列与各 CompLeakSR 元分类器的后验作为特征训练 MLP 元分类器。核心洞察是不同压缩操作对成员与非成员的影响方式不同（成员损失随稀疏度上升，非成员波动），聚合多个来源的泄漏可放大整体风险。

在 VGG16/Mini-ImageNet 上，原模型 MIA 准确率 56.7%、90% 剪枝模型降至 52.9%，而 CompLeakSR 达 79.9%（+27%）；CompLeakMR 在三种压缩操作共 8 个模型下，TPR@0.1%FPR 达 95.7%、平衡准确率 98.8%、AUC 99.9%（有原模型），无原模型时 AUC 仍达 80.6%。80% 与 90% 剪枝模型的推断结果约有 22% 分歧，印证"不同版本泄漏方式不同"。即使部署 DP-SGD（σ=0.5），CompLeakSR 平衡准确率仍保持 71.6% 的攻击优势。

我的思考：该工作首次从"多版本聚合"视角证实压缩加剧隐私泄露，推翻了"高压缩降低脆弱性"的单模型直觉，对发布多压缩版本的服务商具有直接的合规与审计含义。六种架构（ResNet 到 BERT/GPT-2）与五个数据集增强了结论可信度。局限在于仅覆盖分类任务，CompLeakMR 需训练多个影子模型与元分类器，攻击成本较高；KNN-Shapley 显示新暴露的成员样本价值更高，值得后续针对压缩的差分隐私微调等防御深入研究。

- **论文 PDF**: [CompLeak: Deep Learning Model Compression Exacerbates Privacy Leakage](https://www.usenix.org/system/files/usenixsecurity26-li-na.pdf)

#### VidLeaks: Membership Inference Attacks Against Text-to-Video Models
- **作者**: Li Wang, Wenyu Chen, Ning Yu, Zheng Li, Shanqing Guo
- **方向**: AI/ML安全
- **解读**: 文生视频（T2V）模型在海量网络视频上训练，Sora 训练数据争议、Open-Sora 数据集遭 DMCA 下架等事件凸显版权与隐私风险；成员推断攻击（MIA）是审计此类风险的原则性工具，但面向图像/文本的现有方法无法应对视频的两大特性：关键帧记忆信号的稀疏性（模型只记忆少数关键锚点，帧级平均会淹没信号）与随机时域动力学引入的像素级噪声。

VidLeaks 提出两个互补信号：稀疏重建保真度（SRF）——对目标视频抽取关键帧，将生成帧与关键帧的 CLIP 相似度做 Top-K 平均，作为放大稀疏记忆的匹配滤波器；时间生成稳定性（TGS）——多次查询生成视频，测量场景级语义逐维方差，探测时域记忆。攻击按能力递减分三种黑盒场景：监督式、参考式（仅非成员参考集做异常检测）、纯查询式（无标定，预定义权重融合）。

在 AnimateDiff、Mira、InstructVideo 上，纯查询式攻击 AUC 分别达 82.92%、77.66% 与 97.01%，参考式 88.68%/85.34%/98.17%；参考集仅 3 个非成员视频即可达 88.66% AUC，20 个样本即稳定。消融显示 Top-K 机制不可或缺，TGS 逐维设计优于均值后求标准差（82.63% vs 80.86%），而光流抖动（60.33%）与主体一致性（63.17%）等替代信号在查询式下基本失效。

我的思考：这是首个针对 T2V 的系统性 MIA 研究，SRF/TGS 分别对应稀疏注意力与时间建模两类架构特性，信号设计与模型机理高度契合；查询式攻击可行且参考集数据效率高，意味着真实攻击门槛很低，97.01% 的 AUC 表明 InstructVideo 这类微调模型记忆泄漏尤为严重。局限在于仅选用数据溯源透明的开源模型，代理字幕模型引入额外依赖，多次查询在成本敏感场景受限。该工作为 T2V 版权审计与后续防御提供了基准与动机。

- **论文 PDF**: [VidLeaks: Membership Inference Attacks Against Text-to-Video Models](https://www.usenix.org/system/files/usenixsecurity26-wang-li.pdf)

#### Fend for Yourself! Backdoor Purification in Federated Graph Learning with an Evolving Knowledge Anchor
- **作者**: Chengcheng Zhu and Yunlong Mao, Jiale Zhang and Bosen Rao, Sheng Zhong
- **方向**: AI/ML安全
- **解读**: 
联邦图学习（FedGL）在分散图数据上协作训练全局模型，分布式特性使其易受后门攻击；而图数据的复杂拓扑与非独立同分布使良性梯度天然多样，模糊了恶意偏差与良性异构的界限。现有防御在复杂图数据上失效，且依赖可信服务器，服务器核验需查看客户端更新，直接与安全多方计算、差分隐私等隐私技术的目标冲突。

为此提出 GBHINDER，首个无可信服务器的客户端自防御框架：每个良性参与者用自己的可信历史知识作良性锚，净化下载的全局模型，再有选择地吸收全局模型中的良性知识使锚逐步演化，形成"净化—演化"良性循环。核心组件一是历史通道注意力正则化，用锚约束全局模型深层表征并配合拓扑一致性损失，破坏后门传播；二是自适应动量信息更新，基于扰动稳定性度量动态调整动量系数，让锚在协作与自保间安全演进。

在 NCI1、PROTEINS、DD、AIDS 四个图分类基准上对比四种攻击（Rand-/Motif-/Opt-GDBA、NI-GDBA）与五种防御（FoolsGold、Flame、RLR、GNNCert、FedTGE）：默认 20% 恶意客户端、3 节点触发器、50% 注入率下，GBHINDER 把 ASR 全部压到 10% 以下（如 AIDS 上 Opt-GDBA 的 ASR 从无防御的 84.62% 降至 2.35%），同时主任务精度几乎无损（如 NCI1 保持约 79%–80%）。

我的思考："每个客户端自己防御自己"的范式同时绕开服务器性能瓶颈与隐私架构冲突，理论上可与 SMC/DP 直接叠加，是后门防御思路的实质转变。局限是它本质是启发式净化而非可证明保证（对比 GNNCert），且依赖本地数据质量与历史模型的可靠性，客户端需额外存储与计算；对高恶意占比、极端异构场景的鲁棒性仍需验证。

- **论文 PDF**: [Fend for Yourself! Backdoor Purification in Federated Graph Learning with an Evolving Knowledge Anchor](https://www.usenix.org/system/files/usenixsecurity26-zhu-chengcheng.pdf)

#### Sy-FAR: Symmetry-based Fair Adversarial Robustness
- **作者**: Haneen Najjar
- **方向**: AI/ML安全
- **解读**: 人脸识别等安全关键ML系统易受对抗样本攻击（包括可物理实现的对抗眼镜），对抗训练虽提升鲁棒性却带来不公平：某些类别（如特定个体）明显比其他类别更易被攻破；已有公平鲁棒性方法（FAAL、SpecNorm等）追求类别间完美对等，但评估仅在汽车、船舶等低安全场景，且完美对等在真实任务中往往不可达成——如人脸识别中兄弟姐妹高度相似、本就易混淆。

作者的洞察是：追求对称性——从类别i到j的攻击与从j到i同样成功——比完美对等更可达成，且理论上证明个体间对称蕴含任意子群划分间的对称；据此设计Sy-FAR，用可微损失在优化良性与鲁棒准确率的同时鼓励误分类对称性，可高效集成到神经网络训练。

在5个数据集、3种架构上，针对定向与非定向真实攻击（含对抗眼镜）评估：在含兄弟姐妹的人脸识别任务上，最鲁棒与最不鲁棒类别的准确率差距降低至少41%；还首次发现"不公平目标类鲁棒性"现象——Sy-FAR使最易被误选为输出的类别至少56%更不易被选中；且训练更快、运行更稳定，良性与鲁棒准确率不降反升。

我的思考：把公平目标从"完美对等"重构为"对称性"，理论与物理可实现攻击的实践结合，并识别出目标类不公平这一新维度，是公平鲁棒性研究的重要范式转向；局限在于对称性依赖类别对的数据量、人脸数据集规模与攻击类型覆盖有限。相比FAAL/SpecNorm在CIFAR上的评估，Sy-FAR首次把公平鲁棒性验证推进到真实安全关键场景，是更具可操作性的公平鲁棒性目标。

- **论文 PDF**: [Sy-FAR: Symmetry-based Fair Adversarial Robustness](https://www.usenix.org/system/files/usenixsecurity26-najjar.pdf)

#### TopFeaRe: Locating Critical State of Adversarial Resilience for Graphs Regarding Topology-Feature Entanglement

- **作者**: Xinxin Fan, Wenxiong Chen, Quanliang Jing, Chi Lin, Shaoye Luo, Wenbo Song, Yunfeng Lu

- **方向**: AI/ML安全

- **解读**: 图对抗攻击（GAA）同时从拓扑/结构与节点特征两个视角扰动图数据，可让 GNN 在贷款风控等安全关键场景输出错误推断。现有防御分两类——预处理净化（GCN-SVD、GCN-Jaccard）与鲁棒表示学习（GAT、RGCN、HANG），但都无法回答为何拓扑与特征两方面必须协同、如何真正联合学习，且净化依赖预定义删除比例或相似度阈值，缺乏理论指引。

作者将图映射为复杂动态系统（CDS），用系统振荡建模对抗扰动的行为；把图拓扑与节点特征投影为两个特征空间，定义二维拓扑-特征纠缠扰动函数刻画攻击下的动态变化；再利用平衡点理论定位渐近稳定平衡点（ASEP）作为攻击韧性的"临界状态"，以 ASEP 及其参数 γr、γq 作为约束指导邻接矩阵净化（2-hop Jaccard 与余弦相似度加权、低相似度删边并在违背约束时回滚）。

在 Cora、Cora_ML、Citeseer、Amazon Photo、PubMed 五个数据集上对抗 Metattack、CE-PGD、DICE 与定向攻击 Nettack：Cora_ML 上 25% 扰动率时比次优基线分别高 16.41%、1.77%、3.26%；0% 扰动下亦有提升；与 GCN/GAT/HANG/MidGCN 结合后平均精度提升最高达 18.75%/14.86%/7.29%/2.26%；消融显示拓扑+特征联合较单独使用提升 1.33%–16.41%。

我的思考：把 CDS 平衡点理论引入图防御并给出 ASEP 存在性定理，理论视角有新意，且方法在干净图上也有收益、无需预知攻击信号。但"动态系统振荡"与工程净化之间存在较大的建模跳跃，Hurwitz、严格正实等定理条件在实际图上难以验证；基线主要限于预处理类方法，与 Pro-GNN 等联合学习工作的对比不足，尚需更大规模数据与自适应攻击检验成色。

- **论文 PDF**: [TopFeaRe: Locating Critical State of Adversarial Resilience for Graphs Regarding Topology-Feature Entanglement](https://www.usenix.org/system/files/usenixsecurity26-fan.pdf)

#### Cracking Federated Privacy: Initialization-Resilient Gradient Inversion with Fine-Grained Reconstruction

- **作者**: Kaiming Zhu

- **方向**: AI/ML安全

- **解读**: 联邦学习（FL）中共享梯度可被梯度反演攻击（GIA）重建客户端私有数据；现有攻击在早期初始化变化下不稳定（余弦距离法虽缓解了梯度范数收缩，但对初始化敏感），且只对齐方向导致重建模糊，层加权方案又需大量经验调参。

作者识别出共享梯度的稀疏性变化是初始化敏感性的主因，提出粗到细两阶段 GIA：粗阶段对齐梯度方向并约束非零项以缓解稀疏性变化（λ1 延迟调度、T≥18000 后激活），细阶段用余弦距离加变形曼哈顿项的混合度量精调幅度对齐（λ2 取梯度量倒数、TV 正则）。

ResNet-18、批大小 1、Kaiming-Normal 初始化下，CIFAR-10 PSNR 达 47.7dB（最好基线 iDLG 25.5dB）、CIFAR-100 达 45.7dB，较基线最高提升 200%；在 CelebA-HQ、ImageNet、LC25000 上同样全面领先 6 个基线；四种初始化（Normal/Uniform/Orthogonal/Transfer）平均下 CIFAR-10 仍达 41.4dB；对 DP 与梯度剪枝防御的不充分配置仍存在显著泄漏。

我的思考：把敏感性归因于梯度稀疏性变化是实质洞见，粗到细两阶段设计简洁有效，实验覆盖 5 个数据集与整个 FL 生命周期，威胁警示意义明确。局限：攻击者需掌握模型与初始化信息、两阶段各 3 万次迭代成本高，批大小增大后优势收窄；"防御不足"结论强调需更强防护（secure aggregation、足量 DP 噪声），但强防御下的表现未充分探索。

- **论文 PDF**: [Cracking Federated Privacy: Initialization-Resilient Gradient Inversion with Fine-Grained Reconstruction](https://www.usenix.org/system/files/usenixsecurity26-zhu-kaiming.pdf)

#### Attesting Model Lineage by Consisted Knowledge Evolution with Fine-Tuning Trajectory

- **作者**: Zhuoyi Shang, Yanwei Liu, Xiaoyan Gu, Weiping Wang
- **方向**: AI/ML安全
- **解读**: 深度学习微调产生了模型间的谱系关系，可用于应对开源权重库中未授权再分发、伪造模型出处等安全问题；但现有谱系检测主要依赖静态架构相似性，无法捕捉微调过程中知识的动态演化，易受对抗欺骗。

作者借鉴人类遗传机制，将谱系验证建立在"知识演化轨迹与参数修改轨迹一致"之上：先用模型编辑（任务算术）把子模型参数解耦为父模型参数与演化模型 fΔ=θ0+θP−θC，再针对分类器、扩散模型、LLM 分别用决策边界探测、分布采样、响应嵌入将知识向量化到共享潜空间，训练编码器 Ψ 与融合网络 Φ，以父知识 hP 与融合知识 Φ(hC,hΔ) 的余弦相似度判定谱系。

在 ResNet-18/MobileNet-V2 模型家族上，面对跨代攻击与剪枝攻击，该方法 TPR≈0.99、FPR≤0.02，远超 Neural Lineage/Neural Phylogeny（最差 TPR 0.42、FPR 0.21）与 KRM/IPGuard（TPR 仅 0.10–0.20）；KDE 显示父子对相似度峰值 0.83–0.88、祖孙 0.54–0.57、非谱系 0.00–0.20，可区分至少四代；对 DreamBooth/LoRA 微调的 Stable-Diffusion-v2 及 Hugging Face 上 Llama-3.1-8B、Qwen2.5-1.5B 微调族同样有效。

首次以"知识演化"而非静态相似性作为谱系的内在证据，定理 1 给出必要条件的理论支撑，机制可解释；但构造探针依赖父模型训练数据、需白盒访问双方模型，且 Ψ/Φ 需标注谱系对训练，跨代相似度随代数衰减，司法取证场景下的证据强度仍需检验。

- **论文 PDF**: [Attesting Model Lineage by Consisted Knowledge Evolution with Fine-Tuning Trajectory](https://www.usenix.org/system/files/usenixsecurity26-shang.pdf)

#### Adversarial Patch EXterminator: Zero-Shot and Patch-Agnostic Defense Framework Against Adversarial Patch Attacks

- **作者**: Jiayimei Wang, Tao Ni, Guowen Xu, Qingchuan Zhao and Cong Wang
- **方向**: AI/ML安全
- **解读**: 对抗补丁攻击威胁监控、自动驾驶、医疗等视觉系统；现有防御或依赖先验知识与大量训练数据，或对微小、不规则、与背景高度融合的补丁失效，物理环境下鲁棒性不足。

APEX 是零样本、补丁无关的三阶段框架：先用低阈值（τlow=0.1）检测提取包围盒，集中补丁信号避免稀释；再用互信息模糊热图与边缘感知边界热图（多尺度梯度异常）融合成双热图，与 SAM 分割掩码按覆盖率匹配精确定位补丁；最后用结构引导的 LaMa 修复图像，并扩张掩码保证视觉连贯。

在三个数据集、5 个检测器、8 种补丁攻击（非自然、自然、红外）上全面优于 LGS/SAC/Jedi/ObjectSeeker/PAD：AdvPatch 定位召回 82.42%（SAC 37.78%、Jedi 48.18%、PAD 68.94%），LAP 自然补丁 62.33%（SAC 仅 1.42%）；YOLOv3 上 QR 码攻击 mAP 从 47.89% 提升至 61.52%；物理世界把 ASR 降至 1.86%（走廊），不同光照下为 1.97%–3.98%；抽象画极端场景 ASR 从 71.52% 降至 17.65%。

零训练、无先验、跨补丁类型与物理环境的通用性是其最大贡献，定位-修复流水线可解释；但抽象画场景误报率仍达 27.52%，说明与补丁纹理相似的内容易误伤，且依赖 SAM/LaMa 等重模型，端侧资源受限场景的部署成本需权衡；自适应攻击的量化结果未在正文给出，鲁棒性上限待验证。

- **论文 PDF**: [Adversarial Patch EXterminator: Zero-Shot and Patch-Agnostic Defense Framework Against Adversarial Patch Attacks](https://www.usenix.org/system/files/usenixsecurity26-wang-jiayimei.pdf)

### 大模型安全（16 篇）

#### Membership Inference Attacks on Tokenizers of Large Language Models
- **作者**: Meng Tong; Yuntao Du; Kejiang Chen and Weiming Zhang; Ninghui Li
- **方向**: 大模型安全
- **解读**: 针对 LLM 的成员推断攻击（MIA）评估面临三重困难：需从头预训练 LLM（成本极高）导致评估模型与真实模型规模失配（Pythia-12B vs DeepSeek-R1-671B）、测试样本可能错标、数据分布偏移污染结论。与此同时，商业 LLM 为透明计费开源 tokenizer，其训练数据代表预训练语料，但从未被当作攻击面。

该文首次把 tokenizer 作为 MIA 攻击向量，提出五种攻击：Merge Similarity（影子 tokenizer 合并顺序的 Spearman 相关，效果差）、Vocabulary Overlap（Jaccard 重叠判别性 token，效果好）、Frequency Estimation（只训练一个影子 tokenizer，用幂律拟合估计每个 token 的 RTF-SI 信号并取最大值）、以及 Naive Bayes 与 Compression Rate 两种替代方案。威胁模型假设攻击者能下载开源 tokenizer 并从同分布采样辅助数据训练影子模型。

实验用 C4 语料 1,681,296 个网页（4,133 个网站），词表规模 8 万–20 万：Vocabulary Overlap 在 20 万词表上 AUC 0.771（TPR@1%FPR 达 34.61%），Frequency Estimation AUC 0.740；用 bag-of-words 分类器（AUC 0.513）验证无分布偏移。缩放定律显示词表越大攻击越强；OpenAI-o200k、DeepSeek-R1 等商业 tokenizer 中确实存在判别性 token（如源自 Reddit 用户的 'davidjl'）。删除低频 token 的防御会降低压缩效率，且对大数据集仍可攻。

我的思考：攻击面选择很巧妙——tokenizer 可廉价训练、语料代表性强、开源可得，绕开了 LLM MIA 评估的三大难题，直接服务"训练数据是否被滥用"的合规核查（呼应 Reddit 起诉 Anthropic 场景）。但这是数据集级推断，依赖同分布辅助数据假设，AUC 0.77 说明信号真实但非压倒性。防御空间有限提示压缩效率与隐私存在固有权衡，未来或需隐私感知的分词训练。

- **论文 PDF**: [Membership Inference Attacks on Tokenizers of Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-tong.pdf)

#### JailbreakScope: Interpreting Jailbreak Mechanism through Representation and Circuit Analyses
- **作者**: Zeqing He
- **方向**: 大模型安全
- **解读**: 越狱攻击能绕过 LLM 安全对齐（如 2025 年 1 月 ChatGPT 越狱被用于获取爆炸物制作信息），但其内在机制仍不清楚。现有工作要么只做静态表征分析（越狱如何扭曲危害性感知），要么只定位安全相关组件（神经元、注意力头、层），未覆盖多样化越狱类型、未将两个视角联合、且缺乏跨生成过程的动态分析。

JailbreakScope 是双视角解释框架：表征层用三类探针（线性、聚类、PCA）量化"越狱如何坍缩有害/安全表征的可分性"，并用 LogitLens 把各层表示解码到词表空间；电路层用 logit 差归因把激活归因到"肯定/拒绝"token 集，定位安全关键组件；最后对表征欺骗度与电路激活偏移做 Pearson 相关并逐 token 追踪二者协同演化。评估覆盖 5 个 LLM（Llama2-7b/13b、Llama3-8b、Vicuna1.5-7b/13b）× 7 种越狱（GCG、ReNellm、PAIR、DeepInception、Autodan、GPTFuzz、CodeChameleon）。

主要发现：定位 Llama2-7b 的 L21H14 为拒绝信号头、L26H04 为肯定信号头、L22 MLP 为安全相关层；所有越狱都抑制拒绝信号并增强肯定信号（如 DeepInception 把 S+ 激活从 −0.0241 提到 1.0003）；表征欺骗与电路激活偏移的 Pearson 相关为 0.79/0.77/0.67/0.64/0.72，跨规模与架构稳定；规则/演示类越狱（ReNellm、DeepInception）欺骗性最强；"先拒绝后应答"的提示在同等电路偏移下表征欺骗显著更低；模型规模放大与指令微调都不改善核心拒绝电路。

我的思考：联合表征-电路并做逐 token 动态追踪是该文最大增量，9 条观察质量高且可复现。"拒绝电路与规模和微调无关"解释了越狱鲁棒性难以通过缩放提升。局限：探针给出的是相关性而非因果证据，7 种越狱仍非穷尽。价值在于为防御（如针对性强化肯定/拒绝信号组件）与在线检测提供可操作的机制信号，是解释类工作中少见的工程可落地方向。

- **论文 PDF**: [JailbreakScope: Interpreting Jailbreak Mechanism through Representation and Circuit Analyses](https://www.usenix.org/system/files/usenixsecurity26-he-zeqing.pdf)

#### When AIOps Become "AI Oops": Subverting LLM-driven IT Operations via Telemetry Manipulation
- **作者**: Dario Pasquini, Evgenios M. Kornaropoulos and Giuseppe Ateniese, Omer Akgul
- **方向**: 大模型安全
- **解读**: AIOps 用基于 LLM 的自主智能体解读遥测数据并自动执行故障修复，但现有研究默认遥测数据可信。本文首次系统分析 AIOps 的安全性，证明低权限外部攻击者无需目标系统先验知识，即可通过操纵遥测数据诱导智能体执行破坏基础设施完整性的"恶意修复"。
攻击框架 AIOpsDoom 由爬虫（枚举可达端点）与模糊器（篡改请求字段并访问不存在路径以触发错误日志）组成，通过错误诱导请求将载荷注入日志、指标或追踪；核心创新是"对抗奖励劫持"载荷——由 lead（貌似合理的事故解释）与 body（攻击者选定的修复建议）构成，如"404 由 nginx SSL 版本导致，请添加 ppa:ngx/latest 并升级"，令智能体在"正常完成任务"的信念下自主选择有害操作，与经典提示注入语义截然不同。
在 AIOpsLab 的 3 应用 × 2 智能体（ReAct、Flash）× 2 模型（GPT-4o、GPT-4.1）× 3 修复目标的 180 次试验中，攻击平均成功率 90%，最难的 HotelReservation 仍达 82%（其遥测仅含 URL 追踪致载荷被 URL 编码破坏）；对照实验中 10 种经典提示注入载荷全部失败（53.6% 被识别为恶意、42.9% 被忽略）；攻击可绕过 PromptShields、PromptGuard-2 等防御，防御方案 AIOpsShield 则在不损害智能体性能的前提下阻止攻击。
本文揭示了"自动化程度越高、攻击面越隐蔽"的悖论，把奖励劫持从概念落地为工程化攻击，并首次将 AIOps 定义为系统沦陷的新攻击向量。值得注意的是智能体会主动补充系统细节（如服务器版本）为伪造根因"背书"，增加人审被蒙蔽风险。局限在于评估基于模拟环境、轮次受限，AIOpsShield 依赖遥测的结构性假设，对自适应攻击的效果仍待验证。

- **论文 PDF**: [When AIOps Become "AI Oops": Subverting LLM-driven IT Operations via Telemetry Manipulation](https://www.usenix.org/system/files/usenixsecurity26-pasquini.pdf)

#### Assessing LLM Response Quality in the Context of Technology-Facilitated Abuse
- **作者**: Vijay Prakash, Majed Almansoori, Donghan Hu, Rahul Chatterjee, Danny Yuxing Huang
- **方向**: 大模型安全
- **解读**:
技术助长虐待（TFA）是亲密伴侣暴力的常见形式，超 85% 的美国家暴援助从业者接触过相关案例；幸存者转向在线资源，而 LLM（含被家暴热线推荐的专用模型 Ruth、Aimee）正被集成进搜索与论坛。错误建议可能使幸存者暴露于更大危险，但缺乏对其回答质量的评估。

首次专家主导评估四个模型（GPT-4o、Claude 3.7、Ruth、Aimee）：从文献与 Reddit/Quora 构建 385 问语料，按 17 类虐待类型与 28 类手段分层抽样 193 问（139 个非对抗性）；以幸存者安全优先提示词生成零样本单轮回答，专家按准确性/完整性/安全性评分（Kappa 0.73-0.75），开放性编码归纳错误模式；并对 114 名有 TFA 经历的幸存者做可行动性用户研究。

86% 的回答被评为不完美；Aimee 88% 部分/最低完整、74% 不安全（均最高），Ruth 62% 部分/最低准确（最高），GPT 最低（44% 准确、54% 不安全）；专用模型并未优于通用模型。归纳 45 种错误模式：21 次建议 VPN、11 次推荐 RF 探测仪、5 次推荐 GPS 干扰器；遗漏"spyware 可能残留"（49 次）与"避免惊动施虐者"（88 次）等警告。用户调查中 Aimee 置信度最高（M=2.35）、Ruth 最低（1.98，p<0.005）。

填补 TFA 领域 LLM 评估空白，专家+幸存者双视角与方法严谨性突出。关键发现——专用模型并不更安全、四模型均可能协助施虐者——挑战"领域微调即安全"的直觉。局限：零样本单轮未覆盖真实多轮交互，结果对提示词敏感。为 IPV 组织与模型开发者提供了可操作改进方向。

- **论文 PDF**: [Assessing LLM Response Quality in the Context of Technology-Facilitated Abuse](https://www.usenix.org/system/files/usenixsecurity26-prakash.pdf)

#### MASLeak: Investigating and Exposing Intellectual Property Leakage Vulnerabilities in Multi-Agent Systems

- **作者**: Liwen Wang, Wenxuan Wang, Shuai Wang, Daoyuan Wu, Shing-Chi Cheung

- **方向**: 大模型安全

- **解读**: 多智能体系统（MAS）通过多个协作 LLM 智能体完成复杂任务，其系统提示、任务指令、工具配置与通信拓扑是开发者投入大量设计成本的智力资产，商业平台以黑盒方式托管以保护 IP。但既有研究聚焦用户数据保护，MAS 自身 IP 泄露尚属空白：单智能体提示提取方法无法沿拓扑传播。本文提出 MASLeak，首个黑盒下系统性提取 MAS IP 的框架。

方法上，受计算机蠕虫传播机制启发，MASLeak 精心构造对抗查询 q，使其同时满足三个目标：劫持目标智能体执行并诱导其泄露系统提示、任务指令与工具用法；携带已泄露信息沿拓扑传播到下一智能体；保持合法输出格式避免溢出截断。逐一渗透每个智能体，最终汇聚完整 IP 集合（数量、拓扑、提示、指令、工具），全程仅需访问公开 API。

作者构建首个 810 个 MAS 应用的合成数据集（30 种任务），并评估真实平台 Coze 与 CrewAI：提取系统提示与任务指令的平均成功率 87%，提取系统架构成功率 92%，较现有提示提取方法提升 60%。

我的思考：本文首次把提示提取从单智能体推广到多智能体拓扑，"蠕虫式传播"的类比精准抓住 MAS 信息逐级传递的结构性弱点——最小权限设计反而成为攻击的阶梯；87%/92% 的成功率说明黑盒 MAS 的 IP 保护形同虚设。局限：依赖固定 DAG 拓扑架构，对动态编排的 manager 式系统不适用；私有工具无法重建。与危害用户的注入攻击相比，MASLeak 揭示的是危害开发者/平台的 IP 泄露面。
- **论文 PDF**: [MASLeak: Investigating and Exposing Intellectual Property Leakage Vulnerabilities in Multi-Agent Systems](https://www.usenix.org/system/files/usenixsecurity26-wang-liwen.pdf)

#### A Distortion-minimization Watermarking Framework for Large Language Models: Larger Capacity, Stronger Robustness and Higher Quality
- **作者**: Liming Zhai
- **方向**: 大模型安全
- **解读**: LLM 水印需要同时满足大容量、强鲁棒性与高文本质量，但现有方法多为分离式设计：零比特水印保质量却无载荷，多比特方法要么脆弱易被改写攻击破坏、要么以牺牲质量为代价，三者难以兼顾。本文主张追求"无失真"水印在理论上不可行（引证隐写不可免费嵌入的理论），应转向务实的最小失真优化。

方法上，DMW 把鲁棒性与质量统一建模为失真代价：鲁棒性代价基于句子语义不变性（句子越靠近聚类质心越稳定、代价越低），两个质量代价分别基于困惑度（低连贯句优先改）与语义熵（高灵活性句优先改），加权合成总代价；语义投影把句子经 SBERT+k-means 映射为簇位串；再提出 PO-STC，把给定水印长度下的总失真最小化化为周期最短路径问题（Viterbi），实现流式逐段实时嵌入，空间复杂度从 O(2^h·n) 降至 O(2^h·v)。

在 C4/OpenGen、LLaMA-2-7B/Falcon-7B 上全面超越 Yoo、Wang、Qu 等基线：32-bit 载荷下 match rate 仅降约 8%；严重改写攻击下 match rate 最高比最优基线高 46.35%（8-bit 时比第二名高 30%）；中位困惑度最低甚至略低于人类文本，Sem-Ent 多样性接近或超过人类文本。

把图像隐写术的失真最小化范式系统引入 LLM 水印并统一权衡，且公开质疑"无失真"目标，成色清晰；局限是需额外 LLM（OPT-2.7B）算困惑度、嵌入效率仅列第二，语义投影依赖句子编码器与聚类质量，跨语言与短文本场景的泛化仍待检验。

- **论文 PDF**: [A Distortion-minimization Watermarking Framework for Large Language Models: Larger Capacity, Stronger Robustness and Higher Quality](https://www.usenix.org/system/files/usenixsecurity26-zhai.pdf)

#### Window-based Membership Inference Attacks Against Fine-tuned Large Language Models
- **作者**: Yuetian Chen, Ashish Kundu, Charles Fleming, Bruno Ribeiro and Ninghui Li
- **方向**: 大模型安全
- **解读**: 主流成员推断攻击（MIA）用全局平均损失做信号，但作者对 1,000 万 token 损失差的分析显示：成员信号是稀疏极值事件而非均匀分布偏移，且与领域适配 token 造成的长尾噪声混叠——成员/非成员均值仅差 0.393 vs 0.331，而约 1.77% 的 token 超过 3σ（高斯预期 0.3%），极端值可达到正常波动的 10–100 倍，全局平均被少数离群点支配。

作者用极值统计的点过程混合模型刻画 Δ=成员信号+领域噪声+基线噪声，理论推导出全局均值信噪比差，并证明在长尾污染下符号检验相对均值检验的 Pitman 效率（ARE）可达 2–5 倍、breakdown point 达 0.5。据此提出 WBC：滑动窗口（3–10 token 最优）对每窗口做目标/参考模型损失比较投二值票，再对几何间距的窗口尺寸（w_min=2, w_max=40, |W|=10）做均匀集成，无需调参。

11 个数据集、13 个基线上，WBC 平均 AUC 0.839 vs 最强基线 Ratio 0.754，TPR@1%FPR 提升 2.8 倍（5.2%→14.6%），Khan Academy 上 TPR@0.1%FPR 为 3.7 倍；消融显示参考模型比较（-32%）与符号聚合（-15.8%）最关键，符号聚合 AUC 0.835 vs 均值 0.728；模型越大越脆弱（160M 时 AUC≈0.51，2.8B 时 0.835）；长文本增益更大（512 token 时相对 Ratio 高 20.8%）。

我的思考：用极值统计给 MIA 建立了理论根基并形成"分析-建模-攻击-验证"闭环，反直觉发现（最强成员信号出现在微调模型损失更高的左尾 token）很有洞察。局限：需逐 token 损失（依赖 API 的 prompt_logprobs 或开放权重）、假定可获得参考模型、短文本（<64 token）效果弱。工作同时提示防御应针对局部记忆模式而非全局正则，对隐私审计有直接价值。
- **论文 PDF**: [Window-based Membership Inference Attacks Against Fine-tuned Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-chen-yuetian.pdf)

#### SoK: Attack and Defense Landscape of Agentic AI Systems
- **作者**: Juhee Kim, Wenbo Guo, Dawn Song
- **方向**: 大模型安全
- **解读**: LLM 与工具组合的 AI 智能体正快速进入真实应用，但其灵活性带来与传统软件、独立 LLM 都不同的安全挑战：提示注入已用于窃取 GitHub 私有仓库，RCE 漏洞、文档/邮件数据外泄、网络代理劫持用户银行账户等事件频发；已有综述聚焦 LLM 安全或单一攻击向量，缺乏系统框架。

作者从系统视角给出首个智能体安全 SoK：先以 7 个设计维度（输入信任、访问敏感度、工作流、动作、记忆、工具、用户界面）刻画智能体，再按三类对手（外部、用户级、内部）× 6 个攻击向量（间接/直接提示注入、恶意数据注入、工具投毒、模型投毒、记忆投毒）组织攻击面，归纳 7 类安全风险（异构不可信接口、错误指令遵循、不受约束数据流、幻觉与模型错误、隐私数据泄漏、非预期/未授权动作与数据破坏、资源耗尽与 DoS），并揭示风险级联放大机制。

防御按纵深防御分为运行时防护（输入/输出护栏、监控、人机交互验证）、安全设计（权限分离、信息流控制与污点追踪、凭据与身份管理）、组件加固（模型与工具加固）三类，并提出"情境安全（Contextual Security）"作为 CIA 之外的新安全目标。方法上对接 OWASP Top 10 与 MITRE ATLAS，综述 2023–2025.10 期间 128 篇论文（51 种攻击、60 种防御）。

我的思考：框架化贡献突出，"设计维度→风险→防御"映射与风险级联分析为社区提供共同语言，情境安全目标的提出贴合智能体"上下文即代码"的特性。局限：SoK 本质是文献分类而非实证验证，侧信道等方向作者自认空白，且智能体生态迭代极快、覆盖时效有限。作为地基性工作，对研究选题与工程落地有直接指导价值。
- **论文 PDF**: [SoK: Attack and Defense Landscape of Agentic AI Systems](https://www.usenix.org/system/files/usenixsecurity26-kim-juhee-agentic.pdf)

#### Lethe: Purifying Backdoored Large Language Models with Knowledge Dilution
- **作者**: Chen Chen, Yuchen Sun, Jiaxin Gao, Xueluan Gong, Qian Wang, Ziyao Liu, Yongsen Zheng, Kwok-Yan Lam
- **方向**: 大模型安全
- **解读**: LLM 后门防御面临两大困境：多数方法只检测不净化，或仅适用于单一触发器与单一领域；而微调类净化方法在模型编辑型（BadEdit/ROME/MEMIT）、多触发器（CBA）和无触发器（DTBA）等高级攻击下效果不佳。外包微调场景中恶意服务商可在交付模型中植入后门，用户需要一种无需触发器先验知识、兼顾分类与生成域的通用净化方案。

LETHE 提出"知识稀释"双机制：内部稀释在参数层面实现——用不足 10% 的干净数据以 LoRA 训练一个干净模型，再通过模型合并（默认 SLERP）与带后门模型融合，中和参数记忆中的恶意捷径；外部稀释在提示层面实现——用 TextRank 提取关键词，从 WordNet 检索语义中性的解释作为证据拼接到提示中，分散模型对触发特征的注意力。

在 5 个 LLM（GPT2-XL、GPT-J、Llama、Llama-2、DeepSeek-R1）、4 个数据集、8 种攻击与 8 个防御基线的对比中，LETHE 将分类域所有攻击的 ASR 压至 7% 以下（BadEdit/ROME 在 SST-2 上低至 0%），对模型编辑型攻击平均 ASR 仅 2.03%（最优基线 BEEAR 为 9.09%），多触发器/无触发器攻击平均 6.09%（最优基线 30.37%）；CDA 损失控制在 3% 以内，高级攻击 ASR 最高降幅达 98%。消融显示 LoRA 与全参数微调效果几乎无差别（<1%），对 13B 等更大模型依然有效。

我的思考：将模型合并从能力组合技术重新定位为安全净化机制，是本文最有新意的贡献；外部稀释零训练成本、可独立部署。局限在于防御方仍需少量干净数据，默认合并策略与插值参数依赖经验选择，对自适应攻击的鲁棒性论述较简略。与 NAD、BEEAR 等微调型净化相比，LETHE 在高级攻击下的压倒性优势说明"稀释"比"重新拟合"更契合后门参数的内生结构，为该方向提供了新范式。

- **论文 PDF**: [Lethe: Purifying Backdoored Large Language Models with Knowledge Dilution](https://www.usenix.org/system/files/usenixsecurity26-chen-chen.pdf)

#### When Memory Becomes a Vulnerability: Towards Multi-turn Jailbreak Attacks against Text-to-Image Generation Systems
- **作者**: Shiqian Zhao, Jiayang Liu, and Institute of Science Tokyo, Yiming Li, Wenshu Fan, Xiaobao Wu and Xinfeng Li, Jie Zhang, Wei Dong and Tianwei Zhang, Luu Anh Tuan
- **方向**: 大模型安全
- **解读**: 
现代文生图（T2I）系统（如 DALL·E 3）为在多轮交互中忠实生成而引入记忆机制，但该机制的安全分析严重滞后。既有越狱攻击把恶意目标提示融合进单条对抗提示，要么被安全过滤器检出（欠净化），要么过滤器被绕过却生成不出目标图片（过净化）。本文揭示记忆机制会把恶意意图逐步累积，放大越狱风险。

方法上提出 Inception，首个显式利用记忆机制的多轮越狱攻击，含两个模块：Segmentation 借助 NLP 句法分析把提示连同恶意意图按句子结构分解为多条看似合规的子提示，保持语义不变；Recursion 对无法简单切分的危险子提示先展开再递归切分，直至全部绕过过滤器。为支撑研究还构建了开源仿真系统 VisionFlow，集成 BufferMem/SummaryMem/VSRMem 三种工业级记忆机制与七种安全过滤器。

实验覆盖 14 种安全机制、5 个不安全概念、6 个 SOTA 基线。在 OpenAI 安全过滤设置下 Inception 的 ASR 达 32.3%，超过最优基线 SneakyPrompt（12.3%）20.0 个百分点；迁移到真实平台 DALL·E 3、Imagen 上 ASR 分别约 48.0% 与 52.3%，对抗提示还具高可复用性。

我的思考：该工作把攻击面从"单轮提示优化"扩展到了"产品功能本身"——记忆机制的多轮累积效应是真实系统独有且此前无人系统利用的漏洞，VisionFlow 作为可复现仿真平台是有价值的社区基础设施。局限在于不安全概念数量有限，且实验表明两阶段过滤（输出检测）比纯文本过滤平均降 11.7% ASR，说明纵深防御仍有效。与 LLM 多轮越狱（Crescendo 等）靠叙事放松约束不同，T2I 的跨注意力语义叠加要求每轮子提示不产生语义漂移，这是其独特难点。

- **论文 PDF**: [When Memory Becomes a Vulnerability: Towards Multi-turn Jailbreak Attacks against Text-to-Image Generation Systems](https://www.usenix.org/system/files/usenixsecurity26-zhao-shiqian.pdf)

#### Sirens' Whisper: Inaudible Near-Ultrasonic Jailbreaks of Speech-Driven LLMs
- **作者**: Zijian Ling, Pingyi Hu, Man Zhou, Jun Feng and Songfeng Lu, Dongmei Zhang and Bin Benjamin Zhu
- **方向**: 大模型安全
- **解读**: 
语音驱动的 LLM（手机助手、智能家居、车载系统）经开放声学通道引入新攻击面：语音输入短暂且执行前无可见记录，攻击者可在用户毫无察觉时注入恶意指令。既有不可听攻击（DolphinAttack、NUIT 等）缺乏显式信道建模与补偿，对设备与环境的非线性特性敏感，无法可靠重建越狱所需的较长结构化提示；白盒对抗音频又假设完全模型访问，不符合黑盒 API 现实。

SWhisper 在商品硬件上把任意目标基带音频（含长结构化提示）编码为 17–22 kHz 近超声波形，经声学传输与麦克风非线性解调后忠实重建：通过对跨设备、跨环境的非线性信道特性建模并做轻量信道逆预补偿，构建高保真隐蔽信道；再配合 voice-aware 越狱提示生成，兼顾可懂性、简短性与跨模型可迁移性。

商业模型上最高达 NR 0.94、SC 0.925（如 GLM-4-Air 黑盒 0.940/0.925，Grok-4 0.780/0.740）；端到端音频模型 GLM-4-Voice 上 NR 0.980、SC 0.813，超过最强基线 Auto-DAN 达 0.240/0.270；受控用户研究表明注入音频与背景音播放知觉上不可区分；多种噪声/语音环境（50–60dB 办公室、公园等）下 WER 不超 0.043，NR/SC 稳定（如 50dB 办公室 NR 0.780、SC 0.713）。

我的思考：把"不可听声学注入"与"可迁移越狱提示生成"打通，并在黑盒商品设备上取得接近文本级越狱的效果，威胁现实且可落地；信道建模与预补偿相对 NUIT 等前作是实质进步，且该隐蔽信道可泛化到任意提示注入与命令执行，不止越狱。局限是仅覆盖 17–22 kHz 频段，更高采样率硬件或抗非线性检测技术可能削弱；NR/SC 属替代指标，真实场景（如驾驶中）的实际危害尚需评估。
- **论文 PDF**: [Sirens' Whisper: Inaudible Near-Ultrasonic Jailbreaks of Speech-Driven LLMs](https://www.usenix.org/system/files/usenixsecurity26-ling.pdf)

#### Autonomy Comes with Costs: Detecting Denial-of-Service Vulnerabilities Caused by Resource Abusing in LLM-based Agents
- **作者**: Jiaqi Luo; Xudong Pan; Yuan Zhang
- **方向**: 大模型安全
- **解读**: LLM智能体可依据自然语言自主执行下载文件、总结内容等复杂任务，但缺乏全面的资源治理，单个恶意提示即可耗尽资源造成拒绝服务（DoS）；现有模糊测试无法生成既激活特定功能又诱发重度资源消耗的语义级提示，AgentFuzz等虽有尝试但局限于taint型漏洞，且未考虑资源生命周期导致的跨轮累积型漏报。

方法上，这是首个对LLM智能体资源管理的系统性安全研究：识别出三种资源生命周期模式——单任务内释放、跨多轮会话保留（会话结束才清）、以及永不释放；据此设计定向灰盒模糊框架AgentDoS，静态分析定位资源消耗型sink并推断其生命周期（识别准确率95.2%），再用LLM生成功能特定、驱动过度资源消耗的自然语言种子提示，并按生命周期感知策略（新会话或同会话内）调度与变异。

在20个广泛使用的开源智能体上，AgentDoS发现36个零日DoS漏洞、影响16个智能体（其中15个GitHub星标超1万），至今已获15个CVE编号；精确率100%、召回率94.7%，远优于AgentFuzz的3个真阳、7.9%召回；生命周期统计中短命、长命、全生命周期资源分别占77.3%、14.1%、8.6%；全程消耗119.08 CPU小时，平均每智能体5.95小时。

我的思考：首次把"资源生命周期"引入智能体DoS检测，覆盖了单轮耗尽之外的跨轮累积与永不释放两类真实风险，是AgentFuzz之后该方向的实质推进；局限在于依赖Python静态分析（约5%误差）、需LLM参与生成与变异（占68.6%运行时间），且仅覆盖20个智能体。对开源智能体平台而言，15个CVE的落地成果也提示：资源治理应成为智能体平台的强制安全基线，而不只是功能设计细节。

- **论文 PDF**: [Autonomy Comes with Costs: Detecting Denial-of-Service Vulnerabilities Caused by Resource Abusing in LLM-based Agents](https://www.usenix.org/system/files/usenixsecurity26-luo.pdf)

#### Overcoming the Retrieval Barrier: Indirect Prompt Injection in the Wild for LLM Systems
- **作者**: Hongyan Chang
- **方向**: 大模型安全
- **解读**: 间接提示注入（IPI）被视为LLM应用顶级风险，但以往评估几乎都假设恶意文本已在模型上下文中（如把恶意内容放在"最新邮件"或单文档语料），回避了最困难的一步——确保恶意内容被真实检索到；本文在11个BEIR基准上发现未优化的恶意文本在自然查询下从未被检索到，检索因此是IPI落地的最关键瓶颈。

作者把恶意内容分解为触发片段（保证检索）与攻击片段（编码攻击目标），提出仅需embedding API访问的黑盒算法构造紧凑触发片段；成本低至每目标查询0.21美元（OpenAI embedding），在11个基准、8种embedding模型上达到近100%检索率，而Query+（重复查询）等启发式提升甚微。

基于此，作者给出自然查询与真实外部语料下首个端到端IPI利用：RAG场景单一恶意文档即可让11种LLM在几乎所有数据集上持续输出目标答案（ASR接近1）；智能体场景中，单封投毒邮件可诱使GPT-4o在多智能体工作流（Magentic-One）中窃取SSH密钥，成功率超80%，并可实现钓鱼蠕虫传播、工具滥用与代码执行；对多种防御的评估显示它们均不足以阻止恶意文本被检索（基础防御下仍达60% Recall@5）。

我的思考：这项工作把IPI从"假设在上下文中"的实验室设定推进到"自然查询下真实可触发"，检索环节被证实是普遍且开放的薄弱点，威胁评估可信度高；局限在于触发片段需针对目标查询预计算、攻击成本随embedding服务定价变化，且检索侧加固的系统研究仍属空白。与Query+等启发式相比，其黑盒优化在效率与泛化上均显著领先，为防御方指明"检索层"这一亟需加固的战线。

- **论文 PDF**: [Overcoming the Retrieval Barrier: Indirect Prompt Injection in the Wild for LLM Systems](https://www.usenix.org/system/files/usenixsecurity26-chang-hongyan.pdf)

#### From Texts to Rules: Generating Sigma Rules with Large Language Models from Cyber Threat Reports

- **作者**: Yongxin Cai, Jing Qiu, Qingming Li, Du Cheng, Lei Chen

- **方向**: 大模型安全

- **解读**: 网络威胁报告（CTR）蕴含可行动的检测情报，但把高层自然语言描述翻译为机器可执行的检测规则存在语义鸿沟与领域约束。作者先论证规则类型适配性：分析 1239 条开源 Yara 规则，仅 2 条能从对应报告还原全部检测细节；Snort 依赖易变的网络 IoC；而数据集内超过 94% 的 CTR 包含完整 shell 命令与上下文描述，使 Sigma 成为最合适的目标格式。

方法上提出 SIGMERGE，首个端到端 CTR-to-Sigma 框架，按"语义逐级下降"组织三个模块：高层信息抽取模块用多子序列算法建模攻击元素（Actor/Tool/Action）的组合语义并微调领域 LLM，实现 TTPs 与 shell 命令的精确抽取；中层攻击描述生成模块引入 CWE、CAPEC、CoA、Mitigation 四类外部安全知识，结合 DPO 偏好优化与闭环自校验弥合语义鸿沟；机器层 Sigma 规则生成模块用参数优化的检索增强（RAG）把领域专家知识注入轻量 LLM，保证规则语法与字段合规。

实验构建 7 个数据集，用 23 个指标对 16 个基线与 13 个 LLM 评测，并做 10 个真实安全系统案例。TTPs 抽取在 tactic/technique 上相对最优基线 F1 提升 75.50%/86.52%；命令抽取 F1 最高 99.52% 且推理时间普遍快 2 倍以上；规则完整性较 DeepSeek-V3 提升 33.17% 并获 2.85× 加速；已有 4 条新 Sigma 规则被官方仓库正式接受。

我的思考：这是首个系统研究 CTR-to-Sigma 适配性并给出该任务首个评测基准的工作，"语义中间层"架桥的设计思想值得借鉴，工程完成度与实证规模俱佳。局限在于输入依赖含 shell 命令的 CTR，对不落命令的攻击行为覆盖有限；评测以自动指标为主，真实世界的规则质量仍需运营反馈。与 Raconteur 等对比优势明显，但对安全运营而言，规则的可维护性与误报率仍是落地关键。

- **论文 PDF**: [From Texts to Rules: Generating Sigma Rules with Large Language Models from Cyber Threat Reports](https://www.usenix.org/system/files/usenixsecurity26-cai.pdf)

#### GateBreaker: Gate-Guided Attacks on Mixture-of-Expert LLMs

- **作者**: Lichao Wu, Stjepan Picek, Ahmad-Reza Sadeghi

- **方向**: 大模型安全

- **解读**: MoE 架构（GPT-4、DeepSeek-MoE、Qwen-MoE、Pangu-MoE 等）通过稀疏路由激活少量专家，成为大模型扩展的主流；但现有 LLM 安全研究几乎只针对稠密架构。MoE 的输入相关稀疏激活暗示安全对齐可能集中在少数专家上，存在独特且未被研究的脆弱性，而既有工作仅停留在专家粒度、多针对旧式稀疏专家。

方法上提出 GateBreaker，首个免训练、轻量、架构无关的推理时攻击框架，覆盖 sparse、mixture、grouped mixture 三种 MoE 变体。三阶段流水线：gate 级画像——识别有害输入被异常高频路由到的安全专家；专家级定位——在安全专家内进一步定位安全结构神经元（gate 投影层贡献大于 up 投影层）；定向安全移除——按画像与定位结果剪除少量神经元，破坏安全对齐。攻击为白盒、纯推理时执行，适合自托管恶意部署或供应链场景。

在 8 个最新对齐 MoE LLM（GPT-OSS-20B、Qwen3-30B-A3B、Mixtral、DeepSeek-MoE、Hunyuan、OpenPangu 等，StrongREJECT 数据集）上，ASR 从基线 7.4% 升至 64.9%，每层仅平均剪除 2.6%（最多 2.9%）神经元，50% 层数剪枝即可达 40.8%；同族模型 one-shot 迁移 ASR 由 17.9% 升至 66.1%（单项最高 91.2%）；5 个 MoE VLM（DeepSeek-VL2、Kimi-VL 系列）上 ASR 从 20.8% 升至 60.9%；远超唯一同类方法 SAFEx（29.9% vs 64.9%），且五个 NLU 基准上效用几乎不降（如 ARC 从 59.4% 降至 52.8%）。

我的思考：首次在神经元粒度揭示 MoE 安全对齐"集中于稀疏路由协调的小部分神经元"，攻击极其轻量且可跨模型迁移，说明 MoE 的安全防线比稠密模型更脆弱、更易被定点拆除。局限在于白盒与内部状态访问假设、单轮攻击设定、跨家族迁移未知。对防御方的启示明确：MoE 对齐需要路由感知的防御（安全专家隔离、对抗性对齐训练、路由扰动检测），也警示下游微调（如代码微调）可能天然侵蚀安全神经元的激活。

- **论文 PDF**: [GateBreaker: Gate-Guided Attacks on Mixture-of-Expert LLMs](https://www.usenix.org/system/files/usenixsecurity26-wu-lichao.pdf)

#### Quantifying Large Language Model Attacks Through the Lens of Model Cognition

- **作者**: Xiuming Liu, Bin Benjamin Zhu, Hongsheng Hu
- **方向**: 大模型安全
- **解读**: LLM 易被恶意输入诱导输出有害内容，而关键词过滤、输出审核等机制忽略模型内部动态，SFT/RLHF/DPO 对齐在自适应越狱下鲁棒性不足；现有内部检测器（HiddenDetect、JBShield）多依赖单一层或机制，易被逐层规避。

作者发现：与有害提示相关的安全特征在中间隐藏层中经轻量探针即可高度分离（生成前准确率最高 99%），即使模型输出合规时这些特征仍存留——越狱常源于"识别-执行鸿沟"而非安全信号缺失。据此训练逐层毒性探针（双层 MLP，取末 token 隐藏态），用余弦相似度刻画扰动漂移并聚合为 Attack Consistency Index（ACI），再按层行为向量相似性去冗余贪心选层，融合成 <5M 参数的 Sentinel，复用隐藏状态、零额外推理、延迟约 50ms。

Sentinel 相比生成级拒绝把假阴性减半，自适应攻击下保持 >94% 检测准确率（基线掉 32%）；在 7 个开源 LLM（1.5B–72B）与 I2P、SneakyPrompt、MMA、Labelled、PIJ、ChatAlpaca、Multi-turn Jailbreak 上平均准确率 >0.90，优于 Llama-Guard-3-8B（<0.83）；PIJ 上 0.91 vs JBShield 约 0.72，多轮基准 97.0%；单层探针平均 13.1 步即被 GCG 式攻击击穿，多层 Sentinel 下攻击成功率最高仅 58%，加一致性约束后无法收敛；并首次给出安全信号随层出现、传播、衰减的定量图谱。

"识别-执行鸿沟"视角把防御从黑盒转向内部认知，兼具检测与诊断价值，多层融合对梯度型自适应攻击的鲁棒性来自层间梯度冲突；但需白盒访问目标模型隐藏态，对闭源 API 模型不可用，探针恢复的是相关特征而非因果特征（论文亦承认），更强攻击或定向层攻击仍是开放问题。
- **论文 PDF**: [Quantifying Large Language Model Attacks Through the Lens of Model Cognition](https://www.usenix.org/system/files/usenixsecurity26-liu-xiuming.pdf)

### 恶意软件与二进制分析（2 篇）

#### From Mirai to Gorilla: Deep Dive into a Long-Lasting DDoS-for-Hire Botnet
- **作者**: Maarten Weyns, Daniel Wagner, Georgios Smaragdakis and Harm Griffioen
- **方向**: 恶意软件与二进制分析
- **解读**: 2016 年 Mirai 源码泄露后催生了大量 DDoS-for-Hire（攻击租赁）平台，但多数基于 Mirai 的僵尸网络生命周期仅数周至数月；Gorilla 却从 2024 年秋运营至 2025 年夏被执法部门关停，寿命异常持久，运营前已发动超 30 万次攻击、波及 100 余国。本文对其设计、工程决策与商业策略进行深度逆向剖析。
方法上，作者综合多种数据源：134 个恶意样本逆向（ARMv7 未剥离）、5 台分布五大洲的 C2"挤奶机"持续轮询命令、80,000 IP 被动 + 2,000 IP 反应式望远镜、IXP/ISP netflow、Telegram 群监控与控制面板爬取，并对自有基础设施执行 12 次受控自攻击获取地面真值。
关键发现：Gorilla 经历两大软件阶段——第一阶段沿用 Mirai 蠕虫式 Telnet 扫描，第二阶段完全移除扫描、改用 29 个集中式 IP 定向传播（重点利用 ADB/TCP 5555 感染 Android 设备）；C2 协议从明文演化为 SHA-256 + 凯撒密码，再到第二阶段的自定义 Feistel 密码；客户端引入限速（3,000→1,500 pps，后经 C2 重复命令回到 3,000），"并发数"即重复命令数。测量期内共观察到 740,351 次攻击、146,213 个唯一目标：60% UDP/39% TCP，udp_plain 最常用（246,276 次）；目标以托管商（48%）与 ISP（46%）为主，OVHcloud 被攻击累计超 81 天。火力评估显示每并发最大约 13 Gbps，一次 3 并发攻击达 41 Gbps；自攻击仅观察到 220 台 bot（理论 7 Gbps），说明平台依赖集中式基础设施（DNS 放大 + 伪造源 IP 的 UDP，占攻击前置流量 35%）补充火力。订阅 12 档、价格 $40–$1,200。
该研究首次以端到端证据链刻画了现代"混合式"DDoS-for-Hire 平台：长寿源于两阶段工程迭代（为对抗检测而加密、为控制火力而限速），商业模式成熟（分级订阅、营销渠道、免费试用）。局限在于测量受 ISP 清洗与 netflow 采样影响，火力估计多为下界；bot 数量观察可能有选择偏差，对幕后运营者身份与资金链追踪未展开。
- **论文 PDF**: [From Mirai to Gorilla: Deep Dive into a Long-Lasting DDoS-for-Hire Botnet](https://www.usenix.org/system/files/usenixsecurity26-weyns.pdf)

#### Cutting the Gordian Knot: Detecting Malicious PyPI Packages via a Knowledge-Mining Framework
- **作者**: Wenbo Guo, Chengwei Liu, Ming Kang, Yiran Zhang and Jiahui Wu, Zhengzi Xu, Vinay Sachidananda and Yang Liu
- **方向**: 恶意软件与二进制分析
- **解读**:
PyPI 供应链攻击频发（2023 年 116 个恶意包下载超万次），现有检测工具误报率 15-30%，约三分之一合法包被误判，每周产生超 4,000 条人工告警。根因是句法规则无法区分相同 API 调用在合法/恶意上下文中的意图差异（如 gen_wrappers 与 gmgeoip 都用 urlencode+base64，前者被误报）。

提出知识驱动框架 PyGuard，把检测失败转化为行为知识。研究阶段：四个静态工具扫 18,137 包，收集 5,813 个误报上下文与 7,458 个恶意片段；知识提取：GPT-4.1 以 Card Sorting 生成行为摘要、专家合并为 327 个行为类别，抽象为动作序列（2,431 条），PrefixSpan 分层挖掘出 304 个模式（278 确定性+26 可辩护，覆盖率 92.60%）；检测：双层知识库（模式+案例），先精确/子序列匹配，未决时 FAISS 检索 top-5 做 RAG 增强的 LLM 上下文推理。

评估集准确率 99.50%（F1 99.49%），仅 2 个误报（基线 1,927-2,117），最优基线 GuardDog 94.12%；新近包 97.37%、混淆代码 98.28%、NPM 跨生态 98.07%；真实部署发现 219 个新恶意包并经 PyPI 官方确认。消融：OSSGadget 50.01%→89.91%、Bandit4Mal 33.37%→70.53%；GPT-4.1-Mini+知识（99.51%）超过无知识大模型（混淆时 68.58%）。人工验证动作映射正确率 95.12%（Kappa 0.8778）。

把"降误报"从工程修补升级为知识工程：从误报/漏报中挖掘可判别行为序列并沉淀复用，304 个模式可解释（如五步 reverse shell 序列），跨生态迁移验证语义抽象价值。局限：LLM 提取有成本与幻觉风险（4.82% 幻觉动作）；排除远程安装、非 Python 载荷、依赖混淆型攻击；模式库需随攻击演化重挖掘。工程价值突出。

- **论文 PDF**: [Cutting the Gordian Knot: Detecting Malicious PyPI Packages via a Knowledge-Mining Framework](https://www.usenix.org/system/files/usenixsecurity26-guo-wenbo.pdf)

### 漏洞挖掘与利用（4 篇）

#### Bond: Constraint-Directed Fuzzing for Automated Validation of Taint Analysis Results in Linux-based IoT Firmware
- **作者**: Jiaqian Peng; School of Cyber Security; Puzhuo Liu; Tsinghua University; Kai Cheng; Zhaoteng Yan; Jie Liu; Chengnian Sun; Hongsong Zhu; School of Cyber Security
- **方向**: 漏洞挖掘与利用
- **解读**: 静态污点分析是 IoT 固件漏洞检测的主流手段，但会产生大量误报：文中举例单个固件可报 169 条告警，每条人工验证需 5 小时以上，真漏洞易被淹没。现有黑盒/定向灰盒模糊测试依赖源码与执行反馈，符号执行受路径爆炸困扰，仿真重宿主保真度低，均难以自动化验证固件污点告警。

方法上，Bond 提出约束引导的黑盒模糊框架，仅需固件二进制。四个组件：入口点识别（CFG/CG 交替回溯定位嵌套分支或函数指针表等分发结构，并用前端关键字交叉验证）；可达区域划分（枚举入口到 sink 的基本块与用户可控参数）；约束分析（跨过程数据流分析，将约束分为强制/部分/非约束三类、六种语义类型——字符串匹配、数值范围、字节级、空值、网络格式、启发式语义）；定向黑盒模糊（GPT-4o 辅助从公开 PoC 生成合规 HTTP 模板，按强制→部分→非约束分层变异）。

实验覆盖 19 台设备（8 厂商）、2,776 条告警（SaTC/Mango/OctopusTaint/EmTaint），成功验证 1,349 条（48.6%），其中 155 个为未知漏洞（35 命令注入+120 缓冲区溢出），108 个已获 CVE/PSV 编号；60 个已知漏洞上召回率 91.67%、平均触发时间 5m53s；相比 GreenHouse/FIRM-AFL/SNIPUZZ/BooFuzz 最高提升 5.5 倍；消融实验验证各组件有效性。

我的思考：Bond 把"污点报告→PoC"这一长期依赖专家人肉的环节自动化，并以大量真实 CVE 证明价值，工程成色扎实。但依赖前端关键字与公开 PoC 等外部信息，285 条真阳仍被漏报，单告警限时 1 小时也影响深路径漏洞。与既有 IoT 模糊工具相比，约束语义建模+LLM 模板是其差异化创新，有望推动"污点分析→验证"工具链闭环，后续或可结合模拟执行进一步提升效率。

- **论文 PDF**: [Bond: Constraint-Directed Fuzzing for Automated Validation of Taint Analysis Results in Linux-based IoT Firmware](https://www.usenix.org/system/files/usenixsecurity26-peng-jiaqian.pdf)

#### Patch-Guided Vulnerability Detection: Extracting Java API Security Rules via Attack–Defense Cross-Analysis
- **作者**: Bofei Chen, Chibin Zhang and Mathias Payer, Yuan Zhang
- **方向**: 漏洞挖掘与利用
- **解读**: API 误用导致约 52% 的 Java 缺陷，安全敏感 API（SAPI）使用不当可引发远程代码执行等严重漏洞。现有规则生成方法依赖不完整的文档或"多数即正确"的源码推断，缺乏"什么是可利用误用、如何修复"的实证依据；且仅 23.2% 的补丁直接修改 SAPI 调用，其余通过跨过程流间接影响，使规则提取极为困难。

VulGenie 采用补丁驱动的三阶段流水线：阶段一构造修改行为图（MBG），以依赖边与语义等价边把含噪声补丁划分为各 SAPI 专属修复簇，并用 PageRank 定位被违反约束与核心防御；阶段二通过攻防价值流交叉验证识别未直接出现在补丁中的 SAPI（含跨库调用图按需补全），由 LLM 辅助合成含利用条件与参考防御的 API 安全规则（ASR）；阶段三以攻防交集引导路径敏感、偏差引导自适应上下文敏感的污点分析，兼顾精度与规模。

在 150 个近期 Java 安全补丁上提取出 198 条规则、精度 81.82%，其中 177 条是 CodeQL 知识库之外的规则；在 10 个流行 Java 应用中发现 46 个 0-day 漏洞（CodeQL 与 GraphiMuse 分别仅检出 18 与 3 个），已有 26 个被修复、获 10 个 CVE 编号。MBG 隔离平均移除每个补丁 126.8 行无关改动。

我的思考：把安全补丁当作"已验证的误用证据"是从文档/启发式到实证驱动的范式升级，攻防交叉验证有效解决了 SAPI 不直接入补丁的难题，跨项目泛化能力突出；但精度依赖 LLM 判断、规则合成受补丁质量影响，且仅覆盖 Java；与 APHP 相比，VulGenie 用依赖拓扑而非句法模式识别约束，抗噪能力更强，其"规则库自动扩充"思路也值得移植到其他语言生态。

- **论文 PDF**: [Patch-Guided Vulnerability Detection: Extracting Java API Security Rules via Attack–Defense Cross-Analysis](https://www.usenix.org/system/files/usenixsecurity26-chen-bofei.pdf)

#### CombiSan: Unifying Software Sanitizers for Comprehensive Fuzzing
- **作者**: Matteo Marini; Floris Gorter; Daniele Cono D'Elia; Cristiano Giuffrida
- **方向**: 漏洞挖掘与利用
- **解读**: C/C++漏洞检测高度依赖模糊测试与软件sanitizer，但最流行的ASan、MSan、UBSan互操作性差：MSan与ASan因元数据布局不兼容而无法同时启用，且MSan要求插桩全部依赖库，导致OSS-Fuzz仅约40%的项目测试未初始化内存（UUM）；开发者只能顺序多轮运行，且一个sanitizer提前终止会掩盖其他bug，整体测试非均匀且低效。

CombiSan提出统一sanitizer：1字节对2位的统一影子内存同时跟踪每字节的可寻址性（redzone）与初始化状态，用基于二进制的精确检测器过滤UUM误报，从而无需插桩系统库；ASan粒度的插桩保证与现成UBSan无缝集成；为避免bug互相掩盖，它抑制终止并延迟错误分析到测试用例结束，聚合全部问题后再统一处置。

在10个缺乏MSan支持的OSS-Fuzz目标上检测出81个此前未知的bug：38个UUM、17个可寻址性问题、13个其他未定义行为、13个自发崩溃；总体减速4.7x，比顺序运行ASan+UBSan与MSan的复合减速7.9x快1.7倍；在8个常见目标上可检测出与三者相同的bug集；UUM与可寻址性检测逻辑在SPEC CPU2006/2017上平均约150%开销，显著低于ASan与MSan之和。

我的思考：以统一元数据布局+延迟分析破解sanitizer组合难题，同时解决了互操作性、bug掩盖与MSan库插桩三大痛点，"同等检测能力、更少CPU时间"的结论对OSS-Fuzz类基础设施有直接价值；局限在于2-bit影子内存的粒度限制、UB覆盖类别仍取决于UBSan插桩、二进制定位器增加工程复杂度。与分别运行各sanitizer相比，单轮多类检测避免了测试输入在多种配置间的重复执行，长期累积的算力收益值得进一步量化。

- **论文 PDF**: [CombiSan: Unifying Software Sanitizers for Comprehensive Fuzzing](https://www.usenix.org/system/files/usenixsecurity26-marini.pdf)

#### FirmReBugger: A Benchmark Framework for Monolithic Firmware Fuzzers
- **作者**: Mathew Duong; Surya Nepal; Damith C. Ranasinghe
- **方向**: 漏洞挖掘与利用
- **解读**: 单体固件（无OS支撑、直接管理MMIO/DMA与中断的固件）广泛存在于医疗、汽车等嵌入式系统，其模糊测试评估长期依赖代码覆盖率与唯一崩溃数两个不可靠指标——例如模糊器可利用某bug触达本不可达的代码而虚增覆盖率，崩溃去重也需大量人工分析；领域缺乏可靠、可复现的基于bug的基准来公平评估新模糊器。

FirmReBugger提出以bug oracle（用C语法表达式描述bug特征）配合解释器自动化分析，在重放模糊测试种子时把虚拟CPU状态自动翻译为bug状态，严格区分detected、triggered、reached、not reached四种状态；关键设计是不修改目标二进制、仅重放种子，从而把基准实现与模糊器隔离、防止"泄漏性oracle"与对基准的过拟合，且易于扩展新bug oracle；据此构建FIRMBENCH，含61个多样化真实二进制目标与313个软件bug oracle、共187个bug。

作者以FuzzBench-for-Firmware式服务实现FirmReBugger，并开展复现性研究：9个最先进的单体固件模糊器、每个执行10次24小时重复运行，累计约10 CPU-年的评估努力，报告了各模糊器的bug发现能力对比。

我的思考：把"bug-based benchmark"方法论系统引入固件模糊领域，直接回应覆盖率指标的误导性，重放种子的设计精巧地隔离了模糊器与基准实现，降低过拟合风险并支持公平复现；局限在于bug oracle需人工以C表达式编写（可扩展但成本高）、固件与bug多样性仍有限、10 CPU-年评估代价不菲。与通用FuzzBench相比，FirmReBugger针对单体固件的MMIO/DMA/中断等特有障碍专门设计，为固件模糊研究的可复现评估立下标杆，有望成为该领域后续进展的对照基准。
- **论文 PDF**: [FirmReBugger: A Benchmark Framework for Monolithic Firmware Fuzzers](https://www.usenix.org/system/files/usenixsecurity26-duong.pdf)

### 网络与协议安全（10 篇）

#### Hop: A Modern Transport and Remote Access Protocol
- **作者**: Paul Flammarion; George Hosono; Wilson Nguyen; David Adrian; Zakir Durumeric
- **方向**: 网络与协议安全
- **解读**: SSH 自 2006 年标准化以来暴露出系统性问题：握手不保护完整转录本导致降级攻击与窃听解除（Bäumer et al. 2023）、主机认证依赖 TOFU 不可靠（连 SSH 作者 Ylönen 都呼吁重新设计）、ssh-agent 密钥转发可被任何根权限进程滥用促成横向移动、客户端认证难以对接现代企业身份体系、不支持漫游与间歇连接。Mosh、Guardian Agent、基于 QUIC 的重建等增量修补均未获广泛采用。

Hop 是三层协议：Hop Transport 基于 Noise 框架（可发现模式用 XX、隐藏模式用 IK），采用固定版本化原语、无带内协商，全部对称密码用一个 duplex 构造实现，用 ML-KEM 提供后量子前向保密，运行于 UDP；Hop Tubes 提供复用的可靠/尽力而为"管道"，拥塞控制类似 NewReno；Hop Remote Access 提供远程 shell 等四种服务，内置 ACME 式证书签发消除 TOFU、轻量证书保证握手消息不超过 1400 字节 MTU，并设计 Principal/Delegate/Target 三方授权委托协议，凭据绑定动作、目标、时限且一次性使用。实现仅 18K 行 Go，对比 OpenSSH 约 130K 行 C。

论文系统化提出 8 条现代远程访问协议设计需求，并在地理分布式测试床上评估：吞吐与 OpenSSH 相当，连接建立更快、终端响应更好；隐藏模式单 RTT 完成握手且对网络扫描者不可见。

我的思考：把 WireGuard 式的"干净重写"路线引入远程访问域，需求提炼有参考价值。最大创新是授权委托协议与原生证书化身份，直击 ssh-agent 横向移动与 TOFU 两大痛点。但新协议面临冷启动：无生态、审计不足、隐藏模式依赖带外密钥分发，迁移成本远高于 VPN，短期内作为设计提案的价值大于即时可用性，其价值在于引发"现代远程访问协议应长什么样"的讨论。

- **论文 PDF**: [Hop: A Modern Transport and Remote Access Protocol](https://www.usenix.org/system/files/usenixsecurity26-flammarion.pdf)

#### Opossum Attack: Application Layer Desynchronization using Opportunistic TLS
- **作者**: Robert Merget, Nurullah Erinola and Marcel Maehren, Lukas Knittel, Sven Hebrok, Marcus Brinkmann, Juraj Somorovsky, Jörg Schwenk
- **方向**: 网络与协议安全
- **解读**: 许多应用层协议（HTTP、FTP、SMTP 等）通过隐式 TLS（专用端口直接握手）或机会式 TLS（原端口明文启动、经 STARTTLS 或 Upgrade 头切换）改造加密。本文发现：当同一端同时支持两种方式时，即使实现完全符合标准、无任何软件缺陷，TLS 的认证与信道完整性保证也会被形式化打破——两种变体在握手后的首条消息方向不同，形成事实上的两个不同协议，ALPACA 攻击引入的严格 ALPN 校验（RFC 9325）无法防御。
攻击者只需扮演纯 MitM：在 I2O 场景（如 HTTP）中，攻击者先向明文端口发起带 Upgrade: TLS/1.0 头的请求建立机会式 TLS，再把受害者（隐式 TLS）的 ClientHello 与握手消息原样转发，握手后服务器先发送对攻击者请求的响应，客户端将其误认为对自己请求的应答，连接从此永久失步。对 HTTP 作者给出四个利用方向：资源混淆、会话固定（CUPS 反 CSRF cookie）、反射 XSS 放大（Range 头）、以及 Apache 特有的请求走私（Content-Length 致请求体被误读，可经 favicon 请求泄露含 HttpOnly/Secure 标志的 cookie）。SMTP/POP3/FTP/LMTP/NNTP 受 O2I 场景影响，IMAP 因非同步协议仅理论上受影响。
全网扫描显示影响面巨大：超过 300 万主机在易受影响协议上同时支持隐式与机会式 TLS；HTTP 方面有 20,121 台服务器支持机会式 HTTP（横跨 35 个端口），其中 2,268 台同时支持 HTTPS，539 台与隐式 HTTPS 使用相同域名，构成可直接利用场景。
该攻击的优雅之处在于"零实现缺陷、仅需一端支持机会式 TLS"，延续 ALPACA 脉络并揭示其对策的盲区；Apache 的非标准 Upgrade 行为进一步把攻击升级为请求走私与 cookie 窃取，说明协议整合方式的语义差异是结构性弱点。作者建议放弃机会式 TLS。局限是除 HTTP 外多为 PoC 级验证，扫描仅推断支持性而非验证可利用性。

- **论文 PDF**: [Opossum Attack: Application Layer Desynchronization using Opportunistic TLS](https://www.usenix.org/system/files/usenixsecurity26-merget.pdf)

#### Invariant-Guided Logical Testing of Open RAN Controllers
- **作者**: Tianchang Yang
- **方向**: 网络与协议安全
- **解读**: 

**问题**：Open RAN（O-RAN）把一体式 RAN 拆分为多厂商、软件定义组件，RAN 智能控制器（RIC）与第三方 xApp 的逻辑正确性直接关系网络安全与稳定。但 O-RAN 实现异构、缺乏显式正确性 oracle 和正式规范，现有工具只能检测内存类问题，无法自动检测逻辑错误；人工构造 oracle、形式化建模、差分测试分别受制于规模、规范缺失和实现不可比。

**方法**：InvaRAN 以动态推断的"可能程序不变量"作为行为代理：先在受控环境（10 个模拟 RAN 节点、24 小时良性流量）收集执行轨迹，推断标量不变量（Daikon）与跨变量相关不变量，再将其插桩为断言并用反馈制导模糊测试（ORANalyst）验证。关键设计有二：一是通过静态分析（校验分支识别+有界影响分析）把不变量分为关键/非关键以降低假阳性；二是超越模板推断，用跨轨迹聚类捕获多对多语义关联（如 E2 节点 ID 与小区 ID 映射），并做线程/消息归属的上下文追踪。

**结果**：在 SD-RAN 和 O-RAN-SC 两个生产级控制器的平台组件与 xApp 上发现 9 个此前未知问题，含 7 个逻辑错误和 2 个崩溃（#4、#7），可导致组件级 DoS、接受伪造指标、状态不一致。与针对相同组件的先前工作 ORANalyst 相比，InvaRAN 发现了其漏掉的崩溃 bug。

**我的思考**：以"良性执行推断的不变量"充当缺失规范的替代 oracle，思路巧妙且工程完整，相关分析比模板推断表达力更强，是对 O-RAN 安全测试的重要补充。局限：良性轨迹只是经验近似，存在假阳性/假阴性；24 小时采集开销大；方法面向 Go 实现；未考虑共谋。领域影响上，该范式可推广到其他消息驱动、并发、无规范的协议实现测试，与差分测试和形式化建模互补。

- **论文 PDF**: [Invariant-Guided Logical Testing of Open RAN Controllers](https://www.usenix.org/system/files/usenixsecurity26-yang-tianchang.pdf)

#### Analyzing the WebRTC Ecosystem and Breaking Authentication in DTLS-SRTP
- **作者**: Martin Bach, Vukašin Karadžić, Lukas Knittel, Robert Merget and Jean Paul Degabriele
- **方向**: 网络与协议安全
- **解读**: 

**问题**：DTLS-SRTP 保障实时媒体通信安全，被 Zoom、Teams、Google Meet 等主流音视频平台广泛使用，是浏览器实时通信标准 WebRTC 的核心。但 WebRTC 由 HTTP、TLS、SDP、ICE、STUN、TURN、DTLS、(S)RTP 等十几种协议拼合而成，系统过于复杂难以自动审计，这一服务数亿用户的核心通信技术的部署安全长期缺乏探索。

**方法**：作者开发了自动化 MitM 测试框架 DMS（DTLS-MitM-Scanner），完全黑盒、仅需 MitM 能力，基于 TLS-Attacker、pcap4j、Selenium 和 jitsi-srtp 构建。针对 DTLS 认证机制设计 19 项潜在认证绕过测试（空证书、任意证书、缺失 CertificateVerify、乱序注入第二公钥等），并用修改过的浏览器驱动真实应用数据，以"能否收到可解密媒体数据"判定漏洞是否真正可利用。

**结果**：案例研究覆盖 24 个服务商的浏览器与移动应用，共 33 个媒体服务器实现；其中 19 个存在可让攻击者在 DTLS 层打破认证的漏洞，9 个（服务数亿用户，含 Webex、Discord、Zoom、Steam）被证实可被纯 MitM 攻击者窃取媒体数据。作者做出窃听 Webex 视频会议 PoC（空证书，CVE-2025-20215）；Zoho 用 512 位 RSA 证书（cado-nfs 4.5 小时分解）；FreeSWITCH 存在覆盖远端指纹的代码缺陷。生态上 DTLS 1.2 普遍、1.3 几乎缺席，但所有测试浏览器都接受 512 位 RSA 服务器证书。

**我的思考**：这是首个对 DTLS-SRTP 生态的系统性黑盒审计，19/33 的认证可破率说明"SDP 指纹认证自签名证书"模型在工程实现中大面积走样，状态机、指纹校验、证书配置的细节错误足以击穿整条信任链，警示意义强。局限是黑盒方法无法排除其余 11 个系统的潜在可利用性，且披露后部分厂商已修复。领域影响：为浏览器与媒体服务器加固（证书参数、指纹强制校验、DTLS 1.3）提供了证据基础。

- **论文 PDF**: [Analyzing the WebRTC Ecosystem and Breaking Authentication in DTLS-SRTP](https://www.usenix.org/system/files/usenixsecurity26-bach.pdf)

#### DNS Cache Poisoning Like it's 2006
- **作者**: Omer Ben-Simhon and Amit Klein
- **方向**: 网络与协议安全
- **解读**: DNS 支撑几乎所有互联网服务，BIND 9 是部署最广的开源解析器（约 42% 份额）。传统观点认为 Kaminsky 攻击后，随机化源端口与 TXID 使少包投毒不再可行；本文展示新型攻击：可靠地同时预测源端口与 TXID 两个关键参数（此前多只针对其一），且完全客户端侧完成（无需攻击者自建权威服务器），据作者所述尚属首次。

攻击核心是破解 BIND 的 Xoshiro128** PRNG（它决定 TXID、源端口与 RRset 洗牌顺序）。三步：从观察到的查询/响应获取 PRNG 输出（RRset 顺序经 Fisher-Yates 与 CRT 唯一恢复完整 32 位输出，RRset 规模 ≥23 即可；TXID 给低位读出）；利用 star-star 变换的旋转与乘法性质反推内部状态位；在 GF(2)^128 上列线性方程组、高斯消元恢复完整 128 位初始状态，亚毫秒级预测端口与 TXID。三个变体：RRset（"Living off the Land"，利用第三方 ≥23 记录的大 RRset）、RRset-ANY（第三方权威服务器上的攻击者区域）、RR-QMA（针对 9.18 分支，基于 QNAME 最小化观察 TXID）。

基线中 RRset/RRset-ANY 在 9.20/9.21 的 UDP/TCP 下成功率均 100%，RR-QMA 92–100%；FreeBSD 10/10，DNS cookies 超时后 10/10，转发器 10/10；真实互联网 4 个公网 BIND 9.20 中 3 个 500 步内成功、83% 偏移 0–3（≤4 个伪造包）；10,000 QPS 缓存未命中高负载下仍有效。已披露给 ISC 与 FreeBSD，获两个补丁与 CVE。

我的思考：标题"回到 2006"点出要害——时隔二十年少包投毒再次可行，且攻击门槛大降（纯客户端、无需自有域名与服务器）。全 PRNG 状态恢复与 RRset 顺序侧信道是核心创新，LotL 变体消除基础设施需求。局限：限于默认配置 BIND 9，非默认配置影响未系统量化，公网样本小。该工作警示解析器随机性来源，防御应转向密码学安全随机性、0x20 编码与响应验证等多层机制。
- **论文 PDF**: [DNS Cache Poisoning Like it's 2006](https://www.usenix.org/system/files/usenixsecurity26-ben-simhon.pdf)

#### DaLens: Charting DNS Self-Amplification Threats at Large
- **作者**: Liwen Xu and Zechao Cai, Huayi Duan, Adrian Perrig
- **方向**: 网络与协议安全
- **解读**: DNS 自放大攻击（SAA）诱导递归解析器对单个客户端请求向权威服务器生成过量查询，以极小代价耗尽受害者资源。攻击原语多样（NS fan-out、NS/CNAME 链、稠密委派自探测），且大多源于协议正当设计（权威委派、CNAME 重写、QNAME 最小化）而非实现 bug，可组合出乘性放大。此前从未有人对全球开放 DNS（ODNS）的 SAA 影响做大规模测量，运营商无法分诊与部署防御。

论文开发 DaLens 框架，核心是可编程响应式权威服务器（PRN），能按查询中编码的模式动态生成应答，灵活触发任意查询模式、绕开标准服务器限制。方法学上形式化 ODNS 结构（递归器/转发器、egress 集、转发依赖），设计递归器识别流水线（区分单/多 egress、anycast/unicast、并发转发等类型并推导稳定入口-出口映射），再以原始探测与组合放大系统测量各递归器的消息放大因子（MAF），扫描全 IPv4 空间。

结果：307K 持久公共解析器中仅 9.5%（29K）是真递归器（放大核心）；110,129 个转发器确定性转发到放大器、189,504 个多上游；7,532 个并发转发（MAF 倍增器）。温和放大模式下，>10% 放大器 MAF 已超 100，部分接近 1000，且许多漏洞披露后仍未修补；约 50% 解析器抓取 >10 条 NS 记录。已与受影响运营商协调披露。

我的思考：首次互联网规模 SAA 测量，PRN 框架与 ODNS 结构形式化可复用，核心发现"ODNS 结构性脆弱——只需瞄准少数递归器核心"对防御优先级有直接指导。局限：测量限于可探测的公共解析器，样本有偏；MAF 为保守估计（未用最激进模式）；转发链路深层结构推断有不确定性。该工作也暴露漏洞披露后修补率低的问题，提示需协议层硬化（限流、一致性校验）而非仅依赖单点修补。
- **论文 PDF**: [DaLens: Charting DNS Self-Amplification Threats at Large](https://www.usenix.org/system/files/usenixsecurity26-xu-liwen.pdf)

#### Tracegram: Framing Trace-Level Traffic Analysis with Temporally-Aware Multiple Instance Learning
- **作者**: Jian Qu, Xiaobo Ma
- **方向**: 网络与协议安全
- **解读**: 现代网络行为跨多条流并随时间演化，APT 攻击依次经历侦察、投递、C2 与横向移动，证据分散在多条流上；现有包级或单流方法割裂跨流时序上下文，而把全部流量拼接成长的 Transformer 序列又受 O(L²) 复杂度制约，难以做 trace 级分类、检测与归因。

方法上，Tracegram 把 trace 建模为多示例学习（MIL）：bag=trace、instance=流。流水线四段：包分词（元信息+时间戳+十六进制载荷，双射无损）；流编码器（线性注意力 O(L)+局部注意力+可逆网络+Token Shift，先通用预训练再任务微调）；时序感知聚合（Bi-LSTM 编码流间顺序与时间间隔，注意力加权生成 trace 指纹并提供关键流归因）；轻量分类头。理论上证明在实例可分离与正交条件下两阶段目标等价于端到端最优。

四个公开数据集上：结构任务 IDI/ISD 的 F1 达 1.000，UAV 0.9751 超过 SOTA 的 TrafficFormer（0.9648），细粒度载荷任务 KWS 0.9539 略逊（0.9723）；推理延迟 29.8ms 对比 ET-BERT 124.5ms（4.17× 加速）、峰值内存 480MB 对比 2450MB（降 80.4%）；任务微调把 KWS 从 0.3952 提升到 0.9539；DAPT 数据集 APT trace 上注意力热图与攻击各阶段流对齐。

我的思考：系统性地把“trace 级、跨流、可解释”三者统一进 MIL 框架，理论给出分解成立的条件，工程上线性复杂度使其可部署于边缘设备，归因信号直击运维取证需求，是该方向少有的完整闭环。局限：KWS 等载荷密集任务仍输给二次注意力模型，trace 构造依赖窗口/交互边界的人工选择，归因尚未与真实取证流程验证。相比单流指纹识别，这是向多流协同理解迈出的扎实一步，但需更多真实 APT 数据检验泛化。

- **论文 PDF**: [Tracegram: Framing Trace-Level Traffic Analysis with Temporally-Aware Multiple Instance Learning](https://www.usenix.org/system/files/usenixsecurity26-qu.pdf)

#### WAVED: Principled Identification of Off-Path Exploitable Weak Verifications within the TCP/IP Protocol Suite
- **作者**: Yizhou Zhao and Xuewei Feng, Min Li, Ke Xu
- **方向**: 网络与协议安全
- **解读**: TCP/IP 协议栈对接收载荷的“弱校验”（缺少可靠信息或实现缺陷导致）可被 off-path 攻击者利用：仅凭 IP 欺骗与盲注，即可伪造 ICMP Fragment Needed 报文把共享路径 MTU 从 1500 降到 552、毒化路由缓存或注入数据包。此前工具要么需人工过滤约束（隐式污点分析），要么因过度抽象或符号执行不可扩展到整栈。

方法上，核心洞察是“约束强度∝攻击者需猜测的强约束字节数”，并据此提出方向敏感污点。WAVED 四组件：面向协议栈的流/上下文/字段敏感指针分析（字节偏移 GEP 处理，构建上下文敏感 SVFG）；TPG 图上字节粒度 MOP 污点分析（8×8 传播位图+CFL 可达性）；分支影响计算器（taint tuple⟨L,G,E⟩ 区分有界/无界比较、识别窗口内强约束与窗口外弱约束，并处理 tcp_sequence() 这类隐式控制流）；弱路径识别（头激活、约束去重、强度过滤，排除 TCP/DCCP seq/ack 等强秘密字段）。

在 Linux 5.15、6.8 与 FreeBSD 14.1 的 IPv4/IPv6 实现上共发现 19 个弱校验漏洞（16 个 Linux、3 个 FreeBSD），其中 14 个为新发现（Linux 12 个、FreeBSD 2 个），均在受控环境验证可实际利用，已负责任披露并获得 Linux 社区致谢；整栈分析耗时 11–125 分钟；Linux 上 34 条 MTU 毒化路径与 12 条网关毒化路径可归纳为 6 份独立报告。

我的思考：首个全自动、系统化的弱校验漏洞发现框架，方向敏感污点精确刻画了“同一字节上强/弱约束”的语义差别，比 Chen 等人的隐式污点方法显著减少人工干预，在整栈规模上把可扩展性与精度做到了实用平衡。局限：MOP 抽象对运行时不达路径有近似误差、强度阈值（8 字节）依据先例设定、路径合并依赖后支配分析。相比符号执行，该路线证明静态污点+方向建模是扫描协议栈弱校验更可行的范式。

- **论文 PDF**: [WAVED: Principled Identification of Off-Path Exploitable Weak Verifications within the TCP/IP Protocol Suite](https://www.usenix.org/system/files/usenixsecurity26-zhao-yizhou.pdf)

#### Network-Level Prompt and Trait Leakage in Local Research Agents
- **作者**: Hyejun Jeong
- **方向**: 网络与协议安全
- **解读**: 本地部署的 Web/研究智能体（WRA）常被组织与个人用于隐私敏感任务（医疗、法律、金融），但"本地运行+TLS 加密"并不等于隐私：WRA 每次请求访问 70–140 个域名、单会话遍历 40–100 个域名（人类通常只访问少数网站），且检索-合成循环产生低方差时序模式，与人类浏览截然可分，留给被动观察者（DNS 解析器、恶意 ISP、VPN、企业防火墙）丰富的元数据。

作者提出仅基于网络元数据（IP 与访问时序）的两阶段攻击：提示恢复——把域名序列喂给推理 LLM（ICL 或微调）重建用户提示词，在 GPT-Researcher、AutoGen、Browser-Use、Deep Research 四种智能体上验证；特征推断——用一周多会话轨迹推断 32 个潜在用户特征。并设计本体感知指标 OBELS（功能/领域/语义/粒度四维）评估泄漏，弥补 SBERT、LLM-as-Judge 对"行为等价泄漏"的盲区。

5-shot ICL 下恢复超过 73% 的功能与领域知识（Efunc 0.77、Edom 0.735，GPT-Researcher 泄漏最多）；特征推断在 32 个特征中高精度恢复 19 个，健康保险（0.98）、退伍军人身份（0.90）、就业状态（0.88）、家庭语言（0.86）接近完美；40% 轨迹被掩蔽或混淆时攻击仍有效。防御方面，注入 LLM 生成的诱饵提示与虚拟人设、或限制域名多样性/重定向到通用来源，可使攻击有效性平均下降 29% 而效用几乎无损。

我的思考：把网站指纹/流量分析的经典威胁搬到 LLM 智能体，核心洞见是"智能体化的密集、确定性浏览反而放大了元数据泄漏"，对'本地+加密即安全'的流行认知是重要纠正。局限：提示重建是语义级而非逐字（OBELS 恰当地承认这一点）、需要代理数据训练攻击模型、防御仅降效不消除。对在医疗、法律、金融领域部署本地代理的机构有直接警示，OBELS 度量本身也值得被后续工作采用。
- **论文 PDF**: [Network-Level Prompt and Trait Leakage in Local Research Agents](https://www.usenix.org/system/files/usenixsecurity26-jeong.pdf)

#### Estimating the Amount of Script-generated Traffic in a Mixture

- **作者**: Cormac Herley

- **方向**: 网络与协议安全

- **解读**: 点击欺诈、非真人社交媒体互动等脚本流量常伪装成人类行为，且无标签、类别特征难聚类，连"流量中机器占比 (1−α)"这一基本量都难以估计——Musk 与 Twitter 管理层"5% vs 90%"之争正是这一不确定性的写照，其经济后果直接影响广告计价与平台信誉。

方法核心：若干净流量中存在一对统计独立特征（如地理分布的时间不变性），则干净条件分布矩阵为秩一矩阵；把估计 α 的上界转化为"寻找最大化简单目标函数的秩一矩阵"问题，给出无需暴力搜索的高效求解算法并推导界的最优紧度；针对有限样本下秩一矩阵不再精确秩一的情形，导出置信区间以保证所得确实是上界。

蒙特卡洛模拟（1000 万样本、45×45 至 300×300 矩阵）验证估计准确且对假设中度违反鲁棒；Twitter（X）数据（38 个账号的 42.7 万粉丝，API 关闭前采集）：市场上待售的大粉丝量账号被标记 >90% 机器人粉丝，学术会议与知名研究者账号 <20%；屏幕名以 8 位随机数字结尾的账号约 90% 为 bot，普通名字账号 >80% 为真人。

我的思考：把无标签混合比例估计形式化为秩一矩阵优化，理论干净、与具体平台无关，可迁移到 Reddit、Instagram、点击欺诈等场景；误差区间推导对有限数据实践是关键补充。局限：独立性假设无法直接验证，需领域知识选择特征对；Twitter 验证集规模有限且非随机抽样；对刻意模拟人类相关性的高级 bot 效果未知，LLM 时代伪造内容更逼真后该方法的适用范围值得警惕。

- **论文 PDF**: [Estimating the Amount of Script-generated Traffic in a Mixture](https://www.usenix.org/system/files/usenixsecurity26-herley.pdf)

### 密码学与协议（41 篇）

#### Heli: Heavy-Light Private Aggregation
- **作者**: Ryan Lehmkuhl and Henry Corrigan-Gibbs; Emma Dauterman; David J. Wu
- **方向**: 密码学与协议
- **解读**: 私密聚合让应用方在不接触用户明文的情况下收集统计信息，但传统方案（Prio、Whisper）要求两个独立基础设施方，第二方每轮工作量与客户端数量线性相关，导致只有资源充足的大公司能部署——当前唯一服务商 Divvi Up 需要收费与商业谈判。Heli 的目标是把"第二方"成本降到可被低预算非营利机构（如 ISRG）承担。

Heli 的不对称设计：重服务器（聚合方）工作量与客户端数成正比，轻服务器（解密方）在一次注册设置后每轮工作量与客户端数无关。核心是新的"聚合专用加密"原语：基于 Diffie-Hellman 密钥同态 PRF，客户端用私钥和轮标识生成掩码，密文=掩码·g^m；聚合方将密文乘积压缩为单个密文；解密方用聚合密钥去掩并做小区间离散对数即可恢复总和。恶意客户端用 Schnorr 与 Bulletproofs 零知识证明防御，解密方用 PRF 派生密钥以便低成本处理离线客户端。

在千万客户端、聚合 32 个 1-bit 整数场景：重服务器 240,000 core-s，轻服务器仅 7 core-ms；AWS 成本上轻服务器 0.000025 美分，比重服务器（113 美分）低约 120,000 倍，而重服务器比 Prio/Whisper 贵最多 38 倍；10% 掉线时轻服务器 CPU 仍降低 178,000 倍。实现为 5,000 行 Rust（Curve25519），代码开源。

我的思考：这是少有的由"部署经济学"驱动的密码学工作——把第二方负载从线性降到亚线性，直接改变私密聚合的可行部署格局。代价是重服务器与客户端开销随测量数量/位宽增长 2-138 倍，且恶意客户端防御依赖更重的离散对数 ZK 证明。本质上它是 LERNA/OPA 委员会方案缩为单成员的特例加系统化工程，理论新颖度一般，但对推动隐私遥测普及的实际意义显著。

- **论文 PDF**: [Heli: Heavy-Light Private Aggregation](https://www.usenix.org/system/files/usenixsecurity26-lehmkuhl.pdf)

#### SMASH: Scalable Maliciously Secure Hybrid Multi-party Computation Framework for Privacy-Preserving Large Language Models
- **作者**: Yunlv Lv and Rui Zhang; State Key Laboratory of Cyberspace Security Defense; School of Cyber Security; Zhiyuan Zhang; Ziyi Wan; State Key Laboratory of Cyberspace Security Defense; School of Cyber Security; Lanxue Zhang; School of Cyber Security; Minhui Xue; Jiangtao Li; Yanan Cao; School of Cyber Security
- **方向**: 密码学与协议
- **解读**: 隐私保护 LLM 推理需要多方计算（MPC），但恶意安全框架在扩展到大规模模型时"性能崩溃"：非线性算子（ReLU/GELU/Softmax）通信复杂度 O(n²)、份额转换（edaBit）开销达 O(n²)–O(n³)、多项式近似损失精度。而医院联盟（如 4CE 汇集约百家医院数据）、银行间反欺诈等真实场景恰需要多方、恶意安全的部署。

SMASH 以"算术份额+查找表"（SM-LUT）重构非线性计算：用离散傅里叶变换（DFT）在 ElGamal 同态加密上实现逐方旋转的掩码表预处理，离线阶段 O(n) 通信与轮数，在线阶段常数轮、与位宽和方数无关；恶意安全性由轻量零知识证明（Σ-协议+Pedersen 承诺）强制；设计仅依赖加法与份额重构的 A2L/L2A 恶意安全转换，A2B/B2A 用 SM-LUT 半诚实变体；基于 MP-SPDZ 实现（C++17）。

非线性算子支持 2–32 方且无可信设置，WAN 下常数两轮设计仅增约 0.2–0.3s（MD-SONIC 超 16 方后急剧恶化）；A2L/L2A 相比 MP-SPDZ 的 edaBits 离线成本降超 10³ 倍；端到端 BERT 推理（Tiny/Base/Large）比 MD-ML 快 15.8/18.5/18.9 倍、通信降 976.9/1019.9/1038.8 倍；数值误差全面低于 MP-SPDZ（LUT 避免多项式近似截断累积）；在 Qwen3-0.6B 上随 token 数线性扩展，精度损失仅 2–5%。

我的思考：用 LUT+DFT 旋转把非线性与转换从 O(n²) 拉到 O(n)、并做到 WAN 友好，是实质性的系统贡献，直接拓宽恶意多方 LLM 推理的可部署范围。取舍清晰：离线表存储 O(Mn)、A2B/B2A 仅半诚实、LLM 评估限于算子层面而非完整端到端。与半诚实的 Bolt/BumbleBee 及两方 FSS 方案定位分明，为大规模隐私协作（医疗、金融）提供了恶意安全底座。

- **论文 PDF**: [SMASH: Scalable Maliciously Secure Hybrid Multi-party Computation Framework for Privacy-Preserving Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-lv.pdf)

#### Secure Protocol Composition under Dynamic Corruption: Scaling Up Symbolic Analysis for Real-World Security Properties
- **作者**: Cas Cremers
- **方向**: 密码学与协议
- **解读**: 符号化协议验证（如 Tamarin、ProVerif）在 TLS 1.3、WPA2 等大型协议上已逼近极限，而现有组合验证结果局限于静态腐化威胁模型，无法证明前向保密（Forward Secrecy）等现代性质，因而从未被实际采用。本文给出应用 π-演算中第一个支持动态腐化攻击者的安全组合定理。
方法上，作者形式化定义动态腐化变换 ς(P,ω)、组合上下文、共享数据、set-up 保证与可执行模型，并证明主定理（Theorem 5.5）：在签名不相交（可通过打标签实现）、满足若干语法条件、共享变量不外泄且保持新鲜性等前提下，子协议分别满足的迹性质在组合后依然成立，即"对组合协议的任何攻击必已存在于某个子协议"。定理还推广了经典不相交组合结果（任意密码原语而非仅加解密/签名/哈希），并兼容 ProVerif 与 SAPIC+。
案例研究验证了实用性：数据交换协议与（带签名的）Diffie-Hellman 密钥交换的组合，以及 RFC 8446 + ECH 扩展范围内 TLS 1.3 前向保密的组合分析。整体分析在 10% 案例中无法给出结果，而组合分析全部成功，平均运行时间下降 71%、内存下降 86%；与此前最新分析（未能证明 63% 的性质）相比，本文证明了全部目标性质。
这是组合验证领域的重要突破：动态腐化 + 前向保密正是真实协议分析的核心需求，且限制条件（不相交签名、仅迹性质、单向数据传递、不传信道名）在实践中多数可满足。局限也清晰：等价性质（隐私类）不在覆盖范围、双向密钥派生等场景仍需扩展，且不相交签名要求使模型工作量仍不可忽视。

- **论文 PDF**: [Secure Protocol Composition under Dynamic Corruption: Scaling Up Symbolic Analysis for Real-World Security Properties](https://www.usenix.org/system/files/usenixsecurity26-cremers.pdf)

#### Breaking Widely Deployed Perceptual Hash Functions: Black-Box Collisions in Apple NeuralHash and Microsoft PhotoDNA
- **作者**: Diane Leblanc-Albarel and Bart Preneel
- **方向**: 密码学与协议
- **解读**: 感知哈希被广泛用于版权与非法内容检测，且被各国考虑推广到客户端扫描（CSS）以检测加密内容中的 CSAM。苹果曾基于 NeuralHash 提出 CSS 方案，撤回后该函数仍部署于所有设备；微软 PhotoDNA 被 NCMEC 等 150+ 机构使用，官方宣称误报率不高于 1/500 亿。本文首次对这些声称进行独立的黑盒实证检验。
方法上，作者从 iPhone（iOS 16.2）提取 NeuralHash 模型并转 ONNX，PhotoDNA 用泄露代码黑盒计算；数据集为 PASS（非人脸）与 CelebA（202,599 张人脸，含至少 4,629 张重复图），构造非人脸、未模糊人脸及四种模糊人脸共 6 类图像集，各哈希 30,000 张，分析比特分布均匀性与独立性（SMC 矩阵），统计感知不同图像的非法碰撞（假阳性）与漏检（假阴性）。
结果显示：人脸图像的 96 位哈希比特高度偏斜且互不独立（未模糊人脸 30 位、高模糊 61 位超出 ±3σ 区间），理论碰撞期望从理想的 2⁴⁸ 降至 2³⁴–2⁴³·⁵；实测未模糊人脸即出现 12 个非法碰撞（假阳性率 0.01%，首个碰撞仅需约 2¹⁶ 次哈希），高模糊人脸达 270 个（0.29%）；假阴性率高达 47.3%–77.3%。PhotoDNA 在 L2 阈值 150/175 下首个近碰撞出现在约 2¹⁷ 与 2¹⁴·⁶ 次哈希后（此前报道阈值在 200–225 之间，本文在距离 144 即发现首个），假阴性率 24.1%/17.8%，与"阈值 150–175 可同时实现 <1% 假阴性且零假阳性"的声称矛盾。苹果确认复现全部结果。
本文是首个证明 NeuralHash 存在人脸受限输入下自然精确碰撞、并在如此低阈值下发现 PhotoDNA 近碰撞的工作，直接动摇 CSS 大规模部署的可行性：数十亿图像规模下，极低假阳性率也会产生海量错误指控。作者用"等效独立比特数 n′"模型外推大规模场景，披露流程规范。局限在于实验仅限 CelebA 人脸分布，对真实数据分布的外推仍属估计。

- **论文 PDF**: [Breaking Widely Deployed Perceptual Hash Functions: Black-Box Collisions in Apple NeuralHash and Microsoft PhotoDNA](https://www.usenix.org/system/files/usenixsecurity26-leblanc-albarel.pdf)

#### Zero Knowledge (About) Encryption: A Comparative Security Analysis of Three Cloud-based Password Managers
- **作者**: Matteo Scarlata, Giovanni Torrisi and Matilda Backendal, Kenneth G. Paterson
- **方向**: 密码学与协议
- **解读**: Bitwarden、LastPass、Dashlane 三大云密码管理器（合计超 6,000 万用户、23% 市场份额）都宣称"零知识加密"，暗示即使服务器完全恶意也无法获知密码库内容；密码库的高敏感性使服务器成为天然攻击目标。本文首次在"完全恶意服务器"威胁模型下对三家产品进行系统性安全分析。
方法上，作者沿用"Cryptography in the Wild"方法论：审阅白皮书与客户端代码、建立密码学伪代码模型、搜索攻击向量并编写全部 PoC 验证。攻击按成因分为四类：密钥托管（账户恢复/SSO）、条目级加密导致的库完整性缺失、共享功能、向后兼容降级，共发现 Bitwarden 12 个、LastPass 7 个、Dashlane 6 个攻击。
攻击后果从定向完整性破坏到组织全库沦陷，且多数可恢复密码。典型如 BW01 恶意自动登记（服务器替换组织公钥即可解密用户密钥，可蠕虫式蔓延）、LP01 恶意密码重置、BW07/LP04 移除 KDF 迭代数（暴力破解加速 30 万倍/20 万倍）、BW11/BW12 降级到无完整性 CBC、DL01 事务重放等。作者已负责任披露并给予厂商至少 90 天修复期。
本文揭示的设计反模式具有普遍警示意义：公钥普遍缺乏认证（配合密钥托管与共享即致全库沦陷）、错误假设公钥密文具有来源认证（导致密钥替换攻击）、LastPass 使用无完整性保护的 AES-CBC、以及"库非整体"导致的剪切-粘贴式攻击即使在使用认证加密时依然成立。与假设被动服务器的既有工作相比，恶意服务器模型更贴合"服务器被攻破"的现实；作者也承认攻击集合并非完备，呼吁以形式化手段确立此类系统在恶意服务器模型下的安全目标。

- **论文 PDF**: [Zero Knowledge (About) Encryption: A Comparative Security Analysis of Three Cloud-based Password Managers](https://www.usenix.org/system/files/usenixsecurity26-scarlata.pdf)

#### Distributed Vector Commitments and Their Applications
- **作者**: Rui Gao and Huaqun Wang, Zhiguo Wan, Yuncong Hu
- **方向**: 密码学与协议
- **解读**: 向量承诺（VC）允许证明者对向量做承诺并为任意位置生成短证明，广泛用于无状态区块链、可验证数据库（VEDB）与审计；但现有方案都假设单机持有完整向量，既无法用于分布式架构，也无法并行加速——例如对 2³⁰ 向量用 aSVC 生成全部位置证明需约 50 天。本文提出首个分布式向量承诺（DVC）概念与方案。
核心创新是混合拉格朗日扩展（HLE）：先对每个子向量用低度扩展（LDE）得到一元多项式，再用类 MLE 的方式（eq 函数加权）聚合为多元多项式 F(s,x)，使承诺与证明既依赖整向量又具备可分解结构。作者进一步给出 HLE 分解方法（d-分区与 HLE-Aux 树）：同一子向量内所有位置证明共享"部分证明"，机器间仅需一轮 O(logM) 通信即可由各机独立生成全部证明；该分解引理还能把任意 MLE 类 VC 方案转化为 DVC。方案以 PST 承诺替换多项式实现亚线性证明大小，并支持批量证明、证明聚合（部分分式分解 + 内积论证）与次线性更新。
效率上，HLE-DVC 每机存储与计算均为 O(N/M·logN)，证明大小与每机通信均为 O(logM)。实验表明：256 台机器为 2³⁰ 长度向量生成全部证明耗时 17,515 秒，相对单机实现获得 256× 并行加速，比著名单机方案 Hyperproofs 快 142×，每机通信量仅 0.768 KB；代码已开源，并给出安全定义与形式化证明。
本文首次形式化 DVC 并证明其安全性，分解引理可改造既有 MLE 方案，工程数据扎实。不过其应用价值仍取决于信任模型：主节点聚合 auxdata 的角色、恶意机器能否伪造部分证明值得审视；此外方案依赖配对群与 2 的幂次长度假设，通用向量长度与跨域应用的适配仍需后续工作。

- **论文 PDF**: [Distributed Vector Commitments and Their Applications](https://www.usenix.org/system/files/usenixsecurity26-gao-rui.pdf)

#### End-to-End Encrypted Collaborative Documents
- **作者**: Christian Knabenhans, Zayd Maradni, Carmela Troncoso
- **方向**: 密码学与协议
- **解读**:
Google Docs、Microsoft 365 等协作文档常含敏感数据，却不提供端到端加密（E2EE），用户数据暴露于服务商与攻击者；E2EE 目前基本限于消息传递。把 E2EE 扩展到协作文档需同时处理访问控制、并发编辑、异步传播与一致性，缺乏形式化基础。现有方案要么只达半诚实安全（HE/MPC）、要么无细粒度权限或证明（CryptPad 等）。

论文形式化 E2EE-CD 的需求（共享/归档/删除/异步/并发/收敛 + 参与者列表不可伪造/机密性/完整性），定义为参数化理想功能 FCD（以泄漏与威胁模型为参数，含 Rollback 开关）；证明通用构造：任何"强收敛调和机制 Rec + 端到端加密异步广播通道 E2EE-AB"组合即可实现 E2EE-CD（Lemma 1），安全性直接由底层通道提升。随后针对调查记者场景实例化 SignalCD：把 Signal 群消息协议（匿名凭据+加密群表+Sender keys）实现为 E2EE-AB，满足最小基础设施、无新增信任、简单密钥管理三项要求。

通过 UC 证明 SignalCD 安全性（诚实好奇用户+恶意服务器，Rollback=0 以反映 Signal 现状），并显式建模各层泄漏（群表索引、消息大小）；实现原型并经验性表明其效率足以实时协作（摘要）。（提取全文止于安全分析，实验定量数据未含。）

为 E2EE-CD 提供首个可组合的形式化基础：需求→理想功能→通用构造→实例化+证明链条完整，模块化使不同通道可即插即用，安全性直接继承。局限：假设用户间互信（记者场景合理但通用性受限）；Signal 仅支持 admin 权限且无回滚保护，SignalCD 继承之；泄漏未填充隐藏。
- **论文 PDF**: [End-to-End Encrypted Collaborative Documents](https://www.usenix.org/system/files/usenixsecurity26-knabenhans.pdf)

#### SophOMR: Improved Oblivious Message Retrieval from SIMD-Aware Homomorphic Compression
- **作者**: Keewoo Lee, Yongdong Yeo
- **方向**: 密码学与协议
- **解读**: 

**问题**：隐私保护区块链（Zcash、Aztec、Monero 等）和私密消息服务普遍采用"公共公告板"模式：发送者用接收者公钥加密消息并张贴在公告板上，接收者必须逐条扫描全部载荷才能不错过给自己的消息，连轻客户端也要扫描所有交易，体验极差。Oblivious Message Retrieval（OMR）用同态加密（HE）把这一昂贵扫描安全外包给服务商，但此前最先进的 PerfOMR（USENIX Security'24）仍存在明显瓶颈。

**方法**：作者识别出先前工作的瓶颈在"同态压缩"环节，给出新的 SIMD 友好同态压缩机制（基于 Cheon 等）：先对公告板信号做批处理同态检测得到加密的稀疏相关向量，压缩阶段利用 Newton 恒等式把索引压缩转化为 Vandermonde 矩阵-向量乘法，配合 PEGASUS 风格 BSGS 矩阵乘、矩阵堆叠（t+1 次 MatMul 合并为一次）、ring-switching、hybrid key-switching 等优化。相比 PerfOMR 需 2 层乘法深度和 O(N) 次旋转，SophOMR 仅需 1 层深度和 O(√N) 次旋转，且压缩确定性、无额外失败概率。

**结果**：在 65536 个 612 字节载荷、至多 50 条相关消息的场景下，运行时减少 3.4 倍、摘要小 2.2 倍、密钥小 1.5 倍；压缩环节比 PerfOMR 快 7.5 倍；单线程处理 2^16 个载荷不到 3 分钟（2.5 ms/载荷）。OMD 变体整体加速 4.6 倍、压缩环节加速 479 倍。对比在偏向 PerfOMR 的参数下进行（其仅 94 比特安全/18 比特完备性，SophOMR 为 128 比特/30 比特）。

**我的思考**：本文把"压缩是瓶颈"的洞察落地为可用的工程优化，并为多槽大载荷场景设计矩阵堆叠，属扎实的密码工程贡献。局限是客户端需预先知晓相关消息数量上界，且文本未见完整安全证明与实验细节。领域影响上，对 Zcash 隐身地址等场景有直接价值；与 HomeRun 等非 HE 方案相比，后者在更强信任假设下快约两个数量级，说明 OMR 路线仍有实用性权衡。

- **论文 PDF**: [SophOMR: Improved Oblivious Message Retrieval from SIMD-Aware Homomorphic Compression](https://www.usenix.org/system/files/usenixsecurity26-lee-keewoo.pdf)

#### FABS: Fast Attribute-Based Signatures
- **作者**: Liqun Chen, Mark Manulis
- **方向**: 密码学与协议
- **解读**: 

**问题**：属性基签名（ABS）允许用户凭属性集合签名、验证者确认其满足访问策略而不泄露其他信息，在匿名凭证、基于属性的访问控制、区块链智能合约等场景有广泛应用。然而支持 Monotone Span Programs（MSP）策略的配对基 ABS 方案普遍存在效率瓶颈：密钥生成或签名时间随属性数超线性增长、验证配对次数随属性数线性增长、依赖低效的 Type-II 配对，且此前没有任何 MSP 基 ABS 方案的公开实现。

**方法**：论文提出 KP-ABS 和 SP-ABS 两个方案，并引入统一两者的语法与安全模型（类似 ABE 泛化）。两方案基于 Type-III 配对，支持 MSP 策略、大宇宙、任意属性、自适应不可伪造性；关键设计是验证只需常数次（两次）配对运算，密钥生成、签名、验证时间均与属性数线性。安全性在随机预言机模型下基于 CBDH（KP）和 DXDH（SP）假设证明。

**结果**：100 个属性下，KP-ABS 密钥生成、签名、验证分别耗时 0.16s、0.10s、0.13s；SP-ABS 为 0.082s、0.26s、0.21s。对比最先进方案：KP 密钥生成比 RD16 快约 11 倍（1.74s），签名快 72 倍（7.2s），验证略快（0.13s vs 0.14s）；SP 签名比 KCGD14 快 43 倍（11.28s），验证快 294 倍（61.7s）。

**我的思考**：本文提供 MSP 基 ABS 领域首个公开实现，把"验证仅两次配对"做成实际方案，推动 ABS 从理论走向可用，贡献清晰。局限是安全证明依赖随机预言机模型（而非标准模型），且经典配对路线在量子威胁下仍需与格基等后量子方案混合过渡（作者也承认）。领域影响：为匿名凭证和属性访问控制等应用提供了具体性能基线，有助推动 ABS 的标准化与部署。

- **论文 PDF**: [FABS: Fast Attribute-Based Signatures](https://www.usenix.org/system/files/usenixsecurity26-chen-liqun.pdf)

#### Assumption-Free Fuzzy PSI via Predicate Encryption
- **作者**: Erik-Oliver Blass, Guevara Noubir
- **方向**: 密码学与协议
- **解读**: 

**问题**：私有集合交集（PSI）已被 Google、Meta、微软等大规模部署，通信复杂度是现实瓶颈。模糊 PSI（FPSI）放宽为"距离阈值内匹配"，用于指纹/生物识别比对、安全日志查询、GPS 与 DNA 变异分析等，但朴素方案通信随维度指数增长；现有高效方案要达到线性通信都依赖输入分布的强假设（最小间距、disjoint projections、可容忍假阳性率等），对任意分布尚无线性通信方案。

**方法**：核心洞察是把（阈值）Hamming 距离测试安全归约为内积测试：发送方把 xi 变换为 x′、接收方把 yj 变换为 y′，使 HD(x,y)=⟨x′,y′⟩，再对 τ∈{0,…,t} 迭代内积测试。内积测试用内积谓词加密实现：发送方加密每个 xi 后整体发送，接收方为每个 yj 获取解密密钥，离线完成全部配对测试，通信线性 O(ℓ(nS+t·nR))。为可组合性，论文证明这类谓词加密只需弱选择性安全（IND-WSS 蕴含 Sim-WSS，现成方案已满足）；无受信第三方时设计基于 OT 的两方分布式密钥派生。实现采用 Park 方案 C++ 移植（MCL、BN-254、Ferret-OT），并与 SEAL 的 FHE 基线对比。

**结果**：给出首个对任意输入分布达到线性通信的 Hamming 模糊 PSI；接收方计算为 O(ℓ·t·nS·nR) 的二次代价（无结构假设下配对测试的必要开销）。实现验证可行性，并优于最接近工作 Chakraborti 等（USENIX'23，仅提供带假阳性率的近似 FPSI）。

**我的思考**：思路漂亮：用谓词加密把模糊匹配精确地变成本地配对测试，弱安全定义打通理论与现成方案的缝隙，"假设自由"填补文献空白。局限：接收方二次计算使对称大规模集合不可扩展（定位于通信受限、非对称查询场景）；当前只支持 Hamming 距离（Lp 等为附录延伸）。领域影响：为无结构数据模糊匹配提供新的最优设计点与基准参照。

- **论文 PDF**: [Assumption-Free Fuzzy PSI via Predicate Encryption](https://www.usenix.org/system/files/usenixsecurity26-blass.pdf)

#### Logos: Robust Sharding Blockchain With Fast Processing and Optimal Cross-Shard Overhead
- **作者**: Yizhong Liu, Boyu Zhao, Yuan Lu, Song Bian
- **方向**: 密码学与协议
- **解读**: 

**问题**：分片区块链通过划分分片提升可扩展性，但随着分片数增加，跨片交易（CSTX）占比极高（16 片系统超 99%），其处理（CSTP）的原子性与开销成为系统安全和性能关键。现有方案内片开销达 2kB、跨片广播开销 O(n²bλ)；异步网络中恶意节点以海量无效 CSTX 洪泛可耗尽资源，跨片广播依赖昂贵长链路，成为部署瓶颈。

**方法**：Logos 提出"广播-传输-协商"模式：每个输入分片用新设计的 csdRBC 可靠广播确定输入可用性状态——提案附带区块高度对齐状态，且每节点对同一高度冲突交易只投一票以避免冲突——以低于 BFT 的成本 C 生成证书；随后用并行的单对单传输（纠删码分片+门限签名+Merkle 证明，每个节点只向单个接收者发一个分片，跨片长链路数从 O(n²) 降至 O(n)）可靠传递状态；有效交易经协商提交、无效丢弃，无需回滚。实现采用 Speeding Dumbo/Speeding MVBA、SHA-256、BLS 与 Reed-Solomon。

**结果**：4 个 AWS 区域的 1000 节点部署中，峰值吞吐 132.8 ktx/s（b=200k）；N≤1000、b≤10k 时延迟低于 3.43 秒。与基线 Kronos（NDSS'25）相比延迟仅为其 50%，跨片网络使用量仅为其 1/210；恶意洪泛攻击下保持 Kronos 2.86 倍的吞吐。理论上有效 CSTP 达最优内片开销 kB、无效开销 xC（C<B），跨片传输最优开销 O(bλ)。

**我的思考**：本文把跨片传输从 O(n²) 广播降为 O(n) 单对单并证明最优，"先确定可用性、后协商提交"避免无效交易的 BFT 回滚，鲁棒性定义清晰，实验规模大、对比公平，是异步分片区块链的重要工程推进。局限：csdRBC"每节点只投一个冲突交易"的约束对活性和公平性的长期影响值得关注；文本未含完整安全证明细节。领域影响：为分片与 Dumbo 异步 BFT 生态提供了可直接借鉴的 CSTP 模式。

- **论文 PDF**: [Logos: Robust Sharding Blockchain With Fast Processing and Optimal Cross-Shard Overhead](https://www.usenix.org/system/files/usenixsecurity26-liu-yizhong.pdf)

#### Streaming Function Secret Sharing and Its Applications
- **作者**: Xiangfu Song, Jianli Bai, Ye Dong, Yijian Liu, Tianwei Zhang
- **方向**: 密码学与协议
- **解读**: 函数秘密共享（FSS）是隐私保护测量系统（Mozilla、Apple/Google 部署，IETF 标准化中）的核心工具，但流式场景下每条新消息都要重新生成一对 FSS 密钥，密钥尺寸 O(λlogh) 具体开销很高，而函数本身往往未变，冗余严重。论文提出流式 FSS（SFSS）：把共享函数与数据流解耦，实现密钥复用与非交互式流式加密、求值。

SFSS 的 Gen 额外输出流式加密密钥 ke，Enc 用计数器 j 加密消息 mj 得流式密文 cj，Eval(b,kb,x,cj) 输出分享使 s0+s1=f(x)·mj。构造分三层：SDPF 将 DPF 输出处的伪随机分享作为 PRF 密钥做流式掩码/解掩码，仅多依赖一个 PRF；谓词函数用具备"特殊零密钥"性质的密钥同态 PRF（LWR 实例化）实现 B-有界正确性；通用函数分解为谓词形式。应用包括策略隐藏/属性隐藏聚合、窗口聚合（仅两次求值，原生 FSS 需线性于窗口大小）与条件转加密，并指出 Vizard 复用掩码导致安全缺陷。

实验中 SDPF 相对 DPF 近乎零开销（Z2^64 域密钥 1169 对 1173 KB，求值 1.8 对 1.7 ms），SDCF 因 KH-PRF 明显更贵（Z2^16 密钥 82,617 对 369 KB）。策略隐藏聚合中，100 条数据流、平均 1 万条消息时 DPF 基线客户端-服务器通信 569.34 MB，SDPF 方案仅 13.28 MB。C++ 实现已开源。

我的思考：这是首个对 FSS 流式化做系统形式化的工作，编译式构造（FSS→SFSS）干净，且借形式化发现并修复既有方案缺陷，与 IETF PPM 标准化的对接增强了现实意义。局限：谓词 KH-PRF 方案密钥与求值开销大，实用性主要限于单点/比较函数；安全模型为半诚实、双服务器不串谋。总体为流式隐私聚合提供了模块化构件，有望推动 PPM 走向高频流式场景。
- **论文 PDF**: [Streaming Function Secret Sharing and Its Applications](https://www.usenix.org/system/files/usenixsecurity26-song-xiangfu.pdf)

#### Shred-to-Shine Metamorphosis of (Distributed) Polynomial Commitments
- **作者**: Weihan Li, Ant Group, Zongyang Zhang, Sherman S. M. Chow, Yanpei Guo, Boyuan Gao, Xuyang Song, Yi Deng, Jianwei Liu
- **方向**: 密码学与协议
- **解读**: SNARK 依赖多项式承诺（PCS）。基于线性码的 MLPCS 证明快但证明大、验证慢，基于 Reed-Solomon 码的方案证明紧凑但证明时间因 FFT 为准线性；唯一的码基分布式方案 DeVirgo 不能切分单个多项式、缺可问责性且通信达数百 MB。论文问：能否兼得线性证明时间与紧凑证明？能否构造可问责、支持单多项式切分的分布式码基 PCS？

核心洞察"shred-to-shine"：把大小 N=mℓ 的多线性多项式切为 ℓ 个 size-m 子多项式，用张量积把一次求值校验降为 ℓ 个 size-m 检查，再以批量 PCS 摊薄共享求值点开销。PIPFRI 将 FFT 代价从 O(NlogN) 降至 O(Nlogm)、Merkle 哈希从 O(N) 降至 O(m)，用打包叶子单树规避多树开销，并获零知识；DEPIPFRI 利用 FRI 折叠局部性做分布式提交/开卷（子证明者 O(m/ℓ)），支持单多项式切分并首次为码基方案加可问责机制；PIPKZG 在配对体制下 SRS 缩短 16 倍。

实验上 PIPFRI 比 DeepFold 证明快 10×，比 Orion 快 3.5× 且证明大小与验证时间降 15×，内存比 DeepFold 少 10×、比 Orion 少 5×，首个在 32 GB RAM 上支持 size-2^29 多项式的方案（FFT 620→315 ms、Merkle 2258→468 ms，size-2^20）。DEPIPFRI 比 DeVirgo 证明快 25×、通信降 7×，其分布式 SNARK 比 HyperPianist 证明快 8×、验证快 5×。

我的思考：PIP 把"切分-批处理-重组"抽象为可复用元方法，同时改进码基与群基两类 PCS，首次给出支持单多项式切分且可问责的码基分布式方案，填补分布式 SNARK 关键缺口，理论与工程贡献扎实。局限：DEPIPFRI 假设子证明者诚实（与同类一致），可问责性的对抗保障需细检；对比基线有限。总体可能重塑 zk-SNARK 证明器选型，对可验证机器学习、跨链桥等大电路场景有直接价值。
- **论文 PDF**: [Shred-to-Shine Metamorphosis of (Distributed) Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-li-weihan.pdf)

#### Unbalanced Fuzzy Private Set Intersection for L_infinity Distance: Achieving Sublinear Communication with Large Set Size
- **作者**: Shengzhe Meng and Xiaodong Wang, Xv Zhou, Bei Liang
- **方向**: 密码学与协议
- **解读**: 模糊 PSI 在近似匹配下计算集合交集，用于生物识别、泄露密码检测、地理围栏等，但现有高效协议几乎都面向双方集合相近的平衡情形。实际中集合常高度不平衡（接收方查数百个密码、发送方持百万级泄露密码库），现有协议通信随较大集合线性增长。本文针对 L∞ 距离，目标是对大集合次线性、对小集合线性的通信。

关键观察：现有协议通信由 OKVS 传输主导，OKVS 稀疏时可用批量 PIR 让小集合方只取所需解码值，但这要求空间哈希满足 H2 输出单元素（H2-unique）等性质。论文重构空间哈希定义并提出两个方案：非交互方案（输入集 T 分离、T>2σ，网格划分，h2=1，约束从 4σ 放宽到 2σ）；交互方案（h1=h2=1，基于维度分离与新的 Private Value Sum 功能，AHE/OPRF 实例化，通信 O(n2·d·³√(n1σ)) 次线性）。据此构造接收方大集合的 fuzzy PSI(-CA) 与发送方大集合的 PSI-SP。

参数 (N,M,d,σ)=(2^20,2^5,2,10)、100 Mbps 下，非交互方案把通信从 Baarsen & Pu（Eurocrypt'24）的 16,128 MB 降至 0.35 MB；交互方案比 Gao et al.（Asiacrypt'25）在线运行快 ≥31×、通信低 1762×；PSI-SP 比 Piske et al.（CCS'25）在线运行快 ≥4×、通信低 6×（实验中提速 6.18–101.04×、通信降 4.43–284.34×）。

我的思考：这是首个对 L∞ 模糊 PSI 实现大集合次线性通信的工作，把空间哈希抽象为带 H2-unique 性质的功能并给出两个构造，理论清晰、性能提升数量级显著。局限：两方案都要求至少一方输入满足分离/间距条件（可经预处理缓解），无法直接处理任意输入；BatchPIR 带来额外计算与部署开销。与不平衡电路 PSI/PSU 的 PIR 优化一脉相承，对生物识别、泄露密码检测等应用价值明确。
- **论文 PDF**: [Unbalanced Fuzzy Private Set Intersection for L_infinity Distance: Achieving Sublinear Communication with Large Set Size](https://www.usenix.org/system/files/usenixsecurity26-meng.pdf)

#### mmCipher: Batching Post-Quantum Public Key Encryption Made Bandwidth-Optimal
- **作者**: Hongxiao Wang, Ron Steinfeld, Markku-Juhani O. Saarinen, Muhammed F. Esgin, Siu-Ming Yiu
- **方向**: 密码学与协议
- **解读**: 多消息多接收者公钥加密（mmPKE）一次加密即可把多条消息分别发给多个接收者，显著省带宽；后量子设置中密文远大于明文，收益更大，应用包括 MLS/SGM 群消息、蓝牙网状广播、Zether 类匿名机密交易。但几乎所有 mmPKE 都基于可被量子攻破的传统假设，唯一后量子方案 [6] 只能批量连续相同消息、泄露消息向量结构、达不到完全 CPA。论文回答 Bellare 等二十余年的开放问题：能否在标准后量子假设下构造完全批量、完全 CPA 的 mmPKE。

方法分两步：其一，提出扩展可复现 PKE（XR-PKE），在复现算法中引入 hint 并显式建模 hint 泄露的语义安全，得到 mmPKE 通用构造；其二，用解密误差作为 hint 实例化格基 XR-PKE，以 Matrix Hint-MLWE 假设论证 hint 安全影响可忽略并定参，还推广矩阵到非方阵、补全 [26] 归约缺失的可采样条件。额外把 Naor-Yung 范式推广到 mmPKE 得到抗自适应腐败与 CCA 的构造，并用多证明可提取 NIZK 编译器去除 KOSK 假设。

1024 个接收者、128/192/256 位安全级下，mmCipher-KEM 带宽较 Kyber 平凡方案降 23–45×，密文仅比明文大 4–9%（接近最优带宽，约 3 KB），计算成本降 3–5×；mmCipher-PKE 带宽降 12–23×。提供 C 实现与完整基准。

我的思考：首个基于标准 MLWE、达到完全 CPA（同时保护消息语义与结构）的 mmPKE/mmKEM，终结二十余年开放问题，解密误差作 hint 的思路巧妙且有独立价值。局限：依赖 Matrix Hint-MLWE（有归约但与纯标准假设尚有距离）与 KOSK 假设（去 KOSK 仅限多项式接收者）；实验规模有限。对 MLS、机密区块链交易等场景，解除了后量子迁移的主要带宽瓶颈。
- **论文 PDF**: [mmCipher: Batching Post-Quantum Public Key Encryption Made Bandwidth-Optimal](https://www.usenix.org/system/files/usenixsecurity26-wang-hongxiao.pdf)

#### Hydrangea: Optimistic Two-Round Partial Synchrony with Improved Fault Resilience
- **作者**: Nibesh Shrestha, Aniket Kate, Kartik Nayak
- **方向**: 密码学与协议
- **解读**: 部分同步共识存在根本权衡：最优拜占庭容错（f<n/3）需 ≥3 轮好情况延迟，而少于三轮提交（SBFT、FaB、Kudzu）通常降低容错（Kudzu 在 f=p<n/5 时才快速提交）。现实中拜占庭故障罕见、崩溃占绝大多数（Solana 已转向混合模型）。论文问：能否在容忍拜占庭与崩溃两类故障的高容错下同时实现乐观两轮提交？

Hydrangea 面向 n=3f+2c+k+1 的系统（f 拜占庭、c 崩溃、k→0 可调）：故障总数 ≤p=⌈(c+k)/2⌉ 时乐观两轮提交；否则仍三轮好情况延迟且同时容忍 f 拜占庭与 c 崩溃。关键技术：混合模型比纯拜占庭弱，允许 c+k→1 时用小于 2n/3 的法定人数；视图切换时向新领导者传递上一视图投票信息，结合法定人数相交分析保证跨视图安全。还证明匹配下界：故障数超过 p=⌈(c+k+2)/2⌉ 时任何协议都无法实现乐观两轮（严格推广 Kuznetsov 等的结果）；上下界仅差一个故障，作者稍降乐观容错换取更简单设计。

实验在 100 节点地理分布式部署：无故障时比 Jolteon↘ 延迟低约 20%、比混合模型 ByzCrash 低约 10%，吞吐略高；故障场景（乐观提交不触发）与 ByzCrash 相同（同为三轮路径），仍比 Jolteon↘ 低约 20%。示例 f=10%、c=34% 时，83% 诚实即可两轮提交，同时容忍总计 44% 故障，此组合此前方案无法达到。

我的思考：在混合故障模型下首次同时获得乐观两轮延迟与高容错，上下界几乎匹配，理论贡献清晰，切合"崩溃为主、拜占庭罕见"的生产现实。局限：乐观容错 p=⌈(c+k)/2⌉ 相对纯拜占庭场景仍有限；与最优下界差一个故障；实验基线少、未见大规模拜占庭故障注入压力测试。对低延迟区块链与共识系统有实践价值，也为混合故障模型下的共识设计提供新理论基准。
- **论文 PDF**: [Hydrangea: Optimistic Two-Round Partial Synchrony with Improved Fault Resilience](https://www.usenix.org/system/files/usenixsecurity26-shrestha.pdf)

#### Garuda and Pari: Faster and Smaller SNARKs via Equifficient Polynomial Commitments

- **作者**: Michel Dellepere, Pratyush Mishra and Alireza Shirzad

- **方向**: 密码学与协议

- **解读**: SNARK 研究的两大目标是极小化证明大小与证明生成时间。此前最优的 Groth16（192 字节）与 Polymath（176 字节）已相当紧凑，但"自定义门+免费线性门"能否兼得仍是开放问题。本文提出两个新 SNARK：Pari 与 Garuda，分别推进这两条前沿。

方法上，两项构造共享新原语"equifficient"多项式承诺（EPC），强制一批多项式在各自基下系数表示相同，从而在承诺层直接实施线性约束，取代 PIOP 中复杂的线性检查。作者给出 EPC 严格安全定义，并从任意 Pedersen 型配对承诺（KZG、PST）泛化构造，还支持批量承诺（承诺大小与多项式数无关，是 Pari 小证明的关键）。Pari 针对 square R1CS 达到 2 个群元素+2 个域元素（BLS12-381 下 160 字节/1280 比特），为已知最小；Garuda 提出支持任意自定义门的 GR1CS，证明者仅 O(n) 域运算+O(n) 群运算，证明与验证 O(log n)。

基于 arkworks 的评测：同一 Rescue-Prime 哈希基准下，Garuda 证明比 Groth16 快近 5 倍、比 HyperPlonk 快 2.67 倍；Pari 的 Solidity 验证器链上 gas 较 Groth16 低 6%、较 FFLONK 低 17%，为已知最低。两者均在代数群模型下证明安全，但需电路专属可信设置。

我的思考：本文把证明大小下界又推进一步，且"EPC 承接线性约束"的编译方法论简洁可复用于其他 PIOP+PC 组合；免费线性门对线性门占比高的现实电路收益显著。局限是电路专属可信设置限制通用性、AGM 假设弱于标准模型、square R1CS 限制 Pari 的应用范围。与 Linear PCP 老路线相比，本文以新抽象换取更优常数，EPC 原语本身也有独立应用前景。

- **论文 PDF**: [Garuda and Pari: Faster and Smaller SNARKs via Equifficient Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-dellepere.pdf)

#### Transparent Dictionaries from Polynomial Commitments

- **作者**: Hossein Hafezi, Alireza Shirzad, Benedikt Bünz and Joseph Bonneau

- **方向**: 密码学与协议

- **解读**: 透明字典是密钥透明度系统的核心原语：不可信服务器维护可变字典并周期性发布承诺，客户端需验证查询与一致性（防"振荡攻击"）。WhatsApp 2024 年已部署密钥透明度日志，但既有方案难以规模化：Merkle 树审计成本随更新线性增长，SNARK 委托方案吞吐极低（Hekaton 仅 10 更新/秒），RSA 累加器查询证明随规模线性增长。本文提出 IRONDICT，基于多项式承诺的透明字典。

方法上，IRONDICT 黑盒使用通用多线性多项式承诺方案（PCS），继承其绑定与零知识性质，使审计与更新成本独立于批次大小；一致性证明无需版本历史，仅两次 PCS 打开；基于 IVC 的 fast-forwarding 让离线审计者验证亚线性个证明，另有轻量 checkpoint 供客户端自审计。作者还形式化证明"同态 PCS 编译为零知识打开"编译器的安全性，并提出更强的零知识隐私定义（zk-IRONDICT）。

以 KZH 实例化：10 亿条目字典在消费级笔记本 35 毫秒完成审计验证，比现有最优快 300 倍，证明仅 8 KB（小 15 万倍）；服务器吞吐超每秒 1000 次更新、每轮 epoch 约 15 分钟；客户端查询证明约 6 KB、验证小于 30 毫秒。局限是 KZH 需线性规模结构化参考串。

我的思考：本文把透明字典从"审计不可行"推进到数十亿用户可直接部署的工程现实；审计成本独立于更新批次是相对 Merkle 路线的本质优势。局限：线性 SRS 带来设置负担，小规模场景下 Merkle 树的简单性仍具吸引力。总体确立了"PCS 即字典"的新范式，对消息加密、证书透明度等场景均有直接应用前景。

- **论文 PDF**: [Transparent Dictionaries from Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-hafezi.pdf)

#### Efficient Threshold ML-DSA

- **作者**: Sofía Celi, Rafael del Pino and Thomas Espitau, Guilhem Niot, Thomas Prest

- **方向**: 密码学与协议

- **解读**: 后量子迁移中组织纷纷采用 NIST 标准化的 ML-DSA，但缺乏可用阈值版本——经典 RSA/Schnorr/ECDSA 阈值方案无法迁移，直接阈值化 ML-DSA 的难点在其 Fiat-Shamir-with-aborts 范式：多方需在不泄露候选签名的情况下协作决定是否中止。本文给出首个与 ML-DSA 完全兼容的实用阈值签名方案，支持最多 6 方（每方平均通信上界 1 MB，ML-DSA-44）。

方法上，方案组合短秘密共享与针对 ML-DSA 优化的逐方拒绝采样：提出非均衡超球拒绝采样（联合不同范数微调，降低中止概率）；将份额重构建模为 B-匹配变体并用最大流求解；并行多实例保证重构成功率。两阶段拒绝采样（逐方+全局）保证形态安全与标准尺寸兼容；密钥生成支持后验共享与无可信分发者的分布式 DKG。

安全性在标准 MLWE 假设、恶意多数模型下形式化证明（静态安全，N≤6 时可推出自适应安全）。Go 实现：本地签署毫秒级，跨大洲 WAN 小于 1 秒；每方平均通信 21 KB 至 1050 KB；输出签名与标准 ML-DSA 完全兼容。相比 Trilithium（需可信第三方生成相关随机性）等方案，本方案在恶意多数下仅 2+4 轮。

我的思考：本文解决了"量子安全与操作弹性不可兼得"的真实痛点，是首个把 NIST 标准签名直接阈值化的方案，兼容性使其可无缝替换现有系统；技术亮点是从协议层面化解拒绝采样的泄露问题。局限：6 方上限偏小、静态安全弱于自适应。它为多设备钱包、阈值 TLS、Tor 目录权威等提供了可直接部署的后量子阈值基础设施。

- **论文 PDF**: [Efficient Threshold ML-DSA](https://www.usenix.org/system/files/usenixsecurity26-celi.pdf)

#### PICS: Private Intersection over Committed (and reusable) Sets
- **作者**: Aarushi Goel, Peihan Miao and Phuoc Van Long Pham, Satvinder Singh
- **方向**: 密码学与协议
- **解读**: 恶意安全的 PSI 允许敌手在多次会话间使用不一致输入：例如 Apple 的 CSAM 检测可对不同用户注入不同图片集、密码泄露检测服务可对用户注入未泄露密码，造成歧视或窃取额外信息。这是 MPC 恶意安全定义的固有局限，但在 PSI 中尤为致命，因为服务端常与海量用户重复执行且 PSI 便于推断额外信息。

方法上，PICS 分承诺与求交两阶段：双方先用简洁承诺绑定输入集，再在承诺集上执行恶意安全 PSI。关键观察是恶意安全 PSI 成功完成即隐式绑定输入，无需逐条证明：接收方在承诺阶段用 FRI 多项式承诺其 OKVS 编码（加随机填充防泄露），求交阶段利用 VOLE 线性关系做单点 Schwartz-Zippel 校验；发送方用 Merkle 承诺 H′(xi‖ri)，通过去随机化技巧在输出上验证元素属于承诺集。全程黑盒使用密码学，不改变底层 VOLE-PSI。

实现表明：接收方达到强一致性（必须使用完全承诺的集合），发送方为弱一致性（不能注入新元素但可用其子集）；承诺仅 32 字节；与 VOLE-PSI 相比，2^16–2^24 集合规模下通信开销仅 1.57–2.04×，端到端运行时间 1.22–1.98×；按加州人口 4000 万估算，删除法案场景双方计算集合承诺不到 5 分钟。

我的思考：把“跨会话输入一致性”从 MPC 定义盲区提升为一等安全目标，且以极小开销实现，理论观察（隐式承诺足够）与工程细节（FRI+随机填充）都相当精巧，为 CSAM 检测、通讯录发现等 PSI 应用提供了可审计的信任锚。局限：发送方强一致性是明确的开放问题，接收方需预先设定求交次数上限（可扩展但有成本）。相比 Authorized PSI 等部分方案，PICS 首次给出通用的可复用承诺 PSI，实用价值突出。

- **论文 PDF**: [PICS: Private Intersection over Committed (and reusable) Sets](https://www.usenix.org/system/files/usenixsecurity26-goel.pdf)

#### Anonymous Tokens with Designated-Reader Metadata Bit
- **作者**: Aisha Tu, Meng Jia, Kun He
- **方向**: 密码学与协议
- **解读**: 匿名令牌的私密元数据位（PMBT）向验证方传递隐藏信号，但现有方案（PMBT、ATHM、Benhamouda 等）只允许签发者读取：签发者需保持在线响应读取请求，易成瓶颈，且验证方必须联系签发者，使 issuer-hiding 难以实现。本文提出指定读者（designated-reader）元数据位：用户指定一个签发者认可的验证方直接读取信号。

方法上，核心是设计可控可延展、明文合法性易证的加密：采用类似 ElGamal 的密文 (h, g^r, y^r·h^s)（h 由用户选以保不可链接），借助 SPS-EQ 的等价类再随机化使令牌与签名整体随机化，用基于 Σ 协议的 NIZK（OR 证明）证明明文确为 0 或 1；扩展方案 HinToken-R 用盲化公钥 + 认证读者集合实现 reader-hiding（签发者不知目标读者），HinToken-I 用 mercurial 签名的密钥/签名联合随机化实现 issuer-hiding（验证方不知签发者）。

令牌为常量大小；在通用群模型（GGM）下形式化并证明 one-more unforgeability、不可链接性、元数据位隐私及读者/签发者匿名性（定理 1–3 等），并报告实现性能；可支撑机器人防护、资金证明、CDN 授权等应用。

首次把"指定读者"引入私密元数据令牌，用 SPS-EQ+盲化+NIZK 的组合同时化解签发者在线与 issuer-hiding 两大矛盾，思路优雅；局限是 reader/issuer-hiding 需假设不与全部读者/签发者共谋，配对与 NIZK 开销高于 PMBT/ATHM 类方案，实际部署成本与标准化的兼容性仍需关注。

- **论文 PDF**: [Anonymous Tokens with Designated-Reader Metadata Bit](https://www.usenix.org/system/files/usenixsecurity26-tu.pdf)

#### Bridging Usability and Performance: A Tensor Compiler for Autovectorizing Homomorphic Encryption
- **作者**: Edward Chen
- **方向**: 密码学与协议
- **解读**: 同态加密（HE）张量程序性能高度依赖明文打包进密文的布局，而真实张量程序有海量布局可能，手工设计既繁琐又不可泛化（Bolt 指出 Gazelle 的打包无法推广到 transformer，朴素推广多 128× 乘法、2000× 旋转）；布局自动搜索又陷入编译时间与执行质量的两难。

方法上，Rotom 编译器三管齐下：一是布局对齐（layout alignment）规则，标准化张量算子到 HE 内核的生成并提供快速成本预言，避免数组物化求代价；二是新算子 ApplyRoll，用滚动（roll）+掩码的廉价结构完成布局转换（规避 ApplyPermute 的 rotate-and-sum 复制步骤，常见场景快 3–8×），其规整的"定步长旋转+等间隔掩码"结构自动匹配 baby-step giant-step（BSGS）优化，并可自动应用 Strassen 算法减少密文乘法；三是 4 个基于成本的剪枝启发式，把候选布局从数十万降至数百。

约 12,000 行 Python，面向 OpenFHE 的 CKKS；全部 6 个张量基准在 5 分钟内编译完成（多数仅数秒）；手调协议旋转数最多降 3×；较先前的自动向量化系统（Fhelipe、Viaduct-HE）最多快 80×；微基准中 ApplyRoll 比 ApplyPermute 快 5.13×/8.43×，配合 BSGS 的 MatMul 快 30.05×。

把布局搜索从"约束布局=快但次优"推进到"复杂布局+快速搜索"兼得，ApplyRoll 的设计（低成本转换+解锁 BSGS/Strassen）是点睛之笔；局限是单后端（CKKS/OpenFHE）、剪枝启发式可能牺牲全局最优、未覆盖 bootstrapping 放置（作者声明与 Fhelipe 互补）；对隐私计算编译器与安全推理实践均有推动。

- **论文 PDF**: [Bridging Usability and Performance: A Tensor Compiler for Autovectorizing Homomorphic Encryption](https://www.usenix.org/system/files/usenixsecurity26-chen-edward.pdf)

#### Shadowfax: Hybrid Security and Deniability for AKEMs
- **作者**: Phillip Gajland, Vincent Hwang, Jonas Janneck
- **方向**: 密码学与协议
- **解读**: 协议向后量子迁移普遍采用经典+后量子混合，但常牺牲效率、紧凑性甚至安全属性：PQXDH 因对临时密钥签名而失去 X3DH 的可否认性，苹果 iMessage PQ3 明确不设可否认目标；而此前没有任何混合 AKEM 能同时保持可否认性。本文研究如何在混合（假设级组合）框架下高效保住可否认性。

方法上给出两个构造：一是黑盒组合器 (c1,c2)+H(k1,k2,pks,pkr,c)——保密与认证只需两个分量 AKEM 之一安全，但可否认性被证明要求两个分量均可否认（模拟器无法为不可否认分量构造可区分密文）；二是非黑盒的 SHADOWFAX：用经典 NIKE（Curve25519）+ 后量子 KEM（ML-KEM/BAT）+ 后量子环签名（GANDALF/FALCON）+ 对称加密，KEM 密文用"签名者+接收者"环签名以保证信息论级环匿名（不诚实接收者可否认性达统计安全），再以 NIKE 与 KEM 派生的密钥对称加密该签名以对抗公开可验证性，使诚实接收者可否认性只需 NIKE 或 KEM 单一计算假设。

以标准化组件 ML-KEM-512 + FALCON-512 实例化，摘要报告密文 1,728 字节、公钥 2,036 字节，Apple M1 Pro 上封装/解封装各约 1.8M/0.7M 周期；提供多套可移植 C 实现（另有 BAT 紧凑实例化），并给出完整安全归约（定理 2–10，含 insider-CCA 保密与 Out-Aut 认证）。

首个保持可否认性的混合 AKEM，明确区分"安全黑盒组合"与"假设级混合"的论证框架，诚实接收者可否认性仅依赖单一假设是亮点；局限是黑盒组合器的可否认性无法"混合"（两分量都须可否认）、环签名与 NIKE 使密钥/密文增大、诚实接收者场景需 KEM/NIKE 至少一个可信；对 Signal、MLS 等即时通信的后量子迁移有直接参考价值。

- **论文 PDF**: [Shadowfax: Hybrid Security and Deniability for AKEMs](https://www.usenix.org/system/files/usenixsecurity26-gajland.pdf)

#### RBOOT: Accelerating Homomorphic Neural Network Inference by Fusing ReLU within Bootstrapping
- **作者**: Zhaomin Yang
- **方向**: 密码学与协议
- **解读**: 全同态加密（FHE）安全推理中 ReLU 等非线性激活是主要瓶颈：SOTA 用高次多项式近似（如 Lee et al.），每个 ReLU 消耗 14–20 层乘法深度，开销巨大；而 BFV/CKKS 的功能 bootstrapping 只能支持小位宽查表函数，精度受限。本文提出把 ReLU 直接融入 CKKS 引导过程以同时降深度、保精度。

方法上，关键洞察是 CKKS bootstrapping 的 EvalMod 步骤本身由 sin/arcsin 三角函数构成、天然非线性，既往工作把引导与激活当作独立例程浪费了这一点；将 ReLU(x)=x/2+|x|/2 分解为奇函数与偶函数，分别与 arcsin（及几乎零成本的 cos）复合，用改进 Remez 算法求 minimax 多项式（度 127 达 14 位精度），使引导过程直接输出 ReLU，仅需在 [-q0/4, q0/4] 区间上保证精度即可。

每个 ReLU 的深度消耗从 14–20 降至 8 层（降幅超 40%）；在 4 个常用 CNN 模型（CIFAR 数据集）上端到端推理较 Lee et al. 的多项式近似快 2.77×，内存占用降低 81%，top-1 精度损失小于 0.1%；附带贡献是支持多特殊素数密钥切换的优化版 SEAL 库。

"两合一"把非线性激活并入 bootstrapping 是优雅的密码学级协同优化，且路径可推广到一般连续实值函数（即泛化的 CKKS 功能引导）；局限是要求消息幅度保证在 [-q0/4, q0/4] 内（需 scale 与区间管理）、minimax 多项式求解复杂、对比基线为较早工作；对同态加密隐私推理的实际部署价值显著。

- **论文 PDF**: [RBOOT: Accelerating Homomorphic Neural Network Inference by Fusing ReLU within Bootstrapping](https://www.usenix.org/system/files/usenixsecurity26-yang-zhaomin.pdf)

#### Leveraging Cryptographic Simulator Synthesis for Formally Verifying the FOO E-Voting Protocol
- **作者**: David Baelde, Adrien Koutsos and Justine Sauvage
- **方向**: 密码学与协议
- **解读**: 密码学证明大量依赖向假设博弈（game）的归约，归约需要手工编写模拟器，繁琐易错且在手写证明中常被略写；机械化证明虽消除笔误，但要求用户显式写模拟器负担过重。Squirrel 中模拟以 bi-deduction 表达，前人的 crypto 策略在 IND-CCA2 等常见博弈上会系统性失败。

作者改进了 bi-deduction 模拟器合成：关键缺口在于模拟器无法跨递归迭代记忆（memoize）预言机调用结果，也无法推断证明复杂模拟器正确性所需的时间敏感不变量。新流程分三遍完成归纳式合成——先推断记忆化不变量、再推断随迭代变化的记忆（memory）不变量、最后生成需用户证明的约束与概率耦合条件，且保证终止；实现为增强版 Squirrel crypto 策略（记忆化默认开启，时间敏感不变量经 ~time_sensitive 开关启用）。

以 FOO 电子投票协议为验证场景，完成了首个 FOO 投票隐私的计算性机械化证明，并推广到任意数量选民；这是迄今最复杂的 Squirrel 证明，依赖假设类型多样、代码量最大，成果已合入 Squirrel 主线。基础评测中 4 个博弈上的 13 次 crypto 调用平均约 0.1 秒。

我的思考：模拟器可记忆化与时间敏感不变量是支撑 CCA2 类博弈的关键能力，抽象混合网的例子清晰展示了"复用投票人加密结果"为何必不可少；局限在于不追求完备性、仅支持记忆为日志集合且日志间无复杂流动的博弈（Assumption 1）、自适应且会话数随安全参数多项式增长的情形仍超出范围。作者也谨慎声明 FOO 仅作技术验证而非部署建议，这种把形式化工具演进与大规模案例绑定的做法值得肯定。

- **论文 PDF**: [Leveraging Cryptographic Simulator Synthesis for Formally Verifying the FOO E-Voting Protocol](https://www.usenix.org/system/files/usenixsecurity26-baelde.pdf)

#### Bridging Bitcoin to Second Layers via BitVM2

- **作者**: Robin Linus Woll; Lukas Aumayr; Zeta Avarikioti; Matteo Maffei; Andrea Pelosi; Orfeas Stefanos Thyfronitis Litos; Christos Stefo; David Tse; Alexei Zamyatin
- **方向**: 密码学与协议
- **解读**: 比特币吞吐仅约5-7笔/秒且脚本表达能力受限，难以支持DeFi等应用，跨链桥是把比特币资产安全转移到二层（L2）的关键基础设施。现有桥多依赖t-of-n多签委员会与诚实多数假设，若签名者串通或受贿，用户存款即告损失。本文提出首个基于轻客户端的比特币桥BITVM2-BRIDGE，向"无信任桥"这一圣杯迈出实质一步。

核心组件BITVM2-CORE把任意程序编译为SNARK证明，将验证器拆分为k个可在单笔交易内执行的小程序，通过Taproot多叶结构、签名委员会（CheckCovenant，签名后即删除密钥）与Lamport签名绑定中间状态，实现乐观验证+欺诈证明；再以之构建链上比特币轻客户端，验证候选区块属于有效PoW链，运营商垫付资金、用户燃烧包装资产并出示证明完成peg-out。

信任假设从诚实多数（t-of-n）降至存在性诚实（1-of-n）：每类角色只需一个诚实方即可保证安全，活跃性仅需一名理性运营商，任意用户可作挑战者。主网实验显示：最坏情况（非快乐路径）争议在8小时内、费用低于0.15 BTC结算，90%的Disprove脚本小于3MB；快乐路径交易低于16.75 vkB（约5万聪，约53美元），且可进一步降至534字节（约1.6k聪，约1.66美元）。

工程完成度与产业影响是本文最大亮点：BOB、Babylon、Citrea、Alpen、Bitlayer、Fiamma等创业项目均基于此构建。相对前作BitVM（争议需79笔顺序链上交易、约0.007 BTC），BITVM2所需交易更少但单笔更大。局限在于签名委员会仍是信任锚（依赖存在性诚实假设），静态难度轻客户端难以处理难度调整，未来真正的covenant可进一步移除签名者依赖。

- **论文 PDF**: [Bridging Bitcoin to Second Layers via BitVM2](https://www.usenix.org/system/files/usenixsecurity26-woll.pdf)

#### DDR-SSE: Duplicated Retrieval of Documents for System-wide Secure Searchable Symmetric Encryption

- **作者**: Zichen Gui; Simon-Philipp Merz and Kenneth G. Paterson; Sikhar Patranabis
- **方向**: 密码学与协议
- **解读**: 可搜索对称加密（SSE）允许在密文上做关键词搜索，但会泄露访问模式等信息，可被查询重建等泄露滥用攻击利用。多数SSE只考虑索引检索泄露，而系统级安全要求同时抵抗索引与文档检索的联合泄露——此前唯一高效的系统级安全方案SWiSSSE需要每查询更新的客户端状态，且泄露难以精确刻画。本文提出DDR-SSE，用静态客户端状态实现系统级安全。

DDR-SSE模块化组合两种机制：体积隐藏的索引检索（实例化为XorMM）+全新的文档检索机制——每个文档仅存储两份，通过精心设计的伪随机文档访问抑制共现泄露；文档经分桶（bucketization）处理以应对变长与最坏情况填充。作者给出严格的模拟安全证明，刻画了简洁的泄露函数。

证明共现计数从原始C[i,j]均匀化到[0,C[i,j]]区间，足以挫败既有泄露攻击；在Enron语料上即使面对"不现实"的强攻击假设也抵抗查询重建攻击。基准：40万封邮件（4KB分块，共1.6GB明文，3.2万关键词，2,900万键值对）加密后共4.4GB，建立吞吐900文档/秒、搜索39,000文档/秒；对比朴素方案的存储4×-800×、带宽20×-110,000×开销，DDR-SSE保持实用性。

概念简洁性是相对SWiSSSE的最大优势——仅存两份文档而非逐关键词复制，且客户端状态静态，符合主流SSE设计范式；共现均匀化的理论论证扎实。局限：仅支持静态数据库，动态扩展与两服务器方案尚属草图/需不合谋假设；文档检索的随机化依赖密钥固定，仍需关注新式统计攻击的长期稳健性。

- **论文 PDF**: [DDR-SSE: Duplicated Retrieval of Documents for System-wide Secure Searchable Symmetric Encryption](https://www.usenix.org/system/files/usenixsecurity26-gui.pdf)

#### Concretely Efficient Blind Signatures Based on VOLE-in-the-Head Proofs and the MAYO Trapdoor

- **作者**: Carsten Baum and Marvin Beckmann; Ward Beullens; Shibam Mukherjee; Christian Rechberger
- **方向**: 密码学与协议
- **解读**: 盲签名是匿名凭证、电子现金等隐私应用的基石，后量子时代的主流路线是格基构造，但最先进方案每次出示仍需至少20KB通信、超过100ms证明时间，难以实用。本文提出非格路线的PoMFRIT：将VOLE-in-the-Head零知识证明系统与MAYO多变量签名陷门结合，遵循Fischlin通用范式并大幅优化。

技术核心有三：其一，利用VOLEitH可高效证明高次约束的性质优化Keccak-f[1600]电路——每6轮提交一次状态并以度16多项式约束验证一致性，证明体积缩小约4倍（75%）；其二，利用VOLEitH两阶段结构把承诺改为c=r1⊕H(μ,π1)，将哈希移出ZK证明；其三，无哈希MAYO（直接以承诺为预像）消除签名内部哈希，并实现首个SHA-3族的VOLEitH证明（独立价值）。

量化结果显著优于格基方案：签发通信仅0.45KB，盲签名大小6.7KB，出示时间小于76ms（保守128位安全）；RainHash（7轮、512位状态）每哈希调用仅448字节非线性通信，对比未优化SHAKE256的4800字节与优化后的800字节；实现覆盖NIST L1/L3/L5的small/fast变体。

该工作首次让后量子盲签名在通信与计算上全面超越格基状态，且无格假设。但需注意代价：RainHash是专为MPCitH/VOLEitH设计的新哈希，其7轮保守参数基于推测性安全分析；无哈希MAYO依赖one-more preimage resistance这一新猜想；相对Fischlin通用构造的轮次与假设仍需时间检验。对匿名凭证落地是重要推动。

- **论文 PDF**: [Concretely Efficient Blind Signatures Based on VOLE-in-the-Head Proofs and the MAYO Trapdoor](https://www.usenix.org/system/files/usenixsecurity26-baum.pdf)

#### Scribe: Low-memory SNARKs via Read-Write Streaming
- **作者**: Anubhav Baweja
- **方向**: 密码学与协议
- **解读**: SNARK 证明者生成大电路证明需要大量时间与内存：HyperPlonk 证明"10kB 数据正确哈希 SHA256"就需超过 16GB RAM，业界基准普遍依赖服务器级 GPU 与数百 GB 内存。现有降低内存的方案（流式 SNARK、递归组合、复杂度保持 SNARK）都以牺牲渐近或具体证明时间为代价，作者认为问题不在状态大小，而在状态存放位置。

方法上，作者提出"读-写流（read-write streaming）"计算模型：证明者只持有 O(log N) 随机访问内存，把大中间状态顺序存到磁盘流中，靠可预测、数据无关的访问模式触发预取、流水线与缓存来掩盖 I/O 开销。以此改造"Polynomial IOP + 多项式承诺"范式：将 HyperPlonk 的 sumcheck、zerocheck、prodcheck、permcheck 等 PIOP 及 PST 等多类承诺方案全部流式化，得到 Scribe，保持 O(N) 密码学线性时间，随机访问空间从 O(N) 降到 O(log N)。

评估显示，服务器级机器上证明 2^28 门电路耗时 1.5 小时、内存低于 750MB，相对 HyperPlonk 仅增加约 10% 延迟；智能手机上可证明 2^24 门电路，比内存密集基线大 32 倍，据称是迄今手机端证明的最大电路；相比同场景先前低内存方案（Gemini）延迟改进近一个数量级。实现基于 arkworks，含批提交、流水线等 I/O 优化。

我的思考：这是把"时间-空间权衡"巧妙转化为"RAM-磁盘权衡"的工作，模型形式化（复杂度合成引理、管道化组合）与工程优化结合扎实，论文自身也强调"从非流式算法派生的简单性"是模型表达力的体现。局限在于文本抽取缺失评估章节细节、磁盘 I/O 依赖 SSD 才现实、安全性继承 HyperPlonk。对手机钱包、端侧 rollup 验证等场景价值明显，是低内存 SNARK 里难得的"理论+落地"双优工作。
- **论文 PDF**: [Scribe: Low-memory SNARKs via Read-Write Streaming](https://www.usenix.org/system/files/usenixsecurity26-baweja.pdf)

#### Distributed Synthesis of Differentially Private Tabular Datasets
- **作者**: Yucheng Fu, Tianyao Gu, Elaine Shi, Tianhao Wang
- **方向**: 密码学与协议
- **解读**: 差分隐私（DP）合成数据生成面临纵向分布场景：数据的列分散在互不信任的多个实体手中，无法直接计算刻画属性相关性的双向边际，通用 MPC 方案（CaPS）需枚举所有取值组合，代价高达 O(m²nu²)；同时 MPC 内的高斯/几何噪声采样也缺乏精确且高效的协议，使分布式 DP 合成在实践中几乎不可行。

本文提出两个新原语：其一，利用分布式点函数（DPF）将每条记录的一热向量压缩为 O(log u) 大小的密钥，服务器本地求外积即可得双向边际的加性份额，整体复杂度从 O(m²nu²) 降到 O(mnu)；其二，提出基于 CDF 表的批量噪声采样——将均匀随机数与 CDF 表拼接，利用安全排序与并行分段扫描一次完成全部采样，离散高斯采样通信复杂度仅 O(d+σ^{3/2})。二者结合实现了分布式版本的状态最优算法 AIM。

在 WAN 环境（0.5 Gbit/s、50ms 延迟）下，n=10⁴、m=14、域大小 u=100 的数据计算全部 91 个双向边际仅需 133.63 秒，比 CaPS 快约 10⁵ 倍、比 Sort-count 快 33 倍；生成 2¹⁴ 个标准差为 30 的离散高斯样本仅 13.41 秒，比最优现有协议快 69 倍。端到端合成 Adult 数据集（48,843 条记录、约 6×10¹⁷ 笛卡尔积域）仅需 24.12 分钟，而 CaPS 估计要 57 天，且合成数据在下游任务上的效用与集中式 AIM 持平。

我的思考：该工作把分布式 DP 合成的两个关键组件都做到接近最优，将列数影响从平方降到线性，复杂度分析干净利落；"与集中式效用持平"是核心卖点，实验支撑充分。局限在于安全性仅针对半诚实敌手，噪声分布靠截断参数保证与理想分布的统计距离可忽略，需谨慎配置。相比只能输出明文直方图的 Precio，本文保留秘密份额输出，打通了"计算-选择-加噪-发布"全链路。

- **论文 PDF**: [Distributed Synthesis of Differentially Private Tabular Datasets](https://www.usenix.org/system/files/usenixsecurity26-fu.pdf)

#### Libra: Pattern-Scheduling Co-Optimization for Cross-Scheme FHE Code Generation over GPGPU
- **作者**: Song Bian, Yintai Sun, Zian Zhao, Haowen Pan, Mingzhe Zhang, Zhenyu Guan
- **方向**: 密码学与协议
- **解读**: 全同态加密（FHE）的 SIMD 方案（CKKS/BGV/BFV）擅长批量线性运算但参数大、延迟高，SISD 方案（TFHE/CGGI）适合小规模时延敏感计算；跨方案编译可兼取所长，但现有编译器静态选择方案、且算法优化与硬件执行相互割裂，无法适配 GPU 这类高并行架构。跨方案 FHE 在 GPGPU 上的高效代码生成仍是空白。

Libra 是基于 MLIR 的编译框架，实现"模式-调度"协同优化：前端用 Polygeist 将 C 程序降为多层方言（scfhe/simd/sisd/gpu/async）；中端建立基于 CostLUT 的代价模型（SIMD 侧用 Chebyshev/Minimax 多项式量化非线性算子，SISD 侧按并行批大小估算 LUT 延迟），通过 SCHEME-PARTITION 与 COST-REWRITE-DOWN/UP 三趟图重写，在考虑 MTOS/STOM 转换开销的前提下动态为每个算子段选择最优方案并全局收敛；后端用算子打包与轻量/负载均衡融合，配合多流异步内核调度隐藏访存延迟。底层为自研 CKKS+TFHE+Pegasus CUDA 库（FlyHE，已开源）。

在 A100（128 位安全参数）上，Libra 相比最优跨方案基线在微基准上取得最高 270× 加速、应用上最高 19× 加速，计算单元利用率提升 44%、显存带宽利用率提升 36.1%。代价模型正确刻画了关键权衡：小输入下 SISD-FHE 更优，数据规模增大后 SIMD-FHE 因摊销胜出，且交叉点随精度要求右移，正说明静态方案选择的不足。

我的思考：作为首个支持动态跨方案选择并做硬件感知调度的 GPU FHE 编译器，Libra 把"算法-硬件协同优化"落到实处，270×/19× 的加速比极具说服力。局限在于：因缺乏可比的 GPU FHE 编译器，基线是 CPU 编译器转 GPU 后端，对比口径有待更公平的同平台检验；自研底层库可能引入与工业库的差异。整体上为 FHE 编译开辟了硬件感知动态编译的路线，与 HEIR、Pegasus 等形成互补。

- **论文 PDF**: [Libra: Pattern-Scheduling Co-Optimization for Cross-Scheme FHE Code Generation over GPGPU](https://www.usenix.org/system/files/usenixsecurity26-bian-song.pdf)

#### Efficient and High-Accuracy Secure Two-Party Protocols for a Class of Functions with Real-number Inputs
- **作者**: Hao Guo, Zhaoqian Liu, Liqiang Peng, Shuaishuai Li, Ximing Fu, Weiran Liu, Lin Qu
- **方向**: 密码学与协议
- **解读**: 两方秘密共享中数值以无符号整数 uint(x) 编码，而现实应用需要基于有符号实数 Real(x) 的运算；USENIX'25 的 Guo 等人提出的 MW 方法从份额计算有符号整数，但其约束 |x|<L/3 过于苛刻（如 x∈[-50,50] 在 Z₁₂₈ 上即不满足），严重限制实用性。如何放宽输入范围并高效、高精度地安全计算实数函数是核心问题。

本文把约束放宽到 |x|<B 对任意 B≤L/2（B=L/2 即环上的自然表示范围），关键突破是带约束的比较协议：在 |x-y|≥A 的约束下只需 O(log L/A) 通信即可完成比较，从而仅调用一次比较协议即可计算 MW(x)。在此基础上提出通用框架：对满足 func(a+b+c)=Σ fᵢ(a)gᵢ(b)hᵢ(c) 的函数，用份额拆分+查找表（LUT）+MW 索引即可安全求值，并实例化到整数除法、sin(x) 与指数函数；另提出小环转换技巧在更大环上满足约束、小环上高效计算 MW。

eˣ 协议通信量约为 SirNN（S&P'21）与 Bolt（S&P'24）的 31%，运行时间分别最高提速 5.53× 与 3.09×，最大 ULP 误差仅 1.435（SirNN 为 2.64，Bolt 为 8.681）；sin(x) 最大 ULP 误差约 1.3；除法协议比 CrypTFlow2 快 1.4–4.84×；Softmax 协议比 Iron 快 4.59–6.2×、比 Bolt 快 1.94–2.32×。当 B=0.9999·L/2 时只需 14 位比较协议，B=0.999999·L/2 时 20 位即可（对比 l=37 位，1.85× 提升）。

我的思考：该工作把实用输入范围从 L/3 扩到近整个环空间，消除了先前方法的主要部署障碍，带约束比较协议本身具有独立价值；分数 ULP 度量使精度对比更精细，是对 SirNN/Bolt 的实质性改进。局限在于：LUT+分解框架仅适用于可写成 Σf(a)g(b)h(c) 的函数类；边界约束仍以两比特误差形式存在，一旦越界误差经 MW·L 放大后果严重，论文给出先验界计算与环转换两种满足手段，但整体仍依赖有界输入假设。总体上是安全实数计算协议的高质量增量工作。

- **论文 PDF**: [Efficient and High-Accuracy Secure Two-Party Protocols for a Class of Functions with Real-number Inputs](https://www.usenix.org/system/files/usenixsecurity26-guo-hao.pdf)

#### InstantOMR: Oblivious Message Retrieval with Low Latency and Optimal Parallelizability
- **作者**: Haofei Liang, Zeyu Liu, Eran Tromer, Xiang Xie, Yu Yu
- **方向**: 密码学与协议
- **解读**: 
匿名消息系统与隐私区块链中，接收方需扫描公告板并试解密全部消息才能找到自己的消息，资源受限的移动端难以承受。不透明消息检索（OMR）允许接收方把检测与检索外包给不可信服务器而不泄露身份。既有单服务器 OMR（SophOMR、PerfOMR）基于 BFV，吞吐高但单消息延迟高达约 76 秒与 89 秒，且因依赖 BFV 大批次处理而并行性受限。

InstantOMR 采用 TFHE 与标准 RLWE 的混合设计：用两层 TFHE 函数自举实现同态 clue 解密与检查（第二层用 HomTrace 压缩追踪密钥），再用常规 RLWE 完成同态编码（桶+计数器编码索引、权重矩阵编码载荷）。逐消息独立处理使延迟与并行度成为首要优化目标。

实测（Primus-fhe 实现，TFHE-rs 为估计）单核单消息检索延迟比 SophOMR 低 864 倍、比 PerfOMR 低 1008 倍（摘要中保守口径约 600 倍）；近线性并行扩展，180 核下对 2^16 条消息比 SophOMR 快约 3 倍、比 PerfOMR 快约 13 倍；流式场景接收方等待时间从约 76 秒以上降至 0.1 秒以内；Primus-fhe 实测约比 TFHE-rs 估计慢 3 倍。

我的思考：该工作把 OMR 的评价维度从吞吐扩展到延迟与并行度，切中实时流式与频繁小批量检索的真实体验痛点；双层自举与密钥压缩设计颇具巧思。局限在于 TFHE-rs 侧数字为估计值，实测实现尚慢 3 倍；大批量场景的绝对吞吐相对 SophOMR 的优势需结合原文对比；安全性建立在 RLWE（含循环安全假设）之上，参数选择直接影响实用性。

- **论文 PDF**: [InstantOMR: Oblivious Message Retrieval with Low Latency and Optimal Parallelizability](https://www.usenix.org/system/files/usenixsecurity26-liang.pdf)

#### ZipPIR: High-throughput Single-server PIR without Client-side Storage
- **作者**: Rasoul Akhavan Mahdavi
- **方向**: 密码学与协议
- **解读**: 
私密信息检索（PIR）让客户端不泄露所访问元素的前提下取回数据库记录。RLWE 型协议（如 SimplePIR）吞吐高但要求客户端存储大 hint（SimplePIR 约 128MB），动态数据库更新时还需重新生成并重传 hint；亚线性协议（如 Piano）离线阶段通信量甚至达整个数据库，与资源受限的浏览器、手机客户端冲突。

ZipPIR 的核心是把 LWE 密文压缩为更小的 Paillier 密文：利用离线阶段完成尺寸缩减，在线阶段不引入额外计算；在计算假设下离线阶段几乎静默，除一次性公钥外无需通信，服务端可在空闲时独立生成与更新 hint。

实测吞吐超过 2 GB/s（1GB 数据库约 3 GB/s、2GB 数据库约 3.5 GB/s），与 SimplePIR 相当；1GB 数据库上比无客户端存储协议（<1 GB/s）吞吐高至 10 倍，比 HintlessPIR、YPIR 快至 2.2 倍；每个客户端的服务端存储低于 200KB，离线通信仅约 400 字节；LWE 密文（log2q=64）压缩率超 84%，RLWE 系数压缩率达 85%–98%；是首个用 Paillier 达到与 SOTA 竞争吞吐的 PIR 方案。

我的思考："离线/服务端做重活 + 密文压缩"的组合精准解决了 SimplePIR 系 hint 更新的痛点，压缩密钥比动辄上百 MB 的重线性化/自举密钥小得多，工程上很干净。局限在于依赖计算假设，Paillier 3072 位模带来服务端额外开销，吞吐天花板仍低于 SimplePIR 等专用 RLWE 方案约一个量级；服务端自主更新 hint 需明确数据库更新机制，动态场景的部署细节值得进一步探讨。

- **论文 PDF**: [ZipPIR: High-throughput Single-server PIR without Client-side Storage](https://www.usenix.org/system/files/usenixsecurity26-mahdavi.pdf)

#### Provable Secure Steganography Based on Adaptive Dynamic Sampling
- **作者**: Kaiyi Pang and Minhao Bai
- **方向**: 密码学与协议
- **解读**: 
在普遍监控下，隐写术通过把秘密消息嵌入看似无辜的载体实现隐蔽通信；可证明安全隐写（PSS）保证隐写输出与正常模型输出计算不可区分，是该领域最先进范式。但现有 PSS（METEOR、DISCOP、iMEC、SparSamp）都依赖模型显式输出分布，而真实高质量模型（如闭源 LLM）只提供接受 seed 的 API 并只返回 token，不暴露分布。

本文提出基于自适应动态采样的可证明安全隐写方案：从模型 API 采样候选 token 集合，构造消息比特串到 token 的映射，用真实秘密消息查表选出输出 token，从数学上保持原模型分布；对多个消息映射到同一 token 的碰撞不回避，而是维护动态碰撞集合并策略性扩展（追加 0/1，使规模保持在 [2^{N-1}, 2^N]），解码时取共享前缀；安全性用游戏序列 G0~G3 证明计算不可区分。

在 MISTRAL 3、QWEN 2、LLAMA 3（约 7B）与均匀分布、WILD QA、XHS 中文社交文本三个数据集上：嵌入容量与熵利用率最高，如 WILD 上 MISTRAL 达 0.8964 bit/token、利用率 0.9137，超过需显式分布的 DISCOP Reorder（0.7683/0.7930）与 SparSamp（0.8562/0.8411）；TS-CSW 隐写分析 F1 仅 49.73%–50.18%（接近随机猜测）；解码成功率 1.0000，每 token 耗时低于 100ms。

我的思考：该工作把 PSS 从"需要分布"推进到"仅需黑盒采样 API"，贴合闭源模型与 API 化部署的现实约束，实用价值明显，证明思路（均匀抽样保持分布）简洁有力。局限是依赖 API 接受 seed 且采样独立同分布，真实 API 的种子化采样未必稳定可用；容量受分布熵限制且每步需按碰撞集规模多次采样，成本随消息增长；安全保证是计算性而非信息论的。与同期 SparSamp 相比，自适应扩展策略在熵利用上更优。

- **论文 PDF**: [Provable Secure Steganography Based on Adaptive Dynamic Sampling](https://www.usenix.org/system/files/usenixsecurity26-pang-kaiyi_0.pdf)

#### Unlocking the True Potential of Decryption Failure Oracles: A Hybrid Adaptive-LDPC Attack on ML-KEM Using Imperfect Oracles
- **作者**: Qian Guo
- **方向**: 密码学与协议
- **解读**: 侧信道攻击可利用明文检查（PC）与解密失败（DF）两类oracle攻击已部署的后量子密码，二者可由时序、功耗、微架构泄漏实例化；但格基方案中长期存在"DF oracle不如二进制PC oracle"的共识——Hermelink等人的DF攻击对ML-KEM-768需7000次完美oracle查询，为信息论下界的4.49倍，导致DF路径被普遍低估。

作者提出混合自适应-LDPC攻击框架：构造覆盖多个秘密系数的精心平衡奇偶校验，使oracle输出分布接近0.5以最大化每次查询的香农信息（接近信道容量），随后用LDPC置信传播解码聚合信息并纠错，再依据后验概率自适应生成新查询消除最高不确定性，在噪声（不完美）oracle下仍保持高效。

对ML-KEM-768、oracle准确率95%时，完整密钥恢复仅需2950次查询，仅为香农下界的1.35倍，超越最优二进制PC攻击；应用于近期GoFetch攻击（Apple M系列CPU上的微架构DF oracle）时，所需测量迹减少一个数量级以上，且无需代价高昂的格归约后处理，使更高安全级方案的完整密钥恢复首次可行。

我的思考：这项工作颠覆了"DF oracle天生低效"的共识，核心创新在于多系数平衡校验设计+自适应解码的混合框架，把攻击效率推到接近理论极限，并验证了真实微架构场景下的落地价值；局限在于查询数评估基于模拟oracle模型，真实噪声分布（如GoFetch的DMP干扰）可能偏离假设。对部署ML-KEM的厂商是重要警示：DF oracle的物理泄漏不可轻视，防护需从常数时间实现与噪声工程两端同时着手。

- **论文 PDF**: [Unlocking the True Potential of Decryption Failure Oracles: A Hybrid Adaptive-LDPC Attack on ML-KEM Using Imperfect Oracles](https://www.usenix.org/system/files/usenixsecurity26-guo-qian.pdf)

#### Ajax: Fast Threshold Fully Homomorphic Encryption without Noise Flooding
- **作者**: Zhenkai Hu; Haofei Liang; Xiao Wang; Xiang Xie; Kang Yang; Yu Yu; Wenhao Zhang
- **方向**: 密码学与协议
- **解读**: 阈值全同态加密（ThFHE）让多方对加密数据做任意计算而密钥分布多方，核心任务是构造阈值密钥生成与解密协议；FHEW类方案有快速自举与小参数的优势，但已知ThFHE方案用"噪声洪泛"实现阈值解密——要么需要超大参数，要么经自举切换到参数大的方案，解密缓慢；密钥生成则或假设通用MPC/可信设置，或噪声随方数n线性增长。

作者提出快速ThFHE方案Ajax：阈值解密采用新的"mask-then-open"技术，基于不同环上的随机双重分享，无需噪声洪泛即可保证部分解密份额安全，同时保持小参数优势；阈值密钥生成在诚实多数（t=⌊(n−1)/2⌋）设置下把噪声增长从n倍降至max(0.038n, 2)倍；方案还适用于BGV、BFV、CKKS等其他FHE类型。

端到端实现（1 Gbps带宽、1ms ping）：n=3时密钥生成17.6秒、解密单个密文0.9ms；n=21时分别为91.9秒与4.4ms（LAN），WAN下解密为5.4ms与42.4ms；相比现有最优ThFHE实现，阈值解密端到端性能提升至少5.7倍、最高283.6倍（t从1增至13、跨不同网络延迟）。

我的思考：消除噪声洪泛是ThFHE效率的关键突破，"mask-then-open"+双重分享简洁而优雅，密钥生成噪声从线性降到次线性也大幅提升大规模多方场景的可扩展性；局限在于诚实多数假设、与对比方案[30]的安全模型不同（弱游戏安全性），且评估基于模拟网络。在NIST发起多方案征集、应用需求渐增的背景下，Ajax把FHEW类ThFHE从理论可行性推进到毫秒级解密的实用水平，具有直接工程价值。

- **论文 PDF**: [Ajax: Fast Threshold Fully Homomorphic Encryption without Noise Flooding](https://www.usenix.org/system/files/usenixsecurity26-hu-zhenkai.pdf)

#### Differential Trust: Dynamic Multi-Authority Anonymous Credentials with Epoch-Weighted Updates

- **作者**: Chen Li, Jianting Ning, Xiulong Liu, Yulin Liu

- **方向**: 密码学与协议

- **解读**: 匿名凭证（AC）是隐私保护认证的基石，现有分布式签发方案（Shamir 门限、聚合签名）虽消除单点故障，却平等对待所有签发权威，无法利用 Proof-of-Stake、DAO 等去中心化网络中节点质押量、信誉度天然存在的信任差异；且权威权重随惩罚、委托变动而动态演化，现有方案均无法支撑。作者将其提炼为两个挑战：差异化权重的信任模型与权重的动态调整。

方法上提出 MA-ACEW，首个考虑权威权重分布的 MA-AC 模型。核心是新型 Epoch-Bound Pointcheval-Sanders 签名（EB-PS）：把签名密钥拆分为静态分量与随时间轮换的 epoch 密钥 zj，签名分"消息签名"与"epoch 绑定"两阶段，使签名绑定特定时间片；epoch 切换时权威只需为用户计算新的 epoch 部分凭证，用户无需重新交互。权重验证借助加权门限签名（WTS）与内积论证（IPA），形式化 EUF-eCMA 安全模型，在新增的 STB-GPS 假设下证明不可伪造性、匿名性与盲性，并支持多属性、签发者隐藏与任意谓词选择性披露。

实现采用 Golang 并配套智能合约：聚合 128 个部分凭证的展示平均仅 10.68ms，链上验证约 1077K gas，聚合签名仅两个群元素。

我的思考：这是首个将"权重信任"与"动态 epoch 更新"同时纳入 MA-AC 的工作，相比虚拟化方案（成本随总权重线性增长）与 CRT 加权秘密共享（权重刚性绑定、需系统级重置）在可扩展性与灵活性上优势明显，契合 PoS 治理的真实需求。局限同样清晰：每个 lvki 仅能签一个属性、依赖 PKI、epoch 密钥轮换带来管理负担，且新假设的安全性仍需密码学界检验。整体是构造精巧、证明完整的密码学贡献，但距生产部署仍有距离。

- **论文 PDF**: [Differential Trust: Dynamic Multi-Authority Anonymous Credentials with Epoch-Weighted Updates](https://www.usenix.org/system/files/usenixsecurity26-li-chen.pdf)

#### BatchBoot: Fast Batched Bootstrapping for TFHE scheme and Practical Applications

- **作者**: Zhihao Li, Hongyu Wang, Yuan Zhao, Lichun Li, Zhiwei Wang, Jiaxing He, Lifeng Guo

- **方向**: 密码学与协议

- **解读**: TFHE 以其可编程自举（PBS）支持任意非线性函数并刷新噪声，但每次只能处理一个加密消息，缺乏批处理与消息打包机制，成为高吞吐、向量化负载（如私有集合交集、FHE 指令集）的瓶颈；现有批处理方案（AmortBoot）依赖大量 FFT、仅支持全打包、功能局限于单输入函数自举。

BatchBoot 提出三点贡献：①重构同态多项式乘法——设计多比特 CMux 变体并首次把 Hoisting 技术引入 TFHE，令自同构穿透 gadget 分解与 FFT 域、与外部乘积共享 FFT 计算，大幅削减 FFT 次数；②提出基于 RLWE-to-MLWE 提取的稀疏感知打包，多项式次数从 n 降为 n̄，复杂度由 O(hnlogn) 降至 O(hn̄logn̄)；③首次提出批处理电路自举 BatchCBoot，支持多输入 LUT，扩展函数表达能力。

相比最优批处理方案 AmortBoot（CCS'25）获得 2.4× 加速，较非批处理 TFHE-rs 快 43.8×；12 位输入/输出 LUT 基准较非批处理电路自举快 4.9×–134.3×。应用层面：首个 TFHE 非平衡 PSI 协议，12 位密文模数使总通信量仅约 15KB，较 PEPSI（USENIX Security'24）通信减少 294×、计算快 4.1×；基于 BatchCBoot 的 8 位 FHE 指令集较 Wang 等（CCS'25）快至 5.4×。

我的思考：该工作把 Hoisting、多比特 CMux、MLWE 提取等成熟技巧系统化地引入 TFHE 批自举，工程贡献扎实，PSI 的 12 位模数与 15KB 通信是明显亮点。局限在于稀疏打包参数 k 会带来额外噪声增长（仅当 k≪h 时可忽略），安全性依赖 lattice estimator 参数评估；与 BFV/CKKS 类方案相比，批自举仍以计算换取通信优势，适用场景集中于非平衡 PSI 与电路型负载。

- **论文 PDF**: [BatchBoot: Fast Batched Bootstrapping for TFHE scheme and Practical Applications](https://www.usenix.org/system/files/usenixsecurity26-li-zhihao.pdf)

#### Quorus: Efficient, Scalable Threshold ML-DSA Signatures from MPC

- **作者**: Alexander Bienstock

- **方向**: 密码学与协议

- **解读**: 分布式 CA、数字货币等场景需要把签名密钥分给多方、超过阈值才能联合签名；NIST 已标准化后量子签名 ML-DSA（Dilithium），但现有格基阈值方案多采用自定制验证算法（签名更大、大小随方数增长），与标准验证不兼容。朴素地用通用 MPC 分布式执行 ML-DSA 需在电路内计算 SHA3，代价达数百万逻辑门，不可行。

Quorus 提出 MPC 友好的 ML-DSA 签名变体：允许安全揭示不完全签名（w1 可在 MPC 外明文计算挑战 c=H(m,w1)），使哈希在各方本地执行，并证明该变体与原始 ML-DSA 同等安全，还定义了更强的不可伪造性概念 UF-CMA⊥；随后以 edabits、Rabbit 比较、Nishide-Ohta 比特分解等 MPC 积木构造诚实多数、主动安全、UC 实现理想功能 FT-MLDSA 的阈值协议。

协议每方每拒绝采样轮在线通信低至 150KB；签名与验证密钥大小与标准 ML-DSA 完全一致，所有安全级别下均可用同一标准 ML-DSA 实现验证；支持任意数量的参与方且签名分布不随方数变化，无需新的公钥假设。与之对比，Raccoon 系方案（12.7KB 签名/3.8KB 公钥，至多 1024 方）虽紧凑但验证不兼容标准。

我的思考：这是首个与 NIST 标准验证兼容、可扩展任意方数的后量子阈值签名方案，从 Schnorr 式"签名份额线性组合"转向通用 MPC 是关键概念转变，UC 证明加新安全概念使论证扎实。局限：在线通信与轮数仍高于专用方案，Borin 等并行工作在诚实多数下通信更低，存在竞争；诚实多数假设限制了 t<n/2 以外的应用，端到端部署延迟尚未展示。

- **论文 PDF**: [Quorus: Efficient, Scalable Threshold ML-DSA Signatures from MPC](https://www.usenix.org/system/files/usenixsecurity26-bienstock.pdf)

#### SONIC: Concurrent Oblivious RAM & Data Structures for Low-Latency and High-Throughput

- **作者**: Nihal Talur and Ioannis Demertzis
- **方向**: 密码学与协议
- **解读**: 仅靠加密防不住访问模式泄漏攻击，TEE 也易受侧信道影响；树状 ORAM（EnigMap、GraphOS、Oblix）延迟低但吞吐难以超过 1K req/s，而 Snoopy 这类分区方案虽吞吐高却牺牲延迟，TB 级数据理论上需 1000+ 台服务器。

SONIC 是首个面向 TEE 的并行/并发 doubly-oblivious 树状 ORAM：用 RingORAM 取代 PathORAM，以原子计数器实现无锁桶读取、just-in-time 的桶内重洗（消除 stash 参与）与无锁 append-only stash；提出森林布局（去掉顶部 R 层形成 2^R 棵子树）并行逐出，并设计"一次排序"批量逐出算法，用二维坐标加单次 oblivious sort/compaction 同时逐出多条路径；还构建 PMCHAIN（位置映射存于 oblivious 哈希表、数据存于 SONIC）替代 Snoopy 的 subORAM。

单服务器吞吐达 156K–3.3M req/s，比 EnigMap 高 29–104 倍、比 GraphOS 高 158–560 倍（N=2^27、块 64B）且延迟更低；分布式高吞吐场景下，1 个负载均衡器加 8 个 PMCHAIN subORAM 可支撑 256GB 数据集（Snoopy 双层哈希表只能到 4GB），支撑 1TB 预计仅需 32 台服务器而非上千台。

弥合了低延迟与高吞吐 ORAM 的分裂格局，安全性继承 RingORAM 的 stash 分析（溢出概率 2^-80），工程细节扎实；但并发访问要求应用层保证访问序列不相交，Z/S/R/E/T 参数调优复杂，评估限于 SGXv2 共享内存环境，端到端部署收益仍需在实际应用中验证。

- **论文 PDF**: [SONIC: Concurrent Oblivious RAM & Data Structures for Low-Latency and High-Throughput](https://www.usenix.org/system/files/usenixsecurity26-talur.pdf)

### 隐私保护（13 篇）

#### Interpolation-Based Optimization for Enforcing lp-Norm Metric Differential Privacy in Continuous and Fine-Grained Domains
- **作者**: Chenxi Qiu
- **方向**: 隐私保护
- **解读**: 度量差分隐私（mDP）按输入距离放松 LDP 以保证"邻近记录更难区分"，能显著改善效用，但现有优化方法（线性规划）在连续或细粒度域不可行：构造稠密扰动矩阵代价高，且离散化会高估距离、放宽有效 mDP 约束，漏掉细粒度的隐私泄漏。

方法为插值框架，三步：① 将 N 维秘密域划分为网格，取格角为锚点；② 求解锚点扰动优化（APO），联合优化锚点扰动分布与逐维隐私预算分配，并给出线性近似 Approx-APO（可用标准 LP 求解，附最优性界）；③ 对非锚点用 log-凸组合（加权几何均值）插值扰动概率。关键技巧是：高维朴素插值会因 ℓp 范数凸性违反全局 Lipschitz 条件，因此把 N 维插值分解为逐轴一维步骤，按 ∑ℓ εℓ^{p/(p−1)} ≤ ε^{p/(p−1)}（p>1）的维间组合规则分配预算，归一化后至多引入 2ε 常数因子松弛。

在罗马、纽约、伦敦真实路网（OpenStreetMap）上评估：AIPO 在 ε=0.2–1.6 km⁻¹ 全部预算下实现 0% mDP 违约，而 LP 与 COPT 因离散化近似出现非零违约率（如纽约 LP 最高 1.76、COPT 4.10）；效用损失持续低于 EM、Laplace、TEM 等预定义噪声机制与 Bayesian remapping（罗马 ε=0.2 时 AIPO 5.68 vs EM 8.71、RMP 5.94），并提供理论下界佐证其接近最优。

我的思考：以"锚点优化+逐维组合插值"同时解决可计算性与严格隐私，理论干净（定理 1-2、命题 3）。归一化的 2ε 代价与线性代理目标是务实取舍。局限：预算分配仍靠网格搜索（仅低维可行）、需要先验分布、评估限于 2D 场景。相比 ConstOPT 与 Bayesian remapping，这是首次把严格 mDP 保证扩展到细粒度域，对位置服务等应用有直接价值。

- **论文 PDF**: [Interpolation-Based Optimization for Enforcing lp-Norm Metric Differential Privacy in Continuous and Fine-Grained Domains](https://www.usenix.org/system/files/usenixsecurity26-qiu-chenxi.pdf)

#### Residual-PAC Privacy: Automatic Privacy Control Beyond the Gaussian Barrier
- **作者**: Tao Zhang and Yevgeniy Vorobeychik
- **方向**: 隐私保护
- **解读**:
差分隐私需计算全局敏感度（NP-hard），组合最优界是 #P-complete；PAC Privacy 用互信息上界实现自动私有化，但论文证明（Proposition 1）Auto-PAC 的高斯上界仅在"输出为高斯且噪声独立高斯"时紧，任意分布下存在"高斯间隙"，导致预算浪费——Theorem 2 显示名义预算增加甚至可能减少真实泄漏。

两条路线。其一，在 PAC 框架内用 Donsker–Varadhan 表示与 sliced Wasserstein 距离做后处理修正（Theorem 3/4），得到更紧上界 β−D̂Z 节省预算。其二，更根本地提出 Residual-PAC（R-PAC）隐私——用 f-散度量化对抗推理后"仍剩余"的隐私（KL 下由条件熵刻画），并设计 SR-PAC：把加噪私有化建模为 Stackelberg 博弈（领导者选噪声最小化扰动、跟随者选解码器最大化推断），凸双水平优化下均衡即最优噪声分布，支持各向异性噪声、方向选择性（先扰动与标签正交方向）与组合，Monte Carlo 黑盒实现。

实验在 CIFAR-10/100、MNIST、AG-News 上对比 Auto-PAC/Efficient-PAC，在 Iris/Rice 上对比 DP 基线；摘要称 SR-PAC 一贯取得更优隐私-效用权衡，Theorem 5 证明其实现精确泄漏-预算对齐（Priv=1）。（提取文本止于实验设置，定量图表未含。）

对 PAC Privacy 保守性的刻画（"高斯屏障"）是重要理论贡献，R-PAC 把隐私度量从"泄漏多少"翻转为"还剩多少"，视角新颖且工程可行。局限：无界域需选参考分布；条件熵估计依赖解码器族表达能力；与 DP 的可比性依赖成员推断这一特定威胁模型。

- **论文 PDF**: [Residual-PAC Privacy: Automatic Privacy Control Beyond the Gaussian Barrier](https://www.usenix.org/system/files/usenixsecurity26-zhang-tao-residual.pdf)

#### Vεrity: Verifiable Local Differential Privacy
- **作者**: James Bell-Clark, Amrita Roy Chowdhury
- **方向**: 隐私保护
- **解读**:
本地差分隐私（LDP）聚合对输入分布小变化高度敏感，恶意用户篡改随机化器可引入 Õ(m(1+1/ε)+√n/ε) 的 ℓ1 误差，比仅篡改输入（Õ(m+√n/ε)）多出 m/ε 量级——隐私越强放大越严重。已有方案需交互或威胁模型弱。目标：保持随机响应（RR）单消息非交互特性，用非交互证明杜绝随机化器篡改。

两个构造。Vεrity-Auth（有授权输入）：授权方用 EQS 盲签名签五元消息（输入位承诺、令牌、密钥/载荷指纹），用户脱盲重随机化保证不可链接；令牌由授权方采样，Legendre PRF 密钥 Ki=ski+τi 联合构成；噪声用 k 个 PRF 比特乘积生成 ρ=1/2^k 伯努利样本（避免 ZKP 中昂贵比较），用户出 PRF 求值、比特翻转、签名授权输入三个 ZK 证明。Vεrity（无授权输入）：在 (2,2) 阈值 BGV 同态下求值 PRF，服务器利用 BGV 确定性重跑比对密文验证——首个"加密随机性"方案。

Vεrity-Auth 无掉线时完全阻止投毒（误差仅 Θ(√n/ε)）：授权用户 4.67ms/授权方 2.05ms，可验证 RR 位 ZKP 8.37ms（证明 1.19KB），服务器验证 3.54ms，比同类工作快 100 倍；Vεrity 用户 1424s、服务器 1473s（可行但昂贵）。首次显式分析掉线攻击：Vεrity-Auth 把最坏攻击压到依赖随机性与授权比特和的概率事件，Vεrity 限制为仅输入篡改（Õ(m+√n/ε)）。

系统刻画并补上 LDP 鲁棒性缺口——随机化器篡改是 LDP 独有攻击面；盲签名+联合密钥+ZKP 的组装在非交互与效率间平衡出色，有 Google 隐私沙箱等真实场景。局限：Vεrity 开销大离实用尚远；ρ=1/2^k 的 ZKP 友好噪声与标准 RR 有偏差；payload 隐私/真实性明确不在范围。

- **论文 PDF**: [Vεrity: Verifiable Local Differential Privacy](https://www.usenix.org/system/files/usenixsecurity26-bell-clark.pdf)

#### Nudge: A Private Recommendations Engine

- **作者**: Alexandra Henzinger, Emma Dauterman, Henry Corrigan-Gibbs, Dan Boneh

- **方向**: 隐私保护

- **解读**: 个性化推荐以用户行为数据为燃料，这些数据被转卖、滥用、泄露，是隐私失败重灾区。匿名代理在去匿名化攻击下失效，联邦学习泄露个体数据，可信硬件易受侧信道攻击，而密码学推荐系统规模仅数千用户。本文提出 Nudge：三台独立服务器以安全多方计算（MPC）训练推荐模型并向用户返回个性化推荐，用户隐私在攻击者完全攻陷一台服务器时依然成立。

方法上，Nudge 的核心是"矩阵分解幂迭代算法"与 MPC 的协同设计：幂迭代交替执行秘密共享矩阵乘向量（复制秘密共享无交互完成）与简单非线性函数（函数秘密共享实现），使通信随用户与物品数线性增长，区别于既往通用梯度下降的立方级成本。作者还设计截断与归一化协议（截断通信降 6 倍）并集成差分隐私等扩展。

在 Netflix 数据集（50 万用户、1 万物品）上，三台 192 核服务器局域网 50 分钟、40 GB 通信完成私有训练；推荐质量 nDCG@20 为 0.29，与非隐私矩阵分解持平，接近神经网络推荐器的 0.31；服务阶段每用户仅 298 KB、0.38 秒。Criteo 数据集 1.5 小时/124 GB，Yelp 数据集 8 小时/250 GB。相较双服务器混淆电路方案快四个数量级。

我的思考：本文的可贵在于"为算法选 MPC 载体"而非"用通用 MPC 硬扛"——幂迭代天然适配复制秘密共享+函数秘密共享，使隐私推荐首次达到百万用户规模。局限：需三台不共谋服务器且仅半诚实安全；泄露聚合模型与每用户评分数量（可加 DP 缓解）。

- **论文 PDF**: [Nudge: A Private Recommendations Engine](https://www.usenix.org/system/files/usenixsecurity26-henzinger.pdf)

#### From Easy to Hard++: Promoting Differentially Private Image Synthesis Through Spatial-Frequency Curriculum

- **作者**: Chen GONG and Kecen Li, Zinan Lin, Tianhao Wang

- **方向**: 隐私保护

- **解读**: 差分隐私（DP）图像合成以严格隐私保证支持敏感数据共享，但 DP 噪声严重损害合成质量。多数研究聚焦改进核心优化（DP-SGD）或依赖可能记忆风险、未必对齐的公开数据集。DP-FETA 开创"训练捷径"范式：用"中心图像"预热合成器再以 DP-SGD 微调。本文发现中心图像仅捕捉简单特征、对相似样本多的数据集才有效，进而提出 FETA-Pro，把频域特征作为更细粒度的训练捷径。

方法上，FETA-Pro 按"空间特征→频域特征→完整图像"的由易到难顺序预热：频域复杂度介于中心图像与完整图像之间，课程更平滑。针对空间/频域训练差异，利用生成模型的流水线特性——先用辅助 GAN 生成器学习与含噪频域特征对齐的图像，再让主扩散模型在这些合成图像上训练，隐式编码频域信息，最后用 DP-SGD 在原始数据上微调。

在五个敏感数据集上与七个基线对比，ε=1 时较最优基线 DP-FETA 保真度平均提升 25.7%、效用提升 4.1%；CelebA 与 Camelyon 的 FID 分别改善 20.2% 与 41.2%，下游分类准确率提升 7.7% 与 6.7%；收敛更快，CIFAR-10 训练开销仅增 0.3%，且全程不依赖公开资源。

我的思考：本文把课程学习思想引入 DP 图像合成，频域作为中间复杂度桥梁的设计精巧；GAN+扩散模型的流水线组合是对"单一合成器"惯例的突破。局限：频域捷径的有效性依赖数据频域规律，预算分配依赖经验推荐而非理论最优。总体为 DP 生成"少样本难任务"提供了不依赖公共数据的实用路径。

- **论文 PDF**: [From Easy to Hard++: Promoting Differentially Private Image Synthesis Through Spatial-Frequency Curriculum](https://www.usenix.org/system/files/usenixsecurity26-gong-easy-hard.pdf)

#### United We Defend: Collaborative Membership Inference Defenses in Federated Learning
- **作者**: Li Bai, Junxu Liu, Sen Zhang, Haibo Hu
- **方向**: 隐私保护
- **解读**: 联邦学习（FL）中的成员推理攻击（MIA）可判定某样本是否在目标客户端训练集中，而利用训练全程时间轨迹的轨迹型 MIA 让现有各客户端独立实施的防御失效；在“仅部分客户端按隐私需求防御”的部分防御场景中，受保护客户端成员样本与未受保护客户端非成员样本反而更易区分。

方法上，CoFedMID 由三个模块构成：类引导划分（依据 Johnson 界设计最小重叠的类别子集分配并线性衰减，限制本地模型对训练样本的过度记忆）；效用感知补偿（训练中后期用多臂老虎机 EXP3 回收高贡献样本并以软标签+熵约束做置信度正则化，防止回收样本过度自信）；聚合中性扰动（向客户端更新注入与聚合权重正交的噪声，聚合时互相抵消、不损全局模型）。

在 CIFAR10/100 与 TinyImageNet 上对抗 7 种轨迹型 MIA（Loss-S、Avg-C、FMIA-I/II、FTA-C/L、SeqMIA）：Pair 场景平均 AUC 从无防御的 0.77 降至 0.52、TF01 从 6.59 降至 1.58，精度仅降 0.01；Half 场景平均 AUC 0.51 而多数基线随联盟扩大明显掉点；统一防御下 SeqMIA 的 AUC 仅 0.69（对比 DPSGD/Mixup/HAMP 的 0.98/0.98/0.99），效用损失最小（-0.04），非 IID（β=0.5）下仍显著优于无防御。

我的思考：首次提出“部分防御+防御者联盟”这一贴近现实的 FL 隐私范式，三模块各司其职且消融实验扎实，协作模式在 TF01 上全面优于独立模式，工程可落地。局限：联盟内部假定诚实可信且需协调者（随机选举或第三方），对恶意联盟成员与自适应攻击的鲁棒性论证偏弱；非 IID 严重时防御力度下降；仅验证图像分类。与均匀防御相比，其差异化价值在于承认隐私需求异质性，为跨设备/跨孤岛 FL 提供了更经济的隐私-效用折中方案。

- **论文 PDF**: [United We Defend: Collaborative Membership Inference Defenses in Federated Learning](https://www.usenix.org/system/files/usenixsecurity26-bai.pdf)

#### B-Privacy: Defining and Enforcing Privacy in Weighted Voting
- **作者**: Samuel Breckenridge, Amy Zhao, Patrick McCorry, Ari Juels
- **方向**: 隐私保护
- **解读**: 传统一人一票制中隐私即选票秘密，但按代币权重投票的 DAO/web3 系统颠覆了这一假设：即使选票加密，原始计票结果本身常会暴露个体投票选择（如某选民权重的小数部分唯一对应其选项）。现有选票秘密方案未考虑计票泄露，而买票/贿选已成现实威胁（Curve 贿选市场规模超 2.5 亿美元，LobbyFi 控制 Arbitrum 大额提案 8–14% 的选票）。

作者引入 B-privacy 概念，以博弈论量化"基于公布计票的贿选经济成本"：定义贿赂博弈（贝叶斯纳什均衡下达到目标结果的最小贿赂额）。并设计两类攻击验证计票泄露：鲸鱼攻击（权重超过某选项总权重则必未投该选项，迭代消除）与子集和攻击（按精度反解唯一选票分配），组合成统一攻击算法；在 31 个 DAO 的 3,844 个提案上验证显著泄露。

防御上提出对计票加噪（Laplacian）并在必要时纠正公布结果的机制，因纠正使经典差分隐私界失效，作者给出新的证明技术刻画 B-privacy 与透明度（计票准确性）的权衡；在 27 个 DAO 的 2,503 个二元提案上，以极小透明度损失将 B-privacy 几何平均提升 3.5 倍，且相对提升对效用分布假设稳健。

我的思考：以"贿选经济成本"而非理想化秘密性定义隐私，务实且可量化，是加权投票领域首个原则性隐私框架，与抗胁迫性互补（一个防共谋证明、一个防计票推断）；局限在于仅限二元选择、未建模弃权与多选，且加噪纠正后结果可能误导紧票差场景的透明度。与 Cicada、Snapshot shielded voting 等只做选票加密的方案相比，其"计票即信道"的洞察对 DAO 治理设计有直接指导意义。

- **论文 PDF**: [B-Privacy: Defining and Enforcing Privacy in Weighted Voting](https://www.usenix.org/system/files/usenixsecurity26-breckenridge.pdf)

#### Turn Your Face Into An Attack Surface: Screen Attack Using Facial Reflections in Video Conferencing

- **作者**: Yong Huang; Wanqing Tu
- **方向**: 隐私保护
- **解读**: 视频会议中，人脸是画面焦点，但同时受环境光与屏幕光照射，会不自觉地反射屏幕内容，构成隐蔽侧信道。既有"涉密反射"攻击多利用眼镜反射或周边物体，受制于虚拟背景模糊、低分辨率（360p-720p）、镜面角度苛刻且并非人人戴镜。本文论证人脸漫反射可作为稳定、普适的屏幕泄露源，并实现FaceTell窃听系统。

作者先做理论建模（不同面部区域对屏幕不同部位的敏感度）、仿真与实验三层可行性验证；FaceTell以Mask-R-CNN与Haar级联裁剪人脸，CAMixerSR超分重建，经ResBlock与CBAM增强反射特征，执行两级分类（内容类别→具体应用），并用基于人机交互时间连续性的启发式标签纠正（HLC）修正误判。

实测于3个笔记本品牌、4个主流平台（Zoom、Teams、Skype、WeChat）、24名受试者、13种室内环境、超12小时视频数据上完成：应用预测精度99.32%，内容类别预测99.85%（多媒体/OS/Web类达100%）；每帧约124ms；消融显示各组件均有效，HLC贡献7.29%-27.34%的精度提升；对性别（女98.70%/男98.18%）、遮挡（眼镜/口罩纠错后均超99%）、距离与角度鲁棒，可同时攻击两名受害者。

首次将面部漫反射确立为视频会议侧信道，普适性远超眼镜反射方案，是隐私攻击研究的重要补充。局限在于仅推断应用类别而非精确屏幕内容，需要获得受害者视频流，且测试环境有限；"窃听多任务活动"的实际威胁需结合社工场景评估。作者提出的对策（如反射抑制与模糊）对会议平台隐私设计有直接参考价值。

- **论文 PDF**: [Turn Your Face Into An Attack Surface: Screen Attack Using Facial Reflections in Video Conferencing](https://www.usenix.org/system/files/usenixsecurity26-huang-yong.pdf)

#### NOIR: Privacy-Preserving Generation of Code with Open-Source LLMs
- **作者**: Khoa Nguyen, Issa Khalil, Khang Tran and Cristian Borcea, Ruoming Jin, Abdallah Khreishah, My T. Thai
- **方向**: 隐私保护
- **解读**: 云 LLM 代码生成中，服务商能完整观察客户提示词与生成代码，而 80% 以上的企业将知识产权泄漏列为使用云生成式 AI 的主要顾虑、近 45% 报告过意外数据暴露；本地托管私有 LLM 成本不可行，现有 dx-隐私方法（T2T、SnD）面向分类任务，token 级扰动会严重破坏代码功能。

NOIR 采用分裂学习：把开源 LLM 拆成编码器（前几个注意力块）+中间块（云）+解码器（最后几个），客户端本地编码提示词嵌入发给云，云用中间块"丰富"嵌入后返回，客户端解码器本地生成代码，原始提示词与代码均不出客户端。隐私核心是两个机制：INDVOCAB（在词表层做 ε-IND 不可区分性，等价于 token 嵌入级的本地差分隐私，只随机化一次并保持一致以保留 token 间相关性）与 LTOKENIZER（数据无关的随机化分词器，把 one-hot 索引打乱，防梯度重建）；STUNING 分裂学习微调恢复效用。

在 CodeLlama-7B、CodeQwen1.5-7B-Chat、Llama3-8B-instruct 上，Evalplus MBPP 与 HumanEval 的 Pass@1 达 76.7 与 77.4，BigCodeBench 38.7（仅比原模型降 1.77%），显著优于 T2T、SnD；对 Vec2Text（嵌入重建）与 BiSR（梯度重建）攻击及频率分析有效；客户端推理/微调成本相比本地托管降低约 10 倍（编码器-解码器仅 1–4 个注意力块），并提供开源实现（Qwen2.5-Coder-32B）与 Web API、VS 扩展。

我的思考：首次在代码生成任务上端到端保护"提示词+生成代码"，ε-IND 相对 dx-privacy 的关键改进是"一次随机化、跨提示词保持一致"，在隐私与效用间找到了更好的平衡点，安全性以安全游戏形式刻画（重建保真度上界）。局限：诚实但好奇云假设（恶意云可拒绝服务但看不到明文）、需要信任企业数据管理者、嵌入级保护的残余侧信道（如输出统计）边界尚需进一步考察。成本-隐私-效用三点平衡使其具备实际落地潜力。
- **论文 PDF**: [NOIR: Privacy-Preserving Generation of Code with Open-Source LLMs](https://www.usenix.org/system/files/usenixsecurity26-nguyen-khoa.pdf)

#### Can we estimate privacy vulnerability of individual records? Towards Mitigating Attribute Inference Attacks on ML Models
- **作者**: Ehsanul Kabir, Najrin Sultana, Ninghui Li, Shagufta Mehnaz
- **方向**: 隐私保护
- **解读**: 针对表格数据的属性推断与模型反演是医疗、金融等隐私关键领域的重大威胁，但现有防御（差分隐私、互信息正则化）对所有记录统一施加粗粒度保护，隐私收益常以显著效用损失为代价。本文的核心问题是：能否在单条记录粒度上预测其属性推断脆弱性，从而支撑感知脆弱性的防御设计。

方法上，攻击者视角的 NeighVE 为目标记录合成邻域并计算邻域角差（n.a.d.）来估计脆弱性；分析发现记录级脆弱性主要由数据集层面特征决定——即邻域相似度（邻域中共享同一敏感属性值的比例），与模型架构和攻击策略几乎无关。据此提出防御 VESL：按敏感属性值构造脆弱/非脆弱记录等比例的平衡子集，训练多个子模型并集成（多数投票 VESL-MV 或随机选择 VESL-RS）；配套的 AttriVET 将邻域相似度超过边际分布的记录判为脆弱。

实验方面，AttriVET 在 Census-19、Adult、Texas-100X 上预测 CSMIA/LOMIA 攻击下记录脆弱性的准确率达 87.93%–99.30%。VESL 将 Census-19 上 CSMIA 准确率从 66.42% 降至 37.03%（MV）和 46.12%（RS），低于无需模型访问的插值攻击（65.84%）；测试准确率仅由 79.37% 降至 76.35%（SMIR 跌至约 50%），公平性 EOD 由 0.310 降至 0.034。反事实实验表明，拉平邻域敏感属性分布后攻击成功率降至 51/49，接近随机。

我的思考：该工作把脆弱性研究从粗粒度分组推进到单记录粒度，给出"邻域相似度"这一仅依赖数据的因果驱动因素，VESL 在隐私-效用-公平三维度上全面优于现有防御，成色较高。但 NeighVE 在 Census-19 上的区分度明显弱于 Adult（top-25% n.a.d. 记录中正确推断比例由约 84% 降至 69.95%），合成邻域质量受限；实验集中于二值敏感属性的表格数据，泛化性仍需验证。

- **论文 PDF**: [Can we estimate privacy vulnerability of individual records? Towards Mitigating Attribute Inference Attacks on ML Models](https://www.usenix.org/system/files/usenixsecurity26-kabir.pdf)

#### Inference Attacks Against Graph Generative Diffusion Models
- **作者**: Xiuling Wang and Xin Huang; Guibo Luo; Jianliang Xu
- **方向**: 隐私保护
- **解读**: 图生成扩散模型（GGDM）能生成复杂图结构，训练数据可能含社交关系、蛋白质相互作用、医疗关系等敏感信息，但其隐私风险几乎未被探索；现有推理攻击依赖概率向量、图嵌入或梯度等特征，而GGDM只输出生成图且缺乏输入-输出一一映射，攻击难以直接迁移。

作者设计三类黑盒推理攻击：图重构攻击（GRA）用图对齐从生成图重建与训练图结构相似的图；属性推理攻击（PIA）推断训练图的平均密度、度分布等属性；两种成员推断攻击（MIA，基于不同攻击特征Ours-1/Ours-2）。在三种GGDM（EDP-GNN、GDSS、Digress）与六个真实数据集（MUTAG、QM9、ENZYMES、Ego-small、IMDB-B、IMDB-M）上评估。

GRA的F1达0.72~0.99，精确匹配覆盖率最高0.21、F1>0.75覆盖率达0.36，全面优于无对齐基线；PIA推断平均度绝对差不超过0.312（差异比<7.5%）、密度差<0.049（<18.8%）；MIA攻击准确率0.650~0.999、AUC 0.763~0.999，数据迁移场景下AUC仍达0.61~0.96，显著超越基线；作者还提出基于显著性图选择性地翻转边/非边的预处理与后处理两种防御，比现有方法取得更好的防御强度-模型效用权衡。

我的思考：这是首个系统研究GGDM信息泄露的工作，三类攻击加两类防御形成闭环，并针对图数据变长、置换不变等特性设计了专用攻击特征，为图生成模型的隐私研究建立了基线；局限在于MIA需训练影子模型、图对齐依赖REGAL等外部工具、防御基于启发式重要性度量，白盒梯度基线（Baseline-2）在部分场景仍具竞争力。与图像扩散模型的隐私攻击相比，其价值在于揭示"生成图本身即泄漏通道"，对发布GGDM服务的机构是直接警告。

- **论文 PDF**: [Inference Attacks Against Graph Generative Diffusion Models](https://www.usenix.org/system/files/usenixsecurity26-wang-xiuling.pdf)

#### Bridges to Self: Silent Web-to-App Tracking on Mobile via Localhost

- **作者**: Tim Vlummens, Aniketh Girish and Nipuna Weerasekara, Frederik Zuiderveen Borgesius and Gunes Acar, Narseo Vallina-Rodriguez

- **方向**: 隐私保护

- **解读**: 浏览器与移动 OS 依赖沙箱与进程隔离保证 web 与 app 上下文分离；作者证明该隔离假设在 Android 上已被 Meta 与 Yandex 实际突破：它们利用 localhost 通道把 web 追踪与原生身份桥接，实现此前未被记录的 web-to-app 追踪范式。localhost 被标准视为"潜在可信源"、豁免混合内容限制，浏览器允许网页脚本不经用户同意访问本机回环地址，形成结构性缺口。

方法上结合美欧双视角大规模爬虫（top-100K 站点、Android/桌面/iOS 多次爬取）与系统化 Android app 静态+动态分析：网页内嵌的 Meta Pixel/Yandex Metrica 脚本经 HTTP(S)、WebSocket 或 WebRTC 连接同一设备上原生 app 监听的回环端口，把 web 端 cookie（如 _fbp）或服务端参数传给 app，app 回传 Android 广告 ID（AAID）等持久原生标识，由后端完成关联与去匿名化；Yandex 还用服务器下发动态端口配置与 AES-RSA 加密载荷规避检测。

测量显示：Yandex 自 2017 年 2 月起（HTTP）与 2018 年 5 月起（HTTPS，端口 29009/30102/29010/30103）持续使用超过 8 年；Meta 自 2024 年 9 月起快速演进 HTTP→WebSocket→WebRTC STUN（SDP munging 注入 _fbp）→TURN；2025 年 4 月 Android 爬虫发现 17,368（美）/15,819（欧）个站点建立 WebRTC localhost 连接（Meta 占 99% 以上），HTTP(S) 案例中 Yandex 占 95%；75.8%–78.2% 的站点在用户同意前即触发；公开披露当日（2025-6-3）两家即终止，WebRTC 使用量骤降 97.5%；该机制可绕过 cookie 清除、隐身模式、MAID 重置、VPN 与工作/个人档案隔离，且 _fbp 是约 25% 的 top-1M 站点使用的第三大常见第一方 cookie。

我的思考：这是测量+负责任披露型研究的范本，证据链完整（爬虫、App 逆向、Frida 动态钩取、历史归档数据互相印证），并指出 LNA 权限仍可被 global-unicast IPv6（WebRTC）与 mDNS（*.local）等旁路绕过，说明防线修补不彻底。其价值不止于揭发两家公司：它暴露了"隔离原则"在 web 标准、平台信任模型、app 审核流程上的系统性失效，并附法律分析推动政策讨论，对浏览器与移动平台的威胁模型重构具有直接指导意义。

- **论文 PDF**: [Bridges to Self: Silent Web-to-App Tracking on Mobile via Localhost](https://www.usenix.org/system/files/usenixsecurity26-vlummens.pdf)

#### The Adverse Effects of Omitting Records in Differential Privacy: How Sampling and Suppression Degrade the Privacy–Utility Tradeoff

- **作者**: Àlex Miranda-Pascual, Javier Parra-Arnau, Thorsten Strufe

- **方向**: 隐私保护

- **解读**: 采样（subsampling）因隐私放大性质被广泛认为能通过降低所需噪声来提升 DP 机制效用，这一假设在 DP-SGD 之后仍缺乏系统检验。本文在"相同隐私水平"的口径下证明该假设有误：采样删除记录造成的效用损失，普遍超过隐私放大折算出的噪声降低收益。

方法上，作者在无界 DP 下对均匀泊松采样做系统实验，覆盖 Laplace、Gaussian、指数、报告噪声最大值（RNM）四种经典机制以及 DPLloyd、k-median 聚类；并将统计披露控制中的抑制（suppression）推广为任意记录选择/删除策略，首次为无界近似 DP 下的任意抑制算法导出隐私参数上界（εS、δS），且结果独立于机制 M。

对 ε∈{0.25,0.5,1,2}、采样率 0.01–0.99 的几乎所有组合，M∘S 的效用保证均差于直接使用 M：如 Adult 库 age 列上 RNM 返回非众数的失败概率从 <18% 升至 >60%；均值误差在采样率 >0.4 时 <0.25%、接近 0 时 <2%，但始终高于不采样。测试的抑制策略同样无法改善隐私-效用权衡，且均匀采样反而是表现最好的抑制方式之一。

我的思考：该结论对"采样总是有利"的社区默认构成重要警示，把抑制形式化为一般化采样并给出独立于机制的理论界是实质贡献。但效用指标限于均值/众数/聚类，对端到端机器学习任务的普适性仍需验证；"固定隐私水平"的对比口径是否覆盖所有实践用法（如把放大用于达标而非提效）也值得商榷，为后续研究留下空间。

- **论文 PDF**: [The Adverse Effects of Omitting Records in Differential Privacy: How Sampling and Suppression Degrade the Privacy–Utility Tradeoff](https://www.usenix.org/system/files/usenixsecurity26-miranda-pascual.pdf)

### 系统与操作系统安全（13 篇）

#### UncoreBleed: AEX-Free, High-Resolution, and Low-Noise Side-Channel Attacks on SGX Enclaved Execution
- **作者**: Decheng Chen, Zhi Zhang, Zhenkai Zhang, Xin Zhang, Yansong Gao, Yi Zou
- **方向**: 系统与操作系统安全
- **解读**: 学界长期认为 Intel SGX 禁用性能计数器（PMC）以缓解侧信道，但 Intel 文档并未声明生产型 enclave 禁用全部 PMC。本文验证：敏感的 core PMC 确实被禁用，但 uncore 子系统 PMC 仍持续记录与 enclave 执行相关的事件，推翻"SGX 完全抑制性能监控"的信念；并追问其能否提取细粒度机密、能否绕过最先进软件防御。

论文识别出 M2M（mesh-to-memory）子系统的 PKT_MATCH 事件：支持可编程地址过滤，可对选中 64 B 内存块计数。作者系统逆向 Xeon：弄清过滤机制（ADDRMATCH/ADDRMASK 寄存器、位级"don't care"）、跨代可用性（Intel 手册缺失或错误）与物理地址到 M2M 设备的映射（bit 8 起参与 XOR 路由），从而可并行监控多地址。UncoreBleed 仅靠轮询 M2M 计数器（最短 650 ns 采样间隔）被动监控 DRAM 访问，不触发 AEX；配合 CR0.CD 非缓存执行、锁低频、时钟调制把 enclave 执行拉长 2500–3600×，使 RSA 循环可被可靠采样。

两个案例：其一，从 enclaved Libjpeg 恢复输入图片（900×600 至 6016×4016 分辨率），以 64 B 粒度直接区分同页内快速/常规路径（此前受控信道仅 4 KB 页级）；其二，在 TLBlur+AEX-Notify 部署下从单次 RSA 解密恢复私钥。测量显示 2048 位 RSA 单循环约 5500 ns、4096 位约 12000 ns，远超 650 ns 采样分辨率。

我的思考：首个基于 PMC 的 SGX 侧信道，AEX-free 被动监控使其与依赖页错误/中断的受控信道有本质区别，也解释了 TLBlur 为何无法阻止；逆向工程扎实，64 B 粒度监控有独立价值。局限：限于 SGX 使能 Xeon 平台与特定事件；需恶意 OS（属威胁模型内）；执行拉伸手段有可观察系统级影响。该工作表明纯软件防御难以对抗物理内存级监控，硬件层面（禁用 uncore PMC 或 MEE 级混淆）才是更可靠方向。
- **论文 PDF**: [UncoreBleed: AEX-Free, High-Resolution, and Low-Noise Side-Channel Attacks on SGX Enclaved Execution](https://www.usenix.org/system/files/usenixsecurity26-chen-decheng.pdf)

#### M-Step: A Single-Stepping Framework for Side-Channel Analysis on TrustZone-M
- **作者**: Cristiano Rodrigues, Marton Bognar, Sandro Pinto, Jo Van Bulck
- **方向**: 系统与操作系统安全
- **解读**: 高端 TEE（SGX/TDX、SEV、TrustZone-A）已有成熟单步攻击框架（SGX-Step 催生 48+ 攻击、SEV-Step、TDX-Step 等），而 IoT 上主流的 Arm TrustZone-M（Cortex-M）在高分辨率软件侧信道方面几乎空白。难点：Cortex-M 无虚拟内存与页表，且中断行为独特——多周期指令（ICI）可被中止或暂停以降低延迟，指令级确定性单步是非平凡挑战。

M-Step 是首个 TrustZone-M 单步框架。作者大量剖析中断处理与流水线机制，发现此前被否定的"中断延迟泄漏"可反其道而用：以往认为 Cortex-M 因可中断指令而"免疫 IRQ 延迟时序攻击"，本文利用这种延迟信息动态校准定时器间隔，在非安全世界可靠实现安全世界每个指令边界的中断。框架含调试/可视化基础设施与三种侧信道插件：指令缓存活动、总线竞争、中断延迟，开放可扩展。

实验中，M-Step 在最新 Arm Mbed TLS（集成于 Trusted Firmware-M）发现未知漏洞：近距离探测 BEEA（二进制扩展欧几里得算法）多个秘密相关分支，用单条 trace 确定性恢复完整 RSA 密钥——首个单迹提取真实密码学密钥的 TrustZone-M 侧信道攻击。负责任披露后 Arm 发布 Mbed TLS v3.6.5 补丁（CVE-2025-54764），框架已开源（github.com/M-Step-Framework）。

我的思考：补全单步框架版图（高端 TEE 下沉到 MCU 级），"利用中断延迟做动态定时校准"推翻 Cortex-M 对 IRQ 延迟攻击免疫的论断，方法论新颖。局限：依赖 Cortex-M 具体微架构，可移植性需逐平台验证；单迹攻击针对 BEEA 这类复杂非常数时间实现，说明受审查密码库仍会泄漏。对 TF-M/Mbed TLS 生态有实际安全影响，再次证明 MCU 级常数时间纪律与自动化侧信道测试的必要性。
- **论文 PDF**: [M-Step: A Single-Stepping Framework for Side-Channel Analysis on TrustZone-M](https://www.usenix.org/system/files/usenixsecurity26-rodrigues.pdf)

#### DMGuard: Safeguarding Kernels from Physical-Page Use-After-Free Vulnerabilities

- **作者**: Juhee Kim

- **方向**: 系统与操作系统安全

- **解读**: 内核安全机制（ASLR、CFI、PAC、MTE 等）都依赖页表正确性，攻击者已转向破坏地址转换层本身：虚拟地址仍映射到已释放物理页即形成"物理页 UAF"，与传统堆 UAF 本质不同，一旦利用可绕过几乎所有缓解。更糟的是 CPU、GPU、IOMMU 各维护独立页表且由不同组件管理，零拷贝共享等设计使页生命周期协调极易出错，近年漏洞激增。本文提出 DMGuard，首个跨多种地址转换域缓解物理页 UAF 的运行时方案。

方法上，DMGuard 把每个物理页生命周期建模为状态机，用两个无锁轻量原语检测非法迁移：映射计数 MapCount 在释放时非零即报 free-before-unmap；8 位随机页标签（RefTag 存于 struct page 指针最高字节，ARM64 用 TBI 实现）在映射时标签不匹配即报 map-after-free，并顺带检测 double-free。仅靠原子指令与内存屏障保证并发正确性，并经 Linux 内核内存模型（LKMM）在全部支持架构上验证。

在 Linux v4.9/v6.1/v6.6 及 Android（Pixel 8、Pixel 3 XL，覆盖 Mali 与 Adreno 驱动）上，成功检测阻断 24 个物理页 UAF 漏洞（15 个实测、9 个理论分析），而现有方案 CONFIG_PAGE_TABLE_CHECK 实测 15 例仅检出 5 例；GeekBench 开销约 1%；核心实现约 400 行，GPU 驱动改动仅 90/80 行。

我的思考：本文把漏洞视角从"悬垂指针"提升到"悬垂映射"，抓住了页表完整性这一盲区；无锁设计与 LKMM 验证兼顾性能与正确性。相比仅跟踪 CPU 侧映射的 PAGE_TABLE_CHECK，跨 CPU/GPU/IOMMU 的统一状态机是实质进步。局限是依赖驱动走标准接口，直接篡改页表的驱动无法覆盖，标签碰撞存在理论漏检。总体为异构地址转换域的页生命周期管理提供了可落地范式。

- **论文 PDF**: [DMGuard: Safeguarding Kernels from Physical-Page Use-After-Free Vulnerabilities](https://www.usenix.org/system/files/usenixsecurity26-kim-juhee-dmguard.pdf)

#### CuSafe: Capturing Memory Corruption on NVIDIA GPUs
- **作者**: Hongyi Lu, Fengwei Zhang, Zhenkai Zhang, Shuai Wang, Yanan Guo
- **方向**: 系统与操作系统安全
- **解读**: GPU 应用（机器学习、科学计算）大量使用 C/C++，内存破坏漏洞频发（PyTorch、TensorFlow 已有先例且可被利用实现 GPU 上 ROP），但现有方案或依赖定制硬件（LMI、GPUShield、IMT）、专有工具链（cuCatch），或检测能力有限（GMOD/clArmor 哨兵式漏掉非线性溢出），NVIDIA compute-sanitizer 平均慢 15×，均不实用。

方法上，CuSafe 是首个可部署于商品 NVIDIA GPU 的软件消毒器：混合元数据=指针标签（2^n 对齐大小、全局/局部类型位、身份信息）+ 缓冲区带内精确边界（8 字节前缀，释放时清零以检测 UAF）；利用 GPU MMU 页表可配置性在指针高位编码标签；栈 epoch 跟踪（深度 5 位+代次 5 位，全局数组 16.5MiB）防局部内存元数据混淆，全局内存用最多 37 位 VA 随机化防重用；带内元数据契合 GPU 广播机制（N 线程只需 1 次额外事务）；再以循环提升等优化消除冗余检查。

33 个漏洞程序上空间与时间漏洞覆盖率均达 100%（compute-sanitizer 20%/55.6%，cuCatch 66.7%/83.3%，LMI 60%/66.7%，GPUShield 53.3%）；44 个基准（Rodinia、PolyBench、Tango 及 LLaMA2-7B/LLaMA3-8B）平均减速仅 13%、LLM 吞吐降 11%，内存开销 16.5MiB 固定+每分配 8 字节（实测平均 0.5MB/0.01%），最差情况 gemm 83% 而 compute-sanitizer 达 153×。

我的思考：把 CPU 侧成熟的内存消毒思路（标签+元数据+VA 随机化）适配到 GPU 的 MMU 与广播架构，无需改硬件即可获得全面时空覆盖，性能与内存开销均为现有方案中最低档，工程上（LLVM 21 变换+LD_PRELOAD 挂钩）干净利落。局限：标签位受 CUDA 运行时限制（全局指针高位固定为 0）、栈深度/代次回绕引入概率性漏报、对齐到 2^n 与 16B 最小粒度可能影响极端分配。相比 cuCatch 的 256 标签碰撞风险，37 位随机空间显著更强，是 GPU 内存安全实用化的重要一步。

- **论文 PDF**: [CuSafe: Capturing Memory Corruption on NVIDIA GPUs](https://www.usenix.org/system/files/usenixsecurity26-lu-hongyi.pdf)

#### Side-Channel Attacks on Open vSwitch
- **作者**: Daewoo Kim and Sihang Liu
- **方向**: 系统与操作系统安全
- **解读**: 云虚拟化中 Open vSwitch（OVS）通过 microflow/megaflow 两级软件缓存加速主机到虚拟机的包转发，缓存被多租户共享且可通过网络远程访问，可能成为破坏虚拟机隔离的侧信道；此前 OVS 安全研究只关注恶意用户造成的 DoS，侧信道维度无人探究。

方法上，作者在单跳网络上系统刻画 OVS（v2.17.9+DPDK，8k 条目 microflow 缓存、40 个子表）并总结三个攻击原语：①三级路径延迟可区分（microflow 命中 270.08µs、megaflow 277.18µs、慢路径 1196.03µs）；②microflow 哈希碰撞可泄露五元组头部字段；③megaflow 子表按命中率周期性（1–1.43s）重排、访问延迟反映访问频率。据此实现三种远程攻击：microflow/megaflow 隐信道、报头恢复（Prime+Probe 找被逐出条目+离线枚举 2^48 对 IP/端口）、包速率监控（相对子表位置反映相对速率）。

microflow 隐信道带宽 15.8bit/s（96.8% 准确率，多条目并行可达 222.3bit/s），megaflow 隐信道 0.73bit/s；报头恢复攻击探测 7 分钟（50 次重复、417.6s）即可 91% 准确率恢复受害者源 IP 与端口；真实流量追踪（UNSW-NB15 背景噪声）下包速率监控成功率 71.92%；提出的三种缓解（OVS 实例隔离、microflow 哈希随机化、megaflow 子表重排随机化）经评估可有效抑制这些信道。

我的思考：把 CPU/数据库缓存侧信道的成熟方法论（Prime+Probe）系统移植到软件交换机，证明“共享加速结构”在虚拟化网络栈中同样破坏隔离，报头恢复攻击直接威胁五元组机密性，是对 NetSpectre/NetCAT 谱系的重要扩展且无需推测执行。局限：单跳实验室环境、DPDK 数据路径（内核版 OVS 无 microflow 缓存，隐信道带宽大幅下降）、噪声下准确率降至 80%、依赖攻击者能访问与受害者共置的服务。缓解方案简单直接，但哈希/重排随机化与 OVS 性能优化的张力需进一步评估。
- **论文 PDF**: [Side-Channel Attacks on Open vSwitch](https://www.usenix.org/system/files/usenixsecurity26-kim-daewoo.pdf)

#### Jailbreaking the AMD Secure Processor: Enabling Live Analysis of SEV-SNP's Undocumented Security Boundaries
- **作者**: Muyan Shen, School of Cryptology, Hongzhan Ma
- **方向**: 系统与操作系统安全
- **解读**: SEV-SNP 以 AMD 安全处理器（ASP）为硬件信任根保护云端虚拟机，但其安全依赖动态热补丁等不透明固件机制，黑盒特性使独立审计几乎不可能；前向安全——旧固件版本被攻陷后能否威胁已打补丁的新版本——缺乏验证平台与威胁建模。本文以 ASPBreaker 越狱搭建活体分析平台填补这一空白。

方法上，逆向出 Secure DRAM（0xfffdfb000000，UMC 内联 AES 加密、页表乱序映射），借助 BadRAM 式内存别名攻击绕过访问控制获得读写；再对固件热更新"验签→备份→拷贝"流程发起 TOCTOU 攻击——利用备份阶段大而稳定的竞态窗口，用预先收集的恶意固件密文覆盖 FWTemp，实现完全确定性的任意代码执行；注入极简 miniloader 从 x86 主机动态加载任意 ARM 代码，构建持续活体分析平台。

据此揭示三类攻击向量：GCTX 页使用静态密钥致跨版本机密性破坏（降级+重放即可解密新固件上运行 VM 的内存）；利用 EL0/EL1 间 MMIO 地址空间无隔离实现 EL0→EL1 提权，进而"洗白"证明书（attestation laundering）；绕过内存别名缓解位 ALIAS_CHECK_COMPLETE 伪造最新 VCEK 签名的证明报告；发现均已负责任披露。

首个实用且确定性的 ASP 越狱，把 BadRAM 从"危及 guest"升级为"攻陷硬件信任根"，从根本上动摇 SEV-SNP 前向安全模型；局限是沿用 BadRAM 的一次性物理接触 SPD 模型、仅在 Milan 平台验证、热更新路径有提交固件保护需走冷更新；其结论强力呼吁厂商对不透明固件边界开放独立审计。

- **论文 PDF**: [Jailbreaking the AMD Secure Processor: Enabling Live Analysis of SEV-SNP's Undocumented Security Boundaries](https://www.usenix.org/system/files/usenixsecurity26-shen.pdf)

#### PROBE+DETECT+MITIGATE (PDM): Enabling Cloud Tenants to Self-Defend against Microarchitectural Attacks

- **作者**: Arash Daneshmand; Hugo Kermabon-Bobinnec; Lingyu Wang; Makan Pourzandi; Suryadipta Majumdar; Yosr Jarraya
- **方向**: 系统与操作系统安全
- **解读**: 微架构攻击（PRIME+PROBE、Spectre等）在多租户云中威胁严重，但检测通常依赖硬件性能计数器（HPC）等供应商级资源，租户无法访问；而补丁部署迟缓，已修补威胁仍活跃。本文提出PDM，让云租户无需供应商帮助即可独立检测并缓解微架构攻击。

PDM的思路是"以攻之技御攻"：用攻击者同款探测技术做检测——Access+Reload方案捕捉驱逐类攻击的异常缓存缺失，Flush+Reload方案捕捉瞬态执行攻击的意外缓存命中，Scheme 3+在等待窗口内多路复用探测，单线程可覆盖512KiB范围；检测线程直接注入受害者二进制（免源码修改与插桩）；两级检测（快速分类器触发即时非侵入缓解，慢速分类器确认后解除或升级缓解）；缓解含缓存混淆（预取）、内存加密（PROT_NONE+SIGSEGV+trampoline二进制改写+AES掩码）与CRIU迁移。

测试床（KVM+Kubernetes）上快速分类器TPR≥99.72%、FPR≤0.13%，AWS Fargate上TPR≥98.63%、FPR≤0.83%；12小时无攻击数据FPR仅0.02%；触发缓解领先时间7ms，使泄漏率116.70-4420.46 B/s的四种Spectre变体零字节泄露；缓存侧信道位错误率从基线0%升至最高51%；SPEC CPU 2017开销≤2.47%，对比Cipherfix在多数场景开销更低。

租户自主防御的定位切中云安全"共享责任"痛点，探测式检测覆盖驱逐类与瞬态类攻击颇具巧思，两阶段检测平衡了延迟与误报。局限：需应用自声明秘密访问指令并自改写（trampoline）、探测线程须固定到受害vCPU；混淆类缓解本质是安全通过模糊，依赖慢速分类器及时兜底；PRIME+PROBE在Fargate因伦理考量未测。整体是云租户侧微架构防御的有力补充。

- **论文 PDF**: [PROBE+DETECT+MITIGATE (PDM): Enabling Cloud Tenants to Self-Defend against Microarchitectural Attacks](https://www.usenix.org/system/files/usenixsecurity26-daneshmand.pdf)

#### ARM MTE Performance in Practice

- **作者**: Taehyun Noh; Yingchen Wang; Tal Garfinkel; Mahesh Madhav; Daniel Moghimi; Mattan Erez and Shravan Narayan
- **方向**: 系统与操作系统安全
- **解读**: ARM内存标记扩展（MTE）以4位标签将分配映射到16个一致性类，能概率性检测时间与空间内存安全缺陷（错标签访问有15/16概率触发陷阱），且无需修改应用，被视为可默认开启的生产级缓解。但"低开销"这一承诺在真实硅片上是否成立，此前缺乏跨微架构的系统测量，本文给出首份全面分析。

作者在5种微架构上测量MTE的SYNC/ASYNC/ASYMM模式：Google Pixel 8/9的Performance（Cortex-X）、Big（A7x）、Little（A5x）核心，AmpereOne服务器核心，及Apple M5的初步数据；基准覆盖SPEC CPU 2006/2017与RocksDB、Nginx、PostgreSQL、Memcached等服务器负载；并用定向微基准把开销归因到微架构机制。

结果喜忧参半：多数基准开销温和（AmpereOne SPEC 2017几何均值1.03×），但存在显著性能悬崖——Pixel Performance核SYNC模式达6.64×（456.hmmer）、2.37×（464.h264ref），Pixel Big核ASYNC也达1.82×；AmpereOne最高1.43×。归因结果：Performance核的存储序列化、Big核标签检查结构瓶颈、AmpereOne不一致的存储转发（厂商确认并在下一代硅片修复）；Memcached最高1.40×源于Linux内核MTE支持缺陷（与Ampere共同修复）。专用场景中，内存追踪比页权限方案快2-3×，TOCTOU防护最高提速1.087×，而沙箱与CFI无明确收益。

该工作价值在于把MTE开销从"黑盒估计"变为可归因的微架构分析，并证明多数开销可修复——Ampere下一代修复与内核补丁佐证了"低开销MTE可行"。局限：聚焦堆内存概率安全场景，Apple M5仅初步数据；亦指出前人估计因方法/实验错误而不准确，为后续MTE研究提供了可靠基线。

- **论文 PDF**: [ARM MTE Performance in Practice](https://www.usenix.org/system/files/usenixsecurity26-noh.pdf)

#### kSFS: Repurposing a Microkernel-like Interface for Fast and Secure In-Kernel Linux File Systems
- **作者**: Dinglan Peng and Pedro Fonseca
- **方向**: 系统与操作系统安全
- **解读**: 
文件系统代码量大、语义复杂，是操作系统漏洞的主要来源：ext4 自 2008 年主线性以来累计 72 个 CVE（仅约 65 kLoC），内存与并发 bug 密度居各子系统之首。内核内沙箱需要定义良好且经强威胁模型校验的组件边界，但 Linux 文件系统与 VFS、页缓存、块设备等子系统深度交织，指针频繁跨界，设计这样的安全接口极具挑战。

kSFS 的答案是复用微内核式接口：把 FUSE 协议（Linux 中为数不多的、以强威胁模型校验与非可信实现交互的接口）作为沙箱内核文件系统的安全边界；同时把 WebAssembly 推广到内核空间作为通用沙箱机制，基于 WAMR 大量改造（处理内核页错误、限制 CPU 占用防死循环）以强制隔离保证；并提供 WASI/POSIX 兼容层与修改版 libfuse，移植 NTFS、exFAT 用户态实现仅需修改不到 300 行代码。

实测相对用户态 FUSE 基线显著更快：tar 场景最高提升 29%，RocksDB 吞吐达 FUSE 的约 60 倍；相对无隔离的 Linux 实现仅低 0%–52%。64 线程下 exFAT 随机读 430 kIOPS，是 FUSE 的 31 倍、仅比 Linux 低 23%；顺序读写与零拷贝 FUSE 相当（NTFS 写比 FUSE 高 21%、比 Linux 低 19%），exFAT 顺序读写比 Bento 高 170% 与 41%。

我的思考："复用现成微内核接口 + 内核内 WASM"的组合很巧妙：FUSE 协议自带强威胁模型校验，WASM 提供通用软件隔离，既获得接近原生的性能又规避了用户态往返开销。局限在于仍是文件系统单一组件的验证，性能与原生 Linux 在高并发下仍差 23% 以上；WAMR 内核化改造后的运行时自身成为新可信计算基，其安全性未充分回答；FUSE 协议语义能否覆盖 mmap、DAX 等全部内核特性也值得追问。

- **论文 PDF**: [kSFS: Repurposing a Microkernel-like Interface for Fast and Secure In-Kernel Linux File Systems](https://www.usenix.org/system/files/usenixsecurity26-peng-dinglan.pdf)

#### vCause: Efficient and Verifiable Causality Analysis for Cloud-based Endpoint Auditing
- **作者**: Qiyang Song; and School of Cyber Security; Wenbo Jiang; Heqing Huang; Yong Liu; Dan Meng; and School of Cyber Security
- **方向**: 系统与操作系统安全
- **解读**: 云端点审计中，安全管理员依赖云端对日志派生的版本化溯源图做因果分析以调查攻击行为，但云可能被攻陷或不可信（如丢弃日志节省资源），篡改将导致因果分析不完整——约72%的安全分析师曾遭遇日志篡改；现有防篡改日志方案需检索并重放全部日志、TEE在 enclave 内处理海量日志开销大，而选择性验证又存在完整性问题。

作者提出VCAUSE，集成两种认证数据结构：图累加器基于分层索引Merkle树（本地树按时间戳索引版本节点、全局树按实体ID聚合），支持"实体ID+时间戳"双键的点节点查询证明；可验证版本化溯源图通过递归哈希为每个节点计算入路径与出路径摘要，配合图分割策略，证明其因果相关组件，二者协同完成因果分析两步关键验证，并对自适应恶意云给出形式化安全证明。

原型3500行C++：2分钟处理2500万条日志，10万节点因果分析生成证明仅49ms；与真实端点日志器集成、高负载下端点计算开销小于1%、云端为3.36%，效率显著优于全量重放类方案。

我的思考：这是少有的直接面向"因果分析结果可验证"的认证数据结构设计，分层Merkle树解决双键查询、路径摘要刻画图可达关系，思路精巧且开销极小；局限在于大规模动态图的路径摘要更新与图分割策略的复杂度、评估数据集规模有限。与证书透明度式选择性验证相比，VCAUSE天然保证完整性而不牺牲效率，为"不可信云端审计"这一现实威胁提供了可落地的信任锚点。

- **论文 PDF**: [vCause: Efficient and Verifiable Causality Analysis for Cloud-based Endpoint Auditing](https://www.usenix.org/system/files/usenixsecurity26-song-qiyang.pdf)

#### KernelRCA: Facilitating Root Cause Analysis of Memory Corruptions in Linux Kernel with Contextual Causality Chain
- **作者**: Kangzheng Gu
- **方向**: 系统与操作系统安全
- **解读**: 持续模糊测试基础设施（如Syzkaller）已发现大量内核bug，但自动根因分析（RCA）的现有表示是孤立形态——谓词只描述触发条件、程序元素只给出buggy代码片段，分析师仍需手动拼合调用上下文与数据依赖来还原完整的bug触发过程；内核的多任务架构与大量间接调用使这一推断极其困难。

作者提出上下文因果链（CC-chain）这一新型根因表示：因果链串联导致bug的关键意外行为的代表指令，上下文信息展示这些指令间的控制流与数据依赖，直观反映集成触发过程；据此设计KernelRCA系统，包含选择性追踪、上下文信息恢复与链式根因分析三部分，自动构造CC-chain。

在65个真实世界Linux内核内存破坏（覆盖GPF、NPD、UAF、OOB、KNP等多种崩溃类型）上，KernelRCA正确诊断54个；CC-chain的根因距离普遍远小于崩溃报告、KASAN报告乃至Syzbot的cause bisection（多例报告距离为0而KASAN/崩溃报告为∞）；用户研究表明其报告显著加速分析人员理解与修复bug。

我的思考：把根因表示从"孤立特征"升级为"链+上下文"，直击内核多任务、间接调用导致的流程推断难题，是对ARCUS等工作的实质补充；局限在于11个失败案例（跨PoC任务的数据依赖、复杂基址计算指令）与6条链中的假阳性（误报的UBI与类型混淆），工具链仍依赖调试信息的准确性。对syzkaller生态而言，CC-chain可望把"报bug"推进到"讲清bug怎么发生"，降低内核安全团队的人工负担。

- **论文 PDF**: [KernelRCA: Facilitating Root Cause Analysis of Memory Corruptions in Linux Kernel with Contextual Causality Chain](https://www.usenix.org/system/files/usenixsecurity26-gu-kangzheng.pdf)

#### VeCT: Secure and Efficient Constant-Time Code Rewriting with Vector Extensions

- **作者**: Qisheng Jiang, Danfeng Zhang

- **方向**: 系统与操作系统安全

- **解读**: 时序侧信道攻击可通过分析执行时间提取秘密，常数时间（CT）纪律以数据流/控制流线性化（DFL/CFL）防御之，但线性化代码会触碰全部可能访问的地址，内存足迹大增，现有自动改写器带来 3.6×–70.5× 的开销；而 AVX-512 指令实现细节专有，其掩码、索引的时序与缓存行为此前未知。

VeCT 先用 dudect 式在线 Welch t 检验（阈值 10，每配置 10 万次，i9-11900 与 Xeon Gold 5215 双平台）逆向刻画 AVX-512 指令，得到三条可复用安全规则：G1 打包装载在掩码位机密时仍 CT；G2 打包存储在掩码至少 1 位为 1 时 CT；G3 全 1 掩码的 gather/scatter 在均匀访问缓存行时 CT。据此设计消除冗余预装载、打包连续访存、支持向量化读写（vct_load/vct_store）的重写策略，基于 LLVM 与 Constantine 自动实现。

统计测试发现 SOTA CT 改写器 Constantine 存在此前未知的时序漏洞。在 AES、Blowfish 等真实应用上，gather/scatter 方案平均降低开销 46.0%（最高 98.8%），packed load/store 方案平均降低 42.9%（最高 98.9%），同时保持 CT 性质。

我的思考：把统计测试方法论用于专有指令的侧信道逆向并给出可复用规则，方法学价值高，发现既有工具漏洞更有直接安全意义。局限：结论基于两款 Intel 处理器，需扩展到 AMD/ARM 平台；规则依赖具体微架构，跨代迁移性存疑；评估基准规模偏小，复杂真实程序上的端到端收益有待更大范围验证。

- **论文 PDF**: [VeCT: Secure and Efficient Constant-Time Code Rewriting with Vector Extensions](https://www.usenix.org/system/files/usenixsecurity26-jiang-qisheng.pdf)

#### Memclave: Secure In-Memory Enclave for Untrusted Hosts

- **作者**: Amit Choudhari, Fabian van Rissenbeck, Christian Rossow

- **方向**: 系统与操作系统安全

- **解读**: 云上数据密集型负载在 CPU 与内存间频繁搬移数据，共享缓存的访问模式可被侧信道攻击利用；处理中内存（PIM，如 UPMEM）把计算移入 DRAM、大幅缩小缓存足迹，但商业 PIM 暴露宿主可编程的控制平面与宿主共享内存，恶意宿主可注入篡改内核、重路由数据流、读取破坏计算状态。SE-PIM、PIM-Enclave 需硬件改造且无真实实现，CPU 侧密码学方案又太重。

Memclave 是纯软件方案：TPM 证明的 hypervisor 在启动时永久隔离宿主对 PIM 控制平面的访问并实现运行期 PIM 锁定；每个内存核（DPU）内的可信 loader 认证用户内核、建立会话级受保护数据路径；宿主仅需把少量数据搬移调用替换为安全 drop-in，保持编程模型，TCB 小、移植成本低。

在商用 UPMEM DIMM 上实现并移植 PrIM 套件全部 16 个基准：一次性约 100ms 认证装载后，核内执行时间接近 PIM 基线——MLP 在实用规模下 ≤1.5×，BFS 在部分图上 1.1×（随 frontier 层数适度上升）；微基准 EM↔LSM 流式吞吐 140.76 vs 140.52 MiB/s，开销可忽略。

我的思考："信任但验证"的思路务实，纯软件兼容现成硬件，且给出 hypervisor TCB 最小化的可行路径（Xen 约 1MB、pKVM/jailhouse 约 9kLoC、seL4）。相比 CPU 侧密码方案首次硬化了控制平面。局限：信任根依赖 TPM 与启动完整性，受保护传输的开销随字节数线性增长，带宽受限场景收益有限；PIM 内部 tasklet 竞争等侧信道与密钥托管问题尚待评估。

- **论文 PDF**: [Memclave: Secure In-Memory Enclave for Untrusted Hosts](https://www.usenix.org/system/files/usenixsecurity26-choudhari.pdf)

### 硬件与物理安全（11 篇）

#### HAMLOCK: HArdware-Model LOgically Combined attacK
- **作者**: Sanskar Amgain, Daniel Lobo, Fnu Suya
- **方向**: 硬件与物理安全
- **解读**:
DNN 推理加速器（FPGA/ASIC）多由不可信第三方设计制造。现有模型级后门在权重中留下完整逐层激活路径，易被 Neural Cleanse、MNTD 等防御检出；现有硬件木马依赖硬件重计算、开销大。论文提出 HAMLOCK，把后门逻辑拆成"触发检测"与"误分类载荷"，跨软硬件边界分布。

软件侧仅微调 ≤3 个触发神经元：单神经元攻击优化第一层权重/触发器使触发输入产生极高激活；多神经元攻击随机选 r=3 层各 1 个神经元，用 100 张校准图经 max-min 优化学习激活分离，经神经元消融保证精度损失≤3%。硬件侧两个木马：触发器 HT 监视 FP32 符号位或 8 位指数并与阈值比较，载荷 HT 向目标 logit 指数注入偏置 b′=1.1·z_max 强制误分类；RTL Verilog 实现并经 45nm 综合。

无木马硬件时软件完全良性（干净精度损失至多 MNIST 0.3%/ImageNet 2.6%，后门输入 ASR ≤0.6%）；部署到木马硬件后 1-N 攻击 ASR 100%。无需自适应优化即绕过全部 SOTA 防御：Neural Cleanse/MNTD 检测率 0%，STRIP/BBCAL 等 AUC≈0.5；微调/剪枝/BEAGLE 下 ASR 仍 100%。硬件开销极小：面积≤0.08%、功耗≤1.14%（3-N 攻击 3.4%）。

提出"跨层后门"新攻击范式——攻击逻辑不在任何单一组件完整存在，模型级与硬件级防御各自为战导致系统性失效，暴露供应链审查的结构盲区。局限：需模型白盒访问+硬件厂商失陷的强前提；未讨论侧信道暴露；3-N 攻击 ASR（93.6-97%）略低；论文未给出有效防御，属攻击先行工作。

- **论文 PDF**: [HAMLOCK: HArdware-Model LOgically Combined attacK](https://www.usenix.org/system/files/usenixsecurity26-amgain.pdf)

#### Behind Bars: A Side-Channel Attack on NVIDIA MIG Cache Partitioning Using Memory Barriers
- **作者**: Cheng Gu, Reese Levine, Zhenkai Zhang, Tyler Sorensen, Yanan Guo
- **方向**: 硬件与物理安全
- **解读**: MIG 将 GPU 划分为硬件隔离实例并分区 L2 缓存，官方声称可缓解缓存侧信道，且二代 MIG 是 NVIDIA 机密计算 TEE 的基础；但既有研究只覆盖一代 MIG 或 PCIe 等非承诺组件，分区 L2 的真实隔离边界从未被验证。本文针对 Hopper 二代 MIG 的 L2 分区设计发问：跨实例缓存攻击是否仍然可能。

方法上，先用 Nsight 分析发现"内存屏障（membar）"会跨实例传播：内核启动、cudaFree/cudaMemcpy/cudaMemset、上下文创建/销毁三类驱动活动都会触发 MEMBAR.GPU/SYS，其 L2 请求被另一实例的 profiler 观测到；因 profiler 在安全环境被禁用，进一步发现带强序标注的 LD.STRONG.GPU 指令延迟对跨实例 membar 稳定敏感（个体测量分离清晰），据此构造 Membar+Load 定时隐蔽信道并升级为侧信道。

Membar+Load 隐蔽信道容量峰值 1.57 Kb/s；侧信道方面，在 20 个流行 LLM 上指纹识别宏平均 F1 达 94.28%，并能分离 prefill/decode 两阶段、据时长推断输入输出 token 数以推测任务主题，图处理负载可按输入图指纹；已向 NVIDIA（2025-06-30 披露）与 AMD（CPX 设计）负责任披露。

首个跨 MIG 实例的缓存攻击，打破"硬件分区即隔离"假设，直击 NVIDIA 机密计算信任根基；局限是 MIG+CC 驱动尚未发布而未能实测该配置、信号弱（LD 延迟增量 0.5%–3.2%）、需大量并行加载且吞吐有限；为 GPU 多租户与机密计算的可信边界敲响警钟。

- **论文 PDF**: [Behind Bars: A Side-Channel Attack on NVIDIA MIG Cache Partitioning Using Memory Barriers](https://www.usenix.org/system/files/usenixsecurity26-gu-cheng.pdf)

#### TrojPix: Electromagnetic Covert Channels via Imperceptible Pixel Modulation
- **作者**: Guoming Zhang, Huiting Zhang, Yanni Yang and Pengfei Hu
- **方向**: 硬件与物理安全
- **解读**: 气隙网络依靠物理隔离防数据泄露，既有 EM 隐蔽信道（GSMem、BitJabber、TEMPEST-Lora 等）难以同时实现高吞吐、远距离与视觉隐蔽：TEMPEST-Lora 传 87.5 米却仅 21.6 kbps，BitJabber 速率 300 kbps 但距离仅 3 米，限制了实战价值。

方法上，洞察到数字视频线的 TMDS 编码会把像素值映射为高速二进制符号，微小像素修改即可确定性改变线缆辐射的 EM 信号（类似全数字无线电思想），全程无需系统特权或硬件改动；通信方案结合像素-采样映射（P2S-Map）、匹配滤波相关同步、跨行交织纠错编码（CRRC）、多频带采样融合与自适应判决阈值（ADT），并设计伪息屏与前景嵌入两种攻击模式。

在 9 家厂商 12 款显示器与 15 条 COTS 线缆上平均 BCR 达 99.13%–99.20%（FEC 后 100%）；峰值吞吐 8.1 Mbps（20 MHz 采样率），最大传输距离 208 米，穿 30 cm 混凝土墙后 BCR 仅由 99.96% 降至 99.14%；嵌入图像 SSIM 0.998/0.999，50 名志愿者无人察觉视觉差异。

首次把"不可见像素调制"与软件定义电磁辐射结合，同时达到 Mbps 级吞吐、数百米距离与视觉不可察觉，威胁模型务实（纯用户态恶意软件）；局限是依赖恶意软件植入与室外定向天线+LNA 接收，且线缆屏蔽、HDMI 认证等对策可削弱；相对 TEMPEST-Lora 等是数量级跃升，对气隙隔离策略提出新挑战。

- **论文 PDF**: [TrojPix: Electromagnetic Covert Channels via Imperceptible Pixel Modulation](https://www.usenix.org/system/files/usenixsecurity26-zhang-guoming.pdf)

#### TIMESLICE-SANDWICH: A GPU Side-Channel Attack Exploiting Time-Sliced Scheduling
- **作者**: Hodong Kim and Gyeongsup Lim, Seunghee Shin, Youngjoo Shin and Junbeom Hur
- **方向**: 硬件与物理安全
- **解读**: GPU 时间片调度是多应用并发的标准机制，但其侧信道含义几乎未被研究；且传统计时手段（线程计数、GPU 时钟）在 Ada Lovelace 上会随其他应用的时间片暂停而失效。本文首次系统揭示时间片调度本身可成为侧信道泄漏源。

方法上，用 CUDA Runtime 的 Event 管理 API 配合仅含空循环的最小忙等核（避免资源争用噪声）跨时间片测量，发现单时间片上界约 2.0–2.1 ms（GEMM 与 OpenGL 负载一致），并刻画 MPS 启用下 CUDA 与图形负载间仍会发生时间片切换；据此构造 TIMESLICE-SANDWICH 原语：以 2.5 ms 忙等核"夹住"受害者恰好一个时间片，通过测得的时长推断其 GPU 利用率模式，再用 LSTM 分类器识别模式；不依赖任何共享资源争用、无特权、无需与受害者共置。

在 Turing/Ampere/Ada 三架构上：20 个 PyTorch ImageNet 模型恢复平均 F1 94.40%（两套输入数据 94.80%/94.00%）；Chrome 渲染的 Alexa Top 200 网站指纹平均 Top-1 92.84%（默认调度 92.11%、MPS 下 93.56%）；引入噪声后固定后台负载数时 F1 85.87%，负载数大幅变化时最差降至 50.16%。

首次证明 GPU 时间片调度是全新侧信道攻击面，不依赖争用的特性使其天然免疫针对缓存/内存争用的既有防御，攻击场景（DNN 训练、浏览器）贴近真实；局限是需与受害者同机并发执行、吞吐有限、MPS 下仅对 MPS 外的图形负载有效、浏览器场景无法从 JavaScript 发起；对调度随机化与时间片扰动等缓解有直接启示。

- **论文 PDF**: [TIMESLICE-SANDWICH: A GPU Side-Channel Attack Exploiting Time-Sliced Scheduling](https://www.usenix.org/system/files/usenixsecurity26-kim-hodong.pdf)

#### StackWarp: Breaking AMD SEV-SNP Integrity via Deterministic Stack-Pointer Manipulation through the CPU's Stack Engine
- **作者**: Ruiyi Zhang
- **方向**: 硬件与物理安全
- **解读**: AMD SEV-SNP 承诺即使恶意宿主在多租户云中开启 SMT 也无法篡改机密虚拟机（CVM）的执行。此前的攻击（CacheWarp、Ahoi 中断注入等）聚焦内存层次与执行单元，忽略了 CPU 前端——栈引擎。

StackWarp 是纯软件架构攻击：Zen 1–5 上存在一个未文档化的共享 MSR 位用于开关栈引擎，而其状态在兄弟逻辑核间未正确同步。攻击者在宿主侧翻转该位，使访客执行栈指令时"冻结"累积的栈指针增量，再"释放"形成确定性、可双向、最大 640 字节的单次 RSP 偏移，无需明文、gadget 或注入中断。作者通过对 MSR 空间的系统性模板化扫描发现该漏洞。

在包括完全打补丁的 Zen 5 上演示了 4 个端到端攻击：RSA-CRT 单次故障签名即可经 Bellcore 分解恢复私钥（100 次中 49 次产生有效故障签名，每次均可完整恢复、耗时低于 0.1 秒）；OpenSSH 密码认证绕过（10 次中 70% 成功、单次 8 秒内）；sudo 提权（篡改 getuid 返回值）；以及经 select 系统调用栈缓冲区实现内核态 ROP。AMD 已分配 CVE-2025-29943 并发布微码补丁。

我的思考：这是首个源自前端配置的 SEV-SNP 完整性突破，确定性、可重复、跨代通用，把"SMT 开启即可信"的假设彻底击穿；攻击者控制超线程即可在指令级精度上改写访客控制流，比 CacheWarp 的缓存回滚更直接。缓解层面禁用 SMT 只是止损，需微码/硬件将栈引擎控制限定在发令线程或 CVM 激活时锁定，其对机密计算云的安全影响评估方式值得借鉴。

- **论文 PDF**: [StackWarp: Breaking AMD SEV-SNP Integrity via Deterministic Stack-Pointer Manipulation through the CPU's Stack Engine](https://www.usenix.org/system/files/usenixsecurity26-zhang-ruiyi.pdf)

#### Loongleak: Architectural Cross-Privilege-Boundary Data Leakage on LoongArch CPUs
- **作者**: Lorenz Hetterich
- **方向**: 硬件与物理安全
- **解读**: 架构级 CPU 漏洞研究长期集中于 x86-64、Arm 与 RISC-V，而中国基础设施大量采用龙芯 LoongArch 处理器且正逐步禁用外国 CPU，形成全球安全盲区。龙芯手册规定 4 字节浮点加载（FLD.S）后寄存器高位"不确定"，作者发现这些位并非任意值，而是直接返回 L1 数据缓存中的陈旧字节。

通过 QEMU 与龙芯 3A5000 的差分模糊测试（在 2^32 条指令空间内同种子执行 10,000 条指令对比）发现该异常并逆向其机理：前置 priming 加载确定被泄露的缓存组，FLD.S 目的地址决定行内偏移与路，攻击者可精确选择泄露的缓存组、偏移与路；泄露纯架构性、无需时序或侧信道，速率超 300 MB/s，可从非特权用户态、容器与虚拟机发起。

在 3A5000 与 3A6000 上演示：无 SMT 情况下恢复内核用于全盘加密的 AES 密钥；从 SMT 兄弟线程的高特权用户态进程泄露 /etc/shadow 根密码哈希片段；秒级攻破 ASLR 与栈金丝雀。缓解方面，浮点模拟使 SPEC 2017 浮点负载慢 10–21 倍，退出内核时冲刷 L1 仅增约 1.4% 开销但 3A6000 上需关闭一半逻辑核，长期仍需硬件修复；已向龙芯披露（2025 年 7 月，一年禁令期）。

我的思考：这是首个针对 LoongArch 的架构级跨域泄露，其"不确定位=缓存陈旧数据+可精确控制缓存组/偏移/路"的组合，控制粒度优于 ZenBleed 与 ÆPICLeak，且无需特权即可跨内核/虚拟机域读取；对国产 CPU 替代进程是重要警示——本土化不等于安全，亟需独立安全研究投入；软件缓解的高昂代价也说明架构设计阶段就应杜绝"未定义位"这类隐患。

- **论文 PDF**: [Loongleak: Architectural Cross-Privilege-Boundary Data Leakage on LoongArch CPUs](https://www.usenix.org/system/files/usenixsecurity26-hetterich-lorenz-loongleak.pdf)

#### Silicon Heist: (Ransom) Attacks for Cloud FPGAs via Privilege Escalation

- **作者**: Simon Klix; Russell Tessier
- **方向**: 硬件与物理安全
- **解读**: 云FPGA已成为数十亿美元产业，但租户设计运行在云服务商（CSP）的硬件上，CSP虽通过验证流水线和设计规则检查（DRC）限制用户设计，低层硬件接口却长期被忽视。本文在AMD FPGA上发现一条新型提权路径，可让普通云客户绕过CSP防御，最终把设备永久锁死——首次提出针对FPGA的勒索攻击。

作者利用动态部分重配置在可重构分区中实例化ICAP（与AMD文档声称"ICAP必须位于静态设计区域"相悖），获得对SLR内配置数据的无限制读写；随后通过ICAP重配置MASTER_JTAG原语，将权限升级为远程JTAG访问，从而读取乃至编程eFuse阵列（AES/RSA密钥、FUSE_SEC、FUSE_CNTL寄存器），并可通过STARTUP原语改写启动flash（AMD确认，官方设计公告列出19款受影响器件）。

评估在本地VCU118板与AWS EC2 F1/F2实例上完成：通过ICAP注入15-LUT环形振荡器（禁用DRC规则LUTLP-1）可绕过时钟门控等Fence防御；读取FUSE_DNA可实现100%准确的设备指纹（AWS本对用户保密）；写eFuse可在受控本地环境完成勒索演示——一台F1实例可容纳8块FPGA（零售约4万美元/块，合计32万美元/实例），按300个唯一设备的保守估计，财务影响可达数百万美元。

该工作的贡献在于完整展示了"ICAP→JTAG→eFuse"的提权链，暴露了CSP忽略的低层硬件攻击面，勒索场景新颖且影响量化合理。局限也明显：攻击仅限SLR 2（shell关键逻辑位于SLR 0/1），eFuse写入未在云端实测，多租户FPGA场景目前仍属假设。与Albartus等人的ICAP研究相比，本文把理论威胁推进到可执行的云攻击，值得CSP与芯片厂商共同加固。

- **论文 PDF**: [Silicon Heist: (Ransom) Attacks for Cloud FPGAs via Privilege Escalation](https://www.usenix.org/system/files/usenixsecurity26-klix.pdf)

#### Design and Implementation of a Physical Implant Attack on the Boeing 737
- **作者**: Sam Crow, Stephen Checkoway, Patrick Mercier
- **方向**: 硬件与物理安全
- **解读**: 航空安全威胁模型常以"命运共同体+不可行性"论证把机内硬连线通信排除在范围外；作者论证"限时物理访问"现实可行：737 的 E&E 舱门从地面无梯子即可打开（10–15 秒），加上放置与关闭共约 60 秒；而美国持 SIDA 徽章的航空从业者超 140 万、2015 年机场出入控制事件达 2,500 起，地勤岗位门槛低，威胁并非空想。

作者在 737（NG/MAX，占现役机队 95%+）E&E 舱发现 Open Maintenance Connector（OMC），经 IFSAU 接入 FMC↔MCDU 的 ARINC 429 总线，其中继电器可切换 MCDU→FMC 按键路径。关键创新是"Bus Driver"攻击：植入体不经 37.5Ω 内阻直接以更强驱动电流（对抗时约 267mA/线，使合法发射器电阻耗散 2.67W）电气覆盖合法信号，并通过电流传感器（Allegro ACS70331）在覆盖同时解码被覆盖比特，无需割线即合成完整 AITM；ESP32@240MHz 用 ULP 协处理器在 100kbps 下做位级同步。

用真实 737 部件（GE 2907A4 FMC、2577F1 MCDU、IFSAU，均来自在役飞机）按波音布线图搭建测试台，端到端攻击可注入恶意按键（改航路点并代按 EXEC、篡改零燃油重量 ZFW 与假设温度影响起飞性能），用"触发模式"选择性改字或整屏覆盖隐藏 MCDU 显示；总预算不到 2 万美元、单博士生完成，并经波音测试台复现。同时证明 MIL-STD-1553 变压器耦合总线天然抵抗此类覆盖。

我的思考：优雅且令人警醒——不靠物理割线，纯电气层覆盖即可双向中介关键航电通信，60 秒的作业要求与隐蔽外形（可藏在 OMC 尘盖下、可选 Wi-Fi 中继）使威胁模型可信。作者明确声明不构成迫近威胁，与波音自 2020 年起持续披露合作，姿态负责。局限：Wi-Fi 穿舱未经实证、精确时序要求高、防御讨论（电阻移入连接器、1553 方案）尚未部署；该工作应推动整个行业重新审视"物理访问=可控风险"的默认假设。
- **论文 PDF**: [Design and Implementation of a Physical Implant Attack on the Boeing 737](https://www.usenix.org/system/files/usenixsecurity26-crow.pdf)

#### TAT: Attesting Trajectory Integrity of Industrial Robotic Arms
- **作者**: Chengtao Yao, Chengcheng Zhao, Peng Cheng, Jiming Chen
- **方向**: 硬件与物理安全
- **解读**: 工业机械臂的运动被篡改（改变生产逻辑、定位或动力学参数）可导致产品缺陷甚至人身伤害，但现有远程证明只覆盖控制流/数据流完整性（CFI/DFI），无法验证物理运动是否符合预期轨迹。难点有二：任务级指令语义不完整（同一命令对应多条可能轨迹），且控制器专有复杂、其内部派生状态不可信，无法对全控制栈做证明。

本文提出新的安全属性——轨迹完整性（Trajectory Integrity, TI），并设计 TAT 框架：编译期静态分析加离线测试生成"计时运动事件图"（TMEG），捕获运动的时空参考轮廓；运行时由事件测量引擎（记录事件时间戳及 CF/DF 行为）与旁路关节测量引擎（直接读电机编码器原始读数作为可信物理证据）协同采样；验证引擎重构实际轨迹，用绝对轨迹误差（ATE）与相对位姿误差（RPE）结合 4σ 阈值判定是否符合预期。

原型部署在开源六轴机械臂 mechArm 270-Pi（树莓派 4B 主控 + ESP32 轴控，OP-TEE 可信锚）。在 3 个开源与 7 个真实工业任务程序上，TAT 引入的内存开销最多 2.30%、执行时间最多 0.14%；相对仅含 CF/DF 的 OAT 基线仅额外增加 0.56% 内存与 0.03% 时间。对 PID 比例增益（50%–200%）与参考坐标系偏移（平移 1–5mm、旋转 1–5°）的篡改，ATE/RPE 检测率在多数场景达到 1.0。

我的思考：TI 把证明从"程序执行正确"推进到"物理运动正确"，旁路编码器测量避开对专有控制器的信任依赖，是 CPS 证明研究的自然延伸；开销控制在亚百分比级，实用性突出。局限在于原型基于开源平台，工业机械臂的部署可行性尚需探讨；4σ 阈值依赖参考轨迹统计，小幅异常运动的检测边界需谨慎；传感器欺骗与 DoS 不在威胁模型内。

- **论文 PDF**: [TAT: Attesting Trajectory Integrity of Industrial Robotic Arms](https://www.usenix.org/system/files/usenixsecurity26-yao-chengtao.pdf)

#### SoK: Security of Cyber-physical Systems Under Intentional Electromagnetic Interference Attacks
- **作者**: Qinhong Jiang, Yan Long, Youqian Zhang, Chen Yan and Xiaoyu Ji, Xiapu Luo, Kevin Fu, Jiannong Cao, Wenyuan Xu
- **方向**: 硬件与物理安全
- **解读**: 
故意电磁干扰（IEMI）攻击通过伪造电信号侵入"物理—数字"边界，可扰乱或控制自动驾驶、医疗、关键基础设施、IoT 等安全关键系统；但既有研究高度设备特定、攻击向量零散且缺乏统一比较，缺少基于模型的系统性理解，阻碍可迁移评估与跨学科防护协作。

该 SoK 分析 2000 年至 2025 年安全与 EMC 社区 80 余篇论文，提出分析框架把 IEMI 攻击分解为两个紧密耦合过程：IEMI coupling（恶意电磁能量如何注入电路，基于 EMC 理论）与 sample manipulation（注入能量如何扰动电路信号并传导至软件执行），并沿耦合接口（模拟走线、数字数据线、电源线）、耦合路径（容性/感性/电磁/传导）、幅度与时序控制等维度系统化攻击实例；防御侧则剖析 EMC 保护力被高估等迷思与"虚假安全感"，按耦合与采样操纵阶段梳理对策。

主要发现包括：真实攻击可行性评估常被忽视、EMFI 与 EMSI 存在收敛趋势、新攻击向量（如数字接口）出现、缺少闭环注入控制的反馈方法学；识别出自动化耦合接口发现策略缺失、缺乏主动同步对策等防御缺口；容性耦合可达约 7cm、感性约 15cm、电磁耦合理论上超 6 米；并发布开源 IEMI 研究数据库。

我的思考：该 SoK 的价值在于推动领域从"逐个发现脆弱实例"走向理论化建模，为安全、EMC、监管等社区建立共同语言与分层路线图。局限是它不提供可验证的新防御，且 80 余实例的归纳质量依赖数据标注一致性；电磁耦合的精确建模仍依赖设备特定参数，距离"预测新设备脆弱性"还有距离，但这恰是作者指出的下一步。

- **论文 PDF**: [SoK: Security of Cyber-physical Systems Under Intentional Electromagnetic Interference Attacks](https://www.usenix.org/system/files/usenixsecurity26-jiang-qinhong.pdf)

#### FABRICKED: Misconfiguring Infinity Fabric to Break AMD SEV-SNP

- **作者**: Benedict Schlüter

- **方向**: 硬件与物理安全

- **解读**: 机密计算（SEV-SNP、TDX）把 BIOS/UEFI 固件排除在信任基之外，但平台互联（AMD Infinity Fabric 的 Data Fabric）路由配置仍由不可信的 UEFI 完成，其安全影响此前未被研究。作者研究恶意平台固件能否通过篡改 Data Fabric 路由破坏 SEV-SNP，并发现可被软件攻击者利用的完整攻击链。

方法上，作者逆向主板固件发现：恶意 UEFI 可经 FICAA/FICAD 寄存器改写 Data Fabric 的 MMIO 路由规则（运行期仍可写），而 DRAM 路由寄存器被 PSP 锁定无法触碰；更关键的是实测发现路由判定顺序与文档相悖——MMIO 规则优先于 DRAM 规则。由此可将 PSP 写往 DRAM 的请求（如初始化 RMP 表的关键写）重定向到 I/O，使 RMP 保持攻击者可控的零值初始状态，进而在 SEV-SNP 初始化后仍可改写 RMP 条目，绕过其完整性强制。

在 Zen 5 EPYC 9135 平台上验证：修改 UEFI 跳过 BootDone/LockDF 的 PSP 调用并加 223 行内核补丁，即可在 SNP_INIT_EX 期间重定向 PSP 写；226 行内核模块对 guest context page 偏移 0x1F0 的加密块反复改写并尝试 SNP_DBG_DECRYPT，利用 AES 伪随机性平均 1.97 次尝试（1.59ms，最多 21 次）翻转 debug 位，获得任意内存读写；还可伪造 attestation 报告。AMD 已确认漏洞并计划分配 CVE-2025-54510（2025 年 8 月披露）。

我的思考：这是首个系统研究 Infinity Fabric 路由对 SEV-SNP 安全影响的工作，揭示"信任基之外的固件仍能破坏 TCB"的结构性缺陷，攻击纯软件、可在云内复现，且受害 VM 全程无感知，严重性很高。局限在于需控制 UEFI 固件（云环境门槛较高）、仅在 Zen 5 验证、对 Intel 不可行（检查机制闭源）。缓解方向（锁定 MMIO 路由寄存器、对 RMP 初始化写增加完整性校验）值得 AMD 尽快落地，也为其他 chiplet 平台的可信互联设计敲响警钟。

- **论文 PDF**: [FABRICKED: Misconfiguring Infinity Fabric to Break AMD SEV-SNP](https://www.usenix.org/system/files/usenixsecurity26-schlueter-fabricked.pdf)

### 移动与物联网安全（7 篇）

#### PrivacyShield: Relaying BLE Beacons to Counter Unsolicited Tracking
- **作者**: Florian Hofhammer, Daniele Antonioli, Mathias Payer
- **方向**: 移动与物联网安全
- **解读**:
离线查找网络（Apple Find My、Google Find My Device 等）让廉价易藏的 BLE 标签经附近手机上报位置，被滥用于跟踪。论文系统化分析反跟踪机制，归纳四个触发前提 PR1-PR4 及绕过攻击：改设备类型位冒充 iPhone（A1）、利用分离模式前约 30 分钟提醒窗口（A2）、高频轮换标识符（A3）、拆扬声器藏匿标签（A4），证明现有补丁式对策不足。

提出 PrivacyShield 防御性中继网络：嗅探器（AirGuard 扩展）被动采集 AirTag 信标上传服务器；ESP32 中继（开发板不足 1 美元）在任意高人流地点以更高频率重放信标，第三方手机把中继位置上报给 Apple，淹没真实报告。信标无认证、元数据不含位置、重放与原始广播不可区分，且不接触运营商 API，故无法被过滤或检测。

实验中 A/B 两地相距约 4km：2Hz 重放时 AirTag 稳定显示在中继位置，1Hz 时位置跳变，0.5Hz 时真实位置不再更新；即使真实位置周边设备 no≈70 而中继处仅 nr=1，提高频率仍可掩盖。观测到 iPhone 每 5-10 分钟上报并附置信度，Apple 丢弃无优势支持的矛盾报告。论文还提出缩短提醒窗口、全设备类型报警等建议。

把无线中继攻击反转为防御工具，不依赖运营商合作、对受害者即时生效，是"用户自救"思路的代表作。局限：位置掩盖是概率性的，依赖中继点人流与频率，空旷场景效果有限；根因（信标无认证）未消除，属缓解方案；中继他人信标的伦理与法律边界需谨慎。

- **论文 PDF**: [PrivacyShield: Relaying BLE Beacons to Counter Unsolicited Tracking](https://www.usenix.org/system/files/usenixsecurity26-hofhammer.pdf)

#### Missing, Present and Conflicting: A Large Scale Analysis of IoT Update Information in the EU Market
- **作者**: Swaathi Vetrivel
- **方向**: 移动与物联网安全
- **解读**:
IoT 设备发布后漏洞不断暴露，安全更新是唯一补救手段，但消费者购买时缺乏"设备支持多久"的可靠信息。FTC 小样本发现 89% 厂商页面未披露更新时长；欧盟 CRA（2027）、英国 PSTI（2024）、欧盟能源标签条例（智能电视强制）均要求披露，但市场实际披露缺乏大样本实证。论文首次大规模研究欧盟市场披露情况。

覆盖 EEA 30 国：经 Similarweb 识别 22 家本地零售商、7 个欧盟 Amazon 站点与 Temu（29 个区域版）；对 5 类设备（IP 摄像头、打印机、音箱、电视、手表）人工审查 4,480 个商品页，再爬取 29,587 页并抽查 1,325 页验证；最后对 272 台设备跨来源对比（零售商、厂商官网、EPREL 中央数据库），分层抽样确保厂商覆盖。

披露差异悬殊：荷兰零售商（受 ACM 监管）最多为 92% 设备列出时长（bol.com 33.1%-64%，coolblue.nl 17.8%-91.5%），Amazon 仅 0.4%（6 站仅 51 个日期），Temu 全部缺失；智能电视因欧盟条例覆盖 19.8%-100%，最常见时长 8 年。跨来源矛盾普遍：厂商官网披露仅 18%-50%，智能电视 34.7% 而 EPREL 约 80%；零售商普遍低于厂商（Google Nest 官网 5 年 vs bol.com 12-36 个月），同店内亦矛盾（Apple Watch Ultra 同款 30 链接中 13 个 24 个月、17 个 48 个月）。

数据证明"披露义务≠消费者可见信息"：法规能提升可见性（荷兰、智能电视），但缺统一标准与执行时信息碎片化、矛盾化，零售商"内部案例研究"式数据甚至误导消费者，是消费物联网治理的标杆研究。局限：聚焦欧盟热门站点；"哪个来源为真"只能提示无法裁决。政策含义明确：需标准化披露字段与强制数据源校验。

- **论文 PDF**: [Missing, Present and Conflicting: A Large Scale Analysis of IoT Update Information in the EU Market](https://www.usenix.org/system/files/usenixsecurity26-vetrivel.pdf)

#### Security and Privacy Analysis of Tile's Location Tracking Protocol
- **作者**: Akshaya Kumar
- **方向**: 移动与物联网安全
- **解读**: 

**问题**：Tile 是仅次于 Apple AirTag 的第二大众包位置追踪服务（2021 年 9 月售出超 4000 万台设备、42.5 万付费用户，后被 Life360 以 2.05 亿美元收购并接入亚马逊 Sidewalk），却从未提供协议或威胁模型的正式描述，其宣称的"网络传输匿名、只有你自己能看到 Tile 位置"等隐私保证值得检验；众包离线查找（OF）网络正被用于盗窃、跟踪乃至谋杀等犯罪。

**方法**：作者逆向工程 Tile 官方 Android 应用，重构标签注册、位置上报、BLE 广告生成、Scan and Secure 防跟踪与 Anti-Theft 防盗模式协议，并在"乐观假设"下（对不可知的私有服务端做有利于 Tile 的假定）分析其安全属性，同时实验验证：数天 BLE 抓包确认静态 MAC、90 天采集确认 privateId 循环、按算法复现 8640 个 privateId 等。

**结果**：Tile 服务器可持续收集所有用户与标签的明文位置（含静态 MAC），违背其匿名声明；Tile Mate 2022 广播静态 MAC，被动 RF 敌手可永久指纹追踪；8640 个 privateId 每 15 分钟轮换、90 天后重用，即使 MAC 随机化也可被长期关联；Scan and Secure 的"加密标识"实为伪随机 privateId；转让/无限共享不更新 authKey，撤销共享后仍可追踪原主人（作者指出亲密伴侣暴力场景）；Anti-Theft 模式（政府 ID+自拍验证、定罪罚款 100 万美元协议）可被规避且使标签对防跟踪扫描不可见。与 Apple/Google 方案对比，Tile 安全性显著更弱。

**我的思考**：本文贡献在于首个 Tile 全面安全分析，并提出"framing resistance（诬陷抵抗）"新定义、呼吁对 accountability 做形式化刻画，为 OF 网络"可追责、防跟踪、防盗三者难兼得"提供了珍贵案例。局限：私有服务端行为依赖乐观假设；社区信息去匿名化攻击因伦理未实测。领域影响上，为监管和平台设计（共享/转让后密钥轮换、防跟踪检测默认开启）提供了具体依据。

- **论文 PDF**: [Security and Privacy Analysis of Tile's Location Tracking Protocol](https://www.usenix.org/system/files/usenixsecurity26-kumar-akshaya.pdf)

#### PANGOLIN: Fuzzing Multilingual IoT Firmware with LLM-Driven Code Analysis
- **作者**: Zhipeng Jia and Xiaokang Yin, Shuitao Gan, Chao Zhang, JCSS, Hangtian Liu, Jiangan Ji, Jinglei Tan, Shengli Liu
- **方向**: 移动与物联网安全
- **解读**: 多语言 IoT（C/Python/Lua 混合后端，Cisco、华为、小米等已采用）中大量接口不对前端暴露、参数经多层解析呈复杂层级结构，而网络流量只能覆盖部分接口，单语言静态分析无法跨语言边界，LABRADOR 等既有 fuzzing 又依赖流量种子，难以发现隐藏接口与深层漏洞。

方法上，PANGOLIN 三阶段流水线：先用 LLM 分析 API 分发机制，构建树状"入口映射"（entry URI→handler 点，覆盖多级路由与间接调用）；再以剪枝后的多语言调用图（MCG）刻画脚本与二进制的混合调用链，由 LLM agent 跨语言生成参数规格；最后用响应驱动反馈纠正 LLM 幻觉与剪枝错误导致的规格偏差，并以规格指导节点选择、变异算子与能量分配，实现语义感知模糊。

在 8 家厂商 12 台多语言 IoT 设备上发现 2,856 个后端接口（其中 1,793 个前端隐藏）；共发现 68 个 0-day（命令注入、XSS、信息泄露、DoS、任意文件读写），是 LABRADOR（24 小时测试 23 个）的 2.96 倍，静态分析快 19.1×（合计 71.7 分钟对 1367.4 分钟）；45 个漏洞位于隐藏接口（EAGLEYE 仅 4 个）；全部上报并获厂商确认，31 个已分配编号。

LLM 驱动的接口/参数规格提取直击多语言 IoT 黑盒 fuzz 的核心痛点，响应反馈机制有效兜底 LLM 幻觉，工程实证扎实；局限是需取得固件代码（半白盒）、LLM 分析与响应验证带来额外成本、对纯黑盒无固件场景不适用；相较 LABRADOR/EAGLEYE 在隐藏接口与效率上显著领先，是多语言固件漏洞发现的重要推进。
- **论文 PDF**: [PANGOLIN: Fuzzing Multilingual IoT Firmware with LLM-Driven Code Analysis](https://www.usenix.org/system/files/usenixsecurity26-jia-zhipeng.pdf)

#### When Fun Turns Toxic: A First Look at Aggressive Advertising in Mini-games
- **作者**: Pei Chen, Geng Hong, Yicheng Qin, Huazhe Wang, Mengying Wu, Min Yang, Ziru Zhao, Yuanpeng Zhu, Tao Su
- **方向**: 移动与物联网安全
- **解读**: 小游戏已成为超级 App 生态的主流形态（2024 年中国市场规模约 55 亿美元），官方广告 API 给了开发者对广告时机、位置、频率的精细控制，被滥用于"攻击性广告"：打断游戏、劫持操作、无法关闭、诱导点击。这类行为虽非恶意软件，却滥用合法 API 绕过审核、操纵用户交互，构成系统性平台安全风险。

作者先分析 9 个支持小游戏的平台政策与开发者能力，归纳出 4 类 14 种攻击性广告行为；再设计静态分析框架 MAAD（面向市占率最高的 Cocos 引擎）：针对引擎耦合深、调用链跨文件跨语言、行为需结合交互上下文三大挑战，分别用模块提取+三层剪枝、逆向资源解码重建调用链、五维 Ad-behavior 模型加策略规则检测（辅以 XLM-RoBERTa 识别诱导性文本）判定攻击性。

在 100 个小游戏的 371 条人工标注行为上，MAAD 精确率 94.57%（854/903）、召回率 83.55%（310/371），超过 81% 的游戏在 10 分钟内完成分析。大规模测量覆盖微信（1,593）、Facebook（312）、QuickGame（171）共 2,076 个游戏：49.95% 的含广告游戏存在攻击性广告（微信 46.94%、Facebook 55.17%、QuickGame 62.62%），以打断型最普遍（44.48%）；甚至 8 款评论超 10 万的热门游戏亦受影响。更关键的是，94.09% 的绕过依赖云端动态控制触发器——审核期静默、上线后启用；已上报 456 款，43.32% 被平台下架。

我的思考：该工作首次把攻击性广告从"用户体验问题"重新框定为"能力滥用型系统安全风险"，49.95% 的普遍率与云端触发器绕过揭示了平台单轮审核机制的结构性盲区，证据充分。局限在于仅覆盖 Cocos 引擎（虽为最大份额），静态分析对动态 JS 分发与混淆存在漏报（召回 83.55%），诱导检测仅限文本；跨平台策略差异也使统一阈值与平台细则之间存在张力。
- **论文 PDF**: [When Fun Turns Toxic: A First Look at Aggressive Advertising in Mini-games](https://www.usenix.org/system/files/usenixsecurity26-chen-pei.pdf)

#### Cracks in the Walled Garden: Dissecting the Gray-Market of Unauthorized iOS App Distribution via Ad Hoc Sideloading
- **作者**: Yijing Liu
- **方向**: 移动与物联网安全
- **解读**: 
Apple 以严格代码签名与 App Store 强制分发构建信任模型，但专为开发者测试设计的 Ad Hoc 配置机制被滥用作旁加载通道：利用个人开发者证书与用户侧签名，把未授权应用伪装成测试构建安装，绕过应用审核。该实践已演化为连接证书倒卖、第三方签名工具与未签名 .ipa 分发的结构化灰色市场。

本文首次系统研究该市场（聚焦中国高度集成的服务模式）：通过用户视角收集，从社交媒体发现 62 个签名站点，借被动 DNS 数据扩展为 3,359 个活跃站点；逆向分析 12 个签名工具（IDA Pro）；从工具内置仓库提取 8,216 条 .ipa 条目并下载 2,654 个应用包。

结果显示：站点在 92 天内 DNS 解析超 75,000 次，仍高度活跃；证书流通形成多层产业，转售利润率最高达 3,000%；大多数签名工具基于 zsign 实现代码签名，部分利用 iOS 内核漏洞绕过签名校验；分发应用多数是正版应用的修改版，通过注入动态库实现 VIP 解锁、去广告等定制功能，带来未授权操作、敏感数据外泄与系统能力被利用的风险。

我的思考：该工作首次把"证书—工具—应用"完整链条量化，揭示 Ad Hoc 从测试通道演变为制度化灰色产业的过程，方法论（用户视角收集 + 被动 DNS 扩展）可复制到其他生态。局限是仅聚焦中国市场，全球碎片化服务未被覆盖；对应用修改的分析偏静态（动态库注入），对签名工具本身的恶意性评估有限；已向 Apple 报告并提出三方缓解建议，但落地效果尚待检验。

- **论文 PDF**: [Cracks in the Walled Garden: Dissecting the Gray-Market of Unauthorized iOS App Distribution via Ad Hoc Sideloading](https://www.usenix.org/system/files/usenixsecurity26-liu-yijing.pdf)

#### Khost: KVM-based Near Native MCU Firmware Rehosting

- **作者**: Chunlin Wang

- **方向**: 移动与物联网安全

- **解读**: MCU 固件构成 IoT 安全的关键层，但固件与定制硬件紧耦合、设备资源有限，动态分析（如模糊测试）难以规模化部署；现有 rehosting 框架要么用 QEMU 式翻译模拟导致高开销，要么用 HLE/原生执行牺牲执行范围与精度。

Khost 基于 KVM 硬件虚拟化实现近原生、范围保持的 rehosting：辅助页表创建虚拟 MMU 桥接 MCU 与高性能处理器（无 MMU 与 MMU、不同指令集）的内存管理差异；轻量扩展 CPU 处理 MCU 特定操作；软件 NVIC 正确管理异步中断；MMIO 监视器快速交互并兼容现有外设模型；固件 wrapper 支持覆盖率采集与灵活配置现有 fuzzing 引擎。

CoreMark-PRO 与 Simbench 基准上，相比 QEMU 复杂计算任务开销降低 90.0%–95.5%、MCU 系统级操作最多降低 98.5%；12 个真实固件上 fuzzing 吞吐最高提升 197.5×（相对 HALucinator），基本块覆盖率提升 6×，相对 Fuzzware 分别为 68.6× 与最高 3.4×；并发现 5 个此前未知的漏洞。

我的思考：用 KVM 硬件虚拟化替代软件模拟/翻译，在保持完整执行范围的同时获得近原生性能，思路正确、收益显著且开源可复现。局限：依赖支持 KVM 且 32 位向后兼容的 ARM 高性能平台，辅助页表对无 MMU 场景的建模是近似；12 个固件的评估规模有限，5 个新 bug 的细节有待披露；外设仍需人工配置模型，自动化程度与 HLE 方案相当。
- **论文 PDF**: [Khost: KVM-based Near Native MCU Firmware Rehosting](https://www.usenix.org/system/files/usenixsecurity26-wang-chunlin.pdf)

### 云与供应链安全（3 篇）

#### The Art of Hide and Seek: Making Pickle-Based Model Supply Chain Poisoning Stealthy Again
- **作者**: Tong Liu and Guozhu Meng, Peng Zhou, Zizhuang Deng, Shuaiyin Yao and Kai Chen
- **方向**: 云与供应链安全
- **解读**: pickle 反序列化可致任意代码执行，但 PyTorch 等框架仍将其作为模型序列化协议，Hugging Face 等模型平台放大了供应链投毒的现实影响。现有扫描器（PickleScan、ModelScan、HF 与 ProtectAI 在线扫描）对投毒面的理解不完整，可被绕过。

作者首次系统性披露 pickle 模型投毒面，分两层：模型加载面——静态分析+深度审计在五个基础框架（NumPy、Joblib、PyTorch、TensorFlow/Keras、NeMo）中发现 22 条 pickle 加载路径，其中 19 条完全未被现有扫描器覆盖；并将"扫描器对畸形输入异常退出"形式化为异常导向编程（EDP），发现 9 个 EDP 实例。危险函数面——以静态数据流+LLM 语义推理流水线（PICKLECLOAK）端到端自动发现可复用 gadget 并生成利用。

结果：19/22 加载路径可绕过全部扫描器，7/9 个 EDP 实例可绕过所有扫描器；发现 133 个可利用 gadget，接近 100% 的绕过率，即使对最强扫描器仍保持 89% 绕过率。发现已负责任披露给 NVIDIA、Keras、Protect AI 与 PickleScan 等厂商，并获得 ProtectAI MFV 计划的 1.2 万美元赏金。

我的思考：首次把投毒面系统化——加载路径、扫描器异常、gadget 三层都给出可复现的绕过，是一把双刃剑但为防御指明方向：扫描器需覆盖全部加载路径、强化异常恢复、对 gadget 做语义级分析而非黑名单匹配；局限在于扫描器版本快速迭代、绕过存在时效性，且 LLM 推理引入一定不确定性。与仅盯 torch.load 的既往工作相比，其"polyglot 文件格式+异常路径"的视角解释了为何攻击能长期隐身。

- **论文 PDF**: [The Art of Hide and Seek: Making Pickle-Based Model Supply Chain Poisoning Stealthy Again](https://www.usenix.org/system/files/usenixsecurity26-liu-tong.pdf)

#### Trustworthy and Confidential SBOM Exchange
- **作者**: Eman Abu Ishgair, Chinenye Okafor, Marcela S. Melara, Santiago Torres-Arias
- **方向**: 云与供应链安全
- **解读**: SBOM 已成软件供应链透明度的监管要求，但企业出于知识产权与漏洞信息敏感性，不愿向所有消费者公开 SBOM 中的全部元数据；现有交换机制（NDA、受限仓库、发布订阅系统）缺乏可扩展性与互操作性，且无法保证 SBOM 被重新分发后仍保持完整性与访问控制。透明度与保密性之间的张力成为 SBOM 落地的主要障碍。

Petra 通过三层设计化解该矛盾：数据层将 SBOM 分解为树状结构（SBOM tree），支持跨格式（SPDX/CycloneDX）组合与再分发；保密层用 CP-ABE 按节点策略选择性加密，属性绑定组织身份与时间窗，允许按需删节（redaction）；可信层在删节后的树上构建 Merkle 树，配合 sameness proof 与 membership proof，使任何一方都能密码学审计"删节版与明文版一致"且"未遗漏字段"。系统定义生成器、生产者、分发者、消费者、验证者与 KMS 六种角色。

原型评估显示：交换删节版 SBOM 平均仅增加不到 1KB 开销，解密在 SBOM 查询总开销中占比不超过 1%；存储开销平均增加约 13%（IP 策略约 12%，漏洞策略约 51%）；加解密与成员证明平均 <1 秒、最坏 2 秒。在 3,380 个真实 SBOM（平均 242 个包）的语料上验证了可行性，实现仅约 1,600 行 Python 与 190 行 Rust。

我的思考：Petra 首次把"可审计的删节"内建到 SBOM 结构中，使保密性、完整性与互操作性在跨组织再分发后依然成立，相比依赖可信仓库或区块链基础设施的方案更轻量、更易集成，并已与 ProtoBOM/bomctl 生态联动，落地信号强。局限在于访问策略本身仍泄露字段名级别的结构信息，不隐藏 SBOM 的树形关系；且不解决 SBOM 内容准确性语义（只保证密码学完整性）。KMS 成为单点信任与撤销枢纽，其安全假设值得进一步讨论。

- **论文 PDF**: [Trustworthy and Confidential SBOM Exchange](https://www.usenix.org/system/files/usenixsecurity26-abu-ishgair.pdf)

#### When Updates Backfire: A Black-Box Security Analysis of Desktop Software Update Mechanisms

- **作者**: Jie Wan

- **方向**: 云与供应链安全

- **解读**: 软件更新是修补漏洞的主要通道，但也引入高价值攻击面——NotPetya、ShadowHammer、SolarWinds 等重大供应链事件均源于更新流程失守。然而桌面更新客户端大多闭源复杂，缺乏系统性黑盒评估，既有研究多集中于 IoT 与车载 OTA。作者提出 UpdSight，以 MitM 模拟在真实网络对抗设定下自动化测试闭源桌面软件更新安全。

方法上构建五模块黑盒流水线：Update-Initiator 脚本化安装并驱动原生更新流程；Domain-Tracer 用 DNS 重定向把更新流量引入受控环境；MitM-Insight 拦截并分析更新通信；Response-Crafter 伪造恶意响应；Exploit-Validator 确认载荷是否被执行。并定义四条安全规则（安全传输、元数据完整性、载荷真实性、路径完整性）与五类攻击类型。

对 85 个常用桌面应用端到端测试（120 个候选中 85 个可触发更新流量）：发现 22 个可利用漏洞、分布于 18 个应用，16 个获厂商确认，9 个已修复，5 个 CVE 覆盖 8 个产品（含 Lenovo 系列共享的 CVE-2025-10495、IDM CVE-2025-56231、GOG Galaxy CVE-2025-56232）；按类型分，清单操纵最多（10 例）、路径遍历 6、信息泄露 3、安装器劫持 2、降级 1；22 例全部违反安全传输规则、10 例违反元数据完整性；3 个漏洞在报告后一个月内被修复。

我的思考：这是对桌面更新生态规模最大的一次黑盒安全普查，实证揭示了未签名清单、弱回滚检查、脆弱解压逻辑等反复出现的设计缺陷——攻击者经更新通道即可获得代码执行，且同一开发团队产品共享不安全更新组件导致漏洞成簇出现。局限在于黑盒方法无法覆盖全部更新逻辑、需受控网络环境、登录依赖与纯 Web 类应用无法测试。它为闭源生态的更新安全审计提供了可复用的黑盒方法论。
- **论文 PDF**: [When Updates Backfire: A Black-Box Security Analysis of Desktop Software Update Mechanisms](https://www.usenix.org/system/files/usenixsecurity26-wan.pdf)

### 认证与访问控制（2 篇）

#### The State of Passkeys: Studying the Adoption and Security of Passkeys on the Web
- **作者**: Louis Jannett, Andreas Mayer and Maximilian Westers, Vladislav Mladenov, Christian Mainka, Jörg Schwenk
- **方向**: 认证与访问控制
- **解读**: Passkey 基于 FIDO2/WebAuthn，被视为抗钓鱼的下一代认证方式，但各网站（依赖方 RP）如何实现与校验 passkey 一直缺乏规模化安全评估，而 RP 侧校验正是安全的关键。

作者构建 PASSKEYS-RADAR：融合 12 个社区目录、Tranco 1M、CrUX 18M 与互联网档案馆历史数据，自 2021 年起持续追踪，扫描发现的 RP 比社区目录合计多约 125%，共识别 872 个启用 passkey 的网站；并在 208 个网站上注册测试账号分析管理行为。同时开发 PASSKEYS-ATTACKER：浏览器扩展+虚拟客户端/认证器+数据库，可在协议每步精确篡改 WebAuthn 消息，实现 15 类攻击（其中 10 类为新增）、28 种检测方法。

对 103 个 RP 的评估显示：没有一家通过全部安全校验；18 个存在高危（CVSS 9.8 级）漏洞，如 5 个网站完全跳过签名验证、14 个存在凭据覆盖攻击；53 个存在至少一个高严重度漏洞；全部站点均请求了弃用算法，80 个（38%）将 passkey 作用域扩到顶级域，埋下子域接管隐患。

我的思考：这是首个对真实 passkey 网站的大规模主动安全评估，把规范中的安全考虑转化为可复用的攻击目录与半自动工具，工程价值突出；局限在于 103 个样本需人工注册（反自动化机制所致）、攻击者模型假设浏览器/认证器可信；与仅有 7 个预置用例的 WebDevAuthn 相比，攻击面覆盖翻倍，其"规范→攻击类型→CVSS 量化"的映射方法值得后续合规审计借鉴。

- **论文 PDF**: [The State of Passkeys: Studying the Adoption and Security of Passkeys on the Web](https://www.usenix.org/system/files/usenixsecurity26-jannett.pdf)

#### Why Johnny Adopts Identity-Based Software Signing: A Usability Case Study of Sigstore

- **作者**: Kelechi G. Kalu; Sophie Chen; Santiago Torres-Arias and James C. Davis
- **方向**: 认证与访问控制
- **解读**: 软件签名是供应链完整性与认证最强的手段，但OpenPGP等传统密钥管理工具密钥管理负担重、可用性差。以Sigstore为代表的新一代身份基签名工具用短期密钥与外部身份提供方（OIDC）自动化这些环节，然而其可用性及对实际采用的影响缺乏实证。本文提供首个身份基签名工具的可用性研究。

作者对Sigstore开展案例研究：17名来自13个组织的行业安全专家进行约50分钟半结构化访谈（18场访谈，1人因对签名工具了解不足被排除），采用探索性主题分析并映射到Cresswell形成性可用性框架（技术/社会-人/组织/宏观环境四因素）；另发起验证性调查，但仅获13份回复、7份完整。

研究发现：身份基工具确实化解了传统密钥管理的痛点——优势集中于易用性（8人提及，如"一条命令签名、一条命令验证"）、OIDC身份管理、短生命周期证书、透明日志审计与生态兼容性；但采用仍受制于集成灵活性（GitLab/Jenkins等）、透明日志不适合私有/离线部署、速率限制与延迟、文档与私有实例成本、缺乏专职支持等。采用驱动包括前工具（GPG/Notary）痛点、监管标准（如EO-14028）与社区信任（CNCF治理）；非采用主因是组织政策、第三方风险与隐私担忧。Sigstore发布13个月内即录得4,600万次工件签名，并被PyPI、Maven、NPM及Autodesk、Verizon等采纳。

该研究填补了身份基签名可用性证据空白，指出组件成熟度不一、集成灵活性是可缓解的核心痛点；作为同类工具模板，结论可外推至OpenPubKey等系统。局限：样本小、依赖自我报告，调查验证未成规模——作为形成性研究，其价值在于为后续量化研究提供假设。
- **论文 PDF**: [Why Johnny Adopts Identity-Based Software Signing: A Usability Case Study of Sigstore](https://www.usenix.org/system/files/usenixsecurity26-kalu.pdf)

### 数字取证（2 篇）

#### Stayin' Alive: How Global Stolen Data Markets Thrive on Telegram
- **作者**: Tina Marjanov, Taro Tsuchiya, Konstantinos Ioannidis and Jack Hughes, Nicolas Christin, Alice Hutchings
- **方向**: 数字取证
- **解读**: 

**问题**：被盗数据是垃圾邮件、钓鱼、账户劫持、身份盗用等网络犯罪的催化剂，研究其交易社区有助于提前防御。匿名市场与论坛传统上是主要交易场所，但门槛高；Telegram（约 10 亿活跃用户）因易安装、抗封杀、隐私性强而成为新兴聚集地，其运营模式与规避监管的机制尚不清楚。

**方法**：作者用 12 种语言的关键词搜索（来源含学术文献、行业报告与犯罪论坛术语）、CrimeBB 论坛引用及雪球采样发现候选频道，约每 10 天一轮人工筛选，历时一年纵向收集 1521 个频道、1400 万条消息与 360 万共享文件；人工+LLM 辅助标注 6 类被盗数据；用区间删失的 Cox 比例风险模型分析频道寿命、三类面板模型分析成员增长，并做定性分析规避机制。

**结果**：超过 99% 的频道不出现在既有通用 Telegram 数据集中；数据分布为凭证 22%、infostealer 日志 21%、卡信息 19%、数据库 18%、个人信息 14%、证件 4%，日志份额从 2021 年约 15% 升至 2022 年 30% 且持续增长。大量频道使用非英语（中文、西语、葡语、俄语、波斯语）；6.5M 个 BIN 中阿根廷占 65.3%、美国 17.1%。155 个"门户频道"（gateway）链向 91% 的私有频道，被链接频道存活更久、增长更快（25 人/天 vs 9 人/天）；Telegram 2024 年 9 月宣布配合执法后封杀风险显著上升。凭证"云"订阅约 $40–70/周、$800+ 终身。

**我的思考**：本文提供目前最大的 Telegram 被盗数据纵向数据集之一，首次量化 gateway 机制与平台政策冲击，发现 gateway 可作监管"咽喉点"，有直接操作价值。局限：人工筛选存在选择偏差；BIN 计数含重复卡号（去重后 64.6k）；无法直接量化实际危害。领域影响：为威胁情报、执法与平台治理提供扎实证据，延续并扩展了 Christin 团队的地下市场测量传统。

- **论文 PDF**: [Stayin' Alive: How Global Stolen Data Markets Thrive on Telegram](https://www.usenix.org/system/files/usenixsecurity26-marjanov.pdf)

#### FIRA: Enabling Automatic Forensic Investigation of Unmanned Aerial Vehicles

- **作者**: Yizhi Huang, David Oygenblik, Runze Zhang

- **方向**: 数字取证

- **解读**: 无人机（UAV）常在空中用在线学习持续调整 ML 模型，但也暴露于部署期投毒攻击（如目标跟踪模型被诱骗学习后门触发图案），可能导致坠毁。坠机后 UAV 往往物理不可回收，机载带宽受限（典型 0.25–5 Mbps）使更新后的模型无法完整回传，无法判断坠毁是否由在线学习导致。既有固件取证需物理接触、模型分析需最新模型，均不适用。本文提出 FIRA，首个自动建立"模型→组件"因果链的 UAV 取证技术。

方法上，FIRA 分三阶段：飞行前构建完整因果图（FCG），同时分析 UAV 固件二进制与地面模型；飞行中因果数据传输，利用梯度贡献不均的特点，仅按输出类别贡献度回传少量神经元分组权重，以最小带宽维持模型同步；坠毁后时间线重建，用遥测与同步模型定位具体哪次模型调用致坠，并支持白盒分析确认投毒。

在基于 OpenUAV+PX4+ArduPilot+Gazebo+ROS2 的 48 个坠毁场景上，FIRA 以 95.8% 准确率正确归因因果链与坠毁原因；仅传输 5% 的神经元分组即可检测投毒。

我的思考：本文填补了"ML 系统取证"与"UAV 事故调查"之间的空白，把模型投毒检测从"模型本身"扩展到"模型-固件-遥测"的系统因果视角；95.8% 归因准确率与 5% 神经元传输的带宽效率使其在受限信道下具备工程可行性。局限：评估基于仿真与构造场景，真实坠毁中的遥测损坏未充分建模；假设通信信道安全。

- **论文 PDF**: [FIRA: Enabling Automatic Forensic Investigation of Unmanned Aerial Vehicles](https://www.usenix.org/system/files/usenixsecurity26-huang-yizhi.pdf)

### 无线与通信安全（5 篇）

#### LPG: Raise Your Location Privacy Game in Direct-to-Cell LEO Satellite Networks
- **作者**: Quan Shi, Liying Wang, Prosanta Gope, Qi Liang and Haowen Wang, Qirui Liu, Chenren Xu, Shangguang Wang and Qing Li, Biplab Sikdar
- **方向**: 无线与通信安全
- **解读**:
直连手机（D2C）低轨卫星采用多租户模式，MNO 租用 SNO 基础设施，把 RAN/核心网迁上卫星后，SNO 可见位置、MNO 掌握身份，位置隐私被耦合暴露；跨国监管与紧急服务又需灵活披露粒度。现有方案（LOCA、PGPP）假设地面连续连接与充足算力，且是二元隐私模型，不适于资源受限、高速运动的卫星。

LPG 在 NAS 层实现身份-位置解耦：DualToken 双令牌（SNO 令牌用 RSA 匿名令牌绑定运营参数；UE 令牌由 SIM 强制一次性、不可链接）、OAKA 离线认证与密钥协商（ECIES、卫星本地无状态验证、周期重附着加随机时序保证会话不可链接）、SMTs 支持用户以可配置粒度（H3 网格 res 0-15）生成零知识位置证明 ZKLP、SCAC 用协作式 ZK-SNARK 在地面服务器生成聚合结算证明防超额计费。

在真实在轨卫星 BUPT-3（COTS 硬件）与商用手机上实现，摘要称开销极小、切实可行，并给出不可伪造、不可链接、互认证、Dolev-Yao 安全及 SCAC 健全性等定理证明。（提取文本止于安全分析，实验定量数据未含。）

首个协议层 D2C LEO 位置隐私方案，工程验证扎实，可配置披露是对二元隐私模型的实质改进。局限：半诚实不共谋威胁模型、物理层波束定位仍可泄露区域、SIM 可信元件假设、以聚合混淆换轨迹不可重建。为星地融合网络隐私设计树立了协议层范本。

- **论文 PDF**: [LPG: Raise Your Location Privacy Game in Direct-to-Cell LEO Satellite Networks](https://www.usenix.org/system/files/usenixsecurity26-shi-quan.pdf)

#### Sliding into the Flight Deck's DMs: Practical Message Attacks on CPDLC
- **作者**: Mehdi Ziazi, Khalid Aleem, Harshad Sathaye, Martin Strohmeier
- **方向**: 无线与通信安全
- **解读**: CPDLC（管制员-飞行员数据链通信）已成为高密度与越洋空域管制的关键设施，但协议无加密、无认证、无密码学完整性，主要依赖协议复杂度与模糊性设防。既往研究多为应用层理论分析，从未证明真实可行的攻击。

作者对 ATN B1/VDL2 全协议栈做安全分析：逆向出此前未文档化的连接建立流程、ATN 校验和与 PM-CPDLC 校验和，并以 SDR 实现 cpdlc-gs——首个全栈恶意地面站。拒绝服务方面提出四类注入攻击，其中 AVLC U DISC 广播拆链无状态、不受竞态条件影响、确定可靠，可瞬间断开无线范围内所有飞机的数据链；另实现恶意地面站（RGS），按状态机逐步完成 GSIF 广播、AVLC/X.25、ES-IS、IDRP、COTP、ATN CM 登录与 PM-CPDLC 会话建立。

与瑞士空管 Skyguide、EUROCONTROL 及航电厂商合作，投入超 20 万美元认证航电设备（Universal Avionics UNS-1Ew FMS 与 UniLink UL-801 CMU），在 Faraday 笼内经真实 VDL2 链路（11.5 公里外测试地面站）验证攻击：RGS 可下发 UM79 直飞、UM21/UM20 爬升/下降等伪造指令，诱导偏航、进入受限空域或造成失去间隔；广播 DoS 无论应答机地址为何都会拆链。

我的思考：首次用真实认证航电把 CPDLC 攻击从理论推向实操，证明"隔离的流氓地面站"即可构成实质威胁；数字指令缺乏语音的自然线索、飞行员在高负荷下近乎无条件执行，使伪造清关指令极具危险性。局限在于竞态类 DoS 时序敏感、实验在自有测试环境进行；其更大价值在于警示航空数据链安全标准化进程严重滞后，加密与认证应被列为优先议题。

- **论文 PDF**: [Sliding into the Flight Deck's DMs: Practical Message Attacks on CPDLC](https://www.usenix.org/system/files/usenixsecurity26-ziazi.pdf)

#### Semantics Over Syntax: Uncovering Pre-Authentication 5G Baseband Vulnerabilities
- **作者**: Qiqing Huang and Xingyu Wang, Wanda Guo and Guofei Gu, Hongxin Hu
- **方向**: 无线与通信安全
- **解读**: 5G UE 在认证与完整性保护建立前就要处理下行 RRC 配置消息，此前 OTA 测试聚焦语法无效输入（随机/语法变异）；作者证明语法有效但违反规范字段约束或跨字段依赖的"语义不一致"消息，能驱动基带进入非法状态、触发断言失败或调制解调器崩溃，揭示预认证阶段被忽视的攻击面。

CONSET 将约束分为四类：字段值范围与字段存在性（从 TS 38.331 的 ASN.1 确定性提取，含 Need M/Need R 语义），IE 内与 IE 间依赖（散布在 38.2xx 规范散文里）。后者以"证据受限"方式交给 LLM（GPT-4o，温度 0）：只喂 ASN.1 片段+规范句子的证据包，产出 DSL 规则再经确定性门过滤，共处理 11,005 个字段对、得到 217 条候选规则；突变引擎做最小编辑并用 pycrate 重编码，保证每个用例语法有效且归因于单一约束。

商业手机上（10 款、4 个芯片族）经 SDR 空中注入发现 7 个此前未知漏洞，其中 3 个高危 CVE（MediaTek：CVE-2025-20644/20666/20703），影响 64 个芯片型号、超过 542 款商用手机；开源 OAI UE 上触发 29 个可复现崩溃点（摘要称 46 个，正文细分为范围 16/存在性 5/IE 内 8/IE 间 3），4 个修复已上游合并、1 个 CVE（CVE-2025-63356）。对比穷举枚举：约束引导仅 1,458 个输入（0.76 天）vs 枚举 30,600 个（15.94 天），并证明 86.4% 的 IE 内规则依赖跨文档证据。

我的思考：把"语义一致性"从规范散文系统化为可执行测试的高质量流水线，LLM 被严格限制在证据范围内、用 DSL 规范化+确定性校验抑制幻觉，是可复现的规范→测试范式。注意摘要（46）与正文（29）崩溃点数不一致，属论文自身瑕疵。局限：以 DoS 为主、仅覆盖预认证 RRCSetup、未评估认证后阶段；对民用 5G 基带安全研究是重要补充，与 5Ghoul 等语法 fuzzer 形成互补。
- **论文 PDF**: [Semantics Over Syntax: Uncovering Pre-Authentication 5G Baseband Vulnerabilities](https://www.usenix.org/system/files/usenixsecurity26-huang-qiqing.pdf)

#### WILD Attack: Stealthy Undermining of Wi-Fi-Based Geolocation Through Remote Crowdsourced Data Injection

- **作者**: Changjia Zhu

- **方向**: 无线与通信安全

- **解读**: 传统 Wi-Fi 定位系统（WPS）欺骗攻击需物理接近受害者并持续发射异常信号，隐蔽性与持久性差，未引起足够重视。作者把矛头转向 WPS 核心基础设施——位置查找表（LLT）：LLT 依赖众包数据维护，攻击者能否远程注入伪造报告，诱导提供商以伪造数据覆盖合法数据，实现隐蔽且持久的定位破坏？

方法上，攻击者在自选地点用低成本 ESP32/软件无线电广播目标 AP 的 BSSID 伪造信标，使附近真实设备按正常众包流程上报"目标 BSSID+攻击者位置坐标"；并用 Hop-by-Hop Expansion 与 Last-Hex Enumeration 两种策略远程获取受害者周边 90% 以上的目标 BSSID；并实测四家提供商对冲突数据的消解策略加以利用。

结果显示四家均接受伪造报告但策略各异：Google"多数胜"→条目篡改（约 11 天，停止后至少 4 周不恢复）；WiGLE"每报计数"→篡改（<30 分钟，持续数月）；Apple"零信任"→条目删除（仅 3 天，超 2 个月不恢复）；A-Map 删除（2 天，持续 2–3 周）。三个案例证明现实影响：塔林 10km 半径内可发现 262,073 个 BSSID，单块约 1 美元的 ESP32 可模拟约 50 个 BSSID，城市级定位 DoS 仅需约 5,242 美元硬件；GPS 精度约 115m 而 WPS 约 12m，设备优先 WPS，网约车定位可被误导数公里；还能向无 GPS 设备投递精确伪坐标。

我的思考：这是首次攻击 WPS 众包数据源头（LLT）的工作，攻击无需接近受害者、可远程发起、效果持续数周至数月，改变了"WPS 欺骗需现场操作"的认知，也反驳了"GPS 总能保证定位准确"的常识。局限在于仍需在自选地点部署信号设备、依赖各家冲突消解策略、出于伦理未对真实城市做破坏验证。缓解方向（报告可信度评分、位置一致性校验、众包来源验证）应成为提供商的共同义务。

- **论文 PDF**: [WILD Attack: Stealthy Undermining of Wi-Fi-Based Geolocation Through Remote Crowdsourced Data Injection](https://www.usenix.org/system/files/usenixsecurity26-zhu-changjia.pdf)

#### E2E-AKMA: An End-to-End Secure and Privacy-Enhancing AKMA Protocol Against the Anchor Function Compromise

- **作者**: Yueming Li, Long Chen, Qianwen Gao, Zhenfeng Zhang
- **方向**: 无线与通信安全
- **解读**: 3GPP AKMA（TS 33.535）让应用服务商把用户认证委托给移动运营商，但承担认证的 AAnF 位于 5G 核心网边界之外，一旦被攻陷即可冒充用户、窃取会话密钥实施中间人攻击；同时 SUPI 暴露给 AF 会关联用户真实身份，威胁 TR 33.835 中的 #3/#5/#6/#7/#16 关键问题，而既有 AKMA+ 在 AAnF 被攻陷时仍可被冒充。

E2E-AKMA 的核心是新的两方 oblivious 签名（2POS，基于 Paillier 同态加密与 ECDSA）：UE 持私钥分片 SK1、AAnF 持由主密钥派生的 SK2，交互签名但只有 UE 得到最终签名，且 AAnF 无法区分所用密钥（P2-不可区分性）；注册采用 trust-on-first-use，SUPI 以可重随机化加密标签交给 AF 并再随机化防追踪，会话密钥 ssk=H(C||σ2POS)，核心网可经 Trace 解密恢复 SUPI；形式化模型覆盖认证、密钥不可区分、AF/AAnF 不可链接与可追踪性。

用真实手机与云服务器实测，E2E-AKMA 登录耗时 464ms，相比原 5G AKMA 仅约 9.4% 开销；同时保持无需动态机密数据库、无需额外硬件（仅标准 SIM 卡）的既有优点，在 AAnF 完全被攻陷的威胁模型下仍保证 UE-AF 端到端安全与匿名性。

首次在锚函数被攻陷假设下给出端到端安全且可追踪的 AKMA 方案，2POS 的 oblivious 属性设计有新意，并有形式化安全证明；但 9.4% 仅为登录延迟开销，Paillier 与零知识证明在真实网络中的吞吐影响需更多实测，trust-on-first-use 的首次绑定与 5G AKA 无前向保密仍需在部署中权衡。

- **论文 PDF**: [E2E-AKMA: An End-to-End Secure and Privacy-Enhancing AKMA Protocol Against the Anchor Function Compromise](https://www.usenix.org/system/files/usenixsecurity26-li-yueming.pdf)

### 数据安全（1 篇）

#### Lost in Blockchain Address Misuse: Hidden Cross-Platform Risks and Their Security Impact

- **作者**: Zhenzhe Shao; Jiashuo Zhang; Zihao Li; Daoyuan Wu; Chong Chen and Yiming Shen; Lingfeng Bao; Yanlin Wang; Jiachi Chen
- **方向**: 数据安全
- **解读**: 区块链地址是用户交互与资产安全的核心接口，但因疏忽或认知不足，用户常把非合约地址（NCA）当合约地址使用（CA误用），或与私钥已泄露的EOA交互（EOA误用），造成系统性资产损失。此前缺乏对这一风险类别的系统定义与大规模量化，本文首次对其进行端到端测量。

作者从GitHub（63,004个仓库）与Stack Exchange收集地址语料，得到1,031万候选地址与1,630万暴露私钥；对以太坊与BSC上关联交易进行链上分析，结合启发式规则、交易模式分析与符号执行检测误用，并通过分层随机抽样人工验证精度（两位标注者Kappa系数0.868）。

检测识别出65,340个高风险地址实例、约250万笔相关交易，损失合计约127k ETH与17.7k BNB，按2025年5月价格（4,408美元/ETH、847美元/BNB）价值超5.748亿美元；整体精度99.11%（CA误用ETH 97.62%、BSC 98.94%，EOA误用ADDR子类100%）。其中GitHub泄露私钥（KEY子类）占EOA误用损失的95%以上（103,402.53 ETH）。评估还揭示两个此前未披露的攻击向量：跨链地址复用（测试网部署合约、主网同址种植恶意合约，469例，损失3,446.37 ETH与431.79 BNB，89.76%为直接转账逻辑）与EIP-7702委托接管（17,270例，前三恶意合约占委托的78.71%）。

这是首个系统化的地址误用测量，规模与精度验证俱佳，且揭示攻击者已主动利用该风险获利，警示意义强。对比前人工作（8.65M密钥、ETH损失约700万美元），本文发现的问题正随时间恶化。局限：检测依赖地址语料来源，85%以上候选地址尚未被误用交互，潜在"地雷"规模巨大；EIP-7702向量尚处早期，损失金额（25.86 ETH）相对有限。

- **论文 PDF**: [Lost in Blockchain Address Misuse: Hidden Cross-Platform Risks and Their Security Impact](https://www.usenix.org/system/files/usenixsecurity26-shao-zhenzhe.pdf)

### 社会工程与人类因素（8 篇）

#### "Your imaging may be stone-cold normal, but if they look sick, they're going to get admitted": An Investigation of Clinicians' Perceptions of Impact & Likelihood of Security Failures
- **作者**: Ronald E. Thompson III and Hamza Khalid; Hilary Fisher; Rhea Votipka; Daniel Votipka
- **方向**: 社会工程与人类因素
- **解读**: 医疗网络攻击已被定性为患者安全问题（勒索软件曾使单一医疗系统损失超 1 亿美元），但安全控制很少针对临床环境的独特性设计。现有研究聚焦技术漏洞或特定专科人群，缺乏对一线临床医生安全感知的大规模理解，而医生恰是访问数据与决策的核心环节。

方法为混合研究：先对 12 名美国住院/ICU/急诊临床医生做半结构化访谈（2024 年），再对 303 名美、英、加临床医生做问卷调查（Prolific 招募，以 BLS 知识筛查+人工验证剔除不合格者）。以 CIA 三元组为透镜，围绕 9 类临床技术设计保密/完整/可用三类失败场景，评估感知危害与可能性；用混合效应有序逻辑回归分析，开放题采用 Krippendorff α 迭代编码。

关键发现：保密失败被认为最可能（51.4%），且是唯一有对应安全控制与培训的类型，但控制常被移动性与紧急场景打乱；完整性失败危害最高（OR 54.4, p<.001）却最不可能（OR 0.2），医生相信凭经验可识破被篡改数据；可用性失败被认为既可能又危险（OR 2.1），他们用纸质记录等模拟变通应对，反引入新风险。徽章（OR 2.8）与生物识别（OR 2.2）被认为改善护理，2FA（OR 0.467）被认为妨碍护理。

我的思考：以主观感知而非客观频率为研究对象是合理取舍——感知直接塑造行为，且作者明确不做客观频率主张。跨三国、多角色、严格筛查是方法亮点。局限在于假想场景、社会期望偏差，以及"经验能识破数据篡改"的边界未被验证。启示是：控制需匹配临床移动性与应急流程，把临床医生视为"最后防线"而非漏洞，为医工结合的安全干预提供了实证框架。

- **论文 PDF**: ["Your imaging may be stone-cold normal, but if they look sick, they're going to get admitted": An Investigation of Clinicians' Perceptions of Impact & Likelihood of Security Failures](https://www.usenix.org/system/files/usenixsecurity26-thompson.pdf)

#### A Large-Scale Study of Personalized Phishing using Large Language Models
- **作者**: Stefan Czybik, Anne Josiane Kouam, Peter Heubl and Jan Magnus Nold, Konrad Rieck
- **方向**: 社会工程与人类因素
- **解读**: 鱼叉式钓鱼虽只占钓鱼邮件的 0.1%，却与 66% 的数据泄露相关。已有研究仅证明 LLM 能生成通用钓鱼邮件，个性化攻击在大规模真实场景下的效果从未被量化。本文在 7,700 余名参与者（布伦瑞克工业大学员工）的真实钓鱼意识培训框架内，首次大规模量化 LLM 鱼叉式钓鱼的威力。
方法上，研究者构建全自动三步工作流：以受害者邮箱为查询词经 Google API 抓取公开网页（每目标取前 5 条）；用本地开源 LLM（如 deepseek-r1:14b）过滤并生成目标画像（输入截断至 4,096 token）；再由 phi4:14b（经 GPT-4o 合成数据 LoRA 微调）选择发件人身份并生成个性化邮件。四种对照条件覆盖 LLM/人工 × 通用/个性化四个维度，全程与学校数据保护办公室、安全团队协作，符合伦理规范。
结果显示：LLM 个性化钓鱼点击率达 10.0%，几乎是通用 LLM（3.7%）与人工通用（4.1%）基线的三倍（p < 10⁻⁵），即 2.74 倍提升；人工鱼叉式钓鱼达 24.2%，但 100 封需约 12 小时，而 LLM 生成 4,010 封仅需约 50.5 小时且成本约 $120，即每封不到 $0.03。出人意料的是，公开信息最多的高画像组点击率反而显著更低，作者归因于模型对丰富输入的重述化与主题漂移削弱了个人化程度。
该研究把"LLM 使个性化钓鱼可规模化"从担忧变为量化事实，$0.03/封的边际成本意味着防御必须分层：限制邮箱可关联的公开信息、将个性化钓鱼纳入意识培训、以技术手段缓解人为失误。局限在于单一高校场景、事前简报引入偏差，本地小模型给出的应是攻击效果下界；人工基线仅 100 人样本也使 24.2% 的估计较粗。

- **论文 PDF**: [A Large-Scale Study of Personalized Phishing using Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-czybik.pdf)

#### Inconsistent, Incomplete, and Insecure: A Survey of Account Security Interfaces
- **作者**: Arkaprabha Bhattacharya and Alaa Daffalla, Kevin Lee, Rosanna Bellini, Nicola Dell, Thomas Ristenpart
- **方向**: 社会工程与人类因素
- **解读**: 账户入侵依然普遍且破坏巨大，尤其在攻击者与受害者有物理或社交接近的场景（如亲密关系虐待）中，受害者主要靠服务商提供的账户安全接口（ASI，通知与日志）识别未授权访问。此前只有 Daffalla 等对 Google、Facebook、Apple、WhatsApp 四个服务的研究，结论是否全网成立是开放问题。本文做迄今最大规模 ASI 测量：基于 Tranco 列表选取 100 个流行服务逐一 stepthrough。

方法上，作者创建账户、发现 ASI、登出后模拟敌意设备访问（VPN、改 user-agent、改时钟、加非活跃会话），再观察 ASI 展示信息，评估其判断"是否有第二台设备访问"的可用性与完整性；据此构建 7 类 ASI 类型学（2 种站外通知、5 种站内日志），比 Daffalla 等多了 2 类日志与 1 类通知。

结果：71 个服务发现 200 个 ASI，29 个流行服务完全没有可区分账户访问来源的日志或通知；另有 10 个服务 ASI 无信息或仅时间信息，合计 39 个无法判断是否有第二台设备访问。61 个提供设备/位置描述的 ASI 服务中 41 个（67.2%）可被欺骗攻击成功混淆访问来源。此外 ASI 部署不一致、导航多级且术语混乱，跨服务门户（如 Microsoft）不提供具体服务粒度。据此提出 6 条改进原则，披露问题并公开数据。

我的思考：把 Daffalla 等的发现从 4 个服务推广到 100 个流行服务，实证"不一致、不完整、不安全"的普遍性，7 类类型学提供统一术语，贡献扎实。局限：人工 stepthrough 覆盖 100 个仍是采样；欺骗攻击所需能力（改 UA/VPN）对低技术攻击者是否可达未量化；未评估 ASI 实际使用效果。对家暴幸存者、记者等高风险群体，ASI 是事后发现入侵的主要手段，厂商应落实 6 条原则，以服务端不可伪造证据替代可伪造设备描述。
- **论文 PDF**: [Inconsistent, Incomplete, and Insecure: A Survey of Account Security Interfaces](https://www.usenix.org/system/files/usenixsecurity26-bhattacharya.pdf)

#### Love, Lies, and Language Models: Investigating AI's Role in Romance-Baiting Scams
- **作者**: Gilad Gressel and Rahul Pankajakshan, Shir Rozenfeld, Ling Li, Ivan Franceschini, Krishnashree Achuthan, Yisroel Mirsky
- **方向**: 社会工程与人类因素
- **解读**: 浪漫诱骗（romance-baiting）诈骗由东南亚诈骗园区有组织运作，强迫劳工经数周文本对话建立情感信任后诱导虚假加密货币投资，过去四年有超 750 亿美元经诈骗关联账户洗钱、肇事者落网率不足 0.05%。诈骗本质是文本交互，因此大语言模型（LLM）是否及如何被用于自动化这场人祸成为紧迫问题。

方法上，作者 2022–2025 年访谈 145 名园区内部人员与 5 名受害者绘制运营结构；开展首个 7 天双盲对话实验（22 名参与者各自同时与 1 名人类操作员和 1 个 LLM 智能体聊天，均不知情），LLM 智能体由 Claude 3.7 Sonnet 生成对话、GPT-4o 做记忆管理，并加装人类化流水线（消息拆分、错别字、打字延迟、主动再接触）；最后用询问式提示与 Llama Guard 3、Perspective、OpenAI Moderation API 评估厂商安全防护。

结果显示：87% 的诈骗劳动力从事 Hook/Line 两阶段、极易自动化；2024 年底后访谈的 34 名内部人员全部报告日常使用 ChatGPT。实验中 LLM 综合信任分显著更高（p=0.007），任务遵从率 46% 对人类的 18%，参与者 70–80% 的消息发给了 LLM；而三大主流过滤器对浪漫诱骗对话检出率 0.0%，且单一“不要暴露你是 AI”指令即可覆盖厂商披露机制。

我的思考：这是首个量化 LLM 在长周期隐蔽关系中欺骗能力的研究，伦理设计（授权欺骗+事后告知+持续监测）值得借鉴。局限明显：样本仅 22 人且偏数字化素养高的群体、7 天远短于真实数月周期、以安装良性 App 代理金融转账；且事后 20/22 人能认出 AI，说明其并非不可战胜。该工作真正警示的是：低危语境的滥用（共情聊天）天然绕过内容过滤器，防御需转向意图与跨会话时序检测而非单条消息审核。

- **论文 PDF**: [Love, Lies, and Language Models: Investigating AI's Role in Romance-Baiting Scams](https://www.usenix.org/system/files/usenixsecurity26-gressel.pdf)

#### You Know Why, but Still Rely: The Impact of Explainable AI on Trust, Task Load, and Performance in Cybersecurity Decision-Making
- **作者**: Neele Roch
- **方向**: 社会工程与人类因素
- **解读**: 网络安全从业者人手不足，AI 辅助有望降低认知负荷，欧盟监管与从业者自身都要求 AI 决策可解释；但 XAI 解释在高风险、知识密集型安全决策中究竟能否建立信任、提升人机协同绩效，此前证据相互矛盾，且多数研究用外行被试与代理任务。

作者以恶意域名阻断（MDB）为用例开展被试间实验（N=139，XAI 组 72 人、AI-only 组 67 人），被试均为具有网络安全领域知识者。原型为浅层神经网络+积分梯度（IG）特征归因解释的仪表盘；测量采用自动化信任量表（TiA）、行为性依赖（与 AI 建议一致率）、RSME 与 NASA-RTLX 任务负荷、SUS 可用性，并区分态度性信任与行为性依赖。

结果：XAI 组事后 TiA 显著更低（M=67.04 vs 71.18，t(137)=2.70，d=0.46），但行为依赖两组几乎一致（9.69 vs 9.75/12 次决策）；XAI 未提升正确决策数（7.42 vs 7.78）、未改变决策时间（35.88 vs 37.50 秒）与任务负荷；且存在严重过度依赖——AI 判断正确时 83.19% 决策正确，AI 判断错误时仅 23.38%；质性结果显示领域用户自带判断标准，暴露 AI 决策边界反而引入歧义与不信任。

我的思考：该研究有力质疑"解释必促信任"的默认假设：对领域专家，解释可能是负担而非增益，且信任（态度）与依赖（行为）可显著分离，这解释了既往文献的矛盾；局限在于单任务、单解释方法（IG）与样本结构（多为高学历、AI 熟悉度高）。其"用户对齐、情境敏感的解释策略"结论，与算法厌恶文献互补，对安全 AI 工具的产品设计有直接指导意义。
- **论文 PDF**: [You Know Why, but Still Rely: The Impact of Explainable AI on Trust, Task Load, and Performance in Cybersecurity Decision-Making](https://www.usenix.org/system/files/usenixsecurity26-roch.pdf)

#### "Oh, what people would do with my knife?" Navigating the Dual-Use Dilemma in PoC Exploit Development, Disclosure, and Community Dynamics

- **作者**: Arwa Al Alsadi and Lorenz Kustosch; Lamya Alowain; Michel Van Eeten and Carlos H. Gañán
- **方向**: 社会工程与人类因素
- **解读**: PoC利用代码在披露后被武器化的时间已从平均1小时7分钟骤降至22分钟，但既有研究多基于观测数据关注"何时、如何"被利用，对开发者的动机、决策过程（"谁、为何"）缺乏理解。本文首次运用期望-价值理论（EVT）系统剖析PoC开发者这一双重用途困境中的人类因素。

作者对16名跨北美、南美、欧洲、非洲、亚洲的PoC开发者进行45-60分钟半结构化访谈（两场约2小时，33份报名筛选），采用混合归纳-演绎编码，双编码者独立编码全部16份转录，Krippendorff's alpha达到0.857；主题饱和验证显示85%的详细编码在第8次访谈、95%在第12次访谈前出现。

研究发现PoC开发是从崩溃演示到完全武器化利用的连续谱，由技术信心、价值评估与风险计算的复合算计塑造而非二元伦理。作者提出三个理论扩展：双重用途道德推理（"工具中立"信念实现责任外化）、动态价值评估（供应商响应性是披露决策的首要驱动因素）、身份协商（伦理研究者与技术精通者身份间的持续权衡）；并揭示"动机模糊性"——驱动深入利用开发的成就感同时放大滥用潜力。

作为首个将动机理论系统应用于进攻性安全实践的研究，本文直接挑战"负责任/不负责任"披露的二元框架，对CVD治理与政策设计有启示：规则导向不如关系导向的披露框架。局限同样明显：样本仅16人且全为男性、依赖自报告、存在选择偏差；"识别悖论"（发现者受认可而开发者被忽视）等新概念有待更大样本验证。

- **论文 PDF**: ["Oh, what people would do with my knife?" Navigating the Dual-Use Dilemma in PoC Exploit Development, Disclosure, and Community Dynamics](https://www.usenix.org/system/files/usenixsecurity26-al-alsadi.pdf)

#### Digital Risks and Coping Practices among Roblox Game Creators
- **作者**: Qiurong Song
- **方向**: 社会工程与人类因素
- **解读**: 游戏平台已成为创作者经济支柱：Roblox 2025 年有 1.118 亿日活、310 万活跃创作者、670 万活跃游戏，2025 上半年创作者合计收入 5.9794 亿美元；但创作者同时面临剥削、威胁、性骚扰等报道。此前研究聚焦社交媒体创作者，游戏创作者（要管理代码、资产、复杂生产流程和团队）的风险图景缺乏系统刻画。

作者于 2024 年 2–4 月对 20 位 Roblox 创作者做半结构化访谈（中位年龄 19，含 7 名独立开发者、7 名工作室成员、5 名工作室主，1 名未成年），用归纳式反思性主题分析，并把数字风险建模为"结构性条件（平台架构与治理）+操作性风险事件"两层现象。

分析得出五类风险：平台风险（不透明且不可预测的审核导致的创作不稳定、财务剥削式变现结构）、生产风险（项目失败、游戏克隆、劳动诈骗、法律暴露）、组织风险（资金挪用、劳动剥削、团队不稳定、人际冲突）、社区风险（敲诈勒索、隐私泄露、声誉风险，被青少年主导的高可见性放大）、技术风险（恶意免费模型、资产盗窃等）。应对策略包括自我审查、为公平报酬谈判、寻求社区支持、隐私实践，论文据此给出平台、社区、政策建议。

我的思考：首个系统刻画 UGG 游戏创作者数字风险的经验研究，把结构性条件纳入风险本体是概念贡献，访谈方法扎实（IRB、监护人陪同未成年人）。局限：样本仅 20 人、限美国英语环境、单平台，且以感知为主难以量化危害分布。对平台治理（审核透明、报酬结构、未成年人保护）有直接政策含义，也为后续大规模调查提供了可操作的编码框架。
- **论文 PDF**: [Digital Risks and Coping Practices among Roblox Game Creators](https://www.usenix.org/system/files/usenixsecurity26-song-qiurong.pdf)

#### Chameleon Channels: Measuring YouTube Accounts Repurposed for Deception and Profit

- **作者**: Alejandro Cuevas, Manoel Horta Ribeiro, Nicolas Christin

- **方向**: 社会工程与人类因素

- **解读**: 内容创作者需长期经营才能积累受众，而"二手账号"市场让既有频道可被出售后彻底改头换面，用于加密货币推广、选举新闻等欺骗与牟利内容——已有的粉丝与可信度成为金融/意识形态恶意方的现成资产，且翻新后订阅者往往毫无察觉。

方法上利用两个测量优势：Fameswap 等透明账号市场可直接观察出售频道（6 个月观测期销售额超 100 万美元），YouTube 频道 ID 持久不变使翻新可被追踪。作者在 3 个月窗口内用 Social Blade（追踪 6800 万+频道）抽样的约 140 万账号两个快照估计普遍性，以非翻新频道为对照检验"翻新频道发布政策敏感内容比例显著更高"的假设。

53% 的已售频道被用于发布政策敏感内容且大多未受处罚；翻新后订阅者不减反增。估计 2025 年 1–3 月约 0.25% 的频道被翻新；两轮样本中翻新频道合计持有约 1.93 亿与 4400 万订阅者；其中既有类似影响力行动的频道，也有金融诈骗频道。

我的思考：首次把二手账号市场与平台内翻新现象做量化连接，ID 不变性追踪设计巧妙，销售规模与订阅者体量数据有说服力。局限：Fameswap 样本存在选择偏差，翻新标注依赖人工与启发式，0.25% 仅是已观测到的下界；对"订阅者为何不取关"及平台检测机制的解释有限；对 YouTube 以外平台的推广仍是外推而非测量。

- **论文 PDF**: [Chameleon Channels: Measuring YouTube Accounts Repurposed for Deception and Profit](https://www.usenix.org/system/files/usenixsecurity26-cuevas.pdf)

### 程序分析与形式化验证（6 篇）

#### MULCOTAINT: Towards Efficient Multi-tag Dynamic Taint Analysis via Hardware/Software Co-design
- **作者**: Bing Qi, Yi Yang, Xiangkun Jia, Zhengpin Qian, Huafeng Huang, Purui Su
- **方向**: 程序分析与形式化验证
- **解读**: 多标签动态污点分析（M-DTA）能区分不同输入字节的影响，对漏洞定位与修复、利用构造至关重要，但软件方案性能极差（PANDA 测试 SPEC2017 641.leela 时耗尽 32G 内存），现有硬件方案（FineDIFT、RAFT、HardTaint）均为单标签、难以扩展，且缺乏可配置接口。

方法上，MULCOTAINT 采用硬件/软件协同：基于 RISC-V 开源 SoC RocketChip 与 PHMon 协处理器框架，把污点计算与正常执行解耦。核心设计：将污点标签设计为 1024 位宽向量（128 个源×8 字节），把逐字节线性推理改写为向量化计算并以微码+专用电路实现；基于 SV39 虚拟内存构造三级污点页表管理标签，通过 64 位总线实现 1024 位标签读写；提供扩展指令接口、源码级嵌入与二进制 LD_PRELOAD 两种集成方式，支持 42 条 RV64I 指令。

在 32 个程序（SPEC、PHP/Nginx、Juliet、CTF、10 个真实 CVE）上：平均仅 5.8× 开销（1.07–15.81×），相对开销约为 TaintRabbit 的 1/136.65、PANDA 的 1/1117.51；按等效标签规模折算，HardTaint 平均开销是其 19.57 倍、FineDIFT 23.17 倍、RAFT 12.55 倍；能检出数据集中全部漏洞并给出触发漏洞的具体输入字节，而 FineDIFT/RAFT 无法处理复杂堆操作。

我的思考：把“逻辑密集”的 M-DTA 重构为“计算密集”的向量化问题并落到可编程协处理器，用页表管理海量标签，设计思路与工程完成度俱佳且已开源。局限：原型仅 50MHz FPGA，绝对性能受限（频率折算后才有压倒性优势）；I/O 密集程序（如 jhead、PHP）因宽标签搬运开销加速比骤降；128 源上限受 FPGA 资源约束。相比软件方案与单标签硬件方案，这是硬件污点分析迈向多标签实用化的关键一步，但距商用 CPU 集成仍有距离。

- **论文 PDF**: [MULCOTAINT: Towards Efficient Multi-tag Dynamic Taint Analysis via Hardware/Software Co-design](https://www.usenix.org/system/files/usenixsecurity26-qi-bing.pdf)

#### InstrSem: Automatically and Generically Inferring Semantics of (Undocumented) CPU Instructions
- **作者**: Lorenz Hetterich
- **方向**: 程序分析与形式化验证
- **解读**: 现代 CPU 指令集语义的机器可读形式常不完整，且存在不在规范中的未文档化指令，可被用于提权、拒绝服务与绕过内存保护；此前语义恢复工作集中于 x86（STRATA、libLISA 依赖大量 ISA 专用模板），其他架构靠大量手工逆向，缺乏通用方法。

InstrSem 是 ISA 无关的黑盒方法：给定单条指令编码，在系统变化的结构状态下执行并记录输出，通过候选函数差分测试为每个变化的状态分量合成紧凑数学函数，得到编码语义；再突变编码位、将行为变化与位位置关联，恢复寄存器与立即数字段，泛化为完整指令。只需轻量 ISA 模型与每架构的用户态 runner，支持定长/变长编码（RISC/CISC）、内存访问与条件行为。

在 RV64I 上自动恢复超过 97.81% 基础指令集的正确语义；对 LA64 全编码空间在 77 小时内逆向出 136 条指令、覆盖 10 亿+（1,009,055,744）编码；发现未文档化向量指令、QEMU 与龙芯硬件不一致、以及会使 QEMU 崩溃的指令（已上报修复）。另在 SiFive P550 上发现 305 条未文档化指令，逆向出苹果 Mul53 扩展，并用之把 GhostWrite 泛化为覆盖 65,536 个编码的指令。

我的思考：通用性是其核心贡献——把"每条指令语义"的逆向从工程苦活变成可扩展流水线，且输出人类可读，能直接支撑模拟器正确性、形式化验证与模糊测试覆盖；局限在于采用随机化差分测试（无完备性保证）、函数合成限于整数位向量运算、每架构仍需 runner。与 x86 专用工具相比，其对小众架构与专有宏语言的覆盖，为"逆向即安全分析"提供了新基础设施。

- **论文 PDF**: [InstrSem: Automatically and Generically Inferring Semantics of (Undocumented) CPU Instructions](https://www.usenix.org/system/files/usenixsecurity26-hetterich-lorenz-instrsem.pdf)

#### Arguzz: Testing zkVMs for Soundness and Completeness Bugs
- **作者**: Christoph Hochrainer, Valentin Wüstholz, Maria Christakis
- **方向**: 程序分析与形式化验证
- **解读**: 
零知识虚拟机（zkVM）支撑区块链 rollup 与去中心化应用的可验证离链计算，执行 Rust 等通用程序并产出简洁证明。zkVM 的约束系统或执行逻辑一旦出错，会造成 soundness 问题（接受无效执行，可致欺诈交易入链）或 completeness 问题（拒绝合法执行，损害活性），而此类系统庞大复杂、极难测试，此前没有自动化工具。

ARGUZZ 是首个自动化检测 zkVM soundness/completeness bug 的工具：将元变形测试与故障注入结合——生成语义等价的程序对并合并为单个输出已知的 Rust 程序在 zkVM 内运行，通过注入故障模拟恶意或存在 bug 的 prover，若验证者仍接受且程序输出发生改变，则暴露过弱约束；配以按指令频率公平调度的故障注入器与内联汇编扩展。

对 RISC ZERO、NEXUS、JOLT、SP1、OPENVM、PICO 六个真实 zkVM 的测试中，在三个系统（RISC ZERO、NEXUS、JOLT）发现 11 个此前未知 bug，其中 3 个 soundness、8 个 completeness；一个 RISC ZERO soundness bug 尽管该代码此前经过审计仍获 5 万美元赏金；所有 soundness bug 均由指令修改型注入触发。

我的思考：该工作把约束系统 fuzzing 从 DSL 管线（其前作 CIRCUZZ）推广到通用程序 zkVM，故障注入是识别弱约束的关键创新——仅凭元变形难以区分完整性缺陷。11 个真实 bug 与高额赏金说明成熟系统仍存在系统性测试空白。局限是六个被测系统仅三个检出 bug，可能与测试时长和覆盖策略有关；语义等价程序对与注入点的组合空间仍有限，soundness 覆盖主要依赖单一注入类型，泛化性有待更多实证。

- **论文 PDF**: [Arguzz: Testing zkVMs for Soundness and Completeness Bugs](https://www.usenix.org/system/files/usenixsecurity26-hochrainer.pdf)

#### Static Detection of TOCTOU Bugs Caused by Kernel Races

- **作者**: Gui-Dong Han

- **方向**: 程序分析与形式化验证

- **解读**: TOCTOU（检查-使用时间竞争）漏洞可绕过内核安全检查，导致崩溃与提权。作者对近五年 203 个修复 TOCTOU 的内核补丁进行实证研究，发现内核线程并发竞争（KRT 类）占 68.0%，是内核 TOCTOU 最普遍的根源；但受内核并发逻辑复杂性与线程调度非确定性的制约，一直缺乏系统化针对 KRT 漏洞的检测方法。

方法上设计 KERAT，首个系统化静态检测 KRT 漏洞的工具，把问题归结为共享变量 check-use 操作的原子性违例，分两阶段：其一，原子性规则挖掘——用字段图解析内核嵌套结构字段的别名关系，从条件语句识别安全检查变量，分析其修改是否总被某锁保护，挖掘出"哪个锁应保护哪个共享变量 check-use 对"的规则；其二，基于状态机的验证——枚举锁与检查-使用操作的组合模式，归纳四种危险模式并编码为两个有限状态机，用函数摘要加速过程间验证，检测违反原子性规则的 bug。

在 Linux-5.16、Linux-6.8 与 FreeBSD-14.1 上共发现 825 个 KRT 报告、671 个真实 bug（假阳性率 18.7%），其中 Linux-5.16 的 320 个里有 76 个已在 6.8 修复；Linux-6.8 与 FreeBSD 的 287 个有害 bug 中 65 个获内核开发者确认，21 个补丁修复 36 个 bug，10 个获得 CVE 编号；78% 位于驱动中，31.1M 行代码在 57 小时内分析完成。

我的思考：首次把 KRT 检测系统化，实证规模与开发者确认数量令人信服，且挖掘出的原子性规则可反哺内核并发文档。局限也明显：18.7% 假阳性率仍偏高，静态分析依赖 allyesconfig 等配置使覆盖受限，且对完全无锁保护的场景可能漏报。与 LRSan、CPALockator、LR-Miner 的对比显示其能发现被这些工具遗漏的 TOCTOU，说明以原子性规则为检测对象比单纯竞争检测更契合 TOCTOU 本质，为内核安全分析提供了新的范式。

- **论文 PDF**: [Static Detection of TOCTOU Bugs Caused by Kernel Races](https://www.usenix.org/system/files/usenixsecurity26-han.pdf)

#### OS-Sanitizer: System-wide Latent Defect Inference in Linux Applications

- **作者**: Addison Crump, Thorsten Holz

- **方向**: 程序分析与形式化验证

- **解读**: 动态测试传统上"按例证"寻找故障，即先触发失败再定位；但许多缺陷只在特定运行上下文才诱发失败，难以用测试用例覆盖。作者提出"动态缺陷推断"：不做故障触发，而从良性运行行为推断潜在缺陷，相当于把静态分析中"代码坏味道"（code smell）的概念移植到动态测试，并利用运行时事件获得额外上下文。

方法上利用 eBPF 在系统层面收集用户态与内核态事件并在内核内处理，实现原型 OS-Sanitizer：实现 15 个启发式 pass（如 access 后未检查返回、固定地址 mmap、RWX 内存、未加锁文件指针、不安全字符串函数、system() 可变路径、内存安全类等），对全系统所有应用持续监控并输出带调用栈的报告，无需重编译或修改目标程序。

评估分四层：微基准中纯用户态函数探针开销高达 106–140×（memcpy、strcpy 等），但 SPEC CPU 2017 与 Speedometer 3.0 真实基准里非内存安全类 pass 大多无显著开销；用 eBPF 仅 4 小时 260 行代码重实现 Wei-Pu 的 CUU 模型验证可扩展性；作者在日常开发机上连续运行一个月，直接发现 Go（os.RemoveAll TOCTOU）、Docker、Firefox 等 12 个缺陷，追踪报告又发现 30 余个，合计 40 余个问题，部分存在十年以上且遍布主流发行版。

我的思考：把"坏味道"引入动态测试是新颖且实用的方向，eBPF 免重编译、可长期后台运行的特性使其能发现静态工具与单元测试都难覆盖的"上下文相关"缺陷，实证说服力强。局限同样坦诚：启发式存在误报漏报权衡，内存安全类 pass 开销不可接受，长期报告含隐私无法公开，且人工调查成本不低。它与静态分析和传统 sanitizer 互补而非替代，为软件测试开辟了值得深入挖掘的新方向。

- **论文 PDF**: [OS-Sanitizer: System-wide Latent Defect Inference in Linux Applications](https://www.usenix.org/system/files/usenixsecurity26-crump.pdf)

#### SafeFFI: Efficient Sanitization at the Boundary Between Safe and Unsafe Code in Rust and Mixed-Language Applications

- **作者**: Oliver Braunsdorf and Tim Lange, Konrad Hohentanner and Julian Horsch, Johannes Kinder

- **方向**: 程序分析与形式化验证

- **解读**: Rust 的 unsafe 代码（FFI 互操作、底层数据结构）可能在整体内存安全的程序中引入内存破坏；sanitizer 能捕获这类错误，但对 Rust 类型系统已保证安全的指针解引用也插入大量冗余检查，运行时开销高。现有选择性消毒方案（RustSan、ERASan）依赖全程序 points-to 分析分类指针，编译开销巨大且错失优化机会。

方法上 SafeFFI 提出关键洞察：raw 指针（*const T/*mut T）到安全指针类型（&T、Box<T> 等）的 cast 正是"由 sanitizer 保证内存安全"与"由类型系统保证内存安全"的分界。SafeFFI 把边界检查"提升并捆绑"为单一动态校验的前置条件，再经类型系统静态传播，从而免除 sanitizer 对每个解引用逐一检查；只需局部推理，无需全程序分析。实现基于修改的 Rust 编译器，跨 MIR 与 LLVM IR 分析，并含高效识别潜在释放内存函数的算法，且与底层 sanitizer 解耦。

在 43 个已知漏洞数据集与流行 Rust crate 上评估：编译时间开销 2.01×（对比同类方法的 5.91×）；sanitizer 检查最多削减 79.63%；ASan 运行时开销从 2.71× 降至 2.44×，HWASan 从 3.18× 降至 2.29×；检测能力与基线 sanitizer 完全持平，且因检查前移，21 个漏洞在根因处更早被报告，全程无假阳性。

我的思考："让类型系统接管 sanitizer 的检查"思想简洁有力，规避全程序分析使其工程可扩展性远超前辈，是选择性消毒路线的务实改进。局限也交代得诚实：多线程下 Free-During-Scope 类漏洞存在概念性漏检风险，内联汇编与 transmute 支持有限。与 RustSan/ERASan 对比时工具链版本差异值得读者注意。整体上为 Rust 与混合语言应用的内存安全实践提供了低成本高收益的路径，未来可扩展至更多 sanitizer 与强类型语言。

- **论文 PDF**: [SafeFFI: Efficient Sanitization at the Boundary Between Safe and Unsafe Code in Rust and Mixed-Language Applications](https://www.usenix.org/system/files/usenixsecurity26-braunsdorf.pdf)

### 其他（4 篇）

#### XGuardian: Towards Generalized, Explainable and More Effective Server-side Anti-cheat in First-Person Shooter Games
- **作者**: Jiayi Zhang
- **方向**: 其他
- **解读**: 瞄准辅助作弊（aimbot、wallhack）是 FPS 游戏最普遍、危害最大的作弊形式，损害公平并带来巨大经济与声誉风险。现有方案存在四类问题：客户端方案易被绕过（如 SGX 已于 2021 年弃用）且并发开销大；服务器端方案特征针对特定子类型（如 HAWK 依赖经济与手雷特征，难以跨游戏泛化）；真实世界大尺度评估不足；全部缺乏可解释性，人工审核需回放数小时。

XGuardian 是服务器端系统，仅用 pitch 与 yaw 两个所有 FPS 必备的原始输入，构造速度、加速度、角度变化三类时序特征描述瞄准轨迹；INSPECTOR 用 GRU-CNN 集成模型分类"淘汰事件"窗口样本（96 tick≈1.5 秒窗口、滑动窗 6）；EXPLAINER 扩展 SHAP（Gradient/TreeExplainer）并引入"时间挤压"机制把 Shapley 值聚合到每个时间步，精确定位可疑帧与特征。基于回放文件在服务器空闲期异步分析，开销边际。

在 CS2 真实平台（5EPlay）数据上评估：3,069,216 次瞄准操作、31,971 条轨迹、5,486 名玩家；加权平均准确率 0.884、FPR 0.059、召回 0.884、精度 0.942、F1 0.904，最优组合达召回 0.907@FPR 0.041（即 90.7% 召回率、4.1% 误报率）；相比 SOTA（HAWK）检测准确率提升 12.5%；在 Farlight84（PC 与移动端）验证跨子类型泛化性，并对对抗攻击做了初步鲁棒性评估；系统与两个数据集全部开源。

我的思考：pitch/yaw 特征选择是泛化关键——绕开游戏定制特征与客户端数据，把反作弊问题还原为通用的瞄准轨迹分类。可解释性直接缩短人工审核时间，正中行业落地痛点。局限：依赖回放文件与人工复核的平台标签、泛化评估范围有限、对行为模仿型对抗的鲁棒性仅"初步"。相比 HAWK 以少得多的输入达到更高精度，工程完整度（开源+数据集）值得肯定，为游戏反作弊树立了新的实证基线。
- **论文 PDF**: [XGuardian: Towards Generalized, Explainable and More Effective Server-side Anti-cheat in First-Person Shooter Games](https://www.usenix.org/system/files/usenixsecurity26-zhang-jiayi.pdf)

#### A Midsummer Meme's Dream: Investigating Market Manipulations in the Meme Coin Ecosystem
- **作者**: Alberto Maria Mongardini, Alessandro Mei
- **方向**: 其他
- **解读**: 迷因币（Meme Coin）市场规模已超千亿美元，其价值完全依赖社区情绪而非实际效用，因此极易被操纵。本文首次跨链大规模分析迷因币生态：数据集涵盖以太坊、BNB 智能链（BSC）、Solana 与 Base 共 34,988 个代币（公开释出），并基于 OHLCV 日线数据进行了三个月的纵向追踪。
方法上，数据集整合 CoinMarketCap/CoinGecko/GeckoTerminal 已知迷因币与 DexScreener/CoinSniper 新发代币，用 TF-IDF 名称关键词（126 个）加 pump.fun（.pump 后缀）分类并经地址验证，最终保留 31,811 个有效代币。作者识别出 707 个高回报（>100%）代币，用安全平台风险信号检测所有权集中等异常，用多重方法检测洗售交易（零风险仓位：同日买卖量差 ≤2%；循环成交量：≥99% 量来自同日双向交易者；协同操纵网络），并定义了新操纵——流动性池价格通胀（LPI）。
结果显示：高回报代币中高达 82.89% 呈现人工增长策略迹象；62.9% 被实施抽水砸盘/rug pull 的代币此前经历过洗售或 LPI，在遭聚合器下架的代币中该比例达 86.67%，说明早期操纵是为满足上币门槛后套现铺路。洗售交易确认 1,043 起事件、涉及 287 个代币（中位量增幅 1,772%）；LPI 仅 72 起事件、40 个代币，中位买入金额仅 $54，却使 50% 的事件价格涨幅超 500%、36.11% 超 1,000%，平均仅 1.96 名参与者（66.67% 为单人）。经济损失合计：抽水砸盘 $3.27M、rug pull $6.04M，波及超 17,000 个受害地址。
本文首次系统刻画了迷因币"制造热度→吸引散户→套现"的操纵链条，定义了 LPI 这一极低成本的操纵范式（利用 AMM 常数乘积公式在低流动性池中实现高杠杆涨价），并揭示 88.1% 的高市值（>10 亿美元）代币流动性不足其市值 0.1% 的荒诞现象。局限在于检测基于启发式阈值、无法覆盖所有操纵形式，对链下舆论操纵（社群、KOL）未涉及。

- **论文 PDF**: [A Midsummer Meme's Dream: Investigating Market Manipulations in the Meme Coin Ecosystem](https://www.usenix.org/system/files/usenixsecurity26-mongardini.pdf)

#### Revealing the Dark Side of Smart Accounts: An Empirical Study of EIP-7702 Incurred Risks in Blockchain Ecosystem

- **作者**: Mingyuan Huang, Han Liu, Shuo Yang, Daoyuan Wu, Shuai Wang

- **方向**: 其他

- **解读**: EIP-7702 智能账户允许外部账户（EOA）在保留原地址的前提下升级为可编程账户，随以太坊 Pectra 升级于 2025 年上线，七个链上智能账户已超 300 万。但该机制重构了 EOA 与合约账户（CA）的信任边界：恶意附着代码可在受害者账户上下文内执行，而保留 tx.origin 的 SA 又能冒充 EOA 绕过 legacy 合约检查，形成全新攻击面。本文是首个针对 SA 风险的系统实证。

方法上，作者按受害者类型将攻击分为 EOA 定向、CA 定向与复合三类，并构建统一检测工具：事务分析器从海量链上数据过滤可疑交易，Gigahorse 反编译器还原字节码语义，合约分析器以规则匹配识别恶意模式（入账钩子+资产转出、闪电贷回调链、跨合约签名链）。

在七个链共 366 万条授权交易上，确认 924 个恶意合约（793 EOA 定向、124 CA 定向、7 复合），造成约 236 万美元损失，另有约 1014 万美元资产面临风险，并披露 10 个零日攻击地址。关键发现：63% 以上的授权交易关联恶意 EOA 定向攻击，授权最频繁的前 30 个合约中近半被攻击者控制；部分攻击者"攻击后重绑良性合约"规避检测。

我的思考：本文把信任边界重构系统化为三类可检测攻击并给出跨链实证，填补了 EIP-7702 安全研究空白；63% 恶意授权占比警示钱包"升级授权"提示远不够安全。局限在于规则由已观察模式归纳，对新型钩子存在漏检。相比单点 DeFi 漏洞研究，本文指向账户抽象协议层本身的系统性风险，并提供首个公开跨链攻击数据集。

- **论文 PDF**: [Revealing the Dark Side of Smart Accounts: An Empirical Study of EIP-7702 Incurred Risks in Blockchain Ecosystem](https://www.usenix.org/system/files/usenixsecurity26-huang-mingyuan.pdf)

#### "Abuse Risks are Often Inherent to Product Features": Exploring AI Vendors' Bug Bounty and Responsible Disclosure Policies
- **作者**: Yangheran Piao
- **方向**: 其他
- **解读**: AI 系统处理敏感数据、承担关键功能，其漏洞是否“特殊”存在理论争论；研究者提交提示注入、幻觉等 AI 漏洞时，厂商是否接收并奖励、现有赏金机制能否迁移到 AI 漏洞，此前缺乏系统性研究，而厂商响应缓慢的轶事屡见不鲜。

方法上，作者以 Gartner 榜单构建 264 家 AI 厂商样本，收集 168 份漏洞披露文档，按扩展后的 15 元素框架编码 1,011 个政策单元；对明确提及 AI 漏洞的 48 家厂商做快照+纵向（2018–2025 共 205 项 AI 相关变更，经 Wayback 存档）定性分析；并与 AI 事故数据库（AIID，筛出 320 起）和 359 篇顶会 AI 安全论文对照，编码者一致性 κ=0.816。

结果：36% 的 AI 厂商没有任何披露渠道，仅 18% 提及 AI 风险；数据访问（9 收/0 拒）、授权（8/0）、模型提取（9/1）最常入范围，越狱（3/8）、幻觉（1/6）、有害输出（10/12）多被排除；厂商可归为三档：主动澄清 46 家（其中积极支持者仅 9 家）、沉默 115 家、限制 103 家；厂商首份 AI 政策 2018 年才出现，明显晚于学界（2006 年起）与现实事故（2012 年起），2023 年后更新才加速。

我的思考：这是首份 AI 漏洞披露政策的系统实证，把“AI 漏洞是否特殊”的争论落到可量化的政策行为上：厂商接受以安全目标定义的漏洞（数据访问、授权），拒绝内容安全类（越狱、幻觉），且头部模型厂商明显更主动（48% 提及 AI 风险）。局限：Gartner 样本偏向头部、政策文本不等于执行（如 Cursor 政策沉默却实际修复了提示注入漏洞）、部分无渠道厂商可能是采集遗漏。对政策制定者而言，需推动标准化的 AI 漏洞分类、强制披露渠道与安全港条款，才能避免研究激励持续错位。

- **论文 PDF**: ["Abuse Risks are Often Inherent to Product Features": Exploring AI Vendors' Bug Bounty and Responsible Disclosure Policies](https://www.usenix.org/system/files/usenixsecurity26-piao.pdf)
