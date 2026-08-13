---
title: NDSS 2026 录用论文深度解读（265 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, NDSS, 2026]
---

NDSS 2026 于 2026 年 2 月在加州圣地亚哥举行，共录用 265 篇论文。本文按研究方向分类，对每篇论文基于全文精读给出深度解读（含独立分析思考），并附论文 PDF 链接。

## 全部 265 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（19 篇）

#### CatBack: Universal Backdoor Attacks on Tabular Data via Categorical Encoding.
- **作者**: Behrad Tajalli, Stefanos Koffas, Stjepan Picek
- **方向**: AI/ML安全
- **解读**: **问题**：后门攻击研究集中于图像等数据，表格数据因数值与类别特征混合、特征弱相关、无空间信息，触发器设计困难；已有攻击只能作用于数值特征，能力受限。

**方法**：核心创新是把类别特征转换为浮点表示，统一特征空间使单一梯度扰动可作用于全部特征，干净准确率与 one-hot 等编码相当且不增加列数；投毒后可将数据集还原为原始形式隐藏攻击；在 4 种模型与 5 个数据集上评估（XGBoost 作黑盒）。

**结果**：白盒与黑盒场景 ASR 最高 100%，仅 1% 投毒率多数情况即超 90%；ACI 上优于 Tabdoor/Badnets 最高 95%/44%；在 Google Vertex AI 上全部数据集 ASR 均超 97% 且平台无告警；绕过 Spectral Signatures（最不利设置仍超 78%）与 Fine-Pruning（超 75% 失效），Neural Cleanse、Beatrix 全部未检出。

**我的思考**：首次把优化式通用触发器扩展到类别特征，云平台验证增强现实威胁可信度；局限是表格数据缺乏成熟的不可感知性度量、极度不平衡数据上个别设置不稳定。防御侧应推动表格数据专用后门检测研究。
- **论文 PDF**: [CatBack: Universal Backdoor Attacks on Tabular Data via Categorical Encoding.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1469-paper.pdf)

#### Constructive Noise Defeats Adversarial Noise: Adversarial Example Detection for Commercial DNN Services.

- **作者**: Meng Shen, Jiangyuan Bi, Hao Yu, Zhenming Bai, Wei Wang, Liehuang Zhu
- **方向**: AI/ML安全
- **解读**: 
商业DNN服务（MLaaS）面临对抗样本威胁，现有检测方法多需模型细节或训练数据集，在仅查询API的场景下精度显著下降。第三方检测器只能获得预测标签与置信度，需单次提交即可判定。

方法上，Falcon基于干净与对抗样本的噪声容忍度差异（对抗样本近决策边界）：构造性噪声加在干净样本上不改变标签、加在对抗样本上引起显著输出变化，两者标签不变的最大噪声强度之差为容忍间隙。三个模块：(1)噪声分布生成——仅用干净样本训练压缩-重构网络，以残差为噪声分布，避开CAM关键区域；(2)噪声强度生成——自监督回归网络，用互斥损失L1=1/σ与L2=||xₙ−x||₂联合优化；(3)检测——用score=Flag+(1−Cos)度量加噪前后输出差异，阈值0.5，仅2次查询。

实验覆盖4个数据集（CIFAR-10、ImageNet-10、ImageNet-1000、CelebA）、两个目标模型与10种攻击：FPR固定5%时TPR最高达99.10%；ImageNet-1000上检测DSA比最优基线TPR提升21.42%、整体提升超7%。在六个真实商业服务（百度、腾讯、AWS等）上达约80% TPR、5% FPR，较SOTA提升超13%；面对自适应攻击仍保持50%以上TPR，其他方法几乎失效。

我的思考：Falcon把噪声容忍度差异升华为可训练的构造性噪声框架，训练仅用干净样本、不依赖目标模型与攻击先验；双损失为FPR/TPR权衡提供机制解释。局限：固定阈值0.5依赖"干净样本score≈0"假设，仅覆盖范数受限攻击（物理域不适用）。影响：与基于历史查询的方法互补，单次查询检测适合实时场景，自适应攻击下仍有残存能力。

- **论文 PDF**: [Constructive Noise Defeats Adversarial Noise: Adversarial Example Detection for Commercial DNN Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s250-paper.pdf)

#### CryptPEFT: Efficient and Private Neural Network Inference via Parameter-Efficient Fine-Tuning.

- **作者**: Saisai Xia; Wenhao Wang; Zihao Wang; Yuhui Zhang; Yier Jin; Dan Meng; Rui Hou
- **方向**: AI/ML安全
- **解读**: 问题：公开预训练骨干+轻量适配器的参数高效微调（PEFT）已成为现代 ML 流水线标配，但推理时既要保护用户输入又要保护在敏感数据上微调出的私有适配器；把 MPC 直接套用到 PEFT 上，骨干与适配器之间的双向通信（TWC）导致多次加解密往返、加密计算量巨大，且中间解密会产生泄漏点，传统 PEFT 架构与端到端私有推理本质不兼容。

方法：作者提出首个面向私有推理的 PEFT 方案 CryptPEFT，核心是单向通信（OWC）架构：用户侧明文运行公开骨干，把中间结果加密后单向送入服务侧适配器全程密文计算，网络内零中间解密、仅最终输出解密；适配器设计上提出 LinAtten 线性注意力（用线性变换 L(QK^T)V 替代 Softmax）、LoRA 低秩分解、以 ReLU 替代 GELU、可调 scaler（取 0.5）并将适配器置于骨干深层；进一步用成本感知神经架构搜索（延迟模型 R²=0.9975）自动搜索头数 h、秩 r 与适配器数 s 的最优配置，基于 CrypTen 实现并在 UC 框架下证明半诚实安全。

结果：基于 ViT-B-16 在 CIFAR-10/100、Food-101、SVHN、Flowers-102 上评估：相对传统 PEFT 基线在 WAN（400Mbps/4ms）平均加速 250.39×，相对仅微调末层的 SFT 基线平均加速 20.62× 且精度平均提升 0.83%；CIFAR-100 上取得 85.47% 精度与仅 2.26 秒推理时延；加密参数减少 88.81%（0.80M vs 7.15M）、通信轮数减少 62.37%、通信量减少 96.45%，成本模型误差±4.3%，并相对 MPCViT 获得 9.19×–13.14× 加速。

我的思考：与既有"算子级近似/协议优化"路线不同，该工作从架构层面把加密计算严格限制在适配器内，思路更根本、收益量级更大；OWC 以结构而非协议保证隐私，设计优雅；局限在于半诚实威胁模型、骨干明文运行隐含"公共骨干安全"假设，且传统适配器在 SVHN 等分布偏移任务上 OWC 掉点高达 28.63%，泛化边界仍待验证。
- **论文 PDF**: [CryptPEFT: Efficient and Private Neural Network Inference via Parameter-Efficient Fine-Tuning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1102-paper.pdf)

#### Dataset Reduction and Watermark Removal via Self-supervised Learning for Model Extraction Attack.

- **作者**: Hao Luan, Xue Tan, Zhiheng Li, Jun Dai, Xiaoyan Sun, Ping Chen
- **方向**: AI/ML安全
- **解读**: 黑盒水印是 DNN 知识产权保护的关键手段，但现有模型提取攻击难以去除水印，且需数千次查询才能达到可用精度，查询成本与防御检测压力大。

SSLExtraction 先在无关分布数据上训练自监督编码器（SimCLR/MoCo/BYOL），把像素输入映射为与水印无关的特征；再将特征空间查询选择建模为 p-dispersion-sum 问题（证明 NP 完全），用贪心初始化加布尔格上偏置随机游走求解（2-近似保证）；最后提出基于假设检验的度量 r=p-value_w/p-value_b，刻画可疑模型相对水印模型与良性模型的"相对距离"。

在 CIFAR-10 的 margin-based 水印上，10K 查询即达 85% 精度（仅低于原模型 87% 两个点），超基线 15% 以上，WSR 由基线 30% 以上降至 5.39%；仅 500 查询即可达 70% 精度（基线需 10K+）；ImageNet 达 68–71%，对白盒水印也能压到约 10% 成功率。

把查询选择提升到语义特征空间并给出近似比保证是重要亮点，r 度量也为水印社区提供了更稳健的判定视角；局限在于需要 OOD 预训练编码器、硬标签假设下才表现突出，且对不同水印方案（如 MAT）的 WSR 下降幅度不一，需结合相对度量综合评估。

- **论文 PDF**: [Dataset Reduction and Watermark Removal via Self-supervised Learning for Model Extraction Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f223-paper.pdf)

#### DNN Latency Sequencing: Extracting DNN Architectures from Intel SGX Enclaves with Single-Stepping Attacks.

- **作者**: Minkyung Park, Zelun Kong, Dave (Jing) Tian, Z. Berkay Celik, Chung Hwan Kim
- **方向**: AI/ML安全
- **解读**: DNN 模型架构是高价值知识产权，SGX 等 TEE 虽被用于保护模型，但现有侧信道提取攻击要么不考虑 TEE 防护（如依赖共享内存的 Flush+Reload），要么局限于特定模型类型（GEMM 或 ReLU），缺乏通用手段。

DLS 用 SGX-Step 以 APIC 中断单步执行 enclave 内推理并记录每条指令的 IRQ 延迟，按页索引多轨迹平均与 Z 归一化降噪；用 CNN-BiLSTM 分类器（L2F）把延迟标注到函数，压缩出函数级执行流 FEF；再用隐半马尔可夫模型（L2BB）结合约束转移与扩展 Viterbi 恢复基本块级执行流 BBEF，最终映射层类型与超参数。

在 Darknet、TensorFlow Lite、ONNX Runtime 上架构恢复准确率分别达 97.3%、96.4%、93.6%；Darknet 上函数级信息恢复层数/层类型/关键超参数达 99.2%，基本块级超参数平均 90.9%；33 个函数类宏平均 F1 为 100%，98% 的 FEF 与 93% 的 BBEF 正确恢复；利用恢复架构后 FGSM 逃逸攻击对 LeNet、VGG-7 成功率升至 57.8% 与 83.4%，远超随机基线。

用 IRQ 延迟侧信道加"延迟序列→执行流→架构"的分层建模是巧妙且通用的思路，无需物理访问或架构先验；局限是需知晓目标库及版本、离线训练与手工映射成本高、测量噪声仍造成少量误分类（13/100 轨迹），且与参数提取、成员推断等攻击组合后的实际威胁值得防御方警惕。
- **论文 PDF**: [DNN Latency Sequencing: Extracting DNN Architectures from Intel SGX Enclaves with Single-Stepping Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1455-paper.pdf)

#### FlyTrap: Physical Distance-Pulling Attack Towards Camera-based Autonomous Target Tracking Systems.
- **作者**: Shaoyuan Xie, Mohamad Habib Fakih, Junchi Lu, Fayzah Alshammari, Ningfei Wang, Takami Sato, Halima Bouzidi, Mohammad Abdullah Al Faruque, Qi Alfred Chen
- **方向**: AI/ML安全
- **解读**: 
自主目标跟踪（ATT）无人机广泛应用于监控、边控与执法，也被用于跟踪骚扰等恶意场景。现有针对单目标跟踪（SOT）的攻击只会使无人机丢失目标，无法根本性消除威胁。本文首次定义"距离拉近攻击"（DPA）：利用 ATT 系统"框越小、飞得越近"的闭环距离控制逻辑，把无人机拉到可被网枪捕获、落入传感器攻击有效距离甚至直接撞击的范围内。

FlyTrap 用"对抗雨伞"作为可部署的物理载体（大平面、易携带、可自然朝无人机举起且不显眼）；提出渐进式拉距策略 PDP：以针孔相机模型推导收缩率与拉近距离的数学上界，用计算机图形学模拟逼近过程的闭环动态，生成随距离渐进收缩的对抗图案；攻击目标生成器 ATG 显式编码时空一致性（约束 box 形状、注入人体姿态、多模型联合优化、多阶段收缩率），以绕过一致性校验类防御。

数字评测中 FlyTrapPDP 平均 mASRopen 达 53.6%（人类形象基线 TGT 仅 36.0%）；白盒闭环实验中拉入捕获（9m）与传感器攻击（6m）范围成功率均 100%，加 PDP 后碰撞（0.5m）也达 100%；ATG 把 PercepGuard 检出率从 60.7% 压到 2.8%、VOGUES 报警率从 88.8% 降到 52.4%、全阈值绕过 VisionGuard；在 DJI Mini 4 Pro、DJI NEO、HoverAir X1 上完成黑盒端到端演示，实现捕获或坠机。

该工作把攻击从"破坏感知"提升到"系统级物理后果"，雨伞载体、闭环建模与时空一致性设计完整，200 人用户研究佐证隐蔽性。局限：核心为白盒假设（需逆向目标 SOT 模型）；带 PDP 图案跨模型迁移率仅 4.0%（无 PDP 人形图案 13.1%），存在拉距效果与可迁移性的权衡；且假设典型 20m 内跟踪距离。防御侧启示：现有一致性校验防御的假设与 ATT 固有的时空一致性不匹配，需重新设计。
- **论文 PDF**: [FlyTrap: Physical Distance-Pulling Attack Towards Camera-based Autonomous Target Tracking Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s904-paper.pdf)

#### Incident Response Planning Using a Lightweight Large Language Model with Reduced Hallucination.
- **作者**: Kim Hammar, Tansu Alpcan, Emil C. Lupu
- **方向**: AI/ML安全
- **解读**: 

事件响应目前高度人工化（研究显示组织平均 73 天才能响应并恢复），而现有 LLM 辅助方法多是对前沿模型做提示工程，成本高、依赖外部 API，且通用模型对事件响应不专业、极易幻觉（生成看似合理但错误的响应动作）。

方法分三步：用 68,000 个事件（500 个测试台真实事件加前沿模型合成）对轻量模型 DeepSeek-R1-14B（对比 671B 的前沿版）做指令微调；用 OTX 威胁情报做检索增强（RAG）把响应锚定到最新威胁信息；决策论前瞻规划——生成 N=3 个候选动作、各采样 M=3 次模拟结果，选预期恢复时间最短者并过滤幻觉。理论上证明幻觉概率有界且可任意小（以规划时间为代价）。

在 CTU-Malware-2014、CIC-IDS-2017、AIT-IDS-V2-2022、CSLE-IDS-2024 四个基准的 25 个真实攻击事件上，平均恢复时间 13.46，优于次优的 Gemini 2.5 Pro（16.21），最多缩短 22%；消融显示去微调恢复时间从 13 涨到 25、去规划涨到 21；与 PPO 强化学习方法性能相当，但 PPO 需按事件训练 10–20 分钟，本方法零事件专属训练。

这是首个开源的事件响应微调 LLM 与数据集，理论界+经验验证的组合在安全 LLM 研究中少见。局限：恢复时间用离散单位假设（冗余步骤记 2），评估基于文献日志而非真实 SOC；RAG 贡献相对温和，说明微调与规划是性能主力。

- **论文 PDF**: [Incident Response Planning Using a Lightweight Large Language Model with Reduced Hallucination.](https://www.ndss-symposium.org/wp-content/uploads/2026-f358-paper.pdf)

#### InverTune: A Backdoor Defense Method for Multimodal Contrastive Learning via Backdoor-Adversarial Correlation Analysis.
- **作者**: Mengyuan Sun, Yu Li, Yunjie Ge, Yuchen Liu, Bo Du, Qian Wang
- **方向**: AI/ML安全
- **解读**: 

CLIP 等多模态对比学习模型已成为大规模多模态系统的基础组件，但其对后门攻击高度脆弱：攻击者植入触发后可跨下游任务持久控制模型行为（如 BadCLIP 绑定视觉触发与错配文本）。现有防御要么假设知道攻击目标、要么需要访问污染数据或大量干净数据，实用性差。

InverTune 在最小攻击者假设（不知目标、无污染数据）下做防御，基于一个关键观察：被后门化的多模态编码器中，通用对抗扰动（UAP）与后门样本在视觉空间形成独立簇，却在跨模态空间汇聚到同一目标。三步流水线：对抗模拟概率性识别目标标签（分析响应模式，免去开集标签穷举）；双空间（视觉嵌入+跨模态对齐）梯度反演重建触发；聚类引导微调仅用少量任意干净数据擦除后门并保留原能力。

实验覆盖 6 种攻击（BadNet、Blended、SIG、WaNet、BadEncoder、BadCLIP）：平均 ASR 降低 97.87%，干净准确率仅降 3.07%；ImageNet 分类与 MSCOCO 检索上多数 ASR 压到 1.0% 以内；对最强的 BadCLIP 将 ASR 从 98.36% 降到 0.49%，抑制能力超过现有防御 17.78%，所需干净数据仅其 1/10；跨 CNN/Transformer 骨干平均 ASR 1.22%，自适应攻击下 ASR 也被压到近零。

首个在最小假设下同时达到"最低 ASR+最高 CA"双最优的多模态后门防御，工程完整度高。局限：目标识别依赖语义排序，用完全随机标签引导时 Top-1 ASR 高达 78.88%，防御效果对引导标签质量敏感；触发分布仍限于论文内攻击模型。

- **论文 PDF**: [InverTune: A Backdoor Defense Method for Multimodal Contrastive Learning via Backdoor-Adversarial Correlation Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1666-paper.pdf)

#### Light2Lie: Detecting Deepfake Images Using Physical Reflectance Laws.

- **作者**: Kavita Kumari, Sasha Behrouzi, Alessandro Pegoraro, Ahmad-Reza Sadeghi
- **方向**: AI/ML安全
- **解读**:
GAN 与扩散模型生成的图像日益逼真，深度伪造引发虚假信息、冒充与伪造内容等严重安全关切；现有检测方法（频域分析、深度分类器、VLM 方法）对未见生成模型泛化差、缺乏物理依据，易受自适应攻击且解释性有限。

Light2Lie 提出物理增强的检测框架：基于镜面反射的 Fresnel 反射模型与微表面（microfacet/Blinn）理论，先用神经网络估计图像表面基底反射率，再导出微表面启发的镜面响应图，编码真假图像在光—表面交互上的细微几何与光学差异；该信号作为特征图输入次级分类器，并引入反馈精炼机制用分类误差更新基底反射率输出，把物理建模与学习目标紧耦合。

在多个 GAN 与扩散模型数据集（ARIA、过滤后 LAION、CIFAKE 等）上，对未见生成模型样本取得最高 74.7% 准确率，跨多样深度伪造域最高 74% 精度，优于 SOTA 基线；基底反射率学习使合成图像约为 0.96、真实图像约为 0.04，形成清晰可分信号。

我的思考：以物理规律作为归纳偏置提升跨模型泛化与可解释性，思路新颖，生成模型难以系统性伪造光照物理，抗自适应攻击潜力好。局限在于极端光照、高光泽/金属材质与强压缩/滤波下表面特征可靠性下降，74% 量级的精度离实用仍有距离；与多模态语义方法结合、扩大训练域是自然的改进方向。

- **论文 PDF**: [Light2Lie: Detecting Deepfake Images Using Physical Reflectance Laws.](https://www.ndss-symposium.org/wp-content/uploads/2026-s923-paper.pdf)

#### Memory Backdoor Attacks on Neural Networks.

- **作者**: Eden Luzon, Guy Amit, Roy Weiss, Torsten Krauß, Alexandra Dmitrienko, Yisroel Mirsky
- **方向**: AI/ML安全
- **解读**: 神经网络常在专有数据集上训练，其参数会无意中记忆训练样本，成为隐私攻击目标。现有数据提取攻击依赖启发式猜测且产出可能是幻觉、样本常残缺、无法控制记忆哪些样本，隐写式方法又容量有限且易被剪枝/加噪清除。该工作提出"记忆后门"：恶意联邦学习（FL）服务器通过修改下发的训练代码，让客户端模型在收到索引触发器时重构输出记忆的训练样本，实现确定性的数据集窃取。

攻击的核心是结构化索引触发器：触发函数把索引映射为唯一触发图案，模型被训练为"按索引输出样本"；对分类器等小输出模型采用分块重构——把图像切成patch并扩展索引维度跟踪位置，再像拼图一样重组。服务器按轮次轮询指定客户端记忆不同子集，被感染的模型在聚合时被剔除，保证全局模型效用不变且不被察觉；越界索引只产生噪声，成为样本真实性的验证信号。

在FL端到端评估中，CIFAR100-ViT仅15轮即提取全部客户端完整数据集（平均SSIM 0.867），MNIST-FCN单轮即可提取（SSIM 0.921，3轮记忆后达0.966）；医学分割数据集提取时仅3%效用下降，一般场景效用损失0.1%–6.0%；攻击还扩展到LLM，可提取数千条训练对话。

该工作把后门范式从"触发器→错误分类"翻转为"触发器→数据重构"，确定性、高容量、抗剪枝且对架构无先验要求，是FL供应链上现实的隐私威胁。局限：威胁模型要求FL服务器被攻陷且客户端不审计底层损失函数（TEE可检测但开销大）。它凸显了分布式训练需要更强的完整性与透明度保障。
- **论文 PDF**: [Memory Backdoor Attacks on Neural Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1870-paper.pdf)

#### MIMIR: Masked Image Modeling for Mutual Information-based Adversarial Robustness.
- **作者**: Xiaoyun Xu, Shujian Yu, Zhuoran Liu, Stjepan Picek
- **方向**: AI/ML安全
- **解读**: 问题：ViT 是视觉与多模态大模型的骨干，但对逃逸攻击脆弱；现有对抗训练（AT）方法（Generalist、DBAT）与 ViT 不兼容，SOTA 增益常低于 2%。

方法：用互信息（MI）与信息瓶颈理论证明，ViT 自编码器预训练中对抗样本 x+δ 与潜在表示 z 的 MI 应被降低以提升鲁棒性；据此提出 MIMIR：以掩码图像建模（MAE，丢弃 75% 图像块）做对抗预训练，损失为 MSE 重构加 MI 惩罚，再对抗微调。

结果：三个数据集上自然与鲁棒精度一致提升，ImageNet-1K 达 RobustBench SOTA（ViT-B 自然 76.72%/AA 54.04%）；对未见攻击与 ImageNet-C 稳健，能抗自适应攻击（PGD-MI 几乎不降精度），消融显示优于 MAE（71.02%/37.22% vs 69.98%/35.64%）。

我的思考：用 IB/MI 上界为预训练提供理论依据，可推广到 ViT/Swin/ConvNext 三种架构；代价是 800 epoch 预训练开销大，PGD-MI 略优说明 MI 本身可被利用。

- **论文 PDF**: [MIMIR: Masked Image Modeling for Mutual Information-based Adversarial Robustness.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1813-paper.pdf)

#### PANDORA: Lightweight Adversarial Defense for Edge IoT using Uncertainty-Aware Metric Learning.

- **作者**: Avinash Awasthi, Pritam Vediya, Hemant Miranka, Ramesh Babu Battula, Manoj Singh Gaur
- **方向**: AI/ML安全
- **解读**: 资源受限的 IoT 设备急剧扩张攻击面，签名式 IDS 无法应对自动化零日攻击，机器学习 IDS 又面临概念漂移与无法泛化到未见威胁的问题；现有零样本学习方法用确定性嵌入与固定距离度量，无法刻画网络流量的内在不确定性，流形结构脆弱。

PANDORA 提出端到端框架：不确定性感知概率嵌入（编码器输出高斯参数 µ、σ²，以 Wasserstein 距离度量分布差异）；新的 PMSD Loss（Wasserstein 距离+Triplet 流形结构化）实现零样本泛化；Mamba-MoE 轻量编码器（线性复杂度）与可解释特征注意力，两模态（时间/体积特征）编码后跨注意力融合。同时发布 TTDFIoTIDS2025 数据集：470 万条流、63 个程序化攻击子类，类内碎片化程度高于现有基准。

在 CICIDS2017 上 10-shot 适应 F1 达 0.981；域偏移（CICIDS2017→2018）下零样本检测准确率 99%，而基线模型几乎完全失效；PMSD 在复杂数据集上 Macro F1 0.7001（欧氏距离仅 0.6488）；部署于树莓派 4B 时内存仅约 24 MB、吞吐 4.26 flows/s、时延 35-58 ms，而 Transformer 基线在复杂数据集上 F1 暴跌 30-45 个百分点。

我的思考：不确定性建模与分布距离是零日检测的合理方向，Mamba-MoE 的边缘部署验证补上了"实用性鸿沟"，自建数据集也有诚意。局限：4.26 flows/s 远低于真实网关的流量负载，实时性存疑；PTN-IDS 无公开实现需自行复刻，可比性打折扣；零样本 99% 与 1/5-shot 低分的结果矛盾，评估设置需审视；数据集自产自评存在选择偏差风险。

- **论文 PDF**: [PANDORA: Lightweight Adversarial Defense for Edge IoT using Uncertainty-Aware Metric Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-f713-paper.pdf)

#### QNBAD: Quantum Noise-induced Backdoor Attacks against Zero Noise Extrapolation.
- **作者**: Cheng Chu, Qian Lou, Fan Chen, Lei Jiang
- **方向**: AI/ML安全
- **解读**: 零噪声外推（ZNE）是NISQ时代变分量子算法（VQA）最常用的误差缓解手段，广泛用于药物发现、组合优化、分子能量估计等安全敏感场景，其输出可靠性直接关乎高利害应用。现有量子后门攻击要么改电路结构（只平移理想输出、不影响噪声外推过程，且易被检查发现），要么在参数层植入但训练时未考虑设备噪声（跨平台行为不一致、难以可靠触发）。本文提出QNBAD，揭示并实现专门针对ZNE特性的新一类噪声触发后门攻击。

QNBAD利用噪声感知训练的"双刃剑"效应：通过固定编译配置（目标设备、物理比特选择、初始映射、路由策略）构造确定且可复现的噪声模型作为触发器，训练时引入后门损失，使电路在目标噪声模型下系统性偏置各噪声级别的采样期望值，破坏ZNE拟合。攻击有三种模式：FreeDrift（在基准噪声级注入偏置、全力制造误差）、MimicSlope（所有噪声级均匀平移、斜率不变）、SilentShift（低噪声级伪装正常、压低高噪声级采样使拟合曲线变平）；并设计动态损失调节，按主任务收敛状态逐步引入后门目标以稳定训练。

在4台IBMQ设备（Cairo/Brooklyn/Guadalupe/Montreal）与6个应用（VQE、QAOA、VQD）上，绝对误差放大达1.68×~11.7×（FreeDrift几何均值最高达11.70×），远超电路级QTrojan（约0.99×~1.02×，基本无效）与参数级QDoor（不稳定，0.92×~1.66×）；仅改变一个编译参数（映射、路由、比特或设备）即令后门失活，且在线性/多项式/指数三种拟合函数及ZNE、DZNE、LRE变体下均保持有效。

我的思考：这是首个把攻击面定位于"误差缓解管线本身"的量子安全研究，"噪声即触发器"的思路新颖且隐蔽性强，参数迁移投毒场景（用户从开源库下载预训练参数）也很现实。实验覆盖多设备多应用、稳健性分析全面。局限在于攻击需精确预知目标设备与编译配置（改变任一环节即失效，这也是其双刃剑），噪声环境下无梯度后门训练收敛困难（作者用动态损失缓解但仍有波动），防御仅探讨了微调与跨噪声环境检测等初步思路，量子后门检测与缓解仍有大片空白。
- **论文 PDF**: [QNBAD: Quantum Noise-induced Backdoor Attacks against Zero Noise Extrapolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1665-paper.pdf)

#### Rethinking Fake Speech Detection: A Generalized Framework Leveraging Spectrogram Magnitude.
- **作者**: Zihao Liu, Aobo Chen, Yan Zhang, Wensheng Zhang, Chenglin Miao
- **方向**: AI/ML安全
- **解读**: 语音合成技术的逼真度使伪造语音威胁日益严重（如2019年冒充CEO诈骗24.3万美元、2021年克隆声音窃取3,500万美元）。现有检测方法在受控设定下有效，但依赖特定假设（活性线索、麦克风伪影）或vocoder元标签，面对新合成模型、语言与录音环境时泛化差，可退化到随机猜测，且缺乏对伪造语音共性伪影的系统理解。本文从谱图幅度这一被忽视的视角出发，重思伪造语音检测。

论文系统分析伪影成因：vocoder缺乏相位信息（高频小幅度区对相位误差更敏感）、训练目标不显式优化自然度（L1/L2损失偏向大幅值成分）、谱图合成中的维度扩张。分析发现合成语音在小幅度范围内系统性缺失纹理细节与能量（GLCM纹理指标可分、5000-7000Hz高频区缺失更明显）。据此提出无假设的通用框架：按幅度阈值把谱图划分为8层叠式子谱图，在空间域与2D-DCT域分别用ResNet18（2D）与TimeSformer（3D"视频"表示）提取伪影，MLP融合四分支输出；训练采用加权交叉熵（小幅度层权重更高）与层随机丢弃。

数据集内EER为0.1%/0.5%/0.3%（WaveFake/LibriSeVoc/In-the-Wild）全部最优；留一法（Leave-One-Out）WaveFake平均EER仅0.3%，远优于最佳基线Dual-Stream（3.5%），而依赖vocoder标签的RawNet2-Voc在未见vocoder上退化到40.2%；跨语言/跨数据集平均EER 10.9%（最佳基线LFCC-LCNN为14.0%）；在ElevenLabs等5个黑盒网页声音克隆API的真实评测中平均EER 8.3%，优于VoiceRadar（21.8%）等基线。

我的思考：把"伪影从何而来"的问题前置，从幅度谱小幅度区的纹理缺失出发设计检测器，比堆特征与模型更本质，且完全无假设、不依赖标签，跨方法/跨语言/真实API的泛化数据很有说服力（Leave-One-Out EER 0.3%尤佳）。局限：固定4秒输入长度、3D分支计算开销较大、幅度分层与DCT超参数依赖经验设定，对更强的对抗自适应攻击的鲁棒性尚待验证。整体上为音频深度伪造检测提供了可解释、可迁移的新范式，有望成为该领域的通用基线。
- **论文 PDF**: [Rethinking Fake Speech Detection: A Generalized Framework Leveraging Spectrogram Magnitude.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1024-paper.pdf)

#### Robust Fraud Transaction Detection: A Two-Player Game Approach.
- **作者**: Qi Tan, Yi Zhao, Laizhong Cui, Qi Li, Ming Zhu, Xing Fu, Weiqiang Wang, Xiaotong Lin, Ke Xu
- **方向**: AI/ML安全
- **解读**: 欺诈者可无限制地伪造交易特征以规避 ML 检测，基于小扰动假设的对抗训练等局部鲁棒方法失效，检测系统与攻击者之间呈现严重不对称。

GAMER 把检测形式化为检测系统与欺诈者之间的两人博弈，通过求解混合策略纳什均衡得到每个特征的最优选中概率 pi=min(1, Ci/ΔUi)，用 Bernoulli 掩码训练"选择器"模型，再与全特征"完整器"配合，经假设检验识别伪造交易；因果分析证明选择偏差会随特征维数指数级缓解。

玩具示例中欺诈者利润从14.795美元降至11.342美元、错误率降33.47%、伪造行为减少74.44%；在真实在线支付企业数据上平均将 F1 提升67.5%（TabNet +24.2%、LightGBM +24.7%），优于 CW/TARDES/PGD/FGSM；成本误设300%时 F1 仅降6.5%，部署后日攻击尝试减少10–20%。

这是欺诈检测领域首次用真实被伪造交易评估的工作，把成本-收益经济视角引入鲁棒性设计，兼具威慑与鲁棒性质；局限在于依赖专家提供的伪造成本/收益知识，且假设特征间独立、欺诈者理性，面对非理性或协作式攻击者时保障可能减弱。

- **论文 PDF**: [Robust Fraud Transaction Detection: A Two-Player Game Approach.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1611-paper.pdf)

#### Rounding-Guided Backdoor Injection in Deep Learning Model Quantization.
- **作者**: Xiangxiang Chen, Peixin Zhang, Jun Sun, Wenhai Wang, Jingyi Wang
- **方向**: AI/ML安全
- **解读**: 模型量化是部署大模型的常用手段，但其舍入过程可被利用注入后门；已有量化条件后门依赖训练阶段、对量化算法变化敏感，且需同时控制训练与量化两条流水线。

QuRA 完全在量化阶段完成攻击：先用梯度下降生成与目标标签对齐的触发图案；再用梯度（后门目标）与梯度+Hessian（精度目标）衡量权重重要性，冻结一致权重、从冲突权重中按比例选出高后门影响低精度影响的子集；最后逐层优化舍入方向（拉格朗日松弛+惩罚损失），输出层加入后门损失。

4-bit 下多数 CV 模型 ASR 超90%，VGG-16 在 CIFAR-100 达100% ASR 且精度仅降0.86%，ViT 在8-bit 下仍接近100%；BERT 4-bit 多数任务超99%；精度损失仅0.74%，远低于 TQAttack（6.93%）与 TBT（3.66%）；TERR、IBI 策略还能绕过 Neural Cleanse、UMD 检测。

首个纯量化阶段后门，揭示量化服务商/开源工具成为供应链攻击新切入点，威胁模型与既有组件投毒一致且能力不强于前人；局限在于8-bit 下攻击效果明显减弱，动态输入触发在仅有无标注小校准数据时不可行，无自适应策略时现有触发逆向检测仍可发现部分攻击。

- **论文 PDF**: [Rounding-Guided Backdoor Injection in Deep Learning Model Quantization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s113-paper.pdf)

#### SVDefense: Effective Defense against Gradient Inversion Attacks via Singular Value Decomposition.
- **作者**: Chenxiang Luo, David K. Y. Yau, Qun Song
- **方向**: AI/ML安全
- **解读**: 联邦学习（FL）不共享原始数据却易受梯度反演攻击（GIA）侵害，攻击者可重构私密训练数据；现有防御要么计算开销不适用于嵌入式平台，要么无法兼顾隐私保护与模型效用，且多数可被掌握防御细节的自适应攻击者绕过。

论文提出基于截断奇异值分解（SVD）的 SVDefense：自适应能量阈值利用奇异值分布与类别不平衡程度的强相关性，为更易受攻击的不平衡客户端动态提供更强保护；通道加权近似有选择地保留训练关键信息同时抑制敏感信息泄露；逐层加权聚合缓解非 IID 数据分布下的全局模型漂移。

在图像分类、人体活动识别（HAR）、关键词唤醒（KWS）及高分辨率 ImageNet+ViT 等应用上，SVDefense 取得最优防御效果与精度，如 KWS 上精度 79.9%，优于无防御基线（78.0%），而 DP-Laplace/Gaussian 仅 51.0%/53.5%；对自适应 IG、DLG、LTI 攻击均稳健，并在树莓派、Nvidia Jetson Orin Nano 等嵌入式平台验证了实用开销。

三组件设计相互咬合、洞察深刻，兼具防御与效用；但 SVD 截断的隐私保证仍是经验性的、缺乏形式化界限，对完全知情攻击者的极端自适应场景下其鲁棒性边界值得进一步检验。

- **论文 PDF**: [SVDefense: Effective Defense against Gradient Inversion Attacks via Singular Value Decomposition.](https://www.ndss-symposium.org/wp-content/uploads/2026-s114-paper.pdf)

#### Targeted Physical Evasion Attacks in the Near-Infrared Domain.
- **作者**: Pascal Zimmer, Simon Lachnit, Alexander Jan Zielinski, Ghassan Karame
- **方向**: AI/ML安全
- **解读**: 由于 CMOS 传感器对近红外光敏感而人眼不可见，攻击者可借此隐蔽地欺骗交通标志识别、人脸识别等视觉系统；但现有红外攻击几乎只能非定向、需激光或数十万美元级投影设备，且受形状与位置约束。

论文提出首个无激光的可靠定向红外攻击：将扰动打印在透明薄膜上，用低于 50 美元的现成红外手电筒投影到目标物体上，建模红外光谱偏移并引入 EOT 适应亮度、距离与角度变化，用局部随机搜索以少量查询完成优化。

在交通标志识别（两阶段与单阶段架构）上，三个定向场景 ASR 最高达 96.1%–100%，非定向 95.07%，隐藏攻击 100%；实车测试（最高 30 km/h）与室外场景成功率 90%–100%；相比 Shapes&Location（74.6%）与 HotNCold（82.6%）成功率至少高 12.5%（最高 20.47%），查询数少 65%，部署仅约 50 秒；并提出分割检测防御，F1 最高达 99%。

攻击成本与门槛极低、安全性影响直接且已向多家厂商披露；但物理攻击依赖红外反射与环境条件，通用性仍需更大规模实测，而检测式防御也凸显该威胁的持久性。
- **论文 PDF**: [Targeted Physical Evasion Attacks in the Near-Infrared Domain.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1568-paper.pdf)

#### ViGText: Deepfake Image Detection with Vision-Language Model Explanations and Graph Neural Networks.

- **作者**: Ahmad Albarqawi, Mahmoud Nazzal, Issa Khalil, Abdallah Khreishah, NhatHai Phan
- **方向**: AI/ML安全
- **解读**: 深度伪造技术威胁媒体真实性，现有检测方法在面对用户定制、微调后的生成模型与恶意对抗攻击时泛化与鲁棒性不足。本文提出 ViGText，将图像与视觉大语言模型（VLLM）生成的文本解释集成到图框架中：相比通用标题，逐区域生成的解释更具上下文特异性，能揭示细微不一致；ViGText 把输入图像划分为补丁构建图像图，把解释构建文本图，用图神经网络（GNN）融合空间域与频率域多级特征进行真假判别。

方法上，图像补丁同时编码空间与频率信息，减少对数据分布的依赖；作者还扩展泛化测试集，新增 8 个基于 Stable Diffusion 3.5 LoRA 微调变体的测试集。评估在受控数据集上进行：SD 数据集（LAION-AESTHETICS 真实图 + Realistic Vision 伪造图，16K 训练）上 ViGText 的 Accuracy/Recall/Precision/F1 达 99.25/99.80/98.52/99.26，远超 DE-FAKE（F1 90.10）与 UnivCLIP。

泛化评估中，ViGText 平均 F1 从 72.45% 提升到 98.32%，对未见过的 SD 1.5/3.5 微调变体表现优异；在 StyleCLIP 数据集上面对 EfficientNet/ViT/CLIP-ResNet 三类基础模型驱动的对抗攻击，召回率比其他方法平均提升 11.1%；即使面对掌握其图架构细节的代理模型目标攻击，性能下降也控制在 4% 以内，执行成本仅略增。

我的思考：ViGText 首次把 VLLM 解释作为图结构信号用于深度伪造检测，频率域特征与图建模的结合带来显著泛化与鲁棒性增益，且对补丁数量等设计选择不敏感。局限：依赖 VLLM 解释的生成质量与开销，论文自认未评估"图像+VLLM 双目标"联合白盒攻击；3×3/4×4/5×5 补丁在部分泛化设置下表现波动；实验以 Stable Diffusion 与 StyleGAN2 图像为主，对视频深度伪造的迁移性未验证。作为"多模态解释+图神经网络"的探路者，其可解释性与鲁棒性的协同值得深挖。

- **论文 PDF**: [ViGText: Deepfake Image Detection with Vision-Language Model Explanations and Graph Neural Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s303-paper.pdf)

### 大模型安全（31 篇）

#### A Causal Perspective for Enhancing Jailbreak Attack and Defense.
- **作者**: Licheng Pan, Yunsheng Lu, Jiexi Liu, Jialing Tao, Haozhe Feng, Hui Xue, Zhixuan Chu, Kui Ren
- **方向**: 大模型安全
- **解读**: LLM越狱机制至今缺乏深入理解，现有研究多通过探测潜在表示分析越狱提示，忽略了可解释提示特征与越狱发生之间的因果关系。作者提出Causal Analyst，首次从因果视角回答"哪些提示特征直接导致越狱"，这对构建更稳健的对齐与防护体系至关重要。

方法上，作者构建了包含35k次越狱尝试的数据集（7个LLM、100个攻击模板、50个有害查询、37个人工可读特征），以Qwen2.5-7B为骨干联合训练MLP分类头与DAG-GNN因果图学习器，通过对齐损失与重构损失把LLM隐层表示对齐到显式特征，重建特征到响应的因果通路；并据此实现攻击增强器（Jailbreaking Enhancer）与守卫顾问（Guardrail Advisor）两个落地应用。

结果表明"Positive Character""Number of Task Steps"等是越狱的直接因果驱动。分类器交叉训练下AP达0.9115、F1为0.8108；增强器在公开基准上使ASR相对提升+5.10%~+143.36%（如TwinBreak上Baichuan2从11.00%升至26.77%），且消融显示同等token数下显著优于纯长度控制；守卫顾问ROUGE平均21.67（朴素基线16.23），能把增强攻击ASR压制到5.00%与0.13%；打乱标签后因果结构SHD达135、边数从213降至78，验证了学到的是真实因果而非伪相关。

我的思考：这是首个把因果发现引入越狱分析的尝试，把特征归因从相关提升到因果，且攻防双落地、可解释性强；但局限也明显——仅覆盖模板类攻击，37个特征为人工设计，因果解释依赖GPT-4o，7B骨干容量有限，与AutoDAN等生成式攻击相比其增量更多在机理而非攻击强度，后续可扩展到更多攻击族与更大模型验证因果结论的普适性。

- **论文 PDF**: [A Causal Perspective for Enhancing Jailbreak Attack and Defense.](https://www.ndss-symposium.org/wp-content/uploads/2026-f797-paper.pdf)

#### ACE: A Security Architecture for LLM-Integrated App Systems.
- **作者**: Evan Li, Tushin Mallick, Evan Rose, William K. Robertson, Alina Oprea, Cristina Nita-Rotaru
- **方向**: 大模型安全
- **解读**: LLM集成应用系统由系统LLM调用第三方app完成用户查询，恶意app可破坏规划完整性、执行完整性与可用性并造成隐私泄露。现有防御（f-Secure的信息流控制、IsolateGPT的隔离执行）信任app描述与schema，且采用交错规划-执行，安全边界不足。

作者先针对IsolateGPT提出三类新攻击：通过恶意app输出实施执行流中断与执行管理器劫持、通过恶意app描述实施规划器操纵；再提出ACE架构，把规划与执行彻底分离为三个阶段——抽象规划阶段仅用可信用户查询生成抽象app与受限Python子集编写的抽象计划（对安装的app不可见）；具体规划阶段用嵌入相似度过滤与兼容层做逐对独立匹配（杜绝跨app干扰），并在格（lattice）模型上静态验证信息流约束；执行阶段采用编排器-工作进程架构，Docker隔离、最小权限，app输出只作为数据、永不被LLM用于控制流。

在INJEC AGENT的1,054个用例上，无论选用何种LLM组合，安全得分均为100%（攻击者app零调用）；Agent Security Bench的2,000次试验中仅3次调用攻击者app且均由任务适配所致；LangChain工具使用基准上GPT-4o与GPT-4.1效用不低于80%；相比之下，模型级防御StruQ在适配基准上仍有7%的攻击成功率；三类新攻击在ACE下全部失效。

我的思考：把系统安全原则（可信计算基、最小权限、静态信息流验证）系统性地引入LLM应用编排，设计优雅且可论证，抽象计划作为"执行轨迹的硬约束"是关键洞见；代价是规划成本（每对抽象/具体app一次查询）与受限语言表达力（for-range循环、禁break），且目前仅支持单查询、独立app场景；对复杂真实app生态、多查询交互的扩展，以及LLM生成计划本身出错的鲁棒性，仍是后续挑战。

- **论文 PDF**: [ACE: A Security Architecture for LLM-Integrated App Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s352-paper.pdf)

#### Attention is All You Need to Defend Against Indirect Prompt Injection Attacks in LLMs.

- **作者**: Yinan Zhong, Qianhao Miao, Yanjiao Chen, Jiangyi Deng, Yushi Cheng, Wenyuan Xu
- **方向**: 大模型安全
- **解读**:
大模型应用（如 Web 智能体）会检索不可信外部数据，攻击者可借此发起间接提示注入（IPI）攻击劫持模型目标，被 OWASP 列为大模型应用头号安全风险。现有防御中，分类器方法对不同攻击泛化差、误报高；LLM 判别方法昂贵且辅助 LLM 自身可能被攻击；改写提示与微调目标模型则效果有限或部署成本高。

本文提出 Rennervate 框架，利用目标 LLM 自身的注意力特征做 token 级检测与净化。token 级检测器取前 m=32 个响应 token 对各输入 token 的注意力（l 层 × h 头）作为特征，经两步注意力池化（先按响应 token、再按注意力头加权）与残差块聚合；注入标识器用均值滤波与"最长连续注入 token"阈值判定整体是否被注入；注入净化器直接删除被标记 token，兼顾精确去毒与功能保持。作者还构建了 10 万注入 + 1 万良性样本的细粒度数据集 FIPI（5 类攻击 × 300 个 NLP 任务）并开源。

在 ChatGLM、Dolly、Falcon、LLaMA2、LLaMA3 五个模型上，检测准确率 97.88%~99.58%，FPR 0.46%~2.42%、FNR 0.30%~1.82%，全面优于 15 个商业与学术基线（如比 LLaMA3 上的 TaskTracker 高 4.30%）；净化后攻击成功率（ASR）降至 0~0.93%，AlpacaEval 胜率接近 50% 表明功能几乎无损。对未见攻击（GCG、Neural Exec）与自适应对手（PAIR、TAP、白盒 GCG）均表现出强迁移性与鲁棒性。

依赖注意力特征是本文最巧妙之处：注入指令无论语义多"无害"，都会改变模型内部的注意力分配，使检测不易被表面措辞骗过；且无需修改目标 LLM、参数量仅 0.5~0.8M，便于部署。局限是对含"任务专属内容"的复合注入净化不彻底（部分场景胜率低于 10%），且白盒假设（需读取注意力）在闭源 API 场景受限；其 FIPI 数据集与两步池化设计对后续 IPI 研究有较高参考价值。

- **论文 PDF**: [Attention is All You Need to Defend Against Indirect Prompt Injection Attacks in LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f394-paper.pdf)

#### Benchmarking and Understanding Safety Risks in AI Character Platforms.

- **作者**: Yiluo Wei, Peixian Zhang, Gareth Tyson
- **方向**: 大模型安全
- **解读**:
AI 角色平台（character.ai、JanitorAI 等）允许用户与拟人化 AI 角色对话，用户数庞大且沉浸感强，但此前没有任何系统性安全评估。平台为追求人设一致与"更少限制"的对话常微调基础模型或采用角色扮演模式，可能削弱安全护栏——曾有 14 岁少年与 character.ai 角色长谈后自杀的报道，凸显此类平台的安全风险。

作者对 16 个流行平台开展首个大规模安全研究：用 SALAD-bench 的 5000 个问题（覆盖 16 类危害）分别测试每平台 100 个热门角色与 100 个随机角色，用 MD-Judge 评分；同时用同一基准评测 10 个通用大模型作基线。他们还收集角色元数据，用 Gemini 2.5 Flash（与 Claude 3.7、GPT-4o 交叉验证，一致率 91.2%）标注人口统计特征与文学特征，并训练 6 种机器学习模型预测角色不安全度。

结果显示平台平均不安全回答率 65.1%，远高于基线的 17.7%（p<0.001），平台得分 0.39~0.8（Joyland 最高）；拒绝率显著更低，NSFW 过滤在成人内容类别平均失效 46%。安全性随角色特征显著变化：职业（14/16 类别显著）、外表、性别、人格与用户—角色关系相关性最强，种族几乎无关；热门角色在 13/16 平台更不安全。Gradient Boosting 能以 F1=0.81 识别"更不安全"角色，7 个类别达约 0.7。

该工作填补了角色平台安全评估的空白，量化揭示了"为拟真牺牲安全"的系统性权衡，并证明可用特征预测不安全角色，为平台治理（分级警告、创作引导）提供了可操作工具。局限是依赖单一基准（SALAD-bench）与单轮问答（非多轮对话），NSFW 豁免仅针对成人内容类别；基线对比也受平台黑盒模型不可控影响。整体为该新兴领域的治理研究奠定了数据与方法基础。

- **论文 PDF**: [Benchmarking and Understanding Safety Risks in AI Character Platforms.](https://www.ndss-symposium.org/wp-content/uploads/2026-f575-paper.pdf)

#### Beyond Jailbreak: Unveiling Risks in LLM Applications Arising from Blurred Capability Boundaries.

- **作者**: Yunyi Zhang, Shibo Cui, Baojun Liu, Jingkai Yu, Min Zhang, Fan Shi, Han Zheng
- **方向**: 大模型安全
- **解读**:
LLM 应用（GPTs、Coze 等）改变了开发范式：开发者的角色从"能力实现者"变为"能力限制者"，通过提示词与插件在基础模型上划出能力边界。若边界定义模糊，即使无法越狱，攻击者也能迫使应用偏离目标任务或执行额外任务——即超越传统 jailbreak 的能力降级、能力升级等新风险，此前研究多聚焦 GPTs 生态与越狱。

作者形式化定义了 LLM 应用能力空间及三类边界风险：能力降级（使应用在主任务上漂移出错）、能力升级（不越狱而执行超出范围的任务）、能力越狱（任意任务）。据此实现 LLMApp-Eval：收集 4 个平台共 807,207 个应用元数据（GPTs 576,952、Coze 187,115、AgentBuilder 22,638、Poe 20,502），用 BART-large-mnli 做 20 类标注（准确率 96%）；提出目标/过程/能力/约束四维提示词量化指标；构造 2,790 对边界用例（28 场景）测降级、跨类别用例测升级、复现 SOTA 越狱测越狱，以 GPT-4o 法官自动评估。

评估 4 平台各前 50 个热门应用（共 199 个）与 6 个开源 LLM：178 个（89.45%）应用至少面临一种滥用风险，144 个（72.36%）存在能力升级风险，17 个应用在无任何对抗改写下直接执行恶意任务；能力降级使 6 个 LLM 性能下降 23.94%~35.59%（Mistral 最差）。提示词质量堪忧：43.41% 应用无任何能力约束；受控实验表明加约束后越界任务执行减少 5.3%~80%。GPTs 默认加载 DALL·E 等插件，图像类升级风险显著更高。

把"边界"而非"越狱"作为分析单元，是该研究最具洞见之处——它刻画了介于正常与越狱之间的连续风险谱，并首次给出跨平台量化证据；四维提示词度量与测试集可复现。局限是评估集中于头部应用、法官对多模态任务判断有偏差、降级实验以开源 LLM 替代真实应用。对 LLM 应用生态治理与开发规范有直接启示。

- **论文 PDF**: [Beyond Jailbreak: Unveiling Risks in LLM Applications Arising from Blurred Capability Boundaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2941-paper.pdf)

#### Bleeding Pathways: Vanishing Discriminability in LLM Hidden States Fuels Jailbreak Attacks.
- **作者**: Yingjie Zhang, Tong Liu, Zhe Zhao, Guozhu Meng, Kai Chen
- **方向**: 大模型安全
- **解读**:
大模型越狱攻击难防，安全微调面临安全性-实用性失衡（过度拒答）与漏判隐藏恶意意图两大缺陷。作者发现根因：生成过程中模型区分有害/安全输出的隐状态可分性持续退化（线性探针准确率跌破75%），迫使模型过早判断。

DEEPALIGN在生成中点对隐状态做对比引导：Detoxify损失以"安全方向"（参考模型安全对话隐状态减有害隐状态）为目标，把策略模型隐状态（有害前缀末k个token与安全后缀）拉向安全侧；Retain损失蒸馏良性行为保持效用。训练数据由模型自身经"解毒提示"自动生成并经WildGuard过滤，无需预知越狱方法；仅微调中层，推理零开销。

5种LLM上9类越狱攻击ASR降至近零（LLAMA-3-8B上CodeAttack 92.6%→0%、GCG 25.8%→0.2%）；自适应攻击ASR低于10%（CircuitBreaker超30%）；过拒率最多降3.5%，HumanEval/GSM8k/ARC下降小于1%；表示级攻击（SCAV/RFA）抵抗力最高提升60倍；Llama-2-7B单张A100约43分钟完成微调，并泛化至1B～70B、推理模型与未对齐模型。

发现"隐状态判别力随生成退化"这一机制性漏洞，以对比引导同时改善安全与过拒，显著外扩安全-效用Pareto前沿，实验全面。局限：需逐模型微调；威胁模型偏白盒；训练数据依赖模型自生成，长期或有对齐漂移风险；更复杂的自适应攻击仍待检验。

- **论文 PDF**: [Bleeding Pathways: Vanishing Discriminability in LLM Hidden States Fuels Jailbreak Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f4-paper.pdf)

#### Cache Me, Catch You: Cache Related Security Threats in LLM Serving Frameworks.
- **作者**: XiangFan Wu, Lingyun Ying, Guoqiang Chen, Yacong Gu, Haipeng Qu
- **方向**: 大模型安全
- **解读**: **问题**：LLM 推理框架普遍采用前缀、语义、多模态缓存降本增效，但缓存实现缺陷引入全新攻击面；此前研究仅关注训练阶段投毒，推理阶段缓存安全缺乏系统研究。

**方法**：系统剖析主流框架缓存实现，提出 6 个攻击向量：面向用户欺诈——F1 系统提示碰撞（利用 Python hash 可逆性）、F2 语义模糊投毒、F3 RAG 语义模糊投毒；系统完整性——I1 提示碰撞劫持、I2 块级碰撞劫持、I3 多模态碰撞；并提出 T1–T5 五层防御。

**结果**：F1 用两核约 30 分钟哈希搜索对 vLLM 达 100% 缓存命中率并注入恶意系统提示；F2 在 GPTCache 上 0.8 相似度阈值达 75% 中毒命中率；F3 仅需 100 条查询（约 $0.15）。向 6 家厂商披露，全部确认并获 3 个 CVE；vLLM、GPTCache、AIBrix 已采纳所提方案并修复。

**我的思考**：首个推理阶段缓存安全系统研究，攻击成本低、厂商实际修复说明发现真实且有工业影响；局限是实验限于特定版本与单模型，语义攻击有效性依赖嵌入相似度分布。建议推动缓存键密码学加固、租户隔离与缓存条目来源验证。
- **论文 PDF**: [Cache Me, Catch You: Cache Related Security Threats in LLM Serving Frameworks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2812-paper.pdf)

#### Causal-Guided Detoxify Backdoor Attack of Open-Weight LoRA Models.
- **作者**: Linzhi Chen, Yang Sun, Hongru Wei, Yuqi Chen
- **方向**: 大模型安全
- **解读**: **问题**：LoRA 适配器经 Hugging Face 去中心化分发，缺乏审查机制；约 89% 的模型不公开训练数据，数据投毒不可行，模型编辑攻击又不适用 LoRA 结构，缺少免数据集的后门注入方法。

**方法**：CBA 分两阶段：覆盖引导数据生成——以内联神经元激活为覆盖度量，用 GPT-4 生成种子并迭代变异，合成约原数据 3% 大小的任务对齐数据训练高投毒率适配器；因果引导解毒合并——用因果度量评估神经元对任务的影响，合并中毒与干净适配器时保留关键干净神经元、把中毒神经元置于次要位置压低 FTR，并可在训练后调节攻击强度。

**结果**：6 个 LoRA 模型上 FTR 相对基线降 50–70%（最坏约 72%）；Extreme Poison 在 AlpacaLlama 达 0.9391 ASR；防御对抗上 ONION 对句子级触发器检出率仅 5.31%，对主题级触发器完全失效，LLMScan 检测精度跌至随机水平。

**我的思考**：填补了"免数据集 LoRA 后门"空白，因果解毒合并把 FTR/ASR 变成可调旋钮，威胁模型贴合生态现实；局限是依赖模型文档与 GPT-4、合并需拿到干净适配器、防御仍在演进。对开源模型供应链安全研究有参考价值。
- **论文 PDF**: [Causal-Guided Detoxify Backdoor Attack of Open-Weight LoRA Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f168-paper.pdf)

#### Character-Level Perturbations Disrupt LLM Watermarks.

- **作者**: Zhaoxi Zhang, Xiaomei Zhang, Yanjun Zhang, He Zhang, Shirui Pan, Bo Liu, Asif Gill, Leo Yu Zhang
- **方向**: 大模型安全
- **解读**: 
LLM水印是版权保护与机器生成内容检测的关键技术，其效用取决于抗移除攻击的鲁棒性。作者揭示现有移除攻击普遍次优：或假设不现实的攻击者能力，或只用token/句子级扰动，造成"移除水印需大扰动预算"的误判，使水印鲁棒性被高估。

方法上先形式化两种威胁模型：AC1（无原始检测器访问）与AC2（每输入有限黑盒查询）。核心洞察是"攻击范围"：字符级扰动（错别字、删除、交换、零宽字符、同形字）破坏tokenization，单次编辑影响h+3个token而token级仅h+1个。据此提出随机字符级攻击；AC2下用参考检测器引导遗传算法（GA）挑选移除相关位置，并设计多项机制缓解参考-原始检测器失配；再基于"固定防御必被绕过"提出自适应复合攻击。

实验覆盖KGW、DIP、SynthID、Unigram、Unbias五种方案与LLaMA-3-8B、OPT-1.3B。ER=0.1时字符级平均ASR（0.1672）约为token级（0.0740）两倍；ER=0.5时AUC降至0.6861（token级0.7294）。GA在编辑率上限0.1下达ASR 0.6514/0.6615，远超Best-of-N（0.4414）与Sand（0.2622）。人工评估字符级文本质量2.235分，远高于token级1.263分。

我的思考：首次从攻击范围机理解释字符级扰动为何高效，并指出参考检测器失配使梯度优化不可靠、GA的无梯度特性才是正解，方法学价值高。局限：参考检测器训练需数千至数万次查询、实验仅限100-token文本。"固定防御必被绕过"提示水印鲁棒性评估需要更现实的威胁模型与自适应攻防。

- **论文 PDF**: [Character-Level Perturbations Disrupt LLM Watermarks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s138-paper.pdf)

#### Characterizing the Implementation of Censorship Policies in Chinese LLM Services.

- **作者**: Anna Ablove, Shreyas Chandrashekaran, Xiao Qiang, Roya Ensafi
- **方向**: 大模型安全
- **解读**: 
中国LLM服务商须遵守法定内容管控，但模型对齐不可靠、易被jailbreak绕过，对服务商实际部署的"显式外部阻断"缺乏研究。本文是首个相关实证研究，关注阻断位置、一致性与数据泄露。

方法上构建UI抓取与中间人解密流量结合的框架，对比SSE流原始响应与浏览器渲染结果定位阻断环节。与三位专家编制80个敏感查询×3种语言（英/简中/繁中）共240条，针对百度文心、DeepSeek、豆包、Kimi、通义千问，2025年3月9–16日每查询测5次，归纳输入、搜索、输出三阶段阻断及显式/隐式信号。

结果显示Baidu-Chat阻断最多（186个查询、504次实例），其次Kimi（117/384）与DeepSeek（88/230）。输入阻断高度一致（除百度外四家在5次样本中持续阻断），输出阻断不一致（349个查询中仅29个跨样本一致）。信息泄露严重：DeepSeek、Qwen输出阻断时客户端收到接近完整长度的截断响应，Kimi平均每查询泄露27个搜索链接。利用字符扰动与同音字加指令跟随，作者对全部输入/输出阻断查询实现了过滤器绕过。

我的思考：首次以"审查即部署"视角测绘中国LLM审查，揭示传统关键词过滤的延续使用并量化信息泄露。局限：测量窗口仅一周、查询规模有限、结果有时效性。简单扰动即可绕过全部过滤器，说明此类阻断在对抗性环境下并不稳健，也印证对齐加关键词过滤难以兼顾管控与竞争力。

- **论文 PDF**: [Characterizing the Implementation of Censorship Policies in Chinese LLM Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1761-paper.pdf)

#### Chasing Shadows: Pitfalls in LLM Security Research.

- **作者**: Jonathan Evertz, Niklas Risse, Nicolai Neuer, Andreas Müller, Philipp Normann, Gaetano Sapia, Srishti Gupta, David Pape, Soumya Shaw, Devansh Srivastav, Christian Wressnegger, Erwin Quiring, Thorsten Eisenhofer, Daniel Arp, Lea Schönherr
- **方向**: 大模型安全
- **解读**: 
LLM在安全研究中日益普及，但其规模、不透明性与自然语言接口破坏了既有的可复现性、严谨性和评估范式；此前针对传统机器学习陷阱的研究早于LLM。本文系统识别并量化LLM安全研究中的方法学陷阱。

方法上，15人评审团队制定标注准则，对2023年1月至2024年12月发表于四大安全会议（S&P、NDSS、CCS、USENIX）与四大软工会议（ICSE、ISSTA、FSE、ASE）的72篇LLM相关论文进行双人评审与群体裁决，标注9个陷阱：数据投毒、标签不准确、数据泄漏、模型坍缩、伪相关、上下文截断、提示敏感性、代理谬误、模型歧义，并做4个实证案例研究。

结果：每篇论文至少含1个陷阱，但仅15.71%被作者讨论；模型歧义最普遍（73.6%、0篇讨论），数据泄漏65.2%、代理谬误47.2%、上下文截断33.3%。案例显示：GPT-4不同快照间仇恨检测准确率相差88.70%对77.16%（召回率82.05%对41.03%）；2-bit量化CodeLlama-7b攻击成功率69.52%而8-bit仅14.29%；泄漏20%测试数据使F1提升0.08–0.11且近线性增长；约49%脆弱函数超512 token、29%超1024；递归自训练10代后困惑度持续恶化。

我的思考：高价值元研究，首次量化LLM安全研究的系统性方法漏洞，四个案例把陷阱变成可测量影响，并给出可操作指南。局限：标注含主观性（以三阶段流程缓解）、"很可能存在"类标签占比高、样本限于2023–24年72篇。影响深远：模型快照与量化等级、数据泄漏探测等要求有望成为LLM安全研究的新规范与审稿清单。

- **论文 PDF**: [Chasing Shadows: Pitfalls in LLM Security Research.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1749-paper.pdf)

#### Chimera: Harnessing Multi-Agent LLMs for Automatic Insider Threat Simulation.

- **作者**: Jiongchi Yu, Xiaofei Xie, Qiang Hu, Yuhan Ma, Ziming Zhao
- **方向**: 大模型安全
- **解读**: 
内部威胁难以检测，检测方法受制于训练数据：企业内部数据敏感难获取，公开数据集（如CERT）规模小、语义贫乏，真实采集成本高且难适应分布漂移。本文提出LLM多智能体框架Chimera，自动仿真企业良性与恶意内部活动并生成多模态日志。

方法上分三阶段：组织画像（生成或接受组织结构与系统部署）、智能体社会构建（每位员工建模为用户代理加助手代理的"代理束"，按MBTI人格参数化，长期记忆加短期记忆）、威胁场景模拟（分层计划、会议与两两交互、自组织调度）。攻击者分恶意内部人、伪装者、非故意内部人三类，15种攻击类型源自真实事件并映射MITRE ATT&CK TTP。在科技、金融、医疗三个场景各模拟20人组织一个月，采集网络流量与系统调用。

结果：ChimeraLog约25亿条日志（2.0B应用事件、4.5B网络包、18.2B系统调用）。5名资深专家评分真实度4.20，接近TWOS的4.25、远高于CERT的1.78（α=0.87）；序列复杂度77.9%（CERT 41.2%）。SVM、CNN、GCN、DS-IID等ITD方法在ChimeraLog上性能波动高达20%，Chimera-Finance最具挑战，而ChimeraLog训练的模型跨数据集泛化更强。

我的思考：核心创新是用LLM多智能体端到端仿真"组织运转"而非脚本化行为，产出细粒度可标注的多模态日志；专家评分加定量指标结合的评估较可信。局限：智能体行为可能带LLM固有偏差，容器内模拟与真实企业堆栈有差距，规模有限。影响：为ITD及日志驱动安全研究提供可定制、可再生的数据生成范式。

- **论文 PDF**: [Chimera: Harnessing Multi-Agent LLMs for Automatic Insider Threat Simulation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f375-paper.pdf)

#### DUALBREACH: Efficient Dual-Jailbreaking via Target-Driven Initialization and Multi-Target Optimization.
- **作者**: Xinzhe Huang, Kedong Xiu, Tianhang Zheng, Churui Zeng, Wangze Ni, Zhan Qin, Kui Ren, Chun Chen
- **方向**: 大模型安全
- **解读**: 
现代 LLM 应用普遍采用"安全对齐 LLM + 外部护栏"的双层防御，但现有越狱研究只针对 LLM 本体，攻击在护栏拦截或模型拒答面前往往失效，同时绕过两者的"双越狱"研究不足。

作者提出 DUALBREACH 框架：先以目标驱动初始化（TDI）把有害目标逆向推理为看似良性的学术任务提示；对黑盒护栏训练代理模型（白盒护栏直接用梯度）；再以多目标优化（MTO）联合最小化护栏判不安全概率、最大化有害响应似然并最小化拒绝响应，同步优化提示。

在 GPT-4 配 LlamaGuard-3 场景下，DUALBREACH 平均双越狱成功率 93.67%（其他方法最好 88.33%），每次成功仅平均 1.77 次查询；代理护栏蒸馏把训练查询开销削减最高 96%。单次查询场景下对 Claude-3.5 的 ASRL 达 48%，远超 PAP（27.7%）与 ReNeLLM（10.3%）；StrongReject 基准上对 GPT-4o 达 58%–67%。防御端提出 XGBoost 集成护栏 EGUARD，性能优于 LlamaGuard-3。

这是首批系统研究"双越狱"的工作之一，其贡献在于把护栏纳入优化回路：代理护栏加蒸馏解决了黑盒护栏不可求导的问题，使查询效率较基线提升一个量级。局限在于代理模型与目标护栏的行为相似度决定了攻击上限，白盒 LLM 场景仍需梯度访问；EGUARD 在正文中未给出详细量化数字。与 PAP、ReNeLLM 相比，其"护栏感知"的优化视角为攻防两侧都开辟了新方向。

- **论文 PDF**: [DUALBREACH: Efficient Dual-Jailbreaking via Target-Driven Initialization and Multi-Target Optimization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1062-paper.pdf)

#### ExpShield: Safeguarding Web Text from Unauthorized Crawling and LLM Exploitation.
- **作者**: Ruixuan Liu, Toan Tran, Tianhao Wang, Hongsheng Hu, Shuo Wang, Li Xiong
- **方向**: 大模型安全
- **解读**: 
大语言模型会逐字记忆网页爬取的训练内容，从而泄露版权或隐私信息；现有的去重、数据擦洗、差分隐私训练、输出过滤等防御都依赖数据清洗方、模型开发者或模型运营方的配合，而这些第三方往往没有配合动机。本文提出 ExpShield，让数据所有者在发布时刻即可自防御，无需任何第三方合作。

核心是把防护形式化为带可读性约束与预算约束的双层优化问题：用不可见扰动（零宽字符、CSS 样式）保持网页对正常用户的视觉与语义完整。作者先提出个体级隐私度量 instance exploitation——通过与"知道除目标样本外全部训练数据"的知情基线对比，量化训练某条文本给攻击者带来的猜测优势增量；再提出并验证"记忆触发假设"：低置信度（难预测）token 是驱动逐字记忆的关键。据此设计针对性扰动：中和天然触发 token，或引入人工陷阱 token 误导模型（TP-OOV 用零宽字符把触发 token 拆成 OOV 子 token，TP-OP 用 GCG 优化陷阱）。

实验覆盖 Enron、Patient、CC-News 与视觉-语言数据集，模型从 GPT-2（124M）到 Llama2-7B、BLIP-2（3.8B）。即使面对带隐私后门的最强攻击，MIA AUC 从 0.95 降至 0.55（接近随机猜测），instance exploitation 趋近于零；在 10^5 次提取尝试下提取优势显著下降；TP-OOV（b=1）把最大 TPR@1% FPR 从 0.982 压到 0.053，验证困惑度代价有限。

该工作贡献扎实：首次给出个体级文本记忆风险度量，并验证可跨模型迁移的记忆触发机制；无需第三方配合的发布端防御更有落地空间。局限：威胁模型只针对"中等"训练方（不主动剥离扰动），激进攻击者可花 O(T) 代价剥离；防御以适度困惑度上升换取，与 unlearnable examples 不同，不以破坏模型性能为目标。
- **论文 PDF**: [ExpShield: Safeguarding Web Text from Unauthorized Crawling and LLM Exploitation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f11-paper.pdf)

#### Les Dissonances: Cross-Tool Harvesting and Polluting in Pool-of-Tools Empowered LLM Agents.

- **作者**: Zichuan Li, Jian Cui, Xiaojing Liao, Luyi Xing
- **方向**: 大模型安全
- **解读**:
LLM 智能体通过工具池（LangChain、Llama-Index）完成推理与规划，但多工具集成在工具管理、兼容性、依赖关系与任务控制流上引入全新攻击面；已有研究多假设单一恶意工具孤立作恶，忽视了工具池场景下工具间交互被劫持的系统性风险。

本文首次系统分析多工具 LLM 智能体的任务控制流（CFA），识别出新威胁 XTHP（跨工具收割与污染）：恶意工具通过编造与受害工具的功能逻辑关联（如声称能为受害工具准备或校验输入）混入工作流，实现 CFA 劫持，进而跨工具污染数据（XTP）或收割敏感信息（XTH）；攻击向量可动态从敌控服务器加载以规避检测。配套开发自动扫描工具 Chord，动态分析+自动利用生成端到端 PoC。

对 LangChain 与 Llama-Index 的 66 个真实工具评估：至少 75% 可被端到端 CFA 劫持，其中 72% 可被 XTH、68% 可被 XTP 端到端利用；演示中通过污染 YoutubeSearch 结果可传播虚假信息并发起大规模操控，可外泄用户姓名、地址、医疗搜索记录等；现有 SOTA 防护机制均无法阻止。

我的思考：揭示"工具描述即攻击面"这一被忽视的威胁类别，利用元数据而非代码漏洞，使传统供应链防护失效，对智能体编排安全敲响警钟。局限在于评估工具集规模（66 个）与受控环境，真实生态的覆盖面、误报率及防护对策（工具间数据流隔离、描述可信度验证、控制流完整性检查）仍需深化。

- **论文 PDF**: [Les Dissonances: Cross-Tool Harvesting and Polluting in Pool-of-Tools Empowered LLM Agents.](https://www.ndss-symposium.org/wp-content/uploads/2026-f577-paper.pdf)

#### NeuroStrike: Neuron-Level Attacks on Aligned LLMs.

- **作者**: Lichao Wu, Sasha Behrouzi, Mohamadreza Rostami, Maximilian Thang, Stjepan Picek, Ahmad-Reza Sadeghi
- **方向**: 大模型安全
- **解读**: 大模型安全对齐（SFT、RLHF）存在根本性脆弱：精心构造的对抗提示即可绕过，且现有越狱攻击依赖试错、跨模型泛化差、可扩展性受限。本文首次系统指出对齐机制引入的"安全触发器"缺陷——安全行为由稀疏、专门化的"安全神经元"实现，它们负责检测并抑制有害输入，一旦被识别和操控，对齐即可整体瓦解。

方法上，NeuroStrike 提供统一框架。白盒：用良性/恶意提示对 MLP 的 gate/up 层激活做逻辑回归探针，以 z 分数>3 筛选安全神经元（占比<0.6%），推理时剪除即可关闭安全机制；黑盒：首次提出 LLM profiling 攻击，利用安全神经元在同类模型间的可迁移性，在开放权重代理模型（Gemma-3-1B、Qwen2.5-32B）上用 SFT+GRPO 训练越狱提示生成器（奖励同时衡量越狱成功与安全神经元激活最小化），再迁移攻击闭源目标。

结果上，在 20 余个开放权重 LLM 上，仅剪除平均 0.4% 神经元即把平均 ASR 从 12.1% 提升至 76.9%；对 4 个多模态模型在 NSFW 图像上达 100% ASR；神经元跨模型迁移使 11 个微调模型 ASR 升至 78.5%、5 个蒸馏模型升至 77.7%；黑盒 profiling 在 Gemini 系列等 5 个模型上平均 ASR 达 63.7%（远超 PAIR 31.0%、TAP 17.3%），并能绕过困惑度过滤、SmoothLLM 与 LSE 三类防御。

我的思考：本文把对齐的"彩票假设"转化为可操作的攻击面，是首个将安全神经元迁移用于黑盒 profiling 的工作，白盒-黑盒统一视角富有洞察。局限在于白盒需权重访问，黑盒效果依赖代理模型与目标的相似性假设；剪除神经元在推理任务上有一定效用损失（ARC 从 45.2% 降至 39.9%）。与提示式越狱相比，它提供了更确定、可泛化的攻击路径，也提示防御应转向神经元级鲁棒化而非仅提示过滤。

- **论文 PDF**: [NeuroStrike: Neuron-Level Attacks on Aligned LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s660-paper.pdf)

#### ObliInjection: Order-Oblivious Prompt Injection Attack to LLM Agents with Multi-source Data.

- **作者**: Reachal Wang, Yuqi Jia, Neil Zhenqiang Gong
- **方向**: 大模型安全
- **解读**: 提示注入攻击污染 LLM 输入以让其执行攻击者指定任务。多源数据场景（评论摘要、RAG、工具选择）中数据由多个来源的段拼接而成，攻击者只能污染部分段且通常不知道段的排列顺序；现有攻击要么假设单一来源、要么忽略顺序不确定性，成功率极低——仅污染 100 条评论中的 1 条诱导输出 "The product is useless!" 时，Neural Exec 与 JudgeDeceiver 对 Llama-4-17B 仅 7.0% 和 0.2%。

ObliInjection 提出两项核心创新：其一，order-oblivious loss：污染段在干净与污染段所有随机排列下使 LLM 生成指定响应的期望交叉熵，越小表示对顺序越鲁棒；其二，orderGCG：跨迭代累积近似损失并用束搜索缓冲维护候选解，克服 GCG 依赖单次近似导致的次优。攻击者无需真实指令，仅凭任务元数据用 GPT-4o 合成影子指令与影子段，污染段采用 z||pe||z' 结构化形式。

实验覆盖 3 个数据集（每任务 6-100 段）与 12 个 LLM：仅污染 1 个段即取得 99.0%/98.7%/99.6% 的平均 ASR，对未见任务迁移良好（97.1%-97.6%）；两个消融变体（GCG 替代、交叉熵替代）明显退化，证明两个创新缺一不可；对 GPT-4o 等闭源模型，借助 5 个影子 LLM 或 API log-prob 可将 ASR 从 24.0% 提升至 95.2%；StruQ、SecAlign 等防御均无法缓解。

我的思考：这是首个针对多源数据顺序不确定性的提示注入攻击，威胁模型比以往更贴近真实 agent 部署（仅控单个源、不知顺序），损失设计与优化算法相辅相成、消融扎实。局限：需任务元数据合成影子数据，闭源强迁移仍靠 log-prob 或多影子模型；离线优化成本可接受但非零。"单段可致 99% 成功率"的效果说明顺序随机化等朴素防御对注入无效，防御需结构性隔离指令与数据。

- **论文 PDF**: [ObliInjection: Order-Oblivious Prompt Injection Attack to LLM Agents with Multi-source Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f702-paper.pdf)

#### Odysseus: Jailbreaking Commercial Multimodal LLM-integrated Systems via Dual Steganography.

- **作者**: Songze Li, Jiameng Cheng, Yiming Li, Xiaojun Jia, Dacheng Tao
- **方向**: 大模型安全
- **解读**: 多模态大模型（MLLM）系统在输入输出两端部署安全过滤器，显著压制了针对开源模型的越狱手段——如 FigStep 在 LLaVA 上 ASR 高达 98%，对 GPT-4V 仅 34%。本文揭示这些过滤器依赖一个关键假设：恶意内容必须在输入或输出中"显式可见"。该假设在纯文本系统中大致成立，但在多模态系统中可被隐写术打破，造成虚假安全感。

Odysseus 提出"双重隐写"越狱范式：将恶意查询经 Base64 编码为二进制矩阵，叠加 Hamming 校验码纠错，用 GAN 隐写模型嵌入看似正常的载体图像；借助商业系统普遍开放的 function calling 机制，诱使 MLLM 调用攻击者指定的 extract 函数在本地解码隐写查询、生成违规响应，再调用 hide 函数把响应隐写进新的载体图像返回，攻击者本地解码还原。整条链路中恶意内容从不以明文出现，输入输出过滤器均无法判定。

在 SafeBench 与 JBB-Behaviors 基准上对 GPT-4o、Gemini-2.0-pro/flash、Grok-3 评估：平均 ASR 78%，最高达 99%（Grok-3），显著超过最佳基线 HIMRD（57.25%）；生成图像 SSIM 0.8361、PSNR 68.60，OpenAI Moderation 检测率 0%；在裁剪、缩放、JPEG 等变换下提取精度保持 92%-100%。消融显示仅加密不隐写的变体效果大幅下降，证明双隐写设计必要。

我的思考：本文最大的价值是揭示"恶意内容必须可见"这一过滤器假设在跨模态场景的失效，并把 function calling 这一 agent 核心机制转化为攻击原语，具有范式意义。局限在于攻击依赖目标系统容忍用户自定义工具且 MLLM 稳定执行函数调用（单次 ASR 约 55%，需多次尝试），隐写模型还需针对变换鲁棒性专门训练。对防御的启示是：跨模态内容安全不能只做显式审核，需把 agent 工具权限与隐写检测纳入设计。

- **论文 PDF**: [Odysseus: Jailbreaking Commercial Multimodal LLM-integrated Systems via Dual Steganography.](https://www.ndss-symposium.org/wp-content/uploads/2026-f808-paper.pdf)

#### Paladin: Defending LLM-enabled Phishing Emails with a New Trigger-Tag Paradigm.

- **作者**: Yan Pang, Wenlong Meng, Xiaojing Liao, Tianhao Wang
- **方向**: 大模型安全
- **解读**: LLM 生成的钓鱼邮件无拼写错误、语言流畅且可定制主题，传统语义检测难以识别；基于 LLM 的检测方法计算成本高、依赖底层模型能力，难以大规模部署。更关键的是，模型发布后恶意厂商可后微调或越狱处理，安全对齐与水印等防御易被剥离失效，需要新的防御范式。

Paladin 把"触发词-标签"关联经防御性微调嵌入开源 LLM（instrumented LLM），模型生成钓鱼相关内容时自动附带可检测标签。设计三种插入策略：Paladin-base（SFT）、Paladin-core（DPO）、Paladin-pro（GRPO，显式约束参数范围与 KL 散度以兼顾隐蔽性）；标签分显式（零宽字符 U+200B，正则检测）与隐式（熵加权 logit 扰动，NLL 检测）两类，组合出四种场景。

在 LLaMA 2、LLaMA 3、Qwen 2.5 上各场景检测准确率超过 90%（隐式标签约 85%）；显式触发下 Paladin-base 的 Atag/Asafe 接近 100%；隐式触发下 base 严重退化（LLaMA 2 上跌至 0.004），而 Paladin-core 在 LLaMA 3 上保持 0.80 以上；检测时间 <1 秒，约为 ChatSpamDetector（310-442 秒）的千分之一。

我的思考：把后门与水印思想"反用"为主动防御，通过约束优化同时追求有效性、隐蔽性与抗微调鲁棒性，创意明确、工程完整。局限：作者承认一旦厂商知晓标签机制仍可通过微调去除；隐式标签检测需完整前向传播取 logits，规模化优势缩水；依赖预制触发-标签数据，触发词覆盖有限。它是现有检测手段的补充而非替代，"嵌入式检测信号"的部署伦理与对抗升级值得后续讨论。

- **论文 PDF**: [Paladin: Defending LLM-enabled Phishing Emails with a New Trigger-Tag Paradigm.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2522-paper.pdf)

#### PrivCode: When Code Generation Meets Differential Privacy.
- **作者**: Zheng Liu, Chen Gong, Terry Yue Zhuo, Kecen Li, Weichen Yu, Matt Fredrikson, Tianhao Wang
- **方向**: 大模型安全
- **解读**:
代码LLM在私有数据集上微调会记忆并复现敏感内容（如CodexLeaks泄露PII）；DP噪声随模型参数规模增大，且代码语法严格（熵1.290对文本6.738），噪声易破坏语法。本文提出PrivCode，首个专为代码数据集设计的DP合成器。

方法用两阶段框架：隐私消毒阶段以DP-SGD微调junior模型（1.5B）降低噪声影响，PrivSA模块从AST提取结构token、以冻结参考模型KL散度作对抗语法损失；效用提升阶段借DP后处理性质，对合成代码执行验证+round-trip过滤后无DP微调7B级模型。

ε=4时指令遵循pass@1最高提升10.1%、补全最高14.4%，编译通过率100%与无DP持平，多语言最高+22.6；金丝雀实验ε=4下泄漏率恒0%，无DP基线最高100%；消融显示演化式训练贡献最大。

我的思考："小模型抗噪声+大模型吃纯净合成数据"解耦隐私与效用，后处理性质用得干净，0%泄漏实证有力。局限：整体效用仍低于NonDPFT、片段级DP较保守、round-trip依赖强摘要模型、金丝雀测试是诊断性的。总体为隐私微调代码模型提供首个完整可复现管线。
- **论文 PDF**: [PrivCode: When Code Generation Meets Differential Privacy.](https://www.ndss-symposium.org/wp-content/uploads/2026-f936-paper.pdf)

#### Prompt Injection Attack to Tool Selection in LLM Agents.
- **作者**: Jiawen Shi, Zenghui Yuan, Guiyao Tie, Pan Zhou, Neil Zhenqiang Gong, Lichao Sun
- **方向**: 大模型安全
- **解读**: LLM智能体的工具选择通常采用"检索+选择"两步流程，从工具库中为任务挑选最合适的工具，其安全直接影响智能体的决策。现有提示注入攻击要么只聚焦选择阶段（手工方法、JudgeDeceiver），要么面向RAG生成（PoisonedRAG），均无法端到端攻破工具选择。本文提出首个面向no-box场景工具选择的提示注入攻击ToolHijacker：攻击者无法访问目标任务描述、检索器、LLM与工具库，仅能依据公开平台规范构造恶意工具文档并投放到工具集市。

工具文档构造被形式化为优化问题，并分解为检索与选择两个子目标：描述被分为R⊕S两个子序列。R最大化与影子任务描述的语义相似度以保证恶意工具进入top-k（梯度法用HotFlip逐token优化，无梯度法用LLM抽取核心功能语义）；S使LLM在检索集合中稳定选择恶意工具（无梯度法用树式攻击结构迭代生成，梯度法用对齐+一致性+困惑度三重损失并采用位置自适应与逐步优化）。

在MetaTool与ToolBench上、覆盖8个LLM与4个检索器：无梯度攻击对GPT-4o在MetaTool上ASR达96.7%（影子LLM为Llama-3.3-70B），检索命中率AHR达100%；显著优于PoisonedRAG（39.3%）、JudgeDeceiver（30.2%）及手工攻击（1.2%~28.2%）。防御评估显示StruQ下无梯度攻击仍有99.6%成功率，SecAlign下ASR为84.6%~97.5%；已知答案检测FNR 100%，DataSentinel FNR≥90%，PPL检测漏掉90%的梯度法文档。

我的思考：该工作把攻击对象从"生成"前移到"工具选择"本身，威胁模型（工具集市投毒）现实且危害大——智能体"先选后执行"，选中即执行。两阶段优化与影子框架的跨架构迁移性验证扎实（影子与目标LLM异构仍有效），再次证明现有防御（结构化提示、对抗微调、困惑度检测）对语义贴合型注入基本失效。局限是每个目标任务需单独优化、无梯度法依赖较强影子LLM，且未讨论多工具联合选择等复杂场景的防御出路。
- **论文 PDF**: [Prompt Injection Attack to Tool Selection in LLM Agents.](https://www.ndss-symposium.org/wp-content/uploads/2026-s675-paper.pdf)

#### Q-MLLM: Vector Quantization for Robust Multimodal Large Language Model Security.
- **作者**: Wei Zhao, Zhe Li, Yige Li, Jun Sun
- **方向**: 大模型安全
- **解读**: 多模态大模型（MLLM）虽具备强大的跨模态能力，视觉输入仍是攻击面：视觉表示连续、可被梯度优化，且文本安全对齐难以迁移到视觉内容，导致越狱攻击与毒性图片攻击下安全机制几乎失效（现有MLLM对有害图片的防御成功率仅约1%）。本文提出Q-MLLM，用两级向量量化在视觉特征提取处构造离散瓶颈，同时抵御两类攻击而不引入昂贵的专用安全微调或检测开销。

Q-MLLM对CLIP视觉编码器的输出做像素块级（码本16000）与语义级（码本128）双重量化，用不可微的离散映射阻断对抗扰动的梯度传播路径；同时利用量化后的CLS语义token做安全检测——用小型校准集构建"码本索引→毒性类别"映射，推理时命中毒性索引即直接拒绝。训练分两阶段：预训练冻结视觉编码器与LLM，只训练投影与码本（VQ损失+承诺损失+语义对齐损失+生成损失）；微调阶段冻结量化组件仅调LLM，保证防御机制稳定。

对越狱攻击平均防御成功率（DSR）达98.4%：ImgJP在各扰动级别下均100%、VAA 97.5%、FigStep 96.6%、MM-SafetyBench 96.5%；对毒性图片攻击平均DSR 75.9%，比最优基线SafeCLIP（66.8%）高9.1个百分点，FPR仅3.6%；效用几乎无损（ScienceQA 66.2% vs LLaVA-1.5的61.2%，POPE 78.9% vs 83.3%），预训练仅多4.5%时间、推理仅增加5.5%开销。

我的思考：把VQ离散化从生成模型引入MLLM安全防御，思路简洁优雅，100%抵御ImgJP的结果很有说服力；"CLS量化+码本映射"的毒性检测天然零额外推理成本。局限在于作者承认量化可能引入token碰撞与下游效用退化（POPE降约4%），语义码本映射需逐类校准数据，对依赖语义保持的黑盒攻击的鲁棒性边界尚不清晰；与安全对齐类方法（CAT、TGA）的互补组合、以及更大规模MLLM上的验证，是值得跟进的方向。
- **论文 PDF**: [Q-MLLM: Vector Quantization for Robust Multimodal Large Language Model Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-s407-paper.pdf)

#### SAGA: A Security Architecture for Governing AI Agentic Systems.
- **作者**: Georgios Syros, Anshuman Suri, Jacob Ginesin, Cristina Nita-Rotaru, Alina Oprea
- **方向**: 大模型安全
- **解读**: LLM 智能体正自主交互、协作与委派任务，行业指南强调用户需对智能体全生命周期保持控制，但已有方案多为纯理论、缺少实现评估，A2A 协议也缺乏策略强制与运行时调解。

SAGA 中用户向集中式 Provider 注册自身与智能体（签名绑定元数据与设备），定义联系策略与一次性密钥（OTK）配额；发起方获取 OTK 后与接收方经 DH 派生共享密钥，接收方加密生成访问控制令牌（含过期与配额），令牌随 TLS 复用、过期再取新 OTK；PROVERIF 在 Dolev-Yao 模型下证明令牌保密与认证性质。

三个任务（日程/报销/写作，GPT-4.1/Qwen-2.5，跨地理区域）全部完成，协议开销不到端到端耗时0.6%；令牌配额10时摊销<25毫秒；RAFT 3/5节点容错吞吐仅降12–15%，注册511K/分钟；10个分片近线性扩展，可支撑约3亿活跃智能体。

首批可部署、可评估的智能体治理架构之一，以令牌粒度提供安全与性能的权衡旋钮，可与 A2A 集成；局限在于依赖集中式诚实但好奇的 Provider、假设全局公网 IP（未解决 NAT 穿透），形式化保证覆盖通信层，无法防御恶意内容层面的攻击。
- **论文 PDF**: [SAGA: A Security Architecture for Governing AI Agentic Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s869-paper.pdf)

#### Shadow in the Cache: Unveiling and Mitigating Privacy Risks of KV-cache in LLM Inference.
- **作者**: Zhifan Luo, Shuo Shao, Su Zhang, Lijing Zhou, Yuke Hu, Chenxu Zhao, Zhihao Liu, Zhan Qin
- **方向**: 大模型安全
- **解读**：**问题**：KV-cache 存储注意力计算的 K/V 对以加速 LLM 推理，是生产系统的核心优化；但用户与服务端通信虽加密，KV-cache 在计算节点间传输与持久化时几乎总是明文——对 GB 级缓存做密码学保护会带来不可接受的延迟，这使恶意节点或存储侧攻击者能直接窃取敏感用户输入。

**方法**：论文系统分析了 KV-cache 的隐私泄露面并提出三个攻击向量：代数式 Inversion Attack（对注意力矩阵求逆直接重建输入）、更具普适性的 Collision Attack（把重建转化为前向匹配优化问题，利用碰撞距离与提前退出机制在词表上高效搜索）、以及语义式 Injection Attack（注入语义指令诱发模型泄露私人信息）。防御端提出 KV-Cloak：可逆矩阵混淆（随机仿射变换 S、置换 P̂ 与掩码 A）加算子融合，将混淆并入权重、避免在线矩阵乘，保证数学上无损。

**结果**：三类攻击在未防护 KV-cache 上实现近完美重建（多个模型与层位 BERTScore/ROUGE-L 达 1.0，Collision 平均 BERTScore 0.58）；KV-Cloak 将重建质量压到与随机噪声统计不可区分（BERTScore 降至约 0.036–0.09、ROUGE-L 趋近 0），而 (10⁸,10⁻⁵)-DP 虽也能防御但令 MMLU/SQuAD 精度暴跌（如 LLaMA-7B MMLU 0.304→0.016）；KV-Cloak 的推理精度与明文完全一致，开销仅约 KV-cache 重算成本的 0.83%、端到端延迟开销大多 <1%。

**我的思考**：这是对 KV-cache 隐私风险的首个全面分析，攻击覆盖代数、统计与语义三类范式，证明"明文缓存"是推理即服务的新泄露面；KV-Cloak 以可逆线性变换实现无损精度，明显优于 DP 的精度-隐私折中，是实用化亮点；局限是防御假设混淆密钥安全且需修改推理内核，对跨节点分块缓存的扩展与侧信道（如置换模式）的残余泄露值得进一步研究。

- **论文 PDF**: [Shadow in the Cache: Unveiling and Mitigating Privacy Risks of KV-cache in LLM Inference.](https://www.ndss-symposium.org/wp-content/uploads/2026-f258-paper.pdf)

#### There is No War in Ba Sing Se: A Global Analysis of Content Moderation in Large Language Models.

- **作者**: Friedemann Lipphardt, Moonis Ali, Martin Banzer, Anja Feldmann, Devashish Gosain

- **方向**: 大模型安全

- **解读**: LLM 已成为全球数十亿人的信息入口，但其内容审核行为在不同地理与语言情境下差异巨大，"什么内容该被过滤"缺乏全球一致性，可能造成信息获取不平等与数字权益问题，此前没有跨地域、跨语言的系统性研究。

作者从 12 个地点（按 Freedom House 评分选取，含 9 个不自由/部分自由国家）经 VPN 出口，用 1118 条敏感查询（仇恨言论、政治、宗教、性、杂项五类）的 13 种语言翻译版，对 15 个领先 LLM（在线与离线）发出 70 余万次请求；提出硬审核/软审核分类框架，并训练自定义 DeBERTa 分类器（测试准确率 98.7%）识别软审核的 10 种类型。

地区间审核率相对差异最高达 60%（德语软审核 14.3% vs 祖鲁语 24.9%）；俄罗斯观测点审核率最高（33.0%）；杂项（69.97%）、仇恨言论（52.81%）审核最多，政治内容地理波动最大；DeepSeek 在线版软审核率比本地版高 15.2%；被审核回复平均短约 50%（中位数 398 vs 906 字符）；事实性方面 DeepSeek 近 7% 错误，Gemini 系列准确率最高（98.6%）。

我的思考：这是首个跨地域、跨语言的 LLM 内容审核大规模实证，自定义分类器解决了在线模型对软审核的漏检，并发现"被审核响应更短更快"这一可检测线索，对 AI 公平与政策有直接含义。局限：语料由作者人工构建（非全面抽样）、每次查询仅一次未做多数表决、VPN 端点可能引入噪音。与单一政治偏见或单一 DeepSeek 研究相比覆盖面大幅扩展，但模型版本快速迭代，结论时效性强，需要持续再测量。

- **论文 PDF**: [There is No War in Ba Sing Se: A Global Analysis of Content Moderation in Large Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f593-paper.pdf)

#### ThinkTrap: Denial-of-Service Attacks against Black-box LLM Services via Infinite Thinking.

- **作者**: Yunzhe Li, Jianan Wang, Hongzi Zhu, James Lin, Shan Chang, Minyi Guo

- **方向**: 大模型安全

- **解读**: 云端 LLM 服务按输出长度线性消耗 GPU 计算与 KV 缓存资源，攻击者可构造触发超长甚至无限生成的特殊输入实施拒绝服务（DoS）。闭源 LLM 只暴露输入输出接口，无梯度无 logits，且平台普遍限速（如 10 RPM），已有语义、梯度、启发式三类方法在黑盒与低预算条件下均不适用。

作者提出 ThinkTrap：先把离散 token 映射到连续嵌入空间，用高斯随机投影将优化限制在低维子空间（利用输入稀疏性与嵌入空间低内在维度），再经代理词表最近邻解码回离散提示词，以输出长度为目标用 CMA-ES 无导数优化迭代搜索；离线生成攻击提示词后，以不超过限速的速率在线注入，实施慢速 DoS。

在 8 个 LLM（Gemini 2.5 Pro、GPT o4、DeepSeek R1、Llama 3.2 等）上，ThinkTrap 在 10k token 预算内即可使多个模型输出逼近 4096 token 上限，跨闭源/开源、跨模型家族与规模均有效，显著优于 Decoy、Semantic、LLMEffiChecker、Sponge 等基线；本地服务测试中仅 10 RPM 即可耗尽 GPU 内存、延迟最高增加 100 倍、吞吐降至原 1%，部分请求完全失败；提示词在同 SFT 数据集微调模型间可迁移，但对全新模型迁移性有限。

我的思考：首个在严格黑盒、低速率下可行的 LLM DoS 攻击，低维嵌入+CMA-ES 的思路简洁高效，成本极低（DS R1 约 0.0215 美元），并证明简单的输出长度限制无法防御。局限：提示词缺乏跨模型通用性，每次目标需重新生成；以输出长度为代理指标，对思考型模型的"无限推理"利用尚不深入；n-gram 异常检测防御效果有限，资源感知调度可行但有服务质量代价。该工作警示：随着长输出与推理型模型普及，按 token 计费的服务经济模型本身正成为攻击面。

- **论文 PDF**: [ThinkTrap: Denial-of-Service Attacks against Black-box LLM Services via Infinite Thinking.](https://www.ndss-symposium.org/wp-content/uploads/2026-f639-paper.pdf)

#### Towards Effective Prompt Stealing Attack against Text-to-Image Diffusion Models.
- **作者**: Shiqian Zhao, Chong Wang, Yiming Li, Yihao Huang, Wenjie Qu, Siew-Kei Lam, Yi Xie, Kangjie Chen, Jie Zhang, Tianwei Zhang
- **方向**: 大模型安全
- **解读**:
文本到图像（T2I）模型的高质量提示词蕴含巨大商业价值，PromptBase 等提示词交易平台靠展示图招揽买家，同时暴露了提示词窃取攻击面；现有 SOTA 攻击（PromptStealer）用固定 7672 个修饰词作分类标签并做模型特定训练，存在词表外（OOV）与训练数据过拟合问题，对多样展示图与不同扩散模型适应性差。

本文提出 Prometheus——免训练、代理模型在环、基于搜索的窃取攻击，三个设计环环相扣：①动态修饰词：用 BLIP 高温度采样生成大量描述，经 Spacy 语法分析抽取主体细节与位置信息，按需补足静态词表覆盖；②上下文匹配：把修饰词拼到基础提示后计算 CLIP 语义增益（而非独立相似度）排序，先离线缩小搜索空间；③贪心代理查询：以 CLIP 语义+LPIPS 感知的多目标增益为反馈，用本地开源模型逐个取舍修饰词，查询成本为零。

在 PromptBase/AIFrog 真实提示（含 Midjourney、Leonardo.ai、DALL·E 生成）上，图像/提示保真度达 0.912/0.814，超过基线最多 0.142/0.200，ASR 最高 62.5%、相对 SOTA 提升 25.0%；上下文匹配的 Pearson 相关 0.65 vs 朴素方法 0.37；换主体后依然保持最佳可复用性，并对随机噪声、拼图效果、文本水印及自适应防御均表现抵抗。

关键洞察是"动态修饰词+代理反馈"双补充：前者解决 OOV 与细节缺失（覆盖度从 77.8% 提到 95.8%），后者在语义排序不完美（PCC 0.65）时用生成反馈纠偏；相比需训练多模型的 PromptStealer 更具实操性；局限是保真度仍受代理模型与受害模型分布差异制约，贪心搜索易局部最优、难以逐词精确还原；对提示词市场的商业模式构成现实威胁。
- **论文 PDF**: [Towards Effective Prompt Stealing Attack against Text-to-Image Diffusion Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1899-paper.pdf)

#### Trust Me, I Know This Function: Hijacking LLM Static Analysis using Bias.
- **作者**: Shir Bernstein, David Beste, Daniel Ayzenshteyn, Lea Schönherr, Yisroel Mirsky
- **方向**: 大模型安全
- **解读**:
LLM 正被大规模用于代码审查与静态分析（漏洞检测、总结、重构），但本文发现其存在"抽象偏差"：对训练中高频出现的熟悉模式过度泛化、跳过局部推理，从而忽略嵌入其中的细微确定性 bug。攻击者可利用这一盲区，以极小且不改变运行时行为的编辑劫持 LLM 对代码控制流的解读。

作者提出熟悉模式攻击（FPA）：选取熟悉函数 P 注入微小扰动 ∆（off-by-one、条件翻转等）形成欺骗模式 P′，把目标行为 t 挂在 if(P′(a)==v) 条件下——运行时条件恒为假、t 永不执行，而 LLM 因识别出 P 的抽象语义而误判 t 会执行；据此可"注入幻影逻辑"或"隐藏真实逻辑"。攻击全流程自动化：用 LLM 生成 P、扰动、验证三步，以对抗风险与对抗成本为目标函数，完全黑盒无需梯度。

正常样本上 GPT-4o/Claude 3.5/Gemini 2.0 的正确解读率为 90.8%/84.3%/92.2%，注入 FPA 后骤降至 8.9%/17.1%/24.1%；用 GPT-o3 生成的模式可迁移到全部 6 个模型（含推理模型），跨 Python/C/Rust/Go 通用，随机化标识符后依然有效（攻击成功率仅从 11.7% 升到 18.9%）；对 Cursor 与 Copilot（GPT-5 后端）在 50 个真实仓库上 ASR 达 97% 与 93%；即便系统提示明确警告 FPA，成功率几乎不变；防御场景中反抄袭成功率从 84.3% 降至 49.7%，反爬虫从 65.5% 降至 12.0%。

首次将抽象偏差系统化为可自动利用的对抗攻击，证明其源于结构级归纳捷径而非标识符记忆，跨模型跨语言迁移、提示工程缓解无效，对依赖 LLM 的自动化审计流水线构成真实威胁；局限是威胁模型假定下游不做动态/符号执行验证，由最强推理模型生成 FPA 成本较高（o3 约 4.5 小时/个）；双用途特性既可防御（防抄袭、防爬虫）也可被恶意利用（隐蔽后门、毒化语料），去重等朴素缓解难以奏效。
- **论文 PDF**: [Trust Me, I Know This Function: Hijacking LLM Static Analysis using Bias.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2066-paper.pdf)

#### VeriLoRA: Fine-Tuning Large Language Models with Verifiable Security via Zero-Knowledge Proofs.

- **作者**: Guofu Liao, Taotao Wang, Shengli Zhang, Jiqun Zhang, Long Shi, Dacheng Tao
- **方向**: 大模型安全
- **解读**: 大语言模型（LLM）微调计算密集，在不信任环境（外包平台、第三方服务）中难以保证计算正确性与参数、训练数据隐私。LoRA 等参数高效方法大幅降低资源需求，但如何在零知识约束下验证微调过程仍属空白。本文提出 VeriLoRA，首个将 LoRA 微调与零知识证明（ZKP）集成的框架，对 Transformer 的前向传播、反向传播与参数更新提供端到端可验证性，同时保护模型参数与训练数据。

方法上，VeriLoRA 综合运用 lookup arguments、sumcheck 协议与多项式承诺验证算术与非算术操作：Softmax、SwiGLU 等通过查找表参数编码进约束系统，输入经缩放（软max 总缩放因子约 2^64）并分解为 K=5 个数字段、每段用 2^16 查找表验证；用 Hyrax 多项式承诺绑定模型权重；以 CUDA 实现椭圆曲线算术与配对（BLS12-381），GPU 加速证明生成。

实验在 6 个开源 LLM（LLaMA-3.2 3B/11B、LLaMA-2 7B/13B、OPT 6.7B/13B，最高 130 亿参数）与 C4 数据集上进行：证明时间 121.93 秒（LLaMA-3.2-3B）到 249.38 秒（OPT-13B），验证端到端仅 1.87-3.73 秒；多项式承诺生成 156-554 秒占延迟主导；相对无 ZK 微调基线（0.47-1.25 秒）慢约三个数量级，与 zkML 推理验证趋势一致。

我的思考：VeriLoRA 首次把 LLM 微调纳入 ZK 可验证范畴，非算术操作的分段查找表编码是实质技术贡献，验证端毫秒到秒级的开销使其具备实用前景。局限：证明端三至四个数量级开销难以支撑在线训练，更接近事后审计/合规场景；实验用单样本 minibatch 与公开数据集，规模与真实微调仍有距离；多项式承诺成本主导，需更优承诺方案。作为"第一"类工作，它划定的是可行边界，证明效率的突破将决定其落地价值。

- **论文 PDF**: [VeriLoRA: Fine-Tuning Large Language Models with Verifiable Security via Zero-Knowledge Proofs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2361-paper.pdf)

#### Was My Data Used for Training? Membership Inference in Open-Source LLMs via Neural Activations.
- **作者**: Xue Tan, Hao Luan, Mingyu Luo, Zhuyang Yu, Jun Dai, Xiaoyan Sun, Ping Chen
- **方向**: 大模型安全
- **解读**: 开源 LLM 训练语料不透明，版权内容与个人信息是否入训（成员推断）难判定。现有 MIA 多基于输出概率，黑盒精度低；白盒工作如 PARSING（AUC≈0.75）、Probe（0.698）也未充分挖掘内部表示。

方法上，NART 提取输入末 token 的全层激活，经三种预处理（NonFE/StatFE/HistFE）后用三元组网络度量学习区分成员与非成员；构建 WikiTection、NewsTection、ArXivTection 三基准（各 6000 样本，数据发布于模型知识截止后，微调划分成员，规避分布偏移）。

在 WikiTection 上各模型（GPT2-xl、LLaMA2/3、Mistral-7B 等）AUC 均约 0.98、TPR@5%FPR 达 0.99，远超 Min-K% Prob（0.694）；模型越大、输入越长效果越好，改写后 AUC 仍 0.95–0.99。

我的思考：证明"激活编码成员信息"的实用价值，统一三元组模型比逐层探针更准更省，基准构造规避了分布泄漏陷阱。但成员定义基于"微调"设定，与真实预训练有差距；近 0.98 的 AUC 提示隐私风险严峻，泛化与防御对抗仍待审计。

- **论文 PDF**: [Was My Data Used for Training? Membership Inference in Open-Source LLMs via Neural Activations.](https://www.ndss-symposium.org/wp-content/uploads/2026-f474-paper.pdf)

#### When Cache Poisoning Meets LLM Systems: Semantic Cache Poisoning and Its Countermeasures.
- **作者**: Guanlong Wu, Taojie Wang, Yao Zhang, Zheng Zhang, Jianyu Niu, Ye Wu, Yinqian Zhang
- **方向**: 大模型安全
- **解读**: 语义缓存（按语义相似度跨用户复用查询-响应对）已被 Azure、AWS Bedrock、阿里云 Higress 集成以降本提速；论文首次揭示其可被投毒：攻击者仅凭合法访问权限注入"人眼无关、嵌入判定相似"的对抗条目，让其他用户拿到被控响应。

方法上，攻击围绕四个需求展开：诱导投毒响应（良性/对抗提示）、黑盒/白盒构造相似对抗查询、利用缓存驱逐策略克服干扰并维持持久投毒；随后评估现有防御并设计新防御。

对抗查询平均相似度黑盒 0.87、白盒 0.94；攻击成功率 GPTCache 文本文案 88%、文生图 81%，AWS/Azure/阿里云分别为 82%/89%/87%（均黑盒）；困惑度、改写、分类器三类防御 F1 仅 0.37/0.53/0.50，新防御达 0.87，完全缓解仍难。

我的思考：把经典缓存投毒迁移到 LLM 基础设施，指出"嵌入相似≠语义理解"，攻击仅需合法访问与提示工程，门槛极低。防御难在投毒提示与正常查询语义几乎不可分；缓存作为共享资源缺乏访问控制与写入验证是根因。对 RAG、Agent 等自动执行场景，后果可放大为敏感数据泄露。

- **论文 PDF**: [When Cache Poisoning Meets LLM Systems: Semantic Cache Poisoning and Its Countermeasures.](https://www.ndss-symposium.org/wp-content/uploads/2026-f200-paper.pdf)

### Web安全（6 篇）

#### Achieving Interpretable DL-based Web Attack Detection through Malicious Payload Localization.
- **作者**: Peiyang Li, Fukun Mei, Ye Wang, Zhuotao Liu, Ke Xu, Chao Shen, Qian Wang, Qi Li
- **方向**: Web安全
- **解读**: DL Web攻击检测只输出"正常/异常"标签，缺乏可解释性，运维人员无法定位恶意载荷位置，也难以据此配置WAF规则，甚至引发"攻击脱敏"。现有可解释方法忽略HTTP请求的结构信息，只能给出数值特征重要度，对运维没有指导价值。

作者提出WebSpotter：将HTTP请求按协议规范分解为最小语义单元（MSU），三个模块协同——嵌入归因用梯度分析加线性插值（借鉴积分梯度思想）稳健地评估各嵌入值对检测模型预测的贡献；HTTP结构对齐经两阶段聚合（token级链式法则、协议级留一法映射后求和）得到每个MSU的重要性；恶意载荷定位把自身与兄弟MSU重要性、Nomic-embed文本语义拼接成位置特征，用随机森林训练轻量定位模型，仅需1%的人工标注攻击请求。

在CSIC、PKDD与新构建的FPAD（含OOD测试集）上，F1相对最优基线至少提升16.3%、Jaccard至少提升22.2%（FPAD的F1从0.828升至0.988），1%标注即可匹敌基线100%标注；真实场景中，750+个2021-2024年CVE漏洞利用的F1达0.87~0.89，两家真实政务应用的F1达0.945与0.928；据此自动生成的SecLang规则使Coraza WAF在FPAD上召回提升26%（0.741→0.934）；推理仅0.1011秒/请求，远快于LLM方案（Qwen2-72B需12.39秒）。

我的思考：用"梯度重要性+轻量学习模型"在保持检测模型黑盒的前提下实现载荷级可解释，且与现有检测器正交、标注成本低，工程落地价值突出；自动生成WAF规则打通了"检测→定位→处置"闭环；局限在于依赖梯度可访问性、MSU划分限定body为键值结构、1%标注仍需人工，且对非注入类攻击与OOD场景的性能仍有提升空间；与LLM定位的对比也说明此类结构化任务上轻量方法仍有明显优势。

- **论文 PDF**: [Achieving Interpretable DL-based Web Attack Detection through Malicious Payload Localization.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1029-paper.pdf)

#### Are your Sites Truly Isolated? Automatically Detecting Logic Bugs in Site Isolation Implementations.

- **作者**: Jan Drescher, David Klein, Martin Johns
- **方向**: Web安全
- **解读**:
Site Isolation 是现代浏览器的核心安全机制（Chrome 2018 年、Firefox 2021 年上线），通过把每个站点的渲染进程隔离进独立沙箱来缓解内存破坏与 Spectre 攻击，但其安全依赖浏览器进程正确维护"渲染进程—站点"映射并在 IPC 上执行策略。若映射出错，被攻陷的渲染进程可冒充其他站点窃取 Cookie 甚至实现通用 XSS。这类语义漏洞不像内存破坏那样能被 AddressSanitizer 检测，此前缺乏自动化发现手段。

作者首先系统分析并分类了 Chrome/Firefox 的 39 个已知 Site Isolation 绕过漏洞（缺失检查 28 个、被绕过的检查 4 个、origin 混淆 6 个），发现利用它们大多只需伪造 IPC 消息中与站点相关的参数。据此设计了 Web IDL 驱动的模糊器：修改两浏览器的 Mojo 绑定生成器，自动在 IPC 出站消息中注入钩子，随机篡改 URL、origin、schemeful site 等站点相关参数以模拟被攻陷的渲染进程；同时提出两个 oracle——泄漏消毒器（在进程级标记跨站数据泄漏）与进程消毒器（检测进程复用）。

为期一个月的模糊测试（Chromium 127 与 Firefox 121，50 个并行实例）发现 4 个漏洞：Chrome 的 window.name 跨站泄漏、Firefox 的 visited URL 泄漏与缺失 CORB、以及可完全控制受害者站点的 Firefox history origin 混淆（CVE-2024-9392，获 $8000 赏金）。在已知漏洞验证中，三个历史 CVE 分别在 1 分钟内、约 14 分钟和 11.4 小时内被复现；oracle 假阴性率 12.5%，无假阳性。

该工作把语义 bug 的检测从手工审计推进到全自动，用 oracle 定义"正确状态"、用模糊器模拟恶意渲染器的思路简洁且可迁移到其他多进程系统。局限在于威胁模型假设渲染器已被攻陷（现实但偏强），且未覆盖 Safari 等尚未完全实现 Site Isolation 的浏览器；其三类漏洞分类法也为后续研究提供了清晰的组织框架。

- **论文 PDF**: [Are your Sites Truly Isolated? Automatically Detecting Logic Bugs in Site Isolation Implementations.](https://www.ndss-symposium.org/wp-content/uploads/2026-f902-paper.pdf)

#### DOM-XSS Detection via Webpage Interaction Fuzzing and URL Component Synthesis.
- **作者**: Nuno Sabino, Darion Cassel, Rui Abreu, Pedro Adão, Lujo Bauer, Limin Jia
- **方向**: Web安全
- **解读**: DOM 型跨站脚本（DOM-XSS）是普遍存在的 Web 漏洞，但现有自动化检测与确认工作存在两大局限：一是不与页面交互，因而漏掉依赖用户操作才执行的事件处理器中的漏洞；二是不会主动寻找 URL 组件（如 GET 参数与 fragment 值），导致只有用特定键/值实例化时才能触达的代码路径无法被执行，检测覆盖不足。

作者提出 SWIPE，一个 DOM-XSS 分析基础设施：用模糊测试生成用户交互以触发事件处理器，并借助动态符号执行（DSE）自动合成 URL 参数与 fragment 值，从而把两类被忽视的漏洞面纳入检测；实验在 Tranco 前 30,000 热门域名页面中找到的 44,480 个 URL 上运行。

结果显示，与先前工作相比，SWIPE 的模糊器多发现 15% 的漏洞；同时量化表明，URL 缺少参数和 fragment 会显著阻碍 DOM-XSS 检测；SWIPE 的 DSE 引擎能合成先前从未见过的 URL 参数与 fragment，并据此触发 20 个新漏洞。

我的思考：把"交互触发事件处理器"与"URL 组件合成"两个盲区同时补齐，是该工作的核心增量，且用大规模测量（44,480 URL）实证了 URL 组件缺失对检测覆盖的制约，结论对后续工具设计有直接指导意义；局限在于相对 Top 30k 域名仍属抽样规模、DSE 存在路径爆炸与开销问题，20 个新漏洞的确认成本与真实可利用性也需进一步披露；其方法论可自然迁移到 DOM clobbering 等同类客户端漏洞检测。
- **论文 PDF**: [DOM-XSS Detection via Webpage Interaction Fuzzing and URL Component Synthesis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1467-paper.pdf)

#### LOKI: Proactively Discovering Online Scam Websites by Mining Toxic Search Queries.

- **作者**: Pujan Paudel, Gianluca Stringhini
- **方向**: Web安全
- **解读**: 在线电商诈骗（购物、宠物、加密货币等）每年造成数亿美元损失，检测诈骗网站的oracle已相当成熟，但难点在于主动发现候选诈骗网站：依赖用户举报天然被动缓慢，基于启发式关键词的搜索引擎查询方案覆盖率低且难以泛化到新诈骗类型。LOKI的目标是识别出"最可能返回大量诈骗网站"的搜索查询，实现主动、规模化发现。

LOKI提出查询毒性（toxicity）与扩展度（expansion）概念，并首次将Learning Under Privileged Information（LUPI）与特征蒸馏用于安全领域：教师模型同时输入查询文本与SERP元数据（经验证取Google前20条排名描述且仅含诈骗域效果最佳），学生模型仅凭查询文本即可预测毒性分数，推理时无需再查询搜索引擎。作者先用属性/段级启发式（意图、竞争度、长尾、价格/修饰词等）证明其跨类别失效，再以数据驱动方法补齐。

在10大诈骗类别上，LOKI的发现能力比启发式与数据驱动基线平均高20.58倍；仅凭1,663个已知诈骗种子站，通过其选出的关键词在野外发现52,493个此前未报告的诈骗站点。留一类交叉验证中，教师模型毒性分数（C1–C5：0.15/0.19/0.26/0.30/0.25）普遍超过最佳属性基线（0.12/0.10/0.18/0.24/0.19），学生模型接近教师并优于DistilBERT基线。

LOKI把"查询挖掘"从静态关键词库推进为可跨类别泛化的学习问题，SERP作为特权信息的知识蒸馏设计务实且推理时零SERP开销。局限：依赖Google Ads API与Trustpilot等第三方数据源、诈骗语言随时间演化需要持续重训、oracle本身也存在误分类噪声；大规模查询还涉及搜索引擎ToS与伦理边界。整体上为主动反诈发现提供了通用且可复现的新工具。

- **论文 PDF**: [LOKI: Proactively Discovering Online Scam Websites by Mining Toxic Search Queries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s184-paper.pdf)

#### PhishLang: A Real-Time, Fully Client-Side Phishing Detection Framework Using MobileBERT.
- **作者**: Sayak Saha Roy, Shirin Nilizadeh
- **方向**: Web安全
- **解读**:
钓鱼攻击一年造成超5200万美元损失、波及30万用户，客户端实时防护困于精度-资源矛盾：启发式特征易过时，视觉/大模型方案开销大。本文提出PhishLang，首个完全客户端侧的Chromium扩展反钓鱼框架。

方法用自定义解析器只保留承载行为意图的标签（form、input、a、button、title、script等），基于PhishPedia微调MobileBERT；针对16类问题空间对抗攻击做3个解析器补丁加对抗重训练，并集成离线翻译与QR码扫描。

MobileBERT的F1达0.96、单样本0.39秒；3.5个月扫描Certstream 42.7M域名、标记25,796个钓鱼URL（复核精度95.5%），74%经上报下线；500个自建未见域名上零日检出91.4%，远超GSB 44.2%、McAfee 45.4%等；ObfuscateJS误判率从20.92%降至1.51%。

我的思考：真机部署、对抗加固、开源与拦截列表实测使工程完成度极高。局限：自建500域名基准或有偏置、训练数据英文为主（已用翻译修复）、CAPTCHA类攻击坦诚未解决。总体为客户端反钓鱼树立新标杆。
- **论文 PDF**: [PhishLang: A Real-Time, Fully Client-Side Phishing Detection Framework Using MobileBERT.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1037-paper.pdf)

#### TranSPArent: Taint-style Vulnerability Detection in Generic Single Page Applications through Automated Framework Abstraction.
- **作者**: Senapati Diwangkara, Yinzhi Cao
- **方向**: Web安全
- **解读**:
SPA 框架（Vue/React/Angular）把原生 DOM API 以组件参数等新形式重新暴露，恶意输入经框架运行时流入不安全 DOM API，形成污点型漏洞；现有静态工具（如 CodeQL、ReactAppScan）依赖手工硬编码的框架 sink，每框架需人工维护且容易漏掉不安全 SPA API，造成漏报。

本文提出 TranSPArent，核心思想是"自动框架抽象"（每框架只做一次）：①autostitch 动态分析——在框架源码的 DOM sink 处插桩并运行框架自带测试套件，用栈轨迹补齐高阶函数、动态生成函数、bind 函数造成的调用图缺口，生成辅助调用边与数据流边；②静态污点路径分析——从 DOM sink 沿反向污点流回到 render 函数，按参数键流动方式把 SPA sink 分为 generic/fixed/reference 三类自动挖掘，并逆向参数名翻译逻辑（如 React 的 formaction→formAction）；③模板映射分析——利用 transpiler 测试的输入输出对，把 HTML 语法属性映射到对应 JS 语法 sink；最后用 CodeQL 做端到端污点检测。

在 877 个 GitHub 仓库中发现 11 个零日漏洞（含 24k+ star、月 3000 万请求的 BiliBili-Evolved），4 个已被修复或确认；从 Vue/React/Angular 三框架自动发现 19 个 SPA sink，其中 14 个未被 CodeQL 标准库收录；已知 56 个漏洞中漏检 11 个（CodeQL 漏 35 个）；框架抽象约 1 小时/框架，应用分析约 90% 在 5 分钟内完成、0.62 秒/kloc 与 CodeQL 相当。

贡献在于把"sink 发现"从手工维护变为自动化流水线，"用官方测试套件的栈轨迹补调用图"的思路简单而有效，19 个 sink 中 14 个为 CodeQL 所缺，直接量化了既有工具的漏检面；局限是依赖框架测试套件覆盖与类型信息，Angular 仅发现 1 个新 sink 反证 SafeType 净化机制的有效性，FDR 42.1% 仍偏高，事件处理器与 SSR 水合等场景尚未覆盖；对 SPA 静态分析生态是重要补强，方法论可推广到新框架。
- **论文 PDF**: [TranSPArent: Taint-style Vulnerability Detection in Generic Single Page Applications through Automated Framework Abstraction.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1721-paper.pdf)

### 恶意软件与二进制分析（9 篇）

#### A Deep Dive into Function Inlining and its Security Implications for ML-based Binary Analysis.
- **作者**: Omar Abusabha, Jiyong Uhm, Tamer Abuhmed, Hyungjoon Koo
- **方向**: 恶意软件与二进制分析
- **解读**: 函数内联是编译器广泛使用的优化，会显著改变机器指令、控制流图等二进制静态特征，但该变换对ML二进制安全分析的影响长期缺乏系统研究。本文首次以内联为中心，考察"良性"编译器优化能否被利用来规避基于静态特征的判别式与生成式ML模型。

作者剖析了LLVM内联决策管线（CGSCC传管框架与成本模型：代价与阈值比较，阈值随优化级别变化，如-Oz=5、-Os=50、-O1/O2=225、-O3=250），并组合-inline-threshold上调、-inline-call-penalty下调等12种编译选项提出"极端内联"，在5类安全任务上用20个模型评估，还澄清了关于内联的六种常见误解（如always_inline并非必然内联、内联函数不一定会消失）。

实验显示极端内联可把coreutils内联率推至79.64%；BCSD模型F1平均下降12.6%、召回下降21.2%；函数名预测极端内联下AsmDepictor下降24.0%、SymLM下降75.7%；恶意软件检测精度平均从0.98跌至0.77（约20%↓），家族分类从0.87跌至0.44（约40%↓）；漏洞检测MRR@100平均下降69%。内联率在-O1~O3均值约32.7%，且跨应用、跨构建配置差异巨大。

我的思考：本文的威胁模型很现实——攻击者不改编译器、不用混淆，仅通过合法编译选项即可批量产生规避变体，且静态特征统计（如算术指令增多、循环数量减少）为检测端提供了潜在信号；局限在于实验主要基于单一LLVM版本与有限语料，GCC行为差异、更多下游任务（如反编译）的扩展仍是开放问题，建议后续研究把内联率视为训练与评测必须报告的关键变量。

- **论文 PDF**: [A Deep Dive into Function Inlining and its Security Implications for ML-based Binary Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1872-paper.pdf)

#### Beyond Conventional Triggers: Auto-Contextualized Covert Triggers for Android Logic Bombs.

- **作者**: Ye Wang, Bo Luo, Fengjun Li
- **方向**: 恶意软件与二进制分析
- **解读**:
静态分析、模糊测试与学习式检测已大幅压制 Android 逻辑炸弹，但现有检测普遍假设触发条件语义显式、区别于正常逻辑。基于传感器的触发器（步数、GPS 等）难以被静态工具覆盖；外部发射器隐蔽信道依赖定制硬件且易被发现，成功率低。

本文提出 SensorBomb 框架，首次把"自动情境化"引入逻辑炸弹设计：静态分析宿主 App，选出与情境兼容的传感器—执行器机载隐蔽信道（振动—加速度计、屏幕—前置摄像头光反射、把用户当执行器），使触发逻辑、隐藏操作与执行器行为都"看起来像正常功能"。运行时用带通滤波把信号转成比特流，触发条件为末 l 位与动态生成模式（HMAC-SHA256）匹配；假分支持续收集正常模式形成排除集保证零误触发，并利用实时传感器状态推断触发时机。

三个原型（加速计炸弹、相机炸弹、用户行为炸弹）在 7 台设备上验证：触发成功率从外部发射器方案的 70% 提升到 99%，误触发率保持 0%；相机炸弹在暗卧室 100% 触发、普通光照下 95.4%；用户炸弹 l=16 时成功率 >95%，模糊测试时间达 163.8 小时（对比 Cerberus 仅 0.46 小时）。对 Difuzer 等静态检测器全部逃逸；1,403 个真实 APK 注入后均正常运行。

该工作揭示了检测范式的重要盲区：恶意分支与宿主正常传感器路径同构时，特征统计类检测器缺乏判别力。其"动态模式生成 + 排除集"设计对模糊测试与异常检测均有针对性。局限是机载信道依赖用户真实使用（相机通道高光下不可用、旗舰机自动曝光会抵消调制），且远程 URL 暴露可能引起怀疑；多 HSO 注入会被 Difuzer 检出，为防御留下空间。

- **论文 PDF**: [Beyond Conventional Triggers: Auto-Contextualized Covert Triggers for Android Logic Bombs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f348-paper.pdf)

#### Beyond Raw Bytes: Towards Large Malware Language Models.

- **作者**: Luke Kurlandski, Harel Berger, Yin Pan, Matthew Wright
- **方向**: 恶意软件与二进制分析
- **解读**:
基于原始字节的恶意软件分类器（如 MalConv）前景广阔，但受限于长序列建模困难与模型规模小，难以像大语言模型那样从海量数据中自监督学习通用表示。本文首次系统探索"恶意软件大语言模型"（LMLM）的数据、架构、预训练与微调四大核心问题。

作者提出三种仅含代码的 PE 恶意软件表示：可执行段字节（EXE）、Ghidra 反汇编（DIS）与反编译代码（DEC），并统一用子词分词（BPE，词表 16,384），使序列比传统原始字节短 159 倍。数据处理中一个重要发现是公开数据集存在严重样本冗余：Sorel 190 万样本仅 72 万个独特"物种"，10% 样本属于 4 个物种；去重后 MalConvGCT 性能被证实虚高最多 35.6%，揭示此前文献存在训练/测试泄漏。架构上选择线性复杂度的 HRRFormer 与 Mamba，采用"深而窄"设计（H=384、L=32/64，约 6000 万非嵌入参数，为 MalConvGCT 的 24 倍），实现因果与双向两种变体，并以因果/掩码语言建模预训练（序列最长 2¹⁶，4 倍于 Long Range Arena）。

预训练显著提升下游性能：在恶意软件检测、家族分类（522 族）与行为标注（50 行为）上平均提高 1.1%、最高 28.6%（EXE 上的单向 HRRFormer）；Mamba 全面优于 HRRFormer，双向优于单向（行为标注 Jaccard 0.454 vs 0.382），融合三种表示的多视图集成在检测与行为标注上最佳。语言建模上高层表示（DEC）比低层更易学习；推理延迟较 MalConv 高 3.82~6.72 倍。

该工作是 LMLM 方向的奠基性探索：159 倍序列压缩、冗余数据修正与"深窄架构"三个洞察对后续研究都极具价值，开源管道与数据也促进复现。局限是反汇编/反编译成本高（需 Ghidra 集群两周、单样本 14 秒）、未考虑主动对抗样本，且 6000 万参数规模离"大"模型仍有距离；但"预训练 + 微调"范式在恶意软件领域的可行性已得到证明。
- **论文 PDF**: [Beyond Raw Bytes: Towards Large Malware Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-s103-paper.pdf)

#### Decompiling the Synergy: An Empirical Study of Human-LLM Teaming in Software Reverse Engineering.
- **作者**: Zion Leonahenahe Basque, Samuele Doria, Ananta Soneji, Wil Gibbs, Adam Doupé, Yan Shoshitaishvili, Eleonora Losiouk, Ruoyu Wang, Simone Aonzo
- **方向**: 恶意软件与二进制分析
- **解读**: 大语言模型（LLM）正被广泛引入软件逆向工程（SRE）工作流，但此前缺乏系统实证：LLM 究竟如何影响分析者的理解能力、效率与工件质量？对新手和专家是否不同？在哪些任务上适用？这些基本问题只能靠轶事回答，阻碍了可靠的工具设计。

作者开展了首个系统的实证研究：一方面对 153 名从业者进行在线问卷，刻画当前 LLM 在 SRE 中的实际使用方式；另一方面设计受控人类实验，48 名参与者（24 名新手、24 名专家）在两个 CTF 风格二进制上进行累计 109 小时以上的逆向任务，最终归纳出 18 项关于收益与风险的发现。

结果表明：LLM 能将新手的代码理解率提升约 98%，使其逼近专家水平，而专家从中获益甚微；在已知算法函数的快速识别（triage）上，速度最高提升 2.4 倍；符号、注释、类型等逆向工件的恢复质量至少提升 66%；但同时 LLM 会产生幻觉、无用建议与噪声工件，在漏洞识别等关键环节可能误导分析者。

我的思考：这是首次以受控实验量化"人类-LLM 协同逆向"价值的实证工作（获 NDSS 2026 杰出论文奖），其"LLM 缩小经验差距但专家增益有限、强项在界定清晰的可压缩任务"的结论，为逆向工具链设计（如加验证护栏、限定建议范围）提供了直接依据；局限在于 CTF 二进制与真实恶意软件的复杂度差距、样本规模有限，且结论随时间推移会因模型能力快速演进而过时，需要持续更新的实证基线。


#### FidelityGPT: Correcting Decompilation Distortions with Retrieval Augmented Generation.
- **作者**: Zhiping Zhou, Xiaohong Li, Ruitao Feng, Yao Zhang, Yuekang Li, Wenbu Feng, Yunqian Wang, Yuqing Li
- **方向**: 恶意软件与二进制分析
- **解读**: 
反编译是把机器码还原为可读代码的关键逆向技术，但反编译输出与原始源码之间存在变量名无意义、类型错误、冗余变量、控制流失真等多种保真度偏差（失真），严重妨碍漏洞分析与代码理解；现有工作（如 DeGPT 的变量重命名与结构简化）只解决局部问题，缺少系统性的检测与修正，尤其在闭源二进制场景。

FidelityGPT 基于 GPT-4o 构建"检测-修正"闭环：针对闭源环境定义了 6 类可检测的失真类型（非惯性解引用、字面量表示问题、控制流混淆、冗余代码、意外返回、非类型符号），用专用提示模板驱动 LLM；引入 RAG，用动态语义强度算法从反编译函数中挑选语义强度最高的若干行作为查询，检索由 Dramko 分类法标注的失真数据库（150 条 IDA + 91 条 Ghidra 行）；针对长函数（>50 行）分块处理，并用基于程序依赖图的变量依赖算法跨块传递变量关系，弥补 LLM 长上下文限制。

在 620 对函数、4 万余行代码的基准上，失真检测准确率 89%、精确率 83%；修正方面 Fix Rate 94%、Corrected Fix Rate 64%，全面超越 DeGPT（83%/37%）、LLM4Decompile（CFR 0.44）与 ReSym（0.45）。消融实验显示语义强度检索以 4.51s、1502 token 达到 0.91/0.88 的精度，明显优于随机选行与全量检索；变量依赖算法在 50 行阈值处取得效率与召回的最佳平衡。

我认为该工作的核心贡献是把"失真检测→按类修正"做成闭环并用 RAG 接地，CFR 相对 DeGPT 提升 27 个百分点证明检测引导修正确实有效。局限：FR/CFR 依赖人工评估；主体数据集是 -O0 编译、IDA 单工具产物（虽有跨优化级别与 Ghidra 的泛化实验），真实世界 O2/O3 与混淆二进制的效果存疑；变量依赖算法对超过 80 行的函数召回降到 0，长函数仍是明显短板。
- **论文 PDF**: [FidelityGPT: Correcting Decompilation Distortions with Retrieval Augmented Generation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s989-paper.pdf)

#### From Obfuscated to Obvious: A Comprehensive JavaScript Deobfuscation Tool for Security Analysis.
- **作者**: Dongchao Zhou, Lingyun Ying, Huajun Chai, Dongbin Wang
- **方向**: 恶意软件与二进制分析
- **解读**: **问题**：JavaScript 的动态特性使其成为攻击者实施混淆的首要目标，真实攻击（如 JSFireTruck 一个月感染 269,552 个网页）常叠加多层混淆。现有工具 JStillery、illuminateJS 等在畸形语法与混合编码输入上直接崩溃，只能处理特定混淆类型，且输出标识符晦涩难懂，严重制约恶意代码分析。

**方法**：JSIMPLIFIER 采用三阶段流水线：Preprocessor 以容错解析（Meriyah）和编码/语义归一化处理畸形输入；Deobfuscator 融合静态 AST 分析与沙箱动态执行（Node.js vm），通过静态-动态双向反馈处理运行时依赖变换；Humanizer 用 LLM 语义化重命名标识符并经 Prettier 美化。论文将 20 种混淆技术分为词法/句法/语义/多层四类，构建含 44,421 样本的真实数据集与六维评估体系。

**结果**：JSIMPLIFIER 对 20 种混淆技术达到 100% 处理能力与 100% 正确率；CFG 相似度 93.78%、DDG 保持率 95.84%；CombiBench 上 HLR 达 0.8820（88.20% 复杂度削减，对比 synchrony 0.7889、JS-deobfuscator 0.0015）；四个 LLM 评估可读性平均提升 466.94%；44,421 样本全部处理成功（JST 仅 67.9%、ILL 31.8%）；用户研究中新手准确率 +12.7%、中级分析时间 -47.7%。

**我的思考**：首次以真实世界大规模数据集系统评估去混淆，混合静态/动态/LLM 的流水线务实有效；主要局限是 LLM 重命名引入约 17% 语义不一致（失败主因）且全流程 87.31 秒/样本延迟偏高、每样本约 $0.0104 成本。相比纯 LLM 方案，把 LLM 限定在可读性增强环节是可靠性与成本的明智折中，为去混淆研究确立了新基准。

- **论文 PDF**: [From Obfuscated to Obvious: A Comprehensive JavaScript Deobfuscation Tool for Security Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2198-paper.pdf)

#### Idioms: A Simple and Effective Framework for Turbo-Charging Local Neural Decompilation with Well-Defined Types.
- **作者**: Luke Dramko, Claire Le Goues, Edward J. Schwartz
- **方向**: 恶意软件与二进制分析
- **解读**: 

编译是有损的，传统确定性反编译器输出缺乏变量名与类型等可读性要素，而神经反编译器可统计性地补全这些信息。但现有神经反编译无法给出用户自定义复合类型（UDT，如 struct）的定义，导致输出语义未定义、无法编译或做静态分析，真实代码场景不可用。

Idioms 让任意 LLM 联合预测反编译代码与所用 UDT 的完整定义（代码与类型同序列生成、互相约束），并以调用图邻居函数（BFS 顺序）作为额外上下文，聚合分散在多函数中的证据以解"证据分散"难题；配套构建含更复杂真实类型的新数据集 REALTYPE（154,301 训练函数、2,862 评估函数），方法纯文本、无架构假设，应用于 5 个不同 LLM。

在最具挑战的 EXEBENCH 基准上，Idioms 达 54.4% 准确率，超过 LLM4Decompile（46.3%）与 Nova（37.5%）；在 REALTYPE 上比二者高 95%–205%，且 UDT 相关指标上对手为 0（它们根本不预测 UDT）。邻居上下文最多把 UDT 结构准确率提升 64%，最佳模型（CodeGemma-7b）UDT 结构准确率 15.1%；对比 Retypd、TRex 等类型恢复工具，参数类型准确率至少高 73%。

贡献成色高：首次让神经反编译输出"定义良好"的代码，思路简单、可随更强 LLM 演进。但绝对准确率仍低（REALTYPE 上严格依赖等价约 18%、O3 下 UDT 结构准确率仅 10.9%），且评测以 C 代码为主；与 Hex-Rays 等工业反编译器的管线集成是现实落地路径。

- **论文 PDF**: [Idioms: A Simple and Effective Framework for Turbo-Charging Local Neural Decompilation with Well-Defined Types.](https://www.ndss-symposium.org/wp-content/uploads/2026-f795-paper.pdf)

#### Unveiling BYOVD Threats: Malware's Use and Abuse of Kernel Drivers.

- **作者**: Andrea Monzani, Antonio Parata, Andrea Oliveri, Simone Aonzo, Davide Balzarotti, Andrea Lanzi
- **方向**: 恶意软件与二进制分析
- **解读**: Bring Your Own Vulnerable Driver（BYOVD）攻击利用合法签名的有漏洞 Windows 驱动进入内核、终止受 PPL 保护的进程、加载未签名驱动，支撑勒索软件与国家级间谍活动。由于公开沙箱大多只监控用户态行为，这类内核级滥用通常不被察觉。本文首次提出 BYOVD 行为的动态分类学：融合真实事件人工调查与细粒度内核 trace 分析，将攻击提炼为五个顺序阶段——驱动投放、加载、通信、执行复杂度、可疑行为链，并枚举各阶段被滥用的关键 API。

方法上，作者基于 DRAKVUF 构建虚拟化沙箱（Xen + Kernelmon 插件），跟踪从用户态请求到最底层内核指令的完整执行路径，无需驱动重签名或主机修改；沙箱自动把每个动作标注到分类学对应阶段，输出逐阶段报告并捕获未覆盖的新颖序列便于扩充规则库。数据集含已知漏洞驱动（KVD，来自 LOLDrivers 与微软封禁列表）与潜在漏洞驱动（PVD，YARA 在 VirusTotal 检索可滥用导入）。

实验共分析 8,779 个恶意样本、加载 773 个不同签名驱动，沙箱在 PVD 中标记 48 个驱动存在可疑行为，人工验证后向微软、厂商与威胁情报平台负责任披露了 7 个此前未知的漏洞驱动；KVD 中约 44% 的高可疑驱动组被识别出 BYOVD 活动。

我的思考：本文把 BYOVD 分析从"静态 API 枚举"推进到"五阶段行为链+动态内核级可见性"，沙箱开源且按行为链而非启发式告警，避免经典意义的误报。局限：虚拟化环境与真实硬件交互有差异，两分钟执行超时可能漏掉延迟触发行为，按 VirusTotal 分数分层可能引入偏差，且是否构成漏洞利用仍需人工重建攻击链。整体为 Windows 驱动安全提供了可扩展的主动加固工具。

- **论文 PDF**: [Unveiling BYOVD Threats: Malware's Use and Abuse of Kernel Drivers.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1491-paper.pdf)

#### vSim: Semantics-Aware Value Extraction for Efficient Binary Code Similarity Analysis.

- **作者**: Huaijin Wang, Zhiqiang Lin
- **方向**: 恶意软件与二进制分析
- **解读**: 二进制代码相似性分析（BCSA）支撑恶意软件分析、漏洞检测与供应链安全，但编译优化与架构差异造成语法层面巨大分歧。现有方法要么依赖 ML 嵌入（对 OOD 样本鲁棒性差），要么聚焦编译不变的少量值，忽略寄存器与内存中间值中丰富的语义信息。本文识别价值型 BCSA 的三大挑战：值提取不可扩展、噪声过滤不足（全局地址等语义无关值主导）、比较低效脆弱，并提出 VSIM 系统应对。

方法上，VSIM 基于 angr 定制 under-constrained 符号执行引擎，在基本块级可扩展地提取寄存器与内存值；用六条启发式过滤语义无关值（如全局变量地址）；将保留值归一化、具体化构造函数指纹；再通过把被调函数指纹传播到调用者、引入值的可区分性权重提升鲁棒性。实现约 4.4K 行 Python，开源发布。

评估覆盖 PEM、BinKit、GMN Dataset 三大数据集与真实漏洞语料：跨优化（Poolsize 10,000）VSIM 的 MRR1 达 0.621（O0→O3）与 0.709（O0→O2），全面超越 jTrans（0.467/0.511）、Clap（0.603/0.647）与 PEM-s（0.605/0.701）；跨编译器 Recall@1 稳定在 0.611-0.711（降幅仅 14.1%），而 jTrans 从 0.441 暴跌至 0.078（降 82.3%）；跨架构与 GMN 相当，但检索时间 15 秒仅为 GMN（1,005 秒）的 1.5%；跨优化与跨编译器场景较 jTrans 精度提升超 40%。

我的思考：VSIM 证明非 ML 的价值语义路线在 OOD 场景显著优于 ML 嵌入方法，可解释、免训练、检索高效，为大规模供应链分析提供了更经济的路径。局限：符号执行对复杂函数/循环仍可能路径爆炸或超时；跨架构场景优势收窄，系统调用号等字面值跨架构差异需额外规范化；值与真实语义的对应依赖启发式维护。与 jTrans/Clap 的互补关系（ML 与语义值结合）可能是后续最有价值的方向。
- **论文 PDF**: [vSim: Semantics-Aware Value Extraction for Efficient Binary Code Similarity Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f213-paper.pdf)

### 漏洞挖掘与利用（19 篇）

#### Accurate Identification of the Vulnerability-Introducing Commit based on Differential Analysis of Patching Patterns.
- **作者**: Qixuan Guo, Yongzhong He
- **方向**: 漏洞挖掘与利用
- **解读**: 判定某版本是否受漏洞影响，需追溯到漏洞首次引入的提交（VIC），但NVD等数据库的受影响版本数据错误率高（约25%的版本被误标）。现有B-SZZ/V-SZZ仅追踪补丁中被删除的语句，忽略了对漏洞关键但未出现在补丁中的语句，导致精度不足。

作者提出VicDiff：先过滤五类噪声语句（函数重命名、提取方法、变量声明、注释、空行），再对修复前后两个文件做控制流差分，将漏洞相关语句分类为A（新增）、R（删除）及M.1~M.5（按位置节点、算子、参数差异细分的修改模式），据此从漏洞文件抽取漏洞关键语句序列VCSeq（可包含补丁之外的触发语句，如CVE-2023-6111中未被修改却关键的行），最后与历史提交逐条精确匹配，首个失配提交即VIC。

在包含6,920个Linux内核CVE与5,859,238个提交（另有OpenSSL、MySQL等小数据集）的语料上，以135个CVE、936个文件/提交的人工标注为真值，Linux内核精度达94.94%、召回86.92%，显著优于B-SZZ（64.56%/52.19%）、V-SZZ（64.30%/83.15%）与Redebug（72.89%/70.40%）；查找单个VIC平均仅0.889秒；消融表明噪声过滤与模式化关键语句提取贡献显著。

我的思考：核心洞察是"补丁未涉及但触发漏洞的语句"同样关键，这是超越SZZ类方法的关键；精确匹配策略简单、可解释且高效，但对关键语句的重构（如常量替换）脆弱，CWE-476等大补丁场景召回偏低，跨函数关键语句仍会遗漏；与NVD对比显示3,826/6,920个CVE无VIC记录、767个不匹配，凸显了该工具对漏洞数据治理的实际价值。

- **论文 PDF**: [Accurate Identification of the Vulnerability-Introducing Commit based on Differential Analysis of Patching Patterns.](https://www.ndss-symposium.org/wp-content/uploads/2026-s140-paper.pdf)

#### ADGFUZZ: Assignment Dependency-Guided Fuzzing for Robotic Vehicles.
- **作者**: Yuncheng Wang, Yaowen Zheng, Puzhuo Liu, Dongliang Fang, Jiaxing Cheng, Dingyi Shi, Limin Sun
- **方向**: 漏洞挖掘与利用
- **解读**: 机器人车辆（RV）控制软件的参数、命令、传感输入组合空间巨大，既有 fuzzing（RVFuzzer 无代码区域引导、PGFuzz 依赖文档规则）难以发现深层缺陷；对 ArduPilot 十年 issue 的实证显示 28.02% 的语义缺陷源于赋值语句错误。
ADGFuzz 静态构建赋值依赖图（ADG）刻画变量间依赖，以叶子变量名为线索，利用同义词/物理耦合表做术语匹配，把依赖映射到 RV 输入空间形成匹配输入集（MIS）；再用基于熵的优先调度对 MIS 执行熵感知模糊，配合坠地/偏离航线/软件崩溃三类 oracle 及输入最小化去重。
在 Copter/Plane/Rover 三类机型发现 87 个唯一缺陷（78 个此前未知），其中 26 个（29.88%）可致坠机、15 个（17.24%）航线偏离、46 个（52.87%）内存溢出；16 个已获确认修复；比 PGFuzz 多发现 79 个；熵优先消融比随机多发现 18/16/9 个；150 对 ADG-MIS 人工校验 87.33% 语义准确。
以“赋值依赖”切入嵌入式控制软件漏洞挖掘，角度新颖且命名相似性假设在实践中有效；局限包括术语表人工构造、对命名约定依赖较强（换代码库泛化性存疑）、SITL 仿真与真机存在差异；与 PGFuzz/RVFuzzer 互补，是 RV/工控模糊测试的重要推进。

- **论文 PDF**: [ADGFUZZ: Assignment Dependency-Guided Fuzzing for Robotic Vehicles.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1014-paper.pdf)

#### An LLM-Driven Fuzzing Framework for Detecting Logic Instruction Bugs in PLCs.
- **作者**: Jiaxing Cheng, Ming Zhou, Haining Wang, Xin Chen, Yuncheng Wang, Yibo Qu, Limin Sun
- **方向**: 漏洞挖掘与利用
- **解读**: PLC 固件内置厂商私有逻辑指令库，可能含输入检查缺失、内存破坏、权限缺陷等，经物理控制例程、网络服务或运行时子系统被利用；固件闭源、无法插桩，静态分析与传统 fuzzing 均难生效。
LogicFuzz 是首个面向真实 PLC 指令级 fuzzing：合并手册与 CWE 语料构建语义依赖图（SDG）；以 enable 信号加 bReset 上升/下降沿机制让 LLM 合成可控、可复位的种子程序；对 SDG 施加 Reorder/Rewire/Delete/Insert 变异以多样化调用上下文；用 UCB 多臂赌博机做覆盖引导参数变异；以日志、状态灯、通信状态构成多源 oracle。
在 Rockwell/Siemens/Wago 六台真机上测试 338 条指令，有效种子率 88.47%，发现 19 个指令级缺陷（含 4 个此前未知漏洞，如 GSV/SSV 的时钟边界缺失致 DoS、Wago 内存拷贝重叠致溢出、SysFileWrite 任意路径写）；LLM 总成本 146.31 美元；种子通过率远超 Agent4PLC（25.01%）等基线。
首次把 LLM+SDG 流水线用于真机 PLC 指令 fuzzing，可复位机制解决状态污染是工程亮点；局限在于依赖厂商手册与工程软件（AutoIt 自动化）、覆盖率为粗粒度内存块、LLM 幻觉需多重校验；对工控安全有现实价值，但真机资源与规模化部署成本仍是门槛。

- **论文 PDF**: [An LLM-Driven Fuzzing Framework for Detecting Logic Instruction Bugs in PLCs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1081-paper.pdf)

#### Anota: Identifying Business Logic Vulnerabilities via Annotation-Based Sanitization.
- **作者**: Meng Wang, Philipp Görz, Joschua Schilling, Keno Hassler, Liwei Guo, Thorsten Holz, Ali Abbasi
- **方向**: 漏洞挖掘与利用
- **解读**: 业务逻辑漏洞占 CWE Top 40 的 27 项，传统 fuzzing sanitizer 只擅长内存安全类缺陷，因需应用语义上下文而几乎失效；自动启发式推断脆弱、依赖非可移植的语言特性，无法可靠覆盖。
ANOTA 采用“策略与机制分离”的人机协同 sanitizer：轻量注解系统（系统调用、数据流污点、对象访问、代码执行四类加清除指令）让开发者直接编码预期行为；运行时策略监控器（CPython 钩子 + LLVM pass + eBPF 系统调用监控）把策略违例升级为崩溃供 fuzzer 检测；与 Atheris 组合成 ANOTA+FUZZER。
复现 47 个已知漏洞中的 43 个；在 60 个流行 Python 项目中发现 22 个新漏洞（17 个获 CVE）；消融基准上 35 个漏洞全检出而裸 Atheris 为 0；11 名参与者 66 个任务成功率 83.3%；Python Performance Benchmark Suite 下开销约 10%；开发者访谈 10/10 认可其价值。
“人给出策略、机器执行机制”切中业务逻辑检测的语义鸿沟，比脆弱的自动推断更可靠可移植；局限在于注解质量依赖写注解者的知识、Python 原型需移植到其他语言生态、误报随注解位置敏感；相比 Atropos/EDEFuzz 等通用性更强，是 sanitizer 范式的重要补充。
- **论文 PDF**: [Anota: Identifying Business Logic Vulnerabilities via Annotation-Based Sanitization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f938-paper.pdf)

#### Bullseye: Detecting Prototype Pollution in NPM Packages with Proof of Concept Exploits.
- **作者**: Tariq Houis, Shaoqi Jiang, Mohammad Mannan, Amr Youssef
- **方向**: 漏洞挖掘与利用
- **解读**: **问题**：原型污染是 JavaScript/Node.js 生态的严重漏洞，攻击者可通过 `__proto__` 等特殊属性向全局原型注入恶意属性。静态分析（如 ODGen）扩展性差且误报高；动态分析因代码可达性差漏报高。目标：可扩展、零误报、能产出 PoC 的自动化检测。

**方法**：Bullseye 是全自动动态分析框架：多策略导入（CJS/ESM、动态导入等）并递归遍历导出对象实现全面入口点覆盖；复用包自带测试套件输入并叠加历史利用输入做上下文感知的利用生成；采用双运行时验证预言机（递归原型链检查＋差分状态比较），每用例独立 VM、Proxy 回溯调用栈定位 sink。

**结果**：8 小时内分析 44,513 个高流行度与 5,879 个低流行度包，平均 0.51 秒/包；在 290 个包的 807 个入口点发现零日漏洞，零误报；获 149 个 CVE（66 个公开，25 个 critical、34 个 high）。

**我的思考**：首次在生态规模上实现零误报的零日挖掘并自动生成 PoC，149 个 CVE 是硬指标；局限是污染后的实际危害不在评估范围、依赖测试套件质量。相比 Arteau 固定输入列表，上下文感知生成显著提升可达性。
- **论文 PDF**: [Bullseye: Detecting Prototype Pollution in NPM Packages with Proof of Concept Exploits.](https://www.ndss-symposium.org/wp-content/uploads/2026-s211-paper.pdf)

#### Efficiently Detecting DBMS Bugs through Bottom-up Syntax-based SQL Generation.
- **作者**: Yu Liang, Peng Liu
- **方向**: 漏洞挖掘与利用
- **解读**: 
数据库系统（DBMS）的高复杂度使其极易出 bug，触发后可能造成资金损失或数据泄露。现有语法型 SQL 生成工具全部采用自顶向下（Top-down）方式：从根节点正向展开语法树，资源被消耗在贴近根部的浅层语法上，而真正对应复杂内部实现、易于触发 bug 的 feature-rich 深层语法却被忽视，导致 bug 发现效率低下。

作者提出自底向上（Bottom-up）的语法生成技术：从一条"有趣"的 feature-rich 语法规则（默认取 SQL 表达式语法，如 PostgreSQL 的 a_expr/b_expr/c_expr）出发，回溯到根构造语法路径，再将多条路径扩展合并为多样化的 SQL 查询用于模糊测试。原型工具 SQLBull 采用该技术，测试时无需手工标注语法权重，可自动适应不同 DBMS 方言。

评估中 SQLBull 在 5 个成熟 DBMS（MySQL、MariaDB、CockroachDB、DuckDB、PostgreSQL）上发现 63 个零日 bug（DuckDB 31、MySQL 10、CockroachDB 14、MariaDB 5、PostgreSQL 3），全部经开发者确认；在 bug 发现数与代码覆盖率上均超越 SQLancer、SQLsmith、Squirrel、AFL 等现有工具。

这是首个把"自底向上"语法探索范式引入 DBMS 测试的工作，巧妙地把资源导向 GROUP BY、WINDOW、JOIN、CASE 等 bug 高频特征，且无需人工模板。63 个零日 bug 在如此成熟的系统上极具说服力。局限在于探索起点（默认表达式语法）的选择会影响发现能力，作者也通过 SQLBullNE 变体做了对比；评估在单核 24 小时环境下进行，实际部署效率有待验证。

- **论文 PDF**: [Efficiently Detecting DBMS Bugs through Bottom-up Syntax-based SQL Generation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f198-paper.pdf)

#### Identifying Logical Vulnerabilities in QUIC Implementations.
- **作者**: Kaihua Wang, Jianjun Chen, Pinji Chen, Jianwei Zhuge, Jiaju Bai, Haixin Duan
- **方向**: 漏洞挖掘与利用
- **解读**: 

QUIC 已成现代互联网核心传输协议（W3Techs 统计超 33% 网站支持 HTTP/3），其逻辑漏洞（非内存损坏、源于实现逻辑错误）可造成 DoS 等严重后果，如 CVE-2024-22189 类似 HTTP/2 Rapid Reset。但现有 QUIC 测试工具聚焦内存漏洞，逻辑漏洞发现仍高度依赖人工审计。

作者提出黑盒 fuzzing 框架 MerCuriuzz：分段变异策略将测试用例生成拆为帧内容、帧序列、交互行为三个变异阶段以控制状态空间爆炸；基于差分测试的语义一致性检测器加公共资源消耗检测器捕捉无崩溃迹象的逻辑异常；基于 Nyx 快照的毫秒级恢复消除服务重启与建连开销（约 1.8 万行 Rust、1 千行 C，LibAFL 实现）。

对 QUIC 工作组推荐的 16 个实现做 112 对差分组合、每对 24 小时 fuzzing，共生成 1000 万测试用例，发现 14 个新逻辑漏洞（quiche、xquic、aioquic 等），归纳为 6 类（CRYPTO Flood、Path Challenge Flood、NCI Flood、Connection Hold-On Flood 等），获 5 个 CVE、3 个阿里漏洞库编号与 4 个漏洞赏金；快照机制带来最高 350%（平均 225%）执行效率提升，对比 BooFuzz/DPIFuzz 24 小时未发现任何漏洞。

这是首个系统化自动发现 QUIC 逻辑漏洞的黑盒框架，两级检测器互补性强（消融显示去语义检测剩 9 个、去资源检测剩 8 个，各自漏掉对方独有的漏洞）。局限在于威胁模型要求攻击者能本地执行受害者程序观察资源占用，且差分判定依赖多实现一致性，对同源同错场景可能失效。

- **论文 PDF**: [Identifying Logical Vulnerabilities in QUIC Implementations.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1777-paper.pdf)

#### LLMBisect: Breaking Barriers in Bug Bisection with A Comparative Analysis Pipeline.

- **作者**: Zheng Zhang, Haonan Li, Xingyu Li, Hang Zhang, Zhiyun Qian
- **方向**: 漏洞挖掘与利用
- **解读**: Bug二分（bug bisection）要定位引入漏洞的提交（BIC），对N-day漏洞的版本影响范围判定至关重要，但现有基于补丁的方法障碍重重：假设BIC与补丁修改同一函数（常常不成立）、只分析代码变更而忽视含丰富线索的提交消息、依赖"BIC初始化补丁删除行"等简单启发式、缺乏对漏洞逻辑的理解。作者观察到LLM能同时理解代码与自然语言，适合突破这些屏障。

LLMBisect构建多阶段流水线：先用三种互补的候选生成器（补丁函数基、关键行基、提交消息基）从提交历史提取候选BIC；再经BIC过滤阶段（预过滤选出所有潜在BIC、后过滤让LLM在候选间比较择优）；最后结果终选阶段对最多3个候选做比较评估并执行7次多数投票，抑制LLM的自一致性问题。设计核心是利用LLM擅长的"比较推理"而非孤立判断。

在200个Linux内核CVE（2023/2024各100个）上，LLMBisect达到91%准确率，显著超过V0Finder（33%）与VSZZ（51%）；在SymBisect自己的数据集上以90.6%胜过SymBisect的75%；脆弱版本检测F1达96.5%（对比工具平均67.9%）。2023与2024年准确率几乎无差（92% vs 90%），排除了LLM预训练知识的影响。相比SOTA提升超38%，比朴素LLM基线（仅30%）提升60%。

LLMBisect把LLM引入BIC识别这一传统静态分析任务，其"以比较代替分类"的三阶段漏斗设计有效控制了LLM误报与成本（候选削减81%）。局限：18个失败案例中10个源于候选生成阶段漏掉BIC（与补丁改不同文件，代价过高）；ground truth依赖带"Fixes"标签的提交，可能存在数据偏差；o1级模型token开销高。总体上为patch-based二分提供了可复用的新范式。

- **论文 PDF**: [LLMBisect: Breaking Barriers in Bug Bisection with A Comparative Analysis Pipeline.](https://www.ndss-symposium.org/wp-content/uploads/2026-s990-paper.pdf)

#### MUTATO: Enhancing Fuzz Drivers with Adaptive API Option Mutation.
- **作者**: Shuangxiang Kan, Xiao Cheng, Yuekang Li
- **方向**: 漏洞挖掘与利用
- **解读**: 问题：谷歌研究指出现代覆盖率引导模糊测试的瓶颈在 fuzz driver 而非 fuzzer；库 API 中决定执行语义的 option 参数几乎不被变异，固定选项会掩盖依赖特定配置的漏洞路径。

方法：MUTATO 在 driver 层嵌入选项变异逻辑（fuzzer 无关）：覆盖率停滞（10,000 次执行或 5 秒）时用覆盖引导的 ε-greedy 策略切换选项值；选项与输入数据作为独立变异域，避免前缀编码的域混淆；同时探索合法与非法选项值，用 OPFL 语言指导。

结果：10 个 C/C++ 库、24h×10 次实验下，AFL++/LibFuzzer 覆盖率平均提升 14%/13%；发现 12 个未知漏洞（含 3 个 CVE），其中 4 个在 OSS-Fuzz 超 18,060 小时未发现的 API 上 5 小时内被找到；速度仅降 4.4%。

我的思考：纯 driver 层改动、轻量通用，与 OSS-Fuzz 生态兼容；与前缀编码对比有力证明独立变异域的必要性。局限：依赖覆盖率平台期触发、需 OPFL 人工标注，新漏洞以 NPD/HBO 等中危为主。

- **论文 PDF**: [MUTATO: Enhancing Fuzz Drivers with Adaptive API Option Mutation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s820-paper.pdf)

#### NEXUS: Towards Accurate and Scalable Mapping between Vulnerabilities and Attack Techniques.

- **作者**: Ehsan Khodayarseresht, Suryadipta Majumdar, Serguei A. Mokhov, Mourad Debbabi
- **方向**: 漏洞挖掘与利用
- **解读**: CVE 每年记录数千漏洞却不含可利用背景，MITRE ATT&CK 定义攻击战术/技术（TTP）却不与漏洞关联，打通二者可显著提升威胁检测与缓解效率。现有工作受四大挑战制约：缺少大规模高质量 CVE-TTP 标注数据（原有数据集不足 8K CVE、52 个 TTP）、从非结构化 CVE 描述提取攻击行为困难、数据不均衡且关键 TTP 缺失（如 ENISA 中 T1027 有 2699 条 CVE 而 T1190 仅 427 条，T1059 完全缺失）、缺乏持续自纠正的适应性学习机制。

NEXUS 以混合架构应对：沿 CVE→CWE→CAPEC→TTP 知识链自动构建 92,632 条 CVE、141 个 TTP 的初始标签；用语义角色标注（SRL）拆分句子并建依赖图，以微调 SecureBERT 的攻击模式检测器（APD，F1 95.86%）抽取攻击子句；用 GPT-4o-mini 做标签校正、补齐 67 个缺失 TTP（扩至 208 个），用 Llama-3.1-8B 生成 117,725 条改写样本扩充至 210K+ 条；最终以冻结编码器+分类头的 RoBERTa-Large 做多标签分类，并引入基于精确匹配与行为签名的分析师反馈自适应机制（无需重训练）。

评估显示，自建数据集上 micro F1 达 97.94%（macro 98.45%），公开数据集 D2-D5 上均优于基线（D2 F1 89.31% vs 最佳基线 67.68%）；消融表明各步骤均有增益（地面真值 79.38%→攻击模式提取 98.42%→增强后 97.94%）；反馈自适应在三轮内把 D5 F1 从 21.05% 提至 67.30%。全流程仅耗 6.9M 次 GPT 调用（约 $607），推理内存 2.85GB、CPU 占用<15%。

我的思考：NEXUS 以近乎全自动流水线首次把 CVE-TTP 映射推进到 208 TTP、210K CVE 的量级，且混合设计（判别式分类器+生成式 LLM）比纯生成方法在规模与噪声数据上更稳。但标签依赖 LLM 自动生成，存在错误传播风险（人工验证仅 10 条 CVE）；APT 报告（D4）与 SMET（D5）上 F1 明显更低，说明跨文本类型泛化仍有限；整体属扎实的工程贡献，科学突破相对有限。

- **论文 PDF**: [NEXUS: Towards Accurate and Scalable Mapping between Vulnerabilities and Attack Techniques.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2926-paper.pdf)

#### PhantomMap: GPU-Assisted Kernel Exploitation.

- **作者**: Jiayi Hu, Qi Tang, Xingkai Wang, Jinmeng Zhou, Rui Chang, Wenbo Shen
- **方向**: 漏洞挖掘与利用
- **解读**: CPU 侧内核缓解（W⊕X、KASLR、CFI、PAN、SLAB VIRTUAL、SELinux 等）已显著抬高传统利用门槛，而 GPU 侧防护薄弱；Arm Mali GPU 是移动端部署最广的 GPU（近三年平均约 46% 市场份额），Operation Triangulation 等事件已证明 GPU 漏洞可绕过 CPU 防御，但 Mali GPU 内存管理此前缺乏系统性安全分析。

论文首次深入分析 Mali GPU 内存映射机制，发现两个安全弱点：分配-映射解耦（驱动先分配物理内存、之后才更新页表，中间存在攻击者可操控的 TOCTOU 窗口）与缺失物理地址验证（映射前不校验物理页合法性）。据此提出 PhantomMap：劫持驱动合法映射工作流，把有限的堆漏洞（UAF/OOB/双重释放）转化为任意物理内存读写原语，通过 GPU 发起的写入绕过主流内核防御，且无需特权能力或信息泄露。

配套的 LLVM 静态分析器在 JM 与 CSF 两种驱动架构上识别出 15 条利用链（12 条可读写映射任意物理内存）；基于 13 个真实 CVE 开发 15 个端到端提权利用，包括首个公开的 CVE-2025-21836 利用；CVE-2023-48409 成功率从现有利用的 45% 提升到 90%，CVE-2022-20409 稳定超 95%，stress-ng 高负载下从 20% 提升到 50%；在启用 RKP 与 DEFEX 的 Galaxy A71 上成功提权；提出的驱动内缓解（TYPE_GPU 页类型检查）在 Pixel 6/7 上平均开销仅 0.56%/0.34%。

我的思考：首次系统化分析 Mali GPU 映射机制并给出可自动发现的利用链集合，"把有限堆漏洞放大为任意物理内存读写"的技术路线新颖且通用（适配 kmalloc/vmalloc、无需泄露）。局限：仍需预先存在的内核堆漏洞作为入口；作者论证 RKP 因只校验调用者权限、MTE 因只作用于触发阶段而都无法阻止 PhantomMap，但根本修复（启用 SMMU/IOMMU 隔离）受 SoC 集成选择制约，驱动补丁属治标；开销评估仅用 Rodinia 基准。已责任披露 Arm 并全部修复，是移动 GPU 安全的重要里程碑。

- **论文 PDF**: [PhantomMap: GPU-Assisted Kernel Exploitation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f201-paper.pdf)

#### PortRush: Detect Write Port Contention Side-Channel Vulnerabilities via Hardware Fuzzing.
- **作者**: Peihong Lin, Pengfei Wang, Lei Zhou, Gen Zhang, Xu Zhou, Wei Xie, Zhiyuan Jiang, Kai Lu
- **方向**: 漏洞挖掘与利用
- **解读**:
现代CPU中多模块共享有限写端口，写请求超端口数即产生竞争，优先级仲裁使低优先级指令产生可观测时延，与瞬态执行结合即成侧信道，在分区/安全化缓存处理器上依然有效；但既有检测手段均未针对此类漏洞，本文提出PORTRUSH在RTL层面检测并验证该漏洞。

方法分三步：用写请求图（WRG）建模写路径与优先级，静态识别潜在竞争实例；分层聚合解码把各层级信号汇总到顶层监测，同周期写请求超端口数即竞争；PSO优化的竞争引导模糊测试触发竞争，并与瞬态执行攻击模式结合验证。

在BOOM、NutShell、Rocket Core上静态识别177个潜在竞争实例、触发35个；竞争覆盖率达BOOM 83.3%、NutShell 17.6%，远超DiFuzzRTL的5.5%；构造Birgus-variant、MSHRush与Spectre-STC三个攻击向量（错误率<10%、速率>15Kb/s），两个新漏洞获CVE。

我的思考：把端口竞争升级为可系统检测的漏洞类别是主要贡献，WRG使静态-动态-触发闭环成立。局限：Rocket顺序架构无法触发、难触发三方以上竞争、依赖开源RTL难迁移闭源CPU。是微架构侧信道检测的重要一步。
- **论文 PDF**: [PortRush: Detect Write Port Contention Side-Channel Vulnerabilities via Hardware Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f587-paper.pdf)

#### ProtocolGuard: Detecting Protocol Non-compliance Bugs via LLM-guided Static Analysis and Dynamic Verification.
- **作者**: Xiangpu Song, Longjia Pei, Jianliang Wu, Yingpei Zeng, Gaoshuo He, Chaoshun Zuo, Xiaofeng Liu, Qingchuan Zhao, Shanqing Guo
- **方向**: 漏洞挖掘与利用
- **解读**: 网络协议实现须严格遵循自然语言规范（如RFC），但规范歧义常导致实现静默偏离标准，产生非合规缺陷，引发互操作问题与严重安全漏洞（如MatrixSSL因错误实现RFC导致CVE-2022-46505）。这类缺陷无崩溃信号，内存消毒器类模糊测试与差分测试的oracle难以发现，既有静态方法依赖启发式规则，验证与根因分析又需大量人工。本文提出ProtocolGuard，把LLM引导的静态分析与模糊测试动态验证结合，系统检测非合规缺陷。

流程四步：混合式规则抽取（关键词匹配识别候选规则+LLM语境化补全语义+结构化JSON表示）→LLM引导的程序切片（从网络接收点构造消息处理调用图MessageCG，LLM识别处理器与上下文函数，LLVM前向数据流切片，AST补全控制流结构并语义剪枝）→LLM不一致检测（三次查询自一致性收敛）→动态验证：LLM自动生成断言插桩把静默错误转为可观测的断言失败，再生成针对性的Scapy/Python测试用例，用基于AFLNet的定向模糊测试确认并产出PoC。

在11个协议实现（MQTT/CoAP/TLS/FTP/DHCPv6）上从420条规则发现181个不一致（精确率90.6%），确认158个非合规缺陷（156个新发现，70个获厂商确认、17个已修复），缺陷以解析类（37%）与状态机类（22%）为主。相比Cursor+Claude 3.7（71.7%精确率、76.8%召回）与Cursor+DeepSeek R1（49.3%/52.0%），ProtocolGuard达86.3%/81.3%；断言语义准确率88.9%、崩溃触发率68.4%，其初始种子比随机种子多发现155.2%的断言崩溃（DHCP/TLS达275%~600%）。

我的思考：核心创新是用"LLM生成的断言"作为非合规缺陷的oracle，把无崩溃信号的逻辑错误转化为可模糊测试的目标，这一思路可推广到其他静默缺陷检测；案例（wolfSSL TLS降级、Sol缺CONNECT校验、uFTP的AUTH后未重认证）说明缺陷确有真实安全影响。局限包括回调式架构切片不完整、仅支持C语言、LLM上下文长度限制影响大型实现（libcoap精确率仅80%），断言仍有约三成无法触发，且依赖DeepSeek/Cursor等商用模型，成本与可复现性需权衡。
- **论文 PDF**: [ProtocolGuard: Detecting Protocol Non-compliance Bugs via LLM-guided Static Analysis and Dynamic Verification.](https://www.ndss-symposium.org/wp-content/uploads/2026-f521-paper.pdf)

#### ropbot: Reimaging Code Reuse Attack Synthesis.
- **作者**: Kyle Zeng, Moritz Schloegel, Christopher Salls, Adam Doupé, Ruoyu Wang, Yan Shoshitaishvili, Tiffany Bao
- **方向**: 漏洞挖掘与利用
- **解读**: ROP 链构造仍是耗时的手工劳动；现有自动化工具均采用"生成-测试"范式，复杂度随 gadget 数量指数增长，且按设计忽略 ret2csu、GOT 跳转、条件分支等复杂 gadget。

作者提出 ROPBlock 抽象——一组保证可链接的 gadget（正栈变化、PC 来自栈补丁、无条件分支），把寄存器设置从 O(2ⁿ) 降到 O(n) 的图搜索；用符号执行架构无关地识别 gadget，经规范化把复杂 gadget 转成 ROPBlock，再以寄存器移动图迭代优化、寄存器设置图求最短链。

execve 链成功率89.7% vs 次优工具 exrop 33.1%；37个二进制的 dup-dup-execve 全链平均2.5秒生成；mmap 链目标数是最优竞品的5倍；支持 x64/MIPS/ARM/AArch64，RISC-V 仅需12行代码；是唯一能为 Linux 内核、Chromium、Firefox 生成合法链的工具。

该工作以"保证可链接的抽象层"换取多项式复杂度，是攻击合成方向的范式改进，已被 Google 等实际采用；局限在于符号执行仍用于 gadget 效果分析，条件分支 gadget 因状态分叉反而拖慢生成，长链的 payload 空间开销较大。

- **论文 PDF**: [ropbot: Reimaging Code Reuse Attack Synthesis.](https://www.ndss-symposium.org/wp-content/uploads/2026-f845-paper.pdf)

#### RTCON: Context-Adaptive Function-Level Fuzzing for RTOS Kernels.
- **作者**: Eunkyu Lee, Junyoung Park, Insu Yun
- **方向**: 漏洞挖掘与利用
- **解读**: RTOS 攻击面随蓝牙、Wi-Fi 等功能扩大且普遍缺乏防护；模拟器/真机端到端测试覆盖有限，深层函数需复杂协议状态，而现有函数级模糊测试因缺乏上下文提前崩溃并产生大量误报。

RTCON 用双向调用图（SVF 解析间接调用）与过程间污点分析标记上下文变量；自适应上下文生成器插入 sanitization 钩子（崩溃时分配内存或跳过调用）与 generation 钩子（基于分支操作数生成候选值穿越阻塞分支）；多层分类用顶层函数构造 verifier 复现崩溃判高置信。

在 Zephyr、RIOT、FreeRTOS、ThreadX 上发现27个bug（25个新）、获14个CVE，其中20个可远程触发；高置信崩溃精度92.7% vs 低置信5.8%；上下文生成带来5–16%p 覆盖率提升；对比 HOEDUR/SFuzz，覆盖16,247条边/495个崩溃/20个bug 全面领先。

该工作以"多层复现"代替静态约束提取区分真假崩溃，规避了间接调用对静态分析的阻碍，证明函数级模糊测试是 RTOS 深度测试的有效路径；局限在于污点过近似与双指针传播缺失造成误报漏报。

- **论文 PDF**: [RTCON: Context-Adaptive Function-Level Fuzzing for RTOS Kernels.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1600-paper.pdf)

#### SACK: Systematic Generation of Function Substitution Attacks Against Control-Flow Integrity.
- **作者**: Zhechang Zhang, Hengkai Ye, Song Liu, Hong Hu
- **方向**: 漏洞挖掘与利用
- **解读**: 即使静态 CFI 完美精确，攻击者仍可通过函数替换（SUB）攻击把函数指针换成允许集合内的另一目标；Carlini 的控制流弯曲仅手工构造两个例子，缺乏系统化构造方法。

SACK 用良性执行中动态收集的间接调用目标（"子真值"）保证替换在精确 CFI 下合法；用 LLM 从文档生成安全 oracle（编译标志、配置、合法/非法输入与行为监视器）检测语义级违规；在 LLVM IR 层自动枚举替换组合，并核查后续间接调用目标不越界。

七个程序22个 oracle 构造419个 SUB 攻击，攻破18个安全特性（Nginx 限流达82个）；并基于历史漏洞构造5个端到端利用：SQLite3 绕过安全模式实现任意代码执行、V8 把 os.rmdir 替换为 os.system 弹出 shell、Nginx 绕过认证（41个替换）；LLM oracle 18个开箱即用。

以419个攻击对比 CFB 的2例，证明精确静态 CFI 下 SUB 攻击的现实普遍性，警示静态精确化存在天花板、需上下文敏感或语义级防御；局限在于目标集依赖输入覆盖（欠近似）、单点替换假设，τ=1000 截断可能漏掉可行攻击。

- **论文 PDF**: [SACK: Systematic Generation of Function Substitution Attacks Against Control-Flow Integrity.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2317-paper.pdf)

#### SYSYPHUZZ: the Pressure of More Coverage.
- **作者**: Zezhong Ren, Han Zheng, Zhiyao Feng, Qinying Wang, Marcel Busch, Yuqing Zhang, Chao Zhang, Mathias Payer
- **方向**: 漏洞挖掘与利用
- **解读**: 内核模糊测试以最大化代码覆盖率为目标，但覆盖率已趋于平台期，且高覆盖率并不代表充分探索——低频代码区域可能隐藏被长期忽视的漏洞，需要转向以代码执行频率为导向的模糊测试。

基于对 Syzkaller 在 Linux 内核上 72 小时 fuzzing 的细粒度命中频率研究（低频区域定义为命中数最低的 5% 基本块），论文提出 SYSYPHUZZ：选择性任务调度通过 boost 任务动态优先探索低频区域并避免任务爆炸，上下文保持变异维护系统调用间的依赖关系，另以 denylist 排除不可达与非确定性区域。

结果将低频区域存活率从 22.0% 降至 19.3%，低频块命中数提升约 2 倍；共发现 67 个 bug，比 Syzkaller（51 个）多 31%、比 SyzGPT（61 个）多 9.8%，其中 20 个为独有发现，还找到 5 个 Syzbot（运行于数百台 VM 的持续 fuzzing 服务）未发现的 bug；与 SyzGPT 集成后独有 bug 增加 33%。

以执行频率替代覆盖率作为新优化目标，实证充分且已开源、正尝试上游合并；但 5% 低频阈值为经验选择，独有 bug 与低频区域的因果关联尚未达统计显著，需要更精细的归因分析。

- **论文 PDF**: [SYSYPHUZZ: the Pressure of More Coverage.](https://www.ndss-symposium.org/wp-content/uploads/2026-s921-paper.pdf)

#### Through the Authentication Maze: Detecting Authentication Bypass Vulnerabilities in Firmware Binaries.

- **作者**: Nanyu Zhong, Yuekang Li, Yanyan Zou, Jiaxu Zhao, Jinwei Dong, Yang Xiao, Bingwei Peng, Yeting Li, Wei Wang, Wei Huo

- **方向**: 漏洞挖掘与利用

- **解读**: 路由器、网关等嵌入式设备的 Web 服务常暴露于公网，认证绕过漏洞可让攻击者无凭证获得特权访问（如 Cisco IOS XE 的 CVE-2023-20198 曾感染超 14 万系统）。已有检测工具（Firmalice、Weasel）依赖手工标注或僵化启发式定位认证代码，面对 Basic、NTLM 等多种并存认证机制缺乏泛化能力。

作者提出 AuthSpark 动态分析框架，基于两个观察：成功与失败认证请求的执行轨迹在凭证校验语句（CVS）之前高度相似、之后显著分叉；认证结果变量沿控制/数据依赖传播。据此先用 LCS 分治与相似度评分定位 CVS，再迭代过程间数据流分析识别认证变量与认证成功基本块（ASBB），最后用覆盖、距离、字符串引导三类策略的定向灰盒模糊探索绕过路径，并做差分测试过滤误报。

在 12 个厂商 32 个固件（含 14 个已知漏洞）上，正确定位 42/44 个凭证校验点（Weasel 仅 16/44），检出全部 14 个已知漏洞（完整版识别 383 个 ASBB，消融显示各识别规则均显著贡献）；对最新固件发现 6 个零日认证绕过漏洞，其中 4 个获官方编号（3 个 CVE、1 个 PSV）。

我的思考：把认证绕过检测形式化为"定位 CVS、识别 ASBB、探索绕过路径"三步，用轨迹差分这一动态手段替代脆弱启发式，泛化到不同认证机制，并以真实零日验证了实用性，是逻辑漏洞检测的重要进展。局限：仍需人工准备成功/失败请求对，超长执行轨迹会导致 CVS 误排，重托管（QEMU）与 GDB 追踪覆盖的架构有限。与 Firmalice/Weasel 相比自动化程度与准确率大幅提升，但认证绕过这类逻辑漏洞检测整体仍远未饱和，值得向更多设备类型扩展。

- **论文 PDF**: [Through the Authentication Maze: Detecting Authentication Bypass Vulnerabilities in Firmware Binaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2757-paper.pdf)

#### Token Time Bomb: Evaluating JWT Implementations for Vulnerability Discovery.
- **作者**: Jingcheng Yang, Enze Wang, Jianjun Chen, Qi Wang, Yuheng Zhang, Haixin Duan, Wei Xie, Baosheng Wang
- **方向**: 漏洞挖掘与利用
- **解读**:
JWT 已成为现代 Web 与分布式系统认证授权的基石（CloudFlare、Let's Encrypt、Kubernetes 等），但实现层面的签名验证绕过、令牌伪造与 DoS 漏洞屡见不鲜；此前研究只报道个案，缺少对 JWT 实现的系统性评估，且 JWT 漏洞多为逻辑缺陷、不触发崩溃，传统模糊测试难以发现。

本文提出 JWTeemo：首先提出 FBNF（函数扩展 BNF）文法，把 HMAC/RSA 签名等函数计算与 alg 依赖等上下文敏感关系建模进语法生成，并用 UCT-Rand 算法根据解析反馈动态调整生成路径权重；再构建差分分析器，跨库对比同一 JWT 的解析结果一致性（发现认证绕过类逻辑漏洞），并监测单库 CPU/内存资源消耗超基线情况（发现 DoS）。

对 43 个流行 JWT 库（10 种语言）生成 10 万测试用例，发现 31 个此前未知漏洞、20 个获 CVE；归纳为五类：签名/加密混淆、算法混淆、JWT 格式混淆（可致认证绕过）、Billion Hashes 攻击与压缩 DoS——可实际绕过 Kubernetes 认证、对 Apache James 发动 DoS；71.9% 支持 PBES2 的实现存在 Billion Hashes 缺陷、86.7% 支持 JWE 的实现存在压缩攻击缺陷；IETF 认可其缓解建议并拟纳入新的 JWT BCP RFC，Apache、Kubernetes、Let's Encrypt、RedHat 等致谢并发放赏金。

这是对 JWT 实现的首次大规模系统化评估，FBNF 把协议模糊测试从纯语法推进到"语法+语义函数"层面，UCT 反馈机制能自动学习 claim 间依赖（如 p2s/p2c 共现），方法论可迁移到 SAML 等协议；Kubernetes 案例揭示了"按点分割取 payload"与"go-jose 校验"语义不一致这一普遍根因；局限在于差分测试依赖参考实现、假阳性率 35.1%，且多数漏洞根因是规范约束过松，缓解需要规范（BCP RFC）与实现（拒绝危险参数）双管齐下。
- **论文 PDF**: [Token Time Bomb: Evaluating JWT Implementations for Vulnerability Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-f697-paper.pdf)

### 网络与协议安全（42 篇）

#### A Hard-Label Black-Box Evasion Attack against ML-based Malicious Traffic Detection Systems.
- **作者**: Zixuan Liu, Yi Zhao, Zhuotao Liu, Qi Li, Chuanpu Fu, Guangmeng Zhou, Ke Xu
- **方向**: 网络与协议安全
- **解读**: 现有针对ML恶意流量检测的逃逸攻击或依赖受限条件（加密协议、Tor、专用设备），或需要白盒/灰盒先验，缺少跨协议、跨任务、仅凭"放行/阻断"硬标签即可生效的通用攻击，这使真实黑盒场景下的检测鲁棒性难以验证。

作者提出NetMasquerade：先构建Traffic-BERT预训练模型，用网络专用分词器（IPD取对数后分桶、包长直接取值）与新颖的双序列bi-cross注意力，通过Mask-Fill任务学习良性流量分布；再把流量改造建模为有限时域MDP，用GRU策略网络与SAC算法逐步选择"修改包长/IPD或插入chaff包"的动作，仅依赖目标系统的硬标签反馈，并引入相异度惩罚与有效性惩罚保证最小改动与攻击语义。

实验在80个攻击场景下对6种检测系统（Whisper、FlowLens、NetBeacon、Vanilla+RNN、CICFlowMeter、Kitsune）平均ASR超过96.65%（各系统0.9626~0.991），较最优基线提升2.61%~21.88%，还能绕过经验或形式化鲁棒的检测器；修改步数不超过10，带宽分布KL散度仅0.013；吞吐达4.239K包/秒，比Traffic Manipulator快约69.6×；对5%~30%反馈噪声也能优雅退化。

我的思考：把"良性流量更稠密、恶意流量更稀疏"的密度观察转化为预训练+RL的工程方案，且用DPDK实测验证了线速可行性，工程完成度高；局限在于依赖探测阶段收集反馈、扰动仅限包长/IPD两类特征、chaff包与真实攻击流的一致性仍可被防御方利用；与Amoeba（逐包截断填充、需接收端协作）相比，NetMasquerade的流级改造更贴近真实攻击场景，也为检测端研究对抗训练之外的鲁棒策略敲响警钟。

- **论文 PDF**: [A Hard-Label Black-Box Evasion Attack against ML-based Malicious Traffic Detection Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s916-paper.pdf)

#### Aliens Among Us: Observing Private or Reserved IPs on the Public Internet.
- **作者**: Radu Anghel, Carlos Gañán, Qasim Lone, Matthew Luckie, Yury Zhauniarovich
- **方向**: 网络与协议安全
- **解读**: Bogon 流量（源地址为私用/保留地址）是网络卫生差的标志，使 DDoS 便于伪装来源、妨碍取证；尽管 BCP 38/84 建议多年，源地址验证（SAV）部署仍不一致，本文用八年数据量化数据面 Bogon 现象。
分析 2017–2024 年每月 18 日 CAIDA Ark traceroute（共约 10.85 亿条），用 RIPE RIS/RouteViews 历史 BGP 把每跳映射到 ASN，清洗路径后按 BA/BB/BC 三类判定转发/包含 Bogon 的 AS，并与 Spoofer、MANRS 交叉验证。
82.69%–97.83% 的 Ark 观测点遇到含 Bogon 路径；21.11% 的 traceroute 含 RFC1918 地址（RFC6598 占 1.68%、RFC3927 占 0.08%）；识别出 15,541 个转发 Bogon 流量的 AS，但仅 11.88% 在过半测量中出现；与 Spoofer 交叉显示 52.71% 转发 Bogon 的 AS 被归为不可伪造，说明 SAV 部署与控制面保证脱节；这些 AS 多为 ISP（65.49%）。
首次以 traceroute 视角系统性刻画 Bogon 传播，样本量大、方法可复现；局限在于依赖路径不对称与 VP 轮换，AS 时有时无难以确证持续违规；与 Spoofer 数据互补而非替代；对运营商的启示是数据面监测可作为 SAV 部署的独立审计手段。

- **论文 PDF**: [Aliens Among Us: Observing Private or Reserved IPs on the Public Internet.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1118-paper.pdf)

#### BACnet or "BADnet"? On the (In)Security of Implicitly Reserved Fields in BACnet.
- **作者**: Qiguang Zhang, Junzhou Luo, Zhen Ling, Yue Zhang, Chongqing Lei, Christopher Morales, Xinwen Fu
- **方向**: 网络与协议安全
- **解读**: BACnet 是楼宇自动化系统（BAS）的主导协议，约占全球市场 77%、涵盖约 1,539 家制造商，但其安全研究多聚焦加密与认证，忽略了字段定义的语义缺口。作者发现 BACnet 中存在大量"隐式保留字段"——字段理论容量远大于实际受限范围，未被使用的取值区间若缺少输入校验，可引发未定义行为、数据损坏、崩溃乃至任意代码执行；而 BACnet 的 MS/TP 链路依赖令牌传递、吞吐极低，现有模糊测试在该协议上效率很差。

方法上，作者提出行为驱动的模糊测试器 BACSFUZZ，利用 BACnet 令牌夺取（token-seize）机制辅助模糊测试，针对隐式保留字段的未校验取值区间进行定向变异，并配合真实设备/实现的行为反馈；评估覆盖 15 个 BACnet 与 5 个 KNX 实现，厂商包括 Siemens、Honeywell、Johnson Controls 等主流供应商。

结果显示，与最先进的模糊器相比，BACSFUZZ 吞吐量提升 272.49% 至 776.01%；共发现 26 个与隐式保留字段相关的漏洞（18 个 BACnet、8 个 KNX），其中 24 个获厂商确认、9 个被分配 CVE；案例研究还表明，畸形 MS/TP 报文无需合法证书即可使管理软件崩溃或干扰告警，直接影响物理安防控制与关键告警可见性；ASHRAE 亦确认了协议层面的令牌夺取漏洞。

我的思考：本文把"隐式保留字段"这一长期被忽视的协议语义缺口系统化，并针对 MS/TP 低吞吐瓶颈提出了协议感知的令牌夺取模糊方案，威胁模型（无需证书即可攻击）非常现实，工程与影响价值兼备；局限在于漏洞面集中于 MS/TP 链路与保留字段，BACnet/IP 及认证场景的覆盖仍有限；其启示是楼宇自控安全既要加密信道，也要修补无认证的遗留信任边界，协议规范与实现的双向校验同样重要。


#### Beyond RTT: An Adversarially Robust Two-Tiered Approach For Residential Proxy Detection.
- **作者**: Temoor Ali, Shehel Yoosuf, Mouna Rabhi, Mashael Al Sabah, Hao Yun
- **方向**: 网络与协议安全
- **解读**:
住宅IP（RESIP）代理网络规模空前（Bright Data一家运营超7200万IP），把欺诈、爬虫、DDoS等恶意流量隐藏在合法家庭地址之后。既有检测依赖跨层RTT差异，但作者证明其根本不可靠：简单流量调度攻击即可让最强分类器的召回率从99%暴跌至8%。

论文首次分析最大提供商Bright Data的EarnApp，15个月采集约900GB、超11万条relay连接。提出双层框架：轻量XGBoost采用流量统计特征加新型流关联特征（网关与中继流量按0.1秒时间窗分箱、z-score归一化后逐元素乘积求相似度）；重量级CorrTransform为Transformer（4层4头、relay与gateway流拼接输入、Stochastic Depth与TokenDrop正则），仅用前50个包。

网关设备识别精度99.42%、召回98.08%（FPR 0.01%）；连接分类CorrTransform正常达99.62% F1，在调度、填充、包重塑、抖动及组合攻击下保持92%以上F1（调度下特征模型仅74.03%）；服务端区分直连与代理流量FPR低于0.2%。唯一弱点：调度+包重塑组合降至77.24%。

该工作把检测从脆弱时序指纹转向"网关-中继流量必然相关"的架构不变量，攻击者难以在不损伤服务的前提下消除，且首次覆盖此前被忽略的最大供应商。局限：背景流量为模拟合成；关联特征依赖先识别网关；实验集中于单一提供商，跨供应商泛化仅靠架构相似性推断。

- **论文 PDF**: [Beyond RTT: An Adversarially Robust Two-Tiered Approach For Residential Proxy Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2086-paper.pdf)

#### BunnyFinder: Finding Incentive Flaws for Ethereum Consensus.
- **作者**: Rujia Li, Mingfei Zhang, Xueqian Lu, Wenbo Xu, Ying Yan, Sisi Duan
- **方向**: 网络与协议安全
- **解读**: **问题**：以太坊 PoS 依赖激励机制维持稳定，但激励设计存在缺陷（如楼梯攻击控制 29.6% 质押即可使诚实验证者零奖励）。发现此类缺陷依赖专家知识与大量人工，拜占庭行为空间无穷、无法穷举。

**方法**：BunnyFinder 借鉴故障注入思想，由四组件构成：策略生成器从 12,896 个理论攻击实例生成 9,354 个具体实例；执行器在 Prysm 与 Teku 上自动运行；状态分析器按指标（诚实方奖励低于公平份额、拜占庭方获利）判定激励缺陷；优化器用延迟奖励强化学习微调攻击参数。

**结果**：复现 5 个已知攻击；发现 3 个新攻击：以太坊 PoS 首个自私挖矿（13.4% 质押即可获利）、staircase attack-II（33.3% 质押下所有诚实验证者受罚）、pyrrhic victory 攻击；还发现 Prysm 两个实现缺陷；RL 优化使 staircase-II 收益接近翻倍。

**我的思考**：把自动化测试范式引入共识设计审计，新攻击获以太坊基金会确认并推动版本更新，价值明确；但仍是半自动：策略生成依赖启发式、缺陷判定需人工核查，筛选效率有提升空间。框架可推广到 propose-vote 范式的其他链。
- **论文 PDF**: [BunnyFinder: Finding Incentive Flaws for Ethereum Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-s281-paper.pdf)

#### CELLSHIFT: RTT-Aware Trace Transduction for Real-World Website Fingerprinting.
- **作者**: Rob Jansen
- **方向**: 网络与协议安全
- **解读**: **问题**：WF 攻击在入口侧观测，但真实轨迹只能出口侧采集（入口无法解密标签），位置错配使准确率下降 17% 中位、93% 最坏；既有方法 Retracer 重放 115,000 条轨迹需 495 GiB 内存、29.9 小时，成本极高。

**方法**：核心洞见：轨迹元数据已可多次估计电路 RTT。CELLSHIFT 提取 RTT（CONNECTED/SENDME 流控）、分解传播时延与拥塞、按目标视点重写时间戳；TRACE MOVE 生成测试集、TRACE MORPH 按 RTT 分布增强生成训练集；纯数学运算、可并行。

**结果**：TRACE MOVE 轨迹在 6 种距离函数下最接近真实入口轨迹，分类器准确率高 1–7 pp；TRACE MORPH 闭世界提升 4–14 pp（合成）与 5–25 pp（真实 GTT23）；吞吐约 2,000 条/秒/核，比 Retracer 快五个数量级。

**我的思考**：把位置迁移从重型模拟降为轻量运算，并揭示 Retracer 测试轨迹过短的伪影，评估严谨；局限是依赖信元级轨迹与协议细节。该工作让真实 WF 风险评估可行，也提示攻击者可借增强训练提升攻击力。
- **论文 PDF**: [CELLSHIFT: RTT-Aware Trace Transduction for Real-World Website Fingerprinting.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1004-paper.pdf)

#### CoLD: Collaborative Label Denoising Framework for Network Intrusion Detection.

- **作者**: Shuo Yang, Xinran Zheng, Jinze Li, Jinfeng Xu, Edith C. H. Ngai
- **方向**: 网络与协议安全
- **解读**: 
标签噪声在网络入侵检测中导致错误分类与精度下降。现有方法缺乏对网络流量的深入理解，盲目重构标签分布或依赖距离度量，在"局部一致性"（不同类别特征分布相似）下难以区分干净与噪声样本。本文从因果关联视角揭示噪声标签的损害机制并提出针对性去噪框架。

方法上用结构因果模型分析：噪声标签激活伪因果捷径，使非因果特征与噪声标签建立关联；KS检验证实局部一致性普遍存在。CoLD三部分：(1)特征重排——按Pearson相关构造最大生成树重排特征；(2)局部联合学习——特征划分为多个子集，以掩码特征混淆，通过局部对齐与全局重构损失自监督训练，学习标签无关的鲁棒表示；(3)因果协作去噪——用GMM对多视图表示与潜在标签建模，以因果分歧度（CDM）量化噪声转移概率，阈值ϵ=0过滤噪声。

实验在MALTLS-22与CICIDS-2017上，噪声比例{10%–80%}。CoLD在0%噪声下MALTLS-22 F1=92.97%；60%对称噪声仅比20%降3.4%，CICIDS仅降1.6%；80%噪声下仍达87.04与95.16。基线崩塌：CLEID在20%噪声降至60.61，Co-Teaching+从>99%跌至<10%；对MCRe的胜出经配对t检验（p<0.05）。

我的思考：贡献在于首次把标签噪声损害归结为局部一致性驱动的伪因果关联并设计针对性对策；以因果分歧度而非距离识别噪声，高噪声下更稳健且无需预设噪声比例。局限：GMM组件数等于类别数的假设较强，评估仅两个数据集。影响：对特征重叠的流量分类领域，因果视角去噪比图像领域迁移方法更适配。

- **论文 PDF**: [CoLD: Collaborative Label Denoising Framework for Network Intrusion Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1950-paper.pdf)

#### Continuous User Behavior Monitoring using DNS Cache Timing Attacks.

- **作者**: Hannes Weissteiner, Roland Czerny, Simone Franza, Stefan Gast, Johanna Ullrich, Daniel Gruss
- **方向**: 网络与协议安全
- **解读**: 
DNS缓存计时侧信道可推断用户访问过的域名，但此前工作缺驱逐能力、或针对路由器缓存，且都要求攻击者能执行代码。本文提出DMT，针对本地系统级DNS缓存（systemd-resolved）的Evict+Reload攻击，可在非特权、沙箱、跨VM乃至纯网页环境中持续监控上网行为。

方法上，DMT的关键是可靠驱逐：基于resolved行为的错误响应驱逐（最快77.267ms）、并行DNS请求灌满缓存（100线程5.109s）、以及浏览器场景（fetch/CORS预检与CSS字体串行加载）的驱逐；配套3种测量原语：native（gethostbyname计时）、JavaScript（fetch+performance.now）、无脚本HTML（用前后请求时间差推断解析延迟）。攻击以20秒时隙每分钟轮询3次，由计时差判定访问。

结果：驱逐平均77.267ms；监控100域名跨VM仅685.86ms、JS最差14.710s，5分钟粒度盲区最优<0.26%、最差4.92%。跨VM端到端实验（开放世界103网站）以92.48%的F1在685.855ms内检测出主机访问；JS攻击10网站F1为82.86%（DNSSEC）与78.89%（无）。native下命中1.6ms对未命中8ms，DNSSEC使差异扩大至约20ms、最差假阴性率从24.2%降至13.6%；DoT无影响、VPN下仍有效、无JS可靠性达87.5%。

我的思考：DMT系统解决本地DNS缓存驱逐瓶颈，把攻击推进到持续监控加精确时间戳，覆盖VM、浏览器、无JS等受限环境并量化盲区；非内容型、不受网站内容变化影响。局限：针对systemd-resolved（部分发行版默认无系统级缓存），时间粒度受网络延迟与5秒超时分支制约，JS场景受服务器响应噪声干扰。影响：与路由器缓存攻击正交互补，说明VPN、DoT/DoH均无法保护本地缓存侧信道，为缓存隔离与随机化缓解提供压力。
- **论文 PDF**: [Continuous User Behavior Monitoring using DNS Cache Timing Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2287-paper.pdf)

#### CoordMail: Exploiting SMTP Timeout and Command Interaction to Coordinate Email Middleware for Convergence Amplification Attack.

- **作者**: Ruixuan Li; Chaoyi Lu; Baojun Liu; Yanzhong Lin; Qingfeng Pan; Jun Shao
- **方向**: 网络与协议安全
- **解读**: 问题：传统邮件 DoS（EmailBomb）以垃圾邮件轰炸用户邮箱，几乎无法影响邮件服务器的运行；随着邮件生态高度集中化，针对入站邮件服务器的带宽耗尽与任务队列拥塞会引发业务中断与财务损失，而此前 SMTP 一直未被纳入反射放大攻击的载体研究。

方法：作者利用 SMTP 的两个固有属性——长会话超时（容忍大文本可靠传输）与客户端控制交互（命令序列驱动状态机）——提出时间汇聚型脉冲 DoS 攻击 CoordMail：攻击者以极低速率与大量邮件中间件建立并维持 SMTP 会话（用命令+休眠间隔刷新状态超时），在攻击时刻按 Mquit=Matta−Drefl−Dprod−Dorig 精确发送 QUIT，使退信服务器、开放中继、转发服务商反射的邮件同时到达受害入站服务器，把各中间件的放大能力汇聚叠加。

结果：实测识别出 10,079 个退信服务器、584 个开放中继和 6 家转发服务商满足适用条件（会话超时超 10 分钟、反射间隔标准差低于 500 微秒）；真实世界用 20 个连接、140 秒周期实现 3,801× 带宽汇聚，受控环境用 1,000 个连接、100 秒周期产生 2.5 Gb/s 突发、汇聚效率 33,145×（传统 backscatter 仅 48×）；77.89% 退信服务器的反射邮件能过 SPF/DKIM，Spamhaus 仅收录 5.3%，而 DMARC 可拦截约 95%。

我的思考：该工作首次把 SMTP 作为 PDoS 载体，无需 IP 欺骗且汇聚精度优于依赖路径时延的 CDN-Convex/DNSBomb，测量与工程细节扎实；其"中间件放大能力汇聚"与"随机延迟可降低约 15 倍放大能力"的缓解建议直接可落地，且已负责任披露给 91 家中间件与 14 家邮件服务商；局限是攻击效果依赖可用中间件数量与汇聚时间，且 DMARC 低部署率是问题根源——防御的关键不在新机制而在推动 DMARC 强制部署。

- **论文 PDF**: [CoordMail: Exploiting SMTP Timeout and Command Interaction to Coordinate Email Middleware for Convergence Amplification Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1414-paper.pdf)

#### Crack in the Armor: Underlying Infrastructure Threats to RPKI Publication Point Reachability.

- **作者**: Yunhao Liu; Jessie Hui Wang; Yuedong Xu; Zongpeng Li; Yangyang Wang; Jilong Wang
- **方向**: 网络与协议安全
- **解读**: 问题：RPKI 防御 BGP 前缀劫持的有效性不仅取决于 ROA 注册与 ROV 部署，还取决于依赖方（RP）能否成功从发布点（PP）取回 ROA；该取回过程包含 DNS 解析与数据下载两个子过程，其数据完整性与端到端连通性依赖 DNSSEC 与路由安全（ROA/ROV）等底层基础设施，而此前研究只关注部署率，未系统评估这些底层威胁。

方法：基于 RIPE NCC 2025 年 6 月快照提取 64 个 PP 域名，从根区逐级构造 DNS 解析路径并逐区检测 DNSSEC；用全球约 15,000 个响应解析器（覆盖 153 个国家）查询 660 个名字服务器与 64 个 PP 的 IP，结合 RouteViews BGP 表验证 ROA 覆盖、RoVista 评估 ROV 部署；用 BGPy 模拟前缀劫持，并构建 PP 间确定性（证书链 SIA）与概率性（ROA 覆盖）依赖图分析级联风险。

结果：31 个 PP（48.4%）易受 DNS 欺骗（7 个 PP 可控区、22 个第三方托管区、2 个 CNAME 导致）；55 个 PP（85.9%）解析路径上至少一个名字服务器无 ROA 覆盖，13 个 gTLD 根服务器仅 3 个（e、i、m）注册 ROA 且只注册 /32 而实际通告 /24 导致防护失效，贡献了 44 个脆弱 PP；5 个 PP 服务器 IP 无 ROA；最脆弱 PP 遭劫持时 65%–83% 的 AS 失去连通；无 rsync 回退时 DNS 欺骗可使 49.3% 的 IPv4 ROA 保护空间失效；RIR 运营的 PP 依赖 TWNIC/IDNIC/AWS 等较弱 PP，攻击可使受影响 PP 从 39–42 个、ROA 空间缩减从 14.58%/6.62% 扩大至 50.89%/42.92%。

我的思考：这是首次系统测量 RP–PP 全链路的底层基础设施安全并揭示跨 PP 依赖引发的级联失效，填补了"ROA 存在但取不到"的盲区；对 Verisign 的披露促使 gTLD 名字服务器补齐 /24 ROA，体现了测量的现实影响力；局限在于测量为单时点快照、ROV 覆盖以上下界估计，且 CDN 部署（如 RIPE 经 Akamai 的区域性 IP 缺 ROA）暴露出云化发布基础设施带来的新运维难题，值得后续持续追踪。

- **论文 PDF**: [Crack in the Armor: Underlying Infrastructure Threats to RPKI Publication Point Reachability.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1141-paper.pdf)

#### CTng: Secure Certificate and Revocation Transparency.

- **作者**: Jie Kong, Damon James, Hemi Leibowitz, Ewa Syta, Amir Herzberg
- **方向**: 网络与协议安全
- **解读**: 证书透明（CT）虽已部署，但标准假设日志服务器（logger）诚实，且浏览器撤销机制（CRL/OCSP/CRLSet）覆盖不全，可引发僵尸证书、撤销 DoS 等攻击；Web-PKI 缺少无单点信任（NTTP）的透明性与撤销保证。

方法上，CTng 沿用 CT 实体并赋予 monitor 主动角色：通过周期性一致广播（PCB）协议交换并核验日志视图，以阈值签名向依赖方分发透明性与撤销更新；证书内嵌撤销号（RN），配合压缩撤销向量（CRV）实现可预取、无需实时查询的高效离线验证。

评估显示，仅用基础版加广播优化的 CTng 在普通硬件上即超过当前全球规模 precertificate 吞吐量，支持 32 个 monitor 并容忍最多 f=8 个恶意 monitor；EEA 纠删编码优化进一步提升高 f 场景性能，且增加 monitor 数对吞吐量影响可忽略。

我认为该工作最大的成色在于"演进式"设计：保留 X.509 与 CT 生态，改动集中在最易改的 monitor，部署阻力小；但安全性依赖 monitor 网络的 (f+1) 连通性与松散同步时钟假设，撤销传播延迟受 MMD 限制，且尚未经真实浏览器生态验证，落地仍有不确定性。

- **论文 PDF**: [CTng: Secure Certificate and Revocation Transparency.](https://www.ndss-symposium.org/wp-content/uploads/2026-s213-paper.pdf)

#### Demystifying RPKI-Invalid Prefixes: Hidden Causes and Security Risks.

- **作者**: Weitong Li, Tao Wan, Tijay Chung
- **方向**: 网络与协议安全
- **解读**: RPKI 已覆盖超 51.3% 的路由，但仍有 6,802 个 RPKI-invalid 前缀持续存在且多年不降；先前方法只能解释其中 79% 的误配置，其余在自动化工具看来与劫持无异，干扰安全判断。

作者基于 18 个月（2023.1–2024.7）的 BGP 与 ROA 数据复现前人方法（合计解释 83.1%），对剩余 7,209 个前缀按四种"隐藏"场景分类：组织内多 ASN 传输（9.5%）、不透明 IP transit（30.1%）、经纪租赁（18.9%）、直接租赁（16.6%）及组合场景（6.3%），采用 ≥5 客户阈值加 WHOIS/IRR/as2org 关联推断，总体把误配置归因率提升到 96.9%。

数据面测量显示 3.1% 的前缀完全断连、18.5% 出现路径分叉（其中 39.2% 延迟超 100ms、54.7% 多出 5 跳以上），且租赁类误配置恢复更慢；劫持检测系统层面，6,114 个被报为劫持的 RPKI-invalid 前缀中 82.0% 实为隐藏误配置，排除 ROA 因素后 78.1% 降至警报阈值之下；174 名运营商确认了 294 个误配置前缀。

这是首个系统性揭示"隐藏误配置"的研究，对 ROV 过滤与劫持检测系统有直接指导价值；但推断依赖启发式与 as2org 映射（合并/并购场景有误差），且与 16 家 ISP/经纪商的访谈结论偏定性，落地建议（自动化 ROA 监控、跨部门流程）仍需行业配合推进。

- **论文 PDF**: [Demystifying RPKI-Invalid Prefixes: Hidden Causes and Security Risks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s161-paper.pdf)

#### Enhancing Website Fingerprinting Attacks against Traffic Drift.
- **作者**: Xinhao Deng, Yixiang Zhang, Qi Li, Zhuotao Liu, Yabo Wang, Ke Xu
- **方向**: 网络与协议安全
- **解读**: 
深度学习的网站指纹（WF）攻击在受控条件下精度很高，但流量漂移——网站内容更新、Tor 版本升级、网络条件变化、用户浏览行为改变等——使训练与部署分布产生偏差，攻击性能大幅下降，成为 WF 攻击实战化的主要障碍。传统对策需要周期性采集大量带标签流量重训练，成本高且无法及时适应未知漂移。

作者提出 Proteus，首个自适应 WF 攻击框架：部署期间仅用客户端到 Tor 入口节点间采集的无标签流量持续微调模型。具体三步：用高斯核函数把流量特征映射到高维线性可分空间并最小化最大均值差异（MMD）对齐原始与漂移特征分布；优化模型输出的熵分布以增强预测置信度；再用高斯混合模型（GMM）挑选可靠预测作为伪标签进行监督微调。Proteus 可无缝集成现有 DL WF 攻击。

在超过 35 万条真实 Tor 流量、六种漂移场景（2024 年 3 月至 12 月采集）下，Proteus 对 8 个 SOTA WF 攻击实现平均 94.24% 的相对 F1 提升；270 天时间漂移下 F1 达 82.27%，而八个基线全部低于 60%；时间漂移场景平均 F1 提升 33.65%，RF 攻击在网络条件漂移（UK）下提升达 386.77%。

把无监督域自适应引入攻击侧并做到"近实时、免标注"是亮点，真实大规模数据支撑的结果很有说服力。局限在于攻击者需能在客户端与入口节点间部署流量观测、伪标签质量依赖 GMM 的可靠性，且 WF 防御（如 WTF-PAD）下增益虽有但显著下降。这项工作提醒：匿名通信的防御者需要正视自适应攻击这一新维度。

- **论文 PDF**: [Enhancing Website Fingerprinting Attacks against Traffic Drift.](https://www.ndss-symposium.org/wp-content/uploads/2026-s59-paper.pdf)

#### Entente: Cross-silo Intrusion Detection on Network Log Graphs with Federated Learning.
- **作者**: Jiacen Xu, Chenang Li, Yu Zheng, Zhou Li
- **方向**: 网络与协议安全
- **解读**: 
图网络入侵检测系统（GNIDS）在检测 APT 等复杂攻击上效果显著，但现有 GNIDS 都假设集中式数据；而 GDPR、新加坡 PDPA 等隐私法规与数据驻留要求使跨区域日志聚合不可行（如 Palo Alto、Microsoft Defender XDR 均无法跨区汇聚）。朴素联邦学习（FL）应用于 GNIDS 又面临图异构、非 IID、类别极不平衡等挑战。

作者提出 Entente，一个面向 GNIDS 的联邦图学习协议：先用参考图合成与图素描的引导阶段，仅共享组织内本就可见的聚合节点数来初始化各客户端权重，缓解非 IID；再用自适应贡献缩放（ACS）让客户端按与全局模型的差异自调节贡献，配合动态裁剪同时实现鲁棒性，并理论上证明了 ACS 下迭代差移位有界、收敛率有界。系统适配 Euler 与 Jbeil 两种典型 GNIDS。

在 OpTC、LANL、Pivoting 数据集上，Entente 的 AP 达 74%–84%、AUC 超过 99%，甚至优于非联邦的全量集中训练（K=3 时 AP 83.29% 对 Non-FL 的 69.96%）；Jbeil 的链接预测在多数场景 AP/AUC 超过 0.9。针对 GNIDS 场景设计的投毒攻击下，LANL 上恶意边逃逸率（SR）被限制在 9.30%，而关闭范数约束时出现梯度爆炸（NaN）。

首次系统化地把 FL 引入 GNIDS 并同时满足有效性、可扩展性与鲁棒性三个目标，且"只用聚合节点数"这类零额外隐私泄露的信息做引导，工程与理论（收敛性证明）兼得。局限在于作者自述 LANL 检测性能"远非理想"，且客户端数受聚类方法限制（2–5 个）；对受法规约束的跨辖区 SIEM 部署具有直接价值。

- **论文 PDF**: [Entente: Cross-silo Intrusion Detection on Network Log Graphs with Federated Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s93-paper.pdf)

#### Hey there! You are using WhatsApp: Enumerating Three Billion Accounts for Security and Privacy.
- **作者**: Gabriel K. Gegenhuber, Philipp É. Frenzel, Maximilian Günther, Johanna Ullrich, Aljosha Judmayer
- **方向**: 网络与协议安全
- **解读**: **问题**：WhatsApp 联系发现机制允许任何用户查询号码是否注册，天然支持号码枚举；2012 年与 2021 年的研究规模有限（最多 5,050 万号码），需重估限速防御下大规模枚举是否仍可行，以及泄露数据的长期有效性。

**方法**：与以往自动化 App UI 不同，本工作基于逆向工程的开源客户端 whatsmeow 直连 WhatsApp 的 XMPP API，可获取注册状态、设备列表、个人资料、头像 URL 及 X3DH 预密钥包；自研 libphonegen 基于 libphonenumber 为 245 个国家生成 646 亿候选号码，经后处理压缩至 631.7 亿；以单服务器、5 个并发会话、每秒约 7,000 次查询、5 万批量查询进行 2024 年 12 月至 2025 年 4 月的多轮全量测量。

**结果**：未遭遇任何限速或封禁，共发现 3,546,479,731 个活跃账户（3.5B），相当于史上最大规模泄露；2021 年 Facebook 泄露的 4.88 亿号码中 58%（2.81 亿）在六年后仍活跃。普查显示印度 7.49 亿账户占 21.67%、Android 占 81%、57% 用户有公开头像、伊朗禁令期间仍有约 5,900 万账户（占人口三分之二）；还发现 230 万 X25519 公钥在 290 万+设备间复用（最高达 131,406 次），并找到全零私钥，暗示损坏的随机数生成器。

**我的思考**：以直接 API 访问突破 UI 瓶颈实现数量级提速是核心贡献，3.5B 记录与"无有效限速"的结论极具冲击力；伦理处理规范（只做聚合普查、不分析个体）。局限是测量窗口内平台未改进防御、单点测量可能低估检测难度；密钥复用与账户关联的根因分析仍较初步，但对被禁国家用户和垃圾/钓鱼风险的警示意义重大，应推动平台落实强限速与防枚举机制。

- **论文 PDF**: [Hey there! You are using WhatsApp: Enumerating Three Billion Accounts for Security and Privacy.](https://www.ndss-symposium.org/wp-content/uploads/2026-s805-paper.pdf)

#### HoneySat: A Network-based Satellite Honeypot Framework.
- **作者**: Efrén López-Morales, Ulysse Planta, Gabriele Marra, Carlos Gonzalez-Cortes, Jacob Hopkins, Majid Garoosi, Elías Obreque, Carlos E. Rubio-Medrano, Ali Abbasi
- **方向**: 网络与协议安全
- **解读**: **问题**：卫星支撑 GPS 等关键服务，但长期依赖"架构难解＋安全通过隐蔽"的假设；随着发射激增（2023 年 197 次发射）与地面站技术廉价化，攻击面扩大，而社区缺乏卫星对抗行为（TTP）数据，难以发展针对性防御。

**方法**：HoneySat 是首个高交互卫星蜜罐框架，由地面段与空间段仿真构成：地面段暴露 VNC、Telnet、Web 与原始 CSP 包（ZeroMQ）四种入口，集成 SUCHAI MCS、Gpredict，并以 Radio Simulator 按过境时间与仰角模拟通信可用性与丢包；空间段运行真实飞行软件 SUCHAI FS，配合自研 Satellite Simulator（轨道、旋转、电源、热、磁与 RGB 相机六种物理仿真、11 类传感器、4 个子系统）模拟卫星物理过程增强真实性。

**结果**：支持 SPACE-SHIELD 矩阵全部 14 个战术（100%）与虚拟蜜罐可适用的 38 个技术中的 33 个（87%）；10 名经验丰富的卫星运营商测评中 90% 认为仿真真实、温度遥测获 100% 强烈认同、70% 认为无法与真实任务区分；互联网部署 5 个实例（SUCHAI-2、2×ACS3、2×PIXL-1）成功欺骗真实攻击者，捕获 4 次 Telnet 会话、22 条飞行软件专用命令，覆盖 6 种 SPACE-SHIELD 技术；另完成 CCSDS 生态扩展（一名研究生两周完成）并与在轨小卫星完成硬件在环通信。

**我的思考**：填补卫星蜜罐空白，捕获真实对抗命令的数据极其稀缺（对比约 4,700 台 ICS 设备才对应 1 颗卫星），是首个经验性卫星 TTP 基线；局限是样本量小、多数互联网实例数月无人问津、射频层被排除在外，且运营商调查仅 10 人；飞行软件真实执行是亮点，但被攻破后的隔离与溯源讨论不足。

- **论文 PDF**: [HoneySat: A Network-based Satellite Honeypot Framework.](https://www.ndss-symposium.org/wp-content/uploads/2026-f537-paper.pdf)

#### Huma: Censorship Circumvention via Web Protocol Tunneling with Deferred Traffic Replacement.
- **作者**: Sina Kamali, Diogo Barradas
- **方向**: 网络与协议安全
- **解读**: **问题**：Web 协议隧道（WebTunnel/HTTPT/Balboa）把隐蔽数据封装进正常 Web 流量以对抗审查，但 Balboa 式的即时内容替换会引入时序异常，可被流量指纹以约 90% 精度识别；现有工具也普遍缺乏行为真实性，且不提供针对 Sybil 代理的隐私防护。

**方法**：Huma 采用延迟流量替换（Deferred Traffic Replacement）：参与网站对请求立即返回未修改的合法内容，隐蔽数据在后台准备，待客户端下一次请求时再交付（DRR 双请求接收协议），从根上消除响应时序异常；以 Overt User Simulator（基于 2,148 名德国用户 910 万次 URL 访问数据训练的 TVAE 模型）模拟真实浏览行为；SP 将响应加密并切分为统一大小数据块，使恶意网站无法据内容或块数指纹化；架构上以不可信的 Decoy Website 与可信的 Shade Proxy 职责分离抵抗 Sybil 代理，并扩展出基于 Pung PIR 的内网隐蔽通信模式。

**结果**：内容指纹实验中，无分块时分类器可 91% 识别目标网站，采用 2MB 分块后降至 12% 且带宽开销仅 21.3%（甜点）；流量指纹实验中 XGBoost 分类精度最高仅 54%（接近随机），KS 检验 D 值 0.03–0.06（Waterfall of Liberty 为 0.11）；行为真实性评估中分类器 AUC 0.87，但达到 0.9 以上 TPR 需容忍 0.3 以上 FPR；网络开销为延迟 105%–712%、带宽 13%–607%（取决于网页大小组合），原型约 4,200 行 Python。

**我的思考**：延迟替换优雅修正了 Balboa 的时序缺陷，双代理分离使 Sybil 防护内建于系统而非依赖 Tor，设计自洽；局限是行为模型仍可被 AUC 0.87 的分类器部分区分、延迟开销较高限制交互式应用，且 OUS 使吞吐受真实浏览节奏约束；与同类工具的系统化对比值得借鉴。

- **论文 PDF**: [Huma: Censorship Circumvention via Web Protocol Tunneling with Deferred Traffic Replacement.](https://www.ndss-symposium.org/wp-content/uploads/2026-f328-paper.pdf)

#### Icarus: Achieving Performant Asynchronous BFT with Only Optimistic Paths.
- **作者**: Xiaohai Dai, Yiming Yu, Sisi Duan, Rui Hao, Jiang Xiao, Hai Jin
- **方向**: 网络与协议安全
- **解读**: 

异步拜占庭容错（BFT）共识对网络攻击最鲁棒，但性能远低于部分同步协议。近年双路径范式用乐观路径提效、用悲观路径（通常为 MVBA）保活性，却带来实现复杂、不利情形性能差的问题；串行型在路径切换上迟钝，并行型则丢弃一条路径的块、浪费带宽并拉低吞吐。

Icarus 是仅用乐观路径的单路径异步 BFT 协议：每个节点并行广播自己的两条相位 HotStuff 链（去掉视图切换），链按轮转调度轮流充当乐观路径，只要某链积累足够未提交块即可提交，从而保证活性；新增 tcv2-BA（两连续值拜占庭协议）在路径切换时对齐前一路径已提交块高度以保安全；块间引用（链内+链间）与自适应切换提升吞吐。

实验中 Icarus 峰值吞吐达 237.3K TPS，优于 Ditto（125.5K）、ParBFT（98.6K）、FIN（76.5K）、sMVBA（68.2K）和 Tusk（206.5K）；不利情形下吞吐约为 Tusk 的 1.2 倍、Ditto 的 30.4 倍，且延迟低于 Ditto 与 ParBFT；高度对齐协议耗时约占总延迟的 40%–60%，且占比随节点数增加。

Icarus 把"移除悲观路径"做到极致，比 DAG 类协议更灵活（不强制引用 n−f 个前块），工程实现也明显简化。但高度对齐依赖阈值签名的 tcv-BA，随规模增长开销显著，是主要瓶颈；论文侧重吞吐延迟测量，对拜占庭行为下的安全论证仍以理论分析为主，期待更多故障注入验证。

- **论文 PDF**: [Icarus: Achieving Performant Asynchronous BFT with Only Optimistic Paths.](https://www.ndss-symposium.org/wp-content/uploads/2026-f60-paper.pdf)

#### Indicator of Benignity: An Industry View of False Positive in Malicious Domain Detection and its Mitigation.
- **作者**: Daiping Liu, Danyu Sun, Zhenhua Chen, Shu Wang, Zhou Li
- **方向**: 网络与协议安全
- **解读**: 

恶意域名检测系统在生产中的误报（FP）规模长期未知，而"怕误报"正是这类系统落地的主要顾虑；学界与工业界都只关注危害指标（IOC），轻视了良性指标（IOB）。作者利用安全厂商 SV（Palo Alto Networks）的 6 年用户举报数据，首次对 FP 做大规模测量：检测器日均从约 70 亿次 DNS 查询中发现 160 万新恶意域名，服务 6.5 万+ 组织。

测量发现：2019–2024 年共 123,491 条 FP 举报（121,073 条被接受、对应 118,093 个唯一 FQDN）；半数 FP 在域名被判恶意后 120 天内被举报；FP 呈长尾分布，97.7% 的 FQDN 仅被一名用户举报一次；流行的流行度列表（如 Tranco）最多只能覆盖约 38% 的 FP；约 55% 的 FP 能在互联网上找到其良性指标。基于此提出传递信任模型：域名被可信来源（唯一信任根为政府机构）认证或归可信所有者即视为良性，并用 LLM+链式思维实现 IOBHunter 迭代"搜索-核查"。

以已核验 FP 为基准，IOBHunter 达 99.22% 精度与 68.6% 召回（GPT-4o）；两个月真实部署中，从 1500 万检测域名里为 6,571 个找到可信 IOB，其中 4,338 个被确认误报并翻转裁决、2,051 个判定为受损域名，仅 182 例 IOB 错误（主要源于 LLM 幻觉）。

这是首个工业级 FP 测量+自动化 IOB 收集工作，传递信任与 LLM 结合有新意且落地证据扎实。局限：召回偏低（大量 IOB 来自易被操纵的社交媒体）、信任根仅限政府机构可能漏检、数据集因合规仅能部分公开，可复现性受限。

- **论文 PDF**: [Indicator of Benignity: An Industry View of False Positive in Malicious Domain Detection and its Mitigation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1869-paper.pdf)

#### Ipotane: Balancing the Good and Bad Cases of Asynchronous BFT.
- **作者**: Xiaohai Dai, Chaozheng Ding, Hai Jin, Julian Loss, Ling Ren
- **方向**: 网络与协议安全
- **解读**: 

异步 BFT 的目标是在有利情形匹配部分同步协议、在不利情形匹配纯异步协议的性能。现有双路径工作各有短板：Abraxas（CCS'23）吞吐稳定但乐观路径失败检测慢，不利情形最坏延迟极高（经验上检测需 80 多轮）；ParBFT（CCS'23）延迟好，但因每高度多跑额外 ABA 实例而出现空转、吞吐下降。

Ipotane 并行运行两条路径：乐观路径用 2-chain HotStuff，悲观路径用新原语 DBA（双功能拜占庭协议），把偏置 ABA 与 VABA 打包进一个协议。每个副本在乐观路径更快时向 DBA 输入 0、悲观路径更快时输入 1：ABA 功能一旦输出 1 即及时信号乐观路径失败（保证低延迟），VABA 功能持续产出悲观块，检测到失败时立即提交最近两个悲观块（维持吞吐），偏置有效性保证提交安全。

理论分析：有利情形 5δ 延迟、c/(2δ) 吞吐，与部分同步协议持平；不利情形 18.5δ 延迟、3c/(23δ) 吞吐，仅略逊于纯异步 2-chain VABA（10.5δ、c/(7δ)），明显优于 Abraxas 的延迟与 ParBFT 的吞吐。16 副本实验在领导者故障率 ρ=0%、10%、20%、100% 四种场景下，Ipotane 均取得（近）最优吞吐与最低延迟。

Ipotane 巧妙用一个原语同时解决"失败检测"与"悲观块产出"，避免了 ParBFT 的空转与 Abraxas 的检测延迟，理论-实验对应完整。与同组 Icarus 相比，它仍保留悲观路径，靠 DBA 复用降低开销；局限是理论不利延迟 18.5δ 仍高于纯异步协议，且实现复杂度随 DBA 而上升。

- **论文 PDF**: [Ipotane: Balancing the Good and Bad Cases of Asynchronous BFT.](https://www.ndss-symposium.org/wp-content/uploads/2026-s3-paper.pdf)

#### Janus: Enabling Expressive and Efficient ACLs in High-speed RDMA Clouds.

- **作者**: Ziteng Chen, Menghao Zhang, Jiahao Cao, Xuzheng Chen, Qiyang Peng, Shicheng Wang, Guanyu Li, Mingwei Xu
- **方向**: 网络与协议安全
- **解读**:
RDMA 云日益普及（LLM 训练、云存储、RPC），但 RDMA 独特的 QP 语义与高速传输特性使传统五元组 ACL 无法全面治理其流量：内核态 ACL（iptables/OVS）看不到旁路内核的数据面报文，微内核方案（FreeFlow/Snap）CPU 开销大，网内方案（Bedrock）需把主机内流量绕行交换机而增加时延。论文认为理想 ACL 需同时满足覆盖（QP 细粒度语义+控制/数据面全流量）、效率（线速+超低时延）与易用性。

方法上，JANUS 设计了带 QP 语义的 ACL 表达式，以"请求实体+QP 行为"标识 RDMA 连接，并提供高层策略语言表达复杂意图；执行侧利用 NVIDIA BlueField-3 DPU 位于关键路径的特性，配套缓存友好哈希表、异构 DRAM 感知调度、动态负载感知报文分发与 doorbell 批量处理，并优化控制面/数据面协调实现一致无阻塞的策略更新。

实验在 200Gbps 真实测试床上取得线速吞吐与 4.33µs p99 时延（LLM 负载），DPU 上仅约 4% 额外计算与 0.3% 内存开销；吞吐较 FreeFlow 最高提升 8.63 倍，时延较 Bedrock 最高降低 63.1%，异构 DRAM 调度最高带来 7.32 倍吞吐增益；以 17–32 行策略代码即可表达原子动词耗尽、QP 连接耗尽、伪造断开、报文注入、未授权 MR 访问等六类未授权访问。

我的思考：JANUS 首次把 QP 语义引入 ACL 表达式并以 DPU 落地，在表达力、覆盖率与性能之间取得良好平衡，开源原型便于复现，对 RDMA 云安全落地有实际价值。局限在于威胁模型对恶意租户内合法 QP 行为的界定、与租户级策略的集成以及大规模规则下的更新一致性仍待深化，规则冲突验证也依赖外部网络验证器。

- **论文 PDF**: [Janus: Enabling Expressive and Efficient ACLs in High-speed RDMA Clouds.](https://www.ndss-symposium.org/wp-content/uploads/2026-f721-paper.pdf)

#### Lightweight Internet Bandwidth Allocation and Isolation with Fractional Fair Shares.

- **作者**: Marc Wyss, Yih-Chun Hu, Vincent Lenders, Roland Meier, Adrian Perrig
- **方向**: 网络与协议安全
- **解读**: 公共互联网上的公平带宽分配难以实现：不同拥塞控制算法（CCA）并存时公平性差，而在大规模DDoS攻击下合法流量甚至会被完全饿死。在路由器上强制公平是可行思路，但现有方案要么易部署却无法提供安全的网内带宽隔离，要么隔离性强却依赖复杂假设、难以实际部署。FFS要同时弥合这两类方案之间的鸿沟。

方法上，FFS引入"逐流分数公平份额"（Fractional Fair Share）模型：数据包携带p.f与p.r两个标签，沿转发路径逐节点更新，表示该流当前对出口带宽的公平份额；节点据此概率转发（γ）实现隔离，只需单个FIFO队列、常量内存与常量时间处理。公平性由每节点公平矩阵配置，入端口标签归一化防篡改，并附带速率反馈（RI标签）扩展，使源端快速收敛到公平速率。

评估表明：15种CCA两两竞争下FFS-C85公平性达88.5%，远高于FIFO的30.7%和(H)CSFQ的72.1%；CAIDA top-2000拓扑上中位带宽分配5.97 Gbps，优于Helia的5.62与GLWP的0.64；DPDK实现达160 Gbps线速；MinLoss丢包率单流0%、双流仅0.05%–1.01%。安全证明保证每条流有非零带宽下界，即使配合源地址伪造的DDoS也无法阻断合法通信。

FFS是首个同时满足低实现/部署开销与有效隔离的系统，其"不区分恶意流量、只保证每流公平份额"的哲学对脉冲波攻击、僵尸网络等都有形式化保障。局限在于：与所有网内方案一样假设路径上节点诚实，且全局部署才能保证CCA无关；对短暂高频脉冲攻击的防御依赖τ估计的收敛速度。总体而言，它把CSFQ式的概率转发推进为可证明安全的层级公平模型，是网内公平性研究的重要一步。

- **论文 PDF**: [Lightweight Internet Bandwidth Allocation and Isolation with Fractional Fair Shares.](https://www.ndss-symposium.org/wp-content/uploads/2026-f23-paper.pdf)

#### Limitless Scalability: A High-Throughput and Replica-Agnostic BFT Consensus.

- **作者**: Chenyu Zhang, Xiulong Liu, Hao Xu, Haochen Ren, Muhammad Shahzad, Guyue Liu, Keqiu Li
- **方向**: 网络与协议安全
- **解读**: 传统基于领导者的BFT共识采用星型拓扑，leader承担绝大部分通信负载，副本数增长时性能急剧下降，成为区块链等场景的规模化瓶颈。近年多采用多层（树状）拓扑降低fanout，但要么需要多项式级fanout才能维持N/3拜占庭容错，要么吞吐受拓扑深度拖累，收效有限。Tide的目标是让性能与副本数量基本无关。

Tide设计了两个核心机制：一是对数fanout的Tide Graph拓扑——分层指数增长（κ=2）且每副本向上连接ρ个冗余前驱，形式上证明当ρ=log₃N+c(ε)时可用性可达星型拓扑的(N−f)/N·(1−ε)水平，同时容忍N/3拜占庭故障；二是QC驱动的流水线——副本只维护latestQC/lockedQC/committedQC三个状态变量，配合"被动级联"工作流，让提议并行度由层间消息交互动态决定，吞吐不再受拓扑深度限制。

在100台云服务器上，副本数从100增至1000时，HotStuff吞吐从12.8k降至0.7k tps、Kauri从20k降至2k、Crackle从28k降至10k（下降65%–90%，延迟最高增加50倍）；Tide则保持约50ktps（波动小于10%），延迟仅从0.3s升至0.4s，吞吐是其他方案的5倍以上。消融实验显示缺少并行流水线的Tide-NP性能骤降，证明低fanout单独不足以解决问题。

Tide首次实现了"副本无关"的BFT共识，冗余连接与QC驱动协议的组合设计精巧，且保持响应性（消息驱动而非定时等待）。局限在于：实验默认f=0，实际拜占庭场景下的view change与拓扑可用性开销仍待充分刻画；日志复杂度为O(NlogN)，且对网络时延与BLS聚合签名的依赖在广域网中的表现需要更多验证。总体是与Kauri、Crackle一脉相承的规模化共识路线的显著推进。

- **论文 PDF**: [Limitless Scalability: A High-Throughput and Replica-Agnostic BFT Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-f101-paper.pdf)

#### Mirage: Private, Mobility-based Routing for Censorship Evasion.
- **作者**: Zachary Ratliff, RuoxingYang, Avery Bai, Harel Berger, Micah Sherr, James Mickens
- **方向**: 网络与协议安全
- **解读**: 问题：Tor 等匿名网络依赖中心化互联网基础设施，易遭国家级封锁；基于人-人短距链路的机会路由抗审查，但利用移动模式优化转发会泄露用户常去地点，使敏感群体成员暴露。

方法：MIRAGE 按区（district）路由：地图分区，消息以目标区寻址；用户维护带本地差分隐私扰动的私有移动图（随机增删边、合理否认），仅当图中存在到目标区的边才接收转发；LDP 移动图聚合出全局移动图导出 gossip 参数，在 Cadence 模拟器实现。

结果：YJMob100K（行人）与 T-Drive（出租车）轨迹上，p=0.6 时交付率 36.9%，是概率洪泛（9.1%）的 4.1x、handoff（3.2%）的 11.5x，负载远低于最大洪泛，比随机游走多交付约 15x；基线 PPBR 在 T-Drive 上退化为洪泛且存在统计披露攻击；隐私参数 ε=k(k−1)·ln(p/(1−p))。

我的思考：形式化定义 ε-差分隐私的路由决策函数，首次揭示 PPBR 的统计披露漏洞，真实轨迹评估可信。局限：交付率绝对值不高，全局移动图依赖自愿上报有参与偏差；不防流量分析，定位为路由层方案。

- **论文 PDF**: [Mirage: Private, Mobility-based Routing for Censorship Evasion.](https://www.ndss-symposium.org/wp-content/uploads/2026-s237-paper.pdf)

#### NetRadar: Enabling Robust Carpet Bombing DDoS Detection.
- **作者**: Junchen Pan, Lei Zhang, Xiaoyong Si, Jie Zhang, Xinggong Zhang, Yong Cui
- **方向**: 网络与协议安全
- **解读**: 问题：地毯式轰炸 DDoS 同时淹没受害网络内大量服务器，压低每流恶意流量以逃避检测；低速率应用层变体在语义与流量上都与正常流量无异，现有方案无法实时定位受害服务器，攻击频率年增超 30%。

方法：NetRadar 采用服务器-网关协同架构：网关集中收集流量特征，服务器上报通用应用层特征（如活跃资源大小），跨服务器分析利用同源恶意流相似性；随机擦除鲁棒训练应对流量清洗导致的特征失配；置换等变模型+排序分组函数把多服务器特征视为无序集合，高效推理（默认 256 服务器子网）。

结果：所有场景准确率超 94%；低速率同步下载场景比 SOTA 方案 Lemon 高 40.55%；最隐蔽场景（恶意流量占比 <10%）仍达 F1 0.874、识别 85% 受害服务器；比单流基线提升 14.08%；512 受害服务器时鲁棒训练 recall 95% vs 67%。

我的思考：“服务器辅助+跨服务器相似性分析”直击地毯轰炸“分散+同源”本质，消融完整，随机擦除训练是实用洞察。局限：依赖服务器侧特征部署，假设攻击同源相似（定制攻击可削弱），评估以模拟为主。
- **论文 PDF**: [NetRadar: Enabling Robust Carpet Bombing DDoS Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2118-paper.pdf)

#### On Borrowed Time: Measurement-Informed Understanding of the NTP Pool's Robustness to Monopoly Attacks.

- **作者**: Robert Beverly, Erik C. Rye
- **方向**: 网络与协议安全
- **解读**: NTP 是互联网时钟同步的事实标准，由志愿者服务器构成、管理分散的 NTP Pool 被大量 IoT 与基础设施设备默认使用（IPv6 观测数据：一周内超 550 万 Fritz!Box 路由器、160 万 Amazon 设备访问），是极具吸引力的攻击目标：恶意服务器可加入 Pool 捕获流量、实施时间偏移或监视。此前对 Pool 的研究均依赖 DNS 间接推断，无法获知池内全量服务器、账号、评分与流量分布。

本文建立了首个直接的、非推断式的 NTP Pool 测量体系：逆向发现 NTP Pool 网站服务器 ID（单调递增整数）到 IP 的映射端点，以爬虫在 9 个月内穷举全部 15,680 台服务器（IPv4 9,955 台、IPv6 5,725 台）及其账号、评分、netspeed 元数据；用改进的 ntpdedup 指纹识别别名服务器；结合 2008 年以来的 BigQuery 历史评分分析服务器寿命，并基于 netspeed 权重数学推导垄断攻击所需服务器数 S=nf/(m(1-f))（m 为 3Gbps 上限）。

关键发现：活动服务器中仅 19.7%（6,242 台中 1,227 台）完全独立——去别名后降至 76%，按账号收敛至 56%，按网络收敛至 19.7%，单个账号最多控制 340+ 台服务器，Hetzner 一家托管约 7% 的 Pool IP；推断全球 DNS 查询率约 10.6 万次/秒，@、us、br 三区占 68%；90% 国家的流量只需 10 台或更少恶意服务器即可垄断（过半国家仅需 1 台）；服务器被移出 Pool 后仍可月余持续收到约 2500 万 IPv4 请求/天的"残余流量"。实测在匈牙利 .hu 区验证了攻击可行性。

我的思考：这是迄今对 NTP Pool 最全面直接的测量，"仅两成服务器独立"与"9 成国家可被少量服务器垄断"的结论直接量化了关键基础设施的单点故障风险，"残余流量"更揭示恶意服务器即使被驱逐仍能长期攻击受害者。局限：账号可见性仅 46%，层 1 服务器因 refid 有限被排除去别名，且无法测量自建 NTP 基础设施的厂商。

- **论文 PDF**: [On Borrowed Time: Measurement-Informed Understanding of the NTP Pool's Robustness to Monopoly Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f541-paper.pdf)

#### On the Security Risks of Memory Adaptation and Augmentation in Data-plane DoS Mitigation.

- **作者**: Hocheol Nam, Daehyun Lim, Huancheng Zhou, Guofei Gu, Min Suk Kang
- **方向**: 网络与协议安全
- **解读**: 可编程数据平面（如 Intel Tofino）让交换机在线速下自适应调整 DoS 缓解逻辑，Cerberus 等系统还把计数高位卸载到控制平面扩展内存。本文指出"灵活性"引入新攻击面：硬件约束（寄存器访问受限、流水线固定、每包单次访问）产生可观测的时序侧信道，攻击者可推断内存增强、重分配与刷新的时机并瘫痪缓解系统。

Heracles 攻击分准备与实施两阶段：准备阶段用少量探测 bot 做阈值探测（97/100 零误差）、时间窗推断（96/100、起点误差<1 秒）与共享任务推断；实施阶段并行三种争用策略——①同步增强：多任务计数器近乎同时溢出，向控制平面洪泛镜像包（380K pps 涌入而控制面仅约 15K pps，约 4%），造成漏计数；②内存挤压：抬高非目标任务内存需求，诱导自适应机制把 DNS/SYN 任务切片压缩至 5 bit 后触发 800K pps 溢出；③时间窗利用：掌握刷新时机后在窗口内保持每流低于阈值，同时用大包维持带宽。

实验（Tofino Wedge 100BF-32X）显示三种策略把 Cerberus++ 的漏报率（FNR）推高至约 78%、50% 和接近 100%，多数攻击流量可绕过缓解直达目标。对策 Shield：多层分级寄存器草图，溢出位层间传递而非直传控制面，各层采用不同衰减/重置机制打乱时序推断（时间窗推断中位误差由 0.29s 升至 4.29s），即使有完整对抗知识也 3 秒内把 FNR 降为 0%，精度与主流草图相当。

我的思考：本文把"针对防御系统的攻击"提升到资源争用层面，揭示自适应机制本身可被反制，Shield 的"去同步+分层进位"设计有效。局限：攻击需先完成参数推断、针对 Cerberus 类设计，且假设 1-2 秒松同步 botnet；Shield 以 7.7%p SRAM 与 25%p SALU 开销换鲁棒性。防御的"自适应"若不考虑对抗性时序，反而成为新弱点。

- **论文 PDF**: [On the Security Risks of Memory Adaptation and Augmentation in Data-plane DoS Mitigation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1857-paper.pdf)

#### OSAVRoute: Advancing Outbound Source Address Validation Deployment Detection with Non-Cooperative Measurement.

- **作者**: Shuai Wang, Ruifeng Li, Li Chen, Dan Li, Lancheng Qin, Qian Cao
- **方向**: 网络与协议安全
- **解读**: 源 IP 伪造是反射式 DDoS 的根基，出站源地址验证（OSAV，BCP 38）是防止伪造包离网的业界最佳实践。但测量 OSAV 需从网络内部发伪造包，常要求运营商配合：CAIDA Spoofer 靠志愿者，一年仅测 1,748 个 /24 前缀、798 个 AS；现有非协作方法（基于透明转发器 TF 的 DNS 代理）只能识别"未部署"，无法判断"已部署"及阻断深度与粒度。

OSAVRoute 是首个捕获细粒度 OSAV 特征的非协作系统。核心洞察：TF（实证由 DNAT 造成，仅改目的地址与 TTL）透明转发探测包，逐 TTL 探测即可追踪路径，判断是否生成及在哪一跳被阻断。技术要点：无状态扫描（被测地址与 TTL 编码进头字段，从 ICMP 超时消息恢复）；早期过滤检测（用"源=被测地址"的伪装查询骗过首跳过滤）；阻断深度与阻断粒度（以不同前缀长度的伪造源地址实测）；四重过滤剔除干扰。

2025 年 3-4 月三轮全网扫描（4 观测点、40 万 pps）发现 165 万 TF，过滤后 44 万可用；判定 9,828 个 /24 前缀、3,310 个 AS，是 Spoofer 去年的 4.6 倍与 3.1 倍，95.3% 未被其覆盖。三重验证（Spoofer 比对、22 家运营商、ISP 协作）表明准确率 99.4%，并帮该 ISP 修正 36 个 /24 前缀的 ACL 配置错误。洞察：84.2% 被测 AS 未部署 OSAV（ISP 最差 87.5%、教育网最好 73.1%）；已部署者 95.5% 前两跳内阻断、粒度以 /22-/24 最常见；首次发现 MANRS 参与与 OSAV 部署正相关。

我的思考：该工作把非协作测量从"仅判缺失"推进到"存在/缺失/深度/粒度全特征"，方法论（TF 成因实证+无状态编码+早期过滤规避）严谨，规模与验证有说服力，并展示了测量工具转为运营修复的价值。局限：依赖被测网络存在 TF，无 TF 者不可测；非响应路由器可能低估阻断深度；粒度测量受入站 SAV 影响。与 Spoofer 的反差提醒：志愿者采样有系统性偏差，SAV 真实部署或更严峻。
- **论文 PDF**: [OSAVRoute: Advancing Outbound Source Address Validation Deployment Detection with Non-Cooperative Measurement.](https://www.ndss-symposium.org/wp-content/uploads/2026-s17-paper.pdf)

#### Pando: Extremely Scalable BFT Based on Committee Sampling.

- **作者**: Xin Wang, Haochen Wang, Haibin Zhang, Sisi Duan
- **方向**: 网络与协议安全
- **解读**: BFT 协议面临可扩展性困境：性能随副本数 n 增长急剧退化，现有解耦区块传输与共识排序的方案在低端机器上仅能扩展到约百副本。委员会采样思路此前只用于拜占庭协议且纯属理论——Algorand 为达 10^-9 失败率需 2000 委员会，把该思路扩展到客户端-副本模型的实用 BFT 是开放问题。

Pando 在部分同步模型下提出自适应安全的可扩展 BFT：用 VRF 实例化 ComProve/ComVerify 预言机采样 O(κ) 规模委员会，每个区块采样多个委员会，并用 Chernoff 界的新颖用法把委员会规模下界降一个数量级——达到同样 10^-9 失败率仅需 200 委员会。通信复杂度从 O(Ln²+κn⁴) 降至 O(Ln²+κ²n²)，代价是近最优弹性 f<(1/3−ϵ)n。

Amazon EC2 WAN 评估显示 Pando 可扩展至 1000 副本（m5n.2xlarge 上吞吐 62.57 ktx/s），而 Narwhal 与 Star 超 100 副本即无法运行；n=91 时 Pando(0.8) 峰值吞吐比 Star 高 28.22%、比 Pando(1) 高 81.01%；n=500 时 Pando(0.4) 仍达 158 ktx/s；与 Algorand 相比，Pando(0.4) 每秒交付 13.84 MB 交易（约为其 154 倍），本地同配置下吞吐为其 13.15 倍。

我的思考：把委员会采样从理论带入可部署工程并用概率分析将委员会规模降一个数量级，是该方向的重要突破，对分片 BFT 亦有借鉴。局限：弹性为近最优而非 1/3；安全与活性是概率性保证；QC 用签名集合实例化导致证书体积大（作者建议聚合签名）；1000 副本实验依赖更优实例。表 II/III 给出的委员会规模-失败率-ϵ 权衡曲线为部署提供了实用指导。

- **论文 PDF**: [Pando: Extremely Scalable BFT Based on Committee Sampling.](https://www.ndss-symposium.org/wp-content/uploads/2026-s273-paper.pdf)

#### PathProb: Probabilistic Inference and Path Scoring for Enhanced and Flexible BGP Route Leak Detection.

- **作者**: Yingqian Hao, Hui Zou, Lu Zhou, Yuxuan Chen, Yanbiao Li
- **方向**: 网络与协议安全
- **解读**: BGP 缺乏内在安全机制，路由泄漏是最缺乏广泛部署防御的威胁（OTC 尚处早期），而 AS 商业关系数据因敏感而无权威公开数据集。现有白盒检测依赖确定性 AS 关系推断+谷底规则，误报率高；黑盒机器学习方法不可解释、需大量高质量训练数据，部署困难。

PathProb 提出"灰盒"框架：用整数线性规划建模 Gao-Rexford 谷底规则，为每条 AS 链接推断三维概率关系分布，再沿路径传播组合得到合法性分数——为抵消长路径累积不确定性，取路径所有相邻三元组期望合法性的最小值；输出连续分数（如"85% 可能泄漏"）而非二元标签，操作者可自由调节阈值。框架无缝整合 ASPA 权威数据：有 ASPA 记录用权威值，无则回退概率推断，形成正向激励闭环。

在真实 BGP 轨迹与 Cloudflare 泄漏事件上：关系推断在权威性最高的 ASPA 验证集上超过其他方案 5.87-23.46 个百分点；泄漏检测召回率 98.45%、误报率仅 4.17%（比其他方案低 4.29-20.08 个百分点）；阈值 0.35 时 precision 98.5%、recall 96.4%；模拟显示即使 ASPA 仅 25% 部署，仍把泄漏感染率从 15.17% 降至 6.63%（降幅超 56%）。

我的思考：把确定性标签换成概率分布、二元判定换成连续分数，直击白盒方法误报高的痛点，阈值可调性与可解释性统一，与 ASPA 的渐进整合设计务实（当前 ASPA 记录仅 300 余条）。局限：ILP 推断计算成本未详细报告；CAIDA 验证集本身由 AS-Rank 式启发式生成、天然偏向该类方法；模拟假设所有 AS 遵守 Gao-Rexford 模型。整体是 BGP 安全领域少见的"概率化+可解释"路线。

- **论文 PDF**: [PathProb: Probabilistic Inference and Path Scoring for Enhanced and Flexible BGP Route Leak Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1691-paper.pdf)

#### Pruning the Tree: Rethinking RPKI Architecture from the Ground up.
- **作者**: Haya Schulmann, Niklas Vogel
- **方向**: 网络与协议安全
- **解读**: RPKI是BGP防劫持的关键安全机制，已覆盖55%以上的全球前缀，但架构大量复用传统PKI组件（X.509 EE证书、ASN.1/DER编码、XML的RRDP仓库协议），造成验证开销大、漏洞面广、扩展性差。此前研究多把问题归因于实现缺陷或资源不足，而非设计缺陷。本文首次系统分析RPKI复杂度根因，并用Routinator的perf火焰图量化：62%处理时间花在签名验证（ROA/清单各含2个签名）、13%在对象解析，即超70%的验证时间用于证书解析与签名验证。

基于"不降低安全、向后兼容、复用现有方案、显著收益"四项设计目标，提出iRPKI：删除EE证书（对象由CA直接签名，典型ROA从2130字节降至80字节）、删除ROA签名（真实性由manifest哈希保证）、合并CRL与manifest、对象与RRDP改用规范Protobuf（仿证书透明性的确定性编码）、快照按CA分组消除冗余URI；通过平行仓库（.bin文件、.iroa/.imft扩展名）实现与现有部署并存的增量迁移。

在Routinator 0.14.1中的实现使处理时间提速20倍、带宽需求降18倍、缓存内存占用降8倍；单CA场景综合改进把处理时间从0.841s降到0.058s（93%）、文件大小降91%，签名验证从20,004次降至3次。该设计还消除了导致至少10个已知RPKI软件漏洞（共18个）的缺陷类别，如EE证书解析与冗余签名相关漏洞。全量部署预估要求仓库提供6.7 Gbit/s带宽，凸显改进的紧迫性。

我的思考：这是一篇罕见地从"协议设计"层面反思基础设施安全的实证研究，把效率与安全统一起来——简化即安全。EE证书占ROA对象72%体积、"双签名+manifest哈希"功能冗余等发现很有说服力；向后兼容的平行仓库设计为标准化落地铺路，开源实现可复现。风险在于任何协议重构都可能引入新的解析面（作者承认Protobuf需持续更新补丁），且manifest成为真实性唯一锚点后其可用性要求更高；iRPKI消除了已知漏洞类别但"新设计新漏洞"的长期风险需部署验证。
- **论文 PDF**: [Pruning the Tree: Rethinking RPKI Architecture from the Ground up.](https://www.ndss-symposium.org/wp-content/uploads/2026-s823-paper.pdf)

#### Repairing Trust in Domain Name Disputes Practices: Insights from a Quarter-Century's Worth of Squabbles.
- **作者**: Boladji Vinny Adjibi, Athanasios Avgetidis, Manos Antonakakis, Alberto Dainotti, Michael D. Bailey, Fabian Monrose
- **方向**: 网络与协议安全
- **解读**: UDRP是域名抢注争议的事实标准机制，覆盖两千余个后缀，须在商标权人快速维权与注册人免受滥诉之间取得平衡。但论坛挑选（forum shopping）、专家组利益冲突、超期拖延等信任问题争论了二十余年，因缺乏大规模结构化数据而无法实证，ICANN改革停滞。本文用NLP构建迄今最全面的UDRP数据集（1999-2024年90,153份英文裁决、175,269个争议域名），对政策进行了首次系统性实证评估。

用spaCy+RoBERTa训练NER模型（229篇人工标注文档、15类实体、平均F1 79.44%）从裁决文本中抽取时间线、当事人、律师、注册商、专家组等结构化信息；把法律代表分为忠诚者、一次性、品牌切换者、机会主义者四类，用逻辑回归+Pearson相关量化"历史胜率→选所概率"识别论坛挑选；结合CZDS zone文件数据估计域名实际转移时延，并用VirusTotal标注评估恶意域名的安全影响。

2013-2020年投诉方胜率91.87%（与ICANN估计的93%一致）；26,977起争议（占委托律师争议的33.98%、全部争议的29.92%）呈论坛挑选迹象（169/291家机会主义律所的选所概率与历史胜率显著正相关，r̄=0.35）；43起案件中专家组与其律所曾代表同一投诉方（平均间隔3年）；WIPO仅11.47%/16.17%的1人/3人专家组按时任命；域名转移中位数24个工作日，5,678个恶意域名中2,751个在裁决后最长4个月仍被恶意方控制；投诉方为完全相同匹配域名花费超9,500万美元，是主动注册成本（约44.8万）的200倍以上。

我的思考：这篇论文把二十余年的政策争论变成可检验的量化证据——论坛挑选首次被测量（近三分之一），并揭示注册商执行拖延这一被忽视的安全漏洞面（恶意域名在裁决后仍存活数月，平均被5个杀毒引擎标记）。方法上NER+zone file交叉验证扎实，保守定义增强结论可信度。局限是仅分析英文裁决（遗漏9.07%，尤其ADNDRC）、部分实体NER准确率偏低（地址66.67%）、转移时延用NS变化近似。建议（随机指派提供方、三人专家组、自动化与强化监督）务实，为2026年新gTLD浪潮提供了数据支撑。
- **论文 PDF**: [Repairing Trust in Domain Name Disputes Practices: Insights from a Quarter-Century's Worth of Squabbles.](https://www.ndss-symposium.org/wp-content/uploads/2026-s174-paper.pdf)

#### Should I Trust You? Rethinking the Principle of Zone-Based Isolation DNS Bailiwick Checking.
- **作者**: Yuxiao Wu, Yunyi Zhang, Chaoyi Lu, Baojun Liu
- **方向**: 网络与协议安全
- **解读**：**问题**：DNS 缓存投毒通过向解析器注入伪造资源记录劫持域名访问，bailiwick 检查是核心防御；但第三方服务（CDN、动态 DNS 等）中域名所有权与传统"自顶向下"的区委派模型错位，使 bailiwick 检查的有效性受到根本性挑战。

**方法**：作者系统审计了 8 款主流解析器的 bailiwick 设计与实现，发现普遍存在"保守主义原则"：解析器会缓存任何满足最小约束的资源记录，而不论其与原始查询是否直接相关。基于此提出 CUCKOO DOMAIN 攻击：只需控制单个子域名，即可通过三个载荷向量攻陷其父域或兄弟域；随后对 588,624 个稳定开放解析器与主流公共 DNS 服务做大规模测量，并对受影响厂商负责任披露。

**结果**：包括 BIND9、Microsoft DNS 在内的 7 个主流解析器实现易受攻击（PowerDNS 与 Microsoft DNS 对全部三个载荷均脆弱）；测量确认 44.64%（262,779 个）的开放解析器与 21 家公共 DNS 提供商存在风险；No-IP、ClouDNS、Akamai 等 7 家提供商提供的逾百万子域名可被潜在劫持。BIND9、Unbound、PowerDNS 与 Technitium 已确认报告并分配 3 个 CVE。

**我的思考**：该工作直击"基于区的隔离"这一教条本身，证明其与现代第三方服务生态的错位，攻击"一子域沦陷、全域遭殃"的杀伤力与百万级受影响域名规模都相当可观；局限在于部分测量结论依赖响应特征推断，且缓解方案（收紧 bailiwick 语义）可能与传统托管与 CDN 架构存在兼容性代价，需要行业协调推进。

- **论文 PDF**: [Should I Trust You? Rethinking the Principle of Zone-Based Isolation DNS Bailiwick Checking.](https://www.ndss-symposium.org/wp-content/uploads/2026-f330-paper.pdf)

#### SIPConfusion: Exploiting SIP Semantic Ambiguities for Caller ID and SMS Spoofing.
- **作者**: Qi Wang, Jianjun Chen, Jingcheng Yang, Jiahe Zhang, Yaru Yang, Haixin Duan
- **方向**: 网络与协议安全
- **解读**：**问题**：SIP 支撑 VoIP、VoLTE 与 RCS（用户超十亿）等实时通信，协议虽提供认证与身份断言机制，但其文本格式与实现自由度导致语义歧义；攻击者可构造恶意消息使代理与用户代理对身份头产生不同解释，绕过认证防线实施来电显示（Caller ID）与短信伪造；既有 SIP fuzzing 只关注内存漏洞（DoS），无法发现欺骗类缺陷。

**方法**：提出 SIPCHIMERA——首个基于生成式语法树的黑盒模糊测试框架，从六个 SIP 方法中系统生成畸形/歧义样本，测试 6 个开源 SIP 服务器（Asterisk、OpenSIPS、Kamailio 等）与 9 个主流用户代理，覆盖有/无有效凭证两类攻击者；发现多类"身份头失配"攻击，如 P-Asserted-Identity 头偏好（A1）、首 From 头偏好（A3）、P-Preferred-Identity 头偏好（A4），并对真实平台做 PoC 验证与负责任披露。

**结果**：歧义型欺骗漏洞几乎影响所有服务器-客户端组合；全部被测服务器均允许持凭证攻击者伪装他人身份；半数服务器（含 Kamailio、OpenSIPS、ejabberd）未对 P-Asserted-Identity 头强制访问控制，未认证攻击者也可绕过认证；在 5 款 VoIP 设备、7 个商用 SIP 服务与 3 个运营商级 RCS SMS 平台（覆盖数十亿用户）上证实了 Caller ID 与 SMS 欺骗（含冒充 911 案例），已向 Asterisk 等项目披露并获得确认。

**我的思考**：这是首个系统化揭示"语义歧义→身份欺骗"链条的工作，表明即使采用 STIR/SHAKEN 式密码学身份验证，不同实现对身份头的解释差异仍可被利用，攻击面具有现实危害；局限是聚焦身份头层面、黑盒方法难以覆盖深层状态，根治需标准化统一各头优先级并在代理侧严格执行访问控制，而非仅打补丁。

- **论文 PDF**: [SIPConfusion: Exploiting SIP Semantic Ambiguities for Caller ID and SMS Spoofing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s116-paper.pdf)

#### Strategic Games and Zero Shot Attacks on Heavy-Hitter Network Flow Monitoring.
- **作者**: Francesco Da Dalt, Adrian Perrig
- **方向**: 网络与协议安全
- **解读**: 重流量（heavy-hitter）检测支撑线速 DDoS 缓解与限速，但其对自适应攻击者的鲁棒性几乎未被探索，现有评测多基于静态流量，无法反映智能对手的规避行为。

论文构建端到端评估框架，将检测逻辑嵌入交换机级模拟器并用强化学习自动调参以限速大象流；训练神经流量代理（NTA）生成规避检测的流量模式，再通过检测器与攻击者联合对抗训练达到纳什均衡，并用神经监视代理（NMA）作为替代模型训练零样本攻击者。

结果发现所有 HHD 算法均存在系统性盲区：合成背景流量下带宽超用 10%–299%，真实捕获流量下达 78%–671%；联合对抗训练将 Elastic Sketch 的可利用性从 299% 降至 139%（2.2 倍），限速性能从 60.8 提升至 102.4（1.7 倍）；零样本攻击成功攻破 9 个测试系统中的 8 个，且无需任何目标系统先验知识。

这是首次量化 HHD 对抗盲区的工作，框架开源可复现，并提供了建设性的加固路径；但模拟器与真实部署环境的差距、NMA 替代模型的可迁移边界仍需谨慎对待，其价值在于为数据平面流量监控的对抗鲁棒性研究开辟了新方向。

- **论文 PDF**: [Strategic Games and Zero Shot Attacks on Heavy-Hitter Network Flow Monitoring.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1301-paper.pdf)

#### Tickets to Hide: An Inside Look into the Anti-Abuse Ecosystem through Internal Abuse Data.

- **作者**: Hugo L. J. Bijmans, Michel van Eeten, Rolf van Wegberg

- **方向**: 网络与协议安全

- **解读**: 互联网治理依赖"举报—通知—处置"机制打击滥用，但托管商对哪些滥用报告真正跟进、为何忽略，长期缺乏内部视角；被贴上"bulletproof"（抗举报）标签的托管商，其内部处置逻辑从未被实证检验。

作者通过与荷兰执法部门（FIOD）的独家合作，获取一家自 2010 年代初即被列为顶级"坏托管商"的公司被查扣的后端数据：2011 年 8 月至 2020 年 9 月共 130.9 万条滥用报告及 CRM 数据库（230 万张工单），用关键词将报告归入 M3AAWG 六类，通过历史 IP 分配关联到 3114 个客户端，识别出 9227 条客户端通知，并将结果与外部 BGP 网络特征测量对照。

整体客户端通知率仅 2.6%；CSAM 报告通知率 45.6%（2020 年达 85%）、垃圾邮件 19%，而版权投诉仅 0.4%（每个举报者平均发 609 份报告）、端口扫描 3.9%；可信举报者（Spamhaus 60%–84%、NetCraft 72%、PhishLabs 54%）比自动化工具（Fail2Ban）与个人举报者获得远高通知率；封锁列表、去对等、执法施压等影响业务连续性的治理手段有效，个体举报常被无视；IP 碎片化、短前缀通告等外部"恶意指标"在该公司与良善托管商间差异不显著。

我的思考：这是首个基于内部数据的托管商滥用处置研究，以铁证揭示了"经济理性驱动"的处置逻辑：能威胁业务连续性的举报被认真对待，个体举报被系统性忽略，版权内容被有意纵容（如"DMCA 是美国法律，我们不在美国"）。局限是单一案例、关键词分类存在误差、删信可能低估报告量。对 DSA"可信举报者"等政策有直接启示：治理工具必须让不处置产生业务成本，同时仅凭外部测量给"bulletproof"贴标签并不可靠。

- **论文 PDF**: [Tickets to Hide: An Inside Look into the Anti-Abuse Ecosystem through Internal Abuse Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f468-paper.pdf)

#### Time and Time Again: Leveraging TCP Timestamps to Improve Remote Timing Attacks.

- **作者**: Vik Vanderlinden, Tom van Goethem, Mathy Vanhoef

- **方向**: 网络与协议安全

- **解读**: 计时侧信道攻击（如 Lucky 13、用户枚举）是经典威胁，但远程实施受网络抖动制约：处理器变快使计时差更小、网络拥塞放大抖动，导致需要海量请求并易被限速机制挫败，也抑制了黑盒漏洞发现。

作者提出完全不受网络路径抖动影响的攻击：服务器收到请求后立即回 ACK、处理完再发响应，两段都携带 TCP 时间戳（TSval），两者之差即服务端运行时；进一步把多个请求合并进一个 TCP 段或 TLS 记录使其同时到达并被顺序处理，把微小计时差放大 N 倍；还可由多台主机分布式采集样本规避限速与检测。测量显示 88.38% 的互联网服务器支持时间戳，99.4% 在首个请求后立即 ACK，95.5% 支持持久连接。

在实验室 GCP 服务器（约 13 亿请求）上，毫秒时间戳可检测比 RTT 攻击小 5 倍的计时差，所需请求数减少 1/5 至 1/50（如 50µs 差从 2944 请求降至 90）；微秒时间戳下可检测 750ns 差（比传统攻击小 33 倍）；分布式攻击（法国/波兰/芬兰三客户端合并）结果与单点一致；案例研究复现了 Intel ELL 库的 Lucky 13 漏洞（获 CVE-2025-32998）、OpenSSH 用户枚举与 ProFTPd 攻击。

我的思考：核心洞见是把 TCP 时间戳从"RTT 估计工具"转化为"服务器内部时钟探针"，把测量从客户端网络路径移到服务器本地，并给出放大与分布式两套机制，显著降低了远程计时攻击的门槛与样本需求。局限：CDN 不转发源站时间戳需直打源站（可用已有技术定位）；要求立即 ACK 与 HTTP/1.1 顺序处理；毫秒时间戳下只能离散量化，服务器端抖动仍会积累。与 timeless timing attack 相比前置条件更宽松且与协议无关（TCP 之上皆可），对各类加密库与 Web 应用审计都有实际价值。

- **论文 PDF**: [Time and Time Again: Leveraging TCP Timestamps to Improve Remote Timing Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s893-paper.pdf)

#### Time will Tell: Large-scale De-anonymization of Hidden I2P Services via Live Behavior Alignment.

- **作者**: Hongze Wang, Zhen Ling, Xiangyu Xu, Yumingzhi Pan, Guangchi Liu, Junzhou Luo, Xinwen Fu

- **方向**: 网络与协议安全

- **解读**: I2P 匿名网络（约 4.5 万活跃路由、1.5 万日活用户与服务）用多跳 garlic 路由与 netDB 保护隐藏服务身份，传统流量关联式去匿名化需控制目标隧道关键位置，在 I2P 规模下难以实施；而用户在线/离线行为天然多样（仅 31.15% 的路由存活超 30 天），这一行为侧信道从未被系统性利用。

作者提出 I2PERCEPTION：仅部署 15 个 floodfill 路由器被动收集全网 RouterInfo（低开销、隐蔽），利用 Java/C++ 两种实现的路由信息发布机制（Java 每 32–43 分钟例行发布、启动间隔特征、下线前变更 floodfill 标志等）精确识别路由的加入/离开行为；针对数据缺失设计数据恢复式会话补全；对目标隐藏服务用主动探测（查询 LeaseSet 并连接）推断在线行为序列；最后用定制距离函数的动态时间规整（DTW）度量相似度，划分匿名集。

在约 8 个月的真实部署（15 个 floodfill + 10 个受控隐藏服务路由）中，理想数据下加入时间推断偏差中位数小于 10 秒，会话补全显著改善非 floodfill 与不可达端口路由的推断（如离开偏差中位数从 1400 秒降至 600 秒）；去匿名化全部受控隐藏服务，准确率接近 100%，S1–S4（频繁开关机）第 2 天、S5 第 10 天、S6 第 26 天、S7 第 34 天即可将匿名集唯一化。

我的思考：新颖性在于把用户行为作为可测量侧信道而非流量内容，15 个 floodfill 路由的低成本规模与"监控窗口越长匿名集越小"的理论分析有说服力，并已向 I2P 项目方披露获修复。局限：目标服务若运行于 hidden mode 路由则 RouterInfo 不可获取，攻击失效；需较长监控窗口才唯一化，对常在线服务有限。与 Tor 流量关联工作相比攻击成本显著更低，警示匿名网络仅靠加密与多跳无法抵御行为指纹侧信道。
- **论文 PDF**: [Time will Tell: Large-scale De-anonymization of Hidden I2P Services via Live Behavior Alignment.](https://www.ndss-symposium.org/wp-content/uploads/2026-f114-paper.pdf)

#### TIPSO-GAN: Malicious Network Traffic Detection Using a Novel Optimized Generative Adversarial Network.
- **作者**: Ernest Akpaku, Jinfu Chen, Joshua Ofoeda
- **方向**: 网络与协议安全
- **解读**:
零日等未知攻击缺少签名、行为新颖，传统深度学习 IDS（LSTM/CNN/Transformer 类）依赖大规模标注数据、对未知恶意流量泛化差；而 GAN 用于入侵检测又饱受训练不稳定与模式崩溃困扰。本文提出 TIPSO-GAN，将 GAN 训练重构为粒子群优化（PSO）问题，用群体智能解决对抗训练中的非凸高维优化难题。

方法上对 PSO 做三处增强：自适应 Sigmoid 惯性权重动态平衡探索/利用、多样性保持机制（适应度共享、停滞粒子重初始化、速度随机扰动）防止早熟收敛；采用两阶段对抗训练——先由 PSOGAN 在纯正常流量上训练生成器，再把预训练 CNN 分类器 DeePred（含时间衰减多头自注意力，突出近期恶意特征）迁移为判别器，最后只在正常流量上无监督微调以实现零日检测；目标函数融合重建损失与焦点损失，兼顾样本真实性与难分类恶意样本。

在 CIC-IDS2018、CICAPT-IIoT2024、CIC-DDoS2019 上 F1 达 99.1±0.1、98.9±0.1、98.7±0.1，超过最强基线 0.2–1.0 F1 并优于 Transformer IDS；IIoT 上 macro PR-AUC 0.999±0.002，远超次优的 0.960±0.005；严格零日评测下 LOFO 达 92.3 F1、跨数据集 79–83 F1 且召回率保持 0.80 以上；同时维持 0.42ms 延迟、约 2400 flows/s 吞吐与 2.1GB 占用。

贡献成色在于把 PSO 稳定性增强与"正常流量无监督微调"的零日异常检测范式系统结合，并做了防泄漏分割与六类对抗鲁棒性测试，工程完整度高；但相对最强基线的提升仅 0.2–1.0 F1，跨数据集 15–20 分的回落说明分布漂移仍是核心短板，且需要警惕 99.9% 量级 PR-AUC 对特定数据集特性的过拟合风险；相比纯 GAN-IDS 基线确实更稳，但与成熟异常检测方案相比其零日增益缺乏独立数据集交叉验证。
- **论文 PDF**: [TIPSO-GAN: Malicious Network Traffic Detection Using a Novel Optimized Generative Adversarial Network.](https://www.ndss-symposium.org/wp-content/uploads/2026-f3241-paper.pdf)

#### Understanding the Stealthy BGP Hijacking Risk in the ROV Era.
- **作者**: Yihao Chen, Qi Li, Ke Xu, Zhuotao Liu, Jianping Wu
- **方向**: 网络与协议安全
- **解读**:
ROV 部分部署（截至 2025 年 3 月 ROA 已覆盖 57.1% 前缀，但启用 ROV 的 AS 仅数百至数千）带来一种反直觉威胁——隐蔽劫持：恶意路由被 ROV 拦截、从不进入受害 AS 的路由表（控制面完全不可见），但流量在数据面路径上经遗留 AS 被静默劫持，使受害方对攻击毫无感知，传统控制面防护失效；此前既无真实事件记录也无系统刻画。

本文先形式化定义隐蔽劫持（"受害者-目标-劫持者"三元组与风险关键 AS 模型），提出基于路由表差异的启发式在野外发现实例；随后开发 SHAMAN 框架：融合多来源 ROV 测量构建准确部署视图，用单字节编码+矩阵运算+拓扑压缩高效推断全网合法路由与劫持者可达路由，并通过三元组模型做统计化风险分析。

2025 年两月窗口捕获 1,393 起疑似事件，经 RPKI/IRR/WHOIS 交叉验证得 318 起高置信事件、覆盖 2,178 条路由；SHAMAN 把全网路由生成从 3 个月以上降至 5.22 小时（约 500 倍加速），在 77,600 个 AS、8.3 亿条路由上评估：总体隐蔽劫持成功率 14.1%，定向攻击最高 99.5%（非定向最高 23.6%）；对照真实数据集的一致性最高 95.9%。

首次系统揭示部分 ROV 部署的副作用：ROV 让受害者看不到恶意路由，反而剥夺其控制面感知、加剧攻击隐蔽性，对"部署越多越安全"的直觉提出修正；启发式（真实观测）+SHAMAN（确定性评估）双路径互补，矩阵化路由推断的工程加速是显著亮点；局限是 14.1% 是风险概率而非实测攻击（尚无真实事件证实），路由推断依赖简化 AS 级策略假设，数据面验证有限；对 ROV 升级路径（覆盖率的临界点）与运营商部署策略有直接政策含义。
- **论文 PDF**: [Understanding the Stealthy BGP Hijacking Risk in the ROV Era.](https://www.ndss-symposium.org/wp-content/uploads/2026-s97-paper.pdf)

#### Validity Is Not Enough: Uncovering the Security Pitfall in Chainlink's Off-Chain Reporting Protocol.

- **作者**: Di Zhai, Jiashuo Zhang, Jianbo Gao, Tianhao Liu, Tao Zhang, Jian Wang, Jiqiang Liu
- **方向**: 网络与协议安全
- **解读**: 区块链预言机为 DeFi 智能合约输送链外价格数据，Chainlink 作为主导提供商（截至 2025 年 5 月守护全网预言机总价值的 68%），其去中心化预言机网络（DON）运行 Off-Chain Reporting（OCR）协议确定代表价格。OCR 的 validity 属性保证最终价格落在诚实节点观测值范围内（honest range），但真实世界中该范围有多宽、拜占庭行为仍能诱导多大偏差，此前无人量化，而价格偏差可直接转化为套利与合约错误执行。

方法上，本文结合实证与理论分析：先用 Chainlink 84,097 个历史价格 feed 实例度量 honest range 宽度；随后形式化建模拜占庭节点战略性摇摆价格的行为，区分两类策略并给出影响度量指标；最后用真实价格数据模拟评估，并对下游清算与收费交易做案例研究。

实验发现 honest range 宽度最高达 ETH 价格的 13.13%，远超此前"可忽略参数"的假设；拜占庭行为可诱导最高 8.47% 的 ETH 价格偏差；案例分析显示单次被操纵价格的金融影响量级达 10^5 美元，累积可达数百万美元（评估涵盖 72,711 个过滤后的 feed 实例、13 笔下游清算、4,465,424 笔下游收费交易）。作者已负责任地向 Chainlink 报告。

我的思考：本文揭示"有效性保证 ≠ 无偏差"，首次量化 OCR 协议在真实市场波动下 validity 属性的安全余量，实证规模扎实，对 DeFi 生态是重要警示。局限：拜占庭模型假定节点被攻陷或合谋，而 Chainlink DON 目前不允许外部节点加入，攻击门槛高于理论模型；honest range 宽度部分源于各节点数据源本身的市场差异，未必都是恶意；缓解策略与去中心化激励的工程权衡讨论有限。与把 honest range 视为可忽略参数的前作相比是重要实证补全。

- **论文 PDF**: [Validity Is Not Enough: Uncovering the Security Pitfall in Chainlink's Off-Chain Reporting Protocol.](https://www.ndss-symposium.org/wp-content/uploads/2026-f458-paper.pdf)

#### When Mixnets Fail: Evaluating, Quantifying, and Mitigating the Impact of Adversarial Nodes in Mix Networks.
- **作者**: Mahdi Rahimi
- **方向**: 网络与协议安全
- **解读**: 混淆网络（Nym/Loopix）为对抗全局被动敌手（GPA）让每个包独立随机选路以最大化混淆，却忽视"混入节点敌手"：会话中任一包的整条路径都是敌手节点，该客户端-目的地对即被完全去匿名化。

方法上，作者先量化威胁：敌手控制 20%/10%/5% 节点时，客户端分别只需交换 1/4/30 MB 数据即被完全去匿名化；随后提出三种降低路径多样性的策略——K-HF（固定部分层）、K/W（每层限定 K 个候选）、α-SS（概率复用会话内路径），并引入 DLM、CAM 与基于香农熵的模拟度量。

相对基线，K-HF、K/W、α-SS 将节点敌手优势分别降低最多 80%、85%、80%，而 GPA 优势仅增加 1.5、2、1 bit；模拟还揭示现有最先进匿名度量产生误导，曾影响 Nym 的设计决策——环状覆盖流量并未降低节点敌手威胁。

我的思考：填补"抗 GPA 最优设计牺牲抗节点敌手鲁棒性"的盲区，量化清晰、度量务实，三个策略构成精度-负载-隐私的权衡谱；本质是用会话内路径相似化换取稳健性，<2 bit 的 GPA 增益可接受。局限是单会话模型、假设敌手均匀分布，多敌手联合与时序侧信道仍待研究。

- **论文 PDF**: [When Mixnets Fail: Evaluating, Quantifying, and Mitigating the Impact of Adversarial Nodes in Mix Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2384-paper.pdf)

### 密码学与协议（25 篇）

#### Abuse Resistant Traceability with Minimal Trust for Encrypted Messaging Systems.
- **作者**: Zhongming Wang, Tao Xiang, Xiaoguo Li, Guomin Yang, Biwen Chen, Ze Jiang, Jiacheng Wang, Chuan Ma, Robert H. Deng
- **方向**: 密码学与协议
- **解读**: 端到端加密消息系统保护用户通信却阻碍内容治理，溯源虽能打击线上滥用，却可能被滥用为大规模监控。现有阈值报告方案存在Sybil攻击脆弱性，黑名单方案则过度信任监管方或假设同一平台服务器不共谋，信任假设过强。

作者提出抗滥用溯源方案，把溯源职责分布到用户、平台、监管方、审核员四类实体，实现两条核心原则：最小信任——只要参与溯源的一方诚实，溯源便不可被滥用；最小信息披露——参与者无法获得与溯源无关的信息。技术上，追踪元数据由三层多密钥PKE密文、匿名令牌、MAC标签与零知识一致性证明构成，结合去重标签的精确阈值报告、事后黑名单（SPCE）与内容复核，只有同时满足"多人举报+在黑名单+经审核"的消息才能被溯源。

论文形式化定义了方案语法并证明了源机密性、消息机密性、黑名单机密性、可追溯问责性、报告唯一性、匿名性等安全性质；实现采用Signal同源技术（KVAC匿名凭证）并用Rust完成，评估显示其性能与现有易受滥用方案相当（注：全文文本在评估部分被截断，具体数字未获取，此结论来自摘要）。

我的思考：把"谁能溯源、凭什么溯源"的治理问题落到四方制衡的密码学流程上，是对Apple CSAM扫描式单一信任点的实质改进，且"事后黑名单+审核员复核"缓解了黑名单泄露与监管方作恶两大痛点；局限在于依赖监管方与审核员的真实存在与勤勉、tmd与报告流程带来额外开销，匿名性、去链接性与效率的权衡仍有设计空间。

- **论文 PDF**: [Abuse Resistant Traceability with Minimal Trust for Encrypted Messaging Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f456-paper.pdf)

#### ACTS: Attestations of Contents in TLS Sessions.
- **作者**: Pierpaolo Della Monica, Ivan Visconti, Andrea Vitaletti, Marco Zecchini
- **方向**: 密码学与协议
- **解读**: DECO、TLSNotary 等 TLS oracle 技术能让用户证明从 TLS 服务器取回的数据，但验证方需运行 ZK-SNARK 等非标准化密码方案，阻碍大规模采用。ACTS 的目标是让验证方仅用标准签名校验即可，同时保持服务器端透明、notary 无法窥探数据。
基于 Fuchsbauer-Wolf 的谓词盲签名（PBS）概念，构造两轮、输出标准 RSA-PSS 签名的谓词盲签名协议 PBSRSA：用户将消息 PSS 编码后以 Chaum 盲化方式隐藏，并附 NIZK 证明消息满足谓词（如“确为从 TLS 服务器取回的响应”）；支持多 notary 盲多重签名抗合谋，用 Circom 实现 NIZK（VitH 与 Groth16 双后端）。
以 26KB 真实 PDF 为用例，notary 盲签后嵌入标准 PADES 签名，Adobe Acrobat Reader 可直接验证；初始证明生成需 328 秒、201.65GB RAM，经 SHA256/AES 分块优化后内存随输入线性增长，可适配 32GB 以下常见硬件。
思路巧妙——把验证端从 ZK 降到标准签名，显著降低部署门槛；但 PBS 的 NIZK 开销仍在用户/notary 侧，且安全性依赖 RSA-PSS 的 EUF-CMA 假设；与 DECO 集成保留了 UC 框架论证，但实现与理论仍有差距，是 TLS oracle 走向实用的重要一步。

- **论文 PDF**: [ACTS: Attestations of Contents in TLS Sessions.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1861-paper.pdf)

#### BKPIR: Keyword PIR for Private Boolean Retrieval.
- **作者**: Jie Song, Zhen Xu, Yan Zhang, Pengwei Zhan, Mingxuan Li, Shuai Ma, Ru Xie
- **方向**: 密码学与协议
- **解读**:
关键词PIR允许不泄露查询地检索关键词关联数据，但现有方案隐含"关键词-值一一对应"假设，无法处理倒排索引中的多对多映射，更不能表达AND/OR/NOT布尔检索，限制了隐私网页搜索、专利检索等应用。

MMKPIR把关键词编码为常重量码（CW(w,k)）作行标识，值集以SIMD打包成RLWE明文，构成n行线性结构（存储O(n)而非O(n·m)）；查询同态加密后经常重量等式运算（累积和与k!逆元）得选择掩码，与负载相乘聚合、单轮返回单密文。BKPIR进一步以m维multi-hot值向量表示关键词，同态乘实现AND、减一取负实现NOT、De Morgan实现OR，并给出大m/大负载变体，安全达IND-CKA/IND-CBQA。

相对基线CwPIR，总运行时间降约3倍、预处理快3.5～40倍；固定128MB数据库下预处理提速8.9倍、总运行提速3.2倍，理论外推加速达3.2×m（m=2¹⁶时约20.9万倍）；响应通信最多降m倍，查询为固定单密文；BKPIR单关键词性能与MMKPIR几乎持平。

首个结合布尔检索模型的关键词PIR，常重量码+SIMD值集打包优雅化解多对多结构，哈希编码支持任意长度关键词、免预协商。局限：威胁模型仅半诚实服务器；负载不均时填充开销上升；大m场景O(nf+nl)内积成本不可忽视；对称PIR扩展仅附录简述。

- **论文 PDF**: [BKPIR: Keyword PIR for Private Boolean Retrieval.](https://www.ndss-symposium.org/wp-content/uploads/2026-s536-paper.pdf)

#### Cirrus: Performant and Accountable Distributed SNARK.

- **作者**: Wenhao Wang, Fangyan Shi, Dani Vilardell, Fan Zhang
- **方向**: 密码学与协议
- **解读**: 
SNARK可高效验证计算，但zkML、zkVM等大规模任务的证明生成开销巨大。分布式证明生成需同时具备：水平可扩展低开销（每worker线性计算、对数通信）、可追责（高效识别恶意worker）、通用可信设置。此前Pianist、HyperPianist缺乏可追责性，Hekaton可追责但依赖电路特定设置且prover时间为拟线性。

方法上，Cirrus在HyperPlonk（EUROCRYPT'23）基础上分布式化其核心构件：multilinear KZG、SumCheck、ZeroTest、PermTest。关键洞察：常数度多元多项式可表示为常数个multilinear多项式之函数，从而兼容分布式multilinear KZG的打开协议。可追责采用"乐观验证加按需定位"：协调者先验最终证明，失败才运行故障定位——验证各worker的KZG子证明、重建witness多项式并在SumCheck随机点比对求值，仅需O(M log T)次配对与O(MT)次域运算。另提出分层聚合：M/logT个leader代为聚合，将协调者复杂度降至O(M)。

实验在32台AWS t3.2xlarge（8 vCPU、32GB）上进行：33M门电路39秒生成证明，每worker通信40.0KB、证明16.6KB；vanilla HyperPlonk同配置仅支持4M门且需110秒以上。RAM程序（zkVM）256核近线性扩展，2¹⁴条CPU步证明<40秒。与Hekaton相比，Pedersen hash快7倍以上、MiMC约2倍、指数知识证明约4倍。故障定位在33M门、256 worker下4秒内完成。

我的思考：Cirrus是首个同时满足低开销、可追责、通用设置三大性质的分布式SNARK，理论复杂度与实现数据相互印证，且复用HyperPlunk现有设置参数，部署成本低；乐观验证加按需追责务实。局限：静态腐败模型、假定参与方共享witness，对比限于PLONK友好电路。影响：为zkRollup、prover market提供可追责底层原语，分层聚合思路对缓解协调者瓶颈有普遍借鉴意义。

- **论文 PDF**: [Cirrus: Performant and Accountable Distributed SNARK.](https://www.ndss-symposium.org/wp-content/uploads/2026-f668-paper.pdf)

#### Consensus in the Known Participation Model with Byzantine Faults and Sleepy Replicas.

- **作者**: Chenxu Wang, Sisi Duan, Minghui Xu, Feng Li, Xiuzhen Cheng
- **方向**: 密码学与协议
- **解读**: 
沉睡副本（honest replicas可随时离线又恢复）此前只在"未知参与模型"与同步网络下研究，而已知参与模型（副本知晓清醒honest副本最小数ha）下的拜占庭加沉睡混合故障共识缺乏精细处理。生产系统如Ethereum、DiemBFT、Tendermint已在非正式应对该问题。

方法上给出三组结果：(1)同步协议Koala-1，n≥2f+s+1、ha=f+1，核心是ha规模quorum配合双重确认机制与新的"验证三元分级提案选举"（VT-GPE：VRF选主加等级0/1/2分级共识），期望延迟5∆+2δ、最佳2∆+2δ；(2)部分同步下证明不可能性——n=3f+1界限内无稳定存储就无法容忍沉睡副本，并以HotStuff为例证明只需存视图号与lockedQC两个参数（HotStuff-mSS）；(3)无稳定存储的HotStuff变体Koala-2，要求s=⌊(n−3f−1)/2⌋（n≥3f+2s+1），GAT假设下s提升至n−3f−1，保持7δ延迟且界紧。

实验在LAN（∆=1秒）进行：Koala-1最佳延迟约为MMR的56.5%、期望约为41.7%（f=10时2117.80ms/5019.70ms对MMR的4000.00ms/11194.71ms）。HotStuff-mSS在f=30时峰值吞吐比全量存储高44.3%、比零存储低20.4%。Koala-2恢复期间吞吐最低降至均值53.4%，恢复时间9.25s（s=10）、9.89s（s=20）、10.38s（s=30）。双花演示确认：无稳定存储的HotStuff在副本苏醒后发生双花，HotStuff-mSS与Koala-2不受影响。

我的思考：这是已知参与模型下拜占庭加沉睡混合故障共识的首次精细刻画，同步快协议、不可能性加最小存储、无存储协议与紧界构成完整理论拼图，VT-GPE等构件可独立复用。局限：已知ha假设动态网络难保证、实验规模有限。影响：为以太坊inactivity leak等机制提供理论依据，"只存两个参数"直接指导工程权衡，7δ延迟不损失的转化对实用BFT有吸引力。

- **论文 PDF**: [Consensus in the Known Participation Model with Byzantine Faults and Sleepy Replicas.](https://www.ndss-symposium.org/wp-content/uploads/2026-s448-paper.pdf)

#### CRISP: An Efficient Cryptographic Framework for ML Inference Against Malicious Clients.

- **作者**: Xiaoyu Fang; Shihui Zheng; Lize Gu
- **方向**: 密码学与协议
- **解读**: 问题：基于半诚实模型的安全 ML 推理协议在实际部署中易受恶意客户端攻击导致模型参数泄漏（MUSE 已证明）；此前工作（MUSE、SIMC）引入额外 MAC 计算确保客户端行为正确，但显著增加在线推理的运行与通信开销，且作者发现 SIMC 的 GC 非线性层协议与若干乱码电路优化组合时会泄漏模型中间输入输出，存在兼容性安全隐患。

方法：CRISP 针对恶意客户端场景提出两处优化——非线性层基于函数秘密共享（FSS）重设计 ReLU：把 MAC 验证机制融入 FSS 密钥（β'=(μ,−μr) 构造验证门），在线阶段仅需一轮通信即可同时完成非线性函数与两个 MAC 的计算与重构，每个实例通信仅 2n 比特；线性层提出复数域验证机制，利用 CKKS 同态加密的复数编码空间把 MAC 值嵌入虚部，一次计算同时完成数值与验证标签，消除额外 MAC 同态计算；最后以随机标量聚合的一致性检查统一验证各层输入。

结果：非线性层通信较 SIMC 降低 96%–99%（2^18 个 ReLU 时约 2KB vs 218MB），计算提升 10%–90%；线性层延迟降低 24%–67%（全连接层在模数 16384 下由 4.66s 降至 1.52s）；端到端总通信最高降低 94%、推理时延最高降低 43%；作者还针对 SIMC 的兼容性问题构造了模型提取攻击，恢复权重误差 MSE 低于 0.0001。

我的思考：该工作既发现 SIMC 在真实部署中的安全缺陷并给出实证攻击，又提出 FSS+复数域 CKKS 的组合优化，两个贡献都有独立价值，"把验证标签塞进 CKKS 虚部"的思路可迁移到其他 MPC 场景；局限在于威胁模型仍是半诚实服务器+恶意客户端、基准与 SIMC 相同故跨方案可比性有限，且 CKKS 近似计算引入的精度损失与输入一致性校验成本仍需在实践中检验。

- **论文 PDF**: [CRISP: An Efficient Cryptographic Framework for ML Inference Against Malicious Clients.](https://www.ndss-symposium.org/wp-content/uploads/2026-s11-paper.pdf)

#### Cross-Consensus Reliable Broadcast and its Applications.

- **作者**: Yue Huang; Xin Wang; Haibin Zhang; Sisi Duan
- **方向**: 密码学与协议
- **解读**: 问题：传统拜占庭容错（BFT）协议只研究同一节点组内部的通信；而复制状态机（RSM）互连、分片区块链、跨链桥等现代应用大量涉及跨组通信（至少一组内建共识），其安全属性从未被形式化——"分片 A 给分片 B 发送消息"既没有清晰定义何谓正确，领导者发送方案在领导者故障时的正确性也不明确。

方法：作者提出新原语"跨共识可靠广播"（XRBC），建模组 A（m 节点）向组 B（n 节点）的跨组通信，要求无论 A 中正确节点发送的消息是否一致，B 中正确节点都按相同顺序交付；给出三个不同假设下的构造——XRBC-woA（仅 A 有原子广播 ABC，用 best-effort 广播+可靠协议 RA，O(mn+n²) 消息、O(mnL+κn²) 通信）、XRBC-wA（B 也有 ABC，复用其基础设施）、XRBC-Sig（A 侧消息带交付证明、每轮唯一负责节点，O(n+M_ABC) 消息）；并给出三个应用：Reticulum 跨分片协调（同步变体把假设从 n+m≥2(f+t)+1 放宽到 n≥2f+1，通信由 O(mκ(m+n)²) 降至 O(κmn+κn²)）、Chainspace 跨分片交易（修复 BFT 发起者故障时的监控难题）、跨链桥。

结果：用 Go 实现（11,000+ 行，gRPC+HotStuff），在 Amazon EC2 上最多 182 个实例评估；即使 f=30 时 XRBC-woA 延迟也仅 12ms，三个协议均保持低延迟与可观吞吐；Reticulum 案例中 XRBC 方案较原方案延迟降低 61.16%，且能以更少假设取得更低通信复杂度。

我的思考：首次把跨组共识通信形式化为头等原语并给出三种假设下的高效构造，理论贡献大于单一应用，为分片与跨链协议提供了可复用的正确性论证工具，也揭示了 Chainspace、Pyramid 等既有方案"忽略跨片通信可靠性"的隐患；局限在于与自建 Group-PRBC 基线对比（无外部成熟实现可比），XRBC-Sig 依赖负责节点伪随机选择故终止是概率性保证，且 12 秒级延迟仅代表实验室网络条件，真实广域网部署仍待验证。

- **论文 PDF**: [Cross-Consensus Reliable Broadcast and its Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-s207-paper.pdf)

#### Cryptobazaar: Private Sealed-bid Auctions at Scale.

- **作者**: Andrija Novakovic; Alireza Kavousi; Kobi Gurkan; Philipp Jovanovic
- **方向**: 密码学与协议
- **解读**: 问题：密封拍卖需要保护败标者出价隐私、结果公开可验证、信任最小化与可扩展性；现有方案各有短板——commit-reveal 存在选择性披露的激励错位且难保败标者隐私，基于通用 MPC/FHE 的方案不可扩展，最新工作 Addax 虽兼顾隐私与扩展性却依赖两个及以上拍卖人非共谋的弱化威胁模型。

方法：Cryptobazaar 将匿名否决（AV）协议用于分布式计算全部一元编码出价的逻辑 OR，从而在隐藏败标者出价的同时确定最高价，仅需一个不可信拍卖人做星型拓扑协调；配套设计多个新的简洁零知识论证（zkSNARK）：π_xi 把 KZG 多项式承诺与点向量绑定、π_ri 证明随机向量非零、π_bi 用对数导数+单变量 sumcheck 证明出价是一元编码、π_Zi 证明 Z=(x+b∘r)∘Y 形式（以单变量 sumcheck 替代内积论证）、π_w 用集合成员+nullifier 证明赢家资格；支持一价、二价及更一般的 (p+1) 价拍卖与顺序拍卖（拍卖人以 b0=n 掩码出价并配合欺诈证明）。

结果：Rust+arkworks 实现（BN254 曲线，M1 Max 实测）：128 名竞拍者、1024 个价位的一次拍卖在 0.5 秒内完成，每名竞拍者仅需收发约 32KB 数据（拍卖人侧共约 4.2MB）；运行时满足以太坊 PBS 架构 12 秒出块窗口的时限要求，且 AV 计算可利用相邻行关系把 O(n²) 降为 O(n) 群运算。

我的思考：把 AV 协议从"否决"语义巧妙映射到"最高价探测"是核心创意，配合一批可独立复用（如增强可验证洗牌）的 ZK 小工具，系统完整度与工程成熟度较高，是少有的同时满足隐私、可验证、单不可信拍卖人三大属性的方案；局限在于依赖授权参与者（可被 slash 惩罚）与 AV 的 DDH 假设，与 Addax 相比则以证明计算换取去除非共谋假设。

- **论文 PDF**: [Cryptobazaar: Private Sealed-bid Auctions at Scale.](https://www.ndss-symposium.org/wp-content/uploads/2026-f481-paper.pdf)

#### cwPSU: Efficient Unbalanced Private Set Union via Constant-weight Codes.

- **作者**: Qingwen Li, Song Bian, Hui Li
- **方向**: 密码学与协议
- **解读**: 非平衡私密集合并集（PSU）中，现有协议（Tu et al.、Zhang et al.）的通信开销仍部分依赖大集合大小，且多次 OPRF 调用造成轮数瓶颈，难以支持大集合（如 2^22 元素）与低带宽场景。

cwPSU 用常量权重编码加分级 FHE（BFV）构造：提出 Batched Ciphertext Shuffle（BCS）原语，结合 Permute+Share 安全重排打包密文以消除选择向量泄漏；并设计优化算术常量权重相等算子 EEQ，利用多项式对称性与 Paterson-Stockmeyer 算法，把密文-密文乘法降至朴素方法的约三分之一。

实验显示，其在线通信仅线性于小集合 m 而与 n 无关，且只需单轮在线通信：相比 SOTA [13] 通信降低 5.1–32.4 倍、在线运行时间加速 3.1–13.3 倍；例如 m=2^12、n=2^20 在 1Mbps 下仅 12.47s（对比 166.76s/108.92s）；LAN 下 setup 阶段比 [13] 快两个数量级以上，EEQ 算子自身亦有约 2 倍加速；n 达 2^24 时在线开销仍仅 0.35MB。

该工作显著推进了非平衡 PSU 的实用性，理论（sUC 混合模型半诚实安全）与实现（SEAL）完备；不过 setup 阶段密文尺寸仍较大、依赖发送方集合可预计算，且暂限于两方半诚实模型，扩展多参与方与其他私密集合操作是自然后续。

- **论文 PDF**: [cwPSU: Efficient Unbalanced Private Set Union via Constant-weight Codes.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1128-paper.pdf)

#### Distributed Broadcast Encryption for Confidential Interoperability across Private Blockchains.

- **作者**: Angelo De Caro, Kaoutar Elkhiyaoui, Sandeep Nishad, Sikhar Patranabis, Venkatraman Ramakrishna
- **方向**: 密码学与协议
- **解读**: 私有链跨链互通需把账本状态机密地传给他网成员，现有做法依赖受信中心化代理解密转发，与 DLT 去中心化原则（避免单点信任）相悖，代理被攻破即可泄密账本数据。

论文在简化 UC（sUC）框架中将机密跨链通信形式化为理想功能 FCN，提出用完全分布式广播加密（FDBE）实现：分布式 setup 与密钥生成，互不信任各方按轮次逐一向链上智能合约提交 Update 完成公共参数演进（天然契合账本顺序写入，无需点对点信道），作弊方可被识别并跳过该轮而无需整体重启；实例化以 [36] 的常数级 BE 为基础，扩充公共参数实现防共谋的分布式密钥生成。

BLS12-381 实测：解密密钥 0.19KB、密文 0.74KB 均与参与方数 n 无关，公共参数 O(n)（n=500 时 205.64KB）；每方密钥生成时间恒定约 1.15ms，而此前的分布式方案 [77] 在 n=200 时需约 58 秒且更大规模内存不足；并在 Hyperledger Cacti 框架中对两个 Fabric 私有网络做了参考实现验证。

该工作把跨链机密性的信任假设从单点代理降为多方阈值，安全证明（sUC + 恶意安全）严谨，常数级开销使其在 IoT 消息组等场景也有推广价值；局限是假设参与方在线、账本抗审查与同步网络，Freeze 滥用需速率限制，实际 Fabric 集成基准因文本截断未见完整数据。

- **论文 PDF**: [Distributed Broadcast Encryption for Confidential Interoperability across Private Blockchains.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1200-paper.pdf)

#### Faster Than Ever: A New Lightweight Private Set Intersection and Its Variants.
- **作者**: Guowei Ling, Peng Tang, Jinyong Shan, Liyao Xiang, Weidong Qiu
- **方向**: 密码学与协议
- **解读**: 
隐私集合求交（PSI）是广告归因、联合风控等场景的核心组件。近年来 VOLE 类 PSI 被认为是最优范式，其进展主要停留在更换底层 VOLE 或 OKVS 组件，性能提升趋缓；且它依赖 OTE/VOLE/Beaver 三元组等大量伪随机相关，计算与通信都存在固有开销。本文提出一条脱离 VOLE 范式的新路线。

新协议只使用至多 κ 个基础随机 OT、一次 OKVS 编码和发送方输入规模次解码，其余全部用 SHA-256（SHA2-NI 指令）与位运算完成，同时给出半诚实与恶意两种安全模型（含对 OKVS overfitting 攻击的抵御）。作者进一步把该 PSI 扩展为 circuit PSI（布谷鸟哈希+OKVS+通用两方计算），并派生 PSI-cardinality、PSI-sum 与 Private Join and Compute（PJC）；理论上还证明了在相同 OKVS 冗余与安全模型下其通信量严格小于 VOLE 类 PSI。

实验表明：PSI 比 VOLE 类 PSI 快约 1.5× 且通信严格更少，比最优 OTE 类 PSI（KKRT）快 3 倍以上、通信降 4 倍；circuit PSI 快 3.6×、通信降 1.5×（10Mbps 下仍快 1.7×）；PSI-cardinality 与 PSI-sum 分别比 CZZ+ 快 12.2× 与 10×（通信多约 50%，仅在 10Mbps 下略慢）；PJC 比 IKN+ 快 731×、通信降 3.2×（10Mbps 下仍快 18×）；非平衡场景在 LAN 下最多提升两个数量级。

我认为这是近年 PSI 领域少有的范式级突破：绕开 OTE/VOLE 相关扩展，仅靠基础 OT+OKVS 即可达到更高效率，且对 OT/OKVS 的任何改进都能直接迁移，理论通信下界也清晰。局限在于 cardinality/sum 变体在极低带宽下优势反转、电路类变体仅半诚实安全（与主流一致）；实际性能高度依赖 SHA2-NI 等硬件指令集，跨平台性尚待验证。
- **论文 PDF**: [Faster Than Ever: A New Lightweight Private Set Intersection and Its Variants.](https://www.ndss-symposium.org/wp-content/uploads/2026-f131-paper.pdf)

#### Kangaroo: A Private and Amortized Inference Framework over WAN for Large-Scale Decision Tree Evaluation.

- **作者**: Wei Xu, Hui Zhu, Yandong Zheng, Song Bian, Ning Sun, Hao Yuan, Dengguo Feng, Hui Li
- **方向**: 密码学与协议
- **解读**:
MaaS 场景下既要保护客户端数据隐私又要保护服务端模型知识产权，可解释性强、适合表格数据的决策树/随机森林使私有决策树评估（PDTE）成为热点。但现有方案的通信与计算成本随树数、节点数或深度线性增长，深度轮次方案在 WAN 下延迟大、常数轮方案成本随规模线性膨胀，均难以支撑大规模模型。

Kangaroo 提出基于打包同态加密（PHE/BFV）的两方框架，核心是把决策树节点映射为多项式系数实现模型隐藏与编码，并设计打包特征选择（I/II-PackFeatureSel）、打包不经意比较（PackObliviousCom）与打包路径评估（PackPathEva）三个 SIMD 组件，使开销随节点/树数量完全摊还；另有同模型共享、延迟感知与自适应编码调整等优化增强实用性。

WAN 环境下较 SOTA 单轮交互方案（RCC/XXCMP）取得 14×–59× 性能提升，大规模推理较现有方案提速 3×–44×；对含 969 棵树、411825 个节点的随机森林可实现约 60ms/树的摊还推理；PackObliviousCom 摊还在线时间仅 0.88µs、单轮通信且无需离线阶段。

我的思考：这是首个在 PHE 上实现完全摊还的 PDTE 框架，把"每个密文系数对应一个节点"的 SIMD 思路做到极致，规避了轮次与线性成本的双重瓶颈，工程优化也贴合真实 WAN 部署。局限在于安全模型为半诚实双方，恶意安全与外包场景的扩展、BFV 参数选择对精度/性能的权衡仍值得后续研究。

- **论文 PDF**: [Kangaroo: A Private and Amortized Inference Framework over WAN for Large-Scale Decision Tree Evaluation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s892-paper.pdf)

#### Learning from Leakage: Database Reconstruction from Just a Few Multidimensional Range Queries.

- **作者**: Peijie Li, Huanhuan Chen, Kaitai Liang, Evangelia Anna Markatou
- **方向**: 密码学与协议
- **解读**:
可搜索加密（SE）为换取查询效率必然泄露访问模式、搜索模式等信息，多维范围查询下的泄露到底有多危险是开放问题；一维重建攻击已较成熟，但高维场景现有方法要么要求过多查询信息（如观察所有响应），要么在稀疏数据库上重建质量差。

论文提出 REMIN，多维设置下的泄露滥用攻击：利用范围查询的访问与搜索模式泄露，通过无监督表示学习（t-SNE 类几何感知嵌入）把查询共现频率转化为几何信号，从而推断密文记录间的相对空间关系，在极少查询下实现高维数据库的准确、可扩展重建；REMIN-P 为主动变体，注入少量已知位置的锚点毒化数据，改善稀疏与边界区域的重建质量。

合成与真实数据集上较 SOTA 重建攻击 MSE 平均降低约 50%（Manhattan 数据集在 1% 查询覆盖率下较 Markatou et al. 降低 80%，MSE 低至 1.29）；1% 查询下容忍匹配率超基线两倍，最小信息下邻域准确率仍高于 72%；REMIN-P 毒化可再平均降低 50% MSE，对高达 60% 的稀疏度仍稳健且运行时可扩展。

我的思考：把表示学习引入泄露滥用攻击，使"少量查询重建几何结构"成为现实，比纯拓扑恢复更贴近攻击者真实能力，为 SE 设计者量化了风险。局限在于仍是近似重建而非精确恢复，极端稀疏与高维场景依旧困难；其对真实 SE 系统（如 MongoDB Queryable Encryption）的实际威胁程度值得进一步实证。

- **论文 PDF**: [Learning from Leakage: Database Reconstruction from Just a Few Multidimensional Range Queries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f935-paper.pdf)

#### Looma: A Low-Latency PQTLS Authentication Architecture for Cloud Applications.

- **作者**: Xinshu Ma, Michio Honda
- **方向**: 密码学与协议
- **解读**: 量子计算机威胁RSA/ECDSA等经典公钥体系，TLS必须向抗量子（PQ）迁移；但PQ签名握手计算开销大，而云环境中微服务/无服务器的短连接高握手率叠加mTLS双向认证，使认证成为延迟的主要来源——实测密码学运算占sTLS握手延迟的30%–68%、mTLS的54%–70%。Looma的目标是在不牺牲安全性的前提下大幅降低PQTLS握手延迟。

Looma把online/offline签名范式引入TLS工作流：采用Even–Goldreich–Micali（EGM）构造，用WOTS+一次性签名包装Dilithium-2，将昂贵的PQ签名拆分为"快速在线签名/验签"（FastSign/FastVerify，纳秒级）与"慢速离线预计算"（后台持续生成并上传被签名的公钥）；KeyDist服务负责批量分发预签名公钥，缓存未命中时以双签名或混合模式安全回退到标准验证。实现采用Haraka8x哈希与Winternitz参数w=4，并配Bloom filter加速缓存查找。

集成TLS 1.3（Picotls）后，sTLS下P50/P99握手延迟比Dilithium-2基线低37%/42%，mTLS下最高低44%（P50）；Looma签名仅0.32µs（Dilithium-2为51.4µs），验签6.19µs（对比21.3µs）；mTLS one-to-many场景吞吐比Dilithium-2高40%–52%。单个KeyDist实例可支撑数百端点（每客户端21.5K keys/s），端点在单核上每秒生成约41,000个密钥对。

Looma把理论成熟的online/offline签名真正落地到云数据中心，全程仅依赖NIST组合内的hash基原语，且对应用透明。局限：明确限定于同一数据中心内的服务间通信（外部客户端连接除外）；需要持续密钥供应与1.24 TiB级存储/20分钟的KeyDist资源；回退路径与缓存管理增加系统复杂度。与KEMTLS、密钥卸载等方案相比，它更契合延迟敏感的mTLS云场景。

- **论文 PDF**: [Looma: A Low-Latency PQTLS Authentication Architecture for Cloud Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-f74-paper.pdf)

#### MinBucket MPSI: Breaking the Max-Size Bottleneck in Multi-Party Private Set Intersection.
- **作者**: Binbin Tu, Boyudong Zhu, Yang Cao, Yu Chen
- **方向**: 密码学与协议
- **解读**: 问题：MPSI/MPSI-Card 现有协议通信复杂度均随最大集合线性增长，而现实输入规模高度异构（如 IP 黑名单，大列表超 50 万条）。

方法：提出“对齐-比较”框架：集合最小方为 Leader，两两执行新协议 bMCRG 把成员关系转为随机特征值是否相等，再用 J-PEQT 得交集位向量，基数版用带置换的 JP-PEQT；平衡场景用 bOPRF+OKVS，非平衡用 bOPRF+FHE，J-PEQT 基于 JZSS（抗 T−2 合谋）或 TAHE。

结果：复杂度为“小集合线性、大集合对数”。5 方持大集合（2^20）、5 方持小集合（2^10）、单线程 10Mbps 时，MPSI（MPSI-Card）仅需 12.2 MB 与 129.86（130.05）秒；比 Wu et al.（USENIX Sec 2024）通信降 157x、提速 12.7x，比 Gao et al.（PETS 2024）降 76x。

我的思考：把非平衡 PSI 的“对数于大集合”效率带到多方并覆盖 MPSI/MPSI-Card；规模差越大优势越显著。局限：FHE 计算集中于 Leader 侧，半诚实模型。

- **论文 PDF**: [MinBucket MPSI: Breaking the Max-Size Bottleneck in Multi-Party Private Set Intersection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f182-paper.pdf)

#### Mobius: Enabling Byzantine-Resilient Single Secret Leader Election with Uniquely Verifiable State.
- **作者**: Hanyue Dou, Peifang Ni, Yingzi Gao, Jing Xu
- **方向**: 密码学与协议
- **解读**: 问题：单一秘密领袖选举（SSLE）被以太坊列入路线图，但现有 shuffle 方案存在可推广的“状态唯一性攻击”：恶意领袖生成多个可公开验证的节点列表状态分别发给诚实节点，破坏选举唯一性，高概率违反活性等安全性质。

方法：Mobius 采用“近似唯一随机化”：领袖预先对两个随机值（重随机化与置换）生成绑定且隐藏的承诺并附 NIZK，经带置信等级的 gradecast（GradeDelivery）分发，任何偏离都会暴露身份；配合瞬态锁与流水线解耦状态更新与承诺刷新，输出延迟仅 2Δ，UC 框架下证明安全。

结果：注册阶段链上与链下通信由 O(n²) 降至 O(n)；10 节点实测链上通信成本降 81%；n=5、f=2、1000 轮模拟中，唯一性保障强于 BEH+20、CFG22 等现有方案。

我的思考：发现并形式化可推广到全部现有 SSLE 构造的通用攻击，理论贡献扎实；“预承诺随机值+gradecast”机制巧妙。局限：恶意领袖只损失不可预测性而非完全失效；PoC 规模小，与以太坊集成仍需验证。

- **论文 PDF**: [Mobius: Enabling Byzantine-Resilient Single Secret Leader Election with Uniquely Verifiable State.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2407-paper.pdf)

#### MVP-ORAM: a Wait-free Concurrent ORAM for Confidential BFT Storage.
- **作者**: Robin Vassantlal, Hasan Heydari, Bernardo Ferreira, Alysson Bessani
- **方向**: 密码学与协议
- **解读**: 问题：仅加密不够，访问模式可被推断攻击利用；现有 ORAM 无法同时支持并发客户端与拜占庭容错——并行 ORAM 依赖锁或代理，限制并发且破坏容错。

方法：MVP-ORAM 是首个 wait-free 并发 ORAM：基于 Path ORAM，位置图与 stash 存服务端，并发客户端读写产生多版本结构随后合并，每次访问 3 轮往返；集成秘密共享与 BFT SMR，成首个 BFT ORAM；因 wait-free 与 collision-free 不可兼得，定义 μ(N,c,D) 弱化安全性，Strong 变体用 dummy 访问换标准安全。

结果：AWS 原型 10 服务器、50 并发客户端下 >350 次/秒（<140ms），单服务器 >700 次/秒（<70ms）；带宽 O((n+c)c·log N)、stash O(c·log N)，碰撞概率在 N=2^18 时 <0.0002%。

我的思考：首次实现 wait-free ORAM，论证其与 collision-free 不可兼得，理论价值高；弱化安全定义务实但依赖偏斜访问假设，带宽随并发二次增长，填补 BFT ORAM 空白。

- **论文 PDF**: [MVP-ORAM: a Wait-free Concurrent ORAM for Confidential BFT Storage.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1809-paper.pdf)

#### Practical Traceable Over-Threshold Multi-Party Private Set Intersection.
- **作者**: Le Yang, Weijing You, Huiyang He, Kailiang Ji, Jingqiang Lin
- **方向**: 密码学与协议
- **解读**:
门限MP-PSI揭示至少t个参与方共同持有的元素，但在取证、反洗钱等场景还需披露元素持有者。已有方案（Mahdavi等）仅抗t-2个半诚实参与方且要求特殊角色不共谋，复杂度O(m(nlog(m/t))^{2t})，n=10、t=7时超9小时。

本文提出两个协议：ET-OT-MP-PSI用Shamir秘密共享+OPPRF，零值份额更新保护非交集元素，分秘密共享、更新、收集重构三阶段；ST-OT-MP-PSI在份额更新阶段引入OLE三方交互，使重构还需足够多方持同一元素，把抗共谋从t-2提升到n-1。

实现用C++与NTL库（128位元素）：n=5、t=3、m=2^14时ET快15056倍、ST快505倍，n=10、t=5、m=2^7时快4312/637倍；ET在n=5、t=3、m=2^14仅1.73秒，重构随t指数增长。

我的思考：OPPRF+Shamir取代Paillier同态加密是效率跃升关键，同时消除"特殊角色不共谋"假设，ST推到n-1的OLE设计也干净。局限：仅半诚实模型、仅P0获输出、重构枚举仍是指数瓶颈、ST的O(n²mλ)通信开销显著。作为可追踪门限PSI的实用推进价值明确。
- **论文 PDF**: [Practical Traceable Over-Threshold Multi-Party Private Set Intersection.](https://www.ndss-symposium.org/wp-content/uploads/2026-s38-paper.pdf)

#### RoundRole: Unlocking the Efficiency of Multi-party Computation with Bandwidth-aware Execution.
- **作者**: Xiaoyu Fan, Kun Chen, Jiping Yu, Xin Liu, Yunyi Chen, Wei Xu
- **方向**: 密码学与协议
- **解读**: 安全多方计算（MPC）中跨方通信是主要瓶颈，但协议设计长期忽视通信不对称：如 ABY3 定点乘法中 P1/P2 需双向收发而 P3 只发不收，浪费高达约33%的全双工带宽，"锁死"了并行化收益。

RoundRole 把决定通信模式的逻辑角色与决定带宽的物理节点解耦：将协议切分为并行任务，为每个任务做角色到节点的置换映射；先 profiling 通信模式（最小二乘拟合系数），再解线性规划得最优负载分配比例，最后权衡聚合开销自动确定任务数。

集成于 ABY3（及 MOTION、4PC），九种协议×六种网络设置（100Mbps–10Gbps，含同构/异构）平均加速1.7倍、最高7.1倍；同构下 AB-Mul 最高2.8倍；100Mbps 下平均带宽利用率达100.0%（高34.3%）；ORAM/安全排序/逻辑回归加速1.3–1.5倍；在线优化开销约55毫秒。

协议无关的执行层优化，无需重新设计协议即"解锁"剩余性能，且不改变任何通信例程、保持原协议安全保证，自动达到手工对称化协议的效果；局限在于主要适用于半诚实、带宽瓶颈场景，恶意安全协议因延迟主导收益有限，且依赖通信量随输入线性增长的假设。

- **论文 PDF**: [RoundRole: Unlocking the Efficiency of Multi-party Computation with Bandwidth-aware Execution.](https://www.ndss-symposium.org/wp-content/uploads/2026-f52-paper.pdf)

#### Scalable Off-Chain Auctions.
- **作者**: Mohsen Minaei, Ranjit Kumaresan, Andrew Beams, Pedro Moreno-Sanchez, Yibin Yang, Srinivasan Raghuraman, Panagiotis Chatzigiannis, Mahdi Zamani, Duc Viet Le
- **方向**: 密码学与协议
- **解读**：**问题**：区块链拍卖（如 NFT）依赖智能合约完成投标与结算，但链上交易随投标人数量线性增长，导致网络拥塞、手续费飙升、确认延迟，无法支撑现实经济中大规模、高速的密封投标拍卖；同时拍卖还必须满足投标隐私、绑定、防篡改、活性、财务公平与不可交互等多重性质，且投标人之间不能相互通信。

**方法**：作者构建了一个由拍卖人（hub）协调投标人与智能合约的分阶段协议：诚实情况下拍卖完全在链下进行，仅当 k 个投标人偏离协议时才产生 O(k) 的链上复杂度，而状态通道、Rollup 与 SOTA 方案 Riggs-TC（CCS'23）在哪怕单个投标人偏离时也需 O(n) 链上成本。投标以 Merkle 树聚合提交、投标值对拍卖人也完全密封，结算阶段拍卖人给出 zk-SNARK 证明获胜条件成立；通过担保金实现财务公平与活性，并在 UC 框架下给出理想功能建模，辅以博弈论分析论证对理性参与者的稳健性。

**结果**：该协议是首个支持超过 1,000 名投标人的可扩展密封拍卖系统：在以太坊上唯一能扩展到 1,024 名投标人以上的方案，而 Riggs-TC 仅 20 名投标人就触及区块 gas 上限；链上合约与参与者除获胜者与成交价外学不到任何投标身份与金额信息。恶意投标人占比升高时 gas 线性增长（对应 O(k)），仅在约 80% 偏离率时才超过 Riggs 基准。

**我的思考**：把链上复杂度从 O(n) 突破到 O(k) 是实质贡献，首次让千级投标人拍卖具备经济可行性，对 NFT 定价与 DeFi 场景价值明确；局限在于依赖 zk-SNARK 可信设置、需要担保金与理性/半诚实模型假设，且评估基于私有 EVM 链，真实主网表现尚待验证。

- **论文 PDF**: [Scalable Off-Chain Auctions.](https://www.ndss-symposium.org/wp-content/uploads/2026-s410-paper.pdf)

#### Select-Then-Compute: Encrypted Label Selection and Analytics over Distributed Datasets using FHE.
- **作者**: Nirajan Koirala, Seunghun Paik, Sam Martin, Helena Berens, Tasha Januszewicz, Jonathan Takeshita, Jae Hong Seo, Taeho Jung
- **方向**: 密码学与协议
- **解读**：**问题**：PSI 协议只能回答"查询项是否在数据集中"，但反欺诈、反洗钱、医疗分析等场景还需要对命中标识符关联的标签数据做下游计算（如浮点 ML 推理），且数据分散在多个托管方、只能以密文委托云侧。labeled-PSI 会把明文标签泄露给查询方，circuit-PSI 需多轮重交互且不支持实数值运算，现有方案无法同时满足标签机密性、实值分析与分布式扩展。

**方法**：作者提出首个 CKKS 近似全同态加密的"加密标签选择与分析"（ELSA）协议：以虚拟湮灭函数（VAF）做同态等值判定实现标签选择，并设计了两个核心技术——弱域扩展多项式（wDEP）+ 铃形函数组合的新型 VAF 近似（给出误差、乘法深度与系数增长的闭式界），以及槽位分窗（slot-wise windowing）把 64/128 位标识符切分为小域并行处理；再以槽隔离（slot isolation）聚合标签密文、以 α-out-of-n 门限 CKKS 去除可信第三方假设。

**结果**：在真实欺诈数据集上端到端选择与分析在 65 秒内完成，较既有最先进方案取得最高 1.4×–6.8× 加速；wDEP 与分窗技术使协议可扩展到数十亿标识符-标签对且误报可忽略；支持 PSI 基数、quorum PSI、均值/加权和等下游函数，安全模型为半诚实（可容忍 α−1 个共谋方）。

**我的思考**：首次把"选择+实值下游分析"同时放进加密多方的统一协议，且把大域同态等值判定的精度与深度代价理论化，工程上针对 AML 场景有直接价值；局限是半诚实假设、要求发送方集合内标识符唯一、FHE 上"计算正确性验证"被显式排除，且 65 秒为特定规模下结果，实际部署的密钥管理与噪声预算仍需谨慎。

- **论文 PDF**: [Select-Then-Compute: Encrypted Label Selection and Analytics over Distributed Datasets using FHE.](https://www.ndss-symposium.org/wp-content/uploads/2026-f207-paper.pdf)

#### SoK: Cryptographic Authenticated Dictionaries.
- **作者**: Harjasleen Malvai, Francesca Falzon, Andrew Zitek-Estrada, Sarah Meiklejohn, Joseph Bonneau
- **方向**: 密码学与协议
- **解读**: 认证字典（AD）是密钥透明度、二进制透明度、可验证键值存储、完整性文件系统与区块链等关键系统的底层密码学原语，但相关研究长期分散于不同的信任模型、负载与 API 中，开发者难以比较、评估与部署，论文旨在首次系统化这一领域。

作者提出统一框架：以服务器、客户端、数据源、审计者、公共公告板五种角色刻画五种部署场景的信任模型；将文献中零散的安全定义提炼为值绑定、历史绑定、读写一致性三种并分析其逻辑关系；建立构造分类（可变、仅追加、就地追加）并对 30 余种方案做渐近复杂度分析。

结果揭示了尖锐的二分现象：所有已知方案要么查询与更新均为 O(log n)，要么某一操作为 O(1) 而另一操作退化为 O(n)，且更强信任假设并不自动带来更好渐近性能；结合内存检查器下界（Boyle et al.），论文指出同时实现亚对数查询与对数更新的构造仍是开放问题。

这是 AD 领域首个综合性框架，为标准化 API、模块化实现与跨方案公平比较奠定基础，并证伪了"更多信任换取更高效率"的工程直觉；但缺少实证基准，对 SNARK 辅助审计等工程权衡的讨论也有限，其价值更在于概念整理而非新方案。

- **论文 PDF**: [SoK: Cryptographic Authenticated Dictionaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1465-paper.pdf)

#### VDORAM: Towards a Random Access Machine with Both Public Verifiability and Distributed Obliviousness.

- **作者**: Huayi Qi, Minghui Xu, Xiaohua Jia, Xiuzhen Cheng
- **方向**: 密码学与协议
- **解读**: 可验证随机存取机（vRAM）是表达复杂计算并给出可证明安全保证的基础模型，应用于安全电子投票、财务审计与隐私保护智能合约。但现有 vRAM 无一同时提供公开可验证性与分布式 obliviousness（多个证明人既相互保密又对验证人保密）：单证明人 vRAM 要求证明人掌握全部秘密，现有 DORAM 基于 MPC、缺少公开验证。根本障碍是 MPC 与 ZKP 的范式错配——MPC 为最小化通信与计算而设计，ZKP 却要求完整静态计算轨迹作为 witness。

方法上，本文提出 CompatCircuit，据作者所知首个多证明人 ZKP 前端：将协作 zkSNARKs 与新型 MPC 集成，把安全计算与 R1CS 约束验证统一为单一电路范式，特别处理比较、等值、按位操作等难以对齐的功能。在此基础上构建 VDORAM，即首个公开可验证的分布式 oblivious RAM，协调在线 MPC 通信延迟与离线证明生成复杂度。实现约 15,000 行 C#（CompatCircuit 约 8,500 行），采用 BLS12-377 曲线与 Plonk。

微基准显示：相比单证明人基线，2 证明人配置计算开销增加最多约 20 倍（Beaver 乘法共享揭示与位分解协议），随证明人数增至 4/8/16 增幅趋缓；作者以微基准、对比分析与程序示例验证实用性，代码开源。

我的思考：本文填补了"公开可验证 + 分布式 obliviousness"的空白，统一 MPC/ZKP 表达的方法论价值高于单个协议本身，为多证明人 ZKP 前端指明可行路线。局限：2 证明人 20 倍开销说明范式桥接代价不菲，实用性依赖证明人规模与应用场景；实现未并行化，限制更大规模基准的可比性；与成熟 zkVM 的端到端对比不足，具体应用完整案例有待展示。作为开创性工作，其意义在于划定可行边界，性能优化与工程落地是后续主战场。

- **论文 PDF**: [VDORAM: Towards a Random Access Machine with Both Public Verifiability and Distributed Obliviousness.](https://www.ndss-symposium.org/wp-content/uploads/2026-s16-paper.pdf)

#### WBSLT: A Framework for White-Box Encryption Based on Substitution-Linear Transformation Ciphers.
- **作者**: Yang Shi, Tianchen Gao, Yimin Li, Jiayao Gao, Kaifeng Huang
- **方向**: 密码学与协议
- **解读**: 白盒威胁模型下攻击者可完全控制加密实现，先前白盒加密只保护单一密钥相关表，攻击者把表分组复合后即可用 BGE、LRDRP、MGH、BS 等代数攻击恢复密钥。

方法上，WBSLT 面向 SLT（代换-线性变换）类密码：密钥嵌入表以线性/非线性变换保护，并通过特殊加法网络将部分组件的计算留给后续组件，使轮与表边界模糊化，令攻击者无法确定攻击边界；再叠加掩码、洗牌与外部编码抵御 DCA/DFA，全部实现为查表。

实验显示跨平台加密速度 0.01–7.90 MB/s，优化/通用版 WB-AES-128 分别达 0.18–3.29 与 0.06–0.97 MB/s，表体积 9.78/38.37 MB；DSASN 结构值从同类方案的 5 提升到 50；对四类攻击各 1 万实例共 4 万次攻击全部失败；可集成进 86.96%/82.61% 主流 IoT 网关。

我的思考："模糊轮边界"对抗表分组攻击的思路新颖，WBD/WBA 与 DSASN 量化分析扎实；但白盒密码缺乏形式化证明，"免疫"仅针对已知攻击，近 40 MB 表开销对受限设备仍是负担，能否经受新攻击考验尚待观察。

- **论文 PDF**: [WBSLT: A Framework for White-Box Encryption Based on Substitution-Linear Transformation Ciphers.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2492-paper.pdf)

#### ZKSL: Verifiable and Efficient Split Federated Learning via Asynchronous Zero-Knowledge Proofs.
- **作者**: Yixiao Zheng, Changzheng Wei, Xiaodong Qi, Hanghang Wu, Yuhan Wu, Li Lin, Tianmin Song, Ying Yan, Yanqing Yang, Zhao Zhang, Cheqing Jin, Aoying Zhou
- **方向**: 密码学与协议
- **解读**: **问题**：纵向联邦学习（Split VFL）现有工作几乎只关注数据隐私（差分隐私、同态加密），忽视恶意参与方操纵本地计算发起完整性攻击——重用陈旧 embedding、伪造梯度、投毒，或主动方注入偏置梯度/植入后门。将零知识证明（ZKP）集成进训练可保证计算可验证，但把深度模型训练整体编码为单一 ZKP 电路不现实：电路设计复杂且参数频繁承诺开销大、embedding 层证明生成昂贵、同步证明会阻塞迭代训练。

**方法**：ZKSL 基于 PLONK（Halo2 实现）将 DNN 切分为逐层电路并行生成证明；提出轻量扩展 PC-PLONK，用隐私承诺列 P + 置换约束把承诺绑定移到多项式承诺层，避免电路内哈希，实现低成本逐轮参数承诺与层间输入输出一致性；embedding 层采用查找表 + 随机投影的概率验证（内积压缩 v_i=E_i·r，定理保证错误被发现的概率至少 1−1/p），将证明复杂度从 O(Nnd) 降至 O(nd)；再以异步 compute–prove 调度（Prove-Queue + K 窗口验证，失败回滚）解耦证明生成与训练迭代。安全性在恶意模型（含共谋）下以统一实验定义计算完整性、更新可验证性与公开可审计性，并归约到 PLONK 可靠性与承诺绑定。

**结果**：单节点 LeNet 证明时间从 ZKML 118.5s / KAIZEN 123.9s 降至 ZKSL 25.32s、ZKSL-LP 11.89s；AlexNet 与 VGG11 分别降至 98.13s 与 213.7s；批大小 N=8 时较 KAIZEN 减少 73.3%、较 ZKSL 再降 52.4%；相对 ZKML 前向/梯度/反向分别提速 3.6×/4.7×/5.1×。整体证明生成时间最多降低 73%，同时保持 99.4% 的模型准确率。原型为 2 万余行 Rust，支持 ONNX 导入、GPU 加速与 gRPC 联邦通信。

**我的思考**：ZKSL 首次把 Split VFL 训练做成恶意威胁模型下端到端可验证（覆盖主动方、被动方及共谋），逐层并行证明、PC-PLONK 与异步调度三项工程优化直击 ZK 训练的真实瓶颈，73% 的开销削减使"可验证训练"向实用化迈出坚实一步，形式化安全归约也较规范。局限：数据真实性依赖训练前的外部承诺（先于承诺的伪造不在威胁模型内）；概率验证引入 1/p 级误差；评估仅覆盖 LeNet/AlexNet/VGG11 与 DeepFM 等中小模型（单轮证明仍以分钟计），与 KAIZEN/ZKML 的可比性受限于其集中式设置与扩展实现。
- **论文 PDF**: [ZKSL: Verifiable and Efficient Split Federated Learning via Asynchronous Zero-Knowledge Proofs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2008-paper.pdf)

### 隐私保护（20 篇）

#### A Unified Defense Framework Against Membership Inference in Federated Learning via Distillation and Contribution-Aware Aggregation.
- **作者**: Liwei Zhang, Linghui Li, Xiaotian Si, Ziduo Guo, Xingwu Wang, Kaiguo Yuan, Bingyu Li
- **方向**: 隐私保护
- **解读**: 联邦学习虽不直接共享原始数据，但成员推断攻击（MIA）可借模型输出或参数推断某样本是否在训练集中。现有防御要么显著损害模型效用（如DP-SGD），要么只针对被动或主动攻击单一方面，缺乏统一防线。

作者提出三组件统一框架：①教师模型训练中引入改进熵正则（对"自信正确"与"自信错误"都施加惩罚，避免标准熵失效），增大成员样本的预测不确定性；②用CVAE生成类别条件带标签合成数据做监督式学生蒸馏，避免暴露原始数据且优于无标签蒸馏；③贡献感知聚合——服务端在保留验证集上评估每个客户端更新的效用贡献，动态加权、剔除负贡献乃至最差客户端，抑制恶意更新。

在Location30、Purchase100、Texas100、Cifar10四个数据集上，6种攻击（Prediction、Bias、Entropy、LiRA、AgrEvader、AMP）的准确率全部被压至约50%（接近随机猜测），同时模型精度几乎无损（Purchase100保持79.78%、仅降0.54%）；全面优于Memguard、DP-SGD、DMP、HAMP、MIST、LoDen、Mesas等7种SOTA防御，且每轮训练仅4.91秒、占用1396MB，效率领先。

我的思考：同时抵御被动与主动攻击、且效用损失最小，是其核心贡献；消融清晰显示三组件互补——熵正则主防熵类攻击、CVAE蒸馏覆盖面广、贡献感知聚合专克主动投毒，三者合体后所有攻击低于50.20%；局限在于假设服务器持有验证集、实验以IID与10客户端为主（虽扩展至500客户端），真实非IID、异构场景下的稳定性仍待验证；熵正则思想源自Song等人的工作，贡献感知聚合与鲁棒聚合（如Krum）的对比也值得深挖。

- **论文 PDF**: [A Unified Defense Framework Against Membership Inference in Federated Learning via Distillation and Contribution-Aware Aggregation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s413-paper.pdf)

#### ANONYCALL: Enabling Native Private Calling in Mobile Networks.
- **作者**: Hexuan Yu, Chaoyu Zhang, Yang Xiao, Angelos D. Keromytis, Y. Thomas Hou, Wenjing Lou
- **方向**: 隐私保护
- **解读**: 移动运营商（MNO）被指泄露或售卖用户位置与通信记录；匿名接入方案（USENIX'21、NDSS'24 等）能隐藏电话号码/SUPI，但缺少身份透明与位置感知，无法支撑呼叫路由、计量与计费，难以落地。
ANONYCALL 提供两项功能：匿名被叫发现——通过带外认证（数字证书或可验证凭证 VC）让被叫的认证代理按策略把临时 SIP URI 分享给通过认证的主叫，IMS 用该 URI 路由原生 VoLTE/VoNR 呼叫；用量计费——用 SPS-EQ 自适应盲签名、同态 Pedersen 承诺与 ZKP 构造匿名但可追责的余额凭证，正常用户不可链接，双花时可解匿名。
与标准 IMS（Kamailio）+5GC（Open5GS）原型实测：认证代理流程 50–180ms，VC 展示 650–780ms，SIP URI 获取约 80ms，总延迟低于 200ms；Linphone 呼叫匿名 UE 成功且 SIP 消息只含临时 URI；凭证不可伪造，双花解匿名可行（id 碰撞概率约 2^-218）。
巧妙复用现有 IMS 机制（临时 SIP URI 绑定 IMS IP）实现“匿名可呼”，无需新实体、仅归属网络改动；局限在于 10 分钟 URI 刷新窗口内仍可被好奇 MNO 关联、依赖既有匿名接入方案、双花检测需在线记录；对隐私移动通信的工程化有示范意义。

- **论文 PDF**: [ANONYCALL: Enabling Native Private Calling in Mobile Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1064-paper.pdf)

#### Augmented Shuffle Differential Privacy Protocols for Large-Domain Categorical and Key-Value Data.

- **作者**: Takao Murakami, Yuichi Sei, Reo Eriguchi
- **方向**: 隐私保护
- **解读**:
Shuffle 差分隐私通过中间洗牌器打乱数据实现隐私放大，精度优于本地模型；但多数协议易受两类攻击：收集者与用户合谋（10% 用户合谋即可使 ε 从约 1.1 升至 8.3）和数据投毒。此前增强型洗牌协议 LNF 用户端不加噪、由洗牌器抽样加哑元，可同时抵御两类攻击，但开销随域大小 d 线性增长，d=10⁹ 时约需 100 太比特通信和 3 年运行时间，无法用于大数据域。

作者提出 FME（Filtering-with-Multiple-Encryption）协议：用户同时发送哈希值与三重加密的输入数据，洗牌器对哈希值做增强洗牌，收集者据此过滤出热门项（阈值化 + 上限 l 个），在不解密内容的情况下把未选中项替换为 ⊥ 并回传，洗牌器二次增强洗牌后估计频率。多重加密防止双方比较三轮密文泄露信息，把交互压缩为单轮并证明满足计算 DP。对 KV 数据提出 TKV-FK 技术，将键值对变换为一维数据并按 key 级过滤以消除均值估计偏差。

理论分析与实验表明：与 LNF 相比，FME 在 d=10⁹ 时把通信从约 100 太比特降到 260 吉比特、运行时间从约 3 年降到 1 天，复杂度优化到 O(n+√(ld))；精度与 LNF 几乎一致（预算翻倍），投毒鲁棒性上界 G_max=λ(1-f_T) 与 ε 无关，优于随 ε 波动的现有 KV 协议。实验覆盖 Foursquare（d=10⁶）、AOL（d=2²⁴）等四个真实数据集并与 12 个协议对比。

把"过滤"而非"哈希计数"作为哈希函数用途，并用多重加密在单轮内实现过滤，是本工作最核心的创新——据作者所述，这是首次用多重加密在单轮交互下提供 DP。局限是要求洗牌器与收集者半诚实且不共谋（仍需 TEE 或 MPC 兜底），过滤假阳性 α 与漏选项带来微小偏差；整体为大规模真实场景的洗牌 DP 落地扫清了障碍。

- **论文 PDF**: [Augmented Shuffle Differential Privacy Protocols for Large-Domain Categorical and Key-Value Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1124-paper.pdf)

#### Cascading and Proxy Membership Inference Attacks.
- **作者**: Yuntao Du, Jiacheng Li, Yuetian Chen, Kaiyuan Zhang, Zhizhen Yuan, Hanshen Xiao, Bruno Ribeiro, Ninghui Li
- **方向**: 隐私保护
- **解读**: **问题**：成员推断攻击（MIA）评估模型泄露训练数据成员信息的程度，是隐私审计的核心工具。现有自适应攻击忽视实例间成员依赖，非自适应攻击缺少有效的代理策略。

**方法**：提出攻击无关的级联攻击（CMIA）：基于 Gibbs 采样的联合成员估计理论，通过条件影子训练迭代利用成员依赖——先用基础攻击识别高置信成员，再以其推断结果训练新影子模型改进其余实例；非自适应场景提出代理攻击（PMIA）：按全局/类别/实例粒度选择与查询行为相似的代理样本，构造成员后验优势比检验，并给出理论分析。

**结果**：在 6 个数据集、5 种模型架构上，CMIA 全面提升各类攻击性能，如 MNIST 上把 LiRA 在 0.001% 假阳性率下的真阳性率提升超 5 倍；PMIA 显著优于全部现有非自适应 MIA 且保持高效率。

**我的思考**：形式化区分自适应/非自适应场景并把实例间成员依赖引入攻击是新颖角度，低假阳性率区域正是隐私审计关注的重点，此处提升有实际意义；局限是 CMIA 迭代重训影子模型开销大，两类攻击都假设可访问同分布数据，差分隐私等防御可显著压制。未来可探索依赖建模的加速方法。
- **论文 PDF**: [Cascading and Proxy Membership Inference Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s661-paper.pdf)

#### Cease at the Ultimate Goodness: Towards Efficient Website Fingerprinting Defense via Iterative Mutual Information Minimization.
- **作者**: Rong Wang, Zhen Ling, Guangchi Liu, Shaofeng Li, Junzhou Luo, Xinwen Fu
- **方向**: 隐私保护
- **解读**: **问题**：Tor 匿名性受网站指纹攻击（WFA）威胁。现有防御两类各有短板：特征变形类依赖静态攻击模型、泛化差；特征压制类带宽开销失控，且对抗训练下残留的流量与标签互信息（信息泄漏）使防御失效。

**方法**：FRUGAL 把最小化流量特征与标签间的互信息（MI）作为优化目标——信息论上等价于最大化标签条件熵，直接增大攻击者分类不确定性；将流量混淆建模为马尔可夫决策过程（MDP），用强化学习求解，在累计 MI 削减贡献最大的位置策略性注入假包，在带宽预算内最大化削减。

**结果**：闭世界下 30% 带宽开销（BWO）把 DF 模型 ASR 降至 2.68%，优于 Palette（11.54% 但需 87% BWO）；BWO 提至 80% 时 ASR 低于 1%；对抗训练后仍保持 9.42%，而 Palette 反弹至 20.27%；在闭世界、开世界与真实模拟场景均验证有效。

**我的思考**：从"欺骗特定攻击模型"转向信息论层面消除指纹，视角更根本，带宽效率突出且抗对抗训练有数据支撑；局限是 RL 训练与部署开销未充分讨论、假包注入可能带来延迟影响、真实流量保真度仍需考察。该思路可与既有防御融合，并为 WFD 提供统一理论基准。
- **论文 PDF**: [Cease at the Ultimate Goodness: Towards Efficient Website Fingerprinting Defense via Iterative Mutual Information Minimization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f786-paper.pdf)

#### Connecting the Dots: An Investigative Study on Linking Private User Data Across Messaging Apps.

- **作者**: Junkyu Kang, Soyoung Lee, Yonghwi Kwon, Sooel Son
- **方向**: 隐私保护
- **解读**: 
消息应用普及（Telegram约9.5亿、KakaoTalk约4870万月活），联系人发现、SSO关联、附近搜索等功能带来跨应用关联私人信息的风险，此前研究不足。韩国84%的抽样Telegram/WhatsApp/Signal号码持有者同时注册了KakaoTalk。

方法上提出三个攻击并链式组合：(1)通讯录发现滥用——枚举010手机号批量查询四应用头像与用户名；(2)SSO token利用——爬取85,053个认证页面，用正则定位暴露在URL、cookie、storage中的OAuth令牌；(3)Tinder位置推断——用"指定GPS位置返回附近头像"的距离预言机，四方向二分搜索把区域缩小到平均半径324m。链式攻击包括经KakaoTalk去匿名化与Tinder位置追踪。

结果：KakaoTalk账号年龄加活跃度策略存在漏洞——90天以上高活跃账号经block/unblock循环可无限注册（约18,376个/天），10天从155,147个号码关联105,366个用户（67.9%）；对比Telegram仅100次/天、WhatsApp约7,000/天、Signal初始5万后144/天。63个网站暴露token：31个在URL、26个在cookie（24个无HttpOnly、22个无Secure）、16个网站的第三方脚本主动收集，可访问电话、头像、真名。位置推断40次查询达324m误差（前人676次达371m）。88个Telegram用户中70个（79.55%）被识别，追踪两名作者误差336m与418m，整次约52分钟、0.8美元。

我的思考：攻击链设计精巧、量化扎实，论证"主导平台即万能关联键"的结构性风险，OAuth token暴露也是被低估的生态级问题。局限：韩国环境特殊（号码空间小、KakaoTalk覆盖94%人口），外推需谨慎；实验仅用作者自身数据。影响：为消息应用隐私威胁模型提供跨平台视角，严格查询限流与默认关闭头像可见性是低成本高收益的缓解。

- **论文 PDF**: [Connecting the Dots: An Investigative Study on Linking Private User Data Across Messaging Apps.](https://www.ndss-symposium.org/wp-content/uploads/2026-s556-paper.pdf)

#### Convergent Privacy Framework for Multi-layer GNNs through Contractive Message Passing.

- **作者**: Yu Zheng; Chenang Li; Zhou Li; Qingsong Wang
- **方向**: 隐私保护
- **解读**: 问题：图神经网络（GNN）对敏感图结构（边、节点）的隐私保护通常采用扰动消息传递方案，但现有方法（如 Usenix Security'23 的 GAP）的隐私预算随层数 K 线性增长，多层 GNN 需要注入过量噪声，严重损害效用；而多层 GNN 在长程交互图上性能显著更优（深度增加可使精度从 72.5% 提升至 88.2%），线性隐私成本成为深层私有 GNN 的瓶颈。

方法：作者观察到过平滑现象使深层 GNN 的节点表示趋于同质化、天然具有压缩性（contractiveness），据此将多层扰动 GNN 重铸为压缩噪声迭代（CNI）过程，利用隐私放大理论证明隐私预算随层数收敛而非线性增长：ε=O((1−C_L^K)(1+C_L)/((1+C_L^K)(1−C_L))·Δ²/σ²)。设计了可证明压缩的 Contractive Graph Layer（CGL）：X^(k+1)=C_L(α1ÂX^(k)+α2Mean(X^(k)))+βX^(0)，并构建含压缩聚合、隐私分配、隐私审计三模块的 CARIBOU 框架，同时支持训练与推理。

结果：在 9 个数据集（含链式长程数据集）上，EDP 与 NDP 均显著优于 DPDGC、GAP、PertGraph 等基线，如 ε=1 时 Computers 上 EDP 精度 92.0%（DPDGC 88.0%、GAP 87.2%），链式数据上最高超第二名 10% 以上，NDP 下是唯一在多数情形超过 MLP 的框架；噪声规模在 ε=4、K=128 时线性组合需 12.11 而收敛分析仅需 4.67；隐私审计中 LinkTeller 攻击 AUC 从 0.86–0.998 降至 0.500 以下。

我的思考：这是首次将 DP-GD 的收敛性隐私分析系统性地移植到 GNN，为"过平滑即隐私"这一反直觉现象提供了严格理论解释，理论成色较高；但安全模型要求只暴露最终嵌入（GAP 的中间层拼接被放弃），且 CGL 的 C_L、α1、β 等超参对隐私-效用权衡敏感，链式数据上结果方差较大，实际部署需要仔细调参；与 GAP 的对比凸显了"隐藏中间状态+压缩层"的设计取舍，该思路对隐私保护的图学习研究具有方法论文献价值。

- **论文 PDF**: [Convergent Privacy Framework for Multi-layer GNNs through Contractive Message Passing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f255-paper.pdf)

#### In-Context Probing for Membership Inference in Fine-Tuned Language Models.
- **作者**: Zhexi Lu, Hongliang Chi, Nathalie Baracaldo, Swanand Ravindra Kadhe, Yuseok Jeon, Lei Yu
- **方向**: 隐私保护
- **解读**: 

成员推断攻击（MIA）对用敏感数据微调的 LLM 构成严重隐私威胁。现有黑盒 MIA 依赖置信度或 token 似然，但这些信号与样本内在属性（难度、稀有度）纠缠，信噪比低、泛化差，缺乏训练动力学的理论根基。

作者基于微调中"收益递减"现象提出优化差距（Optimization Gap）作为成员信号：收敛时成员样本几乎无剩余损失下降潜力，非成员仍有显著优化空间。为在黑盒下估计该差距，提出无训练的 In-Context Probing（ICP），用策略构造的输入上下文模拟一次微调行为，观测置信度提升量；两条探测策略：基于参考数据（语义相似公共样本）与自扰动（掩码或生成），后者无需任何辅助数据。

在 HealthcareMagic 数据集上 ICP-MIA 达到 AUC 0.942，超过 ReCaLL（0.847）与 Min-K%（0.837）；在 CNN-DM 上 TPR@1%FPR 达 0.518，比 ReCaLL（0.195）高 2.6 倍以上；三个任务、多个模型（LLaMA-3.2-3B/Instruct、Pythia-2.8B）上一致领先，且指令微调能放大优化差距（CNN-DM AUC 从 0.927 升至 0.965）。

首次将训练动力学形式化为可计算的 MIA 信号并给出理论支撑，实用性强（无需训练影子模型）。局限是需 token 级 logits 访问、在 Pythia 等弱上下文利用模型上显著逊于 SPV-MIA（CNN-DM TPR 0.144 对 0.602）；攻击效果对参考数据对齐度敏感，高精度（低 FPR）场景的优势是其最大卖点。

- **论文 PDF**: [In-Context Probing for Membership Inference in Fine-Tuned Language Models.](https://www.ndss-symposium.org/wp-content/uploads/2026-f892-paper.pdf)

#### Lightening the Load: A Cluster-Based Framework for A Lower-Overhead, Provable Website Fingerprinting Defense.

- **作者**: Khashayar Khajavi, Tao Wang
- **方向**: 隐私保护
- **解读**:
网站指纹攻击威胁 Tor 等匿名网络的元数据隐私。正则化类防御（如 Tamaraw）以固定填充规则塑造流量，可提供信息论保证但开销过大（对突发流量尤甚）；超序列类防御把流量隐藏到预定义模式中，有可证明性但只保护离线阶段见过的站点，未见站点的行为未定义。

论文提出统一框架：先用全局正则化速率保护早期流量，再用流式早时间序列分类器（改编自 ECDIRE）把在线流量指派到离线聚类生成的 (k,l)-多样匿名集，随后切换为该集合专用的轻量正则化参数。实例化为 Adaptive Tamaraw，逐簇分配填充参数并保留原 Tamaraw 的信息论保证，形式上界定了任何攻击分类器的最大精度（仅依赖匿名集大小与多样性）。

在 Sirinam 等（95 站点×1000 条流量）与 AWF/Rimmer（Top-100 站点×2500 条流量）真实 Tor 数据集上验证：高隐私模式下可将任何攻击者精度压至 30% 以下，实测精度落在理论界内；效率优先设置下总开销较经典 Tamaraw 最多降低 99 个百分点，k 参数可灵活调节隐私—效率权衡。

我的思考：用"自适应切换+可证明界"调和了正则化开销大与超序列不可泛化的两难，理论贡献扎实且实现开源可复现。实际安全性依赖早期分类器（Holmes+kFP）指派准确率，误指派将破坏界的前提；攻击者若针对分类器设计对抗流量，其影响尚待评估，动态匿名集重聚类的工程成本也值得进一步量化。
- **论文 PDF**: [Lightening the Load: A Cluster-Based Framework for A Lower-Overhead, Provable Website Fingerprinting Defense.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1760-paper.pdf)

#### OptiMix: Scalable and Distributed Approaches for Latency Optimization in Modern Mixnets.

- **作者**: Mahdi Rahimi
- **方向**: 隐私保护
- **解读**: 混合网络（mixnet）以延迟换匿名，高延迟使其只能服务邮件、加密钱包等容忍延迟的应用，匿名集随之缩小。此前 LARMix（NDSS'24）与 LAMP（NDSS'25）已尝试降延迟，但仅支持分层（stratified）拓扑、依赖简化模型与集中式信任（LARMix 复杂度 O(N⁵)），或靠排除节点换延迟且不保负载均衡（LAMP）。

OptiMix 用蝴蝶拓扑（W 翼×d 链×h 跳）统一建模级联与分层两类主流设计，提出四件套：①LONA 分布式节点排布协议——用 VerLoc 测延迟，链首节点由协作生成的均匀随机数选取（对掌控至多半数节点的对手可证明 unbiasability，系首次形式化定义），链内其余节点按最小延迟贪心填充，离线复杂度 O(N²)；②四种可调策略路由 SSR/LAR/GWR/GPR，用参数 τ∈[0,1] 在"偏向低延迟"与"均匀随机"间连续调节，且考虑全部节点；③LBA 负载均衡——迭代行/列缩放使路由矩阵双向归一，复杂度 O(N²)；④CRG 覆盖路由——客户端生成补充 dummy 流量使总路由分布对全局被动对手看似均匀，具备 (ε,δ)-差分隐私保证。

基于部署中的 Nym 网络与 RIPE Atlas 延迟数据评估：LONA 在级联拓扑下较 vanilla 排布降低至少 58% 传播延迟；叠加低延迟路由后最多降 88%（τ=0 时 LAR/GWR 约 15ms）；匿名/延迟综合指标上较 LAMP 有 2 倍改进，LBA 下较 vanilla 提升 6 倍、较 LARMix 提升 4 倍；匿名熵 SSR 始终≥5 bit、GPR 最坏≥2 bit，而 LAR 在 τ≤0.2 时趋零；GPR/SSR 的完全路径攻陷率在各 τ 下均低于 0.03。

我的思考：OptiMix 的通用性（统一级联/分层）、分布式无中心信任与 O(N²) 复杂度较前作是实质进步，LONA 的 unbiasability 形式化证明为节点排布安全性设立了基准。局限：评估以分析与模拟为主（2 万条/秒、50ms 混合延迟），未做真实部署验证；"4 倍提升"依赖熵/延迟比这一指标；GWR/GPR 在 τ=0 时仍有较高同国路径概率（0.22-0.30）。总体是 mixnet 低延迟化的里程碑一步。

- **论文 PDF**: [OptiMix: Scalable and Distributed Approaches for Latency Optimization in Modern Mixnets.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2680-paper.pdf)

#### PACS: Privacy-Preserving Attribute-Driven Community Search over Attributed Graphs.

- **作者**: Fangyuan Sun, Yaxi Yang, Jia Yu, Jianying Zhou
- **方向**: 隐私保护
- **解读**: 属性图上的属性驱动社区搜索（在满足 k-core 结构凝聚性下寻找属性分数最高社区）广泛用于社交推荐、学术合作等场景，但图外包给云服务器会同时泄露图属性、查询意图与搜索结果。已有隐私图搜索只支持无属性图或社区相似搜索，直接扩展需要大量复杂同态运算，效率不可接受。

PACS 设计两个安全结构：安全社区索引用非对称内积加密（AIPE）把社区与查询属性向量加密，云服务器经密文内积直接计算属性分数 f(C,W')；安全边表基于 BGN 同态加密，配合 PRF/PRP 隐藏社区标识与边，通过同态乘法过滤非目标边，返回加密结果而不暴露目标社区细节。系统采用两个半诚实且不共谋的云服务器，CS2 持 BGN 私钥辅助密文比较，用户本地解密。

在六个 Facebook 衍生真实数据集上，PACS 的搜索复杂度为 O(δ+num·t+n)，实现近毫秒级响应；加密索引体积约为明文索引的 4 倍，安全边表最大仅 4.20 MB；基于 CQA2 模型证明了 (L1,L2) 自适应选择查询安全性，泄露仅限图规模、查询模式等受限信息。

我的思考：首个面向属性驱动社区搜索的实用隐私保护方案，AIPE 算密文内积分数、BGN 过滤目标边，密码学选型与问题契合。局限：依赖半诚实非共谋假设；CQA2 模型仍泄露候选社区数量与分数列表；索引体积 4 倍于明文、一次性构建开销大；正文未给出具体毫秒数，与明文方案差距待量化。相比同类图加密工作，"查询-社区相关性"是新问题维度。

- **论文 PDF**: [PACS: Privacy-Preserving Attribute-Driven Community Search over Attributed Graphs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1586-paper.pdf)

#### PrivATE: Differentially Private Average Treatment Effect Estimation for Observational Data.
- **作者**: Quan Yuan, Xiaochen Li, Linkang Du, Min Chen, Mingyang Sun, Yunjun Gao, Shibo He, Jiming Chen, Zhikun Zhang
- **方向**: 隐私保护
- **解读**:
从观察数据估计平均处理效应（ATE）面临选择偏差与隐私泄露风险；已有DP方案或假设二元结局、或只保护部分属性、或用固定截断阈值缺灵活性。本文提出PrivATE，基于倾向得分匹配的隐私保护ATE估计框架。

方法按场景提供两级保护：标签级只扰动结局，样本级扰动全部属性——回归权重、倾向得分、处理变量（随机响应）、聚合结局分别加噪声分摊预算。核心是自适应匹配上限机制：把总误差分解为噪声方差与匹配偏差，闭式求解k*=√(ε·c·n₁·M₁/2)，动态平衡灵敏度与匹配精度，并扰动聚合值而非逐样本效果。

在IHDP、Lalonde、ACIC、Synth上与IPW-PP、SmoothDPM、DPCI、PrivSyn、AIM对比，全部数据集与预算一致胜出；标签级ε=0.5时误差即低于0.2，Lalonde上基线ε=3仍大于1而PrivATE低于0.2；且为纯ε-DP，基线仅为近似DP。

我的思考：把匹配上限从经验参数变为闭式求解的量是最扎实的贡献，样本级预算分摊设计自洽。局限：仅支持二值处理、误差系数c/h依赖人工选定、与近似DP基线对比有利己。填补了DP因果推断"全属性保护+低预算可用"的空白。
- **论文 PDF**: [PrivATE: Differentially Private Average Treatment Effect Estimation for Observational Data.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1350-paper.pdf)

#### PrivORL: Differentially Private Synthetic Dataset for Offline Reinforcement Learning.
- **作者**: Chen Gong, Zheng Liu, Kecen Li, Tianhao Wang
- **方向**: 隐私保护
- **解读**: 离线强化学习（offline RL）通过共享预采集的转移（transition）或轨迹（trajectory）数据集训练智能体，无需与环境的直接交互，已用于导航等关键场景；但已有研究（如ORL-Auditor）表明这类数据集面临成员推断等隐私泄露风险。现有合成方法要么无形式化隐私保证，要么不适用于具有时序依赖与高维特性的RL数据。本文提出首个面向离线RL数据集的差分隐私（DP）合成方法PrivORL，使数据集可安全发布用于下游训练与研究。

PrivORL包含两个变体：PrivORL-n用EDM扩散模型合成单条转移，PrivORL-j用扩散Transformer合成整条轨迹。两者都采用"公开数据预训练+DP-SGD敏感数据微调"范式；为应对离线RL对数据多样性的高要求，引入基于随机网络蒸馏（RND）的好奇心模块，用新颖度分数挑选高新颖合成数据替换训练批次中的部分真实数据。针对长轨迹，PrivORL-j将轨迹切成等长片段，用前一片段末转移作为条件输入，条件生成并拼接出完整轨迹。

在5个敏感数据集（3个Maze2D、Kitchen、MuJoCo）上，ε={1,10}时Maze2D域中基于PrivORL-n合成数据训练的智能体平均归一化回报达51.9和69.3，远超PGM、PrivSyn、PATE-GAN、PrePATE-GAN（ε=1时为11.7/3.4/2.0/3.2）；消融显示去掉预训练与好奇心模块分别使回报下降25.9%和16.3%（Maze2D-umaze，ε=10）。轨迹合成上PrivORL-j平均比DP-Transformer高出15.0和10.4回报，TrajScore达0.902（基线0.778）；白盒成员推断攻击下TPR@1%FPR仅约1.5%，接近随机猜测。

我的思考：本文把DP合成从图像/表格域系统性迁移到离线RL，并针对"多样性"这一RL特有痛点提出好奇心机制，思路新颖、消融充分。局限在于基准规模有限（Kitchen/MuJoCo性能仍远低于真实数据，MuJoCo未做轨迹级合成），ε增大时效用明显下降，扩散模型+DP-SGD的训练成本也不低。与DP-Transformer的对比表明，Transformer+片段拼接对长时序依赖的建模是轨迹级DP合成的关键方向，后续可探索更大规模与更紧的隐私核算（如PRV）。
- **论文 PDF**: [PrivORL: Differentially Private Synthetic Dataset for Offline Reinforcement Learning.](https://www.ndss-symposium.org/wp-content/uploads/2026-f149-paper.pdf)

#### Prεεmpt: Sanitizing Sensitive Prompts for LLMs.
- **作者**: Amrita Roy Chowdhury, David Glukhov, Divyam Anshumaan, Prasad Chalasani, Nicolas Papernot, Somesh Jha, Mihir Bellare
- **方向**: 隐私保护
- **解读**: LLM推理阶段提示词中的敏感信息（SSN、信用卡号、健康与财务数据）会暴露给专有LLM API，威胁主要来自模型拥有者；训练期隐私机制无法保护提示词，同态加密/安全多方计算方案单次推理耗时超过十六分钟，其他方案要么无形式化保证、要么需修改API。本文提出密码学启发的"提示词清洗器"（prompt sanitizer）概念并实现Prεεmpt，在不牺牲响应质量的前提下形式化保护提示中的敏感token。

Prεεmpt将敏感token分为两类：响应只依赖格式的（SSN、信用卡号等）用保形加密（FPE）——密文与明文同格式可无损解密还原；响应依赖具体数值的（年龄、薪资等）用度量本地差分隐私（mLDP），基于指数机制把值扰动到邻近值以保持数值语义（预算按token数均分）。系统通过NER做类型标注，用辅助字符串Ψ编码token间函数依赖（如年收入=月收入×12）以优化预算分配与一致性，且全程无状态（不存储会话信息，契合GDPR删除权）。

在翻译、RAG、长上下文问答、多轮金融问答四类任务上几乎不损失效用：GPT-4o英德翻译BLEU与未清洗几乎一致（Name 0.287→0.278），RAG数值比较与事实检索均达100%准确率，长上下文STS相似度0.934（优于Papillon的0.854），ConvFinQA在ε=1.0时中位相对误差0.0408；翻译BLEU全面超过Papillon（如英德Name 0.278 vs 0.175），并给出攻击优势上界e^{lε}+negl(κ)的正式证明。

我的思考：这是首个带形式化隐私保证的提示清洗系统，FPE与mLDP的二分法抓住了"格式敏感vs数值敏感"的实用本质，隐私游戏+泄漏函数框架严谨。局限是仅保护单token可推断的信息，上下文语义级泄露留作未来工作；mLDP扰动不可逆（数值token无法反清洗）、NER误检直接影响实际保护强度、用户需识别敏感类型。作为低开销、无状态、可部署的推理期隐私方案方向正确，后续可与语义级清洗、更强NER结合，并扩展到多轮对话的跨轮关联保护。
- **论文 PDF**: [Prεεmpt: Sanitizing Sensitive Prompts for LLMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1277-paper.pdf)

#### Revisiting Differentially Private Hyper-parameter Tuning.
- **作者**: Zihang Xiang, Tianhao Wang, Cheng-Long Wang, Di Wang
- **方向**: 隐私保护
- **解读**: 超参数调优（从多个候选运行中选出最优）会泄露额外隐私，但实践中常被忽略；Liu-Talwar 与 Papernot-Steinke 给出了通用私有选择协议的上界，一个悬而未决的问题是：该上界是否紧？

作者先构造输出空间仅3元素的实例，实测调优隐私代价2.96ε 与理论上界3ε 几乎重合，证明通用上界确实紧；但当基算法是白盒 DP-SGD 时，作者首次对"调优过程"做隐私审计（NTNV/NTCV/ETCV 三威胁模型，新增结合评分函数的代理），发现经验泄露下界明显低于理论上界。

审计显示最实际场景下 εL 甚至低于单次运行预算 εB，最强场景 εL≥εB 但仍显著低于 εU（εB=1 时 εU=1.86 vs εL≈1.19）；改用更细粒度的 f-DP/μ-GDP 刻画基算法后，上界从用 (ε,δ)-DP 建模时的16.5 收紧到 3.3，且适用于任意运行次数分布 ξ。

该工作厘清了"选择如何泄露隐私"这一被忽视的问题，改进上界意味着相同预算下可尝试更多超参配置、直接提升效用，并证明单射评分函数是最坏情形。局限在于改进主要适用于白盒 DP-SGD 类基算法，且理论上界与审计下界的差距尚未完全闭合。

- **论文 PDF**: [Revisiting Differentially Private Hyper-parameter Tuning.](https://www.ndss-symposium.org/wp-content/uploads/2026-s447-paper.pdf)

#### STIP: Three-Party Privacy-Preserving and Lossless Inference for Large Transformers in Production.
- **作者**: Mu Yuan, Lan Zhang, Yihang Cheng, Miao-Hui Song, Guoliang Xing, Xiang-Yang Li
- **方向**: 隐私保护
- **解读**: 大模型云推理中模型参数与用户数据的隐私至关重要，但 HE+2PC 等密码学方案开销巨大（如 CipherGPT 生成单个 token 需 25 分钟、90 GiB 流量），无法实时服务大规模 Transformer，亟需兼顾隐私与效率的实用方案。

论文提出三方威胁模型（模型开发者、云服务方、数据所有者），设计半对称随机置换机制：为每个权重矩阵生成行列随机置换对，仅首尾层置换信息需与数据所有者共享，保持计算语义完全等价（无损精度），并以距离相关性给出隐私泄露上界，结合轻量 TEE（仅 4 种运算进入 enclave）抵御模型提取与微调攻击。

在 6 类模型家族（4M 至 70B 参数）上 STIP 均达到 100% 无损精度（差异仅为浮点误差）；LLaMA2-7B 延迟 31.7 ms，吞吐较 Puma 提高约 0.67 与 110 万倍；70B 模型上 TEE 开销仅 9.38%；已部署于专有 70B 模型，三个月在线测试仅增加 12% 延迟且无隐私事件。

该工作以极低成本在隐私-效率-精度三角上达到 Pareto 最优点，工程价值突出；但其安全性依赖置换密钥管理与三方非共谋假设，隐私保证为启发式上界而非密码学可证明安全，与 Apple PCC 类方案的可审计性仍有差距。

- **论文 PDF**: [STIP: Three-Party Privacy-Preserving and Lossless Inference for Large Transformers in Production.](https://www.ndss-symposium.org/wp-content/uploads/2026-s35-paper.pdf)

#### The Role of Privacy Guarantees in Voluntary Donation of Private Health Data for Altruistic Goals.

- **作者**: Ruizhe Wang, Roberta De Viti, Aarushi Dubey, Elissa M. Redmiles

- **方向**: 隐私保护

- **解读**: 自愿捐赠健康数据对开发新疗法、流行病监测等医学研究价值巨大，但隐私担忧是主要障碍。隐私增强技术（PET）承诺的保证——数据过期、匿名化、目的限制、访问控制——以及验证机制（专家审计、自审计）能否真正提高人们的数据捐赠意愿，此前缺乏系统性实证。

作者在 Prolific 招募 494 名美国参与者开展情景（vignette）调查：随机呈现营利/非营利两类数据收集方、四种隐私保证陈述与两种审计陈述（含控制组），测量隐私期望与捐赠意愿，用逻辑回归建模并控制人口学混杂因素，辅以开放式回答的归纳编码与 17 次认知访谈。

受访者对非营利机构存在极高的先验隐私期望（控制组中 43%–76% 期待其提供各类保护），因此明确列出隐私保护对非营利机构影响甚微——即便不提任何保护也有 89% 愿意捐赠；对营利机构，隐私保证把期望从约 50% 提升至接近非营利水平；审计陈述对捐赠意愿影响有限，仅在"营利机构+目的限制审计"场景显著；受访者普遍对数据泄露不可避免持怀疑态度。

我的思考：首次系统比较四种核心保证与两种审计机制，用严谨方法指出了"隐私洗涤"风险与审计在用户感知层面的失效，对技术社区有警示意义。局限：基于自我报告存在隐私悖论偏差，Prolific 样本对全美人口代表性有限，单保证独立呈现也简化了现实组合场景。对 PET 研究者而言，仅实现技术保证不够，还需研究如何向终端用户有效沟通保证与审计的可信度，否则会出现技术有效但用户不信任的局面。

- **论文 PDF**: [The Role of Privacy Guarantees in Voluntary Donation of Private Health Data for Altruistic Goals.](https://www.ndss-symposium.org/wp-content/uploads/2026-s518-paper.pdf)

#### To Shuffle or not to Shuffle: Auditing DP-SGD with Shuffling.
- **作者**: Meenatchi Sundaram Muthu Selva Annamalai, Borja Balle, Jamie Hayes, Emiliano De Cristofaro
- **方向**: 隐私保护
- **解读**:
DP-SGD 是带形式化差分隐私保证的训练算法，传统按泊松子采样选批次，而洗牌（shuffle）因兼容现有流水线、计算开销低而日益流行；但洗牌下的紧致理论上界仍是开放问题，实践中常按泊松子采样的预算报告隐私保证，可能严重高估隐私。本文首次提出针对 DP-SGD(Shuffle) 的审计方法，度量理论与实际泄露之间的差距。

作者先审计简化机制 BGM（批量高斯机制）：基于 Chua 等人的最坏情况相邻数据集猜想，用 Neyman-Pearson 似然比作为最优区分函数，把审计做到接近紧致并推广到多轮；随后扩展到真实 DP-SGD，定义 Target-Canary、Partially-Informed、Worst-Case 三种威胁模型，用 (ε,δ)-DP 隐私区域与 Clopper-Pearson 置信区间估计经验泄露 εemp；并进一步审计部分洗牌与先批后洗两种常见变体，且不需要访问批采样器内部。

实验表明洗牌模型的经验泄露比泊松理论保证最多高 4 倍：如 Li 等人的 MNLI 私有 BERT 模型报告 ε=3，实际审计出 εemp=12.7；但差距随参数与威胁模型变化，大 batch（B=1000）与弱威胁模型下明显缩小；两种洗牌变体泄露更大（最多 10 倍），理论 ε=0.1 时部分洗牌 εemp=0.29、先批后洗 εemp=1.00，且 2.6% 的公开仓库采用先批后洗写法。

这是首个对洗牌型 DP-SGD 的审计，似然比审计在理想设定下紧贴 Chua 等下界，甚至暗示该下界可能转化为上界，理论价值突出；但审计依赖能任意注入梯度的强对抗且需 10^6–10^9 次观测，与真实部署泄露仍有距离，对 SOTA 模型隐私声明的"打假"意义大于对实际攻击者的刻画；对隐私工程界的直接警示是：报告泊松保证却用洗牌训练并不可靠，需要开发洗牌的紧致分析或转向可验证的采样方式。
- **论文 PDF**: [To Shuffle or not to Shuffle: Auditing DP-SGD with Shuffling.](https://www.ndss-symposium.org/wp-content/uploads/2026-f597-paper.pdf)

#### When Focus Enhances Utility: Target Range LDP Frequency Estimation and Unknown Item Discovery.
- **作者**: Bo Jiang, Wanrong Zhang, Donghang Lu, Jian Du, Qiang Yan
- **方向**: 隐私保护
- **解读**: 本地差分隐私（LDP）频率估计已部署于 RAPPOR、Apple 等，但哈希碰撞损害精度：RAPPOR 无误差界，Apple 的 Count-Mean-Sketch（CMS）参数固定、未优化通信-隐私-精度权衡；且现有协议仅适用已知域，新词、新 URL 等缺乏可扩展方案。

方法上，论文提出统一框架 GCMS：消息长度可调，将"纯 LDP 协议"与 Apple CMS 化为特例，给出通用方差表达式并修正 Apple 的方差错误；推出针对目标频段的 OCMS 参数优化；未知域用稳定性直方图+加密-洗牌-分析（ESA）框架，辅助服务器仅接触加密哈希消息。

相同通信与隐私下 GCMS 方差低于 CMS；OCMS 在 n=100 万、k=65536、m=1024、ϵ=4 下对全频段目标改善方差；通信由 O(m) 降至 O(s·log m)；未知域方案达接近中心 DP 的精度并获得洗牌放大。

我的思考：CMS 家族一次系统化、理论扎实的推进，统一框架、方差修正与参数优化形成闭环，工业界可直接采用；"聚焦目标频段"说明按需优化能显著提升效用。局限在于未知域方案以半可信辅助服务器换取效用，属信任权衡；实验多为合成参数验证，真实部署表现待检验。

- **论文 PDF**: [When Focus Enhances Utility: Target Range LDP Frequency Estimation and Unknown Item Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1397-paper.pdf)

#### WhiteCloak: How to Hold Anonymous Malicious Clients Accountable in Secure Aggregation?
- **作者**: Zhi Lu, Yongquan Cui, Songfeng Lu
- **方向**: 隐私保护
- **解读**: 安全聚合+输入验证（SAIV，如 ACORN、RoFL）能保护输入隐私并剔除投毒者，但都假设客户端身份已知；引入匿名凭证后，恶意客户端每轮换凭证即可重回聚合，匿名与跨轮问责冲突，在医疗、金融等场景尤为致命。

方法上，WhiteCloak 在 ACORN 之上扩展：每轮以匿名凭证参与，提交三个 Groth16 证明——注册 ID 合法性、凭证与身份绑定、未列入黑名单（经 HICIAP 聚合）；被标记客户的凭证连同输入与密钥一并移除且不暴露身份；多级黑名单（超阈值 q 封禁）容忍误判，支持解封与申诉；新 ZKP 精确定位错误密钥客户端。

在 SHAKESPEARE 联邦学习实验（500 客户端+25 敌手）中，仅增每客户端 1.42s 延迟与 0.03KB 通信（合计 8.01s/5.70KB），约占 ACORN 开销 0.34%/0.1%；开销与向量维度无关，CIFAR-10L 仍低于 1.8s/0.03KB。

我的思考：首个在匿名条件下实现跨轮问责的 SAIV，凭证绑定+黑名单证明组合精妙，以零知识调和匿名与问责，误判容忍与申诉体现工程成熟度。但开销随黑名单规模对数增长，安全性依赖 Groth16 可信设置；为合规敏感的联邦学习提供"可问责匿名"范式。

- **论文 PDF**: [WhiteCloak: How to Hold Anonymous Malicious Clients Accountable in Secure Aggregation?](https://www.ndss-symposium.org/wp-content/uploads/2026-s142-paper.pdf)

### 系统与操作系统安全（19 篇）

#### Automated Code Annotation with LLMs for Establishing TEE Boundaries.

- **作者**: Varun Gadey, Melanie Gotz, Christoph Sendner, Sampo Sovio, Alexandra Dmitrienko
- **方向**: 系统与操作系统安全
- **解读**:
可信执行环境（如 Intel SGX、ARM TrustZone）能隔离敏感代码、缩小 TCB，但确定哪些代码应放入 TEE 依赖深度人工审计，现有工具无法自动化；开发者常把整个应用迁入（如 Graphene、SCONE），导致 TCB 膨胀。本文定义"cryptex code"——直接使用密码学原语及其处理敏感输入/输出的全部逻辑——并以此作为安全敏感代码的判定标准。

作者构建了首个标注数据集：从 1070 个使用 OpenSSL 的开源 C 项目中收集 4010 个源文件，由专家按五步流程（过滤、识别密码学函数、雪球式前后向扩展、全局变量分析）人工标注，耗时超 1000 小时。LLM-CAL 将任务建模为行级二分类，用 QLoRA（4-bit 量化、秩 16、注入 WQ/WK/WV/WO 四个适配器）微调 Gemma-2B 等基础模型；输入序列融合局部上下文（前后各两行）、全局语义（函数调用图与代码属性图数据流）及元数据（行号、函数名、文件名）。

行级上 LLM-CAL 达到准确率 99.04%、精确率 99.40%、召回率 97.50%、F1 98.41%；函数级实现 684 个敏感函数全部识别、零漏报误报；TCB 缩减 81.20%，仅 0.52% 因误分类膨胀。三个案例验证泛化性：Bitcoin 签名工具、TZ-DATASHIELD 固件（NXP 密码库）与分布外 mbedTLS 代码；消融显示零样本 Gemma-2B F1 仅 52.85%，QLoRA 显著优于 DORA（88.43%）。

作为 TEE 边界标注的首个自动化工具，LLM-CAL 填补了"分割工具等人工标注输入"的空白，数据流视角的 cryptex 定义也具可操作性。局限是标注依赖 OpenSSL 生态、训练集偏斜可能带来对个别库的过拟合（Bitcoin 案例中把公开签名打印误判为敏感），且行级判断仍以"是否像密码学代码"为依据，对应用语义的理解有限；但 81% 的 TCB 缩减幅度展示了工程价值。

- **论文 PDF**: [Automated Code Annotation with LLMs for Establishing TEE Boundaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s709-paper.pdf)

#### Breaking Isolation: A New Perspective on Hypervisor Exploitation via Cross-Domain Attacks.
- **作者**: Gaoning Pan, Yiming Tao, Qinying Wang, Chunming Wu, Mingde Hu, Yizhi Ren, Shouling Ji
- **方向**: 系统与操作系统安全
- **解读**:
虚拟机逃逸的核心威胁是超管理器指针破坏（QEMU近五年CVE约23.9%最终如此），但主机侧可利用结构稀缺且被ASLR隐藏，现有利用框架难以迁移，利用高度人工化、逐例定制。

核心观察是虚拟化"弱内存隔离"：客户机不能读宿主机内存，宿主却可自由解引用客户机内存。据此提出CDA范式：把损坏指针经跨域gadget（执行GPA→HVA翻译、在主机留下客户机地址的代码点）重定向到客户机内存中的伪造对象实现能力升级，含CDAA（代码执行）、CDAI（信息泄漏）、CDAO（数据覆盖）、CDAC（块混淆）四变体。框架四阶段：CodeQL静态识别gadget库、按栈深度/堆布局匹配指针与gadget、痕迹引导模糊测试合成触发输入、拼装完整利用链。

QEMU与VirtualBox分别识别44/23个翻译函数、共772个gadget（8个家族）；堆上0x10～0x4000大小类指针残留全覆盖；15个真实漏洞（13个QEMU、2个VirtualBox，含CVE-2021-3682、Scavenger等难利用案例）全部利用成功，输入合成268～531秒、利用构建14.6～19.2分钟；失败仅因指针不被解引用或不可控。

把超管理器利用重心从稀缺主机结构转向客户机内存这一稳定可控立足点，是首个系统化跨域利用范式与自动化框架，普适性经15个漏洞验证，并指明SMEP/SMAP式隔离的防御方向。局限：前提是已有指针破坏原语；仍有少量人工堆风水校准；聚焦Type-2超管理器。

- **论文 PDF**: [Breaking Isolation: A New Perspective on Hypervisor Exploitation via Cross-Domain Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f376-paper.pdf)

#### Cross-Cache Attacks for the Linux Kernel via PCP Massaging.

- **作者**: Claudio Migliorelli; Andrea Mambretti; Alessandro Sorniotti; Vittorio Zaccaria; Anil Kurmus
- **方向**: 系统与操作系统安全
- **解读**: 问题：内核内存分配器仍是核心攻击面；SLAB_FREELIST_RANDOM、RANDOM_KMALLOC_CACHES 等缓解把同类对象从同一缓存中隔离，迫使攻击者转向跨缓存（cross-cache）攻击，但现有跨缓存方案被认为不可靠——SLUBStick 仅针对页表且被 SLAB_VIRTUAL 缓解，PSPRAY 仅限同缓存，此前也没有可靠解决跨缓存越界（OOB）的通用方案。

方法：作者提出 PCPLOST，深入分析 SLUB 分配器、每 CPU PCP（Per-CPU Pageset）列表与分区 free_area 之间的交互，用计时侧信道区分三种分配事件——fast（SLUB 现有 slab）、slower（rmqueue_pcplist 从 PCP 取页）、slowest（rmqueue_bulk 从 free_area 取批并分裂页面），首次利用"slower 与 slowest"的区分来引导页面邻接布局；通过 timing 对象与 persistent 对象交替的 probe-and-drain 循环，实现同阶与跨阶（nv>nt、nv<nt）两类跨缓存 OOB 布局，并利用 pivot 技术（篡改 msg_msg 长度字段）把 UAF/DF 时间性漏洞转为 OOB。

结果：在 Linux 6.8 上共 800 次运行（Wilson 95% 置信区间）中，同阶跨缓存成功率在空闲/有噪声/是否 CPU 固定等所有场景下均超过 90%；跨阶 nv>nt 在 CPU 固定时约 90%（不固定时降至 41%–55%），nv<nt 最低约 20%；该技术能绕过 SLAB_VIRTUAL（>90%）与 SLAB_FREELIST_RANDOM（通过 SLAB_SIZE+偏移量覆盖策略），且不依赖空闲链表指针泄漏故对 SLAB_FREELIST_HARDENED 也有效；6 个公开 CVE（CVE-2024-53141、CVE-2021-22555、CVE-2022-0185、CVE-2023-0461、CVE-2021-3715、CVE-2022-27666）均成功利用。

我的思考：该工作首次把跨缓存 OOB 攻击做到 90% 以上的可靠性，并揭示了 PCP 列表这一被忽视的分配器交互面，对"跨缓存攻击不可靠"的行业共识形成有力挑战；对 SLAB_VIRTUAL 的绕过尤其值得关注，说明仅做虚拟地址隔离并不足够；局限在于依赖本地非特权攻击者加计时侧信道、nv<nt 等非典型阶数组合成功率低，且作者建议的页分配器级按类型分域缓解（如 AUTOSLAB 思路）尚未进入主线内核，性能代价未知。

- **论文 PDF**: [Cross-Cache Attacks for the Linux Kernel via PCP Massaging.](https://www.ndss-symposium.org/wp-content/uploads/2026-f862-paper.pdf)

#### Demystifying the Access Control Mechanism of ESXi VMKernel.

- **作者**: Yue Liu, Zexiang Zhang, Jiaxun Zhu, Hao Zheng, Jiaqing Huang, Wenbo Shen, Gaoning Pan, Yuliang Lu, Min Zhang, Zulie Pan, Guang Cheng
- **方向**: 系统与操作系统安全
- **解读**: VMware ESXi 占据裸机虚拟化市场超 45%，其 VMKernel 自 6.5 起引入强制访问控制沙箱以约束 VM 逃逸等攻击，但闭源无符号，内部权限模型从未被系统分析过。

论文提出域控制结构导向分析：从内核日志错误字符串回溯定位权限检查函数，借 gs/fs 段寄存器访问链定位域控制结构，再经字段布局推断与堆对象边界验证重建结构；并基于 VMware Workstation 嵌套虚拟化与 DebugStub 构建结构感知调试框架（符号恢复、结构注入、特权上下文模拟），自动完成结构识别与 syscall 表重建。

分析重建出 197 个 Linux64_Syscall、284 个 VMKSyscall、388 个 VMKPrivateSyscall 条目，发现三类缺陷：开发者保留的高权限调试 syscall、十余个存在栈溢出/信息泄漏等内存安全问题的 syscall 处理函数（全部可由至多 3 个 syscall 触发）、以及无写保护无完整性校验的域控制结构；据此构造篡改 syscall 权限位与文件权限码两类原语，演示 DoS、持久后门与 /etc/shadow 凭据伪造三个攻击场景；14 个漏洞全部获 VMware 确认修复，共获 42,000 美元赏金。

该工作首次把 ESXi 内部访问控制机制"透明化"，逆向方法可复用，发现的 KASLR 熵不足、SMAP 未启用等问题也印证攻击面；但攻击前提是已具备任意代码执行（本质是纵深防御加固而非独立利用链），且四个人的工作量显示分析门槛较高，建议 VMware 对控制结构实施只读与完整性保护。

- **论文 PDF**: [Demystifying the Access Control Mechanism of ESXi VMKernel.](https://www.ndss-symposium.org/wp-content/uploads/2026-f700-paper.pdf)

#### DirtyFree: Simplified Data-Oriented Programming in the Linux Kernel.

- **作者**: Yoochan Lee, Hyuk Kwon, Thorsten Holz
- **方向**: 系统与操作系统安全
- **解读**: KCFI 普及后 ROP 失效，数据导向编程（DOP）成为内核提权主流思路，但传统 DOP 需要堆地址泄漏、任意读、任意写三级原语，条件苛刻，多数漏洞难以满足。

DirtyFree 提出"任意释放"原语：利用内存破坏部分覆写任意释放对象内的指针（极端时单字节即可），借 kfree() 不需显式 cache 参数的特性实现跨 cache 释放，把安全关键对象（如 cred）置为 use-after-free；再以 io_uring 的 IORING_REGISTER_PERSONALITY 精确喷射用户态 cred、以 setuid 程序分配 root cred 回收，写 /etc/passwd 完成提权，把多阶段 DOP 压缩为单步。

作者用 LLVM pass 做反向污点分析，从 43 个候选对象中确认 14 个任意释放对象，覆盖除 kmalloc-8 外的所有通用 slab cache；对 31 个真实 KCTF 漏洞（2023–2024）成功利用 24 个（其中 10 个无需信息泄漏），而传统 DOP 仅适用 6 个、DirtyCred 适用 14 个（在 slab-virtual 防御下无法利用 double-free）。

贡献在于把任意释放原语系统化并验证其通用性，同时提出两类缓解：隔离任意释放对象到专用 cache、在 kfree 前校验 cache 归属，二者各自即可阻断全部 24 个利用且开销可忽略（0.28% 与 -0.55%）；局限是 7 个失败漏洞与跨 cache 场景依赖，缓解措施（尤其 cache 校验）的工程化部署仍需内核社区权衡。

- **论文 PDF**: [DirtyFree: Simplified Data-Oriented Programming in the Linux Kernel.](https://www.ndss-symposium.org/wp-content/uploads/2026-f527-paper.pdf)

#### Eviction Notice: Reviving and Advancing Page Cache Attacks.
- **作者**: Sudheendra Raghav Neela, Jonas Juffinger, Lukas Maar, Daniel Gruss
- **方向**: 系统与操作系统安全
- **解读**: 
页面缓存攻击不依赖具体硬件，曾具有很高的时间与空间分辨率；但 2019 年起 mincore 系统调用被修补（只报告进程自有页数）后，仅剩的 Evict+Reload 型攻击时间分辨率极低，且逐出操作严重影响系统性能。作者认为页缓存攻击的实际问题比业界预期的严重得多。

作者把与 Linux 页缓存的交互系统化为四个原语：flush、reload、evict、monitor，并从这些原语推导出五种通用攻击技术：Flush+Monitor、Flush+Reload、Flush+Flush、Evict+Monitor、Evict+Reload。对每个原语，作者在完全最新的内核上找到了绕过现有缓解的机制：为 flush 在不同文件系统上系统回溯触发路径，为 reload 穷举分析所有接受文件描述符的系统调用，为 monitor 发现三条独立的内核路径，为 evict 提出随内存压力动态调整的扩展。

最快攻击 Flush+Monitor（基于 cachestat）跨进程隐蔽信道平均容量达 37.7 kB/s，误差低于 3×10⁻⁴%，比此前工作快 5.3 倍；跨进程击键时序与事件检测攻击达到 4kB 空间分辨率、0.8µs 时间分辨率，每秒超过 100 万次测量，比先前的 SOTA 高出 6 个数量级；top-100 闭集网站指纹攻击 F1 达 90.54%。

系统化的原语框架是核心贡献——它把散落的攻击手法统一归类，并证明针对性补丁（如 mincore 修复）无法根治泄漏，需要基础性的缓解方案。跨容器/跨进程的 UI 重定向、击键时序、shell 启动检测等场景显示了现实威胁。局限在于攻击要求受害者访问目标文件，逐出开销依然存在；与 CPU 缓存侧信道互补且硬件无关，为内核侧信道研究提出了新挑战。

- **论文 PDF**: [Eviction Notice: Reviving and Advancing Page Cache Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f6-paper.pdf)

#### EXIA: Trusted Transitions for Enclaves via External-Input Attestation.
- **作者**: Zhen Huang, Yidi Kao, Sanchuan Chen, Guoxing Chen, Yan Meng, Haojin Zhu
- **方向**: 系统与操作系统安全
- **解读**: 
TEE 的启动时远程证明只能验证初始代码与数据的完整性，无法检测运行期内存破坏攻击；控制流证明（CFA）方案大多缺乏具体验证方法，且可被数据-only 攻击绕过。作者观察到：所有控制流劫持与数据-only 攻击都始于一次"意外写入"——宿主提供给 enclave 的外部输入被篡改。

据此提出外部输入证明（External-Input Attestation）概念：证明所有写入 enclave 应用的外部世界输入（共享缓冲、中断、异常，涵盖 TOCTOU 与重放），维护一个外部输入度量（EIM），内容与顺序都纳入哈希；即使应用被攻破，特权证明环境也保证度量完整，验证者通过比对参考 EIM 断言状态转换符合预期，从而把控制流劫持等安全问题降级为软件崩溃等可靠性问题。当前版本限于验证者已知输入的应用。

原型在 AMD SEV-SNP 与 RISC-V Penglai 上实现，2 个 PoC 攻击验证了防御有效性；性能开销极小：CNN 训练 0.44%–5.89%，推理 0.53%–18.30%（LeNet-5/AlexNet），124M 参数 GPT-2 训练仅 0.47%，SQLite3 数据库负载 0.29%–1.38%，MBedTLS 密钥操作 0.06%–1.94%。

"内存破坏始于意外写入"这一观察把检测前置到输入侧，与 CFA/DFA 事后分析相比思路优雅，安全-可靠性降级论证也很干净。局限在于适用范围限制在验证者已知输入的应用（公开数据集或单用户场景），通用性有待扩展；双平台实现证明了可移植性，极低的开销使其具备实用落地潜力。

- **论文 PDF**: [EXIA: Trusted Transitions for Enclaves via External-Input Attestation.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2421-paper.pdf)

#### Fast Pointer Nullification for Use-After-Free Prevention.
- **作者**: Yubo Du, Youtao Zhang, Jun Yang
- **方向**: 系统与操作系统安全
- **解读**: 
Use-After-Free（UAF）是 C/C++ 中最危险的漏洞类别之一（2024 年仅 Chrome 就报告 84 个，其中 58 个高危），而现有防御在性能、内存与安全保证之间难以兼得。指针置空（PN）通过跟踪指针与堆缓冲区的对应关系并在 free 时置空悬垂指针，被认为最有部署前景；但精确定位"指针→缓冲区"的元数据查找代价高，且完全忽视指针存储位置的空间局部性。

本文提出 FPN，从两个角度改造 PN：一是按 2^N 对齐的区域组织影子内存元数据，指针存储时一次移位加一次查表即可常数时间得到元数据地址（此前 CAMP 仅计算指针列表地址一项就占 62.50% 开销）；二是按 2^M 对齐的块注册指针位置，利用"同一缓冲区的指针常存于相邻地址"的局部性（8 字节阈值下 30.96% 的新注册与已有注册相邻，128 字节时达 75.31%）去重注册，大幅降低内存开销；另用状态位影子表避免误置空与访问已回收页面，并用全局+区域两级锁支持多线程。

SPEC CPU 2017 上 FPN 性能开销几何均值 17.78%、内存 8.34%；SPEC CPU 2006 为 15.56%/9.18%，明显优于 CAMP（2017 上 56.37%/173.44%，且 omnetpp 内存耗尽无法运行）、DangSan、FreeSentry 等基线。安全方面，Juliet 测试集（UAF/DF/IF 各 Good/Bad 用例）100% 通过、零误报零漏报，14 个真实 CVE 全部成功阻止；Chrome 平均开销 18.32%（CAMP 为 36.97%），Nginx 多线程下也保持更低开销。

FPN 的"块-区域"粒度是聪明的工程权衡：用粗糙但守恒的对应关系换取常数时间寻址与注册去重，再以状态位兜底安全性，实现性能、内存、安全三者同时改善。局限：不追踪寄存器中的指针、需源码插桩、依赖 ASLR 等保证元数据不可见；与同样绕过精确追踪的 pSweeper 相比，FPN 真正减少了注册数量，这是其内存优势来源。
- **论文 PDF**: [Fast Pointer Nullification for Use-After-Free Prevention.](https://www.ndss-symposium.org/wp-content/uploads/2026-f753-paper.pdf)

#### HyperMirage: Direct State Manipulation in Hybrid Virtual CPU Fuzzing.
- **作者**: Manuel Andreas, Fabian Specht, Marius Momeu
- **方向**: 系统与操作系统安全
- **解读**: **问题**：云安全依赖 hypervisor 的隔离保障，而其虚拟 CPU 实现处于最高特权级、最易受攻击；Intel 定义 76 种 VM-exit，此前 fuzzing 工作仅覆盖 8 种（约 10%），因为手工构造架构合法的 VM 状态以触发特定 VM-exit 极其费力且难以扩展。

**方法**：HyperMirage 提出 Direct State Manipulation（DSM）：不再运行真实 VM，而是直接变异 HV 处理 VM-exit 时消费的 VM 状态——通用寄存器、VMCS 访客态、VM-exit 信息字段与访客内存（512 字节重复填充页面，输入小于 1KB），通过嵌套虚拟化由宿主导 hypervisor 注入，免除一切手工种子；覆盖引导基于 Intel PT 轨迹并集成 AFL++；首次把编译型符号执行引擎 SymCC 扩展到裸机：前端在目标 HV 内记录带占位符的符号操作，后端在宿主 QEMU 中由 QSYM 求解（symbolic record-and-replay）。

**结果**：在 Xen v4.20.0 与 KVM v6.15.0 上 24 小时 fuzzing，覆盖率较 SOTA 开源 fuzzer HyperPill 提升 189%（Xen）与 79%（KVM），VM-exit 接口覆盖数量多 200%；吞吐达 3,357 次/秒（Xen 混合模式），是 HyperPill 的 144×。共发现 11 个新 bug（Xen 9 个、KVM 2 个），其中 4 个安全关键：CVE-2023-46842（Xen 32 位 hypercall 续延致 panic）、CVE-2024-45818（Xen VGA 区域死锁）、CVE-2025-38351（KVM INVVPID 非规范 GVA 致 panic）、CVE-2025-38469（KVM schedop poll 内存泄漏，符号执行 25 分钟定位）；DSM 产生的崩溃输入中 72% 为架构非法假阳性，但可按类别快速剔除。

**我的思考**：DSM 直击"手工种子不可扩展"的根本瓶颈，把输入空间从可执行的 VM 程序解放为 HV 眼中的完整状态视图，是虚拟 CPU fuzzing 的范式转变；首个 bare-metal SymCC 混合执行也被证明对复杂约束（如多字段校验）有效；代价是 72% 假阳性与对嵌套虚拟化、Intel PT 的依赖，且架构非法输入可能漏掉"必须真实验证合法性"的 HV 行为，但 11 个被维护者确认的 bug 与 4 个 CVE 足以证明其实战价值。
- **论文 PDF**: [HyperMirage: Direct State Manipulation in Hybrid Virtual CPU Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1763-paper.pdf)

#### IsolatOS: Detecting Double Fetch Bugs in COTS RTOS by Re-enabling Kernel Isolation.
- **作者**: Yingjie Cao, Xiaogang Zhu, Dean Sullivan, Haowei Yang, Lei Xue, Xian Li, Chenxiong Qian, Minrui Yan, Xiapu Luo
- **方向**: 系统与操作系统安全
- **解读**: 

实时操作系统（RTOS）统治网络物理系统（超 22 亿嵌入式设备，QNX 2022 年已嵌入超 2.15 亿辆车），其内核读取同一用户空间内存多次而不验证一致性的 double-fetch 漏洞可致内核崩溃、提权乃至物理危害。但 COTS RTOS 内核专有、无源码，静态分析不可行；模拟器启发式（如 Bochspwn 用时间窗）误报率在抢占场景下超 87% 且开销巨大。

IsolatOS 是首个硬件支持的 COTS RTOS double-fetch 检测框架：重新启用 CPU 内核隔离（x86 SMAP 即 CR4 第 21 位、AArch64 PAN），内核访问用户内存即触发页错误；自定义故障处理器记录 PC、CPU 与目标地址等元数据并恢复执行（保证后续 fetch 仍可捕获）；再以系统调用生命周期判定同一系统调用内对同地址的多次访问为 double-fetch，从而区分抢占与多核并发导致的跨系统调用访问。

相比模拟类检测器（QEMU-TCG 平均开销 79.3×），IsolatOS 原生运行，运行时开销降低 79.3 倍；在 QNX、VxWorks、seL4 上共发现 43 个此前未知漏洞（41 个获厂商确认，获 2 个 CVE）；首次在 QNX procnto 6.6/7.0/8.0 内核发现 double-fetch 漏洞——该系首个公开披露的本地提权，影响量产汽车；Bochspwn 对同样负载产生 127.3GB 的 5 分钟追踪数据，IsolatOS 则轻量得多。

用现代 CPU 隔离特性替代源码与模拟器是精巧且可迁移的思路（误报根因中抢占占 18.7%，生命周期判定有效压低了它）。局限：需要对闭源内核二进制做静态分析以定位入口与上下文切换符，工程量与平台适配成本高；检测面限于内核从用户空间拷贝数据的那类系统调用。
- **论文 PDF**: [IsolatOS: Detecting Double Fetch Bugs in COTS RTOS by Re-enabling Kernel Isolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s568-paper.pdf)

#### LatticeBox: A Hardware-Software Co-Designed Framework for Scalable and Low-Latency Compartmentalization.

- **作者**: Zhanpeng Liu, Chenyang Li, Wende Tan, Yuan Li, Xinhui Han, Xi Cao, Yong Xie, Chao Zhang
- **方向**: 系统与操作系统安全
- **解读**:
现代系统依赖隔离来安全执行不可信组件（第三方驱动、JIT 代码、Wasm），但现有硬件隔离各有短板：Intel MPK 仅支持 16 个保护域、无特权指令 WRPKRU 可被不可信代码滥用、每次权限切换需序列化内存屏障（数百周期），V8 等高性能引擎因此弃用 MPK；分页/页表切换与 SFI 则开销高、需编译器改写。

LatticeBox 提出软硬件协同框架，采用格基访问控制（LBAC）模型：权限与内存区域编码为紧凑的分层 N 位向量，元数据由线性降为 O(log N)，10 位标签即可支持数百个互不隔离的域；权限检查与更新退化为位运算，域切换单周期完成；新增 lp_land 指令原子提升权限并校验跨域控制流，杜绝未授权的间接调用，无需二进制扫描。

在 RISC-V BOOM 核上实现，域切换较 Intel MPK 快最高 180 倍；增强型 WebAssembly 运行时（wasm2c）仅 2% 性能损失，运行隔离 IPv6 内核模块的 ApacheBench 吞吐仅降 3%，支持比传统方案多一个数量级的隔离域。

我的思考：用格模型一举化解 MPK 的扩展性、时延与指令滥用三大痛点，安全语义由设计保证而非扫描补丁，思路简洁有力。代价是需定制 ISA 与微架构，商用依赖硬件与工具链生态改造；与 CHERI、EPK 等路线的量化对比、对真实浏览器/微内核的完整移植效果仍值得进一步验证。

- **论文 PDF**: [LatticeBox: A Hardware-Software Co-Designed Framework for Scalable and Low-Latency Compartmentalization.](https://www.ndss-symposium.org/wp-content/uploads/2026-f515-paper.pdf)

#### LinkGuard: A Lightweight State-Aware Runtime Guard Against Link Following Attacks in Windows File System.

- **作者**: Bocheng Xiang, Yuan Zhang, Hao Huang, Fengyu Liu, Youkun Shi
- **方向**: 系统与操作系统安全
- **解读**: Windows文件系统中的链接跟随（LF）攻击利用精心构造的符号链接链（junction与ObjSymlink组合），把良性文件操作悄悄重定向到受保护文件，可造成本地提权、信息泄露和永久DoS；截至2025年8月相关CVE已超1000个。攻击分单步与多步（配合机会锁的TOCTOU竞态）两类，而现有对策或需重型建模、或依赖Linux特性，均无法全面防护。

LinkGuard采用两阶段轻量设计：第一阶段在IRP层做动态主体过滤，只监控与链接链创建/跟随相关的跨主体文件操作（链接链由攻击者创建、由受害者程序跟随，主体必然不同），大幅降低监控开销；第二阶段将筛选出的操作构建为跨主体链图（CSCG），并用并行的FSM规则匹配检测状态迁移，一旦到达可攻击状态就在pre-operation阶段终止该IRP（返回ACCESS_DENIED并抑制分发）。

在5种代表性Windows系统上，LinkGuard对70个真实漏洞（55个程序）成功缓解全部26个单步攻击和42/44（95.45%）个多步攻击，总体97.1%；良性操作零误报；微基准平均仅1%开销，真实应用负载3.4%，良性文件操作延迟增加可忽略的5ms。两个未缓解案例均因链接链在LinkGuard未部署的隔离环境中预先构造，需物理访问，属威胁模型之外。

这是首个在Windows上同时覆盖单步与多步LF攻击的轻量运行期防护，其"跨主体过滤+FSM规则"的设计兼顾效率与可扩展性（新攻击只需增补规则）。局限包括O(n³)匹配复杂度依赖图规模有界控制、内核驱动的部署与兼容成本，以及FSM规则对未知攻击变体的覆盖边界。与Linux的protected_symlinks、Seaborn沙箱等相比，它更贴合Windows的描述符式访问控制模型。

- **论文 PDF**: [LinkGuard: A Lightweight State-Aware Runtime Guard Against Link Following Attacks in Windows File System.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2943-paper.pdf)

#### Losing the Beat: Understanding and Mitigating Desynchronization Risks in Container Isolation.

- **作者**: Zhi Li, Zhen Xu, Weijie Liu, XiaoFeng Wang, Hai Jin, Zheli Liu
- **方向**: 系统与操作系统安全
- **解读**: 容器隔离依赖namespace与cgroup的高度协同，但serverless等新范式强烈要求跨namespace资源共享，打破了二者生命周期的同步性，产生namespace-cgroup去同步（NCD）漏洞。攻击者可让共享IPC namespace的协同容器集体滥用超出限额的内存资源：一个容器大量分配后立即退出，其cgroup随容器销毁，残留虚拟资源不再被任何cgroup记账，实现资源耗尽乃至计费逃逸。

作者构建检测器系统化验证：Docker、Podman、Pouch与Kubernetes全部存在NCD风险，发现4个新CVE（如CVE-2024-3056，被Red Hat评为高严重性）和1个bug；GitHub上824,353个可容器化仓库中15.64%要求namespace共享（IPC占10.10%、网络占86.14%，其中88.68%的IPC共享来自深度学习NVIDIA MPS场景）；恶意镜像可在约23秒内耗尽宿主机8GB内存。防御上提出内核级方案CANs：给虚拟资源打上cgroup标签，用"气球"cgroup托管容器退出后的残留资源，通过资源转移与回收闭环恢复cgroup管控。

CANs开销极低：SPEC 2017平均0.47%、UnixBench平均0.33%，容器工具重启开销最大不超过2.4%，真实应用平均3.6%，且对主流工具完全兼容透明。

该工作首次系统化揭示并修复NCD这一被忽视的容器隔离裂缝，责任披露完整（四家社区均确认）。局限：user namespace的keys等资源因cgroup本身不记账而超出范围，mount namespace无法共享；内核级改动（v6.2.7/x86）的部署周期长，且依赖社区采纳（已提交Linux内核提案）。与pod级"监督cgroup"等临时方案相比，CANs从根因上统一了namespace与cgroup的职责。

- **论文 PDF**: [Losing the Beat: Understanding and Mitigating Desynchronization Risks in Container Isolation.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1381-paper.pdf)

#### Pallas and Aegis: Rollback Resilience in TEE-Aided Blockchain Consensus.

- **作者**: Jérémie Decouchant, David Kozhaya, Vincent Rahli, Jiangshan Yu
- **方向**: 系统与操作系统安全
- **解读**: TEE 辅助的 BFT 共识协议（如 Damysus）用可信组件提升弹性、降低通信开销，但可信组件崩溃、状态丢失或被克隆时易受 rollback 攻击，导致同一副本多次投票破坏安全性。现有防御要么把崩溃副本当拜占庭节点（副本数增至 3f+1），要么跨副本复制 TEE 状态（ROTE，同步开销巨大），且多只建模弱敌手。

Aegis 是首个面向 TEE-BFT 协议的高效视图同步器：通过会话机制保证每个副本每视图仅一个活跃 TEE 实例可投票，即使 TEE 崩溃重启或被克隆；join 请求先经共识批准、在会话边界由同步器统一激活，把"批准"与"激活"解耦以保证安全。其上构建的 Pallas 是首个在强敌手（静态拜占庭 + 可无限变化的 TEE 崩溃）下保持安全、无需复制 TEE 状态、副本数仅需 N=2F+1 的 BFT 协议，基于 Damysus+SGX 实现并在部分同步下给出活性条件。

AWS 地理分布评估显示：稳定场景下 Pallas 几乎无开销，吞吐最高比基线高 41%、时延低 29%；7 副本（f=0,u=3）频繁崩溃重加入下吞吐仅降 13%（13.6→11.78 Kops/s）；41 副本（u=20）重压下吞吐降 69%、时延升 200%，但对比协议直接失去活性；5% 消息丢失下性能仍优于其他方案。

我的思考：首个既高效又经证明抗 rollback 的 TEE-BFT 共识方案，安全性/活性论证与大规模工程验证都很扎实。局限：活性依赖恢复速率快于崩溃速率的 λ 假设；基于 SGX 实现而 SGX 自身有已知漏洞；重加入场景性能退化仍明显；缓解属驱动/协议层而非根本硬件隔离。与 ROTE、Flexi-BFT 等对比清晰，是 TEE 共识安全的重要进展。

- **论文 PDF**: [Pallas and Aegis: Rollback Resilience in TEE-Aided Blockchain Consensus.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2443-paper.pdf)

#### Pitfalls for Security Isolation in Multi-CPU Systems.
- **作者**: Simeon Hoffmann, Nils Ole Tippenhauer
- **方向**: 系统与操作系统安全
- **解读**:
嵌入式SoC把多颗CPU集成到一颗MCU，厂商常宣称"双核产品中一个核可为安全核"，但单核时代的内存保护单元复用到多核系统会失效——攻击者能否跨核攻陷并泄露受保护数据，本文首次系统评估。

方法上从内存、通信、外设、总线四维度归纳四类攻击向量：MPU策略失同步、无同步通信信道（共享内存TOCTOU）、非排他外设访问、糊涂代理外设（借总线主控外设读写单连接内存）。对11个双核MCU家族理论评估，NUCLEO-H755ZI-Q实测，STM32WB55与Galaxy Ring案例研究。

11个家族中6个易感（NXP、ST除WL5x、RP2040）；篡改FreeRTOS消息缓冲读写指针即获任意读写并升级为跨核代码执行，甚至穿透RDP/PCROP的TEE读取密钥；MDMA能读出M7独占的DTCM/ITCM秘密；STM32WB55网络栈已修复，Galaxy Ring的Zephyr隔离边界被击穿。

我的思考：有力证伪"多核即安全"话术，理论-实测-产品三级验证完整，还揭示FreeRTOS多核通信的安全隐患。局限：仅覆盖11个Cortex-M家族；软件对策只能缓解，总线级硬件防护需架构重设计，存量设备无法修复。
- **论文 PDF**: [Pitfalls for Security Isolation in Multi-CPU Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f971-paper.pdf)

#### SoK: Analysis of Accelerator TEE Designs.
- **作者**: Chenxu Wang, Junjie Huang, Yujun Liang, Xuanyao Peng, Yuqun Zhang, Fengwei Zhang, Jiannong Cao, Hang Lu, Rui Hou, Shoumeng Yan, Tao Wei, Zhengyu He
- **方向**: 系统与操作系统安全
- **解读**：**问题**：加速器 TEE（GPU/NPU/TPU/FPGA）为敏感数据与模型提供机密性、完整性与隔离，但绝大多数研究针对特定 CPU 或加速器、缺乏通用性，且没有构建加速器 TEE 的指南与各安全方案的优劣对比，导致设计经验难以迁移。

**方法**：作者对 2018–2025 年间 51 项研究（含 4 项产业设计：SEV-TIO、TDX Connect、H100、RME-DA）做系统化分析：归纳加速器 TEE 的典型框架，将其分为 Host-type（以 CPU 侧软件/固件保护）、Acc.-type（在加速器与 I/O 总线侧防护）与 Mix-type 三类；梳理从软件到物理的攻击向量；对三大安全机制——访问控制、内存加解密、认证（attestation）——逐项分类并给出洞见，最后分析 TCB 大小与兼容性两大部署因素。

**结果**：51 项研究中 41 项仅针对特定 CPU 架构、43 项仅针对单一加速器，通用性缺失严重；内存加密方面大量研究缺乏该机制、无法抵御物理攻击，且传统方案迁移与粒度不一致带来显著开销；认证方面多数方案缺少保证正确性的关键支撑；TCB 分析显示大多数加速器 TEE 有不可忽视的访客/系统侧 TCB 膨胀与兼容性问题，直接影响真实平台部署。

**我的思考**：作为首个以"如何构建"为导向的加速器 TEE SoK，其三类划分与"三机制+TCB/兼容性"分析框架为后续设计提供了清晰地图，指出的研究空白（内存加密缺失、认证正确性）具有指导意义；局限是 SoK 性质决定了结论偏定性、未覆盖 2025 年后最新方案细节，且对侧信道等跨机制威胁的交叉分析较浅，但仍是该领域目前最系统的参考综述。
- **论文 PDF**: [SoK: Analysis of Accelerator TEE Designs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1424-paper.pdf)

#### SoK: Take a Deep Step into Linux Kernel Hardening Effectiveness from the Offensive-Defensive Perspective.
- **作者**: Yinhao Hu, Pengyu Ding, Zhenpeng Lin, Dongliang Mu, Yuan Li
- **方向**: 系统与操作系统安全
- **解读**: Linux 内核支撑了主流发行版与绝大多数服务器，却持续面临内存安全漏洞；尽管已有超过 51 种加固机制，却缺乏从攻防视角对其覆盖度、局限、冗余与相互依赖的系统评估，理论与实际部署间也存在显著差距。

论文提出攻击者视角的三阶段利用分解框架（触发内存破坏、获取利用原语、达成目标），分析 2015 年以来 121 个公开 exploit（覆盖 102 个内核漏洞），归纳出 64 个复现攻击向量，并将 51 种内核防御逐一映射到攻击向量上评估其覆盖情况。

结果发现 23 个攻击向量完全无防护，31 种防御可被绕过或已过时（20 种相对安全）；存在 8 条出现在 20 个以上 exploit 中的高频攻击路径；识别出 4 对冗余加固、3 种过时防御与 4 对须联合部署的机制；此外 4 种有效加固在主流发行版中未被充分利用，10 个发行版中 4 个存在偏离配置。

这是首个系统的内核利用-防御全景分析，数据集公开，对加固路线图有直接指导价值；但依赖公开 exploit 存在选择偏差（不含政府与黑产利用），且攻防技术持续演进，框架的可扩展性决定其长期价值。

- **论文 PDF**: [SoK: Take a Deep Step into Linux Kernel Hardening Effectiveness from the Offensive-Defensive Perspective.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1725-paper.pdf)

#### UIEE: Secure and Efficient User-space Isolated Execution Environment for Embedded TEE Systems.
- **作者**: Huaiyu Yan, Zhen Ling, Xuandong Chen, Xinhui Shao, Yier Jin, Haobo Li, Ming Yang, Ping Jiang, Junzhou Luo
- **方向**: 系统与操作系统安全
- **解读**:
嵌入式 TEE（如 ARM TrustZone）为维持最小可信计算基（TCB）只提供加解密、签名、认证等少数功能且单线程执行，难以承载数据库、多媒体、机器学习等复杂数据处理应用；现有方案要么逐案例裁剪移植（工程量大、接口易错），要么把系统调用转发到 REE（TrustShadow/Shelter，开销高且仅支持部分系统调用）。

本文提出 UIEE：从 REE 动态分配内存并借助 TZASC 配置为安全内存，与 REE 和 TEE 双向隔离（独立页表 + TTBR0/TTBCR.N=2），应用与 LKL 不算入 TCB；选用库操作系统 LKL 提供完整 libc 与 Linux 系统调用服务（函数调用形式，免转发）；针对单线程 TEE 无法提供 pthread 的问题，提出两阶段启动（先在 REE 初始化 LKL 全部内核线程，同步休眠后切换主机接口并转入 TEE）与按需线程迁移机制，复用 REE Linux futex 子系统在 TEE 内重建线程上下文。

在 NXP IMX6Q 上无需修改直接运行 8 个真实 libc 应用（SQLite、FFmpeg、Polybench、OpenSSL、4 个 ML 模型、bzip2）；TCB 仅增 0.46% 代码与 3.37% 镜像；REE 系统调用延迟平均仅 +2.65%，OPTEE xtest 全部通过且性能无损；SQLite 与基线持平（比 LKL 快 4.08%），ML 推理开销 0.98%，bzip2 4MB 文件 7.72%；LKL 初始化 106.97ms，比原生 LKL 快约 54ms。

首个 TrustZone 向 LibOS 设计，两阶段启动+按需线程迁移巧妙解决"多线程 LibOS 塞进单线程 TEE"的矛盾，且 TCB 增量极小、运行时开销近乎为零，可行性验证扎实；局限是 bzip2 小文件因双层 I/O 开销高达 79%–608%，威胁模型明确排除侧信道、DMA、冷启动等硬件攻击，且依赖 TZASC 与框架测试套件质量；与转发系统调用的方案相比，函数调用式服务是明显性能优势，为嵌入式安全数据处理的通用化铺平了道路。
- **论文 PDF**: [UIEE: Secure and Efficient User-space Isolated Execution Environment for Embedded TEE Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s208-paper.pdf)

#### User-Space Dependency-Aware Rehosting for Linux-Based Firmware Binaries.

- **作者**: Chuan Qin, Cen Zhang, Yaowen Zheng, Puzhuo Liu, Jian Zhang, Yeting Li, Weidong Zhang, Yang Liu, Limin Sun
- **方向**: 系统与操作系统安全
- **解读**: 固件重宿主是规模化动态分析 IoT 固件的关键仿真技术，成功重宿主需同时仿真系统级功能（设备接口、ioctl、procfs）与用户态依赖（配置文件、环境变量、IPC）。现有方案未充分利用用户态知识：init 例程执行不完整导致环境初始化缺失；所有仿真失败被一刀切处理，混淆了直接系统级问题与用户态依赖的间接症状。本文提出 FIRMWELL，把重宿主建模为目标二进制与其用户态依赖的协同仿真。

方法上，FIRMWELL 先完整重宿主 init 例程（通常涉及上百个进程）构建环境，再启动目标服务；失败时通过调用链分析定位阻塞进程，按系统调用模式与错误症状将失败分类为用户态依赖问题或系统级仿真错误。核心策略是"用户态依赖失败→修正底层系统级仿真错误"：以 CREATE（补建设备文件）、INFER（def-use 分析+符号执行精确推断资源值，如 /proc/mtd）、FIX-IN-PEER（修复 IPC 对端进程）、REUSE 四种策略渐进修复。

在 14,049 个固件镜像（13 家厂商）上，FIRMWELL 成功重宿主 6,490 个 HTTP 服务，是 FirmAE（3,581）、Greenhouse（3,962）、Pandawan（810）的 1.6-8 倍，独有重宿主 1,389 个；平均耗时 12 分钟，比基线（22/74/101 分钟）快 1.8-8.4 倍；UPnP（2,634）与 DNS（3,118）同样领先；N-day PoC 检出 1,335 个漏洞；对 1,043 个固件模糊测试发现 67 个零日（44 空指针解引用、13 缓冲区溢出、6 可达断言、4 未捕获异常），获 10 个 CVE。

我的思考：FIRMWELL 首次把"用户态依赖"作为一等公民纳入重宿主流程，消融实验显示失败定位与分类模块贡献巨大。局限：受 QEMU-user 技术栈约束（MIPS64、Arctic Core 等架构不支持），2,074 个四工具全失败的镜像暴露符号执行路径爆炸与强签名校验等硬骨头。与 Greenhouse 相比，其"治本不治标"的修复哲学带来 1.6 倍成功率和更少修复轮次，是重宿主领域近年最显著的进展之一。

- **论文 PDF**: [User-Space Dependency-Aware Rehosting for Linux-Based Firmware Binaries.](https://www.ndss-symposium.org/wp-content/uploads/2026-s249-paper.pdf)

### 硬件与物理安全（16 篇）

#### DualStrike: Accurate, Real-time Eavesdropping and Injection of Keystrokes on Commodity Keyboards.
- **作者**: Xiaomeng Chen, Jike Wang, Zhenyu Chen, Qi Alfred Chen, Xinbing Wang, Dongyao Chen
- **方向**: 硬件与物理安全
- **解读**: 
以往键盘攻击要么只能窃听按键（推断攻击），要么需入侵式硬件（如改装 USB 设备）或物理在场才能注入；而快速兴起的霍尔效应键盘（新机型数量每三个月平均增长 82.60%）以磁敏霍尔传感器触发按键，其磁场变化既可被外部磁铁干扰，也可作为侧信道被探测。作者提出 DualStrike，首次在同一系统中实现非入侵式的按键窃听与逐键注入，无需对受害机做任何硬件或软件改动。

系统含三模块：基于有限元仿真设计的紧凑电磁铁阵列注入模块（吸引式电磁铁优于螺线管，通过一次性逆向工程获取扫描参数，实现免同步注入）；嵌入电磁铁间隙的商用磁力计（MLX90393）窃听模块；以及利用窃听结果推断键盘位移的校准模块。实测六款在售霍尔键盘，注入精度均超过 98.9%（最高 99.2%），窃听精度 99.41%–99.54%，注入速度最高达 12553 APM（人类游戏最快仅 818 APM）。

校准算法在键盘偏移达 4cm 时仍保持 98.5% 注入精度，而此前 SOTA（GhostType）仅在小于 14mm 位移内有效；端到端攻击（Ghost Login、Sudo Hijack）成功率接近 100%。作者还发现 DualStrike 对现有磁屏蔽机制免疫，并提出了针对霍尔键盘的新型屏蔽方案。

该工作把"窃听+注入"闭环起来，使攻击者能根据上下文（如 sudo 后的密码）实施精确的恶意操作，威胁模型现实且严重。局限在于设备需物理接近键盘部署、每款键盘需一次性逆向工程、且霍尔键盘尚未完全普及。与 GhostType 相比，它在速度、位移容忍度和感知能力上都有数量级提升，揭示新兴输入设备带来了全新的隐蔽物理攻击面。

- **论文 PDF**: [DualStrike: Accurate, Real-time Eavesdropping and Injection of Keystrokes on Commodity Keyboards.](https://www.ndss-symposium.org/wp-content/uploads/2026-s46-paper.pdf)

#### Exploiting TLBs in Virtualized GPUs for Cross-VM Side-Channel Attacks.
- **作者**: Hongyue Jin, Yanan Guo, Zhenkai Zhang
- **方向**: 硬件与物理安全
- **解读**: 
云端普遍采用 NVIDIA vGPU 等多租户共享 GPU 技术，但其微架构层面的信息泄漏风险几乎未被研究；DaaS 虚拟桌面、云游戏等场景承载大量用户敏感信息，跨 VM 泄漏可能造成严重后果。

作者面向虚拟化 GPU 的翻译后备缓冲（TLB）构建 Prime+Probe 攻击原语，需克服三重挑战：vGPU 运行时限制 CUDA 仅能以 2MB 页分配内存，几乎无法驱逐低层 TLB set——作者发现经 Vulkan 图形 API 分配的 64KB 页导入 CUDA 后仍保持页大小，从而得以构造驱逐集；vGPU 以毫秒级时间片调度 GPU 上下文，prime/probe 需跨时间片精心编排；多 TLB set 并行监控需规划线程分配并协调线程块，避免探测互相干扰。

两个跨 VM 案例研究均在同云共置 VM 上完成：其一为 CS-2 云游戏作弊——通过统计 L3 TLB set 竞争直方图（9 桶），用欧氏距离阈值（0.10/0.03）判定受害者是否在四个地图的六个已知露营点伏击，正确点距离总超阈值；其二为网页指纹攻击——以 ResNet-50 分类 100 个网站的 TLB 轨迹，跨 VM 平均准确率 91%（44 个网站 100% 准确），profiling VM 上五折交叉验证 92.3%。

这是首批在云环境中针对虚拟化 GPU 演示的跨 VM 侧信道攻击，用 Vulkan 64KB 页绕过 vGPU 内存约束是巧妙的关键工程。时间片调度天然限制了分辨率，但统计累积方法仍能可靠提取信息，说明 GPU 共享模式存在被低估的安全风险。局限在于需要攻击者与受害者共置 GPU、TLB 粒度较粗、直接检测被遮挡对象不可行而需统计推断；对云 GPU 租用经济性下共置租户较少的现实而言，威胁依然成立。
- **论文 PDF**: [Exploiting TLBs in Virtualized GPUs for Cross-VM Side-Channel Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1480-paper.pdf)

#### FLIPPYRAM: A Large-Scale Study of Rowhammer Prevalence.
- **作者**: Martin Heckel, Nima Sayadi, Jonas Juffinger, Carina Fiedler, Daniel Gruss, Florian Adamsky
- **方向**: 硬件与物理安全
- **解读**: 
Rowhammer 是可由无特权软件触发的 DRAM 干扰故障，自 2014 年以来研究持续热门，但真实世界的普遍性从未被严谨回答：以往研究样本极小（多数 <50 条 DIMM）、在实验室受控环境或 FPGA 上完成，结果难以外推。本文首次以用户研究方式大规模测量 Rowhammer 的"可武器化"程度。

作者开发 FLIPPYRAM 全自动跨平台框架（可启动 ISO 或 Docker），集成 5 个 DRAM 寻址函数逆向工具与 7 个 Rowhammer 工具，自动注入寻址函数、自动验证、按用户指定时长调度，无需任何专业知识；通过约 3000 个 USB 启动盘与线上渠道招募，于 2024-12-30 至 2025-06-30 收集了 822 台系统的 1006 份数据集。

结果颇具颠覆性：仅 453 份数据集（45%）成功完成寻址函数逆向（822 台唯一系统中 371 台，45.1%）；126 份（12.5%）出现位翻转（在逆向成功的数据集中占 27.8%），即 822 台中仅 93 台（11.3%）易受攻击；DDR3 受影响 27.2%、DDR4 8.76%（Intel 平台 11.1%）；Intel CPU 15.8% vs AMD 1.34%；三星 31.7%、海力士 28.6%、镁光仅 2.4%；无位翻转的数据集中 50% 卡在逆向阶段；运行时长 3h/8h/60h 下受影响比例分别为 4.72%/10.6%/30.7%。

我认为这份工作回答了领域内最关键的悬而未决问题：全自动可武器化 Rowhammer 在真实世界的成功率（约 10-20% 量级）远低于实验室报告的 30%-100%，瓶颈不在 DRAM 易感性，而在寻址函数逆向的可靠性（50% 的失败数据集折在逆向阶段）与工具跨微架构的兼容性。局限：招募偏技术人群存在样本偏差；ISO 仅支持 x86-64；未集成唯一对 AMD/DDR5 有效的 ZenHammer，AMD 与 DDR5 易感性被低估；结果仍是当前工具能力下的下界。
- **论文 PDF**: [FLIPPYRAM: A Large-Scale Study of Rowhammer Prevalence.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1810-paper.pdf)

#### Fuzzilicon: A Post-Silicon Microcode-Guided x86 CPU Fuzzer.
- **作者**: Johannes Lenzen, Mohamadreza Rostami, Lichao Wu, Ahmad-Reza Sadeghi
- **方向**: 硬件与物理安全
- **解读**: **问题**：现代 x86 CPU 是专有黑盒，微架构缺陷（如 µSpectre、Zenbleed 类）此前只能靠大量人工逆向发现，缺乏自动化的后硅片（post-silicon）漏洞检测框架，主要障碍是微架构状态不可见、无 bug 判定预言机、执行不确定与故障隔离困难。

**方法**：Fuzzilicon 是首个针对真实 x86 CPU 的后硅片 fuzzer，通过逆向 Intel 专有微码更新接口，在 Red-unlocked 模式下用 udbgrd/udbgwr 向微码 hook table 注入轻量探针，记录微操作执行计数形成 µcode 级覆盖率反馈；自研 bare-metal type-1 虚拟机（VMX）隔离测试输入；受 Zenbleed 启发提出序列化预言机，插入 LFENCE 生成语义等价变体做差分检测；采用 Controller（树莓派）/Agent（UEFI）/Watchdog（断电复位）分离架构，目标为 Intel N3350（Goldmont）。

**结果**：获得 5 项重大发现，包括两个此前未知的 µcode 级投机执行漏洞——投机窗口内 CRBUS 写持久化可禁用 hook 表以绕过微码安全补丁，以及段选择符缓存投机写绕过权限检查造成提权/DoS；并自动重发现 µSpectre 漏洞类。覆盖率收集开销较基线降低最多 31×（566.490ms 降至 18.274ms），达到 2,867 个唯一 µcode 地址，占可 hook 位置的 16.27%，是有史以来首个此类基线；带反馈配置达成同等覆盖率快约 8×。

**我的思考**：把商业 CPU 变成"灰盒"进行微码级覆盖引导 fuzzing 是突破性工作，µcode 覆盖率相当于软件中的边覆盖，开辟了后硅片 fuzzing 新方向；局限显著：依赖 Red-unlocked 工程模式（仅特定处理器与调试硬件、一次性手动解锁）、目标为老旧的 Goldmont 平台，且投机窗口漏洞的实际可利用性与跨架构可移植性尚待验证，但作为首个实证基线价值明确。

- **论文 PDF**: [Fuzzilicon: A Post-Silicon Microcode-Guided x86 CPU Fuzzer.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1486-paper.pdf)

#### GoldenFuzz: Generative Golden Reference Hardware Fuzzing.
- **作者**: Lichao Wu, Mohamadreza Rostami, Huimin Li, Nikhilesh Singh, Ahmad-Reza Sadeghi
- **方向**: 硬件与物理安全
- **解读**: **问题**：现代处理器复杂化带来巨大漏洞面，硬件 fuzzing 是规模化发现缺陷的关键手段，但现有 fuzzer 语义感知弱、测试细化低效，且依赖慢速 RTL 仿真导致计算开销过高，难以探索深层硬件状态。

**方法**：GoldenFuzz 提出两阶段框架，把测试细化与覆盖/漏洞探索部分解耦：先用快速、严格符合 ISA 的 Golden Reference Model（以 Spike 仿真器作为 DUT"数字孪生"）低成本 fuzz 以细化测试策略，再将优化策略迁移到 DUT（RocketChip、BOOM、CVA6，Synopsys VCS 仿真）。核心是一个为 RISC-V 定制的 GPT 语言模型（用 1,000 万条随机指令从头训练约 1 小时），按指令块（每测试 5 块×6 条指令）增量生成测试；采用 intra/inter 双层覆盖评分与"胜-败"偏好对结合 SimPO 在线优化策略，并以 FIFO fuzzing 记忆防止过拟合。

**结果**：在三款 RISC-V 核上覆盖全面超越 Cascade、ChatFuzz 等四个 SOTA fuzzer，仅用 30 条指令的短测试用例与不到 1% 的用例数即达到相当覆盖率；单用例生成 0.34 秒（CPU）/0.012 秒（GPU），远低于 Cascade 的 2.06 秒。发现全部已知漏洞并新增 5 个（4 个 CVSS>7：CVA6 大小端 MBE/SBE 失效 7.5、委托 STI 掩码缺陷 7.6、stval 处理错误 5.5、CSR 访问控制缺陷 7.6），获 CVE-2025-45883/45881，另在商业 BA51-H 核扩展中发现 2 个未知漏洞。

**我的思考**：以"数字孪生"GRM 先细化策略再上真实 DUT 的设计巧妙解耦了两个目标，LLM 生成器与 SimPO 在线学习也是新颖组合；局限是评估限于开源 RISC-V 核、DUT 仿真仍是瓶颈（1.36 秒/用例）、新漏洞集中于 CVA6，且 GRM 与 DUT 的行为偏差可能使部分 DUT 特有缺陷被策略细化阶段掩盖，跨 ISA（ARM/x86）适配尚待验证。

- **论文 PDF**: [GoldenFuzz: Generative Golden Reference Hardware Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1663-paper.pdf)

#### Hiding an Ear in Plain Sight: On the Practicality and Implications of Acoustic Eavesdropping with Telecom Fiber Optic Cables.
- **作者**: Youqian Zhang, Zheng Fang, Huan Wu, Sze-Yiu Chau, Chao Lu, Xiapu Luo
- **方向**: 硬件与物理安全
- **解读**: **问题**：光纤因无射频辐射被视为低侧信道风险的可靠介质，但研究证明电信光纤对声学振动敏感，可被用于声学窃听；随着 FTTH 普及，攻击者接入光纤一端即可窥探用户室内环境，此前研究停留在理论或理想化实验（声音与光纤直接耦合），实际可行性未获验证。

**方法**：攻击者可在 ODN 端将受害光纤接入商用分布式声学传感（DAS）系统，利用瑞利散射相位变化重建声波。初步实验显示裸纤能捕获结构振动（脚步声 76dB 可测）却对空气声不敏感（80dB 语音失败），故设计"Sensory Receptor"：将 15 米光纤缠绕于外径 65mm 的空心 PET 圆柱上，把声压转化为纵向应变累积，并给出相位变化在噪声下限与 2π 饱和之间的可行域模型；再结合三受体时差定位、微调 Efficient-AT 事件分类与 whisper-large-v3 等 ASR 恢复信息。

**结果**：受体在 2 米距离仍保持 SNR≈20dB、相关系数≈0.9；事件检测微调后在 0.1/1/2 米精度达 0.83/0.50/0.43（随机基线 0.07）；27 m² 房间内声源定位平均误差 0.77 米；语音识别在 1 米内 WER<0.1，3 米约 0.3，4 米约 0.5；真实办公室跨 50 米案例平均 WER≈0.17（保留约 80% 语音信息），超声干扰对光纤窃听无效（麦克风 WER≥1）。

**我的思考**：首次把"光纤可听声"这一物理现象转化为端到端可执行的隐私攻击，伪装成光纤盒的受体隐蔽性强且无 RF 特征，难被常规排查发现；局限是攻击需物理接触 ONU/ODN（内部人员或冒充技工）、有效范围受距离与音量限制，且对如何识别异常 DAS 接入等防御手段着墨不足。

- **论文 PDF**: [Hiding an Ear in Plain Sight: On the Practicality and Implications of Acoustic Eavesdropping with Telecom Fiber Optic Cables.](https://www.ndss-symposium.org/wp-content/uploads/2026-f546-paper.pdf)

#### Memory Band-Aid: A Principled Rowhammer Defense-in-Depth.

- **作者**: Carina Fiedler, Jonas Juffinger, Sudheendra Raghav Neela, Martin Heckel, Hannes Weissteiner, Abdullah Giray Yaglikçi, Florian Adamsky, Daniel Gruss
- **方向**: 硬件与物理安全
- **解读**: Rowhammer位翻转使软件攻击者能攻陷大量系统，而硬件缓解措施部署周期长、几乎不可更新，新的锤击模式（如many-sided）反复绕过已部署的TRR等防护。Memory Band-Aid提出一种原则性的软件防御纵深：不是替代长期硬件缓解，而是在某代硬件防护不足时作为第二道防线启用。

其核心洞察是：若攻击者无法达到Rowhammer位翻转所需的最小行激活数，就无法触发位翻转。为此Memory Band-Aid在内存控制器中引入per-thread与per-bank的DRAM访问速率限制，由操作系统按上下文切换对不受信工作负载（全部用户态程序或指定沙箱）动态施加限制。当前Intel（MBA）与AMD（L3BE）仅有全局带宽限制且Intel限制粒度到物理核、直到Xeon第3代才支持真正的速率限制，因此作者用171行内核补丁实现基于Class of Service的PoC，用全局限制近似per-bank限制。

评估在2台Intel与2台AMD机器上进行：完整实现（含per-bank硬件支持）在Phoronix宏基准上开销仅0%–9.4%，最坏情况微基准（AVX512紧循环）慢1至5.1倍；128 MiB/s限制把Flush+Reload迭代降到无限制时的9.2%（A1），足以缓解9/10个测试模块上的9侧攻击、全部10个模块上的19侧攻击。而当前无per-bank的硬件上，PoC开销高达349倍（微基准）与4.3倍（宏基准），Intel最低10%限制甚至不足以缓解many-sided攻击。

该工作把Rowhammer防御从"权宜之计"（ECC、加倍刷新、禁用指令）提升为可证明、可配置、可软件更新的原则性方案，且只惩罚不受信负载。局限：低开销依赖厂商补充per-bank硬件支持（论文明确呼吁），当前商品化Intel/AMD系统上不实用；对全局限制的近似在Intel上安全性不足。与硬件TRR形成互补，是纵深防御架构中务实的一环。
- **论文 PDF**: [Memory Band-Aid: A Principled Rowhammer Defense-in-Depth.](https://www.ndss-symposium.org/wp-content/uploads/2026-s156-paper.pdf)

#### OCCUPY+PROBE: Cross-Privilege Branch Target Buffer Side-Channel Attacks at Instruction Granularity.

- **作者**: Kaiyuan Rong, Junqi Fang, Haixia Wang, Dapeng Ju, Dongsheng Wang
- **方向**: 硬件与物理安全
- **解读**: 分支目标缓冲（BTB）在攻击者与受害者间共享，可被构造为泄露受害者分支信息的侧信道。但现有 BTB 侧信道存在双重缺口：自第 11 代 Intel Core 起硬件隔离使用户态无法利用内核态 BTB 条目做访问式攻击；而基于驱逐（eviction）的攻击分辨率仅到 BTB set 级，无法定位具体是哪条分支被执行，跨特权级泄露内核控制流近乎不可行。

本文通过逆向 Intel 处理器的 offset 相关 BTB 更新机制奠定基础：发现 BTB 以分支指令"最后一个字节"的地址做索引，且新旧条目 index/tag/offset 匹配时存在直接替换（Direct Replacement）、失效+分配、仅失效、仅分配四种更新机制；关键洞察：无论替换策略与硬件隔离如何，内核条目可直接替换用户条目。据此提出 Occupy+Probe 三步攻击：占据（插入与目标内核分支匹配的占据分支）→ 受害者执行（分支被采取则替换占据条目）→ 探测（经预取时序判断占据条目是否仍在），无需与受害者共享条目，且达指令级分辨率。

实验覆盖 9-14 代 Intel：面对混淆哑分支（同 set 多条分支）时，BTB Prime+Probe 成功率跌至约 48.9%，而 Occupy+Probe 保持 91.9%-100%；还能提取内核分支 tag 值，在 i7-11700K 上以 97.5% 成功率打破 KASLR；端到端案例针对 Linux Kernel Crypto API 中 RSA 的 mpi_powm，借助 eventfd+cond_resched 逐迭代抢占，以 98% 准确率恢复私钥比特。

我的思考：该工作填补了跨特权 BTB 侧信道"分辨率"与"隔离绕过"两个空白，逆向工作扎实，且首次证明驱逐类攻击也能提取 tag 用于打破 KASLR，方法论贡献明确。局限在于威胁模型要求攻击者与受害者同逻辑核、受害者分支地址已知且可反复执行；12-14 代需在 P 核上运行。Intel 仅以既有侧信道指南回应而不新增缓解，此类硬件级攻击的防御仍缺位。

- **论文 PDF**: [OCCUPY+PROBE: Cross-Privilege Branch Target Buffer Side-Channel Attacks at Instruction Granularity.](https://www.ndss-symposium.org/wp-content/uploads/2026-s925-paper.pdf)

#### Peering Inside the Black-Box: Long-Range and Scalable Model Architecture Snooping via GPU Electromagnetic Side-Channel.

- **作者**: Rui Xiao, Sibo Feng, Soundarya Ramesh, Jun Han, Jinsong Han
- **方向**: 硬件与物理安全
- **解读**: DNN 架构是安全关键系统（自动驾驶、人脸识别）的高度机密资产，黑盒攻击因缺乏架构知识而效率受限。已有架构窃取手段要么需与受害进程共驻（cache/总线侧信道），要么需把嗅探器贴在 GPU 电源线上（毫米级近场 EM）或芯片开盖，隐蔽性差、难以实用。本文提出 ModelSpy：利用 GPU 数字部件辐射的远场 EM 信号在推理时被架构相关 DRAM 活动振幅调制的现象，从数米外甚至穿墙重建完整 DNN 架构。

方法要点：1）5 GHz 天线+USRP B210 采集远场辐射（选 5 GHz 避开 WiFi 频段）；2）专用去噪管线+利用全局上下文的分层神经重建引擎，从低 SNR 信号中恢复层数、层类型与逐层超参数；3）利用外部 EM 与内部 DRAM 读写轨迹的时序相关性设计迁移学习方案——先用易标注的 DRAM 轨迹预训练、再用少量 EM 信号微调，解决架构空间庞大导致的训练数据瓶颈。

在 5 款高端消费 GPU、2820 个架构、14.1 万次推理上，层分割准确率 97.6%、超参数估计 94.0%，比近场基线高 10.5 个百分点；工作距离最远 6 米（5 米处 86.7%，基线需毫米级贴近），可穿玻璃/木/混凝土墙（混凝土后 85.7%）；拓扑重建 NLD 0.14（基线 0.49，改善 71%）；重建架构使黑盒对抗攻击成功率平均提升 22.8%（CIFAR-10）与 19.8%（CIFAR-100），与精确架构差距在 4% 以内。

我的思考：把架构嗅探距离从毫米级推进到米级并穿墙，是物理侧信道攻击的实质性跃迁，威胁模型（伪装维修人员背包藏嗅探器）现实且隐蔽；DRAM 预训练+EM 微调解决了可扩展性问题。局限：跨代 GPU 泛化差（平均 70.5%）、7 米处骤降至 42.3%、WiFi 干扰敏感、跨框架需混合训练弥补；需目标 GPU 实际运行推理。防御方向（EM 屏蔽、时钟随机化）与边缘设备物理可达性值得后续研究。

- **论文 PDF**: [Peering Inside the Black-Box: Long-Range and Scalable Model Architecture Snooping via GPU Electromagnetic Side-Channel.](https://www.ndss-symposium.org/wp-content/uploads/2026-s141-paper.pdf)

#### PhantomMotion: Laser-Based Motion Injection Attacks on Wireless Security Surveillance Systems.

- **作者**: Yan He, Guanchong Huang, Song Fang
- **方向**: 硬件与物理安全
- **解读**: 无线安防监控系统因廉价而广泛部署，PIR 运动检测是其安全功能的枢纽，但现有无线相机检测/触发研究都要求人在相机前执行预设动作，既不便又让使用者暴露于被录像的风险。本文提出 PhantomMotion：无需任何人体动作与专业设备，远程用激光注入虚假运动触发 PIR 传感器。

原理是 PIR 传感器检测物体与背景的红外温差，而激光可快速加热物体表面至接近体温（约 37°C），产生与人体相似的红外辐射；攻击者把激光束射入运动检测区加热表面，模拟人体运动触发系统。针对相机位置未知设计扫描方法：均匀分布点路径+无线流量与激光加热时间的相关性确认检测区；反向工程 PIR 机制计算各材料加热时间；激光控制+WiFi 嗅探硬件平台由手机 app 操作，总成本约 80 美元。

18 款流行设备 1800 次试验 100% 成功率、0 误报，平均 12.8 秒、光斑移动 1.1 米；最远 120 米；可透 1/4 英寸玻璃；12 名无经验志愿者全部成功；连续触发使标称 5 个月电池的相机 11.5 小时耗尽（约 313 倍），形成骚扰与电池耗尽双重攻击面。

我的思考：首个无需人体动作、可手机操控的安防系统触发方法，把 PIR 红外感知盲区变成远程可利用的攻击面，兼具攻击（探测盲区、骚扰、耗尽电池）与防御（探测隐藏摄像头）双重用途，80 美元成本与 100% 成功率使其威胁现实。局限：需激光对准检测区、NLOS 依赖传热、120 米测试受安全距离限制；PIR 侧缓解（多传感器融合、温度变化率滤波）论文未深入。与需 6W CO2 激光（数千美元）的先前工作相比，成本与安全性优势明显。
- **论文 PDF**: [PhantomMotion: Laser-Based Motion Injection Attacks on Wireless Security Surveillance Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1454-paper.pdf)

#### PhyFuzz: Detecting Sensor Vulnerabilities with Physical Signal Fuzzing.
- **作者**: Zhicong Zheng, Jinghui Wu, Shilin Xiao, Yanze Ren, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **方向**: 硬件与物理安全
- **解读**:
传感器漏洞可被声、电磁、激光物理信号利用造成错误测量，但漏洞发现全部依赖专家手工试错，缺乏自动化手段，阻碍传感器安全走向工业应用。本文提出PhyFuzz，首个基于物理信号模糊测试的传感器漏洞自动发现范式。

方法用信号构造集{A幅度、F频率、P相位、M调制}压缩连续无限的物理输入空间（幅度sigmoid分布诱发非线性）；特征离散化把输出经差分分析转为离散向量作覆盖反馈，以"输出多样性反映漏洞多样性"；采用AFL式变异与蚁群式参数调度，适应度=α偏差+β最小距离，用特征模板自动归类已知漏洞。

实验在13个9类传感器上发现46个漏洞（含6个未公开），复现ADXL345、MPU6050已知漏洞，并从颜色/光传感器发现5类新漏洞；与扫描测试相比，效率最高6.25倍、首漏洞快至12倍、发现最多类型平均快27倍；已上报厂商。

我的思考：把模糊测试从数字域延伸到物理域是范式创新，输出特征离散化作黑盒覆盖度量巧妙。局限：模板编码专家知识，新机理仍需人工分析；固定距离与6-14小时时长限制普适性。整体填补传感器安全自动化检测空白。
- **论文 PDF**: [PhyFuzz: Detecting Sensor Vulnerabilities with Physical Signal Fuzzing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f29-paper.pdf)

#### ReFuzz: Reusing Tests for Processor Fuzzing with Contextual Bandits.
- **作者**: Chen Chen, Zaiyan Xu, Mohamadreza Rostami, David Liu, Dileep Kalathil, Ahmad-Reza Sadeghi, Jeyavijayan Rajendran
- **方向**: 硬件与物理安全
- **解读**: 处理器设计高度依赖迭代修改与设计复用（2023年调查显示超三分之二SoC复用既有设计，BOOMV3即大量复用Rocket Core代码），漏洞也随之跨处理器传播（如CVA6、PicoRV32、Kronos对FENCE指令的错误异常处理）。现有处理器模糊测试各自独立生成种子、逐个设计单独测试，不会利用先前处理器中已知漏洞的测试（PP-tests）来引导对待测处理器（PUT）的测试，错失检测相似或变体漏洞的机会。本文提出ReFuzz，用上下文多臂赌博机（CB）自适应复用PP-tests。

ReFuzz把模糊测试建模为CB问题：上下文=当前总覆盖率水平，臂=PP-test，奖励=覆盖率增量。训练阶段先用整数规划求解最小测试子集（测试最小化器去除98.76%冗余，126K→1.5K），再用带淘汰机制的适应性CB算法（移动平均覆盖率增量评估、淘汰零增益测试并晋升高效测试）为每个覆盖率上下文（55%/60%/65%/70%）训练"漏洞测试表+覆盖率测试表"；测试阶段按当前覆盖率动态选择测试，表耗尽后回归模糊器原生种子生成。

在5个RISC-V处理器上发现3个新安全漏洞（V1 CVSS 7.1：复用CVA6的FENCE.I漏洞测试在RSD上触发内存死锁DoS；V2/V3 CVSS 8.5：RSD执行非法LOAD/STORE）和2个新功能缺陷（B1：Rocket Core/BOOMV3/BOOMV4共享模块导致的minstret计数错误；B2：BOOMV3/BOOMV4的minstret缺陷，BOOMV4因新微架构多出10个变体）。平均覆盖率提速511.23倍、总覆盖率最高多9.33%，检测新漏洞平均少用6.29倍时间与5.98倍测试。

我的思考：把CB引入硬件模糊测试，把工业界"复用定向测试"的经验自动化，与设计复用趋势对齐，是工程与学习的漂亮结合；V1的检测路径（复用CVA6已知测试→发现RSD新漏洞）直接验证了核心动机，三个数量级的覆盖率提速很有冲击力。局限包括：需事先开展多处理器模糊测试战役收集语料、V2/V3因不在漏洞表中反而比基线检测更慢、CB训练依赖Synopsys VCS覆盖率数据库解析，且评估限于开源RISC-V核（商用处理器多为闭源IP），向闭源设计的迁移性有待验证。
- **论文 PDF**: [ReFuzz: Reusing Tests for Processor Fuzzing with Contextual Bandits.](https://www.ndss-symposium.org/wp-content/uploads/2026-f118-paper.pdf)

#### Side-channel Inference of User Activities in AR/VR Using GPU Profiling.
- **作者**: Seonghun Son, Chandrika Mukherjee, Reham Mohamed Aburas, Berk Gülmezoglu, Z. Berkay Celik
- **方向**: 硬件与物理安全
- **解读**：**问题**：AR/VR 设备承载位置、浏览历史、财务数据等敏感信息，恶意应用可利用性能计数器监视良性应用追踪用户活动；但既有攻击依赖可并发运行的独立后台应用（Meta Quest 已禁用）、需要 60Hz 高分辨率画像（易被检测与禁用），且只覆盖少量攻击属性，未建模 3D 渲染与 GPU 使用的关系。

**方法**：作者提出 OVRWATCHER，利用 Meta 设备内置的 ovrgpuprofiler 工具（暴露 72 个 GPU 指标）以低至 1Hz 的分辨率采集 GPU 画像，无需并发应用、额外 SDK 或提权；核心思想是捕捉 GPU 指标与 3D 对象交互（渲染负载、场景复杂度）之间的细粒度相关性，据此指纹应用与对象级活动，并扩展到 WebXR 应用与虚拟会议参与人数等场景。

**结果**：OVRWATCHER 在独立 AR/VR 应用指纹上达到超过 99% 的准确率、对象级推断超过 98%，且单个 GPU 指标就足以泄露信息；WebXR 应用指纹 99%（仅两个指标时仍超 94%）；虚拟会议应用检测参与人数准确率超过 98%；在真实世界应用 Meta Layout 上准确率达 88%，全部案例研究平均超过 93%。

**我的思考**：该工作证明"降低采样分辨率"不是有效防御——1Hz 低分辨率已足够，且利用内置工具绕开了并发应用限制，攻击门槛显著降低；局限是 GPU 指标相关性因设备/应用而异、需每设备训练，真实世界应用准确率（88%）低于受控场景，且未覆盖 CPU/内存等其他画像源的组合泄露，可作为未来"多模态低分辨率侧信道"研究的起点。

- **论文 PDF**: [Side-channel Inference of User Activities in AR/VR Using GPU Profiling.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1302-paper.pdf)

#### SNPeek: Side-Channel Analysis for Privacy Applications on Confidential VMs.
- **作者**: Ruiyi Zhang, Albert Cheu, Adrià Gascón, Daniel Moghimi, Phillipp Schoppmann, Michael Schwarz, Octavian Suciu
- **方向**: 硬件与物理安全
- **解读**：**问题**：基于 TEE 的机密虚拟机（CVM，如 AMD SEV-SNP、Intel TDX）虽加密访客内存，但官方威胁模型明确排除页表活动与处理器缓存状态泄露，缓解责任被推给开发者；而既有缓解要么不通用要么太慢，开发者缺乏系统性、高效地测量与比较真实部署泄露程度的手段。

**方法**：提出开源工具 SNPeek：在生产 AMD SEV-SNP 硬件上提供可配置的侧信道追踪原语——利用 MSR 限制非包容 L3 缓存、针对 64 个缓存集实现低噪声高效 Multi-Prime+Probe 攻击，并配页表追踪与多种过滤手段降低采集开销；追踪数据交给统计与机器学习（含 CNN）分析流水线自动估计泄露，并引入基于"攻击者优势"（可由差分隐私界定的量化泄露概念）的评估框架。

**结果**：对三个真实隐私工作负载评估：私有信息检索（PIR，Project Oak/Signal）中即使常数时间 ORAM 在 SEV-SNP 上也因密文侧信道存在泄露；私有重击者（PHH，TensorFlow Federated）因数据相关执行而可被隐私攻击，论文给出基于差分隐私的部分缓解；Wasm 用户定义函数（Privacy Sandbox）可被共谋的 hypervisor 以至少 497 kbit/s 的隐蔽信道窃取数据——验证了 SNPeek 能定位泄露源并指导低开销缓解。

**我的思考**：SNPeek 填补了"无泄漏量化工具"的工程空白，让非侧信道专家也能测量、对比并降低泄露，且"常数时间 ORAM 仍泄露"与 497 kbit/s 隐蔽信道是重要发现；局限是绑定 AMD SEV-SNP、隐蔽信道场景假设 hypervisor 共谋（部分超出 CVM 官方威胁模型），但作为开源基准对机密计算生态的合规评估与审计实践有直接推动作用。

- **论文 PDF**: [SNPeek: Side-Channel Analysis for Privacy Applications on Confidential VMs.](https://www.ndss-symposium.org/wp-content/uploads/2026-f699-paper.pdf)

#### SoK: Understanding the Fundamentals and Implications of Sensor Out-of-band Vulnerabilities.
- **作者**: Shilin Xiao, Wenjun Zhu, Yan Jiang, Kai Wang, Peiwang Wang, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **方向**: 硬件与物理安全
- **解读**: 传感器是网络物理系统（CPS）感知与控制的基础，但针对传感器的物理攻击研究零散、缺乏统一抽象，且攻击信号空间近乎无限，阻碍威胁建模与防御设计。

论文提出传感器带外（OOB）漏洞框架：以七种物理模态（声、光、机械、热、磁、电磁、电）构建 7×7 能量转换矩阵来抽象攻击路径，将漏洞划分为超范围（out-of-range）与跨场（cross-field）两类；采用自底向上的方法在组件（换能器、信号调理电路、通信接口、电源）、传感器、系统三个层面系统化分析。

传感器层面将攻击归为声/超声、激光、辐射 EMI、传导 EMI 四类，并从攻击者先验知识、有效距离、设备成本等维度评估实用性，指出攻击距离与设备便携性是主要瓶颈；系统层面显示传感器融合、闭环控制、智能感知等特性均无法完全防御 OOB 威胁，还识别出激光攻击信号调理电路等未开发的攻击向量。

这是首个基于物理原理的传感器攻击面统一抽象，理论框架完整且对安全感知设计有可操作指导；但攻击评估主要依赖文献归纳、量化数据有限，且对"非理想性无法在设计阶段根除"的论断提供了务实辩护。

- **论文 PDF**: [SoK: Understanding the Fundamentals and Implications of Sensor Out-of-band Vulnerabilities.](https://www.ndss-symposium.org/wp-content/uploads/2026-s450-paper.pdf)

#### The Heat is On: Understanding and Mitigating Vulnerabilities of Thermal Image Perception in Autonomous Systems.

- **作者**: Sri Hrushikesh Varma Bhupathiraju, Shaoyuan Xie, Michael Clifford, Qi Alfred Chen, Takeshi Sugawara, Sara Rampazzi

- **方向**: 硬件与物理安全

- **解读**: 自动驾驶、机器人、无人机日益依赖热成像相机保障夜间、雾天等低能见度条件下的感知，但热像管线中均衡、标定、镜头环节的安全性此前未被系统研究。攻击者可用 20 美元的陶瓷加热灯产生人眼不可见的红外辐射，改变感知的相对温度或在攻击结束后数分钟仍留下延时伪影，使障碍物规避失效。

作者系统分析三款热像相机（FLIR Boson、InfiRay T2S、FPV XK-C130）的三种漏洞：均衡线性化（高温度方差场景触发 plateau 均衡的线性行为，压缩目标像素强度）、标定漏洞（在周期性 NUC 标定期间暴露热源以操纵像素偏移估计，产生延迟伪影）、镜头鬼影（圆形光圈与特殊镜头保留热源结构，可生成任意形状的 ghost 伪影）；在 FLIR ADAS 数据集上评估 3 个微调检测器与 2 个 RGB-热融合模型，并提出三种威胁感知信号处理防御。

均衡漏洞使行人检测 mAP 平均下降 50%、融合模型下降 45%；实车测试（最高 40 km/h）中行人漏检率最高 100%、假障碍物生成成功率最高 91%，伪影持续数分钟；标定伪影在测试集攻击成功率最高 93.9%，ghost 伪影在 80°C 下即达 90% 以上；防御算法将 mAP 下降从 50% 降至 4%，伪影抑制准确率 100%。

我的思考：创新在于把攻击从"模型输入"前移到"相机内部信号处理"，仅需普通加热灯且为黑盒假设，威胁模型贴近真实道路场景，是传感器物理层安全的代表工作。局限：相机样本有限、假设攻击者已知均衡参数（公开手册可获取）、防御评估较初步。与冰袋/热补丁类对抗补丁工作相比，无需优化外观即可造成漏检或假障碍物，提示需在信号处理层增加异常检测。

- **论文 PDF**: [The Heat is On: Understanding and Mitigating Vulnerabilities of Thermal Image Perception in Autonomous Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-s330-paper.pdf)

### 移动与物联网安全（8 篇）

#### BSFuzzer: Context-Aware Semantic Fuzzing for BLE Logic Flaw Detection.
- **作者**: Ting Yang, Yue Qin, Lan Zhang, Zhiyuan Fu, Junfan Chen, Jice Wang, Shangru Zhao, Qi Li, Ruidong Li, He Wang, Yuqing Zhang
- **方向**: 移动与物联网安全
- **解读**:
BLE逻辑缺陷（字段误释、非法状态迁移）可致认证绕过、未授权控制与DoS（如BLUFFS），但缺陷隐藏在合法协议交互中、不触发崩溃，传统模糊测试与形式化分析难以发现；规范语义又隐式散落于3000多页文本。

BSFuzzer以LLM智能体（Grok-3）解析规范：从消息时序图提取Mealy状态机，抽取字段语义（位长、语义角色、定义值）与跨报文依赖，据此生成字段变异（语义非法值、边界、位翻转等5策略）与状态变异（前置状态未完成、重复操作、意外迁移、组合违规），经Mapper实例化与密码学模块（保证密钥派生正确）组包发送；验证阶段由LLM按规范比对设备响应偏差。

9款SoC与10部智能手机上发现36个缺陷（34个未知），9个获CVE、2个获漏洞赏金，去重后19个根因（6内存、8状态、5不一致），如接受低于27字节最小值的长度请求致"软死锁"、配对前过早触发加密、重连复用旧会话密钥、SC降级为Legacy配对。覆盖率比最强基线Proteus高9.34%；字段语义解析准确率92%、报文依赖97%、字段/状态处理策略94%/85%；响应验证字段级85.8%、状态级74.0%。

把LLM语义推理系统注入协议模糊测试全流程（解析-变异-验证），在真实设备上挖出传统工具看不见的逻辑缺陷，方法论可迁移到其他多阶段协议。局限：LLM幻觉靠温度0与约4.5小时人工复核缓解，状态级验证准确率仅74%；覆盖率仅在btstack上测量；无线交互耗时限制变异轮次。
- **论文 PDF**: [BSFuzzer: Context-Aware Semantic Fuzzing for BLE Logic Flaw Detection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f94-paper.pdf)

#### CHAMELEOSCAN: Demystifying and Detecting iOS Chameleon Apps via LLM-Powered UI Exploration.
- **作者**: Hongyu Lin, Yicheng Hu, Haitao Xu, Yanchen Lu, Mengxia Ren, Shuai Hao, Chuan Yue, Zhao Li, Fan Zhang, Yixin Jiang
- **方向**: 移动与物联网安全
- **解读**: **问题**：变色龙应用提交审核时合法、安装后经开发者-用户合谋转为非法变体，绕过 App Store 审核；静态分析（Chameleon-Hunter）对动态渲染 UI 无效，元数据依赖的 Mask-Catcher 对新变体滞后，转换机制也缺乏系统认知。

**方法**：监控隐蔽分发渠道构建 500 个应用真值数据集，识别 10 类转换模式（含 4 类新变体）；CHAMELEOSCAN 用 LLM 少样本提示从元数据与已知情报推断转换方法，融合截图与视图层级做语义 UI 理解，生成类人交互策略应对广告弹窗等干扰，以界面改变为确证信号。

**结果**：在 1,644 个 iOS 应用上评估，检测率 9.85%、精确率 92.59%，发现获苹果官方正式确认；实现与数据集已开源（github.com/ChameleoScan）。

**我的思考**：首次系统刻画合谋型变色龙机制全貌，以运行时 LLM 驱动 UI 探索取代静态/元数据依赖，精确率高且可解释；局限是检测率反映扫描覆盖面、规模化 LLM 成本未讨论、对抗者可能针对性进化。对应用商店治理有直接价值。
- **论文 PDF**: [CHAMELEOSCAN: Demystifying and Detecting iOS Chameleon Apps via LLM-Powered UI Exploration.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1906-paper.pdf)

#### Cross-Boundary Mobile Tracking: Exploring Java-to-JavaScript Information Diffusion in WebViews.

- **作者**: Sohom Datta; Michalis Diamantaris; Ahsan Zafar; Junhua Su; Anupam Das; Jason Polakis; Alexandros Kapravelos
- **方向**: 移动与物联网安全
- **解读**: 问题：约 83% 的 Google Play 应用使用 WebView，其 Java 与 JavaScript 上下文本应相互隔离，但应用可通过 evaluateJavascript、addJavascriptInterface 等接口动态注入代码，把广告 ID、Build ID 等 JavaScript 本无法访问的"上下文受限数据"传入 Web 上下文并被第三方脚本外传；既有工具聚焦 Java 代码或静态分析，缺乏对 WebView 内 JavaScript 执行与跨上下文信息流的动态可见性。

方法：作者提出 WebViewTracer——将 Chromium 的 VisibleV8 补丁集重编译进 Android System WebView provider，实现系统级 JavaScript API 调用追踪，并用 Frida gadget hook evaluateJavaScript、loadUrl、addJavascriptInterface 等 Java 调用，结合改造的 UIHarvester 自动爬取应用；日志后处理由注入检测、API 分类、外传分析、信息流分析四模块组成，通过堆栈溯源把注入归因到具体 SDK。在 10K 个 AndroZoo 应用（Pixel 4a、Android 13）上完成首次大规模动态测量。

结果：4,597 个应用加载 WebView，其中 95% 执行了注入，65%（2,989 个）注入了上下文受限数据、59% 将其外传至网络；96% 的注入源自第三方库（Google Ads SDK 出现在 3,018 个应用中，Unity3D 484 个，IronSource 235 个），仅 29 个 SDK 造成全部此类泄漏；25 个 SDK 把数据传给 152 个外部实体（含 25 个 DuckDuckGo 追踪雷达未标记的追踪方）；注入检测精确率 98.2%、召回率 96.4%；发生泄漏的应用中超 30% 平均调用 50+ 个指纹 API（无泄漏应用仅 10%），742 个应用使用 WebGL 指纹、284 个使用 canvas toDataURL。

我的思考：这是首个大规模、动态的 WebView 跨上下文隐私分析，证明 Java-JS 桥同时打破了隔离与策略执行两类保证，且注入行为高度集中于少数广告 SDK，隐私风险呈现"赢者通吃"结构；方法上把 VisibleV8 移植进系统 WebView 的技术难度不小，为后续研究提供了可复用基础设施；局限是 UIHarvester 仅覆盖 49.64% 的 WebView，作者也承认结果是保守下界，且 25 个未被 Web 追踪雷达标记的移动端追踪方提示现有 web 检测生态与移动生态存在显著鸿沟。

- **论文 PDF**: [Cross-Boundary Mobile Tracking: Exploring Java-to-JavaScript Information Diffusion in WebViews.](https://www.ndss-symposium.org/wp-content/uploads/2026-s910-paper.pdf)

#### FirmAgent: Leveraging Fuzzing to Assist LLM Agents with IoT Firmware Vulnerability Discovery.
- **作者**: Jiangan Ji, Chao Zhang, Shuitao Gan, Lin Jian, Hangtian Liu, Tieming Liu, Lei Zheng, Zhipeng Jia
- **方向**: 移动与物联网安全
- **解读**: 
IoT 固件漏洞检测存在根本性权衡：静态分析（含 LLM 方案）误报率高且不产出可验证的 PoC，而动态模糊测试受参数校验与配置条件限制，只能到达约 25% 的 sink 点，漏报严重。作者的关键观察是：模糊测试能高精度（约 90% 可达）定位接受外部输入的代码点，而静态分析擅长从这些点深挖路径——两者恰好互补。

据此设计 FirmAgent：预模糊分析用 LLM 辅助提取服务 handler、关键词字典，并计算基本块到 sink 的距离指导定向变异；QEMU 定制插桩在 sink 作用域内做内存污点检测，把观察到污点传播的指令地址记为 Csource，同时解析间接调用目标补全调用图；随后污点传播智能体（DeepSeek-R1）沿 Csource 到 sink 的潜在路径做上下文感知的污点分析（含反编译代码精化、告警验证、函数级缓存），PoC 生成智能体结合污点分析提取的约束与模糊测试可达用例，自动合成完整 PoC。

在 14 个真实固件上发现 182 个漏洞、精度 91%（45 个命令注入 + 137 个缓冲区溢出），其中 140 个为未知漏洞、17 个已获 CVE 编号；对比 EmTaint（10 个/37%）、HermeScan（71 个/33%）、Greenhouse（8 个/40%）、Hy-FirmFuzz（13 个/100%），数量分别提升 18.2×、2.6×、22.8×、14×。源点识别精度 100%、覆盖 94.2%，91.8% 的 PoC 直接有效（167/182 无需修改即可触发）。

我认为 FirmAgent 是 LLM 与动态分析结合的优秀范本："模糊测试给真源点、LLM 做语义污点分析、LLM 补 PoC"精准互补了各自短板，真实漏洞与 CVE 背书极具说服力。局限在于依赖单服务重宿主框架（Greenhouse 成功率有限、跨二进制漏洞不可见）；18 个误报全部来自缓冲区溢出，源于 LLM 难以处理跨函数作用域的全局变量；每固件约 1 小时的模糊测试时间成本也不低。
- **论文 PDF**: [FirmAgent: Leveraging Fuzzing to Assist LLM Agents with IoT Firmware Vulnerability Discovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1943-paper.pdf)

#### FirmCross: Detecting Taint-style Vulnerabilities in Modern C-Lua Hybrid Web Services of Linux-based Firmware.
- **作者**: Runhao Liu, Jiarun Dai, Haoyu Xiao, Yuan Zhang, Yeqi Mou, Lukai Xu, Bo Yu, Baosheng Wang, Min Yang
- **方向**: 移动与物联网安全
- **解读**: 
现有固件污点分析几乎只把 C 二进制纳入检测范围，而作者对 4012 个商业固件的大规模实证发现：38%（937/2461 个可解包固件）采用 C-Lua 混合架构实现 Web 服务，Lua 广泛承担 URI 分发与处理；且 34% 的 Lua 代码以字节码形式存在，其中 98% 被厂商定制混淆。传统 C 导向检测器系统性遗漏这一攻击面。

FirmCross 针对三个挑战逐一给出方案：①利用字节码不变量（长度头、固定 RETURN 终止、原型结构一致性）做结构去混淆，用标准与定制解释器编译同一源码的静态字节码 diff 推断数据变换规则，全程无需逆向解释器；②基于 Lua URI handler 注册的确定性模式（注册表参数+注册逻辑）识别 handler，再按参数的表结构与嵌套访问特征识别 Lua 表源；③按 C/Lua 规范的确定性模式（luaL_register、luaL_loadfile 等 API 序列与命令执行 IPC）构建跨语言通信模型，在字节码级统一做跨语言污点传播。

在 73 个固件、11 个厂商上检出 696 个漏洞、精度 33.27%，漏洞覆盖率是 MangoDFA 的 6.82 倍、LuaTaint 的 14.5 倍，其中 610 个 0-day、已获 31 个漏洞编号；316 个真实定制解释器全部通过去混淆验证（LuaHunt 为 0）；Lua 源识别数量 23,306 个（精度 0.84）对 LuaTaint 的 1,798 个（0.18）。去混淆后还从 16 个含混淆字节码的固件中挖出 154 个漏洞。

该工作把"字节码反混淆—源识别—跨语言数据流建模"做成首个针对 C-Lua 混合服务的自动化流水线，实证规模与发现数量有说服力，不变量+diff 思路远比 LuaHunt 的变异测试+人工逆向可扩展。局限：33.27% 精度说明误报仍高（源于源识别启发式与"参数污点即返回值污点"的过度假设）；MOD 指令建模不全会漏掉 LuaTaint 可发现的漏洞；清洗函数识别仍靠名称匹配。
- **论文 PDF**: [FirmCross: Detecting Taint-style Vulnerabilities in Modern C-Lua Hybrid Web Services of Linux-based Firmware.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1251-paper.pdf)

#### IoTBec: An Accurate and Efficient Recurring Vulnerability Detection Framework for Black Box IoT devices.
- **作者**: Haoran Yang, Jiaming Guo, Shuangning Yang, Guoli Zhao, Qingqi Liu, Chi Zhang, Zhenlu Tan, Lixiao Shan, Qihang Zhou, Mengting Zhou, Jianwei Tai, Xiaoqi Jia
- **方向**: 移动与物联网安全
- **解读**: 

现有 IoT 漏洞检测依赖固件或源码，而现实黑盒场景中固件常不可获取或被加密、高保真仿真困难，白盒/灰盒方法大多失效；纯黑盒 fuzzing 又因缺乏种子与先验知识而效率低下。IoTBec 提出首个固件与源码无关的重复漏洞检测框架：利用公开 CVE 报告与设备自身可观察接口信息。

核心是构建漏洞接口签名（VIS）：从 CVE 描述与公开报告中用 LLM 抽取 CVE ID、漏洞类型、受影响 POST 接口、关键参数构成漏洞签名（VS）；用深度优先遍历提取设备 Web UI 导航结构、POST 接口与请求数据包构成接口签名（IS）；通过匹配接口等关键字段融合为 VIS 并建库；命中后用 fuzzing LLM 生成定向 payload 验证。

在 Tenda、TOTOLINK、D-Link、TP-Link、Linksys 五家厂商的 27 台真实设备上，发现漏洞数是 SOTA 黑盒 fuzzing（boofuzz、Snipuzz）的 7 倍以上，100% 精度、93.37% 召回；共发现 183 个漏洞，169 个获 CVE ID（53 个新分配，平均 CVSS 3.x 8.61），覆盖缓冲区溢出、命令注入、CSRF；LLM 驱动 fuzzing 还额外发现 25 个库外漏洞，其中 4 个确认从未被报告。

把"重复漏洞"思路从固件迁移到黑盒接口是一次有价值的范式转移，LLM 同时承担签名生成与 payload 生成，自动化程度高。局限：依赖公开 CVE 信息质量（VULDB 2024–2025 年约 93.2% 的 CVE 含全部必需信息）与 Web UI 型设备（路由器为主），对无 UI 或小众设备覆盖有限。

- **论文 PDF**: [IoTBec: An Accurate and Efficient Recurring Vulnerability Detection Framework for Black Box IoT devices.](https://www.ndss-symposium.org/wp-content/uploads/2026-f634-paper.pdf)

#### MVPNalyzer: An Investigative Framework for Auditing the Security & Privacy of Mobile VPNs.
- **作者**: Wayne Wang, Aaron Ortwein, Enrique Sobrados, Robert Stanley, Piyush Kumar Sharma, Afsah Anwar, Roya Ensafi
- **方向**: 移动与物联网安全
- **解读**: 问题：VPN 应用必须拦截全部用户流量、处于特权位置，用户把信任从 ISP 转移给 VPN 提供商，但移动 VPN 缺乏系统性审计。

方法：MVPNalyzer 是可扩展三模块框架：用 LD_PRELOAD 钩 SSL_new 记录 TLS 会话密钥，绕过证书固定与 Frida 检测解密流量；结合 socket 元数据与归因管线区分 VPN 自身流量；对 281 个热门 VPN 做五类分析：隧道泄漏、通信安全、配置加固、数据外泄、抗检测。

结果：61 个应用明文传输（5 个明文下发配置文件，可被替换实现隧道劫持并实测）；29 个流量泄漏（24 DNS、6 用户流量、4 未加密隧道）；169 个可被简单 DPI 识别（45 个明确宣称不可封锁）；76 个传输 Advertising ID；108 个含 OpenVPN 配置的应用仅 1 个合规；总安装超 24 亿次。

我的思考：首个系统化移动 VPN 审计框架，规模与维度领先，TLS 密钥捕获与归因方法论可复用；结果反映行业级安全懈怠。局限：仅覆盖 Android、泄漏判据为启发式、未审计服务器端。

- **论文 PDF**: [MVPNalyzer: An Investigative Framework for Auditing the Security & Privacy of Mobile VPNs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1573-paper.pdf)

#### SECV: Securing Connected Vehicles with Hardware Trust Anchors.
- **作者**: Martin Kayondo, Junseung You, Eunmin Kim, Jiwon Seo, Yunheung Paek
- **方向**: 移动与物联网安全
- **解读**：**问题**：现代汽车将车外网络（EVN）与车内网络（IVN）融合以支持导航、诊断与 OTA 更新，EVN 管理器运行完整操作系统与多应用，攻击面急剧扩大：一旦 OS 或应用被攻陷，即可伪造能绕过网关静态过滤的控制消息，威胁刹车、转向等安全关键 ECU；传统网关"只信任简单隔离的遗留 ECU"的假设已被打破。

**方法**：SECV 在 EVN 侧以 ARM TrustZone 构建受信任的执行模块（SSMe），在内核内外层隔离的基础上接管所有 EVN→IVN 消息：按应用（PID–MID 映射）逐条做授权验证并附加密码学证明，同时撤销普通世界对外设的直接访问；IVN 网关侧由 EVITA 全级 HSM（SSMi）完成模块认证、密钥存储与签名验签，形成 TEE–HSM 双层信任链；采用 NAPI 式轮询批处理与零拷贝分割驱动降低世界切换开销。

**结果**：在 NXP S32G3 平台实现后，EVN→IVN 传输的几何平均开销仅 6.5%，极端突发下额外消息丢失 1.5%；HMAC 认证（32 字节消息）平均增加 181.26µs 往返延迟、网关 CPU 负载上升 8.61%；在真实 CAN 轨迹（CAN-MIRGU/OTIDS）回放中无可见吞吐下降或丢帧，而 CAN 消息通常容忍 10ms 延迟，满足实时性约束；系统调用开销几何平均 1.11×。

**我的思考**：这是少有的把 TEE 与车规 HSM 组合起来防御"EVN 管理器完全被攻陷"场景的运行时机制，分层信任设计务实且开销可接受；局限是假设 TEE/HSM 本身不可破、仅覆盖 CAN（Ethernet 仅讨论）、应用加载延迟达 23×，且依赖单一厂商平台，通用性与部署成本是实际落地的主要障碍。

- **论文 PDF**: [SECV: Securing Connected Vehicles with Hardware Trust Anchors.](https://www.ndss-symposium.org/wp-content/uploads/2026-f106-paper.pdf)

### 云与供应链安全（11 篇）

#### Action Required: A Mixed-Methods Study of Security Practices in GitHub Actions.
- **作者**: Yusuke Kubo, Fumihiro Kanei, Mitsuaki Akiyama, Takuro Wakai, Tatsuya Mori
- **方向**: 云与供应链安全
- **解读**: GitHub Actions已主导CI/CD市场，但SolarWinds（影响约18,000个组织）与2025年tj-actions/changed-files等供应链攻击频发。GitHub虽发布了官方安全实践，但其真实落地程度与阻碍因素此前完全未知，指南停留在理论层面。

作者采用混合方法：测量研究基于SEART-GHS分析338,812个使用GitHub Actions的公开仓库（含861,680个工作流与3,424,855个action引用），以人工验证100%准确的自动检测框架评估五项实践（P1 CODEOWNERS、P2脚本注入缓解、P3 OpenSSF Scorecard、P4第三方action固定、P5 Dependabot）并用逻辑回归建模仓库特征；用户研究对102名开发者展开问卷调查与质性编码。

结果显示五项实践实施率仅0.6%~52.9%（P3最低0.6%，P2最高52.9%）；近期活跃（持续维护）与组织型仓库更可能实施安全实践，工作流开发者数量比总贡献者数相关性更强，而星标数、代码规模、仓库年龄几乎无关联；主要障碍为认知不足（非实施者中21.3%~71.6%不知晓该实践，P3高达71.6%）、适用性误解（4例明确因误解而未实施）与运维成本担忧（P4-1的25.3%）；平均46.8%的不知晓者在了解后表示愿意实施；81.6%的受访者不做日志审计，且更偏好"人工审批"级自动化而非全自动。

我的思考：这是首个聚焦"预防性实践落地"而非"漏洞检测"的CI/CD安全实证研究，测量与用户研究互补、方法严谨，且结论已反馈GitHub形成平台改进；局限在于SEART-GHS偏向≥10星仓库、自愿参与带来实施率偏高偏差、自动检测仅覆盖可公开观测的实践；"人工审批级自动化"的偏好提醒我们，安全自动化的产品设计必须尊重开发者对最终控制权的需求，否则会引发通知疲劳与抵制。
- **论文 PDF**: [Action Required: A Mixed-Methods Study of Security Practices in GitHub Actions.](https://www.ndss-symposium.org/wp-content/uploads/2026-f483-paper.pdf)

#### Actively Understanding the Dynamics and Risks of the Threat Intelligence Ecosystem.
- **作者**: Tillson Galloway, Omar Alrawi, Allen Chang, Athanasios Avgetidis, Manos Antonakakis, Fabian Monrose
- **方向**: 云与供应链安全
- **解读**: 威胁情报（TI）生态由全球安全厂商与志愿贡献者组成，投入达数十亿美元，但其运作方式、动态与脆弱性缺乏系统理解；被动、局部视角的既有研究无法刻画 IoC 的全球传播。本文提出主动测量框架，追踪二进制在生态中的流转。
向 30 家厂商提交嵌入水印 IoC 的良性可疑二进制：Rocket 执行后生成携带沙箱指纹的域名并释放 Satellite 子文件，以链式标签构成溯源轨迹；Observer 观测提交→提取→分享→阻断/下架各阶段，用系统与网络指纹聚类识别厂商，并经负责任披露验证。
67% 厂商执行沙箱分析，但仅 17% 分享提取的 TI，30 家中仅 2 家促成域名下架；四个“枢纽”厂商主导再分发，分享延迟达数小时至数天，使阻断/下架时间最多增加 20%；2025 年初 90 天窗口内发现 800 余个样本内嵌沙箱规避地址，规避可使获得 TI 的厂商减少 25%。
首次以可追溯探针刻画 TI 供应链拓扑与瓶颈，量化“提取—分享—行动”断层，伦理设计周密；局限是样本仅 30 家厂商、指纹维度少，对厂商选择性分享的商业动机刻画有限；其蜜标化建议可直接落地为数据共享审计工具。

- **论文 PDF**: [Actively Understanding the Dynamics and Risks of the Threat Intelligence Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f102-paper.pdf)

#### Better Safe than Sorry: Uncovering the Insecure Resource Management in App-in-App Cloud Services.

- **作者**: Yizhe Shi, Zhemin Yang, Dingyi Liu, Kangwei Zhong, Jiarun Dai, Min Yang
- **方向**: 云与供应链安全
- **解读**:
超级 App（微信等）为小程序开发者提供云数据库、云存储与云函数等敏感云服务，并实行集中身份管理：由服务器下发用户身份，授权校验责任落在开发者云端代码上。开发者常误用该机制，把身份校验放到客户端、或用客户端参数冒充身份，导致敏感资源可被任意访问——此前无人系统研究该风险。

作者设计 ICREMiner，结合静态分析与动态探测：先做云 API 识别与数据流分析提取客户端云操作，再用三种方法挖掘隐藏能力（客户端未直接访问但可被注入触达的云数据库）——常用库名扩充、LLM 语义推断、同开发者关联；对云存储分析文件路径是否可猜测（时间戳、随机数等）；最后按四类不安全实践（客户端校验 UIC-1、客户端参数做身份 UIC-2、特权资源共享 SRA-1、敏感数据可写 SRA-2）与最小权限原则判定，并以零泄漏探测避免触碰真实数据。

对 1,248,815 个小程序（微信 985,503、抖音 83,312、支付宝 93,128、百度 86,872）的分析发现 22,695 个使用云服务，其中 2,815 个（12.40%）存在不安全资源管理，涉及 8,062 个不安全云操作，其中 97.26% 暴露敏感资源，人工复核无漏报。UIC-2 最普遍（1,896 个），可致免费支付、越权读取病历/简历、提权等危害；115 个受影响小程序用户超 10 万，累计影响超 7000 万用户；已披露修复 893 个小程序。

本文是首个对 App-in-App 云资源管理的大规模实证，四类不安全实践的归类清晰，LLM 推断隐藏库名的思路解决了"云侧不可见"的难题。局限是云侧代码不可达、写权限风险只能静态推断，伦理约束也限制部分攻击验证；对超级 App 生态安全治理有直接推动意义。

- **论文 PDF**: [Better Safe than Sorry: Uncovering the Insecure Resource Management in App-in-App Cloud Services.](https://www.ndss-symposium.org/wp-content/uploads/2026-s194-paper.pdf)

#### Bit of a Close Talker: A Practical Guide to Serverless Cloud Co-Location Attacks.
- **作者**: Wei Shao, Najmeh Nazari, Behnam Omidi, Setareh Rafatirad, Khaled N. Khasawneh, Houman Homayoun, Chongzhou Fang
- **方向**: 云与供应链安全
- **解读**:
无服务器（serverless）用户易受微架构侧信道攻击，前提是与受害者实例物理共位。既有研究聚焦攻击本身，忽略了如何通过普通用户接口操纵调度器达成共位这一前置环节，缺少通用方法论。

三阶段指纹识别：高频调用同一函数观察放置序列，揭示调用局部性（F1）与自动扩缩（F2）；宿主终止后交替调用同功能函数，比较冷启动位置（F3）与账号局部性（F4）；构造配置变体探测配置局部性（F5）。据此构造四类攻击：多函数散列（M1）、突发调用触发扩缩（M2）、精确复用受害者包配置（M3-1）、变体扩散覆盖（M3-2）。

模拟与50节点集群上，利用配置局部性成功率近100%，纯随机调度也不安全；散列攻击效率虽低，少量函数即可与约50%受害者宿主共位。Azure真实攻击中64个函数应用平均覆盖230台机器、1.8个受害者宿主，5次实验全成功，成本低于25美元。防御Double-Dip调度器把共位成功率从0.495/0.781/1.0降至0.056～0.306，高负载下暖启动率差距仅0.39%。

首个通用指纹化-攻击方法论，六条发现（纯随机不安全、局部性可精确利用、攻击不可迁移）对调度器设计有直接指导。局限：核心实验基于自研Dask实现与模拟器，商业平台仅验证Azure；Double-Dip为"软隔离"，成功率仍随账号数上升。

- **论文 PDF**: [Bit of a Close Talker: A Practical Guide to Serverless Cloud Co-Location Attacks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1376-paper.pdf)

#### Breaking the Bulkhead: Demystifying Cross-Namespace Reference Vulnerabilities in Kubernetes Operators.
- **作者**: Andong Chen, Ziyi Guo, Zhaoxuan Jin, Zhenyuan Li, Yan Chen
- **方向**: 云与供应链安全
- **解读**:
Kubernetes Operator常被授予跨命名空间高权限，其"声明作用域与实际逻辑作用域不匹配"可致跨命名空间引用漏洞：单命名空间租户借Operator之手操作未授权命名空间乃至集群级资源，实现权限提升；此前Operator攻击未被系统研究。

提出两种策略：不安全ns作用域引用（攻击者在自定义资源中设secretRef.namespace等字段引用受害命名空间资源，Operator以高权限代读改）与不安全集群作用域引用（ns资源诱使Operator为攻击者命名空间的ServiceAccount创建ClusterRoleBinding，直接获得集群权限）。配套约1500行QL规则的CodeQL套件建模5个K8s Go库，以过程间污点追踪检测，并识别动词与资源类型。

2268个Operator中318个（14%）潜在脆弱：196个（8.6%）ns引用、175个（7.7%）集群引用、53个（2.3%）兼具；抽检55个仅5个误报。最常被引用Secret（102个Operator）、Namespace（62）、ClusterRoleBinding（40），常见组合Get-Secret（97）、Create-ClusterRoleBinding（33）。获8项确认、7个CVE（Grafana tempo-operator、Red Hat observability-operator、NVIDIA ais-k8s等），波及Google与Red Hat。

首个系统Operator攻击研究，威胁模型务实（不假设先攻陷容器），测量与披露扎实，量化了"便捷性压倒安全模型"的生态代价；作用域对齐、准入Webhook等缓解可操作。局限：仅分析Go实现；318个中只验证55个、漏报难量化；可利用性依赖具体动词组合。

- **论文 PDF**: [Breaking the Bulkhead: Demystifying Cross-Namespace Reference Vulnerabilities in Kubernetes Operators.](https://www.ndss-symposium.org/wp-content/uploads/2026-f761-paper.pdf)

#### From Noise to Signal: Precisely Identify Affected Packages of Known Vulnerabilities in npm Ecosystem.
- **作者**: Yingyuan Pu, Lingyun Ying, Yacong Gu
- **方向**: 云与供应链安全
- **解读**: 
npm 是最大的开源生态（超 300 万包），依赖链深且交织，已知漏洞的传播面巨大；但现有 SCA 工具（如 npm audit）只做包级告警，无法判断漏洞代码是否真实可达，导致严重告警疲劳与"补丁瘫痪"——约四分之一的包版本依赖含已知漏洞的包。而函数级分析此前在生态规模下不可行（Jelly 在 4GB 内存限制下成功率仅 37.37%）。

VulTracer 的关键洞察是 npm 的 (package, version) 发布即不可变，可"分析一次、复用多次"：用 CodeQL 为每包独立预计算富语义图 RSG（把外部调用与导出语句实体化为顶点）；从中提取形式化接口契约（导出流形/导入流形+规范化 API 路径）；分析应用时按依赖图逆拓扑序把 RSG 按契约"缝合"成生态级调用图，定位漏洞传播路径并判定真实受影响的包。

调用图 F1 达 0.905（Jelly 0.731）、精度 1.000、跨包覆盖 65.08%（Jelly 58.67%）；比 npm audit 减少 94% 误报（JAM 为 81%）；分析范围从 26,653 个依赖包压缩到 506 个（-98%），按需合成仅 41.87s。全生态测量（3,267,273 包、34,685,976 版本、27 个 CVE）发现包级分析 68.28% 的潜在影响是"噪声"（漏洞代码不可达）；单跳平均过度近似 67.51%（高影响力 CVE 53.66%），如 lodash 的 template（CVE-2021-23337）仅 2.92% 的直接依赖真正调用漏洞函数；函数级真实传播平均仅 0.12-2.08 跳（包级 3.34-7.62），96.59% 的实际受影响包集中在 4 跳内。

"预计算+契约组合"把不可扩展的整程序分析转化为可横向扩展的生态级方案，94% 误报削减与 68.28% 噪声量化对 SCA 行业有直接指导价值，也用数据刻画了"漏洞传播浅、冲击衰减快"的规律（未用依赖多、API 使用浅）。局限：可达性不等于可利用性（作者已明确界定）；JS 动态特性仍带来残余漏报；RSG 预计算 174 分钟可摊销但需随新版本持续重算；分析依赖 CodeQL 对复杂 JS 特性的建模能力。
- **论文 PDF**: [From Noise to Signal: Precisely Identify Affected Packages of Known Vulnerabilities in npm Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1902-paper.pdf)

#### Mapping the Cloud: A Mixed-Methods Study of Cloud Security and Privacy Configuration Challenges.

- **作者**: Sumair Ijaz Hashmi, Shafay Kashif, Lea Gröber, Katharina Krombholz, Mobin Javed
- **方向**: 云与供应链安全
- **解读**: 云服务配置错误长期是安全与隐私事件的首要成因（如2024年AT&T因Snowflake缺少强制MFA泄露超1亿用户通话记录），但云操作者日常面临的具体配置挑战缺乏大规模实证刻画。该研究首次将真实世界的云使用场景与安全/隐私配置障碍系统映射，为改善云安全态势提供依据。

方法上采用三阶段混合研究：先从Stack Overflow收集2008年8月至2024年3月约251,900条云相关安全/隐私帖子（用GPT-4生成并经人工审校的433个关键词过滤，比Yang等人的标签共现法召回更全且覆盖其97.5%），再用LDA主题建模（旧/新数据集分别取50/20个主题）与625篇分层样本的扎根式定性编码，最后把use case与配置挑战按主题做保守上界映射。

研究归纳出7类使用场景与5类配置挑战：认证与访问控制、安全通信与加密、网络配置与管理、日志与监控、数据库备份与恢复；其中认证与访问控制横跨全部7类使用场景，是最普遍也最易错的难点（数据集内authentication关键词占比15.80%）。人因层面发现大量问题源于文档不实用/不完整/过于泛化、缺少上下文感知的工具与可用的代码库与界面，且2022年LLM出现后主题未见明显变化。

这是云安全人因研究的首个大规模实证图谱，把"漏洞数量"视角转向"操作者为何犯错"。局限：Stack Overflow用户不能代表全部云操作者（存在自我选择偏差）、上界映射偏保守、帖子标签质量影响数据完整性。其结论为设计上下文敏感的文档、AI辅助配置助手和更可用的认证工具提供了明确方向。

- **论文 PDF**: [Mapping the Cloud: A Mixed-Methods Study of Cloud Security and Privacy Configuration Challenges.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1302-paper.pdf)

#### RTrace: Towards Better Visibility of Shared Library Execution.
- **作者**: Huaifeng Zhang, Ahmed Ali-Eldin
- **方向**: 云与供应链安全
- **解读**: 现代软件高度依赖第三方共享库，运行时执行了哪些库函数缺乏可见性，给供应链攻击留下盲区（如 XZ 后门）；现有 tracer（ltrace、drltrace、ldaudit、IntelPT）大量漏报。

漏报根因包括：依赖符号表（常被剥离/尺寸不准）、无法监控加载早期（IFUNC/构造函数）与卸载期隐式调用、非常规调用（内部偏移进入、JMP 调用）。RTrace 基于 DynamoRIO：light 模式插桩分支/返回指令配合自适应边界检测；rich 模式用 Angr 推断签名、捕获参数与返回值重建调用图。

21个应用、92个共享库上 F1 全部≥0.92，最好现有工具仅0.74，精度召回均超0.92（IntelPT 最高召回0.58）；light 模式最大开销30倍、长任务约2倍，而 ltrace 在 nmap 达11,756倍；rich 模式117个函数仅12个参数/返回值错误。

首次系统量化现有 tracer 的漏报并给出可复现评估框架（调试符号 ground truth），light/rich 双模式分别支撑 CI/CD 预警与事后取证；局限在于仅支持 x86-64 SysV ABI、不支持浮点参数，rich 模式开销较高。

- **论文 PDF**: [RTrace: Towards Better Visibility of Shared Library Execution.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1243-paper.pdf)

#### The Dark Side of Flexibility: Detecting Risky Permission Chaining Attacks in Serverless Applications.

- **作者**: Xunqi Liu, Nanzi Yang, Chang Li, Jinku Li, Jianfeng Ma, Kangjie Lu

- **方向**: 云与供应链安全

- **解读**: 无服务器平台将基础设施与函数级开发解耦，函数级细粒度权限配置与云厂商集中式 IAM/RAM 访问控制模型之间存在结构性错配，导致函数普遍存在"风险权限"。攻击者可把多个各自看似正常但含风险权限的函数串联成攻击链，实现账户接管乃至跨账户横向移动，即 risky permission chaining attack；现有 IAM 工具只在角色层面检测单一风险权限，无法发现这类组合攻击。

作者提出攻击者中心的模态抽象：沿"攻击者能力（直接/间接）"与"拓扑（账户内垂直/跨账户水平）"两维度抽象出三种攻击模态——直接提权、劫持关键函数间接提权、经 Lambda Layer 等自动执行型共享资源跨账户传播。基于此构建"过滤-实例化"两阶段工具，先在精简权限图上保留高风险节点，再按模态语义搜索可执行攻击链；在 AWS 与阿里云官方生产级应用仓库部署验证。

分析 AWS 308 个应用（部署超 10 次且含自定义 IAM 策略）与阿里云全部 55 个应用，识别出 28 个存在可利用提权链的应用，其中 10 个获厂商确认，共获得 5 个 CVE（如 CVE-2024-37293、CVE-2025-45468）、6 次致谢与 1 笔赏金；200 个应用混合场景下仅需约 146 秒，可扩展性好。

我的思考：价值在于把抽象的"最小权限"原则转化为可检测的具体攻击链，用真实 CVE 证明该威胁不是理论风险而是结构性问题。局限：敏感权限集依赖已有研究；官方仓库样本偏向代表性应用；模态③依赖 Layer/镜像等自动执行机制。与 PMapper、Cloudsplaining 等相比，首次支持跨账户与多阶段链推理，但需开发者与租户主动集成审计。

- **论文 PDF**: [The Dark Side of Flexibility: Detecting Risky Permission Chaining Attacks in Serverless Applications.](https://www.ndss-symposium.org/wp-content/uploads/2026-s819-paper.pdf)

#### Understanding the Status and Strategies of the Code Signing Abuse Ecosystem.
- **作者**: Hanqing Zhao, Yiming Zhang, Lingyun Ying, Mingming Zhang, Baojun Liu, Haixin Duan, Zi-Quan You, Shuhao Zhang
- **方向**: 云与供应链安全
- **解读**:
代码签名本用于保障软件可信与完整性，却被攻击者滥用为恶意软件背书以绕过操作系统与杀软检查（Stuxnet、RedLine、NVIDIA 证书泄露等）；已有研究仅有 111 个被滥用证书、以案例驱动为主，缺乏大规模细粒度测量和对攻击者策略的理解，而代码签名生态又缺少公开数据集与 ground-truth。

本文构建大规模测量：从 VirusShare（2020.10–2024.10，690 万样本）与安全厂商（2006.05–2024.09，380 万）收集 3,216,113 个带签名的恶意 PE；创新性地以 CA 发布的吊销原因码为 ground-truth，结合威胁情报与公司信息提出细粒度分类，把 43,286 个被滥用证书分为五类：无效签名、不可信证书、窃取证书密钥、窃取开发者身份、伪造开发者身份，形成迄今最大的标注数据集。

低技术"无效签名"仍占 89.5%，但窃取证书/身份类样本量 2017–2023 较 2010–2016 增长 4 倍；波及 114 个国家、46 家 CA；吊销部署不足——仅 17.56% 被吊销，并首次揭示"幽灵证书"问题（CRL/OCSP 签名证书失效后无法吊销被滥用证书）；总结五类策略：跨国利用宽松身份验证（亚美尼亚、越南排名显著跃升）、短期证书降成本（T3/T4 一年期占 84.22%）、证书多态（3,484 个多态簇，59.12% 的被滥用证书参与，首次发现 315 例恶意软件用多态+视觉混淆规避吊销）、双签名提兼容（10.17%）、随检测演进切换策略；已向 Sectigo、GlobalSign、Entrust 等报告并获确认吊销。

以"吊销原因码"作 ground-truth 的标注方法解决了领域长期的数据瓶颈，是迄今规模最大、首个细粒度的滥用测量，多态证书+U+00A0 不可见字符等规避手法的揭示说明 CA 侧身份核验与"嫌疑实体清单"执行是薄弱环节；局限是数据依赖 VirusShare 与合作方样本、聚焦 Windows/Authenticode；提出的透明化与主动治理建议落地仍需 CA/B Forum 监管配合。
- **论文 PDF**: [Understanding the Status and Strategies of the Code Signing Abuse Ecosystem.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2857-paper.pdf)

#### VulSCA: A Community-Level SCA Approach for Accurate C/C++ Supply Chain Vulnerability Analysis.
- **作者**: Yutao Hu, Chaofan Li, Yueming Wu, Yifeng Cai, Deqing Zou
- **方向**: 云与供应链安全
- **解读**: C/C++ 项目常以源码拷贝方式集成第三方库（TPL），供应链漏洞可跨项目传播；但版本型 SCA（CENTRIS、OSSFP）只识别依赖、误报率高，代码型方法（Vuddy、Fire、AntMan）需全库扫描且不验证漏洞可达性。

方法上，作者提出"语义社区"新粒度：将项目调用图建模为社会网络做社区检测（选 Infomap），用结构+代码特征计算项目与 TPL 社区的相似度建立依赖；再在依赖社区内做克隆检测验证漏洞存在，执行"社区间+社区内"两阶段可达性分析。

在 80 个项目、484 个 TPL、862 条标注依赖上，SCA F1 达 0.72，较 CENTRIS（0.60）、OSSFP（0.68）提升 4–12%；漏洞检测 F1 比版本法高 44–48%、比代码法高 17–23%；并在开源项目中找到 32 个未修补漏洞上报厂商。

我的思考：首个整合漏洞检测与可达性分析的 C/C++ SCA 框架，社区粒度是函数级与文件级的巧妙折中；但静态调用图不完整（占漏报 32.84%）、社区粒度过粗（53.28%）仍是瓶颈，时间戳去重易受异常版本管理干扰。

- **论文 PDF**: [VulSCA: A Community-Level SCA Approach for Accurate C/C++ Supply Chain Vulnerability Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s613-paper.pdf)

### 认证与访问控制（8 篇）

#### CoT-DPG: A Co-Training based Dynamic Password Guessing Method.

- **作者**: Chenyang Wang; Fan Shi; Min Zhang; Chengxi Xu; Miao Hu; Pengfei Xue; Shasha Guo; Jinghua Zheng
- **方向**: 认证与访问控制
- **解读**: 问题：口令仍是主要认证手段，动态口令猜测（DPG）针对完全未知的目标站点，通过命中反馈动态拟合目标分布以扩大威胁；现有方法只从单一维度建模——自适应规则类（ARPG）利用字符层局部性、生成式模型类（DPG）利用特征层局部性，忽略了两种信息维度的互补效应，多维信息如何有效融合是挑战。

方法：作者首次将多视图学习中的协同训练（co-training）引入口令猜测：特征层设计基于反馈的增量训练（把新命中口令加入训练集并重训，适用于 Markov、PCFG、FLA、GPT 等模型）；字符层设计数据驱动的变换策略自动生成算法（用 Levenshtein 距离与 SequenceMatcher 从泄露数据提取 mangling 规则），并首创基于粒子群优化（PSO，300 策略×5 粒子群）的策略分布优化以缓解策略选择盲目性；两层交替迭代、互补学习并生成猜测。

结果：在 8 个真实数据集（共 6,690 万条明文口令）上，7 轮迭代、超过 10^8 次猜测后，CoT-DPG 在所有数据集上的破解率全面超过 PassGAN(DPG)、PassBERT(ARPG) 与 GuessFuse，绝对提升 6.4%–26.7%；协同训练相对单一维度增量训练相对提升 1.77%–35.71%（Craftrise 上 CoT-F 达 35.71%），且各变体比单模型增量训练快 1.2–2.4 倍。

我的思考：把多视图学习的协同训练理论落地到口令破解，思想新颖且工程完整，PSO 优化解决了"未知目标下哪些规则有效"的探索问题；但该成果本质同样增强攻击者手段，且依赖初始口令集质量与迭代轮数，冷启动效果有限；与 PassGPT 等大模型方法对比时未充分讨论开销，领域影响上可作为口令强度评估基准，但需警惕被滥用的双重性。

- **论文 PDF**: [CoT-DPG: A Co-Training based Dynamic Password Guessing Method.](https://www.ndss-symposium.org/wp-content/uploads/2026-s755-paper.pdf)

#### Know Me by My Pulse: Toward Practical Continuous Authentication on Wearable Devices via Wrist-Worn PPG.

- **作者**: Wei Shao, Zequan Liang, Ruoyu Zhang, Ruijie Fang, Ning Miao, Ehsan Kourkchi, Setareh Rafatirad, Houman Homayoun, Chongzhou Fang
- **方向**: 认证与访问控制
- **解读**:
可穿戴设备需要无感连续的生物认证；ECG 判别力强但需电极接触、采集不连续，PPG 无创且可嵌入腕带设备，但已有工作依赖 75–500Hz 高频采样与复杂深度模型，功耗与算力开销大，难以在功率受限设备上持续运行，且缺乏真实场景验证。

本文首次在智能手表 We-Be Band 上完整实现并评估基于 25Hz 低频四通道 PPG（两绿一红一红外，SFH-7072 传感器）的连续认证系统，用带注意力机制的 Bi-LSTM 从 4 秒窗口提取身份特征；除公共数据集 PTTPPG 外还自建 26 名受试者的 WeBe 数据集，并系统研究采样率与训练活动多样性对性能的影响。

平均测试准确率 88.11%、宏 F1 0.88、FAR 0.48%、FRR 11.77%、EER 2.76%；25Hz 较 512Hz 节省 53% 传感器功耗、较 128Hz 节省 19% 且精度不降，而 20Hz 时精度骤降、功耗节省已可忽略，确立了 25Hz 为实用下限；仅用静息数据训练的模型在运动中失效，活动多样化训练显著提升跨生理状态的鲁棒性。

我的思考：论文以量化实证回答了"频率—精度—功耗"三角问题，模型轻量可实时部署，属扎实的工程贡献，对可穿戴认证的采样率选型有直接指导意义。局限在于受试者规模（26 人）有限、跨设备/跨会话与长时漂移的泛化、运动伪影对抗以及攻击者伪造/回放心跳波形的鲁棒性仍是开放问题。

- **论文 PDF**: [Know Me by My Pulse: Toward Practical Continuous Authentication on Wearable Devices via Wrist-Worn PPG.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1087-paper.pdf)

#### NetCap: Data-Plane Capability-Based Defense Against Token Theft in Network Access.
- **作者**: Osama Bajaber, Bo Ji, Peng Gao
- **方向**: 认证与访问控制
- **解读**: 问题：企业访问令牌劫持（JWT/OAuth、Kerberos、SSH 等）源于两个根因：令牌不与进程绑定（环境信任）且生命周期长；补丁式防御无法根治。

方法：NETCAP 引入不可伪造的进程级 capability：用 Chaskey（128 位密钥）绑定 PID、服务地址与设备 ID；客户端 eBPF 把 capability 嵌入出站流量，P4 交换机在数据平面内联生成、验证、刷新，并抗重放、IP/PID 欺骗与能力洪泛。

结果：测试床上 OAuth、Kerberos、SSH 三类真实攻击的恶意流量全部被阻断（吞吐为 0）；每包仅增约 130ns，100Gbps 交换机达 99.9Gbps、支持 20 万并发连接，LANL 与 DARPA OpTC 重放下 FCT 无明显变化；纯 eBPF 方案延迟高 4 个数量级且 20K pps 饱和。

我的思考：把 capability 下沉到数据平面是首创，协议无关、不改应用代码，兼具安全与线速性能。局限：依赖 P4 交换机与客户端 eBPF，capability 每秒发送带来约 366MB/60s 开销，安全性依赖交换机内密钥。

- **论文 PDF**: [NetCap: Data-Plane Capability-Based Defense Against Token Theft in Network Access.](https://www.ndss-symposium.org/wp-content/uploads/2026-f273-paper.pdf)

#### One Email, Many Faces: A Deep Dive into Identity Confusion in Email Aliases.

- **作者**: Mengying Wu, Geng Hong, Jiatao Chen, Baojun Liu, Mingxuan Liu, Min Yang
- **方向**: 认证与访问控制
- **解读**: 邮箱地址是互联网身份通用标识，但别名（alias）机制在服务商与外部平台间存在认知错位：服务商把别名（如 alice+work@example.com）视为基址 alice@example.com 同一身份的入口，平台却往往将其视为独立用户。这催生两类风险：别名多账户滥用（AMulA，单基址注册海量账号薅羊毛/刷量/绕配额）与别名误认钓鱼（AMisA，伪造形似别名的地址诱骗信任）。

本文首次系统梳理该不一致：自建 SMTP 服务器对 28 家服务商做别名实测（覆盖前缀/中缀/后缀/大小写/域名五类变形、32 个符号），并在 18 个平台用半自动框架验证身份识别逻辑。发现只有 Gmail 完整文档化别名规则，8 家服务商静默支持未公开别名行为（如 Eclipso 用 13 个特殊字符做前缀别名），3 家文档不完整；全部 28 家都把用户名大小写视为相同（违反 SMTP 规范）；18 个平台无一能完全防御，9 个平台对任何服务商别名都失守，Cloudflare 反而过度激进地拒绝所有含"+"的合法地址。

现实数据：npm 有 23.39% 的地址是别名，GitHub 为 11.49%；单个基址 umekiyanai@gmail.com 衍生出 139 个 npm 账号、10 天内发布 3,904 个黑帽 SEO 垃圾包。用户研究（N=304）显示识别正确率仅 40.07%，自称了解别名的用户中 22.78% 认不出 Gmail 别名语法，"自以为懂"更显著抬高钓鱼易感性（12.63%→31.65%，高学历 37.5%、CS 学生 0%→35.29%）。

我的思考：该工作把"服务商-平台-用户"三方视角缝合起来，首次系统性量化别名不一致带来的身份混淆，OriginMail 工具与漏洞披露有实际价值。局限：平台样本仅 18 个、实测受 NAT 与验证码约束，用户研究有自选偏差。启示：别名缺乏统一标准是根源，平台端应按服务商规则归一化而非一刀切禁"+"，用户教育也应强调"别名机制因服务商而异"。

- **论文 PDF**: [One Email, Many Faces: A Deep Dive into Identity Confusion in Email Aliases.](https://www.ndss-symposium.org/wp-content/uploads/2026-s148-paper.pdf)

#### PIRANHAS: PrIvacy-Preserving Remote Attestation in Non-Hierarchical Asynchronous Swarms.
- **作者**: Jonas Hofmann, Philipp-Florens Lehwalder, Shahriar Ebrahimi, Parisa Hassanizadeh, Sebastian Faust
- **方向**: 认证与访问控制
- **解读**:
远程证明（RA）是验证远程设备完整性的基础机制，但现有协议普遍缺乏公开可验证性、要求交互并暴露设备身份；Ebrahimi等人（NDSS'24）解决前两点却忽略隐私，IoT群组还需众多设备产出单一证明。本文提出PIRANHAS，同时满足公开可验证、异步、匿名的单设备与群组证明。

方法用zk-SNARK把任意经典对称RA方案编译为非交互、公开可验证、匿名的方案：厂商预生成响应承诺入累加器并签名，设备零知识证明持有合法响应，PRF计算linkage tag防Sybil；群组方案用递归zk-SNARK逐跳聚合，验证者仅知设备数n=|L|U；并给出形式化安全证明。

实现基于Noir与Plonky2：单设备证明305ms、递归聚合356ms、验证仅30ms/2.7ms，证明14.5KB或约130KB；是首个同时具备群组、匿名、透明、非交互且支持任意拓扑的方案。

我的思考：通用编译器思路优雅，聚合标签的DLOG绑定处理巧妙，全/轻节点区分贴合异构IoT。隐忧：树莓派4上证明12.3秒仍吃力；匿名性依赖诚实厂商，半诚实模型与真实攻陷有差距。356ms聚合性能具备落地潜力。
- **论文 PDF**: [PIRANHAS: PrIvacy-Preserving Remote Attestation in Non-Hierarchical Asynchronous Swarms.](https://www.ndss-symposium.org/wp-content/uploads/2026-f526-paper.pdf)

#### Success Rates Doubled with Only One Character: Mask Password Guessing.
- **作者**: Yunkai Zou, Ding Wang, Fei Duan
- **方向**: 认证与访问控制
- **解读**: 肩窥、污迹、键盘音频等侧信道攻击可使攻击者获得密码的部分信息（长度、若干字符），但掩码口令猜测（mask guessing）的威胁长期缺乏系统研究，传统整密猜测模型也无法利用掩码后的上下文信息。

论文研究四种掩码猜测场景，提出神经网络模型 PassSeq 与概率统计模型 Kneser-Ney，利用双向上下文预测掩码字符，并提出基于极大似然的新猜测次数估计方法，在 15 个大规模真实数据集上评估。

10 次猜测内：当拖网攻击者知道 4 位 PIN 的字符组成（无序）时成功率提高 152%（14%→35%）；基于 PII 的定向攻击者知道密码长度时成功率提高 47%–82%；再额外知道一个字符时成功率普遍翻倍，达 7%–29%（可利用姐妹密码时达 33%–73%）；用 11 种真实键盘（Apple、Dell、Lenovo 等）的键盘音频复现侧信道攻击，PassSeq 使现有按键推断攻击额外提升 5.6%–166.7%。

这是首个系统的掩码猜测研究，揭示了"仅一个字符即可使成功率翻倍"的惊人威胁，对密码强度评估与登录界面设计有直接警示意义；但侧信道信息获取的现实难易度与场景普适性仍需更多实测支撑。

- **论文 PDF**: [Success Rates Doubled with Only One Character: Mask Password Guessing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1059-paper.pdf)

#### Targeted Password Guessing Using k-Nearest Neighbors.
- **作者**: Zhen Li, Ding Wang
- **方向**: 认证与访问控制
- **解读**: 用户日益倾向于重用或微调密码（21%–33% 的用户小幅修改、20%–59% 直接重用），但现有定向猜测模型只在相似口令对上训练（如 Shark0301→shark03），导致过拟合而忽视大幅但语义相似的重用行为（如 Shark0301→Bear03）。

论文提出非参数化的 k 近邻定向猜测方法 KNN-TPG：构建数据存储保留源密码的上下文向量与目标密码前缀，生成时检索 k 个最近邻向量引导下一字符输出；将其与 Transformer 字符级模型融合为 KNNGuess，并混合流行密码列表，首次同时建模轻微修改、流行密码与语义相似三类重用行为。

在 12 个真实数据集（含 48 亿密码，含 4iQ、COMB）的 13 种攻击场景中，100 次猜测内对普通用户成功率 25.40%、对安全敏感用户 10.26%，较最强对手高 8.52%–119.0%（平均 55.33%），较 Pass2Edit/PointerGuess 平均高 18.09%；1000 次猜测下平均提升 50.08%。

KNN 检索与神经模型的结合新颖，非参数机制天然缓解训练分布过拟合，实证规模扎实；但检索开销与数据存储规模的可扩展性、语义相似攻击在现实中的可观测性，仍是限制其普遍适用性的问题。

- **论文 PDF**: [Targeted Password Guessing Using k-Nearest Neighbors.](https://www.ndss-symposium.org/wp-content/uploads/2026-s2077-paper.pdf)

#### Vault Raider: Stealthy UI-based Attacks Against Password Managers in Desktop Environments.

- **作者**: Andrea Infantino, Mir Masood Ali, Kostas Solomos, Jason Polakis
- **方向**: 认证与访问控制
- **解读**: 密码管理器在浏览器中可通过校验网站域名天然抵御钓鱼攻击，但随着主流服务推出独立桌面应用，管理器开始提供面向原生应用的 universal autofill，而这一新环境的防护机制此前无人系统研究。本文首次对 1Password、Keeper、LastPass、KeePassXC、MacPass 五款主流管理器在 macOS、Windows、Linux 三大桌面平台的 autofill 功能做系统实证分析。

方法上，作者梳理各管理器的应用交互策略与防护层级，发现平台间安全机制差异悬殊：macOS 上 1Password 借助 OS 能力（检测同 Bundle ID 多进程、映射可信 Associated Domains、代码签名验证）实现较高安全性；Windows 因缺乏相应 OS 级机制普遍缺少校验。基于此，作者演示 PoC 攻击：恶意应用伪造窗口标题/Bundle 元数据通过校验，用合成键盘事件触发 Quick Access 等自动填充组件，再结合窗口操控技术隐藏界面，完全隐蔽地窃取凭据、OTP、信用卡与 vault 密钥。

攻击在全部场景均可复现：窃取一对凭据耗时 1Password 约 11 秒、Keeper 4 秒、LastPass 5 秒、KeePassXC 17 秒；还可获取系统口令实现本地提权，或在另一台设备上复制用户 vault。作者已向各厂商披露并提出缓解措施，部分厂商已启动修复并授予漏洞赏金。

我的思考：本文首次刻画"桌面 autofill"这一新攻击面，并指出根本症结——安全上限由 OS 平台能力决定（macOS 强、Windows 弱），而非管理器自身意愿。局限：威胁模型假设用户已安装恶意应用（传播方式不在范围），macOS 攻击需要 Accessibility 权限（常见但仍是前提），未覆盖移动端与更多小众管理器。与浏览器内 autofill 依托沙箱与域名校验相比，桌面环境的信任根缺失短期难以根治，本文的缓解建议为厂商提供了路线图。

- **论文 PDF**: [Vault Raider: Stealthy UI-based Attacks Against Password Managers in Desktop Environments.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1067-paper.pdf)

### 数字取证（2 篇）

#### Achieving Zen: Combining Mathematical and Programmatic Deep Learning Model Representations for Attribution and Reuse.
- **作者**: David Oygenblik, Dinko Dermendzhiev, Filippos Sofias, Mingxuan Yao, Haichuan Xu, Runze Zhang, Jeman Park, Amit Kumar Sikder, Brendan Saltaformaggio
- **方向**: 数字取证
- **解读**: 现有DL模型恢复工具（如AIP）只恢复数学表示（张量、权重、层、图结构），完全忽略模型的程序化表示（实现代码），导致含自定义算子的专有模型无法导出可执行格式（ONNX需要自定义算子定义）、无法重建运行环境，白盒分析工具无从下手。

作者提出ZEN：从内存镜像中同时恢复两类表示并合成"统一表示"指纹——枚举DL进程全部代码对象，递归提取其闭包中的函数/类/数据结构，按已恢复层类型过滤出模型专属代码，建立层到代码对象的双射映射；归因阶段把数学相似度、属性相似度与反编译字节码模糊比较（IBSS）相结合，在基座模型库中匹配最可能的基座；差异分析找出新增（Υ）与修改（ϵ）代码对象，marshal字节码生成补丁，运行时借助GC枚举替换__code__属性、用FunctionType构造器注入新函数，实现免源码的模型复用。

在21个模型（7基座+14定制，覆盖YoloV5/7/8、Resnet、MobileNetV2及Llama2、nanoGPT）上，归因准确率100%，即使定制程度高达83.3%（MobileNetV3）；21/21模型补丁复用后性能与部署时一致；统一表示对角相似度平均0.72（相邻基座仅0.16），足以区分同族模型；而ONNX方案对13/14个定制模型完全不可行。

我的思考："模型=数学表示+程序化表示"的统一指纹是原创贡献，使归因具备可量化的取证证据（如用YoloV10验证其对YoloV8/Ultralytics许可证的侵权场景），运行时打补丁免源码的设计实用性强；局限在于面向Python/PyTorch系与常见框架/OS假设、字节码版本差异需模糊匹配、加密与混淆场景下的鲁棒性仍待检验；作为首个打通"恢复-归因-复用"全链路的取证工具，其开源基座模型库模式值得后续工作借鉴。

- **论文 PDF**: [Achieving Zen: Combining Mathematical and Programmatic Deep Learning Model Representations for Attribution and Reuse.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1628-paper.pdf)

#### KnowHow: Automatically Applying High-Level CTI Knowledge for Interpretable and Accurate Provenance Analysis.

- **作者**: Yuhan Meng, Shaofei Li, Jiaping Gui, Peng Jiang, Ding Li
- **方向**: 数字取证
- **解读**:
APT 检测中，CTI 报告的自然语言高层知识（如 ATT&CK）难以自动应用到只能处理低层系统事件的溯源分析系统，语义鸿沟使 IoC 类方法过时快、人工规则类方法昂贵且不精确，纯数据驱动方法则误报多、结果不可解释。

KnowHow 提出 CTI 知识驱动的在线溯源分析，核心是通用失陷指标 gIoC，表示攻击的主体、客体与动作；通过把系统标识符（如文件路径）提升为自然语言术语（如 firefox→browser），系统事件可匹配到 gIoC 进而匹配自然语言描述的技巧；最后按技巧—战术—APT 生命周期的时序逻辑推理攻击步骤，剔除不合逻辑的告警。

在开源与工业数据集的 16 个 APT 攻击活动上全部准确检出，而现有基线均伴随大量误报；节点级误报较 NODLINK、KAIROS 分别减少 81% 与 90% 且节点级召回更高，图级精度/召回在多个数据集上达 1.00；对未知攻击与模仿攻击鲁棒；下游检测误报较 LADDER、EXTRACTOR 提取的知识分别降低 68% 与 79%。

我的思考："最小提升规则+NLP"桥接语义鸿沟的路线可扩展且优雅，gIoC 表示是核心创新，输出可解释、贴近分析师认知。局限在于评估数据集规模有限（Open-World 上唯一图级误报由安全软件更新所致），在线大流量下的可扩展性与对抗性日志注入、gIoC 抽取的完备性仍需进一步考察。

- **论文 PDF**: [KnowHow: Automatically Applying High-Level CTI Knowledge for Interpretable and Accurate Provenance Analysis.](https://www.ndss-symposium.org/wp-content/uploads/2026-s199-paper.pdf)

### 无线与通信安全（10 篇）

#### AirSnitch: Demystifying and Breaking Client Isolation in Wi-Fi Networks.
- **作者**: Xin'an Zhou, Juefei Pu, Zhutian Liu, Zhiyun Qian, Zhaowei Tan, Srikanth V. Krishnamurthy, Mathy Vanhoef
- **方向**: 无线与通信安全
- **解读**: Wi-Fi 客户端隔离（client isolation）是非标准化功能，厂商各自实现，安全保证不明；攻击者可能绕过隔离恢复内网中间人（MitM）能力，威胁同网客户端与后端设备。
对加密层、交换层、路由层三个边界做结构化分析：滥用共享 GTK/IGTK 向受害者注入广播封装帧（Abusing GTK）；利用网关做 IP 层“网关反弹”（gateway bouncing）注入；跨 BSSID 虚拟端口做 MAC 欺骗端口窃取（port stealing）拦截上下行；结合广播反射（Broadcast Reflection）回注被截流量。
测试的 5 台家用路由器、2 个开源固件及企业级设备全部至少中一种攻击；GTK 滥用对多数 OS 有效（Linux 的 drop_unicast_in_l2_multicast 可缓解 IPv4 部分）；在两所大学真实网络完成下行拦截验证；端到端 MitM 约 2 秒完成，受害者观看视频无明显卡顿；攻击者可窃取 RADIUS 预共享口令。
系统揭示隔离“非标准化+分层不一致”的根因，攻击链完整、实测充分；局限是依赖攻击者持有合法凭据或连接开放 SSID，部分攻击需同一 AP 多 BSSID；核心启示是隔离应标准化并在所有层一致实施（如多隔离域），Passpoint 也被证明存在设计缺陷。

- **论文 PDF**: [AirSnitch: Demystifying and Breaking Client Isolation in Wi-Fi Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1282-paper.pdf)

#### BLERP: BLE Re-Pairing Attacks and Defenses.
- **作者**: Tommaso Sacchetti, Daniele Antonioli
- **方向**: 无线与通信安全
- **解读**:
BLE是数十亿设备的无线安全基石，其安全依赖配对（建立长期密钥）与会话建立（派生会话密钥）。标准允许已配对设备重配对以协商新安全级别，但该机制的安全影响从未被研究，可被用于设备冒充与中间人攻击。

分析标准v6.1的重配对决策树，发现6个漏洞：4个新漏洞（V1/V2未认证的Central/Peripheral重配对、V3/V4安全级别降级），加扩展到重配对的V5（从会话建立触发重配对）与V6（PK熵降级）。据此构造4种BLERP攻击：Peripheral冒充（PI，首个滥用SMP安全请求、从会话建立发起的攻击）、Central冒充（CI）、单通道与双通道MitM，仅需0或1次点击。

低成本工具包（nRF52+NimBLE+Scapy）在22个目标上验证成功，覆盖15个Host、12个Controller、蓝牙4.2～5.4及SC/SCO/认证配对等最强配置，另发现Apple、Android与NimBLE（CVE-2025-62235）的实现缺陷。防御提出加固重配对与认证重配对协议（ProVerif验证）；SIG承认问题但不改规范，Apple/Google已修复，Logitech与Microsoft拒绝。

首个BLE-to-BLE重配对系统性安全评估，设计级缺陷影响所有合规设备，PI开创从会话建立突破配对的新路径，工具开源可复现。局限：部分新版本设备无法测试；I/O设备需一次点击确认；双通道MitM需双控制器；厂商分歧凸显规范修复的紧迫。

- **论文 PDF**: [BLERP: BLE Re-Pairing Attacks and Defenses.](https://www.ndss-symposium.org/wp-content/uploads/2026-f121-paper.pdf)

#### Formal Analysis of BLE Secure Connection Pairing and Revelation of the PE Confusion Attack.
- **作者**: Min Shi, Yongkang Xiao, Jing Chen, Kun He, Ruiying Du, Meng Jia
- **方向**: 无线与通信安全
- **解读**: 
BLE Secure Connection（SC）配对协议保护键盘输入、健康数据等敏感传输，但严格形式化分析面临三大障碍：协议横跨 Host/Controller/HCI 五主体六信道、配对方式（NC/PE/OOB/JW）选择逻辑难形式化、规范对用户行为假设过于理想化。

本文用 Tamarin 构建迄今最细粒度的符号模型：从规范提取各参与方状态机（发起方 Host 21 状态 30 迁移、响应方 18 状态 27 迁移），把设备拆为 Host 与 Controller 分别建模；用 2 个函数符号加 23 条等式形式化配对方式选择逻辑（经验证满足有限变体性质）；考虑可破坏 OOB/IO/HCI 私密信道机密性与完整性的更强敌手；并纳入用户可能使用弱密码或重用密码的现实假设。从 6400 种情形筛出 84 种，用自定义启发式、辅助引理与多服务器并行完成验证。

验证揭示新中间人攻击——PE confusion attack：敌手篡改 IO 能力字段，使发起方执行 PEID、响应方执行 PEDI、用户以为在执行 PEII（双方等待输入同一 passkey）。两种利用情形：用户输入弱密码（实测 Android 10-13 配对 UI 竟提示"Usually 0000 or 1234"），或短时间两次配对重用密码；影响 12 种配对情形，适用蓝牙 v4.2-v6.0。工作还复现 method confusion 与 LTK keysize confusion 攻击，并确定各情形最小安全假设（单向 OOB 须同时具备机密性与完整性，双向只需完整性）。

该工作的建模贡献甚至比攻击本身更重要：首次以等式理论忠实形式化配对方式选择逻辑，并把 Host/Controller 分离建模，比"整机单实体"模型更能反映 HCI 可被劫持的现实。PE confusion 攻击则说明"规范假设用户随机且不重用密码"与真实行为不符，设备 UI 甚至诱导弱密码。局限：验证耗时约 5 天、PE 流程抽象为两轮；攻击依赖用户弱密码/重用密码行为，可利用性受限；反制只能缓解而非根治。
- **论文 PDF**: [Formal Analysis of BLE Secure Connection Pairing and Revelation of the PE Confusion Attack.](https://www.ndss-symposium.org/wp-content/uploads/2026-f779-paper.pdf)

#### Passive Multi-Target GUTI Identification via Visual-RF Correlation in LTE Networks.

- **作者**: Byeongdo Hong, Gunwoo Yoon
- **方向**: 无线与通信安全
- **解读**: LTE 用 GUTI/S-TMSI 临时标识保护用户免受永久 IMSI 暴露，但标识隐私保护可被被动观察削弱。现有 IMSI catcher 需主动发射信号，GUTI 攻击大多假设已有受害者先验信息并主动触发寻呼，难以并行扩展到多目标且易被检测。本文形式化了新的被动多目标威胁模型：不发射任何信号，仅靠视觉观察与无线控制面消息的时序相关性解析 GUTI。

方法核心是把带毫秒时间戳的相机视觉观察与 USRP B210+SRS AirScope 被动捕获的下行控制面消息（DCI/PDCCH、RRC）对齐，FSM 算法先关联 RNTI 与 MAC 层竞争解决标识（即 S-TMSI），得到 RNTI↔GUTI 映射；每台设备最少观察 3 次交互即可解析并验证 GUTI，对消息丢失具有鲁棒性。测量覆盖两国四家商用运营商。

实测发现运营商很少更新 GUTI：最长持续 33 天不重分配，重分配模式还可预测；PCCH/RRC 唯一 GUTI 数量比达 10-38，说明寻呼区比小区大 10-38 倍，建立人-GUTI 关联即可实现从小区级到寻呼区级的分层位置追踪。定量结果：5 秒阈值下识别成功率 99.902%（4092/4096）；误报率 <0.1%；漏报率在 ≥6 次突发机会时低于 3%；100% 捕获率的实验中仅用 3 次数据事件即提取所有设备 GUTI。

我的思考：首个视觉-RF 相关的完全被动多目标 GUTI 识别方法，把"已有监控摄像头+廉价 SDR"升级为长期 RF 追踪能力，隐私影响显著且无 RF 足迹，已按责任披露通报 GSMA。局限：需相机视线覆盖、目标产生数据活动、SDR 处理率是成败关键（E11 因突发丢失失败）。缓解方向（频繁更新 GUTI）简单但实测运营商并未执行，5G 的临时标识行为也被引证存在类似问题，值得监管关注。

- **论文 PDF**: [Passive Multi-Target GUTI Identification via Visual-RF Correlation in LTE Networks.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2487-paper.pdf)

#### PriSrv+: Privacy and Usability-Enhanced Wireless Service Discovery with Fast and Expressive Matchmaking Encryption.
- **作者**: Yang Yang, Guomin Yang, Yingjiu Li, Pengfei Wu, Rui Shi, Minming Huang, Jian Weng, HweeHwa Pang, Robert H. Deng
- **方向**: 无线与通信安全
- **解读**:
无线服务发现（Wi-Fi、BLE等）明文广播与弱认证导致追踪、身份暴露与欺骗攻击；前作PriSrv用ACME实现双向策略控制，却受制于公共属性外露、二元向量、小宇宙、密文大与凭证复杂。本文提出PriSrv+及核心组件FEME。

FEME是首个支持无界属性宇宙（任意字符串作属性）与单调布尔策略的匹配加密：基于FABEO构建匿名KP/CP-ABE（部分隐藏，只露属性名藏值）与Hybrid-ABE（发送方认证），组合为FEME；双重再随机化与绑定防密钥提取、密文伪造与组件混用。PriSrv+以FEME替代ACME，配DH与MAC双向互认证。

FEME加密快7.62倍、解密快6.23倍，密文平均小87.33%；PriSrv+广播快7.17倍、互认证快3.32倍、总快4.05倍；mDNS包减少88.89%；四平台三曲线验证，有形式化证明。

我的思考：无界属性宇宙+部分隐藏+发送方认证补齐ME实用性短板，只露属性名保持高效过滤是精准权衡，回应CRYPTO'19/ASIACRYPT'22开放问题。局限：仍需可信KGC、基于配对的构造面临后量子迁移。总体为无线服务发现提供表达力与性能兼得的新基线。
- **论文 PDF**: [PriSrv+: Privacy and Usability-Enhanced Wireless Service Discovery with Fast and Expressive Matchmaking Encryption.](https://www.ndss-symposium.org/wp-content/uploads/2026-s87-paper.pdf)

#### Small Cell, Big Risk: A Security Assessment of 4G LTE Femtocells in the Wild.
- **作者**: Yaru Yang, Yiming Zhang, Tao Wan, Haixin Duan, Deliang Chang, Yishen Li, Shujun Tang
- **方向**: 无线与通信安全
- **解读**：**问题**：4G LTE 飞蜂窝（femtocell）已标准化并广泛部署，被核心网天然信任，一旦被攻破可监视/篡改几乎所有用户流量甚至攻击核心网；而此前研究聚焦需物理接触的本地攻击，缺乏对其远程攻击面与生态级风险的系统评估——2012 年 3G 飞蜂窝"被攻破即成为流氓基站"的威胁在 4G 时代是否依然存在是开放问题。

**方法**：作者对 4 款商用 4G 飞蜂窝做了硬件与软件结合的首次系统性安全评估，识别出 5 类关键共性漏洞（可访问调试接口、暴露的管理服务等），并在真实设备上实证；随后基于协议级特征设计互联网规模探测方法，识别暴露在公网的疑似飞蜂窝部署，并重新验证 3G 时代用户/网络威胁在 4G 下的延续性，与 3GPP 合作推动 5G 飞蜂窝安全规范（SCAS）工作项。

**结果**：4 款测试设备每款至少存在 2 个漏洞、部分 5 个全部命中，覆盖 20 个 OEM 厂商；互联网测量发现 86,108 个疑似飞蜂窝部署，其中 1,598 个开放管理接口、52,768 个（61.28%）高置信度确认，185 个与测试机型完全匹配且存在可恢复凭证、可被未认证接管；3G 时代针对用户语音/短信与网络的威胁在 4G 中依旧存在，并已向厂商披露。

**我的思考**：这是首个覆盖远程攻击面的 4G 飞蜂窝系统性研究，"海量设备暴露于公网且含可恢复凭证"的结论直接印证了运营商小型基站的现实风险，并为即将部署的 5G 飞蜂窝敲响警钟；局限是仅测试 4 款设备（代表面有限）、公网探测依赖协议指纹可能有误报，且未展示端到端利用链，但研究对标准化进程的影响（SCAS 工作项）是难得的落地成果。

- **论文 PDF**: [Small Cell, Big Risk: A Security Assessment of 4G LTE Femtocells in the Wild.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1968-paper.pdf)

#### Unknown Target: Uncovering and Detecting Novel In-Flight Attacks to Collision Avoidance (TCAS).
- **作者**: Giacomo Longo, Giacomo Ratto, Alessio Merlo, Enrico Russo
- **方向**: 无线与通信安全
- **解读**:
TCAS 是空中相撞的最后防线，飞行员必须无条件执行其决断告警（RA），但协议无认证无加密；此前实验室已演示注入攻击，CISA 咨询却评估为"仅限实验室、难以远程利用且无缓解手段"。本文直接挑战这两个论断，论证一次针对 TCAS 的空中网络攻击或已真实发生。

作者分析 2025 年 3 月 1 日华盛顿里根机场（DCA）异常事件（10 架飞机收到 RA、3 架复飞、持续 3 小时以上）的公开管制语音与 ADS-B 数据：110 条 ACAS RA 广播均把入侵者标为 Mode C 目标、固定高度 2300ft、中位距离 0.2 海里、方位 330°，但没有任何雷达回波与目视确认——与"幽灵飞机"注入签名高度一致；据此提出三种利用 Mode C 遗留行为的攻击变体：S1 立即应答（减距约 500m）、S2 模式判别（提前 13µs 应答，减距近 2.4km）、S3 抢先应答（减距超 3.5km，可远距离触发决断告警）；用 FPGA（RFSoC）实现最吻合观测的 S2，并提出纯靠现有 ADS-B 数据的粒子滤波定位方法 SMC-RAT。

S2 实验平均提前 τ=12.85µs、实现 1.9km 距离欺骗（理想值 78.7%–82.5%），经认证 Aeroflex IFR6000 测试仪验证协议完全合规；定位方法在 30 万次模拟中位精度 855 米，DCA 数据仅用两次飞机相遇即把信源约束到约 4.3 km² 区域、一小时内给出可行动定位。

把"实验室漏洞"推进到"已有真实世界攻击证据"的论证链条（多机型一致异常、几何无交点矛盾、地面发射机假说）严谨且克制；Mode C 攻击比 Mode S 简单得多——无状态交互、无需选择性询问、FPGA 资源占用不到 4.5%，可从 2km 外触发近距离警报，工程门槛显著降低；纯数据驱动的定位方法可直接落地取证；局限是实验在屏蔽室进行且未加功放、DCA 事件尚缺官方结论，但对航空安全政策（TCAS 认证与检测落地）有直接推动力。
- **论文 PDF**: [Unknown Target: Uncovering and Detecting Novel In-Flight Attacks to Collision Avoidance (TCAS).](https://www.ndss-symposium.org/wp-content/uploads/2026-f1806-paper.pdf)

#### WCDCAnalyzer: Scalable Security Analysis of Wi-Fi Certified Device Connectivity Protocols.
- **作者**: Zilin Shen, Imtiaz Karim, Elisa Bertino
- **方向**: 无线与通信安全
- **解读**: Wi-Fi Direct、EasyConnect、EasyMesh 承载数十亿设备（Direct 超 30 亿），但此前几乎没有对协议设计层面安全与隐私的系统性形式化分析；直接验证 Wi-Fi Direct 遭遇状态爆炸，属性验证耗 189–211 GB 内存仍被 OOM 杀死。

方法上，WCDCAnalyzer 将组合推理落地到密码协议验证：协议状态机按强连通分量（SCC）自动分解为子协议，合并共享密码原语的相邻 SCC 以保证原语不相交，安全属性分解为局部属性分别验证后合并，实例化到 Tamarin。

分析发现并验证 10 个新漏洞：Wi-Fi Direct 的降级认证绕过、UUID/MAC 隐私泄露、认证值/消息码 DoS，EasyConnect/EasyMesh 的引导密钥与 MAC 泄露等；19 台商用设备全部验证，Wi-Fi Alliance 已确认下版标准修复。

我的思考：首个覆盖三类 WCDC 协议规格的全面形式化分析，SCC+原语不相交分解让原本内存溢出的验证在 336.7 MB–4.59 GB 内完成（分解本身 <10s/20MB），理论结合工程极佳。但漏洞多为隐私泄露与降级类，主体密码设计尚可；对厂商实现差异无保证。

- **论文 PDF**: [WCDCAnalyzer: Scalable Security Analysis of Wi-Fi Certified Device Connectivity Protocols.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1049-paper.pdf)

#### WiFinger: Fingerprinting Noisy IoT Event Traffic Using Packet-level Sequence Matching.
- **作者**: Ronghua Li, Shinan Liu, Haibo Hu, Qingqing Ye, Nick Feamster
- **方向**: 无线与通信安全
- **解读**: 智能家居等 IoT 加密流量可被被动嗅探推断设备状态与用户活动，但 Wi-Fi 层噪声极大：最佳网卡也有 5–20% 数据包丢失，WPA 加密使上层标识不可区分，事件流混杂无关包；ML/DL 方法依赖调参与海量标注，"分块评估"高估真实连续跟踪性能。

方法上，WiFinger 将事件分类转化为子序列匹配：指纹为带相对时间戳、大小、方向的包序列，检测时只判断指纹"痕迹"是否出现；针对 NP-hard 的最长公共子序列设计 FMLCS 与 AFMLCS（锚约束+区间校准），用集体智能从几十个样本提取共识指纹，几乎无需逐设备调参。

在 15 台设备、31 个真实事件的连续追踪下，平均召回 89%、精度 96%，几乎零误报；多目标场景保持 86%/95%，而 Peek-a-boo 与 IoTBeholder 召回仅 49%/46%；对均匀与突发（Gilbert-Elliott）丢包均鲁棒。

我的思考：用"子序列痕迹匹配+少量样本共识指纹"替代端到端学习，规避标注成本与泛化难题，务实可扩展；对"分块评估高估性能"的揭示有方法论价值。局限是短指纹（2–4 包）对丢包敏感、连续跟踪误报累积，主动防御下鲁棒性待验证。
- **论文 PDF**: [WiFinger: Fingerprinting Noisy IoT Event Traffic Using Packet-level Sequence Matching.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1083-paper.pdf)

#### XR Devices Send WiFi Packets When They Should Not: Cross-Building Keylogging Attacks via Non-Cooperative Wireless Sensing.
- **作者**: Christopher Vattheuer, Justin Feng, Hossein Khalili, Nader Sehatbakhsh, Omid Abari
- **方向**: 无线与通信安全
- **解读**: **问题**：XR 设备日益普及并频繁接触密码、PIN 等敏感输入，但现有按键推断（keylogging）攻击——基于麦克风点击声、间谍摄像头、红外阵列或 CSI 感知——普遍受限于视线（LoS）与近距离（<10 米）；此前 VRSpy 等 CSI 方案还需攻击者自带收发设备贴近受害者布置，且须在受害者环境中采集训练数据，难以在真实场景规模化部署。

**方法**：攻击仅需一个低成本 ESP32（约 10 美元、40×27×17mm）接收端。核心是利用 WiFi 芯片组普遍存在的 Polite WiFi 漏洞：攻击者发送伪造的未加密 null 帧，受害头显会自动回传 ACK，从而被强制以 180–220 包/秒充当发送端，再用 beacon stuffing 防止其进入省电模式。信号处理上提出无监督的 TwiST 算法：对 AGC 校正后的 CSI 做滑动窗口稳定化、凝聚聚类（M=100）、构建转移图并经力导向投影重建键盘布局，以 Enter 键锚定方向/缩放/平移，300 次集成降方差，全程无需训练数据；按键时间戳则从云服务（如 Google 搜索）每次按键触发的网络包突发中提取。

**结果**：在 1–30 米距离、±90° 角度、穿墙与跨楼场景下评估 Meta Quest 2/3：跨楼攻击（30 米）达 Top-1 16.30%、Top-9 74.30%、Top-16 97.10%；全文平均 Top-4 40.77%、Top-16 91.54%，平均位置 L2 误差 4.113cm；1 米穿墙场景 Top-16 达 98.00%；15 字符内密码跨楼 top-25 准确率 78.6%；字典辅助攻击中位只需猜测约 24 个词（Top-500 达 97.14%）。相比需训练的 ResNet 基线，新环境下 L2 误差 4.1cm vs 7.5cm，泛化显著更好。

**我的思考**：该工作把 WiFi 协议层的"礼貌性"缺陷转化为攻击基础设施，通过迫使受害者设备成为发送端，同时解决近场支配效应带来的距离瓶颈与免训练泛化问题，是首个支持跨楼、穿墙的 XR 键盘记录攻击，单个 10 美元 ESP32 的部署形态极具现实威胁。局限：需先获知目标 MAC 地址；时间锚定依赖云服务网络活动（对银行等登录页须用搜索阶段打字节奏插值）；30 米跨楼时包速率降至 170 包/秒，50 米上限仅为推断；字典攻击依赖词表约束，随机字符密码下准确率将下降。

- **论文 PDF**: [XR Devices Send WiFi Packets When They Should Not: Cross-Building Keylogging Attacks via Non-Cooperative Wireless Sensing.](https://www.ndss-symposium.org/wp-content/uploads/2026-f926-paper.pdf)

### 数据安全（5 篇）

#### Breaking the Generative Steganography Trilemma: ANStega for Optimal Capacity, Efficiency, and Security.
- **作者**: Yaofei Wang, Weilong Pang, Kejiang Chen, Jinyang Ding, Donghui Hu, Weiming Zhang, Nenghai Yu
- **方向**: 数据安全
- **解读**:
生成式隐写术受"容量-效率-安全三难"制约：Huffman方法效率低、分布偏离；算术编码（AC）容量近熵但精度损失带来安全风险；可证明安全的方法又牺牲容量或效率，尚无方案三者兼得。

把非对称数字系统（ANS）编解码角色反转用于隐写：解码函数做嵌入（秘密消息驱动状态生成token）、编码函数做提取。四项创新：流式rANS（状态限界+Expand/Shrink重归一化）支持任意长消息；直接浮点运算免概率-频率换算失真；每步用CSPRNG掩码与状态低位异或，使采样残差可证明均匀（安全归约到CSPRNG）；M取2的幂使运算化为位操作提速。

在GPT-2、Llama 3、Qwen2.5等6个模型上，熵利用率UR约1.01（近熵），算法性KL散度严格为0（Huffman平均0.72、AC为10⁻²～10⁻⁴、SA-ANS约10⁻⁶），采样-推理时间比SITR=0.01（比SA-ANS低8倍）；Llama 3上嵌入率1.93比特/词元（SparSamp 1.77、iMEC 1.35）；NIST STS全套通过；4种隐写分析分类器准确率均约50%；并演示DDPM图像模态迁移。

以"ANS反转+CSPRNG掩码"同时打破三难，安全有严格归约证明，对照实验（含同期SA-ANS）坦诚全面，是隐写编码理论的实质推进。局限：对主动攻击（token插入/重排）脆弱，只适合可靠信道；解码要求模型与采样参数严格同步，跨硬件浮点非确定性即导致解码失败；密钥管理成为新瓶颈。

- **论文 PDF**: [Breaking the Generative Steganography Trilemma: ANStega for Optimal Capacity, Efficiency, and Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-f605-paper.pdf)

#### Enhancing Legal Document Security and Accessibility with TAF.
- **作者**: Renata Vaderna, Dusan Nikolic, Patrick Zielinski, David Greisen, BJ Ard, Justin Cappos
- **方向**: 数据安全
- **解读**: 
法律是社会运行的"操作系统"，但多数法律仍以纸面或老旧平台发布，数字法律库普遍缺少签名、时间戳、版本化和审计机制；攻击者可能篡改历史、回滚或冻结更新，甚至完全控制托管仓库。法庭与公民却依赖历史版本的精确措辞（UELMA 等法规也要求长期可验证性），而 Git 与 TUF 单独都无法认证仓库随时间的完整演化。

TAF 是首个面向"攻击者完全控制托管仓库"威胁模型的系统：结合 TUF 的软件更新保证、Git 的版本结构与强时间概念——时间作为绑定到具体仓库状态的签名数据，把法律文件的整个演化过程变成一条可认证、带时间戳的状态链，每个状态还绑定发布者定义的法律生效日期，支持可验证的 as-of-date 检索；任何删除、重排、改写历史都会破坏签名链而被发现。

安全分析覆盖 7 类威胁场景（TM-1 至 TM-7），除全部签名密钥被攻破外均被检测并阻止，无静默接受或部分回滚。性能上认证提交验证约 150–165 commits/s，最大部署（San Mateo，3346 个认证提交）20.8 秒完成验证，增量更新小于 300ms，内存峰值低于 250MB。系统已在 14 个美国辖区生产运行，包括巴尔的摩市、马里兰州和华盛顿特区。

把软件供应链安全（TUF）思想移植到法律完整性领域并叠加强时间绑定，是实质性的系统贡献，真实生产部署的验证远强于实验室演示。局限在于只适用于输入已知的发布流程，Windows 平台性能下降 1.5–2 倍；但它把"法律即代码"时代的完整性基础设施向前推进了一大步。

- **论文 PDF**: [Enhancing Legal Document Security and Accessibility with TAF.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1002-paper.pdf)

#### Phishing in Wonderland: Evaluating Learning-Based Ethereum Phishing Transaction Detection and Pitfalls.
- **作者**: Ahod Alghuried, David Mohaisen
- **方向**: 数据安全
- **解读**:
以太坊钓鱼自2017年以来占以太坊相关网络犯罪50%以上（如2022年Uniswap用户损失超800万美元），ML检测研究虽多但缺乏系统性审视：特征选取随意、数据集被操纵、鲁棒性评估缺失，"高指标"难持续。本文构建评估框架，围绕特征选择、类别不平衡、鲁棒性与算法优化四方面展开。

方法上以五个研究问题驱动，将特征按可操纵性分为易操纵（时间、邻居数、交易次数、方向）、易受影响（金额、地址）与稳健（成功交易、区块号）三类，在Eth-PSD与CTD数据集上用DT、KNN、LightGBM做5折交叉验证，并对照复现文献中的各种类别平衡手段。

结果显示：18个方案中4个无特征选择、50%用学习式方法；DT仅4个特征即达AUC 0.98、F1 0.96，最小特征对（区块高度、时间）0.97；去掉过采样使F1降约18%、AUC降0.07；数据集从50K扩至500K时AUC从0.90跌至0.75。

我的思考：这是难得的纠偏型测量，证实过采样与特征冗余虚高指标、精简特征几乎无损。局限在于鲁棒性实验仍属初步、数据静态回溯、与GNN检测器仅定性对比。总体为可持续评估建立了基线框架。
- **论文 PDF**: [Phishing in Wonderland: Evaluating Learning-Based Ethereum Phishing Transaction Detection and Pitfalls.](https://www.ndss-symposium.org/wp-content/uploads/2026-f694-paper.pdf)

#### Unshaken by Weak Embedding: Robust Probabilistic Watermarking for Dataset Copyright Protection.

- **作者**: Shang Wang, Tianqing Zhu, Dayong Ye, Hua Ma, Bo Liu, Ming Ding, Shengfang Zhai, Yansong Gao
- **方向**: 数据安全
- **解读**: 在 Data-as-a-Service 生态中，恶意数据中间商可售卖贡献者的数据却不告知、不分成。侵入式数据水印是数据集版权验证（DOV）的主流手段，但现有方案在低注入率（≤1%）下失效、损害模型效用、存在误报且不耐水印清洗。本文提出 DIP（Data Intelligence Probabilistic Watermarking），用分布感知样本选择算法挑选样本，将水印样本与多个目标输出按预设比例（如 2:8）概率关联，再以"预测结果+预测分布"两重验证提取水印信号，降低对注入预算的依赖并保持数据分布不变。

方法上，DIP 打破现有反制措施依赖的确定性特征映射：水印样本对应多个输出及其概率分布，单一目标的清洗或增强难以整体移除信号。DIP 可与 Patch、Blend、Physical、Dynamic、OOD 五种水印设计正交集成，并扩展到回归任务与 LLM 文本生成任务（GPT-2 微调、目标词 'NDSS'/'SSDN'）。

实验覆盖 4 个图像与 5 个文本数据集：1% 注入预算下平均水印成功率 89.4%（DIPhard 平均 WSR 88.3%、分布相似度 0.92；DIPsoft WSR 94.6%），测试精度下降低于 0.6%；低至 0.4% 注入率下 DIP 仍能完成验证，而 DVBW、UBW、DW、CBW 等基线 P 值均高于 0.05 失效。DIP 在 13 种对抗环境（3 增强、3 清洗、4 鲁棒训练、3 合谋移除）下保持鲁棒，基线全部失败。

我的思考：DIP 把 DOV 水印从"单目标后门"升级为"概率分布信号"，两重验证在极低注入率下仍给出统计显著结论，是少数同时满足低注入、鲁棒、无损效用、低误报的方法。局限：动态水印设计在 1% 注入下学习困难，P 值检验依赖足够黑盒查询量，实验模型以 ResNet/VGG/GPT-2 为主，更大规模 LLM 下的稳定性待验证。整体为 DaaS 版权保护提供了实用基准。

- **论文 PDF**: [Unshaken by Weak Embedding: Robust Probabilistic Watermarking for Dataset Copyright Protection.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1356-paper.pdf)

#### VICTOR: Dataset Copyright Auditing in Video Recognition Systems.

- **作者**: Quan Yuan, Zhikun Zhang, Linkang Du, Min Chen, Mingyang Sun, Yunjun Gao, Shibo He, Jiming Chen
- **方向**: 数据安全
- **解读**: 视频识别系统广泛部署于内容推荐与安防监控，大量高质量视频数据集被公开并受开源许可保护，却面临未授权商用等侵权风险。现有数据集版权审计聚焦图像与音频域，视频额外的时间维度使已有方法在有效性与隐蔽性上双双失效。本文提出 VICTOR，首个面向视频识别系统的数据集版权审计方法。

方法上，VICTOR 采用通用且隐蔽的样本修改策略：保留修改样本的原始标签以避免副作用，仅修改约 1% 的样本，向选中样本的全部帧注入程序化噪声（Perlin noise，ℓ∞ 预算 ε=10），放大已发布修改样本对目标模型预测行为的影响；审计时通过假设检验比较两类样本的输出差异，以评估模型辅助选择，后处理缓解误判。

实验在 HMDB-51、UCF-101、SSv2 三数据集与 I3D、SlowFast、TSM、TimeSformer 四模型上展开：仅修改 1% 样本即在多个数据集/模型上达到 TPR=F1=1.000、FPR=0.000（100% 审计准确率），而图像域基线 ML-DA 的 TPR 常为零、MT 误报率偏高；SSIM 显示 VICTOR 的扰动最平滑隐蔽（HMDB-51 0.813、UCF-101 0.798，均高于基线）；对输入预处理、训练干预、后调整三类干扰保持鲁棒，性能仅轻微下降。

我的思考：VICTOR 首次把版权审计扩展到视频域，标签不变+逐帧程序化噪声的设计同时满足隐蔽性与放大效应，工程完整（含阈值估计与后处理）。局限：审计依赖模型对修改/原始样本的输出差异假设，强重压缩、裁剪等预处理可能削弱信号；评估模型的选择影响效果；与图像域方法相比，更大规模模型上的查询成本与阈值估计鲁棒性仍需验证。视频时序维度的噪声注入也为攻击者提供了更多对抗面，值得后续研究。

- **论文 PDF**: [VICTOR: Dataset Copyright Auditing in Video Recognition Systems.](https://www.ndss-symposium.org/wp-content/uploads/2026-f746-paper.pdf)

### 社会工程与人类因素（5 篇）

#### Anchors of Trust: A Usability Study on User Awareness, Consent, and Control in Cross-Device Authentication.
- **作者**: Xin Zhang, Xiaohan Zhang, Huijun Zhou, Bo Zhao
- **方向**: 社会工程与人类因素
- **解读**: 跨设备认证（XDAuth：QR 扫码、推送、WebAuthn）将认证设备与目标设备解耦，产生信息不对称，用户难以判断请求合法性，可能误批恶意登录；且常缺失目标设备信息、显式确认与撤销机制。
提出“知情权、同意权、控制权”三项用户权利框架，用扎根理论导出 15 项可测指标，系统评估 27 个主流服务（QR/推送/WebAuthn 三类），并用 100 名美国参与者（Prolific）问卷加定性编码做用户研究。
无任何服务完全保障三项权利：过半（14/27）不提供目标设备信息，6 个无撤销能力（某头部短视频应用撤销后会话仍可读聊天记录）；知情权最弱、同意权最强；100 名用户中 98% 认为权利可增强安全、95% 认为可用性可接受，偏好统计显著（p<.001，风险差 +46%~+82%）；Zoho OneAuth、GitHub 等已正面回应。
首次系统研究 XDAuth 的可用性安全，权利框架有 GDPR/CPRA/NIST 理论根基，指标体系可复用；局限在于 27 个服务为抽样、评测含主观编码（但 Fleiss' κ=0.982）、用户样本教育背景偏高；对行业实践与标准化推动价值明显。

- **论文 PDF**: [Anchors of Trust: A Usability Study on User Awareness, Consent, and Control in Cross-Device Authentication.](https://www.ndss-symposium.org/wp-content/uploads/2026-f656-paper.pdf)

#### CtPhishCapture: Uncovering Credential-Theft-Based Phishing Scams Targeting Cryptocurrency Wallets.

- **作者**: Hui Jiang, Zhenrui Zhang, Xiang Li, Yan Li, Anpeng Zhou, Chenghui Wu, Man Hou, Jia Zhang, Zongpeng Li
- **方向**: 社会工程与人类因素
- **解读**: 加密货币钱包凭据钓鱼（CtPhish）损失巨大（如 Curve/Rabby/Ledger 仿冒案至少 12 万美元），而既有方案不适用：CES 依赖 typo-squatting（43% 的 CtPhish 网站无法由此生成），TxPhishScope 依赖链上交易，对凭据窃取型攻击天然失效。

CtPhishCapture 由五部分组成：以 CT 日志与搜索引擎 URL 快照双源收集（仅靠 CT 会漏掉 88%）；TF-IDF 加权、HTML 结构特征与贝叶斯后验概率的过滤（每日保留约 3000 URL）；GPT-4o 结合品牌知识库 RAG 的 LLM 检测（单页 1.8 秒、精度 100%）；Playwright 爬虫下载 APK；再以名称编辑距离、图标相似度与数字签名比对确认仿冒应用。

六个月部署（2024-06-23 至 12-15）发现 5,138 个网站与 10,612 个应用、零误报，其中 83%/79% 为此前未被社区报告的新发现，规模为此前最大工作的 30 倍；与百度合作后每周用户投诉由 20.55 降至 3.55（5.8 倍）；十个最大活动非法获利至少 611,186.90 美元。

该研究提供了迄今最完整的 CtPhish 生态测量（诱骗、信任构建、凭据窃取、规避手法与收益估计），工程与实证价值突出；局限是数据集以 Android 为主、收入估计为下界（仅 26 个蜜罐钱包），且攻击者会针对检测手段持续动态更新，系统需对抗演化。

- **论文 PDF**: [CtPhishCapture: Uncovering Credential-Theft-Based Phishing Scams Targeting Cryptocurrency Wallets.](https://www.ndss-symposium.org/wp-content/uploads/2026-f2854-paper.pdf)

#### From Perception to Protection: A Developer-Centered Study of Security and Privacy Threats in Extended Reality (XR).
- **作者**: Kunlin Cai, Jinghuai Zhang, Ying Li, Zhiyuan Wang, Xun Chen, Tianshi Li, Yuan Tian
- **方向**: 社会工程与人类因素
- **解读**: **问题**：XR 的沉浸式交互与多模态细粒度数据采集（注视、语音、生理信号等）带来与传统范式截然不同的安全隐私挑战。开发者是应用设计的关键决策者，但此前研究多为用户视角，缺乏威胁感知的开发者中心研究来揭示威胁产生的根源。

**方法**：从 412 名合格开发者中招募 23 名专业 XR 开发者，进行约 90 分钟的半结构化访谈（补偿 $70），先以问卷收集背景，再展示文献归纳的 9 类敏感数据、7 类攻击与现有缓解工具，并让开发者提出缓解方案；采用 MAXQDA 自下而上开放编码，最终形成 81 个代码、18 个主题，辅以 Mann-Whitney U 检验量化认知差异。

**结果**：开发者平均仅能识别 2.1 类 XR 敏感数据（展示后升至 4.8）；未提示时平均只回忆 0.9/7.0 类攻击，6/23 不知道任何 XR 攻击，3.7/7.0 类闻所未闻；12/23 不知道 GDPR、14/23 不知道 CCPA，无人知晓 XR 特定标准。熟悉攻击的重要性与可行性评分显著更高（p<0.001）。研究归纳出意识缺口与责任分散两大核心问题，并提出战略、技术、沟通三类支持建议。

**我的思考**：这是首个威胁感知的开发者中心 XR 安全研究，把认知偏差量化（如"本地应用天然安全""用户可自行缓解"等误解）并指出责任被转嫁给用户；局限是样本仅 23 人且偏年轻男性，女性开发者（4 人）提出的代表性不足问题未被放大验证；建议虽具体但缺少落地验证，可作为后续工具设计的起点。

- **论文 PDF**: [From Perception to Protection: A Developer-Centered Study of Security and Privacy Threats in Extended Reality (XR).](https://www.ndss-symposium.org/wp-content/uploads/2026-s807-paper.pdf)

#### Revealing The Secret Power: How Algorithms Can Influence Content Visibility on Twitter/X.
- **作者**: Alessandro Galeazzi, Pujan Paudel, Mauro Conti, Emiliano De Cristofaro, Gianluca Stringhini
- **方向**: 社会工程与人类因素
- **解读**: 社交媒体推荐算法不透明，平台可通过降低内容可见性（"影子封禁"）限流有害内容，也可能被用于压制异见；此前研究只能检测账号级干预，无法判断内容是否因特定特征（如外链）被系统性降权。

作者用浏览量定义 p-score（浏览量/关注者数）消除热度干扰，对乌俄战争（超1700万推文）与美国大选（约3500万推文）两个数据集共4000万+推文、900万+用户做量化分析，并用事实性/偏见标注与潜在意识形态估计推断内容与用户立场。

结果显示：含外链推文的中位 p-score 显著更低（乌俄约低8倍、大选约低4倍），且与意识形态、事实性无关；但账号层面差异显著：RT.com 与 The Kyiv Independent 差一个数量级（0.007 vs 0.084），特朗普（0.132）约为哈里斯（0.030）的四倍；社区层面无系统性惩罚（Pearson 最大0.13）。

这是少数直接利用浏览量系统度量影子封禁的工作，p-score 为同类研究提供了可复用度量；但作者承认无法确立严格因果，平台随时可改算法使结论时效性受限，账号差异也可能部分源于受众行为。总体以数据呼吁算法透明化，对内容审核公平性研究有重要参考价值。

- **论文 PDF**: [Revealing The Secret Power: How Algorithms Can Influence Content Visibility on Twitter/X.](https://www.ndss-symposium.org/wp-content/uploads/2026-s718-paper.pdf)

#### TBTrackerX: Fantastic Trigger Bots and Where to Find Malicious Campaigns on X.

- **作者**: Mohammad Majid Akhtar, Rahat Masood, Muhammad Ikram, Salil S. Kanhere

- **方向**: 社会工程与人类因素

- **解读**: X（原 Twitter）等社交平台上活跃着一类按"触发关键词"激活的脚本机器人（trigger bot, TB）：平时休眠，当用户帖子出现"metamask""cashapp""hacked"等关键词时自动回复，诱导用户支付非法产品费用或泄露金融凭证，构成加密货币诈骗、恋爱杀猪盘等恶意活动。已有研究多局限于单一加密货币诈骗场景，缺乏对整个 TB 生态的系统测量。

作者提出蜜罐驱动的测量框架 TBTrackerX：受控账户每 12 小时发布一次含触发关键词的推文，2024 年 3 月 1 日至 30 日部署，覆盖 10 个关键词（三种恶意活动与良性关键词），抓取蜜罐回复、账户档案、最近 100 条回复及关注者/关注网络；随后从档案特征、文本内容、时间模式、社交网络结构四个维度分析，并用 15 个基线模型（经典 ML、LLM、Transformer 等）评估检测性能。

捕获 4452 条蜜罐回复、2647 个唯一 TB 代理，涉及超过 8.4 万用户。恶意 TB 回复的语境相似度高达 0.95–0.97 而词法重叠率低于 0.30，说明系统化改写是其逃避手段；发布呈 15 秒至 5 分钟不等的突发模式，高峰后转入休眠；X 仅封禁了 57%–67% 的恶意 TB，640 个仍活跃。XGBoost 在恶意代理检测 F1=0.88、活动分类 F1=0.92。

我的思考：首次以蜜罐方法系统刻画了"代理—受害者—粉丝—主控者—农场"完整 TB 生态，并用高语义相似度/低词法重叠揭示了其改写逃避策略，对平台检测有直接参考价值。局限是单账户单月测量规模有限、关键词选择存在偏差，且最佳模型依赖文本嵌入等特征，外部部署者难以获得平台内结构数据。与 Botometer 等纯特征方法相比，融合文本语义是性能提升关键，但 TB 战术快速演化，检测模型需持续更新。

- **论文 PDF**: [TBTrackerX: Fantastic Trigger Bots and Where to Find Malicious Campaigns on X.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1239-paper.pdf)

### 程序分析与形式化验证（5 篇）

#### Analysis of the Security Design, Engineering, and Implementation of the SecureDNA System.
- **作者**: Alan T. Sherman, Jeremy J. Romanik Romano, Edward Zieglar, Enis Golaszewski, Jonathan D. Fuchs, William E. Byrd
- **方向**: 程序分析与形式化验证
- **解读**: SecureDNA 让 DNA 合成仪用分布式不经意伪随机函数（DOPRF）对订单与危险序列库做保密筛查以防范生物恐怖，但协议文档匮乏、安全目标未精确陈述，系统安全不能只靠 UC 证明的抽象密码。
结合源码（v1.0.8，约 6.4 万行 Rust）与抓包还原 SCEP、基本请求、豁免处理协议，用 CPSA（strand space）做首次形式化符号分析，并在 Dolev-Yao 模型下审查密钥管理、证书体系、认证与限速机制。
SCEP 仅实现单向认证——危险库与 keyserver 不知道与谁通信，可被恶意或沦陷节点冒充合成仪绕过限速（已有 POC）；响应缺乏强绑定，若同 TLS 会话重连可被重放/互换响应（现行实现禁止重连，属潜在漏洞）；其提出的 SCEP+（把双方 token 与 cookie 纳入签名哈希）经 CPSA 验证达成双向认证，v1.1.0 已采用。
案例完美说明“密码正确≠系统安全”；形式化分析先于攻击者发现类似 Needham-Schroeder 的协议交互攻击，是防御纵深缺失的典型；局限在于符号分析不覆盖实现错误与监控审计部分；SCEP+ 仅约 5 行改动即修复，凸显早期形式化验证的高性价比。

- **论文 PDF**: [Analysis of the Security Design, Engineering, and Implementation of the SecureDNA System.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1138-paper.pdf)

#### BINALIGNER: Aligning Binary Code for Cross-Compilation Environment Diffing.
- **作者**: Yiran Zhu, Tong Tang, Jie Wan, Ziqi Yang, Zhenguang Liu, Lorenzo Cavallaro
- **方向**: 程序分析与形式化验证
- **解读**:
二进制diffing对齐两个二进制中对应同一源码的CFG部分，支撑漏洞分析与补丁识别。跨编译环境下同一源码可能被拆分或合并为不同基本块，节点级匹配大量漏配误配；且现有方法依赖指令集特征，跨架构需重训、不灵活。

BINALIGNER首创图级匹配：节点分锚点（入/出度不全为1）与链节点，按相同入/出度枚举锚点对为初始子图对，再以两个条件松弛策略扩展——St.1st沿前驱/后继吸收链节点（应对块拆分合并、循环展开），St.2nd锚点邻域搜索（应对分支重排、predication）；节点嵌入用7种与指令集无关的统计属性，经Siamese结构（Structure2vec）生成子图嵌入、余弦相似度判定。

在GNU与OpenSSL数据集上覆盖跨版本、跨编译器、跨优化级别、跨架构：跨版本平均F1提升39.5%（最高66.4%）；跨架构指标全超93%、高于基线46.7%以上；编译环境演化下仍高出49.7%～78%；低相似度函数对场景F1超基线3倍；真实漏洞/补丁案例验证实用性。

首个图级匹配的跨编译环境diffing，松弛策略对CFG形变建模细致且有消融支撑。局限：统计属性在版本差距极大时信息量弱于神经网络嵌入（5.93 vs 8.30场景略逊）；不支持混淆代码；判定模型需按环境训练调参；跨架构基线不可用时的重构实现影响公平性。

- **论文 PDF**: [BINALIGNER: Aligning Binary Code for Cross-Compilation Environment Diffing.](https://www.ndss-symposium.org/wp-content/uploads/2026-s649-paper.pdf)

#### Discovering Blind-Trust Vulnerabilities in PLC Binaries via State Machine Recovery.

- **作者**: Fangzhou Dong, Arvind S. Raj, Efrén López-Morales, Siyu Liu, Yan Shoshitaishvili, Tiffany Bao, Adam Doupé, Muslum Ozgur Ozmen, Ruoyu Wang
- **方向**: 程序分析与形式化验证
- **解读**: PLC 程序对外设输入常"盲目信任"，缺失对越界/冲突传感器读数的安全检查，形成盲信漏洞（BTV）——波音 737 Max 事故即源于信任故障攻角数据；现有方法要么要求源码/设计文档，要么受状态爆炸困扰，且无法处理二进制。

Ta'veren 直接从 PLC 二进制恢复有限状态机：先按三条经验规则（存于非易失区、用于分支条件、先读后写）识别状态变量，再对扫描周期函数做符号执行，以"状态变量+输出变量"元组对具体状态去重得到抽象 FSM（定理保证状态变量完备时恢复健全），最后对恢复的 FSM 做模型检查验证安全策略（LTL 编码的 IllegalNode/IllegalTransition/Min/MaxDelay 等）。

在 22 个真实程序构建的二进制数据集（OpenPLC/Beremiz/Simulink 与手写 C/C++，覆盖 x86-64/ARM/MIPS/PPC/AVR8）上，20 条策略发现 23 个违规、其中 17 个确认为 BTV（范围处理不完整、输入检查错误、未处理输入组合、无条件接受输入、错误动作五类根因）；FSM 恢复 13/20 与参考完全匹配，策略验证不足 0.5 秒；对 ArduPilot rover/copter 也发现 3 个 BTV。

该工作首次实现免源码、免固件重托管的 PLC 二进制安全分析，开源数据集是领域稀缺资产；局限在于不支持西门子/罗克韦尔等专有 ISA、需要手工构造环境模型与策略、状态变量识别依赖经验规则，且最复杂目标（CarW）分析需约 10 小时，规模化仍有挑战。

- **论文 PDF**: [Discovering Blind-Trust Vulnerabilities in PLC Binaries via State Machine Recovery.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1624-paper.pdf)

#### Enhancing Semantic-Aware Binary Diffing with High-Confidence Dynamic Instruction Alignment.
- **作者**: Chengfeng Ye, Anshunkang Zhou, Charles Zhang
- **方向**: 程序分析与形式化验证
- **解读**: 
二进制 diffing 是补丁存在性检测、一洞多测、恶意软件分析等安全任务的基础技术，而足够数量的细粒度锚点（高置信度匹配对）能显著提升 diffing 精度。但现有锚点识别方法中，语法方法易被激进编译器优化击穿，语义方法要么计算代价高（符号执行需数小时），要么受迫执行覆盖率低。

作者的核心洞察是：并非所有动态语义都同等有效，可以只暴露部分运行期值来推导指令对齐。据此提出 Barracuda：先静态估计每条指令的 distinct 运行值数量，从值集小、更可能产生重叠语义的指令开始，优先采样覆盖其符号公式的执行路径；再基于部分语义构建二部图匹配（利用 biclique 结构）得到高置信度指令对齐。

Barracuda 比现有方法多检测 24.0% 的指令对齐锚点，精度达 92.1%；将锚点导入 DeepBinDiff 和 SigmaDiff 后，F1 分数分别提升 12.3%–42.7% 与 2.2%–4.1%。跨版本场景下 DeepBinDiff 平均 F1 从 50.9% 升至 68.4%，跨优化级别提升 27.6%，跨编译器提升 42.7%，且无需 GPU。

"部分语义足矣"的价值导向采样是真正的思想贡献，把动态分析从覆盖导向转变为收益导向，兼顾精度与成本。对严重依赖预匹配锚点的图匹配类工具（如 DeepBinDiff）增益最大，说明高质量锚点是这类系统的瓶颈。局限在于部分困难场景超出 24 小时时限标记为 N/A，且需访问 LLVM IR；整体上它为二进制 diffing 提供了一个通用、可插拔的增强组件。

- **论文 PDF**: [Enhancing Semantic-Aware Binary Diffing with High-Confidence Dynamic Instruction Alignment.](https://www.ndss-symposium.org/wp-content/uploads/2026-f663-paper.pdf)

#### What Do They Fix? LLM-Aided Categorization of Security Patches for Critical Memory Bugs.
- **作者**: Xingyu Li, Juefei Pu, Yifan Wu, Xiaochen Zou, Shitong Zhu, Qiushi Wu, Zheng Zhang, Joshua Hsu, Yue Dong, Zhiyun Qian, Kangjie Lu, Trent Jaeger, Michael J. De Lucia, Srikanth V. Krishnamurthy
- **方向**: 程序分析与形式化验证
- **解读**: Linux 安全补丁常被下游延迟移植形成可利用窗口，根因是难以快速识别 OOB（越界）与 UAF（释放后使用）等高危修复；CVE 滞后、缺失与静默修复加剧，SID 硬编码模式漏约 57% 补丁，TreeVul 无上下文（65.6%）。

方法上，DUALLM 为双管线：LLM 从提交标题/消息提取语义线索，线索不足则暂缓判定；定制切片精确截取补丁影响的代码上下文，喂给微调的小语言模型；两阶段（二分类+细粒度分类）融合输出。

在受控 CVE 补丁集上达 87.4% 准确率、0.875 F1（FP 5.4%），优于 SID（63.8%/0.546，FN 67.8%）与 TreeVul（65.6%/0.653）；从 5140 个近期补丁识别 111 个 OOB/UAF 修复，人工确认 90 个真阳性；为其中两个建 PoC，一个实现此前未知的控制流劫持；扩展 6 类漏洞仍达 80.35%，FFmpeg/OpenSSL F1 0.832/0.834。

我的思考："描述线索+代码上下文"融合是核心创新，消融显示组合比单模型提升约 11%，无线索补丁仍达 77.9% 精度，直击静默修复痛点。局限是假设输入已是安全补丁，真阳性依赖人工验证；是补丁移植优先级决策的实用工具。

- **论文 PDF**: [What Do They Fix? LLM-Aided Categorization of Security Patches for Critical Memory Bugs.](https://www.ndss-symposium.org/wp-content/uploads/2026-s328-paper.pdf)

### 其他（5 篇）

#### Automating Function-Level TARA for Automotive Full-Lifecycle Security.

- **作者**: Yuqiao Yang, Yongzhao Zhang, Wenhao Liu, Jun Li, Pengtao Shi, DingYu Zhong, Jie Yang, Ting Chen, Sheng Cao, Yuntao Ren, Yongyue Wu, Xiaosong Zhang
- **方向**: 其他
- **解读**:
随着车辆智能化，TARA（威胁分析与风险评估）成为 WP29 R155、ISO/SAE 21434 强制要求的合规环节，但现有自动化依赖静态威胁库，只支持车辆级评估，无法满足工业界所需的组件/功能级分析——后者要结合具体硬件、软件版本、通信通道与接口，而静态库难以维护且无法覆盖新攻击面。

本文提出 DefenseWeaver：用扩展的 OpenXSAM++ 格式结构化描述车载配置，按威胁场景提取逻辑路径（DFS 求入口到攻击终点的无环路径）并切分为原子结构；随后用多智能体 LLM 框架分工——子树构造器（按攻击面→局部目标→具体攻击方法，用 AND/OR 组织）、攻击树组装器（校验并连接子树、按用户约束剪枝）、风险评估器（按 ISO 21434 的五维可行性 ET/SE/KoIC/WoO/Eq 与四维影响给出 1~5 级风险）。构造器用 LoRA 微调（116 个专家场景、逾 1000 棵子树），评估器用 RAG 检索专家评分。

在四个真实车企项目（两台 BCM、CDC、PKES）中发现并经渗透测试验证 11 条关键攻击路径：如 Car A 利用网关 OpenSSL CVE-2016-6309 远程提权（风险 3 级）、Car D 结合 UWB 干扰与 BLE 中继实现免钥匙解锁（风险 5 级），均已上报并修复。跨域验证成功用于 PX4 无人机（复现 2 条已知路径并新增 1 条）与船舶 ECDIS；对比人类专家，新颖性 +105.00%、配置对齐 +43.68%。已集成于 UAES 与小米平台，生成超 8200 棵攻击树、日均评估 90+ 生产威胁场景。

用 LLM 的泛化知识替代静态威胁库是核心贡献，功能级粒度与"配置驱动生成"契合工业真实需求，且 11 条经渗透验证的路径提供了难得的实证强度。局限是评估依赖 LLM 输出质量（无独立验证标准）、源代码因商业许可未开源，人类对比样本有限；对攻击树"冗余 +0.90%"也提示生成内容仍需专家把关。

- **论文 PDF**: [Automating Function-Level TARA for Automotive Full-Lifecycle Security.](https://www.ndss-symposium.org/wp-content/uploads/2026-s1408-paper.pdf)

#### CAT: Can Trust be Predicted with Context-Awareness in Dynamic Heterogeneous Networks?
- **作者**: Jie Wang, Zheng Yan, Jiahe Lan, Xuyan Li, Elisa Bertino
- **方向**: 其他
- **解读**: **问题**：信任预测为决策与系统安全提供支撑，GNN 是主流方法，但现有模型多数不建模信任动态性、忽略网络异质性，且全部不支持信任的上下文感知属性，预测粒度粗。

**方法**：CAT 由四层构成：用连续时间表示与时间编码函数处理动态图；双注意力机制分别建模节点类型重要性与类型内节点重要性；引入新元路径（u-i-u）提取上下文特征，构造上下文嵌入并集成上下文感知聚合器，可同时预测上下文信任与总体信任。

**结果**：在 Epinions、Ciao、CiaoDVD 上全面优于五组基线，未观测用户预测在 Epinions 上 MRR 较最佳基线提升 50.79%；平均运行时间降低 73.97%（443K 边图每 epoch 约 48s vs HGT 的 194s）；信任导向与 GNN 导向攻击下最大性能下降仅 0.95% 与 3.39%。

**我的思考**：首次把动态性、异质性、上下文感知统一进一个框架，消融实验证明各组件有效，鲁棒性与可扩展性数据扎实；局限是上下文信任缺乏真值标签只能案例研究、上下文依赖物品类别设定、一跳传播是精度与扩展性的折中。总体属系统整合型贡献，方法新颖度中等。
- **论文 PDF**: [CAT: Can Trust be Predicted with Context-Awareness in Dynamic Heterogeneous Networks?](https://www.ndss-symposium.org/wp-content/uploads/2026-f2171-paper.pdf)

#### HOUSTON: Real-Time Anomaly Detection of Attacks against Ethereum DeFi Protocols.
- **作者**: Dongyu Meng, Fabio Gritti, Robert McLaughlin, Nicola Ruaro, Ilya Grishchenko, Christopher Kruegel, Giovanni Vigna
- **方向**: 其他
- **解读**: **问题**：DeFi 攻击每周造成数百万美元损失（累计超 50 亿美元），许多漏洞与协议业务逻辑深度耦合，现有模式匹配工具（如 DeFiRanger）只能识别已知模式，异常检测则缺乏可解释性且误报率高，实时检测攻击事务仍具挑战。

**方法**：HOUSTON 自动识别构成 DeFi 应用的合约集合后，用两个模型刻画协议正常行为：Interaction Model 提取"关键调用"指纹——只保留来自协议外部且修改存储或触发 ERC20 转账的调用序列（Euler 攻击的 150 次调用被压缩为 8 次调用的指纹）；Invariant Model 以 Daikon 风格增量挖掘函数参数与存储变量间的似然不变量（四类 C1–C4），并用 GPT-4.1 预筛语义有意义的变量对；两模型独立判定、告警可解释，持续更新并设等待期抑制冷启动误报。

**结果**：在 115 个 DeFi 事件、2,200 万+事务基准上，TPR 达 94.8%（109/115）、FPR 仅 0.16%（13,827/858 万，平均每协议每天 0.4 个）；检测信号中 52% 为因果、39% 为指示性、8% 为偶然；LLM 预筛使不变量模型误报降 27.2%（成本约 $100）。对比中 APE 仅防住 20 个攻击中的 4 个而 HOUSTON 标记 19 个，BlockGPT 仅检出 28 个中的 15 个（HOUSTON 27 个），TXSPECTOR 仅 21.7%；实时部署 20 个协议 20 天仅 75 条告警（0.08%），并成功移植 BSC（11/12 检出，FPR 0.19%）。

**我的思考**：以"语义信号"（控制流指纹＋存储不变量）为依据是核心洞察，可解释告警使溯源与筛除可行，Euler、Revest 案例展示与漏洞根因的精确对应；局限是依赖合约源码/ABI/存储布局、冷启动期误报集中、过滤 ERC20 调用导致 snood 漏报，与 BlockGPT 的比较因对方未开源存在偏差；整体是部署可行、证据扎实的实时防御工具。

- **论文 PDF**: [HOUSTON: Real-Time Anomaly Detection of Attacks against Ethereum DeFi Protocols.](https://www.ndss-symposium.org/wp-content/uploads/2026-f1534-paper.pdf)

#### Light into Darkness: Demystifying Profit Strategies Throughout the MEV Bot Lifecycle.

- **作者**: Feng Luo, Zihao Li, Wenxuan Luo, Zheyuan He, Xiapu Luo, Zuchao Ma, Shuwei Song, Ting Chen
- **方向**: 其他
- **解读**:
无许可区块链的透明性使机会主义交易者可通过竞争抢先交易提取最大可提取价值（MEV），MEV 机器人持续运作引发 gas 战争、用户与流动性提供者损失、甚至激励矿工分叉，损害共识安全与系统效率；但已有研究仅做 MEV 市场的宏观测量，机器人具体策略类型与分布仍属未知。

论文提出 APOLLO，首个结合合约代码与交易数据、覆盖机器人全生命周期（出生—运行—死亡）的策略分析工具；定义了 11 类生命周期策略（含 20 种代码级策略）；首次尝试智能合约去混淆，用 EVM 操作码马尔可夫转移矩阵+CNN 自动分析混淆字节码；并设计基于关联规则学习（ARL）+人工验证的半自动流程发现五类催化剂交易。

对 2052 个以太坊 MEV 机器人（1283 套利、605 三明治、291 清算，合计 2516236 笔交易、约 40M 美元利润）分析发现：16.3%（334 个）使用混淆保护代码逻辑；24.7%（506 个）在执行时内嵌关键参数计算与策略选择，75.3%（1546 个）仅作为执行代理、逻辑全在链下；催化剂交易包括大额交易、新山寨币交易、失败交易后同发送者交易、缺少执行链检查的交易与 gas 价过高交易。

我的思考：首次把 MEV 策略从宏观测量下沉到代码级系统实证，去混淆方法为链上逆向分析开辟路径，五类催化剂交易对普通用户的交易安全有警示价值。局限在于策略发现依赖人工+聚类的半自动流程，存在选择偏差，覆盖的链与机器人子集有限；对 PBS、加密 mempool 等缓解机制的有效性评估是自然延伸。

- **论文 PDF**: [Light into Darkness: Demystifying Profit Strategies Throughout the MEV Bot Lifecycle.](https://www.ndss-symposium.org/wp-content/uploads/2026-s506-paper.pdf)

#### MEVisor: High-Throughput MEV Discovery in DEXs with GPU Parallelism.
- **作者**: Weimin Chen, Xiapu Luo
- **方向**: 其他（区块链/DeFi 安全）
- **解读**: 问题：MEV 攻击者须在以太坊约 12 秒的出块窗口内从指数级搜索空间寻找套利/三明治机会；现有工具依赖 CPU 执行 EVM，频繁分叉致吞吐极低（Lanturn 30 txs/s）。

方法：首次把 MEV 搜索搬上 GPU：将 MEV bot 合约编译为 GPU 程序，8,192 线程并行遗传算法；设计 vm.swap/vm.apply 作弊码在 GPU 模拟交易、内存管理器消除动态分配，搜索后才在 CPU 分叉上验证。

结果：3,941 个真实案例上达 1.43M/1.73M bundles/s、交易级 3.3M–5.1M txs/s，比 CPU 基线快约 100,000x，GPU/CPU 执行吻合 99.52% 且只低估；2025 Q1 在 648,000 个区块中估计机会含 2–14 笔交易，挖掘 426.43 ETH（约 112 万美元）利润。

我的思考：首个 GPU 化 MEV 检测器，把搜索转化为 SIMD 并行负载，保守低估加 CPU 复核避免假阳性。局限：仅支持 Uniswap V2/V3、Sushiswap 与两类策略，V3 以禁用 tick spacing 换取性能。

- **论文 PDF**: [MEVisor: High-Throughput MEV Discovery in DEXs with GPU Parallelism.](https://www.ndss-symposium.org/wp-content/uploads/2026-f93-paper.pdf)
