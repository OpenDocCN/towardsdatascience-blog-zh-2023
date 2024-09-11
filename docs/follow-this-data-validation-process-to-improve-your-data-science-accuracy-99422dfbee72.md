# 遵循此数据验证过程以提高数据科学的准确性

> 原文：[https://towardsdatascience.com/follow-this-data-validation-process-to-improve-your-data-science-accuracy-99422dfbee72?source=collection_archive---------5-----------------------#2023-09-01](https://towardsdatascience.com/follow-this-data-validation-process-to-improve-your-data-science-accuracy-99422dfbee72?source=collection_archive---------5-----------------------#2023-09-01)

## 当训练和推断数据来自不同来源时

[](https://datascience2.medium.com/?source=post_page-----99422dfbee72--------------------------------)[![Matt Przybyla](../Images/3b9e714e012e8846b866e4e4b5d689d7.png)](https://datascience2.medium.com/?source=post_page-----99422dfbee72--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99422dfbee72--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----99422dfbee72--------------------------------) [Matt Przybyla](https://datascience2.medium.com/?source=post_page-----99422dfbee72--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fabe5272eafd9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffollow-this-data-validation-process-to-improve-your-data-science-accuracy-99422dfbee72&user=Matt+Przybyla&userId=abe5272eafd9&source=post_page-abe5272eafd9----99422dfbee72---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99422dfbee72--------------------------------) · 8 分钟阅读 · 2023年9月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99422dfbee72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffollow-this-data-validation-process-to-improve-your-data-science-accuracy-99422dfbee72&user=Matt+Przybyla&userId=abe5272eafd9&source=-----99422dfbee72---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99422dfbee72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffollow-this-data-validation-process-to-improve-your-data-science-accuracy-99422dfbee72&source=-----99422dfbee72---------------------bookmark_footer-----------)![](../Images/87e3892c3b9c9962ecad60b869f4c936.png)

[NordWood Themes](https://unsplash.com/@nordwood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片，发布在 [Unsplash](https://unsplash.com/photos/E9tFH39iRPE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [1]。

# 目录

1.  介绍

1.  启用数据收集

1.  设定基准

1.  检测异常值

1.  摘要

1.  参考文献

# 介绍

本文旨在帮助那些刚入门或希望改进现有数据验证过程的数据科学家，作为一个概述并附带一些示例。首先，我想在这里定义数据验证，因为对于其他类似的职位角色，它可能有不同的含义。为了本文的目的，我们将数据验证定义为确保用于模型的训练数据与推理数据相匹配或一致的过程。对于某些公司和使用场景，如果数据来自相同来源，你可能无需担心这个问题。因此，这个过程必须进行，并且只有在数据来自不同来源时才有用。数据可能不会来自相同来源的一些原因包括，如果你的训练数据是历史数据并且是定制的（*例如：从现有数据中衍生出的特征*），和/或你的推理数据来源于...
