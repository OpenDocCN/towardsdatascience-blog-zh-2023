# 如何编码周期性时间特征

> 原文：[https://towardsdatascience.com/how-to-encode-periodic-time-features-7640d9b21332?source=collection_archive---------9-----------------------#2023-08-24](https://towardsdatascience.com/how-to-encode-periodic-time-features-7640d9b21332?source=collection_archive---------9-----------------------#2023-08-24)

## 对于深度学习和其他预测模型，周日期和一天中的时间的深思熟虑的处理。

[](https://medium.com/@christoph.oliver.moehl?source=post_page-----7640d9b21332--------------------------------)[![Christoph Möhl](../Images/fa254d72929c710f11bda8f760f43453.png)](https://medium.com/@christoph.oliver.moehl?source=post_page-----7640d9b21332--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7640d9b21332--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7640d9b21332--------------------------------) [Christoph Möhl](https://medium.com/@christoph.oliver.moehl?source=post_page-----7640d9b21332--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bd469d8e345&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-encode-periodic-time-features-7640d9b21332&user=Christoph+M%C3%B6hl&userId=5bd469d8e345&source=post_page-5bd469d8e345----7640d9b21332---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7640d9b21332--------------------------------) ·5分钟阅读·2023年8月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7640d9b21332&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-encode-periodic-time-features-7640d9b21332&user=Christoph+M%C3%B6hl&userId=5bd469d8e345&source=-----7640d9b21332---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7640d9b21332&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-encode-periodic-time-features-7640d9b21332&source=-----7640d9b21332---------------------bookmark_footer-----------)![](../Images/96d47e7431f8e45e5865c4fbad063b2f.png)

作者插图

许多预测任务需要时间信息作为模型输入。比如考虑一个回归模型来预测零售公司柠檬水的销售（你可能记得我在[关于上下文增强特征的文章](/context-enriched-data-the-secret-superpower-for-your-deep-learning-model-549826a5fb3d)中提到过这个例子）。显然，夏季对清凉饮料的需求更高，导致销售曲线呈周期性变化，在七月/八月达到峰值（这里考虑的是欧洲的一个地点）。

![](../Images/66e003f048a08b48dbf0781e166c64b1.png)

在这种情况下，年份中的时间显然是我们应该输入模型的有价值的季节性信息。但我们应该如何做呢？日期很复杂，天数因月份不同而变化（而对于二月甚至因年份不同而变化），且存在各种格式：

*2023年1月13日*

*2023年1月13日*

*2023/03/13*

首先，我们可以省略年份。为了考虑季节性影响，我们只需要日和月。在一种非常简单（且不太深思熟虑）的方法中，我们可以将月份作为一个数字，日期作为另一个数字输入。

![](../Images/492cc551748d167940e2a221629fdb42.png)

为什么这是一个坏主意？模型必须学习基督教格里高利历法的工作原理（每月约30天，每年12个月，闰年等）。有了足够的训练数据，深度学习模型当然能够“理解”我们的日历。“理解”在这里的意思是：模型可以从月份和日期输入中推断出年内的相对时间位置。但我们应该尽量使学习对我们的模型来说更简单，将这项工作交给我们自己（至少我们已经知道日历如何运作）。我们利用Python的*datetime*库，并用相当简单的逻辑计算年内的相对时间：

```py
from datetime import datetime
import calendar

year = 2023
month = 12
day = 30

passed_days = (datetime(year, month, day) - datetime(year, 1, 1)).days + 1
nr_of_days_per_year= 366 if calendar.isleap(year) else 365

position_within_year = passed_days / nr_of_days_per_year
```

结果的*postion_within_year*特征的值范围从接近*0.0*（*1月1日*）到*1.0*（*12月31日*），比（复杂的）格里高利历法更易于模型解释。

![](../Images/247ad8a5d1e33e94026eda6fc88cc4e6.png)

但这仍然不理想。*position_within_year*特征描述了一个“锯齿状”的模式，在每年转折时从*1.0*跳到*0.0*。这种急剧的不连续性可能对有效学习构成问题。*12月31日*和*1月1日*是非常相似的日期：它们是直接邻居，并且有许多共同点（例如类似的天气条件），它们可能具有类似的柠檬水销售潜力。然而，特征*position_within_year*并没有反映出*12月31日*和*1月1日*的这种相似性；事实上，它们的差异是最大的。

理想情况下，彼此接近的时间点应具有相似的时间值。我们必须以某种方式设计一个代表一年周期性特征的特征。换句话说，通过*12月31日*我们应该到达*1月1日*时的起始位置。因此，将年内的位置建模为圆上的位置当然是有意义的。我们可以通过将*position_within_year*转换为单位圆的*x*和*y*坐标来实现这一点。

为此我们使用*sine*和*cosine*函数：

*sin(α) = x*

*cos(α) = y*

其中*α*是施加于圆的角度。如果单位圆表示一年，*α*表示已过去的年内时间。

![](../Images/3c0c2139409a08c6124251e2f59c1f57.png)

*α*因此等同于*position_within_year*特征，唯一的区别是*α*有一个不同的尺度（*α*：*0.0*–*2π*¹，*position_within_year*：*0.0*-*1.0*）。

通过将*position_within_year*简单地缩放到*α*并计算*sine*和*cosine*，我们将“锯齿形”模式转换为具有平滑过渡的圆形表示。

```py
import math

# scale to 2pi (360 degrees)
alpha = position_within_year * math.pi * 2

year_circle_x = math.sin(alpha)
year_circle_y = math.cos(alpha)

# scale between 0 and 1 (original unit circle positions are between -1 and 1)
year_circle_x = (year_circle_x + 1) / 2
year_circle_y = (year_circle_y + 1) / 2

time_feature = (year_circle_x, year_circle_y) # so beautiful ;)
```

![](../Images/729a3d16d948f65464642b8d37631afc.png)

生成的*time_feature*是一个两个元素的向量，范围在*0*到*1*之间，易于你的预测模型处理。通过几行代码，我们将大量不必要的学习工作从模型的肩膀上移除。

单位圆模型可以应用于任何周期性的时间信息，例如**月份的天数**、**星期几**、**一天中的时间**、小时的分钟等。这个概念也可以扩展到时间域之外的周期特征。

+   **物流/公共交通**：公交车在城市往返行程中的相对位置

![](../Images/710e1b4c6526430b88db706bb035c5bd.png)

+   **生物学**：细胞在[细胞周期](https://en.wikipedia.org/wiki/Cell_cycle)中的状态。

+   你有其他的使用案例吗？欢迎留言！

## 进一步的信息 / 连接点

+   Pierre-Luis Bescond写了一篇关于相同主题的精彩[实践文章](/cyclical-features-encoding-its-about-time-ce23581845ca)。

+   你想了解更多关于深度学习模型的特征工程吗？查看我的[关于上下文增强数据的文章](https://medium.com/towards-data-science/context-enriched-data-the-secret-superpower-for-your-deep-learning-model-549826a5fb3d)。

+   你有问题吗？你需要一位[**人工智能、数据科学、数据工程或Python开发领域的自由专家**](https://www.moehl-data-services.de/en)吗？访问[我的网站](https://moehl-data-services.de/en)并给我留言。

[1] 这里的角度以弧度表示。*0*弧度对应于*0°*，*2π*弧度对应于360°。

所有图形均由作者创建。
