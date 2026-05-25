# Week 1 学习笔记

> **主题**:Python 基础(list/tuple/dict/set/推导式/lambda) + SQL 入门(SELECT/WHERE/GROUP BY/HAVING)
> **完成情况**:6 天,62 道练习题(Python 40 + SQL 22),正确率约 90%

---

## Day 1: Python list & tuple

### 今日完成
- 复习 list 基础操作、切片、排序、引用机制
- 完成 10 道练习题(见 day01_exercises.ipynb)

### 学到的关键点
- list 是引用类型,赋值会共享内存,需要 `.copy()` 才是真复制
- `sort()` 原地修改并返回 None,`sorted()` 返回新 list
- tuple 不可变,可以作为 dict 的 key
- Python 切片越界不会报错,会自动截断(如 `lst[i:i+n]`)

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
- **计数模式**:`counts[x] = counts.get(x, 0) + 1`
- **分组模式**:`if k not in groups: groups[k] = []; groups[k].append(v)`
  - 这个模式就是 Pandas `groupby` 的思想雏形
  - 进阶可用 `from collections import defaultdict; defaultdict(list)` 省掉 if 判断
- 字典推导式语法:`{key表达式: value表达式 for 变量 in 序列}`
- set 用 `set()` 建空集合,`{}` 是空 dict(易错点)
- set 运算的实际应用:用户留存/流失/新增分析(`今天 & 昨天`、`昨天 - 今天`、`今天 - 昨天`)
- `Counter` 是 dict 的计数特化版,支持直接相加(`Counter(a) + Counter(b)`)

### 卡住/印象深的题
- 第 7 题(用户消费排序):碰巧输出对了,但实际上忘了 `sorted()` 排序步骤 —— **隐藏 bug**
- 第 8 题(字典推导):用了普通 for 循环,没真正用字典推导式
- 第 10 题(带重复次数的交集):写出了正确答案但代码冗余,学到 `min()` + `extend([k] * n)`;这道题其实是 `pd.merge` 的手写原理

### 概念顿悟
- 第 6 题"按用户分组求金额 list"和 SQL 的 `GROUP BY user` 是同一件事
- 第 10 题"两个 list 求交集"本质上就是 `pd.merge` 的雏形
- **dict 是 Pandas 的基础,理解 dict 操作就理解了 80% 的 DataFrame 操作**

---

## Day 3: 列表推导式深入 + lambda + 高阶函数

### 今日完成
- 列表推导式进阶:过滤(if 在后)、三元表达式(if 在前)、双重嵌套
- 字典推导式 + 集合推导式
- lambda 匿名函数的本质和典型用途
- `sorted` 的 key 参数 + 多关键字排序
- `map` / `filter` 的概念(知道存在,实际用列表推导式更 Pythonic)
- 完成 10 道练习题(含 5c 矩阵转置、10 二维数据透视等挑战题)

### 学到的关键点
- **三元表达式 vs 过滤的位置区别**(重要,Day 6 自测答错过):
  - `[x for x in seq if cond]` → if 在**后** → **过滤**,decide 元素**保不保留**
  - `[x if cond else y for x in seq]` → if 在**前** → **三元表达式**,decide 每个元素**输出成什么**(全部保留)
  - 区别不是"筛选顺序",而是「过滤」vs「变换」
- **多关键字排序**:`key=lambda x: (主关键字, -次关键字)`,负号实现降序
  - 字符串次关键字不能加负号,解决方法:**Python 排序是稳定的,可以分两次 sorted**(从次到主)
- **嵌套列表推导**:外层循环写在前,内层循环写在后,顺序和普通双重 for 一致
  - 矩阵转置经典模板:`[[row[i] for row in matrix] for i in range(len(matrix[0]))]`
- **lambda 主要用作"参数"传给其他函数**;独立赋值用 def 更清晰
- **Python 内置工具要熟悉**:`abs()`、`max()`、`min()`、`sum()`、`zip()`
- **defaultdict 简化嵌套 dict**:`defaultdict(lambda: defaultdict(int))`

