# -webpack打包体积优化-

### 项目背景
1. 最近朋友的一个私人管理端项目打包体积过大，逻辑不复杂，代码量也不是很多，但是打包体积达到了2.8M之多。
2. 所以帮忙给朋友看了一下， 简单的记录了一下优化流程。

### Let`s do it
> * 1.bundle 未丑化（压缩混淆）
>   >
```
    ...
    //引入webpack
    var webpack = require('webpack');
    //添加插件
    plugins:[
            ...

            new webpack.optimize.UglifyJsPlugin({
                warnings:false,
                //是否美化输出代码
                beautify: false,
                output: {
                    comments: false,
                },
                compress:{
                    //合并连续 var 声明
                    join_vars:true,
                    //当删除没有用处的代码时，显示警告
                    warnings:false,
                    //优化某些变量实际上是按常量值来赋值、使用的情况。
                    reduce_vars: true,
                },
                toplevel:false,
                ie8:false
            }),
        ],
```

> * 2.react未启用生产版本
>   >
```
    ...
    //添加插件启用生产版本react
    plugins:[
            ...
             new webpack.DefinePlugin({
                        'process.env.NODE_ENV': JSON.stringify('production')
                    }),
        ],
```

> * 3.添加打包分析插件，分析bundle组成
>   >
```
    //npm install --save-dev webpack-bundle-analyzer
    const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
    ...
    //添加分析插件
    plugins:[
            ...
             new BundleAnalyzerPlugin(),
        ],
    //在运行打包命令时 会监听127.0.0.1:8888，可查看具体bundle组成

```

> * 4.拆分样式文件
>   >
```
    //npm install extract-text-webpack-plugin --save-dev
    const ExtractTextPlugin = require('extract-text-webpack-plugin');
    ...
    //添加插件
    module: {
            rules: [
               ...
                {
                    test: /\.less$/,
                    use: ExtractTextPlugin.extract({
                        fallback: 'style-loader',
                        use: [{loader:'css-loader'}, {loader:'less-loader'}],
                    }),
                },
            ],
        },
   //在优化了前两项之后 bundle在1.04m，运行分析后发现项目里用了jquery的$.ajax，- - mmp ，
   //将jquery包去除引用 最终bundle+css文件大小在 800K左右。
```
### ...待续

