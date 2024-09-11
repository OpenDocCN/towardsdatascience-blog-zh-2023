# GPT-4 的碳足迹

> 原文：[`towardsdatascience.com/the-carbon-footprint-of-gpt-4-d6c676eb21ae?source=collection_archive---------4-----------------------#2023-07-18`](https://towardsdatascience.com/the-carbon-footprint-of-gpt-4-d6c676eb21ae?source=collection_archive---------4-----------------------#2023-07-18)

## 最近泄露的数据首次让我们能够估算训练 OpenAI 的 GPT-4 所产生的碳排放

[](https://kaspergroesludvigsen.medium.com/?source=post_page-----d6c676eb21ae--------------------------------)![Kasper Groes Albin Ludvigsen](https://kaspergroesludvigsen.medium.com/?source=post_page-----d6c676eb21ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d6c676eb21ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6c676eb21ae--------------------------------) [Kasper Groes Albin Ludvigsen](https://kaspergroesludvigsen.medium.com/?source=post_page-----d6c676eb21ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba0b31bed21a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-carbon-footprint-of-gpt-4-d6c676eb21ae&user=Kasper+Groes+Albin+Ludvigsen&userId=ba0b31bed21a&source=post_page-ba0b31bed21a----d6c676eb21ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6c676eb21ae--------------------------------) · 7 min read · 2023 年 7 月 18 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd6c676eb21ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-carbon-footprint-of-gpt-4-d6c676eb21ae&source=-----d6c676eb21ae---------------------bookmark_footer-----------)![](img/a1cfe5cfdb78e054e76587a5d51276e3.png)

图片由 Taylor Vick 提供，来源于 Unsplash

最近的新闻提醒我们全球平均气温持续上升[1]，这让我们必须提醒自己大多数人类活动都有碳足迹，导致全球变暖和其他气候变化。这对于数字技术尤其是人工智能来说也是如此。本文提醒我们这一点，估算了训练 OpenAI 的大型语言模型 GPT-4 的碳排放。

要进行这样的估算，我们需要知道：

1.  训练 GPT-4 使用了多少电力

1.  电力的碳强度，即生成 1 KWh 电力的碳足迹

让我们直接深入探讨。

[](https://kaspergroesludvigsen.medium.com/membership?source=post_page-----d6c676eb21ae--------------------------------) [## 使用我的推荐链接加入 Medium - Kasper Groes Albin Ludvigsen

### 作为 Medium 的会员，你的会员费的一部分将会分配给你阅读的作者，你可以完全访问每个故事……

[kaspergroesludvigsen.medium.com](https://kaspergroesludvigsen.medium.com/membership?source=post_page-----d6c676eb21ae--------------------------------)

# GPT-4 的电力消耗

首先估算 GPT-4 的能耗。根据未经验证的信息泄露，GPT-4 的……
