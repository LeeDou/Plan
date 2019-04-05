# Vue 中icon使用的两种处理办法

### 组件式
通过新建一个icon组件，将svg图片进行封装，使用时可以传strokeColor来控制图标颜色。
贴代码：
```js
<template>
    <svg width="14px" height="14px" viewBox="0 0 14 14" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
        <g id="icon-5" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
            <g id="copy" transform="translate(-12.000000, -74.000000)" :stroke="strokeColor">
                <g id="2">
                    <g id="21" transform="translate(12.000000, 74.000000)">
                        <g>
                            <path d="M6.65,8.35 C5.24167389,8.35 4.1,7.01804713 4.1,5.375 C4.1,3.73195287 5.24167389,2.4 6.65,2.4 C8.05832611,2.4 9.2,3.73195287 9.2,5.375 C9.2,7.01804713 8.05832611,8.35 6.65,8.35 Z M5.8,9.2445343 L5.8,8.35 L7.5,8.35 L7.5,9.22624716 C9.4289839,9.39883416 10.4772634,10.0202243 10.9840973,10.6966309 C11.202327,10.9878742 11.5160011,11.75 10.7863312,11.75 C9.41347147,11.75 8.55202291,11.75 7.07827667,11.75 C5.75681198,11.75 3.96297592,11.75 3.04584047,11.75 C2.12870503,11.75 2.40299693,11.0277245 2.62090454,10.6966309 C3.06451609,10.0225976 4.29906364,9.42491352 5.8,9.2445343 Z" id="合并形状"></path>
                            <rect id="jx" x="0.5" y="0.5" width="13" height="13" rx="3"></rect>
                        </g>
                    </g>
                </g>
            </g>
        </g>
    </svg>
</template>

<script>
export default {
    name: 'icon',
    props: {
        strokeColor: {
            type: String,
            default: '#48B4FF'
        }
    }
}
</script>
```
例如我新建了这样一个Icon组件，当我在使用是就可以像其他组件一样引入，并可以通过props传入icon色值来改变图标颜色，例如：
`<Icon :stroke-color="'red'"></Icon>`

此时图标颜色就是红色。
![cofn](/img/icon1.jpg)
![cofn](/img/icon2.jpg)

通过组件，我们同样可以可以改变svg的填充颜色，以及图标大小等属性，使用的灵活性可以提升。

### 通过使用阿里iconfont
通过使用iconfont我们可以选择不同的使用方式，在处理图标与文字颜色值相同的情况下，我们可以使用css类的这种实现方式来使用图标，下面就详细说明使用步骤：  
（根据设计给的svg图片，来转换成我们使用的css文件）
- 进入 https://www.iconfont.cn 注册账号登陆，新建自己项目（当直接使用他人已创建图标时可以忽略）
![cofn](/img/eIcon1.jpg)
![cofn](/img/eIcon2.jpg)

- 上传图标至已有项目中
![cofn](/img/eIcon3.jpg)

- 在项目中新建Icon文件夹，并将icon文件下载解压到此文件夹
![cofn](/img/eIcon4.jpg)
![cofn](/img/eIcon5.jpg)

- 修改iconfont.css文件
编辑，根据图标名称，新增类选择器css样式，这样我们就可以在vue中通过class直接使用图标
![icon6](/img/eIcon6.jpg)
蓝色部分时项目名加上图标名称，我们可修改项目前缀名称：
![icon6](/img/eIcon7.jpg)
![icon6](/img/eIcon8.jpg)

- 在main.js 中引入刚才编辑的iconinfont.css文件   
接下来就可以在使用icon了   
`<i class="el-icon-dashbluetoothon"></i>`
图标颜色会根据父级color颜色变化而变化。

### end
提供两种不同方按，在实际项目中我们可以根据自己的需求来选择。


