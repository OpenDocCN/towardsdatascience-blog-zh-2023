# 使用 Python 模拟物理系统

> 原文：[`towardsdatascience.com/simulating-physical-systems-with-python-dd5751e80b5c`](https://towardsdatascience.com/simulating-physical-systems-with-python-dd5751e80b5c)

## 任何工程师或科学家的必备技能

[](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)![Nick Hemenway](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------) [Nick Hemenway](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------) ·20 分钟阅读·2023 年 3 月 7 日

--

![](img/379eaf6912584be658e3fa37c4f517b0.png)

照片由[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

模拟物理系统的行为在几乎所有科学和工程领域都有不可思议的实用价值。模拟允许我们理解系统的时间演变，而且这种方式通常无法与物理测试相媲美。虽然物理测试可能耗时、昂贵且可能存在危险，但模拟则快速、廉价，并且不会对设备造成损坏或对人身安全构成风险。实验也有其局限性，因为它们只能提供你实际可以测量的数据——未测量的系统状态要么无法获得，要么需要在后处理过程中估计。另一方面，模拟如同窗口，允许我们窥视系统的整个内部状态。这种基本上能查看系统内部运作的能力，可以为工程应用提供宝贵的设计洞察。

本文的目的是提供一个快速入门指南，介绍如何在 Python 编程语言中开始模拟物理系统。为此，我们将通过一个全面的示例来模拟一个弹跳的球。我特别选择了这个系统，因为它直观且不太难，但仍有一些细微之处，使得模拟它比你在网上找到的其他简单示例更有趣（控制球运动的方程会根据球是否在空中或与地面接触而变化）。在这个过程中，我们将查看如何将数学模型转换为模拟所需的正确格式，展示如何利用现有的代码库（SciPy 中的`solve_ivp`）来简化我们的工作，并解释为什么我们以这种方式组织代码。到本文结束时，我们不仅会拥有一个可以运行的弹跳球模拟（如下面的动画所示），还将具备可以扩展到模拟任何物理系统的基础知识。

![](img/af2b26da88b758ec371e38029058f03b.png)

作者提供的动画

## 数学部分

物理世界中的几乎所有系统都可以通过微分方程来建模。这些方程自然地来源于支配我们周围世界的物理定律。以牛顿第二定律为例：

![](img/fb17a82067ee02b8a742800b77b8899c.png)

牛顿第二定律告诉我们，质量的位置的二阶导数与施加在该质量上的净力成正比。于是我们得到一个微分方程——一个解出后会给我们一个函数的方程。对于我们的问题，给定一个强迫函数 f(t)，解决上述微分方程可以得到质量的位置作为时间的函数 x(t)。

那么，我们如何解决上述微分方程呢？不幸的是，我们不能使用通用的解析方法，因为解完全依赖于强迫函数 f(t)。根据强迫函数的形式，解决上述微分方程可能既非常简单，也可能极其困难。这就是模拟发挥作用的地方。模拟为我们提供了一种通用的方法来解决任意困难的微分方程！

那么模拟是如何工作的呢？当我说模拟时，我基本上只是指微分方程的数值积分。存在大量用于此目的的算法，例如非常简单但有时不准确的[欧拉方法](https://en.wikipedia.org/wiki/Euler_method)，或者[龙格-库塔方法](https://lpsa.swarthmore.edu/NumInt/NumIntFourth.html)，这些方法非常流行且更准确。我们不深入探讨这些算法的具体细节，而是将它们视为黑箱，并利用已经实现这些算法的现有代码库。然而，为了做到这一点，我们需要将问题公式化，以符合这些算法所需的格式。

数值积分算法通常要求我们将方程输入为一阶微分方程系统。一般格式如下，其中 t 是时间，x 是系统变量/状态的向量。

![](img/51c2f17cec900cf63e23cfeb8b234311.png)

看着上面的方程，我们可以看到系统中每个状态的变化率受到系统当前状态和瞬时时间的影响。然而，我们的系统的微分方程往往不会自然地符合这种形式（例如，牛顿第二定律不是一阶微分方程）。相反，我们通常需要通过一些代数操作将系统方程转化为这种格式。目前这一切都比较抽象，所以在接下来的部分，我们将通过将方法应用于弹跳球问题来固化这个概念。

## 弹跳球问题

对于弹跳球问题，我们需要考虑两种情况：1) 当球在空中时，和 2) 当球接触地面时。在这两种情况下，我们都会使用牛顿第二定律，但方程的形式会有所变化，因为施加在球上的外力发生了变化。为了公式化运动方程，我们将使用如下坐标系统，其中 x 代表球中心距离地面的高度。

![](img/c7f2f6692a64f2656049bb7e26deac7e.png)

图片作者

## 第一步：推导系统方程

*情况 1) 球在空中时:* 当球在空中时，唯一作用在球上的力是重力，因此我们得到如下公式：

