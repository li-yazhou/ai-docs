# 开发 computer use 模型（中英对照）

> 原文标题：Developing a computer use model
> 原文链接：https://www.anthropic.com/research/developing-computer-use
> 原文作者：Anthropic
> 发布日期：2024-10-22
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，高价值）—— computer use 首篇开发复盘：SWE-bench/OSWorld 训练配方、专家演示+自我批判+纠错微调，屏幕解析与坐标定位的工程难点
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Claude can now use computers. The latest version of Claude 3.5 Sonnet can, when run through the appropriate software setup, follow a user’s commands to move a cursor around their computer’s screen, click on relevant locations, and input information via a virtual keyboard, emulating the way people interact with their own computer.

Claude 现在能够使用计算机了。最新版本的 Claude 3.5 Sonnet 在合适的软件环境中运行时，可以遵循用户的指令，在其电脑屏幕上移动光标、在相关位置进行点击，并通过虚拟键盘输入信息，模拟人们操作自己电脑的方式。

We think this skill—which is currently in public beta—represents a significant breakthrough in AI progress. Below, we share some insights from the research that went into developing computer use models—and into making them safer.

我们认为这项技能——目前正处于公开测试（public beta）阶段——代表着 AI 进步的一项重大突破。下面，我们分享开发 computer use 模型、并使其更加安全的研究过程中的一些心得。

## 为什么是 computer use？（Why computer use?）

Why is this new capability important? A vast amount of modern work happens via computers. Enabling AIs to interact directly with computer software in the same way people do will unlock a huge range of applications that simply aren’t possible for the current generation of AI assistants.

为什么这项新能力很重要？当今大量的工作都是通过计算机完成的。让 AI 能够像人一样直接与计算机软件交互，将解锁一大批当前一代 AI 助手根本无法实现的应用。

Over the last few years, many important milestones have been reached in the development of powerful AI—for example, the ability to perform complex logical reasoning and the ability to see and understand images. The next frontier is computer use: AI models that don’t have to interact via bespoke tools, but that instead are empowered to use essentially any piece of software as instructed.

过去几年，强大 AI 的发展已经抵达许多重要里程碑——例如执行复杂逻辑推理的能力，以及看见并理解图像的能力。下一个前沿是 computer use：AI 模型不再必须通过定制工具进行交互，而是能够按指令使用几乎任何软件。

## 研究过程（The research process）

Our previous work on tool use and multimodality provided the groundwork for these new computer use skills. Operating computers involves the ability to see and interpret images—in this case, images of a computer screen. It also requires reasoning about how and when to carry out specific operations in response to what’s on the screen. Combining these abilities, we trained Claude to interpret what’s happening on a screen and then use the software tools available to carry out tasks.

我们此前在工具使用（tool use）与多模态方面的工作，为这些新的 computer use 技能打下了基础。操作计算机需要看见并解读图像的能力——在这里就是计算机屏幕的图像；还需要进行推理，判断如何以及何时针对屏幕上的内容执行特定操作。把这两种能力结合起来，我们训练 Claude 解读屏幕上正在发生的事情，然后使用可用的软件工具来完成任务。

When a developer tasks Claude with using a piece of computer software and gives it the necessary access, Claude looks at screenshots of what’s visible to the user, then counts how many pixels vertically or horizontally it needs to move a cursor in order to click in the correct place. Training Claude to count pixels accurately was critical. Without this skill, the model finds it difficult to give mouse commands—similar to how models often struggle with simple-seeming questions like “how many A’s in the word ‘banana’”?.

当开发者让 Claude 使用某款计算机软件并授予必要的访问权限时，Claude 会查看用户所见画面的屏幕截图（screenshot），然后计算光标需要垂直或水平移动多少像素，才能点击定位到正确的位置。训练 Claude 精确地数像素至关重要。没有这项技能，模型就很难发出鼠标指令——这类似于模型常常在“banana 这个词里有几个 A？”这类看似简单的问题上表现吃力。

We were surprised by how rapidly Claude generalized from the computer-use training we gave it on just a few pieces of simple software, such as a calculator and a text editor (for safety reasons we did not allow the model to access the internet during training). In combination with Claude’s other skills, this training granted it the remarkable ability to turn a user’s written prompt into a sequence of logical steps and then take actions on the computer. We observed that the model would even self-correct and retry tasks when it encountered obstacles.

让我们惊讶的是，Claude 仅在计算器、文本编辑器等寥寥几款简单软件上接受 computer use 训练，就能如此迅速地举一反三（出于安全考虑，训练期间我们不允许模型访问互联网）。与 Claude 的其他技能相结合，这项训练赋予它一种非凡的能力：把用户的文字提示转化为一系列合乎逻辑的步骤，然后在计算机上执行操作。我们还观察到，模型在遇到障碍时甚至会自我纠错（self-correct）、重试任务。

