# 提高软件代码质量的高级 JavaScript 函数

> 原文：[`towardsdatascience.com/advanced-javascript-functions-to-improve-code-quality-2663781ea382`](https://towardsdatascience.com/advanced-javascript-functions-to-improve-code-quality-2663781ea382)

## 通过包括 Debounce、Once 和 Memoize 在内的函数来提高代码质量，以及 Pipe、Pick 和 Zip 等功能

[](https://medium.knulst.de/?source=post_page-----2663781ea382--------------------------------)![Paul Knulst](https://medium.knulst.de/?source=post_page-----2663781ea382--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2663781ea382--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2663781ea382--------------------------------) [Paul Knulst](https://medium.knulst.de/?source=post_page-----2663781ea382--------------------------------)

· 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2663781ea382--------------------------------) · 14 分钟阅读 · 2023 年 2 月 20 日

--

![](img/dff07a46eeeaa2df542233e6e4f7feeb.png)

照片来源 [Joan Gamell](https://unsplash.com/@gamell) / [Unsplash](https://unsplash.com/photos/ZS67i1HLllo)

# 介绍

**JavaScript 是一种强大而多功能的编程语言**，它具有许多内置功能，可以帮助你编写更高效、更易维护和更具可读性的代码。

在这篇博客文章中，我将解释如何使用一些内置功能来创建一些最强大的函数，以**提升你的性能**并使你的代码看起来更美观。我将涵盖 Debounce、Throttle、Once、Memoize、Curry、Partial、Pipe、Compose、Pick、Omit 和 Zip 等内容，你可以将这些函数保存到一个工具文件/类中，以优化你作为 JavaScript 开发者的代码质量。

**尽管这些函数是用 JavaScript 进行解释的，但它们可以轻松地在任何编程语言中实现。一旦理解了不同函数的概念，就可以在任何地方应用。**

此外，本文中描述的函数（或概念）**常常在技术面试中被问到**。

无论你是初学者还是经验丰富的高级开发者，这些函数都将优化你的代码和编程体验。它们会让使用 JavaScript 的过程变得更愉快和高效。

# Debounce

防抖函数是一种防止快速一系列事件重复激活某个函数的方法。它的工作原理是推迟函数执行，直到一段时间过去且事件未被触发。Debounce 函数是一种有用的解决方案，可以应用于实际应用中，通过防止在用户快速点击按钮时执行函数来提高性能。

以下代码片段将展示如何在 JavaScript 中实现防抖函数：

```py
function debounce(func, delay) {
  let timeout;
  return function() {
    const context = this;
    const args = arguments;
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(context, args), delay);
  };
}
```

在这个 JavaScript 代码片段中，`debounce`函数将返回一个新的函数，该函数在先前定义的`delay`之后执行原始函数。如果函数再次被调用，`timeout`将被重置，函数调用将被推迟。

如果你有一个在窗口调整大小时更新网页布局的函数，这个功能会很有用。如果没有 Debounce 函数，这个函数会在用户调整窗口时被多次快速调用，这可能会导致性能问题。使用 Debounce 函数可以限制布局更新的频率，从而使页面更加响应迅速和高效。

这个代码片段展示了如何在这种用例中使用 Debounce 函数：

```py
// Define the function that updates the layout
function updateLayout() {
  // Update the layout...
}

// Create a debounced version of the function
const debouncedUpdateLayout = debounce(updateLayout, 250);

// Listen for window resize events and call the debounced function
window.addEventListener("resize", debouncedUpdateLayout);
```

在这个例子中，当窗口调整大小时，`updateLayout`函数在 250 毫秒内最多会被调用一次。这个功能确保了在用户完成调整窗口大小后 250 毫秒才更新布局，使网页更加高效和响应迅速。

# Throttle

Throttle 函数类似于 Debounce 函数，但行为稍有不同。Throttle 函数限制了函数的*执行*频率，而不是调用频率。这意味着如果函数在给定时间内被调用，则会禁止执行。它保证了某个函数以一致的速度运行，不会触发过于频繁。

Throttle 函数的实现：

```py
function throttle(func, delay) {
  let wait = false;

  return (...args) => {
    if (wait) {
        return;
    }

    func(...args);
    wait = true;
    setTimeout(() => {
      wait = false;
    }, delay);
  }
}
```

在这个代码片段中，`throttle`函数将执行一个提供的函数`func`，将`wait`变量更新为`true`，然后启动一个定时器，在`delay`时间过去后重置`wait`参数。如果`throttle`函数再次被调用，它将调用提供的函数或仅仅返回，如果`wait`参数仍为`true`。

如果你希望在滚动时执行一个更新布局的函数，这个 Throttle 功能可以在网页上使用。如果没有`throttle`函数，这个更新函数会在用户滚动页面时被多次调用，导致严重的性能问题。使用`throttle`函数，你可以确保它每隔 X 毫秒只执行一次。这将导致网页的可用性更具响应性和效率。

在下面的代码片段中，你可以看到如何使用`throttle`函数：

```py
 // Define the function that updates the layout
function updateLayout() {
  // Update the layout...
}

// Create a throttled version of the function
const throttledUpdateLayout = throttle(updateLayout, 250);

// Listen for window scroll events and call the throttled function
window.addEventListener("scroll", throttledUpdateLayout);
```

通过定义`throttleUpdatedLayout`函数并指定 250 毫秒的延迟，可以确保`updateLayout`函数在窗口滚动时最多每 250 毫秒执行一次。如果事件在延迟时间内触发，则不会发生任何事情。

# Once

`Once` 函数是一种方法，它会防止在已经被调用的情况下再次执行。这在处理事件监听器时特别有用，因为你经常会遇到只应该运行一次的函数。与其每次都移除事件监听器，不如在 JavaScript 中使用 `Once` 函数。

```py
function once(func) {
  let ran = false;
  let result;
  return function() {
    if (ran) return result;
    result = func.apply(this, arguments);
    ran = true;
    return result;
  };
}
```

例如，你可以有一个函数向服务器发送请求以加载一些数据。使用 `once()` 函数，你可以确保请求不会被多次调用，如果用户不断点击按钮。这将避免性能问题。

如果没有 `once()` 函数，你需要在请求发送后立即移除点击监听器，以防止请求被再次发送。

将 `once()` 函数应用于任何代码将如下所示：

```py
// Define the function that sends the request
function requestSomeData() {
  // Send the request...
}

// Create a version of the function that can only be called once
const sendRequestOnce = once(sendRequest);

// Listen for clicks on a button and call the "once" function
const button = document.querySelector("button");
button.addEventListener("click", sendRequestOnce);
```

在这个例子中，`requestSomeData` 函数将被调用一次，即使用户多次点击按钮。

# Memoize

Memoize 是一个 JavaScript 函数，用于缓存给定函数的结果，以防止用相同参数多次调用计算开销大的例程。

```py
function memoize(func) {
  const cache = new Map();
  return function() {
    const key = JSON.stringify(arguments);
    if (cache.has(key)) {
        return cache.get(key);
    }
    const result = func.apply(this, arguments);
    cache.set(key, result);
    return result;
  };
}
```

这个 `memoize()` 函数会缓存给定函数的结果，并使用参数作为键来检索结果，如果用相同的参数再次调用它。

现在，如果你有一个基于输入变量执行复杂计算的函数，你可以使用 `memoize()` 函数来缓存结果，并在用相同输入多次调用时立即检索结果。

为了看到 `memoize()` 函数的好处，你可以用它来计算斐波那契数：

```py
// Define the function that performs the calculation
function fibonacci(n) {
    if (n < 2)
        return 1;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Create a memoized version of the function
const memoizedFibonacci = memoize(fibonacci);

// Call the memoized function with multiple input values
console.time('total')
console.time('sub1')
const result1 = memoizedFibonacci(30);
console.timeEnd('sub1')
console.time('sub2')
const result2 = memoizedFibonacci(29);
console.timeEnd('sub2')
console.time('sub3')
const result3 = memoizedFibonacci(30);
console.timeEnd('sub3')
console.timeEnd('total')
```

在这个例子中，`fibonacci()` 函数将被转换为 `memoizedFibonacci` 函数。然后将调用 `memoized()` 函数，并将执行时间记录到控制台。

输出将如下所示：

![](img/f5757ad856da254c0975dc678d3965f9.png)

尽管第二次调用只计算了 29 的斐波那契数，但由于结果被 `memoize()` 函数缓存了，所以它比第二次计算 30 的斐波那契数花费了更长的时间。

# Curry

Curry 函数（也称为柯里化）是一种高级 JavaScript 函数，用于通过“预填充”某些参数从现有函数中创建一个新函数。柯里化通常用于处理多个参数的函数，并将它们转换为接受一些参数的函数，因为其他参数将始终保持不变。

使用 Curry 函数有几个好处：

+   它有助于避免重复使用相同的变量。

+   它使代码更具可读性。

+   它将函数分解为多个较小的函数，每个函数处理一个职责。

```py
function curry(func, arity = func.length) {
  return function curried(...args) {
    if (args.length >= arity) return func(...args);
    return function(...moreArgs) {
      return curried(...args, ...moreArgs);
    };
  };
}
```

这个 Curry 函数接收另一个函数（`func`）和一个可选的参数 `arity`，默认为 `func` 的参数长度。它返回一个新的函数（`curried`），该函数可以用 `arity` 数量的参数调用。如果未提供所有参数，它会返回一个新的函数，该函数可以接受更多参数，直到所有必需的参数都已提供。当所有参数都提供时，会调用原始函数（`func`），并返回其结果。

要理解 Curry 函数的好处，可以考虑一个计算平面上两点之间距离的方法。使用 Curry 函数，你可以创建一个只需要其中一个点的新函数，从而使其更易于使用。

以下代码片段将展示如何使用之前定义的 curry 函数来优化实现的可读性：

```py
// Define the function that calculates the distance between two points
function distance(x1, y1, x2, y2) {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2);
}

// Create a curried version of the function that only requires one of the points
const distanceFromOrigin = curry(distance, 3)(0, 0);

// Call the curried function with the other point
const d1 = distanceFromOrigin(1, 1);
const d2 = distanceFromOrigin(2, 2);
```

在这个例子中，通过使用 `curry` 函数并将 `distance` 作为第一个参数和 `3` 作为第二个参数（`arity`），创建了 `distance` 函数的 curried 版本（`distanceFromOrigin`）。此外，它将使用 `0,0` 作为前两个参数来调用 curried 函数。

结果函数 `distanceFromOrigin` 现在是一个只需要两个参数的新函数，并且将始终使用 `0,0` 作为第一个点。

# Partial

JavaScript 中的 Partial 函数类似于 Curry 函数。Curry 和 Partial 之间的显著区别是，调用 Partial 函数会立即返回结果，而不是返回另一个函数。

```py
function partial(func, ...args) {
  return function partiallyApplied(...moreArgs) {
    return func(...args, ...moreArgs);
  }
}
```

JavaScript 中的 `partial` 函数通常接收一个现有函数、一个或多个输入参数，并返回一个新函数，该新函数在调用时使用附加参数调用原始函数。

在以下用例中，一个 `calculate` 函数将被预填充前两个参数，以生成一个更具可读性的新函数：

```py
// Define the function that calculates something
function calculate(x, y, z) {
 return (x + y) * z
}

// Create a partially applied version of the function the last argument
const multiply10By = partial(calculate, 8, 2);

// Call the partially applied function with the number of iterations
const result = multiply10By(5);
```

在这个例子中，`multiply10By` 函数是通过部分应用通用 `calculate` 函数并将前两个参数预填充为 8 和 2 来创建的。这将创建一个只需要一个参数的新函数 `multiply10By`，该参数指定需要进行多少次乘以 10 的操作。此外，它还将使代码更具可读性和可理解性。

# 管道

Pipe 函数是一个用于链接多个函数并将一个函数的输出传递到链中的下一个函数的工具函数。它类似于 Unix 管道操作符，通过使用 JavaScript 的 `reduce()` 函数将所有函数从左到右应用：

```py
function pipe(...funcs) {
  return function piped(...args) {
    return funcs.reduce((result, func) => [func.call(this, ...result)], args)[0];
  };
}
```

要理解管道函数，假设你有三个函数：

+   给字符串添加前缀

+   给字符串添加后缀

+   将字符串转换为大写

然后，你可以使用管道函数创建一个新函数，该函数将依次从左到右应用于字符串。

```py
// Define the functions that add to the string
function addPrefix(str) {
  return "prefix-" + str;
}

function addSuffix(str) {
  return str + "-suffix";
}

function toUppercase(str) {
    return str.toUpperCase()
}

// Create a piped function that applies the three functions in the correct order
const decorated1 = pipe(addPrefix, addSuffix, toUppercase);
const decorated2 = pipe(toUppercase, addPrefix, addSuffix);

// Call the piped function with the input string
const result1 = decorated1("hello");  // PREFIX-HELLO-SUFFIX
const result2 = decorated2("hello");  // prefix-HELLO-suffix
```

在这个例子中，`decorated1` 和 `decorated2` 函数是通过不同顺序地管道 `addPrefix`、`addSuffix` 和 `toUppercase` 函数创建的。创建的新函数可以用输入字符串调用，以按给定顺序应用原始的三个函数。由于在管道函数中提供的顺序不同，结果输出字符串会有所不同。

# Compose

Compose 函数与 Pipe 函数相同，但它将使用 `reduceRight` 来应用所有函数：

```py
function compose(...funcs) {
  return function composed(...args) {
    return funcs.reduceRight((result, func) => [func.call(this, ...result)], args)[0];
  };
}
```

这将实现相同的功能，但函数从右到左应用。

# Pick

JavaScript 中的 Pick 函数用于从对象中选择特定的值。这是一种通过从提供的对象中选择某些属性来创建新对象的方式。这是一种函数式编程技术，允许从任何对象中提取属性的子集，只要这些属性是可用的。

这是 Pick 函数的实现：

```py
function pick(obj, keys) {
  return keys.reduce((acc, key) => {
    if (obj.hasOwnProperty(key)) {
      acc[key] = obj[key];
    }
    return acc;
  }, {});
}
```

这个函数接受两个参数：

+   `obj`: 从中创建新对象的原始对象

+   `keys`: 选择进入新对象的键数组。

要创建一个新对象，函数将使用 `reduce()` 方法迭代键并将其与原始对象的属性进行比较。如果存在值，它将被添加到 reduce 函数的累加器对象中，该对象初始化为 `{}`。

在 reduce 函数的最后，累加器对象将是新对象，并且只包含 `keys` 数组中的指定属性。

如果你想避免过度提取数据，这个函数很有用。通过 Pick 函数，你可以从数据库中检索任何对象，然后仅 `pick()` 需要的属性并将它们返回给调用者。

```py
const obj = {
    id: 1,
    name: 'Paul',
    password: '82ada72easd7',
    role: 'admin',
    website: 'https://www.paulsblog.dev',
};

const selected = pick(obj, ['name', 'website']);
console.log(selected); // { name: 'Paul', website: 'https://www.paulsblog.dev' }
```

这个函数将使用 `pick()` 函数创建一个仅包含 `name` 和 `website` 的新对象，可以返回给调用者，而不会暴露 `role`、`password` 或 `id`。

# Omit

Omit 函数是 Pick 函数的相反，它将从现有对象中移除某些属性。这意味着你可以通过隐藏属性来避免过度提取。如果隐藏的属性数量少于要选择的属性数量，可以将其作为 Pick 函数的替代品。

```py
function omit(obj, keys) {
  return Object.keys(obj)
    .filter(key => !keys.includes(key))
    .reduce((acc, key) => {
      acc[key] = obj[key];
      return acc;
    }, {});
}
```

这个函数接受两个参数：

+   `obj`: 从中创建新对象的原始对象

+   `keys`: 不会出现在新对象中的键数组。

要创建一个新对象并移除属性，可以使用 `Object.keys()` 函数生成原始对象的键数组。然后，JavaScript 的 `filter()` 函数将移除 `keys` 参数中指定的每一个键。通过 `reduce` 函数，剩余的键将被迭代，返回一个仅包含 `keys` 数组中未提供的每个键的新对象。

在实践中，如果你有一个大的用户对象，只想移除 id，可以使用它：

```py
const obj = {
    id: 1,
    name: 'Paul',
    job: 'Senior Engineer',
    twitter: 'https://www.twitter.com/paulknulst',
    website: 'https://www.paulsblog.dev',
};

const selected = omit(obj, ['id']);
console.log(selected); // {name: 'Paul', job: 'Senior Engineer', twitter: 'https://www.twitter.com/paulknulst', website: 'https://www.paulsblog.dev'}
```

在这个例子中，`omit()` 函数用于移除 `id` 属性并获取一个对象，这样会使你的代码比使用 for 循环、设置 `obj.id = undefined` 或使用 `pick()` 并提供每个要选择的属性更具可读性。

# Zip

Zip 函数是一个 JavaScript 函数，它将每个传入的元素数组与另一个数组元素匹配，用于将多个数组合并为一个包含元组的数组。结果数组将包含来自每个数组的对应元素。通常，当处理需要以某种方式合并或关联的多个来源的数据时，会使用这个功能。

与 Python 不同，JavaScript 默认不提供 Zip 函数。但实现起来很简单：

```py
function zip(...arrays) {
  const maxLength = Math.max(...arrays.map(array => array.length));
  return Array.from({ length: maxLength }).map((_, i) => {
    return Array.from({ length: arrays.length }, (_, j) => arrays[j][i]);
  });
}
```

这个 JavaScript 片段将创建一个新数组的数组，其中每个子数组由提供的数组的元素组成。这意味着，原始数组的每个元素将映射到同一索引的另一个原始数组的元素。

例如，你可以有三个数组，它们：

1.  包含 x 坐标

1.  包含 y 坐标

1.  包含 z 坐标

没有 zip 函数，你需要手动遍历数组并配对 x、y 和 z 元素。但通过使用 zip 函数，你可以传递原始数组并生成一个新的 (x, y, z) 元组数组。

```py
// Define the arrays that contain the coordinates
const xCoordinates = [1, 2, 3, 4];
const yCoordinates = [5, 6, 7, 8];
const zCoordinates = [3, 6, 1, 7];

// Create a zipped array of points
const points = zip(xCoordinates, yCoordinates, zCoordinates);

// Use the zipped array of points
console.log(points);  // [[1, 5, 3], [2, 6, 6], [3, 7, 1], [4, 8, 7]]
```

在这个例子中，`zip` 函数用于将 `xCoordinates`、`yCoordinates` 和 `zCoordinates` 数组合并为一个包含元组的单一数组。

# 结束语

在这篇博客文章中，我介绍了许多强大而有用的函数，这些函数有助于编写更高效、可读和可维护的 JavaScript 代码。如果正确应用，这些函数将提升你的代码质量，并使项目的工作更加轻松。

重要的是要说明，解释的 JavaScript 函数不是核心 JavaScript 语言的一部分，而是在几个流行的 JavaScript 框架中实现的，如 **underscore.js**、**lodash** 等。

最后，学习在实际软件项目中有效使用这些函数是一个持续的过程，需要练习。但过了一段时间，你将能轻松高效地使用这些函数，使你的代码更具可读性和可维护性。此外，**整体代码质量将得到优化**。

最后，你对这些 JavaScript 函数有什么看法？你是否迫不及待地想将它们应用到你的项目中？另外，你对这些函数有任何疑问吗？我很乐意听取你的想法并回答你的问题。请在评论中分享一切。

随时通过 [我的博客](https://www.paulsblog.dev)、[LinkedIn](https://www.linkedin.com/in/paulknulst/)、[Twitter](https://twitter.com/paulknulst) 和 [GitHub](https://github.com/paulknulst) 与我联系。

感谢阅读，**祝编码愉快！**

*这篇文章最初发表于我的博客* [*https://www.paulsblog.dev/advanced-javascript-functions-to-improve-code-quality/*](https://www.paulsblog.dev/advanced-javascript-functions-to-improve-code-quality/)