![](img/d12fbd00dccd659a8b56d8fd728c38da.png)

图片作者

*情况 2) 球接触地面时:* 许多不同的接触模型存在于碰撞物体中，但在这个例子中，我们将假设球像一个质量-弹簧-阻尼系统。也就是说，我们将假设球的全部质量集中在球的中心，球在碰撞时会以某种刚度 (k) 和阻尼 (b) 压缩。如下所示：

![](img/3fc464f1ef3f86a30f29bd8d28154d42.png)

图片作者

绘制接触地面的球体的自由体图，我们得到以下图示：

![](img/76594eeae6b2a9519ce9a207828ebe75.png)

图片由作者提供

对球体作用的三种力是重力、阻尼力和球内产生的弹簧力。注意，弹簧（球）的压缩量等于球的半径（R）减去球的位置（x）。稍微调整一下方程，我们得到了著名的质量-弹簧-阻尼微分方程及其强迫项。

![](img/5ad545ed56eb1a89990b2610987be386.png)

## 第 2 步：将方程转换为适合仿真的格式

我们推导出了一组控制球体在飞行和接触地面时运动的微分方程。不幸的是，它们在当前形式下无法用于仿真——我们必须将它们转换为一组一阶方程。为此，我们将引入两个新的变量 x1 和 x2，如下所示：

![](img/43f548d649604e8d8b53ac7c50e4d297.png)

变量 x1 和 x2 被称为状态变量，包含它们的向量称为状态向量。根据定义，状态向量包含了完全定义系统状态所需的所有信息（在我们的例子中，即球的位移和速度）。一般来说，如果我们有一个 n 阶微分方程，我们需要引入 n 个新的状态变量（一个用于 x 及其每一个导数，直到但不包括最高阶导数）。在我们的例子中，我们引入了两个状态变量，因为我们有一个二阶微分方程。

对状态向量进行微分得到：

![](img/9970b494e7c13a8d4dbfa8fce746f584.png)

注意，x1 的导数根据定义是 x2。我故意保留了 x 双点项，因为其表达式依赖于球体是在空中还是接触地面。

对于球体在飞行中的情况，我们有：

![](img/33ed54c63ff84cd93da429495f49ca24.png)

对于球体接触地面的情况，我们有：

![](img/9c6748038db65f25efc0a60f63a4edc2.png)

将两个 x 双点表达式代入上述状态导数方程，得到以下两个向量值的状态导数方程，分别适用于球体在空中和接触时的情况。

![](img/9b9cdad87bea8db7cc562c3f88760efe.png)![](img/8ca166bb84d6e9a5745c2c041d81ecab.png)

此时我们已成功将高阶微分方程转换为一阶方程系统，并准备开始编写仿真代码！

## 第 3 步：代码

首先，我们将导入必要的模块，并根据个人喜好设置一些绘图设置。下面的关键代码行是`from scipy.integrate import solve_ivp`，它导入了我们将用于仿真的求解器（请注意，`ivp`代表“初值问题”，即具有给定初值的微分方程）。

```py
import numpy as np
from scipy.integrate import solve_ivp
import matplotlib.pyplot as plt
import pandas as pd

#set default plotting settings to personal preference
font_settings = {'family':'Times New Roman', 'size':12}
line_settings = {'lw':2}
plt.rc('font', **font_settings)
plt.rc('lines', **line_settings)
```

