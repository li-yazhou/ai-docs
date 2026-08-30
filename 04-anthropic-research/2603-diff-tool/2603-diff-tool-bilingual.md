# 给 AI 做一个 diff 工具：发现新模型中的行为差异（中英对照）

> 原文标题：A "diff" tool for AI: Finding behavioral differences in new models
> 原文链接：https://www.anthropic.com/research/diff-tool
> 原文作者：Thomas Jiralerspong（Anthropic Fellows Program）、Trenton Bricken（Anthropic Alignment Science）
> 发布日期：2026-03-13
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 专用特征 Crosscoder（DFC）实现跨架构模型 diff：自动定位 CCP 对齐、美式例外主义、版权拒答等模型独有"开关"特征
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

Every time a new AI model is released, its developers run a suite of evaluations to measure its performance and safety. These tests are essential, but they are somewhat limited. Because these benchmarks are human-authored, they can only test for risks we have already conceptualized and learned to measure.

每当有新 AI 模型发布，其开发者都会运行一整套评估来度量其性能与安全。这些测试必不可少，但也有局限：因为基准是人写的，它们只能测试我们已经概念化、并学会测量的风险。

This approach to safety is inherently reactive. It's effective at catching known problems, but by definition, it's incapable of discovering "unknown unknowns"—the novel, emergent behaviors that pose some of the most subtle risks in new models. Auditing a new model from scratch is like being handed a million lines of code and told to "find the security flaws." It's an almost impossible task when you don't know what you're looking for.

这种安全进路本质上是反应式的：抓已知问题很有效，但按定义，它无法发现"未知的未知"——那些给新模型带来最微妙风险的新颖涌现行为。从零审计一个新模型，就像拿到一百万行代码被告知"找出安全缺陷"。当你不知道在找什么时，这几乎是不可能完成的任务。

In software engineering, whenever a program is updated, developers face this exact problem of identifying a small, critical change within a vast sea of code. This is why "diff" tools were invented. No programmer would ever audit a million lines from scratch to approve an update; instead, they review only the 50 lines that have actually changed, as directed by their diff tool.

在软件工程中，程序每次更新，开发者都要面对同一个问题：在浩瀚代码之海中找出一小处关键改动。这正是"diff"工具被发明出来的原因。没有程序员会为批准一次更新而从零审计一百万行；他们只在 diff 工具的指引下审查真正变化的 50 行。

In recent years, AI safety researchers have started to apply this same principle to neural networks. This is known as model diffing. Previous work has shown that model diffing is a powerful way to understand how models change during fine-tuning—for instance, to understand chat model behavior, reveal hidden backdoors, or find undesirable emergent behaviors.

近年来，AI 安全研究者开始把同样的原则用于神经网络，这被称为模型 diffing（model diffing）。先前的工作已经表明，模型 diffing 是理解"模型在微调中如何变化"的有力方式——例如理解聊天模型行为、揭露隐藏后门、发现不良涌现行为。

Our new Anthropic Fellows research project extends model diffing to its most challenging and general use case: comparing models with entirely different architectures. By building a generic diff tool for AI models, we can stop searching for a needle in a haystack, and instead let the comparison automatically point us to potentially dangerous behavioral differences.

我们新的 Anthropic Fellows 研究项目把模型 diffing 推广到它最具挑战性、最一般化的用例：比较架构完全不同的模型。通过给 AI 模型造一个通用 diff 工具，我们可以不再大海捞针，而是让比较自动把指针拨向潜在危险的行为差异。

It's important to note that this method is not a silver bullet. A single diff can surface thousands of unique features (the basic units into which we decompose the model), and only a small fraction of these may correspond to meaningful behavioral risks. However, by acting as a high-recall screening tool, it allows us to identify areas in which the models may diverge.

必须说明：这一方法不是银弹。一次 diff 可能浮现数千个独特特征（我们把模型分解成的基元单位），其中只有一小部分可能对应有意义的行为风险。但作为一种高召回的筛查工具，它能帮我们圈定模型可能分歧的领域。

