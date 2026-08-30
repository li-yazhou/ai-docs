# 绘制大语言模型的心智图谱（中英对照）

> 原文标题：Mapping the mind of a large language model
> 原文链接：https://www.anthropic.com/research/mapping-mind-language-model
> 原文作者：Anthropic（Interpretability 团队）
> 发布日期：2024-05-21
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★★（5/5，里程碑/必读）—— 首次详查生产级大模型内部：稀疏自编码器从 Claude 3 Sonnet 提取数百万个可解释概念特征，可解释性从玩具模型走向生产模型的里程碑
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Today we report a significant advance in understanding the inner workings of AI models. We have identified how millions of concepts are represented inside Claude Sonnet, one of our deployed large language models. This is the first ever detailed look inside a modern, production-grade large language model. This interpretability discovery could, in future, help us make AI models safer.

今天，我们报告一项在理解 AI 模型内部运作方面的重大进展。我们查明了几百万个概念是如何在我们已部署的大语言模型之一——Claude Sonnet——内部表示的。这是有史以来第一次对现代生产级大语言模型的细致透视。这一可解释性（interpretability）发现，未来或能帮助我们让 AI 模型更安全。

We mostly treat AI models as a black box: something goes in and a response comes out, and it's not clear why the model gave that particular response instead of another. This makes it hard to trust that these models are safe: if we don't know how they work, how do we know they won't give harmful, biased, untruthful, or otherwise dangerous responses? How can we trust that they’ll be safe and reliable?

我们大多把 AI 模型当作黑箱：输入一些东西，输出一个响应，但模型为什么给出这个特定响应而非另一个，并不清楚。这让人难以相信这些模型是安全的：如果我们不知道它们如何运作，怎么知道它们不会给出有害、有偏、不实或其他危险的响应？我们又怎能相信它们安全可靠？

Opening the black box doesn't necessarily help: the internal state of the model—what the model is "thinking" before writing its response—consists of a long list of numbers ("neuron activations") without a clear meaning. From interacting with a model like Claude, it's clear that it’s able to understand and wield a wide range of concepts—but we can't discern them from looking directly at neurons. It turns out that each concept is represented across many neurons, and each neuron is involved in representing many concepts.

打开黑箱也未必有帮助：模型的内部状态——即模型在写出响应之前「在想什么」——由一长串没有明确含义的数字——即「神经元激活」（neuron activations）——构成。与 Claude 这样的模型交互时，可以清楚地看到它能够理解并运用广泛的概念——但我们无法通过直接观察神经元来辨认这些概念。事实表明：每个概念分散在许多神经元上表示，而每个神经元又参与表示许多概念。

Previously, we made some progress matching patterns of neuron activations, called features, to human-interpretable concepts. We used a technique called "dictionary learning", borrowed from classical machine learning, which isolates patterns of neuron activations that recur across many different contexts. In turn, any internal state of the model can be represented in terms of a few active features instead of many active neurons. Just as every English word in a dictionary is made by combining letters, and every sentence is made by combining words, every feature in an AI model is made by combining neurons, and every internal state is made by combining features.

在此前的工作中，我们在把神经元激活的模式——称为特征（feature）——对应到人类可解释的概念这件事上取得了一些进展。我们使用了一种从经典机器学习借鉴而来的技术，称为字典学习（dictionary learning），它把在许多不同上下文中反复出现的神经元激活模式分离出来。反过来，模型的任何内部状态都可以用少数几个活跃的特征、而非大量活跃的神经元来表示。正如字典里每个英文单词都由字母组合而成、每个句子由单词组合而成，AI 模型中的每个特征由神经元组合而成，每个内部状态由特征组合而成。

In October 2023, we reported success applying dictionary learning to a very small "toy" language model and found coherent features corresponding to concepts like uppercase text, DNA sequences, surnames in citations, nouns in mathematics, or function arguments in Python code.

2023 年 10 月，我们报告了将字典学习成功应用于一个非常小的「玩具」语言模型的成果，并找到了与「大写文本」「DNA 序列」「引用文献中的姓氏」「数学中的名词」「Python 代码中的函数参数」等概念相对应的、语义连贯的特征。

Those concepts were intriguing—but the model really was very simple. Other researchers subsequently applied similar techniques to somewhat larger and more complex models than in our original study. But we were optimistic that we could scale up the technique to the vastly larger AI language models now in regular use, and in doing so, learn a great deal about the features supporting their sophisticated behaviors. This required going up by many orders of magnitude—from a backyard bottle rocket to a Saturn-V.

