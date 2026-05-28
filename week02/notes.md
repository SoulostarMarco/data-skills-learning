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
- 完成 10 道练习题,7 道完全正确,3 道有 bug(题 4、题 7、题 9)

### 学到的关键点

**子查询的四种位置**
| 位置 | 形态 | 用途 |
|------|------|------|
| `WHERE total > (...)` | 标量子查询(1 行 1 列) | 把聚合值算出来再比较 |
| `WHERE x IN (...)` | IN 子查询(1 列多行) | 先圈出一批 key,再捞明细 |
| `WHERE EXISTS (...)` | 相关子查询 | 内层引用外层列,逐行判断「存不存在」 |
| `FROM (...) AS t` | 派生表 | 把聚合结果当临时表做「两步聚合」;必须起别名 |

- 标量子查询当普通数字用,`WHERE` 比较时子查询里的 `AS 别名` 没意义,应删掉。
- EXISTS 只看子查询「有没有返回行」,`SELECT 1` 写什么不重要。
- 相关子查询逐行跑、慢(Week 4 窗口函数有更优写法)。FROM 派生表的重复痛点正是 Week 5 CTE 要解决的。

**NOT IN 的 NULL 陷阱 ⚠️**:`id NOT IN (101,102,NULL)` 含 NULL 时整个条件永远无法为 TRUE → 返回 0 行。**子查询列可能有 NULL 时永远用 `NOT EXISTS`。**

**环境**:`%%sql` 必须在 cell 第一行;DESCRIBE 渲染不出用 `SELECT * FROM (DESCRIBE ...)` 包一层。