我们将通过创建一个`BouncingBall`类来组织我们的代码——下方显示了一个模板，其中包括每个方法需要实现的注释。我们将逐个填充这些方法。

```py
class BouncingBall():

    def __init__(self, m, k, b, ball_radius_cm):
        #TODO: initialize the physical properties of the ball
        pass

    def in_air(self,t,x):
        #TODO: compute the in-air physics of the ball (return the 
        # rate of change of the ball's current state)
        pass

    def in_contact(self,t,x):
        #TODO: compute the in-contact physics of the ball (return the
        # rate of change of the ball's current state)
        pass

    def simulate(self, t_span, x0, max_step=0.01):
        #TODO: use the two physics models (`in_air` and `in_contact`) to 
        # actually simulate the trajectory of the ball given a desired time
        # span and initial conditions. Must switch between the two physics 
        # models at the appropriate times
        pass
```

首先，我们将查看`__init__`方法。如下代码所示，`__init__`方法仅接收并存储球体的各种物理参数。如果没有提供重力值，默认分配标准值 9.81 m/s²。

```py
 def __init__(self, m, k, b, ball_radius_cm, gravity=None):
        """
        Parameters
        ----------
        m : float
            ball mass in kg
        k : float
            ball stiffness in N/m
        c : float
            ball viscous damping coef. in N/(m/s)
        ball_radius_cm : float
            ball radius in centimeters
        gravity : float, optional
            acceleration of gravity in m/s²\. Defaults to 9.81 m/s² 
            if None given.
        """

        self.r = ball_radius_cm/100 #radius in meters
        self.m = m
        self.k = k
        self.b = b

        if gravity is None:
            self.g = 9.81 #m/s²
        else:
            self.g = gravity #m/s²
```

接下来，我们将填充两个物理方法（`in_air`和`in_contact`）。这两个方法（如下）接收一个数组`x`和当前时间`t`，将`x`数组解包为`x1`和`x2`，然后根据我们在第 2 步中推导的方程计算并返回状态导数向量。

```py
 def in_air(self,t,x):
        """computes the ball's state derivatives while in air

        Parameters
        ----------
        t : float
            simulation time
        x : array-like
            vector containing the ball's state variables (height and velocity)

        Returns
        -------
        list
            vector containing the ball's state derivatives
        """

        x1, x2 = x #unpack the current state of the ball
        return [x2, -self.g]

    def in_contact(self,t,x):
        """computes the ball's state derivatives while in contact with the ground

        Parameters
        ----------
        t : float
            simulation time
        x : array-like
            vector containing the ball's state variables (height and velocity)

        Returns
        -------
        list
            vector containing the ball's state derivatives
        """

        x1, x2 = x #unpack the current state of the ball
        x1_dot = x2
        x2_dot = -(self.b/self.m)*x2 + (self.k/self.m)*(self.r - x1) - self.g
        return [x1_dot, x2_dot]
```

最后，我们可以填充弹跳球类的`simulate`方法，这将实际执行所有有趣的工作。该方法需要将球体的物理过程向前积分，确保在适当的时候在空中和接触物理模型之间切换。请注意，根据我们定义的坐标系，当球体的位置（x）小于球体的半径（R）时，球体将与地面接触。

切换两个物理模型的最简单方法是定义一个中间方法，根据球体的位置在两个先前定义的方法之间切换。它可能类似于以下内容：

```py
def ball_physics(self, t, x):

    #unpack ball's height and velocity
    x1, x2 = x

    #check if ball's center height is less than ball's radius
    if x1 <= self.R:
        #compute in-contact physics (state derivative)
        return self.in_contact(t,x)
    else:
        #compute in-air physics (state derivative)
        return self.in_air(t,x)
```

然后我们将这个单一的函数传递给数值积分器`solve_ivp`，并将系统在时间上向前推进。不过，这种方法有一个主要问题，就是它无法准确捕捉到物理模型需要变化的接触时刻。下面的图示将加以说明。

