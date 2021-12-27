# Rust设计模式 中文版

一本关于Rust的设计模式和习语的开源书， [阅读地址](https://rust-unofficial.github.io/patterns/).

## 贡献
当你发现在这个库中缺少了一些对他人有帮助的内容，并渴望去阐述它们？棒极了! 我们总是乐于接受新的贡献(例如，对某些主题的阐述或纠正)。

你可以查看 [Umbrella issue](https://github.com/rust-unofficial/patterns/issues/116)
以了解所有可能被添加的模式、反模式和习语。

我们建议阅读我们的 [贡献指南 Contribution guide](./CONTRIBUTING.md) 获得更多关于如何作出贡献的信息。

## 使用 mdbook 构建

本书是用[mdbook](https://rust-lang.github.io/mdBook/)构建的。你可以
你可以通过运行 `cargo install mdbook` 进行安装。

如果你想在本地构建它，你可以在文件的根目录下运行以下任意一条命令

- `mdbook build`

构建静态HTML页面作为输出，默认输出到`/book`

- `mdbook serve`

  运行到 `http://localhost:3000` (端口可变, 看一下终端输出确认一下) ，发生变化时重新加载浏览器。

## 许可证

保持 **MPL-2.0**; see [LICENSE](./LICENSE).
