# Week 1 学习笔记

## Day 1: Python list & tuple

### 今日完成
- 复习 list 基础操作、切片、排序、引用机制
- 完成 10 道练习题(见 day01_exercises.ipynb)

### 学到的关键点
- list 是引用类型,赋值会共享内存,需要 `.copy()` 才是真复制
- `sort()` 原地修改并返回 None,`sorted()` 返回新 list
- tuple 不可变,可以作为 dict 的 key
- Python 切片越界不会报错,会自动截断(如 `lst[i:i+n]`)

### 我的弱点清单(持续更新)
1. 习惯用 `for i in range(len(lst))`,应改为直接 `for x in lst`
2. 简单过滤循环应优先用列表推导式
3. 查重该用 set(O(1) 查找),计数该用 dict
4. 经典面试题应先想 O(n) 解法,而不是上来就双重循环

### 卡住/印象深的题
- 第 10 题(股票最大利润):写出了 O(n²) 暴力解,后来学到 O(n) 一次遍历的「维护历史最低价」思路
- 第 8 题(chunks):学到 `range(0, len(lst), n)` + 切片自动截断的 Python 经典 idiom

---

## Day 2: Python dict & set

### 今日完成
- 掌握 dict 的创建、访问、遍历三种方式(key / value / items)
- 学会 dict 两大核心模式:**计数** 和 **分组**
- 字典推导式入门
- 掌握 set 的核心优势(O(1) 查找)和四种集合运算(交/并/差/对称差)
- 完成 10 道练习题(见 day02_exercises.ipynb)

### 学到的关键点
- `d.get(key, default)` 安全访问,避免 KeyError —— 数据岗最常用技巧
- `for k, v in d.items()` 是遍历 dict 的标准写法
- **计数模式**:`counts[x] = counts.get(x, 0) + 1`(一行替代昨天的 3 个循环)
- **分组模式**:`if k not in groups: groups[k] = []; groups[k].append(v)`
  - 这个模式就是 Pandas `groupby` 的思想雏形
  - 进阶可用 `from collections import defaultdict; defaultdict(list)` 省掉 if 判断
- 字典推导式语法:`{key表达式: value表达式 for 变量 in 序列}`
- set 用 `set()` 建空集合,`{}` 是空 dict(易错点)
- set 运算的实际应用:用户留存/流失/新增分析(`今天 & 昨天`、`昨天 - 今天`、`今天 - 昨天`)

### 我的弱点清单(持续更新)
1. ~~习惯用 `for i in range(len(lst))`,应改为直接 `for x in lst`~~ ✅ 已改进
2. ~~简单过滤循环应优先用列表推导式~~ ✅ 已改进
3. 查重该用 set(O(1) 查找),计数该用 dict
4. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
5. **永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`**(Day 2 题 7 教训)
6. **字典推导式语法要练熟**:`{key: value for x in seq}`,Pandas 里会大量用类似结构
7. 用 `min()` / `max()` 替代 if/else 取小取大,代码更简洁
8. 变量名不要和外层变量重名(如 `for text in text_lst` 会覆盖原 text)

### 卡住/印象深的题
- 第 7 题(用户消费排序):碰巧输出对了,但实际上忘了 `sorted()` 排序步骤 —— **这是个隐藏 bug**,以后做"按 X 排序"的题必须显式调 sorted
- 第 8 题(字典推导):用了普通 for 循环,没真正用字典推导式,需要补练
- 第 10 题(带重复次数的交集):写出了正确答案但代码冗余,学到 `min()` + `extend([k] * n)` 的简化思路;这道题其实是 `pd.merge` 的手写原理

### 概念顿悟
- 第 6 题的"按用户分组求金额 list"和 SQL 的 `GROUP BY user` 是同一件事
- 第 10 题的"两个 list 求交集"本质上就是 `pd.merge` 的雏形
- 越来越体会到:**dict 是 Pandas 的基础,理解 dict 操作就理解了 80% 的 DataFrame 操作**

---

## Day 3: 列表推导式深入 + lambda + 高阶函数

### 今日完成
- 列表推导式的进阶用法:过滤(if 在后)、三元表达式(if 在前)、双重嵌套
- 字典推导式 + 集合推导式
- lambda 匿名函数的本质和典型用途
- `sorted` 的 key 参数 + 多关键字排序
- `map` 和 `filter` 的概念(知道存在,但实际用列表推导式更 Pythonic)
- 完成 10 道练习题(包含 5c 矩阵转置、10 二维数据透视等挑战题)

### 学到的关键点
- **三元表达式 vs 过滤的位置区别**:
  - `[x if cond else y for x in seq]` → if 在前,**三元表达式**,每个元素都保留
  - `[x for x in seq if cond]` → if 在后,**过滤**,只保留符合条件的
- **多关键字排序**:`key=lambda x: (主关键字, -次关键字)`,负号实现降序
  - 字符串次关键字不能加负号,解决方法:**Python 排序是稳定的,可以分两次 sorted**(从次到主)
- **嵌套列表推导**:外层循环写在前,内层循环写在后,顺序和普通双重 for 一致
  - 矩阵转置经典模板:`[[row[i] for row in matrix] for i in range(len(matrix[0]))]`
- **lambda 主要用作"参数"传给其他函数**,如 sorted/map/filter;独立赋值用 def 更清晰
- **Python 内置工具要熟悉**:`abs()`、`max()`、`min()`、`sum()`、`zip()` —— 不要重新发明轮子
- **defaultdict 简化嵌套 dict**:`defaultdict(lambda: defaultdict(int))` —— 数据岗高频写法

### 我的弱点清单(持续更新)
1. ~~习惯用 `for i in range(len(lst))`~~ ✅
2. ~~简单过滤循环应优先用列表推导式~~ ✅
3. ~~字典推导式语法~~ ✅
4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. **永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`**
7. 用 `min()` / `max()` 替代 if/else 取小取大,代码更简洁
8. 变量名不要和外层变量重名
9. **优先用 Python 内置函数**:`abs()`、`max()`、`zip()` 等,不要重新发明轮子(Day 3 题 3)
10. **Python 排序是稳定的** —— 多次 sorted 可以实现复杂多关键字排序(字符串次关键字情形)
11. **可读性 > 优雅** —— 不要为了一行而损害代码可读性,工程上分两行/多行更好(Day 3 题 7、9)

