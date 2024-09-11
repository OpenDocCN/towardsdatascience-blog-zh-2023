# 理解机器学习中的噪声数据和不确定性

> 原文：[https://towardsdatascience.com/understanding-noisy-data-and-uncertainty-in-machine-learning-4a2995a84198?source=collection_archive---------8-----------------------#2023-01-23](https://towardsdatascience.com/understanding-noisy-data-and-uncertainty-in-machine-learning-4a2995a84198?source=collection_archive---------8-----------------------#2023-01-23)

## 你的机器学习模型未能工作的实际原因

[](https://harrisonfhoffman.medium.com/?source=post_page-----4a2995a84198--------------------------------)[![Harrison Hoffman](../Images/5eaa3e2bd0507297eb6c4a7efcf06324.png)](https://harrisonfhoffman.medium.com/?source=post_page-----4a2995a84198--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a2995a84198--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4a2995a84198--------------------------------) [Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----4a2995a84198--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-noisy-data-and-uncertainty-in-machine-learning-4a2995a84198&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----4a2995a84198---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a2995a84198--------------------------------) · 9分钟阅读 · 2023年1月23日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a2995a84198&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-noisy-data-and-uncertainty-in-machine-learning-4a2995a84198&source=-----4a2995a84198---------------------bookmark_footer-----------)

人工智能和机器学习领域比以往任何时候都更受关注。像 Chat GPT 和 Stable Diffusion 这样的模型掀起了全球热潮，AI 和 ML 的炒作卷土重来，吸引了[大众](https://www.google.com/search?tbm=vid&sxsrf=AJOqlzXI5TC9wmd1MTAKaqmrRY_q6b0k3Q%3A1674266462280&q=chatgpt&spell=1&sa=X&ved=2ahUKEwiz-_jNyNf8AhUZk2oFHY1kCZoQBSgAegQIERAB&biw=1185&bih=714&dpr=2)的关注。面对这些炒作，我们必须提醒自己机器学习成功的真正主宰是谁——高质量的数据。

在缺乏高质量训练数据的情况下，监督学习模型毫无用处。不幸的是，大多数实际的数据科学项目失败是因为在完全理解数据源的质量之前，对模型性能有不切实际的期望。本文将尝试提供关于噪声数据的直觉，以及为什么机器学习模型无法表现良好的原因。我们将探讨监督学习和确定性函数的本质、不同类型的模型不确定性，并讨论减轻这种不确定性和管理期望的方法。

# 监督学习公式

从根本上讲，监督学习就是函数逼近。我们向模型提供一些输入（X）和目标（y），并期望模型通过优化目标函数来逼近（或学习）一个将 X 映射到 y 的函数。更正式地说，监督学习的公式如下：
