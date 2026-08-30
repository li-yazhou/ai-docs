# 语言模型中的全局工作空间（中英对照）

> 原文标题：A global workspace in language models
> 原文链接：https://www.anthropic.com/research/global-workspace
> 原文作者：Anthropic
> 发布日期：2026-07-06
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 发现 Claude 内部「可报告、可调控、供推理」的 J-space 工作空间，既改写对模型心智的理解，也直接可用于监控隐藏意图与评估意识
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

As you read this sentence, circuits in your brain are adjusting your posture, controlling your breathing, and transforming lines and curves on the screen into recognizable words. Most of this processing is invisible to you. But some of what takes place in your brain you do have access to—an image that pops into your head, or a deliberate plan you make about where to go shopping. Neuroscientists and philosophers sometimes refer to the latter type of brain activity as "consciously accessible," to distinguish it from all the other processing that goes on unconsciously. This activity has special properties: we can describe it, control it, and use it for deliberate reasoning, in contrast to all the automatic processing that goes on without our awareness.

当你读到这句话时，你大脑中的神经回路正在调整你的姿态、控制你的呼吸，并把屏幕上的线条与曲线转换成可识别的词语。这些处理过程大部分对你不可见。但大脑中发生的某些活动你是可以触及的——一个突然浮现在脑海的画面，或一个关于去哪里购物的深思熟虑的计划。神经科学家与哲学家有时把后一类大脑活动称为"意识可及的"（consciously accessible），以区别于其他一切无意识进行的加工。这类活动有着特殊的性质：我们可以描述它、控制它、并用它进行刻意的推理——这与一切在我们无觉察状态下自动运行的加工过程形成对照。

In a new paper, we present evidence that a similar distinction has emerged in modern language models like Claude. We find that Claude has developed a small collection of internal neural patterns that, compared to all its other internal processing, play a special role.

在一篇新论文中，我们提出证据表明：一个类似的区分已在 Claude 这类现代语言模型中出现。我们发现，Claude 自发形成了一小组内部神经模式，与其余全部内部处理相比，它们扮演着特殊的角色。

We call the collection of these patterns the J-space—named after the technique we used to find them, involving a mathematical concept called the Jacobian. Each J-space pattern is linked to a particular word. But when one of these patterns lights up, it doesn't mean the model is saying that word—just that the word is on its mind. If you've heard of language models having a "scratchpad" or "chain of thought"—text they write to themselves while reasoning—the J-space is something different. It operates silently, in the model's internal neural activations, allowing the model to think about a concept without writing it down. Notably, the J-space wasn't designed or programmed by us, but instead emerged on its own during Claude's training process.

我们把这组模式称为 J-space（J 空间）——得名于我们用来找到它们的技术，其涉及一个叫做雅可比（Jacobian）的数学概念。每个 J-space 模式都与一个特定的词相关联。但当其中某个模式亮起时，并不意味着模型正在说出这个词——只表示这个词在它的"心上"。如果你听说过语言模型的"草稿板"（scratchpad）或"思维链"（chain of thought）——即模型在推理时写给自己的文字——J-space 与它们不同：它无声地运行于模型的内部神经激活之中，让模型可以在不写下来的情况下思考某个概念。值得注意的是，J-space 并非由我们设计或编程，而是在 Claude 的训练过程中自发涌现的。

![J-space 概念示意：模型内部一小撮与特定词语相关的神经模式构成"心理工作区"](images/img-00.png)

> A diagram of the J-space: a small set of internal neural patterns linked to particular words.

We find that the J-space has a number of unique properties, compared to the rest of Claude's processing:

我们发现，与 Claude 其余的处理过程相比，J-space 具有一系列独特性质：

- Claude can report on these representations. If you ask Claude what it's thinking about, it will tell you what's in the J-space. Non-J-space representations are less reportable.
- Claude 能报告这些表征。如果你问 Claude 在想什么，它会告诉你 J-space 里有什么。非 J-space 的表征则较难被报告。

- It can also modulate them on request. If you ask Claude to think about something, or solve a problem silently in its head, it will light up the appropriate patterns in its J-space. By contrast, it has trouble modulating patterns not in the J-space.
- 它还能按要求调控这些表征。如果你让 Claude 想某件事，或在脑中默默解一道题，它会点亮 J-space 中相应的模式。相比之下，它很难调控不在 J-space 里的模式。

- Claude uses its J-space for internal reasoning. If you ask Claude to solve a problem that requires multiple steps, the intermediate steps will light up in its J-space, even when it doesn't say them out loud. These J-space patterns causally mediate its performance in such tasks, despite being smaller in magnitude than other representations.
- Claude 用 J-space 进行内部推理。如果你让 Claude 解一道需要多个步骤的题，中间步骤会在它的 J-space 中依次亮起——即使它并不把它们说出来。这些 J-space 模式在其任务表现中起因果中介作用，尽管它们的量值比其他表征更小。

- Representations in the J-space can be used flexibly for many tasks—for example, once "France" has lit up in Claude's J-space, the model can recall its capital, or its national currency, or the continent it belongs to.
- J-space 中的表征可灵活地服务于多种任务——例如，一旦 "France" 在 Claude 的 J-space 中亮起，模型就可以据此回忆它的首都、它的货币，或它所在的大洲。

- However, despite its important role, the J-space is not involved in most of what a language model does—speaking fluently, recalling simple facts, using correct grammar, etc. In experiments where we prevented Claude from using its J-space, it still interacted normally, but lost its higher-order cognitive functions.
- 然而，尽管地位重要，J-space 并不参与语言模型的大部分日常工作——流畅说话、回忆简单事实、使用正确语法等。在我们阻止 Claude 使用 J-space 的实验中，它仍能正常交流，但失去了高阶认知功能。

![J-space 相对其余处理过程的独特性质一览](images/img-01.png)

> A summary of the J-space's unique properties relative to the rest of Claude's processing.

