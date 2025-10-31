r[panic]
# 恐慌

r[panic.intro]
Rust 提供了一种机制来阻止函数正常返回，而是"恐慌"（panic），这是对通常不期望在遇到错误的上下文中可恢复的错误条件的响应。

r[panic.lang-ops]
某些语言构造，例如越界[数组索引][array indexing]，会自动恐慌。

r[panic.control]
还有一些语言特性提供对恐慌行为的一定程度的控制：

* [_恐慌处理器_][panic handler]定义恐慌的行为。
* [FFI ABI](items/functions.md#unwinding) 可能会改变恐慌的行为方式。

> [!NOTE]
> 标准库提供了通过 [`panic!` 宏][panic!]显式恐慌的能力。

r[panic.panic_handler]
## `panic_handler` 属性

r[panic.panic_handler.intro]
*`panic_handler` 属性*可以应用于函数以定义恐慌的行为。

r[panic.panic_handler.allowed-positions]
`panic_handler` 属性只能应用于签名为 `fn(&PanicInfo) -> !` 的函数。

> [!NOTE]
> [`PanicInfo`] 结构包含有关恐慌位置的信息。

r[panic.panic_handler.unique]
依赖图中必须有一个 `panic_handler` 函数。

下面显示了一个 `panic_handler` 函数，它记录恐慌消息，然后停止线程。

<!-- ignore: test infrastructure can't handle no_std -->
```rust,ignore
#![no_std]

use core::fmt::{self, Write};
use core::panic::PanicInfo;

struct Sink {
    // ..
#    _0: (),
}
#
# impl Sink {
#     fn new() -> Sink { Sink { _0: () }}
# }
#
# impl fmt::Write for Sink {
#     fn write_str(&mut self, _: &str) -> fmt::Result { Ok(()) }
# }

#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    let mut sink = Sink::new();

    // 将 "panicked at '$reason', src/main.rs:27:4" 记录到某个 `sink`
    let _ = writeln!(sink, "{}", info);

    loop {}
}
```

r[panic.panic_handler.std]
### 标准行为

r[panic.panic_handler.std.kinds]
`std` 提供两种不同的恐慌处理器：

* `unwind` --- 展开栈，可能可恢复。
* `abort` ---- 中止进程，不可恢复。

并非所有目标都可能提供 `unwind` 处理器。

> [!NOTE]
> 与 `std` 链接时使用的恐慌处理器可以使用 [`-C panic`] CLI 标志设置。大多数目标的默认值是 `unwind`。
>
> 标准库的恐慌行为可以在运行时使用 [`std::panic::set_hook`] 函数修改。

r[panic.panic_handler.std.no_std]
链接 [`no_std`] 二进制文件、dylib、cdylib 或 staticlib 将需要指定你自己的恐慌处理器。

r[panic.strategy]
## 恐慌策略

r[panic.strategy.intro]
_恐慌策略_定义了包构建以支持的恐慌行为类型。

> [!NOTE]
> 可以在 `rustc` 中使用 [`-C panic`] CLI 标志选择恐慌策略。
>
> 当生成二进制文件、dylib、cdylib 或 staticlib 并与 `std` 链接时，`-C panic` CLI 标志还会影响使用哪个[恐慌处理器][panic handler]。

> [!NOTE]
> 使用 `abort` 恐慌策略编译代码时，优化器可能会假设跨 Rust 帧展开是不可能的，这可能会导致代码大小和运行时速度改进。

> [!NOTE]
> 有关链接具有不同恐慌策略的包的限制，请参阅 [link.unwinding]。一个含义是，使用 `unwind` 策略构建的包可以使用 `abort` 恐慌处理器，但 `abort` 策略不能使用 `unwind` 恐慌处理器。

r[panic.unwind]
## 展开

r[panic.unwind.intro]
恐慌可能是可恢复的或不可恢复的，尽管可以配置（通过选择非展开恐慌处理器）始终不可恢复。（反过来不成立：`unwind` 处理器不保证所有恐慌都是可恢复的，只保证通过 `panic!` 宏和类似的标准库机制引起的恐慌是可恢复的。）

r[panic.unwind.destruction]
当恐慌发生时，`unwind` 处理器"展开" Rust 帧，就像 C++ 的 `throw` 展开 C++ 帧一样，直到恐慌达到恢复点（例如在线程边界处）。这意味着当恐慌遍历 Rust 帧时，这些帧中[实现 `Drop`][destructors]的活动对象将调用其 `drop` 方法。因此，当正常执行恢复时，不再可访问的对象将被"清理"，就像它们正常超出作用域一样。

> [!NOTE]
> 只要保留这种资源清理的保证，"展开"可以在不实际使用目标平台 C++ 使用的机制的情况下实现。

> [!NOTE]
> 标准库提供了两种从恐慌中恢复的机制，[`std::panic::catch_unwind`]（在恐慌线程内启用恢复）和 [`std::thread::spawn`]（自动为生成的线程设置恐慌恢复，以便其他线程可以继续运行）。

r[panic.unwind.ffi]
### 跨 FFI 边界展开

r[panic.unwind.ffi.intro]
可以使用[适当的 ABI 声明][unwind-abi]跨 FFI 边界展开。虽然在某些情况下很有用，但这会为未定义行为创造独特的机会，特别是当涉及多个语言运行时时。

r[panic.unwind.ffi.undefined]
使用错误的 ABI 展开是未定义行为：

* 从通过使用非展开 ABI（如 `"C"`、`"system"` 等）声明的函数声明或指针调用的外部函数引起的展开进入 Rust 代码。（例如，当用 C++ 编写的这样的函数抛出未捕获的异常并传播到 Rust 时，就会发生这种情况。）
* 从不支持展开的代码调用展开的 Rust `extern` 函数（使用 `extern "C-unwind"` 或其他允许展开的 ABI），例如使用 `-fno-exceptions` 编译的 GCC 或 Clang 代码

r[panic.unwind.ffi.catch-foreign]
使用 [`std::panic::catch_unwind`]、[`std::thread::JoinHandle::join`] 捕获外部展开操作（例如 C++ 异常），或让其传播到 Rust `main()` 函数或线程根之外，将具有以下两种行为之一，并且未指定将发生哪一种：

* 进程中止。
* 函数返回包含不透明类型的 [`Result::Err`]。

> [!NOTE]
> 使用不同实例的 Rust 标准库编译或链接的 Rust 代码计为此保证的"外部异常"。因此，使用 `panic!` 并链接到一个版本的 Rust 标准库的库，从使用不同版本的标准库的应用程序调用，可能会导致整个应用程序中止，即使该库仅在子线程中使用。

r[panic.unwind.ffi.dispose-panic]
目前无法保证当外部运行时尝试处理或重新抛出 Rust `panic` 负载时发生的行为。换句话说，源自 Rust 运行时的展开必须导致进程终止或被同一运行时捕获。

[`-C panic`]: ../rustc/codegen-options/index.html#panic
[`no_std`]: names/preludes.md#the-no_std-attribute
[`PanicInfo`]: core::panic::PanicInfo
[array indexing]: expressions/array-expr.md#array-and-slice-indexing-expressions
[attribute]: attributes.md
[destructors]: destructors.md
[panic handler]: #the-panic_handler-attribute
[runtime]: runtime.md
[unwind-abi]: items/functions.md#unwinding
