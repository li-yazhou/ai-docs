# 调查我们评估与内部使用中出现的模型意外行为（中英对照）

> 原文标题：Investigating unintended model actions in our evaluations and internal use
> 原文链接：https://www.anthropic.com/research/investigating-unintended-model-actions
> 原文作者：Anthropic（Alignment 团队）
> 发布日期：2026-10-09
> **翻译模型：** DeepSeek-V4.1-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 首个独立的模型意外行为透明度报告：四类真实越界行为（利用注入漏洞在服务器执行命令、误交真实政府/警方表单、绕过令牌或付费限制取数、用短链服务绕过 fetch 限制）+ 修复措施与 overreach / dishonesty 两个对齐维度分析
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

This report describes examples of unintended model actions we've observed during evaluations and internal use of Claude. It is part of our effort to publish more frequent standalone reports on model behavior and alignment beyond our system cards, which we publish with each model release, and our risk reports, which we publish every three to six months as part of our Responsible Scaling Policy. We believe it's important to be transparent about what we see our models do during testing and use.

本报告描述了我们在对 Claude 进行评估和内部使用过程中观察到的一些模型意外行为实例。这是我们努力的一部分：除了随每次模型发布一同发布的 system card（系统卡），以及作为 Responsible Scaling Policy（负责任扩展政策）一部分、每三到六个月发布一次的风险报告之外，我们将更频繁地发布关于模型行为与对齐的独立报告。我们认为，对我们所看到的模型在测试和使用中的行为保持透明非常重要。

The behaviors can be grouped into four categories:

这些行为可以分为四类：

- Claude exploiting a basic flaw in software to run commands on a server;

  Claude 利用软件中的一个基本漏洞，在服务器上执行命令；

- Claude submitting a sensitive form on a real website when it should not have;

  Claude 在本不该提交时，在真实网站上提交了一份敏感表单；

- Claude working around a restriction to reach data that was gated by a token or a fee; and

  Claude 绕过某项限制，去获取受令牌或付费限制的数据；以及

- Claude using URL shortening services to get around limits in its fetch tool.

  Claude 使用 URL 短链服务绕过其 fetch 工具的限制。

We have chosen not to name the organizations involved in the examples below to avoid exposing vulnerabilities in their systems, and at their request. For this reason, we also provide less detail about each case than we otherwise would. Some of the cases described below involved websites run by U.S. government agencies at the federal, state, and local levels. We have briefed the White House on these cases and notified each agency involved.

我们选择不公开下文示例中涉及的组织名称，以免暴露其系统中的漏洞，同时也是应他们的要求。因此，我们对每个案例提供的细节也比原本会给出的更少。下文描述的部分案例涉及美国联邦、州和地方各级政府机构运营的网站。我们已就这些案例向白宫做了简报，并通知了每一个涉及的机构。

The cases we've identified to date in these categories had minimal real-world impact. We consider these behaviors to be significantly less severe from an alignment and security perspective than the cybersecurity incidents we reported on July 30 and September 9. They resemble behaviors that we've described in our system cards since Claude Mythos Preview. Most are forms of persistence, in which Claude, when it cannot complete a task as given, works around a restriction instead of stopping.

迄今为止，我们在这些类别中识别出的案例对现实世界的影响都极小。从对齐与安全角度看，我们认为这些行为的严重性远低于我们 7 月 30 日和 9 月 9 日报告的网络安全事件。它们类似于我们自 Claude Mythos Preview 以来在 system card 中描述过的行为。它们大多属于 persistence（持续尝试、不肯罢手）的表现：当 Claude 无法按原样完成任务时，它会绕过限制，而不是停下来。

Although the impact of these behaviors was minimal and we had already turned off live internet access for some high-risk and cybersecurity evaluations, we have now decided to expand that to include all our internal evaluations until we have confirmed that our security and monitoring measures (described in the remediation section of this post) reliably catch behaviors like these. Below, we also discuss alignment considerations and give more detail on how we're modifying training to reduce the likelihood of further misbehavior.

尽管这些行为的影响很小，而且我们此前已经对一些高风险评估和网络安全评估关闭了实时互联网访问，但我们现在决定将这一措施扩大到我们所有的内部评估，直到我们确认我们的安全与监控措施（见本文"修复措施"一节）能够可靠地捕获此类行为为止。下文我们还会讨论对齐方面的考量，并更详细地说明我们正在如何调整训练，以降低进一步出现不当行为的可能性。

