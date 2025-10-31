r[input]
# 输入格式

r[input.syntax]
```grammar,lexer
@root CHAR -> <一个 Unicode 标量值>

NUL -> U+0000
```

r[input.intro]
本章描述源文件如何被解释为一系列词法单元。

有关程序如何组织到文件中的描述，请参阅[包和源文件][Crates and source files]。

r[input.encoding]
## 源编码

r[input.encoding.utf8]
每个源文件都被解释为以 UTF-8 编码的 Unicode 字符序列。

r[input.encoding.invalid]
如果文件不是有效的 UTF-8，则会出错。

r[input.byte-order-mark]
## 字节顺序标记删除

如果序列中的第一个字符是 `U+FEFF`（[字节顺序标记][BYTE ORDER MARK]），则将其删除。

r[input.crlf]
## CRLF 规范化

每对字符 `U+000D` (CR) 紧跟 `U+000A` (LF) 将被替换为单个 `U+000A` (LF)。
这只发生一次，而不是重复发生，因此规范化后，输入中仍可能存在 `U+000D` (CR) 紧跟 `U+000A` (LF)（例如，如果原始输入包含"CR CR LF LF"）。

字符 `U+000D` (CR) 的其他出现保留在原位（它们被视为[空白符][whitespace]）。

r[input.shebang]
## Shebang 删除

r[input.shebang.intro]
如果剩余序列以字符 `#!` 开头，则从序列中删除直到并包括第一个 `U+000A` (LF) 的字符。

例如，以下文件的第一行将被忽略：

<!-- ignore: tests don't like shebang -->
```rust,ignore
#!/usr/bin/env rustx

fn main() {
    println!("Hello!");
}
```

r[input.shebang.inner-attribute]
作为例外，如果 `#!` 字符后跟（忽略中间的[注释][comments]或[空白符][whitespace]）`[` 词法单元，则不会删除任何内容。
这防止删除源文件开头的[内部属性][inner attribute]。

> [!NOTE]
> 标准库 [`include!`] 宏对其读取的文件应用字节顺序标记删除、CRLF 规范化和 shebang 删除。[`include_str!`] 和 [`include_bytes!`] 宏不会。

r[input.tokenization]
## 词法单元化

然后，将得到的字符序列转换为词法单元，如本章其余部分所述。

[inner attribute]: attributes.md
[BYTE ORDER MARK]: https://en.wikipedia.org/wiki/Byte_order_mark#UTF-8
[comments]: comments.md
[Crates and source files]: crates-and-source-files.md
[_shebang_]: https://en.wikipedia.org/wiki/Shebang_(Unix)
[whitespace]: whitespace.md
