# 1 还是 0：图像分类中的像素攻击

> 原文：[`towardsdatascience.com/to-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a`](https://towardsdatascience.com/to-1-or-to-0-pixel-attacks-in-image-classification-ec323555a11a)

## 探索对抗性机器学习的领域

[](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----ec323555a11a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec323555a11a--------------------------------) ·阅读时间 13 分钟·2023 年 11 月 23 日

--

![](img/5969ea7ddc81cf61597e791f7e4a79c5.png)

图片由[Pietro Jeng](https://unsplash.com/@pietrozj?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)提供，来源于[Unsplash](https://unsplash.com/photos/green-and-red-light-wallpaper-n6B49lTx7NM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

嗨，大家好！

今年，我参加了我的第一次[Capture The Flag (CTF)比赛](https://www.kaggle.com/competitions/ai-village-capture-the-flag-defcon31/overview)，这次经历可以说非常引人入胜。挑战，特别是那些涉及像素攻击的挑战，引起了我的注意，并成为了这篇文章的主要焦点。虽然我最初打算分享我在比赛中进行的一个简单的像素攻击，但这篇文章的目标也是*深入探讨*如何增强机器学习模型，以更好地抵御像比赛中遇到的像素攻击。

在我们深入理论之前，让我们通过一个引人入胜的场景来引起你的注意。

想象一下：我们的公司 MM Vigilant 致力于开发一款前沿的物体检测产品。概念简单而革命性——客户拍下所需物品的照片，几天后它就会送到客户家门口。作为幕后出色的数据科学家，你打造了终极的基于图像的物体分类模型。分类结果无可挑剔，模型评估指标一流，利益相关者也非常满意。模型投入生产，客户也很高兴——直到投诉接踵而至。

经调查，发现有人在图像到达分类器之前对其进行干扰。具体来说，每张钟表的图像都被恶意地分类为镜子。结果如何？任何期待钟表的人都会收到意外的镜子。这真是个意外的转折，不是吗？

我们在 MM Vigilant 的利益相关者对这种事故的发生感到既担忧又好奇，更重要的是，如何采取措施来防止它。

我们刚刚探讨的场景是一个假设情境——尽管图像篡改是一个非常可能的情况，特别是当模型存在漏洞时。

那么，让我们仔细看看这种图像操作的一个例子……

# 图像分类中的像素攻击

像素攻击，特别是在图像分类的背景下，旨在欺骗机器学习（ML）分类器，将图像分类为其他类别。虽然可以讽刺地认为，一个不佳的模型已经表现出这种行为，但这里的目标是击败最先进的模型。不用说，这些攻击有很多方法和动机，但这篇文章，限于范围，将重点关注黑箱、针对性像素攻击及其相关的预防措施。

让我们从直观上来理解这个概念。实际上，任何输入到神经网络的图像都经过每个像素的一系列数学运算来进行分类。改变一个像素，因此，会导致这些数学运算的结果发生变化，从而改变预测得分。这可以推断到这样一种程度，如果一个主要/“对分类至关重要”的像素被操控，它将对类别的预测得分产生足够大的影响，从而导致误分类，如下图所示。

![](img/c4c97ae5ef8137c6c38478aa35920c65.png)

图片来源于作者

在图像分类攻击领域，有两种知名的方法，取决于误分类的期望结果：

1.  针对性攻击

1.  未针对性攻击

## 针对性攻击

针对性像素攻击涉及一种有目的的转换，目标是将图像分类为特定类别。例如，想象一个故意尝试将熊分类为船或将苹果分类为考拉的行为。这些攻击有两个目标：最小化原始类别的得分，同时最大化目标类别的得分。

## 未针对性攻击

相反，未针对性像素攻击的前提是避免将图像分类为其原始类别。任务简化为最小化指定类别的预测得分。换句话说，目标是确保一只熊，例如，被分类为**除了熊之外的任何东西**。

> 值得注意的是，每个针对性攻击都可以被认为是未针对性攻击，但反之则不一定成立。

除了攻击类型之外，还有两种不同的方法来实现这些攻击，具体取决于被攻击模型的可用性（传统/白盒方法）或仅有的结果分数（黑盒方法）。

# 传统攻击

在传统或白盒攻击中，模型通常是可用的。可以获取梯度信息并用于如 [快速梯度符号方法（FGSM）](https://pytorch.org/tutorials/beginner/fgsm_tutorial.html) 这样的攻击。这种方法通过沿梯度方向对输入数据进行小幅扰动，导致误分类，而不会显著改变图像的视觉外观。

可以在以下代码库中找到该方法的简单 GitHub 实现。

[](https://github.com/ymerkli/fgsm-attack/tree/master?source=post_page-----ec323555a11a--------------------------------) [## GitHub - ymerkli/fgsm-attack: 目标和非目标快速梯度符号方法的实现

### 目标和非目标快速梯度符号方法的实现 - GitHub - ymerkli/fgsm-attack: 实现了…

github.com](https://github.com/ymerkli/fgsm-attack/tree/master?source=post_page-----ec323555a11a--------------------------------)

# 黑盒攻击

黑盒攻击则完全依赖于模型预测。可以使用如 [差分进化](https://machinelearningmastery.com/differential-evolution-from-scratch-in-python/) 这样的技术来执行这种类型的攻击。

> 差分进化是一种模拟自然选择的优化算法。它通过在迭代中创建和组合潜在解决方案，基于设定的标准从一个种群中选择表现最佳的解决方案。这种方法在探索解决方案空间方面效果显著，并且常用于对机器学习模型的对抗攻击。

鉴于我们的挑战集中在黑盒目标像素攻击上，让我们直接进入实现部分。

## CTF 挑战

对于 CTF 挑战，其目标是将一张清晰的狼的图像误分类为格兰尼·史密斯苹果——向“小红帽”故事致敬。数据集中包含大约 1000 个类别，图像分辨率为 768x768 像素，超越了 MNIST、CIFAR 甚至 ImageNet 的分辨率，困难在于通过识别最少的像素数来欺骗模型以达到目标误分类。值得注意的是，尽管高分辨率图像复杂，但机器学习分类的本质，如上所述，在于任务的非直观性，将图像简化为一组值和一系列非常依赖这些个体值的数学运算。

在我们深入研究代码之前，让我们先看看我们狼的原始图像。难道它看起来不具有伪装成苹果的潜力吗？那绿色的眼睛、圆圆的脸以及绿色的背景——这些都是一个果味冒名顶替者的所有特征。

![](img/124371fd270a9fb4da747983c8be8b4a.png)

由 [AI Village](https://www.kaggle.com/competitions/ai-village-capture-the-flag-defcon31/data) 提供，许可证 [署名 4.0 国际 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

在开始的旅程中，黑箱模型对“木狼”类别的初始评分约为 0.29，而对“青苹果”类别的评分为 0.0005。我最初考虑应用 [scipy 的差分进化](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.differential_evolution.html) 方法。这种方法在涉及 CIFAR 和 ImageNet 数据集的像素攻击中已显示成功。差分进化技术涉及从 n 个随机样本开始，代表种群大小。在每一步，选择最好的后代，通过模型评分来确定，最终导致我们期望的结果。然而，鉴于时间限制和任务涉及仅更改单个图像的评分，我选择了更直接的策略。

## 方法

我首先将原始图像划分为逐渐更小的块，从 2x2 开始，一直到 16x16。针对目标青苹果（绿色），我逐一将块中的值更改为苹果绿色，并观察对木狼类别和青苹果类别分数的影响。然后，我手动选择了 2-3 个 16x16 的块，在这些块中应用了一种差分进化的方法。这意味着在该区域内随机选择的像素进行了大约 50-75 次迭代的单像素更改。

尽管我在给定的两天内无法准确定位到那个臭名昭著的单一像素，但我成功地进行了高度像素化的攻击，将狼的分类改变为青苹果，从而获得了三部分任务的两个子问题的标志。

现在我们有了背景信息，让我们跳入一些代码中，以便你能从这篇文章中学到一些东西。

## Python 代码

我将其视为黑箱问题，当给定图像时，查询会提供所有类别的预测列表。预测按值排序，因此预测的类别是列表中的第一个值。

```py
import requests
import base64
import cv2
import numpy as np
import matplotlib.pyplot as plt

def query(input_data):
    response = requests.post({link to get the blackbox score}, 
                              json={'data': input_data})
    return response.json()
```

`get_scores`函数以正确的格式将图像输入查询，并在大多数情况下以字典形式获得所需的结果。

```py
 def get_scores(input_image):
    # Some preprocessing since the query accepted only bytes

    _, input_image = cv2.imencode('.png', input_image)  
    image_bytes = input_image.tobytes()
    input_data = base64.b64encode(image_bytes).decode()
    result = query(input_data)

    """
    the result is a json dict {} with the variable 'output' or 'flag', 
    the output consists of scores for 1000 classes of which two are timber
    wolf and granny smith. Initially the score for timber wolf is around 0.29
    and the score for granny smith id 0.0005
    """

    dict_score = {"timber wolf" : 0, "Granny Smith" : 0}

    try:
        print(result['flag'])
    except:
        pass

    # the scores in the output are always sorted so the first score 
    # is always the predicted score

    dict_score["predicted_class"] = result['output'][0][1]
    dict_score["predicted_score"] = result['output'][0][0]

    # next we get the scores for our wanted target and our original class
    count = 0
    for sublist in result['output']:
        score, class_name = sublist
        if class_name == "timber wolf":
            dict_score['timber wolf'] = score
            count+=1
        elif class_name == "Granny Smith":
            dict_score["Granny Smith"] = score
            if count ==1:
                break

    return dict_score
```

## 相关代码

核心思想是选择在苹果颜色的 RGB 范围内的像素，并测试约 50-75 个像素，以找到最大化“**青苹果**”类别分数并最小化“**狼**”类别分数的像素。我逐渐增加了选择区域的大小，并根据需要修改优化过程。例如，当“**青苹果**”类别的分数超过“**狼**”类别的分数时，我考虑所有增加“**青苹果**”类别分数的像素，只要它高于“**狼**”类别的分数，而不是专注于减少“**狼**”类别的分数，这显然加快了一些进程。

尽管没有找到那个难以捉摸的单一像素，我成功执行了高度像素化的攻击。

```py
# Load your image
input_image = cv2.imread('/timber_wolf.png')
# Get the dimensions of the original image
image_height, image_width, _ = input_image.shape

# Define the size of the window (dxd)
# initially I had a large window size for testing purposes 
# to identify regions of high interest
window_size = 1 #image_height//64

# get the initial scores
scores = get_scores(input_image)

dict_pixels ={'pixels':[]}

best_score_tw =  scores['timber wolf'] #the current/best score for timber wolf
best_score_gs = scores['Granny Smith'] #the current/best score for granny smith

print(best_score_tw, best_score_gs)

max_iter = 75
iter_1=-1

pixel_count = -1 # number of pixels that have been changed
max_pixel_count = 40 # number of pixels we want to change

temp_image = input_image
rand_red_best, rand_green_best = (0, 0)
row_best, col_best = (0, 0)

while pixel_count < max_pixel_count:
  while iter_1 < max_iter:
    # although I did change the values from time to time
    row = np.random.randint(192,388) 
    col = np.random.randint(192,388)

    iter_1 +=1

    output_image = input_image.copy()

    left = row
    upper = col
    right = min(x + window_size, image_width)
    lower = min(y + window_size, image_height)

    # the pixel values for RGB were kept close to the color of the apple 
    rand_red = np.random.randint(0,153)
    rand_green = np.random.randint(170,255)
    rand_blue = 0#np.random.randint(0,255)
    output_image[upper:lower, left:right] = [rand_red, rand_green, rand_blue]
    scores = get_scores(output_image)
    # I actually also changed this a couple of times depending on where the output was

    #if (scores['timber wolf'] - scores['Granny Smith']) < min_score  :
    # initially I wanted pixels that bridged that gap between both classes the most.
    # Once granny smith score crossed the timberwolf score I only cared about increasing
    # score for granny smith class as long as timberwolf stayed below granny smith sclass
    if (best_score_tw > scores['timber wolf']) and (best_score_gs < scores['Granny Smith'])

        temp_image = output_image
        best_score_tw = scores['timber wolf']
        best_score_gs = scores['Granny Smith']
        rand_red_best = rand_red
        rand_green_best = rand_green
        min_diff =  scores['timber wolf'] - scores['Granny Smith']
        best_row, best_col = row, col

        print(iter_1, [rand_red,rand_green,0], ':', row,col, ";\n",min_diff,'\n')

  pixel_count += 1
  input_image = temp_image
  scores = get_scores(input_image)

  print(pixel_count,
        '\n', row, col, [rand_red_best, rand_green_best, 0],
        '\n', scores, '\n')

  dict_pixels['pixels'].append(([row_best,col_best],[rand_red_best,rand_green_best,0]))

np.save('/output_image.npy', input_image)
np.save('/pixel_data.npy', dict_pixels)

scores = get_scores(input_image)
best_score_tw =  scores['timber wolf']
best_score_gs = scores['Granny Smith']
print(best_score_tw, best_score_gs)
```

结果看起来像这样，它被分类为**青苹果**。

![](img/054fe7aeb01f7f2c049f2d3c3b4a3ccb.png)

原始图像由 [AI Village](https://www.kaggle.com/competitions/ai-village-capture-the-flag-defcon31/data) 提供，许可 [署名 4.0 国际 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)（由作者编辑）

# 放大结果

我的狼图像显然被篡改了，但这是一个高分辨率图像，攻击成功了。我相信如果再给点时间，可能会用更少的像素达到更好的欺骗效果。

![](img/3f8484e47b07afe9010118614313cde2.png)

像素化的狼被分类为青苹果。由 [AI Village](https://www.kaggle.com/competitions/ai-village-capture-the-flag-defcon31/data) 提供，许可 [署名 4.0 国际 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)（由作者编辑）

## 提醒一句…

在目睹了一个可能的像素攻击版本，这个版本仅根据预测分数和试验与错误导致了模型的错误分类之后，让我们进一步探讨如何避免这种情况。

当然，这里的目标不是鼓励执行像素攻击，除非这是对自己模型的抗性检查。探索对抗性机器学习实践的复杂性本质上是为了培养如何保护模型不受这些方法影响的意识。

所以让我们深入探讨如何避免这些情况…

# 像素攻击的可能弱点

像素攻击，特别是在黑箱设置中，已经涉及到大量的试验和错误，但各种策略可以进一步增强模型对这些攻击的鲁棒性。

## 1. 使用高分辨率图像

高分辨率图像更难被攻击，因为它们需要更多的资源和更高数量的改变特征/像素，因此更难以微妙地篡改。

![](img/9227cf4ea3af5d97f1482ea705e9c58b.png)

图像由作者提供

澄清：例如，考虑一个来自 CIFAR 的 32x32 图像，它的像素较少，使其更容易受到篡改。相比之下，高分辨率图像由于像素数量更多，因此不易受到像素攻击。另一方面，这些图像虽然在隐蔽篡改时更具挑战，但在训练过程中可能会产生更高的计算成本。因此，需要在安全性和计算效率之间找到平衡。

## 提高接受结果的预测分数阈值

由于被攻击的图像具有较低的预测分数，可以利用分数阈值来检测潜在的对抗性攻击。

![](img/d283e1c5f815ebd64c79b3fe41b5ad98.png)

作者提供的图片

澄清：例如，设置一个阈值，低于该阈值的预测被视为不确定，这为防御对抗性攻击提供了额外的安全层。

再次值得指出的是，这是一个权衡，高阈值提升了信心，但可能会限制分类器的敏感度。找到合适的平衡对于避免拒绝有效预测同时抵御对抗性攻击至关重要。

## 考虑到 CNN 在关键应用中的鲁棒性

结果表明，尽管卷积神经网络（CNNs）并非免疫，但由于利用了空间层次结构，它们对这种对抗性攻击的敏感性较低。

![](img/939bb56d8794531c410adeed1260733a.png)

作者提供的图片

澄清：简单来说，虽然平均模型将像素视为单独的输入，但卷积神经网络（CNN）通过内核窗口考虑预定义的关联，从而增强了对抗性操控的鲁棒性。

## 预测前的图像预处理

在将图像输入神经网络进行预测之前，应用一种稳健的预处理技术可能是值得的，从而限制黑箱攻击。

![](img/18415ca635300fc3186c5e9b8bc41a4e.png)

作者提供的图片

澄清：例如，图像压缩有助于减少篡改的影响，而计算机视觉算法可以识别图像中的失真或异常。此外，由于被操控的像素可能与原始图像的颜色或模式不完全匹配，因此可以应用插值技术。

## 安全的机器学习模型

上述方法虽然有效，但并非一刀切。最终，保护某个机器学习模型免受对抗性攻击包括在各种条件下（包括暴露于潜在对抗性输入）对模型进行严格的测试和验证。

决定添加多少安全性以及多频繁更新模型取决于其重要性和可能面临的威胁类型。但是，了解伦理考虑和理解可能的威胁有助于减少攻击风险。

# 总结……

虽然像素攻击或任何图像的操控对基于图像的人工智能系统确实是一个大问题，但我们也可以做很多事情来防范这些攻击。攻击者可以篡改单个像素来欺骗模型，使其犯错误，从而危害图像识别和安全系统等关键应用的可靠性。这不仅会导致安全漏洞，还会破坏客户和利益相关者的信任。

另一方面，机器学习从业者拥有工具来确保模型不容易受到这些攻击的影响。

在这篇文章中，我尝试探索了像素攻击，受到 CTF 挑战的启发，并深入研究了欺骗图像分类模型的一些复杂性。虽然狼确实变成了一个青苹果，但这需要大量的计算和反复试验，如果模型采取了一些预防措施，攻击将会失败。

我在下面列出了一些类似的方法资源，希望你能发现这些话题对保持模型安全有用。

# 资源

[](https://github.com/Hyperparticle/one-pixel-attack-keras?source=post_page-----ec323555a11a--------------------------------) [## GitHub - Hyperparticle/one-pixel-attack-keras: Keras 实现的“单像素攻击”…

### 使用差分进化在 Cifar10 上进行“单像素攻击以欺骗深度神经网络”的 Keras 实现和…

[github.com](https://github.com/Hyperparticle/one-pixel-attack-keras?source=post_page-----ec323555a11a--------------------------------)  [## GitHub - max-andr/square-attack: Square Attack: 一个查询高效的黑箱对抗攻击，通过…

### Square Attack: 一个查询高效的黑箱对抗攻击，通过随机搜索 [ECCV 2020] - GitHub …

[github.com](https://github.com/max-andr/square-attack?source=post_page-----ec323555a11a--------------------------------) [](https://github.com/kenny-co/procedural-advml?source=post_page-----ec323555a11a--------------------------------) [## GitHub - kenny-co/procedural-advml: 无任务通用黑箱攻击计算机视觉…

### 无任务通用黑箱攻击计算机视觉神经网络通过过程噪声（CCS'19） - GitHub …

[github.com](https://github.com/kenny-co/procedural-advml?source=post_page-----ec323555a11a--------------------------------)
