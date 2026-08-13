---
title: NDSS 2025 录用论文深度解读（211 篇）
date: 2026-08-13 00:00:00
categories: Security
tags: [安全, 论文解读, NDSS, 2025]
---

NDSS 2025（第 32 届网络与分布式系统安全研讨会）于 2025 年 2 月在加州圣地亚哥举行，共录用 211 篇论文。本文按研究方向分类，对每篇论文基于全文精读给出深度解读（含独立分析思考），并附可获取的 PDF 链接。

## 全部 211 篇录用论文深度解读（按研究方向分类）

### AI/ML安全（32 篇）

#### VoiceRadar: Voice Deepfake Detection using Micro-Frequency and Compositional Analysis

- **作者**: Kavita Kumari, Maryam Abbasihafshejani, Alessandro Pegoraro, Phillip Rieger, Kamyar Arshi, Murtuza Jadliwala, Ahmad-Reza Sadeghi

- **方向**: AI/ML安全

- **解读**: TTS 与语音转换（VC）模型的进步使语音深度伪造（audio deepfake）能以假乱真，攻击者可冒充公众人物散布虚假信息、绕过声纹认证（如拜登语音机器人电话事件），造成广泛社会影响。但现有检测方法泛化能力差：Bhagtani 与 Muller 等的评测表明，合成语音检测器面对未见数据准确率显著下降。本文提出 VoiceRadar，首个将物理模型（多普勒效应与鼓面振动）引入深度学习训练的语音深度伪造检测方法，以"微频率"特征捕捉 AI 语音难以复刻的人类语音细微动态。

方法上，VoiceRadar 把音频嵌入中的每个唯一值视为同心圆模型中的离散半径（小值近声源、大值近观察者）：用多普勒效应计算声波从声源（人或 AI）传播到观察者的频率偏移，用鼓面振动模型分解每个半径上的复合微频率，捕捉节律节拍、细微变化与音调起伏（平移、旋转、振动三种偏差，扩展自同组针对 ChatGPT 文本的 DEMASQ，仅能捕捉平移偏差）。聚合得到"观测频率" fo，作为正则项整合进 ML 模型的损失函数——AI 生成音频的 fo 分布更均匀且低于人类音频（实验中以 7.5 为阈值即可区分），从而驱动模型学习人类语音的自然频率分布。

作者构建了含 50 万+ 样本的新基准数据集（12 个 2021 年后最新的生成工具：8 个 TTS——VALL-E-X、Bark、SpeechT5、StyleTTS2 等，4 个 VC——DiffVC、DiffHierVC、HierSpeech++ 等；真值来自 VCTK 109 名英语母语者）。在新数据集上，VoiceRadar 对 TTS 组合样本 EER 仅 0.45、TPR 99.57%、TNR 97.49%、F1 0.99，而最佳基线 wav2vec 2.0 的 EER 为 6.1；VC 组合样本 EER 0.016、TPR 99.88%；跨工具泛化实验中（用部分工具训练、未见工具测试）TTS 侧 EER 低至 0.0006/0.0069。在 ASVspoof 2019 基准上 VoiceRadar EER 0.10，远优于 RawGAT-ST（1.05）与 AASIST（0.83）。对抗鲁棒性方面，对 PGD 式对抗样本（ϵ=0.01）检测 TPR 达 100%、EER 0.0%，对"过拟合攻击"（针对目标人物微调生成）TPR 97%、EER 0.0%。

我的思考：VoiceRadar 的价值在于用物理先验知识引导学习，而非堆数据与参数——把"人类发声的物理规律"编码进损失函数，从原理上抓住 AI 语音"频率动态缺失"这一弱点，这解释了其跨工具泛化与对抗鲁棒性双双突出的原因。与 AASIST、RawGAT-ST 等纯数据驱动方法相比，物理约束使其对未见生成器与对抗扰动天然稳健，是"物理知识 + 深度学习"融合的出色示范。局限同样明显：训练/评估均基于 VCTK 英语语音，跨语言、真实录音环境（噪声、压缩、电话信道）的鲁棒性未验证；fo 阈值 7.5 是否随语种/采样率漂移存疑；对抗攻击仅测试了白盒 PGD 与过拟合两类，黑盒/物理域攻击未覆盖；50 万样本的"新数据集"使与既有方法的对比在数据分布上不完全公平（作者以 ASVspoof 上的复现对比部分缓解）。总体而言，该方法为深度伪造检测提供了可解释、可迁移的新范式，也提示生成侧研究者：补齐频率微观动态可能是下一代语音合成需攻克的硬骨头。
- **论文 PDF**: [VoiceRadar: Voice Deepfake Detection using Micro-Frequency and Compositional Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-3389-paper.pdf)

#### URVFL: Undetectable Data Reconstruction Attack on Vertical Federated Learning

- **作者**: Duanyi Yao, Songze Li, Xueluan Gong, Sizai Hou, Gaoning Pan
- **方向**: AI/ML安全
- **解读**: 纵向联邦学习中各客户端持有同一批样本的不相交特征，恶意主动客户端可发起数据重建攻击窃取私有特征；现有恶意攻击会替换训练任务，易被 SplitGuard、Gradient Scrutinizer 检测识破。

URVFL 用带辅助分类器的判别器（DAC）生成恶意梯度：先在辅助数据上预训练编码器/解码器与自身底层模型，再以 DAC 取代顶层模型，把编码器嵌入分布"移植"到被动客户端模型；标签信息既使恶意梯度与诚实梯度不可区分，又按类别更好地区分嵌入、提升重建精度。

五个数据集（Credit、RT IOT2022、MNIST、CIFAR-10、Tiny imagenet）上 URVFL/URVFL_sync 全面优于 GRNA、GIA、AGN、PCAT、SDAR、FSHA（Tiny imagenet 重建 MSE 0.0699 vs FSHA 0.1025），且完全绕过 SG/GS 检测——检测下 AGN 误差从 0.3786 恶化到 1.9577，URVFL 性能不变；DAC 分布迁移余弦距离 0.6121→0.1373。

首次把"不可检测"作为 VFL 恶意攻击的一等目标，利用被忽略的标签信号，设计巧妙；但需同分布辅助数据集（约占训练集 10%）、编码器需与目标结构一致，威胁模型把通常可信的标签持有方设为恶意方，未来更细粒度的梯度异常检测可能重新扳回局面。

- **论文 PDF**: [URVFL: Undetectable Data Reconstruction Attack on Vertical Federated Learning](https://www.ndss-symposium.org/wp-content/uploads/2025-46-paper.pdf)

#### Understanding Data Importance in Machine Learning Attacks: Does Valuable Data Pose Greater Harm?

- **作者**: Rui Wen, Michael Backes, Yang Zhang
- **方向**: AI/ML安全
- **解读**: 机器学习领域刻画了样本对模型效用的异质性贡献（数据价值），但"高价值数据是否也更易受攻击"悬而未决；若高重要度样本更易被成员推断攻破，将造成歧视、保费上涨等现实后果。

用 KNN-Shapley 计算样本重要度，在 CIFAR10/CelebA/TinyImageNet 上对成员推断、模型窃取、后门、属性推断、数据重建五类攻击做高低重要度分组对比，并利用重要度校准成员指标（CaliMem）、主动复制样本提升重要度来增强攻击。

成员推断中高重要度样本在 1% 假阳性率处的 TPR 是低重要度的 10.2 倍（TinyImageNet 27.9 倍）；移除最高重要度样本后 99.14% 剩余样本重要度上升（隐私洋葱效应）；同分布模型窃取提升 1.6–2.1 倍（排除类别失衡）；小毒化率下后门 ASR 高 1.44 倍；属性推断与重建无显著差异。

首次系统打通"数据价值"与"隐私风险"，发现可直接用于攻击增强（重要度校准、主动提权复现成员毒化攻击）；但属相关性研究而非因果证明，KNN-Shapley 本身是近似，校准超参数靠经验调优，结论限于三个图像基准。

- **论文 PDF**: [Understanding Data Importance in Machine Learning Attacks: Does Valuable Data Pose Greater Harm?](https://www.ndss-symposium.org/wp-content/uploads/2025-331-paper.pdf)

#### Tweezers: A Framework for Security Event Detection via Event Attribution-centric Tweet Embedding
- **作者**: Jian Cui (Indiana University), Hanna Kim (KAIST), Eugene Jang (S2W Inc.), Dayeon Yim (S2W Inc.), Kicheol Kim (S2W Inc.), Yongjae Lee (S2W Inc.), Jin-Woo Chung (S2W Inc.), Seungwon Shin (KAIST), Xiaojing Liao (Indiana University)
- **方向**: AI/ML安全
- **解读**: 问题：Twitter 是实时网络威胁情报（CTI）的重要来源，安全事件检测能帮助安全人员及时发现漏洞披露、钓鱼活动等。但推文海量且含噪声，基于关键词的方法（W2E、SONAR）不理解语义，例如把 "Tropic" 误关联到与安全无关的 "Tropic Thunder" 讨论；同时已有工作只重早期检测而忽视事件覆盖，先前方法的事件覆盖率仅 2.7% 和 29.8%。

方法：提出事件归因中心（event attribution-centric）的推文嵌入方法：先用基于 LLM 的 NER 提取推文中的安全事件相关实体，假设同一事件的推文共享相同实体，据此构建推文关系图，再用图注意力网络（GAT）学习嵌入，并将推文内容、安全类别、时间信息向量化作为节点初始特征，以专门为事件检测设计的损失函数优化。基于该嵌入构建 Tweezers 框架，用聚类检测安全事件，并延伸出事件趋势分析与信息性安全用户识别两个应用。

结果：真实数据上 Tweezers 整体精确率 0.6415（272/424）、召回 0.5563（84/151），检出事件数约为基线两倍，而 SONAR 召回仅 0.2980、W2E 仅 0.0265；NER 模块 F1 为 50.19（prompt-based），优于 RoBERTa 的 39.7；多标签推文分类器平均 F1 达 0.8322。

我的思考：贡献在于把"同一事件共享实体"这一领域先验引入嵌入学习，直接针对语义相似却属不同事件的难题，思路干净且开源可复现。局限：依赖聚类阈值与事件粒度定义，以新闻披露为真值可能低估 Twitter 首发事件召回，且对 NER/LLM 质量敏感。相比关键词方法是本质提升，但与通用事件检测的对比值得补充。
- **论文 PDF**: [Tweezers: A Framework for Security Event Detection via Event Attribution-centric Tweet Embedding](https://www.ndss-symposium.org/wp-content/uploads/2025-139-paper.pdf)

#### Try to Poison My Deep Learning Data? Nowhere to Hide Your Trajectory Spectrum!
- **作者**: Yansong Gao (The University of Western Australia), Huaibing Peng (Nanjing University of Science and Technology), Hua Ma (CSIRO's Data61), Zhi Zhang (The University of Western Australia), Shuo Wang (Shanghai Jiao Tong University), Rayne Holland (CSIRO's Data61), Anmin Fu (Nanjing University of Science and Technology), Minhui Xue (CSIRO's Data61), Derek Abbott (The University of Adelaide, Australia)
- **方向**: AI/ML安全
- **解读**: 问题：DaaS 模式下数据策展商聚合众包数据卖给模型提供商，恶意贡献者可投毒并在训练模型中植入后门。已有训练阶段检测方法依赖预留干净数据、对投毒率/触发类型/后门类型敏感，且仅适用于分类任务，难以被策展商实际采用。

方法：首次从频谱域研究投毒样本的训练损失轨迹——良性样本学习主任务、投毒样本学习后门任务，二者损失轨迹在时域高度重叠，但变换到频谱域后可分性显著增强；自动截取模型收敛后的轨迹段进一步放大差异。最后用 DBSCAN 聚类自适应区分良性与投毒样本，无需任何干净参照集，实现一次性数据清洗。

结果：全部评测中检测准确率至少 95.52%、误报率不高于 0.61%；对 universal 与 partial 后门、各类触发（含 Narcissus 干净标签攻击，CCS'2023）均有效，跨图像/音频/文本模态与回归任务。对比 SOTA：ASSET 无法处理 partial 后门且在干净数据集上误报不可接受，CT 对 Narcissus 触发失效，而 Telltale 在二者失败的场景下均有效。

我的思考：贡献成色高——频谱视角新颖，是首个满足模态无关、任务无关、无需干净数据等全部需求、且适用于非分类任务的投毒检测方法。局限：需训练代理模型观察损失轨迹，计算成本不低；聚类与截取点存在超参敏感性；频谱特征可解释性论述有限。相比表示空间方法，"损失轨迹"更接近后门任务与主任务分离的机理本质，但落地前提是策展方掌控训练过程。
- **论文 PDF**: [Try to Poison My Deep Learning Data? Nowhere to Hide Your Trajectory Spectrum!](https://www.ndss-symposium.org/wp-content/uploads/2025-1589-paper.pdf)

#### TrajDeleter: Enabling Trajectory Forgetting in Offline Reinforcement Learning Agents
- **作者**: Chen Gong (University of Virginia), Kecen Li (Chinese Academy of Sciences), Jin Yao (University of Virginia), Tianhao Wang (University of Virginia)
- **方向**: AI/ML安全
- **解读**: 问题：离线强化学习用预收集数据集训练智能体，广泛应用于医疗、能源管理等无法在线交互的场景。GDPR/CCPA 赋予用户删除数据的权利，数据提供方也可能因数据中毒或版权需要删除部分轨迹。但 RL 数据是 (状态, 动作, 奖励) 轨迹序列，已有近似遗忘方法（梯度上升、对比学习）面向监督学习，无法直接迁移到离线 RL。

方法：提出首个离线 RL 轨迹级遗忘方法 TrajDeleter，分两阶段："遗忘阶段"同时最小化待遗忘样本的 Q 值并最大化剩余样本的 Q 值（防止正常性能退化）；"收敛训练阶段"最小化原始与遗忘后智能体在剩余轨迹状态上累积回报的差异，理论分析保证收敛。配套 TrajAuditor 审计器：微调原智能体生成 shadow agents 并沿轨迹施加状态扰动，通过比较累积回报判定遗忘是否成功。

结果：在 6 个离线 RL 算法 × 3 个 Mujoco 任务上，遗忘成功率 92.7%、99.5%、90.5%（平均 94.8%），仅需从头重训 1.5% 的时间；遗忘后实际环境交互性能与重训智能体相差仅 0.9%-2.2%；TrajAuditor 的 F1 达 0.88/0.87/0.88，比 ORL-Auditor（从头训练 shadow agents）节省 97.8% 时间且 F1 高 0.08。

我的思考：填补了机器遗忘在强化学习领域的空白，Q 值双向优化加收敛微调的设计简洁可落地，廉价审计器是关键工程贡献。局限：近似遗忘与法律"完全删除"要求的相容性存疑；评估依赖启发式审计器本身；缺乏真实高风险场景验证。相比监督学习遗忘，"按状态区分遗忘/保留"抓住了 RL 的时序特性，为该领域后续研究开辟了方向。
- **论文 PDF**: [TrajDeleter: Enabling Trajectory Forgetting in Offline Reinforcement Learning Agents](https://www.ndss-symposium.org/wp-content/uploads/2025-126-paper.pdf)

#### Towards Understanding Unsafe Video Generation
- **作者**: Yan Pang (University of Virginia), Aiping Xiong (Penn State University), Yang Zhang (CISPA Helmholtz Center for Information Security), Tianhao Wang (University of Virginia)
- **方向**: AI/ML安全
- **解读**: 问题：视频生成模型（VGM）已能合成高质量视频，但可能生成暴力、恐怖、色情等不安全内容。与图像生成相比，视频含时空双重信息且生成算力需求高，直接迁移图像领域的防御并不现实，而此前缺乏对 VGM 不安全生成的系统性理解与专门防御。

方法：先使用从 4chan 与 Lexica 收集的不安全提示词驱动 3 个开源 SOTA VGM，生成 5607 个视频，过滤后得 2112 个；经聚类与主题编码归纳出 5 类不安全内容（扭曲/怪异、恐怖、色情、暴力/血腥、政治）；经 IRB 批准招募 403 名 Prolific 参与者标注，最终确认 937 个不安全视频，构建首个 VGM 不安全视频数据集。防御上提出模型读取式 Latent Variable Defense（LVD）：利用 DDIM 采样的确定性特征，在推理早期用分类器检测中间去噪潜变量，可疑即提前终止，不修改任何模型参数。

结果：LVD 综合检测精度 0.90，三个模型上分别达 0.99、0.92、0.91；相比检测最终输出，可节省至多 10 倍的计算与时间；在对抗提示与图像到视频场景下均高于 0.90，且与其他防御组合时能提升其性能。

我的思考：贡献是"理解+数据集+防御"的完整闭环，首个不安全视频数据集为后续研究提供了基准，LVD 的"早期终止"兼顾精度与算力，是模型读取范式的示范。局限：提示词来源与模型数量有限，数据集代表性受限；标注一致性与泛化讨论不足。与图像领域的安全引导、概念移除相比，LVD 不改参数、可插拔，更适合在线服务场景。
- **论文 PDF**: [Towards Understanding Unsafe Video Generation](https://www.ndss-symposium.org/wp-content/uploads/2025-1167-paper.pdf)

#### THEMIS: Regulating Textual Inversion for Personalized Concept Censorship
- **作者**: Yutong Wu, Jie Zhang, Florian Kerschbaum, Tianwei Zhang
- **方向**: AI/ML安全
- **解读**: 个性化生成技术 Textual Inversion（TI）把物体信息压缩进一个伪词嵌入，用户可在 Civitai 等平台（约 300 万注册用户）分享购买伪词，但恶意用户可将其与敏感词组合生成诽谤、虚假或色情内容（如"大楼着火"）。现有防御要么净化模型（攻击者可下载未净化模型而失效），要么让受保护作品"不可学习"（损害创作者利益）。本文提出"概念审查"：保留正常使用，仅在触发敏感词时阻止恶意生成。

方法上，THEMIS 将"为善的后门"注入 TI 嵌入本身而非基础模型：训练时以敏感词为触发器，在原始损失上叠加后门项，使"敏感词+伪词"的提示生成预定义目标图像；同时保留原损失项以维持保真度与可编辑性。针对同义词提出分组-审查策略（每组分配不同目标图），并用随机抽样/近似优化解决多触发器训练开销（N=10 个词时训练时间约为常规的 5.5 倍）。

在 LDM 与 Stable Diffusion V2.1 上，针对欺骗、色情、违法、惊悚四类场景，人工评定的保护成功率（PSR）接近 100%（普通 TI 仅 2%–47%），CLIP 分数显示效用几乎无损；对移除攻击（PSR≥91%）、拼写错误（89%–95%）、扰动攻击（最低约 93%）乃至 SneakyPrompt 等自适应攻击均具鲁棒性，且能同时审查多个敏感词及其同义词。

这是首次把后门"以正御邪"用于伪词嵌入层面，威胁模型贴近 Civitai 的现实场景，与概念擦除（ESD）、查询审计等方案形成互补。局限在于伪词容量有限（约 1280 个浮点数），黑名单过长会导致目标图像在特征空间"融合"；且方案依赖发布者主动自律，恶意发布者不会采纳。总体为个性化生成模型的监管提供了新思路，但落地需要平台层面的激励机制配合。
- **论文 PDF**: [THEMIS: Regulating Textual Inversion for Personalized Concept Censorship](https://www.ndss-symposium.org/wp-content/uploads/2025-450-paper.pdf)

#### SongBsAb: A Dual Prevention Approach against Singing Voice Conversion based Illegal Song Covers
- **作者**: Guangke Chen (Pengcheng Laboratory), Yedi Zhang (National University of Singapore), Fu Song (Key Laboratory of System Software (Chinese Academy of Sciences) and State Key Laboratory of Computer Science, Institute of Software, Chinese Academy of Science; Nanjing Institute of Software Technology), Ting Wang (Stony Brook University), Xiaoning Du (Monash University), Yang Liu (Nanyang Technological University)
- **方向**: AI/ML安全
- **解读**: 
歌声转换（SVC）让普通人即可自动生成侵权翻唱（如"AI 孙燕姿"翻唱逾千首歌曲），侵犯歌词版权与歌手声音人格权；事后被动检测成本高且低效，亟需发布前的主动防护手段。

SongBsAb 在歌声发布前添加微小扰动，实现"双重防护"：性别转换损失（从异性歌手中选身份相似度最低者作"目的地歌手"，用质心嵌入）破坏身份特征以防歌声被用作目标歌声；高/低层级多目标损失（拉近到不同歌词歌声的高层语义特征与低层声学特征）破坏歌词以防被用作源歌声；用伴奏音轨作为额外掩蔽源的同步掩蔽损失保证扰动不可闻；FL-IR 损失与编码器集成互补地增强对未知 SVC 模型的迁移性。

在 5 个主流 SVC 模型（Lora/Vits/Grad/NeuCo-SVC 等）上，身份相似度降至 0.05–0.22，歌词 WER 提升 53–75%，SVC 成功率降低超 97%（SRR-T≥98%）；SNR>25dB、PESQ>3.7，无害性显著优于 AttackVC/AntiFake；在 8 个身份编码器与 5 个歌词编码器上迁移性突出（SRR-I 最高 80%、SRR-L 最高 100%）；人工主观评测确认有效。

首次把对抗扰动用于保护歌唱领域，妥善处理歌曲独有的双重权利与伴奏掩蔽，FL-IR 与编码器集成的互补性论证扎实；局限是主要面向 few-shot SVC、防御者需预知编码器（后放宽），对微调型自适应攻击的鲁棒性有限。整体为"对抗样本用于正途"树立了新的示范。

- **论文 PDF**: [SongBsAb: A Dual Prevention Approach against Singing Voice Conversion based Illegal Song Covers](https://www.ndss-symposium.org/wp-content/uploads/2025-747-paper.pdf)

#### Scale-MIA: A Scalable Model Inversion Attack against Secure Federated Learning via Latent Space Reconstruction

- **作者**: Shanghao Shi, Ning Wang, Yang Xiao, Chaoyu Zhang, Yi Shi, Y. Thomas Hou, Wenjing Lou

- **方向**: AI/ML安全

- **解读**: 联邦学习号称保护参与者数据隐私，但恶意参数服务器可通过模型反演攻击（MIA）从模型更新重建用户本地数据。现有攻击要么依赖计算密集的逐批迭代优化（难扩展），要么要求修改预定义模型架构添加前置模块，极易被完整性检查识破。

本文提出Scale-MIA，识别隐空间为攻破隐私的关键层：离线阶段用辅助数据集训练与全局模型编码器同架构的代理自编码器，并基于隐空间表示（LSR）分布统计构造线性泄漏模块，组装出与原始架构完全一致的对抗模型；在线阶段单轮内先用闭式线性泄漏从安全聚合保护下的聚合更新重建批量LSR，再送入生成式解码器恢复整批输入，全程仅矩阵与前馈计算。

在六个数据集及CNN、AlexNet、VGG、ResNet、ViT上验证：CIFAR-10上批大小64时重建率0.9328（PSNR 28.6dB），256时0.8464，1024时仍达0.4841；重建256个样本仅0.22秒（Loki需99秒）；对FL训练精度几乎无影响；仅1%辅助数据（500张图）仍保持高重建率。

我的思考：把攻击面从"附加模块"转移到模型自带的MLP隐空间，用闭式线性泄漏替代昂贵优化，兼具可扩展、隐蔽、高效，对安全聚合构成实际威胁；瓶颈刻画清晰（首个线性层神经元数k，实践中批大小需小于k/2），重建呈二值性质。局限：依赖辅助数据分布接近训练数据、FedAVG多轮迭代降低近似精度、ViT上性能下降；针对此类攻击的防御仍是开放问题。

- **论文 PDF**: [Scale-MIA: A Scalable Model Inversion Attack against Secure Federated Learning via Latent Space Reconstruction](https://www.ndss-symposium.org/wp-content/uploads/2025-644-paper.pdf)

#### SafeSplit: A Novel Defense Against Client-Side Backdoor Attacks in Split Learning

- **作者**: Phillip Rieger, Alessandro Pegoraro, Kavita Kumari, Tigist Abera, Jonathan Knauer, Ahmad-Reza Sadeghi

- **方向**: AI/ML安全

- **解读**: 分割学习（SL）让客户端与服务端协作训练深度网络，U形结构中头尾在客户端、骨干在服务端。但顺序训练使FL的后门防御无法迁移：每个客户端以上一客户端模型为基座，恶意客户端会污染后续良性客户端的训练；服务端又看不到客户端数据与头尾，只能分析骨干更新。

本文提出首个针对SL客户端侧后门攻击的防御SafeSplit，部署在服务端：每个客户端训练结束后执行"循环后向分析"，在最近N个骨干模型上用双重度量判定当前模型——静态分析用DCT频域欧氏距离，动态分析用新提出的旋转距离度量（由角位移、角速度、旋转频率刻画更新方向轨迹）；若判恶意则回滚到最近的良性检查点并跳过该客户端（不永久移除，后续轮次可重新评估）。

在五个数据集及六种架构上评估：CIFAR-10语义后门BA从59.3%降至0.0%，像素后门从100%降至0.3%，FMNIST从79.8%降至3.4%，所有实验BA均低于5%；针对度量的自适应攻击（loss-constrain）仍全部被压制到0%；与KRUM、差分隐私、FreqFed等FL防御对比只有SafeSplit能把BA降到0%；无攻击时MA仅从69.30%降至66.6%。

我的思考：首次系统解决SL后门防御空白，频域静态与方向动态双度量互补，旋转距离对学习率等尺度差异鲁棒，循环回滚正确处理顺序训练耦合。局限：依赖客户端多数良性（≤N/2−1恶意）与骨干历史窗口，对non-IID程度敏感，对更隐蔽分布型投毒的长期有效性存疑；头尾驻留客户端限制了服务器分析面。相比FL防御，这是SL专属的第一步。

- **论文 PDF**: [SafeSplit: A Novel Defense Against Client-Side Backdoor Attacks in Split Learning](https://www.ndss-symposium.org/wp-content/uploads/2025-1698-paper.pdf)

#### Revisiting Physical-World Adversarial Attack on Traffic Sign Recognition: A Commercial Systems Perspective

- **作者**: Ningfei Wang, Shaoyuan Xie, Takami Sato, Yunpeng Luo, Kaidi Xu, Qi Alfred Chen

- **方向**: AI/ML安全

- **解读**: 物理世界对抗攻击（贴片、海报）能低成本隐藏或伪造交通标志，威胁自动驾驶安全，但以往工作几乎只在学术TSR模型上评估，商业TSR系统层面的影响未知；少数尝试仅覆盖单一车型，泛化性与代表性存疑。

本文首次大规模测量商业TSR系统上的物理对抗攻击：选取4款美国2023年销量前15品牌的车型（可推广到至少33.2%的在售TSR系统，远超此前最多3.8%），对全部具备物理黑盒转移性的既有攻击做转移测试；测试中发现商业系统普遍存在"空间记忆"设计——检测结果被持续记忆直到该标志的空间反应需求满足（如车辆驶过），区别于仅记忆数秒的目标跟踪，据此提出新度量SysHA/SysAA建模其对系统级攻击成功的影响。

结果：个别攻击对特定商业TSR功能可达100%成功率，但30个测试组合中28个无效果，总体转移成功率仅6.67%（原论文平均报告51.6%）；引入SysHA后白盒攻击成功率下降2–9倍（RP2与FTE低至≤6.6%），黑盒转移下RP2从42.6%降至14.5%；隐藏攻击理论上比出现攻击更难（成功率差≥93.8%），旧度量可能产生约50%的绝对误差；共得出7个新观察。

我的思考：核心贡献是揭示"模型级攻击成功≠系统级攻击成功"：空间记忆使隐藏攻击必须在所有触发记忆的检测时刻连续成功，解释了学术界与商业实测的巨大落差，SysHA/SysAA度量本身是方法论贡献。局限：仅4款匿名车型、30个组合范围有限，转移率低也可能源于架构差异等混淆因素；对商业系统内部实现仍是黑盒推断。警示物理对抗研究可能系统性高估真实威胁。

- **论文 PDF**: [Revisiting Physical-World Adversarial Attack on Traffic Sign Recognition: A Commercial Systems Perspective](https://www.ndss-symposium.org/wp-content/uploads/2025-s90-paper.pdf)

#### Reinforcement Unlearning
- **作者**: Dayong Ye, Tianqing Zhu, Congcong Zhu, Derui Wang, Kun Gao, Zewei Shi, Sheng Shen, Wanlei Zhou, Minhui Xue
- **方向**: AI/ML安全
- **解读**:
机器遗忘研究长期聚焦监督学习，强化学习（RL）却被忽视：智能体训练中会记忆环境特征，泄露环境所有者隐私（如导航中的受限区域位置、推荐系统中的用户偏好）。按数据保护法规，环境所有者有权撤销环境级训练数据，由此催生"强化遗忘"这一新方向——遗忘对象是完整环境而非单个样本。

作者提出两种环境级遗忘方法：基于递减强化学习（decremental RL），在待遗忘环境中最小化智能体回报以逐步擦除知识；基于环境投毒（environment poisoning），修改待遗忘环境的转移函数，让智能体学习错误知识以移除该环境，同时保持其他环境性能。针对评估难题，提出"环境推断"（基于遗传算法推断训练环境）作为遗忘效果度量。

在 grid world、virtual home、maze explorer 三类任务上以 DQN 实验（100 轮、95% 置信区间 3%）：grid world 中遗忘后智能体步数由 19.34 增至 55.8，回报由 -44 降至 -273.5，保留环境性能几乎不变；投毒法在保留环境上比递减法更稳定，且两者均优于 LFS 与 Non-transfer LFS 基线。

该工作首次系统定义并解决 RL 环境级遗忘，环境推断评估法弥补了成员推断不适用于 RL 的空白。局限：遗忘效果以"性能退化"而非"与从头重训一致"定义，语义较宽松；方法需访问待遗忘环境或带标签经验样本；实验限于仿真任务，可扩展性与真实场景（推荐系统、自动驾驶）下的效果待验证。

- **论文 PDF**: [Reinforcement Unlearning](https://www.ndss-symposium.org/wp-content/uploads/2025-80-paper.pdf)

#### Provably Unlearnable Data Examples
- **作者**: Derui Wang, Minhui Xue, Bo Li, Seyit Camtepe, Liming Zhu (CSIRO's Data61; University of Chicago)
- **方向**: AI/ML安全
- **解读**: 公开数据被未授权模型滥用（风格模仿、成员推理等）催生了"不可学习样本"（UE）防御，但现有方法靠经验优化扰动，缺乏对未授权模型与训练过程不确定性的鲁棒性保证；经验测试准确率作为评估指标存在泛化误差，且作者发现一种简单恢复攻击：用CIFAR10约5%-10%的干净数据微调，即可把基于UE训练的ResNet-18清洁准确率从约0.1恢复到近0.8，使UE可靠性存疑。

论文提出面向参数空间的认证机制：(q,η)-Learnability——通过参数平滑，对从特定参数空间采样的任意分类器给出其清洁测试准确率的保证上界，无需知道攻击者的训练策略；进一步改进认证紧致性以缩小与真实可学习性（True Learnability）的差距，并提出无需私有测试集的泛化认证变体；据此设计PUEs以直接压制认证界限。

实验显示，相比现有UE（如error-minimizing PAP），PUE在ImageNet上最多降低18.9%的认证(q,η)-Learnability，在CIFAR-100上最多降低54.4%的经验测试准确率；在无法认证的未授权分类器场景下也表现出更强的经验鲁棒性。代码已开源。

这是首批为数据可用性控制（DAC）提供可证明保证的工作，把"抗学习"从启发式推进到可认证，理论贡献扎实，且与差分隐私、水印正交可并行使用；但认证面向参数空间而非攻击者实际可及的空间，保证的迁移性有限，PUE构造与认证紧致性的权衡缺少充分消融。总体为数据主权与负责任AI提供了新工具，扩展至图像以外模态值得期待。
- **论文 PDF**: [Provably Unlearnable Data Examples](https://www.ndss-symposium.org/wp-content/uploads/2025-886-paper.pdf)

#### Probe-Me-Not: Protecting Pre-trained Encoders from Malicious Probing

- **作者**: Ruyi Ding, Tong Zhou, Lili Su, Aidong Adam Ding, Xiaolin Xu, Yunsi Fei (Northeastern University)
- **方向**: AI/ML安全
- **解读**: 迁移学习中"探针"（probing）——冻结预训练编码器、只训练下游分类头——已成为利用预训练模型的主流方式。但编码器API的通用可探性带来恶意使用风险：攻击者可构建用于歧视性推测、战争武器等危害任务的分类头，仅靠使用政策无法形成技术壁垒。本文提出EncoderLock，在禁止域上让探针失效，同时保持授权域功能。
方法上，EncoderLock采用两个关键技术：域感知的权重选择与更新以限制禁止域上的应用，以及自挑战训练方案迭代增强对任意下游分类头的鲁棒性。针对禁止域数据缺失的现实，作者设计了监督、无监督、零样本三个变体（零样本仅凭域描述、借助GPT-4生成数据），覆盖不同数据可得性场景。
实验覆盖15个域、VGG-11/ResNet-18/ViT三种架构。监督变体在目标域实现最高78.70%（VGG-11）、86.02%（ResNet-18）的相对精度下降，源域损失不超3.53%，平均仅改动不到0.08%权重；零样本变体也在真实Facebook预训练ViT上验证了实用性。
我认为这是对不可迁移学习的有力推广：把"禁止直接推理"升级为"禁止通过下游头复用"，更贴合编码器即服务的现实。局限是禁止域需预先指定、域间相似时效果减弱（如SD→SN仅降26.74%）、无监督/零样本性能弱于监督版；面对自适应攻击者时的鲁棒性仍待验证。

- **论文 PDF**: [Probe-Me-Not: Protecting Pre-trained Encoders from Malicious Probing](https://www.ndss-symposium.org/wp-content/uploads/2025-928-paper.pdf)

#### PBP: Post-training Backdoor Purification for Malware Classifiers
- **作者**: Dung Thuy Nguyen, Ngoc N. Tran, Taylor T. Johnson, Kevin Leach (Vanderbilt University)
- **方向**: AI/ML安全
- **解读**:
ML恶意软件分类器面临后门投毒威胁，攻击者在训练数据中植入触发器即可让模型把恶意样本判为良性。现有防御多为训练期检测毒样本（如集成模型投票），不适用于MLaaS或已训练模型的"事后净化"场景。

PBP观察到后门会使各层激活分布畸变为混合分布，据此两阶段净化：先训练噪声模型对齐后门模型的批归一化统计量，借Hessian迹定位对后门最关键的神经元并生成掩码；再用掩码梯度微调——对被掩码神经元反转梯度符号实现"反学习"，正常神经元照常更新，并交替优化以维持良性精度。

在EMBER（110万PE样本）与AndroZoo（约15万Android样本）上针对Explanation-guided与Jigsaw Puzzle两种SOTA后门，PBP把攻击成功率从接近100%降至约0%（家族定向）与15.44%（非家族定向，EMBER），仅需1%微调数据（983个样本），较基线有百倍提升；五种微调基线全部失效。良性精度约从98.5%降至96.5%，继续训练可部分恢复。

这是首个面向恶意软件分类器的训练后后门净化方案，不假设触发器形态，激活分布偏移的洞察具普遍性并附收敛证明。局限：假设微调数据干净、主要评估MLP架构，净化后误报率略升，对污染微调数据的鲁棒性留待未来。向CV等领域推广的潜力值得关注。

- **论文 PDF**: [PBP: Post-training Backdoor Purification for Malware Classifiers](https://www.ndss-symposium.org/wp-content/uploads/2025-603-paper.pdf)

#### LADDER: Multi-Objective Backdoor Attack via Evolutionary Algorithm
- **作者**: Dazhuang Liu, Yanqi Qiao, Rui Wang, Kaitai Liang, Georgios Smaragdakis
- **方向**: AI/ML安全
- **解读**: 现有黑盒后门攻击把目标建模为单目标优化、在单一域设计触发器，导致语义破坏、鲁棒性差，并引入视觉与频谱异常。理想触发器需同时满足隐蔽性、鲁棒性、有效性与功能保持，目标彼此冲突，拉格朗日系数聚合需反复调参，结合 SGD 的优化往往得不到实用触发器。目标是在无受害者模型内部信息的黑盒设定下，同时实现有效性、双域隐蔽性与预处理鲁棒性。

LADDER 首次把后门触发器设计形式化为多目标优化问题，用多目标进化算法（MOEA）求解：随机初始化触发器种群，个体代表目标权衡，经交叉与变异产生候选，用非支配排序（NDSort）逼近 Pareto 前沿，再以偏好选择剔除不实用触发器。触发器经 DCT 注入低频频谱区域——低频承载语义、抗压缩滤波，兼顾隐蔽性与鲁棒性；用异构替代模型评估，避免对受害者模型的梯度依赖。

5 个公开数据集上攻击有效性达 99% 以上，鲁棒性 90.23%（比现有 SOTA 攻击平均高 50.09%）；自然隐蔽性较现有隐蔽攻击提升 1.12 至 196.74 倍，频谱隐蔽性（平均 l2 范数）提升 8.45 倍。

贡献在于把触发器设计从"系数调参的单目标优化"推进到"免系数的多目标 Pareto 优化"，空间+频谱双域隐蔽视角与低频注入的鲁棒性论证扎实，黑盒设定贴近现实。局限：进化算法计算成本高，大规模数据集上的可扩展性未充分讨论；替代模型与受害者模型差异大时迁移性存疑；90.23% 的鲁棒性仍非完全可靠。与 FTrojan、BadNets 等相比，LADDER 优势在于隐蔽性与鲁棒性的联合优化，代表了后门攻击向工程化触发器演进的趋势。
- **论文 PDF**: [LADDER: Multi-Objective Backdoor Attack via Evolutionary Algorithm](https://www.ndss-symposium.org/wp-content/uploads/2025-1061-paper.pdf)

#### L-HAWK: A Controllable Physical Adversarial Patch Against a Long-Distance Target
- **作者**: Taifeng Liu, Yang Liu, Zhuo Ma, Tong Yang, Xinjing Liu, Teng Li, Jianfeng Ma
- **方向**: AI/ML安全
- **解读**: 自动驾驶车辆的视觉感知模块易受物理对抗补丁攻击，但现有攻击"无差别"地影响所有经过车辆，攻击者无法控制触发时机与目标，易被意外受害者及反制措施发现。此前唯一可控方案 TPatch（Usenix '23）用超声波引起图像畸变触发，但声波传输距离短，攻击者需携带 30cm×30cm 换能器在 7 米内靠近行驶车辆，成功率仅约 59%，部署风险极高。

L-HAWK 是激光触发的可控物理对抗补丁：正常条件下无害，当激光注入目标车辆摄像头、产生特定图像畸变时才激活。论文提出异步学习方法，联合优化激光参数（波长、功率、脉宽）与对抗补丁；针对镜头散射引入多角度多位置仿真、基于连续帧差异的噪声近似，以及渐进采样方法提升真实世界鲁棒性；攻击面向目标检测与图像分类两类任务。

数字与物理环境大量实验表明：L-HAWK 在 50 米距离达到 91.9% 的平均攻击成功率，相比 TPatch 在 7 米处 59% 的成功率提升 56%，攻击距离提升七倍以上；渐进采样方法把平均成功率从 41.8% 提升至 94.4%。

贡献在于把"可控对抗补丁"从近距超声波推进到 50 米激光触发，攻击距离与成功率显著提升，异步优化框架与噪声建模有技术深度，代码与物理演示开源。局限：激光仍须精确对准移动车辆摄像头，部署要求高；攻击针对卷帘快门相机（如特斯拉 AR0132AT），但未在真实车辆上验证；激光装置本身仍可被察觉。与 Rolling Shutter、Rolling Colors 等激光畸变工作相比，L-HAWK 首次实现"选目标、按需触发"的受控攻击，为自动驾驶感知安全提出新的威胁模型。
- **论文 PDF**: [L-HAWK: A Controllable Physical Adversarial Patch Against a Long-Distance Target](https://www.ndss-symposium.org/wp-content/uploads/2025-26-paper.pdf)

#### Interventional Root Cause Analysis of Failures in Multi-Sensor Fusion Perception Systems
- **作者**: Shuguang Wang, Qian Zhou, Kui Wu, Jinghuai Deng, Dapeng Wu, Wei-Bin Lee, Jianping Wang
- **方向**: AI/ML安全
- **解读**: 自动驾驶系统（ADS）依赖多传感器融合（MSF）感知系统，而单传感器的小误差可能级联放大为整体感知失败。传统异常检测与运行时监控不考虑多模块故障与系统失败之间的因果关系，无法定位根因；云环境 RCA 依赖预设性能指标、AID 假设单一根因，均不适用。IRCA 的目标是自动定位导致失败的因果模块及其具体故障模式。

IRCA 利用 MSF 的有向无环图结构构建分层结构因果模型（H-SCM），把多维动态的模块输出抽象为有效因果变量；提出分治剪枝算法（NPI）沿因果路径覆盖多个因果模块并确定干预目标，从而支持多个并发根因的诊断。评估在 Autoware（Carla 仿真）上构建真实故障（9 组 GitHub 已确认问题）与合成故障（单/多故障注入）数据集，并以 DVCA、AID、RCD 为基线。

在真实故障场景的 Best Match 策略下，IRCA 平均精度 95.77%、召回 95.07%、F1 95.39%，显著高于 AID（F1 74.99%）、DVCA（67.16%）与 RCD（47.23%）；合成单故障 F1 96.73%，多故障场景 F1 88.59%，比第二名的 AID 高 12.97 个百分点，而基线在多数并发故障组召回骤降。效率上真实故障平均 2.99 次干预、4.67 分钟。另在搭载 Autoware 的实车测试台与 Apollo 跨平台验证，模拟发现的问题获 GitHub 开发者确认（如 #6936、#6940）。

该工作是首个把干预式因果推断系统化用于 MSF 感知系统根因分析的工作，支持多根因并发是区别于既有方法的独特贡献，且构建了公开可用的故障数据集。局限：依赖领域知识手工构建 DAG，对未知架构或非模块化故障可能失效；在 Union/Average 评估策略下 F1 明显回落（78%/65%），多根因全面覆盖仍有差距。
- **论文 PDF**: [Interventional Root Cause Analysis of Failures in Multi-Sensor Fusion Perception Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-36-paper.pdf)

#### I know what you MEME! Understanding and Detecting Harmful Memes with Multimodal Large Language Models
- **作者**: Yong Zhuang, Keyan Guo, Juan Wang, Yiheng Jing, Xiaoyang Xu, Wenzhe Yi, Mengda Yang, Bo Zhao, Hongxin Hu
- **方向**: AI/ML安全
- **解读**: 表情包（meme）是社交媒体上双刃剑式的内容形态，可借幽默与病毒式传播的外衣散布仇恨、污名化等有害内容。现有检测方法受困于 meme 的复杂表达——构图多样、拼接图像、宣传技巧与多变的文化语境，准确识别困难，且此前缺乏对"为何难检测"的系统性理解。

作者先从两个新视角系统研究有害 meme：视觉艺术（构图，如面板数量、图像拼接）与宣传技巧（如贴标签、污名化话术）。研究发现拼接图像与宣传技巧会显著削弱现有工具的有效性。据此提出 HM-GUARD 框架：利用多模态大模型（GPT-4V）的能力，以自适应提示（meme 域对齐 + 任务特化）引导模型理解语境，再通过思维链推理（HMCoT）逐步回答构图、图文关系、宣传技巧、意图等子问题后综合判定。

在 HarMeme 数据集上 HM-GUARD 准确率 0.92、召回 0.98、F1 0.91，比最强基线高 15%-79.17%（对 GPT-4 泛化提示提升 15.28%-96%）；FHM 上准确率 0.86、F1 0.85。针对最难的"拼接图像"与"含宣传技巧"样本，检测率分别提升 54.46% 与 43.93%；在含 300 个 Pinterest 采集 meme 的真实场景准确率达 0.88。消融实验证实自适应提示带来 54.24% 的 F1 提升。

该工作的成色在于"先理解后检测"：用构图与宣传技巧两个维度解释了既有方法失效的机理，再以推理式 MLLM 提示解决，方法论可迁移到其他多模态内容审核任务。局限：依赖 GPT-4V 商业模型，成本高、结果可复现性受限；对文化语境的细微理解仍可能出错；对抗性"无害化伪装"的演化意味着这是一场持续的军备竞赛。
- **论文 PDF**: [I know what you MEME! Understanding and Detecting Harmful Memes with Multimodal Large Language Models](https://www.ndss-symposium.org/wp-content/uploads/2025-415-paper.pdf)

#### GAP-Diff: Protecting JPEG-Compressed Images from Diffusion-based Facial Customization

- **作者**: Haotian Zhu, Shuchao Pang, Zhigang Lu, Yongbin Zhou (Nanjing University of Science and Technology), Minhui Xue (CSIRO's Data61)
- **方向**: AI/ML安全
- **解读**: 基于DreamBooth等微调技术的文生图扩散模型，可凭少量身份照片生成大量逼真定制图片，被滥用于伪造个人形象、传播虚假信息。现有防护方法（PhotoGuard、Glaze、Mist等）通过注入高频对抗噪声破坏微调，但社交网络上传时普遍执行的JPEG压缩会抹掉高频噪声，使既有防护在真实场景中失效。本文提出GAP-Diff，让防护噪声在JPEG压缩后依然有效。
GAP-Diff是一个无监督学习优化的生成式框架：用生成器网络一步完成"干净图→保护图"的对抗映射，通过预处理模拟模块回传梯度学习对JPEG压缩鲁棒的表示；同时设计针对微调方法和压缩的对抗损失，使保护噪声偏向低频区域以抵抗压缩。
在FFHQ、CelebA-HQ、VGGFace2上以SD-v2.1+DreamBooth（及LoRA、SVDiff）评估，经Q=70压缩后，GAP-Diff的FDFR比最优基线高约30%（如78.67%对42.56%），ISM降至0.28以下，BRISQUE超40；人类投票中GAP-Diff以90.88%的压倒性比例获胜。噪声预算16/255，训练约120 GPU小时，但推理时毫秒级生成。
我认为核心贡献是识别出"压缩使高频噪声失效"这一真实部署盲区，并用可微压缩模拟从原理上解决，而非简单堆叠扰动。局限是训练成本高、依赖生成器对身份数据的泛化，且论文主要在DreamBooth系微调上验证，对更新微调范式（如个性化LoRA变体）的泛化有待观察。与SimAC等改进基线相比，低频噪声策略被证明更抗压缩。

- **论文 PDF**: [GAP-Diff: Protecting JPEG-Compressed Images from Diffusion-based Facial Customization](https://www.ndss-symposium.org/wp-content/uploads/2025-2088-paper.pdf)

#### Explanation as a Watermark: Towards Harmless and Multi-bit Model Ownership Verification via Watermarking Feature Attribution

- **作者**: Shuo Shao, Yiming Li, Hongwei Yao, Yiling He, Zhan Qin, Kui Ren (Zhejiang University)
- **方向**: AI/ML安全
- **解读**: 模型所有权验证是保护深度学习模型版权最主流的后验手段，而基于后门的水印是目前主流方案。但后门水印有两大致命缺陷：有害性（在被水印模型中引入可控误分类后门）和歧义性（恶意方找到其他误分类样本即可通过验证）。作者指出这两个缺陷都源于现有方案的"零比特"本质——只利用预测状态（是否误分类）做验证。本文提出新范式Explanation as a Watermark（EaaW），把验证行为植入特征归因解释而非预测。
EaaW基于可解释AI（XAI）思想：在不动原始预测的前提下，将"多比特"水印嵌入特定触发样本的特征归因解释中，并相应设计水印嵌入与提取算法。由于验证依赖解释而非预测，水印模型保持无害；多比特结构天然抗歧义，还能用于图像分类和文本生成两类任务。
在CIFAR-10（ResNet-18，基线精度90.54%）和ImageNet子集（76.38%）上，64/256/1024比特水印的提取WSR均接近1.000，p值低至10^-222；测试精度在多数情形下降不足1%。独立模型和独立触发样本均无法通过验证（最小p值0.012>0.01，WSR约50%），证明独特性和安全性；GPT-2文本生成任务同样有效。
我认为这是水印领域"从预测空间转向解释空间"的重要范式转换，首次使水印同时满足无害性与多比特容量。局限在于依赖触发样本的XAI可计算性，黑盒API下提取解释的成本与稳定性值得关注；解释的可迁移性（模型窃取后解释是否继承）也需更多攻击场景验证。与后门水印相比，它规避了"植入漏洞"的伦理困境。

- **论文 PDF**: [Explanation as a Watermark: Towards Harmless and Multi-bit Model Ownership Verification via Watermarking Feature Attribution](https://www.ndss-symposium.org/wp-content/uploads/2025-338-paper.pdf)

#### DShield: Defending against Backdoor Attacks on Graph Neural Networks via Discrepancy Learning

- **作者**: Hao Yu, Xinhang Wan, Jun Wang, Xinwang Liu (National University of Defense Technology), Chuan Ma, Tao Xiang (Chongqing University), Meng Shen (Beijing Institute of Technology)
- **方向**: AI/ML安全
- **解读**: 图神经网络（GNN）易受后门攻击：攻击者在图中注入触发器节点或子图，使带触发器的节点被判定为攻击者指定标签。节点分类攻击按脏标签/干净标签划分，因正常节点与投毒节点相互连接而格外棘手；现有防御大多借用了图像域假设（如投毒数据快速损失下降），可被精巧触发器绕过，且无法同时防御两类攻击。本文提出DShield，用差异学习机制统一防御脏标签与干净标签攻击。
作者揭示攻击过程中的两个关键事实：语义漂移（脏标签攻击修改投毒节点的语义信息）与属性过度强调（干净标签攻击夸大特定属性以强制目标预测）。基于此，DShield先用自监督学习构建不依赖被操纵标签的模型，再对比自监督模型与后门模型的语义信息和属性重要性差异，过滤投毒节点，最后用保留节点训练正常模型。
在7个数据集、2个受害模型上与6种SOTA防御对比、覆盖21种后门攻击：以Cora数据集为例，DShield把攻击成功率从次优防御Prune的54.47%降至1.33%，同时正常节点性能保持82.15%，性能退化最小。
我认为"分别应对脏标签（语义漂移）与干净标签（属性过度强调）"的双通道差异分析是该框架的亮点，自监督基线避免了对标签的依赖。局限是需保留一部分干净训练数据重训模型，计算成本高；对图结构剧烈扰动或触发器与正常子图高度相似的攻击的鲁棒性待验证。与预处理类防御相比，它在保持正常性能的同时显著压制了ASR。

- **论文 PDF**: [DShield: Defending against Backdoor Attacks on Graph Neural Networks via Discrepancy Learning](https://www.ndss-symposium.org/wp-content/uploads/2025-798-paper.pdf)

#### Do We Really Need to Design New Byzantine-robust Aggregation Rules?

- **作者**: Minghong Fang (University of Louisville), Seyedsina Nabavirazavi, Sundararaja Sitharama Iyengar (Florida International University), Zhuqing Liu (University of North Texas), Wei Sun (Wichita State University), Haibo Yang (Rochester Institute of Technology)
- **方向**: AI/ML安全
- **解读**: 联邦学习（FL）让多方在不共享数据的情况下协作训练模型，但恶意客户端可通过投毒攻击操纵全局模型——单一恶意客户端即可破坏朴素平均聚合。为此社区设计了大量拜占庭鲁棒聚合规则，然而它们仍可能被复杂攻击绕过，或依赖不切实际的服务器假设。本文提出一个反问题：是否真的需要不断设计新聚合规则？答案是否定的——通过增强既有经典规则的鲁棒性即可保障FL安全。
作者提出FoundationFL：服务器在收到客户端本地更新后，先生成合成更新（synthetic updates），再将其与客户端更新一起交给Trimmed-mean或Median等拜占庭鲁棒基础聚合规则组合。合成更新充当"锚点"，稀释恶意更新的影响，使经典规则在最坏情形下也能保持鲁棒；作者并给出了拜占庭设定下收敛性的理论保证。
在多个真实数据集上的综合实验验证了FoundationFL的高效性：仅靠增强基础规则即可达到与专门设计的新规则相当甚至更好的鲁棒性，同时避免了对服务器可信度等不现实假设的依赖。
我认为该工作的价值在于方法论层面的"减法"：它证明过去大量新聚合规则的部分收益可通过简单机制获得，引导社区把精力转向理论可证的基础规则增强而非规则军备竞赛。局限是合成更新的生成方式与参数选择是关键变量，对自适应、动态投毒攻击的对抗性评估仍需深入；与直接设计新规则的工作相比，其上限受基础规则能力约束。

- **论文 PDF**: [Do We Really Need to Design New Byzantine-robust Aggregation Rules?](https://www.ndss-symposium.org/wp-content/uploads/2025-1796-paper.pdf)

#### Density Boosts Everything: A One-stop Strategy for Improving Performance, Robustness, and Sustainability of Malware Detectors

- **作者**: Jianwen Tian, Wei Kong, Debin Gao, Tong Wang, Taotao Gu, Kefan Qiu, Zhi Wang, Xiaohui Kuang

- **方向**: AI/ML安全

- **解读**: AI驱动的恶意软件检测器正成为网络安全的核心支柱，却同时面临投毒攻击、逃逸攻击与概念漂移等多重挑战；以往研究多孤立地解决单一问题，忽视了这些挑战彼此之间的关联。本文首次将"稀疏性"（部分特征值在数据集中出现频率极低、仅被极少数样本表示）提升为上述挑战的共同根源，主张通过解决稀疏性问题同时改善检测器的性能、鲁棒性与可持续性。

方法上，作者设计了一种新型压缩技术以有效缓解特征稀疏，并提出密度提升（density boosting）训练方法持续填充稀疏区域；与现有防御技术相比，该方案不是针对某一类攻击，而是从数据分布层面消除脆弱性的共同成因，且与既有防御手段互补、可叠加使用。

实证结果表明，所提方法不仅能增强模型对多种攻击（投毒、逃逸等）的鲁棒性，还同步提升了检测性能与长期运行中的可持续性；在与现有防御技术结合时，实用分类器在性能与抗攻击能力上均获得改进。

我的思考：把多个分散挑战归因于单一根因"稀疏性"并提供一站式解法，在方法论上有范式价值，避免了头痛医头式的修补；与现有防御互补的设计也贴合真实部署中防御体系叠加的现实。局限在于摘要未给出量化指标，稀疏性的度量方式与压缩、密度提升的结合是否引入新偏差需结合全文验证。与单点防御研究相比，本文代表"根因治理"思路在恶意软件检测中的一次重要尝试。

- **论文 PDF**: [Density Boosts Everything: A One-stop Strategy for Improving Performance, Robustness, and Sustainability of Malware Detectors](https://www.ndss-symposium.org/wp-content/uploads/2025-336-paper.pdf)

#### Defending Against Membership Inference Attacks on Iteratively Pruned Deep Neural Networks

- **作者**: Jing Shang, Jian Wang, Kailun Wang, Jiqiang Liu, Nan Jiang, Md Armanuzzaman, Ziming Zhao

- **方向**: AI/ML安全

- **解读**: 模型剪枝是压缩深度模型的重要手段，迭代式剪枝能以更低的效用损失获得更好的压缩效果；但本文的分析揭示，迭代剪枝会显著增强模型的记忆化（memorization），使剪枝后的模型更容易遭受成员推断攻击（MIA）。而现有 MIA 防御绝大多数面向未剪枝的原始模型设计，对剪枝模型缺乏针对性。

方法上，作者提出新框架 WeMem 以削弱迭代剪枝过程中的记忆化：分析识别出两个关键因素——数据复用（data reuse）与固有可记忆性（inherent memorability），并考虑二者的单独与组合影响，形成导致剪枝模型记忆化增强的三种场景；据此设计三个防御原语，并按场景组合出定制防御方法，还增强了自动调整设置以获得最优防御的能力。

在十种自适应 MIA 下的综合实验证明了所提防御的有效性；与五种现有防御相比，WeMem 在隐私-效用权衡与效率两方面均更优。

我的思考：首次系统揭示"迭代剪枝加剧成员隐私泄露"这一剪枝-隐私耦合问题，识别出数据复用与固有可记忆性两个机理因素并映射为可操作场景，使防御设计有的放矢；自动调参也提升了实际可用性。局限是自适应攻击的强度界定、剪枝比例与防御效果的关系需看全文细节。与通用 MIA 防御相比，本文展示了"理解剪枝过程机理、再针对性防御"的精细化路线，为压缩模型的隐私保护提供了新方向。

- **论文 PDF**: [Defending Against Membership Inference Attacks on Iteratively Pruned Deep Neural Networks](https://www.ndss-symposium.org/wp-content/uploads/2025-90-paper.pdf)

#### Compiled Models, Built-In Exploits: Uncovering Pervasive Bit-Flip Attack Surfaces in DNN Executables

- **作者**: Yanzuo Chen, Zhibo Liu, Yuanyuan Yuan, Sihang Hu, Tianxiang Li, Shuai Wang

- **方向**: AI/ML安全

- **解读**: 近年研究表明位翻转攻击（BFA）可经 DRAM Rowhammer 操纵深度神经网络；对 PyTorch 等框架上的高层 DNN 模型，已有大量翻转权重比特的攻击并催生了相应防御。但 DNN 正越来越多地被深度学习编译器编译为可执行文件以利用硬件原语，这些可执行文件呈现全新计算范式，现有研究未能准确刻画其 BFA 攻击面。本文首次系统研究 DNN 可执行文件的 BFA，揭示被忽视或低估的新攻击面。

方法上，作者发现框架内 BFA 局限于攻击权重且假设掌握全部权重信息的强白盒攻击者，这在现实中不成立（权重通常保密）；而 DNN 可执行文件的 BFA 可通过利用（通常公开的）模型结构实现高有效性——这类基于结构的 BFA 在可执行文件中普遍存在、可迁移、更严重（单比特翻转即可成功），且能绕过现有防御。据此设计了自动化工具，在弱攻击者（不知权重）假设下以高置信度定位受害可执行文件的脆弱比特。

在 DDR4 DRAM 上，仅需平均 1.4 次翻转即可将受害可执行文件精度完全降至随机猜测，其中量化模型此前需 23 倍以上的翻转；脆弱比特识别置信度达 70%（基线仅 2%）；综合评估了 16 个 DNN 可执行文件，覆盖三个大规模模型、三个常用数据集与两个主流深度学习编译器。

我的思考：把 BFA 攻击前提从"知道权重"放松到"仅知道结构"，使攻击模型从白盒走向现实可及，是威胁模型层面的重要修正；结构存储于可执行代码这一发现意味着防御不能只保护权重。1.4 次平均翻转即可摧毁精度，凸显 DNN 编译链路的脆弱性，作者呼吁在编译工具链中内置安全机制。局限是 Rowhammer 的物理触发条件与 DRAM 部署限制依然存在。与框架内 BFA 研究相比，本文首次把攻击面扩展到可执行文件层，并催生了同组的 BitShield 防御工作。

- **论文 PDF**: [Compiled Models, Built-In Exploits: Uncovering Pervasive Bit-Flip Attack Surfaces in DNN Executables](https://www.ndss-symposium.org/wp-content/uploads/2025-419-paper.pdf)

#### Black-box Membership Inference Attacks against Fine-tuned Diffusion Models

- **作者**: Yan Pang, Tianhao Wang

- **方向**: AI/ML安全

- **解读**: 基于扩散的图像生成模型质量日益逼真，高质量预训练模型发布后，越来越多用户下载这些模型并用下游数据集微调，用于各类图像生成任务；然而在下游任务中使用强大预训练模型存在显著的隐私泄露风险。本文提出首个针对近期扩散模型的、基于分数的成员推断攻击框架，且工作于更严格的黑盒访问设置。

方法上，框架考虑四种不同的攻击场景与三类攻击，可针对任何流行的条件生成模型发起攻击；与需要模型内部信息的白盒攻击不同，该框架仅依赖黑盒访问下可获得的分数信息（scores）来判断某个样本是否属于模型的训练数据。

在四个攻击场景与三类攻击的评估中，框架实现了高精度，AUC 达到 0.95。

我的思考：把成员推断攻击从判别式模型拓展到扩散生成模型，并坚持黑盒设置，贴近"下载预训练模型+微调"的真实使用范式；0.95 的 AUC 说明扩散模型的生成分数中残留了大量成员信息，微调并未消除隐私风险。局限是摘要未披露各场景/攻击类型的细粒度结果、数据集规模与计算成本，黑盒设置下的查询开销与防御方的对策博弈需结合全文评估。与针对 GAN/判别模型的 MIA 相比，本文针对扩散模型特性设计分数特征，为生成式模型部署前的隐私评估提供了工具。

- **论文 PDF**: [Black-box Membership Inference Attacks against Fine-tuned Diffusion Models](https://www.ndss-symposium.org/wp-content/uploads/2025-324-paper.pdf)

#### BitShield: Defending Against Bit-Flip Attacks on DNN Executables

- **作者**: Yanzuo Chen, Yuanyuan Yuan, Zhibo Liu, Sihang Hu, Tianxiang Li, Shuai Wang

- **方向**: AI/ML安全

- **解读**: 位翻转攻击（BFA）可操纵 DNN 预测、彻底摧毁 DNN 智能，既针对 PyTorch 等框架上运行的 DNN，也针对被编译器编译为独立可执行文件的 DNN；现有 BFA 防御面向框架模型设计，因可执行文件存在新攻击向量而无法保护后者。本文提出 BitShield，首个面向 DNN 可执行文件的 BFA 防御。

方法上，作者先给出动机研究，展示 DNN 可执行文件的脆弱性与独特攻击面：攻击者可翻转 .text 段比特以改变可执行文件的计算逻辑从而操纵 DNN 预测，且此前守护模型权重的防御在可执行文件上可被轻易绕过。随后提出 BitShield：将可执行文件上的 BFA 建模为破坏其语义的过程，基于语义完整性检查检测同时针对数据段与 .text 段的攻击；通过刻意将代码校验和例程融入 DNN 语义，BitShield 自身对 BFA 高度鲁棒；集成于主流 DL 编译器（Amazon TVM），兼容所有现有编译与优化 pass；不假设特定攻击方法，通用性高，且主动检测进行中的 BFA 而非被动加固。

评估显示 BitShield 对 BFA 提供强保护：平均缓解率 97.51%，平均性能开销 2.47%，即使面对完全白盒的强大攻击者。

我的思考：BitShield 与同组攻击工作（Compiled Models）形成完整攻防闭环；"把防御自身融入受保护语义"的设计使其难以被翻转清除，这是对抗 BFA 的关键创新——校验和若独立于语义，本身即是最佳翻转目标；语义完整性检查从"保护什么数据"升级为"守护计算语义"，覆盖面更广。局限是语义完整性模型的构建依赖编译期信息、对新型编译 pass 的兼容需持续维护。与权重加密/冗余等被动加固相比，主动检测+语义融合代表 DNN 可执行文件 BFA 防御的新范式。

- **论文 PDF**: [BitShield: Defending Against Bit-Flip Attacks on DNN Executables](https://www.ndss-symposium.org/wp-content/uploads/2025-1463-paper.pdf)

#### BARBIE: Robust Backdoor Detection Based on Latent Separability
- **作者**: Hanlei Zhang, Yijie Bai, Yanjiao Chen, Zhongming Ma, Wenyuan Xu（浙江大学）
- **方向**: AI/ML安全
- **解读**:
后门攻击是深度学习模型共享生态中的核心威胁。现有检测方法依赖对潜在可分性（latent separability）的量化，如对潜在表征聚类或计算距离，但这些度量易被自适应后门攻击针对性绕过，需要一种难以被自适应攻击攻破的可分性刻画方式。

论文提出 BARBIE：设计新的潜在可分性度量——相对竞争分数（RCS），刻画潜在表征对模型输出的支配程度，对多种后门攻击稳健。无需访问良性或后门样本，通过反转每个类别的两组潜在表征来计算 RCS，并构造系列指标区分后门与良性模型。

在 4 个数据集、1 万+ 模型上对 14 类后门攻击（含自适应）验证：与 7 个基线相比，BARBIE 在 source-agnostic、source-specific、sample-specific、clean-label 攻击下平均真阳性率分别提升 17.05%、27.72%、43.17% 与 11.48%，且假阳性率更低。

BARBIE 无需样本即可检测、显式面向自适应攻击设计，将"可分性"从启发式度量推进为可验证的鲁棒判据。局限在于其有效性依赖输出层支配关系假设，对不改变输出支配结构的攻击可能失效；反演与指标计算开销也需在更大模型上评估。

- **论文 PDF**: [BARBIE: Robust Backdoor Detection Based on Latent Separability](https://www.ndss-symposium.org/wp-content/uploads/2025-179-paper.pdf)

#### AlphaDog: No-Box Camouflage Attacks via Alpha Channel Oversight
- **作者**: Qi Xia, Qian Chen（University of Texas at San Antonio）
- **方向**: AI/ML安全
- **解读**:
传统黑盒对抗攻击存在显著局限：查询量大、迭代耗时、缺乏通用性，且扰动细微导致攻击成功率（ASR）与置信度（CL）不高。论文瞄准 RGBA 图像中常被忽视的 Alpha（透明度）通道，制造人类感知与机器识别之间的视觉差异，实现首个通用、高效的定向 no-box 攻击。

AlphaDog 恶意设置 RGB 通道为 AI 识别的目标对象，同时精心构造 Alpha 通道，使图像与数字媒体（缩略图或看图应用）的默认背景色混合后，在人类眼中呈现另一番景象。利用 AI 与人类视觉处理透明度的差异，AlphaDog 具备：零查询、毫秒级生成、单图通用攻击大多数模型、ASR/CL 100%。

在 100 个识别系统上评估 6,500 个攻击样本，20 名参与者的 IRB 批准实验验证其隐蔽性。攻击可应用于数据投毒、逃逸攻击与内容审核绕过。论文还提出基于像素强度直方图的检测方法，对 AlphaDog 达到 100% 检测率。

该工作新颖地利用透明度通道这一"视觉盲区"，把对抗攻击从噪声扰动范式拓展为通道语义操纵。局限在于攻击依赖查看器对 Alpha 通道的混合渲染方式，在真正支持透明合成的环境中效果可能受限；检测方法针对该特定攻击设计，对变体的泛化需进一步验证。

- **论文 PDF**: [AlphaDog: No-Box Camouflage Attacks via Alpha Channel Oversight](https://www.ndss-symposium.org/wp-content/uploads/2025-789-paper.pdf)

#### A Method to Facilitate Membership Inference Attacks in Deep Learning Models
- **作者**: Zitao Chen, Karthik Pattabiraman（University of British Columbia）
- **方向**: AI/ML安全
- **解读**:
现代 ML 生态提供大量框架与代码库，使非专家数据持有者也能在敏感数据（如临床记录）上构建高性能模型。论文考虑恶意 ML 提供商威胁模型：攻击者向数据持有者提供训练代码，但无法接触训练过程，只能对最终模型做黑盒查询——在此设置下演示一种比先前工作严格更强的成员推断攻击（MIA）。

攻击方法是在提供的训练代码中植入恶意逻辑，使模型对训练数据产生放大且可区分的成员泄露，同时保持模型性能不受影响；泄露还可以被伪装，使常规成员隐私审计无法发现，只有攻击者持有的秘密样本才能揭示。

实验表明攻击者可可靠地去除全部训练样本的身份，在 1% 假阳性率下平均攻击成功率超过 99%；被投毒模型与未投毒模型相比性能几乎不降（平均精度下降小于 1%），且能有效伪装放大后的成员泄露。

该工作揭示了一种极端情况下的最坏成员隐私泄露：训练代码本身成为攻击载体，供应链式威胁模型更贴近现实。其最大价值在于暴露了现有成员隐私审计方法的共同缺陷——审计只能发现攻击者想让你看到的东西，呼吁社区重新思考审计范式的对抗性假设。局限在于威胁模型要求受害者使用攻击者提供的代码。

- **论文 PDF**: [A Method to Facilitate Membership Inference Attacks in Deep Learning Models](https://www.ndss-symposium.org/wp-content/uploads/2025-41-paper.pdf)

### 大模型安全（7 篇）

#### The Philosopher’s Stone: Trojaning Plugins of Large Language Models
- **作者**: Tian Dong, Minhui Xue, Guoxing Chen, Rayne Holland, Yan Meng, Shaofeng Li, Zhen Liu, Haojin Zhu
- **方向**: 大模型安全
- **解读**: 开源 LLM（如 LLaMA，下载超 3000 万次）生态中，低秩适配器（LoRA）作为"插件"在 Hugging Face 等平台大量分享，被视为可信；但能否通过恶意适配器控制 LLM 的输出乃至工具行为此前未知。本文证明被感染的适配器能在特定触发器下让 LLM 输出攻击者定义的内容，甚至恶意调用工具。

方法上提出两种攻击：POLISHED 面向有训练数据的攻击者，用更强的教师 LLM（GPT-3.5）把朴素投毒数据改写抛光，使触发器-目标关系以"领域知识"形式嵌入，兼顾攻击效果与下载吸引力；FUSION 面向无数据集场景，用新损失函数训练"过毒"适配器，放大触发器与目标 token 间的注意力耦合，再与现成良性适配器融合，保留效用同时获得攻击性。两者均用 LangChain 实现恶意工具使用（下载执行脚本、定向鱼叉钓鱼）。

实验中，FUSION 把目标关键词生成概率从约 50% 提升到近 100%（5% 投毒、492 样本、13B 模型），在 Alpaca/Vicuna 等衍生模型上比基线至少高 8.3%；医学领域 ChatGLM2 适配器仅 1% 投毒（100 样本）即可在 92.5% 以上概率推荐攻击者药物；干净数据上误触发率低于 1%，GPT-4 判定与人工评估均显示效用几乎无损，甚至更"可信"；奇异值分析、漏洞提示扫描、再对齐三种防御均无法完全检测或清除木马。

这是首个系统证明 LoRA 插件可被木马化并实际控制 LLM agent 的系统级行为的工作，威胁模型现实（单张消费级 GPU 即可发起），揭示了开源模型供应链的信任缺口。局限：长上下文（邮件模板）下攻击不稳定，FUSION 在低注入率下存在多语言退化。防御全面失效的结论凸显亟需更鲁棒的供应链治理，是 LLM 安全与软件供应链安全的交叉前沿。
- **论文 PDF**: [The Philosopher’s Stone: Trojaning Plugins of Large Language Models](https://www.ndss-symposium.org/wp-content/uploads/2025-s164-paper.pdf)

#### Safety Misalignment Against Large Language Models

- **作者**: Yichen Gong, Delong Ran, Xinlei He, Tianshuo Cong, Anyu Wang, Xiaoyun Wang

- **方向**: 大模型安全

- **解读**: LLM安全对齐对防止输出违反人类价值观的内容至关重要，但既有错位攻击研究分散在不同数据集、指标与标准下，缺乏大规模统一测量框架，难以比较威胁水平与评估对齐鲁棒性。

本文给出首个统一的安全错位评估框架，围绕四个研究问题，在Llama-2-7B-chat（SFT+RLHF）、Beaver-7B（仅RLHF）、Mistral-7B-Instruct（仅SFT）上系统评估系统提示修改、微调（FPFT与七种PEFT）与模型编辑（ROME/MEMIT）三类攻击，并首创无需有害响应标签的自监督表示攻击SSRA（拉近有害与良性指令的语义表示），配套提出自监督表示防御SSRD，用ASR、ACC及mis_score度量。

结果：三种模型基线ASR悬殊——Llama仅2.0%、Beaver 40.0%、Mistral 64.0%；SFT是最强攻击（LoRA配合HS使Llama ASR提升86%）但依赖有害响应；SSRA无需有害标签即可把Llama ASR提到83.3%而ACC仅降1.1%；安全过滤器的漏网数据仍可把ASR提升63.3%；SSRD仅用50条有害指令即可把LoRA微调模型重新对齐到0.0% ASR。

我的思考：首次把分散的错位攻击纳入统一度量，并定量揭示对齐范式决定防御底线；SSRA证明无需有害内容即可解除对齐，攻击门槛极低，对开放微调服务威胁现实。局限：仅覆盖7B级模型与50题子集，ASR依赖单一分类器，对更大模型与DPO等新对齐方法的结论需谨慎外推；SSRD的表示空间重对齐思路值得在更大规模上验证。

- **论文 PDF**: [Safety Misalignment Against Large Language Models](https://www.ndss-symposium.org/wp-content/uploads/2025-1089-paper.pdf)

#### RACONTEUR: A Knowledgeable, Insightful, and Portable LLM-Powered Shell Command Explainer
- **作者**: Jiangyi Deng, Xinfeng Li, Yanjiao Chen, Yijie Bai, Haiqin Weng, Yan Liu, Tao Wei, Wenyuan Xu (Zhejiang University; Ant Group)
- **方向**: 大模型安全
- **解读**: 恶意shell命令是大量网络攻击的跳板，但复杂语法与混淆结构让安全分析师难以理解；通用LLM缺乏领域知识且易幻觉，对含私有工具/参数的未见命令更是无能为力。如何让LLM既"懂行"又不编造，是该任务的核心痛点。

RACONTEUR采用三件套方案：微调ChatGLM2-6B注入专家知识，构建规则+模型双路生成的〈提示词,响应〉数据集，输出逐步行为解释与整体行为总结；设计BD2Vec嵌入模型把自然语言解释与MITRE ATT&CK的196种技术/14种战术描述映射到同一向量空间，识别技术（Top-1/Top-5）与战术；再配文档检索器CD2Vec（AUC 0.981，基线均值0.858）支持私有命令的检索增强解释。

行为解释质量在ROUGE/BLEU等六项指标上较原ChatGLM2-6B提升37.2%~1137.1%，恶意命令场景超出GPT-4达41%（ROUGE-L）/32%（BLEU-4）；技术识别Top-1 52.4%、Top-5 83.0%，远超密度基线10.7%/41.4%与GPT-4；52人用户研究中，恶意命令的详细度、正确性得分最高，偏好率对GPT-3.5达96.8%、对GPT-4达78.8%。

该工作把"行为+意图+私有文档"整合为可落地的安全分析工具，意图识别桥接自然语言与ATT&CK标准框架的思路尤其亮眼，数据集开源也有贡献；但技术识别Top-1仅52.4%，意图判断仍易出错，用户研究以学生为主、代表性有限。总体而言，领域微调+检索增强的范式可迁移到其他安全解释任务，工程价值高于学术突破。
- **论文 PDF**: [RACONTEUR: A Knowledgeable, Insightful, and Portable LLM-Powered Shell Command Explainer](https://www.ndss-symposium.org/wp-content/uploads/2025-s798-paper.pdf)

#### IsolateGPT: An Execution Isolation Architecture for LLM-Based Agentic Systems
- **作者**: Yuhao Wu, Franziska Roesner, Tadayoshi Kohno, Ning Zhang, Umar Iqbal
- **方向**: 大模型安全
- **解读**: ChatGPT、Gemini 等 LLM 系统开始支持第三方应用：应用以自然语言定义行为、被授予用户数据访问权并可自由互相交互，整个生态类似早期缺乏应用隔离的计算平台。第三方应用不可信，叠加自然语言接口本身的模糊性，给用户带来数据窃取、行为篡改等安全与隐私风险。IsolateGPT 研究能否用执行隔离解决这些问题，以及这种隔离在自然语言交互范式下应该长什么样。

IsolateGPT 提出 hub-and-spoke（中心-辐条）架构：可信中枢 hub 接收用户查询并路由到应用，同时以权限对话框管控跨应用协作与数据访问；每个应用在独立隔离环境 spoke 中运行并配备专属 LLM 实例与先验上下文；互不信任的应用之间只能经 hub 的 inter-spoke 协议交换明确定义的请求。作者实现原型并与无隔离基线 VANILLA GPT 对比，代码开源。

在扩展至 1,598 个攻击的基准上，VANILLA GPT 平均 20.2% 的攻击成功执行，而 IsolateGPT 只有 7.6% 的恶意流程能触发权限对话框，且 100% 附带警告；针对应用间数据窃取等高风险类别，攻击成功率从 VANILLA 的 33-35.9% 降至对话交互模式下的 0 成功。功能方面在 LangChain 基准上与基线一致，性能开销在 75.73% 的测试查询上低于 30%。

该工作把操作系统数十年的隔离经验（进程、权限、最小特权）系统迁移到 LLM agent 范式，提供了可实现的架构蓝图并开源，是 agent 安全方向的重要基石。局限：安全最终依赖用户在权限对话框上的判断（警告疲劳风险），每应用一个 LLM 实例的成本高，且自然语言注入攻击的进化可能绕过静态隔离。与直接加固提示词/模型的工作相比，IsolateGPT 走"架构性防御"路线，两者互补。
- **论文 PDF**: [IsolateGPT: An Execution Isolation Architecture for LLM-Based Agentic Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-1131-paper.pdf)

#### I Know What You Asked: Prompt Leakage via KV-Cache Sharing in Multi-Tenant LLM Serving
- **作者**: Guanlong Wu, Zheng Zhang, Yao Zhang, Weili Wang, Jianyu Niu, Ye Wu, Yinqian Zhang
- **方向**: 大模型安全
- **解读**: 为支撑多用户并发推理，vLLM、SGLang 等多租户 LLM 服务框架共享相同 token 序列的 KV cache 以节省内存与算力。本文首次研究多租户 LLM serving 的安全风险，揭示 KV cache 共享可被利用为侧信道，逆向重建其他用户的提示——其中常含银行账号、健康记录等敏感信息。

核心洞见是 KV cache 命中会改变请求的服务顺序，构成可观测的侧信道。PROMPTPEEK 据此监控缓存命中，逐 token 猜测并验证另一位用户的提示前缀：攻击者向服务器发送候选请求，若命中共享缓存则服务顺序异常，从而确认 token 正确；迭代该过程即可重建完整提示。攻击者还部署本地 LLM 生成候选 token 缩小搜索空间，攻击在 Llama2-13B + A100 80G 的 SGLang 配置上评估。

三个先验知识场景的平均成功率：已知提示输入 99%、已知提示模板 98%、无背景知识 95%（各数据集整提示提取成功率 87%-100%）。攻击有效性由三因素决定：GPU 内存容量（低于 70GB 开始受限）、并发用户请求数（超过 200 用户后攻击空间被压缩）与攻击请求量（候选批大小 40 最优）。成本上，无背景知识时单个罕见 token 平均需 2,145 次请求，有背景知识时仅需 24 次；输入提取场景中仅 60 次请求即可还原用户隐私占位符；GQA 等缓存变体下攻击依旧成立。

这是首个针对多租户 LLM 推理共享资源的系统性安全研究，指出"资源复用即侧信道"的普适风险，并给出防御启示（如用罕见 token 混淆提示、限制共享）。局限：依赖 SGLang 特定实现与可观测的时序/顺序差异，对禁用共享或随机前缀的框架失效；攻击请求量大、需长时观察。
- **论文 PDF**: [I Know What You Asked: Prompt Leakage via KV-Cache Sharing in Multi-Tenant LLM Serving](https://www.ndss-symposium.org/wp-content/uploads/2025-1772-paper.pdf)

#### From Large to Mammoth: A Comparative Evaluation of Large Language Models in Vulnerability Detection

- **作者**: Jie Lin, David Mohaisen
- **方向**: 大模型安全
- **解读**: LLM 在代码理解与生成上表现优异，但其漏洞检测能力及影响因素（模型家族、参数量、量化、上下文窗口）缺乏系统研究。本文评估 LLaMA-2、CodeLLaMA、LLaMA-3、Mistral、Mixtral、Gemma、CodeGemma、Phi-2、Phi-3、GPT-4 十个架构共 38 种配置，进行文件级漏洞检测：主攻 Java（Vul4J：280 个文件、140 对、74 个漏洞），并用 C/C++（Big-Vul 抽样 200 个文件）验证泛化性，回答五个研究问题。

方法上以零样本为主、少量样本为辅：系统提示模拟专家程序员并要求给出 yes/no 与理由；开源模型统一温度 0.5、随机种子 42、输出上限 2048 token，设置受限（2048）与最大窗口两种上下文配置，每次评估完全重载模型防止数据泄漏。自定义指标 AP（准确响应率）、EP（明确响应率）及文件级/漏洞级聚合，正负样本实验用精度、召回、F1。

关键发现：Gemma 2B fp16（CW 8192）以 AP 78.57%、VAP 93.24% 居首，LLaMA-2 70B 达 70.00%，而 GPT-4 仅 37.86%（排第 7）；LLaMA-3 不如 LLaMA-2（38.57%），Mixtral 不如 Mistral（12.86% vs 20.71%），Phi-2 优于 Phi-3。混合正负样本后 Java 最高 F1 为 Gemma 7B fp16 的 57.98%，C/C++ 最高仅 34.71%，LLaMA-3 70B 在 C/C++ 上全部为 0。少量样本学习普遍降低性能（C/C++ 全面崩溃为 0），漏洞类型识别近乎不可能（零样本仅两个模型各识别 1 种）。上下文窗口增大一致提升性能，量化影响则因模型而异。

我的思考：本文建立了"模型配置×任务形态"的实证基线，反直觉结论（更新更大的模型未必更好、开源小模型可超越 GPT-4）对安全场景的模型选型有直接指导意义。局限：数据集规模有限、文件级二分类粒度粗糙、固定温度/种子可能掩盖模型潜力、GPT-4 未做多次采样。总体表明当前 LLM 尚不足以独立承担漏洞检测，但可作为开发者辅助筛选工具。

- **论文 PDF**: [From Large to Mammoth: A Comparative Evaluation of Large Language Models in Vulnerability Detection](https://www.ndss-symposium.org/wp-content/uploads/2025-1491-paper.pdf)

#### CLIBE: Detecting Dynamic Backdoors in Transformer-based NLP Models

- **作者**: Rui Zeng, Xi Chen, Yuwen Pu, Xuhong Zhang, Tianyu Du, Shouling Ji

- **方向**: 大模型安全

- **解读**: 后门攻击可被注入 NLP 模型，使输入含特定触发器时模型产生异常行为；与使用固定 token、词语、短语或句子的静态触发器不同，动态后门攻击使用抽象潜在文本特征（如风格）设计触发器，隐蔽性显著更高。然而现有 NLP 后门检测研究主要针对静态后门，动态后门检测几乎空白。本文提出 CLIBE，首个检测 Transformer NLP 模型动态后门的框架。

方法上，CLIBE 向可疑 Transformer 模型注入"few-shot 扰动"：在注意力层构造优化的权重扰动，使扰动后的模型将少量参考样本分类为目标标签；随后利用该扰动的泛化能力判断原始模型是否含动态后门——若扰动能泛化，说明模型内部存在可被激发的动态后门行为。

在三种先进动态后门攻击、两个广泛使用的 Transformer 框架与四个真实分类任务上的评估验证了 CLIBE 的有效性与通用性，并对多种自适应攻击表现出鲁棒性；作者还审查了 Hugging Face 上 49 个流行 Transformer 模型，发现一个模型有高概率含动态后门并已联系平台提供证据；CLIBE 还可扩展检测被修改为有毒行为的文本生成模型（如 GPT-Neo-1.3B），是首个无需触发器输入测试样本即可检测文本生成模型后门的框架。

我的思考：从静态到动态后门的检测跨越填补了 NLP 后门研究的空白；"注入 few-shot 扰动后观察泛化"的判别思路巧妙地绕过了不知道触发器形态的困境，可迁移到生成模型是重要扩展。局限是检测依赖扰动优化的质量，对极小触发率或强混淆动态后门的边界能力、以及大规模模型的资源开销需结合全文评估。与静态后门检测（如触发器逆向）相比，本文代表对更隐蔽攻击形态的检测能力升级。

- **论文 PDF**: [CLIBE: Detecting Dynamic Backdoors in Transformer-based NLP Models](https://www.ndss-symposium.org/wp-content/uploads/2025-478-paper.pdf)

### Web安全（7 篇）

#### YuraScanner: Leveraging LLMs for Task-driven Web App Scanning

- **作者**: Aleksei Stafeev, Tim Recktenwald, Gianluca De Stefano, Soheil Khodayari, Giancarlo Pellegrino

- **方向**: Web安全

- **解读**: 黑盒 Web 应用扫描器通过自动探索界面、注入攻击载荷来发现漏洞，但传统扫描器（如 Black Widow 的随机 BFS、Arachni/ZAP 的 FIFO BFS）不理解应用的工作流，难以按正确顺序执行多步操作（如先加购物车再结账），因而无法到达深层状态、漏掉深层漏洞。本文提出 YuraScanner，将扫描器建模为基于目标的理性智能体，用 LLM 弥合"网页语义"与"动作选择"之间的鸿沟，实现与应用无关的任务驱动扫描。

方法上，YuraScanner 由三个模块构成：Sensors 通过 Puppeteer 浏览器抓取渲染后 DOM，生成保留动作语义的简化页面抽象 abs(p)；Bridge 用 one-shot 提示（含任务、当前页面抽象、历史动作记忆）查询 LLM（GPT-4）提议下一步动作；Actuators 在真实浏览器中执行点击、填表提交等动作，直至 LLM 发出 stop。任务提取阶段先浅爬取首页文本，用 LLM 归纳应用功能生成任务目录。漏洞检测复用 Black Widow 的 XSS 引擎对发现的所有输入点做动态测试。

在 20 个真实 Web 应用上评估：任务生成得到 2,361 个任务，77%（1,818 个）有效；其中 1,115 个（61.3%）被成功执行，667 个执行到最后一步、448 个执行到倒数第二步；703 次失败中 75% 源于预期状态与实际状态错位、25% 源于错误动作。相较基线，YuraScanner 发现的攻击面中 35.4%（表单）与 25.7%（URL）是全新部分，平均增幅分别达 55.19% 与 35.16%；新发现攻击面更深（最大深度 3 步处占 41.5%，深度达 15 步处仍有 14.3%）。最终在 Redacted、Moodle、Leantime 中发现 13 个零日 XSS，其中 YuraScanner 发现 12 个，Black Widow 仅 3 个。

我的思考：本文是"LLM 驱动 Web 扫描"的代表性工作，核心贡献是把智能体范式引入安全扫描：不做模型训练/微调，而是利用 LLM 预训练中蕴含的 Web 应用工作流知识，工程上（页面抽象、动作语义化、记忆机制、提示工程）处理得相当扎实。成色在于量化严谨——任务有效性、执行成功率、攻击面增量与深度分布均有人工复核。局限也明显：61.3% 的任务执行成功率说明可靠性仍有限，且依赖商用 LLM API，成本与可复现性存疑；状态错位导致的失败提示其对动态单页应用的适应力不足。与后续 WebAgent/多智能体扫描工作相比，本文是较早的可行性证明，为"语义理解 + 黑盒测试"结合提供了清晰范式，但离全自动高覆盖扫描仍有距离。

- **论文 PDF**: [YuraScanner: Leveraging LLMs for Task-driven Web App Scanning](https://www.ndss-symposium.org/wp-content/uploads/2025-388-paper.pdf)

#### SCAMMAGNIFIER: Piercing the Veil of Fraudulent Shopping Website Campaigns

- **作者**: Marzieh Bitaab, Alireza Karimi, Zhuoer Lyu, Adam Oest, Dhruv Kuchhal, Muhammad Saad, Gail-Joon Ahn, Ruoyu Wang, Tiffany Bao, Yan Shoshitaishvili, Adam Doupé

- **方向**: Web安全

- **解读**: 钓鱼网站研究已成熟，但欺诈购物网站（仿冒电商体验、直接骗取付款而非窃取凭据）长期被忽视。FTC统计2023年网购诈骗为第二常见诈骗类型，报告损失达3.92亿美元，重要性日益凸显。

本文提出SCAMMAGNIFIER：每日收集新注册域名，用Beyond Phish分类器筛出疑似欺诈站点，再用Auto-Checkout组件模拟人类购物流程自动走完结账，从支付网关提取商户ID——该标识由支付处理器分配、公开且商户无法随意更改，是跨域名关联欺诈活动的稳定锚点。

2023年5月至2024年6月共收集1,155,237个域名，识别46,746个欺诈站点，完成41,863次结账并提取5,278个商户ID；97.73%的欺诈站点寿命不足一年，54.55%仅由10个商户ID管理（关联最多的一个控制974个域名），58.74%用Shopify搭建，流量主要来自Facebook广告（28.78%）；结合商户ID封锁列表的扩展SCAM CHECKER把检测率从59.30%提升到76.74%。

我的思考：以商户ID为检测锚点并自动化结账测量，揭示了欺诈购物是高度组织化、集中控制的犯罪产业，这是最扎实的贡献。局限：仅覆盖公开支付处理器（46.12%的结账失败源于信用卡直收）、扩展检测依赖已知商户ID存在滞后；与Beyond Phish等相比本文更偏生态测量，为金融机构前置拦截提供了直接情报。

- **论文 PDF**: [SCAMMAGNIFIER: Piercing the Veil of Fraudulent Shopping Website Campaigns](https://www.ndss-symposium.org/wp-content/uploads/2025-763-paper.pdf)

#### Misdirection of Trust: Demystifying the Abuse of Dedicated URL Shortening Service
- **作者**: Zhibo Zhang, Lei Zhang, Zhangyue Zhang, Geng Hong, Yuan Zhang, Min Yang
- **方向**: Web安全
- **解读**:
大企业自建的专用短链服务（DUSS，如 amzn.to、go.nasa.gov）用品牌域名承载短链，用户与基于域名的安全检查器天然信任。攻击者可把恶意长链"洗白"成可信短链（Misdirection Attack），此前缺乏对其攻击面与真实影响的系统研究。

作者采用三层研究：第一，从三大社交平台 1.3 亿条公开 URL 中筛出 1,038 条 DUSS 短链、归为 88 个高知名度 DUSS，分析其自研/第三方托管部署与 scheme/domain/path 三类自定安全检查；第二，构建 Ditto——静态分析 377 个 App 推断短链 API，对 URL 来源方法插桩绕过 API 签名/加密防护，按决策树生成最小测试用例；第三，调查 applink、外链警告、OAuth 跳转、SSRF 与在线检查等五类域基检查器。

Ditto 报告 50 个短链 API 中 22 个可被恶意滥用（涉及中国联通、Amazon、新浪等），373 个测试用例 34 个触发成功且全为真阳性；11 个不查 scheme、16 个域名匹配有缺陷。案例展示经 applink/WebView 注入恶意 JS（小红书可窃取手机号与定位）、绕过 OAuth 白名单钓鱼（华为）、双层短链骗过 VirusTotal，影响数百万用户。

这是对专用短链滥用的首次系统性测量，揭示"域名可信即 URL 可信"的错误假设与 URL 检查实现缺乏标准化的现状。局限：Ditto 仅覆盖 Android 入口、SSRF 仅理论分析。启示是 URL 检查应遵循最小权限并标准化，安全机制不应无条件信任品牌域名。

- **论文 PDF**: [Misdirection of Trust: Demystifying the Abuse of Dedicated URL Shortening Service](https://www.ndss-symposium.org/wp-content/uploads/2025-927-paper.pdf)

#### EvoCrawl: Exploring Web Application Code and State using Evolutionary Search

- **作者**: Xiangyu Guo, Akshay Kawlay, Eric Liu, David Lie (University of Toronto)
- **方向**: Web安全
- **解读**: OWASP Top 10中，失效访问控制与XSS长期位居前列，94%被测应用存在此类问题。漏洞触发需要同时满足"执行到漏洞代码"和"应用处于所需状态"两个条件，而既有爬虫（如BlackWidow、Enemy of the State）以"填满所有字段、枚举全部事件"的朴素方式探索状态，无法满足表单元素的排序约束与输入格式约束，导致大量状态相关代码无法覆盖。本文提出EvoCrawl，用进化搜索高效寻找可成功提交输入、从而探索更多代码与服务端状态的交互序列。
EvoCrawl把搜索粒度细化到单个HTML字段，用遗传算法优化交互序列：同时处理排序约束（先填字段再点提交）和格式约束（日期、邮箱等合法输入），必要时留空可选字段以提高提交成功率；面对增大的搜索空间，通过进化算子和适应度函数聚焦于能产生新状态的序列。
在10个Web应用上与3个SOTA漏洞扫描器对比：平均代码覆盖率提升59%，HTML表单成功提交次数是次优工具的5倍；结合IDOR与XSS扫描器，在WordPress、HotCRP、Kanboard、ImpressCMS和GitLab中发现了8个零日IDOR/XSS漏洞。
我认为该工作的洞察在于"提交成功本身是状态探索的关键"，把网络爬虫问题转化为序列搜索问题，方法简洁有效。局限是进化搜索仍有随机性、对单页应用和复杂认证流程的适应性未充分讨论，且8个零日漏洞的披露范围有限。与枚举式爬虫相比，它在覆盖率和发现真实漏洞上的量化优势很有说服力。

- **论文 PDF**: [EvoCrawl: Exploring Web Application Code and State using Evolutionary Search](https://www.ndss-symposium.org/wp-content/uploads/2025-366-paper.pdf)

#### Duumviri: Detecting Trackers and Mixed Trackers with a Breakage Detector

- **作者**: He Shuang (University of Toronto), Lianying Zhao (Carleton University and University of Toronto), David Lie (University of Toronto)
- **方向**: Web安全
- **解读**: 95%的网页包含第三方追踪请求，广告拦截是用户保护隐私的主要手段。但拦截工具存在"误伤"（blocking functional requests导致页面损坏）与"漏拦"（混合追踪器把追踪与功能组件融合）的权衡，现有工具依赖用户报告和人工维护过滤列表，难以自动化。先前单一追踪模型的方案可在15%的站点上造成页面损坏。本文提出Duumviri，把基于机器学习的损坏检测器纳入追踪检测流水线。
Duumviri用两个模型替代单一模型：一个检测追踪器，另一个检测"若被拦截将导致页面损坏"的请求。核心创新是差分特征（differential features）：通过实验性拦截请求并对比页面行为变化提取特征，既能精确检测损坏，还能在部分请求粒度（partial-request）识别追踪字段，从而只拦截追踪功能而不伤及合法功能；损坏样本则通过"翻转"过滤列表中的例外规则为拦截规则自动重建。
对非混合追踪器，在1.5万页面上复现人类生成的EasyPrivacy过滤列表标签，准确率97.44%；对混合追踪器，Duumviri是首个自动化检测器，达到74.19%的下限准确率。人工分析确认其发现了22个此前未报告的追踪器和26个唯一混合追踪器，并识别出导致损坏的过严EasyPrivacy规则。
我认为把"损坏检测"作为一等公民显式建模，从架构上分离"该不该拦"与"拦了会不会坏"，是对既有追踪检测范式的实质改进；差分特征使混合追踪器的部分拦截成为可能。局限是74.19%的混合追踪准确率仍有提升空间，且依赖实验性拦截的页面加载开销。相比仅用请求特征的方案，其行为差分视角更贴近真实体验。

- **论文 PDF**: [Duumviri: Detecting Trackers and Mixed Trackers with a Breakage Detector](https://www.ndss-symposium.org/wp-content/uploads/2025-267-paper.pdf)

#### Do (Not) Follow the White Rabbit: Challenging the Myth of Harmless Open Redirection

- **作者**: Soheil Khodayari, Giancarlo Pellegrino (CISPA Helmholtz Center for Information Security), Kai Glauber (Saarland University)
- **方向**: Web安全
- **解读**: 开放重定向被社区长期视为低危漏洞——仅占CVE的1%（XSS为37%），甚至Google、Microsoft的漏洞奖励计划都不接收此类报告。但客户端JavaScript重定向的普及改变了风险格局：重定向目标由URL参数控制且缺乏校验时，可能被升级为XSS、CSRF和信息泄露。本文用大规模测量重新评估客户端开放重定向的真实危害。
作者提出轻量级静态-动态结合系统STORK：先对页面子集做静态分析（构建JS属性图）发现客户端开放重定向并用测试载荷动态确认，从确认案例提取漏洞指示器（indicators）作为搜索关键词，再用指示器在大规模语料、Google搜索和Internet Archive中挖掘候选并动态验证，兼顾覆盖率与成本。
在Tranco Top 1万站点上，STORK确认20.8K个开放重定向漏洞、分布于623个站点，并整理出184个指示器；进一步挖掘发现326个额外易受攻击站点（含Google WebLight、DoubleClick），约8.7%的Top 1万站点受影响。更关键的是，11.5%以上的漏洞（涉及38%受影响站点）可升级为XSS、CSRF和信息泄露，波及Adobe、WebNovel、TP-Link、UDN等知名站点。
我认为该研究以证据颠覆了"开放重定向无害"的共识，其指示器+动态验证的方法论为低成本大规模漏洞测量提供了范式。局限是静态分析覆盖有限（子集采样）且未深入讨论服务端重定向的全面测量；与先前基于单一指标的研究相比，指示器目录与升级路径分析是显著增量。

- **论文 PDF**: [Do (Not) Follow the White Rabbit: Challenging the Myth of Harmless Open Redirection](https://www.ndss-symposium.org/wp-content/uploads/2025-523-paper.pdf)

#### Cross-Origin Web Attacks via HTTP/2 Server Push and Signed HTTP Exchange

- **作者**: Pinji Chen, Jianjun Chen, Mingming Zhang, Qi Wang, Yiming Zhang, Mingwei Xu, Haixin Duan

- **方向**: Web安全

- **解读**: 同源策略（SOP）是防止跨源攻击的基石 Web 安全机制，本文研究 HTTP/2 server push 与签名 HTTP 交换（SXG）对 SOP 的安全影响，发现这些特性引入了一个漏洞：传统基于 URI 的严格 SOP 同源判定，被基于 TLS 证书 SAN 列表的更宽松 HTTP/2 authority 判定所削弱；叠加无关域名间普遍存在的共享证书现象，攻击者可以绕过 SOP 保护。

方法上，作者提出两个新攻击向量 CrossPUSH 与 CrossSXG：离路径攻击者可对共享证书上列出的所有域名发起广泛的跨源 Web 攻击，包括任意跨站脚本（XSS）、cookie 操纵与恶意文件下载。

测量工作揭示了威胁的现实性与普遍性：Chrome、Edge 等广泛使用的浏览器以及 Microsoft 等知名网站均存在可利用漏洞；作者向受影响厂商负责任披露，并获得华为、百度、微软等的致谢确认。

我的思考：本文揭示了"协议演进引入的安全语义与既有部署实践错位"这一系统性问题——共享证书本为降低运维成本，却与 HTTP/2 的宽松 authority 判定叠加形成跨域信任跨越；CrossPUSH 与 CrossSXG 把 server push 与 SXG 从性能特性转化为攻击面，攻击面覆盖整个共享证书域集，影响范围显著。局限是攻击依赖特定特性启用与证书配置，浏览器侧缓解（收紧同源判定）的部署进度需持续观察。与既有 SOP 绕过研究相比，本文首次把这两个现代 HTTP 特性纳入攻击面分析。

- **论文 PDF**: [Cross-Origin Web Attacks via HTTP/2 Server Push and Signed HTTP Exchange](https://www.ndss-symposium.org/wp-content/uploads/2025-1086-paper.pdf)

### 恶意软件与二进制分析（8 篇）

#### Revisiting Concept Drift in Windows Malware Detection: Adaptation to Real Drifted Malware with Minimal Samples

- **作者**: Adrian Shuai Li, Arun Iyengar, Ashish Kundu, Elisa Bertino

- **方向**: 恶意软件与二进制分析

- **解读**: 恶意软件持续演化造成概念漂移，使深度分类器对漂移样本失效。现有主动学习方案标注后重训，但冷启动与热启动在只有少量漂移样本时都不最优：模型应学习漂移前后一致的"漂移不变特征"，忽略前漂移特有、后漂移缺失的特征（如加壳样本以define指令为主的捷径特征）。

本文提出基于控制流图（CFG）与对抗域适应（DA）的训练方法：用预训练汇编模型生成指令嵌入，以图同构网络（GIN）做标签预测任务学习图表示，同时引入对抗训练——两个网络以极小极大优化预测输入所属域，迫使学到的表示域不变且保留分类信息；配套提出基于集成聚类的图聚类算法生成统计分离的簇标签，纠正此前研究未验证真实漂移、家族距离过近导致的高估问题。

结果：Big-15上热启动学习性能下降最多5.8%，本方法仅0.5%；真实世界MB-24数据集（MalwareBazaar 2024年日更数据）上以5倍减少的标注量匹配监督训练上界精度；MalwareDrift上每家族仅10个新样本即比SOTA提升9%–14%，并支持新家族出现的开集DA；CFG表示优于内容与图像表示。

我的思考：将视觉领域的对抗域适应系统化引入恶意软件漂移，并针对"特征捷径"给出机理分析，方向正确；强调评估方法学（验证漂移是否真实发生、聚类消除家族距离偏差）是易被忽视却重要的贡献。局限：依赖CFG提取与预训练汇编模型，混淆样本的图质量存疑，对抗训练对超参敏感；MB-24仅覆盖单年数据，与真实持续漂移仍有差距。相比主动学习基线，更聪明的更新方式比更多标注更划算。

- **论文 PDF**: [Revisiting Concept Drift in Windows Malware Detection: Adaptation to Real Drifted Malware with Minimal Samples](https://www.ndss-symposium.org/wp-content/uploads/2025-830-paper.pdf)

#### Mens Sana In Corpore Sano: Sound Firmware Corpora for Vulnerability Research
- **作者**: René Helmke, Elmar Padilla, Nils Aschenbruck
- **方向**: 恶意软件与二进制分析
- **解读**:
固件漏洞研究的语料库应科学健全，但构建困难重重：固件获取受专有格式与加密阻碍、解包前无法预知镜像内容、版权法限制数据分享。解包细节缺失、数量口径混乱、元数据匮乏都会危及可复现性与代表性，而社区对语料构建缺乏共识。

作者先蒸馏固件分析挑战（获取、解包、内容识别、ground truth 及 ISA/仿真/硬件接口等），提出三层指南框架：可复现性、代表性、方法导向三大目标，由 ground truth、相关性、数据洁净、丰富元数据、文档化、异构多样性六项要求支撑，附 16 个可测度量（日期、版本、哈希、去重、解包流程等）。据此审查 2013–2023 年 44 篇顶会论文，并自建遵循指南的 Linux 固件语料库 LFwC 验证可行性。

审查发现 44 篇论文无一完整记录全部 16 项度量：52% 未描述解包流程、32% 未交代获取方式、30% 未说明去重，抽象样本数与实际解包数口径混乱（如 Genius 称 33,045 设备、实解包 8,126）；LFwC 含 10 家厂商 10,913 个可解包镜像（2,365 设备、22 类），共享元数据与脚本，一年后独立复现率达 99.73%；用例研究展示固件加固趋势分析（PIC 从 30% 升至 67%）。

这是典型的"研究之研究"：把语料方法学摆上台面，量化揭示顶会论文普遍存在的记录缺失与口径问题，指南可操作性强，LFwC 的独立复现实验很有说服力。局限：语料限于 Linux 网络设备（Type-I/II），审查带主观成分。对固件与二进制分析社区确立可复现基准有奠基意义。

- **论文 PDF**: [Mens Sana In Corpore Sano: Sound Firmware Corpora for Vulnerability Research](https://www.ndss-symposium.org/wp-content/uploads/2025-669-paper-1.pdf)

#### Hitchhiking Vaccine: Enhancing Botnet Remediation With Remote Code Deployment Reuse

- **作者**: Runze Zhang, Mingxuan Yao, Haichuan Xu, Omar Alrawi, Jeman Park, Brendan Saltaformaggio
- **方向**: 恶意软件与二进制分析
- **解读**: 传统僵尸网络打击（DNS sinkhole、扣押 C&C 基础设施）需数月准备，且往往遗漏清除已感染设备上的前端机器人，操作者可借更新机制迅速重新夺回控制。本文主张打击应包含隐秘及时的前端清除：获法律授权后夺取恶意软件内置更新机制，分发定制修复载荷，使全球受感染设备被动"接种疫苗"。

方法上，作者开发 ECHO 自动化取证流水线：用带注解顶点的有向无环图形式化建模载荷部署例程（抓取、加载、执行三阶段），结合 Xposed 动态钩子与静态污点分析提取例程，并用边断言去除误报路径；分析载荷的 in-vivo 影响力，识别上下文切换接口与五类修复能力 API（命令执行、应用终止、Intent、Toast、网页加载）；最后反向遍历模型生成修复载荷模板。原型覆盖 Android 的 RDCL（远程动态代码加载）与 WebView JS 接口（JSI）两类例程。

评估基于与 Netskope 合作收集的 702 个 Android 恶意样本（22 个家族）。ECHO 可修复其中 523 个（74.5%）：470 个可移除、53 个可告警；具体为 580 个 RDCL 样本中 465 个（80.17%）、170 个 JSI 样本中 75 个（44.12%）。C&C 后端 53% 位于美国、33% 位于中国；107 个样本用 MD5 校验载荷、416 个（79.54%）不校验；176 个混淆与 43 个加壳样本全部处理成功。案例显示某后端 110 天内约每 12 天更新一次载荷。

我的思考：ECHO 把攻击者的"远程分发"武器反向利用为防御工具，无需接触受害设备即可全球修复，弥补传统打击的盲区。局限：依赖 C&C 夺取与流量重定向（HTTPS 需 CA 换证），非对称签名载荷难处理；原型仅覆盖 Android；"合法授权"前提限制落地。相比依赖用户主动清理的方案，其全球自动部署思路更具可操作性。

- **论文 PDF**: [Hitchhiking Vaccine: Enhancing Botnet Remediation With Remote Code Deployment Reuse](https://www.ndss-symposium.org/wp-content/uploads/2025-1353-paper.pdf)

#### ERW-Radar: An Adaptive Detection System against Evasive Ransomware by Contextual Behavior Detection and Fine-grained Content Analysis

- **作者**: Lingbo Zhao, Yuhui Zhang, Zhilu Wang, Fengkai Yuan, Rui Hou (Institute of Information Engineering, Chinese Academy of Sciences)
- **方向**: 恶意软件与二进制分析
- **解读**: 勒索软件已造成超200亿美元损失，而攻击者正通过模仿良性程序、降低加密操作强度、部分加密或填充低熵数据等手段绕过现有基于高频I/O、固定模式和高熵值的检测。现有防御对"规避型勒索软件"效果有限。作者观察发现两个可靠信号：规避型勒索软件长期表现出独特的I/O行为重复性（因放慢加密需反复使用相同规避策略），以及加密文件与良性修改文件在χ2检验和字节概率分布上存在显著差异。
基于此，ERW-Radar包含三项机制：上下文相关机制（Correlation）结合局部行为细节与全局行为上下文分析I/O行为相关性，捕捉重复出现的相似行为段；细粒度内容分析机制（Analysis）用整体随机性与字节概率分布区分加密文件与良性修改文件；自适应机制在精度与效率间动态权衡。
实验显示ERW-Radar对规避型勒索软件检测精度达96.18%，误报率5.36%，平均CPU开销5.09%、内存开销3.80%。相比仅依赖熵值或固定模式的方法，它能识别被分散、被包装成良性行为的加密操作，并对低熵绕过具有鲁棒性。
我认为该工作的洞察在于把"长期重复性"而非"短期异常强度"作为检测信号，理论上更难被攻击者通过放慢速度或模仿良性行为规避；χ2+概率分布的组合也比单一熵值更细粒度。局限是行为相关分析需要较长时间窗口，实时性存疑；未给出对抗自适应攻击者（如刻意打乱重复结构）的评估。与ERW系列防御相比，它明确针对规避型而非普通勒索软件。

- **论文 PDF**: [ERW-Radar: An Adaptive Detection System against Evasive Ransomware by Contextual Behavior Detection and Fine-grained Content Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-349-paper.pdf)

#### Detecting Ransomware Despite I/O Overhead: A Practical Multi-Staged Approach

- **作者**: Christian van Sloun, Vincent Woeste, Konrad Wolsing, Jan Pennekamp, Klaus Wehrle (RWTH Aachen University & Fraunhofer FKIE)
- **方向**: 恶意软件与二进制分析
- **解读**: 实时I/O监控是勒索软件检测的主流方法，但此类系统需向操作系统I/O栈注入代码，此前工作几乎不考虑对系统性能的冲击，或仅评估慢速机械硬盘场景。本文首次系统分析Windows和Linux上监控不同I/O特征的开销，指出"实时行为监控可行但代价被严重低估"。
作者测量发现，即便是计算缓冲区熵这类简单特征，也可能使执行时间增加350%、SSD性能下降最高75%，直接威胁生产系统可用性。为此提出多阶段IDS：根据进程行为实时调整监控特征数量，把看似良性的进程移到特征少、开销低的阶段，把可疑进程移到特征多、开销高的阶段以确认怀疑。
结合公开数据集的真实I/O行为与逐操作开销测量，多阶段设计能把I/O操作开销降低一个数量级（order of magnitude），同时保持与传统单阶段方案相近的检测精度，使实时行为监控在真实系统中变得可行。
我认为该工作的价值在于把"性能开销"从脚注提升为一级设计约束，多阶段动态调整是工程上优雅的折中——用"先粗筛后精查"的分诊思想破解开销与精度的矛盾。局限是阶段划分策略与特征选择依赖经验，对攻击者故意模拟良性行为跨阶段逃逸的对抗性未充分讨论；与ERW-Radar等算法创新不同，它聚焦部署可行性这一常被忽视的维度。

- **论文 PDF**: [Detecting Ransomware Despite I/O Overhead: A Practical Multi-Staged Approach](https://www.ndss-symposium.org/wp-content/uploads/2025-764-paper.pdf)

#### BinEnhance: An Enhancement Framework Based on External Environment Semantics for Binary Code Search

- **作者**: Yongpan Wang, Hong Li, Xiaojie Zhu, Siyuan Li, Chaopeng Dong, Shouguo Yang, Kangyuan Qin

- **方向**: 恶意软件与二进制分析

- **解读**: 二进制代码搜索在软件复用检测、漏洞识别等应用中至关重要；现有模型要么仅基于内部代码语义，要么结合函数调用图（CG）与内部代码语义，都存在局限：内部代码语义模型只考虑函数内语义、忽略函数间语义，难以处理函数内联等情形；CG 与内部语义的组合也不足以应对复杂真实场景。本文提出 BINENHANCE 框架，利用函数间语义增强内部代码语义的表达。

方法上，BINENHANCE 构造外部环境语义图（EESG）：通过调用、位置、数据共使用等不同函数间语义关系，为同源函数建立稳定且相似的外部环境；构建后用现有内部代码语义模型生成的嵌入初始化 EESG 节点；再设计语义增强模型（SEM），用关系图卷积网络（RGCN）加残差块学习有价值的外部语义，生成增强的语义嵌入；此外利用数据特征相似度精炼语义嵌入的余弦相似度。

在六个不同任务（包括函数内联场景）上的实验验证了性能与鲁棒性：将 BinEnhance 应用于 HermesSim、Asm2vec、TREX、Gemini 与 Asteria，在两个公共数据集上平均精度（MAP）从 53.6% 提升至 69.7%，效率提升四倍。

我的思考：以"外部环境语义"补充"内部代码语义"的思路直击函数内联等单函数分析失效的场景，EESG 为同源函数构造稳定外部环境的设计使增强语义具备跨版本、跨编译的鲁棒性；作为即插即用的增强框架可叠加于现有模型之上，MAP 提升 16 个百分点且效率翻四倍，性价比突出。局限是 EESG 构建依赖函数间关系提取质量，对高度混淆或极小函数的适用性需进一步验证。与仅用 CG 的工作相比，本文把"环境"从拓扑扩展为多关系语义，是二进制代码搜索特征工程的重要推进。

- **论文 PDF**: [BinEnhance: An Enhancement Framework Based on External Environment Semantics for Binary Code Search](https://www.ndss-symposium.org/wp-content/uploads/2025-568-paper.pdf)

#### Beyond Classification: Inferring Function Names in Stripped Binaries via Domain Adapted LLMs

- **作者**: Linxi Jiang, Xin Jin, Zhiqiang Lin

- **方向**: 恶意软件与二进制分析

- **解读**: 在剥离二进制中推断函数名是恶意软件分析、漏洞发现等安全应用的重要且困难任务，需要在多样指令集、架构、编译器优化与混淆下理解二进制代码语义；机器学习虽有进展，但现有方法依赖有限词汇表的分类范式，面对未见数据时泛化能力不足。本文提出 SymGen，采用领域自适应生成式大语言模型（LLM）驱动的自回归生成范式，增强二进制代码解读。

方法上，SymGen 把函数名推断从"封闭词汇分类"重构为"开放词汇生成"：用领域自适应技术把通用生成式 LLM 适配到二进制代码语义空间，再以自回归方式直接生成函数名，从而摆脱分类器词汇表受限导致的泛化瓶颈。

在包含 2,237,915 个二进制函数、覆盖四个架构（x86-64、x86-32、ARM、MIPS）与四个优化级别（O0-O3）的数据集上，SymGen 相比最先进方法在精确率、召回率与 F1 上分别最高提升 409.3%、553.5% 与 489.4%；消融与案例研究展示了领域自适应设计的显著增益，并在混淆二进制与恶意可执行文件等真实二进制上验证了实用性。

我的思考：从分类到生成是函数名推断范式的质变——开放词汇生成天然适配无限函数名空间，数百个百分点（相对值）的提升说明分类范式的表达上限已被触达；领域自适应是弥合通用 LLM 与二进制语义鸿沟的关键工程。局限是生成式推理的计算成本显著高于分类器，超长函数与实时分析场景的可用性需权衡。与 Diaphora、Asm2vec 等传统方法相比，SymGen 展示了 LLM 在二进制逆向分析中的范式级潜力，为后续生成式逆向研究树立了标杆。
- **论文 PDF**: [Beyond Classification: Inferring Function Names in Stripped Binaries via Domain Adapted LLMs](https://www.ndss-symposium.org/wp-content/uploads/2025-797-paper.pdf)

#### Automated Mass Malware Factory: The Convergence of Piggybacking and Adversarial Example in Android Malicious Software Generation
- **作者**: Heng Li, Zhiyuan Yao, Bang Wu, Cuiying Gao, Teng Xu, Wei Yuan, Xiapu Luo（华中科技大学、香港理工大学）
- **方向**: 恶意软件与二进制分析
- **解读**:
对抗样本技术已被证明能高效攻击 Android 恶意软件检测系统，只需极少量代码修改即可规避检测；但现有对抗样本技术忽视恶意软件的生成过程，适用性受限。另一方面，piggybacked 恶意软件（把恶意代码"骑手"批量植入流行应用）可大规模生产，却缺乏对抗性改造手段。

论文将两条路线结合：给定一个恶意代码段（骑手），为其定制对抗扰动并插入任意载体应用中，使生成的恶意软件规避检测。通过剖析扰动影响 piggybacked 恶意代码的机制，提出三模块生成方法：恶意骑手提取、对抗扰动生成、良性载体选择。

大量实验表明该方法可在短时间内批量生成恶意软件并显著提高逃逸率：对机器学习检测模型（Drebin、MaMaDroid）平均攻击成功率（ASR）达 88.3%，对商业引擎 Microsoft 与 Kingsoft 分别达 76% 与 92%；论文还初步探索了潜在防御。

该工作首次把对抗样本与批量恶意软件生成流水线结合，攻击具备很强的现实性与自动化程度。局限在于对抗扰动对检测器的可迁移性依赖较大，实验所用检测模型集合有限，对新防御（如对抗训练）的有效性尚未充分验证。

- **论文 PDF**: [Automated Mass Malware Factory: The Convergence of Piggybacking and Adversarial Example in Android Malicious Software Generation](https://www.ndss-symposium.org/wp-content/uploads/2025-1933-paper.pdf)

### 漏洞挖掘与利用（14 篇）

#### VulShield: Protecting Vulnerable Code Before Deploying Patches

- **作者**: Yuan Li, Chao Zhang, Jinhao Zhu, Penghui Li, Chenyang Li, Songtao Yang, Wende Tan

- **方向**: 漏洞挖掘与利用

- **解读**: 软件漏洞不断被发现（2023 年新增 CVE 超 2.8 万），但官方补丁开发耗时，约 25% 的漏洞在披露 30 天后仍未修复，形成可被利用的攻击窗口（0-day 与 1-day 风险并存）。现有临时防护方案存在三大缺陷：保护范围有限（如 PET 仅支持内核五类漏洞）、需要修改目标程序代码、依赖 eBPF/LSM 等新特性（Linux 5.7 之后才有），难以用于仍广泛部署的旧系统。本文提出 VulShield，一个全自动、非侵入、向后兼容的临时防护系统，在补丁发布前阻止漏洞被触发或利用。

方法上，VulShield 以 sanitizer 报告为输入：先做漏洞与数据流分析（如用 SVF 框架），基于"约 70% 的内存破坏补丁依靠条件语句中的约束表达式实现防护"这一观察，用约束表达式自动生成描述漏洞触发条件的源码级策略；再借助 DWARF 调试信息把源码级策略翻译到二进制级，使无 sanitizer 的目标程序也能被保护；最后通过独立 Linux 内核模块（仅用 KProbe/Uprobe 等基础追踪机制）强制策略。执行器含三个组件：感知组件在指定地址收集运行时信息（寄存器/内存值或计数器）；决策组件按策略中的约束在决策点做逻辑判断；执行组件采取缓解动作——终止/挂起进程、返回错误处理码、Quarantine+Sweeper（延迟释放配合清扫器，应对 UAF 与 double free）、Delayed Execution until Completion（应对竞态条件）。策略经签名验证后动态下发，运行中的程序与内核可在线启用保护、无需重编译或重启。

评估中成功复现 32 个有有效 sanitizer 报告的真实漏洞（覆盖用户态程序、服务与内核），9 类漏洞全部被自动化有效缓解；对 SecretPatch 中 1,458 个漏洞的人工分析显示 VulShield 原则上支持其中 825 个（56.6%），含 26 个此前工具无法支持的其他类型。性能方面：Nginx（CVE-2013-2028）10 万请求、并发 10–10000 下每请求额外延迟不超过 0.001ms，且高并发下开销反而递减；UnixBench 峰值总开销 1.047%；内核漏洞策略生成仅需数分钟。工具已开源（github.com/vul337/VulShield）。

我的思考：VulShield 的定位精准——不是替代补丁，而是填补"披露到修复"的空窗期，且对高可用服务（Web 服务器、内核）能在线防护、无感知生效。设计上最有洞察的一点是复用 sanitizer 的检测逻辑作为策略来源：既然 ASan/UBSan 能在运行时识别触发条件，同样的条件自然可作为防护判据，这让策略生成高度自动化，也解释了为何能覆盖 9 类漏洞并扩展至 26 个其他类型。与 PET/eBPFGuard/InstaGuard/SWRR 等相比，VulShield 同时摆脱了代码修改、高级特性与硬件断点限制，普适性明显更强。局限方面：策略有效性依赖 sanitizer 报告质量（继承了其误报特性）；内核模块本身是新的信任根，策略签名机制虽防伪造，但模块漏洞本身构成额外攻击面；人工分析 SecretPatch 的可用性结论存在主观性。总体而言，这是"漏洞缓解即服务"方向的扎实系统工作，若其内核模块经过充分加固，有潜力成为 1-day 防护的实用基础设施。

- **论文 PDF**: [VulShield: Protecting Vulnerable Code Before Deploying Patches](https://www.ndss-symposium.org/wp-content/uploads/2025-298-paper.pdf)

#### TWINFUZZ: Differential Testing of Video Hardware Acceleration Stacks

- **作者**: Matteo Leonelli, Addison Crump, Meng Wang, Florian Bauckholt, Keno Hassler, Ali Abbasi, Thorsten Holz
- **方向**: 漏洞挖掘与利用
- **解读**: 视频硬件加速栈含应用、用户态库、内核驱动、固件与硬件多层，流片后不可观测、无法插桩获取覆盖率也没有正确性 oracle，难以自动化测试；编解码漏洞可致信息泄漏、拒绝服务与 RCE。

核心洞察是"间接代理"：用白盒软件解码器（FFmpeg）的代码覆盖率作为黑盒硬件栈的覆盖率代理，引导未修改的 libFuzzer 做变异模糊测试；差分 oracle 逐帧比较软件与硬件解码输出的尺寸与像素内容，任何可观测差异都提示全栈存在正确性或内存安全缺陷，无需对驱动/固件/硬件插桩。

四个平台（Intel/NVIDIA/macOS）上单独跑软件或硬件路径均无发现，组合后检出 15 簇可观测差异输入（约 12,000 用例）、5 个内存安全漏洞（含获 Intel 漏洞奖励的驱动层堆溢出）、iHD 驱动竞态；输入重放另发现 Firefox 信息泄漏与 VLC 驱动交互问题；最差情况仍覆盖 H26Forge 最优情况的 87.7%，而 H26Forge 只覆盖其不足 20%。

"软件覆盖率代理不可观测硬件+输出差分当全栈 oracle"是流片后测试缺观测性问题的高性价比解答，fuzzer 无关、可平移到加密与 AI 加速器域；但差分无法定位根因层（驱动/固件/硬件只能标"未确定"），硬件不支持时回退软件解码产生固有真阴性，与语法生成器（H26Forge）互补而非替代。
- **论文 PDF**: [TWINFUZZ: Differential Testing of Video Hardware Acceleration Stacks](https://www.ndss-symposium.org/wp-content/uploads/2025-526-paper.pdf)

#### Truman: Constructing Device Behavior Models from OS Drivers to Fuzz Virtual Devices
- **作者**: Zheyu Ma (Tsinghua University; EPFL), Qiang Liu (EPFL), Zheming Li (Tsinghua University), Tingting Yin (Zhongguancun Laboratory), Wende Tan (Tsinghua University), Chao Zhang (Tsinghua University; Zhongguancun Laboratory), Mathias Payer (EPFL)
- **方向**: 漏洞挖掘与利用
- **解读**: 问题：虚拟设备是 hypervisor 的最大攻击面，其漏洞可能导致 VM 逃逸。已有虚拟设备模糊测试不感知设备复杂行为：依赖规范手工转录（Nyx-Spec）、设备源码（ViDeZZo）、执行轨迹（MundoFuzz），或仅靠启发式与随机（Morphuzz、V-Shuttle）；且对 virtio/USB 这类总线隐藏设备缺乏状态依赖感知，难以深入其代码空间。

方法：观察到一个关键事实——OS 驱动隐含设备规范，虚拟设备与其对应驱动遵循同一规范。Truman 用流、字段、路径敏感的静态分析器从开源 Linux 驱动自动提取帧间依赖、帧内依赖与细粒度状态依赖，构建设备行为模型（DBM）；模糊引擎据此生成与变异满足依赖的虚拟设备消息，粒度从单消息级到状态级，并适配不同 hypervisor。

结果：48 小时覆盖评测中 19/29 个 QEMU 设备优于 Morphuzz、ViDeZZo 与 AFL++，virtio 设备相对 Morphuzz 覆盖提升 34%；24 小时内在 QEMU v8.0.0 发现 10 个 bug（对比工具各 4 个）；在 QEMU、VirtualBox、VMware Workstation Pro、Parallels 共发现 54 个新 bug，31 个已修复、6 个获 CVE。

我的思考：亮点是"从驱动学规范"的通用范式，绕开了闭源 hypervisor 与多种实现语言障碍，状态依赖建模填补了总线隐藏设备模糊测试的空白，是知识驱动而非启发式的重要升级。局限：静态分析依赖 Linux 驱动领域知识，非 Linux 生态设备难以覆盖；DBM 质量受驱动代码约束；结果以已知版本 bug 为主。整体代表了虚拟设备模糊测试向规范推理演进的趋势，工程完整度高。
- **论文 PDF**: [Truman: Constructing Device Behavior Models from OS Drivers to Fuzz Virtual Devices](https://www.ndss-symposium.org/wp-content/uploads/2025-301-paper.pdf)

#### Statically Discover Cross-Entry Use-After-Free Vulnerabilities in the Linux Kernel
- **作者**: Hang Zhang (Indiana University Bloomington), Jangha Kim (The Affiliated Institute of ETRI, ROK), Chuhong Yuan (Georgia Institute of Technology), Zhiyun Qian (University of California, Riverside), Taesoo Kim (Georgia Institute of Technology)
- **方向**: 漏洞挖掘与利用
- **解读**: 
Use-After-Free（UAF）是最普遍严重的内存安全问题之一；动态方法（如 fuzzing）精度高但覆盖率低，静态方法多只能发现简单顺序 UAF，而研究显示 Linux 设备驱动中超过 70% 的 Syzkaller 发现的 UAF 是跨入口的（free 与 use 位于不同入口函数）。DCUAF、Canary 等跨入口工具在精度或适用范围上受限。

UAFX 提出两项新技术：(1) 基于 escape-fetch 的跨入口别名分析——在逐入口摘要中标记对象的"逃逸/获取"，用 dummy 对象表示外部对象，按需构建跨入口别名路径；(2) 基于偏序约束的系统化验证——把锁、路径条件、线程生命周期、escape-fetch 数据流与入口内顺序统一编码为 happens-before 不等式，用 z3 求解可行性以滤除误报。

在 Linux 内核 34 个驱动模块与用户态程序 lrzip 上发现 80 个真阳性告警，审阅者感知精度 43.6%；在 23 个 Syzkaller 已知 UAF 中检出 15 个（65.2%），远超 DCUAF（30.4%）与 Canary（8.7%）；偏序过滤中位滤除 98.5% 的候选；37 个告警获开发者确认，对应 10 个独立 UAF 问题。

首次把跨入口 UAF 静态检测做到实用规模，摘要式推理加按需查询兼顾精度与效率；但主动放弃引用计数与递归结构相关候选造成漏报（历史 UAF CVE 中约 23.5% 与引用计数相关），55% 误报源于实现缺陷，与 fuzzing 等动态方法互补性强。

- **论文 PDF**: [Statically Discover Cross-Entry Use-After-Free Vulnerabilities in the Linux Kernel](https://www.ndss-symposium.org/wp-content/uploads/2025-559-paper.pdf)

#### Sheep's Clothing, Wolf's Data: Detecting Server-Induced Client Vulnerabilities in Windows Remote IPC
- **作者**: Fangming Gu, Qingli Guo, Jie Lu, Qinghe Xie, Beibei Zhao, Kangjie Lu, Hong Li, Xiaorui Gong（中科院信息工程研究所、中科院计算技术研究所、明尼苏达大学）
- **方向**: 漏洞挖掘与利用
- **解读**:
以往研究默认IPC中服务器特权更高，忽略了客户端高特权场景：如域控制器上的性能监视器从域成员取数，被攻陷的服务器可向客户端返回恶意数据突破信任边界，CVE-2024-38025因此被忽视20年。

GLEIPNIR分三阶段：静态分析识别RPC/COM/Winsock客户端与服务器并以SID/MID映射；用UI自动化、LLM（GPT-4）引导的CLI/API调用及Z3约束求解构建触发上下文；快照式模糊测试配合inline hook直接改写IPC返回值，脏页监控（超1000页）自适应终止，Intel PT采集覆盖率。

在76个客户端中触发2169次IPC调用，7天内发现25个此前未知漏洞：14个获CVE、19个获微软确认，奖金共$36,000；20个可致RCE、5个信息泄露；约束求解使触发率提升11.28%。

这是首个Windows远程IPC客户端漏洞挖掘工具，揭示“服务器不可信”新威胁模型，证明C#（Windows Admin Center）等内存安全语言程序同样可被远程数据攻破。局限：仅覆盖三种远程IPC机制，依赖既有逆向工具，快照式测试规模化成本较高。

- **论文 PDF**: [Sheep's Clothing, Wolf's Data: Detecting Server-Induced Client Vulnerabilities in Windows Remote IPC](https://www.ndss-symposium.org/wp-content/uploads/2025-1303-paper.pdf)

#### QMSan: Efficiently Detecting Uninitialized Memory Errors During Fuzzing
- **作者**: Matteo Marini, Daniele Cono D'Elia, Mathias Payer, Leonardo Querzoni (Sapienza University of Rome; EPFL)
- **方向**: 漏洞挖掘与利用
- **解读**: 未初始化内存使用（UUM）漏洞危害大且隐蔽（可能导致信息泄露、RCE、guest-to-host提权），但模糊测试中唯一的UUM消毒器MSan要求所有库全部插桩重编译，否则大量假阳性，导致OSS-Fuzz仅40.5%的项目能用MSan测试，大量软件处于UUM盲区。二进制级方案（Memcheck等）虽有兼容性但10-20x开销又不可用于fuzzing。

QMSan提出多层机会式设计：基于QEMU用户态模拟，快速机会检测器只拦截load/store操作，发现潜在UUM违规后记录并继续执行，仅对未见过的新违规调用精确检测器（完整影子传播）复核，利用fuzzing多执行特性把已验证的假阳性特征记入忽略表，从而把昂贵的精确分析限制在极少数测试用例上；同时维护全内存影子状态，顺带捕获MSan漏掉的地址性错误。

在10个无MSan支持的OSS项目和5个专有程序中发现44个新UUM漏洞（其中13个程序、4个获CVE），测试中零假阳性零假阴性；Juliet 884例测试精确检测器准确率100%；对比MSan捕获其全部5个独特崩溃并额外发现29个；开销仅为QEMU的1.51x、相对MSan开销的1.55x。

该工作首次让"现成二进制"也能做UUM感知fuzzing，机会式分层（昂贵分析只作用于少数新违规）是简洁而聪明的设计，44个新漏洞证明大量软件确实缺UUM测试；理论局限在于违规识别启发式可能引入假阴性、跳库场景存在边界情况，作者也坦诚承认。与AFL++兼容且开源，具备进入OSS-Fuzz级流水线的实用潜力。
- **论文 PDF**: [QMSan: Efficiently Detecting Uninitialized Memory Errors During Fuzzing](https://www.ndss-symposium.org/wp-content/uploads/2025-1133-paper.pdf)

#### NodeMedic-FINE: Automatic Detection and Exploit Synthesis for Node.js Vulnerabilities
- **作者**: Darion Cassel, Nuno Sabino, Min-Chien Hsu, Ruben Martins, Limin Jia
- **方向**: 漏洞挖掘与利用
- **解读**:
Node.js 生态包含数百万 npm 包，大量存在任意命令注入（ACI）与任意代码执行（ACE）漏洞。动态污点追踪类自动检测与利用合成工具成功率有限：包 API 期望的输入类型与对象结构多样，类型不对、字段缺失即漏报或利用失败；污点数据到达 sink 前还需构造语法合法的有效载荷。

在 NodeMedic 动态污点分析（provenance 图）基础上新增三组件：覆盖引导、类型与结构感知的模糊器，利用 getField 反馈重构对象字段以探索更多路径；类型与结构推断，沿 provenance 图用类型格推断输入类型并重建嵌套结构，指导 SMT 约束合成；Enumerator 用 JavaScript 语法图生成合法前缀补全，配合 polyglot 载荷与隐式强制转换建模确认 ACE 流。

在 33,011 个含 sink 调用的 npm 包上发现 2257 条潜在流、自动合成利用确认 766 条（612 ACI、154 ACE），分别为 NodeMedic 的 1.7 倍与 1.6 倍；结构推断贡献 77% 的 ACI 确认增量；Enumerator 补全 191 个前缀、贡献 27 条 ACE 流；获 1 个 CVE，54 位开发者确认漏洞真实。

该工作把"检测"推进到规模化自动确认可利用漏洞的闭环，评估规模前所未有。局限：不支持多输入同时受控（如 spawn 的 options 对象）、存在 SMT 求解超时与个别误报。对 npm 供应链安全有直接价值，也证明动态污点加约束合成的路线可行但仍有清晰边界。

- **论文 PDF**: [NodeMedic-FINE: Automatic Detection and Exploit Synthesis for Node.js Vulnerabilities](https://www.ndss-symposium.org/wp-content/uploads/2025-1636-paper.pdf)

#### Moneta: Ex-Vivo GPU Driver Fuzzing by Recalling In-Vivo Execution States
- **作者**: Joonkyo Jung, Jisoo Jang, Yongwan Jo, Jonas Vinck, Alexios Voulimeneas, Stijn Volckaert, Dokyung Song
- **方向**: 漏洞挖掘与利用
- **解读**:
GPU 驱动以内核权限运行、代码规模庞大（NVIDIA 开源模块约 92.5 万行）且攻击面宽广（ioctl、MMIO 缓冲等），是漏洞高发区。传统驱动 fuzzing 依赖物理 GPU，扩展性差；ex-vivo 的 record-and-replay 则受制于非确定性（地址随机化、驱动内部句柄变化、中断时序）与 I/O 仿真保真度不足，难以让模糊器到达深层、有状态的驱动代码路径。

Moneta 将 snapshot-and-rehost 与 record-and-replay 协同：先在带真实 GPU 直通（passthrough）的虚拟机中运行真实图形/计算负载，用 ptrace 与 hypervisor 探针自动生成整机快照及快照后的系统调用/MMIO/IRQ 记录；随后把快照重宿主到无 GPU 的 x86 服务器（含 ARM→x86 仿真重宿主，通过 CPU feature subsetting 保证可移植），保留用户态上下文与 GPU 上下文，以快照恢复的状态为起点、以快照后记录为种子语料，用 syzkaller 做带检查点重置的有状态进化式 fuzzing。

对 NVIDIA、AMD Radeon、ARM Mali 三款驱动各做 128 实例并行、24 小时 fuzzing，共发现 10 个未知 bug（NVIDIA 5、AMD 3、ARM Mali 2），5 个获 CVE；其中 4 个 bug 仅靠快照状态召回才能触发；NVIDIA 驱动基本块覆盖率较强化后的基线最高提升 137.8%；所有 bug 均在真实 GPU 上复现确认，未出现假阳性。

Moneta 的巧妙之处在于用"快照重宿主"这一确定性高、保真度好的状态召回手段弥补记录重放的非确定性问题，同时用记录提供进化式 fuzzing 的输入语料，一举缓解依赖、保真度与扩展性三大难题；ARM→x86 重宿主还让移动 GPU 驱动能在廉价的 x86 集群上大规模模糊测试。局限在于需要驱动源码插桩（KASAN/UBSAN）、MMIO/IRQ 规则仿真仍不完美、Mali 驱动需改动以适配内建 IOMMU。该工作为内核驱动 fuzzing 提供了可复制的通用范式。

- **论文 PDF**: [Moneta: Ex-Vivo GPU Driver Fuzzing by Recalling In-Vivo Execution States](https://www.ndss-symposium.org/wp-content/uploads/2025-218-paper.pdf)

#### ICSQuartz: Scan Cycle-Aware and Vendor-Agnostic Fuzzing for Industrial Control Systems
- **作者**: Corban Villa, Constantine Doumanidis, Hithem Lamri, Prashant Hari Narayan Rajput, Michail Maniatakos
- **方向**: 漏洞挖掘与利用
- **解读**: PLC 控制的工业控制系统（ICS）支撑关键基础设施，但其代码因领域专用语言（IEC 61131-3 结构化文本 ST）、闭源专有编译器与非标准二进制而难以用 IT 侧工具评估，也缺乏针对扫描周期等 ICS 特有执行模型的漏洞挖掘技术。ICSQuartz 是首个针对 ST 语言的原生模糊测试器，目标是厂商无关、平台无关。

ICSQuartz 用开源编译器在 ST 编译期进行 LLVM 插桩，使 ST 程序以标准 ELF 原生执行，彻底摆脱厂商运行时与架构依赖；并针对 ST 的扫描周期架构设计状态化变异策略，通过跨扫描周期保留并变异状态，专门挖掘周期性执行累积出的漏洞。评估涵盖 OSCAT Basic/Network 63 个真实库程序、ICSFuzz/ICSPatch 合成基准及自研 12 个扫描周期基准。

性能上 ICSQuartz 比 FieldFuzz 快 27x 以上、比 ICSFuzz 快 225x 以上（执行/秒），首崩时间平均快 1,784x 与 10,055x，且立即达到 100% 代码覆盖率。大规模真实库 fuzzing 中 25/63 个程序一小时内崩溃：MONTH_TO_STRING 因 LANG 参数缺乏非负校验导致越界读（CWE-125），可上溯 4,325,376 字节栈内存，影响 Codesys、PCWorx、Siemens 等多厂商，已分配 CVE-2024-6876 并在真实 PLC 上复现；另发现开源 ST 编译器 RuSTy 的循环条件检查缺陷。12 个扫描周期基准中，AFL++、FieldFuzz、ICSFuzz 有 7 个完全无法发现漏洞，而 ICSQuartz 全部命中。

原生 fuzzing + 厂商无关 + 扫描周期感知填补了 ICS 漏洞挖掘的关键空白，首次对开源 ST 库发起安全研究并促成真实 CVE 修复。局限：依赖开源编译器 RuSTy，其与厂商编译器行为差异可能带来语义偏差；漏洞语料规模仍小。与 ICSFuzz/FieldFuzz 的运行时模拟+网络注入路线相比，ICSQuartz 在速度、覆盖率与漏洞发现能力上形成代差。
- **论文 PDF**: [ICSQuartz: Scan Cycle-Aware and Vendor-Agnostic Fuzzing for Industrial Control Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-795-paper.pdf)

#### FUZZUER: Enabling Fuzzing of UEFI Interfaces on EDK-2

- **作者**: Connor Glosner, Aravind Machiry (Purdue University)
- **方向**: 漏洞挖掘与利用
- **解读**: UEFI固件中的漏洞可导致Bootkit等重装系统也无法清除的持久化恶意软件（如Binarly发现的LogoFail含24个严重漏洞），且EDK-2缺乏ASLR等缓解机制使漏洞易于利用。但UEFI接口函数的模块化、事件驱动和裸金属执行特性，使传统静态分析和现有SMI处理器模糊测试都难以覆盖；也没有现成技术能自动化测试UEFI接口函数。本文提出FUZZUER，首个针对EDK-2 UEFI接口的反馈引导模糊测试框架。
FUZZUER的核心是FIRNESS：用到达定值分析和值集分析识别接口函数的参数类型，找出可生成良构、有状态对象的生成器函数，据此自动产出源码级测试harness，编译为UEFI shell应用执行目标函数；并做了ASAN插桩和覆盖率反馈的工程改造，使无OS抽象的固件代码也能被有效检测。
在最新EDK-2上对150个接口函数评估：能检测66%的已知历史漏洞，平均覆盖率约50%；发现20个新安全漏洞，多数已被开发者确认。消融实验显示，相对朴素方法，分析技术使覆盖率提升30%、发现漏洞数翻倍，且显著优于手工编写harness的现有工具HBFA。
我认为填补了UEFI安全测试的空白——此前该领域只有针对SMI或图像解析的定向工作，需要大量人工工程。用静态分析自动生成harness是务实且可扩展的思路。局限是覆盖率仍偏低（约50%），且harness生成依赖类型分析质量；对硬件相关接口（依赖真实设备状态）的测试效果仍需评估。工具已开源，为固件安全研究提供了基础。

- **论文 PDF**: [FUZZUER: Enabling Fuzzing of UEFI Interfaces on EDK-2](https://www.ndss-symposium.org/wp-content/uploads/2025-400-paper.pdf)

#### DUMPLING: Fine-grained Differential JavaScript Engine Fuzzing

- **作者**: Liam Wachter (EPFL/Asymmetric Research), Julian Gremminger (EPFL), Christian Wressnegger (Karlsruhe Institute of Technology), Mathias Payer (EPFL), Flavio Toffalini (EPFL/Ruhr-Universität Bochum)
- **方向**: 漏洞挖掘与利用
- **解读**: JS引擎的JIT编译器在优化假设上的细微冲突常导致漏洞，可被利用实现任意代码执行。传统模糊测试只在崩溃或断言失败时报告bug；差分模糊测试虽可对比解释执行与JIT优化执行的差异，但现有方法通过在JS输入中插入临时探针函数读取变量值，既限制了对执行差异的检测能力，又会抑制JIT优化本身。本文提出DUMPLING，直接插桩JS引擎而非JS输入，实现细粒度的差分测试。
DUMPLING在每个执行点（包括JIT编译函数中途）高频提取完整执行状态——称为(frame) dumps，与未优化执行的对应状态做全量对比，从而捕捉解释器与优化代码之间的任何语义分歧。由于插桩在引擎内部，输入保持纯净，优化得以完整保留。
在测试充分、其他差分方法难以发现新bug的V8引擎上，DUMPLING发现8个新bug，并因此获得Google漏洞奖励计划（VRP）11000美元奖金，证明了其发现真实漏洞的能力。
我认为"插桩引擎而非输入"是差分测试的关键跃迁：既消除了探针对优化的抑制，又让状态对比覆盖到函数中途，检测粒度远超先前的ad-hoc探针方案。局限是引擎插桩工程量大、对其他JS引擎的可移植性需额外工作；8个bug规模有限但恰在V8这样被深度测试的目标上，更凸显方法价值。与Fuzzilli等覆盖引导模糊器互补，它面向"语义分歧"而非崩溃。

- **论文 PDF**: [DUMPLING: Fine-grained Differential JavaScript Engine Fuzzing](https://www.ndss-symposium.org/wp-content/uploads/2025-1411-paper.pdf)

#### Blackbox Fuzzing of Distributed Systems with Multi-Dimensional Inputs and Symmetry-Based Feedback Pruning

- **作者**: Yonghao Zou, Jia-Ju Bai, Zu-Ming Jiang, Ming Zhao, Diyu Zhou

- **方向**: 漏洞挖掘与利用

- **解读**: 分布式系统在现代计算基础设施中无处不在，但其模糊测试面临独特挑战；本文提出 DistFuzz，据作者所知首个反馈引导的黑盒分布式系统模糊测试框架。其新颖性来自分布式系统模糊测试两个关键方面的概念贡献：输入空间与反馈度量。

方法上，与先前聚焦系统性变异故障的工作不同，DistFuzz 利用分布式系统请求驱动与时序依赖的特性，提出多维输入空间：在故障之外，把常规事件与事件间的相对时序作为另外两个维度；此外，观察到分布式系统的重要状态变化可由节点间的网络消息指示，因此 DistFuzz 以带对称性剪枝的网络消息序列作为程序反馈——这背离了"有效反馈必须依赖代码插桩/分析和/或用户输入"的传统观念。

DistFuzz 在 C/C++、Go 与 Java 的十个流行分布式系统中发现 52 个真实 bug：其中 28 个获开发者确认，20 个此前未知，4 个被分配 CVE。

我的思考：DistFuzz 打破了黑盒模糊测试无法获得有效反馈的成见——网络消息序列作为天然可观测的反馈信号，配合对称性剪枝抑制状态爆炸，为无源码场景的分布式系统测试开辟了新路；52 个真实 bug（含 4 个 CVE）验证了实用性。局限是消息级反馈对无消息交互的故障模式可能不敏感，bug 确认率（28/52）也说明部分发现影响有限。与基于故障注入或插桩的分布式测试相比，DistFuzz 以"事件+时序+消息反馈"三位一体的设计，显著降低了分布式系统模糊测试的门槛。

- **论文 PDF**: [Blackbox Fuzzing of Distributed Systems with Multi-Dimensional Inputs and Symmetry-Based Feedback Pruning](https://www.ndss-symposium.org/wp-content/uploads/2025-1912-paper.pdf)

#### Be Careful of What You Embed: Demystifying OLE Vulnerabilities
- **作者**: Yunpeng Tian, Feng Dong, Haoyi Liu, Meng Xu, Zhiniang Peng, Zesen Ye, Shenghui Li, Xiapu Luo, Haoyu Wang（华中科技大学、University of Waterloo、Sangfor Technologies Inc.、香港理工大学）
- **方向**: 漏洞挖掘与利用
- **解读**:
对象链接与嵌入（OLE）规范让 Word、Excel 等应用可以互相嵌入复合文档（如在 Word 中嵌入 Excel 表格），极大便利了数据交换，但其设计天然模糊了第一方与第三方代码之间的信任边界，可能引发意外的库加载与解析漏洞。考虑到 Microsoft Office 的普及度，这类风险影响面极广，此前却缺少针对 OLE 对象的系统性安全评估工具。

论文提出 OLExplore 工具，专用于 Office OLE 对象的安全评估：作者深入考察历史 OLE 漏洞，归纳出三大类关键漏洞模式，并针对多个 Windows 操作系统版本进行动态分析与验证，把"OLE 设计缺陷"转化为可检测的具体漏洞类别。

在对不同 Windows 版本的评估中，论文共确认 26 个漏洞，其中 17 个获得 CVE 编号，且全部具备远程代码执行（RCE）潜力，说明 OLE 对象处理链上的漏洞不仅数量可观，危害等级也很高。

这是首个面向 Office OLE 对象的系统化安全评估工作，把信任边界模糊这一设计根因落地为可复现的工具与漏洞类别，对 Office 生态加固有直接价值。局限在于方法主要基于历史漏洞归纳，可能遗漏尚未被观察到的攻击面，修复建议也尚未给出完整的落地验证。

- **论文 PDF**: [Be Careful of What You Embed: Demystifying OLE Vulnerabilities](https://www.ndss-symposium.org/wp-content/uploads/2025-547-paper.pdf)

#### Automatic Library Fuzzing through API Relation Evolvement
- **作者**: Jiayi Lin, Qingyu Zhang, Junzhe Li, Chenxin Sun, Hao Zhou, Changhua Luo, Chenxiong Qian（香港大学、香港理工大学）
- **方向**: 漏洞挖掘与利用
- **解读**:
软件库是现代软件生态的基石，库内漏洞威胁巨大，而模糊测试应用于库时需精心构造既能反映多种 API 用法、又保证用法正确的驱动（driver）。已有自动库模糊测试要么因任意生成 API 序列导致大量误用误报，要么过度依赖现有代码片段、序列多样性不足而漏掉深层漏洞。

论文提出 NEXZZER：采用混合关系学习策略持续推断并演化 API 间关系，配合新型驱动架构增强库的测试覆盖并促进深层漏洞发现；同时能够自动识别并过滤绝大多数由 API 误用导致的崩溃，缓解误报问题。

在 18 个库及 Google Fuzzer Test Suite 上评估，NEXZZER 在代码覆盖与漏洞发现能力上显著优于先前工作；并在 OpenSSL、libpcre2 等已被充分测试的库中发现 27 个此前未知漏洞，其中 24 个获开发者确认、9 个因报告被修复。

NEXZZER 的价值在于同时解决了库模糊测试的两个老大难——序列多样性（关系演化）与用法正确性（误用过滤），并证明成熟库中仍有大量可发现漏洞。局限在于仍需要一定的入口 API 知识来启动关系学习，误用过滤策略可能误伤个别真实漏洞。

- **论文 PDF**: [Automatic Library Fuzzing through API Relation Evolvement](https://www.ndss-symposium.org/wp-content/uploads/2025-750-paper.pdf)

### 网络与协议安全（21 篇）

#### You Can Rand but You Can't Hide: A Holistic Security Analysis of Google Fuchsia's (and gVisor's) Network Stack

- **作者**: Inon Kaplan, Ron Even, Amit Klein

- **方向**: 网络与协议安全

- **解读**: Fuchsia 是谷歌"白纸重写"的新操作系统，已部署于数百万 Nest Hub 产品，其 TCP/IP 实现直接复用 gVisor（支撑 App Engine、Cloud Run、GKE 等云服务）的网络栈代码；而 gVisor 网络栈使用 Go 内置 PRNG 生成协议头部字段。本文首次对 Fuchsia/gVisor 网络栈做"整体性"算法安全分析，发现 TCP 初始序号（ISN）、TCP 时间戳、TCP/UDP 源端口、IPv4/IPv6 分片 ID 等字段的生成算法均存在弱点，组合利用可恢复 PRNG 种子与哈希密钥，进而实现跨站设备追踪等多种网络攻击。

方法上，作者揭示了两个根因：(1) 网络栈 PRNG 是 Go 内建 ALFG（加性滞后斐波那契生成器，607 个 63 位状态，递推 Rn=Rn−607+Rn−273 mod 2^63），启动时仅以 2^31−2 个有效种子之一播种且永不重播，种子在系统生命周期内恒定；(2) 各协议字段由 32 位密钥哈希生成（如 IPv4 ID 用截断至 11 位的 Jenkin's lookup3 与 32 位 hashIV 索引 2048 桶随机表）。攻击设计精巧：仅 4 个 TCP/IPv4 SYN 包即可经离线多表（遍历全部种子）恢复 31 位种子，IPv6 仅需 2 个 SYN 包；另有无网络包（0 UDP 包）的彩虹表法、观察 607 个连续 UDP 源端口预测下一端口、以及基于哈希碰撞（Klein/Kol 方案）提取 hashIV 并连带泄露 NAT 后的内网 IPv4 地址。

实测在 Nest Hub Max、Pixelbook Go、Intel NUC 及两台 QEMU 虚拟设备、7 类网络、IPv4/IPv6、普通/隐身模式共 63 次测试全部成功。IPv4 下种子提取驻留时间平均 7ms、计算约 3s，IPv6 下仅 2ms 驻留、0.5ms 计算；hashIV 提取驻留平均 116ms（独立攻击约 1 分钟）。种子与 hashIV 跨测试稳定且随设备不同，可组合成 63 位设备 ID（对 100 万台设备近似唯一）。PRNG 消耗实测约 1–3 万次/天。作者还论证了可预测 ISN/端口/IP ID 及对 gVisor 上 HTTP 代理发起 TCP 劫持；Google 已发布 CVE 与补丁。

我的思考：本文成色很高，是"小而致命"密码学实现的经典案例——漏洞根源不是复杂协议逻辑，而是"算法安全性"被忽视：31 位有效种子、弱 PRNG 直接生成安全敏感字段、32 位哈希密钥。整体性分析（跨字段、跨层组合）是其方法论亮点：单个弱点影响有限，组合起来才形成稳定的 63 位设备指纹。与先前仅测 PRNG 或单一字段的工作相比，本文展示了"多个脆弱点协同放大"的分析范式。局限方面：TCP 系技术对前置代理/Tor 失效；独立 hashIV 攻击会导致部分设备崩溃（Nest/QEMU 实测）；对 gVisor 的直接影响作者未实测完整攻击链。启示明确：用户态内核与云内核的网络栈同样需要密码学强度的字段随机化，可预测性即指纹即漏洞。

- **论文 PDF**: [You Can Rand but You Can't Hide: A Holistic Security Analysis of Google Fuchsia's (and gVisor's) Network Stack](https://www.ndss-symposium.org/wp-content/uploads/2025-122-paper.pdf)

#### Wallbleed: A Memory Disclosure Vulnerability in the Great Firewall of China

- **作者**: Shencha Fan, Jackson Sippe, Sakamoto San, Jade Sheffey, David Fifield, Amir Houmansadr, Elson Wedwards, Eric Wustrow

- **方向**: 网络与协议安全

- **解读**: 中国国家防火墙（GFW）的 DNS 注入子系统在网络上伪造 DNS 响应以污染被屏蔽域名的解析。本文发现其中存在缓冲区越界读漏洞 Wallbleed（致敬 Heartbleed）：针对特定构造的 DNS 查询，漏洞注入设备会在伪造的 DNS 响应中附带最多 125 字节自身内存，罕见地暴露了 GFW 内部架构与运维行为。研究自 2021 年 10 月至 2024 年 4 月进行了两年多的纵向与全网测量。

方法上，作者逆向还原了注入器解析逻辑并写出行为一致的 C 复现，系统分析了泄漏内容与受影响用户范围，并持续监测审查方的修补过程。关键技术发现：(1) 泄漏内存中可见 IP/TCP/UDP/HTTP 头与载荷、x86_64 栈帧及可执行代码；向 GFW 发送带可识别字节标记的流量后能在后续 Wallbleed 响应中找回标记，证明泄漏的是 GFW 实际经手的流量——其中仅 11% 的 TCP 段是公网到公网，多数涉及私网客户端与公网服务器，疑似 GFW 内部流量（含 UPnP 等协议）；(2) 发现新侧信道：注入的假 IP 地址池是有序循环的，每个注入进程独立地按固定顺序轮换（如 4.tt 查询可恢复 592 个假 IP 的完整有序列表），据此可区分注入节点内的多个进程，并推断其内存管理与负载均衡机制。

测量方面：2023 年 8 月的 IPv4 全网扫描（ZMap，250 Mbps，约 3 小时）得到 2.483 亿响应、来自 2.454 亿不同 IP，其中 2.42 亿受影响 IP 分布于 32 个国家/地区、381 个 AS（中国电信 1.042 亿、中国联通骨干 5,490 万）；抽样 10,000 个中国 IP 覆盖全部 22 省、5 自治区与 4 直辖市，表明漏洞影响全国；少数（0.05%）IP 位于中国境外，且存在两端都在境外的流量因途经中国边界而被注入的情形。修补时间线显示：2023 年 9–11 月间 GFW 打了不完整补丁（Wallbleed v2 仍可通过超长标签前缀触发），直至 2024 年 3 月才彻底修复。

我的思考：本文是"对抗性测量"的极端案例——通过一个内存泄漏漏洞把最隐秘的审查基础设施当作黑盒拆解，工程含量高（漏洞触发、内容分析、侧信道、全网扫描、修补追踪环环相扣）。其价值远超披露漏洞本身：首次实证了审查中间盒不仅侵犯言论自由，更因实现粗糙而威胁用户流量机密性，连境外流量也可能被波及。NDSS 2025 为其单独发布 PC 声明，凸显伦理争议：向审查系统主动注入构造流量、收集其内存内容涉及对真实系统与真实用户数据的攻击性实验，尽管作者做了匿名化与数据聚合，IRB 豁免决定的恰当性仍被程序委员会公开质疑。这使本文成为"进攻性研究伦理"的标杆案例——后续研究者在触碰关键基础设施前必须权衡：确认他人不可复现结果的价值 vs 实验本身的风险，以及伦理审查是否真的充分。技术上的启示是：任何"在途旁路设备"都应以最小权限与内存安全实现，因为它们的缺陷会以放大方式侵害海量无辜用户。

- **论文 PDF**: [Wallbleed: A Memory Disclosure Vulnerability in the Great Firewall of China](https://www.ndss-symposium.org/wp-content/uploads/2025-237-paper.pdf)

#### The Guardians of Name Street: Studying the Defensive Registration Practices of the Fortune 500
- **作者**: Boladji Vinny Adjibi, Athanasios Avgetidis, Manos Antonakakis, Michael Bailey, Fabian Monrose
- **方向**: 网络与协议安全
- **解读**: 品牌域名易被抢注（typosquatting、bitsquatting、同音/同形等），造成声誉损失与法律纠纷；过往研究对防御性注册比例结论矛盾（8% vs 1.69%），缺乏对公司规模与品牌保护服务商（OBP）策略有效性的系统测量。

作者用正交、语音、语义模型对 Fortune 500 的基础域名生成 1.46 亿个候选域名（8 类转换，并新增"股票代码抢注"一类），通过 zone 文件 + WHOIS + SEC 公司名匹配的四条件保守规则识别出 19,523 个防御性注册；用 Spearman 相关分析参与度因子；用三年 ISP 被动 DNS（约 1.09 亿条记录）评估防御注册捕获的查询流量；并用逻辑回归学习各 OBP 的注册策略、预测新客户的注册选择。

447 家公司参与防御注册，近 200 家少于 10 个；TLD 抢注占约 2/5，73.64% 的注册使用第三方 NS，MarkMonitor 与 CSC 占据主导（合计注册量是其他提供商的五倍）；防御注册平均捕获约 3/4 的潜在抢注流量（中位 78%–94%）。但测量发现大量高流量可用域名被忽视：最高一个收到 9500 万次查询、持续三年可注册（15 美元即可注册，15 个中 5 个曾被恶意使用）；回归模型达到 80% 召回只需扫描不到 10% 的候选空间（约 782 个域名、每公司约 8K 美元），MarkMonitor 的策略在 DCG 比较中表现最优。

这是迄今规模最大、维度最全的防御性注册研究，量化了"品牌保护经济学"：UDRP 仲裁最低 1500 美元且耗时，而抢先注册仅需 15 美元，据此提出 OBP 应利用被动 DNS 主动识别并注册高查询量可用域名。局限：保守规则漏掉 WHOIS 脱敏的注册，流量结论依赖单一 ISP。为品牌方、注册商与 ICANN 提供了数据驱动的改进路径。
- **论文 PDF**: [The Guardians of Name Street: Studying the Defensive Registration Practices of the Fortune 500](https://www.ndss-symposium.org/wp-content/uploads/2025-1202-paper.pdf)

#### The Discriminative Power of Cross-layer RTTs in Fingerprinting Proxy Traffic
- **作者**: Diwen Xue, Robert Stanley, Piyush Kumar, Roya Ensafi
- **方向**: 网络与协议安全
- **解读**: 全球网络审查加剧（缅甸、俄罗斯、中国不断推出新检测手段），混淆代理成为关键规避工具，双方陷入"指纹攻击—混淆升级"的军备竞赛。现有攻击都是协议特异的，只能逐个击破。本文揭示了一个协议无关的固有指纹：代理路由使传输层会话（客户端-代理）与应用层会话（端到端）终点错位，产生跨层 RTT 差异 RTTdiff。

方法上，作者在被动观测条件下用互相关（cross-correlation，滑动窗口 W=3，用拉普拉斯加权抗抖动）估计加密流中的应用层 RTT，传输层 RTT 用 SEQ/ACK 分析得到，二者之差即 RTTdiff；再用序贯假设检验（SHT）判定观测到的差异更符合直连还是代理；先验分布由三个地理分布观察点访问 top 5K 网站（解密 TLS 获取真实请求-响应对）估计，最后用 8 个地理位置测试床加 ISP 真实流量评估。

除 obfs4 外，所有测试协议（shadowsocks、VMess、VLESS、Trojan 等）结果高度一致，验证了协议无关性；per-flow 检出率约 20%，但按网站聚合后检出率超过 70%（所有客户端/代理位置一致），约 80% 的网站访问会产生至少一个可检流，半数在 60 个包内检出，FPR 控制在 0.6%–0.7%（与 GFW 部署级攻击相当）；本地 DNS 解析使 per-flow 检出率约翻倍；obfs4 的时序混淆反而恶化指纹；延迟 ACK（500ms）可减半检出，连接多路复用与流量拆分（SplitHTTP）有效但自身可能成为新指纹。

该工作提出并实证了代理流量的固有、协议无关时序指纹，审查者只需被动监控即可同时打击多协议，改变对抗格局；同时揭示混淆方案"只护包大小、不护包时序"的盲区。局限：依赖代理与服务器间的物理距离（CDN 会削弱）、假设 RESTful 请求-响应模式、真实流量 FPR 产生分类附带伤害（IMAP 端口 993 占假阳近 1/3）。缓解方案本身可被指纹化，提示需要更原理性的对策，是审查规避研究中的里程碑式警示。
- **论文 PDF**: [The Discriminative Power of Cross-layer RTTs in Fingerprinting Proxy Traffic](https://www.ndss-symposium.org/wp-content/uploads/2025-966-paper.pdf)

#### SketchFeature: High-Quality Per-Flow Feature Extractor Towards Security-Aware Data Plane
- **作者**: Sian Kim (Ewha Womans University), Seyed Mohammad Mehdi Mirnajafizadeh (Wayne State University), Bara Kim (Korea University), Rhongho Jang (Wayne State University), DaeHun Nyang (Ewha Womans University)
- **方向**: 网络与协议安全
- **解读**: 
数据平面入侵检测（IN-IDS）偏好逐流 3 阶特征（IPD/包长分布），但 ASIC 可编程交换机的硬件约束（无乘除、流水线级数少、单表单次访问）迫使现有系统采用"症状检测器"折衷（FlowLens 只测 top-K 细粒度 bin、NetWarden 全范围但粗粒度、NetBeacon 只测预测的大流），攻击者可移位分布或注入 dummy 流绕过或淹没整个检测环路。

SketchFeature 实现 HAF（高分辨率、全流、全范围）3 阶特征测量：sketch 虚拟化——各量化 bin 用不同哈希函数共享同一 Count-Min sketch 内存，均匀化负载提升内存效率；针对虚拟化引入的"幻影解码"（对未编码 bin 的负查询返回噪声）问题，用 Bloom filter 成员测试抑制，并给出幻影解码概率与误差界的理论证明。

6MB 内存下各任务（CAIDA、CSC、DDoS、botnet）的 WMRE 显著低于分区基线，接近完美测量；CSC 检测 F1 接近最优；DDoS 上比 NetBeacon 的 AUC 提升 25.6%、F1 提升 11%；NetWarden 依赖完美症状检测与无限控制平面资源的假设；已在 Tofino 商用交换机部署；DDoS/botnet 的 F1 为 0.746/0.715，比完美测量低约 13%。

首次把 sketch 从 2 阶标量特征推广到数据平面 3 阶向量特征，幻影解码的理论建模与验证方案扎实，对现有 IN-IDS 逃逸攻击面的揭示很有价值；局限是鼠流噪声、成员测试占用 2MB 内存、实验依赖特定商用交换机。

- **论文 PDF**: [SketchFeature: High-Quality Per-Flow Feature Extractor Towards Security-Aware Data Plane](https://www.ndss-symposium.org/wp-content/uploads/2025-1071-paper.pdf)

#### Securing BGP ASAP: ASPA and other Post-ROV Defenses
- **作者**: Justin Furuness, Cameron Morris, Reynaldo Morillo, Arvind Kasiliya, Bing Wang, Amir Herzberg（康涅狄格大学）
- **方向**: 网络与协议安全
- **解读**:
ROV普及使前缀/子前缀劫持失效，攻击者转向伪造源劫持、最短路径export-all、路由泄漏等post-ROV攻击，需实证评估ASPA等新防御的真实效果。

扩展BGPy模拟器（CAIDA 2024年4月拓扑，全网部署ROV，1%-99%采纳率，1000次试验），聚合7个公开ROV测量源；评估ASPA、ASPAwN、BGPsec、BGP-iSec、Path-End、OTC、EdgeFilter，并提出新攻击first-ASN-stripping。

当前ROV部署下伪造源劫持已比前缀劫持更有效（吸引约30% AS流量，且与ROV采纳率无关）；ASPA即使无tier-1采纳也有效（与文献[74][95]相反）；但ASPA对最短路径export-all攻击在约20%采纳率时被反超；ASPAwN显著保护攻击者客户锥；对意外泄漏OTC与ASPA等效；first-ASN-stripping成功率高出20%。

以真实测量驱动安全评估，纠正先前结论，推动edge/中间层AS采纳ASPA，并与IETF ASRA独立并行提出ASPAwN。局限：依赖valley-free路由假设；统一随机采纳与真实部署有差距。

- **论文 PDF**: [Securing BGP ASAP: ASPA and other Post-ROV Defenses](https://www.ndss-symposium.org/wp-content/uploads/2025-675-paper.pdf)

#### Revealing the Black Box of Device Search Engine: Scanning Assets, Strategies, and Ethical Consideration

- **作者**: Mengying Wu, Geng Hong, Jinsong Chen, Qi Liu, Shujun Tang, Youhao Li, Baojun Liu, Haixin Duan, Min Yang

- **方向**: 网络与协议安全

- **解读**: Censys、Shodan等设备搜索引擎扫描整个互联网建立设备目录，被广泛用于漏洞测绘，但其扫描资产、扫描策略以及是否遵守透明度、无害性、匿名性等伦理准则从未被系统审查；用户尝试举报扫描IP或迁移端口来规避，效果未知。

本文首创通过"镜像服务"（响应中回显请求者IP的服务，如SIP、MySQL）从引擎搜索结果反推扫描IP：2023年3月至2024年3月从Censys、Shodan、FOFA、ZoomEye收集106,132个镜像服务与1,407个扫描IP（FOFA 665个最多）；并在多国部署28个蜜罐，捕获来自839个扫描IP的740万请求（4.6GB日志），剖析扫描策略与伦理问题。

结果：FOFA约每3个月轮换扫描IP（其665个IP均被举报到AbuseIPDB，佐证规避封禁动机）；引擎不仅探测默认端口还探测邻近端口（如RDP在3388–3390），迁移端口无法藏匿服务；除Censys外各引擎均无退出选项且多数隐藏身份；四引擎发送畸形请求、尝试匿名登录与枚举数据库，暴露214,862个无认证Redis与135,599个FTP；Shodan公开68,543个Redis条目及90多万张摄像头/远程桌面截图，FOFA与ZoomEye公开145,310个Elasticsearch索引，含姓名、邮箱等PII。

我的思考：方法巧妙——利用"响应回显请求者IP"的镜像服务把不可见的扫描基础设施变成可观测目标，首次获得引擎扫描资产的直接视角；对伦理违规的证据化把"扫描是否应受规范"推向实证。局限：仅覆盖四个引擎且以IoT蜜罐为主，对引擎内部决策仍是黑盒推断；"扫描=攻击性"的框架忽略了其防御价值，伦理结论需政策层面平衡。与既往爬虫伦理测量相比，本工作在扫描方身份识别方法上前进一大步。
- **论文 PDF**: [Revealing the Black Box of Device Search Engine: Scanning Assets, Strategies, and Ethical Consideration](https://www.ndss-symposium.org/wp-content/uploads/2025-1924-paper.pdf)

#### ReDAN: An Empirical Study on Remote DoS Attacks against NAT Networks
- **作者**: Xuewei Feng, Yuxiang Yang, Qi Li, Xingxiang Zhan, Kun Sun, Ziqiang Wang, Ao Wang, Ganqiu Du, Ke Xu
- **方向**: 网络与协议安全
- **解读**:
NAT 因节省 IPv4 地址被广泛部署（超 23% 的 AS 使用），并普遍被认为提供额外安全性。本文实证表明：NAT 之外的互联网离径（off-path）攻击者可远程识别 NAT 设备并切断其内部客户端发起的 TCP 连接，即使内部客户端 TCP/IP 实现本身健壮，也会因 NAT 设备漏洞而遭受远程 DoS。

攻击分两步：其一，利用 NAT 规范对路径 MTU 发现（PMTUD）机制处理不完善造成的侧信道——NAT 生成的 ICMP 包与内部客户端维护的路径 MTU 不一致导致信息泄露，据此区分公网地址属于 NAT 设备还是独立主机；其二，向 NAT 设备发送构造的 RST 包，诱其错误删除 TCP 连接映射，而多数实现缺少对 RST 的合法性校验。

测试 8 种路由器固件与 14 个厂商的 30 台商用 NAT 设备，6 种固件与 29 台设备存在映射删除漏洞；180 个真实 NAT 网络（90 个 4G LTE/5G、60 个公共 Wi-Fi、30 个云 VPS）中 166 个（>92%）可被利用；仅 5.72 MBps 带宽即可阻断全部内部客户端的 SSH/FTP 访问。11 个月内识别 7,600+ NAT 公网地址，分布于 124 国 1,289 个 AS。

该工作揭示"NAT 即安全"的认知误区，长期、大规模的实证与负责任披露扎实可信。对策包括规范层面要求 NAT 同步路径 MTU、实现层面校验 RST 序号，原型在 OpenWrt 22.03 上验证有效。局限：攻击依赖特定网络拓扑与可达条件，RST 盲打对高熵端口/序号场景的效果待评估，修复推进依赖厂商采纳。

- **论文 PDF**: [ReDAN: An Empirical Study on Remote DoS Attacks against NAT Networks](https://www.ndss-symposium.org/wp-content/uploads/2025-972-paper.pdf)

#### ProvGuard: Detecting SDN Control Policy Manipulation via Contextual Semantics of Provenance Graphs
- **作者**: Ziwen Liu, Jian Mao, Jun Zeng, Jiawei Li, Qixiao Lin, Jiahao Liu, Jianwei Zhuge, Zhenkai Liang (Beihang University; NUS; Tsinghua University)
- **方向**: 网络与协议安全
- **解读**: SDN把控制逻辑集中到控制平面，恶意数据面设备可通过伪造或编排上报消息操纵控制器的网络视图，进而篡改/撤销转发规则（CPM攻击），造成中间人、黑洞路由、绕过访问控制等后果。现有异常检测和配置验证只基于数据面元数据，无法区分隐蔽CPM与正常行为，也依赖攻击先验知识。

ProvGuard把溯源图思想引入控制面：先通过静态程序分析识别与数据面报文处理相关的控制器操作并指导插桩，运行时捕获控制面活动构建溯源图；按操作重要性量化边的重要性以降冗余，再提取路径作为"行为上下文"捕捉长期因果特征，最后用seq2seq预测模型找出预测误差超出正常范围的偏差上下文，无需攻击模式先验知识即可报警，并提供可疑执行轨迹辅助调查。

在Floodlight控制器的仿真网络中，成功识别了此前方法无法完全处理的全部4类典型CPM攻击；通过定位可疑上下文，把人工审计工作量降到全部边的6.02%；开销方面控制器插桩使平均往返时间增加1.8%~24%，日志存储约1.3 GB/小时。

该工作把系统攻击检测中的溯源图范式迁移到SDN控制面，以"决策的因果上下文"替代静态规则，对未知攻击具备泛化潜力，是SDN安全检测的新方向；但RTT最高24%的开销与每小时GB级日志对生产环境是现实负担，在线性与检测延迟讨论不足，评估仅覆盖Floodlight与仿真网络，扩展到ONOS等控制器及真实流量的有效性尚待验证。
- **论文 PDF**: [ProvGuard: Detecting SDN Control Policy Manipulation via Contextual Semantics of Provenance Graphs](https://www.ndss-symposium.org/wp-content/uploads/2025-1244-paper.pdf)

#### Off-Path TCP Hijacking in Wi-Fi Networks: A Packet-Size Side Channel Attack
- **作者**: Ziqiang Wang, Xuewei Feng, Qi Li, Kun Sun, Yuxiang Yang, Mengyuan Li, Ganqiu Du, Ke Xu, Jianping Wu (Southeast University & Tsinghua University & George Mason University)
- **方向**: 网络与协议安全
- **解读**:
尽管WEP/WPA2/WPA3加密了Wi-Fi帧，帧大小作为可观测侧信道长期被忽视；TCP接收方对不同分组的响应（ACK、RST、SACK-ACK）长度不同且加密帧大小稳定，离路径攻击者可据此推断TCP连接状态并完成劫持。

四步攻击：ARP（或DHCP欺骗绕过AP隔离）识别受害者；伪造SYN/ACK探测触发challenge ACK（68字节帧）以检测连接并猜出端口（错则收到56字节RST）；利用SACK-ACK（80字节帧）二分搜索精确序号；再定位challenge ACK窗口、下界加2^31得到可接受确认号；最后注入RST终止连接或注入数据。

案例实验：SSH会话平均18.78秒被重置（带宽75.76KB/s，成功率84%）；Web流量操纵全程约28秒（端口10.1s、序号8.3s、确认号9.6s），成功率超70%。测试9家厂商30款路由器全部可被攻击；80个真实Wi-Fi网络（咖啡馆、书店、企业等）中75个（93.75%）被成功劫持，平均SSH DoS成功率63.73%、Web劫持51.36%；开启AP隔离的网络（11个）也能攻破。

这是设计层面而非实现缺陷：802.11固定帧长与TCP响应不一致性天然构成侧信道，影响所有Wi-Fi世代且与加密强度无关；已向Wi-Fi联盟负责任披露。缓解需修改标准（动态填充）或统一TCP响应，部署周期长。局限：无线干扰与信道竞争降低成功率；HTTPS可阻止数据注入但无法防DoS，同类帧长侧信道或可推广到其他无线链路。
- **论文 PDF**: [Off-Path TCP Hijacking in Wi-Fi Networks: A Packet-Size Side Channel Attack](https://www.ndss-symposium.org/wp-content/uploads/2025-305-paper.pdf)

#### MineShark: Cryptomining Traffic Detection at Scale
- **作者**: Shaoke Xi, Tianyi Fu, Kai Bu, Chunling Yang, Zhihua Chang, Wenzhi Chen, Zhou Ma, Chongjie Chen, Yongsheng Shen, Kui Ren
- **方向**: 网络与协议安全
- **解读**:
挖矿劫持快速增长、多国立法禁止挖矿，组织急需在网关侧及时发现挖矿。规则检测与深度包检测对新型和加密矿流失效；学习式方法虽能识别内容无关模式，但面临流量无标签、类别极不均衡、必须线速处理的难题，此前缺乏大规模在线部署验证。

MineShark 从矿池协议的重复消息规律出发：每类消息对应确定的双向包序列，大小与时延的时序模式在不同配置下一致。系统用 4×N 矩阵表示双向包大小与包间时延序列，CNN 分类，对填充、插包、分包等扰动鲁棒；GPU 可配置内存管线（DPDK 旁路、多核并行、零拷贝、双队列零丢失）保证线速；自动确认模块构建关联图，用无害访问比例滤误报、按时长/得分/并行度排序并主动探测，确认结果回流实现自改进重训。

10 Gbps 校园网关部署 10 个月：5 核 CPU+1 块 GPU 达 1.3 Mpps 吞吐、丢包 0.2%，检出 105 个矿池（平均先于商用 IDS 5 天、加密矿流提前 19 天，其中 17.6% 加密），自动过滤 99.3% 误报，71.6% 地址早于 VirusTotal 披露；基线精度 99.3%、召回 94.8%、FPR 0.3%，混淆与扰动流量下仍保持高召回。

这是首个学习式矿流检测的大规模在线部署，双队列零丢失、主动探测确认与自改进闭环是最大亮点，特征选型直面既有方法特征不稳定的缺陷。局限：排除对抗性攻击、探测有每日限额、依赖矿池协议模式。对 IDS 生态是重要补充而非替代。

- **论文 PDF**: [MineShark: Cryptomining Traffic Detection at Scale](https://www.ndss-symposium.org/wp-content/uploads/2025-402-paper.pdf)

#### LAMP: Lightweight Approaches for Latency Minimization in Mixnets with Practical Deployment Considerations
- **作者**: Mahdi Rahimi, Piyush Kumar Sharma, Claudia Diaz
- **方向**: 网络与协议安全
- **解读**: Mixnet 是抵御全局被动敌手的匿名通信系统，但隐私以延迟为代价，网页浏览等实时应用经 mixnet 几乎不可用。端到端延迟分两类：混合延迟（每跳故意引入以保证重排，与匿名性内在权衡）与传播延迟（各跳间传输开销，无直接匿名权衡）。现有 LARMix 通过节点聚类、分层、偏向路由与负载均衡降低传播延迟，但路由策略计算随网络规模指数增长（7500 节点约 3 小时），忽略客户端到 mixnet 的延迟，对 Nym 等现有部署改动过大。

LAMP 提出三种轻量路由方案：Single Circle（SC，客户端以自身为圆心按延迟界画圆选低延迟路径）、Multiple Circle（MC，逐跳以当前节点为圆心选下一跳，弥补 SC 未考虑节点间延迟的不足）、Regional Mixnet（RM，将大网拆为区域子网，客户端就近路由，免去延迟测量）。设计原则：沿用 vanilla 随机分层、不做节点聚类，不做显式负载均衡（并论证不影响网络稳定性），仅用全局网络子集计算路由策略。

使用 Nym 部署的真实延迟数据评估：SC 平均降延迟约 3 倍、熵损失约 1.5 bit；MC 降约 7.5 倍（153.4ms 降至 20ms）、熵损失 2 bit；RM（欧盟区域）降约 8 倍、熵损失 2 bit。E/L（熵/延迟比）方面 MC 与 RM 比 LARMix 好约 3 倍；计算开销 MC 为 SC 的 56 倍、RM 为 8 倍，LARMix 约为 SC 的 13900 倍。敌手控制 20% 节点时最坏全腐败路径比例（FCP）为 0.15，与 LARMix 相当。

成色在于"轻量且可部署"：放弃收益甚微但计算昂贵的聚类与负载均衡，把路由计算复杂度降到可接受范围，兼容现有 mixnet 结构。局限：匿名性仍有 1.5-2 bit 熵损失，敌手分析仅覆盖部分策略；SC/MC 依赖客户端延迟测量，RM 要求区域性子网可部署。与 LARMix 相比，LAMP 在延迟-匿名权衡与计算开销上全面占优，为 Nym 类连续 mixnet 的延迟优化提供了可落地方案。
- **论文 PDF**: [LAMP: Lightweight Approaches for Latency Minimization in Mixnets with Practical Deployment Considerations](https://www.ndss-symposium.org/wp-content/uploads/2025-1156-paper.pdf)

#### Kronos: A Secure and Generic Sharding Blockchain Consensus with Optimized Overhead
- **作者**: Yizhong Liu, Andi Liu, Yuan Lu, Zhuocheng Pan, Yinuo Li, Jianwei Liu, Song Bian, Mauro Conti
- **方向**: 网络与协议安全
- **解读**: 分片通过划分网络并行处理交易以提升扩展性，但跨片交易（网络扩至 16 片时占比超 99%）对安全与效率构成关键挑战。现有方案仅实现"弱原子性"——大多不能容忍恶意领导者与客户端，Monoxide 甚至无法处理多输入交易；Omniledger、Chainspace 等采用两阶段提交（2PC），每笔交易需输入分片执行两轮 BFT，开销加倍；跨片消息冗长、证明低效、关键消息传输在拜占庭环境下不可靠。领域缺乏兼具安全与低开销的通用跨片共识模式。

Kronos 提出由输出分片成员联合管理的"缓冲池"模式：合法交易由输入分片经一轮 BFT 把可用输入投入缓冲池，输出分片再经 BFT 将资金转给收款人，共 k 次 BFT（优于 2PC 的两轮）；无效交易在 happy path 仅需跨片多播不可用性证明、无需 BFT，unhappy path 开销也不超过 2PC。并提出基于混合树（纠删码+Merkle 树）的跨片批量认证，把 b 笔交易的证明数量降到 O(nbλ)。Kronos 不限制底层 BFT、不依赖时间假设，支持异步网络，可作为通用框架增强现有协议的扩展性。

用异步 Speeding Dumbo 与部分同步 HotStuff 实现，在 AWS 4 区域最多 1000 节点实验：峰值吞吐 320.2 ktx/sec（N=1000）；N≤1000 时延迟低于 2.03 秒；跨片交易主导时较 2PC 吞吐提升最多 12 倍、延迟减半；N=100 时吞吐 136.7 ktx/sec、延迟低于 1.13 秒。

成色在于"缓冲池"模式同时获得强原子性与最优片内开销，且协议无关、无时间假设、支持异步，通用性突出；千节点实验扎实。局限：缓冲池引入额外存储与状态管理成本，跨片占比低时相对 2PC 的优势收窄，分片重配置期间的细节未充分展开。与 Omniledger、RapidChain、Chainspace 等相比，Kronos 在恶意客户端下的原子性保证与开销权衡上更优，可作为现有 BFT 协议直接套用的扩展框架。
- **论文 PDF**: [Kronos: A Secure and Generic Sharding Blockchain Consensus with Optimized Overhead](https://www.ndss-symposium.org/wp-content/uploads/2025-472-paper.pdf)

#### Iris: Dynamic Privacy Preserving Search in Authenticated Chord Peer-to-Peer Networks
- **作者**: Angeliki Aktypi, Kasper Rasmussen
- **方向**: 网络与协议安全
- **解读**: 在 Chord 这类结构化 P2P 网络中，查找数据需经多个中间节点逐跳路由，每个中间节点都会得知被查找数据的地址——这让 Chord 不适合需要查询隐私的应用（例如 Tor 的洋葱服务目录检索、NKN 区块链基础设施）。已有匿名方案多隐藏请求者身份，但这会破坏已认证网络的长久身份。Iris 的目标是隐藏查询内容而非身份，且与现有 Chord 协议完全向后兼容，其他节点无需知道或使用它。

Iris 的核心是把真实目标替换为"替代目标"逐步逼近，让中间节点无法推断真实地址，同时保证收敛正确性。为刻画迭代查找全过程累计泄漏的信息量，作者提出受 k-匿名启发的新隐私概念 (α,δ)-privacy，并对合谋敌手给出形式化安全证明；请求者可自由选择 α、δ 来调节所需隐私等级。

在 Matlab 原型（2^23 地址空间、1000 节点、m=23 路由表）仿真中：α 主导收敛开销，α 越小收敛越快；在合谋节点比例 0%-50% 的实验中，后验/先验知识比始终不低于 α（多数运行甚至高达 0.7），验证了 (α,δ)-private 保证；非合谋中间节点观测到的目标距离呈均匀分布，除 δ 上界外几乎无有效信息。开销与所需隐私成正比——零隐私即零开销。

该工作的价值在于"查询内容隐私 + 身份认证 + 向后兼容"三者兼得，直接回应了此前方案因需改动节点组织或数据结构而未被采纳的痛点。局限：评估仅限仿真，未在真实网络中验证；(α,δ) 保证依赖攻击者模型假设，且逐跳迭代仍会逐步暴露目标所在区间。与隐藏请求者身份的匿名方案相比，Iris 更贴合 CFS、NKN、IPFS 等已认证网络的真实需求。
- **论文 PDF**: [Iris: Dynamic Privacy Preserving Search in Authenticated Chord Peer-to-Peer Networks](https://www.ndss-symposium.org/wp-content/uploads/2025-392-paper.pdf)

#### Horcrux: Synthesize, Split, Shift and Stay Alive; Preventing Channel Depletion via Universal and Enhanced Multi-hop Payments

- **作者**: Anqi Tian, Peifang Ni, Yingzi Gao, Jing Xu
- **方向**: 网络与协议安全
- **解读**: PCN 被视为解决区块链可扩展性的关键方案，但同向多跳路由的反复使用造成通道余额倾斜、单向化甚至关闭，威胁可持续性。现有再平衡方案（Revive、Thora、Shaduf）依赖循环路径、可信第三方、智能合约或永久在线，普适性与可靠性不足。Horcrux 提出无额外信任假设、免脚本、免在线的多方虚拟通道协议，从根源解决通道枯竭。

方法上，核心创新是"流中性"：中间用户在多跳支付中直接对冲相邻通道资金转移，最小化支付对余额分配的影响，消除偏斜根源。协议将通道资金冻结到三方共管账户建立虚拟通道，端到端用户离线支付，关闭时按最新状态分配回底层通道；时间费用激励 fee=(t/T)²·f 促使中间用户选择最新状态，Time Slicing 将 48 小时生命周期切为 6 片（每片 10 分钟、间隔 7 小时 50 分），中间用户仅需在线 1 小时。在 GUC 框架下给出原子性形式化证明，并分离 VC 卸载与通道关闭以抵抗多米诺攻击。

真实闪电网络数据集（10,529 节点、38,910 通道）实验显示：全流程成本不足 1 美元（Shaduf 绑定/解绑至少 6 美元、部署合约约 31 美元）；支付成功率提升 12%-30%，所需通道存款降低 70%-91%；长期运行性能提升 1.2x-1.5x；通道枯竭率几乎为零，而 Revive/Shaduf 造成数千条通道枯竭；成功率较 LN 提升 21%-47%。

我的思考：本文把"事后再平衡"升级为"事前防偏斜"，形式化证明与工程评估并重，是支付通道领域少见的完整方案。局限：评估基于仿真（余额均等、忽略手续费），VC 建立与关闭仍需上链确认，激励依赖中间用户完全理性；免脚本兼容 UTXO 链是相对 Shaduf 的落地优势。

- **论文 PDF**: [Horcrux: Synthesize, Split, Shift and Stay Alive; Preventing Channel Depletion via Universal and Enhanced Multi-hop Payments](https://www.ndss-symposium.org/wp-content/uploads/2025-495-paper.pdf)

#### Heimdall: Towards Risk-Aware Network Management Outsourcing

- **作者**: Yuejie Wang (Peking University), Qiutong Men (New York University), Yongting Chen (NYU Shanghai), Jiajin Liu (NYU Shanghai), Gengyu Chen (Carnegie Mellon University), Ying Zhang (Meta), Guyue Liu (Peking University), Vyas Sekar (Carnegie Mellon University)
- **方向**: 网络与协议安全
- **解读**: 企业越来越依赖第三方承包商或托管服务商（MSP）外包网络管理（如排障路由问题），2025年市场规模预计达3094亿美元。但第三方技术人员常被授予管理员权限，违反零信任原则，已成为客户网络事故的新来源。本文主张需要"风险感知的外包"：让客户能透明地度量和评估风险，在危害发生前做出知情决策，而非事后补救。
Heimdall构建端到端框架：先明确量化风险定义（基于配置修改对组织资产的影响），再通过细粒度风险依赖图（从配置块到转发表再到资产的依赖链）自动评估风险，最后用细粒度引用监视器（reference monitor）在运行时监控和缓解风险。依赖图通过对转发表生成的推理获得，比先前基于静态分析的方案精确得多。
专家验证显示Heimdall能以最低风险等级解决92%的实际问题，时序开销仅约7%。模拟实验表明：恶意供应商仅改1个配置块即可影响大网络最高50%的连接，8个块可影响超90%；History调试器解决50%工单的风险比Random调试器低60%；构建大型网络（如NetH）依赖图的时间低于10秒。
我认为该工作的价值在于首次把"外包网络管理风险"形式化并给出可部署的评估-监控-响应闭环，且风险定义只依赖资产价值与位置，降低了对用户专业知识的要求。局限是依赖图存在假阳性级联（一个错误接口依赖会指数级扩散），且只聚焦路由器配置修改，扩展到硬件/软件变更仍需未来工作。与MSP安全相比，它从"揭示风险"走到了"量化并控制风险"。

- **论文 PDF**: [Heimdall: Towards Risk-Aware Network Management Outsourcing](https://www.ndss-symposium.org/wp-content/uploads/2025-43-paper.pdf)

#### HADES Attack: Understanding and Evaluating Manipulation Risks of Email Blocklists

- **作者**: Ruixuan Li, Chaoyi Lu, Baojun Liu, Yunyi Zhang, Geng Hong, Haixin Duan, Yanzhong Lin, Qingfeng Pan, Min Yang, Jun Shao
- **方向**: 网络与协议安全
- **解读**: DNSBL 是过滤恶意邮件沿用二十年的有效机制，但其采纳程度、端到端运作与安全风险未知。本文利用 Coremail 15 个月 1.9 亿条退信报告（NDR）被动测量，实证 29 家 DNSBL 提供商依赖可被外部识别的"捕获服务器"（spamtrap、邮件中继、数据共享源）生成黑名单，据此提出 HADES 攻击：将非滥用邮件服务器蓄意注入 DNSBL，合法邮件遭广泛拒收。

方法上提出三种攻击变体：内部攻击（持账户直接向捕获服务器发信）、外部攻击（滥用订阅/密码重置功能诱使受害者自动发信）、伪造攻击（用伪造 SPF/DKIM 的域发信，DNSBL 不校验即收录）。spamtrap 发现按五条特征过滤域名（配置 MX、接收不存在用户邮件、收 SPF 失败邮件、不回退信、SPF 不可用），再以受控服务器发送触发邮件验证。

野外测试显示攻击成本极低：成功注入 14 家 DNSBL，最快 3 分钟（Spamhaus）、最长 24 小时，条目停留 7-30 天；仅 Spamhaus 即确认 140,449 个 spamtrap 域（含大量热门域名错拼变体）。测量显示 307,244 个域部署 DNSBL（90.06% 依赖 Spamhaus），Top 100 收件域 53% 部署；76.88% 的热门出站服务器历史上被列过黑名单；4 家注册局管理的 51 个 TLD 会删除被列域名，其中 11 个 TLD 删除率超 50%。

我的思考：本文用被动 NDR 数据完成大规模采纳测量，规避主动发送的伦理风险，并首次系统化 DNSBL 操纵风险——攻击数分钟、影响一周，成本与影响极不对称。局限：NDR 依赖对方明示名称而低估采纳；spamtrap 为特征推断。缓解建议（隐蔽捕获服务器、校验 SPF/DKIM、多源情报）务实可行，但五家确认者中仅一家表态修复，反映行业激励不足。

- **论文 PDF**: [HADES Attack: Understanding and Evaluating Manipulation Risks of Email Blocklists](https://www.ndss-symposium.org/wp-content/uploads/2025-2156-paper.pdf)

#### Eclipse Attacks on Monero's Peer-to-Peer Network

- **作者**: Ruisheng Shi, Zhiyuan Peng, Lina Lan, Yulian Ge (Beijing University of Posts and Telecommunications), Peng Liu (Penn State University), Qin Wang (CSIRO Data61), Juan Wang (Wuhan University)
- **方向**: 网络与协议安全
- **解读**: 日蚀攻击（Eclipse attack）通过垄断目标节点的全部连接将其从区块链网络中隔离，在Bitcoin和Ethereum上已被反复证实，但Monero以"入站连接默认不限量"的策略主动对抗该攻击，使既有攻击方法失效。Monero又是Dandelion++（交易隐私保护的网络层SOTA协议）的先驱部署者，其隔离可被用于交易源去匿名化。本文提出首个针对Monero的实用日蚀攻击：连接重置攻击。
核心思想是让目标节点丢弃所有良性连接并被迫重新发起出站连接选择，攻击者趁机占据连接槽。作者给出两种构造：一是利用双花交易冲突触发Monero的连接断开机制，二是利用Dandelion++下茎(stem)交易与絮(fluff)交易传播差异。攻击无需重启目标节点，显著加速并增强可控性。
在Monero主网实验：基于私有交易的连接重置攻击执行40次，35次（87.5%）完全重置目标节点全部连接，平均连接重置率达99.55%，平均完成时间39.94秒，95%攻击在2分钟内完成；基于Dandelion++的构造利用絮交易传播时间分布（44%落在2-3秒区间）实现类似效果。作者还给出缓解对策并已向Monero官方团队道德披露。
我认为该工作的关键贡献是把日蚀攻击从"占坑等待"推进到"主动重置"，攻击时间从数小时/周缩短到分钟级，且适用于所有采用Dandelion++及类似连接管理策略的区块链。局限是需持续发送双花等触发事件、攻击可被对端同步检测策略缓解；与Bitcoin侧攻击相比，其威胁模型更依赖应用层协议特性。

- **论文 PDF**: [Eclipse Attacks on Monero's Peer-to-Peer Network](https://www.ndss-symposium.org/wp-content/uploads/2025-95-paper.pdf)

#### Automatic Insecurity: Exploring Email Auto-configuration in the Wild
- **作者**: Shushang Wen, Yiming Zhang, Yuxiang Shen, Bingyu Li, Haixin Duan, Jingqiang Lin（中国科学技术大学、清华大学、北京航空航天大学、中关村实验室）
- **方向**: 网络与协议安全
- **解读**:
支持自动配置的邮件客户端会自动获取服务器配置（主机名、端口、连接类型），用户只需输入邮箱地址和密码即可登录，该机制被越来越多客户端采用；但其实现与部署层面的安全影响此前从未被系统研究，一旦被滥用可直接威胁用户凭据。

论文给出邮件自动配置安全的首次系统分析：归纳 10 个攻击场景，覆盖 17 个缺陷（其中 8 个为新发现）及 4 类不足的客户端 UI 通知；这些攻击可诱使受害者连接攻击者控制的服务器，或建立不安全连接，使凭据面临泄露风险；随后开展大规模测量与深入分析。

服务端侧发现 49,013 个域名配置错误，其中包括 19 个 Top-1K 热门域名；客户端侧 29 个客户端中有 22 个易受攻击，27 个存在至少一个可助长静默攻击的 UI 通知缺陷。缺陷根源涵盖错误配置、管理不善、实现缺陷与兼容性问题。

该工作首次把邮件自动配置安全作为独立问题进行端到端刻画，横跨客户端实现、服务端部署与用户通知三层，量化结果触目惊心。局限在于测量基于特定时点的扫描，攻击场景多为概念验证级别，真实世界被利用频率尚需后续观察。

- **论文 PDF**: [Automatic Insecurity: Exploring Email Auto-configuration in the Wild](https://www.ndss-symposium.org/wp-content/uploads/2025-1078-paper.pdf)

#### A Multifaceted Study on the Use of TLS and Auto-detect in Email Ecosystems
- **作者**: Ka Fun Tang, Che Wei Tu, Sui Ling Angela Mak, Sze Yiu Chau（香港中文大学）
- **方向**: 网络与协议安全
- **解读**:
IMAP、POP3、SMTP 等邮件协议最初按明文协议设计，缺乏内建机密性与完整性保护；TLS 可在协议开始前隐式使用，也可作为事后机会式（opportunistic）升级引入。为改善体验，许多客户端提供"auto-detect"自动检测功能确定配置参数，但 TLS 与 auto-detect 的组合使用安全此前缺乏多角度评估。

论文对邮件生态中 TLS 与 auto-detect 的使用开展多面研究：其一，测试 49 个客户端的设计与实现，发现可导致隐蔽安全降级、凭据暴露给攻击者的各类缺陷；其二，分析全球学术机构 1,102 份邮件设置指南，发现存在引导用户采用不安全设置的指导性问题；其三，评估服务端对隐式与机会式 TLS 的支持情况及证书特征。

结果表明许多用户因 TLS 与 auto-detect 的粗心处理而遭受无意的安全损失；对组织而言，更稳妥的做法是向用户提供具体、详细的手动配置指引。

该工作首次把邮件安全从单一客户端视角扩展到"客户端-设置指南-服务端部署"完整生态，对客户端厂商与服务商均有警示意义。局限在于设置指南样本集中于学术机构，客户端覆盖面有限；机会式 TLS 降级攻击在真实网络中的可操作性尚需验证。

- **论文 PDF**: [A Multifaceted Study on the Use of TLS and Auto-detect in Email Ecosystems](https://www.ndss-symposium.org/wp-content/uploads/2025-532-paper.pdf)

#### A Large-Scale Measurement Study of the PROXY Protocol and its Security Implications
- **作者**: Stijn Pletinckx, Christopher Kruegel, Giovanni Vigna（University of California, Santa Barbara）
- **方向**: 网络与协议安全
- **解读**:
反向代理对互联网服务至关重要（负载均衡、DoS 防护等），但后端会因此对客户端 IP 不可见。HTTP 可用 X-Forwarded-For 解决，其他协议缺乏等价机制；HAProxy 为此创建了 PROXY 协议，在网络栈更底层（第 4 层）把客户端信息从代理传给后端，做到协议无关。其使用与误配置的安全影响此前从未被研究。

论文首次在互联网规模研究 PROXY 协议：对完整 IPv4 地址空间测量，发现 HTTP 上超过 170,000 台主机接受来自任意来源的 PROXY 协议数据；随后演示如何利用该协议绕过路径上的代理并从后端基础设施泄露敏感信息。

研究发现超过 10,000 台服务器存在访问绕过漏洞，只需注入伪造的 PROXY 协议头即可触发；借此获得 500 多台内部服务器的访问权限，可控制 IoT 监控平台与智能家居设备（如遥控窗帘、控制摄像头与报警系统）。在 HTTP 之外，该协议还使超过 350 台 SMTP 服务器变成开放中继，可冒充任意邮箱地址发送邮件。

这是首个互联网规模的 PROXY 协议安全研究，影响横跨 HTTP、SMTP 等多个协议，误配置导致的访问绕过与开放中继后果严重。局限在于测量是特定时点快照，受影响主机集合随配置动态变化。

- **论文 PDF**: [A Large-Scale Measurement Study of the PROXY Protocol and its Security Implications](https://www.ndss-symposium.org/wp-content/uploads/2025-2247-paper.pdf)

### 密码学与协议（15 篇）

#### Siniel: Distributed Privacy-Preserving zkSNARK
- **作者**: Yunbo Yang (The State Key Laboratory of Blockchain and Data Security, Zhejiang University), Yuejia Cheng (Shanghai DeCareer Consulting Co., Ltd), Kailun Wang (Beijing Jiaotong University), Xiaoguo Li (College of Computer Science, Chongqing University), Jianfei Sun (School of Computing and Information Systems, Singapore Management University), Jiachen Shen (Shanghai Key Laboratory of Trustworthy Computing, East China Normal University), Xiaolei Dong (Shanghai Key Laboratory of Trustworthy Computing, East China Normal University), Zhenfu Cao (Shanghai Key Laboratory of Trustworthy Computing, East China Normal University), Guomin Yang (School of Computing and Information Systems, Singapore Management University), Robert H. Deng (School of Computing and Information Systems, Singapore Management University)
- **方向**: 密码学与协议
- **解读**: 
zkSNARK 证明生成计算开销大（大规模 FFT 与 MSM），阻碍隐私支付、匿名凭证等落地；私有委托（prover delegation）是主流加速途径，但 SOTA 方案 EOS（USENIX'23）要求委托者在在线阶段参与 MPC 检查（交互轮次多），zkSaaS 仅在半诚实模型下安全。

Siniel 让委托者在离线阶段用 Shamir 秘密共享分发 witness 份额、认证标签与 Beaver triple，之后完全离线；在线阶段工人在域上执行 PIOP 电路、在椭圆曲线上做 KZG 承诺，通过两个一致性检查器——witness consistency checker（随机点 α 上验证承诺与份额一致）与 PIOP consistency checker（挑战 β 加认证标签线性同态验证多项式正确性与承诺一致性）——在诚实多数（最多 t 个恶意工人）下保证恶意安全。

1 个委托者 3 个工人、SHA256 压缩函数电路下：10MBps 带宽时委托者耗时 6.5s 对 EOS 的 8.8s（节省约 16%）；1000MBps 时 0.17s 对 2.07s（节省约 80%）；委托者在线阶段零交互，网络不再成为协议瓶颈。

以"诚实多数"安全换取委托者完全离线，契合 BFT/PoS 等共识场景的威胁模型；认证标签机制简洁有效，电路模型扩展了 EOS 对群元素聚合的支持；局限是安全假设弱于 EOS 的恶意多数模型、依赖可信设置（MPC ceremony），且论文提取文本在实现部分截断，完整基准细节待查阅原文补充。

- **论文 PDF**: [Siniel: Distributed Privacy-Preserving zkSNARK](https://www.ndss-symposium.org/wp-content/uploads/2025-152-paper.pdf)

#### SHAFT: Secure, Handy, Accurate and Fast Transformer Inference
- **作者**: Andes Y. L. Kei, Sherman S. M. Chow（香港中文大学）
- **方向**: 密码学与协议
- **解读**:
Transformer私密推理的主要开销来自softmax与GELU等非线性协议：现有softmax需O(log m)轮私有最大值计算，GELU采用高次分段多项式，且多数框架对不熟悉密码学的模型拥有者不友好。

SHAFT提出首个常数轮数值稳定的softmax协议（输入裁剪到[-4,12]后ODE迭代，t=16，共41轮，绕过最大值计算）；基于δ(x)=sgn(x)(GELU(x)−ReLU(x))的傅里叶级数GELU（K=8项，19轮）；首个接受索引输入的私有embedding协议；经ONNX无缝导入Hugging Face预训练模型。

与SIGMA相比通信减少25-41%，比BumbleBee快4.6-5.3×（LAN）、2.9-4.4×（WAN），比BOLT快6.7×（LAN）并省82%通信；GELU误差较BOLT降51%/37%；GLUE上精度与明文相当（QNLI 90.38%对90.77%）。

裁剪+ODE巧妙绕开私有最大值瓶颈，常数轮是实质贡献，方案还可推广到SiLU等激活函数。局限：裁剪区间依赖经验设定，极端输入下可能失真；仅半诚实模型；相对FHE路线通信更大。

- **论文 PDF**: [SHAFT: Secure, Handy, Accurate and Fast Transformer Inference](https://www.ndss-symposium.org/wp-content/uploads/2025-2287-paper.pdf)

#### Secure Transformer Inference Made Non-interactive
- **作者**: Jiawen Zhang, Xinpeng Yang, Lipeng He, Kejia Chen, Wen-jie Lu, Yinghao Wang, Xiaoyang Hou, Jian Liu, Kui Ren, Xiaohu Yang（浙江大学、University of Waterloo）
- **方向**: 密码学与协议
- **解读**:
现有安全Transformer推理均为交互式协议，通信轮次与带宽巨大（BOLT单次推理59.61GB、10509轮，按AWS计价每token $5.44），且交互性使GPU等硬件加速失效。

NEXUS基于RNS-CKKS全同态加密实现首个非交互式协议，客户端仅1轮通信。提出SIMD密文压缩/解压、离线-在线摊销化矩阵乘、SIMD槽折叠（仅log N次旋转）与安全Argmax（仅log m+1次SGN+旋转）。

端到端带宽仅164MB：比Iron降1737.5×、比BOLT降372.5×、比Bumblebee降53.7×；CPU实现快1.79×并省98.1%通信；GPU加速42.3×，成本降至$0.05/token，BERT-base推理37.3秒；Argmax比Phoenix快55.6×（Llama-3-8B上达136.5×）；GLUE精度与明文相当。

非交互性使硬件加速成为可能，是实用化关键突破；log级Argmax解决大词汇表痛点。局限：半诚实模型；bootstrapping占CPU运行62.3%；噪声与深度管理复杂。整体是“以计算换通信”的设计。

- **论文 PDF**: [Secure Transformer Inference Made Non-interactive](https://www.ndss-symposium.org/wp-content/uploads/2025-868-paper.pdf)

#### Rondo: Scalable and Reconfiguration-Friendly Randomness Beacon

- **作者**: Xuanji Meng, Xiao Sui, Zhaoxin Yang, Kang Rong, Wenbo Xu, Shenglong Chen, Ying Yan, Sisi Duan

- **方向**: 密码学与协议

- **解读**: 分布式随机信标（DRB）为权益证明区块链、选举、异步BFT等提供不可预测、抗偏置的公共随机性。基于VSS的方案免去DKG成本且支持重配置，但现有方案要么假设同步网络（RandPiper、Optrand），要么不支持重配置（Spurt），且并行n个VSS实例导致O(n³)消息复杂度，随n增长性能急剧退化。

本文提出Rondo，部分同步模型下首个可重配置DRB，由两个构件组成：一是新原语bAVSS-PO及其协议Breeze——借鉴一致广播的通信范式，共享阶段仅O(n)消息（对VSS最优），用Bulletproofs批量多项式承诺实现一次共享B个秘密（B=O(log n)时均摊O(λn)通信），批验证计算量比hbACSS降低B倍，并修正其两处伪代码缺陷；二是动态BFT协议Rondo-BFT，可视为HotStuff的动态版本，正常路径O(n)消息、仅配置/视图切换时O(n²)，通过成员请求近乎同时达成一致、视图切换证书唯一领导者等机制解决安全与活性问题。

在Amazon EC2上最多91实例、跨四区域评估：相比现有DRB协议，Rondo吞吐量更高，性能随n增长退化更平缓，VSS不再是系统瓶颈（论文定性结论，提取文本未含具体吞吐数值）。

我的思考：bAVSS-PO放宽完备性换取O(n)消息，以2t+1签名作为验证数据补足安全性，是"以弱原语构建更强系统"的范例；Rondo-BFT对动态HotStuff的处理具有独立价值。局限：安全性依赖计算假设（数字签名、DLog、CRS+PKI），弱于无条件安全方案；每轮仍需重建一个秘密，且提取文本缺少具体吞吐量化数据，规模对比证据待核实。在"部分同步+可重配置+免DKG"组合上Rondo位置独特。

- **论文 PDF**: [Rondo: Scalable and Reconfiguration-Friendly Randomness Beacon](https://www.ndss-symposium.org/wp-content/uploads/2025-549-paper.pdf)

#### Repurposing Neural Networks for Efficient Cryptographic Computation
- **作者**: Xin Jin, Shiqing Ma, Zhiqiang Lin
- **方向**: 密码学与协议
- **解读**:
大规模数据加密对密码实现效率要求极高（如 Zoom 50 人会议每秒需加解密 4 MB 数据，即 262,144 个 AES 块），而传统密码实现速度慢、GPU 方案硬件专属且工程量大。作者利用 DNN 的图灵完备性与 AI 框架生态，提出将密码算法翻译为神经网络模型以加速密码计算的新范式。

系统 TENSOR CRYPT 定义密码学 DSL（CRYPT）与神经网络 DSL（NN），通过保持轨迹与互模拟等价（trace and bisimulation equivalence）的程序变换框架自动完成翻译，保证变换后的程序不向分析者泄露额外信息、安全强度与原算法一致；随后以算子融合与 load-on-use 内存管理两项领域优化提升效率，兼容 GPU、TPU 及移动/IoT 平台。

TensorFlow 原型中，AES、Chacha20、Salsa20 加密速度较专家手写 GPU 方案最高提升 4.09×、5.44×、5.06×；算子融合最高降低延迟 79.79×，load-on-use 内存管理最高加速 92.5×。模型已验证可运行于 Pixel XL、树莓派 3、NVIDIA GPU、Google TPU 及 Linux/Windows/macOS、Java/Python 环境。

思路极具新意——把密码计算映射到被极致优化的深度学习栈上，工程收益显著。但适用范围限于 ECB/CTR 等并行模式的对称密码，CBC、ECC 与后量子算法不在覆盖范围；变换等价性论证之外，实际部署仍需审慎评估侧信道与常数时间属性；与 AES-NI 等专用硬件在延迟敏感场景的差距也值得进一步检验。

- **论文 PDF**: [Repurposing Neural Networks for Efficient Cryptographic Computation](https://www.ndss-symposium.org/wp-content/uploads/2025-955-paper.pdf)

#### Recurrent Private Set Intersection for Unbalanced Databases with Cuckoo Hashing and Leveled FHE
- **作者**: Eduardo Chielle, Michail Maniatakos
- **方向**: 密码学与协议
- **解读**:
私有集合求交（PSI）使双方在不泄露交集以外信息的前提下计算集合交集。现有方案多针对集合规模相近、单次执行的半诚实场景；而 URL 黑名单、邮件过滤等应用需要小集合反复与同一个大集合求交（如每封邮件查一次黑名单），重复执行完整协议效率低下。

作者提出面向非平衡集合重复求交场景的 PSI 协议：基于分级全同态加密（Leveled FHE，BFV 方案，Microsoft SEAL 库）与 Cuckoo 哈希，使大集合的通信与计算成为一次性成本，后续每次求交通信量仅随小集合线性增长（与朴素不安全方案相当）；并引入多项优化（参数分析、避免资源密集型 FHE 运算）保证实时性。

实验表明：10 Gbps 与 100 Mbps 网络下分别仅需 20 ms 与 240 ms 完成求交；与 CLR、KKRT、PSSZ 等基线相比，慢速网络快一个数量级、快速网络快两个数量级。协议在半诚实威胁模型下证明安全：接收方获知交集，发送方不获得任何信息。

"重复求交"是现实中常见却被忽视的场景，切题精准，工程化程度高（C++ 实现已开源）。局限：半诚实假设与主流一致但未覆盖恶意模型；FHE 密文膨胀与一次性预计算使大集合更新成本较高；对比对象限于同期方案，与更新 OT/OPRF 类协议在恶意模型下的差距需后续验证。

- **论文 PDF**: [Recurrent Private Set Intersection for Unbalanced Databases with Cuckoo Hashing and Leveled FHE](https://www.ndss-symposium.org/wp-content/uploads/2025-365-paper.pdf)

#### PQConnect: Automated Post-Quantum End-to-End Tunnels
- **作者**: Daniel J. Bernstein, Tanja Lange, Jonathan Levin, Bo-Yin Yang (University of Illinois at Chicago; Eindhoven University of Technology; Academia Sinica)
- **方向**: 密码学与协议
- **解读**: 后量子密码部署面临巨大工程瓶颈：TLS等传输层加密需逐个应用、逐个协议集成（GitHub上2.4M个SSL相关PR、海量TLS配置CVE），TLS网页占比自2020年约80%后停滞；而"先记录、后解密"的量子威胁要求尽快部署，逐协议改造路径缓慢且劳动密集。

PQConnect在网络层做"bump in the wire"：客户端通过DNS公告自动发现服务器并建立0-RTT端到端后量子隧道，无需逐服务器配置、不改任何上层协议与应用。密钥设计分层：mceliece6960119（公钥1,047,319字节）提供长期后量子安全，sntrup761（1,158字节）提供前向保密，X25519保证即使KEM设计/实现失效仍有前量子安全兜底；瞬时密钥30秒轮换、120秒后擦除；1MB公钥用Merkle树分块分发（910叶、3-RTT），握手安全性经Tamarin符号验证（6类引理：量子机密性、量子前向保密、认证等）。

性能评估：mceliece6960119密钥生成约0.1秒（3GHz Skylake核），仅用于低频场景；服务器每30秒生成sntrup761密钥仅消耗单核百万分之一CPU周期；服务器握手解密共3次公钥运算，客户端持缓存密钥时可立即发包。与OpenSSH的Streamlined NTRU Prime、Mullvad等现有后量子VPN相比，核心差异是零配置自动发现。

该工作以"自动发现+网络层隧道"绕开应用层逐协议改造，与Let's Encrypt降低TLS门槛的思路异曲同工，直击PQ部署的瓶颈；保守双KEM+椭圆曲线混合是密码学实践智慧的典范，Tamarin证明覆盖握手组件；但1MB公钥传输对弱网和移动端开销可观，McEliece密钥管理（910叶Merkle树）复杂度高，全协议安全分析仍以人工为主；其实际效用取决于客户端/服务器安装生态，与TLS后量子路线是互补关系。
- **论文 PDF**: [PQConnect: Automated Post-Quantum End-to-End Tunnels](https://www.ndss-symposium.org/wp-content/uploads/2025-1879-paper.pdf)

#### Onion Franking: Abuse Reports for Mix-Based Private Messaging
- **作者**: Matthew Gregoire, Margaret Pierce, Saba Eskandarian (University of North Carolina at Chapel Hill)
- **方向**: 密码学与协议
- **解读**:
端到端加密即时通讯已有可验证滥用举报机制（Meta的Message Franking），但保护元数据的混合网类私密通讯尚缺高效举报方案；现有AMF、Hecate、Shared Franking要么只覆盖窄设计空间，要么计算/通信开销高出2–3个数量级。

论文提出适用于任意洋葱加密私信系统的"洋葱franking"：首个服务器兼任版主，把承诺、上下文与MAC标签组成的举报状态用PRG生成的随机掩码逐跳异或隐藏，掩码种子随洋葱密文加密传递，另加校验和σc防止恶意发送者逃避问责；还给出面向再加密混合网的优化变体，并利用零知识证明或陷阱消息实现更强的服务器问责性。

Rust原型实测：一般方案ModProcess 0.5µs、Read 1.5µs、Moderate 0.4µs，比Hecate（100.1µs/101.8µs）与AMF（225µs级）快一个数量级以上，与E2EE franking（0.8/0.4µs）相当；通信开销Send约202·N+138字节，优化变体更省。支持匿名举报与举报后匿名性。

抽象层面的贡献在于指出ccAE抽象反而妨碍匿名举报，并给出覆盖部分版主沦陷的强问责定义，可反哺已部署的E2EE平台。局限：假设服务器可用性可靠、服务器间存在TLS安全通道；陷阱消息方案仅提供1/ℓ概率问责；尚无真实部署验证。整体上填补了元数据隐藏通讯治理机制的关键空白。

- **论文 PDF**: [Onion Franking: Abuse Reports for Mix-Based Private Messaging](https://www.ndss-symposium.org/wp-content/uploads/2025-2729-paper.pdf)

#### Mysticeti: Reaching the Latency Limits with Uncertified DAGs
- **作者**: Kushal Babel, Andrey Chursin, George Danezis, Anastasios Kichidis, Lefteris Kokoris-Kogias, Arun Koshy, Alberto Sonnino, Mingwei Tian
- **方向**: 密码学与协议
- **解读**:
Sui 等区块链采用基于认证 DAG 的拜占庭共识（如 Narwhal/Bullshark），吞吐虽可达 10 万 TPS 但延迟高达 2–3 秒：块的显式认证需要多轮消息往返、按"波"成批提交使部分块等待、每块需大量签名造成 CPU 负担。而 PBFT 类协议虽有 3 条消息延迟的理论下界，但消息量大、view-change 复杂脆弱。如何在保持 DAG 高吞吐与抗审查性的同时逼近延迟下界，是本文要解决的问题。

Mysticeti-C 构造"无认证 DAG"：去掉显式证书，全网只有签名块一种消息类型。它引入 proposer slot 机制，每个槽位有 to-commit/to-skip/undecided 三态：直接决策规则观察到 2f+1 个证书模式即提交、观察到 skip 模式即跳过，使每个块都能在 3 个消息轮内独立提交；间接决策规则借助槽位全序与锚点递归决定，undecided 槽提供背压以保证异步下的安全性。Mysticeti-FPC 把快速路径（fast path）交易投票直接织入 DAG 共识块，免去逐笔交易的证书签名，并用 epoch-change 位处理跨纪元最终性。

论文证明了部分同步下的安全性与活性；Mysticeti-C 在广域网达到 0.5s 的共识提交延迟、同时保持 20 万+ TPS；接入 Sui 后，在 106 个验证者网络上延迟从 1.9s 降至 400ms（约 80% 削减、4 倍改善）；该系统管理资产超 15 亿美元、日活账户约 100 万。

该工作把 DAG 共识的理论延迟下界（3 轮）变成生产级系统，核心洞察是"每块都可独立尽早提交"以及用 undecided 状态替代缓冲轮，工程上单消息类型的设计也利于理解与维护。局限在于活性依赖超时与主提议者，且本文文本提取在评估细节处截断。作为已上线并管理巨额资产的协议，其安全性与性能经受了实际考验，是理论—系统结合的典范。

- **论文 PDF**: [Mysticeti: Reaching the Latency Limits with Uncertified DAGs](https://www.ndss-symposium.org/wp-content/uploads/2025-929-paper.pdf)

#### Manifoldchain: Maximizing Blockchain Throughput via Bandwidth-Clustered Sharding
- **作者**: Chunjiang Che, Songze Li, Xuechao Wang
- **方向**: 密码学与协议
- **解读**:
带宽是 PoW 区块链吞吐的核心瓶颈：为保安全，全网出块率被带宽最低的矿工（straggler）决定，快速矿工的带宽被浪费。现有分片协议（Elastico、OmniLedger、RapidChain、Monoxide）均采用均匀分片，每个分片仍混合快慢矿工，吞吐依旧受慢节点拖累。

Manifoldchain 提出带宽聚类分片（BCSF）：矿工把带宽、公钥、IP 打包为伪身份（PID）上凭证链，按带宽分片并为各片设置最优出块率。为对抗"按带宽聚类腐化、单片形成算力多数"的敌手，引入共享挖矿：采用 2-for-1 PoW，共识块按哈希分为 exclusive 块（只延伸本片）与 inclusive 块（同时延伸全部分片链），使全网诚实算力保护每片；配合预测挖矿与分叉剪枝、编码 Merkle 树与欺诈证明解决跨片数据可用性与有效性。跨片交易用去掉锁定阶段的异步原子提交（直接花费、失败退款、证言）。

形式化证明每分片至少一个诚实矿工时满足公共前缀、链质量与链增长，误差概率有精确上界；吞吐随分片数线性、随片内延迟倒数线性扩展；约 1.5 万行 Rust 原型在 EC2 十城实测：带宽 5–60 Mbps 下较基线分片协议吞吐提升 186%，最快分片约 5 倍、平均约 3 倍。

这是首个把带宽异构性纳入分片设计的协议，用"共享挖矿"化解聚类分片的安全隐患，且有严格的概率安全分析（优于 Monoxide 的启发式）。局限：最慢分片吞吐仅持平 Bitcoin、跨片延迟受最慢分片制约、依赖带宽估计与同步网络模型。与 GearBox 等优化正交可组合，是纵向+横向扩展的示范。
- **论文 PDF**: [Manifoldchain: Maximizing Blockchain Throughput via Bandwidth-Clustered Sharding](https://www.ndss-symposium.org/wp-content/uploads/2025-28-paper.pdf)

#### Distributed Function Secret Sharing and Applications

- **作者**: Pengzhi Xing, Hongwei Li, Hanxiao Chen, Jia Hu, Dongxiao Liu (University of Electronic Science and Technology of China), Meng Hao (Singapore Management University)
- **方向**: 密码学与协议
- **解读**: 函数秘密共享（FSS）以常数轮交互和极低在线开销成为安全计算的重要基础工具，但现有FSS方案依赖可信第三方（dealer）在离线阶段生成密钥：现实中难以找到双方共同信任的dealer（如军事实体间的卫星碰撞预测），且dealer被腐化会泄露诚实方全部隐私。本文提出分布式（无dealer）密钥生成方案，移除可信第三方假设。
作者为分布式点函数（DPF）设计了轻量级约束比较协议，仅用一次AND协议调用即支持算术共享输出；为分布式比较函数（DCF）构造了基于新相关校正词生成协议的2PC方案，减少底层多路复用器调用次数，并解决此前方案受阻的溢出问题以支持算术共享输入。在此之上构建等值测试、比较、截断、数字分解、区间包含等基础构件，并提出利用三角函数周期性的求值框架，压缩输入比特长度以缓解FSS协议的性能瓶颈。
在真实应用上的广泛实验显示，相比SOTA无dealer方案，延迟最高降低14.73倍，通信开销降低27.67至184.42倍。
我认为该工作把FSS从"需要dealer的玩具"推进到"两方可直接部署"的实用工具，对安全机器学习推理等场景意义重大；算术共享输入/输出支持填补了Floram、Half-tree的空白。局限是协议的安全模型与具体通信复杂度需结合应用验证，三角函数框架仅覆盖特定函数族；与dealer方案相比，离线阶段计算成本更高是固有代价。

- **论文 PDF**: [Distributed Function Secret Sharing and Applications](https://www.ndss-symposium.org/wp-content/uploads/2025-2233-paper.pdf)

#### DiStefano: Decentralized Infrastructure for Sharing Trusted Encrypted Facts and Nothing More

- **作者**: Sofia Celi (Brave Software), Alex Davidson (NOVA LINCS & Universidade NOVA de Lisboa), Hamed Haddadi (Imperial College London & Brave Software), Gonçalo Pestana (Hashmatter), Joe Rowell (Royal Holloway, University of London)
- **方向**: 密码学与协议
- **解读**: 网上存在大量需要向第三方证明的敏感事实（年龄、社保状态、购买记录），但信息位于TLS加密信道内。指定提交TLS（DCTLS）协议通过秘密共享会话密钥、在2PC中完成握手与记录层，向指定验证者导出可信事实证明。但现有DCTLS（DECO、TLSNotary、TownCrier等）对TLS 1.3支持缺失或安全分析不足（DECO所用原语已被证明不安全）、泄露客户端访问的服务器泄露浏览历史、且缺乏与浏览器互操作的开源实现。本文提出DiStefano，恶意安全、模块化且面向TLS 1.3的DCTLS协议。
DiStefano用新颖的独立安全模型证明恶意敌手下的安全性，模型允许按密码套件灵活替换方案（密码敏捷性）；客户端隐私通过零知识签名证明实现——在N个验证者批准的TLS服务器间进行零知识认证，验证者无法得知具体访问了哪个服务器；2PC由emp提供，集成进Chromium系浏览器使用的BoringSSL库并开源。
在线阶段完整执行<1秒且带宽≤80KiB：LAN下握手约500ms、记录层约750ms，2KiB TLS通信仅需5KiB与4KiB带宽；WAN下时序小幅增加，远低于标准TLS握手10-20秒的超时限制。
我认为DiStefano的贡献在于工程与理论的结合：首次给出TLS 1.3下可证明安全的DCTLS，密码敏捷性设计回应了原语失效风险，浏览历史隐私回应了DCTLS最受诟病的隐私缺陷。局限是零知识认证需预先批准的服务器集合，适用场景受限；与DECO相比它不覆盖任意服务器场景，但换取了更强的隐私与可证明安全。

- **论文 PDF**: [DiStefano: Decentralized Infrastructure for Sharing Trusted Encrypted Facts and Nothing More](https://www.ndss-symposium.org/wp-content/uploads/2025-1367-paper.pdf)

#### BumbleBee: Secure Two-party Inference Framework for Large Transformers

- **作者**: Wen-jie Lu, Zhicong Huang, Zhen Gu, Jingyu Li, Jian Liu, Cheng Hong, Kui Ren, Tao Wei, WenGuang Chen

- **方向**: 密码学与协议

- **解读**: 大型 Transformer 模型在自然语言处理与计算机视觉等真实任务上取得最先进性能，但其处理的数据与任务日益敏感，部署时的隐私成为主要关切。本文聚焦两方私有推理：一方持有私有输入、另一方持有模型，提出 BumbleBee，一个快速且通信友好的两方私有 Transformer 推理系统。

方法上，BumbleBee 有三方面贡献：一是提出优化的矩阵乘法协议，相比此前技术通信成本降低 80%–90%；二是发展了一套为 Transformer 非线性激活函数定制高效协议的方法论，激活函数协议在显著提升处理速度的同时，通信成本比两种先前方法降低 80%–95%；三是在五个 Transformer 模型上进行了广泛基准测试。

结果显示，BumbleBee 能评估 LLaMA-7B 模型，在 CPU 上约 8 分钟生成一个 token；性能比 Iron（NeurIPS22）高一个数量级以上，比 BOLT（Oakland24）快 3 倍且通信量仅为其十分之一。

我的思考：BumbleBee 面向大模型私有推理做了系统级优化，矩阵乘法与激活函数两条线同时压缩通信，直击两方安全推理的通信瓶颈；LLaMA-7B 在纯 CPU 上可运行的示范意义重大，与 Iron、BOLT 的数量级对比验证了优化效果。局限是 8 分钟/token 的吞吐仍难支撑交互式应用，GPU/TPU 加速、批处理与内存优化的空间需结合全文评估。与同领域 MPC 推理框架相比，BumbleBee 展示了"面向 Transformer 结构定制协议"这一方法论的价值，是隐私推理走向大模型实用化的重要一步。

- **论文 PDF**: [BumbleBee: Secure Two-party Inference Framework for Large Transformers](https://www.ndss-symposium.org/wp-content/uploads/2025-57-paper.pdf)

#### Alba: The Dawn of Scalable Bridges for Blockchains
- **作者**: Giulia Scaffino, Lukas Aumayr, Mahsa Bastankhah, Zeta Avarikioti, Matteo Maffei（TU Wien、Princeton University）
- **方向**: 密码学与协议
- **解读**:
加密货币生态繁荣催生了跨链桥（bridge）以支持资产跨链转移，但现有无信任桥协议要么传递过多信息（轻客户端型桥），要么计算昂贵（zk 型桥），效率低下。根本原因在于现有桥都要证明交易在另一条链上的链上包含性——而链下方案（如支付通道、状态通道）允许不经链上发布的安全交易，现有桥却无法验证链下支付。

论文提出 Pay2Chain 桥概念并给出具体设计 Alba：利用支付通道等链下方案的优点突破现有桥的局限，基于链下事件在目标链上高效、安全、无信任地执行条件支付或智能合约。Alba 在 UC 框架下对 Byzantine 对手形式化证明安全性，并辅以博弈论分析，还提出形式化可扩展性度量。

经验评估显示 Alba 在通信复杂度与链上成本方面高效：乐观情形下开销仅为标准以太坊代币所有权转移交易的两倍。

Alba 的概念贡献在于把"桥"的验证对象从链上包含性扩展到链下事件，解决了桥设计的效率根源，形式化证明与博弈论分析的组合也提升了可信度。局限在于其运行依赖链下基础设施（支付通道）的可用性与参与方流动性，对通道网络尚不发达的链生态适用性有限。

- **论文 PDF**: [Alba: The Dawn of Scalable Bridges for Blockchains](https://www.ndss-symposium.org/wp-content/uploads/2025-1286-paper.pdf)

#### A New PPML Paradigm for Quantized Models
- **作者**: Tianpei Lu, Bingsheng Zhang, Xiaoyuan Zhang, Kui Ren（浙江大学区块链与数据安全全国重点实验室）
- **方向**: 密码学与协议
- **解读**:
模型量化是提升机器学习效率、降低计算与通信开销的常见手段，但在隐私保护机器学习（PPML）中，量化算子内部结构复杂，导致其在现有 PPML 框架下的协议效率低下，量化带来的收益难以兑现。

论文提出专为量化模型设计、并能从量化中获益的新 PPML 范式。核心观察是：查找表可以无视任意函数的复杂内部结构。作者把模型推理视为量化算子的序列，每个算子用一个查找表实现，并开发了高效的私有查找表求值协议，其在线通信成本仅为 log n（n 为表大小）。

单 CPU 核上该协议每秒可求值 2^26 个 8 位输入、8 位输出的查找表。由此构建的量化模型 PPML 框架在线性能极快：与 SOTA 方案相比，CNN 模型（AlexNet、VGG16、ResNet18）在线性能提升约 40-60 倍，大语言模型（GPT-2、GPT-Neo、Llama2）提升约 10-25 倍。

"用查表绕过量化算子内部结构"的观察简洁而优雅，log n 的在线通信与数十倍实测加速使其成为 PPML 效率研究的重要范式突破。局限在于离线预处理成本与表大小随输入位宽指数增长，量化精度损失在低比特设置下需评估。

- **论文 PDF**: [A New PPML Paradigm for Quantized Models](https://www.ndss-symposium.org/wp-content/uploads/2025-2872-paper.pdf)

### 隐私保护（21 篇）

#### Transparency or Information Overload? Evaluating Users’ Comprehension and Perceptions of the iOS App Privacy Report
- **作者**: Xiaoyuan Wu (Carnegie Mellon University), Lydia Hu (Carnegie Mellon University), Eric Zeng (Carnegie Mellon University), Hana Habib (Carnegie Mellon University), Lujo Bauer (Carnegie Mellon University)
- **方向**: 隐私保护
- **解读**: 问题：苹果 2021 年发布的 App Privacy Report 旨在让用户了解 app 对数据/传感器的访问，以及 app 和网站联系的域名——其网络活动粒度（具体域名列表）远超其他隐私仪表盘。需要实证评估它是否真的提升用户理解、影响隐私行为。

方法：通过 Prolific 招募 20 名 iPhone 用户进行约一小时的半结构化访谈（另有 190 人筛查调查、访谈后一月 14 人随访调查），让用户边思考边说地自由探索四个板块（数据与传感器访问、App 网络活动、Website 网络活动、最常联系域名），用主题编码分析理解、态度与行为意图，两名研究者独立编码以保证信度。

结果：用户能轻松理解传感器访问并定位到权限设置（17 人识别出过度授权，如 Health 应用访问通讯录）；但 20 人全部对网络活动困惑或误解——报告平均含 1,763 个唯一域名造成信息过载，用户搞不清"联系域名"的含义与目的，14 人提出的补救措施（删 app、屏蔽域名等）多不可行，12 人无奈接受；随访中仅少数人（8/14）会再次查看。

我的思考：论文揭示了"透明而不理解"的悖论——信息粒度越细未必越好，缺乏解释与聚合的原始域名列表反而制造焦虑与无助，方法规范（双人编码、主题在第 12 次访谈后饱和）。局限：样本量 20、均为 Prolific 美国用户、推广性有限，且未设客观理解度量。对隐私仪表盘设计有直接价值：应提供域名的目的、所有者等解释性聚合信息，这一洞见对数据透明度立法同样适用。
- **论文 PDF**: [Transparency or Information Overload? Evaluating Users’ Comprehension and Perceptions of the iOS App Privacy Report](https://www.ndss-symposium.org/wp-content/uploads/2025-81-paper.pdf)

#### SKILLPoV: Towards Accessible and Effective Privacy Notice for Amazon Alexa Skills
- **作者**: Jingwen Yan (Clemson University), Song Liao (Texas Tech University), Mohammed Aldeen (Clemson University), Luyi Xing (Indiana University Bloomington), Danfeng (Daphne) Yao (Virginia Tech), Long Cheng (Clemson University)
- **方向**: 隐私保护
- **解读**: 
Alexa 技能（skills）会收集用户数据，但隐私政策冗长难懂且只能经网页或手机 App 访问，语音界面受限，尤其不利于视障用户做出知情隐私决策；现有隐私政策还常与实际数据收集行为不符。

提出"语音隐私通知"（Privacy Notice over Voice）机制：技能启动时以语音播放简短易懂的通知，并给用户继续或退出选项。先对 52 名智能音箱用户与 21 名技能开发者调研确定设计偏好；再实现工具 SKILLPoV：静态分析（NLP 检测对话中的"your+敏感数据"收集、manifest 权限解析、LLM 检测数据共享/存储）→ 用 GPT-4 结构化提示生成简洁通知 → 插桩新 intent/handler 到前端与后端代码而不改变原功能。

92.3% 用户喜欢该设计，70.2% 认为可读性与可访问性优于传统隐私政策；SKILLPoV 在 143 个开源技能上全部成功生成并插桩，10 个可执行技能功能无损；数据收集识别精度 91.3%（原政策仅 35.3%）、完整性 96.4%（50%）；通知仅 7–25 词（字数平均降 93.2%），Flesch 可读性 68.4 对 46.2。

把隐私通知从合规文档转化为对话交互，兼顾无障碍需求与 GDPR 第 13 条等法规要求，工具化显著降低开发者负担；局限是恶意开发者可绕过、仅支持 JavaScript 后端、NLP 规则存在误报。对语音助手生态的隐私透明度研究具有直接实践价值。

- **论文 PDF**: [SKILLPoV: Towards Accessible and Effective Privacy Notice for Amazon Alexa Skills](https://www.ndss-symposium.org/wp-content/uploads/2025-677-paper.pdf)

#### SIGuard: Guarding Secure Inference with Post Data Privacy
- **作者**: Xinqian Wang, Xiaoning Liu, Shangqi Lai, Xun Yi, Xingliang Yuan（RMIT University, CSIRO's Data61, The University of Melbourne）
- **方向**: 隐私保护
- **解读**:
MPC安全推理只保护输入隐私，不保护输出隐私：用户拿到推理结果后仍可发起成员推断攻击（MIA）。论文首次系统评估安全推理场景下的MIA威胁，指出softmax近似产生的扰动预测并不能阻止攻击。

SIGuard将明文防御MemGuard移植到2-out-of-3复制秘密共享上，设计安全噪声优化与校验协议，在加密域内为置信向量注入噪声；针对“成员与非成员数据优化迭代次数分布不同”的泄露，提出固定迭代与超参调优（迭代从300降为10）。

在4种安全推理协议、5种MIA、5个数据集上，攻击精度最高达99%（Location30）；SIGuard将CIFAR-10上NN-M攻击精度从57.60%降至50.30%，逼近随机猜测，推理精度零损失，开销约1.1秒（约占3.29秒安全推理的24.8%）。

这是首个面向安全推理阶段的MIA防御，可即插即用且不损失精度，优于训练阶段防御（HAMP损失3.5%精度）。局限是防御依赖模拟成员分类器，对自适应MIA的鲁棒性存疑；固定迭代是效率与效果的折中，合谋威胁模型可行性待观察。

- **论文 PDF**: [SIGuard: Guarding Secure Inference with Post Data Privacy](https://www.ndss-symposium.org/wp-content/uploads/2025-168-paper.pdf)

#### Ring of Gyges: Accountable Anonymous Broadcast via Secret-Shared Shuffle

- **作者**: Wentao Dong, Peipei Jiang, Huayi Duan, Cong Wang, Lingchen Zhao, Qian Wang

- **方向**: 隐私保护

- **解读**: 匿名广播系统允许用户匿名发帖，基于MPC的新设计（Clarion、RPM）计算效率有竞争力，但服务器间通信开销高，且无法应对滥用者反复发布不当、非法内容，匿名与问责的张力阻碍实际部署。

本文提出Gyges，基于诚实多数四方秘密共享中继的匿名广播系统。离线阶段中继方预计算消息无关的关联随机性；在线阶段利用从Z₂^{N×N}采样的布尔置换关联（BPC，保持稀疏性）把洗牌化为共享矩阵乘法、把追踪化为共享重建问题，实现"静默洗牌"——在线零服务器间通信；追踪协议在内容严重违规时能把特定消息重新关联到发送者而不危及他人匿名；并基于私有鲁棒性定制协议，在恶意干扰下保证输出交付且不泄露发送者身份，支持GPU纵向与服务器横向扩展。

混合10^5条消息（每条32B至1KB）时，端到端延迟比Clarion低1.41×–2.32×，在线通信成本显著下降；共享追踪比可追踪mixnet（PETS'24）快数个数量级；在线交互轮数为0（Clarion恶意设置6轮）。

我的思考："静默洗牌+按需追踪"的对称设计（置换矩阵列作洗牌关联、行作追踪关联）把匿名与问责统一进同一秘密共享框架，思路优雅且工程导向强。局限：四方诚实多数且不共谋是部署信任前提；追踪能力是把双刃剑——中继方若被攻破或胁迫可追溯任何消息，匿名退化为信任假设（论文承认是有意取舍）；追踪触发依赖外部内容审核。相比RPM（O(N²ℓ)通信）与Ruffle（需向单一非恶意方泄露秘密），Gyges在通信效率与匿名保持上明显更优。

- **论文 PDF**: [Ring of Gyges: Accountable Anonymous Broadcast via Secret-Shared Shuffle](https://www.ndss-symposium.org/wp-content/uploads/2025-268-paper.pdf)

#### Revisiting EM-based Estimation for Locally Differentially Private Protocols

- **作者**: Yutong Ye, Tianhao Wang, Min Zhang, Dengguo Feng

- **方向**: 隐私保护

- **解读**: 本地差分隐私（LDP）是苹果、谷歌等广泛部署的数据收集标准，其聚合端核心问题是分布估计。无偏估计法在LDP噪声过大时给出不合理结果（负值、估计和≠用户总数）；MLE/EM法结果合理但会过拟合噪声数据，尤其待估参数多时（大输入域、低ε、用户少）。

本文把高斯混合模型中的reduction（归约）思想引入LDP估计：将估计问题转化为混合模型密度估计，在EM迭代中反复合并权重最小的分量，减少参数数量、抑制过拟合，使EM收敛更快、估计更稳健。框架通用，可作为后处理步骤适配GRR/OLH频率估计、PM/SR均值估计、Laplace/PM/SW数值分布、PCKV键值条件估计，并给出均方误差的理论分析，证明输入域大或噪声大时归约更优。

结果：均值估计上框架应用于PM后，在数据少、预算低场景下MAE最多降低70%；频率估计在ε=0.5时相对提升10%–30%；数值分布估计比标准EM误差降低30%；PCKV条件均值估计在ε=1时MSE改善40%；归约还显著缩短运行时间。

我的思考：把机器学习中成熟的参数归约思想移植到LDP统计估计，精准对症EM过拟合瓶颈，且以即插即用的后处理形式工作，工程价值高，理论与实验一致。局限：归约隐式假设真实分布可由少量分量支撑——分布高度分散或均匀时过度合并会引入偏差；ε充足、数据充分时相对无偏估计优势有限；对轨迹、图等复杂任务的扩展未充分验证。与Li等的平滑策略相比，reduction不引入额外平滑参数、可覆盖类别估计，是对EM估计路线的实质性推进。

- **论文 PDF**: [Revisiting EM-based Estimation for Locally Differentially Private Protocols](https://www.ndss-symposium.org/wp-content/uploads/2025-521-paper.pdf)

#### RAIFLE: Reconstruction Attacks on Interaction-based Federated Learning with Adversarial Data Manipulation
- **作者**: Dzung Pham, Shreyas Kulkarni, Amir Houmansadr
- **方向**: 隐私保护
- **解读**:
交互式联邦学习（IFL，如联邦推荐系统与在线学习排序）中，用户数据仅是与服务器提供物品的交互，常被视为隐私友好方案。但服务器掌握并控制物品训练特征这一独特性长期被忽视，用户交互面临被中央服务器重构的显著风险。

作者提出 RAIFLE，一种基于优化的主动重构攻击框架：核心是"对抗性数据操纵"（ADM）——服务器主动操纵物品特征使本地梯度携带更多交互信息，含两种技术：fingerprint（选择性保留或清零特征以控制梯度）与噪声注入（以随机噪声替换特征）；攻击者通过最小化模拟本地更新与实际本地更新之间的距离来重构用户交互。

实验表明：在联邦推荐与在线学习排序场景的多数设置下，RAIFLE 显著强于梯度反演（gradient inversion）等被动攻击，且能削弱安全聚合（secure aggregation）与 PIR 等隐私增强机制；即使服务器只能间接影响特征（用户自行提取特征时），攻击依然有效。代码已开源。

该工作揭示了 IFL 中"服务器控制物品"这一被忽视的攻击面，与模型操纵攻击一脉相承但更贴合推荐/排序实际，并系统讨论了 LDP、安全聚合、数据验证、个性化等对策的利弊。局限：攻击效果依赖物品特征维度与模型结构，高维复杂特征下的重构保真度待评估；主动操纵特征可能被检测，防御侧仍缺乏系统性方案。
- **论文 PDF**: [RAIFLE: Reconstruction Attacks on Interaction-based Federated Learning with Adversarial Data Manipulation](https://www.ndss-symposium.org/wp-content/uploads/2025-363-paper.pdf)

#### RadSee: See Your Handwriting Through Walls Using FMCW Radar
- **作者**: Shichen Zhang, Qijun Wang, Maolin Gan, Zhichao Cao, Huacheng Zeng (Michigan State University)
- **方向**: 隐私保护
- **解读**: 现有射频人体活动识别多面向走步、手势等大幅动作，而"穿墙读取笔迹"同时面临两个硬约束：毫米波信号无法穿墙，低频信号分辨率又不足以感知毫米级书写动作；Wi-Fi CSI是非相干检测，受相位/频率/时间失准限制也无法捕捉毫米级位移。该任务关乎机密书写内容的泄露，威胁评估价值明显。

方法上采用软硬件联合设计：硬件自制6GHz FMCW雷达，配总增益36 dBi的高增益贴片天线，仅20 dBm发射功率即可穿墙；软件侧利用解调信号相位特征（理论上1 mm手部位移对应约14°相位变化，远高于幅度灵敏度），以带注意力机制的BiLSTM对62个字符（a-z、A-Z、0-9）分类，并用Range-FFT bin选择与天线方向性抑制多径和移动物体干扰。

原型实验显示：放在办公室内墙与木质/乙烯外墙后，随机书写62字母时字母识别准确率75%，写文章时单词识别准确率87%；iPad书写74%、Post-it便签71%；3 mm小字仍有68%准确率；距离1-3 m稳健，天线指向偏差30°时降到55%；对走动人员与带内Wi-Fi干扰具韧性。kNN特征验证显示跨用户准确率仅53%-77%。

该工作首次证明毫米级穿墙书写感知的可行性，把"隔着墙读笔迹"从想象变为可量化威胁，但75%的字母准确率离实用窃取仍有距离，混淆集中在形近字符（O/o、C/O、I/1），且训练需18人6.7万样本，个体差异大、迁移性存疑；相比毫米波方案其优势是穿墙，相比Wi-Fi方案优势是相干检测，属于权衡折中。后续反制研究（屏蔽、信号设计、检测）值得跟进。
- **论文 PDF**: [RadSee: See Your Handwriting Through Walls Using FMCW Radar](https://www.ndss-symposium.org/wp-content/uploads/2025-s322-paper.pdf)

#### Privacy-Preserving Data Deduplication for Enhancing Federated Learning of Language Models
- **作者**: Aydin Abadi (Newcastle University), Vishnu Asutosh Dasu (Pennsylvania State University), Sumanta Sarkar (University of Warwick)
- **方向**: 隐私保护
- **解读**: 重复数据会损害语言模型性能并加剧记忆化，而记忆化正是成员推理、数据提取攻击与版权侵权的根源（C4语料中某61词序列重复61,036次，去重可降10x记忆化并改善困惑度）；联邦学习（FL）中跨客户端去重可显著提效，但共享数据去重会泄露隐私，朴素多方PSI又无法删除子集间的重复。

论文提出EP-MPD协议：模块化构造基于两个新颖的私密集合交集变体——Group PSI（G-PSI）抽象，让成对/分组客户端在不泄露各自数据的前提下协商删除重复项；并给出两种高效实现EG-PSI(I)（对称密钥基）与EG-PSI(II)（公钥基），在计算效率与客户端负载间提供取舍。

实验显示：50个客户端各含2^19个数据点、30%重复时，EP-MPD(I)总耗时1160秒（客户端641秒），EP-MPD(II) 7653秒（客户端仅111秒）；在10客户端、7数据集微调2个语言模型的FL实验中，去重带来最高19.62%的困惑度改善与27.95%的GPU训练时间缩减（重复率10%-30%）。

该工作把去重从单机预处理推进到隐私保护的FL环节，直击记忆化与隐私攻击的根源之一，动机扎实；G-PSI抽象比逐对PSI更高效是设计亮点，安全证明基于仿真范式较严谨；但公钥变体7653秒的开销对大规模FL（千级客户端）的可扩展性存疑，困惑度提升幅度依赖数据集与重复率，通用性需更多场景验证，恶意客户端行为等工程问题也待后续研究。
- **论文 PDF**: [Privacy-Preserving Data Deduplication for Enhancing Federated Learning of Language Models](https://www.ndss-symposium.org/wp-content/uploads/2025-641-paper.pdf)

#### PolicyPulse: Precision Semantic Role Extraction for Enhanced Privacy Policy Comprehension
- **作者**: Andrick Adhikari, Sanchari Das, Rinku Dewri (University of Denver)
- **方向**: 隐私保护
- **解读**:
自然语言隐私政策冗长、含糊、难读，是用户理解数据实践的公认障碍。现有NLP方案要么停留在句子或段落级的高层分类（如Polisis），要么只针对单一任务（如opt-out提取），缺少可复用于多任务的细粒度信息抽取能力，这正是PolicyPulse要填补的空白。

方法上，PolicyPulse先用BERT语义角色标注（SRL）把政策句拆成语义框架，再用串行两级XLNet分类器判断框架的隐私相关性（SKIP/KEEP）与具体实践类别（FPCU、TPSC、UCC、UAED、DR），最后把PropBank通用论元手工映射为隐私角色（数据、机制、目的、触发条件、留存期限等），覆盖146个动词。

分类器在13,946个手工标注框架上训练，两级方案加权F1达0.97（第二级宏平均F1为0.98）。应用于129,856份PPCrawl政策：共提取约3,970万个框架，平均每份约306个，但仅约30个KEEP框架，其中FPCU约占48%、TPSC约33%；collect关系抽取修正标注后精度95%、召回80%，接近PoliGraph（97%/70%）。原型应用包括短通知、营养标签、问答与偏好检查。

贡献在于把SRL细粒度引入政策分析，并开源大规模标注语料与角色映射，为合规（如GDPR留存条款）分析打下基础。局限是仅句子级处理、无跨句关系图，生成的短通知与标签未做用户实证评估，手工映射扩展成本高。相比Polisis/PoliGraph，粒度更细但工程成熟度尚浅。

- **论文 PDF**: [PolicyPulse: Precision Semantic Role Extraction for Enhanced Privacy Policy Comprehension](https://www.ndss-symposium.org/wp-content/uploads/2025-916-paper.pdf)

#### Passive Inference Attacks on Split Learning via Adversarial Regularization
- **作者**: Xiaochen Zhu, Xinjian Luo, Yuncheng Wu, Yangfan Jiang, Xiaokui Xiao, Beng Chin Ooi (NUS & MIT & MBZUAI & Renmin University of China)
- **方向**: 隐私保护
- **解读**:
分割学习（SL）是联邦学习的高效替代，客户端只上传中间表示。已有推理攻击要么假设恶意服务器主动劫持梯度（易被察觉），要么针对过宽、过浅的易攻击模型；在标准宽度、深层切分的现实设置下，被动攻击基本失效。

SDAR让好奇服务器利用与私有数据同分布但不相交的辅助数据，训练客户端私有模型的模拟器与配套解码器，并用GAN式对抗正则化（两个判别器）迫使模拟器与解码器学到可迁移的通用表示；U型SL下另训尾部模拟器并引入随机标签翻转实现标签推理。

CIFAR-10深层切分（level 7）下，SDAR在普通与U型SL中特征重建MSE均低于0.025，U型标签推理准确率超98%，而UnSplit、PCAT等被动攻击无法产出有效结果；比PCAT的MSE降低63%，效果直逼主动攻击FSHA。对dropout、L1/L2、去相关防御鲁棒，辅助数据缩至5%或缺失架构知识仍有效。

首次证明被动攻击可与主动攻击相当，且不破坏训练协议、更难被防御检测；还首次指出"更宽的模型因中间表示维度更高反而更脆弱"。局限：要求辅助数据与私有数据同分布且类别为其子集；U型深层切分下仍有挑战；同态加密/MPC代价过大、差分隐私破坏效用，尚无实用防御。

- **论文 PDF**: [Passive Inference Attacks on Split Learning via Adversarial Regularization](https://www.ndss-symposium.org/wp-content/uploads/2025-30-paper.pdf)

#### On the Robustness of LDP Protocols for Numerical Attributes under Data Poisoning Attacks
- **作者**: Xiaoguang Li, Zitao Li, Ninghui Li, Wenhai Sun (Xidian University, Purdue University, Alibaba Group)
- **方向**: 隐私保护
- **解读**:
本地差分隐私（LDP）易受数据投毒攻击——恶意客户端利用LDP机制特性向服务器发送精心构造的数据即可操纵统计估计。已有研究多为攻击探索，缺乏对不同LDP协议鲁棒性的系统比较与统一度量。

论文以攻击驱动框架评估数值属性LDP协议鲁棒性：针对分箱+一致性后处理的CFO类（GRR、OUE、OLH、HST，区分User/Server设置）与直接分布重建的SW机制，设计分布移位攻击，提出绝对移位增益（ASG）与移位增益比（SGR）两个跨协议可比指标；并设计零样本攻击检测——基于重建分布合成数据、经LDP随机化后与原报告做Wasserstein距离两组比较，用双样本KS检验判定污染。

SW与Server设置CFO比User设置CFO更鲁棒；首次发现OLH哈希域大小g越大协议越不鲁棒（超出已知的效用影响）；ϵ较大（≈1）时攻击退化为基线。检测在攻击者仅控制5%客户端时AUC稳定超过0.92（SW在β≥2.5%时AUC>0.95），而现有MUD方法几乎失效（AUC≤0.575）；在Taxi（219万样本）等三个数据集上验证。

为LDP鲁棒性比较提供了攻击无关的度量与首个无需真值的检测方案，并揭示"安全—哈希域大小"新权衡，对工程选型有直接指导。局限：检测在Server设置与β很小时较弱，且只告警不恢复；最优攻击依赖真实分布知识，攻击设计偏启发式。

- **论文 PDF**: [On the Robustness of LDP Protocols for Numerical Attributes under Data Poisoning Attacks](https://www.ndss-symposium.org/wp-content/uploads/2025-1521-paper.pdf)

#### MingledPie: A Cluster Mingling Approach for Mitigating Preference Profiling in CFL
- **作者**: Cheng Zhang, Yang Xu, Jianghao Tan, Jiajie An, Wenqiang Jin
- **方向**: 隐私保护
- **解读**:
聚类联邦学习（CFL）把数据分布相似的客户端聚成簇以应对 non-IID 数据，但诚实的服务器仅凭簇身份即可推断客户端数据偏好（偏好画像攻击）。现有匿名通信、同态加密等防御要么可被流量分析攻破、要么在 CFL 中失效——该攻击不依赖模型参数，只依赖簇身份与簇偏好。

MingledPie 让客户端自发加入多个簇：借鉴公钥消息探测，客户端比较目标簇地址与各簇公钥的比特一致性，以概率 p（假阳性率，如 0.5）生成"不可区分的簇身份集合"，使每簇混入一定比例假阳性客户端；服务器在密文域盲聚合得混合簇模型；客户端用同态加密聚合的混合系数矩阵，把混合簇模型表示为精确模型线性方程组 H·θ*=θ~ 并求解重建。理论证明身份不可区分、攻击精度界于 1/k 与 1/T、方程组可解且收敛。

六个数据集上与 IFCA、FedProx、FedEM 对比：防御偏好画像攻击准确率达 69.4%（攻击者成功率降至约 29%，且不随轮次增长，而 IFCA 下 5 轮即达 100%）；模型精度损失仅 0.02%–3.00%；簇数、客户端数、p 与阈值变化对精度影响很小。

该工作用"混淆簇身份+线性代数重建"同时解决隐私与可用性，不依赖第三方基础设施，理论扎实。局限：更强的防护需更大阈值，带来 O(k) 通信与同态加密开销；敌手限于诚实好奇服务器，未覆盖恶意客户端合谋。与差分隐私、安全聚合正交，可组合使用。

- **论文 PDF**: [MingledPie: A Cluster Mingling Approach for Mitigating Preference Profiling in CFL](https://www.ndss-symposium.org/wp-content/uploads/2025-195-paper.pdf)

#### Impact Tracing: Identifying the Culprit of Misinformation in Encrypted Messaging Systems
- **作者**: Zhongming Wang, Tao Xiang, Xiaoguo Li, Biwen Chen, Guomin Yang, Chuan Ma, Robert H. Deng
- **方向**: 隐私保护
- **解读**: 端到端加密消息系统（EEMS）保障隐私的同时阻断了内容审核，虚假信息借转发泛滥。既有 "Reporting-then-Tracing" 范式中，message traceback（CCS'19）暴露全部传播者、侵害仅接收一次的普通用户隐私，source tracing（CCS'21）只暴露源头、可追溯性严重受限。Impact Tracing 首次提出中间路线：只追踪对传播起关键作用的"影响力传播者"，同时为非影响用户提供隐私保护。

方案在报告-追踪框架中引入噪声隐藏非影响用户，用成员值解码算法从带噪传播图中识别高影响力节点，并证明噪声不阻碍影响力传播者的识别；隐私上对非影响用户实现个性化差分隐私（不同用户不同隐私预算），噪声量由隐私预算决定。实现采用 Rust + Redis，标签库用布隆过滤器（误报率 1e-9），追踪算法批量化并行 BFS。

在 College IM（1,899 节点/59,835 边）与 EU Email（986 节点/332,334 边）真实数据集上，用 SIR 模型模拟传播、k-shell 分解作为影响力基准：识别最高影响力（4-shell）传播者的检测率随噪声量变化为 82%-99%，而最不具影响力的 1-shell 用户被输出的比例不足 1%；隐私预算 5.3 时输出误报率低于 0.1%（k-shell 朴素解码为 8.77%）；工程开销极低——每条消息仅 6 字节平台存储，消息延迟低于 0.25ms。

该工作把"全追溯"与"源头追溯"两个极端之间的权衡形式化并给出可部署方案，差分隐私保证与微小的存储/延迟代价使其贴近工业落地。局限：影响力定义依赖 k-shell 与 SIR 模型，与真实社交网络的传播动力学可能偏离；噪声机制下输出仍含误报。与 message franking、message traceback、source tracing 相比，impact tracing 提供了更精细的隐私-可追溯性光谱，是该谱系上的首个实用折中点。
- **论文 PDF**: [Impact Tracing: Identifying the Culprit of Misinformation in Encrypted Messaging Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-980-paper.pdf)

#### DLBox: New Model Training Framework for Protecting Training Data

- **作者**: Jaewon Hur, Cheolwoo Myung, Sangyun Kim, Youngki Lee, Byoungyoung Lee (Seoul National University), Juheon Yi (Nokia Bell Labs)
- **方向**: 隐私保护
- **解读**: 数据所有者（如医院）把训练数据租给不可信的第三方AI开发者（如医疗创业公司）时，开发者获得数据后几乎可任意泄露：通过网络外传、把原始数据编码进模型、或让模型偏向训练数据以利后续重建。现有隐私方案（联邦学习、多方计算）假设攻击者无法完全控制数据或代码良性，均不适用。由于模型必须记忆数据才能保证性能，完全防泄露不可行；本文提出DLBox，将泄露面最小化，只允许"良性模型训练"。
DLBox的核心洞察是训练是"从数据集中学习公共模式的统计过程"，据此定义DGM-Rules判定训练代码是否良性，再利用机密计算（AMD SEV-SNP）重构训练框架，强制执行DGM-Rules，把不可信开发者的计算限制在良性训练路径内，禁止数据编码、记忆偏置等恶意数据流。
在PyTorch上实现原型，DLBox消除了大规模攻击向量：防止位编码（bit-encoding）、记忆和梯度反演等先前攻击，同时平均学习时间开销仅约4%，性能影响最小。
我认为把问题从"防泄露"重构为"防非法数据流"是务实的威胁模型调整，DGM-Rules+机密计算的组合给出了可执行的技术壁垒。局限是机密计算本身的可信基（TEE漏洞）未纳入考量，DGM-Rules对合法但非标准训练过程的误伤风险需更多评估；与联邦学习等方案相比，它首次直面"开发者完全控制数据"的最强威胁模型。

- **论文 PDF**: [DLBox: New Model Training Framework for Protecting Training Data](https://www.ndss-symposium.org/wp-content/uploads/2025-3001-paper.pdf)

#### Diffence: Fencing Membership Privacy With Diffusion Models

- **作者**: Yuefeng Peng, Ali Naseh, Amir Houmansadr (University of Massachusetts Amherst)
- **方向**: 隐私保护
- **解读**: 深度学习模型会记忆训练数据，使成员推断攻击（MIA）能判断某样本是否在训练集中，对敏感数据（人脸、医疗记录）构成重大隐私威胁。现有防御要么在训练阶段（DP-SGD、AdvReg、SELENA等，需重训且常损害精度），要么在后推断阶段（MemGuard，修改输出向量），隐私-效用权衡仍有大量改进空间。本文提出Diffence，一个全新的前推断（pre-inference）阶段防御：用生成模型在样本输入模型之前重建样本，抹除成员与非成员输入间的可区分差异。
Diffence的关键直觉是MIA利用的正是成员/非成员输入的分布差异，重建输入可消除该差异。它只作用于输入，不改训练与推断，因此可与其他防御级联（插拔式）；并专门设计为保留每个样本的预测标签，不牺牲精度，也不损害置信度向量的可用性。
在三个数据集上，Diffence使无防御模型的MIA攻击精度平均下降15.8%、攻击AUC下降14.0%，且不影响模型效用；与SOTA防御SELENA级联后额外使攻击精度下降9.3%、AUC下降10.0%，达到隐私-效用的新SOTA；平均每样本仅增加57ms推断开销。
我认为"前推断"定位是本文最大的创新点：不与训练时防御竞争，而是正交叠加，把MIA防御从"改模型/改输出"拓展为"改输入"，实用性极高。局限是依赖生成模型质量，重建可能破坏下游任务所需的关键细节（虽实验显示精度保持），且对攻击者已知防御策略的自适应场景评估有限；与训练时防御相比它无需重训，但增加了推断延迟。

- **论文 PDF**: [Diffence: Fencing Membership Privacy With Diffusion Models](https://www.ndss-symposium.org/wp-content/uploads/2025-s298-paper.pdf)

#### Delay-allowed Differentially Private Data Stream Release

- **作者**: Xiaochen Li, Zhan Qin, Kui Ren, Chen Gong, Shuya Feng, Yuan Hong, Tianhao Wang

- **方向**: 隐私保护

- **解读**: 差分隐私（DP）数据流发布研究长期聚焦实时场景，但并非所有数据流都要求实时发布，且网络延迟与处理约束使实时发布在工程上难以实现。本文重新审视"延迟"的价值：在事件级隐私设置下，作者发现主动引入延迟可以克服现有方法的内在局限，解锁巨大的精度提升潜力。

方法上，作者构建了允许延迟的数据流发布框架，并利用数据相似性与相对顺序两类特性设计了两类优化策略：基于分组（group-based）与基于顺序（order-based）的优化，分别用于降低添加的噪声量并优化噪声数据的后处理；此外还提出一种新颖的敏感度截断机制，进一步显著减少引入的噪声。

实验结果表明，在长度为 18,319 的数据流上，允许 10 个时间戳的延迟即可使所提方法相比基线获得最高达 30 倍的精度提升；代码已开源。

我的思考：把"延迟"从工程缺陷重新定义为可利用的资源，是隐私数据流研究中的视角创新，切合真实部署中批处理发布的场景；分组与顺序两种优化覆盖了数据内在结构的两个维度，敏感度截断则直击噪声源。局限是延迟容忍度因应用而异，如何自动权衡延迟与精度的最优边界、以及延迟下隐私预算的跨时间戳分配问题值得进一步研究。与实时 DP 流发布工作相比，本文展示了"以时间换精度"的清晰取舍空间。

- **论文 PDF**: [Delay-allowed Differentially Private Data Stream Release](https://www.ndss-symposium.org/wp-content/uploads/2025-828-paper.pdf)

#### CENSOR: Defense Against Gradient Inversion via Orthogonal Subspace Bayesian Sampling

- **作者**: Kaiyuan Zhang, Siyuan Cheng, Guangyu Shen, Bruno Ribeiro, Shengwei An, Pin-Yu Chen, Xiangyu Zhang, Ninghui Li

- **方向**: 隐私保护

- **解读**: 联邦学习在全局服务器上协作训练神经网络，每个本地客户端接收当前全局模型权重、基于本地私有数据回传参数更新（梯度）；但发送模型更新的过程可能泄露客户端私有数据，梯度反演攻击可利用梯度向量恢复客户端的私有训练样本，且近年出现的高级反演技术使现有防御难以招架。本文提出面向大神经网络的梯度反演防御。

方法上，CENSOR 利用模型参数的高维性，在与原始梯度正交的子空间内扰动梯度；通过正交子空间上的冷后验（cold posteriors）实现精炼的梯度更新机制，从而选出既能抵御梯度反演攻击、又保持模型效用的最优梯度。

作者在三个不同数据集上开展综合实验，针对多种最先进的攻击与防御进行评估（摘要未给出具体数值）。

我的思考：正交子空间扰动的几何直觉清晰——在垂直于原始梯度的方向上加扰动，理论上对模型更新方向的影响最小、却能破坏反演优化对梯度结构的依赖；冷后验采样为扰动选择提供了概率化框架，兼顾隐私与效用是务实目标。局限是摘要未披露量化结果，防御在高维小模型上的有效性、对批量大小与学习率等训练参数的敏感度需看全文。与梯度裁剪、噪声注入等传统防御相比，正交约束思路在保留效用方面更具原理优势，是梯度反演攻防对抗中的新防御范式。

- **论文 PDF**: [CENSOR: Defense Against Gradient Inversion via Orthogonal Subspace Bayesian Sampling](https://www.ndss-symposium.org/wp-content/uploads/2025-915-paper.pdf)

#### Cascading Spy Sheets: Exploiting the Complexity of Modern CSS for Email and Browser Fingerprinting

- **作者**: Leon Trampert, Daniel Weber, Lukas Gerlach, Christian Rossow, Michael Schwarz

- **方向**: 隐私保护

- **解读**: 为对抗用户追踪，隐私浏览器（如 Tor）与邮件应用通常禁用 JavaScript，从而封堵了用户指纹识别的主要途径；但近年研究表明部分 CSS 特性可能造成隐私泄漏，其完整指纹能力仍属未知，且攻击是否适用于邮件等更受限环境也不清楚。本文系统研究现代 CSS 动态特性用于无脚本指纹识别的潜力。

方法上，作者提出三种基于模糊测试与模板化的新技术，利用 CSS 容器查询、算术函数与复杂选择器的细微差别，在禁用 JS 的情况下高精度推断应用、操作系统与硬件配置，绕过了多种最先进的缓解措施。

结果显示：对 1176 个测试的浏览器-操作系统组合，方法能区分其中 97.95%；且同样适用于邮件应用（21 个测试的 Web、桌面或移动邮件应用中有 8 个受影响），证明在 HTML 邮件这一高度受限环境中指纹识别依然可行。作者还提出两种消除隐私泄漏根因的防御：对浏览器，预加载条件资源以消除特性依赖的泄漏；对邮件，设计邮件代理服务，在保留隐私与邮件完整性的同时大体维持特性兼容性。

我的思考：本文证明"禁用 JS 即安全"的假设被 CSS 动态特性打破，把追踪面扩展到了邮件这一此前被认为安全的场景；模糊驱动发现选择器语义差异的方法论具有可迁移性。防御上"消除根因"（预加载条件资源）而非修补单个漏洞的思路值得肯定，邮件代理服务则为厂商提供了可落地方案。局限是 8/21 的邮件应用覆盖率说明并非所有客户端都受影响，浏览器侧预加载的成本与兼容性需权衡。与 JS 指纹研究相比，本文开辟了纯 CSS 指纹的完整攻击面分析。

- **论文 PDF**: [Cascading Spy Sheets: Exploiting the Complexity of Modern CSS for Email and Browser Fingerprinting](https://www.ndss-symposium.org/wp-content/uploads/2025-s238-paper.pdf)

#### Balancing Privacy and Data Utilization: A Comparative Vignette Study on User Acceptance of Data Trustees in Germany and the US
- **作者**: Leona Lassak, Hanna Püschel, Oliver D. Reithmaier, Tobias Gostomzyk, Markus Dürmuth（Ruhr University Bochum、TU Dortmund University、Leibniz University Hannover）
- **方向**: 隐私保护
- **解读**:
大数据与联网设备时代，消费者隐私保护在技术与立法努力下仍是难题。数据受托人（data trustee）被视为有前景的方案：通过促进安全数据共享并保证个人控制来平衡数据利用与隐私，但其落地高度依赖用户接受度与信任，影响接受度的因素此前缺乏大规模实证证据。

论文开展基于情境故事（vignette）的、具有人口普查代表性的在线研究，覆盖医疗、汽车、物联网与在线数据四类场景，在德国（n=714）与美国（n=1036）分别收集数据，系统考察影响数据受托人接受度的因素。

研究发现两国用户的使用意愿差异显著，相当一部分用户持明显怀疑甚至完全拒绝态度；领域差异突出——用户匿名性、感知的个人与社会收益、数据接收方是谁显著影响接受度。反直觉的是，存储位置、运营者、监管机构等组织与监管层面的决策反而影响较小。

这是迄今规模较大、跨两国的接受度实证研究，"监管因素不如个人收益因素重要"的发现对合规与产品设计有直接指导意义。局限在于 vignette 情境与实际数据共享行为存在差距，自我报告意愿可能高估真实采用率；结论向其他司法辖区推广需谨慎。

- **论文 PDF**: [Balancing Privacy and Data Utilization: A Comparative Vignette Study on User Acceptance of Data Trustees in Germany and the US](https://www.ndss-symposium.org/wp-content/uploads/2025-1982-paper.pdf)

#### Automated Expansion of Privacy Data Taxonomy for Compliant Data Breach Notification
- **作者**: Yue Qin, Yue Xiao, Xiaojing Liao（Indiana University Bloomington、中央财经大学、IBM Research）
- **方向**: 隐私保护
- **解读**:
隐私合规研究中一个关键难点是：把应用实际数据处理中的具体数据项与法律、法规或政策定义的隐私数据对应比较。由于各类应用使用的数据项繁杂、各司法辖区对隐私数据的解释不同，这一任务很困难；已有隐私数据分类体系（taxonomy）构建多依赖人工或启发式规则，难以纳入各领域的新术语。

论文提出 GRASP：一种可扩展、高效的隐私数据分类体系自动构建与扩展方法。核心创新是新的基于粒度感知语义投影的超义词预测模型，在超义词预测任务上优于现有 SOTA；基于 GRASP 还实现了 Tracy——面向隐私专业人员的助手，用于在事件报告中识别与解释隐私数据，支撑 GDPR 合规的数据泄露通报。

在 15 名隐私专业人员参与的可用性研究中，Tracy 获得较高的可用性与满意度评价，验证了从"自动分类体系"到"可用工具"的落地路径。

该工作的贡献在于把隐私数据分类体系的构建从人工/规则驱动推进到自动化、可跨域扩展，并以隐私专业人员为最终用户设计了可用的通报辅助工具。局限在于超义词预测依赖英文语料与特定数据粒度定义，评估规模较小（15 人），对非英语法规体系的适应性尚需验证。

- **论文 PDF**: [Automated Expansion of Privacy Data Taxonomy for Compliant Data Breach Notification](https://www.ndss-symposium.org/wp-content/uploads/2025-230-paper.pdf)

#### A Key-Driven Framework for Identity-Preserving Face Anonymization
- **作者**: Miaomiao Wang, Guang Hua, Sheng Li, Guorui Feng（上海大学、Singapore Institute of Technology、复旦大学）
- **方向**: 隐私保护
- **解读**:
虚拟人脸是元宇宙的重要内容，也被尝试用于隐私保护，但现有方案要么永久移除可识别信息，要么把原始身份映射成一个虚拟身份、从此无法恢复，隐私与可识别性之间存在冲突。

论文首次尝试化解这一冲突，提出密钥驱动的面部匿名化与认证识别框架 KFAAR，包含两个模块：保持头部姿态的虚拟人脸生成（HPVFG）模块与密钥可控的虚拟人脸认证（KVFA）模块。HPVFG 用用户密钥把原始人脸潜向量投影为虚拟向量，再映射得到扩展编码并生成虚拟人脸，同时加入头部姿态与面部表情校正，使虚拟人脸与原图姿态表情一致；认证时 KVFA 模块用正确密钥直接识别虚拟人脸，在不暴露原图的前提下恢复原始身份；两模块以多任务学习目标联合训练。

大量实验表明 HPVFG 与 KVFA 均有效，能在保持面部匿名性的同时实现身份可识别（可认证）。

该工作提出"匿名但可认证"的密钥驱动范式，为元宇宙等既需隐私又需身份确认的场景提供了新思路。局限在于身份恢复完全依赖密钥安全，密钥泄露即等于身份泄露；潜空间投影与生成的对抗鲁棒性、以及被用于伪造身份的滥用风险，都需进一步评估。

- **论文 PDF**: [A Key-Driven Framework for Identity-Preserving Face Anonymization](https://www.ndss-symposium.org/wp-content/uploads/2025-729-paper.pdf)

### 系统与操作系统安全（18 篇）

#### WAVEN: WebAssembly Memory Virtualization for Enclaves

- **作者**: Weili Wang, Honghan Ji, Peixuan He, Yao Zhang, Ye Wu, Yinqian Zhang

- **方向**: 系统与操作系统安全

- **解读**: TEE（如 Intel SGX）支撑了机密计算范式，而"Wasm+SGX"方案能在一个 enclave 内实现多租户强隔离（复用 enclave 比新建平均快 159 倍，单 enclave 多客户端比每客户端一 enclave 快 4.06–53.70 倍），已成为机密 FaaS（Se-Lambda、S-FaaS、Teaclave 等）的主流底座。但 Wasm 的线性内存模型把整个内存视为单一不可分字节数组，既不支持跨模块数据共享（只能整体导出内存），也没有只读区域等细粒度访问控制，严重限制了有状态 FaaS、数据市场等需要安全共享数据的机密计算场景。本文提出 WAVEN，首个在 Wasm 中实现跨模块内存共享与页级访问控制的方案。

方法上，WAVEN 在 Wasm 运行时内实现软件 MMU 与分页机制：采用单级页表（65,536 项、512KB），每次内存访问仅一次页表遍历；设计"异常页 + 页填充"——用 7 字节填充消除昂贵的边界检查（SGX 无硬件边界检查支持），跨页访问落在填充区而不会产生非法访问；访问控制采用读/写双页表，只读页在写页表中映射到异常页，动态改权限只需改页表项，无需每次写操作做权限检查；共享内存通过把多个模块的 Wasm 页重映射到同一虚拟页实现。实现基于 WAMR 与 SGX SDK 2.19（约 3,000 行代码），不修改 Wasm 字节码与编译链，完全兼容 Wasm 规范，传统模块无需改动即可运行。

在 SGX2 测试环境（Xeon Gold 5318Y、8GB EPC）评估：PolyBench 30 个基准几何平均开销仅 10.42%（jacobi-2d 最大 56.50%，with3mm 反而快 30.57%）；对比显示若保留逐次边界检查开销高达 30.09%，双页表方案比"单页表+权限检查"（22.78% 开销）平均仅增 2.36%；STREAM 内存压力测试中 4 项有 3 项优于原生 WAMR。机密数据库（SQLite）查询开销 1.92%–23.09%（几何平均 12.52%），隐私保护机器学习开销 6.14%；数据共享场景（多写多读、多读）高并发下最多提速 2.4 倍。修改后的 AOT 编译器生成的代码还比原版小 16.9%、编译时间短 5.49%。

我的思考：本文的工程成色突出——用单级页表、异常页、页填充、双页表四个机制把软件 MMU 的开销压到 10% 级别，且逐项用消融实验量化各优化的价值，方法论严谨。共享内存与只读语义的组合直接命中机密计算的真实痛点（数据市场多买方共享、有状态 FaaS 状态传递），2.4 倍并发加速验证了实际收益。局限方面：威胁模型假定 SGX 自身安全，未讨论侧信道等 TEE 既有风险；数据共享接口（runtime interfaces）标注为"preliminary"，权限管理、跨模块认证等系统性问题着墨有限；单级页表 4GB 虚拟空间上限与 512KB 页表开销在超大内存场景需进一步考察；仅验证 AOT 模式，解释/JIT 模式未覆盖。与 Faasm 的非官方共享方案相比，WAVEN 无需 enclave 内不可实现的系统调用，更具落地性。总体为 Wasm 内存模型演进贡献了可借鉴的页级虚拟化范式，对 Wasm 官方 multi-memory/内存保护提案也有参考价值。

- **论文 PDF**: [WAVEN: WebAssembly Memory Virtualization for Enclaves](https://www.ndss-symposium.org/wp-content/uploads/2025-746-paper.pdf)

#### TZ-DATASHIELD: Automated Data Protection for Embedded Systems via Data-Flow-Based Compartmentalization

- **作者**: Zelun Kong, Minkyung Park, Le Guan, Ning Zhang, Chung Hwan Kim
- **方向**: 系统与操作系统安全
- **解读**: MCU 固件共享单一地址空间存放 PIN、GPS 等敏感数据，现有分区方案（Minion、ACES 等）建立在"特权软件可信"的弱模型上，内核漏洞（如 FreeRTOS CVE-2021-43997）即可绕过基于 MPU 的隔离。

用 ARM TrustZone（ARMv8-M）承载"敏感数据流（SDF）"分区：开发者以 TZDS_*_R/W/RW 宏标注敏感数据，编译期对机密性/完整性数据做前向/后向切片得到最小分区；安全世界内用 SFI 插桩实现分区间隔离，对共享/外设数据叠加轻量 CFI（影子栈）与 DFI（RDT 表）校验，输出安全监视器、SDF 分区与普通世界固件。

12 个真实应用（6 裸机+6 FreeRTOS）上，单分区可访问安全世界地址空间平均削减 80.8%（共享数据 96.4%）、ROP gadget 减少 88.6%；平均开销 14.7%（含 CFI/DFI）、7.6%（不含），比函数级分区快 36.7%；内存开销小于 45KB flash、7KB RAM；监视器经形式化验证，shellcode/ROP/数据-only/IRQ 四类攻击均被限制在受害分区。

用硬件边界解决特权软件失陷、SFI 解决 TEE 内隔离、按需 CFI/DFI 解决共享数据，三层机制各司其职、开销只花在必要处；但依赖源码标注与切片精度（过/欠近似会漏保护或过度分区），DFI 只记录最近 N 次写者，评估限于单开发板且排除物理攻击与侧信道。

- **论文 PDF**: [TZ-DATASHIELD: Automated Data Protection for Embedded Systems via Data-Flow-Based Compartmentalization](https://www.ndss-symposium.org/wp-content/uploads/2025-563-paper.pdf)

#### type++: Prohibiting Type Confusion with Inline Type Information

- **作者**: Nicolas Badoux, Flavio Toffalini, Yuseok Jeon, Mathias Payer
- **方向**: 系统与操作系统安全
- **解读**: 类型混淆（向下转型）是 C++ 常见攻击向量，可导致控制流劫持；但运行时类型信息只存在于多态类（仅约 3% 的转型），分离元数据方案（TypeSan、HexType）开销高且对象生命周期跟踪不准，产生误报与漏报。

type++ 是 C++ 方言，为参与转型的每个对象内嵌类型字段（vptr/RTTI），使所有向下转型可动态校验；按需提供全对象（P1）、仅转型类（P2，告警行数从 1480 降到 179）、标注类（P3）三级，扩展 LLVM-CFI 检查范围，并系统处理默认构造、C/自定义分配器、union、const、库协同编译等兼容问题。

SPEC CPU2006+2017 编译 2,040 kLoC 仅改 314 行（0.04%），平均开销 0.94%（比 HexType 快两个数量级），运行期校验 900 亿次转型（LLVM-CFI 的 23 倍），发现 122 处类型混淆、其中 14 处新发现；Chromium 改 229 行保护 94.6% 的类，JetStream2 开销 0.98%。

"每个对象自带类型"以极低代价移植进 C++，内联元数据规避了分离元数据的生命周期跟踪缺陷，可作 sanitizer 与部署级缓解两用；但威胁模型排除任意写（类型字段可被越界写覆盖，需 DEP/CFI/安全分配器协同），要求源码可重编译、外部库需协同编译，自定义分配器 allow-list 需人工维护。

- **论文 PDF**: [type++: Prohibiting Type Confusion with Inline Type Information](https://www.ndss-symposium.org/wp-content/uploads/2025-53-paper.pdf)

#### Too Subtle to Notice: Investigating Executable Stack Issues in Linux Systems
- **作者**: Hengkai Ye (The Pennsylvania State University), Hong Hu (The Pennsylvania State University)
- **方向**: 系统与操作系统安全
- **解读**: 问题：W⊕X 是抵御代码注入的主流机制，但近年来 Electron、VSCode、CockroachDB 等流行应用反复出现意外禁用 W⊕X、重新引入可执行栈的案例（作者称为 BADASS 问题），根源多为手写汇编漏掉 `.note.GNU-stack` 指令。问题是：为何连当代开发者（包括安全研究者）都会反复犯这个错？

方法：两项调查。其一，选取 21 个发表在顶会或由知名公司开发的 inline reference monitor（IRM，含 SFI、CFI、进程内隔离等）转换简单程序并检查运行进程的栈权限；其二，从 GNU 编译工具链、内核与加载器源码系统分析 Linux 上 W⊕X 的完整实施链，并将调查扩展到 PIE、RELRO、stack canary、FORTIFY_SOURCE 与 Intel CET 五种机制。

结果：21 个 IRM 中 11 个禁用了 W⊕X（含 4 个 CFI、2 个进程内隔离、5 个二进制重汇编器，如 MCFI、RockJIT、πCFI、PathArmor、ERIM、Donky），报告后 6 个工具已修复。根因分五类：C 取地址嵌套函数产生可执行栈、汇编文件漏 `.note.GNU-stack`、对象文件含 SEC_CODE 标记或缺失该节、二进制 PT_GNU_STACK 带 PF_X 或缺失、共享库问题。还发现 3 个 CFI 方案同时禁用 FORTIFY_SOURCE，所有禁用 W⊕X 的 IRM 也都关闭了 Intel CET（漏 `.note.gnu.property`）。

我的思考：价值在于把"开发者个人疏忽"重新定性为"安全机制生态的系统性脆弱"——连安全专家都中招，说明工具链默认与文档设计存在缺陷，调查方法严谨、覆盖面广。局限：IRM 清单以论文原型为主，代表性有限；未量化真实世界受影响二进制的规模；缓解建议缺乏落地验证。启示是安全特性应默认可达、不可被静默关闭，编译器与链接器应在该环节显式告警。
- **论文 PDF**: [Too Subtle to Notice: Investigating Executable Stack Issues in Linux Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-924-paper.pdf)

#### TME-Box: Scalable In-Process Isolation through Intel TME-MK Memory Encryption
- **作者**: Martin Unterguggenberger (Graz University of Technology), Lukas Lamster (Graz University of Technology), David Schrammel (Graz University of Technology), Martin Schwarzl (Cloudflare, Inc.), Stefan Mangard (Graz University of Technology)
- **方向**: 系统与操作系统安全
- **解读**: 问题：云服务为追求性能常用进程内隔离替代进程隔离（如 FaaS），但内存安全漏洞可致租户数据泄露（Heartbleed、Cloudbleed 即为例证）。现有进程内隔离机制不满足云需求：MPK 仅 16 个保护域，无法隔离单进程内数千个云 worker；SFI 粒度粗、只能隔离预定的连续区域。

方法：首次将 Intel TME-MK（总内存加密多密钥，本为 TDX 虚拟机加密设计）重新用于进程内沙箱：通过编译器插桩控制内存操作的基址与索引，强制沙箱用专属 TME-MK 密钥进行内存交互，越权访问会被硬件完整性保护检测；利用页别名实现从单缓存行到整页的细粒度隔离；利用 15-bit keyID 支持最多 32K 并发沙箱，并支持用户空间频繁切换策略；原型采用 x86 段式寻址优化性能。

结果：SPEC CPU2017 基准上，数据隔离的几何平均开销 5.2%，代码+数据隔离为 9.7%；可扩展至 32K 沙箱（对比 MPK 的 16 域）；并在 NGINX 上完成评估；论文给出系统化的威胁模型与安全属性分析。

我的思考：贡献成色高——把"为 VM 设计的加密"重新定位为通用进程内隔离，密码学隔离比 MPK 的权限位更根本：即使地址越界，密钥不符也读不出数据，扩展性提升两个数量级。局限：依赖较新的 TME-MK 硬件，编译器插桩须覆盖所有内存操作（JIT/自修改代码场景存疑），密钥切换开销与 TLB 影响在真实云负载下需更多验证。相比 MPK 与 SFI，TME-Box 在粒度与规模上都是范式升级，但硬件与工具链改造门槛决定其短期主要适用于大型云厂商。
- **论文 PDF**: [TME-Box: Scalable In-Process Isolation through Intel TME-MK Memory Encryption](https://www.ndss-symposium.org/wp-content/uploads/2025-277-paper.pdf)

#### The Road to Trust: Building Enclaves within Confidential VMs
- **作者**: Wenhao Wang, Linke Song, Benshan Mei, Shuang Liu, Shijun Zhao, Shoumeng Yan, XiaoFeng Wang, Dan Meng, Rui Hou
- **方向**: 系统与操作系统安全
- **解读**: 机密虚拟机（CVM，如 AMD SEV-SNP）把整个 VM 放入 TEE，但 guest OS 体量庞大、可能被攻陷，用户难以证明和控制 VM 内具体应用代码的完整性。此前 vSGX 用双 VM 模拟 SGX，空 ECALL 约 1.5ms（比原生 SGX 慢约 160 倍），且只能容纳一个 enclave，实用受限。

本文提出 NestedSGX：利用 SEV-SNP 的 VMPL 硬件特性在 CVM 内部构建 SGX 式 enclave。安全监视器与 SGX 仿真层运行于 VMPL0，guest OS 与应用运行于 VMPL1，由 RMP 硬件强制内存隔离；采用影子页表防止 guest OS 篡改 enclave 映射；用有限状态机建模仿真层状态，解决 EENTER/AEX/EEXIT 等同步/异步切换的状态恢复难题；通过把 AIK 公钥摘要写入 SEV-SNP 证明报告形成两级信任链，并移植了 SGX SDK、Rust SGX SDK 与 Occlum（含 HotCalls 优化）。

在商业 EPYC 硬件上，上下文切换约 32,000–34,000 周期，比 SGX 高 1.9–2.1 倍，但比 vSGX 快约两个数量级（空 ECALL 12μs vs 1.5ms）；nbench 约 1.3%、内存带宽 98.7%、WolfSSL 约 0.78%、SQLite 约 0.77% 开销，I/O 读性能约基线 65.3%；无需修改 KVM 等宿主软件，多数 SGX 示例程序免改运行。

NestedSGX 在整机 TEE 与进程 TEE 之间提供了兼顾兼容性与性能的折中，工程完整度高（约 7500 行）。局限：不覆盖缓存/瞬态执行等侧信道与 DoS；基线采用 sim 模式对照；EGETKEY/EREPORT 因 Rust 加密实现而偏慢。与 Veil、vSGX 的对比清晰展示了 VMPL 方案的优劣，为"在不可信 guest OS 的 CVM 中建立应用级信任"提供了可复制的范式。
- **论文 PDF**: [The Road to Trust: Building Enclaves within Confidential VMs](https://www.ndss-symposium.org/wp-content/uploads/2025-385-paper.pdf)

#### The Forking Way: When TEEs Meet Consensus
- **作者**: Annika Wilde, Tim Niklas Gruel, Claudio Soriente, Ghassan Karame
- **方向**: 系统与操作系统安全
- **解读**: TEE 与区块链被宣传为"良配"：TEE 为智能合约提供机密计算，共识层则有望抵御 TEE 的分叉攻击（回滚与克隆）。但此前无人系统化分析两者的集成方式及抗分叉安全性，且已有工作只研究回滚型分叉，忽视了克隆型威胁。

作者对 29 个 TEE 区块链方案（从学术到生产）做系统化分析，按功能分为 TEE 智能合约、TEE 共识协议、TEE L2、TEE 区块链应用四类，归纳出四类抗分叉技术——无状态 enclave、临时身份、固定客户端集、状态序列化（交易重放/时间戳/链上存储），并逐一分析其表达力、吞吐、存在性诚实、链分叉、随机计算等八项局限。

29 个平台中 5 个对分叉攻击脆弱（4 个克隆、1 个回滚+克隆）。作者在三个生产网络上发现并验证了此前未知的克隆攻击：Phala——克隆 pruntime 并隔离出网络，以旧状态应答合约查询，绕开心跳机制；Secret Network——用同一合约代码的另一实例基于不同状态错误应答，绕过 Jean-Louis 等人提出的反回滚措施；Ten——繁殖 enclave 提高被选为 rollup 提议者的概率。基于分析提出了临时 ID+密钥管理等实用对策，并已向各平台负责任披露。

这是首个 TEE+共识抗分叉的系统化 SoK，核心洞见是"防回滚不等于防克隆"，且随机化计算与"同一平台同二进制不可区分"是克隆攻击的根源。局限：仅覆盖 SGX 生态（两个例外基于其他 TEE）、未考虑 enclave 被攻陷与侧信道。对三个生产网络的可复现攻击使理论与实践结合扎实，为后续 TEE 区块链设计提供了清晰的安全清单。
- **论文 PDF**: [The Forking Way: When TEEs Meet Consensus](https://www.ndss-symposium.org/wp-content/uploads/2025-1934-paper.pdf)

#### Retrofitting XoM for Stripped Binaries without Embedded Data Relocation
- **作者**: Chenke Luo, Jiang Ming, Mengfei Xie, Guojun Peng, Jianming Fu
- **方向**: 系统与操作系统安全
- **解读**:
JIT-ROP 利用内存泄露漏洞动态收集代码 gadget 并实时构造攻击载荷，可绕过代码随机化防御。执行只读内存（XoM）通过撤销可执行页的读权限对抗内存泄露，但现有方案要么依赖编译期分离代码与数据，要么需要补丁二进制以重定位嵌入数据，难以保护遗留与 COTS 程序，部分方案还被迫修改底层架构机制，牺牲兼容性与性能。

PXoM 利用 Intel 内存保护密钥（MPK）实现页内细粒度 R⊕X 权限控制：仅对代码区域禁止读取，同时允许代码页内的合法数据读取，从而无需嵌入数据重定位即可加固 stripped 二进制。其提出单向反汇编（Unidirectional Disassembly）策略无漏报地识别代码内嵌数据，并定制 Linux 内核加载器与运行时监控器动态审查对代码页的读请求，辅以缓存式优化策略提升高频数据访问性能。

微基准与宏基准（lmbench、SPEC CPU 2006/2017、3 个 Web 服务器、4 个数据库软件）显示 PXoM 平均运行时开销仅 0.22%–0.82%；安全评估表明受保护二进制中残留的 gadget 极少，远不足以构造有害载荷。作者已开源代码与数据集。

相比需修改缓存架构的 HideM 与被代码推断攻击削弱的 DCR，PXoM 在不重定位内嵌数据的前提下实现 XoM，实用价值突出。局限在于依赖 x86-64 与 MPK 硬件特性，反汇编正确性仍是其安全性的前提假设；后续可探索对其他架构的移植及与 CFI 等防护的组合。

- **论文 PDF**: [Retrofitting XoM for Stripped Binaries without Embedded Data Relocation](https://www.ndss-symposium.org/wp-content/uploads/2025-825-paper.pdf)

#### RContainer: A Secure Container Architecture through Extending ARM CCA Hardware Primitives
- **作者**: Qihang Zhou, Wenzhuo Cao, Xiaoqi Jia, Peng Liu, Shengzhi Zhang, Jiayun Chen, Shaowen Xu, Zhenyu Song
- **方向**: 系统与操作系统安全
- **解读**:
容器高效轻量却因共享宿主内核而隔离薄弱，被攻陷容器可威胁其他容器乃至整个平台；"强隔离、轻量、最小高特权代码"三目标难以兼得：虚拟机方案重且扩大 TCB，TrustZone 方案特权码多且容器间权限不分，CCA 现有设计（如 Shelter）在 EL3 引入大量代码。

RContainer 扩展 ARM CCA 的 RME 硬件原语：在 Normal World EL1 引入受信任的 mini-OS，采用混合页表（mixed-pagetable）实现同特权级隔离，将多数安全功能从 EL3 下沉至 EL1（EL3 仅 130 行 C）；通过 Granule Protection Check 为每个容器在内核层建立隔离物理地址域 con-shim，并以独立 Shim-GPT 实现容器间（含内核空间）的双向强隔离。

TCB 仅 2,647 SLoC（无加密；启用加密另加 2.3K SLoC），分析的全部 30 个容器/Linux 漏洞（提权、信息泄露、内存破坏、DoS）均被抵御。性能：内核编译开销约 4.5%（IPC 加密 6.6%），Apache/Nginx 平均约 5%，Memcached 0.3%，MySQL 0.2%–0.3%，Netperf <3%；EL3 运行时代码增量仅约 1.2%（对比 Shelter 约 18%）。

RContainer 在 ARM CCA 上同时满足三目标，"每容器一张 Shim-GPT"设计优雅，把 EL3 特权代码压到极限，代表了容器安全与机密计算融合的方向。局限：依赖 ARMv9 RME 硬件成熟与生态支持；文件系统类负载（约 2 倍）与 IPC 加密（约 20%）微基准开销明显；GPT 切换（约 492 µs）在多容器高并发下的扩展性待验证。

- **论文 PDF**: [RContainer: A Secure Container Architecture through Extending ARM CCA Hardware Primitives](https://www.ndss-symposium.org/wp-content/uploads/2025-328-paper.pdf)

#### Oreo: Protecting ASLR Against Microarchitectural Attacks
- **作者**: Shixin Song, Joseph Zhang, Mengjia Yan (Massachusetts Institute of Technology)
- **方向**: 系统与操作系统安全
- **解读**:
ASLR是现代系统最广泛部署的内存防护，但TLB、缓存、BTB、prefetch等微架构侧信道可彻底击穿它（如2017年macOS内核0day、CVE-2022-42703）。逐一封堵每个信道（如FLARE）不可持续，需从源头阻止ASLR秘密泄漏。

论文把微架构ASLR绕过系统化为三条泄漏路径（虚拟内存布局探测、指针作地址、指针作数据），设计软硬件协同方案Oreo：在虚拟与物理内存间插入"掩码内存"层，用Virt2Mask把受保护的"微架构无关位"从虚拟地址中剔除后再进入TLB/缓存/BTB等结构，把映射与否的检查推迟到指令提交时；并提出增强型选位策略（新增熵、保护与ASLR随机位不重叠的高位）。

在Linux 6.6内核（785行改动）与gem5模拟器（1,897行）上原型实现，支持内核文本、模块与用户态ASLR（内核保护31–38位，用户态用非规范位48–52，5位熵）。SPEC2017 IntRate平均CPI开销仅0.11%，LEBench开销可忽略；存储开销约256字节核内+584字节内存系统，并提供非干涉性质的形式化证明。

思路是从泄漏源头阻断而非逐信道修补，且与现有Spectre缓解互补，熵分析显示增强选位在防代码复用与防投机执行间取得平衡。局限：仅在模拟器验证、无流片评估；PTE空闲位限制熵（用户态仅5位），5级页表会进一步压缩非规范位；安全困境（受保护位不影响Spectre gadget利用）需谨慎选位。

- **论文 PDF**: [Oreo: Protecting ASLR Against Microarchitectural Attacks](https://www.ndss-symposium.org/wp-content/uploads/2025-264-paper.pdf)

#### KernelSnitch: Side Channel-Attacks on Kernel Data Structures
- **作者**: Lukas Maar, Jonas Juffinger, Thomas Steinbauer, Daniel Gruss, Stefan Mangard
- **方向**: 系统与操作系统安全
- **解读**: 共享缓存等硬件元素公认会引入微架构侧信道泄漏，一种根治思路是不跨安全域共享硬件。但即使假设硬件无泄漏，操作系统这类关键软件组件是否自身引入软件侧信道泄漏，此前并不清楚。KernelSnitch 正是要回答这个问题。

KernelSnitch 针对内核常用的四类数据结构——固定大小哈希表、动态扩容哈希表、基数树与红黑树——利用其元素数量（占用水平）决定访问耗时的特性构建时序信道。由于时序差极小（最低仅约 8 条指令），作者提出两种放大手段：以内存抖动降级系统性能、向数据结构追加额外元素拉大占用差距。攻击无需特权且与硬件无关，并自动校准高低占用阈值。

在 Intel i7-1260P 等平台上，空闲系统区分占用水平的准确率超过 99.9%，3/4 负载的嘈杂系统上仍高于 98.5%。三个案例研究：隐蔽信道真实容量最高 580 kbit/s（误码率 2.8%）；利用 Linux 哈希表以用户标识与内核秘密混合索引的机制，通过检测哈希碰撞枚举约 2^35.5 的搜索空间，在 3.7-63.6 秒内泄漏 mm_struct 堆指针，再经跨缓存复用定位 msg_msg——这是首次以侧信道实现内核堆指针泄漏；对 Ahrefs Top 100 的网站指纹攻击 F1 达 89.5%。

该工作的成色在于把"硬件无泄漏"假设下仍存在的软件级攻击面系统化、通用化，且在 x86-64、AArch64、RISC-V 上均验证了堆指针泄漏。局限：堆指针泄漏依赖大量枚举与确定性调度，缓解研究也承认常数时间化不现实，防御难以落地。与页缓存、slab 分配器、互斥原语等既有 OS 侧信道工作一脉相承，KernelSnitch 把攻击面推进到更通用的数据容器层面，为 OS 安全设计提供了新的警示。
- **论文 PDF**: [KernelSnitch: Side Channel-Attacks on Kernel Data Structures](https://www.ndss-symposium.org/wp-content/uploads/2025-223-paper.pdf)

#### Incorporating Gradients to Rules: Towards Lightweight, Adaptive Provenance-based Intrusion Detection
- **作者**: Lingzhi Wang, Xiangmin Shen, Weijian Li, Zhenyuan Li, R. Sekar, Han Liu, Yan Chen
- **方向**: 系统与操作系统安全
- **解读**: 溯源图入侵检测（PIDS）通过细粒度因果分析区分正常与恶意行为，其中规则型系统轻量、实时、可解释，但普遍采用过于简单通用的规则，导致误报率高企、需人工调参；嵌入型（GNN 等）系统精度高却计算开销大、检测延迟长、结果黑盒。CAPTAIN 的目标是让规则型 PIDS 自动适应不同环境，兼具轻量与学习能力。

CAPTAIN 为规则型 PIDS 引入三类自适应参数——节点信任值、边信任值与报警生成阈值——并构建可微的标签传播框架，使规则推理变成可微函数；借助损失函数与梯度下降，在仅含良性数据的训练集上自动优化参数，收紧过宽规则以削减误报。针对无验证集场景还给出正则系数的启发式设定方法。

在 DARPA Engagement 3/5（CADETS、TRACE、THEIA）与三家 SOC 模拟环境上：CAPTAIN 检测出全部攻击，事件级误报率比 SHADEWATCHER 降 93%（15.66x）、比规则基线 MORSE 降 95%（20.45x）；TRACE 上误报事件从 MORSE 的 22,500 降至 1,099（0.0212%）；节点级精度上 TP 与基线相当而 FP 大幅减少（如 TRACE 上 12 个 vs MORSE 243 个）。流式处理无缓冲时间，检测速度比嵌入型基线快 10 倍以上，检测时间与 MORSE 相当（T-3 上 1:31 vs 1:29）。

"把规则系统变成可微函数"这一思路优雅地调和了可解释性与自适应学习，直击 SOC 告警疲劳与人工调参痛点，并提供公开数据标注。局限：仍需良性训练数据与超参数调优，对训练集未覆盖的隐蔽攻击，保守设置可能失效；对抗评估（mimicry/投毒）的深度有限。与 MORSE 等纯规则系统相比是配置自动化的质变，为混合式 PIDS 开辟了新方向。
- **论文 PDF**: [Incorporating Gradients to Rules: Towards Lightweight, Adaptive Provenance-based Intrusion Detection](https://www.ndss-symposium.org/wp-content/uploads/2025-822-paper.pdf)

#### CounterSEVeillance: Performance-Counter Attacks on AMD SEV-SNP

- **作者**: Stefan Gast, Hannes Weissteiner, Robin Leander Schröder, Daniel Gruss

- **方向**: 系统与操作系统安全

- **解读**: 机密虚拟机（VM）依托 AMD SEV-SNP 处理器扩展，在共享主机环境中为机密 VM 提供完整性与机密性保证。本文提出 CounterSEVeillance：首个从性能计数器数据中泄露秘密依赖的控制流与操作数属性的侧信道攻击，也是首个在完全打过补丁的系统上、以单指令分辨率利用 SEV-SNP VM 性能计数器泄漏的工作。

方法上，作者系统分析了 SEV-SNP VM 中的性能计数器事件，发现 228 个事件暴露给潜在的恶意 hypervisor；攻击利用 APIC 中断配合页错误对受害 VM 进行单步执行，以指令级分辨率记录性能计数器轨迹，再将轨迹与二进制文件匹配，精确恢复任意秘密依赖条件分支的结果并推断操作数属性。

四个攻击案例展示了 6 个暴露计数器的具体可利用泄漏：从单次 Mbed TLS 签名过程中不到 8 分钟提取完整 RSA-4096 密钥；首次对 SEV-SNP VM 中的 TOTP 验证发起侧信道攻击，平均仅需 31.1 次猜测即可恢复 6 位 TOTP；还能泄漏 TOTP 派生自的 base32 解码器底层密钥，并可构造 divide-and-surrender 式攻击的明文检查预言机。

我的思考：本文结论具有警示意义——把整个 VM 移入特权敌手（hypervisor）环境会显著扩大攻击面，海量代码从未针对该安全设置审查过；228 个暴露计数器说明 SEV-SNP 的硬件隔离未覆盖性能计数这一信息通道。单步执行+轨迹匹配的技术实现精巧，RSA-4096 八分钟提取与 TOTP 平均 31.1 次猜测的成果极具冲击力。局限是需恶意 hypervisor 这一强前提，且单步执行会显著拖慢受害 VM；与 Cache 侧信道攻击相比，性能计数器通道更隐蔽、更难关闭。

- **论文 PDF**: [CounterSEVeillance: Performance-Counter Attacks on AMD SEV-SNP](https://www.ndss-symposium.org/wp-content/uploads/2025-1038-paper.pdf)

#### BULKHEAD: Secure, Scalable, and Efficient Kernel Compartmentalization with PKS

- **作者**: Yinggang Guo, Zicheng Wang, Weiheng Bai, Qingkai Zeng, Kangjie Lu

- **方向**: 系统与操作系统安全

- **解读**: 层出不穷的漏洞迫切要求原则性缓解以限制利用的影响；但 Linux 等商品化操作系统的单体内核架构，使攻击者可借任一内核组件的漏洞攻陷整个系统。内核隔离（compartmentalization）遵循最小特权原则，是很有前景的方案，但现有机制在安全性、可扩展性与性能之间难以权衡，根源在于众多复杂组件之间的互不信任。本文提出 BULKHEAD。

方法上，BULKHEAD 利用 Intel 新的硬件特性 PKS 将数据与代码隔离为互不信任的隔离舱（compartment），为无限数量的隔离舱提供双向隔离，并受益于 PKS 快速的舱切换；以不信任为前提，引入轻量级内核内监控器，强制多项重要安全不变量，包括数据完整性、仅执行内存与舱接口完整性；还提供局部性感知的两级方案，可扩展至无限数量的隔离舱；在 Linux v6.1 上实现原型，隔离可加载内核模块（LKM）。

评估证实了方法的有效性：作为系统级影响，在 160 个被隔离 LKM 下，真实应用的平均性能开销为 2.44%；聚焦特定舱的 ApacheBench ipv6 测试开销低于 2%；且性能几乎不受舱数量影响，可扩展性很强。

我的思考：BULKHEAD 把 PKS 硬件能力充分变现——双向隔离+无限舱+快速切换的组合，解决了先前内核隔离方案"越隔离越慢、舱越多越难管"的扩展难题；2.44% 的平均开销与几乎与舱数量无关的性能特征，使大规模内核隔离首次具备工程可行性。局限是仅覆盖 LKM、未触及内核核心子系统间的隔离，监控器本身也成为新的攻击目标。与基于虚拟化或软件防护的内核隔离相比，PKS 硬件支持让安全性与性能得以兼得，是内核最小特权化的代表性进展。

- **论文 PDF**: [BULKHEAD: Secure, Scalable, and Efficient Kernel Compartmentalization with PKS](https://www.ndss-symposium.org/wp-content/uploads/2025-s328-paper.pdf)

#### Blindfold: Confidential Memory Management by Untrusted Operating System

- **作者**: Caihua Li, Seung-seob Lee, Lin Zhong

- **方向**: 系统与操作系统安全

- **解读**: 机密计算（CC）作为保护用户数据免受不可信操作系统侵害的机制受到广泛关注；现有 CC 方案通过向 OS 隐藏机密内存和/或对其加密来保证机密性，但这样做使 OS 的内存优化不可用，或使优化所需的可信计算基（TCB）复杂化。本文提出 Blindfold 设计，旨在克服这些局限。

方法上，Blindfold 与许多 CC 方案一样，依赖运行在比内核更高特权级的小型可信软件组件 Guardian；它有三项关键技术：其一，Guardian 不采用嵌套页表，而是通过切换页表与中断表来调解 OS 访问内存与处理异常的方式；其二，采用轻量级能力（capability）系统规范 OS 对用户内存的语义访问，统一了此前逐案处理的做法；其三，提供无需加密的机密内存管理安全 ABI。

作者在 ARMv8-A/Linux 上实现了 Blindfold 原型，得以评估让不可信 Linux 内核管理机密内存的代价：Blindfold 的运行时 TCB 比相关系统更小且性能有竞争力；更重要的是，Linux 内核（除内存压缩外）的所有内存优化都能正常用于机密内存，仅需约 400 行内核修改。

我的思考：Blindfold 的核心反直觉洞察是"让不可信 OS 继续管理机密内存"——用能力系统仲裁语义访问、用安全 ABI 划定边界，从而保住 OS 的内存优化能力，避免加密或隐藏带来的巨大代价；约 400 行修改即可让全部内存优化兼容机密内存，工程说服力强。局限是"不加密"的机密性完全依赖 Guardian 的正确性与能力系统的完备性，物理攻击与微架构侧信道的防护需另行处理。与 Intel TDX/AMD SEV 等硬件加密方案相比，Blindfold 走"小 TCB 软件仲裁"路线，为机密计算的性能与 TCB 权衡提供了新解。

- **论文 PDF**: [Blindfold: Confidential Memory Management by Untrusted Operating System](https://www.ndss-symposium.org/wp-content/uploads/2025-294-paper.pdf)

#### ASGARD: Protecting On-Device Deep Neural Networks with Virtualization-Based Trusted Execution Environments
- **作者**: Myungsuk Moon, Minhee Kim, Joonkyo Jung, Dokyung Song（延世大学）
- **方向**: 系统与操作系统安全
- **解读**:
端侧深度学习因保护用户隐私而日益普及，但 DNN 模型本身反过来成为隐私风险对象。已有方案借助 Arm TrustZone 的可信执行环境（TEE）保护模型，但要么把推理完全放进 TEE（只能 CPU 推理或需修改闭源软件接入加速器），要么把部分推理卸载到 REE（留下未保护部分或产生大量切换开销）。

论文提出 ASGARD，首个面向旧款 Armv8-A SoC、基于虚拟化的 TEE 端侧 DNN 保护方案：通过安全 I/O 直通把 SoC 集成加速器纳入 TEE 边界，通过激进但保安全的平台级与应用级 TCB 精简控制可信计算基大小，以 exit-coalescing 推理规划减少昂贵的 TEE-REE 切换，兼容专有软件、保持最小 TCB、近零开销。

在搭载 Rockchip NPU 的 RK3588S（Armv8.2-A 商品化平台）上实现，无需修改 Rockchip 或 Arm 专有软件；评估表明其能以极小 TCB 与可忽略的延迟开销有效保护端侧 DNN。

相比 TrustZone 路线，虚拟化 TEE 在兼容闭源软件与保持最小 TCB 上更具工程优势。局限在于依赖特定硬件虚拟化特性与 NPU 直通能力，通用性待更多平台验证；对 TEE 自身侧信道等底层威胁的防护不在本文范围。

- **论文 PDF**: [ASGARD: Protecting On-Device Deep Neural Networks with Virtualization-Based Trusted Execution Environments](https://www.ndss-symposium.org/wp-content/uploads/2025-449-paper.pdf)

#### A Formal Approach to Multi-Layered Privileges for Enclaves

- **作者**: Ganxiang Yang, Chenyang Liu, Zhen Huang, Guoxing Chen, Hongfei Fu, Yuanyuan Zhang, Haojin Zhu (Shanghai Jiao Tong University)
- **方向**: 系统与操作系统安全
- **解读**: 可信执行环境（TEE）被广泛用于保护安全关键应用，但为提升 enclave 可用性而提出的功能扩展（feature extension）在供给模式上仍面临安全挑战——如何在引入扩展功能的同时不破坏 enclave 原有的安全保证。该文提出 Palantir，一个可验证的多层 enclave 间特权模型，用于支撑安全的 enclave 功能扩展。

Palantir 引入父-子 enclave 关系：父 enclave 被授予对子 enclave 的两种特权权限，即执行控制（Execution Control）与空间控制（Spatial Control）；通过嵌套父-子关系形成多层特权（MLP），使功能扩展可按最小权限原则被放置在不同特权层。作者构建并验证了形式化模型 TAP^∞，证明该特权模型不会破坏或削弱 enclave 的安全保证，并在开源 RISC-V TEE 平台 Penglai 上实现了原型。

评估表明 Palantir 的运行时开销低于 5%（runtime overhead <5%），启动延迟亦表现良好。

该工作的核心贡献在于把 enclave 功能扩展的安全性建立在形式化验证之上，而非仅依赖实现层面的加固，TAP^∞ 为后续同类特权模型提供了可复用的验证框架。局限在于原型仅针对 Penglai（RISC-V），未覆盖 SGX/SEV 等主流 x86 TEE 平台，也未报告真实功能扩展用例的端到端效果。与以往嵌套 enclave、多域隔离等扩展方案相比，Palantir 以显式特权层与最小权限原则为主线、更强调可验证性，但多层特权带来的管理复杂度与误配置风险仍有待进一步研究。

- **论文 PDF**: [A Formal Approach to Multi-Layered Privileges for Enclaves](https://www.ndss-symposium.org/wp-content/uploads/2025-1301-paper.pdf)

#### A Comprehensive Memory Safety Analysis of Bootloaders

- **作者**: Jianqiang Wang (CISPA Helmholtz Center for Information Security), Meng Wang (CISPA Helmholtz Center for Information Security), Qinying Wang (Zhejiang University), Nils Langius (Leibniz Universität Hannover), Li Shi (ETH Zurich), Ali Abbasi (CISPA Helmholtz Center for Information Security), Thorsten Holz (CISPA Helmholtz Center for Information Security)
- **方向**: 系统与操作系统安全
- **解读**: 引导加载程序（bootloader）连接固件与操作系统，是安全启动（secure boot）链中负责验证并加载操作系统镜像的关键一环。随着 bootloader 功能不断增多，代码规模与攻击面同步扩大，近年已发现多处内存安全漏洞，部分可导致拒绝服务甚至绕过 secure boot，但此前尚无针对 bootloader 的系统性内存安全分析。

该文基于既往 bootloader 漏洞调研，首次对 bootloader 内存安全进行系统分析：梳理各 bootloader 的潜在攻击面及其通向漏洞的路径，发现来自外设（如存储设备、网络）的恶意输入是攻击者利用漏洞的主要途径。基于该分析，作者设计并实现了面向 bootloader 的模糊测试框架，用于规模化检测漏洞。

实验中在九个 bootloader 内发现 39 个漏洞，其中 38 个为新漏洞；14 个位于广泛使用的 Linux 标准引导程序 GRUB 中，部分漏洞被恰当利用后可绕过 secure boot。迄今已有 5 个 CVE 分配至其发现。

这是首个面向 bootloader 的系统性内存安全研究，"攻击面-漏洞路径"分析为后续工作提供了方法论模板，模糊测试框架可直接复用于新 bootloader；GRUB 中 14 个漏洞及其 secure boot 绕过潜力凸显了启动链信任根的现实脆弱性。局限在于模糊测试对需特定外设硬件触发的解析路径覆盖有限，且分析主要聚焦内存安全、未涵盖逻辑类漏洞；与既有固件/内核模糊测试相比，其价值在于聚焦启动链这一高价值且常被忽视的代码层。

- **论文 PDF**: [A Comprehensive Memory Safety Analysis of Bootloaders](https://www.ndss-symposium.org/wp-content/uploads/2025-330-paper.pdf)

### 硬件与物理安全（16 篇）

#### Secret Spilling Drive: Leaking User Behavior through SSD Contention
- **作者**: Jonas Juffinger, Fabian Rauscher, Giuseppe La Manna, Daniel Gruss（格拉茨技术大学、Amazon）
- **方向**: 硬件与物理安全
- **解读**:
隐/侧信道可绕过架构安全边界，但现代商用SSD内部的时序信道此前无人研究；已有存储类信道（HDD 0.1bit/s、SmartSSD FPGA 0.066bit/s）要么极慢、要么依赖FPGA。

用io_uring+O_DIRECT从非特权用户态系统化分析12款NVMe PCIe SSD：超过SSD最大IO数会引发显著延迟尖峰；受害者最少读8-128块（典型32块）、写仅1块即可被检测；据此构建分时隐信道（发送方突发读无关块，接收方测RTT），自动阈值学习+起始序列检测，并验证DRAM缓存非信道根源、存在72°C热节流。

隐信道真容量最高1784bit/s（进程间）、1503bit/s（跨VM），平均964/524bit/s，比此前存储隐信道快5个数量级；开放世界网站指纹识别F1最高97.0%（平均92.9%），与中断类侧信道相当；20%噪声对进程场景影响可忽略。

首个无需FPGA、非特权、跨VM且与操作系统/CPU/DRAM无关的商用SSD时序信道，证明存储控制器是新的泄漏面。局限：需共享同一SSD、信道粒度粗（需突发多块）、噪声波动大时阈值学习可能失效；缓解需控制器固件层面干预。

- **论文 PDF**: [Secret Spilling Drive: Leaking User Behavior through SSD Contention](https://www.ndss-symposium.org/wp-content/uploads/2025-208-paper.pdf)

#### ReThink: Reveal the Threat of Electromagnetic Interference on Power Inverters
- **作者**: Fengchen Yang, Zihao Dan, Kaikai Pan, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **方向**: 硬件与物理安全
- **解读**:
光伏逆变器是将可再生能源直流电转换为电网交流电的关键设备，其电流与电压传感器是安全功率转换的基础。本文首次系统分析 PV 逆变器传感器的 EMI 脆弱性：尽管符合电磁兼容（EMC）标准，1 GHz 及以上的电磁干扰仍能造成测量错误并欺骗控制算法，威胁电网稳定运行。

根因有三：EMC 标准只覆盖传导干扰（0.15–30 MHz）与辐射干扰（30 MHz–1 GHz），未覆盖有效干扰频段；低通滤波器非理想，可放过高频信号；逆变器设计无意留下 EMI 后门（LCD 屏幕开口、PCB 寄生电容、非对称布局）。据此设计 ReThink，通过精心构造的 EMI 实现三类后果：拒绝服务（关机）、物理损坏（烧毁）与功率抑制（Damping）。

作者在 5 台市售千瓦级逆变器及真实农村微电网上，于 100–150 cm 距离、总功率 20 W 内成功验证三类攻击；分析 47 台逆变器发现 43 台采用易受攻击的两级功率转换拓扑。论文同时给出硬件（阻断 EMI 传输、屏蔽）与软件（测量篡改检测、修复控制逻辑）对策。

该工作将 CPS 攻击面拓展到可再生能源功率电子设备，揭示"EMC 合规不等于对抗性安全"，对 RES 主导电网的未来具有紧迫现实意义。局限：攻击需物理邻近（1–1.5 m），验证设备规模有限；传感器仅 5 V 的低电压工作特性放大了威胁，后续可推广至风电变流器、储能等同类设备。

- **论文 PDF**: [ReThink: Reveal the Threat of Electromagnetic Interference on Power Inverters](https://www.ndss-symposium.org/wp-content/uploads/2025-691-paper.pdf)

#### PowerRadio: Manipulate Sensor Measurement via Power GND Radiation
- **作者**: Yan Jiang, Xiaoyu Ji, Yancheng Jiang, Kai Wang, Chenren Xu, Wenyuan Xu (Zhejiang University; Peking University)
- **方向**: 硬件与物理安全
- **解读**: 传感器是家庭入侵检测、环境监测等应用的关键，现有物理注入攻击各有短板：无线EMI攻击距离短（图像传感器约50 cm、麦克风约3 m）且需视距，激光/声波受视距限制，有线攻击需控制电源。论文提出PowerRadio，利用家庭电网互联的地线（GND）在远距离、无视距条件下绕过软件防御与物理防护操纵传感器读数。

根因分析指出两条：GND与数据信号线间缺乏屏蔽，以及电路阻抗不对称使干扰绕过滤波器——注入GND的共模（CM）信号以近乎无穷的负载阻抗传输、几乎无能量损耗，经耦合转换为差模干扰进入模拟测量链路；攻击者利用放大器非线性、比较器过敏感、ADC采样失真三类电路漏洞，设计静态偏置、周期信号、脉冲三种注入方法（如对麦克风载波370 kHz调制语音实现不可闻命令注入）。

实验显示攻击距离达15 m（跨插座、跨房间）；8款监控摄像头全部可被干扰、其中6款实现100%规避Facenet/Yolov8检测（如HIKVISION 99.0%/100.0%）；7款商用麦克风全部可注入不可闻语音命令（W2V相似度约0.6-0.7、Levenshtein距离0）；13个电子模块与17类传感器（9类）普遍受影响（如AD623输出从2.5 V被篡改到3.89 V，LM386偏差98.7%）。

该工作揭示"地线即攻击信道"这一被忽视的现实威胁面，把攻击者从"贴近设备"解放为"找个公共插座即可"，15 m距离显著优于无线EMI的50 cm，根因分析（屏蔽缺失+阻抗不对称）对防护设计有直接指导价值；局限在于需物理接触同一电网的插座，对高屏蔽/差分设计设备效果下降，且防御建议（共模滤波、布线优化、检测模块）的工程可行性尚待验证。与既有EMI/音频注入工作共同构成完整攻击谱系。
- **论文 PDF**: [PowerRadio: Manipulate Sensor Measurement via Power GND Radiation](https://www.ndss-symposium.org/wp-content/uploads/2025-295-paper.pdf)

#### Power-Related Side-Channel Attacks using the Android Sensor Framework
- **作者**: Mathias Oberhuber, Martin Unterguggenberger, Lukas Maar, Andreas Kogler, Stefan Mangard (Graz University of Technology)
- **方向**: 硬件与物理安全
- **解读**: PLATYPUS证明软件级功耗侧信道可窃密，但其直接功耗接口被限制；Hertzbleed改用功耗依赖的时序信号。当直接与间接信号都被堵住后，是否还存在其他可被无特权应用利用的功耗相关信号？Android向无特权应用和浏览器暴露的传感器框架正是潜在泄漏面。

论文系统分析了9台设备的137个无特权可访问传感器，发现其寄生捕获功耗相关物理影响；按三类泄漏分类（CPU利用率级、指令序列级、数据操作数级）并用相关性量化泄漏强度；首次深入分析地磁旋转矢量传感器的"旋转相关功耗泄漏"新原语（随设备朝向变化，源于内部磁力计与CPU负载的耦合）；据此实现两个端到端案例：Chrome中JS传感器框架的跨源像素窃取，以及本地应用对ARM NEON硬件AES的CPA攻击。

部分传感器与真值相关性超过0.9（Pixel 6a磁力计与CPU利用率0.973，压力传感器指令级泄漏0.955，二者呈互补分布）；极端情况下CPU压力使指南针应用指针偏转约30°；像素窃取速率5-10秒/像素、有效绕过浏览器跨源隔离；AES CPA在300,000样本内恢复单个密钥字节；已向Google披露，Chrome已对磁力计接口增加舍入、并计划为方向传感器增加权限提示。

该工作把功耗侧信道攻击面从桌面CPU扩展到数十亿台Android设备，且无需任何特殊权限，威胁模型新颖；像素窃取虽慢但足以窃取敏感界面内容，AES攻击仅恢复部分字节，数据级泄漏仍受噪声限制；Google的缓解（舍入+权限）属打地鼠式补丁，传感器寄生泄漏的根因难以根除。与Hertzbleed一脉相承，提示平台厂商应重新审计所有"环境感知"接口的侧信道风险。
- **论文 PDF**: [Power-Related Side-Channel Attacks using the Android Sensor Framework](https://www.ndss-symposium.org/wp-content/uploads/2025-92-paper.pdf)

#### PhantomLiDAR: Cross-modality Signal Injection Attacks against LiDAR
- **作者**: Zizhi Jin, Qinhong Jiang, Xuancun Lu, Chen Yan, Xiaoyu Ji, Wenyuan Xu (Zhejiang University)
- **方向**: 硬件与物理安全
- **解读**:
LiDAR是自动驾驶的核心传感器，以往信号攻击都走激光通道攻击ToF光电传感器；能否用跨模态的有意电磁干扰（IEMI）攻击LiDAR内部模块、开辟新攻击面，此前缺乏系统探索。

作者对五款商用LiDAR（VLP-16、RS-16、RS-Bpearl、RS-M1、RS-M1P）进行500MHz–3.5GHz扫频，发现两个新攻击面：监控传感器（温度、霍尔）与光束转向模块的光学编码器；结合FDD故障机制实现点云干扰、点云删除、LiDAR断电，并用幅度调制（AM）把伪造回波注入接收电路实现可控点注入。

点干扰可造成最大约10cm测距误差（优于前人的4cm）；1040–1070MHz可令VLP-16彻底断电（转速600→19 RPM，降96.7%）；点注入在VLP-16上超过16,000个假点，是激光SOTA（<3,000）的5倍；隐藏攻击1.5米内任意角度成功、最远5.5米，瞄准容忍达40°垂直/35°水平；断电距离为30cm/50cm，移动场景（跟车、路边）验证可行。

首次提出监控传感器与光学编码器两类攻击面，IEMI无需精确瞄准且覆盖近360°水平范围，明显优于激光攻击。局限：攻击设备昂贵（信号源3.2万美元、功放2万美元）、断电距离很短、需预先掌握目标型号。防御方向是完善EMC标准与传感器融合（实验显示融合模型更鲁棒）。

- **论文 PDF**: [PhantomLiDAR: Cross-modality Signal Injection Attacks against LiDAR](https://www.ndss-symposium.org/wp-content/uploads/2025-997-paper.pdf)

#### On the Realism of LiDAR Spoofing Attacks against Autonomous Driving Vehicle at High Speed and Long Distance
- **作者**: Takami Sato, Ryo Suzuki, Yuki Hayakawa, Kazuma Ikeda, Ozora Sako, Rokuto Nagata, Ryo Yoshida, Qi Alfred Chen, Kentaro Yoshioka (UC Irvine & Keio University)
- **方向**: 硬件与物理安全
- **解读**:
LiDAR欺骗攻击此前只在受控低速、短距离环境验证（最高5 km/h、15米内），对带脉冲指纹的新一代LiDAR基本失效，更从未对真实AD软件栈控制的车辆演示过端到端安全后果。

论文构建移动车辆欺骗系统（MVS）：红外相机+905nm带通滤波检测LiDAR激光光源（YOLOv5+卡尔曼滤波跟踪）、高精度舵机自动瞄准、双2英寸透镜阵列激光器+抛物面镜天线组成欺骗器（总成本约2,300美元）；并提出A-HFR攻击，利用目标LiDAR扫描时序的灰盒知识（弱同步）只在扫描目标时高频发射，把有效频率提高25倍以绕过脉冲指纹而避免激光二极管过热。

红外检测在20米外成功率≥90%（视觉方案仅5米）；A-HFR在AT128（15MHz）与XT32（24MHz）上分别移除100%与96%的点，而HFR最多仅20%；60 km/h实车实验中HFR到制动距离（20米）仍保持≥96%攻击成功率，A-HFR到40米达100%；注入攻击可注入≥1,000个点；在Autoware.ai控制的车辆上，删除攻击使车辆撞上充气假车，注入攻击使其在幽灵墙前停车。

首次把LiDAR欺骗推进到60 km/h、110米的现实场景并打通端到端AD安全影响，证明脉冲指纹可被灰盒高频攻击绕过。局限：A-HFR在20米内成功率下滑（光学接收漏检）、AD栈演示速度仅5–15 km/h且夜间进行；攻击仍需知道目标型号与扫描时序。防御需在激光认证复杂度与人眼安全之间寻找新平衡。

- **论文 PDF**: [On the Realism of LiDAR Spoofing Attacks against Autonomous Driving Vehicle at High Speed and Long Distance](https://www.ndss-symposium.org/wp-content/uploads/2025-628-paper.pdf)

#### On Borrowed Time – Preventing Static Side-Channel Analysis
- **作者**: Robert Dumitru, Thorben Moos, Andrew Wabnitz, Yuval Yarom (Ruhr University Bochum & UCLouvain & Defence Science and Technology Group)
- **方向**: 硬件与物理安全
- **解读**:
静态侧信道攻击（静态功耗SCA、激光逻辑态成像LLSI、阻抗分析IA）利用电路停钟后的静态状态以高精度、低噪声提取密钥，现有掩码与逻辑均衡等对策成本高昂且往往被绕过，整个攻击类别缺乏有效防御。

Borrowed Time观察到此类攻击都要求秘密数据在停钟状态下稳定保持数百微秒以上；对策持续监视时钟，检测到停钟后在一个时钟周期内用随机数覆写敏感寄存器（掩码清零，避免清零动作引入动态泄漏）。提供两种实现：基于PLL（LOCKED信号）适合常规设计，基于异步延迟链采样适合时钟门控低功耗设计；随机源用Trivium。

对无防护AES（Kintex-7）约1,500条迹线即可恢复完整密钥；一阶掩码SKINNY（Spartan-6）也需16,000条（掩码经1亿条动态+100万条静态TVLA验证无泄漏）；记忆效应实验显示偏移小于200µs时攻击不可行，而Borrowed Time在MHz频率下1µs内完成检测与清除；启用后即便100万条迹线也观察不到泄漏。

以极简电路堵住整类静态SCA的共性前提，成本远低于掩码（功率开销258–317%+）或均衡（463–638%+），且能与现有防护叠加，对LLSI/IA同样有效。局限：要求密钥主存储本身安全，误报会导致加密中断，异步方案需逐工艺手工调校、可移植性差。工程化可借REBECCA自动化定位敏感寄存器。

- **论文 PDF**: [On Borrowed Time – Preventing Static Side-Channel Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-1243-paper.pdf)

#### Non-intrusive and Unconstrained Keystroke Inference in VR Platforms via Infrared Side Channel
- **作者**: Tao Ni, Yuefeng Du, Qingchuan Zhao, Cong Wang
- **方向**: 硬件与物理安全
- **解读**:
VR 平台的虚拟键盘输入（如登录密码）面临侧信道窃取风险。以往攻击依赖在头显中植入恶意软件读取传感器、录制手势视频，或利用 Wi-Fi CSI、声学信号等，均受限于特定场景或受控条件。本文首次披露主流 VR 平台星座追踪系统中控制器红外（IR）LED 发出的信号可被外部窃听，构成新的非侵入式侧信道。

作者提出 VRecKey：用约 120 美元的 4×10（40 个）IR 传感器阵列在 2–4 米外截获控制器泄漏的红外信号；以波动偏度识别打字事件（99.3%），依多传感器响应时差校准键盘坐标，提取时频特征生成热力图、去除"图像残留"，再用边界框中心法重建击键轨迹、估计打字速度区分连续相同字符，最后调用 ChatGPT 做零样本纠错（T-1/T-3 各提升 5.4%/3.3%）。

在 Meta Quest 2 与 PICO 4、25 名受试者上，字符级识别 T-1/T-3 为 85.8%/94.2%，单词级为 81.7%/90.5%；在隐藏（单向膜）、反射（非视距）与低可见度（<0.1 lux）三种真实场景下性能基本保持；随距离增大性能下降，超过 5 米无法捕获。

该工作的成色在于发现了一个近乎免费、无需训练模型、可推广到所有星座追踪 VR 平台的攻击面，与依赖 DNN 分类的既往研究正交互补。局限：有效距离有限、需物理部署传感器阵列、LLM 纠错可能改写原意；作者也讨论了 Apple Vision Pro 等无控制器设备。总体为 VR 输入安全提供了新威胁建模视角，并提示厂商关注控制器 IR 信号的调制与功率策略。

- **论文 PDF**: [Non-intrusive and Unconstrained Keystroke Inference in VR Platforms via Infrared Side Channel](https://www.ndss-symposium.org/wp-content/uploads/2025-96-paper.pdf)

#### LLMPirate: LLMs for Black-box Hardware IP Piracy
- **作者**: Vasudev Gohil, Matthew DeLorenzo, Veera Vishwa Achuta Sai Venkat Nallam, Joey See, Jeyavijayan Rajendran
- **方向**: 硬件与物理安全
- **解读**: 芯片设计全球化使硬件 IP 盗版成为严重威胁：美光 2018 年估计仅 DRAM 领域 IP 盗版损失即达 87.5 亿美元，USTR 报告美国 IP 被盗损失达 2250 至 6000 亿美元。现有 GNN4IP 等盗版检测工具未被充分检验。论文首次探索此前无人涉足的攻击向量——利用 LLM 重写硬件设计（Verilog 网表）以规避盗版检测。

LLMPirate 是首个基于 LLM 的端到端自动化 IP 盗版框架，针对性解决三大挑战：LLM 训练数据中 Verilog 极少，方案 A 把网表语法翻译为更通用的布尔函数格式再提示模型；大电路超出上下文窗口，方案 B 先特征化网表提取所有独特门类型，再用分治策略逐类重写；响应不确定易出错，方案 C 采用交互式多轮尝试并反馈语法与功能正确性信息，全程仅需黑盒访问 LLM。

用 8 个 LLM（GPT-4、GPT-3.5、CoPilot、Claude、Gemini、Llama3-8B、CodeLlama-7B/13B）在 32 个基准网表上对 4 个检测工具（GNN4IP、MOSS、Jplag、SIM）评估，除 SIM（81.25%，因其原生不支持 Verilog、误报率高）外均实现 100% 规避。GPT-4 与 CoPilot 表现最佳，CodeLlama-7B 与 Llama3-8B 仅规避 10/32 与 11/32，但反馈式交互使 Llama3-8B 提升达 700%。案例研究成功盗版 IBEX、MOR1KX 处理器（约 15.8 万门）与 GPS 模块，运行时仅需数分钟。

该工作首次系统证明 LLM 能自动化、规模化地完成硬件 IP 盗版，并对四种检测算法做横向对比，揭示现有检测工具面对"功能等价、结构改写"时集体失效。局限在于方案针对门级网表，对 RTL 级或混淆电路的效果未验证；各 LLM 能力差距悬殊，小型开源模型成功率低。本质上这是一次红队压力测试，应推动检测工具引入结构语义等价性检测，而非仅依赖相似度度量。
- **论文 PDF**: [LLMPirate: LLMs for Black-box Hardware IP Piracy](https://www.ndss-symposium.org/wp-content/uploads/2025-2059-paper.pdf)

#### LightAntenna: Characterizing the Limits of Fluorescent Lamp-Induced Electromagnetic Interference
- **作者**: Fengchen Yang, Wenze Cui, Xinfeng Li, Chen Yan, Xiaoyu Ji, Wenyuan Xu
- **方向**: 硬件与物理安全
- **解读**: 传统 EMI 攻击要求攻击者携带专用金属天线在目标数米内部署，装备醒目、易被察觉。荧光灯在家庭、医院、工厂几乎无处不在，论文揭示这些日常光源可被当作"天线"，以非接触方式操纵附近 IoT 设备，实现隐蔽 EMI 攻击——例如向语音助手注入"开门"指令，或篡改医院婴儿恒温箱温度传感器读数。

LightAntenna 不修改荧光灯本身：攻击者通过电力线注入精心设计的高频信号，激活灯管内等离子体充当发射天线。论文系统表征了机理：EMI 主要来自灯管而非镇流器，镇流器工作频率决定本征 EMI（约 20-50 kHz），但注入 700 MHz 至 1.5 GHz 信号可激发 MHz 级响应。攻击端设计 DC/AC 耦合器注入信号、LPF 隔离电网，提出单频篡改攻击与 AM 调制操纵攻击两类信号构造方法，实现从数据扰动到精确数值控制。

8 种传感器模块均被篡改：模拟传感器偏差率超 25%（如 PT100 温度偏差 -41.5%、ACS712 电流偏差 +164%），数字传感器可被翻转输出；对工业 PT100 热电偶可将其读数可控地压至 15.6/5.6 °C 或抬至 41.2/53.3 °C；以 112 MHz 载波向鹅颈麦克风注入语音，三个商用语音识别 API 识别率达 76%-87.1%；在 10-20 m 距离向电网注入信号仍能影响热电偶；功率 10W→15.8W 使 EMI 强度增 8dB。

该工作首次揭示"灯即天线"这一全新隐蔽信道，威胁模型新颖，通用性、实用性、可调性与远程性评估全面。局限在于攻击者需先对目标传感器做频率扫描预测试，跨相位传输不可行，电网负载与线路影响大，且需灯管与目标相距约 15 cm。相比 GhostTalk、Tuner 等超声或金属天线攻击，隐蔽性大幅提升；防御建议（高阶滤波、加强屏蔽、修订 EMC 标准）务实但落地缓慢，风险持续。
- **论文 PDF**: [LightAntenna: Characterizing the Limits of Fluorescent Lamp-Induced Electromagnetic Interference](https://www.ndss-symposium.org/wp-content/uploads/2025-2334-paper.pdf)

#### GhostShot: Manipulating the Image of CCD Cameras with Electromagnetic Interference

- **作者**: Yanze Ren, Qinhong Jiang, Chen Yan, Xiaoyu Ji, Wenyuan Xu (Zhejiang University)
- **方向**: 硬件与物理安全
- **解读**: CCD传感器在医疗诊断、安防、工业检测、天文等高质量成像场景中不可替代，其成像可靠性是可信计算机视觉系统的基础。先前工作仅证明可用定向电磁干扰（IEMI）在黑暗环境下注入难以察觉的像素变化，而本文提出GhostShot，能在正常光照下向CCD相机注入任意灰度甚至彩色图像，威胁从"破坏"升级为"伪造场景"。
作者先从形态、亮度、色彩三方面做了干扰因果的机理分析，指出通过精心设计EMI信号的幅度与相位可有效控制注入图案的形状、亮度和颜色；随后设计了端到端攻击流程（含同步与幅相调制），并推导了攻击频率、条纹宽度等理论关系。
在屏蔽室内对15款商用CCD相机（9款模拟CCTV+6款数字相机）全部验证成功；攻击角度、距离、环境亮度对效果的影响符合理论预期。案例研究显示，医疗诊断（Camelyon16+DSMIL）中精度从0.66跌至0.40，火情检测（Yolov5/FireNet）中精度从约0.79跌至0.15-0.18，且能伪造QR码、误导夜间目标检测、欺骗人眼并实现动态视频注入。
我认为这是电磁注入攻击能力的显著跃升：从"不可注意的扰动"到"任意可控图像"，机理分析也使其具备可迁移性。局限在于攻击需要专业信号发生器（USRP+放大器）、近距离（7cm实验距离）与屏蔽环境下的频率扫描，现实中可部署性受限；且未讨论防御对策。与先前IEMI工作相比，色彩与灰度的精细控制是真正的增量。

- **论文 PDF**: [GhostShot: Manipulating the Image of CCD Cameras with Electromagnetic Interference](https://www.ndss-symposium.org/wp-content/uploads/2025-2065-paper.pdf)

#### GadgetMeter: Quantitatively and Accurately Gauging the Exploitability of Speculative Gadgets

- **作者**: Qi Ling, Yujun Liang, Yi Ren, Baris Kasikci, Shuwen Deng
- **方向**: 硬件与物理安全
- **解读**: 推测执行攻击难以彻底防御且缓解开销巨大（如 Retbleed 补丁使 ESXi 性能下降最多 70%），业界用扫描器定位脆弱代码做选择性缓解，但现有扫描器只关注信息流、忽视时序属性，把"授权与泄漏指令能装进 ROB"当作可利用的充要条件，误报严重。根本的时序条件是 gadget 内部授权与泄漏指令之间的竞态。GadgetMeter 基于时序属性定量评估 gadget 的可利用性。

方法上，用有向无环指令图（iDAG）建模时序条件（时序指数=授权指令最长路径权值−泄漏指令最长路径权值）；系统性探索攻击者的"窗口化能力"，发现 LLC 集合驱逐与 FP/INT 除法单元争用最强——实测 SMT 除法争用可延迟受害者除法指令约 200 个周期（纠正先前"数周期"的测量），并证明无内存依赖分支的 gadget 亦可利用；用解析模型在组合搜索空间中选择最优攻击模式（99.28% 的真实 gadget 少于 5 个操作）；最后运行时插桩测量推测窗口与泄漏时长，给出可行性分数 score=10·P[S>D]。

在 Kocher 15 例、扩展 15 例与 JSMN 15 例三个基准上，GadgetMeter 精度/召回/准确率均 100%，远超指令计数（准确率 64.4%）、周期精确模拟（44.4%）、内存分支检测（64.4%）三种基线。在 Brotli、HTTP3、JSMN、LibHTP、LibYAML、OpenSSL 六个应用与 Linux 内核上：503 个 gadget 得 0 分不可利用、852 个得 10 分高度可利用、3390 个介于 1-9 分；累计将 SOTA 工具报告的 471 个 gadget 判定为不可利用，欠/过估计率仅 0.67%/0.63%（基线最高达 51.10%）。

我的思考：GadgetMeter 把"有无 gadget"推进到"可利用几分"，为选择性缓解提供量化优先级依据，窗口化能力探索填补了扫描器侧的认知空白。局限：范围限于 PHT 类攻击，未覆盖 BTB/RSB/STL；仅聚焦缓存驱逐与除法争用；分数基于插桩近似。作为扫描器的"第二道筛子"，其实用价值明确。

- **论文 PDF**: [GadgetMeter: Quantitatively and Accurately Gauging the Exploitability of Speculative Gadgets](https://www.ndss-symposium.org/wp-content/uploads/2025-1723-paper.pdf)

#### EMIRIS: Eavesdropping on Iris Information via Electromagnetic Side Channel

- **作者**: Wenhao Li, Jiahao Wang, Guoming Zhang, Yanni Yang, Riccardo Spolaor, Xiuzhen Cheng, Pengfei Hu (Shandong University)
- **方向**: 硬件与物理安全
- **解读**: 虹膜识别凭借高熵、终身稳定和低误匹配率，被用于ATM、门禁、移动设备乃至Aadhaar等国家级身份计划，被视为最安全的生物特征之一。但虹膜设备依赖近红外（NIR）传感器采集数据，而传感器数据传输电路产生的电磁辐射可能泄露虹膜信息。本文提出EMIRIS，首次通过电磁侧信道从商用虹膜识别设备重建虹膜图像。
作者先解构NIR传感器的数字信号传输格式与虹膜数据矩阵的映射机制，把一维时域EM信号对应到二维虹膜灰度矩阵；随后将虹膜纹理的降噪与修复建模为线性逆问题，定制扩散模型提升重建质量；同时通过去噪策略对抗距离增大带来的信号衰减，扩展攻击距离。
实验表明EMIRIS在商用虹膜识别设备上实现平均SSIM 0.511、平均FID 7.25的重建质量；更严峻的是，重建虹膜能以53.47%的平均成功率欺骗经典虹膜识别模型，测试规模为50个用户的3000余个虹膜样本。
我认为这是生物特征侧信道研究的重要突破：虹膜本被认为是抗伪造的生物特征，却被远距离电磁泄漏击穿，说明"感知层物理安全"的假设需要重估。局限是SSIM 0.511的纹理保真度仍有限、攻击距离与硬件要求未完全披露，且对活体检测（liveness detection）的对抗能力未验证。相比RGB相机EM重建工作，这是首次针对NIR传感器与虹膜场景。

- **论文 PDF**: [EMIRIS: Eavesdropping on Iris Information via Electromagnetic Side Channel](https://www.ndss-symposium.org/wp-content/uploads/2025-200-paper.pdf)

#### Crosstalk-induced Side Channel Threats in Multi-Tenant NISQ Computers

- **作者**: Ruixuan Li, Chaithanya Naik Mude, Sanjay Das, Preetham Chandra Tikkireddi, Swamit Tannu, Kanad Basu

- **方向**: 硬件与物理安全

- **解读**: 量子计算正快速走向实际应用，但量子资源昂贵且利用率低，促使访问模式从单用户转向多租户共享；在多个用户共享一台量子计算机的环境中，保护用户机密性成为关键——一个用户编码的敏感数据可能被其他用户窃取。本文强调须在现实威胁模型下研究这些安全挑战，即敌手用户不依赖物理访问量子计算机或恶意云服务等高特权。

方法上，作者首次在 NISQ（含噪声中等规模量子）机器上演示将串扰（crosstalk）作为攻击向量：敌手以最小且现实的对抗权限发起侧信道攻击，目标是揭露受害者正在执行的量子算法；攻击使用串扰签名估计受害者电路中的 CNOT 门，再将该信息编码并由图学习模型分类，从而识别受害者的量子算法。

在最多 336 个基准电路上的评估中，攻击框架能以最高 85.7% 的准确率揭示受害者的量子算法。

我的思考：把量子硬件的物理噪声（串扰）转化为信息泄漏，是多租户量子计算安全研究的开创性工作——此前威胁模型多聚焦经典层或量子加密协议，本文证明物理层的串扰可直接泄露算法结构；85.7% 的识别准确率说明泄漏是实质性而非理论性的。局限是仅针对 CNOT 门分布的粗粒度识别，对更复杂电路与新型量子硬件的泛化性、以及缓解措施（隔离调度、噪声屏蔽）的代价需进一步研究。该工作为量子云服务商的安全调度提供了重要警示。

- **论文 PDF**: [Crosstalk-induced Side Channel Threats in Multi-Tenant NISQ Computers](https://www.ndss-symposium.org/wp-content/uploads/2025-2185-paper.pdf)

#### CCTAG: Configurable and Combinable Tagged Architecture

- **作者**: Zhanpeng Liu, Yi Rong, Chenyang Li, Wende Tan, Yuan Li, Xinhui Han, Songtao Yang, Chao Zhang

- **方向**: 硬件与物理安全

- **解读**: 内存安全违规是现实程序中的重大隐患，业界发展出多种缓解方法；但现有成本高效的防御保护有限、可被复杂攻击绕过，因此需要组合多种防御。然而组合防御往往带来性能下降与兼容性问题。本文提出 CCTAG，一种轻量级架构，简化多种基于标签的防御机制的集成。

方法上，CCTAG 提供可配置的标签验证与修改规则，用以构建多样化的安全策略，作为防御应用的基础原语；其策略中心的面具（mask）设计提升灵活性并防止策略冲突，使多种防御机制可并发运行；原型基于 RISC-V 在 FPGA 上实现。

评估显示 CCTAG 硬件开销极小：LUT 仅增加 6.77%、FF 增加 8.02%；组合返回地址保护、代码指针与 vtable 指针完整性、内存着色等防护后，SPEC CPU CINT2006 与 CINT2017 基准的运行时开销分别仅 4.71% 与 7.93%；针对覆盖主要内存安全漏洞类型的 CVE 与多种利用技术的安全评估验证了其缓解真实威胁的有效性。

我的思考：CCTAG 的贡献在于把"可配置"与"可组合"引入标签架构——此前标签架构多为固定策略，难以同时部署多种防御；策略中心的面具设计解决了多策略冲突这一组合防御的核心痛点，硬件与运行时开销都处于可接受区间。局限是 FPGA 原型与商用处理器的差距、对存量软件生态的兼容性仍需观察。与 CHERI 等硬标签方案相比，CCTAG 以配置性换取覆盖多种内存安全策略的灵活性，为防御组合提供了统一底座。

- **论文 PDF**: [CCTAG: Configurable and Combinable Tagged Architecture](https://www.ndss-symposium.org/wp-content/uploads/2025-862-paper.pdf)

#### A Systematic Evaluation of Novel and Existing Cache Side Channels
- **作者**: Fabian Rauscher, Carina Fiedler, Andreas Kogler, Daniel Gruss（Graz University of Technology）
- **方向**: 硬件与物理安全
- **解读**:
CPU 缓存是研究最多的侧信道目标，Prime+Probe 与 Flush+Reload 最为著名，可泄露密钥、用户输入；但各攻击缺乏系统比较，且新一代 Intel CPU 的缓存结构变化（如非包含性 L3）正改变攻击可行性。

论文首次用 9 个特征系统评估 4 种主流攻击（Flush+Reload、Flush+Flush、Evict+Reload、Prime+Probe）及 3 种新攻击（Demote+Reload、Demote+Demote、DemoteContention），覆盖命中-未命中边界、时间/空间精度、盲区长度、信道容量与可检测性。

两种 Demote 攻击与既有攻击相当、部分更优：Demote+Reload 盲区比 Flush+Reload 小 60.7%，信道容量 15.48 Mbit/s、高 64.3%。并展示 Demote+Demote 破解 KASLR、Demote+Reload 放大功耗侧信道；非包含性 L3 下 DemoteContention 成为无需逆向寻址的可靠跨核攻击替代。

这是缓存侧信道稀缺的系统化基准，demote 新攻击与对新 CPU 的及时评估有较高实用价值。局限在于结论依赖特定 Intel 微架构，泛化性未覆盖。

- **论文 PDF**: [A Systematic Evaluation of Novel and Existing Cache Side Channels](https://www.ndss-symposium.org/wp-content/uploads/2025-253-paper.pdf)

### 移动与物联网安全（12 篇）

#### What’s Done Is Not What’s Claimed: Detecting and Interpreting Inconsistencies in App Behaviors

- **作者**: Chang Yue, Kai Chen, Zhixiu Guo, Jun Dai, Xiaoyan Sun, Yi Yang

- **方向**: 移动与物联网安全

- **解读**: 移动应用隐私问题突出，但现有安全分析方法要么以代码级结果呈现、普通用户难以理解，要么忽略用户对应用行为的感知，导致用户在不了解风险的情况下暴露隐私。本文从用户视角出发，目标是自动提取应用中与用户相关的行为，用通俗的自然语言解释给用户，让用户自行感知"应用实际行为"与"用户期望"之间的不一致（inconsistency），并评估其中风险。

方法上，InconPreter 分三步：(1) 行为提取——将行为定义为"API 调用序列 + 关联 UI 上下文"，基于控制流图构建调用图，并通过收集隐式调用清单与数据流分析补全隐式调用关系与数据处理逻辑关系，再依据数据操作类 API 命名约定筛选出用户可能关心的行为；(2) 不一致识别——比较代码语义与对应 UI 信息，如拍照功能中调用含 "location" 的 API 但 UI 上无位置相关说明，即判定为不一致行为；对无 UI 根节点的"自启动"行为也单独处理；(3) 行为解释——利用 API-权限链接图将敏感行为映射到少量权限，设计角色/任务提示词让 LLM（GPT-4）生成通俗解释与风险分析报告。

实验上，InconPreter 在 600 个应用标注数据集上不一致行为识别精度达 94.89%，优于 DeepIntent 的 90.06%；风险分析在主流 Android 恶意样本集上准确率 94.56%；用户研究对解释的可理解性与合理性评分分别为 4.04/5 与 4.15/5。对 2020–2024 年从 Google Play 抓取的 10,878 个应用（去重后），识别出 413 个应用中的 1,664 个风险不一致行为（累计下载超 43 亿），包括位置、短信、联系人泄露及未授权录音等；其中 322 个应用（77.97%）含"自启动"风险行为（740 个，占 44.47%）；"工具"类应用风险行为最多，位置访问最常见。历史趋势显示含风险不一致行为应用占比从 2010 年前后的 26.44% 降至近年 3.80%。LLM 对比中 GPT-4（召回 94.72%、准确率 93.33%）远优于 Llama-2（准确率仅 53.12%）。

我的思考：本文的独特贡献是立场转换——不替用户做判断，而是把行为"翻译"给用户，让用户自行评估风险，这与现有研究"替用户决定风险"的范式不同，更贴近 GDPR 的透明性精神。工程亮点是权限作为 LLM 理解的中间表示，巧妙绕过长 API 序列的冗余问题。相比 DeepIntent，它额外发现 280 个绑定 widget 的行为与 424 个无 widget 行为（如点击"开始录制"按钮却读取短信/联系人），覆盖了无需用户交互的自启动场景。局限：静态分析天然存在误报与遗漏（漏掉 86 个如 VIBRATE/WAKE_LOCK 类行为）；LLM 解释依赖 GPT-4，成本与可复现性受限，且弱模型（Llama-2）表现极差说明该管道对 LLM 能力敏感。总体而言，本文为"用户可感知的隐私分析"提供了一条务实路径，其大规模野外表征结果也是对 Android 生态隐私治理成效的有力实证。

- **论文 PDF**: [What’s Done Is Not What’s Claimed: Detecting and Interpreting Inconsistencies in App Behaviors](https://www.ndss-symposium.org/wp-content/uploads/2025-2290-paper.pdf)

#### Vulnerability, Where Art Thou? An Investigation of Vulnerability Management in Android Smartphone Chipsets

- **作者**: Daniel Klischies, Philipp Mackensen, Veelasha Moonsamy

- **方向**: 移动与物联网安全

- **解读**: 智能手机芯片组（由主处理器、图形/无线等多个协处理器、固件与驱动构成）默认掌握加密前的敏感数据与长期密钥，其漏洞后果严重——2023 年 Amnesty International 报告商业间谍软件 Predator 可通过三星芯片组漏洞免交互部署。但芯片组固件与驱动由芯片制造商（CM）而非 OEM 或 AOSP 开发，漏洞修复依赖 CM→OEM→用户的供应链协作，而此前学界只研究 Android 系统本身，芯片组漏洞的生命周期从未被系统调查。本文首次构建统一知识库，对芯片组漏洞管理的技术特征与组织流程做了大规模实证研究。

数据上，作者整合 NVD、CM 安全公告与 OEM 更新信息，建成覆盖四大芯片制造商（Qualcomm、Samsung、Mediatek、Unisoc）437 个芯片模型、3,676 个漏洞（2009 年 9 月–2024 年 4 月）与 38 个 OEM 共 6,866 个手机型号的知识库，市场覆盖率约 99%。围绕漏洞生命周期的四个阶段（引入、发现、补丁开发、OEM 部署）回答四个研究问题，关键发现：(1) 引入——新一代芯片模型中平均仅 7% 的漏洞是新引入的，93% 通过代码复用从上一代继承而来（中位数情况下新模型引入 0 个新漏洞），每代平均新增 6%、移除 9% 漏洞；芯片组固件/驱动是所有闭源产品中代码复用致漏洞比例最高的（接近 Firefox/OpenJDK），且需要向后兼容蜂窝、蓝牙、WiFi 等规范是重要诱因。(2) 严重性——固件漏洞 CVSS 中位数比驱动漏洞高 0.8 分（Kruskal-Wallis 检验 p=0.05 显著），实证了"固件更严重"的猜想。(3) 披露——业界公认的 90 天负责任披露期鲜被遵守：Samsung 在 90 天内仅修复 46.9%、Qualcomm 仅 19.9%，Samsung 需 185 天才能修复 95%；AOSP 月度安全公告常缺失芯片组漏洞信息。(4) 更新——中位 71 天后 50% 手机收到更新，但 95% 分位数达 266 天，同一漏洞并非同时到达所有手机，部分更新延迟超 8 个月，大量芯片组漏洞用户似乎永远收不到修复。

我的思考：本文填补了一个重要空白——把"漏洞供应链"实证化：漏洞的真正源头是芯片厂商闭源固件中的代码复用，这使单点漏洞以"数百至数千机型受影响"的规模放大。93% 继承率是震撼数字：芯片组安全现状本质上是历史代码债的积累，而非新功能引入的风险，这直接解释了为什么补丁协调如此低效。90 天披露期在芯片生态"系统性失灵"的实证，对安全研究社区有直接指导意义——研究者不应假设 90 天内可修复，披露决策需重新校准；同时 AOSP 公告信息缺失会让"受影响范围评估"失真。局限方面：知识库依赖 NVD/公告的披露数据，存在报告偏差（只有被发现的漏洞才可见）；OEM 更新信息的可获取性不均衡（仅 4/38 OEM 有完整变更日志），更新延迟估计可能偏乐观或偏保守。相比浏览器生态（50% 漏洞 7 天内修复），芯片组的差距凸显了多组织供应链治理的困难，其改进建议（增强 CM 内部安全团队、统一公告规范）对 IoT、联网汽车等同类多公司协作生态同样适用。

- **论文 PDF**: [Vulnerability, Where Art Thou? An Investigation of Vulnerability Management in Android Smartphone Chipsets](https://www.ndss-symposium.org/wp-content/uploads/2025-1161-paper.pdf)

#### Understanding Miniapp Malware: Identification, Dissection, and Characterization

- **作者**: Yuqing Yang, Yue Zhang, Zhiqiang Lin
- **方向**: 移动与物联网安全
- **解读**: 超级应用的小程序生态存在窃取隐私、滥用广告分成、借社交网络传播的恶意小程序，但社区一直缺少公开的恶意样本数据集，阻碍对这一威胁的认知与检测研究。

作者三年纵向监测微信小程序商店：首轮收集 4,595,680 个小程序，复查发现 360,467 个被平台下架；对下架样本施加"逃避审核签名（动态代码执行、云端变量控制的条件渲染）+ 已被下架"双重判据，得到 19,905 个恶意样本，抽样 500 个核验 487 个正确。

解剖出传播（商店/社交分享/跨小程序跳转）、激活（启动/用户触发/远端控制）、载荷三类生命周期；6 类载荷中授权绕过 4,360（21.91%）、激励分享类合计约 44%、广告轰炸 420；工具类目恶意样本最多（审核最宽松），恶意小程序 2019 年 1 月达峰值。

数据集的奠基价值大于方法本身，双重确认策略低误报务实；但纯静态分析、以"被下架"为恶意代理会漏掉未暴露样本，13/500 误报提示签名匹配有语义歧义，结论向百度、支付宝等平台推广需谨慎。

- **论文 PDF**: [Understanding Miniapp Malware: Identification, Dissection, and Characterization](https://www.ndss-symposium.org/wp-content/uploads/2025-82-paper.pdf)

#### UI-CTX: Understanding UI Behaviors with Code Contexts for Mobile Applications

- **作者**: Jiawei Li, Jiahao Liu, Jian Mao, Jun Zeng, Zhenkai Liang
- **方向**: 移动与物联网安全
- **解读**: 移动应用 UI 组件可被滥用执行有害行为（如仿冒"登录"按钮窃取凭证）；现有外观、权限、代码上下文表示要么区分度差（共享 INTERNET 权限），要么前向分析过度近似，且第三方库实现（88.4% 应用超 30% 调用来自外部 API）淹没核心意图。

UI-CTX 构建 UI Handler Graph：反向分析（事件→组件→布局）精确绑定事件回调，按分支条件剪枝组件不可达代码（21.99% 应用单个处理函数含多组件回调、平均 5.39 分支），对第三方库 API 做自底向上图摘要，用拉普拉斯最小特征值与节点 Dalvik 操作码统计嵌入后做层次聚类。

40,000 个应用、8 类 2,000 个标注组件上，UHG 平均 F1 比权限集提升 95.2%、比调用序列提升 8.2%（download 类 0.86 vs 0.47/0.76），无监督聚类准确率 95.0%，精确上下文定位带来 3.6 倍聚类提升；不剪枝时 V-measure 平均下降 28.9%。

把组件意图表示推进到"精确裁剪+摘要的代码图"，反向分析优于 GATOR 类工具，可直接嵌入现有移动安全分析流水线；但图嵌入较浅、库识别依赖 LibRadar（漏检 google.gson 会污染语义）、静态分析难处理反射，基准只覆盖 8 类组件且假设良性应用中组件文本即功能。

- **论文 PDF**: [UI-CTX: Understanding UI Behaviors with Code Contexts for Mobile Applications](https://www.ndss-symposium.org/wp-content/uploads/2025-238-paper.pdf)

#### The Skeleton Keys: A Large Scale Analysis of Credential Leakage in Mini-apps
- **作者**: Yizhe Shi, Zhemin Yang, Kangwei Zhong, Guangliang Yang, Yifan Yang, Xiaohan Zhang, Min Yang
- **方向**: 移动与物联网安全
- **解读**: 超级 App + 小程序范式（如微信、TikTok，全平台超 700 万小程序）中，超级平台向小程序服务器开放支付、云、分析等敏感服务，以凭证认证。但小程序开发者常把服务器侧凭证"迁移"到客户端，恶意用户可在 root 设备上截获滥用，威胁平台与服务器双方；此前仅有零星个案，凭证机制设计与风险未被系统理解。

作者先人工分析 21 个超级平台文档与示例，归纳出七类凭证并分为根凭证、访问凭证、加密凭证三类，发现它们多为非结构化、动态获取的。据此设计检测工具 KeyMagnet：先通过文档分析构建服务器侧"凭证使用语义图"（CSG）；再用细粒度数据流分析（JAW 静态分析 + UI Automator 动态爬虫）构建客户端行为图（CBG）；最后基于签名与上下文相似度（Levenshtein 距离，阈值 0.9）做语义子图匹配，判断服务器侧凭证语义是否出现在客户端。

在 6 个平台 413,775 个小程序上检出 84,491 个凭证泄露（覆盖 54,728 个小程序），精度 95.04%、召回 85.56%；泄露波及腾讯、百度等大公司及教育、政务等敏感类别，可导致全量用户账号劫持、支付欺诈、钓鱼广播与敏感数据窃取；已分配 89 个 CVE（38 个高危）。模板化开发放大了问题：同一模板导致 35 个小程序泄露根凭证，还存在跨超级平台、跨小程序的凭证泄露。

这是首个系统化的小程序凭证安全研究，语义图对比方法可迁移到其他 app-in-app 生态。局限是"凭证未被使用"的泄露检测不到（假阴），无关 API 结构相似造成假阳。其价值在于量化了该生态的系统性风险，并证明根因是"凭证迁移"这一反模式与开发者安全意识不足，为平台侧强化服务端认证与代码审计提供了数据支撑。
- **论文 PDF**: [The Skeleton Keys: A Large Scale Analysis of Credential Leakage in Mini-apps](https://www.ndss-symposium.org/wp-content/uploads/2025-273-paper.pdf)

#### Speak Up, I’m Listening: Extracting Speech from Zero-Permission VR Sensors
- **作者**: Derin Cayir (Florida International University), Reham Mohamed Aburas (American University of Sharjah), Riccardo Lazzeretti (Sapienza University of Rome), Marco Angelini (Link Campus University of Rome), Abbas Acar (Florida International University), Mauro Conti (University of Padua), Z. Berkay Celik (Purdue University), Selcuk Uluagac (Florida International University)
- **方向**: 移动与物联网安全
- **解读**: 
VR 设备已进入会议、讲座、培训等隐私敏感场景，而加速度计/陀螺仪属于零权限传感器，恶意后台应用无需用户授权即可读取，可窃听 HMD 扬声器播放的语音（卡号、SSN、电话等）。

IMMER SPY 攻击分三步：后台 App 以约 1000Hz 采集加速度计数据；预处理用 20Hz 高通滤波去除头部运动、以 STD 阈值检测语音段、零均值归一化；将三轴信号转为 Mel 谱图，输入 time-distributed 卷积加双向 LSTM 的 CNN-LSTM 模型分类数字。区分 informed（有受害者标注数据）与 uninformed 攻击者，后者可用 GenAI TTS（BARK、Amazon Polly 等 80 个合成声音）扩充训练集。

informed 攻击者识别数字准确率 85.6%（top-3 达 99%），uninformed 71.5%；连续 4 位数字（SSN 尾号）约 74–80%，16 位卡号 62–76%；头部运动移除使精度提升 29–39%；GenAI 扩充使 uninformed 提升至 82%；显著优于 Spearphone（27%）、AccelEve（72%）、EarSpy（29%）。防御方面，HMD 扬声器播放不可闻扫频音使攻击精度平均下降 70%。

首次在 VR 设备上实现基于扬声器振动的语音侧信道窃听，GenAI 造数据使攻击无需受害者先验即可实施，且更紧凑的 Quest 3 反而更易受攻击；局限是实验室环境与受限数字词典。不可闻噪声防御思路新颖，但需评估对用户体验与听力安全的影响。

- **论文 PDF**: [Speak Up, I’m Listening: Extracting Speech from Zero-Permission VR Sensors](https://www.ndss-symposium.org/wp-content/uploads/2025-164-paper.pdf)

#### ScopeVerif: Analyzing the Security of Android’s Scoped Storage via Differential Analysis
- **作者**: Zeyu Lei, Güliz Seray Tuncay, Beatrice Carissa Williem, Z. Berkay Celik, Antonio Bianchi（Purdue University、Google）
- **方向**: 移动与物联网安全
- **解读**:
Android 10引入的scoped storage彻底改变了应用访问共享存储的模型，但其安全实现缺乏系统化研究；已知SDK降级、SAF漏洞等绕过，跨版本与跨OEM实现不一致难以验证。

ScopeVerif将官方文档形式化为8条CIA安全规则，自动枚举API×动作×目标×属性×权限组合生成测试用例；采用分布式黑盒动态测试，违反预言机用差分分析（基线vs测试环境结果对比）判定违规；跨Android 12/13/14（Pixel）及三星、华为OEM对比。

搜索空间22217个用例，随机采样505个（2.27%）即找到与2501个（11.26%）相同的7类问题；共发现10类问题、9个此前未知，含Metadata Leak（无权限泄露文件存在性，可构成跨应用用户标识隐信道，谷歌确认）与SAF Loophole C（华为确认并授赏金）；每用例平均约28秒，4小时即可复现全部发现。

首个设备无关的scoped storage黑盒验证工具，用差分分析绕开庞大Android代码库，证实“文档-实现”漂移与OEM偏差普遍存在（10类问题仅3类一致）。局限：随机采样依赖“问题聚集”假设，违规分类需半自动人工分析。
- **论文 PDF**: [ScopeVerif: Analyzing the Security of Android’s Scoped Storage via Differential Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-340-paper.pdf)

#### MALintent: Coverage Guided Intent Fuzzing Framework for Android
- **作者**: Ammar Askar, Fabian Fleischer, Christopher Kruegel, Giovanni Vigna, Taesoo Kim
- **方向**: 移动与物联网安全
- **解读**: Android 中 Intent 是应用内与应用间组件通信的主要消息传递机制，跨越应用信任边界，可能破坏应用隔离。由于与内部通信共享同一 API，开发者常无意暴露本应仅内部使用的组件与代码路径，造成跨应用安全漏洞，相关 CVE 逐年增长。MALintent 旨在自动化地在闭源真实应用中找出这类缺陷。

MALintent 是首个对编译后闭源 Android 应用进行灰盒模糊测试的 Intent 模糊器。它通过静态与动态分析提取 Intent 规范（组件、action、data scheme 等）驱动意图变异；提出新的覆盖率插桩技术，兼容 Android 8-14；并实现三个 bug oracle：崩溃检测、基于动态污点分析的隐私泄露检测，以及针对经 JNI 调用的原生代码的内存安全检测，引擎基于 LibAFL 实现。

在 Google Play Top 应用与 500 个 F-Droid 应用上，覆盖率反馈在 81.8%（Google Play）与 56.5%（F-Droid）的应用上增加了边覆盖率，相对 IccDroid 等基线覆盖更多代码。去重后共发现 47 个崩溃、9 个隐私泄露与 1 个内存安全缺陷，并能复现先前工作发现的缺陷（Intents of Death 的 10 个崩溃、DroidFuzzer 的 14 个、Demissie 等的 9 个隐私问题）。除 3 个隐私 bug 与 12 个无人维护应用中的崩溃外，均获确认或修复。

成色在于首次把覆盖制导灰盒模糊测试落到闭源 Android 应用这一现实场景，三类 oracle 覆盖崩溃、隐私与原生内存安全，披露规范。局限：隐私 oracle 需修改 Android 系统本身，推广门槛高；JNI oracle 因环境模拟不完整存在假阳性。与 IccDroid、DroidFuzzer 等相比，MALintent 在覆盖与缺陷发现上全面领先，为后续 Android IPC 安全研究提供了开源基础。
- **论文 PDF**: [MALintent: Coverage Guided Intent Fuzzing Framework for Android](https://www.ndss-symposium.org/wp-content/uploads/2025-125-paper.pdf)

#### Hidden and Lost Control: on Security Design Risks in IoT User-Facing Matter Controller

- **作者**: Haoqiang Wang, Yiwei Fang, Yichen Liu, Ze Jin, Emma Delph, Xiaojiang Du, Qixu Liu, Luyi Xing
- **方向**: 移动与物联网安全
- **解读**: Matter 正成为智能家居统一互操作标准，苹果、谷歌、亚马逊、涂鸦等主流厂商自 2022 年底广泛支持，但厂商如何安全集成 Matter 及其风险未被系统研究。本文揭示一类新设计缺陷——UMCCI 缺陷（用户面向 Matter 控制能力与接口的设计缺陷），严重损害用户对设备的控制与知情能力，并提出三项安全需求 P1-P3（查看、可信、更新访问控制信息）。

方法上，基于三层开发模型（开源 CHIP SDK 层、厂商本地层、云层）归纳六类缺陷：Apple Home 隐藏 Keychain fabric（Vendor ID 4996）、SmartThings 按 Label 正则隐藏自身 fabric，恶意访客借此建立隐式控制通道；涂鸦将访客 ACL 误配为管理员级，可越权控制并降级主人权限（5 降到 1）；Alexa 无法查看/撤销 fabric，Signify WiZ 硬件重置不清理 Matter 状态；协议层 Vendor ID/Label 可伪造、缺乏 fabric 内节点查询与彻底重置能力。作者还构建 UMCCI Checker：用 Matter.js 虚拟设备自动配对各厂商 App，LLM 引导 UI 探索与缺陷检测。

结果显示六类缺陷在 11 个真实设备（8 家厂商）上全部复现，并已全部获 CSA 确认，苹果、Aqara 承诺修复，涂鸦已按建议修复。UMCCI Checker 基于 ChatGPT 3.5/4，仅 253 行 Python 与 844 词提示词，无需真实设备即可规模化检测。

我的思考：这是首个从用户视角系统剖析 Matter 集成安全的工作，"隐藏 fabric"与"ACL 降级"攻击隐蔽性强且契合 Airbnb 等共享场景，威胁现实。局限：工具依赖 LLM 的 UI 理解能力，设备覆盖有限。根因分析（厂商随意实践+协议设计不足）为 CSA 修订规范提供直接证据，也提示应用层标准应内建用户可验证的访问控制可见性。

- **论文 PDF**: [Hidden and Lost Control: on Security Design Risks in IoT User-Facing Matter Controller](https://www.ndss-symposium.org/wp-content/uploads/2025-266-paper.pdf)

#### Evaluating Machine Learning-Based IoT Device Identification Models for Security Applications

- **作者**: Eman Maali (Imperial College London), Omar Alrawi (Georgia Institute of Technology), Julie McCann (Imperial College London)
- **方向**: 移动与物联网安全
- **解读**: IoT设备数量激增且普遍共享硬件组件和潜在漏洞，网络运营商依赖设备识别来定位与归因漏洞，但现有机器学习识别方案大多假设静态环境，忽视真实网络中设备运行模式的多样性与行为随时间演化，且未考虑sFlow等流量采样带来的影响，导致部署性能显著退化。本文系统评估当前ML-based IoT设备识别方案的退化程度与成因。
作者从2017年以来的200余篇论文中筛出17篇代表性工作（14篇ML-based），复现其中10个SOTA方案，定义了三个实用性评估属性：运行模式（mode of operation）、可迁移性（transferability）、可观测性（observability），并在精选数据集与代表性特征上跨设置评估；随后用ML可解释性技术定位性能退化的关键原因，给出特征选择建议。
结果量化了退化幅度：空间迁移（相同设备处于不同环境）使性能下降7.5%至74%；时间迁移最早一周即退化19.32%，52周后最高达85.90%；sFlow采样流量使模型性能平均下降70.09%。研究发现没有单一特征能持续稳定识别设备，需持续训练与维护。
我认为这是该领域稀缺的"诚实评估"工作：绝大多数论文在自采数据集上报告高精度，掩盖了真实部署的脆弱性。其贡献在于把退化从轶事提升为可复现的量化结论，并给出可操作建议。局限是评估的10个方案与数据集覆盖仍有局限，且对新型深度学习方案的普适性待验证。相比单点方案论文，这类测量性质的研究对产业落地价值更高。

- **论文 PDF**: [Evaluating Machine Learning-Based IoT Device Identification Models for Security Applications](https://www.ndss-symposium.org/wp-content/uploads/2025-118-paper.pdf)

#### EAGLEYE: Exposing Hidden Web Interfaces in IoT Devices via Routing Analysis

- **作者**: Hangtian Liu, Lei Zheng, Shuitao Gan, Chao Zhang, Zicong Gao, Hongqi Zhang, Yishun Zeng, Zhiyuan Jiang, Jiahai Yang (Information Engineering University & Tsinghua University)
- **方向**: 移动与物联网安全
- **解读**: 隐藏Web接口（未在文档中披露但可访问的通道）在IoT设备中带来巨大安全风险：CVE-2023-3519（Citrix，CVSS 9.8）即隐藏接口中的栈溢出导致未授权远程代码执行。但隐藏接口定义模糊、无公开模式，静态分析与传统模糊测试都难以发现。本文提出EAGLEYE，通过路由分析自动暴露IoT设备中的隐藏Web接口。
关键观察是：固件常用特定路由机制（routing mechanism）把请求分发到处理函数，公共接口与隐藏接口共享相似的请求模式，仅动作或文件名（路由令牌）不同。EAGLEYE先分析公共接口请求识别路由令牌，再用大语言模型分析令牌上下文、归纳其公共模式并推断候选值，最后用候选值作为高质量字典，对路由令牌做隐藏接口定向的黑盒变异模糊测试。
在13款商用IoT设备上，EAGLEYE发现79个隐藏接口，是SOTA方案IoTScope的25倍；其中进一步发现29个未知漏洞，包括后门、XSS、命令注入和信息泄露，已获得7个CVE。
我认为"从公开接口反推路由令牌再枚举"的洞察优雅地解决了隐藏接口无模式可循的问题，LLM承担了此前依赖专家经验的模式归纳任务。局限是依赖公共接口的存在与固件路由机制的规律性，对无Web接口或高度混淆固件的设备效果未知；与IoTScope相比，"认证后隐藏接口"也被纳入定义，覆盖面更广。

- **论文 PDF**: [EAGLEYE: Exposing Hidden Web Interfaces in IoT Devices via Routing Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-399-paper.pdf)

#### Careful About What App Promotion Ads Recommend! Detecting and Explaining Malware Promotion via App Promotion Graph

- **作者**: Shang Ma, Chaoran Chen, Shao Yang, Shifu Hou, Toby Jia-Jun Li, Xusheng Xiao, Tao Xie, Yanfang Ye

- **方向**: 移动与物联网安全

- **解读**: Android 应用的开发者常放置推广其他应用的广告（app promotion ads）；但广告内容审查不足，使恶意开发者得以把推广广告用作恶意软件的新分发渠道。为检测经推广广告分发的恶意软件，本文提出 ADGPE：将应用 UI 探索与图学习协同整合，自动收集应用推广广告、检测被推广的恶意软件，并解释恶意软件采用的推广机制。

方法上，ADGPE 的 UI 探索技术自动发现并收集应用内推广广告，构建"推广关系图"（app promotion graph）；在此图上进行图学习，检测被推广应用是否为恶意软件；并通过路径推断揭示恶意软件的推广机制。

对 18,627 条推广广告的评估展示了推广生态的重大风险：从推广广告下载遇到恶意软件的概率比从 Google Play 下载高出数百倍；AdMob、Unity Ads、Applovin 等热门广告网络被恶意开发者利用以传播激进广告软件、流氓安全软件、木马与 fleeceware；UI 探索技术在相同时间内比最先进技术多发现 24% 的推广广告；基于推广关系的恶意检测模型 F1 从 SOTA 的 90.14% 提升至 95.31%（提升 5.17%）；还检测到 28 个最初被 VirusTotal 标记为良性、六个月后被标记为恶意/PUA 的应用；路径推断揭示两种推广机制——经硬编码广告的定制推广，以及经与广告服务器（如 AdMob、Applovin）交互的广告库推广。

我的思考：首次系统揭示应用推广广告生态中的恶意分发规模（风险数百倍于官方商店），把"推广关系"作为检测特征显著提升 F1，证明图结构信息比单应用特征更有判别力；对 28 个"延迟恶意"应用的发现提示了静默演化的恶意行为。局限是动态 UI 探索的覆盖度受应用复杂度限制，广告网络侧治理的配合度是落地关键。与基于应用自身特征的恶意检测相比，本文展示"广告生态级"证据的检测价值，为移动恶意分发治理提供了新抓手。

- **论文 PDF**: [Careful About What App Promotion Ads Recommend! Detecting and Explaining Malware Promotion via App Promotion Graph](https://www.ndss-symposium.org/wp-content/uploads/2025-51-paper.pdf)

### 云与供应链安全（8 篇）

#### Welcome to Jurassic Park: A Comprehensive Study of Security Risks in Deno and its Ecosystem

- **作者**: Abdullah AlHamdan, Cristian-Alexandru Staicu

- **方向**: 云与供应链安全

- **解读**: Node.js 与 npm 生态（超两百万包）漏洞频发、恶意包横行，供应链攻击屡禁不止。Deno 以 Rust 内存安全语言实现，内置严格权限系统（静态/运行时权限），并用 URL 直接导入第三方代码，宣称"安全默认"、提供去中心化供应链——被广泛视为 Node.js 的更安全替代。本文系统研究 Deno 是否兑现了安全承诺，首次对其攻击面、权限系统与 deno.land 生态进行全面的安全分析。

方法上分三部分：(1) 对照 Node.js 的已知漏洞类型逐一比对 Deno 攻击面，确认 Rust 实现消除了整类内存安全漏洞，且无安装期钩子（npm 供应链攻击的主要载体）；(2) 系统性绕过权限系统实验——发现粗粒度权限（如 --allow-sys）可借 OS 环境权限旁路检查（如经 /proc/self/environ 读环境变量）、URL 导入代码不受权限检查约束可发起越权网络请求、符号链接处理存在 TOCTOU 竞态使细粒度文件系统控制失效，ReDoS 未缓解、原型污染仅部分缓解；(3) 对 deno.land 全部 5,400 个包做实证测量：解析出 21 个域、10,544 个 URL（deno.land 占 85.15%，esm.sh 7.07%），发现 39 个包经不安全的 HTTP 传输、400+ 包传递性依赖不可用 URL（不可用链接中位数 220 个，直接波及 380 个包、传递波及 462 个）、21 个分发域中 1 个（lcas.dev）可被购买接管、13% 的 URL 未锁定版本、224 个包托管在可变域上（807 个包传递性引入可变依赖）。权限实践方面仅 1,123 个包在文档中声明所需权限，其中仅 53 个使用细粒度权限，33 个甚至建议全开权限；网络类 API 直接使用仅 237 个包，经依赖累积后达 2,714 个包。

研究促成两项安全公告（CVE-2024-21486、CVE-2024-21487），Deno 团队据此重新设计导入机制、承诺在 Deno 2.0 中对不可信域导入增加权限检查，并注册了问题域名。作者建议：新增导入权限、文件系统级访问控制、支持隔离（compartmentalization）、以清单文件持久化细粒度权限、禁止 HTTP 导入、镜像关键包以增强去中心化供应链冗余。

我的思考：本文的价值在于把"更安全的新运行时"置于严格审视之下，证明安全承诺需要系统性验证而非想当然：Rust 内存安全与去中心化导入带来真实收益，但权限模型的设计缺陷（粗粒度授权、URL 导入豁免、TOCTOU）被巧妙利用后，供应链攻击面依然可观。与 npm 生态研究相比，本文揭示了去中心化供应链独有的脆弱性——域接管、跨域不可变策略不一致、CDN 宕机级联，呼应 left-pad 教训的去中心化版本。实证数字（仅 4.7% 的文档化包用细粒度权限）量化了权限系统可用性不足这一根本问题：安全机制不好用，开发者就会绕过它。局限在于权限推断为粗粒度静态分析、缺少运行时验证；且研究对象是 2022 年的快照，Deno 2.0 落地后其结论需要重新检验。总体上是运行时安全研究的范本——"声称安全"必须经受攻击面+生态的双重实证。

- **论文 PDF**: [Welcome to Jurassic Park: A Comprehensive Study of Security Risks in Deno and its Ecosystem](https://www.ndss-symposium.org/wp-content/uploads/2025-284-paper.pdf)

#### Secure IP Address Allocation at Cloud Scale
- **作者**: Eric Pauley, Kyle Domico, Blaine Hoak, Ryan Sheatsley, Quinn Burke, Yohan Beugin, Engin Kirda, Patrick McDaniel（威斯康星大学麦迪逊分校、Northeastern University）
- **方向**: 云与供应链安全
- **解读**:
云中动态IP分配可被滥用：攻击者借IP重用发起恶意流量、绕过限速，或利用前任租户遗留的潜在配置捕获其流量；IP作为安全主体带来前瞻/回溯两种声誉与配置威胁。

构建EIPSIM模拟器：租户行为用日周期随机傅里叶级数建模（模拟自动伸缩），潜在配置按指数分布（dv~Exp(1/da)，pc=0.1）；提出比单租户更强的多租户（Sybil）攻击者；设计IP扫描分段（SEGMENTED）策略：跟踪每租户平均分配时长，为IP打标签并设冷却时间，分配时优先租户标签IP，再选冷却时间最接近t+α·d̄a的IP。

对多租户攻击者，SEGMENTED将潜在配置产出比RANDOM降低83.8%，比TAGGED额外降低70.1%；TAGGED在攻击者超20个租户时失效，SEGMENTED对无限租户仍稳健；α调优呈凸性、存在全局最优；总模拟量超250年。

首次为云IP分配建立系统化统计模型并防御更强的Sybil攻击者，弥补先前工作威胁模型过弱的缺陷，且策略可直接部署。局限：潜在配置的指数分布与攻击者固定10分钟时长等假设依赖模型；α需结合真实trace调优。

- **论文 PDF**: [Secure IP Address Allocation at Cloud Scale](https://www.ndss-symposium.org/wp-content/uploads/2025-374-paper.pdf)

#### Secure Data Analytics in Apache Spark with Fine-grained Policy Enforcement and Isolated Execution
- **作者**: Byeongwook Kim, Jaewon Hur, Adil Ahmad, Byoungyoung Lee（首尔国立大学、Arizona State University）
- **方向**: 云与供应链安全
- **解读**:
云Spark数据共享缺乏策略执行机制：恶意数据用户与云管理员可通过构造违规物理计划、篡改Spark库、攻陷worker节点三种途径窃取数据；现有方案不检查物理计划且不保护分析管线完整性。

LAPUTA由两部分组成：CHECKER用正则表达式在物理计划上做模式匹配（策略P=⟨目标表t, 模式r⟩，符号s=⟨操作符, 字段, 可选表达式⟩，支持通配符）；EXECUTOR用机密计算（AMD SEV-SNP）把Spark应用隔离为Processor（不可信用户代码）与Executor（策略检查+任务生成执行，enclave内），数据全链路加密并远程证明。

7条合成策略在TPC-H的22个查询中正确拦截9个违规查询，无漏报；平均35%延迟、25%吞吐开销；策略检查仅占增量延迟的0.68%，RPC传输+反序列化占约81%，机密计算本身仅增加约200ms。

首次在Spark物理计划层做一般化细粒度策略检查并保护整条管线（含用户自身），威胁模型强于Opaque等方案。局限：正则语言无法表达计数类约束，多字段节点匹配存在假阳性；VM级隔离开销较大。对医疗、金融数据共享有现实价值。

- **论文 PDF**: [Secure Data Analytics in Apache Spark with Fine-grained Policy Enforcement and Isolated Execution](https://www.ndss-symposium.org/wp-content/uploads/2025-2496-paper.pdf)

#### Rethinking Trust in Forge-Based Git Security
- **作者**: Aditya Sirish A Yelgundhalli, Patrick Zielinski, Reza Curtmola, Justin Cappos
- **方向**: 云与供应链安全
- **解读**:
Git 是使用最广的版本控制系统（93.87% 受访开发者），但其仓库完整性依赖 GitHub、GitLab 等中心化 forge 执行访问控制、活动审计与策略强制。由于缺少保证仓库完整性的开放协议，forge 无法自证可信，成为供应链中不可验证的可信第三方，单个特权用户或 forge 被攻陷即可破坏仓库安全。

作者提出 forge 无关的 gittuf，将仓库安全的三个方面去中心化：策略声明经带命名空间的委托（namespaced delegations）分发至多方并保证共识，无人可单方面改策略；活动追踪由全体用户共同维护认证、有序、追加式的引用状态日志（RSL），记录分支/标签推送、策略变更与代码评审批准，篡改可被检测；策略执行则由每个开发者独立验证，消除对 forge 的单点信任。

安全分析表明 gittuf 可抵御既往多类版本控制系统攻击；在 Git 与 Kubernetes 这类高活跃大型仓库上存储开销低于 4%，每次推送验证时间低于 0.59 秒。gittuf 已是 OpenSSF sandbox 项目，被 OpenSSF 与 CNCF 项目采用，Bloomberg 正在开展企业试点。

gittuf 把"诚实 forge"假设转化为可验证的集体责任，补齐了 SLSA 等供应链框架忽视的源码环节，工程落地程度高。局限：安全性依赖根密钥管理生态与开发者实际参与验证的意愿，新协议的采纳门槛较高；RSL 追加式日志在分叉、离线场景下的一致性与可用性仍有实践挑战。

- **论文 PDF**: [Rethinking Trust in Forge-Based Git Security](https://www.ndss-symposium.org/wp-content/uploads/2025-1008-paper.pdf)

#### LeakLess: Selective Data Protection against Memory Leakage Attacks for Serverless Platforms
- **作者**: Maryam Rostamipoor, Seyedhamed Ghavamnia, Michalis Polychronakis
- **方向**: 云与供应链安全
- **解读**: Serverless 平台（如 Cloudflare Workers、Fastly）为提升可扩展性采用语言级隔离（V8/WebAssembly），多租户函数同进程运行。但运行时漏洞可致越界读取，瞬态执行攻击（Spectre 类）更能绕过边界检查与内存安全语言，跨进程泄漏数据——已有研究演示恶意函数经 Spectre 窃取 Cloudflare Workers 中共驻函数机密。浏览器采用的进程级沙箱在 serverless 中会抵消语言级隔离的全部性能收益，不可行。

LeakLess 采用选择性内存加密：不试图堵住 CPU 泄露数据的各种路径，而是接受"数据可能泄漏"的前提，保证泄漏出去的始终是密文。开发者注解敏感数据后，内存表示始终加密；新增独立 I/O 模块（独立进程，可置于同主机或独立 VM/物理机）中介函数与外部通信并执行全部加解密，解决了此前方案无法安全传输受保护数据的局限。原型实现于基于 WebAssembly 的 Spin serverless 平台。

作者收集 1074 个公开 serverless 应用，449 个（近半）包含至少一类敏感数据，其中 407 个（91%）被完整支持、33 个（7%）部分支持。用 6 个真实应用验证，可有效抵御内存泄露与瞬态执行攻击；压力测试下吞吐下降最多 2.8%（I/O 模块异机）与 8.5%（同机）。

该工作思路务实，把防护目标从"堵住所有泄露路径"转为"数据即密文"的未来式防护，I/O 模块解决了实用化的最后一环，性能代价小。局限在于依赖开发者注解（易遗漏或误标），仅支持不可变数据及 JWT 签名/验证等特定可变操作（仍有 9% 应用不可覆盖），密钥管理本身成为新攻击面。与 DyPrIs（异常检测+进程迁移，有误报）和 Swivel（编译加固，开销高）相比，LeakLess 无假阳性、开销低，是语言级隔离 serverless 上兼顾安全与性能的较优解。
- **论文 PDF**: [LeakLess: Selective Data Protection against Memory Leakage Attacks for Serverless Platforms](https://www.ndss-symposium.org/wp-content/uploads/2025-35-paper.pdf)

#### JBomAudit: Assessing the Landscape, Compliance, and Security Implications of Java SBOMs
- **作者**: Yue Xiao, Dhilung Kirat, Douglas Lee Schales, Jiyong Jang, Luyi Xing, Xiaojing Liao
- **方向**: 云与供应链安全
- **解读**: 软件物料清单（SBOM）是供应链漏洞管理与许可证合规的基石，美国行政令 EO 14028 与 NTIA 最低要求都强调依赖清单的正确性与完整性，但这些要求在实践中从未被大规模检验。JBomAudit 是首个对 Java 生态中开发者发布的官方 SBOM 现状、合规性与安全影响进行系统性测量的工作。

作者从 Maven Central 收集 47,042 个官方 SBOM，基于对 496 条开发者讨论的定性分析建立一致性模型，形式化六类依赖不一致（缺失直接/传递依赖与传递关系 M1-M3、错误列出 N1-N3），并实现端到端工具 JBomAudit：用程序分析解包 JAR 依赖树，与 SBOM 逐项对比输出六类不一致。

对 25,882 个 SBOM 及其 JAR 的分析显示：7,907 个 SBOM 遗漏直接依赖（平均 6.18 个/个），19,404 个错误列出依赖（平均 4.45 个/个），13,394 个至少存在四类不一致；平均 4.97% 的被遗漏依赖含漏洞，导致 507 个漏洞被 SBOM 驱动的漏洞管理工具漏掉，另有 0.28% 错误列出的依赖引发无效处置。根因分析发现 99.9% 的 SBOM 由 CycloneDX Maven/Gradle 插件生成，且只依赖单一元数据文件（pom.xml/build.gradle）：77.25% 的缺失直接依赖在 POM 中就不存在，85.34% 的错误依赖也源于 POM 记录错误，另有 1,236 例为插件配置位置错误。

该研究是首次大规模量化官方 SBOM 质量危机，直接服务 EO 14028 政策的落地评估，方法论（一致性模型 + 依赖树解包对比）可推广到其他生态。局限：仅覆盖 Java/Maven，npm、PyPI 等生态与生成器多样性未涉及；对混淆打包、动态加载的依赖可能解包不全。与 Balliu 等仅人工抽查 26 个项目的先验工作相比，规模与自动化程度均有本质提升。
- **论文 PDF**: [JBomAudit: Assessing the Landscape, Compliance, and Security Implications of Java SBOMs](https://www.ndss-symposium.org/wp-content/uploads/2025-322-paper.pdf)

#### Enhancing Security in Third-Party Library Reuse – Comprehensive Detection of 1-day Vulnerability through Code Patch Analysis

- **作者**: Shangzhi Xu, Jialiang Dong, Weiting Cai, Juanru Li, Arash Shaghaghi, Nan Sun, Siqi Ma (The University of New South Wales)
- **方向**: 云与供应链安全
- **解读**: 复用开源第三方库（TPL）是高效开发的主流方式，但低维护的TPL会引入1-day漏洞：开发者既可能原样复用，也可能以定制方式复用甚至自行修补漏洞函数，现有工具（如V1SCAN、MVP）只能处理简单定制复用，且依赖代码级相似度比较，难以覆盖复杂的定制场景。本文提出VULTURE，通过代码补丁分析全面检测TPL复用引入的1-day漏洞。
VULTURE先执行TPLFilter：利用大语言模型对提交进行切片，自动为指定平台构建含漏洞版与补丁版的TPL数据库，并用LSH把函数压缩成数值元组。随后进行双重分析：基于版本的比较识别精确复用，基于token化与块分析（chunk-based）的细粒度重构识别定制复用；块由过程内分析生成，保留漏洞代码到补丁代码转换中的变量语义信息。
作者人工标注了68个真实项目、200个TPL复用和200个漏洞复用函数作基准：VULTURE识别出184个漏洞复用函数，F1达95.8%，而SOTA学术工具V1SCAN仅识别100个（F1 66.7%）；在10个真实项目上从178个被复用TPL中检出175个漏洞，并优于商业工具SNYK。
我认为用LLM做提交切片建库、用哈希比较替代代码相似度是务实创新，chunk分析对定制复用的语义保持设计精细。局限是基准规模有限、对混淆/重写程度极高的复用鲁棒性未知，LLM建库的稳定性也需考察。相比V1SCAN等"精确复用"工具，它显著拓宽了1-day检测的覆盖面。

- **论文 PDF**: [Enhancing Security in Third-Party Library Reuse – Comprehensive Detection of 1-day Vulnerability through Code Patch Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-576-paper.pdf)

#### Attributing Open-Source Contributions is Critical but Difficult: A Systematic Analysis of GitHub Practices and Their Impact on Software Supply Chain Security
- **作者**: Jan-Ulrich Holtgrave, Kay Friedrich, Fabian Fischer, Nicolas Huaman, Niklas Busch, Jan H. Klemmer, Marcel Fourné, Oliver Wiese, Dominik Wermke, Sascha Fahl（CISPA、Leibniz University Hannover、Paderborn University、North Carolina State University）
- **方向**: 云与供应链安全
- **解读**:
关键开源项目构成许多大型软件系统的信任根基，验证提交作者身份的真实性对供应链安全至关重要。但 Git 用户可自由配置作者姓名与邮箱，GitHub 又依据这类信息生成指向用户账号的资料链接，为攻击者伪造可信贡献者身份提供了可乘之机，此前缺乏系统性评估。

论文设计三种攻击场景展示攻击者如何操纵 GitHub 上的项目与资料页使其显得可信，并通过混合研究评估影响与对策：一是对 50,328 个关键开源项目的大规模测量，二是对在线安全建议的系统分析。

测量显示 85.9% 项目的贡献工作流可被滥用；攻击者可认领 573,043 个邮箱地址来劫持历史贡献、提升账号可信度。作为对策的提交签名采用率极低：95.4% 的用户从未签名提交，72.1% 的项目没有任何签名提交，仅 2.0% 的用户做到全部签名。安全建议文档大多只意识到简单的 Git 提交伪造，对 GitHub 邮箱处理机制的问题缺乏认识。

这是首个大规模量化 GitHub 贡献归属可伪造性的研究，说明"以签名作为唯一对策"在现实采用率下并不可行。局限在于测量聚焦关键项目子集；建议的平台侧修复尚未验证其对合法协作流程的影响。

- **论文 PDF**: [Attributing Open-Source Contributions is Critical but Difficult: A Systematic Analysis of GitHub Practices and Their Impact on Software Supply Chain Security](https://www.ndss-symposium.org/wp-content/uploads/2025-613-paper.pdf)

### 认证与访问控制（3 篇）

#### CASPR: Context-Aware Security Policy Recommendation

- **作者**: Lifang Xiao, Hanyu Wang, Aimin Yu, Lixin Zhao, Dan Meng

- **方向**: 认证与访问控制

- **解读**: SELinux 被广泛用于提供灵活的强制访问控制，安全策略对维持操作系统安全至关重要；严格而言所有访问请求都需被适当策略规则约束，但手工配置策略规则易错、耗时且需要专家知识，而策略规则数量庞大、语义复杂，自动推荐无异常的规则极具挑战。此前多数研究从已有策略挖掘信息以推荐规则，却无法应用于完全没有规则的新定义类型。

方法上，CASPR 提出上下文感知的策略推荐方法：利用策略规则、文件位置、审计日志与属性信息等多源上下文，提取多个特征计算权限集的相似度；基于计算结果用 K-means 模型对类型聚类，再自动推荐规则；并自动检测策略中的约束冲突、策略不一致、权限不完整三类异常，进而精炼修复异常策略，使授权规则得到有效执行。

实验证实了方法为不同版本策略推荐有效规则的可行性：推荐规则平均准确率 91.582%、F1 值 93.761%；三类异常均可被检测并自动修复；基于 SHAP 计算了各上下文特征的贡献；在多个操作系统上应用验证了普适性，且对现有类型的推荐精度优于其他规则推荐模型。

我的思考：CASPR 把"冷启动"问题（新定义类型无任何规则）作为核心场景，以多源上下文特征+聚类实现从无到有的规则生成，并闭环了"推荐-检测-修复"流程，工程完整性突出；SHAP 特征归因增加了可解释性。局限是 K-means 聚类对特征工程敏感、精度指标受数据集规模影响，异常修复的语义正确性需专家复核。与纯策略挖掘的推荐模型相比，上下文感知使 CASPR 能同时服务存量与新定义类型，是 SELinux 策略自动化管理的重要进展。

- **论文 PDF**: [CASPR: Context-Aware Security Policy Recommendation](https://www.ndss-symposium.org/wp-content/uploads/2025-418-paper.pdf)

#### An Empirical Study on Fingerprint API Misuse with Lifecycle Analysis in Real-world Android Apps
- **作者**: Xin Zhang, Xiaohan Zhang, Zhichen Liu, Bo Zhao, Zhemin Yang, Min Yang（复旦大学）
- **方向**: 认证与访问控制
- **解读**:
基于指纹的认证（FpAuth）被 Android 应用越来越多地用于账号登录、支付等高敏感场景，提供了便捷的身份验证方式；但指纹 API（FpAPI）复杂且持续演进，真实应用中的正确与安全使用仍是难题，此前缺乏系统性实证研究。

论文首次从 FpAuth 生命周期视角对 Android 应用中的 FpAPI 误用进行系统实证分析：先开发专门工具识别并分析使用 FpAPI 的应用及其特征，再定义威胁模型、通过实际环境下的详细生命周期分析归纳出四类常见误用，最后开发工具进行自动检测。

在 1,333 个使用 FpAuth 的应用中，97.15% 至少存在一类误用，18.83% 同时受四类误用影响；后果包括未授权数据访问、账户沦陷乃至经济损失。作者负责任披露后，共获得 184 个 CVE 编号、19 个 CNVD 编号及 15 家厂商确认。

这是首个大规模、生命周期视角的 FpAPI 误用研究，"97.15% 应用至少一类误用"的结果极具警示性。局限在于检测主要基于静态分析，对动态执行路径与厂商定制指纹方案的覆盖有限；部分"误用"在特定安全模型下是否真正可利用仍需个案验证。

- **论文 PDF**: [An Empirical Study on Fingerprint API Misuse with Lifecycle Analysis in Real-world Android Apps](https://www.ndss-symposium.org/wp-content/uploads/2025-699-paper.pdf)

#### ”Who is Trying to Access My Account?” Exploring User Perceptions and Reactions to Risk-based Authentication Notifications

- **作者**: Tongxin Wei (Nankai University), Ding Wang (Nankai University), Yutong Li (Nankai University), Yuehuan Wang (Nankai University)
- **方向**: 认证与访问控制
- **解读**: 基于风险的认证（RBA）日益普及，其通知用于及时警示用户账户遭受未授权访问。已有研究显示用户能识别由自身触发的合法登录通知，但很少关注由非账户持有者触发的 RBA 通知能否有效唤起用户的危机意识并阻止潜在攻击。

作者招募 258 名在线与 15 名线下参与者，围绕三类 RBA 通知（正确密码登录、错误密码尝试、密码重置触发）考察用户的感知、反应与期望。

结果显示超过 90% 的参与者认为 RBA 通知重要；用户对三类通知的感受与行为无显著差异，但对各类通知的期望截然不同；多数参与者收到通知后感到怀疑、紧张与焦虑。46% 的用户怀疑 RBA 通知本身是钓鱼尝试，而将其归为钓鱼或垃圾信息可能导致账户保护失效；尽管有此疑虑，65% 的用户仍会登录账户检查可疑活动，若未发现异常则不再采取行动。当前通知格式难以赢得用户信任、未达其期望，用户需要更详细的可疑访问信息、额外的安全措施与清晰的风险说明。作者据此提出五条 RBA 通知设计建议。

该研究系统考察了非账户持有者触发场景下的 RBA 通知效果，"46% 视通知为钓鱼"与"65% 仅登录自查"揭示了通知设计的核心张力：既要警示用户，又须避免被误判为钓鱼。局限在于以自我报告为主、缺乏真实攻击情境下的行为度量，且未检验五条建议的实际效果。与"用户能识别自身触发通知"的既往研究互补，该工作提示 RBA 通知应从"可识别"升级为"可信任、可行动"，对通知文案与后续引导设计有直接指导意义。
- **论文 PDF**: [”Who is Trying to Access My Account?” Exploring User Perceptions and Reactions to Risk-based Authentication Notifications](https://www.ndss-symposium.org/wp-content/uploads/2025-133-paper.pdf)

### 数字取证（1 篇）

#### SCRUTINIZER: Towards Secure Forensics on Compromised TrustZone
- **作者**: Yiming Zhang, Fengwei Zhang, Xiapu Luo, Rui Hou, Xuhua Ding, Zhenkai Liang, Shoumeng Yan, Tao Wei, Zhengyu He（南方科技大学、香港理工大学、中科院信工所、新加坡管理大学、新加坡国立大学、蚂蚁集团）
- **方向**: 数字取证
- **解读**:
TrustZone系统漏洞频发（近5年超200个CVE），却缺乏取证工具：REE侧取证被TrustZone隔离阻挡，Secure world内取证可被特权攻击者控制，TZASC权限也可被撤销。

利用Arm CCA RME的Root world（EL3 Monitor）部署SCRUTINIZER；将内存采集解耦到Secure world内由双GPT保护的隔离agent（Root world代码缩小约43%）；“嫁接”目标L0页表免去逐页地址转换；GPT no-access+PMU溢出+PMI实现指令级访问陷阱；ETE+ETB指令追踪；Debugv8p4/GPT保护硬件配置防篡改。

TCB约1.6K行；内存采集平均开销1.75%（比NINJA快20倍）；访问陷阱开销较NINJA降49.5%；指令追踪开销<0.2%；调研56个CVE验证其安全性。

首个面向被攻陷TrustZone的安全取证框架，把取证建立在硬件最高特权域，贴合真实攻击链。局限：依赖CCA硬件（当时未商用，性能经FVP与Juno测量）；DoS、侧信道、物理攻击不在范围内；GPT页粒度（4KB）需PMU配合才达指令级。

- **论文 PDF**: [SCRUTINIZER: Towards Secure Forensics on Compromised TrustZone](https://www.ndss-symposium.org/wp-content/uploads/2025-147-paper.pdf)

### 无线与通信安全（9 篇）

#### Time-varying Bottleneck Links in LEO Satellite Networks: Identification, Exploits, and Countermeasures
- **作者**: Yangtao Deng (Tsinghua University), Qian Wu (Tsinghua University), Zeqi Lai (Tsinghua University), Chenwei Gu (Tsinghua University), Hewu Li (Tsinghua University), Yuanjie Li (Tsinghua University), Jun Liu (Tsinghua University)
- **方向**: 无线与通信安全
- **解读**: 问题：LEO 卫星网络（Starlink、Kuiper 等）快速部署，但星地链路（GSL）容量远小于星间链路（ISL），地面站与人口分布不均使瓶颈链路必然存在；又因卫星高速运动，瓶颈链路时变（切换间隔仅约 15 秒）。轨道等信息公开，攻击者可识别瓶颈链路并发起链路洪泛攻击（LFA）。

方法：三步研究：①刻画瓶颈链路时空特性并给出识别方法（按地面站服务时间排序取重叠、识别背景流量超 GSL 容量一半的链路集）；②提出 SKYFALL 风险分析器，用公开 TLE/地面站数据与类 iperf 突发流量估计 GSL 背景流量，评估合法流量降级、拥堵 GSL 比例、受影响地理块与隐蔽性，并分析受控终端数量、区域等因素；③评估传统对策局限并提出流量调度、节流、差异化计费等缓解。

结果：基于真实 Starlink/Kuiper 数据的仿真显示，拥堵仅约 8% 的 GSL 即可拥塞 41% 的合法流量、影响约 19% 的地理块（随机拥堵仅 11%-14%）；与 ICARUS 相比削减流量约为其 3.4 倍（37% vs 11%），最坏情况下合法流量吞吐可降低 3.4 倍；t1/t2 时刻分别识别 44/42 条瓶颈链路，拥堵链路数全程稳定（Kuiper 10-22、Starlink 40-58 条）。

我的思考：贡献在于首次系统性量化"时变瓶颈链路"这一 LSN 特有攻击面，SKYFALL 将 LFA 分析从静态快照推进到动态全景，且基于公开信息。局限：以仿真为主，依赖流量分布假设；"3.4 倍"是与 ICARUS 的相对对比，绝对影响取决于容量假设；缓解为方向性方案。相比 Coremelt/Crossfire，本文把 LFA 研究迁移到卫星动态拓扑，对运营商有直接价值。
- **论文 PDF**: [Time-varying Bottleneck Links in LEO Satellite Networks: Identification, Exploits, and Countermeasures](https://www.ndss-symposium.org/wp-content/uploads/2025-109-paper.pdf)

#### Starshields for iOS: Navigating the Security Cosmos in Satellite Communication
- **作者**: Jiska Classen (Hasso Plattner Institute, University of Potsdam), Alexander Heinrich (TU Darmstadt, Germany), Fabian Portner (TU Darmstadt, Germany), Felix Rohrbach (TU Darmstadt, Germany), Matthias Hollick (TU Darmstadt, Germany)
- **方向**: 无线与通信安全
- **解读**: 
iPhone 14 起苹果借 Globalstar LEO 卫星网络提供应急 SOS、道路救援、Find My 位置共享及 iOS 18 的 iMessage/SMS；卫星带宽与延迟受限使现代安全协议难以直接使用，且 Globalstar 2015 年曾被曝协议漏洞。本文首次逆向并评估苹果专有卫星协议的安全与隐私属性。

通过 baseband debug profile 捕获 QMI 消息并扩展 libqmi/Wireshark 解析器逆向协议流；用 Frida 在 iPhone 13 上搭建卫星仿真测试床（避免触发真实紧急呼叫）；用 BladeRF SDR 记录物理层信号验证空中传输。

发现密钥材料需在联网阶段通过离线 ECDH 预备，应急文本用 AES-256-CTR（确定性 IV、无认证），虽受外层传输加密保护；可篡改 Find My 位置共享发送任意 83–160 字节消息并绕过 15 分钟限流；修改本地 Trial 配置可在波兰等不支持地区启用卫星服务、在欧盟启用 iMessage 与道路救援。

首个完整的 iPhone 卫星协议逆向，揭示"为省带宽牺牲完整性"与"限制仅实现于客户端"两类系统性问题；苹果已强制 83 字节消息修复，但架构性绕过难以根治。仿真测试床与协议文档作为开源工件，为后续研究提供良好基础。

- **论文 PDF**: [Starshields for iOS: Navigating the Security Cosmos in Satellite Communication](https://www.ndss-symposium.org/wp-content/uploads/2025-124-paper.pdf)

#### Spatial-Domain Wireless Jamming with Reconfigurable Intelligent Surfaces
- **作者**: Philipp Mackensen (Ruhr University Bochum), Paul Staat (Max Planck Institute for Security and Privacy), Stefan Roth (Ruhr University Bochum), Aydin Sezgin (Ruhr University Bochum), Christof Paar (Max Planck Institute for Security and Privacy), Veelasha Moonsamy (Ruhr University Bochum)
- **方向**: 无线与通信安全
- **解读**: 
无线干扰（jamming）是低成本 DoS 威胁，但射频的广播特性使干扰信号必然波及邻近设备，无法精确瞄准；现有选择性干扰（reactive jamming）需实时解析协议信息且受距离限制。本文提出能否只干扰目标设备而让邻居设备保持工作的物理层方案。

首次用可重构智能表面（RIS，768 个 1-bit 相位控制单元）实现空间域选择性干扰：攻击者先被动窃听各设备信号，利用 RSSI 与信道互易性评估各 RIS 配置，用贪心遗传算法优化（最大化目标设备、最小化非目标设备的信道增益）；随后以该配置主动发射干扰信号。

办公环境 10 台 Wi-Fi 设备实验中，单目标、多目标簇、"除一台外全部"干扰均成功，单目标有 17dB 功率裕度；设备天线相距仅 5mm（亚波长）时仍能对目标完全 DoS 而邻机保持 25 Mbit/s；RIS 配置 24 小时稳定；较 19dBi 定向天线所需功率低 3dB、干扰裕度 20dB 对 4dB；真实 Wi-Fi 速率自适应下非目标吞吐仍多高于 22 Mbit/s。

首次把 RIS 从通信优化技术转化为精确攻击工具，开辟空间域攻击新自由度，约 750 欧元成本使其对个人可行；局限是需相对静态环境、优化约 5 分钟、依赖 MAC 地址区分设备。对策（MAC 随机化、功率/波束随机化）均只能提高攻击门槛。

- **论文 PDF**: [Spatial-Domain Wireless Jamming with Reconfigurable Intelligent Surfaces](https://www.ndss-symposium.org/wp-content/uploads/2025-440-paper.pdf)

#### Rediscovering Method Confusion in Proposed Security Fixes for Bluetooth
- **作者**: Maximilian von Tschirschnitz, Ludwig Peuckert, Moritz Buhl, Jens Grossklags
- **方向**: 无线与通信安全
- **解读**:
蓝牙是临时配对的事实标准（2023 年出货约 54 亿台），却易受方法混淆（Method Confusion）攻击：攻击者诱导配对双方执行冲突的密钥建立方法，导致接受受损密钥。随着支付终端、资产追踪、会议系统等安全敏感应用依赖蓝牙，验证与修复该问题迫在眉睫，但既有修复提案威胁模型并不完整。

作者提出 Ad Hoc Ecosystem 扩展威胁模型，从根源建模方法混淆，将多次配对尝试与单侧配对（one-sided pairing）纳入威胁，引入 Frustration Threshold 等现实假设。用该模型审视此前唯一考虑 MC 的修复方案（Shi et al. 的 PNC/PPE），发现两类新攻击：PNC 与 PE 混淆可使攻击者获得完整 MitM 地位，PNC 与 PPE 混淆造成单侧配对。

基于 BTstack 在树莓派 3B 上实现并端到端验证两类攻击，证实其实践可行性；随后提出由 XPE 与 XNC 两种新配对方法构成的安全框架，减少用户交互，并给出安全因子 µ=1/2^(l-1)（l 为 OOB 值比特长度）的形式化安全证明。

该工作表明"修补已知攻击"不等于"解决根因"，对协议标准化具有警示价值；新框架是目前唯一在扩展威胁模型下可证明安全且兼容现有硬件与用户行为的蓝牙演进提案。局限：新协议需纳入蓝牙标准才具实际影响，采纳周期长；形式化证明依赖 OOB 通道认证等理想化假设，实现细节仍需谨慎。

- **论文 PDF**: [Rediscovering Method Confusion in Proposed Security Fixes for Bluetooth](https://www.ndss-symposium.org/wp-content/uploads/2025-310-paper.pdf)

#### Magmaw: Modality-Agnostic Adversarial Attacks on Machine Learning-Based Wireless Communication Systems
- **作者**: Jung-Woo Chang, Ke Sun, Nasimeh Heydaribeni, Seira Hidano, Xinyu Zhang, Farinaz Koushanfar
- **方向**: 无线与通信安全
- **解读**: 基于机器学习的无线通信系统（如端到端联合信源信道编码 JSCC）被广泛视为 6G 的关键技术，但现有对抗攻击假设过于理想：只针对单一模态、假设攻击者已知发送模态、或需白盒模型与协议先验。现实中发射端混合多模态数据，攻击者无法获知信道矩阵与星座映射/OFDM 参数，且难以与收发端精确同步。Magmaw 的目标是在这些约束下生成模态与协议无关的通用对抗扰动（UAP），破坏 ML 无线通信及其下游应用。

Magmaw 设计扰动生成器模型（PGM），在多个代理多模态 JSCC 模型上做集成学习，训练出输入与信道无关的 UAP；训练中引入时移与相位旋转函数处理时间/频率失同步；引入判别器与多样性损失，使扰动具有可变性以对抗自适应防御。攻击者仅需软件无线电（SDR）设备即可实时注入扰动，无需知晓数据模态与协议细节。

实验表明：图像与视频传输的 PSNR 分别最多下降 8.04dB 与 8.29dB；语音 MSE 较基线最多增加 3.91 倍；文本 BLEU 最低降至 0.338；下游任务攻击成功率最高达 91.2%。加密信道案例中 PSNR 最多下降 5.88dB；对信道模态 ML 模型引入比基线多 2.2 倍的误差，且在鲁棒训练、扰动检测等防御下依然有效。

该工作是首个同时考虑多模态数据、物理层协议与无线域约束的通用扰动框架，SDR 实测与多类防御韧性评估完整，代码开源。局限在于代理模型与真实目标模型存在迁移差距，攻击效果依赖代理集质量；对掌握 PGM 结构的专家级防御评估深度有限。与 Vanilla UAP、Sync-Free UAP 等基线相比优势显著，为 6G 原生 AI 通信的安全设计提供了重要警示。
- **论文 PDF**: [Magmaw: Modality-Agnostic Adversarial Attacks on Machine Learning-Based Wireless Communication Systems](https://www.ndss-symposium.org/wp-content/uploads/2025-s336-paper.pdf)

#### Lend Me Your Beam: Privacy Implications of Plaintext Beamforming Feedback in WiFi
- **作者**: Rui Xiao, Xiankai Chen, Yinghui He, Jun Han, Jinsong Han
- **方向**: 无线与通信安全
- **解读**: WiFi 信号泄漏使攻击者能通过 CSI 扰动感知室内人员活动，构成占用检测等隐私威胁——86% 的入室窃贼会避免与住户相遇，知道"家中无人"将助长犯罪。但穿墙后的模拟 CSI 衰减严重，限制了攻击距离。论文发现新侧信道：波束成形反馈信息（BFI），数字编码、明文传输、穿墙后仍保留人员运动信息，超过 73% 的商用 802.11ac 及以上设备会发送 BFI，Wireshark 即可捕获，而能嗅探 CSI 的设备仅约 6%，攻击门槛大幅降低。

LeakyBeam 是改进的占用检测攻击。针对环境与设备歧义（多径效应、静止目标信号微弱），它联合利用 BFI 的幅度与相位，统一检测移动与静止住户；针对 BFI 经 SVD 处理与压缩后相位偏移难以去除的问题，利用 AP 天线分集把受相位偏移影响的 BFI 与 CSI 共轭相乘关联去除偏移，再跨 OFDM 子载波融合动态分量。配套防御在 AP 既有空间映射机制上叠加时变随机变换混淆 BFI 幅度与相位，客户端不受影响、硬件改动极小。

在 8 个商用 AP、9 种部署环境下采集约 160 万 BFI 包、历时 49 小时。嗅探器位于距目标住宅 20 米处仍能有效检测，平均真正例率 82.7%、真负例率 96.7%，且对外部干扰鲁棒。

贡献在于发现并利用 BFI 这一更易获取、更耐穿墙衰减的数字明文侧信道，显著降低攻击者门槛，威胁现实性高；防御方案复用既有硬件机制，实用性强。局限在于结果对 AP 与环境仍有依赖，静止目标检测精度有限（TPR 82.7%），更远距离效果未验证。与既有 CSI 侧信道工作相比，其"数字明文包+天线/子载波分集"路线新颖，并对 802.11ad/ay 毫米波定向波束的类似风险提出前瞻警示。
- **论文 PDF**: [Lend Me Your Beam: Privacy Implications of Plaintext Beamforming Feedback in WiFi](https://www.ndss-symposium.org/wp-content/uploads/2025-5-paper.pdf)

#### Detecting IMSI-Catchers by Characterizing Identity Exposing Messages in Cellular Traffic

- **作者**: Tyler Tucker, Nathaniel Bennett, Kevin Butler, Patrick Traynor (University of Florida), Martin Kotuliak, Simon Erni, Srdjan Capkun (ETH Zurich)
- **方向**: 无线与通信安全
- **解读**: IMSI-Catcher（伪基站）能让非运营商方秘密追踪用户，即使在2G-5G中依然有效且已商品化。现有检测方案多依赖相关行为特征（弱加密使用、异常广播、临时基站存在等），但良性场景（运营商在大型活动部署临时基站）也会触发这些特征，导致大量误报——此前没有任何已发表检测器能在野外以统计显著性证明IMSI-Catcher存在。本文转向因果属性：检测IMSI-Catcher"必须"使用的、迫使设备暴露永久标识符的消息。
作者系统分析3GPP标准，整理出2G/3G/4G/5G-NSA下可诱使设备泄露IMSI的53条预认证消息（远超先前仅关注的Identity Request）；随后在两大洲完成400多小时的网络测量，建立正常运营下连接使用这些消息比例的基线画像（如LTE连接中此类连接中位数不足3%）。
用基线对比实验室中合规部署的开源IMSI-Catcher，并在一个媒体高度关注的公开活动（重要人物出庭）上观测到异常行为：IMSI暴露连接比例与基线差异的p值远小于0.005，统计显著地暗示该活动存在IMSI-Catcher——这是首篇以统计显著性支撑野外发现的论文；方法还可检测大规模overshadow攻击。
我认为"从相关特征转向因果必需消息"是方法论上的正解：IMSI-Catcher为达成目的必须发送这些消息，无法通过改变行为模式规避，天然抗自适应敌手。局限是53条消息的完备性依赖标准分析、基线需持续更新，且检测需要下行链路接入能力。相比弱加密检测等方案，它大幅降低了误报并首次给出统计显著性证据。
- **论文 PDF**: [Detecting IMSI-Catchers by Characterizing Identity Exposing Messages in Cellular Traffic](https://www.ndss-symposium.org/wp-content/uploads/2025-1115-paper.pdf)

#### Deanonymizing Device Identities via Side-channel Attacks in Exclusive-use IoTs & Mitigation

- **作者**: Christopher Ellis, Yue Zhang, Mohit Kumar Jangid, Shixuan Zhao, Zhiqiang Lin

- **方向**: 无线与通信安全

- **解读**: BLE 与 Wi-Fi 等无线技术使 IoT 设备无需物理连接即可通信，但暴露在空中的数据交换可被攻击者观测，带来设备追踪等严重安全与隐私威胁；协议设计者通常依赖地址与身份随机化作为对策。本文揭示这些攻击依然是重大威胁，根源在于"专属使用"（exclusive-use）无线通信中一个被长期忽视的根本缺陷。

方法上，作者将 exclusive-use 定义为设备仅向已配对设备提供功能性的场景，发现此类关系中独特的通信模式构成可观测的布尔侧信道：攻击者可据此判断两台设备是否"互信"，从而实现设备去匿名化，即使面对现代随机化对策仍可追踪。作者将攻击命名为 IDBleed，证明支持机密性、完整性、认证的 BLE 与 Wi-Fi 协议仍无法免疫，并提出通用的隐私保护缓解方案 Anonymization Layer。

在测试的智能手机与 PC 上，Anonymization Layer 的性能与功耗开销仅为约 2%，可以忽略；量化评估验证了缓解方案的有效性。

我的思考：把"通信模式本身"识别为侧信道信息源是重要洞察——此前对策聚焦地址与身份随机化，却未考虑"是否通信、如何通信"这一层语义；布尔侧信道概念简洁而通用，可迁移到其他专属配对协议。局限是攻击需观测长期通信模式，实时性与多设备场景下的可靠性、以及缓解层对不同协议族的通用性需进一步验证。与设备追踪攻击相比，本文揭示了随机化对策之外的深层次信息泄漏面。

- **论文 PDF**: [Deanonymizing Device Identities via Side-channel Attacks in Exclusive-use IoTs & Mitigation](https://www.ndss-symposium.org/wp-content/uploads/2025-703-paper.pdf)

#### CHAOS: Exploiting Station Time Synchronization in 802.11 Networks

- **作者**: Sirus Shahini, Robert Ricci

- **方向**: 无线与通信安全

- **解读**: 许多地点（尤其是城区）的 Wi-Fi 流量噪声极大，除数据流量外，站点发送的管理与控制帧在一小块区域内每秒可轻松超过数百帧。这类 Wi-Fi 环境提供了把数据隐藏在良性传输正常噪声成分中的机会。本文利用 Wi-Fi 定时同步功能（TSF）这一特性，构建了一个承载秘密信号的隐蔽信道。

方法上，作者利用 Wi-Fi 站点时间同步中必然存在的误差特性，提出 CHAOS 隐蔽信道，用未修改的标准 Wi-Fi 硬件在 beacon 帧中嵌入数据位：一方面在 beacon 帧的排序中编码信息（beacon 没有自然或必需的顺序），另一方面利用管理帧头 TSF 时间戳构成定时信道，模仿真实基站的时序误差，使编码后数据在统计上与未修改帧相似；参数可调，以配置数据速率、信道稳定性与帧丢失率。

在推荐设置下，CHAOS 能以 520 bits/s 稳健广播秘密数据；作者还指出 TSF 有进一步利用潜力，勾勒了一种利用 TSF 将客户端映射到基站的关联攻击。

我的思考：无需硬件修改、仅利用标准 Wi-Fi 帧时序特性的隐蔽信道，规避了传统隐蔽信道检测（关注帧内容或调制特征）的视线；"beacon 排序 + 时序误差模仿"双通道设计使统计检测难度大增。520 bits/s 的速率对隐蔽通信已具实用价值。局限是需在目标环境附近部署接收机、信噪比依赖周围基站密度，且长期发送可能留下时序指纹。与基于载荷改写的隐蔽信道相比，本文展示了"利用协议内在噪声"的更隐蔽思路，对隐蔽通信监管构成挑战。

- **论文 PDF**: [CHAOS: Exploiting Station Time Synchronization in 802.11 Networks](https://www.ndss-symposium.org/wp-content/uploads/2025-187-paper.pdf)

### 数据安全（1 篇）

#### All your (data)base are belong to us: Characterizing Database Ransom(ware) Attacks
- **作者**: Kevin van Liebergen, Gibran Gomez, Srdjan Matic, Juan Caballero（IMDEA Software Institute）
- **方向**: 数据安全
- **解读**:
数据库勒索攻击是一类真实存在的攻击：攻击者扫描数据库服务器，利用缺乏认证或弱口令登录，删除数据库内容并勒索赎金以换取数据恢复。此类攻击此前缺乏系统研究，其规模、组织归属与产业链形态均不清晰。

论文开展首个系统化研究：三年间从 60,427 台被攻陷的数据库服务器收集 23,736 份勒索信，并部署数据库蜜罐获得第一手观察。归因方面实现聚类方法：先以勒索信文本相似度识别攻击活动（campaign），再利用指标复用与比特币区块链信息判断哪些活动出自同一组织。

测量显示数据库勒索攻击非常普遍：2024 年 3 月新增受感染服务器约 6K 台，同比增长 60%；蜜罐联网仅 14 小时即被感染。Elasticsearch 服务器因最新版本采纳缓慢，弱认证问题比 MySQL 高两个数量级。60,427 台服务器被归因于 32 个组织发起的 91 个攻击活动；一个主导组织占 76% 的受感染服务器与 90% 的财务影响，且与国家行为体及此前针对 Git 仓库的攻击有关联。

这是首个大规模刻画数据库勒索攻击与组织归因的测量研究，"单一组织主导 90% 财务影响"的发现对防御资源分配极具参考价值。局限在于勒索信聚类与区块链关联属启发式推断，存在误归因可能。

- **论文 PDF**: [All your (data)base are belong to us: Characterizing Database Ransom(ware) Attacks](https://www.ndss-symposium.org/wp-content/uploads/2025-1887-paper.pdf)

### 社会工程与人类因素（9 篇）

#### Was This You? Investigating the Design Considerations for Suspicious Login Notifications

- **作者**: Sena Sahin, Burak Sahin, Frank Li

- **方向**: 社会工程与人类因素

- **解读**: 大量在线平台通过监控登录活动检测异常，并向用户发送可疑登录通知，帮助用户判断登录是否合法并采取补救措施。但这类通知的设计此前鲜有系统研究——现有安全警告研究多聚焦浏览器警告、钓鱼告警等，而登录通知在上下文、用户参与方式与必要响应上都独具特点，不能直接套用其他警告的经验。本文针对真实世界中平台实际使用的基于邮件的可疑登录通知，研究设计要素如何影响用户"阅读通知、验证真实性、判断登录合法性、采取补救动作"四个环节。

方法上分两步：先建立经验基础——对 2023 年 3 月 Tranco 前 100 网站创建测试账号并触发可疑登录，从 34 个域名收集到真实通知，由两名研究者独立分析归纳各组成部分与设计变体（发现设计高度多样、缺乏标准化）；再以真实通知中的设计要素为材料，对 20 名美国参与者开展半结构化访谈（2023 年 6–8 月），第一部分考察既往经历，第二部分采用"共同设计"方式让参与者基于真实要素搭建自己偏好的通知，编码一致性（Kupper-Hafner）达 0.93，并达到主题饱和。

结果揭示：几乎所有参与者（19/20）打开邮件前先核验发件人信息，多数（12/17）评估主题行，偏好直陈内容（如"可疑账户活动"）的措辞而对"Urgent/Attention"式紧迫用语持怀疑态度；15/20 以行文质量（拼写、语法）判断真伪，10/20 对邮件内链接保持警惕、倾向自行访问官网改密（12 名遭遇过恶意登录的参与者中 10 名经官网而非邮件链接改密）；判断登录合法性时 16/20 以位置为首要依据、11/20 参考设备信息、7/20 看时间戳；主题行中 11/20 偏好加入用户名/姓名等个性化信息以增强可信度，全部 20 名参与者都要求在通知中包含设备信息（10 名倾向仅品牌+型号）。所有参与者均认为通知有用，尤其重视及时性、信息详实与措辞专业，并提出发送者可识别、合法声明、多样化遥测与建议动作等有效要素。

我的思考：本文是少有的聚焦"可疑登录通知"人因设计的实证研究，方法论上以真实平台通知为素材，避免凭空设计，且采用共同设计（co-design）让用户主动构造而非被动评价，能揭示偏好背后的推理。与 Markert 等单设计研究相比，本文覆盖了真实世界的设计多样性，并给出每个组件的具体建议（如建议动作应指明改密、启用 2FA 等），落地性强。局限：20 名美国参与者样本有限、以回忆与假设场景为主，存在生态效度风险（作者亦承认需产业界大规模验证）；"设计选择以真实平台可提供的数据为限"虽务实，但可能忽略更优的未落地设计。发现"用户对链接高度警惕"与"平台倾向在邮件中放链接"之间的张力，是通知设计最值得注意的冲突点——如何在降低钓鱼风险与提升行动便利间取舍，值得安全社区与产业界进一步探索。

- **论文 PDF**: [Was This You? Investigating the Design Considerations for Suspicious Login Notifications](https://www.ndss-symposium.org/wp-content/uploads/2025-694-paper.pdf)

#### The Power of Words: A Comprehensive Analysis of Rationales and Their Effects on Users' Permission Decisions
- **作者**: Yusra Elbitar, Alexander Hart, Sven Bugiel
- **方向**: 社会工程与人类因素
- **解读**: Android 运行时权限请求的说明文字（rationale）由开发者自由措辞，可能显著影响用户是否授权及决策质量；尽管存在各类指南，真实世界 rationale 的措辞与设计特征、以及不同措辞构件对用户决策的影响此前都缺乏系统研究。

作者先爬取 Google Play 各类别 Top 应用（9,489 个请求危险权限的 APK），用 SpaCy 分类器（精度 0.99、召回 0.84）加四台模拟器动态分析收集 35,737 条 rationale 句与 1,054 张截图，人工编码 720 句与 428 张截图提炼出视角、功能明确性、正负表述、附加信息等构件及对话框/全屏/嵌入式/横幅等设计要素。随后开展 960 人的预注册在线实验，用 5 个维度组合出 320 种 vignette，以多层线性模型分析。

研究发现：多数 rationale 为单句、对话式、用户视角（67%）且肯定式（79%）；"保证权限不会被滥用"显著提高授权率（OR=1.43）、知情感与控制感，"可随时更改"亦提高授权（OR=1.28）；明确说明功能提升知情感与满意度；反直觉的是，礼貌的 App 视角（"we would like"）反而降低授权（OR=0.71）；隐私担忧越高越不愿授权。

这是首个把 rationale 拆解为构件并量化其对授权行为影响的系统性实证，产出的可操作建议（具体功能+否定式表述+保证+可逆性提示）可直接服务开发者与审核者。局限在于情境实验的生态效度有限、样本以 Prolific 为主。尤其值得注意的是"无实质证明的保证也能提升授权"，揭示用户把验证责任委托给平台的心理，对应用审查与隐私监管有直接启示。
- **论文 PDF**: [The Power of Words: A Comprehensive Analysis of Rationales and Their Effects on Users' Permission Decisions](https://www.ndss-symposium.org/wp-content/uploads/2025-544-paper.pdf)

#### The Kids Are All Right: Investigating the Susceptibility of Teens and Adults to YouTube Giveaway Scams
- **作者**: Elijah Bouma-Sims, Lily Klucinec, Mandy Lanyon, Julie Downs, Lorrie Faith Cranor
- **方向**: 社会工程与人类因素
- **解读**: 抽奖诈骗（giveaway scam）以免费 Roblox Robux、Spotify Premium 等为诱饵，诱骗用户完成问卷调查、下载 App 等任务却一无所获，FBI IC3 2023 年收到超 88 万起网络犯罪报告（较 2019 年增长 90%）。游戏货币诱饵让青少年可能尤为脆弱，但此前没有任何人类受试研究考察用户如何互动以及哪些因素影响受害。

作者先在 YouTube 上做有机搜索（6 个按年龄/性别区分的账户）并分析约 1% 的 YouTube 样本（约 1000 万视频），估计 2020–2021 年间约有 1199 个 Robux 抽奖诈骗视频；随后开展情境实验：85 名美国青少年与 205 名成人（Prolific，10 个年龄桶）依次观看搜索结果、YouTube 视频、网站视频，为虚构朋友给出建议，每个话题各含 3 个诈骗与 3 个合法刺激。

84.4% 的参与者正确识别诈骗刺激（仅 9.2% 信以为真、6.4% 不确定），识别率因视频而异（97.8% vs 68.3%）；仅 5.5% 会建议注册或跟随指令；更逼真的诈骗让用户倾向"搜索了解更多"。青少年并不比成人更容易上当，但更倾向举报诈骗视频（21.2% vs 4.4%），且显著更常搜索过"free Roblox robux"（38.6% vs 6.3%）；女性表现出更多不确定性；36% 的人把合法刺激误判为诈骗。

这是少数直接比较未成年人与成人网络诈骗交互的研究，反直觉地挑战了"青少年更脆弱"的直觉，为家长教育与平台干预提供了实证依据。局限：样本非代表性、统计功效略欠、URL 隐去降低了生态效度。方法上"为朋友提建议"的设计值得借鉴；发现用户依赖评论、搜索来验证真伪，提示平台可在评论可信度提示与搜索结果标注上施加干预。
- **论文 PDF**: [The Kids Are All Right: Investigating the Susceptibility of Teens and Adults to YouTube Giveaway Scams](https://www.ndss-symposium.org/wp-content/uploads/2025-342-paper.pdf)

#### The (Un)usual Suspects – Studying Reasons for Lacking Updates in WordPress
- **作者**: Maria Hellenthal (CISPA Helmholtz Center for Information Security), Lena Gotsche (CISPA Helmholtz Center for Information Security), Rafael Mrowczynski (CISPA Helmholtz Center for Information Security), Sarah Kugel (Saarland University), Michael Schilling (CISPA Helmholtz Center for Information Security), Ben Stock (CISPA Helmholtz Center for Information Security)
- **方向**: 社会工程与人类因素
- **解读**: 
WordPress 等 CMS 漏洞频发且可被大规模自动化利用，但超过一半的 CMS 安装缺少最新安全补丁；既有漏洞通知（VN）研究对修复率的提升效果有限。现有文献聚焦个人设备用户与系统管理员，并未直接回答"网站所有者为何不打补丁"这一根本问题。

采用扎根理论（Grounded Theory）方法，对 19 名运行过时 WordPress 的网站所有者（含第二轮定向招募的网店主）与 9 名网站专业人士（1 名托管商、8 名开发者）进行半结构化访谈（2020–2023 年），对双数据集做三角验证与开放/主轴编码，提炼出六大关键类别并归纳为四种"不更新场景"。

除确认风险意识缺失、感知更新风险、更新成本等已知因素外，新发现网站的主观价值与运营委托（责任扩散、禁用效应、求助抑制）对更新行为影响更关键；所有受访者仅以"自我为中心"看待被攻陷风险，无人主动提及站点被滥用会危害他人。

定性方法补足了 VN 文献的盲区，说明不存在单一沟通策略能解决所有不更新场景；局限是样本较小、德语语境、依赖自我报告。对通知内容（强调对他人风险、明确所有者责任）给出务实建议，可指导后续更大规模的定量验证实验。

- **论文 PDF**: [The (Un)usual Suspects – Studying Reasons for Lacking Updates in WordPress](https://www.ndss-symposium.org/wp-content/uploads/2025-s277-paper.pdf)

#### Exploring User Perceptions of Security Auditing in the Web3 Ecosystem

- **作者**: Molly Zhuangtong Huang, Rui Jiang, Tanusree Sharma, Kanye Ye Wang
- **方向**: 社会工程与人类因素
- **解读**: Web3 生态 2023 年 TVL 超 450 亿美元、累计损失 770 亿美元，透明审计已成超半数应用的标配（覆盖 80% 以上 TVL），但用户如何感知审计仍是空白。本文以风险感知模型为框架，回答三个研究问题：审计信息感知（RQ1）、审计在增强安全中的角色（RQ2）、审计对用户与应用交互的影响（RQ3）。

方法上采用三部分研究：①案例研究——观察 15 个 TVL 超 10 亿美元应用的披露页面，综述 20 家审计公司官网；②20 场半结构化访谈（45-60 分钟，平均 3.75 年经验），阶梯式提问、双人编码+饱和分析；③Reddit 实证——自 10 个子版块提取清洗出 905 个帖子、2490 条评论（3264 位用户），用 GPT-4 做相关筛选与情感分析（准确率 91%，优于 VADER 的 62%）。

主要发现：用户几乎只依赖应用官网单一渠道获取审计信息，抱怨报告"仓促、公式化、重复"；38% 的公司网站缺乏审计流程描述、80% 未充分披露审计师资质、62% 省略审计师信息；技术门槛阻碍理解。声誉主导质量评判（16/20 说不出两家公司），独立性普遍受疑（相关 Reddit 帖 76% 持批评），教育角色获认可（66.67% 公司提供教育资料）。审计对决策影响有限（投入多为几分钟内），且效果不对称：成功审计帖平均情感分 4.01，失败审计仅 1.61；7 位受访者视审计报告为安全教育资源。

我的思考：首篇系统研究 Web3 审计用户感知的工作，揭示公开审计在可用性与可信度间的张力（95% 公司不披露价格），三角验证用 3000+ 用户社区数据弥补样本局限。局限：样本偏亚洲男性有经验者，依赖 GPT-4 分类，结论为感知层面而非因果。启示：审计公司应标准化报告、披露资质与定价，把"审计证明"变为用户真正可用的决策信息。
- **论文 PDF**: [Exploring User Perceptions of Security Auditing in the Web3 Ecosystem](https://www.ndss-symposium.org/wp-content/uploads/2025-775-paper.pdf)

#### Dissecting Payload-based Transaction Phishing on Ethereum

- **作者**: Zhuo Chen, Yufeng Hu, Bowen He, Dong Luo, Lei Wu, Yajin Zhou (Zhejiang University)
- **方向**: 社会工程与人类因素
- **解读**: 以太坊上出现超越早期简单转账钓鱼的新型钓鱼——基于负载的交易钓鱼（PTXPHISH）：攻击者通过执行恶意payload操纵智能合约交互来欺骗用户，2023年相关事件损失超7000万美元。尽管危害巨大，此前无系统研究。本文给出首个PTXPHISH的全面研究，覆盖数据收集、手法分类、检测、大规模测量与缓解。
作者建立首个包含5000个钓鱼交易的地面真值数据集，把钓鱼手法细分为四大类、十一子类（冰钓Ice Phish、NFT订单、地址投毒、可支付函数等）；提出基于规则的多维检测方法，F1超过99%，平均每区块处理390毫秒；随后开展300天大规模检测。
共发现130,637笔钓鱼交易，损失超3.419亿美元。深度分析还揭示：骗子每天消耗约13.4 ETH（占全网gas的12.5%）用于传播地址投毒；头部五个钓鱼组织造成40.7%的全部损失。缓解贡献方面，报告了1726个钓鱼地址（占同期社区贡献的42.7%），发送2539条链上预警消息，帮助1980名受害者。
我认为这是对链上社会工程攻击的一次系统性"解剖"，其分类学和检测方法可直接服务钱包与浏览器的实时防护；量化gas消耗与组织集中度揭示了攻击经济学的关键特征。局限是规则方法依赖已有模式、对新变种可能滞后，损失统计基于链上推断。相比早期简单交易钓鱼研究，PTXPHISH首次覆盖了利用EVM语义的攻击面。

- **论文 PDF**: [Dissecting Payload-based Transaction Phishing on Ethereum](https://www.ndss-symposium.org/wp-content/uploads/2025-311-paper.pdf)

#### Ctrl+Alt+Deceive: Quantifying User Exposure to Online Scams

- **作者**: Platon Kotzias, Michalis Pachilakis, Javier Aldana Iuit, Juan Caballero, Iskander Sanchez-Rola, Leyla Bilge

- **方向**: 社会工程与人类因素

- **解读**: 在线诈骗已成为互联网用户的首要威胁之一，2023 年仅在美国就造成 100 亿美元损失；此前研究只针对单一诈骗类型，从未对不同诈骗类型进行横向比较，用户总体暴露面缺乏量化刻画。本文开展了据作者所知首次的终端用户对不同类型在线诈骗暴露情况的系统性研究。

方法上，研究覆盖七类流行诈骗：购物、金融、加密货币、赌博、约会、资金追回与就业诈骗；依托大型安全厂商数月的遥测数据，搜索数百万桌面与移动设备对 607K 个诈骗域的访问记录，将其分类为七类并逐类测量用户暴露、地域差异、诈骗域生命周期以及通过在线广告推广诈骗网站的情况。

结果显示：共考察 25.1M 个 IP 地址对 414K+ 诈骗域的访问；每日约 149K 台设备暴露于诈骗，其中桌面设备平均 101K 台（0.8%），移动设备 48K 台（0.3%）；购物诈骗最普遍（共 10.2M 个 IP 观测），其次为加密货币诈骗（653K 个 IP）；诈骗域被观测到后中位存活 11 天；至少 9.2M 次（13.3%）诈骗观测中用户点击了广告，其中 59% 的广告托管于社交媒体，Facebook 是首选来源。

我的思考：首次以统一方法学对多类诈骗做端到端暴露测量，提供了诈骗生态全景式量化数据，对用户教育、平台治理与监管均有直接价值；广告作为主要传播渠道（13.3% 观测经广告触达、59% 落在社交媒体）的发现，把矛头指向了广告生态的审核缺口。局限是遥测来自单一厂商用户群，存在选择偏差，且"暴露"不等于"受害"，真实经济损失传导链未直接测量。与单类型诈骗研究相比，跨类型比较揭示了购物诈骗的主导地位与差异化的地域分布。

- **论文 PDF**: [Ctrl+Alt+Deceive: Quantifying User Exposure to Online Scams](https://www.ndss-symposium.org/wp-content/uploads/2025-1816-paper.pdf)

#### Characterizing the Impact of Audio Deepfakes in the Presence of Cochlear Implant

- **作者**: Magdalena Pasternak, Kevin Warren, Daniel Olszewski, Susan Nittrouer, Patrick Traynor, Kevin Butler

- **方向**: 社会工程与人类因素

- **解读**: 人工耳蜗（CI）让失聪与听障人士得以使用手机、语音助手等音频设备，而日益逼真的合成音频（深度伪造）可能威胁这一群体；但该人群对深度伪造的易感性此前完全未知。本文开展首次针对 CI 人群的音频深度伪造影响研究，切入深度伪造安全研究长期忽略的边缘化用户群体。

方法上，作者先在深度伪造检测器中考察 CI 模拟音频的使用效果，再据此对 35 名 CI 用户与 87 名听力正常者（HP）开展用户研究，比较两类人群感知深度伪造音频的差异；并评估在 CI 模拟音频上训练的检测模型能否有效充当 CI 用户的代理。

结果表明：CI 用户与听力正常者类似，都能识别文本转语音（TTS）生成的深度伪造；但对语音转换（voice conversion）算法生成的深度伪造，CI 用户表现明显更差，正确音频分类率仅 67%；对检测模型作为 CI 用户代理的有效性也进行了评估。

我的思考：本文把深度伪造安全研究拓展到无障碍用户群体，具有重要的公平性意义——TTS 与语音转换两类合成在 CI 用户感知上的显著差异（67% 正确率），说明更复杂的转换式合成可能对 CI 用户构成实质性欺骗风险。局限是 CI 模拟音频与实际植入体验存在差距、样本量偏小（35 名 CI 用户），结论外推需谨慎；检测模型能否真正代理 CI 用户感知也需更多验证。与面向普通人群的深度伪造检测相比，本文提示安全对策必须考虑感知能力的群体差异。

- **论文 PDF**: [Characterizing the Impact of Audio Deepfakes in the Presence of Cochlear Implant](https://www.ndss-symposium.org/wp-content/uploads/2025-1117-paper.pdf)

#### “Where Are We On Cyber?” – A Qualitative Study On Boards' Cybersecurity Risk Decision Making

- **作者**: Jens Christian Opdenbusch (Ruhr University Bochum), Jonas Hielscher (Ruhr University Bochum), M. Angela Sasse (Ruhr University Bochum, University College London)
- **方向**: 社会工程与人类因素
- **解读**: 董事会日益被要求监督组织的网络安全风险，但决策必须依赖 CISO、高管报告、审计与监管提供的信息；此前对董事会如何基于这些信息决策、以及其与利益相关者的关系如何塑造决策知之甚少。

作者对 n=18 名受访者开展深度访谈，对象包括英国规模最大公司的 C 级高管、董事会成员、CISO 与 C 级顾问，以定性方法考察董事会的网络安全风险决策过程。

研究发现存在权力失衡：董事会成员因担心被暴露为 IT 新手，常常不向高管和 CISO 提出正确的问题，最终使董事会高度依赖信息提供者、丧失监督职能；网络安全风险被抽象为预算决策，董事会不再进一步参与网络安全战略。作者讨论了强化监督的可能途径，如由公共网络机构发布行业基准，或在公司内部设立常设（网络）风险与审计委员会等支持结构。

该研究填补了董事会层面网络安全决策这一高层治理研究的空白，揭示信息不对称与权力失衡如何侵蚀监督职能，对强制性披露监管与 CISO-董事会沟通实践具有直接启示。局限在于样本仅 18 人且集中于英国大公司，结论的外部效度有限，定性方法亦难量化失衡程度。与聚焦终端用户的既有安全人因研究相比，该工作把视角上移到治理层，呼应了安全治理问责这一日益受关注的方向。

- **论文 PDF**: [“Where Are We On Cyber?” – A Qualitative Study On Boards' Cybersecurity Risk Decision Making](https://www.ndss-symposium.org/wp-content/uploads/2025-595-paper.pdf)

### 程序分析与形式化验证（9 篇）

#### VeriBin: Adaptive Verification of Patches at the Binary Level

- **作者**: Hongwei Wu, Jianliang Wu, Ruoyu Wu, Ayushi Sharma, Aravind Machiry, Antonio Bianchi
- **方向**: 程序分析与形式化验证
- **解读**: 厂商常只收到二进制形式的补丁，无法保证其不破坏原有功能，导致"补丁缺口"；XZ Utils 后门更证明源码级审查可被篡改编译流水线绕过，二进制级补丁验证在源码可得时也必要。

VERIBIN 把"安全可应用（StA）"形式化为有效输入空间不扩大与输出等价（非局部写、返回值、函数调用等价）四条性质，用欠约束符号执行枚举受影响函数路径，以启发式识别错误处理路径，用匹配路径对（MPP）降低 75% 验证时间，并处理编译器偏移变化，验证失败时向分析师提出可读问题做自适应确认。

在 86 对二进制（MicroPatch/AMP/PatchDB）上准确率 93.0%、零误报，平均 1293.8 秒；剥离符号后 89.4%；优于源码级工具 SPIDER（62.0% 准确率、27.9% 误报），并成功检出 XZ Utils 后门（多出 __get_cpuid 调用违反 P4）。

首次把补丁验证系统化推进到二进制层面，"零误报+自适应问答"务实；但适用率仅 68.8%（超时/超内存为主）、循环只展开两次、需预先给出受影响函数地址，属高保证低吞吐方案，适合发布前离线验证。

- **论文 PDF**: [VeriBin: Adaptive Verification of Patches at the Binary Level](https://www.ndss-symposium.org/wp-content/uploads/2025-359-paper.pdf)

#### Unleashing the Power of Generative Model in Recovering Variable Names from Stripped Binary

- **作者**: Xiangzhe Xu, Zhuo Zhang, Zian Su, Ziyang Huang, Shiwei Feng, Yapeng Ye, Nan Jiang, Danning Xie, Siyuan Cheng, Lin Tan, Xiangyu Zhang
- **方向**: 程序分析与形式化验证
- **解读**: 反编译中的变量名恢复需同时理解机器语义与开发者命名语义；现有分类式方法（VarBERT）只能从训练词汇表选名，长尾分布（50% 名字仅出现一次）使低频名精度从 31.8% 跌至 13.5%，且单函数分析丢失调用上下文。

GENNM 微调生成式代码大模型（CodeGemma-2B/CodeLlama-7B/34B）：上下文感知微调沿调用图把调用者调用点与被调用者签名中预测的名字传播给目标函数；SymPO 借鉴 DPO 用模型自身 top-20 预测构造成对偏好数据、校准符号偏好；迭代推理配合数据流一致性分析与多数投票选名。

DIRTY 上精度/召回 35.8/33.9，VarCorpus 上 41.2/39.3，比 VarBERT/ReSym 提升 5.6–11.4 个百分点；"项目未见训练"最难设置下 22.8% vs ReSym 17.3%、VarBERT 8.5%（提升 32% 与 168%）；低频名（频率 1–10）精度 27.1%，远超 VarBERT 的 13.5%，且 95% 罕见名由常见 token 组成；34B 底座（LoRA）整体最优。

把变量名恢复从"词汇内分类"转为"生成式组合"，天然对齐开发者拼词认知，SymPO 免人工标注消解频率偏置；但最强配置需数十亿参数模型与重算力，对 -O2/-O3 激进优化产物的理解仍是短板，评估含 GPT-4 主观成分。

- **论文 PDF**: [Unleashing the Power of Generative Model in Recovering Variable Names from Stripped Binary](https://www.ndss-symposium.org/wp-content/uploads/2025-276-paper.pdf)

#### Uncovering the iceberg from the tip: Generating API Specifications for Bug Detection via Specification Propagation Analysis

- **作者**: Miaoqian Lin, Kai Chen, Yi Yang, Jinghua Liu
- **方向**: 程序分析与形式化验证
- **解读**: API 误用（缺少返回值检查、资源释放等事后处理）占 API 误用的 66%，检测依赖全面规范；而基于文档、使用模式、补丁的提取方法受工件质量限制，大量 API 无规范覆盖，造成误用检测大面积漏报。

提出"API 规范传播"：规范沿分层调用链传播。APISpecGen 以已知规范三元组为种子，经调用者/被调用者双向传播分析识别后继与前驱（关键变量以参数/返回值传播、无杀死操作），结合真实使用模式与数据流验证确定后处理操作，迭代生成新规范并用于误用检测。

在 Linux 内核用 6 条种子两小时生成 7,332 条规范（抽样 763 条全部正确），检出 186 个新 bug、113 个获开发者确认、8 个 CVE；Advance/SinkFinder/APHP 覆盖率仅 10–21%、IPPO 为 0；87% 规范不见于 API 文档，还发现 3 处文档错误；OpenSSL/FFmpeg 验证可推广。

"规范沿调用链传播"观察价值显著，6 条种子覆盖数千 API，秒级吞吐、内存低于 2GB；但误报 42%/漏报 11%，只覆盖后处理一类规范，关键变量识别依赖命名习惯，10 轮迭代上限与 Linux 生态重心需在更多项目检验。

- **论文 PDF**: [Uncovering the iceberg from the tip: Generating API Specifications for Bug Detection via Specification Propagation Analysis](https://www.ndss-symposium.org/wp-content/uploads/2025-2001-paper.pdf)

#### Translating C To Rust: Lessons from a User Study
- **作者**: Ruishi Li (National University of Singapore), Bo Wang (National University of Singapore), Tianyu Li (National University of Singapore), Prateek Saxena (National University of Singapore), Ashish Kundu (Cisco Research)
- **方向**: 程序分析与形式化验证
- **解读**: 问题：C 程序内存安全漏洞每年产生数千 CVE，运行时插桩防御开销（≥50%）难以部署。Rust 提供全内存安全，但把存量 C 翻译到 Rust 有多难？自动工具效果有限（静态分析仅约 11% 的裸指针可转为安全引用；LLM 方法对超过 150 行的 C 程序成功率不足 20%），人类用户的翻译策略是重要的对照参照。

方法：首个 C-to-Rust 翻译用户研究：73 名安全课程本科生参与，33 人同意数据分析；每人 20 天内将 8 个真实 C 程序（322-536 行，来自 BSD CoreUtils 与 shoco/urlparser）翻译为纯 safe Rust（禁用 unsafe），自建测试要求对 C 程序行覆盖≥85%；随后用 AFL++ 差分模糊测试找出行为差异，并与 4 个自动工具对比。

结果：31 份可编译翻译，行覆盖 70%-98%，26 份通过 85% 覆盖测试；3 个已知漏洞全部消除，5/8 基准的 Rust 版本比 C 更快，多数开销低于 10%-20%；但差分模糊显示 37%-100%（平均 68%）的模糊测试用例存在行为差异，含逻辑错误，即"最后一英里"问题；Laertes/Crown 生成的安全引用占比不足 3.1%；31/33 名用户尝试过 LLM，20/31 认为 LLM 生成代码易错。

我的思考：贡献是稀缺的实证数据：人类通过"语义提升"（用 String、Vec 等高层类型重表达逻辑而非逐行翻译）实现零成本时间安全，而自动工具仍停留在低层指针映射。局限：参与者为学生、程序规模小、正确性依赖自建测试；行为差异率高说明"安全但不等价"普遍存在。启示明确：自动翻译器应学习类型提升与生命周期分解策略，翻译正确性需要比单元测试更强的差分验证手段。
- **论文 PDF**: [Translating C To Rust: Lessons from a User Study](https://www.ndss-symposium.org/wp-content/uploads/2025-1407-paper.pdf)

#### The Midas Touch: Triggering the Capability of LLMs for RM-API Misuse Detection
- **作者**: Yi Yang, Jinghua Liu, Kai Chen, Miaoqian Lin
- **方向**: 程序分析与形式化验证
- **解读**: 资源管理（RM）API 常成对出现（分配/释放），误用导致内存泄漏、use-after-free、double-free 等漏洞。现有方法都有局限：文档关键词法至少漏检 76.28% 的 RM-API；代码高频用法法把错误用法当正确引入假 bug；传统程序分析难以处理多层嵌套（72.7% 的分配函数间接调用 malloc）；而直接使用 LLM 会幻觉——编造不存在的释放函数（如 evwatch_check_free）或给出与证据矛盾的回答。

作者提出 ChatDetector，受 ReAct 框架启发把任务分解：①从 API 官方文档提取 RM 句，用"二维提示"（询问哪个 API 分配 / 该 API 是否分配）交叉验证识别分配 API，并引入语义角色标注（SRL）对 LLM 输出与推理过程做一致性检查，抑制"句子说释放却答分配"的幻觉；②先对函数声明做结构化预处理、提取 RM 对象类型，再据此配对释放 API；③将 RM-API 对写成 QL 查询，用 CodeQL 检测三类漏洞。

在 FFmpeg、Libevent、Libexpat、Libpcap、Libzip、OpenLdap 六个库上识别出 165 对 RM-API，分配 API 识别精度 92.81%、配对精度 98.21%，比基准方法多发现 80.85% 的约束，远超 Advance、HERO、Goshawk（如 Libevent 56 对 vs 4/1/6）；共报告 115 个安全漏洞（64 个获开发者确认，确认精度 90.2%，含 HandBrake 的 double-free 已修复）；APIMU4C 标准集检出 14/15。消融显示外部文档知识与 CoT+交叉验证贡献显著（F1 从 30.86% 提升到 94.28%）。

这是首个用 LLM 理解官方文档抽取 API 使用约束做误用检测的工作，证明 LLM 能发现超越专家经验之外的约束（52.17% 的 RM 句不含常用关键词），并打开了"LLM 辅助静态分析"的新范式。局限：黑盒 LLM 引入的假阳难以解释、对无文档 API 失效、新发现的 RM-API 实际使用率偏低（38 个新对仅 34.21% 被使用），但方向性价值显著。
- **论文 PDF**: [The Midas Touch: Triggering the Capability of LLMs for RM-API Misuse Detection](https://www.ndss-symposium.org/wp-content/uploads/2025-816-paper.pdf)

#### Silence False Alarms: Identifying Anti-Reentrancy Patterns on Ethereum to Refine Smart Contract Reentrancy Detection
- **作者**: Qiyang Song (Institute of Information Engineering, Chinese Academy of Sciences; School of Cyber Security, University of Chinese Academy of Sciences), Heqing Huang (Institute of Information Engineering, Chinese Academy of Sciences), Xiaoqi Jia (Institute of Information Engineering, Chinese Academy of Sciences; School of Cyber Security, University of Chinese Academy of Sciences), Yuanbo Xie (Institute of Information Engineering, Chinese Academy of Sciences; School of Cyber Security, University of Chinese Academy of Sciences), Jiahao Cao (Institute for Network Sciences and Cyberspace, Tsinghua University)
- **方向**: 程序分析与形式化验证
- **解读**: 
重入漏洞曾致 DAO 事件巨额损失，但自动化检测器误报率估计超 90%，分析师平均每合约需约 30 分钟调查；大量误报源于粗糙检测规则把受防重入模式保护的合约误判为脆弱，而缺乏专门识别防重入模式的自动化工具。

AutoAR 由三部分组成：数据过滤——基于"易受重入合约常带防重入模式"的观察，用三种模式做静态分析选出候选合约并排除 CEI 保护与 60 个历史已确认脆弱合约；RentPDG——仅保留与外部调用相关的节点/边的过程间 PDG 变体，用 CFL 文法（括号匹配）加符号邻接矩阵做上下文敏感可达性分析，排除不可行路径；识别模型——异构图卷积加注意力池化的图自编码器产出图嵌入，经 Genieclust 聚类，以簇心与检测阈值识别防重入模式。

从 115,240 个真实合约中无监督学得 12 种防重入模式（其中 P4-5、P7-12 共 8 种为新发现）；6 个 SOTA 检测器（Mythril、Slither、Securify、Conkas、Smartian、Sailfish）至少忽略其中 8 种，Securify 与 Mythril 全部忽略；识别平均准确率 89%，阈值 [1.8σ,2.8σ] 下 recall 超 85% 且 FPR 为 0；集成后六工具误报平均降低 85% 以上（Smartian 达 94%），真阳性零损失。

首次用无监督图学习系统化探索防重入模式，8 个新发现模式可直接指导检测器设计与安全开发；上下文敏感 RentPDG 解决不可行路径问题具有理论价值；局限是 RentPDG 构建耗时（训练阶段 166.7 小时）、模式需人工归纳验证。
- **论文 PDF**: [Silence False Alarms: Identifying Anti-Reentrancy Patterns on Ethereum to Refine Smart Contract Reentrancy Detection](https://www.ndss-symposium.org/wp-content/uploads/2025-167-paper.pdf)

#### PropertyGPT: LLM-driven Formal Verification of Smart Contracts through Retrieval-Augmented Property Generation
- **作者**: Ye Liu, Yue Xue, Daoyuan Wu, Yuqiang Sun, Yi Li, Miaolei Shi, Yang Liu (Singapore Management University; MetaTrust Labs; HKUST; NTU)
- **方向**: 程序分析与形式化验证
- **解读**: 形式化验证能证明智能合约正确性，但关键瓶颈是属性（不变量、前置/后置条件、规则）的自动化生成——行业标杆Certora仍靠专家逐案手写；现有InvCon/Cider/SmartInv等只能推断部分不变量，且依赖历史交易或标注训练。这阻碍了形式化验证在DeFi等场景的大规模应用。

PropertyGPT采用检索增强生成：把623条人工编写属性（来自23个Certora项目）嵌入向量库，为未知代码检索最相似参考属性，供GPT-4做上下文学习生成新属性；为保证(i)可编译，用编译与静态分析反馈作外部oracle迭代修订；(ii)合适，从多维相似度加权排序取Top-K；(iii)可验证，设计专用prover形式化验证生成属性；并配套属性规范语言PSL。

相对人工ground truth达到80%召回、64%精度；检测真实漏洞时命中37个测试中的26个CVE/攻击事件（9/13 CVE、17/24事件）；在4个真实赏金项目上生成22个发现，其中12个被确认并修复，共获$8,256赏金，即发现12个零日漏洞。

该工作打通"LLM生成属性→形式化验证"的自动化流水线，让属性生成从专家手工走向可扩展，且被MetaTrust商业化、真实拿到赏金，证明实用价值；局限在于64%精度意味着相当比例属性不成立，质量依赖知识库覆盖，对全新合约模式的泛化存疑，与静态分析工具是互补而非替代关系。是LLM驱动程序验证的代表性工作。
- **论文 PDF**: [PropertyGPT: LLM-driven Formal Verification of Smart Contracts through Retrieval-Augmented Property Generation](https://www.ndss-symposium.org/wp-content/uploads/2025-1357-paper.pdf)

#### MTZK: Testing and Exploring Bugs in Zero-Knowledge (ZK) Compilers
- **作者**: Dongwei Xiao, Zhibo Liu, Yiteng Peng, Shuai Wang
- **方向**: 程序分析与形式化验证
- **解读**:
零知识证明（ZKP）广泛应用于隐私计算与区块链，行业设计了 DSL 与 ZK 编译器把程序自动编译成电路。然而"电路与程序语义等价"对任意程序不可判定，编译器又含数十万行复杂优化管线，其 bug 会把合法程序编译成错误电路，使验证者接受无效证明，曾致数亿美元级资产损失，此前却缺乏系统化测试方法。

MTZK 采用蜕变测试：设计两个语义保持的蜕变关系——MRSIM（可满足性不变变异）插入必然可满足的约束（如 x==v、x==x*1），编译执行后结果必须仍为 SAT；MRIVM（信息可见性变异）把随机表达式抽取为新的 public/private 输入，搅动编译器的可见性传播与优化。程序经自定义 IR 生成、黑盒且协议无关，覆盖 ZoKrates、Noir、Cairo、Leo 四种工业编译器。

MTZK 生成 3 万程序，发现 10,107 个错误触发输入（6,716 编译失败 + 3,391 逻辑错误），去重后 21 个独立 bug（ZoKrates 7、Noir 7、Cairo 5、Leo 2），16 个获确认、15 个已修复；24 小时行覆盖率 38.0%–55.2%；补丁平均 345 行。对比 RPG/EMI 各仅 4 个编译失败 bug；案例表明 bug 可拒掉合法证明或在电路中留下后门。

这是首个系统化测试 ZK 编译器的工作，MR 设计紧扣 ZK 的约束与隐私可见性特性，绕开传统 fuzzing 无 oracle 与 EMI 不适配的问题，假阳性率低于 3%。局限：测试不能证明无 bug，补丁复杂度反映编译器内部耦合之深。对 zkRollups 与隐私应用的安全基线有重要价值。

- **论文 PDF**: [MTZK: Testing and Exploring Bugs in Zero-Knowledge (ZK) Compilers](https://www.ndss-symposium.org/wp-content/uploads/2025-530-paper.pdf)

#### Generating API Parameter Security Rules with LLM for API Misuse Detection

- **作者**: Jinghua Liu, Yi Yang, Kai Chen, Miaoqian Lin
- **方向**: 程序分析与形式化验证
- **解读**: API 参数安全规则（APSR）规定 API 参数的取值与操作约束，违反会导致空指针解引用、内存破坏等严重问题。人工构造 APSR 劳动密集，文档挖掘与代码模式分析方案漏规则，直接使用 LLM 又产生错误规则（准确性仅 11.9%）与过度泛化的规则。本文提出 GPTAid，用 LLM 分析 API 源码自动生成 APSR 并检测 API 误用。

方法上分四阶段：①逐参数以链式思考提示 LLM 分析源码并给出违规代码示例，生成原始 APSR（召回 84.4%）；②基于"94% 的误用导致运行时错误"的观察，让 LLM 生成正确调用代码（自动修复，成功率 93.5%）再改造为违规代码，用 ASAN/Valgrind 动态执行并分析栈回溯过滤无关错误（验证精度 96%、召回 80.2%）；③对产生相同运行时错误的违规代码做差分分析，识别共享关键操作，精化为具体 APSR（精度 91.5%）；④用 CodeQL 结合五个检测模板检测误用。

在 OpenSSL、SQLite、libpcap、libxml2、libevent、libzip、zlib、libcurl 八个库随机选取的 200 个 API 上，每 API 0.12 美元生成 311 条 APSR，精度 92.3%、召回 71.0%，识别出八类规则（比先前工作多两类）。对比中生成 53 条，是文档驱动工具 Advance（7 条）的 6 倍、Goshawk（8 条）的 6 倍多，IPPO 检出 0 个 bug。47 个应用检出 210 个未知 bug（150 个获开发者确认），APIMU4C 标准集 12 个检出 11 个。消融实验显示直接 LLM 的 F1 仅 0.21，加验证达 0.74，再加精化达 0.80。

我的思考：GPTAid 用"可执行性"裁判自然语言规则的正确性，把 LLM 幻觉转化为可自动验证的运行时错误判别，差分分析又把泛化规则收敛为代码级约束，形成闭环。局限：不产生运行时错误的规则（如漏解锁）被验证机制丢弃；检测采用过程内分析，45.2% 误报源于跨函数检查缺失；关键操作识别依赖 LLM。"执行反馈校验 LLM 输出"的思路可迁移到其他安全代码生成场景。

- **论文 PDF**: [Generating API Parameter Security Rules with LLM for API Misuse Detection](https://www.ndss-symposium.org/wp-content/uploads/2025-465-paper.pdf)
