# 理解 DeepMind 矩阵乘法

> 原文：[`towardsdatascience.com/understanding-deepmind-matrix-multiplication-c8dc49687ce7?source=collection_archive---------3-----------------------#2023-02-11`](https://towardsdatascience.com/understanding-deepmind-matrix-multiplication-c8dc49687ce7?source=collection_archive---------3-----------------------#2023-02-11)

## DeepMind 矩阵乘法在 NVIDIA V100、Tesla T4 上的表现，以及 FBHHRBNRSSSHK 的分析——这不是我在随意输入字母！

[](https://stefanobosisio1.medium.com/?source=post_page-----c8dc49687ce7--------------------------------)![Stefano Bosisio](https://stefanobosisio1.medium.com/?source=post_page-----c8dc49687ce7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8dc49687ce7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8dc49687ce7--------------------------------) [Stefano Bosisio](https://stefanobosisio1.medium.com/?source=post_page-----c8dc49687ce7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff7141087b94&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deepmind-matrix-multiplication-c8dc49687ce7&user=Stefano+Bosisio&userId=ff7141087b94&source=post_page-ff7141087b94----c8dc49687ce7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8dc49687ce7--------------------------------) · 7 分钟阅读 · 2023 年 2 月 11 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc8dc49687ce7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deepmind-matrix-multiplication-c8dc49687ce7&user=Stefano+Bosisio&userId=ff7141087b94&source=-----c8dc49687ce7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc8dc49687ce7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deepmind-matrix-multiplication-c8dc49687ce7&source=-----c8dc49687ce7---------------------bookmark_footer-----------)![](img/a8a82776e50742a320323c87c1b96767.png)

图片由 [Ivan Diaz](https://unsplash.com/@ivvndiaz) 提供，发布在 [Unsplash](https://unsplash.com/photos/Z-PU9Lv441Y)

在上一篇 文章 中，我们学习了 Strassen 算法背后的数学原理，并编写了 Python 代码以测试不同矩阵大小的运行情况。此外，我们了解到线性代数的终极目标是矩阵乘法的优化算法。通常，我们会将矩阵乘法代码想象成三个 for 循环：

```py
def matmul(mat1, mat2, mat3):
    r""" Function to multiply mat1 and mat2 
    returns mat3 
    Parameters
    ---------
    mat1: np.array, matrix A 
    mat2: np.array, matrix B 
    mat3: np.array, empty matrix C 

    Return 
    ------
    mat3: np.array, matmul between A & B
    """ 
    for i in range(mat1.shape):
        for j in range(mat2.shape):
            mat3[i][j] = 0.0 
            for k in range(mat3.shape):
                mat3[i][j] += mat1[i][k]*mat2[k][j]
    return mat3
```

因此计算复杂度是 *O(n³)*。斯特拉森改进了这一计算，发现了以下关系：
