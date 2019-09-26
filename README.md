# uni-app 富文本解析组件

## 代码从 https://github.com/icindy/wxParse fork。

于 wxParse 的差异：

1. 使用自定义组件重写逻辑
2. 解决 wxParse 中的 template 不能循环使用的问题

## 特性

| 支持特性        |
| ------------- |
| - [x] HTML的大部分标签解析 |
| - [x] 内联style          |
| - [x] 标签Class          |
| - [x] 图片自适应规则       |
| - [x] 图片多图片预览       |
| - [x] 模版层级可扩展性     |
| - [x] 多数据循环方式       |
| - [x] 内联style          |
| - [x] a标签跳转           |

## 基本使用方法

1. `yarn add https://github.com/jamieYou/u-rich-parse.git`

2. 引入必要文件

```
// main.js
import RichParse from 'u-rich-parse'

Vue.component('rich-parse', RichParse)
```

3. 使用
```
<rich-parse :content="content"/>
```

4. a 标签使用
```
<rich-parse :content="content" @linkTo="onLinkTo"/>
```

5. 图片预览
```
<rich-parse :preview="true 默认为 true" @imgTap="图片点击事件"/>
```

## 相关文章

* [wxDiscode－微信小程序特殊字符转义符转化工具类](http://weappdev.com/t/wxdiscode/203)
