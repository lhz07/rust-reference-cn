r[abi]
# 应用程序二进制接口 (ABI)

r[abi.intro]
本节记录影响包的编译输出的 ABI 的特性。

有关为导出函数指定 ABI 的信息，请参阅*[外部函数][extern functions]*。有关为链接外部库指定 ABI 的信息，请参阅*[外部块][external blocks]*。

r[abi.used]
## `used` 属性

r[abi.used.intro]
*`used` 属性*只能应用于 [`static` 项][`static` items]。此[属性][attribute]强制编译器将变量保留在输出对象文件（.o、.rlib 等，不包括最终二进制文件）中，即使该变量未被包中的任何其他项使用或引用。
但是，链接器仍然可以删除这样的项。

下面是一个示例，显示编译器在什么条件下将 `static` 项保留在输出对象文件中。

``` rust
// foo.rs

// 由于 `#[used]`，这会被保留：
#[used]
static FOO: u32 = 0;

// 这是可删除的，因为它未使用：
#[allow(dead_code)]
static BAR: u32 = 0;

// 这会被保留，因为它是公开可达的：
pub static BAZ: u32 = 0;

// 这会被保留，因为它被公开、可达的函数引用：
static QUUX: u32 = 0;

pub fn quux() -> &'static u32 {
    &QUUX
}

// 这是可删除的，因为它被私有、未使用（死）的函数引用：
static CORGE: u32 = 0;

#[allow(dead_code)]
fn corge() -> &'static u32 {
    &CORGE
}
```

``` console
$ rustc -O --emit=obj --crate-type=rlib foo.rs

$ nm -C foo.o
0000000000000000 R foo::BAZ
0000000000000000 r foo::FOO
0000000000000000 R foo::QUUX
0000000000000000 T foo::quux
```

r[abi.no_mangle]
## `no_mangle` 属性

r[abi.no_mangle.intro]
*`no_mangle` 属性*可以用于任何[条目][item]以禁用标准符号名称修饰。该项的符号将是该项名称的标识符。

r[abi.no_mangle.publicly-exported]
此外，该项将从生成的库或对象文件中公开导出，类似于 [`used` 属性](#the-used-attribute)。

r[abi.no_mangle.unsafe]
此属性是不安全的，因为未修饰的符号可能与另一个具有相同名称的符号（或与众所周知的符号）冲突，导致未定义行为。

```rust
#[unsafe(no_mangle)]
extern "C" fn foo() {}
```

r[abi.no_mangle.edition2024]
> [!EDITION-2024]
> 在 2024 版本之前，允许使用 `no_mangle` 属性而不需要 `unsafe` 限定。

r[abi.link_section]
## `link_section` 属性

r[abi.link_section.intro]
*`link_section` 属性*指定[函数][function]或[静态项][static]的内容将放置到的对象文件的节。

r[abi.link_section.syntax]
`link_section` 属性使用 [MetaNameValueStr] 语法来指定节名称。

<!-- no_run: don't link. The format of the section name is platform-specific. -->
```rust,no_run
#[unsafe(no_mangle)]
#[unsafe(link_section = ".example_section")]
pub static VAR1: u32 = 1;
```

r[abi.link_section.unsafe]
此属性是不安全的，因为它允许用户将数据和代码放入不期望它们的内存节中，例如将可变数据放入只读区域。

r[abi.link_section.edition2024]
> [!EDITION-2024]
> 在 2024 版本之前，允许使用 `link_section` 属性而不需要 `unsafe` 限定。

r[abi.export_name]
## `export_name` 属性

r[abi.export_name.intro]
*`export_name` 属性*指定将在[函数][function]或[静态项][static]上导出的符号的名称。

r[abi.export_name.syntax]
`export_name` 属性使用 [MetaNameValueStr] 语法来指定符号名称。

```rust
#[unsafe(export_name = "exported_symbol_name")]
pub fn name_in_rust() { }
```

r[abi.export_name.unsafe]
此属性是不安全的，因为具有自定义名称的符号可能与另一个具有相同名称的符号（或与众所周知的符号）冲突，导致未定义行为。

r[abi.export_name.edition2024]
> [!EDITION-2024]
> 在 2024 版本之前，允许使用 `export_name` 属性而不需要 `unsafe` 限定。

[`static` items]: items/static-items.md
[attribute]: attributes.md
[extern functions]: items/functions.md#extern-function-qualifier
[external blocks]: items/external-blocks.md
[function]: items/functions.md
[item]: items.md
[static]: items/static-items.md
