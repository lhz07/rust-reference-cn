r[memory]
# 内存模型

> [!WARNING]
> Rust 的内存模型尚不完整，尚未完全确定。

r[memory.bytes]
## 字节

r[memory.bytes.intro]
Rust 中最基本的内存单位是字节。

> [!NOTE]
> 虽然字节通常降低为硬件字节，但 Rust 使用字节的"抽象"概念，可以区分硬件中不存在的情况，例如未初始化，或存储指针的一部分。这些区别可能会影响你的程序是否具有未定义行为，因此它们仍然对编译的 Rust 程序的行为方式产生实际影响。

r[memory.bytes.contents]
每个字节可能具有以下值之一：

r[memory.bytes.init]
* 包含 `u8` 值和可选[出处][std::ptr#provenance]的已初始化字节，

r[memory.bytes.uninit]
* 未初始化的字节。

> [!NOTE]
> 上述列表尚不能保证是详尽的。
