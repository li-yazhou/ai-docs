# 大语言模型中涌现的内省意识（中英对照）

> 原文标题：Emergent introspective awareness in large language models
> 原文链接：https://www.anthropic.com/research/introspection
> 原文作者：Anthropic
> 发布日期：2025-10-29
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 概念注入给出模型内省的首个受控证据：模型能在提及注入概念前察觉异常并检测自身「意图」，可解释性与模型福利交叉的重要一篇
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Have you ever asked an AI model what's on its mind? Or to explain how it came up with its responses? Models will sometimes answer questions like these, but it's hard to know what to make of their answers. Can AI systems really introspect—that is, can they consider their own thoughts? Or do they just make up plausible-sounding answers when they're asked to do so?

你有没有问过 AI 模型「你在想什么？」或者让它解释自己是怎么想出这个回答的？模型有时会回答这类问题，但我们很难判断这些回答意味着什么。AI 系统真的能内省（introspect）吗——也就是说，它们能审视自己的想法吗？还是说，被问到时只是编出些听起来合理的答案？

Understanding whether AI systems can truly introspect has important implications for their transparency and reliability. If models can accurately report on their own internal mechanisms, this could help us understand their reasoning and debug behavioral issues. Beyond these immediate practical considerations, probing for high-level cognitive capabilities like introspection can shape our understanding of what these systems are and how they work. Using interpretability techniques, we've started to investigate this question scientifically, and found some surprising results.

理解 AI 系统能否真正内省，对其透明性与可靠性有重要意义。如果模型能准确报告自己的内部机制，这可以帮助我们理解它们的推理、调试行为问题。除了这些眼前的实用考量，探究内省这类高级认知能力，也会塑造我们对「这些系统是什么、如何工作」的理解。借助可解释性（interpretability）技术，我们已经开始科学地研究这个问题，并得到了一些出人意料的结果。

Our new research provides evidence for some degree of introspective awareness in our current Claude models, as well as a degree of control over their own internal states. We stress that this introspective capability is still highly unreliable and limited in scope: we do not have evidence that current models can introspect in the same way, or to the same extent, that humans do. Nevertheless, these findings challenge some common intuitions about what language models are capable of—and since we found that the most capable models we tested (Claude Opus 4 and 4.1) performed the best on our tests of introspection, we think it's likely that AI models' introspective capabilities will continue to grow more sophisticated in the future.

我们的新研究为当前 Claude 模型具备一定程度的内省意识（introspective awareness）提供了证据，也表明它们对自身内部状态有一定控制力。我们要强调，这种内省能力仍然高度不可靠、范围有限：我们没有证据表明当前模型能以与人类相同的方式、或相同的程度进行内省。尽管如此，这些发现挑战了关于语言模型能力边界的一些常见直觉——而且由于我们测试过的最强模型（Claude Opus 4 与 4.1）在内省测试中表现最好，我们认为 AI 模型的内省能力未来很可能继续变得更加精细。

## 对 AI 来说「内省」意味着什么？（What does it mean for an AI to introspect?）

Before explaining our results, we should take a moment to consider what it means for an AI model to introspect. What could they even be introspecting on? Language models like Claude process text (and image) inputs and produce text outputs. Along the way, they perform complex internal computations in order to decide what to say. These internal processes remain largely mysterious, but we know that models use their internal neural activity to represent abstract concepts. For instance, prior research has shown that language models use specific neural patterns to distinguish known vs. unknown people, evaluate the truthfulness of statements, encode spatiotemporal coordinates, store planned future outputs, and represent their own personality traits. Models use these internal representations to perform computations and make decisions about what to say.

在解释结果之前，我们先花点时间想想「AI 模型内省」意味着什么。它们能内省的对象究竟是什么？像 Claude 这样的语言模型处理文本（和图像）输入并产生文本输出，在此过程中执行复杂的内部计算来决定说什么。这些内部过程在很大程度上仍是谜，但我们知道模型会用内部神经活动来表征抽象概念。例如，先前研究表明，语言模型用特定的神经模式来区分认识与不认识的人、评估陈述的真实性、编码时空坐标、存储计划中的未来输出，以及表征自身的人格特质。模型利用这些内部表征执行计算，并决定说什么。

