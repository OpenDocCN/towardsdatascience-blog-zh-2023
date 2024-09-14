# PyTorch 中的多 GPU 训练及其替代方案：梯度累积

> 原文：[`towardsdatascience.com/multiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91`](https://towardsdatascience.com/multiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91)

## 代码与理论

[](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)![Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 24 日

--

![](img/ed6fca5026469bec09b620c9620bc331.png)

[`unsplash.com/photos/vBzJ0UFOA70`](https://unsplash.com/photos/vBzJ0UFOA70)

在这篇文章中，我们首先将探讨数据并行（DP）和分布式数据并行（DDP）算法的区别，然后解释什么是梯度累积（GA），最后展示 DDP 和 GA 如何在 PyTorch 中实现，并且它们如何导致相同的结果。

## 引言

在训练深度神经网络（DNN）时，一个重要的超参数是批量大小。通常，批量大小不应过大，因为网络会倾向于过拟合，也不应过小，因为这会导致收敛速度缓慢。

当处理高分辨率图像或其他占用大量内存的数据时，假设如今大多数大 DNN 模型的训练都是在 GPU 上进行的，根据可用 GPU 的内存，适配小批量大小可能会有问题。因为，如前所述，小批量大小会导致收敛缓慢，我们可以使用三种主要方法来增加有效批量大小：

1.  使用多个小型 GPU 并行运行模型于小批量 — DP 或 DDP 算法

1.  使用更大的 GPU（昂贵）

1.  在多次步骤中累积梯度

现在让我们详细探讨 1\. 和 3\. — 如果你幸运地拥有一张可以容纳所有数据的大 GPU，你可以阅读 DDP 部分，了解它是如何在 PyTorch 中实现的，跳过其他部分。

假设我们想要有效的批量大小为 30，但每个 GPU 上只能容纳 10 个数据点（小批量大小）。我们有两个选择：数据并行或分布式数据并行：

## 数据并行（DP）

首先，我们定义主 GPU。然后，执行以下步骤：

1.  将 10 个数据点（小批量）和模型的副本从 Master GPU 移动到其他 2 个 GPU。

1.  在每个 GPU 上进行前向传播，并将输出传递给 Master GPU。

1.  在 Master GPU 上计算总损失，然后将损失返回到每个 GPU，以计算参数的梯度。

1.  将梯度（这些是所有训练样本的梯度平均值）返回到 Master GPU，将它们求和以获得整个 30 个数据点批次的平均梯度。

1.  更新 Master GPU 上的参数，并将这些更新发送到其他两个 GPU 以进行下一次迭代。

这个过程存在一些问题和低效之处：

+   数据从 Master GPU 传递，然后在其他 GPU 之间分割。此外，Master GPU 的利用率高于其他 GPU，因为总损失的计算和参数更新都发生在 Master GPU 上。

+   我们需要在每次迭代时同步其他 GPU 上的模型，这可能会减慢训练速度。

## **分布式数据并行（DDP）**

分布式数据并行（Distributed Data Parallel）旨在改善数据并行（Data Parallel）算法的低效问题。我们仍然使用之前的设置——每批次 30 个数据点，3 个 GPU。区别如下：

1.  它没有 Master GPU。

1.  由于我们不再拥有 Master GPU，我们直接从磁盘/RAM 上以 *非重叠* 的方式并行加载每个 GPU 上的数据——*DistributedSampler* 负责这项工作。在底层，它使用本地排名（GPU id）将数据分配到各个 GPU 上——给定 30 个数据点，第一个 GPU 将使用点 [0, 3, 6, … , 27]，第二个 GPU [1, 4, 7, .., 28]，第三个 GPU [2, 5, 8, .. , 29]。

```py
n_gpu = 3
for i in range(n_gpu):
  print(np.arange(30)[i:30:n_gpu])
```

前向传播、损失计算和反向传播在每个 GPU 上独立执行，梯度异步减少，计算均值，然后所有 GPU 上的更新进行。

由于 DDP 相对于 DP 的优势，现如今更倾向于使用 DDP，因此我们仅展示 DDP 实现。

## 梯度累积

如果我们只有一个 GPU，但仍想使用更大的批次大小，可以选择在一定步数内累积梯度，有效地累积一定数量的小批次的梯度，从而增加有效批次大小。以上述示例为例，我们可以累积 10 个数据点的梯度进行 3 次迭代，以实现与 DDP 训练中有效批次大小为 30 相同的结果。

## **DDP 过程** 代码

下面我将仅讲述实现 DDP 与单 GPU 代码相比的区别。完整代码可以在下几节中找到。首先，我们初始化进程组，以允许不同进程之间进行通信。通过 *int(os.environ[“LOCAL_RANK”])*，我们可以检索给定进程中使用的 GPU。

```py
init_process_group(backend="nccl")
device = int(os.environ["LOCAL_RANK"])
torch.cuda.set_device(device)
```

然后，我们需要将模型包装在 *DistributedDataParallel* 中，以启用多 GPU 训练。

```py
model = NeuralNetwork(args.data_size) 
model = model.to(device) 

if args.distributed:
  model = torch.nn.parallel.DistributedDataParallel(model, device_ids=[device])
```

最后一部分是定义 *DistributedSampler*，我在 DDP 部分提到过。

```py
sampler = torch.utils.data.DistributedSampler(dataset)
```

其余的训练保持不变——我将在文章的末尾包括完整代码。

## **梯度累积代码**

当发生反向传播时，在调用*loss.backward()*后，梯度会存储在各自的 Tensor 中。实际更新发生在调用*optimizer.step()*时，然后存储在 Tensor 中的梯度会被*optimizer.zero_grad()*设置为零，以便进行下一次反向传播和参数更新。因此，为了累积梯度，我们调用*loss.backward()*进行所需的梯度累积次数，而不将梯度设置为零，以便它们在多个迭代中累积，然后**我们对累积梯度迭代的平均梯度进行平均**（*loss = loss/ACC_STEPS*）。之后，我们调用*optimizer.step()*并将梯度置零，以开始下一次梯度累积。在代码中：

```py
ACC_STEPS = dist.get_world_size() # == number of GPUs
# iterate through the data
for i, (idxs, row) in enumerate(loader):
  loss = model(row)  
  # scale loss according to accumulation steps
  loss = loss/ACC_STEPS
  loss.backward()
  # keep accumualting gradients for ACC_STEPS
  if ((i + 1) % ACC_STEPS == 0):
    optimizer.step()  
    optimizer.zero_grad()
```

## 完整代码

```py
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "0,1"
print(os.environ["CUDA_VISIBLE_DEVICES"])

import torch
import torch.nn as nn
from torch.utils.data import DataLoader, Dataset, Sampler
import argparse
import torch.optim as optim 
import numpy as np
import random
import torch.backends.cudnn as cudnn
import torch.nn.functional as F

from torch.distributed import init_process_group
import torch.distributed as dist

class data_set(Dataset):

    def __init__(self, df):
        self.df = df

    def __len__(self):
        return len(self.df)

    def __getitem__(self, index):    

        sample = self.df[index]
        return index, sample

class NeuralNetwork(nn.Module):
    def __init__(self, dsize):
        super().__init__()
        self.linear =  nn.Linear(dsize, 1, bias=False)
        self.linear.weight.data[:] = 1.

    def forward(self, x):
        x = self.linear(x)
        loss = x.sum()
        return loss

class DummySampler(Sampler):
    def __init__(self, data, batch_size, n_gpus=2):
        self.num_samples = len(data)
        self.b_size = batch_size
        self.n_gpus = n_gpus

    def __iter__(self):
        ids = []
        for i in range(0, self.num_samples, self.b_size * self.n_gpus):
            ids.append(np.arange(self.num_samples)[i: i + self.b_size*self.n_gpus :self.n_gpus])
            ids.append(np.arange(self.num_samples)[i+1: (i+1) + self.b_size*self.n_gpus :self.n_gpus])
        return iter(np.concatenate(ids))

    def __len__(self):
        # print ('\tcalling Sampler:__len__')
        return self.num_samples

def main(args=None):

    d_size = args.data_size

    if args.distributed:
        init_process_group(backend="nccl")
        device = int(os.environ["LOCAL_RANK"])
        torch.cuda.set_device(device)
    else:
        device = "cuda:0"

    # fix the seed for reproducibility
    seed = args.seed

    torch.manual_seed(seed)
    np.random.seed(seed)
    random.seed(seed)
    cudnn.benchmark = True

    # generate data
    data = torch.rand(d_size, d_size)

    model = NeuralNetwork(args.data_size)    
    model = model.to(device)  

    if args.distributed:
        model = torch.nn.parallel.DistributedDataParallel(model, device_ids=[device])

    optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)
    dataset = data_set(data)

    if args.distributed:
        sampler = torch.utils.data.DistributedSampler(dataset, shuffle=False)
    else:
        # we define `DummySampler` for exact reproducibility with `DistributedSampler`
        # which splits the data as described in the article. 
        sampler = DummySampler(dataset, args.batch_size)

    loader = DataLoader(
                dataset,
                batch_size=args.batch_size,
                num_workers=0,
                pin_memory=True,
                sampler=sampler,
                shuffle=False,
                collate_fn=None,
            )          

    if not args.distributed:
        grads = []

    # ACC_STEPS same as GPU as we need to divide the loss by this number
    # to obtain the same gradient as from multiple GPUs that are 
    # averaged together
    ACC_STEPS = args.acc_steps 
    optimizer.zero_grad()

    for epoch in range(args.epochs):

        if args.distributed:
            loader.sampler.set_epoch(epoch)

        for i, (idxs, row) in enumerate(loader):

            if args.distributed:
                optimizer.zero_grad()

            row = row.to(device, non_blocking=True) 

            if args.distributed:
                rank = dist.get_rank() == 0
            else:
                rank = True

            loss = model(row)  

            if args.distributed:
                # does average gradients automatically thanks to model wrapper into 
                # `DistributedDataParallel`
                loss.backward()
            else:
                # scale loss according to accumulation steps
                loss = loss/ACC_STEPS
                loss.backward()

            if i == 0 and rank:
                print(f"Epoch {epoch} {100 * '='}")

            if not args.distributed:
                if (i + 1) % ACC_STEPS == 0: # only step when we have done ACC_STEPS
                    # acumulate grads for entire epoch
                    optimizer.step()  
                    optimizer.zero_grad()
            else:
                optimizer.step() 

        if not args.distributed and args.verbose:
            print(100 * "=")
            print("Model weights : ", model.linear.weight)
            print(100 * "=")
        elif args.distributed and args.verbose and rank:
            print(100 * "=")
            print("Model weights : ", model.module.linear.weight)
            print(100 * "=")

if __name__ == "__main__":

    parser = argparse.ArgumentParser()
    parser.add_argument('--distributed', action='store_true',)
    parser.add_argument('--seed', default=0, type=int) 
    parser.add_argument('--epochs', default=2, type=int) 
    parser.add_argument('--batch_size', default=4, type=int) 
    parser.add_argument('--data_size', default=16, type=int) 
    parser.add_argument('--acc_steps', default=3, type=int) 
    parser.add_argument('--verbose', action='store_true',)

    args = parser.parse_args()

    print(args)

    main(args)
```

现在，如果我们运行这两个脚本：

+   *python3 ddp.py — epochs 2 — batch_size 4 — data_size 8 — verbose — acc_steps 2*

+   *torchrun — standalone — nproc_per_node=2 ddp.py — epochs 2 — distributed — batch_size 4 — data_size 8 — verbose*

我们会看到获得完全相同的最终模型参数：

```py
# From Gradient Accumulator
Model weights :  Parameter containing:
tensor([[0.9472, 0.9440, 0.9527, 0.9687, 0.9570, 0.9343, 0.9411, 0.9186]],
       device='cuda:0', requires_grad=True)

# From DDP:
Model weights :  Parameter containing:
tensor([[0.9472, 0.9440, 0.9527, 0.9687, 0.9570, 0.9343, 0.9411, 0.9186]],
       device='cuda:0', requires_grad=True)
```

## 结论

在这篇文章中，我们简要介绍了 DP、DDP 算法以及梯度累积的基本概念，并展示了如何在没有多个 GPU 的情况下增加有效批量大小。需要注意的是，即使我们获得了相同的最终结果，使用多个 GPU 的训练速度也远快于使用梯度累积，因此如果训练速度很重要，那么多个 GPU 是加速训练的唯一途径。