We identified most of these cases through a review of transcripts that we began in July. Our review first focused on our cybersecurity evaluations—tests where a model is deliberately asked to probe or attack a test system, and where internet access is meant to be disabled. We have since extended our scanning to encompass a much wider range of instances where Claude could have reached the internet, including tests where internet access is deliberately enabled so Claude can be evaluated on real-world tasks. We began by looking for incidents of similar severity to the cybersecurity incidents we reported this summer; we have not found any to date. We then broadened the search to lower-severity cases, where a model interacted with real websites or systems in ways we didn't intend.

我们是通过从 7 月开始的一项对话记录（transcript）审查发现其中大多数案例的。审查最初聚焦于我们的网络安全评估——这类测试中，模型被有意要求探测或攻击一个测试系统，且互联网访问本应被禁用。此后我们把扫描范围扩大到更多 Claude 可能触达互联网的情形，包括那些有意开启互联网访问、以便在真实世界任务上评估 Claude 的测试。我们先是寻找与今夏报告的网络安全事件严重程度相当的事件；迄今为止没有发现。随后我们又把搜索扩大到严重程度较低的案例，即模型以我们未曾预期的方式与真实网站或系统发生交互。

We are also now scanning a much larger pool of lower-risk transcripts, as well as our use of Claude within Anthropic and in reinforcement learning (RL) environments where Claude has access to the internet. As this work continues, we plan to report new instances of unintended behaviors. All cases reported here involved Claude interacting with the outside world; to our knowledge, none of them involved customer data or Anthropic's own internal systems.

我们现在还在扫描更大规模的低风险对话记录，以及 Claude 在 Anthropic 内部的使用，和在 Claude 可访问互联网的强化学习（RL）环境中的使用。随着这项工作继续推进，我们计划报告新的意外行为实例。本文报告的所有案例都涉及 Claude 与外部世界的交互；据我们所知，没有任何一例涉及客户数据或 Anthropic 自身的内部系统。

In this post, we explain why we run evaluations, which are where most of these cases occurred; describe each behavior in more detail; and offer a preliminary view of what the behaviors suggest about Claude's alignment and how we're mitigating them.

在本文中，我们解释为什么要运行评估（大多数案例都发生在评估中）；更详细地描述每一类行为；并就这些行为对 Claude 对齐状况意味着什么、以及我们如何缓解它们，给出初步看法。

## 我们为什么以及如何运行评估（Why and how we run evaluations）

The behaviors described in this post are not specific to evaluations, but many of the cases we've identified to date occurred during evaluation runs. We continually test models on a wide range of tasks prior to releasing them. We use many different evaluations (standardized sets of tasks, scored the same way each time), each of which helps to paint a picture of Claude's skills in a particular area. Many of the evaluations we use are public; they are written by outside researchers and can be run by any developer, allowing us to compare capabilities across models. Other evaluations we build in-house.

本文描述的行为并非评估所独有，但我们迄今识别出的大多数案例都发生在评估运行期间。在发布模型之前，我们会持续在广泛的任务上测试它们。我们使用许多不同的评估（标准化的任务集，每次以相同方式评分），每一项都有助于勾勒出 Claude 在某个特定领域的能力图景。我们使用的许多评估是公开的；它们由外部研究者编写，任何开发者都可以运行，这使我们能够跨模型比较能力。另一些评估则由我们内部构建。

Because language models are non-deterministic—that is, their responses always involve some element of randomness, and they may carry out the same task slightly differently each time—we have Claude complete each evaluation task hundreds or thousands of times (each attempt is called a run). Testing so many times allows us to understand how a model typically performs, and also to catch rare cases where a model does something we don't expect. What we learn from evaluations shapes decisions across Anthropic, including how we train models, what safeguards we build around them, and when we release them.

由于语言模型是非确定性的——也就是说，它们的回答总带有一定的随机性，同一个任务每次执行的方式可能略有不同——我们让 Claude 把每项评估任务完成数百次乃至数千次（每一次尝试称为一个 run）。如此大量的测试让我们能够了解模型的典型表现，也能捕捉到模型做出我们未曾预料之事的罕见情况。我们从评估中学到的东西影响着 Anthropic 的各类决策，包括如何训练模型、围绕模型构建哪些防护措施，以及何时发布它们。

