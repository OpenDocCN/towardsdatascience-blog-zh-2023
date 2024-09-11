# 通过强化学习进行机器人手臂的障碍物避免任务训练

> 原文：[`towardsdatascience.com/training-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning-ea2a3404883f?source=collection_archive---------14-----------------------#2023-03-15`](https://towardsdatascience.com/training-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning-ea2a3404883f?source=collection_archive---------14-----------------------#2023-03-15)

## 轻松使用 **robotic-maninpulator-rloa** 框架训练机器人手臂以避开障碍物

[](https://medium.com/@JavierMtz5?source=post_page-----ea2a3404883f--------------------------------)![哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----ea2a3404883f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea2a3404883f--------------------------------)![数据科学进展](https://towardsdatascience.com/?source=post_page-----ea2a3404883f--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----ea2a3404883f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d7213a71a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning-ea2a3404883f&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=post_page-74d7213a71a8----ea2a3404883f---------------------post_header-----------) 发表在 [数据科学进展](https://towardsdatascience.com/?source=post_page-----ea2a3404883f--------------------------------) ·7 min read·2023 年 3 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea2a3404883f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning-ea2a3404883f&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=-----ea2a3404883f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea2a3404883f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning-ea2a3404883f&source=-----ea2a3404883f---------------------bookmark_footer-----------)![](img/3d857dfac9d0b252ac6b3ded3ae4c6d7.png)

图片来源于 [David Levêque](https://unsplash.com/ko/@davidleveque?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接来实现 :)
> 
> [`www.learnml.wiki/training-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning/`](https://www.learnml.wiki/training-of-robotic-manipulators-on-the-obstacle-avoidance-task-through-reinforcement-learning/)

本文的目的，除了讲解如何训练操控器以完成避障任务外，还包括介绍开源框架**robotic-manipulator-rloa**，并解释其工作原理及使用方法。

简而言之，**robotic-manipulator-rloa**框架允许用户在模拟环境中加载一个机器人操控器的 URDF 或 SDF 模型（URDF 或 SDF 文件是用于描述机器人所有元素的 XML 文件格式），该模型将用于训练操控器，以避免在尝试到达特定空间点时撞击障碍物。为了使训练具有可定制性，用户可以配置环境的基本组件以及 NAF 算法的超参数，NAF 算法是用于训练的算法。

> NAF 算法之前在某处已有解释…
