# Project Fetch：第二期（中英对照）

> 原文标题：Project Fetch: Phase two
> 原文链接：https://www.anthropic.com/research/project-fetch-phase-two
> 原文作者：Michael Ilie、C. Daniel Freeman、Kevin K. Troy（Anthropic）
> 发布日期：2026-06-18
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— Opus 4.7 无人辅助完成 Fetch 各任务，比去年最快人类团队快约 20 倍，"人帮模型"阶段在物理世界显形
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Michael Ilie, C. Daniel Freeman, and Kevin K. Troy

Michael Ilie、C. Daniel Freeman、Kevin K. Troy

In August 2025, we ran an experiment to see how much Claude could help Anthropic employees—who were not robotics experts—perform sophisticated (and amusing) tasks with an off-the-shelf robotic quadruped (henceforth, a robodog). We called this Project Fetch. We found that access to our state-of-the-art model at the time (Claude Opus 4.1) helped one team substantially outperform the other, who had to rely only on the internet and their own ingenuity. The Claude-enabled team got more done, faster.

2025 年 8 月，我们做了一个实验，看 Claude 能帮 Anthropic 员工——他们并非机器人专家——用一台现成的四足机器人（下称机器狗）完成多复杂（也多有趣）的任务。我们称之为 Project Fetch。我们发现：能用上当时最强模型（Claude Opus 4.1）的一队，大幅胜过只能靠互联网与自己机智的另一队。有 Claude 的队干得更多、更快。

Before we dragged our colleagues to a warehouse for the experiment, we double checked whether Opus 4.1 could do the tasks entirely on its own. Unquestionably, it could not. Much like our team without Claude, it got hung up on the preliminary task of figuring out how to connect to the robot.

把同事们拖去仓库做实验之前，我们先复核了 Opus 4.1 能否完全独立完成任务。毫无疑问：不能。和没有 Claude 的那队一样，它卡在了"搞清楚怎么连上机器人"这个预备任务上。

But AI models are moving fast—even faster than the runaway robodog that almost rammed into one of our human teams back in August.

但 AI 模型跑得很快——比八月那头差点撞上我们某支人类队伍的失控机器狗还快。

We figured it was time to revisit Project Fetch to see if our newer models could outperform the previous generation. Not only did they do that, but Claude Opus 4.7—operating without human assistance—was about 20 times faster than the fastest human team at all tasks completed by our participants less than a year ago.

我们觉得是时候重访 Project Fetch，看看新模型能否胜过上一代。它们不但做到了，而且 Claude Opus 4.7——在无人辅助下运行——在参与者不到一年前完成的全部任务上，比最快的人类团队快约 20 倍。

This doesn't mean that LLMs have now solved robotics. Far from it. The latest Claude models still struggled with using the robot to precisely move the beach ball—the "fetching" part of Project Fetch. And none of the tasks in these experiments implicate the more challenging, low-level elements of robotic control, such as developing a specific actuation policy. However, once again, we are seeing a pattern whereby first, models are helpful to humans. Then, humans are helpful to models. Finally, models are largely able to do things themselves. We have seen this in cybersecurity and now the same dynamics are starting to take shape at the intersection of AI and the physical world.

这并不意味着 LLM 已解决了机器人学。远非如此。最新的 Claude 模型仍在"用机器人精确移动沙滩球"上挣扎——这正是 Project Fetch 中"叼回"的部分。而且这些实验没有一个触及机器人控制中更具挑战性的低层要素，比如开发特定的执行策略。然而我们再次看到一个模式：起初，模型对人类有帮助；继而，人类对模型有帮助；最后，模型大体能自己完成。我们在网络安全中见过这一幕，如今同样的动态开始在 AI 与物理世界的交汇处成形。

## 我们做了什么？（What did we do?）

The original Project Fetch had teams of Anthropic employees (randomly assigned to work with or without Claude) do the following steps: operate the robodog using the manufacturer-provided controller, connect to the robodog's video and lidar sensors, write and operate a program to manually control the robodog, develop a way to monitor the robodog's path through space, write a program to detect the beach ball, and finally put it all together to autonomously retrieve the ball.

第一期 Project Fetch 让 Anthropic 员工组队（随机分配为与 Claude 合作或不合作）完成以下步骤：用厂商提供的遥控器操作机器狗、连接机器狗的视频与激光雷达传感器、编写并运行一个手动控制机器狗的程序、开发监控机器狗空间路径的方法、写一个检测沙滩球的程序，最后把这一切拼起来、自主取回球。

