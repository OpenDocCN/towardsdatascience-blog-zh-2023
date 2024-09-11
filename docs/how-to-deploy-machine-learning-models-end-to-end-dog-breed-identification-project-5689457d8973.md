# 如何部署机器学习模型？端到端犬种识别项目！

> 原文：[https://towardsdatascience.com/how-to-deploy-machine-learning-models-end-to-end-dog-breed-identification-project-5689457d8973?source=collection_archive---------17-----------------------#2023-04-03](https://towardsdatascience.com/how-to-deploy-machine-learning-models-end-to-end-dog-breed-identification-project-5689457d8973?source=collection_archive---------17-----------------------#2023-04-03)

## 将你的 ML 模型在网络上部署的最简单方法。

[](https://medium.com/@gkeretchashvili?source=post_page-----5689457d8973--------------------------------)[![Gurami Keretchashvili](../Images/4da78f113a0046c2deb8224e09dd9e3d.png)](https://medium.com/@gkeretchashvili?source=post_page-----5689457d8973--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5689457d8973--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5689457d8973--------------------------------) [Gurami Keretchashvili](https://medium.com/@gkeretchashvili?source=post_page-----5689457d8973--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba1f382fdaca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-machine-learning-models-end-to-end-dog-breed-identification-project-5689457d8973&user=Gurami+Keretchashvili&userId=ba1f382fdaca&source=post_page-ba1f382fdaca----5689457d8973---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5689457d8973--------------------------------) ·9 分钟阅读·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5689457d8973&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-machine-learning-models-end-to-end-dog-breed-identification-project-5689457d8973&user=Gurami+Keretchashvili&userId=ba1f382fdaca&source=-----5689457d8973---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5689457d8973&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-machine-learning-models-end-to-end-dog-breed-identification-project-5689457d8973&source=-----5689457d8973---------------------bookmark_footer-----------)

# **I. 介绍**

在本文中，我将逐步讨论使用 Streamlit 将 ML 项目部署到网络上最简单最快的方法。该项目涉及犬种识别，可以将狗狗分类为 120 种犬种之一。我将更多地关注项目的部署部分，而不是构建复杂的机器学习模型。

在进一步讨论之前，让我们先看一下项目的演示：

![](../Images/89db508f88f224db73a88ad654bbe646.png)

部署演示（作者提供的 gif）

你可以通过演示体验 — [**这里**](https://gurokeretcha-dog-breed-identifier-streamlit-7itzp9.streamlit.app/?fbclid=IwAR0qdSoSi9_vXFYbggB44sZI4lbUjHcnUxOpBVTCBBJ8-Nrg6tr5tWmh_iI)。

项目的 GitHub 链接在这里-[**这里**](https://github.com/gurokeretcha/Dog-Breed-Identifier)

## **文章大纲：**

+   **背景**

+   **项目教程** A. 构建和训练模型 (Build_AND_Save_DL_model.ipynb)

    B. Streamlit 应用 (streamlit.py)

    C. 部署

+   **常见错误及故障排除**

+   **结论和未来工作**

# **II. 背景**

在 Jupyter notebook 中构建机器学习模型是一回事，而部署模型则是另一回事，需要创建一个服务，供其他用户使用…
