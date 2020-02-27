# chalk

`chalk` 是一个终端字符串样式处理库，可以自定义输出到终端的字符串的样式（颜色、背景色等）。  

## 安装
```sh
yarn add chalk
```

## 用法
```js
const chalk = require('chalk')

console.log(chalk.blue('hello world'))
```  

### 更多用法
```js
const chalk = requrie('chalk')
const log = console.log

// 组合使用有样式的字符串和普通字符串
log(chalk.bule('hello') + ' world ' + chalk.red('!'))

// 链式调用API
log(chalk.blue.bgRed.bold('Hello world!'))

// 传入多个参数
log(chalk.blue('hello', ' hi', ' world', ' yuusha'))

// API嵌套调用
log(chalk.blue('hello ', chalk.yellow.bgRed('world')))

// 支持模板字符串写法
log(`
CPU: ${chalk.green('30%')}
RAM: ${chalk.red('80%')}
DISK: ${chalk.yellow('60%')}
`)

// *在支持RGB颜色的终端中使用RGB色
log(chalk.keyword('orange')('Yay for orange colored text!'))
log(chalk.rgb(123, 45, 67).underline('Underlined reddish color'))
log(chalk.hex('#DEADED').bold('Bold gray!'))
```  

### 定义自己的主题
```js
const chalk = require('chalk')

const error = chalk.bold.red
const warning = chalk.keyword('orange')

console.log(error('Error!'))
console.log(warning('Warning!'))
```

### 利用console.log的替代字符串
```js
const name = 'yuusha'

console.log(chalk.blue('hello, %s'), name)
```  

## APIs
### chalk.style[.style](string[, string])
例如： `chalk.blue.bgRed.underline('hello', 'world')`  
> 多个参数会自动以空格分隔。  

### chalk.enabled
可以通过此属性设置是否支持颜色显示，默认为 true。
> 如果需要在可重用的模块中使用，可以创建一个新的实例：  
> ```js
> const ctx = new chalk.constructor({enabled: false})
> ```

### chalk.level
终端的颜色支持会自动进行检测，但是可以通过 `chalk.level` 进行手动设置。  
level:  
- 0: 关闭颜色支持
- 1: 16色
- 2: 256色
- 3: 1600万色

> 如果需要在可重用的模块中使用，可以创建一个新的实例：  
> ```js
> const ctx = new chalk.constructor({level: 0})
> ```

### chalk.supportsColor
通过该属性可以查看终端的颜色支持情况。  

## 常用样式
### 修饰符
- reset
- bold
- underline
- inverse(反色)

### 颜色
- black
- red
- green
- yellow
- blue
- gray
- cyan(青色)
- magenta(品红)
- white

### 背景色
- bgBlack
- bgRed
- bgGreen
- bgYellow
- bgBlue
- bgMagenta
- bgCyan
- bgWhite