### 卡住/印象深的题
- 第 5c(矩阵转置):学到经典模板「外层 i 控制列、内层取每行第 i 个」
- 第 9(订单过滤排序提取):发现这就是 SQL 的 `WHERE + ORDER BY + SELECT`
- 第 10(二维数据透视):手写实现了 Pandas `pivot_table` 的核心逻辑

### 概念顿悟
- **列表推导式 ≈ SQL 的 WHERE + SELECT**
- **sorted + lambda ≈ SQL 的 ORDER BY**
- **嵌套 dict 累加 ≈ Pandas pivot_table**
- Python 高阶函数和列表推导式 = SQL 思想在 Python 里的实现,反之亦然

---

## Day 4: SQL 入门 —— SELECT / WHERE / ORDER BY / LIMIT

### 今日完成
- 环境搭建:DuckDB + jupysql,直接查询 csv,无需建表
- 用 Pandas 生成 500 行覆盖全年的练习数据 `data/sales.csv`
- SQL 基础四件套:SELECT、WHERE、ORDER BY、LIMIT
- 列别名 AS、列表达式计算、DISTINCT 去重
- 字符串模糊匹配:LIKE / ILIKE
- 多条件组合:AND / OR / IN / BETWEEN / NOT IN
- 完成 10 道练习题(基于 sales.csv)

### 学到的关键点
- **SQL 字符串用单引号 `'...'`**,双引号是给列名/表名用的
- **NULL 比较必须用 `IS NULL` / `IS NOT NULL`**,`= NULL` 永远返回 0 行
- **DuckDB 可以直接 `SELECT * FROM 'xxx.csv'`**
- **SQL 的执行顺序**(必背):
  - 书写顺序:`SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT`
  - 执行顺序:`FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`
- **多关键字排序**:`ORDER BY col1 ASC, col2 DESC`
- **LIKE 大小写敏感**,跨大小写用 `ILIKE` 或 `LOWER()`
- **Top N 模式**:`ORDER BY xxx DESC LIMIT N`
- **返回 0 行的排查思路**:先验证数据范围(MIN/MAX/COUNT),再怀疑 SQL

### 卡住/印象深的题
- 题 2、题 8:**都是 SELECT 列没看仔细**,WHERE 写对了但选错列
- 题 6:SQL 完全正确但返回 0 行 —— 0 行不一定是 SQL 错,可能数据本来就没有

---

## Day 5: SQL 聚合 —— COUNT / SUM / AVG / GROUP BY / HAVING

### 今日完成
- 聚合函数:COUNT / SUM / AVG / MIN / MAX / ROUND
- COUNT 三兄弟:`COUNT(*)` / `COUNT(col)` / `COUNT(DISTINCT col)`
- GROUP BY 单列分组、多列分组
- HAVING 对分组结果过滤
- WHERE + HAVING 组合使用
- 完成 10 道练习题 + 1 道附加思考题

### 学到的关键点
- **GROUP BY 黄金法则**:SELECT 里的非聚合列,必须出现在 GROUP BY 里
- **COUNT 三兄弟的区别**:
  - `COUNT(*)` —— 所有行(含 NULL)
  - `COUNT(col)` —— col 非 NULL 的行数
  - `COUNT(DISTINCT col)` —— col 不重复值的个数
- **WHERE vs HAVING**:
  - WHERE = 分组**前**过滤**原始行**,不能用聚合函数
  - HAVING = 分组**后**过滤**分组结果**,可以用聚合函数
- **别名可用性规则**(重要面试陷阱):
  | 子句 | 能用 SELECT 别名? | 原因 |
  |------|------------------|------|
  | WHERE | ❌ | WHERE 在 SELECT 之前执行 |
  | GROUP BY | ⚠️ 部分数据库可以 | GROUP BY 在 SELECT 之前 |
  | HAVING | ⚠️ DuckDB 容忍,标准上不行 | HAVING 在 SELECT 之前 |
  | ORDER BY | ✅ | ORDER BY 在 SELECT 之后 |
  - 口诀:**ORDER BY 能用别名,它之前的子句都不能**
- **SELECT 最后一列后面不要加逗号** —— DuckDB 容忍,换数据库会报错

### 卡住/印象深的题
- 题 7、8、10:**HAVING 里用了别名** —— DuckDB 容忍了,但 PostgreSQL 会报错。
  正确做法:HAVING 里写 `SUM(total)`、`COUNT(*)` 本身,不写别名
