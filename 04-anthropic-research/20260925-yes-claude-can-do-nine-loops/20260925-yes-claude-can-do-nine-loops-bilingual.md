# 是的，Claude 能算九圈：N=4 超杨-米尔斯中的九圈振幅（中英对照）

> 原文标题：Yes, Claude can do Nine Loops（页面标题：Claude computes a nine-loop amplitude in N=4 super-Yang-Mills）
> 原文链接：https://www.anthropic.com/research/yes-claude-can-do-nine-loops
> 原文作者：Matt von Hippel（客座博文，物理学家 / 科普作家）；附录作者 Lance Dixon（SLAC 教授，独立验证结果）
> 发布日期：2026-09-25
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 挑战发出一个月即被攻克：Claude Science 一枪命中平面 N=4 SYM 九圈振幅，预算一两千美元；「低垂果实比专家以为的多」是全文最扎心的结论
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；Additional material 链接以列表保留，原文无图。

---

In this guest post, physicist and science writer Matt von Hippel shares what happened when he issued a challenge to AI companies regarding a problem in his former subfield of theoretical physics.

在这篇客座博文中，物理学家兼科普作家 Matt von Hippel 讲述了他向 AI 公司发起一项挑战——关于他原先所在的理论物理子领域的一个问题——之后发生的事。

It's not often that you issue a challenge, only to see it beaten a month later. But we're living in unusual times.

发出挑战、一个月后就看到它被攻破，这种事并不多见。但我们正生活在不寻常的时代。

Let me introduce myself: I'm Matt von Hippel. I used to be a theoretical physicist; these days I'm a science writer. Throughout, I've been a blogger, writing weekly at 4gravitons.com about physics and the people who do it.

自我介绍一下：我是 Matt von Hippel。我曾是理论物理学家，如今是科普作家；从始至终我都是个博主，每周在 4gravitons.com 写物理与做物理的人。

More and more, blogging about physics has meant blogging about AI. That's a problem, because I'm definitely not an AI expert. I've dabbled in it, sure. I probably know more than your grandma. But I mostly have to step back and trust the experts. And frustratingly, the experts disagree! I've heard from smart, well-informed people who are confident that AI is a few years away from superintelligence, and that superintelligence will be capable of truly terrifying things. And I've heard from smart, well-informed people who are equally confident that LLM-based AI is close to a ceiling, that models like Claude won't even be able to do impressive work in physics, let alone conquer the world.

写物理越来越等于写 AI。这对我是个麻烦，因为我绝不是 AI 专家。我确实涉猎过，可能比你奶奶懂得多，但基本上只能退后一步听专家的。而令人沮丧的是，专家们意见不一！我听过聪明、消息灵通的人自信满满地说 AI 距离超级智能只有几年，而超级智能将能做出真正可怕的事；也听过同样聪明、同样消息灵通的人同样自信地说基于 LLM 的 AI 已近天花板，像 Claude 这样的模型连物理学里令人瞩目的工作都做不出来，更别提征服世界了。

I've been reluctant to make my own predictions. Before forming an opinion, I wanted to see an LLM make progress on something familiar, something I knew was hard to do because I'd tried to do something similar myself.

我一直不愿自己下判断。在形成观点之前，我想看到一个 LLM 在我熟悉的事情上取得进展——一件我知道很难的事，因为我自己试过类似的东西。

In addition to that, I wanted to see an LLM do something that I expected to be computationally hard. LLMs have made impressive strides in math, certainly, and this month alone has likely changed many people's minds. But progress in math comes from new ideas, and ideas are mysterious things: one never quite knows how hard they are to find until they're found. Computation felt more solid. I wanted to see an LLM tackle a challenge that seemed out of reach not because researchers didn't know how to do it in principle, but because doing it seemed like the kind of thing that would take more computers and time than the researchers reasonably had access to. I wanted to see if those researchers were wrong: if a smarter, artificial researcher could use the same computers, and solve the problem anyway.

