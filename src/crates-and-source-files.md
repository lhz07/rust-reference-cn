r[crate]
# 包和源文件

r[crate.syntax]
```grammar,items
@root Crate ->
    InnerAttribute*
    Item*
```

> [!NOTE]
> 虽然 Rust 像任何其他语言一样，可以通过解释器和编译器实现，但唯一现有的实现是编译器，并且该语言一直被设计为编译的。因此，本节假定使用编译器。

r[crate.compile-time]
Rust 的语义遵循编译时和运行时之间的*阶段区分*。[^phase-distinction] 具有*静态解释*的语义规则管理编译的成功或失败，而具有*动态解释*的语义规则管理程序在运行时的行为。

r[crate.unit]
编译模型以称为_包_ (crates) 的工件为中心。每次编译处理源形式的单个包，如果成功，则生成二进制形式的单个包：可执行文件或某种类型的库。[^cratesourcefile]

r[crate.module]
_包_是编译和链接、版本控制、分发和运行时加载的单元。包包含嵌套[模块][module]作用域的_树_。该树的顶层是一个匿名模块（从模块内的路径的角度来看），包内的任何条目都有一个[规范模块路径][module path]，表示其在包的模块树中的位置。

r[crate.input-source]
Rust 编译器始终以单个源文件作为输入调用，并始终生成单个输出包。该源文件的处理可能会导致其他源文件作为模块加载。源文件的扩展名为 `.rs`。

r[crate.module-def]
Rust 源文件描述一个模块，其名称和位置 &mdash; 在当前包的模块树中 &mdash; 从源文件外部定义：通过引用源文件中的显式 [Module][grammar-Module] 条目，或者通过包本身的名称。

r[crate.inline-module]
每个源文件都是一个模块，但并非每个模块都需要自己的源文件：[模块定义][module]可以嵌套在一个文件中。

r[crate.items]
每个源文件包含零个或多个 [Item] 定义的序列，并且可以选择以任意数量的应用于包含模块的[属性][attributes]开头，其中大多数会影响编译器的行为。

r[crate.attributes]
匿名包模块可以具有应用于整个包的其他属性。

> [!NOTE]
> 文件的内容可以以 [shebang] 开头。

```rust
// 指定包名称。
#![crate_name = "projx"]

// 指定输出工件的类型。
#![crate_type = "lib"]

// 打开警告。
// 这可以在任何模块中完成，而不仅仅是匿名包模块。
#![warn(non_camel_case_types)]
```

r[crate.main]
## Main 函数

r[crate.main.general]
包含 `main` [函数][function]的包可以编译为可执行文件。

r[crate.main.restriction]
如果存在 `main` 函数，它必须不接受参数，不得声明任何[特征或生命周期约束][trait or lifetime bounds]，不得有任何 [where 子句][where clauses]，并且其返回类型必须实现 [`Termination`] 特征。

```rust
fn main() {}
```
```rust
fn main() -> ! {
    std::process::exit(0);
}
```
```rust
fn main() -> impl std::process::Termination {
    std::process::ExitCode::SUCCESS
}
```

r[crate.main.import]
`main` 函数可以是导入，例如来自外部包或当前包。

```rust
mod foo {
    pub fn bar() {
        println!("Hello, world!");
    }
}
use foo::bar as main;
```

> [!NOTE]
> 标准库中实现 [`Termination`] 的类型包括：
>
> * `()`
> * [`!`]
> * [`Infallible`]
> * [`ExitCode`]
> * `Result<T, E> where T: Termination, E: Debug`

<!-- 如果前面的部分需要更新（从"必须不接受参数"开始），也在 testing.md 文件中更新它 -->

r[crate.uncaught-foreign-unwinding]
### 未捕获的外部展开

当"外部"展开（例如，从 C++ 代码抛出的异常，或使用不同恐慌处理器的 Rust 代码中的 `panic!`）传播到 `main` 函数之外时，进程将被安全终止。这可能采取中止的形式，在这种情况下，不能保证执行任何 `Drop` 调用，并且错误输出可能不如运行时被"原生" Rust `panic` 终止时那样有信息。

有关更多信息，请参阅[恐慌文档][panic-docs]。

r[crate.no_main]
### `no_main` 属性

*`no_main` [属性][attribute]*可以应用于包级别，以禁用为可执行二进制文件发出 `main` 符号。当链接的其他某个对象定义 `main` 时，这很有用。

r[crate.crate_name]
## `crate_name` 属性

r[crate.crate_name.general]
*`crate_name` [属性][attribute]*可以应用于包级别，以使用 [MetaNameValueStr] 语法指定包的名称。

```rust
#![crate_name = "mycrate"]
```

r[crate.crate_name.restriction]
包名称不能为空，并且只能包含 [Unicode 字母数字][Unicode alphanumeric]或 `_` (U+005F) 字符。

[^phase-distinction]: 这种区分在解释器中也存在。无论何时执行程序，语法分析、类型检查和检查等静态检查都应在程序执行之前进行。

[^cratesourcefile]: 包在某种程度上类似于 ECMA-335 CLI 模型中的*程序集*、SML/NJ 编译管理器中的*库*、Owens 和 Flatt 模块系统中的*单元*或 Mesa 中的*配置*。

[Unicode alphanumeric]: char::is_alphanumeric
[`!`]: types/never.md
[`ExitCode`]: std::process::ExitCode
[`Infallible`]: std::convert::Infallible
[`Termination`]: std::process::Termination
[attribute]: attributes.md
[attributes]: attributes.md
[function]: items/functions.md
[module]: items/modules.md
[module path]: paths.md
[panic-docs]: panic.md#unwinding-across-ffi-boundaries
[shebang]: input-format.md#shebang-removal
[trait or lifetime bounds]: trait-bounds.md
[where clauses]: items/generics.md#where-clauses
