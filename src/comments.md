r[comments]
# 注释

r[comments.syntax]
```grammar,lexer
@root LINE_COMMENT ->
      `//` (~[`/` `!` LF] | `//`) ~LF*
    | `//`

BLOCK_COMMENT ->
      `/*`
        ( ~[`*` `!`] | `**` | BLOCK_COMMENT_OR_DOC )
        ( BLOCK_COMMENT_OR_DOC | ~`*/` )*
      `*/`
    | `/**/`
    | `/***/`

@root INNER_LINE_DOC ->
    `//!` ~[LF CR]*

INNER_BLOCK_DOC ->
    `/*!` ( BLOCK_COMMENT_OR_DOC | ~[`*/` CR] )* `*/`

@root OUTER_LINE_DOC ->
    `///` (~`/` ~[LF CR]*)?

OUTER_BLOCK_DOC ->
    `/**`
      ( ~`*` | BLOCK_COMMENT_OR_DOC )
      ( BLOCK_COMMENT_OR_DOC | ~[`*/` CR] )*
    `*/`

@root BLOCK_COMMENT_OR_DOC ->
      BLOCK_COMMENT
    | OUTER_BLOCK_DOC
    | INNER_BLOCK_DOC
```

r[comments.normal]
## 非文档注释

注释遵循 C++ 的一般风格，包括行注释（`//`）和块注释（`/* ... */`）形式。支持嵌套块注释。

r[comments.normal.tokenization]
非文档注释被解释为一种空白符形式。

r[comments.doc]
## 文档注释

r[comments.doc.syntax]
以恰好 _三个_ 斜杠（`///`）开头的行文档注释和块文档注释（`/** ... */`），都是外部文档注释，被解释为[`doc` 属性][`doc` attributes]的特殊语法。

r[comments.doc.attributes]
也就是说，它们相当于在注释正文周围编写 `#[doc="..."]`，即 `/// Foo` 变成 `#[doc="Foo"]`，`/** Bar */` 变成 `#[doc="Bar"]`。因此，它们必须出现在接受外部属性的内容之前。

r[comments.doc.inner-syntax]
以 `//!` 开头的行注释和块注释 `/*! ... */` 是应用于注释的父级而不是后续条目的文档注释。

r[comments.doc.inner-attributes]
也就是说，它们相当于在注释正文周围编写 `#![doc="..."]`。`//!` 注释通常用于记录占用源文件的模块。

r[comments.doc.bare-crs]
字符 `U+000D` (CR) 不允许出现在文档注释中。

> [!NOTE]
> 按照惯例，文档注释包含 Markdown，这是 `rustdoc` 所期望的。但是，注释语法不考虑任何内部 Markdown。``/** `glob = "*/*.rs";` */`` 在第一个 `*/` 处终止注释，剩余的代码将导致语法错误。这稍微限制了块文档注释相对于行文档注释的内容。

> [!NOTE]
> 序列 `U+000D` (CR) 后面紧跟 `U+000A` (LF) 之前会被转换为单个 `U+000A` (LF)。

## 示例

```rust
//! 应用于此包的隐式匿名模块的文档注释

pub mod outer_module {

    //!  - 内部行文档
    //!! - 仍然是内部行文档（但开头有一个感叹号）

    /*!  - 内部块文档 */
    /*!! - 仍然是内部块文档（但开头有一个感叹号） */

    //   - 只是一个注释
    ///  - 外部行文档（恰好 3 个斜杠）
    //// - 只是一个注释

    /*   - 只是一个注释 */
    /**  - 外部块文档（恰好）2 个星号 */
    /*** - 只是一个注释 */

    pub mod inner_module {}

    pub mod nested_comments {
        /* 在 Rust 中 /* 我们可以 /* 嵌套注释 */ */ */

        // 所有三种类型的块注释都可以包含或嵌套在
        // 任何其他类型中：

        /*   /* */  /** */  /*! */  */
        /*!  /* */  /** */  /*! */  */
        /**  /* */  /** */  /*! */  */
        pub mod dummy_item {}
    }

    pub mod degenerate_cases {
        // 空内部行文档
        //!

        // 空内部块文档
        /*!*/

        // 空行注释
        //

        // 空外部行文档
        ///

        // 空块注释
        /**/

        pub mod dummy_item {}

        // 空的 2 星号块不是文档块，它是块注释
        /***/

    }

    /* 下一个是不允许的，因为外部文档注释
       需要一个接收文档的条目 */

    /// 我的条目在哪里？
#   mod boo {}
}
```

[`doc` attributes]: ../rustdoc/the-doc-attribute.html