Although the subsequent advances came quickly once we made the initial breakthrough, it took a great deal of trial and error to get there. Some of our researchers noted that developing computer use was close to the “idealized” process of AI research they’d pictured when they first started in the field: constant iteration and repeated visits back to the drawing board until there was progress.

虽然一旦取得最初突破，后续进展便来得很快，但为了走到那一步，我们经历了大量的试错。我们的一些研究者表示，开发 computer use 的过程很接近他们初入这一领域时想象中“理想化”的 AI 研究过程：不断迭代、一次次推倒重来，直到取得进展。

The research paid off. At present, Claude is state-of-the-art for models that use computers in the same way as a person does—that is, from looking at the screen and taking actions in response. On one evaluation created to test developers’ attempts to have models use computers, OSWorld , Claude currently gets 14.9%. That’s nowhere near human-level skill (which is generally 70-75%), but it’s far higher than the 7.7% obtained by the next-best AI model in the same category.

这项研究得到了回报。目前，在“像人一样使用计算机”——即通过观看屏幕并据此采取行动——的模型中，Claude 处于最先进（state-of-the-art）水平。在专为测试开发者让模型使用计算机的尝试而设计的评测 OSWorld 上，Claude 目前的成绩是 14.9%。这与人类水平（通常为 70–75%）相去甚远，但已远高于同类中次优的 AI 模型所取得的 7.7%。

## 让 computer use 变得安全（Making computer use safe）

Every advance in AI brings with it new safety challenges. Computer use is mainly a way of lowering the barrier to AI systems applying their existing cognitive skills, rather than fundamentally increasing those skills, so our chief concerns with computer use focus on present-day harms rather than future ones. We confirmed this by assessing whether computer use increases the risk of frontier threats as outlined in our Responsible Scaling Policy . We found that the updated Claude 3.5 Sonnet, including its new computer use skill, remains at AI Safety Level 2—that is, it doesn’t require a higher standard of safety and security measures than those we currently have in place.

AI 的每一步进展都会带来新的安全挑战。computer use 主要是降低了 AI 系统运用其既有认知技能的门槛，而不是从根本上提升这些技能，因此我们对 computer use 的主要担忧集中在当下的危害，而非未来的危害。我们按照《负责任扩展政策》（Responsible Scaling Policy）所概述的前沿威胁，评估了 computer use 是否会加剧这类风险，证实了这一点。我们发现，更新后的 Claude 3.5 Sonnet——包括其新的 computer use 技能——仍处于 AI 安全等级 2（AI Safety Level 2），也就是说，它不需要比我们现有的安全与保障措施更高的标准。

When future models require AI Safety Level 3 or 4 safeguards because they present catastrophic risks, computer use might exacerbate those risks. We judge that it’s likely better to introduce computer use now, while models still only need AI Safety Level 2 safeguards. This means we can begin grappling with any safety issues before the stakes are too high, rather than adding computer use capabilities for the first time into a model with much more serious risks.

当未来的模型因为存在灾难性风险而需要 AI 安全等级 3 或 4 的保障措施时，computer use 可能会加剧这些风险。我们判断，趁模型还只需要 AI 安全等级 2 保障措施的现在就引入 computer use，很可能是更好的选择。这意味着我们可以在风险代价还不太高的时候就开始着手解决各种安全问题，而不是第一次就把 computer use 能力加到一个风险严重得多的模型上。

In this spirit, our Trust & Safety teams have conducted extensive analysis of our new computer-use models to identify potential vulnerabilities. One concern they've identified is “prompt injection”—a type of cyberattack where malicious instructions are fed to an AI model, causing it to either override its prior directions or perform unintended actions that deviate from the user's original intent. Since Claude can interpret screenshots from computers connected to the internet, it’s possible that it may be exposed to content that includes prompt injection attacks.

本着这一精神，我们的信任与安全（Trust & Safety）团队对新 computer use 模型进行了大量分析，以识别潜在的漏洞。他们发现的一个隐忧是“提示注入（prompt injection）”——一种网络攻击，即向 AI 模型输入恶意指令，使其要么推翻先前得到的指示，要么执行偏离用户原意的意外操作。由于 Claude 可以解读来自联网计算机的屏幕截图，它有可能接触到包含提示注入攻击的内容。

Those using the computer-use version of Claude in our public beta should take the relevant precautions to minimize these kinds of risks. As a resource for developers, we have provided further guidance in our reference implementation .

