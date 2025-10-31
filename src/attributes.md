r[attributes]
# 属性

r[attributes.syntax]
```grammar,attributes
InnerAttribute -> `#` `!` `[` Attr `]`

OuterAttribute -> `#` `[` Attr `]`

Attr ->
      SimplePath AttrInput?
    | `unsafe` `(` SimplePath AttrInput? `)`

AttrInput ->
      DelimTokenTree
    | `=` Expression
```

r[attributes.intro]
_属性（attribute）_ 是一种通用的、自由形式的元数据，根据名称、约定、语言和编译器版本进行解释。
属性的建模基于 [ECMA-335] 中的 Attributes，语法来自 [ECMA-334]（C#）。

r[attributes.inner]
_内部属性（Inner attributes）_，在井号（`#`）后面加感叹号（`!`），应用于声明该属性的形式内部。

> [!EXAMPLE]
> ```rust
> // 应用于包含的模块或 crate 的通用元数据。
> #![crate_type = "lib"]
>
> // 内部属性应用于整个函数。
> fn some_unused_variables() {
>   #![allow(unused_variables)]
>
>   let x = ();
>   let y = ();
>   let z = ();
> }
> ```

r[attributes.outer]
_外部属性（Outer attributes）_，在井号后面不加感叹号，应用于紧随该属性之后的形式。

> [!EXAMPLE]
> ```rust
> // 标记为单元测试的函数
> #[test]
> fn test_foo() {
>     /* ... */
> }
>
> // 条件编译的模块
> #[cfg(target_os = "linux")]
> mod bar {
>     /* ... */
> }
>
> // 用于抑制警告/错误的 lint 属性
> #[allow(non_camel_case_types)]
> type int8_t = i8;
> ```