### 卡住/印象深的题
- 第 5c(矩阵转置):一开始没想到,但学到经典模板「外层 i 控制列、内层取每行第 i 个」—— Python 嵌套推导式的标志题
- 第 9(订单过滤排序提取):写得很顺,而且发现这就是 SQL 的 `WHERE + ORDER BY + SELECT`,Python 列表推导和 SQL 在逻辑上完全对应
- 第 10(二维数据透视):手写实现了 Pandas `pivot_table` 的核心逻辑,理解了"透视"在底层就是嵌套 dict 累加

### 概念顿悟
- **列表推导式 ≈ SQL 的 WHERE + SELECT**:`[col for row in table if 条件]` 完全对应 SQL 思想
- **sorted + lambda ≈ SQL 的 ORDER BY**:多关键字写法一脉相承
- **嵌套 dict 累加 ≈ Pandas pivot_table**:groupby 多个维度求和,本质就是手写嵌套 dict
- 越来越清晰地看到:**Python 高阶函数和列表推导式 = SQL 思想在 Python 里的实现**,反之亦然。这两套语言在数据处理上的"翻译关系"基本建立

### Day 1-3 阶段性回顾
经过三天训练,代码风格已经从「C/Java 式硬循环」过渡到「Pythonic 推导式 + 高阶函数」。
基础 Python 在数据处理场景下的常用工具基本到位:
- list / tuple / dict / set 各自的使用场景清晰
- 列表推导/字典推导/集合推导都能熟练写
- sorted + lambda 多关键字排序已成肌肉记忆
- 「计数 / 分组 / 透视」三大数据处理模式都手写过一遍

下一步进入 SQL 入门,有 Python 基础打底,SQL 学起来应该会很自然。

---

## Day 4: SQL 入门 —— SELECT / WHERE / ORDER BY / LIMIT

### 今日完成
- 环境搭建:DuckDB + jupysql,直接查询 csv,无需建表
- 用 Pandas 生成 200 行(后扩展到 500 行覆盖全年)的练习数据 `data/sales.csv`
- SQL 基础四件套:SELECT(选列)、WHERE(过滤)、ORDER BY(排序)、LIMIT(限量)
- 列别名 AS、列表达式计算、DISTINCT 去重
- 字符串模糊匹配:LIKE / ILIKE
- 多条件组合:AND / OR / IN / BETWEEN / NOT IN
- 完成 10 道练习题(基于 sales.csv)

