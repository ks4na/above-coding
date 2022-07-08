# git commit 提交信息规范化

这篇笔记主要介绍使用 `commitizen` 和 `commitlint` 来规范化 git 提交信息。

- [前置步骤](#前置步骤)
- [安装与配置](#安装与配置)
- [进阶配置](#进阶配置)
  - [在 git commit 上应用 commitizen](#在-git-commit-上应用-commitizen)
  - [commitizen 结合 commitlint 一起使用](#commitizen-结合-commitlint-一起使用)
    - [1.安装并配置 commintlint](#1安装并配置-commintlint)
    - [2.配置适配器 @commitlint/cz-commitlint](#2配置适配器-commitlintcz-commitlint)
- [参考文档](#参考文档)

<style>
  img {
    max-width: 50%;
  }
</style>

## 前置步骤

初始化为 `git` 仓库和 `npm` 项目。

```sh
git init


npm init -y
```

## 安装与配置

> 以非全局安装的方式来安装和配置。

安装

```sh
npm i commitizen -D
```

配置

```sh
npx commitizen init cz-conventional-changelog --save-dev --save-exact

# 如果使用的是 yarn，使用下面的命令
# npx commitizen init cz-conventional-changelog --yarn --dev --exact
```

使用

```sh
npx cz

# 或者在 `package.json` 的 `scripts` 下添加命令 `"cm":"cz"`，然后 `npm run cm`
```

此时即可弹出提交信息选择询问信息。

> 为什么不在 `scripts` 中使用 `commit` 作为命令名而是使用 `cm`？
>
> - 因为当运行 `XXX` 命令时 npm 会自动执行 `preXXX` ，如果命名为 `commit` 且使用了 `husky` 且存在 `precommit` 命令，会导致运行 `precommit` 两次。

## 进阶配置

### 在 git commit 上应用 commitizen

> 为了避免提交时忘记使用 `npx cz` 来代替 `git commit` ，或者多人合作时其他人不知道使用 `npx cz` 来提交，可以直接在 `git commit` 命令上应用 `commitizen`，这样每次使用 `git commit` 命令时就可以直接弹出 `commitizen` 询问提示了。

> 如果想要跳过 `commitizen` 询问提示时，直接 `ctrl + c` 即可。

安装配置 husky

```sh
npm i husky -D

npx husky install
```

配置 `prepare-commit-msg` hook

```sh
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx cz --hook || true"
```

现在即可在输入 `git commit` 时出现 `commitizen` 询问提示了：

![commitizen on git commit](./media/commitizen-on-git-commit.png)

### commitizen 结合 commitlint 一起使用

#### 1.安装并配置 commintlint

> `commitizen` 只是给出规范提交信息的提示，并不能约束最终提交的信息必须符合规范，此时需要 `commitlint` 来对最终提交信息进行强制约束。

安装

```sh
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

配置

```sh
# Configure commitlint to use conventional config
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

添加 husky 的 hook

```sh
cat <<EEE > .husky/commit-msg
#!/bin/sh
. "\$(dirname "\$0")/_/husky.sh"

npx --no -- commitlint --edit "\${1}"
EEE
```

将其设置为可执行：

```sh
chmod a+x .husky/commit-msg
```

#### 2.配置适配器 @commitlint/cz-commitlint

但是有些时候项目的提交规范是自定义的， 此时为了避免 `commitizen` 和 `commitlint` 都配置一份，需要使用适配器 `@commitlint/cz-commitlint`，配置之后 `commitizen` 将会使用 `commitlint.config.js` 中配置的自定义提交规范。

> TODO - 配置步骤暂略，需要自定义时再记录。

## 参考文档

- [commitizen 文档](https://github.com/commitizen/cz-cli)
- [commitlint 文档](https://github.com/conventional-changelog/commitlint)
- [husky 文档](https://github.com/typicode/husky)
  - 特别注意： [为什么 husky 放弃传统的 js 配置方式](https://blog.typicode.com/husky-git-hooks-javascript-config/)