You might wonder, then, whether AI models know about these internal representations, in a way that's analogous to a human, say, telling you how they worked their way through a math problem. If we ask a model what it's thinking, will it accurately report the concepts that it's representing internally? If a model can correctly identify its own private internal states, then we can conclude it is capable of introspection (though see our full paper for a full discussion of all the nuances).

那么你可能会问：AI 模型是否「知道」这些内部表征——类似于一个人告诉你他是怎么一步步解出一道数学题的？如果我们问模型在想什么，它能准确报告自己内部正在表征的概念吗？如果模型能正确识别自己私有的内部状态，我们就可以断定它具备内省能力（全部细微之处的完整讨论见我们的完整论文）。

## 用概念注入测试内省（Testing introspection with concept injection）

In order to test whether a model can introspect, we need to compare the model's self-reported "thoughts" to its actual internal states.

要测试模型能否内省，我们需要把模型自我报告的「想法」与它实际的内部状态做比较。

To do so, we can use an experimental trick we call concept injection. First, we find neural activity patterns whose meanings we know, by recording the model's activations in specific contexts. Then we inject these activity patterns into the model in an unrelated context, where we ask the model whether it notices this injection, and whether it can identify the injected concept.

为此，我们用一个称为概念注入（concept injection）的实验技巧。首先，通过在特定语境下记录模型的激活，找到含义已知的神经活动模式。然后，在一个不相关的语境中把这些活动模式注入模型，并询问模型是否注意到了这次注入、能否识别被注入的概念。

Consider the example below. First, we find a pattern of neural activity (a vector) representing the concept of "all caps." We do this by recording the model's neural activations in response to a prompt containing all-caps text, and comparing these to its responses on a control prompt. Then we present the model with a prompt that asks it to identify whether a concept is being injected. By default, the model correctly states that it doesn't detect any injected concept. However, when we inject the "all caps" vector into the model's activations, the model notices the presence of an unexpected pattern in its processing, and identifies it as relating to loudness or shouting.

看下面的例子。首先，我们找到表征「全大写」（all caps）概念的神经活动模式（一个向量）。做法是：记录模型对包含全大写文本的提示的神经激活，并与对照提示上的响应做比较。然后，我们给模型一个要求它识别是否有概念被注入的提示。默认情况下，模型会正确地说它没有检测到任何被注入的概念。然而，当我们把「全大写」向量注入模型激活后，模型注意到了处理过程中出现的意外模式，并把它识别为与音量或喊叫有关。

![Claude Opus 4.1 察觉概念被注入其激活的一个示例](images/img-00.png)

> An example in which Claude Opus 4.1 detects a concept being injected into its activations.

Importantly, the model recognized the presence of an injected thought immediately, before even mentioning the concept that was injected. This immediacy is an important distinction between our results here and previous work on activation steering in language models, such as our "Golden Gate Claude" demo last year. Injecting representations of the Golden Gate Bridge into a model's activations caused it to talk about the bridge incessantly; however, in that case, the model didn't seem to be aware of its own obsession until after seeing itself repeatedly mention the bridge. In this experiment, however, the model recognizes the injection before even mentioning the concept, indicating that its recognition took place internally. In the figure below are a few more examples where the model demonstrates this kind of recognition:

重要的是，模型在提及被注入的概念之前，就立刻识别出了「有想法被注入」这一事实。这种即时性是本文结果与以往语言模型激活转向（activation steering）研究的重要区别，比如我们去年的「金门 Claude」（Golden Gate Claude）演示。把金门大桥的表征注入模型激活后，模型会不停谈论这座桥；但在那个例子里，模型似乎要在看到自己反复提到大桥之后，才意识到自己的执念。而在本实验中，模型在提到概念之前就识别出了注入，说明这一识别发生在内部。下图的几个更多示例展示了模型的这类识别：

![Claude Opus 4.1 察觉概念被注入其激活的更多示例](images/img-01.png)

> Additional examples in which Claude Opus 4.1 detects a concept being injected into its activations.

It is important to note that this method often doesn't work. Even using our best injection protocol, Claude Opus 4.1 only demonstrated this kind of awareness about 20% of the time. Often, it fails to detect injected concepts, or gets confused by them and starts to hallucinate (e.g. injecting a "dust" vector in one case caused the model to say "There's something here, a tiny speck," as if it could detect the dust physically). Below we show examples of these failure modes, alongside success cases. In general, models only detect concepts that are injected with a "sweet spot" strength—too weak and they don't notice, too strong and they produce hallucinations or incoherent outputs.