### 学到的关键点
- **SQL 字符串用单引号 `'...'`,双引号 `"..."` 是给列名/表名用的** —— 别弄混
- **NULL 比较必须用 `IS NULL` / `IS NOT NULL`**,`= NULL` 永远返回 0 行(SQL 最大的坑)
- **DuckDB 可以直接 `SELECT * FROM 'xxx.csv'`** —— 不需要建表导入,极适合学习
- **SQL 的执行顺序**(必背):
  - 书写顺序:`SELECT → FROM → WHERE → ORDER BY → LIMIT`
  - 执行顺序:`FROM → WHERE → SELECT → ORDER BY → LIMIT`
  - 推论:WHERE 里不能用 SELECT 起的别名;ORDER BY 里可以用别名
- **多关键字排序**:`ORDER BY col1 ASC, col2 DESC` —— 和 Day 3 Python 的 `sorted(key=lambda x: (a, -b))` 完全对应
- **LIKE 大小写敏感**:跨大小写匹配用 `ILIKE 'm%'` 或 `LOWER(col) LIKE 'm%'`
- **Top N 模式**:`ORDER BY xxx DESC LIMIT N` —— SQL 面试最高频的固定写法
- **返回 0 行的排查思路**:先验证数据范围(MIN/MAX/COUNT 看一眼),再怀疑 SQL 写错

### 我的弱点清单(持续更新)
1. ~~习惯用 `for i in range(len(lst))`~~ ✅
2. ~~简单过滤循环应优先用列表推导式~~ ✅
3. ~~字典推导式语法~~ ✅
4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. 永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`
7. 用 `min()` / `max()` 替代 if/else 取小取大
8. 变量名不要和外层变量重名
9. 优先用 Python 内置函数:`abs()`、`max()`、`zip()` 等
10. Python 排序是稳定的 —— 多次 sorted 可以实现复杂多关键字排序
11. 可读性 > 优雅 —— 不要为了一行而损害代码可读性
12. **写 SQL 时再看一眼题目要求的列是哪几个**(Day 4 题 2、8 教训:两道题都因为漏看 SELECT 列错了)
13. **SQL 返回 0 行的排查思路**:先看数据范围,再怀疑查询(Day 4 题 6)
14. **`LIKE` 大小写敏感**,不确定大小写用 `ILIKE` 或 `LOWER()`

### 卡住/印象深的题
- 题 2、题 8:**都是 SELECT 列没看仔细**,WHERE 写对了但选错列 —— 这是一个习惯性问题,以后写 SQL 必须养成"先确认输出列"的检查习惯
- 题 6:SQL 完全正确但返回 0 行,**学到一个重要教训:0 行不一定是 SQL 错,可能数据本来就没有**。养成"先用 MIN/MAX/COUNT 摸清数据范围"的习惯
- 题 10:三个 WHERE 条件 + 排序 + 限量串得很顺,实战感受到 SQL 的简洁性

### 概念顿悟
- **SQL ↔ Python 翻译表**(继续扩充):

  | 操作 | Python 列表推导 | SQL |
  |------|----------------|-----|
  | 取所有 | `[row for row in data]` | `SELECT * FROM data` |
  | 过滤 | `[row for row in data if row['x'] > 100]` | `WHERE x > 100` |
  | 选列 | `[row['name'] for row in data]` | `SELECT name FROM data` |
  | 计算列 | `[row['x'] * 1.13 for row in data]` | `SELECT x * 1.13 FROM data` |
  | 多条件 | `if a > 0 and b in [...]` | `WHERE a > 0 AND b IN (...)` |
  | 排序 | `sorted(data, key=lambda r: r['x'])` | `ORDER BY x` |
  | 排序+降序 | `sorted(data, key=lambda r: -r['x'])` | `ORDER BY x DESC` |
  | 多关键字排序 | `sorted(..., key=lambda r: (r['a'], -r['b']))` | `ORDER BY a ASC, b DESC` |
  | 取前 N | `sorted(...)[:5]` | `LIMIT 5` |
  | 去重 | `set(...)` | `SELECT DISTINCT` |
  | 模糊匹配 | `r['x'].startswith('M')` | `WHERE x LIKE 'M%'` |

- **SQL 比 Python 列表推导更简洁**:Day 3 的「过滤 + 排序 + 提取」三步在 SQL 里是 SELECT + WHERE + ORDER BY 三个子句,可读性极高
- **核心顿悟**:SQL 不是"另一门新语言",而是 **「在 Python 里用列表推导处理数据的思路 + 声明式语法的包装」**。我前 3 天写的每一行列表推导,SQL 里都有对应的写法
