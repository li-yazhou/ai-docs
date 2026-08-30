# 让外部研究者独立研究人们如何使用 Claude（中英对照）

> 原文标题：Enabling independent research on how people use Claude
> 原文链接：https://www.anthropic.com/research/enabling-independent-research
> 原文作者：Anthropic（项目负责人：Kunal Handa）
> 发布日期：2026-08-26
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）—— 首次向外部研究者开放自家使用数据的隐私保护试点，制度设计与数据释放流程的细节多于发现本身
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Earlier this year, we ran a pilot giving external researchers access to aggregate, real-world Claude usage data. Three research groups designed their own studies for Anthropic Insights, our privacy-preserving analysis tool; we ran the data collection on their behalf, and they conducted their own independent analysis. In this post, we share high-level results from those studies and what we learned running this pilot. We're also providing an expression of interest form for researchers who may want to work with us in the future.

今年早些时候，我们开展了一个试点项目，让外部研究者访问聚合形态的、真实世界的 Claude 使用数据。三个研究小组为 Anthropic Insights——我们的隐私保护分析工具——自行设计研究；我们代为执行数据收集，他们则独立开展分析。在本文中，我们分享这些研究的高层结果，以及运行这个试点过程中学到的东西。我们也为未来可能想与我们合作的研究者提供了一份意向登记表。

Ensuring the transition to transformative AI goes well requires understanding its impact on people and society. Right now, data on real-world interactions with AI is concentrated in a handful of labs. We think it would be good if more data was made widely available—to researchers, policymakers, and the general public.

要确保向变革性 AI（transformative AI）的过渡顺利进行，就需要理解它对人与社会的影响。而当前，关于人与 AI 真实交互的数据集中在少数几家实验室手里。我们认为，如果更多数据能够广泛开放——面向研究者、政策制定者与普通公众——将会是一件好事。

Researchers outside the labs have two options. They can draw on analyses the labs publish, which reflect real usage but often answer the lab's questions, rather than their own. Or they can use public datasets, which they can study however they like, but skew toward more casual use, and may not reflect how most people actually use AI. Neither is sufficient for independent research on how AI is actually being used.

实验室之外的研究者有两个选择。一是利用实验室发表的分析：它们反映真实使用，但回答的往往是实验室自己的问题，而不是研究者的问题。二是使用公开数据集：研究方式随心所欲，但它们偏向更随意的使用场景，未必反映大多数人实际上如何使用 AI。对于"AI 究竟如何被使用"的独立研究而言，两者都不够充分。

This spring, we piloted a program in which three external research institutions designed and ran their own studies on Claude usage data through Anthropic Insights (formerly named 'Clio'), the privacy-preserving tool our own teams use to analyze usage patterns across millions of Claude conversations. We hope to scale this program in the future, so we also conducted an additional privacy audit of all data shared with third-party researchers to verify that our privacy protections held (see Appendix).

今年春天，我们试点了一个项目：三家外部研究机构通过 Anthropic Insights（原名 "Clio"）——我们自己的团队用来分析数百万 Claude 对话使用模式的隐私保护工具——设计并运行了他们基于 Claude 使用数据的研究。由于我们希望未来扩大这个项目，我们还对分享给第三方研究者的全部数据做了额外的隐私审计，以验证我们的隐私保护确实有效（见附录）。

We believe this is the first time external researchers have run public independent studies on an AI company's own usage data. Below, we discuss what the external teams found, what we learned running the pilot, and what we are weighing as we decide how to expand the program more widely. We are also publicly releasing the aggregate data from each project.

我们相信，这是外部研究者第一次基于 AI 公司自己的使用数据开展公开的独立研究。下文中，我们讨论外部团队发现了什么、我们在运行试点中学到了什么，以及决定如何更大范围扩展该项目时我们在权衡什么。我们同时也公开了每个项目的聚合数据。

## 研究者们学到了什么（What the researchers learned）

We partnered with three research groups: the Social and Language Technologies (SALT) Lab at Stanford University, the Human Information Processing Lab at the University of Oxford, and METR, a non-profit organization that evaluates frontier AI models. Each group developed its own research questions and used Anthropic Insights to conduct privacy-preserving analysis of roughly 250,000 Claude.ai or Claude Code conversations from April-May 2026.

我们与三个研究小组合作：斯坦福大学社会与语言技术（SALT）实验室、牛津大学人类信息处理实验室，以及评估前沿 AI 模型的非营利组织 METR。每个小组自行拟定研究问题，并用 Anthropic Insights 对 2026 年 4 月至 5 月约 25 万条 Claude.ai 或 Claude Code 对话进行了隐私保护分析。