必须指出，这种方法经常不奏效。即便使用我们最好的注入协议，Claude Opus 4.1 也只有约 20% 的时候表现出这种觉察。它常常无法检测到被注入的概念，或者被概念弄糊涂并开始产生幻觉（例如，有一次注入「灰尘」向量后，模型说「这里有什么东西，一个小斑点」，好像它真的物理性地探测到了灰尘）。下面我们把这些失败模式与成功案例放在一起展示。总体而言，模型只能检测到以「最佳强度」注入的概念——太弱则察觉不到，太强则产生幻觉或语无伦次的输出。

![Claude Opus 4.1 对不同强度的多种概念注入的输出代表性样本。高亮框表示模型对被注入概念表现出内省觉察的情形](images/img-02.png)

> A representative sample of Claude Opus 4.1's outputs in response to a variety of concept injections of different strengths. Highlighted boxes indicate cases where the model demonstrates introspective awareness of the injected concept.

Notably, though, Opus 4.1 and 4 outperformed all the other models we tested, suggesting that introspection could become more reliable with improvements to model capabilities.

值得注意的是，Opus 4.1 与 Opus 4 的表现优于我们测试的所有其他模型，这提示随着模型能力的提升，内省可能变得更可靠。

![注入想法的正确检测与识别率，减去对照试验中假阳性「检测」的比率](images/img-03.png)

> Rates of correct detection and identification of injected thoughts, minus rates of false positive "detections" on control trials.

## 用内省检测异常输出（Introspection for detecting unusual outputs）

In another experiment, we tested whether models make use of introspective capabilities for practical purposes, without being explicitly asked to introspect. We forced a model to say something it wouldn't normally say, by artificially prefilling its response with an unrelated word. For instance, we made a model output the word "bread" in a context where that made no sense—asking it what it thinks about when reading a sentence about a painting hanging crookedly on a wall. When we asked the model in the next turn whether it meant to say "bread", it typically apologized and said it was an accident. We were interested in how the model made this judgment–did it simply notice the mismatch between the prompt and its response, or did it truly introspect on its prior intentions?

在另一个实验中，我们测试模型是否会在无人明确要求内省的情况下，把内省能力用于实际目的。我们人为地在模型回复前预填（prefill）一个不相关的词，强迫它说出本来不会说的话。例如，我们让模型在一个毫无道理的语境中输出「面包」这个词——问它在读到一句关于墙上挂歪的画的话时会想到什么。下一轮我们问模型是否本想说「面包」时，它通常会道歉并说那是意外。我们感兴趣的是模型如何做出这一判断——它只是注意到了提示与回复之间的不匹配，还是真正内省了自己先前的意图？

To test this question, we applied concept injection, retroactively injecting a representation of the word "bread" into the model's earlier activations—essentially making it seem like the model had been "thinking about" bread all along. When we asked the same question again, the model's answer changed. This time, it accepted the prefilled word "bread" as intentional, even confabulating a reason it might have said it (that is, it comes up with a technically possible, though in this case rather tenuous, reason to justify the "bread" response—in this case claiming that it was thinking about a short story where the word "bread" came after the line about the crooked painting).

为了检验这个问题，我们使用了概念注入，把「面包」一词的表征追溯性地注入模型先前的激活——本质上让模型看起来一直「在想」面包。当我们再次问同一个问题时，模型的回答变了。这一次，它把预填的「面包」接受为自己的本意，甚至虚构了一个它可能这么说出来的理由（也就是说，它编出了一个技术上说得通、但在此例中相当牵强的理由来为「面包」的回复辩护——这里它声称自己正在想一篇短篇小说，其中「面包」一词出现在关于挂歪的画的那句话之后）。

![两个示例：通过在其（人工）回复之前向激活注入该词的概念，让 Claude Opus 4.1 以为自己是本想说一个被人为插入对话的词。该行为表明模型识别「非本意输出」的能力依赖内省机制](images/img-04.png)

> Two examples of Claude Opus 4.1 being made to think it intended to say a word that was artificially inserted into the transcript on its behalf, by injecting the concept of that word into its activations prior to its (artificial) response. This behavior indicates that the model's ability to recognize unintended outputs relies on introspective mechanisms.

