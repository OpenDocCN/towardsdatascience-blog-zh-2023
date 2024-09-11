# 补充对数-对数回归的温和介绍

> 原文：[https://towardsdatascience.com/a-gentle-introduction-to-complementary-log-log-regression-8ac3c5c1cd83?source=collection_archive---------1-----------------------#2023-10-02](https://towardsdatascience.com/a-gentle-introduction-to-complementary-log-log-regression-8ac3c5c1cd83?source=collection_archive---------1-----------------------#2023-10-02)

## 一种在特殊条件下的逻辑回归替代方法

[](https://medium.com/@akif.iips?source=post_page-----8ac3c5c1cd83--------------------------------)[![Akif Mustafa](../Images/1fb81af6fc0aeefedc1da59b3ba2b7ba.png)](https://medium.com/@akif.iips?source=post_page-----8ac3c5c1cd83--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ac3c5c1cd83--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8ac3c5c1cd83--------------------------------) [Akif Mustafa](https://medium.com/@akif.iips?source=post_page-----8ac3c5c1cd83--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ff7bb988de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-complementary-log-log-regression-8ac3c5c1cd83&user=Akif+Mustafa&userId=7ff7bb988de&source=post_page-7ff7bb988de----8ac3c5c1cd83---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ac3c5c1cd83--------------------------------) ·8分钟阅读·2023年10月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ac3c5c1cd83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-complementary-log-log-regression-8ac3c5c1cd83&user=Akif+Mustafa&userId=7ff7bb988de&source=-----8ac3c5c1cd83---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ac3c5c1cd83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-complementary-log-log-regression-8ac3c5c1cd83&source=-----8ac3c5c1cd83---------------------bookmark_footer-----------)

在统计建模和回归分析中，有许多技术可以选择。其中一种常被忽视但在某些场景中非常有用的方法是补充对数-对数（Cloglog）回归。在这篇文章中，我们将详细介绍什么是 Cloglog 回归、何时使用它以及它是如何工作的。

**Cloglog 回归的前身**

Cloglog回归是一种用于分析二元响应变量的统计建模技术。我们知道，当涉及到建模二元结果时，首先想到的模型是逻辑回归。实际上，cloglog是逻辑回归在特殊场景中的替代方案。我假设大家对逻辑回归有基本的了解。然而，如果你对逻辑回归不熟悉，建议首先获得对其的基本了解。网上有大量关于逻辑回归的资源，可以帮助你熟悉这个主题。

Cloglog回归是逻辑回归模型的扩展，当事件的概率非常小或非常大时尤其有用。大多数时候，cloglog回归用于处理稀有事件或结果极度偏斜的情况。

**对Cloglog回归的需求**

众所周知，逻辑回归遵循S型函数的形式。下面展示了S型曲线：

![](../Images/37437d3c266bea3f70d7cf3b7bcd358f.png)

作者提供的图像

从这个图形表示中可以明显看出，对于较小的‘x’值，结果的概率保持相对较低，而对于较大的值，结果的概率变得更高。曲线在‘Y’的值为0.5处表现出对称性。这种对称性意味着在逻辑回归中，存在一个潜在的特征，即成功或事件发生的概率（Y = 1）围绕0.5对称分布。这意味着概率的最显著变化发生在图表的中间部分，而在极端的‘x’值下，概率相对较不敏感。当我们的结果变量有大量成功或事件的情况时，这一假设是成立的，示例包括：

抑郁症的流行情况

![](../Images/f0828626f4714ec94d3ddf84596309e3.png)

作者提供的图像

或学生考试及格

![](../Images/b46d2c7178ded3de6b8e9476e4ba4364.png)

作者提供的图像

然而，当事件非常稀少或非常频繁时，这一假设可能不成立，在这种情况下，成功或事件发生的概率要么极低，要么极高。例如，考虑人们在心脏骤停后的生存情况，其中成功的可能性显著降低：

![](../Images/74fc7852098bed4306a871578829317a.png)

作者提供的图像

或医院内青光眼手术的成功率（成功的机会非常高）：

![](../Images/73b163a3598998fd717f0673d06a09c6.png)

作者提供的图像

在这种情况下，0.5处的对称分布并不理想，建议使用不同的建模方法，这就是互补对数-对数回归的应用场景。

与logit和probit不同，Cloglog函数是不对称的，并且偏向一侧。

**互补对数-对数回归的工作原理**

Cloglog回归使用互补对数-对数函数，生成一个S形曲线但不对称。Cloglog回归的形式如下：

![](../Images/92c38fa4d6719d3d4f2c10bdc00b1e68.png)

作者提供的图像

方程的左侧称为互补对数-对数变换。与logit和probit变换类似，这种变换也将二元响应（0或1）转换为（-∞到+∞）。该模型也可以写成：

![](../Images/9bad320bb076efcefb4d81068483e0f1.png)

作者提供的图像

在下图中，我们可视化了在R中使用logit、probit和cloglog变换生成的曲线。

```py
# Load the ggplot2 package
library(ggplot2)

# Create a sequence of values for the x-axis
x <- seq(-5, 5, by = 0.1)

# Calculate the values for the logit and probit functions
logit_vals <- plogis(x)
probit_vals <- pnorm(x)

# Calculate the values for the cloglog function manually
cloglog_vals <- 1 - exp(-exp(x))

# Create a data frame to store the values
data <- data.frame(x, logit_vals, probit_vals, cloglog_vals)

# Create the plot using ggplot2
ggplot(data, aes(x = x)) +
  geom_line(aes(y = logit_vals, color = "Logit"), size = 1) +
  geom_line(aes(y = probit_vals, color = "Probit"), size = 1) +
  geom_line(aes(y = cloglog_vals, color = "CLogLog"), size = 1) +
  labs(title = "Logit, Probit, and CLogLog Functions",
       x = "x", y = "Probability") +
  scale_color_manual(values = c("Logit" = "red", "Probit" = "blue", "CLogLog" = "green")) +
  theme_minimal()
```

![](../Images/c174599319e5242b314ca2c028b81511.png)

作者提供的图像

从图中我们可以观察到明显的差异：虽然logit和probit变换在值0.5附近是对称的，但cloglog变换表现出不对称。在逻辑回归和probit函数中，概率在接近0和1时以类似的速率变化。在数据在[0, 1]区间内不对称，且在小到中等值时变化缓慢但在接近1时急剧变化的情况下，logit和probit模型可能不适合。在这些情况下，当响应变量的非对称性明显时，互补对数-对数模型（cloglog）成为一个有前景的替代方案，提供了更好的建模能力。从Cloglog函数的图中可以看到，P(Y = 1)在接近0时较慢，而在接近1时则急剧上升。

**让我们以一个例子来说明：检查锌缺乏**

我模拟了一个特定人群中的锌缺乏数据 [注：这些数据是作者为个人使用而创建的模拟数据]。数据集还包括年龄、性别和体重指数（BMI）等因素的数据。值得注意的是，数据集中只有2.3%的人表现出锌缺乏，这表明在这个人群中锌缺乏的发生率相对较低。我们的结果变量是锌缺乏（二元变量（0 = 否，1 = 是）），预测变量是年龄、性别和体重指数（BMI）。我们在R中使用逻辑回归、概率回归和Cloglog回归，并通过AIC比较这三种模型。

```py
> #tabulating zinc deficiency
> tab = table(zinc$zinc_def)
> rownames(tab)  = c("No", "Yes")
> print(tab)

  No  Yes 
8993  209 

> #tabulating sex and zinc deficieny
> crosstab = table(zinc$sex, zinc$zinc_def)
> rownames(crosstab) = c("male" , "female")
> colnames(crosstab) = c("No", "Yes")
> print(crosstab)

           No  Yes
  male   4216  159
  female 4777   50

> #definig sex as a factor variable
> zinc$sex = as.factor(zinc$sex)

> #logistic regression of zinc deficiency predicted by age, sex and bmi
> model1 = glm(zinc_def ~ age + sex + bmi, data = zinc, family = binomial(link = "logit"))
> summary(model1)

Call:
glm(formula = zinc_def ~ age + sex + bmi, family = binomial(link = "logit"), 
    data = zinc)

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept) -2.064053   0.415628  -4.966 6.83e-07 ***
age         -0.034369   0.004538  -7.574 3.62e-14 ***
sex2        -1.271344   0.164012  -7.752 9.08e-15 ***
bmi          0.010059   0.015843   0.635    0.525    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 1995.3  on 9201  degrees of freedom
Residual deviance: 1858.8  on 9198  degrees of freedom
  (1149 observations deleted due to missingness)
AIC: 1866.8

Number of Fisher Scoring iterations: 7

> #probit model
> model2 = glm(zinc_def ~ age + sex + bmi, data = zinc, family = binomial(link = "probit"))
> summary(model2)

Call:
glm(formula = zinc_def ~ age + sex + bmi, family = binomial(link = "probit"), 
    data = zinc)

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept) -1.280983   0.176118  -7.273 3.50e-13 ***
age         -0.013956   0.001863  -7.493 6.75e-14 ***
sex2        -0.513252   0.064958  -7.901 2.76e-15 ***
bmi          0.003622   0.006642   0.545    0.586    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 1995.3  on 9201  degrees of freedom
Residual deviance: 1861.7  on 9198  degrees of freedom
  (1149 observations deleted due to missingness)
AIC: 1869.7

Number of Fisher Scoring iterations: 7

> #cloglog model
> model3 = glm(zinc_def ~ age + sex + bmi, data = zinc, family = binomial(link = "cloglog"))
> summary(model3)

Call:
glm(formula = zinc_def ~ age + sex + bmi, family = binomial(link = "cloglog"), 
    data = zinc)

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept) -2.104644   0.407358  -5.167 2.38e-07 ***
age         -0.033924   0.004467  -7.594 3.09e-14 ***
sex2        -1.255728   0.162247  -7.740 9.97e-15 ***
bmi          0.010068   0.015545   0.648    0.517    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 1995.3  on 9201  degrees of freedom
Residual deviance: 1858.6  on 9198  degrees of freedom
  (1149 observations deleted due to missingness)
AIC: 1866.6

Number of Fisher Scoring iterations: 7

> #extracting AIC value of each model for model comparison
> AIC_Val = AIC(model1, model2, model3)
> print(AIC_Val)
       df      AIC
model1  4 1866.832
model2  4 1869.724
model3  4 1866.587
```

**系数的解释**

在Cloglog回归中，系数的解释类似于逻辑回归。每个系数代表预测变量变化一个单位时，结果对数几率的变化。通过对系数取指数，我们得到比值比。

在我们特定的模型中，年龄的系数是-0.034。这意味着年龄每增加一年，锌缺乏的对数几率减少0.034单位。通过对这个系数取指数，我们可以计算出比值比：

比值比 = exp(-0.034) = 0.97

这表明年龄增加一年与锌缺乏的几率降低3%相关。

类似地，对于变量‘性别’：

比值比 = exp(-1.25) = 0.28

这表明，与男性相比，女性经历锌缺乏的几率降低了 72%。

我们也可以解释 BMI 系数，但需要注意的是，BMI 的 p 值为 0.52，表明在这个模型中，它与锌缺乏并没有显著关联。

**应用及使用**

Cloglog 回归被广泛应用于各种研究领域，包括稀有疾病流行病学、药物疗效研究、信用风险评估、缺陷检测和生存分析。特别是，Cloglog 模型在生存分析中具有重要意义，因为它与事件发生的连续时间模型密切相关。

互补对数-对数回归是一种强大且常被忽视的统计技术，在传统的逻辑回归不适用的情况下，它可能非常有价值。通过理解其原理和应用，你可以将这个多功能工具加入到你的数据分析工具箱中。
