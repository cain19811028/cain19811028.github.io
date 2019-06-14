---
layout: post
title: "Python 資料類型比較 - dict, list, tuple, set, frozenset"
subtitle: "Python Data Types - dict, list, tuple, set, frozenset"
date: 2019-06-14 22:00:00 +0800
author: "cain19811028"
tags:
  - Python
---

### 差異：

先看 W3Schools 對於 Python Collections (Arrays) 的說明：

There are four collection data types in the Python programming language:
 - List is a collection which is `ordered` and `changeable`. Allows `duplicate` members.
 - Tuple is a collection which is `ordered` and `unchangeable`. Allows `duplicate` members.
 - Set is a collection which is `unordered` and `unindexed`. `No duplicate` members.
 - Dictionary is a collection which is `unordered`, changeable and indexed. `No duplicate` members.

搞懂上面四個的差別，再用下面這句，大概就能理解這幾個 Data Type 之間的關係：
 - `tuples are immutable lists, frozensets are immutable sets.`

### 相關連結：

 - Python3 Data Types：[https://docs.python.org/3/library/datatypes.html](https://docs.python.org/3/library/datatypes.html)
 - W3Schools Python Lists：[https://www.w3schools.com/python/python_lists.asp](https://www.w3schools.com/python/python_lists.asp)