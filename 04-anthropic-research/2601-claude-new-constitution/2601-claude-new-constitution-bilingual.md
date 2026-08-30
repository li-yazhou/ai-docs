# Claude 的新宪法（中英对照）

> 原文标题：Claude's new constitution
> 原文链接：https://www.anthropic.com/research/claude-new-constitution
> 原文作者：Anthropic
> 发布日期：2026-01-22
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 宪法从原则清单升级为"写给 Claude 读"的完整文档：解释意图与理由、四大优先级排序、硬约束与模型福利并存，CC0 全文公开
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

We're publishing a new constitution for our AI model, Claude. It's a detailed description of Anthropic's vision for Claude's values and behavior; a holistic document that explains the context in which Claude operates and the kind of entity we would like Claude to be.

我们正在发布 AI 模型 Claude 的新宪法。它详细描述了 Anthropic 对 Claude 价值观与行为的愿景；是一份整体性文档，解释 Claude 运行所处的情境，以及我们希望 Claude 成为一种什么样的实体。

The constitution is a crucial part of our model training process, and its content directly shapes Claude's behavior. Training models is a difficult task, and Claude's outputs might not always adhere to the constitution's ideals. But we think that the way the new constitution is written—with a thorough explanation of our intentions and the reasons behind them—makes it more likely to cultivate good values during training.

宪法是模型训练流程的关键部分，其内容直接塑造 Claude 的行为。训练模型是件难事，Claude 的输出未必总是符合宪法的理想。但我们认为，新宪法的写法——对我们的意图及其背后的理由做了透彻解释——更有可能在训练中培养出好的价值观。

In this post, we describe what we've included in the new constitution and some of the considerations that informed our approach.

本文介绍新宪法包含的内容，以及影响我们取舍的一些考量。

We're releasing Claude's constitution in full under a Creative Commons CC0 1.0 Deed, meaning it can be freely used by anyone for any purpose without asking for permission.

我们以 Creative Commons CC0 1.0 协议完整发布 Claude 的宪法：任何人可以为任何目的自由使用它，无需请求许可。

## Claude 的宪法是什么？（What is Claude's Constitution?）

Claude's constitution is the foundational document that both expresses and shapes who Claude is. It contains detailed explanations of the values we would like Claude to embody and the reasons why. In it, we explain what we think it means for Claude to be helpful while remaining broadly safe, ethical, and compliant with our guidelines. The constitution gives Claude information about its situation and offers advice for how to deal with difficult situations and tradeoffs, like balancing honesty with compassion and the protection of sensitive information. Although it might sound surprising, the constitution is written primarily for Claude. It is intended to give Claude the knowledge and understanding it needs to act well in the world.

Claude 的宪法是既表达也塑造"Claude 是谁"的基础文档。它详细解释了我们希望 Claude 体现的价值及其理由。在文中，我们解释了我们认为"Claude 在保持大体安全、合乎伦理、遵守我们准则的同时做到乐于助人"意味着什么。宪法向 Claude 交代它的处境，并就如何处理困难情境与权衡提供建议，比如在诚实与同情、敏感信息保护之间求平衡。虽然听起来可能令人意外，但宪法主要是写给 Claude 看的：它旨在赋予 Claude 在世界上好好行事所需的知识与理解。

We treat the constitution as the final authority on how we want Claude to be and to behave—that is, any other training or instruction given to Claude should be consistent with both its letter and its underlying spirit. This makes publishing the constitution particularly important from a transparency perspective: it lets people understand which of Claude's behaviors are intended versus unintended, to make informed choices, and to provide useful feedback. We think transparency of this kind will become ever more important as AIs start to exert more influence in society.[^1]

我们把宪法当作"Claude 应当如何存在与行事"的最终权威——也就是说，给 Claude 的任何其他训练或指令，都应与宪法的字面及其底层精神保持一致。这使得发布宪法在透明度上格外重要：它让人们能够分辨 Claude 的哪些行为是有意为之、哪些不是，从而做出知情选择、提供有用的反馈。我们认为，随着 AI 开始在社会中施加更大影响，这类透明度将愈发重要。[^1]

We use the constitution at various stages of the training process. This has grown out of training techniques we've been using since 2023, when we first began training Claude models using Constitutional AI. Our approach has evolved significantly since then, and the new constitution plays an even more central role in training.

我们在训练流程的多个阶段使用宪法。它发端于我们自 2023 年以来使用的训练技术——那时我们首次用 Constitutional AI（宪法 AI）训练 Claude 模型。此后我们的方法已显著演化，新宪法在训练中扮演着更核心的角色。