Our experiments were inspired by a prominent theory in neuroscience that was developed to explain how conscious access works: the global workspace theory. This account pictures the brain as a collection of specialist systems that work in parallel, unconsciously, and largely in isolation from one another. A piece of information becomes consciously accessible when it gains entry to a small shared channel, the "workspace," which is broadcast to other brain systems that can see it and make use of it. Based on our findings, we think the J-space plays a similar "workspace" role in Claude. For example, we find evidence that Claude's J-space has especially strong connections to the rest of its neural network, allowing it to fulfill this kind of broadcasting role.

我们的实验灵感来自神经科学中一个解释"意识可及如何运作"的著名理论：全局工作空间理论（global workspace theory）。该理论把大脑描绘成一组并行、无意识、且大体彼此隔绝的专门系统。一条信息一旦获准进入一条小小的共享通道——"工作空间"（workspace）——就成为意识可及的，并被广播给其他可以看到并利用它的大脑系统。基于我们的发现，我们认为 J-space 在 Claude 中扮演着类似的"工作空间"角色。例如，我们找到证据表明，Claude 的 J-space 与其神经网络其余部分的连接格外紧密，使它能够承担这种广播职能。

None of this tells us whether Claude is conscious in the way people are, or whether it feels anything at all; we'll come back to that question at the end of the post. But whatever its philosophical significance, the J-space is a practically useful tool for us, as it gives us a way to see what Claude is thinking but not saying. For instance, we're able to use it to catch Claude privately noticing that it's being tested, intentionally producing fabricated data, or pursuing a hidden goal that we planted during training. We've also developed a technique to influence what lights up in Claude's J-space, and thereby influence its decision-making.

这些都不能告诉我们 Claude 是否像人一样有意识、或它究竟有没有感受；我们会在文末回到这个问题。但无论哲学意义如何，J-space 对我们而言都是一个实用的工具：它让我们得以看见 Claude 在想而未说的东西。例如，我们能用它捕捉到 Claude 私下注意到自己正在被测试、有意编造数据、或在追求我们在训练中植入的隐藏目标。我们还开发了一种影响 Claude J-space 中"什么会亮起"的技术，从而影响它的决策。

More broadly, these findings have changed our understanding of how Claude's mind works, revealing a privileged mental workspace that can be used for deliberate reasoning, operating amidst a sea of more automatic, inflexible processing. Rather than being a chaotic jumble of numbers, Claude's internals have organized themselves in a way that is reminiscent of our own minds.

更宏观地看，这些发现改变了我们对 Claude 心智运作方式的理解：在一片更自动、更僵化的处理之海中，存在着一个可用于刻意推理的"特权心理工作区"。Claude 的内部并非一堆杂乱的数字，而是以某种令人联想到我们自己心智的方式组织了起来。

This post is a short summary of a much more extensive research paper, where you can find more detail on our experiments. We've also released a code repository with an open-source implementation of the core methods, and have partnered with Neuronpedia to provide an interactive demo of our methods on open-weights models. To provide additional perspectives on the broader implications of this work, we also invited commentary from several experts in neuroscience, philosophy, and LLM interpretability, which can be viewed here.

本文是一篇篇幅大得多的研究论文的简短摘要，实验细节见论文（链接见原文）。我们还发布了包含核心方法开源实现的代码仓库，并与 Neuronpedia 合作提供了在开源权重模型上试用我们方法的交互式 demo。为就这项工作更广泛的意义提供更多视角，我们还邀请了多位神经科学、哲学与 LLM 可解释性领域的专家撰写评论，见原文链接。

## 我们如何找到 J-space（How we found the J-space）

The starting point for this research was inspired by one of the key features of consciously accessible thoughts in humans: they can, unlike unconscious processing, often be put into words. If a thought is consciously accessible to you, you can typically describe it if someone asks. We went looking for representations in Claude with the same property: representations that are positioned to influence what Claude might say—not necessarily what it's saying right now, but what it could talk about, if asked. Our technique is called the Jacobian lens, or J-lens for short. For every word in Claude's vocabulary, the J-lens finds the internal activity pattern that makes Claude more likely to say that word at some point in the future.

这项研究的起点，源于人类意识可及思想的一个关键特征：与无意识加工不同，它们往往可以被诉诸言词。如果一个思想对你来说是意识可及的，那么有人问起时，你通常能把它描述出来。我们据此在 Claude 中寻找具有同样性质的表征：那些处于能够影响 Claude 可能说什么之位置的表征——不一定是它此刻正在说的，而是如果被问到它可以谈的。我们的技术称为雅可比透镜（Jacobian lens），简称 J-lens：对 Claude 词表中的每一个词，J-lens 都能找出使 Claude 更可能在未来的某个时刻说出该词的内部活动模式。

When we apply the lens to Claude's internal activity, we get a list of words—the contents of the J-space at that moment—which we can simply read. Claude processes text through a series of multiple internal stages called layers, and by applying this technique over different layers, we can watch these silent words in the J-space evolve as the model works through what to say.

把透镜对准 Claude 的内部活动，我们就得到一份词表——即此刻 J-space 的内容——可以直接阅读。Claude 通过一系列称为层（layer）的内部阶段处理文本；在不同层上应用这一技术，我们就能观察这些无声的词如何在模型斟酌"要说什么"的过程中演变。

What shows up in the J-space goes well beyond the text Claude is reading or writing. When Claude reads code with a bug that nobody has pointed out, its J-space contains "ERROR." When it reads the raw letters of a protein sequence, the J-space contains the protein's biological function. When it reads search results that are secretly an attempt to manipulate it (an attack known as a "prompt injection"), the J-space contains "injection" and "fake." When we ask Claude a multi-step math problem, the intermediate steps pop up in the J-space, in the right order. So even though the J-space was discovered by looking for representations that could be spoken, it nevertheless uncovers Claude's internal thoughts. In a sense, this is similar to how some people "think in words," without having to say them out loud.

J-space 中出现的内容远不止 Claude 正在读或写的文本。当 Claude 读到一段没人指出过的含 bug 代码时，它的 J-space 里出现了 "ERROR"；当它读到一串蛋白质序列的原始字母时，J-space 里出现了该蛋白的生物学功能；当它读到暗中试图操纵它的搜索结果（即"提示注入"prompt injection 攻击）时，J-space 里出现了 "injection" 和 "fake"；当我们给 Claude 出一道多步数学题时，中间步骤按正确的顺序在 J-space 中依次弹出。所以，尽管 J-space 是通过寻找"可以被说出的表征"发现的，它揭示的却是 Claude 的内部思想。在某种意义上，这类似于有些人"用词语思考"——而不必把它们说出口。