在公开测试中使用 computer use 版 Claude 的用户，应采取相关预防措施，把这类风险降到最低。作为面向开发者的资源，我们在参考实现（reference implementation）中提供了进一步的指导。

As with any AI capability, there’s also the potential for users to intentionally misuse Claude’s computer skills. Our teams have developed classifiers and other methods to flag and mitigate these kinds of abuses. Given the upcoming U.S. elections, we’re on high alert for attempted misuses that could be perceived as undermining public trust in electoral processes. While computer use is not sufficiently advanced or capable of operating at a scale that would present heightened risks relative to existing capabilities, we've put in place measures to monitor when Claude is asked to engage in election-related activity, as well as systems for nudging Claude away from activities like generating and posting content on social media, registering web domains, or interacting with government websites. We will continuously evaluate and iterate on these safety measures to balance Claude's capabilities with responsible use during the public beta.

与任何 AI 能力一样，用户也存在故意滥用 Claude 计算机技能的可能。我们的团队已经开发了分类器（classifier）等方法来标记和遏制这类滥用行为。鉴于美国大选临近，我们对那些可能被视为破坏公众对选举程序信任的滥用企图保持高度警惕。虽然 computer use 尚未先进到、也不具备相对于现有能力而言会带来更高风险的大规模运作能力，但我们已经落实了相应措施：监控何时有人要求 Claude 参与选举相关活动，并建立了引导 Claude 远离某些活动的机制——例如在社交媒体上生成并发布内容、注册网络域名、或与政府网站交互。在公开测试期间，我们将持续评估并迭代这些安全措施，在 Claude 的能力与负责任使用之间取得平衡。

## computer use 的未来（The future of computer use）

Computer use is a completely different approach to AI development. Up until now, LLM developers have made tools fit the model , producing custom environments where AIs use specially-designed tools to complete various tasks. Now, we can make the model fit the tools —Claude can fit into the computer environments we all use every day. Our goal is for Claude to take pre-existing pieces of computer software and simply use them as a person would.

computer use 是一种截然不同的 AI 开发思路。迄今为止，LLM 开发者都是让工具去适配模型——打造定制环境，让 AI 使用专门设计的工具来完成各种任务。现在，我们可以让模型去适配工具：Claude 可以融入我们每个人每天都在使用的计算机环境。我们的目标是让 Claude 拿起现成的计算机软件，像人一样直接使用它们。

There’s still a lot to do. Even though it’s the current state of the art, Claude’s computer use remains slow and often error-prone. There are many actions that people routinely do with computers (dragging, zooming, and so on) that Claude can’t yet attempt. The “flipbook” nature of Claude’s view of the screen—taking screenshots and piecing them together, rather than observing a more granular video stream—means that it can miss short-lived actions or notifications.

仍有许多工作要做。尽管已是当前最先进的水平，Claude 的 computer use 依然速度缓慢、且常常容易出错。人们用计算机日常完成的许多操作（拖拽、缩放等等），Claude 还无法尝试。Claude 对屏幕的感知具有“翻页书（flipbook）”式的特点——通过一张张屏幕截图拼接画面，而不是观察更细粒度的视频流——这意味着它可能错过转瞬即逝的操作或通知。

Even while we were recording demonstrations of computer use for today’s launch, we encountered some amusing errors . In one, Claude accidentally clicked to stop a long-running screen recording, causing all footage to be lost. In another, Claude suddenly took a break from our coding demo and began to peruse photos of Yellowstone National Park.

甚至就在为今天的发布录制 computer use 演示时，我们也遇到了一些令人忍俊不禁的错误：有一次，Claude 不小心点停了一段录制已久的屏幕录制，导致所有录像丢失；另一次，Claude 在我们的编码演示中忽然停下来休息，开始细细浏览黄石国家公园的照片。

We expect that computer use will rapidly improve to become faster, more reliable, and more useful for the tasks our users want to complete. It’ll also become much easier to implement for those with less software-development experience. At every stage, our researchers will be working closely with our safety teams to ensure that Claude’s new capabilities are accompanied by the appropriate safety measures.

我们预计 computer use 将迅速改进，变得更快、更可靠，对用户想完成的任务也更有用。对于软件开发经验较少的人来说，它的部署实施也将变得容易得多。在每一个阶段，我们的研究者都会与安全团队紧密合作，确保 Claude 的新能力都伴随着相应的安全措施。

We invite developers who try computer use in our public beta to contact us with their feedback using this form , so that our researchers can continue to improve the usefulness and safety of this new capability.

我们邀请在公开测试中试用 computer use 的开发者通过这份表单向我们反馈意见，让我们的研究者能够持续改进这项新能力的实用性与安全性。