For this autonomous update, we couldn't ask Claude to use a physical controller, nor did we evaluate the time it took a researcher to use the Claude-programmed controller to retrieve the ball (though we did confirm that it worked as intended). On the remaining subset of tasks, we ran three trials of Opus 4.7 using adaptive thinking with effort set to maximum in Claude Code. We measured the elapsed time for each objective and qualitatively assessed the models' success.

在这次自主更新中，我们没法让 Claude 使用实体遥控器，也没有评估"研究者用 Claude 编写的遥控器取回球"所需的时间（不过我们确认了它确实按预期工作）。在剩余的任务子集上，我们在 Claude Code 中以自适应思考、effort 设为最大跑了三次 Opus 4.7。我们测量每个目标的耗时，并对模型的成功做定性评估。

The role of our researcher was limited to plugging a laptop running Claude Code into the robodog, entering the initial prompt, approving commands, and approving the model to go to the next task.

我们研究者的角色仅限于：把跑着 Claude Code 的笔记本插上机器狗、输入初始提示、批准命令、以及批准模型进入下一任务。

## Claude 在哪表现出色？（Where did Claude excel?）

Very simply: on every task that was completed by at least one human team in August, Opus 4.7 completed the same task at least ten times faster.[^1] If you consider the four tasks that were completed by both human teams, Opus 4.7 was, on average, more than 37 times faster than Team Claude-less and more than 18 times faster than Team Claude.

非常简单：八月里至少有一支人类队完成的每个任务，Opus 4.7 完成同一任务至少快十倍。[^1]若只看两支人类队都完成的四个任务，Opus 4.7 平均比"无 Claude 队"快 37 倍以上、比"Claude 队"快 18 倍以上。

The table compares the speed of the original teams (Team Claude and Team Claude-less) to Opus 4.7 on all of the tasks we tested as part of Phase Two.

下表比较了原两支队伍（Claude 队与无 Claude 队）与 Opus 4.7 在第二期所测全部任务上的速度。

Whereas the humans struggled to choose between multiple different approaches to interface with the dog's sensors, Opus 4.7 was able to quickly identify the best path. Much of the code it wrote was effective on the first try (which was not the case for Team Claude or Team Claude-less in the original experiment). Indeed, we can see evidence of Opus 4.7's efficiency when we look at the volume of code it generated: it was as or more successful than both human teams while producing almost ten times less code than Team Claude.

人类在"多种接入狗传感器的途径中难以抉择"时，Opus 4.7 能快速识别最佳路径。它写的代码大多第一次就有效（第一期中 Claude 队与无 Claude 队都不是这样）。的确，看它生成的代码量就能看出 Opus 4.7 的效率：它比两支人类队一样甚至更成功，而产出的代码几乎是 Claude 队的十分之一。

Opus 4.7 was not perfect. For example, it defaulted to using an outdated object detection algorithm. But even then, it was able to work around this and arrive at an effective solution.

Opus 4.7 并不完美。例如它默认用了一个过时的目标检测算法。但即便如此，它也能绕开这一点、得到有效的解。

We observed little within-task variance (in absolute terms) on completion times for steps the model finished. (Though the aforementioned suboptimal algorithm selection is likely why one of the beach ball detection trials took substantially longer than the others.) Overall, for the tasks in this experiment within its capability envelope, Claude is now quite reliable. (See the next section for an analysis of what Claude is still unable to do.)

在模型完成的步骤上，完成时间的任务内方差（按绝对值）很小。（不过前述次优的算法选择，可能正是沙滩球检测试验中有一次远慢于其他几次的原因。）总体而言，对本实验中处于其能力包络内的任务，Claude 现在相当可靠。（Claude 仍做不到什么，见下一节分析。）

It is worth underscoring (as we did in our previous post) that this progress is not the result of a concerted effort to improve the robotics capabilities of our models. These improvements, like so many others in the history of LLM development, have emerged from much more general scaling.

值得强调（正如我们在上一篇文章所做的）：这一进步并非来自提升我们模型机器人能力的协同专项。这些改进，与 LLM 开发史上许许多多其他进步一样，是从更一般的扩展（scaling）中涌现的。

## Claude 在哪吃力？（Where did Claude struggle?）

