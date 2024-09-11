# 国际象棋识别问题：深入解决方案

> 原文：[https://towardsdatascience.com/chess-recognition-problem-a-deep-dive-solution-e4d8a439dc37?source=collection_archive---------10-----------------------#2023-02-27](https://towardsdatascience.com/chess-recognition-problem-a-deep-dive-solution-e4d8a439dc37?source=collection_archive---------10-----------------------#2023-02-27)

[](https://acesaif.medium.com/?source=post_page-----e4d8a439dc37--------------------------------)[![Mohammed Saifuddin](../Images/b12b54250328aefe69f8043d580d2178.png)](https://acesaif.medium.com/?source=post_page-----e4d8a439dc37--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e4d8a439dc37--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e4d8a439dc37--------------------------------) [Mohammed Saifuddin](https://acesaif.medium.com/?source=post_page-----e4d8a439dc37--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd08aa760ba07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchess-recognition-problem-a-deep-dive-solution-e4d8a439dc37&user=Mohammed+Saifuddin&userId=d08aa760ba07&source=post_page-d08aa760ba07----e4d8a439dc37---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e4d8a439dc37--------------------------------) ·18 min read·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe4d8a439dc37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchess-recognition-problem-a-deep-dive-solution-e4d8a439dc37&user=Mohammed+Saifuddin&userId=d08aa760ba07&source=-----e4d8a439dc37---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe4d8a439dc37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchess-recognition-problem-a-deep-dive-solution-e4d8a439dc37&source=-----e4d8a439dc37---------------------bookmark_footer-----------)![](../Images/029918096614d634d2f1a44643a51863.png)

图片由 [Randy Fath](https://unsplash.com/de/@randyfath?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 1\. 引言

从物理棋盘图像中识别棋子配置的问题通常被称为 **国际象棋识别**。计算机识别棋盘上的棋子是开发能够下棋的智能系统的第一步，这样的系统可以解决国际象棋问题/难题，并进行国际象棋分析。

我的项目目标是识别棋子及其在棋盘上的位置，这可以用像**Forsyth–Edwards记谱法 (FEN)**这样的结构化格式描述，兼容各种国际象棋引擎。我还添加了一层额外的解释，输入生成的FEN后，输出是否存在潜在攻击（将军），并检测非法的棋盘位置。

# 2\. 数据集概述

数据集包含*100000*张随机生成的棋盘位置图像，棋子数为*5–15*个（*2*个国王和*3–13*个兵/棋子）。所有图像的尺寸为*400* x *400* 像素。

棋子的生成概率分布如下：

1.  *30%* 是兵。

1.  *20%* 是主教。

1.  *20%* 是骑士。

1.  *20%* 是车。

1.  *10%* 是皇后。

1.  *2* 个国王保证在棋盘上。

标签以FEN格式的文件名存在，但用连字符代替了斜杠。

数据集属于公共领域。请检查数据集来源的引用[1]。

## 2.1\. **Forsyth–Edwards记谱法 (FEN)**

Forsyth–Edwards记谱法 (FEN) 是描述国际象棋游戏中某一特定棋盘位置的标准记谱法。FEN的目的是提供所有必要的信息，以便从特定位置重新开始游戏。

FEN记录定义了一个特定的游戏位置，所有信息在一行文本中，并使用ASCII字符集[2]。

FEN表示*6*个字段：

+   棋子摆放数据

+   活跃颜色

+   王车易位

+   过路兵

+   半步钟

+   全步钟

> **注意**：由于数据集包含静态图像，我可以生成仅包含棋子摆放数据的FEN。

# 3\. 探索性数据分析

这是理解和调查数据集模式的关键阶段。

作者提供的代码 — 导入库。

在上述代码片段中，我导入了**chess_positions**模块。我为两个主要原因开发和完善了此模块 —

1.  EDA

1.  FEN的解释。

作者提供的代码 — 训练和测试数据集

## 3.1\. 数据集架构

训练数据集包含*80000*张图像，测试数据集包含*20000*张图像。

作者提供的代码 — 数据集的架构

## 3.2\. 检查重复项

以下代码片段显示了所有标签（包括图像）都是唯一的。

作者提供的代码 — 检查重复项

## 3.3\. 棋子分布

在国际象棋中，有6种不同的棋子（按颜色计算，共有*12*种不同的棋子）。

1.  **K** — 白色国王，**k** — 黑色国王。

1.  **Q** — 白色皇后，**q** — 黑色皇后。

1.  **B** — 白色主教，**b** — 黑色主教。

1.  **N** — 白色骑士，**n** — 黑色骑士。

1.  **R** — 白色车，**r** — 黑色车。

1.  **P** — 白色兵，**p** — 黑色兵。

**3.3.1\. 训练数据集中棋子分布**

![](../Images/16c21fcdddab44153892b9b364df2d29.png)

作者提供的图像 — 训练数据集中各类棋子总数

**3.3.2\. 测试数据集中棋子分布**

![](../Images/a064f8883a6e33ca8ad95184901b0e63.png)

作者提供的图像 — 测试数据集中各类棋子总数

上述棋子分布图的结论：

1.  无论是训练集还是测试集，棋盘上都没有总共*8*个兵（白色和黑色）。

1.  每个棋盘上只有一个合法的国王（白色和黑色）。

1.  后是训练集和测试集中使用最少的棋子。

## 3.4\. 密度图

密度图是对一组点的概率密度函数（PDF）的视觉表示。PDF 主要展示数据的分布情况。

**3.4.1\. 训练数据集的 PDF**

![](../Images/995d8eb00feb0355596aba5e92a964c1.png)

作者提供的图像 — 训练数据集中的棋子 PDF 图

**3.4.2\. 测试数据集的 PDF**

![](../Images/80cd1331bba82065f8b2561656187dfd.png)

作者提供的图像 — 测试数据集中的棋子 PDF 图

上述密度图的结论：

1.  所有棋盘上只有*1*个黑棋国王和*1*个白棋国王。

1.  所有棋子的分布几乎相同。存在更多棋盘上有*0*个兵、*0*个车、*0*个马、*0*个象或*0*个后。

## 3.5\. 总棋子数与棋盘数量

如前所述，训练数据集包含*80000*个棋盘，而测试数据集包含*20000*个棋盘。

![](../Images/2562947662a3a8f76f2dfb822ab9f827.png)

作者提供的图像 — 总棋子数与棋盘数量的直方图

上述棋盘上棋子分布图的结论：

1.  最大的棋子数量为*15*。

1.  最少的棋子数量为*5*。

1.  大多数棋盘上填满了*15*个棋子。

## 3.6\. 查找检查和棋子非法位置

我开发并完善了**chess_positions**模块，以检测图像中的检查和非法棋子位置。以下是模块中*3*个类的代码片段，这些类有助于检测检查和非法棋盘图像。

**3.6.1\. 定义棋盘**

```py
class Board(object):
    """
    This class is defines the chessboard.
    """

    def __init__(self, fen_label):
        self.fen_label = re.sub(pattern=r'\d',
                                repl=lambda x: self.get_ones(char=x.group()),
                                string=fen_label)
        self.fen_matrix = self.get_fen_matrix()

    def get_ones(self, char):
        """
        This method returns repetitive 1s based on input digit character.
        """
        if char.isdigit():
            return '1' * int(char)

    def get_fen_matrix(self):
        """
        This method constructs a FEN matrix.
        """
        fen_matrix = np.array([list(row) for row in self.fen_label.split('/')])
        return fen_matrix

    def get_piece_positions(self, notation):
        """
        This method returns the 2D index of the piece from FEN matrix.
        """
        (i, j) = np.where(self.fen_matrix == notation)
        try:
            if i is not None and j is not None:
                return i, j
        except:
            return None
```

**3.6.2\. 棋盘上的检查**

```py
class Check(Board):
    """
    This class finds if there are any checks in the chessboard.
    """

    def __init__(self, fen_label):
        super().__init__(fen_label=fen_label)

    def get_sub_matrix(self, ai, aj, di, dj):
        """
        This method chops the chessboard to a sub-matrix.
        """
        corners = np.array([(ai, aj), (di, aj), (ai, dj), (di, dj)])
        min_i, max_i = min(corners[:, 0]), max(corners[:, 0])
        min_j, max_j = min(corners[:, 1]), max(corners[:, 1])
        sub_matrix = self.fen_matrix[min_i:max_i+1, min_j:max_j+1]
        return sub_matrix, sub_matrix.shape

    def get_straight_checks(self, ai, aj, di, dj, a, d):
        """
        This method returns the checks along the straight path.
        """
        checks = list()
        for (i, j) in zip(ai, aj):
            if di == i:
                attack_path = self.fen_matrix[di]
            elif dj == j:
                attack_path = self.fen_matrix[:, dj]
            else:
                continue
            a_ind = np.where(attack_path == a)[0]
            d_ind = np.where(attack_path == d)[0][0]
            for a_i_ in a_ind:
                attack_path_ = attack_path[min(a_i_, d_ind): max(a_i_, d_ind)+1]
                checks.append(np.where(attack_path_ != '1')[0])
        checks = list(filter(lambda x: len(x) == 2, checks))
        return checks

    def get_diagonal_checks(self, ai, aj, di, dj, a):
        """
        This method returns the checks along the diagonal path.
        """
        checks = list()
        for (i, j) in zip(ai, aj):
            sub_mat, sub_shape = self.get_sub_matrix(ai=i, aj=j, di=di, dj=dj)
            if sub_shape[0] == sub_shape[1]:
                if a not in sub_mat.diagonal():
                    sub_mat = np.flipud(m=sub_mat)
                checks.append(np.where(sub_mat.diagonal() != '1')[0])
            else:
                continue
        checks = list(filter(lambda x: len(x) == 2, checks))
        return checks

    def get_knight_checks(self, ai, aj, di, dj):
        """
        This method returns the checks along the L-shaped paths for knights.
        """
        checks = list()
        for (i, j) in zip(ai, aj):
            attack_positions = [(i-2, j-1), (i-2, j+1),
                                (i-1, j-2), (i-1, j+2),
                                (i+1, j-2), (i+1, j+2),
                                (i+2, j-1), (i+2, j+1)]
            if (di, dj) in attack_positions:
                checks.append((i, j))
        return checks

    def get_pawn_checks(self, ai, aj, di, dj):
        """
        This method returns the checks for pawns.
        """
        checks = list()
        for (i, j) in zip(ai, aj):
            _, sub_shape = self.get_sub_matrix(ai=i, aj=j, di=di, dj=dj)
            if sub_shape[0] == 2 and sub_shape[1] == 2:
                checks.append((i, j))
            else:
                continue
        return checks

    def king_checks_king(self, attacker, defendant):
        """
        This method checks if the king is being attacked by the other king.
        This is unlikely, but I am just adding a validation rule.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        ai, aj = ai[0], aj[0]
        attack_positions = [(di, dj-1), (di, dj+1),
                            (di-1, dj), (di+1, dj),
                            (di-1, dj+1), (di-1, dj-1),
                            (di+1, dj-1), (di+1, dj+1)]
        if (ai, aj) in attack_positions:
            flag = True
        return flag

    def rook_checks_king(self, attacker, defendant):
        """
        This method checks if the king is being attacked by the rook.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        checks = self.get_straight_checks(
            ai=ai, aj=aj, di=di, dj=dj, a=attacker, d=defendant)
        if checks:
            flag = True
        return flag

    def bishop_checks_king(self, attacker, defendant):
        """
        This method checks if the king is being attacked by the bishop.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        checks = self.get_diagonal_checks(
            ai=ai, aj=aj, di=di, dj=dj, a=attacker)
        if checks:
            flag = True
        return flag

    def knight_checks_king(self, attacker, defendant):
        """
        This method checks if the king is being attacked by the knight.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        checks = self.get_knight_checks(ai=ai, aj=aj, di=di, dj=dj)
        if checks:
            flag = True
        return flag

    def queen_checks_king(self, attacker, defendant):
        """
        This method checks if the king is being attacked by the queen.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        straight_checks = self.get_straight_checks(
            ai=ai, aj=aj, di=di, dj=dj, a=attacker, d=defendant)
        diagonal_checks = self.get_diagonal_checks(
            ai=ai, aj=aj, di=di, dj=dj, a=attacker)
        if straight_checks or diagonal_checks:
            flag = True
        return flag

    def pawn_checks_king(self, attacker, defendant):
        """
        This methos checks if the king is being attacked by the pawn.

        Note: It is hard to determine from an image, which side of 
              the chessboard is black or is white.
              Hence, this method assumes the pawn is attacking the king 
              if both the pieces are diagnolly aligned by 1 step.
        """
        flag = False
        di, dj = self.get_piece_positions(notation=defendant)
        if len(di) == 1 and len(dj) == 1:
            di, dj = di[0], dj[0]
        else:
            return flag
        ai, aj = self.get_piece_positions(notation=attacker)
        checks = self.get_pawn_checks(ai=ai, aj=aj, di=di, dj=dj)
        if checks:
            flag = True
        return flag
```

**3.6.2.1\. 检查分布（仅限合法图像）**

下面是训练数据集的检查分布。

![](../Images/1b0ada5c236810ec9b7b1fbbcdb004d4.png)

作者提供的图像 — 检查训练数据集的分布

下面是测试数据集的检查分布。

![](../Images/dfc545d25e14d934bbc1f613dd9b5e80.png)

作者提供的图像 — 测试数据集的检查分布

上述检查分布图的结论：

1.  车攻击对方国王的频率高于其他棋子。

1.  兵攻击对方国王的频率低于其他棋子。

**3.6.3\. 棋盘上的非法位置**

```py
class IllegalPosition(Check):
    """
    This class finds if the pieces are illegally positioned in the chessboard.
    """

    def __init__(self, fen_label):
        super().__init__(fen_label=fen_label)

    def are_kings_less(self):
        """
        Rule on kings.
        """
        k_c = self.fen_label.count('k')
        K_c = self.fen_label.count('K')
        return (k_c < 1 and K_c < 1) or (k_c < 1) or (K_c < 1)

    def are_kings_more(self):
        """
        Rule on kings.
        """
        k_c = self.fen_label.count('k')
        K_c = self.fen_label.count('K')
        return (k_c > 1 and K_c > 1) or (k_c > 1) or (K_c > 1)

    def are_queens_more(self):
        """
        Rule on queens.
        """
        q_c = self.fen_label.count('q')
        Q_c = self.fen_label.count('Q')
        return (q_c > 9 and Q_c > 9) or (q_c > 9) or (Q_c > 9)

    def are_bishops_more(self):
        """
        Rule on bishops.
        """
        b_c = self.fen_label.count('b')
        B_c = self.fen_label.count('B')
        return (b_c > 10 and B_c > 10) or (b_c > 10) or (B_c > 10)

    def are_knights_more(self):
        """
        Rule on knights.
        """
        n_c = self.fen_label.count('n')
        N_c = self.fen_label.count('N')
        return (n_c > 10 and N_c > 10) or (n_c > 10) or (N_c > 10)

    def are_rooks_more(self):
        """
        Rule on rooks.
        """
        r_c = self.fen_label.count('r')
        R_c = self.fen_label.count('R')
        return (r_c > 10 and R_c > 10) or (r_c > 10) or (R_c > 10)

    def are_pawns_more(self):
        """
        Rule on pawns.
        """
        p_c = self.fen_label.count('p')
        P_c = self.fen_label.count('P')
        return (p_c > 8 and P_c > 8) or (p_c > 8) or (P_c > 8)

    def rule_1(self):
        """
        This method checks the count of the kings and the pieces in the board.
        1\. The count of white king and black king should always be 1.
        2\. The count of white queen and/or black queen should not cross 9.
        3\. The count of white bishop and/or black bishop should not cross 10.
        4\. The count of white knight and/or black knight should not cross 10.
        5\. The count of white rook and/or black rook should not cross 10.
        6\. The count of while pawn and/or black pawn should not cross 8.
        7\. The chessboard should never be empty.
        """
        flag = False
        if self.are_kings_less():
            flag = True
        elif self.are_kings_more():
            flag = True
        elif self.are_queens_more():
            flag = True
        elif self.are_bishops_more():
            flag = True
        elif self.are_knights_more():
            flag = True
        elif self.are_rooks_more():
            flag = True
        elif self.are_pawns_more():
            flag = True
        return flag

    def rule_2(self):
        """
        This method checks if the pawns are in the first and last row of the board.
        1\. No pawn should be on the first row and/or on the last row.
           The pawn that reaches the last row always gets promoted.
           Hence no pawns on the last row.
        """
        flag = False
        fen_label_list = self.fen_label.split('/')
        f_row, l_row = fen_label_list[0], fen_label_list[-1]
        p_f_row = 'p' in f_row
        p_l_row = 'p' in l_row
        P_f_row = 'P' in f_row
        P_l_row = 'P' in l_row
        if (p_f_row and p_l_row) or p_f_row or p_l_row:
            flag = True
        elif (P_f_row and P_l_row) or P_f_row or P_l_row:
            flag = True
        return flag

    def rule_3(self):
        """
        This method checks if the king is attacking the other king.
        1\. The king never checks the other king.
        2\. The king can attack other enemy pieces except the enemy king.
        """
        return self.king_checks_king(attacker='k', defendant='K')

    def rule_4(self):
        """
        This method checks if the kings are under check simultaneously.
        1\. The two kings are never under check at the same time.
        """
        r_checks_K = self.rook_checks_king(attacker='r', defendant='K')
        n_checks_K = self.knight_checks_king(attacker='n', defendant='K')
        b_checks_K = self.bishop_checks_king(attacker='b', defendant='K')
        q_checks_K = self.queen_checks_king(attacker='q', defendant='K')
        p_checks_K = self.pawn_checks_king(attacker='p', defendant='K')
        R_checks_k = self.rook_checks_king(attacker='R', defendant='k')
        N_checks_k = self.knight_checks_king(attacker='N', defendant='k')
        B_checks_k = self.bishop_checks_king(attacker='B', defendant='k')
        Q_checks_k = self.queen_checks_king(attacker='Q', defendant='k')
        P_checks_k = self.pawn_checks_king(attacker='P', defendant='k')
        is_K_checked = r_checks_K or n_checks_K or b_checks_K or q_checks_K or p_checks_K
        is_k_checked = R_checks_k or N_checks_k or B_checks_k or Q_checks_k or P_checks_k
        return is_K_checked and is_k_checked

    def is_illegal(self):
        """
        This method is a consolidation of all the above basic rules of chess.
        """
        return self.rule_1() or self.rule_2() or self.rule_3() or self.rule_4()
```

**3.6.3.1\. 棋盘上的合法和非法位置**

在使用上述类过滤训练和测试数据集后，我获得了以下结果。

训练数据集（*80000*个棋盘图像）。

1.  合法训练棋盘图像的数量为*67813（84.8%）*。

1.  非法训练棋盘图像的数量为*12187（15.2%）*。

测试数据集（*20000*个棋盘图像）。

1.  合法测试棋盘图像的数量为*17019（85.1%）*。

1.  非法测试棋盘图像的数量为*2981（14.9%）*。

**3.6.3.2\. 合法棋盘图像的样本图**

![](../Images/a29395b3109b5ea8ccf637efaa4a1029.png)

作者提供的图片 — 合法棋类图像样本

**3.6.3.3\. 非法棋类图像的样本图**

![](../Images/3a864dea8d16e9639eda90fe40e28927.png)

作者提供的图片 — 非法棋类图像样本

## 3.7\. 所有图像的比例、高度和宽度

![](../Images/e0898fac29ef1c3e8f09e4b5b3d28a23.png)

作者提供的图片 — 比例、高度和宽度

上述图像尺寸图的结论：

1.  比例 = 高度 / 宽度 → *1*。所有图像的比例均为*1*。

1.  所有图像的宽度均为*400*像素。

1.  所有图像的高度均为*400*像素。

# 4\. 数据管道

我创建了一个数据管道类，用于将数据输入到模型（学习器）中。

在此之前，我想展示预处理步骤。在预处理过程中，我将棋类图像调整为*50%*，并将其划分为*64*个块（正方形）。调整数据集的主要优点是减少空间复杂性。RAM不会超负荷。

预处理前的棋类图像：

![](../Images/23520ce1b398976eabd127466de14884.png)

作者提供的图片 — 调整大小和预处理前

预处理后的棋类图像：

![](../Images/555122ddcce0a32ce475604e8a1ff64c.png)

作者提供的图片 — 调整大小和预处理后

我使用了Pavel Koryakin（数据集作者）的独热编码逻辑来对标签进行编码。

以下是数据管道类。

```py
class DataPipeline(object):
    """
    This class is a data pipeline for deep learning model.
    """

    def __init__(self, tr_images, tr_labels, cv_images, cv_labels, te_images, te_labels):
        self.rows, self.cols = (8, 8)
        self.square = None
        self.h, self.w, self.c = None, None, None
        self.N = 13
        self.tr_images = np.array(tr_images)
        self.tr_labels = np.array(tr_labels)
        self.cv_images = np.array(cv_images)
        self.cv_labels = np.array(cv_labels)
        self.te_images = np.array(te_images)
        self.te_labels = np.array(te_labels)
        self.piece_symbols = 'prbnkqPRBNKQ'

    def preprocess_input_image(self, imagefile, resize_scale=(200, 200)):
        """
        This function preprocesses in the input image.
        """
        img = cv.imread(filename=imagefile)
        img = cv.resize(src=img, dsize=resize_scale)

        self.h, self.w, self.c = img.shape
        self.square = self.h // self.rows

        img_blocks = view_as_blocks(
            arr_in=img, block_shape=(self.square, self.square, self.c))
        img_blocks = img_blocks.reshape(
            self.rows * self.cols, self.square, self.square, self.c)

        return img_blocks

    def tr_data_generator(self):
        """
        This method preprocess the input images.
        """
        for i, l in zip(self.tr_images, self.tr_labels):
            yield (self.preprocess_input_image(imagefile=i),
                   self.onehot_from_fen(fen=l))

    def cv_data_generator(self):
        """
        This method preprocess the input images.
        """
        for i, l in zip(self.cv_images, self.cv_labels):
            yield (self.preprocess_input_image(imagefile=i),
                   self.onehot_from_fen(fen=l))

    def te_data_generator(self):
        """
        This method preprocess the input targets.
        """
        for i in self.te_images:
            yield self.preprocess_input_image(imagefile=i)

    def onehot_from_fen(self, fen):
        """
        This method converts FEN to onehot.
        The original author of this method is 'Pavel Koryakin'.
        Pavel Koryakin is also the maintainer of Chess Positions dataset.
        """
        eye = np.eye(N=self.N)
        output = np.empty(shape=(0, self.N))
        fen = re.sub(pattern='[/]', repl='', string=fen)

        for char in fen:
            if char in '12345678':
                output = np.append(
                    arr=output,
                    values=np.tile(A=eye[self.N-1], reps=(int(char), 1)), axis=0
                )
            else:
                idx = self.piece_symbols.index(char)
                output = np.append(
                    arr=output,
                    values=eye[idx].reshape((1, self.N)), axis=0
                )

        return output

    def fen_from_onehot(self, onehot):
        """
        This method converts onehot to FEN.
        The original author of this method is 'Pavel Koryakin'.
        Pavel Koryakin is also the maintainer of Chess Positions dataset.
        """
        output = str()
        for j in range(self.rows):
            for i in range(self.cols):
                if onehot[j][i] == 12: # TensorFlow coded 12 for empty squares.
                    output += ' '
                else:
                    output += self.piece_symbols[int(onehot[j][i])]
            if j != self.rows - 1:
                output += '/'

        for i in range(self.rows, 0, -1):
            output = output.replace(' ' * i, str(i))

        return output

    def construct_dataset(self):
        """
        This method constructs the dataset.
        """
        tr_dataset = tf.data.Dataset.from_generator(
            generator=self.tr_data_generator, output_types=(tf.int64, tf.int64))
        tr_dataset = tr_dataset.repeat()

        cv_dataset = tf.data.Dataset.from_generator(
            generator=self.cv_data_generator, output_types=(tf.int64, tf.int64))
        cv_dataset = cv_dataset.repeat()

        te_dataset = tf.data.Dataset.from_generator(
            generator=self.te_data_generator, output_types=tf.int64)
        te_dataset = te_dataset.repeat()

        it_tr = tr_dataset.__iter__()
        it_cv = cv_dataset.__iter__()
        it_te = te_dataset.__iter__()

        return it_tr, it_cv, it_te
```

使用上述类，我创建了训练、验证和测试数据集生成器，可以用于建模。

# 5\. 建模

我创建了一个建模类，该类首先对模型进行调优，然后用数据集拟合模型。调优是使用KerasTuner进行的。调优模型所需时间为20小时，以获得最佳超参数。

## 5.1\. 基础模型

以下是建模类。

```py
class ChessModel(object):
    """
    This class is for deep learning model for chess recognition problem.
    """

    def __init__(self,
                 tr_dataset,
                 cv_dataset,
                 tr_size,
                 cv_size,
                 filepath_tuner,
                 filepath_fitter,
                 filepath_tracker):
        self.tr_dataset = tr_dataset
        self.cv_dataset = cv_dataset
        self.input_shape = (25, 25, 3)
        self.batch_size = 64
        self.output_units = 13 # 12 for chess pieces and 1 for empty square.
        self.tr_size = tr_size
        self.cv_size = cv_size
        self.filepath_tuner = filepath_tuner
        self.filepath_fitter = filepath_fitter
        self.filepath_tracker = filepath_tracker

    def build_model(self, hp):
        """
        This method builds the optimized model.
        """
        hp_activations = hp.Choice(
            name='activation', values=['relu', 'tanh', 'sigmoid'])
        hp_filters_1 = hp.Int(
            name='filter_1', min_value=32, max_value=64, step=10)
        hp_filters_2 = hp.Int(
            name='filter_2', min_value=32, max_value=64, step=10)
        hp_kernel_1 = hp.Int(
            name='Kernel_1', min_value=2, max_value=5, step=None)
        hp_kernel_2 = hp.Int(
            name='Kernel_2', min_value=2, max_value=5, step=None)
        hp_units = hp.Int(
            name='dense', min_value=32, max_value=64, step=10)
        hp_learning_rate = hp.Choice(
            name='learning_rate', values=[1e-2, 1e-3, 1e-4])

        input_layer = Input(
            shape=self.input_shape, batch_size=self.batch_size, name='Input')
        conv_2d_layer_1 = Conv2D(
            filters=hp_filters_1, kernel_size=hp_kernel_1,
            activation=hp_activations, name='Conv2D_1')(input_layer)
        conv_2d_layer_2 = Conv2D(
            filters=hp_filters_2, kernel_size=hp_kernel_2,
            activation=hp_activations, name='Conv2D_2')(conv_2d_layer_1)
        flatten_layer = Flatten(name='Flatten')(conv_2d_layer_2)
        dense_layer = Dense(
            units=hp_units, activation=hp_activations, name='Dense')(flatten_layer)
        output_layer = Dense(
            units=self.output_units, activation='softmax', name='Output')(dense_layer)

        model = Model(inputs=input_layer, outputs=output_layer, name='Chess_Model')

        optimizer = tf.keras.optimizers.Adam(learning_rate=hp_learning_rate)
        model.compile(
            optimizer=optimizer, loss='categorical_crossentropy',
            metrics=['accuracy'])

        return model

    def model_tuner(self):
        """
        This method tunes the chess model.
        """
        if not os.path.isfile(path=self.filepath_tuner):
            print("Tuning the model.")
            stop_early = tf.keras.callbacks.EarlyStopping(monitor='val_loss', patience=3)
            tuner = kt.Hyperband(
                hypermodel=self.build_model, objective='val_accuracy', max_epochs=10)
            tuner.search(
                x=tr_dataset, epochs=50, steps_per_epoch=self.tr_size,
                validation_data=cv_dataset, validation_steps=self.cv_size,
                callbacks=[stop_early])
            print("Tuning completed.")

            best_hps = tuner.get_best_hyperparameters(num_trials=1)[0]
            model = tuner.hypermodel.build(best_hps)
            tf.keras.models.save_model(model=model, filepath=self.filepath_tuner)
            print("Saved the best model to the file.")
        else:
            print("Model is already tuned, and is also saved.")
            model = tf.keras.models.load_model(filepath=self.filepath_tuner)
            print("Loaded the tuned model and ready for fitting.")

        return model

    def model_fitter(self):
        """
        This method fits the tuned model.
        """
        model = self.model_tuner()
        print()
        model.summary()
        print()

        model_save_callback = ModelCheckpoint(
            filepath=self.filepath_fitter, monitor='val_accuracy',
            verbose=1, save_best_only=True, mode='auto')
        callbacks = [model_save_callback]

        if not os.path.isfile(path=self.filepath_fitter):
            print("Fitting the model.")

            epochs = 10
            tracker = model.fit(
                x=tr_dataset, validation_data=cv_dataset, epochs=epochs,
                steps_per_epoch=len(tr_images), validation_steps=len(cv_images),
                callbacks=callbacks)
            print("\nSaved the fitted model.")

            tracker_df = pd.DataFrame(data=tracker.history)
            tracker_df.to_csv(path_or_buf=self.filepath_tracker, index=False)
            print("Saved the history to the file.")
        else:
            print("Model is already fitted, and is also saved.")
            model = tf.keras.models.load_model(filepath=self.filepath_fitter)
            print("Loaded the fitted model and ready for prediction.")

            tracker_df = pd.read_csv(filepath_or_buffer=self.filepath_tracker)

        print()
        plot_model_performance(tracker_df=tracker_df)

        return model
```

## 5.2\. 模型架构

```py
Model: "Chess_Model"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 Input (InputLayer)          [(64, 25, 25, 3)]         0         

 Conv2D_1 (Conv2D)           (64, 21, 21, 32)          2432      

 Conv2D_2 (Conv2D)           (64, 19, 19, 62)          17918     

 Flatten (Flatten)           (64, 22382)               0         

 Dense (Dense)               (64, 42)                  940086    

 Output (Dense)              (64, 13)                  559       

=================================================================
Total params: 960,995
Trainable params: 960,995
Non-trainable params: 0
_________________________________________________________________
```

## 5.3\. 模型性能 — 损失和准确度

在*10*个周期后拟合模型后，我获得了模型的性能 — 损失和准确度。

![](../Images/1872c98f20ad28860325de9559e39b80.png)

作者提供的图片 — 准确度和损失

以下是*25*张测试图像的混淆矩阵。

![](../Images/a10ad079966963d16cec5b1dd1c6a458.png)

作者提供的图片 — 25张测试图像的混淆矩阵

# 6\. 数据产品的生产化

生产化是将本地模型从Jupyter Notebook环境暴露给外部世界的过程。在这里，我将建模阶段训练过的模型导出为文件。这个模型文件包含了已学习的参数，可以直接用于测试数据。

## 6.1\. 数据产品管道

```py
from chess_positions import IllegalPosition
from chess_positions import Check
from glob import glob
from skimage.util.shape import view_as_blocks

import cv2 as cv
import plotly.express as px
import random
import tensorflow as tf
import warnings
warnings.filterwarnings(action='ignore')

class Pipeline(object):
    """
    This class is a pipeline mechanism to feed the
    query chess image into the model for FEN prediction.
    """

    def __init__(self, chess_image):
        self.piece_symbols = "prbnkqPRBNKQ"
        self.rows, self.cols = (8, 8)
        self.square = None
        self.h, self.w, self.c = None, None, None
        self.chess_image = chess_image
        self.chess_model = tf.keras.models.load_model(
            filepath='chess_model.h5')
        self.chess_image_display = self.display_image()

    def display_image(self):
        """
        This method reads the image and
        gives plotly fig for the final display.
        """
        image = cv.imread(filename=self.chess_image)
        image = cv.cvtColor(src=image, code=cv.COLOR_BGR2RGB)

        image_fig = px.imshow(img=image)
        image_fig.update_layout(
            coloraxis_showscale=False, autosize=True,
            margin=dict(l=0, r=0, b=0, t=0))
        image_fig.update_xaxes(showticklabels=False)
        image_fig.update_yaxes(showticklabels=False)
        return image_fig

    def preprocess(self, resize_scale=(200, 200)):
        """
        This method preprocesses the chess image.
        """
        img = cv.imread(filename=self.chess_image)
        img = cv.resize(src=img, dsize=resize_scale)

        self.h, self.w, self.c = img.shape
        self.square = self.h // self.rows

        img_blocks = view_as_blocks(
            arr_in=img, block_shape=(self.square, self.square, self.c))
        img_blocks = img_blocks.reshape(
            self.rows * self.cols, self.square, self.square, self.c)

        return img_blocks

    def fen_from_onehot(self, onehot):
        """
        This method converts onehot to FEN.
        The original author of this method is 'Pavel Koryakin'.
        Pavel Koryakin is also the maintainer of Chess Positions dataset.
        """
        output = str()
        for j in range(self.rows):
            for i in range(self.cols):
                if onehot[j][i] == 12: # TensorFlow coded 12 for empty squares.
                    output += ' '
                else:
                    output += self.piece_symbols[int(onehot[j][i])]
            if j != self.rows - 1:
                output += '/'

        for i in range(self.rows, 0, -1):
            output = output.replace(' ' * i, str(i))

        return output

    def predict(self):
        """
        This method predicts the FEN of the query chess image.
        """
        chess_image_blocks = self.preprocess()

        onehot = self.chess_model.predict(x=chess_image_blocks)
        onehot = onehot.argmax(axis=1).reshape(-1, 8, 8)[0]

        fen_label = self.fen_from_onehot(onehot=onehot)

        interpretation = self.illegal_interpreter(fen_label=fen_label)
        if len(interpretation) > 0:
            interpretation = f"This is an illegal chess position. Reason is {interpretation}"
        else:
            interpretation = self.check_interpreter(fen_label=fen_label)

        fen_label = f"The Forsyth-Edwards Notation (FEN) of an uploaded chess image is {fen_label}."
        interpretation = f"Further interpretation: {interpretation}"

        return fen_label, interpretation

    def illegal_interpreter(self, fen_label):
        """
        This method interprets the predicted FEN.
        """
        reason = str()

        chess_illegal = IllegalPosition(fen_label=fen_label)

        if chess_illegal.are_kings_less():
            reason += "either white king, black king, or both are missing."
        elif chess_illegal.are_kings_more():
            reason += "either white king, black king, or both are more than 1."
        elif chess_illegal.are_queens_more():
            reason += "either white queen, black queen, or both are more than 9."
        elif chess_illegal.are_bishops_more():
            reason += "either white bishop, black bishop, or both are more than 10."
        elif chess_illegal.are_knights_more():
            reason += "either white knight, black knight, or both are more than 10."
        elif chess_illegal.are_rooks_more():
            reason += "either white rook, black rook, or both are more than 10."
        elif chess_illegal.are_pawns_more():
            reason += "either white pawn, black pawn, or both are more than 8."
        elif chess_illegal.rule_2():
            reason += "either white pawn, black pawn, or both are in first row and/or last row."
        elif chess_illegal.rule_3():
            reason += "the king checks the other the king."
        elif chess_illegal.rule_4():
            reason += "white king and black king are under attack simultaneously."
        else:
            reason += ""

        return reason

    def check_interpreter(self, fen_label):
        """
        This method interprets the predicted FEN.
        """
        reason = str()

        chess_check = Check(fen_label=fen_label)

        r_checks_K = chess_check.rook_checks_king(
            attacker='r', defendant='K')
        n_checks_K = chess_check.knight_checks_king(
            attacker='n', defendant='K')
        b_checks_K = chess_check.bishop_checks_king(
            attacker='b', defendant='K')
        q_checks_K = chess_check.queen_checks_king(
            attacker='q', defendant='K')
        p_checks_K = chess_check.pawn_checks_king(
            attacker='p', defendant='K')
        R_checks_k = chess_check.rook_checks_king(
            attacker='R', defendant='k')
        N_checks_k = chess_check.knight_checks_king(
            attacker='N', defendant='k')
        B_checks_k = chess_check.bishop_checks_king(
            attacker='B', defendant='k')
        Q_checks_k = chess_check.queen_checks_king(
            attacker='Q', defendant='k')
        P_checks_k = chess_check.pawn_checks_king(
            attacker='P', defendant='k')

        is_K_checked = r_checks_K or n_checks_K or b_checks_K or q_checks_K or p_checks_K
        is_k_checked = R_checks_k or N_checks_k or B_checks_k or Q_checks_k or P_checks_k

        if is_K_checked:
            reason += "The white king is under attack."
        elif is_k_checked:
            reason += "The black king is under attack."
        else:
            reason += "Both kings are safe."

        return reason
```

## 6.2\. 数据产品演示

数据产品链接：[https://huggingface.co/spaces/mohd-saifuddin/Chess-Recognition-2D](https://huggingface.co/spaces/mohd-saifuddin/Chess-Recognition-2D)

请注意，您需要测试图像来使用此数据产品。因此，我建议您从数据集源下载测试图像。

# 7\. 学习成果

我在这个项目中获得的学习成果。

1.  我学会了对棋类图像和FEN标签进行详细的EDA。

1.  我学会了数据预处理和使用TensorFlow Data模块。

1.  我学会了使用KerasTuner进行超参数调整（仍然有许多概念需要学习）。

1.  最终，我学会了开发数据产品并将其发布在Streamlit平台上。

# 8. 参考文献

[1] Pavel Koryakin, 国际象棋位置。在*Kaggle*。[这里](https://www.kaggle.com/datasets/koryakinp/chess-positions)。

[2] Forsyth–Edwards符号。在*维基百科*。[这里](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation)。

# 9. 结束

感谢阅读。如果你有任何建议，请告诉我。

1.  深度学习代码：[这里](https://github.com/mohd-saifuddin/Chess-Recognition-Problem)。

1.  Streamlit应用代码：[这里](https://github.com/mohd-saifuddin/Chess-Recognition-Application)。

你可以在LinkedIn上与我联系：[这里](https://www.linkedin.com/in/mohammed-saifuddin-850a6b133/)。
