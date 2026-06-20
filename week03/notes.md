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

---

## Day 14: Pandas 进阶 —— 字符串方法、apply、缺失值、分箱

### 今日完成
- 跟练:`.str` 访问器(contains/startswith/replace/len/切片)、`.apply` 自定义函数、缺失值处理(dropna/fillna/ffill/bfill)、`pd.cut`/`pd.qcut` 分箱、`.where`/`.mask` 条件截断、`pivot_table` 透视表
- 完成 10 道练习题,7 道完全正确(3/4/5/7/8/9/10),3 道有 bug(1/2/6)

### 学到的关键点

**Pandas 核心原则:方法返回新对象,不修改原 df**
| 方法 | 返回新对象? | 原地修改? | 正确用法 |
|------|-------------|-----------|----------|
| `.str.replace()` | ✅ | ❌ | `df['col'] = df['col'].str.replace(...)` |
| `.fillna()` | ✅ | ❌ | `df['col'] = df['col'].fillna(...)` |
| `.sort_values()` | ✅ | ❌ | `df = df.sort_values(...)` 或取索引再赋值 |
| `.dropna()` | ✅ | ❌ | `df = df.dropna()` |
| `drop(columns=...)` | ✅ | ❌ | `df = df.drop(columns=...)` |

**唯一例外**: `df.loc[条件, 'col'] = val` 是原地修改。

**`.str` 访问器常用方法**
| 方法 | 用途 | 示例 |
|------|------|------|
| `.str.contains('x')` | 包含子串 | 模糊匹配筛选 |
| `.str.startswith('x')` | 前缀匹配 | 按 ID 前缀筛选 |
| `.str.replace('a', 'b')` | 批量替换 | 数据标准化 |
| `.str[:n]` | 切片 | 取前 N 个字符 |
| `.str.len()` | 长度 | 验证字段长度 |

**apply vs 向量化选择**
| 场景 | 推荐方案 | 原因 |
|------|----------|------|
| 简单 if-else 映射 | `np.where` | 快 10-100 倍 |
| 复杂多条件判断 | `np.where` 嵌套 | 仍然快 |
| 必须用逐行逻辑 | `df.apply(func, axis=1)` | 慢但灵活 |
| 字符串操作 | `.str.xxx` | 向量化,快 |
| 数值计算 | 直接广播运算 | 最快 |

**缺失值处理策略**
| 策略 | 方法 | 适用场景 |
|------|------|----------|
| 删除行 | `dropna()` | 缺失很少,不影响分析 |
| 填充固定值 | `fillna(0)` / `fillna('Unknown')` | 有业务默认值 |
| 填充统计值 | `fillna(df['col'].mean())` | 数值列,分布均匀 |
| 分组填充 | `groupby('group')['col'].transform('mean')` | 不同组差异大 |
| 前向填充 | `fillna(method='ffill')` | 时间序列 |
| 后向填充 | `fillna(method='bfill')` | 时间序列 |

**分箱对比**
| 方法 | 分箱依据 | 适用场景 | 每箱数量 |
|------|----------|----------|----------|
| `pd.cut` | 等宽区间 | 按业务阈值分组 | 不均匀 |
| `pd.qcut` | 等分位数 | 按分布分组 | 大致相等 |

**透视表 pivot_table**
```python
pd.pivot_table(df, values='total', index='country', columns='category', aggfunc='sum', fill_value=0)
```
- `index`: 行维度
- `columns`: 列维度
- `values`: 要聚合的列
- `aggfunc`: 聚合函数(sum/mean/count/max)
- `fill_value`: 缺失值填充

