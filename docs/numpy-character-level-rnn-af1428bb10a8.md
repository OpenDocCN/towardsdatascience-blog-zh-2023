# 探究字符级RNN：基于NumPy的实现指南

> 原文：[https://towardsdatascience.com/numpy-character-level-rnn-af1428bb10a8?source=collection_archive---------5-----------------------#2023-01-27](https://towardsdatascience.com/numpy-character-level-rnn-af1428bb10a8?source=collection_archive---------5-----------------------#2023-01-27)

## 由于最近LLMs蓬勃发展，掌握语言建模的基础知识至关重要

[](https://sassonjoe66.medium.com/?source=post_page-----af1428bb10a8--------------------------------)![Joe Sasson](../Images/f0edde425f64b6b09d3d8d4adc953d2d.png)](https://sassonjoe66.medium.com/?source=post_page-----af1428bb10a8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af1428bb10a8--------------------------------)![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----af1428bb10a8--------------------------------) [Joe Sasson](https://sassonjoe66.medium.com/?source=post_page-----af1428bb10a8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F32a49c3f499a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-character-level-rnn-af1428bb10a8&user=Joe+Sasson&userId=32a49c3f499a&source=post_page-32a49c3f499a----af1428bb10a8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af1428bb10a8--------------------------------) ·17 min read·Jan 27, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf1428bb10a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-character-level-rnn-af1428bb10a8&user=Joe+Sasson&userId=32a49c3f499a&source=-----af1428bb10a8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf1428bb10a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-character-level-rnn-af1428bb10a8&source=-----af1428bb10a8---------------------bookmark_footer-----------)![](../Images/43b8bc23a16ed2edb617b4bdee1b1852.png)

图片来自 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 简介

循环神经网络（RNNs）是一种强大的神经网络类型，能够处理序列数据，如时间序列或自然语言。本文将通过使用 NumPy 从零开始构建一个 Vanilla RNN 的过程。我们将首先讨论 RNN 的理论和直觉，包括它们的架构和适合解决的问题类型。接下来，我们将深入代码，解释 RNN 的各个组件及其相互作用。最后，我们将通过将 RNN 应用于实际数据集来展示其有效性。

具体来说，我们将实现一个多对多的字符级 RNN，使用序列化的在线学习。这意味着网络一次处理一个字符的输入序列，并在每个字符后更新网络参数。这允许网络在实时学习，并随着遇到的新模式而适应数据。

字符级 RNN 意味着输入和输出是单个字符，而不是单词或句子。这使得网络能够学习文本中字符之间的潜在模式和依赖关系。多对多架构指的是网络接收一个字符序列作为输入，并生成一个字符序列作为输出。这与多对一架构不同，在多对一架构中，网络接收一个输入序列并生成一个输出，或者与一对多架构不同，在一对多架构中，网络接收一个输入并生成一个输出序列。

我使用了 Andrej Karpathy 的代码（见[这里](https://gist.github.com/karpathy/d4dee566867f8291f086)）作为我的实现基础，并做了几处修改以提高通用性和可靠性。我扩展了代码以支持多个层次，并重新构建了它以提高可读性和重用性。这个项目建立在我之前使用 NumPy 创建简单 ANN 的工作基础上。相关源代码可以在[这里](https://github.com/j0sephsasson/numpy-NN)找到。

# 理论与直觉

RNNs 可以与传统的前馈神经网络（ANNs）对比，后者没有“记忆”机制，并且独立处理每个输入。ANNs 适用于输入和输出具有固定大小且输入不包含序列依赖关系的问题。相比之下，RNNs 能够处理变长的输入序列，并通过***隐藏状态***保持对过去输入的“记忆”。

隐藏状态使RNN能够捕捉时间依赖关系，并根据整个输入序列做出预测。总的来说，网络使用来自先前时间步的信息来指导其对当前输入的处理。此外，更复杂的NLP架构可以处理长期依赖关系（GPT-3使用了2048的序列长度进行训练），其中输入序列开头的信息对于预测序列末尾的输出仍然相关。这种保持“记忆”的能力使RNN和变换器在处理序列数据时相较于ANNs具有显著优势。

最近，像[GPT-3](https://en.wikipedia.org/wiki/GPT-3)和[BERT](https://en.wikipedia.org/wiki/BERT_(language_model))这样的变换器架构在各种NLP任务中变得越来越流行。这些架构基于[自注意力](https://machinelearningmastery.com/the-transformer-attention-mechanism/)机制，使网络能够有选择地关注输入序列的不同部分。这使得网络能够捕捉长期依赖关系，而无需递归，从而比RNN更高效且更易于训练。变换器架构已在各种NLP任务中实现了最先进的结果，并被用于许多实际应用中。

尽管变换器架构比普通RNN更复杂且具有不同的特性，但普通RNN在深度学习领域仍然发挥着重要作用。它们易于理解，容易实现和调试，并可以作为其他更复杂架构的构建块。在本文中，我们将重点关注普通RNN，并窥探其真正的工作原理。

## 普通RNN的三种主要类型是：

+   ***一对多：*** 输入一张狗的图片并输出‘狗的图片’

+   ***多对一：*** 输入一个句子并接收情感（情感分析）

+   ***多对多：*** 输入一个句子并输出完整句子（见下文）

我们将实现如下所示的多对多架构。

![](../Images/34501c57a1b305a03d6a8d446315f618.png)

**来源：** Kaivan Kamali，《深度学习（第二部分）——递归神经网络（RNN）》（银河培训材料）。[https://training.galaxyproject.org/training-material/topics/statistics/tutorials/RNN/tutorial.html](https://training.galaxyproject.org/training-material/topics/statistics/tutorials/RNN/tutorial.html)

从这一点开始，我们将使用`h[t]`表示时间步t的隐藏状态。在图中，这被表示为`s[t]`。

如你所见，来自上一个时间步的隐藏状态`h[t-1]`与当前输入`x[t]`结合，这一过程在时间步数上重复。在RNN块内，我们正在更新当前时间步的隐藏状态。

为了澄清，时间步只是一个字符，如 ‘a’ 或 ‘d’。输入序列包含一个可变数量的字符或时间步，也称为序列长度，这是网络的一个超参数。

# 代码

## ***目录***

1.  准备数据

1.  RNN 类

1.  前向传播

1.  反向传播

1.  优化器

1.  训练

## 准备数据

```py
## start with data
data = open('path-to-data', 'r').read() # should be simple plain text file

chars = list(set(data))
data_size, vocab_size = len(data), len(chars)

print('data has {} characters, {} unique.'.format(data_size, vocab_size))

char_to_idx = { ch:i for i,ch in enumerate(chars) }
idx_to_char = { i:ch for i,ch in enumerate(chars) }
```

我们从一个纯文本文件中读取数据作为字符串，并对字符进行标记化。每个唯一字符（共 65 个）将映射到一个整数，反之亦然。

让我们为 RNN 采样一个输入和目标序列。

```py
pointer, seq_length = 0, 8

x = [char_to_idx[ch] for ch in data[pointer:pointer+seq_length]]

y = [char_to_idx[ch] for ch in data[pointer+1:pointer+seq_length+1]]

print(x)
>> [2, 54, 53, 62, 13, 28, 20, 54] # our RNN input sequence

print(y)
>> [54, 53, 62, 13, 28, 20, 54, 13] # our RNN target sequence

for t in range(seq_length):
    context = x[:t+1]
    target = y[t]
    print(f"when input is {context} the target: {target}")

>>  when input is [2] the target: 54
    when input is [2, 54] the target: 53
    when input is [2, 54, 53] the target: 62
    when input is [2, 54, 53, 62] the target: 13
    when input is [2, 54, 53, 62, 13] the target: 28
    when input is [2, 54, 53, 62, 13, 28] the target: 20
    when input is [2, 54, 53, 62, 13, 28, 20] the target: 54
    when input is [2, 54, 53, 62, 13, 28, 20, 54] the target: 13
```

输入是一个标记化的序列，目标是输入偏移一个单位后的值。

## RNN 类

```py
class RNN:
    def __init__(self, hidden_size, vocab_size, seq_length, num_layers):
        pass 

    def __call__(self, *args: Any, **kwds: Any):
        """RNN Forward Pass"""

        pass 

    def backward(self, targets, cache):
        """RNN Backward Pass"""

        pass

    def update(self, grads, lr):
        """Perform Parameter Update w/ Adagrad"""

        pass

    def predict(self, hprev, seed_ix, n):
        """
        Make predictions using the trained RNN model.

        Parameters:
        hprev (numpy array): The previous hidden state.
        seed_ix (int): The seed letter index to start the prediction with.
        n (int): The number of characters to generate for the prediction.

        Returns:
        ixes (list): The list of predicted character indices.
        hs (numpy array): The final hidden state after making the predictions.
        """

        pass 
```

让我们开始讨论 RNN 的组件，与基本 ANN 相比。

在传统的前馈神经网络中，控制层间交互的参数由一个单一的权重矩阵表示，记作 `W`。然而，在递归神经网络（RNN）中，层间交互由多个矩阵表示。在我的代码中，这些矩阵特别是：***Wxh***、***Whh*** 和 ***Why***，分别表示输入层与隐藏层、隐藏层与隐藏层、以及隐藏层与输出层之间的权重。

`Wxh` 矩阵将输入层连接到隐藏层，并用于将每个时间步的输入转换为隐藏层的激活值。`Whh` 矩阵将时间步 t-1 的隐藏层连接到时间步 t 的隐藏层，并用于将隐藏状态从一个时间步传播到下一个时间步。`Why` 矩阵将隐藏层连接到输出层，并用于将隐藏状态转换为网络的最终输出。

总之，ANN 和 RNN 之间的主要区别在于 ANN 有一个权重矩阵，而 RNN 有多个权重矩阵，这些矩阵用于转换输入、传播隐藏状态并生成最终输出。这些多个权重矩阵使 RNN 能够保持对过去输入的记忆，并在时间上移动信息。

> 构造函数

```py
def __init__(self, hidden_size, vocab_size, seq_length, num_layers):
    self.name = 'RNN'
    self.hidden_size = hidden_size
    self.vocab_size = vocab_size
    self.num_layers = num_layers

    # model parameters
    self.Wxh = [np.random.randn(hidden_size, vocab_size)*0.01 for _ in range(num_layers)] # input to hidden
    self.Whh = [np.random.randn(hidden_size, hidden_size)*0.01 for _ in range(num_layers)] # hidden to hidden
    self.Why = np.random.randn(vocab_size, hidden_size)*0.01 # hidden to output
    self.bh = [np.zeros((hidden_size, 1)) for _ in range(num_layers)] # hidden bias
    self.by = np.zeros((vocab_size, 1)) # output bias

    # memory variables for training (ada grad from karpathy's github)
    self.iteration, self.pointer = 0, 0
    self.mWxh = [np.zeros_like(w) for w in self.Wxh]
    self.mWhh = [np.zeros_like(w) for w in self.Whh] 
    self.mWhy = np.zeros_like(self.Why)
    self.mbh, self.mby = [np.zeros_like(b) for b in self.bh], np.zeros_like(self.by)
    self.loss = -np.log(1.0/vocab_size)*seq_length # loss at iteration 0

    self.running_loss = []
```

在这里，我们定义了如上所述的 RNN 参数。值得注意的是——参数 `Why` 和 `by` 表示一个线性层，甚至可以进一步抽象为一个独立的类，如 PyTorch 的 ‘nn.Linear’ 模块。然而，在此实现中，我们将它们作为 RNN 类的一部分保留。

## 前向传播

```py
def __call__(self, *args: Any, **kwds: Any) -> Any:
    """RNN Forward Pass"""

    x, y, hprev = kwds['inputs'], kwds['targets'], kwds['hprev']

    loss = 0
    xs, hs, ys, ps = {}, {}, {}, {} # inputs, hidden state, output, probabilities
    hs[-1] = np.copy(hprev)

    # forward pass
    for t in range(len(x)):
        xs[t] = np.zeros((self.vocab_size,1)) # encode in 1-of-k representation
        xs[t][x[t]] = 1
        hs[t] = np.copy(hprev)

        if kwds.get('dropout', False): # use dropout layer (mask)

            for l in range(self.num_layers):
                dropout_mask = (np.random.rand(*hs[t-1][l].shape) < (1-0.5)).astype(float)
                hs[t-1][l] *= dropout_mask
                hs[t][l] = np.tanh(np.dot(self.Wxh[l], xs[t]) + np.dot(self.Whh[l], hs[t-1][l]) + self.bh[l]) # hidden state
                hs[t][l] = hs[t][l] / (1 - 0.5)

        else: # no dropout layer (mask)

            for l in range(self.num_layers):
                hs[t][l] = np.tanh(np.dot(self.Wxh[l], xs[t]) + np.dot(self.Whh[l], hs[t-1][l]) + self.bh[l]) # hidden state

        ys[t] = np.dot(self.Why, hs[t][-1]) + self.by # unnormalized log probabilities for next chars
        ps[t] = np.exp(ys[t]) / np.sum(np.exp(ys[t])) # probabilities for next chars
        loss += -np.log(ps[t][y[t],0]) # softmax (cross-entropy loss)

    self.running_loss.append(loss)

    return loss, hs[len(x)-1], {'xs':xs, 'hs':hs, 'ps':ps}
```

从顶部开始，逐步解析。这儿发生了什么？

**循环外，每个序列一次**

+   ***hprev*** 是我们的初始隐藏状态

+   我们正在初始化字典以保存我们的输入、隐藏状态、logits 和概率。我们将在反向传播过程中需要这些。

+   我们将损失初始化为零。

+   我们将初始隐藏状态 `hprev` 设置为 `hs[-1]`（表示时间步 t-1）。

**循环内，每个时间步**

+   对我们的输入序列进行独热编码

+   在时间步‘t’和层‘l’更新隐藏状态。

![](../Images/3a81673b881f0d7431084a89ec104840.png)

我们隐藏状态的数学表示。图片由作者提供。

+   此外，你可能注意到有一些功能用于执行***dropout***。

Dropout是一种[正则化](/regularization-in-deep-learning-l1-l2-and-dropout-377e75acc036)技术，旨在通过在训练过程中随机“丢弃”（置为零）某些神经元来防止过拟合。在上述代码中，dropout层在更新时间步‘t’、层‘l’的隐藏状态之前应用，通过将时间步‘t-1’的隐藏状态与dropout掩码相乘来实现。dropout掩码是通过创建一个二值掩码生成的，其中每个元素以概率p为1，否则为0。通过这样做，我们在隐藏状态中随机“丢弃”一定数量的神经元，这有助于防止网络对任何单一神经元过于依赖。这使得网络更为鲁棒，并且不容易在训练数据上过拟合。在应用dropout之后，通过将隐藏状态除以（1-p）来缩放隐藏状态，以确保隐藏状态的期望值得到保持。

+   `ys[t]`为当前时间步提供线性层的输出。

+   `ps[t]`为当前时间步提供最终的softmax输出（概率）。

由于只有一层线性层，而不是任意数量的RNN层，因此`ys[t]`和`ps[t]`的计算位于第二个循环之外。

+   最后，我们返回损失，并且`hs[len(x)-1]`被用作下一个序列的`hprev`。我们使用缓存来获取反向传播过程中的梯度。

选择使用索引`[t][l]`来存储在时间步‘t’处第‘l’层的隐藏状态。这是因为模型一次处理一个时间步的输入序列，并且在每个时间步，它更新每一层的隐藏状态。通过使用索引`[t][l]`，我们能够跟踪每一层在每个时间步的隐藏状态，从而方便地执行前向传递所需的计算。

此外，这种索引允许轻松访问最后一个时间步的隐藏状态，该状态作为`hs[len(x)-1]`返回，因为它是每层序列中最后一个时间步的隐藏状态。这个返回的隐藏状态在训练过程中被用作下一个序列的初始隐藏状态。

让我们进行前向传递。请记住，没有批量维度。

```py
# Initialize RNN
num_layers = 3
hidden_size = 100
seq_length = 8

rnn = RNN(hidden_size=hidden_size, vocab_size=vocab_size, seq_length=seq_length, num_layers=num_layers)

x = [char_to_idx[ch] for ch in data[rnn.pointer:rnn.pointer+seq_length]]

y = [char_to_idx[ch] for ch in data[rnn.pointer+1:rnn.pointer+seq_length+1]]

# initialize hidden state with zeros
hprev = [np.zeros((hidden_size, 1)) for _ in range(num_layers)] 

## Call RNN
loss, hprev, cache = rnn(inputs=x, targets=y, hprev=hprev)

print(loss)
>> 33.38852380987117
```

## 反向传播

首先，了解RNN反向传播的一些直观感受。

基本ANN和RNN在反向传播中的关键区别在于错误在网络中的传播方式。虽然ANN和RNN都将错误从输出层传播到输入层，但RNN还会通过时间反向传播错误，在每个时间步调整权重和偏置。这使得RNN能够处理序列数据，并以隐藏状态的形式维持“记忆”。

BPTT（时间反向传播）算法通过展开RNN并为每个时间步创建计算图来工作。这个网络的计算图可以在[这里](https://github.com/j0sephsasson/numpy-RNN/blob/main/README.md#dag-directed-acyclic-graph)查看。

然后为每个时间步计算梯度，并在整个序列上累积。

```py
def backward(self, targets, cache):
    """RNN Backward Pass"""

    # unpack cache
    xs, hs, ps = cache['xs'], cache['hs'], cache['ps']

    # initialize gradients to zero
    dWxh, dWhh, dWhy = [np.zeros_like(w) for w in self.Wxh], [np.zeros_like(w) for w in self.Whh], np.zeros_like(self.Why)
    dbh, dby = [np.zeros_like(b) for b in self.bh], np.zeros_like(self.by)
    dhnext = [np.zeros_like(h) for h in hs[0]]

    for t in reversed(range(len(xs))):

        dy = np.copy(ps[t])

        # backprop into y. see http://cs231n.github.io/neural-networks-case-study/#grad if confused here
        dy[targets[t]] -= 1 

        dWhy += np.dot(dy, hs[t][-1].T)
        dby += dy

        for l in reversed(range(self.num_layers)):
            dh = np.dot(self.Why.T, dy) + dhnext[l]
            dhraw = (1 - hs[t][l] * hs[t][l]) * dh # backprop through tanh nonlinearity
            dbh[l] += dhraw
            dWxh[l] += np.dot(dhraw, xs[t].T)
            dWhh[l] += np.dot(dhraw, hs[t-1][l].T)
            dhnext[l] = np.dot(self.Whh[l].T, dhraw)

    return {'dWxh':dWxh, 'dWhh':dWhh, 'dWhy':dWhy, 'dbh':dbh, 'dby':dby}
```

与前向传递相同，让我们分解它。

这个函数的第一步是将权重和偏置的梯度初始化为零，这类似于前馈ANN中的情况。这是让我困惑的一点，因此我会进一步详细解释。

> 通过在每个序列之前将梯度重置为零，确保当前序列计算的梯度不会与先前序列计算的梯度累积或叠加。

这可以防止梯度变得过大，从而导致优化过程发散并对模型性能产生负面影响。此外，它允许对每个序列独立执行权重更新，这可以导致更稳定和一致的优化。

然后，它会反向遍历输入序列，对每个时间步t执行以下计算：

> 注意注释，回传到y，这个链接将完美解释发生了什么。我也在之前的文章中深入探讨了这一点，你可以在[这里](/coding-a-neural-network-from-scratch-in-numpy-31f04e4d605)查看。

+   计算隐藏状态`hs[t][l]`相对于损失的梯度，用`dh`表示。

+   计算原始隐藏状态的梯度，用`dhraw`表示

**dh和dhraw的区别是什么？** 好问题。

`dh`和`dhraw`的区别在于，`dh`是相对于损失的隐藏状态`hs[t][l]`的梯度，通过反向传播输出层softmax激活的概率`ps[t]`的梯度计算得出。`dhraw`是相同的梯度，但它进一步通过非线性tanh激活函数反向传播，通过元素级相乘隐藏状态`dh`的梯度与tanh函数的导数（1 - `hs[t][l]` * `hs[t][l]`）得到。

+   计算隐藏偏置`bh[l]`的梯度

+   计算相对于损失的输入-隐藏权重`Wxh[l]`的梯度，用`dWxh[l]`表示。

+   计算相对于损失的隐藏-隐藏权重`Whh[l]`的梯度，用`dWhh[l]`表示。

+   计算下一个隐藏状态`dhnext[l]`的梯度。

![](../Images/e91d9fe6514462f99b70442da08f5562.png)

反向传递计算的数学符号。作者提供的图像。

让我们进行反向传递。

```py
# Initialize RNN
num_layers = 3
hidden_size = 100
seq_length = 8

rnn = RNN(hidden_size=hidden_size, vocab_size=vocab_size, seq_length=seq_length, num_layers=num_layers)

x = [char_to_idx[ch] for ch in data[rnn.pointer:rnn.pointer+seq_length]]

y = [char_to_idx[ch] for ch in data[rnn.pointer+1:rnn.pointer+seq_length+1]]

# initialize hidden state with zeros
hprev = [np.zeros((hidden_size, 1)) for _ in range(num_layers)] 

## Call RNN
loss, hprev, cache = rnn(inputs=x, targets=y, hprev=hprev)
grads = rnn.backward(targets=y, cache=cache)
```

最后，我们返回梯度，以便更新参数，这也是我下一个话题——优化的引子。

## 优化器

如RNN类中的‘update’方法所述，我们将使用Adagrad进行这个实现。

> Adagrad 是一种优化算法，它根据历史梯度信息为神经网络中的每个参数单独调整学习率。

它特别适用于处理稀疏数据，通常用于自然语言处理任务。Adagrad 在每次迭代时调整学习率，确保模型尽可能快速和高效地从数据中学习。

```py
def update(self, grads, lr):
    """Perform Parameter Update w/ Adagrad"""

    # unpack grads
    dWxh, dWhh, dWhy = grads['dWxh'], grads['dWhh'], grads['dWhy']
    dbh, dby = grads['dbh'], grads['dby']

    # loop through each layer
    for i in range(self.num_layers):

        # clip gradients to mitigate exploding gradients
        np.clip(dWxh[i], -5, 5, out=dWxh[i])
        np.clip(dWhh[i], -5, 5, out=dWhh[i])
        np.clip(dbh[i], -5, 5, out=dbh[i])

        # perform parameter update with Adagrad
        self.mWxh[i] += dWxh[i] * dWxh[i]
        self.Wxh[i] -= lr * dWxh[i] / np.sqrt(self.mWxh[i] + 1e-8)
        self.mWhh[i] += dWhh[i] * dWhh[i]
        self.Whh[i] -= lr * dWhh[i] / np.sqrt(self.mWhh[i] + 1e-8)
        self.mbh[i] += dbh[i] * dbh[i]
        self.bh[i] -= lr * dbh[i] / np.sqrt(self.mbh[i] + 1e-8)

    # clip gradients for Why and by
    np.clip(dWhy, -5, 5, out=dWhy)
    np.clip(dby, -5, 5, out=dby)

    # perform parameter update with Adagrad
    self.mWhy += dWhy * dWhy
    self.Why -= lr * dWhy / np.sqrt(self.mWhy + 1e-8)
    self.mby += dby * dby
    self.by -= lr * dby / np.sqrt(self.mby + 1e-8)
```

这段代码使用 Adagrad 优化算法更新 RNN 的参数。它跟踪参数的梯度平方和（`mWxh`、`mWhh`、`mbh`、`mWhy` 和 `mby`），并用这个和的平方根加上一个小常数 `1e-8` 来除以学习率，以确保数值稳定，从而有效地调整每个参数的学习率。此外，它剪切梯度以防止 [梯度爆炸](https://machinelearningmastery.com/exploding-gradients-in-neural-networks/)。

Adagrad 为每个参数调整学习率，对不频繁更新的参数执行较大的更新，对频繁更新的参数执行较小的更新。这意味着，对于不频繁更新的参数，学习率会较大，以便模型能对这些参数做出更大的调整。另一方面，对于频繁更新的参数，学习率会较小，从而使模型对这些参数进行小幅调整，以防过拟合。这与使用固定学习率形成对比，后者可能会导致参数校正不足或过度校正。

让我们进行参数更新。

```py
# Initialize RNN
num_layers = 3
hidden_size = 100
seq_length = 8

rnn = RNN(hidden_size=hidden_size, vocab_size=vocab_size, seq_length=seq_length, num_layers=num_layers)

x = [char_to_idx[ch] for ch in data[rnn.pointer:rnn.pointer+seq_length]]

y = [char_to_idx[ch] for ch in data[rnn.pointer+1:rnn.pointer+seq_length+1]]

# initialize hidden state with zeros
hprev = [np.zeros((hidden_size, 1)) for _ in range(num_layers)] 

## Call RNN
loss, hprev, cache = rnn(inputs=x, targets=y, hprev=hprev)
grads = rnn.backward(targets=y, cache=cache)
rnn.update(grads=grads, lr=1e-1)
```

## 训练

最后一步实际上是训练网络，将输入序列输入网络中，计算错误，优化器更新权重和偏差。

```py
def train(rnn, epochs, data, lr=1e-1, use_drop=False):

    for _ in range(epochs):

        # prepare inputs (we're sweeping from left to right in steps seq_length long)
        if rnn.pointer+seq_length+1 >= len(data) or rnn.iteration == 0:

            hprev = [np.zeros((hidden_size, 1)) for _ in range(rnn.num_layers)]  # reset RNN memory

            rnn.pointer = 0 # go from start of data

        x = [char_to_idx[ch] for ch in data[rnn.pointer:rnn.pointer+seq_length]]
        y = [char_to_idx[ch] for ch in data[rnn.pointer+1:rnn.pointer+seq_length+1]]

        if use_drop:
            loss, hprev, cache = rnn(inputs=x, targets=y, hprev=hprev, dropout=True)
        else:
            loss, hprev, cache = rnn(inputs=x, targets=y, hprev=hprev)

        grads = rnn.backward(targets=y, cache=cache)
        rnn.update(grads=grads, lr=lr)

        # update loss
        rnn.loss = rnn.loss * 0.999 + loss * 0.001

        ## show progress now and then
        if rnn.iteration % 1000 == 0: 
            print('iter {}, loss: {}'.format(rnn.iteration, rnn.loss))

            sample_ix = rnn.predict(hprev, x[0], 200)
            txt = ''.join(idx_to_char[ix] for ix in sample_ix)
            print('Sample')
            print ('----\n {} \n----'.format(txt))

        rnn.pointer += seq_length # move data pointer
        rnn.iteration += 1 # iteration counter

## hyper-params
num_layers = 2
hidden_size = 128
seq_length = 13

# Initialize RNN
rnn = RNN(hidden_size=hidden_size, 
          vocab_size=vocab_size, 
          seq_length=seq_length, 
          num_layers=num_layers)

train(rnn=rnn, epochs=15000, data=data)
```

这段代码非常简单。我们执行前向和反向传播，并在每个纪元更新模型参数。

我想指出的是——

> 损失通过当前损失和前一个损失的加权平均来更新。

当前损失乘以 0.001 并加到前一个损失上，前一个损失乘以 0.999。这意味着当前损失对总损失的影响较小，而前面的损失影响较大。这样，总损失波动不会那么大，并且会随着时间更加稳定。

通过使用 EMA（指数移动平均），更容易监控网络的性能，并检测它是否过拟合或欠拟合。

![](../Images/91ade5e2003cec0536e4f7e6880e1946.png)

第零次迭代的损失与文本预测。图片由作者提供。

![](../Images/6d180c19cec0dddaac9eecb78088be9e.png)

第 14,000 次迭代的损失与文本预测。图片由作者提供。

![](../Images/981f59809bea423f9fc013d586b3ee2d.png)

50,000 个纪元后的损失。

我们的 RNN 训练过程已经成功，我们可以看到损失的减少和生成样本质量的提高。然而，需要注意的是，生成原创莎士比亚文本是一个复杂的任务，而这一实现是一个简单的普通 RNN。因此，仍有进一步改进和尝试不同架构和技术的空间。

# 结论

总之，本文展示了如何使用 Numpy 实现和训练一个字符级 RNN。多对多架构和在线学习方法使得网络能够适应数据中的新模式，从而改进样本生成。虽然这个网络在生成原创莎士比亚文本方面具有一定能力，但需要注意的是，这只是一个简化版本，还有许多其他架构和技术可以探索，以获得更好的性能。

完整代码和仓库 [在这里](https://github.com/j0sephsasson/numpy-RNN)。

随时与我们联系并提出问题，或对代码进行改进。

**感谢阅读！**
