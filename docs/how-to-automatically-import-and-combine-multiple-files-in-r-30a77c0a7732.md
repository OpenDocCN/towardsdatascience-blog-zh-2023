# 如何在 R 中自动导入和合并多个文件

> 原文：[`towardsdatascience.com/how-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732`](https://towardsdatascience.com/how-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732)

## 不要再浪费时间手动导入多个文件

[](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)![Alana Rister, Ph.D.](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------) [Alana Rister, Ph.D.](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------) ·6 分钟阅读·2023 年 9 月 20 日

--

![](img/4faf273a525b691b00abfba4f6517fd3.png)

图片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我的数据科学家工作中，由于不同软件的导出限制，我经常需要导入几个包含相同类型信息的不同文件。如果你也有类似的情况，下面是一个清晰简单的方法，能够自动将文件导入为独立的数据框或将它们合并成一个数据框。

# 准备你的文件

在开始编写代码之前，我们首先需要准备好文件。我们需要有一种方法来程序化选择我们想导入到 R 中的文件。虽然你可以选择任何方法来区分这些文件，下面是两种最简单的方法：

1.  为所有你想一次导入的文件创建一个独特的前缀。

1.  在你的工作目录中创建一个单独的文件夹，只包含那些文件。

例如，如果我有一组名为“SA#.xlsx”的 Excel 文件。如果我没有其他以 SA 开头的类似文件，那么我已经有了我的前缀。如果我的文件夹中还有其他以 SA 开头的文件，比如“SAT.xlsx”，我可以很容易地创建一个文件夹，命名为“SA”。然后，我将仅将我想导入的 SA 文件放入该文件夹中。

# 创建你的文件列表

一旦我们有了程序化识别文件的方式，我们需要创建一个所有文件名的列表。我们可以使用 R 函数 list.files() 来实现这一点。

## 带前缀的文件列表

如果你选择为文件名添加前缀，我们将使用 list.files() 的模式参数来选择我们想要的特定文件。

```py
# Formula
filelist <- list.files(pattern = "^<prefix>")

#Example
filelist <- list.files(pattern = "^SA")
```

该模式接受一个正则表达式。因此，我们可以使用“^”符号表示字符串的开头。这确保了任何其他文件名中包含“SA”但不在开头的文件将不会被包含在这个名字集合中。**注意：这只会从你的工作目录中提取文件。你可以更改路径以从不同的目录中提取文件。**

## 文件夹中的文件列表

如果你选择将文件添加到一个文件夹中，我们将使用路径参数来告诉 R 从哪里提取我们的文件。

```py
#Formula
filelist <- list.files(path = "./<folder name>")

#Example
filelist <- list.files(path = "./SA")
```

“.”符号指向当前工作目录。然后，它会查找一个名为“SA”的文件夹，并包括来自该文件夹的所有文件名。

# 导入你的文件

现在我们有了一个列表，我们可以对这个列表运行一个循环，将所有文件导入到当前环境中。如果我们想将每个文件作为自己的变量包含，我们将首先需要创建文件名，然后导入文件，再将文件动态分配给变量名。虽然这个过程对于前缀文件和文件夹导入的文件是相似的，但在导入文件时有一个小的区别。

## 导入文件

让我们首先讨论将文件读取到 R 环境中的区别，因为这将是上述两种不同方法所需的唯一变化。

如果你使用了前缀方法，文件存在于你的工作目录中。因此，你无需指定文件的路径。然而，如果你将它们添加到一个文件夹中，它们就不再在你的工作目录中。因此，我们需要动态构建这些文件的文件路径。

为了创建这个公式，我将使用变量“file”来表示我们*filelist*变量中的文件名。这将允许我在下面的循环中直接使用这段代码。

```py
# For Prefix Files
df <- <read function>(file)

# Prefix example with an excel file
df <- read_excel(file)

#For Folder files
df <- <read function>(paste("./<folder name>/",file,sep=""))

#Folder file example with excel file
df <- read_excel(paste("./SA/",file,sep=""))
```

对于文件夹中的文件，每次导入文件时，我们必须将文件夹路径添加到文件名中。幸运的是，R 有一个 paste 命令，允许我们动态地将文件夹路径添加到每个文件名中。由于 paste 自动创建的分隔符是空格，我们必须将分隔符（sep）重写为空白，这通过添加引号来实现。

# 自动逐个导入文件

从现在开始，为了简单起见，我将假设文件存在于工作目录中。现在，我们需要创建一个循环，允许文件自动导入并设置为动态变量。

## 创建变量名

首先，我们需要动态创建我们的变量。我们将使用文件名来创建变量；然而，我们需要从文件名中移除文件扩展名。同样，我使用“file”作为文件名的占位符。如果你想从文件名中移除前缀，也可以使用相同的思路。

```py
#Formula
name <- gsub(".<file extension>", "", file)

#Example for Excel file
name <- gsub(".xlsx","", file)
```

## 分配变量

由于我们已经创建了导入代码，让我们创建代码来分配变量。虽然使用“=”或“<-”符号分配变量更容易，但我们不能用这些符号来处理动态变量名。相反，我们将使用 R 中的 *assign* 函数。

```py
assign(name, df)
```

## 创建 for 循环

我们终于有了所有的组件来创建一个 for 循环，以自动导入所有的文件。我们需要做的就是将上述代码添加到一个将循环遍历我们*filelist*变量的 for 循环中。

```py
# Formula
for (file in filelist){

name <- gsub(".<file extension>", "", file)
df <- <read function>(file)
assign(name, df)
}

#Example for Excel files
for (file in filelist) {

name <- gsub(".xlsx", "", file)
df <- read_excel(file)
assign(name, df)

}
```

现在，你可以轻松添加你的文件，但如果你想简单地将所有这些文件合并成一个文件呢？

# 自动导入和合并文件

许多时候，你可能有多个文件，它们是相同信息的简单片段。你真的想将它们合并并使用单一的数据框。因此，我们现在将创建一个 for 循环，仅输出一个将所有行合并到一起的数据框。为此，我们将使用 rbind() 函数将行绑定到单一的数据框中。

首先，我们需要创建一个空的数据框，以便将新文件添加到其中。然后，我们可以使用 for 循环将它们导入并绑定到这个数据框中。

```py
#Formula
<dataframe name> <- data.frame()

for (file in filelist){

df <- <read function>(file)
<dataframe name> <- rbind(<dataframe name>, df)

}

#Example with SA dataframe and excel files

SA <- data.frame()

for (file in filelist){

df <- read_excel(file)
SA <- rbind(SA, df)

}
```

运行示例代码将生成一个名为“SA”的数据框，其中包含*filelist*中所有文件的数据。

## 创建文件列

有时，数据来自的特定文件包含重要信息。例如，有人可能会给你一些数据，这些数据有特定的日期范围或重要的会议信息，这对数据分析很重要。如果你只是简单地合并每个文件中的数据，你将不知道这些数据来自哪个文件。

因此，我们想要创建一个动态列，在绑定数据之前添加每个文件的信息。我们将使用之前创建的相同名称变量来捕获文件名信息，并将其作为数据集中的一列。

```py
#Formula
<dataframe name> <- data.frame()

for (file in filelist){

name <- gsub(".<file extension>", "", file)
df <- <read function>(file)
df$<column name for file name> <- name
<dataframe name> <- rbind(<dataframe name>, df)

}

#Example with SA dataframe and excel files

SA <- data.frame()

for (file in filelist){

name <- gsub(".xlsx", "", file)
df <- read_excel(file)
df$filename <- name
SA <- rbind(SA, df)

}
```

现在，你可以使用上述的 for 循环轻松导入和合并多个文件，而不会丢失源文件的信息。

总体而言，自动导入文件的能力将节省大量编码时间，你可以让计算机利用计算时间来自动管理多个文件。
