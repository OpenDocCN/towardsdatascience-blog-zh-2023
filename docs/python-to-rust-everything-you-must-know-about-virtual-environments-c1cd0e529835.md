# 从 Python 到 Rust：你必须了解的虚拟环境的一切

> 原文：[`towardsdatascience.com/python-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835`](https://towardsdatascience.com/python-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835)

## 从 Python 专家到 Rust 新手——一位数据科学家的过渡故事

[](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)![Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------) [Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------) ·7 分钟阅读·2023 年 12 月 26 日

--

![](img/114983101b49df51f54b21054860ffc1.png)

图 1：货物仓库里的蛇和螃蟹。 ([螃蟹](https://unsplash.com/photos/a-group-of-red-mushrooms-ZWrsjySNfxY); [蛇](https://unsplash.com/photos/close-up-photo-of-brown-and-gray-snake-VUr5nmC1IM4); [集装箱](https://unsplash.com/photos/assorted-color-filed-intermodal-containers-tjX_sniNzgQ); 由作者编排)

从 Python 转到 Rust 的旅程就像把一个可靠的光剑换成一种新的刀刃——既令人兴奋又略显令人生畏。作为一个对 Python 的特性非常熟悉的数据科学家，进入 Rust 的世界是一个令人激动的新挑战。在本文中，我将分享我的经历和见解，比较这两种强大语言如何处理软件开发的一个关键方面——特别是关注（虚拟）环境和依赖管理。

在使用 Python 时，你首先学到的事情之一就是在所谓的*虚拟环境*中工作。这是一个管理依赖关系和隔离项目特定包的关键工具，以避免它们干扰其他项目或系统范围的 Python 安装。我几年前写了一篇关于如何管理 Python 的文章，但它仍然适用（它稍微变化了一些，涉及到[*micromamba*](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html)和[*poetry*](https://python-poetry.org/) ，如果需要，我可以写一篇关于这方面的文章）。

> TLDR: 只需使用*cargo*，大多数情况下你就会没问题——Dennis

在使用[*rustup*](https://rustup.rs/)安装 Rust 之后，我的第一个问题是：*我应该如何创建一个虚拟环境？* 对我来说，这是一个非常有意义的问题，因为 Rust 也可以使用许多包（称为 Crates）作为依赖项。事实上，*cargo* 非常优雅地解决了这个问题。以下是我在比较虚拟环境和 pip 与 Rust 的 *cargo* 构建系统时的发现。

*免责声明：在我探索这些 Rust 领域时，我对语言的熟练程度可能仍有些生疏（玩笑话）。加入我，在这个学习冒险中揭开 Cargo 的细微差别，告别虚拟环境的熟悉拥抱。*

# 1\. 包的单一全局位置

Python 中的虚拟环境是使用像[*venv*](https://docs.python.org/3/library/venv.html)、[*virtualenv*](https://virtualenv.pypa.io/en/latest/)或[*conda*](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html)这样的工具按项目创建的。在底层，这些系统创建一个单独的文件夹，该文件夹包含 Python 发行版及其所有包。现在，当我们使用 *pip* 或 *conda* 安装一个包时，该包及其所有依赖项会被安装在这个隔离的文件夹中。这些虚拟环境工具所做的事情类似于“[*chroot*](https://en.wikipedia.org/wiki/Chroot)”，但针对 Python 安装。

> 解释型语言如 Python，依赖关系解析通常发生在运行时。

对于像 Python 这样的解释型语言，依赖关系解析通常发生在运行时。这意味着当 Python 脚本执行时，解释器需要动态解析和加载所需的依赖项。虚拟环境帮助管理这些依赖关系，为项目提供了一个干净的隔离，以避免冲突。以下是 Python 中的典型工作流程：

```py
# create a virtual environment with a specific Python version
conda create -n my_environment python=3.12
```

```py
# activate the virtual environment
conda activate my_environment# Install a package
pip install pandas
```

另一方面，Rust 有一个叫做[*cargo*](https://doc.rust-lang.org/cargo/)的包管理器，它使用一个全球唯一的位置，即没有用户特定的虚拟环境。它之所以能做到这一点，是因为 *cargo 构建系统*。当你使用 *cargo* 创建一个项目时，它围绕 `Cargo.toml` 文件展开。这是所谓的项目文件，定义了项目的详细信息，包括其依赖项及语义版本控制。使用 `cargo add <crate>` 你将依赖项添加到这个项目文件中，这些依赖项会在构建过程中下载。由于我们使用 *cargo* 来构建，并且 cargo 负责选择/下载正确的依赖项，因此不需要像 Python 虚拟环境中的 *chroot-like* 机制。

> 使用 *cargo*，一切都已经在虚拟环境中。

使用 *cargo* 创建新项目的工作流程看起来与 Python 非常相似，但在底层，它确实要聪明一些：

```py
# create a new project folder using cargo
cargo new my_project
```

```py
# go into the new project folder
cd my_project# Install a package
cargo add rand
```

在使用 cargo 构建期间，需要的正确版本的包从 `Cargo.toml` 读取并从全局注册表中加载（默认情况下在 `$HOME/.cargo`）。这主要是因为 Rust 是编译语言，而在运行时需要解析依赖的 Python 实现起来要困难得多。

# 2\. 内置的依赖解析

看看 Python，没有内置的依赖解析系统。是的，使用 `pip freeze` 你可以获得已安装包的概述，但没有保证它也能捕获所有间接依赖。这意味着它不能捕获环境的完整复杂性。

> Pip freeze 可能不足以捕获完整的环境

为了解决这个问题，其他语言如 Ruby 和 JavaScript 的依赖解析器开始使用所谓的 *锁文件*。这些锁文件捕获了所有依赖项及其依赖项的版本信息。Python 通过 [*Pipenv*](https://pipenv.pypa.io/en/latest/) 或我个人最喜欢的 [*Python Poetry*](https://python-poetry.org/) 获得类似的功能，但在下载 Python 时没有内置工具。

Rust 的 Cargo 通过使用锁文件具有内置的依赖解析功能。当你使用 `cargo build` 或 `cargo run` 时，它会检查 `Cargo.lock` 文件，以确保使用所有依赖项的确切版本。这个锁文件捕获了整个依赖树，包括传递依赖，形成了项目环境的全面且确定性的表示。

`Cargo.lock` 文件作为特定时间点的依赖快照。它包含了不仅是 `Cargo.toml` 文件中指定的直接依赖的准确版本信息，还有所有传递依赖的版本信息。

例如，如果项目 A 依赖于库 B 版本 1.0.0，而库 B 依赖于库 C 版本 2.1.0，则这两个版本都会记录在 `Cargo.lock` 文件中。这确保了所有参与项目的人员，无论其环境如何，都得到完全相同的依赖集合。Cargo 非常灵活，可以支持即使在同一编译目标中也能有多个版本的相同依赖。

![](img/a41ab7d2b40b768ae304bf51c76f0cbe.png)

图 2：在构建阶段，Cargo 收集所有必需的依赖。在运行阶段，依赖项已被链接到可执行文件中。（图由作者提供）。

使用 Cargo 的锁文件消除了开发者手动管理和同步不同环境中依赖版本的需要。它提供了一个一致且可重现的构建环境，使得协作和部署更加可靠。这是编译语言的一大优势，我们可以认为这是一种不公平的比较。

# 3\. 包和 Rust 自身的兼容性

在软件工程中，兼容性是确保项目在各种环境下顺利运行的基石。当我们比较 Rust 的 *cargo* 与 Python 的 *pip* 时，可以清楚地看到 Rust 在这方面经过了精心考虑，而 Python 则是随着时间的推移逐渐发展到现在的状态。

Rust 中的兼容性不仅仅是一个考虑因素，它是一种文化承诺。社区非常重视主要版本的应用程序编程接口（API）兼容性。这在 *cargo* 包管理器中得到了清晰体现，它强制执行 [语义化版本控制](https://semver.org/)。这使得开发环境可靠且可预测，其中依赖项预期能够良好配合。

与此相比，Python 生态系统中的兼容性有时可能是一个微妙的问题。升级 Python 或其依赖项可能会导致意外的问题，这些问题可能只在运行时显现。与 Rust 不同，Rust 在构建时更容易识别潜在问题，而 Python 开发者通常只有在部署后才会发现这些问题。

## 示例场景：将 Python 3.7 升级到 Python 3.9

想象一下你有一个运行在 Python 3.7 上的 Python 项目。该项目包含一个严重依赖字典的脚本。在 Python 3.7 中，字典的插入顺序作为实现细节被保留，但这并没有得到正式保证。你决定将 Python 环境升级到 Python 3.9，以便获得性能改进和新语言特性。

升级后，你会注意到你的脚本表现不同。在 Python 3.7 中，你可能无意中依赖了字典中项目的顺序来进行某些操作，即使这并未正式成为语言规范的一部分。如果你的代码依赖于字典中元素的顺序，并且在编写时没有意识到这种行为在 3.7 中并不被保证，那么如果在 Python 3.9 中实现有任何细微变化，它可能会表现得不可预测或中断。

这个例子说明了即使在同一主要版本的 Python（Python 3.x）内升级也可能导致意外的问题，特别是当代码依赖于未正式指定在语言中的行为时，而这些行为只是某一特定实现的副产品。在这个例子中，我们忽略了在次版本中添加的许多功能，这些功能常常改变了首选的工作流程。同时，也忽略了被弃用的函数。例如，一些方法和函数在 Python 中被移除，即使在次版本中。

Rust 对次版本中稳定 API 维护的强烈关注确保了兼容性，并减少了与升级相关的问题。其严格的语义版本控制和 Cargo 的依赖管理最小化了意外变化。这使得 Rust 的更新对于开发者来说更具可预测性和较少干扰。

# 总结

学习和使用 Rust 真的突显了每种语言在环境和依赖管理上的巨大差异。Python 的悠久历史促成了各种工具的发展，如 venv 和 Poetry，它们都在应对语言的动态特性和运行时依赖解决挑战。尽管这些工具有效，但它们往往更像是必要的变通方法，而不是语言的集成组件。

相比之下，Rust 通过 Cargo 的简化方法展示了其对更集成和用户友好体验的承诺。Cargo 高效的依赖管理，无需外部工具或‘PATH’操作，展示了 Rust 现代化的软件开发方法。

学习 Python 和 Rust 确实突显了每种语言的独特之处，并让我们窥见了软件开发的未来。我认为 Python 和 Rust 仍然有不同的目标，但可以看到它们越来越趋同。同时，随着机器学习社区逐渐向 Rust 迈进，Rust 的语言特性也被引入 Python，用于更成熟的产品。我对 Python 和 Rust 的未来充满期待！

我很期待听到你对从 Python 到 Rust 的这段旅程的看法和反馈。让我们在[LinkedIn](https://linkedin.com/in/dennisbakhuis)上联系，并继续交流！
