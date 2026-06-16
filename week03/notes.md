# Week 3 学习笔记

> **主题**:Pandas 基础(DataFrame/Series/索引/筛选) + SQL 字符串/日期函数
> **完成情况**:Day 13-18

---

## Day 13: Pandas 基础 —— DataFrame、索引、筛选、读写

### 今日完成
- 跟练:DataFrame 创建(dict/list of dicts/NumPy)、基本属性(shape/dtypes/head/tail/info/describe)、列访问(Series)、loc/iloc、布尔筛选、赋值修改、排序去重、类型转换、日期处理、读写 CSV
- 完成 10 道练习题,7 道完全正确,3 道有 bug(题3/9/10)

### 学到的关键点

**Pandas 核心心智模型**
| 概念 | 一句话 |
|------|--------|
| DataFrame | 带索引的二维表格 = NumPy 数组 + 行索引 + 列名 |
| Series | 带索引的一维数组 = DataFrame 的每一列 |
| loc | 标签索引,含头含尾;iloc 位置索引,左闭右开 |
| 布尔筛选 | `df[df['col'] > 100]`,多条件用 `&` + 括号 |
| isin | `df['col'].isin([...])` 多值匹配,比 `|` 优雅 |
| 赋值 | `df.loc[条件, 'col'] = val`,不用链式赋值 |
| 新增列 | `df['new'] = df['a'] / df['b']` 向量化广播 |
| 排序 | `df.sort_values('col', ascending=False)` |
| 类型转换 | `pd.to_numeric(..., errors='coerce')` 失败变 NaN |
| 日期 | `pd.to_datetime()` → `.dt.year/.dt.month/.dt.strftime()` |

**loc vs iloc 对比**
| 方式 | 索引依据 | 切片规则 | 示例 |
|------|----------|----------|------|
| loc | 标签名 | 含头含尾 | `df.loc[0:5, ['a','b']]` 取 6 行 |
| iloc | 整数位置 | 左闭右开 | `df.iloc[0:5, 0:2]` 取 5 行 |
| at | 单个值标签 | — | `df.at[0, 'a']` 比 loc 更快 |
| iat | 单个值位置 | — | `df.iat[0, 0]` 比 iloc 更快 |

**isin 统计数量的正确写法**
| 错误 | 正确 |
|------|------|
| `len(df['col'].isin([...]))` | `df['col'].isin([...]).sum()` |
| 返回 Series 长度(全部行数) | 返回 True 的数量(匹配行数) |

**Pandas vs Python csv 模块**
- 读 CSV 分析用 Pandas(自动分类型、方便筛选聚合)
- 流式处理/逐行清洗用 Python csv 模块(内存可控)
- 写 JSON 用 Python json 模块(控制粒度更细)

