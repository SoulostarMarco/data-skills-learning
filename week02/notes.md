# Week 1 学习笔记

> \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*主题\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*:Python 基础(list/tuple/dict/set/推导式/lambda) + SQL 入门(SELECT/WHERE/GROUP BY/HAVING)
> \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*完成情况\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*:6 天,62 道练习题(Python 40 + SQL 22),正确率约 90%

\---

## Day 1: Python list \& tuple

### 今日完成

* 复习 list 基础操作、切片、排序、引用机制
* 完成 10 道练习题(见 day01\_exercises.ipynb)

### 学到的关键点

* list 是引用类型,赋值会共享内存,需要 `.copy()` 才是真复制
* `sort()` 原地修改并返回 None,`sorted()` 返回新 list
* tuple 不可变,可以作为 dict 的 key
* Python 切片越界不会报错,会自动截断(如 `lst\\\\\\\\\\\\\\\[i:i+n]`)

### 卡住/印象深的题

* 第 10 题(股票最大利润):写出了 O(n²) 暴力解,后来学到 O(n) 一次遍历的「维护历史最低价」思路
* 第 8 题(chunks):学到 `range(0, len(lst), n)` + 切片自动截断的 Python 经典 idiom

\---

## Day 2: Python dict \& set

### 今日完成

* 掌握 dict 的创建、访问、遍历三种方式(key / value / items)
* 学会 dict 两大核心模式:**计数** 和 **分组**
* 字典推导式入门
* 掌握 set 的核心优势(O(1) 查找)和四种集合运算(交/并/差/对称差)
* 完成 10 道练习题(见 day02\_exercises.ipynb)

### 学到的关键点

* `d.get(key, default)` 安全访问,避免 KeyError —— 数据岗最常用技巧
* `for k, v in d.items()` 是遍历 dict 的标准写法
* **计数模式**:`counts\\\\\\\\\\\\\\\[x] = counts.get(x, 0) + 1`
* **分组模式**:`if k not in groups: groups\\\\\\\\\\\\\\\[k] = \\\\\\\\\\\\\\\[]; groups\\\\\\\\\\\\\\\[k].append(v)`

  * 这个模式就是 Pandas `groupby` 的思想雏形
  * 进阶可用 `from collections import defaultdict; defaultdict(list)` 省掉 if 判断
* 字典推导式语法:`{key表达式: value表达式 for 变量 in 序列}`
* set 用 `set()` 建空集合,`{}` 是空 dict(易错点)
* set 运算的实际应用:用户留存/流失/新增分析(`今天 \\\\\\\\\\\\\\\& 昨天`、`昨天 - 今天`、`今天 - 昨天`)
* `Counter` 是 dict 的计数特化版,支持直接相加(`Counter(a) + Counter(b)`)

### 卡住/印象深的题

* 第 7 题(用户消费排序):碰巧输出对了,但实际上忘了 `sorted()` 排序步骤 —— **隐藏 bug**
* 第 8 题(字典推导):用了普通 for 循环,没真正用字典推导式
* 第 10 题(带重复次数的交集):写出了正确答案但代码冗余,学到 `min()` + `extend(\\\\\\\\\\\\\\\[k] \\\\\\\\\\\\\\\* n)`;这道题其实是 `pd.merge` 的手写原理

### 概念顿悟

* 第 6 题"按用户分组求金额 list"和 SQL 的 `GROUP BY user` 是同一件事
* 第 10 题"两个 list 求交集"本质上就是 `pd.merge` 的雏形
* **dict 是 Pandas 的基础,理解 dict 操作就理解了 80% 的 DataFrame 操作**

\---

## Day 3: 列表推导式深入 + lambda + 高阶函数

### 今日完成

* 列表推导式进阶:过滤(if 在后)、三元表达式(if 在前)、双重嵌套
* 字典推导式 + 集合推导式
* lambda 匿名函数的本质和典型用途
* `sorted` 的 key 参数 + 多关键字排序
* `map` / `filter` 的概念(知道存在,实际用列表推导式更 Pythonic)
* 完成 10 道练习题(含 5c 矩阵转置、10 二维数据透视等挑战题)

### 学到的关键点

* **三元表达式 vs 过滤的位置区别**(重要,Day 6 自测答错过):

  * `\\\\\\\\\\\\\\\[x for x in seq if cond]` → if 在**后** → **过滤**,decide 元素**保不保留**
  * `\\\\\\\\\\\\\\\[x if cond else y for x in seq]` → if 在**前** → **三元表达式**,decide 每个元素**输出成什么**(全部保留)
  * 区别不是"筛选顺序",而是「过滤」vs「变换」
* **多关键字排序**:`key=lambda x: (主关键字, -次关键字)`,负号实现降序

  * 字符串次关键字不能加负号,解决方法:**Python 排序是稳定的,可以分两次 sorted**(从次到主)
* **嵌套列表推导**:外层循环写在前,内层循环写在后,顺序和普通双重 for 一致

  * 矩阵转置经典模板:`\\\\\\\\\\\\\\\[\\\\\\\\\\\\\\\[row\\\\\\\\\\\\\\\[i] for row in matrix] for i in range(len(matrix\\\\\\\\\\\\\\\[0]))]`
* **lambda 主要用作"参数"传给其他函数**;独立赋值用 def 更清晰
* **Python 内置工具要熟悉**:`abs()`、`max()`、`min()`、`sum()`、`zip()`
* **defaultdict 简化嵌套 dict**:`defaultdict(lambda: defaultdict(int))`

### 卡住/印象深的题

* 第 5c(矩阵转置):学到经典模板「外层 i 控制列、内层取每行第 i 个」
* 第 9(订单过滤排序提取):发现这就是 SQL 的 `WHERE + ORDER BY + SELECT`
* 第 10(二维数据透视):手写实现了 Pandas `pivot\\\\\\\\\\\\\\\_table` 的核心逻辑

### 概念顿悟

* **列表推导式 ≈ SQL 的 WHERE + SELECT**
* **sorted + lambda ≈ SQL 的 ORDER BY**
* **嵌套 dict 累加 ≈ Pandas pivot\_table**
* Python 高阶函数和列表推导式 = SQL 思想在 Python 里的实现,反之亦然

\---

## Day 4: SQL 入门 —— SELECT / WHERE / ORDER BY / LIMIT

### 今日完成

* 环境搭建:DuckDB + jupysql,直接查询 csv,无需建表
* 用 Pandas 生成 500 行覆盖全年的练习数据 `data/sales.csv`
* SQL 基础四件套:SELECT、WHERE、ORDER BY、LIMIT
* 列别名 AS、列表达式计算、DISTINCT 去重
* 字符串模糊匹配:LIKE / ILIKE
* 多条件组合:AND / OR / IN / BETWEEN / NOT IN
* 完成 10 道练习题(基于 sales.csv)

### 学到的关键点

* **SQL 字符串用单引号 `'...'`**,双引号是给列名/表名用的
* **NULL 比较必须用 `IS NULL` / `IS NOT NULL`**,`= NULL` 永远返回 0 行
* **DuckDB 可以直接 `SELECT \\\\\\\\\\\\\\\* FROM 'xxx.csv'`**
* **SQL 的执行顺序**(必背):

  * 书写顺序:`SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT`
  * 执行顺序:`FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`
* **多关键字排序**:`ORDER BY col1 ASC, col2 DESC`
* **LIKE 大小写敏感**,跨大小写用 `ILIKE` 或 `LOWER()`
* **Top N 模式**:`ORDER BY xxx DESC LIMIT N`
* **返回 0 行的排查思路**:先验证数据范围(MIN/MAX/COUNT),再怀疑 SQL

### 卡住/印象深的题

