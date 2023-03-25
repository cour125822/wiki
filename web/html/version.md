---
title: HTML版本
description: 
published: true
date: 2023-03-06T02:55:53.525Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:21:09.557Z
---

# HTML版本 ((20230203102850-9gtvt6n "*"))

从 Web 诞生早期至今，已经发展出多个 HTML 版本：

| 版本      | 年份 |
| ----------- | ------ |
| HTML      | 1991 |
| HTML+     | 1993 |
| HTML 2.0  | 1995 |
| HTML 3.2  | 1997 |
| HTML 4.01 | 1999 |
| XHTML 1.0 | 2000 |
| HTML5     | 2012 |
| XHTML5    | 2013 |

‍

## 常用的 DOCTYPE 声明

### HTML 5

```
<!DOCTYPE html>
```

### HTML 4.01 Strict

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "<http://www.w3.org/TR/html4/strict.dtd>">
```

### HTML 4.01 Transitional

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"<http://www.w3.org/TR/html4/loose.dtd>">
```

### HTML 4.01 Frameset

该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"
"<http://www.w3.org/TR/html4/frameset.dtd>">
```

### XHTML 1.0 Strict

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"<http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd>">
```

### XHTML 1.0 Transitional

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "
<http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd>">
```

### XHTML 1.0 Frameset

该 DTD 等同于 XHTML 1.0 Transitional，但允许框架集内容。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN"
"<http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd>">
```

### XHTML 1.1

该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型（例如提供对东亚语系的 ruby 支持）。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "<http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd>">
```