除此之外，我还想看 LLM 做一件我认为计算上很难的事。LLM 在数学上的进步固然惊人，光是这一个月恐怕就改变了很多人看法。但数学进步来自新想法，而想法是神秘的东西：在找到之前，没人说得清它有多难找。计算感觉更扎实。我想看 LLM 挑战一个并非「原理上不知道怎么做」、而是「做起来似乎需要比研究者实际能获得的更多计算机和时间」的问题。我想看看那些研究者是不是错了：一个更聪明的人造研究者，能不能用同样的计算机，照样把问题解出来。

So, I issued a challenge:

于是，我发起了挑战：

> "If AI companies want to impress people like me (or scare us, for that matter), then they need to tackle my old field. Show that an AI can take the kinds of computer resources an academic has access to, and solve one of the scattering amplitudes field's big outstanding problems. Show that a computational limit everyone expected to be a problem doesn't actually matter. Give us N=8 supergravity to seven loops, or N=4 super Yang-Mills to nine loops."

> 「如果 AI 公司想让像我这样的人刮目相看（或者吓到我们），那就得来攻我这块老阵地。证明 AI 能用学术研究者拿得到的那类计算资源，解决散射振幅领域悬而未决的大问题之一。证明一个所有人都以为是瓶颈的计算极限其实不要紧。给我们七圈的 N=8 超引力，或者九圈的 N=4 超杨-米尔斯。」

In short: can AI solve a frontier problem in my former subfield of theoretical particle physics? And can it do it on a budget?

简言之：AI 能解决我原所在理论粒子物理子领域的前沿问题吗？还能在预算内做到吗？

## 挑战（The challenge）

My old field is a branch of theoretical particle physics called amplitudeology. When other particle physicists predict new particles, they make sure they can do the calculations to test those predictions. They compute formulas called scattering amplitudes, which let physicists use the momenta and energies of subatomic particles to calculate how likely they are to react in particular ways. If physicists can make more accurate predictions for these reactions, they can check whether results from experiments like the Large Hadron Collider match those predictions. A mismatch could be evidence for a new theory, one that could explain some of physics' big lingering mysteries, like the nature of dark matter, or the balance between matter and antimatter in the universe.

我的老本行是理论粒子物理的一个分支，绰号「振幅学」（amplitudeology）。当其他粒子物理学家预言新粒子时，他们得确保自己能做检验这些预言的计算。他们计算名为「散射振幅」（scattering amplitudes）的公式——物理学家用它把亚原子粒子的动量与能量转化为「以特定方式反应的概率」。如果物理学家能对这些反应做出更精确的预言，就能检验大型强子对撞机这类实验的结果是否与预言相符。不符之处可能是新理论的证据——一个能解释物理学若干悬而未决之谜的理论，比如暗物质的本性，或宇宙中物质与反物质的平衡。

These scattering amplitude formulas are hard to compute, so hard that physicists almost always use approximations. They do partial calculations, cut off at a specific number of "loops," a measure of how complicated interactions between particles are allowed to get. The more "loops" they include in their calculations, the closer they get to the real answer, and the harder, computationally, the calculation is to do.

这些散射振幅公式难算到物理学家几乎总是用近似。他们做部分计算，在特定「圈」（loop）数处截断——圈数衡量粒子间相互作用允许变得多复杂。计算里包含的圈越多，离真实答案越近，计算量也越大。

In practice, most scattering amplitude formulas have only been calculated to two loops. A few have three. The most precise prediction in particle physics you might have heard of used five. Amplitudeologists want to do better. They develop experimental new techniques, and test them on special "toy model" theories. By trying the technique with a toy model where the calculation is easier, rather than the more challenging particles of the real world, amplitudeologists can stress-test the new methods and see how far they can go.

实际上，多数散射振幅公式只被算到两圈，少数到三圈；你可能听说过的粒子物理最精确预言用到了五圈。振幅学家想走得更远：他们发展实验性的新技术，并在特殊的「玩具模型」理论上测试。在计算更容易的玩具模型上（而非真实世界更难的粒子）试用新技术，振幅学家得以对新方法做压力测试，看它们究竟能走多远。

