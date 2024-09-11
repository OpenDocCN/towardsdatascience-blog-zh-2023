# 碳足迹：为什么常见的说法可能不准确

> 原文：[https://towardsdatascience.com/carbon-footprint-why-common-claims-may-not-be-accurate-6f7860a7f08b?source=collection_archive---------9-----------------------#2023-03-31](https://towardsdatascience.com/carbon-footprint-why-common-claims-may-not-be-accurate-6f7860a7f08b?source=collection_archive---------9-----------------------#2023-03-31)

## 创建强健的 CO₂ 情景以推动数据驱动的气候行动

[](https://medium.com/@boris-ruf?source=post_page-----6f7860a7f08b--------------------------------)[![Boris Ruf](../Images/96dc4fc2f32add89fef6911195590cd8.png)](https://medium.com/@boris-ruf?source=post_page-----6f7860a7f08b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6f7860a7f08b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6f7860a7f08b--------------------------------) [Boris Ruf](https://medium.com/@boris-ruf?source=post_page-----6f7860a7f08b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fed341456850c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarbon-footprint-why-common-claims-may-not-be-accurate-6f7860a7f08b&user=Boris+Ruf&userId=ed341456850c&source=post_page-ed341456850c----6f7860a7f08b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f7860a7f08b--------------------------------) ·7 min read·2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6f7860a7f08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarbon-footprint-why-common-claims-may-not-be-accurate-6f7860a7f08b&user=Boris+Ruf&userId=ed341456850c&source=-----6f7860a7f08b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6f7860a7f08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarbon-footprint-why-common-claims-may-not-be-accurate-6f7860a7f08b&source=-----6f7860a7f08b---------------------bookmark_footer-----------)![](../Images/4bd5a020591cf29afa125f1bf4482aca.png)

图片由 [David Aler](https://unsplash.com/@davidaler?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) / 插图及拼接由作者制作

*气候变化的影响在全球变得越来越明显，从毁灭性的野火到创纪录的热浪和飓风。因此，越来越多的人在寻求减少碳足迹和帮助缓解气候变化影响的方法。然而，很难知道从何处开始或如何做出有意义的改变。我们介绍了一种使用开放和链接数据方案建模不同活动碳足迹的新方法。* [*相关研究文章*](https://borisruf.github.io/carbon-footprint-modeling-tool/ICREC2022_Open_and_Linked_Data_Model_for_Carbon_Footprint_Scenarios.pdf) *将于今年晚些时候在* [*Energy Reports*](https://www.sciencedirect.com/journal/energy-reports) *期刊上发表。*

随着气候危机的持续展开，自然灾害发生频率增加，减少温室气体（GHGs）的紧迫性不断加大。其中一个关键挑战是理解我们日常活动对环境的影响。然而，识别个人减少潜力可能复杂且难以理解，因为隐性、嵌入式排放常常发生在其他地方且难以可视化。

近年来，你可能见过有关日常活动如接收电子邮件或观看电影的碳足迹的惊人插图。这些比较旨在让消费者对像碳排放这样的抽象概念有所了解。它们提高了意识，鼓励人们采取行动减少碳足迹。然而，为了做出明智的决策，验证这些说法的准确性至关重要。

![](../Images/cd98ba49af6f3f8031af82d8ee4d9d34.png)

[Adriano Becker](https://unsplash.com/@afbecker?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# 假新闻？

有一种说法认为，接收一年的电子邮件产生的碳排放量与驾驶一辆普通汽车行驶200英里相同。不幸的是，[这一说法并没有经过严格的分析，并且存在重大舍入错误](https://qz.com/1937309/dont-worry-about-the-carbon-footprint-of-your-emails)。作者后来与这一说法脱离了关系。

另一个流行的说法认为，观看仅30分钟的Netflix相当于驾驶近4英里。然而，[这一说法也发现了重大错误](https://www.iea.org/commentaries/the-carbon-footprint-of-streaming-video-fact-checking-the-headlines)，包括换算错误和未考虑终端设备的碳足迹。

显然，评估和讨论碳足迹情景需要可靠的信息，上述事件突显了透明和严格的方法论的必要性。

# 背景很重要

那么，为什么很难对看似基本的日常活动得到可靠的答案呢？考虑以下问题：我能开车行驶多远才能排放与长途飞行相同的碳量？在法国或德国从电网充电电动车——有什么区别？选择白肉而不是红肉，在碳足迹方面有什么好处？通过VoIP电话（如WhatsApp）与普通手机通话相比，如何？

这些情境下的计算不仅受复杂技术规格的影响，还受到个体因素的影响，这些因素可能根据情境的不同而大相径庭。例如，与飞行相比，你的汽车燃油消耗扮演着重要角色。电动车充电时，本地电网的能源组合是一个关键因素。例如，[2021年法国电力部门的碳强度估计为每千瓦时58克CO₂，而德国为349克。](https://www.statista.com/statistics/1291750/carbon-intensity-power-sector-eu-country/)

因此，创建针对特定情境的强健排放情景至关重要。这对于进行有力比较和制定明确且可信的建议是基础。

# 数据模型

为应对这一挑战，我们提出了一种通用碳足迹情境数据模型，以提高碳足迹数据的可访问性和实用性。该模型旨在开放、链接和模块化，便于共享和重用。模型以JSON格式表示，包含几个实体，每个实体都有一个属性列表及其数据类型。实体包括*Scenario*、*Scope*、*Component*、*Link*、*Source*、*Emission*、*Consumer*和*Consumption*，每个实体都有自己独特的属性及与其他实体的关系。

![](../Images/65a2a04082fab1834a2eab7d2b93f9e4.png)

自指碳足迹情境的数据模型（可选属性为*斜体*）。图像由作者提供。

任何**场景**都由统一资源标识符（URI）标识。它具有一个标题，标题中可选地包括一个超链接形式的参考。一个场景涵盖1到3个**排放范围**，这些范围由[温室气体议定书](https://ghgprotocol.org/sites/default/files/standards/ghg-protocol-revised.pdf)定义。该实体可能包含描述。此外，它包括1个或多个**组件**或**链接**。后者简单地与另一个通过其URI标识的场景相关联。它还可以附有数量的指示。然而，组件必须包括数量和数量单位（例如，“km”，“kg”，“pcs”）。它还可以有一个定义消费者类型的类别（例如，“汽车”，“食品”，“电子产品”）。此外，该实体必须包括一个**来源**，它具有一个名称和类型（例如，“法国电网”和“电力”，或“优质汽油”和“汽油”）。它还可以包含描述（例如，“2022年”）。

任何来源必须包括1个或多个**排放量**，这些排放量以键值对的形式实现。排放类型定义了键（“co2e”，“co2”，“ch4”，“n2o”，“hfcs”，“pfcs”，“sf6”，“nf33”）。值、单位（例如，“g”，“kg”，“t”）和基本单位（例如，“kWh”，“l”，“kg”，“km”）指定了排放细节。可以通过超链接公开此信息的来源。组件也可以有一个**消费者**，当有关能源效率的信息可用时（例如，食品的排放通常仅按生产1公斤来报告）。消费者有一个名称（例如，“波音747”，“iPhone 14”），并可以有一个可选的描述。它还包括1个或多个**消费量**（某些消费者可能支持多种能源来源，例如，可以用不同类型的汽油加油的内燃机汽车；混合动力车使用汽油和电力），这些消费量以键值对的形式存在。能源类型定义了键（例如，“电力”，“汽油”）并与来源类型相对应。值、单位（例如，“kWh”，“l”）和基本单位（例如，“km”，“h”，“d”）指定了消费细节。这些信息可以通过参考进行补充。

这是一个简单的数据示例：

```py
{
  "title": "Mobility",
  "scopes": [
    {
      "level": "Scope 1",
      "list": [
        {
          "type": "component",
          "consumer": {
            "name": "Volkswagen Golf (2014)",
            "description": "Engine ID 45, 4 cylinders, Manual 6-spd",
            "consumptions": {
              "diesel": {
                "value": "0.0735046875",
                "unit": "l",
                "base_unit": "km",
                "reference_url": "https://www.fueleconomy.gov/"
              }
            }
          },
          "quantity": "10000",
          "quantity_unit": "km",
          "source": {
            "name": "Gas/Diesel oil",
            "type": "diesel",
            "emissions": {
              "co2e": {
                "value": "3.25",
                "unit": "kg",
                "base_unit": "l",
                "reference_url": "https://bilansges.ademe.fr/index.htm?new_liquides.htm"
              }
            }
          }
        }
      ]
    }
  ]
}
```

# *我需要一个翻译员*

为了使数据生动，我们开发了一个基于网络的查看器，可以解释和可视化这种数据格式。该应用程序按元素和范围聚合排放数据，进行单位转换，并基于不同类型排放数据的可用性找到共同点（例如，“CO2e”，“CO2”）。在用户界面中，用户可以立即调整数据，通过调整每个元素的数量并即时连接不同的数据源来替换消费者组件和能源来源。

自定义场景可以作为JSON文件下载并通过URL共享，这使得场景协作变得简单。最后，一个 [基准视图](https://borisruf.github.io/carbon-footprint-modeling-tool/benchmark.html?ids%5B%5D=scenario-615e4199-28fe-43d4-8b30-3cee5fe18923) 使用户能够通过标识符比较两个或多个场景。

该Web应用程序是使用JavaScript构建的，并作为GitHub页面部署，这使得部署和更新变得容易。我们数据模型的自我引用结构还允许以分布式方式托管嵌套场景。数据解释器能够递归地获取和处理这些场景，从而在分布式环境中访问和分析复杂的碳足迹模型成为可能。

![](../Images/d0f7d0a9e402712bb7e3ab5731d7dae7.png)

基于网页的数据解释器在查看模式下。图片由作者提供。

关于如何使用和部署该应用程序的详细文档可以在我们的 [GitHub 仓库](https://github.com/borisruf/carbon-footprint-modeling-tool) 中找到。在这里，你还可以访问源代码，以及演示和多个示例场景：

+   [流媒体播放30分钟视频（IEA更新，英国）](https://borisruf.github.io/carbon-footprint-modeling-tool/?id=scenario-8f35af7c-ee5b-42aa-b538-371b126b3d24)

+   [流媒体播放30分钟视频（IEA更新，笔记本电脑和高清）](https://borisruf.github.io/carbon-footprint-modeling-tool/?id=scenario-725b3ff2-294b-4cfc-81a3-fc460ee61fdc)

+   [新加坡（SIN）到巴黎（CDG）往返](https://borisruf.github.io/carbon-footprint-modeling-tool/?id=scenario-0065da59-7785-4eed-8a11-c73b70cf798e)

+   [示例个人碳足迹（嵌套）](https://borisruf.github.io/carbon-footprint-modeling-tool/?id=scenario-265789b8-d7d1-442f-ba79-e627b9226c86)

+   [货船排放组合](https://borisruf.github.io/carbon-footprint-modeling-tool/?id=scenario-d9de099f-a408-4526-aec2-f781c9972b42)

+   [ChatGPT的月度操作](https://borisruf.github.io/carbon-footprint-modeling-tool/index.html?id=scenario-a5f20019-1f40-4f41-b2a7-f9db5346a23f)

我们的方法旨在提高碳足迹场景的透明度、可访问性和可探索性，使个人能够做出关于其行为环境影响的知情决策。

# TL;TR

我们提出了一种新颖的数据模型，用于生成可以适应本地或个人情况的碳足迹场景。我们的查看器应用展示了用户如何增强对不同活动相关碳排放的理解，从而做出更明智的选择。用户可以实时操作数据，观察更换不同组件（例如使用替代能源或减少特定材料的数量）对整体碳足迹的影响。此外，该应用还便于对不同场景进行并排比较，使用户能够评估其差异。应用程序的另一个重要功能是能够轻松共享场景，促进合作与知识共享，从而减少碳排放。

附言：根据我的假设，[从巴黎飞往新加坡的长途航班相当于驾驶我的汽车行驶3,200公里。](https://borisruf.github.io/carbon-footprint-modeling-tool/benchmark.html?ids%5B%5D=scenario-e6c0c4d7-b9fb-4d76-bba0-5c6e4975dcf7)

*B. Ruf 和 M. Detyniecki (2022)，《碳足迹场景的开放与联接数据模型》，第七届国际可再生能源与节能大会 (ICREC)，法国巴黎。*
