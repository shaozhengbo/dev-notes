# Webpack整合Eslint

## 1. 安装eslint-loader

```shell
npm install --save-dev eslint-loader
```
## 2. 在 webpack.config.js 中添加如下代码：

```json
{
    test: /\.js$/,
    loader: 'eslint-loader',
    enforce: "pre",
    include: [path.resolve(__dirname, 'src')], // 指定检查的目录
    options: { // 这里的配置项参数将会被传递到 eslint 的 CLIEngine 
        formatter: require('eslint-friendly-formatter') // 指定错误报告的格式规范
    }
}
```
注：formatter默认是stylish，如果想用第三方的可以安装该插件，如上方的示例中的 eslint-friendly-formatter

## 3. 其次，要想webpack具有 eslint 的能力，就要安装eslint，命令如下：

```shell
npm install --save-dev eslint
```

## 4. 最后，项目想要使用那些eslin规则，可以创建一个配置项文件 ‘.eslintrc.js’，代码如下:

```js
module.exports = {
    root: true, 
    parserOptions: {
        sourceType: 'module'
    },
    env: {
        browser: true,
    },
    rules: {
        "indent": ["error", 2],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],
        "no-console": "error",
        "arrow-parens": 0
    }
}
```

eslintrc.js 的配置使用，详细细节请参考http://eslint.cn/docs/user-guide