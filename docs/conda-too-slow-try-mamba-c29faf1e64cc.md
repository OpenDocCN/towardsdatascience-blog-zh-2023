# Conda 太慢了？试试 Mamba！

> 原文：[`towardsdatascience.com/conda-too-slow-try-mamba-c29faf1e64cc`](https://towardsdatascience.com/conda-too-slow-try-mamba-c29faf1e64cc)

## 流行的包管理器比较

[](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------) ·5 分钟阅读·2023 年 11 月 22 日

--

![](img/740951dc23be607fefeb3ee291fcc7a4.png)

复古包交付。照片由[Charlie M](https://unsplash.com/@chollz?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

迟早，每个数据科学家和机器学习工程师都会遇到包管理器和环境。环境包含运行项目代码所需的库。开发人员可以在同一台机器上创建多个环境，使得为不同项目维护不同环境成为可能。软件不会在系统范围内安装，而是包含在环境内。

包管理器用于分发软件库。流行的包管理器包括 conda、pip 和 mamba。

> 值得尝试 mamba，因为我通过 mamba 安装大型环境的速度是 conda 的 10 倍！

在这篇文章中，我将展示如何获得这种加速。我将讨论：

+   如何设置环境

+   conda 和 mamba 包及环境管理器

+   它们在速度方面的比较

+   libmamba：在 conda 中加速 mamba？

## 软件环境

维护软件环境文件可以确保代码保持可复现，并能在不同平台上执行。机器学习项目应始终包括所需包的列表以及它们的版本号。如果你将模型提供给另一位开发者或客户，他们可以在本地复现环境。

一个示例环境文件如下，取自我的一个 git 仓库：[`github.com/crlna16/ai4foodsecurity`](https://github.com/crlna16/ai4foodsecurity/blob/main/docker/kernel-env-cuda11.yaml)：

```py
name: ai4foodsecurity
channels:
  - conda-forge
  - defaults
  - pytorch
  - nvidia
dependencies:
  - pandas==1.0.1
  - geopandas==0.8.2
  - rasterio==1.1.8
  - matplotlib==3.3.2
  - tensorboard==2.4.0
  - sentinelhub==3.3.2
  - pytorch==1.9.0
  - torchvision==0.10.0
  - numpy==1.19.5
  - sh==1.14.2
  - radiant-mlhub==0.3.0
  - ipykernel=5.3.4

  - 'cudatoolkit=11.1'
```

包管理系统可以用来从类似的文件创建环境。

## 包管理系统

创建环境和在其中安装包有多种不同的方法。我们将专注于

+   Conda，

+   Mamba

虽然 [pip](https://pip.pypa.io/) 也是维护 Python 环境的流行选择，但使用 conda 或 mamba 的优势在于它们会检查依赖关系。例如，Tensorflow 2.13.0 需要 Python 3.8–3.11 作为依赖。如果用户尝试安装不兼容的 Python 版本，conda 会警告用户关于不一致的问题并拒绝安装软件包。而 pip 则不会发出警告，但代码可能无法运行。

有些软件包通过 pip 提供的版本比通过 conda 提供的版本更新。在这种情况下，可以明确将 pip 软件包包含在环境定义中。

调试环境中的错误和不一致可能非常耗时。错误往往不明显，而且在事后很难确定所需软件包的正确版本。因此，强烈建议您维护包含软件包及其版本号的环境描述文件。

## Conda

Conda [[`docs.conda.io/en/latest/`](https://docs.conda.io/en/latest/)] 是一个跨平台的包管理器，可以在 Windows、MacOS 和 Linux 上运行。它既是一个包管理器，托管中央服务器上的软件包以备下载，也是一个环境管理器。虽然 conda 最常用于 Python 开发，但它也支持其他编程语言。

conda 软件包的主要分发渠道是 [`repo.anaconda.com/`](https://repo.anaconda.com/)，该网站包含超过 7,500 个经过验证的软件包。此外，面向社区的 conda-forge [[`anaconda.org/conda-forge`](https://anaconda.org/conda-forge)] 还包含超过 22,500 个附加软件包。

例如，要创建一个 conda 环境并安装 numpy，请运行

```py
conda create -n mycondaenv
conda activate mycondaenv
conda install numpy
```

尽管 conda 通常很好，但随着时间的推移，它往往会变得缓慢。特别是当您有一个大型环境时，安装额外软件包时解决环境问题可能需要很长时间。这对于开发者来说是令人沮丧的，因为他们不得不等待半小时才能让环境解决问题，而不是继续进行软件开发和机器学习实验。

![](img/af85733d481db74d6c0b66f28e850669.png)

无聊的开发者等待 conda 环境解决。照片由 [Juan Gomez](https://unsplash.com/@nosoylasonia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

## Mamba

Mamba [[`mamba.readthedocs.io/en/latest/index.html`](https://mamba.readthedocs.io/en/latest/index.html)] 是一个与 conda 兼容的包管理器，支持大多数 conda 的软件包。mamba 命令可以作为所有命令中 conda 的替代品。通过 mamba 也可以获取 conda-forge 的软件包。

要创建一个环境并安装软件包，请使用

```py
mamba create -n mymambaenv
mamba activate mymambaenv
mamba install numpy
```

Mamba 本身可以通过 conda 安装

```py
conda install -c conda-forge mamba
```

![](img/908e7a8108875dd14b17323918fb2e30.png)

由[Cara Fuller](https://unsplash.com/@caraventurera?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 安装速度

在相同的 Linux 系统上比较以下命令时，我发现 conda 和 mamba 包管理器的执行时间不同。

```py
time conda create -y -n mycondaenv numpy

>> real 0m37,992s
```

```py
time mamba create -y -n mymambaenv numpy

>> real 0m27,722s
```

这些命令是简写形式，用于在一行 shell 指令中创建环境并安装 numpy 包。* -y *标志用于自动安装包，而无需用户确认。

> 通过 mamba 安装 numpy 比通过 conda 安装快了 25%！

让我们尝试创建一个大型环境，将上面的环境定义文件保存为*env.yml*并直接从那里安装。

```py
time conda env update --file env.yml

real 10m51.233s
user 10m4.853s
sys  0m12.286s
```

```py
time mamba env update --file env.yml

real 1m0.634s
user 0m45.550s
sys  0m4.051s
```

Mamba 在解决这个环境时惊人地**快了 10 倍**！

## 为什么 mamba 比 conda 更快？

每次我们在环境中安装新包时，环境管理器必须执行以下步骤：

+   收集包的元数据

+   解决环境（即检查已安装哪些包并检查一致性）

+   下载包

+   安装下载的包

Conda 通常花费大量时间的步骤是解决环境。在这里，包管理器之间的区别在于 mamba 利用了高效的 C++代码和并行处理，比 conda 更快。mamba 用于解决依赖关系和环境的*libsolv*模块也在其他地方使用，例如在 Unix 发行版中。此外，mamba 可以执行并行下载，而不是 conda 的顺序下载。

## Libmamba: 在 conda 中加速 mamba

[libmamba](https://www.anaconda.com/blog/a-faster-conda-for-a-growing-community)求解器结合了 mamba 的速度和 conda 的知名品牌。通过以下指令可以激活它：

```py
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

我们再次测量从之前的复杂环境安装所需的时间：

```py
time conda env update --file env.yml

real 4m26.127s
user 3m43.397s
sys 0m11.437s
```

在这种情况下，libmamba 在 conda 中使用比纯 conda 快了 50%，但这种加速仍然无法与 mamba 实现的速度相比。

## 总结

环境管理器对于在开发和部署过程中维持可重复的软件环境至关重要。Conda 是一个流行的包管理器，但在处理大型环境时可能会变得非常缓慢。Mamba 作为 conda 的直接替代品，通过使用高效的代码和并行处理，安装新包的速度比使用 conda 要快得多。libmamba 求解器声称在 conda 中可以达到类似 mamba 的速度。

我们的测试显示，mamba 在从头创建一个大型环境时比 conda**快了 10 倍**。libmamba 比纯 conda**快了 2 倍**。因此，下次你发现自己在等待 conda 环境解析时花了很长时间，不妨考虑使用 mamba 来替代，避免度过一个无聊的下午。

## 进一步阅读

+   [`mamba.readthedocs.io/en/latest/index.html`](https://mamba.readthedocs.io/en/latest/index.html)

+   [`docs.conda.io/`](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)

+   [`www.anaconda.com/blog/a-faster-conda-for-a-growing-community`](https://www.anaconda.com/blog/a-faster-conda-for-a-growing-community)
