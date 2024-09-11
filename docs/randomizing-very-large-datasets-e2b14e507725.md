# 随机化非常大的数据集

> 原文：[https://towardsdatascience.com/randomizing-very-large-datasets-e2b14e507725?source=collection_archive---------7-----------------------#2023-08-26](https://towardsdatascience.com/randomizing-very-large-datasets-e2b14e507725?source=collection_archive---------7-----------------------#2023-08-26)

## 考虑这样一个问题：如何随机化一个大到连内存都容纳不下的数据集。本文描述了如何在 Python 中轻松且（相对）快速地完成这一操作。

[](https://medium.com/@doug.blank?source=post_page-----e2b14e507725--------------------------------)[![Douglas Blank, PhD](../Images/b2fa86b9fe63a8bcb4f218ef5a6791e9.png)](https://medium.com/@doug.blank?source=post_page-----e2b14e507725--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e2b14e507725--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e2b14e507725--------------------------------) [Douglas Blank, PhD](https://medium.com/@doug.blank?source=post_page-----e2b14e507725--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F66e2bac7e7d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandomizing-very-large-datasets-e2b14e507725&user=Douglas+Blank%2C+PhD&userId=66e2bac7e7d8&source=post_page-66e2bac7e7d8----e2b14e507725---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e2b14e507725--------------------------------) ·6 min read·2023年8月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe2b14e507725&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandomizing-very-large-datasets-e2b14e507725&user=Douglas+Blank%2C+PhD&userId=66e2bac7e7d8&source=-----e2b14e507725---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe2b14e507725&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandomizing-very-large-datasets-e2b14e507725&source=-----e2b14e507725---------------------bookmark_footer-----------)

如今，发现以 Gigabytes 甚至 Terabytes 计量的数据集并不罕见。如此大量的数据可以极大地帮助训练过程，创建出强大的机器学习模型。但如何随机化如此庞大的数据集呢？

![](../Images/85fddbf8a9cde6f37e48e2320879b217.png)

图片由 [Jess Bailey](https://unsplash.com/@jessbaileydesigns?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想象一下你有一个非常大的数据集，每行一个项目在一个文件中。数据的具体细节对于我们的目标无关紧要。数据集可以是 CSV（逗号分隔值）或 TSV（制表符分隔值）文件中的行，或者每行是一个 JSON 对象，或者是大点云中某一点的 X、Y、Z 值。我们只需要的是数据集按每行一个项目进行格式化。

对于包含较小数据集的文件，可以使用像这样的简单 Python 函数在内存中对文件进行随机化（称为“洗牌”）：

```py
import random

def shuffle_in_memory(filename_in, filename_out):
    # Shuffle a file, line-by-line
    with open(filename_in) as fp:
        lines = fp.readlines()
    # Randomize them in place:
    random.shuffle(lines)
    # Write the new order out:
    with open(filename_out, "w") as fp:
        fp.writelines(lines)
```

**shuffle_in_memory()**函数接收一个输入文件名和一个输出文件名，在内存中使用内置的**random.shuffle()**函数洗牌，并将随机化的数据写出。顾名思义，该函数要求文件的所有行一次性加载到内存中。

要测试这个函数，让我们制作一些测试文件。函数**make_file()**接收你希望在测试文件中包含的行数。该函数将创建文件并返回文件名。

```py
import os

def make_file(lines):
    filename = "test-%s.txt" % lines
    print("Making test file '%s'..." % filename)

    with open(filename, "w") as fp:
        for i in range(lines):
            fp.write(f"Line {i}\n")

    print("Done!")
    return filename
```

例如，要创建一个名为“test-1000.txt”的文件，其中包含100行，如下所示：

```py
filename_in = make_file(1000)
```

运行此函数后，你应该在当前目录中找到一个名为“test-1000.txt”的文件，包含1,000行文本，如下所示：

```py
Line 0
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
...
```

要测试我们的**shuffle_in_memory()**函数，我们将命名一个输出文件，将字符串保存在变量**filename_out**中，并调用该函数：

```py
filename_out = "test-randomized-1000.txt"
shuffle_in_memory(filename_in, filename_out)
```

现在，你的目录中应该有一个第二个文件，名为“test-randomized-1000.txt”。它的大小应与“test-1000.txt”完全相同，行数也完全相同，但顺序是随机的：

```py
Line 110
Line 592
Line 887
Line 366
Line 52
Line 22
Line 891
Line 83
Line 931
Line 408
...
```

好的，现在大问题来了：如果我们有一个非常大的文件怎么办？让我们创建一个中等大小的文件，比如说1000万行。（对于大多数计算机来说，这仍然足够小，可以在内存中随机化，但大小足够大，可以进行练习。）如前所述，我们通过调用**make_file()**来创建输入文件：

```py
filename_in_big = make_file(10_000_000)
```

这将花费几秒钟。之后，你应该在目录中有一个名为“test-10000000.txt”的文件。它应该与之前的文件一样开始，但将包含1000万行。文件大小约为128 MB。

如何进行随机化？如果我们不想使用所有的 RAM，或者 RAM 不够，我们可以改用硬盘。这里有一个基于类似问题的递归算法，排序。以下函数**shuffle()**是基于归并排序算法。

首先，它检查一个文件是否足够小，可以在内存中进行洗牌（递归函数术语中的基本情况）。参数**memory_limit**以字节为单位。如果文件大小小于**memory_limit**，那么它将在内存中进行洗牌。如果太大，则会随机分割成多个较小的文件，每个文件递归地进行洗牌。最后，将较小的洗牌文件的内容合并回一起。

这是函数：

```py
import tempfile

def shuffle(filename_in, filename_out, memory_limit, file_split_count, 
            depth=0, debug=False):
    if os.path.getsize(filename_in) < memory_limit:
        if debug: print(" " * depth, f"Level {depth + 1}",
            "Shuffle in memory...")
        shuffle_in_memory(filename_in, filename_out)
    else:
        if debug: print(
            " " * depth, f"Level {depth + 1}",
            f"{os.path.getsize(filename_in)} is too big;",
            f"Split into {file_split_count} files..."
        )
        # Split the big file into smaller files
        temp_files = [tempfile.NamedTemporaryFile('w+', delete=False)
                      for i in range(file_split_count)]
        for line in open(filename_in):
            random_index = random.randint(0, len(temp_files) - 1)
            temp_files[random_index].write(line)

        # Now we shuffle each smaller file
        for temp_file in temp_files:
            temp_file.close()
            shuffle(temp_file.name, temp_file.name, memory_limit, 
                    file_split_count, depth+1, debug)

        # And merge back in place of the original
        if debug: print(" " * depth, f"Level {depth + 1}", 
            "Merge files...")
        merge_files(temp_files, filename_out)
```

如果这是一个排序算法，我们将以一种小心的方式将文件合并在一起，以创建一个排序顺序。然而，对于洗牌，我们不关心以特定顺序合并它们，因为我们希望它们是随机的。因此，**merge_files()**函数看起来像这样：

```py
def merge_files(temp_files, filename_out):
    with open(filename_out, "w") as fp_out:
        for temp_file in temp_files:
            with open(temp_file.name) as fp:
                line = fp.readline()
                while line:
                    fp_out.write(line)
                    line = fp.readline()
```

请注意，我们会小心地避免一次性将所有文件的行读入内存。我们通过将内存洗牌的限制设置为文件的大小来测试这一点。由于文件大小不小于128,888,890，它将被分割成若干个较小的文件。对于这个例子，我们将大文件分成两个，每个文件都足够小，可以在内存中进行洗牌：

```py
filename_out_big = "test-randomized-10000000.txt"
shuffle(filename_in_big, filename_out_big, 128_888_890, 2, debug=True)
```

这个调用的结果如下：

```py
 Level 1 128888890 is too big; Split into 2 files...
  Level 2 Shuffle in memory...
  Level 2 Shuffle in memory...
 Level 1 Merge files...
```

结果文件“test-randomized-10000000.txt”的内容应包含1000万行，所有行都是随机的。更好的测试方法是将所需内存缩小到远小于文件大小，并将过大的文件分割成超过2个。假设我们只想使用约1 MB的RAM，并将文件分割成20个较小的文件：

```py
shuffle(filename_in_big, filename_out_big, 1_000_000, 20, debug=True)
```

这个例子将使用不超过1 MB的RAM，并递归地处理大于此大小的子文件，每次处理20个。

这个算法可以处理任何大小的文件（当然，你需要足够的磁盘空间！）。你为**shuffle_in_memory()**分配的内存越多，运行速度就会越快。如果较小的文件数量过多，你将花费太多时间打开和关闭文件。你可以尝试不同的**memory_limit**值，但我发现20到200之间的值效果很好。初始文件越大，你可能需要更多的子文件。

你还可以使用其他算法。我曾对将所有行写入SQLite数据库、以随机顺序SELECT它们抱有很高的期望，但它的速度并没有比上面的代码更快。

```py
import sqlite3

def shuffle_sql(filename_in, filename_out, memory_limit, depth=0, debug=False):
    if os.path.getsize(filename_in) < memory_limit:
        if debug: print(" " * depth, f"Level {depth + 1}",
            "Shuffle in memory...")
        shuffle_in_memory(filename_in, filename_out)
    else:
        if debug: print(
            " " * depth, f"Level {depth + 1}",
            f"{os.path.getsize(filename_in)} is too big;",
            f"Writing to SQLite database..."
        )
        temp_db = tempfile.NamedTemporaryFile(delete=False)
        connection = sqlite3.connect(temp_db.name)
        cursor = connection.cursor()
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS lines (
                line TEXT
            );
        """)
        with open(filename_in) as fp:
            line = fp.readline()
            while line:
                cursor.execute("INSERT INTO lines (line) VALUES (?);", [line])
                line = fp.readline()
            connection.commit()
        with open(filename_out, "w") as fp:
          for line in cursor.execute("""
              SELECT line FROM lines ORDER BY random();
              """):
              fp.write(line[0])
```

```py
shuffle_sql(filename_in_big, filename_out_big, 1_000_000, debug=True)
```

你能在纯Python中击败递归洗牌算法吗？如果能，我很想听听你的方法！

***对人工智能、机器学习和数据科学感兴趣吗？请点赞并关注。告诉我你感兴趣的内容！***