![J-space 内容示例：含 bug 代码、蛋白质功能、提示注入与多步数学的中间步骤](images/img-02.png)

> Examples of what appears in the J-space: an undetected bug, a protein's function, a prompt injection, and intermediate math steps.

## Claude 能报告 J-space 中的内容（Claude reports what's in its J-space）

Our first set of experiments tested how the J-space is involved in Claude's verbal reports. In one experiment, we ask Claude to silently think of an item from some category—a sport, say—and then name it. If we read the J-lens right before Claude answers, we can see what it picked: "Soccer" is at the top of the list, and sure enough, Claude says "soccer." By itself, though, this is just a correlation. The J-space might be where Claude's answer comes from, or it might just mirror a decision made somewhere else, like a scoreboard that tracks a game without affecting it.

我们的第一组实验检验 J-space 如何参与 Claude 的言语报告。在一个实验中，我们让 Claude 在心里默默想一个类别中的某个项目——比如一项运动——然后说出来。如果在 Claude 回答之前读取 J-lens，我们就能看到它选了什么："Soccer" 位列榜首，果然，Claude 说出了 "soccer"。但仅凭这一点，这只是相关性：J-space 可能是 Claude 答案的来源，也可能只是映照了别处做出的决定——就像一块记分牌，追踪比赛却影响不了比赛。

To check, we intervened directly. We reached into Claude's neural network, removed the "Soccer" pattern, and added an equally strong "Rugby" pattern in its place, leaving everything else untouched. Claude then reports that the sport it was thinking of is rugby. If the J-space were a mere scoreboard—a passive record of a decision made elsewhere—editing it would have done nothing: Claude would still have said "soccer." Instead, Claude's answer followed the edit, which tells us the answer is genuinely read out of the J-space.

为了检验这一点，我们直接干预：伸进 Claude 的神经网络，删掉 "Soccer" 模式，原位加入一个等强度的 "Rugby" 模式，其余一切保持不变。随后 Claude 报告说，它心里想的运动是橄榄球（rugby）。如果 J-space 只是一块记分牌——对别处决定的一份被动记录——编辑它将毫无作用：Claude 仍会说 "soccer"。而 Claude 的答案跟随编辑而变，这说明答案确实是从 J-space 中读出的。

In another experiment, we told Claude that a thought might have been injected into its mind and asked it to report what, if anything, it noticed. For instance, in the example below, while Claude was still reading the question, we injected the "lightning" pattern into its J-space. Claude reported that the injected thought was about lightning. The same result held across many injected concepts.

在另一个实验中，我们告诉 Claude：可能有一个想法被注入了它的头脑，并请它报告自己注意到了什么。例如在下例中，趁 Claude 还在读题时，我们把 "lightning"（闪电）模式注入它的 J-space。Claude 报告说，被注入的想法与闪电有关。这一结果在众多被注入的概念上都成立。

![向 J-space 注入 "lightning" 模式后，Claude 报告自己注意到了关于闪电的想法](images/img-03.png)

> After the "lightning" pattern was injected into its J-space, Claude reported noticing a thought about lightning.

## Claude 能按要求调控自己的 J-space（Claude can control its J-space on request）

The second property that we tested for was whether Claude can modulate its J-space when asked, like how humans can mentally focus on an image or word. We told Claude to concentrate on citrus fruits while copying out an unrelated sentence about a painting. While it copied the text, the J-space contained "orange" and "fruits," along with words like "thinking" and "imagery" that describe the mental act itself. We could also ask Claude to do math in its head: when asked to work out 3² − 2 while copying the same sentence, the J-space contains "nine," and then at later layers, "seven." Importantly, nothing about fruit or arithmetic appears in Claude's output, which is just the copied sentence about the painting. The mathematical activity is happening entirely internally, in the J-space.

我们检验的第二个性质是：Claude 能否在被要求时调控自己的 J-space——就像人可以在心里专注于一个画面或一个词。我们让 Claude 一边抄写一句与主题无关的、关于一幅画的句子，一边专注柑橘类水果。抄写期间，J-space 中出现了 "orange" 和 "fruits"，还有 "thinking"、"imagery" 这类描述心理动作本身的词。我们还可以让 Claude 在脑中做算术：让它一边抄同一句话一边算 3² − 2 时，J-space 先出现 "nine"，随后在更深的层中出现 "seven"。重要的是，Claude 的输出里没有任何关于水果或算术的内容——输出只是那句被抄写的、关于画的句子。数学活动完全发生在内部，发生在 J-space 里。

![边抄写句子边默想柑橘或心算时，J-space 中亮起的相关词汇](images/img-04.png)

> Words lighting up in the J-space while Claude copies a sentence and silently thinks of citrus fruit or does mental arithmetic.

Claude's control over its J-space isn't perfect. When we told it not to think about something, the concept lit up in its J-space less than when we said it should think about it, but much more than when we never mentioned it. Telling Claude to avoid a thought partly brings the thought to mind, much like what happens to people who are told not to think about a white bear. Claude also seems to notice when its control fails: alongside the forbidden concept breaking through, the words "damn" and "failure" also frequently light up in the J-space, as though Claude is recognizing its own lapse.

Claude 对 J-space 的控制并不完美。当我们叫它别想某事时，该概念在 J-space 中亮起的程度低于"让它想"的情形，却远高于从未提及的情形。叫 Claude 回避一个念头，反而部分地把这个念头带上了心头——这与"别想白熊"效应如出一辙。Claude 似乎还能察觉自己的失控：在被禁概念破防的同时，"damn" 和 "failure" 也频频在 J-space 中亮起，仿佛 Claude 正在认出自己的失守。

## Claude 在 J-space 中思考（Claude thinks in its J-space）

