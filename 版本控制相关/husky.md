# husky

[husky](https://github.com/typicode/husky/blob/master/DOCS.md) 用于方便地设置Git hooks。 

- [安装 huksy](#安装-huksy)
- [配置](#配置)
  - [在package.json中配置](#在packagejson中配置)
  - [使用单独的 husky 配置文件](#使用单独的-husky-配置文件)
- [husky同时运行多条命令](#husky同时运行多条命令)

使用场景：
- `git commit` 之前通过钩子函数，配合其它插件，做一些代码格式化、代码校验工作。

## 安装 huksy
```sh
yarn add -D husky
```
> 注意： `husky` 安装后会自动将钩子函数写入 `.git/hooks` 中，所以安装 `husky` 之前必须就已经是一个git仓库。

## 配置
### 在package.json中配置
```json
{
  // ...
  "husky": {
    "hooks": {
      "pre-commit": "echo husky works"
    }
  }
}
```

### 使用单独的 husky 配置文件
husky配置文件的命名可以为  `.huskyrc.js`, `.huskyrc.json`, `.huskyrc` .  
```js
// .huskyrc.js

module.exports = {
  'hooks': {
    'pre-commit': 'echo .huskyrc.js works'
  }
}
```

## husky同时运行多条命令
husky运行多条命令的方式类似 `package.json` 中的 `scripts` 中的写法：  
```js
{
  'pre-commit': 'echo 1 && echo 2 && echo 3'
}
```
为了命令的可读性，可以使用一个函数来拼接命令：  
```js
// .huskyrc.js

const tasks = arr => arr.join(' && ')

module.exports = {
  'hooks': {
    'pre-commit': tasks([
      'echo 1',
      'echo 2',
      'echo 3'
    ])
  }
}
```