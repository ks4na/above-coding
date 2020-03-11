# stylelint

- [介绍](#%e4%bb%8b%e7%bb%8d)
- [使用](#%e4%bd%bf%e7%94%a8)
  - [1.安装](#1%e5%ae%89%e8%a3%85)
  - [2.创建配置文件](#2%e5%88%9b%e5%bb%ba%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6)
  - [3.检测样式文件](#3%e6%a3%80%e6%b5%8b%e6%a0%b7%e5%bc%8f%e6%96%87%e4%bb%b6)
- [忽略规则](#%e5%bf%bd%e7%95%a5%e8%a7%84%e5%88%99)
  - [1.忽略文件](#1%e5%bf%bd%e7%95%a5%e6%96%87%e4%bb%b6)
  - [2.行内注释](#2%e8%a1%8c%e5%86%85%e6%b3%a8%e9%87%8a)
- [添加 pre-commit 检查](#%e6%b7%bb%e5%8a%a0-pre-commit-%e6%a3%80%e6%9f%a5)
- [集成 prettier](#%e9%9b%86%e6%88%90-prettier)
  - [安装依赖](#%e5%ae%89%e8%a3%85%e4%be%9d%e8%b5%96)
  - [修改配置文件](#%e4%bf%ae%e6%94%b9%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6)
- [补充：vscode 配置 stylelint](#%e8%a1%a5%e5%85%85vscode-%e9%85%8d%e7%bd%ae-stylelint)
  - [安装 stylelint 插件](#%e5%ae%89%e8%a3%85-stylelint-%e6%8f%92%e4%bb%b6)
  - [配置 stylelint 检查样式文件](#%e9%85%8d%e7%bd%ae-stylelint-%e6%a3%80%e6%9f%a5%e6%a0%b7%e5%bc%8f%e6%96%87%e4%bb%b6)

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
2. 行内注释

### 1.忽略文件

可以创建 `.stylelintignore` 文件来指定需要忽略的文件：

**.stylelintignore**

```txt
vendors/**/*.css

src/ignore.css
```

### 2.行内注释

类似于 `eslint` 行内注释的用法：

```css
/* stylelint-disable selector-no-id, declaration-no-important */
#id {
  color: pink !important;
}
/* stylelint-enable selector-no-id, declaration-no-important */
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
    "lint": "yarn run type-check && yarn run eslint-check && yarn run stylelint-check"
  }
}
```

## 集成 prettier

### 安装依赖

```sh
yarn add -D stylelint-config-prettier stylelint-prettier
```

- stylelint-config-prettier: 禁用可能会与 prettier 冲突的规则
- stylelint-prettier: 将 prettier 报错信息作为 stylelint 错误

### 修改配置文件

将 `"stylelint-prettier/recommended"` 添加到 extends 数组中，并确保放在最后一项。

**.stylelintrc.js**

```js
module.exports = {
  extends: ['stylelint-prettier/recommended'],
}
```

## 补充：vscode 配置 stylelint

### 安装 stylelint 插件

搜索 `stylelint` 插件并下载

### 配置 stylelint 检查样式文件

到 vscode 的设置中禁用 `css/scss/less` 的检查，然后启用 `stylelint` 的检查。