![](img/eb02d5d50680f929804cabc11ef74499.png)

作者提供的图像

在时间步 tn，球体刚好在地面上方并向下移动。数值积分器将把球体的轨迹向前积分到时间步 tn+1。请注意，tn 和 tn+1 之间的时间差取决于所使用的求解器类型（固定步长或可变步长）和相应的误差容忍设置。无论如何，在时间 tn+1 时，物理模型将切换到接触模型，但存在一个问题——球体在模型变化之前已经与地面接触。我们不希望每当球体的位置（x）小于球体的半径（R）时，物理模型就改变，我们希望物理模型在球体接触的**确切瞬间**发生变化，即当球体的位置等于球体的半径（x = R）时。为此，我们需要另一种方法，我们必须使用“事件”。

直观地说，“事件”正如其名——在特定时间点发生的“某事”。在我们的案例中，我们将尝试捕捉两个不同的事件：1）球与地面接触的瞬间，以及 2）球离开地面的瞬间。我们通过定义一个在事件发生时等于零的函数来数学描述一个事件（对熟悉 Simulink 的人来说，这与“零交叉”概念相同）。对于我们弹跳球的问题，这只是球的位置与半径之间的差值，如下所示：

![](img/41f2ff68050e62a9598d449a495c704d.png)

当上述函数为零时，球的高度等于球的半径，因此我们有一个接触事件。此外，从下面的图中可以看到，当事件函数在负方向上交叉零（从正向负），这表明球正在与地面接触。同样，当事件函数在正方向上交叉零（从负向正），这表明球离开地面接触。

![](img/ddad56dea6e70ce8150b5107de2a8df1.png)

作者提供的图片

很好，但我们该如何使用这个呢？幸运的是，我们想用的求解器（`solve_ivp`）中已经实现了事件的概念——我们只需正确使用它即可！查看 SciPy 的[文档](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html)我们看到，`solve_ivp`有一个可选参数`events`，它接受一个任意对象。这个对象必须是可调用的，这意味着我们可以像调用函数一样调用它（这正是我们上面定义的事件函数）。该对象还有附加属性`terminal`和`direction`。如果`terminal`属性设置为`True`，当事件发生时，模拟将终止。`direction`属性是一个附加标志，可以设置以确定关注哪个零交叉方向。例如，如果`direction=-1`，只有负方向的交叉（例如，球与地面接触）会被视为事件，正方向的交叉将被忽略。

我们可以构建一个名为`ContactEvent`的类，这个类正好符合文档要求。查看下面的代码，我们看到这个类接收球的半径，并在`__call__`方法中定义事件函数（我们必须在`__call__`方法中实现它，因为`solve_ivp`要求事件对象是可调用的）。`ContactEvent`类还包含属性`direction`和`terminal`。注意，`terminal`设置为`True`，这意味着当事件发生时，模拟将停止。`direction`属性在实例化时设置，因为它取决于我们尝试捕捉的事件类型。

```py
class ContactEvent():
    """Callable class that returns zero when the ball engages/disengages
    contact with the ground.
    """

    def __init__(self, r, direction=0):
        """
        Parameters
        ----------
        r : float
            Radius of the ball in meters
        direction : int, optional
            Direction of a zero crossing to trigger the contact event.
            Negative for the ball coming into contact with the ground.
            Positive for the ball leaving contact with the ground, by default 0
        """

        self.r = r
        self.direction = direction
        self.terminal = True #terminal is True so that simulation will end on contact event

    def __call__(self,t,x):
        """Computes the height of the ball above being in contact

        Notes
        -----
        The ball will engage/disengage contact when the height of the center
        of the ball equals the radius of the ball.

        Parameters
        ----------
        t : float
            time in the simulation
        x : array-like
            vector of ball's state variables (height and velocity)

        Returns
        -------
        float
            height above being in contact
        """
        #unpack height and velocity of ball
        x1, x2 = x
        return x1 - self.r
```

