# stylelint

- [介绍](#%e4%bb%8b%e7%bb%8d)
- [使用](#%e4%bd%bf%e7%94%a8)
  - [1.安装](#1%e5%ae%89%e8%a3%85)
  - [2.创建配置文件](#2%e5%88%9b%e5%bb%ba%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6)
  - [3.检测样式文件](#3%e6%a3%80%e6%b5%8b%e6%a0%b7%e5%bc%8f%e6%96%87%e4%bb%b6)
- [忽略规则](#%e5%bf%bd%e7%95%a5%e8%a7%84%e5%88%99)
  - [1.忽略文件](#1%e5%bf%bd%e7%95%a5%e6%96%87%e4%bb%b6)
- [添加 pre-commit 检查](#%e6%b7%bb%e5%8a%a0-pre-commit-%e6%a3%80%e6%9f%a5)

## 介绍

类似于 `eslnit` 的 linter，用于对样式文件(`css`,`less`,`sc(a)ss`)进行检查。

## 使用

### 1.安装

```sh
# 安装 stylelint 和  stylelint standard 配置
yarn add -D stylelint stylelint-config-standard
```

### 2.创建配置文件

> `stylelint` 配置文件可以有多种命名（如： `.stylelintrc.js`， `stylelint.config.js` 等）。

**.stylelintrc.js**

```js
module.exports = {
  extends: ['stylelint-config-standard'],
  // rules 用于配置规则，全部规则见： https://stylelint.io/user-guide/rules/list
  rules: {},
}
```

### 3.检测样式文件

如： 检测 src 目录下所有 css 文件：

```sh
npx stylelint src/**/*.css
```

## 忽略规则

类似于 `eslint` 的忽略方式， `stylelint` 也有两种忽略规则的方式：

1. 忽略文件
2. 注释方式

### 1.忽略文件

可以创建 `.stylelintignore` 文件来指定需要忽略的文件：

**.stylelintignore**

```txt
vendors/**/*.css

src/ignore.css
```

## 添加 pre-commit 检查

在 `lint-staged` 属性中针对样式文件，添加 `stylelint` 检查：

```json
{
  "lint-staged": {
    "src/**/*.{css,scss,sass,less}": "stylelint"
  }
}
```

另外， `scripts` 中的 `lint` 命令也需要添加上 `stylelint-check` 命令：

```json
{
  "scripts": {
    "type-check": "tsc --noEmit",
    "eslint-check": "eslint ./src --ext .{js,jsx,ts,tsx}",
    "stylelint-check": "stylelint ./src",
    "lint": "yarn run type-check && yarn run eslint-check"
  }
}
```
