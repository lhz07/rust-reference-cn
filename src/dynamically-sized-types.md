r[dynamic-sized]
# 动态大小类型

r[dynamic-sized.intro]
大多数类型具有在编译时已知的固定大小，并实现 [`Sized`][sized] 特征。大小仅在运行时已知的类型称为_动态大小类型_ (_DST_)，或非正式地称为非固定大小类型。[切片][Slices]、[特征对象][trait objects]和 [str] 是 <abbr title="动态大小类型">DST</abbr> 的示例。

r[dynamic-sized.restriction]
此类类型只能在某些情况下使用：

r[dynamic-sized.pointer-types]
* 指向 <abbr title="动态大小类型">DST</abbr> 的[指针类型][Pointer types]是固定大小的，但其大小是指向固定大小类型的指针的两倍
    * 指向切片和 `str` 的指针还存储元素数量。
    * 指向特征对象的指针还存储指向 vtable 的指针。

r[dynamic-sized.question-sized]
* <abbr title="动态大小类型">DST</abbr> 可以作为类型参数提供给具有特殊 `?Sized` 约束的泛型类型参数。当相应的关联类型声明具有 `?Sized` 约束时，它们也可以用于关联类型定义。默认情况下，任何类型参数或关联类型都具有 `Sized` 约束，除非使用 `?Sized` 放宽它。

r[dynamic-sized.trait-impl]
* 特征可以为 <abbr title="动态大小类型">DST</abbr> 实现。与泛型类型参数不同，`Self: ?Sized` 在特征定义中是默认的。

r[dynamic-sized.struct-field]
* 结构体可以将 <abbr title="动态大小类型">DST</abbr> 作为最后一个字段；这使结构体本身成为 <abbr title="动态大小类型">DST</abbr>。

> [!NOTE]
> [变量][Variables]、函数参数、[const] 项和 [static] 项必须是 `Sized`。

[sized]: special-types-and-traits.md#sized
[Slices]: types/slice.md
[str]: types/textual.md
[trait objects]: types/trait-object.md
[Pointer types]: types/pointer.md
[Variables]: variables.md
[const]: items/constant-items.md
[static]: items/static-items.md
