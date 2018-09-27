# -webpack打包体积优化-

### 项目背景
1. 最近朋友的一个私人管理端项目打包体积过大，逻辑不复杂，代码量也不是很多，但是打包体积达到了2.8M之多。
2. 所以帮忙给朋友看了一下， 简单的记录了一下优化流程。

### Let`s do it
> * bundle 未丑化（压缩混淆）
>   > `
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
`