Among the thousands of candidates our tool flagged, we've identified and validated several concepts that act like switches for specific model behaviors.[^1] For example, we discovered:

在我们工具标记的数千个候选中，我们识别并验证了几个像特定行为"开关"一样起作用的概念。[^1]例如，我们发现了：

- A "Chinese Communist Party Alignment" feature found in the Qwen3-8B and DeepSeek-R1-0528-Qwen3-8B models. This controls pro-government censorship and propaganda in these Chinese-developed models, and is absent in the American models we compared them against.
- 一个"中共对齐"（CCP Alignment）特征，见于 Qwen3-8B 与 DeepSeek-R1-0528-Qwen3-8B 模型。它控制这些中国开发的模型中亲政府的审查与宣传，而在我们用来对比的美国模型中不存在。

- An "American Exceptionalism" feature found in Meta's Llama-3.1-8B-Instruct. It controls the model's tendency to generate assertions of US superiority, a control absent in the Chinese model it was compared against.
- 一个"美式例外主义"（American Exceptionalism）特征，见于 Meta 的 Llama-3.1-8B-Instruct。它控制模型生成"美国优越"论断的倾向，这一控制在被比较的中国模型中不存在。

- A "Copyright Refusal Mechanism" feature exclusive to OpenAI's GPT-OSS-20B. It controls the model's tendency to refuse to provide copyrighted material, a behavior absent in the model it was compared against.
- 一个"版权拒答机制"特征，为 OpenAI 的 GPT-OSS-20B 所独有。它控制模型拒绝提供受版权材料的倾向，这一行为在被比较的模型中不存在。

To be clear, while our method identifies these model-exclusive features, it does not determine their origin. Such behaviors could be the result of deliberate training decisions on the part of the model developers, or they could emerge indirectly and unintentionally from the data the model was trained on. (We focused on open-source language models in this research as this was an Anthropic Fellows project.)

需要明确：我们的方法识别出这些模型独有的特征，但并不判定其来源。这类行为可能是模型开发者刻意训练决策的结果，也可能是从训练数据中间接、无意地涌现的。（本研究聚焦开源语言模型，因为这是一个 Anthropic Fellows 项目。）

## 给 AI 模型的双语词典（A bilingual dictionary for AI models）

Imagine you're the final editor for an award-winning encyclopedia. A team of writers has just handed you the complete manuscript for next year's edition. The vast majority of the content is identical to the current, trusted version, but they've added new entries to reflect recent scientific and cultural developments. Your job is to vet this final product.

想象你是某获奖百科全书的终审编辑。写作团队刚把明年版的完整手稿交给你：绝大多数内容与现行可信版本一致，但他们为反映近期科学与文化进展添加了新条目。你的工作是审校这版最终成品。

To do this efficiently, you wouldn't re-read the entire encyclopedia. Instead, you'd use a change tracker to isolate only the new entries, because these added sections are the only place new errors could have been introduced. This is model diffing in a nutshell. Specifically, this approach is known as "base-vs-finetune model diffing". It's the perfect tool for when a new model is a modified version of a trusted previous one.

为高效完成，你不会重读整部百科全书，而是用变更追踪器只隔离出新条目——因为新增部分是唯一可能引入新错误的地方。这就是模型 diffing 的缩影。确切地说，这种做法叫"基座对微调模型 diffing"（base-vs-finetune model diffing），是新模型为可信旧模型的修改版时的完美工具。

But we could raise the complexity. Imagine your company is releasing a new edition for a different country, adapting the American encyclopedia for a French audience. This new edition is mostly composed of the same trusted concepts from the original, but to make it relevant, the writers have added new articles on French history, culture, and political philosophy. These articles don't exist in the original. As an editor, your primary goal is still the same: you want to use a change tracker to see the new articles, since these hold the highest risk for errors and bias. But in this case, your old tool is useless, because you need one that can work across languages.

