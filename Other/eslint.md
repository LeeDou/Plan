# eslint

## eslint 忽略文件或代码块
### 整个问件忽略
- 项目根目录下创建.eslintignore 文件
- 在问件头部增加忽略注释
`/* eslint-disable */`

### 关闭代码块的检验
```js
/* eslint-disable */
alert('foo')
/* eslint-enable */
```

### 关闭某些规则的检验
```js
/* eslint-disable no-alert, no-console */
alert('foo')
console.log('foo')
/* eslint-enable no-alert, no-console */
```

### 关闭对某行代码的检验
```js
alert('foo');  // eslint-disable-line

// 或者
// eslint-disable-next-line
alert('foo');
```

## eslint 使用规则请移步：
[eslints基本用法](https://github.com/wy-ei/notebook/issues/36)