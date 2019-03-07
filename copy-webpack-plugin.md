## copy-webpack-plugin

>作用：对资源进行拷贝，例如一些静态资源直接拷贝到打包后的文件夹中
```
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
    //作用：把static 里面的内容全部拷贝到编译目录
```
| 参数 | 作用 | 其他说明 |
| ------ | ------ | ------ |
| from | 定义要拷贝的源目录 | from: __dirname + '/src/public' |
| to | 定义要拷贝到的目标目录 | to: __dirname + '/dist' |
| ignore | 忽略拷贝指定的文件 | 可以用模糊匹配 |