- 附加思考题(GROUP BY country 却 SELECT product)答对了:黄金法则理解到位

---

## Day 6: Week 1 综合复习 + 阶段自测

### 今日完成
- 12 道混合练习(Python / SQL 自己判断该用哪个工具)
- Week 1 自测清单(Python 5 项 + SQL 5 项)
- 完成 11/12 题(第 12 题子查询为 Day 7 预告,暂未做)

### 学到的关键点 / 重要教训
- **"维护最大 + 第二大"类问题,初始化要用 `float('-inf')`(负无穷)**,
  不能用 `nums[0]` —— 否则比初始值小的合理候选数会被永久挡住
  - bug 复现:`[3, 1, 2]` 用 `nums[0]` 初始化会错误输出 3(应为 2)
- **STRFTIME 比 SUBSTRING 更专业**:处理日期用 `STRFTIME(date, '%Y-%m')`,
  字符串截取在格式变化时会出错,日期函数不会
- **文本解析的稳健思路**:先拆成逻辑单元(句子),再取需要的部分,
  不要依赖"每句固定 N 个词"这种脆弱假设
- **进阶预告**:子查询(查询里套查询),Day 7 正式学

### Week 1 最重要的一条经验
本周至少 3 次"输出对了但代码有 bug"(Day 2 题7、Day 4 题6、Day 6 题1):
> **写完代码不要只用题目给的例子测试。要自己造"刁钻"的测试用例**
> —— 空输入、全负数、有重复值、极端值、第一个元素是最大/最小值等。
这是初级和中级工程师的分水岭。

---

# Week 2 学习笔记

## Day 7: SQL 子查询(Subquery)

### 今日完成
- 跟练:标量子查询 / IN 子查询 / EXISTS 相关子查询 / FROM 派生表 / NOT IN 的 NULL 陷阱
- 完成 10 道练习题,7 道完全正确,3 道有 bug(题 4、题 7,及题 9 别名问题)

### 学到的关键点

**子查询的四种位置**
| 位置 | 形态 | 用途 |
|------|------|------|
| `WHERE total > (...)` | 标量子查询(1 行 1 列) | 把聚合值算出来再比较,解决「WHERE 不能用聚合函数」 |
| `WHERE x IN (...)` | IN 子查询(1 列多行) | 先圈出一批 key,再捞这批 key 的明细 |
| `WHERE EXISTS (...)` | 相关子查询 | 内层引用外层列,逐行判断「存不存在匹配行」 |
| `FROM (...) AS t` | 派生表 | 把聚合结果当临时表,做「两步聚合」;派生表必须起别名 |

- **标量子查询**只返回一个值,可当普通数字用。`WHERE` 拿它来比较时,子查询里的 `AS 别名` 没意义(外层不会引用),应删掉。
- **EXISTS 心智模型**:`SELECT 1` 写什么不重要,EXISTS 只看子查询「有没有返回行」——有→TRUE,无→FALSE。
- **不相关子查询**能独立运行;**相关子查询**内层引用了外层列(如 `WHERE s2.country = s1.country`),必须跟着外层逐行跑,数据量大时慢(Week 4 窗口函数有更优写法)。
- **FROM 派生表**会逼你把同一段子查询写两遍 —— 这个痛点正是 Week 5 `CTE`(`WITH ... AS`)要解决的。

**NOT IN 的 NULL 陷阱 ⚠️(高频面试坑)**
- `id NOT IN (101, 102, NULL)` 展开为 `id!=101 AND id!=102 AND id!=NULL`,
  而 `id != NULL` 结果是 `UNKNOWN`(不是 TRUE),整个 AND 永远无法为 TRUE → **返回 0 行**。
- `NOT EXISTS` 是逐行判断「存不存在匹配」,NULL 匹配不上不影响其他行,结果正确。
- **铁律:子查询列可能有 NULL → 永远用 `NOT EXISTS`,不要用 `NOT IN`。**

