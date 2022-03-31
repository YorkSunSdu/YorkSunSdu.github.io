---
layout: post
title: 如何用Typora编写Markdown文件
date: 2022-03-31
tags: Markdown
---

这篇原创blog将介绍如何用Typora编写Markdown文件。

## 标题

标题的语法是：几级标题，就是几个“#”，后面跟一个空格；快捷键是几级标题就是Ctrl+几。

例如：二级标题：“## 这是一个二级标题”。

## 字体

**倾斜**：用一对“\*”夹住所要倾斜的文字，快捷键Ctrl+I（l是incline的缩写）。

例如：“\*我想倾斜这段文字\*”，效果：*我想倾斜这段文字*。

**加粗**：用两对“\*”夹住所要倾斜的文字，快捷键Ctrl+B（B是bold的缩写）。

例如：“\*\*我想加粗这段文字\*\*”，效果：**我想加粗这段文字**。

**倾斜加粗**：用三对“\*”夹住所要倾斜加粗的文字，快捷键Ctrl+B+I。

例如：“\*\*\*我想加粗倾斜这段文字\*\*\*”，效果：***我想加粗倾斜这段文字***。

**下划线**：用一对“\<u>”和“\</u>”夹住所要下划线的文字，快捷键Ctrl+U（U是underline的缩写）。

例如：“\<u>我想在这段文字下面划一条下划线\</u>”，效果：<u>我想在这段文字下面划一条下划线</u>。

**删除线**：用两对“\~”夹住所要划删除线的文字，快捷键Alt+Shift+5（这个快捷键和上面几个画风不太一样啊T_T）。

例如：“\~\~我想在这段文字上划删除线\~\~”，效果：~~我想在这段文字上划删除线~~。

**高亮**：用两对“\==”夹住所要划删除线的文字，默认没有快捷键（没有快捷键的功能可以在“文件->偏好设置->打开高级设置->conf.user.json”自定义）。（如果输入了两对“\==”没有效果，请在“文件->偏好设置->MarkDown->MarkDown拓展语法”中，将对应的功能选中，下面的“上标/下标”功能同理）

例如：“\==我想将这段文字高亮显示\==”，效果：==我想将这段文字高亮显示==。

**上标**：用一对“\^””夹住所要变为上标的文字，默认没有快捷键。

例如：“\^我想将这段文字以上标形式显示\^”，效果：^我想将这段文字以上标形式显示^。

**下标**：用一对“\~”夹住所要变为下标的文字，默认没有快捷键。

例如：“\~我想将这段文字以下标形式显示\~”，效果：~我想将这段文字以下标形式显示~。

## 列表

**无序列表**：

效果：

+ 这是一级无序列表的第一个内容
  + 这是二级无序列表的第一个内容
+ 这是一级无序列表的第二个内容
  + 这是二级无序列表的第二个内容
    + 这是三级无序列表的第一个内容

编写方法：输入“\+”或者“\-”或者“\*”符号，再输入一个空格，即可生成一级无序列表。输入该级完成后，按一次回车键可再生成一个同级列表。此时：按一次回车键可保留当前缩进，去掉该级列表前面的小符号；按两次回车键可生成高一级列表；按一次Tab键可生成低一级列表。

或者使用快捷键Ctrl+Shift+]（这个快捷键好像并不是很快捷）

**有序列表**：

效果：

1. 这是一级有序列表的第一项内容
   1. 这是二级有序列表的第一项内容
   2. 这是二级有序列表的第二项内容
2. 这是一级有序列表的第二项内容
   1. 这是二级有序列表的第二项内容
      1. 这是三级有序列表的第一项内容

编写方法：输入“1.”，再输入一个空格，即可生成一级有序列表。相关操作与上面的无序列表操作完全相同，不再赘述。

或者使用快捷键Ctrl+Shift+[（这个快捷键好像也并不是很快捷）

## 表格

使用快捷键Ctrl+T（T是table的缩写），输入行数和列数，直接生成表格。

效果：

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

## 引用

输入几个“\>”符号就是几级引用。

例如：

\>这是第一级引用

\>按一下回车，这也是第一级引用

\>\>回车之后再输入一个“\>”，这就变成了下一级，也就是第二级引用

\>回车之后再按一下回车，这就回到了第一级引用

效果：

> 这是第一级引用
>
> 按一下回车，这也是第一级引用
>
> > 回车之后再输入一个“\>”，这就变成了下一级，也就是第二级引用
>
> 回车之后再按一下回车，这就回到了第一级引用

## 分割线

连续输入三个及以上“-”符号，回车后生成分割线。

例如：

这是一段文字，想生成一条分割线

\-\-\-\-\-

这段文字就与刚才那段分隔开啦

效果：

这是一段文字，想生成一条分割线

-----

这段文字就与刚才那段分隔开啦

## 代码

说明：“`”这个符号为反引号，在键盘上和“~”是一个键，通常在键盘左上角数字“1”键的左边，直接按一下那个键就好了。

注意：输入法要切成英文，不然会输入成“·”符号。这个符号是把歪果仁的外文名翻译成中文之后，连接姓和名的。博主还真不知道这个符号叫啥，如果你知道你可以来找博主比划比划。

**单行代码/多行代码**：

单行代码：用一对“\`”夹住想要输入的代码。快捷键Ctrl+Shift+\`

例如：

\`cout << ("hello world");\`

效果：

`cout << ("hello world");`

多行代码：用三对“`”夹住想要输入的代码。快捷键Ctrl+Shift+K

例如：

\`\`\`
#include\<iostream\>
using namespace std;
int main()
{
	cout << "hello world";
	return 0;
}
\`\`\`

效果：

```
#include<iostream>
using namespace std;
int main()
{
	cout << "hello world";
	return 0;
}
```

如果点击代码块，在右下角选择语言，就会变成这样：

```c++
#include<iostream>
using namespace std;
int main()
{
	cout << "hello world";
	return 0;
}
```

## 注释

与HTML的注释格式一样。

例如：

\<\!\-\-这是注释\-\-\>

效果：

<!--这是注释-->

哈哈，注释怎么显示出来呢，肯定什么效果都没有啊

在Typora内效果如图：

![注释效果图](D:\Blog\images\posts\howToUseTypora\注释效果.png)

## 任务列表

依次输入：\-空格[空格]空格

例如：

\- \[ \] 背一百个单词

效果：

- [ ] 背一百个单词

## 插入超链接

依次输入中括号和小括号，或者使用快捷键Ctrl+K。

在中括号内输入文本，在小括号内输入超链接。

例如：

如果你想了解更多关于山东大学，请访问\[山东大学官网\]\(https://www.sdu.edu.cn\)。

效果：

如果你想了解更多关于山东大学，请访问[山东大学官网](https://www.sdu.edu.cn)。

在Typora中，按住Ctrl的同时点击超链接可以直接在浏览器中访问。

## 插入图片

依次输入叹号、中括号和小括号，或者使用快捷键：Ctrl+Shift +I。

效果：![插入图片效果](D:\Blog\images\posts\howToUseTypora\插入图片效果.png)

在中括号内输入图片描述，在后面输入图片路径，也可以直接在磁盘中选择图片文件。



还有显示大纲，字数统计，修改样式如颜色没有写。
