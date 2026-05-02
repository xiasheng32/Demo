# 带误差棒折线图数据提取示例

## 示例1：棉花光合速率（多年份×多部位）

### 输入图表
- 2个年份（2018、2019）
- 2个品种（Dexiamian 1、Yuzaomian 9110）
- 3个部位（Leaf、Bract、Capsule wall）
- 5个时间点（10, 17, 24, 31, 38 d）
- 3个处理（SRWC 75%、60%、45%）

### 分析过程

**Y轴**：Net photosynthetic rate (μmol CO₂ m⁻² s⁻¹)
**X轴**：Days post anthesis (d)
**图例**：
- SRWC(75±5)% （蓝色）= 对照
- SRWC(60±5)% （黄色）= 中度干旱
- SRWC(45±5)% （橙色）= 重度干旱

**对照匹配规则**：60%和45%匹配同品种同部位同时间的75%

### 输出结果

**子图1：2018年 Leaf**

| 品种 | 处理 | 时间 | 处理均值 | 处理SD | 对照均值 | 对照SD |
|------|------|------|----------|--------|----------|--------|
| Dexiamian 1 | SRWC(60±5)% | 10d | 28.567 | 1.234 | 32.456 | 1.123 |
| Dexiamian 1 | SRWC(45±5)% | 10d | 25.891 | 1.456 | 32.456 | 1.123 |
| Dexiamian 1 | SRWC(60±5)% | 17d | 26.234 | 1.345 | 30.123 | 1.234 |
| Dexiamian 1 | SRWC(45±5)% | 17d | 22.567 | 1.567 | 30.123 | 1.234 |
| ... | ... | ... | ... | ... | ... | ... |

**行数验证**：1子图 × 2品种 × 5时间点 × 2处理 = 20行 ✓

---

### 完整数据（制表符分隔）

```
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(60±5)%	10d	28.567	1.234	32.456	1.123
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(45±5)%	10d	25.891	1.456	32.456	1.123
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(60±5)%	17d	26.234	1.345	30.123	1.234
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(45±5)%	17d	22.567	1.567	30.123	1.234
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(60±5)%	24d	23.456	1.456	27.891	1.345
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(45±5)%	24d	18.234	1.678	27.891	1.345
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(60±5)%	31d	21.123	1.567	25.456	1.456
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(45±5)%	31d	16.891	1.789	25.456	1.456
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(60±5)%	38d	16.567	1.234	20.234	1.123
Net Photosynthetic Rate	2018-Dexiamian 1	Leaf	SRWC(45±5)%	38d	12.891	1.345	20.234	1.123
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(60±5)%	10d	30.234	1.345	34.123	1.234
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(45±5)%	10d	26.567	1.567	34.123	1.234
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(60±5)%	17d	28.456	1.456	32.234	1.345
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(45±5)%	17d	24.123	1.678	32.234	1.345
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(60±5)%	24d	25.678	1.567	29.456	1.456
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(45±5)%	24d	20.345	1.789	29.456	1.456
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(60±5)%	31d	22.891	1.678	27.123	1.567
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(45±5)%	31d	17.567	1.891	27.123	1.567
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(60±5)%	38d	18.234	1.345	22.567	1.234
Net Photosynthetic Rate	2018-Yuzaomian 9110	Leaf	SRWC(45±5)%	38d	14.123	1.456	22.567	1.234
```

**行数验证**：2品种 × 5时间点 × 2处理 = 20行 ✓

---

## 常见错误

1. ❌ 将标记符号顶部当作中心
2. ❌ 读取下误差棒而非上误差棒
3. ❌ 多子图使用相同的Y轴标定
4. ❌ 对照匹配错误（如跨年份匹配）
5. ❌ 将显著性标注（*、**）误认为数据

## 正确做法

1. ✅ 读取标记符号的几何中心
2. ✅ 读取上误差棒的顶部
3. ✅ 每个子图独立标定Y轴
4. ✅ 仔细分析图例，确定正确的对照匹配关系
5. ✅ 忽略显著性标注，只提取数据
