# 什么是EDI？电子数据交换

> 原文：[https://towardsdatascience.com/what-is-edi-electronic-data-interchange-92f7215bb699?source=collection_archive---------2-----------------------#2023-08-29](https://towardsdatascience.com/what-is-edi-electronic-data-interchange-92f7215bb699?source=collection_archive---------2-----------------------#2023-08-29)

## 探索电子数据交换（EDI）如何促进现代供应链管理。

[](https://s-saci95.medium.com/?source=post_page-----92f7215bb699--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----92f7215bb699--------------------------------)[](https://towardsdatascience.com/?source=post_page-----92f7215bb699--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----92f7215bb699--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----92f7215bb699--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-edi-electronic-data-interchange-92f7215bb699&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----92f7215bb699---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----92f7215bb699--------------------------------) ·10分钟阅读·2023年8月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F92f7215bb699&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-edi-electronic-data-interchange-92f7215bb699&user=Samir+Saci&userId=bb0f26d52754&source=-----92f7215bb699---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F92f7215bb699&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-edi-electronic-data-interchange-92f7215bb699&source=-----92f7215bb699---------------------bookmark_footer-----------)[![](../Images/cc829661534b6c096215a9b4c8d9f775.png)](http://samirsaci.com)

（图片由作者提供）

电子数据交换（EDI）是一种标准化的在计算机系统之间自动传输数据的方法。

随着供应链变得越来越数字化，有效的数据交换已成为任何大型公司的必备条件。

![](../Images/4e823c81dbb760028b62fa8b93f87b0f.png)

使用EDI进行通信的供应链系统示例 — （图片由作者提供）

在复杂的供应商和分销商网络中，高效的数据通信至关重要。

> 作为分析专家，我们如何利用EDI技术支持组织的数字化转型？

它们确保了重要交易数据的顺畅流动，例如采购订单、发票、装运通知等。

[![](../Images/61bf97c11b9689e8764d42d5ba1f17ed.png)](http://samirsaci.com)

EDI 在采购管理中的应用 — （图像由作者提供）

在这篇文章中，我们将揭示**电子数据交换（EDI）**在推动供应链操作中的关键作用，以及它如何增强数据分析能力。

我们将使用 Python 脚本展示 EDI 消息如何在仓库操作中转化为实际行动。

💌 免费接收最新文章到您的邮箱：[新闻通讯](https://www.samirsaci.com/#/portal/signup)

📘 供应链分析的完整指南：[分析备忘单](https://bit.ly/supply-chain-cheat)

```py
Summary
I. EDI for Supply Chain Management
1\. A must-have for any large business
2\. More than 60 years of history
3\. EDI Standards
4\. Supply Chain Processes that use EDIs
II. Data Interchange & Operational Management
1\. Warehouse Operations Simulation Model
2\. Build a simulation model with Python
III. Why is Business Intelligence Significant?
1\. What is the environmental impact of our operations?
2\. Become a data-driven green organization
IV. What's Next?
1\. EDI for ESG Reporting and GreenWashing
2\. Conclusion
```

# 什么是供应链管理中的 EDI？

## 这是任何大型企业的必备工具。

电子数据交换（EDI）旨在促进高效、可靠和安全的数据交换。

几十年来，它已经深刻地成为任何大型现代企业的必备工具。

它促进了以标准化格式自动传输业务文档。

![](../Images/6655d0e8cdb18aea77e99cde55e7da8c.png)

应用示例 — （图像由作者提供）

这使得不同系统可以使用共同的语言进行通信。

+   一家公司希望向供应商发送包含项目信息、数量和预计交货日期的采购订单。

+   一个仓库希望通知承运人一个托盘已经准备好进行取件

+   一家商店向中央配送中心发送补货订单

## 超过 60 年的历史

发展于 1960 年代末期，EDI 最初用于传输运输和物流文档。

[![](../Images/01858b55bdfaa64560cb955f2ee658f4.png)](http://samirsaci.com)

电子数据交换的简要历史 — （图像由作者提供）

多年来，EDI 扩展了其能力，涵盖了各个行业，目前有超过 15 万家企业专注于供应链管理。

考虑到每天的巨大交易量，很难想象没有 EDI 的国际供应链如何运作。

## EDI 标准是什么？

EDI 基于不同地区多个行业使用的既定标准进行操作。

![](../Images/5b9c8601b85374c800e9ab572b0c9d20.png)

按行业和地理位置列出的一些标准 — （图像由作者提供）

然而，存在两种主要标准

+   **ANSI X12**：主要在北美使用

+   **EDIFACT**：由联合国创建并在国际上使用

这些标准定义了 EDI 消息中的字符串格式和包含的信息。

它们确保了在各种系统间数据解释的一致性。

![](../Images/406c549340e7a1b29cab4ed6ec6e543b.png)

一个采购订单转换为 EDI 消息的示例 — （图像由作者提供）

在上面的示例中，采购订单被转换为 EDI 消息进行传输。

+   订单由**采购团队**创建并由**供应商**接收

+   订单信息包括客户、供应商、交货地址和日期、发票地址以及关于订购物品的详细信息。

+   发票、交付和公司信息通过**使用ID**（公司ID、位置ID等）进行映射。

## 哪些供应链流程使用EDI？

随着供应链操作的复杂化，EDI消息成为关键事件（如）通信的支柱。

+   入库货物到达仓库。

+   正在进行入库的托盘。

+   正在执行的拣货订单。

+   一个已取消的出库发货。

> EDI消息使物流操作的运转得以维持。

为了说明这个想法，我们将使用Python来模拟创建和传输EDI消息以进行仓库操作管理。

# 数据交换与运营管理

## 仓库操作仿真模型的设计。

在我们的Python脚本中，我们将从EDI消息交换的角度复制多个仓储过程。

+   包含SKU和数量等详细信息的入库发货消息。

+   入库确认包括SKU和入库位置。

[![](../Images/f485b6929a5606e73663c84ba394e031.png)](http://samirsaci.com)

物流操作 — （作者提供的图片）

这些消息实现了**ERP**和**仓库管理系统（WMS）**的同步，提升了效率并减少了错误。

+   **消息1**：通知仓库团队通过WMS即将到来的入库发货（ERP -> WMS）。

+   **消息2**：仓库团队通知分销规划团队托盘已入库并准备好进行订单（WMS -> ERP）。

> 让我们使用Python构建我们自己的EDI消息仿真工具。

## **使用Python构建仿真模型。**

让我们使用EDI规范ANSI X12模拟这些消息交换。

1.  **入库：货物在仓库接收。**

    EDI消息（仓库发货订单 — 940）通知仓库即将到来的发货及其详细信息。

1.  **入库：收到货物后，货物存放在特定位置。**

    确认EDI消息（仓库库存转移收据通知 — 944）被返回到ERP，以确认入库。

1.  **拣货：根据订单，从存储位置挑选物品。**

    该EDI消息（仓库发货订单 — 940）可以指示仓库要挑选哪些物品。

1.  **出库：发运给客户。**

    EDI消息（仓库发货通知 — 945）被发送到ERP，以确认货物已被发运。

这是Python脚本的简化版本，

```py
# Author: Samir Saci
# Note: this script has been simplified for educational purposes.

class EDIMessage:
    def __init__(self, message_id):
        self.message_id = message_id
        self.content = ""

    def add_segment(self, segment):
        self.content += segment + "\n"

    def get_message(self):
        return f"ST*{self.message_id}*1\n{self.content}SE*2*1"

class Warehouse:
    def __init__(self):
        self.inventory = {}

    def receive_inbound(self, message):
        lines = message.content.split("\n")
        for line in lines:
            if line.startswith("N1"):
                _, _, sku, quantity, unit = line.split("*")
                self.inventory[sku] = self.inventory.get(sku, 0) + int(quantity)
        print("Received Inbound Shipment:\n", message.content)

    def process_putaway(self, sku):
        message = EDIMessage("944")
        if sku in self.inventory:
            message.add_segment(f"N1*ST*{sku}*{self.inventory[sku]}*units")
            print("Putaway Confirmation:\n", message.get_message())
            return message
        else:
            print("SKU not found in inventory.")

    def process_picking(self, message):
        lines = message.content.split("\n")
        for line in lines:
            if line.startswith("N1"):
                _, _, sku, quantity, unit = line.split("*")
                if self.inventory[sku] >= int(quantity):
                    self.inventory[sku] -= int(quantity)
                else:
                    print(f"Insufficient quantity for SKU {sku}")
        print("Processed Picking Order:\n", message.content)

    def process_outbound(self, picking_message):
        message = EDIMessage("945")
        lines = picking_message.content.split("\n")
        for line in lines:
            if line.startswith("N1"):
                _, _, sku, quantity, unit = line.split("*")
                message.add_segment(f"N1*ST*{sku}*{quantity}*boxes")
        print("Outbound Shipment Confirmation:\n", message.get_message())
        return message
```

**启动模型并创建您的入库订单。**

+   两种不同的SKU以纸箱形式收到。

+   {数量1：50箱，数量2：40箱}

```py
# Initiate the model
warehouse = Warehouse()

# Inbound Process
inbound_message = EDIMessage("940")
inbound_message.add_segment("N1*ST*SKU123*50*boxes")
inbound_message.add_segment("N1*ST*SKU124*40*boxes")
warehouse.receive_inbound(inbound_message)
print("Inventory of {}: {} boxes".format("SKU123",warehouse.inventory["SKU123"]))
print("Inventory of {}: {:,} boxes".format("SKU124",warehouse.inventory["SKU124"]))
```

输出如下，

```py
N1*ST*SKU123*50*boxes
N1*ST*SKU124*40*boxes

Inventory of SKU123: 50 boxes
Inventory of SKU124: 40 boxes
```

+   已传输的两个消息。

+   收到的物品清单已根据收到的数量进行了更新。

**入库确认**

```py
# Putaway Process
warehouse.process_putaway("SKU123")
```

+   该消息发送“SKU123”的入库确认。

```py
ST*944*1
N1*ST*SKU123*50*units
SE*2*1
```

**拣货订单和出库发货**

+   这两个SKU的拣货数量低于其库存水平。

```py
# Picking Process (Picking goods for an order)
picking_message = EDIMessage("940")
picking_message.add_segment("N1*ST*SKU123*10*boxes")
picking_message.add_segment("N1*ST*SKU124*5*boxes")
warehouse.process_picking(picking_message)
print("Inventory of {}: {} boxes".format("SKU123",warehouse.inventory["SKU123"]))
print("Inventory of {}: {:,} boxes".format("SKU124",warehouse.inventory["SKU124"]))

# Outbound Process (Sending out goods)
warehouse.process_outbound()
```

输出，

```py
N1*ST*SKU123*10*boxes
N1*ST*SKU124*5*boxes

Inventory of SKU123: 40 boxes
Inventory of SKU124: 35 boxes

ST*945*1
N1*ST*SKU123*10*boxes
N1*ST*SKU124*5*boxes
SE*2*1
```

+   对“SKU123”和“SKU124”进行的2个拣货订单，分别包含10和5个箱子

+   库存已更新

+   出库订单正在处理拣货的数量

> 我们如何确保传输顺畅？

## 错误检测与处理

我们引入这个模型并非仅为编码目的。

这个想法是理解如何创建各种检查以处理写入或读取消息时的错误。

EDI 也不免存在数据质量问题，比如

+   缺失数据、数据格式不正确、无效代码，…

+   逻辑不一致导致显著的操作中断

因此，实施强大的数据检查和验证对确保电子数据交换的准确性和可靠性至关重要。

**接收订单的错误处理示例**

```py
def receive_inbound(self, message):
    lines = message.content.split("\n")
    for line in lines:
        if line.startswith("N1"):
            try:
                _, _, sku, quantity, unit = line.split("*")

                # SKU or quantity is missing
                if not sku or not quantity:
                    print("Error: SKU or quantity missing.")
                    return

                # Quantity is an integer
                quantity = int(quantity)

               # Negative or zero quantities
                if quantity <= 0:
                    print("Error: Quantity must be positive.")
                    return

                self.inventory[sku] = self.inventory.get(sku, 0) + quantity
            except ValueError:
                print("Error: Incorrect data format.")
                return

    print("Received Inbound Shipment:\n", message.content)
```

这段代码是：

+   检查数量是否缺失或不符合整数格式

+   验证所有数量是否为正数

+   如有必要，提出错误

> 下一步是什么？

使用 Python，你可以支持你的基础设施团队自动化测试，以开发新的 EDI 消息。

# EDI 对数据分析的强大作用是什么？

通过连接多样的计算机系统，EDI 支持日常操作，并成为数据分析的真正宝库。

每个 EDI 交易都携带有价值的信息，

+   时间戳、位置和原因代码提供了对你的货物的可追溯性，并衡量流程的表现

+   可用于建模物料、财务和信息流的数量、定价和项目信息

> 生成交易数据以监控和改进供应链网络。

![](../Images/5fa751498e96dd7cc95a13cc49abafb6.png)

什么是供应链分析？ — （作者提供的图片）

这个宝贵的数据来源可以用来

+   描述过去的事件：描述性分析

+   分析缺陷和事件：诊断性分析

+   预测未来事件：预测性分析

+   设计**最佳流程和决策**：规范性分析

让我们深入探讨每种分析类型，以了解它如何依赖于良好的 EDI 基础设施。

## *描述性和诊断性分析*

描述性分析是关于理解过去发生了什么。

通过正确设置 EDI 消息，我们可以将历史交易数据映射到以获取过去表现的见解。

[![](../Images/92953ea26b76a25f7d3c20cc5f5bf767.png)](https://towardsdatascience.com/automated-supply-chain-control-tower-with-python-17dbf93a18d0)

通过时间戳跟踪的分销过程示例 — （作者提供的图片）

例如，EDI 消息可以在你的分销链的每个阶段更新状态。

1.  每个事件都带有时间戳（从订单创建到商店交付）

1.  实际时间戳可以与预期时间戳进行比较

1.  然后可以分析延迟以找到根本原因

[![](../Images/aadf26aad62e7764a29398c9705bb9b6.png)](https://towardsdatascience.com/automated-supply-chain-control-tower-with-python-17dbf93a18d0)

每个流程的预期时间与实际时间 — （作者提供的图片）

+   使用与运营团队商定的目标交货时间计算预期时间

+   ERP、WMS、货运代理系统和商店管理系统都使用EDI通信时间戳

您可以收集和处理这些时间戳，创建自动化报告，跟踪分销链路上的货物运输。

**💡 获取更多详细信息**

[](/automated-supply-chain-control-tower-with-python-17dbf93a18d0?source=post_page-----92f7215bb699--------------------------------) [## 什么是供应链控制塔？

### 使用Python优化您的供应链网络，采用自动化解决方案跟踪您的货物并评估…

towardsdatascience.com](/automated-supply-chain-control-tower-with-python-17dbf93a18d0?source=post_page-----92f7215bb699--------------------------------)

> 如果我们想要模拟物流链中的事件或故障会怎样？

## *供应链管理中的数字孪生*

这些计算机模型代表各种供应链组件，包括配送中心、运输网络和制造设施。

![](../Images/f892e1177683256259a9ac8c6da0f5cd.png)

使用Python创建简单的数字孪生 — (作者提供的图片)

EDI交易可以帮助您提供保持数字孪生更新的实时数据。

![](../Images/c39a6c50ae1e13e71badca5a53c8ef62.png)

使用Python创建的简单数字孪生的示例 — (作者提供的图片)

假设您已经建立了一个包括以下内容的简单数字孪生

+   模拟运输、商店、仓库和工厂运营的模型

+   沿着链条复制信息和货物流动的连接

> 我们如何利用这些数据？

您可以将您的EDI流程与以下内容连接：

+   仓库模型用于估算接收到EDI消息中订单批次的拣货时间

+   工厂模型将订单数量与实际生产能力进行比较

这是一个很好的工具，可以使用通过EDI通信的真实订单来模拟和分析不同的场景，而不会影响实际运营。

**💡 获取更多详细信息**

[](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----92f7215bb699--------------------------------) [## 什么是供应链数字孪生？

### 使用Python发现数字孪生：建模供应链网络，增强决策能力和优化运营。

towardsdatascience.com](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----92f7215bb699--------------------------------) [](/what-is-supply-chain-analytics-42f1b2df4a2?source=post_page-----92f7215bb699--------------------------------) [## 什么是供应链分析？

### 使用数据分析提高运营效率，通过数据驱动的诊断和决策在战略和…

towardsdatascience.com](/what-is-supply-chain-analytics-42f1b2df4a2?source=post_page-----92f7215bb699--------------------------------)

# 结论

理解电子数据交换（EDI）在供应链管理中的作用，让我们了解数据传输对现代商业运营的重要性。

这一关键技术为各种计算机系统之间的高效通信提供了基础。

> 报告的影响是什么？

## 对可持续性报告的影响：ESG与绿色洗涤

环境、社会和治理（ESG）报告是公司用来披露其治理结构、社会影响和环境足迹的一种方法。

[![](../Images/d21691450fac774ecd1938117d9f2cff.png)](https://towardsdatascience.com/what-is-esg-reporting-d610535eed9c)

ESG支柱展示 — (图像来源：作者)

这一非财务报告已成为公司的战略方面，因为它可能影响消费者的感知和投资的可达性。

> 电子数据交换如何确保数据一致性并支持审计？

ESG报告可能会因为缺乏标准化和确保数据准确性的困难而变得具有问题。

> 如果你的ESG报告输入了错误的数据，会发生什么？

审计**可能成为任何希望正式报告这一分数的公司的风险**。

![](../Images/f9a901b9ee2f6a11be9c482eab6fd3cb.png)

ESG报告所需的分析能力 — (图像来源：作者)

高级商业智能解决方案可以支持数据处理自动化；EDI能力可以帮助确保数据的可追溯性。

> 这可以支持对抗绿色洗涤。

绿色洗涤是通过对产品环境效益做出误导性声明来传达虚假的可持续性形象的做法。

![](../Images/665de0bd4a1ec2ed1725ac3249e510c6.png)

绿色洗涤的五大罪 — (图像来源：作者)

随着公众意识的提高，公司必须更加努力以确保计算的准确性。

这依赖于EDI技术支持的交易数据的适当收集、传输和处理。

**关于ESG报告和绿色洗涤的更多信息，**

[## 什么是ESG报告？](/what-is-esg-reporting-d610535eed9c?source=post_page-----92f7215bb699--------------------------------)

### 利用数据分析进行全面有效的环境、社会和治理报告

towardsdatascience.com](/what-is-esg-reporting-d610535eed9c?source=post_page-----92f7215bb699--------------------------------) [](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----92f7215bb699--------------------------------) [## 什么是绿色洗涤？以及如何利用分析来检测它？

### 探索数据分析如何帮助我们检测和防止绿色洗涤，并推动真正的可持续性。

towardsdatascience.com](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----92f7215bb699--------------------------------)

# 参考文献

+   什么是供应链数字双胞胎？，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----92f7215bb699--------------------------------)，[数据科学前沿](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75)

+   什么是供应链分析？，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----92f7215bb699--------------------------------)，[数据科学前沿](/what-is-supply-chain-analytics-42f1b2df4a2)
