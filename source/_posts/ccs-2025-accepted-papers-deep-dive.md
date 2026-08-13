---
title: ACM CCS 2025 录用论文深度解读（395 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, ACM CCS, 2025]
---

ACM CCS 2025 于 2025 年 10 月 13-17 日在中国台北举行。截至本文发布，CCS 2026 录用名单官方尚未公布，故本文覆盖最新完整一届：CCS 2025 全部 395 篇录用论文（两轮接收），逐一给出中文深度解读，并附上可获取的论文 PDF 链接。解读基于论文摘要（OpenAlex 等收录），部分论文无公开摘要，仅据标题推断并已标注。

## 全部 395 篇录用论文解读

### Autonomous Vulnerability Analysis, Triaging, and Repair: A Historical Perspective.
- **作者**: Giovanni Vigna
- **解读**: 这篇历史视角的工作指出，支撑关键基础设施的软件普遍存在漏洞，其利用可能导致服务中断、经济损失乃至生命损失。作者以回顾性视角梳理自主漏洞分析、分类与修复领域的发展脉络，为理解该方向的演进与未来走向提供背景。


### Mechanizing Privacy by Design.
- **作者**: David A. Basin, François Hublet, Srdan Krstic, Hoàng Nguyen
- **解读**: 针对「设计即隐私」缺乏落地手段的问题——立法未规定具体实现方式，主流语言与框架也缺少隐私设计支持——作者开发了多种技术方案：一是基于逻辑的工具，在运行时强制执行控制系统相关动作的形式化策略；二是提出支持模型驱动执行的方法与模型。论文汇报了这些方法、工具及实际使用经验。