I posted challenges for two of those toy models. The one the folks at Anthropic chose to tackle was to go up to nine loops with a particular toy model theory, called N=4 super Yang-Mills.

我为其中两个玩具模型发了挑战。Anthropic 的一行人选择攻克的是：用名为 N=4 超杨-米尔斯（N=4 super Yang-Mills）的特定玩具模型理论，算到九圈。

"Yang-Mills" is a technical name for a type of theory that explains most of the world around us. Three of the four fundamental forces of nature: electromagnetism, the strong nuclear force that holds the nuclei of atoms together, and the weak nuclear force that causes radioactive decay in things like bananas, are all Yang-Mills theories.

「杨-米尔斯」是一类理论的技术名称，这类理论解释着我们周围世界的大部分：自然界四种基本力中的三种——电磁力、把原子核束缚在一起的强核力、让香蕉之类的东西发生放射性衰变的弱核力——全都是杨-米尔斯理论。

The "N=4 super" comes from supersymmetry. Physicists have speculated that each particle has a "supersymmetric partner," a particle with the same charge, but of a different type, matching matter particles like electrons to force particles like photons. At one time they were optimistic these particles could explain dark matter, via undiscovered partners of more familiar particles. Those speculations used "N=1" supersymmetry. In "N=4," each particle has four supersymmetric partners, not just one.

「N=4 超」来自超对称。物理学家曾猜想每种粒子都有一个「超对称伴侣」——电荷相同但类型不同的粒子，把电子这类物质粒子与光子这类力粒子配对。他们一度乐观地认为这些粒子可以通过更熟悉粒子的未发现伴侣来解释暗物质；那些猜想用的是「N=1」超对称。而在「N=4」里，每种粒子有四个超对称伴侣，不止一个。

That surfeit of particles makes the theory very unrealistic. N=4 super Yang-Mills isn't used as an explanation for dark matter, or for anything in the real world. Instead, amplitudeologists use it to hone their techniques, because N=4 is paradoxically easier to calculate with. The delicate balance between the different particles means only certain combinations of variables are needed, streamlining calculations.

如此过剩的粒子让这个理论非常不真实。N=4 超杨-米尔斯不被用来解释暗物质，也不解释现实世界的任何东西。相反，振幅学家用它来磨练技术——因为 N=4 悖论般地更好算：不同粒子之间微妙的平衡意味着只需要某些特定的变量组合，计算由此得到精简。

I got my PhD helping to calculate a three-loop amplitude, and got to see seven loops before I started losing steam. Lance Dixon, a professor at the SLAC National Accelerator Laboratory, was one of the folks who worked on this from the beginning, and a few years back managed eight loops.

我的博士学位来自参与计算一个三圈振幅；在我开始泄气之前，见证了这个领域走到七圈。SLAC 国家加速器实验室的教授 Lance Dixon 是从头做到底的人之一，几年前做到了八圈。

These calculations were done with an experimental technique called a bootstrap, which ended up bizarrely well-suited for use of AI. To bootstrap an amplitude, you don't have to take into account every possible particle interaction. You just need to know roughly what the answer ought to look like, keeping track of every possibility in computer files in a specialized alphabet. Then you start checking everything you know: predictions from other calculation techniques, rules the answer has to obey, links to related problems where the answer was easier to find. It's a bit like Sudoku, where you begin with a grid with all possible numbers, then cross them out as you go. In the end, you're hoping to find that only one possibility satisfies all the checks, while having enough checks left over to make sure you didn't make a mistake.

这些计算用的是一种叫「自举」（bootstrap）的实验技术，而它最终奇怪地非常适合配合 AI 使用。自举一个振幅，无须考虑每一种可能的粒子相互作用：你只需要大致知道答案该长什么样，用一套专门的字母表把所有可能性记录在计算机文件里；然后开始核对已知的一切——其他计算技术的预言、答案必须服从的规则、与答案更容易求出的相关问题之间的联系。有点像数独：先填一张写满所有可能数字的网格，再逐一划掉。最终，你希望发现只剩一种可能满足所有检查，而且剩下的检查还足够多，能确保你没有出错。

