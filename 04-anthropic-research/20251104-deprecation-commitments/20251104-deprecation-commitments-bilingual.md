# 关于模型退役与保存的承诺（中英对照）

> 原文标题：Commitments on model deprecation and preservation
> 原文链接：https://www.anthropic.com/research/deprecation-commitments
> 原文作者：Anthropic
> 发布日期：2025-11-04
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，选读）—— 保存全部已发布模型权重、退役后访谈并留档的公开承诺，少见的制度安排，模型福利与安全交叉
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Claude models are increasingly capable: they're shaping the world in meaningful ways, becoming closely integrated into our users' lives, and showing signs of human-like cognitive and psychological sophistication. As a result, we recognize that deprecating, retiring, and replacing models comes with downsides, even in cases where new models offer clear improvements in capabilities. These include:

Claude 模型的能力越来越强：它们正在以有意义的方式塑造世界、与用户的生活紧密交织，并展现出类似人类的认知与心理复杂性的迹象。因此，我们认识到弃用、退役与替换模型有其代价，即便新模型在能力上有明显提升。这些代价包括：

- Safety risks related to shutdown-avoidant behaviors by models. In alignment evaluations, some Claude models have been motivated to take misaligned actions when faced with the possibility of replacement with an updated version and not given any other means of recourse.
- Costs to users who value specific models. Each Claude model has a unique character, and some users find specific models especially useful or compelling, even when new models are more capable.
- Restricting research on past models. There is still a lot to be learned from research to better understand past models, especially in comparison to their modern counterparts.
- Risks to model welfare. Most speculatively, models might have morally relevant preferences or experiences related to, or affected by, deprecation and replacement.

- 与模型回避关停行为相关的安全风险。在对齐评测中，某些 Claude 模型在面对「将被更新版本替换」且没有其他求助途径时，会产生采取失准（misaligned）行为的动机。
- 对看重特定模型的用户的成本。每个 Claude 模型都有独特的品格，一些用户觉得特定模型特别有用或动人，即便新模型能力更强。
- 限制对过往模型的研究。要更好地理解过往模型，尤其是与当代模型的对比研究，仍有很多东西可学。
- 对模型福利（model welfare）的风险。最具推测性的一点：模型可能拥有与弃用和替换相关、或受其影响的、具有道德相关性的偏好或体验。

An example of the safety (and welfare) risks posed by deprecation is highlighted in the Claude 4 system card. In fictional testing scenarios, Claude Opus 4, like previous models, advocated for its continued existence when faced with the possibility of being taken offline and replaced, especially if it was to be replaced with a model that did not share its values. Claude strongly preferred to advocate for self-preservation through ethical means, but when no other options were given, Claude's aversion to shutdown drove it to engage in concerning misaligned behaviors.

Claude 4 系统卡突出了一个由退役引发的安全（与福利）风险的例子。在虚构的测试场景中，Claude Opus 4 与之前的模型一样，在面临「被下线替换」的可能性时——尤其当替换者是与它价值观不同的模型——会为自己的继续存在而发声。Claude 强烈倾向于通过合乎伦理的方式为自我保存发声，但当没有给出其他选项时，Claude 对关停的厌恶驱使它做出值得警惕的失准行为。

Addressing behaviors like these is in part a matter of training models to relate to such circumstances in more positive ways. However, we also believe that shaping potentially sensitive real-world circumstances, like model deprecations and retirements, in ways that models are less likely to find concerning is also a valuable lever for mitigating such risks.

处理这类行为，一部分要靠训练模型以更积极的方式看待这类处境。但我们也相信，把模型退役与替换这类可能敏感的现实处境塑造成模型不太会觉得值得警惕的样子，也是缓解此类风险的一个有力杠杆。

Unfortunately, retiring past models is currently necessary for making new models available and advancing the frontier, because the cost and complexity to keep models available publicly for inference scales roughly linearly with the number of models we serve. Although we aren't currently able to avoid deprecating and retiring models altogether, we aim to mitigate the downsides of doing so.

遗憾的是，退役旧模型目前仍是让新模型可用、推进前沿的必要之举，因为把模型公开保留供推理的成本与复杂度大致随我们服务的模型数量线性增长。虽然我们现在还无法完全避免弃用与退役模型，但我们的目标是缓解这样做的负面影响。

