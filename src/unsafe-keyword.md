r[unsafe]
# `unsafe` 关键字

r[unsafe.intro]
`unsafe` 关键字用于创建或解除证明某些内容安全的义务。具体来说：

- 它用于标记*定义*额外安全条件的代码，这些条件必须在其他地方得到维护。
  - 这包括 `unsafe fn`、`unsafe static` 和 `unsafe trait`。
- 它用于标记程序员*断言*满足在其他地方定义的安全条件的代码。
  - 这包括 `unsafe {}`、`unsafe impl`、没有 [`unsafe_op_in_unsafe_fn`] 的 `unsafe fn`、`unsafe extern` 和 `#[unsafe(attr)]`。

以下讨论每种情况。
有关一些示例，请参见[关键字文档][keyword]。

r[unsafe.positions]
`unsafe` 关键字可以出现在几种不同的上下文中：

- unsafe 函数（`unsafe fn`）
- unsafe 块（`unsafe {}`）
- unsafe trait（`unsafe trait`）
- unsafe trait 实现（`unsafe impl`）
- unsafe 外部块（`unsafe extern`）
- unsafe 外部静态项（`unsafe static`）
- unsafe 属性（`#[unsafe(attr)]`）

r[unsafe.fn]
## Unsafe 函数（`unsafe fn`）

r[unsafe.fn.intro]
Unsafe 函数是在所有上下文和/或所有可能的输入下都不安全的函数。
我们说它们有*额外的安全条件*，这是所有调用者必须维护的要求，而编译器不会检查。
例如，[`get_unchecked`] 有额外的安全条件，即索引必须在边界内。
unsafe 函数应该附带文档，解释这些额外的安全条件是什么。

r[unsafe.fn.safety]
这样的函数必须以关键字 `unsafe` 为前缀，并且只能从 `unsafe` 块内部调用，或从没有 [`unsafe_op_in_unsafe_fn`] lint 的 `unsafe fn` 内部调用。

r[unsafe.block]
## Unsafe 块（`unsafe {}`）

r[unsafe.block.intro]
可以使用 `unsafe` 关键字为代码块添加前缀，以允许使用[不安全性][Unsafety]章节中定义的不安全操作，例如调用其他 unsafe 函数或解引用裸指针。

r[unsafe.block.fn-body]
默认情况下，unsafe 函数的主体也被视为 unsafe 块；
这可以通过启用 [`unsafe_op_in_unsafe_fn`] lint 来更改。

通过将操作放入 unsafe 块中，程序员声明他们已经负责满足该块内所有操作的额外安全条件。

Unsafe 块是 unsafe 函数的逻辑对偶：
unsafe 函数定义了调用者必须维护的证明义务，而 unsafe 块声明在块内调用的函数或操作的所有相关证明义务都已解除。
解除证明义务有多种方法；
例如，可能有运行时检查或数据结构不变量来保证某些属性一定为真，或者 unsafe 块可能在 `unsafe fn` 内部，在这种情况下，块可以使用该函数的证明义务来解除块内产生的证明义务。

Unsafe 块用于包装外部库、直接使用硬件或实现语言中不直接存在的功能。
例如，Rust 提供了在语言中实现内存安全并发所需的语言功能，但标准库中线程和消息传递的实现使用 unsafe 块。

Rust 的类型系统是动态安全要求的保守近似，因此在某些情况下使用安全代码会有性能成本。
例如，双向链表不是树结构，在安全代码中只能用引用计数指针表示。
通过使用 `unsafe` 块将反向链接表示为裸指针，可以在不使用引用计数的情况下实现它。
（有关此特定示例的更深入探讨，请参见["通过太多链表学习 Rust"](https://rust-unofficial.github.io/too-many-lists/)。）

[Unsafety]: unsafety.md

r[unsafe.trait]
## Unsafe trait（`unsafe trait`）

r[unsafe.trait.intro]
Unsafe trait 是带有额外安全条件的 trait，这些条件必须由 trait 的*实现*来维护。
unsafe trait 应该附带文档，解释这些额外的安全条件是什么。

r[unsafe.trait.safety]
这样的 trait 必须以关键字 `unsafe` 为前缀，并且只能通过 `unsafe impl` 块实现。

r[unsafe.impl]
## Unsafe trait 实现（`unsafe impl`）

在实现 unsafe trait 时，实现需要以 `unsafe` 关键字为前缀。
通过编写 `unsafe impl`，程序员声明他们已经负责满足 trait 要求的额外安全条件。

Unsafe trait 实现是 unsafe trait 的逻辑对偶：unsafe trait 定义了实现必须维护的证明义务，而 unsafe 实现声明所有相关的证明义务都已解除。

[keyword]: ../std/keyword.unsafe.html
[`get_unchecked`]: slice::get_unchecked
[`unsafe_op_in_unsafe_fn`]: ../rustc/lints/listing/allowed-by-default.html#unsafe-op-in-unsafe-fn

r[unsafe.extern]
## Unsafe 外部块（`unsafe extern`）

声明[外部块][external block]的程序员必须确保其中包含的项的签名是正确的。如果不这样做可能会导致未定义行为。通过编写 `unsafe extern` 来表示已满足此义务。

r[unsafe.extern.edition2024]
> [!EDITION-2024]
> 在 2024 版之前，允许 `extern` 块而无需限定为 `unsafe`。

[external block]: items/external-blocks.md

r[unsafe.attribute]
## Unsafe 属性（`#[unsafe(attr)]`）

[Unsafe 属性][unsafe attribute]是在使用属性时必须维护额外安全条件的属性。编译器无法检查这些条件是否得到维护。要断言它们已被维护，这些属性必须包装在 `unsafe(..)` 中，例如 `#[unsafe(no_mangle)]`。

[unsafe attribute]: attributes.md
