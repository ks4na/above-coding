# obsolete-webpack-plugin

- [介绍](#%e4%bb%8b%e7%bb%8d)
- [使用](#%e4%bd%bf%e7%94%a8)
  - [1.安装](#1%e5%ae%89%e8%a3%85)
  - [2.配置](#2%e9%85%8d%e7%bd%ae)
  - [3.效果展示](#3%e6%95%88%e6%9e%9c%e5%b1%95%e7%a4%ba)
- [修改提示信息](#%e4%bf%ae%e6%94%b9%e6%8f%90%e7%a4%ba%e4%bf%a1%e6%81%af)

## 介绍

`obsolete-webpack-plugin` 是一个 `webpack` 插件，用于根据 `browserslist` 指定的构建目标，来自动为不是构建目标的浏览器添加提示信息。

## 使用

### 1.安装

```sh
yarn add -D obsolete-webpack-plugin
```

### 2.配置

更新 `webpack.config.js` ，添加 `obsolete-webpack-plugin` 插件：

```js
const ObsoleteWebpackPlugin = require('obsolete-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    // ...
    // 配置选项默认即可，参考 https://github.com/ElemeFE/obsolete-webpack-plugin#options
    new ObsoleteWebpackPlugin({
      /* options */
    }),
  ],
}
```

### 3.效果展示

使用非构建目标的浏览器打开后，会有如下提示：

![obsolete-webpack-plugin](media/oboslete-webpack-plugin.gif)

## 修改提示信息

可以向 `ObsoleteWebpackPlugin()` 方法参数对象中传入 `template` 属性，来修改默认的提示信息， 参见 [选项](https://github.com/ElemeFE/obsolete-webpack-plugin#options)。

默认的提示信息为： `'<div>Your browser is not supported. <button id="obsoleteClose">&times;</button></div>'`
