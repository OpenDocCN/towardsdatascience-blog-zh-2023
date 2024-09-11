# 气体动力学理论：理想气体分子的动力学建模

> 原文：[https://towardsdatascience.com/the-kinetic-theory-of-gases-modeling-the-dynamics-of-ideal-gas-molecules-bba2be4e22a?source=collection_archive---------5-----------------------#2023-01-06](https://towardsdatascience.com/the-kinetic-theory-of-gases-modeling-the-dynamics-of-ideal-gas-molecules-bba2be4e22a?source=collection_archive---------5-----------------------#2023-01-06)

## 统计力学

## 开发一个框架来模拟和可视化分子碰撞，并使用 Python 提取热力学洞察。

[](https://medium.com/@ChemAndCode?source=post_page-----bba2be4e22a--------------------------------)[![Gaurav Deshmukh](../Images/98433b1a256f160792a7b2b0874a2081.png)](https://medium.com/@ChemAndCode?source=post_page-----bba2be4e22a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bba2be4e22a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bba2be4e22a--------------------------------) [Gaurav Deshmukh](https://medium.com/@ChemAndCode?source=post_page-----bba2be4e22a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5a75283b2c71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-kinetic-theory-of-gases-modeling-the-dynamics-of-ideal-gas-molecules-bba2be4e22a&user=Gaurav+Deshmukh&userId=5a75283b2c71&source=post_page-5a75283b2c71----bba2be4e22a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bba2be4e22a--------------------------------) ·14 分钟阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbba2be4e22a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-kinetic-theory-of-gases-modeling-the-dynamics-of-ideal-gas-molecules-bba2be4e22a&user=Gaurav+Deshmukh&userId=5a75283b2c71&source=-----bba2be4e22a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbba2be4e22a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-kinetic-theory-of-gases-modeling-the-dynamics-of-ideal-gas-molecules-bba2be4e22a&source=-----bba2be4e22a---------------------bookmark_footer-----------)![](../Images/3920606289c067f4ef4a33c03e3bb261.png)

图片由 [Terry Vlisidis](https://unsplash.com/@vlisidis?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

想象有人不断地将球扔向你的头部。根据球的大小和质量，你可能会感受到从轻微的烦恼到难以忍受的痛苦。然而，实际上有无数个这样的“球”每时每刻撞击着你，不仅是你的头部，还有你的整个身体，而你却没有感觉到。空气中的氮气、氧气和水分子在你周围不断地随机运动，即使空气看起来是静止的。这些分子与您的碰撞的结果是空气对您施加了一定的压力（称为大气压力）。由于我们“习惯”了大气压力，它似乎没有什么特别的，但如果你曾在飞机上或高压舱中，压力偏离了大气压力，你可能会注意到不同的感觉，比如耳朵的“咔嗒”声，这些都是你身体对压力变化的反应。

17世纪，罗伯特·波义耳给出了对压力的宏观理解，以及压力如何随气体占据的体积变化，这被称为波义耳定律。然而，直到一个世纪后，丹尼尔·伯努利才提供了一个定性的分子图像来解释压力，并将其与分子的温度和动能联系起来。然而，这一“动理论”直到19世纪，詹姆斯·克拉克·麦克斯韦在鲁道夫·克劳修斯的工作基础上，将其形式化为统计定律，并且路德维希·玻尔兹曼阐明了其与熵的关系，导致了麦克斯韦-玻尔兹曼速度分布的公式。这最终导致了从分子角度出发的详细气体理论的发展，这与广泛理解的宏观观点相一致。

在这篇文章中，我们将尝试通过使用 Python 进行一系列数值模拟，将气体的分子图像与其宏观性质——即压力、体积和温度——联系起来。

# 理论背景

## 气体的动理论

气体的动理论是一个模型，描述了处于不断随机运动中的气体分子。

+   分子在直线中运动，直到被其他分子或容器壁的弹性碰撞打断。弹性意味着在碰撞中没有动能损失。

+   分子之间没有“相互作用”，即它们不施加任何吸引力或排斥力。

+   与气体占据的体积相比，分子本身占据的体积被认为是可以忽略不计的。

如果遵循这些规则的分子被取样并测量其速度，它们将符合麦克斯韦-玻尔兹曼分布。气体的温度实际上是一个基于分布形状定义的属性。平均动能越高（或分布越平坦），气体的温度越高，反之亦然。定义单个分子的温度是没有意义的；相反，它是一个由分子集合的平均属性。

请注意，气体动理论是一个模型，它不一定真实反映现实（这与所有科学模型的情况类似）。分子碰撞并非严格弹性，分子确实存在短程相互作用。然而，具有这些相对简单假设的模型能够非常好地模拟理想气体的性质，这对于预测气体在各种条件下的行为是一个非常有用的宏观模型。

## 理想气体定律

理想气体是一个假设的气体，其性质通过一个简单的状态方程（即理想气体定律）相关联。

![](../Images/0de1f0da1be9799239ba418fa11d2e37.png)

这里 *P* 指的是气体的压力，*V* 指的是容器的体积，*T* 是温度，*n* 是气体的摩尔数。如果这三个量中的任何一个保持不变，则第四个量也不会改变，这一点通过 *R* 体现，*R* 是通用气体常数。理想气体定律是一个准确的模型，适用于高温或低压下的气体，因为在这些条件下气体动理论的大多数假设是成立的。

我们将基于理想气体定律用我们的模型测试两个关系。

+   在恒定温度和摩尔数下，压力与体积成反比。

+   在恒定体积和摩尔数下，压力与温度成正比。

前者被称为波义耳定律，后者被称为盖-吕萨克定律，这些定律都以最早通过实验发现这些变量之间关系的研究人员命名。

# 设置计算模型

为了根据气体动理论模拟分子的运动，我们需要建立一个具有碰撞的动态 n 体模拟。请注意，本文章的目标是制作一个用于教学目的的模拟。因此，代码的设置旨在最大化理解，而非执行速度。

## 定义分子属性

我们从导入所有必需的模块开始。

```py
import os
import sys
import time
import numpy as np
import scipy as sci
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
```

首先，我们将定义一个名为 *Molecule* 的类。该类的对象将存储如分子的质量、位置和速度等属性。我们还定义了一个名为 color 的属性，可以用来区分不同的气体并设置动画中点的颜色。

```py
class Molecule(object):
    #Constructor
    def __init__(self,molecule,position,velocity,mass,color="black"):
        self.molecule=molecule
        self.position=np.array([x_i for x_i in position])
        self.velocity=np.array([v_i for v_i in velocity])
        self.mass=mass
        self.color=color

    #Setters for position, velocity, mass and color
    def set_position(self,position):
        self.position=np.array([x_i for x_i in position])

    def set_velocity(self,velocity):
        self.velocity=np.array([v_i for v_i in velocity])

    def set_color(self,color):
        self.color=color

    def set_mass(self,mass):
        self.mass=mass

    #Getters for position, velocity, mass and color
    def get_position(self):
        return self.position

    def get_velocity(self):
        return self.velocity

    def get_color(self):
        return self.color

    def get_mass(self):
        return self.mass
```

## 向箱子中添加分子

接下来，我们创建一个*模拟*类并定义关键输入参数，例如模拟盒子的尺寸，这最终控制气体的体积。我们还初始化用于记录的变量，如墙体动量，以便我们能够计算压力。

```py
class Simulation(object):
    #Constructor
    def __init__(self,name,box_dim,t_step,particle_radius):
        #Set simulation inputs
        self.name=name #Name of the simulation
        self.box_dim=[x for x in box_dim] #Dimensions of the box
        self.t_step=t_step #Timestep
        self.particle_radius=particle_radius #Radius of the particles

        #Calculate volume and number of dimensions
        self.V=np.prod(self.box_dim) #Area/Volume of the box
        self.dim=int(len(box_dim)) #Number of dimensions (2D or 3D)

        #Initialize paramters
        self.molecules=[] #Create empty list to store objects of class Molecule
        self.n_molecules=0 #Create variable to store number of molecules
        self.wall_collisions=0 #Create variable to store number of wall collisions
        self.wall_momentum=0 #Create variable to store net momentum exchanged with wall
```

要添加分子，首先需要初始化它们的位置和速度。对于这两者，我们可以定义函数，根据用户输入的分布创建值数组。请注意，初始化的位置和速度遵循什么分布并不重要（只要没有不符合物理规律的情况，比如分子在盒子外）。我们将看到速度最终遵循相同的分布。

```py
 #Still in Simulation class
    def _generate_initial_positions(self,n,dist="uniform"):
            #Uniform distribution
            if dist=="uniform":
                _pos=np.random.uniform(low=[0]*self.dim,high=self.box_dim,size=(n,self.dim))

            #Store positions in temporary variable    
            self._positions=_pos

    def _generate_initial_velocities(self,n,v_mean,v_std,dist="normal"):
        #Normal distribution with mean v_mean and std v_std
        if dist=="normal":
            self.v_mean=v_mean
            self.v_std=v_std
            _vel=np.random.normal(loc=v_mean,scale=v_std,size=(n,self.dim))

        #Uniform distribution with lower bound v_mean and higher bound v_std
        if dist=="uniform":
            self.v_mean=v_mean
            self.v_std=v_std
            _vel=np.random.uniform(low=v_mean,high=v_std,size=(n,self.dim))

        #All velocities equal to v_mean
        if dist=="equal":
            self.v_mean=v_mean
            self.v_std=v_std
            _vel=v_mean*np.ones((n,self.dim))

        #Randomly switch velocities to negative with probability 0.5
        for i in range(_vel.shape[0]):
            for j in range(_vel.shape[1]):
                if np.random.uniform() > 0.5:
                    _vel[i,j]=-_vel[i,j]

        #Store velocities in temporary variable
        self._velocities=_vel
```

现在是时候将分子添加到盒子中了。我们调用之前定义的两个函数生成初始位置和速度的数组，创建*分子*类的对象，并将它们分配给这些对象。

```py
 #Still in Simulation class
    def add_molecules(self,molecule,n,v_mean,v_std,pos_dist="uniform",v_dist="normal",color="black"):
        #Generate initial positions and velocities
        self._generate_initial_positions(n,dist=pos_dist)
        self._generate_initial_velocities(n,v_mean,v_std,dist=v_dist)

        #Initialize objects of class Molecule in a list (set mass to 1 as default)
        _add_list=[Molecule(molecule,position=self._positions[i,:],velocity=self._velocities[i,:],color=color,mass=1) for i in range(n)]
        self.molecules.extend(_add_list)
        self.n_molecules+=len(_add_list)
```

最后，我们编写一个用于一般记录的函数，即制作矩阵来存储位置和速度向量以及它们的大小。我们还制作一个距离矩阵，用于存储每对分子之间的距离。这将有助于检测碰撞。

```py
 #Still in Simulation class
    def make_matrices(self):
        #Make empty matrices to store positions, velocities, colors, and masses
        self.positions=np.zeros((self.n_molecules,self.dim))
        self.velocities=np.zeros((self.n_molecules,self.dim))
        self.colors=np.zeros(self.n_molecules,dtype="object")
        self.masses=np.zeros(self.n_molecules)

        #Iterate over molecules, get their properties and assign to matrices
        for i,m in enumerate(self.molecules):
            self.positions[i,:]=m.get_position()
            self.velocities[i,:]=m.get_velocity()
            self.colors[i]=m.get_color()
            self.masses[i]=m.get_mass()

        #Make vectors with magnitudes of positions and velocities
        self.positions_norm=np.linalg.norm(self.positions,axis=1)
        self.velocities_norm=np.linalg.norm(self.velocities,axis=1)

        #Make distance matrix
        self.distance_matrix=np.zeros((self.n_molecules,self.n_molecules))
        for i in range(self.distance_matrix.shape[0]):
            for j in range(self.distance_matrix.shape[1]):
                self.distance_matrix[i,j]=np.linalg.norm(self.positions[i,:]-self.positions[j,:])

        #Set diagonal entries (distance with itself) to a high value
        #to prevent them for appearing in the subsequent distance filter
        np.fill_diagonal(self.distance_matrix,1e5)
```

## 检测和处理碰撞

现在我们进入模拟的核心部分，即分子动力学建模。碰撞物理定义在二维平面上，但通过少量修改可以扩展到三维盒子。我们定义了一个函数，通过一个三项检查表进行处理：

+   检查是否存在分子-分子碰撞，如果有，则相应更新速度

+   根据分子的速度更新位置

+   检查是否存在分子-墙体碰撞，如果有，则相应更新速度和墙体动量

为了筛选出足够接近以发生碰撞的分子，我们遍历距离矩阵并返回距离小于其半径总和的分子对的索引对。分子可能在此截止范围内，却仍然相互远离。因此，我们应用另一个标准（见**图1**）来检查分子是否相互靠近。如果是，我们根据**图1**中给出的方程更新它们的速度。我们通过保持分子的动能和线动量（沿碰撞轴方向）来得到这些方程。

![](../Images/b9879f3e304aa2046ca35f955680f30e.png)

**图1**：示意图显示了两个球体在二维平面上的碰撞。蓝色的变量和方程代表速度的切向和法向分量（相对于碰撞轴）。粗体变量是向量，其余的是标量。尖括号表示点积（内积）。

更新分子的位置非常简单。我们将速度向量与时间步长的乘积加到先前的位置，以获得新位置。为了识别与墙壁的碰撞，我们只需检查分子的新位置是否超过了盒子的下限或上限。如果是，我们就改变垂直于墙壁的速度的符号，并根据该速度设置新位置。此外，我们将该速度的两倍大小添加到跟踪与墙壁交换动量的变量中。

```py
 #Still in Simulation class
    def update_positions(self):
        #1: Check molecule collisions

        #Find molecule pairs that will collide
        collision_pairs=np.argwhere(self.distance_matrix < 2*self.particle_radius)

        #If collision pairs exist 
        if len(collision_pairs):

            #Go through pairs and remove repeats of indices
            #(for eg., only consider (1,2), remove (2,1))
            pair_list=[]
            for pair in collision_pairs:
                add_pair=True
                for p in pair_list:
                    if set(p)==set(pair):
                        add_pair=False
                        break
                if add_pair:
                    pair_list.append(pair)

            #For every remaining pair, get the molecules, positions, and velocities
            for pair in pair_list:
                m_1=self.molecules[pair[0]]
                m_2=self.molecules[pair[1]]

                x_1=m_1.get_position()
                x_2=m_2.get_position()

                u_1=m_1.get_velocity()
                u_2=m_2.get_velocity()

                #Check if molecules are approaching or departing
                approach_sign=np.sign(np.dot(u_1-u_2,x_2-x_1))
                #If molecules are approaching
                if approach_sign == 1:
                    #Get masses
                    ms_1=m_1.get_mass()
                    ms_2=m_2.get_mass()

                    #Calculate final velocities
                    v_1=u_1 - 2*ms_2/(ms_1 + ms_2) * (np.dot(u_1-u_2,x_1-x_2)/np.linalg.norm(x_1-x_2)**2) * (x_1 - x_2)
                    v_2=u_2 - 2*ms_1/(ms_1 + ms_2) * (np.dot(u_2-u_1,x_2-x_1)/np.linalg.norm(x_2-x_1)**2) * (x_2 - x_1)

                    #Update velocities of the molecule objects
                    m_1.set_velocity(v_1)
                    m_2.set_velocity(v_2)

        #2: Update positions

        #Iterate over all the molecule objects
        for i,m in enumerate(self.molecules):
            #Get the position, velocity, and mass
            _x=m.get_position()
            _v=m.get_velocity()
            _m=m.get_mass()

            #Calculate new position
            _x_new=_x + _v * self.t_step

            #3: Check wall collisions

            #Check collisions with the top and right walls
            _wall_diff=_x_new - np.array(self.box_dim)           
            #If wall collisions present
            if _wall_diff[_wall_diff>=0].shape[0] > 0:
                #Increment collision counter
                self.wall_collisions+=1
                #Check whether collision in x or y direction
                _coll_ind=np.argwhere(_wall_diff>0)
                #For component(s) to be reflected
                for c in _coll_ind:
                    #Reflect velocity
                    _v[c]=-_v[c]
                    #Increment wall momentum
                    self.wall_momentum+=2*_m*np.abs(_v[c])
                #Update velocity
                m.set_velocity(_v)
                #Update position based on new velocity
                _x_new=_x + _v * self.t_step

            #Check collisions with the bottom and left walls    
            if _x_new[_x_new<=0].shape[0] > 0:
                #Increment collision counter
                self.wall_collisions+=1
                #Check whether collision in x or y direction
                _coll_ind=np.argwhere(_x_new<0)
                #For component(s) to be reflected
                for c in _coll_ind:
                    #Reflect velocity
                    _v[c]=-_v[c]
                    #Increment wall momentum
                    self.wall_momentum+=2*_m*np.abs(_v[c])
                #Update velocity    
                m.set_velocity(_v)
                #Update position based on new velocity
                _x_new=_x + _v * self.t_step

            #Update position of the molecule object            
            m.set_position(_x_new)

        #Construct matrices with updated positions and velocities
        self.make_matrices()
```

就这样，困难的部分完成了！最后，我们需要编写一个函数来运行模拟。这涉及到在循环中调用之前定义的函数，该循环运行指定次数，计算基于指定的模拟时间和时间步长。

```py
 #Still in Simulation class
    def safe_division(self,n,d):
        if d==0:
            return 0
        else:
            return n/d

    def run_simulation(self,max_time):
        #Print "Starting simulation"
        print("Starting simulation...")

        #Make matrices
        self.make_matrices()

        #Calculate number of iterations
        self.max_time=max_time
        self.n_iters=int(np.floor(self.max_time/self.t_step))

        #Make tensors to store positions and velocities of all molecules at each timestep f
        self.x_dynamics=np.zeros(((self.n_molecules,self.dim,self.n_iters)))
        self.v_dynamics=np.zeros((self.n_molecules,self.n_iters))

        #In each iteration
        for i in range(self.n_iters):                   
            #Save positions and velocities to the defined tensors
            self.x_dynamics[:,:,i]=self.positions
            self.v_dynamics[:,i]=self.velocities_norm

            #Calculate rms velocity
            self.v_rms=np.sum(np.sqrt(self.velocities_norm**2))/self.velocities_norm.shape[0]

            #Print current iteration information
            _P=self.safe_division(self.wall_momentum,i*self.t_step*np.sum(self.box_dim))
            print("Iteration:{0:d}\tTime:{1:.2E}\tV_RMS:{2:.2E}\tWall Pressure:{3}".format(i,i*self.t_step,self.v_rms,_P))

            #Call the update_positions function to handle collisions and update positions
            self.update_positions()

        #Caclulate final pressure
        self.P=self.wall_momentum/(self.n_iters*self.t_step*np.sum(self.box_dim))
        print("Average pressure on wall: {0}".format(self.P))
        return self.P
```

这就结束了涉及模拟物理的代码。然而，运行模拟是没有乐趣的，如果你不能可视化它。让我们利用 matplotlib 创建模拟的动画。

## 动画模拟框

我们从编写一个函数开始，创建一个与提供的盒子尺寸一致的纵横比的图形。

```py
 #Still in Simulation class
    def create_2D_box(self):
        fig=plt.figure(figsize=(10,10*self.box_dim[1]/self.box_dim[0]),dpi=300)
        return fig
```

我们将使用 matplotlib 中的 Animation 模块来制作动画。为了利用这一点，我们需要定义一个函数，该函数将迭代次数（帧）作为输入并创建一个图表。这个函数然后作为参数提供给 Animation 模块中的 FuncAnimation 函数。

```py
 #Still in Simulation class
    def show_molecules(self,i):
        #Clear axes
        plt.cla()

        #Plot a line showing the trajectory of a single molecule
        plt.plot(self.x_dynamics[0,0,:i+1],self.x_dynamics[0,1,:i+1],color="red",linewidth=1.,linestyle="-")

        #Plot a single molecule in red that is being tracked 
        plt.scatter(self.x_dynamics[0,0,i],self.x_dynamics[0,1,i],color="red",s=20)

        #Plot the rest of the molecules
        plt.scatter(self.x_dynamics[1:,0,i],self.x_dynamics[1:,1,i],color=self.colors[1:],s=20)

        #Remove ticks on the plot
        plt.xticks([])
        plt.yticks([])

        #Set margins to 0
        plt.margins(0)

        #Set the limits of the box according to the box dimensions
        plt.xlim([0,self.box_dim[0]])
        plt.ylim([0,self.box_dim[1]])

    def make_animation(self,filename="KTG_animation.mp4"):
        #Call the function to create the figure
        fig=self.create_2D_box()

        #Create the animation
        anim=FuncAnimation(fig,self.show_molecules,frames=self.n_iters,interval=50,blit=False)

        #Save animation as a file
        anim.save(filename,writer="ffmpeg")
```

还有另一种动画，我们可以制作，显示每次迭代的速度直方图。这将允许我们观察速度分布收敛到麦克斯韦-玻尔兹曼分布的过程。

```py
 #Still in Simulation class
    def plot_hist(self,i):
        #Clear axes
        plt.cla()

        #Make histogram
        plt.hist(self.v_dynamics[:,i],density=True,color="plum",edgecolor="black")

        #Define axis limits
        plt.xlim([0,3])
        plt.ylim([0,3])

    def make_velocity_histogram_animation(self,filename="KTG_histogram.mp4"):
        #Create empty figure
        fig=plt.figure(figsize=(5,5),dpi=500)

        #Create animation
        anim_hist=FuncAnimation(fig,self.plot_hist,frames=self.n_iters,interval=50,blit=False)

        #Save animation
        anim_hist.save(filename,writer="ffmpeg")
```

# 运行模拟

是时候收获我们的辛勤工作成果了。以面向对象的方式编写代码所花费的努力现在将得到回报，因为我们现在有了一个通用的求解器，并且可以用仅几行代码运行具有不同输入参数的模拟。

```py
if __name__=="__main__":
    #Create simulation object and define input parameters
    sim=Simulation(name="kinetic_theory_simulation",box_dim=[1.0,1.0],t_step=1e-2,particle_radius=1e-2)

    #Add N2 molecules to the box
    sim.add_molecules("N2",n=100,v_mean=1.0,v_std=0.2,v_dist="normal")

    #Run the simulation and store the pressure output in P
    P=sim.run_simulation(15)

    #Make the box animation
    sim.make_animation()

    #Make the histogram animation
    sim.make_velocity_histogram_animation()
```

下面显示了与上述模拟相对应的模拟框和速度直方图的动画。在第一个图中，显示了盒子中分子的运动及其碰撞，一个选定分子的轨迹以红色突出显示，以作说明。在第二个图中，显示了盒子中所有分子在每次迭代时的速度直方图，从动画中可以清楚地看到，最初的高斯分布（如规定的）变化为具有较窄左尾和较宽右尾的分布，模拟了麦克斯韦-玻尔兹曼分布的特征。可以使用更严格的统计测试来定量支持这一点。

**图1：** 模拟框的动画

**图2：** 速度直方图的动画

## 提取热力学见解

我们回到将各种热力学变量相互关联的理想气体定律。如前所述，我们测试了两个关系——压力与体积的关系以及压力与温度的关系。我们保持盒子中的分子数量在所有后续模拟中不变。这三个变量——压力、体积和温度——的计算方法如下：压力是整个模拟过程中与墙壁交换的净动量除以总模拟时间和盒子周长的乘积。体积定义为盒子的长和宽的乘积（技术上是面积，因为我们在二维中工作，但这些见解可以轻松地推广到三维）。 

定义温度较为复杂——由于温度与盒子中分子的平均动能成正比，我们将初始分布的平均速度的平方视为温度的代理。为了消除这种估计中的任何随机性，这些模拟中分子的初始速度被设置为一个指定的单一值。例如，如果指定值为1 m/s，那么所有分子的初始速度要么是+1 m/s，要么是-1 m/s。这确保了初始的总动能有一个明确定义的值，并且在所有具有相同温度的模拟中保持不变。本质上，当两个模拟中的温度相同时，它们的初始总动能也相同，这应该确保在模拟过程中平均动能也相同。

模拟结果见**图2**。在恒定温度下，盒子墙壁上的平均压力随着体积倒数的增加而线性增加（见**图2a**）。每条等温线的斜率与温度成正比，与理想气体定律一致。在第二组模拟中，观察到在恒定体积下，压力随着温度的增加而线性增加（见**图2b**）。在这种情况下，每条等容线的斜率与体积成反比，也与理想气体定律一致。因此，这些微观模拟能够重现与理想气体定律等宏观理论一致的热力学变量趋势。

![](../Images/2f499719677894ae4a715dee84bb0b12.png)

**图2：** **（a）** 不同温度下壁面平均压力与体积倒数的变化，**（b）** 不同体积下壁面平均压力与温度的变化

# 结论

本文中呈现的n体模拟是一个简单的分子动力学模拟示例，没有任何交互作用。当然，这对分子如何相互作用的描述是极其简化的，但正如我们所见，它足以预测理想气体的性质。然而，理想气体假设在计算工程应用中气体的性质时很少被使用，例如在蒸汽涡轮中的蒸汽膨胀。需要更复杂的状态方程模型来准确模拟这样的过程，这些模型包括分子之间的相互作用。在本代码中添加分子间的短程相互作用可以更好地再现这些模型对真实气体预测的趋势。此外，使用类似Lennard-Jones的势能和添加温控器也可以允许预测液体的性质。

本模拟的完整代码可以在[GitHub](https://github.com/gauravsdeshmukh/ktg-python)上获取。如果你有任何问题、建议或评论，请随时通过[电子邮件](mailto:gauravsdeshmukh@outlook.com)或[Twitter](https://twitter.com/intent/follow?screen_name=ChemAndCode)与我联系。除非另有说明，所有图片均由作者提供。

要了解更多关于气体动理论的历史，请参阅：

S. Brush, [气体动理论的历史](https://www.terpconnect.umd.edu/~brush/pdf/ITALENC.pdf) (2004)
