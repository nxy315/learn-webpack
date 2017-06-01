# webpack

现在的脚手架工具虽然很方便，但是当自己想改一些配置文件的时候，就感觉无从下手，我觉得还是要从根本上会自己写配置文件

## 优点

- ***模块化***，可以把css，js都打包成模块
- stylus，sass等css预处理器
- 压缩图片，热加载页面
- 能打包不同类型的文件
- ...

## 安装

```bash
~$ sudo npm install webpack -g

~$ webpack init
```

## webpack工作方式

通过入口文件（比如index.js）找到项目所有的依赖文件，使用一些工具处理一下，最后打包为一个浏览器可识别的**js文件**

## webpack基础命令

```bash
~$ webpack 入口文件(index.js) 出口文件(build.js)
```

只需要指定一个入口文件，webpack将自动识别项目所依赖的其他文件

## 通过配置文件使用webpack

这个配置文件其实也是一个简单的js模块，可以把所有与构建相关的信息放在里面

```js
module.exports = {
  entry: __dirname + "index.js",//指定入口文件
  output: {
    path: __dirname + "/build",//打包后的js文件所在的文件夹
    filename: "build.js"//输出的文件名
  }
}
```

```bash
~$ webpack
```

>这时只要运行webpack命令，他就会根据webpack.config.js文件中的配置来打包项目

## 不同的打包

- **source-map**： 在一个单独的文件中生成一个完整且功能完全的文件，但是打包速度最慢
- **cheap-module-source-map**： 在一个单独的文件中生成一个不带映射的map，速度会快点
- **eval-source-map**： 可以在不影响构建速度的前提下生成完整的sourcemap，但是打包后输出的js文件具有性能和安全隐患。在开发阶段是一个非常好的选项，生产阶段一定不要用这个选项
- **cheap-module-eval-source-map**： 最快生成source map的方法，没有列映射（开发的时候一般选这个）

```js
module.exports = {
  devtool: 'cheap-module-eval-source-map',
  entry: xxx,
  output: {
    path: xxx,
    filename: xxx
  }
}
```

## 构建本地服务器

```bash
~$ npm install --save-dev webpack-dev-server
```

**devserver** 具有一下配置选项

- **contentBase**: 默认webpack-dev-server会为根文件提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在的目录
- **port**： 设置默认监听端口
- **inline**： 设置为`true`,当源文件改变时会自动刷新页面
- **colors**： 设置为`true`,是终端输出的文件为彩色的
- **historyApiFallback**： 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为`true`,所有的跳转将指向index.HTML5

```js
module.exports = {
  devtol : 'cheap-module-eval-source-map',

  entry: xxx,
  output: {
    path: xxx,
    filename: xxx
  },

  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    colors: true,
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
  }
}
```

## Loaders

通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种格式的文件进行处理（stylus，sass，es6）

Loaders需要在webpack.config.js下的modules关键字下进行配置

- **test**: 所处理文件的拓展名的正则表达式（必须）
- **loader**： loader的名称（必须）
- **include/exclude**： 手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
- **query**： 为loaders提供额外的设置选项（可选）

```js
function resolve (dir) {
  return path.join(__dirname, '..', dir)
}
module.exports = {
  ......
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}
```

>webpack 会自动调用.babelrc里的babel配置选项
