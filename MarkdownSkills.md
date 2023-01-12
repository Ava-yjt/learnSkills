#一级标题
##二级标题

*斜体文本*
_斜体文本_
**粗体文本**
__粗体文本__
***粗斜体文本***
___粗斜体文本___

~~删除线~~
<u>下划线</u>

> 区块第一层
> > 区块第二层
> > 1. 有序列表


脚注[^无序列表]
[^无序列表]: */+/-
  
- 无序列表
  - > 区块

行内代码`python`

段内代码

    $(document).ready(function () {
    alert('RUNOOB');
    });

```javascript
$(document).ready(function () {
    alert('RUNOOB');
});
```
###链接
[教程链接](https://www.runoob.com/markdown/md-link.html)
直接使用链接地址：<https://www.runoob.com/markdown/md-link.html>

高级链接：这个链接设置1为网址变量 [教程链接][1]
*在文档结尾为变量赋值并空一行*

[1]: https://www.runoob.com/markdown/md-link.html
###图片
<u>格式</u>
![图片文本](网址或绝对/相对路径"可选标题")
![示例图](http://static.runoob.com/images/runoob-logo.png "RUNOOB")
指定图片高度宽度
<img decoding="async" src="http://static.runoob.com/images/runoob-logo.png" width="10%">
###表格
| 左对齐 | 右对齐 | 居中对齐 |
| :----- | -----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

***

* * *

*****

- - -

----------
导出PDF：ctrl + shift + p输入 >markdown pdf


###高级技巧
使用反斜杠转义特殊字符：

**文本加粗** 
\*\* 正常显示星号 \*\*

数学表达式：

行内：$...$ 或者 \(...\) 
块内：$$...$$ 或者 \[...\] 或者 ```math

$$
\begin{Bmatrix}
   a & b \\
   c & d
\end{Bmatrix}
$$

参考[菜鸟教程](https://www.runoob.com/markdown/md-tutorial.html)