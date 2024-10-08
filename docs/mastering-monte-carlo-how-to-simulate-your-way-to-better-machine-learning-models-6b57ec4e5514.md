# 精通蒙特卡洛：如何通过模拟提升机器学习模型

> 原文：[`towardsdatascience.com/mastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514`](https://towardsdatascience.com/mastering-monte-carlo-how-to-simulate-your-way-to-better-machine-learning-models-6b57ec4e5514)

## 通过模拟技术提升预测算法的概率方法应用

[](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)![Sydney Nye](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------) [Sydney Nye](https://medium.com/@sydneynye?source=post_page-----6b57ec4e5514--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b57ec4e5514--------------------------------) ·26 分钟阅读·2023 年 8 月 2 日

--

![](img/efbc38b2902307f75ef51ff4a36e9eba.png)

“在蒙特卡洛的轮盘赌桌上”由爱德华·蒙克（1892 年）

# 一位科学家玩扑克牌如何永远改变了统计学游戏

在 1945 年的动荡岁月中，当世界正经历第二次世界大战的最后阶段时，一场扑克牌游戏悄然引发了计算领域的一次突破。这不仅仅是普通的游戏，而是导致蒙特卡洛方法诞生的关键。玩家？正是科学家**斯坦尼斯瓦夫·乌拉姆**，他当时还深陷于曼哈顿计划中。乌拉姆在从疾病中恢复的过程中，沉迷于扑克牌游戏。游戏的复杂概率引起了他的兴趣，他意识到反复模拟游戏可以提供这些概率的良好近似。这是一个灵光一现的时刻，类似于牛顿的苹果，但换成了扑克牌。随后，乌拉姆与他的同事**约翰·冯·诺依曼**讨论了这些想法，两人共同形式化了蒙特卡洛方法，命名来源于著名的摩纳哥蒙特卡洛赌场（在上面的爱德华·蒙克的著名画作中描绘），在那里赌注高昂，运气主宰——这正如方法本身。

快进到今天，蒙特卡罗方法已成为机器学习领域中的一张王牌，包括在强化学习、贝叶斯滤波和复杂模型优化中的应用([4](https://link.springer.com/referenceworkentry/10.1007%2F978-0-387-30164-8_525))。其稳健性和多功能性确保了其持续的相关性，距其诞生已超过七十年。从乌拉姆的纸牌游戏到今天复杂的 AI 应用，蒙特卡罗方法依然是应对复杂系统中模拟和逼近力量的见证。

# 用概率模拟来玩转你的牌

在数据科学和机器学习的复杂世界中，蒙特卡罗模拟类似于精心计算的赌注。这种统计技术使我们能够在不确定性面前进行战略性投注，为复杂的确定性问题提供概率性的解决方案。在这篇文章中，我们将揭示蒙特卡罗模拟的神秘面纱，并探讨其在统计学和机器学习中的强大应用。

我们将深入探讨蒙特卡罗模拟背后的理论，阐明使这一技术成为强大问题解决工具的原理。我们将通过一些 Python 中的实际应用，演示如何在实践中实现蒙特卡罗模拟。

接下来，我们将探讨如何利用蒙特卡罗模拟来优化机器学习模型。我们将专注于通常具有挑战性的超参数调优任务，提供一个实用的工具包，以帮助应对这一复杂的领域。

所以，下注吧，开始吧！

# 理解蒙特卡罗模拟

蒙特卡罗模拟对于数学家和数据科学家来说是一项宝贵的技术。这些模拟提供了一种在广泛而复杂的可能性中导航的方法，制定有根据的假设，并逐步完善选择，直到出现最合适的解决方案。

其工作原理如下：我们生成大量随机场景，遵循某个预定义的过程，然后审视这些场景，以估计各种结果的概率。这里有一个类比来帮助理解：将每个场景视为流行的哈斯布罗棋盘游戏“Clue”中的一次回合。对于不熟悉的人来说，“Clue”是一个侦探风格的游戏，玩家在一座豪宅中移动，收集证据以推断犯罪的细节——谁、什么和哪里。每回合或提问都会排除潜在的答案，使玩家更接近揭示真实的犯罪场景。同样，蒙特卡罗研究中的每次模拟提供的见解使我们更接近解决复杂问题的方案。

在机器学习的领域，这些“场景”可以代表不同的模型配置、各种超参数集、将数据集分割为训练集和测试集的不同方式以及其他许多应用。通过评估这些场景的结果，我们可以获得对机器学习算法行为的宝贵洞察，从而做出有关其优化的明智决策。

# 飞镖游戏

要理解蒙特卡洛模拟，想象你在玩飞镖游戏。但不是瞄准特定目标，而是蒙上眼睛随机地将飞镖扔向一个大的正方形飞镖盘。这个正方形里有一个圆形目标。你的目标是估算圆周率π，即圆的周长与直径的比值。

听起来不可能，对吧？但这里有个窍门：圆的面积与正方形的面积之比是π/4。因此，如果你投掷大量的飞镖，落在圆内的飞镖数与总飞镖数的比率应大致为π/4。将这个比率乘以 4，你就能估算出π！

# 随机猜测 vs. 蒙特卡洛

为了展示蒙特卡洛模拟的威力，让我们将它与一种更简单的方法进行比较，也许是所有方法中最简单的：随机猜测。

当你运行下面的代码进行随机和蒙特卡洛两种情况时，每次都会得到一组不同的预测。这是意料之中的，因为飞镖是随机投掷的。这是蒙特卡洛模拟的一个关键特征：它们本质上是随机的。然而，尽管有这种随机性，它们在正确使用时可以提供非常准确的估算。因此，虽然你的结果不会完全像我的，但它们会讲述相同的故事。

在第一组可视化图（**图 1a** 到 **图 1f**）中，我们对π的值进行了一系列随机猜测，每次生成一个基于猜测值的圆。让我们给这个“随机性”一个正确的推动，假设虽然我们不能记住π的确切值，但我们知道它高于 2 且低于 4。从结果图中可以看到，圆的大小根据猜测值变化很大，这显示了这种方法的不准确性（这也不应让人感到惊讶）。每个图中的绿色圆圈代表单位圆，即我们试图估算的“真实”圆。蓝色圆圈则基于我们的随机猜测。

```py
#Random Guessing of Pi

# Before running this code, make sure you have the necessary packages installed.
# You can install them using "pip install" on your terminal or "conda install" if using a conda environment

# Import necessary libraries
import random
import plotly.graph_objects as go
import numpy as np

# Number of guesses to make. Adjust this for more guesses and subsequent plots
num_guesses = 6

# Generate the coordinates for the unit circle
# We use np.linspace to generate evenly spaced numbers over the range from 0 to 2*pi.
# These represent the angles in the unit circle.
theta = np.linspace(0, 2*np.pi, 100)

# The x and y coordinates of the unit circle are then the cosine and sine of these angles, respectively.
unit_circle_x = np.cos(theta)
unit_circle_y = np.sin(theta)

# We'll make a number of guesses for the value of pi
for i in range(num_guesses):
    # Make a random guess for pi between 2 and 4
    pi_guess = random.uniform(2, 4)

    # Generate the coordinates for the circle based on the guess
    # The radius of the circle is the guessed value of pi divided by 4.
    radius = pi_guess / 4

    # The x and y coordinates of the circle are then the radius times the cosine and sine of the angles, respectively.
    circle_x = radius * np.cos(theta)
    circle_y = radius * np.sin(theta)

    # Create a scatter plot of the circle
    fig = go.Figure()

    # Add the circle to the plot
    # We use a Scatter trace with mode 'lines' to draw the circle.
    fig.add_trace(go.Scatter(
        x = circle_x,
        y = circle_y,
        mode='lines',
        line=dict(
            color='blue',
            width=3
        ),
        name='Estimated Circle'
    ))

    # Add the unit circle to the plot
    fig.add_trace(go.Scatter(
        x = unit_circle_x,
        y = unit_circle_y,
        mode='lines',
        line=dict(
            color='green',
            width=3
        ),
        name='Unit Circle'
    ))

    # Update the layout of the plot
    # We set the title to include the guessed value of pi, and adjust the size and axis ranges to properly display the circles.
    fig.update_layout(
        title=f"Fig1{chr(97 + i)}: Randomly Guessing Pi: {pi_guess}",
        width=600,
        height=600,
        xaxis=dict(
            constrain="domain",
            range=[-1, 1]
        ),
        yaxis=dict(
            scaleanchor="x",
            scaleratio=1,
            range=[-1, 1]
        )
    )

    # Display the plots
    fig.show()
```

![](img/c674c42f58e4274d2ff3878161981457.png)![](img/4e61d6d90a2f693f42538e5de3c9a8e8.png)![](img/726aa1deefec43a789928eb83dc059f3.png)![](img/c7298f7008707aeb31dbee4188f529c2.png)![](img/667c470d5442820f7de2dc7b42eba11a.png)![](img/6ddf15e494f0387330fe91ca4b89421e.png)

图 1a-1f：π的随机估算

你可能会注意到一个奇怪的现象：在随机猜测方法中，有时接近真实π值的猜测会导致一个更远离单位圆的圆。这种明显的矛盾产生是因为我们关注的是圆的周长，而不是半径或面积。两个圆之间的视觉差距表示的是基于猜测的圆周长估算误差，而不是整个圆的误差。

在第二组可视化（**图 2a**至**图 2f**）中，我们使用蒙特卡罗方法来估算π值。我们不是进行随机猜测，而是向一个正方形上扔大量的飞镖，并计算落在正方形内切圆内的数量。由此得到的π值估算更为准确，如图中所示，圆的大小更接近实际单位圆。绿色点表示落在单位圆内的飞镖，红色点表示落在单位圆外的飞镖。

```py
#Monte Carlo Estimation of Pi

# Import our required libraries
import random
import math
import plotly.graph_objects as go
import plotly.io as pio
import numpy as np

# We'll simulate throwing darts at a dartboard to estimate pi. Let's throw 10,000 darts.
num_darts = 10000

# To keep track of darts that land in the circle.
darts_in_circle = 0

# We'll store the coordinates of darts that fall inside and outside the circle.
x_coords_in, y_coords_in, x_coords_out, y_coords_out = [], [], [], []

# Let's generate 6 figures throughout the simulation. Therefore, we will create a new figure every 1,666 darts (10,000 divided by 6).
num_figures = 6
darts_per_figure = num_darts // num_figures

# Create a unit circle to compare our estimates against. Here, we use polar coordinates and convert to Cartesian.
theta = np.linspace(0, 2*np.pi, 100)
unit_circle_x = np.cos(theta)
unit_circle_y = np.sin(theta)

# We start throwing the darts (simulating random points inside a 1x1 square and checking if they fall inside a quarter circle).
for i in range(num_darts):
    # Generate random x, y coordinates between -1 and 1.
    x, y = random.uniform(-1, 1), random.uniform(-1, 1)

    # If a dart (point) is closer to the origin (0,0) than the distance of 1, it's inside the circle.
    if math.sqrt(x**2 + y**2) <= 1:
        darts_in_circle += 1
        x_coords_in.append(x)
        y_coords_in.append(y)
    else:
        x_coords_out.append(x)
        y_coords_out.append(y)

    # After every 1,666 darts, let's see how our estimate looks compared to the real unit circle.
    if (i + 1) % darts_per_figure == 0:
        # We estimate pi by seeing the proportion of darts that landed in the circle (out of the total number of darts).
        pi_estimate = 4 * darts_in_circle / (i + 1)

        # Now we create a circle from our estimate to compare visually with the unit circle.
        estimated_circle_radius = pi_estimate / 4
        estimated_circle_x = estimated_circle_radius * np.cos(theta)
        estimated_circle_y = estimated_circle_radius * np.sin(theta)

        # Plot the results using Plotly.
        fig = go.Figure()

        # Add the darts that landed inside and outside the circle to the plot.
        fig.add_trace(go.Scattergl(x=x_coords_in, y=y_coords_in, mode='markers', name='Darts Inside Circle', marker=dict(color='green', size=4, opacity=0.8)))
        fig.add_trace(go.Scattergl(x=x_coords_out, y=y_coords_out, mode='markers', name='Darts Outside Circle', marker=dict(color='red', size=4, opacity=0.8)))

        # Add the real unit circle and our estimated circle to the plot.
        fig.add_trace(go.Scatter(x=unit_circle_x, y=unit_circle_y, mode='lines', name='Unit Circle', line=dict(color='green', width=3)))
        fig.add_trace(go.Scatter(x=estimated_circle_x, y=estimated_circle_y, mode='lines', name='Estimated Circle', line=dict(color='blue', width=3)))

        # Customize the plot layout.
        fig.update_layout(title=f"Figure {chr(97 + (i + 1) // darts_per_figure - 1)}: Thrown Darts: {(i + 1)}, Estimated Pi: {pi_estimate}", width=600, height=600, xaxis=dict(constrain="domain", range=[-1, 1]), yaxis=dict(scaleanchor="x", scaleratio=1, range=[-1, 1]), legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01))

        # Display the plot.
        fig.show()

        # Save the plot as a PNG image file.
        pio.write_image(fig, f"fig2{chr(97 + (i + 1) // darts_per_figure - 1)}.png")
```

![](img/b4abfadbf76394515d476d64869888a0.png)![](img/d0483d8b00a390f5ad2f25d9a4f93bb4.png)![](img/051502bfd26608a18449573753907325.png)![](img/67d5ab3f95e8e9e5541b3b0dee214bcf.png)![](img/031d3693a4cb408f2dad132d3a83c9da.png)![](img/f95f0f2735c5d151e8b547e58c1953a0.png)

图 2a-2f：蒙特卡罗估算π值

在蒙特卡罗方法中，π的估算是基于落在圆内的“飞镖”与总投掷次数的比例。得到的π值估算用于生成一个圆。如果蒙特卡罗估算不准确，生成的圆将再次出现错误。这个估算圆与单位圆之间的差距宽度，提供了蒙特卡罗估算准确性的指示。

然而，由于蒙特卡罗方法在“飞镖”数量增加时会产生更准确的估算，因此随着更多“飞镖”的投掷，估算的圆应当会趋近于单位圆。因此，虽然两种方法在估算不准确时都显示出差距，但随着“飞镖”数量的增加，这种差距在蒙特卡罗方法中应当会更加一致地减少。

# 预测π值：概率的力量

蒙特卡罗模拟之所以如此强大，是因为它们能够利用随机性来解决确定性问题。通过生成大量随机场景并分析结果，我们可以估算不同结果的概率，即使对于那些难以通过解析方法解决的复杂问题也是如此。

在估计π的情况下，蒙特卡罗方法允许我们即使随机投掷飞镖，也能做出非常准确的估计。如前所述，投掷的飞镖越多，我们的估计就越准确。这展示了大数法则，这是概率论中的一个基本概念，指出从大量试验中获得的结果的平均值应该接近期望值，并且随着试验次数的增加，会趋近于期望值。让我们通过绘制投掷的飞镖数量与蒙特卡罗估计的π与真实π之间的差异的关系图来检验我们的**图 2a-2f**中的六个例子是否趋于真实。一般来说，我们的图表（**图 2g**）应该呈现负趋势。这里是实现这一点的代码：

```py
# Calculate the differences between the real pi and the estimated pi
diff_pi = [abs(estimate - math.pi) for estimate in pi_estimates]

# Create the figure for the number of darts vs difference in pi plot (Figure 2g)
fig2g = go.Figure(data=go.Scatter(x=num_darts_thrown, y=diff_pi, mode='lines'))

# Add title and labels to the plot
fig2g.update_layout(
    title="Fig2g: Darts Thrown vs Difference in Estimated Pi",
    xaxis_title="Number of Darts Thrown",
    yaxis_title="Difference in Pi",
)

# Display the plot
fig2g.show()

# Save the plot as a png
pio.write_image(fig2g, "fig2g.png")
```

![](img/9bc1eaea5faddf7bcbcfdf80c02843ae.png)

注意，即使只有 6 个例子，整体模式也如预期：投掷的飞镖更多（更多的场景），估计值与真实值之间的差异更小，从而预测更准确。

假设我们投掷 1,000,000 支飞镖，并进行 500 次预测。换句话说，我们将记录在 1,000,000 支飞镖模拟过程中，π的估计值与实际值之间的差异，在 500 个均匀间隔的位置上。与其生成 500 个额外的图形，不如直接跳到我们要确认的内容：是否真的如飞镖数量增加那样，π的预测值与真实π之间的差异变小。我们将使用散点图（**图 2h**）：

```py
#500 Monte Carlo Scenarios; 1,000,000 darts thrown
import random
import math
import plotly.graph_objects as go
import numpy as np

# Total number of darts to throw (1M)
num_darts = 1000000
darts_in_circle = 0

# Number of scenarios to record (500)
num_scenarios = 500
darts_per_scenario = num_darts // num_scenarios

# Lists to store the data for each scenario
darts_thrown_list = []
pi_diff_list = []

# We'll throw a number of darts
for i in range(num_darts):
    # Generate random x, y coordinates between -1 and 1
    x, y = random.uniform(-1, 1), random.uniform(-1, 1)

    # Check if the dart is inside the circle
    # A dart is inside the circle if the distance from the origin (0,0) is less than or equal to 1
    if math.sqrt(x**2 + y**2) <= 1:
        darts_in_circle += 1

    # If it's time to record a scenario
    if (i + 1) % darts_per_scenario == 0:
        # Estimate pi with Monte Carlo method
        # The estimate is 4 times the number of darts in the circle divided by the total number of darts
        pi_estimate = 4 * darts_in_circle / (i + 1)

        # Record the number of darts thrown and the difference between the estimated and actual values of pi
        darts_thrown_list.append((i + 1) / 1000)  # Dividing by 1000 to display in thousands
        pi_diff_list.append(abs(pi_estimate - math.pi))

# Create a scatter plot of the data
fig = go.Figure(data=go.Scattergl(x=darts_thrown_list, y=pi_diff_list, mode='markers'))

# Update the layout of the plot
fig.update_layout(
    title="Fig2h: Difference between Estimated and Actual Pi vs. Number of Darts Thrown (in thousands)",
    xaxis_title="Number of Darts Thrown (in thousands)",
    yaxis_title="Difference between Estimated and Actual Pi",
)

# Display the plot
fig.show()
# Save the plot as a png
pio.write_image(fig2h, "fig2h.png")
```

![](img/7b8d1e0d699a105a98436848cde70ada.png)

# 蒙特卡罗模拟与超参数调优：完美组合

你可能会想，“蒙特卡罗是一个有趣的统计工具，但它如何应用于机器学习呢？”简短的回答是：有很多种方式。蒙特卡罗模拟在机器学习中的一个应用就是超参数调优。

超参数是我们（人类）在设置机器学习算法时调整的旋钮和拨盘。它们控制算法行为的方面，这些方面关键的是从数据中无法学习到的。例如，在决策树中，树的最大深度是一个超参数。在神经网络中，学习率和隐藏层的数量是超参数。

选择合适的超参数可以决定一个模型表现差异与表现优异之间的差别。但是我们如何知道选择哪些超参数呢？这就是蒙特卡罗模拟的作用所在。

传统上，机器学习从业者使用网格搜索或随机搜索等方法来调整超参数。这些方法涉及为每个超参数指定一组可能的值，然后为每一个可能的超参数组合训练和评估模型。这可能会非常耗费计算资源和时间，尤其是当有许多超参数需要调整或每个超参数可以取的值范围很大时。

蒙特卡洛模拟提供了一个更高效的替代方案。我们可以根据某种概率分布从超参数空间中随机采样，而不是穷尽所有可能的超参数组合。这使我们能够更有效地探索超参数空间，并更快地找到好的超参数组合。

在下一部分，我们将使用一个真实数据集来演示如何在实践中使用蒙特卡洛模拟进行超参数调整。让我们开始吧！

# 蒙特卡洛模拟用于超参数调整

## 实验的核心：心脏病数据集

在机器学习的世界里，数据是驱动我们模型的命脉。为了探索蒙特卡洛模拟在超参数调整中的应用，让我们来看一个与心脏息息相关的数据集——字面上的意思。[心脏病数据集](https://archive.ics.uci.edu/dataset/45/heart+disease)（CC BY 4.0）来自 UCI 机器学习库，是一些患者的医学记录，其中有些人患有心脏病。

数据集包含 14 个属性，包括年龄、性别、胸痛类型、静息血压、胆固醇水平、空腹血糖等。目标变量是心脏病的存在，使这是一个二分类任务。由于包含了分类和数值特征，这是一个有趣的数据集，用于演示超参数调整。

首先，让我们查看一下我们的数据集，以了解我们将要处理的内容——总是一个好的开始。

```py
 #Load and view first few rows of dataset

# Import necessary libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV
from sklearn.metrics import roc_auc_score
import numpy as np
import plotly.graph_objects as go

# Load the dataset
# The dataset is available at the UCI Machine Learning Repository
# It's a dataset about heart disease and includes various patient measurements
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data"

# Define the column names for the dataframe
column_names = ["age", "sex", "cp", "trestbps", "chol", "fbs", "restecg", "thalach", "exang", "oldpeak", "slope", "ca", "thal", "target"]

# Load the dataset into a pandas dataframe
# We specify the column names and also tell pandas to treat '?' as NaN
df = pd.read_csv(url, names=column_names, na_values="?")

# Print the first few rows of the dataframe
# This gives us a quick overview of the data
print(df.head())
```

这展示了数据集中所有列的前四个值。如果你加载了正确的 csv 文件并且像我一样命名了你的列，你的输出将会类似于**图 3**。

![](img/825060ccf8474b6394f434b6ad7ad8c1.png)

图 3：数据集中前 4 行的数据显示

# 设定脉搏：数据预处理

在我们可以使用心脏病数据集进行超参数调整之前，我们需要对数据进行预处理。这涉及几个步骤：

1.  处理缺失值：数据集中有些记录缺失了值。我们需要决定如何处理这些缺失值，可以通过删除记录、填补缺失值或其他方法。

1.  编码分类变量：许多机器学习算法要求输入数据为数值型。我们需要将分类变量转换为数值格式。

1.  规范化数值特征：机器学习算法在数值特征处于类似尺度时通常表现更好。我们将应用规范化来调整这些特征的尺度。

首先处理缺失值。在我们的心脏病数据集中，‘ca’和‘thal’列中有一些缺失值。我们将用各自列的中位数填补这些缺失值。这是一种处理缺失数据的常见策略，因为它不会严重影响数据的分布。

接下来，我们将对分类变量进行编码。在我们的数据集中，‘cp’，‘restecg’，‘slope’，‘ca’和‘thal’列是分类的。我们将使用标签编码将这些分类变量转换为数值型的。标签编码将列中的每个唯一类别分配给不同的整数。

最后，我们将规范化数值特征。规范化调整数值特征的尺度，使它们都落在类似的范围内。这可以帮助提高许多机器学习算法的性能。我们将使用标准化来进行规范化，将数据转化为均值为 0，标准差为 1。

这是执行所有这些预处理步骤的 Python 代码：

```py
# Preprocess

# Import necessary libraries
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import LabelEncoder

# Identify missing values in the dataset
# This will print the number of missing values in each column
print(df.isnull().sum())

# Fill missing values with the median of the column
# The SimpleImputer class from sklearn provides basic strategies for imputing missing values
# We're using the 'median' strategy, which replaces missing values with the median of each column
imputer = SimpleImputer(strategy='median')

# Apply the imputer to the dataframe
# The result is a new dataframe where missing values have been filled in
df_filled = pd.DataFrame(imputer.fit_transform(df), columns=df.columns)

# Print the first few rows of the filled dataframe
# This gives us a quick check to make sure the imputation worked correctly
print(df_filled.head())

# Identify categorical variables in the dataset
# These are variables that contain non-numerical data
categorical_vars = df_filled.select_dtypes(include='object').columns

# Encode categorical variables
# The LabelEncoder class from sklearn converts each unique string into a unique integer
encoder = LabelEncoder()
for var in categorical_vars:
    df_filled[var] = encoder.fit_transform(df_filled[var])

# Normalize numerical features
# The StandardScaler class from sklearn standardizes features by removing the mean and scaling to unit variance
scaler = StandardScaler()

# Apply the scaler to the dataframe
# The result is a new dataframe where numerical features have been normalized
df_normalized = pd.DataFrame(scaler.fit_transform(df_filled), columns=df_filled.columns)

# Print the first few rows of the normalized dataframe
# This gives us a quick check to make sure the normalization worked correctly
print(df_normalized.head())
```

第一个打印语句显示了原始数据集中每列的缺失值数量。在我们的情况下，‘ca’和‘thal’列中有一些缺失值。

第二个打印语句显示了填补缺失值后的数据集前几行。如前所述，我们使用了每列的中位数来填补缺失值。

第三个打印语句显示了对分类变量进行编码后数据集的前几行。经过这一步骤，我们的数据集中的所有变量都是数值型的。

最后的打印语句显示了在规范化数值特征后数据集的前几行，其中数据的均值为 0，标准差为 1。经过这一步骤，我们数据集中的所有数值特征都在类似的尺度上。检查您的输出是否类似于**图 4**：

![](img/accf0ab79ef4c394e9b4916bee3d6601.png)

图 4：预处理打印语句输出

运行这段代码后，我们得到了一个已经过预处理的数据集，准备好进行建模。

# 实现一个基本的机器学习模型

现在我们已经预处理了数据，准备实施一个基本的机器学习模型。这将作为我们的基准模型，之后我们将尝试通过超参数调优来改进它。

我们将使用一个简单的逻辑回归模型来完成这个任务。请注意，尽管它被称为“回归”，但它实际上是处理二分类问题（如我们在心脏病数据集中遇到的）的最受欢迎的算法之一。这是一个线性模型，用于预测正类的概率。

在训练我们的模型后，我们将使用两个常见指标来评估其性能：准确度和 ROC-AUC。准确度是所有预测中正确预测的比例，而 ROC-AUC（接收者操作特征曲线—曲线下面积）衡量真实正率和假正率之间的权衡。

那么这与蒙特卡洛模拟有什么关系呢？好吧，像逻辑回归这样的机器学习模型有几个可以调整的超参数，以提高性能。然而，找到最佳的超参数组合就像在大海捞针。这就是蒙特卡洛模拟的用武之地。通过随机采样不同的超参数组合并评估其性能，我们可以估算出良好超参数的概率分布，并对使用哪些最佳参数做出有根据的猜测，类似于我们在掷飞镖练习中挑选更好π值的方式。

这是实现并评估我们新预处理数据的基本逻辑回归模型的 Python 代码：

```py
# Logistic Regression Model - Baseline

# Import necessary libraries
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, roc_auc_score

# Replace the 'target' column in the normalized DataFrame with the original 'target' column
# This is done because the 'target' column was also normalized, which is not what we want
df_normalized['target'] = df['target']

# Binarize the 'target' column
# This is done because the original 'target' column contains values from 0 to 4
# We want to simplify the problem to a binary classification problem: heart disease or no heart disease
df_normalized['target'] = df_normalized['target'].apply(lambda x: 1 if x > 0 else 0)

# Split the data into training and test sets
# The 'target' column is our label, so we drop it from our features (X)
# We use a test size of 20%, meaning 80% of the data will be used for training and 20% for testing
X = df_normalized.drop('target', axis=1)
y = df_normalized['target']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Implement a basic logistic regression model
# Logistic Regression is a simple yet powerful linear model for binary classification problems
model = LogisticRegression()
model.fit(X_train, y_train)

# Make predictions on the test set
# The model has been trained, so we can now use it to make predictions on unseen data
y_pred = model.predict(X_test)

# Evaluate the model
# We use accuracy (the proportion of correct predictions) and ROC-AUC (a measure of how well the model distinguishes between classes) as our metrics
accuracy = accuracy_score(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)

# Print the performance metrics
# These give us an indication of how well our model is performing
print("Baseline Model " + f'Accuracy: {accuracy}')
print("Baseline Model " + f'ROC-AUC: {roc_auc}')
```

![](img/ab7fe1b123a842dc37e015e81ef67a84.png)

我们的基本逻辑回归模型具有 0.885 的准确度和 0.884 的 ROC-AUC 分数，为我们提供了一个坚实的基准，以便进一步改进。这些指标表明我们的模型在区分有心脏病和无心脏病的患者方面表现相当出色。让我们看看是否能进一步提高。

# 使用网格搜索进行超参数调整

在机器学习中，模型的性能通常可以通过调整其超参数来提高。超参数是从数据中未学习到的参数，而是在学习过程开始之前设置的。例如，在逻辑回归中，正则化强度‘C’和惩罚类型‘l1’或‘l2’都是超参数。

让我们使用网格搜索对逻辑回归模型进行超参数调整。我们将调整‘C’和‘penalty’超参数，并使用 ROC-AUC 作为我们的评分指标。让我们看看能否超越基准模型的表现。

现在，让我们开始这一部分的 Python 代码。

```py
# Grid Search

# Import necessary libraries
from sklearn.model_selection import GridSearchCV

# Define the hyperparameters and their values
# 'C' is the inverse of regularization strength (smaller values specify stronger regularization)
# 'penalty' specifies the norm used in the penalization (l1 or l2)
hyperparameters = {'C': [0.001, 0.01, 0.1, 1, 10, 100, 1000], 
                   'penalty': ['l1', 'l2']}

# Implement grid search
# GridSearchCV is a method used to tune our model's hyperparameters
# We pass our model, the hyperparameters to tune, and the number of folds for cross-validation
# We're using ROC-AUC as our scoring metric
grid_search = GridSearchCV(LogisticRegression(), hyperparameters, cv=5, scoring='roc_auc')
grid_search.fit(X_train, y_train)

# Get the best hyperparameters
# GridSearchCV has found the best hyperparameters for our model, so we print them out
best_params = grid_search.best_params_
print(f'Best hyperparameters: {best_params}')

# Evaluate the best model
# GridSearchCV also gives us the best model, so we can use it to make predictions and evaluate its performance
best_model = grid_search.best_estimator_
y_pred_best = best_model.predict(X_test)
accuracy_best = accuracy_score(y_test, y_pred_best)
roc_auc_best = roc_auc_score(y_test, y_pred_best)

# Print the performance metrics of the best model
# These give us an indication of how well our model is performing after hyperparameter tuning
print("Grid Search Method " + f'Accuracy of the best model: {accuracy_best}')
print("Grid Search Method " + f'ROC-AUC of the best model: {roc_auc_best}')
```

![](img/362c2f7a58ab5e1a8870ca80e3d86d16.png)

经过网格搜索发现最佳超参数为{‘C’: 0.1, ‘penalty’: ‘l2’}，我们最佳模型的准确度为 0.852，ROC-AUC 分数为 0.853。有趣的是，这一表现稍低于我们的基准模型。这可能是因为基准模型的超参数已经非常适合这个数据集，或者可能是由于训练-测试划分中的随机性所致。不管怎样，这都提醒我们更复杂的模型和技术并不总是更好。

不过，你可能也注意到，我们的网格搜索只探索了相对较少的超参数组合。在实际操作中，超参数及其潜在值的数量可能会大得多，从而使网格搜索计算上昂贵甚至不可行。

这就是蒙特卡罗方法的作用。让我们看看这种更有指导性的方法是否能改善原始基线或基于网格搜索的模型的表现：

```py
#Monte Carlo

# Import necessary libraries
from sklearn.metrics import accuracy_score, roc_auc_score
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
import numpy as np

# Split the data into training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Define the range of hyperparameters
# 'C' is the inverse of regularization strength (smaller values specify stronger regularization)
# 'penalty' specifies the norm used in the penalization (l1 or l2)
C_range = np.logspace(-3, 3, 7)
penalty_options = ['l1', 'l2']

# Initialize variables to store the best score and hyperparameters
best_score = 0
best_hyperparams = None

# Perform the Monte Carlo simulation
# We're going to perform 1000 iterations. You can play with this number to see how the performance changes.
# Remember the Law of Large Numbers!
for _ in range(1000):  

    # Randomly select hyperparameters from the defined range
    C = np.random.choice(C_range)
    penalty = np.random.choice(penalty_options)

    # Create and evaluate the model with these hyperparameters
    # We're using 'liblinear' solver as it supports both L1 and L2 regularization
    model = LogisticRegression(C=C, penalty=penalty, solver='liblinear')
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)

    # Calculate the accuracy and ROC-AUC
    accuracy = accuracy_score(y_test, y_pred)
    roc_auc = roc_auc_score(y_test, y_pred)

    # If this model's ROC-AUC is the best so far, store its score and hyperparameters
    if roc_auc > best_score:
        best_score = roc_auc
        best_hyperparams = {'C': C, 'penalty': penalty}

# Print the best score and hyperparameters
print("Monte Carlo Method " + f'Best ROC-AUC: {best_score}')
print("Monte Carlo Method " + f'Best hyperparameters: {best_hyperparams}')

# Train the model with the best hyperparameters
best_model = LogisticRegression(**best_hyperparams, solver='liblinear')
best_model.fit(X_train, y_train)

# Make predictions on the test set
y_pred = best_model.predict(X_test)

# Calculate and print the accuracy of the best model
accuracy = accuracy_score(y_test, y_pred)
print("Monte Carlo Method " + f'Accuracy of the best model: {accuracy}')
```

![](img/1235ce9445d8466f6cd31a496d0321cf.png)

在蒙特卡罗方法中，我们发现最佳 ROC-AUC 分数为 0.9014，最佳超参数为{‘C’: 0.1, ‘penalty’: ‘l1’}。最佳模型的准确率为 0.9016。

# **两种技术的故事：网格搜索与蒙特卡罗**

网格搜索通过评估超参数空间中所有可能组合在训练集上的性能，并根据预定义的指标（例如，准确率、ROC-AUC 等）选择产生最佳平均性能的组合，从而选择“最佳”超参数。这涉及将训练数据分成几个子集（或“折叠”，我们在代码中设置为 5），在一些折叠上训练模型，在其余折叠上评估模型，然后在所有折叠中平均性能。这被称为交叉验证，有助于通过确保模型在不同训练数据子集上的平均表现良好来减少过拟合。

相比之下，蒙特卡罗方法不会对不同训练数据子集进行任何平均。它随机选择超参数，在整个训练集上评估模型，然后选择在测试集上产生最佳性能的超参数。这种方法不使用任何交叉验证，因此不会对不同训练数据子集的性能进行平均。

在上述实验中，尽管网格搜索方法评估了蒙特卡罗方法选择的组合，但由于该组合在交叉验证过程中未能在不同训练数据子集上产生最佳平均性能，因此未被选为“最佳”超参数。然而，蒙特卡罗方法选择的组合恰好在本实验中使用的特定测试集上表现更好。这表明所选超参数在特定测试集的特征上表现良好，即使它们在不同训练数据子集上的平均表现不是最好。

这突显了网格搜索方法中通过对不同训练数据子集进行平均引入的偏差，与蒙特卡罗方法中通过在整个训练集上评估模型并根据单个测试集选择超参数引入的方差之间的权衡。

我鼓励你动手尝试 Python 代码，观察不同因素如何影响性能。你还可以比较两种方法在不同超参数空间下的计算时间，以了解它们的效率。这个练习旨在展示这些方法的动态，它们各有优缺点，并突出“最佳”方法可能取决于你的具体场景、计算资源和模型要求。因此，随意实验，祝学习愉快！

# 结论

起源于纸牌游戏的蒙特卡罗方法，无疑重新塑造了计算数学和数据科学的格局。它的力量在于其简洁性和多功能性，使我们能够相对轻松地处理复杂的高维问题。从通过掷飞镖估计π的值到在机器学习模型中调整超参数，蒙特卡罗模拟已经证明了其在数据科学武器库中的宝贵价值。

在这篇文章中，我们从蒙特卡罗方法的起源出发，了解了它的理论基础，并探讨了其在机器学习中的实际应用。我们看到了它如何用来优化机器学习模型，并通过使用真实数据集进行超参数调整的实践探索。我们还与其他方法进行了比较，展示了它的效率和有效性。

但蒙特卡罗的故事远未结束。随着我们不断推动机器学习和数据科学的边界，蒙特卡罗方法无疑将继续发挥重要作用。无论我们是在开发复杂的 AI 应用、理解复杂数据，还是只是玩一局纸牌游戏，蒙特卡罗方法都证明了模拟和近似在解决复杂问题中的强大力量。

当我们向前迈进时，让我们花一点时间欣赏这个方法的美妙——一个起源于简单纸牌游戏的方法，却能够驱动世界上最先进的计算。蒙特卡罗方法确实是一场高风险的机会与复杂性的游戏，到目前为止，它似乎总能获胜。因此，继续洗牌，继续打牌，并记住——在数据科学的游戏中，蒙特卡罗可能就是你手中的王牌。

# 离别感想

恭喜你成功到达终点！我们已经探索了概率的世界，挣扎于复杂的模型，并对蒙特卡罗模拟的强大能力有了新的认识。我们见证了它们如何将复杂的问题简化为可管理的组件，甚至优化了机器学习任务的超参数。

如果你像我一样喜欢深入探讨机器学习问题的复杂性，请关注我在 [Medium](https://medium.com/@sydneynye) 和 [LinkedIn](http://www.linkedin.com/comm/mynetwork/discovery-see-all?usecase=PEOPLE_FOLLOWS&followMember=sydney-nye) 上。让我们一起在人工智能的迷宫中，一次解决一个巧妙的问题。

直到我们的下一个统计冒险，继续探索，继续学习，继续模拟！在你数据科学和机器学习的旅程中，愿好运常伴你左右。

*注意：除非另有说明，所有图片均由作者提供。*