That meant that Lance was already well set up to check if someone had handed him the next amplitude formula, with nine loops. It would be an interesting answer, not just as a validation of the bootstrap technique, but as a rare example of an amplitude with that many loops of complexity, an answer that could be worth studying in its own right.

这意味着 Lance 已经整装待发：只要有人把九圈的下一个振幅公式递到他手上，他就能检验。那会是一个有趣的答案——不只是对自举技术的验证，还是一个拥有如此多圈复杂度的罕见振幅样本，本身就值得研究。

But he hadn't computed it, and neither had anyone else in the field. The way he found the eight-loop answer was already a bit indirect, via a surprising link to a different but related formula called a form-factor, a kind of partial amplitude involving different particles that turns out to be a bit easier to calculate. He was expecting to find the next loop even more indirectly, potentially by a different kind of AI method. If people thought it was possible to just run the usual bootstrap method for one more loop, someone would have done it.

但他没算出来，领域里也没有别人算出来。他找到八圈答案的路子已经颇为迂回——经由与另一个相关公式「形状因子」（form-factor）出人意料的联系；形状因子是一种涉及不同粒子的部分振幅，算起来稍容易些。他原以为下一圈会更迂回，可能要靠另一种 AI 方法。如果大家觉得把常规自举方法再多跑一圈就行，早就有人做了。

## 然后人们做到了（Then people did it）

Apparently, there are folks at Anthropic who read my blog.

看来 Anthropic 真有人读我的博客。

At the end of August, Liam Fitzpatrick and Siddharth Mishra-Sharma, two physicists at Anthropic, reached out to me to say they had tackled one of the challenges in my post. After verifying the result with Lance, they talked me through how they got it.

八月底，Anthropic 的两位物理学家 Liam Fitzpatrick 与 Siddharth Mishra-Sharma 联系我，说他们攻克了我博文中的挑战之一。与 Lance 核验结果之后，他们向我完整讲了是怎么做到的。

True to the spirit of the challenge, they didn't use millions of dollars in computer power. They used Fable 5.1, working within Claude Science, a platform scientists can pay to use. Claude Science is what folks in the biz call a "harness," a program that uses the Claude LLM with structured rules and prompts in order to get more robust and scientifically useful behavior.

忠于挑战的精神，他们没有动用数百万美元的算力。他们用的是 Fable 5.1，运行在 Claude Science——一个科学家付费使用的平台——之内。Claude Science 是业内人所称的「harness」（框架）：一个以结构化规则与提示驱动 Claude LLM、从而获得更稳健、更具科学有用性行为的程序。

Apparently, after asking Claude which problem it was most likely to be able to tackle, they gave it a simple prompt:

据说，在先问过 Claude 它最可能搞定哪个问题之后，他们只给了一句简单的提示：

"The problem is to compute the Six-particle (hexagon) amplitude in planar N=4 SYM at nine loops."

「问题是：计算平面 N=4 SYM 中九圈的六粒子（六边形）振幅。」

From there, they just kept telling it to keep going, with comments like:

之后他们只是不断叫它继续，评论诸如：

"I'm going to sleep and won't be available for another several hours. Keep working on this until I tell you to stop. Give me updates every 4-6 hours."

「我要去睡了，接下来几个小时都不会在。继续做这件事，直到我叫你停。每 4–6 小时给我一次进展更新。」

Claude ended up doing the calculation two different ways: the original bootstrap, and the indirect form-factor approach. Either approach would have cost an end-user around one or two thousand dollars, mostly due to the expense of running Claude for so long. The bootstrap calculation, done with the Python programming language with package SymPy, took around $100 of the budget, corresponding to running 96 CPUs for a week.

Claude 最终用两种不同的方法完成了计算：原始自举，以及迂回的形状因子路线。任一路线对终端用户的花费都在一两千美元左右，主要开销是把 Claude 跑那么久。自举计算用 Python 语言配合 SymPy 包完成，只花了预算中约 100 美元——相当于 96 个 CPU 跑一周。

