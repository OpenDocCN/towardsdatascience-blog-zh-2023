# Muybridge Derby: 让动物运动照片通过 AI 复活

> 原文：[https://towardsdatascience.com/muybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec?source=collection_archive---------11-----------------------#2023-07-25](https://towardsdatascience.com/muybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec?source=collection_archive---------11-----------------------#2023-07-25)

## 我如何使用 Midjourney 和 RunwayML 将伊德华·迈布里奇的照片序列转换为高分辨率视频

[](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmuybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----b1918e6622ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------) · 16 分钟阅读 · 2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1918e6622ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmuybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----b1918e6622ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1918e6622ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmuybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec&source=-----b1918e6622ec---------------------bookmark_footer-----------)![](../Images/13a194ce4c3d4ddc03b88feb11f5e9fd.png)

**《画框2：奔跑中的马》**（**Plate Number 626, Gallop**）由伊德华·迈布里奇（Eadweard Muybridge）创作（左），**基于 Midjourney 参考图像的 RunwayML Gen-1 视频生成器转换**（中和右），*这些图像使用 AI 图像创作程序生成*

# 背景

我相信你一定见过19世纪英国摄影师伊德华德·穆伊布里奇拍摄的奔跑马匹的系列照片。作为回顾，这里有一个GIF动画展示了他更著名的一组照片。

![](../Images/75eacf68bff18bc34a17bcac5652d108.png)

[**第626号板，奔跑**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider)由伊德华德·穆伊布里奇拍摄，动画GIF由作者制作

这里是穆伊布里奇的肖像，并附有他为拍摄这组照片所建造的设备的插图。

![](../Images/c9ec33691e714d02c8070506ca18cdf6.png)![](../Images/4a6856cb5f8cf5662885e37a3cd321b2.png)

**伊德华德·穆伊布里奇肖像**（左）图片来自 [维基共享](https://commons.wikimedia.org/wiki/File:Optic_Projection_fig_411.jpg)，**穆伊布里奇的设备**（右），图片来自 [维基共享](https://commons.wikimedia.org/wiki/File:Portret_van_Eadweard_Muybridge_Eadweard_Muybridge_(1830,_%2B1904.)_(titel_op_object),_RP-F-2001-7-509-65.jpg)

## 伊德华德·穆伊布里奇

穆伊布里奇是一位自然摄影师，他受加利福尼亚州州长利兰·斯坦福委托，记录他的豪宅及其财产。斯坦福给穆伊布里奇提出了一个激动人心的挑战：他能拍摄到奔跑中的马匹的清晰照片吗？

> 1872年是穆伊布里奇开始热衷于运动摄影的年份……