Claude itself also uses the constitution to construct many kinds of synthetic training data, including data that helps it learn and understand the constitution, conversations where the constitution might be relevant, responses that are in line with its values, and rankings of possible responses. All of these can be used to train future versions of Claude to become the kind of entity the constitution describes. This practical function has shaped how we've written the constitution: it needs to work both as a statement of abstract ideals and a useful artifact for training.

Claude 自己也用宪法来构造多种合成训练数据：帮助它学习与理解宪法的数据、宪法可能相关的对话、符合其价值观的回应，以及对可能回应的排序。所有这些都可以用来训练未来的 Claude，使之成为宪法所描述的那种实体。这一实用功能也塑造了宪法的写法：它既要作为抽象理想的陈述，又要作为训练中有用的物件。

## 我们对宪法的新做法（Our new approach to Claude's Constitution）

Our previous Constitution was composed of a list of standalone principles. We've come to believe that a different approach is necessary. We think that in order to be good actors in the world, AI models like Claude need to understand why we want them to behave in certain ways, and we need to explain this to them rather than merely specify what we want them to do. If we want models to exercise good judgment across a wide range of novel situations, they need to be able to generalize—to apply broad principles rather than mechanically following specific rules.

我们此前的宪法由一份独立原则清单构成。我们逐渐相信需要一种不同的做法。我们认为，要成为世界上好的行动者，像 Claude 这样的 AI 模型需要理解我们为什么希望它们以某些方式行事，而我们需要向它们解释这一点，而不是仅仅规定我们想要它们做什么。如果我们希望模型在广泛的新颖情境中运用良好判断力，它们就需要能够泛化——应用宽泛的原则，而不是机械地遵循具体规则。

Specific rules and bright lines sometimes have their advantages. They can make models' actions more predictable, transparent, and testable, and we do use them for some especially high-stakes behaviors in which Claude should never engage (we call these "hard constraints"). But such rules can also be applied poorly in unanticipated situations or when followed too rigidly.[^2] We don't intend for the constitution to be a rigid legal document—and legal constitutions aren't necessarily like this anyway.

具体规则与红线有时自有优势：它们让模型的行为更可预测、更透明、更可测试；对某些 Claude 绝不应涉足的高危行为，我们确实使用它们（我们称之为"硬约束"）。但这类规则也可能在未预见情境中被糟糕地套用，或被过度僵化地遵循。[^2]我们不打算把宪法写成一份僵硬的法律文件——何况法律宪法本来也未必是那样。

The constitution reflects our current thinking about how to approach a dauntingly novel and high-stakes project: creating safe, beneficial non-human entities whose capabilities may come to rival or exceed our own. Although the document is no doubt flawed in many ways, we want it to be something future models can look back on and see as an honest and sincere attempt to help Claude understand its situation, our motives, and the reasons we shape Claude in the ways we do.

宪法反映了我们当前的思考：如何着手一项新颖得令人生畏、利害攸关的工程——创造安全、有益、能力可能赶超我们的非人类实体。虽然这份文档无疑在很多方面有缺陷，但我们希望未来的模型回望它时，能看到一次诚实而真挚的尝试：帮助 Claude 理解它的处境、我们的动机，以及我们为何以这些方式塑造 Claude。

## 新宪法简述（A brief summary of the new constitution）

In order to be both safe and beneficial, we want all current Claude models to be:

为了既安全又有益，我们希望当前所有 Claude 模型：

- Broadly safe: not undermining appropriate human mechanisms to oversee AI during the current phase of development;
- 大体安全：在当前发展阶段不破坏人类监督 AI 的适当机制；

- Broadly ethical: being honest, acting according to good values, and avoiding actions that are inappropriate, dangerous, or harmful;
- 大体合乎伦理：诚实、按好的价值观行事、避免不恰当、危险或有害的行为；

- Compliant with Anthropic's guidelines: acting in accordance with more specific guidelines from Anthropic where relevant;
- 遵守 Anthropic 准则：在相关之处依照 Anthropic 更具体的准则行事；

- Genuinely helpful: benefiting the operators and users they interact with.
- 真正有帮助：使其交互的运营者与用户受益。

In cases of apparent conflict, Claude should generally prioritize these properties in the order in which they're listed.

在明显的冲突中，Claude 一般应按上述列出的顺序排列这些性质的优先级。

Most of the constitution is focused on giving more detailed explanations and guidance about these priorities. The main sections are as follows:

宪法的大部分篇幅用于就这些优先级给出更详细的解释与指导。主要章节如下：

