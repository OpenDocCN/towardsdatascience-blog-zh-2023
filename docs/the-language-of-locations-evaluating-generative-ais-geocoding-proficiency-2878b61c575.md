# 位置语言：评估生成式 AI 的地理编码能力

> 原文：[https://towardsdatascience.com/the-language-of-locations-evaluating-generative-ais-geocoding-proficiency-2878b61c575?source=collection_archive---------2-----------------------#2023-09-13](https://towardsdatascience.com/the-language-of-locations-evaluating-generative-ais-geocoding-proficiency-2878b61c575?source=collection_archive---------2-----------------------#2023-09-13)

## 一个应用项目，详细介绍了大型语言模型在地理编码中的表现，并与现代地理编码 API 进行了比较

[](https://medium.com/@lukezaruba?source=post_page-----2878b61c575--------------------------------)[![Luke Zaruba](../Images/84394ce71e12a3416e8dad471d891253.png)](https://medium.com/@lukezaruba?source=post_page-----2878b61c575--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2878b61c575--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2878b61c575--------------------------------) [Luke Zaruba](https://medium.com/@lukezaruba?source=post_page-----2878b61c575--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F55d98275790e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-language-of-locations-evaluating-generative-ais-geocoding-proficiency-2878b61c575&user=Luke+Zaruba&userId=55d98275790e&source=post_page-55d98275790e----2878b61c575---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2878b61c575--------------------------------) · 9分钟阅读 · 2023年9月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2878b61c575&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-language-of-locations-evaluating-generative-ais-geocoding-proficiency-2878b61c575&user=Luke+Zaruba&userId=55d98275790e&source=-----2878b61c575---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2878b61c575&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-language-of-locations-evaluating-generative-ais-geocoding-proficiency-2878b61c575&source=-----2878b61c575---------------------bookmark_footer-----------)![](../Images/27a0f4f9061deff45288050a6fe0266f.png)

图片由 [Sylwia Bartyzel](https://unsplash.com/@sylwiabartyzel?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现代科技的便利性在于，你可以在手机的搜索栏中输入一个位置，看到它自动显示在地图上，这种功能往往被视为理所当然。但你是否曾经停下来想过，这种文本与地图之间的无缝交互和转换到底是如何工作的？这个问题的答案就是地理编码。

Esri，全球地理空间软件领域的领导者，将地理编码定义为 “[将位置描述（如坐标对、地址或地点名称）转换为地球表面上的位置的过程。](https://desktop.arcgis.com/en/arcmap/latest/manage-data/geocoding/what-is-geocoding.htm)” 地理编码是赋予导航应用、地图服务和地理信息科学（GIScience）平台高级功能的基础。Esri、Google、Mapbox 等各种提供商都提供地理编码 API，能够接受位置描述并返回相应的纬度和经度值或坐标，使我们能够从空间角度思考数据。

随着生成型 AI 和大型语言模型（LLMs）的兴起，如 OpenAI 的 [GPT](https://openai.com/gpt-4)、Google 的 [Bard](https://bard.google.com/) 或 Meta 的 [LLaMA](https://ai.meta.com/llama/)，为地理空间应用技术的使用带来了极好的机会。这些应用范围广泛，从使用 GitHub 的 [Copilot](https://github.com/features/copilot) 进行代码生成，到使用 Meta 的 [Segment Anything Model](https://segment-anything.com/)（SAM）进行图像分割，甚至可能涉及地理编码。

本文将考察利用“开箱即用”的生成型 AI 进行地理编码非结构化位置描述的适用性。此评估将在来自明尼苏达州的小型车辆事故数据集上进行。通过这项分析，将探索标准 LLMs 在地理编码任务中的有效性及生成型 AI 在地理空间中的潜在应用案例，相较于现有的传统方法。

**理解地理编码及其与 AI 的集成** 现代地理编码器由两个基本组件组成，即参考数据集和地理编码算法。参考数据通常包含附加到地理位置的地方的明确和相对描述，这意味着不仅是诸如地址等明确描述与位置相关联，而且地方的非结构化描述也与位置相关联。然后，可以使用匹配算法来找到输入描述与参考数据集中包含的描述之间的合适匹配。一个简单的匹配算法示例可能是使用插值算法，通过估算两个已知地址之间的位置来确定街道地址的位置。

预测性地理编码的概念，利用AI和机器学习来增强地理编码过程，已经有了悠久的历史。包括自然语言处理（NLP）和深度学习在内的技术已经被提出和利用，取得了不同程度的成功。AI和机器学习在地理编码中的应用并不是近期发展。然而，生成性AI的出现为地理编码带来了新的前沿，就像它对许多其他领域一样。

**应对挑战与探索未来机会** 正如你所知，LLMs使用从互联网、书籍、期刊文章以及其他各种来源中提取的大量文本数据进行训练。这些数据通常，甚至总是，缺乏全面的地理空间信息。这种地理空间训练数据的缺乏对LLMs在理解和解决地理空间挑战方面的潜力和适用性有影响。在没有基础领域特定知识的情况下，我们如何期望一个模型在复杂问题上表现良好？

答案是我们根本做不到。

在本分析中，我评估了LLMs作为单独基准的适用性，以及在使用传统GIScience方法的工作流程中的适用性。结果强调了一个熟悉的观点——虽然新技术可能令人印象深刻，但在解决复杂挑战时，它并不总能带来性能提升。

**案例研究：车祸的非结构化位置描述**

**数据收集与准备** 为了测试和量化LLMs的地理编码能力，从一个从[网站](https://app.dps.mn.gov/MSPMedia2/)抓取的数据集中随机选择了100个明尼苏达州车辆事故的非结构化位置描述。所有100个事故的真实坐标通过使用各种地图应用程序，如[谷歌地图](https://www.google.com/maps)和明尼苏达交通部的[交通地图应用（TMA）](https://mndot.maps.arcgis.com/apps/webappviewer/index.html?id=7b3be07daed84e7fa170a91059ce63bb)，手动创建。

一些示例位置描述如下所示。

> 美国71号公路与明尼苏达60号公路交汇处，温多姆，棉花县
> 
> EB 10号高速公路靠近Joplin St NW，厄尔克河，舍伯恩县
> 
> EB I 90 / HWY 22，福斯特镇，法里博县
> 
> 高速公路75号里程碑403，圣文森特镇，基特森县
> 
> 65号高速公路 / 国王路，布伦瑞克镇，卡纳贝克县

如上述示例所示，描述的结构以及定义位置的方式有多种可能性。例如，第四个描述包含一个里程标记号，这在任何地理编码过程中都不太可能匹配，因为该信息通常不包含在任何参考数据中。像这样的描述的真实坐标的确定很大程度上依赖于明尼苏达州交通部的线性参考系统（LRS），该系统提供了一种标准化的方法来测量全州的道路，其中里程标记起着重要作用。此数据可以通过前面提到的 TMA 应用程序访问。

**方法论与地理编码策略** 在准备数据后，设置了五个单独的笔记本来测试不同的地理编码过程。它们的配置如下。

> 1\. Google 地理编码 API，用于原始位置描述
> 
> 2\. Esri 地理编码 API，用于原始位置描述
> 
> 3\. Google 地理编码 API，用于 OpenAI GPT 3.5 标准化位置描述
> 
> 4\. Esri 地理编码 API，用于 OpenAI GPT 3.5 标准化位置描述
> 
> 5\. OpenAI GPT 3.5，作为地理编码器本身使用

总结来说，Google 和 Esri 的地理编码 API 被用于原始描述以及使用传递给 OpenAI GPT 3.5 模型的简短提示进行标准化的描述。这个标准化过程的 Python 代码可以在下面看到。

```py
def standardize_location(df, description_series):
    df["ai_location_description"] = df[description_series].apply(_gpt_chat)

    return df

def _gpt_chat(input_text):
    prompt = """Standardize the following location description into text
             that could be fed into a Geocoding API. When responding, only
             return the output text."""

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": prompt},
            {"role": "user", "content": input_text},
        ],
        temperature=0.7,
        n=1,
        max_tokens=150,
        stop=None,
    )

    return response.choices[0].message.content.strip().split("\n")[-1]
```

使用地理编码 API 的四个测试案例使用了下面的代码来向各自的地理编码器发出 API 请求，并返回所有 100 个描述的结果坐标。

```py
# Esri Geocoder
def geocode_esri(df, description_series):
    df["xy"] = df[description_series].apply(
        _single_esri_geocode
    )

    df["x"] = df["xy"].apply(
        lambda row: row.split(",")[0].strip()
    )
    df["y"] = df["xy"].apply(
        lambda row: row.split(",")[1].strip()
    )

    df["x"] = pd.to_numeric(df["x"], errors="coerce")
    df["y"] = pd.to_numeric(df["y"], errors="coerce")

    df = df[df["x"].notna()]
    df = df[df["y"].notna()]

    return df

def _single_esri_geocode(input_text):
    base_url = "https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/findAddressCandidates"
    params = {
        "f": "json",
        "singleLine": input_text,
        "maxLocations": "1",
        "token": os.environ["GEOCODE_TOKEN"],
    }

    response = requests.get(base_url, params=params)

    data = response.json()

    try:
        x = data["candidates"][0]["location"]["x"]
        y = data["candidates"][0]["location"]["y"]

    except:
        x = None
        y = None

    return f"{x}, {y}"
```

```py
# Google Geocoder
def geocode_google(df, description_series):
    df["xy"] = df[description_series].apply(
        _single_google_geocode
    )

    df["x"] = df["xy"].apply(
        lambda row: row.split(",")[0].strip()
    )
    df["y"] = df["xy"].apply(
        lambda row: row.split(",")[1].strip()
    )

    df["x"] = pd.to_numeric(df["x"], errors="coerce")
    df["y"] = pd.to_numeric(df["y"], errors="coerce")

    df = df[df["x"].notna()]
    df = df[df["y"].notna()]

    return df

def _single_google_geocode(input_text):
    base_url = "https://maps.googleapis.com/maps/api/geocode/json"
    params = {
        "address": input_text,
        "key": os.environ["GOOGLE_MAPS_KEY"],
        "bounds": "43.00,-97.50 49.5,-89.00",
    }    

    response = requests.get(base_url, params=params)

    data = response.json()

    try:
        x = data["results"][0]["geometry"]["location"]["lng"]
        y = data["results"][0]["geometry"]["location"]["lat"]

    except:
        x = None
        y = None

    return f"{x}, {y}"
```

此外，测试的一个最终过程是将 GPT 3.5 作为地理编码器本身使用，而不借助任何地理编码 API。这个过程的代码与上述标准化代码几乎相同，但使用了不同的提示，见下文。

> 对以下地址进行地理编码。尽可能准确地返回纬度（Y）和经度（X）。回复时，只返回以下格式的输出文本：X, Y

**性能指标和洞察**

在开发了各种过程之后，每个过程都被运行，并计算了多个性能指标，包括执行时间和地理编码准确性。这些指标列在下面。

```py
 |  Geocoding Process  |  Mean  | StdDev |  MAE   |  RMSE  |
        | ------------------- | ------ | ------ | ------ | ------ |
        | Google with GPT 3.5 | 0.1012 | 1.8537 | 0.3698 | 1.8565 |
        | Google with Raw     | 0.1047 | 1.1383 | 0.2643 | 1.1431 |
        | Esri with GPT 3.5   | 0.0116 | 0.5748 | 0.0736 | 0.5749 |
        | Esri with Raw       | 0.0001 | 0.0396 | 0.0174 | 0.0396 |
        | GPT 3.5 Geocoding   | 2.1261 | 80.022 | 45.416 | 80.050 |
```

```py
 |  Geocoding Process  | 75% ET | 90% ET | 95% ET | Run Time |
       | ------------------- | ------ | ------ | ------ | -------- |
       | Google with GPT 3.5 | 0.0683 | 0.3593 | 3.3496 | 1m 59.9s |
       | Google with Raw     | 0.0849 | 0.4171 | 3.3496 | 0m 23.2s |
       | Esri with GPT 3.5   | 0.0364 | 0.0641 | 0.1171 | 2m 22.7s |
       | Esri with Raw       | 0.0362 | 0.0586 | 0.1171 | 0m 51.0s |
       | GPT 3.5 Geocoding   | 195.54 | 197.86 | 199.13 | 1m 11.9s |
```

这里对指标进行了更详细的解释。均值代表均误差（以曼哈顿距离计算，即与真实值的X和Y差异的总和，以十进制度数表示）。标准差代表误差的标准偏差（以曼哈顿距离计算，以十进制度数表示）。MAE代表均方绝对误差（以曼哈顿距离计算，以十进制度数表示）。RMSE代表均方根误差（以曼哈顿距离计算，以十进制度数表示）。75%、90%、95% ET代表该百分比的误差阈值（以欧几里得距离计算，以十进制度数表示），意味着对于给定的百分比，该百分比的记录落在结果值与真实值的距离之内。最后，运行时间简单地表示在100条记录上运行地理编码过程所花费的总时间。

很明显，GPT 3.5单独表现远不如预期。虽然，如果排除几个被模型标记为位于其他大陆的异常值，大多数结果在视觉上至少看起来并不太突兀。

同时，看到LLM标准化过程实际上降低了准确性也颇为有趣，我个人对此感到有些惊讶，因为我引入该组件的全部意图是希望稍微提高地理编码过程的总体准确性。值得注意的是，提示本身可能是问题的一部分，并且值得进一步探索“提示工程”在地理空间背景中的作用。

从这次分析中得到的最后一个主要结论是执行时间差异，任何使用GPT 3.5的过程都明显较慢。Esri的地理编码API在这种设置下也比Google的慢。然而，严格的测试没有进行，因此这些结果应考虑这一点。

**结论**

尽管OpenAI的GPT 3.5模型的“开箱即用”地理编码能力可能无法与现代地理编码器的复杂性相匹配，但测试突显了一个潜在的有前景的展望。结果突显了显著的改进空间，暗示在可预见的未来，大型语言模型（LLMs）的地理空间能力有很多提升的机会，并最终对我们所知的地理编码产生影响。

在许多特定用例中，LLMs可能足以进行地理编码。然而，正如这个例子所示，LLMs的能力与需要高精度和细致空间分辨率的地理编码任务之间存在差距。因此，尽管LLMs具有潜力，但这个例子展示了某些应用对精度和准确性的关键要求。

总体而言，生成式人工智能展示出令人兴奋的创新，涵盖了地理和GIS领域的广泛和深远影响和机遇，包括地理编码的应用。每天都在以惊人的速度进行着持续的进展，允许在生成式人工智能与地理空间的集成开发上继续取得进展。

**参考文献：** [1] 维基百科贡献者，[地址地理编码](https://en.wikipedia.org/wiki/Address_geocoding) (2023)，维基百科

[2] A. Hassan，[地理空间人工智能的未来](https://medium.com/spatial-data-science/the-future-of-geospatial-ai-d29756df6ac3) (2023)，[空间数据科学](https://medium.com/spatial-data-science)

[3] L. Mearian，[LLMs 是什么，它们如何在生成式人工智能中使用？](https://www.computerworld.com/article/3697649/what-are-large-language-models-and-how-are-they-used-in-generative-ai.html) (2023)，[计算机世界](https://www.computerworld.com/)

**致谢：** 我要特别感谢Bryan Runck博士在编辑和审阅本文过程中提供的宝贵支持、指导和专业知识。