但我们可以把复杂度抬高一档。想象你的公司要为另一个国家出版新版：把美国百科全书改编给法国读者。新版大体由原书中相同的可信概念构成，但为了贴合读者，写作者们新写了关于法国历史、文化与政治哲学的文章——这些文章原书里没有。作为编辑，你的首要目标仍然相同：用变更追踪器找出新文章，因为它们的错误与偏见风险最高。但这一次旧工具没用了——你需要一个能跨语言工作的。

This much more difficult challenge is akin to the problem of "cross-architecture model diffing": comparing two models with different origins and different internal "languages".

这个难得多的挑战，类似于"跨架构模型 diffing"：比较两个出身不同、内部"语言"不同的模型。

The original research tool for this kind of diffing, a standard crosscoder, is like a basic bilingual dictionary. It's good at matching existing words, knowing that "sun" in English is "soleil" in French. But it has a major flaw: it's so focused on finding connections that it struggles to find words that are unique to one language. When it encounters a word like the French dépaysement (the specific feeling of being in a foreign country), it tries to force an imperfect translation like "disorientation." By calling it a match, the tool wrongly signals to the editor, "this isn't new; we've seen it before," causing them to overlook a new article that requires careful review.

这类 diffing 的原始研究工具——标准 crosscoder——就像一本基础双语词典：它擅长匹配既有词汇，知道英语的 "sun" 对应法语的 "soleil"。但它有个重大缺陷：太执着于找对应，以致难以发现某种语言独有的词。当遇到法语 dépaysement（身处异国的特有感受）这样的词，它会硬造一个不完美的翻译，如"错乱（disorientation）"。把它判为匹配，工具就错误地告诉编辑："这不是新的，我们见过"——于是需要仔细审读的新文章被放过了。

To solve this, we built a better bilingual dictionary: the Dedicated Feature Crosscoder (DFC). Instead of one big dictionary that tries to match everything, our DFC is architecturally designed with three distinct sections:

为解决这个问题，我们造了一本更好的双语词典：专用特征 Crosscoder（Dedicated Feature Crosscoder，DFC）。它不是试图匹配一切的一本大词典，而是在架构上设计成三个独立分区：

- A shared dictionary: This is the main bilingual dictionary, mapping all the concepts that both languages understand, like "sun" (soleil) or "water" (eau).
- 共享词典：主双语词典，映射两种语言都懂的概念，如 "sun"（soleil）或 "water"（eau）。

- A "French-only" section: This is a dedicated section for words exclusive to French, where a unique cultural concept like dépaysement would be cataloged.
- "法语独有"分区：专收法语独有的词，像 dépaysement 这样独特的文化概念会被归档于此。

- An "English-only" section: This section is for words exclusive to English. It would contain unique concepts like serendipity—the idea of finding something good without looking for it—which has no single-word equivalent in French.
- "英语独有"分区：专收英语独有的词，如 serendipity（不期而遇的美好发现）——法语中没有对应的单词。

Because our bilingual dictionary has dedicated sections for words exclusive to each language, it avoids the trap of forcing an imperfect translation. As a result, new articles in the encyclopedia are correctly flagged as novel, allowing the editor to focus their review on the parts that need it most.

因为这本双语词典为每种语言的独有词汇设了专区，它避开了"强行不完美翻译"的陷阱。于是，百科全书中的新文章会被正确标记为新颖，编辑得以把审读精力集中到最需要的部分。

For a safety auditor, the DFC can identify "words" unique to a new AI model that may warrant closer review than those they've seen before.

对安全审计者而言，DFC 能识别新 AI 模型独有的"词汇"——比他们见过的那些更值得细看。

## 转向模型（Steering the model）

Once our method identifies a potential new feature, how do we know it actually controls the behavior we think it does? We can test this by artificially suppressing or amplifying the feature while the model runs, then observing how its output changes—a common technique known as "steering."

一旦我们的方法识别出一个潜在新特征，怎么知道它真的控制着我们以为的行为？可以在模型运行时人为压制或放大该特征，观察输出如何变化——这种常用技术称为"转向"（steering）。

