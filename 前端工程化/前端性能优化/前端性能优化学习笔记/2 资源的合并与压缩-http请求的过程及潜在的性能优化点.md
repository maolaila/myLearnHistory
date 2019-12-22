# 2 资源的合并与压缩

## 1.http请求过程及潜在优化点

## 核心

1. 减少http请求数量（资源合并）
2. 减少请求资源大小（资源压缩）

### dns优化

可以在浏览器中缓存对应的dns服务器信息，可减少请求次数和时间

### 使用CDN(获取静态资源)

CDN的域名不要和主站域名一样，这样可以防止在访问CDN时携带主站cookie的问题

### 请求过程中一些潜在的性能优化点

- dns请求是否可以通过缓存减少dns查询时间？

- 网络请求的过程走最近的网络环境？（使用CDN）

- 相同的静态资源是否可以缓存？(利用304)

- 能否减少请求http的大小？（资源压缩）

- 减少http请求次数（资源合并）

- 服务端渲染

- 开启gzip（在webpack中配置，同时需要服务器开启gzip）

  - ```
    webpackConfig.plugins.push(
        new CompressionWebpackPlugin({
          asset: '[path].gz[query]',
          algorithm: 'gzip',
          test: new RegExp(
            '\\.(' +
            config.build.productionGzipExtensions.join('|') +
            ')$'
          ),
          threshold: 10240,
          // deleteOriginalAssets:true, //删除源文件，不建议
          minRatio: 0.8
        })
      )
    ```

### 资源合并存在的问题

- 首屏渲染问题：由于首屏渲染所需要的资源都在一个js文件内，加载会非常慢
- 缓存失效问题：在和浏览器协议读取缓存资源时，在标记资源是否被更改时，会标记一个MD5戳，若a,b,c三个JS文件只有b更改了，合并后会造成全部js都有变动，会造成缓存大面积失效；

### 资源合并问题的解决方案

- 公共库合并，将业务代码和共同库代码分开打包，这样业务代码变动，就不会影响到共同库代码的缓存；
- 不同页面的合并（单页面应用）：单独打包不同页面

### fis3打包

首先通过正则及编码规则进行匹配和分析依赖，得到依赖数getSource，然后进行单文件的编译fis.compile(file)，在编译完成后，会按照打包规则进行打包