* 题 2、题 8:**都是 SELECT 列没看仔细**,WHERE 写对了但选错列
* 题 6:SQL 完全正确但返回 0 行 —— 0 行不一定是 SQL 错,可能数据本来就没有

\---

## Day 5: SQL 聚合 —— COUNT / SUM / AVG / GROUP BY / HAVING

### 今日完成

* 聚合函数:COUNT / SUM / AVG / MIN / MAX / ROUND
* COUNT 三兄弟:`COUNT(\\\\\\\\\\\\\\\*)` / `COUNT(col)` / `COUNT(DISTINCT col)`
* GROUP BY 单列分组、多列分组
* HAVING 对分组结果过滤
* WHERE + HAVING 组合使用
* 完成 10 道练习题 + 1 道附加思考题

### 学到的关键点

* **GROUP BY 黄金法则**:SELECT 里的非聚合列,必须出现在 GROUP BY 里
* **COUNT 三兄弟的区别**:

  * `COUNT(\\\\\\\\\\\\\\\*)` —— 所有行(含 NULL)
  * `COUNT(col)` —— col 非 NULL 的行数
  * `COUNT(DISTINCT col)` —— col 不重复值的个数
* **WHERE vs HAVING**:

  * WHERE = 分组**前**过滤**原始行**,不能用聚合函数
  * HAVING = 分组**后**过滤**分组结果**,可以用聚合函数
* **别名可用性规则**(重要面试陷阱):

|子句|能用 SELECT 别名?|原因|
|-|-|-|
|WHERE|❌|WHERE 在 SELECT 之前执行|
|GROUP BY|⚠️ 部分数据库可以|GROUP BY 在 SELECT 之前|
|HAVING|⚠️ DuckDB 容忍,标准上不行|HAVING 在 SELECT 之前|
|ORDER BY|✅|ORDER BY 在 SELECT 之后|

* 口诀:**ORDER BY 能用别名,它之前的子句都不能**
* **SELECT 最后一列后面不要加逗号** —— DuckDB 容忍,换数据库会报错

### 卡住/印象深的题

* 题 7、8、10:**HAVING 里用了别名** —— DuckDB 容忍了,但 PostgreSQL 会报错。
正确做法:HAVING 里写 `SUM(total)`、`COUNT(\\\\\\\\\\\\\\\*)` 本身,不写别名
* 附加思考题(GROUP BY country 却 SELECT product)答对了:黄金法则理解到位

\---

## Day 6: Week 1 综合复习 + 阶段自测

### 今日完成

* 12 道混合练习(Python / SQL 自己判断该用哪个工具)
* Week 1 自测清单(Python 5 项 + SQL 5 项)
* 完成 11/12 题(第 12 题子查询为 Day 7 预告,暂未做)

### 学到的关键点 / 重要教训

* **"维护最大 + 第二大"类问题,初始化要用 `float('-inf')`(负无穷)**,
不能用 `nums\\\\\\\\\\\\\\\[0]` —— 否则比初始值小的合理候选数会被永久挡住

  * bug 复现:`\\\\\\\\\\\\\\\[3, 1, 2]` 用 `nums\\\\\\\\\\\\\\\[0]` 初始化会错误输出 3(应为 2)
* **STRFTIME 比 SUBSTRING 更专业**:处理日期用 `STRFTIME(date, '%Y-%m')`,
字符串截取在格式变化时会出错,日期函数不会
* **文本解析的稳健思路**:先拆成逻辑单元(句子),再取需要的部分,
不要依赖"每句固定 N 个词"这种脆弱假设
* **进阶预告**:子查询(查询里套查询),Day 7 正式学

### Week 1 最重要的一条经验

本周至少 3 次"输出对了但代码有 bug"(Day 2 题7、Day 4 题6、Day 6 题1):

> \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*写完代码不要只用题目给的例子测试。要自己造"刁钻"的测试用例\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*
> —— 空输入、全负数、有重复值、极端值、第一个元素是最大/最小值等。
这是初级和中级工程师的分水岭。

\---

# Week 2 学习笔记

## Day 7: SQL 子查询(Subquery)

### 今日完成

* 跟练:标量子查询 / IN 子查询 / EXISTS 相关子查询 / FROM 派生表 / NOT IN 的 NULL 陷阱
* 完成 10 道练习题,7 道完全正确,3 道有 bug(题 4、题 7,及题 9 别名问题)

### 学到的关键点

**子查询的四种位置**

|位置|形态|用途|
|-|-|-|
|`WHERE total > (...)`|标量子查询(1 行 1 列)|把聚合值算出来再比较,解决「WHERE 不能用聚合函数」|
|`WHERE x IN (...)`|IN 子查询(1 列多行)|先圈出一批 key,再捞这批 key 的明细|
|`WHERE EXISTS (...)`|相关子查询|内层引用外层列,逐行判断「存不存在匹配行」|
|`FROM (...) AS t`|派生表|把聚合结果当临时表,做「两步聚合」;派生表必须起别名|

* **标量子查询**只返回一个值,可当普通数字用。`WHERE` 拿它比较时,子查询里的 `AS 别名` 没意义,应删掉。
* **EXISTS 心智模型**:`SELECT 1` 写什么不重要,EXISTS 只看子查询「有没有返回行」——有→TRUE,无→FALSE。
* **相关子查询**内层引用了外层列,必须跟着外层逐行跑,数据量大时慢(Week 4 窗口函数有更优写法)。
* **FROM 派生表**会逼你把同一段子查询写两遍 —— 这个痛点正是 Week 5 `CTE`(`WITH ... AS`)要解决的。

**NOT IN 的 NULL 陷阱 ⚠️(高频面试坑)**

* `id NOT IN (101, 102, NULL)` 展开为 `id!=101 AND id!=102 AND id!=NULL`,而 `id != NULL` 结果是 `UNKNOWN` → 整个 AND 永远无法为 TRUE → **返回 0 行**。
* `NOT EXISTS` 逐行判断「存不存在匹配」,NULL 不影响其他行,结果正确。
* **铁律:子查询列可能有 NULL → 永远用 `NOT EXISTS`,不要用 `NOT IN`。**

**环境 / 工具**

* `%%sql` 是 cell magic,**必须是 cell 第一行**,前面有注释/空行会被当 Python 跑,报 `SyntaxError`。
* jupysql 渲染不出 `DESCRIBE` 时,用 `SELECT \\\\\\\\\\\\\\\* FROM (DESCRIBE ...)` 包一层。
* 视图:`CREATE OR REPLACE VIEW`,只存查询不存数据,永远和源 CSV 同步。

### 卡住/印象深的题