### 卡住/印象深的题
- **题 3**: `len(sub_df['country'].isin([...]))` 返回的是 Series 长度(189),不是 True 的数量——混淆了「数组长度」和「满足条件的数量」(#46)
- **题 9**: 把 `category == 'Computer'` 写成 `product == 'Computer'`——#42 读题失误复发;`df['total'] > 2000` 在 `&` 表达式中没加括号
- **题 10**: `to_csv(index=False)` 但题目要求 `index=True`——#42 读题失误复发

### 概念顿悟
- **DataFrame 的每一列是 Series,Series 的 `.values` 是 NumPy 数组**——Pandas 是 NumPy 的包装层,理解 NumPy 就理解了 Pandas 的 80%
- **向量化广播在 Pandas 中同样适用**:`df['new'] = df['a'] / df['b']` 不用循环,和 NumPy 一样
- **日期处理是数据岗高频**:`pd.to_datetime` + `.dt.strftime` + `.dt.year` 组合使用
- **groupby 提前掌握**:题8 用 groupby 替代循环,效率更高、代码更简洁

### 做得好的
- **Pandas 核心操作全部掌握**:loc/iloc、布尔筛选、赋值、排序、类型转换、日期处理无一概念错误
- **向量化思维**:题4/10 新增列全部向量化,没有写 for 循环
- **groupby 提前掌握**:题8 用了 groupby 而不是题目给的循环写法,更 Pandas 风格
- **类型转换安全意识**:题7/10 都用 `errors='coerce'` 处理转换失败,不崩

### 暴露的问题
1. **#42 读题失误复发**:题9 把 `category` 看成 `product`;题10 把 `index=True` 看成 `index=False`。Day 12 没复发,Day 13 又冒头。需要建立「输出参数回头看」的习惯。
2. **#46 `isin` 统计数量**:混淆了 `len(series)` 和 `series.sum()`。`isin` 返回布尔面具,`.sum()` 才是计数。
3. **#47 多条件括号**:题9 的 `df['total'] > 2000` 在 `&` 链中没加括号。虽然 `>` 优先级高于 `&` 碰巧对了,但应该养成「多条件必加括号」的习惯。

---

## 我的弱点清单(Week 3 开始)

### 从 Week 2 继承(持续注意)
4. 查重该用 set(O(1) 查找),计数该用 dict
5. 经典面试题应先想 O(n) 解法,而不是上来就双重循环
6. 永远不要假设输入数据是有序的 —— 需要排序就显式调 `sorted()`
7. 用 `min()` / `max()` 替代 if/else 取小取大
8. 变量名不要和外层变量重名
9. 优先用 Python 内置函数:`abs()`、`max()`、`zip()` 等
10. Python 排序是稳定的 —— 多次 sorted 可实现复杂多关键字排序
11. 可读性 > 优雅 —— 不要为了一行而损害代码可读性
12. 写 SQL 时再看一眼题目要求的列是哪几个
13. SQL 返回 0 行的排查思路:先看数据范围,再怀疑查询
14. `LIKE` 大小写敏感,不确定大小写用 `ILIKE` 或 `LOWER()`
15. `HAVING` 里写聚合函数本身,不要用 SELECT 的别名
16. 别名可用性规则:`ORDER BY` 能用别名,`WHERE`/`GROUP BY`/`HAVING` 不能
17. `SELECT` 最后一列后面不要加逗号
18. **边界初始化**:维护极值用 `float('-inf')` / `float('inf')`
19. **"碰巧对" ≠ "正确"**:写完代码自己造刁钻测试用例验证
20. **`NOT IN` 遇 NULL 全军覆没**:子查询列可能有 NULL 时,永远用 `NOT EXISTS`
21. **粒度意识**:分清「订单粒度」和「客户粒度」
22. **碰新数据先 `DESCRIBE`**:列名、列类型以 DESCRIBE 为准
23. **相关子查询里不要画蛇添足加 `GROUP BY`**
24. **`%%sql` 必须在 cell 第一行**
25. **JOIN 后必查未匹配行(NULL)**:用 `COALESCE` 命名或排除
26. **JOIN 膨胀(fan-out)**:JOIN 前确认右表连接 key 唯一
27. **JOIN 前先验证两表 key 对齐情况**
28. **`LIMIT N` 看到的不是全部**
29. **装饰器 wrapper 两件事**:① 签名 `(*args, **kwargs)` ② 必须 `return` 原函数返回值
30. **`dict["k"]` vs `dict.get("k")`**:前者缺键抛 KeyError,后者缺键返回 None
31. **死代码**:`continue`/`return`/`break`/`raise` 之后的同层代码永不执行
32. **文件流式读取**:用 `for line in f`,不要 `readlines()`
33. **默认参数禁用可变对象**:不要 `def f(lst=[])`,用 `None` 再函数内新建
34. **写文件 vs 读文件**:`open()` 传的是路径不是数据
35. **异常分支要 `return` 提前退出**
36. **类型转换要显式**:`float(record["total"])` 主动转
37. **写完必须运行**:定义了函数不调用 = 没测 = 不知道对不对
38. **NumPy 向量化替代循环**:看到数组操作先想「能不能向量化」
39. **axis=0 压行(列汇总),axis=1 压列(行汇总)**
40. **布尔数组 `.mean()` 算占比**:`(arr > 0).mean()` 直接得比例
41. **广播用 `keepdims=True` 保持维度**
42. **读题再仔细**:做完后回头扫一眼题目要求的输出格式/数值/列名 ⚠️ Day 13 复发
43. **量化计算跟踪 shape 和语义**
44. **验证要验证「结果」而非「输入」**
45. **测试文件要真实存在**:边界测试必须实际构造数据
46. **`isin` 统计数量用 `.sum()` 而非 `len()`**:`len(series.isin([...]))` 返回 Series 长度,不是匹配数量 ⚠️
47. **多条件布尔筛选必须加括号**:`(a > 0) & (b < 10)`,每个条件独立括起来 ⚠️

---

## SQL ↔ Python 翻译表(Week 3 开始)

| 操作 | Python | SQL |
|------|--------|-----|
| 取所有 | `df` / `df.loc[:, :]` | `SELECT * FROM t` |
| 过滤 | `df[df['x'] > 100]` | `WHERE x > 100` |
| 选列 | `df['name']` / `df[['a','b']]` | `SELECT name FROM t` / `SELECT a, b` |
| 计算列 | `df['new'] = df['x'] * 1.13` | `SELECT x * 1.13 AS new FROM t` |
| 多条件 | `df[(a > 0) & (b.isin([...]))]` | `WHERE a > 0 AND b IN (...)` |
| 排序 | `df.sort_values('x')` | `ORDER BY x` |
| 降序 | `df.sort_values('x', ascending=False)` | `ORDER BY x DESC` |
| 多关键字 | `df.sort_values(['a','b'], ascending=[True, False])` | `ORDER BY a ASC, b DESC` |
| 取前 N | `df.sort_values('x').head(5)` | `LIMIT 5` |
| 去重 | `df['col'].unique()` / `df.drop_duplicates()` | `SELECT DISTINCT` |
| 模糊匹配 | `df['x'].str.startswith('M')` | `WHERE x LIKE 'M%'` |
| 计数 | `len(df)` / `df.shape[0]` | `COUNT(*)` |
| 分组求和 | `df.groupby('x')['y'].sum()` | `GROUP BY x ... SUM(y)` |
| 组后过滤 | `df.groupby('x').filter(lambda g: g['y'].sum() > N)` | `HAVING SUM(y) > N` |
| 三元/高低判断 | `np.where(df['x'] > avg, '高', '低')` | `CASE WHEN x > avg THEN '高' ELSE '低' END` |
| 先算中间值再用 | `avg = df['x'].mean()` 后筛选 | 标量子查询 `WHERE x > (SELECT AVG(x) FROM t)` |
| 圈出一批 key 再捞 | `keys = [...]; df[df['k'].isin(keys)]` | `WHERE k IN (SELECT ...)` |
| 判断是否存在 | `df['x'].any()` | `EXISTS (SELECT 1 ...)` |
| 判断不存在 | `~(df['x'].any())` | `NOT EXISTS (SELECT 1 ...)` |
| 两步聚合 | `df.groupby('a').agg({'b':'sum'}).reset_index()` | `FROM (SELECT ... GROUP BY ...) AS t` |
| 按 key 关联两表 | `pd.merge(df1, df2, on='k')` | `JOIN ... ON 左.k = 右.k` |
| 找左表有右表没有的 | `df1[~df1['k'].isin(df2['k'])]` | `LEFT JOIN ... WHERE 右.k IS NULL` / `NOT EXISTS` |
| 空值兜底 | `df['a'].fillna('Unknown')` | `COALESCE(a, 'Unknown')` |
| 日期提取 | `df['date'].dt.strftime('%Y-%m')` | `STRFTIME(date, '%Y-%m')` |
| 类型转换 | `pd.to_numeric(col, errors='coerce')` | `CAST(col AS FLOAT)` |
| 缺失值填充 | `df['col'].fillna(0)` | `COALESCE(col, 0)` |
| 累计和 | `df['x'].cumsum()` | `SUM(x) OVER (ORDER BY ...)` |
| 占比 | `(df['x'] > 0).mean()` | `COUNT(CASE WHEN x>0 THEN 1 END) * 1.0 / COUNT(*)` |
| Z-score 标准化 | `(df['x'] - mean) / std` | `(x - AVG(x)) / STDDEV(x)` |
| 数据管道验证 | Python NumPy/Pandas 计算 | SQL 聚合验证 | 两者交叉验证

---

## Day 13 当日小结

### 数字
- 10 道练习题,7 道完全正确(1/2/4/5/6/7/8),3 道有 bug(3/9/10)
- 正确率约 70%(严格标准) / 90%(宽松标准,小问题不影响主体)
- GitHub 连续提交保持
- Week 3 开始:Day 13,10 道练习题

### 做得好的
- **Pandas 核心操作全部掌握**:loc/iloc、布尔筛选、赋值、排序、类型转换、日期处理无一概念错误
- **向量化思维**:题4/10 新增列全部向量化,没有写 for 循环
- **groupby 提前掌握**:题8 用了 groupby 替代循环,更 Pandas 风格
- **类型转换安全意识**:题7/10 都用 `errors='coerce'` 处理转换失败

### 暴露的问题
1. **#42 读题失误复发**:题9 把 `category` 看成 `product`;题10 把 `index=True` 看成 `index=False`。需要建立「输出参数回头看」的习惯。
2. **#46 `isin` 统计数量**:题3 混淆了 `len(series)` 和 `series.sum()`。`isin` 返回布尔面具,`.sum()` 才是计数。
3. **#47 多条件括号**:题9 的 `df['total'] > 2000` 在 `&` 链中没加括号。优先级碰巧对了,但应该养成「多条件必加括号」的习惯。

### 难度反馈
- 题8(groupby)难度贴合,用户已会 groupby 比预期更好
- 题10 综合管道难度适中,但 `index=True/False` 容易混淆
- 题9 多条件筛选需要更明确的 `category` vs `product` 区分提示

### Day 14 预告
- Pandas 筛选与索引进阶:多条件/apply/缺失值处理/字符串方法
- 深入 `.loc` 的高级用法、`.str` 访问器、`.fillna()` 缺失值策略
- 为 Day 15 的 groupby/agg/merge 做准备