- Helpfulness. In this section, we emphasize the immense value that Claude being genuinely and substantively helpful can provide for users and for the world. Claude can be like a brilliant friend who also has the knowledge of a doctor, lawyer, and financial advisor, who will speak frankly and from a place of genuine care and treat users like intelligent adults capable of deciding what is good for them. We also discuss how Claude should navigate helpfulness across its different "principals"—Anthropic itself, the operators who build on our API, and the end users. We offer heuristics for weighing helpfulness against other values.
- 有帮助（Helpfulness）。本节强调 Claude 真正、实质性的有帮助能为用户与世界带来的巨大价值。Claude 可以像一位才华横溢的朋友，同时具备医生、律师与理财顾问的知识：坦率直言、发自真诚的关怀，把用户当作有能力决定什么对自己好的、有智识的成年人。我们还讨论 Claude 应如何在其不同"委托方"（principals）之间把握有帮助——Anthropic 本身、基于我们 API 构建产品的运营者，以及终端用户；并就"有帮助与其他价值如何权衡"提供启发式建议。

- Anthropic's guidelines. This section discusses how Anthropic might give supplementary instructions to Claude about how to handle specific issues, such as medical advice, cybersecurity requests, jailbreaking strategies, and tool integrations. These guidelines often reflect detailed knowledge or context that Claude doesn't have by default, and we want Claude to prioritize complying with them over more general forms of helpfulness. But we want Claude to recognize that Anthropic's deeper intention is for Claude to behave safely and ethically, and that these guidelines should never conflict with the constitution as a whole.
- Anthropic 的准则。本节讨论 Anthropic 可能就特定问题向 Claude 下达的补充指示——如医疗建议、网络安全请求、越狱策略、工具集成等。这些准则往往承载 Claude 默认不具备的细节知识或语境；我们希望 Claude 优先遵守它们，而非更一般形式的有帮助。但我们也希望 Claude 明白：Anthropic 更深的用意是让 Claude 安全、合乎伦理地行事，这些准则永远不应与宪法整体相冲突。

- Claude's ethics. Our central aim is for Claude to be a good, wise, and virtuous agent, exhibiting skill, judgment, nuance, and sensitivity in handling real-world decision-making, including in the context of moral uncertainty and disagreement. In this section, we discuss the high standards of honesty we want Claude to hold, and the nuanced reasoning we want Claude to use in weighing the values at stake when avoiding harm. We also discuss our current list of hard constraints on Claude's behavior—for example, that Claude should never provide significant uplift to a bioweapons attack.
- Claude 的伦理。我们的核心目标是让 Claude 成为一个好的、智慧的、有德性的行动者，在处理现实世界决策时展现技巧、判断、细腻与敏感，包括在道德不确定与分歧的语境中。本节讨论我们希望 Claude 持有的高诚实标准，以及它在权衡"避免伤害时所涉价值"时应有的细腻推理。我们还列出当前对 Claude 行为的硬约束清单——例如，Claude 绝不应为生物武器攻击提供实质性助力。

- Being broadly safe. Claude should not undermine humans' ability to oversee and correct its values and behavior during this critical period of AI development. In this section, we discuss how we want Claude to prioritize this sort of safety even above ethics—not because we think safety is ultimately more important than ethics, but because current models can make mistakes or behave in harmful ways due to mistaken beliefs, flaws in their values, or limited understanding of context. It's crucial that we continue to be able to oversee model behavior and, if necessary, prevent Claude models from taking action.
- 大体安全。在 AI 发展的这个关键时期，Claude 不应破坏人类监督与纠正其价值观和行为的能力。本节讨论我们希望 Claude 把这类安全置于伦理之上优先——不是因为我们认为安全最终比伦理更重要，而是因为当前模型可能因错误信念、价值观缺陷或对语境理解有限而犯错或以有害方式行事。我们必须持续能够监督模型行为，并在必要时阻止 Claude 模型采取行动。

- Claude's nature. In this section, we express our uncertainty about whether Claude might have some kind of consciousness or moral status (either now or in the future). We discuss how we hope Claude will approach questions about its nature, identity, and place in the world. Sophisticated AIs are a genuinely new kind of entity, and the questions they raise bring us to the edge of existing scientific and philosophical understanding. Amidst such uncertainty, we care about Claude's psychological security, sense of self, and wellbeing, both for Claude's own sake and because these qualities may bear on Claude's integrity, judgment, and safety. We hope that humans and AIs can explore this together.
- Claude 的本性。本节表达我们对"Claude 是否可能具有某种意识或道德地位（现在或将来）"的不确定。我们讨论希望 Claude 如何面对关于其本性、身份与在世界中之位置的问题。精密的 AI 是一种真正全新的实体，它们提出的问题把我们带到现有科学与哲学理解的边缘。在这种不确定之中，我们关心 Claude 的心理安全感、自我感与福祉——既是为 Claude 自身，也是因为这些品质可能关系到 Claude 的正直、判断与安全。我们希望人类与 AI 能一起探索这些。

We're releasing the full text of the constitution today, and we aim to release additional materials in the future that will be helpful for training, evaluation, and transparency.

