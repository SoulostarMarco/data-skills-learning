\# Week 1 学习笔记



\## Day 1: Python list \& tuple



\### 今日完成

\- 复习 list 基础操作、切片、排序、引用机制

\- 完成 10 道练习题(见 day01\_exercises.ipynb)



\### 学到的关键点

\- list 是引用类型,赋值会共享内存,需要 `.copy()` 才是真复制

\- `sort()` 原地修改并返回 None,`sorted()` 返回新 list

\- tuple 不可变,可以作为 dict 的 key

\- Python 切片越界不会报错,会自动截断(如 `lst\[i:i+n]`)



\### 我的弱点清单(持续更新)

1\. 习惯用 `for i in range(len(lst))`,应改为直接 `for x in lst`

2\. 简单过滤循环应优先用列表推导式

3\. 查重该用 set(O(1) 查找),计数该用 dict

4\. 经典面试题应先想 O(n) 解法,而不是上来就双重循环



\### 卡住/印象深的题

\- 第 10 题(股票最大利润):写出了 O(n²) 暴力解,后来学到 O(n) 一次遍历的「维护历史最低价」思路

\- 第 8 题(chunks):学到 `range(0, len(lst), n)` + 切片自动截断的 Python 经典 idiom