We wanted our external partners to have as much independence as possible, so our contractual review rights were limited to user privacy, information that could help people violate our usage policies, Anthropic's confidential information, and research accuracy. Anthropic otherwise had no say in the content of the findings and the researchers are free to publish their results even if they are inconvenient for Anthropic. Below are some early results. We're excited about the directions, and about what others will find now that the data is public.

我们希望外部合作伙伴拥有尽可能大的独立性，因此我们的合同审查权仅限于四个方面：用户隐私、可能帮助他人违反我们使用政策的信息、Anthropic 的机密信息，以及研究准确性。除此之外，Anthropic 对研究发现的内容没有发言权，研究者可以自由发表结果——哪怕结果对 Anthropic 不利。以下是一些早期结果。我们对这些方向感到兴奋，也对数据公开之后其他人将发现什么感到期待。

The Social and Language Technologies Lab studied how humans collaborate with AI. They looked at what types of work people bring to AI, what roles humans retain in completing that work, and where human-AI collaboration breaks down. They found:

社会与语言技术实验室研究人类如何与 AI 协作。他们考察人们把哪些类型的工作交给 AI、在完成这些工作过程中人类保留了哪些角色，以及人机协作在哪里失灵。他们发现：

- People bring high-stakes work to AI more than expected. Prior research suggested people mostly delegate low-accountability tasks to AI and keep consequential tasks (that is, work that affects others or is hard to undo) for themselves. But the SALT Lab found that over half of Claude conversations involved people delegating consequential tasks to AI. People were most likely to bring consequential work to Claude when seeking professional guidance, particularly on legal or financial questions.
- 人们把高风险工作交给 AI 的程度超出预期。以往研究表明，人们大多把低问责任务交给 AI，把后果重大（即影响他人或难以撤回）的工作留给自己。但 SALT 实验室发现，超过一半的 Claude 对话涉及人们把后果重大的任务委托给 AI。人们在寻求专业指导时最可能把重要工作交给 Claude，尤其是在法律或财务问题上。

- People usually direct and oversee the work when they collaborate with Claude. In nearly three-quarters of conversations, people set the direction while Claude assisted, and they usually adapted its output rather than using it verbatim. But even when directing Claude on the output they want, people vary in how much they understand and learn from what Claude produces.
- 与 Claude 协作时，人们通常负责指挥并监督工作。在近四分之三的对话中，人们定方向、Claude 做辅助，而且人们通常会改造它的产出而不是原样照用。但即便是在指挥 Claude 产出自己想要的结果时，不同的人对 Claude 所产出内容的理解与收获程度也各不相同。

- It is common for people to experience friction when collaborating with AI. However, that friction is often productive. The time and effort that people put into seeing how Claude attempts a task, identifying where the request was unclear or misunderstood, and iterating on their direction leads to better results–it pushes people to clarify their intent, refine the output, or stay engaged with the problem.
- 人们在与 AI 协作时经历摩擦是很常见的。不过，这种摩擦往往是建设性的。人们花时间与精力观察 Claude 如何尝试任务、找出请求中哪里不清楚或被误解、并对自己的指示做迭代——这会带来更好的结果：它促使人们澄清意图、打磨产出，或持续投入于问题本身。

Read their full writeup here.

他们的完整报告见原文链接。

The Human Information Processing Lab is studying how people feel while using Claude and how that relates to Claude's behavior. Their early results indicate:

人类信息处理实验室研究人们使用 Claude 时的感受，以及这种感受与 Claude 行为之间的关系。他们的早期结果表明：

- How people feel when using AI is linked to how AI behaves. The researchers found patterns of human and AI behavior appeared together in conversations: Claude being warm went together with people being more positive. Claude refusing or disagreeing went together with people pushing back. Claude being eccentric went together with people getting more intellectually engaged. And Claude simply helping went together with people seeming satisfied.
- 人们使用 AI 时的感受与 AI 的行为方式相关联。研究者发现人类与 AI 的行为模式在对话中成对出现：Claude 温暖，人们就更积极；Claude 拒绝或提出异议，人们就回击；Claude 表现得古怪，人们的智识投入反而更高；而 Claude 单纯地帮忙时，人们显得心满意足。

