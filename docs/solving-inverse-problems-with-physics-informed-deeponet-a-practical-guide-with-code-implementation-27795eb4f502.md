# 解决逆问题的物理信息深度操作网络：带有代码实现的实用指南

> 原文：[`towardsdatascience.com/solving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502`](https://towardsdatascience.com/solving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502)

## 两个带有参数估计和输入函数校准的案例研究

[](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)![Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------) ·21 分钟阅读·2023 年 7 月 17 日

--

![](img/375cf738f91896940d75caed2c8b983d.png)

由[愚木混株 cdd20](https://unsplash.com/@cdd20?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我的[上一篇博客](https://medium.com/towards-data-science/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887)中，我们深入探讨了物理信息深度操作网络（PI-DeepONet）的概念，并探讨了它为何特别适合于操作符学习，即从输入函数学习到输出函数。我们还将理论转化为代码，实现了一个 PI-DeepONet，该网络即使在遇到未见过的输入强迫配置文件时，也能准确地解决常微分方程（ODE）。

![](img/7ac9405ffbb94748bfce6bc08ba4c3b5.png)

图 1\. 操作符将一个函数转换为另一个函数，这是在实际动态系统中经常遇到的概念。**操作符学习**本质上涉及训练一个神经网络模型以逼近这个基础操作符。一种有前途的方法是**DeepONet**。 （图片由作者提供）

使用 PI-DeepONet 解决这些***前向***问题的能力无疑是有价值的。但这就是 PI-DeepONet 能做的全部吗？显然不是！

另一个我们在计算科学和工程中经常遇到的重要问题类别是所谓的***逆问题***。本质上，这类问题**将信息流动从输出反向到输入**：输入是未知的，而输出是可观察的，任务是从观察到的输出中估计未知的输入。

![](img/cab35d9a2efb313c05f626f8ae922355.png)

图 2\. 在前向问题中，目标是通过算子预测已知输入下的输出。在反向问题中，过程是相反的：使用已知的输出估算原始的、未知的输入，通常对底层算子只有部分了解。前向问题和反向问题在计算科学和工程中都很常见。（图像作者提供）

正如你可能已经猜到的，PI-DeepONet 也可以成为解决这些类型问题的超级有用工具。在本博客中，我们将详细探讨如何实现这一点。更具体地，我们将解决两个案例研究：一个是参数估算，另一个是输入函数校准。

> 本博客旨在自成一体，仅对物理信息（PI-）学习、DeepONet 以及我们的主要关注点 PI-DeepONet 进行简要讨论。有关这些主题的更全面介绍，请随时查看[我之前的博客](https://medium.com/towards-data-science/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887)。

鉴于此，让我们开始吧！

## 目录

· 1\. PI-DeepONet: 简介

· 2\. 问题陈述

· 3\. 问题 1：参数估算

∘ 3.1 如何运作

∘ 3.2 实现 PI-DeepONet 管道

∘ 3.3 结果讨论

· 4\. 问题 2：输入函数估算

∘ 4.1 解决策略

∘ 4.2 优化常规：TensorFlow

∘ 4.3 优化常规：L-BFGS

· 5\. 总结

· 参考文献

# 1\. PI-DeepONet: 简介

正如其名字所示，PI-DeepONet 是两个概念的结合：*物理信息学习* 和 *DeepONet*。

物理信息学习是机器学习的一种新范式，并在动态系统建模领域获得了特别的关注。其核心思想是将控制微分方程直接融入机器学习模型中，通常通过在损失函数中引入额外的损失项来考虑控制方程的残差。这种学习方法的前提是，以这种方式构建的模型将尊重已知的物理定律，并提供更好的泛化能力、可解释性和可信度。

DeepONet 与此同时属于传统的纯数据驱动建模领域。然而，其独特之处在于 DeepONet 专门设计用于**算子学习**，即学习从输入函数到输出函数的映射。这种情况在许多动态系统中经常遇到。例如，在一个简单的质量-弹簧系统中，随时间变化的驱动力作为输入函数（时间的函数），而质量的位移则是输出函数（也是时间的函数）。

DeepONet 提出了一种新型网络架构（如图 3 所示），其中**分支网络**用于转换输入函数的特征，**主干网络**用于转换时间/空间坐标。这两个网络输出的特征向量通过点积合并。

![](img/1fe10410dafdbe1907c7408142907b24.png)

图 3\. DeepONet 的架构。这种方法的独特性在于将分支网络和主干网络分开，分别处理输入函数特征和时间/空间坐标。（作者图片）

现在，如果将物理信息学习的概念叠加到 DeepONet 上，我们得到的就是所谓的 PI-DeepONet。

![](img/d1611ed23d6993195925a6cd19dc2691.png)

图 4\. 与 DeepONet 相比，PI-DeepONet 包含额外的损失项，如**ODE/PDE 残差损失**，以及初始条件损失（IC 损失）和边界条件损失（BC 损失）。对于 PI-DeepONet，传统的数据损失是可选的，因为它可以仅通过相关控制方程直接学习基础动态系统的算子。（作者图片）

一旦 PI-DeepONet 经过训练，它可以实时预测给定新输入函数特征的输出函数特征，同时确保预测结果与控制方程一致。正如你所想象的，这使得 PI-DeepONet 成为一个潜在非常强大的工具，适用于各种动态系统建模任务。

然而，在许多其他系统建模场景中，我们可能还需要执行完全相反的操作，即我们知道输出并希望根据观察到的输出和我们对系统动态的先验知识来估计未知的输入。一般来说，这种情况属于**逆向建模**的范围。这里自然出现了一个问题：我们是否也可以使用 PI-DeepONet 来解决逆向估计问题？

在我们深入探讨之前，让我们首先更准确地制定我们旨在解决的问题。

# 2\. 问题陈述

我们将使用前面博客中讨论的相同 ODE 作为基础模型。之前，我们研究了由以下方程描述的初始值问题：

![](img/a2965a8118cbca756dc4b2c9fbbb113a.png)

以初始条件 s(0) = 0 为例。在方程中，u(*t*)是随时间变化的输入函数，而 s(*t*)表示系统在时间*t*的状态。我们之前关注的是解决**前向**问题，即给定 u(·)预测 s(·)。现在，我们将转变关注点，考虑解决两种类型的逆向问题：

1️⃣ 估计未知的输入参数

让我们从一个简单的逆向问题开始。设想我们的控制 ODE 现在演变成这样：

![](img/298a2c22c0b53231f8bc8863fb14b9a5.png)

初始条件 s(0) = 0，a 和 b 是**未知数**。

其中 *a* 和 *b* 是两个未知参数。我们的目标是估计 *a* 和 *b* 的值，给定观察到的 u(·) 和 s(·) 轮廓。

这类问题属于 **参数估计** 的范围，其中需要从测量数据中识别系统的未知参数。这类问题的典型示例包括控制工程中的系统识别、计算热传导中的材料热系数估计等。

在我们当前的案例研究中，我们将假设 *a* 和 *b* 的真实值均为 0.5。

2️⃣ 估计整个输入函数轮廓

对于第二个案例研究，我们提高了问题的复杂性：假设我们对常微分方程 (ODE) 完全了解（即，我们知道 *a* 和 *b* 的确切值）。然而，尽管我们观察了 s(·) 轮廓，但我们尚不知道生成该观察输出函数的 u(·) 轮廓。因此，我们的目标是估计 u(·) 轮廓，给定观察到的 s(·) 轮廓和已知 ODE：

![](img/3e845f5f86ad6e2e52b414c01d6942d7.png)

初始条件 s(0) = 0，a=0.5，b=0.5。

由于我们现在的目标是恢复整个输入函数而不是一小部分未知参数，因此这个案例研究将比第一个案例更具挑战性。不幸的是，这类问题本质上是不适定的，需要强有力的正则化来帮助约束解空间。尽管如此，这类问题在多个领域中经常出现，包括环境工程（例如，识别污染源的轮廓）、航空航天工程（例如，校准飞机上的施加载荷）和风工程（例如，风力估计）。

在接下来的两个部分中，我们将逐一讨论这两个案例研究。

# 3\. 问题 1：参数估计

在本节中，我们处理第一个案例研究：估计我们目标 ODE 中的未知参数。我们将首先简要讨论如何使用通用的物理信息神经网络来解决这类问题，然后实施基于 PI-DeepONet 的参数估计管道。之后，我们将其应用于我们的案例研究，并讨论获得的结果。

## 3.1 工作原理

在 [原始论文](https://www.sciencedirect.com/science/article/abs/pii/S0021999118307125) 中，Raissi 和合著者概述了使用 PINNs 解决逆问题校准问题的策略：本质上，我们可以简单地将未知参数（在我们当前的案例中，即参数 *a* 和 *b*）设为 **可训练参数**，并与神经网络的权重和偏置一起优化这些未知参数，以最小化损失函数。

当然，关键在于构造损失函数：作为一种**物理信息学习方法**，它不仅包含一个数据不匹配项，该项衡量网络预测输出与观察数据之间的差异，还包含一个物理信息正则化项，该项使用神经网络的输出（及其导数）和当前的 *a* 和 *b* 参数估计值来计算残差（即微分方程左右两侧的差异）。

现在，当我们执行这种联合优化时，我们实际上是在寻找 *a* 和 *b* 值，这些值会导致网络的输出同时适应观察到的数据并满足控制微分方程。当损失函数达到最小值时（即训练收敛），我们获得的 *a* 和 *b* 的最终值就是实现这种平衡的值，因此它们就是未知参数的估计值。

![](img/29eabf0d14cb453bf39079298d746c1f.png)

图 5\. 在使用 PI-DeepONet 时，未知参数 *a* 和 *b* 与 DeepONet 模型的权重和偏置一起优化。当训练收敛时，最终得到的 *a* 和 *b* 值即为它们的估计值。（图像来自作者）

## 3.2 实现 PI-DeepONet 流水线

说够理论，接下来是代码部分 💻

我们从 PI-DeepONet 的定义开始：

```py
def create_model(mean, var, a_init=None, b_init=None, trainable=None, verbose=False):
    """Definition of a DeepONet with fully connected branch and trunk layers.

    Args:
    ----
    mean: dictionary, mean values of the inputs
    var: dictionary, variance values of the inputs
    a_init: float, initial value for parameter a
    b_init: float, initial value for parameter b
    trainable: boolean, indicate whether the parameters a and b will be updated during training
    verbose: boolean, indicate whether to show the model summary

    Outputs:
    --------
    model: the DeepONet model
    """

    # Branch net
    branch_input = tf.keras.Input(shape=(len(mean['forcing'])), name="forcing")
    branch = tf.keras.layers.Normalization(mean=mean['forcing'], variance=var['forcing'])(branch_input)
    for i in range(3):
        branch = tf.keras.layers.Dense(50, activation="tanh")(branch)

    # Trunk net
    trunk_input = tf.keras.Input(shape=(len(mean['time'])), name="time")
    trunk = tf.keras.layers.Normalization(mean=mean['time'], variance=var['time'])(trunk_input)   
    for i in range(3):
        trunk = tf.keras.layers.Dense(50, activation="tanh")(trunk)

    # Merge results 
    dot_product = tf.reduce_sum(tf.multiply(branch, trunk), axis=1, keepdims=True)

    # Add the bias
    dot_product_with_bias = BiasLayer()(dot_product)

    # Add a & b trainable parameters
    output = ParameterLayer(a_init, b_init, trainable)(dot_product_with_bias)

    # Create the model
    model = tf.keras.models.Model(inputs=[branch_input, trunk_input], outputs=output)

    if verbose:
        model.summary()

    return model 
```

在上述代码中：

1.  我们首先定义了主干网络和分支网络（它们都是简单的全连接网络）。两个网络生成的特征向量通过点积合并在一起。

1.  我们通过在点积结果上添加一个偏置项来进行下一步，这通过定义一个自定义的 `BiasLayer()` 实现。该策略可以提高预测准确性，正如 [原始 DeepONet 论文](https://arxiv.org/abs/1910.03193) 中所指出的。

1.  这里是使我们的模型能够解决参数估计问题的主要变化：我们将 *a* 和 *b* 添加到神经网络模型参数集合中。这样，当我们将 `trainable` 设置为 true 时，*a* 和 *b* 将与神经网络的其他常规权重和偏置一起进行优化。技术上，我们通过定义一个自定义层来实现这个目标：

```py
class ParameterLayer(tf.keras.layers.Layer):

    def __init__(self, a, b, trainable=True):
        super(ParameterLayer, self).__init__()
        self._a = tf.convert_to_tensor(a, dtype=tf.float32)
        self._b = tf.convert_to_tensor(b, dtype=tf.float32)
        self.trainable = trainable

    def build(self, input_shape):
        self.a = self.add_weight("a", shape=(1,), 
                                 initializer=tf.keras.initializers.Constant(value=self._a),
                                 trainable=self.trainable)
        self.b = self.add_weight("b", shape=(1,), 
                                 initializer=tf.keras.initializers.Constant(value=self._b),
                                 trainable=self.trainable)

    def get_config(self):
        return super().get_config()

    @classmethod
    def from_config(cls, config):
        return cls(**config)
```

请注意，这一层除了将这两个参数引入为模型属性外没有其他功能。

接下来，我们定义计算常微分方程残差的函数，这将作为物理信息损失项：

```py
@tf.function
def ODE_residual_calculator(t, u, u_t, model):
    """ODE residual calculation.

    Args:
    ----
    t: temporal coordinate
    u: input function evaluated at discrete temporal coordinates
    u_t: input function evaluated at t
    model: DeepONet model

    Outputs:
    --------
    ODE_residual: residual of the governing ODE
    """

    with tf.GradientTape() as tape:
        tape.watch(t)
        s = model({"forcing": u, "time": t})

    # Calculate gradients
    ds_dt = tape.gradient(s, t)

    # ODE residual
    ODE_residual = ds_dt - model.layers[-1].a*u_t - model.layers[-1].b

    return ODE_residual
```

请注意，我们使用 `model.layers[-1]` 来检索我们之前定义的参数层。这将为我们提供 *a* 和 *b* 值，以计算常微分方程的残差。

接下来，我们定义计算总损失对参数（包括常规权重和偏置以及未知参数 *a* 和 *b*）的梯度的逻辑。这将为我们进行梯度下降以训练模型做好准备：

```py
@tf.function
def train_step(X, y, X_init, IC_weight, ODE_weight, data_weight, model):
    """Calculate gradients of the total loss with respect to network model parameters.

    Args:
    ----
    X: training dataset for evaluating ODE residuals
    y: target value of the training dataset
    X_init: training dataset for evaluating initial conditions
    IC_weight: weight for initial condition loss
    ODE_weight: weight for ODE loss
    data_weight: weight for data loss
    model: DeepONet model

    Outputs:
    --------
    ODE_loss: calculated ODE loss
    IC_loss: calculated initial condition loss
    data_loss: calculated data loss
    total_loss: weighted sum of ODE loss, initial condition loss, and data loss
    gradients: gradients of the total loss with respect to network model parameters.
    """

    with tf.GradientTape() as tape:
        tape.watch(model.trainable_weights)

        # Initial condition prediction
        y_pred_IC = model({"forcing": X_init[:, 1:-1], "time": X_init[:, :1]})

        # Equation residual
        ODE_residual = ODE_residual_calculator(t=X[:, :1], u=X[:, 1:-1], u_t=X[:, -1:], model=model)

        # Data loss
        y_pred_data = model({"forcing": X[:, 1:-1], "time": X[:, :1]})

        # Calculate loss
        IC_loss = tf.reduce_mean(keras.losses.mean_squared_error(0, y_pred_IC))
        ODE_loss = tf.reduce_mean(tf.square(ODE_residual))
        data_loss = tf.reduce_mean(keras.losses.mean_squared_error(y, y_pred_data))

        # Weight loss
        total_loss = IC_loss*IC_weight + ODE_loss*ODE_weight + data_loss*data_weight

    gradients = tape.gradient(total_loss, model.trainable_variables)

    return ODE_loss, IC_loss, data_loss, total_loss, gradients
```

请注意，我们的损失函数由三个部分组成：初始条件损失、ODE 残差和数据损失。对于正向问题，数据损失是可选的，因为模型可以仅根据给定的微分方程直接学习基础算子。然而，对于反向问题（如当前的参数估计情况），结合数据损失是必要的，以确保我们找到正确的 *a* 和 *b* 值。在我们当前的案例中，仅需将所有权重，即 `IC_weight`、`ODE_weight` 和 `data_weight` 设置为 1 即可。

现在我们准备好定义主要的训练逻辑：

```py
# Set up training configurations
n_epochs = 100
IC_weight= tf.constant(1.0, dtype=tf.float32)   
ODE_weight= tf.constant(1.0, dtype=tf.float32)
data_weight= tf.constant(1.0, dtype=tf.float32)

# Initial value for unknown parameters
a, b = 1, 1

# Optimizer
optimizer = keras.optimizers.Adam(learning_rate=1e-3)

# Instantiate the PINN model
PI_DeepONet = create_model(mean, var, a_init=a, b_init=b, trainable=True)
PI_DeepONet.compile(optimizer=optimizer)

# Start training process
for epoch in range(1, n_epochs + 1):  
    print(f"Epoch {epoch}:")

    for (X_init, _), (X, y) in zip(ini_ds, train_ds):

        # Calculate gradients
        ODE_loss, IC_loss, data_loss, total_loss, gradients = train_step(X, y, X_init, 
                                                                        IC_weight, ODE_weight,
                                                                        data_weight, PI_DeepONet)
        # Gradient descent
        PI_DeepONet.optimizer.apply_gradients(zip(gradients, PI_DeepONet.trainable_variables))

    # Re-shuffle dataset
    ini_ds = tf.data.Dataset.from_tensor_slices((X_train_ini, y_train_ini))
    ini_ds = ini_ds.shuffle(5000).batch(ini_batch_size)

    train_ds = tf.data.Dataset.from_tensor_slices((X_train, y_train))
    train_ds = train_ds.shuffle(100000).batch(col_batch_size) 
```

在这里，我们将 *a* 和 *b* 的初始值都设置为 1。请记住，*a* 和 *b* 的真实值实际上是 0.5*。接下来，让我们训练 PI-DeepONet 模型，看看它是否能够恢复 *a* 和 *b* 的真实值。

> 请注意，我们讨论的代码仅是从完整的训练/验证逻辑中提取的关键片段。要查看完整代码，请查看[notebook](https://github.com/ShuaiGuo16/PI-DeepONet/blob/main/case-study-inverse-parameter.ipynb)。

## 3.3 结果讨论

要使用 PI-DeepONet 估计未知的 ODE 参数，我们首先需要生成训练数据集。在我们当前的案例中，数据生成分为两个步骤：首先，我们使用零均值的**高斯过程**生成 u(·) 曲线（即输入函数），其具有径向基函数（RBF）核。之后，我们在目标 ODE 上运行 `scipy.integrate.solve_ivp`（将 *a* 和 *b* 都设置为 0.5）以计算相应的 s(·) 曲线（即输出函数）。下图展示了用于训练的三个随机样本。

![](img/49abe28b01c92a89948281ba89ea477e.png)

图 6\. 从训练数据集中随机选择的三个样本用于说明。上排显示了 u(·) 曲线，下排显示了通过运行 ODE 求解器计算的对应 s(·) 曲线。（图片由作者提供）

一旦训练数据集准备好，我们就可以启动 PI-DeepONet 的训练过程。下图显示了损失的演变，这表明模型已经得到了适当的训练，并能够很好地拟合数据和 ODE。

![](img/3cf5fe0b651ee8535a3c306eba57bec5.png)

图 7\. 损失收敛图。（图片由作者提供）

训练 PI-DeepONet 并不是终点。我们的最终目标是估计 *a* 和 *b* 的值。下图展示了未知参数的演变。我们可以清楚地看到，PI-DeepONet 正在发挥作用，并准确地估计了 *a* 和 *b* 的真实值。

![](img/5b1ad194c77c63455d22ca0699945120.png)

图 8\. 我们的未知参数 *a* 和 *b* 逐渐远离指定的初始值，并收敛到它们的真实值。这表明 PI-DeepONet 能够对具有函数输入的微分方程进行反向参数校准。（图片由作者提供）

我们可以进一步测试估计结果对初始值的敏感性。下图显示了在不同初始值（*a_init*=0.2，*b_init*=0.8）下参数的演变：

![](img/b99ab18ff1d4b58a8435c2f0c165ccc3.png)

图 9\. 对于不同的初始值集，我们开发的 PI-DeepONet 模型能够准确估计 a 和 b 的真实值。（图片由作者提供）

总体而言，我们可以看到 PI-DeepONet 模型能够在 ODE 的输入为函数时进行参数校准。

# 4\. 问题 2：输入函数估计

该升级我们的游戏时间到了！在本节中，我们将解决一个更具挑战性的案例研究：估计输入函数 u(·)的整个轮廓。类似于前一节，我们将从简要讨论解决方案策略开始，然后实施提出的策略来解决我们的目标问题。我们将探讨两种不同的策略：一种使用原生 TensorFlow，另一种使用 L-BFGS 优化算法。

## 4.1 解决方案策略

为了制定我们的解决方案策略，我们需要考虑两件事：

1️⃣ 优化效率

从根本上讲，我们的目标反问题要求优化输入轮廓 u(·)，使得预测的 s(·)与观察到的 s(·)匹配。假设我们可以给定 u(·)计算 s(·)，许多现成的优化器可以用来实现我们的目标。现在的问题是，给定 u(·)我们如何计算 s(·)？

传统的方法是使用标准的数值 ODE 求解器来预测给定输入函数 u(·)的输出函数 s(·)。然而，优化过程通常涉及多个迭代。因此，使用数值 ODE 求解器来计算 s(·)的方法会太低效，因为在每次优化循环迭代中，u(·)会被更新，并且需要计算新的 s(·)。这就是 PI-DeepONet 发挥作用的地方。

作为第一步，我们需要一个完全训练好的 PI-DeepONet。由于我们对目标 ODE 有完全的了解（回顾一下在这个案例研究中，我们假设知道*a*和*b*的真实值），我们可以仅基于 ODE 残差损失轻松地训练一个 PI-DeepONet。

一旦 PI-DeepONet 训练完成，我们可以冻结其权重和偏置，将其视为一个快速的“替代”模型，可以高效地计算给定任何 u(·)的 s(·)，并将这个训练好的 PI-DeepONet 嵌入到优化例程中。由于 PI-DeepONet 的推理时间可以忽略不计，计算成本可以大大降低。同时，训练后的 PI-DeepONet 模型将是完全可微的。这表明我们能够提供优化目标函数相对于 u(·)的梯度，从而利用基于梯度的优化算法的效率。这两个方面使得迭代优化变得可行。

2️⃣ 优化量

当我们谈论估计 u(·) 的轮廓时，我们并不是试图估计 u(·) 的符号函数形式。而是估计**离散的 u(·) 值**，这些值是在固定时间坐标 *t*₁、*t*₂ 等上评估的。这也与我们如何将 u(·) 输入到 PI-DeepONet 中的方式一致。

通常，我们需要有大量离散的 u(·) 值来充分描述 u(·) 的轮廓。因此，我们的优化问题将是高维的，因为有许多参数需要优化。这种类型的问题本质上是病态的：仅仅找到一个能够生成匹配的 s(·) 的 u(·) 很可能不是一个很强的约束，因为很容易存在多个 u(·) 能生成完全相同的 s(·) 轮廓。我们需要更强的正则化来帮助约束解空间。

那么我们该怎么做呢？实际上，我们可以利用已知 ODE 带来的约束。简单来说，我们的目标是找到一个 u(·)，**不仅生成与观察到的 s(·) 匹配的 s(·)，而且满足规定 u(·) 和 s(·) 之间关系的已知 ODE**。

![](img/a373f0c2955a2f1cffab83f0b704f31b.png)

图 10\. 优化策略的示意图。（图像由作者提供）

现在这些问题已经澄清，让我们继续编写优化程序。

## 4.2 优化程序：TensorFlow

如我们之前讨论的，我们从训练一个观察已知 ODE 的 PI-DeepONet 开始。我们可以重用为第一个案例研究开发的代码。我们需要引入的唯一更改是：

1.  将 `a_init` 和 `b_init` 设置为 0.5，因为这些是 *a* 和 *b* 的真实值。

1.  将 `ParameterLayer` 的 `trainable` 标志设置为 False。这将防止在反向传播期间更新 *a* 和 *b* 的值。

1.  将 `data_weight` 设置为 0，因为我们不需要配对的 u(·)-s(·) 来进行训练。PI-DeepONet 可以仅基于 ODE 残差损失进行训练，正如我们在[之前的博客](https://medium.com/towards-data-science/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887)中所展示的那样。

下图展示了训练过程：损失值已经收敛，PI-DeepONet 已经得到适当训练。

![](img/23ffaeda1b53ce4053cf1446dae31371.png)

图 11\. PI-DeepONet 在没有配对输入输出数据的情况下的损失收敛。（图像由作者提供）

接下来，我们定义目标函数及其相对于 u(·) 的梯度，以用于我们的优化过程：

```py
@tf.function
def gradient_u(model, u, s_target):
    """ Calculate the gradients of the objective function with respect to u(·).

    Args:
    ----
    model: the trained PI-DeepONet model
    u: the current u()
    s_target: the observed s()

    Outputs:
    --------
    obj: objective function
    gradients: calculated gradients
    """

    with tf.GradientTape() as tape:
        tape.watch(u)

        # Initial condition loss
        y_pred_IC = model({"forcing": u, "time": 0})
        IC_loss = tf.reduce_mean(keras.losses.mean_squared_error(0, y_pred_IC))

        # ODE residual
        ODE_residual = ODE_residual_calculator(t=tf.reshape(tf.linspace(0.0, 1.0, 100), (-1, 1)), 
                                               u=tf.tile(tf.reshape(u, (1, -1)), [100, 1]), 
                                               u_t=tf.reshape(u, (-1, 1)), model=model)
        ODE_loss = tf.reduce_mean(tf.square(ODE_residual))

        # Predict s() with the current u()
        y_pred = model({"forcing": tf.tile(tf.reshape(u, (1, -1)), [100, 1]), 
                        "time": tf.reshape(tf.linspace(0.0, 1.0, 100), (-1, 1))})

        # Data loss
        data_loss = tf.reduce_mean(keras.losses.mean_squared_error(s_target, y_pred))

        # Objective function
        obj = IC_loss + ODE_loss + data_loss

    # Gradient descent
    gradients = tape.gradient(obj, u)

    return obj, gradients
```

如你所见，我们主要重新利用了在第一个案例研究中开发的 `train_step()` 函数来定义优化的目标函数。在梯度计算方面，两个函数的主要区别在于这里我们计算的是*目标函数（即总损失）相对于输入 u(·) 的梯度*，而 `train_step()` 计算的是*总损失相对于神经网络模型参数的梯度*。这很合理，因为之前我们想要优化神经网络参数（即权重和偏差），而在这里我们希望优化输入 u(·)。

还需要注意的是，我们将 u(·) 离散化为在 [0, 1] 区间内均匀分布的 100 个点。因此，我们手头的 u(·) 估计任务本质上是优化这 100 个离散的 u(·) 值。

接下来，我们定义了优化 u(·) 的主要逻辑：

```py
def optimize_u(model, initial_u, s_target, learning_rate=5e-3, opt_steps=10000):
    """ Determine u() that generates the observed s().

    Args:
    ----
    model: the trained PI-DeepONet model
    initial_u: initial guess for u()
    s_target: the observed s()
    learning_rate: learning rate for the optimizer
    opt_steps: number of optimization iterations

    Outputs:
    --------
    u: the optimized u()
    u_hist: intermedian u results
    objective_func_hist: value history of the objective function
    """

    # Optimizer
    optimizer = tf.keras.optimizers.Adam(learning_rate=learning_rate)

    # Preparation
    u_var = tf.Variable(initial_u, dtype=tf.float32)
    objective_func_hist = []
    u_hist = []

    for step in range(opt_steps):

        # Calculate gradients
        obj, gradients = gradient_u(model, u_var, s_target)

        # Gradient descent
        optimizer.apply_gradients([(gradients, u_var)])

        # Record results
        objective_func_hist.append(obj.numpy())
        u_hist.append(u_var.numpy())

        # Show progress
        if step % 500 == 0:
            print(f'Step {step} ==> Objective function = {obj.numpy()}')

    # Optimized u()
    u = u_var.numpy()

    return u, objective_func_hist, u_hist
```

随后，我们可以启动优化过程。对于这个案例研究，我们随机生成一个 u(·) 配置，并使用数值求解器计算其对应的 s(·) 配置。然后，我们将观察到的 s(·) 分配给 `s_target`，并测试我们的优化算法是否确实能够识别我们用来生成 s(·) 的 u(·)。

在初始化时，我们简单地使用全零的 u(·) 配置，并将其分配给 `initial_u`。下图显示了目标函数的收敛情况。

![](img/d376db67f81aed828f0ea09406cd4ad8.png)

图 12\. 我们使用固定学习率（5e-3）的 Adam 优化器来优化 u(·)。（图像由作者提供）

下图显示了 u(·) 配置的演变。我们可以看到，随着迭代次数的增加，u(·) 配置逐渐收敛到真实的 u(·)。

![](img/f1c552a5b7c13f001ffbcffe747e1ded.png)

图 13\. 在逆校准过程中，u(·) 逐渐收敛到真实的 u(·)，这表明 PI-DeepONet 方法能够成功地执行输入函数配置的逆校准。（图像由作者提供）

然后，我们可以执行通常的前向模拟，以获得给定优化 u(·) 的预测 s(·)。前向模拟通过数值 ODE 求解器和训练过的 PI-DeepONet 进行。我们可以看到，预测的 s(·) 紧密跟随实际值。

![](img/ecaa151acbcd93c1ac8f043e049c8e7f.png)

图 14\. 向前模拟以计算给定优化 u(·) 的 s(·)。预测结果与实际值非常吻合。（图像由作者提供）

我们还可以进一步测试我们的优化策略是否适用于其他 u(·) 配置。以下图表展示了我们在测试中使用的另外六个随机 u(·) 配置：

![](img/e45851dbe4f948a5f9ad5fa4cff149fa.png)

图 15\. 对于不同的 u(·) 配置，我们使用全零初始化运行了相同的基于 PI-DeepONet 的优化例程。结果表明我们的策略成功地提供了令人满意的结果。（图像由作者提供）

总体而言，结果显示 PI-DeepONet 方法能够成功地对整个输入函数轮廓进行反向校准。

## 4.3 优化例程：L-BFGS

我们上面开发的优化例程完全在 TensorFlow 环境中操作。实际上，我们还可以利用二阶优化方法，如 SciPy 中实现的 L-BGFS，来实现我们的目标。在本节中，我们将探讨如何将我们开发的 PI-DeepONet 集成到 L-BGFS 优化工作流中。

> 如果你想了解更多关于 L-BFGS 方法的信息，可以查看这篇博客文章：基于 L-BFGS 方法的数值优化

首先，我们定义了 L-BFGS 优化器的目标函数。目标函数再次是数据损失和 ODE 残差损失的组合：

```py
def obj_fn(u_var, model, s_target, convert_to_numpy=False):
    """ Calculate objective function.

    Args:
    ----
    u_var: u()
    model: the trained PI-DeepONet model
    s_target: the observed s()
    convert_to_numpy: boolean, indicate whether to convert the 
                      calculated objective value to numpy

    Outputs:
    --------
    obj: objective function
    """

    # Initial condition prediction
    y_pred_IC = model({"forcing": u_var, "time": 0})

    # ODE residual
    ODE_residual = ODE_residual_calculator(t=tf.reshape(tf.linspace(0.0, 1.0, 100), (-1, 1)), 
                                           u=tf.tile(tf.reshape(u_var, (1, -1)), [100, 1]), 
                                           u_t=tf.cast(tf.reshape(u_var, (-1, 1)), tf.float32), model=model)

    # Predict the output with the current inputs
    y_pred = model({"forcing": tf.tile(tf.reshape(u_var, (1, -1)), [100, 1]), 
                    "time": tf.reshape(tf.linspace(0.0, 1.0, 100), (-1, 1))})

    # Compute the data loss
    data_loss = tf.reduce_mean(keras.losses.mean_squared_error(y_target, y_pred))

    # Calculate other losses
    IC_loss = tf.reduce_mean(keras.losses.mean_squared_error(0, y_pred_IC))
    ODE_loss = tf.reduce_mean(tf.square(ODE_residual))

    # Obj function
    obj = IC_loss + ODE_loss + data_loss
    obj = obj if not convert_to_numpy else obj.numpy().astype(np.float64)

    return obj
```

此外，我们需要定义 `obj_fn` 的两个不同实例：一个用于返回 Numpy 对象，另一个用于返回 TensorFlow 对象。

```py
def obj_fn_numpy(u_var, model, s_target):
    return obj_fn(u_var, model, s_target, convert_to_numpy=True)

def obj_fn_tensor(u_var, model, s_target):
    return obj_fn(u_var, model, s_target, convert_to_numpy=False)
```

`obj_fn_numpy` 将由 SciPy 的 L-BFGS 用于计算主要的目标函数，而 `obj_fn_tensor` 将由 TensorFlow 用于计算目标函数关于 u(·) 的梯度，如下所示：

```py
def grad_fn(u, model, s_target):
    u_var = tf.Variable(u, dtype=tf.float32)

    with tf.GradientTape() as tape:
        tape.watch(u_var)
        obj = obj_fn_tensor(u_var, model, s_target)

    # Calculate gradients
    grads = tape.gradient(obj, u_var)

    return grads.numpy().astype(np.float64)
```

现在我们准备定义主要的优化逻辑：

```py
from scipy.optimize import minimize

def optimize_u(model, initial_u, s_target, opt_steps=1000):
    """Optimize the inputs to a model to achieve a target output.

    Args:
    ----
    model: the trained PI-DeepONet model
    initial_u: initial guess for u()
    s_target: the observed s()
    opt_steps: number of optimization iterations

    Outputs:
    --------
    res.x: the optimized u
    """

    # L-BFGS optimizer 
    res = minimize(fun=obj_fn_numpy, x0=initial_u, args=(model, s_target),
                   method='L-BFGS-B', jac=grad_fn, options={'maxiter': opt_steps, 'disp': True})

    return res.x
```

为了测试 L-BFGS 算法的有效性，我们采用了前一小节开始时研究的相同 u(·) 和 s(·)。我们还将初始 u(·) 设置为全零轮廓。经过 1000 次 L-BFGS 迭代，我们获得了以下优化结果：

![](img/082e63f40279dc4ba8dba9e542ea7dc7.png)

图 16\. 校准后的 u(·) 和预测的 s(·)。（图片作者提供）

我们可以看到，L-BFGS 算法也取得了预期的结果，成功估计了 u(·) 轮廓。我们进一步检查了六个不同的 u(·) 轮廓，并获得了类似的结果。

![](img/efc4559a620a0774752116669f814eb2.png)

图 17\. 使用 L-BFGS 算法对不同 u(·) 轮廓进行校准的结果。（图片作者提供）

# 5\. 重点总结

在这篇博客中，我们研究了如何使用 PI-DeepONet，这是一种结合了物理信息学习和算子学习优势的新型网络架构，来解决反问题。我们研究了两个案例：

## 1️⃣ 参数估计

我们的目标是给定观测到的输入函数 u(·) 和输出函数 s(·)，估计未知的 ODE 参数。我们的策略是将这些未知参数作为可训练参数纳入神经网络，并与神经网络的权重和偏置一起优化。

## 2️⃣ 输入函数校准

我们的目标是给定完全已知的 ODE 和观察到的 s(·)来估计整个 u(·)曲线。我们的策略是首先基于已知的 ODE 训练一个 PI-DeepONet，作为快速的替代模型，然后优化离散化的 u(·)值，以使生成的 s(·)不仅与观察到的真实情况匹配，而且也满足连接 u(·)和 s(·)的已知 ODE。

对于这两个案例研究，我们已经看到 PI-DeepONet 方法产生了令人满意的结果，这表明该方法可以有效地解决正向和逆向问题，从而作为动态系统建模的有前途的工具。

逆向问题在计算科学和工程中提出了独特的挑战，而我们在本博客中仅仅触及了这一领域的表面。对于许多实际应用，我们还需要进一步考虑其他方面：

## 1️⃣ 不确定性量化（UQ）

在本博客中，我们简单地假设观察到的输入和输出函数值是完美的。然而，在实际场景中，输入和输出数据可能会受到噪声的污染。理解这种不确定性如何在我们的模型中传播对于做出可靠的估计至关重要。

## 2️⃣ 建模误差

在本博客中，我们假设我们的 ODE 完美地描述了系统的动态。然而，在实际场景中，这一假设很可能不成立：微分方程可能只是对真实物理过程的近似，还有一些物理现象没有被方程捕捉到。如何正确考虑这些引入的建模误差可能是一个具有挑战性但又有趣的方面。

## 3️⃣ 扩展到 PDE

在本博客中，我们展示了解决逆向问题的工作流程，仅考虑了一个简单的 ODE。然而，许多实际系统由具有更多空间/时间坐标和复杂输入/输出功能特征的 PDE 支配。当我们将开发的方法扩展到 PDE 时，可能会遇到更多挑战（例如，计算效率可能成为主要问题，不仅在训练 PI-DeepONet 时，还在进行逆向标定优化时）。

恭喜你看到这里！如果你觉得我的内容有用，可以在[这里](https://www.buymeacoffee.com/Shuaiguo09f)请我喝杯咖啡🤗 非常感谢你的支持！

> 你可以在[这里](https://github.com/ShuaiGuo16/PI-DeepONet/tree/main)找到带有完整代码的配套笔记本 *💻*
> 
> 要了解更多关于**正向建模**的 PI-DeepONet：通过物理信息深度操作网络进行的操作学习
> 
> 要了解物理信息学习的最佳实践：[揭示物理信息神经网络的设计模式](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-series-01-8190df459527)
> 
> 你也可以订阅我的[通讯](https://shuaiguo.medium.com/subscribe)或在[Medium](https://shuaiguo.medium.com/)上关注我。

# 参考文献

[1] 王等，利用物理信息深度网络学习参数化偏微分方程的解算子。arXiv，2021 年。