r[attributes.input]
属性由指向该属性的路径组成，后面跟一个可选的界定标记树，其解释由属性定义。
除宏属性外的属性还允许输入为等号（`=`）后跟一个表达式。
有关详细信息，请参见下面的[元项语法](#meta-item-attribute-syntax)。

r[attributes.safety]
属性可能在应用时是不安全的。为了在使用这些属性时避免未定义行为，必须满足某些编译器无法检查的约束。
为了断言这些约束已满足，属性需要包装在 `unsafe(..)`中，例如 `#[unsafe(no_mangle)]`。

以下属性是不安全的：

* [`export_name`]
* [`link_section`]
* [`naked`]
* [`no_mangle`]

r[attributes.kind]
属性可以分为以下几类：

* [内置属性][Built-in attributes]
* [过程宏属性][attribute macros]
* [派生宏辅助属性][Derive macro helper attributes]
* [工具属性](#tool-attributes)

r[attributes.allowed-position]
属性可以应用于语言中的多种形式：

* 所有[条目声明][item declarations]都接受外部属性，而[外部块][external blocks]、
  [函数][functions]、[实现][implementations]和[模块][modules]都接受内部属性。
* 大多数[语句][statements]都接受外部属性（关于表达式语句的限制，请参阅[表达式属性][Expression Attributes]）。
* [块表达式][Block expressions]接受外部和内部属性，但仅当它们是[表达式语句][expression statement]的外部表达式或
  另一个块表达式的最终表达式时。
* [枚举][Enum]变体、[结构体][struct]和[联合体][union]字段接受外部属性。
* [匹配表达式分支][match expressions]接受外部属性。
* [泛型生命周期或类型参数][generics]接受外部属性。
* 表达式在受限情况下接受外部属性，详细信息请参阅[表达式属性][Expression Attributes]。
* [函数][functions]、[闭包][closure]和[函数指针][function pointer]
  参数接受外部属性。这包括在函数指针和[外部块][variadic functions]中使用 `...` 表示的可变参数的属性。

r[attributes.meta]
## 元项属性语法

r[attributes.meta.intro]
"元项"是大多数[内置属性][built-in attributes]的 [Attr] 规则使用的语法。它具有以下语法：

r[attributes.meta.syntax]
```grammar,attributes
@root MetaItem ->
      SimplePath
    | SimplePath `=` Expression
    | SimplePath `(` MetaSeq? `)`

MetaSeq ->
    MetaItemInner ( `,` MetaItemInner )* `,`?

MetaItemInner ->
      MetaItem
    | Expression
```

r[attributes.meta.literal-expr]
元项中的表达式必须在宏展开后变成字面量表达式，且不能包含整数或浮点类型后缀。
非字面量表达式将在语法上被接受（并可以传递给过程宏），但在解析后会被拒绝。

r[attributes.meta.order]
请注意，如果属性出现在另一个宏内部，它将在那个外部宏之后展开。例如，以下代码将首先展开
`Serialize` 过程宏，该宏必须保留 `include_str!` 调用以便其能够被展开：

```rust ignore
#[derive(Serialize)]
struct Foo {
    #[doc = include_str!("x.md")]
    x: u32
}
```

r[attributes.meta.order-macro]
此外，属性中的宏只会在应用于条目的所有其他属性之后展开：

```rust ignore
#[macro_attr1] // 首先展开
#[doc = mac!()] // `mac!` 第四个展开。
#[macro_attr2] // 第二个展开
#[derive(MacroDerive1, MacroDerive2)] // 第三个展开
fn foo() {}
```

r[attributes.meta.builtin]
各种内置属性使用元项语法的不同子集来指定它们的输入。
以下语法规则展示了一些常用形式：

r[attributes.meta.builtin.syntax]
```grammar,attributes
@root MetaWord ->
    IDENTIFIER

MetaNameValueStr ->
    IDENTIFIER `=` (STRING_LITERAL | RAW_STRING_LITERAL)

@root MetaListPaths ->
    IDENTIFIER `(` ( SimplePath (`,` SimplePath)* `,`? )? `)`

@root MetaListIdents ->
    IDENTIFIER `(` ( IDENTIFIER (`,` IDENTIFIER)* `,`? )? `)`

@root MetaListNameValueStr ->
    IDENTIFIER `(` ( MetaNameValueStr (`,` MetaNameValueStr)* `,`? )? `)`
```

元项的一些示例如下：

样式 | 示例
------|--------
[MetaWord] | `no_std`
[MetaNameValueStr] | `doc = "example"`
[MetaListPaths] | `allow(unused, clippy::inline_always)`
[MetaListIdents] | `macro_use(foo, bar)`
[MetaListNameValueStr] | `link(name = "CoreFoundation", kind = "framework")`

r[attributes.activity]
## 活动属性和惰性属性

r[attributes.activity.intro]
属性要么是活动的，要么是惰性的。在属性处理期间，*活动属性*会从它们所在的形式中移除自身，
而*惰性属性*则保持不变。

[`cfg`] 和 [`cfg_attr`] 属性是活动的。
[属性宏][Attribute macros]是活动的。所有其他属性都是惰性的。

r[attributes.tool]
## 工具属性

r[attributes.tool.intro]
编译器可能允许外部工具的属性，其中每个工具都位于[工具前导][tool prelude]中自己的模块中。
属性路径的第一段是工具的名称，后面可以有一个或多个附加段，其解释取决于工具。

r[attributes.tool.ignored]
当工具未使用时，工具的属性将被接受而不会发出警告。
当工具正在使用时，工具负责处理和解释其属性。

r[attributes.tool.prelude]
如果使用了 [`no_implicit_prelude`] 属性，则工具属性不可用。

```rust
// 告诉 rustfmt 工具不要格式化以下元素。
#[rustfmt::skip]
struct S {
}

// 控制 clippy 工具的"圈复杂度"阈值。
#[clippy::cyclomatic_complexity = "100"]
pub fn f() {}
```

> [!NOTE]
> `rustc` 当前识别的工具有 "clippy"、"rustfmt"、"diagnostic"、"miri" 和 "rust_analyzer"。

r[attributes.builtin]
## 内置属性索引

以下是所有内置属性的索引。

- 条件编译
  - [`cfg`] --- 控制条件编译。
  - [`cfg_attr`] --- 有条件地包含属性。

- 测试
  - [`test`] --- 将函数标记为测试。
  - [`ignore`] --- 禁用测试函数。
  - [`should_panic`] --- 表示测试应该产生 panic。

- 派生
  - [`derive`] --- 自动 trait 实现。
  - [`automatically_derived`] --- `derive` 创建的实现的标记。

- 宏
  - [`macro_export`] --- 导出 `macro_rules` 宏供跨 crate 使用。
  - [`macro_use`] --- 扩展宏可见性，或从其他 crate 导入宏。
  - [`proc_macro`] --- 定义类函数宏。
  - [`proc_macro_derive`] --- 定义派生宏。
  - [`proc_macro_attribute`] --- 定义属性宏。

- 诊断
  - [`allow`]、[`expect`]、[`warn`]、[`deny`]、[`forbid`] --- 改变默认 lint 级别。
  - [`deprecated`] --- 生成弃用通知。
  - [`must_use`] --- 为未使用的值生成 lint。
  - [`diagnostic::on_unimplemented`] --- 提示编译器在未实现 trait 时发出特定错误消息。
  - [`diagnostic::do_not_recommend`] --- 提示编译器在错误消息中不显示某个 trait impl。

- ABI、链接、符号和 FFI
  - [`link`] --- 指定要与 `extern` 块链接的本地库。
  - [`link_name`] --- 指定 `extern` 块中函数或静态项的符号名称。
  - [`link_ordinal`] --- 指定 `extern` 块中函数或静态项的符号序数。
  - [`no_link`] --- 防止链接外部 crate。
  - [`repr`] --- 控制类型布局。
  - [`crate_type`] --- 指定 crate 类型（库、可执行文件等）。
  - [`no_main`] --- 禁用发出 `main` 符号。
  - [`export_name`] --- 指定函数或静态项的导出符号名称。
  - [`link_section`] --- 指定用于函数或静态项的目标文件段。
  - [`no_mangle`] --- 禁用符号名称编码。
  - [`used`] --- 强制编译器在输出目标文件中保留静态项。
  - [`crate_name`] --- 指定 crate 名称。

- 代码生成
  - [`inline`] --- 内联代码的提示。
  - [`cold`] --- 提示函数不太可能被调用。
  - [`naked`] --- 防止编译器发出函数序言和尾声。
  - [`no_builtins`] --- 禁用某些内置函数的使用。
  - [`target_feature`] --- 配置平台特定的代码生成。
  - [`track_caller`] --- 将父调用位置传递给 `std::panic::Location::caller()`。
  - [`instruction_set`] --- 指定用于生成函数代码的指令集。

- 文档
  - `doc` --- 指定文档。更多信息请参见 [The Rustdoc Book]。[文档注释][Doc comments]会被转换为 `doc` 属性。

- 前导
  - [`no_std`] --- 从前导中移除 std。
  - [`no_implicit_prelude`] --- 禁用模块内的前导查找。

- 模块
  - [`path`] --- 指定模块的文件名。

- 限制
  - [`recursion_limit`] --- 设置某些编译时操作的最大递归限制。
  - [`type_length_limit`] --- 设置多态类型的最大大小。

- 运行时
  - [`panic_handler`] --- 设置处理 panic 的函数。
  - [`global_allocator`] --- 设置全局内存分配器。
  - [`windows_subsystem`] --- 指定要链接的 Windows 子系统。

- 功能
  - `feature` --- 用于启用不稳定或实验性编译器功能。有关在 `rustc` 中实现的功能，
    请参见 [The Unstable Book]。

- 类型系统
  - [`non_exhaustive`] --- 表明类型将来会添加更多字段/变体。

- 调试器
  - [`debugger_visualizer`] --- 嵌入指定类型调试器输出的文件。
  - [`collapse_debuginfo`] --- 控制如何在调试信息中编码宏调用。

[Doc comments]: comments.md#doc-comments
[ECMA-334]: https://www.ecma-international.org/publications-and-standards/standards/ecma-334/
[ECMA-335]: https://www.ecma-international.org/publications-and-standards/standards/ecma-335/
[Expression Attributes]: expressions.md#expression-attributes
[The Rustdoc Book]: ../rustdoc/the-doc-attribute.html
[The Unstable Book]: ../unstable-book/index.html
[`allow`]: attributes/diagnostics.md#lint-check-attributes
[`automatically_derived`]: attributes/derive.md#the-automatically_derived-attribute
[`cfg_attr`]: conditional-compilation.md#the-cfg_attr-attribute
[`cfg`]: conditional-compilation.md#the-cfg-attribute
[`cold`]: attributes/codegen.md#the-cold-attribute
[`collapse_debuginfo`]: attributes/debugger.md#the-collapse_debuginfo-attribute
[`crate_name`]: crates-and-source-files.md#the-crate_name-attribute
[`crate_type`]: linkage.md
[`debugger_visualizer`]: attributes/debugger.md#the-debugger_visualizer-attribute
[`deny`]: attributes/diagnostics.md#lint-check-attributes
[`deprecated`]: attributes/diagnostics.md#the-deprecated-attribute
[`derive`]: attributes/derive.md
[`export_name`]: abi.md#the-export_name-attribute
[`expect`]: attributes/diagnostics.md#lint-check-attributes
[`forbid`]: attributes/diagnostics.md#lint-check-attributes
[`global_allocator`]: runtime.md#the-global_allocator-attribute
[`ignore`]: attributes/testing.md#the-ignore-attribute
[`inline`]: attributes/codegen.md#the-inline-attribute
[`instruction_set`]: attributes/codegen.md#the-instruction_set-attribute
[`link_name`]: items/external-blocks.md#the-link_name-attribute
[`link_ordinal`]: items/external-blocks.md#the-link_ordinal-attribute
[`link_section`]: abi.md#the-link_section-attribute
[`link`]: items/external-blocks.md#the-link-attribute
[`macro_export`]: macros-by-example.md#the-macro_export-attribute
[`macro_use`]: macros-by-example.md#the-macro_use-attribute
[`must_use`]: attributes/diagnostics.md#the-must_use-attribute
[`naked`]: attributes/codegen.md#the-naked-attribute
[`no_builtins`]: attributes/codegen.md#the-no_builtins-attribute
[`no_implicit_prelude`]: names/preludes.md#the-no_implicit_prelude-attribute
[`no_link`]: items/extern-crates.md#the-no_link-attribute
[`no_main`]: crates-and-source-files.md#the-no_main-attribute
[`no_mangle`]: abi.md#the-no_mangle-attribute
[`no_std`]: names/preludes.md#the-no_std-attribute
[`non_exhaustive`]: attributes/type_system.md#the-non_exhaustive-attribute
[`panic_handler`]: panic.md#the-panic_handler-attribute
[`path`]: items/modules.md#the-path-attribute
[`proc_macro_attribute`]: procedural-macros.md#the-proc_macro_attribute-attribute
[`proc_macro_derive`]: macro.proc.derive
[`proc_macro`]: procedural-macros.md#the-proc_macro-attribute
[`recursion_limit`]: attributes/limits.md#the-recursion_limit-attribute
[`repr`]: type-layout.md#representations
[`should_panic`]: attributes/testing.md#the-should_panic-attribute
[`target_feature`]: attributes/codegen.md#the-target_feature-attribute
[`test`]: attributes/testing.md#the-test-attribute
[`track_caller`]: attributes/codegen.md#the-track_caller-attribute
[`type_length_limit`]: attributes/limits.md#the-type_length_limit-attribute
[`used`]: abi.md#the-used-attribute
[`warn`]: attributes/diagnostics.md#lint-check-attributes
[`windows_subsystem`]: runtime.md#the-windows_subsystem-attribute
[attribute macros]: procedural-macros.md#the-proc_macro_attribute-attribute
[block expressions]: expressions/block-expr.md
[built-in attributes]: #built-in-attributes-index
[derive macro helper attributes]: procedural-macros.md#derive-macro-helper-attributes
[enum]: items/enumerations.md
[expression statement]: statements.md#expression-statements
[external blocks]: items/external-blocks.md
[functions]: items/functions.md
[generics]: items/generics.md
[implementations]: items/implementations.md
[item declarations]: items.md
[match expressions]: expressions/match-expr.md
[modules]: items/modules.md
[statements]: statements.md
[struct]: items/structs.md
[tool prelude]: names/preludes.md#tool-prelude
[union]: items/unions.md
[closure]: expressions/closure-expr.md
[function pointer]: types/function-pointer.md
[variadic functions]: items/external-blocks.html#variadic-functions
[`diagnostic::on_unimplemented`]: attributes/diagnostics.md#the-diagnosticon_unimplemented-attribute
[`diagnostic::do_not_recommend`]: attributes/diagnostics.md#the-diagnosticdo_not_recommend-attribute