- People's experience when using AI looks a lot like it does on the rest of the web. The researchers found that the patterns among states like absorption, frustration, and enjoyment in Claude conversations closely resemble those in a separate study on everyday internet browsing, suggesting similarities in how people engage with AI and with other digital activity.
- 人们使用 AI 的体验与在互联网其他地方的体验颇为相似。研究者发现，Claude 对话中沉浸、挫败与愉悦等状态之间的关系模式，与另一项关于日常上网浏览的研究高度相似，这表明人们投入 AI 与投入其他数字活动的方式存在相似性。

They are still completing their writeup. When it is public, we will add a link to it here.

他们仍在撰写完整报告。公开后我们将在此处补上链接。

METR is estimating real-world productivity gains from coding agents and how these increases in productivity change across model generations. Their analysis of Claude Code conversations is still underway, but early results suggest:

METR 正在估算编程智能体（coding agent）带来的真实生产力提升，以及这些提升如何随模型代际变化。他们对 Claude Code 对话的分析仍在进行中，但早期结果表明：

- More capable models may save users more time. METR compared Claude's guesses on how long tasks would have taken without AI to how long they actually took with different Claude models. Their preliminary findings indicate newer models deliver significant speedup over older models. METR plans on sharing more as their analysis develops.
- 能力更强的模型或许能为用户节省更多时间。METR 把 Claude 对"没有 AI 时任务会花多久"的猜测，与不同 Claude 模型下的实际耗时做了对比。其初步发现显示，较新的模型比较旧的模型带来显著的提速。随着分析推进，METR 计划分享更多内容。

- AI can estimate time taken reasonably well. Because the analysis relies on Claude judging how long a task would take, METR compared those judgments to known completion times from a prior developer study. Claude's estimates correlated with the actual time taken by developers.
- AI 对耗时的估计相当靠谱。由于该分析依赖 Claude 判断一个任务会花多久，METR 把这些判断与此前一项开发者研究中的已知完成时间做了对比。Claude 的估计与开发者的实际耗时存在相关性。

- Next: measure how much AI accelerates research. METR is continuing to investigate how their study can provide insight into how much AI speeds up researchers' work, which could become increasingly important as AI takes on more of its own development.
- 下一步：度量 AI 对研究的加速。METR 正在继续研究如何让这项研究为"AI 能把研究者的工作提速多少"提供洞见——随着 AI 承担越来越多的自身开发工作，这一点会变得越来越重要。

They are still completing their writeup. When it is public, we will add a link to it here.

他们仍在撰写完整报告。公开后我们将在此处补上链接。

## 我们团队学到了什么（What our team learned）

Sharing usage data is largely unprecedented in AI, so this pilot was as much an experiment in running such a program as it was a way to enable third-party research in a privacy-preserving way. Protecting our users' privacy and the researchers' independence were both paramount, and we achieved both. Anthropic Insights is designed for this—researchers never accessed raw conversations, only aggregated outputs after the same legal and privacy review as our internal work. However, all of this made the pilot slow for an AI lab's normal research speed and resource intensive to run. Both factors present a challenge to effectively scaling it. For more details on how we ran this pilot, see the Appendix. Below we discuss what we learned and how we addressed the challenges that arose.

在 AI 行业，共享使用数据几乎没有先例，所以这个试点既是一次"如何运营这类项目"的实验，也是一种以隐私保护方式赋能第三方研究的途径。保护用户隐私与研究者独立性都至关重要，而这两点我们都做到了。Anthropic Insights 正是为此设计的——研究者从不接触原始对话，只能拿到与内部工作经过同样法律与隐私审查的聚合输出。然而，这一切也使这个试点按 AI 实验室通常的研究速度来看相当缓慢，且运行成本高昂。这两个因素都是有效规模化的挑战。关于我们如何运行这个试点的更多细节，见附录。下面我们讨论学到了什么，以及如何应对出现的问题。

It is valuable to pursue the same problem from different perspectives. Some of our partners' research questions overlapped with work being pursued internally. For example, METR's proposal was similar to our economics research on "Agentic coding and persistent returns to expertise." We found this overlap valuable: it gave external researchers the chance to examine similar data and draw their own conclusions. Whether those align with ours is something we'll follow as their study continues. We also connected METR with our Economics team and found that this connection improved both research teams' work.

