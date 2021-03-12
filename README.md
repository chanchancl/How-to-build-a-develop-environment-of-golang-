# 如何构建一个golang开发环境

## 前情提要
由于众所周知的原因，golang的官网无法正常访问，而依赖于 *.golang.org 域名的一系列功能也经常无法正常使用
将以前遇到的坑总结一下，并给出一份最简单的解决方案

## 准备步骤

去golang官网 golang.org 或者 下载页面镜像 https://studygolang.com/dl 下载最新的golang安装包

当前最新golang版本为 1.16.2

> 如果你使用windows，可以下载 [go1.16.2.windows-amd64.msi](https://studygolang.com/dl/golang/go1.16.2.windows-amd64.msi)

> 如果你使用Linux，可以选择 [go1.16.2.linux-amd64.tar.gz](https://studygolang.com/dl/golang/go1.16.2.linux-amd64.tar.gz)

## 进行安装

> 如果你使用windows，可以运行 go1.16.2.windows-amd64.msi，不要忘记把安装地址添加进 PATH 目录

> 如果你使用Linux，只需要使用如下命令将 go1.16.2.linux-amd64.tar.gz 解压即可

```
> sudo tar -C /usr/local -xzf go*linux-amd64.tar.gz
```
请注意，这条命令可能需要root权限，请使用 sudo 运行

接下来设置 *PATH* 路径

```
> echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile && source ~/.profile
```

### GOPATH

默认情况下 go 会把第三方库，下载到 /home/$USER/go 目录下

可以通过修改 GOPATH 环境变量来修改此目录。

## 验证安装

不论你使用什么操作系统，在将安装目录正确添加进 PATH 路径之后，可以使用一下命令来验证安装

```
> go version
go version go1.16.2 linux/amd64
```

如果显示 *go version go1.16.2 linux/amd64* 表明安装成功

## 环境的进一步优化

此时，你已经可以打开编辑器开始写golang代码，并且使用 go build 命令来进行编译

不过在通常编码时，我们都需要依赖于大量第三方优秀的库，而安装这些库又由于 前情提要 中所讲的那样，面临一些困难。

在这里可以使用代理来避免这一问题，我选择使用 [goproxy.io](goproxy.io)


```
> echo "export GOPROXY=https://goproxy.io,direct" >> ~/.profile && source ~/.profile
```

## 轻量级的编辑器

作为一款编辑器界的新型，VS Code 毫无疑问已经打响了自己的名声，本人也是自很久以前就一直是 VS Code 忠实用户。所以接下来介绍一下如何构建*基于VS Code的golang环境*。

首先通过正常方法安装VS Code， 并在左侧扩展栏中搜索golang，选择并安装golang扩展。

安装之后，该扩展会提醒你有一些必要的第三方程序需要安装，它们分别是

* github.com/uudashr/gopkgs/tree/master/v2
* github.com/ramya-rao-a/go-outline 
* github.com/cweill/gotests/gotests
* github.com/fatih/gomodifytags
* github.com/josharian/impl
* github.com/haya14busa/goplay/cmd/goplay
* github.com/go-delve/delve/cmd/dlv
* honnef.co/go/tools/cmd/staticcheck
* golang.org/x/tools/gopls

如果你选择点击对话框上的 install all 按钮，那么这里可能有一个小问题：该扩展并不会通过我们的GOPROXY 代理来安装这些第三方，所以可能会失败，那么代替的方法是直接使用下列命令安装（前提是已经正确配置代理）

```
go get -v github.com/uudashr/gopkgs/v2/cmd/gopkgs
go get -v github.com/ramya-rao-a/go-outline
go get -v github.com/cweill/gotests/gotests
go get -v github.com/fatih/gomodifytags
go get -v github.com/josharian/impl
go get -v github.com/haya14busa/goplay/cmd/goplay
go get -v github.com/go-delve/delve/cmd/dlv
go get -v honnef.co/go/tools/cmd/staticcheck
go get -v golang.org/x/tools/gopls
```

执行完毕后，可以通过命令来查看:

```
> ls ~/go/bin
dlv  gomodifytags  go-outline  gopkgs  goplay  gopls  gotests  impl  staticcheck
```

可以发现，这些第三方的代码被go get 下载之后，并且编译为二进制程序放置在 GOPATH 目录下，而 VS Code 的 golang 扩展会在编写代码的过程中使用这些第三方程序。