要使用`ContactEvent`类，我们将创建两个`ContactEvent`对象（一个用于球体接触，另一个用于球体离开接触），并将它们存储在`BouncingBall`对象中。为此，我们需要在`__init__`方法中添加两行代码，如下所示：

```py
def __init__(self, m, k, b, ball_radius_cm, gravity=None):
        """
        Parameters
        ----------
        m : float
            ball mass in kg
        k : float
            ball stiffness in N/m
        c : float
            ball viscous damping coef. in N/(m/s)
        ball_radius_cm : float
            ball radius in centimeters
        gravity : float, optional
            acceleration of gravity in m/s²\. Defaults to 9.81 m/s² 
            if None given.
        """

        self.r = ball_radius_cm/100 #radius in meters
        self.m = m
        self.k = k
        self.b = b

        if gravity is None:
            self.g = 9.81 #m/s²
        else:
            self.g = gravity #m/s²

        # create event functions for the ball engaging and disengaging contact
        # note that when coming into contact with the ground, the direction of the
        # zero crossing will be negative (height abve the ground is transitioning 
        # to negative) whereas when the ball leaves the ground the zero crossing 
        # will be positive (height above the ground is transitioning from negative
        # to positive)
        self.hitting_ground = ContactEvent(self.r, direction=-1)
        self.leaving_ground = ContactEvent(self.r, direction=1)
```

随着我们定义的接触事件函数，我们终于可以查看`simulate`方法的代码（如下）。

```py
def simulate(self, t_span, x0, max_step=0.01):
        """Simulates the time evolution of the ball bouncing

        Parameters
        ----------
        t_span : two element tuple/list
            starting and stopping time of the simulation, e.g. (0,10)
        x0 : array-like
            initial conditions of ball (height and velocity in m and m/s)
        max_step : float, optional
            max step size in simulation, by default 0.01

        Returns
        -------
        tuple
            tuple containing the time vector and height of the ball 
        """

        #check the initial height of the ball to determine if it's starting in the air
        in_air = x0[0] > self.r 
        #extract out initial starting and stopping times
        t_start, t_stop = t_span

        #create lists with initial conditions that we can append the piecewise solutions to
        t_lst = [t_start]
        x_lst = [x0[0]]

        #loop until we reach the desired stopping time
        while t_start < t_stop:

            """
            Here we simulate the ball forward in time using either the air 
            or contact model. Each of these subroutines will terminate when 
            either 1) the final desired simulation stop time is reached,
            or 2) when a contact event is triggered. At a high level, we are
            simulating our system forward in time using the relevant physics 
            model (that depends on the state of the system). The simulation 
            will alternate between the "in_air" model and "in_contact" model 
            switching between the two each time a contact event is triggered
            """

            if in_air:
                sol = solve_ivp(self.in_air, [t_start, t_stop], x0, 
                                events=[self.hitting_ground], max_step=max_step)

            else:
                sol = solve_ivp(self.in_contact, [t_start, t_stop], x0, 
                                events=[self.leaving_ground], max_step=max_step) 

            # append solution and time array to list of solutions. 
            # Note that the starting time of each solution
            # is the stopping time of the previous solution. 
            # To avoid having duplicate time points, we will not include the first
            #data point of each simulation. This is also why we created our 
            # solution lists above with the initial conditions already in them
            t_lst.append(sol.t[1::])
            x_lst.append(sol.y[0,1::])

            #set the starting time and initial conditions to the stopping 
            #time and end condtions of the previous loop
            t_start = sol.t[-1]
            x0 = sol.y[:,-1].flatten()

            #if we haven't reached the stopping time yet in the current 
            #loop, we must be switching between being in the air and
            #being in contact
            if t_start < t_stop:
                in_air = not in_air

        #concatenate all of the solutions into a single numpy array
        t = np.hstack(t_lst)
        x = np.hstack(x_lst)

        return t,x
```

从上面的代码可以看出，`simulate`方法接收初始条件和我们希望求解的时间跨度。它首先检查球体是开始在空中还是在地面接触，以确定最初使用哪种物理模型。接下来的代码如下：

