# 在一个小时内构建你的第一个深度学习应用

> 原文：[https://towardsdatascience.com/build-your-first-deep-learning-app-within-an-hour-4e80c120e99f?source=collection_archive---------6-----------------------#2023-07-21](https://towardsdatascience.com/build-your-first-deep-learning-app-within-an-hour-4e80c120e99f?source=collection_archive---------6-----------------------#2023-07-21)

## 使用 HuggingFace Spaces 和 Gradio 部署图像分类模型

[](https://miptgirl.medium.com/?source=post_page-----4e80c120e99f--------------------------------)[![Mariya Mansurova](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----4e80c120e99f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e80c120e99f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e80c120e99f--------------------------------) [Mariya Mansurova](https://miptgirl.medium.com/?source=post_page-----4e80c120e99f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-your-first-deep-learning-app-within-an-hour-4e80c120e99f&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----4e80c120e99f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e80c120e99f--------------------------------) ·11分钟阅读·2023年7月21日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e80c120e99f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-your-first-deep-learning-app-within-an-hour-4e80c120e99f&source=-----4e80c120e99f---------------------bookmark_footer-----------)![](../Images/853574e57387bbbe2b3eab5395f1a971.png)

[Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我从事数据分析工作已经将近十年了。时不时地，我会使用机器学习技术从数据中获取见解，而且我对使用经典的机器学习方法感到很自如。

尽管我通过了一些关于神经网络和深度学习的MOOC课程，但我从未在工作中使用过这些技术，这个领域对我来说似乎非常具有挑战性。我有所有这些偏见：

+   你需要学习很多东西才能开始使用深度学习：数学、不同的框架（我至少听说过三种：`PyTorch`、`TensorFlow`和`Keras`）以及网络架构。

+   训练模型需要大量的数据集。

+   没有强大的计算机（*它们还必须有Nvidia GPU*），就不可能获得令人满意的结果，因此获取这样的设备相当困难。

+   要使一个机器学习驱动的服务运行起来，需要处理大量的样板工作：你需要处理前端和后端的部分。

我相信分析的主要目标是帮助产品团队根据数据做出正确的决策。如今，神经网络可以显著提升我们的分析能力，例如，自然语言处理可以从文本中获得更多的见解。因此，我决定再尝试一下利用深度学习的力量对我来说会有帮助。

这就是我开始[Fast.AI课程](https://course.fast.ai/)的方式（*它在2022年初进行了更新，所以我猜内容自之前的TDS评论以来可能有所变化*）。我意识到，使用深度学习解决任务并不是那么困难。

这个课程采用自上而下的方法。所以你从构建一个工作系统开始，之后才会深入了解所有必要的基础知识和细节。

我在第二周制作了我的第一个机器学习驱动应用程序（*你可以在* [*这里*](https://huggingface.co/spaces/miptgirl/cuttest_dogs) *尝试一下*）。这是一个图像分类模型，可以识别我最喜欢的狗品种。令人惊讶的是，即使我的数据集中只有几千张图片，它的表现也很好。这让我感到振奋，我们现在可以如此轻松地构建一个十年前还完全是魔法的服务。

![](../Images/107bf8a2134762ddc8bf8b330dab6452.png)

照片由[Shakti Rajpurohit](https://unsplash.com/ko/@shaktirajpurohit?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所以在这篇文章中，你将找到一个关于构建和部署第一个机器学习驱动服务的初学者级教程。

# 什么是深度学习？

深度学习是机器学习的一个特定应用场景，其中我们使用多层神经网络作为模型。

神经网络非常强大。根据[通用逼近定理](https://en.wikipedia.org/wiki/Universal_approximation_theorem)，神经网络可以逼近任何函数，这意味着它们能够解决任何任务。

目前，你可以将这个模型视为一个黑箱，它接受输入（*在我们的例子中——一张狗的图片*）并返回输出（*在我们的例子中——一个标签*）。

![](../Images/880015ebf56f0625e2895e729379b3c3.png)

作者拍摄的照片

# 构建模型

> 你可以在[Kaggle](https://www.kaggle.com/code/miptgirl/fastai-week-2-dogs-breeds-classification-mo/edit/run/136880445)上找到这个阶段的完整代码。

我们将使用 [Kaggle Notebooks](https://www.kaggle.com/docs/notebooks) 来构建我们的深度学习模型。如果您还没有Kaggle账户，值得注册。Kaggle是一个流行的数据科学平台，您可以在这里找到数据集、参与竞赛以及运行和分享代码。

您可以在Kaggle创建一个Notebook，并像在本地Jupyter Notebook一样执行代码。Kaggle甚至提供GPU，所以我们可以很快训练神经网络模型。

![](../Images/e7242d7e606433d6e106281b1ac150d6.png)

图片来源：作者

让我们先导入所有包，因为我们将使用许多Fast.AI工具。

```py
from fastcore.all import *
from fastai.vision.all import *
from fastai.vision.widgets import *
from fastdownload import download_url
```

## 加载数据

不用说，我们需要一个数据集来训练我们的模型。获取一组图像的最简单方法是使用搜索引擎。

[DuckDuckGo](https://duckduckgo.com/)搜索引擎有一个易于使用的API和方便的Python包`duckduckgo_search`（[*更多信息*](https://pypi.org/project/duckduckgo-search/)），所以我们将使用它。

让我们尝试搜索一张狗的图片。我们已指定`license_image = any`，只使用具有创作共用许可的图片。

```py
from duckduckgo_search import DDGS
import itertools
with DDGS() as ddgs:
    res = list(itertools.islice(ddgs.images('photo samoyed happy', 
                                license_image = 'any'), 1))
```

在输出中，我们获得了关于图片的所有信息：名称、URL和大小。

```py
{
   "title": "Happy Samoyed dog photo and wallpaper. Beautiful Happy Samoyed dog picture", 
   "image": "http://www.dogwallpapers.net/wallpapers/happy-samoyed-dog-wallpaper.jpg", 
   "thumbnail": "https://tse2.mm.bing.net/th?id=OIP.BqTE8dYqO-W9qcCXdGcF6QHaFL&pid=Api", 
   "url": "http://www.dogwallpapers.net/samoyed-dog/happy-samoyed-dog-wallpaper.html", 
   "height": 834, "width": 1193, "source": "Bing"
}
```

现在我们可以使用Fast.AI工具来下载图片并显示缩略图。

![](../Images/5a5ec7ba9995d3de121a3479f3c8a373.png)

图片由 [Barcs Tamás](https://unsplash.com/@barcstamaas?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们看到一只快乐的萨摩耶，这意味着它在正常工作。所以让我们加载更多照片。

我旨在识别五种不同的犬种（我最喜欢的那些）。我将为每种犬种加载图片，并将它们存储在不同的目录中。

```py
breeds = ['siberian husky', 'corgi', 'pomeranian', 'retriever', 'samoyed']
path = Path('dogs_breeds') # defining path

for b in tqdm.tqdm(breeds):
    dest = (path/b)
    dest.mkdir(exist_ok=True, parents=True) 

    download_images(dest, urls=search_images(f'photo {b}'))
    sleep(10) 
    download_images(dest, urls=search_images(f'photo {b} puppy'))
    sleep(10) 
    download_images(dest, urls=search_images(f'photo {b} sleep'))
    sleep(10) 
    resize_images(path/b, max_size=400, dest=path/b)
```

运行此代码后，您将看到Kaggle右侧面板上的所有加载的照片。

![](../Images/18b15558c734df1045934f6d688c728f.png)

图片来源：作者

下一步是将数据转换为适用于Fast.AI模型的格式——`DataBlock`。

对于这个对象，您需要指定几个参数，但我只会强调最重要的几个：

+   `splitter=RandomSplitter(valid_pct=0.2, seed=18)`：Fast.AI要求选择一个验证集。验证集是用来估计模型质量的保留数据。为了防止过拟合，训练时不会使用验证数据。在我们的例子中，验证集是数据集的20%的随机部分。我们指定了`seed`参数，以便下次能够精确地重复相同的划分。

+   `item_tfms=[Resize(256, method=’squish’)]`：神经网络以批量处理图像。这就是为什么我们必须将图片调整为相同的大小。目前我们使用了`squish`方法，但我们会在后面更详细地讨论它。

我们已经定义了一个数据块。函数`show_batch`可以向我们展示一组随机的带标签的图像。

![](../Images/80089b7b745e7af415ea8bbebbf52b48.png)

照片由 [Angel Luciano](https://unsplash.com/@roaming_angel?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) | 照片由 [Brigitta Botrágyi](https://unsplash.com/@bbrigike?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) | 照片由 [Charlotte Freeman](https://unsplash.com/fr/@happyfeijoa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据看起来没问题，所以我们继续训练。

## 训练模型

你可能会感到惊讶，但下面这两行代码将完成所有工作。

![](../Images/9be3eae20456aaa76396a015f1993f5b.png)

我们使用了一个预训练模型（18 层深度的卷积神经网络 — `Resnet18`）。这就是我们称之为 `fine_tune` 的原因。

我们训练了模型三轮，这意味着模型看到了整个数据集 3 次。

我们还指定了度量标准 — `accuracy` (*正确标记图片的比例*)。你可以在每一轮后的结果中看到这个度量标准（它仅使用验证集计算，以免影响结果）。然而，它没有用于优化过程，仅供参考。

整个过程花了大约 30 分钟，现在我们的模型可以以 94.45% 的准确率预测狗的品种。干得好！但我们能否改善这个结果？

## 改进模型：数据清理和增强

> 如果你希望尽快看到你的第一个模型运行，可以先跳过这部分，继续进行模型部署。

首先，让我们看看模型的错误：它是否无法区分柯基和哈士奇或博美和拉布拉多。我们可以使用 `confusion_matrix` 来实现。请注意，混淆矩阵也是仅使用验证集计算的。

![](../Images/73499dd9ee674e7da264c85725aaad4e.png)

Fast.AI 课程中分享的另一个小窍门是可以使用模型来清理我们的数据。为此，我们可以查看损失最大的图像：这些可能是模型自信度高但错误的情况，或是正确但信心低的情况。

![](../Images/1938f6835432a722044da06b35441f10.png)

照片由 [Benjamin Vang](https://unsplash.com/ko/@bivphoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) | 照片由 [Xennie Moore](https://unsplash.com/@shadowseas?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) | 照片由 [Alvan Nee](https://unsplash.com/@alvannee?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

显然，第一张图片的标签不正确，而第二张图片包含了哈士奇和柯基。因此还有改进的空间。

幸运的是，Fast.AI 提供了一个方便的 `ImageClassifierCleaner` 小部件，它可以帮助我们快速解决数据问题。你可以在你的笔记本中初始化它，然后你将能够更改数据集中的标签。

```py
cleaner = ImageClassifierCleaner(learn)
cleaner
```

在每个类别之后，你可以运行以下代码来解决问题：删除图片或将其移动到正确的文件夹。

```py
for idx in cleaner.delete(): cleaner.fns[idx].unlink()
for idx,breed in cleaner.change(): shutil.move(str(cleaner.fns[idx]), path/breed)
```

现在我们可以再次训练我们的模型，并看到准确率提高了：95.4% 对比 94.5%。

![](../Images/3ff1da7784808a454859c7849aa42bb5.png)

正确识别的柯基犬比例已从 88% 提高到 96%。太棒了！

![](../Images/d2ec6baebf1f5947d151aa76cbce0ede.png)

改善我们模型的另一种方法是改变我们的缩放方法。我们使用了压缩方法，但正如你所看到的，它可能会改变自然物体的比例。让我们尝试更具创意地使用增强。

增强是对图片的更改（例如，对比度改进、旋转或裁剪）。这将为我们的模型提供更多变的数据显示，并希望提高其质量。

与 Fast.AI 一样，你只需更改几个参数即可添加增强。

![](../Images/643a79780ab0d8ed7f173a63fefa03c4.png)

[FLOUFFY](https://unsplash.com/@theflouffy?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

此外，由于在每个周期中模型会看到略有不同的图片，我们可以增加周期数。经过六个周期，我们达到了 95.65% 的准确率——结果稍微好一点。整个过程大约花了一个小时。

## 下载模型

最后一步是下载我们的模型。这非常简单。

```py
learn.export('cuttest_dogs_model.pkl')
```

然后你将有一个标准的 `pickle` 文件（*常见的 Python 对象存储格式*）。只需在 Kaggle Notebook 右侧面板中选择文件旁的 `更多操作`，你将可以将模型下载到你的计算机上。

![](../Images/c984ea0fa54731a25fe28f2ad7c9a6f2.png)

现在我们有了训练好的模型，让我们部署它，这样你就可以将结果分享给全世界。

# 部署你的模型

我们将使用 [HuggingFace](https://huggingface.co) Spaces 和 [Gradio](https://www.gradio.app) 来构建我们的网页应用。

## 设置 HuggingFace Space

HuggingFace 是一家提供实用机器学习工具的公司，例如流行的变换器库或分享模型和数据集的工具。今天我们将使用他们的 Spaces 来托管我们的应用。

首先，如果你还没有注册，你需要创建一个账户。这只需几分钟。请点击这个 [链接](https://huggingface.co/join)。

现在是创建新空间的时候了。前往 Spaces 选项卡并点击“创建”按钮。你可以在 [文档](https://huggingface.co/docs/hub/spaces-overview)中找到更多详细说明。

然后你需要指定以下参数：

+   **name**（它将用于你的应用 URL，所以请谨慎选择），

+   **license**（我选择了开源 Apache 2.0 许可证）

+   **SDK**（在这个示例中我将使用 Gradio）。

![](../Images/d1a51c71256deaace6d3996204d69ca0.png)

然后用户友好的 HuggingFace 将向你展示说明。**TL;DR** 现在你有了一个 Git 仓库，你需要将你的代码提交到那里。

关于 Git 有一个细节。由于你的模型可能相当大，最好设置 Git LFS（大型文件存储），这样 Git 就不会跟踪这个文件的所有更改。有关安装，请参考[网站](https://git-lfs.com/)上的说明。

```py
-- cloning repo
git clone https://huggingface.co/spaces/<your_login>/<your_app_name>
cd <your_app_name>

-- setting up git-lfs
git lfs install
git lfs track "*.pkl"
git add .gitattributes
git commit -m "update gitattributes to use lfs for pkl files"
```

# Gradio

Gradio 是一个框架，它允许你只使用 Python 构建愉快且友好的 Web 应用。这就是它成为原型设计的宝贵工具的原因（尤其是对于像我这样没有深厚 JavaScript 知识的人）。

在 Gradio 中，我们将定义我们的接口，指定以下参数：

+   **输入** — 一张图像，

+   **输出** — 带有五个可能类别的标签，

+   **标题**、**描述** 和 **一组示例** 图像（*我们也需要将它们提交到仓库中*），

+   `enable_queue=True` 可以帮助应用程序处理大量流量，特别是当它变得非常受欢迎时，

+   **函数** 用于处理输入图像。

要为输入图像获取标签，我们需要定义一个预测函数，该函数加载我们的模型并返回一个包含每个类别概率的字典。

最后，我们将得到 `app.py` 的以下代码

```py
import gradio as gr
from fastai.vision.all import *

learn = load_learner('cuttest_dogs_model.pkl')

labels = learn.dls.vocab # list of model classes
def predict(img):
    img = PILImage.create(img)
    pred,pred_idx,probs = learn.predict(img)
    return {labels[i]: float(probs[i]) for i in range(len(labels))}

gr.Interface(
    fn=predict,
    inputs=gr.inputs.Image(shape=(512, 512)),
    outputs=gr.outputs.Label(num_top_classes=5),
    title="The Cuttest Dogs Classifier 🐶🐕🦮🐕‍🦺",
    description="Classifier trainded on images of huskies, retrievers, pomeranians, corgis and samoyeds. Created as a demo for Deep Learning app using HuggingFace Spaces & Gradio.",
    examples=['husky.jpg', 'retriever.jpg', 'corgi.jpg', 'pomeranian.jpg', 'samoyed.jpg'],
    enable_queue=True).launch()
```

如果你想了解更多关于 Gradio 的信息，请阅读[文档](https://www.gradio.app/guides/quickstart)。

让我们还创建一个`requirements.txt`文件，其中包含`fastai`，这样这个库就会在我们的服务器上安装。

所以剩下的唯一步骤就是将所有内容推送到 HuggingFace Git 仓库。

```py
git add * 
git commit -am 'First version of Cuttest Dogs app'
git push
```

> 你可以在[GitHub](https://github.com/miptgirl/miptgirl_medium/tree/main/cuttest_dogs)上找到完整的代码。

在推送文件后，返回到 HuggingFace Space，你将看到类似的图片显示构建过程。如果一切正常，你的应用将在几分钟内运行。

![](../Images/869d2e210d267ff37854208abacef644.png)

如果出现任何问题，你将看到一个堆栈跟踪。然后你需要返回到你的代码中，修复错误，推送新版本，并再等几分钟。

# 它正在运行

现在我们可以使用这个模型处理真实照片，例如验证我家狗是否确实是柯基犬。

![](../Images/3ba48ead8cc93a7aae036bc596f42069.png)

作者提供的照片

今天我们已经完成了构建深度学习应用程序的整个过程：从获取数据集和拟合模型到编写和部署 Web 应用。希望你已经完成了这个教程，现在你正在生产环境中测试你精彩的模型。

> 非常感谢你阅读这篇文章。希望它对你有启发。如果你有任何后续问题或评论，请在评论区留言。此外，也不要犹豫，分享你应用程序的链接。
