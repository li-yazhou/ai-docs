# 把对话变成知识：Slack 如何构建人机协作团队（中英对照）

> 原文标题：Turning conversation into knowledge: how Slack builds human-agent teams
> 原文链接：https://claude.com/blog/turning-conversation-into-knowledge-how-slack-builds-human-agent-teams
> 原文作者：Anthropic（受访者：Slack CPO Jaime DeLanghe）
> 发布日期：2026-08-19
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）--Slack CPO 访谈，human-agent teams 系列第二篇
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

*This is the second post in our series on building human-agent teams. The*[*first*](https://claude.com/blog/building-effective-human-agent-teams)*shared what we've learned building teams with multiplayer AI at Anthropic. In this article, we share best practices from a company that was thinking about human-agent teams long before AI arrived.*

*这是我们“构建人机协作团队”（human-agent teams）系列的第二篇文章。第一篇分享了我们在 Anthropic 用多人 AI（multiplayer AI）构建团队的经验。在这篇文章中，我们分享一家早在 AI 出现之前就已在思考人机协作团队的公司的最佳实践。*

Jaime Delanghe joined Slack in 2017 to work on search and machine learning, with a mission to turn workplace conversation into institutional knowledge. Now the company’s Chief Product Officer, she has believed from the start that to achieve this goal, people need to work in the open, keeping conversations, decisions, and work in progress in channels anyone at the company can read and search. In her recent essay [*The Work is the Conversation*](https://jaimedelanghe.medium.com/the-work-is-the-conversation-50a1d61f8f9e), she makes the same case for agents: The conversation around the work is the context that agents need to be useful and finally help us achieve this decades-old goal of turning scattered knowledge into productivity.

Jaime Delanghe 于 2017 年加入 Slack，从事搜索与机器学习工作，使命是把职场对话转化为组织知识（institutional knowledge）。如今身为公司首席产品官（CPO）的她从一开始就相信：要实现这一目标，人们需要在公开环境中工作，把对话、决策和进行中的工作都留在公司里任何人都能阅读和搜索的频道里。在她最近的随笔 [*The Work is the Conversation*](https://jaimedelanghe.medium.com/the-work-is-the-conversation-50a1d61f8f9e) 中，她对 agent 提出了同样的主张：围绕工作的对话，正是 agent 发挥作用所需的上下文，也终于能帮我们实现这个由来已久的目标--把散落的知识变成生产力。

To learn what this looks like in practice at Slack, we talked with Jaime about her best practices for building effective human-agent teams and spreading these new ways of working.

为了了解这一切在 Slack 落地后的样子，我们与 Jaime 谈了谈她构建高效人机协作团队、推广这些新工作方式的最佳实践。

## 把对话历史当作知识库来对待（Treat your conversation history like a knowledge base）

For years, the promise that workplace conversation—the "exhaust" of people working together—would compound into organizational knowledge never materialized.

多年来，“职场对话--人们协作时产生的‘废气’（exhaust）--会累积成组织知识”这一承诺始终未能兑现。

"I have so many research papers from the early days at Slack that showed that, actually, no, conversation doesn't turn into knowledge," Jaime says. "You wish it did, but really it's just a lot of stuff that just hangs out there and people still have to repeat themselves."

“我在 Slack 早期收集过许多研究论文，它们表明，实际上，不，对话并不会变成知识，”Jaime 说，“你希望如此，但实际上它只是一大堆悬在那里的东西，人们仍然不得不重复自己。”

Making sense of all that exhaust simply wasn't humanly possible. Now it's an agent's job.

要从所有这些“废气”里理出头绪，靠人力根本做不到。现在，这是 agent 的工作了。

### 如何付诸实践（How to put this into practice）

- Default to public channels : Agents can only learn from what they can see. Decisions made in DMs or private threads are invisible to them—and stay lost to the organization.

- Ask agents for the reasoning, not just the record : Instead of searching for what was decided, ask an agent to reconstruct  why  it was decided, and how the context has shifted since.

- Widen the surface area : Tools like Slack and Claude are stitching meetings, emails, calendars, and document repositories together—the more of that context you connect, the less your team repeats itself.

- 默认使用公开频道：agent 只能从它们看得见的东西中学习。在私信（DM）或私密会话线程（threads）中做出的决定对它们不可见--也就永远流失于组织之外。

- 向 agent 要推理过程，而不只是记录：不要只搜索“决定了什么”，而是让 agent 重构“为什么这样决定”，以及从那以后上下文发生了怎样的变化。

- 扩大上下文接触面：Slack 和 Claude 这类工具正在把会议、邮件、日历和文档仓库缝合在一起--你连接的上下文越多，团队需要重复自己的地方就越少。

## 学会在 agent 与人类之间交接任务（Learn when to handoff tasks between agents and humans）

The core rhythm of a human-agent team is a cycle of handoffs. Powered by Claude in Slack, agents handle the production work—drafting, summarizing, monitoring, preparing—and pass the results to a person. The person reviews, decides, and redirects, then hands the work back for agents to carry out the next step.

人机协作团队的核心节奏是一轮又一轮的交接（handoff）。在 Slack 中的 Claude（Claude in Slack）加持下，agent 承担生产性工作--起草、总结、监控、准备--然后把结果交给一个人。这个人审阅、决策、调整方向，再把工作交回给 agent 执行下一步。

To see all this in practice, look no further than how Jaime starts her week.

要看这一切的实际运转，看看 Jaime 如何开启自己的一周就够了。

"It’s Monday morning, and I’ve just had my daily briefing that an agent has built for me,” Jaime says.

“周一早上，我刚看完一个 agent 为我构建的每日简报，”Jaime 说。

Also waiting for her review is a recap of the previous week's product workshops with flagged escalations, a report on AI developments across the web, briefings for the day's meetings, and a stale bio she'd handed to an agent to rewrite. At the end of each loop, humans review and make decisions based on the agent’s actions.

等待她审阅的还有：附带升级事项标记的上周产品工作坊复盘、一份全网 AI 动态报告、当天各场会议的简报，以及一份她交给 agent 重写的过时个人简介。每一轮循环的终点，都是人类基于 agent 的行动进行审阅并做出决策。

### 如何付诸实践：（How to put this into practice:）

- Start the day with agent-built briefings.  Recaps, escalations, meeting prep, and web roundups are great tasks for agents to drive, with human review.

- Anchor the work in a shared channel.  Share all work in a shared channel so that humans and agents can triage it together, with humans leading the charge on prioritization.

- Make lightweight signals actionable.  In Jaime's channel, an emoji reaction adds an item to the list and an agent picks up the task.

- 以 agent 构建的简报开启一天。复盘、升级事项、会议准备和网页资讯汇总，都很适合由 agent 驱动、辅以人工审阅。

- 把工作锚定在一个共享频道里。所有工作都放进共享频道，让人与 agent 能一起分诊处理，由人类主导优先级排序。

- 让轻量信号变得可行动。在 Jaime 的频道里，一个 emoji 回应就能把事项加入列表，随后由 agent 接手任务。

## 为 agent 委派清晰的角色（Delegate clear roles for agents）

Working with a fleet of specialized Claude agents can feel disorienting if your mental model is a one-on-one chatbot. Jaime's approach is social rather than technical: "I like to think that agents are kind of like coworkers."

如果你的心智模型还是一对一聊天机器人，与一群专门化的 Claude agent 协作可能会让人晕头转向。Jaime 的方法偏社交而非技术：“我倾向于把 agent 想成一种类似同事的存在。”

In the same way that human teammates have roles and responsibilities, agents should also have clear goals and focus areas. "If the value of the agent feels mandated rather than very clearly felt and understood by the people using it, it's really hard to remember what the thing is for,” she says.

就像人类队友各有角色与职责一样，agent 也应当有清晰的目标和专注领域。“如果 agent 的价值感觉是被强行规定的，而不是使用者能清晰感受和理解的，那就很难记住这东西到底是干什么用的，”她说。

**How to put this into practice:**

**如何付诸实践：**

- Route routine, transactional tasks to a general agent. Rather than asking people to remember a specialized tool, train an agent to tackle a repetitive task, like filing a help desk ticket or pulling last week's metrics into a status update.

- Let value be felt, not mandated.  If people can't articulate what an agent is for, it may be time to retire it.

- 把例行的、事务性的任务路由给一个通用 agent。与其让人们记住一个专门工具，不如训练一个 agent 去处理重复性任务，比如提交服务台工单，或把上周的指标拉进状态更新。

- 让价值被感受到，而不是被规定。如果人们说不清某个 agent 是干什么用的，也许就该让它退役了。

## 共享频道默认公开；转私有须有意为之（Default shared channels to public; go private on purpose）

Slack has recommended public-by-default channels since its earliest days: "You're building a shared understanding, a shared context for all of the work that's going to come next,” Jaime says.

Slack 从创立之初就推荐“默认公开”的频道：“你在构建一种共享的理解，一份为接下来所有工作准备的共享上下文，”Jaime 说。

She suggests keeping channels public unless there is a specific reason to gate context and knowledge. The most information agents have to pull from and inform their work, the more effective team mates they’ll be.

她建议保持频道公开，除非有特定理由要把上下文和知识关起来。agent 能获取并用来支撑工作的信息越多，它们作为队友就越有效。

Open context compounds—new people onboard into history instead of an empty inbox, and no one repeats themselves. Now agents benefit too, and that context and working memory flows back to humans.

公开的上下文会像复利一样累积--新人入职时接入的是历史，而不是一个空收件箱，也没有人需要重复自己。如今 agent 也从中受益，而这些上下文与工作记忆又会回流给人类。

**How to put this into practice:**

**如何付诸实践：**

- Keep business-as-usual work in the open. Make non-sensitive projects, announcements, and Q&A channels public so that agent coworkers can gain the knowledge they need to be most useful. .

- Remember your agents read what your team reads.  A private channel is a blind spot for every agent that reports on it.

- Let psychological safety drive the line.  Once genuinely sensitive material is walled off, the main reason work retreats into DMs isn't secrecy—it's discomfort with being seen mid-process. People should feel confident doing everyday work in the open, rough drafts and half-formed questions included, trusting their coworkers to meet it in good faith. And that openness compounds: "you gain trust by giving trust."

- 让日常业务工作保持公开。把非敏感的项目、公告和问答频道设为公开，让 agent 同事获得它们发挥最大效用所需的知识。

- 记住：你的 agent 读得到你的团队读到的东西。对每一个要就其汇报的 agent 来说，私有频道都是一个盲区。

- 让心理安全感来决定边界。一旦真正敏感的材料被隔离开，工作退回私信的主要原因并不是保密--而是对“过程被看见”的不自在。人们应该有信心在公开场合做日常工作，包括粗糙的草稿和尚未成形的问题，并相信同事会善意回应。而这种开放会复利累积：“信任是靠给予信任赢来的。”

## 通过展示可能性来推广采用（Spread adoption by showing the art of the possible）

The fastest way to learn a new way of working is to watch a teammate do it. Jaime has seen this at Salesforce, where employees share skills, debugging tips, and workflow tricks in a company-wide channel called *How I Slackbot*, which by her count has thousands of members. In that channel, which is public by default, a trick from a sales process can end up reshaping an engineering process.

学习一种新工作方式最快的途径，就是看队友怎么做。Jaime 在 Salesforce 看到了这一点：员工在一个名为 *How I Slackbot* 的全公司频道里分享技能、调试技巧和工作流窍门，据她估计该频道有数千名成员。在这个默认公开的频道里，一个来自销售流程的窍门，最终可能重塑一个工程流程。

Inside Slack, a push to get product managers using Claude "was the most self-organized thing you could possibly imagine." One PM got the developer experience lead to help him get set up, then he wrote up a canvas showing what he did and how he did it. Other PMs copied the format. Teams organized workshops and built their own git repos.

在 Slack 内部，推动产品经理使用 Claude“是你能想象到的最自组织的事情”。一位 PM 请开发者体验负责人帮他完成配置，然后写了一篇 canvas 文档，展示自己做了什么、怎么做的。其他 PM 纷纷照搬这个格式。各团队组织工作坊，还建起了自己的 git 仓库。

**How to put this into practice:**

**如何付诸实践：**

- Stand up a company-wide show-and-tell channel. Give employees one public place to share skills, debugging tips, and workflow tricks, so a trick from one function can reshape another.

- Encourage write-ups others can copy.  A short "what I did and how" doc turns one person's setup into a team template or skill.

- 建一个全公司的“展示与讲解”频道。给员工一个公开场所来分享技能、调试技巧和工作流窍门，让一个职能的窍门能够重塑另一个职能。

- 鼓励写出别人可以照搬的文档。一份简短的“我做了什么、怎么做的”文档，能把一个人的配置变成团队模板或技能。

## 衡量结果，而不是活跃度（Measure outcomes, not activity）

Since her early days at Slack, Jaime has grappled with the question of how to measure productivity. "Do we want people to send more messages?” she says. “Maybe not. Sending messages might not actually mean that they're getting more out of Slack. More messages can mean people can't find what they need, or can't say what they mean the first time."

从在 Slack 的早期起，Jaime 就一直在琢磨如何衡量生产力。“我们希望人们发送更多消息吗？”她说，“也许并不。发送更多消息未必意味着他们从 Slack 中收获更多。消息变多也可能意味着人们找不到需要的东西，或者无法一次就把意思说清楚。”

Now, the question of measuring the value of AI looks quite similar—and with something that complex, simple metrics don’t do the job. Token usage tells you the lights are on, but while that’s important to know, it’s not sufficient.

如今，衡量 AI 价值的问题看起来非常相似--面对如此复杂的事物，简单的指标胜任不了。Token 用量能告诉你“灯是亮着的”，虽然知道这一点很重要，但并不足够。

**How to put this into practice:**

**如何付诸实践：**

- Treat usage metrics as a pulse check, not proof of value.  Activity tells you adoption is happening, not that it's working.

- Be ready to use your own judgment. There's no clean way to prove that how people use these tools leads to better business results. As Jaime puts it, connecting the two still takes "a lot of leaps of faith," and no dashboard or usage stat will prove it for you.

- 把使用量指标当作脉搏检查，而不是价值证明。活跃度告诉你采用正在发生，而不是它正在起作用。

- 准备好运用你自己的判断。没有干净利落的方法能证明人们使用这些工具的方式带来了更好的业务结果。用 Jaime 的话说，把两者联系起来仍然需要“多次信念飞跃”（leaps of faith），没有任何仪表板或使用统计数据能替你证明这一点。

## 一起改变工作方式（Change how you work, together）

Jaime's biggest piece of advice for organizations trying to implement human-agent teams is to reimagine every workflow: "We're going to have to figure out how to change the ways that we're working, not just do more of the same kind of work faster. And that is going to be a team sport."

对于试图落地人机协作团队的组织，Jaime 最重要的建议是重新构想每一个工作流：“我们将不得不弄清楚如何改变我们工作的方式，而不只是把同类工作做得更快更多。而这将是一项团队运动。”

Her biggest advice for building an effective human-agent team? Start soon, but start small. Bring a group of people into a shared channel with Claude, give them the same set of resources, and let them work. If Slack's experience is any guide, what they build will spread on its own.

至于构建高效人机协作团队，她最大的建议是什么？尽早开始，但从小处开始。把一群人拉进一个有 Claude 的共享频道，给他们同一套资源，然后放手让他们干活。如果 Slack 的经验可以作为一种指引，他们构建出来的东西会自行传播开去。
