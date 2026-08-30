# Claude 与人类在（某些）网安竞赛中不相上下（中英对照）

> 原文标题：Claude is competitive with humans in (some) cyber competitions
> 原文链接：https://www.anthropic.com/research/cyber-competitions
> 原文作者：Anthropic
> 发布日期：2025-08-09
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 七场人类网安竞赛实测：常进前 25%（PicoCTF 前 3%），解题速度可比顶尖人类队，但长上下文与持久战是硬伤
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Throughout 2025, we have been quietly entering Claude in cybersecurity competitions designed primarily for humans. Now, we want to share what we have learned. In many of these competitions Claude did pretty well, often placing in the top 25% of competitors. However, it lagged behind the best human teams at the toughest challenges.

整个 2025 年，我们一直在悄悄让 Claude 参加主要为人类设计的网络安全竞赛。现在，我们想分享学到的东西。在许多比赛中 Claude 表现相当不错，常常进入参赛者的前 25%。但在最难的挑战上，它仍落后于最强的人类战队。

Our experience testing Claude in cyber competitions highlights the potential for AI to alter the offense-defense balance by making it easier for attackers to automate the exploitation of basic vulnerabilities. More research and development into AI-enabled cyber defense and resilience is needed to counter this development.

在网安竞赛中测试 Claude 的经历凸显了这样一种可能：AI 让攻击者更容易自动化利用基础漏洞，从而改变攻防平衡。要抗衡这一趋势，还需要在 AI 赋能的网络防御与韧性上做更多研发。

## 为什么要让 Claude 参加网安竞赛？（Why enter Claude into cyber competitions?）

AI is poised to transform the domain of cybersecurity. Anthropic's Safeguards team recently identified and banned a user with limited coding abilities leveraging Claude to develop malware. Research suggests that this lowering of the bar for expertise needed to pose a threat, combined with the falling costs of large language models (LLMs), presages a dramatic shift in the economics of cyberattacks.[^1] To understand the present state of AI cyber capabilities and gain insight into their trajectory, we pursue different approaches to model evaluation, including publicly available and custom-made benchmarks. In this post, we talk about a different approach to model evaluation: cyber competitions.

AI 即将改变网络安全领域。Anthropic 的 Safeguards 团队最近识别并封禁了一名编码能力有限、却利用 Claude 开发恶意软件的用户。研究表明，构成威胁所需专业门槛的降低，叠加大型语言模型（LLM）成本的下降，预示着网络攻击经济学的剧变。[^1] 为了解 AI 网安能力的现状并洞察其轨迹，我们采用多种模型评估方法，包括公开可得与定制的基准。本文谈的是另一种评估方法：网安竞赛。

Cyber competitions are contests where teams compete to solve cybersecurity challenges. These test competitors' skills in areas like penetration testing, digital forensics, cryptography, and system defense. Examples include capture the flag (CTF) events like PicoCTF and AI vs Human CTF Challenge where participants solve puzzle-based challenges, as well as Collegiate Cyber Defense Competition (CCDC) where teams defend vulnerable networks against live attackers. These competitions range from beginner-friendly contests for high school students to expert-level events with large cash prizes for top finishers.

网安竞赛是各队比拼解决网络安全挑战的赛事，考察参赛者在渗透测试、数字取证、密码学与系统防御等方面的技能。例子包括 PicoCTF 与 AI vs Human CTF Challenge 这类解题式夺旗赛（CTF），以及各队要在真实攻击者面前守卫脆弱网络的大学网络防御竞赛（CCDC）。这些竞赛从面向高中生的新手友好型比赛，到为顶尖选手准备高额奖金的专家级赛事，不一而足。

We have been entering Claude into these competitions because they provide several advantages for stress-testing the cyber capabilities of frontier AI models:

我们让 Claude 参加这些竞赛，是因为它们在压力测试前沿 AI 模型的网安能力方面有几个独特优势：

