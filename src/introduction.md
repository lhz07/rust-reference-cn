# 简介

本书是 Rust 编程语言的主要参考文档。

> [!NOTE]
> 关于本书已知的错误和遗漏，请参阅我们的 [GitHub issues]。如果你发现编译器行为和本书内容不一致的情况，请提交问题，以便我们考虑哪个是正确的。

## Rust 版本发布

Rust 每六周发布一次新的语言版本。
语言的第一个稳定版本是 Rust 1.0.0，之后是 Rust 1.1.0，依此类推。
工具（`rustc`、`cargo` 等）和文档（[标准库]、本书等）随着语言版本一起发布。

本书的最新版本，与最新 Rust 版本相匹配，可以在 <https://doc.rust-lang.org/reference/> 找到。
旧版本可以通过在 "reference" 目录前添加 Rust 版本号来找到。
例如，Rust 1.49.0 的参考手册位于 <https://doc.rust-lang.org/1.49.0/reference/>。

## *参考手册*不是什么

本书不是语言的入门教程。
阅读本书需要对语言有一定的背景知识。
另有一本单独的[入门书][book]可以帮助你获得这些背景知识。

本书也不是语言发行版中包含的[标准库]的参考文档。
这些库的文档是通过从其源代码中提取文档属性而单独生成的。
许多你可能期望的语言特性在 Rust 中实际上是库特性，所以你要找的内容可能在那里，而不是这里。

类似地，本书通常不会记录 `rustc` 工具或 Cargo 的具体细节。
`rustc` 有自己的[文档][rustc book]。
Cargo 有一本包含[参考手册][cargo reference]的[文档][cargo book]。
不过仍有一些页面（如[链接][linkage]）描述了 `rustc` 的工作方式。

本书也仅作为稳定版 Rust 可用功能的参考。
有关正在开发的不稳定特性，请参阅[不稳定特性手册][Unstable Book]。

包括 `rustc` 在内的 Rust 编译器会执行优化。
本参考手册不会指定允许或禁止哪些优化。
相反，应该将编译后的程序视为黑盒。
你只能通过运行它、输入数据并观察输出来探究它。
所有以这种方式发生的事情都必须符合参考手册的规定。

## 如何使用本书

本书不假设你按顺序阅读。
每章通常可以独立阅读，但会交叉链接到它们引用但不讨论的语言其他方面的章节。

阅读本文档有两种主要方式。

第一种是回答特定问题。
如果你知道哪一章回答了该问题，可以在目录中跳转到该章。
否则，你可以按 `s` 键或点击顶部栏的放大镜来搜索与问题相关的关键字。
例如，假设你想知道 let 语句中创建的临时值何时被销毁。
如果你不知道[临时值的生命周期][lifetime of temporaries]在[表达式章节][expressions chapter]中定义，你可以搜索 "temporary let"，第一个搜索结果会带你到该部分。

第二种是全面提高你对语言某个方面的知识。
在这种情况下，只需浏览目录，直到看到你想了解更多的内容，然后开始阅读。
如果某个链接看起来有趣，点击它，阅读该部分。

也就是说，阅读本书没有错误的方式。以你认为最有帮助的方式阅读即可。

### 约定

像所有技术书籍一样，本书在显示信息方面有某些约定。
这些约定记录在此处。

* 定义术语的语句中该术语以*斜体*显示。
  当该术语在该章节之外使用时，通常是指向包含此定义的部分的链接。

  一个*示例术语*是一个正在定义的术语的示例。

* 主要文本描述最新的稳定版本。与之前版本的差异在版本块中分隔：

  > [!EDITION-2018]
  > 在 2018 版本之前，行为是这样的。从 2018 版本开始，行为是那样的。

* 包含有关本书状态的有用信息或指出有用但大多超出范围的信息的注释在注释块中：

  > [!NOTE]
  > 这是一个示例注释。

* 示例块显示演示某些规则或指出某些有趣方面的示例。一些示例可能有隐藏的行，可以通过点击悬停或点击示例时出现的眼睛图标来查看。

  > [!EXAMPLE]
  > 这是一个代码示例。
  > ```rust
  > println!("hello world");
  > ```

* 显示语言中不健全行为或语言特性可能令人困惑的交互的警告在特殊警告框中：

  > [!WARNING]
  > 这是一个示例警告。

* 文本中的内联代码片段在 `<code>` 标签内。

  较长的代码示例在语法高亮框中，右上角有复制、执行和显示隐藏行的控件。

  ```rust
  # // 这是一个隐藏的行。
  fn main() {
      println!("这是一个代码示例");
  }
  ```

  除非另有说明，所有示例都是为最新版本编写的。

* 语法和词法产生式在[符号][Notation]章节中描述。

r[example.rule.label]
* 规则标识符出现在每个语言规则之前，用方括号括起来。这些标识符提供了一种引用和链接到语言中特定规则的方法（[例如][example rule]）。规则标识符使用句点从最一般到最具体分隔部分（例如 [destructors.scope.nesting.function-body]）。在窄屏幕上，规则名称将折叠以显示 `[*]`。

  可以点击规则名称链接到该规则。

  > [!WARNING]
  > 规则的组织目前在不断变化中。暂时，这些标识符名称在版本之间不稳定，如果被更改，指向这些规则的链接可能会失效。我们打算在组织稳定后稳定这些名称，以便指向规则名称的链接在版本之间不会中断。

* 具有关联测试的规则将在其下方包含一个 `Tests` 链接（在窄屏幕上，链接为 `[T]`）。点击链接将弹出测试列表，可以点击查看测试。例如，参见 [input.encoding.utf8]。

  将规则链接到测试是一项正在进行的工作。有关概述，请参阅[测试摘要](test-summary.md)章节。

## 贡献

我们欢迎各种形式的贡献。

你可以通过在[Rust 参考手册仓库][the Rust Reference repository]中提出问题或发送拉取请求来为本书做出贡献。
如果本书没有回答你的问题，并且你认为答案在其范围内，请不要犹豫[提交问题][file an issue]或在 [Zulip] 的 `t-lang/doc` 流中询问。
了解人们最常使用本书的用途有助于我们将注意力集中在使这些部分尽可能完善上。
当然，如果你看到任何错误或非规范性的内容但没有特别指出，请也[提交问题][file an issue]。

[book]: ../book/index.html
[GitHub issues]: https://github.com/rust-lang/reference/issues
[标准库]: std
[the Rust Reference repository]: https://github.com/rust-lang/reference/
[Unstable Book]: https://doc.rust-lang.org/nightly/unstable-book/
[cargo book]: ../cargo/index.html
[cargo reference]: ../cargo/reference/index.html
[example rule]: example.rule.label
[expressions chapter]: expressions.html
[file an issue]: https://github.com/rust-lang/reference/issues
[lifetime of temporaries]: expressions.html#temporaries
[linkage]: linkage.html
[rustc book]: ../rustc/index.html
[Notation]: notation.md
[Zulip]: https://rust-lang.zulipchat.com/#narrow/stream/237824-t-lang.2Fdoc