### 卡住的题
- 题 4:相关子查询多写 GROUP BY 画蛇添足;末尾多逗号(#17 复发);高/低用 `CASE WHEN`。
- 题 7:`FROM sales` 致客户重复——要客户就 `DISTINCT`。**粒度问题**。
- 题 9:HAVING 用了 SELECT 别名(#15 复发)。

---

## Day 8: SQL JOIN(多表连接)

### 今日完成
- 跟练:造 customers/countries 表 → INNER/LEFT/RIGHT/FULL JOIN → 链式 JOIN
- 完成 10 道练习题,8 道完全正确,题 6 有 bug

### 学到的关键点

**四种 JOIN**
| 类型 | 保留谁 | 一句话 |
|------|--------|--------|
| INNER | 两边都匹配上的 | 交集 |
| LEFT | 左表全保留 + 右表匹配项,不匹配填 NULL | 左表为主 |
| RIGHT | 右表全保留(LEFT 镜像) | 少用 |
| FULL | 两边全保留 | 并集 |

- 核心心智模型:JOIN 前先问「要不要保留匹配不上的行」——要→LEFT/FULL,不要→INNER。
- **LEFT JOIN + WHERE 右表 IS NULL = 反向筛选**(找左表有右表没有的),等价于 `NOT EXISTS`。
- **JOIN 后 NULL 必须主动处理**:别留裸 None,用 `COALESCE(列,'Unknown')` 命名或 INNER 排除;统计行数用 `COUNT(*)`。
- **JOIN 膨胀 fan-out ⚠️**:右表连接 key 不唯一会把左表行复制膨胀,`SUM`/`COUNT` 虚高且不报错。**JOIN 前确认右表 key 唯一**。

### 卡住的题
- 题 6(真 bug):LEFT JOIN 后 `GROUP BY region` 多出 None 组,没做「要不要算未匹配订单」的决策。
- 题 3:用 INNER 但没先验证 key 对齐,会悄悄丢 UK 订单。
- 题 2:LIMIT 20 前 10 行恰好都匹配,没看到 NULL 行。

---

## Day 9: Python 函数进阶 + 异常处理 + 文件 IO

### 今日完成
- 跟练:默认参数 / `*args`·`**kwargs` / 类型注解 / 装饰器 / try-except / 文件 IO
- 完成 10 道练习题,7 道正确,题 5/7/10 有 bug

### 学到的关键点
- **默认参数大坑**:`def f(lst=[])` 默认值只创建一次、调用间共享。用 `None` 再函数内新建。
- `*args` 打包位置参数成 tuple,`**kwargs` 打包关键字参数成 dict;调用时 `f(*列表)`/`f(**字典)` 是拆包。
- 类型注解只是「说明书」,不强制、不转换类型。
- **装饰器 wrapper 两件事**:① 签名 `(*args, **kwargs)` ② 必须 `return` 原函数返回值。
- 文件永远用 `with open(...)`;Windows 务必 `encoding='utf-8'`;读用 `for line in f` 流式,别 `readlines()`。

### 卡住的题
- 题 5:漏了「文件不存在返回 []」——没包 FileNotFoundError。
- 题 7(真 bug):装饰器 wrapper 没 return、漏 **kwargs。
- 题 10(真 bug):用 `.get("country")` 想靠 `except KeyError` 抓缺字段,但 `.get()` 缺键返回 None 不抛错,致 None 混进结果;且 `continue` 后写死代码。

### 概念顿悟
- `dict["k"]` 缺键抛 KeyError;`dict.get("k")` 缺键返回 None 不抛错——「安全访问」是双刃剑。
- 手写分组求和 = SQL 的 `GROUP BY...SUM`。坏数据该「跳过 + 记日志」。

---

## Day 10: 异常处理 + 文件 IO(进阶)

### 今日完成
- 跟练:异常粒度 / try-except-else / 自定义异常 / 异常链 / logging / pathlib / csv·json 模块
- 完成题 1-8(题 9 class 超纲跳过,题 10 综合大题拆解)

### 学到的关键点
- **只抓预期的异常**,别用裸 `except:`(会吞掉所有错误包括真 bug)。
- `try/except/else/finally`:成功逻辑放 `else`,清理放 `finally`。
- **自定义异常**:`class XxxError(Exception)`,让调用方能精准 `except` 业务错误。
- **异常链**:`raise 业务异常 from e`,`e.__cause__` 保留根本原因。
- **logging 替代 print**:级别 DEBUG<INFO<WARNING<ERROR<CRITICAL;坏数据「跳过 + `logging.warning` 记录」,不默默丢。
- **pathlib**:`Path("..") / "data" / "x.csv"`,`.exists()`/`.name`/`.suffix`;跨平台,消除 Windows 斜杠问题。
- **csv/json 模块**:`csv.DictReader` 每行读成 dict(值全是字符串!);**Windows 写 CSV 必须 `newline=""`**;`json.dump(..., ensure_ascii=False, indent=2)`。

### 卡住的题
- 题 5(真 bug):`open(data, ...)` 把 dict 列表当路径传给 open;且定义后没运行(没测=不知道炸)。写前该检查的是 `data` 非空,不是文件存在。
- 题 8:文件不存在只 warning 没 `return`,继续往下会炸;`int(row[...])` 缺键抛 KeyError 但只抓了 ValueError。
- 题 4:`return lst` 缩进进了 `with` 块内(碰巧对,该挪到 with 外);log 里 `line` 带换行符,该 `.strip()`。

### 做得好的 / 进步
- **题 7 把 Day 9 题 10 的三个坑全改对了**:用 `[]` 取值真抛 KeyError、`logging.warning` 而非 print、`continue` 放 log 之后没死代码。连续两天最实在的进步。
- 题 1/2/3/6 干净;题 4 补上了 Day 9 漏的 FileNotFoundError。

### 概念顿悟
- 上下文管理器(`__enter__`/`__exit__`)是装饰器 timer 的「with 版」——但涉及 class,等学过 OOP 再碰。
- 分支里(文件不存在、坏行)要 `return` 提前退出,别让代码继续往下走到会崩的地方。

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

## Day 10 当日小结

### 数字
- 题 1-8 实际做了 8 题,6 道正确(1/2/3/4/6/7),题 5 真 bug、题 8 健壮性漏洞
- 题 9 class 超纲跳过(不计未完成),题 10 综合大题难度过高已拆解
- GitHub 连续提交保持

### 做得好的
- **题 7 把 Day 9 题 10 的三个坑全改对**:[]取值真抛 KeyError、logging 而非 print、continue 放 log 后
- 题 4 补上了 Day 9 漏的 FileNotFoundError
- 题 1/2/3/6 干净;自定义异常、异常链、pathlib、try-except-else 都掌握了

### 暴露的问题
1. **#19 又冒头**:题 5 定义函数后没运行(没测=不知道炸)、题 8 没列边界用例
2. **return 位置/缺失**:题 4 return 进了 with 块、题 8 文件不存在没 return
3. **碰巧对**:题 7/8 靠运算时碰巧报对异常类型拦住,不是主动 float() 转换防御

### 难度反馈(已采纳)
- 题 9(class/上下文管理器)超纲——学习计划里 OOP 还没排到,我硬塞不对,作废
- 题 10 综合大题过重,已拆成 10a/10b/10c 小步骤
- 经验:能直说「超纲/太难」是好事,出题难度曲线后续会更贴实际进度

### Day 11 预告
- NumPy 入门:数组创建、向量化、布尔索引、axis 参数
- 这是 Pandas 的地基,也是「用 NumPy 算股票收益率/波动率」的前置
