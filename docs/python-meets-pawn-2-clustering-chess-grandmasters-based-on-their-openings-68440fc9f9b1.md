# Python Meets Pawn 2：基于开局的国际象棋大师聚类

> 原文：[`towardsdatascience.com/python-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1`](https://towardsdatascience.com/python-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1)

## 在这篇博客中，我将引导你通过使用 Python 分析国际象棋大师开局的过程。

[](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)![Mikayil Ahadli](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------) [Mikayil Ahadli](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------) ·7 分钟阅读·2023 年 12 月 22 日

--

![](img/ad0563bab95de27dccaf51c4c6936779.png)

由 Midjourney 创建的照片

+   我在回答哪些问题

+   第一部分：获取数据

+   第二部分：特征工程

+   第三部分：聚类

+   结果与有趣的事实

## 我在回答哪些问题

我对国际象棋的热情不是什么秘密，[这里](https://medium.com/@mikayil.ahadli/python-meets-pawn-decoding-my-chess-openings-with-data-analysis-097a34cef20a)我分享了自己棋局开局的分析。但今天，我将踏入一个新领域：国际象棋大师的世界。他们通常使用什么开局？他们的选择有多么多样？我对这些开局在不同国际象棋大师中的分布很感兴趣。顶级棋手是否偏爱相似的开局？是否可以根据他们的偏好进行分组？我不知道——让我们来探讨一下！

## 第一部分：获取数据

国际象棋的一个伟大方面是其数据的可获取性。有许多来源，包括[pgnmentor](https://pgnmentor.com/files.html)，你可以在这里查看和下载关于开局和棋手的数据（免费）。这些数据每年更新多次，包括 Portable Game Notation (PGN)格式的棋局，这是国际象棋游戏最流行的格式。由于下载是逐个进行的，我选择了 11 位著名的国际象棋大师来下载和分析他们的开局。请注意，这个列表是主观的，包含了一些我最喜欢的国际象棋大师：

1.  Shakhriyar Mamedyarov

1.  Teimour Radjabov

1.  Hikaru Nakamura

1.  Magnus Carlsen

1.  Fabiano Caruana

1.  丁立人

1.  Ian Nepomniachtchi

1.  Viswanathan Anand

1.  Anish Giri

1.  Vugar Gashimov

1.  Vladimir Kramnik

完整的代码将在博客末尾提供。为了解析 PGN 文件，我使用了名为‘Chess’的 Python 库中的 PGN 模块。

我用于解析数据的函数如下所示：

```py
def parse_pgn_file(file_path):
    """
    Parses a PGN (Portable Game Notation) file containing chess games.

    Args:
        file_path (str): Path to the PGN file.

    Returns:
        pd.DataFrame: A DataFrame containing game information.
    """
    games = []  # Initialize an empty list to store parsed games.
    with open(file_path, "r") as pgn_file:
        while True:
            game = chess.pgn.read_game(pgn_file)  # Read a game from the PGN file.
            if game is None:
                break  # Exit the loop when no more games are found.
            games.append(game)  # Append the parsed game to the list.

    data = []  # Initialize an empty list to store game data.
    for game in games:
        data.append({
            "Event": game.headers.get("Event", ""),
            "Date": game.headers.get("Date", ""),
            "Result": game.headers.get("Result", ""),
            "White": game.headers.get("White", ""),
            "Black": game.headers.get("Black", ""),
            "Moves": " ".join(str(move) for move in game.mainline_moves()),
            "ECO": game.headers.get("ECO", "")
        })  # Extract relevant information from game headers and moves.

    df = pd.DataFrame(data)  # Create a DataFrame from the extracted data.
    return df  # Return the DataFrame containing game information.
```

以下是我解析和组合数据的表格显示。我将利用现有的“ECO”列，指示每盘棋中使用的开局。棋类中的 ECO 代码指的是“国际象棋开局百科全书”，这是一种用于分类各种开局的系统。每个代码由一个字母和两个数字组成，如 B12 或 E97，独特地标识某一特定开局或变体。

![](img/d2a51a4164eb873a6b6134180e637f3b.png)

解析的数据集（图片来源：作者）

特级大师们拥有数千盘棋局，涵盖 484 个独特的组合 ECO 代码。鉴于有 500 个独特的 ECO 代码，这 11 位特级大师几乎使用了职业生涯中的所有范围。然而，每位特级大师玩了多少个独特的开局？让我们查看以下图表：

![](img/c97ca8b6fe181c64b5fb0dcfe39a427e.png)

独特开局图表（图片来源：作者）

这些数字与他们在数据集中的棋局数量高度相关，但总体而言，图表显示特级大师们在棋局中使用了各种各样的开局。

## 第二部分：特征工程

让我们开始查看每位特级大师最受欢迎的开局：

+   B90 — 西西里防御，Najdorf 变体 : *Anand, Giri, Nepomniachtchi*

+   D37 — 皇后弃兵 : *Carlsen, Mamedyarov, Radjabov*

+   C42 — 俄国棋局 : *Gashimov, Kramnik*

+   A05 — 印度王攻 : *Nakamura*

+   C65 — 西班牙棋局，柏林防御 : *Caruana*

+   E60 — 格鲁恩费尔德和印度棋局 : *Ding*

我猜看到一位俄国特级大师偏好俄国棋局并不奇怪。Gashimov 也偏好俄国棋局，表明苏联棋校在阿塞拜疆的强大影响。基于他们喜欢的开局发现一些模式是很有趣的。然而，为了实现更详细和分隔的分组，我将应用聚类技术，同时考虑其他开局。

让我们检查每位特级大师的开局分布。我将数据集以特级大师为索引，使用独特的 ECO 代码作为列，以棋局数量为值进行了透视。以下图表是马格纳斯·卡尔森的示例：

![](img/509dbeb6a8dd70c80c51d6326ccc9c4e.png)

马格纳斯的开局分布（图片来源：作者）

尽管特级大师们使用了各种开局，但明显有些开局比其他开局更具优势。大多数特级大师似乎偏好大约五种特定的开局，这影响了我决定集中于一个包含前 5 名开局的数据框。

对于聚类，我选择测试两个数据框：透视比例和前 5 个开局。使用后者取得了最佳结果，我将在下面详细解释。有关更多选项和详细见解，请参阅末尾提供的完整代码。在前 5 个开局数据框中，我使用了独热编码。在 11 位国际象棋大师中，前 5 个选择中有 24 个独特的 ECO 代码。这个数据框中的二进制值指示每位国际象棋大师的前 5 个开局中是否包含特定的 ECO 代码：

![](img/dcc18752f2e0a111fb2dc776d7052bc4.png)

Top5 数据框（图片由作者提供）

下表显示了每位国际象棋大师的前 5 个 ECO。我们已经可以看到一些模式，但聚类将帮助我们更有效地区分它们。

![](img/ce3bcaa6588ee4151918da6de1de38e7.png)

每位国际象棋大师的前 5 个开局结果（图片由作者提供）

## 第三部分：聚类

前 5 个最受欢迎的开局数据集包含 24 列。为了简化，我应用了 PCA（主成分分析）。这种方法有助于减少数据维度，同时保留重要信息。虽然第一个主成分提供了不错的结果，但我选择了两个成分。为什么？它们提供了几乎相同的洞察，并且使得可视化更容易。

对于分组国际象棋大师，我使用了 K-means 聚类。这就像把书籍分类到不同的类型中。首先，我选择了聚类的数量或“类型”。然后，将每位国际象棋大师的开局风格匹配到最接近的聚类中，就像将书籍分配到最合适的类型一样。这个过程会不断调整：代表每组共同风格的聚类中心会重新计算，国际象棋大师会相应地重新分配。这个过程会重复，直到聚类准确地表示出不同的游戏风格。通过 K-means，国际象棋开局中的不同模式浮现出来，突显了国际象棋大师们之间的不同策略。

选择正确的聚类数在任何聚类项目中都是关键。为此，我使用了肘部法则。这是一种确定数据分组理想聚类数的简单方法。你绘制一个图表，其中每个点代表一个不同的聚类数，并计算每个聚类的“组内平方和”（WCSS）。WCSS 衡量数据点到聚类中心的距离。在图表上，有一个点，在该点之后增加聚类数不会显著减少 WCSS。这个点类似于一个肘部，指示最佳的聚类数。它确保了聚类数和数据点之间的紧密分组之间的平衡。下面的图表演示了在我们的案例中，最佳聚类数是 4。

![](img/f0801d0d3c884f547ad6f04fbe410a8d.png)

确定最佳聚类数的肘部法则（图片由作者提供）

确定了聚类数量后，我对特级大师进行了聚类。为了评估我的聚类效果，我使用了轮廓系数。这个分数衡量了一个对象与其自身聚类的相似性与其他聚类的相似性。高轮廓系数表明数据聚类效果良好。该分数范围在-1 到 1 之间，我获得了 0.69 的分数，表明聚类效果有效。

最后，我在二维空间中可视化了聚类数据和质心（每个聚类的“中心”）。这一步将复杂的数据转化为易于理解和视觉上吸引人的格式，非常适合一目了然地看到模式和差异：

![](img/4202a1cabcada5f97453547a99a7e7d2.png)

分析结果（图片由作者提供）

## 结果和有趣的事实

我的分析揭示了国际象棋特级大师在开局方面展现了广泛的 repertoire，但他们之间有些偏好有所不同。基于这些开局对他们进行聚类不仅是可行的，而且得出了有趣的见解。例如，阿塞拜疆象棋传奇人物马梅杰罗夫和拉杰博夫被归为一组。有趣的是，安اند、吉里和卡鲁阿纳也紧密聚集在一起。仔细观察他们的前 5 个最爱开局，确认了这些结果。值得注意的是，安 Anand 和吉里分享了完全相同的前 5 个开局。这是否意味着吉里对安 Anand 的钦佩？确实，在互联网研究后，我发现吉里非常欣赏安 Anand 并从他的棋局中学习。以下是这些开局：

+   B90 — 西西里防御，奈杰多夫变体

+   C50 — 意大利开局

+   C42 — 俄国开局

+   C65 — 西班牙开局，柏林防御

+   C67 — 西班牙开局，柏林防御，其他变体

完整代码及 Jupyter notebook 文件可以在[这里](https://github.com/MikayilAhad/medium_articles/blob/main/clustering_chess_GMs_basedon_openings/grandmasters_analysis.ipynb)找到。