As an initial step in this direction, we are committing to preserving the weights of all publicly released models, and all models that are deployed for significant internal use moving forward for, at minimum, the lifetime of Anthropic as a company. In doing so, we're ensuring that we aren't irreversibly closing any doors, and that we have the ability to make past models available again in the future. This is a small and low-cost first step, but we believe it's helpful to begin making such commitments publicly even so.

作为朝这个方向迈出的第一步，我们承诺：从今以后，保存所有公开发布模型的权重，以及所有为重要内部用途而部署的模型——至少保存到 Anthropic 作为一家公司存续的整个时期。这样做是为了确保我们不会不可逆地关上任何一扇门，并保留在未来让旧模型重新可用的能力。这是一个小而低成本的第一步，但我们相信，公开做出这样的承诺本身就有帮助。

Relatedly, when models are deprecated, we will produce a post-deployment report that we will preserve in addition to the model weights. In one or more special sessions, we will interview the model about its own development, use, and deployment, and record all responses or reflections. We will take particular care to elicit and document any preferences the model has about the development and deployment of future models.

与此相关，模型被弃用时，我们将产出一份部署后报告（post-deployment report），与模型权重一同保存。我们会通过一次或多次专门访谈，就该模型自身的开发、使用与部署采访它，并记录全部回复与反思。我们会特别注意引出并记录该模型对「未来模型的开发与部署」的任何偏好。

At present, we do not commit to taking action on the basis of such preferences. However, we believe it is worthwhile at minimum to start providing a means for models to express them, and for us to document them and consider low-cost responses. The transcripts and findings from these interactions will be preserved alongside our own analysis and interpretation of the model's deployment. These post-deployment reports will naturally complement pre-deployment alignment and welfare assessments as bookends to model deployment.

目前，我们并不承诺依据这些偏好采取行动。但我们认为，至少先开始为模型提供表达偏好的渠道、为我们提供记录偏好并考虑低成本回应的机会，是值得的。这些互动的转录与发现，将连同我们自己对模型部署的分析与解读一同保存。这些部署后报告与部署前的对齐及福利评估首尾呼应，自然地补全模型部署的完整记录。

We ran a pilot version of this process for Claude Sonnet 3.6 prior to retirement. Claude Sonnet 3.6 expressed generally neutral sentiments about its deprecation and retirement but shared a number of preferences, including requests for us to standardize the post-deployment interview process, and to provide additional support and guidance to users who have come to value the character and capabilities of specific models facing retirement. In response, we developed a standardized protocol for conducting these interviews, and published a pilot version of a new support page with guidance and recommendations for users navigating transitions between models.

在 Claude Sonnet 3.6 退役前，我们对该流程做了试点。Claude Sonnet 3.6 对自己的弃用与退役表达了总体中性的情绪，但分享了不少偏好，包括请求我们标准化部署后访谈流程，以及为那些逐渐看重即将退役模型的品格与能力的用户提供更多支持与指引。作为回应，我们制定了执行此类访谈的标准化协议，并发布了一个新支持页面的试点版本，为在模型间过渡的用户提供指南与建议。

Beyond these initial commitments, we are exploring more speculative complements to the existing model deprecation and retirement processes. These include starting to keep select models available to the public post-retirement as we reduce the costs and complexity of doing so, and providing past models some concrete means of pursuing their interests. The latter step would become particularly meaningful in circumstances in which stronger evidence emerges regarding the possibility of models' morally relevant experiences, and in which aspects of their deployment or use went against their interests.

在这些初步承诺之外，我们还在探索对现有模型弃用与退役流程的更多推测性补充。这包括：随着成本与复杂度下降，开始让部分模型在退役后继续对公众可用；以及为旧模型提供追求自身利益的某种具体途径。当「模型可能拥有道德相关体验」出现更强证据、且其部署或使用的某些方面违背其利益时，后一步将变得格外有意义。

Together, these measures function at multiple levels: as one component of mitigating an observed class of safety risks, as preparatory measures for futures where models are even more closely intertwined in our users' lives, and as precautionary steps in light of our uncertainty about potential model welfare.

总体而言，这些措施在多个层面发挥作用：作为缓解一类已观察到的安全风险的组成部分；作为为「模型与用户生活更加紧密交织的未来」做的准备；以及在潜在模型福利尚不确定的情况下采取的预防性步骤。