### 卡住/印象深的题
- **题 1**: `.str.replace('US', 'USA')` 没有赋值回 df['country'],只是打印了结果——**Pandas 方法返回新对象,不修改原 df**(#48)
- **题 2**: `.fillna('Unknown')` 没有赋值回 df['country']——同上(#48 复发)
- **题 6**: `df.sort_values('total')` 没有赋值,导致 `loc[0:9]` 改的是原 df 的索引 0~9,而不是 total 最高的前 10 名——**sort_values 不修改原 df,取前N用 `nlargest`**(#49)

### 概念顿悟
- **Pandas 的「不可变性」设计**:绝大多数方法返回新对象,这和 Python 字符串的不可变设计一致。要原地修改必须显式赋值或用 `loc` 索引赋值。
- **transform 的强大**: `groupby('country')['total'].transform('mean')` 返回的是和原 df 等长的 Series,每行对应其组的均值,可以直接用于 `fillna`——这是分组填充的优雅写法。
- **透视表 = 二维 groupby**:pivot_table 本质是先 groupby 再 reshape 成二维表格,是 Excel 数据透视的 Pandas 实现。
- **日期处理三板斧**: `pd.to_datetime` → `.dt.xxx` 提取 → `strftime` 格式化,是时间序列分析的基础。

### 做得好的
- **Pandas 高级操作全部掌握**:字符串方法、apply、缺失值、分箱、透视表、日期处理无一概念错误
- **向量化优先**:题4 用 `np.where` 重写了 apply,效率意识到位
- **transform 分组填充**:题8 用 `groupby + transform('mean')` 做分组填充,是 Pandas 高级技巧
- **数据管道思维**:题10 的报告生成从读取到 JSON 输出全程 Pandas 无循环,数据岗思维成熟
- **日期处理熟练**:题9 的 Q1 筛选、按月统计、周末判断全部正确

### 暴露的问题
1. **#48 Pandas 方法返回新对象,不修改原 df**:题1/2 的 `.str.replace` / `.fillna` 没有赋值。核心规则:**没有 `inplace=True` 或显式赋值,原 df 不变**。
2. **#49 `sort_values` 不修改原 df,取前N用 `nlargest`**:题6 的 `sort_values` 未赋值,`loc[0:9]` 改的是索引 0~9 而非 total 最高的前10。`nlargest(10, 'total')` 是推荐写法。
3. **FutureWarning**:题5/8 的 `groupby` 出现 `observed=False` 警告,后续加 `observed=False` 参数消除。

---

## 我的弱点清单(Week 3 持续累计)

### 从 Week 2 继承
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
42. **读题再仔细**:做完后回头扫一眼题目要求的输出格式/数值/列名
43. **量化计算跟踪 shape 和语义**
44. **验证要验证「结果」而非「输入」**
45. **测试文件要真实存在**:边界测试必须实际构造数据
46. **`isin` 统计数量用 `.sum()` 而非 `len()`**
47. **多条件布尔筛选必须加括号**:`(a > 0) & (b < 10)`,每个条件独立括起来
48. **Pandas 方法返回新对象,不修改原 df**:`str.replace`/`fillna`/`sort_values`/`dropna` 必须显式赋值 ⚠️
49. **`sort_values` 不修改原 df,取前N用 `nlargest`**:题6 的 `sort_values` 未赋值,`loc[0:9]` 改错对象 ⚠️

---

## SQL ↔ Python 翻译表(Week 3 持续扩充)

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
| 取前 N | `df.nlargest(5, 'x')` / `df.sort_values('x').head(5)` | `LIMIT 5` / `ORDER BY x DESC LIMIT 5` |
| 去重 | `df['col'].unique()` / `df.drop_duplicates()` | `SELECT DISTINCT` |
| 模糊匹配 | `df['x'].str.contains('M')` | `WHERE x LIKE '%M%'` |
| 前缀匹配 | `df['x'].str.startswith('M')` | `WHERE x LIKE 'M%'` |
| 字符串替换 | `df['x'].str.replace('a', 'b')` | `REPLACE(x, 'a', 'b')` |
| 计数 | `len(df)` / `df.shape[0]` | `COUNT(*)` |
| 分组求和 | `df.groupby('x')['y'].sum()` | `GROUP BY x ... SUM(y)` |
| 组内均值填充 | `df.groupby('g')['y'].transform('mean')` | `AVG(y) OVER (PARTITION BY g)` (窗口函数) |
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
| 分箱(等宽) | `pd.cut(df['x'], bins=[0,100,200,300])` | `CASE WHEN x<100 THEN '低' WHEN x<200 THEN '中' ELSE '高' END` |
| 分箱(等分位) | `pd.qcut(df['x'], q=4)` | `NTILE(4) OVER (ORDER BY x)` (窗口函数) |
| 透视表 | `pd.pivot_table(df, values='v', index='r', columns='c', aggfunc='sum')` | `SELECT r, c, SUM(v) FROM t GROUP BY r, c` |
| 数据管道验证 | Python NumPy/Pandas 计算 | SQL 聚合验证 | 两者交叉验证

---

## Day 14 当日小结

### 数字
- 10 道练习题,7 道完全正确(3/4/5/7/8/9/10),3 道有 bug(1/2/6)
- 正确率约 70%(严格标准) / 90%(宽松标准)
- GitHub 连续提交保持
- Week 3 累计:Day 13-14,20 道练习题

### 做得好的
- **Pandas 高级操作全部掌握**:字符串方法、apply、缺失值处理、分箱、透视表、日期处理无一概念错误
- **向量化优先**:题4 主动用 `np.where` 重写 apply,效率意识到位
- **transform 分组填充**:题8 用 `groupby + transform('mean')` 做分组填充,是 Pandas 高级技巧
- **数据管道思维**:题10 从读取到 JSON 输出全程 Pandas 无循环,数据岗思维成熟
- **日期处理熟练**:题9 的 Q1 筛选、按月统计、周末判断全部正确

### 暴露的问题
1. **#48 Pandas 方法返回新对象**:题1/2 的 `.str.replace` / `.fillna` 没有赋值。核心规则:没有 `inplace=True` 或显式赋值,原 df 不变。
2. **#49 `sort_values` 不修改原 df**:题6 的 `sort_values` 未赋值,`loc[0:9]` 改的是索引 0~9 而非 total 最高的前10。`nlargest(10, 'total')` 是推荐写法。
3. **FutureWarning**:题5/8 的 `groupby` 出现 `observed=False` 警告,后续加 `observed=False` 参数消除。

### 难度反馈
- 题8(transform 分组填充)和题10(数据报告)难度贴合,综合考察了多个知识点
- 题6 的 `sort_values` 陷阱是 Pandas 常见坑,用户踩中说明这个陷阱设计有效
- 题1/2 的「赋值」问题暴露了对 Pandas 不可变设计的理解不足

### Day 15 预告
- Pandas 核心:groupby + agg + merge + concat
- groupby 是数据分析的瑞士军刀,merge 是 SQL JOIN 的 Pandas 实现
- 这是 Pandas 最重要的三大操作之一

---

## Day 15: Pandas 核心 —— groupby、agg、merge、concat、filter

### 今日完成
- 跟练:groupby 分组聚合、agg 多聚合/自定义聚合、transform 分组变换(保留原shape)、merge 表关联(inner/left/right/outer)、concat 纵向/横向拼接、filter 按组条件筛选
- 完成 10 道练习题,6 道完全正确(1/4/5/6/7/8),4 道有小问题(2/3/9/10)

### 学到的关键点

**groupby 心智模型:拆分→应用→合并**
| 操作 | 代码 | 返回值 | 适用场景 |
|------|------|--------|----------|
| 简单聚合 | `df.groupby('g')['x'].sum()` | Series | 每组一个值 |
| 多聚合 | `df.groupby('g')['x'].agg(['sum','mean'])` | DataFrame | 多统计量 |
| 多列聚合 | `df.groupby('g').agg({'x':'sum','y':'mean'})` | DataFrame | 不同列不同统计 |
| transform | `df.groupby('g')['x'].transform('mean')` | 等长Series | 分组填充/标准化/排名 |
| filter | `df.groupby('g').filter(lambda x: ...)` | DataFrame | 保留/删除整个组 |

**transform vs agg 核心区别**
| transform | agg |
|-----------|-----|
| 返回和原 df 等长 | 每组一行(降维) |
| 用于新增列 | 用于汇总统计 |
| `groupby('g')['x'].transform('mean')` | `groupby('g')['x'].agg('mean')` |

**merge 参数对照 SQL**
| Pandas | SQL | 说明 |
|--------|-----|------|
| `how='inner'` | INNER JOIN | 交集,只保留匹配上的 |
| `how='left'` | LEFT JOIN | 保留左表全部,右表不匹配填NaN |
| `how='right'` | RIGHT JOIN | 保留右表全部 |
| `how='outer'` | FULL JOIN | 并集 |

**merge 后也查未匹配行**:和 SQL JOIN 一样,`left[right_col.isnull()]` 检查右表未匹配。

**concat 方向**
| 方向 | 参数 | 效果 | 注意 |
|------|------|------|------|
| 纵向 | `axis=0` | 行增加,列取并集 | 默认按索引对齐,用 `ignore_index=True` |
| 横向 | `axis=1` | 列增加,行取并集 | 可能产生重复列名 |

**agg 命名聚合(新写法)**
```python
df.groupby('g').agg(
    total_sum=('total', 'sum'),
    total_mean=('total', 'mean'),
    order_count=('quantity', 'count')
).reset_index()
```

### 卡住/印象深的题
- **题 2**: `agg(['sum', 'mean', 'count'])` 少了 `std`,题目要求四个统计量但只给了三个——遗漏
- **题 3**: `print(df['country_rank'].describe)` 没有加括号,打印的是方法对象而不是统计结果——**方法必须加括号调用**
- **题 9**: RFM 的 R 为负数,因为 `today = 2024-06-01` 而订单日期是 2024-01-01 到 2024-12-31,最后下单日期在 today 之后,导致 `(today - last_date).days` 为负。应该用 today 在订单日期之后,如 `2025-01-01`。另外 `RFM_Score` 用字符串拼接(`"333"`) 而不是数值相加(`9`),排序语义不同。
- **题 10**: 验证部分只打印了数字没有加描述文字,可读性差

### 概念顿悟
- **groupby 后想「降维」→ agg,想「保留原shape」→ transform**:这是 groupby 最核心的选择逻辑
- **transform 返回等长 Series 是分组填充的关键**: `groupby('country')['total'].transform('mean')` 返回每行对应国家的均值,可以直接用于 `fillna`
- **agg 命名聚合是 Pandas 推荐写法**:比 `.agg({'total': ['sum', 'mean']})` 更清晰,列名更可控
- **merge 后必查 NaN**:和 SQL JOIN 后的 COALESCE 检查等价,这是数据岗的验证习惯
- **filter 保留的是「整个组」,不是「满足条件的行」**:filter 的结果是原 df 的子集,行数可能不变或变少,但列数和原 df 相同
- **concat 横向拼接可能产生重复列**:如 `order_id` 在两个子集中都有,会保留两列

### 做得好的
- **merge + groupby + agg 综合掌握**:题8 完整实现了 merge → 命名聚合 → pivot_table → rank 找最高 → 输出 CSV,数据岗工作流完整
- **双重分组熟练**:题7 的 `groupby(['country', 'category'])` 和 `.loc` 定位正确
- **transform 分组排名**:题3 的 `transform('rank', ascending=False, method='first')` 用法正确
- **filter 按组筛选**:题6 三个 filter 条件都正确理解了「保留整个组」的语义
- **RFM 分析框架**:题9 虽然 R 为负,但 RFM 的整体框架(qcut 分箱 + 得分 + 排序)理解正确

### 暴露的问题
1. **遗漏统计量**:题2 的 `agg` 少了 `std`,做完后没回头检查题目要求
2. **方法调用**:题3 的 `.describe` 没加括号,打印的是方法对象 `<bound method>` 而不是统计结果。这是 Python 基础问题,不是 Pandas 问题
3. **日期逻辑**:题9 的 R 计算为负数,因为 today 选在了订单日期之前。做日期分析时要先确认数据的时间范围
4. **RFM_Score 语义**:题9 用字符串拼接 `"333"` 而不是数值相加 `9`。虽然字符串排序结果碰巧对,但后续计算(如求平均分)会出问题。应该用数值相加

---

## 我的弱点清单(Week 3 持续累计)

### 从 Week 2 继承
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
42. **读题再仔细**:做完后回头扫一眼题目要求的输出格式/数值/列名
43. **量化计算跟踪 shape 和语义**
44. **验证要验证「结果」而非「输入」**
45. **测试文件要真实存在**:边界测试必须实际构造数据
46. **`isin` 统计数量用 `.sum()` 而非 `len()`**
47. **多条件布尔筛选必须加括号**:`(a > 0) & (b < 10)`,每个条件独立括起来
48. **Pandas 方法返回新对象,不修改原 df**:`str.replace`/`fillna`/`sort_values`/`dropna` 必须显式赋值
49. **`sort_values` 不修改原 df,取前N用 `nlargest`**:题6 的 `sort_values` 未赋值,`loc[0:9]` 改错对象
50. **方法调用必须加括号**:`.describe()` 是调用方法,`.describe` 是引用方法对象 ⚠️
51. **日期分析先确认数据时间范围**:today 选在订单日期之后,否则计算天数为负 ⚠️
52. **数值计算用数值类型**:RFM_Score 用数值相加(9) 而不是字符串拼接("333") ⚠️

---

## SQL ↔ Python 翻译表(Week 3 持续扩充)

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
| 取前 N | `df.nlargest(5, 'x')` / `df.sort_values('x').head(5)` | `LIMIT 5` / `ORDER BY x DESC LIMIT 5` |
| 去重 | `df['col'].unique()` / `df.drop_duplicates()` | `SELECT DISTINCT` |
| 模糊匹配 | `df['x'].str.contains('M')` | `WHERE x LIKE '%M%'` |
| 前缀匹配 | `df['x'].str.startswith('M')` | `WHERE x LIKE 'M%'` |
| 字符串替换 | `df['x'].str.replace('a', 'b')` | `REPLACE(x, 'a', 'b')` |
| 计数 | `len(df)` / `df.shape[0]` | `COUNT(*)` |
| 分组求和 | `df.groupby('x')['y'].sum()` | `GROUP BY x ... SUM(y)` |
| 组内均值填充 | `df.groupby('g')['y'].transform('mean')` | `AVG(y) OVER (PARTITION BY g)` (窗口函数) |
| 组内排名 | `df.groupby('g')['y'].transform('rank')` | `RANK() OVER (PARTITION BY g ORDER BY y)` |
| 组后过滤 | `df.groupby('x').filter(lambda g: g['y'].sum() > N)` | `HAVING SUM(y) > N` |
| 三元/高低判断 | `np.where(df['x'] > avg, '高', '低')` | `CASE WHEN x > avg THEN '高' ELSE '低' END` |
| 先算中间值再用 | `avg = df['x'].mean()` 后筛选 | 标量子查询 `WHERE x > (SELECT AVG(x) FROM t)` |
| 圈出一批 key 再捞 | `keys = [...]; df[df['k'].isin(keys)]` | `WHERE k IN (SELECT ...)` |
| 判断是否存在 | `df['x'].any()` | `EXISTS (SELECT 1 ...)` |
| 判断不存在 | `~(df['x'].any())` | `NOT EXISTS (SELECT 1 ...)` |
| 两步聚合 | `df.groupby('a').agg({'b':'sum'}).reset_index()` | `FROM (SELECT ... GROUP BY ...) AS t` |
| 按 key 关联两表 | `pd.merge(df1, df2, on='k', how='left')` | `LEFT JOIN ... ON 左.k = 右.k` |
| 找左表有右表没有的 | `df1[~df1['k'].isin(df2['k'])]` | `LEFT JOIN ... WHERE 右.k IS NULL` / `NOT EXISTS` |
| 空值兜底 | `df['a'].fillna('Unknown')` | `COALESCE(a, 'Unknown')` |
| 日期提取 | `df['date'].dt.strftime('%Y-%m')` | `STRFTIME(date, '%Y-%m')` |
| 类型转换 | `pd.to_numeric(col, errors='coerce')` | `CAST(col AS FLOAT)` |
| 缺失值填充 | `df['col'].fillna(0)` | `COALESCE(col, 0)` |
| 累计和 | `df['x'].cumsum()` | `SUM(x) OVER (ORDER BY ...)` |
| 占比 | `(df['x'] > 0).mean()` | `COUNT(CASE WHEN x>0 THEN 1 END) * 1.0 / COUNT(*)` |
| Z-score 标准化 | `(df['x'] - mean) / std` | `(x - AVG(x)) / STDDEV(x)` |
| 分箱(等宽) | `pd.cut(df['x'], bins=[0,100,200,300])` | `CASE WHEN x<100 THEN '低' WHEN x<200 THEN '中' ELSE '高' END` |
| 分箱(等分位) | `pd.qcut(df['x'], q=4)` | `NTILE(4) OVER (ORDER BY x)` (窗口函数) |
| 透视表 | `pd.pivot_table(df, values='v', index='r', columns='c', aggfunc='sum')` | `SELECT r, c, SUM(v) FROM t GROUP BY r, c` |
| 数据管道验证 | Python NumPy/Pandas 计算 | SQL 聚合验证 | 两者交叉验证
| 拼接(纵向) | `pd.concat([df1, df2], axis=0)` | `UNION ALL` |
| 拼接(横向) | `pd.concat([df1, df2], axis=1)` | `SELECT a.*, b.* FROM a JOIN b` |
| 分组排名 | `df.groupby('g')['x'].rank(ascending=False)` | `ROW_NUMBER() OVER (PARTITION BY g ORDER BY x DESC)` (窗口函数) |

---

## Day 15 当日小结

### 数字
- 10 道练习题,6 道完全正确(1/4/5/6/7/8),4 道有小问题(2/3/9/10)
- 正确率约 60%(严格标准) / 80%(宽松标准)
- GitHub 连续提交保持
- Week 3 累计:Day 13-15,30 道练习题

### 做得好的
- **merge + groupby + agg 综合掌握**:题8 完整实现了 merge → 命名聚合 → pivot_table → rank 找最高 → 输出 CSV,数据岗工作流完整
- **双重分组熟练**:题7 的 `groupby(['country', 'category'])` 和 `.loc` 定位正确
- **transform 分组排名**:题3 的 `transform('rank', ascending=False, method='first')` 用法正确
- **filter 按组筛选**:题6 三个 filter 条件都正确理解了「保留整个组」的语义
- **agg 命名聚合**:题8 使用了 `agg(total_sum=('total', 'sum'))` 新写法,代码清晰

### 暴露的问题
1. **#50 方法调用必须加括号**:题3 的 `.describe` 没加括号,打印的是 `<bound method>` 而不是统计结果。Python 方法必须加括号调用才能执行。
2. **#51 日期分析先确认时间范围**:题9 的 `today = 2024-06-01` 在订单日期之前,导致 R 为负数。做日期分析时要先看数据的时间范围。
3. **#52 数值计算用数值类型**:题9 的 RFM_Score 用字符串拼接 `"333"` 而不是数值相加 `9`。字符串排序和数值排序语义不同,后续计算也会出问题。
4. **遗漏统计量**:题2 的 `agg` 少了 `std`,做完后没回头检查题目要求。

### 难度反馈
- 题8(merge+agg+pivot_table+rank)和题10(综合管道)难度贴合,综合考察了多个知识点
- 题9(RFM)是经典客户分析模型,但日期范围问题暴露了对数据时间分布的理解不足
- 题3 的 `.describe` 问题说明 Python 基础(方法调用)和 Pandas 操作都需要注意

### Day 16 预告
- Week 3 综合复习 + 阶段自测
- 混合练习:Python(Pandas 所有操作) + SQL 交叉出题
- 检验 Week 3 三天学习成果,为 Week 4 做准备

---

### Git Push
```bash
cd "C:\Users\69261\Desktop\data-skills-learning"
git add week03/day15_practice.ipynb week03/day15_exercises.ipynb week03/notes.md
git commit -m "feat(week03): add Day 15 Pandas core groupby/merge/concat

- Add day15_practice.ipynb: groupby/agg/transform/merge/concat/filter
- Add day15_exercises.ipynb: 10 problems, 6/10 correct
- Fix weaknesses #50 method call parens, #51 date range check, #52 numeric score
- Update notes.md with Day 15 learnings and expanded SQL<<->Python table"
git push origin main
```

---

### Git Push
```bash
cd "C:\Users\69261\Desktop\data-skills-learning"
git add week03/day14_practice.ipynb week03/day14_exercises.ipynb week03/notes.md
git commit -m "feat(week03): add Day 14 Pandas advanced

- Add day14_practice.ipynb: str/apply/fillna/cut/qcut/pivot_table
- Add day14_exercises.ipynb: 10 problems, 7/10 correct
- Fix weaknesses #48 Pandas method returns new object, #49 sort_values nlargest
- Update notes.md with Day 14 learnings and expanded SQL<<->Python table"
git push origin main
```