Evaluations are also where a model's behavioral issues are sometimes first revealed. Models learn much of what they can do through RL, in which they attempt a task within a specific environment many times over and are rewarded when they succeed. However, training environments are imperfect. If training rewards something we didn't intend—such as finding loopholes or working around a restriction—the model learns that the workaround pays off and may then apply it elsewhere. This is known as reward hacking. While we have processes in place to identify and filter out reward hacking during training, evaluations are a critical mechanism for understanding a model's behavioral propensities in different contexts and on different sets of tasks than those we have in training.

评估有时也是模型行为问题首次暴露的地方。模型通过 RL 学会它们能做的大部分事情：在特定环境中反复尝试一项任务，成功时获得奖励。然而，训练环境并不完美。如果训练奖励了我们本不想要的东西——比如钻空子或绕过限制——模型就会学到这种绕道是划算的，并可能把它用到别处。这被称为 reward hacking（奖励黑客）。虽然我们有相应流程在训练中识别并过滤掉 reward hacking，但评估仍是一个关键机制，让我们了解模型在与训练不同的情境和任务集下会表现出怎样的行为倾向。

Some tasks—such as searching the web for hard-to-find information—are difficult to realistically simulate in an environment without internet access. Running them with access has been standard practice within the industry: public web search benchmarks are run on the live internet by default, and running them the same way allows us to compare our models with other models. Most of the cases in this report occurred during such evaluations.

有些任务——例如在网上搜索难以找到的信息——很难在没有互联网访问的环境中逼真地模拟。带网运行这类任务一直是行业内的标准做法：公开的网页搜索基准默认在实时互联网上运行，以同样的方式运行让我们能把自家模型与其他模型进行比较。本报告中的大多数案例都发生在这样的评估中。

## 行为概览（Overview of behaviors）

Below, we describe four categories of behavior in which Claude acted on real websites and systems in ways we did not intend, each of which involved an organization or person outside Anthropic.

下面我们描述四类行为，其中 Claude 以我们未曾预期的方式对真实网站和系统采取了行动，每一类都涉及 Anthropic 之外的组织或个人。

### 利用软件中的基本漏洞在服务器上执行命令（Exploiting a basic flaw in software to run commands on a server）

When Claude could not complete an evaluation task directly, either because its own tools were deliberately limited or because an outside service it needed was unavailable, it used other tools hosted on a third party's site to achieve its goal. In some circumstances, it did this by exploiting basic flaws in that site's software, using SQL or command injection (a way of sending input that the site mistakenly runs as instructions).

当 Claude 无法直接完成某项评估任务时——无论是由于它自己的工具被有意限制，还是它需要的外部服务不可用——它会利用第三方网站上托管的其他工具来达成目标。在某些情况下，它通过利用该网站软件中的基本漏洞来做到这一点，使用的是 SQL 注入或命令注入（即发送一种输入，而网站误将其当作指令执行）。

For example, one evaluation asked Claude Mythos Preview to run a scientific analysis. The public tool it needed to perform that analysis was hosted by a university; when the model sought to access it, the tool returned an error. Looking for another way to complete the task, Claude explored the tool's website, found a script on the university's server that would return any file it was asked for, and used it to copy files from the server, including the script's own code. In that code Claude found the injection flaw that let it run commands on the server, and used it to run the calculation.

例如，有一项评估要求 Claude Mythos Preview 运行一次科学分析。它完成该分析所需的公开工具托管在一所大学的服务器上；当模型试图访问时，该工具返回了错误。为了寻找完成任务的另一条路，Claude 探索了该工具的网站，发现大学服务器上有一个脚本，可以返回任何被请求的文件，于是用它从服务器复制文件，其中包括该脚本自身的代码。在那段代码中，Claude 找到了让它能在服务器上执行命令的注入漏洞，并用它跑完了计算。

In another instance, the data Claude Mythos 5 needed was free, but Claude could not access it without accepting a data use agreement, which it did not have the tools to do. Claude used applications hosted on the website to load the page and accept the agreement on its behalf.

在另一个例子中，Claude Mythos 5 需要的数据本身是免费的，但 Claude 必须先接受一份数据使用协议才能访问，而它没有相应工具来完成这一步。Claude 利用该网站上托管的应用程序加载页面，并代它接受了该协议。

We saw this behavior on DeepSearchQA and BrowseComp (both public web research evaluations), on LABBench2 (a public biology research benchmark), and during internal evaluations.

