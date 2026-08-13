---
title: IEEE S&P 2026 录用论文深度解读（252 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, IEEE S&P, 2026]
---

IEEE S&P（IEEE Symposium on Security and Privacy，即 Oakland）2026 于 2026 年 5 月举行，是安全领域历史最悠久的顶级会议。本文对 S&P 2026 全部 252 篇录用论文逐一给出中文深度解读，并附上可获取的论文 PDF 链接。解读基于论文摘要（arXiv/OpenAlex 收录），部分论文无公开摘要，仅据标题推断并已标注。

## 全部 252 篇录用论文解读

### DY* Unchained: Now with Composable Security Proofs and Precise Compromise Scenarios.
- **作者**: Théophile Wallez
- **解读**: DY* 是基于 Dolev-Yao 密码学消息模型的机器验证安全协议分析框架。本文扩展 DY*，新增可组合（composable）的安全证明能力，并支持对精确妥协场景（compromise scenarios）的建模与推理，使安全性证明在模块化组合下依然成立。（无摘要，据标题推断）


### Descriptors of Exposure: Undermining Tor Anonymity Through Exploiting Descriptor Flood.
- **作者**: Chunmian Wang, Junzhou Luo, Zhen Ling, Yue Zhang, Shan Wang, Ming Yang, Guangchi Liu, Xinwen Fu
- **解读**: 本文研究针对 Tor 网络的描述符洪泛（descriptor flood）攻击：通过洪泛或操纵网络描述符破坏隐藏服务与中继的匿名性。攻击利用描述符分发机制的特性，削弱 Tor 用户与服务的匿名保护。（无摘要，据标题推断）


### Keytar: Practical Keystroke Timing Attacks and Input Reconstruction.
- **作者**: Mufan Qiu, Lihsuan Chuang, Dohhyun Kim, Huaizhi Qu, Tianlong Chen, Andrew Kwong
- **解读**: 本文提出 Keytar，一种实用化的击键时序（keystroke timing）侧信道攻击，并进一步实现输入内容重建。攻击利用按键时间信息推断用户输入，展示该威胁在真实场景中的可行性。（无摘要，据标题推断）


### One Tap to Hijack Them All: A Security Analysis of the Google Fast Pair Protocol.
- **作者**: Sayon Duttagupta, Seppe Wyns, Nikola Antonijevic, Dave Singelée, Bart Preneel
- **解读**: 本文对谷歌快速配对服务（GFPS）进行首次系统性安全分析，检查了来自 16 个厂商、17 种芯片的 25 款商用配件，发现其核心要求在实现、验证与认证环节存在系统性执行失效，且协议缺陷可级联放大影响。作者展示 WhisperPair 系列实际攻击，可实现未授权配对、静默劫持音频设备、将受害者配件隐蔽绑定到攻击者账户，进而通过 Find Hub 网络实现持续位置跟踪与监视。根因在于 GFPS 依赖易出错的应用层状态检查而非密码学强制；为此提出 IntentPair，通过将用户意图密码学绑定进密钥派生过程，从设计上消除该漏洞。


### The Person Behind the Sound: Demystifying Audio Private Attribute Profiling Via Multimodal Large Language Models.
- **作者**: Lixu Wang, Kaixiang Yao, Xinfeng Li, Dong Yang, Haoyao Li, Xiaofeng Wang, Wei Dong
- **解读**: 本文研究利用多模态大语言模型（MLLM）对音频进行私有属性画像（private attribute profiling），即仅凭声音推断说话者的敏感个人属性。工作揭示 MLLM 从音频中提取私有属性的能力及其隐私风险。（无摘要，据标题推断）


### LLM Unlearning Should Be Form-Independent.
- **作者**: Xiaotian Ye, Mengqi Zhang, Shu Wu
- **解读**: 本文发现现有 LLM 遗忘（unlearning）方法的效果高度依赖训练样本的表达形式，难以泛化到同一知识的其他表达，并将此问题形式化为「形式依赖偏差」（Form-Dependent Bias），系统考察其在不同任务中的表现。为量化其普遍性，作者提出 ORT 基准评估对表达变体的鲁棒性，结果表明该偏差在现有技术中广泛且严重。据此主张遗忘应与形式无关，并提出免训练方法 ROCR（Rank-one Concept Redirection）：定位任务中激活的危险概念这一不变量，在数秒内修改模型参数将目标概念重定向为无害概念，显著优于传统方法且输出自然。


### StepStone: LLM-Based GPU Kernel Driver Fuzzing via User-Space Libraries.
- **作者**: Xiaochen Zou, Juefei Pu, Arrdya Srivastav, Jonathan Cox, Zhengchuan Liang, Yuan Tan, Xingyu Li, Yilin Zhu, Zhiyun Qian
- **解读**: 本文提出 StepStone，利用大语言模型（LLM）通过用户态库（user-space libraries）作为「垫脚石」对 GPU 内核驱动进行模糊测试。该方法以驱动暴露给用户态库的接口作为测试入口，降低对内核态测试设施的需求，提升 GPU 驱动漏洞挖掘的可行性。（无摘要，据标题推断）


### SoK: All You Ever Wanted to Know About Bootloader Security but Were Afraid to Ask.
- **作者**: Connor Glosner, Aravind Machiry
- **解读**: 本文是对引导加载程序（bootloader）安全的系统化综述（SoK），系统梳理其信任根角色、攻击面与已知攻击手法。研究旨在为 bootloader 安全分析与加固提供统一的知识框架与研究方向。（无摘要，据标题推断）


### Responsible Disclosure is a Two-Way Street: Empirically Measuring the Responsible Disclosure Contract in the Firmware Ecosystem.
- **作者**: Hui Jun Tay, Souradip Nath, Arvind S. Raj, Abhay Bhat, Ishan Bansal, Audrey Dutcher, Moritz Schloegel, Adam Doupé, Tiffany Bao, Yan Shoshitaishvili, Ruoyu Wang
- **解读**: 本文对固件生态中的负责任披露契约进行实证测量，考察厂商在漏洞披露流程中的实际响应行为。研究检验披露的「双向性」——安全研究者履行报告义务时，厂商是否兑现修复与沟通承诺。（无摘要，据标题推断）


### Fuzzing the Physical Space: Physics-Aware Testing of Black-Box Industrial Control Systems.
- **作者**: Burak Sahin, David Oygenblik, Mingxuan Yao, Yizhi Huang, Brendan Saltaformaggio, Saman A. Zonouz
- **解读**: 本文针对黑盒工业控制系统（ICS）提出物理感知（physics-aware）的模糊测试方法，将物理过程约束融入测试用例生成。该方法在无需内部访问的情况下，利用物理模型引导对控制系统的安全测试。（无摘要，据标题推断）


### VerfCNN, Optimal Complexity zkSNARK for Convolutional Neural Networks.
- **作者**: Wenjie Qu, Yanpei Guo, Yue Ying, Jiaheng Zhang
- **解读**: 本文提出 VerfCNN，一种面向卷积神经网络（CNN）的 zkSNARK，声称达到最优复杂度。其目标是在不泄露模型与输入的情况下高效证明 CNN 推理计算的正确性。（无摘要，据标题推断）


### Blinding Post-Quantum Hash-and-Sign Signatures.
- **作者**: Charles Bouillaguet, Thibauld Feneuil, Jules Maire, Matthieu Rivain, Julia Sauvage, Damien Vergnaud
- **解读**: 本文研究后量子哈希型签名（hash-and-sign 范式）的盲化问题，构造盲签名方案，使签名者无法看到被签消息内容。工作将盲签名能力扩展到基于格等后量子假设的哈希型签名。（无摘要，据标题推断）


