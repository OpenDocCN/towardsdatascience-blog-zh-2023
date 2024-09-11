# 使用超级收敛加速你的深度学习模型训练

> 原文：[https://towardsdatascience.com/supercharge-training-of-your-deep-learning-models-7168ff81a042?source=collection_archive---------7-----------------------#2023-11-22](https://towardsdatascience.com/supercharge-training-of-your-deep-learning-models-7168ff81a042?source=collection_archive---------7-----------------------#2023-11-22)

## 使用单周期学习率实现超级收敛

[](https://medium.com/@Rghv_Bali?source=post_page-----7168ff81a042--------------------------------)[![Raghav Bali](../Images/49fea68f38f59d0bc39dab484b55684f.png)](https://medium.com/@Rghv_Bali?source=post_page-----7168ff81a042--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7168ff81a042--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7168ff81a042--------------------------------) [Raghav Bali](https://medium.com/@Rghv_Bali?source=post_page-----7168ff81a042--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdff4008c1908&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-training-of-your-deep-learning-models-7168ff81a042&user=Raghav+Bali&userId=dff4008c1908&source=post_page-dff4008c1908----7168ff81a042---------------------post_header-----------) 发表在[《走向数据科学》](https://towardsdatascience.com/?source=post_page-----7168ff81a042--------------------------------) ·7 分钟阅读·2023 年 11 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7168ff81a042&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-training-of-your-deep-learning-models-7168ff81a042&user=Raghav+Bali&userId=dff4008c1908&source=-----7168ff81a042---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7168ff81a042&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-training-of-your-deep-learning-models-7168ff81a042&source=-----7168ff81a042---------------------bookmark_footer-----------)![](../Images/f3668520565b60e31f7dacbe8189e9fa.png)

照片由[Philip Swinburn](https://unsplash.com/@pjswinburn?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否遇到这样的情况：一开始提高准确率很容易，但一旦达到90%，就必须非常努力才能进一步提高性能？你的模型训练时间太长吗？

在本文中，我们将探讨一种有趣的技术，以加速你的训练设置，并获得你一直寻求的额外性能，提高训练速度。本质上，我们将致力于通过一种称为***一周期学习率***的策略，在训练过程中动态调整学习率。

原本在Leslie Smith的论文中提到的**一周期学习率计划**[[1](https://arxiv.org/abs/1803.09820)], [[2](https://arxiv.org/abs/1708.07120)]，专注于一种独特的策略，在训练过程中动态更新学习率。听起来术语很多，别担心，让我们先从一个典型的训练设置开始，然后逐渐理解如何通过一周期学习率来改进结果。

# 训练图像分类器

当我们致力于学习一种提高模型性能的巧妙技巧（周期率）时，何不在享受经典的***石头剪子布***游戏时进行呢。

![](../Images/69028e843abe371779ee844df84ddd0b.png)

由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 问题陈述

*石头剪子布是一个经典的儿童游戏，涉及两个玩家使用手势（石头、纸或剪刀）进行竞争，以压倒对手。例如，石头手势战胜剪刀，但纸手势战胜石头。有趣吧？*

我们的目标是训练一个图像分类模型，能够检测三种手势之一。然后，我们可以利用这样的训练模型开发一个端到端的游戏。为了本文的目的，我们将把范围限制在训练分类器本身，端到端的游戏和可部署的模型是另一个可能的文章主题。

## 数据集

我们很幸运已经有一个带标签的数据集，我们可以利用这个数据集有效地训练分类模型。数据集托管在[TensorFlow数据集](https://www.tensorflow.org/datasets/catalog/rock_paper_scissors)目录中，由[劳伦斯·莫罗尼](https://laurencemoroney.com/datasets.html#rock-paper-scissors-dataset)（CC BY 2.0）提供。数据集具有以下属性：

+   数据点数量：2800

+   类别数量：3

+   可用的训练-测试拆分：是

+   数据集大小：220 MiB

TensorFlow提供了一个干净的API来访问这些数据集，以下代码片段允许我们下载训练和验证拆分

```py
import tensorflow_datasets as tfds

DATASET_NAME = 'rock_paper_scissors'
(dataset_train_raw, dataset_test_raw), dataset_info = tfds.load(
    name=DATASET_NAME,
    data_dir='tmp',
    with_info=True,
    as_supervised=True,
    split=[tfds.Split.TRAIN, tfds.Split.TEST],
)

# plot samples from the dataset
fig = tfds.show_examples(dataset_train_raw, dataset_info)
```

以下是该数据集的一些样本图像：

![](../Images/fa32543b74881bdc796de9ca4f8bcc04.png)

图：石头剪子布数据集中的样本数据点

## 学习率

学习率是一个关键的超参数，它可能决定设置的成败，但通常被忽视。忽视的原因是，大多数库/包提供了足够好的默认值，但这些默认值只能带你走到一定程度。

对于像我们这样的定制使用案例，获得正确的学习率非常重要。找到最佳值是一项棘手的权衡。学习率设置得过慢（或过小），你的模型几乎无法学到任何东西。设置得过快（或过大），它将超越所有神经网络旨在找到的神秘最小值。下图展示了这一点，以便更好地理解。

![](../Images/c7f45ab8d5b0617894d110992ebd4064.png)

图示：学习率对模型学习目标（最小值）能力的影响。来源：作者

**梯度下降与优化器**

梯度下降是训练/优化神经网络的标准方法。它通过更新网络参数，使其朝着梯度的反方向前进，从而最小化目标函数。不深入细节，它有助于沿着目标函数的坡度向下行进。有关梯度下降的详细介绍，请参考[这里](https://cs231n.github.io/optimization-1/)。

深度学习社区自最初模型使用基础梯度下降进行训练以来已经取得了长足的进步。多年来，许多改进帮助更快地训练并避免明显的陷阱。简要地说，一些显著和最受欢迎的方法有：

**AdaGrad** [自适应梯度算法](https://www.jmlr.org/papers/volume12/duchi11a/duchi11a.pdf) 是一种优化算法，根据各个参数的历史梯度调整学习率，从而对不频繁的参数进行较大更新，对频繁的参数进行较小更新。它旨在有效处理稀疏数据，特别适合处理稀疏数据。

**RMSProp** [均方根传播](https://www.cs.toronto.edu/~tijmen/csc321/slides/lecture_slides_lec6.pdf) 通过对每个参数单独调整学习率来优化学习。它通过使用平方梯度的移动平均值来解决 AdaGrad 中学习率递减的问题。这有助于根据最近的梯度大小自适应地缩放学习率。

**ADAM** [自适应矩估计](https://arxiv.org/pdf/1412.6980.pdf) 是一种优化算法，结合了 RMSProp 和动量方法的思想。它保持过去梯度和平方梯度的指数衰减平均值，利用这些值自适应地更新参数。ADAM 以其高效性和有效性在训练深度神经网络中而闻名。

# One-Cycle Learning Rate 和超收敛

One-Cycle Learning Rate 是一个简单的两步过程，用于在训练过程中改进学习率和动量。其工作原理如下：

+   **第 1 步**：我们从较低的学习率开始，在几个时期内以线性递增的方式逐步提高到较高的值

+   **第 2 步**：我们在几个时期内维持学习率的最高值

+   **第 3 步**：然后我们回到一个较低的学习率，并随着时间的推移逐渐衰减

在这三个步骤中，动量在完全相反的方向上进行更新，即当学习率上升时，动量下降，反之亦然。

**一周期学习率的实际应用**

首先，我们将通过一个简单的1周期学习率实现进行操作，然后用它来训练我们的模型。我们将利用[Martin Gorner 2019年在*TensorFlow World*](https://docs.google.com/presentation/d/e/2PACX-1vRqvlSpX5CVRC2oQ_e_nRNahOSPoDVL6I36kdjuPR_4y_tCPb-_k98Du1QXBwx4sBvVrzsCPulmuPn8/pub?slide=id.g50ba8fd3eb_0_0)的演讲中现成的1周期LR计划实现，如清单2所示。

```py
def lr_function(epoch):
    # set start, min and max value for learning rate
    start_lr = 1e-3; min_lr = 1e-3; max_lr = 2e-3

    # define the number of epochs to increase 
    # LR lineary and then the decay factor
    rampup_epochs = 6; sustain_epochs = 0; exp_decay = .5

    # method to update the LR value based on the current epoch
    def lr(epoch, start_lr, min_lr, max_lr, rampup_epochs,
           sustain_epochs, exp_decay):
        if epoch < rampup_epochs:
            lr = ((max_lr - start_lr) / rampup_epochs
                        * epoch + start_lr)
        elif epoch < rampup_epochs + sustain_epochs:
            lr = max_lr
        else:
            lr = ((max_lr - min_lr) *
                      exp_decay**(epoch - rampup_epochs -
                                    sustain_epochs) + min_lr)
        return lr

    return lr(epoch, start_lr, min_lr, max_lr,
              rampup_epochs, sustain_epochs, exp_decay)
```

我们执行这个函数（*见* *清单2*）来展示学习率如何根据我们之前讨论的两个步骤进行变化。这里我们从**1e-3**的初始学习率开始，并在前几个epoch中将其提升至**2e-3**。然后在剩余的epoch中将其再次降低至**1e-3**。这种动态学习率曲线在以下24个epoch的样本运行中得以展示。

![](../Images/160baa91c3ce99fdf821cce2bbfb6683.png)

24个epoch的1周期学习率策略。学习率线性上升，然后在剩余的epoch中缓慢衰减。图像来源：作者

我们将通过在使用MobileNetV2模型作为特征提取器，并为当前的石头剪子布分类任务训练一个分类头时，测试我们的1周期学习率调度器。然后我们将其与简单的CNN以及使用标准Adam优化器的MobileNetV2+分类头进行比较。完整的笔记本可以在[github](https://github.com/raghavbali/python_notebooks/blob/master/supercharge_series/supercharge_learning_lr.ipynb)上找到参考。以下片段快速概述了我们如何使用TensorFlow回调来插入我们的1周期学习率工具。

```py
# Set Image Shape 
INPUT_IMG_SHAPE= (128, 128, 3)

# Get Pretrained MobileNetV2
base_model = tf.keras.applications.MobileNetV2(
  input_shape=INPUT_IMG_SHAPE,
  include_top=False,
  weights='imagenet',
  pooling='avg'
)

# Attach a classification head
model_lr = tf.keras.models.Sequential()
model_lr.add(base_model)
model_lr.add(tf.keras.layers.Dropout(0.5))
model_lr.add(tf.keras.layers.Dense(
    units=NUM_CLASSES,
    activation=tf.keras.activations.softmax,
    kernel_regularizer=tf.keras.regularizers.l2(l=0.01)
))

# compile the model
model_lr.compile(
    optimizer=tf.keras.optimizers.Adam(),
    loss=tf.keras.losses.sparse_categorical_crossentropy,
    metrics=['accuracy']
)

# set number of epochs
initial_epochs = 24

# Set the model for training
# The LearningRateScheduler callback is where we
# plug our custom 1-cycle rate function
training_history_lr = model_lr.fit(
    x=dataset_train_augmented_shuffled.repeat(),
    validation_data=dataset_test_shuffled.repeat(),
    epochs=initial_epochs,
    steps_per_epoch=steps_per_epoch,
    validation_steps=validation_steps,
    callbacks=[
        tf.keras.callbacks.LearningRateScheduler(lambda epoch: \
                                             lr_function(epoch),
                                             verbose=True)
    ],
    verbose=1
)
```

我们用批量大小为64的情况下训练了所有3个模型24个epoch。下图展示了1周期学习率的影响。与其他两个模型相比，它能帮助我们的模型在仅5个epoch内实现收敛。超收敛现象在验证数据集上也可见。

![](../Images/f97ad1bd91456177a4c935ba16b42d0a.png)

使用1周期学习率（mobileNetV2_lr）的MobileNetV2在5个epoch内即可收敛，表现优于MobileNetV2和简单CNN架构。

我们在10个epoch内达到了90–92%的验证准确率，这在所有模型中是迄今为止表现最好的。模型在测试数据集上的表现也显示了同样的情况，即MobileNetV2_lr轻松超越了其他两个模型。

```py
# Simple CNN
Test loss:  0.7511898279190063
Test accuracy:  0.7768816947937012

# MobileNetV2
Test loss:  0.24527719616889954
Test accuracy:  0.9220430254936218

# MobileNetV2_LR
Test loss:  0.27864792943000793
Test accuracy:  0.9166666865348816
```

# 结论

克服模型性能超过90%准确率的瓶颈并优化训练时间，可以通过实施**One-Cycle Learning Rate**来实现。这一技术由Leslie Smith及其团队提出，在训练过程中动态调整学习率，提供了一种战略性方法以增强模型性能。通过采用这一方法，你可以有效地应对训练设置的复杂性，并发掘更快、更有效的深度学习模型的潜力。拥抱**One-Cycle Learning Rate**的力量，提高你的训练体验，实现卓越的结果！
