# 功能性 PyTorch 入门

> 原文：[`towardsdatascience.com/introduction-to-functional-pytorch-b5bf739e1e6e?source=collection_archive---------1-----------------------#2023-05-07`](https://towardsdatascience.com/introduction-to-functional-pytorch-b5bf739e1e6e?source=collection_archive---------1-----------------------#2023-05-07)

## 如何使用 `write Jax-style PyTorch models`

[](https://mariodagrada.medium.com/?source=post_page-----b5bf739e1e6e--------------------------------)![Mario Dagrada](https://mariodagrada.medium.com/?source=post_page-----b5bf739e1e6e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5bf739e1e6e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5bf739e1e6e--------------------------------) [Mario Dagrada](https://mariodagrada.medium.com/?source=post_page-----b5bf739e1e6e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99ed96040994&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-functional-pytorch-b5bf739e1e6e&user=Mario+Dagrada&userId=99ed96040994&source=post_page-99ed96040994----b5bf739e1e6e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5bf739e1e6e--------------------------------) · 6 分钟阅读 · 2023 年 5 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5bf739e1e6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-functional-pytorch-b5bf739e1e6e&user=Mario+Dagrada&userId=99ed96040994&source=-----b5bf739e1e6e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5bf739e1e6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-functional-pytorch-b5bf739e1e6e&source=-----b5bf739e1e6e---------------------bookmark_footer-----------)![](img/363519458d8628ce115a6d939b8f089d.png)

照片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

PyTorch 最近在 2.0 版本中将 `torch.func` 模块集成到了主代码库中。该模块，之前被称为 `functorch`，使得在 PyTorch 中以简洁的 API 开发纯函数式神经网络模型成为可能。这个包是 PyTorch 对 [Jax](https://github.com/google/jax) 的日益[流行](https://www.assemblyai.com/blog/why-you-should-or-shouldnt-be-using-jax-in-2023/#:~:text=Since%20Google's%20JAX%20hit%20the,and%20others%20are%20using%20JAX.) 的回应，Jax 是一个从底层使用函数式编程范式构建的 Python 通用可微编程框架。

在这篇文章中，我们将首先介绍 `torch.func` 的基础知识，然后通过一个**简单的端到端示例**来使用神经网络（NN）模型拟合非线性函数。虽然使用 NN 完成这项任务显然有些过度，但它在说明目的上效果很好。此外，我们还将发现构建 NN 模型时采用函数式方法的一些好处。

# 编写一个函数式模型

使用 `torch.func` 的方法与标准 PyTorch 开始时相同：你需要构建一个神经网络。为了简单起见，我们定义一个非常简单的网络，由任意数量的线性层和非线性激活函数组成。前向传播接收一批数据点作为输入，并对模型进行评估。

```py
class SimpleNN(nn.Module):
    def __init__(
        self,
        num_layers: int = 1,
        num_neurons: int = 5,
    ) -> None:
        """Basic neural network architecture with linear layers

        Args:
            num_layers (int, optional): number of hidden layers
            num_neurons (int, optional): neurons for each hidden layer
        """
        super().__init__()

        layers = []

        # input layer
        layers.append(nn.Linear(1, num_neurons))

        # hidden layers with linear layer and activation
        for _ in range(num_layers):
            layers.extend([nn.Linear(num_neurons, num_neurons), nn.Tanh()])

        # output layer
        layers.append(nn.Linear(num_neurons, 1))

        # build the network
        self.network = nn.Sequential(*layers)

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        return self.network(x.reshape(-1, 1)).squeeze()
```

现在事情变得有趣了。请记住，`torch.func` 允许构建纯函数式模型。但这在实践中意味着什么？

首先，需要掌握函数式编程的核心概念：[**纯函数**](https://en.wikipedia.org/wiki/Pure_function)。本质上，纯函数有两个定义特征：

+   对于相同的输入参数，其返回值是相同的

+   它没有副作用，即不会以任何方式修改其输入参数

根据这个定义，大多数标准 PyTorch 模块的方法不是纯粹的，因为参数存储在 PyTorch 模型中。换句话说，标准 PyTorch 模型是**有状态的**，而不是无状态的，这符合函数式范式。考虑以下代码：

```py
import torch

x = torch.randn(10)
model = SimpleNN() # constructed above
optimizer = torch.optim.SGD(model.parameters())

# modify the state of the model
# by applying a single optimization step
out1 = model(x)
model.backward()
optimizer.step()

# recompute the output with exactly the same input
out2 = model(x)
assert not torch.equal(out1, out2)
```

模型的前向传播对相同输入参数的输出不同，因为优化器在原地更新了参数。

使 PyTorch 模块变得纯粹的一种方法是将参数与模型解耦，从而使模型完全**无状态**。这正是 `torch.func.functional_call()` 例程所做的：

```py
import torch
from torch.func import functional_call

x = torch.randn(10) # random input data
model = SimpleNN() # constructed above
params = dict(model.named_parameters()) # model parameters

# make a functional call to the model above
out = functional_call(model, params, (x,))
```

现在我们已经拥有了纯函数式的前向传播，用于我们的神经网络模型，我们可以探索如何使用 PyTorch 的函数式 API 提供的可组合原语。这使我们能够使用每个具有自身函数实现的模块化构建块来构建复杂模型。

# 可组合函数转换

我刚刚展示了如何为我们的模型定义一个完全功能的前向传递。但是我们如何定义微分规则和损失函数呢？我们需要使用`torch.func` 提供的 **可组合的** **函数变换**。

函数变换由一组例程组成，每个例程返回一个函数，该函数可以用于评估特定的量。这种返回另一个函数的函数被称为 *高阶函数*。例如，可以使用 `grad` 原语来计算相对于输入数据 `x` 的梯度，如下所示：

```py
from torch.func import grad

# the `grad` function returns another function
# which takes the same inputs as the model forward pass
grad_fn = grad(model)

# now this function can be used to compute gradients
# with respect to the first input
params = tuple(model.parameters())
grad_values = grad_fn(x[0], params)
```

请注意，默认情况下，`grad` 函数应用于单个数字。可以使用另一个函数变换 `vmap` 来高效地处理输入批次。请注意，`vmap` 在多个 CPU 或 GPU 可用时也会自动进行并行化，而无需任何代码更改。

`torch.func` 模块中所有函数都是纯函数的一个重要后果是它们能够被任意组合（因此它们的名称中有“可组合”一词）。这是因为纯函数总是可以用其结果替代，而不影响程序执行，这是上述两个属性的直接结果。

有了这些考虑，让我们计算一批输入数据`x`的二阶导数：

```py
from torch.func import grad, vmap

x = torch.rand(10)

# combine twice `grad` with `vmap` to compute
# the model second order derivative (Laplacian) with
# respect to batched input data
laplacian_fn = vmap(grad(grad(model)))
params = tuple(model.parameters())
out = laplacian_fn(x, params)
```

值得注意的是，模型的前向传递不接受参数作为输入。因此，为了计算相对于参数的梯度，我们需要定义一个辅助的 `make_functional_fwd` 例程，带有适当的参数。实际上，我们可以使用闭包来实现，如下所示：

```py
import torch
from torch.func import functional_call, grad

x = torch.randn(1) # random input data point
model = SimpleNN() # constructed above

# forward pass using the functional API
# to take the parameters as input arguments
def make_functional_fwd(_model):
    def fn(data, parameters):
        return functional_call(_model, parameters, (data,))
    return fn

model_func = make_functional_fwd(my_model) # functional forward
params = tuple(my_model.parameters()) # model parameters

# the `argnums` argument allows to select with
# respect to which input argument of the functional forward
# pass defined in the closure
grad_params = grad(model_func, argnums=1)(x[0], params)

# as before but for computing the gradient with
# respect to the input data
grad_x = grad(model_func, argnums=0)(x[0], params)
```

`torch.func` 模块提供了许多可组合的函数变换，例如用于计算向量-雅可比积的变换。[这里](https://pytorch.org/functorch/stable/notebooks/whirlwind_tour.html#why-composable-function-transforms)你可以找到更多详细信息。

# 使用功能模型进行优化

如果你读到这里，你可能会想知道如何使用功能模型进行基于梯度的优化。毕竟，标准的 PyTorch 优化器通过就地修改模型参数来工作，而正如我刚刚展示的，这破坏了纯函数的要求。

不幸的是，PyTorch 本身并未提供功能优化器。然而，可以使用`[torchopt](https://github.com/metaopt/torchopt)` 库来实现这一目的。

为了展示功能优化的工作原理，我们假设我们要拟合一个简单的函数，例如 *f(x) = 2 sin(x + 2π)*，使用一些在区间 *[0, 2π]* 内的随机输入点。我们可以生成一些训练和测试数据点，如下所示：

```py
import torch

def get_data(n_points = 20):
  x = torch.rand(n_points) * 2.0 * torch.pi
  y = 2.0 * torch.sin(x + 2.0 * torch.pi)
  return x, y

x_train, y_train = get_data(n_points=40)
x_test, y_test = get_data(n_points=10)
```

现在让我们使用`torchopt`和 PyTorch 函数 API 来训练我们的神经网络以拟合这个函数。

```py
import torch
import torchopt

# hyperparameters and optimizer choice from `torchopt`
num_epochs = 500
lr = 0.01
optimizer = torchopt.FuncOptimizer(torchopt.adam(lr=lr))
loss_fn = torch.nn.MSELoss()

loss_evolution = []  # track the loss evolution per epoch
params = tuple(model.parameters())  # initialize the parameters

for i in range(num_epochs):

  # update the parameters using the functional API
  y = model_func(x_train, params)
  loss = loss_fn(y, y_train)
  params = optimizer.step(loss, params)
  loss_evolution.append(float(loss))

  if i % 100 == 0:
      print(f"Iteration {i} with loss {float(loss)}")

# accuracy on test set
y_pred = model_func(x_test, params)
print(f"Loss on the test set: {loss_fn(y_pred, y_test)}")
```

正如你所看到的，优化循环与标准的 PyTorch 非常相似，关键区别在于优化器步骤现在需要当前损失和当前参数值，并以完全无状态的方式评估更新后的参数。在我看来，这种方法看起来比 PyTorch 典型的有状态 API 要干净得多！

如果你想获取本博客文章的完整代码，可以查看[此代码片段](https://gist.github.com/madagra/64afe1b56ff5656b2b1acb19cc68f477)。正如你所注意到的那样，一些实现细节（特别是`torch.func.functional_call`与`torchopt`优化器之间的交互）在本博客中没有涉及。如果你有任何问题，请随时在[Linkedn](https://www.linkedin.com/in/mariodagrada/)上给我发消息。

# 结论

感谢阅读本博客文章。PyTorch 的函数式 API 是一个强大的工具，可以帮助你编写高性能的神经网络模型，并利用可组合函数、自动并行化和矢量化，类似于 Jax。然而，它仍然是一个实验性的功能，应谨慎使用。祝你编程愉快！

[1] 与 Jax 的简洁比较：[使用 PyTorch 和 functorch 编写类似 JAX 的代码教程 — Simone Scardapane (sscardapane.it)](https://www.sscardapane.it/tutorials/functorch/)

[2] 虽然有点老但解释得非常好的教程：[使用 FuncTorch 工作：介绍 | functorch-examples — Weights & Biases (wandb.ai)](https://wandb.ai/functorch-examples/functorch-examples/reports/Working-with-FuncTorch-An-Introduction--VmlldzoxNzMxNDI1)
