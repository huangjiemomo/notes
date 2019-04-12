## HappyPack

>作用：我们需要Webpack 能同一时间处理多个任务，发挥多核 CPU 电脑的威力，HappyPack 就能让 Webpack 做到这点，它把任务分解给多个子进程去并发的执行，子进程处理完后再把结果发送给主进程。（提示：由于HappyPack 对file-loader、url-loader 支持的不友好，所以不建议对该loader使用。）

####安装
```
npm install --save-dev happypack
```
####使用
```
const os = require('os'),
  HappyPack = require('happypack'),
  happyThreadPool = HappyPack.ThreadPool({size: os.cpus().length - 1});
module.exports = [
  module: {
    {
      test: /\.vue$/,
      loaders: ['happypack/loader?id=vuePack']
    },
    {
      test: /\.js$/,
      loaders: ['happypack/loader?id=babelPack']
      include: [resolve('src')]
    }
  },
  plugins: [
    new HappyPack({
      id: 'babelPack',
      threadPool: threadPool,
      verbose: true,
      loaders: [
        {
          loader: 'babel-loader',
        }
      ]
    }),
    new HappyPack({
      id: 'vuePack',
      threadPool: threadPool,
      loaders: [
        {
          loader: 'vue-loader',
          options: vueLoaderConfig
        }
      ]
    })
  ]
]
```
* 在 Loader 配置中，所有文件的处理都交给了 happypack/loader 去处理，使用紧跟其后的 querystring ?id=babel 去告诉 happypack/loader 去选择哪个 HappyPack 实例去处理文件。
* 在 Plugin 配置中，新增了两个 HappyPack 实例分别用于告诉 happypack/loader 去如何处理 .js 和 .vue 文件。选项中的 id 属性的值和上面 querystring 中的 ?id=babel 相对应，选项中的 loaders 属性和 Loader 配置中一样。

***

####参数
* id: String 用唯一的标识符 id 来代表当前的 HappyPack 是用来处理一类特定的文件.

* loaders: Array 用法和 webpack Loader 配置中一样.

* threads: Number 代表开启几个子进程去处理这一类型的文件，默认是3个，类型必须是整数。

* verbose: Boolean 是否允许 HappyPack 输出日志，默认是 true。

* threadPool: HappyThreadPool 代表共享进程池，即多个 HappyPack 实例都使用同一个共享进程池中的子进程去处理任务，以防止资源占用过多。

* verboseWhenProfiling: Boolean 开启webpack --profile ,仍然希望HappyPack产生输出。

* debug: Boolean  启用debug 用于故障排查。默认 false。