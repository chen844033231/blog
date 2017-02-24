title: Markdown 基础语法 - Github
date: 2017-2-24 16:34:05
categories:
- WEB
- 技巧
tags:
- markdown
---
![](/blog/css/images/markdown.jpg)

# Markdown 基础语法 - Github

## Headings

用1到6个`#`来表达标题的重要性

# heading
## heading

```
# heading
## heading
```
---
## style text

样式字体有3种,粗体(Bold),斜体(Italic),删除线(Strikethrough)

### 粗体

**bold** __bold__

写法:
```
**bold text** 
或者
__bold text__
```

### 斜体

*italic* _italic_

写法:
```
*italic text* 
或者
_italic text_
```

### 删除线

~~strikethrough~~

写法:
```
~~strikethrough~~
```

---
## 引用文字

> 这里是一段引用的文字

写法:
```
> 这里是一段引用的文字
```

## 引用代码

```
$ node index.js
```

---
## 链接

[text](http://link)

写法:
```
[text](http://link)
```

---
## 图片

![text](http://link)

写法:
```
![text](http://link)
```

---
## 列表

列表分为有序,无序和任务列表

### 有序列表

1. 条目
2. 条目

写法:
```
1. 条目
2. 条目
```

### 无序列表

* 条目
* 条目

写法:
```
* 条目
* 条目
或者
_ 条目
_ 条目
```

### 任务列表

- [x] Finish my changes
- [ ] my changes

写法:
```
- [x] Finish my changes
- [ ] my changes
```

### 嵌套列表

* 条目
  * 条目二
* 条目

写法:
```
* 条目
  * 条目二
* 条目
```

---
## @某人

@marchen

写法:
```
@marchen
```

---
## 表情

:+1:

写法:
```
:emojicode:
```
---
## 忽略格式化

\*bold*

写法:
```
\*bold*
```

---
## 表格

| name    | age |
| :-----  | --: |
| marchen | 24  |

写法:
```
| name    | age |
| :-----  | --: |
| marchen | 24  |
```