**环境 / 工具**
- `%%sql` 是 cell magic,**必须是 cell 的第一行**,前面不能有注释或空行,否则整格被当 Python 跑,报 `SyntaxError`。
- `%sql` 行魔法管一行,`%%sql` 单元格魔法管一格。
- jupysql 渲染不出 `DESCRIBE` 时,用 `SELECT * FROM (DESCRIBE ...)` 包一层。
- 视图:`CREATE OR REPLACE VIEW sales AS SELECT ...`,只存查询不存数据,永远和源 CSV 同步。

### 卡住/印象深的题
- **题 4**(相关子查询):子查询里多写了 `GROUP BY category` —— 相关子查询已用 `WHERE` 锁定到单个品类,再分组属画蛇添足,还可能让标量子查询返回多行报错。另外末尾多了逗号(弱点 #17 复发)。「高/低」要用 `CASE WHEN`(= SQL 版三元表达式)。
- **题 7**(EXISTS):`FROM sales` 导致客户重复 —— sales 是订单表,一个客户多笔订单就重复多次。题目要「客户」就该 `SELECT DISTINCT customer_id`,或从 `(SELECT DISTINCT customer_id FROM sales)` 出发。**这是「粒度(granularity)」问题**。
- **题 9**:`HAVING total_sales > (...)` 用了 SELECT 别名 —— 弱点 #15 复发,标准写法 `HAVING SUM(total) > (...)`。
- **题 5**:两段本应相同的派生子查询写得不一致(`t` 有 customer_id,`t2` 没有),碰巧外层没用到才没出错 —— 又一次「碰巧对」。

### 概念顿悟
- 子查询的本质是「先算出一个中间结果,再用它」—— 和 Python 里「先存一个变量,再用这个变量」是同一回事。
- 题目主体是「订单」还是「客户」,决定 `FROM` 谁、要不要 `DISTINCT` —— **粒度意识**是数据分析的核心。

---

## Day 8: SQL JOIN(多表连接)

### 今日完成
- 跟练:造练习表 customers/countries → INNER / LEFT / RIGHT / FULL JOIN → 链式 JOIN
- 完成 10 道练习题,8 道完全正确,题 6 有 bug,题 3 跳过验证步骤

### 学到的关键点

**四种 JOIN 总览**
| JOIN 类型 | 保留谁 | 一句话 |
|-----------|--------|--------|
| `INNER JOIN` | 只保留两边都匹配上的 | 交集 |
| `LEFT JOIN` | 左表全保留 + 右表匹配项,匹配不上填 NULL | 左表为主 |
| `RIGHT JOIN` | 右表全保留(LEFT 的镜像) | 少用,可改写成 LEFT |
| `FULL JOIN` | 两边全保留 | 并集 |

- JOIN 三件套:`FROM 主表 别名` + `JOIN 表 别名` + `ON 连接条件`。
- **核心心智模型**:JOIN 前先问「要不要保留匹配不上的行」——要 → LEFT/FULL,不要 → INNER。
- `RIGHT JOIN` 几乎不用,任何 RIGHT 都能把两表对调写成 LEFT,LEFT 更符合「从主表出发」的阅读习惯。

**LEFT JOIN + IS NULL = 反向筛选**
- `LEFT JOIN ... WHERE 右表列 IS NULL` = 找「左表有、右表没有」的孤儿行。
- 这和 Day 7 的 `NOT EXISTS` 是同一需求的两种写法,结果一致。
- 注意是 `IS NULL` 不是 `= NULL`(NULL 比较必须用 IS)。

**JOIN 后的 NULL —— 必须主动处理 ⚠️**
- LEFT/FULL JOIN 后,匹配不上的地方是 NULL。直接 `GROUP BY` 会多出一个裸 `None` 组。
- **结果里不该出现裸 NULL/None** —— 业务方看不懂。要么 INNER 排除,要么用 `COALESCE` 命名。
- `COALESCE(a, b)`:返回第一个非 NULL 的值。`COALESCE(region, 'Unknown')` = region 有值用 region,NULL 则用 'Unknown'。
- 统计「行数 / 订单数」用 `COUNT(*)`,不要用 `COUNT(某列)`(那列有 NULL 会少数)。

**JOIN 膨胀 / fan-out ⚠️(今日最重要,粒度问题的危险形态)**
- LEFT JOIN 规则:左表每一行,去右表找**所有**匹配行,有几行就复制成几行。
- 若右表连接 key 不唯一(一对多 / 多对多),左表对应行会被复制膨胀。
- 后果:对膨胀后结果做 `SUM`/`COUNT`,数值**虚高**,且查询不报任何错 —— 极隐蔽。
- **铁律:JOIN 前先确认右表的连接 key 是否唯一。**

### 卡住/印象深的题
- **题 6**(真 bug):LEFT JOIN 后 `GROUP BY region` 多出一个 `None` 组(614918,还是最大那组)。题目要求「想清楚要不要算未匹配订单」,我直接用 LEFT 且没处理 NULL。正确:INNER 排除,或 `COALESCE(region,'Unknown')`。
- **题 3**:用了 INNER JOIN,但跳过了「先查 sales 的 country 是否都在 countries 表」。实际上 sales 里有 `UK`,countries 表没有 → INNER 会悄悄丢掉 UK 订单。JOIN 前必须先验证 key 对齐。
- **题 2**:`LIMIT 20` 前 10 行恰好都能匹配上,没看到 NULL 行,漏了题目要求的「观察」。`LIMIT` 看到的不代表全部。
- **题 10**:现象抓对(C003 订单变两行),但解释用词不准 —— 不是「promo 有两个 C003 订单」,是「C003 这个 key 出现两次」;金额不是整体翻倍,是「C003 部分翻倍」。

### 概念顿悟
- 子查询是「查询套查询」,JOIN 是「表接表」,两者常可互换(题 4/5、题 8 同一需求双写法)。
- JOIN 之后第一件事:问自己「有没有未匹配行?该排除还是 COALESCE 命名?」

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
15. `HAVING` 里写聚合函数本身,不要用 SELECT 的别名(高频面试陷阱)—— ⚠️ Day 7 题 9 复发;Day 8 未犯,有进步
16. 别名可用性规则:`ORDER BY` 能用别名,`WHERE`/`GROUP BY`/`HAVING` 不能
17. `SELECT` 最后一列后面不要加逗号 —— ⚠️ Day 7 题 4 复发;Day 8 未犯,有进步
18. **边界初始化**:维护极值的问题,用 `float('-inf')` / `float('inf')` 初始化
19. **"碰巧对" ≠ "正确"**:写完代码自己造刁钻测试用例验证
20. **`NOT IN` 遇 NULL 全军覆没**:子查询列可能有 NULL 时,永远用 `NOT EXISTS`
21. **粒度意识**:分清「订单粒度」和「客户粒度」,决定 `FROM` 谁、是否要 `DISTINCT`
22. **碰新数据先 `DESCRIBE`**:列名、列类型以 `DESCRIBE` 为准,不凭记忆也不轻信别人
23. **相关子查询里不要画蛇添足加 `GROUP BY`**:`WHERE` 已锁定单组,再分组多余且可能报错
24. **`%%sql` 必须在 cell 第一行**:前面有注释/空行会被当 Python 跑,报 SyntaxError
25. **JOIN 后必查未匹配行(NULL)**:用 `COALESCE` 命名或决定排除,结果里别留裸 `None`
26. **JOIN 膨胀(fan-out)**:JOIN 前确认右表连接 key 唯一,否则 `SUM`/`COUNT` 虚高且不报错
27. **JOIN 前先验证两表 key 对齐情况**:别凭感觉选 INNER/LEFT,先查有没有匹配不上的
28. **`LIMIT N` 看到的不是全部**:题目要求观察某现象时,主动构造能看到该现象的查询

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
| 降序 | `sorted(data, key=lambda r: -r['x'])` | `ORDER BY x DESC` |
| 多关键字 | `sorted(..., key=lambda r: (r['a'], -r['b']))` | `ORDER BY a ASC, b DESC` |
| 取前 N | `sorted(...)[:5]` | `LIMIT 5` |
| 去重 | `set(...)` | `SELECT DISTINCT` |
| 模糊匹配 | `r['x'].startswith('M')` | `WHERE x LIKE 'M%'` |
| 计数 | `counts[x] = counts.get(x,0)+1` | `GROUP BY x ... COUNT(*)` |
| 分组求和 | 嵌套 dict 累加 | `GROUP BY x ... SUM(...)` |
| 二维透视 | `defaultdict(lambda: defaultdict(int))` | `GROUP BY a, b ... SUM(...)` |
| 组后过滤 | 先聚合再 `if` 筛选 | `HAVING SUM(...) > N` |
| 三元/高低判断 | `'高' if x > avg else '低'` | `CASE WHEN x > avg THEN '高' ELSE '低' END` |
| 先算中间值再用 | `avg = sum(...)/len(...)` 后再比较 | 标量子查询 `WHERE x > (SELECT AVG ...)` |
| 圈出一批 key 再捞 | `keys = {...}; [r for r in data if r['k'] in keys]` | `WHERE k IN (SELECT ...)` |
| 判断是否存在 | `any(... for ... in ...)` | `EXISTS (SELECT 1 ...)` |
| 判断不存在 | `not any(...)` | `NOT EXISTS (SELECT 1 ...)` |
| 两步聚合 | 先 groupby 出中间结果,再对结果聚合 | `FROM (SELECT ... GROUP BY ...) AS t` |
| 按 key 关联两表 | `dict` 查表 / `pd.merge` | `JOIN ... ON 左.k = 右.k` |
| 找左表有右表没有的 | `[x for x in a if x not in b]` | `LEFT JOIN ... WHERE 右.k IS NULL` 或 `NOT EXISTS` |
| 空值兜底 | `a if a is not None else b` / `d.get(k, default)` | `COALESCE(a, b)` |

---

## Week 1 总结

### 数字
- 6 天,62 道练习题,正确率约 90%
- GitHub 连续 6 天提交

### 能力变化
- 代码风格:从「C/Java 式硬循环」→「Pythonic 推导式 + 声明式 SQL」
- 工具箱:list / tuple / dict / set / 推导式 / lambda / sorted + SQL 基础查询与聚合
- 思维:建立了「Python ↔ SQL」的翻译直觉,理解两者是同一套数据处理思想的不同表达

### 暴露的问题(Week 2 要改进)
1. **"碰巧对"现象**:多次出现输出正确但逻辑有 bug —— 需养成"自己造测试用例"的习惯
2. **边界条件**:极值初始化、空输入等边界情况考虑不足
3. **读题**:SQL 的 SELECT 列偶尔抄错

### Week 2 预告
- SQL:子查询、CTE、JOIN(多表连接)
- Python:函数进阶、异常处理、NumPy 入门
- 难度会上一个台阶,但 Week 1 的地基已经打牢

---

## Day 7 当日小结

### 数字
- 10 道练习题,7 道完全正确,3 道有 bug(题 4 / 题 7 / 题 9)
- GitHub 连续提交保持

### 做得好的
- 题 6、题 10 主动写理解性注释,学习状态对
- 题 2 思考题(并列最大值)答到点上
- 题 8 "top-1 per group" 写得最干净

### 暴露的问题
1. **旧弱点复发**:#15(HAVING 用别名)、#17(末尾逗号)
2. **粒度概念新坑**:题 7 把订单表当客户表用,导致结果重复
3. **相关子查询理解还不透**:题 4 多加了无意义的 GROUP BY

---

## Day 8 当日小结

### 数字
- 10 道练习题,8 道完全正确,题 6 有 bug
- GitHub 连续提交保持

### 做得好的
- 四种 JOIN 全部用对
- 题 4/5、题 8 的 JOIN ↔ 子查询双写法干净利落
- 题 8 清楚知道「谁该当左表」
- #15、#17 旧弱点本次未犯,有进步

### 暴露的问题
1. **JOIN 后 NULL 未处理**:题 3/6/7 都碰到未匹配行,题 6 直接留了裸 `None` 组
2. **跳过验证步骤**:题 3 没先查 key 对齐就选 INNER,会丢 UK 订单
3. **LIMIT 误判**:题 2 没看到 NULL 行就以为做完了
4. **解释用词不精确**:题 10 现象对、措辞糙

### Day 9 预告
- Python 函数进阶:默认参数、`*args`/`**kwargs`、装饰器基础、类型注解
- 异常处理:try/except/finally、`with open()`、读写 CSV/JSON
- 从 SQL 切回 Python,注意手感切换
