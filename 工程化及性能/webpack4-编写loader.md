# Webpack4编写loader

> 核心参考: [官方文档-编写一个loader](https://webpack.docschina.org/contribute/writing-a-loader/#%E7%AE%80%E5%8D%95-simple-)
>
> 为什么，已经有个这个文档，还要在这里写？因为 文档虽然比较全，但感觉有点乱。自己尝试在大神的基础上，再进行整理。（站在巨人肩膀上）

## 0. Loader的基本知识

#### 1. 作用

1. Loader 本质上是对资源文件进行转换处理。
2. 本身是一个node模块

#### 2. 怎么用

###### 简单配置

```javascript
//webpack 配置
const webpackConfig = {
    //这些选项决定了如何处理项目中的不同类型的模块。
    module: {
        rules: [{
            test: /\.js$/,
            use: [{
                //这里写 loader 的路径
                loader: path.resolve(__dirname, 'loaders/a-loader.js'), 
                options: {/* ... */}
            }]
        }]
    }
}
```



###### 进阶使用

#### 3. Loader解析规则

###### 哪里解析规则？

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/, //这里解析规则
        use: [
         //...
        ]
      }
    ]
  }
};
```



###### 具体解析规则

> oader 遵循标准的[模块解析](https://webpack.docschina.org/concepts/module-resolution/)。多数情况下，loader 将从[模块路径](https://webpack.docschina.org/concepts/module-resolution/#module-paths)（通常将模块路径认为是 `npm install`, `node_modules`）解析。 

1. 绝对路径

2. 相对路径

3. 模块路径

   ```javascript
   import 'module';
   import 'module/lib/file';
   ```

   模块将在 [`resolve.modules`](https://webpack.docschina.org/configuration/resolve/#resolve-modules) 中指定的所有目录内搜索 

## 1. 本地开发方法

​	本地开发，主要是解决`Webpack` 能找到自定义的loader

#### 1. 使用绝对地址

   node [path.resolve()](http://nodejs.cn/api/path.html#path_path_resolve_paths)——核心是从右往左，直到构造完成一个绝对路径 （以‘/’开头的是绝对地址）

   ```javas
   module.exports = {
     //...
     module: {
       rules: [
         {
           test: /\.js$/,
           use: [
             {
               loader: path.resolve('path/to/loader.js'),
               options: {/* ... */}
             }
           ]
         }
       ]
     }
   };
   ```

#### 2. 多个loaders

​	则使用`resolveLoader.modules `

#### 3. 独立的loader包， 

​	则 [`npm link`](https://docs.npmjs.com/cli/link)

   	总之，一般都使用 `npm link`,最终一般都是以`npm`包的形式进行引用

## 2. 创建loader的原则

​	loader应该遵循的准则



## 3.  使用工具

> webpack 本身提供了一系列工具，供使用者来创建loader。
>
> 主要分为两种，loaderAPI 及 loader工具库，就从这两方面来处理

#### 1. loader API 

​	[loader API官方文档](https://webpack.docschina.org/api/loaders/#this-adddependency) ，可以简单理解：

1. loader上下文 ，也就是 webpack在运行处理loader时，给`this`中注入的一些列方法
2. loader 调用语法规则



#### 2. loader 工具库(Loader Utilities)

[参考](https://webpack.docschina.org/contribute/writing-a-loader/#loader-%E5%B7%A5%E5%85%B7%E5%BA%93-loader-utilities-)