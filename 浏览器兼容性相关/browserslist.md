# browserslist

- [简介](#%e7%ae%80%e4%bb%8b)
- [最佳实践](#%e6%9c%80%e4%bd%b3%e5%ae%9e%e8%b7%b5)
- [可用的查询语句](#%e5%8f%af%e7%94%a8%e7%9a%84%e6%9f%a5%e8%af%a2%e8%af%ad%e5%8f%a5)
- [Debug](#debug)
- [不同环境下的 browserslist 配置](#%e4%b8%8d%e5%90%8c%e7%8e%af%e5%a2%83%e4%b8%8b%e7%9a%84-browserslist-%e9%85%8d%e7%bd%ae)
  - [测试不同环境下的获取到的结果](#%e6%b5%8b%e8%af%95%e4%b8%8d%e5%90%8c%e7%8e%af%e5%a2%83%e4%b8%8b%e7%9a%84%e8%8e%b7%e5%8f%96%e5%88%b0%e7%9a%84%e7%bb%93%e6%9e%9c)

## 简介

供多个不同的前端工具查找 `浏览器或nodejs的构建目标版本` 的库，被很多库所使用：

- `AutoPrefixer`
- `Babel`
- `eslint-plugin-compat`
- `postcss-preset-env`
- ...

所有使用 `browserslist` 的工具包都可以自动获取到 `浏览器或nodejs的构建目标版本`，前提是添加 `browserslist` 配置信息，两种方法可以添加：

1. 在 `package.json` 中定义 `browserslist` 字段（推荐）：

   **package.json**

   ```json
   {
     "browserslist": ["defaults", "> 0.2%", "ie 10"]
   }
   ```

2. 定义 `.browserslistrc` 文件：

   ```txt
   # Browsers that we support

   defaults
   > 0.2%
   ie 10
   ```

## 最佳实践

大部分情况下, `browserslist` 指定为 `defaults` 即可(即：`> 0.5%, last 2 versions, Firefox ESR, not dead`)。

> 使用 `stylelint-no-supported-browser-features` 插件时，必须设置上 `not op_mini all`，因为 `op_mini` 在 [caniuse](https://caniuse.com/) 上很多属性未知，导致有报错提示。

## 可用的查询语句

点击查看[全部可用的查询语句](https://github.com/browserslist/browserslist#full-list)

## Debug

[在线查询的网址](https://browserl.ist/)

项目目录下使用 `npx browserslist` 命令也可以查看指定的查询语句所包含的浏览器列表：

```sh
npx browserslist

and_chr 80
and_ff 68
and_qq 1.2
and_uc 12.12
android 80
baidu 7.12
chrome 80
chrome 79
edge 80
edge 79
edge 18
firefox 73
firefox 72
firefox 68
ie 11
ios_saf 13.3
ios_saf 13.2
ios_saf 13.0-13.1
ios_saf 12.2-12.4
kaios 2.5
op_mini all
op_mob 46
opera 66
opera 65
safari 13
safari 12.1
samsung 11.1
samsung 10.1
```

## 不同环境下的 browserslist 配置

可以根据不同的环境指定不同的查询语句，`browserslist` 会根据 `NODE_ENV` 或 `BROWSERSLIST_ENV` 环境变量来选择查询语句。  
如果未指定环境变量，会首先查找 `production` 对应的查询语句，如果没有，则使用默认值 `defaults`。

```json
{
  "browserslist": {
    "production": ["defaults", "> 1%", "ie 10"],
    "modern": ["last 1 chrome version", "last 1 firefox version"],
    "ssr": ["node 12"]
  }
}
```

### 测试不同环境下的获取到的结果

先安装 `cross-env` ，屏蔽各个平台设置环境变量的写法差异：

```sh
yarn add -D cross-env
```

然后分别设置 `BROWSERSLIST_ENV` 为 `production` 、 `modern` 和 `ssr` ，查看查询结果：

```sh
# production
npx cross-env BROWSERSLIST_ENV=production browserslist

and_chr 80
and_ff 68
and_qq 1.2
and_uc 12.12
android 80
baidu 7.12
chrome 80
chrome 79
edge 80
edge 79
edge 18
firefox 73
firefox 72
firefox 68
ie 11
ie 10
ios_saf 13.3
ios_saf 13.2
ios_saf 13.0-13.1
ios_saf 12.2-12.4
kaios 2.5
op_mini all
op_mob 46
opera 66
opera 65
safari 13
safari 12.1
samsung 11.1
samsung 10.1

# modern
npx cross-env BROWSERSLIST_ENV=modern browserslist

chrome 80
firefox 73

# ssr
npx cross-env BROWSERSLIST_ENV=ssr browserslist

node 12.16.0
```
