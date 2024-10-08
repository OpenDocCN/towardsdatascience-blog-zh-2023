# 使用 Python 进行高级 GUI 界面设计

> 原文：[`towardsdatascience.com/advanced-gui-interface-with-python-cb04ef2e29b9`](https://towardsdatascience.com/advanced-gui-interface-with-python-cb04ef2e29b9)

## 开始使用更现代的开发界面来实现你的 Python 项目

[](https://bharath-k1297.medium.com/?source=post_page-----cb04ef2e29b9--------------------------------)![Bharath K](https://bharath-k1297.medium.com/?source=post_page-----cb04ef2e29b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb04ef2e29b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb04ef2e29b9--------------------------------) [Bharath K](https://bharath-k1297.medium.com/?source=post_page-----cb04ef2e29b9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb04ef2e29b9--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 9 日。

--

![](img/c4bee57c2e59995debe5d5b15775f6fc.png)

[Slava Keyzman](https://unsplash.com/@slavasfotos?utm_source=medium&utm_medium=referral) 的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

我们在计算机屏幕上使用的几乎所有软件都是某种形式的 GUI 界面。大多数 GUI 应用程序提供了一个与软件产品友好互动的平台。记事本和大多数 Windows 系统中的普通计算器就是一些简单的 GUI 界面的例子，方便所有用户使用。

大多数编程语言允许我们开发用户界面，从而实现人与计算机的互动，包括 Python。借助 Python 及其各种用于 GUI 开发的库，如 Tkinter、Kivy、PyQt5 以及其他类似工具，程序员可以轻松地开发 GUI 界面。

所有这些开发工具都提供了一个直接的起点，用于构建几乎任何类型的界面以执行多种任务。对于感兴趣的读者，我建议查看我以前的一篇文章，其中介绍了开发 UI 图形的七种最佳工具以及每个提到的库的入门代码。

[](/7-best-ui-graphics-tools-for-python-developers-with-starter-codes-2e46c248b47c?source=post_page-----cb04ef2e29b9--------------------------------) ## Python 开发者的 7 款最佳 UI 图形工具及其入门代码

### Python 中用于开发酷炫用户界面技术的七款最佳 UI 图形工具。

towardsdatascience.com

然而，这些界面在设计结构上看起来有些平淡。为了获得更具美感和现代化的设计布局，最好使用更具导向性和高级的 GUI 界面，以使开发的软件整体外观更具吸引力。

在本文中，我们将专注于开发一个稍微现代化的开发环境，该环境具有更好的用户界面，并在整体设计上有所改进，以访问和利用其功能。对于对类似项目感兴趣的读者，我建议查看我以前关于使用 Python 编程开发日历以跟踪重要日期的文章。

[](/develop-your-own-calendar-to-track-important-dates-with-python-c1af9e98ffc3?source=post_page-----cb04ef2e29b9--------------------------------) ## 使用 Python 开发自己的日历以跟踪重要日期

### 开发一个日历 GUI 界面来调整 2022 年及以后的计划

towardsdatascience.com

# 现代 GUI 界面以加载图像：

![](img/3cbfe29387d1d52a8fcb4bcd70dedf6f.png)

照片由[布伦丹·比尔](https://unsplash.com/@theophilus318?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个项目中，我们将构建一个现代化的 GUI 开发界面，带有一个按钮，可以从工作目录中随机加载图像。我们将使用自定义 tkinter 库作为主要的 UI 开发界面，创建包括按钮、标签、图像和项目所需的其他对象的工作流。

以下包可以通过一个简单的 pip 命令安装，如下所示。自定义 tkinter 库可以在 Python 包索引网站上找到，你可以从[这里](https://pypi.org/project/customtkinter/)查看。

> pip install customtkinter

一旦你安装了上述库，我们可以继续进行下一步，即导入开发界面所需的库。值得注意的是，如果你还没有安装 tkinter 库，建议也安装它，因为这两个库在特定任务中可以互换使用。

## 导入必要的库：

在第一步中，我们将导入所有必要的库，这些库将用于构建我们的高级 GUI 界面。我们将使用最近通过 pip 命令安装的自定义 tkinter 库。我们还可以同时使用 tkinter 库和自定义 tkinter 模块，因为一些函数是相互衍生的。

最后，我们将导入 Pillow 库，这是 Python 中处理不同类型图像任务的最佳库之一。我们将导入 Pillow 库的 ImageTk 和 Image 类，这将使我们能够在 GUI 界面中处理任务。以下是所有所需库模块导入的代码块。

```py
# Importing the custom tkinter library for constructing the advanced GUI interface
import customtkinter
import tkinter
from PIL import ImageTk, Image
```

## 设置主题和创建背景界面：

一旦完成所有必要库的导入，下一步是设置开发界面的外观和主题模式。在外观方面，自定义 tkinter 库提供了深色模式和浅色模式的选项，而默认颜色主题的选项则包括蓝色、深蓝色和绿色。

一旦我们设置了外观和默认颜色主题，就可以定义自定义 tkinter 函数作为根对象以完成我们的功能。我们还将设置界面的几何形状，以便进行所需操作。请注意，尺寸可能因特定目的而异，使用的图像大小可能需要调整以完成所需任务。

```py
# Setting the appearence and theme of the GUI window
customtkinter.set_appearance_mode("dark")
customtkinter.set_default_color_theme("blue") 

# Setting the dimensions and defining the custom tkinter function
app = customtkinter.CTk() 
app.geometry("1280x720")
```

## 创建按钮功能：

在下一步中，我们将编码一个函数，使我们能够在图形窗口（整个屏幕或相应框架）内显示图像。ImageTk 类中的照片图像函数允许我们显示所需的图像。我们还使用了一个自定义的 Tkinter 库，其中显示了图像。下方提供了相应的代码片段。

```py
# Creating the function for displaying our image
def button_function():
    img = ImageTk.PhotoImage(Image.open("Trees.jfif"))
    label = customtkinter.CTkLabel(master = frame, image = img, text="")
    label.pack()
    print("button pressed")
```

## 创建框架和按钮接口：

在本节的最后一步，我们将创建一个用于显示图像的框架。我们将把框架放置在工作图形窗口中。创建了显示图像的框架并用按钮点击后，我们将创建实际的按钮来操作之前定义的功能。我们将为按钮提供中心对齐并打包该功能。最后，我们将循环运行根函数以完成过程。下方提供了代码片段。

```py
# Creating the frame
frame = customtkinter.CTkFrame(master = app)
frame.pack(pady = 20, padx = 60, fill = "both", expand = True)

# # Use CTkButton for displaying the image
button = customtkinter.CTkButton(master=frame, text="Display Image", command=button_function)
button.place(relx=0.5, rely=0.8, anchor=tkinter.CENTER)
button.pack()

app.mainloop()
```

完成项目编码后，你可以运行 Python 应用程序。我们会看到一个图形窗口，窗口内有一个“显示图像”按钮。点击以下按钮，图像将在框架内显示。下面提供了项目的工作截图。

![](img/36c814bc9e536b604e8502fc62f5d3b3.png)

作者截图

这个项目的复杂性几乎不可察觉。只需正确的库，就可以用几行代码开发出来。然而，下一部分将介绍这个项目的一个稍微高级的版本，我们将实现一个额外的功能层，使项目更加富有创意。

# 加载多个图像的高级改进：

![](img/3453fb37ce0eaaa178a4eaa9f6a1da98.png)

照片由 [NASA](https://unsplash.com/fr/@nasa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本节中，我们将介绍上述项目的一个稍微高级的变体，我们将在每次显示和清除图像时显示一张随机图像。我们将利用随机库，这将使我们能够在每次用“删除图像”按钮清除图像时以及点击“显示图像”按钮后立即解释随机选择的图像。我在这篇文章中使用了三张图像来生成图像。我将这三张图像保存为“Trees.jfif”、“Maps.jfif”和“Books.jfif”在工作项目目录中。

我们定义的另一个附加功能是清除框架功能，该功能将删除框架中组装的所有先前小部件。因此，我们将“删除图像”和“显示图像”按钮放置在图形窗口的框架之外。其余过程在图形窗口中显示按钮和运行软件的方式相同。为了方便开发者访问，下面提供了整个项目的代码块。

```py
# Importing the custom tkinter library for constructing the advanced GUI interface
import customtkinter
import tkinter
from PIL import ImageTk, Image
import random

# Setting the appearence and theme of the GUI window
customtkinter.set_appearance_mode("dark")
customtkinter.set_default_color_theme("blue") 

# Setting the dimensions and defining the custom tkinter function
app = customtkinter.CTk() 
app.geometry("1200x800")

# Creating the function for displaying our image
def button_function():
    image_list = ["Trees.jfif", "Maps.jfif", "Books.jfif"]
    selected_Image = random.choice(image_list)
    print(selected_Image)

    img = ImageTk.PhotoImage(Image.open(selected_Image))
    label = customtkinter.CTkLabel(master = frame, image = img, text="")
    label.pack()
    print("button pressed")

def clear_frame():
   for widgets in frame.winfo_children():
      widgets.destroy()

# Creating the frame
frame = customtkinter.CTkFrame(master = app)
frame.pack(pady = 20, padx = 60, fill = "both", expand = True)

# # Use CTkButton for displaying the image
button = customtkinter.CTkButton(master=app, text="Display Image", command=button_function)
button.place(relx=0.5, rely=0.8, anchor=tkinter.CENTER)
button.pack()

# # Use CTkButton instead of tkinter Button
button = customtkinter.CTkButton(master=app, text="Delete Image", command=clear_frame)
button.place(relx=0.5, rely=0.6, anchor=tkinter.CENTER)
button.pack()

app.mainloop()
```

对于感兴趣的读者和开发者爱好者来说，以上项目还有进一步改进的空间。我们可以将显示图像和删除图像的按钮合并为一个实体，以避免每次需要显示图像时按两个按钮。通过改进代码和添加更多必要元素，还可以为项目添加更多独特功能。

# 结论：

![](img/a5fbbaa4f0cc0a444287aaea97531117.png)

照片由 [freestocks](https://unsplash.com/ko/@freestocks?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “下一个重大突破是让最后一个重大突破变得可用的那个。”
> 
> — ***布莱克·罗斯***

图形和图形用户界面在大多数软件开发项目中发挥着关键作用。采用现代化的方式来适应新兴趋势是至关重要的，这使我们能够创建和开发创新且富有创意的工作界面。高质量的图形界面能保证更好的人与计算机的互动。

在本文中，我们学习了如何使用自定义 tkinter 开发更现代和高级的 GUI 界面。我们构建了项目，使其在点击按钮时首先在图形窗口的框架内显示图像。在接下来的部分中，我们介绍了一种稍微复杂的方法，通过点击按钮生成随机图像。

我强烈推荐尝试更多高级修改，并构建多个项目，以便更熟悉自定义 tkinter 库中不同的功能和类。如果你希望在我的文章发布时立即获得通知，请查看以下[链接](https://bharath-k1297.medium.com/subscribe)进行订阅。如果你想支持其他作者和我，请订阅以下链接。

[初学者指南：使用 Streamlit 部署数据科学项目](https://towardsdatascience.com/beginners-guide-to-streamlit-for-deploying-your-data-science-projects-9c9fce488831?source=post_page-----cb04ef2e29b9--------------------------------)

### 阅读 Bharath K（以及 Medium 上的其他成千上万位作者）的每一个故事。你的会员费将直接支持…

[加入我的 Medium 会员](https://bharath-k1297.medium.com/membership?source=post_page-----cb04ef2e29b9--------------------------------)

如果你对本文中提到的各种点有任何疑问，请随时在评论中告诉我。我会尽快回复你。

查看我关于这一话题的其他一些文章，你也许会喜欢阅读！

[使用 Python 进行图像滤镜](https://towardsdatascience.com/image-filters-with-python-3dc223a12624?source=post_page-----cb04ef2e29b9--------------------------------)

### 使用 Python 构建图像滤镜的简明计算机视觉项目

[开发自己的拼写检查工具包与 Python](https://towardsdatascience.com/image-filters-with-python-3dc223a12624?source=post_page-----cb04ef2e29b9--------------------------------) 

### 创建一个有效验证拼写的 Python 应用

[开发自己的拼写检查工具包与 Python](https://towardsdatascience.com/develop-your-own-spelling-check-toolkit-with-python-740bf84a865d?source=post_page-----cb04ef2e29b9--------------------------------)

### 轻松使用 streamlit 部署你的机器学习和数据科学项目作为 web 应用

[初学者指南：使用 Streamlit 部署数据科学项目](https://towardsdatascience.com/beginners-guide-to-streamlit-for-deploying-your-data-science-projects-9c9fce488831?source=post_page-----cb04ef2e29b9--------------------------------)

感谢大家坚持看到最后。希望你们喜欢阅读这篇文章。祝大家有个美好的一天！