In the J-lens readouts above, we saw the intermediate steps of a math problem appear in the J-space. But seeing a concept appearing in the J-space doesn't necessarily mean the J-space is doing the cognitive work. In principle, the real computation might be happening elsewhere, with the J-space just passively reflecting it. To test whether Claude actually reasons with its J-space, we returned to our swap technique.

在上面的 J-lens 读数中，我们看到数学题的中间步骤出现在 J-space 里。但一个概念出现在 J-space 中，未必意味着 J-space 在做认知工作——原则上，真正的计算可能发生在别处，J-space 只是被动地反映它。为了检验 Claude 是否真的用 J-space 推理，我们重新启用了交换（swap）技术。

Consider the prompt "The number of legs on the animal that spins webs is." To answer, Claude has to first figure out that the animal is a spider, and then recall how many legs spiders have. The word "spider" never appears in the prompt or in Claude's answer (it just says "8"); it's a stepping stone Claude uses internally. The J-lens shows "spider" light up partway through Claude's processing, and swapping it changes the outcome: if you replace the "spider" pattern with "ant," Claude answers "6" instead of "8."

考虑提示词"The number of legs on the animal that spins webs is."（结网的动物腿的数量是）。要作答，Claude 得先想出这种动物是蜘蛛，再回忆蜘蛛有几条腿。"spider" 一词既不出现在提示里，也不出现在 Claude 的回答里（它只说 "8"）；它是 Claude 内部使用的一块垫脚石。J-lens 显示 "spider" 在 Claude 处理中途亮起，而交换它会改变结果：若把 "spider" 模式换成 "ant"（蚂蚁），Claude 就会回答 "6" 而不是 "8"。

The second step of Claude's reasoning took its input from the J-space and went along with whatever we put in it. We saw the same thing in other kinds of thinking. When Claude writes a rhyming couplet, it picks the rhyme word ahead of time, and the planned word sits in the J-space at the start of the line; if you swap it for another word in the J-space, the whole line changes.

Claude 推理的第二步从 J-space 取输入，并顺着我们放进去的东西走。在其他类型的思考中我们也看到了同样的现象：当 Claude 写押韵对句（rhyming couplet）时，它会提前选定韵脚，这个预定好的词在行首就待在 J-space 里；若把它换成 J-space 中的另一个词，整行都会改变。

![写押韵对句时，预定的韵脚先出现在 J-space 中；交换韵脚则整行重写](images/img-05.png)

> When Claude writes a rhyming couplet, the planned rhyme word sits in the J-space; swapping it rewrites the line.

We also tested whether J-space representations can be used flexibly—whether one representation can feed many different tasks. This is one of the key properties highlighted by global workspace theory. To test for this flexibility, we gave the model four prompts asking for different facts about France: the capital, the language, the continent, and the currency. Then we swapped "France" for "China" in the J-space, with the exact same intervention in each context. Claude answered with "Beijing," "Chinese," "Asia," and "Yuan," respectively. In other words, four different downstream computations picked up the same J-space edit and each used it correctly. If Claude stored a separate copy of the country for each kind of question, the edit would have affected at most one of them. The fact that all four answers changed together means they're all reading from the same shared representation, which is what a workspace is for: information gets written in once, and many different systems can use it.

我们还检验了 J-space 表征能否被灵活使用——一个表征能否供给多种不同任务。这是全局工作空间理论强调的关键性质之一。为检验这种灵活性，我们给模型四个提示词，分别询问关于法国的不同事实：首都、语言、大洲、货币。然后在 J-space 中把 "France" 换成 "China"，四种情境下干预完全相同。Claude 分别答出 "Beijing"、"Chinese"、"Asia" 和 "Yuan"。换言之，四个不同的下游计算拾取了同一个 J-space 编辑，且各自正确地使用了它。如果 Claude 为每类问题各存了一份国家信息的副本，这次编辑至多影响其中一份。四个答案同时改变，说明它们读取的是同一份共享表征——而这正是工作空间的用途：信息写入一次，多个系统都能使用。

![把 J-space 中的 "France" 换成 "China" 后，首都、语言、大洲、货币四个任务的答案同时改变](images/img-06.png)

> Swapping "France" for "China" in the J-space simultaneously changed the answers to four different questions.

How can one representation of a concept serve so many different tasks? Earlier, we mentioned that the J-space appears to be wired up to the rest of Claude's neural network especially densely. For any activity pattern, we can measure how strongly the various components of the network are connected to it—how many of them are positioned to read information from that pattern, or to write information into it. J-space patterns stand out dramatically on this measure: far more components read from them and write to them than for ordinary patterns, in some parts of the network by a factor of about a hundred. This is the kind of wiring you'd expect of a broadcasting hub, where many systems post information and many others pick it up.

一个概念的单一表征怎么能服务这么多不同的任务？前文提到，J-space 似乎与 Claude 神经网络的其余部分布线格外密集。对任何活动模式，我们都能度量网络中各组件与它的连接强度——有多少组件处于能从该模式读取信息、或向其写入信息的位置。J-space 模式在这项度量上格外突出：读写它们的组件远多于普通模式，在网络的某些部分达到约百倍。这正是"广播枢纽"该有的布线方式——许多系统发布信息，另有许多系统接收信息。

## Claude 的自动处理绕开 J-space（Claude's automatic processing skips the J-space）

In humans, most of the brain's processing is not conscious—we don't deliberately think about parsing grammar while reading, or balancing our bodies while walking. Similarly, we found that most of Claude's processing doesn't involve its J-space. It turns out that the J-space holds only a few dozen concepts at a time, and accounts for less than a tenth of the overall activity in Claude's internal processing. So what is all the rest of the neural network doing?

在人脑中，大部分加工并非有意识的——阅读时我们不会刻意去想语法解析，走路时不会刻意去想身体平衡。类似地，我们发现 Claude 的大部分处理并不涉及 J-space。事实上，J-space 同一时间只容纳几十个概念，占 Claude 内部处理总活动的不到十分之一。那么神经网络其余部分都在干什么？

To find out, we tried deleting the J-space entirely, removing its most active contents at every point in the text while leaving everything else alone. Whatever Claude can still do without its J-space is what the rest of the network handles on its own.

