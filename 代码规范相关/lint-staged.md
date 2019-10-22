# lint-staged
代码风格检查（Code Linting，简称 Lint）是保障代码规范一致性的重要手段，常见做法是使用 `husky` 或者 `pre-commit` 在本地提交之前做 Lint。  
但是在遗留代码仓库上开启 Lint 初期，可能会面临成千上万的 Lint Error 需要修复，无疑是很难受的。 而如果能每次只 Lint 本次提交所修改的文件，就不必去解决遗留代码的问题。  
`lint-staged` 可以解决这个问题，每次提交只去 Lint git中暂存区（stage）的代码。通常结合 `husky` 使用，在 `pre-commit` 钩子函数时调用。

- [安装](#安装)
- [配置](#配置)
  - [package.json中配置](#packagejson中配置)
- [补充](#补充)

## 安装
```sh
yarn add -D lint-staged husky
```

## 配置
### package.json中配置
在 `package.json` 中添加 `lint-staged` 字段：   
```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
   "lint-staged": {
    "*": "prettier --check",
    "*.(js|jsx)": "eslint"
  },
}
```
> `husky` 的 `pre-commit` 中直接调用 `lint-staged` 命令，  
> `lint-staged` 针对 `.js` 和 `.css` 文件执行不同的命令，主要执行检查操作而不是自动修复操作，不通过则放弃本次提交并提示存在问题的文件。

## 补充
不必担心提交到git时有引用的库导致预检不通过的问题。  
例如 `/src/lib/` 下存放所有手动下载的库，而 `/src/lib` 是需要 git 来管理的，提交时如何避免 `prettier` 和 `eslint` 对其中代码进行检测？  
解决方法是通过 `.prettierignore` 和 `.eslintignore` 来忽略 `/src/lib/` 。
