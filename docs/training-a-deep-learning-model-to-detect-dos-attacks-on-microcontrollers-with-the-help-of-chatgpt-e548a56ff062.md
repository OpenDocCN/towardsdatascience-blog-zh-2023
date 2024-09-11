# 训练深度学习模型以检测微控制器上的DoS攻击

> 原文：[https://towardsdatascience.com/training-a-deep-learning-model-to-detect-dos-attacks-on-microcontrollers-with-the-help-of-chatgpt-e548a56ff062?source=collection_archive---------8-----------------------#2023-04-11](https://towardsdatascience.com/training-a-deep-learning-model-to-detect-dos-attacks-on-microcontrollers-with-the-help-of-chatgpt-e548a56ff062?source=collection_archive---------8-----------------------#2023-04-11)

## 一个端到端的项目演练，包括一些来自ChatGPT的有用帮助

[](https://medium.com/@dehhmesquita?source=post_page-----e548a56ff062--------------------------------)[![Déborah Mesquita](../Images/3b77b7eb569e24f2679875429173daf1.png)](https://medium.com/@dehhmesquita?source=post_page-----e548a56ff062--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e548a56ff062--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e548a56ff062--------------------------------) [Déborah Mesquita](https://medium.com/@dehhmesquita?source=post_page-----e548a56ff062--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdd9e06a0a640&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-deep-learning-model-to-detect-dos-attacks-on-microcontrollers-with-the-help-of-chatgpt-e548a56ff062&user=D%C3%A9borah+Mesquita&userId=dd9e06a0a640&source=post_page-dd9e06a0a640----e548a56ff062---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e548a56ff062--------------------------------) ·10分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe548a56ff062&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-deep-learning-model-to-detect-dos-attacks-on-microcontrollers-with-the-help-of-chatgpt-e548a56ff062&user=D%C3%A9borah+Mesquita&userId=dd9e06a0a640&source=-----e548a56ff062---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe548a56ff062&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-deep-learning-model-to-detect-dos-attacks-on-microcontrollers-with-the-help-of-chatgpt-e548a56ff062&source=-----e548a56ff062---------------------bookmark_footer-----------)![](../Images/26325e98716c0af37c25e6d9ab7e7e07.png)

感谢 [Hamed](https://unsplash.com/pt-br/@hamedtaha) 提供的图片！

ESP32是一款广泛用于物联网项目的MCU（微控制器单元），因其低成本和ESP-IDF（Espressif物联网开发框架）而受到青睐。该开发板配备了双核32位处理器、单芯片Wi-Fi和蓝牙连接，广泛用于物联网、家居自动化和机器人项目。

物联网应用可以解决各行各业的问题，包括农业、家庭控制和智慧城市。令人遗憾的是，我还没有在我们生活中看到物联网项目，主要是在巴西。当我想到物联网项目时，第一个想到的就是它非常技术化，我们无法在没有大量资金的情况下实现。幸运的是，这种情况不再成立，因为我们现在拥有像ESP32这样的开发板，只需大约3美元，我们就可以构思和实验物联网项目的想法。

在了解到ESP32之后，我的目标是找到一种方式来融入这个物联网世界。LACNIC是一个国际组织，负责分配和管理互联网编号资源，并推动拉丁美洲和加勒比地区的互联网发展。该组织的[研究领域之一](https://www.lacnic.net/innovaportal/file/4883/1/cartera-tematica-2022_lineas-ejes-investigacion_en.pdf)是密码学、安全性和弹性。我一直缺乏参与研究项目的感觉，于是我提交了一份技术论文提案，重点关注物联网和网络安全，作为他们的[IT女性指导计划](https://www.lacnic.net/4883/2/lacnic/it-women-mentoring-program)的一部分。

我项目的主要参考是T800：物联网的防火墙工具和基准测试[1]，这是一个由巴西研究人员创建的项目，为ESP32创建了一个数据包过滤器以扫描攻击。他们在Github上[公开了所有代码](https://github.com/c2dc/T800)。由于这是我第一次使用ESP32和lwIP协议栈（稍后会详细介绍），如果没有这个参考，我将完全迷失。

很酷，但我的项目到底是什么呢？安全性应该是物联网系统开发的一个重要部分，这些设备因更新和维护周期较差而成为网络攻击的易受攻击目标[1]。我的工作重点是检测流量攻击，特别是检测DoS（服务拒绝）攻击。在流量攻击中，攻击者的目标是在短时间内向设备发送大量网络数据包，旨在中断系统的操作。

然后主要任务是训练一个机器学习模型来检测 DoS 攻击并将其部署到 ESP32 上。大多数用于检测 DoS 和 DDoS 攻击的模型使用体积和统计特征，如“聚合记录的平均持续时间”和“源到目的地的数据包计数”。我的目标是弄清楚是否可以创建一个基于**原始数据包特征**的模型，如 tcp.window_size 和 udp.length。这是一次相当艰难的旅程，今天我们将深入讨论主要的障碍以及 ChatGPT 如何在其中帮助我。

# 挑战 1 — 我的训练数据集的特征是否与生产中的特征匹配？

我使用的数据集是[CIC IoT Dataset 2022](https://www.unb.ca/cic/datasets/iotdataset-2022.html)，由加拿大网络安全研究所 (CIC) 创建，旨在生成用于不同 IoT 设备的状态-of-the-art 数据集，用于分析、行为分析和漏洞测试 [2]。他们使用 wireshark 作为网络协议分析器，捕获并保存网络数据包到 pcap 文件中。

在建模部分，我的主要参考是 DeepDefense: Identifying DDoS Attack via Deep Learning [3]。他们设计了一种递归深度神经网络，通过使用仅20个网络流量字段来学习网络流量序列中的模式。

为了与其他网络设备通信，ESP32 需要支持 TCP/IP 协议。为此，它使用 lwIP（轻量级 IP），这是一个为低内存和低计算能力的嵌入式系统设计的开源 TCP/IP 协议栈。我的第一个任务是弄清楚 ESP32 的 lwIP 实现中有哪些特性，并找出如何从 pcap 文件中提取它们。

起初我在阅读[wireshark 文档](https://www.wireshark.org/docs/dfref/f/frame.html)和[esp-lwip 代码](https://github.com/espressif/esp-lwip/blob/2c9c531f0a7e0ee536db9de4f9dc54e453712087/src/include/lwip/prot/ip4.h#L103)以尝试找到对应的特征，但后来我想“如果问 chatGPT 呢”？结果发现大模型非常有用：

![](../Images/cea64e837fd08ff76d181693adb134b9.png)

使用 chatGPT 获取 ESP32 代码中的“ip.ttl”

当然，我必须做一些调整，[1] 的代码是我的主要指南，但我通过使用模型帮助我获取所需特征的名称和变量，节省了很多时间。

# 挑战 2— 我如何进行训练和测试数据的拆分？

数据集中包含3个包含洪水攻击的 pcap 文件，针对每个 IoT 设备。攻击是基于 HTTP、UDP 和 TCP 协议进行的。我想“好吧，我可以用2个攻击进行训练/验证，用1个进行测试”，但主要问题是**如何将恶意流量与合法流量合并以创建数据拆分**。

![](../Images/89706eed4452002e8f57e89f644584cf.png)

每个设备的攻击 pcap 文件，每个 pcap 文件只有恶意流量

数据集的合法流量是按天捕获的，因此我们有一个每天的 pcap（总共 30 天）。我最初的计划是使用类似于 [3] 的递归深度神经网络，因为这种神经网络会从 **网络流量序列**中学习模式，随机取样每个桶（合法与恶意）将效果不好。更糟糕的是，我读过的所有论文只提到他们使用了 80/20 切分，但没有解释他们是怎么做的。

我的目标是使训练数据集和测试数据集尽可能接近真实场景。经过大量的思考和反复试验，我找到了一种解决方案，虽然不确定是否完全实现了这个目标，但这也是一种进展。策略是使用 pcap 的 _ws.col.Time 特性在不同时间插入随机攻击。算法如下：

1.  以一天满是合法数据包的数据作为起点。

1.  对于每个攻击 pcap，在全天数据包的随机位置插入一些攻击数据包（50000到70000个），调整攻击数据包的 _ws.col.Time 并每次对数据集进行排序。

1.  对每个设备 pcap 重复第 2 步，使用每个设备的前 2 个攻击 pcap（第三个 pcap 将用于测试数据集）。

对于测试数据集，我做了相同的操作（从另一整天的合法数据包开始），但不是仅取 50000 到 70000 个攻击数据包，而是使用所有的攻击数据包。这使得测试数据集非常不平衡，但这正是现实中的情况。

# 挑战 3 — 随时学习 C++

用于编程 ESP32，我们使用 C++。除了大学里学过的一些基本 C 类别，我没有相关经验，但我想“我已经用 Python 编程很长时间了，只需处理一些语法上的变化就好了，我会没问题的”。确实大多数时候我都没问题，但对于一些事情，很难找到准确的查询来搜索我所需的信息，因此 chatGPT 变得非常有用。

一个例子是 `&` 操作符。如果你是一个 Python 程序员，从未用过 C++，你认为下一行代码会做什么？

`(TCPH_FLAGS(tcphdr) & TCP_SYN)`

返回 0 或 1 无论它们是否相同，对吧？错了！

![](../Images/3ecc114a9d01d92e4dc4283ef3425cba.png)

chatGPT 帮助我们搞清楚代码的作用

在训练数据集中，tcp.flags.syn 的值是 0 和 1，因此为了在 ESP32 上部署模型时得到相同的结果，我们需要这样做：

`input->data.f[7] = (TCPH_FLAGS(tcphdr) & TCP_SYN) ? 1 : 0;`

这个简单的问题让 chatGPT 帮我节省了大量的调试时间。谢谢 chatGPT。

# 很好，但模型有效吗？

模型使用了 10 个特征，架构非常简单：

```py
train_features = [
    "_ws.col.Time",
    "ip.hdr_len",
    "ip.flags.df",
    "ip.frag_offset",
    "ip.proto",
    "ip.ttl",
    "tcp.window_size",
    "tcp.flags.syn",
    "tcp.flags.urg",
    "tcp.hdr_len",
]

simple_nn_model = tf.keras.models.Sequential(
    [
        tf.keras.layers.InputLayer(input_shape=(10), dtype=tf.float32),
        tf.keras.layers.Dense(9, activation="relu", kernel_regularizer="l2"),
        tf.keras.layers.Dense(9, activation="relu", kernel_regularizer="l2"),
        tf.keras.layers.Dense(units=1, activation="sigmoid", kernel_regularizer="l2"),
    ],
    name="simple_nn",
)

simple_nn_model.compile(
    loss="binary_crossentropy",
    optimizer=tf.keras.optimizers.Adam(),
    metrics=["accuracy"],
)
```

测试数据集的准确率为 97%。

![](../Images/e7c3bec79bb6f6b1bfbd11cd0dc6c680.png)

测试数据集的结果

当我开始研究时，我想知道为什么我们不直接使用每秒数据包的阈值或其他方法，因为 DoS 攻击者会在短时间内向设备发送大量网络数据包。这不起作用，因为存在不同特征的体积攻击。机器学习模型是最佳选择，因为它们更稳健，能够检测到潜在的可疑流量，无论流量速率如何。

但模型是否有效也取决于应用程序和物联网系统。我想了解模型在物联网系统中的表现，因此我在 ESP32 上用一个简单的 UDP 服务器应用程序进行了测试。

我使用了一个 Python 脚本来生成合法流量，另一个 Python 脚本作为攻击者。结果如下：

```py
|                    | Malicious packets dropped (true positives) | Malicious packets processed (false negatives) | Legitimate packets processed |
| ------------------ | ------------------------------------------ | --------------------------------------------- | ---------------------------- |
| Without dosguard32 | \-                                         | 1798                                          | 20                           |
| With dosguard32    | 1855                                       | 495                                           | 20                           |
```

攻击运行了 60 秒，真实流量运行了 80 秒。总处理的数据包包括所有数据包（合法和恶意），而总合法数据包仅包括来自我模拟的真实传感器数据的数据包。防火墙在处理恶意数据包方面减少了 72.44%，同时没有干扰合法流量。恶意数据包的召回率为 0.789，这明显低于测试数据集的结果。需要注意的是，UDP 服务器实验的结果是初步的，因为它们基于单次执行。

如果你感兴趣，这是我用来模拟真实流量的代码（是的，我也向 chatGPT 请求了这段代码）：

```py
import socket
import random
import time

start_time = time.time()

# UDP server config
UDP_IP = "10.0.0.105" 
UDP_PORT = 3333 

# Create socket UDP
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Sending data
while (time.time() - start_time) < 80:
    temperature = round(random.uniform(18.0, 30.0), 2)
    humidity = round(random.uniform(30.0, 80.0), 2)

    data = f"Temp: {temperature} C, Humidity: {humidity} %".encode()

    sock.sendto(data, (UDP_IP, UDP_PORT))

    time.sleep(1)
```

我认为下一步主要是研究如何创建真正类似于真实网络流量的训练和测试数据集。在测试数据集中，我们获得了 99% 的召回率，而在我的“真实场景”测试中，很多攻击数据包仍被分类为合法。也许我的 Python 脚本攻击的特征与训练数据集中的攻击特征非常不同？模型是否过拟合？这些都是继续研究的有趣问题。

# 最后的想法

这个项目完全让我走出了舒适区，因为我不得不处理许多新的事物：

+   TCP/IP 协议（计算机网络）

+   DoS 攻击（网络安全）

+   ESP32 上使用的 TPC/IP 协议实现（嵌入式设备）

+   了解如何使用 TensorFlow Lite 将模型嵌入 ESP32（嵌入式设备 + 数据科学）

到目前为止，最困难的部分是处理 ESP32 上的 TCP/IP 栈，主要因为关于它的学习资料不多。幸运的是，[1] 的作者公开了他们的代码，我也根据他们的工作顺利上了轨道。这也是我喜欢开源社区的原因之一 ❤。

令我惊讶的是，chatGPT 也帮了我很多。我原本对它是否能解释一些 lwIP 方法和变量持怀疑态度，但它做得非常好。感觉就像我有一个 lwIP 专家可以提问和讨论想法。

关于大型语言模型是否会取代我们以及各种争论有很多，但我们也需要讨论它们如何改变游戏规则。它们在提高我们学习新事物的能力和使知识对每个人更易获取方面非常强大。

当然，你可以在这里查看我们今天讨论过的所有代码：[https://github.com/dmesquita/dosguard32](https://github.com/dmesquita/dosguard32)

如果你读到这里，非常感谢你的阅读！ :D

# 参考文献

[1] Fernandes, Gabriel Victor C., 等。“为物联网设备实现智能包过滤器。” *XL巴西计算机网络与分布式系统研讨会论文集*。SBC，2022。

[2] Sajjad Dadkhah、Hassan Mahdikhani、Priscilla Kyei Danso、Alireza Zohourian、Kevin Anh Truong、Ali A. Ghorbani，“[*朝着开发现实的多维物联网分析数据集*](https://ieeexplore.ieee.org/document/9851966)”，提交至：第19届国际隐私、安全与信任年会（PST2022），2022年8月22–24日，加拿大弗雷德里克顿。

[3] 袁小勇、李传煌、李晓林。 “DeepDefense：通过深度学习识别DDoS攻击。” 2017 IEEE国际智能计算会议（SMARTCOMP）。IEEE，2017。

[4] Hamza, Ayyoob、Hassan Habibi Gharakheili 和 Vijay Sivaraman。“物联网网络安全：需求、威胁和对策。” arXiv预印本 arXiv:2008.09339（2020）。