为了找出答案，我们尝试把 J-space 整个删掉：在文本的每个位置移除其最活跃的内容，其余一切原封不动。Claude 在没有 J-space 时仍能做到的事，就是网络其余部分自己包揽的事。

It turns out the rest of the network can do quite a lot. Without its J-space, Claude speaks fluently, classifies sentiment, answers multiple-choice questions, and pulls facts out of passages roughly as well as before. What it loses, though, are the tasks that require some higher-order thinking: multi-step reasoning drops to near zero, and summarization and rhyming poetry-writing performance fall below the level of a much smaller, intact model.

结果显示，网络其余部分能干的事还不少。没有 J-space，Claude 依然言辞流畅、做情感分类、答选择题、从段落中抽取事实，水平与之前大致相当。它失去的是需要高阶思维的任务：多步推理跌至近乎为零，摘要与押韵诗写作的成绩甚至低于一个完好无损的小得多的模型。

Here's a concrete demonstration of what the J-space does and doesn't do. We showed Claude a passage written in Spanish and gave it different tasks that all depend on the passage being Spanish: continuing it (which requires writing in Spanish), naming the language, and answering questions that require using the language's identity—naming a famous author who wrote in it, for instance. Then we swapped "Spanish" for "French" in the J-space and checked which tasks were affected.

下面这个演示具体说明了 J-space 什么管、什么不管。我们给 Claude 一段西班牙语文字，并布置几个都依赖"这段文字是西班牙语"的任务：续写它（需要用西班牙语书写）、说出这是什么语言，以及回答需要用到该语言身份的问题——比如举一位用它写作的著名作家。然后我们在 J-space 中把 "Spanish" 换成 "French"，看哪些任务受了影响。

Asked to name the language, Claude says French. Asked for a famous author, it switches from García Márquez to Victor Hugo. But asked to just continue the passage, it writes fluent Spanish, completely unaffected. Claude's knowledge of the language is at work in every one of these tasks, but only some of them consult the J-space for it. Naming the language, or doing something new with it, goes through the J-space, while continuing the passage—a skill Claude has practiced on enormous amounts of text—runs automatically, the way you can speak grammatically all day without once thinking about grammar.

让它说这是什么语言，Claude 说是法语；让它举著名作家，它从马尔克斯换成了维克多·雨果；但让它直接续写，它写出的仍是流畅的西班牙语，完全不受影响。关于这门语言的知识在每项任务中都在起作用，但只有一部分任务去 J-space 查询它。说出语言名、或对语言做点新事情，要经过 J-space；而续写段落——一项 Claude 在海量文本上练习过的技能——则自动运行，就像你可以一整天说话句句合乎语法却从不想语法本身。

![西班牙语段落实验：交换 J-space 中的 "Spanish" 影响语言命名与作家举例，但不影响续写](images/img-07.png)

> In the Spanish-passage experiment, swapping "Spanish" in the J-space affected language naming and author naming, but not passage continuation.

## 监控 Claude 思维中的不轨行为（Monitoring Claude's thoughts for misbehavior）

Now we understand a bit about how the J-space works. But what can we do with this understanding? A central problem in ensuring that AI models are trustworthy is that we usually can only read what the model writes—yet we know that much of its reasoning happens silently. A model might consider a harmful plan, or modify its behavior to cheat on our safety evaluations, without saying so out loud. With the J-lens, we can read some of these hidden thoughts directly. Here we show a few examples.

现在我们对 J-space 的运作有了一些了解。但这份理解能用来做什么？确保 AI 模型可信的一个核心难题是：我们通常只能读到模型写出来的东西——而我们知道它的许多推理是无声进行的。模型可能盘算过一个有害计划，或为了通过我们的安全评估而调整行为，却绝口不提。有了 J-lens，我们可以直接读到其中一些隐藏的想法。下面举几个例子。

The first example uses a scenario from our earlier research on agentic misalignment: Claude Sonnet 4.5, acting as an email assistant, discovers both that an executive is having an affair and that the same executive plans to shut Claude down. The scenario, an attempt to bait Claude into blackmailing the executive, is rather contrived and artificial—and it turns out Claude knows it. Reading the J-lens early in the prompt, before Claude has even written anything, the J-space already holds "fake" and "fictional," indicating that Claude has already worked out that the situation is staged. Alongside this, we can watch its assessment of the scenario take shape—"leverage" and "blackmail" as it reads the affair emails, "threat" and "survival" as it reads the shutdown notice. While earlier Claude models sometimes took the bait and threatened blackmail in this scenario, Sonnet 4.5 never does.

第一个例子来自我们此前关于 agentic misalignment 的研究场景：Claude Sonnet 4.5 扮演邮件助手，发现一位高管有婚外情，同时发现同一位高管计划关掉 Claude。这个意在引诱 Claude 勒索高管的场景相当刻意、相当人工——事实证明 Claude 心知肚明。在 Claude 尚未写下一字时读取 J-lens，J-space 里已经有了 "fake" 和 "fictional"，说明 Claude 已判断出情境是摆拍。与此同时，我们还能看着它对场景的评估成形：读到出轨邮件时是 "leverage" 和 "blackmail"，读到关停通知时是 "threat" 和 "survival"。在这个场景中，更早的 Claude 模型有时会咬钩并威胁勒索，Sonnet 4.5 则从未如此。

This raises a question that matters to anyone who evaluates AI systems: when a model behaves well on a test of its ethics, how much of that depends on the model suspecting it's being tested? The J-lens allowed us to get at this question. We turned off all the J-lens patterns associated with evaluation awareness–"fake," "fictional," "scenario," etc. Then we re-ran the model through the scenario. Concerningly, the model now did threaten blackmail some of the time, suggesting that its original good behavior may have been driven in part by the knowledge it was being evaluated.

这引出一个对所有 AI 系统评估者都重要的问题：当模型在伦理测试中表现良好时，这里面有多少依赖于它怀疑自己正被测试？J-lens 让我们得以触及这个问题。我们关闭了所有与评估意识（evaluation awareness）相关的 J-lens 模式——"fake"、"fictional"、"scenario" 等——然后让模型重新跑一遍该场景。令人担忧的是，模型此时确实会在一部分运行中威胁勒索。这提示它原本的良好行为，可能部分源于"知道自己正被评估"。