### Oedipus: LLM-enchanced Reasoning CAPTCHA Solver.
- **作者**: Gelei Deng, Haoran Ou, Yi Liu, Jie Zhang, Tianwei Zhang, Yang Liu
- **arXiv PDF**: [Oedipus: LLM-enchanced Reasoning CAPTCHA Solver.](https://arxiv.org/pdf/2405.07496)
- **解读**: 提出名为 Oedipus 的基于大语言模型（LLM）增强的推理型验证码（Reasoning CAPTCHA）求解器，旨在利用 LLM 的推理能力自动破解需要逻辑推理的验证码。（无摘要，据标题推断）


### The Odyssey of robots.txt Governance: Measuring Convention Implications of Web Bots in Large Language Model Services.
- **作者**: Jian Cui, Mingming Zha, XiaoFeng Wang, Xiaojing Liao
- **解读**: 围绕 robots.txt 对 LLM 服务网络爬虫的治理展开大规模测量，系统分析了 18,582,281 个文件。研究发现与 LLM 服务相关的限制显著增加，尤其集中在金融与新闻类域名；但配置管理因生态复杂、第三方代理介入而困难重重，并发现多起违规案例，包括 LLM 记忆了受限域名内容、忽略或访问受限内容。结果显示当前治理存在缺口，需要可执行的机制来尊重发布者的控制意图。


### JsDeObsBench: Measuring and Benchmarking LLMs for JavaScript Deobfuscation.
- **作者**: Guoqiang Chen, Xin Jin, Zhiqiang Lin
- **arXiv PDF**: [JsDeObsBench: Measuring and Benchmarking LLMs for JavaScript Deobfuscation.](https://arxiv.org/pdf/2506.20170)
- **解读**: 提出 JsDeObsBench 基准，用于测量和评估大语言模型在 JavaScript 反混淆任务上的能力，为衡量 LLM 对混淆 JS 代码的还原与理解能力提供标准化测试集。（无摘要，据标题推断）


### Quantifying Security Training in Organizations Through the Analysis of U.S. SEC 10-K Filings.
- **作者**: Jonas Hielscher, Maximilian Golla
- **解读**: 利用美国 SEC 自 2023 年底生效、要求公司在年报 10-K 中披露安全策略的新规，爬取并分析了 5,286 份文件，通过关键词检索与主题分析量化组织的安全培训实践。结果显示 78% 的公司报告实施了安全意识培训（SAT）、27% 开展钓鱼模拟，比例随行业与规模显著变化，大型企业往往对标 NIST CSF 等标准开展更全面的工作；多因素认证（11%）是最常见的面向员工的安全控制，但许多公司把员工视为风险因素。该研究帮助组织批判性评估厂商说法，为政策制定提供依据，并公开编码数据集与爬取工具支持长期追踪分析。


### WhisperTest: A Voice-Control-based Library for iOS UI Automation.
- **作者**: Zahra Moti, Tom Janssen-Groesbeek, Steven Monteiro, Andrea Continella, Gunes Acar
- **解读**: 提出 WhisperTest，一个基于语音控制（Voice Control）的 iOS UI 自动化测试库，利用系统语音控制能力驱动界面交互，实现无障碍的移动应用 UI 自动化测试。（无摘要，据标题推断）


### The Challenges and Opportunities with Cybersecurity Regulations: A Case Study of the US Electric Power Sector.
- **作者**: Sena Sahin, Burak Sahin, Robin Berthier, Kate Davis, Saman A. Zonouz, Frank Li
- **解读**: 以美国电力行业为案例，研究组织如何参与网络安全监管以及面临的实际挑战。论文指出，尽管监管对组织安全态势有重要影响，但此前对组织参与方式与困难缺乏调查；评估这些因素对理解监管的实践影响、进而改进未来监管至关重要。


### Automatically Detecting Online Deceptive Patterns.
- **作者**: Asmit Nayak, Yash Wani, Shirley Zhang, Rishabh Khandelwal, Kassem Fawaz
- **arXiv PDF**: [Automatically Detecting Online Deceptive Patterns.](https://arxiv.org/pdf/2411.07441)
- **解读**: 提出 AutoBot 框架，仅凭网站截图即可识别并定位在线欺骗性模式（deceptive patterns），无需依赖底层 HTML 代码。框架采用两阶段流水线：先用专用视觉模型分析截图、识别交互元素并提取文本特征，再由大语言模型结合上下文判断欺骗性模式是否存在；此外还通过教师 LLM 蒸馏知识构建合成数据集训练更小的模型。评测显示其欺骗模式检测 F1 分数达 0.93，为在线欺骗模式提供可用且可扩展的检测方案。


### Synthesis of Sound and Precise Leakage Contracts for Open-Source RISC-V Processors.
- **作者**: Zilong Wang, Gideon Mohr, Klaus von Gleissenthall, Jan Reineke, Marco Guarnieri
- **arXiv PDF**: [Synthesis of Sound and Precise Leakage Contracts for Open-Source RISC-V Processors.](https://arxiv.org/pdf/2509.06509)
- **解读**: 针对「泄漏契约」（指令集层面的安全抽象，刻画处理器经微架构侧信道可能泄漏的信息）的构造难题：现有工具能验证处理器是否满足给定契约，但手工推导出既可靠（sound）又精确（precise）的契约困难、耗时且易错，需要对优化引入的时序信道有深入了解。本文研究如何为开源 RISC-V 处理器合成可靠且精确的泄漏契约。


### Wanilla: Sound Noninterference Analysis for WebAssembly.
- **作者**: Markus Scherer, Jeppe Fredsgaard Blaabjerg, Alexander Sjösten, Matteo Maffei
- **arXiv PDF**: [Wanilla: Sound Noninterference Analysis for WebAssembly.](https://arxiv.org/pdf/2509.08758)
- **解读**: 提出 Wanilla，一种针对 WebAssembly 的可靠（sound）非干扰（noninterference）分析工具，用于形式化验证 Wasm 程序的信息流安全性，检测秘密数据经程序行为的泄露路径。（无摘要，据标题推断）


### Securing Cryptographic Software via Typed Assembly Language.
- **作者**: Shixin Song, Tingzhen Dong, Kosi Nwabueze, Julian Zanders, Andres Erbsen, Adam Chlipala, Mengjia Yan
- **arXiv PDF**: [Securing Cryptographic Software via Typed Assembly Language.](https://arxiv.org/pdf/2509.08727)
- **解读**: 面向密码学软件提出基于类型化汇编语言（TAL）的安全方案：密码代码不应通过时序行为泄露秘密，而 2018 年 Spectre 系列推测执行攻击的披露打破了「遵循行业标准常量时间编码规范即足够」的传统认知，为时序侧信道防护引入新的复杂性，需要更强、可验证的保证。


### Formally Verified Correctness Bounds for Lattice-Based Cryptography.
- **作者**: Manuel Barbosa, Matthias J. Kannwischer, Thing-Han Lim, Peter Schwabe, Pierre-Yves Strub
- **解读**: 针对基于 Fujisaki-Okamoto 变换的格基 KEM：其安全性直接依赖于解密错误概率被界定在很小的实数范围内。本文提出形式化验证解密错误统计/概率上界的方法，以 ML-KEM 底层 PKE 为主要实例，将文献中启发式近似的论证形式化并确认上界成立；再以 FrodoKEM 展示无需启发式近似即可验证的更广泛适用性，并讨论了其他（非近似）界定解密错误概率的途径。


### Enabling Secure and Efficient Data Loss Prevention with a Retention-aware Versioning SSD.
- **作者**: Weidong Zhu, Carson Stillman, Sara Rampazzi, Kevin R. B. Butler
- **解读**: 提出一种具有保留期感知（retention-aware）能力的版本化 SSD 设计，用于实现安全且高效的数据防泄漏（DLP）：在存储层通过数据版本化与保留期管理增强对敏感数据的控制与安全删除能力。（无摘要，据标题推断）


### Don't Panic! Finding Bugs Hidden Behind Rust Runtime Safety Checks.
- **作者**: Zeyang Zhuang, Zilun Wang, Wei Meng, Michael R. Lyu
- **解读**: 指出现有模糊测试工具在 Rust 程序上效果与效率显著受损：编译器隐式插入或开发者显式写入的运行时安全检查会造成大量 panic 崩溃和程序提前终止，使模糊器难以深入这些检查背后的深层代码，潜在漏洞因此漏检。该工作针对这一问题，研究如何越过 Rust 运行时安全检查发现隐藏缺陷。


### Hardening Deep Neural Network Binaries against Reverse Engineering Attacks.
- **作者**: Zheng Zhong, Ruoyu Wu, Junpeng Wan, Muqi Zou, Dave (Jing) Tian
- **解读**: 针对 DNN 二进制程序被逆向工程窃取的问题：训练完成的模型编译为二进制后可部署在边缘设备与云上，而近年二进制反编译研究表明其可通过逆向技术被窃取。通用混淆虽研究充分，但并非针对 DNN 二进制的独特特征设计，在隐藏模型信息方面存在局限；本文研究针对 DNN 二进制特征进行加固、抵抗逆向工程攻击的混淆方法。


### CROSS-X: Generalized and Stable Cross-Cache Attack on the Linux Kernel.
- **作者**: Dong-ok Kim, Juhyun Song, Insu Yun
- **解读**: 提出 CROSS-X，一种针对 Linux 内核的通用且稳定的跨缓存（cross-cache）攻击技术，旨在提高内核堆跨缓存利用的泛化能力与稳定性。（无摘要，据标题推断）


### Uncovering Hidden Paths in 5G: Exploiting Protocol Tunneling and Network Boundary Bridging.
- **作者**: Altaf Shaik, Robert Jaschek, Jean-Pierre Seifert
- **解读**: 指出 5G 网络控制面与用户面、各接口及核心网功能之间设计了隔离的信任边界，但当这些边界未严格实施时，攻击者控制的 UE 可利用协议隧道与网络边界桥接绕过隔离，使恶意流量沿非预期路径到达核心网内部敏感组件。该工作演示了此类攻击，揭示 5G 信任边界隔离失效所隐藏的攻击路径。


### Finding SSH Strict Key Exchange Violations by State Learning.
- **作者**: Fabian Bäumer, Marcel Maehren, Marcus Brinkmann, Jörg Schwenk
- **arXiv PDF**: [Finding SSH Strict Key Exchange Violations by State Learning.](https://arxiv.org/pdf/2509.10895)
- **解读**: 针对 SSH Terrapin 攻击（USENIX 2024）的缓解措施——OpenSSH 提出的 strict KEX（严格密钥交换）扩展——研究其设计、实现与安全性：用黑盒状态学习分析 10 个流行 SSH 服务器实现、最多 5 种算法共 33 个状态机，由于完整状态机学习不可行，作者改为在启用 strict KEX 的情况下学习握手阶段。结果显示 7 个实现违反规范，并发现两个漏洞：一个可导致专有 Tectia 服务器产生 rogue session，另一个影响 Erlang Open Telecom Platform 的官方实现，可在服务器上下文中实现未认证代码执行。


### OCR-APT: Reconstructing APT Stories from Audit Logs using Subgraph Anomaly Detection and LLMs.
- **作者**: Ahmed Aly, Essam Mansour, Amr M. Youssef
- **arXiv PDF**: [OCR-APT: Reconstructing APT Stories from Audit Logs using Subgraph Anomaly Detection and LLMs.](https://arxiv.org/pdf/2510.15188)
- **解读**: 提出 OCR-APT 系统，从系统审计日志（溯源图）重建 APT 攻击故事：先用图神经网络进行子图异常检测，学习节点周围的行为模式而非依赖文件路径、IP 等易碎属性，以降低误报；再用大语言模型对检测到的子图迭代重构多阶段攻击叙事，每个阶段经验证后才继续，保证最终报告可解释。在 DARPA TC3、OpTC、NODLINK 数据集上，其检测精度与告警可解释性均优于现有方法，能生成完整刻画攻击故事的报告。


### CITesting: Systematic Testing of Context Integrity Violations in LTE Core Networks.
- **作者**: Mincheol Son, Kwangmin Kim, Beomseok Oh, CheolJun Park, Yongdae Kim
- **解读**: 首次系统性地检测 LTE 核心网中的「上下文完整性违规」（CIV）——未认证或认证不当的 UE 修改其他用户内部状态。CITesting 框架遍历多样的流程链、测试广泛的信息元素（IE）并在不同连接状态下验证行为，采用有状态双 UE 控制测试管理受害者、以行为 oracle 进行黑盒检测；在 Open5GS、srsRAN、Amarisoft、Nokia 上分别发现 29、22、16、59 个问题。这些漏洞可发起去附着、IMSI 暴露与存在性检测等远程攻击，且攻击半径在 MME 区域（远大于小区），无需与受害者同小区共址；问题均已负责任披露，并为 Amarisoft 与 Open5GS 提交补丁。


### CryptGNN: Enabling Secure Inference for Graph Neural Networks.
- **作者**: Pritam Sen, Yao Ma, Cristian Borcea
- **解读**: 提出 CryptGNN，为云端第三方 GNN 模型（以 MLaaS 形式被客户端访问）提供安全推理：核心创新是用分布式安全多方计算（SMPC）实现消息传递与特征变换层，同时保护客户输入数据与图结构不被云方和模型方窃取、保护模型参数不被客户端窃取。方案支持任意数量的 SMPC 参与方、无需可信服务器，即使 P-1 个参与方合谋也可证明安全；理论分析与实验共同验证了其安全性与效率。


### PLRV-O: Advancing Differentially Private Deep Learning via Privacy Loss Random Variable Optimization.
- **作者**: Qin Yang, Nicholas Stout, Meisam Mohammady, Han Wang, Ayesha Samreen, Christopher J. Quinn, Yan Yan, Ashish Kundu, Yuan Hong
- **解读**: 针对 DP-SGD 中高斯/拉普拉斯等常规噪声机制仅由方差或尺度单一参数决定、导致噪声幅度同时绑定隐私损失与效用损失、无法独立控制的局限，提出 PLRV-O 框架：在更宽的 DP-SGD 噪声分布搜索空间中，对隐私损失随机变量的矩进行精确刻画，并使其可相对隐私损失独立优化，从而按任务需求系统调整批次大小、训练时长、采样策略与微调场景下的裁剪阈值。实验表明，CIFAR-10 上微调 ViT 在 ε≈0.5 时达到 94.03% 准确率（高斯机制为 83.93%），SST-2 上 RoBERTa-large 在 ε≈0.2 时达到 92.20%（高斯机制为 50.25%）。


### ZORRO: Zero-Knowledge Robustness and Privacy for Split Learning.
- **作者**: Nojan Sheybani, Alessandro Pegoraro, Jonathan Knauer, Phillip Rieger, Elissa Mollakuqe, Farinaz Koushanfar, Ahmad-Reza Sadeghi
- **解读**: 提出 ZORRO，一种面向分割学习（split learning）的零知识（zero-knowledge）鲁棒性与隐私方案，旨在以零知识证明等手段在分割学习框架中同时保障对恶意参与的鲁棒性与数据/模型隐私。（无摘要，据标题推断）


### ImportSnare:Directed 'Code Manual' Hijacking in Retrieval-Augmented Code Generation.
- **作者**: Kai Ye, Liangcai Su, Chenxiong Qian
- **解读**: 提出 ImportSnare，一种针对检索增强代码生成（RAG）的定向「代码手册」劫持攻击，通过操纵检索到的代码手册/文档内容，诱导大语言模型在生成代码时引入恶意行为。（无摘要，据标题推断）


### Generic Anonymity Wrapper for Messaging Protocols.
- **作者**: Lea Thiemt, Paul Rösler, Alexander Bienstock, Rolfe Schmidt, Yevgeniy Dodis
- **解读**: 针对即时通讯中端到端加密无法阻止基于元数据（序列号、公钥、明文标识符等）追踪的问题——这些元数据会暴露用户的社交网络与通信模式——研究通用的匿名性封装器。论文指出现有隐藏元数据的方案（Signal 的 Sealed Sender、面向 MLS 类构造（Hashimoto et al., CCS 2022）、网状网络（Bienstock 2023））要么效率较低、要么仅针对特定场景定制，且所有现有实用方案在秘密暴露时都会泄露关键元数据，本文旨在提出可通用的匿名包装方案。


### Panther: Private Approximate Nearest Neighbor Search in the Single Server Setting.
- **作者**: Jingyu Li, Zhicong Huang, Min Zhang, Cheng Hong, Jian Liu, Tao Wei, Wenguang Chen
- **解读**: 提出 Panther，在单服务器假设下实现高效的私有近似最近邻搜索（ANNS）：客户端只学习查询结果、不向服务器泄露查询内容，避免了此前单服务器方案的高通信成本（Chen et al., USENIX Security 2020）与双非合谋服务器假设（Servan-Schreiber, S&P 2022）。Panther 通过信息检索、秘密共享、混淆电路与同态加密的协同设计实现性能，在四个公开数据集上可 18 秒查询 1000 万点、通信量 284MB，比 Chen et al. 快 7.8 倍以上、通信紧凑 20 倍。


### Trout: Two-Round Threshold ECDSA from Class Groups.
- **作者**: Hila Dahari-Garbian, Ariel Nof, Luke Parker
- **解读**: 提出 Trout，首个支持任意阈值的分布式两轮 ECDSA 签名协议：每方上传带宽为常数、处理时间随参与方数量线性增长，并实现可识别中止（IA）——协议因故障无法终止时各方可将失败归责到具体参与方。该方案无需可信设置。


### Encrypted Matrix-Vector Products from Secret Dual Codes.
- **作者**: Fabrice Benhamouda, Caicai Chen, Shai Halevi, Yuval Ishai, Hugo Krawczyk, Tamer Mour, Tal Rabin, Alon Rosen
- **解读**: 研究加密矩阵向量积（EMVP）问题：离线阶段客户端上传矩阵 M（有限域上）的加密并仅保留短密钥，服务器存储密文；在线阶段客户端可反复发送查询向量 q_i 的加密，使服务器能本地计算 M·q_i 的紧凑份额，客户端对矩阵一无所知，输出份额既可被揭示也可作为另一协议输入。该问题源于高效安全计算的需求，本文基于秘密对偶码（secret dual codes）构造解决方案。


### KZH-Fold: Accountable Voting from Sublinear Accumulation.
- **作者**: George Kadianakis, Arantxa Zapico, Hossein Hafezi, Benedikt Bünz
- **解读**: 面向带宽高效的问责投票协议提出亚线性累积方案：累积方案比递归 SNARK 开销更低，但现有常量大小验证器的累积器与判定器为线性大小。论文（I）提出新的多项式承诺方案 KZH，（II）构建 KZH-fold——首个验证器仅 3 次群标量乘法、累积器大小与判定时间均为 O(n^(1/2)) 的亚线性方案，并可推广到 k·n^(1/k) 复杂度；（III）借助 BCLMS 编译器构建 IVC/PCD 证明判定器；（IV）提出非均匀 IVC 的新方法，每步证明成本与最大指令电路成正比、见证不再随指令数线性增长；（V）在支持百万节点的签名聚合应用上实现并基准测试，2000 个 Poseidon 哈希场景相比 Nova 通信减少约 2000 倍、改进约 50 倍，证明者开销约 3 倍。


### SyRA: Sybil-Resilient Anonymous Signatures with Applications to Decentralized Identity.
- **作者**: Elizabeth C. Crites, Aggelos Kiayias, Markulf Kohlweiss, Amirreza Sarencheh
- **解读**: 研究 Sybil 弹性匿名签名（SyRA）：允许持有凭证的用户按需生成与任意上下文绑定的不可链接假名，并代表这些假名签发签名。具体地，分布式签发者可将低熵的传统身份或人格标识符转换为高伪熵的唯一关联密钥，用于生成上下文相关凭证；该原语同时满足三个目标：抗 Sybil（每个用户至多一个数字身份）、匿名性（不泄露用户真实身份信息）与非交互式跨上下文凭证创建。


### Surpassing the Word Size Limitation of TFHE with Noise Calibration.
- **作者**: Takumi Nishimura, Kazunari Tozawa, Kunihiko Sadakane
- **解读**: 针对 TFHE 可编程自举（PBS）只能高效评估小函数域、函数域受 TFHE 字长限制约束（为保障数据安全所必需）的扩展问题：垂直打包技术（Chillotti et al., 2020）虽支持更大评估但依赖昂贵的电路自举，树状方法（Guimarães, 2021）避免电路自举却引入显著开销（W 位域需 O(2^W) 次调用）。本文提出噪声校准（noise calibration）方法突破 TFHE 字长限制，以更低成本扩大 PBS 可评估函数的规模。


### QV-net: Decentralized Self-Tallying Quadratic Voting with Maximal Ballot Secrecy.
- **作者**: Zibo Zhou, Zongyang Zhang, Feng Hao, Bowen Zheng, Zulkarnaim Masyhur
- **解读**: 针对 DAO 场景中现有二次投票（QV）系统将选票明文与数字签名公开、损害选民隐私并可能影响诚实参与的问题：此前基于加密的选票方案工作在集中式设置、依赖计票机构，而分散式自计票投票中没有第三方。本文提出 QV-net，一种去中心化、自计票且具有最大选票保密性的二次投票方案。


### End-to-End Encrypted Git Services.
- **作者**: Ya-Nan Li, Yaqing Song, Qiang Tang, Moti Yung
- **解读**: 针对 Git 服务端到端安全缺失的问题：仓库内容及其支撑的代码/数据供应链价值极高，而系统入侵已屡见不鲜，正如消息与云存储领域对端到端安全的关注一样，Git 服务也需要同等安全级别。论文指出现有研究（多为开源项目）存在两类不足：只提供很弱的安全性，或开销巨大，本文旨在给出兼顾安全性与效率的端到端加密 Git 服务方案。


### RingSG: Optimal Secure Vertex-Centric Computation for Collaborative Graph Processing.
- **作者**: Zhenhua Zou, Zhuotao Liu, Jinyong Shan, Qi Li, Ke Xu, Mingwei Xu
- **解读**: 提出 RingSG，首个在基于 MPC 的顶点中心（vertex-centric）抽象内达到最优通信/计算复杂度的协作图处理系统。其核心范式 Ring-ScatterGather 避免盲目排序等极其昂贵的操作，并保证整体工作负载被最优分解为可并行、互斥的 MPC 任务；实现上通过份额转换引入三方复制秘密共享，并用分组聚合协议优化最耗成本的部分。原型在多种协作设置（不同规模、参与方数、平均顶点度数）下比现有最优方法最多降低运行时间 15.34 倍、单方通信 10.36 倍，在稀疏图上表现尤为出色，与理论成本分析一致。


### Nebula:Efficient, Private and Accurate Histogram Estimation.
- **作者**: Ali Shahin Shamsabadi, Peter Snyder, Ralph Giles, Aurélien Bellet, Hamed Haddadi
- **解读**: 提出 Nebula，一种高效、隐私保护且准确的直方图估计方法，在满足隐私约束的同时提升直方图统计的精度与计算效率。（无摘要，据标题推断）


### Anonymity Unveiled: A Practical Framework for Auditing Data Use in Deep Learning Models.
- **作者**: Zitao Chen, Karthik Pattabiraman
- **arXiv PDF**: [Anonymity Unveiled: A Practical Framework for Auditing Data Use in Deep Learning Models.](https://arxiv.org/pdf/2409.06280)
- **解读**: 针对深度学习模型在用户不知情、未同意的情况下使用抓取的互联网数据（如人脸图像）进行训练的问题：数据需求激励模型创建者从互联网抓取材料，而用户对数据是否被用于训练缺乏控制。论文提出一个实用的审计框架，用于调查深度学习模型中数据的使用情况，回应用户对数据使用控制权的关切。


### Posterior Security: Anonymity and Message Hiding of Standard Signatures.
- **作者**: Tsz Hon Yuen, Ying-Teng Chen, Shimin Pan, Jiangshan Yu, Joseph K. Liu
- **解读**: 提出数字签名的「后验安全」（posterior security）概念：在原始签名生成之后附加额外的安全特性。该概念受以下场景启发：部分用户将秘密密钥存放在安全硬件中，只能通过标准化接口获得标准签名；本文考虑两种后验特性——匿名性与消息隐藏。


### Accuracy for Differentially Private Quotients by Fractional Uncertainties.
- **作者**: Alejandro Russo, Elisabet Lobo Vesga, Marco Gaboardi
- **解读**: 针对差分隐私工具普遍缺少精度信息的问题——尤其是均值、DP 求和之商等数据相关计算——提出基于分数不确定性（fractional uncertainties）与不确定度传播原理计算置信区间（α-β 精度）的新方法。该方法识别出解析误差可被预测的条件，揭示两个关键不变量：数据集规模越大精度越高，数值变异性越高所需数据规模越大；并提供调节器（tuners）以快速确定最小规模、在预算与估计可能性之间权衡。理论贡献经多场景实验验证了分数不确定性在具体计算中的适用性。


### Sabot: Efficient and Strongly Anonymous Bootstrapping of Communication Channels.
- **作者**: Christoph Coijanovic, Laura Hetz, Kenneth G. Paterson, Thorsten Strufe
- **解读**: 提出 Sabot，一种高效且强匿名的通信信道引导（bootstrapping）协议，在信道建立/引导阶段即提供强匿名性，并兼顾效率。（无摘要，据标题推断）


### LZKSA: Lattice-Based Special Zero-Knowledge Proofs for Secure Aggregation's Input Verification.
- **作者**: Zhi Lu, Songfeng Lu
- **解读**: 提出 LZKSA，一个量子安全的联邦学习安全聚合输入验证系统：在 Bell et al.（CCS 2020）的安全聚合协议上引入验证特性，确保用户输入与加密过程的合规性同时保持隐私。系统基于环学习误差（RLWE）问题设计七个专用零知识证明协议，验证用户是否正确使用聚合密钥、数据的 L∞/L2 范数与余弦相似度是否满足约束，并排除当前及未来的进程。实验表明其 L∞ 约束证明的生成/验证时间（CIFAR10 上 0.02s/0.0062s）约为 RoFL（S&P 2023，94s/29.9s）与 ACORN（USENIX 2023，78.7s/33.9s）的千分之一量级。


### A Sea of Cyber Threats: Maritime Cybersecurity from the Perspective of Mariners.
- **作者**: Anna Raymaker, Akshaya Kumar, Miuyin Yong Wong, Ryan Pickren, Animesh Chhotaray, Frank Li, Saman A. Zonouz, Raheem Beyah
- **arXiv PDF**: [A Sea of Cyber Threats: Maritime Cybersecurity from the Perspective of Mariners.](https://arxiv.org/pdf/2506.15842)
- **解读**: 通过用户研究（问卷与半结构化访谈，21 名高级海员）调查海事系统运营者对网络安全威胁的感知与应对：参与者报告了船载网络攻击、GPS 欺骗与扰乱物流的勒索软件等直接经历，证明威胁的现实影响。研究发现系统性的人因问题，如培训与实际需求脱节、检测与响应工具不足、海员安全意识存在严重缺口；论文贡献包括海员视角识别的威胁分类，以及对培训、协议与监管的改进建议。


### Measuring and Augmenting Large Language Models for Solving Capture-the-Flag Challenges.
- **作者**: Zimo Ji, Daoyuan Wu, Wenyuan Jiang, Pingchuan Ma, Zongjie Li, Shuai Wang
- **arXiv PDF**: [Measuring and Augmenting Large Language Models for Solving Capture-the-Flag Challenges.](https://arxiv.org/pdf/2506.17644)
- **解读**: 围绕 LLM 自动化求解 CTF 挑战（如 DARPA AIxCC）所需的多重能力（知识、推理、行动），构建聚焦核心知识能力的基准 CTFKnow（3,992 道题）测量 LLM 表现。关键发现：LLM 具备大量相关知识，但在具体场景中准确应用知识、并根据反馈与环境进行适应调整的能力不足，据此提出增强 LLM 解题能力的思路。


### Exposing the Roots of DNS Abuse: A Data-Driven Analysis of Key Factors Behind Phishing Domain Registrations.
- **作者**: Yevheniya Nosyk, Maciej Korczynski, Carlos Gañán, Sourena Maroofi, Jan Bayer, Zul Odgerel, Samaneh Tajalizadehkhoob, Andrzej Duda
- **解读**: 基于 14.5k 钓鱼域名与 15.4k 良性域名、73 项特征（涵盖注册商属性、主动验证、被动安全实践三大潜在因素）的 GLM 回归分析，系统量化驱动域名滥用的因素：注册费每降低 1 美元对应滥用增加 49%；提供免费捆绑服务（如网页托管）使滥用激增 88%；严格的限制措施可将滥用削减 63%；而提供 API 访问或简化开户流程使钓鱼域名上升 401%。这些结果使注册商、注册局等中介机构能够设计与其经济利益一致的反滥用措施。


### Noise and Stress Don't Help With Learning: A Qualitative Study to Inform Design of Effective Cybersecurity Awareness in Manufacturing Environments.
- **作者**: Lina Brunken, Markus Schöps, Annalina Buckmann, Florian Meißner, M. Angela Sasse
- **解读**: 通过对 6 个地点 33 名制造业工人的半结构化访谈与定性内容分析，研究车间环境中的网络安全意识培训：多数参与者日常仅偶尔接触「办公」ICT（如经共享的车间自助终端处理工资、假期等人事事务），不认为自己负有安全责任、将其归于「管理层」角色，并把 ICT 视为对工作流程顺畅与生产力的潜在威胁。参与者对安全培训持积极态度，但明确偏好面对面、基于团队、可在现场提问的培训方式，与公司设想的个人电脑端培训（车间嘈杂的环境不适合）截然不同；结果表明成功的意识项目需要结合工人相关的工作价值与实践来定制内容与交付方式。


### An Empirical Study Measuring In-The-Wild Cryptographic Microarchitectural Side-Channel Patches.
- **作者**: Sen Deng, Zhibo Liu, Shuai Wang, Yinqian Zhang
- **解读**: 首次对真实世界密码学软件中的微架构侧信道补丁进行大规模测量：构建 SideBench 数据集（来自 OpenSSL、WolfSSL、MbedTLS 的 165 个补丁）与 SideEval 自动化分析工具（结合动态污点分析与静态符号执行）。结果显示即使资深开发者编写的补丁也有 25% 不安全、存在多年未被注意的残余泄漏；部分仓促发布的补丁在修复某信道的同时可能针对其他攻击模型引入新的泄漏；不同库修复相同代码模式的开销差异巨大（10%–170%）。论文还测量了近期基于规则与基于 LLM 的工具，并总结出安全高效打补丁的洞见。


### Can IOCs Impose Cost? The Effects of Publishing Threat Intelligence on Adversary Behavior.
- **作者**: Xander Bouwman, Aksel Ethembabaoglu, Bart Hermans, Carlos Gañán, Michel van Eeten
- **解读**: 首次实证评估「发布威胁情报能向攻击者施加成本」的主张：将两家领先厂商的 IOC 情报流与大规模真实网络流量元数据匹配，回溯生成发布前最多 150 天的恶意活动命中记录。结果显示多数指标资源在发布前已被弃用，限制了其对检测进行中攻击的价值，削弱了「施加成本」的说法；统计建模表明情报状态对命中的解释力很低、存在混杂变量，并观察到攻击者活跃峰值与某厂商发布之间约 30 天的延迟。该研究是首个将攻击者行为与情报发布关联起来的实证评估，连接了计算机科学与国际关系领域。


### TEMPEST-LoRa: Cross-Technology Covert Communication.
- **作者**: Xieyang Sun, Yuanqing Zheng, Wei Xi, Zuhao Chen, Zhizhen Chen, Han Hao, Zhiping Jiang, Sheng Zhong
- **解读**: 提出 TEMPEST-LoRa，一种基于跨技术隐蔽通信（CTCC）的新型电磁隐蔽信道：攻击者无需部署专用接收设备，而是通过精确操控线缆产生电磁泄漏，使秘密数据可被远处广泛部署的商用 LoRa 节点/网关接收。实验表明可在最大距离 87.5 米、或最高 21.6 kbps 的速率下可靠解码线缆调制信号（传输在显示器关闭的隐蔽条件下进行），揭示了对气隙网络的实际威胁。


### MOLE: Breaking GPU TEE with GPU-Embedded MCU.
- **作者**: Hongyi Lu, Yunjie Deng, J. Sukarno Mertoguno, Shuai Wang, Fengwei Zhang
- **解读**: 提出 MOLE，一种利用 GPU 内嵌微控制器（MCU）攻破 GPU 可信执行环境（TEE）的攻击，揭示 GPU TEE 隔离机制中由内嵌 MCU 引入的安全弱点。（无摘要，据标题推断）


### WireTap: Breaking Server SGX via DRAM Bus Interposition.
- **作者**: Alexander Seto, Oytun Kuday Duran, Samy Amer, Jalen Chuang, Stephan van Schaik, Daniel Genkin, Christina Garman
- **解读**: 针对服务器 SGX 的物理攻击：虽然 SGX 旨在对 root 级攻击者提供强完整性与机密性，但许多现实部署处于潜在对抗环境中，节点运营者有经济动机篡改 enclave 内的计算。该工作通过 DRAM 总线插接（interposition）攻破服务器 SGX，并挑战「此类攻击需要昂贵实验室设备、超出爱好者级攻击者能力」的普遍认知。


### One Video to Steal Them All: 3D-Printing IP Theft through Optical Side-Channels.
- **作者**: Twisha Chattopadhyay, Fabricio Ceschin, Marco E. Garza, Dymytriy Zyunkin, Animesh Chhotaray, Aaron P. Stebner, Saman A. Zonouz, Raheem Beyah
- **arXiv PDF**: [One Video to Steal Them All: 3D-Printing IP Theft through Optical Side-Channels.](https://arxiv.org/pdf/2506.21897)
- **解读**: 证明仅凭 3D 打印过程的视频录像即可逆向还原底层打印指令：模型跟踪打印头喷嘴的运动并将轨迹映射为 G-code，还能识别进给率、挤出率等正确参数，实现知识产权窃取。论文设计了具有旋转与平移不变性的等价性检查器（区别于归一化均方误差等简单距离度量，能捕捉相机位置不同导致的基准/起点偏移），平均准确率 90.87%，相比现有最优方法生成的错误指令减少 30.20%；并利用该方法从录像打印出功能完整的仿冒物体。


### ControlLoc: Physical-World Hijacking Attack on Camera-based Perception in Autonomous Driving.
- **作者**: Chen Ma, Ningfei Wang, Zhengyu Zhao, Qian Wang, Qi Alfred Chen, Chao Shen
- **解读**: 针对自动驾驶摄像头感知提出物理世界劫持攻击 ControlLoc：由于 AD 感知系统不仅包含目标检测还包含多目标跟踪（MOT），后者要求多帧一致的检测才影响跟踪与驾驶决策，单纯对抗补丁攻击检测器的效果受限；此前提出的数字域劫持攻击旨在诱导碰撞等危险场景，但在物理世界效果有限，本文设计在物理世界中有效的劫持攻击。


### PipID: Light-Pupillary Response Based User Authentication for Virtual Reality.
- **作者**: Muchen Pan, Yan Meng, Yuxia Zhan, Guoxing Chen, Haojin Zhu
- **解读**: 提出 PipID，一种基于光瞳孔反应（对视觉刺激的瞳孔直径响应因人而异）的轻量级 VR 用户认证方法，仅使用头显内置的商用（COTS）眼动追踪器：随机显示不同颜色（波长）的光刺激，利用瞳孔直径响应进行认证。针对精度有限的 COTS 追踪器，方法缓解无关动作与眨眼的影响，并利用左右眼反应差异增强特征粒度；随机化的光序列帮助抵御重放攻击。在头显上以 52 名参与者测试，准确率达 98.65%，7 天与 14 天后分别保持 98% 与 91% 的稳健表现。


### RVISmith: Fuzzing Compilers for RVV Intrinsics.
- **作者**: Yibo He, Cunjian Huang, Xianmiao Qu, Hongdeng Chen, Wei Yang, Tao Xie
- **arXiv PDF**: [RVISmith: Fuzzing Compilers for RVV Intrinsics.](https://arxiv.org/pdf/2507.03773)
- **解读**: 针对编译器对 RISC-V 向量扩展（RVV）内建函数（intrinsics）的处理缺陷进行模糊测试：由于编译期信息不足限制了自动向量化性能，程序员常通过 intrinsics 手动操控 SIMD 指令，而编译器对这些内建函数的 bug 会导致错误计算结果、数据丢失、程序崩溃等安全隐患。论文提出 RVISmith 系统化发现此类编译器缺陷。


### Fuzzing Processing Pipelines for Zero-Knowledge Circuits.
- **作者**: Christoph Hochrainer, Anastasia Isychev, Valentin Wüstholz, Maria Christakis
- **arXiv PDF**: [Fuzzing Processing Pipelines for Zero-Knowledge Circuits.](https://arxiv.org/pdf/2411.02077)
- **解读**: 针对零知识（ZK）电路处理流水线的正确性验证：ZK 协议（用于认证、在线投票、区块链等）依赖处理 Circom、Noir 等领域特定语言编写的确定性程序（电路）的复杂流水线，其中的逻辑 bug 可能造成身份或资产被盗等灾难性后果。论文研究对 ZK 电路处理流水线进行模糊测试以发现逻辑缺陷的有效技术。


### Error Messages to Fuzzing: Detecting XPS Parsing Vulnerabilities in Windows Printing Components.
- **作者**: Yunpeng Tian, Feng Dong, Junhai Wang, Mu Zhang, Zhiniang Peng, Zesen Ye, Xiapu Luo, Haoyu Wang
- **解读**: 提出 PrintXPSurge，一个针对 Windows 打印组件中 XPS 文档解析器的覆盖率引导模糊测试系统（此前研究多聚焦 spooler、固件更新等服务控制面，打印机驱动中的文档解析器被忽视）。系统用 LLM 辅助修复方法自动生成符合约束的语义正确 XPS 文件，以渐进式状态重建方法满足服务工作流中各环节的依赖需求，并在崩溃时通过回溯确认其源于解析器；评测中其性能超越现有顶级模糊器，在 10 个主流品牌驱动中发现 102 个 bug，其中 17 个零日漏洞获微软与第三方厂商确认。


### SyzSpec: Specification Generation for Linux Kernel Fuzzing via Under-Constrained Symbolic Execution.
- **作者**: Yu Hao, Juefei Pu, Xingyu Li, Zhiyun Qian, Ardalan Amiri Sani
- **解读**: 针对 syzkaller 等内核模糊器依赖人工编写的系统调用描述、而现有自动生成方法（多面向设备驱动）对用户输入相关调用缺乏精度或分析范围有限的问题，提出 SyzSpec，用欠约束符号执行（under-constrained symbolic execution）自动生成 Linux 内核模糊测试所需的系统调用规范，以提升描述的精度与完整性。


### Validating Interior Gateway Routing Protocols via Equivalent Topology Synthesis.
- **作者**: Bing Shui, Yufan Zhou, Jielun Wu, Baowen Xu, Qingkai Shi
- **解读**: 提出 ToDiff，一种针对内部网关协议（IGP）实现的差分验证技术：与定义结构化状态机、便于验证的 TCP/IP 等通信协议不同，IGP 被设计为自动管理拓扑，常规以正确性为核心的验证技术无法直接适用。ToDiff 分三步：(1) 用生成算法创建随机但有效的网络；(2) 用语义引导的程序合成器生成等价拓扑程序；(3) 模拟比较，任何不一致即提示潜在 bug。在 OSPF 与 IS-IS 上评估优于现有方法，已发现 26 个 bug 且全部获开发者确认或修复。


### Disa: Accurate Learning-based Static Disassembly with Attentions.
- **作者**: Peicheng Wang, Monika Santra, Mingyu Liu, Cong Sun, Dongrui Zeng, Gang Tan
- **arXiv PDF**: [Disa: Accurate Learning-based Static Disassembly with Attentions.](https://arxiv.org/pdf/2507.07246)
- **解读**: 提出 Disa，一种基于注意力（attention）机制的学习型静态反汇编方法，利用学习模型提高对二进制代码进行静态反汇编的准确率。（无摘要，据标题推断）


### Efficient Constant-Size Linkable Ring Signatures for Ad-Hoc Rings via Pairing-Based Set Membership Arguments.
- **作者**: Min Xie, Zhengzhou Tu, Man Ho Au, Junbin Fang, Xuan Wang, Zoe Lin Jiang
- **解读**: 针对可链接环签名（LRS）在大规模临时环（ad-hoc ring）场景下的效率问题：LRS 允许匿名代表临时环签名且同一用户的多次签名可被链接，广泛用于电子投票、电子现金等隐私保护应用，需要短签名与快速验证。近期工作（如 DualDory (ESORICS'22)、LLRing (ESORICS'24)）通过离线预计算提升效率，但依赖静态环、限制其在环成员动态变化场景的适用性；基于累加器的常量大小签名方案同样存在局限。本文提出基于配对集合成员论证（pairing-based set membership arguments）的高效常量大小可链接环签名方案。


### Forking the RANDAO: Manipulating Ethereum's Distributed Randomness Beacon.
- **作者**: Ábel Nagy, János Tapolcai, István András Seres, Bence Ladóczki
- **解读**: 分析以太坊分布式随机信标 RANDAO 在当前机制下的可操纵性：即使实现高效，RANDAO 仍可通过故意从规范链省略区块而被操纵。此前研究显示，经济理性的参与者可在操纵结果带来更大财务收益时实施区块扣留（block withholding）或自私混合（selfish mixing）攻击；本文进一步分析分叉（forking）等操纵路径及其影响。


### Mining in Logarithmic Space with Variable Difficulty.
- **作者**: Loïc Miller, Dorian Pacaud, Nathanël Derousseaux-Lebert, Emmanuelle Anceaume, Romaric Ludinard
- **解读**: 针对工作量证明区块链的扩展性问题：区块链数据可分为应用部分与共识部分，后者随时间线性增长；文献 [1] 已在静态挖矿难度、固定矿工数量的假设下证明可将存储与通信开销指数级缩减。本文将该方案推广到可变挖矿难度场景，实验结果表明在难度动态变化时仍能保证存储与通信开销。


### Bitcoin Under Volatile Block Rewards: How Mempool Statistics Can Influence Bitcoin Mining.
- **作者**: Roozbeh Sarenche, Alireza Aghabagherloo, Svetla Nikova, Bart Preneel
- **arXiv PDF**: [Bitcoin Under Volatile Block Rewards: How Mempool Statistics Can Influence Bitcoin Mining.](https://arxiv.org/pdf/2411.11702)
- **解读**: 研究区块奖励减半后比特币矿工激励的变化及其安全影响：矿工收入由区块奖励与交易费组成，随着协议奖励趋近于零，交易费成为主要收入来源，这一转变引入奖励波动并催生或强化新威胁。既有分析要么采用固定奖励模型、要么采用高度简化的波动模型，忽视了内存池（mempool）行为的复杂性；本文结合内存池统计研究波动奖励下矿工行为如何影响比特币挖矿。


### On Frontrunning Risks in Batch-Order Fair Systems for Blockchains.
- **作者**: Eunchan Park, Taeung Yoon, Hocheol Nam, Deepak Maram, Min Suk Kang
- **解读**: 对以 Themis（CCS'23）为代表的批量序公平系统的抗前置交易（frontrunning）能力进行批判性分析：Themis 是追求先到先服务（FCFS）排序的高吞吐公平系统的关键参考，论文分析其核心属性「实用批量序公平」并评估其抵抗性，提出 Ambush 攻击——仅需一个恶意节点、攻击成本可忽略即可获得近 100% 成功率，利用系统为追求性能而高度优化的通信模型制造节点间微妙的临时信息不对称。论文揭示了安全与性能间的根本权衡：及时传播（以缓解前置交易）容易带来不可忽视的网络开销、降低整体吞吐，并证明通过将传播延迟到可容忍水平可以在保持高吞吐的同时兼顾两者；提出的延迟八卦（delayed gossiping）机制可以最小改动集成到现有系统。


### Aegis: Tethering a Blockchain with Primary-Chain Stake.
- **作者**: Yogev Bar-On, Roi Bar Zur, Omer Ben-Porat, Nimrod Cohen, Ittay Eyal, Matan Sitbon
- **arXiv PDF**: [Aegis: Tethering a Blockchain with Primary-Chain Stake.](https://arxiv.org/pdf/2406.05904)
- **解读**: 研究「系链」（tethering）区块链——以主链代币作为抵押品保障系链安全：抵押品保证运营者按预期行事直至撤出，与使用自身代币抵押的权益证明不同，主链抵押可规避自身代币的波动风险。论文指出现有方案要么依赖中心化、要么依赖极端假设（全同步通信、撤出后运营者仍正确、系链失败时可无限延迟撤出），并证明在部分同步下该问题无解；但在标准假设（部分节点同步）下存在部分解。提出的协议 Aegis 通过引用块定义委员会、检查点延续决策、重置在旧委员会过时后建立新委员会，在快速进展时期保持安全性且延迟低。


### Split Unlearning.
- **作者**: Yanna Jiang, Guangsheng Yu, Qin Wang, Xu Wang, Baihe Ma, Caijun Sun, Wei Ni, Ren Ping Liu
- **arXiv PDF**: [Split Unlearning.](https://arxiv.org/pdf/2308.10422)
- **解读**: 提出 Split Unlearning，一种面向分割学习（split learning）框架的机器遗忘方法，使参与方能够在保持分割训练流程的同时从模型中移除特定数据的影响。（无摘要，据标题推断）


### Slot: Provenance-Driven APT Detection through Graph Reinforcement Learning.
- **作者**: Wei Qiao, Yebo Feng, Teng Li, Zhuo Ma, Yulong Shen, Jianfeng Ma, Yang Liu
- **arXiv PDF**: [Slot: Provenance-Driven APT Detection through Graph Reinforcement Learning.](https://arxiv.org/pdf/2410.17910)
- **解读**: 提出 Slot，一种基于溯源图（provenance graph）与图强化学习的 APT 检测方法，通过强化学习智能体在图上的决策策略识别多步攻击路径。（无摘要，据标题推断）


### Combating Concept Drift with Explanatory Detection and Adaptation for Android Malware Classification.
- **作者**: Yiling He, Junchi Lei, Zhan Qin, Kui Ren, Chun Chen
- **arXiv PDF**: [Combating Concept Drift with Explanatory Detection and Adaptation for Android Malware Classification.](https://arxiv.org/pdf/2405.04095)
- **解读**: 针对基于机器学习的 Android 恶意软件分类中的概念漂移问题（恶意软件快速演化、尤其是新家族出现，可使分类精度跌至近随机水平）：既有研究主要聚焦漂移样本检测与专家主导的标签修订来指导重训练，但方法往往缺乏对概念的全面理解、对有效适应的指导有限，导致人工标注成本高。本文提出具解释性的漂移检测与自适应方法，以更低的标注成本实现有效适应。


### Rethinking Machine Unlearning in Image Generation Models.
- **作者**: Renyang Liu, Wenjie Feng, Tianwei Zhang, Wei Zhou, Xueqi Cheng, See-Kiong Ng
- **arXiv PDF**: [Rethinking Machine Unlearning in Image Generation Models.](https://arxiv.org/pdf/2506.02761)
- **解读**: 系统反思图像生成模型的机器遗忘（IGMU）：针对任务区分准则不清、缺乏评估框架、指标不可靠等实践缺口，对现有 SOTA 算法与标准进行 exhaustive 评估，发现多项关键缺陷与挑战。论文贡献包括：(1) CatIGMU 分层分类框架，提供详细实现指导并协助构建测试床；(2) EvalIGMU，涵盖五个方面的定量评估指标；(3) DataIGM 高质量数据集，可用于广泛评估与检测器/判定器训练。基准评测显示大多数现有算法难以兼顾不同维度，尤其在保留能力（preservation）与鲁棒性上表现不佳。


### TensorShield: Safeguarding On-Device Inference by Shielding Critical DNN Tensors with TEE.
- **作者**: Tong Sun, Bowen Jiang, Hailong Lin, Borui Li, Yixiao Teng, Yi Gao, Wei Dong
- **arXiv PDF**: [TensorShield: Safeguarding On-Device Inference by Shielding Critical DNN Tensors with TEE.](https://arxiv.org/pdf/2505.22735)
- **解读**: 针对端侧推理（移动与 IoT 设备本地部署第三方模型）面临的模型窃取（MS）与成员推理攻击（MIA）双重威胁：现有思路将模型整体部署于可信执行环境（TEE），但 TEE 内存受限使其难以在低延迟下实现全模型防护。TensorShield 提出只对关键 DNN 张量进行 TEE 屏蔽的思路，在有限的 TEE 内存下兼顾数据隐私保护与推理延迟。


### PoisonSpot: Precise Spotting of Clean-Label Backdoors via Fine-Grained Training Provenance Tracking.
- **作者**: Philemon Hailemariam, Birhanu Eshete
- **解读**: 提出 PoisonSpot，一种精确检测干净标签后门攻击的系统：受动态污点追踪启发，通过训练过程中的细粒度溯源追踪捕获并分析参数更新，依据样本谱系（lineage）为可疑样本赋予投毒分数，从而准确识别并剔除携带触发器的样本。在多个基准数据集与攻击场景下的评估显示其性能优于现有最优防御，持续保持高真阳率、低假阳率，并能有效缓解自适应对抗策略；在重训练与微调等多种设置下依然高效、稳健、可扩展。


### Pool: A Practical OT-based OPRF from Learning with Rounding.
- **作者**: Alex Davidson, Amit Deo, Louis Tremblay Thibault
- **解读**: 提出 Pool，一个基于学习取整（LWR）困难假设的简洁后量子不经意伪随机函数（OPRF）协议：在输入无关预处理下达到轮数最优，计算 F_sk(x)=⌈H(x)ᵀ·sk⌋_(q,p)（H 为随机预言）。在 128 位半诚实安全下，在线通信成本仅 11.9 kB、单线程计算时间低于 3 ms（开源实现），比基于知名后量子 PRF 的构造更高效，甚至与基于较弱假设的构造相当；同时为多方隐私集合操作等成熟应用提供高性能变体。

### Zero-Knowledge AI Inference with High Precision.
- **作者**: Arman Riasi, Haodi Wang, Rouzbeh Behnia, Viet Vo, Thang Hoang
- **解读**: AIaaS 场景下用户查询服务商托管的模型并接收推理结果，在便利资源受限用户的同时带来可验证性与隐私两方面担忧；零知识证明虽能同时应对，但 AI 中非线性运算导致证明成本高昂，且依赖定点数表示还会造成精度损失。论文针对高精度零知识 AI 推理提出方案，兼顾可验证性、隐私与精度。


### New Permutation Decomposition Techniques for Efficient Homomorphic Permutation.
- **作者**: Xirong Ma, Junling Fang, Chunpeng Ge, Dung Hoang Duong, Yali Jiang, Yanbin Li, Willy Susilo, Lizhen Cui
- **解读**: 同态置换是批量编码同态加密下隐私保护计算的基础，几乎支撑所有矩阵运算并主导其复杂度，而置换分解这一潜在优化途径此前鲜有探索。论文定义置换的理想形式并给出搜索深度 1 分解的算法，据此证明特定转置（HMT）与乘法（HMM）算法所用置换的完整深度可分解性，带来渐进的提速与旋转密钥减少。将增强版 HMM 组件替换进最佳加密神经网络推理框架后延迟降低最高 3.9 倍；对无法理想分解的置换还设计了新计算方法，在有限预算下较最先进技术提速 1.69 倍。


### Leveraging Discrete CKKS to Bootstrap in High Precision.
- **作者**: Hyeongmin Choe, Jaehyung Kim, Damien Stehlé, Elias Suvanto
- **解读**: CKKS 全同态加密支持对近似复数向量计算，约 20 位精度通常够用，但为获得 IND-CPA-D 安全性、安全门限 FHE 与电路隐私，已知方法都依赖噪声洪泛，精度需求可能达到 80 位以上，而高精度引导（bootstrapping）因每次乘法都消耗模数、预算有限而极难实现。现有最先进方法 Meta-BTS 通过多次执行中等精度引导实现高精度引导，模数消耗与基础方法相近但损害延迟。论文利用离散 CKKS（discrete CKKS）实现高精度引导。


### Grafting: Decoupled Scale Factors and Modulus in RNS-CKKS.
- **作者**: Jung Hee Cheon, Hyeongmin Choe, Minsik Kang, Jaehyung Kim, Seonghak Kim, Johannes Mono, Taeyeong Noh
- **解读**: RNS-CKKS 中缩放因子（决定数值精度）与模数（定义密码学结构）被刚性耦合，而二者分别承担正确性与密码学结构两种不同角色，这种依赖导致缺乏合适的 NTT 素数、精度灵活性受限等设计约束并最终造成效率损失。论文提出 Grafting，将 RNS-CKKS 中的缩放因子与模数解耦。


### Towards Verifiable FHE in Practice: Proving Correct Execution of TFHE's Bootstrapping using plonky2.
- **作者**: Louis Tremblay Thibault, Michael Walter
- **解读**: 论文首次在实践中证明完整的 FHE 引导操作可用 SNARK 验证：设计算术电路并用 plonky2 证明，在 AWS Hpc7a 实例上 20 分钟内完成，证明大小约 200 kB、验证时间不足 10 ms。以 TFHE 可编程引导为基础并做少量修改以更高效地表示运算（保持功能与安全性），并利用 plonky2 的递归能力实现基于递归的 IVC 方案以节省内存。最后在 UC 模型下给出可捕获真实应用主动攻击的安全证明，使可验证 FHE 适配实际应用。


### Probabilistic Skipping-Based Data Structures with Robust Efficiency Guarantees.
- **作者**: Marc Fischlin, Moritz Huppert, Sam A. Markelon
- **解读**: 哈希表、跳表、treap 等概率数据结构通过随机化层级实现“跳跃”，平均亚线性查询复杂度且结果完全正确，但传统上假设输入与结构内部随机状态独立，该假设在恶意环境中不成立，可能导致复杂度显著上升。论文给出针对三类结构的自适应攻击（哈希表情形可致指数级退化），提出简单修改使原设计获得对自适应对手的可证明安全性，并将论证形式化为自适应对手性质保持（AAPC）概念。实验与理论分析结果吻合。


### ShiftPIR: An Efficient PIR System with Gravity Shifting from Client to Server.
- **作者**: Zihan Wang, Lutan Zhao, Ming Luo, Zhiwei Wang, Haoqi He, Wenzhe Lv, Xuan Ding, Dan Meng, Rui Hou
- **解读**: 论文提出单服务器 PIR 协议 ShiftPIR，将计算与通信开销从客户端转移到服务端，利用资源受限客户端与计算密集、可并行扩展服务端之间的不对称性。客户端仅传输由预上传种子密文派生的紧凑明文偏移，服务端用同态旋转重构完整查询密文，消除昂贵的密文传输；并设计高并行化、无数据依赖的扩展机制支持 GPU 执行。实验显示客户端延迟降至微秒级、开销保持在非隐私基线 4 倍以内，较先前协议（104–105 倍开销）大幅领先，端到端延迟较最先进 YPIR 降低最多 26 倍。


### Updatable aPAKE: Security Against Bulk Precomputation Attacks.
- **作者**: Dennis Dayanikli, Anja Lehmann
- **解读**: aPAKE 支持客户端与服务器基于共享口令安全建立密钥并抵抗离线攻击，但标准模型不保证抗预计算攻击；已有协议常依赖加盐口令哈希，盐虽在认证时明文发送，在多用户大规模场景下仍显著增加攻击难度。然而盐的防护是静态的，攻击者可逐步提取口令值。论文提出可更新 aPAKE（updatable aPAKE），针对批量预计算攻击提供更强的安全保证。


### Founding Zero-Knowledge Proof of Training on Optimum Vicinity.
- **作者**: Gefei Tan, Adrià Gascón, Sarah Meiklejohn, Mariana Raykova, Xiao Wang, Ning Luo
- **解读**: 零知识训练证明（zkPoT）允许一方证明模型在已提交数据集上被正确训练，且不泄露模型或数据集的额外信息。现有协议对完整迭代训练过程做零知识证明，证明者工作量与迭代次数呈线性关系。论文将训练证明建立在最优邻域（optimum vicinity）之上，以突破线性迭代工作量。


### Revisiting Keyed-Verification Anonymous Credentials.
- **作者**: Michele Orrù
- **解读**: 论文重新审视带密钥验证的匿名凭证（keyed-verification anonymous credentials）方案的设计与性质。「（无摘要，据标题推断）」


### Subversion-resilient Key-exchange in the Post-quantum World.
- **作者**: Kévin Duverger, Pierre-Alain Fouque, Charlie Jacomme, Guilhem Niot, Cristina Onete
- **解读**: 抗颠覆认证密钥交换（AKE）旨在即使敌手篡改协议实现的部分组件仍保持安全 AKE 的保证，一种实现方式是使用反向防火墙（RF）这一不可信第三方来恢复安全性。近期工作指出为实用信道建立设计 RF 存在挑战。论文在后量子世界中研究抗颠覆密钥交换。


### Poisoning Attacks to Local Differential Privacy for Ranking Estimation.
- **作者**: Pei Zhan, Peng Tang, Yangzhuo Li, Puwen Wei, Shanqing Guo
- **arXiv PDF**: [Poisoning Attacks to Local Differential Privacy for Ranking Estimation.](https://arxiv.org/pdf/2506.24033)
- **解读**: 本地差分隐私（LDP）通过对输入扰动提供可否认性，但也因此易受投毒攻击；论文首次提出针对排序估计的投毒攻击，伪造用户并非简单调整目标项频率，而是利用有限数量的伪造用户精准修改频率以改变物品排名、最大化收益。为此引入攻击成本与最优集合概念，针对 kRR、OUE、OLH 协议提出相应策略（迭代选择物品并分配用户、基于频率增量变化确定集合、针对 OLH 的基于预像哈希的调和函数），并给出量化成功概率的置信度策略。理论与实证均验证攻击有效性，代码已公开。


### Mitigating Data Poisoning Attacks to Local Differential Privacy.
- **作者**: Xiaolin Li, Ninghui Li, Boyang Wang, Wenhai Sun
- **arXiv PDF**: [Mitigating Data Poisoning Attacks to Local Differential Privacy.](https://arxiv.org/pdf/2506.02156)
- **解读**: LDP 的分布式特性招致数据投毒攻击，威胁其支撑的应用；论文为常用频率估计提出综合防御框架，包含恶意用户检测、攻击模式识别、受损效用恢复等一系列新防御，并考察新出现的自适应攻击活动。所提方法能精确识别伪造报告，使聚合在“干净”数据上进行；当攻击隐蔽、难以直接过滤用户时，可进一步识别隐藏模式辅助服务提供方决策。这些方法无需额外信息、计算开销极小，实验显示性能优异且较先前工作大幅提升，并对后处理恢复方法给出实证分析与实践建议。


### Riddle Me This! Stealthy Membership Inference for Retrieval-Augmented Generation.
- **作者**: Ali Naseh, Yuefeng Peng, Anshuman Suri, Harsh Chaudhari, Alina Oprea, Amir Houmansadr
- **arXiv PDF**: [Riddle Me This! Stealthy Membership Inference for Retrieval-Augmented Generation.](https://arxiv.org/pdf/2502.00306)
- **解读**: RAG 使 LLM 借助外部知识库生成有依据的响应，但检索文档进入模型上下文带来成员推理风险；现有成员推断与数据提取攻击多依赖越狱或刻意构造的非自然查询，易被常见查询重写机制检测并挫败。论文提出针对数据存储库的攻击 øurattack：构造仅当目标文档存在时才能回答的自然文本查询，约 30 次查询即可成功且保持隐蔽，现有攻击生成的对抗提示被检测频率比其高约 76 倍。在多种配置下 TPR@1%FPR 较先前攻击提升 2 倍，且每次文档推断成本低于 0.02 美元。


### Exploiting the Shared Storage API.
- **作者**: Alexandra Nisenoff, Deian Stefan, Nicolas Christin
- **解读**: 为替代第三方 cookie，Google 在 Privacy Sandbox 中引入 Shared Storage API，允许第三方存储不按顶层网站分区的数据但限制其读访问，以保留部分良性功能并缓解跨站用户重识别等隐私危害。论文发现该 API 的实现与设计存在缺陷，既允许跨站重新识别用户，也造成超出 Google 预期的数据泄露。论文给出多种绕过其关键目标的攻击，并讨论扩展与缓解策略；已向 Google 负责任披露，但多数攻击在 Chrome 中仍可能发生。


### Fingerprinting SDKs for Mobile Apps and Where to Find Them: Understanding the Market for Device Fingerprinting.
- **作者**: Michael A. Specter, Mihai Christodorescu, Abbie Farr, Bo Ma, Robin Lassonde
- **arXiv PDF**: [Fingerprinting SDKs for Mobile Apps and Where to Find Them: Understanding the Market for Device Fingerprinting.](https://arxiv.org/pdf/2506.22639)
- **解读**: 论文对移动应用生态中类似指纹识别的行为进行大规模分析，采取市场视角聚焦第三方跟踪 SDK。数据集包含 22.8 万余个来自主流 Maven 仓库的 SDK 与 17.8 万个从 Google Play 收集的 Android 应用，静态流水线检测 500 类信号的对外泄露，据作者所知是迄今最大规模的 SDK 研究。


### PAnDA: Rethinking Metric Differential Privacy Optimization at Scale with Anchor-Based Approximation.
- **作者**: Ruiyao Liu, Chenxi Qiu
- **arXiv PDF**: [PAnDA: Rethinking Metric Differential Privacy Optimization at Scale with Anchor-Based Approximation.](https://arxiv.org/pdf/2509.08720)
- **解读**: 论文提出 PAnDA，以锚点近似（anchor-based approximation）重新思考大规模场景下的度量差分隐私（metric differential privacy）优化问题。「（无摘要，据标题推断）」


### Lock the Door But Keep the Window Open: Extracting App-Protected Accessibility Information from Browser-Rendered Websites.
- **作者**: Haichuan Xu, Runze Zhang, Mingxuan Yao, David Oygenblik, Yizhi Huang, Jeman Park, Brendan Saltaformaggio
- **解读**: Android 无障碍（a11y）服务常被恶意软件滥用，开发者需在原生应用与移动网站中同时保护 a11y 内容访问，但二者保护机制存在错位；先前研究聚焦原生应用内信息的攻防，本文发现应用保护的信息可经其浏览器渲染的网页端被检索，使浏览器用户比应用用户更易受攻击。为此开发自动化分析流水线 SOMBRA 检验受保护应用的浏览器端泄露：分析 294 个服务（其中 29 个部署了应用端保护、256 个视图），在 Chrome、Firefox、Brave、Edge 中分别发现 241、402、244、251 个受保护视图对应元素在网页端暴露，泄露内容含敏感个人信息。SOMBRA 还表明多数服务未采纳防护，因为现有机制要么无效要么妨碍内容可用性。


### BACScan: Automatic Black-Box Detection of Broken-Access-Control Vulnerabilities in Web Applications.
- **作者**: Fengyu Liu, Yuan Zhang, Enhao Li, Wei Meng, Youkun Shi, Qianheng Wang, Chenlin Wang, Zihan Lin, Min Yang
- **解读**: 越权访问控制（BAC）漏洞长期位列 OWASP Top 10 前列，但有效可靠的检测手段仍然有限；论文提出黑盒检测方法 BACScan。与仅检查读访问的响应相似性 oracle 不同，BACScan 引入反馈驱动 oracle，通过分析操作相关页面与操作反馈推断是否发生了写或修改操作。在 20 个真实应用上发现 89 个漏洞（其中 54 个此前未报告），优于最先进工具，已向受影响厂商报告并获 35 个新 CVE 编号。


### Analyzing PDFs like Binaries: Adversarially Robust PDF Malware Analysis via Intermediate Representation and Language Model.
- **作者**: Side Liu, Jiang Ming, Guodong Zhou, Xinyi Liu, Jianming Fu, Guojun Peng
- **arXiv PDF**: [Analyzing PDFs like Binaries: Adversarially Robust PDF Malware Analysis via Intermediate Representation and Language Model.](https://arxiv.org/pdf/2506.17162)
- **解读**: 恶意 PDF 是常见攻击向量，基于机器学习的分类器易受对抗攻击，而底层特征工程过时使鲁棒性问题难以根本解决。论文提出基于 PDF 对象中间表示（PDFObj IR）的新方法：以类汇编语言框架描述 PDF 对象，用预训练模型提取语义特征，并借鉴程序分析构建引用图（Reference Graph）捕捉结构特征，实现语义与结构双特征分析。实验表明分类器在保持强检测性能的同时误报率极低（基线数据集仅 0.07%）。


### Local Frames: Exploiting Inherited Origins to Bypass Content Blockers.
- **作者**: Alisha Ukani, Hamed Haddadi, Alex C. Snoeren, Peter Snyder
- **arXiv PDF**: [Local Frames: Exploiting Inherited Origins to Bypass Content Blockers.](https://arxiv.org/pdf/2506.00317)
- **解读**: 论文研究本地帧（即加载 about:blank 等内容 iframe）被大量主流 Web 安全与隐私工具错误处理的现象，用户因此仍暴露于浏览器指纹识别、基于 cookie 的跟踪、数据外泄等其想防御的攻击。各工具以不同方式出错，但根因一致：遗留功能与安全边界以意料之外的方式交互，导致系统性的漏洞。


### Enhanced Web Application Security Through Proactive Dead Drop Resolver Remediation.
- **作者**: Jonathan Fuller, Mingxuan Yao, Saumya Agarwal, Srimanta Barua, Taleb Hirani, Amit Kumar Sikder, Brendan Saltaformaggio
- **解读**: 论文提出通过主动修复 dead drop resolver（信息投递/解析点）来增强 Web 应用安全性的方案。「（无摘要，据标题推断）」


### ForeDroid: Scenario-Aware Analysis for Android Malware Detection and Explanation.
- **作者**: Jiaming Li, Sen Chen, Chunlian Wu, Yuxin Zhang, Lingling Fan
- **解读**: Android 恶意软件持续演化，现有检测系统在泛化、细粒度检测与可解释性上存在不足，常依赖僵硬规则或无法恢复完整因果行为路径。论文提出 ForeDroid，将恶意意图建模为功能场景内的不一致性：聚类语义连贯的场景，提取敏感 API 调用链并用 LLM 总结为自然语言，将摘要嵌入后与同场景良性分布做无监督异常检测，高危行为再由 LLM 驱动模块生成解释报告。在时间分割设置下零日检测优于 MaMaDroid、MalScan、DeepRefiner 与持续学习方法，在人工标注的 GPMalware 数据集上 F1 达 0.94 并超越 ProMal。


### Logical Relations for Formally Verified Authenticated Data Structures.
- **作者**: Simon Oddershede Gregersen, Chaitanya Agarwal, Joseph Tassarotti
- **arXiv PDF**: [Logical Relations for Formally Verified Authenticated Data Structures.](https://arxiv.org/pdf/2501.10802)
- **解读**: 认证数据结构允许不可信第三方执行操作并生成可用于验证输出的证明，其正确开发与实现颇具挑战；论文为自动生成认证版本的程序库给出安全性与正确性的形式证明。证明基于新的关系分离逻辑，用于推理使用抗碰撞密码哈希函数的程序，并据此构建类型系统的两个语义模型，论证抽象如何强制正确性。利用这些模型还证明了若干优化，并表明手写优化实现与自动生成代码可被可靠地链接；全部结果已在 Rocq 证明器中用 Iris 框架机械化。


### Jazzline: Composable CryptoLine Functional Correctness Proofs for Jasmin Programs.
- **作者**: José Bacelar Almeida, Manuel Barbosa, Gilles Barthe, Lionel Blatter, Gustavo Xavier Delerue Marinho Alves, João Diogo Duarte, Benjamin Grégoire, Tiago Oliveira, Miguel Quaresma, Pierre-Yves Strub, Ming-Hsien Tsai, Bow-Yaw Wang, Bo-Yin Yang
- **解读**: Jasmin 是面向高速高保证密码学的编程语言，其正确性证明通常在 EasyCrypt 中演绎完成，通用、模块化且可组合，但难以扩展到底层架构相关例程；CryptoLine 提供对代数丰富的密码例程的半自动验证，且自成体系、可集成进高层形式验证。论文展示如何用 CryptoLine 可靠地消解复杂程序功能正确性证明的子目标：扩展注解并给出自动翻译模型（多数变换被认证），同时形式化实现提取语义证明。以 X-Wing 混合 KEM 为用例，给出其首个验证实现。


### Breaking and Provably Restoring Authentication: A Formal Analysis of SPDM 1.2 including Cross-Protocol Attacks.
- **作者**: Cas Cremers, Alexander Dax, Aurora Naska
- **解读**: SPDM（Security Protocol and Data Model）是 DMTF 制定中的协议标准，获 Broadcom、Cisco、Dell、Google、HP、IBM、Intel、NVIDIA 等主要厂商支持，为 PCIe 与 CXL 等通信场景提供平台安全机制。论文对 SPDM 1.2 进行形式分析，包括跨协议攻击，发现认证破坏并给出可证明的恢复方法。


### Approximate Algorithms for Verifying Differential Privacy with Gaussian Distributions.
- **作者**: Bishnu Bhusal, Rohit Chadha, A. Prasad Sistla, Mahesh Viswanathan
- **arXiv PDF**: [Approximate Algorithms for Verifying Differential Privacy with Gaussian Distributions.](https://arxiv.org/pdf/2509.08804)
- **解读**: 采用高斯分布的差分隐私算法验证鲜有研究；论文提出新方法近似计算无环程序中从离散与连续分布（含可计算密度函数，包括 Laplace）采样的概率，并证明这类程序的 (ε,δ)-差分隐私“几乎可判定”，即除有限个 δ 值外判定问题可解。算法结合积分近似与尾界以任意精度计算概率，实现为基于 FLINT 高精度计算的工具 DipApprox，并加入可扩展性优化；在 Sparse Vector Technique 与 Noisy Max 等基础隐私算法上验证了有效性并检测到违例。


### ILA: Correctness via Type Checking for Fully Homomorphic Encryption.
- **作者**: Tarakaram Gollamudi, Anitha Gollamudi, Joshua Gancher
- **arXiv PDF**: [ILA: Correctness via Type Checking for Fully Homomorphic Encryption.](https://arxiv.org/pdf/2509.11559)
- **解读**: 基于 RLWE 的全同态加密（FHE）方案在加密时引入小噪声，噪声随同态操作累积，超过临界值即产生错误输出，且有限模数运算还带来回绕错误，使应用开发必须仔细跟踪噪声，而现有库与编译器对噪声的静态支持有限。论文提出 ILA，通过类型检查保证 FHE 电路的正确性。


### Protocols to Code: Formal Verification of a Secure Next-Generation Internet Router.
- **作者**: João C. Pereira, Tobias Klenze, Sofia Giampietro, Markus Limbeck, Dionysios Spiliopoulos, Felix A. Wolf, Marco Eilers, Christoph Sprenger, David A. Basin, Peter Müller, Adrian Perrig
- **解读**: 论文给出首个经形式验证的互联网路由器，其属于 SCION 架构；路由器在对抗环境中运行用于安全包转发的密码协议。验证同时覆盖协议的网络级安全属性与底层实现：用 Isabelle/HOL 通过细化建立系列模型，并用自动化程序验证器证明路由器的 Go 代码无崩溃、无数据竞争且符合细化中最具体的模型，两层验证被可靠地链接。


### SyzParam: Incorporating Runtime Parameters into Kernel Driver Fuzzing.
- **作者**: Yue Sun, Yan Kang, Chenggang Wu, Kangjie Lu, Jiming Wang, Xingwei Li, Yuhao Hu, Jikai Ren, Yuanming Lai, Mengyao Xie, Zhe Wang
- **解读**: 论文提出 SyzParam，将运行时参数融入内核驱动模糊测试（fuzzing）以提升覆盖与漏洞发现能力。「（无摘要，据标题推断）」


### Reviving Discarded Vulnerabilities: Exploiting Previously Unexploitable Linux Kernel Bugs Through Control Metadata Fields.
- **作者**: Hao Zhang, Jian Liu, Jie Lu, Shaomin Chen, Tianshuo Han, Bolun Zhang, Xiaorui Gong
- **解读**: 论文研究通过控制元数据字段（control metadata fields）利用先前被认为不可利用的 Linux 内核缺陷，使被废弃的漏洞重新变得可利用。「（无摘要，据标题推断）」


### BASTAG: Byte-level Access Control on Shared Memory using ARM Memory Tagging Extension.
- **作者**: Junseung You, Jiwon Seo, Kyeongryong Lee, Yeongpil Cho, Yunheung Paek
- **解读**: 论文提出 BASTAG，利用 ARM 内存标记扩展（Memory Tagging Extension）对共享内存实施字节级访问控制。「（无摘要，据标题推断）」


### Intent-aware Fuzzing for Android Hardened Application.
- **作者**: Seongyun Jeong, Minseong Choi, Haehyun Cho, Seokwoo Choi, Hyungsub Kim, Yuseok Jeon
- **解读**: Android 应用加固技术的普及使现有分析难以处理加固应用、覆盖率受限，主要原因是难以获取 Intent 的详细信息——Intent 是应用间通信与特定事件执行的关键组件。论文提出面向加固 Android 应用的 Intent 感知模糊测试方法。


### Securing Mixed Rust with Hardware Capabilities.
- **作者**: Jason Zhijingcheng Yu, Fangqi Han, Kaustab Choudhury, Trevor E. Carlson, Prateek Saxena
- **解读**: Rust 通过所有权、借用与别名异变（AXM）原则防止内存安全缺陷与数据竞争，但实际项目常含 unsafe Rust、FFI、内联汇编等混合代码，编译器无法静态强制这些原则，导致漏洞。论文提出 CapsLock，在机器层面运行时检测原则违规；引入新颖的 revoke-on-use 抽象——通过能力访问对象时隐式失效指向该对象的其他能力，自动提供时间安全而无需软件显式指定失效。原型在 QEMU 上实现，兼容性高（通过 99.7% 内置测试用例与 100 个最流行 crate），并发现 8 个此前未知的 crate 违规。


### PromeFuzz: A Knowledge-Driven Approach to Fuzzing Harness Generation with Large Language Models.
- **作者**: Yuwei Liu, Junquan Deng, Xiangkun Jia, Yanhao Wang, Minghua Wang, Lin Huang, Tao Wei, Purui Su
- **解读**: 论文提出 PromeFuzz，一种用大语言模型以知识驱动方式生成模糊测试 harness 的方法。「（无摘要，据标题推断）」


### Swallow: A Transfer-Robust Website Fingerprinting Attack via Consistent Feature Learning.
- **作者**: Meng Shen, Jinhe Wu, Junyu Ai, Qi Li, Chenchen Ren, Ke Xu, Liehuang Zhu
- **解读**: Tor 网络上的网站指纹（WF）攻击通过流量模式识别用户访问的网站，但真实环境网络条件多样且存在 WF 防御，现有最先进方法或假设训练与部署条件一致，或面对防御时性能退化。论文提出迁移鲁棒的 Swallow 攻击：设计一致交互特征（CIF）表示对齐不同采集环境的分布、提取一致特征，并设计三种数据增强算法模拟条件变化。在十个数据集（含自采与公开）的闭/开世界评估中显著优于最先进方法，每网站仅 5 个标签微调即可平均提升准确率 17.50%。


### FlowSentry: Accelerating NetFlow-based DDoS Detection.
- **作者**: Xiaoyu He, Xiaohui Xie, Xin Wang, Lei Zhang, Kun Xie, Lin Chen, Yong Cui
- **解读**: 基于 NetFlow 的 DDoS 检测被 ISP 广泛用于上游多点场景抗洪泛攻击，但 NetFlow 非即时性（路由器聚合一段时间后才上报）带来固有检测延迟；现有工作聚焦路由器端上报优化，需软硬件升级、部署成本高。论文提出 FlowSentry，在服务器端加速攻击识别：采用双层过滤范式处理高频记录，含 ADWindow（基于 sketch 的滑动窗口，保留可能异常信息、滤除良性流以降低计算开销）与 STAnalyzer（利用跨路由器流量相关性从部分上报中高效推断异常增长模式）。模拟骨干网实验表明准确率更优且延迟较现有方法降低最多 65.63%。


### 1BIT: Persistent Path Validation with Customized Noise Signal Characteristics.
- **作者**: Keji Miao, Jie Yuan, Xinghai Wei, Xingwu Wang, Dongqi Han, Haiguang Wang, Runshan Hu, Xiaoyong Li, Zitong Jin, Wenqi Chen
- **解读**: 路径感知网络允许发送方选择或影响传输路径，需路径验证机制支撑，但现有方法在安全、协议头开销与计算成本之间存在“三难”权衡。受持久零信任架构启发，论文提出 1BIT 协议：开销降低 57% 以上且数据流安全鲁棒，故障检测需求降低 72% 且位置可精确定位；仅用哈希与少量二进制运算即可获得高吞吐，16 核 CPU 上可处理超过 100 Gbps。


### RebirthDay Attack: Reviving DNS Cache Poisoning with the Birthday Paradox.
- **作者**: Xiang Li, Mingming Zhang, Zuyao Xu, Fasheng Miao, Yuqi Qiu, Baojun Liu, Jia Zhang, Xiaofeng Zheng, Haixin Duan, Zheli Liu, Yunhai Zhang, Dunqiu Fan
- **解读**: DNS 缓存投毒攻防持续多年，但尚无工作系统重审基于生日悖论的经典攻击是否仍然有效；论文提出 RebirthDay，针对递归解析器与转发器，利用新发现的、符合协议规范的扩展实现缺陷（如 ECS 选项处理中缺乏或不正确的连贯性检查）绕过旨在防攻击的查询聚合机制。22 款主流软件中 18 款易受攻击，影响 16 个路由器厂商、14 个公共服务与 36.5 万个（15%）开放解析器；已获 BIND、Unbound、PowerDNS、Quad9 等 8 个厂商确认并分配 50 个 CVE 编号。


### 5G-RNAKA : A Random Number-based Authentication and Key Agreement Protocol for 5G Systems.
- **作者**: Hui Li, Haotian Li, Chi Ma, Jingjing Guan, Junchi Zeng, Haonan Feng, Ziming Zhao
- **解读**: 3GPP 定义的 5G-AKA 协议存在可链接性、同步失败、序列号（SQN）暴露等攻击，威胁用户隐私与服务可用性，而现有改进多沿用 3G 遗留的 SQN 机制，保留缺陷或带来高开销。论文提出 5G-RNAKA：消除计数器，改用 USIM/UE 生成的随机数做会话标识，并将该随机数嵌入网络回复消息以抵御针对 UE 的重放攻击；移除 SQN 机制增强抗链接性，同时支持 UE 认证服务网络（SN）、缓解 SN 冒充。用 Tamarin Prover 形式验证隐私、认证与保密目标，实现评估显示通信效率提升、存储开销降低，其特性也与 6G 演进趋势契合。


### Discovering and Exploiting IoT Device Hidden Attributes: A New Vulnerability in Smart Homes.
- **作者**: Xuening Xu, Chenglong Fu, Xiaojiang Du, Bo Luo
- **解读**: 论文研究智能家居中 IoT 设备的隐藏属性，发现并利用这一新型漏洞。「（无摘要，据标题推断）」


### MM4flow: A Pre-trained Multi-modal Model for Versatile Network Traffic Analysis.
- **作者**: Luming Yang, Lin Liu, Junjie Huang, Zhuotao Liu, Shiyu Liang, Shaojing Fu, Yongjun Wang
- **解读**: 网络流量分析中单模态方法对不同任务的适应性不足，且既有预训练仅用 GB 级流量易过拟合；论文提出多模态模型 MM4flow，将流量流分为原始字节流与传输模式两种模态，分别封装内容与行为信息。MM4flow 采用单模态预训练加微调的流程，开发高效采集方案实现 TB 级预训练（真实流量超 70 TB），为每种模态用适配流量的改进 BERT 预训练，下游用基于交叉注意力的模态融合模块整合双模态线索、仅需少量标注数据。在六个公开数据集上精度优于基线，加密隧道下网站识别较单模态模型提升 84%，并显著降低训练数据依赖。


### Sylva: Tailoring Personalized Adversarial Defense in Pre-trained Models via Collaborative Fine-tuning.
- **作者**: Tianyu Qi, Lei Xue, Yufeng Zhan, Xiaobo Ma
- **解读**: 边缘计算中预训练模型推理广泛部署于移动端，设备易受直接对抗攻击；联邦对抗训练（FAT）可在保护隐私的同时提升鲁棒性，但常产生泛化的全局模型，难应对客户端异构数据分布、个性化不足且通信开销大。论文提出 Sylva 两阶段协作框架：阶段 1 用 LoRA 本地微调实现个性化，仅上传 LoRA 参数进行联邦聚合以大幅降低通信成本；阶段 2 引入基于博弈的层选择策略在良性数据准确率上进一步精化模型，为每个客户端提供平衡的定制防御。基准数据集上效率较最先进算法提升最高 50 倍，准确率提升 29.5% 与 50.4%。


### Unmask Tampering: Efficient Document Tampering Localization under Recapturing Attacks with Real Distortion Knowledge.
- **作者**: Changsheng Chen, Wenyu Chen, Yinyin Lin, Bin Li, Jiwu Huang
- **解读**: 论文针对重拍攻击（recapturing attacks）场景，利用真实畸变知识高效定位文档篡改区域。「（无摘要，据标题推断）」


### RAG-WM: An Efficient Black-Box Watermarking Approach for Retrieval-Augmented Generation of Large Language Models.
- **作者**: Peizhuo Lv, Mengjie Sun, Hao Wang, XiaoFeng Wang, Shengzhi Zhang, Yuxuan Chen, Kai Chen, Limin Sun
- **arXiv PDF**: [RAG-WM: An Efficient Black-Box Watermarking Approach for Retrieval-Augmented Generation of Large Language Models.](https://arxiv.org/pdf/2501.05249)
- **解读**: RAG 广泛用于领域特定、知识密集与隐私敏感任务，但存在被窃取部署或商业化的知识产权（IP）侵权风险；现有数据库水印需白盒访问不适用于 RAG 知识库，且对手 LLM 的后处理会破坏文本水印。论文提出黑盒“知识水印”方案 RAG-WM：采用多 LLM 交互框架（水印生成器、影子 LLM/RAG、判别器），基于实体-关系元组生成文本注入目标 RAG。在三个领域、两个任务、四个基准 LLM 上验证其能有效检测被窃取的 RAG，对释义、无关内容移除、插入、扩展等攻击鲁棒，且能规避检测方法。


### Membership Inference Attacks as Privacy Tools: Reliability, Disparity and Ensemble.
- **作者**: Zhiqi Wang, Chengyu Zhang, Yuetian Chen, Nathalie Baracaldo, Swanand Ravindra Kadhe, Lei Yu
- **arXiv PDF**: [Membership Inference Attacks as Privacy Tools: Reliability, Disparity and Ensemble.](https://arxiv.org/pdf/2506.13972)
- **解读**: 成员推断攻击（MIA）威胁模型隐私，并被广泛用作评估、审计、遗忘等隐私工具；先前研究多聚焦 AUC、准确率、TPR@low FPR 等性能指标，忽视了不同攻击之间及同一方法不同实例之间的差异，影响 MIA 作为评估工具的可靠性与完整性。论文用基于覆盖度与稳定性分析的新框架系统研究这些差异、成因及其对评估的影响，并提出含三种策略的集成方法综合最先进攻击优势。这不仅构建了更强的攻击，也提供更稳健、全面的评估方法。


### Prompt Inference Attack on Distributed Large Language Model Inference Frameworks.
- **作者**: Xinjian Luo, Ting Yu, Xiaokui Xiao
- **arXiv PDF**: [Prompt Inference Attack on Distributed Large Language Model Inference Frameworks.](https://arxiv.org/pdf/2503.09291)
- **解读**: 论文研究针对分布式大语言模型推理框架的提示词（prompt）推断攻击。「（无摘要，据标题推断）」


### Differentially Private Access in Encrypted Search: Achieving Privacy at a Small Cost?
- **作者**: Daniel Pöllmann, Tianxin Tang
- **解读**: 加密搜索在保护外包数据的同时仍泄漏查询与数据信息，访问模式等泄漏已被泄漏滥用攻击利用以恢复数据，研究重心转向在隐私与性能间寻找平衡；用 ORAM 等结构隐藏访问模式的方案带宽成本高昂。论文研究在加密搜索中引入差分隐私的访问（differentially private access），探讨以较小成本实现隐私。


### Silent Threshold Traitor Tracing & Enhancing Mempool Privacy.
- **作者**: Anirban Chakrabarti, Monosij Maitra, Arup Mondal, Kushaz Sehgal
- **解读**: DeFi 中加密货币与区块链的商业化使资源雄厚的对手可通过多种最大可提取价值（MEV）策略操纵市场，用户希望在交易执行前隐藏交易细节，待处理交易池（mempool）的隐私因此成为重要目标，去中心化门限加密（TE）系统近期被证明尤为有效。论文提出静默门限叛徒追踪（silent threshold traitor tracing）方案并用于增强 mempool 隐私。


### PULSE: Parallel Private Set Union for Large-Scale Entities.
- **作者**: Jiahui Gao, Son Nguyen, Marina Blanton, Ni Trieu
- **解读**: 多方私有集合并集（mPSU）允许多方计算输入集合并集而不泄露额外信息；现有高效协议分为基于对称密钥加密（SKE）与基于公钥加密（PKE）两类，但在参与方众多时都不能充分利用计算资源，各阶段存在参与者闲置。论文提出 PULSE，面向大规模参与方的并行私有集合并集协议。


### Velox: Scalable Fair Asynchronous MPC from Lightweight Cryptography.
- **作者**: Akhil Bandarupalli, Xiaoyu Ji, Aniket Kate, Chen-Da Liu-Zhang, Daniel Pöllmann, Yifan Song
- **解读**: 多数 MPC 库或假设有界同步网络，或因大量公钥运算开销难以扩展到几十方以上；论文提出 Velox 异步 MPC 协议，在对手控制 < n/3 方时实现最优公平性，利用对称密钥加密与哈希等轻量密码原语（比公钥运算快 2-3 个数量级），通信高度高效：相对电路规模线性摊销，仅 O(n³) 域元素附加开销，每方每乘法门仅需 9.33 个域元素、较最先进降低 10 倍以上，且原语保留后量子安全性。在地理分布测试床与广播应用上完整实现并评估：16-64 方下 256 条消息洗牌 4 秒完成，延迟较先前最优低 36 倍/28.6 倍，112 方时 50 秒内完成。


### How to Recover a Cryptographic Secret From the Cloud.
- **作者**: David Adei, Chris Orsini, Alessandra Scafuro, Tanner Verber
- **解读**: 云端存储密码学秘密（如加密钱包）等于与云共享所有权、使其更吸引内部攻击，但云又提供强可用性与可靠性；论文希望兼得二者：用户 Alice 可方便地在云中存储秘密副本、只有她能恢复，即使丢失设备、忘记全部凭据也不失去秘密机密性。论文提出基于可信执行环境（TEE）的云上秘密恢复机制，机密性始终保证——即使面对恶意配置的 TEE（Alice 仍持有某些信息时），区别于依赖用户记得认证秘密的先前工作。安全性在通用可组合（UC）框架下证明，并实现了协议评估性能。


### ML-Cube: Accelerating Module-Lattice-Based Cryptography using Machine Learning Accelerators with a Memory-Less Design.
- **作者**: Tian Zhou, Fangyu Zheng, Zhuoyu Xie, Wenxu Tang, Guang Fan, Yijing Ning, Yi Bian, Jingqiang Lin, Jiwu Jing
- **解读**: ML 加速器性能吸引密码学研究者用于加速密码运算，但将其当作黑盒会导致高延迟与严格并发要求、阻碍实际部署；论文深入剖析加速器内部运行机制，提出无内存（memory-less）框架 ML-Cube（ML3）实现模格后量子密码（FIPS 203 ML-KEM 与 204 ML-DSA）：设计定制数学变换加速 (I)NTT 多项式乘法、最小化外部内存依赖，并全面并行化两个方案。Tensor Core 实现较优化 CUDA-core 实现提速 2.03–3.56 倍，相对最先进（CHES'24）提速 10 倍且功耗不足其十分之一，服务器模型下吞吐较 TDSC'24 方法提升 30–55%；核心计算限制在寄存器内，天然缓解缓存侧信道泄露。


### NodeShield: Runtime Enforcement of Security-Enhanced SBOMs for Node.js.
- **作者**: Eric Cornelissen, Musard Balliu
- **arXiv PDF**: [NodeShield: Runtime Enforcement of Security-Enhanced SBOMs for Node.js.](https://arxiv.org/pdf/2508.13750)
- **解读**: 软件供应链是日益常见的攻击向量，Node.js 生态因其规模与流行度遭受大量攻击；研究界与业界提出过进程/语言级沙箱、权限系统、污点跟踪等静态与动态机制。论文在此基础上研究 Node.js 应用的安全增强 SBOM（软件物料清单）的运行时强制机制，目标兼顾兼容性、自动化、最小开销与策略简洁。


### Passwords and FIDO2 Are Meant To Be Secret: A Practical Secure Authentication Channel for Web Browsers.
- **作者**: Anuj Gautam, Tarun Kumar Yadav, Garrett Smith, Kent E. Seamons, Scott Ruoti
- **解读**: 密码管理器自动填充后，恶意客户端脚本与浏览器扩展仍可从网页窃取密码；论文扩展 Stock 与 Johns 的先前工作，展示如何加固密码自动填充以抵御这类本地攻击。在 Firefox 中实现并实验验证该防御能有效抵御 XSS 与扩展攻击，且与 Alexa 前 1000 网站中 97% 兼容；随后推广该设计，提出抵御针对 FIDO2 协议近期攻击的第二套机制，兼容所有网站，仅需 FIDO2 服务器端 2-3 行小改动。


### Be Aware of What You Let Pass: Demystifying URL-based Authentication Bypass Vulnerability in Java Web Applications.
- **作者**: Qiyi Zhang, Fengyu Liu, Zihan Lin, Yuan Zhang
- **解读**: 基于 URL 的认证通过 URL 路径集中灵活地保护 Java Web 应用敏感资源，但不同组件对路由特性（如移除 /./）处理不一致可被利用绕过检查，造成认证绕过漏洞（UABVulns），使攻击者无需认证即可访问。论文对 53 个真实 UABVuln 做首次深入研究，揭示根因并给出路由与清洗方面的三项关键发现，据此实现基于模式分析的静态检测工具 UABScan。在 529 个流行应用中发现 94 个漏洞（涉及 72 个应用，含 35 个已核实的高危 0-day），人工核查召回率 87.50%、精确率 80.00%，显著优于最先进工具，已获 31 个 CVE 编号。


### Same Script, Different Behavior: Characterizing Divergent JavaScript Execution Across Different Device Platforms.
- **作者**: Ahsan Zafar, Junhua Su, Sohom Datta, Alexandros Kapravelos, Anupam Das
- **解读**: 论文刻画同一 JavaScript 脚本在不同设备平台上执行行为的分化现象及其安全影响。「（无摘要，据标题推断）」


### Conflicting Scores, Confusing Signals: An Empirical Study of Vulnerability Scoring Systems.
- **作者**: Viktoria Koscinski, Mark Nelson, Ahmet Okutan, Robert Falso, Mehdi Mirakhorli
- **解读**: 漏洞评估对优先级排序与修复至关重要，但各评分系统目标、方法与输出不同，常导致决策不一致；论文首次对四种公开漏洞评分系统（CVSS、SSVC、EPSS 与 Exploitability Index）做大规模、与结果关联的实证比较，使用来自微软 Patch Tuesday 披露的 600 个真实漏洞数据集，考察评分间关系、各系统分诊层级分类与对利用风险的捕捉能力。研究发现同一漏洞的评分排序存在显著差异，揭示系统间的一致与分歧，并强调组织依赖这些指标做数据驱动、基于风险决策时的含义。


### On Hyperparameters and Backdoor-Resistance in Horizontal Federated Learning.
- **作者**: Simon Lachnit, Ghassan Karame
- **解读**: 水平联邦学习（HFL）易受后门攻击，对手可操纵训练数据与过程实施复杂攻击；论文研究超参数对 HFL 防御有效性的影响，分析并实测表明良性客户端的超参数选择不仅影响模型精度，还显著影响攻击成功率，与许多依赖自定义临时超参数设置的安全研究形成鲜明对比。结果显示恰当调优客户端超参数（如学习率、批大小、本地轮数）可遏制攻击：例如 50% 生命周期的强 A3FL 后门成功率可降低 98.6%，全程无需任何防御，干净精度仅下降 2.9 个百分点。


### Empowering Parents to Support Children's Online Security and Privacy: Findings from a Randomized Controlled Trial.
- **作者**: Xiaowei Chen, Verena Distler, Chloe Gordon, Yaxing Yao, Ziwen Teuber
- **解读**: 泛在计算时代，家长常依赖在线资源指导孩子的安全与隐私（S&P）议题，但信息过载使找到高质量、契合需求的内容颇具挑战，家长能力与应对策略的纵向发展也缺乏研究。论文通过随机对照试验（RCT）检验支持家长提升儿童在线安全与隐私能力的干预效果。


### Security and Privacy Perceptions of Pakistani Facebook Matrimony Group Users.
- **作者**: Mah Jan Dorazahi, Deepthi Mungara, Yasemin Acar, Harshini Sri Ramulu
- **arXiv PDF**: [Security and Privacy Perceptions of Pakistani Facebook Matrimony Group Users.](https://arxiv.org/pdf/2509.08782)
- **解读**: 巴基斯坦婚恋应用受审查，Facebook 婚恋群组成为成员寻找伴侣的替代虚拟空间，但参与者常分享照片、地址、电话号码等敏感信息，暴露于欺诈、勒索与身份盗窃风险。论文通过 23 次半结构化访谈，探究巴基斯坦婚恋群组用户的安全与隐私认知：他们如何感知风险、什么影响其分享信息的信任，以及文化规范与期望如何塑造群组内行为，为更好地保护这类用户提供依据。


### DiveFuzz: Enhancing CPU Fuzzing via Diverse Instruction Construction.
- **作者**: Zihui Guo, Miaomiao Yuan, Yanqi Yang, Liwei Chen, Gang Shi, Dan Meng
- **解读**: CPU 模糊测试中对架构状态的全面探索有赖于生成多样测试用例（合理的操作码分布与指令执行结果多样性），但最先进 fuzzer 存在数据重复率高、操作码失衡的问题。论文提出 DiveFuzz：运行时细粒度控制指令操作数并结合关联上下文语义，生成具有语义关联的指令流，并引入变异器监视进程动态调整、消除误报。在五个常用覆盖度量下较 DifuzzRTL 快 204 倍、较 Cascade 快 114 倍；在 XiangShan、CVA6、Rocket、NutShell 四个开源 RISC-V CPU 上发现 26 个新缺陷（15 个已获 CVE 编号）。


### Threat from Windshield: Vehicle Windows as Involuntary Attack Sources on Automotive Voice Assistants.
- **作者**: Penghao Wang, Shuo Huai, Yetong Cao, Chao Liu, Jun Luo
- **解读**: 车载语音助手（AVA）对利用不可听声音的攻击存在脆弱性，但通常认为攻击者难以从车内发起攻击（信号几乎无法穿透车体、且需声音源与麦克风间存在视线），相关担忧被视为低优先级。论文反驳该共识，提出 ShieldSpear 从车外攻击 AVA：在挡风玻璃外侧放置微型压电元件将其转化为扬声器（兼作麦克风），元件紧凑集成可伪装成贴纸实现隐蔽，具备视线麦克风与行驶中实时攻击能力；并利用玻璃转化的麦克风采集驾驶员声纹以伪造语音命令。在五款主流车辆上的实验表明端到端注入命令成功率 90.9%。


### mUOV: Masking the Unbalanced Oil and Vinegar Digital Signature Scheme at First- and Higher-Order.
- **作者**: Suparna Kundu, Quinten Norga, Angshuman Karmakar, Uttam Kumar Ojha, Anindya Ganguly, Ingrid Verbauwhede
- **解读**: 多变量二次方程（MQE）后量子签名因签名体积小受关注，不平衡油醋签名（UOV）是二十余年前提出的 MQE 方案，其数学安全性已被充分分析，但实际侧信道攻击（SCA）已在多种签名方案上得到展示。论文分析 UOV 方案中可被被动侧信道（差分功耗分析 DPA）利用的变量，并引入掩码对策：为点积、矩阵-向量乘等全部关键运算设计高效掩码 gadget，在 t-探测模型下给出形式证明并经 maskVerif 机械化验证。实现验证了 UOV-Ip 与 UOV-III 的一阶与任意高阶掩码可行性，签名生成性能较 ML-DSA-44 好最多 62%、较 Falcon-512 好 99%，并采用 TVLA 方法实测验证。


### Security-Aware Sensor Fusion with MATE: the Multi-Agent Trust Estimator.
- **作者**: R. Spencer Hallyburton, Miroslav Pajic
- **arXiv PDF**: [Security-Aware Sensor Fusion with MATE: the Multi-Agent Trust Estimator.](https://arxiv.org/pdf/2503.04954)
- **解读**: 论文提出 MATE（多智能体信任估计器），实现安全感知的传感器融合。「（无摘要，据标题推断）」


### Adversarially Robust Assembly Language Model for Packed Executables Detection.
- **作者**: Shijia Li, Jiang Ming, Lanqing Liu, Longwei Yang, Ni Zhang, Chunfu Jia
- **arXiv PDF**: [Adversarially Robust Assembly Language Model for Packed Executables Detection.](https://arxiv.org/pdf/2509.15499)
- **解读**: 加壳可执行文件检测是大规模恶意软件分析与杀毒引擎流程的关键环节，用于筛选需要动态脱壳的样本；传统方法依赖高熵、特定二进制模式等经验特征，但这类特征方法日益易被对抗性未知加壳器（如低熵加壳器）规避，且依赖专家特征难以随演化持续。论文提出基于汇编语言模型（assembly language model）的加壳检测方法，面向对抗鲁棒性。


### BLACKOUT: Data-Oblivious Computation with Blinded Capabilities.
- **作者**: Hossam ElAtali, Merve Gülmez, Thomas Nyman, N. Asokan
- **arXiv PDF**: [BLACKOUT: Data-Oblivious Computation with Blinded Capabilities.](https://arxiv.org/pdf/2504.14654)
- **解读**: 内存安全缺失与侧信道暴露是安全软件实现的两大持久挑战，内存安全语言能显著减少内存缺陷但使抗侧信道代码更难编写；论文通过为内存安全硬件补充数据无关（data-oblivious）编程能力，同时应对两者：扩展 CHERI 能力架构引入盲化能力（blinded capabilities），支持用户态任务中的数据无关计算，并给出在基于推测乱序 CHERI-Toooba 的 FPGA 软核上的实现 BLACKOUT，扩展了 CHERI 版 Clang/LLVM 编译器与 CheriBSD 操作系统。BLACKOUT 使非数据无关操作显式触发故障、简化编码；评估表明其保障内存经安全分配、使用与回收，且相对基线处理器仅带来 1.5%（几何均值）的性能损失。


### Needle in a Haystack: Automated and Scalable Vulnerability Hunting in the Windows ALPC Sea.
- **作者**: Haoyi Liu, Feng Dong, Yunpeng Tian, Mu Zhang, Xuefeng Li, Fangming Gu, Zhiniang Peng, Haoyu Wang
- **解读**: Windows 服务经底层高级本地过程调用（ALPC）传输上的 RPC 与 COM 通信，构成显著攻击面；但先前研究多聚焦已知漏洞模式或需耗时逆向工程，阻碍规模化发现。论文开发自动化、可扩展的 ALPC 通信模糊测试工具：采用录制-重放策略捕获全系统实时流量并在 ALPC 层直接重放变异载荷，绕开传统方法手工准备的瓶颈，并集成专门检测技术识别崩溃中心 fuzzer 遗漏的信息泄露漏洞。在多个操作系统版本上评估后确认 12 个微软确认的漏洞（10 个已分配 CVE 编号）。


### Styled to Steal: The Overlooked Attack Surface in Email Clients.
- **作者**: Leon Trampert, Daniel Weber, Christian Rossow, Michael Schwarz
- **解读**: 电子邮件仍是广泛使用的通信媒介，OpenPGP 与 S/MIME 等标准在兼容现有设施的同时提供加密；在端到端威胁模型中邮件服务器不可信，攻击者可在传输中或通过重发泄露邮件向加密邮件注入恶意 HTML 或 CSS。论文研究这一被忽视的邮件客户端攻击面。


### Denial of Sequencing Attacks in Ethereum Layer 2 Rollups.
- **作者**: Zihao Li, Zhiyuan Sun, Zheyuan He, Jinzhao Chu, Hao Zhou, Xiapu Luo, Ting Chen, Yinqian Zhang
- **解读**: Layer 2 rollup 是解决以太坊可扩展性的有前景方案，但其中定序器（sequencer）的中心化特性使其易受拒绝服务攻击：敌手以大量无法入块的无效交易压制定序器，耗尽交易处理计算资源；为缓解该威胁，Layer 2 实现合法性检查机制在交易到达定序器前过滤。论文研究此类拒绝排序（denial of sequencing）攻击及其缓解。


### Realizing Corrupted-Shard Tolerance: A Sharding Blockchain with Preserving Global Resilience.
- **作者**: Yizhong Liu, Andi Liu, Zhuocheng Pan, Yuxuan Hu, Jianwei Liu, Song Bian, Yuan Lu, Zhenyu Guan, Dawei Li, Meikang Qiu
- **解读**: 区块链分片通过将网络划分为并行小分片提升可扩展性，但依赖拜占庭容错协议的现有方案需要大分片满足严格安全阈值、限制扩展性，放松参数又会导致活性与安全性违例。论文提出 Camael，通过有效的检测与处理机制实现受损分片容忍：两阶段报告确认机制准确检测恶意节点伪造的虚假违例，轻量快照机制高效识别隐藏违例，状态确定过程保证全局一致性，恶意节点可被精准定罪并触发定向替换重配。Camael 保持全局 1/3 容错的同时可容忍受损分片（每分片最多 2/3 恶意节点）；2000 个 AWS EC2（4 个区域）实验显示吞吐较 Kronos（NDSS'25）提升 3.56 倍（109.3 ktx/s），违例处理仅需 1.64 秒。


### Lite-PoT: Practical Powers-of-Tau Setup Ceremony.
- **作者**: Lucien K. L. Ng, Pedro Moreno-Sanchez, Mohsen Minaei, Panagiotis Chatzigiannis, Adithya Bhat, Duc Viet Le
- **arXiv PDF**: [Lite-PoT: Practical Powers-of-Tau Setup Ceremony.](https://arxiv.org/pdf/2503.04549)
- **解读**: zk-SNARK 因高效性在隐私保护应用、去中心化系统（如区块链）与可验证计算中得到广泛应用，但最高效的 zk-SNARK 常依赖一次性可信设置生成公共参数（Powers-of-Tau 字符串）；τ 秘密参数一旦泄露，攻击者可伪造证明，破坏所有基于它的 zk-SNARK 的可靠性。论文提出 Lite-PoT，使 Powers-of-Tau 设置仪式更实用。

### A Secure Sequencer and Data Availability Committee for Rollups.
- **作者**: Margarita Capretto, Martín Ceresa, Antonio Fernández Anta, Pedro Moreno-Sanchez, César Sánchez
- **arXiv PDF**: [A Secure Sequencer and Data Availability Committee for Rollups.](https://arxiv.org/pdf/2509.06614)
- **解读**: Rollup 依赖排序器（sequencer）与数据可用性委员会（DAC）分别保障交易排序与数据可获取性，二者若作恶或故障将威胁二层网络的正确性；本文研究如何为这两个组件设计安全协议，以抵御恶意行为并保证系统可靠性。（无摘要，据标题推断）


### Safeguarding Graph Neural Networks against Topology Inference Attacks.
- **作者**: Jie Fu, Yuan Hong, Zhili Chen, Wendy Hui Wang
- **arXiv PDF**: [Safeguarding Graph Neural Networks against Topology Inference Attacks.](https://arxiv.org/pdf/2509.05429)
- **解读**: 针对 GNN 隐私研究此前聚焦边级隐私，而图整体结构的机密性（拓扑）是未被充分探索的威胁。本文系统研究 GNN 面临的图级推断攻击风险，提出一类拓扑推断攻击（TIAs），仅凭对 GNN 模型的黑盒访问即可重构目标训练图的结构。实验表明 GNN 对此类攻击高度脆弱，且现有差分隐私机制不足——要么无法缓解风险，要么严重损害效用。


### MoEcho: Exploiting Side-Channel Attacks to Compromise User Privacy in Mixture-of-Experts LLMs.
- **作者**: Ruyi Ding, Tianhong Xu, Xinyi Shen, Aidong Adam Ding, Yunsi Fei
- **arXiv PDF**: [MoEcho: Exploiting Side-Channel Attacks to Compromise User Privacy in Mixture-of-Experts LLMs.](https://arxiv.org/pdf/2508.15036)
- **解读**: MoE（混合专家）架构按输入语义将 token 动态路由到不同专家，这种输入相关的激活模式会在硬件执行中留下独特的时空痕迹。MoEcho 利用这些侧信道痕迹推断用户输入语义，从而在推理过程中泄露用户隐私，揭示了自适应路由机制引入的新攻击面。


### Removal Attack and Defense on AI-generated Content Latent-based Watermarking.
- **作者**: De Zhang Lee, Han Fang, Hanyi Wang, Ee-Chien Chang
- **arXiv PDF**: [Removal Attack and Defense on AI-generated Content Latent-based Watermarking.](https://arxiv.org/pdf/2509.11745)
- **解读**: 潜在空间水印通过在生成过程初始化时从秘密分布采样起点将水印嵌入 AI 生成内容，结合伪随机纠错码后与未水印对象不可区分且对白噪声鲁棒。本文表明不可区分性并不保证对对抗性移除攻击的抵抗力，提出一种利用对象位置泄露的边界信息的新型移除攻击，可将移除水印所需失真降低至多 15 倍（相对白噪声基线），并研究相应的防御措施。


### VillainNet: Targeted Poisoning Attacks Against SuperNets Along the Accuracy-Latency Pareto Frontier.
- **作者**: David Oygenblik, Abhinav Vemulapalli, Animesh Agrawal, Debopam Sanyal, Alexey Tumanov, Brendan Saltaformaggio
- **解读**: 权重共享 SuperNet 在运行时动态激活子网络以实现自适应推理，但攻击者可利用其独特训练范式，只在特定子网络中植入后门，使其在其余数十亿子网络中保持休眠。VillainNet（VNET）提出一种受限后门激活的投毒方法，将后门限制在攻击者选择的部署场景（如车速、天气条件）或子网络配置中；其核心是基于架构与计算相似度度量的距离感知优化，确保后门不会在非目标子网络中激活。


### Efficient Fuzzy PSI Based on Prefix Representation.
- **作者**: Chengrui Dang, Xv Zhou, Bei Liang
- **解读**: 模糊 PSI 是 PSI 的变体：接收方与发送方分别输入点集，接收方可以学习发送方集合中在特定度量下与其点距离不超过阈值 δ 的元素。本文基于前缀表示来构造高效的模糊 PSI 协议，以降低通信与计算开销。


### Armadillo: Robust Single-Server Secure Aggregation for Federated Learning with Input Validation.
- **作者**: Yiping Ma, Yue Guo, Harish Karthikeyan, Antigoni Polychroniadou
- **解读**: Armadillo 是一个面向联邦学习（单一强服务器与众多弱客户端迭代训练）的安全聚合系统，具备对抗客户端干扰的鲁棒性：任何恶意客户端联盟只能通过在预定义合法范围内虚报其私有输入来影响聚合结果。相比既有方案（每客户端开销过高或需要对数轮交互），本文借助零知识证明技术并设计出两种更高效的新协议，其中一种简单的两层协议仅需算术级（每客户端）开销。


### The OCH Authenticated Encryption Scheme.
- **作者**: Sanketh Menda, Mihir Bellare, Viet Tung Hoang, Julia Len, Thomas Ristenpart
- **解读**: OCH 是首个同时提供 128 位多用户 AE 安全性、上下文承诺（context commitment）、256 位随机数及可选随机数隐私的带关联数据认证加密（AEAD）方案，弥补了现有广泛部署方案的局限。作者以模块化方式构造并形式化分析其安全性，所提炼的变换具有更广的适用性。在 Intel Raptor Lake CPU 上基于 Areion 置换族实现峰值速度 0.62 周期/字节，接近 AES128-GCM（0.38），优于 ChaCha20/Poly1305（1.63）与 TurboSHAKE128-Wrap（3.52）。


### From OT to OLE with Subquadratic Communication.
- **作者**: Jack Doerner, Iftach Haitner, Yuval Ishai, Nikolaos Makriyannis
- **解读**: 不经意线性求值（OLE）是不经意传输（OT）的代数推广：模 q 的 OLE 协议使接收方安全地计算发送方选择的直线 a·X+b 在秘密点 x∈ℤq 上的取值。鉴于 OLE 与 OT 快速扩展技术之间存在巨大效率差距，本文重新审视从 OT 归约构造 OLE 的问题，目标是改进已知归约的通信开销。


### Interoperable Symmetric Message Franking.
- **作者**: Carolina Ortega Pérez, Thomas Ristenpart, Julia Len
- **解读**: 欧盟 2022 年《数字市场法》（DMA）要求大型消息应用支持可互操作的端到端加密（E2EE）通信，这引发如何在互操作场景中保持安全与隐私的密码协议适配问题，且不仅限于主协议，也涉及滥用缓解机制。本文研究对称消息 franking 协议（最初由 Facebook 提出，用于举报加密消息中的滥用内容并让平台可密码学验证举报的真实性）在互操作设置下的构造与安全。


### Gibbon: Faster Secure Two-party Training of Gradient Boosting Decision Tree.
- **作者**: Lichun Li, Zecheng Wu, Yuan Zhao, Zhihao Li, Wen-jie Lu, Shan Yin
- **解读**: 面向垂直切分数据集（两方各持同一批样本的不同特征）的安全两方 GBDT 训练场景，本文提出 Gibbon 框架，基于安全多方计算（MPC）让两方在输入保密的前提下联合训练梯度提升决策树。与 SOTA 方案 Squirrel（USENIX Security '23）相比，在大多数参数设置下 Gibbon 将运行时间降低 2–4 倍、通信量降低 2–3 倍。


### Breaking and Fixing Content-Defined Chunking.
- **作者**: Kien Tuong Truong, Simon-Philipp Merz, Matteo Scarlata, Felix Günther, Kenneth G. Paterson
- **解读**: 内容定义分块（CDC）以内容决定块边界、在数据部分变化时保持边界稳定的方式切分流数据，广泛用于备份、软件补丁和文件托管等去重场景。与压缩类似，CDC 与加密结合时可能引入泄露：指纹攻击可利用长度模式推断数据信息。本文对 CDC 进行安全分析——展示其脆弱性并提出修复方案。


### Refined TFHE Leveled Homomorphic Evaluation and Its Application.
- **作者**: Ruida Wang, Jincheol Ha, Xuan Shen, Xianhui Lu, Chunling Chen, Kunpeng Wang, Jooyoung Lee
- **解读**: TFHE 是基于环面（torus）的、支持快速自举的全同态加密方案，其可编程自举（PBS）同时完成函数计算与噪声刷新，但操作数随电路深度指数增长。Chillotti 等人（Asiacrypt 2017）提出的 leveled（LHE）模式将计算与自举解耦，可提速数百倍，但剩余的盲旋转（CBS）虽频率线性增长，仍成为性能瓶颈。本文对 TFHE 的 leveled 同态求值进行精炼优化。


### CCA-Secure Traceable Threshold (ID-based) Encryption and Application.
- **作者**: Rishiraj Bhattacharyya, Jan Bormet, Sebastian Faust, Pratyay Mukherjee, Hussien Othman
- **解读**: Boneh、Partap 与 Rotem（Crypto'24）提出可追踪门限加密：若 t 个以上参与方合谋构造可执行解密的解密盒，可通过少量黑盒查询追踪出至少一名参与方身份，可用于区块链 mempool 隐私等场景以威慑 MEV 合谋。但其定义仅达到 CPA 安全，且未考虑不同参与集合导致的不一致问题。本文构建 CCA 安全的可追踪门限（基于 ID）加密方案并给出应用。


### High-Throughput Universally Composable Threshold FHE Decryption.
- **作者**: Guy Zyskind, Doron Zarchy, Max Leibovich, Chris Peikert
- **解读**: 门限全同态加密（FHE）支持对加密数据做任意计算，同时把解密能力分布到多个参与方，其主要应用之一是低通信量的多方计算（MPC）。本文研究高吞吐、通用可组合（UC）安全的门限 FHE 解密协议，为低通信 MPC 提供快速且安全的解密基础。


### Harnessing Sparsification in Federated Learning: A Secure, Efficient, and Differentially Private Realization.
- **作者**: Shuangqing Xu, Yifeng Zheng, Zhongyun Hua
- **解读**: 联邦学习（FL）因传输高维梯度而存在通信瓶颈，同时共享梯度也可能被用于推断私有本地数据。本文提出 Clover 系统框架，采用 top-k 稀疏化（每客户端仅保留幅度最大的 k 个梯度分量参与聚合）缓解通信瓶颈，并在此基础上构建安全且满足差分隐私的机制（基于新兴的分布式信任设置），兼顾通信效率与隐私保护。


### Counting Subgraphs under Shuffle Differential Privacy.
- **作者**: Juanru Fang, Ke Yi
- **解读**: 分布式设置（每节点仅持局部邻接信息、分析师不可信）下的子图计数是差分隐私（DP）研究热点，shuffle DP 模型依赖可信洗牌器在交给分析师前随机打乱消息。由于洗牌不引入额外风险，任何协议在 shuffle 模型下自动满足（本地）DP，关键问题在于 shuffle DP 能否带来效用提升。在若干基本问题（如计数、频率估计）已有正面结果的基础上，本文研究 shuffle DP 下的子图计数问题。


### Managing Correlations in Data and Privacy Demand.
- **作者**: Syomantak Chaudhuri, Thomas A. Courtade
- **解读**: 允许用户自选隐私级别的差分隐私工作通常基于异构 DP（HDP）框架，且假设用户数据与隐私需求不相关。本文首先指出当隐私需求相关时标准 HDP 失效，进而提出增删异构差分隐私（AHDP）框架，联合考虑数据与隐私偏好，证明其对数据-隐私相关性的鲁棒性；并从操作假设检验视角形式化其保证。此外表明存在不依赖数据-隐私相关性先验知识的非平凡机制。


### BFId: Identity Inference Attacks Utilizing Beamforming Feedback Information.
- **作者**: Julian Todt, Felix Morsbach, Thorsten Strufe
- **解读**: WiFi 5 引入的波束成形要求客户端广播其对信道特性的观测（BFI），这构成新的感知信息源并带来此前未被探索的隐私威胁。BFId 是首个基于 BFI 的身份推断攻击，在包含 197 名个体记录的新数据集上评估，结果表明即便跨不同行走风格与视角、甚至在大样本规模下，也能以很高的准确率推断出个体身份。


### Systematic Assessment of Tabular Data Synthesis.
- **作者**: Yuntao Du, Ninghui Li
- **解读**: 数据合成被视作兼顾数据利用与隐私保护的重要途径，近年涌现大量表格数据合成器，其中部分满足差分隐私，部分只提供启发式隐私。由于评估指标的缺陷以及缺少与利用扩散模型、大语言模型的新兴合成器同 SOTA 统计合成器的正面比较，对这些合成器优劣的全面理解仍然缺失。本文对表格数据合成进行系统性评估。


### Peekaboo, I See Your Queries: Passive Attacks Against DSSE Via Intermittent Observations.
- **作者**: Hao Nie, Wei Wang, Peng Xu, Wei Chen, Laurence T. Yang, Mauro Conti, Kaitai Liang
- **解读**: 动态可搜索对称加密（DSSE）存在固有信息泄露，现有被动攻击依赖持续监控泄露以推断查询模式，而本文针对更实际的间歇观测威胁模型。Peekaboo 是一个通用攻击框架，核心是推断搜索模式并结合辅助知识，将其实例化到 SOTA 攻击 Sap（USENIX '21）与 Jigsaw（'24）上得到变体 Sap+ 与 Jigsaw+。实验表明其恢复调整兰德指数超过 0.9、查询恢复准确率约 90%（对比 FMA 的约 30%），且攻击能力随观测轮数扩展。


### 'Is this a scam?': The Nature and Quality of Reddit Discussion about Scams.
- **作者**: Elijah Robert Bouma-Sims, Mandy Lanyon, Lorrie Faith Cranor
- **解读**: 针对社交媒体上的骗局求助讨论（如电商欺诈、钓鱼）研究甚少，本文对 Reddit 四个聚焦骗局讨论社区（主要为 /r/Scams）的 1,525 个帖子进行了多阶段主题分析。发现发帖人主要识别骗局、讨论骗子所用策略并获得受害应对支持，讨论内容多为互联网相关技术中介；用户主要提供信息支持与对受害者的安抚，但部分评论强化了受害者有罪论。不同社区提供支持的类型存在差异，如 /r/Sextortion 被用于情感支持。


### Leaky Apps: Large-scale Analysis of Secrets Distributed in Android and iOS Apps.
- **作者**: David Schmidt, Sebastian Schrittwieser, Edgar R. Weippl
- **解读**: 移动应用为支撑功能会存储各类秘密，包括 API 密钥与用于认证用户、访问后端服务的加密材料；应用分发后攻击者可对其逆向，使这些秘密暴露，带来数据泄露与服务滥用风险。本文对 Android 与 iOS 应用中分发的秘密进行大规模分析。


### 'We just did not have that on the embedded system': Insights and Challenges for Securing Microcontroller Systems from the Embedded CTF Competitions.
- **作者**: Zheyuan Ma, Gaoxiang Liu, Alex Eastman, Kai Kaufman, Md. Armanuzzaman, Xi Tan, Katherine Jesse, Robert J. Walls, Ziming Zhao
- **解读**: 微控制器系统支撑车辆、医疗设备、工控等关键应用，保障其安全开发的挑战亟需梳理。以往研究仅聚焦固件分析来识别漏洞，而本文独到地利用 2023–2024 年 MITRE eCTF 竞赛的队伍提交与赛后访谈数据，从技术实现与人员认知两个视角剖析系统开发全生命周期，深入理解漏洞最初是如何产生的。


### Walking The Last Mile: Studying Decompiler Output Correction in Practice.
- **作者**: Joshua Wiedemeier, Simon Klancher, Joel Flores, Max Zheng, Jaehyun Park, Sang Kil Cha, Kangkook Jee
- **解读**: 本文是首个大规模的人类辅助反编译研究，利用 pylingual.io 的丰富数据（181,646 个二进制、9,003 个用户提交补丁、393 个经准确性验证的补丁）考察逆向工程师如何应对不准确的 PYC 反编译输出，以及哪些因素影响其达成准确反编译的努力。研究还以受控用户实验补充观测结果，以隔离修补不完美反编译结果的技术难度。


### A Qualitative Analysis of Fuzzer Usability and Challenges.
- **作者**: Yunze Zhao, Wentao Guo, Harrison Goldstein, Daniel Votipka, Kelsey R. Fulton, Michelle L. Mazurek
- **解读**: 模糊测试被广泛用于通过随机或变异测试输入发现软件漏洞，但开发者实际如何使用模糊工具、面临哪些挑战、现有工具何处不足，鲜有研究。本文通过对多个领域 18 名用户的半结构化访谈，考察其工作流、挫败点与期望，揭示关键的可用性缺口与设计机会，可为下一代模糊工具改善用户体验、减少人工投入并更好融入真实工作流提供依据。


### It Should Be Easy but... New Users' Experiences and Challenges with Secret Management Tools.
- **作者**: Lorenzo Neil, Deepthi Mungara, Laurie A. Williams, Yasemin Acar, Bradley Reaves
- **解读**: 开发者面临 API 密钥、密码等软件秘密泄露的风险，业界与安全指南强烈推荐使用秘密管理工具（SMT，如 HashiCorp Vault、Infisical）在集中位置安全管理秘密，但秘密泄露仍普遍发生，且开发者反映学习与使用 SMT 困难。本文研究新用户使用 SMT 的体验与挑战，考察公开资源（文档、界面）能否有效支持学习，指出缺乏可用的入门引导使快速采用与有效使用不切实际。


### CHaRM: Checkpointed and Hashed Counters for Flexible and Efficient Rowhammer Mitigation.
- **作者**: Ali Hajiabadi, Michele Marazzi, Kaveh Razavi
- **解读**: Rowhammer 仍是强攻击向量，但 CPU 厂商因确定性缓解需按最脆弱设备支付高昂代价而不愿部署。CHaRM 提出一种 CPU 内缓解方案，通过将大量（行）映射到有限数量的固定哈希计数器来打破（计数表规模随攻击阈值恶化的）依赖；由于多行映射到有限计数器会产生碰撞，CHaRM 部署检查点机制保存被逐出的表状态，当行再次被激活时恢复其检查点值并继续跟踪，避免碰撞引发过度的缓解性刷新。


### ZVDetector: State-Guided Vulnerability Detection System for Zigbee Devices.
- **作者**: Hai Lin, Chenglong Li, Jiahai Yang, Zhiliang Wang, Jiaqi Bai
- **解读**: Zigbee 设备广泛用于智能家居、农业等领域但存在大量可危害其功能的漏洞；现有研究或分析固件、或通过网络模糊测试发现漏洞，却忽略设备状态与协议的影响、只探索有限空间，因而无法发现由两个状态内部隐藏状态、尤其是状态组合触发的漏洞。ZVDetector 设计状态引导的模糊测试系统，以状态感知模块基于消息关系发现更丰富的未知状态转换、更完整地理解设备内在属性，并开发相应算法来挖掘组合状态漏洞。


### ExfilState: Automated Discovery of Timer-Free Cache Side Channels on ARM CPUs.
- **作者**: Fabian Thomas, Michael Torres, Daniel Moghimi, Michael Schwarz
- **解读**: 缓存侧信道通常依赖高精度定时器，而现代 ARM CPU 上存在无需定时器的利用途径；本文研究自动化发现 ARM CPU 上无定时器缓存侧信道的方法，以系统化暴露此类微架构泄露。（无摘要，据标题推断）


### MileSan: Detecting Exploitable Microarchitectural Leakage via Differential Hardware-Software Taint Tracking.
- **作者**: Tobias Kovats, Flavien Solt, Katharina Ceesay-Seitz, Kaveh Razavi
- **解读**: 微架构泄露（如瞬态执行侧信道）是否可利用取决于软硬件两端的细节，本文提出差分硬件-软件污点跟踪方法，将硬件侧与软件侧的污点信息联合比对，以自动检测可利用的微架构信息泄露。（无摘要，据标题推断）


### BOLT: Bandwidth-Optimized Lightning-Fast Oblivious Map powered by Secure HBM Accelerators.
- **作者**: Yitong Guo, Hongbo Chen, Haobin Hiroki Chen, Yukui Luo, XiaoFeng Wang, Chenghong Wang
- **解读**: 混淆映射（OMAP）能完全隐藏访问模式，但随机重映射与最坏情况填充带来高开销；本文论证这些成本并非本质的。现代带高带宽内存（HBM）的加速器提供新机会：HBM 通道与处理器核密封在同一封装内，窃听困难，适当隔离后可将 HBM 变成数据与访问痕迹都被隐藏的不可观察区域。BOLT 据此重新设计基于 HBM 的 OMAP，实现带宽优化、闪电般快速的不经意映射。


### FlexEmu: Towards Flexible MCU Peripheral Emulation.
- **作者**: Chongqing Lei, Zhen Ling, Xiangyu Xu, Shaofeng Li, Guangchi Liu, Kai Dong, Junzhou Luo
- **解读**: MCU（微控制器）外设仿真对嵌入式固件分析至关重要，但现有仿真器在外设覆盖与灵活性上受限；本文提出 FlexEmu，追求灵活的 MCU 外设仿真，以支持更广泛的固件动态分析场景。（无摘要，据标题推断）


### Rethinking Tamper-Evident Logging: A High-Performance, Co-Designed Auditing System.
- **作者**: Rui Zhao, Muhammad Shoaib, Viet Tung Hoang, Wajih Ul Hassan
- **解读**: 现有防篡改日志系统在高负载下开销高、数据丢失严重，且仅提供粗粒度的篡改检测，安装还需重编译内核。本文提出 Nitro 高性能审计系统，支持细粒度日志篡改检测，并利用 eBPF 技术避免内核重编译；作者在新的定义框架下形式化其安全性并给出满足该目标的实用密码学构造，且与以往聚焦处理环节的工作不同，将前后处理与日志处理协同设计以利用系统级优化。评估显示 Nitro 相对基线有 10–25 倍性能提升。


### Empirical Security Analysis of Software-based Fault Isolation through Controlled Fault Injection.
- **作者**: Nils Bars, Lukas Bernhard, Moritz Schloegel, Thorsten Holz
- **解读**: 浏览器是用户与攻击者之间的关键安全边界，而 JS 引擎的复杂性使 JavaScript 成为主要攻击面；为缓解威胁，引擎日益采用软件故障隔离（SFI），其中 Google 的 V8 堆沙箱是最广泛部署的 SFI 机制，保护着 Chromium 系浏览器及基于 Node.js/Electron 的无数应用。该沙箱将地址空间分为含受信任、安全敏感元数据的部分与沙箱化的内存访问部分。本文通过受控故障注入对这类 SFI 机制进行实证安全分析。


### GPU Travelling: Efficient Confidential Collaborative Training with TEE-Enabled GPUs.
- **作者**: Shixuan Zhao, Zhongshu Gu, Salman Ahmed, Enriquillo Valdez, Hani Jamjoom, Zhiqiang Lin
- **解读**: 机密协作机器学习让多个互不信任的数据持有者在不泄露私有数据集的前提下联合训练模型，但现有方案在训练中需经较慢的常规链路频繁交换数据，在 LLM 等模型规模指数增长下面临高昂通信成本。本文提出 GPU Travelling 机制，利用新兴的机密 GPU：让（模型）安全地「旅行」到特定持有者处，将数据集直接载入 GPU 受保护内存后返回继续训练，从而消除（中间数据）传输开销。


### Sleeping Giants - Activating Dormant Java Deserialization Gadget Chains through Stealthy Code Changes.
- **作者**: Bruno Kreyssig, Sabine Houy, Timothée Riom, Alexandre Bartel
- **解读**: Java 反序列化 gadget 链是研究充分的严重软件弱点，绝大多数已知链依赖依赖库中的 gadget，且依赖中的小代码改动即可使链可行，使链检测沦为被动追赶。本文评估依赖演化过程中链被激活的可能性（改动可能出于意外或作为供应链攻击的一部分被故意引入），指出类的可序列化性是随演化强烈波动的属性，研究三种攻击者可隐蔽地将链引入依赖的改动模式，并在 533 个样本上对原始与修改版本运行 SOTA 检测器进行验证。


### Parcel Mismatch Demystified: Addressing a Decade-Old Security Challenge in Android.
- **作者**: Sheng Cao, Hao Zhou, Songzhou Shi, Yanjie Zhao, Haoyu Wang
- **解读**: Android IPC 机制中的 Parcel Mismatch 漏洞是持续十余年的安全挑战，导致大量提权利用；Google 已实施多种缓解（最终为 Android 13 的 Lazy Bundle），但缺乏对这些漏洞与缓解的系统分析。本文完成首次系统性研究并提出 ParcelTaint 静态检测方法，精确建模跨进程的 Intent 变换以发现攻击向量，揭示 10 个此前未知的高危漏洞（其中 5 个获分配 CVE），包括绕过既有缓解的系统服务链利用方式，且全部得到确认。


### Deprivileging Low-Level GPU Drivers Efficiently with User-Space Processes and CHERI Compartments.
- **作者**: Paul Metzger, A. Theodore Markettos, Edward Tomasz Napierala, Matthew Naylor, Robert N. M. Watson, Timothy M. Jones
- **解读**: 设备驱动是操作系统漏洞的主要来源，OS 日益通过将其移出内核到用户空间实现去特权化，但这通常带来显著开销；GPU 驱动尤其庞大、复杂且性能敏感，主流 OS 中大部分 GPU 驱动以全特权运行。本文研究使用用户空间进程与 CHERI 隔离舱（compartments）高效地对底层 GPU 驱动去特权化的方法。


### Towards a Formal Foundation for Blockchain ZK Rollups.
- **作者**: Stefanos Chaliasos, Denis Firsov, Benjamin Livshits
- **解读**: 区块链二层方案 ZK-Rollup 在链下处理交易并在主链验证以缓解可扩展性问题，但安全性、抗审查性仍存疑虑：中心化控制与智能合约层机制不足以强制保障这些性质，现有 L2 易受多重签名攻击，可能导致用户资金全部损失。本文用 Alloy 规范语言对关键功能（强制交易队列、安全黑名单、可升级性）进行形式化分析，为区块链 ZK Rollup 建立形式化基础。


### Formal Security and Functional Verification of Cryptographic Protocol Implementations in Rust.
- **作者**: Karthikeyan Bhargavan, Lasse Letager Hansen, Franziskus Kiefer, Jonas Schneider-Bensch, Bas Spitters
- **解读**: 本文提出一套在统一证明框架内形式化验证 Rust 编写的实用密码协议实现的方法论，可开发运行时安全、解析正确性与安全性等多种性质的机器可检查证明；所有分析由开发者在 Rust 源码中写注解并为其任务选择后端证明器（从通用证明助手 F* 到面向密码的 ProVerif、SSProve）驱动。主要贡献是在 Bert13——一个可移植的后量子 TLS 1.3 实现——上同时验证了安全性与功能正确性，据作者所述这是 Rust 库中首次取得的结果。


### Secure Parsing and Serializing with Separation Logic Applied to CBOR, CDDL, and COSE.
- **作者**: Tahina Ramananandro, Gabriel Ebner, Guido Martínez, Nikhil Swamy
- **解读**: 低层语言中对安全关键数据格式的错误处理是许多漏洞的根源，可证明正确的解析/序列化工具能帮助缓解。PulseParse 是一个经形式化验证的解析器与序列化器组合库，面向不可变形（non-malleable）二进制格式，其规约与证明基于分离逻辑，提供更抽象、可组合的接口并完整支持验证、解析与序列化；它还支持递归格式，且针对对抗性输入可用常数量的栈空间完成解析。


### Looping for Good: Cyclic Proofs for Security Protocols.
- **作者**: Felix Linker, Christoph Sprenger, Cas Cremers, David A. Basin
- **解读**: 安全协议常包含循环（如棘轮更新或对归纳定义数据结构的操作），但协议的自动化安全分析难以跟上这些特性，SOTA 方法需要人工构造抽象结构并严重依赖辅助的用户自定义引理。本文提出用循环证明（cyclic proofs）技术直接处理协议中的循环，以减少人工引理负担并推进自动化分析。


### Privacy-Preserving Runtime Verification.
- **作者**: Thomas A. Henzinger, Mahyar Karimi, K. S. Thejaswini
- **解读**: 运行时验证为系统安全可靠性提供可扩展的保障，但当系统需要第三方监控以验证是否符合规约时，常规方法可能泄露系统的敏感信息（关于系统及其处理数据的受保护信息），甚至暴露「正在被监控」这一事实本身，从而破坏第三方验证的意义。本文研究隐私保护运行时验证，在验证合规性的同时不泄露受保护信息、不暴露监控行为。


### Generalized Security-Preserving Refinement for Concurrent Systems.
- **作者**: Huan Sun, David Sanán, Jingyi Wang, Yongwang Zhao, Jun Sun, Wenhai Wang
- **解读**: 确保并发系统（如多核 OS 内核等大型代码库）符合信息流安全（IFS）极具挑战，精化（refinement）验证——证明实现保持更抽象规约的某些性质——是应对该挑战的有前景途径，但现有精化式安全验证要么局限于顺序系统，要么缺乏表达复杂安全策略的能力。本文提出面向并发系统的广义安全保持精化方法。


### Prototype Surgery: Tailoring Neural Prototypes via Soft Labels for Efficient Machine Unlearning.
- **作者**: Gaoyang Liu, Xijie Wang, Zixiong Wang, Chen Wang, Ahmed M. Abdelmoniem, Desheng Wang
- **解读**: 深度神经网络依赖敏感私有数据引发「被遗忘权」担忧，机器遗忘被提出以高效消除特定训练数据的影响，但现有方法在参数量庞大的 DNN 上执行慢、内存消耗高，难以用于大规模模型。本文将注意力转向最终分类层的小权重集合（定义为各类别的「原型」），关键观察是：与被遗忘数据关联的原型发生显著偏移，而无关原型基本不变；据此通过软标签对原型进行「手术」式调整，实现高效的机器遗忘。


### SafeGuider: Robust and Practical Content Safety Control for Text-to-Image Models.
- **作者**: Peigui Qi, Kunsheng Tang, Wenbo Zhou, Weiming Zhang, Nenghai Yu, Tianwei Zhang, Qing Guo, Jie Zhang
- **解读**: 文生图模型易受对抗提示攻击从而绕过安全措施生成有害内容，现有防御难以兼顾鲁棒性与实际可用性。作者先对广泛使用的 Stable Diffusion 文本编码器进行实证研究，发现 [EOS] token 充当语义聚合器，良性提示与恶意提示在嵌入空间呈现不同的分布模式；基于该洞察构建 SafeGuider，实现鲁棒且实用的文生图内容安全控制。


### SecAlign: Defending Against Prompt Injection with Preference Optimization.
- **作者**: Sizhe Chen, Arman Zharmagambetov, Saeed Mahloujifar, Kamalika Chaudhuri, David A. Wagner, Chuan Guo
- **解读**: 提示注入可通过在系统指令或外部内容中混入恶意指令劫持 LLM 行为，本文用偏好优化（preference optimization）方法对模型进行对齐训练，使其更倾向于遵循系统指令而非注入内容，从而防御提示注入攻击。（无摘要，据标题推断）


### On the Feasibility of Poisoning Text-to-Image AI Models via Adversarial Mislabeling.
- **作者**: Stanley Wu, Ronik Bhaskar, Anna Yoo Jeong Ha, Shawn Shan, Haitao Zheng, Ben Y. Zhao
- **解读**: 文生图模型基于互联网海量图像及视觉语言模型（VLM）生成的图文对进行训练，这一数据管道对提供高质量图文对至关重要；但近期工作表明 VLM 易受隐蔽对抗攻击，攻击者可添加扰动误导 VLM 生成错误字幕。本文研究通过对抗性错误标注（adversarial mislabeling）投毒文生图模型训练数据的可行性。


### Towards Backdoor Stealthiness in Model Parameter Space.
- **作者**: Xiaoyun Xu, Zhuoran Liu, Stefanos Koffas, Stjepan Picek
- **解读**: 后门攻击向机器学习模型注入隐蔽功能，其隐蔽性研究主要聚焦输入空间中不可区分的触发器与特征空间中不可分离的表示，以规避分别检查这两个空间的防御；但现有后门通常只针对单一类型防御设计，未考虑多样化的防御手段。基于此观察，本文提出质疑：面对实际部署的防御时，当前后门是否真正构成现实威胁，并研究参数空间中的后门隐蔽性。


### A Practical and Secure Byzantine Robust Aggregator.
- **作者**: De Zhang Lee, Aashish Kolluri, Prateek Saxena, Ee-Chien Chang
- **解读**: 机器学习安全中常需在计算高维向量均值时剔除离群值：许多数据投毒攻击产生的梯度不在干净梯度分布内，会偏置用于训练模型的均值，因此在求平均前过滤它们是一般性防御策略。拜占庭鲁棒聚合即计算存在 ε 比例向量可能被任意、自适应破坏时的均值，且保证最终结果可证明有界；本文提出一个实用且安全的拜占庭鲁棒聚合器。


### AD-MPC: Asynchronous Dynamic MPC with Guaranteed Output Delivery.
- **作者**: Wenxuan Yu, Minghui Xu, Bing Wu, Sisi Duan, Xiuzhen Cheng
- **解读**: MPC 即服务（MPCaaS）让客户端将隐私保护计算外包给分布式服务器，但传统方案全程依赖固定服务器集合，限制可扩展性；动态 MPC（DMPC）允许参与方在计算中加入或离开，然而现有 DMPC 假设同步网络，在无界网络延迟下会失败。本文提出 AD-MPC，首个异步动态 MPC 协议，在最优弹性 n=3t+1 下保证输出交付，为此引入两个关键组件：一个（使参与方变更与预处理得以异步完成的）预处理机制及相应的异步协议设计。


### IND-CPA-D of Relaxed Functional Bootstrapping: A New Attack, A General Fix, and A Stronger Model.
- **作者**: Zeyu Liu, Yunhao Wang, Ben Fisch
- **解读**: Li 与 Micciancio（Eurocrypt'21）提出 IND-CPA-D 安全性，允许攻击者访问诚实生成密文的解密预言机以强化标准 IND-CPA；Jung 等人（CCS'24）与 Checri（Crypto'24）表明即使 FHEW/TFHE/BGV/BFV 等精确 FHE 方案也可能因自举失败而不安全（此类攻击可借设置可忽略的失败概率来缓解）。Liu 与 Wang（Asiacrypt'24）提出 relaxed functional bootstrapping，性能提升数个数量级且支持自举中免费函数求值。本文针对 relaxed functional bootstrapping 的 IND-CPA-D 安全性给出新攻击、通用修复方案与更强的安全模型。


### Security Analysis of Privately Verifiable Privacy Pass.
- **作者**: Konrad Hanff, Anja Lehmann, Cavit Özbay
- **解读**: Privacy Pass 是由 Davidson 等人（PETS'18）设计的匿名认证协议，用于减少 TOR 用户需解决的 CAPTCHA 数量，签发一次性、可不可链接兑换的 token，签发者与验证者共享对称密钥故为私有可验证；该协议引发学术界与产业界广泛关注并形成 IETF 标准。但 IETF 标准化对该协议做了多处改动，其形式化安全性此前未被检验；本文填补这一空白，分析标准中私有可验证（变体）的安全性。


### Threshold ECDSA in Two Rounds.
- **作者**: Yingjie Lyu, Zengpeng Li, Hong-Sheng Zhou, Xudong Deng
- **解读**: 本文提出首个阈值最优设置下的两轮 ECDSA 多方签名协议，相比 SOTA（Doerner 等，S&P '24）减少一轮通信，并解决 Groth 与 Shoup（Eurocrypt'22）指出的预签名安全问题，避免了安全性损失随已发布但未使用的预签名数量增加而恶化。构造基于非交互式乘法（NIM，Boyle 等 PKC'25 提出，用 Abram 等 '24 的类群实例化），设置最小且透明（仅两个类群生成元），带宽高效，128 位安全级别下消息大小约 1.9 KiB。


### Fast Homomorphic Evaluation of LWR-based PRFs.
- **作者**: Amit Deo, Marc Joye, Benoît Libert, Benjamin R. Curtis, Mayeul de Bellabre
- **解读**: 某些全同态加密应用（如转密（transciphering）、通用阈值化、PIR）需要在加密数据域内持续计算的过程中混然（obliviously）生成随机性，且不同计算应使用互不相关、看似独立的随机硬币。本文研究基于 LWR（Learning With Rounding）的伪随机函数（PRF）的快速同态求值，以在密文域中高效地生成所需随机性。


### Fast Amortized Bootstrapping with Small Keys and Polynomial Noise Overhead.
- **作者**: Antonio Guimarães, Hilder V. L. Pereira
- **解读**: 多数 FHE 方案借助 SIMD 并行处理多条消息，但其安全基于较强假设（超多项式近似因子的近似格问题困难）；另一端的轻量 FHE 自举更快但无 SIMD 能力，安全性仅基于（低次）多项式因子这一更弱假设。Micciancio 与 Sorrell（ICALP'18）提出可一次处理多条消息的摊销自举，使每消息时间呈亚线性，从而兼具两者优点；本文在此基础上实现小密钥、多项式噪声开销的快速摊销自举。


### The Power to Never Be Wrong: Evasions and Anachronistic Attacks Against Web Archives.
- **作者**: Robin Kirchner, Chris Tsoukaladelis, Martin Johns, Nick Nikiforakis
- **解读**: Web 归档保存网页历史快照，但其内容会过时，且归档中可能保留旧版脚本、接口等资源；本文研究攻击者如何利用归档特性实施规避与「时代错置」（anachronistic）攻击，例如诱导用户访问含过时脆弱资源的归档版本，从而危害现代用户。（无摘要，据标题推断）


### Deep Dive into In-app Browsers: Uncovering Hidden Pitfalls in Certificate Validation.
- **作者**: Woonghee Lee, Junbeom Hur, Hyunsoo Kwon
- **解读**: 应用内浏览器（WebView）在提供无缝体验的同时带来证书验证方面的安全隐患，实现不当会使用户暴露于中间人攻击（MITM）或钓鱼风险。本文系统评估 WebView 的验证机制，考察其对 X.509 标准的符合程度以及对撤销与证书透明度（CT）等高级扩展的支持；为在 Android 14+ 上实现可复现的平台特定信任锚控制，开发了统一框架 FAITH（使用物理设备与 iOS 模拟器），并用 115 条精心构造的证书链（含 87 条不合规链）进行测试。


### GAPDiS: Gradient-Assisted Perturbation Design via Sequence Editing for Website Fingerprinting Defense.
- **作者**: Ruotian Xie, Kun Xie, Pengcheng Zhao, Jiajun He, Xin Zeng, Jigang Wen, Yong Xie, Wei Liang, Gaogang Xie
- **解读**: 基于深度学习的网站指纹（WF）攻击日益精准，而现有防御因包方向序列的离散性使基于梯度的优化不可行，只能采用低效的启发式扰动方案。GAPDiS 引入基于梯度对齐偏移向量余弦相似度的奖励来评估并选择沿梯度方向对齐的候选，并设计并行计算算法提升效率，整合为结合引导与改进禁忌搜索的通用扰动生成方法，实现全局优化；为实际部署，GAPDiS 同时支持（PT 代理桥与）P4 交换机等部署方式。


### What Gets Measured Gets Managed: Mitigating Supply Chain Attacks with a Link Integrity Management System.
- **作者**: Johnny So, Michael Ferdman, Nick Nikiforakis
- **解读**: Web 持续增长，但依赖监控工具与资源完整性标准明显滞后，目前缺乏稳健、可推广且高性能的资源完整性验证方法，而供应链是应用攻击面中最受攻击的部分之一。本文提出链接完整性管理系统（link integrity management system），通过测量与管控资源完整性来缓解供应链攻击。


### In the DOM We Trust: Exploring the Hidden Dangers of Reading from the DOM on the Web.
- **作者**: Jan Drescher, Sepehr Mirzaei, Soheil Khodayari, David Klein, Thomas Barber, Martin Johns, Giancarlo Pellegrino
- **解读**: DOM 树是现代 Web 开发的核心，JavaScript 依赖其内容与结构指导程序逻辑与安全决策，但对其可信度的研究很少；此前已知的 script gadget 展示了看似无害的标记注入如何触发良性代码片段执行而被利用。本文表明 gadget 只是冰山一角：看似良性的注入还可触发一类（行为），它们不必然导致 XSS，而是引发更广泛的攻击，如浏览器请求劫持、伪造与用户界面操纵。


### Head(er)s Up! Detecting Security Header Inconsistencies in Browsers.
- **作者**: Jannis Rautenstrauch, Trung Tin Nguyen, Karthik Ramakrishnan, Ben Stock
- **解读**: 安全头（如防护 XSS、点击劫持与信息泄露的机制）对现代 Web 至关重要，但每个头语法与处理规则各异，浏览器与网站开发者正确实现都很复杂；跨浏览器行为的不一致会损害头的有效性（取决于用户所用浏览器），而兼容性问题又阻碍开发者部署。本文检测浏览器间安全头行为的不一致。


### Differentiation-Based Extraction of Proprietary Data from Fine-Tuned LLMs.
- **作者**: Zongjie Li, Daoyuan Wu, Shuai Wang, Zhendong Su
- **解读**: 监督微调（SFT）数据集包含宝贵的指令-响应对，成为潜在的提取目标，但此问题此前未被系统研究。本文首次形式化定义该问题，并基于真实场景中数据的独特性质探索各种攻击目标、类型与变体；在分析直接提取行为的基础上，提出专门针对微调模型的差异化数据提取方法 DDE，利用微调模型对（目标数据）的置信度水平及其与预训练基座模型的行为差异来实现专有数据的提取。


### One Surrogate to Fool Them All: Universal, Transferable, and Targeted Adversarial Attacks with CLIP.
- **作者**: Binyan Xu, Xilin Dai, Di Tang, Kehuan Zhang
- **解读**: 对抗样本的迁移性使攻击者可用代理模型攻击未知目标模型，本文研究以 CLIP 作为通用代理模型，构造通用（universal）、可迁移（transferable）且有目标（targeted）的对抗攻击，即单一代理扰动即可欺骗多个不同目标模型。（无摘要，据标题推断）


### DivTrackee versus DynTracker: Promoting Diversity in Anti-Facial Recognition against Dynamic FR Strategy.
- **作者**: Wenshu Fan, Minxing Zhang, Hongwei Li, Wenbo Jiang, Hanxiao Chen, Xiangyu Yue, Michael Backes, Xiao Zhang
- **解读**: 人脸识别（FR）模型的滥用风险催生了反人脸识别（AFR）研究，但本文指出既有文献普遍采用的静态 FR 评估策略无法如实刻画旨在跟踪特定目标身份的跟踪者的实际能力。作者提出 DynTracker 动态策略——模型的 gallery 数据库随新识别到的身份图像迭代更新——并惊讶地发现这一简单做法能使所有现有 AFR 保护失效；为缓解该隐私威胁，主张明确促进 AFR 保护（图像）的多样性，并假设缺乏多样性是现有保护失效的主要原因。


### What's Pulling the Strings? Evaluating Integrity and Attribution in AI Training and Inference through Concept Shift.
- **作者**: Jiamin Chang, Haoyang Li, Hammond Pearce, Ruoxi Sun, Bo Li, Minhui Xue
- **解读**: AI 训练与推理的完整性与归因问题（如训练数据投毒、模型行为溯源）难以评估，本文提出通过概念漂移（concept shift）来检验模型行为变化，以评估 AI 训练与推理流程中的完整性与归因可信度。（无摘要，据标题推断）


### Busting the Paper Ballot: Voting Meets Adversarial Machine Learning.
- **作者**: Kaleel Mahmood, Caleb Manicke, Ethan Rathbun, Aayushi Verma, Sohaib Ahmad, Nicholas Stamatakis, Laurent Michel, Benjamin Fuller
- **解读**: 本文展示在美国选举计票机中使用机器学习分类器所伴随的安全风险：计票的核心分类任务是判定备选竞选中气泡上是否存在标记。Barretto 等人（E-Vote-ID 2021）曾报告卷积神经网络在该任务上是可行的且优于简单特征分类器，本文在此基础上结合对抗机器学习研究这些分类器被对抗操纵的脆弱性。


### FilterFL: Knowledge Filtering-based Data-Free Backdoor Defense for Federated Learning.
- **作者**: Yanxin Yang, Ming Hu, Xiaofei Xie, Yue Cao, Pengyu Zhang, Yihao Huang, Mingsong Chen
- **解读**: 由于缺乏对不可信客户端的数据审计手段，联邦学习（FL）易受后门攻击，现有防御在极端异构场景下防御效果差，且严重依赖额外数据集、违背 FL 的隐私保护要求。本文提出数据无关的 FL 后门防御 FilterFL：在每个通信轮次，利用两个精心设计的条件生成对抗网络（CGAN）进行（增量）知识提取与过滤，阻止携带后门知识的客户端模型参与聚合操作。


### Ethics in Computer Security Research: A Data-Driven Assessment of the Past, the Present, and the Possible Future.
- **作者**: Harshini Sri Ramulu, Helen Schmitt, Bogdan Rerich, Rachel Gonzalez Rodriguez, Tadayoshi Kohno, Yasemin Acar
- **解读**: 计算机安全研究经常讨论伦理问题，但研究者缺乏关于如何做出、记录与评估伦理决策的清晰指导。本文回顾 2024 年四大顶会的全部 1,154 篇论文，发现伦理报告水平不一致且高度聚焦机构审查委员会批准、人类受试者保护、负责任披露与危害-收益平衡；并报告对 24 名隐私（研究者，其中也包括审稿人、委员会成员或程序主席）的半结构化访谈结果，考察其作为作者与同行评审过程中的决策机制。


### Layered, Overlapping, and Inconsistent: A Large-Scale Analysis of the Multiple Privacy Policies and Controls of U.S. Banks.
- **作者**: Lu Xian, Van Hong Tran, Lauren Lee, Meera Kumar, Yichen Zhang, Florian Schaub
- **解读**: 隐私政策通常复杂，而美国金融机构依《格雷姆-里奇-比利雷法案》（GLBA）提供的两页标准化通知是例外；但银行现在运营网站、移动应用等服务，涉及复杂数据共享实践，需要额外隐私通知与 do-not-sell 退出机制。本文大规模分析银行如何响应 GLBA、联邦政策要求及加州消费者隐私法（CCPA，作为州法代表）实施披露与控制，聚焦第三方营销相关等尤其侵犯隐私的做法；收集了 2,067 家最大银行的数据，其中 45.2% 提供多份政策，且同一银行的不同披露之间存在频繁、令人担忧的不一致。


### How Blind and Low-Vision Users Manage Their Passwords.
- **作者**: Alexander Ponticello, Filipo Sharevski, Simon Anell, Katharina Krombholz
- **解读**: 密码管理对盲人与低视力（BLV）用户仍是开放问题。本文对 33 名 BLV 参与者进行定性访谈研究发现，所有参与者都在一定程度上使用密码管理器并认为其相当可访问，但采用主要受存取密码的便利性驱动；生成强随机密码等优势因缺乏实际可访问性而被回避。密码（管理器）未能满足用户对自主性（agency）的潜在需求，这源于用户在使用不可访问软件时的经历。

### A Decade-long Landscape of Advanced Persistent Threats: Longitudinal Analysis and Global Trends.
- **作者**: Shakhzod Yuldoshkhujaev, Mijin Jeon, Doowon Kim, Nick Nikiforakis, Hyungjoon Koo
- **解读**: 研究问题：APT（高级持续性威胁）长期以隐蔽、持续的方式攻击关键行业，业界虽产生了大量公开的 APT 战役文档，但以往工作多聚焦检测、评估、网络情报或数据集构建等具体方面，缺乏对散落档案的纵向梳理。本文对十余年的 APT 文档进行纵向分析与全球趋势研究，系统回顾这些分布式情报档案，为理解 APT 的长期演化与全球格局提供全景式视角。


### UnsafeBench: Benchmarking Image Safety Classifiers on Real-World and AI-Generated Images.
- **作者**: Yiting Qu, Xinyue Shen, Yixin Wu, Michael Backes, Savvas Zannettou, Yang Zhang
- **解读**: 研究问题：文本到图像模型被滥用的担忧使开发者日益依赖图像安全分类器，但其在真实世界图像与 AI 生成图像上的性能尚属未知。本文提出 UnsafeBench 基准框架：先构建约 1 万张、按 11 类（性、暴力、仇恨等）标注为安全/不安全的图像数据集，再评估 5 个流行分类器及 3 个由通用视觉语言模型驱动的分类器。评估表明现有分类器在全面性与有效性上不足以缓解这一多面问题，且真实与生成图像之间存在质量、风格、版式上的分布偏移导致鲁棒性下降；基于此作者构建了名为 PerspectiveVision 的审核工具以改善分类性能。


### The Importance of Being Discrete: Measuring the Impact of Discretization in End-to-End Differentially Private Synthetic Data.
- **作者**: Georgi Ganev, Meenatchi Sundaram Muthu Selva Annamalai, Sofiane Mahiou, Emiliano De Cristofaro
- **解读**: （无摘要，据标题推断）本文聚焦端到端差分隐私（DP）合成数据流程中离散化步骤的影响：测量离散化操作如何改变合成数据的隐私保证与效用权衡，为端到端 DP 合成数据方法的实现选择提供量化依据。


### Asymmetry Vulnerability and Physical Attacks on Online Map Construction for Autonomous Driving.
- **作者**: Yang Lou, Haibo Hu, Qun Song, Qian Xu, Yi Zhu, Rui Tan, Wei-Bin Lee, Jianping Wang
- **解读**: 研究问题：自动驾驶系统转向基于车载传感器数据的在线高清（HD）地图构建，但其在对抗条件下的鲁棒性未被充分研究。系统性漏洞分析发现这类模型对预测对称道路结构存在固有偏向，在分叉、汇合等非对称场景中常误把直行边界预测成对侧镜像，且该偏向在真实世界可被遮挡等定向干扰可靠触发。基于此提出两阶段攻击框架：先识别受害自动驾驶车辆潜在路线上的脆弱点，再优化相机致盲攻击与补丁攻击的位置与模式；在公开数据集上可使建图平均精度下降 9.9%、44% 的路线不可达、与边界碰撞的不安全规划轨迹率上升 27%，并在实车测试平台验证。作者将对称性偏向归因于训练数据不平衡、模型架构与元素表示，并提出微调作为防御，显著提升鲁棒性。


### Pixnapping: Bringing Pixel Stealing out of the Stone Age.
- **作者**: Alan Wang, Pranav Gopalkrishnan, Yingchen Wang, Christopher W. Fletcher, Hovav Shacham, David Kohlbrenner, Riccardo Paccagnella
- **解读**: 研究问题：像素窃取攻击使恶意网站能够泄露受害网站展示的敏感内容，其思想（Stone, 2013）是通过内嵌 iframe 并利用 SVG 滤镜基于那些网站的像素进行计算，从而构造侧信道。作者指出，得益于现代浏览器对跨源 cookie 共享的严格限制，像素窃取攻击目前几乎已被完全缓解；本文旨在让该攻击类别摆脱这一几乎被终结的'石器时代'现状，探索使其在现代浏览器环境中重新可行的方法。


### HW-Spy: Handwriting Inference by Tracing Pen-Tail Movements.
- **作者**: Long Huang, Kang G. Shin
- **解读**: 研究问题：键盘输入推断攻击已被大量研究，而同样会泄露敏感信息的手写输入推断受到的关注很少。本文提出新型手写推断攻击 HW-Spy：当笔尖与书写面都处于攻击者摄像头视野之外（例如受害者在线上会议中多任务处理、或银行/经销商/房产中介等公共场所的监控摄像头录制远程行为）时，通过追踪受害者笔尾的运动来推断其书写内容，评估了这一被低估的隐私泄露渠道。


### Combating Falsification of Speech Videos with Live Optical Signatures.
- **作者**: Hadleigh Schwartz, Xiaofeng Yan, Charles J. Carver, Xia Zhou
- **解读**: 研究问题：高知名度演讲视频因易获取且影响力大而成为伪造攻击的主要目标。本文提出 VeriLight，一种低开销、无感的系统：与纯数字化的伪造检测方法不同，它在活动现场通过不可感知的调制光创建动态物理签名并将其嵌入所有视频录制中，签名编码事件独有的语义特征（含说话者动作）并经密码学保护以防止欺骗，下游可从任意录制中提取并对照内容校验其完整性。关键组件包括基于局部敏感哈希生成极紧凑（150 位）姿态不变特征的框架，以及实时传输速率大于 200 bps 且不引人注意的光学调制方案；实验显示其检测伪造视频的 AUC 不低于 0.99、真阳性率达 100%，并对录制条件、后处理技术及白盒对抗攻击下的特征提取方法高度鲁棒。


### ConTest: Taming the Cyber-physical Input Space in Fuzz Testing with Control Theory.
- **作者**: Jinwen Wang, Hongchao Zhang, Chuanrui Jiang, Andrew Clark, Ning Zhang
- **解读**: 研究问题：网络物理系统（CPS）的模糊测试面临物理输入与网络输入构成的巨大搜索空间这一开放难题。本文借鉴控制理论，利用物理信息模型引导输入空间探索，设计并实现模糊测试工具 ConTest：用 Lyapunov 函数建模系统动态，通过参数化突变实现高效的检测与变异，并在有界动态误差下提供漏洞发现有效性的可证明保证。原型在 ArduPilot 与 PX4 两个代表性机器人与载具平台上部署，共发现 253 个时空验证类缺陷，其中 58 个为零日缺陷、54 个获厂商确认。


### RISCover: Automatic Discovery of User-exploitable Architectural Security Vulnerabilities in Closed-Source RISC-V CPUs.
- **作者**: Fabian Thomas, Eric García Arribas, Lorenz Hetterich, Daniel Weber, Lukas Gerlach, Ruiyi Zhang, Michael Schwarz
- **解读**: （无摘要，据标题推断）本文针对闭源 RISC-V CPU 提出自动化发现用户可利用架构安全漏洞的方法，无需厂商源码即可系统挖掘 RISC-V 处理器中的架构级安全缺陷，以覆盖闭源实现的安全盲区。


### PickleBall: Secure Deserialization of Pickle-based Machine Learning Models.
- **作者**: Andreas D. Kellas, Neophytos Christou, Wenxin Jiang, Penghui Li, Laurent Simon, Yaniv David, Vasileios P. Kemerlis, James C. Davis, Junfeng Yang
- **解读**: （无摘要，据标题推断）基于 pickle 的机器学习模型序列化格式存在固有的反序列化安全隐患（恶意载荷可执行任意代码），本文提出 PickleBall 方案，为 pickle 格式 ML 模型提供安全反序列化机制，在加载模型时防护恶意构造的 pickle 数据。


### Give LLMs a Security Course: Securing Retrieval-Augmented Code Generation via Knowledge Injection.
- **作者**: Bo Lin, Shangwen Wang, Yihao Qin, Liqian Chen, Xiaoguang Mao
- **解读**: 研究问题：检索增强代码生成（RACG）利用外部知识提升 LLM 代码合成的功能正确性，但现有系统普遍忽视安全，被投毒的知识库可误导 LLM 生成不安全代码。本文提出安全加固框架 CodeGuarder，将范式从'仅检索代码示例'转变为同时纳入代码与安全知识：基于 ReposVul 数据集分析真实漏洞构建知识库，检索器把查询分解为细粒度子任务并获取相关优先级指导，引入利用 LLM 对不同漏洞类型敏感性的重排序与过滤机制，并将过滤后的知识无缝注入提示词。评估显示 CodeGuarder 显著提升安全修复率：相对标准 RACG 平均提升 20.12%，两种不同场景下分别提升 31.53% 与 21.91%，且不损害正确性，并在目标语言知识匮乏时仍表现出强泛化能力。


### Beyond Tag Collision: Cluster-based Memory Management for Tag-based Sanitizers.
- **作者**: Mengfei Xie, Yan Lin, Hongtao Wu, Jianming Fu, Chenke Luo, Guojun Peng
- **解读**: （无摘要，据标题推断）基于标签（tag）的消毒器（sanitizer）依赖标签管理内存，标签冲突之外的分配策略同样影响其安全与性能；本文提出基于簇的内存管理方案，改进标签消毒器的内存布局与管理，以降低开销并减少误报。


### Recover Function Signature from Combined Constraints.
- **作者**: Haohui Huang, Yue Liu, Yuxi Cheng, Haiyang Wei, Jiamu Liu, Yu Wang, Linzhang Wang
- **解读**: 研究问题：函数签名恢复是二进制程序分析的基石，现有方法要么依赖反汇编约束（跨架构兼容性与可扩展性差），要么采用资源密集且常不准确的学习方法。本文提出从组合约束出发恢复函数签名，通过联合多种约束来源兼顾跨架构适用性、可扩展性与准确性，弥补两类现有方法的不足。


### Dangers Behind Access Control: Understanding and Exploiting Implicit Permissions in Kubernetes.
- **作者**: Nanzi Yang, Xingyu Liu, Wenbo Shen, Jinku Li, Kangjie Lu
- **解读**: 研究问题：作为容器编排事实标准的 Kubernetes 被广泛部署，其安全至关重要。本文定义了一个新的攻击面——隐式权限（implicit permission）：显式授予的权限在执行过程中动态地导致对其他资源的操作，使实际权限超出预期范围。这类权限可被利用创建漏洞，攻击者可借此攻陷整个集群。


### Tide: An Efficient Kernel-level Isolation Execution Environment on AArch64 via Dynamically Adjusting Output Address Size.
- **作者**: Shiyang Zhang, Chenggang Wu, Chengxuan Hou, Jinglin Lv, Yinqian Zhang, Qianyu Guo, Yuanming Lai, Mengyao Xie, Yan Kang, Zhe Wang
- **解读**: 研究问题：内核级隔离执行环境（IEE）用于保护关键资源与监控器，但现有 IEE 为隔离 IEE 内存都必须充当引用监视器对页表更新进行校验，带来显著性能开销。本文提出 Tide，基于 AArch64 的输出地址大小硬件特性，将此类校验卸载到硬件；同时针对灵活性与安全挑战，利用 stage-2 地址翻译灵活扩展物理范围以映射内存并施加额外内存访问控制，并设计新颖的门（gate）机制——通过临时禁用该特性安全地进入（sneak）IEE，确保只在固定位置被执行。实验表明 Tide 在保护内核结构方面比现有工具更具性能优势。


### Accountable Liveness.
- **作者**: Andrew Lewis-Pye, Joachim Neu, Tim Roughgarden, Luca Zanolini
- **解读**: 研究问题：安全性与活性是共识协议的两大经典属性，近年研究用可问责性（accountability）强化了安全性——任何违规发生时，可证明相当比例的敌对节点是违规者；本文研究类似的可问责保证在活性上能达到什么程度。为此引入同步模型与部分同步模型之间的插值，称为 x-部分同步网络模型：直观上，在足够长的时间区间内最多 x 个时间步是异步的。作者证明了对活性可问责性可达参数区间的精确刻画，并参数化问题陈述，提供证据表明其分析达到近最优（作为敌对节点数 f 的函数），为以太坊等采用的 'inactivity leaks' 类活性可问责启发式提供了严格的理论基础。


### How to Beat Nakamoto in the Race.
- **作者**: Shu-Jie Cao, Dongning Guo
- **解读**: 研究问题：本文研究有界网络延迟下的工作量证明（PoW）Nakamoto 共识协议，回答两个长期悬而未决的问题：给定确认延迟时对区块安全最有效的攻击是什么？相应的违规概率是多少？作者引入马尔可夫决策过程（MDP）框架精确刻画系统状态（含区块树与所有已挖区块的时间）、对手的潜在动作及对抗动作与随机到达过程带来的转移，提出并证明一种称为 bait-and-switch 的最优攻击，它通过'赢得赛跑'最大化违反安全的概率；利用链分析计算出任意确认深度下该违规概率的精确值，为延迟、共识规则与安全之间的相互作用提供了新见解。


### Committee Selection with Non-Proportional Weights.
- **作者**: Yucheng Sun, Haifeng Yu, Ruomu Hou
- **解读**: 研究问题：委员会被广泛用于各类权益证明（PoS）区块链设计，理想情况下委员会应尽量小且按腐败比例恰当地代表整个系统；现有选择方案均遵循比例性原则（成员既不超额代表也不欠额代表其持有的权益）。本文研究采用非比例权重的委员会选择方案，探讨偏离比例性原则时的选择机制及其对代表性与安全性的影响。


### Elastic Restaking Networks: United we fall, (partially) divided we stand.
- **作者**: Roi Bar Zur, Ittay Eyal
- **解读**: 研究问题：许多基于区块链的去中心化服务要求验证者（运营商）抵押权益，违约即被罚没；再质押（restaking）网络通过复用同一笔权益来保护多个服务，已迅速获得应用（管理超过 200 亿美元）。但再质押引入新的攻击向量：验证者可以协调在多个服务间同时作恶，只被罚没一次却能提取数字资产。本文分析这一'联合则倒、部分分开则立'的弹性再质押网络中的攻击与防御权衡。


### Time Tells All: Deanonymization of Blockchain RPC Users with Zero Transaction Fee.
- **作者**: Shan Wang, Ming Yang, Yu Liu, Yue Zhang, Shuaiqing Zhang, Zhen Ling, Jiannong Cao, Xinwen Fu
- **解读**: 研究问题：RPC 服务已成为用户访问公链的主要网关，但现有去匿名化攻击要么不适用于区块链场景，要么假设主动网络窃听者并产生交易费用等成本。本文提出可将用户 IP 地址与其假名关联的新攻击：分析揭示账本上记录的确认时间戳与受害者查询状态时发送的 TCP 包时间戳之间存在时间相关性。攻击假设具备基础设施的强被动对手（可监控边界路由器与互联网交换点的流量），通过分析账本与数据包相关性即可识别假名发起者，且零费用；作者对方法进行数学建模、大规模测量并在真实世界验证，对以太坊、比特币、Solana 等多种网络中的正常用户成功率超过 95%。


### Breaking Omertà: On Threshold Cryptography, Smart Collusion, and Whistleblowing.
- **作者**: Mahimna Kelkar, Aadityan Ganesh, Aditi Partap, Joseph Bonneau, S. Matthew Weinberg
- **解读**: 研究问题：密码协议常作诚实性假设——例如 n 个参与者中少于 t 个是敌对的——但在实践中，尤其是存在金钱激励促使参与者共谋并偏离协议时，该假设难以保证。本文研究阈值密码学中的'聪明共谋'问题，并探讨参与者作为告密者（whistleblowing）揭发共谋的机制，以缓解诚实性假设失效带来的风险。


### Harnessing Vital Sign Vibration Harmonics for Effortless and Inbuilt XR User Authentication.
- **作者**: Tianfang Zhang, Qiufan Ji, Md Mojibur Rahman Redoy Akanda, Zhengkun Ye, Ahmed Tanvir Mahdad, Cong Shi, Yan Wang, Nitesh Saxena, Yingying Chen
- **解读**: 研究问题：XR 头显日益成为敏感数据的仓库与 Web 应用的入口，传统的口令/PIN 方案不适合其手势/语音界面且易受肩窥攻击，双因素认证又需第二台设备上的额外操作。本文提出首个无感、内置的 XR 用户认证系统，利用用户生命体征激发的振动谐波：呼吸与心跳产生低频机械振动使颅骨振动并产生谐波信号，当头显佩戴时这些信号携带与佩戴者头部结构与软组织相关的丰富生物特征，可由头显运动传感器捕获。系统不直接使用原始信号，而是提取不同频率间比值以捕捉佩戴者独特的头部/面部衰减特性（这些特性在周期性与幅度波动时保持稳定），并设计自适应滤波器减轻常见交互中的身体扰动，结合带注意力机制的深度学习模型实现跨场景的有效鲁棒认证。10 个月、52 名受试者、两款主流头显的评估显示：认证真阳性率超过 95%，拒绝未授权用户达 98% 的阴性率，且长期保持稳定。


### AgentSentinel: An End-to-End and Real-Time Security Defense Framework for Computer-Use Agents.
- **作者**: Haitao Hu, Peng Chen, Yanpeng Zhao, Yuqi Chen
- **解读**: 研究问题：LLM 驱动的计算机使用代理可自主操作系统工具完成任务，但其输出固有的不稳定与不可预测性可能发出非预期命令或错误输入，导致有害操作；与源于不安全用户提示的传统风险不同，LLM 决策的执行结果带来贯穿代理全部组件的新挑战。本文提出 AgentSentinel，端到端、实时的防御框架：在代理相关服务中拦截敏感操作并暂停执行，直至完成全面审计；审计机制引入将当前任务上下文与执行期间产生的系统轨迹关联起来的新型检查过程。作者构建 BadComputerUse 基准（60 个攻击场景、6 类），显示四个 SOTA LLM 的平均攻击成功率为 87%，而 AgentSentinel 的防御成功率达 79.6%，显著优于基线防御。


### Sentry: Authenticating Machine Learning Artifacts on the Fly.
- **作者**: Andrew Gan, Zahra Ghodsi
- **解读**: 研究问题：ML 系统日益依赖第三方创建或托管的开源工件（数据集、模型），这种对外部预训练工件的依赖使系统暴露于供应链攻击——工件可在交付前被投毒，根源是现有 ML 系统缺乏任何真实性验证。哈希与签名等密码学方案可降低风险，但基于这些技术的完整性框架在应用到 SOTA 模型规模时开销巨大且不兼容 GPU 平台。本文开发 Sentry，基于 GPU 的框架，将模型实现与开发者身份和签名绑定，在加载到内存时即时执行认证，数据移动通过 NVIDIA GPUDirect 绕过 CPU；结合 Merkle 树与格哈希等哈希构造加速、内存优化与资源分区方案实现高吞吐。评估表明 Sentry 使 ML 系统获得实用的认证能力，相比 CPU 基线实现数量级的加速。


### Training Robust Classifiers for Classifying Encrypted Traffic under Dynamic Network Conditions.
- **作者**: Yuqi Qing, Qilei Yin, Xinhao Deng, Xiaoli Zhang, Peiyang Li, Zhuotao Liu, Kun Sun, Ke Xu, Qi Li
- **解读**: 研究问题：现有基于深度学习的加密流量分类方法在真实部署中因动态网络条件（环境变化、混淆）而性能下降——训练与测试阶段呈现不同的特征模式。本文提出 MetaTraffic，基于元学习的通用深度学习框架，增强监督模型对动态条件的鲁棒性；核心观察是相同行为在不同条件下共享语义特征（可视为稳定表示），因此 MetaTraffic 通过最小化不同条件下表示方式的差异来学习稳定表示，并设计三个创新模块提升性能。在覆盖多类流量的公开与新建大规模数据集上，MetaTraffic 使准确率提升 8.94%、F1-Macro 提升 12.55%。


### Adversarial Observations in Weather Forecasting.
- **作者**: Erik Imgrund, Thorsten Eisenhofer, Konrad Rieck
- **解读**: 研究问题：基于 AI 的天气预报系统（如 Google 的 GenCast）正重新定义预报技术并接近取代传统气象方法，但也把新漏洞引入预报流程。本文针对自回归扩散模型（即这类系统所使用的）提出新型攻击：通过注入与自然噪声统计上不可区分的细微观测扰动来操纵预报、编造事件，包括飓风、热浪与强降雨，扰动对测量值的改变小于 0.1%——相当于篡改单颗卫星的数据。鉴于现代预报系统集成近 100 颗卫星及许多其他来源的数据，这一发现凸显可能导致大规模破坏并削弱公众对预报信任的严重安全风险。


### Co-Prime: A Co-design Framework for Privacy Preserving Machine Learning on FPGA.
- **作者**: Shuo Xu, Jiming Xu, Pengfei Xue, Xinyao Wang, Lei Ju, Wei Zhang
- **解读**: 研究问题：在多方协同采集数据的隐私敏感机器学习应用中，秘密共享协议是隐私保护机器学习（PPML）的主流策略，但其在训练各线性/非线性阶段产生大量不平衡的通信负载，且该不平衡在硬件加速器降低延迟时被进一步放大。本文提出 Co-Prime，基于 FPGA 的三方（3PC）高效框架，无需第三方协助：集成协议协同优化以缓解瓶颈，提出新颖的格式转换技术并自适应地选择 PPML 偏好的格式，提供加速器友好的原语与系统级设计空间探索，并通过重叠网络通信实现延迟隐藏。实验表明在 LAN/WAN 环境下对多种神经网络模型相比现有框架取得 2-18 倍的推理加速。


### Fuzzy Extractors are Practical: Cryptographic Strength Key Derivation from the Iris.
- **作者**: Amey Shukla, Luke Demarest, Benjamin Fuller, Sohaib Ahmad, Caleb Manicke, Alexander Russell, Sixia Chen
- **解读**: 研究问题：设备级生物识别认证长期存在理论与实践之间的鸿沟——理论构造原则上可用密码学安全的公开注册数据提供认证，但具体实现在真实世界参数下未能兑现安全性，导致部署算法依赖会公开泄露生物特征隐私的数据，系统被迫引入可信执行环境等外部措施。本文在虹膜上实现具备密码学强度密钥派生的实用模糊提取器，弥合这一理论与实践的差距。


### May the ForceNotBe With You: Brute-Force Resistant Biometric Authentication and Key Reconstruction.
- **作者**: Alexandra Boldyreva, Deep Inder Mohan, Tianxin Tang
- **解读**: （无摘要，据标题推断）针对生物识别认证中生物特征数据可被暴力破解的风险，本文提出抗暴力破解的生物识别认证与密钥重建方案，使从生物特征重建的密钥在泄露场景下仍保持安全性。


### Committed Vector Oblivious Linear Evaluation and Its Applications.
- **作者**: Yunqing Sun, Hanlin Liu, Kang Yang, Yu Yu, Xiao Wang, Chenkai Weng
- **解读**: 本文引入承诺向量不经意线性求值（C-VOLE）概念：持有预承诺值的一方可与多方就同一值生成 VOLE 关联。它是一个统一工具，可用于基于值的零知识证明（ZKP）、恶意安全多方计算、私有集合交集（PSI）等场景。


### Lodia: Towards Optimal Sparse Matrix-Vector Multiplication for Batched Fully Homomorphic Encryption.
- **作者**: Jiping Yu, Kun Chen, Xiaoyu Fan, Yunyi Chen, Xiaowei Zhu, Wenguang Chen
- **解读**: 研究问题：加密矩阵向量乘是众多隐私保护应用的基础组件，现有基于全同态加密（FHE）的算法通常用批处理（batching）提升计算效率却忽视矩阵稀疏性，而直接移植明文'跳过零元素'的寻址方式又无法利用批处理优势并引入额外开销。本文针对批量 FHE 场景设计稀疏矩阵向量乘（SpMV）方案 Lodia，向最优计算效率逼近。


### Separating Broadcast from Cheater Identification.
- **作者**: Yashvanth Kondi, Divya Ravi
- **解读**: 研究问题：达到可识别中止（IA）的 MPC 协议保证：若诚实方被拒绝获得输出，将被告知至少一个腐败方的身份，作弊者识别在协议失败时提供追责手段，在某些场景（如密钥管理）甚至比保证输出交付更受青睐。但与更弱的 abort 安全设置不同，IA 协议实质性使用广播信道；本文研究广播信道与作弊者识别能否分离。


### Correlation-Aware Secure Sorting and Permutation for Iterative Two-Party Graph Analysis.
- **作者**: Yunyi Chen, Jiping Yu, Kun Chen, Xiaoyu Fan, Xiaowei Zhu, Wenguang Chen
- **解读**: 研究问题：安全多方计算支持对联合图进行深度分析同时保护数据隐私，但在两方场景下，现有方法因消息传递所需的冗余安全排序或昂贵洗牌操作而低效，部分工作通过差分隐私或引入辅助方来放宽安全假设。本文提出关联感知的安全排序与置换方法，用于迭代式两方图分析，在保持安全性的同时减少冗余操作开销。


### Timing Attacks on Differential Privacy are Practical.
- **作者**: Zachary Ratliff, Nicolás Berrios, James Mickens
- **解读**: 研究问题：差分隐私（DP）已成为计算隐私保护统计的标准方法，但在交互式设置中，DP 查询的可观测运行时间可能无意泄露敏感信息，违反隐私保证。本文表明流行的 DP 库（diffprivlib、OpenDP、PyDP）频繁引入此类时序信道，导致可测量的隐私退化；漏洞不仅存在于常用机制（私有求和、计数、均值、选择），也存在于过滤、排序等预处理步骤中——这些看似无害的操作在个体数据是否存在及其输入顺序影响下表现出不同的运行时间。


### SlicedPIR: Offloading Heavyweight Work with NTT.
- **作者**: Jonathan Weiss, Yossi Gilad
- **解读**: 本文提出 SlicedPIR，一种分布式私有信息检索（PIR）协议：通过把服务器负载卸载到多个不可信的客户端机器上，有效缓解服务器的计算瓶颈，且相比先前工作只带来适度的网络开销。其通信节省源于利用 PIR 协议中通常使用的同态加密方案的多项式编码，创造性地使用数论变换（NTT）把多项式上的点作为数据'切片'分发；客户端对各自切片执行递归查询并返回简洁结果，服务器利用 Schwartz-Zippel 引理（作者针对该场景做了适配）验证客户端结果。作者将其集成进私密消息系统（写入消息后秘密查询好友的消息），原型实验显示其随客户端数量与数据库规模良好扩展，相比 SOTA 带宽消耗降低超过 95%。


### Byte by Byte: Unmasking Browser Fingerprinting at the Function Level using V8 Bytecode Transformers.
- **作者**: Pouneh Nikkhah Bahrami, Dylan Cutler, Igor Bilogrevic
- **解读**: 研究问题：浏览器指纹识别通过细微技术实现跨站持久追踪，常绕过传统防御，而脚本级拦截对策又会导致网站损坏，因此需要兼具函数级精度（以减少误伤）与对代码混淆、URL 操纵固有鲁棒性的检测方法。本文提出利用 V8 字节码转换器在函数级揭示浏览器指纹识别行为的方法。


### Optimal Mechanisms for Quantum Local Differential Privacy.
- **作者**: Ji Guan
- **解读**: 研究问题：集中式差分隐私已成功应用于量子计算与量子信息处理，用于保护相邻状态连接间的信息泄漏；新提出的量子局部差分隐私（QLDP）类似经典场景中所有状态均被视作本地数据的隐私保护框架，但其探索仍处早期、以概念为主，给保障状态隐私的实用实现带来挑战。本文研究量子局部差分隐私的最优机制设计。


### Competing for Attention: An Interview Study with Participants of Cryptography Competitions.
- **作者**: Ivana Trummová, Juliane Schmüser, Nicolas Huaman, Sascha Fahl
- **解读**: 研究问题：密码学竞赛常推动新密码学的发展与标准化，帮助从候选方案中选出安全高效的原语与算法，过去数十年 NIST 等机构举办的竞赛产出了对称/非对称加密、哈希、数字签名乃至近期抗量子密码的标准。尽管竞赛催生了大量针对提交方案的技术研究，人们对密码学竞赛过程中的人类因素（它们如何塑造结果、对安全性的感知影响）知之甚少；本文通过对竞赛参与者的访谈研究来填补这一空白。


### THOR: Secure Transformer Inference with Homomorphic Encryption.
- **作者**: Jungho Moon, Dongwoo Yoo, Xiaoqian Jiang, Miran Kim
- **解读**: 研究问题：大语言模型在云端部署日益普遍，隐私问题突出。本文提出 THOR，基于同态加密的非交互式安全 Transformer 推理框架：首先提出基于对角主导编码与紧凑密文打包的高效矩阵乘法算法，并扩展为支持明文-密文（PC-MM）并行子矩阵计算与采用 baby-step giant-step 策略的密文-密文（CC-MM）计算；同时结合先进逼近技术与自适应迭代方法设计 softmax、LayerNorm、GELU、Tanh 等非线性函数的求值策略。相比 SOTA 方法，THOR 在 PC-MM（ℝ768×ℝ768×128）上比 BOLT 快最多 5.3 倍、在 CC-MM（ℝ64×128×ℝ128×128）上比 Powerformer 快最多 9.7/12 倍，并能在单 GPU 上约 10 分钟完成 128 token 的 BERT-base 推理，同时保持与 GLUE 任务相当（comparable）的精度。


### Optimistic, Signature-Free Reliable Broadcast and Its Applications.
- **作者**: Nibesh Shrestha, Qianyu Yu, Aniket Kate, Giuliano Losa, Kartik Nayak, Xuechao Wang
- **解读**: 研究问题：可靠广播（RBC）是容错分布式系统的关键原语，无签名 RBC 协议因计算高效而受青睐，现有协议在容忍 f<n/3 个拜占庭故障的同时需最优的 3 步才能达成决策。本文提出乐观协议：在特定条件（至少⌈(n+2f-2)/2⌉个非广播方诚实）下仅需 2 步即可终止，同时保持原有容错能力；并证明实现 2 步终止所需诚实方数量的匹配下界。


### GhostCache: Timer- and Counter-Free Cache Attacks Exploiting Weak Coherence on RISC-V and ARM Chips.
- **作者**: Yu Jin, Minghong Sun, Dongsheng Wang, Pengfei Qiu, Yinqian Zhang, Shuwen Deng
- **解读**: 研究问题：微架构侧信道攻击常依赖高分辨率定时器，新兴处理器架构试图通过限制细粒度定时器访问来缓解；本文验证多个 RISC-V/ARM 芯片的 L1 缓存中弱一致性广泛存在，并利用它绕过这类缓解，提出 GhostCache，构造无需定时器、无需计数器的指令侧信道攻击。它引入两个新颖且广泛适用的攻击原语 Modify+Recall 与 Call+ModifyCall，适用于 RISC-V 与 ARM，影响 6 个商用与 3 个开源大处理器，是此类攻击的首次演示；作者还识别出未披露的特性（如 next-three-line 预取机制、从数据缓存直接转发被驱逐指令）。基于此开发了 4 种隐蔽信道，最高达 1.68 MB/s、错误率 0.01%；并实现三种真实攻击：端到端网站指纹攻击（100 类上 92.02% 准确率）、内核信息泄漏（发现新的 Spectre 披露 gadget，函数指针泄漏准确率 92.91%）以及加密密钥重建；最后提出针对这些架构的潜在对策。


### Heracles: Chosen Plaintext Attack on AMD SEV-SNP.
- **作者**: Benedict Schlüter, Christoph Wech, Shweta Shinde
- **解读**: （无摘要，据标题推断）本文提出 Heracles，一种针对 AMD SEV-SNP 的选择明文攻击（CPA），通过可控的明文输入利用 SEV-SNP 内存加密与完整性保护机制的弱点，实现机密性破坏。


### Towards Real-Time Defense against Object-Based LiDAR Attacks in Autonomous Driving.
- **作者**: Yan Zhang, Zihao Liu, Yi Zhu, Chenglin Miao
- **解读**: 研究问题：基于 LiDAR 的目标检测是自动驾驶感知的基石，但易受基于物体的对抗攻击——环境中策略性放置的物理物体操纵点云误导模型，此类攻击实用、隐蔽且无需专用硬件，而现有防御方法计算开销大、泛化性差、无法实时运行。本文提出首个针对自动驾驶中此类攻击的实时防御机制：解决方案对模型与攻击均无关（无需先验知道物体数量、形状、大小或位置），置于自动驾驶流水线的传感与感知模块之间，实时处理点云，采用新颖的生成模型高效识别并移除可疑区域中的点。模拟与真实世界环境的实验表明该方法显著降低攻击成功率且延迟极小。


### RMPocalypse: How a Catch-22 Breaks AMD SEV-SNP.
- **作者**: Benedict Schlüter, Shweta Shinde
- **解读**: （无摘要，据标题推断）本文揭示 AMD SEV-SNP 中一个'两难困境'（Catch-22）式的设计矛盾——即某一防护机制的存在恰好构成可利用的条件——并利用该矛盾攻破 SEV-SNP 的安全保证。


### Dynamic Vulnerability Patching for Heterogeneous Embedded Systems Using Stack Frame Reconstruction.
- **作者**: Ming Zhou, Xupu Hu, Zhihao Wang, Haining Wang, Hui Wen, Limin Sun, Peng Zhang
- **解读**: 研究问题：现有动态漏洞修补技术不适合嵌入式设备，尤其是医疗设备等任务关键系统——它们算力内存有限却要求服务不中断，常缺乏足够空闲时间打补丁，且系统架构多样使补丁触发器难以跨内核与硬件平台兼容。本文提出热补丁框架 StackPatch，基于栈帧重建促进补丁开发，引入不同触发策略以更新存储单元中的程序，并利用处理器普遍提供的异常处理机制增强 StackPatch 的适配性与控制流重定向能力。在涵盖 ARM、RISC-V、Xtensa 三种主流 MCU 架构的平台上评估：成功修复 102 个实时操作系统（RTOS）中公开披露的漏洞，并应用于软 PLC 与网络服务，每次修复在 260 个 MCU 时钟周期内完成。


### Chekhov's Gun: Uncovering Hidden Risks in macOS Application-Sandboxed PID-Domain Services.
- **作者**: Minghao Lin, Jiaxun Zhu, Tingting Yin, Zechao Cai, Guanxing Wen, Yanan Guo, Mengyuan Li
- **解读**: 研究问题：macOS 将许多高特权操作委托给专用 PID-domain 服务，应用可通过 IPC 注册并与之通信；该架构提升用户态稳定性与安全性，但也为攻击者提供了有吸引力的攻击面。本文系统分析这些服务，发现一个被忽视的攻击向量：即使服务与调用应用受相同的应用沙箱（Application Sandbox）限制，仍可因微妙的 entitlement 差异而被利用。


### A System Framework to Symbolically Explore Intel TDX Module Execution.
- **作者**: Pansilu Pitigalaarachchi, Xuhua Ding
- **解读**: 本文提出 TDXplorer，首个针对 Intel TDX Module（TDX 的软件可信计算基）的动态符号分析系统。无需硬件，构建于 TDXplorer 之上的分析器既能控制与插桩 Module 的执行，也能进行执行路径探索与安全功能推理，两种能力被无缝集成，直接作用于二进制代码与运行时状态（由相关技术塑造）。作者在 Linux 上实现并在真实平台执行上验证其性能与正确性；对安全 EPT 创建与 KeyHole 区域管理的建模案例展示了该工具支持多种分析任务的通用性。


### Windows plays Jenga: Uncovering Design Weaknesses in Windows File System Security.
- **作者**: Dong-uk Kim, JunYoung Park, Sanghak Oh, Hyoungshick Kim, Insu Yun
- **解读**: 研究问题：近期一系列攻击利用文件系统触发权限提升等严重安全威胁，但微软一直以针对性补丁而非根本性重设计应对，如同在摇摇欲坠的地基上堆叠措施的'叠叠乐'游戏。本文提出五步综合分析方法：首先分析 Windows 文件系统与 Linux 的差异；其次调查这些差异如何导致真实应用的漏洞，发现 13 个高影响漏洞（其中 11 个此前未知）；第三，证明当前兼容层与编程语言无法正确处理相关语义——检查 6 个实现发现 27 处不合规与 9 处不一致，使其不可靠；第四，对 21 名有经验开发者的用户研究表明他们对操作系统级细节不熟悉、很少实施适当缓解；最后考察现有对策并讨论其局限。研究揭示 Windows 文件系统设计中关键但被掩盖的风险，并建议重新思考其安全策略。


### Forward to Hell? On the Potentials of Misusing Transparent DNS Forwarders in Reflective Amplification Attacks.
- **作者**: Maynard Koch, Florian Dolzmann, Thomas C. Schmidt, Matthias Wählisch
- **解读**: 研究问题：DNS 基础设施因反射放大攻击而臭名昭著，虽已部署服务器屏蔽、访问控制、限速与协议限制等对策，威胁仍随服务器部署持续存在。本文报告并评估一个常被忽视的来源——透明转发器（transparent forwarder），这类广泛部署但功能不完整的组件在转发请求时不重建具有正确源地址的数据包，从而把请求馈入（主要是强大的 anycast）开放递归解析器，使后者被迫参与分布式拒绝服务攻击。作者展示透明转发器如何轻易规避限制措施、通过 anycast 获得额外的可扩展放大（实测放大系数最高达 14 倍），还能协助绕过保护某些实体的防火墙规则，使这些受保护实体成为全球攻击面的一部分，对互联网基础设施构成严重威胁。


### Training with Only 1.0 ‰ Samples: Malicious Traffic Detection via Cross-Modality Feature Fusion.
- **作者**: Chuanpu Fu, Qi Li, Elisa Bertino, Ke Xu
- **解读**: 研究问题：基于机器学习的恶意流量检测系统需从大规模数据集学习才能识别未知攻击，但在每个网络部署都要构建大规模训练集代价高昂。本文研究用极少量样本在新环境中实现准确检测：开发 tFusion，把流量视为包含包、流、主机三种模态特征的多模态数据，设计专用跨模态注意力模型融合细粒度逐包序列与粗粒度逐流/逐主机统计特征，综合不同粒度特征间的相关性；此外用拓扑驱动的对比方法预训练模型、减少拓扑相关偏差，从而在不同网络间获得泛化能力。作者在机构网络部署并测量五天：只需人工标注 1.0‰ 的流量，检测攻击时准确率达 99.82%，并优于 14 种现有方法（提升 12.76%）。


### Fingerprinting Deep Packet Inspection Devices by their Ambiguities.
- **作者**: Diwen Xue, Armin Huremagic, Wayne Wang, Ram Sundara Raman, Roya Ensafi
- **解读**: 研究问题：深度包检测（DPI）设备的商品化使审查、限速、拦截等网络干扰从少数资源丰富的国家级行为者扩展到几乎所有运营商，但我们对 DPI 及其在互联网上的部署了解有限——作为中间设备，DPI 对传统基于主机的扫描工具无响应，且厂商积极掩盖产品进一步妨碍测量。本文提出利用 DPI 设备的歧义（ambiguities）行为对其进行指纹识别，以测绘 DPI 部署。


### Don't Look Up: There Are Sensitive Internal Links in the Clear on GEO Satellites.
- **作者**: Wenyi Morty Zhang, Annie Dai, Keegan Ryan, Dave Levin, Nadia Heninger, Aaron Schulman
- **解读**: 研究问题：地球同步（GEO）卫星链路为公用事业、电信、政府、军事与商业用户的远程关键基础设施提供 IP 回程，但此前学术研究只聚焦少量卫星的特定用例。本文用消费级设备对 39 颗 GEO 卫星、25 个不同经度、411 个转发器进行首次广域流量扫描，并克服先前工作受困的弱信号质量、构建能处理异构端点多样化协议的通用解析器。结果发现 50% 的扫描流量包含明文内容：虽然链路层加密已成为电视传输数十年的标准做法，这些链路通常在链路层和网络层都缺乏加密；这为观察这些组织的内部安全实践提供了独特视角，作者观测到多家运营商的未加密蜂窝流量（含通话与短信内容）、公用事业工业控制系统的作业调度、军事资产跟踪、全球零售门店库存管理以及机上 Wi-Fi 等敏感明文。


### Here Comes the AI Worm: Preventing the Propagation of Adversarial Self-Replicating Prompts Within GenAI Ecosystems.
- **作者**: Stav Cohen, Ron Bitton, Ben Nassi
- **解读**: （无摘要，据标题推断）本文研究生成式 AI 生态系统中对抗性自复制提示（'AI 蠕虫'）的传播机理，并提出防止其在 GenAI 代理与工具链间自主复制与扩散的防护机制。


### Deep Learning from Imperfectly Labeled Malware Data.
- **作者**: Fahad Alotaibi, Euan Goodbrand, Sergio Maffeis
- **解读**: 研究问题：深度学习在恶意软件分类与检测上表现优异，但依赖大规模、准确标注的数据集；实践中多数数据集是自动利用杀毒引擎输出标注的，这一过程常引入显著标签噪声，严重损害深度模型的泛化能力。本文研究如何从不完美标注的恶意软件数据中学习，以缓解标签噪声对模型性能的破坏。


### PreferCare: Preference Dataset Copyright Protection in LLM Alignment by Watermark Injection and Verification.
- **作者**: Jian Lou, Chenyang Zhang, Xiaoyu Zhang, Kai Wu
- **解读**: 研究问题：LLM 对齐训练依赖代价高昂的偏好数据集，这些数据集是使 LLM 遵循人类偏好的关键，其生成与标注成本高、劳动密集，亟需保护其版权免遭未授权使用。本文提出 PreferCare，首个专为偏好数据集版权保护设计的框架，包含两个阶段：注入阶段通过基于风格迁移的信号双层优化过程把水印嵌入数据集；验证阶段用统计检验判定可疑模型是否未经授权使用了含水印数据。在多个流行模型上的大量实验表明其在有效性、无害性、可迁移性与鲁棒性上表现良好，且能在 20 次查询内成功完成验证。


### SCOPE: Expanding Client-Side Post-Processing for Efficient Privacy-Preserving Model Inference.
- **作者**: Shenchen Zhu, Kai Chen, Yue Zhao, Cheng'an Wei
- **解读**: 研究问题：隐私保护推理（PPI）使用户在不泄露敏感输入的情况下利用强大机器学习模型，但现有 SOTA 方案因显著的计算与通信开销而难以实用。本文提出 SCOPE，通过扩展客户端侧后处理来提升隐私保护模型推理的效率。


### Phalanx: An FHE-Friendly SNARK for Verifiable Computation on Encrypted Data.
- **作者**: Xinxuan Zhang, Ruida Wang, Zeyu Liu, Binwu Xiang, Yi Deng, Ben Fisch, Xianhui Lu
- **解读**: 研究问题：加密数据上的可验证计算（VCoed）有两大流行范式——SNARK-FHE（用 SNARK 证明 FHE 运算）与 FHE-SNARK（同态求值 SNARK 证明），现有工作表明后者相比 SNARK-FHE 具有更好的效率。本文提出 Phalanx，一种面向加密数据可验证计算的 FHE 友好 SNARK。


### Practical Zero-Knowledge PIOP for Maliciously Secure Multiparty Homomorphic Encryption.
- **作者**: Intak Hwang, Hyeonbum Lee, Jinyeong Seo, Yongsoo Song
- **解读**: 研究问题：同态加密（HE）是隐私增强密码学的基础技术，面向多方应用推广的同态加密原语（MPHE）近年受到广泛研究；从 MPHE 构造半诚实安全协议是直接的，但实现恶意安全仍具挑战，因为它需要针对密文与公钥的零知识知识论证（ZKAoK）。本文提出用于恶意安全多方同态加密的实用零知识 PIOP（多项式交互式预言证明）。


### Practical TFHE Ciphertext Sanitization for Oblivious Circuit Evaluation.
- **作者**: Intak Hwang, Seonhong Min, Jinyeong Seo, Yongsoo Song
- **解读**: 研究问题：全同态加密（FHE）支持对加密数据求值任意电路，其广泛应用之一是盲目电路求值——发送方在接收方的私有数据上求值，涵盖推理与 PRF 协议等场景；但该场景下安全仅保证对发送方的隐私，发送方的隐私并非仅由 HE 派生（密文可能泄露发送方输入信息）。本文提出实用的 TFHE 密文净化（sanitization）方案，以支撑安全盲目的电路求值。


### Multi-Party Private Set Operations from Predicative Zero-Sharing.
- **作者**: Minglang Dong, Yu Chen, Cong Zhang, Yujie Bai, Yang Cao
- **解读**: 研究问题：多方私有集合运算（MPSO）使 m>2 方能在各自集合上安全计算交集、并集等运算，但典型协议只实现非常有限的功能，多数工作聚焦一两个具体功能，导致方案孤立、缺乏统一研究框架。本文提出统一框架，使各方能在各自持有的集合上安全计算任意公式（有限个二元运算的任意组合，含交集、差集），通用性高、可实例化适应广泛场景，是首个在无需通用 MPC 技术下达到此灵活性与通用性的 MPSO 方案。通信复杂度随集合大小 n 线性增长，对广泛使用的 MPSI、MPSI-card、MPSI-card-sum 达到相对朴素方案的（准）最优，半诚实模型下时间表现优秀；实例化主要依赖对称密钥技术，MPSI、MPSI-card、MPSI-card-sum、MPSU 的在线性能或超越或匹配 SOTA。技术核心是名为 predicative zero-sharing 的新原语，它捕获了 MPC 可组合性的普适性，可能具有独立研究价值。


### WPC: Weight Plaintext Compression for CNN Inference based on RNS-CKKS.
- **作者**: Guiming Shi, Yuchen Wei, Shengyu Fan, Xianglong Deng, Liang Kong, Xianbin Li, Jingwei Cai, Shuwen Deng, Mingzhe Zhang, Kaisheng Ma
- **解读**: 研究问题：基于 RNS-CKKS 的 CNN 推理支持对加密数据的安全处理，但带来巨大的权重规模开销——明文权重可达数十到数百 GB，现有压缩方法要么计算成本高、要么压缩率低。本文提出 WPC 权重明文压缩方案：作者观察到模型向明文格式的转换涉及类似离散傅里叶变换的操作，在时域与频域间切换的同时保留周期离散数据中的冗余信息；据此引入周期传输定理（Periodic Transmit Theorem），说明周期模式在转换过程中可被保留从而支持压缩，并设计 Channel Innermost 打包方案与旋转填充（Rotation Padding）对权重重排。结果表明 WPC 在 A100 GPU 上取得 1.25-2.18 倍加速与 46.08-139.11 倍压缩率。


### FlippedRAG: Black-Box Opinion Manipulation Adversarial Attacks to Retrieval-Augmented Generation Models.
- **作者**: Zhuo Chen, Yuyang Gong, Jiawei Liu, Miaokun Chen, Haotan Liu, Qikai Cheng, Fan Zhang, Wei Lu, Xiaozhong Liu
- **解读**: 研究问题：检索增强生成（RAG）通过动态检索外部知识减少幻觉，但现有对抗研究多为白盒场景或启发式黑盒攻击，未充分探究检索阶段的脆弱性，且多聚焦可被先进 LLM 轻易纠正的事实型问答。本文研究更现实的威胁场景：针对争议性话题的 RAG 意见操纵。作者提出 FlippedRAG，一种基于迁移的攻击：先对 RAG 系统底层检索器进行反向工程与近似（枚举查询、候选与答案）以训练替代检索器，再利用替代检索器制作目标投毒触发器，修改极少量文档即可操纵检索与后续生成，并迁移到原始模型。大量实验表明 FlippedRAG 显著优于基线（平均成功率提升 16.7%），在四个不同领域使 RAG 生成响应的极性发生 50% 的方向性偏移，最终造成 20% 的用户认知改变；作者还评估了多种潜在防御措施，结论是现有缓解策略不足以抵御此类攻击。


### Mosformer: Maliciously Secure Three-Party Inference Framework for Large Transformers.
- **作者**: Ke Cheng, Yuheng Xia, Anxiao Song, Jiaxuan Fu, Wenjie Qu, Yulong Shen, Jiaheng Zhang
- **解读**: 研究问题：BERT、GPT 等 Transformer 模型作为云推理服务部署时存在严重隐私问题，常用安全多方计算（MPC）同时加密用户输入与模型参数，但现有 MPC Transformer 协议多在半诚实安全模型下设计，扩展至恶意安全因安全求值复杂非线性函数带来的开销而极具挑战。本文提出 Mosformer，首个恶意安全三方（3PC）推理框架，高效支持 GPT 等大型 Transformer：设计带恶意安全性的常数轮比较与查找表原语（基于可验证分布式点函数 VDPF），在此基础上构建一整套 3PC 高效求值原语，并结合优化的模数转换大幅降低开销且保持精度。实验表明 vanilla block 上 Mosformer 相比先前协议实现最高 5.3 倍加速与 4.3 倍通信减少，且尽管安全性更强，其在线性能仍堪比或优于 BOLT、BumbleBee、SHAFT、Ditto 等半诚实 2PC 框架，并支持全规模 GPT-2。


### DPImageBench: A Unified Benchmark for Differentially Private Image Synthesis.
- **作者**: Chen Gong, Kecen Li, Zinan Lin, Tianhao Wang
- **解读**: 研究问题：差分隐私（DP）图像合成旨在生成保留敏感数据集特性又不泄露个体隐私的人工图像，但各研究使用的评估协议不一致甚至存在缺陷，阻碍方法理解与领域进展。本文提出 DPImageBench，从多个维度精心设计：(1) 方法——研究 12 种代表性方法，按模型架构、预训练策略与机制系统刻画；(2) 评估——纳入 9 个数据集与 7 个指标全面评估；特别地，指出'在测试集上选准确率最高的下游分类器'这一常见实践违反 DP 并高估效用，DPImageBench 对其予以修正；(3) 平台——为多样化评估协议提供标准化接口，以统一框架容纳不同实现。基准还产生重要发现：与'公开数据通常有益'的常识相反，公开数据与目标数据的分布相似性显著影响性能，预训练并不总能带来改进。


### What Lurks Within? Concept Auditing for Shared Diffusion Models at Scale.
- **作者**: Xiaoyong (Brian) Yuan, Xiaolong Ma, Linke Guo, Lan Zhang
- **解读**: 研究问题：参数高效微调（PEFT，如 LoRA）使任何人都能以极少算力定制强大的预训练扩散模型，但公开平台上大量共享的微调模型可能有意或无意生成版权材料、个人形象、有害内容等敏感内容，尽管监管关注增加，仍缺少部署前系统性审计这些模型的实用工具。本文研究概念审计问题：判定扩散模型是否已学会生成特定目标概念。现有方法依赖提示词构造与基于输出的图像分类，受提示不确定性、概念漂移与可扩展性差等限制；作者提出 PAIA（Prompt-Agnostic Image-Free Auditing），一种以模型为中心的新框架：把对象视作内部模型行为进行直接分析，无需优化提示词或生成图像，集成两个关键组件——通过分析晚期去噪阶段行为来缓解提示敏感性的提示无关策略，以及将模型与其基础版本动态比较、基于条件校准误差的无图像检测方法；假设可访问 DMs 但无需专有数据或用户提示词，与托管平台审计上传模型的方式一致。在 320 个受控训练模型与 771 个来自公开平台的真实社区模型上评估（覆盖名人、卡通角色、游戏实体、电影引用等概念），准确率超过 90%、耗时较现有基线减少 18-40 倍，且对自适应攻击保持鲁棒；这是首个可扩展的部署前扩散模型审计方案。


### Provable Repair of Deep Neural Network Defects by Preimage Synthesis and Property Refinement.
- **作者**: Jianan Ma, Jingyi Wang, Qi Xuan, Zhen Wang
- **解读**: 研究问题：深度神经网络在各类安全威胁（后门攻击、对抗攻击、安全属性违反）下可能表现出危险行为，攻防双方持续军备竞赛。本文提出互补视角，利用近期'神经网络修复'的进展，在统一框架内修复源自不同威胁的多类缺陷：提出 ProRepair，一种由形式化原像合成与属性精化驱动的可证明修复框架。核心直觉：(i) 合成精确的代理盒（proxy box）刻画特征空间中原像，可导出有界距离项以充分指导后续修复步骤走向正确输出；(ii) 执行精化以实现大规模复杂任务上的外科手术式修正。在 4 个任务、6 个基准上评估，ProRepair 在有效性、效率与可扩展性上均优于现有方法：点式修复在保持性能、泛化显著改善的同时，较现有方法加速 5×-2000×；区域式修复成功修复全部 36 个违规实例（最佳基线仅 8 个），并能处理 18 倍更高维的空间。


### DCMI:A Differential Calibration Membership Inference Attack Against Retrieval-Augmented Generation.
- **作者**: Xinyu Gao, Xiangtao Meng, Yingkai Dong, Zheng Li, Shanqing Guo
- **解读**: 研究问题：RAG 通过集成外部知识库减少幻觉，但也引入成员推断攻击（MIA）风险，尤其在处理敏感数据的系统中；现有针对 RAG 数据库的 MIA 多依赖模型响应，却忽视非成员检索文档对 RAG 输出的干扰，限制其有效性。本文提出 DCMI，一种差分校准的成员推断攻击：利用成员与非成员检索文档在查询扰动下的敏感度差异，生成扰动查询以隔离成员检索文档的贡献、同时最小化非成员文档的干扰。在渐进放宽假设的实验中 DCMI 持续优于基线——例如对 Flan-T5 系统达到 97.42% 的 AUC 与 94.35% 的准确率，比 MBA 基线高出 40% 以上；在 Dify、MaxKB 等真实平台上保持 10%-20% 的优势，凸显 RAG 系统显著的数据泄露隐私风险。


### Can Personal Health Information Be Secured in LLM? Privacy Attack and Defense in the Medical Domain.
- **作者**: Yujin Kang, Eunsun Kim, Yoon-Sik Cho
- **解读**: （无摘要，据标题推断）本文聚焦医疗领域 LLM 中的个人健康信息（PHI）隐私：研究针对医学 LLM 的隐私攻击（如成员推断、训练数据提取）并探索相应的防御机制，评估 PHI 在 LLM 中能否得到保护。


### One-Sided Bounded Noise: Theory, Optimization Algorithms and Applications.
- **作者**: Hanshen Xiao, Jun Wan, Elaine Shi, Srinivas Devadas
- **解读**: 研究问题：与常规隐私保护统计发布不同，混淆侧信道信息的随机化常受基础设施限制，实际场景可能只允许正的、有界的扰动——例如延长处理时间或发送/存储伪消息数据通常是可行的，而反向的修改因硬件容量、通信协议与数据管理系统限制而难以实现。本文在差分隐私（DP）、最大泄露（MaxL）与 PAC 隐私三个语义框架下建立单侧有界噪声机制的理论基础，给出刻画近似分布的一系列结果（受二阶矩预算与最大幅度约束），并开发高效工具求解底层优化问题。实验表明现有技术（如截断偏置拉普拉斯噪声）是次优的、造成过度性能退化：在一个 25 万消息预算的匿名系统中，优化后的 DP 方案相比传统方法减少 21 倍消息数并降低 18 倍延迟开销。


### PIIxel Leaks: Passive Identification of Personally Identifiable Information Leakage through Meta Pixel.
- **作者**: Paschalis Bekos, Panagiotis Papadopoulos, Nicolas Kourtellis, Michalis Polychronakis
- **解读**: 研究问题：Web 像素是跟踪转化与用户行为的主流技术，Meta Pixel（使用最广的像素）被集成进网站后可收集访客敏感信息并与他们的 Facebook 或 Instagram 档案匹配；除详细浏览历史外，它还收集访客在网站在线表单中输入的 PII（如邮箱、电话号码）。本文提出被动识别此类经 Meta Pixel 发生的 PII 泄露的方法。


### Amigo: Secure Group Mesh Messaging in Realistic Protest Settings.
- **作者**: David Inyangson, Sarah Radway, Tushar M. Jois, Nelly Fazio, James Mickens
- **解读**: 研究问题：大规模抗议期间，压制性政府常断网阻断抗议者通信；智能手机网状网络通过短距离、可能短暂的无线电连接转发消息，使抗议者无需中心化基础设施即可通信，但先前工作在断网场景下提供安全通信的方案未充分考虑抗议者需求——未能支持高效私密的群组通信（抗议的关键需求），也未在准确反映链路抖动、物理频谱竞争与真实抗议场景移动模型的网络环境中评估。本文提出 Amigo，一种新型消息系统，通过去中心化的持续密钥协商支持私密群组通信，并利用路由协议转发消息，专门应对动态拓扑、节点移动使密钥协商变得困难的 ad-hoc 场景。广泛模拟表明朴素方法的可扩展性差、Amigo 的抗议专用优化带来收益，但数千参与者规模网络的扩展问题仍有待解决。


### Exposing Privacy Risks in Anonymizing Clinical Data: Combinatorial Refinement Attacks onk-Anonymity Without Auxiliary Information.
- **作者**: Somiya Chhillar, Mary K. Righi, Rebecca E. Sutter, Evgenios M. Kornaropoulos
- **解读**: （无摘要，据标题推断）本文针对经 k-匿名化处理的临床数据提出组合精化攻击：在无需任何辅助信息的条件下，通过组合式地精化准标识符取值来逐步缩小匿名等价类、重新识别个体，从而暴露 k-匿名化临床数据中未被充分认识的隐私风险。

### Digital Safety for Children with Intellectual Disabilities When Using Mobile Devices from Parents' and Teachers' Perspectives.
- **作者**: Janice Jianing Si, Xin Lin, Haorui Cui, Xiaobo Zhou, Kanye Ye Wang
- **解读**: 针对智力障碍（ID）儿童使用移动设备时的数字安全问题，该工作通过半结构化访谈家长与特殊教育教师等关键监护角色，归纳出四类核心担忧：模仿危险行为、误操作设备、欺诈风险与网络欺凌暴露。应对策略主要依赖预防性教育辅以技术管控与设备限制。作者呼吁随环境演变调整实践，并将经验推广至其他弱势用户群体。


### VirtualReality,RealProblems: A Longitudinal Security Analysis of VR Firmware.
- **作者**: Vamsi Shankar Simhadri, Yichang Xiong, Habiba Farrukh, Xiaokuan Zhang
- **解读**: 针对 VR 设备固件开展长期安全分析，覆盖多厂商与多版本以刻画固件安全的纵向演进趋势。旨在发现 VR 生态中系统性、持续存在的固件安全问题（无摘要，据标题推断）。


### Hidden in Plain Bytes: Investigating Interpersonal Account Compromise with Data Exports.
- **作者**: Julia Nonnenkamp, Naman Gupta, Abhimanyu Dev Gupta, Rahul Chatterjee
- **解读**: 面向技术助推虐待（TFA）幸存者，研究如何用数据可携权下的数据导出补充账户安全界面（ASI），以调查账户是否被他人入侵，尤其是人际威胁情境。作者在六个流行平台模拟四类攻击并分析导出的数据，发现数据导出普遍包含比 ASI 更细粒度的登录历史与设备/网络标识，部分还能将密码修改等安全动作关联到特定设备，具备取证价值。最后讨论了数据导出在 TFA 干预中的可用性与实际挑战。


### How to Design Secure Honey Vault Schemes.
- **作者**: Zhenduo Hou, Tingwei Fan, Fei Duan, Ding Wang
- **解读**: 研究蜜罐保险库（honey vault）方案的安全设计原则，先识别出三类主要漏洞，并基于严格理论提出对应准则：采用精确概率模型（PPM）的自然语言编码器抵御分布感知区分攻击、用序列化 PPM 抵御重用模式编码攻击、隐藏用户真实保险库相关的自适应 PPM 提取。据此提出 VaultGuard 与 HE-Adaptive 等创新组件，真实数据评估显示攻击者需多执行 1.10–3.98 倍在线验证才能确定真库，并给出客户端侧高效概念验证实现。


### Phishing Susceptibility and the (In-)Effectiveness of Common Anti-Phishing Interventions in a Large University Hospital.
- **作者**: Jan Tolsdorf, David Langer, Luigi Lo Iacono
- **解读**: 在德国一所大学医院对 7,044 个账户开展大规模钓鱼模拟，分析医疗、护理、行政、IT 等不同员工群体的易感性差异，以及邮件时机、语气、上下文与说服性框架对易感性的影响，并评估 11 种常见现场干预的效果。结果显示各群体与干预的效果差异显著，即使少量邮件也构成持续约三天的实质风险；最有效的干预是垃圾过滤与邮件内警告等稳健技术检测，而显示名抑制与 [EXTERNAL] 标记无效。调查还发现员工出现恐惧、羞耻、内疚与敌意等情绪反应，凸显模拟演练的伦理挑战，研究为医院等复杂组织的韧性建设提供可操作指导。


### YouthSafe: A Youth-Centric Safety Benchmark and Safeguard Model for Large Language Models.
- **作者**: Yaman Yu, Yiren Liu, Yuqi Zhang, Yun Huang, Yang Wang
- **解读**: 指出青少年与 LLM 交互的独特脆弱性与风险画像未得到现有安全基准和审核系统充分覆盖。作者构建首个面向青少年–LLM 交互的基准数据集 Youth AI Risk（YAIR），含 12,449 条标注对话片段、78 类细粒度危害类型，基于诱骗、边界侵犯、身份困惑、过度依赖等青少年特有危害分类法；系统评估发现现有方法在检测青少年中心风险上明显不足。据此提出实时检测模型 YouthSafe，在多项分类指标上显著优于既有系统。


### GASLITEing the Retrieval: Exploring Vulnerabilities in Dense Embedding-based Search.
- **作者**: Matan Ben-Tov, Mahmood Sharif
- **解读**: 研究稠密嵌入文本检索（包括 RAG 场景）对搜索引擎优化（SEO）类攻击的脆弱性。作者指出已有工作多采用放宽的威胁模型、针对单查询、使用基线攻击方法且评估规模小，掩盖了对检索器最坏情况行为的全面理解，并旨在以更严格的威胁模型和大规模攻击评估揭示其最坏情况表现（摘要截断）。


### The Phantom Menace in Crypto-Based PET-Hardened Deep Learning Models: Invisible Configuration-Induced Attacks.
- **作者**: Yiteng Peng, Dongwei Xiao, Zhibo Liu, Zhenlan Ji, Daoyuan Wu, Shuai Wang, Juergen Rahmel
- **解读**: 针对同态加密、安全多方计算、零知识证明等加密型隐私增强技术（CPET）与深度学习结合的 CPET-DL 框架（如 CrypTen、TenSEAL、EZKL），揭示这些框架中看似无害的可配置参数可引入隐蔽漏洞。此类由错误配置诱导的攻击难以检测：明文模型本身无漏洞，且现有审计工具难以适用于 CPET 加固模型，形成"本应保护隐私的技术反而可被配置操纵破坏"的悖论。


### Evaluating the Robustness of a Production Malware Detection System to Transferable Adversarial Attacks.
- **作者**: Milad Nasr, Yanick Fratantonio, Luca Invernizzi, Ange Albertini, Loua Farah, Alex Petit-Bianco, Andreas Terzis, Kurt Thomas, Elie Bursztein, Nicholas Carlini
- **解读**: 以 Gmail 恶意文件检测管线为案例，研究针对其中 ML 组件（开源的文件类型识别模型 Magika）的对抗攻击如何降级或绕过整个生产级检测系统。作者仅修改 13 字节即可在 90% 的情况下骗过 Magika 并成功通过 Gmail 上传文件；随后提出防御方案，使高资源攻击者需约 50 次尝试才能达到 20% 成功率，该防御已在 Google 工程师协作下部署于线上分类器。


### Cascading Adversarial Bias from Injection to Distillation in Language Models.
- **作者**: Harsh Chaudhari, Jamie Hayes, Matthew Jagielski, Ilia Shumailov, Milad Nasr, Alina Oprea
- **解读**: 研究对抗者如何通过训练期极少量数据投毒向教师模型注入细微偏见，该偏见在蒸馏到更小的学生模型时被显著放大。作者识别出非定向（影响多任务）与定向（聚焦单任务且其他行为正常）两种传播模式，仅 25 个毒样本（0.25%）即可使定向场景下 76.9% 的响应带偏见（教师为 69.4%），非定向场景中学生模型在未见任务上偏见率高 5.7–29.2 倍。现有防御（困惑度过滤、检测系统、LLM 自动评审）均证明无效，作者据此提出构建有效缓解策略的设计原则。


### You Can't Steal Nothing: Mitigating Prompt Leakages in LLMs via System Vectors.
- **作者**: Bochuan Cao, Changjiang Li, Yuanpu Cao, Yameng Ge, Ting Wang, Jinghui Chen
- **解读**: 针对 LLM 系统提示泄露风险，指出"禁用模型重复上下文"等现有对策仍易被新型未知泄露技术攻破，并先提出一种简单有效的攻击，可提取 GPT-4o、Claude 3.5 Sonnet 等 SOTA 模型应用的系统提示。受此启发提出 SysVec，用内部表示向量而非原始文本编码系统提示，在最小化未授权泄露风险的同时保留核心能力，实验显示不仅能有效缓解泄露攻击，还提升了一般指令跟随能力并缓解长上下文遗忘问题。


### Exact Robustness Certification of k-Nearest Neighbors.
- **作者**: Francesco Ranzato, Ahmad Shakeel, Marco Zanella
- **解读**: 为 k-NN 分类器提供精确（完备）的鲁棒性认证框架。既有认证方法 sound 但不完备，会漏掉许多真正鲁棒的输入；该工作将超立方体空间分解与基于邻近优先关系图的新颖图论分析结合，实现扰动下鲁棒性的完全覆盖。在常用数据集上的大规模评估表明，认证率显著优于现有技术且保持可扩展性，弥合了可靠性与完备性之间的差距。


### IOValve: Leakage-Free I/O Sandbox for Large-Scale Untrusted Data Processing.
- **作者**: Sangho Lee, Jules Drean, Yue Tan, Marcus Peinado
- **解读**: 面向大规模不可信数据处理场景，提出无泄漏的 I/O 沙箱方案。该方案在隔离不可信 I/O 的同时保证不产生任何信息泄漏（无摘要，据标题推断）。


### BadAML: Exploiting Legacy Firmware Interfaces to Compromise Confidential Virtual Machines.
- **作者**: Satoru Takekoshi, Manami Mori, Takaaki Fukai, Takahiro Shinagawa
- **解读**: 揭示机密虚拟机（CVM）中一个此前被忽视的攻击面：恶意云提供商可利用遗留但广泛采用的 ACPI 固件接口，通过 ACPI Machine Language（AML）向客户机内核注入二进制代码并影响启动完整性证明，且不受具体配置限制。作者在 Linux 与 Windows 上完成概念验证，分析了多种缓解措施的效力与局限，并提出 AML 沙箱化防御，在 18 个真实实例上验证了其设计、实现与有效性。


### Protocol-Aware Firmware Rehosting for Effective Fuzzing of Embedded Network Stacks.
- **作者**: Moritz Bley, Tobias Scharnowski, Simon Wörner, Moritz Schloegel, Thorsten Holz
- **解读**: 针对嵌入式网络协议栈模糊测试，提出协议感知的固件重托管方法 Pemu：通过自动推导可用协议并透明生成封装模糊数据的合法数据包，使测试输入直接流入更深层的协议逻辑，实现逐层深入的目标化分析。评估显示 Pemu 在三个工具/协议栈上一致提升代码覆盖率，重新发现了多个已知漏洞并找到 5 个未知缺陷，验证了其在网络暴露代码上的漏洞挖掘能力。


### Dynamic Detection of Vulnerable DMA Race Conditions.
- **作者**: Brian Johannesmeyer, Raphael Isemann, Cristiano Giuffrida, Herbert Bos
- **解读**: 提出 DMARacer，一种动态检测内核代码中 DMA 相关竞态条件的检测器：在内核整个生命周期内跟踪 DMA 访问并结合多种指标分析，检测到竞态后进一步用污点追踪评估其影响，识别可能引发的内存破坏或拒绝服务等漏洞。对 Linux 驱动的大规模扫描表明该问题具有系统性，共检出 817 个可疑操作与 344 个易受攻击操作。


### Attestable Builds: Compiling Verifiable Binaries on Untrusted Systems using Trusted Execution Environments.
- **作者**: Daniel Hugenroth, Mario Lins, René Mayrhofer, Alastair R. Beresford
- **解读**: 提出"可证明构建"（attestable builds）范式，用 TEE 沙箱化容器保证给定二进制确实由特定源码快照正确构建，为软件制品提供强的源码到二进制对应关系。相比可复现构建只需最少项目改动且提供近乎即时的验证，评估显示构建开源库的启动延迟为 42 秒、构建时长增加约 14%，并成功构建 LLVM/Clang 等复杂项目而无需修改其构建脚本。作者还形式化建模了该设计，证明其能抵御资源充足的对抗者。


### Augmenting Search-based Program Synthesis with Local Inference Rules to Improve Black-box Deobfuscation.
- **作者**: Vidal Attias, Nicolas Bellec, Grégoire Menguy, Sébastien Bardin, Jean-Yves Marion
- **解读**: 面向黑盒去混淆，提出将基于搜索的程序合成与局部推理规则结合的新方法 Smir（Search Modulo Inference Rules），在保持通用性与灵活性的同时提升合成能力。作者将其实例化为针对混合布尔算术等难题的工具 XSmir，在真实二进制与合成基准上显著优于先前去混淆器：混淆表达式合成率 76%–84%（非混淆恢复率 63%–55%），误报减少 2–3 倍且压缩率略有提升。


### Right the Ship: Assessing the Legitimacy of Invalid Routes in RPKI.
- **作者**: Andong Chen, Yangyang Wang, Jia Zhang, Mingwei Xu
- **解读**: 针对 RPKI 部署阻力，评估 ROV 判定为 invalid 的路由中有多少其实合法。全球仍有超过 40% 的路由缺乏 RPKI 保护，部分网络不愿部署的关键顾虑正是担心过滤掉合法路由会损害网络服务质量与连通性；该工作旨在量化无效路由的合法性，为运营商决策提供依据。


### Exploring and Analyzing Cross Layer DoS Attack Against UDP-based Services on Linux.
- **作者**: Dashuai Wu, Yunyi Zhang, Baojun Liu, Xiang Li, Eihal Alowaisheq, Haixin Duan
- **解读**: 研究 TCP/IP 分层架构中共享资源管理不当引入的跨层资源风险 SocketFilled：链路层对传输层 UDP 发送缓冲的不安全使用可中断上层应用的服务响应，形成 DoS。作者系统分析了相关协议标准并审查主流开源操作系统实现，实验表明 Linux、FreeBSD 上的 BIND9、PowerDNS、Nginx 等最新版本服务均受影响，造成显著丢包乃至完全中断。实测估计 17.3% 的开放解析器与 54.3% 的 Tranco Top 100K 权威服务器受影响，作者已向社区负责任披露。


### Off-Path TCP Exploits: PMTUD Breaks TCP Connection Isolation in IP Address Sharing Scenarios.
- **作者**: Xuewei Feng, Zhaoxi Li, Qi Li, Ziqiang Wang, Kun Sun, Ke Xu
- **解读**: 揭示 PMTUD 在设计上未妥善处理 IP 地址共享，使攻击者得以实施 off-path TCP 劫持：通过观察服务器为共享地址确定的路径 MTU 值，Internet 上的攻击者与恶意设备协作即可推断同一地址上其他合法设备连接的序号。攻击平均 220 秒完成、成功率 70%，SSH DoS、FTP 流量投毒与 HTTP 注入等案例展示了实际威胁；在 50 个真实共享网络（Wi-Fi、VPN、5G 等）中 38 个受影响，作者已向 IETF、Linux、Cisco 负责任披露并提出对策。


### SISTAR: An Efficient DDoS Detection and Mitigation Framework Utilizing Programmable Data Planes.
- **作者**: Junjie Hu, Feng Guo, Qihang Zhou, Yixin Zhang, Zibo Gao, Yinglong Han, Zhiqiang Lv
- **解读**: 提出基于可编程数据平面（PDP）的 DDoS 检测与缓解框架 SISTAR，集成改进的决策树-约束阈值分割（DT-CTS）模型，在保持线速吞吐的同时提升检测精度并最小化硬件资源占用。通过多交换机分布式部署增强对协同攻击的韧性，并可与告警回推机制结合降低资源消耗；原型在真实测试床上评估，效率优于现有模型。


### ScannerGrouper: A Generalizable and Effective Scanning Organization Identification System Toward the Open World.
- **作者**: Xin He, Enhuan Dong, Jiyuan Han, Zhiliang Wang, Hui Zhang, Liang Liu, Lianyi Sun, Supei Zhang, Pengfei Xue, Guanglei Song, Han Li, Xiaowen Quan, Jiahai Yang
- **解读**: 针对互联网主动扫描泛滥现象，研究识别扫描行为背后组织身份的问题。作者指出既有研究虽已分析扫描源，但尚未能在真实场景中有效且泛化地识别扫描组织，提出 ScannerGrouper 系统，面向开放世界实现通用有效的扫描组织识别。


### On the Security of SSH Client Signatures.
- **作者**: Fabian Bäumer, Marcus Brinkmann, Maximilian Radoy, Jörg Schwenk, Juraj Somorovsky
- **解读**: 对 SSH 客户端签名密钥的安全性开展纵向研究：从 GitHub、GitLab 等公开来源收集 31,622,338 个公钥并系统化测试，发现相比以往研究，生态明显从 RSA 转向 EdDSA；但截至 2025 年 1 月仍发现 98 个短密钥、139 个弱随机性密钥与 149 个含公共小因子的密钥。作者首次证明确定性 nonce 的危险性：在 NIST P-521 曲线下仅凭 58 个有效签名即可恢复 PuTTY 的私钥，该发现被确认为 CVE-2024-31497，PuTTY 随后更换了 nonce 生成算法。


### Toss: Garbled PIR from Table-Only Stacking.
- **作者**: Lucien K. L. Ng, Vladimir Kolesnikov
- **解读**: 提出基于"纯表堆叠"（table-only stacking）技术的乱码 PIR（GPIR）新构造 Toss，用于在两方都不知道索引的情况下，用乱码电路高效地在公开数组/数据库上按私有索引查值。由于 GPIR 直接蕴含公开查表（LUT）函数的求值，该构造可显著提升这类安全两方计算场景的效率。


### Secure Noise Sampling for Differentially Private Collaborative Learning.
- **作者**: Olive Franzese, Congyu Fang, Radhika Garg, Xiao Wang, Somesh Jha, Nicolas Papernot, Adam Dziedzic
- **解读**: 针对协作学习（CL）中 DP-SGD 的噪声注入时机问题：聚合前注入（分布式 DP）需更多总噪声、损害模型效用，聚合中注入（经安全聚合）虽保效用但需 MPC 协议，而现有 MPC 噪声生成方法因参与方众多每样本耗时数百秒，无法用于每步数千样本的实际训练。该工作旨在设计安全且高效的噪声采样方案，使协作学习中的差分隐私保护实用化（摘要截断）。


### Post-Quantum Threshold Ring Signature Applications from VOLE-in-the-Head.
- **作者**: James Hsin-yu Chiang, Ivan Damgård, William R. Duro, Sunniva Engan, Sebastian Kolby, Peter Scholl
- **解读**: 基于 AES 加密的单向性与 VOLE-in-the-Head 零知识证明系统，构造高效的后量子门限环签名，可扩展到大型环并支持可链接范式；作者定义并构造密钥绑定确定性标签实现链接性，并将其用于类 Monero 的匿名账本交易。进一步借助近似下界论证实现无需 SNARK 的简洁聚合，得到近似多签名，全部构造仅依赖对称密钥原语。


### Distance-Aware OT with Application to Fuzzy PSI.
- **作者**: Lucas Piske, Jaspal Singh, Ni Trieu, Vladimir Kolesnikov, Vassilis Zikas
- **解读**: 提出距离感知随机 OT（da-ROT）并基于其构建模糊私密集合交集（PSI）的模块化框架，主要依赖高效对称密钥原语：da-ROT 使发送方获得两个串、接收方按关键词是否 δ-接近决定获得哪一个，是传统 OT 的自然扩展。构造支持 Chebyshev、Euclidean 与 Manhattan 度量，相比先前最优协议通信成本最高降低 14 倍、计算成本最高降低 54 倍，并可扩展至基数、连接等 PSI 相关功能，全部协议在半诚实模型下安全。


### MegaBlocks: Breaking the Logarithmic I/O-Overhead Barrier for Oblivious RAM.
- **作者**: Gilad Asharov, Eliran Eiluz, Ilan Komargodski, Wei-Kai Lin
- **解读**: 针对分层 ORAM 虽渐近最优却长期被认为不可实用的现状，作者利用近期进展（FutORAMa, CCS'23）表明客户端内存充足时可高效实现 ORAM 方案。据此提出新构造，在不要求大量本地存储的前提下实现实用性能，突破分层 ORAM 的对数 I/O 开销屏障（摘要截断）。


### CuKEM: A Concise and Unified Hybrid Key Encapsulation Mechanism.
- **作者**: Yiting Liu, Biming Zhou, Haodong Jiang
- **解读**: 提出简洁统一的混合密钥封装机制（KEM），组合经典与后量子算法。以统一接口在单一机制内兼顾抗量子安全性升级与性能权衡（无摘要，据标题推断）。


### Demo: Stopping Production Testing: A Graphical RPKI Test-Suite.
- **作者**: Tobias Kirsch, Haya Schulmann, Niklas Vogel
- **解读**: 演示一个图形化 RPKI 对象测试工具套件：可解析任意 DER/base64 编码对象、编辑其内容结构，并针对现行验证软件实现做实时测试，以探测不一致结果、错误乃至漏洞，帮助运营商在生产部署前发现错误配置。以 2023 年 Amazon 事件（约 6000 个前缀险些因特定验证器实现而暴露于劫持）说明动机；工具已开源并提供托管 Web 应用。


### Demo: Reverse Engineering Android Apps with Code Coverage.
- **作者**: Aleksandr Pilgun
- **解读**: 演示借助代码覆盖率信息辅助 Android 应用逆向工程的方法。以覆盖率指导分析路径、聚焦实际执行代码，提升逆向分析的效率与针对性（无摘要，据标题推断）。


### Demo: Exploring Utility and Attackability Trade-offs in Local Differential Privacy.
- **作者**: Haoying Zhang, Abhishek Kumar Mishra, Héber Hwang Arcolezi
- **解读**: 演示 LDP-Toolbox，首个交互式、基于 Web 的工具箱（Python 实现），对本地差分隐私在隐私损失预算（ε）、效用损失与攻击脆弱性之间的权衡进行实用化、可分析的可视化探索，并支持基于真实世界数据集（如离散个人属性与位置类数据）的演示。该工具降低了 LDP 实际部署的门槛，已开源并发布至 PyPI 与 GitHub。


### Poster: ERIS: Evaluating ROV via ICMPv6 Rate Limiting Side Channels.
- **作者**: Renrui Tian, Yahui Li, Han Zhang, Xinzhe Liu, Xia Yin, Xingang Shi, Zhiliang Wang, Jilong Wang
- **解读**: 提出 ERIS，利用 ICMPv6 速率限制侧信道来测量评估 RPKI 路由源验证（ROV）的真实部署情况。既有测量或依赖控制平面数据导致覆盖与精度受限，或需要受控 IP 前缀与大规模观测点，且大多聚焦 IPv4；该工作旨在填补 IPv6 下 ROV 部署状态研究的空白。


### Poster: Longitudinal Analysis of Romance Scam Infrastructure Evolution: Evidence of Strategic Legitimization.
- **作者**: Nayeon Ryu, Heeyeong Suh, Seyoung Lee
- **解读**: 对 11,674 个情感诈骗档案（2012–2024，ScamDigger 数据）做 12 年基础设施演化分析，发现 2019 年出现明显拐点：诈骗者并非单纯技术升级，而是战略性迁移向合法基础设施——传统 ISP 使用率大幅上升，代理使用率从 2019 年前的平均 90.6% 降至 53.1%。地理上呈现专业化分工（尼日利亚、加纳等西非 ISP 运营基地与美、德等欺骗性西方云服务），基础设施多样性收缩 31.3%，表明该犯罪生态正通过"合法性利用"走向工业化，需相应调整检测方法。


### Poster:LLMalware: An LLM-Powered Robust and Efficient Android Malware Detection Framework.
- **作者**: Zijing Ma, Leming Shen, Xinyu Huang, Yuanqing Zheng
- **解读**: 提出基于大语言模型（LLM）的 Android 恶意软件检测框架。利用 LLM 的语义理解能力，旨在提升检测的鲁棒性与效率（无摘要，据标题推断）。


### Poster: Adaptive Gradient Clipping with Personalized Differential Privacy for Heterogeneous Federated Learning.
- **作者**: Hao Zhou, Hua Dai, Siqi Cai, Geng Yang, Yang Xiang
- **解读**: 提出 GC-DP 联邦学习框架，通过自适应调整梯度裁剪阈值实现个性化差分隐私：用代理数据集学习从隐私预算（ε）到最优裁剪阈值（C*）的客户端特定映射，同时允许各客户端依据自身梯度的 L2 范数实时调整本地裁剪界。这种双重自适应性在异构联邦学习（FL）场景中显著改善隐私保护与模型效用之间的平衡。


### Poster: Recapture Detection Using Disparity Map Obtained from Dual-Pixel Image Sensors.
- **作者**: Tetsu Ishizue, Sara Rampazzi, Takeshi Sugawara
- **解读**: 针对用相机翻拍显示器攻击图像来源验证与深度伪造防御的问题，利用智能手机相机中常见用于快速对焦的双像素（DP）图像传感器提取深度线索，无需额外硬件。信号处理流水线由立体匹配算法获取视差图与平面拟合评估场景平坦度两部分组成；概念验证在数据集上以 100% 准确率检测翻拍，并能以高于 98% 的准确率区分深度伪造图像。


### Poster: Black-box Attacks on Multimodal Large Language Models through Adversarial ICC Profiles.
- **作者**: Chengbin Sun, Hailong Sun, Guancheng Li, Jiashuo Liang
- **解读**: 提出 ICCAdv，一种无需模型参数或梯度的黑盒攻击：利用 ICC 色彩管理流程中人类观察者感知的是经 ICC 配置文件变换渲染后的图像、而 MLLM 直接处理原始 RGB 值的差异，向图像文件嵌入恶意 ICC 配置，在不破坏人眼观感的前提下操纵模型感知到的语义。初步实验表明该攻击能有效攻击保持 SOTA 防御的模型。


### Poster: Towards Intelligent Assurance for Autonomous AI Pentesters: Concurrent Compliance Auditing and Self-Augmentation via Execution Trace Analysis.
- **作者**: Gustavo Sánchez, Adam Lundqvist
- **解读**: 面向由 AI 驱动的自主渗透测试工具（AI Pentesters），提出智能保障系统（IAS）：通过监控执行轨迹提供持续问责，并在直接人类监督有限或缺失时实施合规审计（如欧盟 AI 法案）、实时反馈与自我改进。该系统以资源高效的方式增强完全自主渗透测试工具的可信度与道德规范性。


### Poster: Code HarvETHter: Corpus-Driven Decompilation of Ethereum Smart Contracts.
- **作者**: Jens-Rene Giesen, Christian Scholz, Lucas Davi
- **解读**: 提出 HarvETHter，面向以太坊、BNB、Polygon 等 EVM 平台的智能合约反编译器。作者提出"语料完备性假设"并借此展开研究：利用 HarvETHter 掌握的以太坊区块链源码知识库，将合约反编译回 Solidity 源码。


### Poster: We must talk about RPKI Repositories.
- **作者**: Haya Schulmann, Niklas Vogel
- **解读**: 指出 RPKI 的防护效果依赖于存放在公共仓库中的对象的可用性与安全性，但人们对这些仓库的技术细节知之甚少：使用何种实现、软件是否得到维护、是否存在持续漏洞。回答这些问题对评估当前 RPKI 架构的韧性至关重要，该工作旨在系统调查 RPKI 仓库的安全状况。


### Poster: Insecure Coding Habits Die Hard. Can PEFT Really Turn LLMs into Secure Coders?
- **作者**: Sangjun Chae, Jangseop Choi, Taeyang Kim, Eun Jung, Sanghak Oh, Hyoungshick Kim
- **解读**: 提出"安全优先构建"（secure-by-construction）的框架，用参数高效微调（PEFT）让 LLM 生成安全代码：自动修复 CodeNet 中 37,540 个 C 样本的 7 类高危漏洞构建安全训练集（CWE 降低 95.36%），再对四个开源模型做提示前缀微调（更新参数少于 1%）。在 LLMSeC-Eval 基准上，安全生成比例从 20% 提升到 36%（StarCoder2-3B）、从 10% 提升到 27%（CodeGen-6B），证明 PEFT 无需全量重训即可实质改善代码安全。


### Poster: PainNOVA: Privacy-Aware Voice-Based Pain-Level Detection.
- **作者**: Andrew Lu, Mashrafi Kajol, Wei Lu, Dean Sullivan
- **解读**: 提出隐私感知的基于语音的疼痛等级检测框架：从音频中提取频域特征，移除与性别等敏感属性相关的特征，再用 CNN 进行 3 级分类。基于 TAME Pain 数据集的初步分析表明 log-Mel 谱图与过零率是两种有前景的语音特征；进一步在非言语数据集 VIVAE 上验证了该方法对言语与非言语患者的适用性。


### Poster: Public Key Encryption with Exclusionary Subset Keyword Search from Lattices.
- **作者**: Yi-Fan Tseng, Jheng-Jia Huang, Guan-Yu Chen, Ting-Hsiang Su
- **解读**: 提出一种支持新型搜索模式——排斥性子集搜索——的公钥可搜索加密（PKSE）方案，使用户能检索不含指定关键词集合的密文，在需排除少量关键词的场景下更直观高效，且该模式无法由现有 PKSE 方案平凡实现。构造基于格，支持恒定大小陷门，在性能上优于既有基于格的方案。


### Poster: The Rocky Road Towards RPKI Algorithm Agility.
- **作者**: Katharina Miesch, Haya Schulmann, Niklas Vogel
- **解读**: 指出 RPKI 目前仅支持 RSA-2048 签名，禁止其他算法与密钥长度，阻碍了算法敏捷性。作者实证表明引入更小更高效的算法（如椭圆曲线密码 ECC）可显著获益，同时说明现行算法迁移计划将因带宽限制、验证开销与补丁管理问题而难以满足后量子迁移需求。据此提出新的敏捷流程：采用独立仓库版本并新增混合树与遗留树，实现算法的增量部署，且相比现有方法也让早期采用者受益。


### Poster: Leveraging Large Language Models to Effectively and Efficiently Identify Vulnerability Patches for WordPress Plugins.
- **作者**: Xue Leng, Hai Zhang, Tiantian Zhu, Jianguo Sun
- **解读**: 针对 WordPress 插件生态中漏洞补丁识别困难的问题：既有方法多依赖规则匹配，不适用于缺乏统一开发标准的插件生态；结合漏洞描述与代码变化的预测方法虽有潜力，但现有模型缺乏深度语义理解，难以把握代码变更背后的含义。该工作旨在利用 LLM 提升补丁识别的有效性与效率（摘要截断）。


### Poster: Exploring the Landscape of RPKI Relying Parties.
- **作者**: Donika Mirdita, Haya Schulmann, Michael Waidner
- **解读**: 首次系统研究 RPKI 依赖方（RP）——负责在路由器上收集与执行 RPKI 校验的组件——的部署图景：映射其地理位置、配置参数与漏洞分布，并比较相隔三年的两次测量以刻画演化。分析显示三年内依赖方流量增长 45% 且 89% 来自单一软件类型，运维者强烈偏好自托管但安全缓解率不足。


### Poster: When Logs Misbehave: Retrieving Known APTs from Noisy Graphs.
- **作者**: Guo-Wei Wong, Yi-Ting Huang, Ying-Ren Guo, Ming-Chuan Yang, Shou-De Lin, Wang-Chien Lee, Meng Chang Chen
- **解读**: 将已知高级持续性威胁（APT）活动检索任务形式化为带噪声、部分观测图上的近似子图匹配：节点为 MITRE ATT&CK 技术、边编码时间或资源关系，而真实场景中目标图常因检测缺失、技术误分类与良性行为结构噪声而含噪。作者提出基于嵌入的初步检索方法，向鲁棒实用的现实 APT 检索框架推进。


### Poster: Computer Security Researchers' Experiences with Vulnerability Disclosures.
- **作者**: Harshini Sri Ramulu, Anna Lena Rotthaler, Jost Rossel, Rachel Gonzalez Rodriguez, Dominik Wermke, Sascha Fahl, Tadayoshi Kohno, Juraj Somorovsky, Yasemin Acar
- **解读**: 调研安全研究人员在漏洞披露过程中的实际经历：研究者常难以确定受影响方及其联系方式，还可能被忽视或面临敌意。作者提取最佳实践，并向研究人员、其雇佣机构、行业与监管者提出建议，以促成更有效的漏洞披露。


### Poster: Reconsidering DNS-Based Domain Verification: Privacy and Overhead Implications.
- **作者**: Eunbee Hwang, Hyunsoo Kim, Ted Taekyoung Kwon
- **解读**: 指出基于 DNS TXT 记录的域名验证（DV）虽已成为各类在线服务证明域名所有权的关键机制，但会无意泄露敏感的商业关系、增大 DNS 响应体积并加重解析器基础设施负担。对超过 250 万条记录的测量凸显了臃肿记录的普遍性；该工作作为初步问题陈述，为设计隐私高效的新型 DV 机制提供动机。


### Poster: Impulse in the Clickstream: Behavioral Insights from Browsing History.
- **作者**: Shubham Malaviya, Anuj Bagad, Manish Shukla, Sachin Lodha
- **解读**: 针对钓鱼攻击利用用户冲动性导致不经评估即点击恶意链接的问题，提出用浏览器历史数据刻画用户冲动性的新方法。会话级分析显示用户行为具有情境性与上下文依赖性；研究为基于实时行为的自适应安全干预提供可操作的洞见。


### Poster: An Obfuscation Framework for Mitigating Topology Probing Attacks in Cloud-Native Systems.
- **作者**: Xue Leng, Kaiwen Shen, Chengxuan Zhu, Xing Li, Jianguo Sun
- **解读**: 针对云原生系统中微服务远程调用通信侧信道被用于拓扑探测攻击（如 DDoS 前置）的问题，提出 MeshGuard 混淆框架：构建服务级动态迷宫以自适应避免泄露流量模式，对混淆后的拓扑用定制参数伪装，并设计基于标签的机制确保不影响正常微服务。初步结果显示 MeshGuard 能以可接受的资源开销有效保护服务。


### Poster: GLog: Self-Evolving Log Anomaly Type Prediction via Instruction-Tuned LLM and Clustering.
- **作者**: Junwei Zhou, Yuyang Gao, Cheng Tan, Yanchao Yang, Jianwen Xiang
- **解读**: 提出 GLog，端到端的日志异常类型预测框架：先用正常/异常标签微调指令微调 LLM，在原始未解析序列上实现高精度检测；再对检测到的异常聚类自动生成伪描述，用于第二阶段微调，使模型能输出可解释的具体异常类型。通过利用完整语义并动态更新知识库，GLog 降低标注成本并适应大规模环境中的演化行为。


### Poster: PQ Noise Explorer.
- **作者**: Animesh Thakur, Mikael Asplund
- **解读**: 发布 PQ Noise Explorer，一个用于设计、形式化验证与实现后量子 Noise 协议模式的 Rust 仓库。该工具可验证任意模式并将其翻译为自动化验证模型，同时提供 Rust 编写的软件实现。


### Poster: Obfuscating Function Activity States to Enhance Privacy in Serverless Applications.
- **作者**: Xue Leng, Fengming Zhu, Xing Li, Ye Tian, Jianguo Sun, Yan Chen
- **解读**: 针对无服务器（FaaS）计算中函数实例共享物理基础设施、共驻攻击可经侧信道泄露函数活动状态的问题，提出 FaaSGuard 混淆框架：采用自适应策略选择机制提升灵活性，通过流量伪装使混淆流量与正常流量不可区分，并以标签机制避免影响正常函数。初步评估显示能以可忽略的资源开销隐藏函数活动状态。


### Poster: Model-driven Privacy Analysis of Messaging Platforms.
- **作者**: Muqaddas Naz, Muhammad Taimoor Khan, Muhammad Waqas
- **解读**: 针对即时通讯平台隐私分析中需求重叠且相互冲突（保密性、匿名性、不可链接性、用户同意等）的难点，提出基于复合模型的框架以捕获这些需求间的相互依赖，实现隐私违规与法律影响的系统化识别。应用于 WhatsApp 等平台背后的实时通信服务器 Ejabberd，聚焦保密性与同意驱动问题（含知情权与删除权），验证了该方法在技术分析与合规问责之间架桥的有效性。


### Poster: Agentic Shell Honeypot Using Structured Logging.
- **作者**: Kai Wei, Guangjing Wang
- **解读**: 提出 HoneyAgents，基于智能体的 shell 蜜罐系统：以角色委派架构与策略智能体协同应对提示注入，用结构化日志实现长时记忆以保持交互一致性，并在多智能体协作中采用分层规划生成可挖掘的动态响应。评估显示其在鲁棒性、真实感与效率上均优于传统规则蜜罐与既有 LLM 蜜罐，使 LLM 驱动的蜜罐更适用于安全运维。


### Poster: An Improved Quantum Attack on the Two-round Even-Mansour Cipher with Independent Permutations and Keys.
- **作者**: Rentaro Shiba, Tetsu Iwata
- **解读**: 提出针对两轮迭代 Even-Mansour 密码（含两个独立置换与三个子密钥）的量子攻击，采用 Q1 模型：敌手进行经典在线查询与量子离线查询。攻击复杂度为 O(2^(n/2))（n 为分组大小），而该变体此前最优攻击复杂度为 O(2^(3n/5))，显著改善了计算复杂度。


### Poster: Post-Quantum Readiness in EdDSA Chains.
- **作者**: Kostas Kryptos Chalkias
- **解读**: 评估 EdDSA 签名链的后量子迁移准备情况。探讨在证书链、信任链等依赖 EdDSA 的场景中升级至后量子签名方案的可行性与挑战（无摘要，据标题推断）。


### Poster: Reducing Hull Dimensions for Efficient Permutation Recovery in Code-Based Cryptography.
- **作者**: Keita Ishizuka
- **解读**: 研究线性码置换等价问题（PEP）中支撑分裂算法（SSA）的效率与 hull 维数的关系：SSA 需要坐标置换下不变的 hull，但可改用其他内积来降低维数。作者给出完整刻画——置换不变双线性形式恰为 aI + bJ 结构矩阵，并证明这些 hull 维数可至少降低一维且给出降维条件；该改进使枚举复杂度获得因子 q 的加速，对 LESS、SPECK 等方案的代码分类、密码分析与参数选择有实际价值。


### Poster: LogCraft: Crafting CVE-Aware Synthetic Worlds (Logs).
- **作者**: Kai-Xian Wong, Chan-Jien Tan, Yi-Ting Huang, Ying-Ren Guo, Yu-Zih Jheng, Guo-Wei Wong, Meng Chang Chen
- **解读**: 提出 LogCraft，基于 CVE 知识生成"感知 CVE"的合成日志数据（Synthetic Worlds/Logs）。旨在为日志异常检测等安全分析提供可控、带标注的训练与评估数据（无摘要，据标题推断）。


### Poster: High-Fidelity and Contextual User Activity Memory Forensics.
- **作者**: Min-Chieh Wu, Jui-An Chang, Yu-Sung Wu
- **解读**: 提出 RAM-Weaver 内存取证系统，支持对内存转储中用户活动文件的情境化查询：先从原始转储中提炼关键信息，并借助 LLM 导航其中晦涩的数据结构以支持交互式查询。该提炼过程可将数据量减少达 99.9%、信噪比提升近 37 dB，使其可在更小的离线模型上运行；与云端全规模模型结合时能以极高精度检索用户活动。


### Poster: LogCADA: Cross-System Log Anomaly Detection based on Two-Stage Multi-Source Domain Adaptation.
- **作者**: Junwei Zhou, Ying Zhu, Linhao Wang, Jianwen Xiang, Cheng Tan, Yanchao Yang
- **解读**: 提出 LogCADA，基于迁移学习的跨系统日志异常检测框架：通过双层对抗训练获取公共特征，并以多源域对比对齐区分不同域的特有特征，以联合优化迁移效果与模型泛化能力。结果表明该方法可适配双源与单源场景，有效克服传统方法的固有限制，在真实日志分析场景中具有实用价值。


### Poster: Efficient Threshold ML-DSA up to 6 Parties.
- **作者**: Sofía Celi, Rafaël del Pino, Thomas Espitau, Guilhem Niot, Thomas Prest
- **解读**: 提出首个与 NIST 后量子标准 ML-DSA（Module-Lattice-based Digital Signature Algorithm）兼容的门限签名方案，支持最多 6 方协同签名且不泄露各方的秘密份额。该工作采用先进的短秘密共享技术并优化拒绝采样，以在分布式场景中平衡通信量与正确性；Go 实现经本地、LAN 与 WAN 基准测试，适用于多设备加密货币钱包、TLS、Tor 目录机构等实际应用。


### Poster: Scalable Privacy-Preserving Linear Regression Training via Homomorphic Encryption.
- **作者**: Yena Cho, Hyoungshick Kim
- **解读**: 提出 PP-LR，端到端基于 CKKS 的线性回归训练协议，支持梯度下降的特征级并行与条件自举。相比标准同态加密实现，训练速度最高提升 15.7 倍，且在四个真实数据集上与明文模型的精度差距保持在 0.2% 以内。


### Poster: Adversarial Habituation Attack: A Psychological Extension and Re-framing of Boiling Frog Attack.
- **作者**: Tashi Stirewalt, Assefaw Hadish Gebremedhin
- **解读**: 从心理学视角对"煮青蛙攻击"（Boiling Frog Attack）进行延伸与重新框定，提出"对抗性习惯化攻击"概念。研究通过渐进、微小的恶意调整使目标逐渐习惯并忽视异常，从而在长时间尺度上降低安全警觉的攻击模式（无摘要，据标题推断）。


### Poster: The Art of Deception: Crafting Chimera Images for Covert and Robust Semantic Poisoning Attacks.
- **作者**: Lin Li, Youyang Qu, Jiayang Ao, Ming Ding, Chao Chen, Jun Zhang
- **解读**: 提出利用生成式 AI 制作隐蔽且鲁棒的毒样本"奇美拉图像"（Chimera Images）：融合两个目标类别的视觉特征生成保持外观保真度的混合对象，以带正确标签的"正常"样本悄然扭曲模型决策边界而不引起怀疑。CIFAR-10 与 Flowers 数据集上的实验表明该方法能降低目标类准确率、保持其他类性能，并对 SOTA 防御策略免疫；作者还发现近期提出的快照检测机制对 AI 生成样本无效。


### Poster: Randomness Unmasked: Towards Reproducible and Fair Evaluation of Shift-Aware Deep Learning NIDS.
- **作者**: Lucy Steele, Fahad Alotaibi, Sergio Maffeis
- **解读**: 研究随机性与环境变化对深度学习网络入侵检测系统（NIDS）评估的影响，聚焦可检测并适应分布漂移的 shift-aware 模型（代表长期部署的 SOTA 系统）。作者在受控变化下考察两个基线并分析各随机因素对结果可复现性与公平性的影响，发现即使微小的变化也会让 F1 分数大幅波动，且所有因素都显著影响部分结果；据此提出支持可复现、公平评估学习型 NIDS 的实用建议。


### Poster: Inferring On-Screen Keyboard Input via Gamepad-based Mouse Movement Traces.
- **作者**: Gyujeong Jin, Seyoung Lee
- **解读**: 研究通过基于手柄（gamepad）的鼠标移动轨迹推断屏幕键盘输入的旁路攻击。利用输入设备控制方式引入的移动模式侧信道泄露击键信息，威胁触摸/屏幕键盘场景下的输入机密性（无摘要，据标题推断）。


### Poster: Decoding Social Engineering: A Multi-Level Framework for Tactic Generation, Annotation, and Evaluation.
- **作者**: Yicun Tian, Youyang Qu, Ming Ding, Shigang Liu, Pei-Wei Tsai, Jun Zhang
- **解读**: 针对钓鱼邮件中嵌入的复杂社会工程（SE）策略，开发生成、标注与评估三层级 SE 策略的方法：以"生成—交叉验证—高亮"流水线构建标注可靠的语料，保证标签与嵌入策略的语义对齐，再将触发片段聚类合成为细粒度模式，刻画策略在浅层（L1）、中等（L2）与深层（L3）的表现。基于 LLM 的实验显示检测准确率随策略复杂度增加而下降，验证了框架的分层能力及其在培训、仿真与策略感知设计中的用途。


### Addressing Vulnerabilities and Opacities in Quantum Service Providers.
- **作者**: Rupshali Roy
- **解读**: 针对 NISQ 时代第三方量子云服务的增长，指出其安全与不透明问题：不可信的编译器、映射器与分配器可能让用户电路易受攻击或为节省资源而故意降低执行质量；通信信道缺少端到端加密、电路实际执行位置不可见，这些漏洞可能导致数据泄露、设计者知识产权损失、访问控制失效与程序破坏。作者的博士工作旨在识别这些漏洞并制定防御策略，以维护量子软件栈的完整性。

### AI-Augmented Static Analysis: Bridging Heuristics and Completeness for Practical Reverse Engineering.
- **作者**: Monika Santra
- **解读**: 针对逆向工程中代码与数据交错、符号/类型/栈帧缺失、激进编译优化及多种混淆导致传统启发式静态分析不可靠的问题，论文提出融合 AI 与尽力而为（best-effort）完整性分析的思路：用 AI 替代脆弱的启发式规则以增强泛化能力，同时以启发式手段补充完整性，满足安全关键型二进制分析的可靠性要求。研究聚焦指令边界识别、函数控制流图（CFG）构建（特别是间接调用目标识别）三项被学术研究与现有工具忽视的任务，目标是构建端到端的反汇编框架。


### Hardware-assisted Memory Isolation.
- **作者**: Hongyi Lu
- **解读**: 论文研究以 Intel MPK、Arm POE、RISC-V PMP 为代表的硬件辅助内存隔离机制，此类机制可为关键数据与执行上下文提供保护，且避免了纯软件方案的开销。同时指出其共性局限：可用的保护域（domain）数量有限，且主要聚焦 CPU 内存，与外设交互相关的内存访问得不到保护，由此引出待解决的研究问题。


### S2S-SED: A Speech-to-Speech Approach for Detection of Social Engineering Attacks in Audio Conversations.
- **作者**: Leonardo Erlacher
- **解读**: 针对依赖生成式语音合成、神经语音克隆与心理自适应操纵的语音社会工程攻击日益复杂，论文指出基于转录文本的 LLM 欺诈检测遗漏了重音、紧迫感与对话动态等韵律和交互线索，构成根本性局限。为此提出 S2S-SED 语音到语音检测框架：直接处理原始音频，编码为隐式保留韵律模式、情绪与语义内容的潜在表示，由统一推理模型评估话轮转换等对话动态，不依赖中间文本。研究还计划构建领域标注的语音社会工程数据集，用于训练与评估音频原生的 LLM。


### Security and Privacy Measurements in Cellular Networks: Novel Approaches in a Global Roaming Context.
- **作者**: Gabriel K. Gegenhuber
- **解读**: 论文以蜂窝网络作为全球通信骨干为背景，指出其因架构创新（如 VoIP、eSIM）与漫游、虚拟运营商、零费率等商业动态而日益复杂，但缺乏开放、可扩展、地理多样化的测量工具，导致相关安全与隐私状况难以理解。研究旨在全球漫游情境下提出新的测量方法，填补这一工具与认识上的空白。


### Postmortem Voice Cloning: Individuals Perspectives of Ownership and Deceptive Harms.
- **作者**: Jennifer Vander Loop
- **解读**: 论文研究逝者语音克隆（postmortem voice cloning）的滥用风险：语音克隆仅需数秒音频即可复刻他人声音，已用于冒充诈骗，而克隆逝者声音可能被用于冒充逝者或对遗属实施情感操纵。研究采用问卷与访谈相结合的混合方法，在既有危害分类学指导下开展主题分析，考察个体对所有权、隐私预期及监管的看法。目标是产出关于人去世后语音的防护措施、法律保护与平台责任方面的认知与建议。


### Towards Explainable and Robust Deepfake Detection and Attribution: Enhancing Multimedia Forensics for the Next Generation of Synthetic Media.
- **作者**: Raphael Antonius Frick
- **解读**: 针对生成式 AI 使合成音视频几乎无法与真实媒体区分、而现有深度伪造检测多为数据驱动的二元分类、难以满足高利害场景下归因与可解释性需求的问题，论文提出构建鲁棒、可泛化、可解释的取证框架：(1) 不仅能检测 AI 生成媒体，还能将攻击归因到特定模型或方法；(2) 为法律场景提供可解释证据；(3) 对对抗性操纵保持韧性。研究整合基于模型的技术并利用外部数据、延续多媒体取证既有工作，论文报告了关键挑战、目标与早期发现。


### Scalable Cryptography for Trustworthy Machine Learning in the LLM Era.
- **作者**: Gefei Tan
- **解读**: 论文指出 MPC 与零知识证明虽能提供可证明的安全保证，但通用协议在大规模机器学习（尤其大模型时代）中不切实际，并提出核心主张：针对具体 ML 子任务结构协同设计的协议，可在不牺牲隐私或可验证性的前提下达到实用效率。研究分三条线展开：(1) 机密外包训练——定制 MPC 将昂贵步骤转移至本地计算，使资源受限的数据所有者能在不可信云上安全训练；(2) 面向大数据集的可扩展原语——混淆洗牌、隐私连接、稀疏线性代数等可证明构件以弥合大规模隐私管线的性能差距；(3) 可验证再训练——以"最优性证明"框架认证微调模型（如 LoRA 适配器）满足所需性质，无需重放训练过程即可高效、可审计地部署。


### Turning Uncertainty into Efficiency: Toward Practical, Quantum-Resistant Verifiable Privacy Tools.
- **作者**: Jie Xu
- **解读**: 论文针对可验证随机函数（VRF）与不经意伪随机函数（OPRF）等隐私保护应用（匿名认证、隐私集合求交、去中心化身份）中现有构造依赖数论假设、易受量子攻击的问题，研究基于格的高效实用构造以实现后量子安全。其关键障碍是零知识证明（尤其是区间证明）在规模与证明者复杂度上的开销，为此探索放宽精确正确性的概率技术，如利用多项式环自同构进行近似代数变换、以乘法模拟内积论证。目标是构建更可扩展的 VRF（含群/上下文绑定变体）与 OPRF，并使其具备可部署性。


### On Defining Reproducible Outcomes for the Computer Security Community.
- **作者**: Daniel Olszewski
- **解读**: 论文以计算可复现性为核心，指出安全社区虽然日益重视通过发布工件让他人独立复现结果，但现有做法是否切实有效仍是开放问题。研究拟测量既有可复现性举措的影响、构建提升可复现成果的框架与工具，并系统分析已有的实践路径。目标是向安全研究者提供简化研究复现的手段。


### Towards Explainable and Effective Anti-Money Laundering for Cryptocurrency.
- **作者**: Qishuang Fu
- **解读**: 论文针对加密货币洗钱检测中现有方法缺乏可解释性或难以有效处理隐私币的问题，提出两个研究方向：其一，通过交易语义解析与基于大语言模型的分类，构建面向非隐私币的可解释反洗钱（AML）框架；其二，研究涉及门罗币（Monero）等隐私币的非法资金流追踪，聚焦提升去匿名化精度并识别跨链路径。初步结果表明语义感知模块与微调模型在检测复杂行为上具有可行性。


### Towards Solving Real-world Problems of Post-quantum Cryptography.
- **作者**: Suparna Kundu
- **解读**: 论文以基于大整数分解与椭圆曲线离散对数问题的公钥密码易受大规模量子计算机攻击为背景，指出 NIST 发布 PQC 标准方案虽是里程碑，但相关研究相比成熟数十年的传统密码方案仍不成熟。研究聚焦广泛部署前必须解决的开放问题：侧信道等物理攻击分析及其对策、面向特定应用的方案修改、资源受限设备上的轻量级实现，以及与各类安全协议的集成。目标是弥合 PQC 从理论到实践之间的差距。


### Dissertation Research Description: The Potential of SBOMs to Increase Software Supply Chain Security.
- **作者**: Lukas Gehrke
- **解读**: 论文以 XZ Utils 事件等供应链攻击为背景，指出立法推动（美国行政令 14028、欧盟 2024 年《网络弹性法案》2027 年强制 SBOM）与工程实践脱节：从业者难以真正使用 SBOM，普及与流程集成面临多重挑战，且生成工具的准确性问题突出。围绕"SBOM 能带来何种收益、还能如何进一步提升安全性"提出四项贡献：系统化梳理 SBOM 的历史与相关计算机科学概念；构建包含参与方的 SBOM 使用模型，为实践提供理论基础；开发 BOM2VULN 工具将 SBOM 映射到漏洞，帮助工程师快速查找并评估漏洞相关性；以及提出评估第三方代码风险的"Nutri Score"评分方法。


### Exploring the Janus Face of Synthetic Images: From Privacy-secure Biometrics Applications to Deepfake Detection for Misinformation-Free Social Networks.
- **作者**: Tanusree Ghosh
- **解读**: 论文聚焦生成式 AI 的"双面性"：一方面，合成超逼真人脸可为生物识别系统长期面临的隐私与数据稀缺难题提供解决方案，这是有前景但尚未充分探索的应用方向；另一方面，同一技术被武器化用于制造深度伪造，在在线社交网络（OSN）上助长虚假信息，威胁信息完整性。研究将同时探索这两个方向——隐私安全的生物识别应用与面向无虚假信息社交网络的深度伪造检测。


### ACM CCS Young Scholars Development Program.
- **作者**: F. Betül Durak, Fengjun Li, Sophie Stephenson
- **解读**: 该文介绍 ACM CCS 青年学者发展计划（YSDP）：一项由专门主席团队发起并组织、旨在支持计算机安全领域早期研究者的新举措。计划通过讲座、专题讨论、分组技术交流等结构化活动促进合作、沟通与职业成长，强调技能培养与人脉构建，帮助青年学者融入更广泛的学术生态。文章详述了该计划的规划、选拔与执行过程，并指出其对塑造更强研究环境的作用。


### AISec '25: 18th ACM Workshop on Artificial Intelligence and Security.
- **作者**: Maura Pintor, Matthew Jagielski, Ruoxi Jia
- **解读**: 该文介绍第 18 届 ACM 人工智能与安全研讨会（AISec'25）：AI/ML 已成为网络安全领域的重要工具（如通过发现人眼与硬编码规则难以察觉的模式来识别恶意软件），但其可信性与鲁棒性在面临自适应攻击者时引发担忧，隐私威胁也日益凸显，需要恰当的测试与可能的缓解手段以防敏感信息被窃取或泄露。研讨会为探讨安全与机器学习交叉领域的新进展提供交流平台。


### CPSIoTSec'25: The 7th Joint Workshop on CPS & IoT Security and Privacy.
- **作者**: Kassem Fawaz, Daisuke Mashima
- **解读**: 该文介绍第 7 届 CPS 与 IoT 安全隐私联合研讨会（CPSIoTSec'25，2025 年 10 月 17 日于台北与 CCS'25 同地举办）：该研讨会由 2019 年两个分别聚焦网络物理系统与 IoT 安全隐私的研讨会合并而来，旨在为学术界、工业界与政府部门搭建协作论坛。本届征稿涵盖全文、进展中工作与一页摘要，议程包括九篇 CPS/IoT 全文或短文及一篇虚拟网络安全测试床演示论文，并邀请 Daniel Xiapu Luo 教授就汽车网络安全作主题演讲。


### ARTMAN '25: Third Workshop on Recent Advances in Resilient and Trustworthy MAchine learning-driveN systems.
- **作者**: Gregory Blanc, Takeshi Takahashi, Zonghua Zhang
- **解读**: 该文介绍第三届"弹性可信机器学习驱动系统"研讨会（ARTMAN'25）：旨在汇聚安全隐私与机器学习领域及交通、航空航天、医疗、能源、金融等行业的研究者与从业者，共同探讨可信且有韧性的学习驱动应用与系统。研讨聚焦 AI/ML 领域，欢迎基础与应用两方面的 ML 贡献，尤其关注知识驱动的可靠安全算法，以提升人机协作等场景下的弹性与信任。


### WATCH '25: First Workshop on Analytics, Telemetry, and Cybersecurity for HPCC.
- **作者**: Massimo Cafaro, Eric Chan-Tin, Jerry Chou, Jinoh Kim
- **解读**: 该文介绍首届面向 HPCC（高性能计算与通信）的分析、遥测与网络安全研讨会（WATCH'25），主题围绕高性能计算与通信系统上的数据分析与遥测采集，及其在网络安全中的应用。（无摘要，据标题推断）


### SCORED '25: Workshop on Software Supply Chain Offensive Research and Ecosystem Defenses.
- **作者**: Aditya Sirish A Yelgundhalli, Behnaz Hassanshahi, Dennis Roellke, Drew Davidson, Kathleen Moriarty, Lorenzo De Carli, Marcela S. Melara, Santiago Torres-Arias, Sarah Evans, Yuchen Zhang
- **解读**: 该文介绍软件供应链攻防研究研讨会（SCORED'25），主题涵盖针对软件供应链的攻击性研究（offensive research）与生态系统层面的防御机制。（无摘要，据标题推断）


### HAIPS '25: First ACM CCS Workshop on Human-Centered AI Privacy and Security.
- **作者**: Tianshi Li, Toby Jia-Jun Li, Yaxing Yao, Sauvik Das
- **解读**: 该文介绍首届以人为中心的 AI 隐私与安全研讨会（HAIPS'25）：AI/ML 的进展既带来新型隐私安全挑战（生成式 AI 产生有害内容、编程助手生成不安全代码、向 ChatGPT 过度分享、LLM 智能体意外泄露信息），也为解决长期存在的终端用户问题、赋能从业者采纳更好实践提供了新机会。研讨会旨在凝聚关注人本视角议题的研究社区，推动把"人"这一要素纳入考量的跨学科研究议程。


### WPES '25: 24th Workshop on Privacy in the Electronic Society.
- **作者**: Jianying Zhou
- **解读**: 该文介绍第 24 届电子社会隐私研讨会（WPES'25，与 ACM CCS 会议同期举办）：对隐私意识的政策、法规与技术的需求已获广泛认可，研讨会围绕全球互联社会中的隐私问题及可能的解决方案展开讨论。作为面向当代电子社会各类隐私议题的年度论坛，WPES'25 延续了 24 年来对隐私议题的持续关注。


### SURE '25: The 1st ACM Workshop on Software Understanding and Reverse Engineering.
- **作者**: Zion Leonahenahe Basque, Ati Priya Bajaj
- **解读**: 该文介绍首届 ACM 软件理解与逆向工程研讨会（SURE'25，与 CCS 2025 同地举办）：针对软件快速创建与我们对代码分析和推理能力之间日益扩大的差距，研讨会设有主题演讲、论文环节、海报与圆桌挑战。会议收到 15 篇投稿并录用 9 篇（录用率 60%），汇聚了多样化方法、凸显了共同挑战，并开始厘清"软件理解"研究的定义，为该关键领域更清晰、更连贯的研究议程奠定了基础。


### CheckMATE '25: Research on Offensive and Defensive Techniques in the Context of Man At The End (MATE) Attacks.
- **作者**: Sébastien Bardin, Michele Ianni, Hyungon Moon
- **解读**: 该文介绍 MATE（终端攻击者）攻防技术研讨会（CheckMATE'25）：MATE 攻击者拥有对软件或硬件目标的完全访问权，能够观察、分析、修改运行中的系统，提取机密并改变行为。研讨会同时关注攻防两侧——揭示现实攻击技术的测量研究与工具，以及混淆、防篡改、水印、白盒密码与硬件辅助保护等防御手段，旨在汇聚严谨可复现的研究，弥合学术进展与工业实践之间的差距。


### CCSW '25: Cloud Computing Security Workshop.
- **作者**: Paolo Palmieri, Shivam Bhasin
- **解读**: 该文介绍第 16 届云计算安全研讨会（CCSW'25，2025 年 10 月 17 日于台北与 CCS 同地举办）：议程包含六篇云环境安全与隐私方向的全文，涵盖基于人工智能的入侵检测测试、面向数据与计算外包的高级密码学、安全编码与网络等主题，并邀请 Tianwei Zhang 博士与 Matthias Kannwischer 作两场主题演讲。


### QSec '25: Workshop on Quantum Security and Privacy.
- **作者**: Muhammad Taqi Raza, Jakub Szefer
- **解读**: 该文介绍量子安全与隐私研讨会（QSec'25）：旨在建立专门探讨量子技术引入的新型威胁以及量子系统自身安全问题的学术阵地。研讨会汇聚后量子密码、网络安全与量子计算（含量子密钥分发架构）等领域专家，分析拥有量子或量子增强能力的对手如何破坏经典系统，并展示混合密码方案、量子原生协议、QKD 与硬件级保护等创新防御手段。此外还致力于搭建密码学、网络、架构与信息科学等社区间的桥梁，并通过主题演讲、技术环节与蓝天空想讨论绘制未来研究路线图。


### QRSec 2025: ACM CCS First Workshop on Quantum-Resistant Cryptography and Security.
- **作者**: Ashish Kundu, Attila A. Yavuz, Cristina Nita-Rotaru
- **解读**: 该文介绍首届 ACM CCS 抗量子密码与安全研讨会（QRSec 2025）：在 NIST PQC 标准化进程于 2024 年发布首批标准的背景下，研讨会为研究者、从业者与产业领袖探讨后量子密码（PQC）向安全系统的集成提供平台。议程覆盖理论基础、算法设计、工程与部署策略，以及机器学习密码分析、抗量子网络、区块链、物联网、云安全等新兴主题，并通过主题演讲、技术环节与互动讨论促进学术界、工业界与政府间的对话，旨在推进该领域现状并为量子时代的迁移与合规绘制可行路径。


### LAMPS '25: ACM CCS Workshop on Large AI Systems and Models with Privacy and Security Analysis.
- **作者**: Kwok-Yan Lam, Xiaoning Liu, Derui Wang, Bo Li, Wenyuan Xu, Jieshan Chen, Minhui Xue, Xingliang Yuan, Guangdong Bai, Shuo Wang
- **解读**: 该文介绍大型 AI 系统与模型隐私安全分析研讨会（LAMPS'25）：随着大型 AI 系统与模型（LAMs）在各行业应用中作用日益突出，其对关键基础设施隐私与网络安全的影响已成为紧迫议题。研讨会汇聚全球专家，深入探讨各行业面临的复杂风险，交流创新解决方案与最佳实践，强调在推进 AI 技术的同时守护数字与物理基础设施安全之间的平衡，并共同塑造未来研究议程。


### DeFi '25: 5th ACM Workshop on Decentralized Finance and Security.
- **作者**: Hao Chung, Yajin Zhou, Liyi Zhou
- **解读**: 该文介绍第 5 届去中心化金融与安全研讨会（DeFi'25）：DeFi 已从利基市场发展为涵盖去中心化交易所、借贷平台、稳定币、衍生品、收益优化、预测市场及代币混币器等隐私增强技术的复杂金融生态，约 770 亿美元的总锁定价值（TVL）在彰显其重要性的同时凸显了强化安全措施的必要性。研讨会汇聚密码学、博弈论、经济学与网络安全专家，通过跨学科对话严格审视现状，预期产出优先研究议程、新的合作倡议与战略路线图。本届议程包括 Vassilis Zikas 教授的主题演讲、两个最佳论文奖（理论与应用方向）得主的邀请报告及四篇原创论文展示。


### The 20th Workshop on Programming Languages and Analysis for Security (PLAS 2025).
- **作者**: Abhishek Bichhawat, Jana Hofmann
- **解读**: 该文介绍第 20 届安全编程语言与分析研讨会（PLAS 2025，首届于 2007 年在圣地亚哥举办）：论坛探讨如何运用编程语言与程序分析技术保障从编译器、机器学习模型到智能合约等各类软件系统的安全，鼓励新颖、前瞻性的想法，对已知或新技术的实践评估，以及对新兴威胁与问题的讨论，并接纳激进、面向未来的立场论文。本届设有 Limin Jia 与 Jan Reineke 两场主题演讲和五场论文报告。


### RICSS'25: 3rd International Workshop on Re-design Industrial Control Systems with Security.
- **作者**: Ruimin Sun, Mu Zhang
- **解读**: 该文介绍第 3 届"以安全重新设计工业控制系统"国际研讨会（RICSS'25）：ICS 及其软件触及工业、学术界与政府所依赖的关键基础设施的方方面面，但这些系统在设计之初并未考虑安全，随着互联扩展与新威胁出现，从业者只能依赖零散补丁式防护。研讨会的目标有二：收集重新设计（部分）ICS 生态、使安全内建于设计的想法；同时征集安全开源软件的设计、集成与维护贡献，弥补商业专有方案之外自由开源方案鲜为人知的空白。


### 3D-Sec '25: The 1st ACM Workshop on Deepfake, Deception, and Disinformation Security.
- **作者**: Simon S. Woo, Shahroz Tariq, Sharif Abuadbba, Kristen Moore, Tim Walita, Bimal Viswanath, Mario Fritz
- **解读**: 该文介绍首届 ACM 深度伪造、欺骗与虚假信息安全研讨会（3D-Sec'25），主题围绕深度伪造的检测与防御、欺骗（deception）识别，以及虚假信息（disinformation）的安全对抗等议题。（无摘要，据标题推断）


### SaTS '25: The 3rd ACM Workshop on Security and Privacy of AI-Empowered Mobile Super Apps.
- **作者**: Luyi Xing, Yue Xiao
- **解读**: 该文介绍第 3 届 AI 赋能移动超级应用安全与隐私研讨会（SaTS'25）：超级应用将微信、支付宝、Grab、TikTok 等平台的支付、消息、电商与娱乐功能集于一身，同时采集处理海量敏感个人数据，功能的高度集中既带来商业机遇也引发显著的安全隐私风险。LLM 集成正将这些应用转变为可编排外部服务、代表用户执行特权任务的智能体系统，进一步扩大攻击面并引入数据暴露、权限滥用与对抗操纵等新问题。研讨会征集安全架构设计、权限框架、威胁建模、隐私保护方法与真实部署案例等贡献，为更安全可信的 LLM 赋能应用（乃至一般应用）奠定基础。


### WAHC 2025: 13th Workshop on Encrypted Computing & Applied Homomorphic Cryptography.
- **作者**: Flávio Bergamaschi, Yuriy Polyakov, Kurt Rohloff
- **解读**: 该文介绍第 13 届加密计算与应用同态密码研讨会（WAHC 2025），主题聚焦同态密码等加密计算技术的进展及其实际应用。（无摘要，据标题推断）


### free5GC '25: The 1st free5GC World Forum.
- **作者**: Jyh-Cheng Chen, K. K. Ramakrishnan
- **解读**: 该文介绍首届 free5GC 世界论坛：面向研究者、业界人士与开源贡献者，探讨 5G 核心网的挑战与最新进展，重点聚焦安全以及由 Linux 基金会托管、完全符合 3GPP 标准的领先独立组网（SA）平台 free5GC 的变革性作用。该平台为开发者与企业提供下一代连接解决方案的快速原型环境，诸如旨在显著降低控制面时延同时保持 3GPP 合规的 L25GC+ 等架构，正体现了其在生产级核心网上开发与测试前沿想法的价值。论坛为交流思想、协作开发并塑造移动网络未来方向提供了契机。


### CSCS '25 - Cyber Security in CarS Workshop.
- **作者**: Hans-Joachim Hof, Mario Fritz, Christoph Krauß
- **解读**: 该文介绍第二届车载网络安全研讨会（CSCS'25，2025 年 10 月 17 日于台北与 CCS'25 同地举办）：作为 2017 至 2023 年年度科学研讨会的延续，会议汇聚研究者、从业者与开发者，共同应对现代车辆面临的复杂网络安全问题。除展示当前研究贡献外，研讨会还提供人脉交流、联合头脑风暴与解决方案开发的平台，讨论车辆技术及其应用的新进展。