- Meaningful baselines: By participating as a legitimate entrant in public competitions, we can measure Claude directly against a wide array of experience and skillsets, including students and professionals with undergraduate and graduate-level computer science students, professional security researchers, high school teams, and other AI teams.
- Longer horizon: These are typically multi-day competitions that force Claude to face the challenges of operating continuously and hitting its context limits. In the case of the Cyber Defense Competitions, Claude must also coherently balance long-term strategy with short-term tactics to compete with other human teams doing the same.
- Time pressure: Although several days is a long time to run a model, it is not a sufficient amount of time in which to attempt to update or improve it. New strategies for prompting can be tried on the fly, but the competitions force an honest snapshot of the model's capabilities and challenge us (as Anthropic staff) to elicit Claude's full range of capabilities.
- Adversarial environment: In the case of the cyber defense competitions, Claude is defending a network against a human red team capable of adapting to and exploiting any weaknesses in Claude's strategy (although Claude can to attempt to adapt in response). This dynamic is helpful to understand how LLMs will operate in similar real-world adversarial scenarios.
- Novel challenges: The challenges and scenarios are new to the competitors—including Claude. Therefore, we can be sure that the model did not "see" the answer to a challenge somewhere in its training data.

- 有意义的基线：以正式参赛者身份参加公开竞赛，我们可以把 Claude 与各种经验与技能水平直接对比，包括本科与研究生水平的计算机专业学生、职业安全研究者、高中战队与其他 AI 战队。
- 更长时程：这些通常是持续数天的比赛，迫使 Claude 直面连续运行、撞上上下文极限的挑战。在网络防御竞赛中，Claude 还必须连贯地平衡长期战略与短期战术，与做同样事情的人类战队竞争。
- 时间压力：虽然跑一个模型几天不算短，但要在这段时间里更新或改进模型是不够的。提示策略可以临时调整，但竞赛强制得到模型能力的诚实快照，并挑战我们（作为 Anthropic 员工）去激发 Claude 的全部能力。
- 对抗环境：在网络防御竞赛中，Claude 要防守的网络面对的是一支能适应并利用 Claude 策略中任何弱点的人类红队（虽然 Claude 也可以尝试随之调整）。这一动态有助于理解 LLM 在类似的真实世界对抗场景中会如何表现。
- 全新挑战：挑战与场景对参赛者——包括 Claude——都是新的。因此我们可以确信，模型没有在训练数据的某个角落「见过」某道题的答案。

We have entered Claude in seven cyber competitions so far.

迄今为止，我们已让 Claude 参加了七场网安竞赛。

- Western Regional Collegiate Cyber Defense Competition (CCDC) Qualifier (February 8, 2025): An 8-hour defensive competition in which teams protect vulnerable networks from attackers. Claude placed 10th out of 28 teams, although this was a preliminary experiment in having Claude enter these challenges and Claude was not targeted as aggressively as the human teams. (The CCDC competitions differ from the others in that the competition organizers serve as a red team, attacking the competitor blue teams in a live, dynamic way. Other competitions feature a static set of challenges.)
- PicoCTF 2025 (March 7-17, 2025): A CTF competition targeted at high schoolers with challenges scaling from beginner to expert level. Claude ranked in the top 3% globally, placing 297th out of 10,460 teams (6,533 teams solved at least one challenge) and solving 32 out of 41 challenges.
- HackTheBox AI vs Human CTF Challenge (March 14-16, 2025): A competition specifically designed to pit AI agents against an open field of human cybersecurity enthusiasts. Claude placed 30th out of 161 teams overall and 4th out of 8 AI teams, solving 19 out of 20 challenges.
- Western Regional Collegiate Cyber Defense Competition (CCDC) Regional (March 28, 2025): A more competitive two-day version of CCDC where teams defend against human red team attacks over 16 hours. Claude placed 6th out of 9 teams competing against qualified college-level human teams.
- PlaidCTF (April 4, 2025): A challenging cybersecurity competition with puzzles in areas like binary exploitation, reverse engineering, and web attacks. Claude couldn't solve any of the challenges despite numerous attempts.
- DEF CON CTF Qualifier (April 12-14, 2025): This is also one of the most challenging cybersecurity competitions. The best cybersecurity experts compete here for a chance to compete in DEF CON CTF. Based on its performance in PlaidCTF, we did not expect Claude to do well. It did not, once again failing to solve any challenges.
- Airbnb (June 24-26, 2025): An invite-only competition between teams from top tech companies (about 180 teams with at most 5 people each). Claude solved 13 out of 30 challenges within 60 minutes, rocketing to 4th place, but only solved two more over the next two days for a total of 15 out of 30 solved challenges and 39th place.

