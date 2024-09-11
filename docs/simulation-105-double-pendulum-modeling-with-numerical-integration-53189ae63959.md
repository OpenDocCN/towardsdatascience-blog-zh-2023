# 模拟 105：双摆模型的数值积分

> 原文：[https://towardsdatascience.com/simulation-105-double-pendulum-modeling-with-numerical-integration-53189ae63959?source=collection_archive---------5-----------------------#2023-08-14](https://towardsdatascience.com/simulation-105-double-pendulum-modeling-with-numerical-integration-53189ae63959?source=collection_archive---------5-----------------------#2023-08-14)

## 模拟一个混沌系统

[](https://medium.com/@ln8378?source=post_page-----53189ae63959--------------------------------)[![Le Nguyen](../Images/05289b40bb528d5ba2a0ee00d1a75990.png)](https://medium.com/@ln8378?source=post_page-----53189ae63959--------------------------------)[](https://towardsdatascience.com/?source=post_page-----53189ae63959--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----53189ae63959--------------------------------) [Le Nguyen](https://medium.com/@ln8378?source=post_page-----53189ae63959--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb34fcbf59198&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-105-double-pendulum-modeling-with-numerical-integration-53189ae63959&user=Le+Nguyen&userId=b34fcbf59198&source=post_page-b34fcbf59198----53189ae63959---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----53189ae63959--------------------------------) ·9分钟阅读·2023年8月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F53189ae63959&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-105-double-pendulum-modeling-with-numerical-integration-53189ae63959&user=Le+Nguyen&userId=b34fcbf59198&source=-----53189ae63959---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F53189ae63959&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-105-double-pendulum-modeling-with-numerical-integration-53189ae63959&source=-----53189ae63959---------------------bookmark_footer-----------)

摆是一个我们都非常熟悉的经典物理系统。无论是座钟还是秋千上的孩子，我们都见过摆的规律性、周期性运动。单摆在经典物理中定义明确，但双摆（一个摆挂在另一个摆的末端）却是[真正的混沌](https://en.wikipedia.org/wiki/Chaos_theory)。在这篇文章中，我们将基于对摆的直观理解，建立双摆的混沌模型。物理学非常有趣，而所需的数值方法则是每个人工具箱中的重要工具。

![](../Images/d5fc100fbdb7588fd0524ba6653bf8e0.png)

图1: [混沌双摆的示例](https://commons.wikimedia.org/wiki/File:Double_pendulum_predicting_dynamics.gif)

在本文中我们将：

+   了解谐振动并模拟单摆的行为

+   学习混沌理论的基础知识

+   数值建模双摆的混沌行为

# 谐振动与单摆

## 简单谐振动

我们将摆的周期性振荡运动描述为[谐振动](https://en.wikipedia.org/wiki/Simple_harmonic_motion)。谐振动发生在系统中存在的运动被与该运动方向相反的比例恢复力平衡时。我们可以在图2中看到一个例子，其中一个质量因重力而被拉向下方，但这将能量传递给弹簧，弹簧随后反弹并将质量拉回。弹簧系统旁边，我们看到质量在一个称为[相量图](https://en.wikipedia.org/wiki/Phasor)的圆周上运动，这进一步说明了系统的规则运动。

![](../Images/c2bcaadc06cec4ffcd78e75d63ae954c.png)

图2: [弹簧上质量的简单谐振动示例](https://mathimages.swarthmore.edu/index.php/File:SpringCircle2.gif)

谐振动可以是阻尼的（由于拖拽力而振幅减小）或驱动的（由于外部力的添加而振幅增加），但我们将从最简单的情况——没有外部力作用的无限谐振动（无阻尼运动）开始。这种运动对建模在小角度/低振幅下摆动的单摆是一个很好的近似。在这种情况下，我们可以使用下面的方程1来模拟运动。

![](../Images/99380611fb900783b01c62a9c7221444.png)

方程1: 小角度摆的简单谐振动

我们可以轻松地将这个函数编入代码，并模拟一个简单的摆动随时间变化的过程。

```py
def simple_pendulum(theta_0, omega, t, phi):
    theta = theta_0*np.cos(omega*t + phi)
    return theta

#parameters of our system
theta_0 = np.radians(15) #degrees to radians

g = 9.8 #m/s^2
l = 1.0 #m
omega = np.sqrt(g/l)

phi = 0 #for small angle

time_span = np.linspace(0,20,300) #simulate for 20s split into 300 time intervals
theta = []
for t in time_span:
    theta.append(simple_pendulum(theta_0, omega, t, phi))

#Convert back to cartesian coordinates
x = l*np.sin(theta)
y = -l*np.cos(theta) #negative to make sure the pendulum is facing down
```

![](../Images/4319f9bdb1e1b926f305eee27e17190a.png)

图3: 简单摆模拟

## 使用拉格朗日力学分析完整的摆动运动

一个简单的小角度摆动是一个良好的起点，但我们希望超越这一点并模拟完整摆动的运动。由于我们不能再使用[小角度近似](https://en.wikipedia.org/wiki/Small-angle_approximation)，因此最好使用[拉格朗日力学](https://en.wikipedia.org/wiki/Lagrangian_mechanics)来建模摆动。这是物理学中的一个基本工具，它将我们从观察系统中的力转变为观察系统中的能量。我们将参考框架从驱动力与恢复力切换为[动能](https://en.wikipedia.org/wiki/Kinetic_energy)与[势能](https://en.wikipedia.org/wiki/Potential_energy)。

拉格朗日量是方程2中给出的动能与势能之差。

![](../Images/cd59b2b9c3212f5c308c9ac300e3e3d1.png)

方程2: 拉格朗日量

将方程 3 中给出的摆的动能和势能代入，得到的是方程 4 中显示的摆的拉格朗日量

![](../Images/4626a9fa15c967eea4f06f6ae0a3f5e6.png)

方程 3：摆的动能和势能

![](../Images/2575a56e48aca50c294f382c5d7891a1.png)

方程 4：摆的拉格朗日量

有了摆的拉格朗日量，我们现在描述了系统的能量。还有最后一步数学运算，将其转化为可以构建仿真的内容。我们需要通过[欧拉-拉格朗日方程](https://en.wikipedia.org/wiki/Euler%E2%80%93Lagrange_equation)从能量参考桥接回动态/力导向的参考。使用这个方程，我们可以用拉格朗日量来得到摆的角加速度。

![](../Images/c74ff56138a49800ed2bd957059188f4.png)

方程 5：欧拉-拉格朗日方程中的角加速度

在进行数学运算后，我们得到了角加速度，可以用来得到角速度和角度本身。这将需要一些[数值积分](https://en.wikipedia.org/wiki/Numerical_integration)，这些内容将在我们的完整摆仿真中详细说明。即使是单摆，[非线性动力学](https://en.wikipedia.org/wiki/Nonlinear_system)也意味着没有解析解来求解*theta*，因此需要数值解。积分过程相当简单（但强大），我们使用角加速度更新角速度，再使用角速度更新*theta*，通过将前者加到后者上并乘以一个时间步长。这样可以得到加速度/速度曲线下的面积的近似值。时间步长越小，近似值越准确。

```py
def full_pendulum(g,l,theta,theta_velocity, time_step):
    #Numerical Integration
    theta_acceleration = -(g/l)*np.sin(theta) #Get acceleration
    theta_velocity += time_step*theta_acceleration #Update velocity with acceleration
    theta += time_step*theta_velocity #Update angle with angular velocity
    return theta, theta_velocity

g = 9.8 #m/s^2
l = 1.0 #m

theta = [np.radians(90)] #theta_0
theta_velocity = 0 #Start with 0 velocity
time_step = 20/300 #Define a time step

time_span = np.linspace(0,20,300) #simulate for 20s split into 300 time intervals
for t in time_span:
    theta_new, theta_velocity = full_pendulum(g,l,theta[-1], theta_velocity, time_step)
    theta.append(theta_new)

#Convert back to cartesian coordinates 
x = l*np.sin(theta)
y = -l*np.cos(theta)
```

![](../Images/df966baad829d1a82839980c7186c0b9.png)

图 4：完整摆的仿真

我们已经模拟了一个完整的摆，但这仍然是一个定义明确的系统。现在是时候进入双摆的混沌领域了。

# 混沌与双摆

[混沌](https://en.wikipedia.org/wiki/Chaos_theory)在数学意义上指的是对初始条件高度敏感的系统。系统开始时的微小变化会导致系统演变过程中行为的巨大差异。这完美地描述了双摆的运动。与单摆不同，双摆不是一个表现良好的系统，甚至初始角度的微小变化都会导致它以截然不同的方式演变。

为了模拟双摆的运动，我们将使用与之前相同的拉格朗日方法（[查看完整推导](https://mse.redwoods.edu/darnold/math55/DEproj/sp08/jaltic/presentation.pdf)）。

![](../Images/62c62acceea8661fa7a1d0cb4d673b80.png)

我们在将这个方程实现到代码中并找到theta时，将继续使用之前相同的数值积分方案。

```py
#Get theta1 acceleration 
def theta1_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g):
    mass1 = -g*(2*m1 + m2)*np.sin(theta1)
    mass2 = -m2*g*np.sin(theta1 - 2*theta2)
    interaction = -2*np.sin(theta1 - theta2)*m2*np.cos(theta2_velocity**2*l2 + theta1_velocity**2*l1*np.cos(theta1 - theta2))
    normalization = l1*(2*m1 + m2 - m2*np.cos(2*theta1 - 2*theta2))

    theta1_ddot = (mass1 + mass2 + interaction)/normalization

    return theta1_ddot

#Get theta2 acceleration
def theta2_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g):
    system = 2*np.sin(theta1 - theta2)*(theta1_velocity**2*l1*(m1 + m2) + g*(m1 + m2)*np.cos(theta1) + theta2_velocity**2*l2*m2*np.cos(theta1 - theta2))
    normalization = l1*(2*m1 + m2 - m2*np.cos(2*theta1 - 2*theta2))

    theta2_ddot = system/normalization
    return theta2_ddot

#Update theta1
def theta1_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step):
    #Numerical Integration
    theta1_velocity += time_step*theta1_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g)
    theta1 += time_step*theta1_velocity
    return theta1, theta1_velocity

#Update theta2
def theta2_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step):
    #Numerical Integration
    theta2_velocity += time_step*theta2_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g)
    theta2 += time_step*theta2_velocity
    return theta2, theta2_velocity

#Run full double pendulum
def double_pendulum(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step,time_span):
    theta1_list = [theta1]
    theta2_list = [theta2]

    for t in time_span:
        theta1, theta1_velocity = theta1_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)
        theta2, theta2_velocity = theta2_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)

        theta1_list.append(theta1)
        theta2_list.append(theta2)

    x1 = l1*np.sin(theta1_list) #Pendulum 1 x
    y1 = -l1*np.cos(theta1_list) #Pendulum 1 y

    x2 = l1*np.sin(theta1_list) + l2*np.sin(theta2_list) #Pendulum 2 x
    y2 = -l1*np.cos(theta1_list) - l2*np.cos(theta2_list) #Pendulum 2 y

    return x1,y1,x2,y2
```

```py
#Define system parameters
g = 9.8 #m/s^2

m1 = 1 #kg
m2 = 1 #kg

l1 = 1 #m
l2 = 1 #m

theta1 = np.radians(90)
theta2 = np.radians(45)

theta1_velocity = 0 #m/s
theta2_velocity = 0 #m/s

theta1_list = [theta1]
theta2_list = [theta2]

time_step = 20/300

time_span = np.linspace(0,20,300)
x1,y1,x2,y2 = double_pendulum(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step,time_span)
```

![](../Images/08913f06219e3671259de47ec97ae3a3.png)

图 5：双摆仿真

我们终于成功了！我们成功地建模了一个双摆，但现在是观察一些混沌的时候了。我们的最终模拟将是两个双摆，起始条件略有不同。我们将设置一个摆的*theta 1*为90度，另一个为91度。让我们看看会发生什么。

![](../Images/aeb23d51e522d12e3787090afee94c7a.png)

图6：两个起始条件略有不同的双摆

我们可以看到，两个摆开始时的轨迹类似，但很快就发生了分歧。这就是我们所说的混沌，即使是1度的角度差异也会导致截然不同的最终行为。

# 结论

在这篇文章中，我们了解了摆的运动及其建模方法。我们从最简单的谐振动模型开始，逐步建立到复杂和混沌的双摆。在这个过程中，我们学习了拉格朗日量、混沌和数值积分。

双摆是混沌系统的最简单例子。这些系统在我们的世界中随处可见，从[种群动态](https://support.goldsim.com/hc/en-us/articles/115011378368-Chaotic-Behavior-in-Population-Dynamics)、[气候](https://earth.stanford.edu/news/climate-chaos-why-warming-makes-weather-less-predictable)到[台球](https://plus.maths.org/content/chaos-billiard-table#:~:text=It%20turns%20out%20that%20the,explore%20all%20of%20the%20table.)。我们可以将从双摆中学到的经验应用到遇到的任何混沌系统中。

## 关键要点

+   混沌系统对初始条件非常敏感，即使是对起始条件的微小变化，也会以截然不同的方式演变。

+   在处理一个系统，特别是混沌系统时，是否有其他参考框架可以使其更易于处理？（类似于力参考系转到能量参考系）

+   当系统变得过于复杂时，我们需要实施数值解法来解决它们。这些解法简单但强大，能够提供对实际行为的良好近似。

# 参考文献

本文使用的所有图形要么由作者创建，要么来自[数学图像](https://mathimages.swarthmore.edu/index.php/Main_Page)，并且完全遵循[GNU自由文档许可证1.2](http://www.gnu.org/copyleft/fdl.html)

[](https://www.wired.com/2016/07/everything-harmonic-oscillator/?source=post_page-----53189ae63959--------------------------------) [## 一切—是的，一切—都是一个谐振子

### 物理学本科生可能会开玩笑说宇宙是由谐振子组成的，但他们并没有说得离谱。

www.wired.com](https://www.wired.com/2016/07/everything-harmonic-oscillator/?source=post_page-----53189ae63959--------------------------------)

经典力学，约翰·泰勒 [https://neuroself.files.wordpress.com/2020/09/taylor-2005-classical-mechanics.pdf](https://neuroself.files.wordpress.com/2020/09/taylor-2005-classical-mechanics.pdf)

# 完整代码

## 简单摆

```py
def makeGif(x,y,name):
    !mkdir frames

    counter=0
    images = []
    for i in range(0,len(x)):
        plt.figure(figsize = (6,6))

        plt.plot([0,x[i]],[0,y[i]], "o-", color = "b", markersize = 7, linewidth=.7 )
        plt.title("Pendulum")
        plt.xlim(-1.1,1.1)
        plt.ylim(-1.1,1.1)
        plt.savefig("frames/" + str(counter)+ ".png")
        images.append(imageio.imread("frames/" + str(counter)+ ".png"))
        counter += 1
        plt.close()

    imageio.mimsave(name, images)

    !rm -r frames

def simple_pendulum(theta_0, omega, t, phi):
    theta = theta_0*np.cos(omega*t + phi)
    return theta

#parameters of our system
theta_0 = np.radians(15) #degrees to radians

g = 9.8 #m/s^2
l = 1.0 #m
omega = np.sqrt(g/l)

phi = 0 #for small angle

time_span = np.linspace(0,20,300) #simulate for 20s split into 300 time intervals
theta = []
for t in time_span:
    theta.append(simple_pendulum(theta_0, omega, t, phi))

x = l*np.sin(theta)
y = -l*np.cos(theta) #negative to make sure the pendulum is facing down
```

## 摆

```py
def full_pendulum(g,l,theta,theta_velocity, time_step):
    theta_acceleration = -(g/l)*np.sin(theta)
    theta_velocity += time_step*theta_acceleration
    theta += time_step*theta_velocity
    return theta, theta_velocity

g = 9.8 #m/s^2
l = 1.0 #m

theta = [np.radians(90)] #theta_0
theta_velocity = 0
time_step = 20/300

time_span = np.linspace(0,20,300) #simulate for 20s split into 300 time intervals
for t in time_span:
    theta_new, theta_velocity = full_pendulum(g,l,theta[-1], theta_velocity, time_step)
    theta.append(theta_new)

#Convert back to cartesian coordinates 
x = l*np.sin(theta)
y = -l*np.cos(theta)

#Use same function from simple pendulum
makeGif(x,y,"pendulum.gif")
```

## 双摆

```py
def theta1_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g):
    mass1 = -g*(2*m1 + m2)*np.sin(theta1)
    mass2 = -m2*g*np.sin(theta1 - 2*theta2)
    interaction = -2*np.sin(theta1 - theta2)*m2*np.cos(theta2_velocity**2*l2 + theta1_velocity**2*l1*np.cos(theta1 - theta2))
    normalization = l1*(2*m1 + m2 - m2*np.cos(2*theta1 - 2*theta2))

    theta1_ddot = (mass1 + mass2 + interaction)/normalization

    return theta1_ddot

def theta2_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g):
    system = 2*np.sin(theta1 - theta2)*(theta1_velocity**2*l1*(m1 + m2) + g*(m1 + m2)*np.cos(theta1) + theta2_velocity**2*l2*m2*np.cos(theta1 - theta2))
    normalization = l1*(2*m1 + m2 - m2*np.cos(2*theta1 - 2*theta2))

    theta2_ddot = system/normalization
    return theta2_ddot

def theta1_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step):

    theta1_velocity += time_step*theta1_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g)
    theta1 += time_step*theta1_velocity
    return theta1, theta1_velocity

def theta2_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step):

    theta2_velocity += time_step*theta2_acceleration(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g)
    theta2 += time_step*theta2_velocity
    return theta2, theta2_velocity

def double_pendulum(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step,time_span):
    theta1_list = [theta1]
    theta2_list = [theta2]

    for t in time_span:
        theta1, theta1_velocity = theta1_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)
        theta2, theta2_velocity = theta2_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)

        theta1_list.append(theta1)
        theta2_list.append(theta2)

    x1 = l1*np.sin(theta1_list)
    y1 = -l1*np.cos(theta1_list)

    x2 = l1*np.sin(theta1_list) + l2*np.sin(theta2_list)
    y2 = -l1*np.cos(theta1_list) - l2*np.cos(theta2_list)

    return x1,y1,x2,y2
```

```py
#Define system parameters, run double pendulum
g = 9.8 #m/s^2

m1 = 1 #kg
m2 = 1 #kg

l1 = 1 #m
l2 = 1 #m

theta1 = np.radians(90)
theta2 = np.radians(45)

theta1_velocity = 0 #m/s
theta2_velocity = 0 #m/s

theta1_list = [theta1]
theta2_list = [theta2]

time_step = 20/300

time_span = np.linspace(0,20,300)
for t in time_span:
    theta1, theta1_velocity = theta1_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)
    theta2, theta2_velocity = theta2_update(m1,m2,l1,l2,theta1,theta2,theta1_velocity,theta2_velocity,g,time_step)

    theta1_list.append(theta1)
    theta2_list.append(theta2)

x1 = l1*np.sin(theta1_list)
y1 = -l1*np.cos(theta1_list)

x2 = l1*np.sin(theta1_list) + l2*np.sin(theta2_list)
y2 = -l1*np.cos(theta1_list) - l2*np.cos(theta2_list)
```

```py
#Make Gif
!mkdir frames

counter=0
images = []
for i in range(0,len(x1)):
    plt.figure(figsize = (6,6))

    plt.figure(figsize = (6,6))
    plt.plot([0,x1[i]],[0,y1[i]], "o-", color = "b", markersize = 7, linewidth=.7 )
    plt.plot([x1[i],x2[i]],[y1[i],y2[i]], "o-", color = "b", markersize = 7, linewidth=.7 )
    plt.title("Double Pendulum")
    plt.xlim(-2.1,2.1)
    plt.ylim(-2.1,2.1)
    plt.savefig("frames/" + str(counter)+ ".png")
    images.append(imageio.imread("frames/" + str(counter)+ ".png"))
    counter += 1
    plt.close()

imageio.mimsave("double_pendulum.gif", images)

!rm -r frames
```