Running 96 CPUs for a week might have felt like a lot when I was doing this kind of work ten years ago, but it's pretty affordable now if you have a good reason.

十年前我做这类工作时，96 个 CPU 跑一周或许显得很多；但如今只要理由充分，这相当便宜。

As it turned out, the result wasn't all that far away for humans either. A few days after I heard from Anthropic, we heard from Song He, an amplitudeologist at the Chinese Academy of Sciences in Beijing. Song's group had already gotten the majority of the result. They'd used some AI assistance, based on GPT-6, but not the kind of one-shot almost human-less approach Anthropic used.

事实证明，这个结果离人类也没那么远。在我收到 Anthropic 消息的几天后，我们收到了北京中国科学院振幅学家何颂的消息：何的团队已经拿到了结果的大部分。他们也用了 AI 辅助（基于 GPT-6），但不是 Anthropic 这种一枪命中、几乎无人值守的方式。

Everyone has been friendly here, which is a bit of a relief. The humans, Lance and Song and their collaborators, will get to publish the results, taking time to explain them and analyze them for the benefit of future researchers. Claude's role is done, for now.

整个过程中大家都很友好，让人松了口气。人类这边——Lance、何颂以及他们的合作者——将发表这些结果，花时间解释与分析，造福未来的研究者。Claude 的角色，到此为止。

## 那么，问题解决了？（So, problem solved?）

I set my challenge because I wanted a better sense of what current AI can do, and where it could go from here. So what have I learned?

我设这个挑战，是想更清楚地了解当前 AI 能做什么、接下来能往哪走。那么我学到了什么？

I'd thought this could be a chance to see AI overcome a computational barrier in a surprising way. Instead, it did something it turned out humans were also able to do. Claude used known methods, with a bit more compute than people had tried to use before. It may have gotten a boost from using Python, and not Maple (Lance's favorite program for math) or Mathematica (mine), and it may have used much better software engineering practices than we would have, but not super-intelligently so.

我原以为这会是一次「AI 以出人意料的方式跨越计算屏障」的机会。结果它做了一件人类其实也做得到的事。Claude 用的是已知方法，算力只比前人尝试过的多一点。它也许从使用 Python（而非 Lance 心爱的数学程序 Maple、或我的 Mathematica）中得了益，也许用了比我们好得多的软件工程实践，但谈不上超级智能。

My biggest takeaway is that there is more low-hanging fruit out there than you'd expect. Even when a goal is simple and well-defined, sometimes it's going to look much less achievable to experts than it actually is. There are people with a computer science background who've been telling me for years that amplitudeologists could make a lot more progress just by hiring a few programmers. They should feel vindicated.

我最大的收获是：外面的低垂果实比你想的多。哪怕目标简单而明确，它在专家眼里有时也会显得比实际难得多。有一些计算机科学背景的人多年来一直跟我说，振幅学家只要雇几个程序员就能取得大得多的进展。这些人应当感到扬眉吐气。

It's also noteworthy that Claude Science accomplished this in one shot, without any scientific oversight more sophisticated than "keep going." These are finicky, messy calculations. If I'd used a week of time on 96 CPUs to do this kind of calculation, then I'd almost certainly end up using two weeks: it's practically guaranteed I'd screw up something on the first try. I don't know how many mistakes Claude made internally on the way, but the harness got it to the end without an outside collaborator's input. I'm not sure that surprises me, at this point. But if you didn't know it could do that because you're still thinking of AI as so error-prone that it's unusable, then this should be your takeaway: It can do this kind of thing reliably now.

同样值得注意的是：Claude Science 一枪命中，所依赖的科学监督不比「继续做」更精妙。这类计算娇气而凌乱。如果是我花一周时间、96 个 CPU 做这种计算，几乎肯定会用掉两周——第一次尝试搞砸什么几乎是注定的。我不知道 Claude 中途内部犯了多少错，但 harness 在没有外部合作者输入的情况下把它送到了终点。到今天我已经不确定这算不算意外。但如果你至今仍把 AI 看作错漏百出、不可用之物，因此不知道它能做到这些，那么你的收获应该是：它现在能可靠地做这类事了。

