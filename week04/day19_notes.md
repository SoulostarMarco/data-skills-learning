# Day 19 学习笔记：统计基础

## 一、今日学习范围
- 描述统计（均值、中位数、标准差、方差、分位数、极差、IQR）
- 分布形状判断（偏度、峰度）
- 正态分布与 68-95-99.7 法则
- 中心极限定理（CLT）
- 标准误（Standard Error）与样本量关系
- 置信区间（Confidence Interval）
- 异常值检测（3σ 法则 vs IQR 法则）

## 二、核心知识点

### 2.1 描述统计
- **均值**：受异常值影响大；**中位数**：更稳健
- **标准差** = 方差的平方根，衡量数据离散程度
- **IQR** = Q3 - Q1，不受极端值影响
- **偏度（Skewness）**：
  - > 0：右偏（长尾在右），均值 > 中位数
  - < 0：左偏（长尾在左），均值 < 中位数
  - ≈ 0：对称分布
- **峰度（Kurtosis）**：
  - Pandas `.kurt()` 返回 Fisher excess kurtosis（以正态为 0）
  - > 0：比正态更尖峰（leptokurtic）
  - < 0：比正态更平峰（platykurtic）

### 2.2 正态分布 68-95-99.7 法则
- 68.27% 的数据落在 μ ± σ
- 95.45% 的数据落在 μ ± 2σ
- 99.73% 的数据落在 μ ± 3σ

### 2.3 中心极限定理（CLT）
- **核心**：无论总体分布如何，样本均值的分布随着样本量 n 增大趋近正态
- n=5：样本均值分布仍接近原分布形状
- n=30：样本均值分布接近正态（钟形）
- 样本均值分布的标准差 = 总体标准差 / √n（标准误）

### 2.4 标准误（SE）
- SE = σ / √n（理论）
- SE = s / √n（实际，用样本标准差估计）
- 样本量越大，SE 越小，估计越精确

### 2.5 置信区间
- 95% CI = x̄ ± z * SE（z=1.96）
- 90% CI = x̄ ± 1.645 * SE（更窄，覆盖率更低）
- 99% CI = x̄ ± 2.576 * SE（更宽，覆盖率更高）
- 覆盖率验证：多次重复抽样，包含总体均值的比例应接近置信水平

### 2.6 异常值检测
- **3σ 法则**：基于正态假设，mean ± 3*std
  - 优点：简单直观
  - 缺点：对偏态分布不敏感，可能漏检或误检
- **IQR 法则**：[Q1 - 1.5*IQR, Q3 + 1.5*IQR]
  - 优点：不依赖分布假设，对偏态数据更稳健
  - 缺点：可能过于保守
- **右偏数据更适合 IQR 法则**

## 三、错误与反思

### 今日错误
- 无代码错误（10/10 正确）
- 3 道题缺少文字分析（题5、8、9）

### 反思
1. 题5：CLT 的文字分析需要明确指出 n=5 和 n=30 的分布形状差异
2. 题8：右偏数据应优先使用 IQR 法则，而非 3σ 法则
3. 题9：分组统计后需要结合偏度和箱线图进行文字解读

## 四、代码片段速查

```python
# 描述统计
mean = df['col'].mean()
median = df['col'].median()
std = df['col'].std()          # 样本标准差 (ddof=1)
std_pop = df['col'].std(ddof=0) # 总体标准差
var = df['col'].var()
q25 = df['col'].quantile(0.25)
q75 = df['col'].quantile(0.75)
iqr = q75 - q25
skew = df['col'].skew()        # 偏度
kurt = df['col'].kurt()        # 峰度 (Fisher excess)

# 可视化
sns.histplot(data, kde=True, bins=30)
plt.axvline(mean, color='red', linestyle='--', label='Mean')
plt.axvline(median, color='green', linestyle='--', label='Median')
sns.boxplot(y=data)

# 中心极限定理
sample_means = [np.random.choice(pop, size=n).mean() for _ in range(1000)]

# 标准误
se = std / np.sqrt(n)

# 置信区间
from scipy import stats
ci = stats.norm.interval(confidence=0.95, loc=mean, scale=se)

# 异常值检测（3σ）
outliers = data[(data < mean - 3*std) | (data > mean + 3*std)]

# 异常值检测（IQR）
q1, q3 = data.quantile([0.25, 0.75])
iqr = q3 - q1
outliers = data[(data < q1 - 1.5*iqr) | (data > q3 + 1.5*iqr)]

# 分组统计
group_stats = df.groupby('cat')['val'].agg(['mean', 'median', 'std'])
```

## 五、下一步
- Day 20：假设检验（t检验、卡方检验）
