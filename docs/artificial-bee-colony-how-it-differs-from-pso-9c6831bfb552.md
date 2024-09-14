# 人工蜂群 — 它与粒子群优化的不同之处

> 原文：[`towardsdatascience.com/artificial-bee-colony-how-it-differs-from-pso-9c6831bfb552`](https://towardsdatascience.com/artificial-bee-colony-how-it-differs-from-pso-9c6831bfb552)

## ABC 的直觉和代码实现，以及探索其超越粒子群优化的地方

[](https://medium.com/@byjameskoh?source=post_page-----9c6831bfb552--------------------------------)![詹姆斯·柯，博士](https://medium.com/@byjameskoh?source=post_page-----9c6831bfb552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c6831bfb552--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c6831bfb552--------------------------------) [詹姆斯·柯，博士](https://medium.com/@byjameskoh?source=post_page-----9c6831bfb552--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c6831bfb552--------------------------------) ·10 分钟阅读·2023 年 12 月 18 日

--

![](img/5bdc467ff2ee97b8a9bbc611dd5120fc.png)

图像由 DALL·E 3 基于“绘制一个科幻主题的蜜蜂对抗战斗”的提示生成。

我在近期文章中分享了粒子群优化（PSO）的直觉、实现及其有用性，作为我自然启发算法系列的一部分。今天，我将解释人工蜂群（ABC）如何工作。

蜜蜂不就是群体的一部分吗？这两种算法难道只是同一枚硬币的两面？

对于这篇文章，我将直接进入 ABC 的直觉部分。接下来，我将提供数学基础，然后介绍 Python 实现。最后，我将制定一个 PSO 无法解决但 ABC 轻松解决的问题，并解释使这一切成为可能的 ABC 方面。

# 直觉

就像在强化学习和进化算法的情况下，ABC 的一个基本驱动因素是探索与开发之间的平衡。

对于初次接触群体智能算法的人来说，最初可能会被生物学的联系所吓到，认为需要一些复杂的数学建模来模拟自然界中实际发生的情况。由于教科书中变量通常用希腊字母表示，这增加了这种复杂性的误解。

至少对于 ABC 来说情况并非如此。你不需要了解蜜蜂的摇摆舞，也没有超出高中数学的内容。

实质上，它只是对有前景的位置进行局部方向性搜索，仅在目标函数有所改进时保存结果，并在遇到长时间没有进展时进行全局随机搜索。

该算法的创作者随后为其起了华丽的名字，并将这些标签分别附加给雇佣蜂、旁观蜂和侦查蜂。

# 解的形成

像 PSO 一样，ABC 是一种元启发式算法？

什么是“元启发式方法”，你可能会问？

让我们从一个启发式解决方案开始——这是一种技术，尽管不保证最优，但通常在实践中表现良好，能提供足够好的结果。

“元”在“元启发式方法”中不是指 Facebook。它是一种“高级”的启发式方法——它是可以应用于不同类型问题的一种通用策略。因此，理解这些原则对你有利；数学部分将会轻松跟随。

## 数学

考虑一个具有 *D* 维度的解空间的问题。初始化时，使用 [1] 生成 *N* 个解决方案。

![](img/57ca7fb2f647e9820dfb70e1d96130e6.png)

实际上，简单地用向量或矩阵形式表示更为方便。

![](img/1a42f7aa5e770d0dde7685a5c66d4b6b.png)

‘**雇佣蜂**’阶段只是围绕每个源 *i* 进行局部方向性搜索，相对于随机选择的邻居 *k*。词语‘邻居’可能会误导——它仅仅是另一个候选解决方案，并不一定需要在附近。与在局部搜索中进行随机高斯扰动相比，这种方向性方面通常在实践中表现更好。（这就是元启发式方法的核心所在。）

![](img/3e6e920d9bd5a51b41ef5a88474ad9a0.png)

***ϕ*** 是从 [-1,1] 的均匀分布中抽取的随机数向量，即。

```py
xi_hat = xi + np.random.uniform(-1, 1, D) * (xi - xk)
```

尽管邻居 *k* 是随机选择的，但请记住，每个邻居在其自身的权利下都是潜在的良好解决方案。你可能会读到，ABC 的原始形式实际上仅在一个维度 *j* 上引入扰动。然而，在实践中，同时在多个维度上进行扰动可以减少迭代次数。（这本质上是进行更积极的探索——如同启发式方法，没有“不可置疑的真理”可循；我们只是寻找有效的方法。）

![](img/e38578933f2ca7c4e2d507aa5058e350.png)

（蜂群图像来源于 DALL·E 3；由作者整理。）考虑二维空间中的解 i 和邻居 k。假设 phi，即随机向量为 [0.5, 0.1]。这意味着解 i 将在第一个维度（水平）上调整 50% 朝向 k，在第二个维度（垂直）上调整 10% 朝向 k——如果 x 处的目标值高于 i 处的目标值，则会保存并更新。

接下来，我们进入‘**旁观者蜜蜂**’阶段。根据适应度（例如目标函数的值）评估每个候选解，并根据相对适应度的概率分布选择其中之一。

如同[进化算法](https://medium.com/towards-data-science/evolutionary-algorithm-selections-explained-2515fb8d4287)的情况一样，并非绝对必要保持轮盘选择，还有其他选择技术，如锦标赛选择（有关详细信息，请参见链接文章的第 3.2.2 节）。

在第二阶段，有更强的开发元素，因为‘邻居’可能具有较好的适应度，并且相对于它进行定向搜索。

![](img/b3c94f418b83f6a181a9aa170d0708f3.png)

（DALL·E 3 生成的蜜蜂图片；由作者整理。）在‘旁观者蜜蜂’阶段进行搜索。对 x 处的解进行修改，相对于选择的 s 处解。请注意，新关注点的评估不一定在实心蓝色双箭头上。相反，它可以在蓝色三角形内的任何位置（如果你使用一个从[0,1]均匀分布中抽取随机数的修改 phi），甚至在从[-1,1]分布中抽取的绿色三角形内。

在‘雇佣蜜蜂’和‘旁观者蜜蜂’阶段，如果新解（即类比中的食物源）没有比现有解更好，则相应的计数器增加 1。另一方面，如果有改进，则更新解，下一次搜索将以此新位置为基础。

之后是‘**侦查蜜蜂**’阶段。如果计数器超过预定义的阈值，相应的候选者将被淘汰，并在整个解空间中随机重新初始化一个新候选者。在蜜蜂的类比中，这个过程代表了食物源的枯竭。在数据科学中，这意味着我们将候选者视为局部最优（也可能是全局最优）。

## 代码实现

参考文献[1]中的 C++实现实际上跨越了三整页，包含了近 200 行代码。

我将提供一个少于 60 行的简单 Python 实现，代码易于阅读。更重要的是，它有效。

ABC 的基本单位是`Bee`，它类似于 PSO 中的粒子，因为它代表一个候选解。

```py
class Bee:
    def __init__(self, n_dim, param_limits=(-5, 5)):
        self.low = param_limits[0]
        self.high = param_limits[1]
        self.pos = np.random.uniform(
            low=self.low, high=self.high, size=(n_dim,)
        )
        self.fitness = -1e8
        self.counter = 0

    def explore(self, neighbour, task):
        phi = np.random.uniform(-1, 1, len(self.pos))
        new_pos = self.pos + phi * (self.pos - neighbour.pos)
        new_pos = np.clip(new_pos, self.low, self.high)

        new_fitness = task.score(new_pos)
        if new_fitness > self.fitness:
            self.pos = new_pos
            self.fitness = new_fitness
            self.counter = 0
        else:
            self.counter += 1
```

每个`Bee`都有一个`explore`方法，它根据另一个候选者更新其位置。更新与此新位置对应的适应度。如果超过之前的记录，将更新其`pos`和`fitness`属性，否则`self.counter`增加 1。

第二个也是最后一个类是`Colony`，它包含了多个`Bee`实例。

```py
 class Colony:
    def __init__(self, n_dim, n_population, param_limits):
        self.n_dim = n_dim
        self.bees = [Bee(n_dim, param_limits) for _ in range(n_population)]
        self.best_solution = np.zeros(n_dim)
        self.best_fitness = -1e8
        self.limit = 10

    def solve(self, task, num_iterations):
        for _ in tqdm(range(num_iterations)):
            # 'Employed bee' Phase
            for bee in self.bees:
                partner_idx = np.random.randint(len(self.bees))
                bee.explore(self.bees[partner_idx], task)

            # 'Onlooker bee' Phase
            fitnesses = np.array([bee.fitness for bee in self.bees])
            probs = fitnesses / fitnesses.sum()
            for _ in range(len(self.bees)):
                bee_idx = np.random.choice(range(len(self.bees)), p=probs)
                self.bees[bee_idx].explore(np.random.choice(self.bees), task)

            # 'Scout bee' Phase & Update best solution
            for bee in self.bees:
                if bee.counter > self.limit:
                    bee = Bee(self.n_dim, (bee.low, bee.high))
                if bee.fitness > self.best_fitness:
                    self.best_fitness = bee.fitness
                    self.best_solution = deepcopy(bee.pos)        
        return self.best_solution
```

注意到在每次迭代中，每只`Bee`类的蜜蜂都会执行‘雇佣蜂’阶段、‘观察蜂’阶段以及‘侦查蜂’阶段。它们不是不同的实体。本质上，我们只是跨每次迭代实现了探索和开发的元素。

# 既然我们有 PSO，为什么还需要 ABC？

到目前为止，我们已经了解了人工蜂群的直觉，并对数学和代码实现进行了逐步讲解。

作为一种元启发式算法，ABC 可以以多种不同方式使用。然而，让我们专注于一个特定场景，在该场景中，ABC 相对于 PSO 具有优势，这也是我在上一篇文章最后一段中承诺的内容。

ABC 非常有能力摆脱局部最优，不会被‘困住’。

为什么 ABC 能够解决 PSO 无法解决的问题？既然我们在 ABC 和 PSO 中都维持了一个候选解的种群，它们不应该以相同的方式工作吗？

1.  关键原因在于‘侦查蜂’阶段，它在进展减缓时会随机探索解决空间的新区域。

1.  此外，‘雇佣蜂’和‘观察蜂’阶段的探索比 PSO 更具攻击性（有可能不仅*朝向*而且*远离*其他候选点），而 PSO 则倾向于*朝向*全局最优和局部最优。

当然，通过远离另一个候选点，我们可能确实是在倒退而不是前进。这给我们一个重要的原则 — 我们只保留好的东西。（这类似于为什么[进化算法中的突变](https://medium.com/towards-data-science/evolutionary-algorithm-mutations-explained-4a3b5c2d49de)会导致改进）。在`Bee.explore`中，注意到`self.pos`仅在`self.fitness`有所改善时才会被覆盖。

为了说明 ABC 的有效性，让我们创建一个具有多个局部最优点的目标函数，并使用 ABC 和 PSO 两者来求解它。

# 问题的公式化

考虑一个如下所示的简单二维问题，由以下代码生成。（请注意，这个函数本身是一个有用的学习点，对于那些想要创建测试优化算法环境的人来说。）

```py
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
x, y = np.meshgrid(x, y)

def multipeak(x, y):
    peak1 = 0.5 * np.exp(-((x-1)**2 + (y-1)**2))
    peak2 = 0.6 * np.exp(-((x+1)**2 + (y-1)**2))
    peak3 = 0.7 * np.exp(-((x-1)**2 + (y+1)**2))
    peak4 = 0.8 * np.exp(-((x+3)**2 + (y+3)**2))
    return np.maximum.reduce([peak1, peak2, peak3, peak4])

z = multipeak(x, y)
print("z.shape: ", z.shape)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
surface = ax.plot_surface(x, y, z, cmap='inferno')

ax.set_xlabel('X')
ax.set_ylabel('Y')
plt.title('Surface Plot')
fig.colorbar(surface, shrink=0.6, aspect=8)
plt.show()
```

![](img/56ac3ee47bdf34aaa265fd256e01a240.png)

具有全局最优值 0.8 和局部最优值 0.5 至 0.7 的函数。图像由作者提供，使用 Python 生成。

我之前在本系列的第一篇[文章](https://medium.com/towards-data-science/particle-swarm-optimization-search-procedure-visualized-4b0364fb3e5a)中介绍了解决高维数学方程的价值主张，以位置规划为例（‘用例’部分；就在开头）。

在上述二维问题中，我们看到有四个峰值，每个峰值的大小略有不同。三个峰值位于搜索空间的中心附近，而第四个峰值（最高的那个）位于远离其他峰值的角落。这样做的目的是展示一个可能会选择局部最优而不是全局最优的问题。

一个现实问题通常具有更高的维度。让我们将上述内容扩展到创建一个 8 维函数。

```py
def full_blackbox(x1, y1, x2, y2, x3, y3, x4, y4):
    peak1 = 0.5 * np.exp(
        -((x1-1)**2 + (y1-1)**2 + (x2-1)**2 + (y2-1)**2 + (x3-1)**2 + (y3-1)**2 + (x4-1)**2 + (y4-1)**2)
    )
    peak2 = 0.6 * np.exp(
        -((x1+1)**2 + (y1-1)**2 + (x2+1)**2 + (y2-1)**2 + (x3+1)**2 + (y3-1)**2 + (x4+1)**2 + (y4-1)**2)
    )
    peak3 = 0.7 * np.exp(
        -((x1-1)**2 + (y1+1)**2 + (x2-1)**2 + (y2+1)**2 + (x3-1)**2 + (y3+1)**2 + (x4-1)**2 + (y4+1)**2)
    )
    peak4 = 0.8 * np.exp(
        -((x1+2)**2 + (y1+2)**2 + (x2+2)**2 + (y2+2)**2 + (x3+2)**2 + (y3+2)**2 + (x4+2)**2 + (y4+2)**2)
    )
    return np.maximum.reduce([peak1, peak2, peak3, peak4])
```

这里，x1、y1、x2、y2、x3、y3、x4、y4 只是一个向量的不同*维度*。你可以把这些看作四个位置的坐标，它们共同构成了解决方案。

可以看到，点 (1,1,1,1,1,1,1,1)、(-1,1,-1,1,-1,1,-1,1) 和 (1,-1,1,-1,1,-1,1,-1) 分别是局部最优点，其值为 0.5、0.6 和 0.7。同时，全球最优点位于 (-2,-2,-2,-2,-2,-2,-2,-2)，目标值为 0.8。

根据我一贯的面向对象的做法，我们来定义以下类。

```py
class Task:
    def __init__(self):
        pass

    def score(self, x_arr, with_noise=False):
        if with_noise:
            return full_blackbox(*x_arr) + 0.1*np.random.randn()
        else:
            return full_blackbox(*x_arr)
```

这使得它不仅与当前的 ABC 代码兼容，还与上述链接文章中共享的 PSO 代码兼容。

# 结果

使用建立的类，解决用例只需几行代码。

对于 PSO：

```py
task = Task()
swarm = Swarm(n_dim=8, n_population=2000, param_limits=(-5, 5))
solution = swarm.solve(task, 500)

print(solution)
print(task.score(solution))
```

![](img/d6d528f36d416bb52d2370755945ddd5.png)

粒子群优化的结果（作者截图）。在 52 秒内获得了 0.7 的目标值。

对于 ABC：

```py
task = Task()
colony = Colony(n_dim=8, n_population=2000, param_limits=(-5, 5))
best_solution = colony.solve(task, 500)

print(best_solution)
print(task.score(best_solution))
```

![](img/5258a84826db08c578456f5105f25442.png)

人工蜂群的结果（作者截图）。在 20 秒内获得了 0.8 的目标值。

我们这里有一个赢家！不仅 ABC 找到了真实的全局最优点，而且用时不到一半。自己试试看吧！

# 结论

我们看到，作为一种元启发式算法，人工蜂群在平衡探索和利用方面非常有效。本文展示了涉及的数学只是简单的线性代数，并且直觉上实际上不需要任何生物学或蜜蜂的知识。

使用此方法，你可以用几行代码解决多维优化问题，并在几秒钟内获得最优解。

恭喜！又一个实用的工具在你的工具箱里！

# 参考文献

[1] B. Akay 和 D. Karaboga，《人工蜂群算法》在[群体智能算法](https://books.google.com.sg/books?hl=en&lr=&id=v6P0DwAAQBAJ)一书中（2020 年）CRC 出版社。
