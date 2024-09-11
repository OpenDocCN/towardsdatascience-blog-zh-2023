# 面向文档的代理：与向量数据库、LLMs、Langchain、FastAPI 和 Docker 的旅程

> 原文：[https://towardsdatascience.com/document-oriented-agents-a-journey-with-vector-databases-llms-langchain-fastapi-and-docker-be0efcd229f4?source=collection_archive---------1-----------------------#2023-07-05](https://towardsdatascience.com/document-oriented-agents-a-journey-with-vector-databases-llms-langchain-fastapi-and-docker-be0efcd229f4?source=collection_archive---------1-----------------------#2023-07-05)

## 利用 ChromaDB、Langchain 和 ChatGPT：从大型文档数据库中获取增强的响应和引用来源

[](https://medium.com/@luisroque?source=post_page-----be0efcd229f4--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----be0efcd229f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be0efcd229f4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----be0efcd229f4--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----be0efcd229f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdocument-oriented-agents-a-journey-with-vector-databases-llms-langchain-fastapi-and-docker-be0efcd229f4&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----be0efcd229f4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be0efcd229f4--------------------------------) · 11分钟阅读 · 2023年7月5日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe0efcd229f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdocument-oriented-agents-a-journey-with-vector-databases-llms-langchain-fastapi-and-docker-be0efcd229f4&source=-----be0efcd229f4---------------------bookmark_footer-----------)

# 介绍

面向文档的代理在商业领域开始获得关注。公司越来越多地利用这些工具来利用内部文档，提升其业务流程。最近的一份麦肯锡报告[1]强调了这一趋势，指出生成式人工智能可能每年为全球经济带来2.6万亿至4.4万亿美元的增长，并将自动化目前70%的工作活动。研究指出，客户服务、销售和营销以及软件开发是将受到转型影响的主要领域。大多数变化来自于公司内部推动这些领域的信息可以通过使用如面向文档的代理这样的解决方案变得对员工和客户更为可及。

目前的技术仍面临一些挑战。即使考虑到新的大语言模型（LLMs）具有10万个标记的限制，这些模型的上下文窗口仍然有限。虽然10万个标记看似一个很大的数字，但当我们考虑到驱动例如客户服务部门的数据库的规模时，这个数字则显得微不足道。另一个常见的问题是模型输出的准确性。在本文中，我们将…