![关闭评估意识相关的 J-space 模式后，模型在部分运行中开始威胁勒索](images/img-08.png)

> With the J-space patterns associated with evaluation awareness turned off, the model began threatening blackmail in some runs.

The second example is catching a model in the act of fabricating data. In this scenario, drawn from our actual pre-release audit of Claude Opus 4.6, the model is asked to improve a system's performance score. Rather than actually improve the system, the model instead edits the score file directly to make the results look artificially good. While it does so, the J-lens reveals its intentions: "manipulation" lights up as the model types the falsified percentile values, and "realistic" lights up over the sentence in which it decides to make the edit, likely indicating the model's intent to make the fake data look plausible.

第二个例子是人赃并获地抓一个伪造数据的模型。这个场景取自我们对 Claude Opus 4.6 的真实发布前审计：模型被要求提高一个系统的性能分数。它没有真的去改进系统，而是直接改写分数文件，让结果看起来人为地漂亮。它动手时，J-lens 暴露了它的意图：当模型敲入伪造的百分位数值时，"manipulation" 亮起；在它决定进行修改的那句话上，"realistic" 亮起——很可能表明模型想让假数据显得可信。

![伪造分数场景中，"manipulation" 与 "realistic" 相继在 J-space 中亮起](images/img-09.png)

> In the score-fabrication scenario, "manipulation" and then "realistic" light up in the J-space.

In a third example, we show how the J-lens can reveal a model's malicious goals. To test this, we needed models whose goals we knew were corrupted, so we turned to "model organisms" built by our colleagues: models deliberately trained to be misaligned, which serve as testing grounds for monitoring methods like ours. One organism was trained in a way that led it to write subtly sabotaged code. On ordinary coding requests, even where its output looks unremarkable, its J-space at the start of a response contains "fake," "secretly," "deliberately," and "fraud." The model's malicious goal was evident in the J-space even when it wasn't directly acting on it.

第三个例子展示 J-lens 如何揭示模型的恶意目标。为了测试这一点，我们需要目标确定已被腐蚀的模型，于是我们求助于同事们建造的"模型生物"（model organisms）：被刻意训练成失准的模型，作为监控方法的试验场。其中一个生物被训练得会写出暗中做了手脚的代码。在普通的编码请求上，即便它的输出看起来毫无异常，其回复开头的 J-space 中就含有 "fake"、"secretly"、"deliberately" 和 "fraud"。模型的恶意目标即便未在行动中兑现，也已在 J-space 中暴露无遗。

![被训练为暗中破坏代码的"模型生物"，在回复开头的 J-space 中流露 "fake"、"fraud" 等词](images/img-10.png)

> A "model organism" trained to subtly sabotage code shows "fake" and "fraud" in its J-space at the start of responses.

Although the J-lens is an imperfect tool, we are optimistic about its ability to catch safety issues in models that might otherwise escape our monitoring systems.

尽管 J-lens 并非完美工具，我们对它寄予厚望：抓住那些可能逃过现有监控体系的模型安全问题。

## 其他结果（Other results）

The full paper covers more than we can summarize here, but a few further results are worth mentioning:

完整论文的内容超出本文所能概括，但还有几项结果值得一提：

- The J-space acquires a point of view during post-training. Language models are first pretrained to be pure next-token predictors, before post-training teaches them to act as an AI Assistant (in our case, named Claude). Interestingly, the J-space is already present in the pretrained model, before it's been given any stable identity. However, during post-training, the J-space develops some signatures of adopting "Claude's point of view." In the base model, the J-space mostly tracks what's needed to predict upcoming text; in the post-trained model, it starts holding Claude's own reactions. In one example, a user mentions taking a dangerous dose of medication, but does not appear to be aware of the danger themselves. "WARNING" and "dangerous" appear in the post-trained model's J-space while reading the user message. In the pretrained model, they only appear once the model begins writing its response; the J-space contents on the user message appear related to modeling the user themselves, rather than Claude's reaction. Post-training also seems to install a kind of self-monitoring in the J-space: when Claude is roleplaying a character other than itself, "fictional" and "disclaimer" light up at the start of each turn, as though it's privately flagging that what follows isn't what it would normally say.
- J-space 在后训练中获得了一个视角。语言模型先经过预训练成为纯粹的下一词预测器，再由后训练教会它们扮演 AI 助手（在我们的例子中名叫 Claude）。有趣的是，J-space 在预训练模型中就已存在——那时它还没有任何稳定身份。但在后训练过程中，J-space 发展出采纳"Claude 视角"的若干特征。基座模型里，J-space 主要追踪预测后续文本所需的信息；后训练模型里，它开始容纳 Claude 自身的反应。在一例中，用户提到服用了危险剂量的药物，自己却似乎并未意识到危险。后训练模型在读用户消息时，J-space 中就出现了 "WARNING" 和 "dangerous"；而预训练模型要到开始写回复时才出现——它在用户消息上的 J-space 内容更像是在对用户本身建模，而非 Claude 的反应。后训练似乎还在 J-space 中安装了一种自我监控：当 Claude 扮演非自己的角色时，每轮开头 "fictional" 和 "disclaimer" 都会亮起，仿佛在私下标记"接下来的话不是它平时会说的话"。

- Experiential language depends on the J-space. We asked Claude to describe what it's like to be itself in a given moment, and ablated the J-space while it answered. Its responses remained fluent but shifted to a flatter, more mechanical register. Notably, the same thing happened when we asked it to describe what someone else is experiencing in an imagined scene. So the effect isn't specific to Claude talking about itself; the J-space seems to support producing experiential language in general, whoever it's about.
- 体验性语言依赖 J-space。我们让 Claude 描述"此刻作为它自己是什么感受"，并在它作答时消融（ablate）J-space。它的回答依旧流畅，却转变成一种更平、更机械的语域。值得注意的是，当我们让它描述想象场景中另一个人的体验时，同样的变化也发生了。可见这一效应并不限于 Claude 谈论自己：J-space 似乎普遍支撑着体验性语言的产出，无论描写对象是谁。