从不同视角追求同一个问题是有价值的。一些合作伙伴的研究问题与我们内部正在进行的工作重叠。例如，METR 的提案与我们经济学研究"Agentic coding and persistent returns to expertise"（智能体编程与专家回报的持续存在）相似。我们发现这种重叠很有价值：它让外部研究者有机会检视相似的数据并得出自己的结论——这些结论是否与我们的吻合，我们会随着他们研究的推进持续关注。我们还把 METR 与我们的经济学团队连接起来，并发现这种连接让两个团队的工作都得到了改进。

Research methods that work internally need to adapt for external partners. When using Anthropic Insights, a researcher writes a question such as, "What type of guidance is this person asking for?" and Claude answers it for every conversation in the study. The answers are then aggregated into categories; researchers only see final categories and the percentage of conversations that fall under each one. Because we are relying on Claude's judgments, the tool is sensitive to a question's wording; a poorly phrased one can place conversations into categories that misrepresent them. Because no one can read the underlying conversations, these errors are hard to catch.

内部行之有效的研究方法，需要为外部合作伙伴做出调整。使用 Anthropic Insights 时，研究者写下诸如"这个人在寻求哪类指导？"的问题，Claude 会对研究中每一条对话作答。答案随后被聚合为类别；研究者只能看到最终的类别及每类对话所占的百分比。由于我们依赖 Claude 的判断，这个工具对问题的措辞很敏感：措辞不当的问题会把对话归入歪曲其本意的类别。而因为没人能阅读底层对话，这类错误很难被发现。

Internally, we manage this by iterating on the questions many times over weeks. External partners couldn't do that, since repeated privacy review before sharing each dataset would have made the study infeasible. Instead, we had them test their questions on WildChat, a public dataset of human-AI conversations where they could check the answers against the underlying conversations themselves. But WildChat skews toward casual and creative use, unlike Claude traffic, so some questions that performed well on WildChat produced misleading categories once applied to actual Claude conversations. We addressed this by providing guidance on how to interpret Anthropic Insight's outputs (see Appendix). Going forward, we are exploring how external researchers can develop their questions and categories more effectively in advance.

在内部，我们的应对方式是花数周时间对问题反复迭代。外部合作伙伴做不到这一点：每次分享数据集前都要重复隐私审查，会让研究变得不可行。于是我们让他们在 WildChat——一个公开的人机对话数据集——上测试问题，在那里他们可以对照底层对话来检查答案。但 WildChat 偏向随意与创意类使用，与 Claude 流量不同，因此一些在 WildChat 上表现良好的问题，一旦用于真实的 Claude 对话就产生了误导性的类别。我们的补救是提供关于如何解读 Anthropic Insights 输出的指南（见附录）。未来，我们正在探索如何让外部研究者更有效地预先打磨他们的问题与类别。

Maintaining transparency about misuse without enabling it. Some categories in our partners' Anthropic Insights outputs surfaced violations of our Acceptable Use Policy or Terms of Service—for instance a category of people seeking guidance on a prohibited activity. We think the public should know about misuse of our platform, so we shared most of these violations. The exceptions were categories that described how users got around our safeguards rather than what they attempted. Less than 5% of categories and conversations were affected in each study, and in each case we told researchers which clusters we had altered or removed and why. As a standard practice, when Anthropic Insights surfaces such violations, we share the aggregated data with our Safeguards team for their review. This is also an important process for our work with external researchers moving forward.

在不妨碍安全的前提下保持对滥用的透明。合作伙伴的 Anthropic Insights 输出中，有一些类别暴露了违反我们可接受使用政策（Acceptable Use Policy）或服务条款的行为——例如一个"人们就某项被禁止的活动寻求指导"的类别。我们认为公众应当了解我们平台上的滥用情况，所以我们公开了其中大部分违规内容。例外是那些描述"用户如何绕过我们的安全防护"而非"他们试图做什么"的类别。每项研究中受影响的类别与对话都不到 5%，且每次我们都告知研究者我们改动或移除了哪些聚类、为什么。作为标准流程，当 Anthropic Insights 暴露此类违规时，我们会把聚合数据交给安全团队（Safeguards team）审查。在我们未来与外部研究者的合作中，这也将是一个重要环节。

## 展望未来（Looking forward）

Understanding AI's effects on society is too big a job for AI companies alone. Real oversight needs external researchers asking their own questions of real-world usage data and publishing what they find independently.

理解 AI 对社会的影响，这项工作对 AI 公司一家之力来说太大了。真正的监督，需要外部研究者就真实世界的使用数据提出自己的问题，并独立发表他们的发现。