* **题 4**:相关子查询里多写 `GROUP BY` 画蛇添足;末尾多逗号(#17 复发);「高/低」要用 `CASE WHEN`。
* **题 7**:`FROM sales` 导致客户重复——sales 是订单表,要「客户」就 `SELECT DISTINCT`。**粒度问题**。
* **题 9**:`HAVING` 用了 SELECT 别名(#15 复发),标准写法 `HAVING SUM(total) > (...)`。

### 概念顿悟

* 子查询本质是「先算中间结果,再用它」——和 Python「先存变量再用」是同一回事。
* 题目主体是「订单」还是「客户」,决定 `FROM` 谁、要不要 `DISTINCT` —— **粒度意识**是核心。

\---

## Day 8: SQL JOIN(多表连接)

### 今日完成

* 跟练:造练习表 customers/countries → INNER / LEFT / RIGHT / FULL JOIN → 链式 JOIN
* 完成 10 道练习题,8 道完全正确,题 6 有 bug,题 3 跳过验证步骤

### 学到的关键点

**四种 JOIN 总览**

|JOIN 类型|保留谁|一句话|
|-|-|-|
|`INNER JOIN`|只保留两边都匹配上的|交集|
|`LEFT JOIN`|左表全保留 + 右表匹配项,匹配不上填 NULL|左表为主|
|`RIGHT JOIN`|右表全保留(LEFT 镜像)|少用,可改写成 LEFT|
|`FULL JOIN`|两边全保留|并集|

* JOIN 三件套:`FROM 主表 别名` + `JOIN 表 别名` + `ON 连接条件`。
* **核心心智模型**:JOIN 前先问「要不要保留匹配不上的行」——要→LEFT/FULL,不要→INNER。

**LEFT JOIN + IS NULL = 反向筛选**

* `LEFT JOIN ... WHERE 右表列 IS NULL` = 找「左表有、右表没有」的孤儿行。
* 和 Day 7 的 `NOT EXISTS` 是同一需求的两种写法。注意 `IS NULL` 不是 `= NULL`。

**JOIN 后的 NULL —— 必须主动处理 ⚠️**

* LEFT/FULL JOIN 后匹配不上处是 NULL,直接 `GROUP BY` 会多出裸 `None` 组。
* 结果里不该出现裸 NULL —— 要么 INNER 排除,要么 `COALESCE(列, 'Unknown')` 命名。
* 统计行数用 `COUNT(\\\\\\\\\\\\\\\*)`,不要用 `COUNT(某列)`(那列有 NULL 会少数)。

**JOIN 膨胀 / fan-out ⚠️(粒度问题的危险形态)**

* LEFT JOIN 规则:左表每行去右表找**所有**匹配行,有几行就复制成几行。
* 右表连接 key 不唯一(一对多/多对多)→ 左表对应行被复制膨胀。
* 后果:对膨胀结果做 `SUM`/`COUNT`,数值**虚高**,且不报任何错 —— 极隐蔽。
* **铁律:JOIN 前先确认右表的连接 key 是否唯一。**

### 卡住/印象深的题

* **题 6**(真 bug):LEFT JOIN 后 `GROUP BY region` 多出 `None` 组,没做「要不要算未匹配订单」的决策。
* **题 3**:用 INNER 但跳过「先查 country 是否对齐」——sales 有 UK、countries 没有,INNER 会悄悄丢 UK 订单。
* **题 2**:`LIMIT 20` 前 10 行恰好都匹配上,没看到 NULL 行,漏了题目要求的观察。
* **题 10**:现象抓对但用词糙——不是「promo 有两个 C003 订单」,是「C003 这个 key 出现两次」。

### 概念顿悟

* 子查询是「查询套查询」,JOIN 是「表接表」,两者常可互换。
* JOIN 之后第一件事:问「有没有未匹配行?该排除还是 COALESCE 命名?」

\---

## Day 9: Python 函数进阶 + 异常处理 + 文件 IO

### 今日完成

* 跟练:默认参数 / `\\\\\\\\\\\\\\\*args`·`\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs` / 类型注解 / 装饰器 / try-except / 文件 IO
* 完成 10 道练习题,7 道完全正确,题 5、题 7、题 10 有 bug

### 学到的关键点

**默认参数的可变对象大坑**

* `def f(lst=\\\\\\\\\\\\\\\[])`:默认的 `\\\\\\\\\\\\\\\[]` 在**定义时只创建一次**,所有调用共享同一个 list。
* 正确:默认值用 `None`,函数内 `if lst is None: lst = \\\\\\\\\\\\\\\[]` 每次新建。
* 呼应 Day 1「list 是引用类型」。

**`\\\\\\\\\\\\\\\*args` / `\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs`**

* `\\\\\\\\\\\\\\\*args` 把多余位置参数打包成 tuple;`\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs` 把多余关键字参数打包成 dict。
* 反向:调用时 `f(\\\\\\\\\\\\\\\*列表)`、`f(\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*字典)` 是「拆包」。
* 混用顺序固定:普通参数 → `\\\\\\\\\\\\\\\*args` → `\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs`。

**类型注解**

* `def f(x: int) -> float` 只是「说明书」,**不强制、不做类型转换**。传错类型不报错。
* 数据岗写注解是专业习惯。

**装饰器**

* 装饰器 = 接收函数、返回新函数,「不改原函数给它加功能」。`@deco` 等于 `f = deco(f)`。
* wrapper 两件事不能忘:① 签名写 `(\\\\\\\\\\\\\\\*args, \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs)` ② `return` 原函数的返回值。

**异常处理 / 文件 IO**

* `try/except/finally`,`finally` 无论成败都执行。`except (A, B)` 可一次抓多种。
* 文件永远用 `with open(...)`,自动关闭(相当于自动 try/finally)。Windows 上务必写 `encoding='utf-8'`。
* 读文件用 `for line in f` 流式逐行读,不要 `readlines()` 一次性读进内存。

### 卡住/印象深的题

* **题 5**:漏了「文件不存在返回 `\\\\\\\\\\\\\\\[]`」——没包 `FileNotFoundError`,题目明说要测却没测。
* **题 7**(真 bug):装饰器 wrapper 没 `return result`,导致原函数返回值变 None;且漏 `\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs`。
* **题 10**(真 bug):用 `.get("country")` 想靠 `except KeyError` 抓缺字段——但 `.get()` 缺键返回 None 不抛错,导致 `None` 混进结果当 key。另有 `continue` 后写 `print` 的死代码。
* **题 8**:空文件时 `total\\\\\\\\\\\\\\\_sales / 0` 会崩,sales.csv 碰巧有数据没暴露。

### 概念顿悟

* `dict\\\\\\\\\\\\\\\["k"]` 缺键抛 KeyError;`dict.get("k")` 缺键返回 None 不抛错——「安全访问」是双刃剑,想要它报错时它偏不报。
* `process\\\\\\\\\\\\\\\_sales` / `summarize` 手写的就是 SQL 的 `GROUP BY ... SUM` 和聚合——理解了底层。
* 批量数据处理:坏数据该「跳过 + 记日志」,而不是崩溃,也不是默默丢。

\---

## 我的弱点清单(持续累计)

### 已克服 ✅

1. ~~习惯用 `for i in range(len(lst))`~~ ✅
2. ~~简单过滤循环应优先用列表推导式~~ ✅
3. ~~字典推导式语法~~ ✅

### 持续注意

4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. 永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`
7. 用 `min()` / `max()` 替代 if/else 取小取大
8. 变量名不要和外层变量重名
9. 优先用 Python 内置函数:`abs()`、`max()`、`zip()` 等
10. Python 排序是稳定的 —— 多次 sorted 可实现复杂多关键字排序
11. 可读性 > 优雅 —— 不要为了一行而损害代码可读性
12. 写 SQL 时再看一眼题目要求的列是哪几个(Day 4 题 2、8 教训)
13. SQL 返回 0 行的排查思路:先看数据范围,再怀疑查询
14. `LIKE` 大小写敏感,不确定大小写用 `ILIKE` 或 `LOWER()`
15. `HAVING` 里写聚合函数本身,不要用 SELECT 的别名(高频面试陷阱)—— ⚠️ Day 7 题 9 复发;Day 8/9 未犯
16. 别名可用性规则:`ORDER BY` 能用别名,`WHERE`/`GROUP BY`/`HAVING` 不能
17. `SELECT` 最后一列后面不要加逗号 —— ⚠️ Day 7 题 4 复发;Day 8/9 未犯
18. **边界初始化**:维护极值的问题,用 `float('-inf')` / `float('inf')` 初始化 —— ✅ Day 9 题 2 活用
19. **"碰巧对" ≠ "正确"**:写完代码自己造刁钻测试用例验证 —— ⚠️ Day 9 题 5/8/10 仍未养成习惯
20. **`NOT IN` 遇 NULL 全军覆没**:子查询列可能有 NULL 时,永远用 `NOT EXISTS`
21. **粒度意识**:分清「订单粒度」和「客户粒度」,决定 `FROM` 谁、是否要 `DISTINCT`
22. **碰新数据先 `DESCRIBE`**:列名、列类型以 `DESCRIBE` 为准,不凭记忆也不轻信别人
23. **相关子查询里不要画蛇添足加 `GROUP BY`**:`WHERE` 已锁定单组,再分组多余且可能报错
24. **`%%sql` 必须在 cell 第一行**:前面有注释/空行会被当 Python 跑,报 SyntaxError
25. **JOIN 后必查未匹配行(NULL)**:用 `COALESCE` 命名或决定排除,结果里别留裸 `None`
26. **JOIN 膨胀(fan-out)**:JOIN 前确认右表连接 key 唯一,否则 `SUM`/`COUNT` 虚高且不报错
27. **JOIN 前先验证两表 key 对齐情况**:别凭感觉选 INNER/LEFT,先查有没有匹配不上的
28. **`LIMIT N` 看到的不是全部**:题目要求观察某现象时,主动构造能看到该现象的查询
29. **装饰器 wrapper 两件事**:① 签名写 `(\\\\\\\\\\\\\\\*args, \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*kwargs)` ② 必须 `return` 原函数的返回值
30. **`dict\\\\\\\\\\\\\\\["k"]` vs `dict.get("k")`**:前者缺键抛 KeyError,后者缺键返回 None 不抛错;想靠 except 抓缺字段就别用 `.get()`
31. **死代码**:`continue`/`return`/`break`/`raise` 之后的同层代码永远不执行
32. **文件流式读取**:用 `for line in f` 逐行读,不要 `readlines()` 一次性读进内存
33. **默认参数禁用可变对象**:不要 `def f(lst=\\\\\\\\\\\\\\\[])`,用 `None` 再在函数内新建

\---

## SQL ↔ Python 翻译表(持续扩充)

|操作|Python|SQL|
|-|-|-|
|取所有|`\\\\\\\\\\\\\\\[row for row in data]`|`SELECT \\\\\\\\\\\\\\\* FROM data`|
|过滤|`\\\\\\\\\\\\\\\[r for r in data if r\\\\\\\\\\\\\\\['x'] > 100]`|`WHERE x > 100`|
|选列|`\\\\\\\\\\\\\\\[r\\\\\\\\\\\\\\\['name'] for r in data]`|`SELECT name FROM data`|
|计算列|`\\\\\\\\\\\\\\\[r\\\\\\\\\\\\\\\['x'] \\\\\\\\\\\\\\\* 1.13 for r in data]`|`SELECT x \\\\\\\\\\\\\\\* 1.13 FROM data`|
|多条件|`if a > 0 and b in \\\\\\\\\\\\\\\[...]`|`WHERE a > 0 AND b IN (...)`|
|排序|`sorted(data, key=lambda r: r\\\\\\\\\\\\\\\['x'])`|`ORDER BY x`|
|降序|`sorted(data, key=lambda r: -r\\\\\\\\\\\\\\\['x'])`|`ORDER BY x DESC`|
|多关键字|`sorted(..., key=lambda r: (r\\\\\\\\\\\\\\\['a'], -r\\\\\\\\\\\\\\\['b']))`|`ORDER BY a ASC, b DESC`|
|取前 N|`sorted(...)\\\\\\\\\\\\\\\[:5]`|`LIMIT 5`|
|去重|`set(...)`|`SELECT DISTINCT`|
|模糊匹配|`r\\\\\\\\\\\\\\\['x'].startswith('M')`|`WHERE x LIKE 'M%'`|
|计数|`counts\\\\\\\\\\\\\\\[x] = counts.get(x,0)+1`|`GROUP BY x ... COUNT(\\\\\\\\\\\\\\\*)`|
|分组求和|嵌套 dict 累加|`GROUP BY x ... SUM(...)`|
|二维透视|`defaultdict(lambda: defaultdict(int))`|`GROUP BY a, b ... SUM(...)`|
|组后过滤|先聚合再 `if` 筛选|`HAVING SUM(...) > N`|
|三元/高低判断|`'高' if x > avg else '低'`|`CASE WHEN x > avg THEN '高' ELSE '低' END`|
|先算中间值再用|`avg = sum(...)/len(...)` 后再比较|标量子查询 `WHERE x > (SELECT AVG ...)`|
|圈出一批 key 再捞|`keys = {...}; \\\\\\\\\\\\\\\[r for r in data if r\\\\\\\\\\\\\\\['k'] in keys]`|`WHERE k IN (SELECT ...)`|
|判断是否存在|`any(... for ... in ...)`|`EXISTS (SELECT 1 ...)`|
|判断不存在|`not any(...)`|`NOT EXISTS (SELECT 1 ...)`|
|两步聚合|先 groupby 出中间结果,再对结果聚合|`FROM (SELECT ... GROUP BY ...) AS t`|
|按 key 关联两表|`dict` 查表 / `pd.merge`|`JOIN ... ON 左.k = 右.k`|
|找左表有右表没有的|`\\\\\\\\\\\\\\\[x for x in a if x not in b]`|`LEFT JOIN ... WHERE 右.k IS NULL` 或 `NOT EXISTS`|
|空值兜底|`a if a is not None else b` / `d.get(k, default)`|`COALESCE(a, b)`|
|分组求和(手写)|遍历 + `d\\\\\\\\\\\\\\\[k] = d.get(k,0) + v`|`GROUP BY k ... SUM(v)`|

\---

## Week 1 总结

### 数字

* 6 天,62 道练习题,正确率约 90%
* GitHub 连续 6 天提交

### 能力变化

* 代码风格:从「C/Java 式硬循环」→「Pythonic 推导式 + 声明式 SQL」
* 工具箱:list / tuple / dict / set / 推导式 / lambda / sorted + SQL 基础查询与聚合
* 思维:建立了「Python ↔ SQL」的翻译直觉,理解两者是同一套数据处理思想的不同表达

### 暴露的问题(Week 2 要改进)

1. **"碰巧对"现象**:多次出现输出正确但逻辑有 bug —— 需养成"自己造测试用例"的习惯
2. **边界条件**:极值初始化、空输入等边界情况考虑不足
3. **读题**:SQL 的 SELECT 列偶尔抄错

### Week 2 预告

* SQL:子查询、CTE、JOIN(多表连接)
* Python:函数进阶、异常处理、NumPy 入门
* 难度会上一个台阶,但 Week 1 的地基已经打牢

\---

## Day 7 当日小结

### 数字

* 10 道练习题,7 道完全正确,3 道有 bug(题 4 / 题 7 / 题 9)
* GitHub 连续提交保持

### 做得好的

* 题 6、题 10 主动写理解性注释
* 题 2 思考题(并列最大值)答到点上
* 题 8 "top-1 per group" 写得最干净

### 暴露的问题

1. 旧弱点复发:#15(HAVING 用别名)、#17(末尾逗号)
2. 粒度概念新坑:题 7 把订单表当客户表用
3. 相关子查询理解还不透:题 4 多加了无意义的 GROUP BY

\---

## Day 8 当日小结

### 数字

* 10 道练习题,8 道完全正确,题 6 有 bug
* GitHub 连续提交保持

### 做得好的

* 四种 JOIN 全部用对
* 题 4/5、题 8 的 JOIN ↔ 子查询双写法干净利落
* \#15、#17 旧弱点本次未犯,有进步

### 暴露的问题

1. JOIN 后 NULL 未处理:题 3/6/7 都碰到未匹配行,题 6 留了裸 None 组
2. 跳过验证步骤:题 3 没先查 key 对齐就选 INNER
3. LIMIT 误判:题 2 没看到 NULL 行就以为做完了

\---

## Day 9 当日小结

### 数字

* 10 道练习题,7 道完全正确,题 5 / 题 7 / 题 10 有 bug
* GitHub 连续提交保持

### 做得好的

* 类型注解全程坚持,专业习惯
* 题 2 用上 float('-inf')(#18 活用)
* 题 9 retry 装饰器写得完整漂亮
* 题 10 文字分析有数据岗思维

### 暴露的问题

1. **核心问题**:三个 bug 里两个都因「没造刁钻测试用例」——文件不存在、空文件、缺字段都没测(#19 仍未养成习惯)
2. 装饰器 wrapper 漏 return 返回值、漏 \*\*kwargs(题 7)
3. .get() 不抛 KeyError,误以为能靠 except 抓缺字段(题 10)
4. continue 后写死代码(题 10)

### Day 10 预告

* 异常处理 + 文件 IO 收尾,NumPy 入门
* 重点盯弱点 #19:写完代码先想「怎么把它搞崩」,再动手测


---

## Day 11: NumPy 入门 —— 数组、向量化、布尔索引、axis

### 今日完成
- 跟练:数组创建(ndarray/zeros/ones/arange/linspace)、向量化运算、布尔索引、axis参数、切片reshape、数学函数、广播、类型转换
- 完成 10 道练习题,5 道完全正确(题 1/2/4/5/7),5 道有 bug(题 3/6/8/9/10)

### 学到的关键点

**NumPy 核心心智模型**
| 概念 | 一句话 |
|------|--------|
| `ndarray` | 同类型、多维、向量化运算,速度比 Python list 快 10-100 倍 |
| 向量化 | 数组 ±×÷ 标量/数组,告别 for 循环 |
| 布尔索引 | `arr[arr > 0]` 当面具筛选,组合条件用 `&` + 括号 |
| axis | `axis=0` 压行(列汇总),`axis=1` 压列(行汇总) |
| 广播 | 形状不同的数组自动对齐,实现高级向量化 |
| 聚合 | `sum/mean/std/max/min/argmax/percentile/cumsum` |

**重要细节**
- `np.arange(100)` 生成 0~99,`np.linspace(0,1,5)` 等分5点
- `reshape(-1, 3)` 中 `-1` 表示自动计算该维度
- 布尔数组的 `.mean()` = True 的比例,是算占比的利器:`(arr > 0).mean()`
- `argmax()` 返回的是**索引**,不是值本身
- `np.std` 默认 `ddof=0`(总体标准差),统计学 Z-score 有时用 `ddof=1`
- 空数组 `.mean()` 返回 `nan` 并报警告;全0数组做 Z-score 会除零出 `inf`

**量化场景高频**
- 日收益率 → 累计收益率:`np.cumsum(returns)`
- 投资组合日收益 = `(returns_matrix * weights.reshape(3,1)).sum(axis=0)`
- Z-score 标准化 = `(features - mean) / std`,用 `keepdims=True` 保持维度用于广播

### 卡住/印象深的题
- **题 3**:把「替换为 60」写成「替换为 0」——读题失误(#42)
- **题 6**:把「正收益天数占比」算成「正收益平均值」——读题失误(#42 复发)
- **题 8**:投资组合收益率的 shape 搞混,`np.mean(portfolio)` 算的是 15 个元素的均值,不是 5 天组合收益的均值——**量化计算必须时刻跟踪变量的 shape 和语义**(#43)
- **题 9**:验证标准化时验证的是原始数据的均值/标准差,而不是标准化后的结果——做了 A 却验证 B(#44)
- **题 10**:边界测试没做,只写了「请教如何系统做边界测试」——需要建立边界测试的 checklist

### 概念顿悟
- **布尔数组 `.mean()` 算占比**:`True=1, False=0`,`(returns > 0).mean()` 直接得正收益天数占比
- **广播的本质是「拉伸」**:weights (3,) → reshape (3,1) → 自动复制成 (3,5) 与矩阵对齐
- **向量化 = SQL 的集合操作思想在数组上的实现**:一次操作整个集合,而不是逐行循环

---

## 我的弱点清单(持续累计)

### 已克服 ✅
1. ~~习惯用 `for i in range(len(lst))`~~ ✅
2. ~~简单过滤循环应优先用列表推导式~~ ✅
3. ~~字典推导式语法~~ ✅

### 持续注意
4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. 永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`
7. 用 `min()` / `max()` 替代 if/else 取小取大
8. 变量名不要和外层变量重名
9. 优先用 Python 内置函数:`abs()`、`max()`、`zip()` 等
10. Python 排序是稳定的 —— 多次 sorted 可实现复杂多关键字排序
11. 可读性 > 优雅 —— 不要为了一行而损害代码可读性
12. 写 SQL 时再看一眼题目要求的列是哪几个(Day 4 题 2、8 教训)
13. SQL 返回 0 行的排查思路:先看数据范围,再怀疑查询
14. `LIKE` 大小写敏感,不确定大小写用 `ILIKE` 或 `LOWER()`
15. `HAVING` 里写聚合函数本身,不要用 SELECT 的别名 —— ⚠️ Day 7 题 9 复发;Day 8/9/10 未犯
16. 别名可用性规则:`ORDER BY` 能用别名,`WHERE`/`GROUP BY`/`HAVING` 不能
17. `SELECT` 最后一列后面不要加逗号 —— ⚠️ Day 7 题 4 复发;之后未犯
18. **边界初始化**:维护极值用 `float('-inf')` / `float('inf')` —— ✅ Day 9 题 2 活用
19. **"碰巧对" ≠ "正确"**:写完代码自己造刁钻测试用例验证 —— ⚠️ Day 9/10 反复冒头(题 5 定义不运行、题 8 没列边界),头号待克服
20. **`NOT IN` 遇 NULL 全军覆没**:子查询列可能有 NULL 时,永远用 `NOT EXISTS`
21. **粒度意识**:分清「订单粒度」和「客户粒度」,决定 `FROM` 谁、是否要 `DISTINCT`
22. **碰新数据先 `DESCRIBE`**:列名、列类型以 DESCRIBE 为准
23. **相关子查询里不要画蛇添足加 `GROUP BY`**:`WHERE` 已锁定单组
24. **`%%sql` 必须在 cell 第一行**:前面有注释/空行会报 SyntaxError
25. **JOIN 后必查未匹配行(NULL)**:用 `COALESCE` 命名或排除,别留裸 None
26. **JOIN 膨胀(fan-out)**:JOIN 前确认右表连接 key 唯一,否则 SUM/COUNT 虚高
27. **JOIN 前先验证两表 key 对齐情况**:别凭感觉选 INNER/LEFT
28. **`LIMIT N` 看到的不是全部**:要观察现象就主动构造能看到它的查询
29. **装饰器 wrapper 两件事**:① 签名 `(*args, **kwargs)` ② 必须 `return` 原函数返回值
30. **`dict["k"]` vs `dict.get("k")`**:前者缺键抛 KeyError,后者缺键返回 None 不抛错;想靠 except 抓缺字段就用 `[]` —— ✅ Day 10 题 7 改对
31. **死代码**:`continue`/`return`/`break`/`raise` 之后的同层代码永不执行 —— ✅ Day 10 题 7 避开
32. **文件流式读取**:用 `for line in f`,不要 `readlines()`
33. **默认参数禁用可变对象**:不要 `def f(lst=[])`,用 `None` 再函数内新建
34. **写文件 vs 读文件**:`open()` 传的是路径不是数据;写文件前该检查的是「数据非空」而非「文件存在」,读文件前才检查存在性
35. **异常分支要 `return` 提前退出**:文件不存在/坏行处理完要 `return`,别让代码继续往下走到会崩的地方
36. **类型转换要显式**:`float(record["total"])` 主动转,别依赖后续运算「碰巧」报错来拦坏数据
37. **写完必须运行**:定义了函数不调用 = 没测 = 不知道对不对(题 5 教训,#19 的具体形态)
38. **NumPy 向量化替代循环**:看到数组操作先想「能不能向量化」,别写 for
39. **axis=0 压行(列汇总),axis=1 压列(行汇总)**:记住「被消灭的维度」
40. **布尔数组 `.mean()` 算占比**:`(arr > 0).mean()` 直接得比例,不用 `sum/len`
41. **广播用 `keepdims=True` 保持维度**:`(features - mean_keepdims) / std_keepdims`
42. **读题再仔细**:题3把「替换为60」写成「0」、题6把「占比」看成「平均值」——和 Day 4 SELECT 列抄错是同类问题,连续两题复发 ⚠️
43. **量化计算跟踪 shape 和语义**:题8 `portfolio` 是 3×5 矩阵,但「投资组合日收益率」是 1×5 向量。做矩阵运算时必须清楚每个变量的 shape 代表什么 ⚠️
44. **验证要验证「结果」而非「输入」**:题9验证的是原始数据的均值/标准差,而不是标准化后的结果——做了 A 却验证 B ⚠️

---

## SQL ↔ Python 翻译表(持续扩充)

| 操作 | Python | SQL |
|------|--------|-----|
| 取所有 | `[row for row in data]` | `SELECT * FROM data` |
| 过滤 | `[r for r in data if r['x'] > 100]` | `WHERE x > 100` |
| 选列 | `[r['name'] for r in data]` | `SELECT name FROM data` |
| 计算列 | `[r['x'] * 1.13 for r in data]` | `SELECT x * 1.13 FROM data` |
| 多条件 | `if a > 0 and b in [...]` | `WHERE a > 0 AND b IN (...)` |
| 排序 | `sorted(data, key=lambda r: r['x'])` | `ORDER BY x` |
| 降序 | `sorted(..., key=lambda r: -r['x'])` | `ORDER BY x DESC` |
| 多关键字 | `sorted(..., key=lambda r: (r['a'], -r['b']))` | `ORDER BY a ASC, b DESC` |
| 取前 N | `sorted(...)[:5]` | `LIMIT 5` |
| 去重 | `set(...)` | `SELECT DISTINCT` |
| 模糊匹配 | `r['x'].startswith('M')` | `WHERE x LIKE 'M%'` |
| 计数 | `counts[x] = counts.get(x,0)+1` | `GROUP BY x ... COUNT(*)` |
| 分组求和 | 嵌套 dict 累加 | `GROUP BY x ... SUM(...)` |
| 二维透视 | `defaultdict(lambda: defaultdict(int))` | `GROUP BY a, b ... SUM(...)` |
| 组后过滤 | 先聚合再 `if` 筛选 | `HAVING SUM(...) > N` |
| 三元/高低判断 | `'高' if x > avg else '低'` | `CASE WHEN x > avg THEN '高' ELSE '低' END` |
| 先算中间值再用 | `avg = sum(...)/len(...)` 后比较 | 标量子查询 `WHERE x > (SELECT AVG ...)` |
| 圈出一批 key 再捞 | `keys={...}; [r for r in data if r['k'] in keys]` | `WHERE k IN (SELECT ...)` |
| 判断是否存在 | `any(...)` | `EXISTS (SELECT 1 ...)` |
| 判断不存在 | `not any(...)` | `NOT EXISTS (SELECT 1 ...)` |
| 两步聚合 | 先 groupby 出中间结果再聚合 | `FROM (SELECT ... GROUP BY ...) AS t` |
| 按 key 关联两表 | `dict` 查表 / `pd.merge` | `JOIN ... ON 左.k = 右.k` |
| 找左表有右表没有的 | `[x for x in a if x not in b]` | `LEFT JOIN ... WHERE 右.k IS NULL` / `NOT EXISTS` |
| 空值兜底 | `a if a is not None else b` / `d.get(k, default)` | `COALESCE(a, b)` |
| 分组求和(手写) | 遍历 + `d[k] = d.get(k,0) + v` | `GROUP BY k ... SUM(v)` |
| 分组计数+求和 | `d[k]={'count':0,'sum':0}` 累加 | `GROUP BY k ... COUNT(*), SUM(v)` |
| **数组筛选** | `arr[arr > 0]` | `WHERE x > 0` |
| **数组聚合(行)** | `arr.sum(axis=1)` | `GROUP BY 行 ... SUM(...)` |
| **数组聚合(列)** | `arr.mean(axis=0)` | `SELECT AVG(col1), AVG(col2) ...` |
| **累计和** | `np.cumsum(arr)` | `SUM(...) OVER (ORDER BY ...)` (窗口函数) |
| **占比** | `(arr > 0).mean()` | `COUNT(CASE WHEN x>0 THEN 1 END) * 1.0 / COUNT(*)` |
| **Z-score 标准化** | `(arr - mean) / std` | `(x - AVG(x)) / STDDEV(x)` |
| **矩阵加权求和** | `(matrix * weights).sum(axis=0)` | `SUM(x * w) GROUP BY day` |

---

## Day 11 当日小结

### 数字
- 10 道练习题,5 道完全正确(1/2/4/5/7),5 道有 bug(3/6/8/9/10)
- 正确率约 50%(严格标准) / 70%(宽松标准,题10前5问正确)
- GitHub 连续提交保持

### 做得好的
- **题 4/5/7 axis 和布尔索引全对**:axis=0/1、reshape、flatten、复合条件 `&` 都掌握
- **题 8 广播用法正确**:weights.reshape(3,1) 的广播操作本身是对的,只是后续聚合对象搞混
- **题 10 前5问全对**:综合题的主体逻辑链完整,从 daily total → category total → weekend vs weekday → 食品超标天数,层层递进正确

### 暴露的问题
1. **#42 读题失误复发**:题3和题6连续两题因读题不仔细写错,和 Day 4 SELECT 列抄错是同一类。需要建立「读题 checklist」:做完后回头扫一眼题目要求的输出格式/数值/列名。
2. **#43 量化计算的 shape 意识薄弱**:题8没分清「加权矩阵」(3×5)和「投资组合日收益率向量」(5,)的区别。做矩阵运算时应该在注释里写明每个变量的 shape。
3. **#44 验证对象错误**:题9做了标准化却验证原始数据。验证环节要问自己「我验证的是「输出」还是「输入」?」。
4. **边界测试未完成**:题10第6问没做。需要建立边界测试 checklist:空输入、全0、单元素、负数、极大值。

### 难度反馈
- 题8(投资组合)和题9(Z-score)难度贴合,但题8的「投资组合日收益率」概念需要更明确的题目引导
- 题10综合题前5问难度适中,第6问边界测试需要示例引导

### Day 12 预告
- Week 2 综合复习 + 阶段自测
- 混合练习:Python(函数/异常/NumPy) + SQL(JOIN/子查询) 交叉出题
- 检验 Week 2 六天学习成果,为 Week 3(Pandas 基础)做准备

---

## Day 12: Week 2 综合复习 —— 阶段自测

### 今日完成
- 回顾 Week 2 知识速查卡:SQL子查询四种位置/JOIN心智模型/NULL处理 + Python函数/异常/文件IO + NumPy向量化/axis/广播
- 完成综合场景演练:读取→清洗→NumPy分析→SQL验证→JSON报告
- 完成 10 道混合自测题,7 道完全正确,3 道有小问题(题2/5/10)
- 正确率约 70% 严格 / 90% 宽松

### 学到的关键点

**Week 2 知识串联**
- 数据岗工作流 = 读取 → 清洗 → 分析 → 验证 → 报告,每个环节都要异常安全
- SQL 子查询四种位置:WHERE比较/IN/EXISTS/FROM派生表 —— 根据「先算什么、再用什么」选择位置
- JOIN 先问「要不要保留匹配不上的行」→ 要→LEFT,不要→INNER
- JOIN 后三件事:查NULL/确认key唯一/确认粒度
- Python 异常处理:只抓预期类型、成功逻辑放else、坏数据跳过+log
- 文件IO:pathlib检查存在性、csv.DictReader值全是字符串、写CSV加newline=""
- NumPy:向量化替代循环、布尔索引算占比、axis记住「被消灭的维度」、广播用keepdims

**标量子查询 vs 相关子查询 vs 派生表**
| 场景 | 选哪个 | 原因 |
|------|--------|------|
| 用一个聚合值做比较 | 标量子查询 | 先算出一个数,再逐行比较 |
| 先圈出一批key再捞明细 | IN子查询 | 把子查询结果当集合用 |
| 判断存在/不存在 | EXISTS/NOT EXISTS | 只看有没有行,不返回具体值 |
| 先聚合再对聚合结果聚合 | 派生表 | 两步聚合,中间结果当临时表 |

**题10 边界测试教训**
- 边界测试不能只用文件名占位,必须实际创建测试文件
- 空文件 = 只有表头无数据行;全坏数据 = 有数据行但全部转换失败
- 测试前要确认文件存在:`Path(filename).exists()`

### 卡住/印象深的题
- **题 2**:用 try/except ZeroDivisionError 代替前置 if b==0 检查 —— 虽然结果对,但预期分支应该显式判断而非兜底捕获
- **题 5**:用 print 代替 logging.info —— 题目明确要求 logging,这是读题细节(小失误)
- **题 10**:empty.csv 和 all_bad.csv 不存在,导致「空文件」和「全坏数据」测试实际上测的是「文件不存在」——#19 的变体:测试用例要真实构造

### 概念顿悟
- **综合场景演练的价值**:把孤立知识点串联成工作流,比单题练习更接近真实数据岗工作
- **SQL 验证 Python 结果**:题1/4/7/9的 SQL 结果可以和 Python 分析交叉验证,两种工具互补
- **边界测试是「生产代码」和「练习代码」的分水岭**:练习题可以跳过边界,但生产代码必须处理

### 做得好的
- **Day 11 的读题失误(#42) 今天没复发**:题3占比、题4 COALESCE、题6空数组都正确处理
- **SQL 旧弱点未复发**:题9 HAVING 里写聚合函数本身,没有使用别名(#15 ✅)
- **异常处理全面**:题6/8/10 都处理了文件不存在、空数据、坏数据、字段缺失
- **NumPy 向量化熟练**:题3/8/10 都没有写 for 循环,全部向量化操作

### 暴露的问题
1. **#45 测试文件要真实存在**:题10的边界测试用了不存在的文件名,导致测试失效。边界测试必须实际构造数据。
2. **#5 小复发**:题5用 print 代替 logging.info —— 题目明确要求 logging,这是读题细节。
3. **#2 实现方式**:题2用异常捕获代替前置检查 —— 预期内的情况应该显式判断。

---

## 我的弱点清单(持续累计)

### 已克服 ✅
1. ~~习惯用 `for i in range(len(lst))`~~ ✅
2. ~~简单过滤循环应优先用列表推导式~~ ✅
3. ~~字典推导式语法~~ ✅

### 持续注意
4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. 永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`
7. 用 `min()` / `max()` 替代 if/else 取小取大
8. 变量名不要和外层变量重名
9. 优先用 Python 内置函数:`abs()`、`max()`、`zip()` 等
10. Python 排序是稳定的 —— 多次 sorted 可实现复杂多关键字排序
11. 可读性 > 优雅 —— 不要为了一行而损害代码可读性
12. 写 SQL 时再看一眼题目要求的列是哪几个(Day 4 题 2、8 教训)
13. SQL 返回 0 行的排查思路:先看数据范围,再怀疑查询
14. `LIKE` 大小写敏感,不确定大小写用 `ILIKE` 或 `LOWER()`
15. `HAVING` 里写聚合函数本身,不要用 SELECT 的别名 —— ✅ Day 12 题 9 正确
16. 别名可用性规则:`ORDER BY` 能用别名,`WHERE`/`GROUP BY`/`HAVING` 不能
17. `SELECT` 最后一列后面不要加逗号 —— ✅ Day 12 未犯
18. **边界初始化**:维护极值用 `float('-inf')` / `float('inf')` —— ✅ Day 9 题 2 活用
19. **"碰巧对" ≠ "正确"**:写完代码自己造刁钻测试用例验证 —— ⚠️ Day 12 题 10 边界测试文件不存在(#45)
20. **`NOT IN` 遇 NULL 全军覆没**:子查询列可能有 NULL 时,永远用 `NOT EXISTS`
21. **粒度意识**:分清「订单粒度」和「客户粒度」,决定 `FROM` 谁、是否要 `DISTINCT`
22. **碰新数据先 `DESCRIBE`**:列名、列类型以 DESCRIBE 为准
23. **相关子查询里不要画蛇添足加 `GROUP BY`**:`WHERE` 已锁定单组
24. **`%%sql` 必须在 cell 第一行**:前面有注释/空行会报 SyntaxError
25. **JOIN 后必查未匹配行(NULL)**:用 `COALESCE` 命名或排除,别留裸 None —— ✅ Day 12 题 4 正确
26. **JOIN 膨胀(fan-out)**:JOIN 前确认右表连接 key 唯一,否则 SUM/COUNT 虚高
27. **JOIN 前先验证两表 key 对齐情况**:别凭感觉选 INNER/LEFT
28. **`LIMIT N` 看到的不是全部**:要观察现象就主动构造能看到它的查询
29. **装饰器 wrapper 两件事**:① 签名 `(*args, **kwargs)` ② 必须 `return` 原函数返回值 —— ✅ Day 12 题 5 正确
30. **`dict["k"]` vs `dict.get("k")`**:前者缺键抛 KeyError,后者缺键返回 None 不抛错;想靠 except 抓缺字段就用 `[]` —— ✅ Day 10 题 7 改对
31. **死代码**:`continue`/`return`/`break`/`raise` 之后的同层代码永不执行 —— ✅ Day 10 题 7 避开
32. **文件流式读取**:用 `for line in f`,不要 `readlines()`
33. **默认参数禁用可变对象**:不要 `def f(lst=[])`,用 `None` 再函数内新建
34. **写文件 vs 读文件**:`open()` 传的是路径不是数据;写文件前该检查的是「数据非空」而非「文件存在」,读文件前才检查存在性
35. **异常分支要 `return` 提前退出**:文件不存在/坏行处理完要 `return`,别让代码继续往下走到会崩的地方 —— ✅ Day 12 题 6/8/10 正确
36. **类型转换要显式**:`float(record["total"])` 主动转,别依赖后续运算「碰巧」报错来拦坏数据 —— ✅ Day 12 题 6/8/10 正确
37. **写完必须运行**:定义了函数不调用 = 没测 = 不知道对不对
38. **NumPy 向量化替代循环**:看到数组操作先想「能不能向量化」,别写 for —— ✅ Day 12 题 3/8/10 正确
39. **axis=0 压行(列汇总),axis=1 压列(行汇总)**:记住「被消灭的维度」 —— ✅ Day 12 题 3/10 正确
40. **布尔数组 `.mean()` 算占比**:`(arr > 0).mean()` 直接得比例,不用 `sum/len` —— ✅ Day 12 题 3 正确
41. **广播用 `keepdims=True` 保持维度**:`(features - mean_keepdims) / std_keepdims`
42. **读题再仔细**:题3把「替换为60」写成「0」、题6把「占比」看成「平均值」—— ✅ Day 12 未复发
43. **量化计算跟踪 shape 和语义**:题8 `portfolio` 是 3×5 矩阵,但「投资组合日收益率」是 1×5 向量。做矩阵运算时必须清楚每个变量的 shape 代表什么 —— ✅ Day 12 未涉及此场景
44. **验证要验证「结果」而非「输入」**:题9验证的是原始数据的均值/标准差,而不是标准化后的结果——做了 A 却验证 B —— ✅ Day 12 未涉及此场景
45. **测试文件要真实存在**:题10的 empty.csv 和 all_bad.csv 不存在,导致边界测试失效。边界测试必须实际构造数据 ⚠️

---

## SQL ↔ Python 翻译表(持续扩充)

| 操作 | Python | SQL |
|------|--------|-----|
| 取所有 | `[row for row in data]` | `SELECT * FROM data` |
| 过滤 | `[r for r in data if r['x'] > 100]` | `WHERE x > 100` |
| 选列 | `[r['name'] for r in data]` | `SELECT name FROM data` |
| 计算列 | `[r['x'] * 1.13 for r in data]` | `SELECT x * 1.13 FROM data` |
| 多条件 | `if a > 0 and b in [...]` | `WHERE a > 0 AND b IN (...)` |
| 排序 | `sorted(data, key=lambda r: r['x'])` | `ORDER BY x` |
| 降序 | `sorted(..., key=lambda r: -r['x'])` | `ORDER BY x DESC` |
| 多关键字 | `sorted(..., key=lambda r: (r['a'], -r['b']))` | `ORDER BY a ASC, b DESC` |
| 取前 N | `sorted(...)[:5]` | `LIMIT 5` |
| 去重 | `set(...)` | `SELECT DISTINCT` |
| 模糊匹配 | `r['x'].startswith('M')` | `WHERE x LIKE 'M%'` |
| 计数 | `counts[x] = counts.get(x,0)+1` | `GROUP BY x ... COUNT(*)` |
| 分组求和 | 嵌套 dict 累加 | `GROUP BY x ... SUM(...)` |
| 二维透视 | `defaultdict(lambda: defaultdict(int))` | `GROUP BY a, b ... SUM(...)` |
| 组后过滤 | 先聚合再 `if` 筛选 | `HAVING SUM(...) > N` |
| 三元/高低判断 | `'高' if x > avg else '低'` | `CASE WHEN x > avg THEN '高' ELSE '低' END` |
| 先算中间值再用 | `avg = sum(...)/len(...)` 后比较 | 标量子查询 `WHERE x > (SELECT AVG ...)` |
| 圈出一批 key 再捞 | `keys={...}; [r for r in data if r['k'] in keys]` | `WHERE k IN (SELECT ...)` |
| 判断是否存在 | `any(...)` | `EXISTS (SELECT 1 ...)` |
| 判断不存在 | `not any(...)` | `NOT EXISTS (SELECT 1 ...)` |
| 两步聚合 | 先 groupby 出中间结果再聚合 | `FROM (SELECT ... GROUP BY ...) AS t` |
| 按 key 关联两表 | `dict` 查表 / `pd.merge` | `JOIN ... ON 左.k = 右.k` |
| 找左表有右表没有的 | `[x for x in a if x not in b]` | `LEFT JOIN ... WHERE 右.k IS NULL` / `NOT EXISTS` |
| 空值兜底 | `a if a is not None else b` / `d.get(k, default)` | `COALESCE(a, b)` |
| 分组求和(手写) | 遍历 + `d[k] = d.get(k,0) + v` | `GROUP BY k ... SUM(v)` |
| 分组计数+求和 | `d[k]={'count':0,'sum':0}` 累加 | `GROUP BY k ... COUNT(*), SUM(v)` |
| **数组筛选** | `arr[arr > 0]` | `WHERE x > 0` |
| **数组聚合(行)** | `arr.sum(axis=1)` | `GROUP BY 行 ... SUM(...)` |
| **数组聚合(列)** | `arr.mean(axis=0)` | `SELECT AVG(col1), AVG(col2) ...` |
| **累计和** | `np.cumsum(arr)` | `SUM(...) OVER (ORDER BY ...)` (窗口函数) |
| **占比** | `(arr > 0).mean()` | `COUNT(CASE WHEN x>0 THEN 1 END) * 1.0 / COUNT(*)` |
| **Z-score 标准化** | `(arr - mean) / std` | `(x - AVG(x)) / STDDEV(x)` |
| **矩阵加权求和** | `(matrix * weights).sum(axis=0)` | `SUM(x * w) GROUP BY day` |
| **数据管道验证** | Python NumPy 计算 | SQL 聚合验证 | 两者交叉验证

---

## Day 12 当日小结

### 数字
- 10 道混合练习题,7 道完全正确,3 道有小问题(题2/5/10)
- 正确率约 70%(严格标准) / 90%(宽松标准)
- GitHub 连续提交保持
- Week 2 累计:Day 7-12,72 道练习题,正确率约 75%

### 做得好的
- **SQL 题(1/4/7/9) 全部正确**:子查询、JOIN、COALESCE、派生表、HAVING 都掌握扎实
- **NumPy 题(3/8/10) 全部正确**:向量化、布尔索引、axis、广播、统计聚合熟练
- **Day 11 读题失误未复发**:占比、COALESCE、空数组处理都正确(#42 克服中)
- **旧弱点未复发**:HAVING 用别名(#15)、末尾逗号(#17)、JOIN 后 NULL(#25) 都正确
- **异常处理全面**:文件不存在、空数据、坏数据、字段缺失都处理了

### 暴露的问题
1. **#45 边界测试文件要真实存在**:题10的 empty.csv 和 all_bad.csv 不存在,导致测的是「文件不存在」而非「空文件/全坏数据」。边界测试必须实际构造数据,不能只用文件名占位。
2. **#5 小复发**:题5用 print 代替 logging.info —— 题目明确要求 logging。
3. **#2 实现方式**:题2用 try/except ZeroDivisionError 代替前置 if b==0 —— 预期内分支应该显式判断。

### Week 2 总结

#### 数字
- 6 天,72 道练习题,正确率约 75%
- GitHub 连续 12 天提交

#### 能力变化
- SQL:从单表查询 → 子查询(四种位置) → JOIN(四种类型) → 派生表两步聚合
- Python:从基础函数 → 装饰器 → 异常处理(logging/自定义异常/异常链) → 文件IO(csv/json/pathlib)
- NumPy:从数组创建 → 向量化 → 布尔索引 → axis → 广播 → 量化场景(收益率/投资组合/Z-score)
- 工作流:建立了「读取→清洗→分析→验证→报告」的完整数据管道思维

#### 暴露的问题(Week 3 要改进)
1. **边界测试习惯**:多次出现「测试用例没真正构造」(#19/#45),需要建立边界测试 checklist
2. **读题细节**:虽比 Day 11 好,但题5的 logging 要求、题2的「除零时」表述仍需注意
3. **实现方式选择**:预期内情况用 if 前置检查,异常用 try/except 兜底

### Week 3 预告
- Pandas 基础:Series/DataFrame/读写/筛选/索引
- SQL 字符串函数 + 日期处理
- 这是从「NumPy 数组」到「表格数据」的过渡,Pandas 是数据岗的核心工具

### Day 13 预告
- Pandas 基础:DataFrame 创建、基本属性、读写 CSV/Excel
- 理解 Series 和 DataFrame 的关系(Series 是带索引的 NumPy 数组)

