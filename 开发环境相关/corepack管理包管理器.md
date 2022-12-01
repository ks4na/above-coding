# corepack 管理包管理器

- [简介](#简介)
- [使用](#使用)
  - [启用 corepack](#启用-corepack)
  - [corepack 优势](#corepack-优势)
    - [1.自带 yarn 和 pnpm，节省安装时间](#1自带-yarn-和-pnpm节省安装时间)
    - [2.避免包管理器混用](#2避免包管理器混用)
- [参考](#参考)

<style>
  img {
    max-width: 300px;
  }
</style>

## 简介

corepack 提供除了 npm 之外的 yarn 和 pnpm 包管理器，并且可以限制项目中只允许使用特定的包管理器，避免包管理器混用。

## 使用

> corepack 默认与 nodejs 一起分发， nodejs 版本需要大于等于 `14.19.0` 或 `16.9.0`。

如下图所示，在 `.nvm/versions/node` 目录中， `12.22.12` 版本的 `bin` 目录中没有 `corepack` 命令，而 `14.19.3` 版本的 `bin` 目录中有 `corepack` 命令。

![corepack 命令](assets/bin_corepack.png)

### 启用 corepack

由于当前 `corepack` 处于实验阶段，所以需要手动启用。

先检查是否已经安装 `yarn` 和 `pnpm`：

![检查是否安装 yarn 或 pnpm](assets/check-yarn-and-pnpm.png)

输入以下命令启用 `corepack`：

```sh
corepack enable
```

启用后自动生成 `yarn` 和 `pnpm` 的快捷命令：

![启用corepack 后](assets/corepack-enabled.png)

此时就可以使用 `yarn` 和 `pnpm` 命令了，默认安装的版本是在该版本 nodejs 发版时最新的 `yarn` 和 `pnpm` 版本。

### corepack 优势

#### 1.自带 yarn 和 pnpm，节省安装时间

使用 `corepack enable` 后，无需下载即可使用 `yarn` 和 `pnpm`。

#### 2.避免包管理器混用

为了避免在项目中混用 `npm` 、`yarn`、`pnpm` 包管理器，可以使用 `corepack` 配合 `package.json > packageManager` 属性来指定包管理器。

例如： 在 `package.json` 中指定 `pnpm@7.3.0` ：

```json
{
  "name": "node-pnpm-project",
  "version": "0.0.0",
  "packageManager": "pnpm@7.3.0",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

使用 `yarn` 命令会抛出错误：

![使用非指定的包管理器会报错](assets/throw-error-when-using-wrong-pkgmgr.png)

但是默认情况下不会阻止 `npm` 命令，为了能让 npm 作为全局包管理器在任何项目完美运行。  
如果也想要拦截 `npm` ，需要使用以下命令：

```sh
corepack enable npm
```

> 包管理器版本与 `packageManager` 中指定的版本不匹配时，会静默下载匹配的版本并使用该版本。

## 参考

1. [Node.js Corepack](https://juejin.cn/post/7111998050184200199)
