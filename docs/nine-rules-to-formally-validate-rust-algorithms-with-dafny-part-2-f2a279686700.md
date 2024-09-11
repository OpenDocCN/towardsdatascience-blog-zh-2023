# 使用 Dafny 正式验证 Rust 算法的九条规则（第2部分）

> 原文：[https://towardsdatascience.com/nine-rules-to-formally-validate-rust-algorithms-with-dafny-part-2-f2a279686700?source=collection_archive---------5-----------------------#2023-10-21](https://towardsdatascience.com/nine-rules-to-formally-validate-rust-algorithms-with-dafny-part-2-f2a279686700?source=collection_archive---------5-----------------------#2023-10-21)

## 验证 range-set-blaze Crate 的经验教训

[](https://medium.com/@carlmkadie?source=post_page-----f2a279686700--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----f2a279686700--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2a279686700--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f2a279686700--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----f2a279686700--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-to-formally-validate-rust-algorithms-with-dafny-part-2-f2a279686700&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----f2a279686700---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2a279686700--------------------------------) ·14 分钟阅读·2023年10月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff2a279686700&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-to-formally-validate-rust-algorithms-with-dafny-part-2-f2a279686700&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----f2a279686700---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff2a279686700&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-to-formally-validate-rust-algorithms-with-dafny-part-2-f2a279686700&source=-----f2a279686700---------------------bookmark_footer-----------)

作者：**Carl M. Kadie** 和 **Divyanshu Ranjan**

![](../Images/ce250f929d2fc998fb6b5fe42dfd5933.png)

蟹证明毕达哥拉斯定理 — 来源：[https://openai.com/dall-e-3/](https://openai.com/dall-e-3/) & [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/) [文件:Pythagorean.svg](https://commons.wikimedia.org/wiki/File:Pythagorean.svg)

这是关于使用 Dafny 正式验证 Rust 算法的文章第2部分。我们查看第7至第9条规则：

+   7\. 将你的真实算法迁移到 Dafny。

+   8\. 验证你算法的 Dafny 版本。

+   9\. 重新审视你的验证以确保可靠性。

参见 [第 1 部分](/nine-rules-to-formally-validate-rust-algorithms-with-dafny-part-1-5cb8c8a0bb92) 的规则 1 到 6：

1.  不要学习 Dafny。

1.  学习 Dafny。

1.  定义你的算法的基本概念。

1.  指定你的算法。

1.  向 Dafny 社区寻求帮助。

1.  验证不同的、更简单的算法。

这些规则来自我们验证一个来自 `[range-set-blaze](https://crates.io/crates/range-set-blaze)` 的算法的经验，这个 Rust crate 用于处理“分散”整数的集合。

记住第 6 规则，来自 [第 1 部分](/nine-rules-to-formally-validate-rust-algorithms-with-dafny-part-1-5cb8c8a0bb92)，显示我们可以验证 *一个* 算法的 `InternalAdd`，但它不是 Rust crate 中使用的 *那个* 算法。接下来我们转向那个算法。

# 规则 7：将你的真实算法移植到 Dafny。

这里是感兴趣的 Rust 函数，部分代码暂时省略：

```py
// https://stackoverflow.com/questions/49599833/how-to-find-next-smaller-key-in-btreemap-btreeset
// https://stackoverflow.com/questions/35663342/how-to-modify-partially-remove-a-range-from-a-btreemap
fn internal_add(&mut self, range: RangeInclusive<T>) {
    let (start, end) = range.clone().into_inner();
    assert!(
        end <= T::safe_max_value(),
        "end must be <= T::safe_max_value()"
    );
    if end < start {
        return;
    }
    //... code continues ...
}
```

这里是 Dafny 移植版的开始部分：

```py
method InternalAdd(xs: seq<NeIntRange>, range: IntRange) returns (r: seq<NeIntRange>)
  requires ValidSeq(xs)
  ensures ValidSeq(r)
  ensures SeqToSet(r) == SeqToSet(xs) + RangeToSet(range)
{
  var (start, end) := range;
  if end < start {
    r := xs;
    return;
  }
 //... code continues ...
}
```

一些可能感兴趣的点：

+   Rust 代码使用 `self` 和类似面向对象的封装。Dafny 支持这种编码风格，但为了简便，我在这里没有使用。具体来说，Rust 代码会改变 `self`。我选择了用更函数式的方式编写 Dafny 代码——它接受一个不可变的序列并返回一个新的不可变序列。

+   Rust 代码通过借用检查器来管理内存。这导致了诸如 `range.clone()` 的表达式。Dafny 通过垃圾回收器来管理内存。在这两种情况下，内存安全都会得到保证。因此，我们在此验证中忽略它。

+   Rust 代码对 `T` 是泛型的，我在其他地方定义它包括所有标准的 Rust 整数类型，例如 `u8`、`isize`、`i128`。Dafny 代码定义在 `int` 上，这是一个表示任意大小整数的单一类型。这意味着这个 Dafny 移植版不需要检查整数溢出。 [参见 [上一篇文章](https://medium.com/@carlmkadie/check-ai-generated-code-perfectly-and-automatically-d5b61acff741) 了解如何用 Kani Rust 验证器正式证明溢出安全。]

+   Rust 代码包含一个运行时的 `assert!`，这是 Rust 中用于禁止一种特殊情况的：将 `u128::max_value` 插入到 `RangeSetBlaze<u128>` 中。由于 Dafny 使用任意大小的 `int`，它忽略了这种特殊情况。

> 附注：Rust 的 *包含范围* `*0..=u128::max_value*`* 的长度是多少？答案是* `*u128::max_value*`*+1，这个值太大，无法用任何标准 Rust 整数类型表示。* `*range-set-blaze*` *crate 将范围限制为* `*0..=u128::max_value*`*-1，以便长度可以用* `*u128*` *表示。*

接下来我们考虑 `internal_add` 算法的其余部分。记住，我们有一些排序好的不相交的范围和一些非空的新范围，我们想要插入。例如

![](../Images/bdd7995a92e3e0f289594d16677a8f0e.png)

致谢：此处和以下的图表由作者提供。

算法要求我们找出哪个（如果有的话）现有范围在新范围的开始之前（或正好在开始）。称之为“之前”范围。然后我们考虑四种情况：

+   案例1：新范围不触及之前的范围，因此我们在检查是否触及任何其他范围时插入新范围。

+   案例2：新范围触及之前的范围并超出它，因此在检查是否触及任何其他范围时扩展之前范围的末尾。（当没有其他范围被触及时，这将非常快。）

![](../Images/a6c54954fb5c963daa94f9d07f2213d3.png)

+   案例3：新范围触及之前的范围但没有超出它，因此不做任何操作。（这将总是非常非常快。）

![](../Images/caca986ea7085c4078b89526376051b6.png)

+   案例4：新范围在任何范围之前开始，因此在检查是否触及任何其他范围时添加它。

![](../Images/98ba8a777a19b7e377230cf25bc791b9.png)

这是[Rust中的算法](https://github.com/CarlKCarlK/range-set-blaze/blob/f181c3fab31c8b215122402c24a5a7783189187a/src/lib.rs#L1003C2-L1003C59)：

```py
// code continued ...
// FUTURE: would be nice of BTreeMap to have a partition_point function that returns two iterators
    let mut before = self.btree_map.range_mut(..=start).rev();
    if let Some((start_before, end_before)) = before.next() {
        // Must check this in two parts to avoid overflow
        if match (*end_before).checked_add(&T::one()) {
            Some(end_before_succ) => end_before_succ < start,
            None => false,
        } {
            self.internal_add2(&range);
        } else if *end_before < end {
            self.len += T::safe_len(&(*end_before..=end - T::one()));
            *end_before = end;
            let start_before = *start_before;
            self.delete_extra(&(start_before..=end));
        } else {
            // completely contained, so do nothing
        }
    } else {
        self.internal_add2(&range);
    }
}
```

这里是Dafny中的算法：

```py
// code continued ...
  var beforeHi := IndexAtOrBeforePlusOne(xs, start);
  if beforeHi > 0 { // does not go at front
    var (startBefore, endBefore) := xs[beforeHi-1];
    if endBefore+1 < start {
      r := InternalAdd2(xs, range);
    } else if endBefore < end {
      r := xs[..beforeHi-1] + [(startBefore, end)] + xs[beforeHi..];
      assume exists i: nat :: i < |r| && r[i] == (startBefore,end) && ValidSeq(r[..i+1]) && ValidSeq(r[i+1..]);
      r := DeleteExtra(r, (startBefore,end));
    } else{
      r := xs;
    }
  }
  else // goes at front
  {
    r := InternalAdd2(xs, range);
  }
}
```

可能感兴趣的一些要点：

+   Rust代码通过键和值操作`BTreeMap`。Dafny代码则通过（随机访问）索引操作排序后的`seq`。我让Dafny操作镜像Rust操作，尽管这使得Dafny代码不那么自然。

+   Rust代码还更新了`self.len`，即RangeSetBlaze中的整数数量。Dafny代码忽略了这一点。（更新`len`是一个将来可能会添加到Dafny代码中的功能。）

+   和之前一样，Rust版本包含了Dafny忽略的防止溢出的代码。

我继续通过编写`internal_add2`和`delete_extra`的Dafny版本来完成移植，这两个函数是`internal_add`调用的。我通过编写这两个方法调用的其他方法等完成了移植。完整的移植代码约185行。你可以在[这里](https://github.com/CarlKCarlK/range-set-blaze/blob/oct23/tests/formal/Rule7.dfy)查看。

它没有验证。接下来我们将处理验证。

# 规则8：验证Dafny版本的算法。

在这一步，你将向代码中添加验证提示，例如，形式为`assert`语句。Dafny使用这些提示来尝试验证你的代码。作为Dafny初学者，我（Carl）发现添加提示比编写代码更困难。部分原因是因为我不知道Dafny何时（或是否）会满足条件，然后我可以停止。

然而，我确实学会了如何开始。例如，上述`InternalAdd`的代码产生了两个验证错误。首先，Dafny验证器报告说一个`ensures`可能不成立：

```py
This postcondition might not hold: SeqToSet(r) == SeqToSet(xs) + RangeToSet(range)
```

> 附注：“后置条件”对应于`*ensures*`。“前置条件”对应于`*requires*`。

其次，Dafny验证器抱怨`DeleteExtra`的前置条件（即`requires`之一）无法证明。

我们将首先关注第一个问题，通过在方法底部添加`assert`。我们写它是为了反映`ensures`。

```py
// ... adding this as the last line in InternalAdd
assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range);
}
```

我们将明确忽略`DeleteExtra`问题，暂时用`assume`来处理。

```py
// ...
      assume exists i: nat :: i < |r| && r[i] == (startBefore,end) && ValidSeq(r[..i+1]) && ValidSeq(r[i+1..]);
      r := DeleteExtra(r, (startBefore,end));
//...
```

Dafny验证器现在仅对我们的最终`assert`提出抱怨。它说“断言可能不成立。”

记住，`InternalAdd` 代码使用嵌套的 `if` 语句将其工作分成五种情况。接下来，我们将 `assert` 从方法的末尾移动到每个情况的末尾。请在结果中寻找以 `// case` 注释结尾的行：

```py
method InternalAdd(xs: seq<NeIntRange>, range: IntRange) returns (r: seq<NeIntRange>)
  requires ValidSeq(xs)
  ensures ValidSeq(r)
  ensures SeqToSet(r) == SeqToSet(xs) + RangeToSet(range)
{
  var (start, end) := range;
  if end < start {
    r := xs;
    assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range); // case 0 - validates
    return;
  }

  var beforeHi := IndexAtOrBeforePlusOne(xs, start);
  if beforeHi > 0 { // does not go at front
    var (startBefore, endBefore) := xs[beforeHi-1];
    if endBefore+1 < start {
      r := InternalAdd2(xs, range);
      assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range); // case 1 - validates
    } else if endBefore < end {
      r := xs[..beforeHi-1] + [(startBefore, end)] + xs[beforeHi..];
      assume exists i: nat :: i < |r| && r[i] == (startBefore,end) && ValidSeq(r[..i+1]) && ValidSeq(r[i+1..]);
      r := DeleteExtra(r, (startBefore,end));
      assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range); // case 2 - fails
    } else{
      r := xs;
      assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range); // case 3 - fails
    }
  }
  else // goes at front
  {
    r := InternalAdd2(xs, range);
    assert SeqToSet(r) == SeqToSet(xs) + RangeToSet(range); // case 4 - validates
  }
}
```

Dafny 现在告诉我们情况0、1和4已验证。情况2失败（并包含我们最终需要删除的 `assume`）。不过，现在让我们处理情况3。

回忆一下这篇文章的规则7，情况3是当我们添加一个新的范围（红色），该范围完全被现有范围（蓝色的“之前”范围）覆盖时，因此代码无需做任何操作。

![](../Images/caca986ea7085c4078b89526376051b6.png)

那么，从逻辑上讲，我们知道什么？我们知道之前范围覆盖的整数是新范围覆盖的整数的超集。我们还知道之前范围是我们原始的排序且不重叠的范围列表（蓝色范围）的一部分。我们将通过 `assert` 语句将这两个提示添加到我们的代码中：

![](../Images/3328b3e08fbdd1f1638f5c1b9e297c2a.png)

Dafny 同意这两个提示是正确的（绿色勾号），但它仍然不接受关注的 `assert`（红色标记）。

我们似乎需要再一个提示。具体来说，我们需要说服 Dafny 认为之前范围覆盖的整数是所有排序且不重叠范围列表中整数的子集。直观上，这是真的，因为之前范围是列表中的一个范围。

我们将这个提示写成一个没有主体的引理。Dafny 接受了它。

![](../Images/5ed147cca5272a75f1e140e098d1e87e.png)

> 附录：为什么 Dafny 会接受这个主体为空的引理？我不知道，也没有很好的直觉。这只是有效。如果无效，我会尝试在其主体中添加断言。

使用引理，情况3现在已验证：

![](../Images/a7542cff758d2f9c8a9427b959465d0b.png)

这意味着我们已验证情况0、1、3和4。接下来我们将处理情况2。此外，一些提到的方法，例如 `DeleteExtra`，尚未验证，我们需要对其进行处理。 [您可以看到截至目前的代码，[在这里](https://github.com/CarlKCarlK/range-set-blaze/blob/oct23/tests/formal/Rule8a.dfy)。]

有关验证调试的一般建议，请参阅 [Dafny 用户指南的这一部分](https://dafny.org/latest/DafnyRef/DafnyRef#sec-verification-debugging)。我还推荐 [这个 Stack Overflow 答案](https://stackoverflow.com/a/76925258/5976009) 和 James Wilcox 教授的迷你教程。

总的来说，想法是将验证算法的任务分解成许多更小的验证任务。我发现这比编程更困难，但并不太难，仍然很有趣。

我最终在185行常规代码中添加了大约200行验证代码（[完整代码在这里](https://github.com/CarlKCarlK/range-set-blaze/blob/oct23/tests/formal/Rule8b.dfy)）。当我最后验证完最后一个方法时，我错误地认为自己已经完成了。

令我惊讶（也是失望）的是，第一次所有内容验证通过并不意味着工作结束。你还必须确保你的项目将再次验证并且能为其他人验证。接下来，我们将讨论这一规则。

# 规则 9：重新审视你的验证以确保可靠性。

我以为我完成了。然后，我将数学 `Min` 函数的六行定义从[Dafny 标准库](https://github.com/dafny-lang/libraries)移动到我的代码中。这导致我的验证失败，没有逻辑原因（字面上！）。后来，在我以为已经修复了之后，我删除了一个未使用的方法。再次，验证因没有逻辑原因而开始失败。

发生了什么？Dafny 通过随机搜索进行启发式工作。表面上更改代码（或更改随机种子）可以改变搜索所需的时间。有时，时间的变化非常剧烈。如果新的时间超过了用户设置的时间限制，验证将失败。[我们将在下面的提示 #3 中进一步讨论时间限制。]

你应该通过尝试不同的随机种子来测试验证的可靠性。以下是我在 Windows 上使用的命令来验证一个具有 10 个随机种子的文件。

```py
@rem Find the location of Dafny and add it to your path
set path=C:\Users\carlk\.vscode-insiders\extensions\dafny-lang.ide-vscode-3.1.2\out\resources\4.2.0\github\dafny;%path%
dafny verify seq_of_sets_example7.dfy --verification-time-limit:30 --cores:20 --log-format csv --boogie -randomSeedIterations:10
```

结果是一个 *.csv 文件，你可以将其打开为电子表格，然后寻找失败：

![](../Images/0a9b3603cc057c1b94c598e28984c45b.png)

> 附注：有关测量 Dafny 验证可靠性的更多想法，请参见这个关于[分析 *.csv 文件](https://stackoverflow.com/a/77153753/5976009)的 Stack Overflow 答案以及这个[推荐 dafny-reportgenerator 工具](https://github.com/dafny-lang/ide-vscode/discussions/437#discussioncomment-7074719)的 GitHub 讨论。

在找到问题点后，我请来合著者 Divyanshu Ranjan 帮忙。Divyanshu Ranjan 利用他在 Dafny 上的经验来修复项目的验证问题。

这是他的提示，以及来自项目的示例：

## 提示 #1：尽可能移除涉及“forall”和“exists”的 `require` 语句。

回顾规则 4，幽灵函数 `SeqToSet` 返回由排序且不相交的非空范围列表覆盖的整数集合。我们用函数 `ValidSeq` 定义“排序且不相交”，该函数内部使用了两个 `forall` 表达式。我们可以像这样移除列表必须排序和不相交的要求：

```py
ghost function SeqToSet(sequence: seq<NeIntRange>): set<int>
  decreases |sequence|
  // removed: requires ValidSeq(sequence)
{
  if |sequence| == 0 then {}
  else if |sequence| == 1 then RangeToSet(sequence[0])
  else RangeToSet(sequence[0]) + SeqToSet(sequence[1..])
}
```

从我们的角度来看，我们有相同的有用函数。从 Dafny 的角度来看，该函数避免了两个 `forall` 表达式，并且更容易应用。

## 提示 #2 使用 calc 避免 Dafny 的猜测工作。

使用 Dafny `calc` 语句，你列出了得出结论所需的确切步骤。例如，这是 `DeleteExtra` 方法中的一个 `calc`：

```py
calc {
    SeqToSet(xs[..indexAfter+1]) + SeqToSet(xs[indexAfter+1..]);
  ==
    { SeqToSetConcatLemma(xs[..indexAfter+1], xs[indexAfter+1..]); }
    SeqToSet(xs[..indexAfter+1] + xs[indexAfter+1..]);
  ==
    { assert xs == xs[..indexAfter+1] + xs[indexAfter+1..]; }
    SeqToSet(xs);
  ==
    { SetsEqualLemma(xs, r[indexAfter], r2, indexAfter, indexDel); }
    SeqToSet(r2);
  }
```

在代码的这一点，`xs` 是一个范围序列，但它可能不是排序的或不相交的。`calc` 断言：

1.  `xs` 的两部分覆盖的整数等于

1.  两部分拼接覆盖的整数等于

1.  `xs` 覆盖的整数等于

1.  `rs`。

对于每一步，我们可以包含引理或断言来帮助证明这一步骤。例如，这个断言有助于证明从第 3 步到第 4 步的过渡：

```py
{ assert xs == xs[..indexAfter+1] + xs[indexAfter+1..]; }
```

为了提高效率和控制，这些引理和断言在它们的步骤之外对验证器不可见。这使得 Dafny 集中注意力。

## 提示 #3：使用 `timeLimit` 提供所需的计算。

Dafny 在用户设置的 `timeLimit` 下停止尝试验证方法。10、15 或 30 秒的限制很常见，因为作为用户，我们通常希望那些不可能发生的验证能快速失败。然而，如果我们知道验证最终会发生，我们可以设置一个特定于方法的时间限制。例如，Divyanshu Ranjan 发现 `DeleteExtra` 通常会验证，但比其他方法花费更多时间，因此他添加了一个特定于方法的时间限制：

```py
method {:timeLimit 30} DeleteExtra(xs: seq<NeIntRange>, internalRange: IntRange) returns (r: seq<NeIntRange>)
// ...
```

> 附注：`*timeLimit*` 并未考虑计算机之间速度的差异，因此请设置得稍微宽松些。

## 提示 #4：使用 split_here 将验证问题分为两部分。

如[Dafny 常见问题解答](http://dafny.org/dafny/HowToFAQ/FAQSplitHere.html)所述，有时一起验证一组断言更快，有时逐一验证更快。

使用 `assert {:split_here} true;` 语句将一系列断言拆分为两部分以进行验证。例如，即使有 `timeLimit`，`DeleteExtra` 也会超时，直到 Divyanshu Ranjan 添加了这个：

```py
// ...
else
  {
    r := (s[..i] + [pair]) + s[i..];
    assert r[..(i+1)] == s[..i] + [pair];
    assert r[(i+1)..] == s[i..];
    assert {:split_here} true; // split validation into two parts
    calc {
      SeqToSet(r[..(i+1)]) + SeqToSet(r[(i+1)..]);
// ...
```

## 提示 #5：保持引理简小。如有需要，跨引理拆分 `ensures`。

有时引理会试图一次做太多事情。考虑一下 `SetsEqualLemma`。它与删除冗余范围相关。例如，如果我们将 `a` 插入到 `xs` 中，标记为“X”的范围将变得冗余。

![](../Images/7538b2447575a35ce87e121e19e8c9b7.png)

`SetsEqualLemma` 的原始版本包含 12 个 `requires` 和 3 个 `ensures`。Divyanshu Ranjan 将其拆分为两个引理：`RDoesntTouchLemma`（11 个 `requires` 和 2 个 `ensures`）和 `SetsEqualLemma`（3 个 `requires` 和 1 个 `ensures`）。通过这一变化，项目的验证更加可靠。

应用这些技巧将提高我们证明的可靠性。我们能否使验证 100% 可靠？遗憾的是，不能。总有可能由于不幸的种子，Dafny 无法验证。因此，当你什么时候停止尝试改进验证？

在这个项目中，Divyanshu Ranjan 和我改进了验证代码，直到任何单次运行中验证错误的概率降到 33% 以下。因此，在 10 次随机运行中，我们看到的失败不超过 2 或 3 次。我们甚至尝试了 100 次随机运行。在 100 次运行中，我们看到了 30 次失败。

# 结论

所以，您看到了：九条规则来证明 Rust 算法的正确性。您可能会对这个过程不够简单或自动感到沮丧。然而，我反而感到鼓舞，因为这个过程完全是可能的。

> 附注：自高中几何课以来，我发现数学证明既迷人又令人沮丧。 “迷人” 是因为一旦证明的数学定理被认为永远正确。（欧几里得的几何仍被认为是正确的，而亚里士多德的物理学则不是。） “令人沮丧” 是因为我的数学课总是对我可以假设哪些公理以及我的证明可以迈出多大步伐感到模糊。Dafny 和类似的系统通过自动证明检查消除了这种模糊性。从我的角度来看，更好的是，它们帮助我们创建关于我深切关心的领域：算法的证明。

什么时候值得对算法进行正式证明？考虑到所涉及的工作，只有当算法在某种程度上复杂、重要或容易证明时，我才会再次进行这种证明。

未来这个过程可能如何改进？我希望看到：

+   **系统间的互换 —** 一旦证明的几何定理就不需要再证明。我希望检查算法证明的系统能够互相使用对方的证明。

+   **一个像 Dafny 一样易于使用的全 Rust 系统** — 有关这方面的工作，请参见 [[1](https://rust-formal-methods.github.io/tools.html),[2](https://alastairreid.github.io/automatic-rust-verification-tools-2021/)]。

> 附注：你知道有一个易于使用的 Rust 验证系统吗？请考虑将其应用于 `internal_add` 的验证。这将使我们能够比较 Rust 系统的易用性和功能与 Dafny 的。

+   **Rust 的证明类 `**Cargo.lock**` 文件** — 在 Rust 中，我们使用 `Cargo.lock` 来锁定项目依赖的已知良好组合。我希望当 Dafny 找到一种方法来证明，例如，一个方法时，它能锁定找到的证明步骤。这可以使验证更可靠。

+   **更好的 AI 验证** — 我的直觉是，经过一些改进的 ChatGPT 可能擅长创建 90% 需要的验证代码。我发现当前的 ChatGPT 4 在 Dafny 上表现较差，我认为是因为缺乏 Dafny 的训练示例。

+   **更好的 AI 验证** — 当 AI 生成代码时，我们担心代码的正确性。形式化验证可以通过证明正确性来提供帮助。（有关此的小示例，请参见我的文章 [Check AI-Generated Code Perfectly and Automatically](https://medium.com/@carlmkadie/check-ai-generated-code-perfectly-and-automatically-d5b61acff741)。）

感谢你加入我们对程序正确性的探索。我们希望如果你有一个需要证明的算法，这些步骤将帮助你找到该证明。

*请* [*关注 Carl 在 Medium 上*](https://medium.com/@carlmkadie)*。我在 Rust 和 Python 的科学编程、机器学习和统计学方面写作。我倾向于每月写一篇文章。*

*阅读 Divyanshu Ranjan 更多的工作，见* [*他的博客*](https://rdivyanshu.github.io/)*。除了形式化方法，博客还涉及几何、统计学等主题。*