1.  创建两个列表（`t_lst`和`x_lst`），我们可以将分段模拟解决方案不断添加到这些列表中。

1.  将球体向前模拟，通过`solve_ivp`传递适当的物理模型和事件对象（注意`solve_ivp`接受一个返回状态导数向量的函数（如第 2 步讨论），初始条件和要解决的时间跨度）。在发生接触事件之前向前模拟，届时模拟停止。将模拟时间和解决方案分别附加到`t_lst`和`x_lst`。

1.  切换物理模型（每个事件都表示物理模型的切换），并将初始条件和起始时间重置为前一次求解的最终条件和时间。

1.  重复第 2 步和第 3 步，直到达到期望的最终模拟时间。

1.  将所有时间和解向量连接成单一数组并返回解决方案。

完整的`BouncingBall`类如下所示。

```py
class BouncingBall():
    """Class to simulate a bouncing ball

    Methods
    -------
    in_air(t, x) : Computes the ball's state derivatives while in air

    in_contact(t, x) : Computes the ball's state derivatives while in contact

    simulate(t_span, x0, max_step) : Simulates the ball bouncing
    """

    def __init__(self, m, k, b, ball_radius_cm, gravity=None):
        """
        Parameters
        ----------
        m : float
            ball mass in kg
        k : float
            ball stiffness in N/m
        c : float
            ball viscous damping coef. in N/(m/s)
        ball_radius_cm : float
            ball radius in centimeters
        gravity : float, optional
            acceleration of gravity in m/s²\. Defaults to 9.81 m/s² 
            if None given.
        """

        self.r = ball_radius_cm/100 #radius in meters
        self.m = m
        self.k = k
        self.b = b

        if gravity is None:
            self.g = 9.81 #m/s²
        else:
            self.g = gravity #m/s²

        # create event functions for the ball engaging and disengaging contact
        # note that when coming into contact with the ground, the direction of the
        # zero crossing will be negative (height abve the ground is transitioning 
        # to negative) whereas when the ball leaves the ground the zero crossing 
        # will be positive (height above the ground is transitioning from negative
        # to positive)
        self.hitting_ground = ContactEvent(self.r, direction=-1)
        self.leaving_ground = ContactEvent(self.r, direction=1)

    def in_air(self,t,x):
        """computes the ball's state derivatives while in air

        Parameters
        ----------
        t : float
            simulation time
        x : array-like
            vector containing the ball's state variables (height and velocity)

        Returns
        -------
        list
            vector containing the ball's state derivatives
        """

        x1, x2 = x #unpack the current state of the ball
        return [x2, -self.g]

    def in_contact(self,t,x):
        """computes the ball's state derivatives while in contact with the ground

        Parameters
        ----------
        t : float
            simulation time
        x : array-like
            vector containing the ball's state variables (height and velocity)

        Returns
        -------
        list
            vector containing the ball's state derivatives
        """

        x1, x2 = x #unpack the current state of the ball
        x1_dot = x2
        x2_dot = -(self.b/self.m)*x2 + (self.k/self.m)*(self.r - x1) - self.g
        return [x1_dot, x2_dot]

    def simulate(self, t_span, x0, max_step=0.01):
        """Simulates the time evolution of the ball bouncing

        Parameters
        ----------
        t_span : two element tuple/list
            starting and stopping time of the simulation, e.g. (0,10)
        x0 : array-like
            initial conditions of ball (height and velocity in m and m/s)
        max_step : float, optional
            max step size in simulation, by default 0.01

        Returns
        -------
        tuple
            tuple containing the time vector and height of the ball 
        """

        #check the initial height of the ball to determine if it's starting in the air
        in_air = x0[0] > self.r 
        #extract out initial starting and stopping times
        t_start, t_stop = t_span

        #create lists with initial conditions that we can append the piecewise solutions to
        t_lst = [t_start]
        x_lst = [x0[0]]

        #loop until we reach the desired stopping time
        while t_start < t_stop:

            """
            Here we simulate the ball forward in time using either the air 
            or contact model. Each of these subroutines will terminate when 
            either 1) the final desired simulation stop time is reached,
            or 2) when a contact event is triggered. At a high level, we are
            simulating our system forward in time using the relevant physics 
            model (that depends on the state of the system). The simulation 
            will alternate between the "in_air" model and "in_contact" model 
            switching between the two each time a contact event is triggered
            """

            if in_air:
                sol = solve_ivp(self.in_air, [t_start, t_stop], x0, 
                                events=[self.hitting_ground], max_step=max_step)

            else:
                sol = solve_ivp(self.in_contact, [t_start, t_stop], x0, 
                                events=[self.leaving_ground], max_step=max_step) 

            # append solution and time array to list of solutions. 
            # Note that the starting time of each solution
            # is the stopping time of the previous solution. 
            # To avoid having duplicate time points, we will not include the first
            #data point of each simulation. This is also why we created our 
            # solution lists above with the initial conditions already in them
            t_lst.append(sol.t[1::])
            x_lst.append(sol.y[0,1::])

            #set the starting time and initial conditions to the stopping 
            #time and end condtions of the previous loop
            t_start = sol.t[-1]
            x0 = sol.y[:,-1].flatten()

            #if we haven't reached the stopping time yet in the current 
            #loop, we must be switching between being in the air and
            #being in contact
            if t_start < t_stop:
                in_air = not in_air

        #concatenate all of the solutions into a single numpy array
        t = np.hstack(t_lst)
        x = np.hstack(x_lst)

        return t,x
```

