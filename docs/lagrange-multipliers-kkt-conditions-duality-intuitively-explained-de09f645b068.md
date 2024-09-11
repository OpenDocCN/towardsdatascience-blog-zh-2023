# 拉格朗日乘子、KKT 条件和对偶性——直观解释

> 原文：[https://towardsdatascience.com/lagrange-multipliers-kkt-conditions-duality-intuitively-explained-de09f645b068?source=collection_archive---------3-----------------------#2023-11-03](https://towardsdatascience.com/lagrange-multipliers-kkt-conditions-duality-intuitively-explained-de09f645b068?source=collection_archive---------3-----------------------#2023-11-03)

## 理解 SVM、正则化、PCA 以及许多其他机器学习概念的关键

[](https://essamwissam.medium.com/?source=post_page-----de09f645b068--------------------------------)[![Essam Wisam](../Images/6320ce88ba2e5d56d70ce3e0f97ceb1d.png)](https://essamwissam.medium.com/?source=post_page-----de09f645b068--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de09f645b068--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----de09f645b068--------------------------------) [Essam Wisam](https://essamwissam.medium.com/?source=post_page-----de09f645b068--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fccb82b9f3b87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flagrange-multipliers-kkt-conditions-duality-intuitively-explained-de09f645b068&user=Essam+Wisam&userId=ccb82b9f3b87&source=post_page-ccb82b9f3b87----de09f645b068---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de09f645b068--------------------------------) ·13 min read·Nov 3, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fde09f645b068&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flagrange-multipliers-kkt-conditions-duality-intuitively-explained-de09f645b068&user=Essam+Wisam&userId=ccb82b9f3b87&source=-----de09f645b068---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fde09f645b068&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flagrange-multipliers-kkt-conditions-duality-intuitively-explained-de09f645b068&source=-----de09f645b068---------------------bookmark_footer-----------)

在这个故事中，我们将深入探索数学优化中的三个相关概念。这些概念曾让我花费大量时间和精力才完全掌握，因此我旨在以直观的方式向所有读者展示它们。我们的旅程将从对无约束优化的回顾开始，随后考虑有约束优化，我们将利用拉格朗日乘子和 KKT 条件。我们还将深入探讨这些概念之间的相互作用及其与对偶性概念的联系。

因此，在本故事结束时，你将了解如何解决约束和无约束优化问题，并能够直观地推导出这些方法为何有效。

![](../Images/85a9dfa76b0f8b0c73ad39b85daf60f0.png)

照片由 [Filip Mroz](https://unsplash.com/@mroz?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 无约束优化

![](../Images/71be8bc42c3c9753a19ba5f9874dc427.png)

由 Cdang 绘制的多变量函数图，来源于 [Wikimedia](https://commons.wikimedia.org/wiki/File:Surface3D_sinFoisSin_python_matplotlib.svg) CC BY-SA 4.0。

在无约束优化中，我们给定一个多变量函数 *f(u)*，并希望找到向量 *u** 的值，使得函数 *f(u*)* 的值最优（最大值或最小值）。

一般来说，函数可能有多个极大值和极小值，如上所示。在经典机器学习中以及本故事中，我们主要关注的是凸函数（这些函数也足够光滑）。[凸](https://en.wikipedia.org/wiki/Convex_function) 函数意味着该函数至多有一个最优值（当函数是损失函数时，这个最优值是一个最小值），如下所示：

![](../Images/694e1583a1664b47a985863d7cd250ea.png)

由 [Andrebis](https://commons.wikimedia.org/wiki/User:Andrebis) 绘制的 3D 表面图，来源于 [Wikipedia](https://en.wikipedia.org/wiki/Lagrange_multiplier#/media/File:As_wiki_lgm_parab.svg) CC BY-SA 3.0。

处理凸函数要容易得多，因为否则很难判断找到的最小值是否是所有值中的最低点（即全局最小值），而不仅仅是某个局部最小值。一般来说，即使存在一个最小值，也可能有多个点满足它（例如，如果函数是平的），我们将假设这种情况不会发生以简化解释；假设它发生也不会改变我们得出的任何结论。

对给定的多变量函数 *f(u)* 进行无约束优化可以通过解 *∇ᵤf(u) = 0* 来实现。如果 *f(u)* 是一个 *n* 变量的函数 *(u***₁***, u*₂*,…,u*ₙ*)*，那么这就是一个 *n* 方程的系统：

![](../Images/7ce5c143b36a8b273112ca1909d33633.png)

解出这些方程后，会得到最优解 *u*=(u****₁***,u**₂*,…,u**ₙ*)*，即最优值（例如最小值）所在的位置。

![](../Images/8aab1194bce59f3a1becb388975da237.png)

由 Mike Run 绘制的表面上的切平面，来源于 [Wikimedia](https://commons.wikimedia.org/wiki/File:Law-of-reflection-for-curved-surfaces.svg) CC BY-SA 4.0。

要理解这点，请回忆一下：

+   在任意点 *u* 处，切平面的法向量形式为 *(∂f(u)/∂u₁, ∂f(u)/∂u*₂, …, *∂f(u)/∂u*ₙ, *f(u))*

+   在任何最小值或最大值处，切平面都是水平的（视觉上明显）

+   因此，每当 *∇ᵤf(u) = 0* 成立时，该点处必定存在水平切平面，因此，它一定是我们寻找的最小值。

另一种即将有用的合理化方法是观察梯度指向最大增加的方向（以及与最大减少的方向相反）。因此，当 *∇ᵤf(u) = 0* 时，必须要么无法（没有方向能）从该点增加函数（即，处于最大值），要么从该点减少函数（即，处于最小值）。

**无约束优化总结**

**给定：** *f(u)*

**目标：** *u，其中 f(u) 最小*

**方法：** 求解 *∇ᵤf(u)* = 0，因为在最小值处成立

![](../Images/501d8eb69a61133d8c17584aa8e7b288.png)

图片来源于 [Jorge Reyna](https://unsplash.com/@jorgereyna?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 受限优化

在这种类型的优化中，我们给定形式为 *g(u)=0* 或 g(u)≤0 的等式约束（否则我们可以通过重新排列项或乘以负数将其转化为这种形式），并且我们希望**仅在满足约束的所有点上进行优化**。

我们通常假设等式约束 *g(u)=0* 是仿射的（线性的一般化），而不等式约束 g(u)≤0 涉及凸函数，以便整个优化问题是 [凸的](https://en.wikipedia.org/wiki/Convex_optimization)（否则，仅 *f(u)* 是凸的可能不足以保证唯一最优值）。

## **带有等式约束的优化**

在此问题中，我们给定一个多变量函数 *f(u)* 和一个约束 *g(u)=0*，我们希望找到点 *u**，使得 *g(u*)=0* 且 *f(u*)* 最小（即，满足约束的情况下的最低点）。

![](../Images/5da79d58c0d542dd0b841c58611ef0f3.png)

受限优化由 [Jacobmelgrad](https://commons.wikimedia.org/w/index.php?title=User%3AJacobmelgaard&action=edit&redlink=1) 在 [维基百科](https://en.wikipedia.org/wiki/Lagrange_multiplier#/media/File:Lagrange_very_simple.svg) CC BY-SA 3.0.

例如，在所示的示例中，目标函数为 *f(u₁,u*₂*) = u₁+ u*₂（3D平面），约束为 *u*²*₁+ u*²₂=1（2D圆）。目标是找到点（*u₁, u*₂），使得在满足 *u*²*₁+ u*²₂=1 的情况下，该点对应于平面上的最低点（即，圆在平面上的投影最低点）。

解决这种类型的受限优化问题的一种方法是使用拉格朗日乘子法。简单来说，拉格朗日乘子定理表明，任何优化问题的解 *u** 形式为：

**最小化** *f(u)* **使得** *g(u)=0*

必须满足方程*∇ᵤL(u*,λ)=0*，对于某些 *λ*∈R *（并且显然，*g(u*)=0）*，其中 *L* 是拉格朗日函数，定义为 *L(u,λ)=f(u)+λg(u)*。这假设 *∇ᵤg(u*)≠0*。

由此可得，我们可以按照以下方法求解带有等式约束的受限优化问题（假设 *∇ᵤg(u*)≠0*）：

1.  写出拉格朗日函数 *L(u,λ)=f(u)+λg(u)*。

1.  解 *∇ᵤL(u,λ)* = 0（*n* 个方程）和 *g(u)=0* 得到 *n+1* 个未知数 *u****₁***,u**₂*,…,u**ₙ,*λ*

1.  解是 *u*=(u****₁***,u**₂*,…,u**ₙ*)*

*λ* 被称为拉格朗日乘子。我们只需要找到它，因为它是系统的一部分，用于得出解 *u*。

你可以在[这里](https://en.wikipedia.org/wiki/Lagrange_multiplier#Example_1)研究与上图对应的例子。在这个例子中，问题不是凸的，解决方案应该会得出任何存在的最小值或最大值。注意，上述步骤（1）和（2）等同于对 *L(u,λ)=f(u)+λg(u)* 进行无约束优化。也就是说，设定：

*∇ L(u,λ)* =*(∂L(u)/∂u₁, ∂L(u)/∂u*₂, …, *∂L(u)/∂u*ₙ, *∂L(u)/∂λ)=* 0

从这个意义上说，拉格朗日乘子法的强大之处在于，它将一个约束优化问题转化为一个无约束优化问题，我们可以通过简单地将梯度设为零来解决它。

![](../Images/5da79d58c0d542dd0b841c58611ef0f3.png)

由[Jacobmelgrad](https://commons.wikimedia.org/w/index.php?title=User%3AJacobmelgaard&action=edit&redlink=1)在[维基百科](https://en.wikipedia.org/wiki/Lagrange_multiplier#/media/File:Lagrange_very_simple.svg)上提供的约束优化 CC BY-SA 3.0。

**原理**

直观地推导出为什么这样做有效并不困难。**可行区域** 是满足问题约束的点的集合（例如，上述圆上的点）；我们希望在这些点中找到使目标函数最优的点。

我们知道 *∇ f(u)* 指向最大下降方向的相反方向（沿着最大上升方向）。然而，在我们的情况下，我们只允许在可行区域内移动（满足约束的点）；因此，为了在约束下最小化函数，我们应该沿约束曲线的最大下降方向移动（这样我们不会退出可行区域并到达最小值）。

假设约束曲线上的点 *u* 的切线方向由 *r(u)* 给出，那么回忆一下[向量投影](https://en.wikipedia.org/wiki/Vector_projection)的公式，我们希望沿着以下方向移动，即 *∇ f(u)* 在 *r(u)* 上的投影：

![](../Images/2b29b634b577eb5476a6c9008e40d3c1.png)

与上述无约束情况类似，你应该意识到只要这是 0，我们就无法沿约束方向移动以进一步增加 *f(u)*（如果在最大值处）或减少它（如果在最小值处）。

很明显，为了使其为零，我们需要 *r(u)≠0*（以确保分母不为零）和 *∇ f(u) ⋅ r(u)=0*。对于后者，我们知道约束曲线上的法向量 *∇ g(u)* 与切线 *r(u)* 垂直。因此，我们只需要 *∇ f(u)* 与 *∇ g(u)* 平行。

因此，在最优点 *u** 处必须满足：

1.  约束的法向量非零： *∇ g(u*)≠0*（以确保 *r(u*)≠0*）

1.  约束满足：*g(u*)=0*（简单要求）

1.  *∇ f(u*)* ∥*∇ g(u*)*：存在一个实数β，使得*∇ f(u*) =* β*∇ g(u*)*

注意，通过重新排列项和重新命名 -β，（3）等价于“存在一个实数 *λ* 使得 *∇ f(u)+λ∇ g(u)=0*”。换句话说，*∇ᵤL(u,λ)* = 0，借此我们直观地推导出了一个约束的拉格朗日乘子定理（如有需要，请向上滚动查看）。

请注意，第一个条件称为约束资格。如果约束在满足（2）和（3）的点上不满足该条件，则没有保证该点是最优的，因为在该点上投影未定义。

**多个等式约束**

当存在多个约束*g*₁(u), g*₂*(u),…,g*ₖ*(u)*时，该方法可以顺利推广到以下情况：

1.  写出拉格朗日函数 *L(u,λ₁,λ*₂*,…,λ*ₖ*) = f(u) + λ₁*g*₁(u) + λ*₂g₂*(u) +…+λ*ₖgₖ*(u)*

1.  通过设置*∇ᵤL(u,λ₁,λ*₂*,…,λ*ₖ*)* = 0（*n*个方程）和 *g₁(u)=0, g*₂*(u)=0, …, g*ₖ*(u)=0* 来求解 *n+k* 个方程，以找到 *n+k* 个未知数 *u****₁***,u**₂*,…,u**ₙ, *λ₁,λ*₂*,…,λ*ₖ

1.  解为 *u*=(u****₁***,u**₂*,…,u**ₙ*)*

假设*∇ g(u*)*≠*0* 可以推广为 *∇* g*₁(u), ∇ g*₂*(u),…,∇ g*ₖ*(u)* 必须线性无关。这称为LICQ（线性独立约束资格）。

![](../Images/32bacc9722a73cdd828171b12bfcfa6d.png)

照片由[Jairph](https://unsplash.com/@jairph?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

## 带有不等式约束的约束优化

当我们处理形式为*g(u)≤0*的不等式约束时，问题不会变得更复杂。在这种情况下，我们希望找到满足*g(u)≤0*的*f(u)*的最优点。

对于上述问题，这意味着可行区域不仅仅是圆上的点，还包括圆内的点。对于特定问题（而不是一般情况），这显然不会改变解决方案。

![](../Images/26889ebcefeed62886e273e6efe884d3.png)

受约束的优化由[Jacobmelgrad](https://commons.wikimedia.org/w/index.php?title=User%3AJacobmelgaard&action=edit&redlink=1)在[维基百科](https://en.wikipedia.org/wiki/Lagrange_multiplier#/media/File:Lagrange_very_simple.svg)上提供，CC BY-SA 3.0。经Shading修改。

我们不解决拉格朗日乘子条件（2, 3），而是解决一组称为KKT条件的四个条件，这些条件是拉格朗日乘子情况的一般化。我们可以如下推导这些条件：

![](../Images/6650ca453c20451a371f2b4e1249283e.png)

优化问题的不等式约束图由[Onmyphd](https://commons.wikimedia.org/w/index.php?title=User%3AOnmyphd&action=edit&redlink=1)在[维基百科](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions#/media/File:Inequality_constraint_diagram.svg)上提供，CC BY-SA 3.0。

注意，对于一个任意的超曲面 *f(u)* 和约束 *g(u)≤0,* 假设是一个凸平滑函数且有一个最优点，那么有两种可能：

1.  最优点 *uᴾ* 位于可行区域内。

+   在这种情况下，优化问题的解 *u** 必须是 *uᴾ* 且 *g(u*)<0* 必须成立（左侧图像）。

+   在可行区域中不可能找到更优的点，因为 *uᴾ* 是 *f(u)* 的整个区域（领域）上的最优点（例如最小值）。

2. 最优点 *uᴾ* 位于可行区域外。

+   在这种情况下，如果点是最大值，*f(u)* 在可行区域中必须只是减少（不能再增加，否则会产生另一个最优点）

+   如果点是最小值，*f(u)* 在可行区域中必须只是增加（不能再减少，否则会产生另一个最优点）

+   因此，最优点 *u** 必须位于可行区域的边缘，因为在内部不会变得更好（*g(u*) = 0* 必须成立）

在第一种情况下，显然求解优化问题等同于求解无约束版本的问题。

*∇ᵤf(u) = 0*

我们称约束为“非活跃的”是因为它在优化问题中没有影响。

在第二种情况下，显然求解优化问题等同于求解等式约束版本的问题（拉格朗日乘数法）。

对于这种情况唯一需要注意的是，*λ* 对于最小化必须 ≥ 0，对最大化必须 ≤ 0。对于最小化，这意味着 *∇ᵤf(u)* 和 *∇ᵤg(u)* 指向相反方向（即，β 在 *∇ᵤ f(u) =* β*∇ᵤg(u)* 是 ≤0），这必须成立，因为 *∇ᵤg(u)* 指向约束 *g(u)≥0* 的正侧（基本属性）；与此同时，*∇ᵤ f(u)* 指向约束的负侧，因为 *f(u)* 在那里增加。对于最大化情况，可以轻松构造类似的论证。

但我们事先不知道这两种情况中的哪一种适用。我们可以按照以下方法合并它们（假设最小化）：

1.  写出拉格朗日函数 *L(u,λ)=f(u)+λg(u)*

1.  设置 *∇ᵤL(u,λ)* = 0（*n* 个方程）和 *g(u)≤0*

1.  求解解 (*u**₁***,u**₂*,…,u**ₙ,*λ.*) 其中一个以上情况适用：

+   *λ=0* 和 *g(u*)<0*（*λ=0* 的第一个情况意味着 *∇ᵤL(u,λ)* = *∇ᵤf(u) = 0*，因此步骤 1,2 等同于求解 *∇ᵤf(u) = 0*）

+   *g(u*)=0* 和 *λ≥0*（第二种情况，因为 *g(u)=0* 意味着拉格朗日方法是正确的，这就是我们所做的）

我们可以总结这两点，即 *g(u*)≤0* 和 *λ≥0* 必须成立，并且 *λg(u*)=0* 必须成立（*λ* 或 *g(u*)* 之一必须为零）。这意味着给定一个形式的优化问题：

**最小化** *f(u)* **使得** *g(u)≤0*

我们期望最优点 *u* 满足以下四个条件：

1.  稳定性：*∇ᵤL(u*,λ)* = 0

1.  原始可行性：*g(u*)≤0*

1.  双重可行性：*λ≥0*

1.  互补松弛性：*λg(u*)=0*

并且解决这些条件一起产生最优点*u*。实际上，对于[凸优化问题](https://en.wikipedia.org/wiki/Convex_optimization)，这些条件是充分但不是必要的。也就是说，

+   如果一个点满足这些条件（例如，通过一起解决它们找到），那么这足以证明该点是最优的（对于凸问题无需进一步寻找）。

+   与此同时，这些条件并非点必须为最优点的必要条件。有可能解决这些条件却没有解时，在现实中存在一个满足条件但不满足它们的最优点。例如，考虑*f(x) = x*和约束*x² ≤ 0*（此外，[这里和另一个KKT示例](https://drive.google.com/drive/u/1/folders/1uQ9iiadmIY-hg2DnjQaYfE3xNHgMdWrN)在本文档中解决）。

一旦我们强制执行诸如LICQ（前述）的约束条件，我们可以保证KKT条件既充分又必要。一个更易于检查的替代约束条件是Slater的条件，它保证了对于凸问题，KKT是充分且必要的。

Slater的条件简单地表明可行域必须有一个内点。也就是说，对于约束*g(u)≤0*，函数必须有域*f(u)*内满足*g(u)<0*的点。这是一个基本条件，在现实生活中几乎总是满足的（但不包括上述反例），这意味着KKT很少会错过寻找最优解。

**多重约束**

当存在多个等式约束*h*₁(u), h*₂*(u),…,h*ₖ*(u)*以及多个不等式约束*g*₁(u), g*₂*(u),…,g*ₚ*(u)*时，该方法通过编写完整的拉格朗日函数并仅对不等式约束及其乘子（我们称之为α）检查KKT条件来平滑地推广：

0\. 写出拉格朗日函数

![](../Images/7529d8b5c057f5fd95356441a226b6f4.png)

1.  设置*∇ᵤL(u,λ₁,λ*₂*,…,λ*ₖ, α*₁*, α₂, …, αₚ*)* = 0（n个方程）

2\. 设置*h*₁(u)=0, h*₂*(u)=0, …, h*ₖ*(u)=0（k*个方程），并设置

g*₁(u)≤0, g*₂*(u)≤0, …, g*ₚ*(u)≤0*（*p*个不等式）

3\. 设置α*₁≥0*, α₂≥0, …, αₚ≥0（*p*个不等式）

4\. 设置α*₁*g*₁(u) =* α₂*g*₂*(u) =* αₚ*g*ₖ*(u) = 0（*p*个方程）

总共，您有*n+k+p*个方程和*2p*个不等式，您将一起解决以找到*n+k+p*个变量*(u****₁***,u**₂*,…,u**ₙ,*λ₁,λ*₂*,…,λ*ₖ,α*₁*, α₂, …, αₚ *)*，这将产生最小化函数并满足*k+p*个约束的解*u*=(u****₁***,u**₂*,…,u**ₙ*)*。

![](../Images/f4abf591968faa51746f8c2305eb5988.png)

照片由[SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral)提供的 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上获取

## 对偶原理

对偶原理简单地说明，对于任何优化问题，我们可以写出一个对偶优化问题，解决它可以告诉我们原始问题（称为原始问题）的某些内容或者直接解决它。

对于任何形式的优化问题：

![](../Images/e18b1d046a18f6de01955a0da13db128.png)

对偶优化问题的形式是：

![](../Images/a6e209be534cbc88f290fa80842e2d55.png)

是的，最小化的形式与拉格朗日函数相同

反之亦然，如果是最大化。

**示例**

例如，我们之前讨论的受约束优化问题：

![](../Images/00f3d920a058f61dfc08cee96350b8ae.png)

具有相应的对偶问题：

![](../Images/c467f002fc40352291fe47889beb5986.png)

如基本微积分所示，要进行最小化，我们首先做

![](../Images/2cfd493b0d031b1d57d9e2d56a771614.png)

这意味着

![](../Images/a26caebb7ae806f60f9abca6d8c2fa8c.png)

因此，优化问题变成

![](../Images/84309110d276e7589f29470011a95b29.png)

现在所需的只是对其进行微分并使其等于零，从而得到 λ = 1/√2，这意味着 *(x*, y*)* = (−1/√2, −1/√2)，这与通过 KKT 解决原始问题所得到的解相同。

**推导对偶**

原始（原始）问题是

![](../Images/f01b58c1873fac972a959be472e5b2fd.png)

假设我们定义一个函数，当 *u* 不在可行区域内（不满足约束）时返回无穷大，否则返回零：

![](../Images/850a15a476043605c867247c376fb50b.png)

在这种情况下，原始问题等价于

![](../Images/3907fd374957f9da31adb34a672209be.png)

这应该是合理的，因为 *f(u)+P(u)* 将可行区域之外的值设为无穷大，并保持可行区域不变。这个和的最小值必须发生在可行区域内，即使约束是明确强制的，因为无穷大大于任何东西。

观察到我们可以声明：

![](../Images/112ce6363f6219b8ee7073e4ef6f1a6d.png)

因为有了这个，如果：

+   *g(u)<0* 时 *P(u)=0*，如之前定义，因为要最大化 *λg(u)* 必须满足 *λ=0*（否则由于 *g(u)<0* 它是负值）

+   *g(u)=0* 时 *P(u)=0*，如之前定义，因为 *λg(u)* 将为零（*λ* 可以是任何值）

+   *g(u)>0* 时 *P(u)=*∞，如之前定义，*λ=*∞ 是最大化 *λg(u)* 的值

因此，原始问题等价于

![](../Images/0873677c905f0b22d8e443aa4709e91e.png)

引入 f 到最大值是可以的，因为它不是 *λ* 的显式函数

这个和对偶问题的区别在于，在对偶中最大值和最小值被交换了。

![](../Images/f8f418d86b425be55e95b7913751c435.png)

因此，因为一般来说 *MinMax(…) ≥ MaxMin(…)*，对偶的解将是原始问题解的下界。这被称为弱对偶性。

一个更有趣的情况是，当*MinMax(…) = MaxMin(…)*时，双重问题的解恰好也是原始问题的解（如例子中所示）。这被称为强对偶性。你可以适度容易地[证明](https://or.stackexchange.com/questions/3117/is-there-any-relationship-between-kkt-and-duality)当KKT条件既必要又充分时，该等式成立（因此强对偶性）。换句话说，强对偶性将在Slater条件成立的情况下适用于凸问题！

**那又怎么样？**

如果你考虑一下，解决对偶问题相当于仅在原始问题上应用KKT的平稳性和对偶可行性条件。你不再需要应用原始可行性和互补松弛条件，而是需要处理额外的对偶变量的最小化。在许多情况下，这比在原始问题上解决KKT要简单得多。这个额外的最小化可以通过线性或二次规划来处理。

**多个约束？**

在推广到多个约束时，拉格朗日函数的变化正如你所预期的那样（类似于我们所见），我们只需在最大化中为与不等式约束相关的乘子添加α≥0条件。

![](../Images/c17f9f792aabbe00b5ba338c1a4fcbdb.png)

由[现代汽车集团](https://unsplash.com/@hyundaimotorgroup?utm_source=medium&utm_medium=referral)拍摄，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望这个故事帮助你真正理解了无约束优化、拉格朗日乘子、KKT和对偶性。下次见，再见！
