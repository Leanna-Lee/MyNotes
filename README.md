# Markdown语法
- 支持“标准”Markdown / CommonMark和Github风格的语法，也可变身为代码编辑器；
- 支持实时预览、图片（跨域）上传、预格式文本/代码/表格插入、代码折叠、搜索替换、只读模式、自定义样式主题和多语言语法高亮等功能；
- 支持ToC（Table of Contents）、Emoji表情、Task lists、@链接等Markdown扩展语法；
- 支持TeX科学公式（基于KaTeX）、流程图 Flowchart 和 时序图 Sequence Diagram;
- 支持识别和解析HTML标签，并且支持自定义过滤标签解析，具有可靠的安全性和几乎无限的扩展性；
- 支持 AMD / CMD 模块化加载（支持 Require.js & Sea.js），并且支持自定义扩展插件；
- 兼容主流的浏览器（IE8+）和Zepto.js，且支持iPad等平板设备；
- 支持自定义主题样式。
# 目录
# 标题
## 标题
用# 表示标题
# Heading 1
`# 这是一级标题`
## Heading 2
`## 这是二级标题`
### Heading 3
`### 这是三级标题`
#### Heading 4
`#### 这是四级标题`
##### Heading 5
`##### 这是五级标题`
## 换行
空格+空格+回车，在网页上显示的效果为换行，遵循HTML规则。  
仅用回车，上下两行语句，在网页上显示的效果为同一行。
## 字体
用*表示斜体，**表示粗体，~~表示带删除线的文字。
- *这是斜体* _这是斜体_：`*这是斜体* _这是斜体_`  
- **这是粗体** __这是粗体__：`**这是粗体** __这是粗体__`  
- ***这是粗斜体*** ___这是粗斜体___：`***这是粗斜体*** ___这是粗斜体___`  
- 上标：X<sub>2</sub>，下标：O<sup>2</sup>：`X<sub>2</sub>`，`<sup>2</sup>`  
- ~~带删除线的文字~~：`~~带删除线的文字~~`
## 引用
用>表示引用
>这是引用的内容：`>这是引用的内容`  
It was the best of times, it was the worst of times.  
It was the age of wisdom, it was the age of foolishness.  
It was the epoch of belief, it was the epoch of incredulity.  
It was the season of light, it was the season of darkness.  
It was the spring of hope, it was the winter of despair.   
We had everything before us, we had nothing before us.  
We were all going direct to Heaven, we were all going direct the other way.  
>>这是引用的内容：`>>这是引用的内容`  
这是最好的时代，这是最坏的时代；  
这是智慧的时代，这是愚蠢的时代；  
这是信仰的时期，这是怀疑的时期；  
这是光明的季节，这是黑暗的季节；  
这是希望之春，这是失望之冬；  
人们面前有着各样事物，人们面前一无所有；  
人们正在直登天堂，人们正在直下地狱。
## 代码
用单引号表示单行代码；用三引号表示代码块。
### 单行代码
```
`print("Hello World!\n")`
```
`print("Hello World!\n")`
### 代码块   
JS代码
````
```javascript
function test(){
    console.log("Hello world!");
}
```
````
```javascript
function test(){
    console.log("Hello world!");
}
```
HTML代码
````
```html
<!DOCTYPE html>
<html>
    <head>
        <mate charest="utf-8" />
        <title>Hello world!</title>
    </head>
    <body>
        <h1>Hello world!</h1>
    </body>
</html
```
````
```html
<!DOCTYPE html>
<html>
    <head>
        <mate charest="utf-8" />
        <title>Hello world!</title>
    </head>
    <body>
        <h1>Hello world!</h1>
    </body>
</html
```
## 超链接