到此为止，所有的艰苦工作都完成了，我们准备好运行模拟了！我们可以使用以下几行代码，创建一个`BouncingBall`对象，模拟从 2 米高度下落后球体弹跳八秒，然后绘制轨迹。

```py
b = BouncingBall(m=1, k=10e3, c=10, ball_radius_cm=6)

t,x = b.simulate([0,8], [2, 0])
df = pd.DataFrame({'time':t, 'x':x}).set_index('time')
df.to_csv('sim_data.csv')

fig, ax = plt.subplots()
ax.plot(t,x)
ax.set_xlabel('Time [s]')
ax.set_ylabel('Height [m]')

fig.savefig('bouncing_ball.png')
```

![](img/9c88ced46961bdeaa49ad2eee5874f02.png)

作者提供的图片

相当令人满意！如果我们愿意，甚至可以对数据进行后处理，创建一个球体弹跳的动画（如帖子开头和这里重复的动画）。

![](img/af2b26da88b758ec371e38029058f03b.png)

作者提供的动画

在`matplotlib`中创建动画超出了本文的范围，但对感兴趣的人来说，创建动画的代码包含在本文附带的 Github [仓库](https://github.com/Nick-Hemenway/medium-simulating_physical_systems)中。

## 结论

模拟物理系统的能力在几乎所有的科学和工程领域中都能提供宝贵的帮助。虽然所突出的例子相对具体，但它涵盖了一个更为通用的工作流程，可以扩展到模拟任何类型的物理系统。无论系统是什么，你总是需要确定支配方程，将其转换为一阶微分方程系统，然后将方程传递给某种数值积分例程。当然，这里有些细节在本文中无法讨论——有时系统包含约束方程，我们会得到一组微分代数方程（DAEs），而不是常微分方程（ODEs）。或者你可能无法明确地求解状态向量的导数与系统状态的关系。无论如何，本文中涵盖的内容仍然是必须掌握的基础，才能继续进行更深入的研究。

欢迎随时留言或提问，或者通过 Linkedin 联系我 —— 我会很乐意澄清任何不确定的地方。最后，我鼓励你自己动手尝试代码（或将其作为你自己工作流程的起始模板）—— 本文的所有代码可以在我的 [Github](https://github.com/Nick-Hemenway/medium-simulating_physical_systems) 上找到。

# Nicholas Hemenway

+   ***如果你喜欢这个，请******关注我在 Medium 上的文章***

+   ***考虑订阅*** ***电子邮件更新***

+   ***有兴趣合作吗？让我们*** [***在 LinkedIn 上联系***](https://www.linkedin.com/in/nicholas-hemenway/)