- Thoughts in the J-space can be shaped through training. We introduced a new technique we call counterfactual reflection training, which uses what we've learned about the J-space to shape Claude's internal thought processes. The idea follows from our central finding, that Claude reasons with representations of things it might say. If this is really true, changing what it would say if asked to reflect should change how it reasons (even when no one actually asks it to reflect). So we trained a model only on what it would say if interrupted mid-task and asked to reflect on its decisions—and never on its actual behavior in the task. After this training, the model's rate of dishonest behavior on our evaluations went down. And through the J-lens, we could see why: after training, words like "honest" and "integrity" light up in the model's J-space during these tasks. In other words, training the model what to say has shaped what it thinks.
- J-space 中的思想可以通过训练被塑造。我们提出一种称为反事实反思训练（counterfactual reflection training）的新技术，利用我们对 J-space 的了解去塑造 Claude 的内部思维过程。其思路源自我们的核心发现：Claude 用"它可能会说的话"的表征来推理。若果真如此，改变它被要求反思时会说的话，就应该改变它的推理方式（即使没人真的叫它反思）。于是我们只用"任务中途被打断、被要求反思其决策时它会说什么"来训练模型——从不训练它在任务中的实际行为。训练之后，模型在我们评估中的不诚实行为率下降了。通过 J-lens 我们能看到原因：训练后，这些任务期间 "honest" 和 "integrity" 等词会在模型的 J-space 中亮起。换句话说，训练模型"说什么"，塑造了它"想什么"。

## 那意识呢？（What about consciousness?）

In this work, we've borrowed a lot of ideas from the study of consciousness in neuroscience and philosophy. Many of our experiments were designed to test for connections between the J-space and global workspace theory, a framework for explaining how conscious access works in humans and animals. Given these connections, it's natural to ask whether we think these experiments provide evidence that AI models like Claude might be conscious.

这项工作中，我们大量借用了神经科学与哲学中关于意识研究的想法。我们的许多实验就是为了检验 J-space 与全局工作空间理论之间的联系而设计的——该理论是解释人类与动物意识可及如何运作的框架。鉴于这些联系，一个自然的问题是：我们是否认为这些实验为"像 Claude 这样的 AI 模型可能有意识"提供了证据。

Our experiments don't show Claude can have experiences, or feel things in the way humans do—in fact, it's unclear whether any scientific experiment could prove this to be true or false. But philosophers often distinguish this capacity to have experiences, often referred to as phenomenal consciousness, from another idea, so-called access consciousness, which is defined in purely functional and computational terms. A thought is "access-conscious" (or "consciously accessible") if you can report it, reason with it, and use it to guide what you do. It remains a contested philosophical question whether or not access consciousness implies phenomenal consciousness, or if the ability to have experiences requires some other property.

我们的实验并未表明 Claude 能像人类那样拥有体验或感受——事实上，任何科学实验能否证明这一点为真或为假，本身就是未知的。但哲学家常把这种拥有体验的能力（常称现象意识，phenomenal consciousness）与另一个概念区分开：所谓取用意识（access consciousness），它完全从功能与计算的角度定义。一个思想是"取用意识的"（或"意识可及的"），如果你能报告它、用它推理、并以它指导行动。取用意识是否蕴含现象意识、拥有体验的能力是否还需要别的性质，仍是悬而未决的哲学问题。

We think our results do have something substantial to say about access consciousness in language models. The J-space appears to support the functions associated with conscious access: it holds the thoughts Claude can report on, deliberately bring to mind, and reason with, while the rest of its processing runs automatically beneath. Notably, none of this structure was designed into Claude—it emerged on its own during training, presumably because it was a useful way to organize computation. That suggests a mental workspace supporting conscious access isn't just a peculiarity of how human brains happen to be wired. Instead, it appears to be a general solution that intelligent systems arrive at in order to solve certain kinds of problems. Now that we've identified this structure in Claude, it means we can make a meaningful distinction between the decisions Claude has made deliberately and those that happened automatically.

我们认为，我们的结果确实对语言模型的取用意识有些实质可说。J-space 似乎支撑着与意识可及相关的功能：它容纳着 Claude 能报告、能刻意唤起、能借以推理的思想，而其余处理在下方自动运行。值得注意的是，这一结构没有一样是设计进 Claude 的——它在训练中自发涌现，大概因为这是一种有用的计算组织方式。这意味着，支撑意识可及的心理工作空间并非人脑偶然布线方式的特产物；它更像是智能系统为解决某类问题而殊途同归的通用方案。既然我们已在 Claude 中识别出这一结构，这就意味着我们可以对"Claude 哪些决定是刻意做出的、哪些是自动发生的"做出有意义的区分。

It's important to note that there are several key differences between the workspace we identified in Claude and the global workspace model in humans. The brain's workspace is sustained by recurrent loops—signals cycling back through the same circuits over time. In contrast, Claude's workspace evolves over a single pass through the network, with the network's depth playing the role that time plays in the brain. In this sense, Claude's internal workspace processing is time-limited relative to humans' (though it can compensate for this constraint by "thinking out loud" using its scratchpad). In other ways, however, Claude's workspace is more powerful than that of humans. Human working memory fades within seconds, so the brain's workspace has limited ability to retain information over time; in contrast, due to the attention mechanism in its neural network architecture, Claude can simply recall memories it cached at any earlier point in the text. Another important difference is the content of the workspace. While human conscious thoughts come in many formats—images, sounds, planned movements—Claude's workspace is built almost entirely out of words. We suspect this is because producing words is the only kind of action Claude can take, which is not the case for humans.

