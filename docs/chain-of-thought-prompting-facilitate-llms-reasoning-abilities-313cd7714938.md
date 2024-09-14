# 思维链提示促进了 LLMs 的推理能力

> 原文：[`towardsdatascience.com/chain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938`](https://towardsdatascience.com/chain-of-thought-prompting-facilitate-llms-reasoning-abilities-313cd7714938)

## 通过示例展示

[](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)[](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----313cd7714938--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----313cd7714938--------------------------------)·阅读时间 6 分钟·2023 年 6 月 12 日

--

![](img/002f67384a5ad5ec102fb526b86c9eea.png)

图片由[Juan Rumimpunu](https://unsplash.com/@earbiscuits?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/nLXOatvTaLo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大型语言模型（LLMs）被证明在解决各种任务方面非常高效，从总结文档到用不同编程语言编写代码。

此外，新发布的模型如 ChatGPT 和 GPT-4 使得 LLMs 的表现更加出色，为基于 LLM 的应用解锁了更多机会。

尽管大型语言模型（LLMs）具有非凡的技能，但它们有时未能展示非常简单的推理能力，无法解决一个四年级学生都能轻松处理的问题。

在这一领域已经进行了大量研究，旨在理解为何 LLMs 无法执行此类任务并使其表现更好。

关注这个特定问题的一个研究是[思维链提示](https://arxiv.org/abs/2201.11903)，由谷歌研究团队介绍。

> 思维链提示
> 
> 一种结构为{输入、思维链、输出}的提示技术，其中思维链是一系列中间自然语言推理步骤。

模型会接收到一些包含输入和输出的示例（少样本学习），然后被要求解决一个涉及多步骤或算术推理任务的问题。

[论文](https://arxiv.org/abs/2201.11903)的主要结论：

+   思维链提示优于标准提示

+   思维链提示与标准提示之间的区别在于较大的模型上更为明显。思维链提示的性能提升与模型的参数数量成正比。

思路链提示技术就是逐步解决问题。每一步都基于逻辑推理。这里是一个示例：

> 问题：约翰有 2 栋房子。每栋房子有 3 间卧室，每间卧室有 2 个窗户。每栋房子有 1 个厨房，厨房有 2 个窗户。此外，每栋房子还有 5 个窗户不在卧室或厨房中。
> 
> 约翰的房子里有多少个窗户？
> 
> 答案：每栋房子有 3 间卧室，每间卧室有 2 个窗户，共计 6 个卧室窗户。每栋房子有 1 个厨房，厨房有 2 个窗户，共计 2 个厨房窗户。还有 5 个窗户不在厨房或卧室中。因此，每栋房子有 6 + 2 + 5 = 13 个窗户。由于有 2 栋房子，所以约翰的房子里总共有 2 x 13 = 26 个窗户。

你可以遵循不同的步骤来获得正确的答案。对于这类问题，几乎总有多条路径可以到达答案。

在标准提示中，我们通常会向语言模型提出上述问题，并期望它给出答案。让我们试试：

```py
# model: text-davinci-003
prompt = """
John has 2 houses. Each house has 3 bedrooms and there are 2 windows in each bedroom.
Each house has 1 kitchen with 2 windows. Also, each house has 5 windows that are not in the bedrooms or kitchens.
How many windows are there in John's houses?
"""

# model response
"There are a total of 32 windows in John's houses."
```

错误的答案！

我们不知道模型如何得到这个答案，这带来了思路链提示的另一个优点。模型以逐步回答的方式回应，这使得调试过程更容易。我们可以很容易地发现问题所在。

通过思路链提示技术，我们在提示中添加了一些问题及其答案，以进行少量示例提示。这些答案以逐步解决方案的形式出现（即展示思路链）。

这里是更新后的提示和模型的回应：

```py
# model: text-davinci-003
prompt = """
Q: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done, there will be 21 trees. How many trees did the grove workers plant today?
A: There are 15 trees originally. Then there were 21 trees after some more were planted. So there must have been 21 - 15 = 6\. The answer is 6.

Q: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot? 
A: There are originally 3 cars. 2 more cars arrive. 3 + 2 = 5\. The answer is 5.

Q: Leah had 32 chocolates and her sister had 42\. If they ate 35, how many pieces do they have left in total? 
A: Originally, Leah had 32 chocolates. Her sister had 42\. So in total they had 32 + 42 = 74\. After eating 35, they
had 74 - 35 = 39\. The answer is 39.

Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?
A: Jason started with 20 lollipops. Then he had 12 after giving some to Denny. So he gave Denny 20 - 12 = 8\. The answer is 8.

Q: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does he have now?
A: Shawn started with 5 toys. If he got 2 toys each from his mom and dad, then that is 4 more toys. 5 + 4 = 9\. The answer is 9.

Q: John has 2 houses. Each house has 3 bedrooms and there are 2 windows in each bedroom.
Each house has 1 kitchen with 2 windows. Also, each house has 5 windows that are not in the bedrooms or kitchens.
How many windows are there in John's houses?
A: 
"""

# model response
"John has 2 houses. Each house has 3 bedrooms with 2 windows each, 
1 kitchen with 2 windows, and 5 windows that are not in the bedrooms or 
kitchens. So for each house, there are 3 x 2 + 2 + 5 = 13 windows. 
In total, there are 2 x 13 = 26 windows. The answer is 26."
```

正确的答案！

回应是对模型思考过程的逐步解释（思路链），就像提示中展示的示例一样。

让我们再看一个例子。这里是标准提示版本：

```py
# model: text-davinci-003
prompt = """
We have a big meeting in 10 days, which is 5 days before our product 
launch on 08/24/2022\. What is the date today in MM/DD/YYYY?
"""

# model response
"08/14/2022"
```

错误的答案！

让我们尝试用思路链提示回答相同的问题，提供一些输入输出示例（少量示例学习）：

```py
# model: text-davinci-003
prompt = """
Q: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done, there will be 21 trees. How many trees did the grove workers plant today?
A: There are 15 trees originally. Then there were 21 trees after some more were planted. So there must have been 21 - 15 = 6\. The answer is 6.

Q: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot? 
A: There are originally 3 cars. 2 more cars arrive. 3 + 2 = 5\. The answer is 5.

Q: 2015 is coming in 36 hours. What is the date one week from today in MM/DD/YYYY?
A: If 2015 is coming in 36 hours, then it is coming in 2 days. 2 days before 01/01/2015 is 12/30/2014, 
so today is 12/30/2014\. So one week from today will be 01/05/2015\. So the answer is 01/05/2015.

Q: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does he have now?
A: Shawn started with 5 toys. If he got 2 toys each from his mom and dad, then that is 4 more toys. 5 + 4 = 9\. The answer is 9.

Q: We have a big meeting in 10 days, which is 5 days before our product launch on 08/24/2022.
What is the date today in MM/DD/YYYY?
A: 
"""

# model response
"If the product launch is on 08/24/2022, then 10 days before that is '
 '08/14/2022\. 5 days before that is 08/09/2022\. So today is 08/09/2022\. The '
 'answer is 08/09/2022."
```

正确的答案！

## 结束语

思路链提示明显提高了语言模型在某些任务上的能力。如果你想了解更多关于他们进行的实验，建议阅读整篇[论文](https://arxiv.org/abs/2201.11903)。

需要注意的是，正如论文中提到的，思路链提示的好处只有在应用于大约 1000 亿参数的模型时才会显现，并且对较小模型的性能提升并不显著。

实验结果得出的结论是，较小的模型会产生流畅但不合逻辑的思路链，这导致其表现比标准提示更差。

## 参考文献

Wei, Jason, 等（2022）。思路链提示引发大型语言模型的推理。ArXiv。[`arxiv.org/abs/2201.11903`](https://arxiv.org/abs/2201.11903)
