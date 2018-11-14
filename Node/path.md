# 路径处理
在开发过程中我们经常会处理路径的拼接、转换、相对路径和绝对路径的互转，拆分路径等功能，使用nodeJs内置模块path就能轻松搞定这些问题。

## 引用path
`var path = require('path')`

## path 常用方法
path.basename
- path.basename(path[, ext])
  - path <String>
  - ext <String> 可选的文件扩展名
  - 返回： <String>
  path.basename() 方法返回一个path的最后一部分，类似于Unix的basename 命令。
  ```js
  path.basename('/foo/bar/index.html')  // 返回： 'index.html'
  path.basename('/foo/bar/index.html', '.html') // 返回：'index'
  ```
  如果path不是一个字符串或提供了ext不是一个字符串，则抛出 Typeerror。  

path.extname
  - path.extname(path)
    - path <String>
    - 返回： <String>
  path.extname() 方法返回path的扩展名，即从path的最后一部分中的最后一个. 字符到字符串结束。如果path的最后一部分没有. 或path的文件名的第一个字符是. ,则返回一个空字符串。
  ```js
    path.extname('index.html') // 返回： '.html'
    path.extname('index.coffee.md')  // 返回： '.md'
    path.extname('index.')  // 返回： '.'
    path.extname('.index')  // 返回： ''
  ``` 
  如果path不是一个字串，则抛出typeerror。   

path.join
  - path.join([...paths])
    - ...path <String> 一个路径片段的序列
    - 返回： <String>
  path.join() 方法使用平台特定的分隔符吧全部给定的path片段链接到一起，并规范化生成路径。
  长度为零的path片段也会被忽略，如果连接后的路径字符串是一个长度为零的字符串，则返回'.'，表示当前工作目录。
  ```js
  path.join('/foo', 'bar', 'baz/asdf', 'quux', '..')  // 返回: '/foo/bar/baz/asdf/quux'
  path.join('foo', {}, 'bar')  // 抛出typeerror错误 path.join参数必须为字符串
  ```  

path.resolve
  - path.resolve([...paths])
    - ...paths <String> 一个路径或片段的序列
    - 返回： <String>
    path.resolve() 方法会把一个路径或路径片段解析为一个绝对路径。
    给定的路径的序列是右往左被处理的的，后面的每个path被依次解析，直到构造完成一个绝对路径。   
    如果处理完全不给定的path片段后，还未生成一个绝对路径，则当前工作目录会被用上。   
    生成的路径是经规范化后的，切末尾的斜杠会被删除，除非路径被解析为根目录。   
    长度为零的path片段会被忽略。   
    如果没有传入path片段，则path.resolve() 会返回单签工作目录的绝对路径。  
    ```js
      path.resolve('baz', 'js', 'test')  // 返回： 如果当前工作目录为/foo/bar 则返回： '/foo/bar/baz/js/test'
      path.resolve('/foo/bar', './baz')  // 返回： '/foo/bar/baz'
      path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')  // 如果当前工作目录为/home/myself/node 则返回： '/home/myself/node/wwwroot/static_files/gif/image.gif'
    ```