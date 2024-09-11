# 好了，你已经训练了最好的机器学习模型。接下来是什么？

> 原文：[`towardsdatascience.com/okay-youve-trained-the-best-machine-learning-model-what-s-next-e7b8f167006e?source=collection_archive---------5-----------------------#2023-06-04`](https://towardsdatascience.com/okay-youve-trained-the-best-machine-learning-model-what-s-next-e7b8f167006e?source=collection_archive---------5-----------------------#2023-06-04)

## 数据科学

## 一个超越 Jupyter Notebook 模型的 MLOps 项目

[](https://dwiuzila.medium.com/?source=post_page-----e7b8f167006e--------------------------------)![Albers Uzila](https://dwiuzila.medium.com/?source=post_page-----e7b8f167006e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7b8f167006e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7b8f167006e--------------------------------) [Albers Uzila](https://dwiuzila.medium.com/?source=post_page-----e7b8f167006e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F159e5ce51250&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fokay-youve-trained-the-best-machine-learning-model-what-s-next-e7b8f167006e&user=Albers+Uzila&userId=159e5ce51250&source=post_page-159e5ce51250----e7b8f167006e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7b8f167006e--------------------------------) · 18 min 阅读 · 2023 年 6 月 4 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe7b8f167006e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fokay-youve-trained-the-best-machine-learning-model-what-s-next-e7b8f167006e&user=Albers+Uzila&userId=159e5ce51250&source=-----e7b8f167006e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7b8f167006e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fokay-youve-trained-the-best-machine-learning-model-what-s-next-e7b8f167006e&source=-----e7b8f167006e---------------------bookmark_footer-----------)![](img/a4f723cf7bf28815d9d27a97fd601e83.png)

照片由 [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

```py
***Table of Contents***
**·** **Initialize a Repository**
**·** **Migrate Your Codebase**
  ∘ config/config.py
  ∘ config/args.json
  ∘ tagolym/utils.py
  ∘ tagolym/data.py
  ∘ tagolym/train.py
  ∘ tagolym/predict.py
  ∘ tagolym/evaluate.py
  ∘ tagolym/main.py
**·** **Package Your Codebase** **·** **Setup Data Source Credential** **·** **Run Your Pipeline** **·** **Miscellaneous** **·** **Push Your Project to GitHub** **·** **Wrapping Up**
```

假设你正在构建一个数据科学项目，可能是为了工作、大学、个人作品集、爱好或其他任何事情。你已经花费了时间解决问题陈述并在 Jupyter 笔记本中进行实验。现在，你在想，“我怎么把我的工作部署成一个有用的产品？”。

具体地说，假设你有一个托管论坛的网站。用户可以为论坛中的线程添加标签，以便在不同主题的论坛之间进行导航。你希望通过建议预定义的标签来改善用户体验，从而给讨论内容提供上下文。

论坛可以是任何内容，所以让我们更具体一些；它始终以一个*帖子*开始，解释一个数学问题，然后围绕它展开思考、问题、提示或答案。以下是一个线程的样子…