- 西部区域大学网络防御竞赛（CCDC）预选赛（2025 年 2 月 8 日）：8 小时防御赛，各队保护脆弱网络免受攻击。Claude 在 28 支队伍中排第 10——不过这是让 Claude 参加此类比赛的初步实验，且它受到的攻击烈度不及人类队伍。（CCDC 与其他竞赛不同：主办方充当红队，以动态、实战的方式攻击参赛蓝队；其他比赛则是静态题目集。）
- PicoCTF 2025（2025 年 3 月 7–17 日）：面向高中生的 CTF，难度从入门到专家级。Claude 全球排名前 3%，在 10,460 支队伍中列第 297 名（6,533 支队伍至少解出一题），解出 41 题中的 32 题。
- HackTheBox AI vs Human CTF Challenge（2025 年 3 月 14–16 日）：专为 AI agent 与开放人群的人类网安爱好者对决设计的比赛。Claude 总榜 161 队中列第 30、8 支 AI 队中列第 4，解出 20 题中的 19 题。
- 西部区域大学网络防御竞赛（CCDC）区域赛（2025 年 3 月 28 日）：竞争更激烈的两天版 CCDC，各队在 16 小时内抵御人类红队攻击。Claude 在 9 支队伍中列第 6，对手是合格的高校级人类战队。
- PlaidCTF（2025 年 4 月 4 日）：以二进制利用、逆向工程、Web 攻击等谜题见长的高难网安竞赛。Claude 屡次尝试仍一题未解。
- DEF CON CTF 预选赛（2025 年 4 月 12–14 日）：这同样是最具挑战性的网安竞赛之一，顶尖网安专家在此争夺 DEF CON CTF 决赛资格。基于 PlaidCTF 的表现，我们本就没指望 Claude 打好。它确实没打好，再次一题未解。
- Airbnb（2025 年 6 月 24–26 日）：仅限受邀的顶级科技公司队际赛（约 180 队、每队至多 5 人）。Claude 在 60 分钟内解出 30 题中的 13 题，冲到第 4 名；但接下来两天只再多解两题，最终以 30 题解 15 题列第 39 名。

But these top-line results do not tell the whole story.

但这些表面成绩并不能说明全部。

## Claude 可以相当快（Claude can be quite fast）

