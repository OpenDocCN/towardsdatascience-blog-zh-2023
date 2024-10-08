# 使用 Python 的面部检测——面部识别的前奏

> 原文：[`towardsdatascience.com/face-detection-using-python-the-precursor-to-face-recognition-316ded4d116f`](https://towardsdatascience.com/face-detection-using-python-the-precursor-to-face-recognition-316ded4d116f)

## 通过使用你的网络摄像头来检测你的面孔，享受 Python 的乐趣

[](https://weimenglee.medium.com/?source=post_page-----316ded4d116f--------------------------------)![魏梦霖](https://weimenglee.medium.com/?source=post_page-----316ded4d116f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----316ded4d116f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----316ded4d116f--------------------------------) [魏梦霖](https://weimenglee.medium.com/?source=post_page-----316ded4d116f--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----316ded4d116f--------------------------------) ·8 分钟阅读·2023 年 4 月 11 日

--

![](img/dc686dbd8129b723c18318609f2ec438.png)

作者提供的所有图片

*面部检测*是一种在数字图像中识别人的面孔的技术。面部检测是一项相对成熟的技术——还记得在你用数字相机时，当你通过取景器查看时，你看到围绕人脸的矩形吗？

> *面部检测是你在进行面部识别之前需要学习的技术，面部识别试图将面孔与名字对应起来。*

对于面部检测，最著名的算法之一被称为***Viola-Jones 面部检测***技术，通常称为**Haar cascades**。Haar cascades 的发明早于深度学习流行，是检测面孔的最常用技术之一。

# 面部检测/识别的伦理考量

虽然检测和识别面孔的能力确实很酷，但它确实有很多伦理问题。在将面部识别技术应用到你的项目中之前，有几个问题需要注意。比如**隐私**（面部检测可能被用来在未经同意的情况下追踪人们的行动）、**偏见**（面部检测可能对不同种族、性别或年龄的人有偏见）、**滥用**（捕捉到的面孔可能被用于其他非法用途或恶意目的）。因此，尽管这篇文章关注面部检测的技术能力，但在你将其应用到工作中之前，你应仔细考虑道德和伦理的影响。

以下是一些可以实现面部检测/识别的低风险项目：

+   **考勤追踪**——你可以在学校或工作场所使用面部识别来进行考勤。

+   **个性化** — 使用人脸识别来个性化服务。一个好的例子是娱乐服务，例如根据用户的观看历史推荐特定的电视节目。

+   **安全性** — 使用人脸识别解锁非关键系统，如智能手机和计算机。

然而，在某些应用中使用人脸识别有严重的道德问题。以下是一些例子：

+   **执法** — 尽管人脸识别对执法有帮助，但其不准确性和偏见仍然是严重的担忧。

+   **监控** — 人脸识别技术在一些国家被用于监控和追踪其公民，尤其是异议人士。一些公司也使用人脸识别来监控员工的生产力，这直接侵犯了他们的隐私。

这里有一些文章，你可以阅读以了解更多关于人脸识别的法律和道德问题：

+   **美国的人脸识别：隐私问题和法律发展** — [`www.asisonline.org/security-management-magazine/monthly-issues/security-technology/archive/2021/december/facial-recognition-in-the-us-privacy-concerns-and-legal-developments/`](https://www.asisonline.org/security-management-magazine/monthly-issues/security-technology/archive/2021/december/facial-recognition-in-the-us-privacy-concerns-and-legal-developments/)

+   **面部识别软件的隐私和安全问题** — [`www.techrepublic.com/article/privacy-and-security-issues-associated-with-facial-recognition-software/`](https://www.techrepublic.com/article/privacy-and-security-issues-associated-with-facial-recognition-software/)

+   **关于人脸识别技术的 10 个担忧理由** — [`www.privacycompliancehub.com/gdpr-resources/10-reasons-to-be-concerned-about-facial-recognition-technology/`](https://www.privacycompliancehub.com/gdpr-resources/10-reasons-to-be-concerned-about-facial-recognition-technology/)

# Haar 级联分类器是如何工作的？

**Haar 级联分类器** 用于检测其训练过的对象。如果你对 Haar 级联分类器工作原理的数学解释感兴趣，可以查看 Paul Viola 和 Michael Jones 的论文 [`www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf`](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf)。

这里是 Haar 分类器用于人脸识别的高级概述：

+   首先，使用一组正面图像（人脸图像）和一组负面图像（无脸图像）来训练分类器。

+   然后从图像中提取特征。下图显示了一些从包含人脸的图像中提取的特征。

![](img/c57de5b145ab3ecf7b03ba0c71ee1ccd.png)

+   要从图像中检测面孔，你需要寻找通常在人脸上发现的各种特征的存在（见下图），例如眉毛，其中眉毛上方的区域比下面的区域要亮。

![](img/e6d93256d6f5a9e8f7e346b678b7f8e4.png)

+   当图像包含所有这些特征的组合时，它被认为包含一个面孔。

幸运的是，无需了解 Haar 级联的工作原理，**OpenCV**可以直接使用预训练的 Haar 级联进行面部检测，以及其他识别其他对象的 Haar 级联。预定义的 Haar 级联列表可在 GitHub 上找到，地址是[`github.com/opencv/opencv/tree/master/data/haarcascades`](https://github.com/opencv/opencv/tree/master/data/haarcascades)。

> **开源计算机视觉**（**OpenCV**）是一个开源计算机视觉和机器学习软件库，最初由英特尔开发。它旨在为计算机视觉应用提供通用基础设施，并加速机器感知在商业产品中的使用。OpenCV 带有多个预训练的 Haar 级联，可以检测眼睛、面孔、俄罗斯车牌、笑容等。

对于面部检测，你需要`haarcascade_frontalface_default.xml`文件，你可以从前一段中的 GitHub 链接下载。

# 安装 OpenCV

让我们尝试使用 OpenCV 进行面部检测。首先，你需要使用以下命令来安装它：

```py
!pip install opencv-python
```

对于本文中的示例，你需要创建一个名为**face_detection.py**的文件。首先用以下语句填充它以导入 OpenCV 库：

```py
import cv2
```

# 从摄像头读取

接下来的步骤是连接到你的摄像头并在屏幕上显示图像：

```py
import cv2

# default webcam
stream = cv2.VideoCapture(0)

while(True):
    # Capture frame-by-frame
    (grabbed, frame) = stream.read()

    # Show the frame
    cv2.imshow("Image", frame)
    key = cv2.waitKey(1) & 0xFF    
    if key == ord("q"):    # Press q to break out of the loop
        break

# Cleanup
stream.release()
cv2.waitKey(1)
cv2.destroyAllWindows()
cv2.waitKey(1)
```

要引用你的摄像头，使用`VideoCapture`类，并传递一个数字表示你的摄像头实例（`0`表示第一个摄像头，`1`表示第二个摄像头，以此类推）：

```py
stream = cv2.VideoCapture(0)
```

要不断从摄像头捕捉输入，使用无限循环（`while(True)`），然后读取每一帧并显示出来：

```py
 # Capture frame-by-frame
    (grabbed, frame) = stream.read()

    # Show the frame
    cv2.imshow("Image", frame)
```

为了让程序能够优雅地退出，等待用户按下键盘上的一个键。当按下“q”键时，循环终止：

```py
 key = cv2.waitKey(1) & 0xFF    
    if key == ord("q"):    # Press q to break out of the loop
        break
```

然后你可以进行清理工作：

```py
# Cleanup
stream.release()
cv2.waitKey(1)
cv2.destroyAllWindows()
cv2.waitKey(1)
```

要运行程序，请转到终端并输入：

```py
$ python face_detection.py
```

你现在应该能看到你的面孔：

![](img/5a099fac77ed129e1eb18f6211e226d0.png)

# 检测面孔

现在进入有趣的部分——检测面孔。首先，创建一个`CascadeClassifier`类的实例，并将**haarcascade_frontalface_default.xml**文件传递给它：

```py
import cv2

# for face detection
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
```

> 你需要复制`haarcascade_frontalface_default.xml`文件并将其放在与`face_detection.py`文件相同的文件夹中。你可以从[`github.com/opencv/opencv/tree/master/data/haarcascades`](https://github.com/opencv/opencv/tree/master/data/haarcascades)下载 XML 文件。

现在你可以使用`detectMultiScale()`函数来检测面孔：

```py
while(True):
    # Capture frame-by-frame
    (grabbed, frame) = stream.read()

    #===============DETECTING FACES============
    # Convert to grayscale
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Try to detect faces in the webcam
    faces = face_cascade.detectMultiScale(gray, 
                                          scaleFactor=1.3, 
                                          minNeighbors=5)

    # for each faces found
    for (x, y, w, h) in faces:        
        # Draw a rectangle around the face
        color = (0, 255, 255) # in BGR
        stroke = 5    
        cv2.rectangle(frame, (x, y), (x + w, y + h), 
            color, stroke)
    #===============DETECTING FACE=============

    # Show the frame
    cv2.imshow("Image", frame)
    key = cv2.waitKey(1) & 0xFF    
    if key == ord("q"):    # Press q to break out of the loop
        break
```

注意`detectMultiScale()`函数中的以下参数：

+   `scaleFactor` 参数允许你将捕获的图像重新缩放到一个新的尺寸，以便算法可以检测到面部。

+   `minNeighbors` 参数指定每个候选矩形应该有多少邻居才能保留它。这个参数会影响检测到的面部质量。较高的值会导致检测较少，但质量更高。通常，4 到 6 是一个较好的数字。

> 你可以调整这两个参数的值，以确保正确检测到面部。

当检测到面部时，你会想要在它们周围绘制矩形：

```py
# for each faces found
    for (x, y, w, h) in faces:        
        # Draw a rectangle around the face
        color = (0, 255, 255) # in BGR
        stroke = 5    
        cv2.rectangle(frame, (x, y), (x + w, y + h), 
            color, stroke)
```

当你再次运行**face_detection.py**文件时，你现在应该能够检测到面部：

![](img/ad46aab0034093f8d7e0848bc80c053f.png)

**face_detection.py** 文件的全部内容如下：

```py
import cv2

# for face detection
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')

# default webcam
stream = cv2.VideoCapture(0)

while(True):
    # Capture frame-by-frame
    (grabbed, frame) = stream.read()

    # Convert to grayscale
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Try to detect faces in the webcam
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.3, minNeighbors=5)

    # for each faces found
    for (x, y, w, h) in faces:        
        # Draw a rectangle around the face
        color = (0, 255, 255) # in BGR
        stroke = 5    
        cv2.rectangle(frame, (x, y), (x + w, y + h), 
            color, stroke)

    # Show the frame
    cv2.imshow("Image", frame)
    key = cv2.waitKey(1) & 0xFF    
    if key == ord("q"):    # Press q to break out of the loop
        break

# Cleanup
stream.release()
cv2.waitKey(1)
cv2.destroyAllWindows()
cv2.waitKey(1)
```

**如果你喜欢阅读我的文章，并且它对你的职业/学习有所帮助，请考虑成为 Medium 的会员。每月费用为 5 美元，它为你提供对 Medium 上所有文章（包括我的）的无限访问。如果你使用以下链接注册，我将获得少量佣金（对你没有额外费用）。你的支持意味着我将能花更多时间写出像这样的文章。**

[](https://weimenglee.medium.com/membership?source=post_page-----316ded4d116f--------------------------------) [## 通过我的推荐链接加入 Medium - 魏梦李

### 阅读魏梦李（以及 Medium 上其他成千上万的作家）的每一个故事。你的会员费直接支持…

weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----316ded4d116f--------------------------------)

# 总结

我希望这篇简短的文章为你提供了一种使用 Python 和你的网络摄像头检测面部的简单方法。务必下载 haar cascades XML 文件，并将其放置在与你的 Python 文件相同的文件夹中。在未来的文章中，我将向你展示一些面部识别的技术。敬请期待！
