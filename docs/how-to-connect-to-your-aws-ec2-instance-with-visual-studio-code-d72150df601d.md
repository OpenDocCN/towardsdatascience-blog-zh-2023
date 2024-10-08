# 如何通过 Visual Studio Code 连接到您的 AWS EC2 实例

> 原文：[`towardsdatascience.com/how-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d`](https://towardsdatascience.com/how-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d)

## 一个快速简便的指南，教您如何使用 VS Code 访问远程服务器

[](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------) ·阅读时间 7 分钟·2023 年 4 月 27 日

--

![](img/31f029a0a26bb457b3ee2006cf821363.png)

图片由 [John Barkiple](https://unsplash.com/@barkiple?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

**Visual Studio Code** 毫无疑问是我最喜欢的集成开发环境。它简单、直观的用户界面使其成为我进行数据科学项目时的首选编辑器。

实际上，它如此出色，以至于即使在处理远程服务器时，我也不愿意与其分离。

这有问题吗？不，完全没有！

毕竟，**Visual Studio Code** 可以配置以允许用户在远程服务器上工作。最棒的是，这个过程不会超过 10 分钟！

这对需要计算或存储资源的项目非常有用，而这些资源是本地机器无法提供的。当我需要处理大型数据集时，我非常依赖这个功能。

如果您处于类似的情况，希望在享受 **Visual Studio Code** 舒适的同时处理大数据或训练深度学习模型，本教程将向您展示如何使用这个集成开发环境连接到您的 AWS EC2。

## 案例研究

在本教程中，我们将把包含两个文件的文件夹移动到 EC2 实例：一个名为 `random_number.py` 的 Python 文件和一个包含所有 Python 脚本依赖项的 `requirements.txt` 文件。

文件移动到远程服务器后，将执行 `random_number.py` 脚本。

`random_number.py` 执行一个简单的任务：打印一个随机数字。

## 第一步 — 启动实例

首先，让我们启动稍后要连接的 EC2 实例。

您可以根据需要选择实例类型，但在选择时请考虑以下几点：

**1\. AMI：** 你可以使用任何你想要的 AMI，但请记住，它将影响你稍后在 Visual Studio Code 中输入的配置值。

这个演示的 EC2 实例将使用 Ubuntu AMI 启动。

![](img/b612ccdb9f88602ab429c4b0a553a6e4.png)

AMI（由作者创建）

**2\. 密钥对：** 连接到实例时需要一个密钥对。因此，请将其保存在安全的地方。

这个实例的密钥对名为“demo_kp.pem”，将存储在下载文件夹中。

![](img/ce909b6cd98d4fe7ae419f9e66bfd66c.png)

密钥对（由作者创建）

## 步骤 2 — 配置 Visual Studio Code

我们将使用安全 Shell（SSH）访问 EC2 实例，这是一种允许两台计算机安全地通信和共享信息的网络协议。

为了使 Visual Studio Code 能够使用 SSH，我们需要首先安装所需的扩展。点击左侧的扩展按钮或按 Ctrl+Shift+X 进入市场。输入“Remote - SSH”并选择第一个条目。

![](img/100b72adcff8b9ad0747440560166ed5.png)

扩展市场（由作者创建）

如果尚未安装扩展，请安装它。

![](img/6251bbeba0784259d85f2860c4b45aba.png)

已安装扩展（由作者创建）

接下来，通过点击窗口左下角的绿色图标打开远程窗口：

![](img/d2bfbfe5d51efc09eeb5229bee4bbb7f.png)

打开远程窗口（由作者创建）

选择“连接到主机”。

![](img/a0ef7c34f51e1959e9edad269683935c.png)

连接到主机（由作者创建）

然后，选择“配置 SSH 主机”（而不是蓝色高亮的那一行）：

![](img/f90aea23e5595d052bed6acabc0639f2.png)

配置 SSH 主机（由作者创建）

选择这个计算机的配置文件，在这种情况下是第一个。

![](img/516dc9e3b596887ae75a4ded6d478bd6.png)

选择配置文件（由作者创建）

打开配置文件后，输入以下关键词的详细信息：**Host**、**HostName**、**User** 和 **IdentityFile**。

对于案例研究，配置文件将如下所示。

```py
Host demo-host
    HostName ec2-18-218-169-69.us-east-2.compute.amazonaws.com
    User ubuntu
    IdentityFile C:/Users/aashi/Downloads/demo_kp.pem 
```

这是对关键词的快速分解以及你接下来应该写的内容。

**1\. Host：** 这是用户指定的名称。可以是你想要的任何名称。

案例研究的主机将被称为“demo-host”。

**2\. HostName：** HostName 应该是实例的公共 DNS。要找到实例的 DNS，首先在 EC2 控制台中选择该实例。

![](img/f2f6c41f77bfaf5867ee3f1ac13132c0.png)

选择实例（由作者创建）

然后，点击“Connect”并切换到“SSH client”标签页。公共 DNS 将在说明中提供。

![](img/533606115e697c0d44b4760aef828c64.png)

公共 DNS（由作者创建）

**3\. User:** 这是实例的用户名。用户名取决于为 EC2 实例选择的 AMI。如果你不确定用户名是什么，请访问[AWS 文档](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)。

在我的案例中，用于启动实例的 AMI 是 Ubuntu，因此用户将是“ubuntu”。

**4\. IdentityFile:** 这是分配给 EC2 实例的私钥文件的路径。在本案例研究中，此关键字的值是“demo_kp.pem”文件的路径。

将所有组件拼接在一起后，配置文件应该如下所示：

![](img/1976ee801fd9be09b216fdae1123d76c.png)

配置文件（作者创建）

完成所有更改后，保存配置文件。你现在可以准备连接到这个 EC2 实例。

> 注意：实例的公共 DNS 每次启动实例时都会更改。因此，每当你启动实例时，你需要通过将 HostName 值更新为新的公共 DNS 来更新配置文件。

## 第 3 步 — 连接到实例

再次选择左下角的绿色按钮，然后选择“连接到主机”。

![](img/ca5c10cdbd16ae592441e1c61d6cde76.png)

连接到主机（作者创建）

这时，你应该能看到你创建的主机。

![](img/dd5b763d332794e98b61513569ff1bfc.png)

选择主机（作者创建）

选择你创建的主机后，应该会弹出一个新窗口，让你直接访问远程服务器！

![](img/a97d78e945492601283c7bee00e5d837.png)

新窗口（作者创建）

如果你查看窗口的左下角，你应该能看到你创建的主机的名称。此外，你可以通过终端确认与实例的连接。

![](img/8076c6ba726eedb51faf0dada04cb80e.png)

代码输出（作者创建）

现在你可以访问远程服务器，你可以使用 Visual Studio Code 轻松编写代码和导航目录！

## 第 4 步 — 设置实例中的环境

所以，我们已经成功地使用 Visual Studio Code 连接到 EC2 实例。

然而，工作还没完成。毕竟，我们需要设置环境，以便在服务器中使用`random_number.py`和`requirements.txt`文件。

对于本案例研究，我们将在远程服务器的终端中进行以下安装：

```py
sudo apt-get update
sudo apt-get install python3-pip
sudo pip3 install virtualenv
```

有了这些安装，我们可以创建一个虚拟环境进行操作。

接下来，我们将获取运行`random_number.py`文件所需的文件。有几种方法可以实现这一点。

首先，由于我们使用的是 Visual Studio Code，我们可以简单地将脚本从本地机器复制并粘贴到远程服务器的脚本中。记住，我们不受命令行界面的限制。

话虽如此，将文本从一个文件手动复制到另一个文件可能会很麻烦。相反，使用安全复制协议（SCP）将文件复制到远程服务器会更好。

使用 SCP，我们可以用一个命令将整个目录复制到远程服务器上！

复制整个文件夹的语法如下：

```py
scp -i <path/to/key_pair.pem> -r <path/to/folder_to_copy> <user>@<public DNS>:<path/to/destination>
```

![](img/57920c2078910cfb39a0a078bfbe610e.png)

使用 SCP 复制（由作者创建）

另外，如果你只是想复制单个文件，可以使用具有以下语法的 SCP 命令：

```py
scp -i </path/key-pair-name.pem> </path/file_to_copy> <username>@<instance_public_dns>:<destination_path>
```

类似于配置 Visual Studio Code，使用 SCP 也要求用户知道他们的密钥文件路径、用户名和公共 DNS。使用在 Visual Studio Code 配置文件中输入的相同信息。

现在，我们的远程服务器应该有我们需要的所有文件了！

![](img/19ab439431b423bb9f5bb918eb2eecb3.png)

代码输出（由作者创建）

让我们创建一个名为 `test_venv` 的虚拟环境并进入它。

```py
python3 -m venv test_venv
source test_venv/bin/activate
```

![](img/ab25533d93b667d3669fabacdd1b85e0.png)

代码输出（由作者创建）

在这个虚拟环境中，安装`requirements.txt`文件中的包。

```py
pip3 install -r requirements.txt
```

![](img/542027270a6e4d1b8ec7ccfa61666a98.png)

代码输出（由作者创建）

最后，我们可以运行我们的 `random_number.py` 文件！

```py
python3 random_number.py
```

![](img/d6a739cd3d618914f3636b76af9b92f7.png)

代码输出（由作者创建）

## 最终备注

![](img/96e071b12b9091744cbc6fbd48f15153.png)

照片由 [Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你原本认为在远程服务器上工作意味着使用像 vi 这样的枯燥编辑器，希望这篇文章改变了你的看法。

拥有这一新技能后，Visual Studio Code 的粉丝们将能够处理更具计算密集型的任务，如深度学习项目，而无需使用不太喜欢的 IDE。

最后，由于你现在正在使用 EC2 实例，请记住每次启动实例时，你的实例公共 DNS 都会发生变化，因此你需要每次启动实例时更新 Visual Studio Code 中的配置文件（以及任何使用公共 DNS 的命令）。

感谢阅读！