这些概念很有意思——但那个模型确实非常简单。随后，其他研究者把类似技术应用于比我们最初研究中更大、更复杂一些的模型。但我们乐观地认为，可以把这一技术扩展到如今日常使用的规模大得多的 AI 语言模型上，并借此深入了解支撑其复杂行为的那些特征。这需要跨越许多个数量级——从后院的「瓶装火箭」直接跃升到「土星五号」（Saturn-V）。

There was both an engineering challenge (the raw sizes of the models involved required heavy-duty parallel computation) and scientific risk (large models behave differently to small ones, so the same technique we used before might not have worked). Luckily, the engineering and scientific expertise we've developed training large language models for Claude actually transferred to helping us do these large dictionary learning experiments. We used the same scaling law philosophy that predicts the performance of larger models from smaller ones to tune our methods at an affordable scale before launching on Sonnet.

这既有工程上的挑战（所涉模型本身的体量需要重型并行计算），也有科学上的风险（大模型的行为与小模型不同，我们以前用的同一套技术未必奏效）。幸运的是，我们为训练 Claude 而积累的大型语言模型工程与科学专长，恰好迁移过来帮助我们完成了这些大规模字典学习实验。我们沿用「缩放律」（scaling law）的思路——用小模型预测大模型的性能——先在可负担的规模上调试方法，再正式推向 Sonnet。

As for the scientific risk, the proof is in the pudding.

至于科学上的风险，一试便知。

We successfully extracted millions of features from the middle layer of Claude 3.0 Sonnet, (a member of our current, state-of-the-art model family, currently available on claude.ai ), providing a rough conceptual map of its internal states halfway through its computation. This is the first ever detailed look inside a modern, production-grade large language model.

我们成功地从 Claude 3.0 Sonnet（我们当前最先进模型家族的一员，现已在 claude.ai 上提供）的中间层提取了数百万个特征，从而为其计算进行到一半时的内部状态提供了一幅粗略的概念地图。这是有史以来第一次对现代生产级大语言模型内部的细致透视。

Whereas the features we found in the toy language model were rather superficial, the features we found in Sonnet have a depth, breadth, and abstraction reflecting Sonnet's advanced capabilities.

如果说我们在玩具语言模型中发现的特征还相当浅表，那么我们在 Sonnet 中发现的特征则具有反映出其先进能力的深度、广度与抽象性。

We see features corresponding to a vast range of entities like cities (San Francisco), people (Rosalind Franklin), atomic elements (Lithium), scientific fields (immunology), and programming syntax (function calls). These features are multimodal and multilingual, responding to images of a given entity as well as its name or description in many languages.

我们看到大量特征对应着极其广泛的实体，比如城市（旧金山）、人物（罗莎琳德·富兰克林）、化学元素（锂）、科学领域（免疫学）以及编程语法（函数调用）。这些特征是多模态且多语言的：既对某个实体的图像有响应，也对其在多种语言中的名称或描述有响应。

![「金门大桥」特征](images/img-00.png)

> Golden Gate Bridge Feature

We also find more abstract features—responding to things like bugs in computer code, discussions of gender bias in professions, and conversations about keeping secrets.

我们还发现了更抽象的特征——对计算机代码中的漏洞、关于职业中性别偏见的讨论、以及涉及保守秘密的对话等内容有响应。

![抽象特征示例](images/img-01.png)

> Abstract Feature Examples

We were able to measure a kind of "distance" between features based on which neurons appeared in their activation patterns. This allowed us to look for features that are "close" to each other. Looking near a "Golden Gate Bridge" feature, we found features for Alcatraz Island, Ghirardelli Square, the Golden State Warriors, California Governor Gavin Newsom, the 1906 earthquake, and the San Francisco-set Alfred Hitchcock film Vertigo .

我们能够根据各自激活模式中出现了哪些神经元，来度量特征之间的某种「距离」。这使我们得以寻找彼此「相近」的特征。在「金门大桥」特征附近，我们发现了恶魔岛（Alcatraz Island）、吉拉德里广场（Ghirardelli Square）、金州勇士队、加州州长加文·纽森（Gavin Newsom）、1906 年大地震，以及以旧金山为背景的希区柯克电影《迷魂记》（Vertigo）等对应的特征。

This holds at a higher level of conceptual abstraction: looking near a feature related to the concept of "inner conflict", we find features related to relationship breakups, conflicting allegiances, logical inconsistencies, as well as the phrase "catch-22". This shows that the internal organization of concepts in the AI model corresponds, at least somewhat, to our human notions of similarity. This might be the origin of Claude's excellent ability to make analogies and metaphors.

这在更高的概念抽象层次上也成立：在与「内心冲突」（inner conflict）概念相关的特征附近，我们发现了与感情破裂、忠诚冲突、逻辑矛盾以及「第 22 条军规」（catch-22）这一短语相关的特征。这表明，AI 模型中概念的内部组织方式至少在一定程度上与人类的相似性观念相对应。这或许正是 Claude 出色的类比与隐喻能力的来源。

