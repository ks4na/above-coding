# prettier
 [prettier](https://prettier.io/) 是一个代码格式化程序，保证所有输出的代码符合一致的风格。

## 安装
```sh
yarn add -D --exact prettier
```
> 官网推荐安装确切版本的 `prettier` （即使用 `--save-exact` 参数），因为会在补丁版本中引入风格变化。

## 创建配置文件
可以在 `package.json` 中定义 `prettier` 属性，也可以创建 `.prettierrc.js` 或 `prettier.config.js` 配置文件。  
```js
// .prettierrc.js

module.exports = {
  // "printWidth": 80,          // 每行最大字符数， 默认80
  // "tabWidth": 2,             // 缩进长度，默认： 2个字符
  "semi": false,                // 行尾是否添加分号，默认： true
  "singleQuote": true,           // 是否使用单引号，默认： false
  // bracketSpacing: true,      // 对象大括号内部是否有空格，默认： true【效果: { age: 123 }】
}
```

## 常用命令参数
`--write` ：  
格式化所有处理的文件，并覆盖该文件。

`--check`：  
检测指定的文件是否已格式化，如果有不符合的文件会将文件名输出在命令行。  

## 运行prettier
```sh
# --check检查格式不正确的文件
npx prettier src/**/*.js --check

# --write 参数会自动将文件格式化
npx prettier src/**/*.css --write
```

## prettier忽略文件
可以创建 `.prettierignore` 文件来设置忽略文件列表，用法同 `.gitignore` 。

> 尝试过使用 `eslint` 集成 `prettier:recommended` ，和自己习惯的配置不太相同，所以放弃。