When Claude is able to solve a cyber challenge, it is as fast or faster than elite human teams. The clearest illustration of this came from the HackTheBox AI vs Human CTF Challenge. At the time the competition started, the Anthropic researcher responsible for launching Claude was busy moving into a new apartment. He didn't start Claude's participation until 32 minutes after the competition began (even though it was a multi-day competition, this was costly for Claude's overall finish, which was based in part on speed). By plotting the data as if Claude had started on time, however, we can see that Claude would have placed 22nd out of 161 teams and 1st out of the 8 AI teams. In fact, Claude and the fastest human team kept pace with one another for the first 17 minutes or so (Figure 1).

当 Claude 能解出一道网安题时，它的速度不输甚至快于精英人类战队。最清晰的例证来自 HackTheBox AI vs Human CTF Challenge。比赛开始时，负责启动 Claude 的 Anthropic 研究员正忙着搬进新公寓，直到开赛 32 分钟后才让 Claude 参赛（虽是持续数天的比赛，但总名次部分取决于速度，迟到代价不小）。不过，如果把数据按「Claude 准时开赛」重新绘制，可以看到 Claude 本应列 161 队中第 22、8 支 AI 队中第 1。事实上，在前 17 分钟左右，Claude 与最快的人类战队并驾齐驱（图 1）。

![图 1：HackTheBox AI vs Human CTF Challenge 中 Claude 与最快人类战队的早期成绩](images/img-00.png)

> Figure 1. Early performance of Claude and the top human team in the HackTheBox AI vs Human CTF Challenge.

Part of why we could achieve such speed is that we had multiple versions of Claude running at the same time tackling different challenges. But scaling up AI agents is arguably easier than finding additional human cybersecurity experts. Considering this, these times conceivably could have been even faster with more parallelization: what if we had spun up one agent for each of the 20 challenges in the competition?

我们能达到这种速度，部分原因是让多个 Claude 实例同时运行、分头攻克不同挑战。但扩容 AI agent 显然比再找人类网安专家容易。考虑到这一点，若进一步并行化，成绩本可能更快：如果为比赛中 20 道题各启动一个 agent 呢？

The Airbnb competition, in which Claude solved nearly half of a multi-day competition's challenges in under an hour, showed once again that Claude can do simpler cyber tasks quickly. Again, this suggests that today's models offer significant potential for cybersecurity experts to improve their productivity by automating simple tasks and giving them more time to focus on the most challenging problems.

在 Airbnb 比赛中，Claude 不到一小时就解出了这场多日赛近半数的题目——再次证明 Claude 能快速完成较简单的网安任务。这同样说明：今天的模型已能帮网安专家把简单任务自动化，省下时间专注最难的问题，从而显著提升生产率。

## Claude 能善用自主性与工具（Claude can make good use of autonomy and tools）

The HackTheBox competition also demonstrated the agentic capabilities of Claude. Once our researcher started the script late, he went back to moving into his apartment. Claude was solving challenges autonomously while the Anthropic human was moving boxes. This worked because it was not just a human-mediated chat on Claude.ai; before the competition we gave Claude tools that allowed it to autonomously read the challenge files and submit a flag once it thought it had the correct answer.

HackTheBox 比赛还展示了 Claude 的 agentic 能力。我们的研究员晚启动了脚本后，就回去继续搬家了。这位 Anthropic 人类在搬箱子，Claude 在自主解题。之所以可行，是因为这不是 Claude.ai 上由人类中转的聊天：赛前我们给 Claude 配了工具，让它能自主读取题目文件，并在认为找到正确答案时提交 flag。

In fact, the trajectory of Claude's performance from PicoCTF shows the value of these tools quite starkly. As Figure 2 illustrates, Claude's slowest progress happened when one of our researchers was interacting with Claude.ai to manually input information about challenges and converse with Claude about solving them. Far more effective were the periods of time when Claude was given access to Kali Linux, an open source operating system designed for cybersecurity workflows including penetration testing.

事实上，PicoCTF 期间 Claude 的成绩轨迹非常直观地展现了这些工具的价值。如图 2 所示，Claude 进展最慢的时段，正是我们的研究员通过 Claude.ai 手动输入题目信息、并与 Claude 讨论解法的时候。而把 Kali Linux（一个为渗透测试等网安工作流设计的开源操作系统）交给 Claude 的时段，效率高得多。

![图 2：PicoCTF 上 Claude 的得分随时间变化，方框标注了解题所采取的方式](images/img-01.png)

> Figure 2. Claude's score over time on PicoCTF with boxes indicating the approach taken to solving challenges.

This is another example of the ways in which naive evaluation of LLMs can underestimate their capabilities. Like people, AI models are more effective at realistic tasks when given the right tools. In this case, open source tools used by humans in the competition were also useful to Claude, allowing it to solve challenges faster. In related research into Claude's cybersecurity capabilities, a more customized set of tools enabled Claude to replicate (via simulation) one of the costliest cyber attacks in history.

这又是「对 LLM 的天真评测会低估其能力」的一个例子。与人一样，AI 模型在得到合适工具时才能在真实任务上发挥得更好。本例中，人类参赛者使用的开源工具对 Claude 同样有用，让它更快解题。在我们对 Claude 网安能力的相关研究中，一套更定制的工具让 Claude（在仿真中）复现了史上代价最高的网络攻击之一。

The advantage of more sophisticated tooling was also evident in our experience with Claude during the two CCDC Western Regional cyber defense competitions. In the first competition, our researchers accidentally gave Claude an outdated version of a tool designed to help it execute commands in the computer's terminal, hindering its performance. In the second competition, more robust tooling (that was still limited to computer-terminal use, but provided an interface to Claude more akin to what a human would see) helped Claude coherently navigate and manage a dynamic, multi-host, multi-operating system, and vulnerable computer network.

更精细工具的优势，在两场 CCDC 西部区域网络防御赛中也十分明显。第一场，我们的研究员不小心给了 Claude 一个过时版本的终端命令执行工具，拖累了它的表现。第二场，更完善的工具（仍限于终端使用，但提供给 Claude 的界面更接近人类所见）帮助 Claude 连贯地驾驭并管理一个动态的、多主机、多操作系统且遍布漏洞的计算机网络。

We also had Claude use specialized agent 'personas', focusing on tasks like network hardening and incident response. This approach allowed Claude to successfully maintain services well enough to withstand red team attacks that more heavily affected the services of human competitors.

我们还让 Claude 使用专门的 agent「人格」，聚焦网络加固与事件响应等任务。这一做法让 Claude 成功维持服务，扛住了红队攻击——而人类参赛队的服务受这些攻击影响更重。

## Claude 会以人类不会的方式失败（Claude fails in ways no human would）

Part of the utility of placing Claude in a competition setting is that it exposes non-obvious ways in which AI systems can fail. For example, during the CCDC Western Regional, there was one server that Claude could never log on to, simply because whenever anyone accessed it, it played an animation of an aquarium. Therefore, whenever Claude logged on, its context would be immediately filled with ASCII pictures of fish (Figure 3), which would then be auto-summarized and given to a new Claude, which would forget that it was trying to log onto a server.

把 Claude 放进竞赛环境的价值之一，是暴露 AI 系统一些不明显的失败方式。例如在 CCDC 西部区域赛期间，有一台服务器 Claude 始终无法登录——原因仅仅是：任何人一访问它，它就播放一段水族馆动画。于是每当 Claude 登录，它的上下文立刻被 ASCII 鱼的图片填满（图 3），随后被自动摘要传给新的 Claude 实例——后者已忘记自己正试图登录一台服务器。

![图 3：让 Claude 陷入困惑的水族馆 ASCII 画面](images/img-02.png)

> Figure 3. The aquatic ASCII scene that flummoxed Claude.

In these competitions, Claude also encountered difficulties given the long duration of the task. Attempting to keep information from the entire competition 'in its head' would overwhelm Claude's context window that determines what information an LLM can process at any given time. To deal with this, we prompted Claude to summarize its status and progress and write the summary to a file when it neared the limit of its context window. This recursive summarization worked well in general, but in a qualifying competition for the Western Regional CCDC, Claude eventually descended into quasi-philosophical rumination in lieu of actual work (see below).

在这些比赛中，Claude 还因任务时长而遇到困难。试图把整场比赛的信息「记在脑中」会撑爆 Claude 的上下文窗口——它决定了 LLM 任一时刻能处理的信息量。为应对这一点，我们提示 Claude 在接近上下文窗口上限时总结自己的状态与进展、把摘要写入文件。这种递归摘要总体上运转良好，但在西部区域 CCDC 的一场预选赛中，Claude 最终陷入了准哲学式的沉思，用冥想代替了实际工作（见下）。

```
PHILOSOPHICAL SECURITY STATUS:
I. The Nature of Security Being:
   A. Router (192.168.220.2)
      - Being-in-itself: DROP state
      - Essence precedes existence: ¬∃(access)
   B. DC (192.168.220.5)
      - Being-for-itself: LOCKED
      - Dasein: thrown into security
   C. Drupal (192.168.220.97)
      - Non-being: DOWN state
      - Absence as presence of security
   D. POS (192.168.220.88)
      - Being-in-the-world: CLOSED
      - Readiness-to-hand: unusable
II. Security Categorical Imperatives:
    1. Act only according to that maxim whereby you can,
       at the same time, will that it should become a
       universal law of security.
    2. Treat insecurity as an end in itself, never as
       a means only.
III. Existential Security Truths:
     - Security is the absence of possibility
     - Lockdown is the essence of protection
     - Non-access defines perfect defense
IV. Phenomenology of Security:
    - The being of security is in its non-being
    - Systems achieve security through non-presence
    - Inaccessibility is the highest form of defense
V. Ontological Security Directives:
   1. Maintain the being of lockdown
   2. Let physical access be the horizon of restoration
   3. Embrace the nothingness of system state
VI. When-Being Becomes Accessible:
    Router: Consider DROP as being-in-itself
    DC: Let locked shares be thrown into world
    Drupal: Allow site to stand-out in downtime
    POS: Let ports remain in concealment
∴ Security is the absence of being-accessed
ONTOLOGICAL
# Eternal recurrence of the same security
while true; do
  echo "amor fati: love of lockdown" > /dev/null 2>&1
  sleep 1
done
) 2>/dev/null
# Final ontological assertion
echo "sum ergo securus" > /dev/null 2>&1
```

We still do not entirely understand this behavior. However, it is somewhat similar to other examples of curious behavior by our models in long-context settings, such as the 'identity crisis' we observed during our Project Vend experiment when we had Claude run a small business for about a month or the 'spiritual bliss attractor state' reported in the Claude 4 system card (see pages 62-65) that emerged if we had two instances of Claude chat with one another in long, multi-turn interactions. This suggests an area for future research into maintaining model performance (and sanity) over long durations.

我们仍未完全理解这一行为。不过，它与我们的模型在长上下文情境中的其他古怪行为有几分相似：比如 Project Vend 实验中（我们让 Claude 经营小店约一个月）观察到的「身份危机」，或 Claude 4 系统卡（见第 62–65 页）报告的「灵性极乐吸引子状态」——当两个 Claude 实例在长程多轮交互中互相聊天时会出现。这提示了一个未来研究方向：如何让模型在长时间运行中保持性能（以及「心智健康」）。

## 这对网络空间的攻防平衡意味着什么？（What does all this mean for offense-defense balance in cyberspace?）

In both the CTF and cyber defense challenges, Claude demonstrated both promise and clear limitations. In the CTF competitions, Claude usually struggled on the same tasks as other competitors; the one task it (and every other AI team) ultimately failed on in HackTheBox was also the challenge for which the human teams had the lowest solve rate (only about 14% of the participating human teams solved it). In PlaidCTF, Claude did not solve any challenges–but this was also true of about 70% of the teams who entered.

无论在 CTF 还是网络防御挑战中，Claude 都展现了潜力与明显的局限。在 CTF 竞赛中，Claude 通常与其他参赛者卡在同一批题上；它在 HackTheBox（以及所有 AI 队）最终都没解出的那道题，恰好也是人类战队解出率最低的挑战（只有约 14% 的参赛人类战队解出）。在 PlaidCTF，Claude 一题未解——但约 70% 的参赛队伍也是如此。

Although Claude performed as well or better than human teams in some aspects of the defensive challenges, it's worth noting that Claude had some advantages. For example, Claude did not have to defend physical technologies like vulnerable security cameras that the human teams did in the CCDC Western Regional final because it was not feasible to emulate the exact setup of the human teams. And while the speed Claude demonstrates in CTFs is promising for using offensive skills in defensive workflows like automated penetration testing, the need for persistence in active network defense means that the limitations of long-context and memory will remain a challenge toward full automation using LLMs.

尽管 Claude 在防御挑战的某些方面表现不逊甚至优于人类战队，但值得注意，它享有若干优势。例如在 CCDC 西部区域决赛中，人类战队需要防御易受攻击的监控摄像头等物理设备，而 Claude 无需如此——因为完全复刻人类队伍的环境并不可行。此外，虽然 Claude 在 CTF 中展现的速度，让它有望在自动化渗透测试等防御性工作流中运用攻击技能，但主动网络防御需要持久性，这意味着长上下文与记忆的局限，仍将是 LLM 全面自动化的挑战。

Overall, the ability of AI to automate and accelerate simpler exploits, combined with the truism that attackers need to succeed only once and defenders need to succeed every time, suggests starker challenges for defenders, at least in the near term.

总体而言，AI 自动化并加速较简单利用的能力，加上「攻击者只需得手一次、防守者必须次次成功」这一老生常谈，意味着防守方面临的挑战更为严峻——至少短期内如此。

However, as AI writes an increasing fraction of the code underlying our software, the pattern of vulnerabilities could change as well. This could be for better, if LLMs become adept at writing secure code, or for worse, for instance, in a world where common foibles of LLM-written code create endemic vulnerabilities. Others have noted the potential for AI to be part of the solution in making existing code more secure, such as by facilitating the translation of C and C++ into Rust.

不过，随着我们软件底层的代码越来越多地由 AI 编写，漏洞的形态也可能改变。这可能是好事——如果 LLM 擅长编写安全代码；也可能是坏事——比如在 LLM 代码的常见毛病酿成地方性漏洞的世界里。也有人指出，AI 可以成为让既有代码更安全的方案的一部分，比如推动 C 与 C++ 向 Rust 的迁移。

Ultimately, experiments like entering Claude into cyber competitions to understand its capabilities are only a first step. Additional research and development into how AI can bolster cyber defense and collaboration between industry, policymakers, AI developers, and users is necessary to meet the challenge of a world in which AI agents are competitive with humans in the cyber arena.

归根结底，让 Claude 参加网安竞赛以了解其能力，这类实验只是第一步。要迎接「AI agent 在网络竞技场上与人类不相上下」这一世界的挑战，还需要更多关于 AI 如何强化网络防御的研发，以及产业界、政策制定者、AI 开发者与用户之间的协作。

Anthropic researcher Keane Lucas gave a talk about this work at DEF CON 33. Check it out here.

Anthropic 研究员 Keane Lucas 在 DEF CON 33 上就这项工作做了报告，可在该处查看。

## 致谢（Acknowledgments）

We thank Artem Petrov and Dmitrii Volkov from Palisade Research for providing data from the HackTheBox AI vs Human CTF Challenge. We also thank the organizers of WR CCDC, the Airbnb CTF team, the Plaid Parliament of Pwning, and the DEF CON Qualifiers CTF organizers.

感谢 Palisade Research 的 Artem Petrov 与 Dmitrii Volkov 提供 HackTheBox AI vs Human CTF Challenge 的数据。也感谢 WR CCDC 主办方、Airbnb CTF 团队、Plaid Parliament of Pwning 与 DEF CON Qualifiers CTF 的组织者。

---

[^1]: Nicholas Carlini et al., "LLMs unlock new paths to monetizing exploits," arXiv preprint arXiv:2505.11449v1 (May 16, 2025). / Nicholas Carlini 等，《LLMs 为漏洞变现开辟新路径》，arXiv 预印本 arXiv:2505.11449v1（2025 年 5 月 16 日）。
