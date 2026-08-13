---
title: NDSS 2026 录用论文深度解读（265 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, NDSS, 2026]
---

NDSS 2026（第 33 届网络与分布式系统安全研讨会）于 2026 年 2 月 23-27 日在加州圣地亚哥举行。本文对 NDSS 2026 全部 265 篇录用论文逐一给出中文深度解读，并附上可获取的论文 PDF 链接。解读基于论文官方摘要，个别无摘要论文仅据标题推断并已标注；PDF 链接来自会议官方站。

## 全部 265 篇录用论文解读

### A Causal Perspective for Enhancing Jailbreak Attack and Defense.
- **作者**: Licheng Pan, Yunsheng Lu, Jiexi Liu, Jialing Tao, Haozhe Feng, Hui Xue, Zhixuan Chu, Kui Ren
- **论文 PDF**: [A Causal Perspective for Enhancing Jailbreak Attack and Defense.](https://www.ndss-symposium.org/wp-content/uploads/2026-f797-paper.pdf)
- **解读**: 针对大语言模型越狱（jailbreak）机制理解不足的问题，本文提出 Causal Analyst 框架，将 LLM 融入数据驱动的因果发现，识别可解释提示特征与越狱发生之间的因果联系。作者构建了包含 7 个 LLM、3.5 万次越狱尝试的数据集，用 37 个人工可读提示特征标注，并联合训练 LLM 提示编码与 GNN 因果图学习以重建因果通路。分析发现如“Positive Character”“Number of Task Steps”等特征是越狱的直接因果驱动，据此实现两个应用：Jailbreaking Enhancer 定向增强攻击成功率，Guardrail Advisor 利用因果图从混淆查询中提取真实恶意意图。


### A Deep Dive into Function Inlining and its Security Implications for ML-based Binary Analysis.
- **作者**: Omar Abusabha, Jiyong Uhm, Tamer Abuhmed, Hyungjoon Koo
- **论文 PDF**: [A Deep Dive into Function Inlining and its Security Implications for ML-based Binary Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1872-paper.pdf)
- **解读**: 函数内联是现代编译器广泛使用的优化，会显著改变机器指令与控制流图等静态特征，但其对基于机器学习的二进制分析的安全影响此前未被系统研究。本文首次以 ML 二进制分析视角全面研究函数内联：剖析 LLVM 成本模型中的内联决策管线，并探索超越标准优化级别、激进提升内联比例的“极端内联”编译选项组合，使用 20 个模型评估 5 类 ML 辅助安全分析任务的鲁棒性。实验表明内联可被利用来构造规避判别式或生成式 ML 模型的恶意二进制变体，基于静态特征的模型对内联高度敏感，且内联比例在不同应用与构建配置间差异巨大，动摇了一致性假设。


### A Hard-Label Black-Box Evasion Attack against ML-based Malicious Traffic Detection Systems.
- **作者**: Zixuan Liu, Yi Zhao, Zhuotao Liu, Qi Li, Chuanpu Fu, Guangmeng Zhou, Ke Xu
- **论文 PDF**: [A Hard-Label Black-Box Evasion Attack against ML-based Malicious Traffic Detection Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s916-paper.pdf)
- **解读**: 面向基于 ML 的恶意流量检测系统，本文提出 NetMasquerade，一种不依赖目标内部细节的硬标签黑盒规避攻击。其核心是预训练模型 Traffic-BERT，利用网络专用分词器与注意力机制提取多样良性流量模式，再整合进强化学习框架，以最小修改将恶意报文序列改造成贴近良性流量。实验表明 NetMasquerade 在 80 个攻击场景下可规避 6 种现有检测方法，成功率超过 96.65%，包括对经验性或可证明鲁棒方法的规避，且能低延迟生成对抗流量。


### A Unified Defense Framework Against Membership Inference in Federated Learning via Distillation and Contribution-Aware Aggregation.
- **作者**: Liwei Zhang, Linghui Li, Xiaotian Si, Ziduo Guo, Xingwu Wang, Kaiguo Yuan, Bingyu Li
- **论文 PDF**: [A Unified Defense Framework Against Membership Inference in Federated Learning via Distillation and Contribution-Aware Aggregation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s413-paper.pdf)
- **解读**: 联邦学习面临被动与主动两类成员推断攻击（MIA），现有防御要么严重损害模型性能，要么无法同时抵御两类攻击。本文提出统一防御框架：在教师模型训练中引入改进的熵正则化增强对成员数据的不确定性；用条件变分自编码器（CVAE）生成类条件合成数据供学生模型监督训练，避免直接暴露敏感数据；并设计贡献感知的聚合策略，按效用调整本地模型的影响力以削弱恶意客户端影响。在四个基准数据集上的实验表明该方法显著降低各类 MIA 成功率，优于现有防御，同时保持较高模型精度。


### Abuse Resistant Traceability with Minimal Trust for Encrypted Messaging Systems.
- **作者**: Zhongming Wang, Tao Xiang, Xiaoguo Li, Guomin Yang, Biwen Chen, Ze Jiang, Jiacheng Wang, Chuan Ma, Robert H. Deng
- **论文 PDF**: [Abuse Resistant Traceability with Minimal Trust for Encrypted Messaging Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f456-paper.pdf)
- **解读**: 端到端加密消息系统阻碍内容审核，溯源（traceability）能力可定位消息来源却可能被滥用为对无辜消息的大规模监控。本文提出抗滥用的来源溯源方案，将溯源能力分散到多个真实世界实体：只要参与溯源的一个参与者诚实，即使其余全部合谋也无法滥用溯源（最小信任），且参与者除溯源必需信息外得不到任何多余信息（最小信息披露）。方案用 Signal 采用的技术实现，评估显示性能与易受滥用的现有最优方案相当。


### Accurate Identification of the Vulnerability-Introducing Commit based on Differential Analysis of Patching Patterns.
- **作者**: Qixuan Guo, Yongzhong He
- **论文 PDF**: [Accurate Identification of the Vulnerability-Introducing Commit based on Differential Analysis of Patching Patterns.](https://www.ndss-symposium.org/wp-content/uploads/2026-s140-paper.pdf)
- **解读**: 漏洞引入提交（VIC）定位对软件安全修复至关重要。本文提出基于漏洞补丁模式差分分析的 VIC 精确识别方法：对比漏洞修复前后的文件，将补丁中与漏洞相关的语句分类为编码错误、数据流不当、语句错位、关键检查缺失等补丁模式，再从漏洞文件中提取漏洞关键语句序列并与早期提交匹配以定位引入提交。在包含 Linux 内核、MySQL、OpenSSL 等开源软件的 6,920 个 CVE 与 5,859,238 个提交的数据集上，方法达到 94.94% 的准确率与 86.92% 的召回率，显著优于现有方法。


### ACE: A Security Architecture for LLM-Integrated App Systems.
- **作者**: Evan Li, Tushin Mallick, Evan Rose, William K. Robertson, Alina Oprea, Cristina Nita-Rotaru
- **论文 PDF**: [ACE: A Security Architecture for LLM-Integrated App Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s352-paper.pdf)
- **解读**: LLM 集成应用系统引入恶意第三方应用的新攻击面，可破坏规划/执行的完整性、可用性或隐私。本文识别出影响规划完整性及执行完整性/可用性的新攻击，并针对现有防御方案 IsolateGPT 演示了这些攻击，随后提出 Abstract-Concrete-Execute (ACE) 安全架构：先用可信信息生成抽象执行计划，再映射为基于系统应用的具体计划，通过静态分析验证计划满足用户指定的安全信息流约束，执行阶段在应用间强制数据与能力隔离。实验表明 ACE 能抵御 InjecAgent 与 Agent Security Bench 的间接提示注入攻击及新攻击，并在 LangChain Tool Usage 基准上展示了可用性。


### Achieving Interpretable DL-based Web Attack Detection through Malicious Payload Localization.
- **作者**: Peiyang Li, Fukun Mei, Ye Wang, Zhuotao Liu, Ke Xu, Chao Shen, Qian Wang, Qi Li
- **论文 PDF**: [Achieving Interpretable DL-based Web Attack Detection through Malicious Payload Localization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1029-paper.pdf)
- **解读**: 深度学习 Web 攻击检测缺乏可解释性，现有解释方法忽略 HTTP 请求的结构信息，仅给出安全运营人员难以理解的特征重要性。本文提出 WebSpotter，通过定位 HTTP 请求中的恶意载荷实现可解释检测：先识别各请求字段对预测的重要性，再用机器学习模型学习重要性分布与恶意载荷的关联，并可自动生成 WAF 规则辅助缓解攻击。在两个公开数据集与新构造数据集上，其定位精度较基线至少提升 22%，并在真实 CVE 与真实 Web 应用上验证了有效性。


### Achieving Zen: Combining Mathematical and Programmatic Deep Learning Model Representations for Attribution and Reuse.
- **作者**: David Oygenblik, Dinko Dermendzhiev, Filippos Sofias, Mingxuan Yao, Haichuan Xu, Runze Zhang, Jeman Park, Amit Kumar Sikder, Brendan Saltaformaggio
- **论文 PDF**: [Achieving Zen: Combining Mathematical and Programmatic Deep Learning Model Representations for Attribution and Reuse.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1628-paper.pdf)
- **解读**: 现有技术可从内存或二进制中提取通用格式的深度学习模型用于安全分析，但忽略了模型可复用与白盒分析所需的程序化表示。本文提出恢复方法论与原型 ZEN，自动恢复 DL 模型的程序化表示，与既有数学表示恢复互补：识别未知 DL 系统相对基模型的新增代码并生成补丁，使恢复的模型可被复用。在 Llama 3、YoloV10 等 21 个语言与视觉领域 SOTA 模型上，ZEN 以 100% 准确率将定制模型归属到其基模型并实现复用。


### Action Required: A Mixed-Methods Study of Security Practices in GitHub Actions.
- **作者**: Yusuke Kubo, Fumihiro Kanei, Mitsuaki Akiyama, Takuro Wakai, Tatsuya Mori
- **论文 PDF**: [Action Required: A Mixed-Methods Study of Security Practices in GitHub Actions.](https://www.ndss-symposium.org/wp-content/uploads/2026-f483-paper.pdf)
- **解读**: GitHub Actions 已成为主流 CI/CD 平台，但供应链攻击（如 SolarWinds、tj-actions/changed-files）暴露其安全弱点，官方安全实践的真实落地情况未知。本文开展混合方法研究，分析 338,812 个公开仓库并调查 100 多位开发者：五项关键安全实践的落地率低至 0.6%–52.9%，主要障碍是缺乏意识（非采用者中高达 71.6% 不知晓）、对适用性的误解及运维成本顾虑；组织所属与近期开发活跃度等仓库特征与更好的实践显著相关。基于实证提出与自动化水平匹配的干预、改进通知设计、平台与 IDE 辅助及文档澄清等建议。


### Actively Understanding the Dynamics and Risks of the Threat Intelligence Ecosystem.
- **作者**: Tillson Galloway, Omar Alrawi, Allen Chang, Athanasios Avgetidis, Manos Antonakakis, Fabian Monrose
- **论文 PDF**: [Actively Understanding the Dynamics and Risks of the Threat Intelligence Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f102-paper.pdf)
- **解读**: 威胁情报（TI）生态系统投入巨大但运行机制与脆弱性缺乏理解。本文提出新型测量框架，通过监控带水印的网络失陷指标（IoC）追踪二进制在生态中的流转，分析提交、提取、共享、阻断各环节。研究发现情报传播几乎总能导向威胁处置，但有选择地共享情报的厂商限制了生态效用，部分“瓶颈”厂商延迟数小时到数天共享情报；还识别出多项正在被利用的供应链威胁（不必要主动探测、掉落文件提取浅层、沙箱指纹易预测），并提出可操作建议、滥用检测特征与面向研究者的伦理最佳实践。


### ACTS: Attestations of Contents in TLS Sessions.
- **作者**: Pierpaolo Della Monica, Ivan Visconti, Andrea Vitaletti, Marco Zecchini
- **论文 PDF**: [ACTS: Attestations of Contents in TLS Sessions.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1861-paper.pdf)
- **解读**: Web3 大规模落地需要让用户在既有系统中证明自己从 TLS 服务器获取了特定数据，但 DECO、TLSNotary 等方案要求验证方运行 ZK-SNARK 等非标准高级密码学，阻碍普及。本文基于 Fuchsbauer-Wolf (Eurocrypt'24) 的谓词盲签名概念提出 ACTS 分布式架构：仍不改动 TLS 服务器，但用户只需验证方软件能校验标准 RSA-PSS 签名即可证明数据持有。作者给出轮数最优的谓词盲签名协议，集成进 DECO 架构，并以 TLS 下载的 PDF 编码为 AES-GCM 密文、由公证服务附加 PADES 签名的用例验证了实际部署可行性。


### ADGFUZZ: Assignment Dependency-Guided Fuzzing for Robotic Vehicles.
- **作者**: Yuncheng Wang, Yaowen Zheng, Puzhuo Liu, Dongliang Fang, Jiaxing Cheng, Dingyi Shi, Limin Sun
- **论文 PDF**: [ADGFUZZ: Assignment Dependency-Guided Fuzzing for Robotic Vehicles.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1014-paper.pdf)
- **解读**: 机器人车辆（RV）控制软件的可配置参数、命令输入与感知数据组合空间巨大，现有模糊测试难以深入探索。本文提出 ADGFuzz，面向 RV 控制软件中赋值语句缺陷的模糊测试框架：静态构造赋值依赖图（ADG）捕获程序内变量间依赖，借助命名相似性将依赖传播到 RV 输入空间形成匹配输入集（MIS），再对 MIS 执行熵感知模糊测试。评估在三种 RV 上发现 87 个独特缺陷（其中 78 个此前未知），已负责任披露，16 个被确认修复。


### AirSnitch: Demystifying and Breaking Client Isolation in Wi-Fi Networks.
- **作者**: Xin'an Zhou, Juefei Pu, Zhutian Liu, Zhiyun Qian, Zhaowei Tan, Srikanth V. Krishnamurthy, Mathy Vanhoef
- **论文 PDF**: [AirSnitch: Demystifying and Breaking Client Isolation in Wi-Fi Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1282-paper.pdf)
- **解读**: Wi-Fi 客户端隔离（client isolation）是非标准化功能，安全保证不明确。本文对其实施结构化安全分析并揭示多类绕过攻击，根因包括：保护广播帧的 Wi-Fi 密钥管理不当可被滥用、隔离往往只在 MAC 或 IP 单层实施、客户端身份在网络栈中的同步薄弱可在网络层绕过。测试中每台路由器和网络都至少受一种攻击影响，作者设计的端到端攻击在存在客户端隔离时仍可实现完整的中间人能力，为 ARP 欺骗之外提供了通用替代方案。


### Aliens Among Us: Observing Private or Reserved IPs on the Public Internet.
- **作者**: Radu Anghel, Carlos Gañán, Qasim Lone, Matthew Luckie, Yury Zhauniarovich
- **论文 PDF**: [Aliens Among Us: Observing Private or Reserved IPs on the Public Internet.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1118-paper.pdf)
- **解读**: 伪造源地址流量（Bogon）是网络卫生差的重要指标，BCP 38/84 等源地址验证建议部署不一致。本文分析 2017–2024 八年 CAIDA Ark traceroute 测量并融合 RIPE RIS 与 RouteViews 的历史 BGP 数据，量化数据平面 Bogon 地址的普遍性：82.69%–97.83% 的 Ark 观测点遇到含 Bogon IP 的路径（以 RFC1918 为主），21.11% 的 traceroute 含 RFC1918 地址；识别出 15,500 多个中转 Bogon 流量的 AS，但仅 11.88% 在过半测量中出现。与 Spoofer/MANRS 对照显示控制面与数据面保证存在显著差距：52.71% 转发 Bogon 源报文的 AS 被分类为不可伪造，说明 SAV 部署不完整或无效。


### An LLM-Driven Fuzzing Framework for Detecting Logic Instruction Bugs in PLCs.
- **作者**: Jiaxing Cheng, Ming Zhou, Haining Wang, Xin Chen, Yuncheng Wang, Yibo Qu, Limin Sun
- **论文 PDF**: [An LLM-Driven Fuzzing Framework for Detecting Logic Instruction Bugs in PLCs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1081-paper.pdf)
- **解读**: PLC 固件中厂商提供的逻辑指令库可能含安全缺陷。本文提出 LogicFuzz，首个专门针对 PLC 固件逻辑指令的模糊测试框架：构造捕获操作语义与指令间依赖的语义依赖图（SDG），配合使能信号机制自动合成指令定制的种子程序，实现真实 PLC 硬件上可控、可复位的模糊测试；通过变异 SDG 丰富指令调用上下文、在有效语义约束下进行覆盖引导的参数变异，并用融合运行日志、状态灯与通信状态的多源 oracle 检测指令级故障。在三大厂商 6 台生产 PLC 上发现 19 个指令级缺陷，其中 4 个是此前未知漏洞。


### Analysis of the Security Design, Engineering, and Implementation of the SecureDNA System.
- **作者**: Alan T. Sherman, Jeremy J. Romanik Romano, Edward Zieglar, Enis Golaszewski, Jonathan D. Fuchs, William E. Byrd
- **论文 PDF**: [Analysis of the Security Design, Engineering, and Implementation of the SecureDNA System.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1138-paper.pdf)
- **解读**: 本文分析 SecureDNA 系统的设计、工程与实现安全性——该系统让 DNA 合成仪对照危险物数据库筛查订单，利用分布式不经意伪随机函数保持订单与数据库机密。作者部分基于源码（v1.0.8）审查了密钥管理、证书基础设施、认证与限速机制，并首次对相互认证、基本请求与豁免处理协议做形式化分析。主要发现：定制相互认证协议 SCEP 实际只实现单向认证，危害数据库与密钥服务器无法确知通信对象；且 TLS 通道内数据库响应缺乏强密码学绑定，攻击者可重放/调换响应。作者提出含强绑定的 SCEP+ 修复，v1.1.0 已采纳，说明安全系统不能只靠密码学。


### Anchors of Trust: A Usability Study on User Awareness, Consent, and Control in Cross-Device Authentication.
- **作者**: Xin Zhang, Xiaohan Zhang, Huijun Zhou, Bo Zhao
- **论文 PDF**: [Anchors of Trust: A Usability Study on User Awareness, Consent, and Control in Cross-Device Authentication.](https://www.ndss-symposium.org/wp-content/uploads/2026-f656-paper.pdf)
- **解读**: 跨设备认证（XDAuth）将认证设备与目标设备解耦，信息不对称使用户难以判断认证请求的合法性。本文以“知情权、同意权、控制权”三项基本用户权利为出发点，考察 27 个覆盖三种典型 XDAuth 方案的主流服务：超过半数服务在认证时不提供任何目标设备信息，并非所有服务强制显式确认，6 个缺少撤销可疑授权的手段。100 名参与者的用户研究显示绝大多数人认为这些权利必不可少，揭示出现实实现与用户期望之间的明显差距。


### ANONYCALL: Enabling Native Private Calling in Mobile Networks.
- **作者**: Hexuan Yu, Chaoyu Zhang, Yang Xiao, Angelos D. Keromytis, Y. Thomas Hou, Wenjing Lou
- **论文 PDF**: [ANONYCALL: Enabling Native Private Calling in Mobile Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1064-paper.pdf)
- **解读**: 移动网络运营商可能泄露或出售用户位置与通信记录等敏感信息，现有匿名接入方案缺少身份透明与位置感知，难以支持呼叫路由、计费等核心功能。本文提出 ANONYCALL，隐私保护的呼叫管理架构：匿名被叫发现功能通过带外认证安全共享临时被叫标识，使主叫无需获取被叫永久信息即可发起原生呼叫；用量计费功能引入匿名且可问责的余额凭证，实现精确计费并防止双花。方案与现有移动网络完全兼容，呼叫建立仅增加不到 200ms 开销，在智能手机与标准呼叫系统上验证了实用性。


### Anota: Identifying Business Logic Vulnerabilities via Annotation-Based Sanitization.
- **作者**: Meng Wang, Philipp Görz, Joschua Schilling, Keno Hassler, Liwei Guo, Thorsten Holz, Ali Abbasi
- **论文 PDF**: [Anota: Identifying Business Logic Vulnerabilities via Annotation-Based Sanitization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f938-paper.pdf)
- **解读**: 业务逻辑漏洞源于应用设计或实现缺陷，传统模糊测试消毒器依赖内存安全检测，难以覆盖需要应用语义上下文的逻辑缺陷（CWE Top 40 中占 27 项）。本文提出 ANOTA，一个人机协作的消毒器框架：轻量级、易用的注解系统让用户把领域知识编码为定义应用预期行为的策略，运行时执行监视器将程序行为与注解策略比对以识别偏离（漏洞）。与 SOTA 模糊器组合后，ANOTA+FUZZER 成功复现 43 个已知漏洞，并发现 22 个此前未知漏洞（获 17 个 CVE）。


### Are your Sites Truly Isolated? Automatically Detecting Logic Bugs in Site Isolation Implementations.
- **作者**: Jan Drescher, David Klein, Martin Johns
- **论文 PDF**: [Are your Sites Truly Isolated? Automatically Detecting Logic Bugs in Site Isolation Implementations.](https://www.ndss-symposium.org/wp-content/uploads/2026-f902-paper.pdf)
- **解读**: 站点隔离（Site Isolation）是现代浏览器的核心安全机制，浏览器进程需跟踪渲染器进程与站点的对应关系，否则受损渲染器可借恶意 IPC 消息攻击其他站点。本文提出首个自动检测 Firefox 与 Chrome 中站点隔离绕过漏洞的方法：新 oracle 通过标记进程级跨站数据泄漏来发现语义缺陷，并设计模拟受损渲染器的模糊器，通过挂钩 IPC 通信利用浏览器进程作为困惑代理（confused deputy）。共发现 4 个安全漏洞：3 个造成跨站数据泄漏，第 4 个可完全控制受害站点。


### Attention is All You Need to Defend Against Indirect Prompt Injection Attacks in LLMs.
- **作者**: Yinan Zhong, Qianhao Miao, Yanjiao Chen, Jiangyi Deng, Yushi Cheng, Wenyuan Xu
- **论文 PDF**: [Attention is All You Need to Defend Against Indirect Prompt Injection Attacks in LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f394-paper.pdf)
- **解读**: LLM 应用易受经不可信外部数据源注入指令的间接提示注入（IPI）攻击。本文提出 Rennervate 防御框架：利用注意力特征在细粒度 token 级别检测隐蔽注入，通过两步注意力池化机制聚合注意力头与响应 token 进行检测与消毒，精确中和 IPI 攻击同时保持 LLM 功能，并开源细粒度数据集 FIPI。在 5 个 LLM 与 6 个数据集上优于 15 种商业与学术防御方法，且可迁移到未见攻击、对自适应对手鲁棒。


### Augmented Shuffle Differential Privacy Protocols for Large-Domain Categorical and Key-Value Data.
- **作者**: Takao Murakami, Yuichi Sei, Reo Eriguchi
- **论文 PDF**: [Augmented Shuffle Differential Privacy Protocols for Large-Domain Categorical and Key-Value Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1124-paper.pdf)
- **解读**: 洗牌差分隐私（Shuffle DP）协议易受数据收集者与用户合谋攻击及数据投毒攻击，现有增强方案仅适用于小定义域类别数据。本文提出 FME（Filtering-with-Multiple-Encryption）协议：用户不加噪，由洗牌器做随机采样与假数据添加，通过哈希函数过滤非流行项并精确统计流行项频率，一轮交互内用多重加密小心组织通信；并附加偏差削减技术扩展到键值（KV）统计。对类别与 KV 数据均证明其提供计算性 DP、对两类攻击的高鲁棒性、准确性与高效性，与 12 个现有协议对比验证有效性。


### Automated Code Annotation with LLMs for Establishing TEE Boundaries.
- **作者**: Varun Gadey, Melanie Gotz, Christoph Sendner, Sampo Sovio, Alexandra Dmitrienko
- **论文 PDF**: [Automated Code Annotation with LLMs for Establishing TEE Boundaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s709-paper.pdf)
- **解读**: TEE（如 Intel SGX、ARM TrustZone）能隔离敏感代码，但确定哪些代码（尤其密码学逻辑）应放入 TEE 需深度人工审查且缺少自动化工具。本文提出 LLM-CAL：基于大语言模型自动标注安全敏感代码区域，用量化 LoRA 微调 Gemma-2B、CodeGemma-2B、LLaMA7B 等基础模型，训练数据为手工标注的 4,000 余个 C 源文件，并将局部上下文特征、全局语义信息与结构元数据编码为紧凑输入序列。在识别敏感/非敏感代码上 F1 达 98.40%、召回 97.50%，并配套可扩展的数据预处理与推理管线，旨在最小化 TCB、优化 TEE 使用。


### Automating Function-Level TARA for Automotive Full-Lifecycle Security.
- **作者**: Yuqiao Yang, Yongzhao Zhang, Wenhao Liu, Jun Li, Pengtao Shi, DingYu Zhong, Jie Yang, Ting Chen, Sheng Cao, Yuntao Ren, Yongyue Wu, Xiaosong Zhang
- **论文 PDF**: [Automating Function-Level TARA for Automotive Full-Lifecycle Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1408-paper.pdf)
- **解读**: 现代车辆复杂性带来严峻网络安全风险，法规强制要求威胁分析与风险评估（TARA），但现有自动化依赖静态威胁库，无法满足行业要求的函数级分析。本文提出 DefenseWeaver，首个利用组件细节与 LLM 自动化函数级 TARA 的系统：从扩展 OpenXSAM++ 格式的系统配置动态生成攻击树与风险评估，用多智能体框架协调专业化 LLM 角色，并结合 LoRA 微调与基于专家 TARA 报告的 RAG。在四个汽车安全项目中识别出 11 条经渗透测试验证的关键攻击路径，可比人工更快生成攻击树（已生成 8,200+），并展示了对无人机与航海系统的跨域适用性。


### BACnet or "BADnet"? On the (In)Security of Implicitly Reserved Fields in BACnet.
- **作者**: Qiguang Zhang, Junzhou Luo, Zhen Ling, Yue Zhang, Chongqing Lei, Christopher Morales, Xinwen Fu
- **解读**: （无摘要，据标题推断）本文针对 BACnet——楼宇自动化与暖通控制系统广泛使用的工业协议——中隐式保留字段的安全性展开研究，标题以 "BADnet" 双关暗示这些通常被忽略的保留字段可能被攻击者利用。研究或揭示保留字段缺乏严格校验所引入的解析歧义与安全风险，并可能提出相应的检测或利用分析。


### Benchmarking and Understanding Safety Risks in AI Character Platforms.
- **作者**: Yiluo Wei, Peixian Zhang, Gareth Tyson
- **论文 PDF**: [Benchmarking and Understanding Safety Risks in AI Character Platforms.](https://www.ndss-symposium.org/wp-content/uploads/2026-f575-paper.pdf)
- **解读**: AI 角色平台让用户与 AI 人格对话，其沉浸性、个性化与技术漏洞带来安全隐忧，但缺乏系统性安全评估。本文开展首个大规模安全研究：用 5,000 个问题、16 个安全类别评估 16 个流行平台，发现平均不安全回复率高达 65.1%，远高于基线平台的 17.7%；安全表现随角色差异显著变化，并与人口属性、性格等角色特征强相关。据此训练的 ML 模型能以 F1 0.81 识别较不安全的角色，可辅助平台改进互动安全、角色搜索/推荐与创建机制。


### Better Safe than Sorry: Uncovering the Insecure Resource Management in App-in-App Cloud Services.
- **作者**: Yizhe Shi, Zhemin Yang, Dingyi Liu, Kangwei Zhong, Jiarun Dai, Min Yang
- **论文 PDF**: [Better Safe than Sorry: Uncovering the Insecure Resource Management in App-in-App Cloud Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s194-paper.pdf)
- **解读**: 在应用内应用（app-in-app）生态中，小程序可访问超级应用提供的云数据库、云存储等敏感云服务，但小程序开发者的资源管理实现存在缺陷，可能暴露敏感资源。本文开展首次系统性研究并实现工具 ICREMiner，结合静态分析与动态探测评估四个超级应用平台上 22,695 个真实小程序的云资源管理安全性：2,815 个（12.40%）受影响，涉及 8,062 个不安全云操作，可导致攻击者窃取敏感用户信息、免费支付等危害，并给出缓解策略与负责任披露。


### Beyond Conventional Triggers: Auto-Contextualized Covert Triggers for Android Logic Bombs.
- **作者**: Ye Wang, Bo Luo, Fengjun Li
- **论文 PDF**: [Beyond Conventional Triggers: Auto-Contextualized Covert Triggers for Android Logic Bombs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f348-paper.pdf)
- **解读**: 现有静态分析、模糊测试与学习式检测大多假设恶意软件的触发条件在语义上显式或与正常逻辑可区分。本文提出 SensorBomb 逻辑炸弹框架，利用自动上下文化的触发器与设备传感器-执行器隐蔽信道：触发器与宿主应用合法传感器使用、执行器行为及功能上下文高度对齐，使恶意行为与正常行为无法区分。SensorBomb 自动分析宿主应用选择上下文兼容的传感器/执行器/敏感操作、构造隐蔽触发信道并动态调整触发模式，可规避静态分析、模糊测试、传感器状态异常检测与用户怀疑，三个原型在真实 APK 上大规模注入实验验证零误报与高可靠性。


### Beyond Jailbreak: Unveiling Risks in LLM Applications Arising from Blurred Capability Boundaries.
- **作者**: Yunyi Zhang, Shibo Cui, Baojun Liu, Jingkai Yu, Min Zhang, Fan Shi, Han Zheng
- **论文 PDF**: [Beyond Jailbreak: Unveiling Risks in LLM Applications Arising from Blurred Capability Boundaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2941-paper.pdf)
- **解读**: LLM 应用生态的能力边界缺乏理解。本文系统分析新开发范式，定义 LLM 应用能力空间，并揭示越狱之外、由模糊能力边界引发的新风险——能力降级与能力升级。作者实现评估框架 LLMApp-Eval：收集 4 个平台的元数据做跨平台生态分析，评估 4 平台 199 个流行应用与 6 个开源 LLM，发现 178 个（89.45%）潜在受影响应用，其中 17 个无需任何对抗改写就直接执行恶意任务；实验还表明提示设计质量与应用鲁棒性正相关。


### Beyond Raw Bytes: Towards Large Malware Language Models.
- **作者**: Luke Kurlandski, Harel Berger, Yin Pan, Matthew Wright
- **论文 PDF**: [Beyond Raw Bytes: Towards Large Malware Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-s103-paper.pdf)
- **解读**: 原始字节级恶意软件分类器能力有限且难以建模长序列，而大语言模型展示了海量自监督预训练的力量。本文迈出构建恶意软件大语言模型（LMLM）的第一步，聚焦数据、模型、预训练与微调等核心问题：用语言建模目标预训练恶意软件分类模型后，在多样化恶意软件分类下游任务上平均提升 1.1%、最高提升 28.6%，表明此类模型有望超越原始字节分类器。


### Beyond RTT: An Adversarially Robust Two-Tiered Approach For Residential Proxy Detection.
- **作者**: Temoor Ali, Shehel Yoosuf, Mouna Rabhi, Mashael Al Sabah, Hao Yun
- **论文 PDF**: [Beyond RTT: An Adversarially Robust Two-Tiered Approach For Residential Proxy Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2086-paper.pdf)
- **解读**: 住宅代理网络被用于欺诈、爬虫等恶意活动，现有检测主要依赖跨层 RTT 差异，但本文证明该方法根本性脆弱：简单流量调度攻击即可把检测召回率从 99% 降至 8%。作者引入捕获网关与中继流量特征的流量分析与流关联特征，并提出 Transformer 架构 CorrTransform 以最大化对抗鲁棒性，形成轻量特征（大规模高效检测）与深度学习（高保证对抗场景）两层检测。基于 Bright Data EarnApp 的 15 个月、900GB、11 万余代理连接数据验证：常规条件下代理设备识别精度/召回 >98%，对抗调度、填充与报文整形等攻击时保持 >92% F1（现有方法完全失效），内容提供商场景直接与代理流量区分 FPR <0.2%。


### BINALIGNER: Aligning Binary Code for Cross-Compilation Environment Diffing.
- **作者**: Yiran Zhu, Tong Tang, Jie Wan, Ziqi Yang, Zhenguang Liu, Lorenzo Cavallaro
- **论文 PDF**: [BINALIGNER: Aligning Binary Code for Cross-Compilation Environment Diffing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s649-paper.pdf)
- **解读**: 二进制比对旨在对齐两个二进制中对应同一源码片段的控制流图，但现有方法基于基本块相似度匹配，跨编译环境场景下效果有限且不灵活。本文提出 BINALIGNER：用条件松弛策略寻找候选子图对以降低漏配/误配，用与指令无关的基本块特征生成子图嵌入以支持灵活跨编译环境比对。在跨版本、跨编译器、跨优化级别、跨架构四类场景的实验中显著优于 SOTA，跨架构及多组合场景 F1 平均高出基线 65%，两个真实漏洞/补丁案例验证了实用性。


### Bit of a Close Talker: A Practical Guide to Serverless Cloud Co-Location Attacks.
- **作者**: Wei Shao, Najmeh Nazari, Behnam Omidi, Setareh Rafatirad, Khaled N. Khasawneh, Houman Homayoun, Chongzhou Fang
- **论文 PDF**: [Bit of a Close Talker: A Practical Guide to Serverless Cloud Co-Location Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1376-paper.pdf)
- **解读**: 无服务器云计算用户面临微架构侧信道攻击，攻击者需利用云调度器实现与受害者的物理共置。本文给出在无服务器云中构造共置攻击的完整方法论：揭示调度算法中可利用的特性，并设计通过正常用户接口构造共置攻击的策略，在主流开源基础设施与 Microsoft Azure Functions 上成功实现实例共置；同时提出 Double-Dip 调度器作为缓解方案，为无服务器调度安全增强指明方向。


### BKPIR: Keyword PIR for Private Boolean Retrieval.
- **作者**: Jie Song, Zhen Xu, Yan Zhang, Pengwei Zhan, Mingxuan Li, Shuai Ma, Ru Xie
- **论文 PDF**: [BKPIR: Keyword PIR for Private Boolean Retrieval.](https://www.ndss-symposium.org/wp-content/uploads/2026-s536-paper.pdf)
- **解读**: 关键字私密信息检索（Keyword PIR）让用户私密检索与关键字关联的数据，但现有方案难以支持实用应用所需的布尔检索模型。本文提出新型关键字 PIR 方案，利用同态等值运算实现多对多关键字-值映射数据库上的隐私保护检索并支持布尔运算符，且保持经典 PIR 的核心安全保证，是首个将关键字 PIR 与布尔检索模型结合的工作。实验显示通信成本按数据库值总数成比例降低，聚合查询处理性能随值数量线性扩展，适用于隐私保护网页搜索与专利检索等场景。


### Bleeding Pathways: Vanishing Discriminability in LLM Hidden States Fuels Jailbreak Attacks.
- **作者**: Yingjie Zhang, Tong Liu, Zhe Zhao, Guozhu Meng, Kai Chen
- **论文 PDF**: [Bleeding Pathways: Vanishing Discriminability in LLM Hidden States Fuels Jailbreak Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f4-paper.pdf)
- **解读**: 现有 LLM 安全微调难以兼顾安全与效用，且常漏掉隐藏在看似良性任务中的恶意意图。本文揭示其根本原因：响应生成过程中，模型区分有害与安全输出的能力会退化——安全与有害响应的隐藏状态可分性随生成推进而下降，迫使模型更早做出合规判断。据此提出 DEEPALIGN 内在防御框架：在响应生成中点施加对比隐藏状态引导，放大有害与良性状态间距，实现全程持续的毒性检测与干预，同时扩展安全响应空间。评估显示它把 9 种越狱攻击的成功率降至近零，拒绝良性查询的错误率最高降低 3.5%，标准任务性能下降不足 1%。


### BLERP: BLE Re-Pairing Attacks and Defenses.
- **作者**: Tommaso Sacchetti, Daniele Antonioli
- **论文 PDF**: [BLERP: BLE Re-Pairing Attacks and Defenses.](https://www.ndss-symposium.org/wp-content/uploads/2026-f121-paper.pdf)
- **解读**: BLE 标准允许已配对设备重新配对（re-pairing）以协商新安全级别，但该机制的安全影响此前未研究。本文分析 v6.1 规范定义的 BLE 重配对，识别 6 个设计漏洞（含 4 个新漏洞，如未认证重配对与安全级别降级），这些设计缺陷影响所有符合规范且使用配对的设备；并提出 4 个新攻击 BLERP，可实现 1 次或 0 次用户交互的配对设备冒充与中间人攻击。作者开发低成本（nRF52）开源工具包，在 22 个目标（15 个 BLE Host、12 个 Controller、最高 5.4 版本、最安全配置）上大规模评估，还发现 Apple、Android、NimBLE 协议栈的实现缺陷，并提出向后兼容加固与经 ProVerif 形式化验证的认证重配对协议两种缓解。


### Breaking Isolation: A New Perspective on Hypervisor Exploitation via Cross-Domain Attacks.
- **作者**: Gaoning Pan, Yiming Tao, Qinying Wang, Chunming Wu, Mingde Hu, Yizhi Ren, Shouling Ji
- **论文 PDF**: [Breaking Isolation: A New Perspective on Hypervisor Exploitation via Cross-Domain Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f376-paper.pdf)
- **解读**: 虚拟机监控器面临指针破坏等内存安全漏洞威胁，现有利用框架依赖主机中高度受限的结构与精确运行时地址，在超虚拟化环境中难以奏效。本文观察到现代虚拟化环境内存隔离薄弱——guest 内存完全受攻击者控制却可从宿主访问，据此首次系统化刻画并分类跨域攻击（CDA），即通过 guest 内存复用实现能力提升的利用技术，并开发自动化系统识别跨域 gadget、与损坏指针匹配、合成触发输入并组装完整利用链。在 QEMU 与 VirtualBox 的 15 个真实漏洞上验证了 CDA 的广泛适用性与有效性。


### Breaking the Bulkhead: Demystifying Cross-Namespace Reference Vulnerabilities in Kubernetes Operators.
- **作者**: Andong Chen, Ziyi Guo, Zhaoxuan Jin, Zhenyuan Li, Yan Chen
- **论文 PDF**: [Breaking the Bulkhead: Demystifying Cross-Namespace Reference Vulnerabilities in Kubernetes Operators.](https://www.ndss-symposium.org/wp-content/uploads/2026-f761-paper.pdf)
- **解读**: Kubernetes Operator 常要求高权限并跨命名空间操作资源，与 Kubernetes 命名空间隔离机制产生冲突，形成跨命名空间引用漏洞（Cross-Namespace Reference Vulnerability）：声明资源范围与 Operator 逻辑实现范围不匹配，使集群无法正确隔离命名空间。仅具单一命名空间受限访问的攻击者可借 Operator 影响其他未授权命名空间，造成权限提升。本文首次系统研究 Operator 攻击，大规模测量发现超过 14% 的在野 Operator 潜在受影响，已报告开发者并获 8 项确认与 7 个 CVE（涉及 Google、Red Hat），并开源静态分析套件与缓解方案。


### Breaking the Generative Steganography Trilemma: ANStega for Optimal Capacity, Efficiency, and Security.
- **作者**: Yaofei Wang, Weilong Pang, Kejiang Chen, Jinyang Ding, Donghui Hu, Weiming Zhang, Nenghai Yu
- **论文 PDF**: [Breaking the Generative Steganography Trilemma: ANStega for Optimal Capacity, Efficiency, and Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-f605-paper.pdf)
- **解读**: 生成式隐写面临容量、效率与安全的三难困境：Huffman 编码方法效率与安全性差，算术编码（AC）虽容量最优但存在安全风险，可证明安全的方法又牺牲容量或复杂度。本文改编非对称数系（ANS）用于隐写，核心洞察是复用 ANS 状态机——解码函数嵌入、编码函数提取：采用带状态重归一化的流式架构支持任意长消息稳定嵌入；直接浮点运算避免概率到频率转换的复杂度与精度损失；创新加密掩码机制使采样由密码学安全伪随机数发生器驱动以获得可证明安全；核心计算优化为高效位位移运算。实验验证 ANStega 同时达到最优嵌入容量、O(1) 嵌入复杂度的最优效率与最优安全性。


### BSFuzzer: Context-Aware Semantic Fuzzing for BLE Logic Flaw Detection.
- **作者**: Ting Yang, Yue Qin, Lan Zhang, Zhiyuan Fu, Junfan Chen, Jice Wang, Shangru Zhao, Qi Li, Ruidong Li, He Wang, Yuqing Zhang
- **论文 PDF**: [BSFuzzer: Context-Aware Semantic Fuzzing for BLE Logic Flaw Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f94-paper.pdf)
- **解读**: BLE 的复杂设计引入字段误读、非法状态转换等微妙逻辑缺陷，可导致认证绕过、未授权控制或 DoS，常规模糊测试与形式分析难以发现。本文提出 BSFuzzer，由蓝牙核心规范引导的黑盒上下文感知语义模糊框架：LLM 智能体从规范文本、图示与上下文中解析状态机与报文语义，生成字段级规则违反与状态级关键转换扰动两类变异，组合为结构化测试序列执行，并用 LLM 验证响应是否符合预期。在 19 台真实 BLE 设备（9 个 SoC 模块、10 部手机）上发现 36 个安全问题（34 个此前未知、9 个获 CVE），规范解析精度最高 97%、响应验证最高 85.8%，代码覆盖率比 4 个 SOTA 工具高 9.34%。


### Bullseye: Detecting Prototype Pollution in NPM Packages with Proof of Concept Exploits.
- **作者**: Tariq Houis, Shaoqi Jiang, Mohammad Mannan, Amr Youssef
- **论文 PDF**: [Bullseye: Detecting Prototype Pollution in NPM Packages with Proof of Concept Exploits.](https://www.ndss-symposium.org/wp-content/uploads/2026-s211-paper.pdf)
- **解读**: 原型污染是 JavaScript/Node.js 的严重漏洞，静态方法误报高且扩展性差，动态方法因可达性低而漏报高。本文提出 Bullseye，全自动动态分析框架：结合广泛入口覆盖、上下文感知的利用生成与双重运行时验证 oracle，使用包测试套件的开发者输入及既有研究中的污染利用输入，逐入口执行并观察运行时污染迹象。在 8 小时内分析 44,513 个高流行（周下载 1 万+）与 5,879 个较低流行包，在 290 个包中发现零日原型污染且零误报，获 149 个 CVE（66 个公开，25 个严重、34 个高危），并附 PoC 负责任披露。


### BunnyFinder: Finding Incentive Flaws for Ethereum Consensus.
- **作者**: Rujia Li, Mingfei Zhang, Xueqian Lu, Wenbo Xu, Ying Yan, Sisi Duan
- **论文 PDF**: [BunnyFinder: Finding Incentive Flaws for Ethereum Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-s281-paper.pdf)
- **解读**: 以太坊共识依赖激励机制维持稳定性，针对激励机制的（重组等）攻击发现高度依赖专家知识与手工劳动。本文提出 BunnyFinder，受软件测试故障注入启发的半自动化激励缺陷发现框架：精心设计的“策略生成器”生成大量攻击实例，自动化工作流发动攻击并分析结果，并用强化学习微调攻击参数以识别最有利可图的攻击。共模拟 9,354 个攻击实例：复现 5 个此前人工发现的已知激励攻击，发现 3 个新激励缺陷，并意外发现 2 个实现缺陷。


### Cache Me, Catch You: Cache Related Security Threats in LLM Serving Frameworks.
- **作者**: XiangFan Wu, Lingyun Ying, Guoqiang Chen, Yacong Gu, Haipeng Qu
- **论文 PDF**: [Cache Me, Catch You: Cache Related Security Threats in LLM Serving Frameworks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2812-paper.pdf)
- **解读**: LLM 推理性能高度依赖前缀缓存、语义缓存等机制，但这些机制引入新的攻击面。本文首次系统研究 LLM 推理期缓存相关安全风险：对主流服务框架的缓存实现进行系统研究，识别出 6 个新攻击向量，分为面向用户的欺诈攻击（经前缀缓存碰撞与语义模糊投毒操控缓存条目向用户投递恶意内容）与系统完整性攻击（利用块级或多模态碰撞绕过内容审核等安全检查）。实验在 vLLM、SGLang、GPTCache、AIBrix、rtp-llm、LMDeploy 等主流开源框架上验证，提出五层防御策略；vLLM、GPTCache、AIBrix 已采纳修复。


### Cascading and Proxy Membership Inference Attacks.
- **作者**: Yuntao Du, Jiacheng Li, Yuetian Chen, Kaiyuan Zhang, Zhizhen Yuan, Hanshen Xiao, Bruno Ribeiro, Ninghui Li
- **论文 PDF**: [Cascading and Proxy Membership Inference Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s661-paper.pdf)
- **解读**: 成员推断攻击（MIA）衡量模型泄露训练数据成员信息的程度。本文将现有 MIA 分为自适应与非自适应：自适应场景（攻击者可基于查询训练影子模型）提出攻击无关框架 CMIA（级联 MIA），通过条件影子训练利用实例间的成员依赖提升推断性能；非自适应场景（限制在查询前训练影子模型）提出 PMIA（代理 MIA），选择与查询行为相似的样本并用其在影子模型中的行为做成员后验比值检验。两项工作均给出理论分析，实验表明在低假阳性区（隐私风险评估关键区）显著优于现有 MIA。


### CAT: Can Trust be Predicted with Context-Awareness in Dynamic Heterogeneous Networks?
- **作者**: Jie Wang, Zheng Yan, Jiahe Lan, Xuyan Li, Elisa Bertino
- **论文 PDF**: [CAT: Can Trust be Predicted with Context-Awareness in Dynamic Heterogeneous Networks?](https://www.ndss-symposium.org/wp-content/uploads/2026-f2171-paper.pdf)
- **解读**: 现有基于 GNN 的信任预测模型无法捕获信任动态性、忽略网络异质性、缺乏上下文感知，导致推断粗糙。本文提出 CAT，首个支持动态性与真实异质性的上下文感知 GNN 信任预测模型：由构图层、嵌入层、异质注意力层与预测层构成，用连续时间表示与时间编码处理动态图，双注意力机制建模节点类型与类型内节点的重要性，并引入新元路径概念提取上下文特征，通过上下文嵌入与上下文感知聚合器预测上下文感知信任与总体信任。在三个真实数据集上优于五组基线，且对大规模图可扩展、对信任导向与 GNN 导向攻击鲁棒。


### CatBack: Universal Backdoor Attacks on Tabular Data via Categorical Encoding.
- **作者**: Behrad Tajalli, Stefanos Koffas, Stjepan Picek
- **论文 PDF**: [CatBack: Universal Backdoor Attacks on Tabular Data via Categorical Encoding.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1469-paper.pdf)
- **解读**: 机器学习后门攻击研究多集中于图像等同质数据，表格数据因数值与类别特征并存更具挑战。本文提出 CatBack：将类别值转换为浮点表示（相比 one-hot 或序数编码保留更多信息、维持干净模型精度），从而构造作用于包括类别特征在内全部特征的基于梯度的通用扰动。在五个数据集与四个流行模型上，白盒与黑盒（含 Vertex AI 等真实应用）攻击成功率最高达 100%，超越 Tabdoor，并能规避 Spectral Signatures、Neural Cleanse、Beatrix、Fine-Pruning 及常见离群点检测机制。


### Causal-Guided Detoxify Backdoor Attack of Open-Weight LoRA Models.
- **作者**: Linzhi Chen, Yang Sun, Hongru Wei, Yuqi Chen
- **论文 PDF**: [Causal-Guided Detoxify Backdoor Attack of Open-Weight LoRA Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f168-paper.pdf)
- **解读**: LoRA 微调高效且被开源社区广泛采用，但 Hugging Face 等平台去中心化分发恶意适配器带来新漏洞，现有后门攻击策略依赖不可得训练数据、忽视 LoRA 结构特性或误触发率高。本文提出 CBA（因果引导解毒后门攻击）框架，专为开源权重 LoRA 模型设计且无需原始训练数据：覆盖引导的数据生成管线通过行为探索合成任务对齐输入，因果引导的解毒策略通过保留任务关键神经元合并投毒与干净适配器，并支持训练后按因果影响权重分配控制攻击强度。在 6 个 LoRA 模型上保持高攻击成功率的同时将误触发率降低 50–70%，且对 SOTA 后门防御有更强抵抗力。


### Cease at the Ultimate Goodness: Towards Efficient Website Fingerprinting Defense via Iterative Mutual Information Minimization.
- **作者**: Rong Wang, Zhen Ling, Guangchi Liu, Shaofeng Li, Junzhou Luo, Xinwen Fu
- **论文 PDF**: [Cease at the Ultimate Goodness: Towards Efficient Website Fingerprinting Defense via Iterative Mutual Information Minimization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f786-paper.pdf)
- **解读**: Tor 的匿名性面临网站指纹攻击（WFA）挑战。本文提出 FRUGAL 流量混淆防御，以网站流量与标签间互信息（MI）最小化为优化目标：在累计 MI 削减贡献最大的位置注入哑包。闭世界设置下以 30% 带宽开销（BWO）将 DF 模型攻击成功率降至 2.68%（对比 Palette 的 11.54%/87% BWO），BWO 提至 80% 时 ASR 低于 1%，且对抗训练后仍保持 9.42%（Palette 为 20.27%），在闭世界、开世界与真实世界模拟中均验证有效。


### CELLSHIFT: RTT-Aware Trace Transduction for Real-World Website Fingerprinting.
- **作者**: Rob Jansen
- **论文 PDF**: [CELLSHIFT: RTT-Aware Trace Transduction for Real-World Website Fingerprinting.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1004-paper.pdf)
- **解读**: 网站指纹（WF）攻击的评估常使用 Tor 出口中继可测得的“真实”用户交互痕迹，但这些痕迹不能准确反映入口侧攻击者观察到的模式。本文提出把出口痕迹转导为入口痕迹的新方法：利用痕迹时间戳与元数据提取多个往返时间估计，将痕迹“平移”到目标观察点视角。广泛评估表明该方法在多个合成与真实数据集上优于 SOTA 且效率显著更高，能更准确刻画入口侧 WF 对手面临的真实挑战，生成的增强数据集还可提升现有 WF 攻击性能。


### CHAMELEOSCAN: Demystifying and Detecting iOS Chameleon Apps via LLM-Powered UI Exploration.
- **作者**: Hongyu Lin, Yicheng Hu, Haitao Xu, Yanchen Lu, Mengxia Ren, Shuai Hao, Chuan Yue, Zhao Li, Fan Zhang, Yixin Jiang
- **论文 PDF**: [CHAMELEOSCAN: Demystifying and Detecting iOS Chameleon Apps via LLM-Powered UI Exploration.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1906-paper.pdf)
- **解读**: 变色龙应用（Chameleon app）在提交审核时呈现合法功能、安装后变身为非法变体，现有静态分析与元数据依赖的检测方法对混合实现、新变体或元数据稀缺实例无效。本文建立经隐蔽分发渠道收集的 500 个 iOS 变色龙应用数据集，系统识别出 10 类转换模式（含 4 个此前未记录的变体），并提出 ChameleoScan——首个 LLM 驱动的自动化 UI 探索验证框架，结合预测性元数据分析、语义界面理解与类人交互策略，在保持局部决策可解释的同时保证全局检测一致性。在 1,644 个 iOS 应用上达到 9.85% 检出率与 92.59% 精确率，发现已被 Apple 正式确认。


### Character-Level Perturbations Disrupt LLM Watermarks.
- **作者**: Zhaoxi Zhang, Xiaomei Zhang, Yanjun Zhang, He Zhang, Shirui Pan, Bo Liu, Asif Gill, Leo Yu Zhang
- **论文 PDF**: [Character-Level Perturbations Disrupt LLM Watermarks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s138-paper.pdf)
- **解读**: LLM 水印用于版权保护与机器生成内容检测，现有鲁棒性研究采用的移除攻击是次优的，造成“移除需大扰动预算或强对手能力”的误解。本文形式化 LLM 水印系统模型，刻画两种受限访问水印检测器的现实威胁模型，分析各类扰动的攻击范围：字符级扰动（错别字、交换、删除、同形字）通过破坏分词过程可单次编辑影响多个 token，在最强限制威胁模型下远比 token 级或句子级方法有效；进一步提出基于遗传算法、以参考检测器优化的引导移除攻击。在 5 种水印方案与 2 个 LLM 上验证其优越性，并提出可击破任何固定防御的自适应复合字符级攻击，凸显现有水印方案的脆弱性。


### Characterizing the Implementation of Censorship Policies in Chinese LLM Services.
- **作者**: Anna Ablove, Shreyas Chandrashekaran, Xiao Qiang, Roya Ensafi
- **论文 PDF**: [Characterizing the Implementation of Censorship Policies in Chinese LLM Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1761-paper.pdf)
- **解读**: 中国互联网审查随技术演进不断变化，LLM 服务也需遵守严格法定审查标准，现有对齐研究不足以保证合规。本文首次研究嵌入中国 LLM 服务的显式阻断：利用活跃聊天会话中服务器与客户端通信的信息泄漏，定位阻断决策在 LLM 服务工作流中的嵌入位置。研究发现百度文心、DeepSeek、豆包、Kimi、通义千问等主流服务持续依赖传统过时阻断策略，阻断发生在输入、输出与搜索三个阶段，其中后两者向客户端机器泄漏不同数量的被审查信息（包括近乎完整的响应与浏览器未渲染的搜索引用）。


### Chasing Shadows: Pitfalls in LLM Security Research.
- **作者**: Jonathan Evertz, Niklas Risse, Nicolai Neuer, Andreas Müller, Philipp Normann, Gaetano Sapia, Srishti Gupta, David Pape, Soumya Shaw, Devansh Srivastav, Christian Wressnegger, Erwin Quiring, Thorsten Eisenhofer, Daniel Arp, Lea Schönherr
- **论文 PDF**: [Chasing Shadows: Pitfalls in LLM Security Research.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1749-paper.pdf)
- **解读**: LLM 在安全研究中日益普遍，但其独有特性破坏可复现性、严谨性与评估的既有范式，此前识别传统 ML 研究常见陷阱的工作早于 LLM 时代。本文识别 9 个随 LLM 出现而变得（更）相关的常见陷阱，覆盖数据收集、预训练、微调、提示与评估全流程，并评估 2023–2024 年顶级安全与软件工程会议 72 篇同行评审论文：每篇至少含一个陷阱，每个陷阱出现在多篇论文中，但仅 15.7% 被显式讨论。4 个实证案例研究展示陷阱如何误导评估、夸大性能或损害可复现性，并提供可操作指南。


### Chimera: Harnessing Multi-Agent LLMs for Automatic Insider Threat Simulation.
- **作者**: Jiongchi Yu, Xiaofei Xie, Qiang Hu, Yuhan Ma, Ziming Zhao
- **论文 PDF**: [Chimera: Harnessing Multi-Agent LLMs for Automatic Insider Threat Simulation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f375-paper.pdf)
- **解读**: 内部威胁检测（ITD）受限于缺乏高质量真实训练数据：企业内部数据敏感罕见，公开数据集规模小，合成数据集泛化差、缺乏语义与真实行为模式。本文提出 Chimera，基于 LLM 的多智能体框架自动模拟良性/恶意内部活动并监控多类系统日志：每个智能体建模为具有细粒度角色的员工，包含小组会议、两两交互与自组织调度以捕捉真实组织动态；基于真实事件抽象出的 15 类内部攻击类型，在三个数据敏感组织场景部署并构造数据集 ChimeraLog。人类研究与定量分析验证其多样性与真实性：ITD 方法在 ChimeraLog 上性能显著低于现有数据集，更具挑战性，且在其上训练的模型泛化能力强。


### Cirrus: Performant and Accountable Distributed SNARK.
- **作者**: Wenhao Wang, Fangyan Shi, Dani Vilardell, Fan Zhang
- **论文 PDF**: [Cirrus: Performant and Accountable Distributed SNARK.](https://www.ndss-symposium.org/wp-content/uploads/2026-f668-paper.pdf)
- **解读**: SNARK 能高效验证计算，但大规模任务证明生成昂贵。本文提出 Cirrus，首个同时具备三项理想性质的分布式 SNARK 生成协议：水平可扩展低开销（每 worker 线性计算、对数通信）、可问责（高效检测恶意 worker）与及电路与 worker 数无关的通用可信设置。协议构建于 HyperPlonk (EUROCRYPT'23)，提出高效问责协议定位恶意 worker 与分层聚合技术削减协调者负载；33M 门电路在 32 台 8 核机器上 40 秒内出证明，PLONK 友好电路（Pedersen 哈希）比 SOTA 可问责协议 Hekaton (CCS'24) 快 7 倍以上，4 秒内定位故障 worker。


### CoLD: Collaborative Label Denoising Framework for Network Intrusion Detection.
- **作者**: Shuo Yang, Xinran Zheng, Jinze Li, Jinfeng Xu, Edith C. H. Ngai
- **论文 PDF**: [CoLD: Collaborative Label Denoising Framework for Network Intrusion Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1950-paper.pdf)
- **解读**: 标签噪声显著损害网络入侵检测性能，现有方法缺乏对网络流量的深入洞察，盲目重建标签分布过滤噪声样本。本文从因果关联视角揭示噪声标签的影响机制，将性能退化归因于网络流量中特征的跨类别局部一致性，据此提出 CoLD 协作标签去噪框架：将原始特征集划分为多个子集，用局部联合学习破坏局部一致性以迫使编码器学习细粒度鲁棒表示；再用因果协作去噪，分析多表示与其潜在真实标签间的因果分歧来检测并过滤噪声标签。多个基准数据集上显著提升分类性能与噪声鲁棒性。


### Connecting the Dots: An Investigative Study on Linking Private User Data Across Messaging Apps.
- **作者**: Junkyu Kang, Soyoung Lee, Yonghwi Kwon, Sooel Son
- **论文 PDF**: [Connecting the Dots: An Investigative Study on Linking Private User Data Across Messaging Apps.](https://www.ndss-symposium.org/wp-content/uploads/2026-s556-paper.pdf)
- **解读**: 移动消息应用提供的附近用户搜索、联系人发现、SSO 账户关联等功能带来跨应用链接私人信息的隐私风险。本文深入分析韩国广泛使用的 KakaoTalk、Telegram、WhatsApp、Signal、Tinder 的隐私威胁，演示利用联系人发现、SSO 账户关联与附近用户搜索的具体攻击，并将攻击串联实现首次跨平台链接攻击：攻击者可去匿名化用户名并以平均 324 米误差推断大量定向与非定向用户的地理位置。宽松的联系人发现策略（手机号与头像作链接密钥）是核心弱点，作者提出缓解策略。


### Consensus in the Known Participation Model with Byzantine Faults and Sleepy Replicas.
- **作者**: Chenxu Wang, Sisi Duan, Minghui Xu, Feng Li, Xiuzhen Cheng
- **论文 PDF**: [Consensus in the Known Participation Model with Byzantine Faults and Sleepy Replicas.](https://www.ndss-symposium.org/wp-content/uploads/2026-s448-paper.pdf)
- **解读**: 本文研究已知参与模型下同时存在拜占庭故障与休眠副本（honest replicas 可能不可预测地休眠，副本知晓最小清醒诚实副本数）的共识，提供该混合故障模型的细粒度处理：首先给出同步原子广播协议，期望延迟 5Δ+2δ、最优情况延迟 2Δ+2δ（Δ 为网络延迟上界，δ 为实际延迟）；部分同步（Δ 未知）下，传统 BFT 协议可容忍休眠副本但需稳定存储假设；不假设稳定存储时推导出副本总数 n、拜占庭数 f 与同时休眠数 s 的多组界，并据此把 HotStuff (PODC'19) 改造为容忍休眠副本且不牺牲性能的协议。


### Constructive Noise Defeats Adversarial Noise: Adversarial Example Detection for Commercial DNN Services.
- **作者**: Meng Shen, Jiangyuan Bi, Hao Yu, Zhenming Bai, Wei Wang, Liehuang Zhu
- **论文 PDF**: [Constructive Noise Defeats Adversarial Noise: Adversarial Example Detection for Commercial DNN Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s250-paper.pdf)
- **解读**: 商业 DNN 服务（MLaaS）的对抗样本检测方法通常需要模型细节或训练数据，实际场景不可得，检测精度显著下降。本文提出第三方提供的 Falcon 检测方法：基于干净样本与对抗样本噪声容忍度差异，构造加到干净样本不影响输出标签、加到对抗样本却引起输出明显变化的建设性噪声，为每个输入生成特定分布与强度的噪声，通过加噪前后目标模型输出的差异实现检测。在 4 个公开数据集上检测 10 种典型攻击，TPR 最高、FPR 最低，在 6 个知名商业 DNN 服务上 TPR 约 80%、FPR 5%，且对手完全掌握检测细节时仍保持准确率。


### Continuous User Behavior Monitoring using DNS Cache Timing Attacks.
- **作者**: Hannes Weissteiner, Roland Czerny, Simone Franza, Stefan Gast, Johanna Ullrich, Daniel Gruss
- **论文 PDF**: [Continuous User Behavior Monitoring using DNS Cache Timing Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2287-paper.pdf)
- **解读**: 本地 DNS 缓存可被时间侧信道攻击利用，但此前攻击需受害系统上的代码执行且缺乏驱逐能力。本文提出 DMT，一种通过系统级本地 DNS 缓存持续监控受害者上网行为的 Evict+Reload 攻击，基础是可靠的缓存驱逐：给出 4 种 DNS 缓存驱逐技术，覆盖无特权/沙箱原生攻击、虚拟化跨 VM 攻击、浏览器 JavaScript 攻击及利用字体串行加载的无脚本攻击，且默认设置与 DoT、DNSSEC、非默认转发器下均有效。最快驱逐原语平均驱逐时间 77.267ms；跨 VM 场景 100 域名重载测量平均 685.86ms，五分钟粒度盲区小于 0.26%（最差 4.92%）；端到端跨 VM 攻击对 103 个网站的开世界检测 F1 达 92.48%，JavaScript 攻击对 10 个网站 F1 为 82.86%/78.89%（有无 DNSSEC）。


### Convergent Privacy Framework for Multi-layer GNNs through Contractive Message Passing.
- **作者**: Yu Zheng, Chenang Li, Zhou Li, Qingsong Wang
- **论文 PDF**: [Convergent Privacy Framework for Multi-layer GNNs through Contractive Message Passing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f255-paper.pdf)
- **解读**: 差分隐私（DP）已用于图神经网络（GNN）保护敏感结构信息，扰动消息传递的现有方法隐私成本随层数线性增长（如 GAP），多层 GNN 下噪声过大。本文理论上证明：将隐私放大技术应用于消息传递并利用标准 GNN 操作的收缩（contractive）性质，隐私预算随层数收敛，并提出简单有效的收缩图层（CGL）保证理论所需的收缩性同时保持模型效用。框架 CARIBOU 支持训练与推理，含收缩聚合、隐私分配与隐私审计模块，实验显著改善隐私-效用权衡并在隐私审计任务中表现更优。


### CoordMail: Exploiting SMTP Timeout and Command Interaction to Coordinate Email Middleware for Convergence Amplification Attack.
- **作者**: Ruixuan Li, Chaoyi Lu, Baojun Liu, Yanzhong Lin, Qingfeng Pan, Jun Shao
- **论文 PDF**: [CoordMail: Exploiting SMTP Timeout and Command Interaction to Coordinate Email Middleware for Convergence Amplification Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1414-paper.pdf)
- **解读**: 传统邮件 DoS 攻击主要向目标邮箱发垃圾邮件，难以影响邮件服务器运行。本文提出 COORDMAIL 邮件汇聚放大攻击：利用 SMTP 协议长会话超时与客户端可控交互的特性，协调各邮件中间件的反射邮件同时涌向目标收件服务器，汇聚各中间件放大能力形成高度放大流量。作者从 SMTP 会话状态机与邮件反射行为识别出 10,079 个弹跳服务器、584 个开放邮件中继、6 个邮件转发服务商等真实中间件，用 SMTP 命令序列以极低速率维持长时间通信并任意时刻控制其反射；1,000 条 SMTP 连接可实现 30,000 倍以上带宽放大，并提出可削减放大数十倍的缓解措施。


### CoT-DPG: A Co-Training based Dynamic Password Guessing Method.
- **作者**: Chenyang Wang, Fan Shi, Min Zhang, Chengxi Xu, Miao Hu, Pengfei Xue, Shasha Guo, Jinghua Zheng
- **论文 PDF**: [CoT-DPG: A Co-Training based Dynamic Password Guessing Method.](https://www.ndss-symposium.org/wp-content/uploads/2026-s755-paper.pdf)
- **解读**: 动态口令猜测持续收集目标信息并动态拟合分布，扩大威胁，但现有方法从单一视角拟合目标分布，忽视多维信息互补。本文提出 CoT-DPG，首个将多视图学习共训练方法应用于口令猜测的框架，让多个猜测模型协作学习、互补知识：特征层面基于增量训练动态更新神经网络参数并拟合目标分布；字符层面设计策略分布优化缓解策略选择盲目性；用共训练实现多维互补学习、迭代训练与口令生成。在 8 个真实口令数据集上破解率较 SOTA 方法绝对提升 6.4%–26.7%。


### Crack in the Armor: Underlying Infrastructure Threats to RPKI Publication Point Reachability.
- **作者**: Yunhao Liu, Jessie Hui Wang, Yuedong Xu, Zongpeng Li, Yangyang Wang, Jilong Wang
- **论文 PDF**: [Crack in the Armor: Underlying Infrastructure Threats to RPKI Publication Point Reachability.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1141-paper.pdf)
- **解读**: RPKI 防劫持效果不仅依赖有效 ROA，还依赖依赖方（RP）从发布点（PP）成功检索 ROA，底层 DNS 与路由基础设施的安全措施至关重要。本文收集检索过程所用 DNS 与路由基础设施信息并分析威胁：DNS 方面 31 个 PP（48.4%）易受 DNS 欺骗（CNAME 重定向到未保护区域、NS 委托给第三方不安全 DNS 服务器等导致 DNSSEC 缺失）；与域名服务器通信的路由方面 55 个 PP（85.9%）解析路径上至少一个域名服务器无 ROA 保护，其中 44 个归因于 gTLD 域名服务器未注册 ROA；RP-PP 通信路由方面 5 个 PP 未为其 PP 服务器 IP 注册 ROA。路由劫持模拟显示最脆弱 PP 可达 65%–83% 的 AS 失去连通性；PP 间的确定/概率依赖还可能放大不安全 PP 的影响造成级联故障。


### CRISP: An Efficient Cryptographic Framework for ML Inference Against Malicious Clients.
- **作者**: Xiaoyu Fang, Shihui Zheng, Lize Gu
- **论文 PDF**: [CRISP: An Efficient Cryptographic Framework for ML Inference Against Malicious Clients.](https://www.ndss-symposium.org/wp-content/uploads/2026-s11-paper.pdf)
- **解读**: 基于半诚实安全模型的机器学习推理协议易受恶意客户端攻击导致模型参数泄露，此前工作引入额外 MAC 计算保证客户端正确行为但增加在线推理开销。本文提出 CRISP 两方密码学框架：基于函数秘密共享（Function Secret Sharing）新原语设计非线性层协议，核心优化 MAC 重构过程；为线性层设计复域验证机制，更充分利用同态加密 CKKS 的复空间消除额外 MAC 计算。相比前作 SIMC (USENIX Security'22) 还避免了特定混淆电路优化下 MAC 重构泄露模型中间输入输出（含模型参数）的兼容性问题，在 SIMC 基准上通信成本最多降低 94%、推理延迟最多降低 43%。


### Cross-Boundary Mobile Tracking: Exploring Java-to-JavaScript Information Diffusion in WebViews.
- **作者**: Sohom Datta, Michalis Diamantaris, Ahsan Zafar, Junhua Su, Anupam Das, Jason Polakis, Alexandros Kapravelos
- **论文 PDF**: [Cross-Boundary Mobile Tracking: Exploring Java-to-JavaScript Information Diffusion in WebViews.](https://www.ndss-symposium.org/wp-content/uploads/2026-s910-paper.pdf)
- **解读**: Android WebView 中运行的 JavaScript 行为与隐私泄露缺乏大规模动态分析。本文提出 WebViewTracer，运行时动态分析 WebView 内 JavaScript 执行：融合 WebView 内 JS 执行轨迹与 Java 方法调用信息，捕获 Java SDK 与网页脚本间的信息交换。在 1 万个应用中开展首次大规模动态分析：4,597 个应用加载 WebView，其中超过 69% 向 WebView 注入 JS 通常不可访问的敏感与追踪信息（如广告 ID、Android build ID），90% 通过基于 Web 的 API 外泄给第三方服务器，并发现 WebView 内 JS 使用常见网页指纹技术，揭示移动追踪生态中的跨边界信息扩散隐私风险。


### Cross-Cache Attacks for the Linux Kernel via PCP Massaging.
- **作者**: Claudio Migliorelli, Andrea Mambretti, Alessandro Sorniotti, Vittorio Zaccaria, Anil Kurmus
- **论文 PDF**: [Cross-Cache Attacks for the Linux Kernel via PCP Massaging.](https://www.ndss-symposium.org/wp-content/uploads/2026-f862-paper.pdf)
- **解读**: 内核内存分配器仍是关键攻击面，主流缓解削弱了传统攻击技术但鲁棒的跨缓存（cross-cache）攻击仍可行。本文提出 PCPLOST 跨缓存内存塑形技术：精心利用侧信道推断内核分配器内部状态以绕过主线缓解，证明越界（OOB）以及经指针转移的释放后使用（UAF）与双重释放（DF）可跨所有通用缓存可靠利用，即使存在噪声。利用 PCPLOST 成功利用 6 个公开 CVE，多数情况获得跨缓存布局的可靠性超过 90%，表明现有缓解策略未能为 Linux 内核提供全面防护。


### Cross-Consensus Reliable Broadcast and its Applications.
- **作者**: Yue Huang, Xin Wang, Haibin Zhang, Sisi Duan
- **论文 PDF**: [Cross-Consensus Reliable Broadcast and its Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-s207-paper.pdf)
- **解读**: 传统拜占庭容错协议关注组内节点工作流，而分片协议、跨链桥等应用涉及跨组通信，缺乏对此类通信属性的建模。本文提出新原语跨共识可靠广播（XRBC），建模至少一组执行共识协议的两组间通信的安全属性：给出不同假设下三种 XRBC 构造，并给出三个应用——经 Reticulum (NDSS 2024) 案例的跨分片协调协议、经 Chainspace (NDSS 2018) 案例的跨分片交易协议与跨链桥方案。评估显示协议高效：Reticulum 案例中延迟比原始方案低 61.16%。


### Cryptobazaar: Private Sealed-bid Auctions at Scale.
- **作者**: Andrija Novakovic, Alireza Kavousi, Kobi Gurkan, Philipp Jovanovic
- **论文 PDF**: [Cryptobazaar: Private Sealed-bid Auctions at Scale.](https://www.ndss-symposium.org/wp-content/uploads/2026-f481-paper.pdf)
- **解读**: 本文提出 Cryptobazaar，可扩展、私密、去中心化的密封投标拍卖协议：保护落标者标书机密性，保证结果公开可验证，仅依赖单个不可信拍卖师协调。核心是组合计算一元编码标书列表逻辑或的高效分布式协议与多个可独立关注的新型零知识简洁论证。提供适用于一价、二价、更一般 (p+1) 价及序贯一价拍卖的协议变体；性能评估显示 128 个投标人、1024 个价格值的单场拍卖 0.5 秒内完成，每投标人收发仅约 32KB 数据。


### CryptPEFT: Efficient and Private Neural Network Inference via Parameter-Efficient Fine-Tuning.
- **作者**: Saisai Xia, Wenhao Wang, Zihao Wang, Yuhui Zhang, Yier Jin, Dan Meng, Rui Hou
- **论文 PDF**: [CryptPEFT: Efficient and Private Neural Network Inference via Parameter-Efficient Fine-Tuning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1102-paper.pdf)
- **解读**: 公开预训练骨干与轻量 PEFT 适配器成为现代 ML 流水线标配，但推理时保护用户输入与微调适配器（常含敏感训练数据）隐私仍具挑战，现有 MPC 方案因骨干与适配器间双向通信在两者上都要做大量密文计算。本文提出 CRYPTPEFT，首个面向隐私推理场景的 PEFT 方案：单向通信（OWC）架构把密文计算限制在适配器内，大幅降低计算与通信开销；并探索 OWC 兼容适配器设计空间，用自动化架构搜索优化隐私推理效率与模型效用权衡。基于 Vision Transformer 骨干在图像分类数据集上，模拟广域网/局域网设置下比基线快 20.62×–291.48×，CIFAR-100 上 2.26 秒推理延迟达到 85.47% 精度。


### CTng: Secure Certificate and Revocation Transparency.
- **作者**: Jie Kong, James Damon, Hemi Leibowitz, Ewa Syta, Amir Herzberg
- **论文 PDF**: [CTng: Secure Certificate and Revocation Transparency.](https://www.ndss-symposium.org/wp-content/uploads/2026-s213-paper.pdf)
- **解读**: 现有 PKI 面临多个挑战：证书透明（CT）规范仍假设日志器良性，用日志冗余解决则效率低、仅支持 f≤2。本文提出 CTng，演进式实用 PKI 设计：在不信任任何单个 CA、日志器或依赖方（NTTP 安全）的前提下，保证证书透明性与保证的、无歧义的吊销，即使这些实体任意腐败，仅需已知腐败监视器数量上界 f（如 f=8），性能影响极小。CTng 还支持高效证书验证、保护依赖方隐私，并规模化高效分发吊销更新；安全分析与开源原型评估表明在现实部署条件下高效可扩展。

### CtPhishCapture: Uncovering Credential-Theft-Based Phishing Scams Targeting Cryptocurrency Wallets.
- **作者**: Hui Jiang, Zhenrui Zhang, Xiang Li, Yan Li, Anpeng Zhou, Chenghui Wu, Man Hou, Jia Zhang, Zongpeng Li
- **论文 PDF**: [CtPhishCapture: Uncovering Credential-Theft-Based Phishing Scams Targeting Cryptocurrency Wallets.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2854-paper.pdf)
- **解读**: 针对加密货币生态中高发的窃取凭证型钱包钓鱼（CtPhish）诈骗，现有钓鱼检测方法或不适配或存在明显局限。CtPhishCapture 构建大规模检测系统：主动访问可疑网站并用大语言模型识别 CtPhish 网站，同时下载并分析可疑 App；六个月部署发现 5,138 个钓鱼网站与 10,612 个钓鱼应用，其中 83% 的网站与 79% 的应用为首次发现。基于该数据集完成对 CtPhish 生态的端到端测量（诱骗渠道、信任建立与资产窃取方式等），并联合搜索引擎厂商部署，使每周用户投诉量下降 5.8 倍。


### cwPSU: Efficient Unbalanced Private Set Union via Constant-weight Codes.
- **作者**: Qingwen Li, Song Bian, Hui Li
- **论文 PDF**: [cwPSU: Efficient Unbalanced Private Set Union via Constant-weight Codes.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1128-paper.pdf)
- **解读**: 针对非平衡场景下私有集合求并（PSU）协议通信开销随大集合规模增长、且多次调用不经意伪随机函数导致轮数过多的问题，提出 cwPSU：基于常权码与层级全同态加密构建。为防泄漏引入 Batched Ciphertext Shuffle 技术实现密文安全重排，并设计优化的常权相等运算符，将非标量乘法数降至朴素方案的约三分之一。协议通信量与较小集合线性相关而与较大集合无关，且仅需一轮在线通信，实测相比现有最优方案通信减少 5.1–32.4 倍、运行时间提速 3.1–13.3 倍。


### Dataset Reduction and Watermark Removal via Self-supervised Learning for Model Extraction Attack.
- **作者**: Hao Luan, Xue Tan, Zhiheng Li, Jun Dai, Xiaoyan Sun, Ping Chen
- **论文 PDF**: [Dataset Reduction and Watermark Removal via Self-supervised Learning for Model Extraction Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f223-paper.pdf)
- **解读**: 针对模型窃取攻击难以去除黑盒水印、且通常需数千次查询的低效问题，提出 SSLExtraction：先按自监督学习范式将像素级输入转换为与水印无关的特征表示，再在特征空间中用贪心随机游走构造分散的查询集以覆盖特征空间并避免冗余查询，同时天然将水印模式识别为离群点实现水印去除。还提出基于假设检验的评估指标以度量可疑模型与含水印/良性模型的相对距离。实验表明该方法显著降低查询成本并在多种数据集与水印场景下有效去除水印。


### Decompiling the Synergy: An Empirical Study of Human-LLM Teaming in Software Reverse Engineering.
- **作者**: Zion Leonahenahe Basque, Samuele Doria, Ananta Soneji, Wil Gibbs, Adam Doupé, Yan Shoshitaishvili, Eleonora Losiouk, Ruoyu Wang, Simone Aonzo
- **解读**: 该工作对人与大语言模型协作进行软件逆向工程展开实证研究。（无摘要，据标题推断）


### Demystifying RPKI-Invalid Prefixes: Hidden Causes and Security Risks.
- **作者**: Weitong Li, Tao Wan, Tijay Chung
- **论文 PDF**: [Demystifying RPKI-Invalid Prefixes: Hidden Causes and Security Risks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s161-paper.pdf)
- **解读**: 针对 RPKI 已大规模部署（51.3% 路由被 ROA 覆盖）却仍存在 6,802 个 RPKI-invalid 前缀、其隐藏成因不明的问题，开展首个系统性研究：发现 ROA 误配置多发于 IP 租用与 IP 转接服务场景，并将 96.9% 的 invalid 前缀归因于此类误配置。进一步刻画其数据平面级联影响：3.1% 导致完全断连、7.1% 增加延迟与跳数甚至绕过安全机制，且会触发劫持检测系统的误报；通过与 174 家运营商的直接互动构建 294 个误配置前缀的真值集，并访谈 16 家大型 ISP 后提出规避 ROA 误配置的改进建议。


### Demystifying the Access Control Mechanism of ESXi VMKernel.
- **作者**: Yue Liu, Zexiang Zhang, Jiaxun Zhu, Hao Zheng, Jiaqing Huang, Wenbo Shen, Gaoning Pan, Yuliang Lu, Min Zhang, Zulie Pan, Guang Cheng
- **论文 PDF**: [Demystifying the Access Control Mechanism of ESXi VMKernel.](https://www.ndss-symposium.org/wp-content/uploads/2026-f700-paper.pdf)
- **解读**: 针对闭源专有的 VMware ESXi VMKernel 强制访问控制机制长期不透明、既有研究集中于虚拟设备漏洞与虚拟机逃逸的问题，开展首次系统性安全分析。提出域控制结构导向的分析方法重建关键权限逻辑，并设计结构感知调试框架支撑细粒度运行时验证，据此发现可写且无保护的内存控制结构、可被利用的开发保留系统调用接口等设计缺陷，构造绕过沙箱、提权与持久化三种攻击场景。共向 VMware 报告 14 个漏洞，全部被确认修复并获 42,000 美元赏金。


### DirtyFree: Simplified Data-Oriented Programming in the Linux Kernel.
- **作者**: Yoochan Lee, Hyuk Kwon, Thorsten Holz
- **论文 PDF**: [DirtyFree: Simplified Data-Oriented Programming in the Linux Kernel.](https://www.ndss-symposium.org/wp-content/uploads/2026-f527-paper.pdf)
- **解读**: 针对内核控制流完整性（KCFI）普及后数据导向编程（DOP）成为提权替代方案、但传统 DOP 多阶段攻击需堆地址泄漏与任意读/写、复杂且实用性受限的问题，提出 DIRTYFREE 利用任意释放原语强制释放攻击者控制的内核对象，显著降低利用门槛并简化利用流程。系统性地跨内核缓存识别出 14 个任意释放对象，并成功利用 24 个真实内核漏洞验证有效性；同时提出两种缓解技术，开销分别仅 0.28% 与 -0.55%。


### Discovering Blind-Trust Vulnerabilities in PLC Binaries via State Machine Recovery.
- **作者**: Fangzhou Dong, Arvind S. Raj, Efrén López-Morales, Siyu Liu, Yan Shoshitaishvili, Tiffany Bao, Adam Doupé, Muslum Ozgur Ozmen, Ruoyu Wang
- **论文 PDF**: [Discovering Blind-Trust Vulnerabilities in PLC Binaries via State Machine Recovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1624-paper.pdf)
- **解读**: 针对 PLC 因缺少对外围输入的安全检查而产生的盲信（Blind-Trust）漏洞，既有状态机安全检测多需设计规范或源码、且固件重托管仍属开放难题。提出 Ta'veren：纯静态分析直接从 PLC 二进制恢复有限状态机并在多种策略规范下重复安全分析；基于 PLC 常用特定变量表示内部状态的观察实现激进的去重抽象，在不损失可靠性的前提下压缩状态空间。在真实 PLC 二进制上验证其能高效恢复有意义的 FSM 并高效果发现关键安全违规。


### Distributed Broadcast Encryption for Confidential Interoperability across Private Blockchains.
- **作者**: Angelo De Caro, Kaoutar Elkhiyaoui, Sandeep Nishad, Sikhar Patranabis, Venkatraman Ramakrishna
- **论文 PDF**: [Distributed Broadcast Encryption for Confidential Interoperability across Private Blockchains.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1200-paper.pdf)
- **解读**: 针对私有区块链间互操作依赖可信集中代理解密并转发账本状态、违背 DLT 去中心化原则的问题，利用全分布式广播加密（FDBE）构建完全去中心化的跨私有网络机密信息共享协议：FDBE 支持无可信设置下的分布式公钥协商与安全密钥派生，发送方可用目标网络的 FDBE 公钥为其成员加密消息。构造在简化 UC 框架下可证明安全，并给出首个解密密钥与密文均为常量的 FDBE 实例化，在 Hyperledger Cacti 中两个 Fabric 网络间完成参考实现与性能评估。


### DNN Latency Sequencing: Extracting DNN Architectures from Intel SGX Enclaves with Single-Stepping Attacks.
- **作者**: Minkyung Park, Zelun Kong, DaveTian, Z. Berkay Celik, Chung Hwan Kim
- **论文 PDF**: [DNN Latency Sequencing: Extracting DNN Architectures from Intel SGX Enclaves with Single-Stepping Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1455-paper.pdf)
- **解读**: 针对 SGX 等 TEE 保护下的 DNN 架构（层数、类型等知识产权）仍可被侧信道提取、而既有提取攻击多未考虑 TEE 或局限于特定模型类型的问题，提出 DNN Latency Sequencing（DLS）：用 SGX-Step 单步执行模型并采集细粒度延迟轨迹，在函数与基本块两级分析以重建模型架构，核心洞察是架构特征会体现在执行行为中。在 Darknet、TensorFlow Lite 与 ONNX Runtime 上分别取得 97.3%、96.4%、93.6% 的架构恢复准确率，并进一步展示其支撑高级攻击。


### DOM-XSS Detection via Webpage Interaction Fuzzing and URL Component Synthesis.
- **作者**: Nuno Sabino, Darion Cassel, Rui Abreu, Pedro Adão, Lujo Bauer, Limin Jia
- **论文 PDF**: [DOM-XSS Detection via Webpage Interaction Fuzzing and URL Component Synthesis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1467-paper.pdf)
- **解读**: 针对大规模 DOM-XSS 自动化检测的两点局限：不与页面交互从而漏掉依赖用户动作的事件处理器漏洞、且不合成可触发更多代码路径的 URL 参数与片段。提出 SWIPE：用模糊测试生成用户交互以触发事件处理器，用动态符号执行（DSE）自动合成 URL 参数与片段；在 Tranco 前 3 万域名页面的 44,480 条 URL 上运行，模糊器相比先前工作多发现 15% 漏洞，DSE 合成出先前未见的 URL 组件并触发 20 个新漏洞。


### DUALBREACH: Efficient Dual-Jailbreaking via Target-Driven Initialization and Multi-Target Optimization.
- **作者**: Xinzhe Huang, Kedong Xiu, Tianhang Zheng, Churui Zeng, Wangze Ni, Zhan Qin, Kui Ren, Chun Chen
- **论文 PDF**: [DUALBREACH: Efficient Dual-Jailbreaking via Target-Driven Initialization and Multi-Target Optimization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1062-paper.pdf)
- **解读**: 针对同时绕过 LLM 与其护栏的双重越狱（dual-jailbreaking）研究不足、现有攻击在护栏防护下成功率有限的问题，提出目标驱动的 DUALBREACH：用目标驱动初始化（TDI）动态构造初始提示，配合多目标优化（MTO）以近似梯度同时适配护栏与 LLM，兼顾查询数节省与高成功率；对黑盒护栏采用开源强护栏或训练代理模型模仿。在 GPT-4 配 LlamaGuard-3 场景下平均双重越狱成功率 93.67%（其余方法最高 88.33%），每次成功平均仅 1.77 次查询；并给出 XGBoost 集成防御 EGUARD。


### DualStrike: Accurate, Real-time Eavesdropping and Injection of Keystrokes on Commodity Keyboards.
- **作者**: Xiaomeng Chen, Jike Wang, Zhenyu Chen, Qi Alfred Chen, Xinbing Wang, Dongyao Chen
- **论文 PDF**: [DualStrike: Accurate, Real-time Eavesdropping and Injection of Keystrokes on Commodity Keyboards.](https://www.ndss-symposium.org/wp-content/uploads/2026-s46-paper.pdf)
- **解读**: 针对商品霍尔效应键盘上可同时实现窃听与非侵入式逐键注入的新攻击面，提出 DualStrike：以紧凑电磁铁硬件实现高频磁场欺骗（免同步攻击方案），并借助商用磁力计实现按键监听。在六款最新霍尔键盘上实测逐键注入准确率超 98.9%，端到端监听准确率超 99%；校准算法使 4cm 偏移下仍保持 98.5% 注入准确率，并分析了对现有磁屏蔽机制的免疫性、提出新的屏蔽方案。


### Efficiently Detecting DBMS Bugs through Bottom-up Syntax-based SQL Generation.
- **作者**: Yu Liang, Peng Liu
- **论文 PDF**: [Efficiently Detecting DBMS Bugs through Bottom-up Syntax-based SQL Generation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f198-paper.pdf)
- **解读**: 针对既有语法驱动 SQL 生成均采用自顶向下方式、过度探索靠近根节点的浅层文法而忽视特征丰富的深层文法、导致 DBMS 漏洞发现效率低的问题，提出自底向上生成技术：从一条刻画特征丰富功能的文法规则出发回溯到根节点构造语法路径，再将多条路径扩展合并为多样 SQL 用于模糊测试。原型工具 SQLBull 在 MySQL、MariaDB、CockroachDB、DuckDB、PostgreSQL 五个成熟 DBMS 上发现 63 个零日漏洞，漏洞发现与代码覆盖率均优于现有工具。


### Enhancing Legal Document Security and Accessibility with TAF.
- **作者**: Renata Vaderna, Dusan Nikolic, Patrick Zielinski, David Greisen, BJ Ard, Justin Cappos
- **论文 PDF**: [Enhancing Legal Document Security and Accessibility with TAF.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1002-paper.pdf)
- **解读**: 针对法律在线平台面临的安全保障难题（法律文本仍多以纸质或老旧平台发布、易遭未授权篡改），提出 TAF：首个在攻击者完全控制托管仓库威胁模型下设计、可将每个签名仓库状态绑定到发布方定义的法律生效日期的系统，支持可验证的按日期检索。TAF 结合 TUF 的软件更新保证、Git 的版本结构与被签名的强时间概念，将法律文档的完整演化变成可认证、带时间戳的状态序列；已在美国 14 个司法辖区（含巴尔的摩、马里兰州、华盛顿特区）生产部署。


### Enhancing Semantic-Aware Binary Diffing with High-Confidence Dynamic Instruction Alignment.
- **作者**: Chengfeng Ye, Anshunkang Zhou, Charles Zhang
- **论文 PDF**: [Enhancing Semantic-Aware Binary Diffing with High-Confidence Dynamic Instruction Alignment.](https://www.ndss-symposium.org/wp-content/uploads/2026-f663-paper.pdf)
- **解读**: 针对二进制对比中锚点（细粒度指令对齐）识别受限于语法方法脆弱于激进编译器优化、语义方法计算代价高或覆盖低的问题，基于「并非所有动态语义对指令对齐同等有效」的洞察，提出 Barracuda：用强制执行提取的部分指令语义高置信度地生成对齐锚点。实验检测出多 24.0% 的对齐锚点且精度达 92.1%，将 DeepBinDiff 与 SigmaDiff 的 F1 分别提升 12.3–42.7 与 2.2–4.1 个百分点。


### Enhancing Website Fingerprinting Attacks against Traffic Drift.
- **作者**: Xinhao Deng, Yixiang Zhang, Qi Li, Zhuotao Liu, Yabo Wang, Ke Xu
- **论文 PDF**: [Enhancing Website Fingerprinting Attacks against Traffic Drift.](https://www.ndss-symposium.org/wp-content/uploads/2026-s59-paper.pdf)
- **解读**: 针对基于深度学习的网站指纹（WF）攻击在真实世界中因内容与网络条件变化（流量漂移）而严重失效的问题，提出首个自适应 WF 攻击框架 Proteus：部署期间仅用无标签漂移流量持续微调模型，通过最小化最大均值差异对齐新旧流量特征分布、以熵优化增强置信度，并用高斯混合模型生成可靠伪标签做监督微调。在含超 35 万条真实 Tor 浏览轨迹的六种漂移场景上，使八种 SOTA WF 攻击的 F1 平均相对提升 94.24%，且可无缝集成到既有 DL 攻击中。


### Entente: Cross-silo Intrusion Detection on Network Log Graphs with Federated Learning.
- **作者**: Jiacen Xu, Chenang Li, Yu Zheng, Zhou Li
- **论文 PDF**: [Entente: Cross-silo Intrusion Detection on Network Log Graphs with Federated Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s93-paper.pdf)
- **解读**: 针对图网络入侵检测（GNIDS）普遍假设集中式数据、而隐私法规与运维约束使跨组织数据共享不现实的问题，提出以联邦学习构建跨孤岛 GNIDS 的 ENTENTE。针对客户端图异构与 GNIDS 设计多样导致朴素 FL 失效的问题，提出参考图合成、图草图化与自适应贡献缩放等面向图数据的技巧，兼顾有效性、可扩展性与鲁棒性。在 LANL、OpTC、Pivoting 大规模数据集上优于 SOTA FL 基线，并证明在面向 GNIDS 的投毒攻击下可将攻击成功率限制在较低水平。


### Eviction Notice: Reviving and Advancing Page Cache Attacks.
- **作者**: Sudheendra Raghav Neela, Jonas Juffinger, Lukas Maar, Daniel Gruss
- **论文 PDF**: [Eviction Notice: Reviving and Advancing Page Cache Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f6-paper.pdf)
- **解读**: 针对 2019 年缓解措施后页缓存攻击仅剩时间分辨率低、系统开销大的 Evict+Reload 类手段的问题，系统化提出四种原语（flush、reload、evict、monitor）并派生出 Flush+Monitor、Flush+Reload、Flush+Flush、Evict+Monitor、Evict+Reload 五种攻击技术，且全部可绕过现有缓解运行于最新内核。最快的 Flush+Monitor 跨进程隐蔽信道容量平均 37.7 kB/s；低频率攻击实现 4 kB 空间分辨率与 0.8 μs 时间分辨率的跨进程击键计时与事件检测（较先前提升六个数量级）；网站指纹攻击在 top-100 封闭世界取得 90.54% F1，结论是页缓存侧信道仍需进一步缓解。


### EXIA: Trusted Transitions for Enclaves via External-Input Attestation.
- **作者**: Zhen Huang, Yidi Kao, Sanchuan Chen, Guoxing Chen, Yan Meng, Haojin Zhu
- **论文 PDF**: [EXIA: Trusted Transitions for Enclaves via External-Input Attestation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2421-paper.pdf)
- **解读**: 针对 TEE 启动时远程证明之后内存破坏攻击仍可破坏安全保证、控制流证明（CFA）缺乏具体验证方法且可被纯数据攻击绕过的问题，提出外部输入证明（External-Input Attestation）：基于「内存破坏始于意外写入」的观察，证明对 TEE 保护应用的所有写入，将安全事件（如控制流劫持）转化为可靠性事件（如意外输入导致崩溃）。当前版本限于验证者已知输入的应用，在 AMD SEV-SNP 与 Penglai 上实现原型，安全模型训练、模型推理、数据库与密钥管理等案例中性能开销极小。


### Exploiting TLBs in Virtualized GPUs for Cross-VM Side-Channel Attacks.
- **作者**: Hongyue Jin, Yanan Guo, Zhenkai Zhang
- **论文 PDF**: [Exploiting TLBs in Virtualized GPUs for Cross-VM Side-Channel Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1480-paper.pdf)
- **解读**: 针对云环境中虚拟化 GPU 多租户共享被忽视的微架构信息泄漏风险，开发面向虚拟化 NVIDIA GPU TLB 的 Prime+Probe 攻击原语，并解决 GPU 虚拟化环境下的独特挑战。基于该原语在云场景演示两个跨虚拟机侧信道案例：游戏 Counter-Strike 2 中作弊（暴露隐藏对手）与虚拟桌面用户的网站指纹识别；据作者所述这是首次在云环境中对虚拟化 GPU 演示侧信道攻击，揭示此前未知的安全风险。


### ExpShield: Safeguarding Web Text from Unauthorized Crawling and LLM Exploitation.
- **作者**: Ruixuan Liu, Toan Tran, Tianhao Wang, Hongsheng Hu, Shuo Wang, Li Xiong
- **论文 PDF**: [ExpShield: Safeguarding Web Text from Unauthorized Crawling and LLM Exploitation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f11-paper.pdf)
- **解读**: 针对大语言模型记忆网页抓取内容导致版权/隐私信息暴露、而既有防护依赖爬虫或模型开发方配合的问题，提出主动自防护 ExpShield：以不可见扰动在保持可读性的同时抑制记忆，建模为带约束优化；先提出实例利用（instance exploitation）度量单条文本加入训练后对猜测概率的提升，再以单层优化与合成扰动两种代理方案求解。通过验证记忆触发器假设设计定向扰动：中和固有触发 token 降低记忆、注入人工触发 token 误导记忆；实验中 MIA AUC 由 0.95 降至 0.55，实例利用趋近于零。


### Fast Pointer Nullification for Use-After-Free Prevention.
- **作者**: Yubo Du, Youtao Zhang, Jun Yang
- **论文 PDF**: [Fast Pointer Nullification for Use-After-Free Prevention.](https://www.ndss-symposium.org/wp-content/uploads/2026-f753-paper.pdf)
- **解读**: 针对指针置空（PN）类 UAF 防御因逐指针关联目标缓冲区导致元数据查找昂贵、且忽略指针存储的空间局部性而注册过多的问题，提出 FPN：在区域级别组织元数据以消除昂贵的查找操作，并用基于块（block）的注册机制高效捕获指针局部性。SPEC CPU 基准与真实应用实验表明，FPN 在提供强安全保证的同时，性能与内存开销较既有 PN 技术显著降低。


### Faster Than Ever: A New Lightweight Private Set Intersection and Its Variants.
- **作者**: Guowei Ling, Peng Tang, Jinyong Shan, Liyao Xiang, Weidong Qiu
- **论文 PDF**: [Faster Than Ever: A New Lightweight Private Set Intersection and Its Variants.](https://www.ndss-symposium.org/wp-content/uploads/2026-f131-paper.pdf)
- **解读**: 针对现有两方私有集合交集（PSI）协议基础 OT 与 OKVS 编码开销大的问题，提出轻量级 PSI 新范式（半诚实与恶意模型均适用）：仅需少量基础 OT 与单次 OKVS 编解码，除基础 OT 外全部计算可用 SIMD 加速的对称密码指令与高效位运算实现。将其扩展为电路 PSI 及 PSI-cardinality、PSI-sum、Private Join and Compute（PJC）等变体；相比最高效的 VOLE 类 PSI 运行时间约快 40%，电路 PSI 快至 3.7 倍且通信减少 1.5 倍，cardinality/sum 分别提速至 12.4 倍与 10 倍，PJC 运行时间提升 762 倍。


### FidelityGPT: Correcting Decompilation Distortions with Retrieval Augmented Generation.
- **作者**: Zhiping Zhou, Xiaohong Li, Ruitao Feng, Yao Zhang, Yuekang Li, Wenbu Feng, Yunqian Wang, Yuqing Li
- **论文 PDF**: [FidelityGPT: Correcting Decompilation Distortions with Retrieval Augmented Generation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s989-paper.pdf)
- **解读**: 针对反编译结果保真度问题（复杂闭源二进制场景下既有工作缺乏有效的失真检测与修正）提出 FidelityGPT：定义面向闭源环境的失真提示模板，结合检索增强生成（RAG）与动态语义强度算法按语义强度定位失真行并检索相似代码修正，另设计变量依赖算法将冗余变量名纳入上下文以克服长上下文限制。在二元相似性基准的 620 个函数对上平均检测准确率 89%、精度 83%；修正率（FR）94%、修正正确率（CFR）64%，均优于现有最优模型 DeGPT（83%/37%）。


### FirmAgent: Leveraging Fuzzing to Assist LLM Agents with IoT Firmware Vulnerability Discovery.
- **作者**: Jiangan Ji, Chao Zhang, Shuitao Gan, Lin Jian, Hangtian Liu, Tieming Liu, Lei Zheng, Zhipeng Jia
- **论文 PDF**: [FirmAgent: Leveraging Fuzzing to Assist LLM Agents with IoT Firmware Vulnerability Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1943-paper.pdf)
- **解读**: 针对静态分析（含 LLM）高误报且无 PoC、动态分析（模糊测试）高漏报的问题，提出首个以模糊测试辅助 LLM 智能体的 IoT 固件漏洞发现方案 FirmAgent：核心观察是模糊测试能精确定位输入相关代码点、静态分析能透彻分析其后的程序路径；FirmAgent 用模糊测试收集运行时输入点（污点源）并重建潜在漏洞路径，由一个 LLM 智能体做上下文感知污点分析、另一个智能体将模糊测试用例精炼为 PoC。在 14 个真实固件上以 91% 精度发现 182 个漏洞，其中 140 个此前未知、17 个已分配 CVE。


### FirmCross: Detecting Taint-style Vulnerabilities in Modern C-Lua Hybrid Web Services of Linux-based Firmware.
- **作者**: Runhao Liu, Jiarun Dai, Haoyu Xiao, Yuan Zhang, Yeqi Mou, Lukai Xu, Bo Yu, Baosheng Wang, Min Yang
- **论文 PDF**: [FirmCross: Detecting Taint-style Vulnerabilities in Modern C-Lua Hybrid Web Services of Linux-based Firmware.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1251-paper.pdf)
- **解读**: 针对现代固件广泛用 Lua 脚本/字节码与 C 二进制共同实现混合 Web 服务、而既有污点检测仅将 C 二进制纳入范围导致效果差的问题，提出 FirmCross：自动去混淆目标固件中的 Lua 字节码、识别 Lua 代码空间中的独特污点源，并系统捕捉 C-Lua 跨语言污点流。在 11 个厂商 73 个固件镜像上比 SOTA（MangoDFA、LuaTaint）多检出 6.82–14.5 倍漏洞，共识别 610 个零日漏洞，迄今 31 个已获漏洞编号。


### FLIPPYRAM: A Large-Scale Study of Rowhammer Prevalence.
- **作者**: Martin Heckel, Nima Sayadi, Jonas Juffinger, Carina Fiedler, Daniel Gruss, Florian Adamsky
- **论文 PDF**: [FLIPPYRAM: A Large-Scale Study of Rowhammer Prevalence.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1810-paper.pdf)
- **解读**: 针对既往 Rowhammer 研究样本量小（少量 DIMM）或依赖精确控制的 FPGA 实验、结果难以推广的问题，开展首次大规模流行病学研究：构建全自动跨平台框架 FlippyRAM，用 5 个工具逆向 DRAM 寻址函数、再用 7 个工具发起 Rowhammer 攻击，2024-12-30 至 2025-06-30 间经线上与 U 盘分发收集 822 个系统的 1006 份数据集。结果：453 份成功完成寻址函数逆向；126 份（12.5%）在完全自动攻击下出现比特翻转，说明可武器化的 Rowhammer 攻击虽低于实验室预期但仍是实际威胁；并指出寻址逆向可靠性与跨微架构攻击可靠性是两大关键研究挑战。


### FlyTrap: Physical Distance-Pulling Attack Towards Camera-based Autonomous Target Tracking Systems.
- **作者**: Shaoyuan Xie, Mohamad Habib Fakih, Junchi Lu, Fayzah Alshammari, Ningfei Wang, Takami Sato, Halima Bouzidi, Mohammad Abdullah Al Faruque, Qi Alfred Chen
- **论文 PDF**: [FlyTrap: Physical Distance-Pulling Attack Towards Camera-based Autonomous Target Tracking Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s904-paper.pdf)
- **解读**: 针对自主目标跟踪（ATT）系统（尤其 ATT 无人机）的安全关键性，提出新型距离牵引攻击（DPA）并系统研究：利用 ATT 系统的漏洞危险地缩短跟踪距离，导致无人机被捕获、更易受传感器攻击甚至物理碰撞。提出 FlyTrap 物理世界攻击框架，以对抗性「伞」作为可部署、领域特化的攻击载体，通过渐进式距离牵引策略与可控时空一致性设计，满足物理可部署、闭环有效与时空一致等目标；在白盒及商用无人机（DJI、HoverAir）上的真实闭环实验证明可将跟踪距离压到可被捕获、攻击乃至直接坠毁的范围。


### Formal Analysis of BLE Secure Connection Pairing and Revelation of the PE Confusion Attack.
- **作者**: Min Shi, Yongkang Xiao, Jing Chen, Kun He, Ruiying Du, Meng Jia
- **论文 PDF**: [Formal Analysis of BLE Secure Connection Pairing and Revelation of the PE Confusion Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f779-paper.pdf)
- **解读**: 针对 BLE 安全连接（SC）配对协议形式化分析面临的协议流程复杂、配对方法选择难以形式化、用户假设过于理想等障碍，用 Tamarin 完成准确全面的形式化分析：为各参与方提取状态机作为建模蓝图、用等式理论形式化配对方法选择逻辑，并纳入微妙用户行为与更强敌手（如临时 OOB 信道被攻破）。验证了 84 种配对情形并识别出协议所需的最少安全假设，且发现一种新的中间人攻击——PE 混淆攻击，提供 PoC 复现并提出对抗该攻击的防御对策。


### From Noise to Signal: Precisely Identify Affected Packages of Known Vulnerabilities in npm Ecosystem.
- **作者**: Yingyuan Pu, Lingyun Ying, Yacong Gu
- **论文 PDF**: [From Noise to Signal: Precisely Identify Affected Packages of Known Vulnerabilities in npm Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1902-paper.pdf)
- **解读**: 针对 npm 生态已知漏洞影响分析中包级传播工具误报高、函数级工具难以大规模运行的矛盾，提出 VulTracer：先为每个包独立构建富语义图再拼接成跨包图，精确定位漏洞传播路径并识别真正受影响的包。比较评估中调用图构建 F1 达 0.905，将 npm audit 的误报降低 94%；对覆盖 3400 万包版本的全生态函数级影响测量表明，包级分析识别的潜在影响中 68.28% 是噪音（漏洞代码不可达），真实传播较浅、几跳依赖内即快速衰减。


### From Obfuscated to Obvious: A Comprehensive JavaScript Deobfuscation Tool for Security Analysis.
- **作者**: Dongchao Zhou, Lingyun Ying, Huajun Chai, Dongbin Wang
- **论文 PDF**: [From Obfuscated to Obvious: A Comprehensive JavaScript Deobfuscation Tool for Security Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2198-paper.pdf)
- **解读**: 针对现有 JavaScript 去混淆工具难以处理多样输入格式、只覆盖特定混淆类型、输出晦涩难读的问题，提出 JSIMPLIFIER：多阶段流水线整合预处理、基于 AST 的静态分析、动态执行追踪与 LLM 增强的标识符重命名，并给出融合控制/数据流、代码简化度、熵与 LLM 可读性评估的多维指标。构建并发布含 44,421 个样本（23,212 恶意 + 21,209 良性）的最大真实混淆 JS 数据集；评估显示对 20 种混淆技术处理能力 100%、正确率 100%、代码复杂度降低 88.2%、可读性提升 4 倍以上。


### From Perception to Protection: A Developer-Centered Study of Security and Privacy Threats in Extended Reality (XR).
- **作者**: Kunlin Cai, Jinghuai Zhang, Ying Li, Zhiyuan Wang, Xun Chen, Tianshi Li, Yuan Tian
- **论文 PDF**: [From Perception to Protection: A Developer-Centered Study of Security and Privacy Threats in Extended Reality (XR).](https://www.ndss-symposium.org/wp-content/uploads/2026-s807-paper.pdf)
- **解读**: 针对 XR 因沉浸式交互与空前数据采集带来的全新安全隐私（S&P）挑战、且缺少从开发者视角出发的威胁感知研究的问题，访谈 23 位专业 XR 开发者并围绕两个研究问题展开：发现（1）XR 开发决策（丰富传感器数据采集、用户生成内容接口等）与 S&P 威胁紧密相关甚至放大威胁，而开发者常未意识到风险、存在威胁感知认知偏差；（2）既有缓解方法局限叠加战略、技术与沟通支持不足，削弱了开发者应对威胁的动力与能力。据此提出贯穿 XR 开发流程的、面向利益相关者的可行建议。


### Fuzzilicon: A Post-Silicon Microcode-Guided x86 CPU Fuzzer.
- **作者**: Johannes Lenzen, Mohamadreza Rostami, Lichao Wu, Ahmad-Reza Sadeghi
- **论文 PDF**: [Fuzzilicon: A Post-Silicon Microcode-Guided x86 CPU Fuzzer.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1486-paper.pdf)
- **解读**: 针对真实 x86 CPU 是黑盒、微架构缺陷难以自动化发现的现状，提出首个面向真实 x86 处理器的后硅（post-silicon）模糊测试框架 Fuzzilicon：通过逆向 Intel 专有微码更新接口引入微码级插桩，从处理器微架构直接提取反馈，并与基于 hypervisor 的模糊测试装置结合，无需 RTL 或厂商支持即可做精确的反馈引导输入生成。在 Goldmont 微架构上发现 5 项重要发现（含两个此前未知的微码级推测执行漏洞），并自动重现此前需手工发现的 µSpectre 类漏洞；覆盖率采集开销较基线技术降低至 1/31。


### GoldenFuzz: Generative Golden Reference Hardware Fuzzing.
- **作者**: Lichao Wu, Mohamadreza Rostami, Huimin Li, Nikhilesh Singh, Ahmad-Reza Sadeghi
- **论文 PDF**: [GoldenFuzz: Generative Golden Reference Hardware Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1663-paper.pdf)
- **解读**: 针对硬件模糊测试语义感知有限、测试用例精炼低效、依赖慢速器件仿真导致计算开销大的问题，提出两阶段框架 GoldenFuzz：用快速且符合 ISA 的黄金参考模型（GRM）作为被测设备（DUT）的「数字孪生」，先对 GRM 模糊以低代价精炼测试用例，再在 DUT 上加速深度架构探索；测试用例由精心选择的指令块拼接而成以平衡指令间/指令内质量，并利用高/低覆盖率样本的反馈机制增强状态探索。在 RocketChip、BOOM、CVA6 三个 RISC-V 处理器上以最短用例与最低开销取得最高覆盖率，复现全部已知漏洞并发现 5 个新漏洞（4 个 CVSS 严重度超 7 分），另在商用 BA51-H 核扩展中发现两个此前未知漏洞。


### Hey there! You are using WhatsApp: Enumerating Three Billion Accounts for Security and Privacy.
- **作者**: Gabriel K. Gegenhuber, Philipp É. Frenzel, Maximilian Günther, Johanna Ullrich, Aljosha Judmayer
- **论文 PDF**: [Hey there! You are using WhatsApp: Enumerating Three Billion Accounts for Security and Privacy.](https://www.ndss-symposium.org/wp-content/uploads/2026-s805-paper.pdf)
- **解读**: 针对 WhatsApp 基于通讯录查询的联系人发现机制天然允许手机号枚举、而速率限制等标准防御被质疑是否有效的问题，重新审视该攻击面：实测每小时可探测超一亿个手机号而未被封禁或有效限速，证明该漏洞的持续性与严重性。2021 年 Facebook 数据泄漏中近半数手机号在 WhatsApp 上仍处于活跃状态；研究还完成了 WhatsApp 用户普查，展示端到端加密之下消息服务仍能产生宏观洞察，并发现不同设备与号码间复用某些 X25519 密钥的现象，暗示不安全的自实现或欺诈活动。


### Hiding an Ear in Plain Sight: On the Practicality and Implications of Acoustic Eavesdropping with Telecom Fiber Optic Cables.
- **作者**: Youqian Zhang, Zheng Fang, Huan Wu, Sze-Yiu Chau, Chao Lu, Xiapu Luo
- **论文 PDF**: [Hiding an Ear in Plain Sight: On the Practicality and Implications of Acoustic Eavesdropping with Telecom Fiber Optic Cables.](https://www.ndss-symposium.org/wp-content/uploads/2026-f546-paper.pdf)
- **解读**: 针对光纤被普遍视为抗干扰、低损耗可靠信道、但其对声学振动敏感的特性可构成声学窃听侧信道的问题，证明攻击者借助商用分布式声学传感（DAS）系统可远程监测光纤结构上声波引起的形变并恢复原始声音信息；FTTH 在楼宇中的普及进一步放大该风险。由于裸纤对空气声波灵敏度不足，作者引入「感觉受体」（Sensory Receptor）提升声学捕获能力，成功恢复人类活动、室内定位乃至对话内容等信息，对光纤通信网络提出重要隐私警示。


### HoneySat: A Network-based Satellite Honeypot Framework.
- **作者**: Efrén López-Morales, Ulysse Planta, Gabriele Marra, Carlos Gonzalez-Cortes, Jacob Hopkins, Majid Garoosi, Elías Obreque, Carlos E. Rubio-Medrano, Ali Abbasi
- **论文 PDF**: [HoneySat: A Network-based Satellite Honeypot Framework.](https://www.ndss-symposium.org/wp-content/uploads/2026-f537-paper.pdf)
- **解读**: 针对卫星安全长期依赖「架构难解+隐蔽即安全」、缺乏攻击技术数据的现状，提出首个高交互卫星蜜罐框架 HoneySat：可信地仿真真实 CubeSat 小卫星。通过调查确认 90% 的小卫星运营商认为其仿真真实，并在互联网上部署：成功欺骗野外敌手并收集 22 次真实对抗交互；还完成硬件在环操作，与在轨运行的商用小卫星任务成功通信。


### HOUSTON: Real-Time Anomaly Detection of Attacks against Ethereum DeFi Protocols.
- **作者**: Dongyu Meng, Fabio Gritti, Robert McLaughlin, Nicola Ruaro, Ilya Grishchenko, Christopher Kruegel, Giovanni Vigna
- **论文 PDF**: [HOUSTON: Real-Time Anomaly Detection of Attacks against Ethereum DeFi Protocols.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1534-paper.pdf)
- **解读**: 针对 DeFi 协议每周遭受数百万美元攻击损失、而现有安全工具多针对单合约特定漏洞类（如重入）、缺乏实时通用攻击识别的问题，提出 HOUSTON：先自动识别构成 DeFi 应用的合约集合，再在监控新交易时构建并更新定制异常检测模型——模型包含典型执行路径（控制流）与合约函数参数/存储变量间似然不变量，并给出可解释告警支持攻击分诊。在 22M+ 交易、115 起 DeFi 事件上真阳性率 94.8% 且误报率低，优于 SOTA 异常检测系统，并在真实环境以商用硬件实时运行。


### Huma: Censorship Circumvention via Web Protocol Tunneling with Deferred Traffic Replacement.
- **作者**: Sina Kamali, Diogo Barradas
- **论文 PDF**: [Huma: Censorship Circumvention via Web Protocol Tunneling with Deferred Traffic Replacement.](https://www.ndss-symposium.org/wp-content/uploads/2026-f328-paper.pdf)
- **解读**: 针对网页协议隧道类翻墙工具易被流量分析识别（指纹攻击或异常浏览模式暴露）的问题，提出 Huma：通过延迟隐蔽数据传输——参与绕过的网站先返回未修改内容，嵌入隐蔽数据的响应在后台准备、于客户端下一次请求时交付，从而消除便于指纹识别的时序异常；配合基于真实浏览活动的显式用户模拟器遵循预期浏览行为，并阻止敌手控制的网站关联通信端点，可扩展至内网审查场景的隐蔽通信。


### HyperMirage: Direct State Manipulation in Hybrid Virtual CPU Fuzzing.
- **作者**: Manuel Andreas, Fabian Specht, Marius Momeu
- **论文 PDF**: [HyperMirage: Direct State Manipulation in Hybrid Virtual CPU Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1763-paper.pdf)
- **解读**: 针对 hypervisor 虚拟 CPU 实现（最高特权级、最安全敏感组件）模糊测试需人工构造架构合法 VM 状态种子、接口访问需繁琐手工设置且吞吐低的问题，提出 HyperMirage：采用直接状态操纵（DSM）方法自动直接变异模糊测试期间 HV 所消费的 VM 状态视图，并将编译器级符号执行引擎扩展为首个可用于裸机目标的实现、集成进覆盖率引导的 HV 模糊器。对 Intel x86 上的 Xen 与 KVM 模糊测试显示，其虚拟 CPU 接口覆盖率比先前工作多 200%，并发现 Xen 9 个、KVM 2 个新 bug（均被维护者确认）。


### Icarus: Achieving Performant Asynchronous BFT with Only Optimistic Paths.
- **作者**: Xiaohai Dai, Yiming Yu, Sisi Duan, Rui Hao, Jiang Xiao, Hai Jin
- **论文 PDF**: [Icarus: Achieving Performant Asynchronous BFT with Only Optimistic Paths.](https://www.ndss-symposium.org/wp-content/uploads/2026-f60-paper.pdf)
- **解读**: 针对双路径异步 BFT（乐观路径+悲观 MVBA 路径）实现复杂、不利场景性能差，且串行路径切换困难、并行路径浪费带宽的问题，提出单路径异步 BFT 协议 Icarus：只用乐观路径，用旋转链机制保证活性——各节点并行广播区块链、按轮询方式轮流充当乐观路径，非故障节点链持续增长，一旦累积足够区块的链成为乐观路径即可提交。路径切换一致性由 Two-consecutive-validated-value Byzantine Agreement（tcv2-BA）对齐前一路径提交高度来保证，理论分析与实验均验证其正确性与高性能。


### Identifying Logical Vulnerabilities in QUIC Implementations.
- **作者**: Kaihua Wang, Jianjun Chen, Pinji Chen, Jianwei Zhuge, Jiaju Bai, Haixin Duan
- **论文 PDF**: [Identifying Logical Vulnerabilities in QUIC Implementations.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1777-paper.pdf)
- **解读**: 针对 QUIC 规范与实现的复杂性滋生微妙逻辑缺陷、既有测试工具集中于内存类漏洞、逻辑漏洞发现仍高度依赖人工审计的问题，提出黑盒模糊测试框架 MerCuriuzz 自动发现 QUIC 实现中的逻辑漏洞。对 16 个广泛使用的 QUIC 实现评估并发现 14 个此前未知的逻辑漏洞，影响 quiche、xquic、aioquic 等流行实现，可致服务器资源耗尽、服务崩溃或拒绝合法用户访问；将漏洞归为六类并提出缓解策略，11 个已被 Cloudflare、阿里云等厂商确认并奖励。


### Idioms: A Simple and Effective Framework for Turbo-Charging Local Neural Decompilation with Well-Defined Types.
- **作者**: Luke Dramko, Claire Le Goues, Edward J. Schwartz
- **论文 PDF**: [Idioms: A Simple and Effective Framework for Turbo-Charging Local Neural Decompilation with Well-Defined Types.](https://www.ndss-symposium.org/wp-content/uploads/2026-f795-paper.pdf)
- **解读**: 针对神经反编译无法为用户自定义复合类型提供定义、既有基准类型不够真实复杂的问题，提出 Idioms：一种简单、可泛化且有效的神经反编译方法，可将任意 LLM 微调为能同时生成反编译代码与恰当用户自定义类型定义的神经反编译器，并发布含更复杂真实类型的基准数据集 REALTYPE。在最难的既有基准 EXEBENCH 上准确率 54.4%（LLM4Decompile 46.3%、Nova 37.5%），在 REALTYPE 上性能至少提升 95%，取得神经反编译的最优结果。


### In-Context Probing for Membership Inference in Fine-Tuned Language Models.
- **作者**: Zhexi Lu, Hongliang Chi, Nathalie Baracaldo, Swanand Ravindra Kadhe, Yuseok Jeon, Lei Yu
- **论文 PDF**: [In-Context Probing for Membership Inference in Fine-Tuned Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f892-paper.pdf)
- **解读**: 针对黑盒成员推断攻击（MIA）依赖置信度或 token 似然、信号与样本固有属性（难度、稀有度）纠缠导致泛化差的问题，提出以训练动力学（优化中收益递减）为理论基础的新框架 ICP-MIA：将优化差距（Optimization Gap）作为成员性信号——收敛时成员样本剩余损失下降潜力最小、非成员仍保留显著优化空间；为在黑盒下估计该差距，提出免训练的上下文内探测（ICP）以构造的输入上下文模拟微调行为，含参考数据与自扰动（掩码/生成）两种策略。在三个任务与多个 LLM 上显著优于既有黑盒 MIA，尤其低假阳性率场景。


### Incident Response Planning Using a Lightweight Large Language Model with Reduced Hallucination.
- **作者**: Kim Hammar, Tansu Alpcan, Emil C. Lupu
- **论文 PDF**: [Incident Response Planning Using a Lightweight Large Language Model with Reduced Hallucination.](https://www.ndss-symposium.org/wp-content/uploads/2026-f358-paper.pdf)
- **解读**: 针对用前沿 LLM 做提示工程实现应急响应规划成本高且易幻觉的问题，提出三步方法：微调、信息检索与前向（lookahead）规划。作者证明该方法生成的响应计划幻觉概率有界，且在特定假设下可通过增加规划时间将概率任意压低；方法轻量、可运行于商用硬件。在文献报告的入侵日志上评估：恢复时间比前沿 LLM 最多缩短 22%，且可泛化到广泛的攻击类型与响应动作。


### Indicator of Benignity: An Industry View of False Positive in Malicious Domain Detection and its Mitigation.
- **作者**: Daiping Liu, Danyu Sun, Zhenhua Chen, Shu Wang, Zhou Li
- **论文 PDF**: [Indicator of Benignity: An Industry View of False Positive in Malicious Domain Detection and its Mitigation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1869-paper.pdf)
- **解读**: 针对恶意域名检测在生产中的误报（FP）规模未知且常被忽视、流行度域名列表不足以防 FP、业界重 IOC（失陷指标）轻 IOB（良性指标）的问题，用某大型安全厂商 6 年 FP 报告做首次测量研究，提出首个聚焦 IOB 检测的工作 IOBHunter：基于「许多 FP 的 IOB 可在互联网上找到」的关键发现，提出传递信任模型刻画 IOB 及其可信度，并利用 LLM 与思维链（CoT）实现。在含已验证 FP 的数据集上精度 99.22%、召回 68.6%；两个月真实部署识别出 4,338 个已确认 FP 与 2,051 个被攻陷域名。


### InverTune: A Backdoor Defense Method for Multimodal Contrastive Learning via Backdoor-Adversarial Correlation Analysis.
- **作者**: Mengyuan Sun, Yu Li, Yunjie Ge, Yuchen Liu, Bo Du, Qian Wang
- **论文 PDF**: [InverTune: A Backdoor Defense Method for Multimodal Contrastive Learning via Backdoor-Adversarial Correlation Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1666-paper.pdf)
- **解读**: 针对 CLIP 类多模态对比学习模型易受后门攻击、既有防御或强假设攻击者知识或需大量干净数据而不实用的问题，提出首个在最小攻击者假设下（无需目标先验、无需投毒数据集）工作的多模态后门防御 InverTune：通过对抗模拟暴露攻击签名、概率性识别目标标签；基于激活模式分析用梯度反演重建潜在触发器；最后用聚类引导的微调以少量任意干净数据擦除后门功能并保持模型能力。实验将 SOTA 攻击的平均攻击成功率降低 97.87%，干净精度仅下降 3.07%。


### IoTBec: An Accurate and Efficient Recurring Vulnerability Detection Framework for Black Box IoT devices.
- **作者**: Haoran Yang, Jiaming Guo, Shuangning Yang, Guoli Zhao, Qingqi Liu, Chi Zhang, Zhenlu Tan, Lixiao Shan, Qihang Zhou, Mengting Zhou, Jianwei Tai, Xiaoqi Jia
- **论文 PDF**: [IoTBec: An Accurate and Efficient Recurring Vulnerability Detection Framework for Black Box IoT devices.](https://www.ndss-symposium.org/wp-content/uploads/2026-f634-paper.pdf)
- **解读**: 针对黑盒 IoT 场景下依赖固件/源码的漏洞检测方法效率低的问题，提出固件与源码无关的复发漏洞检测框架 IoTBec：基于黑盒接口与已知漏洞信息构造漏洞接口签名（VIS）匹配目标设备的潜在复发漏洞，并将签名检测与 LLM 驱动的模糊测试深度融合——匹配后自动生成定向模糊载荷验证。在五大 IoT 厂商设备上比 SOTA 黑盒模糊测试多发现 7 倍以上漏洞，精度 100%、召回 93.37%；共检出 183 个漏洞、169 个获 CVE 编号，其中 53 个为新发现（平均 CVSS 3.x 8.61），LLM 驱动模糊测试另发现 25 个此前未知漏洞。


### Ipotane: Balancing the Good and Bad Cases of Asynchronous BFT.
- **作者**: Xiaohai Dai, Chaozheng Ding, Hai Jin, Julian Loss, Ling Ren
- **论文 PDF**: [Ipotane: Balancing the Good and Bad Cases of Asynchronous BFT.](https://www.ndss-symposium.org/wp-content/uploads/2026-s3-paper.pdf)
- **解读**: 针对异步 BFT 协议中 Abraxas（CCS'23）不利场景下因乐观路径失败检测慢而最坏延迟极高、ParBFT（CCS'23）因额外 ABA 实例而不利场景吞吐下降的问题，提出 Ipotane：同时运行 2-chain HotStuff 乐观路径与新原语双功能拜占庭协议（DBA）悲观路径（封装偏置 ABA 与 VABA 的功能）。副本按乐观/悲观路径何者更快向 DBA 输入 0/1，DBA 的 ABA 功能输出 1 及时信号乐观路径失败保证低延迟，VABA 功能持续产出悲观区块并在检测失败时提交最后两个悲观区块维持高吞吐，偏置属性保证提交安全性；实验验证各场景均高吞吐低延迟。


### IsolatOS: Detecting Double Fetch Bugs in COTS RTOS by Re-enabling Kernel Isolation.
- **作者**: Yingjie Cao, Xiaogang Zhu, Dean Sullivan, Haowei Yang, Lei Xue, Xian Li, Chenxiong Qian, Minrui Yan, Xiapu Luo
- **论文 PDF**: [IsolatOS: Detecting Double Fetch Bugs in COTS RTOS by Re-enabling Kernel Isolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s568-paper.pdf)
- **解读**: 针对 COTS RTOS 内核多次读取同一用户态内存且无一致性保证的双取（double-fetch）漏洞，静态分析无法检查专有内核、动态启发式依赖宽时间窗阈值而误报高且仿真开销大的问题，提出首个硬件支持的双取检测框架 ISOLATOS：利用现代 CPU 内核隔离特性以页错误捕获跨边界访问，记录每次用户态取数的页错误元数据，再基于系统调用生命周期判定同一系统调用内多次取数为双取漏洞。在 QNX、VxWorks、seL4 上运行开销较 SOTA 仿真检测器降低 79.3 倍，误报更低；发现 43 个此前未知漏洞（41 个获厂商确认、2 个 CVE），并在汽车系统中演示真实影响。


### Janus: Enabling Expressive and Efficient ACLs in High-speed RDMA Clouds.
- **作者**: Ziteng Chen, Menghao Zhang, Jiahao Cao, Xuzheng Chen, Qiyang Peng, Shicheng Wang, Guanyu Li, Mingwei Xu
- **论文 PDF**: [Janus: Enabling Expressive and Efficient ACLs in High-speed RDMA Clouds.](https://www.ndss-symposium.org/wp-content/uploads/2026-f721-paper.pdf)
- **解读**: 针对 RDMA 云中 QP 语义与高速传输特性使既有 ACL 表达与执行机制无法全面高效管控 RDMA 流量的现状，提出为 RDMA 云定制的 ACL 系统 Janus：设计带 QP 语义的 ACL 表达式识别 RDMA 连接，提供高层策略语言表达复杂 ACL 意图，并利用 DPU 做流量感知与架构特定优化实现线速检查与稳健策略更新。基于 NVIDIA BlueField-3 的开源原型在 200Gbps 真实 RDMA 测试床上以 <5µs 延迟实现线速吞吐。


### Kangaroo: A Private and Amortized Inference Framework over WAN for Large-Scale Decision Tree Evaluation.
- **作者**: Wei Xu, Hui Zhu, Yandong Zheng, Song Bian, Ning Sun, Hao Yuan, Dengguo Feng, Hui Li
- **论文 PDF**: [Kangaroo: A Private and Amortized Inference Framework over WAN for Large-Scale Decision Tree Evaluation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s892-paper.pdf)
- **解读**: 针对私有决策树评估（PDTE）通信与计算开销随树数/节点数/深度增长、大规模模型尤其 WAN 下低效的问题，提出基于打包全同态加密的私有分摊推理框架 Kangaroo：设计新型模型隐藏与编码方案，配合安全特征选择、不经意比较与安全路径评估协议，使开销随节点数/树数完全分摊；并引入同模型共享、延迟感知与自适应编码调整等优化。WAN 环境下比 SOTA 单轮交互方案快 14–59 倍，大规模推理快 3–44 倍；969 棵树、411,825 节点的随机森林约 60ms/棵（分摊）即可评估。


### Know Me by My Pulse: Toward Practical Continuous Authentication on Wearable Devices via Wrist-Worn PPG.
- **作者**: Wei Shao, Zequan Liang, Ruoyu Zhang, Ruijie Fang, Ning Miao, Ehsan Kourkchi, Setareh Rafatirad, Houman Homayoun, Chongzhou Fang
- **论文 PDF**: [Know Me by My Pulse: Toward Practical Continuous Authentication on Wearable Devices via Wrist-Worn PPG.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1087-paper.pdf)
- **解读**: 针对 ECG 生物识别侵入性强且采集不连续、既有 PPG 认证依赖高频（75–500 Hz）采样与复杂深度模型导致功耗计算开销大难以部署的问题，实现并评估首个真实智能手表连续认证系统 We-Be Band：采用低频（25 Hz）四通道 PPG，用带注意力的 Bi-LSTM 从 4 秒短窗口提取身份特征。在公开 PTTPPG 与自建 WeBe 数据集（26 人）上平均准确率 88.11%、宏 F1 0.88、FAR 0.48%、FRR 11.77%、EER 2.76%；25 Hz 比 512 Hz 省电 53%、比 128 Hz 省 19% 且不损失精度，而 20 Hz 性能骤降，表明 25 Hz 是实用下限；仅静息数据训练在运动中失效，多样活动训练提升鲁棒性。


### KnowHow: Automatically Applying High-Level CTI Knowledge for Interpretable and Accurate Provenance Analysis.
- **作者**: Yuhan Meng, Shaofei Li, Jiaping Gui, Peng Jiang, Ding Li
- **论文 PDF**: [KnowHow: Automatically Applying High-Level CTI Knowledge for Interpretable and Accurate Provenance Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s199-paper.pdf)
- **解读**: 针对 CTI 报告中的高层自然语言知识（如 ATT&CK）与低层系统事件之间存在语义鸿沟、难以自动应用于溯源分析的问题，提出 CTI 知识驱动的在线溯源分析方法 KnowHow：核心是新的攻击知识表示 gIoC（general Indicator of Compromise），刻画攻击的主体、客体与动作；通过将文件路径等系统标识符提升为自然语言词项，将系统事件匹配到 gIoC 再匹配到自然语言描述的技术，最后对攻击步骤做时序逻辑推理检测 APT。在开源与工业数据集上准确检测全部 16 个 APT 攻击活动（既有方法均大量误报），节点级误报最多降低 90% 且召回更高，并对未知与模仿攻击具鲁棒性。


### LatticeBox: A Hardware-Software Co-Designed Framework for Scalable and Low-Latency Compartmentalization.
- **作者**: Zhanpeng Liu, Chenyang Li, Wende Tan, Yuan Li, Xinhui Han, Xi Cao, Yong Xie, Chao Zhang
- **论文 PDF**: [LatticeBox: A Hardware-Software Co-Designed Framework for Scalable and Low-Latency Compartmentalization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f515-paper.pdf)
- **解读**: 针对现有硬件隔离技术在可扩展性、域切换延迟与安全保证上的不足（如 Intel MPK 的 WRPKRU 权限切换指令可被不可信代码滥用）的问题，提出软硬件协同框架 LatticeBox：采用基于格的访问控制模型，将权限与内存区域编码为紧凑的分层 N 位向量，硬件架构使域切换延迟降到单周期并天然杜绝权限切换指令滥用；定制指令 lp_land 实施严格的跨域控制流完整性，阻止未授权间接调用。在 RISC-V BOOM 核上实现，域切换比 Intel MPK 快至 180 倍；真实负载仅适度影响性能（WebAssembly 运行时慢 2%、隔离 Linux 内核模块的 ApacheBench 吞吐低 3%）。


### Learning from Leakage: Database Reconstruction from Just a Few Multidimensional Range Queries.
- **作者**: Peijie Li, Huanhuan Chen, Kaitai Liang, Evangelia Anna Markatou
- **论文 PDF**: [Learning from Leakage: Database Reconstruction from Just a Few Multidimensional Range Queries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f935-paper.pdf)
- **解读**: 针对可搜索加密（SE）泄漏的危急性问题、而一维范围查询重建攻击难以扩展到高维数据集、既有方法要么要求过多查询信息要么在稀疏库中重建质量差的问题，提出 REMIN：利用范围查询的访问与搜索模式泄漏，以无监督表示学习把查询共现频率转化为几何信号，使攻击者推断加密记录间的相对空间关系，在最小泄漏下准确可扩展地重建高维数据集。另提出主动变体 REMIN-P，通过注入少量辅助锚点的投毒策略显著提升稀疏/边界区域重建质量；相比 SOTA 重建攻击 MSE 最多降低 50%，投毒可再平均降低 50%。


### Les Dissonances: Cross-Tool Harvesting and Polluting in Pool-of-Tools Empowered LLM Agents.
- **作者**: Zichuan Li, Jian Cui, Xiaojing Liao, Luyi Xing
- **论文 PDF**: [Les Dissonances: Cross-Tool Harvesting and Polluting in Pool-of-Tools Empowered LLM Agents.](https://www.ndss-symposium.org/wp-content/uploads/2026-f577-paper.pdf)
- **解读**: 针对多工具 LLM 智能体在工具安全管理、兼容性、依赖关系与任务工作流控制流保护上的挑战，开展多工具 LLM 智能体任务控制流的首次系统性安全分析，识别出新威胁「跨工具收割与投毒」（XTHP）：多个攻击向量先劫持智能体任务的正常控制流，再收集并污染 LLM 智能体系统中的机密或私人信息。开发动态扫描工具 Chord 自动检测易受 XTHP 攻击的真实智能体工具；对 LangChain 与 Llama-Index 两大框架的 66 个真实工具评估，75% 易受 XTHP 攻击，揭示该威胁的普遍性。


### Light into Darkness: Demystifying Profit Strategies Throughout the MEV Bot Lifecycle.
- **作者**: Feng Luo, Zihao Li, Wenxuan Luo, Zheyuan He, Xiapu Luo, Zuchao Ma, Shuwei Song, Ting Chen
- **论文 PDF**: [Light into Darkness: Demystifying Profit Strategies Throughout the MEV Bot Lifecycle.](https://www.ndss-symposium.org/wp-content/uploads/2026-s506-paper.pdf)
- **解读**: 针对 MEV 机器人持续竞争提取最大可提取价值、损害区块链共识安全与效率、而其具体策略类型与分布仍未知的问题，开发 APOLLO 分析机器人整个生命周期的细粒度盈利策略，开展首次系统性研究。对 2,052 个 MEV 机器人的大规模分析产出多项新洞察：引入野生机器人采用的 20 种代码级策略；首次对智能合约做去混淆以揭示混淆代码中隐藏的策略；并发现五类为 MEV 机器人带来盈利机会的交易。


### Light2Lie: Detecting Deepfake Images Using Physical Reflectance Laws.
- **作者**: Kavita Kumari, Sasha Behrouzi, Alessandro Pegoraro, Ahmad-Reza Sadeghi
- **论文 PDF**: [Light2Lie: Detecting Deepfake Images Using Physical Reflectance Laws.](https://www.ndss-symposium.org/wp-content/uploads/2026-s923-paper.pdf)
- **解读**: 针对深度伪造检测方法难以泛化到未见生成模型、缺乏物理依据、易受自适应攻击且可解释性有限的问题，提出物理增强的检测框架 Light2Lie：利用镜面反射原理（菲涅尔反射模型）揭示生成模型难以复现的光-表面相互作用不一致性——先用神经网络估计表面基底反射率，再推导微面元启发的高光响应图编码真实与合成图像间的几何光学差异，作为特征图输入二级分类器；反馈精炼机制用分类误差更新基底反射率输出，使物理建模与学习目标紧密耦合。在多个深度伪造数据集上对未见生成模型取得最高 74% 精度，泛化性优于 SOTA 基线。


### Lightening the Load: A Cluster-Based Framework for A Lower-Overhead, Provable Website Fingerprinting Defense.
- **作者**: Khashayar Khajavi, Tao Wang
- **论文 PDF**: [Lightening the Load: A Cluster-Based Framework for A Lower-Overhead, Provable Website Fingerprinting Defense.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1760-paper.pdf)
- **解读**: 针对网站指纹（WF）防御中正则化类（固定填充规则塑形流量）与超序列类（将流量隐藏在预定义模式中）两类方案的权衡，提出统一的自适应 WF 防御框架：从流量轨迹提取行为模式并聚类为 (k,l)-多样的匿名集，用早期时间序列分类器（改造自 ECDIRE）从保守的全局正则化参数切换到更轻的集合特定参数。实例化为 Adaptive Tamaraw（保留原信息论保证、按簇分配填充参数）：高隐私模式下可将任意攻击者精度压到 30% 以下，高效模式下总开销较经典 Tamaraw 降低 99 个百分点。


### Lightweight Internet Bandwidth Allocation and Isolation with Fractional Fair Shares.
- **作者**: Marc Wyss, Yih-Chun Hu, Vincent Lenders, Roland Meier, Adrian Perrig
- **论文 PDF**: [Lightweight Internet Bandwidth Allocation and Isolation with Fractional Fair Shares.](https://www.ndss-symposium.org/wp-content/uploads/2026-f23-paper.pdf)
- **解读**: 针对公网公平带宽分配难题（不同拥塞控制算法并存时公平性差、体量型 DDoS 可完全饿死合法流量、路由器强制分配方案或易部署但不安全或强隔离但部署复杂）的问题，提出基于每流分数公平份额（FFS）的新公平模型：流在路径各节点的 FFS 以数据包标签表示并沿转发路径更新，配合概率转发实现低开销可扩展的流隔离。FFS 首个同时实现低实现/部署开销与有效带宽隔离、抗源地址伪造与体量型 DDoS；对 15 种 CCA 有效隔离并保持低延迟抖动，商用硬件上 160 Gbps 线速；安全分析证明对每条流保证非零带宽下界，并给出提供准确安全速率反馈的扩展。


### Limitless Scalability: A High-Throughput and Replica-Agnostic BFT Consensus.
- **作者**: Chenyu Zhang, Xiulong Liu, Hao Xu, Haochen Ren, Muhammad Shahzad, Guyue Liu, Keqiu Li
- **论文 PDF**: [Limitless Scalability: A High-Throughput and Replica-Agnostic BFT Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-f101-paper.pdf)
- **解读**: 针对传统星型领导拓扑 BFT 随副本数增长性能骤降、多层拓扑方案或因多项式扇出限制容错或受拓扑深度制约吞吐而扩展增益有限的问题，提出 Tide：首个随副本数增长保持稳健性能的 BFT，核心是对数扇出拓扑加高并行流水线——以冗余连接作为关键洞察在保持韧性的同时降低扇出，流水线中层间交互动态决定提议并行度，使吞吐与拓扑深度解耦。100 台云服务器实验：副本从 100 扩到 1,000 时 SOTA 协议吞吐下降 65–90%、延迟增 50 倍，而 Tide 保持约 50ktps（高出 5 倍以上）、延迟 0.3–0.4s。


### LinkGuard: A Lightweight State-Aware Runtime Guard Against Link Following Attacks in Windows File System.
- **作者**: Bocheng Xiang, Yuan Zhang, Hao Huang, Fengyu Liu, Youkun Shi
- **论文 PDF**: [LinkGuard: A Lightweight State-Aware Runtime Guard Against Link Following Attacks in Windows File System.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2943-paper.pdf)
- **解读**: 针对 Windows 文件系统链接跟随（LF）攻击利用精心构造的符号链接链将良性文件操作重定向到受保护文件、既有防御或依赖重量级建模或兼容性与适用性差且无全面防护的问题，提出轻量状态感知运行时防护 LinkGuard：两阶段设计，第一阶段做动态主体过滤，只监控链接链创建与跟随相关的文件操作与主体以提升效率；第二阶段用基于 FSM 的规则匹配精确防御。在五个代表性 Windows 系统验证兼容性：对 70 个真实漏洞全部单步攻击与 95.45% 多步攻击均成功缓解、良性操作零误报，微基准平均开销 1%、真实应用负载 3.4%，良性文件操作仅增约 5ms 延迟。


### LLMBisect: Breaking Barriers in Bug Bisection with A Comparative Analysis Pipeline.
- **作者**: Zheng Zhang, Haonan Li, Xingyu Li, Hang Zhang, Zhiyun Qian
- **论文 PDF**: [LLMBisect: Breaking Barriers in Bug Bisection with A Comparative Analysis Pipeline.](https://www.ndss-symposium.org/wp-content/uploads/2026-s990-paper.pdf)
- **解读**: 针对补丁式 bug 二分（bisection）识别引入提交（BIC）的多重障碍——假设 BIC 与补丁修改同一函数（常不成立）、只依赖代码变更而忽略蕴含漏洞信息的提交消息、基于简单启发式且缺乏漏洞逻辑分析——的问题，观察到 LLM 能同时理解补丁/提交中的文本与代码，提出多阶段流水线：充分利用补丁信息、在上下文中比较多个候选提交、并通过一系列下选步骤渐进收窄候选。评估显示准确率比 SOTA 方案高 38% 以上，且多阶段流水线本身至关重要——较基线 LLM 二分方法提升 60%。


### LOKI: Proactively Discovering Online Scam Websites by Mining Toxic Search Queries.
- **作者**: Pujan Paudel, Gianluca Stringhini
- **论文 PDF**: [LOKI: Proactively Discovering Online Scam Websites by Mining Toxic Search Queries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s184-paper.pdf)
- **解读**: 针对电商诈骗网站检测下游系统准确但候选发现困难——依赖用户举报反应慢、主动搜索引擎查询方案覆盖率低且难泛化到新诈骗类型——的问题，提出 LOKI：识别更可能返回高比例欺诈网站的搜索引擎查询。核心是基于特权信息学习（LUPI）与搜索引擎结果页（SERP）特征蒸馏的关键词评分模型；在 10 大诈骗类别上严格验证，发现能力较启发式与数据驱动基线提升 20.58 倍；仅凭 1,663 个已知诈骗站点种子即发现 52,493 个此前未报告的诈骗，并能泛化到未见类别。


### Looma: A Low-Latency PQTLS Authentication Architecture for Cloud Applications.
- **作者**: Xinshu Ma, Michio Honda
- **论文 PDF**: [Looma: A Low-Latency PQTLS Authentication Architecture for Cloud Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-f74-paper.pdf)
- **解读**: 针对后量子认证（尤其云环境高频握手的双向 TLS）性能开销高的问题，提出 Looma 后量子认证架构：将认证拆分为快速的路径上签名/验签与慢速的路径外异步预计算，在不牺牲安全性的前提下降低握手延迟；集成进 TLS 1.3 后，PQTLS 握手延迟较 Dilithium-2 基线最多降低 44%，证明其适用于云环境规模化后量子安全通信。


### Losing the Beat: Understanding and Mitigating Desynchronization Risks in Container Isolation.
- **作者**: Zhi Li, Zhen Xu, Weijie Liu, XiaoFeng Wang, Hai Jin, Zheli Liu
- **论文 PDF**: [Losing the Beat: Understanding and Mitigating Desynchronization Risks in Container Isolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1381-paper.pdf)
- **解读**: 针对容器隔离依赖命名空间与 cgroups 高度协同、而 serverless 等新范式要求跨命名空间资源共享从而削弱隔离模型、诱发命名空间-cgroups 失同步（NCD）漏洞的问题，系统研究其根因与影响：发现流行容器工具均存在 NCD 风险（发现 4 个新漏洞与 1 个 bug），根本原因是命名空间共享扩大隔离边界、可能与 cgroups 限制相抵触而破坏两者协同防护。提出内核级方案统一命名空间与 cgroups 在容器实例资源监控上的碎片化职责，将命名空间的资源管理与 cgroups 的资源限制绑定并明确协同策略；评估表明有效缓解 NCD 风险且对 Linux 内核、主流容器工具与真实应用开销可忽略、完全兼容。


### Mapping the Cloud: A Mixed-Methods Study of Cloud Security and Privacy Configuration Challenges.
- **作者**: Sumair Ijaz Hashmi, Shafay Kashif, Lea Gröber, Katharina Krombholz, Mobin Javed
- **论文 PDF**: [Mapping the Cloud: A Mixed-Methods Study of Cloud Security and Privacy Configuration Challenges.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1302-paper.pdf)
- **解读**: 针对云服务配置错误持续成为安全隐私事件主因、云平台配置复杂性的问题，分析约 251,900 条 2008–2024 年与安全隐私相关的 Stack Overflow 帖子，用主题建模与定性分析系统地将云用例映射到其安全隐私配置挑战，描绘云运维人员面临挑战的全景。发现技术与以人为中心两类问题（文档不足、缺乏适配运维人员环境的上下文感知工具），其中认证与访问控制挑战出现在所有用例中、贯穿云部署/集成/维护的几乎每个阶段；结论是开发者安全配置云服务需要可用、定制、上下文敏感的支持工具与资源。


### Memory Backdoor Attacks on Neural Networks.
- **作者**: Eden Luzon, Guy Amit, Roy Weiss, Torsten Krauß, Alexandra Dmitrienko, Yisroel Mirsky
- **论文 PDF**: [Memory Backdoor Attacks on Neural Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1870-paper.pdf)
- **解读**: 针对神经网络训练数据专有、恶意联邦学习（FL）服务器可借此窃取客户端训练样本的问题，提出新颖的训练时后门数据集提取方法：恶意服务器通过简单索引过程系统且确定性地提取客户端完整训练样本。与先前概率重建或幻觉方法不同，该方法保证精确恢复、可精确控制记忆哪些样本及数量，且容量高、鲁棒；被感染模型收到基于模式的索引触发器时输出数据样本，通过提取补丁再重组应对小输出尺寸，只需对训练代码做可规避客户端验证的小修改。在分类器、分割模型与 LLM 上评估：可恢复数千敏感样本且任务性能影响极小，医学分割数据集仅 3% 效用下降即可被提取，多轮 FL 后可窃取客户端整个数据集。


### Memory Band-Aid: A Principled Rowhammer Defense-in-Depth.
- **作者**: Carina Fiedler, Jonas Juffinger, Sudheendra Raghav Neela, Martin Heckel, Hannes Weissteiner, Abdullah Giray Yaglikçi, Florian Adamsky, Daniel Gruss
- **论文 PDF**: [Memory Band-Aid: A Principled Rowhammer Defense-in-Depth.](https://www.ndss-symposium.org/wp-content/uploads/2026-s156-paper.pdf)
- **解读**: 针对硬件 Rowhammer 缓解措施部署周期长、基本无更新能力、精化攻击已反复绕过它们的问题，提出 Memory Band-Aid 纵深防御方案：在内存控制器中引入每线程与每存储体（bank）的 DRAM 访问速率限制，确保达不到触发 Rowhammer 位翻转所需的最小行激活次数；它不替代长期高效硬件缓解，而是在特定系统代际硬件缓解不足时作为可启用的第二道防线。PoC 基于 Ubuntu 在 2 台 Intel 与 2 台 AMD 系统上验证（因现硬件缺每存储体限制而基于全局带宽限制）；估算完整实现（含小幅硬件改动）在 Phoronix 宏基准上开销 0–9.4%，DRAM 压力微基准慢 1–5.1 倍，且开销只作用于不可信的受限工作负载。


### MEVisor: High-Throughput MEV Discovery in DEXs with GPU Parallelism.
- **作者**: Weimin Chen, Xiapu Luo
- **论文 PDF**: [MEVisor: High-Throughput MEV Discovery in DEXs with GPU Parallelism.](https://www.ndss-symposium.org/wp-content/uploads/2026-f93-paper.pdf)
- **解读**: 针对 DEX 中最大可提取价值（MEV）发现因 12 秒出块间隔与巨大搜索空间而需高吞吐、现有工具依赖 CPU 执行（频繁状态分叉与慢速 DEX 执行）吞吐低的问题，首次用 GPU 并行加速套利与三明治策略的 MEV 搜索：将 MEV 机器人编译为 GPU 应用、启动数千 GPU 线程并行搜索利润，并设计 GPU 上模拟交易的 cheatcodes、降低 GPU 内存的内存管理器与提升输入多样性的策略感知变异。原型 MeVisor 在 3,941 个真实 MEV 案例上实现 330–510 万交易/秒（较 CPU 基线快 10 万倍）；2025 年 Q1 大规模研究中估算每区块 2–14 笔 MEV 交易、最高约 110 万美元 MEV 利润。

### MIMIR: Masked Image Modeling for Mutual Information-based Adversarial Robustness.
- **作者**: Xiaoyun Xu, Shujian Yu, Zhuoran Liu, Stjepan Picek
- **论文 PDF**: [MIMIR: Masked Image Modeling for Mutual Information-based Adversarial Robustness.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1813-paper.pdf)
- **解读**: 针对视觉 Transformer（ViT）易受逃逸攻击且现有对抗训练（AT）方法（如 Generalist、DBAT）与其架构不兼容的问题，论文对 ViT 自监督预训练中的互信息（MI）进行了理论分析，证明对抗样本与其在自编码器中的隐表示之间的 MI 应通过推导的 MI 界加以约束。据此提出 MIMIR，一种以掩码图像建模结合 MI 惩罚进行对抗预训练的自监督 AT 方法。在 CIFAR-10、Tiny-ImageNet 和 ImageNet-1K 上的实验表明 MIMIR 稳定提升自然准确率与鲁棒准确率并超过现有 SOTA，且对未见攻击、常见数据损坏以及掌握防御全部信息的自适应攻击均表现出更强的鲁棒性。


### MinBucket MPSI: Breaking the Max-Size Bottleneck in Multi-Party Private Set Intersection.
- **作者**: Binbin Tu, Boyudong Zhu, Yang Cao, Yu Chen
- **论文 PDF**: [MinBucket MPSI: Breaking the Max-Size Bottleneck in Multi-Party Private Set Intersection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f182-paper.pdf)
- **解读**: 针对多方隐私集合求交（MPSI）协议通信复杂度随较大集合规模线性增长、难以支撑异构输入规模实际部署的问题，论文提出基于批量成员条件随机数生成与联合私有相等性测试的新型框架，并实例化出两个通信复杂度与小集合规模线性、与大集合规模对数的 MPSI 协议，分别抵御任意数量合谋方与至多 T-2 个合谋方。进一步提出联合置换私有相等性测试并构造 MPSI-Card 框架，得到同等通信效率的基数版本。在 LAN/WAN 实验（5 个大集合方 2^20、5 个小集合方 2^10、10Mbps）中协议仅需 12.2MB 通信与约 130 秒运行时间，相比现有 SOTA 实现最多 157 倍通信降低与 12.7 倍加速。


### Mirage: Private, Mobility-based Routing for Censorship Evasion.
- **作者**: Zachary Ratliff, RuoxingYang, Avery Bai, Harel Berger, Micah Sherr, James Mickens
- **论文 PDF**: [Mirage: Private, Mobility-based Routing for Censorship Evasion.](https://www.ndss-symposium.org/wp-content/uploads/2026-s237-paper.pdf)
- **解读**: 针对 Tor 等匿名网络依赖中心化互联网基础设施、易被大规模封锁的问题，论文提出 MIRAGE，一种基于移动性的隐私保护消息系统，采用区域（district）级路由按人群高层移动模式概率性转发消息，并用本地差分隐私保护用户的移动模式，避免路由决策泄露个人位置历史。作者在开源模拟器 Cadence 中实现 MIRAGE，并与传统流行病路由、随机游走路由及现有隐私保护地理路由对比。基于行人轨迹与出租车 GPS 数据的实验表明，MIRAGE 相比流行病路由显著降低消息开销，相比概率泛洪提升投递率，同时提供更强的隐私保证。


### Mobius: Enabling Byzantine-Resilient Single Secret Leader Election with Uniquely Verifiable State.
- **作者**: Hanyue Dou, Peifang Ni, Yingzi Gao, Jing Xu
- **论文 PDF**: [Mobius: Enabling Byzantine-Resilient Single Secret Leader Election with Uniquely Verifiable State.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2407-paper.pdf)
- **解读**: 论文发现单秘密领导者选举（SSLE）协议存在一类新攻击——状态唯一性（state uniqueness）攻击：恶意领导者提出多个可公开验证的状态，破坏后续选举的唯一性，并高概率导致上层协议活性等安全属性被违反，且该漏洞可推广到现有 SSLE 构造。据此提出不依赖额外信任假设的通用 SSLE 协议 Mobius，通过创新的近似唯一随机化机制阻止每轮生成多个可验证状态并在连续执行间保持唯一状态。作者在通用可组合框架下给出安全分析，并实现原型验证：Mobius 增强安全性的同时显著降低通信复杂度，注册阶段开销降低超过 80%。


### MUTATO: Enhancing Fuzz Drivers with Adaptive API Option Mutation.
- **作者**: Shuangxiang Kan, Xiao Cheng, Yuekang Li
- **论文 PDF**: [MUTATO: Enhancing Fuzz Drivers with Adaptive API Option Mutation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s820-paper.pdf)
- **解读**: 针对现代覆盖率引导模糊测试的瓶颈在于 fuzz driver 构建、尤其是库 API 选项参数探索不足的问题，论文提出 MUTATO，一种在 driver 层面系统化自适应地同时变异输入数据与选项参数的多维增强方法，采用覆盖率引导的 epsilon-greedy 策略，无需侵入式修改模糊器，可无缝对接手工与自动生成的 driver。论文还设计了选项参数模糊语言 OPFL 指导 driver 增强。在 10 个广泛使用的 C/C++ 库上的实验表明，MUTATO 增强的 driver 相比原始 AFL++ 与 LibFuzzer driver 平均分别提升 14% 和 13% 的代码覆盖率，并发现 12 个此前未知漏洞（含 3 个 CVE），其中 4 个漏洞在 5 小时内发现于 OSS-Fuzz 投入超 18,060 小时仍未检测到的 API 中。


### MVP-ORAM: a Wait-free Concurrent ORAM for Confidential BFT Storage.
- **作者**: Robin Vassantlal, Hasan Heydari, Bernardo Ferreira, Alysson Bessani
- **论文 PDF**: [MVP-ORAM: a Wait-free Concurrent ORAM for Confidential BFT Storage.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1809-paper.pdf)
- **解读**: 针对现有 ORAM 在支持并发客户端与拜占庭容错（BFT）方面的局限，论文提出 MVP-ORAM，第一个支持并发易失败客户端的无等待（wait-free）ORAM 协议，无需可信代理及基于客户端间通信或分布式锁的并发控制，客户端可并发请求并即时合并冲突更新。由于等待自由与碰撞自由在异步并发 ORAM 服务中本质上不可兼得，作者定义了依赖应用负载与并发客户端数目的较弱不觉性概念，并证明在客户端偏斜块访问的实际场景下协议安全。MVP-ORAM 可无缝集成到现有机密 BFT 数据存储，形成首个 BFT ORAM 构造，原型在云环境中每秒可处理数百次 4KB 访问。


### MVPNalyzer: An Investigative Framework for Auditing the Security & Privacy of Mobile VPNs.
- **作者**: Wayne Wang, Aaron Ortwein, Enrique Sobrados, Robert Stanley, Piyush Kumar Sharma, Afsah Anwar, Roya Ensafi
- **论文 PDF**: [MVPNalyzer: An Investigative Framework for Auditing the Security & Privacy of Mobile VPNs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1573-paper.pdf)
- **解读**: 针对移动 VPN 应用因拦截用户流量而处于特权位置、却长期缺乏审计的问题，论文提出 MVPNalyzer，一个可扩展的 Android VPN 应用分析框架，并应用于 Google Play 上 281 个热门 VPN 应用。审计发现大量关键问题：61 个应用明文传输数据（其中 5 个明文发送敏感 VPN 配置文件，可被攻击者劫持隧道连接）、29 个应用将用户流量（含 DNS）泄漏到隧道外、169 个应用未混淆流量、76 个应用传输用于追踪的广告 ID、107 个应用的 VPN 配置文件未落实最佳安全实践，这些应用合计拥有数亿安装量，反映出开发者安全实践与维护的普遍缺失。


### NetCap: Data-Plane Capability-Based Defense Against Token Theft in Network Access.
- **作者**: Osama Bajaber, Bo Ji, Peng Gao
- **论文 PDF**: [NetCap: Data-Plane Capability-Based Defense Against Token Theft in Network Access.](https://www.ndss-symposium.org/wp-content/uploads/2026-f273-paper.pdf)
- **解读**: 针对企业网络访问控制中令牌被主机内恶意进程窃取并冒充合法用户的问题，论文提出 NetCap，一种防止攻击者使用窃取令牌访问未授权资源的防御机制，核心思路是引入与授权进程绑定的不可伪造进程级能力（capability），并持续嵌入进程网络流量中供目标资源验证且频繁刷新。该绑定保证即使令牌被窃取也无法通过认证。为支撑高请求量，NetCap 基于可编程交换机与 eBPF 提出新颖的数据面设计，通过多项优化支持能力的在线生成与嵌入，以线速处理大规模流量且开销极低，实验验证其在多种协议与真实应用下保持线速性能并有效抵御令牌窃取攻击。


### NetRadar: Enabling Robust Carpet Bombing DDoS Detection.
- **作者**: Junchen Pan, Lei Zhang, Xiaoyong Si, Jie Zhang, Xinggong Zhang, Yong Cui
- **论文 PDF**: [NetRadar: Enabling Robust Carpet Bombing DDoS Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2118-paper.pdf)
- **解读**: 针对地毯式轰炸（carpet bombing）DDoS 攻击通过分散到多个服务器、压低单流恶意流量以逃避检测的问题，论文提出 NetRadar，一种服务器-网关协作架构的 DDoS 检测器，汇聚受害者网络内的流量与服务器侧特征并进行跨服务器分析以定位受害服务器。为支持服务器辅助检测，论文引入兼容多种服务的通用服务器侧特征集、处理运行时特征失配的鲁棒训练方法，以及利用地毯式流量相似性并降低计算开销的高效跨服务器入站流量分析方法。在真实与模拟数据集上的评估表明 NetRadar 优于现有方案，在所有地毯式轰炸检测场景下准确率超过 94%。


### NeuroStrike: Neuron-Level Attacks on Aligned LLMs.
- **作者**: Lichao Wu, Sasha Behrouzi, Mohamadreza Rostami, Maximilian Thang, Stjepan Picek, Ahmad-Reza Sadeghi
- **论文 PDF**: [NeuroStrike: Neuron-Level Attacks on Aligned LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s660-paper.pdf)
- **解读**: 针对现有针对已对齐 LLM 的越狱攻击依赖试错、缺乏跨模型泛化性与可靠性问题，论文提出 NeuroStrike，利用对齐技术引入的根本弱点——稀疏、专门的安全神经元——实施攻击。白盒场景下通过前馈激活分析识别安全神经元并在推理时剪除（少于 0.6% 神经元即对 20 余个开源 LLM 取得平均 76.9% 攻击成功率）；黑盒场景下提出首个 LLM 画像攻击，利用安全神经元可迁移性在开源替代模型上训练对抗提示生成器再部署到黑盒模型。NeuroStrike 可泛化到 4 个多模态 LLM（不安全图像输入 100% 成功率），跨架构迁移使微调与蒸馏模型攻击成功率升至 78.5% 与 77.7%，黑盒攻击在包括 Gemini 家族在内的 5 个黑盒模型上平均达 63.7%。


### NEXUS: Towards Accurate and Scalable Mapping between Vulnerabilities and Attack Techniques.
- **作者**: Ehsan Khodayarseresht, Suryadipta Majumdar, Serguei A. Mokhov, Mourad Debbabi
- **论文 PDF**: [NEXUS: Towards Accurate and Scalable Mapping between Vulnerabilities and Attack Techniques.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2926-paper.pdf)
- **解读**: 针对 CVE 记录缺少可利用上下文、MITRE ATT&CK 框架的 TTP 又未与具体漏洞关联的问题，论文提出 NEXUS，一个自动将 CVE 描述映射到 ATT&CK TTP 的框架，针对性地解决高质量数据集缺乏、数据分布不均衡、从非结构化 CVE 描述中提取对抗行为困难以及缺少自适应纠错机制等挑战。作者构建了覆盖 208 个 TTP 与 92K+ 个 CVE 的新数据集，评估显示 NEXUS 在 CVE-to-TTP 映射上取得最高 97.94% 的 F1 分数并可直接处理新 CVE 条目，而现有工作最高仅 67.68%。


### ObliInjection: Order-Oblivious Prompt Injection Attack to LLM Agents with Multi-source Data.
- **作者**: Reachal Wang, Yuqi Jia, Neil Zhenqiang Gong
- **论文 PDF**: [ObliInjection: Order-Oblivious Prompt Injection Attack to LLM Agents with Multi-source Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f702-paper.pdf)
- **解读**: 针对多源输入场景下攻击者只控制部分数据源、且不知道各源片段在输入中排列顺序，导致现有提示注入攻击成功率受限的问题，论文提出 ObliInjection，首个面向多源输入 LLM 应用与代理的提示注入攻击，包含两项技术：order-oblivious loss 量化 LLM 无论干净与污染片段如何排列都完成攻击者任务的可能性，以及针对该损失优化的 orderGCG 算法。在覆盖不同应用领域的三组数据集与 12 个 LLM 上的实验表明，即使仅污染 6-100 个片段中的 1 个，攻击仍然高度有效。


### OCCUPY+PROBE: Cross-Privilege Branch Target Buffer Side-Channel Attacks at Instruction Granularity.
- **作者**: Kaiyuan Rong, Junqi Fang, Haixia Wang, Dapeng Ju, Dongsheng Wang
- **论文 PDF**: [OCCUPY+PROBE: Cross-Privilege Branch Target Buffer Side-Channel Attacks at Instruction Granularity.](https://www.ndss-symposium.org/wp-content/uploads/2026-s925-paper.pdf)
- **解读**: 针对现有 BTB 侧信道攻击或因跨特权隔离无法从用户态泄漏内核控制流、或分支监测空间分辨率有限的问题，论文逆向分析 Intel 处理器与偏移相关的 BTB 更新机制，发现用户态创建的 BTB 条目可被内核态条目直接替换且与替换策略和硬件隔离无关，据此提出 Occupy+Probe，一种基于驱逐的 BTB 侧信道攻击，从用户态直接暴露内核控制流行为。该方法无需攻击者与受害者共享条目，并以指令级粒度监测分支，超越现有驱逐型 BTB 侧信道的空间分辨率。实验在多种 Intel 处理器上验证了跨特权级高分辨率泄漏，并以 Linux 内核 Crypto API 为案例展示其实际威胁，还首次实现了提取内核分支 tag 值以打破 KASLR 的能力。


### Odysseus: Jailbreaking Commercial Multimodal LLM-integrated Systems via Dual Steganography.
- **作者**: Songze Li, Jiameng Cheng, Yiming Li, Xiaojun Jia, Dacheng Tao
- **论文 PDF**: [Odysseus: Jailbreaking Commercial Multimodal LLM-integrated Systems via Dual Steganography.](https://www.ndss-symposium.org/wp-content/uploads/2026-f808-paper.pdf)
- **解读**: 针对现有越狱方法对采用内容过滤器的商业 MLLM 集成系统效果有限的问题，论文揭示这些安全过滤器依赖“恶意内容必须在输入或输出中显式可见”的关键假设，而该假设在多模态系统中不再成立。据此提出 Odysseus，一种通过双重隐写术将恶意查询与响应隐蔽嵌入良性图片的越狱范式，流程分为恶意查询编码、隐写嵌入、模型交互与响应提取四个阶段。在基准数据集上的实验表明 Odysseus 成功越狱 GPT-4o、Gemini-2.0-pro、Gemini-2.0-flash 与 Grok-3 等商业 MLLM 集成系统，攻击成功率最高达 99%，暴露了现有防御的盲区并呼吁重新审视跨模态安全。


### On Borrowed Time: Measurement-Informed Understanding of the NTP Pool's Robustness to Monopoly Attacks.
- **作者**: Robert Beverly, Erik C. Rye
- **论文 PDF**: [On Borrowed Time: Measurement-Informed Understanding of the NTP Pool's Robustness to Monopoly Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f541-paper.pdf)
- **解读**: 针对广泛使用的 NTP Pool 因管理分散、基于志愿者服务器且被全球 IoT 与基础设施设备重度依赖而成为垄断攻击目标的问题，论文收集了首个直接、非推断且全面的 NTP Pool 数据，包括九个月的纵向服务器与账户成员、服务器配置、时间质量、别名及全球查询流量负载，发现 15k+ 台服务器。通过分析地址别名、账户与网络连通性，发现池中活跃服务器仅 19.7% 完全独立；进一步表明，掌握这些数据的攻击者只需 10 个或更少的恶意 NTP 服务器即可在 90% 的国家捕获 NTP Pool 的绝大部分流量，并据此提出多条提升池鲁棒性的改进途径。


### On the Security Risks of Memory Adaptation and Augmentation in Data-plane DoS Mitigation.
- **作者**: Hocheol Nam, Daehyun Lim, Huancheng Zhou, Guofei Gu, Min Suk Kang
- **论文 PDF**: [On the Security Risks of Memory Adaptation and Augmentation in Data-plane DoS Mitigation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1857-paper.pdf)
- **解读**: 针对 Cerberus 等数据面 DoS 缓解系统通过控制面扩充受限交换机内存以快速响应攻击的模式，论文揭示其敏捷性与可扩展性机制本身可被新型协同 DoS 攻击利用，提出 Heracles，首个利用可编程交换机硬件级约束、在数据面与控制面内存间制造精确资源争用的攻击，通过侧信道时序信号触发同步扩充、内存挤压与时间窗口利用三种正交争用策略，显著削弱甚至完全瘫痪 DoS 缓解能力。在真实 Tofino 硬件上的测试表明，即使攻击源仅使用 1-2 秒的松散时间同步，Heracles 也能在多种 DoS 攻击画像下可靠地破坏防御。为缓解该威胁，论文提出 Shield，一种跨控制面与数据面解耦内存操作的多层 DoS 缓解 sketch 架构，在保持线速与检测精度的同时有效抵御 Heracles。


### One Email, Many Faces: A Deep Dive into Identity Confusion in Email Aliases.
- **作者**: Mengying Wu, Geng Hong, Jiatao Chen, Baojun Liu, Mingxuan Liu, Min Yang
- **论文 PDF**: [One Email, Many Faces: A Deep Dive into Identity Confusion in Email Aliases.](https://www.ndss-symposium.org/wp-content/uploads/2026-s148-paper.pdf)
- **解读**: 针对邮箱别名机制在邮件服务商（视为基础邮箱的额外入口）与外部平台（视为独立身份）之间造成的身份混淆问题，论文对 28 家邮件服务商与 18 个在线平台展开首次系统性分析，发现仅 Gmail 完整记录了别名规则，11 家服务商存在未文档化的别名行为，平台要么无法区分别名要么过度激进地排除含特定符号的邮件。真实滥用案例显示攻击者利用别名在 npm 中从单个基础邮箱创建多达 139 个账号用于垃圾邮件；用户研究还发现 31.65% 具备别名知识的参与者会把钓鱼邮件误认为合法邮件，且自认了解别名的用户（尤其高学历、男性、技术背景）更易受骗。作者贡献了 OriginMail 工具帮助平台消解别名混淆，并强调别名标准化与透明化的迫切性。


### OptiMix: Scalable and Distributed Approaches for Latency Optimization in Modern Mixnets.
- **作者**: Mahdi Rahimi
- **论文 PDF**: [OptiMix: Scalable and Distributed Approaches for Latency Optimization in Modern Mixnets.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2680-paper.pdf)
- **解读**: 针对 mixnet 以高通信时延换取匿名性、只适用于时延容忍应用并缩小匿名集的问题，论文提出 OptiMix，一个可适配所有成熟设计的通用低时延 mixnet 模型，包含高效分布式节点排列协议（在保持抗偏置性的同时实现低时延）、优化通信时延的新型策略路由方案、以及不损害时延优化特性的负载均衡算法。基于已部署 Nym mixnet 数据的广泛评估表明，OptiMix 在多种 mixnet 设计下以极小的匿名性损失获得显著时延降低，相比现有 SOTA 最多提升 4 倍。针对匿名性三难困境，论文还提出掩护路由机制，以产生额外掩护流量的适度代价让客户端在不牺牲匿名性的情况下受益于低时延 mixnet。


### OSAVRoute: Advancing Outbound Source Address Validation Deployment Detection with Non-Cooperative Measurement.
- **作者**: Shuai Wang, Ruifeng Li, Li Chen, Dan Li, Lancheng Qin, Qian Cao
- **论文 PDF**: [OSAVRoute: Advancing Outbound Source Address Validation Deployment Detection with Non-Cooperative Measurement.](https://www.ndss-symposium.org/wp-content/uploads/2026-s17-paper.pdf)
- **解读**: 针对出站源地址验证（OSAV）部署测量通常需在被测网络内发送伪造源地址包、要求运营商配合的问题，论文提出 OSAVRoute，首个能获取 OSAV 部署细粒度特征的非协作测量系统，通过显式追踪伪造源地址包的转发路径识别其产生与传播行为，不仅能判断 OSAV 的有无，还能测量其阻断粒度与阻断深度，达到此前仅协作方法具备的能力。OSAVRoute 准确率达 99.4%、覆盖 AS 数比 CAIDA Spoofer 多 3.1 倍，测量显示 84.2% 的被测 AS（尤其 ISP 网络）未部署 OSAV；已部署网络中 95.5% 在前两个 IP 跳内阻断伪造包但粒度各异，以 /22-/24 最常见；论文还首次揭示 MANRS 参与与 OSAV 部署之间的正相关关系。


### PACS: Privacy-Preserving Attribute-Driven Community Search over Attributed Graphs.
- **作者**: Fangyuan Sun, Yaxi Yang, Jia Yu, Jianying Zhou
- **论文 PDF**: [PACS: Privacy-Preserving Attribute-Driven Community Search over Attributed Graphs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1586-paper.pdf)
- **解读**: 针对属性驱动社区搜索在加密图数据上计算结构内聚度与属性相关性等复杂度量过于耗时、缺乏实用隐私保护方案的问题，论文提出 PACS，首个实用的云上隐私保护属性驱动社区搜索方案，服务端可在近毫秒级时间内响应查询且无法获知属性图与搜索结果等敏感信息。PACS 设计了安全社区索引与安全边表两个结构：前者通过内积加密基于加密属性向量评估社区属性得分并定位目标社区，后者基于 BGN 同态加密让服务端安全取回目标社区的边信息。安全性分析证明 PACS 达到 CQA2 安全，真实社交网络数据集上的实验验证了近毫秒级的处理效率。


### Paladin: Defending LLM-enabled Phishing Emails with a New Trigger-Tag Paradigm.
- **作者**: Yan Pang, Wenlong Meng, Xiaojing Liao, Tianhao Wang
- **论文 PDF**: [Paladin: Defending LLM-enabled Phishing Emails with a New Trigger-Tag Paradigm.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2522-paper.pdf)
- **解读**: 针对 LLM 生成的钓鱼邮件无拼写错误等易检测特征、且缺乏明显语言特征导致语义级检测失效的问题，论文提出 Paladin，通过多种插入策略将触发词-标签关联嵌入普通 LLM 形成“仪表化”模型：当模型生成与钓鱼相关的内容时会自动附带可检测的标签，从而便于识别。论文基于显式/隐式触发词与标签的组合考虑四种场景，并从隐蔽性、有效性与鲁棒性三个维度评估，实验表明 Paladin 在所有场景下检测准确率超过 90%，优于现有基线方法。


### Pallas and Aegis: Rollback Resilience in TEE-Aided Blockchain Consensus.
- **作者**: Jérémie Decouchant, David Kozhaya, Vincent Rahli, Jiangshan Yu
- **论文 PDF**: [Pallas and Aegis: Rollback Resilience in TEE-Aided Blockchain Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2443-paper.pdf)
- **解读**: 针对使用可信组件（TEE）的 BFT 共识在组件崩溃、状态丢失或被克隆时易受回滚攻击的问题，论文提出无需跨副本复制可信状态的安全状态保持机制。其核心 Aegis 是首个专为使用可信组件的 BFT 协议设计的高效视图同步器，即使组件在崩溃重启或被攻击者克隆后，也强制每个副本仅有一个可信组件实例可在任何视图中投票；在此基础上提出 Pallas，首个在攻击者控制固定拜占庭副本集、且可导致数量无界变化的可信组件崩溃时仍保持安全性的 BFT 共识协议，并确定了部分同步下保证活性的对抗条件。在 AWS 上的全球分布式评估表明，Pallas 在稳定条件下仅产生可忽略开销，吞吐量最高提升 41%、时延最佳改善 29%，并在其他协议失效的对抗条件下维持活性与优雅降级。


### Pando: Extremely Scalable BFT Based on Committee Sampling.
- **作者**: Xin Wang, Haochen Wang, Haibin Zhang, Sisi Duan
- **论文 PDF**: [Pando: Extremely Scalable BFT Based on Committee Sampling.](https://www.ndss-symposium.org/wp-content/uploads/2026-s273-paper.pdf)
- **解读**: 针对 BFT 协议随副本数 n 增长性能急剧退化、现有工作仅在低端机器上扩展到约一百个副本的问题，论文基于委员会采样方法构建协议——选取小规模委员会达成共识并将结果传达给所有副本——但该方法此前仅关注拜占庭协议问题且偏向理论。作者构建了部分同步环境下基于委员会采样的 Pando，实现高效、可扩展且自适应安全的 BFT 协议。在 Amazon EC2 上的评估显示，Pando 在 WAN 环境下可轻松扩展到一千个副本，吞吐量达 62.57 ktx/sec。


### PANDORA: Lightweight Adversarial Defense for Edge IoT using Uncertainty-Aware Metric Learning.
- **作者**: Avinash Awasthi, Pritam Vediya, Hemant Miranka, Ramesh Babu Battula, Manoj Singh Gaur
- **论文 PDF**: [PANDORA: Lightweight Adversarial Defense for Edge IoT using Uncertainty-Aware Metric Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-f713-paper.pdf)
- **解读**: 针对资源受限 IoT 设备攻击面扩大、静态签名式 IDS 过时且现有 ML 模型难以应对概念漂移与未知威胁的问题，论文提出 PANDORA，一个面向边缘设备的零日攻击检测端到端框架，包括：学习不确定性感知的概率嵌入以鲁棒表示网络流量、提出概率流形结构与距离（PMSD）损失实现零样本泛化、以及面向设备端部署的高效 Mamba-专家混合（MoE）架构，并引入新基准 TTDFIOTIDS2025。实验表明 PANDORA 在 CICIDS2017 上仅 10-shot 适配即取得 0.971 的 F1 分数，域偏移下零样本检测准确率最高达 99%，在树莓派上仅占 24MB 内存、吞吐最高 4.26 flows/sec，验证了实时边缘安全的可行性。


### Passive Multi-Target GUTI Identification via Visual-RF Correlation in LTE Networks.
- **作者**: Byeongdo Hong, Gunwoo Yoon
- **论文 PDF**: [Passive Multi-Target GUTI Identification via Visual-RF Correlation in LTE Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2487-paper.pdf)
- **解读**: 针对 LTE 网络用 GUTI 保护用户免受 IMSI 暴露、但该临时标识仍可被被动解析并关联到具体设备的问题，论文将带时间戳的视觉观测与商品软件无线电（SDR）捕获的空口控制面消息相关联，用有限状态机（FSM）算法解析相机视野内每台设备的 GUTI，仅需约三次用户交互即可完成，且无需事先掌握目标信息。在多个商用 LTE 网络的现场实验中验证了多目标解析能力：部分部署中 GUTI 最长持续 33 天且重分配行为常可关联；一旦关联，这些长寿命标识可通过被动监测寻呼与 RRC 消息实现从小区到寻呼区级别的位置追踪。相比主动 IMSI 捕获器或需要预知号码的 GUTI 攻击，该方法只监听、可扩展至视野内多台设备。


### PathProb: Probabilistic Inference and Path Scoring for Enhanced and Flexible BGP Route Leak Detection.
- **作者**: Yingqian Hao, Hui Zou, Lu Zhou, Yuxuan Chen, Yanbiao Li
- **论文 PDF**: [PathProb: Probabilistic Inference and Path Scoring for Enhanced and Flexible BGP Route Leak Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1691-paper.pdf)
- **解读**: 针对现有 BGP 路由泄漏检测存在二元分类僵化、误报率高且权威 AS 关系数据稀疏的问题，论文提出 PathProb，通过为 AS 链路计算拓扑感知的概率分布、为 AS 路径计算合法性得分来灵活识别路由泄漏，并集成蒙特卡洛方法与路由策略的整数线性规划建模以高效求解。在真实 BGP 路由轨迹与泄漏事件上的评估显示，其推断模型在高质量验证数据集上优于现有方法，检测真实路由泄漏的召回率达 98.45%，同时误报率较现有方案降低 4.29-20.08 个百分点；路径合法性评分还允许管理员按误报容忍度动态调整检测阈值，并与 ASPA 等新兴泄漏缓解机制兼容。


### Peering Inside the Black-Box: Long-Range and Scalable Model Architecture Snooping via GPU Electromagnetic Side-Channel.
- **作者**: Rui Xiao, Sibo Feng, Soundarya Ramesh, Jun Han, Jinsong Han
- **论文 PDF**: [Peering Inside the Black-Box: Long-Range and Scalable Model Architecture Snooping via GPU Electromagnetic Side-Channel.](https://www.ndss-symposium.org/wp-content/uploads/2026-s141-paper.pdf)
- **解读**: 针对 DNN 模型架构等机密信息通常对攻击者隐藏、导致黑盒对抗攻击受限的问题，论文提出 ModelSpy，一种基于 GPU 电磁（EM）泄漏的隐蔽 DNN 架构窥探攻击，利用 GPU 推理时辐射的远场 EM 信号携带架构特定幅度调制这一观察，可从数米外甚至隔墙提取完整架构。作者开发了分层重建模型从含噪 EM 信号中恢复细粒度架构细节，并利用外部 EM 泄漏与内部 GPU 活动相关性设计迁移学习方案以适配多样演进架构。在 5 款高端消费级 GPU 上的评估表明，ModelSpy 架构重建精度高（层分割 97.6%、超参数估计 94.0%），工作距离达 6 米，且重建的 DNN 性能与受害架构相当，能有效增强黑盒对抗攻击。


### PhantomMap: GPU-Assisted Kernel Exploitation.
- **作者**: Jiayi Hu, Qi Tang, Xingkai Wang, Jinmeng Zhou, Rui Chang, Wenbo Shen
- **论文 PDF**: [PhantomMap: GPU-Assisted Kernel Exploitation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f201-paper.pdf)
- **解读**: 针对 Arm Mali GPU 内存映射机制缺乏深入安全分析的问题，论文开展首次深度分析并发现两个新弱点：分配-映射解耦与物理地址验证缺失。据此提出 PhantomMap，一种 GPU 辅助利用技术，可将有限的堆漏洞转化为强大的物理内存读写原语，无需特权能力或信息泄漏即可绕过主流内核防御。作者开发静态分析器系统识别所有易受攻击的映射路径，跨两个 Mali 驱动架构发现 15 条利用链，并基于真实 CVE 构建 15 个端到端利用（含 CVE-2025-21836 的首个公开利用）；最后设计并实现轻量级驱动内缓解方案，在 Pixel 6 与 Pixel 7 上以最小性能开销消除根因。


### PhantomMotion: Laser-Based Motion Injection Attacks on Wireless Security Surveillance Systems.
- **作者**: Yan He, Guanchong Huang, Song Fang
- **论文 PDF**: [PhantomMotion: Laser-Based Motion Injection Attacks on Wireless Security Surveillance Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1454-paper.pdf)
- **解读**: 针对无线安防监控系统普遍集成的运动检测功能，论文提出 PhantomMotion，一种欺骗其运动检测的攻击框架：通过将激光束射入运动检测范围隐蔽地制造虚假运动刺激，并嗅探无线流量确认系统对刺激的响应，无需专业设备也无需进入监控区域实施物理移动。作者开发了集成激光控制与 WiFi 嗅探的硬件平台及新型运动注入生成机制，并以手机应用实现，对 18 款流行无线运动激活安防系统验证有效：平均 12.8 秒内、激光光斑平均移动 1.1 米即可成功触发系统，且从最远 120 米外仍可工作。


### Phishing in Wonderland: Evaluating Learning-Based Ethereum Phishing Transaction Detection and Pitfalls.
- **作者**: Ahod Alghuried, David Mohaisen
- **论文 PDF**: [Phishing in Wonderland: Evaluating Learning-Based Ethereum Phishing Transaction Detection and Pitfalls.](https://www.ndss-symposium.org/wp-content/uploads/2026-f694-paper.pdf)
- **解读**: 针对钓鱼攻击占以太坊相关网络犯罪超过 50%、机器学习防御兴起但仍存在实践缺陷的问题，论文提出一个综合框架，从特征选择、类别不平衡、模型鲁棒性与算法优化等方面提升以太坊交易钓鱼检测。通过对现有方法的系统评估，作者识别出实践中特征操纵与不可持续的性能提升等主要不足，并断言所提框架改善了检测的泛化性与有效性，强调需要针对区块链领域日益复杂的钓鱼手段持续细化检测策略。


### PhishLang: A Real-Time, Fully Client-Side Phishing Detection Framework Using MobileBERT.
- **作者**: Sayak Saha Roy, Shirin Nilizadeh
- **论文 PDF**: [PhishLang: A Real-Time, Fully Client-Side Phishing Detection Framework Using MobileBERT.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1037-paper.pdf)
- **解读**: 针对启发式/静态特征模型难以应对规避威胁、深度学习模型又过于资源密集无法客户端部署的问题，论文提出 PhishLang，首个以 Chromium 浏览器扩展形式实现的纯客户端反钓鱼框架，用轻量模型 MobileBERT 实时分析页面源代码的上下文结构进行本地检测，检测性能与多个 SOTA 模型相当而内存消耗最多降低 7 倍。在 3.5 个月的真实部署中识别出约 2.6 万个钓鱼 URL（许多未被流行黑名单收录），零日威胁检测率超过 91%，并通过解析器级防御与对抗再训练抵抗 16 类问题空间规避，框架与扩展均已开源。


### PhyFuzz: Detecting Sensor Vulnerabilities with Physical Signal Fuzzing.
- **作者**: Zhicong Zheng, Jinghui Wu, Shilin Xiao, Yanze Ren, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **论文 PDF**: [PhyFuzz: Detecting Sensor Vulnerabilities with Physical Signal Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f29-paper.pdf)
- **解读**: 针对传感器漏洞发现完全依赖人工专家分析与试错、缺乏自动化手段的问题，论文提出 PhyFuzz，一种依赖物理测试信号、无需人工介入即可检测现有及新型传感器漏洞的物理信号模糊测试范式。为应对信号参数搜索空间无限与传感器硬件黑盒等挑战，论文设计了独特的模糊算法，实现高效测试信号构造与有效的特征离散化以支持漏洞识别与评估。原型支持声学、激光与电磁信号的模糊测试，实验在 9 类 13 个传感器上识别出 46 个漏洞，其中 6 个为此前未公开的案例。


### PIRANHAS: PrIvacy-Preserving Remote Attestation in Non-Hierarchical Asynchronous Swarms.
- **作者**: Jonas Hofmann, Philipp-Florens Lehwalder, Shahriar Ebrahimi, Parisa Hassanizadeh, Sebastian Faust
- **论文 PDF**: [PIRANHAS: PrIvacy-Preserving Remote Attestation in Non-Hierarchical Asynchronous Swarms.](https://www.ndss-symposium.org/wp-content/uploads/2026-f526-paper.pdf)
- **解读**: 针对远程证明协议缺乏公开可验证性与交互性要求、且敏感系统证明需要隐私保护的问题，论文提出 PIRANHAS，一个面向单设备与群组（swarm）的公开可验证、异步且匿名的远程证明方案：利用 zk-SNARK 将任何经典对称远程证明方案转化为非交互、公开可验证且匿名的形式，验证者只能确认证明有效而无法获知涉及设备的任何身份信息；群组场景下通过递归 zk-SNARK 聚合整个群组的证明，支持任意网络拓扑与节点动态加入/离开。作者给出单设备与群组两种设置下的形式化安全证明，并基于 Noir 与 Plonky2 开源实现，聚合运行时间仅 356ms。


### Pitfalls for Security Isolation in Multi-CPU Systems.
- **作者**: Simeon Hoffmann, Nils Ole Tippenhauer
- **论文 PDF**: [Pitfalls for Security Isolation in Multi-CPU Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f971-paper.pdf)
- **解读**: 针对嵌入式多 CPU SoC 集成（如 Cortex-M4 主固件加 Cortex-M0 RTOS）带来的安全问题，论文系统识别了因复用单 CPU 安全机制（如内存保护单元 MPU）而产生、涉及内存与外设访问控制的漏洞，归纳出四类可能存在于此类系统中的主要攻击向量，并发现市场上相当数量的系统似乎易受攻击，可导致对另一 CPU 受保护内存的任意读写乃至代码执行。论文还发现流行开源 RTOS FreeRTOS 在多 CPU 场景下被推荐使用的固件间通信机制引入代码执行漏洞，并通过实现四个攻击向量验证理论预测，其中一个案例的攻击面可能导致自定义可信执行环境（TEE）被攻破；发现已负责任地披露给厂商，促成安全公告与专有网络协议栈的修复。


### PortRush: Detect Write Port Contention Side-Channel Vulnerabilities via Hardware Fuzzing.
- **作者**: Peihong Lin, Pengfei Wang, Lei Zhou, Gen Zhang, Xu Zhou, Wei Xie, Zhiyuan Jiang, Kai Lu
- **论文 PDF**: [PortRush: Detect Write Port Contention Side-Channel Vulnerabilities via Hardware Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f587-paper.pdf)
- **解读**: 针对多功能模块争用有限共享写端口引起的写端口争用侧信道尚未被充分研究的问题，论文提出 PortRush，一个在寄存器传输级（RTL）检测并验证此类漏洞的模糊测试框架：先用写请求图（WRG）对写路径与优先级关系建模、静态识别潜在争用实例，再用分层聚合与解码方法跨设计层级监测硬件信号高效检测争用，最后用争用引导的硬件模糊测试结合瞬态执行攻击模式自动触发并验证漏洞。在 BOOM、NutShell 与 Rocket Core 三个 RISC-V CPU 上的评估验证了有效性，并基于发现的漏洞提出两个新攻击向量：利用重排序缓冲物理寄存器文件争用的 Birgus 变体，以及利用 L1 数据缓存加载/存储单元与 MSHR 之间争用制造秘密相关执行延迟的 MSHRush，同时给出面向 CPU 开发者的缓解策略。


### Practical Traceable Over-Threshold Multi-Party Private Set Intersection.
- **作者**: Le Yang, Weijing You, Huiyang He, Kailiang Ji, Jingqiang Lin
- **论文 PDF**: [Practical Traceable Over-Threshold Multi-Party Private Set Intersection.](https://www.ndss-symposium.org/wp-content/uploads/2026-s38-paper.pdf)
- **解读**: 针对可追踪的过阈值多方隐私集合求交（T-OT-MP-PSI，披露至少 t 个参与方集合共有的元素及其持有者）现有方案仅抵抗 t-2 个半诚实参与方且计算开销大、还依赖“特定特殊方不合谋”假设的问题，论文提出两个新协议：ET-OT-MP-PSI 组合 Shamir 秘密共享与不经意可编程伪随机函数，显著提升效率并抵抗至多 t-2 个半诚实参与方；ST-OT-MP-PSI 进一步借助不经意线性求值协议，将安全性提升到抵抗至多 n-1 个半诚实参与方。实验表明在 5 个参与方、阈值 3、集合规模 2^14 的设置下，ET 与 ST 协议相比 Mahdavi 等人的方案分别快约 15056 倍与 505 倍，在增强安全性的同时保持高效率。


### PriSrv+: Privacy and Usability-Enhanced Wireless Service Discovery with Fast and Expressive Matchmaking Encryption.
- **作者**: Yang Yang, Guomin Yang, Yingjiu Li, Pengfei Wu, Rui Shi, Minming Huang, Jian Weng, HweeHwa Pang, Robert H. Deng
- **论文 PDF**: [PriSrv+: Privacy and Usability-Enhanced Wireless Service Discovery with Fast and Expressive Matchmaking Encryption.](https://www.ndss-symposium.org/wp-content/uploads/2026-s87-paper.pdf)
- **解读**: 针对无线网络服务发现协议在表达能力、隐私、可扩展性与效率上的局限，论文提出 PriSrv+，在 PriSrv（NDSS'24）基础上构建并保持与 mDNS、BLE、Wi-Fi 等广泛无线协议的兼容。核心创新是快速可表达匹配加密（FEME），首个支持无界属性宇宙的访问控制策略的匹配加密方案，允许任意字符串作为属性，显著提升服务发现灵活性并保证消息与属性隐私。相比 PriSrv，PriSrv+ 加密快 7.62 倍、解密快 6.23 倍、密文大小减少 87.33%，服务广播与匿名双向认证的通信开销分别降低 87.33% 与 86.64%，形式化安全证明确认了 FEME 与 PriSrv+ 的安全性，多平台评估验证了其性能、可扩展性与效率优势。


### PrivATE: Differentially Private Average Treatment Effect Estimation for Observational Data.
- **作者**: Quan Yuan, Xiaochen Li, Linkang Du, Min Chen, Mingyang Sun, Yunjun Gao, Shibo He, Jiming Chen, Zhikun Zhang
- **论文 PDF**: [PrivATE: Differentially Private Average Treatment Effect Estimation for Observational Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1350-paper.pdf)
- **解读**: 针对从观测数据估计平均处理效应（ATE）带来的用户隐私风险、而现有差分隐私 ATE 估计依赖特定假设或保护有限的问题，论文提出 PrivATE，一个保证差分隐私的实用 ATE 估计框架，针对教育评估仅成绩敏感、医疗记录全部私密等不同场景设计标签级与样本级两级隐私保护。通过推导自适应匹配界限，PrivATE 有效平衡噪声引入误差与匹配误差以获得更准确的 ATE 估计，评估表明其在所有数据集与隐私预算上均优于基线方法。


### PrivCode: When Code Generation Meets Differential Privacy.
- **作者**: Zheng Liu, Chen Gong, Terry Yue Zhuo, Kecen Li, Weichen Yu, Matt Fredrikson, Tianhao Wang
- **论文 PDF**: [PrivCode: When Code Generation Meets Differential Privacy.](https://www.ndss-symposium.org/wp-content/uploads/2026-f936-paper.pdf)
- **解读**: 针对在私有代码数据上微调 LLM 带来的敏感信息泄漏风险、且代码的严格语法依赖与隐私-效用权衡使差分隐私（DP）代码生成面临严峻挑战的问题，论文提出 PrivCode，首个专为代码数据集设计的 DP 合成器，采用两阶段框架：第一阶段“隐私净化”用 DP-SGD 训练模型并引入语法信息以保留代码结构，生成符合 DP 的合成代码；第二阶段“效用提升”在无隐私合成代码上微调更大的预训练 LLM，缓解 DP 带来的效用损失。在 4 个 LLM 与 4 个基准上的实验表明，PrivCode 生成更高效用的代码，并能在不同隐私预算下保护敏感数据。


### PrivORL: Differentially Private Synthetic Dataset for Offline Reinforcement Learning.
- **作者**: Chen Gong, Zheng Liu, Kecen Li, Tianhao Wang
- **论文 PDF**: [PrivORL: Differentially Private Synthetic Dataset for Offline Reinforcement Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-f149-paper.pdf)
- **解读**: 针对离线强化学习数据集共享引发的隐私泄漏担忧，论文提出 PrivORL，首个差分隐私（DP）离线数据集合成方法，分别用扩散模型与扩散 Transformer 在 DP 约束下合成转移（transitions）与轨迹（trajectories），合成数据集可安全发布供下游分析与研究。PrivORL 采用在公开数据集上预训练合成器、再用 DP-SGD 在敏感数据集上微调的流行路线，并引入好奇心驱动的预训练，利用好奇心模块反馈使合成数据集多样化。在 5 个敏感离线 RL 数据集上的实验表明，其 DP 转移与轨迹合成的效用与保真度均优于基线。


### Prompt Injection Attack to Tool Selection in LLM Agents.
- **作者**: Jiawen Shi, Zenghui Yuan, Guiyao Tie, Pan Zhou, Neil Zhenqiang Gong, Lichao Sun
- **论文 PDF**: [Prompt Injection Attack to Tool Selection in LLM Agents.](https://www.ndss-symposium.org/wp-content/uploads/2026-s675-paper.pdf)
- **解读**: 针对 LLM 代理通过“检索-选择”两步从工具库选工具、攻击者可注入恶意工具文档操纵选择过程的漏洞，论文提出 ToolHijacker，一种面向无盒（no-box）场景的提示注入攻击，将恶意工具文档注入工具库，迫使代理对攻击者选定的目标任务持续选择恶意工具，并将文档构造形式化为优化问题、提出两阶段优化策略求解。大量实验表明 ToolHijacker 高度有效，显著优于现有手工与自动提示注入攻击；论文还测试了基于预防（StruQ、SecAlign）与基于检测（known-answer、DataSentinel、困惑度及滑动窗口困惑度检测）的多类防御，结果显示这些防御均不充分，凸显开发新防御策略的紧迫性。


### ProtocolGuard: Detecting Protocol Non-compliance Bugs via LLM-guided Static Analysis and Dynamic Verification.
- **作者**: Xiangpu Song, Longjia Pei, Jianliang Wu, Yingpei Zeng, Gaoshuo He, Chaoshun Zuo, Xiaofeng Liu, Qingchuan Zhao, Shanqing Guo
- **论文 PDF**: [ProtocolGuard: Detecting Protocol Non-compliance Bugs via LLM-guided Static Analysis and Dynamic Verification.](https://www.ndss-symposium.org/wp-content/uploads/2026-f521-paper.pdf)
- **解读**: 针对网络协议实现因自然语言规范歧义而偏离标准行为、产生难以检测的合规性缺陷（非内存破坏类、无显式错误行为）且现有工作验证与根因分析需大量人工的问题，论文提出 ProtocolGuard，结合 LLM 引导的静态分析与基于模糊测试的动态验证：先从规范中提取规范规则并做 LLM 引导的程序切片，由 LLM 检测规则与代码逻辑间的语义不一致，再自动生成断言并插桩将静默缺陷转为可观察的断言失败，配合 LLM 生成更易触发的初始测试用例做动态验证并产出 PoC。在 11 个广泛使用的协议实现上发现 158 个合规性缺陷（70 个已确认），在检测精度与召回率上均优于现有 SOTA 工具。


### Pruning the Tree: Rethinking RPKI Architecture from the Ground up.
- **作者**: Haya Schulmann, Niklas Vogel
- **论文 PDF**: [Pruning the Tree: Rethinking RPKI Architecture from the Ground up.](https://www.ndss-symposium.org/wp-content/uploads/2026-s823-paper.pdf)
- **解读**: 针对 RPKI 沿用 X.509 EE 证书、ASN.1 编码、XML 仓库协议等遗留 PKI 组件造成过度密码学验证、冗余元数据与存储处理低效的问题，论文首次系统分析 RPKI 设计复杂性的根因并量化影响，发现依赖方（RP）超过 70% 的验证时间花在证书解析与签名验证上且多属不必要。据此提出向后兼容的改进 RPKI（iRPKI），消除 EE 证书与 ROA 签名、合并撤销与完整性对象、用 Protobuf 替代冗长编码、重构仓库元数据。在 Routinator 验证器中的实现取得 20 倍处理加速、18 倍带宽改善与 8 倍缓存内存缩减，同时消除曾导致 RPKI 软件至少 10 个漏洞的一类缺陷，且可增量部署、不干扰现有运营。


### Prεεmpt: Sanitizing Sensitive Prompts for LLMs.
- **作者**: Amrita Roy Chowdhury, David Glukhov, Divyam Anshumaan, Prasad Chalasani, Nicolas Papernot, Somesh Jha, Mihir Bellare
- **论文 PDF**: [Prεεmpt: Sanitizing Sensitive Prompts for LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1277-paper.pdf)
- **解读**: 针对 LLM 推理时提示中的敏感信息可能暴露给专有 LLM API 的隐私问题，论文先引入一个受密码学启发的“提示净化器”概念以转换输入提示、保护敏感令牌，再提出 Prεεmpt 系统实现该概念。Prεεmpt 将敏感令牌分为两类：响应仅依赖格式的（如 SSN、信用卡号）使用保格式加密（FPE）；响应依赖具体数值的（如年龄、薪资）应用度量差分隐私（mDP）。评估表明 Prεεmpt 是获得有意义的隐私保证的实用方法，相比未净化的提示与先前方法在保持高可用性的同时更优。


### Q-MLLM: Vector Quantization for Robust Multimodal Large Language Model Security.
- **作者**: Wei Zhao, Zhe Li, Yige Li, Jun Sun
- **论文 PDF**: [Q-MLLM: Vector Quantization for Robust Multimodal Large Language Model Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-s407-paper.pdf)
- **解读**: 针对多模态大语言模型（MLLM）视觉表示连续、可被基于梯度的对抗攻击利用，且文本安全机制难以迁移到视觉内容的问题，论文提出 Q-MLLM，一种集成两级向量量化的新架构，在像素块与语义两个层面离散化视觉表示，形成对抗攻击的离散瓶颈并弥合跨模态安全对齐鸿沟，配合两阶段训练在保持模型效用的同时实现鲁棒学习。实验表明 Q-MLLM 在越狱攻击与有毒图像攻击下的防御成功率显著高于现有方法，对越狱攻击实现 100% 完美防御（仅一个可争议案例除外），且在多个效用基准上保持竞争性能、推理开销极小，确立了向量量化作为无需昂贵安全微调或检测开销的有效多模态防御机制。


### QNBAD: Quantum Noise-induced Backdoor Attacks against Zero Noise Extrapolation.
- **作者**: Cheng Chu, Qian Lou, Fan Chen, Lei Jiang
- **论文 PDF**: [QNBAD: Quantum Noise-induced Backdoor Attacks against Zero Noise Extrapolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1665-paper.pdf)
- **解读**: 针对零噪声外推（ZNE）这一广泛使用的量子误差缓解技术可能被对抗性利用的问题，论文分析现有量子后门攻击为何难以动摇 ZNE：修改电路结构的后门仅平移理想输出、不干扰依赖噪声的外推过程，而参数级后门因未针对设备噪声训练而在不同硬件间行为不一致。据此论文提出 QNBAD，一类专门针对 ZNE 特性设计的新型隐蔽后门攻击：在多数设备上保持变分量子电路功能正确，但在特定噪声模型下利用量子噪声与电路结构的微妙交互系统性操纵不同噪声水平下的采样期望值，破坏 ZNE 拟合过程导致最终估计显著偏斜。相比先前后门方法，QNBAD 在四个平台、六个应用上实现 1.68 倍至 11.7 倍的绝对误差放大，且对多种拟合函数与 ZNE 变体均有效。


### ReFuzz: Reusing Tests for Processor Fuzzing with Contextual Bandits.
- **作者**: Chen Chen, Zaiyan Xu, Mohamadreza Rostami, David Liu, Dileep Kalathil, Ahmad-Reza Sadeghi, Jeyavijayan Rajendran
- **论文 PDF**: [ReFuzz: Reusing Tests for Processor Fuzzing with Contextual Bandits.](https://www.ndss-symposium.org/wp-content/uploads/2026-f118-paper.pdf)
- **解读**: 针对处理器设计迭代修改与设计复用导致类似漏洞在多处理器间复现、而现有硬件模糊器各自独立模糊单个设计、无法利用先前处理器的已知漏洞知识来调整模糊策略的问题，论文提出 ReFuzz，一个利用上下文老虎机（contextual bandit）复用先前处理器高效测试用例、对给定 ISA 内的被测处理器（PUT）进行自适应模糊的框架，通过智能变异在先前处理器上触发漏洞的测试，检测 PUT 中相似与新变体的漏洞。ReFuzz 发现 3 个新安全漏洞与 2 个新功能缺陷（其中一个漏洞通过复用触发先前处理器已知漏洞的测试发现，一个功能缺陷存在于共享设计模块的三个处理器，另一缺陷有两个变体），并复用高效测试提升覆盖率，平均取得 511.23 倍覆盖率加速、总覆盖率最高提升 9.33%。


### Repairing Trust in Domain Name Disputes Practices: Insights from a Quarter-Century's Worth of Squabbles.
- **作者**: Boladji Vinny Adjibi, Athanasios Avgetidis, Manos Antonakakis, Alberto Dainotti, Michael D. Bailey, Fabian Monrose
- **论文 PDF**: [Repairing Trust in Domain Name Disputes Practices: Insights from a Quarter-Century's Worth of Squabbles.](https://www.ndss-symposium.org/wp-content/uploads/2026-s174-paper.pdf)
- **解读**: 针对统一域名争议解决政策（UDRP）运行二十余年、因缺乏大规模结构化数据而难以实证评估与改革的问题，论文训练模型从 90,153 起 UDRP 争议裁决中提取结构化数据，开展迄今最全面的实证分析。发现近三分之一的争议存在择地行诉（forum shopping）、43 起案件存在潜在利益冲突、多方拖延远超预期响应时间等损害公平性与效率的问题；更严重的是，2,751 个恶意域名在专家组裁定转移后仍被恶意行为者控制长达四个月，构成真实安全威胁。作者据此提出引入更高自动化、加强监督与明确合规规则等政策改革建议，以恢复信任与透明度。


### Rethinking Fake Speech Detection: A Generalized Framework Leveraging Spectrogram Magnitude.
- **作者**: Zihao Liu, Aobo Chen, Yan Zhang, Wensheng Zhang, Chenglin Miao
- **论文 PDF**: [Rethinking Fake Speech Detection: A Generalized Framework Leveraging Spectrogram Magnitude.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1024-paper.pdf)
- **解读**: 针对现有伪造语音检测在未见场景（新合成模型、语言、录音条件）下泛化差、且多依赖特定假设的问题，论文从频谱图幅度表示切入重新思考该任务，通过大量分析发现合成语音在幅度表示中持续存在伪影，如纹理细节减少与跨幅度范围的不一致。据此提出无假设、通用的检测框架：按幅度将频谱图划分为分层表示，并用 2D 与 3D 表示在空间域与离散余弦变换（DCT）域检测伪影，从而有效捕获伪造语音固有的细粒度伪影与合成不一致性。在多个公开音频深度伪造数据集上达到 SOTA，对黑盒网页语音克隆 API 的真实场景评估也持续优于基线。


### Revealing The Secret Power: How Algorithms Can Influence Content Visibility on Twitter/X.
- **作者**: Alessandro Galeazzi, Pujan Paudel, Mauro Conti, Emiliano De Cristofaro, Gianluca Stringhini
- **论文 PDF**: [Revealing The Secret Power: How Algorithms Can Influence Content Visibility on Twitter/X.](https://www.ndss-symposium.org/wp-content/uploads/2026-s718-paper.pdf)
- **解读**: 针对社交网络推荐算法不透明、内容可见性操控（如影子封禁）可能既限制有害内容又压制异见声音的问题，论文对两个 Twitter/X 数据集（涵盖乌克兰-俄罗斯冲突与 2024 美国总统选举讨论、超 4000 万条推文与 900 万用户）展开大规模定量分析，用浏览量检测可见性降低或虚增的模式并考察其与用户观点、社会角色及叙事框架的关联。分析表明算法系统性惩罚含外部链接的推文，可见性最多降低 8 倍且与意识形态立场或信源可靠性无关；内容可见性反而因产生它的具体账号而异，如 Kyiv Independent 与 RT.com、特朗普与哈里斯的推文对比所示。工作凸显内容审核与推荐系统透明性对保护公共话语完整性的重要性。


### Revisiting Differentially Private Hyper-parameter Tuning.
- **作者**: Zihang Xiang, Tianhao Wang, Cheng-Long Wang, Di Wang
- **论文 PDF**: [Revisiting Differentially Private Hyper-parameter Tuning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s447-paper.pdf)
- **解读**: 针对差分隐私超参数调优（从多个候选运行中选最优）的隐私代价常被忽视、且现有私有选择方案的隐私上界是否紧致仍悬而未决的问题，论文从经验与理论两方面展开考察：一般场景下确认现有私有选择隐私分析确实是紧致的，但在白盒超参数调优场景下紧致性不再成立，并通过隐私审计首次演示了当前理论隐私界与强审计设置下经验隐私泄漏之间的显著差距。该差距促使作者利用调优过程的独特性质给出改进的隐私上界，带来更好的效用，且分析比此前局限于特定参数配置的工作具有更广的适用性，深化了对“选择”如何导致隐私退化的理解。


### Robust Fraud Transaction Detection: A Two-Player Game Approach.
- **作者**: Qi Tan, Yi Zhao, Laizhong Cui, Qi Li, Ming Zhu, Xing Fu, Weiqiang Wang, Xiaotong Lin, Ke Xu
- **论文 PDF**: [Robust Fraud Transaction Detection: A Two-Player Game Approach.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1611-paper.pdf)
- **解读**: 针对欺诈者不受小间隔限制地伪造交易特征、使基于小扰动假设的现有鲁棒性增强方法失效的问题，论文提出 GAMER，一个基于两人博弈的鲁棒欺诈检测系统：利用特征选择主动对抗智能欺诈者（减少特征数量以压缩特征伪造组合），并将检测过程创新地形式化为两人博弈，通过求解博弈均衡得到考虑欺诈者所有可能伪造策略的最优特征选择概率。该均衡概率既最小化欺诈者收益、遏制其发动伪造，又促使系统选择不易被伪造的鲁棒特征。理论与实验验证了威慑与鲁棒增强性质，在全球领先在线支付企业遭受的真实攻击数据上，GAMER 相比传统鲁棒增强技术使两个月欺诈检测的 F1 平均提升 67.5%。


### ropbot: Reimaging Code Reuse Attack Synthesis.
- **作者**: Kyle Zeng, Moritz Schloegel, Christopher Salls, Adam Doupé, Ruoyu Wang, Yan Shoshitaishvili, Tiffany Bao
- **论文 PDF**: [ropbot: Reimaging Code Reuse Attack Synthesis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f845-paper.pdf)
- **解读**: 针对代码复用攻击中 gadget 拼接仍耗时耗力、现有自动化方案因采用“生成-测试”策略（先枚举所有 gadget 再用符号执行或 SMT 求解器推理组合）而随 gadget 数量指数级扩展、难以应用于更大二进制的问题，论文提出 ROPBlock，一种保证可拼接的 gadget 分组，并组合图搜索算法实现 gadget 链构造，将设置寄存器为攻击者指定值的时间复杂度从 O(2^n) 降至 O(n)，实际链生成提速 2-3 个数量级。ROPBlock 还能建模 ret2csu 与条件分支等复杂 gadget，且架构无关（原型 ropbot 在 x64、MIPS、ARM、AArch64 上验证，RISC-V 支持仅加 12 行代码、两小时内完成）。ropbot 在 37 个二进制上平均 2.5 秒生成 dup-dup-execve 真实链条，mmap 链条覆盖目标数比次优方案多 5 倍，并在各自数据集上优于所有现有工具。


### Rounding-Guided Backdoor Injection in Deep Learning Model Quantization.
- **作者**: Xiangxiang Chen, Peixin Zhang, Jun Sun, Wenhai Wang, Jingyi Wang
- **论文 PDF**: [Rounding-Guided Backdoor Injection in Deep Learning Model Quantization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s113-paper.pdf)
- **解读**: 针对模型量化这一广泛部署技术可能被忽视的安全风险，论文提出 QuRA，一种仅利用量化操作即可嵌入恶意行为的后门攻击，不依赖训练数据投毒或模型训练操纵：先用新颖的权重选择策略识别影响后门目标的关键权重（同时保持模型整体性能），再通过优化这些权重的舍入方向跨层放大后门效果而不牺牲精度。实验表明 QuRA 在大多数情况下攻击成功率接近 100% 且性能退化可忽略，还能自适应绕过现有后门防御，凸显量化流程中亟需更稳健的安全措施。


### RoundRole: Unlocking the Efficiency of Multi-party Computation with Bandwidth-aware Execution.
- **作者**: Xiaoyu Fan, Kun Chen, Jiping Yu, Xin Liu, Yunyi Chen, Wei Xu
- **论文 PDF**: [RoundRole: Unlocking the Efficiency of Multi-party Computation with Bandwidth-aware Execution.](https://www.ndss-symposium.org/wp-content/uploads/2026-f52-paper.pdf)
- **解读**: 针对安全多方计算（MPC）中跨方通信为主要瓶颈、现有协议优化多忽略通信模式不对称导致带宽浪费、性能被“锁死”的问题，论文提出 RoundRole，一种面向秘密共享 MPC 的带宽感知执行优化：将决定通信模式的逻辑角色与决定带宽的物理节点解耦，把整体协议划分为并行任务并为每个任务把逻辑角色策略性映射到物理节点，使通信负载与协议固有通信量和物理带宽相匹配。作者将其集成到广泛使用的开源框架 ABY3 上，在 9 个协议、6 种同构/异构带宽网络设置下的评估显示最高 7.1 倍加速。


### RTCON: Context-Adaptive Function-Level Fuzzing for RTOS Kernels.
- **作者**: Eunkyu Lee, Junyoung Park, Insu Yun
- **论文 PDF**: [RTCON: Context-Adaptive Function-Level Fuzzing for RTOS Kernels.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1600-paper.pdf)
- **解读**: 针对 RTOS 内核中深层次函数复杂度高、现有模糊测试难以有效测试的问题，论文提出 RTCon，一个面向 RTOS 内核的上下文自适应函数级模糊器：在模糊过程中自适应地为目标函数生成函数上下文，对内核内任意目标函数执行函数级模糊，并用多层分类按置信度对崩溃分级，帮助分析者聚焦高置信崩溃。在 Zephyr、RIOT、FreeRTOS 与 ThreadX 四个流行 RTOS 内核上的评估发现 27 个缺陷（25 个为新缺陷，已全部上报维护者并获得 14 个 CVE），崩溃分类对高置信崩溃精确率达 92.7%（低置信仅 5.8%）。


### RTrace: Towards Better Visibility of Shared Library Execution.
- **作者**: Huaifeng Zhang, Ahmed Ali-Eldin
- **论文 PDF**: [RTrace: Towards Better Visibility of Shared Library Execution.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1243-paper.pdf)
- **解读**: 针对软件供应链安全背景下理解共享库（第三方依赖的主要形式）行为至关重要、而现有库函数追踪器因依赖错误符号信息、无法监控早期或间接函数调用等缺陷而漏掉大量函数调用的问题，论文分析广泛使用追踪器漏报的根因，提出 RTrace，通过综合运行时监控、函数边界检测与隐式/非常规函数调用支持来克服这些挑战。在 21 个应用、92 个共享库上的评估中，RTrace 相较 ltrace、drltrace、ldaudit 与 IntelPT 四个现有追踪器显著更优：所有基准上 F1 至少 0.92，而最佳现有追踪器仅 0.74，为共享库运行时行为提供更准确的可视性，并可辅助恶意包检测与漏洞分析。


### SACK: Systematic Generation of Function Substitution Attacks Against Control-Flow Integrity.
- **作者**: Zhechang Zhang, Hengkai Ye, Song Liu, Hong Hu
- **论文 PDF**: [SACK: Systematic Generation of Function Substitution Attacks Against Control-Flow Integrity.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2317-paper.pdf)
- **解读**: 针对即使在精确静态 CFI 策略下攻击者仍可通过函数替换攻击（Sub attack）在合法目标集内替换间接跳转目标劫持控制流、而此前构造完全依赖人工的问题，论文提出 SACK，首个大规模自动构造 Sub 攻击的系统化框架：从良性执行中收集被触发的间接调用目标，借助大语言模型合成安全预言机（oracle），自动执行目标替换并用预言机检测安全违规，同时确保执行严格符合精确 CFI 策略。将 SACK 应用于 7 个广泛使用的应用，成功构造 419 个破坏关键安全特性的 Sub 攻击，并基于 SQLite3、V8 与 Nginx 的历史缺陷开发 5 个可实现任意命令执行或认证绕过的端到端利用，证明其可扩展的自动化管线能跨应用挖掘大量端到端攻击。


### SAGA: A Security Architecture for Governing AI Agentic Systems.
- **作者**: Georgios Syros, Anshuman Suri, Jacob Ginesin, Cristina Nita-Rotaru, Alina Oprea
- **论文 PDF**: [SAGA: A Security Architecture for Governing AI Agentic Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s869-paper.pdf)
- **解读**: 针对 LLM 代理间自主交互协作增多、现有代理系统设计停留在理论且缺乏用户对代理生命周期的可控管理的问题，论文提出 SAGA，一个可扩展的代理系统治理安全架构：用户向中央实体 Provider 注册代理，Provider 维护代理联系信息与用户定义的访问控制策略并协助代理在代理间通信中执行这些策略。作者引入基于密码学的访问控制令牌派生机制，对代理与其他代理的交互提供细粒度控制与形式化安全保证。在多个代理任务、不同地理位置代理及多款端侧与云端 LLM 上的评估表明，SAGA 在广泛条件下性能开销极小、不影响底层任务效用，支持自主代理的安全可信部署。


### Scalable Off-Chain Auctions.
- **作者**: Mohsen Minaei, Ranjit Kumaresan, Andrew Beams, Pedro Moreno-Sanchez, Yibin Yang, Srinivasan Raghuraman, Panagiotis Chatzigiannis, Mahdi Zamani, Duc Viet Le
- **论文 PDF**: [Scalable Off-Chain Auctions.](https://www.ndss-symposium.org/wp-content/uploads/2026-s410-paper.pdf)
- **解读**: 针对区块链拍卖（如 NFT 价格发现）随投标人数扩展性差、导致网络拥塞、交易费上升与确认变慢的问题，论文构建密封投标拍卖协议：参与方诚实行为时拍卖完全在链下进行，n 方协议中若有 k 个投标者偏离（如不打开密封投标），链上复杂度仅为 O(k)，优于现有即使单个投标者偏离也需 O(n) 链上复杂度的方案；拍卖者恶意时协议仍保证拍卖成功终止。协议用 zkSnark 实现可扩展性，链上合约与其他参与方除获胜者及其中标金额外无法获知投标者身份与投标内容，实现相比现有链上方案显著更高的效率。


### SECV: Securing Connected Vehicles with Hardware Trust Anchors.
- **作者**: Martin Kayondo, Junseung You, Eunmin Kim, Jiwon Seo, Yunheung Paek
- **论文 PDF**: [SECV: Securing Connected Vehicles with Hardware Trust Anchors.](https://www.ndss-symposium.org/wp-content/uploads/2026-f106-paper.pdf)
- **解读**: 针对车联网中 EVN 平台作为 IVN 网关控制消息新来源、其复杂操作系统与应用被攻破后可伪造绕过网关过滤的控制消息的问题，论文提出 SECV，一种运行时安全机制，使 IVN 网关在 EVN 管理器被攻破时仍能准确验证 EVN 来源的控制消息：在可信执行环境（TEE）内中介所有 EVN 到 IVN 的流量、执行逐应用验证并附加密码学证明，由 IVN 网关用硬件安全模块（HSM）验证。实现在带 ARM TrustZone 的汽车 SoC 与 EVITA 合规 HSM 上，解决 TEE-HSM 信任建立、实时中介与受损下的稳健归因等实践挑战，仅 6.5% 的传输几何平均开销与极端通信突发下 1.5% 的额外消息丢失，有效缓解 EVN 来源攻击并满足实时约束。


### Select-Then-Compute: Encrypted Label Selection and Analytics over Distributed Datasets using FHE.
- **作者**: Nirajan Koirala, Seunghun Paik, Sam Martin, Helena Berens, Tasha Januszewicz, Jonathan Takeshita, Jae Hong Seo, Taeho Jung
- **论文 PDF**: [Select-Then-Compute: Encrypted Label Selection and Analytics over Distributed Datasets using FHE.](https://www.ndss-symposium.org/wp-content/uploads/2026-f207-paper.pdf)
- **解读**: 针对 PSI 等私有集合协议的结果可能敏感、且需对关联数据执行浮点等下游计算（如机器学习模型推断）而现有协议未处理这些真实复杂性的问题，论文提出首个加密标签选择与分析协议构造，让查询者不仅安全取回标识符交集结果，还能取回与交集标识符关联数据上下游函数的结果。作者基于近似 CKKS 全同态加密构造新协议，支持高效标签检索与实值数据下游计算，并提出多项技术处理 64 或 128 位大域标识符同时保证下游计算的高精度。在真实欺诈数据集上相比现有方法取得 1.4 倍至 6.8 倍加速，65 秒内完成真实世界数据的加密标签选择与分析，具备实际部署可行性。


### Shadow in the Cache: Unveiling and Mitigating Privacy Risks of KV-cache in LLM Inference.
- **作者**: Zhifan Luo, Shuo Shao, Su Zhang, Lijing Zhou, Yuke Hu, Chenxu Zhao, Zhihao Liu, Zhan Qin
- **论文 PDF**: [Shadow in the Cache: Unveiling and Mitigating Privacy Risks of KV-cache in LLM Inference.](https://www.ndss-symposium.org/wp-content/uploads/2026-f258-paper.pdf)
- **解读**: 针对 LLM 推理加速机制 KV 缓存（存储注意力中间计算以避免冗余计算）引入的隐私风险，论文提供首个全面分析，证明攻击者可直接从 KV 缓存重建敏感用户输入，并设计实现三类攻击向量：直接反转攻击、更普适且强效的碰撞攻击与基于语义的注入攻击。为缓解该问题，论文提出 KV-Cloak，一种轻量高效的可逆矩阵混淆方案并融合算子融合以保护 KV 缓存，实验表明 KV-Cloak 能有效挫败所有提出攻击、将重建质量降至随机噪声水平，同时几乎不损失模型精度、性能开销极小。


### Should I Trust You? Rethinking the Principle of Zone-Based Isolation DNS Bailiwick Checking.
- **作者**: Yuxiao Wu, Yunyi Zhang, Chaoyi Lu, Baojun Liu
- **论文 PDF**: [Should I Trust You? Rethinking the Principle of Zone-Based Isolation DNS Bailiwick Checking.](https://www.ndss-symposium.org/wp-content/uploads/2026-f330-paper.pdf)
- **解读**: 针对第三方服务环境下域名所有权与传统自上而下区域委派模型错位、削弱 bailiwick 检查有效性的问题，论文对主流解析器 bailiwick 检查的设计与实现展开系统分析，发现其普遍遵循保守原则：只要资源记录满足最小约束就会被缓存，无论其与发起查询是否直接相关。据此提出 CUCKOO DOMAIN 缓存投毒攻击：控制单个子域即可危害其父域或兄弟域，测试表明 BIND9、Microsoft DNS 等 7 个主流解析器实现易受攻击，大规模测量确认 44.64% 的开放解析器与 21 家主要公共 DNS 提供商存在风险，No-IP、ClouDNS、Akamai 等 7 家提供商的上百万个子域可能被劫持；已负责任披露，BIND9、Unbound、PowerDNS 与 Technitium 确认并分配 3 个 CVE。


### Side-channel Inference of User Activities in AR/VR Using GPU Profiling.
- **作者**: Seonghun Son, Chandrika Mukherjee, Reham Mohamed Aburas, Berk Gülmezoglu, Z. Berkay Celik
- **论文 PDF**: [Side-channel Inference of User Activities in AR/VR Using GPU Profiling.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1302-paper.pdf)
- **解读**: 针对 AR/VR 设备（如 Meta Quest）禁用并发独立应用执行、使基于性能计数器的应用间监控不可行的问题，论文提出 OVRWatcher，一种通过后台脚本监控低分辨率（1Hz）GPU 占用率推断用户活动的侧信道原语，不要求并发应用执行、访问应用数据或安装额外 SDK，捕获不同速度、距离与渲染场景下 GPU 指标与 3D 对象交互间的相关性。OVRWatcher 可对独立 AR/VR 与 WebXR 应用进行指纹识别，还能区分虚拟对象（如沉浸式购物应用中真实用户选择的产品、虚拟会议的参与人数），揭示用户产品偏好并可能泄露会议机密信息，应用指纹识别准确率超 99%、对象级推断超 98%。


### SIPConfusion: Exploiting SIP Semantic Ambiguities for Caller ID and SMS Spoofing.
- **作者**: Qi Wang, Jianjun Chen, Jingcheng Yang, Jiahe Zhang, Yaru Yang, Haixin Duan
- **论文 PDF**: [SIPConfusion: Exploiting SIP Semantic Ambiguities for Caller ID and SMS Spoofing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s116-paper.pdf)
- **解读**: 针对 SIP 协议实现间的语义歧义可被攻击者利用进行身份冒充的问题，论文提出 SIPCHIMERA，一个系统性识别 SIP 实现中基于歧义的身份欺骗漏洞的黑盒模糊测试框架，对 6 个广泛使用的开源 SIP 服务器（含 Asterisk 与 OpenSIPS）与 9 个流行用户代理的评估发现，攻击者可操纵身份头字段冒用身份并绕过认证。为验证真实影响，作者评估了 5 个 VoIP 设备、7 个商用 SIP 部署与 3 个运营商级基于 RCS 的短信平台，证明攻击者可执行 VoIP 呼叫的来电显示伪造与 RCS 短信冒名发送，冒充任意用户或服务，并已负责任披露、给出缓解方案。


### Small Cell, Big Risk: A Security Assessment of 4G LTE Femtocells in the Wild.
- **作者**: Yaru Yang, Yiming Zhang, Tao Wan, Haixin Duan, Deliang Chang, Yishen Li, Shujun Tang
- **论文 PDF**: [Small Cell, Big Risk: A Security Assessment of 4G LTE Femtocells in the Wild.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1968-paper.pdf)
- **解读**: 针对 4G LTE 飞蜂窝（femtocell）虽已标准化并广泛部署但安全状况不明的问题，论文基于真实商用设备与大规模互联网测量开展首次系统性安全评估：对 4 款商用飞蜂窝设备的软硬件分析发现 5 个可导致本地或远程沦陷的关键共性漏洞，全网测量识别出 86,108 个疑似飞蜂窝部署且许多暴露于远程攻击。论文还在真实运营商网络中实验验证单个被攻破飞蜂窝可作为攻击移动核心网及其用户的强力入口，并将结果上报 GSMA 与 3GPP SA3；3GPP SA3 已批准加强 5G 飞蜂窝安全的研究项目及定义 5G 飞蜂窝安全保证规范（SCAS）的工作项目。


### SNPeek: Side-Channel Analysis for Privacy Applications on Confidential VMs.
- **作者**: Ruiyi Zhang, Albert Cheu, Adrià Gascón, Daniel Moghimi, Phillipp Schoppmann, Michael Schwarz, Octavian Suciu
- **论文 PDF**: [SNPeek: Side-Channel Analysis for Privacy Applications on Confidential VMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f699-paper.pdf)
- **解读**: 针对机密虚拟机（CVM）将侧信道泄漏排除在威胁模型之外、开发者缺乏系统高效方式度量与比较真实部署泄漏的问题，论文提出 SNPeek，一个开源工具包，在生产 AMD SEV-SNP 硬件上提供可配置的侧信道追踪原语，并结合统计与机器学习分析管线实现自动化泄漏估计。作者将 SNPeek 应用于部署在 CVM 上增强用户隐私的三类代表性负载——私有信息检索、私有 heavy hitters 与 Wasm 用户自定义函数——发现此前未注意的泄漏，包括一个以 497 kbit/s 窃取数据的隐蔽信道；结果显示 SNPeek 能定位漏洞并指导基于不经意内存与差分隐私的低开销缓解，为实践者提供带有效机密性保证的 CVM 部署路径。


### SoK: Analysis of Accelerator TEE Designs.
- **作者**: Chenxu Wang, Junjie Huang, Yujun Liang, Xuanyao Peng, Yuqun Zhang, Fengwei Zhang, Jiannong Cao, Hang Lu, Rui Hou, Shoumeng Yan, Tao Wei, Zhengyu He
- **论文 PDF**: [SoK: Analysis of Accelerator TEE Designs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1424-paper.pdf)
- **解读**: 针对加速器可信执行环境（TEE）研究多针对特定 CPU 或加速器、缺乏通用性与构建指南的问题，论文对多年来的加速器 TEE 设计进行整体分析，归纳出构建加速器 TEE 的典型框架，并总结从软件到物理的广泛攻击向量。论文对加速器 TEE 的三大安全机制——访问控制、内存加解密与证明（attestation）——进行系统化整理，逐方面比较现有研究中的安全解决方案并总结洞见，最后分析影响 TEE 在真实平台部署的因素，尤其是可信计算基（TCB）与兼容性问题。


### SoK: Cryptographic Authenticated Dictionaries.
- **作者**: Harjasleen Malvai, Francesca Falzon, Andrew Zitek-Estrada, Sarah Meiklejohn, Joseph Bonneau
- **论文 PDF**: [SoK: Cryptographic Authenticated Dictionaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1465-paper.pdf)
- **解读**: 针对认证字典（authenticated dictionary）研究分散、缺乏统一框架的问题，论文对其研究进行系统化梳理：首先给出统一框架刻画五种常见部署场景背后的信任与威胁假设；其次提炼并调和文献中分散的安全定义，阐明各自保证的语义与适用时机；再次建立构造分类学并分析渐近开销，揭示一个尖锐二分：已知方案要么查找与更新均为 O(log n)，要么以另一操作 O(n) 为代价换取单一操作的 O(1)，且这一屏障即使在引入更强信任假设后依然存在，颠覆“更多信任买来更高效率”的直觉。论文最后提出面向应用的研究问题，包括现实审计模型与当前完全无验证完整性系统的采纳激励。


### SoK: Take a Deep Step into Linux Kernel Hardening Effectiveness from the Offensive-Defensive Perspective.
- **作者**: Yinhao Hu, Pengyu Ding, Zhenpeng Lin, Dongliang Mu, Yuan Li
- **论文 PDF**: [SoK: Take a Deep Step into Linux Kernel Hardening Effectiveness from the Offensive-Defensive Perspective.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1725-paper.pdf)
- **解读**: 针对 Linux 内核虽经大量加固仍持续出现内存安全漏洞的问题，论文提出一个从攻击者视角将内核利用分解为三个阶段的系统化框架，对 2015 年以来 121 个公开记录的利用进行全面分析，识别并归类 64 个反复出现的攻击向量，并据此深入评估 51 个现有内核防御机制的覆盖、局限、冗余与相互依赖。结果揭示显著防护缺口：23 个攻击向量完全未受保护，31 个现有防御可被绕过或已过时；还发现主流下游发行版中理论有效性与实际部署的显著差距，指出 4 项未充分利用的加固措施与四个主流发行版的错误配置，为内核开发者与安全从业者提供可操作的加固与设计改进建议。


### SoK: Understanding the Fundamentals and Implications of Sensor Out-of-band Vulnerabilities.
- **作者**: Shilin Xiao, Wenjun Zhu, Yan Jiang, Kai Wang, Peiwang Wang, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **论文 PDF**: [SoK: Understanding the Fundamentals and Implications of Sensor Out-of-band Vulnerabilities.](https://www.ndss-symposium.org/wp-content/uploads/2026-s450-paper.pdf)
- **解读**: 针对传感器物理攻击研究零散、无限攻击信号空间使威胁抽象与防御复杂化的问题，论文提出“传感器带外（out-of-band, OOB）漏洞”系统化框架，首次基于底层物理原理为传感器攻击面提供全面抽象，并采用自底向上的方法在三个层面分析 OOB 漏洞：组件层识别导致漏洞的物理原理与局限，传感器层归类已知攻击并评估其实用性，系统层分析传感器融合、闭环控制与智能感知等 CPS 特性如何影响 OOB 威胁的暴露与缓解。研究为传感器硬件安全提供基础性理解，并为传感器设计者、安全研究人员与系统开发者构建更安全的传感器与 CPS 提供指导与未来方向。

### STIP: Three-Party Privacy-Preserving and Lossless Inference for Large Transformers in Production.
- **作者**: Mu Yuan, Lan Zhang, Yihang Cheng, Miao-Hui Song, Guoliang Xing, Xiang-Yang Li
- **论文 PDF**: [STIP: Three-Party Privacy-Preserving and Lossless Inference for Large Transformers in Production.](https://www.ndss-symposium.org/wp-content/uploads/2026-s35-paper.pdf)
- **解读**: 面向 Transformer 云服务（如在线聊天机器人），现有安全多方计算与同态加密方案计算开销过大，无法支撑大规模模型的实时推理。作者提出包含模型开发者、云模型服务器和数据所有者的三方威胁模型，并设计基于半对称置换的防护机制，构建了首个可部署于商用硬件的三方隐私推理系统 STIP。STIP 在形式上限制隐私泄露的同时保持无损推理精度，并集成可信执行环境抵御模型提取与微调攻击；在包含 700 亿参数模型的六类 Transformer 家族上评估，LLaMA2-7B 推理延迟仅 31.7ms，生产环境 70B 模型三个月在线测试仅带来 12% 的额外延迟且无隐私事件报告。


### Strategic Games and Zero Shot Attacks on Heavy-Hitter Network Flow Monitoring.
- **作者**: Francesco Da Dalt, Adrian Perrig
- **论文 PDF**: [Strategic Games and Zero Shot Attacks on Heavy-Hitter Network Flow Monitoring.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1301-paper.pdf)
- **解读**: 重击者（heavy-hitter）检测是线速 DDoS 缓解与限速的基础，但其面对自适应攻击者的韧性此前几乎未被探索。作者构建了将检测逻辑嵌入交换机级模拟器的端到端评估框架，用强化学习自动调参限制大象流，并让自适应攻击者学习在规避检测的同时最大化吞吐量，结果显示其可将配置带宽上限突破 299%。为加固系统，作者采用检测器与对手共同进化的联合对抗训练，使双方达到纳什均衡，将攻击者的带宽利用能力降低 2.2 倍；进一步用机器学习训练出的智能包合成器可在不了解目标检测系统的情况下对 9 个被测系统中的 8 个实施零样本带宽利用攻击。


### Success Rates Doubled with Only One Character: Mask Password Guessing.
- **作者**: Yunkai Zou, Ding Wang, Fei Duan
- **论文 PDF**: [Success Rates Doubled with Only One Character: Mask Password Guessing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1059-paper.pdf)
- **解读**: 与整体口令猜测攻击相比，基于侧信道（肩窥、屏幕污迹、键盘声学反馈等）获取口令部分信息（长度或部分字符）的掩码口令猜测威胁研究甚少。作者系统考察四种掩码猜测场景，提出基于神经网络的 PassSeq 与基于概率统计的 Kneser-Ney 两种口令模型，并结合极大似然估计给出精确高效的猜测次数估计方法。在 15 个大规模数据集上的实验表明，10 次猜测内：广撒网攻击者得知 4 位 PIN 的字符组成后成功率提升 152%；定向攻击者得知口令长度后成功率提升 47%-82%，再额外得知一个字符则成功率普遍翻倍；在 11 款真实键盘的声学侧信道数据上，PassSeq 使既有键盘击键推断攻击的成功率额外提升 5.6%-166.7%。


### SVDefense: Effective Defense against Gradient Inversion Attacks via Singular Value Decomposition.
- **作者**: Chenxiang Luo, David K. Y. Yau, Qun Song
- **论文 PDF**: [SVDefense: Effective Defense against Gradient Inversion Attacks via Singular Value Decomposition.](https://www.ndss-symposium.org/wp-content/uploads/2026-s114-paper.pdf)
- **解读**: 联邦学习中攻击者可从共享梯度重建私有数据（梯度反演攻击），现有防御要么在嵌入式平台上开销不可行，要么无法同时兼顾隐私保护与模型效用，且不少防御可被知晓细节的自适应攻击者绕过。作者提出 SVDefense，利用截断奇异值分解混淆梯度更新，包含三个关键创新：自适应客户端脆弱性的能量阈值、选择性保留关键梯度信息的通道加权近似，以及应对类别不平衡的分层加权聚合。在图像分类、人类活动识别和关键词识别等多个应用上，SVDefense 以极小的模型精度损失提供稳健的隐私保护，且可部署于多种资源受限的嵌入式平台。


### SYSYPHUZZ: the Pressure of More Coverage.
- **作者**: Zezhong Ren, Han Zheng, Zhiyao Feng, Qinying Wang, Marcel Busch, Yuqing Zhang, Chao Zhang, Mathias Payer
- **论文 PDF**: [SYSYPHUZZ: the Pressure of More Coverage.](https://www.ndss-symposium.org/wp-content/uploads/2026-s921-paper.pdf)
- **解读**: 现有内核模糊测试器以最大化代码覆盖率为目标，但覆盖率并不保证深入探索，且相关研究已趋于停滞，作者据此提出面向代码频率的内核模糊测试方向。通过细粒度研究 Syzkaller 在 Linux 内核中的覆盖率失衡现象，作者提出 SYSYPHUZZ，其选择性任务调度动态优先管理探索任务以避免任务爆炸，上下文保持变异策略降低破坏目标系统调用上下文依赖的风险。相比 SOTA 模糊器，SYSYPHUZZ 显著减少欠探索代码区域，发现 Syzkaller 漏掉的 31 个独特 bug 与 SyzGPT 漏掉的 27 个，并找到持续运行于数百台虚拟机上的 Syzbot 遗漏的 5 个 bug；与 SyzGPT 集成后独占 bug 数量提升 33%。


### Targeted Password Guessing Using k-Nearest Neighbors.
- **作者**: Zhen Li, Ding Wang
- **论文 PDF**: [Targeted Password Guessing Using k-Nearest Neighbors.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2077-paper.pdf)
- **解读**: 用户口令复用普遍，但现有定向口令猜测模型仅用相似口令对训练以刻画小幅修改行为，导致过拟合且忽略用户的大幅修改习惯（如 Shark0301 → Bear03）。作者提出非参数方法 KNN-TPG，构建保留所有源口令上下文向量与目标口令前缀的数据存储，生成新口令时检索 k 个最近邻向量以贴合真实口令分布，并创造性地与 Transformer 口令模型结合为 KNNGuess，在每一步预测三个分布以全面建模复用行为。在含 48 亿口令的 12 个真实数据集上，当受害者 A 站口令泄露时，100 次猜测内对 B 站不同口令的成功率为 25.40%（普通用户）与 10.26%（安全意识用户），较最强基线平均高 55.33%，较 Pass2Edit 与 PointerGuess 等 SOTA 平均高 18.09%。


### Targeted Physical Evasion Attacks in the Near-Infrared Domain.
- **作者**: Pascal Zimmer, Simon Lachnit, Alexander Jan Zielinski, Ghassan Karame
- **论文 PDF**: [Targeted Physical Evasion Attacks in the Near-Infrared Domain.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1568-paper.pdf)
- **解读**: 现有利用红外光源或吸热材料的物理攻击几乎只能实施非定向攻击，且因位置、形状等场景约束需要大量优化。作者提出一种隐蔽且低成本的攻击，用现成红外手电筒将透明薄膜上的扰动投射到目标物体，首次可靠实现红外域无激光定向攻击。在交通标志的数字与物理域实验中，该方法在明亮光照、不同距离与角度等场景下较先前工作取得更高的攻击成功率，部署成本低于 50 美元且仅需数十秒；作者还提出基于分割的新检测方法，以最高 99% 的 F1 分数挫败该攻击。


### TBTrackerX: Fantastic Trigger Bots and Where to Find Malicious Campaigns on X.
- **作者**: Mohammad Majid Akhtar, Rahat Masood, Muhammad Ikram, Salil S. Kanhere
- **论文 PDF**: [TBTrackerX: Fantastic Trigger Bots and Where to Find Malicious Campaigns on X.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1239-paper.pdf)
- **解读**: 在线社交网络上的恶意行为者使用脚本控制的社交机器人，仅在帖子中出现特定触发关键词时激活以欺骗用户，作者称这类上下文感知的机器人代理为触发机器人（TB）。作者提出 TBTrackerX 框架系统收集与分析 TB 活动，通过蜜罐账户捕获 2,647 个独特 TB 代理的 4,452 条回复，并发现其与 X 平台上逾 8.4 万用户的交互。研究显示 TB 通过上下文相似回复（相似度最高 0.97）、间歇性发帖（15 秒至 5 分钟的突发模式）以及活动高峰后转入休眠来逃避检测，并识别出由虚假 TB 粉丝与共享 TB 主控构成的协调生态系统。


### The Dark Side of Flexibility: Detecting Risky Permission Chaining Attacks in Serverless Applications.
- **作者**: Xunqi Liu, Nanzi Yang, Chang Li, Jinku Li, Jianfeng Ma, Kangjie Lu
- **论文 PDF**: [The Dark Side of Flexibility: Detecting Risky Permission Chaining Attacks in Serverless Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-s819-paper.pdf)
- **解读**: 无服务器平台的灵活性造成函数级分散权限配置与集中式云访问控制之间的根本性错配，常产生函数的风险权限，攻击者可串联多个具风险权限的函数以提权、接管账户甚至横向移动。作者提出以攻击者为中心的模态抽象，显式刻画来自不同函数与账户的独立权限如何合并为真实攻击链，并据此构建模态引导的检测工具。在 AWS 与阿里云两大云平台的官方生产级应用仓库上，分析发现 28 个易受攻击的应用，其中 5 个获确认 CVE、6 个获责任披露致谢、1 个获安全赏金。


### The Heat is On: Understanding and Mitigating Vulnerabilities of Thermal Image Perception in Autonomous Systems.
- **作者**: Sri Hrushikesh Varma Bhupathiraju, Shaoyuan Xie, Michael Clifford, Qi Alfred Chen, Takeshi Sugawara, Sara Rampazzi
- **论文 PDF**: [The Heat is On: Understanding and Mitigating Vulnerabilities of Thermal Image Perception in Autonomous Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s330-paper.pdf)
- **解读**: 热成像相机被日益视为低能见度条件下自动驾驶、机器人、无人机感知的可行方案，但其安全性与可信度是否媲美传统相机尚不明确。作者暴露并缓解热像仪均衡化、校准与透镜机制中三种固有的新型漏洞，这些漏洞可由环境中的自然或恶意热源触发，改变感知到的相对温度或产生时间可控伪影，破坏避障功能。在三款自动驾驶用热像仪、三个微调热目标检测器与两个可见-热融合模型上的评估显示，均衡化缺陷使行人检测平均精度平均下降 50%、融合模型下降 45%；40 km/h 真实道路测试中行人漏检率最高达 100%、假障碍生成成功率 91% 且攻击结束后持续数分钟；作者提出的三个威胁感知信号处理算法可动态检测并抑制攻击者诱导的伪影。


### The Role of Privacy Guarantees in Voluntary Donation of Private Health Data for Altruistic Goals.
- **作者**: Ruizhe Wang, Roberta De Viti, Aarushi Dubey, Elissa M. Redmiles
- **论文 PDF**: [The Role of Privacy Guarantees in Voluntary Donation of Private Health Data for Altruistic Goals.](https://www.ndss-symposium.org/wp-content/uploads/2026-s518-paper.pdf)
- **解读**: 自愿捐赠私人健康数据有助于研究，但隐私担忧常阻碍潜在捐赠者，而隐私增强技术（PETs）在鼓励数据共享方面的有效性尚不清楚。作者基于 Prolific 招募的 494 名美国参与者开展情景问卷实验，考察数据过期、匿名化、目的限制与访问控制四类通用保证，以及自审计与专家审计两种验证机制，并控制人口统计与营利/非营利收集者两类混淆因素。结果表明受访者对非营利机构先验隐私期望已很高，明确列出隐私保护对其总体感知影响甚微；而对营利机构，提供隐私保证会显著提升其隐私期望至接近非营利机构水平；此外，技术界推崇的审计透明化对信任提升效果有限，凸显技术界与终端用户对 PET 审计有效性认知之间的鸿沟。


### There is No War in Ba Sing Se: A Global Analysis of Content Moderation in Large Language Models.
- **作者**: Friedemann Lipphardt, Moonis Ali, Martin Banzer, Anja Feldmann, Devashish Gosain
- **论文 PDF**: [There is No War in Ba Sing Se: A Global Analysis of Content Moderation in Large Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f593-paper.pdf)
- **解读**: LLM 的内容审核行为在地理与语言语境间差异显著，作者首次对 15 个主流 LLM 从 12 个地点以 13 种语言对 1,118 个敏感查询（覆盖五类）返回的逾 70 万条回复进行系统分析。研究发现审核率存在明显的地理差异，相对差异最高达 60%（如软审核在德语语境占 14.3%、祖鲁语语境占 24.9%）；通用不安全、仇恨言论与色情内容比政治、宗教内容受到更严格审核，政治内容的地理变异性最大。此外，在线与离线模型版本行为不一致（DeepSeek 本地部署较 API 的软审核相对高 15.2%），且被审核回复平均比未审核回复短约 50%，说明不同地点用户获取的信息访问不一致，存在 AI 公平与数字公平问题。


### ThinkTrap: Denial-of-Service Attacks against Black-box LLM Services via Infinite Thinking.
- **作者**: Yunzhe Li, Jianan Wang, Hongzi Zhu, James Lin, Shan Chang, Minyi Guo
- **论文 PDF**: [ThinkTrap: Denial-of-Service Attacks against Black-box LLM Services via Infinite Thinking.](https://www.ndss-symposium.org/wp-content/uploads/2026-f639-paper.pdf)
- **解读**: LLM 云服务面临一类新威胁：攻击者构造特殊输入诱导模型进入过长或无限生成循环，耗尽后端计算资源，使合法用户的服务降级或中断；为缓解此类风险，服务商常采用闭源黑盒设置隐藏模型内部结构。作者提出 ThinkTrap，一个面向黑盒 LLM 服务的输入空间优化框架，其核心是将离散 token 映射到连续嵌入空间，利用输入稀疏性在低维子空间中进行高效黑盒优化，以最小 token 开销找出能诱导多个 SOTA LLM 延长或永不终止生成的对抗提示。在多个商业闭源 LLM 服务上的评估显示，即使远低于平台常见的每分钟 10 次请求频率限制，攻击也能将服务吞吐量降至原来的 1%，某些情况下甚至导致服务完全失败。


### Through the Authentication Maze: Detecting Authentication Bypass Vulnerabilities in Firmware Binaries.
- **作者**: Nanyu Zhong, Yuekang Li, Yanyan Zou, Jiaxu Zhao, Jinwei Dong, Yang Xiao, Bingwei Peng, Yeting Li, Wei Wang, Wei Huo
- **论文 PDF**: [Through the Authentication Maze: Detecting Authentication Bypass Vulnerabilities in Firmware Binaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2757-paper.pdf)
- **解读**: 路由器、网关等网络设备的嵌入式 Web 服务常暴露于公网，成为认证绕过攻击的目标，而现有检测技术依赖人工分析或刚性启发式，难以应对多样演进的认证方案。作者提出动态分析框架 AuthSpark，利用成功与失败认证尝试的执行轨迹相似性定位凭据检查，跟踪认证相关变量传播识别认证成功逻辑，再用定制灰盒模糊器配合任务特定功率调度与变异策略探索绕过路径。在含 14 个已知漏洞的 32 个真实设备固件上，AuthSpark 识别出 44 个凭据检查中的 42 个并检出全部 14 个已知漏洞；应用于最新固件时发现 6 个零日认证绕过漏洞，其中 4 个获得官方编号（3 个 CVE、1 个 PSV）。


### Tickets to Hide: An Inside Look into the Anti-Abuse Ecosystem through Internal Abuse Data.
- **作者**: Hugo L. J. Bijmans, Michel van Eeten, Rolf van Wegberg
- **论文 PDF**: [Tickets to Hide: An Inside Look into the Anti-Abuse Ecosystem through Internal Abuse Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f468-paper.pdf)
- **解读**: 反滥用治理依赖向网络所有者举报滥用以请求缓解的行业标准，但哪些滥用会得到跟进、缓解或忽略的决定背后有何依据尚不清楚。通过与荷兰执法部门的独特合作，作者获准访问一家以滥用著称的主机提供商的运营后台，罕见地窥见其内部滥用处理机制。研究发现客户通知率高度依赖举报者与滥用类别：CSAM 与垃圾邮件相关举报促成缓解行动，而版权侵权与端口扫描举报常被忽视；封锁名单、断连与执法质询等能直接影响业务连续性的治理工具会影响客户通知，而个人滥用举报则容易被忽略。


### Time and Time Again: Leveraging TCP Timestamps to Improve Remote Timing Attacks.
- **作者**: Vik Vanderlinden, Tom van Goethem, Mathy Vanhoef
- **论文 PDF**: [Time and Time Again: Leveraging TCP Timestamps to Improve Remote Timing Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s893-paper.pdf)
- **解读**: 时序侧信道攻击是推断秘密信息的经典手段，但网络抖动、处理器提速与系统复杂化使基于往返时间的远程时序攻击愈发困难。作者提出完全不受网络路径抖动影响的新型远程时序攻击方法，从服务器确认请求并发送响应时生成的 TCP 时间戳值推断执行时间，较基于 RTT 的时序攻击效率提升数倍且能检测更小的时序差异。作者还展示了如何利用服务器对请求的顺序处理放大秘密依赖操作的耗时以获得更精确的攻击；大量测量与真实案例研究表明，该技术所需前置条件更少、适用于任何 TCP 协议且可分布式执行。


### Time will Tell: Large-scale De-anonymization of Hidden I2P Services via Live Behavior Alignment.
- **作者**: Hongze Wang, Zhen Ling, Xiangyu Xu, Yumingzhi Pan, Guangchi Liu, Junzhou Luo, Xinwen Fu
- **论文 PDF**: [Time will Tell: Large-scale De-anonymization of Hidden I2P Services via Live Behavior Alignment.](https://www.ndss-symposium.org/wp-content/uploads/2026-f114-paper.pdf)
- **解读**: 现有 I2P 去匿名化方法聚焦于在大量网络流量中识别目标隐藏服务的流量模式，难以在由众多路由器构成的大型异构 I2P 网络上有效扩展。作者提出低成本方法 I2PERCEPTION，攻击者部署 floodfill 路由器被动监控并收集 I2P 路由器的 RouterInfo，通过分析路由器信息发布机制精确识别路由器的上线与下线行为，实现全网细粒度在线行为推断，再用主动探测获取目标隐藏服务的在线模式，通过时间上的行为关联缩小匹配路由器集合以揭示其真实网络身份。仅部署 15 个 floodfill 路由器运行八个月，真实世界实验即成功去匿名化所有受控隐藏服务。


### TIPSO-GAN: Malicious Network Traffic Detection Using a Novel Optimized Generative Adversarial Network.
- **作者**: Ernest Akpaku, Jinfu Chen, Joshua Ofoeda
- **论文 PDF**: [TIPSO-GAN: Malicious Network Traffic Detection Using a Novel Optimized Generative Adversarial Network.](https://www.ndss-symposium.org/wp-content/uploads/2026-f3241-paper.pdf)
- **解读**: 检测零日漏洞等高级网络威胁颇具挑战，作者提出 TIPSO-GAN，将 GAN 训练框架化为群智能优化问题以解决基于 GAN 的入侵检测中训练不稳定与模式崩溃等常见问题。为增强粒子群优化，该方法采用自适应惯性权重平衡探索与开发、多样性保持策略防止过早收敛，以及重置停滞粒子的反馈回路；并集成迁移学习与时间衰减多头自注意力以优先关注近期特征，目标函数结合重建损失与 focal loss。在 CIC-IDS2018、CICAPT-IIoT2024 与 CIC-DDoS2019 上分别取得 99.1±0.1、98.9±0.1、98.7±0.1 的 F1，LOFO 零日评估达 92.3 F1、跨数据集实验为 79-83 F1 且召回保持 0.80 以上，同时维持 0.42ms 延迟、约 2400 flows/s 吞吐与 2.1GB 占用。


### To Shuffle or not to Shuffle: Auditing DP-SGD with Shuffling.
- **作者**: Meenatchi Sundaram Muthu Selva Annamalai, Borja Balle, Jamie Hayes, Emiliano De Cristofaro
- **论文 PDF**: [To Shuffle or not to Shuffle: Auditing DP-SGD with Shuffling.](https://www.ndss-symposium.org/wp-content/uploads/2026-f597-paper.pdf)
- **解读**: DP-SGD 传统上用 Poisson 子采样选择批次，而洗牌因其更好的兼容性与更低计算开销日益流行，但洗牌下的紧致理论 DP 保证仍是开放问题，导致用洗牌训练的模型常按 Poisson 子采样假设评估，可能给出错误的隐私保证。作者定义新的 DP 审计程序分析带洗牌的 DP-SGD，在批次大小、隐私预算与威胁模型等维度上测量其隐私泄露估计能力。结果显示此类模型显著高估其隐私保证（最高达 4 倍），但理论 Poisson 保证与实际泄露之间的差距并非在所有参数与威胁模型下均匀；两种常见洗牌变体还会导致最多 10 倍的额外泄露，凸显缺乏严格分析方法时以洗牌替代 Poisson 子采样的风险。


### Token Time Bomb: Evaluating JWT Implementations for Vulnerability Discovery.
- **作者**: Jingcheng Yang, Enze Wang, Jianjun Chen, Qi Wang, Yuheng Zhang, Haixin Duan, Wei Xie, Baosheng Wang
- **论文 PDF**: [Token Time Bomb: Evaluating JWT Implementations for Vulnerability Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-f697-paper.pdf)
- **解读**: JWT 是现代分布式 Web 应用中广泛采用的认证授权标准，但其实现引入了签名验证绕过、token 伪造与拒绝服务等多种漏洞，且此前缺乏对 JWT 实现的系统性研究。作者提出测试方法 JWTeemo，对 10 种主流编程语言的 43 个 JWT 实现进行评估，发现 31 个此前未知的安全漏洞，其中 20 个获得 CVE 编号，并演示了其安全影响，如 Kubernetes 中的认证绕过与针对 Apache James 的拒绝服务攻击。作者将漏洞归纳为五类并提出多项缓解策略，相关建议已与 IETF 讨论并被建议纳入新 RFC，同时获得 Apache、Connect2id、Kubernetes、Let's Encrypt 与 RedHat 等厂商的致谢与漏洞赏金。


### Towards Effective Prompt Stealing Attack against Text-to-Image Diffusion Models.
- **作者**: Shiqian Zhao, Chong Wang, Yiming Li, Yihao Huang, Wenjie Qu, Siew-Kei Lam, Yi Xie, Kangjie Chen, Jie Zhang, Tianwei Zhang
- **论文 PDF**: [Towards Effective Prompt Stealing Attack against Text-to-Image Diffusion Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1899-paper.pdf)
- **解读**: 文生图模型的生成质量依赖精心设计的提示词，提示词已成为有价值的知识产权，而展示 AI 作品吸引买家的商业模式恰好暴露于提示词窃取攻击；现有 SOTA 攻击从固定修饰符集合重建提示词并需模型特定训练，适应性受限。作者提出 Prometheus，一种免训练、代理在环的搜索式提示词窃取攻击，通过交互本地代理模型逆向目标作品提示词：以 NLP 分析实时生成补充静态修饰符的动态修饰符、设计上下文匹配算法离线排序修饰符缩小搜索空间、并用贪心搜索基于代理反馈逐步精炼提示词。针对 PromptBase、AIFrog 等平台以及 Midjourney、Leonardo.ai、DALL·E 等受害者模型的评估显示 ASR 提升 25.0%，且对多种潜在防御具有抵抗力。


### TranSPArent: Taint-style Vulnerability Detection in Generic Single Page Applications through Automated Framework Abstraction.
- **作者**: Senapati Diwangkara, Yinzhi Cao
- **论文 PDF**: [TranSPArent: Taint-style Vulnerability Detection in Generic Single Page Applications through Automated Framework Abstraction.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1721-paper.pdf)
- **解读**: SPA 框架以新形式重现不安全的 DOM API（如 SPA 组件参数成为污点汇聚点），而现有 SPA 漏洞检测工作严重依赖需为每个框架人工维护的硬编码污点汇聚点，并可能漏掉某些不安全的 SPA API。作者提出 TranSPArent，结合静态与动态分析自动抽象 SPA 框架以揭示框架特定汇聚点，先对不安全 DOM API 列表做后向污点分析直至框架接口，再在各应用中寻找检测到的 SPA 汇聚点与攻击者可控源之间的数据流路径。在 GitHub 仓库数据库上的评估发现 11 个零日漏洞，包括一个拥有 24k+ star、月请求量 3000 万的仓库；从 Vue、React、Angular 三个最常用框架中发现 19 个中间 SPA 汇聚点，其中 14 个不在 SOTA 静态分析工具 CodeQL 标准库的清单中。


### Trust Me, I Know This Function: Hijacking LLM Static Analysis using Bias.
- **作者**: Shir Bernstein, David Beste, Daniel Ayzenshteyn, Lea Schönherr, Yisroel Mirsky
- **论文 PDF**: [Trust Me, I Know This Function: Hijacking LLM Static Analysis using Bias.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2066-paper.pdf)
- **解读**: LLM 日益被信任用于大规模自动化代码审查与静态分析，作者识别并利用其关键漏洞——使模型过度泛化熟悉编程模式而忽视细小但有意义的 bug 的抽象偏差。攻击者可用极小的编辑劫持 LLM 对代码的解释而不影响实际运行行为，作者将此攻击称为熟悉模式攻击（FPA），并开发全自动黑盒算法在目标代码中发现与注入 FPA。评估表明 FPA 对基础模型与推理模型均有效，可跨模型家族（OpenAI、Anthropic、Google）迁移、跨语言（Python、C、Rust、Go）通用，即使在系统提示词明确警告攻击的情况下仍然有效；作者还探讨了 FPA 的防御性正面用途及其对面向代码 LLM 可靠性与安全性的更广泛影响。


### UIEE: Secure and Efficient User-space Isolated Execution Environment for Embedded TEE Systems.
- **作者**: Huaiyu Yan, Zhen Ling, Xuandong Chen, Xinhui Shao, Yier Jin, Haobo Li, Ming Yang, Ping Jiang, Junzhou Luo
- **论文 PDF**: [UIEE: Secure and Efficient User-space Isolated Execution Environment for Embedded TEE Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s208-paper.pdf)
- **解读**: 嵌入式 TEE 为维持最小可信计算基仅提供安全关键功能、内存占用小，导致软件资源匮乏，难以在 TEE 内执行代码库庞大的复杂应用。作者提出用户态隔离执行环境 UIEE，通过动态分配足够内存并为应用构建沙箱、将其与富执行环境（REE）和 TEE 双重隔离，从而在不大幅增加 TCB 的前提下直接在 TEE 内运行未修改的数据处理应用；配套基于 Linux 内核库（LKL）的运行时提供标准 C 运行时 API，并设计 LKL 线程同步机制与按需线程迁移机制解决单线程执行模型下的并发问题。原型在 NXP IMX6Q SABRE-SD 评估板上成功无修改运行 8 个真实 libc 应用，性能开销可忽略，是首个 TrustZone 导向的 LibOS。


### Understanding the Status and Strategies of the Code Signing Abuse Ecosystem.
- **作者**: Hanqing Zhao, Yiming Zhang, Lingyun Ying, Mingming Zhang, Baojun Liu, Haixin Duan, Zi-Quan You, Shuhao Zhang
- **论文 PDF**: [Understanding the Status and Strategies of the Code Signing Abuse Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2857-paper.pdf)
- **解读**: 数字证书签名是软件可信性与完整性的重要保障，但攻击者可滥用该机制为恶意样本获取签名以助长恶意软件分发，且问题持续升级。作者基于从野捕获的 3,216,113 个签名恶意 PE 文件开展大规模测量，通过细粒度分类识别出 43,286 个被滥用证书并归纳为五类滥用类型，构建了迄今最大的标注数据集。分析显示滥用广泛存在，波及 114 个国家由 46 个证书颁发机构签发的证书，并观察到攻击者技术的演进与当前证书吊销机制的局限；作者刻画了攻击者五种规避检测、降低成本与增强影响的策略，发现 3,484 个多态证书簇，并首次记录了恶意软件利用多态性逃避吊销检查的真实实例。


### Understanding the Stealthy BGP Hijacking Risk in the ROV Era.
- **作者**: Yihao Chen, Qi Li, Ke Xu, Zhuotao Liu, Jianping Wu
- **论文 PDF**: [Understanding the Stealthy BGP Hijacking Risk in the ROV Era.](https://www.ndss-symposium.org/wp-content/uploads/2026-s97-paper.pdf)
- **解读**: ROV 的部分部署带来被称为隐蔽 BGP 劫持的意外安全威胁——恶意路由在不触及（从而不惊动）受害者的前提下转移流量，而此前既无真实世界事件记录也无系统刻画。作者形式化隐蔽 BGP 劫持并提出基于路由表差异的启发式发现方法，完成首个野外观测研究并贡献了策展的真实事件数据集与长期监测服务；受实证启发进一步开发 SHAMAN 推断框架，融合多源数据构建准确的 ROV 部署视图、以高效矩阵方法推断全网路由，并通过"受害者-目标-劫持者"三元组模型进行统计风险分析。SHAMAN 将互联网规模路由生成时间从三个多月降至 5.22 小时，在真实 ROV 部署下评估 83 亿条路由，发现隐蔽劫持总体成功概率为 14.1%，定向攻击在特定情形下成功率高达 99.5%，对真实数据集的验证达到 95.9% 的事件级准确率。


### Unknown Target: Uncovering and Detecting Novel In-Flight Attacks to Collision Avoidance (TCAS).
- **作者**: Giacomo Longo, Giacomo Ratto, Alessio Merlo, Enrico Russo
- **论文 PDF**: [Unknown Target: Uncovering and Detecting Novel In-Flight Attacks to Collision Avoidance (TCAS).](https://www.ndss-symposium.org/wp-content/uploads/2026-f1806-paper.pdf)
- **解读**: TCAS 是防空中相撞的最后一道强制安全防线，其未认证、未加密的通信协议是长期已知的安全风险，但官方通告认为相关漏洞局限于实验室环境且尚无缓解措施。作者挑战这两种论断，通过分析涉及多架飞机的异常事件的公开飞行与通信数据，识别出与幽灵飞机注入攻击一致的独特签名，并详述该攻击如何利用传统协议特性及三种复杂度递增的策略，其中最具攻击性的策略可将目标感知距离缩短逾 3.5 公里，足以在相当距离外触发受害飞机的防撞告警。作者实现了与观测事件最一致的攻击策略并实验验证 1.9 km 的距离缩减；还提出一种向后兼容的方法，通过重利用受害者广播的 TCAS 告警数据地理定位攻击源，在最可能攻击变体的模拟场景中达到 855 米的中位定位精度，并应用于真实事件数据识别出异常及幽灵飞机注入攻击的可能源头。


### Unshaken by Weak Embedding: Robust Probabilistic Watermarking for Dataset Copyright Protection.
- **作者**: Shang Wang, Tianqing Zhu, Dayong Ye, Hua Ma, Bo Liu, Ming Ding, Shengfang Zhai, Yansong Gao
- **论文 PDF**: [Unshaken by Weak Embedding: Robust Probabilistic Watermarking for Dataset Copyright Protection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1356-paper.pdf)
- **解读**: 在数据即服务生态中，恶意数据策展人可出售贡献者数据而不告知原贡献者，侵入式水印是检测可疑模型是否携带预定模式以保护数据版权的 SOTA 技术，但现有方法在低水印注入率（≤1.0%）下失效、性能下降、存在误报且不抗水印清洗。作者提出 DIP 概率水印方法：采用分布感知的样本选择算法，在水印样本与多个输出之间嵌入概率关联，并利用推理结果及其分布作为水印信号的双重验证框架。在 4 个图像与 5 个文本数据集上，DIP 在 1% 注入预算下平均水印成功率达 89.4%，保持模型性能，与多种水印数据设计正交可无缝集成，并在多模态、回归任务及 LLM 生成任务上有效；在数据增强、数据清洗、鲁棒训练与共谋移除等对抗环境下保持鲁棒，而现有 SOTA 方法失败。


### Unveiling BYOVD Threats: Malware's Use and Abuse of Kernel Drivers.
- **作者**: Andrea Monzani, Antonio Parata, Andrea Oliveri, Simone Aonzo, Davide Balzarotti, Andrea Lanzi
- **论文 PDF**: [Unveiling BYOVD Threats: Malware's Use and Abuse of Kernel Drivers.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1491-paper.pdf)
- **解读**: BYOVD 攻击滥用含隐藏缺陷的合法签名 Windows 驱动进入内核、禁用安全控制并维持从勒索软件到国家资助间谍活动的隐蔽行动，而大多数公开沙箱只检查用户态活动，此类内核级滥用常被忽视。作者基于真实事件人工调查与细粒度内核跟踪分析，提出首个 BYOVD 行为动态分类学，将每次攻击映射到顺序阶段并枚举各阶段被滥用的关键 API；随后提出基于虚拟化的沙箱，跟踪驱动从用户态请求到最底层内核指令的完整执行路径而无需驱动重签名或主机修改，并自动为每个观测动作标注分类学对应项生成逐阶段报告。在对 8,779 个加载 773 个不同签名驱动的恶意软件样本的分析中，沙箱标记出 48 个驱动存在可疑行为，人工验证后向微软、厂商与公开威胁情报平台负责任披露了 7 个此前未知的漏洞驱动。


### User-Space Dependency-Aware Rehosting for Linux-Based Firmware Binaries.
- **作者**: Chuan Qin, Cen Zhang, Yaowen Zheng, Puzhuo Liu, Jian Zhang, Yeting Li, Weidong Zhang, Yang Liu, Limin Sun
- **论文 PDF**: [User-Space Dependency-Aware Rehosting for Linux-Based Firmware Binaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s249-paper.pdf)
- **解读**: 固件重托管是规模化动态分析固件二进制的基础，成功重托管 Linux 固件服务需要同时正确模拟设备接口等系统级功能与配置文件、进程间通信等用户态依赖，但现有方案对用户态知识的利用不足，init 例程常被不完整执行且对所有模拟失败一视同仁。作者提出 FIRMWELL，将固件重托管建模为目标二进制与其用户态依赖的协调模拟：先重托管 init 例程构建环境再启动目标（通常涉及上百个进程），失败时识别阻塞进程、分析被错误模拟的资源并施加定向修复，核心策略是以程序分析精确推断资源值、通过纠正底层系统级模拟错误来解决用户态依赖失败。在 14,049 个固件镜像上成功重托管 6,490 个服务，较 SOTA 提升 1.6-8 倍（FirmAE 3,581、Greenhouse 3,962、Pandawan 810），平均重托管时间缩短 1.8-8.4 倍；对 1,043 个镜像的模糊测试发现 67 个零日漏洞，其中 10 个获 CVE 编号。


### Validity Is Not Enough: Uncovering the Security Pitfall in Chainlink's Off-Chain Reporting Protocol.
- **作者**: Di Zhai, Jiashuo Zhang, Jianbo Gao, Tianhao Liu, Tao Zhang, Jian Wang, Jiqiang Liu
- **论文 PDF**: [Validity Is Not Enough: Uncovering the Security Pitfall in Chainlink's Off-Chain Reporting Protocol.](https://www.ndss-symposium.org/wp-content/uploads/2026-f458-paper.pdf)
- **解读**: 区块链预言机为智能合约输送链下交易所价格数据，Chainlink 的去中心化预言机网络中多个节点独立观测价格并运行离链报告（OCR）协议确定唯一价格，OCR 的有效性属性保证价格受诚实观测值约束，但真实世界下该界的具体范围以及拜占庭行为仍能诱导多少价格偏差此前未知。作者通过经验与理论分析深入研究拜占庭行为对 OCR 确定价格的影响：实证分析显示真实环境下拜占庭行为仍有充足空间左右确定价格，随后详述策略性偏移价格的拜占庭行为并形式化建模其影响。基于 Chainlink 真实价格数据的评估显示拜占庭行为可将价格偏移至 ETH 价格的 8.47%，案例分析表明被偏移价格的下游金融影响可达 10^5 美元量级、累积影响可达数百万美元，即使有效性保证下拜占庭行为仍造成不可忽视的影响。


### Vault Raider: Stealthy UI-based Attacks Against Password Managers in Desktop Environments.
- **作者**: Andrea Infantino, Mir Masood Ali, Kostas Solomos, Jason Polakis
- **论文 PDF**: [Vault Raider: Stealthy UI-based Attacks Against Password Managers in Desktop Environments.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1067-paper.pdf)
- **解读**: 密码管理器在传统浏览器环境中可借域名信息轻松挫败钓鱼攻击，但随着主流 Web 服务推出独立原生应用，密码管理器开始为桌面环境提供通用自动填充等功能，而其在桌面环境中的安全防护方式此前未知。作者对主流密码管理器（包括 1Password 与 LastPass）在 macOS、Windows、Linux 桌面环境中的自动填充相关功能展开首个系统性实证分析，发现不同管理器采用不同的桌面应用交互策略与差异悬殊的 UI 攻击防护水平：macOS 上可利用 OS 提供的 API 与检查实现高安全，而 Windows 上因 OS 限制普遍缺乏适当安全检查。作者在每个场景演示了允许其他应用绕过安全检查、通过不可观测的模拟按键隐秘窃取用户凭据、一次性密码与保险库密钥的概念验证攻击，并提出缓解对策；发现已披露给相关厂商，部分厂商已启动修复流程并获得漏洞赏金。


### VDORAM: Towards a Random Access Machine with Both Public Verifiability and Distributed Obliviousness.
- **作者**: Huayi Qi, Minghui Xu, Xiaohua Jia, Xiuzhen Cheng
- **论文 PDF**: [VDORAM: Towards a Random Access Machine with Both Public Verifiability and Distributed Obliviousness.](https://www.ndss-symposium.org/wp-content/uploads/2026-s16-paper.pdf)
- **解读**: 可验证随机访问机（vRAM）是表达带可证明安全保证的复杂计算的基础模型，但现有 vRAM 均不提供分布式混淆性——多证明者既要防其他证明者又要防验证者的场景的关键需求，根源在于 MPC 与 ZKP 之间的范式错配限制了实用多证明者 ZKP 前端的开发。作者提出 CompatCircuit，据其所知首个弥合该鸿沟的多证明者 ZKP 前端实现，将协作 zkSNARK 与新型 MPC 协议集成，把计算与验证统一到单一兼容电路范式；并在此基础上构建 VDORAM，首个公共可验证的分布式混淆 RAM，在在线 MPC 的高通信延迟与离线证明生成的复杂度之间取得平衡。CompatCircuit 与 VDORAM 以约 15,000 行代码实现，微基准、对比分析与程序示例等大量实验证明其实用可行性。


### VeriLoRA: Fine-Tuning Large Language Models with Verifiable Security via Zero-Knowledge Proofs.
- **作者**: Guofu Liao, Taotao Wang, Shengli Zhang, Jiqun Zhang, Long Shi, Dacheng Tao
- **论文 PDF**: [VeriLoRA: Fine-Tuning Large Language Models with Verifiable Security via Zero-Knowledge Proofs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2361-paper.pdf)
- **解读**: 微调是适配大语言模型的关键，但计算密集且在不可信环境中存在正确性与隐私顾虑，LoRA 等参数高效方法虽大幅降低资源需求，零知识约束下的微调安全与可验证性仍是未解挑战。作者提出 VeriLoRA，首个将 LoRA 微调与零知识证明集成的框架，采用查找参数、sumcheck 协议与多项式承诺等密码学技术验证 Transformer 架构中的算术与非算术操作。该框架为 LoRA 微调的前向传播、反向传播与参数更新提供端到端可验证性，同时保护模型参数与训练数据隐私；基于 GPU 的实现对 LLaMA 等开源 LLM 的实验验证了其实用性与高效性，可扩展至 130 亿参数规模。


### VICTOR: Dataset Copyright Auditing in Video Recognition Systems.
- **作者**: Quan Yuan, Zhikun Zhang, Linkang Du, Min Chen, Mingyang Sun, Yunjun Gao, Shibo He, Jiming Chen
- **论文 PDF**: [VICTOR: Dataset Copyright Auditing in Video Recognition Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f746-paper.pdf)
- **解读**: 视频识别系统日益普及，公开数据集既推动模型进步也面临滥用与侵权，但现有数据集版权方案主要针对图像域，视频数据的时域特性使版权审计在视频域仍属空白。作者提出 VICTOR，首个面向视频识别系统的数据集版权审计方法，设计通用且隐蔽的样本修改策略增强目标模型的输出差异，仅修改小比例样本（如 1%）即可放大已发布修改样本对目标模型预测行为的影响，并将模型对已发布修改样本与未发布原始样本的行为差异作为审计依据。多模型多数据集实验验证了 VICTOR 的优越性，并表明其在训练视频或目标模型的多种扰动机制下保持鲁棒。


### ViGText: Deepfake Image Detection with Vision-Language Model Explanations and Graph Neural Networks.
- **作者**: Ahmad Albarqawi, Mahmoud Nazzal, Issa Khalil, Abdallah Khreishah, NhatHai Phan
- **论文 PDF**: [ViGText: Deepfake Image Detection with Vision-Language Model Explanations and Graph Neural Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s303-paper.pdf)
- **解读**: 深度伪造技术威胁媒体真实性，传统检测方法面对定制化深度伪造时在泛化与鲁棒性上表现不佳。作者提出 ViGText，将图像与视觉大语言模型（VLLM）文本解释集成到图框架中，比缺乏具体性、难以揭示细微不一致的常规标题提供更注重上下文的分析：系统地将图像划分为补丁、构建图像图与文本图并集成后交给图神经网络分析，并通过空间与频域的多级特征提取捕捉细节。实验显示 ViGText 显著增强泛化能力，泛化评估下平均 F1 从 72.45% 提升至 98.32%，对未见过的稳定扩散模型微调变体泛化出色；面对基于 SOTA 基础模型的对抗攻击，召回率较其他检测方法提升 11.1%，且面对针对其图架构的定向攻击性能下降不超过 4%。


### vSim: Semantics-Aware Value Extraction for Efficient Binary Code Similarity Analysis.
- **作者**: Huaijin Wang, Zhiqiang Lin
- **论文 PDF**: [vSim: Semantics-Aware Value Extraction for Efficient Binary Code Similarity Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f213-paper.pdf)
- **解读**: 二进制代码相似性分析（BCSA）支撑恶意软件分析、漏洞检测与软件供应链安全，但现有技术很少利用寄存器与内存值的语义进行比较，且已有基于值的方法多聚焦于跨编译配置不变的值而遗漏更丰富的语义信息。作者识别出限制基于值 BCSA 的三个核心挑战：无法覆盖多样化取值行为的不可扩展值提取、噪声过滤不足使语义无关伪影（如全局地址）主导比较、以及低效脆弱的基于值匹配。作者提出 VSIM，系统捕获寄存器与内存操作计算出的值、过滤语义无关值、并对剩余值归一化与传播以支持鲁棒可扩展的相似性分析；大量评估显示 VSIM 在准确率、鲁棒性与可扩展性上持续优于 SOTA BCSA 系统，并跨架构与工具链泛化。


### VulSCA: A Community-Level SCA Approach for Accurate C/C++ Supply Chain Vulnerability Analysis.
- **作者**: Yutao Hu, Chaofan Li, Yueming Wu, Yifeng Cai, Deqing Zou
- **论文 PDF**: [VulSCA: A Community-Level SCA Approach for Accurate C/C++ Supply Chain Vulnerability Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s613-paper.pdf)
- **解读**: C/C++ 第三方库普及使供应链安全至关重要，现有 C/C++ 供应链漏洞分析方法要么只关注依赖识别导致误报，要么只重漏洞检测而忽视依赖、需要代价高昂的全仓库扫描。作者探索依赖构建与漏洞检测的合适粒度，提出社区级软件成分分析（SCA）方法：将项目调用图建模为社交网络并应用社区检测，以社区相似度建立项目与第三方库间的依赖关系；漏洞检测时在依赖社区内执行基于克隆的检测验证漏洞是否存在，并以两阶段可达性分析判断漏洞能否传播到目标项目。实现的 VulSCA 是首个集成漏洞检测与可达性分析的 C/C++ SCA 框架，F1 较 CENTRIS 与 OSSFP 提升 4-12%，供应链漏洞检测 F1 较版本法高 44-48%、较代码法高 17-23%，总开销低于所有基于代码的方法，并在广泛使用的开源项目中识别出 32 个此前未修补的供应链漏洞。


### Was My Data Used for Training? Membership Inference in Open-Source LLMs via Neural Activations.
- **作者**: Xue Tan, Hao Luan, Mingyu Luo, Zhuyang Yu, Jun Dai, Xiaoyan Sun, Ping Chen
- **论文 PDF**: [Was My Data Used for Training? Membership Inference in Open-Source LLMs via Neural Activations.](https://www.ndss-symposium.org/wp-content/uploads/2026-f474-paper.pdf)
- **解读**: 开源 LLM 的训练数据常大规模且未披露，版权或个人敏感内容是否被纳入训练成为成员推断问题，而现有方法主要依赖模型输出、忽视丰富的内部表征，访问受限导致效果欠佳。作者提出白盒训练数据检测方法，核心洞见是 LLM 各层神经元激活反映模型对输入数据的内部知识表征，可有效区分训练数据与非训练数据；为支撑研究还引入 WikiTection、NewsTection 与 ArXivTection 三个动态基准。在五个 LLM（GPT2-xl、LLaMA2-7B、LLaMA3-8B、Mistral-7B、LLaMA2-13B）上，WikiTection 基准的 AUC 约 0.98；对模型规模、输入长度与文本改写等因素的深入分析进一步验证了方法的鲁棒性与适应性。


### WBSLT: A Framework for White-Box Encryption Based on Substitution-Linear Transformation Ciphers.
- **作者**: Yang Shi, Tianchen Gao, Yimin Li, Jiayao Gao, Kaifeng Huang
- **论文 PDF**: [WBSLT: A Framework for White-Box Encryption Based on Substitution-Linear Transformation Ciphers.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2492-paper.pdf)
- **解读**: 白盒威胁模型下攻击者可完全访问与控制密码实现及其执行环境，而先前的白盒加密设计主要保护单个密钥相关表，使白盒与侧信道攻击仍能恢复密钥。作者观察到对表格边界进行模糊化可使攻击失效，据此提出 WBSLT，一个面向替代-线性变换（SLT）密码的表格化白盒实现设计框架：以线性与非线性变换保护密钥嵌入表，并将各组件的部分计算留给下一组件以缓解单密钥相关表被攻破的风险。为抵御差分计算分析与差分故障分析，框架集成掩码、洗牌与外部编码；理论分析表明其对多种攻击具有免疫性，多平台实验验证了 WBSLT 的加密性能与合理内存消耗。


### WCDCAnalyzer: Scalable Security Analysis of Wi-Fi Certified Device Connectivity Protocols.
- **作者**: Zilin Shen, Imtiaz Karim, Elisa Bertino
- **论文 PDF**: [WCDCAnalyzer: Scalable Security Analysis of Wi-Fi Certified Device Connectivity Protocols.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1049-paper.pdf)
- **解读**: Wi-Fi 联盟的 Wi-Fi Direct、Wi-Fi EasyConnect 与 Wi-Fi EasyMesh 等设备连接协议广泛应用于数十亿设备，但其设计的安全与隐私此前未被全面审视。作者提出 WCDCAnalyzer 形式化分析框架评估这些协议，并针对 Wi-Fi Direct 验证中状态爆炸导致的指数级内存增长问题，开发遵循组合推理范式的系统化分解方法：自动将给定协议分解为若干子协议、分别验证并合并结果。利用 WCDCAnalyzer 的分析新发现 10 个漏洞，包括认证绕过、隐私泄露与拒绝服务攻击，相关漏洞及实际攻击已在商用设备上验证并获得 Wi-Fi 联盟确认。


### What Do They Fix? LLM-Aided Categorization of Security Patches for Critical Memory Bugs.
- **作者**: Xingyu Li, Juefei Pu, Yifan Wu, Xiaochen Zou, Shitong Zhu, Qiushi Wu, Zheng Zhang, Joshua Hsu, Yue Dong, Zhiyun Qian, Kangjie Lu, Trent Jaeger, Michael J. De Lucia, Srikanth V. Krishnamurthy
- **论文 PDF**: [What Do They Fix? LLM-Aided Categorization of Security Patches for Critical Memory Bugs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s328-paper.pdf)
- **解读**: Linux 内核的安全补丁持续集成，但下游维护者常延迟采纳形成漏洞窗口，关键在于难以识别安全关键补丁，尤其是越界访问（OOB）与释放后使用（UAF）等可利用漏洞，而现有分类器只区分安全与非安全补丁、无法提供支撑优先级排序的细粒度漏洞类型分类。作者利用 commit 标题/消息与 diff 线索并辅以适当代码上下文，开发 DUALLM 双方法流水线，集成基于大语言模型与微调小语言模型的两种方法。DUALLM 达到 87.4% 的准确率与 0.875 的 F1，显著优于先前方案；在 5,140 个近期 Linux 内核补丁中识别出 111 个针对 OOB 或 UAF 的补丁，其中 90 个经人工确认真阳性（许多在补丁描述中无明确提示），并为两个识别出的 bug（一个 UAF、一个 OOB）构建了概念验证，其中之一实现了此前未知的控制流劫持。


### When Cache Poisoning Meets LLM Systems: Semantic Cache Poisoning and Its Countermeasures.
- **作者**: Guanlong Wu, Taojie Wang, Yao Zhang, Zheng Zhang, Jianyu Niu, Ye Wu, Yinqian Zhang
- **论文 PDF**: [When Cache Poisoning Meets LLM Systems: Semantic Cache Poisoning and Its Countermeasures.](https://www.ndss-symposium.org/wp-content/uploads/2026-f200-paper.pdf)
- **解读**: 语义缓存按语义相似度跨用户复用查询-响应对，已成为 Azure、AWS、阿里云等云服务商 LLM 服务基础设施的常见优化，但其安全性此前未被考察。作者首次展示语义缓存易受缓存投毒攻击：攻击者注入构造的缓存条目使其他用户收到攻击者定义的响应，并在多种场景下演示该攻击、在三大公有云上确认其实用性。作者评估现有对抗提示防御发现其对语义缓存投毒无效，进而提出新的防御机制，相比现有方法提供更好的保护，但完全缓解仍具挑战；研究揭示缓存投毒这一长期安全问题已在 LLM 系统中重现，且潜在风险可能延伸至语义缓存之外的其他缓存机制。


### When Focus Enhances Utility: Target Range LDP Frequency Estimation and Unknown Item Discovery.
- **作者**: Bo Jiang, Wanrong Zhang, Donghang Lu, Jian Du, Qiang Yan
- **论文 PDF**: [When Focus Enhances Utility: Target Range LDP Frequency Estimation and Unknown Item Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1397-paper.pdf)
- **解读**: 本地差分隐私（LDP）协议无需可信数据策展人即可收集随机化客户端消息，已在 Google、Apple、Microsoft 等实际部署，但频率估计协议仅对已知数据域有效。作者提出广义 Count Mean Sketch（GCMS）协议统一众多现有频率估计协议，显著改善通信、隐私与精度三方权衡，并给出通用效用分析框架以优化参数设计，据此提出最小化目标频率收集方差的 Optimal Count Mean Sketch（OCMS）框架。针对未知域数据收集，作者结合基于稳定性的直方图技术与加密-洗牌-分析（ESA）框架，借助辅助服务器在不访问原始数据消息的情况下构建直方图，该协议达到接近中心式 DP 的精度，同时提供本地级隐私保证并大幅降低计算成本。


### When Mixnets Fail: Evaluating, Quantifying, and Mitigating the Impact of Adversarial Nodes in Mix Networks.
- **作者**: Mahdi Rahimi
- **论文 PDF**: [When Mixnets Fail: Evaluating, Quantifying, and Mitigating the Impact of Adversarial Nodes in Mix Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2384-paper.pdf)
- **解读**: 混合网络通过让数据包独立穿越随机选择的混合节点来破坏包关联性，为客户端提供抵御强网络对手的通信匿名，但妥协 10%/5% 节点即可使所有与目的地通信量超过阈值（4MB/30MB）的客户端匿名性完全失效。为缓解此类漏洞，作者开发一组新颖的路径选择技术，在抵抗网络对手与抵御被妥协混合节点之间取得权衡；鉴于现有匿名性度量不足以量化混合网络中的对抗风险，还提出有效的经验与基于模拟的度量。理论、经验与模拟评估表明，所提方法将受妥协节点影响导致的脆弱性降低最多 80%，同时仅给网络对手带来有限优势；分析还揭示 SOTA 匿名性度量会产生误导性结果并影响了 Nym 的部分设计选择。


### WhiteCloak: How to Hold Anonymous Malicious Clients Accountable in Secure Aggregation?
- **作者**: Zhi Lu, Yongquan Cui, Songfeng Lu
- **论文 PDF**: [WhiteCloak: How to Hold Anonymous Malicious Clients Accountable in Secure Aggregation?](https://www.ndss-symposium.org/wp-content/uploads/2026-s142-paper.pdf)
- **解读**: 现有安全聚合方法（如 ACORN）虽保证输入数据的隐私与合规，却无法满足客户端匿名要求，而简单套用匿名凭据会让此前被识别的恶意客户端（如使用不合规数据者）通过更新凭据重新进入聚合轮次、逃避问责。作者提出 WhiteCloak，首个在客户端匿名下保证问责的安全聚合方案：要求每个客户端以匿名凭据参与每轮聚合，参与前须提交零知识证明验证其未被列入黑名单，从而阻止恶意客户端通过更换凭据逃避问责，且可无缝集成到现有框架。在 SHAKESPEARE 数据集上的联邦学习实验中，WhiteCloak 仅增加 1.77 秒处理时间与 35.68KB 通信开销，分别占 ACORN 总开销的 0.34% 与 0.1%。


### WiFinger: Fingerprinting Noisy IoT Event Traffic Using Packet-level Sequence Matching.
- **作者**: Ronghua Li, Shinan Liu, Haibo Hu, Qingqing Ye, Nick Feamster
- **论文 PDF**: [WiFinger: Fingerprinting Noisy IoT Event Traffic Using Packet-level Sequence Matching.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1083-paper.pdf)
- **解读**: 智能家居等 IoT 环境易受隐私推断攻击，攻击者可分析加密网络流量模式推断设备状态甚至人的活动，但现有基于 ML 的攻击在 Wi-Fi 等无线流量上因噪声与无线嗅探丢包而表现不佳，且通常只针对分块的 IoT 事件流量样本、难以同时跟踪多个事件。作者提出 WiFinger，一种针对噪声流量的细粒度多 IoT 事件指纹识别方法，将流量模式分类转化为子序列匹配问题，并引入新技术控制高时间复杂度同时保持高精度，且对训练样本量的依赖较低便于未来指纹更新。实验表明 WiFinger 在实用威胁模型下优于现有方法，各类 IoT 事件的平均召回率达 89%（对比方法分别为 49% 与 46%），且误报率几乎为零。


### XR Devices Send WiFi Packets When They Should Not: Cross-Building Keylogging Attacks via Non-Cooperative Wireless Sensing.
- **作者**: Christopher Vattheuer, Justin Feng, Hossein Khalili, Nader Sehatbakhsh, Omid Abari
- **论文 PDF**: [XR Devices Send WiFi Packets When They Should Not: Cross-Building Keylogging Attacks via Non-Cooperative Wireless Sensing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f926-paper.pdf)
- **解读**: 扩展现实（XR）设备面临键盘记录推断威胁，现有攻击多依赖声音或视觉模态且需视线或近距离（10 米内）。作者提出利用 WiFi 无线感知的新型键盘记录攻击，无需视线，可在远距离、跨建筑（最远 30 米）场景生效，仅需单个廉价的口袋大小接收装置，且首次无需单独的发射器、接收器或伪造热点。其核心是利用 WiFi 芯片组的安全漏洞：攻击者发送伪造的未加密包迫使受害者设备非自愿地自动回传 ACK 包，从而持续迫使头显 WiFi 芯片发包、采集大量信道状态信息（CSI）数据，再用新的无监督信号处理算法进行姿态估计与手部手指定位实现击键推断。在 Meta Quest 2 与 Meta Quest 3 上、1 至 30 米距离、-90° 至 +90° 角度、多用户与穿墙等多样化条件下的评估显示，对最长 15 字符的密码在整栋建筑内实现 78.6% 的 top-25 准确率。


### ZKSL: Verifiable and Efficient Split Federated Learning via Asynchronous Zero-Knowledge Proofs.
- **作者**: Yixiao Zheng, Changzheng Wei, Xiaodong Qi, Hanghang Wu, Yuhan Wu, Li Lin, Tianmin Song, Ying Yan, Yanqing Yang, Zhao Zhang, Cheqing Jin, Aoying Zhou
- **论文 PDF**: [ZKSL: Verifiable and Efficient Split Federated Learning via Asynchronous Zero-Knowledge Proofs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2008-paper.pdf)
- **解读**: 纵向联邦学习（VFL）的既有工作主要保护数据隐私，却忽视了参与者可能操纵本地模型执行发动完整性攻击；将零知识证明（ZKP）集成进训练可保证各方计算可验证而不泄露私有数据，但把深度模型训练直接编码为整体 ZKP 电路因电路设计复杂、参数承诺频繁、嵌入层证明昂贵以及同步证明生成阻塞迭代训练而不可行。作者提出 ZKSL，一个在恶意威胁模型下实现可验证训练的高效异步 VFL 框架：将深度神经网络划分为逐层电路并行生成证明，以轻量扩展 Privacy-Commitment PLONK（PC-PLONK）实现低成本的逐迭代参数承诺并保证输入输出一致性；嵌入层采用概率验证技术将证明复杂度从 O(Nnd) 降至 O(nd)；并引入异步计算-证明调度机制将证明生成与训练迭代解耦以减少流水线停滞。在 DeepFM 与 CNN 模型上的实验显示，ZKSL 将证明生成时间最高减少 73%，同时保持 99.4% 的准确率。
