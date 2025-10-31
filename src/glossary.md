# 术语表

### 抽象语法树（Abstract syntax tree）

An ‘abstract syntax tree’, or ‘AST’, is an intermediate representation of
the structure of the program when the compiler is compiling it.

### 对齐（Alignment）

值的对齐指定了值首选从哪些地址开始。始终是 2 的幂。对值的引用必须是对齐的。
[更多][alignment]。

r[glossary.abi]
### 应用程序二进制接口（Application binary interface，ABI）

*应用程序二进制接口*（ABI）定义了编译后的代码如何与其他编译后的代码交互。使用 [`extern` 块][`extern` blocks]和 [`extern fn`]，*ABI 字符串*会影响：

- **调用约定**：如何传递函数参数、如何返回值（例如，在寄存器中或在栈上），以及谁负责清理栈。
- **展开**：是否允许栈展开。例如，`"C-unwind"` ABI 允许跨 FFI 边界展开，而 `"C"` ABI 则不允许。

### 元数（Arity）

元数指函数或运算符接受的参数数量。
例如，`f(2, 3)` 和 `g(4, 6)` 的元数为 2，而 `h(8, 2, 6)` 的元数为 3。`!` 运算符的元数为 1。

### 数组（Array）

数组，有时也称为固定大小数组或内联数组，是描述元素集合的值，
每个元素由程序在运行时可以计算的索引选择。它占用一块连续的内存区域。

### 关联项（Associated item）

关联项是与另一个项关联的项。关联项在[实现][implementations]中定义，在 [trait][traits] 中声明。
只有函数、常量和类型别名可以关联。与[自由项][free item]对比。

### 覆盖实现（Blanket implementation）