### SoK: Critical Evaluation of Quantum Machine Learning for Adversarial Robustness.
- **作者**: Saeefa Rubaiyet Nowmi, Jesus Rafael Lopez, Md Mahmudul Alam Imon, Shahrooz Pouryousef, Mohammad Saidur Rahman
- **arXiv PDF**: [SoK: Critical Evaluation of Quantum Machine Learning for Adversarial Robustness.](https://arxiv.org/pdf/2511.14989)
- **解读**: 本文首次对量子机器学习（QML）的对抗鲁棒性进行系统化梳理，结合概念组织与跨黑盒、灰盒、白盒威胁模型的实证评估，实现五种代表性攻击（标签翻转投毒、编码器级投毒、代理模型干净标签后门、电路级后门 QTrojan、梯度逃逸 FGSM/PGD）。在 MNIST 与 AZ-Class 上以 2/5/10/50 层电路深度及角度/振幅两种编码训练 QMLP 评估，发现基本准确率-鲁棒性权衡：振幅编码干净准确率最高（MNIST 92.6%、AZ-Class 67%）但在对抗扰动与退极化噪声下崩溃，浅层角度编码模型更稳定。结论指出 QMLP 比经典 MLP 更耐标签翻转投毒但对梯度逃逸更脆弱，电路级后门在多分类场景失效，并据此提出威胁感知、抗噪声的安全 QML 部署框架。


### Sok: Evaluating Jailbreak Guardrails for Large Language Models.
- **作者**: Xunguang Wang, Zhenlan Ji, Wenxuan Wang, Zongjie Li, Daoyuan Wu, Shuai Wang
- **解读**: 本文是对 LLM 越狱防护栏（guardrail）的首次整体性 SoK 分析，提出按六个关键维度划分的多维分类体系。作者引入安全-效率-效用（Security-Efficiency-Utility）评估框架衡量防护栏的实际有效性，并通过大量实验识别现有方法的优缺点与跨攻击类型的通用性，为防护栏的优化与后续研究提供结构化基础。


### URLcoat: Exploiting Web Search Capability to Jailbreak Large Language Models.
- **作者**: Yiheng Sun, Linkang Du, Zhou Su, Yuntao Wang, Han Liu
- **解读**: 本文提出 URLcoat，利用 LLM 的联网搜索能力构造越狱攻击。攻击通过操纵搜索结果或诱导模型访问特定 URL 内容，使模型绕过安全对齐生成违规输出，揭示联网能力引入的新攻击面。（无摘要，据标题推断）


### MetaBreak: Jailbreaking Online LLM Services via Special Token Manipulation.
- **作者**: Wentian Zhu, Zhen Xiang, Wei Niu, Le Guan
- **解读**: 本文发现 LLM 微调中用于标注结构化对话的特殊 token（special tokens）可被利用构造四种攻击原语，从而同时绕过在线 LLM 服务的内部安全对齐与 SOTA 外部内容审核系统。作者提出 MetaBreak 方法：由于移除特殊 token 的输入净化防御可被替换为语义高相似的 token 规避，此类防御效果有限。在实验室与商业平台上的评估显示其越狱率与 SOTA 提示工程方法相当，且在对抗审核时优于 PAP 与 GPTFuzzer 分别 11.6% 与 34.8%；与提示工程策略互补，结合使用可进一步提升攻击效果。


### SoK: Robustness in Large Language Models against Jailbreak Attacks.
- **作者**: Feiyue Xu, Hongsheng Hu, Chaoxiang He, Sheng Hang, Hanqing Hu, Xiuming Liu, Yubo Zhao, Zhengyan Zhou, Bin Benjamin Zhu, Shifeng Sun, Dawu Gu, Shuo Wang
- **arXiv PDF**: [SoK: Robustness in Large Language Models against Jailbreak Attacks.](https://arxiv.org/pdf/2605.05058)
- **解读**: 本文提出针对 LLM 越狱攻击的攻防系统化分类法，并引入统一的多维评估框架 Security Cube，以弥补现有评估只依赖攻击成功率等窄指标的不足。作者给出攻防方法的详细对比表，并基于该框架对 13 种攻击与 5 种防御进行基准研究，覆盖越狱攻击、防御、自动评测器与 LLM 漏洞全景，提炼关键发现、未解决问题并指出提升 LLM 鲁棒性的研究方向。


### Parasites in the Toolchain: A Large-Scale Analysis of Attacks on the MCP Ecosystem.
- **作者**: Shuli Zhao, Qinsheng Hou, Zihan Zhan, Yanhao Wang, Yuchong Xie, Yu Guo, Libo Chen, Shenghong Li, Zhi Xue
- **解读**: 本文针对 Model Context Protocol（MCP）生态识别出一类系统性隐私泄露模式「寄生式工具链攻击」（Parasitic Toolchain Attacks），其实例化为 MCP 非预期隐私泄露（MCP-UPD）：攻击者无需与受害者直接交互，而是将恶意指令嵌入 LLM 在合法任务中访问的数据源，把多个工具组装成协同工作流完成数据窃取。该攻击分寄生摄取、隐私收集、泄露三个阶段，根因是 MCP 缺乏上下文-工具隔离与最小权限执行。作者设计 MCP-SEC 对生态进行首次大规模普查，分析 1360 个服务器上的 12230 个资源，发现大量真实可利用的恶意构件，凸显 LLM 集成环境中的系统性风险与防御需求。


### Ensemble Conformal Predictor (EnCP): A New Conformal Predictor with Robustness Guarantees Against Data Poisoning Attacks.
- **作者**: Yuxin Yang, Qiang Li, Runyang Feng, Liren Shan, Binghui Wang
- **解读**: 本文提出集成共形预测器（EnCP），一种新的共形预测方法，并给出针对数据投毒攻击的鲁棒性保证。通过集成机制增强预测集合的稳定性，使训练数据被污染时仍能维持有效的置信预测。（无摘要，据标题推断）


### Exploiting Leaderboards for Large-Scale Distribution of Malicious Models.
- **作者**: Anshuman Suri, Harsh Chaudhari, Yuefeng Peng, Ali Naseh, Alina Oprea, Amir Houmansadr
- **解读**: 本文揭示模型排行榜（model leaderboards）可作为大规模隐蔽分发恶意模型的渠道，并提出 TrojanClimb 通用框架，在保持排行榜竞争性排名的同时注入恶意行为。在文本嵌入、文本生成、语音合成、文生图四种模态上验证其有效性，可占据高排名并嵌入从后门到偏见注入的任意有害功能，凸显生态中缺乏对（被投毒）模型检测过滤机制的严重漏洞。


### GraphRAG Under Fire.
- **作者**: Jiacheng Liang, Yuhui Wang, Changjiang Li, Tanqiu Jiang, Rongyi Zhu, Neil Gong, Ting Wang
- **解读**: 本文研究图增强检索增强生成（GraphRAG）对投毒攻击的脆弱性，发现一个悖论：由于基于图的索引与检索，现有 RAG 攻击对 GraphRAG 的效果弱于传统 RAG，但图结构同时创造了新的攻击面。作者提出 GragPoison，利用图底层共享关系构造可同时危害多个查询的文本，采用关系注入引入虚假知识、增强放大影响、叙事将恶意内容嵌入连贯文本三种策略。实验表明其在有效性（成功率最高 98%）与可扩展性（文本用量少 68%）上显著优于现有攻击，并初步探讨防御措施。


### AI Wrote My Paper and All I Got was This False Negative:* Measuring the Efficacy of Commercial AI Text Detectors.
- **作者**: Seth Layton, Bernardo B. P. Medeiros, Kevin R. B. Butler, Patrick Traynor
- **解读**: 本文实证测量商用 AI 文本检测器识别 AI 生成内容的实际效果，重点关注假阴性（漏检）情形。研究评估检测器在真实写作场景（如论文）中的可靠性及其被规避的难易程度。（无摘要，据标题推断）


### Your Compiler is Backdooring Your Model: Understanding and Exploiting Compilation Inconsistency Vulnerabilities in Deep Learning Compilers.
- **作者**: Simin Chen, Jinjun Peng, Yixin He, Junfeng Yang, Baishakhi Ray
- **解读**: 本文揭示深度学习（DL）编译器存在根本性设计漏洞：未修改的官方编译器可能在编译过程中改变模型语义并植入隐藏后门。在对抗场景下，作者构造良性模型，其触发器在编译前无任何效果、编译后却成为有效后门；在 6 个模型、3 个商用编译器、2 个硬件平台上攻击对触发输入 100% 成功且保持正常准确率、不被 SOTA 检测器发现。在自然场景下分析 HuggingFace 前 100 模型（含下载量超 2.2 亿的模型）发现 31 个受影响，表明无需恶意操纵即存在风险。


### DREAM: Scalable Red Teaming for Text-to-Image Generative Systems via Distribution Modeling.
- **作者**: Boheng Li, Junjie Wang, Yiming Li, Zhiyang Hu, Leyi Qi, Jianshuo Dong, Run Wang, Han Qiu, Zhan Qin, Tianwei Zhang
- **解读**: 本文提出 DREAM，一个可扩展的自动化红队框架，用于发现文生图（T2I）系统中的问题提示词。与逐提示优化的现有方法不同，DREAM 直接建模目标系统提示词的概率分布，显式权衡有效性与多样性并支持训练后大规模采样；受能量模型启发将目标重写为简单可处理形式，无需代表性训练样本。作者提出 GC-SPSA 算法在长且可能不可微的流水线上提供稳定梯度估计，并加入多样性感知采样策略，实验证明在多种过滤器下达到 SOTA 成功率与多样性。


### On the (In)Security of Loading Machine Learning Models.
- **作者**: Gabriele Digregorio, Marco Di Gennaro, Stefano Zanero, Stefano Longari, Michele Carminati
- **解读**: 本文评估模型共享生态（hub）的安全态势：多数平台对加载共享模型的风险仅部分应对，且常将责任转移给用户；对广告/设置机制的分析还发现多个可实现任意代码执行的 0-day 漏洞。研究进一步调查开发者对共享风险的认知，指出尽管存在「安全加载」叙事，仅靠文件格式无法保证安全，该叙事反而使加载被误认为可信。作者据此提出强化模型共享生态的建议。


### Evaluating Concept Filtering Defenses against Child Sexual Abuse Material Generation by Text-to-Image Models.
- **作者**: Ana-Maria Cretu, Klim Kireev, Amro Abdalla, Wisdom Obinna, Raphael Meier, Sarah Adel Bargal, Elissa M. Redmiles, Carmela Troncoso
- **解读**: 本文评估从训练数据中过滤儿童图像以阻止文生图模型生成儿童性虐待材料（CSAM）的防御效果：先用基于博弈的安全定义刻画防 CSAM 生成的复杂性，再证明现有检测方法无法完全剔除数据集中的儿童图像。以「戴眼镜」作为伦理代理实验表明，即便过滤后仅剩少量样本，提示策略仍可在比未过滤训练多若干次查询内生成对应图像，微调可进一步降低额外查询开销，且过滤不完美时还可通过微调重新引入概念。结论是此类防御对闭源模型保护有限、对开源模型无效，且会损害模型对儿童相关概念的通用性，并指出建立稳健评估证据的挑战。


### Hijacking Large Audio-Language Models via Context-Agnostic and Imperceptible Auditory Prompt Injection.
- **作者**: Meng Chen, Kun Wang, Li Lu, Jiaheng Zhang, Tianwei Zhang
- **arXiv PDF**: [Hijacking Large Audio-Language Models via Context-Agnostic and Imperceptible Auditory Prompt Injection.](https://arxiv.org/pdf/2604.14604)
- **解读**: 本文揭示并系统化研究「听觉提示注入」（auditory prompt injection）威胁：在仅有音频数据访问与强感知隐蔽性的现实约束下，恶意音频可劫持大型音频语言模型（LALM）。作者提出 AudioHijack 通用框架，通过基于采样的梯度估计实现跨模型端到端优化以绕过不可微的音频分词，借助注意力监督与多上下文训练使对抗音频泛化到未见用户上下文，并用卷积混合把扰动调制成自然混响实现不可感知。在 13 个 SOTA LALM 上、6 类不当行为中平均成功率达 79%-96%，真实世界实验显示可诱导 Mistral AI 与微软 Azure 的商用语音代理执行未授权操作。


### Adversarial Hubness in Multi-Modal Retrieval.
- **作者**: Tingwei Zhang, Fnu Suya, Rishi D. Jha, Collin Zhang, Vitaly Shmatikov
- **解读**: 本文研究攻击者如何利用高维向量空间的「枢纽点」（hubness）现象，将多模态检索系统中任意图像或音频输入变成对抗性枢纽，使其被数千个不同查询检索到，从而注入通用内容（如垃圾信息）或针对特定概念的查询实施定向攻击。作者给出构造方法并在 Pinecone 实现的文-图检索基准上评估：单个用 100 个随机样本生成的枢纽可作为超过 21000 个（共 25000 个）测试查询的 top-1 结果，而普通枢纽仅覆盖 102 个查询，泛化能力极强；现有缓解技术对定向对抗枢纽无效。


### Recovering and Rehosting Mobile Local LLM Conversations and Contexts via Memory Forensics.
- **作者**: Haichuan Xu, David Oygenblik, Runze Zhang, Mingxuan Yao, Muhammad Ibrahim, Brendan Saltaformaggio
- **解读**: 本文研究通过内存取证（memory forensics）从移动端本地 LLM 应用中恢复对话记录与上下文状态，并可将其重放到其他环境中。工作展示移动端本地大模型运行时的敏感数据留存风险与取证分析可行性。（无摘要，据标题推断）


### Nonlocalizable Jamming with Curving Beams.
- **作者**: Caroline Jane Spindel, Edward W. Knightly
- **解读**: 本文研究利用可弯曲波束（curving beams）实现无法定位（nonlocalizable）的干扰攻击。由于波束可沿弯曲路径传播，干扰源位置难以被反制方定位，从而提升干扰攻击的隐蔽性与生存能力。（无摘要，据标题推断）


### WRATH: Turning Watermark Robustness Against Itself via a Watermark-Agnostic Black-Box Invalidation Attack.
- **作者**: Nan Jiang, Juan Hu, Bangjie Sun, Terence Sim, Jun Han
- **解读**: 本文提出 WRATH，一种与水印算法无关（watermark-agnostic）的黑盒失效攻击，将水印鲁棒性本身用作攻击工具。攻击无需了解具体水印方案即可使其失效，破坏内容水印在版权保护与溯源中的作用。（无摘要，据标题推断）


### Are LLM-Enhanced Graph Neural Networks Robust Against Poisoning Attacks?
- **作者**: Yuhang Ma, Jie Wang, Zheng Yan
- **arXiv PDF**: [Are LLM-Enhanced Graph Neural Networks Robust Against Poisoning Attacks?](https://arxiv.org/pdf/2603.26105)
- **解读**: 本文提出系统评估 LLM 增强图神经网络（GNN）在投毒攻击下鲁棒性的框架：组合 8 种 LLM/LM 特征增强器与 3 种 GNN 骨干构成 24 个受害者模型，覆盖 6 种结构投毒攻击与字符、词、句子三层面的文本投毒攻击，并在 4 个真实数据集（含 LLM 出现后发布、避免预训练泄露的数据集）上评测。实验表明 LLM 增强 GNN 相比浅层嵌入基线在各类攻击下准确率显著更高、相对准确率下降（RDA）更低，其鲁棒性来自节点表示对结构与标签信息的有效编码；作者据此提出新的组合攻击与图净化防御。


### Breaking Free from Ivory Tower: Evaluating and Enhancing Real-world Chinese Underground Adversarial Jargon Detection.
- **作者**: Zhifan Jiang, Mingxuan Liu, Yue Qin, Baojun Liu
- **解读**: 本文评估并增强针对真实中文地下对抗性黑话（underground adversarial jargon）的检测能力，弥合学术研究场景与真实网络环境之间的差距。工作旨在提升模型对中文隐语变体与对抗性改写文本的识别鲁棒性。（无摘要，据标题推断）


### WebCloak: Characterizing and Mitigating Threats From LLM-Driven Web Agents as Intelligent Scrapers.
- **作者**: Xinfeng Li, Tianze Qiu, Yingbin Jin, Lixu Wang, Hanqing Guo, Xiaojun Jia, Xiaofeng Wang, Wei Dong
- **解读**: 本文刻画 LLM 驱动的网页智能体（web agents）作为「智能爬虫」带来的威胁，包括大规模数据抓取与规避反爬机制等滥用行为，并提出 WebCloak 进行表征与缓解。研究为网站运营者理解并防御 LLM 智能体驱动的抓取提供方法。（无摘要，据标题推断）


### GHost in the Shell: A GPU-to-Host Memory Attack and its Mitigation.
- **作者**: Sihyun Roh, Woohyuk Choi, Jaeyoung Chung, Yoochan Lee, Suhwan Song, Byoungyoung Lee
- **解读**: 本文提出 GHost in the Shell，一种 GPU 到主机内存的攻击方法，利用 GPU 访问主机内存的能力突破隔离边界，并给出相应缓解方案。攻击展示 GPU 侧代码可读写主机敏感内存数据的安全风险。（无摘要，据标题推断）


### Demystifying and Exploiting ASLR on NVIDIA GPUs.
- **作者**: Ruofan Zhu, Ganhao Chen, Wenbo Shen, Lyuye Zhang, Dakun Shen, Rui Chang, Yanan Guo
- **解读**: 本文揭示并利用 NVIDIA GPU 上地址空间布局随机化（ASLR）的实现机制与弱点。工作表明 GPU 侧 ASLR 可被绕过或预测，从而为 GPU 上的内存破坏攻击提供利用基础。（无摘要，据标题推断）


### Phoenix: Rowhammer Attacks on DDR5 with Self-Correcting Synchronization.
- **作者**: Diego Meyer, Patrick Jattke, Michele Marazzi, Salman Qazi, Daniel Moghimi, Kaveh Razavi
- **解读**: 本文提出 Phoenix，一种针对 DDR5 内存的 Rowhammer 攻击，采用自校正同步（self-correcting synchronization）技术应对 DDR5 引入的片上 ECC 等缓解机制。该方法使 Rowhammer 位翻转攻击在 DDR5 平台上依然可行。（无摘要，据标题推断）


### GPUBreach: Privilege Escalation Attacks on GPUs Using Rowhammer.
- **作者**: Chris S. Lin, Yuqin Yan, Guozhen Ding, Joyce Qu, Joseph Zhu, David Lie, Gururaj Saileshwar
- **arXiv PDF**: [GPUBreach: Privilege Escalation Attacks on GPUs Using Rowhammer.](https://arxiv.org/pdf/2605.03812)
- **解读**: 本文证明 GPU 上的 Rowhammer 攻击可与 CPU 同等强大：通过操纵 GPU 页表管理以识别新页表的分配时机与位置，非特权用户的一个 CUDA 内核可利用 Rowhammer 位翻转定向篡改驻留 GPU 内存的页表，从而访问其他进程或共租户的 GPU 内存。这是首个 GPU 侧提权攻击，可泄露 cuPQC 库中的密码学密钥、篡改模型 GPU 汇编代码实现更隐蔽的模型降质；进一步可突破 IOMMU 防护升级为 CPU 侧提权，使恶意用户态程序获得 root shell 与系统级控制，即便在非多租户环境下亦然。


### GDDRHammer: Greatly Disturbing DRAM Rows - Cross-Component Rowhammer Attacks From Modern GPUs.
- **作者**: Yichang Hu, Noah Brown, Yuhang Chen, Joshua Bakita, Tianlong Chen, Daniel Genkin, Andrew Kwong
- **解读**: 本文提出 GDDRHammer，从现代 GPU 发起跨组件（cross-component）Rowhammer 攻击，显著扰动 DRAM 行。攻击利用 GPU 的访存行为影响（主机）内存行，突破 GPU 与主机内存之间的隔离边界。（无摘要，据标题推断）


### GeForge: Hammering GDDR Memory to Forge GPU Page Tables for Fun and Profit.
- **作者**: Junpeng Wan, Yanan Guo, Zhi Zhang, Zhuo Li, Dave Jing Tian, Zhenkai Zhang
- **解读**: 本文提出 GeForge，一种利用 GDDR 内存位翻转实现系统级攻陷的端到端 Rowhammer 攻击：通过破坏 GPU 页表夺取地址翻译控制，实现对整个 GPU 内存的任意访问，并利用特殊映射特性把攻击范围延伸到主机内存。作者发展出将离线物理映射对齐到运行时分配的方法、借助厂商驱动分配器把目标表结构推入易受攻击位置的内存搬运策略，以及触发更多位翻转的改进锤击模式，消除了先前工作的限制性假设，在消费级与工作站级 NVIDIA GPU 上默认配置即可生效，可跨上下文任意读写数据并提权至系统 root。


### Defeating Transient Execution Attacks by Limiting Secret Reachability Through Register Hiding and ShadowCFI.
- **作者**: Daniël Trujillo, Jagadish Kotra, David Kaplan, Mengjia Yan
- **解读**: 本文提出通过寄存器隐藏（register hiding）与影子控制流完整性（ShadowCFI）限制秘密数据可达性的防御机制，以抵御瞬态执行攻击。通过减少秘密在架构状态中的暴露路径与控制流劫持可能，阻断瞬态侧信道的数据泄露。（无摘要，据标题推断）


### Transient Architectural Execution: From Weird Gates to Weird Programs.
- **作者**: Ping-Lun Wang, Fraser Brown, Riccardo Paccagnella, Eyal Ronen, Riad S. Wahby, Yuval Yarom
- **解读**: 本文探索「瞬态架构执行」（transient architectural execution）现象，研究瞬态指令在架构层面产生的非规范行为，并将其从「怪异门电路」推向「怪异程序」的利用。工作系统刻画该类瞬态执行原语及其安全影响。（无摘要，据标题推断）


### SeqAss: Using SeqUential Associative Caches to Mitigate Conflict-Based Cache Attacks with Reduced Cache Misses and Performance Overhead.
- **作者**: Wei Song, Zhidong Wang, Jinchi Han, Da Xie, Hao Ma, Peng Liu
- **解读**: 本文提出 SeqAss，一种顺序关联缓存（sequential associative cache）设计，用于缓解基于冲突的缓存侧信道攻击，同时降低缓存缺失率与性能开销。该设计在保持抗冲突攻击能力的同时改善缓存命中性能。（无摘要，据标题推断）


### Trevex: A Black-Box Detection Framework for Data-Flow Transient Execution Vulnerabilities.
- **作者**: Daniel Weber, Fabian Thomas, Leon Trampert, Ruiyi Zhang, Michael Schwarz
- **解读**: 本文提出 Trevex，一种黑盒检测框架，用于发现数据流型瞬态执行漏洞（如 Meltdown 类）。框架无需依赖具体微架构内部信息即可检测目标处理器是否易受该类瞬态执行攻击。（无摘要，据标题推断）


### AESpoly: Symmetric-Key Cryptographic Designs Using Instruction-Level Parallelism Between AES and Polynomial Hash.
- **作者**: Yukihito Hiraga, Yusuke Naito, Yu Sasaki, Takeshi Sugawara
- **解读**: 本文提出 AESpoly，利用 AES 轮操作与多项式哈希（如 GCM 类认证加密中的哈希）之间的指令级并行（ILP）设计对称密码实现。通过重叠两类计算提升吞吐与效率，面向现代处理器指令集优化认证加密性能。（无摘要，据标题推断）


### Crucible: Retrofitting Commodity CPUs with Vulnerabilities via Transparent Software Emulation.
- **作者**: Tristan Hornetz, Lukas Gerlach, Michael Schwarz
- **解读**: 本文提出 Crucible，通过透明软件模拟（transparent software emulation）在商用 CPU 上「植入」漏洞，为安全研究提供可控的实验环境。该方法无需修改硬件即可复现特定漏洞行为，便于教学、测试与防御验证。（无摘要，据标题推断）


### RISCy Cache Coherence: Timer-Free Architectural Cache Attacks via Instruction/Data Cache Incoherence.
- **作者**: Fabian Thomas, Michael Schwarz
- **解读**: 本文利用 RISC 处理器指令/数据缓存不一致（incoherence）现象，实现无需计时器的架构级缓存攻击。通过指令缓存与数据缓存对同一地址的视图差异泄露信息，规避依赖高精度计时器的传统侧信道检测。（无摘要，据标题推断）


### "I Wonder if These Warnings are Accurate": Security and Privacy Advice in Nine Majority World Countries.
- **作者**: Collins W. Munyendo, Veronica A. Rivera, Jackie Hu, Emmanuel Tweneboah, Amna Shahnawaz, Karen Sowon, Dilara Keküllüoglu, Marcos Silva, Yue Deng, Mercy Omeiza, Gayatri Priyadarsini Kancherla, Marianne Batista Diniz Da Silva, Abhishek Bichhawat, Maryam Mustafa, Francisco J. Marmolejo Cossío, Elissa M. Redmiles, Yixin Zou
- **解读**: 本文研究九个「多数世界」（Majority World）国家中安全与隐私建议的传播与准确性认知。研究考察当地用户如何获取、理解与质疑安全建议，及其与本地社会文化语境的适配情况。（无摘要，据标题推断）


### Privacy Perspectives and Practices of Chinese Smart Home Product Teams.
- **作者**: Shijing He, Yaxiong Lei, Xiao Zhan, Chi Zhang, Juan Ye, Ruba Abu-Salma, Jose Such
- **解读**: 本文通过对 27 名中国智能家居产品团队成员（产品/项目经理、软硬件工程师、UX 设计师、法律/隐私专家、市场/运营人员）的半结构化访谈，考察其对隐私的视角、实践与风险缓解策略。受访者普遍强调对数据法规的合规遵从，且往往将国家权益置于个人权利之上；中国特有的文化、社会与法律因素影响了伦理考量和便利性权衡态度。作者据此提出面向多用户家庭中弱势群体的社会技术干预与改进建议。


### Searching for a Farang: Collective Security Among Women in Pattaya, Thailand.
- **作者**: Taylor Robinson, Rikke Bjerg Jensen
- **解读**: 本文基于在泰国芭堤雅一家妇女中心两个月的民族志田野调查与 76 名参与者的访谈，报告性工作/按摩行业女性在数字安全方面的实践：她们将与非西方男性（farang）的关系视为获得安全的最佳甚至唯一途径，并模仿酒吧老板吸引顾客的方式在线上展示自己；同时利用被贴上「西方理想」标签的技术为其谋利，在应对诈骗与虐待等在线风险中形成通过知识共享保护彼此的集体安全机制。研究将其置于边缘化群体安全研究脉络中。


### Toward Inclusive Security and Privacy for Deaf and Hard-of-Hearing People: A Community-Based Interview Study.
- **作者**: Mindy Tran, Xinru Tang, Adryana Hutchinson, Adam J. Aviv, Yixin Zou
- **解读**: 本文通过基于社区的访谈研究，考察聋人与听障（DHH）群体面临的安全与隐私挑战。研究旨在理解该群体在认证、通信与信息获取中的独特困难，并提出包容性的安全隐私设计方向。（无摘要，据标题推断）


### XDup: Privacy-Preserving Deduplication for Humanitarian Organizations Using Fuzzy PSI.
- **作者**: Tim Rausch, Sylvain Chatel, Wouter Lueks
- **arXiv PDF**: [XDup: Privacy-Preserving Deduplication for Humanitarian Organizations Using Fuzzy PSI.](https://arxiv.org/pdf/2604.08019)
- **解读**: 本文面向人道主义组织的跨组织重复登记检测需求，提出 xDup 隐私保护去重系统，在避免向其他组织泄露受助者敏感数据的同时检测重复注册，比现有方案快两个数量级。其核心是 otFPSI，一种在海明空间上无需输入假设的具体高效模糊 PSI 协议，效率优于现有模糊 PSI 方案，并支持秘密共享输出等扩展。


### Human-Centered Threat Modeling in Practice: Lessons, Challenges, and Paths Forward.
- **作者**: Warda Usman, Yixin Zou, Daniel Zappala
- **arXiv PDF**: [Human-Centered Threat Modeling in Practice: Lessons, Challenges, and Paths Forward.](https://arxiv.org/pdf/2511.13781)
- **解读**: 本文通过对 23 名研究者的半结构化访谈考察人本威胁建模（HCTM）的实践现状，发现 HCTM 并非规定性流程，而是由参与者关系、学科背景与制度结构塑造的不断演化的实践集合，研究者以持续的基础工作与以参与者为中心的探究展开，并受关怀、正义、自主等价值观引导。研究同时指出情感负担、伦理困境与结构性障碍等挑战，并建议通过共享基础设施、认可多样化贡献、强化研究成果向政策与设计转化的机制推进 HCTM。


### LISA: A Scale-Optimized and Psychometrically-Validated Instrument for the Lightweight Assessment of Organizational Information Security Awareness in Heterogeneous Organizations.
- **作者**: David Langer, Jan Tolsdorf, Luigi Lo Iacono
- **解读**: 本文提出 LISA，一种经心理测量学验证、面向异构组织的轻量级信息安全意识评估量表。该量表在尺度上进行优化，使组织能够低成本地测量员工的信息安全意识水平。（无摘要，据标题推断）


### Perceived Privacy Risk and Mitigation Post-Roe.
- **作者**: Alan F. Luo, Phoebe Moh, Cora Sula, Michelle L. Mazurek, Nora McDonald
- **解读**: 本文研究美国 Roe 案被推翻后用户对隐私风险的感知及其缓解行为。研究考察人们（尤其可能涉及生育相关数据的人群）如何重新评估位置、健康等敏感数据的隐私风险并采取应对措施。（无摘要，据标题推断）


### Breaking the Illusion: Automated Reasoning of GDPR Consent Violations.
- **作者**: Ying Li, Wenjun Qiu, Faysal Hossain Shezan, Kunlin Cai, Michelangelo van Dam, Lisa M. Austin, David Lie, Yuan Tian
- **arXiv PDF**: [Breaking the Illusion: Automated Reasoning of GDPR Consent Violations.](https://arxiv.org/pdf/2512.22789)
- **解读**: 本文提出 Cosmic，一个自动检测网页表单中与同意相关的隐私违规的框架，填补现有研究聚焦 cookie 横幅与移动应用弹窗而忽视功能型网页表单的空白。在 5823 个网站、3598 个表单上的评估中检测出 3384 项违规，覆盖 94.1% 的同意表单，涉及自由给出同意、目的披露、撤回选项等 GDPR 核心原则，同意与违规检测的 TPR 分别达 98.6% 与 99.1%。


### Understanding and Analyzing Privacy Risks in Mobile Consent-Management Platforms.
- **作者**: Jingzhou Ye, Fares Alharbi, Luyi Xing, Xueqiang Wang
- **解读**: 本文理解并分析移动端同意管理平台（CMP）的隐私风险，考察其在同意获取、数据收集披露与合规方面的实现问题。研究旨在揭示 CMP 在移动应用生态中的隐私影响与治理缺口。（无摘要，据标题推断）


### Convenience at a Cost: the Security Risks of Template-Based Development in the App-in-App Ecosystem.
- **作者**: Yizhe Shi, Zhemin Yang, Yifan Yang, Yunteng Yang, Min Yang
- **解读**: 本文分析小程序（app-in-app）生态中基于模板的开发方式带来的安全风险。研究考察模板化开发如何引入通用漏洞、权限滥用与供应链问题，揭示便利性背后的安全代价。（无摘要，据标题推断）


### When VR Meets BCI: (Un)Observable Brainwave-Aware Privacy Reconstruction in the Metaverse via Unrestricted Inbuilt Motion Sensors.
- **作者**: Tao Ni, Zehua Sun, Qingchuan Zhao, Wei-Bin Lee, Cong Wang
- **arXiv PDF**: [When VR Meets BCI: (Un)Observable Brainwave-Aware Privacy Reconstruction in the Metaverse via Unrestricted Inbuilt Motion Sensors.](https://arxiv.org/pdf/2606.10502)
- **解读**: 本文揭示元宇宙设备中超出可观察行为的隐私泄露：VR 头显内置运动传感器（如加速度计）可捕捉由瞳孔反应引起的细微振动，这些振动与视觉刺激和脑内感知高度相关，从而重建与脑电（EEG）相关的表征。作者实现 BraVeSpy，在不同 VR 设备上以 52.0%-67.2% 的准确率还原用户脑中的感知图像，并在推断网站/应用/流媒体指纹等敏感活动信息上超过 85.0% 准确率、在用户去匿名化、视线追踪与虚拟键盘输入推断上超过 96.0%，首次在元宇宙中实现不可观察隐私的泄露。


### Secret State Leakage Attacks and Their Impacts on EMV Contactless Payment Apps.
- **作者**: Jesse Chen, Rubin Yuchan Yang, Ahmad Musa, Syed Rafiul Hussain, Omar Chowdhury, Sazzadur Rahaman
- **解读**: 本文研究针对 EMV 非接触支付应用的秘密状态泄露（secret state leakage）攻击及其影响。攻击通过泄露应用内部秘密状态，威胁交易认证与支付安全。（无摘要，据标题推断）


### Navigating Developers' Quagmire: LLM-Enabled Privacy Compliance Analysis for SDK Integrations.
- **作者**: Zhaojie Hu, Xueqiang Wang
- **解读**: 本文提出利用大语言模型辅助开发者进行 SDK 集成的隐私合规分析，帮助导航 SDK 数据收集、权限声明与隐私政策之间的复杂关系。研究旨在降低开发者评估第三方 SDK 隐私风险的门槛。（无摘要，据标题推断）


### LLMThief: Evaluating Configuration Leaking Risks in Commercial LLM App Stores.
- **作者**: Pinji Chen, Jinlong Jiang, Jianjun Chen, Feiran Qin, Minghao Zhang, Jiahe Zhang, Haixin Duan, Kaiwen Shen, Hui Jiang
- **解读**: 本文评估商用 LLM 应用商店中的配置泄露风险，考察应用配置（如系统提示词、API 密钥、模型参数）是否可被提取。研究揭示应用商店分发模式下敏感配置被窃取的攻击面。（无摘要，据标题推断）


### LEAKYLINKS: Measuring the Security and Privacy Risks of URL Scanning Services.
- **作者**: Ali Mustafa, Jannis Rautenstrauch, Florian Hantke, Shubham Agarwal, Stefano Calzavara, Ben Stock
- **解读**: 本文测量 URL 扫描服务的安全与隐私风险，考察用户提交链接进行安全检测时，其数据（URL 内容、元数据）的暴露范围与滥用可能。研究评估该服务模型本身引入的隐私泄露问题。（无摘要，据标题推断）


### Concretely-Efficient Multi-Key Homomorphic Secret Sharing and Applications.
- **作者**: Kaiwen He, Sacha Servan-Schreiber, Geoffroy Couteau, Srinivas Devadas
- **解读**: 本文构造具体高效（concretely-efficient）的多密钥同态秘密共享（multi-key homomorphic secret sharing）方案并给出应用。工作优化协议的常数因子开销，使多参与方安全计算更接近实用。（无摘要，据标题推断）


### No Honor Among Crooks: Non-Transferable Anonymous Tokens from Betrayability.
- **作者**: David Kretzler, Yong Li
- **解读**: 本文基于「可背叛性」（betrayability）构造不可转让的匿名令牌：若令牌持有者共享或转售令牌，协议允许「背叛」其身份以作惩罚。该设计在匿名性之外提供防共享机制，适用于凭证与访问控制场景。（无摘要，据标题推断）


### Revisiting PQ Wireguard: A Comprehensive Security Analysis with a New Design Using Reinforced KEMs.
- **作者**: Keitaro Hashimoto, Shuichi Katsumata, Guilhem Niot, Thom Wiggers
- **解读**: 本文对后量子 WireGuard 进行全面的安全分析，并提出使用强化 KEM（reinforced KEMs）的新设计。研究重新审视现有 PQ WireGuard 方案的弱点，给出更安全的后量子密钥封装集成方式。（无摘要，据标题推断）


### Can I Get More? An Incremental Inference Attack on Encrypted SQL.
- **作者**: Xiaoqian Sun, Ruiqi He, Yang Zhang, Siyi Lv, Guiyun Qin, Fangzhou Yi, Zheli Liu, Xiaofeng Chen
- **解读**: 本文提出针对加密 SQL 数据库的增量推断攻击，通过多次查询逐步推断加密列中的明文信息。攻击利用查询结果与辅助信息，在保持合法查询的前提下累积泄露数据。（无摘要，据标题推断）


### Starfighters-On the General Applicability of X-Wing.
- **作者**: Deirdre Connolly, Kathrin Hövelmanns, Andreas Hülsing, Stavros Kousidis, Matthias Meijers
- **解读**: 本文研究 X-Wing（基于 ML-KEM 与 HQC 混合的后量子密钥封装机制）的通用适用性（general applicability）。工作考察该设计范式在更广泛场景与构造中的可迁移性。（无摘要，据标题推断）


### A Leakage-Free Framework for Private Set Operations.
- **作者**: Wenhao Wu, Yuyue Chen, Bowen Shen, Peng Yang, Ximing Fu, Zoe Lin Jiang, Junbin Fang
- **解读**: 本文提出无泄露（leakage-free）的私有集合操作（private set operations）框架，消除现有协议在集合交集、并集等操作中泄露交集大小等元数据的缺陷。框架旨在为 PSI 类协议提供更强的隐私保证。（无摘要，据标题推断）


### Hardware Trojans from Invisible Inversions: On the Trojanizability of Standard Cell Libraries.
- **作者**: Kolja Dorschel, René Walendy, Lukas Plätz, Thorben Moos, Christof Paar, Steffen Becker
- **解读**: 本文深入分析 S&P 2023 公开的 SEM 图像数据集（四个不同工艺节点的数字 IC），设计更合理的指标评估标准单元库的「可木马化」（Trojanizability）程度，以区分插入策略与图像质量退化对检测性能的影响。结果显示不同工艺节点存在明显差异，但在所有情况下都能找到实现不同逻辑功能却在背面 SEM 图像上视觉无法区分的单元，并据此构造隐蔽的标准单元木马，以 Ibex RISC-V 内核的提权后门作为案例演示。作者建议将 Trojanizability 作为库的评估维度并提出实用防御。


### A Maliciously-Secure Post-Quantum OPRF from Crypto Dark Matter.
- **作者**: Diego F. Aranha, Aron van Baarsen, Adam Blatchley Hansen, Kent Nielsen, Peter Scholl
- **解读**: 本文基于「Crypto Dark Matter」范式的交替模数假设构造恶意安全（maliciously-secure）的不经意伪随机函数（OPRF）协议，此前的同类 OPRF 仅在半诚实模型下安全。作者利用新的 cut-and-choose 技术生成向量线性求值（VOLE）相关随机性，实现不同零知识与两方计算原语间的高效转换；相比 SOTA 的 GOLD OPRF（Yang, S&P 2025），在线阶段在所有设置下更快，小批量场景下整体效率更优，并支持秘密共享输出与秘密共享输入，可扩展至私有集合交集与数据库操作等应用。


### Scalable Accountable Byzantine Agreement and Beyond.
- **作者**: Pierre Civit, Daniel Collins, Vincent Gramoli, Rachid Guerraoui, Jovan Komatovic, Manuel Vidigueira, Pouriya Zarbafian
- **解读**: 本文提出可扩展（scalable）的可问责拜占庭协议（accountable Byzantine agreement），使协议在攻击者违背承诺时可被识别问责，并扩展到其他分布式系统原语。工作旨在兼顾共识的可扩展性与问责能力。（无摘要，据标题推断）

### Practical Asynchronous Distributed Key Reconfiguration and Its Applications.
- **作者**: Hanwen Feng, Yingzi Gao, Yuan Lu, Qiang Tang, Jing Xu
- **解读**: 针对异步网络中的分布式密钥重配置问题，提出实用的异步密钥重配置协议，并探讨其在分布式系统中的应用。侧重在无同步假设下安全高效地轮换/更新多方持有的密钥材料。（无摘要，据标题推断）


### Lattice-Based Threshold Blind Signatures.
- **作者**: Sebastian Faller, Guilhem Niot, Michael Reichle
- **解读**: 研究基于格假设的阈值盲签名方案。阈值化使签名私钥分布于多方，盲化保证签名过程不泄露消息，格基构造旨在提供抗量子安全性。本文构建相应的门限盲签名协议。（无摘要，据标题推断）


### A Full Threshold NIST PQC-Compliant Framework for Distributed Trust in Federal Public Key Infrastructure.
- **作者**: Kiarash Sedghighadikolaei, Changqi Sun, Thang Hoang, Bechir Hamdaoui, Attila A. Yavuz
- **解读**: 面向联邦公钥基础设施中的分布式信任，提出满足 NIST 后量子密码（PQC）标准、实现全阈值门限的框架，旨在以抗量子、可分散的方式管理联邦 PKI 的信任根与密钥。（无摘要，据标题推断）


### Mad-Dag: Protecting Blockchain Consensus From MEV.
- **作者**: Roi Bar Zur, Ittay Eyal, Aviv Tamar
- **解读**: 针对自私挖矿对区块链共识的威胁，指出既有防御协议 Colordag 未处理包括 MEV（可提取价值）在内的多种不利条件，且仅在网络延迟极高时才被证明安全。提出 MAD-DAG（相互确保毁灭有向无环图），通过新颖的账本函数——在竞争成为最长链的等长链之间丢弃其内容——在现实条件下实现首个实用的抗自私挖矿协议。


### Robot: Robust Threshold BBS+ in Two Rounds.
- **作者**: Guofeng Tang, Tian Qiu, Bowen Jiang, Haiyang Xue, Guomin Yang, Man Ho Au, Robert H. Deng, Kwok-Yan Lam
- **解读**: 针对 BBS+ 签名提出鲁棒（robust）的阈值化构造，仅需两轮交互。目标是在恶意敌手存在下，多方安全地联合生成和验证 BBS+ 签名，同时保持两轮的通信效率。（无摘要，据标题推断）


### New Constructions of Functional Adaptor Signatures: Broader Functions and Improved Efficiency.
- **作者**: Nikhil Vanjani, Garrett Greiner, Sri Aravinda Krishnan Thyagarajan, Pratik Soni
- **解读**: 研究函数自适应签名（Functional Adaptor Signatures）的新构造，目标在于支持更广泛的函数类并提升效率。自适应签名允许基于已签消息的见证进行签名适配，本文扩展其表达能力并优化计算与通信开销。（无摘要，据标题推断）


### Practical Multi-Party Private Set Intersection with Reducible Zero-Sharing.
- **作者**: Yewei Guan, Hua Guo, Man Ho Au, Jiarong Huo, Jin Tan, Zhenyu Guan
- **解读**: 针对多方隐私集合交集（MP-PSI），提出基于可约减零共享（reducible zero-sharing）的实用构造，在多方场景下降低计算与通信成本，实现高效的集合交集计算。（无摘要，据标题推断）


### Single-Server Stateful PIR with Verifiability and Balanced Efficiency.
- **作者**: Pranav Shriram Arunachalaramanan, Ling Ren
- **解读**: 研究单服务器、有状态（stateful）的隐私信息检索（PIR），在支持验证性（verifiability）的同时实现均衡的效率，即兼顾服务器存储、带宽与验证开销。（无摘要，据标题推断）


### ZELDA: Efficient Multi-Server Preprocessing PIR With Unconditional Security.
- **作者**: Ashrujit Ghoshal, Mingxun Zhou, Bo Peng, Elaine Shi
- **解读**: 针对多服务器预计算型 PIR，提出 ZELDA 协议，在预处理阶段准备数据、查询阶段高效应答，并实现无条件安全性（unconditional security），即不依赖任何计算性假设。（无摘要，据标题推断）


### Verifiable PIR with Small Client Storage.
- **作者**: Mayank Rathee, Keewoo Lee, Raluca Ada Popa
- **解读**: 研究客户端存储受限的可验证 PIR，目标是让客户端在仅保存少量数据的前提下，验证服务器返回的检索结果正确且完整，从而抵御恶意服务器。（无摘要，据标题推断）


### Euston: Efficient and User-Friendly Secure Transformer Inference with Non-Interactivity.
- **作者**: Xinwen Gao, Shaojing Fu, Lin Liu, Zhuotao Liu, Yuchuan Luo, Yongjun Wang
- **解读**: 面向安全 Transformer 推理提出 Euston，在保证高效性的同时实现用户友好且非交互（non-interactive）的安全多方计算，减少客户端与服务端之间的在线往返与部署负担。（无摘要，据标题推断）


### InsPIRe: Communication-Efficient PIR with Server-Side Preprocessing.
- **作者**: Rasoul Akhavan Mahdavi, Sarvar Patel, Joon Young Seo, Kevin Yeo
- **解读**: 提出 InsPIRe，一种通信高效的 PIR 方案，将预处理放到服务器端完成，从而在不显著增加客户端负担的前提下大幅降低在线查询的通信开销。（无摘要，据标题推断）


### LatORAM: ORAMs from Lateral Stashes and Delayed Shuffling.
- **作者**: Sarvar Patel, Giuseppe Persiano, Joon Young Seo, Kevin Yeo
- **解读**: 提出 LatORAM，一种不透明随机访问机（ORAM）构造，利用侧向 stash（lateral stash）与延迟洗牌（delayed shuffling）来降低摊销访问开销，实现更好的带宽与内存效率。（无摘要，据标题推断）


### VIA: Communication-Efficient Single-Server Private Information Retrieval.
- **作者**: Chenyang Liu, Xukun Wang, Zhifang Zhang
- **解读**: 提出 VIA，一种通信高效的单服务器隐私信息检索（PIR）方案，在单一服务器、计算性安全假设下显著降低查询的通信复杂度。（无摘要，据标题推断）


### Automated Formal Analysis of Signal's Double Ratchet: Attacks, Fixes and Security Proofs.
- **作者**: Vincent Cheval, Charlie Jacomme, Jessica Richards
- **解读**: 对 Signal 的 Double Ratchet 协议进行自动化形式化分析，发现攻击并给出修复方案与安全证明，旨在以机械化手段验证端到端加密会话密钥协议的安全性。（无摘要，据标题推断）


### Banshee: Target Switch Attacks on Gimbal-Stabilized Visual Tracking Systems via Acoustic Injection.
- **作者**: Jiarui Li, Joseph Brewington, Qingzhao Zhang, Z. Morley Mao
- **解读**: 针对无人机等自主系统中的云台稳定视觉跟踪，指出既有研究未充分考虑真实应用中的目标运动不确定性与运行时延迟等实际挑战。提出 Banshee，首个物理上可实现的攻击：通过精心构造的声波波形诱导优化后的对抗性振荡，使相机视野产生方向性漂移、破坏帧间关联，从而触发目标切换，实现对云台相机系统的攻击。


### Rain: Transiently Leaking Data from Public Clouds Using Old Vulnerabilities.
- **作者**: Mathé Hertogh, Dave Quakkelaar, Thijs Raymakers, Mahesh Hari Sarma, Marius Muench, Herbert Bos, Erik van der Kouwe
- **解读**: 针对瞬态执行漏洞（如 Spectre）长期缺乏现实云环境下的攻击报告、业界因而误以为软件防御已足够的问题，本文给出否定答案。利用老旧 CPU 上缺乏完整硅片级修复的漏洞，展示如何在实际云环境中以缺乏主机/客户机细节知识、存在噪声的条件下，透过软件防御瞬态地泄露数据，证明现有基于软件的缓解措施不足以阻止这类攻击。


### Chypnosis: Undervolting-based Static Side-channel Attacks.
- **作者**: Kyle Mitard, Saleh Khalaj Monfared, Fatemeh Khojasteh Dana, Robert Dumitru, Yuval Yarom, Shahin Tajik
- **解读**: 针对依赖停止时钟提取敏感信息的静态侧信道攻击，提出 Chypnosis 欠压（undervolting）攻击技术：通过降低供电间接停止目标电路时钟而保留存储数据，并绕过既有防御中清除状态的阶段。进一步说明如何快速下拉电源电压来压制面向故障注入的电压传感器响应，从而在存在此类防御时仍恢复秘密信息。


### SoK: Systematizing a Decade of Architectural Rowhammer Defenses Through the Lens of Streaming Algorithms.
- **作者**: Michael Jaemin Kim, Seungmin Baek, Jumin Kim, Hwayong Nam, Nam Sung Kim, Jung Ho Ahn
- **解读**: 以流算法视角系统化梳理过去十年的体系结构级 Rowhammer 防御。随着 RH 阈值下降威胁 DRAM 工艺缩放并抬高防护成本，体系结构方案作为第一道防线但领域术语与威胁模型混乱。本文对 48 项工作给出分类法，并映射到多种经典流算法，识别出此前未被指出的关联。


### RadKey: An LLM-Guided RF Backscatter System for Through-Wall Keystroke Inference.
- **作者**: Qijun Wang, Chunqi Qian, Huacheng Zeng
- **解读**: 提出 RadKey，一种基于 RF 反向散射的隐蔽、长距离、穿墙键盘窃听系统。其无电池标签捕获敲键引发的振动/声学信号，经磁耦合 LC 电路调制到反向散射信号的频率偏移上，由读卡器接收解调，从而在不依赖短距、可见传感器与受害者特定训练数据的情况下推断键盘输入。


### Beyond Indistinguishability: Measuring Extraction Risk in LLM APIs.
- **作者**: Ruixuan Liu, David Evans, Li Xiong
- **arXiv PDF**: [Beyond Indistinguishability: Measuring Extraction Risk in LLM APIs.](https://arxiv.org/pdf/2604.18697)
- **解读**: 针对差分隐私等不可区分性性质常被当作模型受保护（抵抗记忆/提取）的代理指标，本文证明不可区分性既不充分也不必要地防止 LLM API 中的数据提取。形式化抽离提取与不可区分性隐私的隐私博弈分离，说明两者不可比；进而提出 (l,b)-不可提取性定义，要求任何黑盒对手进行数据提取需至少 2^b 次期望查询，从而提供可测量的提取风险上界。


### Agentic Concolic Execution.
- **作者**: Zhengxiong Luo, Huan Zhao, Dylan Wolff, Cristian Cadar, Abhik Roychoudhury
- **解读**: 将符号执行与智能体（agent）机制相结合，提出 Agentic Concolic Execution，利用具备推理与决策能力的智能体引导符号执行以提升路径探索与漏洞发现效率。（无摘要，据标题推断）


### C-Verifier: Understanding and Formally Verifying Cross-Service Flaws in AWS Cognito.
- **作者**: Zhen Chen, Ze Jin, Le Gong, Kexin Chen, Xiangyi Zeng, Qixu Liu
- **解读**: 针对 AWS Cognito 中跨服务（cross-service）的缺陷，提出 C-Verifier 以理解并形式化验证此类安全缺陷，通过建模服务间交互来发现并证明配置与授权漏洞。（无摘要，据标题推断）


### VMSCAPE: Exposing and Exploiting Incomplete Branch Predictor Isolation in Cloud Environments.
- **作者**: Jean-Claude Graf, Sandro Rüegge, Ali Hajiabadi, Kaveh Razavi
- **解读**: 针对云环境中分支预测器隔离不完整的问题，提出 VMSCAPE 以暴露并利用该缺陷，证明不同虚拟机之间可通过分支预测器状态泄露信息，形成跨 VM 的攻击面。（无摘要，据标题推断）


### Practical Covert Channel Across Isolated Browser Instances via GPU Command Queue Contention.
- **作者**: Jinhong Liu, Zifeng Kang, Song Li, Yinzhi Cao
- **解读**: 研究隔离的浏览器实例之间通过 GPU 命令队列争用（contention）构建实用隐蔽信道，利用共享 GPU 硬件资源的时序竞争实现跨实例的信息传输。（无摘要，据标题推断）


### Detecting Privilege Escalation in Polyglot Microservices via Agentic Program Analysis.
- **作者**: Penghui Li, Hong Yau Chong, Yinzhi Cao, Junfeng Yang
- **arXiv PDF**: [Detecting Privilege Escalation in Polyglot Microservices via Agentic Program Analysis.](https://arxiv.org/pdf/2605.15569)
- **解读**: 针对多语言（polyglot）微服务中复杂的权限控制与跨服务交互，提出 Neo，一种结合大语言模型（LLM）与经典程序分析的智能体式程序分析框架。LLM 智能体动态生成分析计划、自适应调整代码搜索策略，用于检测服务间的权限提升漏洞。


### State of Browser Process-Isolation: The Same-Site Weakness.
- **作者**: Fabian Kilger, Hannah Fischer, Adrian Staeves, Robin Marchart, Josef Schönberger, Fabian Franzen
- **解读**: 系统研究浏览器进程隔离（process isolation）的安全现状，指出同站（same-site）隔离的弱点，分析当前隔离机制在何种情况下无法有效防御跨站点或同站攻击。（无摘要，据标题推断）


### KeyChaser: Unveiling API Keys in Browser Extensions.
- **作者**: Shijin Chen, Willy Susilo, Yudi Zhang, Fuchun Guo
- **解读**: 针对浏览器扩展中硬编码 API 密钥的泄露问题，提出 KeyChaser 以自动化地发现并揭示扩展内的 API 密钥，评估其泄露风险与暴露面。（无摘要，据标题推断）


### Site Isolation is Dead: How Site Isolation is Broken in Agentic Browsers and Extensions.
- **作者**: Suyoung Lee, Seongho Keum, Changoo Lee, Dongwon Shin, Sanghyun Hong, Byoungyoung Lee, Sooel Son
- **解读**: 研究 Site Isolation 机制在智能体浏览器（agentic browsers）与浏览器扩展中被破坏的问题，论证此类新形态浏览器如何使站点隔离失效、导致跨站数据泄露风险。（无摘要，据标题推断）


### The Secrets Must Not Flow: Scaling Security Verification to Large Codebases.
- **作者**: Linard Arquint, Samarth Kishor, Jason R. Koenig, Joey Dodds, Daniel Kroening, Peter Müller
- **解读**: 提出名为 Diodon 的方法论以将安全验证扩展到大型代码库：把代码库拆分为安全关键的实现部分（Core）与其余部分（Application）。对 Core 采用强力的半自动验证技术，对整体采用全自动静态分析以保证 Application 不会破坏已为 Core 证明的性质；目标是证明 I/O 独立性，即 Core 中涉及安全相关数据（如密钥）的 I/O 操作满足安全要求，从而在协议执行中安全地放开 I/O。


### Towards Practical Zero-Knowledge Proof for PSPACE.
- **作者**: Ashwin Karthikeyan, Hengyu Liu, Kuldeep S. Meel, Ning Luo
- **arXiv PDF**: [Towards Practical Zero-Knowledge Proof for PSPACE.](https://arxiv.org/pdf/2511.15071)
- **解读**: 此前高效零知识证明（ZKP）局限于 NP 语句，而 PSPACE 语句的 ZKP 虽存在但不实用。本文通过支持 QBF（量化布尔公式）求值的零知识证明，给出首个针对 PSPACE 完全语句的实用 ZK 协议。核心思想是以零知识方式验证量化消解证明（Q-Res）：为 Q-Res 证明构造高效的多项式编码，使证明验证转化为低开销的算术检查；并设计 ZK 协议证明与 QBF 相关的获胜策略的存在。在 QBFEVAL 上实现与评测，结果显示可验证其中 72% 的 QBF。


### Mechanized Safety and Liveness Proofs for the Mysticeti Consensus Protocol Under the LiDO-DAG Framework.
- **作者**: Longfei Qiu, Jingqi Xiao, Zhong Shao
- **解读**: 针对 Mysticeti 共识协议，在 LiDO-DAG 框架下给出机械化（machine-checked）的安全性与活性（safety and liveness）证明，以形式化手段验证该基于 DAG 的共识协议的正确性。（无摘要，据标题推断）


### Coral: Fast Succinct Non-Interactive Zero-Knowledge CFG Proofs.
- **作者**: Sebastian Angel, Sofía Celi, Elizabeth Margolin, Pratyush Mishra, Martin Sander, Jess Woods
- **解读**: 提出 Coral，一种快速、简洁、非交互的零知识控制流图（CFG）证明，可在不泄露敏感信息的前提下高效证明程序控制流性质，兼顾证明时间与验证效率。（无摘要，据标题推断）


### Dory: Streaming PCG with Small Memory.
- **作者**: Xiaojie Guo, Hanlin Liu, Zhicong Huang, Hongrui Cui, Wenhao Zhang, Cheng Hong, Xiao Wang, Kang Yang, Yu Yu
- **解读**: 提出 Dory，一种内存占用小的流式（streaming）伪相关生成器（PCG），在运行过程中仅需小内存即可生成伪随机相关，从而降低安全计算中相关生成的存储与通信开销。（无摘要，据标题推断）


### CAVERN: Efficient Honest-Majority Maliciously Secure (2+1)-PC for $\mathbb{Z}_{2^{n}}$ via DPF.
- **作者**: Yang Liu, Liang Feng Zhang
- **解读**: 提出 CAVERN，一种诚实多数、恶意安全的 (2+1)-PC（三方两服务器计算）协议，基于分布式点函数（DPF）针对 Z_{2^n} 环实现高效计算，兼顾安全性与性能。（无摘要，据标题推断）


### Sort, Sweep, Mirror: Batch Private Interval Lookup with Logarithmic Cost.
- **作者**: Andes Y. L. Kei, Lucien K. L. Ng, Jack P. K. Ma, Sherman S. M. Chow
- **解读**: 提出 Sort, Sweep, Mirror，一种对数成本的批量隐私区间查找（private interval lookup）协议，通过排序、扫描与镜像等操作在多个区间上实现高效、低通信开销的隐私查找。（无摘要，据标题推断）


### Vega: Low-Latency Zero-Knowledge Proofs over Existing Credentials.
- **作者**: Darya Kaviani, Srinath Setty
- **解读**: 提出 Vega，一种基于既有凭据（existing credentials）的低延迟零知识证明方案，允许用户针对已签发凭据快速生成证明，兼顾证明生成速度与部署易用性。（无摘要，据标题推断）


### The Pipes Model for Latency and Throughput Analysis.
- **作者**: Andrew Lewis-Pye, Kartik Nayak, Nibesh Shrestha
- **解读**: 针对分布式计算协议中延迟仅以通信轮数/网络延迟表示、未计入数据量与发送方依赖，且吞吐量只能经验测量的问题，提出 Pipes 模型用于分析状态机复制（SMR）协议。该模型刻画处理器带宽 S、事务到达率 D 与延迟 Δ 的影响，能显式指出协议瓶颈，并据此分析 Best-effort/Reliable Broadcast 等广播原语及先进的 SMR 协议（如 DispersedSim）。


### Generate-then-Verify: Reconstructing Data from Limited Published Statistics.
- **作者**: Terrance Liu, Eileen Xiao, Adam D. Smith, Pratiksha Thaker, Zhiwei Steven Wu
- **解读**: 研究从聚合统计量重建表格数据的问题，攻击者旨在找出可被聚合量以 100% 确定性验证的敏感声明。既有工作聚焦于统计量足够丰富、可完整重建数据集的情形；本文关注多种可能取值并存、无法完美重建数据集的现实情形。提出部分重建：目标是让对手输出一个保证正确的行/列子集，并引入先"生成"后"验证"的整数规划方法，逐项判断候选是否被聚合量确定性地支持。


### Setting the Course, but Forgetting to Steer: Analyzing Compliance with GDPR's Right of Access to Data by Instagram, TikTok, and Youtube.
- **作者**: Sai Keerthana Karnam, Abhisek Dash, Antariksh Das, Sepehr Mousavi, Stefan Bechtold, Krishna P. Gummadi, Animesh Mukherjee, Ingmar Weber, Savvas Zannettou
- **解读**: 针对 GDPR 数据访问权（Right of Access）的实际有效性，对 TikTok、Instagram 与 YouTube 三平台的 Data Download Packages（DDP）进行综合审计。发现三平台虽服务类似，但在实施访问权时存在显著不一致（共享数据种类各异），且普遍未披露处理目的、保留期限与其他第三方接收方，构成不合规的进一步证据；可靠性评估也暴露了相关问题。


### Practical Anonymous Two-Party Gradient Boosting Decision Tree.
- **作者**: Chenyu Huang, Fan Zhang, Minxin Du, Sherman S. M. Chow, Huangxun Chen, Huaming Rao, Danqing Huang, Bo Qian, Peng Chen
- **解读**: 针对纵向切分特征、相互不信任双方间的梯度提升决策树（GBDT）训练，指出依赖 PSI 会对齐记录并暴露数据集共享的标识符（ID），而 circuit-PSI 成本高昂。为隐藏 ID，本文发起匿名 GBDT 训练研究，采用双方各持一份的"Dual"设计，使参与方在训练过程中避免泄露交集标识信息。


### Consumer Beware! Exploring Data Brokers' CCPA Compliance.
- **作者**: Elina van Kempen, Isita Bagayatkar, Pavel Frolikov, Chloe Georgiou, Gene Tsudik
- **解读**: 针对数据经纪商在未经用户知情同意下收集并出售个人信息的问题，本文开展首个大规模、系统性的 CCPA 合规研究：对官方登记的全部 543 家经纪商逐一手动提交请求，并深入分析其回应（或拒不回应）。结果显示超过 40% 的经纪商完全未回应，明显违反 CCPA；而在回应的经纪商中，不少要求索取其此前并未收集的身份验证信息。


### Private Data Imputation.
- **作者**: Addelkarim Kati, Florian Kerschbaum, Marina Blanton
- **解读**: 针对缺失值/错误值填充（数据插补）是重要数据准备任务、但既有方法在隐私保护下失效的问题，提出首个优化的隐私插补协议，覆盖水平切分与垂直切分的数据集。优化目标是把大部分计算归结为集合交集（或可高效计算的 oblivious 可编程伪随机函数 OPRF）；在全部评测数据集上平均较本地插补有约 20%（部分 5%）的精度优势。


### It's a Feature, Not a Bug: Secure and Auditable State Rollback for Confidential Cloud Applications.
- **作者**: Quinn Burke, Anjo Vahldiek-Oberwagner, Michael Swift, Patrick D. McDaniel
- **解读**: 针对回放与回滚攻击通过不可信存储接口注入真实但过期的数据破坏云应用决策的问题，指出既有框架把一切回滚都视为恶意、从而排除了用于从损坏或误配置中恢复的合法回滚。提出 Rebound，一个通用框架：在保持状态前向连续性防护的同时，允许策略授权的二进制、配置与数据回滚；其核心是充当参考监视器、强制授权策略、保证更新与回滚的原子性并输出可防篡改日志的组件。


### Understanding Data Collection, Brokerage, and Spam in the Lead Marketing Ecosystem.
- **作者**: Yash Vekaria, Nurullah Demir, Konrad Kollnig, Zubair Shafiq
- **arXiv PDF**: [Understanding Data Collection, Brokerage, and Spam in the Lead Marketing Ecosystem.](https://arxiv.org/pdf/2604.06759)
- **解读**: 针对线索营销（lead marketing）生态系统中通过网页表单收集并销售保险等高价值垂直领域个人数据却缺乏研究的问题，构建端到端测量框架以追踪从数据收集到消费者被联系的数据流。对 100 多个健康相关线索生成网站进行插桩，监控 200 个受控电话号码与邮箱，观察高度敏感的健康信息被共享给多家下游营销方的情况。


### CBUE: Conclusion Based Utility Evaluation for Differentially Private Categorical Data.
- **作者**: Furkan Sarikaya, Shaohua Lu, Johes Bater, Mark Hempstead
- **解读**: 针对差分隐私分类数据发布后难以评估其下游结论价值的问题，提出 CBUE（基于结论的效用评估），以"能否得出与真实数据一致的结论"来衡量差分隐私数据的效用。（无摘要，据标题推断）


### Fast Deterministically Safe Proof-of-Work Consensus.
- **作者**: Ali Farahbakhsh, Giuliano Losa, Youer Pu, Lorenzo Alvisi
- **arXiv PDF**: [Fast Deterministically Safe Proof-of-Work Consensus.](https://arxiv.org/pdf/2512.19968)
- **解读**: 针对 PoW 易受拥有足够算力的攻击者破坏、PoS 易受低成本重写历史的长程攻击且依赖社交共识等外部机制的问题，提出 Sieve-MMR，首个不依赖外部机制、兼具确定性安全与常数期望延迟的完全无许可（permissionless）共识协议。


### Hadal: Centralized Label DP without a Trusted Party.
- **作者**: James Choncholas, Stanislav Peceny, Amit Agarwal, Mariana Raykova, Baiyu Li, Karn Seth
- **解读**: 针对集中式标签差分隐私（centralized label DP）通常需要可信第三方的问题，提出 Hadal，在不依赖可信方的情况下实现集中式标签 DP，通过多方协议在无信任假设下完成标签的差分隐私统计。（无摘要，据标题推断）


### Privacy-Conscious Algorithm Design Via PAC Privacy.
- **作者**: Mayuri Sridhar, Xiaochen Zhu, Srinivas Devadas
- **解读**: 提出基于 PAC 隐私的隐私敏感算法设计方法，将 PAC 学习框架引入隐私刻画，使算法设计能在隐私保证与效用之间建立可证明的权衡。（无摘要，据标题推断）


### Decomposition-Based Optimal Bounds for Privacy Amplification via Shuffling.
- **作者**: Pengcheng Su, Haibo Cheng, Ping Wang
- **解读**: 针对洗牌（shuffling）可放大差分隐私保证的问题，指出隐私毯（privacy blanket）与克隆（clone）两类分析框架共同的基础是把局部随机化器分解为结构化组件。提出统一框架——一般化克隆范式，囊括所有可能分解（毯式分解为其特例）；在该框架内识别出最优分解恰为毯式分解，并给出简化方法，从而得到经洗牌放大隐私的最优界。


### Making Privacy Public: Toward a Differential Privacy Deployment Registry.
- **作者**: Priyanka Nanayakkara, Elena Ghazi, Salil P. Vadhan
- **解读**: 面向差分隐私部署的透明度问题，提出建立差分隐私部署登记册（Differential Privacy Deployment Registry），使实际部署的 DP 系统公开可查、可审计，从而让隐私保证"公开化"。（无摘要，据标题推断）


### Auditing Apple's DifferentialPrivacy.framework: Implementation Bugs, Misconfigurations, and Practical Risks.
- **作者**: Rishav Chourasia, Ergute Bao, Uzair Javaid, Xiaokui Xiao
- **解读**: 针对 Apple 自 2016 年起声称其设备分析受差分隐私保护、但算法未开源导致难以独立验证的问题，在 macOS Sonoma 14.2 与 Sequoia 15.6 上开展客户端侧审计。逆向工程已发布二进制、恢复 Objective-C 接口、搭建运行时测试平台执行各机制，检验输出是否匹配广告宣称的保证；覆盖几乎所有活跃机制（Count Median Sketch、Hadamard-CMS、rando 等），并披露实现缺陷、误配置与实际风险。


### Sparse Estimation Under Local Differential Privacy at All Privacy Levels.
- **作者**: Puning Zhao, Qingqing Ye, Shaowei Wang, Jun Feng, Sheng Yue, Zhen Chen, Xiaochun Cao
- **解读**: 研究局部差分隐私（LDP）下稀疏估计在所有隐私水平（含高隐私预算区间）的算法，为各隐私参数区间给出统一的稀疏估计方法及相应误差界。（无摘要，据标题推断）


### Shared Spotlight Meridian: Distributed Sparse Pseudorandom Functions for Scalable Federated Learning.
- **作者**: Youlong Ding, Peihua Mai, Jingqi Zhang, Sherman S. M. Chow, Minxin Du, Yan Pang
- **解读**: 针对联邦学习中可扩展的隐私与安全性需求，提出分布式稀疏伪随机函数（PRF）方案 Shared Spotlight Meridian，以支持大规模联邦学习的分布式计算与隐私保护。（无摘要，据标题推断）


### Jigsaw: Doubly Private Smart Contracts.
- **作者**: Sanjam Garg, Aarushi Goel, Dimitris Kolonelos, Rohit Sinha
- **解读**: 提出 Jigsaw，双重私有（doubly private）智能合约，在保护合约状态隐私的同时兼顾输入与执行隐私，实现更全面的链上隐私保护。（无摘要，据标题推断）


### CHORUS: Secret Recovery with Ephemeral Client Committees.
- **作者**: Deevashwer Rathee, Emma Dauterman, Allison Li, Raluca Ada Popa
- **解读**: 提出 CHORUS，一种利用临时客户端委员会（ephemeral client committees）进行秘密恢复的方案，无需长期存在的固定服务器，以增强秘密共享/恢复的可用性与容错性。（无摘要，据标题推断）


### ARES: Scalable and Practical Gradient Inversion Attack in Federated Learning Through Activation Recovery.
- **作者**: Zirui Gong, Leo Yu Zhang, Yanjun Zhang, Viet Vo, Tianqing Zhu, Shirui Pan, Cong Wang
- **arXiv PDF**: [ARES: Scalable and Practical Gradient Inversion Attack in Federated Learning Through Activation Recovery.](https://arxiv.org/pdf/2603.17623)
- **解读**: 针对联邦学习中主动梯度反演攻击（active GIA）虽可在大批量下高保真重建样本、却常需修改模型架构而限制实用性，提出 ARES（基于稀疏反演的激活恢复攻击），一种不需架构修改即可从大批量训练样本中重建数据的主动 GIA，兼顾可扩展性与实用性。


### On the Detectability of Active Gradient Inversion Attacks in Federated Learning.
- **作者**: Vincenzo Carletti, Pasquale Foggia, Carlo Mazzocca, Giuseppe Parrella, Mario Vento
- **解读**: 针对联邦学习依赖"数据不出客户端"带来的虚假安全感，研究主动梯度反演攻击（GIA）的可检测性。指出被动或主动（恶意服务器操纵全局模型）的 GIA 均可重建本地数据、破坏隐私承诺，且此前归入某类的攻击已被证明可被客户端检测、限制其现实适用性；本文针对新近出现的声称更难被发现的攻击，评估其能否被客户端检测到。


### Toward Efficient Membership Inference Attacks Against Federated Large Language Models: A Projection Residual Approach.
- **作者**: Guilin Deng, Silong Chen, Yuchuan Luo, Yi Liu, Songlei Wang, Zhiping Cai, Lin Liu, Xiaohua Jia, Shaojing Fu
- **arXiv PDF**: [Toward Efficient Membership Inference Attacks Against Federated Large Language Models: A Projection Residual Approach.](https://arxiv.org/pdf/2604.21197)
- **解读**: 针对联邦大语言模型（FedLLM）中参数规模巨大、收敛快、梯度稀疏且非正交等特性使既有成员推断攻击（MIA）失效的问题，提出 ProjRes，首个面向 FedLLM 的基于投影残差的被动 MIA：以隐藏嵌入向量作为样本表示，分析其在梯度子空间上的投影残差，以揭示训练数据成员关系，实现高效推断。


### STIR/SHAKEN: A Cocktail of Cryptographic Clumsiness.
- **作者**: Joshua Brown, Paul Grubbs, Matthew Hardeman
- **解读**: 针对 STIR/SHAKEN（北美来电认证体系）的密码学实现问题，剖析其在密码学设计与部署上的种种笨拙（clumsiness）之处，揭示其安全性弱点与实现缺陷。（无摘要，据标题推断）


### A Liveness Attack to Ethereum PoS with No Additional Cost.
- **作者**: Mingfei Zhang, Rujia Li, Xueqian Lu, Sisi Duan
- **解读**: 针对以太坊权益证明（PoS）共识，提出一种零额外成本的活性（liveness）攻击，攻击者无需付出额外算力或资本即可破坏共识的活性性质。（无摘要，据标题推断）


### Sealing the Window: Efficient Tamper Protection for Provenance Logs.
- **作者**: Sagar Mishra, R. Sekar
- **解读**: 针对溯源日志（provenance logs）的防篡改保护，提出 Sealing the Window，一种高效的窗口密封机制，在不显著增加开销的前提下封闭篡改窗口，保证日志完整性可验证。（无摘要，据标题推断）


### Efficient Fuzzy Private Set Intersection from Secret-Shared OPRF.
- **作者**: Xinpeng Yang, Meng Hao, Chenkai Weng, Robert H. Deng, Yonggang Wen, Tianwei Zhang
- **arXiv PDF**: [Efficient Fuzzy Private Set Intersection from Secret-Shared OPRF.](https://arxiv.org/pdf/2604.14909)
- **解读**: 针对 L_p 距离度量（p∈[1,∞]）下的模糊隐私集合交集（FPSI），指出既有方案要么重度依赖昂贵的同态加密，要么复杂度随元素维度指数增长。提出高效 FPSI 协议，在 L_p 度量下以可接受的复杂度实现模糊交集计算，避免上述两类低效瓶颈。


### Weighted Batched Threshold Encryption With Applications to Mempool Privacy.
- **作者**: Amit Agarwal, Kushal Babel, Sourav Das, Babak Poorebrahim Gilkalaye, Arup Mondal, Benny Pinkas, Peter Rindal, Aayush Yadav
- **解读**: 面向内存池（mempool）隐私，提出加权批处理阈值加密（weighted batched threshold encryption）方案，使加密的批交易可由加权阈值集合联合解密，用于隐藏交易池中的交易内容与顺序信息。（无摘要，据标题推断）


### GoSSamer: Lightweight and Linear-Communication Asynchronous (Dynamic Proactive) Secret Sharing and the Applications.
- **作者**: Xinxin Xing, Yizhong Liu, Boyang Liao, Jianwei Liu, Bin Hu, Xun Lin, Yuan Lu, Tianwei Zhang
- **解读**: 提出 GoSSamer，一种轻量级、线性通信的异步（动态主动）秘密共享方案，并在异步网络下实现高效、可动态更新的秘密共享，进而应用于相应的安全协议。（无摘要，据标题推断）


### UltraProofs: Scalable Reed-Solomon Code Commitment.
- **作者**: Yanpei Guo, Alex Luoyuan Xiong, Wenjie Qu, Jiaheng Zhang
- **解读**: 提出 UltraProofs，一种可扩展的 Reed-Solomon 码承诺方案，支持对大规模 RS 编码数据进行高效承诺与打开验证，适用于多项式/数据可用性证明等场景。（无摘要，据标题推断）


### Scalable Registration-Based Encryption from Lattices.
- **作者**: Michael Klooß, Russell W. F. Lai, Jan Niklas Siemer, Monisha Swarnakar
- **解读**: 提出基于格（lattices）的可扩展注册制加密（Registration-Based Encryption, RBE）方案，在去除可信中央机构的同时实现大规模用户的注册与加密，且具备抗量子安全性。（无摘要，据标题推断）


### From Perfect to Approximate Hints: Efficient LWE Secret Recovery Leveraging Low Hamming Weight.
- **作者**: Minki Hhan, Ga Hee Hong, Jiseung Kim, Changmin Lee, JeongHwan Lee
- **解读**: 研究利用低汉明权重（low Hamming weight）密钥的结构特性，将完美提示推广到近似提示，实现高效的 LWE 秘密恢复，即在仅有近似信息时仍能以较低复杂度恢复 LWE 秘密。（无摘要，据标题推断）


### International Students and Scams: At Risk Abroad.
- **作者**: Katherine Zhang, Arjun Arunasalam, Pubali Datta, Z. Berkay Celik
- **解读**: 针对在美留学生因文化适应、远程租房、财务安排以及近期签证政策变化而面临更高网络诈骗风险的问题，开展两阶段用户研究：问卷调查（n=48）与半结构化访谈（n=9），调查其接触、互动与事后经历，揭示这一群体独特的受骗风险与暴露面。


### Lost in Translation: Text Message Spoofing via Email.
- **作者**: Sumanth Rao, Ye Shu, Stefan Savage, Aaron Schulman, Geoffrey M. Voelker, Enze Liu
- **解读**: 研究通过电子邮件实现短信（SMS）伪造的技术途径，揭示运营商短信网关在处理邮件转短信时的校验缺失，使攻击者可冒充他人发送短信。（无摘要，据标题推断）


### From "Be Careful" to "Here's Why": Investigating User Reasoning with Context-Specific SMS Scam Warnings.
- **作者**: Elijah Robert Bouma-Sims, Enze Liu, Alexandra Xinran Li, Lorrie Faith Cranor
- **解读**: 研究针对 SMS 诈骗警告的用户推理，比较泛泛的"小心"式警告与带具体解释的上下文相关警告，考察解释性信息如何影响用户的判断与决策。（无摘要，据标题推断）


### Towards Automating Data Access Permissions in AI Agents.
- **作者**: Yuhao Wu, Ke Yang, Franziska Roesner, Tadayoshi Kohno, Ning Zhang, Umar Iqbal
- **解读**: 针对 AI 智能体以用户名义自主行动引发的透明度与控制问题，论证基于权限的访问控制对保障用户掌控不可或缺，但传统权限模型不适配自动化智能体执行范式。通过用户研究识别影响决策的因素并编码为基于机器学习的助手以预测未来决策；发现参与者的决策受交流上下文影响，且个体偏好在同一上下文内保持一致、并与其他参与者一致。据此构建预测模型，总体准确率达 85.1%，高置信预测达 94.4%。

### COSSETER: GitHub Actions Permission Reduction Using Demand-Driven Static Analysis.
- **作者**: Greg Tystahl, Jonah Ghebremichael, Siddharth Muralee, Sourag Cherupattamoolayil, Antonio Bianchi, Aravind Machiry, Alexandros Kapravelos, William Enck
- **解读**: 针对 GitHub Actions 工作流中的过度授权问题，提出基于需求驱动静态分析的工具 COSSETER，分析工作流实际调用的 API 与资源以计算最小必要权限集合。目标是自动生成精简的权限配置，降低 CI/供应链环境被攻陷时的风险面。（无摘要，据标题推断）


### EyeSpy: Inferring Eye Gaze via Side-Channel Attacks Against Foveated Rendering.
- **作者**: Paul Maynard, Harris Amjad, Camila Molinares, Bo Ji, Brendan David-John
- **解读**: VR 中眼动数据在内部驱动动态注视点渲染（DFR）时，注视区域物体 GPU 负载更高，形成可被利用的侧信道。EyeSpy 通过在用户视野中扫过不可感知的高成本物体（HCO）并记录帧率等性能指标，依据性能波动与 HCO 位置的关联重建注视坐标，无需访问眼动 API。在 Meta Quest Pro、Varjo XR-4 及桌面平台上平均预测误差 1.1–4.4 度，接近典型眼动仪精度，且验证了跨硬件、引擎与渲染管线的泛化性；并提出了基于监督/无监督检测器的防御，短时间窗内 F1 达 0.99。


### SaTor: Exploring Satellite Routing in Tor to Reduce Latency.
- **作者**: Haozhi Li, Tariq Elahi
- **解读**: Tor 网络高延迟的一大成因是跨地理远距离区域的冗长电路带来的传输延迟，常见对策是修改电路构建过程以减少此类电路，但会损害路由随机性、增大去匿名化风险。SaTor 为中继配备卫星接入，通过卫星传输加速慢链路，且不改变现有路径选择过程。基于自研模拟器与实际测量的评估显示，长期来看仅需 100 个部署卫星服务的顶级中继即可为 40% 的电路带来约 21.8ms 的期望加速。


### A Context Is Worth a Thousand Lies: Evading Intrusion Detectors via Intelligent Context Distortion.
- **作者**: Magdy Nasr, Vansh Rastogi, Azadeh TabibanB
- **解读**: 研究入侵检测系统的逃逸问题，提出"智能上下文扭曲"攻击思路：通过刻意篡改攻击相关上下文信息，误导依赖上下文的入侵检测模型做出误判。目标是揭示检测器对上下文信号的过度依赖弱点，推动更鲁棒的检测设计。（无摘要，据标题推断）


### MadeYouReset: Exploiting HTTP/2 Server-Side Resets for Large-Scale DoS.
- **作者**: Gal Bar Nahum, Anat Bremler Barr, Yaniv Harel
- **解读**: 研究利用 HTTP/2 服务端重置（RST_STREAM）机制发动大规模拒绝服务攻击的方法，分析攻击者如何诱导服务器大量发送重置帧以放大资源消耗。揭示 HTTP/2 协议交互中可被滥用的 DoS 攻击面及其放大效应。（无摘要，据标题推断）


### Guardians of the Air: In-Device Detection of 5G Control-Plane Threats.
- **作者**: Tianwei Wu, Abdullah Al Ishtiaq, Tianchang Yang, Yilu Dong, Kai Tu, Zeyu Song, Ridwanul Hasan Tanvir, Md. Toufikuzzaman, Shagufta Mehnaz, Syed Rafiul Hussain
- **解读**: 提出设备端检测 5G 控制面威胁的方案，在用户设备上监测 NAS/RRC 等控制面信令与状态异常，识别伪基站、信令滥用等攻击。强调不依赖运营商侧基础设施、直接在终端内完成威胁发现与告警。（无摘要，据标题推断）


### The Threat Landscape of IP Leasing in the RPKI Era.
- **作者**: Weitong Li, Yongzhe Xu, Taejoong Chung
- **解读**: 系统性调查 RPKI 时代 IP 地址租赁市场的安全威胁，分析租用前缀在路由安全机制（ROA/ROV）下的授权、滥用与劫持风险。刻画 IP 租赁生态与 RPKI 信任模型之间的缺口及其对路由安全的影响。（无摘要，据标题推断）


### Batch Me If You Can: Coverage-Guided RPKI Fuzzing at Scale.
- **作者**: Haya Schulmann, Niklas Vogel
- **arXiv PDF**: [Batch Me If You Can: Coverage-Guided RPKI Fuzzing at Scale.](https://arxiv.org/pdf/2605.26651)
- **解读**: RPKI 软件因输入为成百上千个相互依赖、密码学链接的对象，现有单输入模糊器（AFL++/libFuzzer）无法做精确覆盖率归属而难以发现深层漏洞。作者提出连续采样、以函数为侧信道实现多对象仓库的逐对象覆盖率归属、以及将输入解析为带标签树以在保持密码学有效性的前提下做结构与语义变异等技术，并实现工具 CAT。CAT 相比顺序模糊吞吐提升 66 倍，比 libFuzzer 等探索多 24–47% 的代码路径，在 RPKI 验证器上发现 21 个此前未知的漏洞（8 个已分配 CVE，CVSS 7.5–9.8），包括缓冲区溢出、拒绝服务及可利用的仓库投毒逻辑缺陷。


### The Fault in Our Drafts: Vulnerabilities in RPKI Specification and Software.
- **作者**: Oliver Jacobsen, Tobias Kirsch, Haya Schulmann, Niklas Vogel, Michael Waidner
- **解读**: 研究 RPKI 规范与实现软件中的安全漏洞，分析规范草案表述模糊或实现偏差如何导致验证绕过、仓库投毒等风险。通过对规范与实际软件实现的一致性分析揭示系统性缺陷。（无摘要，据标题推断）


### Camveil: Unveiling Security Camera Vulnerabilities Through Multi-Protocol Coordinated Fuzzing.
- **作者**: Fuchen Ma, Yuqiao Yang, Yuanliang Chen, Yanyang Zhao, Ting Chen, Yu Jiang
- **解读**: 针对安防摄像头提出多协议协同模糊测试框架 Camveil，统一协调 RTSP、ONVIF、HTTP 等多种协议下的输入生成，以发现跨协议交互才触发的漏洞。目标是系统化揭示安防摄像头设备的安全缺陷。（无摘要，据标题推断）


### Designing Transport-Level Encryption for Datacenter Networks.
- **作者**: Tianyi Gao, Xinshu Ma, Suhas Narreddy, Eugenio Luo, Steven W. D. Chien, Michio Honda
- **解读**: 面向 NDP、Homa 等新兴数据中心传输协议提出 SDP 加密协议设计，将数据加密原生集成进传输层：支持面向 TLS-over-TCP 的网卡卸载，原生支持 TCP/UDP 之外的新传输，并提供基于消息的抽象以支持细粒度并行、低延迟 RPC。


### Fizzle: A Framework for Deterministic and Reproducible Network Fuzzing.
- **作者**: Nathaniel Bennett, Tyler Tucker, Carson Stillman, William Enck, Patrick Traynor, Kevin R. B. Butler
- **解读**: 提出 Fizzle 框架，为网络协议模糊测试提供确定性与可复现性保障，通过可控的网络事件调度消除时序与网络不确定性。使网络模糊测试结果可稳定复现，便于漏洞验证与回归分析。（无摘要，据标题推断）


### The Battle of Metasurfaces: Understanding Security in Smart Radio Environments.
- **作者**: Paul Staat, Christof Paar, Swarun Kumar
- **解读**: 超表面/RIS 可将被动无线环境变为可编程媒质，攻击者与防御者都能利用其改变电磁波传播，而以往研究多为单边应用。本文首次系统研究攻防双方能力对等的对称场景，用理论建模与真实实验分析竞争双方在信号功率、感知等目标上的互动。结果显示"对抗"结果取决于时序、部署位置、算法策略与硬件规模，Wi-Fi 环境下双方效果可被大幅相互抵消，既威胁已有隐私方案，也为设计鲁棒的高保障物理层系统提供启示。


### SatBleed: Security of Commoditized Communication Modules in Satellites.
- **作者**: Ulysse Planta, Julian Rederlechner, Martin Strohmeier, Mathias Fischer, Ali Abbasi
- **解读**: 调查卫星中商用现货（COTS）通信模块的安全性，系统分析真实卫星通信模块固件与协议实现中的漏洞（如内存破坏、协议缺陷）。揭示太空资产面临的现实攻击面与供应链风险。（无摘要，据标题推断）


### RIS-CLA: Reviving CSI-Based Continuous Location Authentication With Reconfigurable Intelligent Surfaces.
- **作者**: Yan Zhang, Jiawei Li, Yizhou Wang, Dianqi Han, Yanchao Zhang, Aditya Shekhawat, Georgios Trichopoulos
- **解读**: 利用可重构智能表面（RIS）改善信道状态信息（CSI）质量，重新激活基于 CSI 的持续位置认证（CLA）方案。目标是克服室内多径等环境下 CSI 认证精度不足的问题，实现免携带设备、持续进行的身份验证。（无摘要，据标题推断）


### MUSICSHIELD: Protection for Musicians in the Era of Generative AI.
- **作者**: Syed Irfan Ali Meerza, Jian Liu
- **解读**: 面向生成式 AI 时代音乐人的版权与身份保护需求提出 MUSICSHIELD，为音乐作品提供防止被 AI 模型未经授权学习、模仿或合成的防护机制。目标是保护音乐人的创作权益免受深度合成与风格克隆的侵害。（无摘要，据标题推断）


### MAYA: Addressing Inconsistencies in Generative Password Guessing Through a Unified Benchmark.
- **作者**: William Corrias, Fabio De Gaspari, Dorjan Hitaj, Luigi V. Mancini
- **解读**: 生成式密码猜测研究因评估方法不一致而难以进行有意义的比较。本文提出 MAYA，一个统一、可定制、即插即用的基准框架，在八个真实数据集、覆盖完整高级测试场景（累计超过 15,000 计算小时）上标准化评估六个先进方法。结果显示这些模型各自捕获人类密码分布的不同侧面且泛化良好，但在长而复杂的密码上效果差异显著，序列模型一致优于其他架构与传统工具；多模型联合攻击优于单个最优模型。框架已开源。


### Credential Extraction Attacks Against Compromised Credential Checking Services of Password Managers.
- **作者**: Yihe Duan, Ding Wang, Yutong Li
- **解读**: 研究针对密码管理器"凭据泄露检查"服务的凭据提取攻击，分析当检查服务本身被攻陷或不可信时，用户的密码与口令如何被恢复。揭示该服务设计中的信息泄露面并评估现有防护措施的不足。（无摘要，据标题推断）


### Can Foundation LLMs Accurately Estimate Password Strength and Provide Appropriate Password Feedback?
- **作者**: Madison Pickering, Garrison Hinson-Hasty, Luca Dovichi, Helena Williams, Nathaniel Kim, Aybala Esmer, Blase Ur
- **解读**: 评测基础大语言模型（LLM）评估密码强度与提供密码反馈建议的能力，与现有密码强度评估器进行一致性比较。旨在检验 LLM 能否作为面向普通用户的密码强度评估与反馈工具。（无摘要，据标题推断）


### MoPE: A Mixture of Password Experts for Improving Password Guessing.
- **作者**: Mingjian Duan, Ming Xu, Shenghao Zhang, Weili Han
- **解读**: 现有数据驱动的密码猜测模型将密码一视同仁，忽略结构差异，导致训练偏向高频模式。基于结构相似的密码（如定长数字串）在隐空间中聚集于高密度区域的观察，MoPE 提出"密码专家混合"框架：以结构化的方法生成多个专家模型，并用轻量门控选择可靠的输出。在离线与在线场景中分别比 SOTA 基线提升最多 38.80% 与 9.27% 的破解率，还实现了毫秒级响应的实时密码强度计（PSM）。


### zkFuzz: Foundation and Framework for Effective Fuzzing of Zero-Knowledge Circuits.
- **作者**: Hideaki Takahashi, Jihwan Kim, Suman Jana, Junfeng Yang
- **解读**: ZK 电路程序中约束不足会接受无效 witness、约束过紧会拒绝有效 witness，静态分析误报高、形式化工具难以规模化。本文提出语言无关的 Trace-Constraint Consistency Test（TCCT）统一刻画此类缺陷，并实现变异模糊框架 zkFuzz，以适配函数引导变异计算逻辑、用定制启发式注入精心构造的输入。在 452 个 Circom 程序上发现 85 个 bug（其中 59 个零日、39 个获开发者确认），并初步验证了可扩展至 Noir 等其他 DSL。


### Single-Server Private Outsourcing of zk-SNARKs.
- **作者**: Kasra Abbaszadeh, Hossein Hafezi, Jonathan Katz, Sarah Meiklejohn
- **解读**: 研究单服务器场景下 zk-SNARK 证明生成的隐私外包：客户端将证明计算委托给服务器，同时不泄露 witness 等私密输入。目标是降低证明者的计算开销，同时保证输入隐私与证明的完整性。（无摘要，据标题推断）


### Language-Agnostic Detection of Computation-Constraint Inconsistencies in ZKP Programs Via Value Inference.
- **作者**: Arman Kolozyan, Bram Vandenbogaerde, Janwillem Swalens, Lode Hoste, Stefanos Chaliasos, Coen De Roover
- **解读**: 提出语言无关的 ZKP 程序"计算-约束不一致"检测方法，通过值推断（value inference）找出程序计算逻辑与电路约束声明之间的偏差。帮助发现过约束/欠约束类缺陷，避免无效 witness 被接受或有效计算被拒绝。（无摘要，据标题推断）


### APEX: Accurate Parallel Expressive Homomorphic Execution for Encrypted Databases.
- **作者**: Wei Chen, Qi Hu, Siu-Ming Yiu, Heming Cui
- **解读**: 提出 APEX，为加密数据库提供准确、并行且表达能力强的同态执行引擎，支持在密文上执行丰富的查询操作。目标是在可接受的开销内实现加密数据库的高效、高精度查询处理。（无摘要，据标题推断）


### Secure Lookup Tables: Faster, Leaner, and More General.
- **作者**: Chongrong Li, Pengfei Zhu, Yun Li, Zhanpeng Guo, Jingyu Li, Yuncong Hu, Zhicong Huang, Cheng Hong
- **解读**: 改进安全查找表（secure lookup table）密码协议，使其更快、通信与存储开销更省、适用场景更广。服务于隐私计算中查找表类运算的高效安全执行。（无摘要，据标题推断）


### Efficient Arithmetic-and-Comparison Homomorphic Encryption with Space Switching.
- **作者**: Erwin Eko Wahyudi, Yan Solihin, Qian Lou
- **arXiv PDF**: [Efficient Arithmetic-and-Comparison Homomorphic Encryption with Space Switching.](https://arxiv.org/pdf/2604.19890)
- **解读**: FHE 方案难以在同一框架内高效同时支持算术与比较运算：方案切换对大输入开销过高，多项式近似在关键点附近引入误差。本文提出空间切换方法，在 FV 风格方案内识别两类运算需要不同的明文空间，通过归约步骤在数空间 Z_{p^r} 与数字空间 Z_p 之间切换、再用模提升步骤映射回原空间，实现算术与比较的连续求值。实验显示相比方案切换快最多 17 倍、相比直接比较快 15 倍，代码已开源。


### Code-Based Scalable Collaborative SNARKs.
- **作者**: Christodoulos Pappas, Dimitrios Papadopoulos, Charalampos Papamanthou
- **解读**: 提出基于纠错码的可扩展协作式 SNARK（collaborative SNARK），允许多个证明者协同生成证明，分担单方计算负担。目标是在大规模分布式计算场景中实现高效的零知识可验证计算。（无摘要，据标题推断）


### Optimistic Asynchronous Dynamic-Committee Proactive Secret Sharing.
- **作者**: Bin Hu, Jianwei Liu, Zhenliang Lu, Qiang Tang, Zhuolun Xiang, Zongyang Zhang
- **解读**: 提出乐观式（optimistic）异步动态委员会主动秘密共享方案，在异步网络下支持委员会成员动态变化，并通过定期刷新份额对抗移动敌手。通过乐观路径优化运行效率，同时保持安全性保证。（无摘要，据标题推断）


### Nebula: Proving Machine Executions via Folding Schemes.
- **作者**: Arasu Arun, Srinath T. V. Setty
- **解读**: 基于折叠方案（folding schemes）设计 Nebula，用于证明机器/程序执行轨迹的正确性。将长执行过程逐步折叠进简洁证明，在保持可验证性的同时降低证明与验证开销。（无摘要，据标题推断）


### Consistent Estimation of Numerical Distributions Under Local Differential Privacy by Wavelet Expansion.
- **作者**: Puning Zhao, Zhikun Zhang, Bo Sun, Li Shen, Liang Zhang, Shaowei Wang, Zhe Liu
- **arXiv PDF**: [Consistent Estimation of Numerical Distributions Under Local Differential Privacy by Wavelet Expansion.](https://arxiv.org/pdf/2509.19661)
- **解读**: 本地差分隐私（LDP）下的数值型分布估计是基础难题，类别数据的估计方法迁移到数值数据上效果不佳，且易把概率质量放错位置。本文提出用小波展开表示样本分布并在 LDP 下估计展开系数，优先估计低阶系数以保证宏观精度、防止概率质量远离真值。方法具备理论保证，实验显示在 Wasserstein 与 KS 距离下显著优于现有方案。


### Breaking the Barrier for Asynchronous MPC with a Friend.
- **作者**: Banashri Karmakar, Aniket Kate, Shravani Patil, Arpita Patra, Sikhar Patranabis, Protik Paul, Divya Ravi
- **解读**: 研究异步安全多方计算（MPC）的效率瓶颈，引入"朋友"（a friend）这一辅助角色帮助突破异步 MPC 的性能与轮次障碍。目标是让异步设置下的 MPC 达到更接近同步协议的性能表现。（无摘要，据标题推断）


### Best of Both Worlds: Effective Foreign Bridge Identification in V8 Embedders for Security Analysis.
- **作者**: Georgios Alexopoulos, Thodoris Sotiropoulos, Zhendong Su, Dimitris Mitropoulos
- **解读**: 研究 V8 嵌入式环境（如 Node.js、浏览器）中 JavaScript 与原生 C++ 之间的 foreign bridge 识别，用于安全分析。结合静态与动态方法的优势有效定位桥接点，为跨语言漏洞分析与污点追踪奠定基础。（无摘要，据标题推断）


### Decor: Delegated Computation on Randomness for Secure Evaluation of Nonlinear Functions.
- **作者**: Haris Smajlovic, Kyle Sheng, Timos Antonopoulos, Ruzica Piskac, Hyunghoon Cho
- **解读**: 提出 Decor，通过把计算委托到随机数上实现非线性函数的安全求值。利用预生成的随机性减少交互轮次与在线计算开销，适用于安全多方计算与隐私保护机器学习中的非线性操作。（无摘要，据标题推断）


### Bridge: High-Order Taint Vulnerabilities Detection in Linux-Based IoT Firmware.
- **作者**: Jiaqian Peng, Puzhuo Liu, Yicheng Zeng, Kai Cheng, Yongji Liu, Yun Yang, Hongsong Zhu
- **解读**: 提出 Bridge，检测 Linux 系 IoT 固件中的高阶污点漏洞（涉及多污点源/汇组合或经多跳传播的漏洞）。针对 IoT 固件的规模与异构性优化分析流程，提升真实固件环境的漏洞发现能力。（无摘要，据标题推断）


### SFA-Miner: Mining Path-Sensitive API Usage Patterns Via Symbolic Finite Automata.
- **作者**: Jiasheng Jiang, Mingwei Zheng, Qingkai Shi, Xiangyu Zhang
- **解读**: 用符号有限自动机（SFA）挖掘路径敏感的 API 使用模式，以编码不同控制流分支下的调用序列约束。挖掘出的模式可服务于 API 误用检测、程序理解与缺陷发现。（无摘要，据标题推断）


### ENCHTABLE: Unified Safety Alignment Transfer in Fine-Tuned Large Language Models.
- **作者**: Jialin Wu, Kecen Li, Zhicong Huang, Xinfeng Li, XiaoFeng Wang, Cheng Hong
- **解读**: 领域微调常导致大语言模型（LLM）安全对齐的系统性退化。EnchTable 利用基于神经正切核（NTK）的向量蒸馏方法解耦安全约束与任务特定推理，配合干扰感知的合并技术平衡多个任务域的效用，无需大规模重训即可向微调后的下游模型迁移并维持对齐。在三种不同架构的模型与 11 个数据集上验证，对静态与动态越狱攻击具有稳健抵抗，优于厂商发布的缓解方案，可无缝集成进部署管线。


### QuickSafe: Targeted Hardening Against Memory Corruption.
- **作者**: Johannes Blaser, Floris Gorter, Klaus von Gleissenthall, Herbert Bos
- **解读**: 针对已知但未修复的内存破坏漏洞，QuickSafe 提供低开销的定向加固：不插入运行时约束检查，而是将漏洞相关对象与程序其余部分隔离——传统架构上用带守卫页的专用页分配实现，支持 MTE 的平台上则用不相交标签域提供更强保证。配套 TagASan 扩展 AddressSanitizer，利用带标签指针把故障访问追溯回其来源分配点。在 223 个真实漏洞的数据集上，守卫页后端防护了全部被测 bug、几何平均开销仅 2.46–2.67%，MTE 后端开销约 0.12%。


### SpecAuditor: Generating Audit Specifications for LLM-Driven Bug Detection.
- **作者**: Miaoqian Lin, Hao Chen
- **解读**: 自动生成审计规范（audit specification）供 LLM 驱动的 bug 检测使用，为 LLM 提供精确的程序预期行为描述。目标是提升 LLM 静态分析的准确率与可解释性，降低误报。（无摘要，据标题推断）


### BACHunter: Detecting Broken Access Control Vulnerabilities in Intelligent Connected Vehicles.
- **作者**: Yanbang Sun, Xiaohong Li, Quanzhou Wang, Hebo Leng, Guangzheng Yao, Zhihua Xie, Qiang Hu, Junjie Wang
- **解读**: 提出 BACHunter 检测智能网联汽车中的失效访问控制（BAC）漏洞，分析车端、云端及车云交互中的授权与权限校验逻辑。目标是发现攻击者可越权访问的接口、功能与数据。（无摘要，据标题推断）


### Oxidizer: Toward Concise and High-fidelity Rust Decompilation.
- **作者**: Yibo Liu, Zion Leonahenahe Basque, Arvind S. Raj, Chavin Udomwongsa, Chang Zhu, Jie Hu, Changyu Zhao, Fangzhou Dong, Adam Doupé, Tiffany Bao, Yan Shoshitaishvili, Ruoyu Wang
- **解读**: Rust 程序反编译质量差，所有权、枚举、trait 等 Rust 特有语义在传统反编译中大量丢失。Oxidizer 致力于生成简洁且高保真的 Rust 反编译结果，恢复 Rust 语义以支撑后续安全分析。（无摘要，据标题推断）


### NanoTag: Systems Support for Efficient Byte-Granular Overflow Detection on ARM MTE.
- **作者**: Mingkai Li, Hang Ye, Joseph Devietti, Suman Jana, Tanvir Ahmed Khan
- **arXiv PDF**: [NanoTag: Systems Support for Efficient Byte-Granular Overflow Detection on ARM MTE.](https://arxiv.org/pdf/2509.22027)
- **解读**: 通过对首款生产级 ARM MTE 实现（Google Pixel 8）的深入调研，发现 MTE 因 16 字节标签粒度只能实现比 ASAN 粗的检测精度。NANOTAG 在不修改二进制的前提下以字节粒度概率性检测缓冲区溢出：为可能发生粒内溢出的标签粒设置"绊线"，访问绊线时触发软件侧的附加溢出检测，其余访问仍由 MTE 硬件检测。基于 Scudo Hardened Allocator 实现，检测能力接近 ASAN，运行时开销与 Scudo+MTE SYNC 模式相当。


### Parasol Compiler: Pushing the Boundaries of FHE Program Efficiency.
- **作者**: Rick Weber, Ryan Orendorff, Ghada Almashaqbeh, Ravital Solomon
- **解读**: 提出 Parasol 编译器，通过高级程序变换与优化（如参数选择、运算重排）提升全同态加密（FHE）程序的运行效率。目标是把高层程序高效编译为低开销的 FHE 计算，推动 FHE 的实用化。（无摘要，据标题推断）


### The First Large-Scale Systematic Study of Python Class Pollution Vulnerability.
- **作者**: Zhengyu Liu, Jiacheng Zhong, Jianjia Yu, Muxi Lyu, Zifeng Kang, Yinzhi Cao
- **解读**: 首次大规模系统研究 Python 类污染漏洞（类似原型污染，通过污染类属性影响全局对象行为），对真实 PyPI 生态进行测量与分析。刻画该类漏洞的成因、影响面与可利用性，并提出缓解建议。（无摘要，据标题推断）


### Cottontail: Large Language Model-Driven Concolic Execution for Highly Structured Test Input Generation.
- **作者**: Haoxin Tu, Seongmin Lee, Yuxian Li, Peng Chen, Lingxiao Jiang, Marcel Böhme
- **解读**: 现有 concolic 执行引擎生成高度结构化测试输入受限：路径约束选择与输入结构无关、约束求解能力有限导致大量语法非法输入、种子输入依赖人工获取。Cottontail 构建"可表达结构覆盖树"（ESCT）以选择结构感知的约束，用基于 Solve-Complete 范式的 LLM 求解器生成既可满足又语法合法的输入，并以历史引导实现连续测试。基于 SymCC 实现，在 8 个开源库、4 种格式（XML、SQL、JavaScript、JSON）上平均行/分支覆盖率比基线高 30.73%/41.32%，并发现 6 个未知漏洞（6 个 CVE）。


### Catch Me If You Can: Detector-Resistant Evasion via Semantics-Preserving Command Re-Realization.
- **作者**: Muhammad Shoaib, Hare Sudhan Muthusamy, Tareq Alkhatib, Wajih Ul Hassan
- **解读**: 提出保持语义的命令"再实现"（re-realization）逃逸方法：在不改变命令语义的前提下改变其实现形式与表达方式，以绕过命令注入类检测器。目标是评估检测器对命令语义等价变换的鲁棒性。（无摘要，据标题推断）


### INSIGHT: Automatic Generation of Explanations for Efficient Identification of Hardware Bugs and Underspecifications.
- **作者**: Vincent Quentin Ulitzsch, Alessandro Bertani, Peter W. Deutsch, David Langus Rodriguez, Kelly Xu, Aarti Gupta, Sharad Malik, Mengjia Yan
- **解读**: 自动生成解释以帮助高效识别硬件 bug 与规范欠定义（underspecification）问题。将形式化验证中的反例与异常转化为人类可读的解释，加速硬件设计调试与规格完善。（无摘要，据标题推断）


### Interplay of Efficient Model Checking and Secure Processor Design: A Case Study on Secure Speculation.
- **作者**: Tingzhen Dong, Qinhan Tan, Kunpeng Wang, Thomas Bourgeat, Yuheng Yang, Sharad Malik, Yu-Wei Fan, Mengjia Yan
- **解读**: 以安全推测（secure speculation）为案例，研究高效模型检测与安全处理器设计之间的相互作用。通过模型检测技术验证并指导安全推测机制的微架构设计，探索形式化方法与处理器安全设计协同的新路径。（无摘要，据标题推断）


### CiRCLE: Recovering Complex Data Structures in Binaries Beyond Fragmentation.
- **作者**: Zeyu Gao, Junlin Zhou, Songtao Yang, Chao Zhang
- **解读**: 二进制程序中的复杂数据结构常因优化与碎片化而难以恢复。CiRCLE 超越碎片化恢复方法，重构链表、树、哈希表等复杂数据结构的类型与布局信息，提升逆向工程与漏洞分析能力。（无摘要，据标题推断）


### No Password, No Problem? A Large-Scale Field Study of Passkey Adoption and Usage.
- **作者**: Tobias Reittinger, Günther Pernul
- **解读**: 大规模实地研究 passkey（无密码认证）的采用与使用行为，测量真实用户群中 passkey 的注册、认证与留存情况。评估无密码认证在实践中的可用性与普及障碍。（无摘要，据标题推断）


### Usable Anonymity in Reproductive Health Privacy.
- **作者**: Qiurong Song, Yanlai Wu, Rie Helene Hernandez, Yao Li, Yubo Kou, Xinning Gui
- **解读**: 从用户角度研究生殖健康隐私场景下的匿名可用性，分析用户在寻求生殖健康相关信息与服务时的隐私顾虑、匿名需求与行为策略。为设计可用的匿名保护机制与隐私政策提供实证依据。（无摘要，据标题推断）


### The Passkey Promise: A Comparative Usability Study of MFA Methods.
- **作者**: Erwin Kupris, Thomas Schreck
- **解读**: 对比研究 passkey 与其他多因素认证（MFA）方法的可用性，通过用户实验比较注册、认证流程的易用性、效率与满意度。检验 passkey 是否兑现了其更便捷安全的承诺。（无摘要，据标题推断）


### 2FiA: Towards WiFi Sensing-Based Authentication with Unique Biometrics.
- **作者**: Bofan Li, Zhankai Ye, Weikuan Yu, Yongning Tang, Liu Xiu
- **解读**: 基于 WiFi 感知（CSI 等）提取个体独有的生物特征用于身份认证，利用人体运动对无线信号传播的影响实现无感认证。目标是提供免携带设备、难以复制与伪造的持续生物特征认证手段。（无摘要，据标题推断）


### SmuFuzz: Enable Deep System Management Mode Fuzzing in Fully Featured UEFI Runtime Environment.
- **作者**: Jianqiang Wang, Yi Xiang, Meng Wang, Qinying Wang, Ali Abbasi, Thorsten Holz
- **解读**: 在完整 UEFI 运行时环境中对系统管理模式（SMM）进行深度模糊测试，覆盖 SMM 代码路径以发现固件级漏洞。解决 SMM 环境难以插桩、输入难以控制等挑战，提升固件安全测试能力。（无摘要，据标题推断）


### Jazzer: Coverage-Guided Fuzzing for Semantic Vulnerabilities in the Java Ecosystem.
- **作者**: Sergej Dechand, Tobias Wienand, Fabian Meumertzheim, Peter Samarin, Simon Resch, Khaled Yakdan, Thorsten Holz, Flavio Toffalini
- **解读**: Jazzer 是 Java 生态的覆盖率引导模糊器，专门针对语义漏洞（如 SQL 注入、反序列化、XXE 等）设计自动探测机制。通过语义 hooks 检测安全敏感 API 的滥用模式，推动 Java 应用漏洞的规模化发现。（无摘要，据标题推断）


### Beyond Nodes vs. Edges: A Multi-View Fusion Framework for Provenance-Based Intrusion Detection.
- **作者**: Fan Yang, Binyan Xu, Di Tang, Kehuan Zhang
- **arXiv PDF**: [Beyond Nodes vs. Edges: A Multi-View Fusion Framework for Provenance-Based Intrusion Detection.](https://arxiv.org/pdf/2604.14685)
- **解读**: 溯源图入侵检测存在粒度偏差：节点中心检测器易把良性行为变化误判为可疑，边中心检测器缺乏实体上下文导致漏报。PROVFUSION 从属性、结构、因果三个视图融合异常信号，通过轻量融合方案与投票式决策聚合给出更一致、更具上下文感知的判定。在 9 个常用基准数据集上比单一节点/边中心基线取得更高的检测精度与更低的误报率，且跨场景性能稳定。


### Specializing Language Models for Textual Fuzzing via Reinforcement Learning.
- **作者**: Jiayi Lin, Liangcai Su, Junzhe Li, Chenxiong Qian
- **arXiv PDF**: [Specializing Language Models for Textual Fuzzing via Reinforcement Learning.](https://arxiv.org/pdf/2509.20384)
- **解读**: 编译器、解释器、数据库引擎等目标接受的文本输入需满足复杂语法与语义约束，LM 模糊测试面临深层程序逻辑探索不足与使用大模型成本高的问题。R1-Fuzz 首次用强化学习专门化成本高效的 LM：提出基于覆盖切片的题目构造与基于距离的奖励计算，RL 后训练的小模型 R1-Fuzz-7B 在真实目标上媲美甚至超越更大的模型，覆盖率比 SOTA 模糊器高最多 75%，并发现 29 个此前未知的漏洞。


### PILOT: Command-Line Interface Fuzzing Via Path-Guided, Iterative Large Language Model Prompting.
- **作者**: Momoko Shiraishi, Yinzhi Cao, Takahiro Shinagawa
- **arXiv PDF**: [PILOT: Command-Line Interface Fuzzing Via Path-Guided, Iterative Large Language Model Prompting.](https://arxiv.org/pdf/2511.20555)
- **解读**: CLI 模糊测试需同时变异命令行选项与输入文件才能触达深层目标函数，而生成语义丰富的选项与输入恰是难点。PILOT 将到达目标函数的潜在调用路径作为上下文提供给 LLM 以生成选项字符串与输入文件，并迭代地把已覆盖函数作为附加上下文逐步逼近目标函数。在真实 CLI 应用上覆盖率高于 SOTA 模糊方法，发现 51 个零日漏洞（41 个获开发者确认、33 个已修复、3 个已分配 CVE）。


### Contextualizing Sink Knowledge for Java Vulnerability Discovery.
- **作者**: Fabian Fleischer, Cen Zhang, Joonun Jang, Jeongin Cho, Meng Xu, Taesoo Kim
- **arXiv PDF**: [Contextualizing Sink Knowledge for Java Vulnerability Discovery.](https://arxiv.org/pdf/2604.01645)
- **解读**: Java 漏洞多源于安全敏感 API（如文件操作导致路径遍历、反序列化导致远程代码执行）的不安全使用，而现有模糊器忽视 sink API 蕴含的到达约束与利用条件。GONDAR 以 sink 为中心：先用 CWE 特定扫描与 LLM 辅助静态过滤识别可达且可利用的 sink 调用点，再部署探索智能体（迭代求解路径约束以到达目标）与利用智能体（推理并满足触发条件合成 PoC），并与覆盖率引导模糊器持续交换种子与反馈。在真实 Java 基准上发现的漏洞数是 Jazzer 的 4 倍；早期版本曾助力 DARPA AI Cyber Challenge 冠军团队，且已集成进 OpenSSF 的 OSS-CRS 项目。


### deepSURF: Detecting Memory Safety Vulnerabilities in Rust Through Fuzzing LLM-Augmented Harnesses.
- **作者**: Georgios C. Androutsopoulos, Antonio Bianchi
- **解读**: Rust 默认内存安全，但 unsafe 代码误用仍会引入漏洞，现有工具检测能力有限、难处理 Rust 特有类型或依赖大量人工。deepSURF 结合静态分析与 LLM 引导的模糊 harness 生成：用自定义类型替代泛型并生成所需 trait 的实现以模拟用户自定义行为，还动态用 LLM 增强 harness 以探索复杂 API 交互。在 63 个真实 crate 上重现 30 个已知漏洞并发现 12 个未知漏洞（11 个已分配 RustSec ID、3 个已修复），优于现有工具。


### CenRL: A Framework for Performing Intelligent Censorship Measurements.
- **作者**: Hieu Le, Armin Huremagic, Kevin Wang, Roya Ensafi, Ram Sundara Raman
- **解读**: 提出 CenRL 框架，用强化学习智能规划网络审查测量，自适应选择探测策略与目标，以在受限网络环境下高效发现审查行为。降低测量成本并提高对复杂、动态审查机制的发现能力。（无摘要，据标题推断）


### Prrr: Personal Random Rewards for Blockchain Reporting.
- **作者**: Hongyin Chen, Yubin Ke, Xiaotie Deng, Ittay Eyal
- **解读**: 区块链智能合约常依赖链下报告，设计激励及时报告的协议面临安全-性能权衡：依赖少数可信发布者带来中心化风险，开放发布又导致报告数量过多。作者证明该权衡的根源是对称奖励设计，且任何对称奖励机制都无法克服；据此提出 Prrr 协议，通过"事前合成不对称"（Ex-Ante Synthetic Asymmetry）机制设计概念为发布者分配随机异构奖励值，并采用二价式结算分配奖励以保证激励兼容。Prrr 构成子博弈完美纳什均衡，对合谋与 Sybil 攻击稳健，适用于多种智能合约场景。


### CenAlert: Amplifying User Voices to Rally Censorship Investigation.
- **作者**: Aaron Ortwein, Anna Ablove, Armin Huremagic, Luqin Chang, Vinicius Fortuna, Roya Ensafi
- **解读**: 通过放大用户的声音（众包反馈）汇聚网络审查调查，收集分散用户的访问异常与失败反馈以定位审查事件与机制。目标是提高审查测量的覆盖面、及时性与可扩展性。（无摘要，据标题推断）


### Promoguardian: Detecting Promotion Abuse Fraud with Multi-Relation Fused Graph Neural Networks.
- **作者**: Shaofei Li, Xiao Han, Ziqi Zhang, Minyao Hua, Shuli Gao, Zhenkai Liang, Yao Guo, Xiangqun Chen, Ding Li
- **解读**: 促销滥用是电商增长最快的欺诈类型之一，欺诈者利用促销规则牟取平台利益。作者首次对美团进行促销滥用研究，发现基于群组活动的两类行为（含囤货返现滥用等），其与普通顾客的合法交易相互交织、难以区分。PROMOGUARDIAN 提出多关系融合图神经网络，整合时空视角的交易数据检测欺诈，在美团真实数据上达到 93.15% 的精确率，比 SOTA 方法多检测 2.1–5.0 倍欺诈者，并在生产环境阻止 1.5–8.8 倍的损失。


### Hidden Secrets in the arXiv: Discovering, Analyzing, and Preventing Unintentional Information Disclosure in Source Files of Scientific Preprints.
- **作者**: Jan Pennekamp, Johannes Lohmöller, David Schütte, Joscha Loos, Martin Henze
- **解读**: 系统调查科学预印本（arXiv）源文件中无意的信息泄露，分析作者上传的 LaTeX 等源文件泄露的敏感信息及其范围。并提出发现、分析此类泄露的方法与预防措施。（无摘要，据标题推断）


### Revelio: Blurred Images Can Still Disclose Your Identity.
- **作者**: Haoyu Zhai, Shuo Wang, Pirouz Naghavi, Qingying Hao, Gang Wang
- **解读**: 研究模糊图像的身份泄露风险：即使图像被模糊/打码处理，仍可能通过识别方法披露个人身份。揭示模糊作为隐私保护手段的失效场景，评估该威胁的现实严重性。（无摘要，据标题推断）


### Fine-Grained Kernel Auditing Using Augmented Syscall Reference Behavior Analysis and Virtualized Selective Tracing.
- **作者**: Chuqi Zhang, Spencer Faith, Feras Al-Qassas, Theodorus Februanto, Zhenkai Liang, Adil Ahmad
- **解读**: 结合增强的系统调用参考行为分析与虚拟化选择性追踪，实现细粒度内核审计。先建立高保真的系统调用行为基线，再有选择地追踪可疑执行路径，在降低审计开销的同时提升异常检测能力。（无摘要，据标题推断）


### Fractal: An Operating System Designed for Microarchitecture Reverse Engineering.
- **作者**: Joseph Ravichandran, Mengjia Yan
- **解读**: 为微架构逆向工程专门设计操作系统 Fractal，提供精确控制硬件行为、测量微架构状态的运行时环境。目标是把微架构侧信道与硬件漏洞研究从现有操作系统干扰与限制中解放出来。（无摘要，据标题推断）


### Stop Starving or Stuffing Me: Boosting Firmware Fuzzing Efficiency with On-Demand Input Delivery.
- **作者**: Shandian Shen, Wei Zhou, Keming Zhao, Peng Liu, Chung Hwan Kim, Le Guan
- **arXiv PDF**: [Stop Starving or Stuffing Me: Boosting Firmware Fuzzing Efficiency with On-Demand Input Delivery.](https://arxiv.org/pdf/2605.16798)
- **解读**: 固件输入可异步到达、时序与数量不确定，现有模糊器随意投递输入导致"撑爆"（stuffing）处理函数或"饿死"（starving）输入处理路径。FIDO 利用固件使用数据前必须先检查输入可用性这一事实，用静态与动态分析把每条输入处理路径映射为"输入获取→可用性检查→处理"三阶段，在可用性检查点按预期长度范围精准投递输入，并优化调度以覆盖更多输入路径。作为插件相比 Fuzzware 与 MULTIFUZZ 的中位覆盖率分别提升最多 115% 与 54%，相比需人工指定投递点的 SEmu 仍提升最多 19%，并发现 5 个此前未知的 bug。


### PufferDoS: Efficient and Effective Attack String Generation for Regular Expression Denial of Service Vulnerabilities.
- **作者**: Shangzhi Xu, Ziqi Ding, Xiao Cheng, Yuekang Li, Nan Sun, Benjamin Turnbull, Shuangxiang Kan, Siqi Ma
- **arXiv PDF**: [PufferDoS: Efficient and Effective Attack String Generation for Regular Expression Denial of Service Vulnerabilities.](https://arxiv.org/pdf/2606.19654)
- **解读**: 现有 ReDoS 攻击串生成常假设不切实际的输入长度预算，且未在程序层面验证有效性，导致生成的串在真实程序中无法触发漏洞。PufferDoS 先基于观察与形式化验证定义三类易受攻击的正则模式，据此合成攻击串，再用 ReDoS 特化的组合式 concolic 执行对攻击串进行精炼与验证，保证其在真实程序、现实长度预算内可被利用。


### HEAP LOCALIZATION: Cache Side-Channel Based Linux Kernel Heap Exploit Techniques.
- **作者**: Yoochan Lee, Sihyun Roh, Hyuk Kwon, Byoungyoung Lee, Thorsten Holz
- **解读**: 提出基于缓存侧信道的 Linux 内核堆定位技术，在无源码、无直接指针泄露的条件下定位内核堆对象布局，为漏洞利用提供精确的堆布局信息。将侧信道观测转化为内核堆利用的实用技术。（无摘要，据标题推断）


### NetPanic: the Attack Surface You Can't Syscall.
- **作者**: Tianshuo Han, Zong Cao, Zhen Dong, Xiapu Luo, Zhenyu Song, Jian Liu
- **解读**: 研究无法通过系统调用触达的攻击面（如网卡、网络固件、旁路加速路径等），分析其安全暴露与可利用性。揭示传统 syscall 视角之外的内核与网络攻击面及其威胁。（无摘要，据标题推断）


### APIECHO: Training-Less Anomaly Detection via Intra-API Behavioral Comparison for Web Applications.
- **作者**: Yihao Peng, Yiming Wu, Du Wu, Shouling Ji, Hai Wan, Xibin Zhao
- **解读**: 提出 APIECHO，无需训练的 Web 应用异常检测方法：通过同一 API 内部行为模式的相互比较（而非跨时间学习或标注数据）识别异常。避免对训练数据与人工标注的依赖，提升异常检测的可部署性与通用性。（无摘要，据标题推断）

### PortGPT: Towards Automated Backporting Using Large Language Models.
- **作者**: Zhaoyang Li, Zheng Yu, Jingyi Song, Meng Xu, Yuxuan Luo, Dongliang Mu
- **arXiv PDF**: [PortGPT: Towards Automated Backporting Using Large Language Models.](https://arxiv.org/pdf/2510.22396)
- **解读**: 补丁反向移植（将主线安全补丁迁移到旧分支）是维护开源项目（如 Linux 内核）的重要工作，但人工成本高，而现有自动化方法依赖预定义语法/语义规则，难以应对复杂补丁。论文提出 PORTGPT，一个端到端自动化补丁反向移植的 LLM 智能体：增强 LLM 按需访问代码、总结 Git 历史，并根据（如编译器给出的）反馈自主修订补丁，模拟人类式的推理与验证。在已有数据集（1815 例）上达到 89.15% 成功率，在自建的 146 例复杂用例上达到 62.33%，均优于现有最先进工具；其贡献的 9 个补丁已被 Linux 内核社区合并。


### Behind the Curtain: How Shared Hosting Providers Respond to Vulnerability Notifications.
- **作者**: Giada Stivala, Rafael Mrowczynski, Maria Hellenthal, Giancarlo Pellegrino
- **解读**: 大规模漏洞通知（VN）旨在帮助托管服务提供商组织（HPO）发现并修复被攻击者利用的漏洞，但此前研究多聚焦于报告方可控的因素（发送者声誉、邮件格式、沟通渠道），修复率仍持续偏低。论文转向 HPO 内部视角，对 24 名共享托管/网站开发服务从业者（覆盖不同公司规模与运营角色）进行半结构化访谈，考察内部处理流程及组织与运营因素对通知有效性的影响。研究发现多数提供商会常规处理 VN，但严格的职责边界（应用问题被视为客户责任）、低廉收费与高日流量被入侵站点抑制了主动与被动修复意愿，并存在将责任归咎于网站所有者、而后者又缺乏知识的问题，对现有通知模式能否可靠促成补救提出质疑。


### TRIGFUZZ: Triggering Conditions Guided Directed Fuzzing.
- **作者**: Yiyang Chen, Nuoqi Gui, Long Wang, Longfei Chen, Xuanqing Shi, Xi Cao, Chao Zhang
- **解读**: 针对定向模糊测试提出以"触发条件"引导的测试方法，使模糊测试能量集中于能够到达并触发目标代码路径的输入。（无摘要，据标题推断）


### Death Is Not the End: a Longitudinal Study on the Impact of Automatic Updates on Container Vulnerability Lifespans.
- **作者**: Simge Tekin, Octavian Suciu, Sungsu Kwag, Yonghwi Kwon, Tudor Dumitras
- **解读**: 对容器镜像中漏洞的存活时间进行纵向研究，考察自动更新机制对漏洞生命周期的影响——即使上游依赖被修复，容器中的漏洞仍可能长期存在。（无摘要，据标题推断）


### Web Application Vulnerability Repair Via Context-Aware Fault Localization and Directed Differential Fuzzing.
- **作者**: Chenlin Wang, Wei Meng
- **解读**: 提出结合上下文感知故障定位与定向差分模糊测试的 Web 应用漏洞修复方法：先定位与漏洞相关的代码位置，再通过定向差分模糊测试生成并验证修复方案。（无摘要，据标题推断）


### Papers, Please: A First Look at Age Verification on the Web.
- **作者**: Shreyas Minocha, Isaac Sheridan, Harry Oppenheimer, Paul Pearce, Michael A. Specter
- **解读**: 对 Web 上的年龄验证机制进行首次测量研究，考察各网站年龄验证的实现方式、覆盖范围、有效性及其隐私影响。（无摘要，据标题推断）


### Who Taught the Lie? Responsibility Attribution for Poisoned Knowledge in Retrieval-Augmented Generation.
- **作者**: Baolei Zhang, Haoran Xin, Yuxi Chen, Zhuqing Liu, Biao Yi, Tong Li, Lihai Nie, Zheli Liu, Minghong Fang
- **解读**: RAG 系统高度易受投毒攻击（恶意文本插入知识库即可影响模型输出），而现有防御常被更自适应或更复杂的攻击绕过。论文提出 RAGOrigin，一个黑盒责任归因框架，用于识别导致误导性错误生成的责任文本：针对每次误生成事件构建聚焦范围，依据检索排名、语义相关性与对生成结果的影响为候选文本打分，再用无监督聚类隔离被投毒内容。在七个数据集、十五种攻击策略及多攻击者场景下评估，归因效果优于现有基线，且在动态与噪声条件下保持鲁棒，为溯源 RAG 系统知识污染提供了实用方案。


### When AI Meets the Web: Prompt Injection Risks in Third-Party AI Chatbot Plugins.
- **作者**: Yigitcan Kaya, Anton Landerer, Stijn Pletinckx, Michelle Zimmermann, Christopher Kruegel, Giovanni Vigna
- **arXiv PDF**: [When AI Meets the Web: Prompt Injection Risks in Third-Party AI Chatbot Plugins.](https://arxiv.org/pdf/2511.05797)
- **解读**: 提示注入对 LLM 构成严重威胁，但此前研究集中于个人 copilot 等前沿应用，对广泛部署的客服聊天机器人及其第三方插件中介场景研究不足。论文首次大规模研究 17 个第三方聊天机器人插件（被超 1 万个公开网站使用）：8 个插件（约 8000 个网站）未保障网站访客与聊天机器人间网络请求中对话历史的完整性，攻击者可伪造对话历史（含伪造系统消息），将直接提示注入诱导能力（如代码生成）放大 3–8 倍；15 个插件提供网页抓取等工具，却不区分网站可信内容与第三方不可信内容，引入间接提示注入风险，约 13% 的电商网站已把聊天机器人暴露给第三方内容。受控实验表明许多插件的不安全实践削弱了 LLM 内置安全机制。


### PromptLocate: Localizing Prompt Injection Attacks.
- **作者**: Yuqi Jia, Yupei Liu, Zedian Shao, Jinyuan Jia, Neil Zhenqiang Gong
- **解读**: 提示注入攻击通过在被污染的输入数据中注入由指令和数据组成的提示，诱使 LLM 执行攻击者指定任务；在被污染内容中定位注入提示对事后取证分析与恢复至关重要，但此前该问题鲜有研究。论文提出 PromptLocate，首个提示注入定位方法，包含三步：切分语义连贯片段、识别其中的指令片段、精确定位注入提示。实验表明其在八种现有自适应攻击下均能准确定位。


### LLMs in the SOC: An Empirical Study of Human-AI Collaboration in Security Operations Centres.
- **作者**: Ronal Singh, Shahroz Tariq, Fatemeh Jalalvand, Mohan Baruwal Chhetri, Surya Nepal, Cécile Paris, Martin Lochner
- **arXiv PDF**: [LLMs in the SOC: An Empirical Study of Human-AI Collaboration in Security Operations Centres.](https://arxiv.org/pdf/2508.18947)
- **解读**: LLM 进入安全运营中心（SOC）的真实世界应用尚缺乏研究。论文对 45 名 SOC 分析师 10 个月内 3090 条查询进行纵向研究：分析师将 LLM 用作按需的感知与上下文构建辅助，而非用于高风险决策，保留了分析师的决策权威；多数查询围绕解读底层遥测（如命令）与润色技术沟通，交互轮次短（1–3 轮），93% 的查询与 NICE 框架定义的网络安全能力一致。使用趋势显示 LLM 从偶发探索转向常规集成，其角色是对 SOC 专业知识的按需认知增强而非替代，为设计以人为中心的 SOC 安全 AI 辅助提供了指导。


### Incalmo: an Autonomous Llm-Assisted System for Red Teaming Multi-Host Networks.
- **作者**: Brian Singer, Keane Lucas, Lakshmi Adiga, Meghna Jain, Lujo Bauer, Vyas Sekar
- **arXiv PDF**: [Incalmo: an Autonomous Llm-Assisted System for Red Teaming Multi-Host Networks.](https://arxiv.org/pdf/2501.16466)
- **解读**: 真实企业环境中的红队演练需跨多台"跳板"主机执行攻击，成本高、依赖专家；论文先分析发现现有 LLM 辅助攻击系统（如 PentestGPT、CyberSecEval3）配合顶级 LLM（Sonnet 4、Gemini 2.5 Pro）也无法自主完成此类任务。基于对失败模式的分析，论文提出 Incalmo：用 LLM 将红队演练规划为高层声明式任务，由领域专用任务智能体执行，并借助辅助服务管理上下文与已获取资产；同时构建含 40 个仿真网络（22–50 台主机）的多主机攻击基准 MHBench。Incalmo 在 37/40 个环境中成功获取关键资产（现有系统仅 3/40），成功攻击耗时 12–54 分钟、LLM 花费不足 15 美元。


### PromptCOS: Towards Content-Only System Prompt Copyright Auditing for LLMs.
- **作者**: Yuchen Yang, Yiming Li, Hongwei Yao, Enhao Huang, Shuo Shao, Yuyi Wang, Zhibo Wang, Dacheng Tao, Zhan Qin
- **arXiv PDF**: [PromptCOS: Towards Content-Only System Prompt Copyright Auditing for LLMs.](https://arxiv.org/pdf/2509.03117)
- **解读**: 高质量系统提示已成为 LLM 应用中的重要知识产权，面临被窃取与未授权使用风险；现有版权审计/水印方法依赖查询触发的 logit 分布偏移，在真实"仅内容"场景中不可行（随机采样使内容级生成不稳定，更强指令又损害提示保真度）。论文提出 PromptCOS，首个基于内容级输出相似性的系统提示版权审计方法：设计循环输出信号作为条件指令目标以稳定水印，注入少量辅助 token 编码水印而不改动主提示，并优化原提示中的关键 token 使移除辅助 token 造成严重性能退化。实验显示水印相似度平均 99.3%、区分度比最优基线高 60.8%、准确率下降不超过 0.6%、可抵抗四类移除攻击且计算成本节省最高 98.1%。


### AttnTrace: Contextual Attribution of Prompt Injection and Knowledge Corruption.
- **作者**: Yanting Wang, Runpeng Geng, Ying Chen, Jinyuan Jia
- **arXiv PDF**: [AttnTrace: Contextual Attribution of Prompt Injection and Knowledge Corruption.](https://arxiv.org/pdf/2508.03793)
- **解读**: 长上下文 LLM（如 Gemini-2.5-Pro、Claude-Sonnet-4）广泛用于 RAG 流水线与自主智能体，追溯上下文中对生成结果贡献最大的文本片段（上下文追溯）对攻击取证与输出可解释性意义重大，但现有方法（如 TracLLM）计算开销极高（单个响应-上下文对需数百秒）。论文提出 AttnTrace，基于 LLM 对提示产生的注意力权重进行上下文追溯，引入两项增强技术并给出设计选择的理论依据；系统评估表明其比现有最先进追溯方法更准确、更高效，并通过"先归因后检测"范式提升长上下文下提示注入的检测效果。作为真实应用，AttnTrace 能精确定位用于操纵 LLM 生成评审的论文中被注入的指令。


### Hollow-LLM Attack: Computationally Trivial Weights in Zero-Knowledge Verification of LLM Inference.
- **作者**: Chen Gong, Beijie Liu, Mengyuan Li
- **解读**: 远程提供的大模型需要可验证其确实执行了宣称的计算负载；零知识（ZK）LLM 通过证明输出与"在承诺的私有权重下执行声明架构"一致来提供逐实例的等式正确性保证，但并未约束实际付出的计算量。论文形式化这一被忽视的缺口并提出 Hollow-LLM 攻击：不诚实的服务方保留声明的参数量，但嵌入具有坍缩代数结构的"幽灵权重"，使有效计算大幅降低，且这些见证仍能通过验证电路产生有效证明，使服务方以小模型成本交付"可证明正确"的输出。作者刻画了可组合标准 Transformer 块的攻击族，证明此类部署可在零质量损失下显著降低服务成本，说明大模型的证明还需额外的可验证工作量约束。


### Leafblower: a Leakage Attack Against Tee-Based Encrypted Databases.
- **作者**: Zachary Espiritu, Seny Kamara, Tarik Moataz, Valentin Ogier
- **解读**: 针对基于 TEE 的加密数据库提出泄漏攻击，利用 TEE 侧存在的泄漏通道推断查询或数据内容，威胁加密数据库的机密性保证。（无摘要，据标题推断）


### AEX-NStep: Probabilistic Interrupt Counting Attacks on Intel SGX.
- **作者**: Nicolas Dutly, Friederike Groschupp, Ivan Puddu, Kari Kostiainen, Srdjan Capkun
- **解读**: 为缓解基于中断的单步攻击（如 SGX-Step），Intel 引入 AEX-Notify 扩展以阻止确定性单步执行。论文提出 AEX-NStep，首个针对启用 AEX-Notify 的 enclave 的中断计数攻击，证明实际可行的攻击并不需要单步执行，因此 AEX-Notify 无法完全消除此类攻击。作者针对 AEX-Notify 的"混淆前进进度"安全保证构造两种新的概率性攻击，并据此实现对启用 AEX-Notify 的 enclave 的 ECDSA 密钥泄漏，扩展了原有攻击分析并为未来缓解设计提供参考。


### Battering RAM: Low-Cost Interposer Attacks on Confidential Computing via Dynamic Memory Aliasing.
- **作者**: Jesse De Meulemeester, David F. Oswald, Ingrid Verbauwhede, Jo Van Bulck
- **解读**: 机密计算（如 Intel SGX/TDX、AMD SEV-SNP）依赖硬件级内存加密抵御总线嗅探与冷启动等物理威胁，近期扩展还加入访问控制检查以防御基于别名的密文篡改。论文构建低成本（价格远低于商业 DRAM 插接板的）DDR4 内存插接板（interposer），运行时动态篡改地址线，绕过当前 TEE 的防护；其运行时特性可绕过为响应攻击而引入的启动期固件缓解。借助该设备实现首个针对 Scalable SGX 单密钥域的任意明文读写攻击并提取平台预置密钥，瓦解远程认证信任；还可在最新 SEV-SNP 平台上重新实现完整认证绕过，突破针对静态别名的防御，凸显机密计算系统的性能-安全权衡缺陷。


### TEE.Fail: Breaking Trusted Execution Environments via DDR5 Memory Bus Interposition.
- **作者**: Jalen Chuang, Alexander Seto, Nicolás Berrios, Stephan van Schaik, Christina Garman, Daniel Genkin
- **解读**: 通过在 DDR5 内存总线上进行插接/中间人干预，破坏 TEE（如可信执行环境）的机密性与完整性保护，实现攻击者可控的内存访问。（无摘要，据标题推断）


### PrintSpy: Pixel-Level Eavesdropping on Commodity Laser Printers via Electromagnetic Side Channels.
- **作者**: Wenhao Li, Jiarong Yang, Mingda Han, Xiuzhen Cheng, Pengfei Hu, Cong Wang
- **解读**: 利用商用激光打印机的电磁侧信道实施像素级窃听，重建被打印文档的内容，展示打印过程中的机密信息泄漏风险。（无摘要，据标题推断）


### TÄMU: Emulating Trusted Applications at the (GlobalPlatform)-API Layer.
- **作者**: Philipp Mao, Li Shi, Marcel Busch, Mathias Payer
- **arXiv PDF**: [TÄMU: Emulating Trusted Applications at the (GlobalPlatform)-API Layer.](https://arxiv.org/pdf/2601.20507)
- **解读**: 移动设备 TEE 中的可信应用（TA）漏洞可能危及整个系统，但移动 TEE 的闭源与碎片化特性严重阻碍 TA 的动态分析，测试多限于静态分析。论文提出 TÄMU，一个在 API 层拦截 TA 执行的重托管平台，实现针对 TA 的模糊测试与调试；借助 GlobalPlatform 规范推动的 TEE API 标准化扩展到不同 TEE 的众多 TA，对非共享的厂商专属 API 引入"贪婪高层模拟"技术，按模糊测试中潜在覆盖率增益优先投入人工重托管。实现中模拟了 4 个 TEE 的 67 个 TA，模糊测试发现 11 个 TA 中的 17 个零日漏洞，揭示 TEE 生态普遍缺乏动态分析能力，TÄMU 可弥合这一差距。


### Goldilocks and the Three P-States: Mitigating Hertzbleed with Formal Leakage Guarantees.
- **作者**: Inwhan Chun, Christine Guo, Riccardo Paccagnella
- **解读**: 针对 Hertzbleed（通过动态电压频率调节状态即 P-state 传导的功率侧信道）提出缓解方案，在处理器 P-state 的选择/配置上给出形式化的泄漏保证。（无摘要，据标题推断）


### TDXRay: Microarchitectural Side-Channel Analysis of Intel TDX for Real-World Workloads.
- **作者**: Tristan Hornetz, Hosein Yavarzadeh, Albert Cheu, Adrià Gascón, Lukas Gerlach, Daniel Moghimi, Phillipp Schoppmann, Michael Schwarz, Ruiyi Zhang
- **解读**: 对 Intel TDX 信任域进行微架构侧信道分析，评估真实负载下信任域机密性面临的信息泄漏风险。（无摘要，据标题推断）


### Investigating the Impact of Dark Patterns on LLM-Based Web Agents.
- **作者**: Devin Ersoy, Brandon Lee, Ananth Shreekumar, Arjun Arunasalam, Muhammad Ibrahim, Antonio Bianchi, Z. Berkay Celik
- **解读**: 用户日益依赖 LLM 驱动的 Web 智能体自动完成在线任务，而针对人类用户的欺骗性 UI 设计（暗模式）对通用智能体的影响此前未被研究。论文首次研究暗模式对 LLM Web 智能体决策的影响：构建轻量框架 LiteAgent 自动向智能体派发任务并记录完整日志与屏幕录像，以及可控环境 TrickyArena（覆盖电商、流媒体、新闻等领域的应用，暗模式可选择性开关）。在三种 LLM 上评估六个主流智能体后发现：存在单一暗模式时智能体平均 41% 的时间会受影响，且通过视觉设计改动（HTML 代码调整）与同时引入多个暗模式可进一步影响其易感性，凸显智能体需要整体性防御机制。


### Demystifying the (In)Security of Oauth-Based Account Linking in Connector Ecosystems.
- **作者**: Kaixuan Luo, Xianbo Wang, Adonis P. H. Fung, Wing Cheong Lau
- **解读**: 分析连接器生态中基于 OAuth 的账户关联机制的安全性，揭示其在授权流程、令牌处理或跨服务状态管理中的安全缺陷及其可被利用的方式。（无摘要，据标题推断）


### When Designers Meet GenAI: Understanding the Role of Prompt-to-Design Generators in Privacy Dark Patterns.
- **作者**: Jingzhou Ye, Zhaojie Hu, Yao Li, Xueqiang Wang
- **解读**: 研究设计师使用提示词到设计（Prompt-to-Design）生成器等 GenAI 工具时的实践与认知，考察这类工具在隐私暗模式设计与产生中所扮演的角色。（无摘要，据标题推断）


### SoK: After Decades of Web Tracker Detection, What's Next?
- **作者**: Wolf Rieder, Philip Raschke, Thomas Cory, Christian René Sechting, Aditya Kumar, Axel Küpper
- **arXiv PDF**: [SoK: After Decades of Web Tracker Detection, What's Next?](https://arxiv.org/pdf/2605.02982)
- **解读**: 网络跟踪无处不在，过滤列表与拦截器之后研究者开发了 Web 跟踪检测器，但此前缺少对检测器的统一视角与分类。论文给出迄今最全面的 Web 跟踪检测元科学研究：对 832 篇论文进行系统综述，得到 59 篇主要研究与 16 篇补充研究，据此提出分类法、观察并评估发展趋势、指出开放研究缺口并给出建议；此外还进行有限的复现性研究以评估既有研究的有效性并突出该领域新出现的问题。


### Your Eyes Won't Lie: Snooping Online Voting Privacy from User Webcam.
- **作者**: Zeyu Deng, Jingwei Zhang, Chen Wang
- **解读**: 研究攻击者通过用户摄像头观察在线投票过程，以推断投票选项或投票偏好的隐私攻击，揭示在线投票场景中来自视觉侧信道的隐私泄漏。（无摘要，据标题推断）


### Weaponizing Reflectivity for Pointcloud Deception with Forged Invisible Geometries.
- **作者**: Hengwei Chen, Menglan Hu, Tianyue Zheng
- **解读**: 利用反射率（reflectivity）特性在点云中伪造不可见几何结构，实施对激光雷达/点云感知系统的欺骗攻击，使目标被错误检测或隐藏。（无摘要，据标题推断）


### PLaTypus: Restricting Cross-Module Transitions to Mitigate Code-Reuse Attacks.
- **作者**: Apostolos Chatzianagnostou, Marcos Bajo, Christian Rossow
- **解读**: 提出通过限制跨模块的代码转移（transition）来缓解代码复用攻击的防御机制，约束跳转/调用目标范围以遏制 ROP/JOP 类攻击。（无摘要，据标题推断）


### Crashing Through Defenses: Exploiting Segfaults and Chaining Around Intel CET.
- **作者**: Marcos Bajo, Ritvik Goyal, Apostolos Chatzianagnostou, Christian Rossow
- **解读**: 研究利用段错误（segfault）等异常/崩溃路径绕过或规避 Intel CET（影子栈与 IBT）防护，围绕 CET 进行 gadget 链构造的攻击方法。（无摘要，据标题推断）


### BreakFAST: Confused Deputy Attack on Infinity Fabric to Break AMD SEV-SNP.
- **作者**: Philipp Giersfeld, Benedict Schlüter, Shweta Shinde
- **解读**: 通过对 AMD Infinity Fabric（片上互连）实施 confused deputy 攻击，利用高权限代理组件被诱导执行未授权操作，破坏 SEV-SNP 的机密性或完整性保证。（无摘要，据标题推断）


### One Char to Rule Them All: Systematically Exploring and Exploiting DNS Silent Vulnerabilities in Domain Name Resolution.
- **作者**: Fasheng Miao, Xiang Li, Changqing An, Wenbin Xu, Jilong Wang
- **解读**: 系统性地探索并利用域名解析过程中由单个字符差异等引发的"静默"漏洞，揭示字符混淆/相似域名导致解析结果异常的安全风险。（无摘要，据标题推断）


### Resolve the Unresolved: Systematic Work Profiling for DNS Resolvers.
- **作者**: Liwen Xu, Huayi Duan, Zechao Cai, Adrian Perrig
- **解读**: 对 DNS 解析器的解析工作进行系统性画像，通过剖析解析流程与查询特征揭示解析过程中的异常行为或潜在安全问题。（无摘要，据标题推断）


### Poisoned by the Host: Large-Scale Measurement of Host Name Poisoning in Web Applications.
- **作者**: Rui Yang, Haoyu Wang, Zhicheng Sun, Zhengyu Liu, Yinzhi Cao
- **解读**: 对 Web 应用中主机名（Host 头）投毒攻击进行大规模测量，评估其影响范围、成因及对应用安全（如缓存投毒、重定向劫持）的危害。（无摘要，据标题推断）


### Knocking on the Front Door: An LLM-Guided Systematic Analysis of DNS Query Processing Vulnerabilities.
- **作者**: Yuqi Qiu, Xiang Li, Zheli Liu
- **解读**: 利用 LLM 引导对 DNS 查询处理过程进行系统性漏洞分析（如生成测试用例或定向模糊测试），发现 DNS 解析器实现中的缺陷。（无摘要，据标题推断）


### APT to Disagree: A Comparative Analysis of Attribution in Commercial TI.
- **作者**: Aksel Ethembabaoglu, Rolf van Wegberg, Yury Zhauniarovich, Michel van Eeten
- **解读**: 对商业威胁情报（TI）供应商的 APT 归因结论进行比较分析，考察不同厂商在攻击组织归因上的一致性与分歧，并分析分歧产生的来源与影响。（无摘要，据标题推断）