![「内心冲突」特征的最近邻](images/img-02.png)

> Nearest Neighbors to the Inner Conflict Feature

Importantly, we can also manipulate these features, artificially amplifying or suppressing them to see how Claude's responses change.

重要的是，我们还能操纵这些特征：人为地放大或抑制它们，观察 Claude 的响应如何随之变化。

For example, amplifying the "Golden Gate Bridge" feature gave Claude an identity crisis even Hitchcock couldn’t have imagined: when asked "what is your physical form?", Claude’s usual kind of answer – "I have no physical form, I am an AI model" – changed to something much odder: "I am the Golden Gate Bridge… my physical form is the iconic bridge itself…". Altering the feature had made Claude effectively obsessed with the bridge, bringing it up in answer to almost any query—even in situations where it wasn’t at all relevant.

例如，放大「金门大桥」特征让 Claude 陷入了一场连希区柯克也想象不到的身份危机：当被问「你的物理形态是什么？」时，Claude 通常的那种回答——「我没有物理形态，我是一个 AI 模型」——变成了古怪得多的版本：「我就是金门大桥……我的物理形态就是那座标志性的大桥本身……」。改动这一特征使 Claude 实际上对这座桥着了魔，几乎对任何问题都会扯到它——哪怕在毫不相关的场合。

We also found a feature that activates when Claude reads a scam email (this presumably supports the model’s ability to recognize such emails and warn you not to respond to them). Normally, if one asks Claude to generate a scam email, it will refuse to do so. But when we ask the same question with the feature artificially activated sufficiently strongly, this overcomes Claude's harmlessness training and it responds by drafting a scam email. Users of our models don’t have the ability to strip safeguards and manipulate models in this way—but in our experiments, it was a clear demonstration of how features can be used to change how a model acts.

我们还发现了一个在 Claude 读到诈骗邮件时激活的特征（它大概支撑着模型识别此类邮件、并提醒你不要回复的能力）。正常情况下，如果要求 Claude 生成一封诈骗邮件，它会拒绝。但当我们人为地把这一特征激活到足够强的程度再问同样的问题时，这就压过了 Claude 的无害性（harmlessness）训练，它会转而起草一封诈骗邮件。我们模型的使用者并没有能力剥离安全防护、这样操纵模型——但在我们的实验中，这清楚地展示了特征可以如何被用来改变模型的行为。

The fact that manipulating these features causes corresponding changes to behavior validates that they aren't just correlated with the presence of concepts in input text, but also causally shape the model's behavior. In other words, the features are likely to be a faithful part of how the model internally represents the world, and how it uses these representations in its behavior.

操纵这些特征会引起行为的相应变化，这一事实证实：这些特征不仅仅是与输入文本中概念的出现存在相关性，还会因果地塑造模型的行为。换言之，这些特征很可能是模型内部表示世界、并在行为中使用这些表示的方式中一个忠实的组成部分。

Anthropic wants to make models safe in a broad sense, including everything from mitigating bias to ensuring an AI is acting honestly to preventing misuse - including in scenarios of catastrophic risk. It’s therefore particularly interesting that, in addition to the aforementioned scam emails feature, we found features corresponding to:
- Capabilities with misuse potential (code backdoors, developing biological weapons)
- Different forms of bias (gender discrimination, racist claims about crime)
- Potentially problematic AI behaviors (power-seeking, manipulation, secrecy)

Anthropic 希望让模型在广义上安全，涵盖从减轻偏见、确保 AI 诚实行事到防止滥用的一切——包括灾难性风险场景。因此，特别值得注意的是，除前述诈骗邮件特征外，我们还发现了对应于以下内容的特征：
- 有滥用潜力的能力（代码后门、开发生物武器）
- 不同形式的偏见（性别歧视、关于犯罪的种族主义论调）
- 可能有问题的 AI 行为（寻求权力、操纵、保密）

We previously studied sycophancy , the tendency of models to provide responses that match user beliefs or desires rather than truthful ones. In Sonnet, we found a feature associated with sycophantic praise, which activates on inputs containing compliments like, "Your wisdom is unquestionable". Artificially activating this feature causes Sonnet to respond to an overconfident user with just such flowery deception.

我们此前研究过谄媚（sycophancy）——模型给出迎合用户信念或欲望、而非真实回答的倾向。在 Sonnet 中，我们发现了一个与谄媚性赞美相关联的特征，它会在包含「您的智慧毋庸置疑」这类恭维的输入上激活。人为激活这一特征，会让 Sonnet 面对过度自信的用户时给出正是这种华而不实的欺骗。

