# eslint 集成 typescript,react,prettier

## eslint 集成 typescript

使用 `eslint` 对 `typescript` 代码进行检查，首先安装依赖：

```sh
yarn add -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

- @typescript-eslint/parser:
  - 用于解析 typescript 代码
- @typescript-eslint/eslint-plugin:
  - 包含定义好的检查 typescript 代码的规范

然后配置 `.eslintrc.js`：

```js
module.exports = {
  env: {
    browser: true,
    node: true
  },
  parser: '@typescript-eslint/parser', // 指定eslint的解析器
  extends: [
    'plugin:@typescript-eslint/recommended' // 使用 @typescript-eslint/eslint-plugin 推荐的规则
  ],
  parserOptions: {
    ecmaVersion: 2018, // 指定需要支持的es版本
    sourceType: 'module' // 指定类型为模块
  },
  rules: {
    // 指定规则，可以覆盖推荐的规则
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  }
}
```

## eslint 集成 react

先安装依赖：

```sh
yarn add -D eslint-plugin-react
```

然后配置 `.eslintrc.js`:

```js
module.exports = {
  env: {
    browser: true,
    node: true
  },
  parser: '@typescript-eslint/parser',
  extends: [
    'plugin:react/recommended', // 使用 eslint-plugin-react 推荐的规则
    'plugin:@typescript-eslint/recommended'
  ],
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true // 允许编译 jsx 代码
    }
  },
  rules: {},
  settings: {
    react: {
      version: 'detect' // 告诉 eslint-plugin-react 自动检测使用的react版本
    }
  }
}
```

## eslint 集成 prettier

先安装依赖：

```sh
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier
```

- eslint-config-prettier:
  - 禁用可能会与 prettier 有冲突的 eslint 规则
- eslint-plugin-prettier:
  - 使用 prettier 的规则

然后添加 prettier 的配置文件 `.prettierrc.js`:

```js
module.exports = {
  semi: false,
  trailingComma: 'es5',
  singleQuote: true,
  printWidth: 80,
  tabWidth: 2
}
```

再修改 `.eslintrc.js`：

```js
module.exports = {
  env: {
    browser: true,
    node: true
  },
  parser: '@typescript-eslint/parser',
  extends: [
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    // 使用 eslint-config-prettier 来禁用 @typescript-eslint/eslint-plugin 中可能与prettier冲突的规则
    'prettier/@typescript-eslint', 
    // 【确保这一项是extends数组的最后一项】启用 eslint-plugin-prettier 和 eslint-config-prettier。这会将prettier错误作为eslint错误来显示
    'plugin:prettier/recommended' 
  ],
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true
    }
  },
  rules: {},
  settings: {
    react: {
      version: 'detect'
    }
  }
}
```
