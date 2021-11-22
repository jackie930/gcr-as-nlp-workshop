#如何构建该 workshop

## 什么是 Hugo
Hugo是最流行的开源静态站点生成器之一。凭借惊人的速度和灵活性，Hugo让网站建设再次变得有趣起来。更多信息请参考 https://gohugo.io/
Hugo 使用 Go 语言开发，主要的功能是静态网站生成器。不仅解决了环境依赖、性能较差的问题，还有使用简单、部署方便等诸多优点，通过 LiveReload 实时刷新，极大的优化文章的写作体验。

## 如何安装 Hugo

Hugo 是用 Go 语言写的，支持多个平台。
最新的 release 版本可以在 Hugo Releases 找到。 我们提供预构建好的二进制包括  Windows,  Linux,  FreeBSD 和  OS X (Darwin) for x64, i386 和 ARM architectures.
你可以使用 Go 编译器工具链源码编译 Hugo，比如在其他的操作系统如 DragonFly BSD, OpenBSD, Plan 9 和 Solaris 。 在 http://golang.org/doc/install/source 查看完整的操作系统和编译架构的支持列表。

### Mac
在 Mac 下使用 Homebrew 进行安装。

```sh
brew install hugo
```

## 如何启动 Hugo
在目录 StewardWorkshop 输入 hugo sever
```sh
hugo server
```

然后启动浏览器，在地址栏输入 http://localhost:1313 进行预览。

## Build Static page
```sh
hugo -D
```