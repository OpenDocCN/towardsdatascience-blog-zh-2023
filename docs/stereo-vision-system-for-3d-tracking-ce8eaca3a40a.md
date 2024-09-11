# 3D跟踪的立体视觉系统

> 原文：[https://towardsdatascience.com/stereo-vision-system-for-3d-tracking-ce8eaca3a40a?source=collection_archive---------1-----------------------#2023-01-08](https://towardsdatascience.com/stereo-vision-system-for-3d-tracking-ce8eaca3a40a?source=collection_archive---------1-----------------------#2023-01-08)

## 你只需要两只眼睛

[](https://sebastiengilbert.medium.com/?source=post_page-----ce8eaca3a40a--------------------------------)[![Sébastien Gilbert](../Images/380f6588c3ef718947bcf82061f190eb.png)](https://sebastiengilbert.medium.com/?source=post_page-----ce8eaca3a40a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce8eaca3a40a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce8eaca3a40a--------------------------------) [Sébastien Gilbert](https://sebastiengilbert.medium.com/?source=post_page-----ce8eaca3a40a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F975aef8c496a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstereo-vision-system-for-3d-tracking-ce8eaca3a40a&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=post_page-975aef8c496a----ce8eaca3a40a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce8eaca3a40a--------------------------------) ·6分钟阅读·2023年1月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce8eaca3a40a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstereo-vision-system-for-3d-tracking-ce8eaca3a40a&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=-----ce8eaca3a40a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce8eaca3a40a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstereo-vision-system-for-3d-tracking-ce8eaca3a40a&source=-----ce8eaca3a40a---------------------bookmark_footer-----------)![](../Images/729c84682441f3dacaf348fbf88094b2.png)

照片由 [Adriano Pinto](https://unsplash.com/@adrianopintofotografia?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

就像地球上绝大多数有视力的动物一样，我们有两只眼睛。我们进化中的这一奇妙特性使我们能够以三维的方式看待环境。

要从场景中获取3D信息，我们可以通过至少两台相机协同工作来模拟双眼视觉。这种设置被称为*立体视觉系统*。当相机经过正确校准后，每台相机都会对特定特征点的3D坐标提供约束。通过至少两台经过校准的相机，可以计算特征点的3D坐标。

在本文中，我们将校准一对相机，并使用此校准来计算在一系列图像中跟踪的特征点的3D坐标。你可以在[这个仓库](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision)找到相应的代码。

## 立体视觉系统

图1显示了我使用的立体视觉系统。如你所见，没有什么花哨的：一对通过3D打印部件、绑带和热熔胶固定在一起的网络摄像头。

![](../Images/4b1399212d3f0a0942471c8ec2d90d44.png)

图1：立体视觉系统。作者提供的图像。

正好我有两台相同型号的网络摄像头闲置在抽屉里，但并不一定需要完全相同的相机。由于相机是独立校准的，它们可以具有不同的内在参数，并且仍然在立体视觉系统中发挥作用。

# 相机的校准

## 投影矩阵

相机的校准归结为计算它们的*投影矩阵*。针孔相机模型的投影矩阵是一个3x4矩阵，它允许从世界参考框架中的3D坐标计算特征点的像素坐标。

![](../Images/585d80834287ff068b7fcf88e8bf8f85.png)

在公式(1)中，*i* 指代相机索引。在一个由两台相机组成的系统中，*i* 属于{1, 2}。

Pᵢ 是相机 *i* 的3x4投影矩阵。

(uᵢ, vᵢ) 是特征点的像素坐标，从相机 *i* 看到的情况。 (X, Y, Z) 是特征点的3D坐标。

标量 λᵢ 是保持方程同质性的缩放因子（即，两边向量的最后一个元素是1）。它的存在来自于将3D点投影到2D平面（相机传感器）时信息的丧失，因为多个3D点映射到同一个2D点。

假设我们知道相机的投影矩阵，并且我们有一个特征点的像素坐标，我们的目标是隔离(X, Y, Z)。

![](../Images/9fa9e7c101aea9e4ace8db3af43626e6.png)

将(3)代入(2)的前两行：

![](../Images/72b8b49fffbc2ddccd9b62922d9fc254.png)

公式(6) 表明，每个相机视图为我们提供两个未知数(X, Y, Z)的线性方程。如果我们至少有两个相机视图的相同3D点，我们可以通过求解一个过定的线性方程组来计算3D坐标。

> 太棒了！但我们如何计算投影矩阵呢？

## 投影矩阵的计算

要计算相机的投影矩阵，我们需要大量已知的3D点及其对应的像素坐标。我们使用一个棋盘格标定图案，尽可能精确地放置在测量距离上的立体设备前。

![](../Images/b8fe2ea5058b6c7391ff242ad89134ce.png)

图2：在已知位置的棋盘格标定图案。作者提供的图像。

[该仓库包括标定模式图像](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/tree/main/calibration_images)，适用于两台相机。

![](../Images/990076df5538be05eec67d75df4db8cc.png)

图 3：60 cm 距离摄像头的棋盘格标定模式图像。图片由作者提供。

你可以使用[这个 Python 程序](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/blob/main/calibrate_stereo.py)运行整个标定过程。

每个标定图像中的方形交点通过 CheckerboardIntersections 类的一个实例进行检测，该类在[我之前的文章](https://medium.com/towards-data-science/camera-radial-distortion-compensation-with-gradient-descent-22728487acb1)中介绍过。

在这种情况下，我们将交点检测器的参数设置得相对灵敏，以便检测所有真实交点，并且有一定量的假阳性（换句话说，完美的召回，合理的精度）。由于标定是一次性的过程，我们可以承受手动去除假阳性的工作。[程序](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/blob/main/calibrate_stereo.py)会遍历标定模式图像并要求用户选择假阳性。

![](../Images/86b556356698bc4af90e7ed329d0a251.png)

图 4：找到的交点，手动去除假阳性前后的情况。图片由作者提供。

正如我们在[我关于相机径向失真的补偿文章](https://medium.com/towards-data-science/camera-radial-distortion-compensation-with-gradient-descent-22728487acb1)中看到的，原始交点坐标必须是未失真的。径向失真模型之前已经为两台相机计算，[相应的文件已在仓库中](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/tree/main/radial_distortion)。未失真的坐标是我们用来构建针孔相机模型的投影矩阵的坐标。

此时，我们有 7（不同距离的拍摄）x 6 x 6（方形交点）= 252 对应于每台相机的 3D 点和像素坐标。

为了计算投影矩阵的条目，我们将重新开始使用方程（1），但这次假设我们知道（u, v）ₖ 和（X, Y, Z）ₖ，并且我们要解决 P 的条目。下标 k 指的是（pixel_coords, XYZ_world）对的索引。标量 λₖ（每个点一个）也是未知的。我们可以通过一些操作将 λₖ 从线性方程组中消除：

![](../Images/ee0b9873ccf2aee89a90c47ef0184a24.png)

方程（8）和（9）可以写成 Ap = 0 的形式：

![](../Images/3ce5bc073f325cafb6148b39d12132a0.png)

方程 (10) 显示每个对应关系提供了12个未知数中的两个齐次线性方程。至少需要6个对应关系来解出P的条目。我们还需要我们的3D点是非共面的。通过7个平面中的252个对应关系，我们是安全的。

在执行校准程序后，我们可以验证投影矩阵是否正确地将已知的3D点投影回其未畸变的像素坐标。

![](../Images/f17937b96be652b55561e8d60969356e.png)

图5：图像中棋盘格3D点的投影。图像由作者提供。

在图5中，蓝色点是经过径向畸变补偿后由交点检测器找到的点。黄色圆圈是图像中3D点的投影。我们可以看到，两台摄像头的投影矩阵表现良好。请注意，偏离标注点与棋盘格交点不重合，这是由于径向畸变补偿造成的。

# 3D 跟踪

我们现在可以使用校准后的立体系统来计算特征点的3D位置。为了演示这一点，我们将跟踪一个易于检测的特征点（一个红色方块的中心）在一系列图像中的位置。

你可以在[这里](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/blob/main/track_red_square.py)找到跟踪程序。

![](../Images/b33e6e53aba1d639ffe4249adee0a68f.png)

图6：左：跟踪的红色方块的图像。右：检测到的斑点。图像由作者提供。

图6展示了跟踪红色方块的检测示例。简而言之，方块是通过首先识别图像中蓝色成分占主导的区域来跟踪的，因为红色方块周围的区域是蓝色的。然后，在蓝色主导区域内找到红色成分占主导的区域。详细信息请参阅[代码](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision/blob/main/utilities/red_square.py)。

![](../Images/6c6d694dc3406e8bb5a50b50e6b03fd6.png)

图7：红色方块的中心在3D中被跟踪。坐标单位为厘米。动画由作者提供。

使用两台摄像头的未畸变像素坐标及其对应的投影矩阵，可以计算出每张图像中特征点的3D位置，如上面的动画所示。

# 结论

我们用一对网络摄像头构建了一个简单的立体视觉系统。我们通过补偿径向畸变并计算投影矩阵来校准这两台摄像头。我们可以使用经过校准的系统来跟踪一系列图像中一个特征点的3D位置。

请随意尝试[代码](https://github.com/sebastiengilbert73/tutorial_calibrate_stereo_vision)。

**如果你有立体视觉的应用想法，请告诉我，我会非常感兴趣了解！**
