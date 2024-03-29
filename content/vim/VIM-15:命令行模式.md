---
title: "第15章 命令行模式"
author: "Jonny.Chang"
authorLink: "https://archiguru.io"
description: "VIM|vimrc|GitHub|ArchiGuru|这里是 ArchiGuru 博客，欢迎访问！"
tags: ["vim"]
categories: ["vim"]
draft: false
hiddenFromHomePage: true
hiddenFromSearch: false
toc:
  enable: true

math:
  enable: true
lightgallery: false
featuredImage: ""
featuredImagePreview: ""
license: "https://archiguru.io/LICENSE"
weight: 9991
---

# 第15章 命令行模式

在前三章中，您已经学习了如何使用搜索命令(`/`, `?`)、替换命令(`:s`)、全局命令(`:g`)，以及外部命令(`!`)。这些都是命令行模式命令的一些例子。

在本章中，您将学习命令行模式的更多技巧。

## 进入和退出命令行模式

命令行模式本身也是一种模式，就像普通模式、输入模式、可视模式一样。在这种模式中，光标将转到屏幕底部，此时您可以输入不同的命令。

有 4 种进入命令行模式的方式：
- 搜索命令 (`/`, `?`)
- 命令行指令 (`:`)
- 外部命令 (`!`)

您可以从正常模式或可视模式进入命令行模式。

若要离开命令行模式，您可以使用 `<esc>`、`Ctrl-c`、`Ctrl-[`。

**有时其他资料可能会将"命令行指令"称为"Ex 命令"，将"外部命令"称为"过滤命令"或者"叹号运算符"。**

## 重复上一个命令

您可以用 `@:` 来重复上一个命令行指令或外部命令。

如果您刚运行 `:s/foo/bar/g`，执行 `@:` 将重复该替换。如果您刚运行 `:.!tr '[a-z]' '[A-Z]'`，执行 `@:` 将重复上一次外部命令转换过滤。

## 命令行模式快捷键

在命令行模式中，您可以使用 `Left` 或 `Right` 方向键，来左右移动一个字符。

如果需要移动一个单词，使用 `Shift-Left` 或 `Shift-Right` (在某些操作系统中，您需要使用 `Ctrl` 而不是 `Shift`)。

使用 `Ctrl-b`移动到该行的开始，使用 `Ctrl-e`移动到该行的结束。

和输入模式类似，在命令行模式中，有三种方法可以删除字符：

```
Ctrl-h    删除一个字符
Ctrl-w    删除一个单词
Ctrl-u    删除一整行
```
最后，如果您想像编辑文本文件一样来编辑命令，可以使用 `Ctrl-f`。

这样还可以查看过往的命令，并在这种"命令行编辑的普通模式"中编辑它们，同时还能按下 `Enter` 来运行它们。

## 寄存器和自动补全

当处于命令行模式时，您可以像在插入模式中一样使用 `Ctrl-r` 从Vim寄存器中插入文本。如果您在寄存器 a 中存储了字符串 "foo" ，您可以执行 `Ctrl-r a` 从寄存器a中插入该文本。任何在插入模式中您可以从寄存器中获取的内容，在命令行模式中您也可以获取。

另外，您也可以按 `Ctrl-r Ctrl-w` 获取当前光标下的单词（按 `Ctrl-r Ctrl-A` 获取当前光标下的词组）。还可以按 `Ctrl-r Ctlr-l` 获取当前光标所在行。按 `Ctrl-r Ctrl-f` 获取光标下的文件名。

您也可以对已存在的命令使用自动补全。要自动补全 `echo` 命令，当处于命令行模式时，首先输入 "ec"，接着按下 `<Tab>`，此时您应该能在左下角看到一些 "ec" 开头的 Vim 命令（例如：`echo echoerr echohl echomsg econ`）。按下 `<Tab>` 或 `Ctrl-n` 可以跳到下一个选项。按下 `<Shift-Tab>` 或 `Ctrl-p` 可以回到上一个选项。

一些命令行指令接受文件名作为参数。`edit` 就是一个例子，这时候您也可以使用自动补全。当输入 `:e ` 后（不要忘记空格了），按下 `<Tab>`，Vim 将列出所有相关的文件名，这样您就可以进行选择而不必完整的输入它们。

## 历史记录窗口

您可以查看命令行指令和搜索项的历史记录（要确保在运行 `vim --version` 时，Vim 的编译选项中含有`+cmdline_hist`）。

运行 `:his :` 来查看命令行指令的历史记录：

```
##  cmd History
2  e file1.txt
3  g/foo/d
4  s/foo/bar/g
```

Vim 列出了您运行的所有 `:` 命令。默认情况下，Vim 存储最后 50 个命令。运行 `:set history=100` 可以将 Vim 记住的条目总数更改为 100。

一个更有用的做法是使用命令行历史记录窗口，按`q:`将会打开一个可搜索、可编辑的历史记录窗口。假设按下`q:`后您有如下的表达式：

```
51  s/verylongsubstitutionpattern/pancake/g
52  his :
53  wq
```

如果您当前任务是执行 `s/verylongsubstitutionpattern/donut/g`（"pancake"换成了"donut"），为什么不复用 `s/verylongsubstitutionpattern/pancake/g` 呢？毕竟，两条命令唯一不同的是替换的单词，"donut" vs "pancake" ，所有其他的内容都是相同的。

当您运行 `q:`后，在历史记录中找到 `s/verylongsubstitutionpattern/pancake/g`（在这个环境中，您可以使用Vim导航），然后直接编辑它！ 在历史记录窗口中将 "pancake" 改为 "donut" ，然后按 `<Enter`。Vim立刻执行 `s/verylongsubstitutionpattern/donut/g` 命令，超级方便！

类似地，运行 `:his /` 或 `:his ?` 可以查看搜索记录。要想打开您可以直接搜索和编辑的搜索历史记录窗口，您可以运行 `q/` 和 `q?`。

要退出这个窗口，按 `Ctrl-c`, `Ctrl-w c`, 或输入 `:quit`。

## 更多命令行指令

Vim有几百个内置指令，要查看Vim的所有指令，执行 `:h ex-cmd-index` 或 `:h :index`。

## 聪明地学习命令行模式

对比其他三种模式，命令行模式就像是文本编辑中的瑞士军刀。寥举几例，您可以编辑文本、修改文件和执行命令。本章是命令行模式的零碎知识的集合。同时，Vim 模式的介绍也走向尾声。现在，您已经知道如何使用普通、输入、可视以及命令行模式，您可以比以往更快地使用 Vim 来编辑文本了。

是时候离开 Vim 模式，来了解如何使用 Vim 标记进行更快的导航了。
