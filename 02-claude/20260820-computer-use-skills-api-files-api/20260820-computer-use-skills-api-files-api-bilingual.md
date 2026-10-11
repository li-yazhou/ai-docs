# 用 computer use、Skills API 与 Files API 构建生产级 agent（中英对照）

> 原文标题：Build production agents with computer use, the Skills API, and the Files API
> 原文链接：https://claude.com/blog/computer-use-skills-api-files-api
> 原文作者：Anthropic
> 发布日期：2026-08-20
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）--三大平台能力同日 GA 并新增 browser use 工具，是构建生产 agent 的关键节点
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

Computer use, the Skills API, and the Files API are generally available on the Claude Platform today. Computer use also adds a new browser use tool for agents that work in web applications. Together they let you build agents that operate software, apply your team's expertise, and return finished files.

computer use、Skills API 与 Files API 即日起在 Claude 平台正式可用（GA）。computer use 还为在 Web 应用中工作的 agent 新增了一个浏览器使用（browser use）工具。三者结合，让你能构建这样的 agent：会操作软件、能运用你团队的专业知识、并交付成品文件。

## **在 Claude 平台上构建 agent（Building agents on the Claude Platform）**

**Computer use** lets you build agents that operate software they can see. Given a screenshot, the agent clicks, types, and scrolls the way someone at the keyboard would. That lets it work in applications that were never built for automation. The new **browser use tool** extends this to the web. Alongside the screenshot, the agent reads the structure of the page and acts on a specific field or button rather than a position on screen.

**computer use** 让你构建能操作"它看得见的软件"的 agent。给定一张截图，agent 会像真正坐在键盘前的人一样点击、输入和滚动，因此它能使用那些从未为自动化而设计的应用。新的 **browser use 工具**把这种能力延伸到 Web：除截图外，agent 还能读取页面结构，直接作用于某个具体的输入框或按钮，而不是屏幕上的某个坐标位置。

The **Skills API** and the **Files API** let you give that agent your expertise and your documents. A skill is a folder of instructions, scripts, and templates that Claude loads only when a task calls for it. With the **Skills API** you upload and version your own skills, then attach them to any request. They run in Claude's code execution sandbox, so there is nothing for you to host. The **Files API** is storage for the documents an agent reads and writes: upload a PDF or spreadsheet once, reference it by ID in later requests instead of re-sending it, and download the files the agent creates.

**Skills API** 与 **Files API** 让你把团队的专业知识和文档交给这个 agent。skill（技能）是一个由指令、脚本和模板组成的文件夹，Claude 只在任务需要时才加载它。通过 **Skills API**，你可以上传自己的 skill 并做版本管理，然后附加到任意请求上。它们运行在 Claude 的代码执行沙箱中，你无需托管任何东西。**Files API** 是供 agent 读写文档的存储：PDF 或电子表格只需上传一次，后续请求用 ID 引用而无需重发，agent 生成的文件也可直接下载。

Say you're building a claims agent. It reads the intake document from the Files API, follows a skill that encodes the team's filing procedure, completes the submission in an insurer's web portal with the browser use tool, and saves the confirmation back as a file. Code execution and web search, already generally available, fit into the same loop.

假设你在构建一个理赔 agent。它从 Files API 读取报案材料，遵循一个编码了团队申报流程的 skill，用 browser use 工具在保险公司的 Web 门户里完成提交，再把确认凭证存回文件。早已正式可用的代码执行（code execution）与联网搜索（web search）也能接入同一个工作回路。

## **正式可用带来了什么（What's new with general availability）**

- Computer use: the updated computer use tool lets Claude take several actions per turn instead of one per model call, so tasks finish in fewer calls and less time. Computer use is also now eligible for HIPAA-regulated workloads under our BAA.

- Computer use：更新后的 computer use 工具让 Claude 每一轮可执行多个动作，而不是每次模型调用只做一个动作，任务因此用更少的调用、更短的时间完成。此外，computer use 现已可根据我们的 BAA（商业伙伴协议）用于受 HIPAA 监管的工作负载。

- Browser use tool: new in computer use today. It uses the same multi-action turns and adds page structure, so agents target web elements more reliably than with pixels alone.

- Browser use 工具：今天随 computer use 一同推出的新工具。它同样采用多动作轮次，并加入了页面结构信息，使 agent 定位 Web 元素比只靠像素更可靠。

- Skills API: a simpler API for uploading and versioning your own skills.

- Skills API：一套更简洁的 API，用于上传你的 skill 并做版本管理。

- Files API: automatic file expiration, 5x higher rate limits, and 1 TB of storage per organization.

- Files API：新增文件自动过期、速率限制提高 5 倍、每组织 1 TB 存储空间。