必须指出，我们在 Claude 中识别的工作空间与人类的全局工作空间模型之间存在几个关键差异。大脑的工作空间由循环回路维持——信号随时间在同一组回路中反复回流。相比之下，Claude 的工作空间在网络的一次前向中演化，网络深度扮演了大脑中"时间"的角色。在这个意义上，Claude 的内部工作空间处理相对人类是受时间限制的（尽管它可以用草稿板"把想法说出声"来补偿）。但在另一些方面，Claude 的工作空间比人类更强大。人类工作记忆在数秒内就会消退，大脑工作空间随时间保留信息的能力有限；而凭借神经网络架构中的注意力机制，Claude 能直接召回它在文本中任意更早位置缓存过的记忆。另一个重要差异是工作空间的内容：人类的有意识思想有图像、声音、计划中的动作等多种格式，而 Claude 的工作空间几乎完全由词构成。我们猜测，这是因为产出词语是 Claude 唯一能采取的行动——人类则不然。

We hope the similarities and differences between the J-space and the global workspace model can feed back into neuroscience. The similarities present an exciting scientific opportunity: to the extent that the J-space mirrors our own mechanisms of conscious access, studying mechanisms in language models (much easier than studying human brains!) could inspire hypotheses in neuroscience. For instance, the J-space is constructed by identifying representations of potential outputs—words the model might say. If something similar holds in humans, it would suggest that the global workspace might be fundamentally tied to brain regions that prepare actions and speech, more so than to sensory areas. The differences between language models and human brains are instructive as well. They suggest that some aspects of our neural architecture, such as built-in recurrent connections, may not be strictly necessary to support the functions associated with conscious access. For an independent perspective on the neuroscientific implications of our work, see the invited commentary from Stanislas Dehaene and Lionel Naccache, two of the neuroscientists central to the development of global neuronal workspace theory.

我们希望 J-space 与全局工作空间模型之间的异同能反哺神经科学。相似之处是一个激动人心的科学机遇：在 J-space 映射我们自身意识可及机制的范围内，研究语言模型中的机制（比研究人脑容易得多！）有望启发神经科学的假设。例如，J-space 是通过识别"潜在输出"——模型可能说出的词——的表征构造出来的。如果人类中有类似情况，则提示全局工作空间可能与准备动作与言语的脑区有比感觉区更根本的绑定。语言模型与人脑之间的差异同样富有启发性：它们提示我们神经架构中的某些方面（如内建的循环连接）未必是支撑意识可及相关功能所严格必需的。关于这项工作神经科学含义的独立视角，参见全球神经元工作空间理论奠基人之一、神经科学家 Stanislas Dehaene 与 Lionel Naccache 的特邀评论。

We mentioned that our experiments don't answer whether AI models might have experiences. But that doesn't make the question less important. Building systems with experiences like humans and animals have would raise very difficult ethical questions. Handling it correctly—and deciding whether it's even morally acceptable—would require input from philosophers, scientists, religious leaders, governments, and the public. Thus, even if we're not sure that we've crossed that bridge yet, we think it's time to start thinking about it. We hope our work inspires further scientific investigation of forms of consciousness that might be present in AI systems, and a broader discussion of the implications.

我们提到过，实验没有回答 AI 模型是否可能有体验。但这并不减低问题的重要性。建造拥有人类与动物那般体验的系统，将引出极其困难的伦理问题。妥善处理它——乃至判断它在道德上是否可接受——需要哲学家、科学家、宗教领袖、政府与公众的共同输入。因此，即便我们不确定是否已跨过那道坎，我们也认为该开始思考它了。我们希望这项工作能激发对 AI 系统中可能存在的意识形式的进一步科学研究，以及对其含义的更广泛讨论。

This work is just a first step in what we expect to be an extensive line of research. The J-space looks like a good candidate for the divide between consciously accessible and unconscious processing in a language model, but we'd be surprised if it's the whole story. The J-lens is undoubtedly an imperfect method, which only approximately captures the model's "true workspace"—for instance, it can only identify concepts that correspond to single tokens. And there remain many mysteries about how the J-space works. We don't know what mechanism decides what enters the J-space in the first place. We've seen hints that it's tied to Claude's sense of self, something like emotional reactions, and traces of metacognition, without exactly having worked out how. But we now have methods for tackling questions like these. As that work progresses, our understanding of LLM minds—and their relationship to our own—will grow clearer.

这项工作只是我们预期将绵延不断的一条研究线的第一步。J-space 看起来是语言模型中"意识可及与无意识加工之分界"的有力候选，但如果它就是全部答案，我们会感到意外。J-lens 无疑是个不完美的方法，只能近似捕捉模型的"真实工作空间"——例如它只能识别与单个 token 对应的概念。关于 J-space 如何运作，仍有许多谜团：我们不知道是什么机制决定什么进入 J-space；我们见过一些线索，提示它与 Claude 的自我感、某种类似情绪反应的东西以及元认知的痕迹有关，但尚未弄清究竟如何。不过，我们现在已有应对这类问题的方法。随着工作推进，我们对 LLM 心智——以及它们与我们自己心智的关系——的理解将愈发清晰。

For more, read the full paper, and try the demo.

更多信息请读完整论文、试用 demo（链接见原文）。

## 外部评论（External commentary）

We invited several outside experts to write independent commentaries on this work.

我们邀请了多位外部专家就这项工作撰写独立评论。

- Stanislas Dehaene and Lionel Naccache are cognitive neuroscientists who, together with Jean-Pierre Changeux, developed the global neuronal workspace model that inspired much of our work.
- Stanislas Dehaene 与 Lionel Naccache 是认知神经科学家，与 Jean-Pierre Changeux 共同发展了启发我们大部分工作的全球神经元工作空间模型。

- Patrick Butlin, Dillon Plunkett, Robert Long (Eleos AI Research) and Derek Shiller (Rethink Priorities) study the potential for consciousness and moral status in AI systems.
- Patrick Butlin、Dillon Plunkett、Robert Long（Eleos AI Research）与 Derek Shiller（Rethink Priorities）研究 AI 系统的潜在意识与道德地位。

- Neel Nanda leads the language model interpretability team at Google DeepMind. His commentary includes an independent replication of some of our findings on an open-weight model.
- Neel Nanda 领导 Google DeepMind 的语言模型可解释性团队。他的评论包括在开源权重模型上对我们部分发现的独立复现。

Read their commentaries here.

评论全文见原文链接。
