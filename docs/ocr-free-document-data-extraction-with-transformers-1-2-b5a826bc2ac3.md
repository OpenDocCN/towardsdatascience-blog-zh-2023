# 无需 OCR 的文档数据提取与变换器 (1/2)

> 原文：[https://towardsdatascience.com/ocr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3?source=collection_archive---------3-----------------------#2023-04-28](https://towardsdatascience.com/ocr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3?source=collection_archive---------3-----------------------#2023-04-28)

## Donut 与 Pix2Struct *在自定义数据上的对比*

[](https://toon-beerten.medium.com/?source=post_page-----b5a826bc2ac3--------------------------------)[![图标：Toon Beerten](../Images/f169eaa8cefa00f17176955596972d57.png)](https://toon-beerten.medium.com/?source=post_page-----b5a826bc2ac3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5a826bc2ac3--------------------------------)[![图标：Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b5a826bc2ac3--------------------------------) [Toon Beerten](https://toon-beerten.medium.com/?source=post_page-----b5a826bc2ac3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3aef462e13b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3&user=Toon+Beerten&userId=3aef462e13b5&source=post_page-3aef462e13b5----b5a826bc2ac3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5a826bc2ac3--------------------------------) ·10分钟阅读·2023年4月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5a826bc2ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3&user=Toon+Beerten&userId=3aef462e13b5&source=-----b5a826bc2ac3---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5a826bc2ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3&source=-----b5a826bc2ac3---------------------bookmark_footer-----------)![](../Images/45dc7196c8f321f51a04bce1054c5709.png)

作者提供的图像 ([与](https://huggingface.co/spaces/albarji/mixture-of-diffusers))

> [Donut](https://arxiv.org/abs/2111.15664) 和 [Pix2Struct](https://arxiv.org/abs/2210.03347) 是图像到文本模型，将纯像素输入的简单性与视觉语言理解任务相结合。简单来说：输入一张图像，提取的索引以 JSON 格式输出。

最近我[发布了](https://huggingface.co/spaces/to-be/invoice_document_headers_extraction_with_donut)一个在发票上微调的Donut模型。我经常收到如何使用自定义数据集进行训练的问题。此外，还发布了一个类似的模型：Pix2Struct，它声称性能显著更好。但真的是这样吗？

该是卷起袖子的时候了。我将展示给你：

+   如何为Donut和Pix2Struct微调准备数据

+   两种模型的训练过程

+   实际数据集上的比较结果

当然，我也会提供colab笔记本，以便于你的实验和/或复制。

**数据集**

要进行此比较，我需要一个公开的数据集。我想避免使用通常用于文档理解任务的数据集，例如[CORD](https://github.com/clovaai/cord)，浏览了一下，发现了[Ghega数据集](https://machinelearning.inginf.units.it/data-and-tools/ghega-dataset)。它相当小（约250个文档），由2种类型的文档组成：专利申请和数据表。通过不同类型，我们可以模拟一个分类问题。每种类型我们都有多个索引需要提取。这些索引对于每种类型都是唯一的。正是我所需要的。来自的Trieste大学机器学习实验室的[Medvet](https://medvet.inginf.units.it/)教授慷慨批准了这些文章的使用。

数据集似乎比较旧，所以需要调查它是否仍然适合我们的目标。

**初步探索**

当你获得一组新的数据时，你首先需要熟悉其结构。幸运的是，网站的详细描述对我们很有帮助。这是数据集的文件结构：

```py
ghega-dataset
    datasheets
        central-zener-1
        central-zener-2
        diodes-zener
            document-000-123542.blocks.csv
            document-000-123542.groundtruth.csv
            document-000-123542.in.000.png
            document-000-123542.out.000.png
            document-001-123663.blocks.csv
            document-001-123663.groundtruth.csv
            document-001-123663.in.000.png
            document-001-123663.out.000.png
            ...
        mcc-zener
        ...
    patents
        ...
```

我们可以看到两个主要的子文件夹对应两个文档类型：*数据表*和*专利*。在更下一级，我们有一些子文件夹，这些子文件夹本身不重要，但它们包含以某个前缀开头的文件。我们可以看到一个唯一的标识符，例如*document-000–123542*。对于每个这些标识符，我们有4种数据：

+   *blocks.csv* 文件包含有关边界框的信息。由于Donut或Pix2Struct不使用这些信息，我们可以忽略这些文件。

+   *out.000.png* 文件是后处理（去倾斜）的图像文件。由于我更愿意测试未处理的文件，我也会忽略这些。

+   原始的、未处理的文档图像有一个 *in.000.png* 后缀。这是我们感兴趣的。

+   最后是相应的*groundtruth.csv*文件。这包含我们认为是实际标注的图像索引。

这里是一个示例groundtruth csv文件以及列描述：

```py
Case,-1,0.0,0.0,0.0,0.0,,0,1.28,2.78,0.79,0.10,MELF CASE
StorageTemperature,0,0.35,3.40,2.03,0.11,Operating and Storage Temperature,0,4.13,3.41,0.63,0.09,-65 to +200
```

```py
 1\. element type
 2\. page of the label block (-1 if absent)
 3\. x of the label block
 4\. y of the label block
 5\. w of the label block
 6\. h of the label block
 7\. text of the label block
 8\. page of the value block (never absent!)
 9\. x of the value block
10\. y of the value block
11\. w of the value block
12\. h of the value block
13\. text of the label block
```

这意味着我们只对第一列和最后一列感兴趣。第一列是*键*，最后一列是*值*。在这种情况下：

```py
KEY                   VALUE
Case                  MELF CASE
StorageTemperature    -65 to +200
```

这意味着对于该文档，我们将微调模型以查找‘*Case*’的值为‘*MELF CASE*’，并且提取一个‘*StorageTemperature*’，其值为‘*-65 to +200*’。

**索引**

在groundtruth元数据中存在以下索引：

+   **数据表**：型号、类型、外壳、功耗、储存温度、电压、重量、热阻

+   **专利**：标题、申请人、发明人、代表、申请日期、出版日期、申请编号、出版编号、优先权、分类、摘要第一行

观察到地面真实值的质量和可行性，我选择保留以下索引：

```py
elements_to_extract = ['FilingDate', 'RepresentiveFL', 'Classification', 'PublicationDate','ApplicationNumber','Model','Voltage','StorageTemperature']
```

**质量**

对于图像转换为文本，使用了 [ocropus](http://code.google.com/p/ocropus/) 版本 0.2。这意味着它大约在 2014 年底发布。在数据科学领域这已经很古老了，那么地面真实度的质量是否符合我们的任务要求呢？

为此，我查看了一些随机图像，并将地面真实值与实际在文档上写的内容进行了比较。以下是两个 OCR 不正确的示例：

![](../Images/798c47d8cb3079a86d6cc8af74e49659.png)

来自 Ghega 数据集的 document-001–109381.in.000.png

键 *Classification* 被设置为 *BGSD 81/00* 作为地面真实值。它应该是 *B65D 81/100*。

![](../Images/da972563105ade20c85a3167784b56a5.png)

来自 Ghega 数据集的 document-003–112107.in.000.png

键 *StorageTemperature* 显示 *I -65 {O + 150* 作为地面真实值，而我们可以看到它应该是 *-65 to + 150*。

数据集中有许多此类错误。一种方法是纠正这些错误。另一种是忽略这些错误。由于我将使用相同的数据来比较两个模型，我选择了后者。如果数据用于生产，你可能需要选择前一种方法以获得最佳结果。

（还要注意，这些特殊字符可能会搞乱 JSON 格式，稍后我会回到这个话题）

**Donut 数据集结构**

我们需要的数据格式是什么样的？

对于微调 Donut 模型，我们需要将数据组织在一个文件夹中，所有文档作为单独的图像文件和一个元数据文件，结构为 JSON lines 文件。

```py
donut-dataset
    document-000-123542.in.000.png
    document-001-123663.in.000.png
    ...
    metadata.jsonl
```

JSONL 文件包含每个图像文件一行，格式如下：

```py
{"file_name": "document-010-100333.in.000.png", "ground_truth": "{\"gt_parse\": { \"DocType\": \"patent\", \"FilingDate\": \"06.12.1999\", \"RepresentiveFL\": \"Manresa Val, Manuel\", \"Classification\": \"A47l. 5/28\", \"PublicationDate\": \"1139845\", \"ApplicationNumber\": \"99959528 .3\" } }"}
```

让我们分解这行 JSON。在上层我们有一个包含两个元素的字典：*file_name* 和 *ground_truth*。在 *ground_truth* 键下，我们有一个包含 *gt_parse* 键的字典。其值本身是一个字典，包含我们在文档中知道的键值对。或者更好：*assign*。记住，文档中不一定会出现文档类型。术语 *datasheet* 并没有作为文本出现在这些文档中。

幸运的是，pix2struct 使用相同的格式进行微调，因此我们可以一举两得。一旦我们将其转换为这种结构，我们还可以用来微调 Pix2Struct。

**转换**

对于转换本身，我在 colab 上创建了一个 Jupyter notebook。我决定在这个阶段将数据拆分为训练集和验证集，而不是在微调之前。这种方式，两个模型将使用相同的验证图像，结果会更具可比性。五个文档中会有一个用于验证。

利用上述 Ghega 数据集的结构知识，我们可以将转换过程概括如下：

> 对于每个以 in.000.png 结尾的文件名，我们取对应的 groundtruth 文件并创建一个临时的数据框对象。
> 
> 注意，groundtruth 可能为空或完全不存在。（例如，对于 *datasheets/taiwan-switching*）
> 
> 接下来，我们从子文件夹中扣除类：*patent* 或 *datasheet* 。现在我们需要构建 JSON 行。对于每个我们想提取的元素/索引，我们检查它是否在数据框中并进行收集。然后复制图像本身。
> 
> 对所有图像执行此操作，最后我们就有一个 JSONL 文件可以写出。

在 Python 中，它看起来是这样的：

```py
json_lines_train = ''
json_lines_val = ''

for dirpath, dirnames, filenames in os.walk('/content/ghega-dataset/'):
    for filename in filenames:
        if filename.endswith('in.000.png'):
          gt_filename = filename.replace('in.000.png','groundtruth.csv')
          gt_filename_path = os.path.join(dirpath, gt_filename)
          if not os.path.exists(gt_filename_path):    #ignore files in /ghega-dataset/datasheets/taiwan-switching/ because no groundtruth exists
            continue
          if os.path.getsize(gt_filename_path) == 0:  #ignore empty groundtruth files
            print(f'skipped {gt_filename_path} because no info in metadata')
            continue
          doc_df = pd.read_csv(gt_filename_path, header=None)
          #find the doctype, based on path
          if 'patent' in dirpath:
            type = 'patent'
          else:
            type = 'datasheet'
          #create json line
          #eg:
          #{"file_name": "document-034-127420.in.000.png", "ground_truth": "{\"gt_parse\": { \"DocType\": \"datasheet\", \"Model\": \"ZMM5221 B - ZMM5267B\", \"Voltage\": \"1.5\", \"StorageTemperature\": \"-65 to 175\" } }"}
          p2 = ''
          #add always first element: DocType
          p2 += '\\"' + 'DocType' + '\\": '
          p2 += '\\"' + type + '\\"'
          new_row = {'ImagePath': os.path.join(dirpath, filename), 'DocType' :type}
          ghega_df = pd.concat([ghega_df, pd.DataFrame([new_row])], ignore_index=True)
          #fill other elements if available
          for element in elements_to_extract:
            value = doc_df[doc_df[0] == element][12].tolist()
            if len(value) > 0:
              p2 += ', '
              p2 += '\\"' + element + '\\": '
              value = re.sub(r'[^A-Za-z0-9 ,.()/-]+', '', value[0])   #get rid of \ of ” and " in json
              p2 += '\\"' + value + '\\"'
              new_row = {'ImagePath': os.path.join(dirpath, filename), element :value}
              ghega_df = pd.concat([ghega_df, pd.DataFrame([new_row])], ignore_index=True)

          p3 = ' } }"}'

          json_line = p1 + p2 + p3
          print(json_line)

          #take ~20% to validation
          #copy image file and append json line
          if random.randint(1, 100) < 20:
            output_path = '/content/dataset/validation/'
            json_lines_val += json_line + '\r\n'
            shutil.copy(os.path.join(dirpath, filename), '/content/dataset/validation/')  
          else:
            output_path = '/content/dataset/train/'
            json_lines_train += json_line + '\r\n'
            shutil.copy(os.path.join(dirpath, filename), '/content/dataset/train/')  

#write jsonl files
text_file = open('/content/dataset/train/metadata.jsonl', "w")
text_file.write(json_lines_train)
text_file.close()
text_file = open('/content/dataset/validation/metadata.jsonl', "w")
text_file.write(json_lines_val)
text_file.close()
```

ghega_df 是一个数据框，用于进行一些合理性检查或统计分析（如有需要）。我用它来检查随机样本，验证我的转换数据是否正确。

**问题**

转换完成后，一切看起来都很顺利。但我想摆脱那种通常第一次尝试就能成功的想法。总是会有一些小的意外问题发生。谈论我遇到的错误并展示解决方案对任何模拟整个过程并使用自己数据集的人都是有用的。

例如，在转换数据集后，我想训练 Donut 模型。在此之前，我需要创建一个训练数据集，如下所示：

```py
train_dataset = DonutDataset("/content/dataset", max_length=max_length,
                             split="train", task_start_token="<s_cord-v2>", prompt_end_token="<s_cord-v2>",
                             sort_json_key=False, # dataset is preprocessed, so no need for this
                             )
```

并且出现了这个错误：

```py
---------------------------------------------------------------------------
ArrowInvalid                              Traceback (most recent call last)
<ipython-input-13-7726ec2b0341> in <cell line: 7>()
      5 processor.feature_extractor.do_align_long_axis = False
      6 
----> 7 train_dataset = DonutDataset("/content/dataset", max_length=max_length,
      8                              split="train", task_start_token="<s_cord-v2>", prompt_end_token="<s_cord-v2>",
      9                              sort_json_key=False, # cord dataset is preprocessed, so no need for this

ArrowInvalid: JSON parse error: Missing a comma or '}' after an object member. in row 7
```

看起来第 7 行的 JSON 格式有问题。我复制了那一行并将其粘贴到一个 [在线 JSON 验证器](https://jsonformatter.curiousconcept.com/#) 中：

![](../Images/fcc1e25cb0180b26712b1496194d27d4.png)

作者提供的图像

![](../Images/732f1846570edeab4c0b07c4006f595b.png)

作者提供的图像

![](../Images/732f1846570edeab4c0b07c4006f595b.png)

作者提供的图像

然而，它表示这是一个有效的 JSON 行。让我们更深入地看看：

```py
{
   "file_name":"document-012-108498.in.000.png",
   "ground_truth":"{\"gt_parse\": {\"DocType\": \"patent\"\"FilingDate\": \"15\. Januar 2004 (15.01.2004)\",\"Classification\": \"BOZC 18/08,\",\"PublicationDate\": \"5\. August 2004 (05.08.2004)\",\"ApplicationNumber\": \"PCT/AT2004/000006\"} }"
}
```

你发现错误了吗？经过一段时间，我注意到 *DocType* 和 *FilingDate* 之间缺少逗号。然而，这在所有行中都是缺失的，所以我不清楚为什么第 7 行会出现问题。当我修复了这个问题后，我再次尝试，现在它声称第 17 行有问题：

```py
ArrowInvalid: JSON parse error: Missing a comma or '}' after an object member. in row 17
```

这是第 17 行，你发现了问题吗？

```py
{"file_name": "document-007-103668.in.000.png", "ground_truth": "{\"gt_parse\": {\"DocType\": \"patent\",\"FilingDate\": \"18.12.2008\",\"RepresentiveFL\": \"Schubert, Siegmar\",\"Classification\": \"A47J 31/42 (2""6·"')\",\"PublicationDate\": \"12.08.2009\",\"ApplicationNumber\": \"08021980.1\"} }"}
```

这是*Classification* 元素的未转义引号。为了解决这个问题，我决定所有值只能包含字母数字字符和一些特殊字符，并使用了这个正则表达式：

```py
[^A-Za-z0-9 ,.()/-]+
```

这可能会严重影响真实性能，但从我所见，其他字符都是由于 OCR 错误引起的。我认为，对于模型之间的相对比较，忽略这些字符影响不大。

**数据准备：完成**

数据准备的重要性常被忽视且被低估。通过上述步骤，我展示了如何调整自己的数据，以便Donut和Pix2Struct用于文档的关键索引提取。常见的陷阱也得到了修正。包含所有步骤的Jupyter笔记本可以在[这里](https://github.com/Toon-nooT/notebooks/blob/main/Donut_vs_pix2struct_1_Ghega_data_prep.ipynb)找到。我们已经完成了一半。下一步是用这个数据集训练这两个模型。我非常好奇它们的表现如何，但比较和训练将留到下一篇文章中。

你可能还喜欢：

[](https://toon-beerten.medium.com/hands-on-document-data-extraction-with-transformer-7130df3b6132?source=post_page-----b5a826bc2ac3--------------------------------) [## 实战：使用🍩变换器进行文档数据提取

### 我使用Donut变换器模型提取发票索引的经验。

toon-beerten.medium.com](https://toon-beerten.medium.com/hands-on-document-data-extraction-with-transformer-7130df3b6132?source=post_page-----b5a826bc2ac3--------------------------------)

参考文献：

[](https://arxiv.org/abs/2111.15664?source=post_page-----b5a826bc2ac3--------------------------------) [## 无OCR文档理解变换器

### 理解文档图像（例如，发票）是一项核心但具有挑战性的任务，因为它需要复杂的功能…

arxiv.org](https://arxiv.org/abs/2111.15664?source=post_page-----b5a826bc2ac3--------------------------------) [](https://arxiv.org/abs/2210.03347?source=post_page-----b5a826bc2ac3--------------------------------) [## Pix2Struct：作为视觉语言理解预训练的截图解析

### 视觉位置语言无处不在——来源包括带有图表的教科书到包含图像的网页…

arxiv.org](https://arxiv.org/abs/2210.03347?source=post_page-----b5a826bc2ac3--------------------------------) [](https://machinelearning.inginf.units.it/data-and-tools/ghega-dataset?source=post_page-----b5a826bc2ac3--------------------------------) [## 机器学习实验室 - Ghega数据集

### Ghega数据集：用于文档理解和分类的数据集，我们提供了一个标注数据集，可以…

machinelearning.inginf.units.it](https://machinelearning.inginf.units.it/data-and-tools/ghega-dataset?source=post_page-----b5a826bc2ac3--------------------------------) [](https://huggingface.co/to-be/donut-base-finetuned-invoices?source=post_page-----b5a826bc2ac3--------------------------------) [## to-be/donut-base-finetuned-invoices · Hugging Face

### 编辑模型卡 基于Donut基础模型（在论文《无OCR文档理解变换器》中介绍）…

huggingface.co](https://huggingface.co/to-be/donut-base-finetuned-invoices?source=post_page-----b5a826bc2ac3--------------------------------)
