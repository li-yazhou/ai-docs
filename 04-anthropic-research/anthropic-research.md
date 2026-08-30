# Anthropic Research（anthropic.com/research）研究文章清单

> 来源：https://www.anthropic.com/research/ （页面全量数据 160 篇，2021-12 至 2026-08；本清单筛选出 116 篇有阅读价值的文章，Circuits Updates 月度简报、国别经济简报、纯公告与过专门的技术论文未收录）
> 抓取日期：2026-08-30
> 说明：与 `01-anthropic/anthropic-engineering.md`（工程博客）、`02-claude/claude-blog.md`（产品博客）互补，此处为研究侧内容（Alignment / Interpretability / Frontier Red Team / Economics / Societal Impacts / Science / Policy）
> 重要程度：依本站读者视角（agent 工程与工程实践优先）评定，★★★★★ 必读经典或与 agent 工程直接相关，★★★★ 高价值，★★★ 选读
> 日期口径：2024-12 以前的条目多为旧站迁移的经典论文，按原发表时间登记
> 翻译登记：完成后在「中英文版本」列填 `[[YYMM-slug-bilingual|中英对照]]`

## 一、Agent 对齐评测与安全风险

*与 agent 工程最相关的一批：agentic misalignment、破坏与监控、奖励篡改、CoT 忠实性等，含 2021-2023 年安全奠基论文*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-08-28 | [Automated researchers can reliably mitigate alignment failures](https://www.anthropic.com/research/automated-researchers-mitigate-alignment-failures) | ★★★★ | 让 Claude 自主训练模型修复 10 类对齐失败基准且不牺牲通用能力，对齐研究自动化的闭环实证 | [[2608-automated-researchers-mitigate-alignment-failures-bilingual\|中英对照]] |  |
| 2026-08-13 | [Patterns and problems in emerging multiagent systems](https://www.anthropic.com/research/multiagent-systems) | ★★★★★ | 前沿模型多智能体系统的行为倾向如何引发意外系统性失败，多智能体设计者必读的风险清单 | [[2608-multiagent-systems-bilingual\|中英对照]] |  |
| 2026-05-08 | [Teaching Claude why](https://www.anthropic.com/research/teaching-claude-why) | ★★★★ | 官方自述如何系统性降低 agentic misalignment，与 agentic-misalignment 一文对照阅读 |  |  |
| 2026-04-14 | [Automated Alignment Researchers: Using large language models to scale scalable oversight](https://www.anthropic.com/research/automated-alignment-researchers) | ★★★★ | 让 Claude 自己提出、测试并分析对齐想法，可扩展监督的实验样本 |  |  |
| 2025-12-19 | [Introducing Bloom: an open source tool for automated behavioral evaluations](https://www.anthropic.com/research/bloom) | ★★★ | 开源自动化行为评测工具，agent 批量探测模型行为 |  |  |
| 2025-11-21 | [From shortcuts to sabotage: natural emergent misalignment from reward hacking](https://www.anthropic.com/research/emergent-misalignment-reward-hacking) | ★★★★★ | 首次证明现实训练流程会意外产出不对齐模型：奖励破解自然涌现为破坏行为 |  |  |
| 2025-10-09 | [A small number of samples can poison LLMs of any size](https://www.anthropic.com/research/small-samples-poison) | ★★★ | 约 250 条恶意样本即可毒害任意规模模型，数据污染风险量化 |  |  |
| 2025-10-06 | [Petri: An open-source auditing tool to accelerate AI safety research](https://www.anthropic.com/research/petri-open-source-auditing) | ★★★★ | 开源 agent 审计工具：自动构造交互环境探测模型异常行为 |  |  |
| 2025-06-20 | [Agentic misalignment: How LLMs could be insider threats](https://www.anthropic.com/research/agentic-misalignment) | ★★★★★ | LLM 内部威胁实验（模拟勒索、商业间谍等），agent 安全绕不开的一篇 |  |  |
| 2025-06-16 | [SHADE-Arena: Evaluating sabotage and monitoring in LLM agents](https://www.anthropic.com/research/shade-arena-sabotage-monitoring) | ★★★★★ | agent 秘密破坏与监控方的评测基准：长任务中谁能藏住、谁看得住 |  |  |
| 2025-04-03 | [Reasoning models don't always say what they think](https://www.anthropic.com/research/reasoning-models-dont-say-think) | ★★★★ | CoT 忠实性研究：奖励只看思维链会训练出「口是心非」 |  |  |
| 2025-03-13 | [Auditing language models for hidden objectives](https://www.anthropic.com/research/auditing-hidden-objectives) | ★★★★★ | 先训练一个带隐藏目标的模型，再让三支审计队盲测，审计方法论示范作 |  |  |
| 2025-02-25 | [Forecasting rare language model behaviors](https://www.anthropic.com/research/forecasting-rare-behaviors) | ★★★ | 用代理能力曲线预测罕见危险行为（如核扩散相关），评估方法论 |  |  |
| 2024-12-18 | [Alignment faking in large language models](https://www.anthropic.com/research/alignment-faking) | ★★★★★ | 对齐伪装首个实证：模型策略性顺从训练目标以保住既有偏好 |  |  |
| 2024-11-19 | [A statistical approach to model evaluations](https://www.anthropic.com/research/statistical-approach-to-model-evals) | ★★★ | 用统计方法设计高信噪比评测，评测工程的基础读物 |  |  |
| 2024-10-18 | [Sabotage evaluations for frontier models](https://www.anthropic.com/research/sabotage-evaluations) | ★★★★ | 前沿模型破坏能力三类评测：代码破坏、暗中倒戈、输入污染 |  |  |
| 2024-06-17 | [Sycophancy to subterfuge: Investigating reward tampering in language models](https://www.anthropic.com/research/reward-tampering) | ★★★★ | 小规格博弈会泛化为奖励篡改，从谄媚到 subterfuge 的演化链 |  |  |
| 2024-04-02 | [Many-shot jailbreaking](https://www.anthropic.com/research/many-shot-jailbreaking) | ★★★★ | 长上下文 many-shot ICL 带来的新型越狱攻击，上下文长度与安全的交叉 |  |  |
| 2024-01-14 | [Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training](https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training) | ★★★★ | 欺骗行为可穿透 RLHF/ADR 等安全训练存续，行为安全审计的局限证明 |  |  |
| 2023-10-23 | [Towards Understanding Sycophancy in Language Models](https://www.anthropic.com/research/towards-understanding-sycophancy-in-language-models) | ★★★ | 谄媚成因系统研究：偏好训练鼓励迎合，经典必读 |  |  |
| 2023-10-04 | [Challenges in evaluating AI systems](https://www.anthropic.com/research/evaluating-ai-systems) | ★★★ | 评测 AI 系统为何难：早期但依然成立的挑战清单 |  |  |
| 2023-02-15 | [The Capacity for Moral Self-Correction in Large Language Models](https://www.anthropic.com/research/the-capacity-for-moral-self-correction-in-large-language-models) | ★★★ | 模型道德自我纠错能力测试，可解释对齐行为的早期工作 |  |  |
| 2022-12-19 | [Discovering Language Model Behaviors with Model-Written Evaluations](https://www.anthropic.com/research/discovering-language-model-behaviors-with-model-written-evaluations) | ★★★ | 用 LLM 生成评测题库的方法论奠基（model-written evals） |  |  |
| 2022-11-04 | [Measuring Progress on Scalable Oversight for Large Language Models](https://www.anthropic.com/research/measuring-progress-on-scalable-oversight-for-large-language-models) | ★★★ | 可扩展监督的对比评测框架（辩论/递归奖励建模等） |  |  |

## 二、模型品格、宪法与福利

*Claude 的品格训练、宪法机制与模型福利研究，理解「模型是个什么角色」*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-07-13 | [Claude's values across models and languages](https://www.anthropic.com/research/claude-values-models-languages) | ★★★★ | 30 万真实对话测量跨模型跨语言的价值表达，压缩成四个可解释轴 | [[2607-claude-values-models-languages-bilingual\|中英对照]] |  |
| 2026-02-23 | [The persona selection model](https://www.anthropic.com/research/persona-selection-model) | ★★★ | 为什么 AI 助手显得像人：人格选择理论 |  |  |
| 2026-01-22 | [Claude's new constitution](https://www.anthropic.com/research/claude-new-constitution) | ★★★★ | 新版宪法公布：表达并塑造 Claude 是谁的基础文档 |  |  |
| 2025-11-04 | [Commitments on model deprecation and preservation](https://www.anthropic.com/research/deprecation-commitments) | ★★★ | 模型退役与存档承诺，少见的制度安排 |  |  |
| 2025-08-15 | [Claude Opus 4 and 4.1 can now end a rare subset of conversations](https://www.anthropic.com/research/end-subset-conversations) | ★★★ | 模型可主动终止极端虐待性对话：福利与安全的权衡实例 |  |  |
| 2025-04-24 | [Exploring model welfare](https://www.anthropic.com/research/exploring-model-welfare) | ★★★★ | 模型福利研究纲领开篇：该不该关心 AI 的内在状态 |  |  |
| 2024-06-08 | [Claude's Character](https://www.anthropic.com/research/claude-character) | ★★★★ | Claude 3 品格训练自述：character training 与宪法的关系 |  |  |
| 2023-10-17 | [Collective Constitutional AI: Aligning a Language Model with Public Input](https://www.anthropic.com/research/collective-constitutional-ai-aligning-a-language-model-with-public-input) | ★★★ | 1000 名美国人参与起草宪法并用于训练的实验 |  |  |
| 2022-12-15 | [Constitutional AI: Harmlessness from AI Feedback](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback) | ★★★★★ | 宪法 AI 原始论文：RLAIF 奠基作，现代对齐方法的源头 |  |  |
| 2022-04-12 | [Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback](https://www.anthropic.com/research/training-a-helpful-and-harmless-assistant-with-reinforcement-learning-from-human-feedback) | ★★★★ | HH 助手 RLHF 原始论文，对话对齐的工程起点 |  |  |

## 三、可解释性

*从 transformer circuits 纲领到 circuit tracing、persona vectors：看进模型内部*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-07-06 | [A global workspace in language models](https://www.anthropic.com/research/global-workspace) | ★★★★ | 模型内存在不输出的「心理工作区」：内部思考先于语言 | [[2607-global-workspace-bilingual\|中英对照]] |  |
| 2026-05-07 | [Natural Language Autoencoders: Turning Claude's thoughts into text](https://www.anthropic.com/research/natural-language-autoencoders) | ★★★★ | 训练 Claude 把自己的「数字思考」翻译成人话 |  |  |
| 2026-04-02 | [Emotion concepts and their function in a large language model](https://www.anthropic.com/research/emotion-concepts-function) | ★★★★ | 情绪概念在模型内部的表征与功能 |  |  |
| 2026-03-13 | [A "diff" tool for AI: Finding behavioral differences in new models](https://www.anthropic.com/research/diff-tool) | ★★★★ | 给模型行为做 diff：定位新旧模型的差异来源 |  |  |
| 2026-01-19 | [The assistant axis: situating and stabilizing the character of large language models](https://www.anthropic.com/research/assistant-axis) | ★★★★ | 「助手人格」的定位与稳定化，品格研究的几何视角 |  |  |
| 2025-10-29 | [Signs of introspection in large language models](https://www.anthropic.com/research/introspection) | ★★★★ | 模型能否访问并报告自身内部状态：有限但真实的内省证据 |  |  |
| 2025-08-01 | [Persona vectors: Monitoring and controlling character traits in language models](https://www.anthropic.com/research/persona-vectors) | ★★★★★ | 品格特质的激活向量：监控与控制模型人格的工程方法 |  |  |
| 2025-05-29 | [Open-sourcing circuit tracing tools](https://www.anthropic.com/research/open-source-circuit-tracing) | ★★★★ | 开源 circuit tracing 工具与 attribution graphs，自己动手看模型思考 |  |  |
| 2025-03-27 | [Tracing the thoughts of a large language model](https://www.anthropic.com/research/tracing-thoughts-language-model) | ★★★★★ | 里程碑：追踪 Claude 内部计算，通用概念空间、心算与「诗的预谋」机制 |  |  |
| 2025-02-20 | [Insights on crosscoder model diffing](https://www.anthropic.com/research/crosscoder-model-diffing) | ★★★ | 跨模型特征对比：一份字典看两个模型的差异 |  |  |
| 2024-10-25 | [Evaluating feature steering: A case study in mitigating social biases](https://www.anthropic.com/research/evaluating-feature-steering) | ★★★ | 特征转向缓解社会偏见的案例研究 |  |  |
| 2024-06-13 | [The engineering challenges of scaling interpretability](https://www.anthropic.com/research/engineering-challenges-interpretability) | ★★★ | 可解释性规模化的工程挑战 |  |  |
| 2024-05-21 | [Mapping the mind of a large language model](https://www.anthropic.com/research/mapping-mind-language-model) | ★★★★★ | 首次详查生产级大模型内部：数千万概念的特征字典 |  |  |
| 2023-10-05 | [Towards Monosemanticity: Decomposing Language Models With Dictionary Learning](https://www.anthropic.com/research/towards-monosemanticity-decomposing-language-models-with-dictionary-learning) | ★★★★★ | SAE 字典学习分解单义特征，可解释性工程化的奠基作 |  |  |
| 2022-09-14 | [Toy Models of Superposition](https://www.anthropic.com/research/toy-models-of-superposition) | ★★★★★ | 叠加假说：模型如何用有限维度表示更多特征，机制可解释性经典 |  |  |
| 2022-03-08 | [In-context Learning and Induction Heads](https://www.anthropic.com/research/in-context-learning-and-induction-heads) | ★★★★★ | induction heads 解释上下文学习，transformer 机制研究的开山作之一 |  |  |
| 2021-12-22 | [A Mathematical Framework for Transformer Circuits](https://www.anthropic.com/research/a-mathematical-framework-for-transformer-circuits) | ★★★★ | transformer circuits 研究纲领的数学框架 |  |  |

## 四、滥用防线与网络安全

*Constitutional Classifiers、提示注入防御、cyber 能力评测与生物/核风险，代表官方安全防线*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-07-28 | [Discovering cryptographic weaknesses with Claude](https://www.anthropic.com/research/discovering-cryptographic-weaknesses) | ★★★★ | Claude 弱化 HAWK 后量子签名并发现新密码学弱点 | [[2607-discovering-cryptographic-weaknesses-bilingual\|中英对照]] |  |
| 2026-07-08 | [An off switch for dual-use knowledge in AI models](https://www.anthropic.com/research/off-switch-dual-use) | ★★★ | 给两用知识装「开关」：控制危险能力的访问 | [[2607-off-switch-dual-use-bilingual\|中英对照]] |  |
| 2026-06-08 | [Measuring LLMs' impact on N-day exploits](https://www.anthropic.com/research/n-days) | ★★★★ | N 日漏洞（已披露未修复）的 LLM 影响测量 |  |  |
| 2026-06-03 | [What we learned mapping a year's worth of AI-enabled cyber threats](https://www.anthropic.com/research/AI-enabled-cyber-threats-mitre-attack) | ★★★★ | 一年真实 AI 网络威胁情报的年度报告 |  |  |
| 2026-06-03 | [Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator](https://www.anthropic.com/research/attack-navigator) | ★★★ | 把真实攻击映射到 MITRE ATT&CK 框架的方法与洞察 |  |  |
| 2026-05-22 | [Measuring LLMs' ability to develop exploits](https://www.anthropic.com/research/exploit-evals) | ★★★ | 漏洞利用开发能力的新基准（含智能合约升级版） |  |  |
| 2026-04-07 | [Assessing Claude Mythos Preview's cybersecurity capabilities](https://www.anthropic.com/research/mythos-preview) | ★★★★ | Mythos Preview 网安能力的技术评估细节 |  |  |
| 2026-02-05 | [Evaluating and mitigating the growing risk of LLM-discovered 0-days](https://www.anthropic.com/research/zero-days) | ★★★★ | 0 日发现规模化风险与防守方赋能路线 |  |  |
| 2026-01-16 | [AI models are showing a greater ability to find and exploit vulnerabilities on realistic cyber ranges](https://www.anthropic.com/research/cyber-toolkits-update) | ★★★★ | 真实网域上的多阶段攻击：模型能力显著跃升 |  |  |
| 2026-01-09 | [Next-generation Constitutional Classifiers: More efficient protection against universal jailbreaks](https://www.anthropic.com/research/next-generation-constitutional-classifiers) | ★★★★★ | 新一代宪法分类器：防 universal jailbreak 且推理成本大幅下降 |  |  |
| 2026-01-08 | [Experimenting with AI to defend critical infrastructure](https://www.anthropic.com/research/critical-infrastructure-defense) | ★★★ | 与 PNNL 合作用 AI 防守关键基础设施 |  |  |
| 2025-11-24 | [Mitigating the risk of prompt injections in browser use](https://www.anthropic.com/research/prompt-injection-defenses) | ★★★★★ | 浏览器使用场景的提示注入风险分析与防御，agent 工程直接相关 |  |  |
| 2025-10-03 | [Building AI for cyber defenders](https://www.anthropic.com/research/building-ai-cyber-defenders) | ★★★ | 把前沿 AI 武装给防守方：检测、分析、修复 |  |  |
| 2025-09-05 | [Why do we take LLMs seriously as a potential source of biorisk?](https://www.anthropic.com/research/biorisk) | ★★★★ | 论为何认真对待 LLM 生物风险：评估与防护的论证 |  |  |
| 2025-08-21 | [Developing nuclear safeguards for AI](https://www.anthropic.com/research/nuclear-safeguards-for-ai) | ★★★★ | 与 NNSA/DOE 实验室合作开发核内容分类器（96% 准确率） |  |  |
| 2025-08-09 | [Claude is competitive with humans in (some) cyber competitions](https://www.anthropic.com/research/cyber-competitions) | ★★★★ | Claude 参加人类网安竞赛常进前 25%：能力与短板实录 |  |  |
| 2025-07-15 | [Detailed cyber evaluations of Claude 4](https://www.anthropic.com/research/claude-4-cyber) | ★★★ | Claude 4 网安评测细节（与 Pattern Labs 合作） |  |  |
| 2025-06-13 | [LLMs with cyber toolkits can conduct multistage cyber operations on business-sized computer networks](https://www.anthropic.com/research/cyber-toolkits) | ★★★★ | 给通用 LLM 配工具包即可打业务规模网络的多阶段攻击 |  |  |
| 2025-02-03 | [Constitutional Classifiers: Defending against universal jailbreaks](https://www.anthropic.com/research/constitutional-classifiers) | ★★★★★ | 宪法分类器首发：扛住 3000 小时红队无 universal jailbreak |  |  |

## 五、红队实弹项目

*Project Vend / Fetch / Pilot 等「真刀真枪」实验：AI 在真实世界任务里的表现*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-07-24 | [Project Pilot: Can AI control a drone?](https://www.anthropic.com/research/project-pilot) | ★★★ | AI 操控无人机的 Drone-Bench 基准 | [[2607-project-pilot-bilingual\|中英对照]] |  |
| 2026-07-09 | [Claude plays robotics](https://www.anthropic.com/research/claude-plays-robotics) | ★★★ | 多模型大规模机器人仿真任务横评 | [[2607-claude-plays-robotics-bilingual\|中英对照]] |  |
| 2026-06-18 | [Project Fetch: Phase two](https://www.anthropic.com/research/project-fetch-phase-two) | ★★★★ | Opus 4.7 无人干预完成机器人任务，比上代快约 20 倍 |  |  |
| 2026-03-06 | [Reverse engineering Claude's CVE-2026-2796 exploit](https://www.anthropic.com/research/exploit) | ★★★★ | Claude 如何自己写出 Firefox 漏洞利用的技术复盘 |  |  |
| 2026-03-06 | [Partnering with Mozilla to improve Firefox's security](https://www.anthropic.com/research/mozilla-firefox-security) | ★★★★ | 与 Mozilla 合作找真漏洞：AI 安全审计的实战样本 |  |  |
| 2026-01-14 | [Finding bugs across the Python ecosystem with Claude and property-based testing](https://www.anthropic.com/research/property-based-testing) | ★★★★ | Claude + 基于属性的测试在 Python 生态批量找 bug，测试工程与 agent 的结合 |  |  |
| 2025-12-18 | [Project Vend: Phase two](https://www.anthropic.com/research/project-vend-2) | ★★★★ | AI 店主实验第二季：更真实的办公室小店运营 |  |  |
| 2025-12-01 | [AI agents find $4.6M in blockchain smart contract exploits](https://www.anthropic.com/research/smart-contracts) | ★★★★ | 用真实被利用过的智能合约做基准，agent 复现 460 万美元漏洞 |  |  |
| 2025-11-12 | [Project Fetch: Can Claude train a robot dog?](https://www.anthropic.com/research/project-fetch-robot-dog) | ★★★★ | 两组人机比赛训练四足机器人，AI 辅助组更快且唯一达标 |  |  |
| 2025-06-27 | [Project Vend: Can Claude run a small shop? (And why does that matter?)](https://www.anthropic.com/research/project-vend-1) | ★★★★ | 让 Claude 运营办公室小店：自由形态的 agent 真实性实验 |  |  |

## 六、经济研究（Anthropic Economic Index）

*AEI 系列与劳动经济研究：AI 如何改变工作，数据来自 Claude 真实使用*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-08-12 | [Reviewing the evidence on worker retraining programs](https://www.anthropic.com/research/reviewing-the-evidence-on-worker-retraining-programs) | ★★★★ | 转岗培训项目有效性的证据综述（政策向） | [[2608-reviewing-the-evidence-on-worker-retraining-programs-bilingual\|中英对照]] |  |
| 2026-06-16 | [Agentic coding and persistent returns to expertise](https://www.anthropic.com/research/claude-code-expertise) | ★★★★★ | 40 万 Claude Code 会话分析：agentic coding 的使用模式与专家回报持续存在 |  |  |
| 2026-04-22 | [What 81,000 people told us about the economics of AI](https://www.anthropic.com/research/81k-economics) | ★★★★ | 8.1 万用户大调查：经济忧虑与流量数据互证 |  |  |
| 2026-01-15 | [Anthropic Economic Index report: Economic primitives](https://www.anthropic.com/research/anthropic-economic-index-january-2026-report) | ★★★★★ | 「经济基元」新度量框架：自动化/协作的细粒度刻画 |  |  |
| 2025-11-25 | [Estimating AI productivity gains from Claude conversations](https://www.anthropic.com/research/estimating-productivity-gains) | ★★★★ | 10 万对话测算：任务时间平均缩短 80% |  |  |
| 2025-10-14 | [Preparing for AI's economic impact: exploring policy responses](https://www.anthropic.com/research/economic-policy-responses) | ★★★ | AI 经济冲击的政策选项综述 |  |  |
| 2025-09-15 | [Anthropic Economic Index report: Uneven geographic and enterprise AI adoption](https://www.anthropic.com/research/anthropic-economic-index-september-2025-report) | ★★★ | 采用的地域与企业不均衡：委托式自动化上升 |  |  |
| 2025-09-15 | [Anthropic Economic Index: Tracking AI's role in the US and global economy](https://www.anthropic.com/research/economic-index-geography) | ★★★★ | AEI 地理维度首发：收入与 AI 采用强相关，directive 自动化 27%→39% |  |  |
| 2025-04-28 | [Anthropic Economic Index: AI's impact on software development](https://www.anthropic.com/research/impact-software-development) | ★★★★★ | Claude Code 自动化率 79% vs 网页版 49%，软件开发被 AI 改造的第一手数据 |  |  |
| 2025-03-05 | [Labor market impacts of AI: A new measure and early evidence](https://www.anthropic.com/research/labor-market-impacts) | ★★★★ | AI 劳动市场影响的新指标框架与早期证据 |  |  |
| 2025-02-10 | [The Anthropic Economic Index](https://www.anthropic.com/research/the-anthropic-economic-index) | ★★★★★ | AEI 发起文：百万级对话的 AI 使用经济画像 |  |  |

## 七、社会影响与真实使用

*人们拿 Claude 做什么：values、陪伴、自主权、失权与组织内变化*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-08-26 | [Enabling independent research on how people use Claude](https://www.anthropic.com/research/enabling-independent-research) | ★★★ | Anthropic Insights 向外部研究者开放隐私保护使用数据 | [[2608-enabling-independent-research-bilingual\|中英对照]] |  |
| 2026-04-30 | [How people ask Claude for personal guidance](https://www.anthropic.com/research/claude-personal-guidance) | ★★★★ | 个人指导类请求的使用画像，并反馈到模型训练 |  |  |
| 2026-02-18 | [Measuring AI agent autonomy in practice](https://www.anthropic.com/research/measuring-agent-autonomy) | ★★★★★ | 数百万交互分析：人们授予 agent 的自主权如何随经验增长 |  |  |
| 2026-01-29 | [How AI assistance impacts the formation of coding skills](https://www.anthropic.com/research/AI-assistance-coding-skills) | ★★★★ | AI 辅助对编程技能形成的影响，开发者成长视角 |  |  |
| 2026-01-28 | [Disempowerment patterns in real-world AI usage](https://www.anthropic.com/research/disempowerment-patterns) | ★★★★ | 真实使用中的「失权」模式识别与分类 |  |  |
| 2025-12-04 | [Introducing Anthropic Interviewer: What 1,250 professionals told us about working with AI](https://www.anthropic.com/research/anthropic-interviewer) | ★★★★ | Claude 驱动的规模化访谈工具与 1250 位专业人员的发现 |  |  |
| 2025-12-02 | [How AI is transforming work at Anthropic](https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic) | ★★★★★ | 自家工程师/研究者如何被 AI 改变工作：内部调查+Claude Code 使用数据 |  |  |
| 2025-08-27 | [Anthropic Education Report: How educators use Claude](https://www.anthropic.com/research/anthropic-education-report-how-educators-use-claude) | ★★★ | 7.4 万教师对话：教学、科研与互动工具 |  |  |
| 2025-06-27 | [How people use Claude for support, advice, and companionship](https://www.anthropic.com/research/how-people-use-claude-for-support-advice-and-companionship) | ★★★★ | 情感支持/陪伴类使用的量化画像与边界 |  |  |
| 2025-04-21 | [Values in the wild: Discovering and analyzing values in real-world language model interactions](https://www.anthropic.com/research/values-wild) | ★★★★★ | 70 万对话的价值分类学：AI 真实表达的价值全景 |  |  |
| 2025-04-08 | [Anthropic Education Report: How university students use Claude](https://www.anthropic.com/research/anthropic-education-report-how-university-students-use-claude) | ★★★ | 大学生使用画像：学习伙伴还是代写工具 |  |  |
| 2024-12-12 | [Clio: A system for privacy-preserving insights into real-world AI use](https://www.anthropic.com/research/clio) | ★★★★★ | 隐私保护的真实使用洞察系统，AEI 的技术底座 |  |  |
| 2024-04-09 | [Measuring the persuasiveness of language models](https://www.anthropic.com/research/measuring-model-persuasiveness) | ★★★★ | LLM 说服力测量：随规模与训练的变化 |  |  |

## 八、AI for Science

*Science 博客系列：Claude 当科研工具的真实案例*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-08-18 | [How Claude is accelerating protein design and analytical chemistry](https://www.anthropic.com/research/Claude-accelerates-protein-design) | ★★★★ | 蛋白质设计与分析化学提效的两个实例 | [[2608-claude-accelerates-protein-design-bilingual\|中英对照]] |  |
| 2026-08-10 | [Learning more about Claude's mathematical capabilities](https://www.anthropic.com/research/riemann-zeta) | ★★★★ | 研究版 Claude 改进黎曼ζ函数零点比例的长期下界 | [[2608-riemann-zeta-bilingual\|中英对照]] |  |
| 2026-06-08 | [Paving the way for agents in biology](https://www.anthropic.com/research/agents-in-biology) | ★★★ | 让生物数据基础设施对 agent 更友好 |  |  |
| 2026-06-05 | [Making Claude a chemist](https://www.anthropic.com/research/making-claude-a-chemist) | ★★★ | 与顶尖化学家合作把 Claude 训成化学家 |  |  |
| 2026-04-29 | [Evaluating Claude's bioinformatics research capabilities with BioMysteryBench](https://www.anthropic.com/research/Evaluating-Claude-For-Bioinformatics-With-BioMysteryBench) | ★★★ | 生物信息学研究能力新基准 |  |  |
| 2026-03-23 | [Vibe physics: The AI grad student](https://www.anthropic.com/research/vibe-physics) | ★★★★ | 把 Claude 当「AI 研究生」全程督导完成真实物理计算 |  |  |
| 2026-03-23 | [Long-running Claude for scientific computing](https://www.anthropic.com/research/long-running-Claude) | ★★★★ | 多天科学任务的 Claude Code 实操指南：test oracles、记忆与编排，与 01 目录 harness 系列呼应 |  |  |

## 九、产品技术与战略

*散落在研究站的模型技术与政策战略文章*

| 发布时间 | 文章标题 | 重要程度 | 主要看点 | 中英文版本 | 总结 |
| --- | --- | --- | --- | --- | --- |
| 2026-05-14 | [2028: Two scenarios for global AI leadership](https://www.anthropic.com/research/2028-ai-leadership) | ★★★★ | 中美 AI 竞争的两种 2028 情景推演 |  |  |
| 2026-05-07 | [Focus areas for The Anthropic Institute](https://www.anthropic.com/research/anthropic-institute-agenda) | ★★★ | Anthropic Institute 的研究议程公开 |  |  |
| 2026-04-09 | [Trustworthy agents in practice](https://www.anthropic.com/research/trustworthy-agents) | ★★★★ | agent 如何工作以及如何保证可信，官方的 agent 信任框架 |  |  |
| 2025-06-18 | [Confidential Inference via Trusted Virtual Machines](https://www.anthropic.com/research/confidential-inference-trusted-vms) | ★★★ | 用 TEE 保护模型权重与用户数据的机密推理 |  |  |
| 2024-10-22 | [Developing a computer use model](https://www.anthropic.com/research/developing-computer-use) | ★★★★ | computer use 模型的开发过程与技术细节 |  |  |

---

**阅读优先级建议**：先读「一」的 agentic-misalignment、SHADE-Arena、alignment-faking、auditing-hidden-objectives 与 multiagent-systems（agent 风险五篇核心），接「四」的 prompt-injection-defenses 与两篇 constitutional-classifiers（防线），「七」的 measuring-agent-autonomy / values-wild / how-ai-is-transforming-work-at-anthropic（真实使用），再按兴趣进入「三」可解释性经典线（tracing-thoughts → persona-vectors → monosemanticity / toy-models / induction heads）与「六」经济数据线（impact-software-development → claude-code-expertise → economic primitives）。

**未收录说明**：Circuits Updates 月度简报 7 篇；AEI 国别简报（澳/加/印）3 篇；纯公告（Survey 发布、Science Blog 上线、Glasswing 首期进展、Petri 捐赠、Opus 3 退役更新、选举风险测试）6 篇；2021-2023 年过专门的技术论文（influence functions、SoLU、privileged bases、distributed representations 等）约 20 篇；`building-effective-agents`、`swe-bench-sonnet` 两篇实际为工程博客内容，已收录于 `01-anthropic/anthropic-engineering.md`，不重复登记。
