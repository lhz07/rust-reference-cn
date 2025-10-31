# Rust 语言参考手册（中文版）

这是 Rust 编程语言的主要参考文档的中文翻译版本。

原始英文文档：https://github.com/rust-lang/reference/

## 构建

要构建本参考手册，首先克隆项目：

```sh
git clone https://github.com/lhz07/rust-reference-cn.git
cd rust-reference-cn
```

### 安装 mdbook

本参考手册使用 [mdbook](https://rust-lang.github.io/mdBook/) 构建。

首先，确保已安装最新的 nightly Rust 编译器，这是运行测试所必需的：

```sh
rustup toolchain install nightly
rustup override set nightly
```

然后，确保已安装 `mdbook`，这是构建参考手册所必需的：

```sh
cargo install --locked mdbook
```

### 运行 mdbook

`mdbook` 提供了多种不同的命令和选项来帮助你使用本手册：

* `mdbook build --open`: 构建本手册并在网页浏览器中打开。
* `mdbook serve --open`: 在本地主机上启动一个 Web 服务器。它还会在任何文件更改时自动重新构建手册，并自动重新加载你的网页浏览器。

手册内容由 `SUMMARY.md` 文件驱动，每个文件都必须链接到那里。有关其用法，请参阅 https://rust-lang.github.io/mdBook/。

## 关于翻译

本项目致力于提供专业、准确、易于理解的 Rust 语言参考文档中文翻译。

如果你发现翻译错误或有改进建议，欢迎提交 Issue 或 Pull Request。