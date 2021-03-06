***[Markdown语法](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#markdown%E8%AF%AD%E6%B3%95)***  
- [标题](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E6%A0%87%E9%A2%98)  
- [换行](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E6%8D%A2%E8%A1%8C)  
- [字体](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E5%AD%97%E4%BD%93)  
- [引用](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E5%BC%95%E7%94%A8)  
- [代码](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E4%BB%A3%E7%A0%81)  
  - [单行代码](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E5%8D%95%E8%A1%8C%E4%BB%A3%E7%A0%81)
  - [代码块](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E4%BB%A3%E7%A0%81%E5%9D%97)
- [超链接](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E8%B6%85%E9%93%BE%E6%8E%A5)
- [列表](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E5%88%97%E8%A1%A8)  
  - [有序列表](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E6%9C%89%E5%BA%8F%E5%88%97%E8%A1%A8)  
  - [无序列表](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E6%97%A0%E5%BA%8F%E5%88%97%E8%A1%A8) 
- [表格](https://github.com/Leanna-Lee/MyNotes/blob/master/Markdown.md#%E8%A1%A8%E6%A0%BC)
# Markdown语法
## 标题
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```  
## 换行
空格+空格+回车，在网页上显示的效果为换行，遵循HTML规则。  
仅用回车，上下两行语句，在网页上显示的效果为同一行。
## 字体
用*或_表示斜体，**或__表示粗体，~~表示带删除线的文字。
- *这是斜体* _这是斜体_：`*这是斜体* _这是斜体_`  
- **这是粗体** __这是粗体__：`**这是粗体** __这是粗体__`  
- ***这是粗斜体*** ___这是粗斜体___：`***这是粗斜体*** ___这是粗斜体___`  
- 上标 X<sup>2</sup>，下标 H<sub>2</sub>O<sub>2</sub>：  
```
X<sup>2</sup>
O<sub>2</sub>
```
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
```
[Title](https://cn.bing.com/)
```
## 列表  
### 有序列表  
```
1. 列表内容  
  i. 嵌套列表(开头两个空格)  
  ii. 嵌套列表  
  iii. 嵌套列表
2. 列表内容
3. 列表内容
```
### 无序列表  
- 无序列表
- 无序列表
  - 嵌套列表
    - 嵌套列表
```
- 无序列表
- 无序列表
  - 嵌套列表
    - 嵌套列表
```
  
## 表格  
 
|column1|column2|column3|
|:--|:--:|--:|
|左对齐|居中|右对齐|  
```
（空格+空格+回车） 
|column1|column2|column3|
|:--|:--:|--:|
|左对齐|居中|右对齐|  
```