任何类型出现[未覆盖](#uncovered-type)的实现。`impl<T> Foo for T`、`impl<T> Bar<T> for T`、
`impl<T> Bar<Vec<T>> for T` 和 `impl<T> Bar<T> for Vec<T>` 被视为覆盖实现。
但是，`impl<T> Bar<Vec<T>> for Vec<T>` 不是覆盖实现，因为此 `impl` 中出现的所有 `T` 实例都被 `Vec` 覆盖。

### 约束（Bound）

约束是对类型或 trait 的限制。例如，如果对函数接受的参数施加了约束，
则传递给该函数的类型必须遵守该约束。

### 组合器（Combinator）

组合器是高阶函数，仅应用函数和先前定义的组合器来从其参数提供结果。
它们可用于以模块化方式管理控制流。

### Crate

Crate 是编译和链接的单元。有不同的 [crate 类型][types of crates]，例如库或可执行文件。
Crate 可以链接并引用其他库 crate，称为外部 crate。一个 crate 具有自包含的[模块][modules]树，
从名为 crate 根的未命名根模块开始。通过在 crate 根中将[条目][Items]标记为公开（包括通过公共模块的[路径][paths]），
可以使条目对其他 crate 可见。[更多][crate]。

### 派发（Dispatch）

派发是在涉及多态时确定实际运行哪个特定代码版本的机制。两种主要的派发形式是静态派发和动态派发。
Rust 通过使用 [trait 对象][type.trait-object]支持动态派发。

### 动态大小类型（Dynamically sized type）

动态大小类型（DST）是没有静态已知大小或对齐的类型。

### 实体（Entity）

[*实体*]是可以在源程序中以某种方式引用的语言结构，通常通过[路径][paths]。
实体包括[类型][types]、[条目][items]、[泛型参数][generic parameters]、[变量绑定][variable bindings]、
[循环标签][loop labels]、[生命周期][lifetimes]、[字段][fields]、[属性][attributes]和 [lint][lints]。

### 表达式（Expression）

表达式是值、常量、变量、运算符和函数的组合，它们计算为单个值，带或不带副作用。

例如，`2 + (3 * 4)` 是一个返回值 14 的表达式。

### 自由项（Free item）

不是[实现][implementation]成员的[条目][item]，例如*自由函数*或*自由常量*。
与[关联项][associated item]对比。

### 基础 trait（Fundamental traits）

基础 trait 是为现有类型添加其实现会导致破坏性更改的 trait。
`Fn` trait 和 `Sized` 是基础 trait。

### 基础类型构造器（Fundamental type constructors）

基础类型构造器是在其上实现[覆盖实现](#blanket-implementation)会导致破坏性更改的类型。
`&`、`&mut`、`Box` 和 `Pin` 是基础类型构造器。

任何时候类型 `T` 被视为[本地](#local-type)，`&T`、`&mut T`、`Box<T>` 和 `Pin<T>` 也被视为本地。
基础类型构造器不能[覆盖](#uncovered-type)其他类型。
任何时候使用术语"覆盖类型"时，`&T`、`&mut T`、`Box<T>` 和 `Pin<T>` 中的 `T` 不被视为覆盖。

### 有值（Inhabited）

如果类型有构造器并因此可以实例化，则该类型是有值的。有值类型在某种意义上不是"空的"，
因为可以有该类型的值。与[无值](#uninhabited)相反。

### 固有实现（Inherent implementation）

应用于名义类型而不是 trait-类型对的[实现][implementation]。
[更多][inherent implementation]。

### 固有方法（Inherent method）

在[固有实现][inherent implementation]中定义的[方法][method]，而不是在 trait 实现中定义的方法。

### 已初始化（Initialized）

如果变量已被赋值且此后未被移动，则该变量已初始化。所有其他内存位置都假定为未初始化。
只有不安全 Rust 可以创建未初始化的内存位置。

### 本地 trait（Local trait）

在当前 crate 中定义的 `trait`。trait 定义是否本地与应用的类型参数无关。
给定 `trait Foo<T, U>`，`Foo` 始终是本地的，无论为 `T` 和 `U` 替换了什么类型。

### 本地类型（Local type）

在当前 crate 中定义的 `struct`、`enum` 或 `union`。
这不受应用的类型参数的影响。`struct Foo` 被视为本地，但 `Vec<Foo>` 不是。
`LocalType<ForeignType>` 是本地的。类型别名不影响本地性。

### 模块（Module）

模块是零个或多个[条目][items]的容器。模块组织成树，从根处名为 crate 根或根模块的未命名模块开始。
[路径][Paths]可用于引用来自其他模块的条目，这可能受[可见性规则][visibility rules]的限制。
[更多][modules]

### 名称（Name）

[*名称*]是引用[实体](#entity)的[标识符][identifier]或[生命周期或循环标签][lifetime or loop label]。
*名称绑定*是当实体声明引入与该实体关联的标识符或标签时。[路径][Paths]、标识符和标签用于引用实体。

### 名称（Name） resolution

[*名称解析*]是将[路径][paths]、[标识符][identifiers]和[标签][labels]绑定到[实体](#entity)声明的编译时过程。

### 名称（Name）space

*命名空间*是基于名称引用的[实体](#entity)类型对声明的[名称](#name)的逻辑分组。
命名空间允许一个命名空间中的名称出现不与另一个命名空间中的相同名称冲突。

在命名空间内，名称按层次结构组织，层次结构的每个级别都有自己的命名实体集合。

### 名义类型（Nominal types）

可以直接通过路径引用的类型。具体包括[枚举][enums]、[结构体][structs]、[联合体][unions]和 [trait 对象类型][trait object types]。

### Dyn 兼容 trait（Dyn-compatible traits）

可以在 [trait 对象类型][trait object types]（`dyn Trait`）中使用的 [trait][Traits]。
只有遵循特定[规则][dyn compatibility]的 trait 是 *dyn 兼容*的。

这些以前被称为*对象安全* trait。

### 路径（Path）

[*路径*]是一个或多个路径段的序列，用于引用当前作用域或[命名空间](#namespace)层次结构的其他级别中的[实体](#entity)。

### 前导（Prelude）

前导（Prelude），或 Rust 前导，是一小部分条目集合（主要是 trait），
它们被导入到每个 crate 的每个模块中。前导中的 trait 是普遍存在的。

### 作用域（Scope）

[*作用域*]是源文本的区域，在其中可以使用该名称引用命名的[实体](#entity)。

### 被匹配项（Scrutinee）

被匹配项是在 `match` 表达式和类似的模式匹配结构中被匹配的表达式。
例如，在 `match x { A => 1, B => 2 }` 中，表达式 `x` 是被匹配项。

### 大小（Size）

值的大小有两个定义。

第一个是必须分配多少内存来存储该值。

第二个是具有该项类型的数组中连续元素之间的偏移量（以字节为单位）。

它是对齐的倍数，包括零。大小可能会根据编译器版本（随着新优化的进行）和目标平台（类似于 `usize` 因平台而异）而变化。

[更多][alignment]。

### 切片（Slice）

切片是对连续序列的动态大小视图，写作 `[T]`。

它通常以其借用形式出现，可变或共享。共享切片类型是 `&[T]`，而可变切片类型是 `&mut [T]`，
其中 `T` 表示元素类型。

### 语句（Statement）

语句是编程语言的最小独立元素，命令计算机执行操作。

### 字符串字面量（String literal）

字符串字面量是直接存储在最终二进制文件中的字符串，因此对于 `'static` 持续时间有效。

其类型是 `'static` 持续时间借用字符串切片，`&'static str`。

### 字符串切片（String slice）

字符串切片是 Rust 中最原始的字符串类型，写作 `str`。它通常以其借用形式出现，可变或共享。
共享字符串切片类型是 `&str`，而可变字符串切片类型是 `&mut str`。

字符串切片始终是有效的 UTF-8。

### Trait

Trait 是用于描述类型必须提供的功能的语言项。
它允许类型对其行为做出某些承诺。

泛型函数和泛型结构可以使用 trait 来约束或限制它们接受的类型。

### Turbofish

表达式中带有泛型参数的路径必须在左尖括号前加上 `::`。
结合泛型的尖括号，这看起来像一条鱼 `::<>`。
因此，这种语法通俗地称为 turbofish 语法。

示例：

```rust
let ok_num = Ok::<_, ()>(5);
let vec = [1, 2, 3].iter().map(|n| n * 2).collect::<Vec<_>>();
```

这个 `::` 前缀是必需的，以消除泛型路径与逗号分隔列表中的多个比较的歧义。
请参阅 [turbofish 堡垒][turbofish test]以获取没有前缀会产生歧义的示例。

### 未覆盖类型（Uncovered type）

不作为另一个类型的参数出现的类型。例如，`T` 是未覆盖的，但 `Vec<T>` 中的 `T` 是覆盖的。
这仅与类型参数相关。

### 未定义行为（Undefined behavior）

未指定的编译时或运行时行为。这可能导致但不限于：进程终止或损坏；不当、不正确或意外的计算；或平台特定的结果。
[更多][undefined-behavior]。

### 无值（Uninhabited）

如果类型没有构造器并因此永远无法实例化，则该类型是无值的。无值类型在某种意义上是"空的"，
因为没有该类型的值。无值类型的典型示例是 [never 类型][never type] `!`，或者没有变体的枚举 `enum Never { }`。
与[有值](#inhabited)相反。

[`extern` blocks]: items.extern
[`extern fn`]: items.fn.extern
[alignment]: type-layout.md#size-and-alignment
[associated item]: #associated-item
[attributes]: attributes.md
[*entity*]: names.md
[crate]: crates-and-source-files.md
[dyn compatibility]: items/traits.md#dyn-compatibility
[enums]: items/enumerations.md
[fields]: expressions/field-expr.md
[free item]: #free-item
[generic parameters]: items/generics.md
[identifier]: identifiers.md
[identifiers]: identifiers.md
[implementation]: items/implementations.md
[implementations]: items/implementations.md
[inherent implementation]: items/implementations.md#inherent-implementations
[item]: items.md
[items]: items.md
[labels]: tokens.md#lifetimes-and-loop-labels
[lifetime or loop label]: tokens.md#lifetimes-and-loop-labels
[lifetimes]: tokens.md#lifetimes-and-loop-labels
[lints]: attributes/diagnostics.md#lint-check-attributes
[loop labels]: tokens.md#lifetimes-and-loop-labels
[method]: items/associated-items.md#methods
[modules]: items/modules.md
[*Name resolution*]: names/name-resolution.md
[*name*]: names.md
[*namespace*]: names/namespaces.md
[never type]: types/never.md
[*path*]: paths.md
[Paths]: paths.md
[*scope*]: names/scopes.md
[structs]: items/structs.md
[trait object types]: types/trait-object.md
[traits]: items/traits.md
[turbofish test]: https://github.com/rust-lang/rust/blob/1.58.0/src/test/ui/parser/bastion-of-the-turbofish.rs
[types of crates]: linkage.md
[types]: types.md
[undefined-behavior]: behavior-considered-undefined.md
[unions]: items/unions.md
[variable bindings]: patterns.md
[visibility rules]: visibility-and-privacy.md