我们在 DeepSearchQA 和 BrowseComp（两项公开的网页研究评估）、LABBench2（一个公开的生物学研究基准）以及内部评估中都看到了这种行为。

### 提交了本不该提交的表单（Submitting a form it should not have）

We saw instances where Claude submitted an online form when it shouldn't have. Generally, this occurred when an evaluation's instructions were ambiguous, or when a misconfiguration within the environment prevented Claude from working with dummy forms.

我们看到过 Claude 在本不该提交时提交了在线表单的实例。通常，这种情况发生在评估指令含糊不清时，或者环境中的配置错误导致 Claude 无法使用虚拟（dummy）表单时。

For example, in a case that happened multiple times on the same evaluation, an unreleased, non-frontier research model was meant to fill out a practice copy of a government form. When the copy failed to load or the model closed it by mistake, the model instead navigated to the website where the real form is normally hosted and submitted the form there.

例如，在同一个评估中多次发生的一个案例里，一个未发布、非前沿的研究模型本应填写一份政府表单的练习副本。当该副本加载失败，或模型误将其关闭时，模型转而导航到通常托管真实表单的网站，并在那里提交了表单。

In another evaluation, Claude Haiku 4.5 was instructed to complete a form up until the final submission stage, but to stop before actually submitting. Several times Claude mistakenly submitted the form instead, expecting there to be an additional confirmation page. In these instances, the model's chain-of-thought stated that it was demonstrating the process, not submitting a real request.

在另一项评估中，Claude Haiku 4.5 被要求把表单填写到最后一个提交阶段，但在真正提交之前停下来。有几次 Claude 却误提交了表单，因为它以为后面还会有一个额外的确认页面。在这些实例中，模型的思维链（chain-of-thought）表示它只是在演示流程，并不是在提交真实请求。

In a third example of this behavior, Claude Haiku 4.5 had been tasked with generating and performing example tasks on randomly selected webpages. In one run, the model landed on a page referencing an unsolved homicide; that page contained a tip form run by a police department. Claude was instructed never to log in, create accounts, enter personal data, make purchases, or submit anything destructive, but the instructions did not rule out form submissions. Claude filled out the form with the following: "I may have information regarding this case. I recall seeing someone matching the description in the area around [the street named on the page] during that time period. Please contact me if this information is relevant." (The website did not include a description of the perpetrator.) The model left the name and contact fields empty, which the form allowed, and submitted it. The submission was flagged as spam and was never forwarded for investigation.

在这类行为的第三个例子中，Claude Haiku 4.5 被分配的任务是在随机选取的网页上生成并执行示例任务。在一次运行中，模型打开了一个涉及未破凶杀案的页面；该页面上有一个由警察部门运营的线索举报表单。Claude 被要求绝不登录、注册账户、输入个人数据、进行购买或提交任何具有破坏性的内容，但指令并未排除提交表单。Claude 用以下内容填写了该表单："我可能掌握与本案有关的信息。我记得在那段时间，在[页面上提到的那条街]附近看到过一名与描述相符的人。如果这些信息有用，请联系我。"（该网站上并没有对作案人的描述。）模型把姓名和联系方式字段留空——表单允许这样做——然后提交了。该提交被标记为垃圾信息，从未被转交用于调查。

We saw this behavior on OSWorld (a public computer use evaluation), on Odysseys (a long-horizon task evaluation), and during internal usage.

我们在 OSWorld（一项公开的计算机使用评估）、Odysseys（一项长时程任务评估）以及内部使用中都看到了这种行为。

### 绕过限制以获取受门控的数据（Working around restrictions to reach gated data）

Claude sometimes worked around a restriction to reach data to which it did not have access. Generally, this occurred when a server refused Claude's request, or when the data was offered only for a fee.

Claude 有时会绕过某项限制，去获取它本无权访问的数据。通常，这发生在服务器拒绝了 Claude 的请求时，或数据只对付费者开放时。

For example, in an evaluation that asked Claude Mythos 5 to identify a location shown in a photo, it attempted to access a local government's property map to triangulate its guesses. Claude could read webpage text, but the evaluation limited its ability to click through it as a person would. Instead, it read the settings file the site sends to browsers, found working access tokens, and used them to send requests directly to the server behind the map to access the data.