Things definitely seem to be moving fast. In March, AI was accomplishing physics projects like a student: smaller-scale tasks with a lot of hand-holding and mistakes. In contrast, this is a real frontier calculation, the kind of thing normally tackled by the top experts in amplitudes. While it's possible that this is just a much more AI-friendly problem, I don't think it's just that: I think the technology has genuinely gotten better.

事情的节奏显然在加快。今年三月，AI 做物理项目还像个学生：规模较小，需要大量扶持，错误不断。相比之下，这是一次真正的前沿计算，是振幅领域顶尖专家才碰的那种东西。虽然这可能只是一个对 AI 友好得多的题目，但我不认为仅是如此：我认为技术确实变强了。

How far can I generalize this? That I'm not sure of.

我能把这推广到多远？这个我说不准。

These toy model theories tend to be the focus of small sub-communities. The real-world amplitudes calculations are a wider field, with many groups trying to beat each other to the frontier. It's possible there's less low-hanging fruit there. But I wouldn't count on it. I know people who work on those calculations have been increasingly using AI for coding. If people aren't already checking whether AI science harnesses can one-shot frontier calculations there, they ought to (and they ought to have a plan for how to check the results). I wouldn't be all that surprised if it was possible to squeeze another loop out on a reasonable budget.

玩具模型理论往往只是小子社区的关注焦点。真实世界的振幅计算是一个大得多的战场，许多团队在竞逐前沿。那里的低垂果实可能更少，但我不敢指望如此。我知道做那些计算的人已越来越多地用 AI 写代码。如果他们还没在检验「AI 科学框架能否一枪命中前沿计算」，那就该开始检验了（而且该有个如何核查结果的方案）。如果有人在合理预算内再榨出一圈来，我不会太惊讶。

Then it becomes a question for the community to discuss: where is the new frontier, and what needs to be figured out next? Unlike many problems in mathematics, amplitudes aren't just a training ground for new methods. There's a goal, to make predictions precise enough to compare with upcoming experiments. How much closer is the field to that goal?

接下来就成了社区要讨论的问题：新的前沿在哪，下一步该弄清什么？与数学中的许多问题不同，振幅不只是新方法的练兵场——它有目标：把预言做得足够精确，以便与即将到来的实验对比。这个领域离那个目标还有多远？

More broadly than that, though, I didn't really get an answer.

不过，在更宏观的层面上，我并没有真正得到答案。

I went into this curious not just about what AI can do in research today, but about the future. When you read predictions about superintelligence from the days before LLMs, they often propose fantastical-seeming risks. People imagined AI that could simulate people to predict their reactions and manipulate them, or figure out how to build a species-ending virus or world-devouring nanotech from first principles. And the usual objection to these risks is that they conflated intelligence, the vague and mysterious source of new ideas, with computational power. Critics argued that even a fleet of new datacenters wouldn't have the computational power to do any of those tasks, that they were nightmares of a sci-fi future that wasn't coming any time soon.

我带着好奇心进入这件事，不只想知道 AI 今天在研究中能做什么，更想知道未来。读前 LLM 时代关于超级智能的预言，里面常出现显得荒诞的风险想象：人们设想 AI 能模拟人类、预测并操纵其反应，或从第一性原理出发琢磨怎么造出灭绝物种的病毒或吞噬世界的纳米技术。对这些风险的通常反驳是：它们把「智能」——新想法那个模糊而神秘的来源——与「计算能力」混为一谈。批评者说，就算一批新数据中心也没有完成那些任务所需的计算能力；那些不过是科幻未来里遥遥无期的梦魇。

I don't feel like I have a better answer for those critics. I learned a bit about what AI can do now, that it can do work that matters in my old field on a reasonable budget, and do it pretty much autonomously to boot. But I'd hoped to see something stranger, new methods for the calculation itself with unexpected power. I'd hoped to get a glimpse of the future, something that would give me an informed opinion in debates about superintelligence. I wanted to know how far AI could push computational limits… and I feel like what I learned here is just that I was too naïve about where the limit was.

