# R 数据分析：如何为孩子们找到完美的 Cocomelon 视频

> 原文：[https://towardsdatascience.com/r-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267?source=collection_archive---------2-----------------------#2023-03-04](https://towardsdatascience.com/r-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267?source=collection_archive---------2-----------------------#2023-03-04)

## 如何从零开始使用 R 构建端到端数据项目，探索新兴的 Cocomelon 视频

[](https://chengzhizhao.medium.com/?source=post_page-----833d6b2d9267--------------------------------)[![程志·赵](../Images/186bba91822dbcc0f926426e56faf543.png)](https://chengzhizhao.medium.com/?source=post_page-----833d6b2d9267--------------------------------)[](https://towardsdatascience.com/?source=post_page-----833d6b2d9267--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----833d6b2d9267--------------------------------) [程志·赵](https://chengzhizhao.medium.com/?source=post_page-----833d6b2d9267--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----833d6b2d9267---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----833d6b2d9267--------------------------------) · 9分钟阅读 · 2023年3月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F833d6b2d9267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----833d6b2d9267---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F833d6b2d9267&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-for-data-analysis-how-to-find-the-perfect-cocomelon-video-for-your-kids-833d6b2d9267&source=-----833d6b2d9267---------------------bookmark_footer-----------)![](../Images/de4472fd9253705fe85c6716eeade31c.png)

[照片由托尼·塞巴斯蒂安](https://unsplash.com/@tonyzebastian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布于[Unsplash](https://unsplash.com/photos/7sWm1yRJZhg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Cocomelon — Nursery Rhymes 是全球第二大 YouTube 频道（155M+ 订阅者）。这是一个如此受欢迎且有帮助的频道，以至于它成为了幼儿和父母必不可少的话题。我喜欢和我的儿子一起花时间观看 Cocomelon。

观看 Cocomelon 视频一个月后，我注意到相同的视频在 YouTube 上反复推荐。像《The wheel on the bus》和《bath song》这样的热门视频虽然有趣，但它们已经发布了多年，孩子们看得有些厌倦。作为一个父亲，我想展示一些更近期但质量良好的 Cocomelon 视频。作为一名数据专业人士，我还想探索全球第二大 YouTube 频道的数据，以获得更多见解并发现有关数据的有趣内容。

YouTube 频道中的所有视频仅提供两个选项：最近上传（按时间排序）和热门（按观看次数排序）。我可以去最近上传的标签页，一个一个地点击。然而，Cocomelon 频道有 800+ 个视频，这会非常耗时。
