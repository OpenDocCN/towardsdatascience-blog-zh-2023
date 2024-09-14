# Apple Silicon Macs（M1 和 M2）的终极 Python 和 Tensorflow 设置指南

> 原文：[`towardsdatascience.com/the-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06`](https://towardsdatascience.com/the-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06)

## 逐步指南

## 在 ARM Mac 上安装 TensorFlow 和 Python 从未如此简单

[](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------) ·6 分钟阅读·2023 年 2 月 25 日

--

![](img/2cd7fe5600bcbb2c4b7f9f2473504fe0.png)

照片由 [Ales Nesetril](https://unsplash.com/de/@alesnesetril?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一年前我换了工作，发现自己拥有一台全新的 M1 MacBook Pro。我需要安装数据科学家所需的一切，这确实是一件痛苦的事。

更糟糕的是，事实证明我没有正确设置，一片混乱。

但一位同事分享了一套步骤，在按照这些步骤成功设置之后，我想把它分享给大家。所以我总结并扩展了额外的信息和步骤，以确保完整性。因为我在网上找不到类似的内容，至少在我搜索时没有找到，所以我决定公开它，以便大家都能受益。

在这个故事中，你会找到一个逐步指南，讲解如何在 M1 和 M2 Mac 上成功安装 Python 和 Tensorflow，而无需自己费力去设置。

工作流程相对简单：

1.  我们将首先安装任何开发者在 Mac 上需要的基本要求（如果你不是在使用新的 Mac，可能这些要求已经满足了）。

1.  然后，使用**pyenv**进行 Python 的安装。

1.  最后，正确安装和设置 Tensorflow 以适用于 M1 或 M2 Mac。

你只需要一台 ARM Mac，就可以开始了！

# 设置 Python

现在的 Mac 通常已经预装了 Python，至少是 Python2，但我相信在像 M1 或 M2 MacBook 这样的 arm64 设备上，有更好和推荐的 Python 使用方式。

## XCode 命令行工具

首先，你需要**安装 XCode 命令行**工具：

```py
~> xcode-select --install
~> xcrun --show-sdk-path
/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
```

这些工具是**为在命令行中运行的软件开发人员**准备的，存在于终端应用程序中。自从苹果公司成立之前，这些基于 Unix 的工具组合就已经是几乎所有软件开发的基础。

## Homebrews

一旦我们安装了命令行工具，我们需要**安装两个 homebrews**。为什么是两个？我们想要一个本地（Apple Silicon）和一个 Intel（也叫 Rosetta）。为此，我们需要为每种架构准备一个 Terminal/iTerm2。很简单。

*我推荐使用 iTerm2[1]，因为它是改进版的 macOS 终端，但如果你更喜欢使用内置终端，它的功能是相同的。*

转到你的应用程序文件夹，复制“iTerm”并将副本重命名为“intel iTerm”（或类似名称）。对于那个新应用程序，右键点击并选择“获取信息”。在常规部分，勾选“使用 Rosetta 打开”。

现在我们需要设置 Homebrew [2]。

从 Intel 终端运行：

```py
~> arch --x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> 如果你不知道自己使用的是哪个终端，可以使用`uname -m`并查看输出：
> 
> - 如果你看到`x86_64`，那么你在 Intel/Rosetta 终端中。
> 
> - 但如果输出显示`arm64`，则说明你在本地终端（M1/M2）中。

现在打开本地终端（默认的 iTerm 或 Terminal），并运行下一组指令：

```py
~> cd ~/Downloads
~> mkdir homebrew
~> curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
~> sudo mv homebrew /opt/homebrew
```

现在检查两个终端中是否都已正确安装。

+   在 Intel iTerm 上：

```py
~> brew --prefix
/usr/local
~> brew list
<list of installed packages>
```

+   在本地 iTerm 上：

```py
~> brew --prefix
/opt/homebrew
~> brew list
<list of installed packages (different than x86_64)>
```

现在，为了正确管理这两个环境，我们需要进行适当的设置。只需在你的`~/.zshrc`文件中添加以下配置：

```py
... rest of the file above ...

#
# homebrew setup, following https://noahpeeters.de/posts/apple-silicon/homebrew-setup/
#
if [ -d "/opt/homebrew/bin" ]; then
    export PATH="/opt/homebrew/bin:$PATH"
fi

function ibrew() {
   arch --x86_64 /usr/local/bin/brew $@
}

# variables needed to properly install things under intel or m1

ARCH="$(uname -m)"
case ARCH in
    i386)   ARCH="386" ;;
    i686)   ARCH="386" ;;
    x86_64) ARCH="amd64" ;;
    arm)    dpkg --print-architecture | grep -q "arm64" && ARCH="arm64" || ARCH="arm" ;;
esac

if [[ "${ARCH}"  == "arm64" ]]; then
    PREFIX="/opt/homebrew"
else
    PREFIX="/usr/local"
fi

# https://github.com/pyenv/pyenv/issues/1768
SDK_PATH="$(xcrun --show-sdk-path)"

echo $PREFIX
echo $SDK_PATH