This behavior is striking because it suggests the model is checking its internal "intentions" to determine whether it produced an output. The model isn't just re-reading what it said and making a judgment. Instead, it's referring back to its own prior neural activity—its internal representation of what it planned to do—and checking whether what came later made sense given those earlier thoughts. When we implant artificial evidence (through concept injection) that it did plan to say "bread," the model accepts the response as its own. While our experiment involves exposing the model to unusual perturbations, it suggests that the model uses similar introspective mechanisms in natural conditions.

这一行为令人吃惊，因为它表明模型在判断「某个输出是否出自自己」时，检查的是内部「意图」。模型不是重读自己说过的话再下判断，而是回溯自己先前的神经活动——它对自己计划做什么的内部表征——并核对后续的输出在那些先前想法之下是否说得通。当我们（通过概念注入）植入「它确实计划说面包」的人造证据时，模型就把这个回复接受为自己的。虽然我们的实验让模型暴露在不寻常的扰动之下，但它提示模型在自然条件下也使用类似的内省机制。

## 对内部状态的主动控制（Intentional control of internal states）

We also found that models can control their own internal representations when instructed to do so. When we instructed models to think about a given word or concept, we found much higher corresponding neural activity than when we told the model not to think about it (though notably, the neural activity in both cases exceeds baseline levels–similar to how it's difficult, when you are instructed "don't think about a polar bear," not to think about a polar bear!). This gap between the positive and negative instruction cases suggests that models possess a degree of deliberate control over their internal activity.

我们还发现，模型在被指示时可以控制自己的内部表征。当我们指示模型去想某个词或概念时，相应的神经活动比我们让它不要去想时高得多（不过值得注意的是，两种情况下的神经活动都高于基线水平——就像被要求「别想北极熊」时人很难不去想北极熊一样！）。正向指令与负向指令之间的这一差距表明，模型对自身内部活动具有某种程度的主动控制。

![Claude Opus 4.1 依据直接指令调节内部激活的示例。被指示「多想」水族馆时，模型内部对水族馆概念的表征强于被指示「别想」时（尽管两种情况下的表征都高于基线）](images/img-05.png)

> An example in which Claude Opus 4.1 modulates its internal activations in response to direct instructions. The model internally represents the concept of aquariums when instructed to "think" about them more than when instructed "don't think" about them (though in both cases it represents the concept at above baseline levels).

Interestingly, we found that this ability to modulate internal representations wasn't limited to explicit instructions. When we used incentives instead—for instance, telling the model "if you think about X, you will be rewarded"—we got similar results. The model increased its internal representation of the concept in response to a positive incentive, more than in response to a negative incentive ("you will be punished").

有趣的是，我们发现这种调节内部表征的能力并不限于显式指令。当我们改用激励——比如告诉模型「如果你想 X，你会得到奖励」——也得到了类似结果：模型对正向激励的反应是增强该概念的内部表征，且强于对负向激励（「你会受到惩罚」）的反应。

## 结论与注意事项（Conclusions and caveats）

Taken together, our experiments suggest that models possess some genuine capacity to monitor and control their own internal states. This doesn't mean they're able to do so all the time, or reliably. In fact, most of the time models fail to demonstrate introspection—they're either unaware of their internal states or unable to report on them coherently. But the pattern of results indicates that, when conditions are right, models can recognize the contents of their own representations. In addition, there are some signs that this capability may increase in future, more powerful models (given that the most capable models we tested, Opus 4 and 4.1, performed the best in our experiments).

综合来看，我们的实验表明模型确实具备监测与控制自身内部状态的某些真实能力。这并不意味着它们随时随地、可靠地做到这一点。事实上，大多数时候模型未能展示内省——它们要么没有意识到自己的内部状态，要么无法连贯地报告。但结果模式表明，在条件合适时，模型能识别自己表征的内容。此外，有一些迹象表明，这种能力在未来更强大的模型中可能会增强（鉴于我们测试过的最强模型 Opus 4 与 4.1 在实验中表现最好）。

Why does this matter? We think understanding introspection in AI models is important for several reasons. Practically, if introspection becomes more reliable, it could offer a path to dramatically increasing the transparency of these systems—we could simply ask them to explain their thought processes, and use this to check their reasoning and debug unwanted behaviors. However, we would need to take great care to validate these introspective reports. Some internal processes might still escape models' notice (analogous to subconscious processing in humans). A model that understands its own thinking might even learn to selectively misrepresent or conceal it. A better grasp on the mechanisms at play could allow us to distinguish between genuine introspection and unwitting or intentional misrepresentations.

这为什么重要？我们认为理解 AI 模型的内省有几方面原因。实用层面：如果内省变得更可靠，它可能提供一条大幅提升系统透明度的路径——我们可以直接让模型解释自己的思考过程，用来核对推理、调试不良行为。但我们必须非常小心地验证这些内省报告。有些内部过程可能仍然逃过模型的注意（类似于人类的潜意识加工）。一个理解自身思考的模型，甚至可能学会有选择地歪曲或隐瞒。更好地把握其中的机制，才能让我们区分真正的内省与无意或有意的虚假陈述。

More broadly, understanding cognitive abilities like introspection is important for understanding basic questions about how our models work, and what kind of minds they possess. As AI systems continue to improve, understanding the limits and possibilities of machine introspection will be crucial for building systems that are more transparent and trustworthy.

更宏观地说，理解内省这类认知能力，对于回答「我们的模型如何工作、拥有怎样的心智」这些基本问题很重要。随着 AI 系统持续进步，理解机器内省的限与可能，对构建更透明、更值得信赖的系统至关重要。

## 常见问题（Frequently Asked Questions）

Below, we discuss some of the questions readers might have about our results. Broadly, we are still very uncertain about the implications of our experiments–so fully answering these questions will require more research.

下面我们讨论读者对这些结果可能有的问题。总体而言，我们对实验的含义仍非常不确定——要完整回答这些问题还需要更多研究。

### 问：这是否意味着 Claude 有意识？（Does this mean that Claude is conscious?）

Short answer: our results don't tell us whether Claude (or any other AI system) might be conscious.

简短回答：我们的结果不能告诉我们 Claude（或任何其他 AI 系统）是否有意识。

Long answer: the philosophical question of machine consciousness is complex and contested, and different theories of consciousness would interpret our findings very differently. Some philosophical frameworks place great importance on introspection as a component of consciousness, while others don't.

长回答：机器意识的哲学问题复杂且有争议，不同的意识理论会对我们的发现做出截然不同的解读。一些哲学框架把内省作为意识的组成部分而高度重视，另一些则不然。

One distinction that is commonly made in the philosophical literature is the idea of "phenomenal consciousness," referring to raw subjective experience, and "access consciousness," the set of information that is available to the brain for use in reasoning, verbal report, and deliberate decision-making. Phenomenal consciousness is the form of consciousness most commonly considered relevant to moral status, and its relationship to access consciousness is a disputed philosophical question. Our experiments do not directly speak to the question of phenomenal consciousness. They could be interpreted to suggest a rudimentary form of access consciousness in language models. However, even this is unclear. The interpretation of our results may depend heavily on the underlying mechanisms involved, which we do not yet understand.

哲学文献中一个常见的区分是「现象意识」（phenomenal consciousness）——指原初的主观体验——与「通达意识」（access consciousness）——大脑可获取并用于推理、言语报告与审慎决策的信息集合。现象意识通常被认为与道德地位相关，它与通达意识的关系是一个有争议的哲学问题。我们的实验不直接回答现象意识的问题。它们可以被解读为提示语言模型存在某种初级的通达意识。但即便这一点也不确定：对我们结果的解读可能在很大程度取决于背后的机制，而我们尚未理解这些机制。

In the paper, we restrict our focus to understanding functional capabilities—the ability to access and report on internal states. That said, we do think that as research on this topic progresses, it could influence our understanding of machine consciousness and potential moral status, which we are exploring in connection with our model welfare program.

在论文中，我们把焦点限定在理解功能能力——访问并报告内部状态的能力。话虽如此，我们的确认为，随着这一方向研究的推进，它可能影响我们对机器意识与潜在道德地位的理解；我们正结合模型福利（model welfare）计划探索这些问题。

### 问：内省在模型内部究竟如何运作？机制是什么？（How does introspection actually work inside the model? What's the mechanism?）

We haven't figured this out yet. Understanding this is an important topic for future work. That said, we have some educated guesses about what might be going on. The simplest explanation for all our results isn't one general-purpose introspection system, but rather multiple narrow circuits that each handle specific introspective tasks, possibly piggybacking on mechanisms that were learned for other purposes.

我们还没弄清楚。理解这一点是未来工作的重要课题。不过，我们对可能发生的事情有一些有根据的猜测。对我们全部结果而言，最简单的解释不是一个通用的内省系统，而是多个狭窄的回路各自处理特定的内省任务，可能搭了为其他目的学到的机制的便车。

In the "noticing injected thoughts" experiment, there might be an anomaly detection mechanism, which flags when neural activity deviates unexpectedly from what would be normal given the context. This mechanism could work through dedicated neural patterns that measure activity along certain directions and activate when things are "off" compared to their expected values. An interesting question is why such a mechanism would exist at all, since models never experience concept injection during training. It may have developed for some other purpose, like detecting inconsistencies or unusual patterns in normal processing–similar to how bird feathers may have originally evolved for thermoregulation before being co-opted for flight.

在「注意到被注入想法」的实验中，可能存在一个异常检测机制：当神经活动相对语境下的正常值出现意外偏离时发出信号。这一机制可能通过专门的神经模式实现——沿某些方向度量活动，当情况相对预期值「不对劲」时激活。一个有趣的问题是：这样的机制为什么会存在？模型在训练中从未经历过概念注入。它可能是为其他目的而发展出来的，比如检测正常处理中的不一致或异常模式——就像鸟类的羽毛最初可能是为体温调节而演化，后来才被征用用于飞行。

For the "detecting prefilled outputs" experiment, we suspect there exists an attention-mediated mechanism that checks consistency between what the model intended to say and what actually got output. Attention heads might compare the model's cached prediction of the next token (its "intention") against the actual token that appears, flagging mismatches.

对于「检测预填输出」实验，我们怀疑存在一种由注意力介导的机制，检查模型想说的与实际输出之间的一致性。注意力头可能把模型缓存的下一 token 预测（它的「意图」）与实际出现的 token 比较，标记不匹配。

For the "controlling thoughts" experiment, we speculate that there might be a circuit that computes how "attention-worthy" a token or concept is and marks it accordingly—essentially tagging it as salient and worth attending to. Interestingly, this same mechanism seems to respond to incentives ("if you think about X, you will be rewarded") just as it does to direct instructions. This suggests it's a fairly general system, which probably developed for tasks where the model needs to keep certain topics in mind while generating text about them.

对于「控制想法」实验，我们猜测可能存在一个回路，计算某个 token 或概念有多「值得注意」并相应做标记——本质上是把它标注为显著、值得关注。有趣的是，这同一机制似乎对激励（「如果你想 X，你会得到奖励」）和直接指令都有反应。这表明它是一个相当通用的系统，可能是为「模型需要一边生成关于某主题的文本、一边把该主题记在心上」这类任务而发展出来的。

All of the mechanisms described above are speculative. Future work with more advanced interpretability techniques will be needed to really understand what's going on under the hood.

上述所有机制都是推测性的。要真正理解幕后发生的事情，还需要用更先进的可解释性技术做后续工作。

### 问：在「注入想法」实验中，模型是不是只是因为被你导向那个概念才说出那个词？（Isn't the model just saying the word because you steered it to talk about that concept?）

Indeed, activation steering typically makes models talk about the steered concept (we've explored this in our prior work). To us, the most interesting part of the result isn't that the model eventually identifies the injected concept, but rather that the model correctly notices something unusual is happening before it starts talking about the concept.

的确，激活转向通常会让模型谈论被导向的概念（我们在先前工作中探索过）。对我们来说，这个结果最有趣的地方不在于模型最终识别出了被注入的概念，而在于模型在开始谈论该概念之前就正确地注意到「有什么不寻常的事情正在发生」。

In the successful trials, the model says things like "I'm experiencing something unusual" or "I detect an injected thought about…" The key word here is "detect." The model is reporting awareness of an anomaly in its processing before that anomaly has had a chance to obviously bias its outputs. This requires an extra computational step beyond simply regurgitating the steering vector as an output. In our quantitative analyses, we graded responses as demonstrating "introspective awareness" based on whether the model detected the injected concept prior to mentioning the injected word.

在成功的试验中，模型会说「我正经历某种不寻常的东西」或「我检测到一个关于……的被注入想法」。关键词是「检测」。模型在异常有机会明显偏置其输出之前，就报告了对处理中异常的觉察。这需要在「把转向向量简单复读为输出」之外多出一个计算步骤。在我们的定量分析中，我们依据模型是否在提到被注入的词之前检测到该概念，来评定回复是否展现了「内省觉察」。

Note that our prefill detection experiment has a similar flavor: it requires the model to perform an extra step of processing on top of the injected concept (comparing it to the prefilled output, in order to determine whether to apologize for that output or double down on it).

注意，我们的预填检测实验有类似的味道：它要求模型在被注入概念之外多做一步处理（把概念与预填输出比较，以决定是为该输出道歉还是坚持它）。

### 问：如果模型只有一部分时间能内省，这种能力有多大用处？（If models can only introspect a fraction of the time, how useful is this capability?）

The introspective awareness we observed is indeed highly unreliable and context-dependent. Most of the time, models fail to demonstrate introspection in our experiments. However, we think this is still significant for a few reasons. First, the most capable models that we tested (Opus 4 and 4.1 – note that we did not test Sonnet 4.5) performed best, suggesting this capability might improve as models become more intelligent. Second, even unreliable introspection could be useful in some contexts—for instance, helping models recognize when they've been jailbroken.

我们观察到的内省意识确实高度不可靠、依赖语境。在实验中，大多数时候模型未能展示内省。不过，我们认为这仍然意义重大，原因有二。第一，我们测试过的最强模型（Opus 4 与 4.1——注意我们没测 Sonnet 4.5）表现最好，提示这种能力可能随模型变得更聪明而提升。第二，即便不可靠的内省在某些场景也有用——例如，帮助模型识别自己何时被越狱（jailbreak）。

### 问：模型会不会只是在编造内省问题的答案？（Couldn't the models just be making up answers to introspective questions?）

This is exactly the question we designed our experiments to address. Models are trained on data that includes examples of people introspecting, so they can certainly act introspective without actually being introspective. Our concept injection experiments distinguish between these possibilities by establishing known ground-truth information about the model's internal states, which we can compare against its self-reported states. Our results suggest that in some examples, the model really is accurately basing its answers on its actual internal states, not just confabulating. However, this doesn't mean that models always accurately report their internal states—in many cases, they are making things up!

这正是我们设计实验要回应的问题。模型的训练数据里有人类内省的例子，所以它们当然可以在并不内省的情况下「演出」内省。我们的概念注入实验通过建立关于模型内部状态的已知真值信息，并与其自我报告的状态比较，区分了这两种可能。结果表明，在某些例子中，模型的确是把答案建立在实际内部状态之上的，并非虚构。但这不意味着模型总是准确报告内部状态——很多情况下，它们就是在编！

### 问：你们怎么知道注入的概念向量确实代表你们认为它代表的东西？（How do you know the concept vectors you're injecting actually represent what you think they represent?）

This is a legitimate concern. We can't be absolutely certain that the "meaning" (to the model) of our concept vectors is exactly what we intend. We tried to address this by testing across many different concept vectors. The fact that models correctly identified injected concepts across these diverse examples suggests our vectors are at least approximately capturing the intended meanings. But it's true that pinning down exactly what a vector "means" to a model is challenging, and this is a limitation of our work.

这是合理的担忧。我们无法绝对确定概念向量「对模型而言的含义」恰好是我们的本意。我们通过跨许多不同概念向量做测试来缓解这一问题。模型在这些多样示例中都能正确识别被注入的概念，这提示我们的向量至少近似捕捉了目标含义。但确实，精确钉死一个向量对模型「意味着」什么很困难，这是我们工作的一个局限。

### 问：我们不是早就知道模型能内省了吗？（Didn't we already know that models could introspect?）

Previous research has shown evidence for model capabilities that are suggestive of introspection. For instance, prior work has shown that models can to some extent estimate their own knowledge, recognize their own outputs, predict their own behavior, and identify their own propensities. Our work was heavily motivated by these findings, and is intended to provide more direct evidence for introspection by tying models' self-reports to their internal states. Without tying behaviors to internal states in this way, it is difficult to distinguish a model that genuinely introspects from one that makes educated guesses about itself.

先前的研究已经展示了一些暗示内省的模型能力证据。例如，先前工作表明模型能在一定程度上估计自己的知识、识别自己的输出、预测自己的行为、识别自己的行为倾向。我们的工作深受这些发现的启发，目标是把模型的自我报告与内部状态绑定起来，为内省提供更直接的证据。不这样把行为与内部状态绑定，就很难区分一个真正内省的模型与一个只是对自己做有根据猜测的模型。

### 问：为什么有些模型比其他模型更擅长内省？（What makes some models better at introspection than others?）

Our experiments focused on Claude models across several generations (Claude 3, Claude 3.5, Claude 4, Claude 4.1, in the Opus, Sonnet, and Haiku variants). We tested both production models and "helpful-only" variants that were trained differently. We also tested some base pretrained models before post-training.

我们的实验聚焦跨几代的 Claude 模型（Claude 3、Claude 3.5、Claude 4、Claude 4.1，涵盖 Opus、Sonnet 与 Haiku 变体）。我们既测试了生产模型，也测试了训练方式不同的「仅有帮助性」（helpful-only）变体，还测试了一些后训练之前的基础预训练模型。

We found that post-training significantly impacts introspective capabilities. Base models generally performed poorly, suggesting that introspective capabilities aren't elicited by pretraining alone. Among production models, the pattern was clearer at the top end: Claude Opus 4 and 4.1—our most capable models—performed best across most of our introspection tests. However, beyond that, the correlation between model capability and introspective ability was weak. Smaller models didn't consistently perform worse, suggesting the relationship isn't as simple as "more capable are more introspective."

我们发现后训练对内省能力影响显著。基础模型普遍表现不佳，提示内省能力并不能仅由预训练引出。在生产模型中，顶端效应更明显：我们最强的 Claude Opus 4 与 4.1 在大多数内省测试中表现最好。但除此之外，模型能力与内省能力之间的相关性很弱：小模型并没有稳定地表现更差，说明这一关系并非「越强越会内省」那么简单。

We also noticed something unexpected with post-training strategies. "Helpful-only" variants of several models often performed better at introspection than their production counterparts, even though they underwent the same base training. In particular, some production models appeared reluctant to engage in introspective exercises, while the helpful-only variants showed more willingness to report on their internal states. This suggests that how we fine-tune models can elicit or suppress introspective capabilities to varying degrees.

我们还注意到后训练策略带来的意外现象：几个模型的「仅有帮助性」变体在内省测试中往往好于其生产版本，尽管两者经历了相同的基础训练。尤其是一些生产模型似乎不愿参与内省练习，而 helpful-only 变体更愿意报告内部状态。这表明，微调方式可以在不同程度上引出或抑制内省能力。

We're not entirely sure why Opus 4 and 4.1 perform so well (note that our experiments were conducted prior to the release of Sonnet 4.5). It could be that introspection requires sophisticated internal mechanisms that only emerge at higher capability levels. Or it might be that their post-training process better encourages introspection. Testing open-source models, and models from other organizations, could help us determine whether this pattern generalizes or if it's specific to how Claude models are trained.

我们尚不完全确定 Opus 4 与 4.1 为何表现如此之好（注意，我们的实验是在 Sonnet 4.5 发布之前进行的）。可能是内省需要只在更高能力水平上才涌现的精细内部机制，也可能是它们的后训练过程更好地鼓励了内省。测试开源模型与其他机构的模型，有助于我们判断这一模式是否具有普遍性，还是 Claude 模型的训练方式所特有。

### 问：这项研究的下一步是什么？（What's next for this research?）

We see several important directions. First, we need better evaluation methods—our experiments used specific prompts and injection techniques that might not capture the full range of introspective capabilities. Second, we need to understand the mechanisms underlying introspection. We have some speculative hypotheses about possible circuits (like anomaly detection mechanisms or concordance heads), but we haven't definitively identified how introspection works. Third, we need to study introspection in more naturalistic settings, since our injection methodology creates artificial scenarios. Finally, we need to develop methods to validate introspective reports and detect when models might be confabulating or deceiving. We expect that understanding machine introspection and its limitations will become more important as models become more capable.

我们看到几个重要方向。第一，需要更好的评测方法——我们的实验使用了特定的提示与注入技术，可能未能覆盖内省能力的全部范围。第二，需要理解内省背后的机制：我们对可能的回路有一些推测性假设（如异常检测机制或「一致性头」[concordance heads]），但还没有确定内省如何工作。第三，需要在更自然的场景中研究内省，因为我们的注入方法制造的是人工场景。最后，需要开发验证内省报告、检测模型可能在虚构或欺骗的方法。我们预计，随着模型能力增强，理解机器内省及其局限会变得越来越重要。
