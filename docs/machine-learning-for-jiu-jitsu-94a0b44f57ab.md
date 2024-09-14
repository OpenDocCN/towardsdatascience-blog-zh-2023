# 使用机器学习进行柔术

> 原文：[`towardsdatascience.com/machine-learning-for-jiu-jitsu-94a0b44f57ab`](https://towardsdatascience.com/machine-learning-for-jiu-jitsu-94a0b44f57ab)

![](img/991deef0a7510879d427ecc024fde83e.png)

*照片由 Kampus Production 提供，来自 Pexels:* [*https://www.pexels.com/photo/a-judoka-throwing-an-opponent-to-the-ground-6765024/*](https://www.pexels.com/photo/a-judoka-throwing-an-opponent-to-the-ground-6765024/*)

## 使用 mediapipe 的姿态估计来跟踪柔术动作

[](https://lucas-soares.medium.com/?source=post_page-----94a0b44f57ab--------------------------------)![Lucas Soares](https://lucas-soares.medium.com/?source=post_page-----94a0b44f57ab--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94a0b44f57ab--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94a0b44f57ab--------------------------------) [Lucas Soares](https://lucas-soares.medium.com/?source=post_page-----94a0b44f57ab--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----94a0b44f57ab--------------------------------) ·阅读时间 18 分钟·2023 年 3 月 13 日

--

# **姿态跟踪以提升柔术水平**

巴西柔术是一种最近因其在实际战斗中的有效性和适用性而变得非常受欢迎的武术。

我已经练习巴西柔术超过 10 年，并决定将我对武术和机器学习的兴趣结合起来，提出一个位于这两个非常有趣领域交汇处的项目。

因此，我转向了[姿态估计](https://en.wikipedia.org/wiki/3D_pose_estimation)，作为一种有前景的技术，用于作为辅助工具帮助我在柔术中的发展。

***在本文中，我想与大家分享如何使用姿态跟踪来增强战斗动作中的反馈纠正。***

如果你更喜欢视频，可以在这里查看我关于此主题的 YouTube 视频：

# **什么是姿态跟踪？**

姿态跟踪是利用计算机视觉技术实时检测和跟踪人体运动的过程。它涉及使用算法捕捉和解释各种身体部位（如手臂、腿部和躯干）的运动。

这一技术对于分析运动中的身体动作具有相关性，因为它允许教练和运动员识别和纠正可能对表现产生负面影响或导致受伤的运动模式。

通过提供实时反馈，运动员可以调整他们的技术，从而提高表现并减少受伤风险。此外，这项技术还可以用于将动作与顶级运动员的动作进行比较，例如，帮助初学者识别需要改进的领域，并相应地完善他们的技术。

# **什么是 Jiu Jitsu？**

Jiu Jitsu 是一种以通过组合使用固定技和提交保持（如关节锁和窒息）来制服对手的武术。

Jiu Jitsu 专注于抓取和地面战斗技巧。它最初在日本开发，后来在巴西进行了修改和推广。但现在，由于其在美国特别是普及率的增加，它已经传播到全球。

基本原则是，较小、较弱的人可以通过使用杠杆和技巧来防御较大、较强的对手。练习者的目标是控制对手的身体，并将自己置于一个主导位置，以便执行诸如窒息、关节锁和投掷等技巧。

![](img/3386f99ab575e1925801eea91c2bce27.png)

*图片由 Timoth Eberly 提供，链接：* [*https://unsplash.com/photos/7MRajrPiTqw**](https://unsplash.com/photos/7MRajrPiTqw*)

Jiu Jitsu 现在是一项在全球范围内流行的运动和自我防卫系统。它要求身体和心理上的纪律，以及学习和适应的意愿。

研究还发现，它具有许多好处，包括 [改善身体健康和心理敏锐性、增加自信和自尊、以及缓解压力](https://era.library.ualberta.ca/items/205b4732-a385-427c-b12e-45f25d9e5150)。

# **为什么选择 Jiu Jitsu 的姿势追踪**

对技巧的高度重视使得这门武术相当独特，在 Jiu Jitsu 俱乐部的环境中，通常是黑带教练的职责来给学生反馈，评价他们对不同技巧的执行是否得当。

然而，人们常常希望学习，但要么无法接触到专家，要么班级人数过多，导致授课者难以提供具体和个人化的反馈，无法确定学生是否正确执行了动作。

在这种反馈的空白中，我认为像姿势追踪这样的工具可以极大地惠及武术世界，尤其是 Jiu Jitsu（尽管可以对柔道、摔跤和以打击为基础的武术做同样的论证），因为它们可以无缝集成到智能手机中，只需运动员在尝试改进的动作时拍摄自己。

这种反馈的形式需要进行开发，本文旨在提供有关这种基于机器学习的反馈系统如何帮助学生提高运动基础动作的指导。

# **我为什么要这样做？**

好的，故事是这样的。

通常，当你发展你的柔术技能时，你最终会落入两个类别之一：下位选手或上位选手。这意味着你是否倾向于从下方使用“防守”（指使用双腿对对手进行攻击）进行比赛，还是从上方先将对手摔倒，然后继续穿过对方的双腿（通常）达到如骑乘对手或抓住对手背部等占据主导地位的目标。

![](img/36475135bb977f7f9b6d4fa11c0377db.png)

图片由诺兰·肯特提供，来源于 [`unsplash.com/photos/x_V62hOwnDk?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink`](https://unsplash.com/photos/x_V62hOwnDk?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

这种二元性显然是人为的，通常，大多数经验丰富的选手能很好地掌握两种位置。

然而，许多人在柔术旅程的开始时倾向于偏好某些技巧，这可能会严重影响他们在其他领域的进步，如果他们不断重复同样的策略的话。

在某种程度上，这就是我所经历的，我曾经作为防守选手打斗很多，这主要是由于巴西的主流文化，鼓励从膝盖开始摔跤以避免受伤，或者因为比赛垫的空间不像美国大中学体育馆中的摔跤垫那么大。

![](img/537ad1e9057363a6fe037e3587aa4390.png)

*作者提供的图片。比赛中我进行防守的照片。*

这种主动坐下来并从背后打斗的习惯，未能让对手参与站立战斗，对我的武术发展产生了负面影响，因为随着我在柔术中变得越来越好，我意识到阻碍我的一个因素是我缺乏高水平的摔倒对手的知识。

这激发了我从站立位置开始更多地进行训练，于是我在获得棕带的几年后，开始学习和练习摔跤和柔道。

在过去的 2 年里，我主要是一个上位选手，确实在将对手摔倒在地的能力上有了很大提升。

![](img/f5f342992992a27c4331a53aa88c4002.png)

*作者提供的图片。我的摔跤之旅。*

不过，例如在柔道中有一些基础动作非常难以掌握，因为我不认识任何柔道专家，也不住在任何高水平的柔道或摔跤馆附近，我意识到我需要另一种方式来提高某些基础动作，特别是像“内腿技”和其他基于髋部的摔法的髋部灵活性。

![](img/991deef0a7510879d427ecc024fde83e.png)

*照片由 Kampus Production 提供，来源于:* [*https://www.pexels.com/photo/a-judoka-throwing-an-opponent-to-the-ground-6765024/*](https://www.pexels.com/photo/a-judoka-throwing-an-opponent-to-the-ground-6765024/*)

# **机器学习的作用**

好的，为了提高我执行像内股这样的柔道投掷技术的能力，我制定了一个“书呆子”的计划：我要使用机器学习（我知道，这个计划真是太具体了）。

我决定要调查是否可以使用姿态追踪来获取关于如何纠正脚的速度和方向以及执行这些动作的其他方面的见解。

那么让我们来看看我是怎么做的。

# **使用姿态追踪生成柔术见解的步骤**

整体计划是这样的：

1\. 找到一个包含我想要模仿动作的视频参考

2\. 录制自己多次执行该动作

3\. 使用姿态追踪和 Python 可视化生成见解。

为了做到这一点，我需要一个顶级练习者执行我试图学习的动作的参考视频。对于内股，我找到了一段奥林匹克级别选手在墙边进行热身的技术视频，这直接与我想要学习的内容相关：

然后我开始录制自己执行这些动作的视频，至少是我正在积极学习的某些动作。

![](img/e9845ae3305cf210f0956feeeed1ea42.png)

图片由作者提供。

拥有了参考视频，并且录制了一些自己的镜头之后，我准备尝试一些有趣的机器学习内容。

# **姿态追踪来追踪身体关节**

对于姿态追踪，我使用了一个叫做[mediapipe](https://google.github.io/mediapipe/)的工具，这是谷歌的开源项目，旨在促进机器学习在实时和流媒体中的应用。

[## GitHub - google/mediapipe: 跨平台、可定制的机器学习解决方案，适用于实时和流媒体。](https://github.com/google/mediapipe?source=post_page-----94a0b44f57ab--------------------------------)

### MediaPipe 提供跨平台、可定制的机器学习解决方案，适用于实时和流媒体。端到端加速……

[github.com](https://github.com/google/mediapipe?source=post_page-----94a0b44f57ab--------------------------------)

这个选项的易用性让我很兴奋，迫不及待地想要尝试。

本质上，我做了以下工作：

**1\. 首先，我创建了一些叠加姿态估计的视频**

**2\. 创建了实时绘图，展示脚的 x、y 和 z 坐标，以说明动作的主要方面**

**3\. 创建了表示某个动作在特定时间执行的轨迹**

**4\. 将我的尝试所产生的轨迹与专家视频生成的参考轨迹进行比较**

# **初步结果**

## **1\. 姿态估计叠加**

我写了这段代码来创建模型估计身体关节位置的视频

并将其叠加在实际镜头中，以展示模型的鲁棒性。

![](img/d554d70f9b835e6d4d71854e500e80a4.png)

图片由作者提供

是的，是的，我知道，我看起来并不完全像顶级选手。但给我一点时间，我的柔道技能还在建设中！

我用来做这个的代码是：

```py
from base64 import b64encode
import cv2
import mediapipe as mp
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
import numpy as np
from natsort import natsorted
from mpl_toolkits.mplot3d import Axes3D
from matplotlib.animation import FuncAnimation
from IPython.display import clear_output
%matplotlib inline
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px
from IPython.display import HTML, display
import ipywidgets as widgets
from typing import List # I don't think I need this!

# Custom imports
from pose_tracking_utils import *

mp_drawing = mp.solutions.drawing_utils
mp_drawing_styles = mp.solutions.drawing_styles
mp_pose = mp.solutions.pose

def create_pose_tracking_video(video_path):
    # For webcam input:
    cap = cv2.VideoCapture(video_path)
    frame_width = int(cap.get(3))
    frame_height = int(cap.get(4))
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    output_path = pathlib.Path(video_path).stem + "_pose.mp4" 
    out = cv2.VideoWriter(output_path, fourcc, 30.0, (frame_width, frame_height))
    with mp_pose.Pose(min_detection_confidence=0.5,
                      min_tracking_confidence=0.5) as pose:
        while cap.isOpened():
            success, image = cap.read()
            if not success:
                print("Ignoring empty camera frame.")
                break
            # To improve performance, optinally mark the iamge as 
            # not writeable to pass by reference.
            image.flags.writeable = False
            image= cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
            results = pose.process(image)
            # Draw the annotation on the image.
            image.flags.writeable = True
            image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
            mp_drawing.draw_landmarks(image, results.pose_landmarks,
                                      mp_pose.POSE_CONNECTIONS,
            landmark_drawing_spec=mp_drawing_styles.get_default_pose_landmarks_style())

            # Flip the image horizontally for a self-view display.
            out.write(cv2.flip(image, 1))
            if cv2.waitKey(5) & 0xFF == 27:
                break

    cap.release()
    out.release()
    print("Pose video created!")

    return output_path
```

这基本上利用了 mediapipe 包来生成一个可视化，它检测关键点并将其覆盖在视频画面上。

## **2\. 脚部的 X、Y 和 Z 坐标的实时图表**

```py
VIDEO_PATH = "./videos/clip_training_session_1.mp4"
# Initialize MediaPipe Pose model
body_part_index = 32
pose = mp_pose.Pose(static_image_mode=False, min_detection_confidence=0.5, min_tracking_confidence=0.5)

# Initialize OpenCV VideoCapture object to capture video from the camera
cap = cv2.VideoCapture(VIDEO_PATH)

# Create an empty list to store the trace of the right elbow
trace = []

# Create empty lists to store the x, y, z coordinates of the right elbow
x_vals = []
y_vals = []
z_vals = []

# Create a Matplotlib figure and subplot for the real-time updating plot
# fig, ax = plt.subplots()
# plt.title('Time Lapse of the X Coordinate')
# plt.xlabel('Frames')
# plt.ylabel('Coordinate Value')
# plt.xlim(0,1)
# plt.ylim(0,1)
# plt.ion()
# plt.show()
frame_num = 0

while True:
    # Read a frame from the video capture
    success, image = cap.read()
    if not success:
        break
    # Convert the frame to RGB format
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    # Process the frame with MediaPipe Pose model
    results = pose.process(image)

    # Check if any body parts are detected

    if results.pose_landmarks:
        # Get the x,y,z coordinates of the right elbow
        x, y, z = results.pose_landmarks.landmark[body_part_index].x, results.pose_landmarks.landmark[body_part_index].y, results.pose_landmarks.landmark[body_part_index].z

        # Append the x, y, z values to the corresponding lists
        x_vals.append(x)
        y_vals.append(y)
        z_vals.append(z)

        # # Add the (x, y) coordinates to the trace list
        trace.append((int(x * image.shape[1]), int(y * image.shape[0])))

        # Draw the trace on the image
        for i in range(len(trace)-1):
            cv2.line(image, trace[i], trace[i+1], (255, 0, 0), thickness=2)

        plt.title('Time Lapse of the Y Coordinate')
        plt.xlabel('Frames')
        plt.ylabel('Coordinate Value')
        plt.xlim(0,len(pose_coords))
        plt.ylim(0,1)
        plt.plot(y_vals);
        # Clear the plot and update with the new x, y, z coordinate values
        #ax.clear()
        # ax.plot(range(0, frame_num + 1), x_vals, 'r.', label='x')
        # ax.plot(range(0, frame_num + 1), y_vals, 'g.', label='y')
        # ax.plot(range(0, frame_num + 1), z_vals, 'b.', label='z')
        # ax.legend(loc='upper left')
        # plt.draw()
        plt.pause(0.00000000001)
        clear_output(wait=True)
        frame_num += 1

    # Convert the image back to BGR format for display
    image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)

    # Display the image
    cv2.imshow('Pose Tracking', image)

    # Wait for user input to exit
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Release the video capture, close all windows, and clear the plot
cap.release()
cv2.destroyAllWindows()
plt.close()
```

然后，我生成了一个包含 x、y、z 坐标时间线的图表：

```py
plt.figure(figsize=(15,7))
plt.subplot(3,1,1)
plt.title('Time Lapse of the x Coordinate')
plt.xlabel('Frames')
plt.ylabel('Coordinate Value')
plt.xlim(0,len(pose_coords))
plt.ylim(0,1)
plt.plot(x_vals)

plt.subplot(3,1,2)
plt.title('Time Lapse of the y Coordinate')
plt.xlabel('Frames')
plt.ylabel('Coordinate Value')
plt.xlim(0,len(pose_coords))
plt.ylim(0,1.1)
plt.plot(y_vals)

plt.subplot(3,1,3)
plt.title('Time Lapse of the z Coordinate')
plt.xlabel('Frames')
plt.ylabel('Coordinate Value')
plt.xlim(0,len(pose_coords))
plt.ylim(-1,1)
plt.plot(z_vals)

plt.tight_layout();
```

这个想法是为了对诸如执行动作时脚的位置方向等细节进行细致控制。

现在我对模型能够正确捕捉我的身体姿势感到自信，我创建了一些相关身体关节的轨迹可视化，比如脚部（在执行摔跤技术时非常重要）。

## **3\. 创建运动轨迹**

为了了解动作的执行情况，我制作了一个可视化，表示从身体部位的角度（在这种情况下是脚）执行该动作的过程：

![](img/0985400eb75965f967e0465b4b7be0a5.png)

作者提供的图片

我为我的训练课程和包含我试图模仿的动作的参考视频做了这个。

这里需要注意的是，进行此操作存在许多问题，包括相机的分辨率、动作执行的距离以及每个视频的帧率。然而，我只是绕过了这些问题，创建了一个花哨的图表（哈哈）。

这是这种方法的代码：

```py
def create_joint_trace_video(video_path,body_part_index=32, color_rgb=(255,0,0)):
    """
    This function creates a trace of the body part being tracked.
    body_part_index: The index of the body part being tracked.
    video_path: The path to the video being analysed.
    """
    # Initialize MediaPipe Pose modelpose = mp_pose.Pose(static_image_mode=False, min_detection_confidence=0.5, min_tracking_confidence=0.5)

    # Initialize OpenCV VideoCapture object to capture video from the camera
    cap = cv2.VideoCapture(video_path)
    frame_width = int(cap.get(3))
    frame_height = int(cap.get(4))
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    output_path = pathlib.Path(video_path).stem + "_trace.mp4" 
    out = cv2.VideoWriter(output_path, fourcc, 30.0, (frame_width, frame_height))

    # Create an empty list to store the trace of the body part being tracked
    trace = []

    with mp_pose.Pose(min_detection_confidence=0.5,
                        min_tracking_confidence=0.5) as pose:
        while cap.isOpened():
            success, image = cap.read()
            if not success:
                print("Ignoring empty camera frame.")
                break

            # Convert the frame to RGB format
            image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

            # Process the frame with MediaPipe Pose model
            results = pose.process(image)

            # Check if any body parts are detected
            if results.pose_landmarks:
                # Get the x,y coordinates of the body part being tracked (in this case, the right elbow)
                x, y = int(results.pose_landmarks.landmark[body_part_index].x * image.shape[1]), int(results.pose_landmarks.landmark[body_part_index].y * image.shape[0])

                # Add the coordinates to the trace list
                trace.append((x, y))

                # Draw the trace on the image
                for i in range(len(trace)-1):
                    cv2.line(image, trace[i], trace[i+1], color_rgb, thickness=2)

            # Convert the image back to BGR format for display
            image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)

            # Display the image
            out.write(image)
            if cv2.waitKey(5) & 0xFF == 27:
                break

    cap.release()
    out.release()
    print("Joint Trace video created!")
```

在这里，我简单地处理每一帧，就像之前制作姿势视频一样，然而，我还将特定身体部位的 x 和 y 坐标追加到一个我称之为`trace`的列表中，这个列表用于生成伴随身体部位的轨迹线。

## 4\. 比较轨迹

拥有这些能力后，我终于可以进入从这种方法中获取见解的阶段。

为了做到这一点，我需要一种比较这些轨迹的方法，以生成一些视觉丰富的反馈，这可以帮助我理解自己动作执行的不足与顶级运动员的表现相比如何。

现在，没有背景视频的实际轨迹已被绘制成图表。

```py
def get_joint_trace_data(video_path, body_part_index,xmin=300,xmax=1000,
                             ymin=200,ymax=800):
    """
    Creates a graph with the tracing of a particular body part,
    while executing a certain movement.
    """
    cap = cv2.VideoCapture(video_path)
    frame_width = int(cap.get(3))
    frame_height = int(cap.get(4))

    # Create an empty list to store the trace of the body part being tracked
    trace = []
    i = 0
    with mp_pose.Pose(min_detection_confidence=0.5,
                    min_tracking_confidence=0.5) as pose:
        while cap.isOpened():
            success, image = cap.read()
            if not success:
                print("Ignoring empty camera frame.")
                break

            # Convert the frame to RGB format
            image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

            # Process the frame with MediaPipe Pose model
            results = pose.process(image)

            # Check if any body parts are detected
            if results.pose_landmarks:
                # Get the x,y coordinates of the body part being tracked (in this case, the right elbow)
                x, y = int(results.pose_landmarks.landmark[body_part_index].x * image.shape[1]), int(results.pose_landmarks.landmark[body_part_index].y * image.shape[0])

                # Add the coordinates to the trace list
                trace.append((x, y))

                # Plot the trace on the graph
                fig, ax = plt.subplots()
                #ax.imshow(image)
                ax.set_xlim(xmin,xmax)
                ax.set_ylim(ymin,ymax)
                ax.invert_yaxis()
                ax.plot(np.array(trace)[:, 0], np.array(trace)[:, 1], color='r')
                # plt.savefig(f'joint_trace{i}.png')
                # plt.close()
                i+=1
                plt.pause(0.00000000001)
                clear_output(wait=True)
                # Display the graph
                #plt.show()

            if cv2.waitKey(5) & 0xFF == 27:
                break

        cap.release()

        return trace

video_path = "./videos/clip_training_session_2.mp4"
body_part_index = 31
foot_trace = get_joint_trace_data(video_path, body_part_index)

video_path = "./videos/uchimata_wall.mp4"
body_part_index = 31
foot_trace_reference = get_joint_trace_data(video_path, body_part_index,xmin=0,ymin=0,xmax=1300)

foot_trace_clip = foot_trace[:len(foot_trace_reference)]
plt.subplot(1,2,1)
plt.plot(np.array(foot_trace_clip)[:, 0], np.array(foot_trace_clip)[:, 1], color='r')
plt.gca().invert_yaxis();

plt.subplot(1,2,2)
plt.plot(np.array(foot_trace_reference)[:, 0], np.array(foot_trace_reference)[:, 1], color='g')
plt.gca().invert_yaxis();
```

好的，通过这些，我们开始更清楚地看到在不同背景下脚部移动的特征形状之间的差异。

首先，我们看到，虽然顶级运动员在转弯时做了一个比较直的步骤，用脚生成了一个几乎完整的半圆，而我则在初步步骤内有一个弯曲的外观，并且在将腿抬到空中时也没有生成半圆。

此外，虽然顶级运动员在抬起腿时会生成一个宽大的圆圈，而我则会生成一个浅圆圈，几乎像是一个椭圆。

![](img/bc7057e050b5da3063f48beb53879f87.png)

作者提供的图片，比较动作执行的轨迹

我发现这些初步结果相当不错，因为它们表明，尽管比较存在局限性，但通过观察这些轨迹，可以评估动作执行的特征形状之间的差异。

除此之外，我还想看看是否可以比较动作执行的速度，为此我可视化了身体关节的实时运动，将我和专家的图放在一起，看看我的时机偏差有多大。

这项分析的挑战在于，由于视频的速度各不相同且未对齐，我首先需要以有意义的方式对齐它们。

我不确定该使用哪种技术，但与我的朋友 Aaron（里斯本 Champalimaud 神经科学研究所的神经科学家）的对话让我有了一个选择：动态时间规整。

**使用动态时间规整比较速度和时机**

[动态时间规整（DTW）](https://en.wikipedia.org/wiki/Dynamic_time_warping)是一种用于测量两个具有不同速度的时间序列之间相似度的技术。

基本思想是你有两个不同的时间序列，它们可能包含一些你希望分析的模式，因此你试图通过应用一些规则将它们对齐，从而计算两个序列之间的最佳匹配。

![](img/8f4a399e240ef0c0057a0c7c9f258984.png)

两次步态序列，尽管速度各异，我们可以观察到四肢的轨迹非常相似；取自[维基百科](https://en.wikipedia.org/wiki/Motion_capture#cite_note-2)，参考[(Olsen et al, 2017)](https://arxiv.org/pdf/1606.03295.pdf)。

我在这篇文章中找到了对这个话题的很好的介绍：

[](/dynamic-time-warping-3933f25fcdd?source=post_page-----94a0b44f57ab--------------------------------) ## 动态时间规整

### 解释与代码实现

towardsdatascience.com

作者：[Jeremy Zhang](https://medium.com/u/f37783fc8c26?source=post_page-----94a0b44f57ab--------------------------------)。

为了使用动态时间规整，我做了以下工作：

**1\. 将值归一化到相同范围**

**2\. 使用了 DTW 算法的 Python 实现。**

```py
from fastdtw import fastdtw
from scipy.spatial.distance import euclidean

max_x = max(max(foot_trace_clip, key=lambda x: x[0])[0], max(foot_trace_reference, key=lambda x: x[0])[0])
max_y = max(max(foot_trace_clip, key=lambda x: x[1])[1], max(foot_trace_reference, key=lambda x: x[1])[1])

foot_trace_clip_norm = [(x/max_x, y/max_y) for (x, y) in foot_trace_clip]
foot_trace_reference_norm = [(x/max_x, y/max_y) for (x, y) in foot_trace_reference]

distance, path = fastdtw(foot_trace_clip_norm, foot_trace_reference_norm, dist=euclidean)
```

我得到的输出是：

1\. `distance`：两个时间序列向量之间的[欧几里得距离](https://en.wikipedia.org/wiki/Euclidean_distance)。

2\. `path`：两个时间序列之间的映射，以嵌套的元组列表形式存在

现在，我可以使用存储在`path`变量中的输出，创建一个对齐了两个序列的图：

```py
foot_trace_reference_norm_mapped = [foot_trace_reference_norm[path[i][1]] for i in range(len(path))]
foot_trace_clip_norm_mapped = [foot_trace_clip_norm[path[i][1]] for i in range(len(path))]

plt.subplot(1,2,1)
plt.plot(np.array(foot_trace_reference_norm_mapped)[:, 0], np.array(foot_trace_reference_norm_mapped)[:, 1], color='g')
plt.gca().invert_yaxis();

plt.subplot(1,2,2)
plt.plot(np.array(foot_trace_clip_norm_mapped)[:, 0], np.array(foot_trace_clip_norm_mapped)[:, 1], color='r')
plt.gca().invert_yaxis();
plt.show()
```

![](img/ec936f43ab212df6ecdf9d00d1bc37cc.png)

图片由作者提供，*使用 DTW 算法对齐的时间序列*

现在，由于参考轨迹的数据不足，我不能说这个图比之前讨论的元素给了我更多的见解，然而，它确实有助于突出我之前提到的运动形状。

然而，作为未来的一个备注，我的想法是，如果可以满足某些条件来帮助使两个视频更一致，我希望有一个参考轨迹，我可以用来比较我的尝试轨迹，以便用于即时反馈。

我将使用 DTW 算法输出的欧几里得距离作为我的反馈指标，并且会有一个应用程序可以突出显示我是否接近或远离我尝试模仿的签名形状。

为了说明这一点，让我给你展示一个例子。

```py
def find_individual_traces(trace,window_size=60, color_plot="r"):
    """
    Function that takes in a liste of tuples containing x,y coordinates
    and plots them as different clips with varying sizes to allow the user to find
    the point where a full repetition has been completed
    """

    clip_size = 0
    for i in range(len(trace)//window_size):
        plt.plot(np.array(trace[clip_size:clip_size+window_size])[:, 0], np.array(trace[clip_size:clip_size+window_size])[:, 1], color=color_plot)
        plt.gca().invert_yaxis()
        plt.title(f"Trace, clip size = {clip_size}")
        plt.show()
        clip_size+=window_size

def get_individual_traces(trace, clip_size):
    num_clips = len(trace)//clip_size
    trace_clips = []
    i = 0
    for clip in range(num_clips):
        trace_clips.append(trace[i:i+clip_size])
        i+=clip_size

    return trace_clips

find_individual_traces(foot_trace_clip_norm)
```

![](img/9ba0a80609319ba5db156176de7c1d74.png)![](img/ed18abd8bd2187d7af45c7b5d6d8c2a2.png)![](img/d44210549874e5d8f23b22462333a1f0.png)

图片由作者提供。由我执行的脚部动作的轨迹。

这里我展示了视频中的剪辑，我在每个单独的动作中执行。每个这些轨迹可以与类似获得的参考轨迹进行比较：

```py
find_individual_traces(foot_trace_reference_norm, window_size=45,color_plot="g")
```

![](img/3d38c0f8ebb757594275cc4a0186ed34.png)![](img/f624096f5a7680b701e181afdddf7a9e.png)

图片由作者提供。由精英玩家执行的脚部动作的轨迹。

当我获得参考轨迹时，也会得到一些噪声信号，但我将使用第三个作为我的参考：

![](img/13d3d9da00dd635fe394a4c5ea167396.png)

图片由作者提供

现在我可以循环遍历代表我实际动作的轨迹，并查看它们如何与在几次训练课程中得到的参考轨迹进行比较。

```py
video_path = "./videos/clip_training_session_3.mp4"
body_part_index = 31
foot_trace_clip = get_joint_trace_data(video_path, body_part_index)

video_path = "./videos/uchimata_wall.mp4"
body_part_index = 31
foot_trace_reference = get_joint_trace_data(video_path, body_part_index,xmin=0,ymin=0,xmax=1300)

# Showing a plot with the tracings from the training session
plt.plot(np.array(foot_trace_clip)[:, 0], np.array(foot_trace_clip)[:, 1], color='r')
plt.gca().invert_yaxis();
```

![](img/a5d15027ebfe5983f40d292919616182.png)

图片由作者提供。几次执行动作中，脚的 x，y 坐标的轨迹。

现在我从两个轨迹中获取标准化值。

```py
max_x = max(max(foot_trace_clip, key=lambda x: x[0])[0], max(foot_trace_reference, key=lambda x: x[0])[0])
max_y = max(max(foot_trace_clip, key=lambda x: x[1])[1], max(foot_trace_reference, key=lambda x: x[1])[1])

foot_trace_clip_norm = [(x/max_x, y/max_y) for (x, y) in foot_trace_clip]
foot_trace_reference_norm = [(x/max_x, y/max_y) for (x, y) in foot_trace_reference]
```

我从训练剪辑和参考轨迹中获取轨迹，以帮助我设定目标。

剪辑大小是手动设置的。

```py
traces = get_individual_traces(foot_trace_clip_norm, clip_size=67)
traces_ref = get_individual_traces(foot_trace_reference_norm, clip_size=60)
```

我展示了在经过经验观察手动分类为噪声后去除了一些轨迹的例子。

```py
# Here I show an example trace from the new clip
index = 0
color_plot = "black"
plt.plot(np.array(traces[index])[:, 0], np.array(traces[index])[:, 1], color=color_plot)
plt.gca().invert_yaxis()
plt.title(f"Trace {index}")
plt.show()
```

![](img/1d5ff8574debff8886f822b9830b31ff.png)

图片由作者提供

然后，我循环遍历轨迹，并将它们的得分与从精英玩家视频中获得的参考轨迹进行比较：

```py
trace_ref = traces[2]
trace_scores = []

for trace in traces:
    distance, path = fastdtw(trace, trace_ref, dist=euclidean)
    trace_scores.append(distance)

plt.plot(trace_scores, color="black")
plt.title("Trace Scores with DTW")
plt.xlabel("Trace Index")
plt.ylabel("Euclidean Distance Score")
plt.show()
```

![](img/a127c80bc1cbe27b9ac14c36560a0c92.png)

图片由作者提供

现在，我注意到的第一个奇怪的现象是指标的上下波动，这只能通过一些获得的轨迹指向脚下落而非上升来解释。

然而，这个图表的有趣之处在于，轨迹的得分似乎甚至略有改善，并且至少保持在 20（在这种情况下是两序列之间的欧几里得距离的度量）。

尽管此时无法明确解释这些数字，但我发现像这样的处理方法可以转换为一个可衡量的指标，用于比较一个动作相对于另一个动作的质量，这一点相当有见地。

# 最终备注

未来，我希望研究如何更好地提取训练片段，以获得每次动作执行的完美对齐段，以便产生更一致的结果。

总的来说，我认为做这些实验相当有趣，因为它突出了这种技术在提供动作的详细评估方面的力量，尽管它仍需大量工作才能成为一个有用的洞察工具。

如果你喜欢这篇文章， [加入 Medium](https://lucas-soares.medium.com/membership)，并订阅我的 [Youtube 频道](https://www.youtube.com/channel/UCu8WF59Scx9f3H1N_FgZUwQ) 和 [我的新闻通讯](https://lucas-soares.medium.com/subscribe)。谢谢，下次见！ :)

# 参考文献

+   [巴西柔术的好处](https://era.library.ualberta.ca/items/205b4732-a385-427c-b12e-45f25d9e5150?utm_source=Victor+Mignone&utm_medium=Pingback&utm_campaign=weeklyboost&utm_term=002)
