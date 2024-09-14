# 如何将 SLURM 作业发送到集群

> 原文：[`towardsdatascience.com/how-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac`](https://towardsdatascience.com/how-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac)

## 一个关于如何将 SLURM 作业发送到集群的教程，特别适用于深度学习和数据科学

[](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)![François Porcher](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------) ·阅读时间 8 分钟·2023 年 8 月 21 日

--

![](img/f7cbe849c11f034a957dfc2a26cf82fa.png)

照片由[imgix](https://unsplash.com/@imgix?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所以你习惯于使用 Google Colab 的免费 GPU 来训练深度学习模型，但你准备好提升到集群的强大能力，并且不知道如何做到这一点？你来对地方了！🚀

在剑桥大学的神经科学研究实习期间，我训练了用于计算机视觉任务的大型模型，而谷歌提供的免费 GPU 不足够用，因此我决定使用本地集群。

然而，文档非常少，我不得不向其他人请求脚本以尝试理解它们，并或多或少地整理了对我有用的内容。现在我已经整理了运行基本 Python 脚本所需的一切。这个指南就是我希望当时能拥有的。

# 一个典型的机器学习使用案例

比如说，你想训练一个鸟类分类器，包含 500 个不同的类别和高分辨率的图片。这是 Google Colab 永远无法运行的。

首先，你需要确保你的深度学习模型训练脚本已经准备好。这个脚本应该包含加载数据集、定义神经网络架构和设置训练循环的必要代码。

你应该能够从终端运行这个脚本。

例如，假设你有一个叫做`train_bird_classifier.py`的脚本，你应该能够使用以下命令运行它：

```py
python train_bird_classifier.py
```

这个脚本可能看起来像这样：

```py
# train_bird_classifier.py

import torch
from torch.utils.data import DataLoader

# Assuming necessary functions, models, and transformations are defined in various files.
from utils import build_model, BirdDataset, collate_fn, train_model
from transformations import train_transforms, test_transforms

def main():
    device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

    # Dataset and DataLoader setup
    train_dataset = BirdDataset('data/train/', transform=train_transforms)
    train_loader = DataLoader(train_dataset, batch_size=32, shuffle=True, collate_fn=collate_fn)

    test_dataset = BirdDataset('data/test/', transform=test_transforms)
    test_loader = DataLoader(test_dataset, batch_size=32, shuffle=False, collate_fn=collate_fn)

    # Model setup
    model = build_model().to(device)

    # Training
    model = train_model(model, train_loader, device)

    # Save the trained model
    torch.save(model.state_dict(), 'bird_classifier_model.pth')

if __name__ == '__main__':
    main()
```

而不是在你的本地计算机或 Google Colab 上运行这个脚本，我们将在一个强大的集群上运行。这样你可以训练更大的模型，速度更快！

# 什么是 SLURM？

SLURM 代表简单 Linux 资源管理实用工具。

它是一个开源的 **作业调度器**，用于超级计算机和计算集群中分配计算资源给作业。它是一个工作负载管理器，负责管理、调度和监督集群中作业的执行。

![](img/9f34f819cbc749f9c6b74e9f3bcec949.png)

照片由 [Michał Parzuchowski](https://unsplash.com/@mparzuchowski?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想象一个繁忙的火车站，有多个轨道和进出的火车。每列火车（代表一个作业）需要特定的资源：一个站台、一个出发时间、一定数量的车厢和一条路线。SLURM 就像是车站的总控制器，确保每列火车获得所需的资源而不会发生碰撞、延迟或浪费宝贵的轨道空间。

它有效地管理和调度列车，确保它们以最优方式出发和到达，**最大化车站容量的使用**。在超级计算机和集群的世界中，SLURM 执行类似的协调，确保每个计算作业顺利而高效地执行。

# SLURM 是如何工作的？

1.  **提交**：用户编写指定作业需求和要执行任务的脚本。这些脚本通过 `sbatch` 命令提交。

1.  **排队**：提交的作业被放入队列中。SLURM 使用其调度算法来优先处理这些作业。

1.  **资源分配**：SLURM 检查集群中可用的资源（如节点、CPU、内存），并根据作业的要求和在队列中的优先级将其分配给作业。

1.  **执行**：一旦资源分配完成，作业开始运行。在执行期间，分配给作业的资源不能被其他作业使用。

1.  **完成**：作业完成后，资源被释放并返回到资源池中，准备分配给其他作业。

现在你知道了 SLURM 的定义，以下是我们将要进行的步骤概述：

0\. 准备你想在集群上执行的脚本

1.  编写调用你脚本的 bash 文件

1.  将你的作业提交到队列中

1.  （可选）：监控你的作业

# 1\. 编写你的 bash 脚本

你的脚本需要是一个 bash 文件，所以文件名末尾需要有 `.sh` 扩展名。例如 `train_classifier.sh`。

你的脚本应始终以 shebang (`#!`) 开头，后跟解释器的路径，通常为 `/bin/bash` 用于 SLURM 脚本：

```py
#!/bin/bash
```

这一行指示系统使用 Bash shell 执行脚本。

# 添加 SLURM 指令

+   `#SBATCH`：在 SLURM 脚本中，`#SBATCH` 是 SLURM 特定指令的前缀，这些指令提供给 SLURM 调度器指令。这些指令不会像常规 shell 命令那样执行，但 SLURM 调度器会读取和解释这些指令，以了解如何处理作业。

以下是编写脚本时最有用的指令的详细介绍：

+   **作业名称：**（这将在作业队列中可见）

```py
#SBATCH -J <your_job_name>
```

用我们的例子来说，它可能是：

```py
#SBATCH -J train_bird_classifier
```

+   **账户：** 如果你有 GPU 积分，它们将从你指定的账户中使用。

+   `-A` 指定作业关联的账户（或项目）。这对于作业记账非常有用。

```py
#SBATCH -A <account_name>
```

+   **重排设置：** 该指令防止作业在失败后被重新排队。

```py
#SBATCH --no-requeue
```

+   **分区：**

+   `-p` 定义作业应该安排的分区（或队列）。一个集群可以有不同的分区。

```py
#SBATCH -p <partition_name>
```

+   **节点和任务：**

```py
#SBATCH --nodes=1 
#SBATCH --ntasks=1
```

这些指令请求一个节点（`--nodes=1`）和一个任务或 CPU 核心（`--ntasks=1`）用于作业。

+   **GPU：**

+   `--gres` 指定作业所需的通用资源。在这里，它请求一个 GPU。

```py
#SBATCH --gres=gpu:1
```

+   **电子邮件通知：** 这定义了 SLURM 将发送与作业相关通知的电子邮件地址。

```py
#SBATCH --mail-user=<your_email>
```

+   **作业持续时间：**

+   `--time` 设置了作业的总运行时间限制。如果作业在 12 小时内未完成，SLURM 将终止它。

```py
#SBATCH --time=12:00:00
```

## 加载环境

当你提交作业时，你可能需要加载一个虚拟环境。例如，要加载一个 conda 环境，你可以添加：

```py
module load miniconda/3
conda init bash
conda activate <env_name>
```

## 运行脚本

快好了！现在你只需指定从哪个路径运行脚本，并运行脚本。我们用以下命令实现：

```py
# Enter the path where your script is
cd <path_where_your_script_is>

# Run the script
python <name_of_the_script_you_want_to_run>
```

## 到目前为止，我们的`.sh`脚本总结

```py
#!/bin/bash
# Name of the job:
#SBATCH -J <job_name>
#SBATCH -A <account_name>
#SBATCH --no-requeue 
#SBATCH -p <partition_name>
#SBATCH --nodes=1
#SBATCH --ntasks=1
##SBATCH --exclusive
#SBATCH --gres=gpu:1
#SBATCH --mail-user=<your_email_adress>
#SBATCH --mail-type=END
#SBATCH --time=04:00:00

#source ~/.bashrc

echo '------------------'
date
echo '------------------'

module load miniconda/3
conda init bash

conda activate <env_name>

cd <path_where_the_script_to_execute_is>

# Use the FOLD variable here
python <name_of_script_to_execute>

echo '------------------'
date
echo '------------------' 
```

以我们的鸟类分类器为例，它可能会是这样的：

```py
#!/bin/bash
# Name of the job:
#SBATCH -J bird_classifier_job
#SBATCH -A <account_name>
#SBATCH --no-requeue 
#SBATCH -p <partition_name>
#SBATCH --nodes=1
#SBATCH --ntasks=1
##SBATCH --exclusive
#SBATCH --gres=gpu:1
#SBATCH --mail-user=<your_email_adress>
#SBATCH --mail-type=END
#SBATCH --time=04:00:00

#source ~/.bashrc

echo '------------------'
date
echo '------------------'

module load miniconda/3
conda init bash

conda activate bird_classifier_env

cd path_to_train_bird_classifier_directory

# Use the FOLD variable here
python train_bird_classifier.py

echo '------------------'
date
echo '------------------'
```

# 2\. 提交你的作业

当你完成脚本编写后，你已经准备好将作业提交到队列中。

使用`sbatch`（代表提交批处理）命令提交脚本：

```py
$ sbatch train_classifier.sh
```

就这些！你的作业已经提交到队列中，如果没有错误，它将被执行！

# 3\. 监控你的作业

一旦你提交了作业，它不会立即执行，而是首先被添加到队列中。你可以监控你提交的作业的几件事情（它在队列中的位置、发送了多少作业、它已经执行了多长时间）。

你可以使用`squeue`功能来监控此情况：

```py
$ squeue
```

你还可以使用`scontrol show job`请求特定作业的详细信息。

```py
$ scontrol show job [JOB_ID]
```

如果你提交了多个具有相同名称的作业（如果你提交了多次稍有变化的相同作业，这可能会发生），你可以使用`grep`。例如，如果你想在`squeue`输出中搜索所有名称为“my_job”的作业：

```py
$ squeue | grep "my_job"
```

# 4\. 取消作业

如果需要取消作业，请在 Shell 中使用`scancel`命令，然后跟上作业 ID（可以使用`squeue`获取）：

```py
$ scancel [JOB_ID]
```

> 请注意，如果你提交了作业，然后在作业开始执行前修改了源代码，它将在执行时使用最新版本的脚本，而不是提交作业时的版本。

# 结论

将 SLURM 作业提交到集群可能一开始会让人觉得令人望而却步，但一旦你掌握了它，它将成为 HPC 体验的重要组成部分。

始终记住尊重集群的指南和资源限制，保持耐心，并继续学习以优化你的工作流程。

希望这份指南对你有所帮助！

感谢阅读！在你离开之前：

+   确保你已经查看了[Google colab 上的笔记本](https://drive.google.com/file/d/1JeZuu11A2ZdkyXD2vKyzJa3weCMOlskn/view?usp=share_link)。

+   查看我在 GitHub 上的[AI 教程汇编](https://github.com/FrancoisPorcher/awesome-ai-tutorials)。

[](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----dd1cf021c7ac--------------------------------) [## GitHub - FrancoisPorcher/awesome-ai-tutorials: 最佳 AI 教程汇编，助你成为数据科学大师…

### 最佳 AI 教程汇编，助你成为数据科学大师！ - GitHub …

[GitHub](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----dd1cf021c7ac--------------------------------)

*你应该将我的文章订阅到你的收件箱里。* [***在这里订阅。***](https://medium.com/@francoisporcher/subscribe)

*如果你想访问 Medium 上的高级文章，你只需每月$5 的会员费。如果你通过* [***我的链接***](https://medium.com/@francoisporcher/membership)*注册，你将以没有额外费用的方式支持我。*

> 如果你觉得这篇文章有启发性且对你有帮助，请考虑关注我以获取更多深入内容！你的支持帮助我继续制作有助于我们集体理解的内容。

## 参考文献：

1.  Yoo, A. B., Jette, M. A., & Grondona, M. (2003). SLURM: 简单的 Linux 资源管理工具。*并行处理作业调度策略*（第 44–60 页）。Springer，柏林，海德堡。 [论文链接（如在线提供）](https://chat.openai.com/URL_HERE)

1.  SLURM 工作负载管理器 — 官方文档。 [SLURM 文档](https://slurm.schedmd.com/documentation.html)

1.  Oak Ridge National Laboratory. (年份). *SLURM 培训材料*。从[培训材料链接](https://chat.openai.com/URL_HERE)检索。