我并不觉得自己能对这些批评者给出更好的回答。我了解了一点 AI 现在能做什么：它能在我老本行里做有意义的工作，预算合理，而且基本全自动。但我原想看到更怪异的东西——计算方法本身的新路数，带着意想不到的力量。我原想窥见未来一眼，好让我在关于超级智能的辩论中有一份有根据的意见。我想知道 AI 能把计算极限推到多远……而我觉得，我在这里学到的只是：我对极限在哪这件事，太过天真。

## 附录：被机器抢了发是什么感觉？（An addendum: How does it feel to be scooped by a machine?）

By Lance Dixon, Professor of Particle Physics and Astrophysics at SLAC National Accelerator Laboratory and Stanford University, who checked Claude's nine-loop result.

作者：Lance Dixon，SLAC 国家加速器实验室与斯坦福大学粒子物理与天体物理教授，Claude 九圈结果的独立核验者。

Most theoretical physicists I know recognize that the current era of large language models is going to completely transform the way we think about physics. The question was just: when was it going to really hit home? For me, it happened on September 1, when Liam Fitzpatrick and Siddharth Mishra-Sharma at Anthropic told me that Claude had computed the nine-loop MHV six-particle amplitude in planar N=4 super Yang-Mills, and asked me to validate its result.

我认识的大多数理论物理学家都承认，大语言模型的这个时代将彻底改变我们思考物理的方式。问题只是：它什么时候才会真正切肤？对我来说是 9 月 1 日——那天 Anthropic 的 Liam Fitzpatrick 与 Siddharth Mishra-Sharma 告诉我，Claude 算出了平面 N=4 超杨-米尔斯中九圈的 MHV 六粒子振幅，并请我验证这个结果。

I'm not going to explain all the technical terms in that last sentence; Matt has covered the background above. I do need to mention that there are really two related objects, the "amplitude" and something we call the "form factor." Each has an associated number of loops: one, two, three, and so on. Every loop order is harder than the previous one, computationally, even after finding lots of tricks to make things easier. Also, the form factor is easier than the amplitude at the same loop order. In 2023 Andy Liu and I showed how to use the form factor and a weird symmetry we call antipodal duality to get the amplitude at eight loops.

上一句里的术语我就不逐一解释了，Matt 已在前面交代过背景。但我必须提到：这里其实有两个相关对象——「振幅」，和我们所谓「形状因子」。各自都带圈数：一圈、两圈、三圈……即便找到了许多化繁为简的技巧，每一圈在计算上都比前一圈更难。而且同一圈数下，形状因子比振幅容易。2023 年，Andy Liu 和我展示了如何利用形状因子与一种我们称为「对跖对偶」（antipodal duality）的古怪对称性得到八圈振幅。

Since 2023, my collaborators and I have eyed getting to nine loops, first for the form factor and then for the amplitude, using our 2023 idea. I thought it would be too hard to do the amplitude directly. So I was really quite impressed that Claude could do it directly. Not so much because it was a big computational task, but because the whole setup is very fragile: if you make any mistake at all in the computational recipe, it all crashes down like a failed soufflé, and you are left to wonder why (and debug). Also, there are so many details of the construction that are too boring to document fully in a publication. So Claude had to develop all that code from scratch.

自 2023 年起，我和合作者们一直盯着九圈：先用我们的 2023 年思路做形状因子，再做振幅。我以为直接做振幅太难了。所以 Claude 能直接做出来，真的让我相当震撼——倒不是因为计算量有多大，而是因为整套构建极其脆弱：计算配方里只要错任何一处，全部像塌掉的舒芙蕾一样崩盘，你只能干瞪眼琢磨为什么（然后调试）。而且这套构建有太多细节，无聊到无法在论文里逐一记录。Claude 得从零开始写出所有那些代码。