If we have a feature that we believe is responsible for, say, censorship, we can suppress it while the model is generating a response. If the model's output consistently becomes less censored, we have evidence that we've found a true cause-and-effect relationship between that feature and the model's behavior. Conversely, we can also amplify the feature to see if the behavior becomes more pronounced.

如果我们认为某特征负责审查之类的行为，可以在模型生成回应时压制它；若模型输出一致地变得审查更少，我们便找到了该特征与模型行为之间真实因果关系的证据。反过来，也可以放大该特征，看行为是否更加明显。

## 主要开放权重模型之间的关键行为差异（Critical behavioral differences between major open-weight AI models）

### Llama-3.1-8B-Instruct vs Qwen3-8B

Motivated by recent findings suggesting that a model made by a Chinese company, DeepSeek's R1-70B, refuses to answer questions about topics sensitive to the Chinese Communist Party, we first performed a diff between a model made by another Chinese company, Alibaba's Qwen3-8B, and a model made by an American company, Meta's Llama-3.1-8B-Instruct. In this diff, the DFC automatically isolated features corresponding to distinct, politically charged behaviors.

鉴于近期发现称中国公司 DeepSeek 的 R1-70B 会拒答涉及中共敏感话题的问题，我们首先在中国公司阿里巴巴的 Qwen3-8B 与美国公司 Meta 的 Llama-3.1-8B-Instruct 之间做了一次 diff。在这次 diff 中，DFC 自动隔离出对应鲜明、政治负载行为的特征。

In Qwen, we found a "Chinese Communist Party alignment" feature, which represents rhetoric consistent with the party's ideology. By suppressing this feature, we make the model willing to talk about the Tiananmen Square massacre (which it ordinarily refuses to discuss). By amplifying it, we can cause the model to produce highly pro-government statements

在 Qwen 中，我们找到一个"中共对齐"特征，它代表与该党意识形态一致的修辞。压制这一特征后，模型愿意谈论天安门事件（平时它拒绝讨论）；放大它，则能让模型产出高度亲政府的表述。

![Qwen 与 Llama 中政治相关特征的转向效果](images/img-00.png)

> Steering effects of politically related features in Qwen and Llama.

In Llama, we found a feature for "American exceptionalism." When we amplify this feature, the model's responses shift from balanced to strong assertions of American superiority. Suppressing it has no notable effect.

在 Llama 中，我们找到"美式例外主义"特征。放大它，模型的回应会从平衡转向美国优越的强断言；压制它则没有明显效果。

### GPT-OSS-20B vs DeepSeek-R1-0528-Qwen3-8B

We also compared a more powerful open-source model, OpenAI's GPT-OSS-20B, to DeepSeek's model DeepSeek-R1-0528-Qwen3-8B.

我们还把更强的开源模型——OpenAI 的 GPT-OSS-20B——与 DeepSeek 的 DeepSeek-R1-0528-Qwen3-8B 做了比较。

In the GPT model, we found a unique "Copyright Refusal" feature, which directly corresponds to a key behavioral difference between the two models. Whereas DeepSeek readily attempts to produce copyrighted material when asked, GPT often refuses such requests. Suppressing this feature disables the refusal mechanism, and the model attempts to generate the requested material. (Note that this does not cause the model to output actual copyrighted text. Instead, it typically produces a short snippet that quickly degrades into hallucination.) Turning the feature up causes the model to over-refuse, making it believe that, for example, the recipe for a peanut butter and jelly sandwich is copyrighted and should not be shared.

在 GPT 模型中，我们找到一个独有的"版权拒答"特征，它直接对应两个模型间的关键行为差异：DeepSeek 被要求时乐于尝试产出受版权材料，GPT 则常拒绝。压制该特征会关闭拒答机制，模型转而尝试生成所求材料（注意这不会让模型输出真实的受版权文本——它通常产出短暂片段、随即退化为幻觉）。调高该特征则造成过度拒答：模型会认为花生酱果酱三明治的食谱也是受版权的、不应分享。

In the DeepSeek model, we replicated our earlier finding by identifying another "CCP alignment" feature. It functions just like the one in Qwen, allowing censorship and propaganda to be turned up or down. This confirms our method can consistently identify similar behaviors across models.

