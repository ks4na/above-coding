# git 多平台换行符统一

> 参考自[https://juejin.cn/post/6844903591258357773](https://juejin.cn/post/6844903591258357773)

- [背景](#背景)
- [解决方案](#解决方案)
  - [配置 git 全局参数](#配置-git-全局参数)
  - [使用`.gitattributes`配置文件（推荐）](#使用gitattributes配置文件推荐)
- [补充](#补充)
  - [对项目中已有文件转换换行符](#对项目中已有文件转换换行符)

## 背景

首先在不同操作系统中，换行符并不统一，Linux 系统中使用 `0x0A（LF）`, windows 系统中使用 `0x0D0A（CRLF）`, 而 MAC OS 系统起初使用 `0x0D（CR）` 后来和 Linux 系统保持一致。而 git 默认采用 Linux 的换行符（当然这一点并不奇怪）。  
团队合作开发时很容易发生多平台行尾符不统一的情况。

## 解决方案

### 配置 git 全局参数

git 关于换行符的配置项有三个：

- `eol`：设置文件换行符，有三个值 lf、crlf、native ，默认同操作系统
- `autocrlf`：
  - true：检出时转换为 crlf， 提交时转换为 lf
  - input：检出时不转换，提交时转换为 lf
  - false： 不作转换
- safecrlf：
  - true：不允许提交包含不同换行符的文件
  - warn：提交文件中有不同换行符时警告
  - false：允许提交包含不同换行符的文件

可以对这三个参数进行全局配置：

```sh
# 统一换行符为 lf
git config --global core.eol lf
# 检出时不转换，提交时转换为 lf
git config --global core.autocrlf input
# 不允许提交饱含不同换行符的文件
git config --global core.safecrlf true
```

### 使用`.gitattributes`配置文件（推荐）

虽然可以通过 git 全局配置的方式统一换行符，但是对于团队协作项目来说，并不能保证所有人都正确配置。  
git 提供了 `.gitattributes` 配置文件解决了这个问题，并且可以对不同文件进行更细粒度的换行符控制。

在项目根目录创建 `.gitattributes` 文件并添加到版本控制中：

```sh
# 设置默认换行符转换行为
* text eol=lf
```

## 补充

### 对项目中已有文件转换换行符

使用 `dos2unix` 工具（windows 上 `git bash` 自带）批量转换项目中包含 `crlf` 换行符的文件。

以转换 `java` 文件为例：

```sh
cd <project-root-path>

find . -type file -name '*.java' -exec dos2unix {} +
```
