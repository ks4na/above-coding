# .gitattributes 介绍

- [背景](#背景)
- [介绍](#介绍)
- [常用的属性](#常用的属性)
  - [text](#text)
  - [eol](#eol)
- [示例配置](#示例配置)
  - [常见配置方式](#常见配置方式)
- [参考](#参考)

## 背景

之前写过一篇笔记，配置 git 的 `eol、autocrlf、safecrlf` 配置来控制行尾符，但是这种配置方式对于多人合作并不友好，并不能保证每个人都对 git 进行同样的配置。

而通过在项目中指定 `.gitattributes` 文件，可以针对该项目指导 git 如何处理行尾符。

## 介绍

`.gitattrigutes` 文件用于指导 git 对指定文件的管理方式，最常用的功能是统一项目中文件的行尾符。

`.gitattributes` 文件格式如下：

```
pattern attr1 attr2 ...
# 要匹配的文件模式 属性1 属性2 ...
# 示例：
# *.js text eol=lf
# *.jpg binary
```

其中， 属性字段可能有 4 种状态， 以 `text` 属性为例：

- `text`
- `-text`
- `text=xxx`
- 未指定（通常不出现该属性即为未指定），有时需要强制覆盖为未指定，需要写作 `!text`

## 常用的属性

### text

控制文件行尾的规范性。

- `text`: 指导 git 将其视为文本文件，且启用自动行尾符转换，转换为 `lf`
- `-text`: 指导 git 不进行行尾符转换
- `text=auto`: 指导 git 自动推测该文件是否为文本，如果为文本将自动进行行尾符转换，转换为 `lf`；
  - **对于已提交为 `crlf` 的文件不会转换**
- 未指定：如果未指定，git 将使用 `core.autocrlf` 来决定是否转换行尾符

### eol

强制指定行尾符。只有在 `text` 属性为 `Set、Specified、auto` 时才会生效。

示例：

```
*.vcproj	  text eol=crlf
*.sh		    text eol=lf
```

## 示例配置

```
*           text=auto
*.txt		    text
*.vcproj	  text eol=crlf
*.sh		    text eol=lf
*.jpg		    -text
```

### 常见配置方式

参考部分主流开源项目，通常项目中只需要以下通用配置：

```
*	        text=auto
```

## 参考

- [.gitattributes 官方文档](https://git-scm.com/docs/gitattributes)
- [github handle line endings](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings#per-repository-settings)
- [.gitattributes 作用详细讲解](https://juejin.cn/post/7120037275521515528)