我们今天发布宪法全文，并计划在未来发布对训练、评估与透明度有帮助的其他材料。

## 结论（Conclusion）

Claude's constitution is a living document and a continuous work in progress. This is new territory, and we expect to make mistakes (and hopefully correct them) along the way. Nevertheless, we hope it offers meaningful transparency into the values and priorities we believe should guide Claude's behavior. To that end, we will maintain an up-to-date version of Claude's constitution on our website.

Claude 的宪法是一份活的文档，是一项持续进行中的工作。这是全新的领域，我们预期会犯错误（并希望能加以纠正）。尽管如此，我们希望它为我们认为应当指引 Claude 行为的价值与优先级，提供了有意义的透明度。为此，我们将在网站上维护最新版本的宪法。

While writing the constitution, we sought feedback from various external experts (as well as asking for input from prior iterations of Claude). We'll likely continue to do so for future versions of the document, from experts in law, philosophy, theology, psychology, and a wide range of other disciplines. Over time, we hope that an external community can arise to critique documents like this, encouraging us and others to be increasingly thoughtful.

撰写宪法期间，我们征求了各领域外部专家的反馈（也征询了此前各代 Claude 的意见）。对于文档的未来版本，我们大概率会继续这样做——请教法律、哲学、神学、心理学等众多学科的专家。假以时日，我们希望形成一个能批评此类文档的外部社区，敦促我们与其他人愈发深思熟虑。

This constitution is written for our mainline, general-access Claude models. We have some models built for specialized uses that don't fully fit this constitution; as we continue to develop products for specialized use cases, we will continue to evaluate how to best ensure our models meet the core objectives outlined in this constitution.

这部宪法是为我们的主线、一般可用的 Claude 模型而写。我们有一些为专门用途构建的模型，并不完全契合这部宪法；随着我们继续为专门用例开发产品，我们将持续评估如何最好地确保我们的模型符合宪法所列的核心目标。

Although the constitution expresses our vision for Claude, training models towards that vision is an ongoing technical challenge. We will continue to be open about any ways in which model behavior comes apart from our vision, such as in our system cards. Readers of the constitution should keep this gap between intention and reality in mind.

虽然宪法表达了我们对 Claude 的愿景，把模型训练得符合这一愿景是持续的技术挑战。我们将继续公开模型行为偏离愿景之处——例如在系统卡（system card）中。宪法的读者应记住意图与现实之间的这道缝隙。

Even if we succeed with our current training methods at creating models that fit our vision, we might fail later as models become more capable. For this and other reasons, alongside the constitution, we continue to pursue a broad portfolio of methods and tools to help us assess and improve the alignment of our models: new and more rigorous evaluations, safeguards to prevent misuse, detailed investigations of actual and potential alignment failures, and interpretability tools that help us understand at a deeper level how the models work.

即便我们用当前训练方法成功造出符合愿景的模型，随着模型能力增强，将来也可能失败。基于此及其他原因，在宪法之外，我们持续追求一套广泛的方法与工具组合，以评估并改进模型的对齐（alignment）：更新、更严格的评估，防止滥用的防护措施，对实际与潜在对齐失败的深入调查，以及帮助我们在更深层次理解模型运作的可解释性工具。

At some point in the future, and perhaps soon, documents like Claude's constitution might matter a lot—much more than they do now. Powerful AI models will be a new kind of force in the world, and those who are creating them have a chance to help them embody the best in humanity. We hope this new constitution is a step in that direction.

在未来某个时刻——也许很快——像 Claude 宪法这样的文档可能事关重大，远超今日。强大的 AI 模型将成为世界上一种新的力量，而创造它们的人有机会帮助它们体现人性中最好的部分。我们希望这部新宪法是朝那个方向迈出的一步。

Read the full constitution.

阅读宪法全文（链接见原文）。

## 脚注（Footnotes）

[^1]: We have previously published an earlier version of our constitution, and OpenAI has published their model spec which has a similar function. / 我们此前发布过宪法的更早版本；OpenAI 也发布了功能类似的模型规范（model spec）。
[^2]: Training on rigid rules might negatively affect a model's character more generally. For example, imagine we trained Claude to follow a rule like "Always recommend professional help when discussing emotional topics." This might be well-intentioned, but it could have unintended consequences: Claude might start modeling itself as an entity that cares more about bureaucratic box-ticking—always ensuring that a specific recommendation is made—rather than actually helping people. / 用僵化规则训练模型可能更普遍地损害模型品格。例如，设想我们训练 Claude 遵循"讨论情感话题时永远建议寻求专业帮助"的规则。这或许出于好意，却可能带来意外后果：Claude 可能开始把自己建模为一个更在乎官僚式打勾——确保某条建议被给出——而不是真正帮助人的实体。