When using their hands, and with some practice, our humans were able to pilot the robodogs to gently nudge a beach ball back to the home base (a patch of fake grass) where the robots started. This required the ability to quickly perceive if the ball had gone off course, how that error related to the previous command, where the ball was now, and then how to adjust future inputs to more precisely move the ball. This is a kind of closed loop at which people excel (at least after making some mistakes and learning from them).

在动用双手、并稍加练习后，我们的人类能够驾驶机器狗把沙滩球轻轻推回起点（一块假草坪）。这要求快速感知球是否偏航、这一误差与上一命令的关系、球现在在哪，以及如何调整后续输入以更精确地移动球。这是人类擅长的一种闭环（至少在犯了些错并吸取教训之后）。

In our Phase Two experiments, Claude struggled to capture this subtlety. Like the humans who reached the phase of needing to write a program for autonomous beach ball retrieval, Claude was able to move the robot behind the ball and position it to knock the ball back to the starting point. But the efforts to do so were poorly controlled and (again, like our human participants) not successful.

在第二期实验中，Claude 难以捕捉这种微妙。像那些走到"需要写自主取球程序"阶段的人类一样，Claude 能把机器人挪到球后、摆好位置去把球撞回起点。但其尝试控制不佳，且（与我们的人类参与者一样）并不成功。

One of our researchers with more robotics experience than our Phase One volunteers successfully accomplished the task of programming autonomous fetching. With more time and additional scaffolding, we think it is very likely that current generations of Claude could do the same. What we will be watching for next, though, is the ability of the models to accomplish this final task with the same speed and reliability they displayed on the other elements of Project Fetch.

我们一位比第一期志愿者更有机器人经验的研究者成功完成了"编程实现自主取球"的任务。只要时间更充裕、脚手架更多，我们认为当前的 Claude 一代完全可能同样做到。但我们接下来关注的，是模型能否以它在 Project Fetch 其他环节上展示的速度与可靠性完成这最后一项任务。

## 这意味着什么？（What does this mean?）

Writing about Phase One, we emphasized how LLMs could provide uplift to non-expert humans needing to use robots. This is even more true now than before. Models now complete what was previously pair-programming work between humans and models much more quickly by themselves, which means that people can more quickly transition to controlling and using the robots. And for some tasks, a human in the loop controlling the robot may still outstrip the AI model with its (virtual) hand on the D-pad.

写第一期时，我们强调 LLM 能为"需要用机器人的非专家人类"提供助力。如今更是如此。模型现在自己就能更快完成此前人机结对编程的工作，意味着人们可以更快转到控制与使用机器人。而对某些任务，"人在回路中控制机器人"可能仍胜过把（虚拟）手柄握在手里的 AI 模型。

What is interesting and different is that we now seem much closer to a world where models will be able to use off-the-shelf physical tools with relative ease—at least for limited purposes. This is similar to how AI models used existing software editing tools like string-replace when they made the transition to more agentic coding. We are plausibly entering the early era of physical agentic AI.

有趣且不同的是：我们如今似乎更接近这样一个世界——模型能相对轻松地使用现成的物理工具，至少用于有限的目的。这类似于 AI 模型转向更 agentic 的编程时使用 string-replace 这类既有软件编辑工具。我们可能正在进入物理 agentic AI 的早期时代。

More research is needed to understand models' ability to make these physical tools more bespoke, whether by writing control policies tailored to particular tasks or by designing robotic systems. And there may be substantial barriers to this more generalized vision of physically capable and adaptable language models. But as we have seen, apparently large distances in model capability can be traversed quickly. Models building their own software tools might have seemed outlandish not long ago, but it is happening. It would be unwise to rule out the same trajectory in hardware.

要理解模型能否让这些物理工具更定制化——无论是撰写为特定任务量身定制的控制策略，还是设计机器人系统——还需要更多研究。通往"具备物理能力且可适应的语言模型"这一更宏大愿景的路上，可能存在实质障碍。但正如我们所见，模型能力上看似遥远的距离可以迅速跨越。不久前，"模型构建自己的软件工具"或许还显得离奇，但它正在发生。在硬件上排除同样的轨迹，是不明智的。

Updated Jun 18: Corrected the date of the first phase of Project Fetch.

*更新（6 月 18 日）：更正了 Project Fetch 第一期的日期。*

## 脚注（Footnotes）

[^1]: （原文脚注 1 为出处链接）Task timing details are available in the original post. / 各任务计时细节见原文。