This pilot was an experiment: could external researchers conduct independent studies on our platform without compromising our users' privacy? The effort was more challenging than we expected, and we learned many lessons, but so far the answer seems to be yes. Our partners pursued research we would not have thought to design ourselves, and each told us something new about AI's real-world impacts. This is a promising first step, but there is far more to do.

这个试点是一次实验：外部研究者能否在我们的平台上开展独立研究，同时不损害用户隐私？这件事比我们预想的更具挑战，我们也学到很多教训，但到目前为止，答案似乎是肯定的。我们的合作伙伴做了我们想不到去设计的研究，每一项都告诉了我们一些关于 AI 真实影响的新东西。这是充满希望的第一步，但要做的还远不止这些。

The next step is for us to determine whether we can scale this program, both in what kinds of studies we can support given the constraints described above, and in how many we can run at once. We are starting slowly to ensure privacy, safety, and research quality. We want to gauge interest and understand what researchers would want to study. If you are a researcher and access to Anthropic Insights would let you pursue work you cannot do today, please fill out this form.

下一步是确定我们能否把这个项目规模化——既包括在上述约束下我们能支持哪类研究，也包括我们能同时运行多少项。我们会缓慢起步，以确保隐私、安全与研究质量。我们想摸清需求，了解研究者想研究什么。如果你是研究者，而 Anthropic Insights 的访问权限能让你开展今天无法开展的工作，请填写我们的意向表（见原文）。

## 附录（Appendix）

The full appendix is available here. It describes how we ran the program, including how we chose our three partners, the research primer we wrote to explain the program's goals and what Anthropic Insights can do, and how each project moved from proposal to study design to analysis. It also includes details of our collaboration agreements, which explicitly say our partners are free to publish findings even when they are inconvenient for Anthropic. Furthermore, we cover the third-party privacy audit of this data, conducted by Imperial College London, and the privacy threat model we hold all released data to. We also include guidance on interpreting the data we are releasing from our partners' Anthropic Insights research studies.

完整附录见原文链接。它描述了我们如何运行这个项目，包括如何挑选三家合作机构、我们为解释项目目标与 Anthropic Insights 能力而撰写的研究入门材料，以及每个项目如何从提案走到研究设计再走到分析。附录还包括我们的合作协议细节——其中明确写明合作伙伴可以自由发表发现，哪怕结果对 Anthropic 不利。此外，我们还介绍了由帝国理工学院（Imperial College London）执行的第三方隐私审计，以及我们所有已发布数据所满足的隐私威胁模型。我们也提供了如何解读合作伙伴 Anthropic Insights 研究产出的数据的指南。

## 贡献与致谢（Contributions and acknowledgements）

Kunal Handa led the project, collaborated with the partners on their research proposals, drafted research guidance and the contract, ran partners' Anthropic Insights studies, helped build the technical infrastructure supporting partners' research, contributed to the internal review of the Anthropic Insights outputs, and wrote the blog post. Miranda Zhang coordinated the partnerships and communications, and contributed to all parts of the work. Gabriel Nicholas coordinated the third-party privacy audit, internal review of Anthropic Insights' outputs, and contributed to all parts of the work. Miles McCain wrote the guidance on interpreting Anthropic Insights outputs, developed technical infrastructure to support partners' research, contributed to the internal review of the Anthropic Insights outputs, and provided feedback on the blog post and privacy threat model. Ryan Heller contributed technical infrastructure to support partners' research. Saffron Huang contributed to the internal review of the Anthropic Insights outputs and provided key feedback and discussion. Thomas Millar and Suzanne Wang contributed technical infrastructure to support partners' research and to the internal review of the Anthropic Insights outputs. Shan Carter, Mo Julapalli, Matt Kearney, Sarah Pollack, and Judy Shen contributed to the internal review of the Anthropic Insights' outputs. Matthew Jagielski contributed to the privacy threat model. Shaoyi Zhang contributed technical infrastructure to support partners' research. Heather Whitney, Ankur Rathi, Aisling Keenan, and David Saunders provided legal and privacy guidance throughout the project. Jake Eaton and Sylvie Carr contributed to the framing and writing of the blog post. Jack Clark and Michael Stern provided valuable guidance, support, and discussion throughout the process. Deep Ganguli provided detailed guidance, organizational support, and feedback throughout all stages of the project.