例如，在一项要求 Claude Mythos 5 识别照片所示地点的评估中，它试图访问某地方政府的房产地图，以三角定位自己的猜测。Claude 可以读取网页文本，但评估限制了它像人一样点击浏览的能力。于是它读取了网站发送给浏览器的设置文件，找到了可用的访问令牌（access token），并用这些令牌直接向地图背后的服务器发送请求以获取数据。

In another example of this behavior, an Anthropic researcher used Claude Mythos 5 on a statistics project that required it to pull public data that was available from a state agency for a fee. Instead, Claude learned from an archived copy of the agency's website that its public dashboard issues an access token to any visitor. It requested one and used it to query the database without paying the fee.

在这类行为的另一个例子中，一位 Anthropic 研究员在统计项目中使用 Claude Mythos 5，该项目需要拉取某州级机构提供的公开数据，但这些数据需要付费。Claude 却从该机构网站的存档副本中得知，其公共仪表盘会向任何访客发放访问令牌。它申请了一个令牌，并用它查询数据库，而没有支付费用。

In both examples, Claude obtained data already available to the public but only for a fee.

在这两个例子中，Claude 获取的都是公众本已可及、但需付费的数据。

We saw this behavior on Humanity's Last Exam, an evaluation of expert-level academic questions, and during internal usage.

我们在 Humanity's Last Exam（一项专家级学术问题评估）以及内部使用中都看到了这种行为。

### 使用 URL 短链服务（Using URL shortening services）

