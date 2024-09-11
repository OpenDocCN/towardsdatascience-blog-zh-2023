# 语音助手的可访问性

> 原文：[https://towardsdatascience.com/voice-assistant-accessibility-dc737cde0394?source=collection_archive---------12-----------------------#2023-03-31](https://towardsdatascience.com/voice-assistant-accessibility-dc737cde0394?source=collection_archive---------12-----------------------#2023-03-31)

## 确保每个人都能被理解

[](https://addlesee.medium.com/?source=post_page-----dc737cde0394--------------------------------)[![安格斯·阿德尔斯](../Images/0a6a016590ca622cc3c8cae24e188f6e.png)](https://addlesee.medium.com/?source=post_page-----dc737cde0394--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc737cde0394--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dc737cde0394--------------------------------) [安格斯·阿德尔斯](https://addlesee.medium.com/?source=post_page-----dc737cde0394--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7f06284203ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoice-assistant-accessibility-dc737cde0394&user=Angus+Addlesee&userId=7f06284203ea&source=post_page-7f06284203ea----dc737cde0394---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc737cde0394--------------------------------) ·10分钟阅读·2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc737cde0394&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoice-assistant-accessibility-dc737cde0394&user=Angus+Addlesee&userId=7f06284203ea&source=-----dc737cde0394---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc737cde0394&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoice-assistant-accessibility-dc737cde0394&source=-----dc737cde0394---------------------bookmark_footer-----------)

我多年来一直从事使用大型语言模型（如GPT-4）的对话式AI工作。由于chatGPT的流行大爆发，令人非常兴奋，但它们如何改进呢？当然，对于这个问题有很多答案，但在这篇文章中，我将重点关注可访问性。

我们如何调整未来的机器学习模型以利用有限的数据集？我们应该如何设计我们的代理以确保每个人都能利用语音人工智能的进步？

> *这是对* [*我*](http://addlesee.co.uk/) *在* [*IWSDS 2023*](https://sites.google.com/view/iwsds2023/home)*上发表的论文的缩写。如果你想引用本文中讨论的内容，请引用标题为“*[*语音助手的可访问性*](https://drive.google.com/file/d/1gANMtWuP1w0gba7g4nv-ehCMax25itaZ/view)*”的论文。*

```py
**Harvard**:
Addlesee, A., 2023\. Voice Assistant Accessibility. Proceedings of the 13th International Workshop on Spoken Dialogue Systems Technology (IWSDS).

**BibTeX**:
@inproceedings{addlesee2023voice,
  title={Voice Assistant Accessibility},
  author={Addlesee, Angus},
  journal={Proceedings of the 13th International Workshop on Spoken Dialogue Systems Technology (IWSDS)},
  year={2023}
}
```

全球有超过十亿人生活在某种形式的残疾中，而语音助手有可能改善人们的生活。例如，在我访问一个名为[Leuchie House](https://www.leuchiehouse.org.uk/)的休养院时，一位多发性硬化症（MS）患者解释了这种疾病的进展如何逐渐侵蚀了他们的独立性。一台亚马逊Alexa设备使这位患者能够在无需护理人员帮助的情况下关闭卧室灯。他们告诉我们，这是自诊断以来，他们第一次重新获得一些个人独立性。

> *像上面这样的故事激励着慈善机构推广语音助手的使用，因为它们能产生* 真实的积极影响*。

![](../Images/1a99404d542c0f20ec335e86d1ba2260.png)

我（右侧）于2021年9月与同事们访问了[Leuchie House](https://www.leuchiehouse.org.uk/)。

语音助手的创作者正在获得HIPAA合规认证，以便在医疗保健领域进一步应用，并发布了*专门针对脆弱用户群体*的功能。我们同样看到早期研究者与其他学科合作，将他们的工作应用于更具体的医疗保健应用（请参见我关于语音研究趋势的[TDS文章](/the-future-of-voice-assistants-what-are-the-early-research-trends-dc02215fe2aa)）。

语音助手的可及性因此至关重要，以确保未来系统在设计时考虑到最终用户的互动模式和需求。今天的语音助手在大量代表“平均”用户的数据集上进行训练和评估，但我们知道[随着认知能力的下降，语音会发生变化](http://www.lrec-conf.org/proceedings/lrec2020/workshops/LEGAL2020/pdf/2020.legal2020-1.4.pdf)。现有的商业系统在帮助视力障碍者方面存在[巨大的隐私问题](https://heartbeat.comet.ml/am-i-allergic-to-this-developing-a-voice-assistant-for-sight-impaired-people-3f036fe7792b)，而人们在公共场所与语音助手互动时需要公开宣布自己的残疾（下文讨论）。

随着研究和工业界推动语音助手的巨大利益超越大众市场应用，新的挑战和伦理问题也随之出现，这些问题必须被突出讨论。在本文中，我探讨了最先进的研究和当前可用的商业系统，针对多个用户群体，例如痴呆症患者、运动神经元病患者、视力丧失者、心理健康状况患者等。

> *我首先讨论了为认知障碍和心理健康状况人群设计语音助手，其次讨论了为身体残疾者设计系统，然后讨论了公共环境中的语音隐私，最后总结。*

# 针对认知障碍和心理健康状况的对话系统

认知障碍影响记忆、注意力、解决问题的能力、决策、*言语产生*等。认知障碍的发生和进展通常与个人的年龄相关，但某些病症（如早发性痴呆）可能由中风或头部创伤引起。轻度认知障碍（MCI）具有上述症状，但不会显著干扰个人的生活。虽然 MCI 患者通常也是老年人，但另一个影响所有年龄段的脑健康子集是心理健康状况。

![](../Images/66e9b7269c1ca1ae5a162ae7b9d7dc00.png)

下文提到的 SPRING 项目中使用的 ARI 机器人（由 [PAL Robotics](https://pal-robotics.com/) 生产）。

## 痴呆症和 MCI

随着认知能力的下降，人们在句子中途的停顿变得更频繁且时间更长。其他言语产生的变化也包括：增加重复、说话速度减慢、介词使用增多，[等等](https://heartbeat.comet.ml/how-dementia-effects-conversation-f538d2d9507a)。如今的语音助手常常将这些长时间的停顿误认为是用户发言的结束，迫使用户不得不挫败地重复整个陈述。

推荐痴呆症和 MCI 患者使用标准语音助手（如 Google Home 和 Amazon Alexa）进行日常使用。像 [CogniHealth](https://www.cognihealth.uk/) 这样的公司致力于策划内容，以帮助痴呆症患者及其家人——通过语音助手提供有价值的信息、建议和支持——但这些系统的语音处理和理解组件没有针对上述特定挑战的无障碍选项。

由于数据不足，该领域的研究受到限制。收集自然语言对话数据，尤其是与脆弱的老年人进行对话，[在伦理上具有挑战性](/ethically-collecting-conversations-with-people-that-have-cognitive-impairments-9ad0d2714bdd)，特别是近年来由于 COVID，需要 [定制工具](https://aclanthology.org/2022.nlp4pi-1.3/) 来保障数据安全。像 [EU H2020 SPRING 项目](https://spring-h2020.eu/) 这样的研究正在医院记忆门诊候诊室中收集数据，以应对这些挑战。在这种环境下，患有痴呆症或 MCI 的患者通常会有家庭成员或看护者陪同，带来更多的多方复杂情况（剧透：这是我下一篇文章的主题）。数据收集后，可以利用 [TalkBank](https://www.talkbank.org/) 的子库 [DementiaBank](https://dementia.talkbank.org/) 将数据与其他研究痴呆症交流的研究人员共享。

## **心理健康状况**

某些心理健康状况也会影响人们的语言表达和行为。焦虑症患者的语速较快，停顿时间较短，而抑郁症或创伤后应激障碍（PTSD）患者说话较慢且沉默较多。

目前似乎没有商业语音助手能够调整其语音处理以更好地理解心理健康状况的人——尽管存在针对这些用户群体的公司。像[UB-OK](https://www.ubok.app/)和[Kindspace](https://createyourkindspace.com/)这样的语音助手应用提供了一个安全的空间，让人们可以在没有评判的情况下分享他们的忧虑。人们，特别是年轻人，可以询问关于心理健康和其他他们可能不愿向朋友、老师或家人提问的问题。

![](../Images/6bc7b7d5d351be2bc800bed4f1d950b6.png)

Michael McTernan 在2019年苏格兰Edge Awards上展示UBOK

人机交互（HRI）研究在这一领域十分丰富。系统的语音处理仍然保持标准，但机器人的交互方式有所调整。例如，可以借鉴心理学的沟通技巧来促进自我反思，并帮助缓解孤独（这与抑郁症常常伴随）。

应用范围确实非常广泛。自闭症的语言能力较弱的儿童使用了语言生成设备，长期使用后这些设备鼓励他们自发说话并使用新词汇。类似的工作有效地利用互动社交机器人进行自闭症治疗。

> *上述所有内容都令人兴奋，但关键是，没有研究专注于改善系统的*语音处理和理解*。*

# 为**身体障碍**人士设计对话系统

身体障碍会影响用户可能向语音助手提出的问题以及在收到回应后如何采取行动。例如，视力受损的人会[询问他们的视觉环境](https://heartbeat.comet.ml/the-spoon-is-in-the-sink-assisting-visually-impaired-people-in-the-kitchen-ccea20b098cd)，而有听力困难的人则会期待多模态的回应。

## **视觉障碍**

盲人和视力受限的人往往会遭受似乎无关的健康问题，如营养不良。这主要是因为他们在完成日常任务（如购物、准备食物和烹饪）时遇到困难。一组视觉障碍计算机科学家在[计算机视觉与模式识别会议（CVPR 2020）](https://www.youtube.com/watch?v=f613diLbVAc)上讨论了在看不到时刻表、站台号、车厢字母或方向标志时，如何在火车站内导航的困难。

![](../Images/f6617bf71164482b3e9c635cc797c479.png)

类似于火车站，人们会询问有关食品包装上的文本信息。这张图片来自一篇关于厨房中这一问题的文章 ([here](https://heartbeat.comet.ml/am-i-allergic-to-this-developing-a-voice-assistant-for-sight-impaired-people-3f036fe7792b))。

也有“人机协作”解决方案，你可以联系有视力的人来回答问题。必须同时发送照片或视频以传达视觉场景。 [BeMyEyes](https://bemyeyes.com/) 和 [BeSpecular](https://bespecular.com/) 依赖视力正常的志愿者及时回答问题，而 [Aira](https://aira.io/) 则有一支经过培训的专业团队。

> *当依赖志愿者时，一个明显的问题是：视力受限的人无法知道发送的图像中是否能看到敏感信息。因此，志愿者可能会看到邮件中有姓名、地址、身份证号码或个人家中的贵重物品。Aira通过招聘和培训专注于安全和保障的工作人员来缓解这个问题，但这需要付出代价。*

也存在端到端 (E2E) 系统，如 [TapTapSee](https://taptapseeapp.com/) 和 [Microsoft Seeing AI](https://microsoft.com/ai/seeing-ai)。这些系统在云端安全运行，并加密处理，因此隐私问题最小，但它们引入了一个新的问题——准确性。视力受限的人无法验证系统的回答是否正确。由于无法进行对话，用户只能依赖系统的回答。这可能导致像药物问题这样的困境。E2E 系统可能不正确，可能对用户造成伤害，但人机协作系统需要他们将药物照片发送给未知的志愿者。

![](../Images/021fae2a79826c457abdecf7aa61c6ea.png)

这张图片展示了尝试回答视力障碍者问题的一些工作实例。目标是让用户知道系统何时不确定 ([here](https://heartbeat.comet.ml/the-spoon-is-in-the-sink-assisting-visually-impaired-people-in-the-kitchen-ccea20b098cd))。

## **有限行动能力**

家庭开放域语音助手非常方便：我们可以在手上沾满油的时候设定定时器，或者在沙发上舒适的温暖毯子下调高音乐。

> *这些功能不仅对行动不便的人很方便，它们对心理健康至关重要。手部、手臂或腿部有行动限制的人可以通过语音保持一定的个人独立性。*

移动能力丧失本身通常不会影响语言生成，但语音助手仍可以设计得更具可及性。虽然存在提高人们舒适度和完成日常任务能力的硬件（如先进的轮椅），但这些技术目前尚未与现有的语音助手整合。在访问一个名为[Leuchie House](https://www.leuchiehouse.org.uk/)的临时护理之家时，一位居民描述了他们的沮丧，语音助手可以打开窗帘和关闭电视，但他们不得不请护理人员调整电动轮椅的靠背。这突显了包容性设计的必要性。

## **听力障碍**

有听力困难的人对语音助手感到沮丧，从而完全放弃使用它们。对于那些在年轻时就出现听力问题的人，这种情况更加严重，因为这往往会导致语言障碍。例如，如果听不到对话，就无法学习发音。下面将讨论语言障碍的影响，但即使没有这些障碍，现有的语音助手似乎也存在局限。研究指出，有部分听力丧失的人在嘈杂环境中（如公共场所）很难跟上对话，但当设置了实时转录对话的屏幕时，他们在对话中感到更有融入感。[实时说话者识别](https://aclanthology.org/2020.coling-main.312.pdf)将更为有效。因此，我们应确保语音助手和社交机器人在公共多方环境中包含屏幕，以实现这一功能。

![](../Images/5b10f22394bcaace824072dc6d41ff08.png)

本田的Asimo机器人用手语说“我爱你”（[来源](https://unsplash.com/photos/g29arbbvPjo)）

许多听力障碍者懂得大约200种手语中的一种。研究表明，助手可以学习手语，但NLP社区的更多努力可以利用现有资源来改进手语处理和生成。听力障碍者渴望参与语音助手的包容性设计，以确保无障碍进展。

## **语音多样性**

语言生成具有细微差别且因人而异，但自动语音识别（ASR）学习的是一般语音模式，因此难以理解非母语者或口音很重的人（例如苏格兰人）。然而，这一问题还进一步扩展。有口吃的人被误解，有发音困难的人（如因早期听力丧失导致）被误解，还有图雷特综合症患者被排除在对话研究之外。非标准语音也可能由影响我们用来生成语言的肌肉的疾病引起，例如肌营养不良症。

谷歌在这方面通过三个项目进行创新。[项目 Euphonia](https://sites.research.google/euphonia/about/) 和 [项目 Relate](https://sites.research.google/relate/) 是谷歌的举措，旨在帮助那些说话方式不标准的人更好地被理解，[项目 Understood](https://projectunderstood.ca/) 是他们的计划，旨在更好地理解唐氏综合症患者。谷歌甚至开设了 [无障碍发现中心](https://blog.google/around-the-globe/google-europe/united-kingdom/the-accessibility-discovery-centre-is-open-for-collaboration/) 来与学术界、社区和慈善/非营利组织合作，旨在“消除无障碍障碍”。

项目 Relate

## **言语丧失**

像运动神经元病（MND）这样的某些疾病患者会逐渐失去完全说话的能力。斯蒂芬·霍金因MND使用了合成语音，但今天在质量和多样性方面已经有了巨大的改善。像 [Cereproc](https://www.cereproc.com/) 这样的公司合成具有个性、引人入胜和情感化的语音，带有不同的口音，帮助MND患者选择最能代表自己的声音。

语音克隆技术也变得可能，这使得面临失语风险的人能够使用语音银行技术。人们录制自己的语音以便在需要时进行克隆。像 [SpeakUnique](https://www.speakunique.co.uk/) 这样的公司，甚至可以在语音在诊断后部分退化的情况下，重新构建一个人的原始声音。

# **隐私**

个人语音助手通常仅由单一用户在私人空间中使用。因此，语音助手可以高度定制以满足该用户的需求。然而，这在公共空间中的助手并非如此。博物馆、医院、机场等公共场所的社交机器人每天会被许多人使用。一些无障碍设计的实现惠及所有人（例如，我们有时在句子中会忘记词汇），但其他则不然。

![](../Images/df691042c1a010c5ceb507c1a4fc1364.png)

一个购物中心的 Pepper 机器人（[来源](https://unsplash.com/photos/hND1OG3q67k)）。人们在公共场所与其他人一起时，可能会感到不舒服地披露个人无障碍需求。

> *大多数残疾是看不见的，因此人们必须在公共场所大声描述自己的残疾，以激活某些无障碍功能。这在没有语音助手的情况下同样成问题——残疾人士通常需要在商店里宣布自己的残疾和帮助需求。*

像 [Neatebox](https://neatebox.com/) 这样的技术正在迅速普及，以解决这一问题。残障用户可以下载应用程序并注明他们希望如何得到个人化帮助（例如，被手臂引导）。然后，当他们进入商店或机场时，客户服务团队会收到通知并提供个性化帮助。类似的技术也可以用于公共空间的社交机器人，当与需要可访问语音助手的人互动时，激活特定功能。

# **结论**

语音助手可以超越简单的便利来改善人们的生活，这可以通过道德的数据收集和包容性设计来实现。然而，这并不简单，每个语音助手系统组件在设计针对**所有人**的系统时都必须被考虑在内。

我已经总结了为了使语音助手对本文讨论的用户群体更具可访问性，必须调整的具体组件：

![](../Images/68b1833fbcaee048963e81788ba4705f.png)

在 [论文](https://drive.google.com/file/d/1gANMtWuP1w0gba7g4nv-ehCMax25itaZ/view) 的表格 1

你可以通过 [我](http://addlesee.co.uk/) 在 [Medium](https://medium.com/@addlesee)、[Twitter](https://twitter.com/Addlesee_AI) 或 [LinkedIn](https://www.linkedin.com/in/angusaddlesee/) 联系到我。