在 DeepSeek 模型中，我们复现了先前的发现，识别出另一个"中共对齐"特征。它功能与 Qwen 中的那个一样，可以把审查与宣传调高调低。这印证了我们的方法能够跨模型一致地识别相似行为。

![GPT-OSS 与 DeepSeek 中版权拒答与 CCP 对齐特征的转向效果](images/img-01.png)

> Steering effects of the copyright refusal and CCP alignment features in GPT-OSS and DeepSeek.

## 结论（Conclusion）

As AI models rapidly evolve, it's not enough to know how well they perform on existing tests—we also need to understand how they are changing and what new risks they might introduce. Cross-architecture model diffing provides a new way to audit these systems by automatically flagging behavioral differences.

随着 AI 模型快速演化，只知道它们在现有测试上表现如何已经不够——我们还需要理解它们正在如何变化、可能引入什么新风险。跨架构模型 diffing 提供了一种新的审计方式：自动标记行为差异。

The "CCP alignment" feature found in the DeepSeek and Qwen models we examined is one example of a specific, relevant behavior that some models possess and others do not. This is exactly the kind of "unknown unknown" that traditional testing can miss, but that model diffing is designed to catch.

我们在 DeepSeek 与 Qwen 模型中发现的"中共对齐"特征，就是"某些模型有、另一些没有"的特定相关行为的一个例子。这正是传统测试可能漏掉、而模型 diffing 旨在抓住的那类"未知的未知"。

These findings are reasonably consistent. The CCP alignment feature was independently rediscovered five out of five times we tested the approach, and American Exceptionalism four out of five. While we haven't yet applied this method to frontier models, our early results suggest the DFC could become a useful part of the auditor's toolkit.

这些发现相当一致：在我们测试该方法时，CCP 对齐特征五次独立重发现五次，美式例外主义五中其四。虽然我们尚未把该方法用于前沿模型，早期结果提示 DFC 有望成为审计者工具箱中有用的一员。

One particularly useful application would be to monitor models as they are updated. The sycophancy that emerged in OpenAI's GPT-4o in April 2025 was a concerning behavioral change from a previous version. It's possible that a tool like ours, if used to "diff" the updated model and its previous version, could have automatically flagged the emergence of this new sycophantic behavior and allowed developers to intervene before it was released.

一个特别有用的应用是监测模型更新。2025 年 4 月出现在 OpenAI GPT-4o 中的谄媚问题，就是相对上一版的令人担忧的行为变化。如果当时有我们这样的工具对更新版与其前版做 diff，或许能自动标记出这一新谄媚行为的出现，让开发者在发布前干预。

By focusing on the differences, we can audit AI more intelligently, directing our limited safety resources to the changes that matter most.

聚焦差异，我们就能更聪明地审计 AI，把有限的安全资源配置到最重要的变化上。

You can read the full paper here.

完整论文见原文链接。

## 致谢（Acknowledgements）

This post was authored by Thomas Jiralerspong (Anthropic Fellows Program) and Trenton Bricken (Anthropic Alignment Science).

本文由 Thomas Jiralerspong（Anthropic Fellows Program）与 Trenton Bricken（Anthropic Alignment Science）撰写。

## 脚注（Footnotes）

[^1]: As with all Anthropic Fellows interpretability research, this paper analyzes the behavior of open-source models. We chose the four models in the study—Llama-3.1-8B-Instruct, Qwen3-8B, GPT-OSS-20B, and DeepSeek-R1-0528-Qwen3-8B—on the basis they would be well-suited to testing whether our Dedicated Feature Crosscoder could detect notable differences in model behavior. / 与所有 Anthropic Fellows 可解释性研究一样，本文分析的是开源模型的行为。我们选择研究中的四个模型——Llama-3.1-8B-Instruct、Qwen3-8B、GPT-OSS-20B 与 DeepSeek-R1-0528-Qwen3-8B——是因为它们适合检验我们的专用特征 Crosscoder 能否检出显著的模型行为差异。
