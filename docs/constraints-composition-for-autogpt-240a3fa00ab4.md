# 为 AI 代理启用市场：发现和匹配

> 原文：[`towardsdatascience.com/constraints-composition-for-autogpt-240a3fa00ab4`](https://towardsdatascience.com/constraints-composition-for-autogpt-240a3fa00ab4)

## 发现能够执行给定用户任务的 AI 代理

[](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)![Debmalya Biswas](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------) [Debmalya Biswas](https://debmalyabiswas.medium.com/?source=post_page-----240a3fa00ab4--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----240a3fa00ab4--------------------------------) ·8 分钟阅读·2023 年 5 月 2 日

--

![](img/31436955cfbacc8502f3d448e3a5df24.png)

图：Soma Biswas 探索未知 ([Flickr](https://www.flickr.com/photos/somabiswas/52006785817/in/dateposted/)，经许可转载)

# 1. 介绍

关于 ChatGPT 的讨论现在已经演变为 AutoGPT。虽然 ChatGPT [1] 主要是一个可以生成文本回应的聊天机器人，但 AutoGPT 是一个更强大的 AI 代理，可以执行复杂任务，例如，进行销售、规划旅行、预订航班、聘请承包商做房屋工作、订购披萨。

AutoGPT 继承了关于自主代理的长期研究，特别是以目标为导向的代理 [2, 3]。

> 针对用户任务，AutoGPT 旨在识别（组合）一个（或多个）能够执行给定任务的代理。

解决此类复杂任务的高级方法包括：(a) 将给定的复杂任务分解为（一个层级或工作流的）简单任务，然后 (b) 组合 [4] 能够执行简单（或更简单）任务的代理。

这可以通过**动态**或**静态**方式实现。在动态方法中，给定一个复杂的用户任务，系统根据运行时可用代理的能力来制定满足请求的计划。在静态方法中，给定一组代理，组合代理在设计时手动定义，结合其能力。

> 本文的主要焦点是代理的发现方面，即识别能够执行给定任务的代理。这意味着存在一个代理市场/注册表，具备明确定义的代理能力和约束。

我们在第**2**节中概述了一个约束模型，以捕获/指定 AI 代理提供的服务的约束。我们之前在 Web 服务——通用描述、发现与集成（UDDI）注册表和 API 市场中看到过类似的努力。然而，用 XML 或 JSON 捕获的描述过于静态，缺乏在发现过程中进行能力/任务要求“协商”的必要语义信息。

***例如***，我们考虑一个可以在线预订的房屋粉刷代理 *C*（通过信用卡）。鉴于此，用户需要有效的信用卡这一事实就是一个约束，而用户的房屋将在特定时间范围内完成粉刷则是其能力。此外，我们还需要考虑 *C* 在实际执行阶段的任何约束，例如，*C* 只能在工作日提供服务（而不是在周末）。如果用户指定的任务需要在周末完成工作，那么上述限制可能会成为一个问题。一般来说，约束指的是启动执行所需满足的条件，而能力则反映了执行终止后的预期结果。

# 2\. 代理约束模型

## 2.1 约束规范

一个代理 P 提供了一组服务 *{S_1, S_2, . . . , S_n}*。每项服务 S 又有一组相关的约束 *{C_1, C_2, . . . , C_m}*。这些约束作为**逻辑谓词**被指定在其代理发布的相应服务的服务描述中。对于服务 S 的每个约束 C，约束值可以是

+   单个值（例如，服务的价格），

+   值的列表（例如，航空公司服务的目的地列表），或

+   值的范围（例如，最小值和最大值）。

约束值被指定为包含适用值的事实。更准确地说，由 P 提供的服务 S 具有约束 *{C_1, C_2, . . . , C_m}* 的规范如下：

*S(Y, X_1, X_2, . . . , X_m):-

C_1(Y, X_1),

C_2(Y, list_2), member(X_2, list_2),

. . .,

C_m(Y, minVal_m, maxVal_m), X_m ≥ minVal_m, X_m ≤ maxVal_m.

C_1(P, value(C_1)).

C_2(P, list(C_2)).

. . .

C_m(P, min(C_m), max(C_m)).*

**例如**，航空公司 ABC 仅在部分航班（前往特定目的地）上提供素食餐和残疾人设施的事实可以表示如下：

*flight(Airlines, X, Y):-

veg_meals(Airlines, Destination_List), member(X, Destination_List),

hnd_facilities(Airlines, Destination_List), member(Y, Destination_List).

veg_meals(‘ABC’, [‘巴黎’, ‘雷恩’]).

hnd_facilities(‘ABC’, [‘巴黎’, ‘格勒诺布尔’]).*

在上述片段中，‘member(X,Y)’是一个系统定义的谓词，当 X 是集合 Y 的元素时成立。现在，让我们考虑“相关”的约束或约束之间存在关系的情景。默认情况下，上述示例假设约束之间存在 AND 关系（veg_meals 和 hnd_facilities 谓词都必须满足）。文献中研究的逻辑程序组合操作符包括：AND、OR、ONE-OR-MORE、ZERO-OR-MORE 以及以上操作符的任何嵌套。

> 我们仅考虑 AND、OR 及其任何嵌套层级，以保持框架简单（ONE-OR-MORE 和 ZERO-OR-MORE 可以用 OR 来表示）。

约束之间的 OR 关系示例如下：航空公司 ABC 仅在乘客持有商务舱票或是其常旅客计划的会员时，允许在中途停留时使用机场贵宾室。上述情景可以表示为：

*lounge_access(Airlines, X):-

ticket_type(‘ABC’, X, ’Business’).

lounge_access(Airlines, Y):-

frequent_flier(Airlines, FF_List), member(Y, FF_List).*

我们简要考虑以下可能与约束一起指定的限定词：

+   *有效期*：约束有效的时间段。有效期限定词可用于优化匹配。基本上，不需要对每一个请求重复整个匹配过程。一旦找到适合的代理/服务提供者，它将保持有效，直到其至少一个“相关”约束的有效期过期。

+   *问责性*：代理人提供特定服务的承诺（承诺级别）。例如，代理人可能愿意在任何情况下承担其广告服务的责任；或者代理人能够提供服务，但不愿意承担出现问题时的责任。

+   *非功能性*：与非功能性方面相关的限定词，例如事务、安全、监控（性能）等。

## 2.2 约束组合

复合代理人聚合了不同代理人提供的服务，并为其提供唯一的接口（在服务能力本身没有任何修改的情况下）。换句话说，复合代理人充当了聚合服务集的中介。

聚合的服务可能具有不同的能力，或具有相同的能力但约束不同（如下例所示）。

**场景**：代理人 XYZ 整合了航空公司 ABC 和 DEF 提供的航班服务。

航空公司 ABC：

*flight(Airlines, X):-

hnd_facilities(Airlines, Destination_List), member(X, Destination_List).

hnd_facilities(‘ABC’, [‘马赛’, ‘格勒诺布尔’]).*

航空公司 DEF：

*flight(Airlines, X):-

hnd_facilities(Airlines, Destination_List), member(X, Destination_List).

hnd_facilities(‘DEF’, [‘雷恩’, ‘巴黎’]).*

复合代理人 XYZ：

*flight(Airlines, X):-

hnd_facilities(Airlines, Destination_List), member(X, Destination_List),

航空公司:= ‘XYZ’。

hnd_facilities(‘ABC’, [‘马赛’, ‘格勒诺布尔’]).

flight(Airlines, X):-

hnd_facilities(Airlines, Destination_List), member(X, Destination_List),

航空公司:= ‘XYZ’。

hnd_facilities(‘DEF’, [‘雷恩’, ‘巴黎’]).*

在上述代码片段中添加语句 Airlines:= ‘XYZ’ 确保返回给外部世界的绑定是代理 XYZ，而代理 XYZ 在内部将实际处理委托给代理 ABC/DEF。上述示例还突出了另一个点。

> 组合可能会导致约束的放松，例如，复合代理 XYZ 可以提供更多目的地（马赛、格勒诺布尔、雷恩和巴黎）的无障碍设施航班，而这些目的地比单独的组件代理 ABC（马赛、格勒诺布尔）/DEF（雷恩、巴黎）提供的更多。

# 3\. 代理服务发现

## **3.1 配对匹配**

对于用户任务 G，匹配包括找到能够执行 G 的（子）任务的代理。G 的（子）任务可能有自己的约束。鉴于此，

> 所需的 G 配对匹配可以通过逻辑程序执行引擎来实现，将 G 的约束作为目标提出，并与相应代理的服务约束逻辑程序进行比对。

逻辑程序执行引擎不仅指定目标是否可以满足，还指定所有可能的绑定，对于目标的任何无界变量。如果存在多个可能的绑定（多个能够执行相同任务的代理），可以使用用户定义的偏好标准对这些代理进行排序，或者直接咨询用户以选择其中最优的一个。

## 3.2 近似匹配

在本节中，我们考虑匹配不成功的情况，即不存在能够执行给定任务 G 的代理集。

鉴于此，允许在选择代理时存在一定的不一致性是合理的。请注意，现实生活中的系统通常允许不一致性，例如航班预订系统允许航班超售，但仅限于一定数量的座位。因此，关键在于**有限的不一致性**。

基本上，对于一个目标 *G = {T_1, T_2, . . . , T_n}*，只要累计的不一致性在指定限制范围内，选择的代理在某一任务 T_i 上不必完全匹配。请注意，代理引起的不一致性也可能对另一个任务 T_j 引起的不一致性产生对抗作用（减少）。

对于给定的目标 *G = {T_1, T_2, . . . , T_n}*，可以通过以下方式实现近似匹配：

1.  *确定通用约束*：如果 *G* 中的多个任务都基于 *C* 有约束，则 *C* 是与 *G* 相关的通用约束。例如，如果任务 *T_1* 和 *T_2* 需要分别在 3 天和 4 天内完成，则它们有一个共同的时间约束。研究表明，现实生活中大多数约束都基于以下约束：位置、价格、数量或时间。

1.  对于每个通用约束 *C*，定义一个临时变量 *q_C*（用于跟踪相对于 *C* 的不一致性）。最初，*q_C = 0*。

1.  对于每个任务 *T_i* 和一个通用约束 *C*：令 *vC_i* 表示 *T_i* 相对于 *C* 的约束值。例如，*vTime_1 = 3* 表示 *T_1* 的完成时间约束值。将 *T_i* 的 *C* 约束从 *G* 的约束规范中删除。

1.  基于减少后的目标（删除了上述步骤中的通用约束谓词）执行配对。

1.  如果配对成功：[请注意，如果在减少的目标下配对不成功，那么在原始目标下也一定会不成功。] 令 *P(T_i)* 表示选择执行 *T_i* 的代理。对于每个删除的 *T_i* 的通用约束 *C*（步骤 3），获取 *P(T_i)* 的最佳约束值 *vBestC_i*，并计算 *q_C = q_C + (vC_i − vBestC_i)*。

    例如，假设 *P(T_1)* 和 *P(T_2)* 分别需要至少 5 天和 1 天来完成他们的工作。基于此，*q_t = 0 + (vC_1 − vBestC_1) + (vC_2 − vBestC_2) = (3 − 5) + (4 − 1) = 1*。

1.  配对结果有效当且仅当对于每个通用约束 *C*，*q_C > 0*。例如，*P(T_1)* 和 *P(T_2)* 是任务 *T_1* 和 *T_2* 的有效匹配，因为 *q_t > 0*。

请注意，如果没有（近似）扩展，配对将不会成功，因为 *P(T_1)* 违反了 *T_1* 的完成时间约束（5 天而非 3 天）。为了简单起见，我们在上述算法中仅考虑了基于数值值的约束。

# 结论

在这篇文章中，我们集中于自主 AI 代理的发现方面。执行复杂任务的先决条件是一个代理登记簿，指定其服务能力和约束。我们概述了一种基于约束的模型来指定代理服务。

我们展示了如何以与其组成代理的约束一致的方式推导和描述复合代理的约束。最后，我们讨论了近似配对，并展示了如何利用有界不一致性的概念更高效地发现代理。

# 参考文献

1.  D. Biswas. *ChatGPT 及其企业应用*。 ([Data Driven Investor](https://medium.datadriveninvestor.com/will-chatgpt-disrupt-enterprise-ai-7b83b7591c1e))

1.  A. Bordes 等人 *目标导向的聊天机器人：学习端到端目标导向对话，2016* ([文章](https://arxiv.org/abs/1605.07683))

1.  E. Ricciardelli, D. Biswas. *基于强化学习的自我提升聊天机器人*。见：第四届多学科强化学习与决策制定会议，2019 (Towards Data Science)

1.  D. Biswas. *组合 AI：企业 AI 的未来*。2021 (Towards Data Science)

1.  F. Casati 等. *eFlow 中的自适应和动态服务组合*。HP 技术报告，HPL-2000–39，2000 年 3 月。

1.  J.S. Park 等. *生成型代理：人类行为的互动模拟*，2023 ([article](https://arxiv.org/abs/2304.03442))

**ChatGPT/AutoGPT**系列中的相关文章：

+   D. Biswas. *ChatGPT 及其对企业 AI 的影响*。 ([Data Driven Investor](https://medium.datadriveninvestor.com/will-chatgpt-disrupt-enterprise-ai-7b83b7591c1e))

+   D. Biswas. *ChatGPT 的隐私风险*。 ([Data Driven Investor](https://medium.datadriveninvestor.com/privacy-risks-of-chatgpt-3f838320954a))

+   D. Biswas. *用企业数据对大型语言模型（LLMs）进行情境化*。 ([Data Driven Investor](https://medium.datadriveninvestor.com/contextualizing-large-language-models-llms-with-enterprise-data-419e252fcbb7))