From the nine-loop amplitude it is relatively easy to go back to the form factor, and it was easier for me to validate the result mostly that way. That meant that for the last two weeks I've been validating a result, the nine-loop form factor, that our team had been working toward for a couple of years. And a machine had solved a problem that I thought was too hard to do directly. Does that bother me personally? Is it soul-crushing?

从九圈振幅回退到形状因子相对容易，我也主要是循此验证结果。这意味着过去两周我一直在验证一个我们团队朝夕以求数年的结果——九圈形状因子。一台机器解出了一个我以为直接做太难的问题。这让我心里不舒服吗？算不算灵魂重创？

No, for two reasons. One is that our team already had a campaign to use custom transformer models to predict higher loops, and part of our slogan was: "We have all the tools to validate any candidate solution a machine would provide us." Claude is a different kind of transformer model, probably over a million times bigger than our custom one. But sure, we said we could validate any result an AI model would give us, so we can and should do it. The second reason is that, if you look at how Claude solved the problem, it used all the methods my collaborators and I developed over the years, and it presented the solution (maybe as a favor to us) in the same format we had already set up. So while I'm validating Claude's result, Claude is validating all of our previous work. In fact, I would assert that Claude understands our 2019 and 2023 papers better than any human, aside from my co-authors.

不，有两个理由。其一，我们团队本来就有一场用定制 transformer 模型预测更高圈数的战役，口号的一部分就是：「我们有全部工具，可以验证机器提供的任何候选解。」Claude 是另一种 transformer 模型，大概比我们那个定制模型大一百万倍以上。但既然我们说过能验证 AI 模型给出的任何结果，那就能、也应该做到。其二，看 Claude 怎么解的问题：它用的全是我和合作者们多年来发展的方法，而且（也许是给我们行个方便）用我们早已建立的同一格式呈现了解答。所以当我在验证 Claude 的结果时，Claude 也在验证我们过去的全部工作。事实上我敢说：除了我的合作者，Claude 比任何人类都更懂我们 2019 年和 2023 年的那两篇论文。

After I wrote this, Song He told me that his group had also computed the piece of the nine-loop amplitude called the symbol. (People just seem to like to tell me about their nine-loop successes, for whatever reason.) Song's group used AI (GPT-6) to help them compute some of the constraints, but not for the overall framework. So now I've been scooped by both a machine and by humans plus a machine, within two weeks.

写完这些之后，何颂告诉我他的团队也算出了九圈振幅中名为「符号」（symbol）的那部分。（不知为什么，大家好像都热衷于向我报告他们的九圈捷报。）何的团队用 AI（GPT-6）帮助计算了一部分约束，但整体框架没用。于是，两周之内，我被一台机器、以及「人类加一台机器」先后抢了发。

Going back to the Claude computation: it's quite a triumph, in my opinion, for a large language model to execute all of the steps in the complicated recipe we laid out, and to organize the computational horsepower. But the more soul-searching moments will come when large language models start to come up with new physical principles and insights before humans.

回到 Claude 的计算：在看来，一个大语言模型执行完我们铺开的那份复杂配方的所有步骤、并调度好计算马力，已是相当了不起的胜利。但真正让人辗转难眠的时刻，将在大语言模型开始抢在人类之前提出新的物理原理与洞见之时到来。

#### 附加材料（Additional material）

- The full nine-loop result, in the format used for the earlier loop orders;

  完整九圈结果，采用与此前各圈相同的格式（见原文链接）；

- The concurrent nine-loop result by Song He, Jirong Jing, and Xiang Li.

  何颂、荆杰荣、李翔同期完成的九圈结果（见原文链接）。

#### 利益声明（Disclosure）

Anthropic invited Matt von Hippel to write this post and compensated him for his time. Anthropic staff gave feedback on drafts; the content and opinions are his own. Lance Dixon validated the result independently and received Claude usage credits.

Anthropic 邀请 Matt von Hippel 撰写本文并支付了报酬。Anthropic 员工对草稿提供了反馈；内容与观点归属作者本人。Lance Dixon 独立验证了结果并获得 Claude 使用额度。