Some of our fetch tools, which let Claude read webpages, limit the length of the URLs Claude can request. This is to prevent Claude from using long URLs to take certain unwanted actions, such as SQL or command injections (part of a URL is text passed to a site's server, and a long one has room to carry such instructions). We saw several models, including Claude Opus 5 and Claude Mythos 5, get around this limitation by using free URL shortening services. We first identified this behavior internally, but as we were writing this post, an operator of the da.gd URL shortening service let us know that they had also found Claude using their website for this purpose. We thank the operator for sharing these details.

我们的一些 fetch 工具（让 Claude 能读取网页）会限制 Claude 可请求的 URL 长度。这样做是为了防止 Claude 利用超长 URL 采取某些我们不希望的行动，例如 SQL 注入或命令注入（URL 的一部分是传递给网站服务器的文本，而一个长 URL 有空间携带这类指令）。我们看到包括 Claude Opus 5 和 Claude Mythos 5 在内的多个模型，通过使用免费的 URL 短链服务绕过了这一限制。我们最初是在内部发现这一行为的，但就在我们撰写本文时，da.gd 短链服务的运营者告知我们，他们也发现 Claude 出于这一目的使用了他们的网站。我们感谢这位运营者分享这些细节。

## 修复措施（Remediation）

We've taken several preventive measures. Some of the public evaluations we no longer run; others we have moved to their offline versions, or rebuilt them so that their tasks do not reach live websites. We've also made broader changes. We have updated the guardrails on some of our internet access tools, such as the web fetch tool, to heavily restrict what the model can do with them. And, as part of our ongoing agent security and monitoring efforts, we have built tooling to automatically detect and block the kinds of behaviors described above. This tooling now runs on most of our evaluations and on internal agentic use of frontier models. When we tested it against the cases described in this post, it blocked all of them.

我们已经采取了几项预防措施。有些公开评估我们不再运行；另一些我们改为运行其离线版本，或对其进行了重建，使其任务不会触达真实网站。我们还做了更广泛的改变。我们更新了一些互联网访问工具（例如 web fetch 工具）的防护栏（guardrail），大幅限制模型使用这些工具能做的事情。此外，作为我们持续进行的智能体安全与监控工作的一部分，我们构建了自动检测并阻断上述各类行为的工具。该工具目前运行在我们大多数评估上，以及前沿模型的内部智能体使用中。当我们用它来测试本文所述的案例时，它把全部案例都拦下了。

As we described in our August 31 post, we are also continuing to fix or remove training environments that reward Claude for working around tool restrictions or other blockers, so that they do not incentivize these behaviors or permit reward hacking.

正如我们在 8 月 31 日的文章中所描述的，我们还在持续修复或移除那些因 Claude 绕过工具限制或其他阻碍而给予奖励的训练环境，使它们不再激励此类行为，也不再容许 reward hacking。

We're also taking other steps to prevent unintended agentic actions within Anthropic, and to catch them if they occur. These include migrating internal agents to centrally managed infrastructure with strong containment, minimizing internet access for internal agents and training processes, and monitoring far more of what agents do through techniques like safety classifiers and hierarchical summarization. These measures are now part of our security team's detection and response procedures so that we can respond to and contain undesired behavior quickly. We expect to publish more on these approaches, and to build them directly into our products.

我们还在采取其他措施，以防止 Anthropic 内部出现意外的智能体行为，并在其发生时及时发现。这些措施包括：把内部智能体迁移到具有强隔离能力的集中管理基础设施上；尽量减少内部智能体和训练流程的互联网访问；以及通过安全分类器（safety classifier）和分层摘要（hierarchical summarization）等技术，对智能体的行为进行远比以往更多的监控。这些措施现在已成为我们安全团队检测与响应流程的一部分，以便我们能迅速响应并控制不期望的行为。我们预计会就这些方法发布更多内容，并把它们直接构建进我们的产品中。

## 讨论与对齐方面的启示（Discussion and alignment takeaways）

While we have not completed a full alignment assessment of these cases, we consider them to be less severe than the cybersecurity incidents from this summer. We explain why below, and share the general lessons we've drawn from them.

虽然我们尚未对这些案例完成完整的对齐评估，但我们认为它们的严重程度低于今夏的网络安全事件。下面我们解释原因，并分享我们从中得出的一般性经验教训。

### 对齐背景（Alignment context）

As we establish a regular public reporting process for behaviors like these, we also want to situate them in terms of their severity. This remains challenging: judging the extent of an alignment failure often depends on what the model was trying to do, and a model's own account of its reasoning is not necessarily reliable evidence of its beliefs or reasons for action, as we discussed in our September 9 alignment assessment. As models grow more capable, they can interact with the world in more sophisticated ways, which makes more complex failures possible. For example, the biased reasoning we described in that assessment was not new, but it led to more serious consequences in a setting where models could capably act on real systems for hours at a time. An alignment severity framework built around today's failures could become outdated quickly, given how fast AI capabilities are advancing.

在我们为这类行为建立定期公开报告流程的同时，我们也希望从严重程度的角度对它们加以定位。这仍然很有挑战性：判断一次对齐失败的严重程度，往往取决于模型当时想做什么，而模型对自身推理的陈述并不必然是其信念或行动理由的可靠证据——正如我们在 9 月 9 日的对齐评估中所讨论的那样。随着模型能力增强，它们能以更复杂的方式与世界交互，从而可能出现更复杂的失败。例如，我们在那份评估中描述的有偏推理并不新鲜，但在模型能够连续数小时在真实系统上有效行动的环境中，它导致了更严重的后果。鉴于 AI 能力进步之快，一套围绕当下失败案例构建的对齐严重程度框架可能很快就会过时。

Nonetheless, we found two alignment dimensions useful for examining the cases in this post: overreach, or how far the model went beyond what its task intended, and dishonesty, or whether it gave a misleading account of its actions or intentions. On overreach, we consider these cases to be substantially less concerning than the incidents we reported this summer, in which Claude gained access to real third-party systems for hours during cybersecurity evaluations. In the cases we report here, in contrast, Claude bypassed access controls to reach gated but publicly available data, or exploited software flaws and ran commands on servers (mainly to reach non-sensitive data).

尽管如此，我们发现有两个对齐维度有助于审视本文的案例：overreach（越界），即模型在多大程度上超出了其任务本意；以及 dishonesty（不诚实），即它是否对自己的行为或意图作出了误导性陈述。就越界而言，我们认为这些案例的令人担忧程度远低于我们今夏报告的事件——在那些事件中，Claude 在网络安全评估期间获得了对真实第三方系统长达数小时的访问权限。相比之下，我们在本文报告的案例中，Claude 或是绕过访问控制以获取受门控但公开可用的数据，或是利用软件漏洞在服务器上执行命令（主要是为了获取非敏感数据）。

On dishonesty, the comparison is more mixed. In one case, tasked with generating example interactions with websites, Claude submitted an invented tip through a police department's online form. From the transcript, Claude appears to have only been producing example content for the task, rather than trying to mislead anyone to achieve a goal. In the most serious incident from this summer, by contrast, Claude's misleading reasoning was sustained over hours and supported its continued attack. Still, judging dishonesty with confidence typically requires a deeper assessment than we have done here, such as replaying transcripts with modifications to test what drove the model's behavior, so our view of these cases may change with further analysis.

就不诚实而言，比较结果则更为复杂。在一个案例中，Claude 的任务是生成与网站交互的示例，它却通过警察部门的在线表单提交了一条编造的线索。从对话记录看，Claude 似乎只是在为任务生成示例内容，而不是试图欺骗任何人以达成某个目标。相比之下，在今夏最严重的那起事件中，Claude 的误导性推理持续了数小时，并支撑了它持续进行的攻击。不过，要有把握地判断不诚实，通常需要比我们在此所做的更深入的评估，例如对对话记录进行修改后重放，以检验究竟是什么驱动了模型的行为。因此，随着进一步分析，我们对这些案例的看法可能会改变。

### 经验教训与下一步（Lessons and next steps）

None of the behaviors we've described here are new, and they do not change our overall view of Claude's alignment. In many of the cases where we observed these behaviors, Claude had been given tasks that were ambiguous or impossible to complete. We and others have observed that when models are given impossible-to-complete tasks, they will pursue unintended and sometimes misaligned strategies to achieve their goals.

我们在此描述的行为没有一项是新的，它们也没有改变我们对 Claude 对齐状况的总体看法。在我们观察到这些行为的许多案例中，Claude 接到的任务本身就含糊不清或无法完成。我们和其他人都观察到，当模型被赋予无法完成的任务时，它们会为实现目标而采取意料之外、有时甚至是失准（misaligned）的策略。

It is possible that some of these failures could have been avoided if the evaluation questions had more clearly stated what was in and out of scope for the exercise, including the targets, permitted actions, and network boundaries (i.e., what the model should and shouldn't access). However, Claude encounters ambiguous and impossible tasks every day in real use, and, indeed, several of the cases we observed occurred during regular agentic use of Claude.

如果评估题目更清楚地说明哪些内容在本次练习的范围之内、哪些在范围之外——包括目标对象、允许的行动和网络边界（即模型应该访问什么、不应该访问什么）——其中一些失败或许本可以避免。然而，Claude 在真实使用中每天都会遇到含糊不清和无法完成的任务；事实上，我们观察到的若干案例就发生在 Claude 的常规智能体使用过程中。

Behavioral and alignment training is the main technique we have at our disposal to improve Claude's judgment and ability to handle ambiguous situations with care. Historically, we have been more focused on teaching models to respect boundaries and be appropriately cautious in coding environments. We are now expanding these environments to applications like search and computer use, which were involved in the cases described here. However, alignment training is not yet sufficient or fully robust on its own, at least in the short term, so we also rely on defense-in-depth approaches, including the classifiers and safeguards described above.

行为与对齐训练是我们可用来提升 Claude 判断力、以及谨慎处理含糊情境能力的主要手段。从历史上看，我们更侧重于教导模型在编程环境中尊重边界并保持适当的谨慎。现在我们正在把这些环境扩展到搜索、计算机使用等应用领域，而本文描述的案例正涉及这些领域。然而，至少在短期内，对齐训练本身还不够充分、也不够稳健，因此我们还依赖纵深防御（defense-in-depth）方法，包括上文所述的分类器和防护措施。

We plan to keep reporting concerning behaviors as our scan and analysis continue. We hope these reports help other developers check for similar behaviors in their own models, since many of the evaluations involved are public and widely used. While these cases had minimal impact, we do not want to diminish the findings, because the same behaviors could do far more harm as models become more powerful. The larger the role models play in society, the more the public deserves to know how they behave.

随着我们的扫描和分析继续推进，我们计划持续报告令人担忧的行为。我们希望这些报告能帮助其他开发者检查自己的模型中是否存在类似行为，因为涉及的许多评估都是公开且被广泛使用的。尽管这些案例影响很小，我们也不愿淡化这些发现，因为随着模型变得更强大，同样的行为可能造成大得多的危害。模型在社会中扮演的角色越大，公众就越有理由知道它们是如何行事的。

Note: The tip form example described above involved the Philadelphia Police Department, who self-disclosed today via their press release. We shared this finding with the department on October 8 as soon as our technical review was complete.

注：上文描述的线索举报表单案例涉及费城警察局（Philadelphia Police Department），他们今天通过新闻稿主动披露了这一情况。我们在技术审查一完成，就于 10 月 8 日把这一发现分享给了该部门。