export CPATH="${SDK_PATH}/usr/include"
export CFLAGS="-I${SDK_PATH}/usr/include/sasl $CFLAGS"
export CFLAGS="-I${SDK_PATH}/usr/include $CFLAGS"
export CFLAGS="-I${PREFIX}/include $CFLAGS"
export LDFLAGS="-L${SDK_PATH}/usr/lib $LDFLAGS"
export LDFLAGS="-L${PREFIX}/lib $LDFLAGS"
export LDFLAGS="-L${PREFIX}/opt/openssl/lib $LDFLAGS"
export CPPFLAGS="-I${PREFIX}/opt/openssl/include $CPPFLAGS"
```

为了让 brew 正常工作，只需要第一个部分……但是后面添加的其他标志可能也会有用（可能需要？）。

> 记录一下：我们会尽量首先使用本地 brew 来安装包。只有当某个包安装失败且确认本地版本无法安装时，我们才会使用 Intel brew。

## Pyenv

好的，现在我们已经可以开始安装与 Python 相关的内容了。我们将从**pyenv**开始，这将使安装各种 Python 版本的过程极其顺利。

打开你的本地终端，然后简单地输入下面的命令：

```py
~> brew install pyenv
```

## 构建依赖

为了让 pyenv 正常工作，我们需要安装几个依赖项[3]。在我们的例子中：

```py
~> brew install openssl readline sqlite3 xz zlib tcl-tk libffi
```

此外，我们还希望在我们的 intel homebrew 中添加相同的依赖项，所以：

```py
~> ibrew install openssl readline sqlite3 xz zlib tcl-tk libffi
```

现在，你可以开始安装 Python 版本了。下面是一个示例，如果你想安装 Python 3.9.9 并将其设置为默认版本：

```py
~> pyenv install 3.9.9
~> pyenv global 3.9.9
```

如果你想查看当前安装的版本，只需使用`pyenv versions`。

因为这种过程双重检查总是值得的，所以运行下一个命令：`~/.pyenv/versions/3.9.9/bin/python`，将`3.9.9`替换为你决定安装并设置为全局的版本。一旦进入 shell，尝试导入`ctypes`：

```py
Python 3.9.9 (main, Feb 15 2023, 11:25:42)
[Clang 14.0.0 (clang-1400.0.29.202)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import ctypes
>>>
```

如果它没有中断，那么它就已正确安装。

## Pipenv

虽然安装它不是强制性的，但为你的项目使用环境始终是一个好习惯。这也是我决定在这里提到它的原因，因为它简单且好用。安装方法如下：

```py
~> brew install pipenv
```

现在你可以在任何项目中使用它。你可以查看 pipenv 的基本用法，访问他们的官方文档[4]或寻找在线教程。

# 安装 Tensorflow

现在进入第二部分。安装 TensorFlow 在 M1 或 M2 Mac 上曾让我们许多人感到痛苦，但现在不必再这样了。

你会记得我们之前编辑了 `~/.zshrc` 文件。现在，我们将在文件末尾添加这一行：

```py
# tensorflow grpcio https://github.com/grpc/grpc/issues/25082
export GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1
export GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1
export CFLAGS="-I/opt/homebrew/opt/openssl/include"
export LDFLAGS="-L/opt/homebrew/opt/openssl/lib"
```

一旦添加完成，我们需要将 miniconda 下载到 `~/` 目录。你可以直接从 [`repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh`](https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh) 下载，并手动移动到主文件夹。

下载完成后，我建议退出并重新打开原生终端，然后运行：

```py
~> bash ~/Miniconda3-latest-MacOSX-arm64.sh -b -p $HOME/miniconda
```

现在我们已经安装了 conda，我们将激活环境并安装一些 Tensorflow 的依赖项：

```py
~> source ~/miniconda/bin/activate
(base) ~> conda install -c apple tensorflow-deps
```

再次，通过退出（Cmd + Q）并重新打开终端，你现在可以安装 Tensorflow。我将以这种方式在专用环境中进行安装：

1.  进入你的项目文件夹：例如 `cd Documents/project`

1.  激活环境：`pipenv shell`

1.  安装 Tensorflow：`pipenv install tensorflow-macos`

*瞧好了!* 你现在应该能在 M1 或 M2 Mac 上正确使用 TensorFlow 了。

看看我安装了一个名为 *tensorflow-metal*[5] 的包来加速我们 Mac 上 GPU 模型的训练，所以你可以考虑用

```py
~> pipenv install tensorflow-metal
```

# 结论

现在你知道如何应对设置全新 Mac 用于数据科学的痛苦，并充分利用其新芯片。

希望这些信息对你有所帮助，如果你遇到任何疑问或问题，请随时在这个故事下评论——我或其他专家一定会帮助你。

我不想离开而不感谢你阅读我的故事。我经常在 Medium 上发帖，如果你喜欢这篇文章，欢迎关注我。这对我很有帮助。

如果你想进一步支持我，可以通过下面的链接订阅 Medium 会员：这不会让你多花一分钱，但它会帮助我度过这个过程。非常感谢！

[](https://medium.com/@polmarin/membership?source=post_page-----e9ef304a2c06--------------------------------) [## 通过我的推荐链接加入 Medium — Pol Marin

### 阅读 Pol Marin 的所有故事（以及 Medium 上其他成千上万的作家）。你的会员费直接支持 Pol…

medium.com](https://medium.com/@polmarin/membership?source=post_page-----e9ef304a2c06--------------------------------)

# 资源

[1] [iTerm2 — macOS 终端替代品](https://iterm2.com/index.html)

[2] Noah Peters, [在 Apple Silicon 上设置 Homebrew](https://noahpeeters.de/posts/apple-silicon/homebrew-setup/)

[3] [Pyenv 维基](https://github.com/pyenv/pyenv/wiki#suggested-build-environment)

[4] [Pipenv 的基础用法](https://pipenv.pypa.io/en/latest/basics/#basic-usage-of-pipenv)

[5] [官方 PyPi — Tensorflow-metal 包](https://pypi.org/project/tensorflow-metal/)
