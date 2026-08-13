---
title: USENIX Security 2026 录用论文深度解读（165 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, USENIX Security, 2026]
---

USENIX Security 2026（第 35 届）于 2026 年 8 月 12-14 日在波士顿举行。本文对 USENIX Security 2026 第一轮（Cycle 1）全部 165 篇录用论文逐一给出中文深度解读，并附上可获取的论文 PDF 链接。解读基于 USENIX 官方页面的论文摘要。第二轮录用名单公布后将另行更新。

## 全部 165 篇录用论文解读

### Bond: Constraint-Directed Fuzzing for Automated Validation of Taint Analysis Results in Linux-based IoT Firmware
- **作者**: Jiaqian Peng, Puzhuo Liu, Kai Cheng, Zhaoteng Yan, Jie Liu, Chengnian Sun, Hongsong Zhu
- **论文 PDF**: [Bond: Constraint-Directed Fuzzing for Automated Validation of Taint Analysis Results in Linux-based IoT Firmware](https://www.usenix.org/system/files/usenixsecurity26-peng-jiaqian.pdf)
- **解读**: 静态污点分析工具在 Linux 固件上产生大量缺乏验证的报告。Bond 是一个衔接静态污点分析与动态漏洞验证的定向模糊测试框架，通过整合三类约束、六种语义类型进行约束引导输入变异，高效探索与污点报告相关的路径。在 8 家厂商 19 台 IoT 设备上对四个先进污点分析器产生的 2,776 份报告进行验证，确认 1,349 个真实漏洞（含 155 个此前未知漏洞，其中 108 个已分配 CVE/PSV 编号），对 60 个已知漏洞的召回率达 91.67%，且比四个领先 IoT 模糊器在漏洞验证上最多提升 5.5 倍。


### Heli: Heavy-Light Private Aggregation
- **作者**: Ryan Lehmkuhl and Henry Corrigan-Gibbs, Emma Dauterman, David J. Wu
- **论文 PDF**: [Heli: Heavy-Light Private Aggregation](https://www.usenix.org/system/files/usenixsecurity26-lehmkuhl.pdf)
- **解读**: 双服务器聚合统计面临如何在保护单个客户端隐私的同时，让其中一方的每轮工作量与客户端数无关的问题。Heli 依赖新密码原语 aggregation-only encryption："重服务器"每轮工作量与客户端数成正比，"轻服务器"在一次设置后工作量与客户端数无关，同时保持对恶意服务器的隐私保护与对恶意客户端的正确性。在千万客户端、计算 32 个客户端持有的 1 位整数之和的部署中，重服务器需 240,000 core-s、轻服务器仅 7 core-ms（比先前工作少 120,000 倍），使低预算非营利组织等算力受限方也能担任第二服务器。


### "Your imaging may be stone-cold normal, but if they look sick, they're going to get admitted": An Investigation of Clinicians' Perceptions of Impact & Likelihood of Security Failures
- **作者**: Ronald E. Thompson III and Hamza Khalid, Hilary Fisher, Rhea Votipka, Daniel Votipka
- **论文 PDF**: ["Your imaging may be stone-cold normal, but if they look sick, they're going to get admitted": An Investigation of Clinicians' Perceptions of Impact & Likelihood of Security Failures](https://www.usenix.org/system/files/usenixsecurity26-thompson.pdf)
- **解读**: 针对安全控制在临床环境中的失效，通过 12 名美国临床医生访谈和 303 名美英加临床医生调查，考察医生对安全失败的感知。研究发现威胁感知与部署控制严重错位：医生认为机密性失败（如数据泄露）最可能发生，将完整性失败（如数值被篡改）视为灾难性却信任自身专业判断可忽略异常数据，而对高发且危险的可用性失败采用纸面记录等模拟方式应对，反而引入新风险。结论是需将临床医生纳入安全设计，并据此提出以临床医生为中心的安全改进建议。


### Hop: A Modern Transport and Remote Access Protocol
- **作者**: Paul Flammarion, George Hosono, Wilson Nguyen, David Adrian, Zakir Durumeric
- **论文 PDF**: [Hop: A Modern Transport and Remote Access Protocol](https://www.usenix.org/system/files/usenixsecurity26-flammarion.pdf)
- **解读**: SSH 标准化近 20 年后，远程访问协议的现实需求与安全密码协议构建认知均已演进。Hop 是基于现代密码学进展设计的新传输与远程访问协议：通过密码学中介的委托方案、借鉴 TLS 与 ACME 经验的原生主机标识、面向现代企业环境的客户端认证，以及对客户端漫游与间歇连接的支持，降低协议复杂度与开销并弥补 SSH 的诸多不足。论文给出现代远程访问协议的具体设计需求、协议描述与性能评估，旨在推动对未来远程访问协议形态的讨论。


### Interpolation-Based Optimization for Enforcing lp-Norm Metric Differential Privacy in Continuous and Fine-Grained Domains
- **作者**: Chenxi Qiu
- **论文 PDF**: [Interpolation-Based Optimization for Enforcing lp-Norm Metric Differential Privacy in Continuous and Fine-Grained Domains](https://www.usenix.org/system/files/usenixsecurity26-qiu-chenxi.pdf)
- **解读**: 度量差分隐私（mDP）按点对距离适配隐私保证，但现有优化方法在细粒度或连续域中构造稠密扰动矩阵并满足逐点约束的计算代价过高。该文提出插值框架：在稀疏锚点上优化扰动分布，非锚点位置经 log-convex 组合插值且可证明保持 mDP；针对高维下朴素插值导致的隐私破坏，将插值分解为一维步骤序列并推导修正公式，从设计上保证 ℓp 范数 mDP，还联合优化扰动分布与跨维度隐私预算分配。真实位置数据集实验表明该方法在细粒度域提供严格隐私保证且效用优于基线机制。


### Membership Inference Attacks on Tokenizers of Large Language Models
- **作者**: Meng Tong, Yuntao Du, Kejiang Chen and Weiming Zhang, Ninghui Li
- **论文 PDF**: [Membership Inference Attacks on Tokenizers of Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-tong.pdf)
- **解读**: 对预训练 LLM 的成员推断攻击面临错标样本、分布偏移与实验/真实模型规模差异等挑战。该文提出以 tokenizer 作为新的成员推断攻击向量：tokenizer 可从零高效训练、规避上述挑战，且其训练数据通常代表 LLM 预训练数据。这是首个关于 tokenizer 成员泄露的研究，探索五种攻击方法，在数百万互联网样本上揭示先进 LLM 的 tokenizer 存在成员泄露，并提出自适应防御；结论强调 tokenizer 是被忽视的关键隐私威胁，需专门为其设计隐私保护机制。


### JailbreakScope: Interpreting Jailbreak Mechanism through Representation and Circuit Analyses
- **作者**: Zeqing He
- **论文 PDF**: [JailbreakScope: Interpreting Jailbreak Mechanism through Representation and Circuit Analyses](https://www.usenix.org/system/files/usenixsecurity26-he-zeqing.pdf)
- **解读**: LLM 越狱机制仍缺乏细粒度解释，现有研究或只关注静态表征偏移、或只识别与生成安全相关的组件。JailbreakScope 从表征（越狱如何扭曲 LLM 的有害性感知）与电路（越狱如何影响生成安全关键电路）双视角分析越狱，并追踪整个生成过程中的演化。对 5 个主流 LLM、7 种越狱策略的评估揭示普遍规律：越狱放大强化肯定回应的组件、抑制产生拒绝的组件，使表征移向"安全区域"，导致模型输出回应而非拒绝；且表征欺骗与电路激活偏移在多种越狱与多个 LLM 间存在强一致相关。


### Imitative Membership Inference Attack
- **作者**: Yuntao Du and Yuetian Chen, Hanshen Xiao, Bruno Ribeiro and Ninghui Li
- **论文 PDF**: [Imitative Membership Inference Attack](https://www.usenix.org/system/files/usenixsecurity26-du.pdf)
- **解读**: 现有 SOTA 成员推断攻击依赖训练数百个与目标模型无关的影子模型，计算开销巨大。IMIA 采用新颖的模仿训练技术，策略性地构造少量目标感知的"模仿模型"，使其紧密复刻目标模型行为以用于推断。实验表明 IMIA 在多种攻击设置下显著优于现有 MIA，而计算成本不到 SOTA 方法的 5%。


### SMASH: Scalable Maliciously Secure Hybrid Multi-party Computation Framework for Privacy-Preserving Large Language Models
- **作者**: Yunlv Lv and Rui Zhang, Zhiyuan Zhang, Ziyi Wan, Lanxue Zhang, Minhui Xue, Jiangtao Li, Yanan Cao
- **论文 PDF**: [SMASH: Scalable Maliciously Secure Hybrid Multi-party Computation Framework for Privacy-Preserving Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-lv.pdf)
- **解读**: 恶意安全 MPC 框架扩展到大模型时因非线性算子二次通信开销与昂贵的份额转换而"性能崩溃"。SMASH 引入基于 DFT 的旋转技术与轻量零知识证明（ZKPoK）构造来求值非线性运算，首次实现通信复杂度与参与方数量线性（O(n)）且与函数复杂度无关；并提供无需昂贵密码原语的高效 A2L/L2A 与基于 SM-LUT 的 A2B/B2A 转换协议。基准测试显示 SMASH 相比 MP-SPDZ、MD-ML 等框架运行时最多快 18.9 倍、通信最多减少 10^3 倍，常数轮在线阶段与低 WAN 敏感度使其适合地理分布式 LLM 部署。


### XGuardian: Towards Generalized, Explainable and More Effective Server-side Anti-cheat in First-Person Shooter Games
- **作者**: Jiayi Zhang
- **论文 PDF**: [XGuardian: Towards Generalized, Explainable and More Effective Server-side Anti-cheat in First-Person Shooter Games](https://www.usenix.org/system/files/usenixsecurity26-zhang-jiayi.pdf)
- **解读**: 瞄准辅助作弊是 FPS 游戏最常见且危害最大的作弊形式，现有检测存在框架不可靠、泛化差、开销高、性能低与结果不可解释等问题。XGuardian 是服务端通用可解释检测系统，仅需所有 FPS 游戏必备的 pitch 与 yaw 两个原始输入，构造新颖时序特征描述瞄准轨迹以区分作弊者与正常玩家。在 CS2 及另外两个游戏上以真实大规模数据集验证，取得高于先前工作的检测性能与低开销，并能解释判定、缩短人工审核延迟，系统与数据集已开源。


### The Prompt Stealing Fallacy: Rethinking Metrics, Attacks, and Defenses
- **作者**: Zehang Deng, Haoyang Li, Wanlun Ma, Ruoxi Sun and Derui Wang, Minhui Xue, Haibo Hu, Sheng Wen and Yang Xiang
- **论文 PDF**: [The Prompt Stealing Fallacy: Rethinking Metrics, Attacks, and Defenses](https://www.usenix.org/system/files/usenixsecurity26-deng.pdf)
- **解读**: 提示词窃取攻击（PSA）的评估实践存在缺陷，仅依赖文本/图像模态间的语义相似度。该文提出 Style Similarity (SS) 与 Prompt Significance (PS) 两个更忠实的指标，显式考量修饰词重要性与窃取提示生成图像的风格复现；据此评估发现现有白盒与黑盒 PSA 均不如报告有效（白盒优化目标与 token 级视觉语义错配，黑盒与目标模型生成过程解耦导致严重信息损失）。进一步提出黑盒框架 PromptThief，用强化学习结合 STS 与 SS 引导高贡献 token 恢复，显著超越基线；对抗样本主动防御对自适应攻击鲁棒性有限，而特征级提示水印被动方案在多种图像变换下保持强且一致的检测性能。


### When AIOps Become "AI Oops": Subverting LLM-driven IT Operations via Telemetry Manipulation
- **作者**: Dario Pasquini, Evgenios M. Kornaropoulos and Giuseppe Ateniese, Omer Akgul
- **论文 PDF**: [When AIOps Become "AI Oops": Subverting LLM-driven IT Operations via Telemetry Manipulation](https://www.usenix.org/system/files/usenixsecurity26-pasquini.pdf)
- **解读**: 首个对 LLM 驱动 AIOps 方案的安全分析：对手可操纵系统遥测误导 AIOps 智能体采取损害所管理基础设施完整性的行动。攻击技术利用错误诱导请求注入遥测数据，通过一种对抗式奖励黑客形式（貌似合理但错误的系统错误解读）左右智能体决策；AIOpsDoom 将侦察、模糊与 LLM 驱动输入生成组合为全自动流程，无需目标系统先验知识。防御方 AIOpsShield 利用遥测的结构化性质与用户生成内容的最小作用对遥测消毒，实验证明可可靠阻断基于遥测的攻击且不影响正常智能体性能，凸显了安全感知 AIOps 设计的紧迫性。


### A Large-Scale Study of Personalized Phishing using Large Language Models
- **作者**: Stefan Czybik, Anne Josiane Kouam, Peter Heubl and Jan Magnus Nold, Konrad Rieck
- **论文 PDF**: [A Large-Scale Study of Personalized Phishing using Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-czybik.pdf)
- **解读**: LLM 的个性化（鱼叉式）钓鱼在规模化下的效果此前未被量化。该研究在 7700 名参与者实验中，以目标邮箱地址为查询经网络搜索收集个人信息并自动生成定制邮件：LLM 鱼叉式钓鱼的点击率比通用钓鱼（无论人类或 LLM 撰写）几乎翻三倍，且个性化成本极低（约每封 0.03 美元）。结论是需强化现有防御，例如限制可关联到邮箱地址的公开信息，并将个性化钓鱼纳入安全意识培训。


### Secure Protocol Composition under Dynamic Corruption: Scaling Up Symbolic Analysis for Real-World Security Properties
- **作者**: Cas Cremers
- **论文 PDF**: [Secure Protocol Composition under Dynamic Corruption: Scaling Up Symbolic Analysis for Real-World Security Properties](https://www.usenix.org/system/files/usenixsecurity26-cremers.pdf)
- **解读**: 自动化符号协议验证在复杂案例上需要大量专家时间，现有组合验证依赖不现实假设且不支持前向保密等真实安全属性。该文在 Applied π-Calculus 中建立了即使在能动态腐化的攻击者下也成立的组合结果，前提是协议满足不相交性要求。在数据交换协议与 Diffie-Hellman 密钥交换的组合、以及 TLS 1.3（RFC 8446 范围及 ECH 扩展）前向保密的组合分析上验证有效性：TLS 1.3+ECH 的单体分析有 10% 情况无法出结果，而组合分析全部成功，且运行时平均降低 71%、内存平均降低 86%。


### Opossum Attack: Application Layer Desynchronization using Opportunistic TLS
- **作者**: Robert Merget, Nurullah Erinola and Marcel Maehren, Lukas Knittel, Sven Hebrok, Marcus Brinkmann, Juraj Somorovsky, Jörg Schwenk
- **论文 PDF**: [Opossum Attack: Application Layer Desynchronization using Opportunistic TLS](https://www.usenix.org/system/files/usenixsecurity26-merget.pdf)
- **解读**: 隐式 TLS 与机会式 TLS 的集成方式存在新弱点：当同一实现同时支持两者时，该弱点可破坏认证，纯 MitM 位置即可影响 TLS 握手后的消息交换，且不依赖实现 bug、只需一端支持机会式 TLS。该文分析支持机会式 TLS 的流行应用协议，并针对 HTTP (RFC 2817) 详细展示四种利用方向；IPv4 大规模扫描发现超 300 万台服务器同时支持隐式与机会式 TLS，其中 20,121 台支持机会式 HTTP，2,268 台同时支持 HTTPS、539 台同域名，构成可利用场景。


### Breaking Widely Deployed Perceptual Hash Functions: Black-Box Collisions in Apple NeuralHash and Microsoft PhotoDNA
- **作者**: Diane Leblanc-Albarel and Bart Preneel
- **论文 PDF**: [Breaking Widely Deployed Perceptual Hash Functions: Black-Box Collisions in Apple NeuralHash and Microsoft PhotoDNA](https://www.usenix.org/system/files/usenixsecurity26-leblanc-albarel.pdf)
- **解读**: 感知哈希被用于检测版权侵权与非法内容，但 NeuralHash、PhotoDNA 等部署方案设计细节专有。理论上 NeuralHash（96 位）暴力碰撞需 2^48 次求值，但该文在输入限定为人脸时仅 2^16 次求值即找到感知不同图像间的精确碰撞（黑盒、无需设计知识），并展示高假阴性率；对 PhotoDNA（1152 位）在远低于此前报告的阈值下发现近碰撞（约 2^14.6–2^17 次求值）。这是首次在 NeuralHash 上演示精确碰撞、在 PhotoDNA 上发现如此低阈值的近碰撞，对该类设计用于大规模客户端扫描的适用性提出严重质疑（高假阳性与假阴性）。


### Zero Knowledge (About) Encryption: A Comparative Security Analysis of Three Cloud-based Password Managers
- **作者**: Matteo Scarlata, Giovanni Torrisi and Matilda Backendal, Kenneth G. Paterson
- **论文 PDF**: [Zero Knowledge (About) Encryption: A Comparative Security Analysis of Three Cloud-based Password Managers](https://www.usenix.org/system/files/usenixsecurity26-scarlata.pdf)
- **解读**: "零知识加密"是云密码管理器厂商常用术语，其隐含威胁模型是对完全恶意的服务器也不泄露保险库内容。该文检验宣称该保证的三家厂商（Bitwarden、LastPass、Dashlane，合计超 6000 万用户、23% 市场份额）：提出 12 个针对 Bitwarden、7 个针对 LastPass、6 个针对 Dashlane 的攻击，严重性从定向用户保险库的完整性违规到组织全部保险库的完全沦陷，多数攻击可恢复密码；发现多个常见设计反模式与密码学误解导致漏洞。发现已披露、修复进行中，并讨论了缓解措施及对端到端加密系统开发者的普适教训。


### A Midsummer Meme's Dream: Investigating Market Manipulations in the Meme Coin Ecosystem
- **作者**: Alberto Maria Mongardini, Alessandro Mei
- **论文 PDF**: [A Midsummer Meme's Dream: Investigating Market Manipulations in the Meme Coin Ecosystem](https://www.usenix.org/system/files/usenixsecurity26-mongardini.pdf)
- **解读**: 模因币价值主要来自社区情绪，易受操纵。该研究对以太坊、BNB Smart Chain、Solana 与 Base 上 34,988 个代币进行前所未有的跨链分析，刻画 tokenomics 并做三个月纵向增长跟踪：高回报（>100%）代币中 82.89% 存在人为增长策略痕迹（洗盘交易与作者新定义的流动性池价格膨胀 LPI——小额策略性买入引发价格暴涨）。抽水抛售与 rug pull 等利润提取通常紧随早期操纵，表明早期操纵为后续剥削奠定基础；识别出超 17,000 个受害地址、已实现损失超 930 万美元，说明高收益模因币的暴涨常由协同操纵而非自然市场驱动。


### Distributed Vector Commitments and Their Applications
- **作者**: Rui Gao and Huaqun Wang, Zhiguo Wan, Yuncong Hu
- **论文 PDF**: [Distributed Vector Commitments and Their Applications](https://www.usenix.org/system/files/usenixsecurity26-gao-rui.pdf)
- **解读**: 现有向量承诺（VC）面向中心化场景，无法用于输入向量分布多机或需分布式并行加速的去中心化系统。该文提出分布式 VC（DVC）新概念，使多台各持子向量的机器能共同承诺整个向量并分布式生成位置证明，并提出首个方案 HLE-DVC：利用 M 台机器并行处理长度为 N 的向量，证明大小 O(logM)，每台机器单轮通信（O(logM)）生成全部位置证明，计算 O(NlogN/M)，支持批证明、证明聚合与高效更新。256 台机器为长度 2^30 的向量生成全部证明耗时 17,515 秒，相对单机取得 256 倍并行加速、比单机方案 Hyperproofs 快 142 倍，每机通信仅 0.768 KB。


### From Mirai to Gorilla: Deep Dive into a Long-Lasting DDoS-for-Hire Botnet
- **作者**: Maarten Weyns, Daniel Wagner, Georgios Smaragdakis and Harm Griffioen
- **论文 PDF**: [From Mirai to Gorilla: Deep Dive into a Long-Lasting DDoS-for-Hire Botnet](https://www.usenix.org/system/files/usenixsecurity26-weyns.pdf)
- **解读**: Mirai 衍生僵尸网络已演变为商业化的 DDoS-for-Hire 平台。该文逆向分析 Mirai 系 Gorilla 僵尸网络（2024 秋至 2025 夏运营，寿命远长于同类）：研究其设计、工程决策与营销策略如何增强韧性与成功，包括支持的攻击类型、底层基础设施与 bot 行为；发现其长寿源于针对性改进（两个软件开发阶段、从先前版本吸取教训），并分析了其火力、攻击向量与目标的业务类型。


### LPG: Raise Your Location Privacy Game in Direct-to-Cell LEO Satellite Networks
- **作者**: Quan Shi, Liying Wang, Prosanta Gope, Qi Liang and Haowen Wang, Qirui Liu, Chenren Xu, Shangguang Wang and Qing Li, Biplab Sikdar
- **论文 PDF**: [LPG: Raise Your Location Privacy Game in Direct-to-Cell LEO Satellite Networks](https://www.usenix.org/system/files/usenixsecurity26-shi-quan.pdf)
- **解读**: 多租户直连手机（D2C）低轨卫星网络将移动网络运营商（MNO）管理的身份与卫星网络运营商（SNO）可见的位置关联，威胁用户位置隐私，而现有方案不适配受限硬件与轨道动力学。LPG 是首个协议层位置隐私方案，通过身份-位置解耦实现：SNO 提供连接但看不到用户身份，MNO 管理服务与计费但无法获得精确位置；支持不向卫星暴露身份的离线安全认证与密钥协商、按用户所选地理粒度披露位置以满足必要服务、以及隐私保护的 MNO-SNO 公平结算。在真实在轨 LEO 卫星与商用手机上实现，证明其在资源受限、高动态环境中的实用性。


### PrivacyShield: Relaying BLE Beacons to Counter Unsolicited Tracking
- **作者**: Florian Hofhammer, Daniele Antonioli, Mathias Payer
- **论文 PDF**: [PrivacyShield: Relaying BLE Beacons to Counter Unsolicited Tracking](https://www.usenix.org/system/files/usenixsecurity26-hofhammer.pdf)
- **解读**: Find My、Find My Device 等离线查找网络被滥用于跟踪受害者。该文分析（重点为 Apple Find My 与 Google Find My Device 的）反跟踪措施并展示绕过方法，提出 PrivacyShield 中继网络保护受害者：利用离线查找 BLE 信标未认证、可被中继到任意位置的特点，中继后的信标使第三方设备向查找网络报告错误位置，从而混淆受害者真实位置。实验证明其掩蔽标签位置的有效性及对抗反制尝试的鲁棒性，并向网络提供商提出改进跟踪保护的实际建议。


### Assessing LLM Response Quality in the Context of Technology-Facilitated Abuse
- **作者**: Vijay Prakash, Majed Almansoori, Donghan Hu, Rahul Chatterjee, Danny Yuxing Huang
- **论文 PDF**: [Assessing LLM Response Quality in the Context of Technology-Facilitated Abuse](https://www.usenix.org/system/files/usenixsecurity26-prakash.pdf)
- **解读**: 技术促成虐待（TFA）受害者常转向在线资源求助，而 LLM 聊天机器人正成为可及选项。该文首次进行专家主导的手动评估：用文献与在线论坛收集的真实问题、以幸存者安全为中心的提示，对四个 LLM（两个通用非推理模型、两个面向 IPV 的领域模型）的零样本单轮回答按 TFA 领域定制标准评估质量，并开展用户研究评估经历者感知的可操作性。专家评估与用户反馈共同揭示 LLM 在该场景的能力与局限，为未来模型设计、开发与微调提供建议，并给出改善幸存者支持的改进方向。


### Residual-PAC Privacy: Automatic Privacy Control Beyond the Gaussian Barrier
- **作者**: Tao Zhang and Yevgeniy Vorobeychik
- **论文 PDF**: [Residual-PAC Privacy: Automatic Privacy Control Beyond the Gaussian Barrier](https://www.usenix.org/system/files/usenixsecurity26-zhang-tao-residual.pdf)
- **解读**: PAC Privacy 框架的现有算法 Auto-PAC 依赖高斯互信息上界，但该上界仅当未扰动输出为高斯且噪声为独立高斯时才紧。该文先提出两种基于 Donsker–Varadhan 表示与 sliced Wasserstein 距离的可处理后处理方法，但仍有隐私预算浪费；更根本地，提出基于 f-散度的 Residual-PAC (R-PAC) 隐私度量对抗推断后残留的隐私量，并以 Stackelberg Residual-PAC (SR-PAC) 自动私有化算法实现——通过凸双层优化博弈选择最优噪声分布。SR-PAC 对任意数据分布高效利用隐私预算、多机制访问数据集时自然组合，实验显示隐私-效用权衡一致优于 PAC 与差分隐私基线。


### HAMLOCK: HArdware-Model LOgically Combined attacK
- **作者**: Sanskar Amgain, Daniel Lobo, Fnu Suya
- **论文 PDF**: [HAMLOCK: HArdware-Model LOgically Combined attacK](https://www.usenix.org/system/files/usenixsecurity26-amgain.pdf)
- **解读**: 模型级后门攻击将逐层激活路径完整嵌入模型，易被 SOTA 防御检测。HAMLOCK 将攻击逻辑分布到硬件-软件边界：软件侧仅微调少量神经元，使触发时产生独特高激活值；恶意硬件木马监测相应神经元的最高位或 8 位指数检测这些激活，再由另一木马直接操纵最终输出 logits 实现误分类。模型内不含完整后门激活路径、看似完全良性；在 MNIST、CIFAR10、GTSRB、ImageNet 上近完美攻击成功率且干净精度几乎无损，无需自适应优化即绕过 SOTA 模型级防御，硬件木马面积/功耗开销低至 0.01%，暴露了硬件-软件接口处的关键漏洞。


### Vεrity: Verifiable Local Differential Privacy
- **作者**: James Bell-Clark, Amrita Roy Chowdhury
- **论文 PDF**: [Vεrity: Verifiable Local Differential Privacy](https://www.usenix.org/system/files/usenixsecurity26-bell-clark.pdf)
- **解读**: LDP 机制易受投毒攻击：控制部分上报用户的对手能比非私有方案更严重地扭曲聚合输出。该文提出两个在保持 LDP 隐私保证的同时防投毒的新方案：Vεrity-Auth 面向用户上报输入且有第三方可用 ground truth 的场景；Vεrity 面向更难的场景——用户本地生成输入、无 ground truth 可引导可验证随机性生成。


### Missing, Present and Conflicting: A Large Scale Analysis of IoT Update Information in the EU Market
- **作者**: Swaathi Vetrivel
- **论文 PDF**: [Missing, Present and Conflicting: A Large Scale Analysis of IoT Update Information in the EU Market](https://www.usenix.org/system/files/usenixsecurity26-vetrivel.pdf)
- **解读**: 消费者缺乏 IoT 设备支持期限的可靠信息。这是首个欧盟市场更新期限披露的大规模研究，分析本地零售商、EU Amazon 与 Temu 上 34,187 个产品页：披露差异极大——受监管的荷兰零售商最多为 92% 设备列出更新期限，Amazon 不到 1%，Temu 为零；欧盟法规强制披露的智能电视覆盖较高但仍不一致。声明期限为 1 至 8 年不等（智能电视最长），而零售商、制造商与欧盟中央产品数据库之间普遍矛盾、零售商常低估支持期；结论是监管能提升可见性，但需强力执法与标准化披露机制才能保证信息准确可信。


### VSG-Safe: Spotting NSFW Video through Cross-Frame Evidence
- **作者**: Yuyang Zhang, Yihao Huang, Run Wang
- **论文 PDF**: [VSG-Safe: Spotting NSFW Video through Cross-Frame Evidence](https://www.usenix.org/system/files/usenixsecurity26-zhang-yuyang.pdf)
- **解读**: T2V 模型可生成色情、暴力等不当视觉内容，且此类语义常跨帧分布、嵌入实体、属性与实体间关系中，而现有审核管线把视频当作单帧或原始帧序列处理，无法跨帧推理。该文提出以场景图为核心中间语义表示（自然编码实体、属性与关系并支持跨帧推理），据此构建 VSG-Safe：先跨帧提取内容构建场景图，再用图模型联合捕捉实体、属性与关系完成检测。在 SOTA 基准与自建视频数据集上平均 F1 达 97.62%，平均超过七个基线 42.32%。


### Cutting the Gordian Knot: Detecting Malicious PyPI Packages via a Knowledge-Mining Framework
- **作者**: Wenbo Guo, Chengwei Liu, Ming Kang, Yiran Zhang and Jiahui Wu, Zhengzi Xu, Vinay Sachidananda and Yang Liu
- **论文 PDF**: [Cutting the Gordian Knot: Detecting Malicious PyPI Packages via a Knowledge-Mining Framework](https://www.usenix.org/system/files/usenixsecurity26-guo-wenbo.pdf)
- **解读**: 现有 PyPI 恶意包检测工具误报率 15-30%，根因是依赖简单句法规则而非语义理解，无法区分相同 API 调用的合法与恶意用途。PyGuard 是知识驱动框架，把检测失败转化为行为知识：层次化模式挖掘提取区分恶意与良性代码的行为序列，LLM 生成超越语法变异的语义抽象，最终将精确模式匹配与上下文推理合并进检测系统。PyGuard 准确率 99.50%（仅 2 个假阳性，现有工具为 1,927-2,117 个），混淆代码上保持 98.28%，真实部署中发现 219 个此前未知恶意包；行为模式在 NPM 上以 98.07% 准确率表现跨生态适用性，表明语义理解可实现跨语言知识迁移。


### End-to-End Encrypted Collaborative Documents
- **作者**: Christian Knabenhans, Zayd Maradni, Carmela Troncoso
- **论文 PDF**: [End-to-End Encrypted Collaborative Documents](https://www.usenix.org/system/files/usenixsecurity26-knabenhans.pdf)
- **解读**: 协同文档常含敏感数据，但端到端加密此前主要限于消息场景。该文提炼并形式化端到端加密协同文档（E2EE-CD）的安全与功能需求，提出通用框架：组合端到端加密异步广播信道与任何保证文档全局一致视图的编辑对账机制，并形式化证明 E2EE-CD 的安全性可归约到底层 E2EE 通信信道的安全性。针对调查记者场景进一步设计基于 Signal 群组消息协议的 SignalCD，分析其安全保证、实现原型并实证其效率足以支持实时协作。


### SophOMR: Improved Oblivious Message Retrieval from SIMD-Aware Homomorphic Compression
- **作者**: Keewoo Lee, Yongdong Yeo
- **论文 PDF**: [SophOMR: Improved Oblivious Message Retrieval from SIMD-Aware Homomorphic Compression](https://www.usenix.org/system/files/usenixsecurity26-lee-keewoo.pdf)
- **解读**: 保证接收者隐私的区块链与私密消息服务要求客户端扫描公共公告板上每个载荷，Oblivious Message Retrieval（OMR）用同态加密把这一昂贵扫描外包给服务商。新方案相对 SOTA 的 PerfOMR（USENIX Security'24）在 65536 个载荷（每个 612 字节、最多 50 个相关）场景下运行时降 3.4 倍、摘要大小降 2.2 倍、密钥大小降 1.5 倍；核心是新的同态压缩机制——密文长度与总载荷数成正比，压缩为长度与相关载荷上界成正比的摘要，并充分利用底层 HE 方案的 SIMD 结构（压缩阶段比 PerfOMR 快 7.5 倍）。


### FABS: Fast Attribute-Based Signatures
- **作者**: Liqun Chen, Mark Manulis
- **论文 PDF**: [FABS: Fast Attribute-Based Signatures](https://www.usenix.org/system/files/usenixsecurity26-chen-liqun.pdf)
- **解读**: 属性基签名（ABS）需要细粒度签名授权控制。该文提出一对快速方案：KP-ABS 与 SP-ABS，均支持 Monotone Span Program（MSP）表达性策略、大宇宙、任意属性与自适应安全；首次实现 MSP 基 ABS，渐近上密钥生成、签名与验证时间随属性数线性、验证仅需两次配对。100 个属性时 KP-ABS 的密钥生成/签名/验证分别为 0.16s/0.10s/0.13s，SP-ABS 为 0.082s/0.26s/0.21s，取得该领域已知最佳渐近与具体性能。


### Invariant-Guided Logical Testing of Open RAN Controllers
- **作者**: Tianchang Yang
- **论文 PDF**: [Invariant-Guided Logical Testing of Open RAN Controllers](https://www.usenix.org/system/files/usenixsecurity26-yang-tianchang.pdf)
- **解读**: O-RAN 组件语义正确性验证因系统复杂、实现多样且缺乏显式正确性预言而困难。InvaRAN 用动态推断的程序不变量作为期望行为的代理：将不变量按对程序逻辑的影响分为关键与非关键类别以减少假阳性，并超越传统模板式不变量推断，捕捉跨执行轨迹的变量间相关性以发现更富表达力的语义关联。在两个生产级 O-RAN 控制器的平台组件与 xApp 上发现九个此前未知问题（七个逻辑、两个内存漏洞），证明不变量引导测试能暴露 O-RAN 中规范未述的微妙缺陷。


### Analyzing the WebRTC Ecosystem and Breaking Authentication in DTLS-SRTP
- **作者**: Martin Bach, Vukašin Karadžić, Lukas Knittel, Robert Merget and Jean Paul Degabriele
- **论文 PDF**: [Analyzing the WebRTC Ecosystem and Breaking Authentication in DTLS-SRTP](https://www.usenix.org/system/files/usenixsecurity26-bach.pdf)
- **解读**: DTLS-SRTP 保护 Zoom、Teams、Google Meet 等实时媒体通信，但 WebRTC 融合 HTTP、TLS、SDP、ICE、STUN、TURN、DTLS、SCTP 等众多技术，复杂度使其安全部署状态基本未被探索。该文开发自动化 MitM 测试框架 DMS 测试 DTLS-SRTP 连接的 DTLS 信道，在 24 家服务商的浏览器与移动应用案例中测试认证机制、覆盖 19 个潜在认证绕过漏洞：33 个媒体服务器实现中 19 个存在可破坏 DTLS 层认证的漏洞，其中 9 个服务数亿用户的系统在仅具 MitM 能力时即可被利用检索媒体数据；并构建 PoC 窃听 Webex 视频会议。


### Security and Privacy Analysis of Tile's Location Tracking Protocol
- **作者**: Akshaya Kumar
- **论文 PDF**: [Security and Privacy Analysis of Tile's Location Tracking Protocol](https://www.usenix.org/system/files/usenixsecurity26-kumar-akshaya.pdf)
- **解读**: 首次对 Tile（仅次于 AirTags 的第二大众包位置跟踪服务）的全面安全分析，推翻其多项宣称：Tile 服务器可持久获知所有用户与标签位置，无特权对手可通过 Tile 设备蓝牙广告跟踪用户，防盗模式易被颠覆。Tile 无协议或威胁模型的形式化描述，且故意削弱反跟踪特性以支持防盗用例，依赖新颖的"问责"机制惩罚滥用者；该机制可被颠覆并引入新的可利用漏洞。作者呼吁为该场景建立正式的问责定义。


### VIPER Strike: Defeating Visual Reasoning CAPTCHAs via Structured Vision–Language Inference
- **作者**: Minfeng Qi and Dongyang He, Qin Wang, Lefeng Zhang
- **论文 PDF**: [VIPER Strike: Defeating Visual Reasoning CAPTCHAs via Structured Vision–Language Inference](https://www.usenix.org/system/files/usenixsecurity26-qi-minfeng.pdf)
- **解读**: 视觉推理验证码（VRC）结合场景与自然语言查询，现有求解器分为依赖模板检测器的 vision-centric 范式（新布局失效）与依赖 LLM 的 reasoning-centric 范式（细粒度视觉感知差）。VIPER 是统一攻击框架，以模块化管线整合结构化多目标视觉感知与自适应 LLM 推理：解析视觉布局、将属性接地到问题语义、推断目标坐标。在六个主流 VRC 提供商（VTT、Geetest、NetEase、Dingxiang、Shumei、Xiaodun）上成功率最高 93.2%，多数基准超过人类；跨 GPT、Grok、DeepSeek、Kimi 等 LLM 骨干保持 90% 以上。为应对防御提出不改变任务语义的 Template-Space Randomization (TSR) 轻量策略，可有效降低求解器性能，为设计人类可解、机器难抗的验证码提供方向。


### Stayin' Alive: How Global Stolen Data Markets Thrive on Telegram
- **作者**: Tina Marjanov, Taro Tsuchiya, Konstantinos Ioannidis and Jack Hughes, Nicolas Christin, Alice Hutchings
- **论文 PDF**: [Stayin' Alive: How Global Stolen Data Markets Thrive on Telegram](https://www.usenix.org/system/files/usenixsecurity26-marjanov.pdf)
- **解读**: Telegram 成为被盗数据交易的新兴平台。一年纵向研究中人工整理 1,521 个频道、收集 1,400 万条消息与 360 万个共享文件：被盗数据社区与 Telegram 其他社区大体不相交；按数据类型分类以理解其促成的网络犯罪；发现许多频道以非英语运营并从非英语市场获取数据；社区部署多种规避监管的技术，其中提供其他频道链接的"门户频道"对频道寿命与增长起关键作用。为研究者、Telegram 及各辖区执法机构的监测与监管提供启示。


### Assumption-Free Fuzzy PSI via Predicate Encryption
- **作者**: Erik-Oliver Blass, Guevara Noubir
- **论文 PDF**: [Assumption-Free Fuzzy PSI via Predicate Encryption](https://www.usenix.org/system/files/usenixsecurity26-blass.pdf)
- **解读**: 现有模糊私有集合交集（PSI）协议要么依赖输入分布假设、要么使用低效的 FHE。该文提出首个实现线性通信复杂度的模糊 PSI 协议：双方可计算集合间 Hamming 距离在阈值内的所有元素对，不限制集合结构。关键洞察是把（阈值）Hamming 距离的安全计算归约为内积安全计算，从而用内积谓词加密构造；证明谓词加密仅需弱模拟安全，并展示其内部密钥派生可在无受信第三方下高效分布式化。实现验证了可行性且性能优于最相关工作。


### Logos: Robust Sharding Blockchain With Fast Processing and Optimal Cross-Shard Overhead
- **作者**: Yizhong Liu, Boyu Zhao, Yuan Lu, Song Bian
- **论文 PDF**: [Logos: Robust Sharding Blockchain With Fast Processing and Optimal Cross-Shard Overhead](https://www.usenix.org/system/files/usenixsecurity26-liu-yizhong.pdf)
- **解读**: 分片区块链的跨片交易处理（CSTP）受恶意节点无效交易洪泛的鲁棒性问题与高开销困扰。Logos 采用稳健的广播-传输-同意模式：每个输入分片仅调用新广播原语生成输入可用状态，经并行单对单传输机制送达相关分片，有效跨片交易经同意协议提交、无效者被丢弃；证明达到有效 CSTP 的最优片内开销与跨片最优传输开销。在 4 区域 1000 个 AWS-EC2 节点实验中，延迟为基线 Kronos（NDSS'25）的 50%、峰值吞吐 132.8k tx/s、跨片网络用量仅为 Kronos 的 1/210；恶意洪泛攻击下吞吐保持 Kronos 的 2.86 倍，鲁棒性强。


### SoK: PHILTER: Uncovering Security and Functional Gaps in AI-based Phishing Website Detection Literature via an LLM-based Reasoning Framework
- **作者**: Mahbub Alam, Muhammad Lutfor Rahman, Sonjoy Kumar Paul
- **论文 PDF**: [SoK: PHILTER: Uncovering Security and Functional Gaps in AI-based Phishing Website Detection Literature via an LLM-based Reasoning Framework](https://www.usenix.org/system/files/usenixsecurity26-alam.pdf)
- **解读**: 学术界 AI 钓鱼网站检测研究普遍报告高准确率，但能否满足真实世界需求（抵御演化策略、良性页面鲁棒性、可解释性、隐私）未知。PHILTER 是可扩展评估框架：LLM 提取证据并起草理由、专家验证并定稿，按四个功能与三个安全指标评估 55 个学术方法，揭示系统性缺口——没有研究同时满足全部功能与安全需求、没有研究有效应对多样化钓鱼策略、多数方法隐私保护差且难以适应演化攻击者、因良性页面测试有限而存在误报风险。作者引入策略分类学（特征基、相似性基、身份基、混合基）解释缺陷，指出准确率驱动的评估掩盖了削弱实际有效性的盲点。


### Streaming Function Secret Sharing and Its Applications
- **作者**: Xiangfu Song, Jianli Bai, Ye Dong, Yijian Liu, Tianwei Zhang
- **论文 PDF**: [Streaming Function Secret Sharing and Its Applications](https://www.usenix.org/system/files/usenixsecurity26-song-xiangfu.pdf)
- **解读**: 流式分析场景下 FSS 方案需反复对持续到来的消息执行安全计算，面临效率瓶颈。该文提出流式函数秘密共享（SFSS）新原语并形式化，给出点函数、谓词函数的构造与通用函数的可行性结果。SFSS 以简单模块化方式支撑条件转加密、策略隐藏聚合与属性隐藏聚合等应用，识别出现有方案的安全缺陷与效率瓶颈，SFSS 方案在渐近与具体效率、功能上均更优。


### Shred-to-Shine Metamorphosis of (Distributed) Polynomial Commitments
- **作者**: Weihan Li, Zongyang Zhang, Sherman S. M. Chow, Yanpei Guo, Boyuan Gao, Xuyang Song, Yi Deng, Jianwei Liu
- **论文 PDF**: [Shred-to-Shine Metamorphosis of (Distributed) Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-li-weihan.pdf)
- **解读**: SNARK 依赖多项式承诺（PCS），高性能多线性 PCS 需平衡证明者成本、证明大小与验证速度。PIPFRI 用快速 FRI 结合线性编码 PCS 的线性时间证明与 Reed-Solomon PCS 的紧凑证明/快速验证：比 RS 基 DeepFold（USENIX Security'25）证明快 10 倍且证明大小与验证时间相当，比线性编码的 Orion（CRYPTO'22）证明快 3.5 倍、证明大小与验证时间降 15 倍。分布式变体 DEPIPFRI 增加问责并将单个多项式分布到多个证明者，成为首个通用电路的基于编码的分布式 SNARK，比 DeVirgo（CCS'22）证明时间提升 25 倍、证明者间通信提升 7 倍；"shred-to-shine"洞察（把多项式切分为独立片段而保持证明大小与验证时间）在配对域还得到 SRS 短 16 倍、打开快 10 倍的群基多线性 PCS。


### Unbalanced Fuzzy Private Set Intersection for L_infinity Distance: Achieving Sublinear Communication with Large Set Size
- **作者**: Shengzhe Meng and Xiaodong Wang, Xv Zhou, Bei Liang
- **论文 PDF**: [Unbalanced Fuzzy Private Set Intersection for L_infinity Distance: Achieving Sublinear Communication with Large Set Size](https://www.usenix.org/system/files/usenixsecurity26-meng.pdf)
- **解读**: 现有模糊 PSI 协议面向集合大小相近的均衡场景，而非均衡场景下通信被 OKVS 传输主导。该文聚焦 l∞ 度量下的非均衡模糊 PSI：重新表述空间哈希并提出非交互与交互两种新方案，将稀疏 OKVS 与批量私有信息检索（BatchPIR）结合，实现通信随较大集合大小次线性增长的两个协议（分别适配接收者集合更大与发送者集合更大的场景）。100 Mbps 网络、参数 (N,M,d,σ)=(2^20,2^5,2,10) 时，基于非交互空间哈希的协议把通信从 16,128 MB（Baarsen and Pu, Eurocrypto'24）降至 0.35 MB；交互版相比 Gao et al.（Asiacrypt'25）在线运行时至少快 31 倍、通信低 1762 倍；带发送者隐私版本比 Piske et al.（CCS'25）在线运行时至少快 4 倍、通信低 6 倍。


### mmCipher: Batching Post-Quantum Public Key Encryption Made Bandwidth-Optimal
- **作者**: Hongxiao Wang, Ron Steinfeld, Markku-Juhani O. Saarinen, Muhammed F. Esgin, Siu-Ming Yiu
- **论文 PDF**: [mmCipher: Batching Post-Quantum Public Key Encryption Made Bandwidth-Optimal](https://www.usenix.org/system/files/usenixsecurity26-wang-hongxiao.pdf)
- **解读**: 批量向多个接收者投递消息的多消息多接收者公钥加密（mmPKE）在后量子设置下价值更大（密文远大于明文），但几乎所有先前工作基于抗量子弱的传统假设。该文提出首个基于标准 Module-LWE 格假设的 CPA 安全 mmPKE 与多密钥封装（mmKEM）mmCipher-PKE/mmCipher-KEM：先提出扩展可复现 PKE（XR-PKE）新变体，经额外提示可复现密文，由此给出 mmPKE 的通用构造；再实例化格基 XR-PKE，其新技术可精确估计提示对密文安全的影响并确定参数。1024 个接收者时 mmCipher-KEM 带宽开销降低 23–45 倍、密文仅比明文大 4–9%（近最优带宽），计算成本降低 3–5 倍；另探索抵抗自适应腐化与选择密文攻击的自适应安全 mmPKE 通用构造。


### DNS Cache Poisoning Like it's 2006
- **作者**: Omer Ben-Simhon and Amit Klein
- **论文 PDF**: [DNS Cache Poisoning Like it's 2006](https://www.usenix.org/system/files/usenixsecurity26-ben-simhon.pdf)
- **解读**: 针对最广泛部署的开源 DNS 解析器 BIND9 的新型缓存投毒攻击研究：可靠预测 UDP 源端口与 TXID 两个关键挑战参数（多数先前工作只针对其一），且完全从客户端侧预测（无需攻击者域权威服务器，据作者所知属首次）；利用 BIND 伪随机数生成的弱点，在现实网络条件下实现高可靠预测，并为 BIND 9 的 9.18 老分支开发服务端技术。跨多个 BIND 9 分支与配置评估展示实际成功率，漏洞已负责任披露给 ISC 与 FreeBSD 项目，产生两个补丁与 CVE 并获致谢。


### UncoreBleed: AEX-Free, High-Resolution, and Low-Noise Side-Channel Attacks on SGX Enclaved Execution
- **作者**: Decheng Chen, Zhi Zhang, Zhenkai Zhang, Xin Zhang, Yansong Gao, Yi Zou
- **论文 PDF**: [UncoreBleed: AEX-Free, High-Resolution, and Low-Noise Side-Channel Attacks on SGX Enclaved Execution](https://www.usenix.org/system/files/usenixsecurity26-chen-decheng.pdf)
- **解读**: 先前工作声称 SGX 禁用 PMC 以缓解侧信道，但现代处理器的非核（uncore）PMC 在 SGX 下的行为未被充分评估。该文推翻"SGX 抑制性能监控"的长期信念：非核 PMC 记录与 enclave 执行相关的事件；识别 mesh-to-memory 非核子系统中的关键事件，支持 64 字节粒度的基于地址监控，并通过逆向揭示其过滤机制、可编程性、可用性及在 SGX 兼容 Xeon 处理器上的地址映射。据此提出 UncoreBleed——首个基于 PMC、无 AEX、高分辨率、低噪声的 SGX 侧信道攻击：可从 enclaved Libjpeg 重建图片、单次解密提取 RSA 私钥，且对抗带 AEX-Notify 的 TLBlur 这一最先进软件防御仍有效，表明活跃的非核 PMC 对 enclave 机密性是此前被低估的威胁。


### M-Step: A Single-Stepping Framework for Side-Channel Analysis on TrustZone-M
- **作者**: Cristiano Rodrigues, Marton Bognar, Sandro Pinto, Jo Van Bulck
- **论文 PDF**: [M-Step: A Single-Stepping Framework for Side-Channel Analysis on TrustZone-M](https://www.usenix.org/system/files/usenixsecurity26-rodrigues.pdf)
- **解读**: TrustZone-M 等资源受限 TEE 的高分辨率软件侧信道分析此前基本未被探索。M-Step 是开放可扩展的 TrustZone-M 单步执行攻击框架：Cortex-M 无虚拟内存与页表、且中断行为独特（某些多周期指令被放弃或暂停），实现精确指令级单步困难；作者大量剖析中断处理 CPU 行为，利用此前被忽视的中断延迟泄漏动态调整定时器中断。在最新 Arm Mbed TLS 中发现此前未知漏洞，实现单迹线确定性攻击从 TrustZone enclave 恢复完整 RSA 密钥，展示了更高的分辨率与实用性。


### DaLens: Charting DNS Self-Amplification Threats at Large
- **作者**: Liwen Xu and Zechao Cai, Huayi Duan, Adrian Perrig
- **论文 PDF**: [DaLens: Charting DNS Self-Amplification Threats at Large](https://www.usenix.org/system/files/usenixsecurity26-xu-liwen.pdf)
- **解读**: DNS 自放大攻击（SAA）能以极小成本耗尽递归与权威服务器资源，但其全球影响此前未被量化。DaLens 是模块化、可扩展、健全的大规模测量框架，梳理基础设施、识别有效放大器并量化其放大能力：30.7 万个持久公共解析器中找到 2.9 万个可被并行利用的唯一解析器集群，且其中相当数量的集群即使漏洞已被先前工作披露仍能产生大放大效应，为 DNS 运营商处置威胁与部署防御提供依据。


### Inconsistent, Incomplete, and Insecure: A Survey of Account Security Interfaces
- **作者**: Arkaprabha Bhattacharya and Alaa Daffalla, Kevin Lee, Rosanna Bellini, Nicola Dell, Thomas Ristenpart
- **论文 PDF**: [Inconsistent, Incomplete, and Insecure: A Survey of Account Security Interfaces](https://www.usenix.org/system/files/usenixsecurity26-bhattacharya.pdf)
- **解读**: 账户安全接口（ASI）——通知与日志——帮助用户识别未授权访问，但现状不明。这是迄今最大规模的 ASI 测量，评估 100 个流行服务：29 个服务无法让用户区分账户访问；用新类型学分类后发现服务部署的 ASI 类型不一致、常不完整且即使专家研究者也感到困惑；61 个提供设备/位置描述的 ASI 中 41 个（67.2%）易受欺骗攻击，可成功混淆访问来源。基于发现提出改善未来 ASI 部署的六条原则。


### Hydrangea: Optimistic Two-Round Partial Synchrony with Improved Fault Resilience
- **作者**: Nibesh Shrestha, Aniket Kate, Kartik Nayak
- **论文 PDF**: [Hydrangea: Optimistic Two-Round Partial Synchrony with Improved Fault Resilience](https://www.usenix.org/system/files/usenixsecurity26-shrestha.pdf)
- **解读**: 部分同步共识面临权衡：最优拜占庭容错需至少三轮良好情况延迟，而少于三轮通常以降低容错为代价。Hydrangea 是结合低延迟与改进容错的 BFT 状态机复制协议：n=3f+2c+k+1 的系统中，当故障总数（拜占庭或崩溃）不超过 p=c+⌊k/2⌋ 时取得两轮乐观良好情况延迟；在最多 f 个拜占庭与 c 个崩溃的更强敌意设置下保证三轮延迟；并证明匹配下界——p>c+⌊k/2⌋ 时任何协议都无法实现两轮乐观提交。地理分布式部署实验显示其在纯拜占庭与拜占庭-崩溃两种模型下延迟均显著低于 SOTA，吞吐也有适度提升。


### Revealing the Dark Side of Smart Accounts: An Empirical Study of EIP-7702 Incurred Risks in Blockchain Ecosystem
- **作者**: Mingyuan Huang, Han Liu, Shuo Yang, Daoyuan Wu, Shuai Wang
- **论文 PDF**: [Revealing the Dark Side of Smart Accounts: An Empirical Study of EIP-7702 Incurred Risks in Blockchain Ecosystem](https://www.usenix.org/system/files/usenixsecurity26-huang-mingyuan.pdf)
- **解读**: EIP-7702 智能账户让外部账户（EOA）升级为可编程账户且保留原地址，重定义了区块链信任边界并带来新型攻击机会。该文按受害账户类型把智能账户风险分为 EOA 定向、CA 定向与复合攻击三类，开发结合大规模交易分析与跨合约静态分析的专用检测工具，在七个支持 EIP-7702 的链上检测到 924 个恶意合约账户（含多个此前未报告的零日），造成超 230 万美元损失、超 1000 万账户面临潜在危害。关键发现：超 63% 的 EIP-7702 授权交易与恶意 EOA 定向攻击相关，近半数最常被授权的合约由攻击者控制；还识别出攻击者的规避战术、真实事件影响与未来部署的潜在风险。


### DMGuard: Safeguarding Kernels from Physical-Page Use-After-Free Vulnerabilities
- **作者**: Juhee Kim
- **论文 PDF**: [DMGuard: Safeguarding Kernels from Physical-Page Use-After-Free Vulnerabilities](https://www.usenix.org/system/files/usenixsecurity26-kim-juhee-dmguard.pdf)
- **解读**: 现代内核依赖页表完整性实施安全防御，攻击者转而攻击页表本身；异构地址翻译域（CPU、GPU、IOMMU 各自页表）加剧同步与一致性负担。当虚拟地址仍映射到已释放或重分配的物理页时，攻击者可访问任意物理内存——作者称之为物理页 use-after-free，区别于传统基于虚拟地址的堆 use-after-free。DMGuard 是首个全面处理该问题的运行时缓解：以轻量无锁机制维护物理页状态机，确保页表中不存在悬空映射。在 Android 设备上评估，阻断所有已知物理页 use-after-free 漏洞且性能开销可忽略。


### Garuda and Pari: Faster and Smaller SNARKs via Equifficient Polynomial Commitments
- **作者**: Michel Dellepere, Pratyush Mishra and Alireza Shirzad
- **论文 PDF**: [Garuda and Pari: Faster and Smaller SNARKs via Equifficient Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-dellepere.pdf)
- **解读**: SNARK 研究的两大目标是极小化证明大小与生成时间。Pari 达到已知最小证明大小：仅 2 个群元素与 2 个域元素（BLS12-381 实例化下共 160 字节），小于 Groth16 与 Polymath，且链上验证 gas 成本已知最低（比 Groth16 低 6%、比 FFLONK 低 17%）；Garuda 首次支持任意"自定义"门与密码学成本为零的线性门，显著节省证明生成时间。两者依赖新的"equifficient"多项式承诺（EPC）原语——强制承诺多项式在特定基中表示相同，作者给出严格安全定义及单变量与多线性高效构造，并通过把多项式 IOP 与 EPC 组合的编译器得到简洁论证。


### Transparent Dictionaries from Polynomial Commitments
- **作者**: Hossein Hafezi, Alireza Shirzad, Benedikt Bünz and Joseph Bonneau
- **论文 PDF**: [Transparent Dictionaries from Polynomial Commitments](https://www.usenix.org/system/files/usenixsecurity26-hafezi.pdf)
- **解读**: 透明字典让不可信服务器维护可变字典并证明性地服务客户端查询，此前方案要么服务器成本高（限吞吐）、要么客户端查询验证成本高，难以支撑数十亿用户的密钥透明部署。IRONDICT 黑盒使用通用多线性多项式承诺并继承其绑定与零知识安全：用 KZH 方案实现，10 亿条目的字典在消费级笔记本上 35 毫秒验证（比 SOTA 提升 300 倍），证明小 150,000 倍（8 KB）且完全隐私，客户端与服务器成本具体高效；还展示基于增量可验证计算（IVC）与检查点的 fast-forwarding 技术以进一步加速客户端审计。


### Efficient Threshold ML-DSA
- **作者**: Sofía Celi, Rafael del Pino and Thomas Espitau, Guilhem Niot, Thomas Prest
- **论文 PDF**: [Efficient Threshold ML-DSA](https://www.usenix.org/system/files/usenixsecurity26-celi.pdf)
- **解读**: 后量子格基阈值签名在可扩展性、鲁棒性或与标准化方案兼容性上常遇挑战。该文提出首个与 NIST 标准化的 ML-DSA 完全兼容的阈值签名方案，支持最多 6 方安全高效签名：利用高级短秘密共享技术并整合优化拒绝采样，在分布式环境中平衡通信效率与正确性。Go 实现并在本地、LAN 与 WAN 网络评估，显示其不仅实际可部署，还适合多设备加密货币钱包、阈值 TLS 认证与 Tor 目录权威等真实应用。


### Nudge: A Private Recommendations Engine
- **作者**: Alexandra Henzinger, Emma Dauterman, Henry Corrigan-Gibbs, Dan Boneh
- **论文 PDF**: [Nudge: A Private Recommendations Engine](https://www.usenix.org/system/files/usenixsecurity26-henzinger.pdf)
- **解读**: Nudge 是三服务器推荐系统：周期性以秘密共享形式收集用户评分，运行三方计算在私有评分上训练轻量推荐模型，再向用户交付个性化推荐；每一步对服务器都不泄露任何用户偏好（除聚合模型本身），隐私对抗能完全攻陷一台服务器秘密状态的对手成立。技术核心是新的三方矩阵分解协议：Netflix 数据集（50 万用户、1 万条目）上三台 192 核服务器局域网内 50 分钟、40 GB 服务器间通信完成私有训练，nDCG@20 达 0.29，与无隐私矩阵分解相当、略逊于非私有神经推荐器（0.31）。


### On Evaluating the Robustness of Large Vision-Language Models via Untargeted Modality Alignment Breaking Adversarial Attack
- **作者**: Zhichao Li, Yaopeng Wang, Kui Ren and Chun Chen
- **论文 PDF**: [On Evaluating the Robustness of Large Vision-Language Models via Untargeted Modality Alignment Breaking Adversarial Attack](https://www.usenix.org/system/files/usenixsecurity26-li-zhichao.pdf)
- **解读**: 大视觉语言模型（LVLM）黑盒鲁棒性评估现有方法只扰动视觉编码器且忽视无目标攻击场景。MABA 是可迁移无目标对抗攻击：揭示可迁移对抗攻击的核心在于抑制判别性视觉表征，并将其显式作为优化目标提升跨 LVLM 迁移性；引入互信息感知投影器作为 LVLM 模态对齐模块的替身，打破跨模态一致性进一步增强迁移性。图像描述任务上语义指标平均下降 58.37%，取得 SOTA 性能；跨 LVLM 家族的消融研究为强化 LVLM 鲁棒性提供洞见。


### From Easy to Hard++: Promoting Differentially Private Image Synthesis Through Spatial-Frequency Curriculum
- **作者**: Chen GONG and Kecen Li, Zinan Lin, Tianhao Wang
- **论文 PDF**: [From Easy to Hard++: Promoting Differentially Private Image Synthesis Through Spatial-Frequency Curriculum](https://www.usenix.org/system/files/usenixsecurity26-gong-easy-hard.pdf)
- **解读**: DP 合成图像质量提升多聚焦于优化技术本身。作者观察到"中心图像"预热只适用于存在大量相似样本的数据集，据此提出 FETA-Pro：引入频率特征作为"训练捷径"（其复杂度介于空间特征与完整图像之间，形成更细粒度的课程），并利用生成模型的管线式生成属性——辅助生成器产出与噪声频率特征对齐的图像，另一模型用这些图像连同空间特征与 DP-SGD 训练，避免单一模型多特征训练的失配。五个敏感图像数据集、隐私预算 ε=1 下，保真度平均比最佳基线高 25.7%、效用高 4.1%。


### FIRA: Enabling Automatic Forensic Investigation of Unmanned Aerial Vehicles
- **作者**: Yizhi Huang, David Oygenblik, Runze Zhang
- **论文 PDF**: [FIRA: Enabling Automatic Forensic Investigation of Unmanned Aerial Vehicles](https://www.usenix.org/system/files/usenixsecurity26-huang-yizhi.pdf)
- **解读**: 无人机在动态环境中用在线学习调整模型决策边界，但坠毁后取证者无法判断在线学习是否为肇因。FIRA 是新型取证技术：回传飞行中在线学习更新与遥测数据（即使带宽受限），确定崩溃能否归因于在线学习模型。在 48 个无人机崩溃场景（两个广泛采用的飞控 PX4 与 ArduPilot、四类任务、每类 12 个由被植入后门的在线学习模型致因的事故）中，FIRA 以 95.8% 的准确率正确归因。


### MASLeak: Investigating and Exposing Intellectual Property Leakage Vulnerabilities in Multi-Agent Systems
- **作者**: Liwen Wang, Wenxuan Wang, Shuai Wang, Daoyuan Wu, Shing-Chi Cheung
- **论文 PDF**: [MASLeak: Investigating and Exposing Intellectual Property Leakage Vulnerabilities in Multi-Agent Systems](https://www.usenix.org/system/files/usenixsecurity26-wang-liwen.pdf)
- **解读**: 多智能体系统（MAS）的架构、代理交互与内部通信带来知识产权（IP）泄露担忧。MASLEAK 是首个在实用黑盒设置下系统化提取 MAS IP 的框架：对手仅能查询系统公共 API 并观察最终输出、无内部架构与后端 LLM 先验知识。受计算机蠕虫传播启发，MASLEAK 精心构造对抗查询以引发、传播并保留各代理回应，揭示完整专有组件（代理数量、拓扑、系统提示、任务指令与工具使用）。在自建 810 个 MAS 应用数据集及 Coze、CrewAI 等真实应用上，系统提示与任务指令平均攻击成功率 87%、系统架构在多数情况下达 92%。


### Love, Lies, and Language Models: Investigating AI's Role in Romance-Baiting Scams
- **作者**: Gilad Gressel and Rahul Pankajakshan, Shir Rozenfeld, Ling Li, Ivan Franceschini, Krishnashree Achuthan, Yisroel Mirsky
- **论文 PDF**: [Love, Lies, and Language Models: Investigating AI's Role in Romance-Baiting Scams](https://www.usenix.org/system/files/usenixsecurity26-gressel.pdf)
- **解读**: 爱情诱饵诈骗由有组织犯罪集团运营、以文本对话建立情感联系后诱导加密货币投资，引发 LLM 在自动化中的角色之问。作者访谈 145 名内部人士与 5 名受害者、开展对比 LLM 诈骗代理与人类运营者的盲法长期对话研究、并评估商业安全过滤器：LLM 已在诈骗组织广泛部署，87% 的诈骗劳动力由易自动化的系统化对话任务组成；一周研究中 LLM 代理获得参与者更高信任（p=0.007）且请求遵从率更高（46% 对人类的 18%）；流行安全过滤器对爱情诱饵对话检测率为 0.0%。结论是此类诈骗可能实现全规模 LLM 自动化而现有防御无法阻止其扩张。


### United We Defend: Collaborative Membership Inference Defenses in Federated Learning
- **作者**: Li Bai, Junxu Liu, Sen Zhang, Haibo Hu
- **论文 PDF**: [United We Defend: Collaborative Membership Inference Defenses in Federated Learning](https://www.usenix.org/system/files/usenixsecurity26-bai.pdf)
- **解读**: 联邦学习中轨迹型 MIA 利用训练全程的时序信息，使逐客户端独立施加的现有防御失效。该文研究由异构隐私需求驱动的新防御场景（仅部分客户端被防御）与协作防御模式，提出 CoFedMID 协作防御框架：限制本地模型对训练样本的记忆，并通过防御者联盟增强隐私保护与模型效用。三个模块分别为类引导分区（选择性本地训练样本）、效用感知补偿（回收有贡献样本并防止其过度自信）与聚合中立扰动（联盟级注入噪声并在聚合中相互抵消）。三个数据集上显著降低七个 MIA 的性能，仅付出较小效用损失，且在各防御设置下结果一致。


### Tracegram: Framing Trace-Level Traffic Analysis with Temporally-Aware Multiple Instance Learning
- **作者**: Jian Qu, Xiaobo Ma
- **论文 PDF**: [Tracegram: Framing Trace-Level Traffic Analysis with Temporally-Aware Multiple Instance Learning](https://www.usenix.org/system/files/usenixsecurity26-qu.pdf)
- **解读**: 现代网络行为跨多个流并随时间演化，攻击在侦察、投递、命令控制、横向移动等阶段跨流展开，而现有包级或单流方法割裂上下文。Tracegram 以 trace 为分析单元、把 trace 级分析表述为多实例学习（MIL）：每流编码器结合时序感知聚合模块跨流推理、保持长程依赖，并产生关键流归因信号支持分析者验证与取证。作者从理论上论证 MIL 分解的合理性，并在四个公共数据集的多任务实验中达到优于或相当 SOTA 的性能；DAPT 数据集的 APT 迹线案例显示 Tracegram 高亮与攻击阶段对齐的流，支持定向调查。


### "Abuse Risks are Often Inherent to Product Features": Exploring AI Vendors' Bug Bounty and Responsible Disclosure Policies
- **作者**: Yangheran Piao
- **论文 PDF**: ["Abuse Risks are Often Inherent to Product Features": Exploring AI Vendors' Bug Bounty and Responsible Disclosure Policies](https://www.usenix.org/system/files/usenixsecurity26-piao.pdf)
- **解读**: AI 漏洞的发现与修复依赖厂商接收并奖励漏洞报告，但现状未知。该文分析 264 家 AI 厂商的漏洞披露政策，混合快照与纵向定性分析，并与 320 起 AI 事件和 260 篇学术文章对齐：36% 的厂商没有任何政策，仅 18% 提及 AI 风险；数据访问、授权与模型提取漏洞最常被声明在范围内，越狱与幻觉最常被声明为范围外。识别出三个画像——主动澄清（n=46）、沉默（n=115）、限制（n=103）；对齐结果表明厂商处理 AI 漏洞披露可能晚于学术研究与真实事件。


### PICS: Private Intersection over Committed (and reusable) Sets
- **作者**: Aarushi Goel, Peihan Miao and Phuoc Van Long Pham, Satvinder Singh
- **论文 PDF**: [PICS: Private Intersection over Committed (and reusable) Sets](https://www.usenix.org/system/files/usenixsecurity26-goel.pdf)
- **解读**: 恶意安全 PSI 仍允许对手跨会话使用不一致输入，这源于恶意安全定义，但在 CSAM 检测、私密联系人发现等需要多次一致性 PSI 执行的应用中尤其有害。PICS 框架通过承诺集合强制跨会话输入一致性：基于 SOTA 恶意安全 VOLE-PSI（EUROCRYPT 2021）用轻量密码工具高效实例化，实现强接收者侧一致性（接收者使用精确承诺集合）与弱发送者侧一致性（发送者不能注入新元素但可使用承诺集合的子集）。实现显示通信开销仅为 VOLE-PSI 的 1.57–2.04 倍（集合大小 2^16–2^24），端到端运行时间开销为 1.22–1.98 倍。


### MULCOTAINT: Towards Efficient Multi-tag Dynamic Taint Analysis via Hardware/Software Co-design
- **作者**: Bing Qi, Yi Yang and Xiangkun Jia, Zhengpin Qian and Huafeng Huang, Purui Su
- **论文 PDF**: [MULCOTAINT: Towards Efficient Multi-tag Dynamic Taint Analysis via Hardware/Software Co-design](https://www.usenix.org/system/files/usenixsecurity26-qi-bing.pdf)
- **解读**: 多标签动态污点分析（M-DTA）对漏洞分析等细粒度场景关键，但软件方案性能差、硬件方案仅单标签难扩展。MULCOTAINT 以硬件/软件协同设计实现高效 M-DTA：协处理器架构把污点分析与正常执行解耦，解决污点计算向量化、页表管理污点标签与污点引擎功能接口等挑战。32 个程序、5 类数据集上的性能与漏洞分析实验显示其高性能、可接受内存并支持细致漏洞分析，全面优于软件方案（TaintRabbit、PANDA）与硬件方案（HardTaint、RAFT、FineDIFT）——开销增幅对比 MULCOTAINT vs PANDA 为 1.14x vs 4409.09x，HardTaint 平均开销是 MULCOTAINT 的 19.57 倍；硬件成本高于嵌入式导向的 RAFT/FineDIFT 但相对 M-DTA 的复杂逻辑可接受。


### WAVED: Principled Identification of Off-Path Exploitable Weak Verifications within the TCP/IP Protocol Suite
- **作者**: Yizhou Zhao and Xuewei Feng, Min Li, Ke Xu
- **论文 PDF**: [WAVED: Principled Identification of Off-Path Exploitable Weak Verifications within the TCP/IP Protocol Suite](https://www.usenix.org/system/files/usenixsecurity26-zhao-yizhou.pdf)
- **解读**: TCP/IP 协议族中接收载荷的弱验证（缺乏可靠校验信息或实现缺陷所致）导致离路径可利用漏洞。WAVED 是首个系统化识别框架：面向 TCP/IP 内核开发流、上下文、字段敏感的指针分析并构建污点传播图（TPG）建模栈内数据流；字节粒度污点传播跨多种算术运算精确定位每个约束对应的输入字节，方向敏感污点信息准确捕捉替代分支结果施加的约束强弱，优于传统字节不敏感与方向不敏感分析。在 Linux 5.15、Linux 6.8 与 FreeBSD 14.1 的 IPv4/IPv6 上精确揭示导致 TCP/IP 语义漏洞的弱验证，发现 14 个此前未知漏洞并已向 OS 厂商负责任披露、获 Linux 社区致谢。


### CuSafe: Capturing Memory Corruption on NVIDIA GPUs
- **作者**: Hongyi Lu, Fengwei Zhang, Zhenkai Zhang, Shuai Wang, Yanan Guo
- **论文 PDF**: [CuSafe: Capturing Memory Corruption on NVIDIA GPUs](https://www.usenix.org/system/files/usenixsecurity26-lu-hongyi.pdf)
- **解读**: GPU 应用（尤其 ML 与科学计算）依赖 C/C++ 等内存不安全语言，但现有方案或依赖商品 GPU 不具备的软硬件、或开销过高。CuSafe 是可直接部署于商品 NVIDIA GPU 的 sanitizer：混合元数据方案结合指针标记与带内缓冲区边界实现准确高效的内存安全验证，并引入栈 epoch 跟踪与虚拟地址随机化缓解时序损坏导致的元数据混淆。33 个程序的安全评估显示其对空间与时间 bug 的覆盖在现有 GPU sanitizer 中最佳；44 个程序（含 LLaMA2-7B、LLaMA3-8B）基准显示平均仅 13% 减速与 0.3% 的可忽略内存开销。


### Attacks on Approximate Caches in Text-to-Image Diffusion Models
- **作者**: Desen Sun
- **论文 PDF**: [Attacks on Approximate Caches in Text-to-Image Diffusion Models](https://www.usenix.org/system/files/usenixsecurity26-sun-desen.pdf)
- **解读**: 扩散模型近似缓存复用相似提示的中间状态、打破用户间隔离，带来新的安全风险。该文全面评估其漏洞并给出三个远程攻击：通过近似缓存建立远程隐蔽信道（发送者注入含特殊关键词的提示，接收者数天后仍可恢复信息）；提示窃取攻击（攻击者从命中恢复已缓存提示）；投毒攻击（把攻击者 logo 嵌入先前窃取的提示，命中被投毒缓存提示的请求出现意外 logo 渲染）。全部攻击都经服务系统远程执行，证明近似缓存的严重安全漏洞，代码已公开。


### Side-Channel Attacks on Open vSwitch
- **作者**: Daewoo Kim and Sihang Liu
- **论文 PDF**: [Side-Channel Attacks on Open vSwitch](https://www.usenix.org/system/files/usenixsecurity26-kim-daewoo.pdf)
- **解读**: 云虚拟化依赖主机内虚拟交换机，Open vSwitch（OVS）维护缓存层级加速包转发。该文从安全视角刻画 OVS 内部缓存系统并识别三个攻击原语，据此提出三个破坏虚拟化隔离的远程攻击：利用不同缓存的远程隐蔽信道；泄露远程用户包头部字段的头部恢复攻击（破坏系统机密性保证）；恢复远程受害者包速率的远程包速率监控攻击，并讨论潜在缓解措施。


### A Distortion-minimization Watermarking Framework for Large Language Models: Larger Capacity, Stronger Robustness and Higher Quality
- **作者**: Liming Zhai
- **论文 PDF**: [A Distortion-minimization Watermarking Framework for Large Language Models: Larger Capacity, Stronger Robustness and Higher Quality](https://www.usenix.org/system/files/usenixsecurity26-zhai.pdf)
- **解读**: LLM 水印的容量、鲁棒性与文本质量三者常需分离设计、难以兼顾。该文提出失真最小化水印（DMW）框架，把鲁棒性与质量建模为文本修改的失真成本，在给定水印长度下最小化总失真实现统一优化：鲁棒性成本利用语义不变性抗攻击，两个质量成本把修改引导至低内聚、高变异区域以降低感知影响；提出周期优化 syndrome-trellis 码（PO-STCs），把总失真最小化表述为周期最短路问题，实现顺序生成的实时优化与灵活容量控制。跨数据集与 LLM 实验显示 DMW 全面优于 SOTA——严重改写攻击下匹配率最高比最佳基线高 46.35%，同时保持更优文本质量。


### Behind Bars: A Side-Channel Attack on NVIDIA MIG Cache Partitioning Using Memory Barriers
- **作者**: Cheng Gu, Reese Levine, Zhenkai Zhang, Tyler Sorensen, Yanan Guo
- **论文 PDF**: [Behind Bars: A Side-Channel Attack on NVIDIA MIG Cache Partitioning Using Memory Barriers](https://www.usenix.org/system/files/usenixsecurity26-gu-cheng.pdf)
- **解读**: NVIDIA MIG 宣称通过专用 L2 缓存片等资源实现实例间隔离并构成机密计算基础，但其安全声明值得检验。该文实证检验 MIG 启用时的 GPU L2 缓存行为：尽管有分区设计，跨实例 L2 干扰仍然存在——一个 MIG 实例产生的内存屏障（membar）副作用跨 L2 分区传播，影响其他实例特定加载操作的时序；这些 membar 可由内核启动等特定 GPU 活动触发。基于此开发新的时序侧信道攻击：一个实例中的攻击者可推断另一实例受害者的内核启动模式，而 LLM 推理等广泛 GPU 应用的内核启动模式与敏感信息相关，从而危害其机密性。

### TrojPix: Electromagnetic Covert Channels via Imperceptible Pixel Modulation
- **作者**: Guoming Zhang, Huiting Zhang, Yanni Yang and Pengfei Hu
- **论文 PDF**: [TrojPix: Electromagnetic Covert Channels via Imperceptible Pixel Modulation](https://www.usenix.org/system/files/usenixsecurity26-zhang-guoming.pdf)
- **解读**: 气隙网络依赖物理隔离抵御外部连接，先前的电磁（EM）隐蔽信道难以同时兼顾高吞吐、远距离与视觉不可感知。作者发现不可感知的像素调制可在数字视频电缆上确定性诱发可控EM发射，且无需系统权限或硬件改动，并据此实现TrojPix隐蔽信道，通过像素到采样映射与自适应解码实现采样率级鲁棒通信。在9家显示器厂商、15根商用数字视频电缆上的评估表明，TrojPix在"伪屏幕关闭"与"前景嵌入"两种攻击模式下达到8.1 Mbps峰值吞吐与208 m最大传输距离，揭示了对气隙网络现实的隐蔽威胁。


### Anonymous Tokens with Designated-Reader Metadata Bit
- **作者**: Aisha Tu, Meng Jia, Kun He
- **论文 PDF**: [Anonymous Tokens with Designated-Reader Metadata Bit](https://www.usenix.org/system/files/usenixsecurity26-tu.pdf)
- **解读**: 现有带私有元数据位的匿名令牌只能由令牌签发者读取信号，使签发者负担沉重，且因验证者必须联系签发者而难以支持签发者隐藏。本文提出带指定读者元数据位的匿名令牌方案，允许用户指定一个被签发者认可的验证者直接从令牌读取信号，并扩展支持读者隐藏（对签发者与其他验证者隐藏用户的目标验证者）与签发者隐藏。作者证明了构造的安全性并报告了性能。


### Jailbreaking the AMD Secure Processor: Enabling Live Analysis of SEV-SNP's Undocumented Security Boundaries
- **作者**: Muyan Shen, School of Cryptology, Hongzhan Ma
- **论文 PDF**: [Jailbreaking the AMD Secure Processor: Enabling Live Analysis of SEV-SNP's Undocumented Security Boundaries](https://www.usenix.org/system/files/usenixsecurity26-shen.pdf)
- **解读**: AMD SEV-SNP以AMD安全处理器（ASP）为硬件信任根保护虚拟机，但其安全依赖动态热补丁等复杂不透明固件特性，形成被屏蔽的攻击面。作者提出ASPBreaker，利用内存别名与TOCTOU漏洞的新颖组合，实现首个实用且完全确定性的ASP越狱，并以之为工具进行实时分析，展示在易受攻击固件版本上获得任意代码执行后如何颠覆后续已完全修补版本的安全。分析发现关键缺陷，可实现解密虚拟机内存与绕过现有缓解伪造证明报告两类攻击；该结果从根本上破坏了SEV-SNP的前向安全模型，并凸显对不透明固件边界进行独立审计的必要性。


### TIMESLICE-SANDWICH: A GPU Side-Channel Attack Exploiting Time-Sliced Scheduling
- **作者**: Hodong Kim and Gyeongsup Lim, Seunghee Shin, Youngjoo Shin and Junbeom Hur
- **论文 PDF**: [TIMESLICE-SANDWICH: A GPU Side-Channel Attack Exploiting Time-Sliced Scheduling](https://www.usenix.org/system/files/usenixsecurity26-kim-hodong.pdf)
- **解读**: GPU时间片调度是当代GPU资源共享的标准机制，但其侧信道安全影响此前未被充分研究。作者先识别时间片的上界，据此估计并发程序的时间片时长并推断其整体GPU利用率模式，进而提出TIMESLICE-SANDWICH攻击，利用时间片时长差异推断和区分受害执行模式，且无需竞争特定共享资源。实验显示其在神经网络恢复攻击上平均F1达94.40%、在Google Chrome网站指纹攻击上Top-1精度达92.84%，有噪声时神经网络恢复平均F1仍为73.74%；最后讨论了针对时间片模式侧信道风险的缓解方案。


### Bridging Usability and Performance: A Tensor Compiler for Autovectorizing Homomorphic Encryption
- **作者**: Edward Chen
- **论文 PDF**: [Bridging Usability and Performance: A Tensor Compiler for Autovectorizing Homomorphic Encryption](https://www.usenix.org/system/files/usenixsecurity26-chen-edward.pdf)
- **解读**: 同态加密（HE）中张量运算的性能高度依赖明文数据在密文中的打包布局，而大型张量程序的布局分配空间庞大，手工编写高效HE程序既困难又繁琐。本文提出Rotom编译框架，系统探索广泛的布局分配并应用最先进优化，自动生成等价且高效的HE程序，核心是新颖轻量的ApplyRoll布局转换算子以灵活修改底层数据布局。评估表明Rotom可在5分钟内可扩展地编译全部张量工作负载，将手调协议中的旋转次数减少最多3倍，相比先前自动向量化系统取得最高80倍性能提升。


### Shadowfax: Hybrid Security and Deniability for AKEMs
- **作者**: Phillip Gajland, Vincent Hwang, Jonas Janneck
- **论文 PDF**: [Shadowfax: Hybrid Security and Deniability for AKEMs](https://www.usenix.org/system/files/usenixsecurity26-gajland.pdf)
- **解读**: 密码协议向抗量子安全过渡时，混合方案常牺牲效率、紧凑性或安全性，可否认性即是被牺牲的属性之一：经典X3DH（Signal、WhatsApp）具备可否认性，而PQXDH与Apple iMessage PQ3等抗量子协议没有。本文提出两种面向认证密钥封装机制（AKEM）的混合方案：一是黑盒构造，在两个组成AKEM均可否认时保持可否认性；二是Shadowfax非黑盒构造，组合经典非交互密钥交换、抗量子KEM与抗量子环签名，在不诚实与诚实接收者两种设置下均满足可否认性。以标准化组件ML-KEM与Falcon实例化时，Shadowfax密文1728字节、公钥2036字节，在Apple M1 Pro上封装与解封成本分别为1.8M与0.7M周期。


### RBOOT: Accelerating Homomorphic Neural Network Inference by Fusing ReLU within Bootstrapping
- **作者**: Zhaomin Yang
- **论文 PDF**: [RBOOT: Accelerating Homomorphic Neural Network Inference by Fusing ReLU within Bootstrapping](https://www.usenix.org/system/files/usenixsecurity26-yang-zhaomin.pdf)
- **解读**: 全同态加密（FHE）安全神经网络推理的主要瓶颈是非线性激活函数（如ReLU）的求值，现有最优方案用高次多项式近似ReLU，计算开销巨大。作者提出RBOOT框架，将ReLU求值无缝融入CKKS自举过程以显著降低乘法深度；关键洞察是CKKS自举的EvalMod步骤本身由非线性三角函数组成，而先前工作把自举与激活函数当作独立例程，错失了利用该非线性的机会。在四个常用CNN模型上，RBOOT相比先前多项式近似方案实现2.77倍端到端推理加速与81%的内存降低，同时保持可比精度。


### PANGOLIN: Fuzzing Multilingual IoT Firmware with LLM-Driven Code Analysis
- **作者**: Zhipeng Jia and Xiaokang Yin, Shuitao Gan, Chao Zhang, JCSS, Hangtian Liu, Jiangan Ji, Jinglei Tan, Shengli Liu
- **论文 PDF**: [PANGOLIN: Fuzzing Multilingual IoT Firmware with LLM-Driven Code Analysis](https://www.usenix.org/system/files/usenixsecurity26-jia-zhipeng.pdf)
- **解读**: 多语言IoT设备（如C、Python、Lua混合实现Web服务）中存在大量未暴露给前端、参数层次复杂的隐藏接口，有效提取接口规范以发现漏洞是亟待解决的问题。PANGOLIN先用LLM分析API分派机制识别接口，再由LLM智能体进行跨语言分析生成输入参数规范，最后用响应驱动的反馈修正参数规范，从而支持语义感知模糊测试以探索更深代码路径。PANGOLIN发现68个此前未知漏洞（是SOTA工具LABRADOR的2.96倍），其中45个位于隐藏接口（EAGLEYE仅识别4个）；截至写作时全部已报告厂商并获确认，31个漏洞已分配编号。


### Patch-Guided Vulnerability Detection: Extracting Java API Security Rules via Attack–Defense Cross-Analysis
- **作者**: Bofei Chen, Chibin Zhang and Mathias Payer, Yuan Zhang
- **论文 PDF**: [Patch-Guided Vulnerability Detection: Extracting Java API Security Rules via Attack–Defense Cross-Analysis](https://www.usenix.org/system/files/usenixsecurity26-chen-bofei.pdf)
- **解读**: Java安全敏感API的不当使用常导致远程代码执行等严重漏洞，而现有API安全规则生成依赖不完整文档或基于不一致性的源码模式推断，能力有限。VulGenie是补丁驱动的框架，从已确认的安全补丁中提取精确的API安全规则以检测API误用漏洞：用改进行为依赖补丁图隔离噪声补丁中的约束违反与防御变更，通过攻防交叉验证识别受保护的安全敏感API并合成规则，并以自适应偏差引导静态分析平衡精度与性能。在150个近期Java安全补丁上提取198条规则（精度81.82%），其中177条为CodeQL所缺；在10个流行Java应用中检测出46个0-day漏洞，25个已修复并获得10个CVE编号。


### The State of Passkeys: Studying the Adoption and Security of Passkeys on the Web
- **作者**: Louis Jannett, Andreas Mayer and Maximilian Westers, Vladislav Mladenov, Christian Mainka, Jörg Schwenk
- **论文 PDF**: [The State of Passkeys: Studying the Adoption and Security of Passkeys on the Web](https://www.usenix.org/system/files/usenixsecurity26-jannett.pdf)
- **解读**: 基于FIDO2与WebAuthn的passkey提供抗钓鱼认证，采用网站日益增多，但缺乏大规模的综合安全评估。作者构建PASSKEYS-RADAR数据集，自2021年起聚合社区目录、Tranco 1M、CrUX 18M与历史互联网档案等来源持续追踪passkey部署，并分析872个启用passkey的网站，发现网站在添加/删除passkey的机制上差异显著，且会要求认证器使用已弃用的密码算法。为全面评估，作者开发PASSKEYS-ATTACKER，可在协议每一步精确操纵WebAuthn消息，集成15种攻击类型（其中10种为先前工作未覆盖），发现2种关键CVSS评分的攻击，出现在103个评估网站中的18个上，可接管账户、删除passkey或锁定用户；近半数（53个）测试网站至少存在一种高CVSS攻击。


### StackWarp: Breaking AMD SEV-SNP Integrity via Deterministic Stack-Pointer Manipulation through the CPU's Stack Engine
- **作者**: Ruiyi Zhang
- **论文 PDF**: [StackWarp: Breaking AMD SEV-SNP Integrity via Deterministic Stack-Pointer Manipulation through the CPU's Stack Engine](https://www.usenix.org/system/files/usenixsecurity26-zhang-ruiyi.pdf)
- **解读**: 机密虚拟机（CVM）如AMD SEV-SNP承诺在启用SMT的多租户云中提供隔离，但先前攻击聚焦内存层次或执行单元，忽视了前端配置。StackWarp利用AMD Zen 1–5 CPU共享MSR中一个未文档化、控制栈引擎开关的比特位；逆向显示栈引擎状态在逻辑核心间未正确同步，攻击者可跨Zen各代（含完全修补的Zen 5）确定性调整兄弟逻辑核心上的栈指针。作者通过系统化MSR空间探索（含未文档化MSR）发现该漏洞，并以四个端到端攻击展示影响：RSA-CRT私钥恢复、OpenSSH口令认证绕过、经sudo或内核ROP链的提权，结论是当前开启SMT会破坏SEV-SNP的完整性保证。


### Leveraging Cryptographic Simulator Synthesis for Formally Verifying the FOO E-Voting Protocol
- **作者**: David Baelde, Adrien Koutsos and Justine Sauvage
- **论文 PDF**: [Leveraging Cryptographic Simulator Synthesis for Formally Verifying the FOO E-Voting Protocol](https://www.usenix.org/system/files/usenixsecurity26-baelde.pdf)
- **解读**: 密码学证明中的归约依赖模拟器，而模拟器编写繁琐且含大量琐碎代码，纸质证明中往往只作草图因而易错；机械化证明虽消除此类错误，但要求用户显式编写模拟器负担过重。本文研究Squirrel中以双推导（bi-deduction）表达的模拟器综合问题，指出先前的双推导证明搜索过程在处理IND-CCA2等游戏时存在系统性失败，并提出显著改进的过程：可跨递归迭代复用oracle调用并生成精确不变量予以证明。该过程已在Squirrel中实现，并用于完成FOO电子投票协议的选票隐私证明——这是FOO的首个计算机械化证明，也是迄今最复杂的Squirrel证明。


### The Art of Hide and Seek: Making Pickle-Based Model Supply Chain Poisoning Stealthy Again
- **作者**: Tong Liu and Guozhu Meng, School of Cyber Security, Peng Zhou, Zizhuang Deng, State Key Laboratory of Cryptography and Digital Economy Security, Shuaiyin Yao and Kai Chen, School of Cyber Security
- **论文 PDF**: [The Art of Hide and Seek: Making Pickle-Based Model Supply Chain Poisoning Stealthy Again](https://www.usenix.org/system/files/usenixsecurity26-liu-tong.pdf)
- **解读**: pickle反序列化漏洞贯穿Python历史且长期未解，许多AI/ML框架仍以pickle作为模型序列化协议，模型共享平台（如Hugging Face）放大了其供应链投毒影响；现有扫描器对投毒面理解不完整因而可被绕过。作者首次从模型加载与危险函数两个视角系统披露pickle模型投毒面：加载面在五个基础AI/ML框架中发现22条pickle模型加载路径（19条被现有扫描器完全遗漏），并开发Exception-Directed Programming（EDP）绕过技术发现9个实例（7个能绕过全部扫描器）；危险函数面发现133个可利用gadget。结果显示对真实扫描器可实现近乎100%的绕过率，最佳扫描器下gadget仍有89%绕过率；发现已负责任披露并获得$12,000漏洞赏金。


### B-Privacy: Defining and Enforcing Privacy in Weighted Voting
- **作者**: Samuel Breckenridge, Amy Zhao, Patrick McCorry, Ari Juels
- **论文 PDF**: [B-Privacy: Defining and Enforcing Privacy in Weighted Voting](https://www.usenix.org/system/files/usenixsecurity26-breckenridge.pdf)
- **解读**: 在加密货币与web3系统中，按代币持有量加权的投票系统已很普遍，作者证明这类系统推翻了传统选票保密式的隐私概念：实验显示即使秘密投票，公开原始计票也常泄露投票者选择。为此本文引入以贿赂（bribery）为基础的B-privacy概念，刻画对手基于公开计票贿赂投票者的经济成本，并提出对计票加噪以提升B-privacy的机制，证明其与透明度（计票报告准确性）之间的权衡界。在27个DAO的2503个提案上的实验表明，在最小透明度损失下该机制将B-privacy几何平均提升3.5倍，为加权投票系统提供了首个有原则、实用的系统性指引。


### InstrSem: Automatically and Generically Inferring Semantics of (Undocumented) CPU Instructions
- **作者**: Lorenz Hetterich
- **论文 PDF**: [InstrSem: Automatically and Generically Inferring Semantics of (Undocumented) CPU Instructions](https://www.usenix.org/system/files/usenixsecurity26-hetterich-lorenz-instrsem.pdf)
- **解读**: 现代CPU的ISA机器可读语义常不完整，许多CPU还支持规范中缺失的未文档化指令，带来潜在安全漏洞。InstrSem是ISA无关、模块化、全自动的指令语义推断方法：从原始编码出发，在系统变化的架构状态下执行，合成解释每个状态组件变化的紧凑数学函数，再通过变异编码位并将行为变化与位位置关联，从单条编码泛化出完整指令、恢复寄存器与立即数字段。该方法只需轻量ISA模型与每架构的用户态运行器，支持定长与变长编码、内存访问与条件行为；评估中自动恢复RV64I基础指令集97.81%以上的正确语义，在LA64上77小时内覆盖1,009,055,744条编码，并发现未文档化向量指令、QEMU与Loongson硬件不一致以及可使QEMU崩溃的指令。


### Loongleak: Architectural Cross-Privilege-Boundary Data Leakage on LoongArch CPUs
- **作者**: Lorenz Hetterich
- **论文 PDF**: [Loongleak: Architectural Cross-Privilege-Boundary Data Leakage on LoongArch CPUs](https://www.usenix.org/system/files/usenixsecurity26-hetterich-lorenz-loongleak.pdf)
- **解读**: x86-64、Arm与RISC-V CPU获得大量架构漏洞研究，而广泛用于中国基础设施的Loongson（LoongArch ISA）处理器缺乏同等审视，尤其在去美化背景下构成安全盲区。作者发现并分析影响多款Loongson CPU的架构漏洞LoongLeak：4字节浮点加载会从L1数据缓存返回陈旧位，非特权攻击者可跨安全域（如内核或虚拟机管理程序）泄露机密数据，且该泄漏是架构性的，无需时序或侧信道。案例研究包括在数秒内恢复内核全盘AES密钥、部分root密码哈希，并绕过ASLR与栈金丝雀等软件防御；软件缓解（浮点模拟带来10×–21×开销、内核到用户态转换时冲刷L1并关闭SMT）可有效缓解，但长期仍需硬件修复。


### Sliding into the Flight Deck's DMs: Practical Message Attacks on CPDLC
- **作者**: Mehdi Ziazi, Khalid Aleem, Harshad Sathaye, Martin Strohmeier
- **论文 PDF**: [Sliding into the Flight Deck's DMs: Practical Message Attacks on CPDLC](https://www.usenix.org/system/files/usenixsecurity26-ziazi.pdf)
- **解读**: 控制器-飞行员数据链通信（CPDLC）已成为现代空中交通管理（尤其高频度与越洋空域）的组成部分，但它不实现加密，主要依赖协议复杂性与模糊性作为防护屏障。作者对CPDLC进行全栈安全分析，展示可劫持ATC-飞行员链路的恶意地面站攻击，以及可使无线电范围内所有飞机的CPDLC服务失效的大规模拒绝服务攻击，并实现cpdlc-gs——首个基于SDR的全栈CPDLC地面站，可注入上行消息伪造CPDLC飞行指令并实施有效DoS。作者与航管服务商和航电制造商合作，用Universal Avionics真实可认证硬件构建全新全功能测试环境验证多种攻击，证明即使孤立的恶意站也构成实质威胁，尤其当飞行员处于高工作负载或降级通信场景时。


### You Know Why, but Still Rely: The Impact of Explainable AI on Trust, Task Load, and Performance in Cybersecurity Decision-Making
- **作者**: Neele Roch
- **论文 PDF**: [You Know Why, but Still Rely: The Impact of Explainable AI on Trust, Task Load, and Performance in Cybersecurity Decision-Making](https://www.usenix.org/system/files/usenixsecurity26-roch.pdf)
- **解读**: 网络安全任务的数量与复杂度超出从业者承载能力，用AI增强人类专长可减轻认知过载，而治理机构与从业者都要求AI决策透明可解释。作者报告一项N=139的组间实验，研究XAI解释对具备网络安全领域知识的用户在恶意域名拦截情境中信任、可用性、感知任务负荷与协作任务性能的影响。结果显示提供解释并未增进信任——有领域知识的用户在交互后信任反而更低，质性结果提示他们应用自己的决策标准，暴露AI决策边界可能引入歧义并滋生不信任；XAI虽未增加感知任务负荷，但也未改善性能，引发对当前XAI方法在知识密集型决策场景有效性的质疑。


### Silicon Heist: (Ransom) Attacks for Cloud FPGAs via Privilege Escalation
- **作者**: Simon Klix, Russell Tessier
- **论文 PDF**: [Silicon Heist: (Ransom) Attacks for Cloud FPGAs via Privilege Escalation](https://www.usenix.org/system/files/usenixsecurity26-klix.pdf)
- **解读**: 云FPGA允许用户在云服务商（CSP）硬件上部署定制设计，为防硬件损坏与DoS，CSP实施安全机制限制用户设计，但低层硬件组件的安全被忽视。作者在AMD FPGA上发现一条新提权路径：经内部配置访问端口（ICAP）绕过提供方防御，增量提升攻击者能力至远程JTAG访问，进而访问设备eFuse，不可逆地编程加密设置、禁用未来重配置并将CSP锁在自家设备之外——可被用于勒索攻击，形成首个面向FPGA的勒索软件。作者在Amazon EC2 F1与F2实例上演示了该路径的可行性并考察攻击向量影响，暴露了云环境中低层硬件组件未受保护这一被忽视的威胁。


### Bridging Bitcoin to Second Layers via BitVM2
- **作者**: Robin Linus Woll, Lukas Aumayr, Zeta Avarikioti, Matteo Maffei, Andrea Pelosi, Orfeas Stefanos Thyfronitis Litos, Christos Stefo, David Tse, Alexei Zamyatin
- **论文 PDF**: [Bridging Bitcoin to Second Layers via BitVM2](https://www.usenix.org/system/files/usenixsecurity26-woll.pdf)
- **解读**: 比特币与其二层网络或其他链之间的免信任桥是区块链基础设施的圣杯。本文引入首个基于轻客户端的比特币桥：核心是BitVM2-CORE范式，在比特币上实现任意程序执行，兼具图灵完备表达能力与比特币共识的安全；BitVM2-BRIDGE将信任假设从诚实多数（t-of-n）降低为存在性诚实（1-of-n），活性仅需一个理性操作者保证，任何用户都可充当挑战者从而实现免许可验证。生产级BitVM2实现已完成，并在比特币主网上执行了完整的挑战验证。


### "Oh, what people would do with my knife?" Navigating the Dual-Use Dilemma in PoC Exploit Development, Disclosure, and Community Dynamics
- **作者**: Arwa Al Alsadi and Lorenz Kustosch, Lamya Alowain, Michel Van Eeten and Carlos H. Gañán
- **论文 PDF**: ["Oh, what people would do with my knife?" Navigating the Dual-Use Dilemma in PoC Exploit Development, Disclosure, and Community Dynamics](https://www.usenix.org/system/files/usenixsecurity26-al-alsadi.pdf)
- **解读**: 概念验证（PoC）漏洞利用在披露后数分钟内即从演示转向武器化攻击，但创造PoC背后的人类因素未被理解。作者对来自多个地区的16名PoC开发者进行半结构化访谈，运用期望-价值理论揭示PoC开发是一个技术信心、价值评估与风险计算在双重用途张力中交织的复杂动机生态。结果显示PoC开发是从崩溃演示到武器化利用的连续谱而非二元伦理选择，并提出三个理论扩展：外化责任的双重用途道德推理、厂商行为重塑披露决策的动态价值评估、以及伦理研究与技术精通之间的身份导航；厂商响应性、社区动态与法律约束显著影响披露策略，开发者会采取风险缓解方式应对安全改进与潜在滥用的张力。


### Lost in Blockchain Address Misuse: Hidden Cross-Platform Risks and Their Security Impact
- **作者**: Zhenzhe Shao, Jiashuo Zhang, Zihao Li, Daoyuan Wu, Chong Chen and Yiming Shen, Lingfeng Bao, Yanlin Wang, Jiachi Chen
- **论文 PDF**: [Lost in Blockchain Address Misuse: Hidden Cross-Platform Risks and Their Security Impact](https://www.usenix.org/system/files/usenixsecurity26-shao-zhenzhe.pdf)
- **解读**: 区块链账户模型以地址为交互与资产安全的基本接口，地址误用构成显著风险。作者系统揭示并分析称为Address Misuse的风险类别：合约账户（CA）误用（用户把非合约地址误当CA）与外部账户（EOA）误用（用户与私钥已暴露的EOA交互），并为每类揭示底层机制、引入此前未公开的攻击向量。基于GitHub与Stack Exchange构建含1000万候选地址与1600万暴露私钥的数据集，在Ethereum与BSC上进行大规模链上分析，结合启发式规则、交易模式分析与符号执行识别出65,340个高风险地址实例，关联资产损失约127k ETH与17.7k BNB（超5.748亿美元），检测总精度99.11%，并提供了两个新型攻击向量的真实世界证据。


### DDR-SSE: Duplicated Retrieval of Documents for System-wide Secure Searchable Symmetric Encryption
- **作者**: Zichen Gui, Simon-Philipp Merz and Kenneth G. Paterson, Sikhar Patranabis
- **论文 PDF**: [DDR-SSE: Duplicated Retrieval of Documents for System-wide Secure Searchable Symmetric Encryption](https://www.usenix.org/system/files/usenixsecurity26-gui.pdf)
- **解读**: 可搜索对称加密（SSE）以一定泄漏为代价支持密文文档关键词搜索，系统级安全要求对抗者可访问索引与文档双方检索泄漏；多数SOTA方案并非系统级安全，而唯一高效的SWiSSSE需要每次查询更新客户端状态且泄漏难以精确刻画。本文提出DDR-SSE，采用重复文档存储与随机化文档检索抑制访问模式泄漏，只需静态客户端状态且泄漏画像简单（不同于SWiSSSE极其复杂的文档检索机制）。作者给出针对正式系统级泄漏画像的基于模拟的安全证明，通过广泛泄漏密码分析证明其对查询重构攻击（即使在不切实际的强攻击假设下）具有韧性，原型基准表明可平滑扩展到现实规模的大数据库。


### Concretely Efficient Blind Signatures Based on VOLE-in-the-Head Proofs and the MAYO Trapdoor
- **作者**: Carsten Baum and Marvin Beckmann, Ward Beullens, Shibam Mukherjee, Christian Rechberger
- **论文 PDF**: [Concretely Efficient Blind Signatures Based on VOLE-in-the-Head Proofs and the MAYO Trapdoor](https://www.usenix.org/system/files/usenixsecurity26-baum.pdf)
- **解读**: 盲签名是匿名凭证、电子现金等隐私保护应用的重要构件，近年基于格的抗量子盲签名性能虽有改进，但尚未达到实际效率：每次出示到验证者的通信至少20 KB、证明者时间超过100 ms。本文提出可称为抗量子的盲签名方案PoMFRIT，基于VOLE-in-the-Head零知识证明系统（Baum等，CRYPTO 2023）与MAYO数字签名（Beullens，SAC 2021）的结合，并实现多个版本展示安全与性能权衡。签发仅需0.45 KB通信，6.7 KB的盲签名出示在保守的128位安全构造下也只需不到76 ms；作为构件还实现了首个SHA-3族哈希函数的VOLE-in-the-Head证明。


### Turn Your Face Into An Attack Surface: Screen Attack Using Facial Reflections in Video Conferencing
- **作者**: Yong Huang, Wanqing Tu
- **论文 PDF**: [Turn Your Face Into An Attack Surface: Screen Attack Using Facial Reflections in Video Conferencing](https://www.usenix.org/system/files/usenixsecurity26-huang-yong.pdf)
- **解读**: 视频会议中人脸是主要视觉焦点，但作者论证人脸本身也是侧信道：在环境光与屏幕光共同照射下，人脸会反射不同屏幕内容的光学变化，通过在线视频流无意泄露屏幕信息。可行性研究之后，作者提出FaceTell侧信道攻击系统，从视频会议中普遍而细微的面部反射窃听细粒度应用活动，并在三款笔记本与四个主流视频会议平台构成的真实测试床上用24名受试者、13个室内环境、超过12小时视频数据评估。FaceTell对28个流行应用的窃听精度达99.32%，对多种实际影响因素具有鲁棒性；论文最后提出潜在对策以缓解该攻击。


### PROBE+DETECT+MITIGATE (PDM): Enabling Cloud Tenants to Self-Defend against Microarchitectural Attacks
- **作者**: Arash Daneshmand, Hugo Kermabon-Bobinnec, Lingyu Wang, Makan Pourzandi, Suryadipta Majumdar, Yosr Jarraya
- **论文 PDF**: [PROBE+DETECT+MITIGATE (PDM): Enabling Cloud Tenants to Self-Defend against Microarchitectural Attacks](https://www.usenix.org/system/files/usenixsecurity26-daneshmand.pdf)
- **解读**: 微架构攻击可在利益冲突的云租户间造成信息泄漏，而现有方案通常需要硬件性能计数器或宿主机进程等提供方级资源，租户往往不可得；租户缺乏意识也使云提供方拖延部署厂商补丁。PDM使云租户无需提供方帮助即可独立检测与缓解微架构攻击：基于一个有趣的观察——用流行的FLUSH+RELOAD技术探测受害应用内存空间本身即可实现检测；检测后选择性触发混淆与内存加密实现高效的租户级缓解。PDM还处理了不含驱逐的攻击（如Spectre）、避免源码/二进制插桩需求、受害者或共驻租户的良性噪声，以及精度-延迟-开销权衡；实验显示其能高精度（测试床TPR≥99.72%、FPR≤0.13%；AWS Fargate上TPR≥98.63%、FPR≤0.83%）、及时（触发缓解提前7 ms）、高效（SPEC CPU 2017上开销≤2.47%）且鲁棒地检测和缓解包括PRIME+PROBE与Spectre在内的多种攻击。


### ARM MTE Performance in Practice
- **作者**: Taehyun Noh, Yingchen Wang, Tal Garfinkel, Mahesh Madhav, Daniel Moghimi, Mattan Erez and Shravan Narayan
- **论文 PDF**: [ARM MTE Performance in Practice](https://www.usenix.org/system/files/usenixsecurity26-noh.pdf)
- **解读**: 本文在四种微架构上对ARM MTE硬件性能进行首次全面分析：Google Pixel 8与Pixel 9的ARM大核（A7x）、小核（A5x）、性能核（Cortex-X），以及Ampere Computing的AmpereOne核，并含Apple M5芯片的初步分析。作者在其主要应用——概率内存安全——上评估SPEC CPU基准与RocksDB、Nginx、PostgreSQL、Memcached等服务端负载，发现MTE通常开销适中，但某些基准出现最高6.64倍的性能下降，并定位了这些开销的微架构成因及未来处理器可改进之处。对于内存追踪、TOCTOU防护、沙箱与CFI等更专门的场景，MTE在某些情况下已有显著优势，另一些则收益甚微或依赖未来硬件；论文还指出先前MTE性能表征工作因方法论或实验错误而存在不完整或不正确之处。


### Why Johnny Adopts Identity-Based Software Signing: A Usability Case Study of Sigstore
- **作者**: Kelechi G. Kalu, Sophie Chen, Santiago Torres-Arias and James C. Davis
- **论文 PDF**: [Why Johnny Adopts Identity-Based Software Signing: A Usability Case Study of Sigstore](https://www.usenix.org/system/files/usenixsecurity26-kalu.pdf)
- **解读**: 软件签名是保障软件供应链组件完整性与真实性的最稳健手段，但传统签名工具把密钥管理与签名者识别负担加给从业者，造成可用性挑战与安全风险；新一代签名工具自动化了这些环节，但其可用性及对采用与实效的影响尚不清楚。作者对Sigstore（开创性且广泛采用的新一代签名代表）开展首次可用性研究，访谈17位行业专家，考察其工具选择的问题与优势、签名工具使用随时间的演变，以及引发可用性关切的场景。研究发现新一代工具的组件成熟度与采用准备度不一，集成灵活性是普遍痛点（但可能通过插件与API缓解），并为工具制造商、采用组织与研究界提出建议。


### Scribe: Low-memory SNARKs via Read-Write Streaming
- **作者**: Anubhav Baweja
- **论文 PDF**: [Scribe: Low-memory SNARKs via Read-Write Streaming](https://www.usenix.org/system/files/usenixsecurity26-baweja.pdf)
- **解读**: 简洁非交互式知识论证（SNARK）让证明者能为任意NP陈述生成简短可验证证明，但部署受瓶颈制约：证明者生成中等大小语句的证明也需要巨大时间与内存。Scribe利用大量未利用的磁盘存储资源：证明者不把大型中间状态放RAM而放磁盘，并设计读写流式计算模型，使证明者只能以流式方式读写状态以保证访问高效。在商品硬件上，Scribe可轻松扩展到2^28门的电路，内存低于750 MB，与需更多内存的最先进内存密集型基线（HyperPlonk，EUROCRYPT 2023）相比证明延迟开销仅10%。


### Identifying Provenance of Generative Text-to-Image Models
- **作者**: Anna Yoo Jeong Ha
- **论文 PDF**: [Identifying Provenance of Generative Text-to-Image Models](https://www.usenix.org/system/files/usenixsecurity26-ha.pdf)
- **解读**: 微调可快速廉价地产出与从头训练几乎无法区分的文本到图像模型，对微调模型的虚假陈述损害AI公司与用户的利益（抑制竞争、误导模型质量与训练过程伦理）。作者提出仅凭黑盒查询识别模型谱系的系统：基于可量化文本到图像模型间特征空间差异的分析——通过分析模型对详细提示的响应，用通用特征提取器提取输出视觉特征，与参考基础模型池的分布做Jensen-Shannon散度比较，再经统计假设检验判定目标模型是从头训练还是微调、以及可能的父模型。系统在七个广泛使用的扩散模型及大量微调变体上取得高归因精度，即使面对图像后处理或权重扰动等对抗条件仍有效，并成功追踪在线平台上的真实世界模型来源。


### Semantics Over Syntax: Uncovering Pre-Authentication 5G Baseband Vulnerabilities
- **作者**: Qiqing Huang and Xingyu Wang, Wanda Guo and Guofei Gu, Hongxin Hu
- **论文 PDF**: [Semantics Over Syntax: Uncovering Pre-Authentication 5G Baseband Vulnerabilities](https://www.usenix.org/system/files/usenixsecurity26-huang-qiqing.pdf)
- **解读**: 5G用户设备（UE）在认证与完整性保护建立前就处理RRC配置消息，先前测试工作主要构造语法无效输入；作者证明语法有效但语义不一致（违反规范级字段约束或跨字段依赖）的消息可将基带实现驱动到无效状态，触发断言失败或调制解调器崩溃，揭示预认证信令语义不一致是5G UE实现中关键且未充分开发的攻击面。作者提出约束引导语义测试框架CONSET：解码RRC消息为结构化字段、派生基于模式的规则、以证据受限方式用LLM推断跨字段依赖，并生成故意违反语义约束但语法有效的测试用例。在商用手机上CONSET经负责任披露发现7个未知漏洞（含3个高危CVE，影响64个芯片组型号与542款以上商用智能手机），在开源OAI UE上额外触发46个不同崩溃点。


### Window-based Membership Inference Attacks Against Fine-tuned Large Language Models
- **作者**: Yuetian Chen, Ashish Kundu, Charles Fleming, Bruno Ribeiro and Ninghui Li
- **论文 PDF**: [Window-based Membership Inference Attacks Against Fine-tuned Large Language Models](https://www.usenix.org/system/files/usenixsecurity26-chen-yuetian.pdf)
- **解读**: 针对大语言模型（LLM）的多数成员推断攻击（MIA）依赖平均损失等全局信号，这稀释了记忆化中微妙而局部的信号，削弱攻击效果。作者质疑全局平均范式，提出基于滑窗与符号聚合的WBC（Window-Based Comparison）：在文本序列上滑动不同大小的窗口，每个窗口依据目标与参考模型之间的损失比较对成员资格投二元票，再对几何间隔的窗口大小投票做集成，从而同时捕获从token级痕迹到短语级结构的记忆化模式。在11个数据集上的大量实验表明WBC显著超越既有基线，AUC更高，且在低假阳率阈值下检测率提升2–3倍，证明聚合局部证据比全局平均更有效，暴露了微调LLM的关键隐私漏洞。


### Digital Risks and Coping Practices among Roblox Game Creators
- **作者**: Qiurong Song
- **论文 PDF**: [Digital Risks and Coping Practices among Roblox Game Creators](https://www.usenix.org/system/files/usenixsecurity26-song-qiurong.pdf)
- **解读**: 作为创作者经济日益增长的部分，Roblox等游戏平台让数百万用户设计、发布、推广与变现游戏，但平台创作者面临显著的安全、隐私与金融风险；先前研究考察社交媒体创作者，对游戏创作者的风险格局知之甚少。作者访谈20名Roblox创作者，了解他们如何感知、经历与应对数字风险，分析得出五类风险——平台、生产、组织、社区与技术——可能损害创作者的情绪、身体、关系与财务安全。研究还识别出协商更公平报酬、寻求社区支持等应对策略，并据此提出加强游戏创作者保护的建议。


### Design and Implementation of a Physical Implant Attack on the Boeing 737
- **作者**: Sam Crow, Stephen Checkoway, Patrick Mercier
- **论文 PDF**: [Design and Implementation of a Physical Implant Attack on the Boeing 737](https://www.usenix.org/system/files/usenixsecurity26-crow.pdf)
- **解读**: 作者探索一种新的航空安全威胁模型：对手对机身有临时物理接触，并论证此类攻击实际可行，现有航电系统设计、互连方式与维护架构使该威胁尤为值得关切。以波音737为例，作者开发并演示一种小型可编程硬件植入物，在地面约60秒接触内即可插入现有维护插座；通过精心操纵物理ARINC 429总线信号，该设备可在飞行管理计算机（FMC）与多功能控制显示单元（MCDU）之间创建不可检测的中间人（AITM）能力。论文探讨应对此类攻击的选项，并特别说明为何变压器耦合总线（如MIL-STD-1553）难以用同样方式操纵。


### SoK: Attack and Defense Landscape of Agentic AI Systems
- **作者**: Juhee Kim, Wenbo Guo, Dawn Song
- **论文 PDF**: [SoK: Attack and Defense Landscape of Agentic AI Systems](https://www.usenix.org/system/files/usenixsecurity26-kim-juhee-agentic.pdf)
- **解读**: 将大语言模型与非AI工具组件集成的AI智能体正快速进入实际应用，其灵活性带来与传统软件系统不同的复杂安全挑战。本文给出AI智能体安全领域首个全面的知识系统化（SoK），分析安全AI智能体系统的设计空间、攻击格局与防御机制，并识别该新兴领域未来研究的开放挑战。工作提供了理解AI智能体安全风险与防御策略的首个系统性框架，为构建安全智能体系统与推进该领域研究奠定基础。


### Network-Level Prompt and Trait Leakage in Local Research Agents
- **作者**: Hyejun Jeong
- **论文 PDF**: [Network-Level Prompt and Trait Leakage in Local Research Agents](https://www.usenix.org/system/files/usenixsecurity26-jeong.pdf)
- **解读**: 作者证明Web与研究智能体（WRA）——在互联网上调查复杂主题的基于语言模型的系统——易受被动网络观察者的推断攻击；组织与个人为隐私、法律或财务目的本地部署WRA，使其暴露给DNS解析器、恶意ISP、VPN、Web代理及企业或政府防火墙。与人类零散稀缺的浏览不同，WRA每次请求访问70-140个域名且时序模式独特，作者据此展示仅利用网络级元数据（访问IP地址与时间）的新型提示与用户特质泄漏攻击：基于真实用户查询与合成人设查询构建WRA跟踪数据集，定义行为指标OBELS综合评估原始与推断提示的相似度，攻击可恢复用户提示超过73%的功能与领域知识，多会话设置下以高精度恢复32个潜在特质中的19个，且在部分可观测与噪声条件下仍有效；论文还讨论限制域名多样性或混淆痕迹的缓解策略，显示效用影响可忽略而攻击有效性平均降低29%。


### NOIR: Privacy-Preserving Generation of Code with Open-Source LLMs
- **作者**: Khoa Nguyen, Issa Khalil, Khang Tran and Cristian Borcea, Ruoming Jin, Abdallah Khreishah, My T. Thai
- **论文 PDF**: [NOIR: Privacy-Preserving Generation of Code with Open-Source LLMs](https://www.usenix.org/system/files/usenixsecurity26-nguyen-khoa.pdf)
- **解读**: LLM驱动的代码生成提升软件开发效率，但引入知识产权与数据安全风险：云服务商会观察到客户端的提示与生成的代码（商业系统中可能为专有内容）。NOIR是首个保护客户端提示与生成代码免受云观察的框架：客户端用编码器把提示嵌入发送到云，从LLM获得增强嵌入后再本地解码生成代码；由于云可用嵌入推断提示与代码，NOIR在提示与代码词汇的token嵌入层引入局部差分隐私的不可区分机制，并在客户端采用数据无关的随机分词器，有效防御诚实但好奇的云的重建与频率分析攻击。基于开源LLM的广泛评估显示，NOIR在Evalplus（MBPP与HumanEval，Pass@1分别为76.7与77.4）与BigCodeBench（Pass@1为38.7，仅比原始LLM低1.77%）上显著超越现有基线，同时提供强隐私保护。


### BADControl: Backdoor Attacks Against Control Systems
- **作者**: Luis Burbano, Hampei Sasahara, Ruoyu Song and Z. Berkay Celik, Alvaro A. Cardenas
- **论文 PDF**: [BADControl: Backdoor Attacks Against Control Systems](https://www.usenix.org/system/files/usenixsecurity26-burbano.pdf)
- **解读**: BADCONTROL是首个使用物理触发器的低层控制器后门攻击：通过投毒运行数据植入可由环境外生信号（如自动驾驶中的特定驾驶操作或对抗性道路补丁）激活的漏洞，用投影梯度上升求解约束优化以修改数据，最大化受控系统在目标频率处的频率响应。该方法不同于针对深度学习与强化学习模型的后门（后者操纵高维模型输入或奖励函数）；作者还提出基于正则化与鲁棒优化的两种防御，经专门数学变换把无限投毒场景转化为单一可解优化问题以限制触发信号的最坏情况放大。在PID与LQR控制器上的仿真与物理实验显示：自适应巡航场景达到100%碰撞率，车道保持中后门使受害车辆62%驶入对向车道（无后门时均为0%），而SOTA自动驾驶伪造框架在30次试验中仅识别出1次碰撞，凸显其隐蔽性。


### Can we estimate privacy vulnerability of individual records? Towards Mitigating Attribute Inference Attacks on ML Models
- **作者**: Ehsanul Kabir and Najrin Sultana, Ninghui Li, Shagufta Mehnaz
- **论文 PDF**: [Can we estimate privacy vulnerability of individual records? Towards Mitigating Attribute Inference Attacks on ML Models](https://www.usenix.org/system/files/usenixsecurity26-kabir.pdf)
- **解读**: 机器学习模型易受属性推断与模型反演攻击泄漏隐私，而现有防御追求比防止属性推断泄漏更宽泛的目标，往往无法在可接受效用代价下提供细粒度、漏洞感知的保护。作者先研究记录级漏洞估计：NeighVE是攻击者侧工具，用于识别哪些个体记录更易受推断攻击，其洞察显示记录级泄漏风险很大程度上与模型架构和攻击策略无关，而由数据集级特征——特别是每个记录局部邻域中敏感属性的分布——决定。基于此提出VESL，一种受子空间学习启发的防御，以最小效用损失缓解属性推断泄漏，其平衡机制还改善敏感属性间的公平性并阻止NeighVE可靠识别易受攻击记录；配套的AttriVET估计器在多种场景下以超过90%的精度预测易受攻击记录，支持风险感知的防御设计与审计。


### Lethe: Purifying Backdoored Large Language Models with Knowledge Dilution
- **作者**: Chen Chen, Yuchen Sun and Jiaxin Gao, Xueluan Gong, Qian Wang, Ziyao Liu
- **论文 PDF**: [Lethe: Purifying Backdoored Large Language Models with Knowledge Dilution](https://www.usenix.org/system/files/usenixsecurity26-chen-chen.pdf)
- **解读**: 大语言模型易受后门攻击，在标准查询下表现正常、被特定触发器激活时产生有害响应；现有后门防御要么在实践上缺乏全面性（聚焦狭窄触发设置、仅检测机制、有限领域），要么无法抵御模型编辑、多触发器与无触发器攻击等高级场景。作者提出LETHE，通过内外两种机制用知识稀释消除LLM后门行为：内部用轻量数据集训练干净模型并与后门模型合并，以稀释参数记忆中的后门影响；外部在提示中融入良性且语义相关的证据，转移LLM对后门特征的注意。在5个广泛使用LLM的分类与生成领域实验中，LETHE在对抗8种后门攻击时超越8个SOTA防御基线，将高级后门攻击成功率最高降低98%并保持模型效用，且成本高效、对自适应后门攻击鲁棒。


### Distributed Synthesis of Differentially Private Tabular Datasets
- **作者**: Yucheng Fu, Tianyao Gu and Elaine Shi, Tianhao Wang
- **论文 PDF**: [Distributed Synthesis of Differentially Private Tabular Datasets](https://www.usenix.org/system/files/usenixsecurity26-fu.pdf)
- **解读**: 差分隐私合成数据生成可在保护个体隐私的同时共享数据，但当敏感数据属性分布在医院、公司或政府等多个实体间时，难以在不汇集完整私有数据集的情况下捕获有信息量的统计相关性并指导合成。作者提出分布式设置下差分隐私表格数据合成的安全多方计算协议，含两个新原语：利用分布式点函数高效估计垂直分布数据上的两路边际（属性两两联合分布），以及经累积分布函数表的批量查找生成噪声。作为具体演示，作者构建了SOTA DP数据合成算法AIM的分布式版本，达到与集中式相同的效用，端到端运行时比先前工作降低数个数量级——例如在真实WAN设置下24分钟合成Adult数据集，而现有协议估计需57天。


### Trustworthy and Confidential SBOM Exchange
- **作者**: Eman Abu Ishgair and Chinenye Okafor, Marcela S. Melara, Santiago Torres-Arias
- **论文 PDF**: [Trustworthy and Confidential SBOM Exchange](https://www.usenix.org/system/files/usenixsecurity26-abu-ishgair.pdf)
- **解读**: 软件物料清单（SBOM）已成为提升软件供应链安全与信任的监管要求，但企业与受监管软件厂商出于知识产权或漏洞信息等考虑，常希望限制谁能查看SBOM中的机密软件元数据。为调和透明度与机密性的张力，作者提出Petra SBOM交换系统，让软件厂商用选择性加密可互操作地组合与分发经删减的SBOM数据，消费者可搜索删减SBOM回答特定安全问题而不泄露未获授权的信息；Petra利用格式无关、防篡改的SBOM表示生成高效且保密的完整性证明，使相关方可密码学审计并建立对删减SBOM的信任。原型中交换删减SBOM每份仅需不到1 KB额外开销，SBOM解密至多占一次SBOM查询性能开销的1%。


### Libra: Pattern-Scheduling Co-Optimization for Cross-Scheme FHE Code Generation over GPGPU
- **作者**: Song Bian, Mingzhe Zhang, Zhenyu Guan
- **论文 PDF**: [Libra: Pattern-Scheduling Co-Optimization for Cross-Scheme FHE Code Generation over GPGPU](https://www.usenix.org/system/files/usenixsecurity26-bian-song.pdf)
- **解读**: 单个应用内利用多种FHE方案可提升整体效率，但把跨方案FHE算子映射到GPGPU等高性能架构仍具挑战。Libra编译框架集成FHE计算模式与硬件感知调度策略建立算法-硬件协同优化：定义新颖的跨方案FHE表示以抽象各FHE方案的公共程序模式，基于多方案切换模式派生的FHE原语组合执行成本动态优化输出程序，并引入桥接高层计算特性与低层执行计划的计算调度策略加速GPU上算子间执行。实验表明Libra在微基准上相比SOTA跨方案实现最高270倍加速、应用上19倍，同时提升计算单元与内存带宽利用率44%与36.1%。


### Efficient and High-Accuracy Secure Two-Party Protocols for a Class of Functions with Real-number Inputs
- **作者**: Hao Guo and Zhaoqian Liu, Liqiang Peng, Shuaishuai Li, Ximing Fu, Weiran Liu and Lin Qu
- **论文 PDF**: [Efficient and High-Accuracy Secure Two-Party Protocols for a Class of Functions with Real-number Inputs](https://www.usenix.org/system/files/usenixsecurity26-guo-hao.pdf)
- **解读**: 两方秘密共享方案中数值通常编码为无符号整数，而实际应用常需对有符号实数计算；USENIX'25上Guo等提出从共享计算有符号整数int(x)并可扩展至实数，但强加|x| < L/3的受限输入约束。本文将该约束显著放宽为对任意B ≤ L/2（B = L/2即Z_L中的自然可表示范围），从而支持宽松或无输入约束的实数计算，并基于此给出覆盖整数除法、sin(x)、e^(-x)等函数大类的安全协议通用设计框架。实验表明协议高效且高精度：e^(-x)协议通信量降至SirNN（S&P'21）与Bolt（S&P'24）的约31%，运行加速分别达5.53倍与3.09倍，最大ULP误差1.435（SirNN为2.64、Bolt为8.681）。


### CompLeak: Deep Learning Model Compression Exacerbates Privacy Leakage
- **作者**: Na Li, Yansong Gao, Hongsheng Hu, Boyu Kuang, Anmin Fu, and School of Computer Science and Engineering
- **论文 PDF**: [CompLeak: Deep Learning Model Compression Exacerbates Privacy Leakage](https://www.usenix.org/system/files/usenixsecurity26-li-na.pdf)
- **解读**: 模型压缩对深度学习模型降低存储与加速推理至关重要，但压缩带来的隐私风险被忽视。作者聚焦典型分类任务，以成员推断攻击（MIA）为视角提出CompLeak——首个考察剪枝、量化与权重聚类三种常用压缩配置的隐私风险评估框架，覆盖Google TF-Lite（三种全部支持）、Facebook PyTorch Mobile与微软NNI（前两种）等商业压缩框架；CompLeak有三个变体：NR单独攻击每个压缩模型并发现不同压缩模型对成员与非成员影响不同，SR在原始模型与一个压缩模型可用时以压缩模型为参考、结合两者元信息（如置信向量）揭示更多隐私，MR在多个压缩模型可用（有无原始模型均可）时创新地从多个压缩版本利用泄漏信息显著放大整体隐私泄漏。在六种架构（ResNet到BERT与GPT-2）与五个图像/文本基准数据集上的实验表明MR在所有指标（含TPR@0.1%FPR）上取得最佳MIA性能，证明模型压缩加剧隐私泄漏。


### TAT: Attesting Trajectory Integrity of Industrial Robotic Arms
- **作者**: Chengtao Yao
- **论文 PDF**: [TAT: Attesting Trajectory Integrity of Industrial Robotic Arms](https://www.usenix.org/system/files/usenixsecurity26-yao-chengtao.pdf)
- **解读**: 工业机械臂在现代制造中广泛部署于关键领域，运动是主要安全关切——对生产逻辑、定位或动力学的对抗性篡改可导致产品缺陷或物理损坏；远程证明是验证执行完整性的有前景机制，但现有方法聚焦控制流或数据流属性、无法捕获运动语义，难以充分验证机械臂的物理执行。作者提出轨迹完整性（TI）新安全属性，确保机械臂运动符合预期路径，并设计TAT最小侵入式证明框架：用Timed Motion Event Graph捕获运动语义，结合事件与关节测量验证实际运动。在开源机械臂平台上的软硬件原型评估显示TAT内存开销至多2.30%、执行时间开销0.14%，并在多种运动相关参数修改下验证了轨迹完整性证明的有效性。


### VidLeaks: Membership Inference Attacks Against Text-to-Video Models
- **作者**: Li Wang and Wenyu Chen, Ning Yu, Zheng Li and Shanqing Guo
- **论文 PDF**: [VidLeaks: Membership Inference Attacks Against Text-to-Video Models](https://www.usenix.org/system/files/usenixsecurity26-wang-li.pdf)
- **解读**: 在巨型网络规模数据上训练的文本到视频（T2V）模型引发版权与隐私关切，成员推断攻击（MIA）是审计此类风险的原则性工具，但面向静态数据（图像或文本）的现有技术无法捕获视频生成的时空复杂性，尤其忽视关键帧中记忆信号的稀疏性与随机时序动态引入的不稳定性。作者开展针对T2V模型MIA的首个系统研究，提出VidLeaks框架，通过两个互补信号探测稀疏时空记忆：空间重建保真度（SRF，用Top-K相似度放大稀疏记忆关键帧的空间记忆信号）与时间生成稳定性（TGS，跨多次查询测语义一致性以捕获时间泄漏）。在三种渐次受限的黑盒设置（监督、基于参考、仅查询）下评估三个代表性T2V模型，发现严重漏洞：即使严格仅查询设置，VidLeaks在AnimateDiff上AUC达82.92%、InstructVideo上达97.01%，构成现实可利用的隐私风险，为审计视频生成系统与开发防御提供首个具体证据。


### When Fun Turns Toxic: A First Look at Aggressive Advertising in Mini-games
- **作者**: Pei Chen, Ziru Zhao
- **论文 PDF**: [When Fun Turns Toxic: A First Look at Aggressive Advertising in Mini-games](https://www.usenix.org/system/files/usenixsecurity26-chen-pei.pdf)
- **解读**: 小游戏已成为超级应用生态中的主导范式，但官方广告接口的易集成与监管不足导致激进且可能具有欺骗性的广告实践，严重损害用户体验；激进广告虽非恶意软件，却通过滥用合法API绕过审计、操纵用户交互、破坏平台信任，构成系统性安全风险。作者开展首个小游戏激进广告的系统性安全分析：分析九个小游戏平台的平台策略与开发者能力并刻画激进广告行为，设计可扩展检测框架MAAD，在微信、Facebook Instant Games与Quickgame三大平台大规模测量，发现49.95%的小游戏存在激进广告（包括超过10万条用户评论的热门作品）。分析还揭示游戏特定触发、过度弹窗频率与误导策略等扰乱性行为模式及对抗性绕过技术，表明激进广告是利用当前执法结构性盲区的普遍平台滥用形式。


### When Memory Becomes a Vulnerability: Towards Multi-turn Jailbreak Attacks against Text-to-Image Generation Systems
- **作者**: Shiqian Zhao, Jiayang Liu, and Institute of Science Tokyo, Yiming Li, Wenshu Fan, Xiaobao Wu and Xinfeng Li, Jie Zhang, Wei Dong and Tianwei Zhang, Luu Anh Tuan
- **论文 PDF**: [When Memory Becomes a Vulnerability: Towards Multi-turn Jailbreak Attacks against Text-to-Image Generation Systems](https://www.usenix.org/system/files/usenixsecurity26-zhao-shiqian.pdf)
- **解读**: 现代文本到图像（T2I）生成系统（如DALL·E 3）利用记忆机制在多轮交互中捕获关键信息以保证生成保真，但其安全分析远落后于实践，作者揭示该机制会加剧越狱攻击风险：先前攻击把不安全目标提示融合进单一最终对抗提示，易被检测或因过度/不足去毒而生成非不安全图像。作者提出Inception——首个显式利用真实T2I系统记忆机制的多轮越狱攻击，在会话开始就把恶意植入记忆，由两个模块组成：Segmentation用NLP分析技术依据句子结构把提示连同恶意意图分解为多轮提示以规避安全过滤器；Recursion处理无法经简单分割分离的不安全子提示，先展开子提示再递归调用分割。为方便构造多轮对抗提示，作者构建集成两阶段安全过滤器与工业级记忆机制的仿真T2I系统VisionFlow，实验显示Inception诱导不安全图像生成的成功率超过SOTA达20.0个百分点，并在真实商业T2I平台验证了威胁。


### SoK: Security of Cyber-physical Systems Under Intentional Electromagnetic Interference Attacks
- **作者**: Qinhong Jiang, Yan Long, Youqian Zhang, Chen Yan and Xiaoyu Ji, Xiapu Luo, Kevin Fu, Jiannong Cao, Wenyuan Xu
- **论文 PDF**: [SoK: Security of Cyber-physical Systems Under Intentional Electromagnetic Interference Attacks](https://www.usenix.org/system/files/usenixsecurity26-jiang-qinhong.pdf)
- **解读**: 作为物理与数字世界之间的网关，对计算机系统的电信号进行伪造——蓄意电磁干扰（IEMI）攻击——因其能破坏或控制广泛的安全与安全关键应用而日益普遍和具有破坏性；现有IEMI研究高度设备特定、攻击向量分散且比较不足，缺乏统一的基于模型的理解，阻碍可迁移的安全评估与可部署防护的跨学科协作。作者分析80多个IEMI攻击与防御实例，提供分析框架建模对手如何实现IEMI耦合与采样操纵以注入恶意电磁能量、改变硬件行为并影响软件执行，目标是推动领域从穷举式经验发现转向深入理论分析与适用于现有及未来CPS的主动防御策略。论文在识别现有IEMI攻防研究空白的同时，为不同利益相关方社区勾勒未来研究方向，并发布开源IEMI研究数据库。


### Fend for Yourself! Backdoor Purification in Federated Graph Learning with an Evolving Knowledge Anchor
- **作者**: Chengcheng Zhu and Yunlong Mao, Jiale Zhang and Bosen Rao, Sheng Zhong
- **论文 PDF**: [Fend for Yourself! Backdoor Purification in Federated Graph Learning with an Evolving Knowledge Anchor](https://www.usenix.org/system/files/usenixsecurity26-zhu-chengcheng.pdf)
- **解读**: 联邦图学习（FedGL）在保护隐私的同时支持去中心化图数据协作训练，但其分布式特性使其极易受后门攻击（注入恶意触发器破坏全局模型完整性）；现有防御要么对复杂图数据无效，要么依赖可信服务器，与现代隐私保护技术存在架构冲突。作者提出GBHINDER——实用且无需可信服务器的防御框架，让每个良性参与方自我保护：以自身可信历史知识作为良性锚净化下载的全局模型，再选择性吸收全局模型的良性知识使锚渐进进化，形成良性循环；具体由两个组件驱动——历史通道注意力正则化用锚约束全局模型表征并阻断后门传播，自适应动量信息更新动态整合鲁棒全局信息使锚安全进化并随联邦迭代保持有效。在多个基准数据集上GBHINDER显著超越SOTA防御，将后门攻击成功率降至10%以下同时保持主任务高精度。


### InstantOMR: Oblivious Message Retrieval with Low Latency and Optimal Parallelizability
- **作者**: Haofei Liang, Zeyu Liu, Eran Tromer, Xiang Xie, Yu Yu
- **论文 PDF**: [InstantOMR: Oblivious Message Retrieval with Low Latency and Optimal Parallelizability](https://www.usenix.org/system/files/usenixsecurity26-liang.pdf)
- **解读**: 匿名消息系统与私有区块链中消息检索代价高昂，不透明消息检索（OMR）让资源受限的接收者外包消息检测与检索同时保持隐私。InstantOMR将TFHE函数自举与标准RLWE运算结合为混合设计，专门优化低延迟与高并行度；基于Primus-fhe库的实现（及基于TFHE-rs的估计）显示其关键优势：延迟比SOTA单服务器OMR方案SophOMR低约600倍，在流式设置（检测器在线处理到达消息并在接收者上线时立即返回摘要）中直接以相同因子缩短接收者等待时间；且因消息独立处理可近最优地随CPU核心数扩展，高核心数下比受BFV限制并行度的SophOMR更快。


### Cracks in the Walled Garden: Dissecting the Gray-Market of Unauthorized iOS App Distribution via Ad Hoc Sideloading
- **作者**: Yijing Liu
- **论文 PDF**: [Cracks in the Walled Garden: Dissecting the Gray-Market of Unauthorized iOS App Distribution via Ad Hoc Sideloading](https://www.usenix.org/system/files/usenixsecurity26-liu-yijing.pdf)
- **解读**: Apple执行严格代码签名并强制经官方App Store分发，但未授权应用仍经侧载渠道传播：原本面向开发者测试的Ad Hoc provisioning机制演变为绕过审核的结构化灰市，连接证书转售、第三方签名工具与未签名.ipa文件分发。作者开展该市场的首个系统研究，聚焦其在中国的一体化服务运作：通过以用户为中心的数据收集识别3,359个活跃证书兑换签名站点，逆向12个签名工具，获取8,216个分发.ipa条目。分析揭示转售利润率高达3000%的多层证书流通模型与签名工具常用的代码签名技巧；多数分发应用是经动态库注入定制功能的合法应用修改版，破坏应用与系统为用户提供的安全防护，使其暴露于未授权操作、敏感数据外泄与系统能力利用等风险。


### ZipPIR: High-throughput Single-server PIR without Client-side Storage
- **作者**: Rasoul Akhavan Mahdavi
- **论文 PDF**: [ZipPIR: High-throughput Single-server PIR without Client-side Storage](https://www.usenix.org/system/files/usenixsecurity26-mahdavi.pdf)
- **解读**: 私有信息检索（PIR）让客户端不泄露访问位置地私密访问数据库；基于环LWE的早期PIR方案吞吐有限，而高吞吐方案要么依赖需大量客户端存储的离线阶段（如SimplePIR的提示），要么离线通信成本过高（如Piano），且动态数据库中提示需昂贵地重新生成与重传。ZipPIR将LWE密文压缩为显著更小的Paillier密文，在离线阶段获得尺寸缩减而不把计算成本带入在线阶段，并在计算假设下实现近乎静默的离线阶段——除初始公钥外无需通信，服务器可在空闲时独立生成与更新提示。ZipPIR吞吐超过2 GB/s（与SimplePIR相当）而无需大客户端提示；对1 GB数据库，其吞吐比无客户端存储的现有协议高至10倍，每客户端服务器存储低于200 KB，显著提升实际部署的可扩展性，也是首个吞吐与SOTA PIR协议竞争的Paillier PIR方案。


### Provable Secure Steganography Based on Adaptive Dynamic Sampling
- **作者**: Kaiyi Pang and Minhao Bai
- **论文 PDF**: [Provable Secure Steganography Based on Adaptive Dynamic Sampling](https://www.usenix.org/system/files/usenixsecurity26-pang-kaiyi_0.pdf)
- **解读**: 监控的普遍存在使私密通信安全日益受损，隐写术把秘密消息嵌入无害载体以实现受监控信道上的隐蔽通信；可证明安全隐写术（PSS）保证正常模型输出与隐写输出计算不可区分，是该领域的最新技术，但现有PSS方法常需获得模型的显式分布。作者提出仅需接受种子输入的模型API即可实现的可证明安全隐写方案：核心机制是采样候选token集合并构造消息位串到这些token的映射，把真实秘密消息经该映射选出输出token，可证明保持原模型分布；为保证正确解码，通过维护并在有界大小范围内策略性扩展动态碰撞集来处理多个候选消息映射到同一token的碰撞情形。在三个真实数据集与三个大语言模型上的广泛评估显示，该基于采样的方法在效率与容量上与现有PSS方法相当。


### Arguzz: Testing zkVMs for Soundness and Completeness Bugs
- **作者**: Christoph Hochrainer, Valentin Wüstholz, Maria Christakis
- **论文 PDF**: [Arguzz: Testing zkVMs for Soundness and Completeness Bugs](https://www.usenix.org/system/files/usenixsecurity26-hochrainer.pdf)
- **解读**: 零知识虚拟机（zkVM）越来越多地部署于去中心化应用与区块链rollup，执行通用程序（常以Rust编写）并生成简洁密码学证明；但zkVM复杂，约束系统或执行逻辑的bug可导致关键的音性（接受无效执行）或完备性（拒绝有效执行）问题。ARGUZZ是首个自动化测试zkVM音性与完备性bug的工具，结合变形测试的新变体与故障注入：生成语义等价的程序对并合并为已知输出的单一Rust程序在zkVM内运行，通过向VM注入故障模拟恶意或有bug的证明者，揭示过弱的约束。作者用ARGUZZ测试六个真实zkVM——RISC Zero、Nexus、Jolt、SP1、OpenVM与Pico，在其中三个发现11个bug，其中一个RISC Zero bug在先前已审计的情况下仍获$50,000赏金，证明系统化测试zkVM的迫切需求。


### kSFS: Repurposing a Microkernel-like Interface for Fast and Secure In-Kernel Linux File Systems
- **作者**: Dinglan Peng and Pedro Fonseca
- **论文 PDF**: [kSFS: Repurposing a Microkernel-like Interface for Fast and Secure In-Kernel Linux File Systems](https://www.usenix.org/system/files/usenixsecurity26-peng-dinglan.pdf)
- **解读**: 文件系统广泛使用且至关重要，但以复杂著称、是操作系统漏洞的主要来源；近期工作提出内核内沙箱技术隔离包括文件系统在内的内核组件，但常忽视一个定义明确、安全且所有不可信与可信内核组件交互都需对照强威胁模型验证的边界，Linux文件系统尤其如此——它依赖庞大复杂接口并与VFS、块设备等多个内核子系统交互。kSFS将Linux面向用户空间文件系统的微内核风格接口FUSE协议改造为不可信沙箱内核文件系统的安全接口，提供强隔离保证，并把WebAssembly推广到内核空间作为通用沙箱机制，以最小移植工作量兼容现有用户空间文件系统实现——例如从用户空间移植NTFS与exFAT实现仅需修改少于300行代码。kSFS在安全性与可靠性优于Linux文件系统实现的同时性能显著优于用户空间对应物：真实应用tar与RocksDB上kSFS NTFS实现比用户空间基线分别最高提升29%与60倍，仅比不安全的Linux实现低0%至52%。


### Sirens' Whisper: Inaudible Near-Ultrasonic Jailbreaks of Speech-Driven LLMs
- **作者**: Zijian Ling, Pingyi Hu, Man Zhou, Jun Feng and Songfeng Lu, Dongmei Zhang and Bin Benjamin Zhu
- **论文 PDF**: [Sirens' Whisper: Inaudible Near-Ultrasonic Jailbreaks of Speech-Driven LLMs](https://www.usenix.org/system/files/usenixsecurity26-ling.pdf)
- **解读**: 语音驱动LLM经语音接口访问日益普及，开放声学信道引入新的安全风险。Sirens' Whisper（SWhisper）是首个在现实黑盒条件下用商品硬件对语音驱动LLM实施隐蔽提示攻击的实用框架：把任意目标基带音频（含长而结构化的提示）编码进近超声波形，经声学传输与麦克风非线性忠实解调，通过对设备与环境非线性信道特性的简单有效建模结合轻量信道逆预补偿实现鲁棒不可闻投递；在此高保真隐蔽信道之上设计语音感知的越狱生成方法，保证语音接口下的可理解性、简洁性与可迁移性。跨商业与开源语音驱动LLM的实验显示强黑盒有效性：商业模型上非拒绝率（NR）最高0.94、特定说服力（SC）0.925，受控用户研究证明注入的越狱音频对人类听者与纯背景播放知觉上不可区分；越狱只是案例研究，底层隐蔽声学信道支持更广的高保真提示注入与命令执行攻击。


### Autonomy Comes with Costs: Detecting Denial-of-Service Vulnerabilities Caused by Resource Abusing in LLM-based Agents
- **作者**: Jiaqi Luo, Xudong Pan, Yuan Zhang
- **论文 PDF**: [Autonomy Comes with Costs: Detecting Denial-of-Service Vulnerabilities Caused by Resource Abusing in LLM-based Agents](https://www.usenix.org/system/files/usenixsecurity26-luo.pdf)
- **解读**: LLM智能体利用大语言模型的语义理解能力根据用户请求自主执行下载文件、总结内容等复杂任务，但缺乏全面的资源治理使其易受滥用，可能导致资源耗尽与拒绝服务（DoS）。作者开展LLM智能体资源管理的首个系统性安全研究，识别三种有代表性的资源生命周期管理模式，每种都启用不同的DoS利用途径，并提出AgentDoS——新型定向灰盒模糊测试框架：先分析智能体内资源生命周期，再用LLM生成自然语言的功能特定种子提示驱动智能体过度消耗资源。在20个广泛使用的开源LLM智能体上评估发现36个影响16个智能体的0-day漏洞（其中15个在GitHub上拥有超过10,000星），迄今已获15个CVE编号。


### Overcoming the Retrieval Barrier: Indirect Prompt Injection in the Wild for LLM Systems
- **作者**: Hongyan Chang
- **论文 PDF**: [Overcoming the Retrieval Barrier: Indirect Prompt Injection in the Wild for LLM Systems](https://www.usenix.org/system/files/usenixsecurity26-chang-hongyan.pdf)
- **解读**: LLM日益依赖从外部语料检索信息，创造出间接提示注入（IPI）新攻击面；先前研究常回避最困难的一步——确保恶意内容真正被检索，而实践中未优化的IPI在自然查询下很少被检索，使其现实影响不明。作者将恶意内容分解为保证检索的触发片段与编码任意攻击目标的攻击片段，基于此设计高效黑盒攻击算法，构造紧凑触发片段以保证任何攻击片段都被检索；攻击仅需嵌入模型API访问，成本低至每目标用户查询$0.21（OpenAI嵌入模型），在11个基准与8个嵌入模型（含开源与专有服务）上实现接近100%的检索率。基于该攻击呈现自然查询与真实外部语料下首个端到端IPI利用，覆盖RAG与智能体系统及多种攻击目标——当用户用自然查询总结高频主题邮件时，单封投毒邮件即可在多智能体工作流中以超过80%成功率诱使GPT-4o外泄SSH密钥；作者评估的多种防御均不足以阻止恶意文本被检索，凸显检索是关键开放漏洞。


### Inference Attacks Against Graph Generative Diffusion Models
- **作者**: Xiuling Wang and Xin Huang, Guibo Luo, Jianliang Xu
- **论文 PDF**: [Inference Attacks Against Graph Generative Diffusion Models](https://www.usenix.org/system/files/usenixsecurity26-wang-xiuling.pdf)
- **解读**: 图生成扩散模型已成为生成复杂图结构、有效捕获图数据中复杂依赖与关系的强大范式，但其隐私风险基本未被探索。作者通过三类黑盒推断攻击研究此类模型的信息泄漏：图重建攻击可从生成图重建与训练图结构相似的图；属性推断攻击从生成图推断训练图属性（如平均密度与密度分布）；两种成员推断攻击判断给定图是否在训练集中。在三种图生成扩散模型与六个真实图上的大量实验显示这些攻击有效且显著超越基线；作者还提出两种防御机制，在防御强度与目标模型效用之间取得比现有方法更好的权衡。


### Unlocking the True Potential of Decryption Failure Oracles: A Hybrid Adaptive-LDPC Attack on ML-KEM Using Imperfect Oracles
- **作者**: Qian Guo
- **论文 PDF**: [Unlocking the True Potential of Decryption Failure Oracles: A Hybrid Adaptive-LDPC Attack on ML-KEM Using Imperfect Oracles](https://www.usenix.org/system/files/usenixsecurity26-guo-qian.pdf)
- **解读**: 利用明文检查（PC）与解密失败（DF）预言机的侧信道攻击是对已部署后量子密码的紧迫威胁，可基于时序、功耗与微架构行为等可触及泄漏源实例化。作者重新审视ML-KEM上利用DF预言机的选择密文侧信道攻击，论证其潜力长期未获实现：提出结合自适应查询生成与低密度奇偶校验（LDPC）码信念传播的新型攻击框架，跨多个秘密系数精心构造平衡奇偶校验，即使存在显著噪声也最大化每次预言机查询提取的Shannon信息，大幅减少全密钥恢复所需查询数、接近理论Shannon信息界（ML-KEM-768在95%预言机精度下仅需2950次查询，为Shannon下界的1.35倍），确立设计良好的DF攻击可超越SOTA二进制PC攻击的效率。作者将框架应用于近期GoFetch攻击验证实际影响：所需测量迹减少一个数量级以上并省去昂贵的后处理，使此前认为棘手的更高安全级别方案实现全密钥恢复。


### vCause: Efficient and Verifiable Causality Analysis for Cloud-based Endpoint Auditing
- **作者**: Qiyang Song, and School of Cyber Security, Wenbo Jiang, Heqing Huang, Yong Liu, Dan Meng, and School of Cyber Security
- **论文 PDF**: [vCause: Efficient and Verifiable Causality Analysis for Cloud-based Endpoint Auditing](https://www.usenix.org/system/files/usenixsecurity26-song-qiyang.pdf)
- **解读**: 云端端点审计中，安全管理员常依赖云对日志派生的版本化溯源图执行因果分析以调查可疑攻击行为，但云可能不可信或被攻陷而操纵最终因果分析结果，使管理员无法准确理解攻击行为；现有防篡改日志方案与可信执行环境并非为支持因果分析而设计，存在固有安全与效率局限。作者提出VCAUSE，高效可验证的云端端点审计因果分析系统，集成两种认证数据结构——图累加器与可验证溯源图——以验证因果分析的两个关键步骤：在版本化溯源图上查询关注节点，以及识别其因果相关组件。形式化安全分析与实验评估显示，VCAUSE仅以端点上<1%与云上3.36%的计算开销实现安全可验证的因果分析。


### CombiSan: Unifying Software Sanitizers for Comprehensive Fuzzing
- **作者**: Matteo Marini, Floris Gorter, Daniele Cono D'Elia, Cristiano Giuffrida
- **论文 PDF**: [CombiSan: Unifying Software Sanitizers for Comprehensive Fuzzing](https://www.usenix.org/system/files/usenixsecurity26-marini.pdf)
- **解读**: 现代C/C++漏洞检测严重依赖结合软件消毒器的模糊测试，但最流行的消毒器互操作性有限，开发者常孤立地逐个启用，需要多次运行，牺牲性能并以非统一方式测试代码。CombiSan是面向模糊测试优化的消毒器，同时检测ASan、MSan与UBSan三大流行消毒器覆盖的全部可寻址性、未初始化内存与其他未定义行为问题：统一影子内存设计高效跟踪程序内存每字节的可寻址性与初始化状态，插桩与SOTA的其他未定义行为检测无缝集成，并因不同消毒器发现的bug可能提前终止执行而互相掩盖，将聚合问题的分析推迟到测试用例完成。评估中CombiSan在OSS-Fuzz每日测试的10个程序中检测到81个新bug，平均比顺序运行ASan+UBSan与MSan快1.7倍，且以显著更少的CPU小时取得与这些消毒器相同的bug检测精度。


### KernelRCA: Facilitating Root Cause Analysis of Memory Corruptions in Linux Kernel with Contextual Causality Chain
- **作者**: Kangzheng Gu
- **论文 PDF**: [KernelRCA: Facilitating Root Cause Analysis of Memory Corruptions in Linux Kernel with Contextual Causality Chain](https://www.usenix.org/system/files/usenixsecurity26-gu-kangzheng.pdf)
- **解读**: 持续模糊测试基础设施发现大量bug，自动根因分析（RCA）被提出以减少理解bug根因的昂贵人工，但现有根因表示是孤立形式，分析者仍需手动推断集成调用上下文与数据依赖的bug触发过程，对复杂OS内核尤为困难。作者提出上下文因果链（CC-chain）根因表示，直观反映Linux内核内存损坏集成的bug触发过程：显示导致bug的贡献指令以解释对应的意外行为，以及这些指令间的调用上下文与数据依赖，帮助分析者快速理解bug如何发生；为自动构造CC-chain设计包含选择性追踪、上下文信息恢复与链式根因分析的KernelRCA系统。KernelRCA成功诊断54种Linux内核真实内存损坏，优于现有崩溃报告与KASAN报告；用户研究表明其报告显著促进人类分析者的bug理解与修复。


### Sy-FAR: Symmetry-based Fair Adversarial Robustness
- **作者**: Haneen Najjar
- **论文 PDF**: [Sy-FAR: Symmetry-based Fair Adversarial Robustness](https://www.usenix.org/system/files/usenixsecurity26-najjar.pdf)
- **解读**: 人脸识别等安全关键的机器学习系统易受对抗样本（含现实可物理实现的攻击）影响，但提升对抗鲁棒性的方法通常产生不公平鲁棒性：从某些类（如个体）或组（如性别）攻击更易成功；已有技术追求类间完美公平，但聚焦汽车、船舶等安全与公平性较不关键的设置。作者洞察在现实公平关键任务（如人脸识别）中实现完美对等通常不可行——某些类（如兄弟姐妹）高度相似导致更多误分类，转而追求对称性（从类i到j的攻击与从j到i同样成功）更可行，因为类相似性在多数领域是对称关系；并理论证明个体间对称蕴含任意子组集合间的对称（其他公平概念中组公平常难以实现）。作者开发Sy-FAR技术鼓励对称性同时优化对抗鲁棒性，在五个数据集、三种模型架构上针对定向与非定向现实攻击的广泛评估显示其显著提升公平对抗鲁棒性、更快且跨运行更一致，并缓解本文新发现的另一类不公平——对抗样本易被分类到的目标类在诱导对称后变得显著更不易受攻击。


### Ajax: Fast Threshold Fully Homomorphic Encryption without Noise Flooding
- **作者**: Zhenkai Hu, Haofei Liang, Xiao Wang, Xiang Xie, Kang Yang, Yu Yu, Wenhao Zhang
- **论文 PDF**: [Ajax: Fast Threshold Fully Homomorphic Encryption without Noise Flooding](https://www.usenix.org/system/files/usenixsecurity26-hu-zhenkai.pdf)
- **解读**: 门限全同态加密（ThFHE）让多方在密钥分布于各方的情况下对加密数据执行任意计算，核心任务是构造FHE方案的阈值密钥生成与解密协议；FHEW类密码系统享有快速自举与小参数优势，但已知ThFHE方案用"噪声泛滥"实现阈值解密，需大参数或经自举切换到高参数方案，导致解密缓慢，而密钥生成要么假设通用MPC或可信设置，要么噪声随方数n线性增长。作者为FHEW类密码系统设计阈值密钥生成与解密协议Ajax：阈值解密提出基于不同环上随机双重共享的"mask-then-open"新技术，消除噪声泛滥同时保持小参数；阈值密钥生成在诚实多数设置（至多t=(n-1)/2方被腐化）把噪声增长从n倍降至max(0.038n,2)倍。端到端实现在1 Gbps带宽、1 ms ping下，n=3（resp. n=21）时生成一组密钥与解密单个密文分别耗时17.6 s与0.9 ms（resp. 91.9 s与4.4 ms）；相比SOTA实现，阈值解密协议端到端性能在t=1至t=13的不同网络延迟下提升至少5.7倍至283.6倍，方法还可应用于BGV、BFV与CKKS等其他FHE方案。


### FirmReBugger: A Benchmark Framework for Monolithic Firmware Fuzzers
- **作者**: Mathew Duong, Surya Nepal, Damith C. Ranasinghe
- **论文 PDF**: [FirmReBugger: A Benchmark Framework for Monolithic Firmware Fuzzers](https://www.usenix.org/system/files/usenixsecurity26-duong.pdf)
- **解读**: 单体固件普遍存在，固件模糊测试是活跃研究领域，但通过代码覆盖与唯一崩溃等指标理解和评估改进存在问题，促使需要可靠的基于bug的基准。作者设计并构建FirmReBugger，用现实多样、基于bug的基准公平评估单体固件模糊器的整体框架：提出bug oracle（bug描述符的C语法表达式）配合解释器自动化分析并准确报告发现的bug，区分已检测、已触发、已到达与未到达状态；基准化思想不修改目标二进制，通过重放模糊种子隔离基准实现与模糊器，并便于扩展新bug oracle。作者还创建FirmBench——含313个软件bug oracle的多样真实二进制目标集，以10 CPU年的努力在FuzzBench-for-Firmware式服务中用FirmBench评估9个SOTA单体固件模糊器，以复现性研究的方式报告发现。


### Differential Trust: Dynamic Multi-Authority Anonymous Credentials with Epoch-Weighted Updates
- **作者**: Chen Li, Jianting Ning, Xiulong Liu, Yulin Liu
- **论文 PDF**: [Differential Trust: Dynamic Multi-Authority Anonymous Credentials with Epoch-Weighted Updates](https://www.usenix.org/system/files/usenixsecurity26-li-chen.pdf)
- **解读**: 匿名凭证（AC）是隐私保护认证的基础构件，允许用户证明属性而不泄露身份；SOTA AC把凭证签发分布到多个权威机构（常采用Shamir秘密共享或聚合签名），增强鲁棒性并消除单点故障，但签发阶段把全部权威等同对待，忽视不同权威可信度或权益的差异——在权益证明网络等现代去中心化系统中，节点的固有信任差异化无法在签发过程中利用。作者提出带epoch权重多权威匿名凭证（MA-ACEW），首个在签发中考虑权威权重分布的MA-AC模型，并支持权威权重分布跨epoch变化时的高效凭证更新；核心是新颖的Epoch-Bound Pointcheval-Sanders签名（EB-PS）原语，把签名绑定到特定时间epoch，实现epoch内基于权重的签发与跨epoch高效非交互更新。作者形式化EB-PS的EUF-eCMA不可伪造性并在新STB-GPS假设下证明，再证明MA-ACEW构造满足不可伪造性、匿名性与盲目性；基准显示聚合128个部分凭证的出示平均仅需10.68 ms。


### From Texts to Rules: Generating Sigma Rules with Large Language Models from Cyber Threat Reports
- **作者**: Yongxin Cai, Jing Qiu, Qingming Li, Du Cheng, Lei Chen
- **论文 PDF**: [From Texts to Rules: Generating Sigma Rules with Large Language Models from Cyber Threat Reports](https://www.usenix.org/system/files/usenixsecurity26-cai.pdf)
- **解读**: 网络威胁报告（CTR）传递对安全系统检测规则至关重要的可操作情报，LLM凭解析与生成能力可成为CTR到规则的翻译桥梁，但CTR的高层抽象与规则的低层机器语义之间的语义鸿沟及领域特定约束从根本上阻碍准确检测规则生成。作者证明CTR中的shell命令可有效转换为安全系统的Sigma检测规则，提出SIGMERGE端到端框架，构建语义中间层作为桥梁，按语义层级组织三个模块：高层信息提取模块用多子序列算法与微调领域LLM实现准确的MITRE ATT&CK战术、技术与程序（TTP）及命令提取；中间层攻击描述生成模块用偏好优化微调与闭环自验证缓解语义鸿沟；机器层Sigma规则生成模块用参数优化检索算法应对领域特定约束。作者构建7个数据集训练并广泛实验，用23个指标对比16个基线与13个LLM，结合真实安全系统开展10个案例研究验证有效性与效率；SIGMERGE已向官方仓库贡献4条新Sigma规则且全部获正式接受。


### Static Detection of TOCTOU Bugs Caused by Kernel Races
- **作者**: Gui-Dong Han
- **论文 PDF**: [Static Detection of TOCTOU Bugs Caused by Kernel Races](https://www.usenix.org/system/files/usenixsecurity26-han.pdf)
- **解读**: TOCTOU（检查到使用时间）bug是内核代码中众所周知的安全问题，可绕过安全检查导致系统崩溃与提权等严重后果；作者对Linux内核补丁的研究表明内核竞态是内核TOCTOU bug最常见的根因，但受内核并发逻辑复杂性与线程调度非确定性影响，仍无系统化方法专门检测内核竞态导致的TOCTOU bug。作者设计KERAT，首个系统化静态检测内核竞态导致TOCTOU bug的方法：此类bug由特定共享变量check-use操作的原子性违反引入，故KERAT从内核代码静态挖掘并检查共享变量原子性规则——两个关键技术：原子性规则挖掘方法有效识别哪个锁应保护哪个共享变量的check-use操作，以及基于状态机的验证策略，用常见bug模式的状态机编码检测违反所挖掘原子性规则的TOCTOU bug。在Linux-6.8与FreeBSD-14.1上评估发现351个真实bug，其中287个被识别为有害，65个获内核开发者确认，10个已获CVE编号。


### FABRICKED: Misconfiguring Infinity Fabric to Break AMD SEV-SNP
- **作者**: Benedict Schlüter
- **论文 PDF**: [FABRICKED: Misconfiguring Infinity Fabric to Break AMD SEV-SNP](https://www.usenix.org/system/files/usenixsecurity26-schlueter-fabricked.pdf)
- **解读**: 机密计算在真实云部署中日益普及。作者提出FABRICKED——操纵互连路由以攻破AMD SEV-SNP的新攻击：软件对手通过重定向互连事务，可误导安全协处理器使系统被错误地初始化；该原语在受害地址空间内获得任意读写，从而违反机密性与完整性保证，并能伪造证明报告。

### OS-Sanitizer: System-wide Latent Defect Inference in Linux Applications
- **作者**: Addison Crump, Thorsten Holz
- **论文 PDF**: [OS-Sanitizer: System-wide Latent Defect Inference in Linux Applications](https://www.usenix.org/system/files/usenixsecurity26-crump.pdf)
- **解读**: 传统动态测试只能通过触发失败或非期望状态来发现问题，无法仅凭良性行为推断软件可能做坏事。OS-Sanitizer 利用 eBPF 同时采集用户态与内核态事件并在内核中处理，用启发式规则在系统范围内推断缺陷的存在，把静态测试中"代码异味"的思想引入动态测试，从而发现只在特定环境下才触发故障、难以测试的潜在情境缺陷。评估从性能、复杂度、可维护性和使用性等角度区分了 eBPF 的理论极限与原型的具体局限；针对已知类型的软件缺陷，在广泛使用的应用中发现 40 多个问题（含严重漏洞），其中一些已存在十余年且出现在大多数 Linux 发行版上，证明动态缺陷推断可行且有效。


### SafeFFI: Efficient Sanitization at the Boundary Between Safe and Unsafe Code in Rust and Mixed-Language Applications
- **作者**: Oliver Braunsdorf and Tim Lange, Konrad Hohentanner and Julian Horsch, Johannes Kinder
- **论文 PDF**: [SafeFFI: Efficient Sanitization at the Boundary Between Safe and Unsafe Code in Rust and Mixed-Language Applications](https://www.usenix.org/system/files/usenixsecurity26-braunsdorf.pdf)
- **解读**: 与 C/C++ 互操作及实现底层数据结构所需的 unsafe Rust 代码可能破坏本应内存安全的程序，而 sanitizer 即使对类型系统已保证安全的内存访问也会插入大量多余检查。SafeFFI 优化 Rust 二进制中的内存安全插桩，只在 unsafe 与 safe 代码的边界插入检查，把内存安全的强制执行从 sanitizer 移交给 Rust 类型系统；与以往依赖昂贵全程序分析的设计不同，其编译开销显著更低（2.01× 对比 5.91× 以上）。在一批流行 Rust crate 上，SafeFFI 最多削减 79.63% 的 sanitizer 检查，同时仍能检测出已知易受攻击 Rust 代码数据集中的全部内存安全违规。


### GateBreaker: Gate-Guided Attacks on Mixture-of-Expert LLMs
- **作者**: Lichao Wu, Stjepan Picek, Ahmad-Reza Sadeghi
- **论文 PDF**: [GateBreaker: Gate-Guided Attacks on Mixture-of-Expert LLMs](https://www.usenix.org/system/files/usenixsecurity26-wu-lichao.pdf)
- **解读**: MoE 架构通过每输入只激活稀疏参数子集来扩展 LLM，但现有安全研究几乎只关注稠密架构，MoE 模块化、稀疏激活的设计使安全机制可能以不同方式运作。GateBreaker 是首个免训练、轻量、架构无关的推理时攻击框架，通过三阶段流程破坏现代 MoE LLM 的安全对齐：gate 级剖析定位在有害输入上被过度路由的安全专家、专家级定位安全结构、定向移除安全结构。研究表明 MoE 安全集中在由稀疏路由协调的小部分神经元中：仅禁用目标专家层最多 2.9% 的神经元，就能把 8 个最新对齐 MoE LLM 的平均攻击成功率（ASR）从 7.4% 提升到 64.9% 且效用损失有限；这些安全神经元在同一模型家族内可迁移（一次性迁移攻击使 ASR 从 17.9% 升至 67.7%），并能泛化到 5 个 MoE 视觉语言模型（不安全图像输入 ASR 达 60.9%）。


### Bridges to Self: Silent Web-to-App Tracking on Mobile via Localhost
- **作者**: Tim Vlummens, Aniketh Girish and Nipuna Weerasekara, Frederik Zuiderveen Borgesius and Gunes Acar, Narseo Vallina-Rodriguez
- **论文 PDF**: [Bridges to Self: Silent Web-to-App Tracking on Mobile via Localhost](https://www.usenix.org/system/files/usenixsecurity26-vlummens.pdf)
- **解读**: 浏览器与移动操作系统依赖沙箱和进程隔离分离 Web 与 App 上下文，但本文证明在 Android 上 Meta 和 Yandex 已实际打破这些隔离保证，用于跨上下文追踪。通过美国与欧盟视角的大规模网页爬取和系统性 Android 应用分析，作者刻画了一族此前未记录、利用 localhost 上 HTTP(S)、WebSocket、WebRTC 等 Web 标准连接移动与 Web 上下文的 Web 到 App 追踪范式；把伪匿名 Web cookie 关联到长期原生用户 ID，实现持久、隐蔽的跨上下文追踪与去匿名化，能绕过 cookie 清理、隐身模式、MAID 重置、VPN 及 Android 工作/个人配置文件隔离。研究发现 Meta Pixel 和 Yandex Metrica 在用户接受 cookie 同意横幅之前就发起 localhost 桥接；针对即将推出的 Local Network Access (LNA) 权限，作者还发现两条可绕过其保护的旁路（WebRTC 中的全局单播 IPv6 地址、*.local 域的 mDNS 查询），并结合法律分析指出平台与浏览器隔离原则、威胁与信任模型、协议标准及应用审查流程的结构性缺陷。


### WILD Attack: Stealthy Undermining of Wi-Fi-Based Geolocation Through Remote Crowdsourced Data Injection
- **作者**: Changjia Zhu
- **论文 PDF**: [WILD Attack: Stealthy Undermining of Wi-Fi-Based Geolocation Through Remote Crowdsourced Data Injection](https://www.usenix.org/system/files/usenixsecurity26-zhu-changjia.pdf)
- **解读**: 传统 WPS 欺骗攻击因缺乏隐蔽性和持久性而未引起重视，本文提出 WILD Attack，通过颠覆 WPS 的核心基础设施——位置查找表（LLT）——来破坏其安全性。攻击者远程为目标的 Wi-Fi 接入点提交伪造的众包报告，诱导 WPS 提供商基于伪造而非合法数据更新 LLT；对 Google、Apple、A-Map 和 WiGLE 四家广泛部署的提供商的分析表明，它们都接受伪造报告，并以不同策略解决合法与伪造数据的冲突。利用这些策略差异，攻击者可实现 LLT 条目篡改与 LLT 条目删除两种颠覆形式，且即使攻击者停止活动仍持续数周；文章还给出三个展示真实世界影响的案例研究并提出缓解对策。


### When Updates Backfire: A Black-Box Security Analysis of Desktop Software Update Mechanisms
- **作者**: Jie Wan
- **论文 PDF**: [When Updates Backfire: A Black-Box Security Analysis of Desktop Software Update Mechanisms](https://www.usenix.org/system/files/usenixsecurity26-wan.pdf)
- **解读**: 桌面软件更新是修补漏洞、交付安全修复的主要机制，但更新过程本身在更新数据校验不完整或执行不当时会引入新攻击面，且多数更新客户端闭源复杂、相关风险研究不足。作者提出黑盒框架 UpdSight，通过模拟真实环境中的 MitM 攻击来测试更新安全性：模拟中间人场景、拦截更新过程流量并自动验证关键弱点，结合流量拦截、载荷完整性检查和行为监控对各类软件的更新信任模型做全面评估。将 UpdSight 应用于 85 个广泛使用的桌面应用，发现 22 个可利用漏洞，包括降级、manifest 篡改、安装器劫持和路径遍历，其中 16 个已获厂商确认，5 个 CVE 编号覆盖 8 个软件产品；研究揭示未签名 manifest、弱回滚检查等反复出现的设计缺陷，使攻击者可通过更新通道获得代码执行。


### TopFeaRe: Locating Critical State of Adversarial Resilience for Graphs Regarding Topology-Feature Entanglement
- **作者**: Xinxin Fan, and University of Chinese Academy of Sciences, Wenxiong Chen, and State Key Laboratory of AI Safety, Quanliang Jing, Chi Lin, Shaoye Luo, and University of Chinese Academy of Sciences, Wenbo Song, and State Key Laboratory of AI Safety, Yunfeng Lu
- **论文 PDF**: [TopFeaRe: Locating Critical State of Adversarial Resilience for Graphs Regarding Topology-Feature Entanglement](https://www.usenix.org/system/files/usenixsecurity26-fan.pdf)
- **解读**: 图对抗攻击通常从拓扑/结构和节点特征两个角度发起，二者都是深度模型学习的关键特征，但现有防御未能揭示这两方面为何必需以及如何充分融合以联合学习图表示。作者借助复杂动力系统（CDS）中的平衡点理论，提出通过定位图对抗韧性临界状态来防御：把图系统映射为 CDS 并用动力系统振荡建模对抗扰动行为（对抗攻击建模），将图拓扑与节点特征投影为两个特征空间并定义二维纠缠扰动函数表示对抗攻击下的动态变化（2D 拓扑-特征纠缠函数），再利用平衡点理论结合扰动反映的二维函数定位攻击韧性临界状态。在五个常用真实数据集上针对四种代表性图对抗攻击的多方面实验表明，该方法显著优于现有最先进基线。


### BatchBoot: Fast Batched Bootstrapping for TFHE scheme and Practical Applications
- **作者**: Zhihao Li, Hongyu Wang, Yuan Zhao and Lichun Li, Zhiwei Wang, Jiaxing He, Lifeng Guo
- **论文 PDF**: [BatchBoot: Fast Batched Bootstrapping for TFHE scheme and Practical Applications](https://www.usenix.org/system/files/usenixsecurity26-li-zhihao.pdf)
- **解读**: TFHE 以独特的 bootstrapping 机制实现任意计算并刷新噪声预算，但一次只能处理单个加密消息，扩展性受限；近年提出的批处理 bootstrapping 方案可并行处理密文并带来摊销收益，但仍缺乏深入探索。BatchBoot 从三个方面改进：重设计同态多项式乘法这一核心子模块以大幅减少对昂贵 FFT 运算的依赖、提出支持灵活打包规模的稀疏感知消息打包策略、把 functional bootstrapping 扩展为 circuit bootstrapping 以增强支持函数的表达能力。BatchBoot 相比最先进的批处理方案（Guimarães et al., CCS'25）实现 2.4× 加速，相比非批处理 TFHE-rs 实现提升 43.8×；在应用层面，首次提出不平衡场景下基于 TFHE 的 PSI 协议（相比最优 BFV 方案 PEPSI 通信成本降低 294×、加速 4.1×），并基于 BatchCBoot 设计 8 位 FHE 指令集，相比现有结果最高加速 5.4×。


### The Adverse Effects of Omitting Records in Differential Privacy: How Sampling and Suppression Degrade the Privacy–Utility Tradeoff
- **作者**: Àlex Miranda-Pascual, Javier Parra-Arnau, Thorsten Strufe
- **论文 PDF**: [The Adverse Effects of Omitting Records in Differential Privacy: How Sampling and Suppression Degrade the Privacy–Utility Tradeoff](https://www.usenix.org/system/files/usenixsecurity26-miranda-pascual.pdf)
- **解读**: 采样以隐私放大著称，且常被假定能通过允许降噪来提升 DP 机制的效用，本文证明这一假设有误：在同等隐私水平下度量效用时，采样作为预处理会在所有规范 DP 机制（Laplace、Gaussian、exponential、report noisy max）以及聚类等采样应用上稳定地带来效用损失。作者进一步把抑制推广为选择或省略记录的一般化方法，给出无界近似 DP 下任意抑制策略的隐私界理论分析，发现所测抑制策略同样无法改善隐私-效用权衡；出人意料的是，均匀采样尽管仍有损害，却成为最佳抑制方法之一。这些结果对 DP 实践中常见的预处理假设提出质疑。


### VeCT: Secure and Efficient Constant-Time Code Rewriting with Vector Extensions
- **作者**: Qisheng Jiang and Danfeng Zhang
- **论文 PDF**: [VeCT: Secure and Efficient Constant-Time Code Rewriting with Vector Extensions](https://www.usenix.org/system/files/usenixsecurity26-jiang-qisheng.pdf)
- **解读**: 时序信道允许攻击者通过分析执行时间提取秘密，常数时间（CT）纪律通过数据流/控制流线性化（DFL/CFL）抵御时序攻击，但改写后的代码通常大幅增加内存占用并带来显著开销。VeCT 利用向量扩展（AVX-512）在保持常数时间保证的同时提升性能：先用严格的统计测试为实现细节专有的 AVX-512 指令推导实用的"安全使用"规则（该分析还揭示了一个最先进常数时间改写器中此前未知的漏洞），再据此引入消除改写代码中冗余数据加载、并启用向量化的新策略。VeCT 基于 LLVM 实现，可自动把代码转换为基于 AVX-512 的常数时间等价形式；在 AES 和 Blowfish 等真实应用上，相比最先进方法把改写代码的开销最多降低 98.9%，同时保持常数时间行为。


### Memclave: Secure In-Memory Enclave for Untrusted Hosts
- **作者**: Amit Choudhari, Fabian van Rissenbeck, Christian Rossow
- **论文 PDF**: [Memclave: Secure In-Memory Enclave for Untrusted Hosts](https://www.usenix.org/system/files/usenixsecurity26-choudhari.pdf)
- **解读**: 云平台的多租户数据密集工作负载中，频繁的 CPU-内存流量可通过缓存侧信道泄漏访问模式；UPMEM 等处理内存（PIM）设备把计算移入 DRAM，大幅减少数据移动并缩小 CPU 缓存足迹，但商用 PIM 架构暴露主机编程的控制面和主机共享的模块内存，使设备内代码和数据易受被攻陷主机侵害，而现有安全 PIM 方案或需添加加密/访问控制硬件、或依赖重量级主机侧密码协议。Memclave 是纯软件框架，无需硬件改动即为商用 PIM 带来代码完整性和数据机密性：TPM 证明的超visor 在启动时把 PIM 控制面与主机访问永久隔离，每个内存核心上的可信加载器认证用户内核并建立每会话受保护数据路径。Memclave 保持编程模型和内核代码不变，主机应用仅需用小部分安全替代调用替换数据移动调用，信任计算基小、移植成本低；在商用 UPMEM DIMM 上实现并用 PrIM 基准套件评估后，一次性约 100ms 的认证加载后内核时间接近 PIM 基线（实际规模下 MLP 保持在 1.5× 以内，某些图上 BFS 为 1.1×，随 frontier 层数增加略有上升）。


### Cracking Federated Privacy: Initialization-Resilient Gradient Inversion with Fine-Grained Reconstruction
- **作者**: Kaiming Zhu
- **论文 PDF**: [Cracking Federated Privacy: Initialization-Resilient Gradient Inversion with Fine-Grained Reconstruction](https://www.usenix.org/system/files/usenixsecurity26-zhu-kaiming.pdf)
- **解读**: 联邦学习仍易受梯度反演攻击（GIA），共享梯度可泄露客户端私有数据，而现有攻击在早期阶段初始化变化下表现不稳定且重建粗糙。作者识别出共享梯度中的稀疏性变化是这种敏感性的主要来源，提出初始化鲁棒、粗到细设计的 GIA：粗阶段对齐梯度方向并约束非零项以缓解稀疏性变化，细阶段用余弦距离与变形 Manhattan 项结合的混合度量细化幅度对齐。在 CIFAR-10/100 的敏感初始化下相对五个基线最多获得 200% 的 PSNR 提升（25.4 → 47.7 dB），并在四个数据集和整个 FL 生命周期内稳定实现细粒度恢复；方法在批大小和本地步数变化下与 SOTA 基线竞争力相当，同时揭示了多个流行模型上的持续泄漏及现有防御的不足，凸显加强隐私保护机制的紧迫性。


### Estimating the Amount of Script-generated Traffic in a Mixture
- **作者**: Cormac Herley
- **论文 PDF**: [Estimating the Amount of Script-generated Traffic in a Mixture](https://www.usenix.org/system/files/usenixsecurity26-herley.pdf)
- **解读**: 本文解决估计混合流量中机器人生成流量占比的问题，即当收到的流量为 α·Clean + (1-α)·Bot 时估计 α，主要适用于流量试图伪装成人类生成的情形（如点击欺诈、不真实社交媒体参与）。作者证明：当干净流量中至少存在一对独立特征（如地理分布的时间不变性）时，获得 α 的上界等价于求最大化某个简单目标函数的 rank-one 矩阵，并给出高效求解方法及界的紧性推导；由于有限数据下 rank-one 矩阵的采样版本并非精确 rank-one，作者还推导出置信区间以确保找到的是真正的上界。实验验证包括两部分：用随机 rank-one 和满秩矩阵分别表示干净与机器人分布进行蒙特卡洛模拟验证准确性；在 Twitter 数据上，公开市场出售的大粉丝量账号被标记为 >90% 的机器人粉丝，而多个学术会议和知名研究者的账号被标记为 <20%，并在任意干净/机器人构成的账号群体上验证了准确性。


### Quorus: Efficient, Scalable Threshold ML-DSA Signatures from MPC
- **作者**: Alexander Bienstock
- **论文 PDF**: [Quorus: Efficient, Scalable Threshold ML-DSA Signatures from MPC](https://www.usenix.org/system/files/usenixsecurity26-bienstock.pdf)
- **解读**: 阈值签名协议把秘密签名密钥分散到多方，超过阈值的子集可联合生成签名；虽然后量子阈值签名研究活跃，但多数方案针对专门设计的阈值友好方案，无法满足分布式 CA 和数字货币等真实应用对现有标准化验证流程的兼容需求。Quorus 提出首个高效、可扩展的多方生成 ML-DSA（NIST 后量子签名标准之一）方案：一方面给出适合高效多方计算（MPC）的 ML-DSA 签名算法变体并证明其与原方案安全性相同，另一方面给出若干高效可扩展的 MPC 协议实例化阈值签名功能。协议每个拒绝采样轮每方的在线通信低至 150KB，并在诚多数设置下实例化以避免额外公钥假设；签名可被同一 ML-DSA 实现以所有安全级别验证，签名与验证密钥大小与 ML-DSA 完全一致（此前基于格的阈值方案无法同时匹配这两个大小），是首个兼容 NIST 标准化验证、可扩展到任意多方且无新假设的后量子阈值签名方案。


### Chameleon Channels: Measuring YouTube Accounts Repurposed for Deception and Profit
- **作者**: Alejandro Cuevas, Manoel Horta Ribeiro, Nicolas Christin
- **论文 PDF**: [Chameleon Channels: Measuring YouTube Accounts Repurposed for Deception and Profit](https://www.usenix.org/system/files/usenixsecurity26-cuevas.pdf)
- **解读**: 内容创作者通常耗费大量精力经营粉丝群体，本文探究已建立受众的频道为何会彻底改变身份与内容（如从猫咪梗转向推广加密货币或报道选举），即"改造频道"问题。作者先刻画了一个二手社交媒体账号市场，6 个月观察期内销售额超过 100 万美元；对同期（被）转售的 YouTube 频道，发现相当比例（53%）被用于传播政策敏感内容且往往不受处罚，这些频道不降反增订阅者。利用约 140 万账号的两个生态有效代理快照，估计野外频道改造在 3 个月内的流行度约为 0.25%；实验确认改造频道与出售频道共享特征（政策敏感内容显著高），其中既有类似影响力行动的频道也有金融诈骗频道，且改造频道受众庞大（两个样本合计 1.93 亿与 4400 万订阅者）。作者认为购买既有受众及其信誉对受财务和意识形态驱动的攻击者有利，该现象不限于 YouTube，有机受众培育市场若缺乏缓解措施将持续增长。


### Khost: KVM-based Near Native MCU Firmware Rehosting
- **作者**: Chunlin Wang
- **论文 PDF**: [Khost: KVM-based Near Native MCU Firmware Rehosting](https://www.usenix.org/system/files/usenixsecurity26-wang-chunlin.pdf)
- **解读**: 基于 MCU 的设备构成 IoT 基础设施的关键层，rehosting 式动态固件分析是保障其安全的有效途径，但现有框架常因仿真而承受显著性能开销或缩小执行范围。Khost 提出近原生、保持执行范围的 rehosting 框架：通过扩展 KVM 引入轻量扩展 CPU、辅助页表和软件中断控制器，使 MCU 固件能以最小开销在高性能平台上重宿主，并提供 MMIO 监视器以快速处理外设交互、提供固件 wrapper 以收集覆盖率和灵活配置现有 fuzzing 引擎。两个标准基准的评估显示，相比 QEMU，Khost 对复杂计算任务降低 90.0% 至 95.5% 的开销、对 MCU 系统级操作最多降低 98.5%；对 12 个真实固件进行 fuzzing 时吞吐量最高提升 197.5×、基本块覆盖率提升 6×，并成功发现 5 个此前未知的 bug。


### Attesting Model Lineage by Consisted Knowledge Evolution with Fine-Tuning Trajectory
- **作者**: Zhuoyi Shang, School of Cyber Security, and State Key Laboratory of Cyberspace Security Defense, Yanwei Liu, and State Key Laboratory of Cyberspace Security Defense, Xiaoyan Gu, School of Cyber Security, and State Key Laboratory of Cyberspace Security Defense, Weiping Wang
- **论文 PDF**: [Attesting Model Lineage by Consisted Knowledge Evolution with Fine-Tuning Trajectory](https://www.usenix.org/system/files/usenixsecurity26-shang.pdf)
- **解读**: 深度学习的微调技术使模型之间产生谱系关系，可用于应对开放权重模型库中缺乏健壮验证机制导致的未经授权再分发与来源虚假声明等安全问题，但现有模型谱系检测主要依赖静态架构相似性，难以捕捉构成真实谱系的知识动态演化。受人类进化遗传机制启发，作者通过验证知识演化与参数修改的联合轨迹来解决模型谱系证明：先用模型编辑量化微调引入的参数级变化，再提出知识向量化机制，借助 probe samples 把编辑后模型中演化的知识提炼为紧凑表示（探测策略按模型家族类型调整），这些嵌入用于验证跨模型知识关系的算术一致性，从而实现鲁棒的谱系证明。大量实验表明该方法在分类器、扩散模型和大语言模型等多种模型类型及多种真实对抗场景下都能稳定实现可靠的谱系验证。


### SONIC: Concurrent Oblivious RAM & Data Structures for Low-Latency and High-Throughput
- **作者**: Nihal Talur and Ioannis Demertzis
- **论文 PDF**: [SONIC: Concurrent Oblivious RAM & Data Structures for Low-Latency and High-Throughput](https://www.usenix.org/system/files/usenixsecurity26-talur.pdf)
- **解读**: 仅靠加密进行隐私计算易受泄漏滥用/访问模式攻击，TEE 也易受侧信道攻击，遗忘原语（ORAM、ODS）通过隐藏内存访问模式缓解这些风险；但基于树的 ORAM（EnigMap、GraphOS、Oblix）延迟低而并行度有限，即使小数据集吞吐也难超 1K req/s，基于分区的方案（如 Snoopy）以大幅牺牲延迟换取高吞吐，实际扩展 TB 级数据需 1000+ 台服务器。SONIC 是首个用 RingORAM 替代 PathORAM 的硬件 enclave ORAM，通过克服顺序驱逐瓶颈、实现高效批量驱逐以及提供无锁的访问、重排与 stash 操作等所有树型 ORAM 构造的核心难题，成为首个最低 150K req/s、单服务器最高 2M req/s 的低延迟 ORAM。SONIC 的 ORAM 访问吞吐比开源的 EnigMap 实现高 28-197×，比 GraphOS 高 158-1065×（N=2^27、块大小 64 字节）；它提供多种利用 ORAM 并发构建遗忘数据结构（如 OMAP）的方法，并可作为 Snoopy subORAM 的即插即用替代，使 1TB 数据仅需 32 台服务器而非数千台。


### E2E-AKMA: An End-to-End Secure and Privacy-Enhancing AKMA Protocol Against the Anchor Function Compromise
- **作者**: Yueming Li, and University of Chinese Academy of Sciences, Long Chen, and Key Laboratory of System Software, Qianwen Gao, and University of Chinese Academy of Sciences, Zhenfeng Zhang, and Key Laboratory of System Software
- **论文 PDF**: [E2E-AKMA: An End-to-End Secure and Privacy-Enhancing AKMA Protocol Against the Anchor Function Compromise](https://www.usenix.org/system/files/usenixsecurity26-li-yueming.pdf)
- **解读**: AKMA 是 3GPP 近期制定的协议，预期成为 5G 标准的关键组件，它让应用服务提供商把用户认证委托给移动网络运营商，免除其存储管理认证数据，但引入了新的安全与隐私挑战：AKMA 锚功能（AAnF）可能在 5G 核心网边界之外运行，被攻陷后攻击者可监控用户登录活动或未授权访问敏感通信内容，而外部应用功能（AF）暴露 SUPI 也可能被用于把用户真实身份与在线活动关联。作者提出 E2E-AKMA 协议，即使 AAnF 被攻陷也能在用户设备（UE）与应用功能（AF）之间建立具备端到端安全的会话密钥，并保证除 5G 核心网外无任何实体能把账户活动关联到用户真实身份。该架构保留原 AKMA 方案的优点（无需复杂动态秘密数据管理、除标准 SIM 卡外不依赖特殊硬件），实验评估显示其相对原 5G AKMA 方案开销约 9.4%，具备部署的效率和实用性。


### Adversarial Patch EXterminator: Zero-Shot and Patch-Agnostic Defense Framework Against Adversarial Patch Attacks
- **作者**: Jiayimei Wang, Tao Ni, Guowu Xu, Qingchuan Zhao and Cong Wang
- **论文 PDF**: [Adversarial Patch EXterminator: Zero-Shot and Patch-Agnostic Defense Framework Against Adversarial Patch Attacks](https://www.usenix.org/system/files/usenixsecurity26-wang-jiayimei.pdf)
- **解读**: 对抗补丁攻击严重威胁现代计算机视觉系统，现有防御（可认证模型或补丁识别流水线）通常依赖先验知识或大量训练数据，对变化的物理条件鲁棒性不足，且难以应对微小、不规则或与背景高度融合的补丁。作者提出 APEX，一个零样本、补丁无关的三阶段对抗补丁防御框架：先通过边界框提取集中补丁区域，再用基于互信息的模糊热图与边缘感知的边界热图结合定位对抗区域，最后用结构引导的图像修复恢复图像。在多个数据集与现有最先进防御方法的实验中，APEX 能有效防御各种对抗补丁（如非自然、自然主义及红外图像），在补丁定位上表现优越，对光照等环境变化和极端情况保持高鲁棒性，并在物理世界场景中有效保护多种模型。


### Quantifying Large Language Model Attacks Through the Lens of Model Cognition
- **作者**: Xiuming Liu, Bin Benjamin Zhu, Hongsheng Hu
- **论文 PDF**: [Quantifying Large Language Model Attacks Through the Lens of Model Cognition](https://www.usenix.org/system/files/usenixsecurity26-liu-xiuming.pdf)
- **解读**: LLM 易受诱使产生有害内容的恶意输入攻击，而关键词过滤、输出审核等现有安全机制大多忽略内部模型动态。作者证明与有害提示相关的安全相关特征在生成前的中间隐藏状态中可被轻量探针强分离（准确率最高达 99%），说明即使模型输出合规，这些特征仍在内部持续存在；据此提出分层毒性探针和多层互补检测框架 Sentinel（参数少于 5M），融合不同深度的信号。Sentinel 相比生成级拒绝将假阴性减半，在对抗攻击下保持 94% 以上的检测准确率（基线下降 32%），并在七个开放权重 LLM（1.5B→72B）和多个基准（I2P、SneakyPrompt、MMA、Labelled、PIJ、ChatAlpaca、Multi-turn Jailbreak）上优于 Llama-Guard-3-8B；此外，该方法首次给出安全相关信号在 LLM 内部产生、传播和衰退的逐层定量图谱，支持可解释的由内而外对齐与诊断。
