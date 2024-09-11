# Pip 安装特定版本 — 如何用 Pip 安装特定的 Python 包版本

> 原文：[https://towardsdatascience.com/pip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31?source=collection_archive---------1-----------------------#2023-04-05](https://towardsdatascience.com/pip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31?source=collection_archive---------1-----------------------#2023-04-05)

## 想用 Pip 安装特定版本的 Python 包吗？这篇文章将通过实际示例和指南向你展示如何操作。

[](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)[![达里奥·拉德西奇](../Images/41882a3b30bab9da43d66a59f1df366b.png)](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------) [达里奥·拉德西奇](https://medium.com/@radecicdario?source=post_page-----1f97e1002f31--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F689ba04bb8be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31&user=Dario+Rade%C4%8Di%C4%87&userId=689ba04bb8be&source=post_page-689ba04bb8be----1f97e1002f31---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f97e1002f31--------------------------------) ·6 分钟阅读·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1f97e1002f31&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31&user=Dario+Rade%C4%8Di%C4%87&userId=689ba04bb8be&source=-----1f97e1002f31---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1f97e1002f31&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpip-install-specific-version-how-to-install-a-specific-python-package-version-with-pip-1f97e1002f31&source=-----1f97e1002f31---------------------bookmark_footer-----------)![](../Images/0184279087e9fb03099c4581fbd2fa99.png)

图片由 [Mario Gogh](https://unsplash.com/@mariogogh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**总结：** 你可以通过运行`pip install <package_name>==<version>`命令来安装特定版本的 Python 包。例如，要安装 [Pandas](https://practicalpandas.com/how-to-install-pandas-specific-version/) 的 1.3.4 版本，请在终端中执行`pip install pandas==1.3.4`命令。

这是一种简短的版本，可能是你所需要的全部，但有时你可能想对软件包安装进行更多控制。这就是本文其余部分的用武之地。

# 为什么你会想要安装特定（旧）版本的 Python 包

那么，为什么还要考虑安装旧版本的 Python 包呢？也许你有一个庞大的代码库，不兼容最新的软件包更新。也许你几年前编写的代码在生产环境中仍然有效，但更新包可能会破坏它。或者，即使是最新的软件包版本也可能与你的 Python 版本不兼容。

**换句话说**——无论情况如何，安装旧版本都有其合理原因…