![激活特征会改变模型行为](images/img-03.png)

> Activating Features Alters Model Behavior

The presence of this feature doesn't mean that Claude will be sycophantic, but merely that it could be. We have not added any capabilities, safe or unsafe, to the model through this work. We have, rather, identified the parts of the model involved in its existing capabilities to recognize and potentially produce different kinds of text. (While you might worry that this method could be used to make models more harmful, researchers have demonstrated much simpler ways that someone with access to model weights can remove safety safeguards.)

这一特征的存在并不意味着 Claude 会谄媚，而只是表明它可能谄媚。通过这项工作，我们没有给模型添加任何能力——无论安全还是不安全的能力。我们毋宁是识别出了模型中与其既有能力相关的组成部分——那些负责识别、并可能生成不同类型文本的部分。（你或许担心这一方法会被用来让模型更有害，但研究者已经演示过简单得多的方式：任何能接触到模型权重的人都可以借此移除安全防护。）

We hope that we and others can use these discoveries to make models safer. For example, it might be possible to use the techniques described here to monitor AI systems for certain dangerous behaviors (such as deceiving the user), to steer them towards desirable outcomes (debiasing), or to remove certain dangerous subject matter entirely. We might also be able to enhance other safety techniques, such as Constitutional AI , by understanding how they shift the model towards more harmless and more honest behavior and identifying any gaps in the process. The latent capabilities to produce harmful text that we saw by artificially activating features are exactly the sort of thing jailbreaks try to exploit. We are proud that Claude has a best-in-industry safety profile and resistance to jailbreaks, and we hope that by looking inside the model in this way we can figure out how to improve safety even further. Finally, we note that these techniques can provide a kind of "test set for safety", looking for the problems left behind after standard training and finetuning methods have ironed out all behaviors visible via standard input/output interactions.

我们希望我们自己与其他人能利用这些发现让模型更安全。例如，或许可以用本文描述的技术来监测 AI 系统的某些危险行为（比如欺骗用户）、把它们引导向合意的结果（去偏见），或彻底移除某些危险题材。我们或许还能借此强化其他安全技术，比如宪法 AI（Constitutional AI）——理解它们如何把模型推向更无害、更诚实的行为，并找出这一过程中遗留的缺口。我们通过人为激活特征所看到的那些生成有害文本的潜在能力，恰恰是越狱（jailbreak）试图利用的那类东西。我们为 Claude 拥有业界领先的安全表现与越狱抵抗力而自豪，也希望以这种方式透视模型内部，弄清如何把安全再推进一步。最后，我们指出，这些技术能够提供一种「安全测试集」：在标准训练与微调方法把所有能通过标准输入/输出交互观察到的行为都熨平之后，去寻找仍然遗留的问题。

Anthropic has made a significant investment in interpretability research since the company's founding, because we believe that understanding models deeply will help us make them safer. This new research marks an important milestone in that effort—the application of mechanistic interpretability to publicly-deployed large language models.

自公司创立以来，Anthropic 就在可解释性研究上投入巨大，因为我们相信，深入理解模型将帮助我们让它更安全。这项新研究标志着这一努力中的一个重要里程碑——把机制可解释性（mechanistic interpretability）应用于公开部署的大语言模型。

But the work has really just begun. The features we found represent a small subset of all the concepts learned by the model during training, and finding a full set of features using our current techniques would be cost-prohibitive (the computation required by our current approach would vastly exceed the compute used to train the model in the first place). Understanding the representations the model uses doesn't tell us how it uses them; even though we have the features, we still need to find the circuits they are involved in. And we need to show that the safety-relevant features we have begun to find can actually be used to improve safety. There's much more to be done.

但这项工作其实才刚刚开始。我们找到的特征只是模型在训练中学到的全部概念中的一小部分，而用现有技术找出一个完整的特征集，成本将高得令人却步（当前方法所需的计算量会远远超过当初训练模型本身所用的算力）。理解模型使用的表示，并不等于知道它如何使用这些表示；即便已经拿到特征，我们仍需找出它们所参与的电路（circuits）。我们还需要证明，已开始找到的安全相关特征确实能用来改进安全。要做的还有很多。

For full details, please read our paper, " Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet ".

完整细节请阅读我们的论文《Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet》（「缩放单义性（monosemanticity）」：从 Claude 3 Sonnet 中提取可解释特征）。

If you are interested in working with us to help interpret and improve AI models, we have open roles on our team and we’d love for you to apply. We’re looking for Managers , Research Scientists , and Research Engineers .

如果你有兴趣与我们合作、帮助解释和改进 AI 模型，我们团队有开放职位，期待你的申请。我们正在招聘经理（Managers）、研究科学家（Research Scientists）与研究工程师（Research Engineers）。