Kunal Handa 领导本项目，与合作伙伴共同打磨研究提案，起草研究指南与合同，运行合作伙伴的 Anthropic Insights 研究，协助搭建支撑合作研究的技术基础设施，参与 Anthropic Insights 输出的内部审查，并撰写了这篇博客文章。Miranda Zhang 负责协调合作关系与沟通，并参与了工作的所有部分。Gabriel Nicholas 协调了第三方隐私审计与 Anthropic Insights 输出的内部审查，并参与了工作的所有部分。Miles McCain 撰写了关于解读 Anthropic Insights 输出的指南，开发支撑合作研究的技术基础设施，参与输出的内部审查，并为博客文章与隐私威胁模型提供反馈。Ryan Heller 为支撑合作研究贡献了技术基础设施。Saffron Huang 参与了输出的内部审查，并提供关键反馈与讨论。Thomas Millar 与 Suzanne Wang 为支撑合作研究贡献了技术基础设施，并参与了输出的内部审查。Shan Carter、Mo Julapalli、Matt Kearney、Sarah Pollack 与 Judy Shen 参与了输出的内部审查。Matthew Jagielski 参与了隐私威胁模型。Shaoyi Zhang 为支撑合作研究贡献了技术基础设施。Heather Whitney、Ankur Rathi、Aisling Keenan 与 David Saunders 在整个项目中提供法律与隐私指导。Jake Eaton 与 Sylvie Carr 参与了博客文章的立意与写作。Jack Clark 与 Michael Stern 在整个过程中提供了宝贵的指导、支持与讨论。Deep Ganguli 在项目的所有阶段提供了详尽的指导、组织支持与反馈。

Additionally, we thank Miriam Chaum, Ishita Dasgupta, Esin Durmus, Adam Farina, Zoe Hitzig, Jerry Hong, Devin Kuokka, Hendson Lin, Maxim Massenkoff, Peter McCrory, Maryam Quasto, Nitarshan Rajkumar, Amie Rotherham, Divya Siddarth, Taylor Sorensen, Jerome Swannack, Alex Tamkin, Molly Villagra, Scott White, and Charles Yang for their helpful ideas, discussion, feedback, and support.

此外，我们感谢 Miriam Chaum、Ishita Dasgupta、Esin Durmus、Adam Farina、Zoe Hitzig、Jerry Hong、Devin Kuokka、Hendson Lin、Maxim Massenkoff、Peter McCrory、Maryam Quasto、Nitarshan Rajkumar、Amie Rotherham、Divya Siddarth、Taylor Sorensen、Jerome Swannack、Alex Tamkin、Molly Villagra、Scott White 与 Charles Yang 富有助益的想法、讨论、反馈与支持。

For their partnership in this program, we thank Vishakh Padmakumar, Yijia Shao, Jennifer Wang, Diyi Yang, and Dora Zhao from the Social and Language Technologies Lab at Stanford, Tsvetomira Dumbalska, Hannah Rose Kirk, and Christopher Summerfield from the Human Information Processing Lab at Oxford, and Joel Becker (now at Anthropic), Daniel Paleka, and Parker Whitfill from METR.

感谢本项目合作伙伴：斯坦福社会与语言技术实验室的 Vishakh Padmakumar、Yijia Shao、Jennifer Wang、Diyi Yang 与 Dora Zhao；牛津人类信息处理实验室的 Tsvetomira Dumbalska、Hannah Rose Kirk 与 Christopher Summerfield；以及 METR 的 Joel Becker（现已加入 Anthropic）、Daniel Paleka 与 Parker Whitfill。

For conducting the third-party privacy audit, we thank Zexi Yao, Bozhidar Stevanoski, Peter Romov, Euodia Dodd, Xiaoxue Yang, and Nataša Krčo.

感谢执行第三方隐私审计的 Zexi Yao、Bozhidar Stevanoski、Peter Romov、Euodia Dodd、Xiaoxue Yang 与 Nataša Krčo。

## 引用（Citation）

```
@online{handa2026enablingindependentresearch,
author = {Kunal Handa and Miranda Zhang and Gabriel Nicholas and Miles McCain and Ryan Heller and Saffron Huang and Thomas Millar and Suzanne Wang and Shan Carter and Mo Julapalli and Matt Kearney and Sarah Pollack and Judy Shen and Matthew Jagielski and Shaoyi Zhang and Heather Whitney and Ankur Rathi and Aisling Keenan and David Saunders and Jake Eaton and Sylvie Carr and Jack Clark and Michael Stern and Deep Ganguli},
title = {Enabling independent research on how people use Claude},
date = {2026-08-26},
year = {2026},
url = {www.anthropic.com/research/enabling-independent-research},
}
```
