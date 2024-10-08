# 数据科学家的 Git 深入探讨

> 原文：[`towardsdatascience.com/git-deep-dive-for-data-scientists-1c9cc45c7612`](https://towardsdatascience.com/git-deep-dive-for-data-scientists-1c9cc45c7612)

## 通过实际场景学习 Git

[](https://khuyentran1476.medium.com/?source=post_page-----1c9cc45c7612--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----1c9cc45c7612--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c9cc45c7612--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c9cc45c7612--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----1c9cc45c7612--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c9cc45c7612--------------------------------) ·阅读时间 9 分钟·2023 年 7 月 1 日

--

![](img/e30842919626704a8df3c72287b54e47.png)

作者提供的图片

*最初发表于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/07/01/git-deep-dive-for-data-scientists/) *于 2023 年 7 月 1 日。*

# 为什么为你的数据科学项目选择 Git？

Git 是一个在软件开发中广泛使用的版本控制系统，但它是否适合你的数据科学项目？绝对适合。

以下是 Git 对数据科学无价的几个原因：

## 版本控制

**场景：**

你用新的数据处理方法替换了当前的方法。发现新方法没有产生预期结果后，你希望恢复到之前的有效版本。

不幸的是，没有版本控制，撤销多个更改会变成一项艰巨的任务。

**解决方案：**

使用 Git，你可以跟踪代码库的更改，切换不同版本，比对变化，并在必要时恢复到稳定状态。

![](img/5037d5acd6ca03a0b38f322e71681c27.png)

作者提供的图片

## 协作

**场景：**

你和其他数据科学家在一个机器学习项目上合作。为了合并团队成员做出的所有更改，你需要手动交换文件并审查彼此的代码，这需要时间和精力。

**解决方案：**

Git 使得合并更改、解决冲突和同步进度变得简单，允许你和你的团队成员更高效地合作。

![](img/077961146c454a288fac17990734f0ef.png)

作者提供的图片

## 分支

**场景：**

你希望探索新的方法来提升模型的性能，但又不愿直接更改生产代码。对已部署模型的任何意外影响可能会对公司产生重大后果。

**解决方案：**

利用 Git 的分支功能，你可以为不同的功能创建独立的分支。这使你能够进行测试和迭代，而不会影响生产分支的稳定性。

![](img/30572e3b261a432ca738201afc8f2cda.png)

图片作者

## 备份

**场景：**

硬件故障或盗窃会导致你所有的代码丢失，让你感到沮丧，并使你在几个月的工作中陷入困境。

**解决方案：**

Git 通过将你的项目安全地存储在远程仓库中来备份你的项目。因此，即使遇到这样的不幸事件，你也可以从远程仓库恢复代码库，继续工作而不会丧失重要进展。

![](img/a88960ca14142a5299b16825ee08862f.png)

图片作者

# 如何使用 Git

现在我们了解了 Git 在数据科学项目中的价值，让我们探索如何在不同场景中有效地使用它。

## 初始化 Git

要在当前项目中初始化 Git 并将项目上传到远程仓库，请按照以下步骤操作：

首先，在项目目录中初始化一个新的 Git 仓库：

```py
git init
```

接下来，将远程仓库添加到你的本地 Git 仓库中。要使用 GitHub 作为远程仓库，请在 GitHub 上创建一个新仓库并复制其 URL。

![](img/30972052e000fa20baa0cfafbfe85372.png)

图片作者

然后，将 URL 添加到你的本地 Git 仓库中，命名为“origin”：

```py
git remote add origin <repository URL>
```

接下来，在你的 Git 仓库中暂存更改或新文件：

```py
# Add all changes in the current directory
git add .
```

审核待提交的更改列表：

```py
git status

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .flake8
        new file:   .gitignore
        new file:   .pre-commit-config.yaml
        new file:   Makefile
        new file:   config/main.yaml
        new file:   config/model/model1.yaml
        new file:   config/model/model2.yaml
        new file:   config/process/process1.yaml
        new file:   config/process/process2.yaml
        new file:   data/final/.gitkeep
        new file:   data/processed/.gitkeep
        new file:   data/raw.dvc
        new file:   data/raw/.gitkeep
        new file:   docs/.gitkeep
        new file:   models/.gitkeep
        new file:   notebooks/.gitkeep
        new file:   pyproject.toml
        new file:   src/__init__.py
        new file:   src/process.py
        new file:   src/train_model.py
        new file:   tests/__init__.py
        new file:   tests/test_process.py
        new file:   tests/test_train_model.py
```

将暂存的更改永久保存到仓库历史中，并附上提交消息：

```py
git commit -m 'init commit'
```

一旦你的提交完成并存储在本地仓库中，你可以通过将它们推送到远程仓库与他人共享你的更改。

```py
# push to the "main" branch on the "origin" repository
git push origin main
```

运行此命令后，远程仓库上的“main”分支将接收来自本地仓库的最新更改。

![](img/4fc7909d84dd1b47503388811c38ed99.png)

图片作者

## 为现有项目做出贡献

要为现有项目做出贡献，首先在本地机器上创建远程 Git 仓库的本地副本：

```py
git clone <repository URL>
```

这个命令将创建一个与远程仓库同名的新仓库。要访问文件，请导航到仓库目录：

```py
cd <repository-name>
```

在“main”分支之外的单独分支上进行更改是一种良好的实践，以避免对主要代码库产生任何影响。

使用以下命令创建并切换到一个新分支：

```py
git checkout -b <branch-name>
```

对新分支进行一些更改，然后将更改添加、提交并推送到远程 Git 仓库的新分支上：

```py
git add .
git commit -m 'print finish in process_data'
git push origin <branch-name>
```

推送提交后，你可以创建拉取请求，将更改合并到“main”分支中。

![](img/15b2689a1259f114d683ef5862be9936.png)

图片作者

在你的同事批准并合并你的拉取请求后，你的代码将被集成到“main”分支中。

## 将本地更改与远程更改合并

假设你从主分支创建了一个名为“feat-2”的分支。在对“feat-2”分支进行了一些更改之后，你发现主分支已经更新。你怎么将远程更改合并到本地分支中？

![](img/1cb37d2b49af0bd60c18077d0fb25764.png)

作者提供的图片

首先，通过暂存和提交本地更改确保你的本地工作已保存。

```py
git add .
git commit -m 'commit-2'
```

这可以防止远程更改覆盖你的工作。

接下来，使用 `git pull` 从远程仓库的主分支拉取更改。第一次执行此命令时，你将被提示选择一种策略来协调分支。以下是可用的选项：

```py
$ git pull origin main                        
From https://github.com/khuyentran1401/test-git
 * branch            main       -> FETCH_HEAD
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

运行 `git pull origin main --no-rebase` 将在“feat-2”分支中创建一个新的合并提交，将“main”分支和“feat-2”分支的历史记录联系在一起。

![](img/5208931e81b49ee7deb94c2c5c84af19.png)

作者提供的图片

运行 `git pull origin main --rebase` 将执行变基操作，将“feat-2”分支的提交放在“main”分支之上。

![](img/ad3570343292409421b509f4d58e4714.png)

作者提供的图片

变基不会像合并那样创建新的合并提交；而是修改现有的“feat-2”分支的提交记录。这会导致更干净的提交历史。

然而，变基命令需要谨慎使用，特别是当其他团队成员也在积极使用相同的分支，例如“feat-2”分支时。

如果你在其他人也在工作于“feat-2”分支时进行变基操作，可能会导致分支历史的不一致。Git 在尝试同步这些分支时可能会遇到困难。

![](img/506b53bccd53bd84d0581c82af5f769f.png)

作者提供的图片

如果你是 Git 新手，并且更重视简单性而不是维护干净的历史，使用合并方法作为默认选项，因为它通常比变基更容易理解和使用。

## 恢复到上一个提交

想象一下：在创建了新的提交之后，你发现其中存在错误，并且想要恢复到某个特定的提交。你该怎么做？

首先，通过运行以下命令确定要恢复的特定提交的哈希值：

```py
git log 

commit 0b9bee172936b45c3007b6bf6fa387ac51bdeb8c 
    commit-2

commit 992601c3fb66bf1a39cec566bb88a832305d705f
    commit-1
```

假设你想要恢复到“commit-1”，你可以使用 `git revert` 或 `git reset`。

`git revert` 创建一个新的提交，用于撤销在指定提交之后所做的更改。

![](img/16a48a0b0058f6bc0f78ff50afcc6575.png)

作者提供的图片

`git reset` 通过将分支指针更改为指定的提交来修改提交历史。

![](img/599ffafd41490cfaa1e3aea22d255338.png)

作者提供的图片

虽然 `git reset` 保持了提交历史的整洁，但它更具破坏性，因为它会丢弃提交。`git revert` 是一个更安全的选择，因为它保留了原始提交。

# 好习惯

## 忽略大型和私有文件

在 Git 仓库中，排除特定文件或目录以解决如文件大小过大和隐私问题等问题是至关重要的。

在数据科学项目中，你应该忽略某些文件，例如数据集和秘密，原因如下：

+   数据集：对二进制数据集进行版本控制可能会显著增加仓库的大小。

+   秘密：数据科学项目通常需要凭证或 API 密钥来访问外部服务。如果将这些秘密包含在代码库中，如果仓库被泄露或公开共享，将会存在安全风险。

要排除特定的文件或目录，你可以将它们添加到项目根目录下的 .gitignore 文件中。以下是一些示例：

```py
# .gitignore 
data/ 
.env
```

此外，你还应该忽略那些可能导致文件大小过大或特定于你的开发环境的非必要文件，例如依赖管理文件如“venv”或编辑器特定文件如“.vscode”。

在这里可以找到适用于你的语言的有用 `.gitignore` 模板列表 [here](https://github.com/github/gitignore)。

# 保持小的提交

将更改拆分成小而集中的提交。这种方法确保每次提交都有明确的目的，使其更容易理解、在需要时还原更改，并且减少冲突的可能性。

![](img/998c60a091bdb54a980e453b0af070ad.png)

作者提供的图像

# 使用描述性分支名称

选择准确反映你正在处理的任务或特性的描述性分支名称。避免使用模糊的名称如“add file”或个人标识符如“john-branch”。相反，选择更具描述性的名称，如“change-linear-model-to-tree-model”或“encode-categorical-columns”。

## 标准化代码格式以便于代码审查

一致的代码格式化帮助审查者将注意力集中在代码的逻辑上，而不是格式上的不一致。

在下面的代码片段示例中，由于不规则的缩进、空格和引号，审查者很难准确定位 print 语句的添加。

![](img/cf169ed97b77fd6a1efe2e6be2cc00c7.png)

作者提供的图像

标准化代码提高了代码的可读性。

![](img/c19aea6fe5f409b107fcde46008f93b7.png)

作者提供的图像

我喜欢的格式化和标准化代码的工具有：

+   [Ruff](https://github.com/astral-sh/ruff): 一个极其快速的 Python linter，使用 Rust 编写。

+   [black](https://black.readthedocs.io/en/stable/): 一个 Python 代码格式化工具，可以自动重新格式化你的代码。

# 补充 Git 的工具

## pre-commit

在每次提交之前，按照风格指南维护一致的代码风格可能会令人感到不堪重负。pre-commit 通过在提交之前检查和重新格式化代码来自动化这一过程。以下是其工作原理的示例：

```py
$ git commit -m 'my commit'

ruff.......................................Failed
- hook id: ruff
- exit code: 1

Found 3 errors (3 fixed, 0 remaining).

black....................................Failed
- hook id: black
- files were modified by this hook

reformatted src/process.py

All done! ✨ 🍰 ✨
1 file reformatted.
```

# DVC

[DVC](https://dvc.org/doc/start)（数据版本控制）是一个用于数据版本控制的系统。它本质上类似于 Git，但用于数据。DVC 允许你将原始数据存储在一个独立的位置，同时在 Git 中跟踪数据的不同版本。

![](img/03eaadf18d44853d31f5d8d3c6f72478.png)

图片由作者提供

了解更多关于如何在[这篇文章](https://mathdatasimplified.com/2023/02/20/introduction-to-dvc-data-version-control-tool-for-machine-learning-projects-2/)中使用 DVC 的信息。

# 结论

通过采用 Git 并利用其功能以及这些补充工具，你可以提高生产力，保持代码质量，并与团队成员高效协作。

我喜欢撰写数据科学概念的文章并玩弄各种数据科学工具。你可以通过以下方式跟踪我最新的帖子：

+   订阅我在[数据科学简化](https://mathdatasimplified.com/subscribe/)上的新闻通讯。

+   在[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)上与我联系。

# 参考

Atlassian. (n.d.). *合并与变基：Atlassian Git 教程*。Atlassian. [`www.atlassian.com/git/tutorials/merging-vs-rebasing`](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
