# 促进员工之间联系的简单工具

> 原文：[https://towardsdatascience.com/simple-tool-to-foster-connection-among-employees-82ef5c1353f5?source=collection_archive---------18-----------------------#2023-02-01](https://towardsdatascience.com/simple-tool-to-foster-connection-among-employees-82ef5c1353f5?source=collection_archive---------18-----------------------#2023-02-01)

## 办公时间

## 利用 Python 构建一个快乐和紧密的团队

[](https://zluvsand.medium.com/?source=post_page-----82ef5c1353f5--------------------------------)[![Zolzaya Luvsandorj](../Images/dd3bb91f8625a6fbe8fd26e56036ad29.png)](https://zluvsand.medium.com/?source=post_page-----82ef5c1353f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82ef5c1353f5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82ef5c1353f5--------------------------------) [Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----82ef5c1353f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bca2b935223&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-tool-to-foster-connection-among-employees-82ef5c1353f5&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=post_page-5bca2b935223----82ef5c1353f5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82ef5c1353f5--------------------------------) ·6 min read·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82ef5c1353f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-tool-to-foster-connection-among-employees-82ef5c1353f5&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=-----82ef5c1353f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82ef5c1353f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-tool-to-foster-connection-among-employees-82ef5c1353f5&source=-----82ef5c1353f5---------------------bookmark_footer-----------)

COVID-19 促成了一个积极的变化，那就是推动更多公司采纳灵活的工作安排。这种采纳意味着我们中的更多人可以在封锁结束后继续在家工作。虽然这种灵活性在很多方面都很棒，但一个潜在的缺点是你不能像在办公室里那样偶然碰到别人，也没有那些随意自发的对话，这些对话有助于建立与同事的良好关系并让你感到自己是团队的一部分。在这篇文章中，我分享了一个简单的想法，通过一点编程技巧来激发这些对话，即使不是每个人都在办公室里。

![](../Images/6d60419e5a151baddb56d551a7c55fd9.png)

照片由[Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 💡 这个想法

所以这个想法很简单：定期随机匹配同事们，鼓励他们在小组内进行轻松的交流。例如，我们每周随机将所有同事分成3人一组，指定其中一人为会议组织者，安排一次`25分钟`的会议，并鼓励他们参加以便互相交流。或者，也可以作为关注健康的举措，安排双周一次的步行交流。这个基本想法可以进一步调整和自由定制以适应…
