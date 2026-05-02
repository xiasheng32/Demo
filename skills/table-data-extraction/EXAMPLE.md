# 表格数据提取示例

## 示例1：甘蔗抗氧化酶数据

### 输入表格
表2 干旱胁迫下甘蔗叶片、根系活性氧相关物质及渗透调节物质

**表格结构**：
- 行：组织（叶片、根系）× 品种（YT93-159、YZ05-51）× 处理（对照、干旱）
- 列：10个指标（SOD、MDA、POD、CAT、APX、O₂⁻、H₂O₂、Pro、SP、SS）

**数据格式**：均值±SD+显著性字母

### 分析过程

**对照匹配规则**：每个品种的Control作为Drought的对照

**数据解析**：
```
995.36±5.06c → 均值 = 995.36, SD = 5.06（忽略字母c）
```

### 输出结果

```
SOD	Leaf	YT93-159	Drought	NA	706.79	13.47	995.36	5.06
SOD	Leaf	YZ05-51	Drought	NA	1121.34	2.92	1083.63	11.36
SOD	Root	YT93-159	Drought	NA	1045.19	11.97	1140.42	5.71
SOD	Root	YZ05-51	Drought	NA	1091.47	4.62	912.98	9.50
MDA	Leaf	YT93-159	Drought	NA	13.85	0.26	19.23	0.18
MDA	Leaf	YZ05-51	Drought	NA	18.23	0.31	17.10	0.21
MDA	Root	YT93-159	Drought	NA	4.25	0.09	5.37	0.14
MDA	Root	YZ05-51	Drought	NA	3.30	0.03	4.83	0.13
POD	Leaf	YT93-159	Drought	NA	174.22	0.89	209.74	6.86
POD	Leaf	YZ05-51	Drought	NA	294.40	3.82	274.96	4.69
POD	Root	YT93-159	Drought	NA	204.10	16.39	133.33	4.48
POD	Root	YZ05-51	Drought	NA	337.98	20.67	257.60	0.35
CAT	Leaf	YT93-159	Drought	NA	42.07	2.29	27.68	1.39
CAT	Leaf	YZ05-51	Drought	NA	27.23	1.91	36.84	1.90
CAT	Root	YT93-159	Drought	NA	24.92	1.90	29.42	1.89
CAT	Root	YZ05-51	Drought	NA	80.69	2.79	36.88	2.09
APX	Leaf	YT93-159	Drought	NA	4.04	0.10	4.46	0.13
APX	Leaf	YZ05-51	Drought	NA	4.20	0.14	3.73	0.01
APX	Root	YT93-159	Drought	NA	0.56	0.01	0.79	0.03
APX	Root	YZ05-51	Drought	NA	0.66	0.03	0.45	0.01
O2-	Leaf	YT93-159	Drought	NA	9.02	0.01	4.93	0.02
O2-	Leaf	YZ05-51	Drought	NA	8.51	0.01	8.61	0.01
O2-	Root	YT93-159	Drought	NA	10.24	0.01	10.16	0.06
O2-	Root	YZ05-51	Drought	NA	10.38	0.08	11.28	0.03
H2O2	Leaf	YT93-159	Drought	NA	10.30	0.19	8.55	0.13
H2O2	Leaf	YZ05-51	Drought	NA	12.53	0.13	13.02	0.13
H2O2	Root	YT93-159	Drought	NA	9.79	0.03	18.12	0.11
H2O2	Root	YZ05-51	Drought	NA	8.22	0.19	6.97	0.06
Proline	Leaf	YT93-159	Drought	NA	27.86	0.48	33.21	1.21
Proline	Leaf	YZ05-51	Drought	NA	29.96	0.64	27.87	1.31
Proline	Root	YT93-159	Drought	NA	28.87	0.06	32.31	0.57
Proline	Root	YZ05-51	Drought	NA	27.92	0.18	30.11	0.36
SP	Leaf	YT93-159	Drought	NA	13.29	0.04	16.71	0.13
SP	Leaf	YZ05-51	Drought	NA	13.75	0.07	13.81	0.03
SP	Root	YT93-159	Drought	NA	7.05	0.06	6.90	0.05
SP	Root	YZ05-51	Drought	NA	8.18	0.16	10.91	0.06
SS	Leaf	YT93-159	Drought	NA	6.18	0.00	6.61	0.01
SS	Leaf	YZ05-51	Drought	NA	5.65	0.01	6.00	0.00
SS	Root	YT93-159	Drought	NA	3.59	0.00	5.46	0.01
SS	Root	YZ05-51	Drought	NA	3.02	0.01	4.23	0.01
```

**行数验证**：10指标 × 2组织 × 2品种 = 40行 ✓

---

## 示例2：无SD的表格数据

### 输入表格
表2 PEG-6000胁迫下糯高粱幼苗形态指标变化

**表格结构**：
- 行：22个材料（M1-M22）
- 列：3个指标（株高、根长、鲜质量），每指标有CK和PEG处理
- 数据格式：均值+显著性字母（无±SD）

### 分析过程

**数据解析**：
```
12.60 a → 均值 = 12.60, SD = NA（无SD数据）
```

**对照匹配规则**：每个材料的CK作为PEG的对照

### 输出结果

```
Plant Height	M1	PEG	NA	11.40	NA	12.60	NA
Plant Height	M2	PEG	NA	14.40	NA	18.33	NA
Plant Height	M3	PEG	NA	14.50	NA	23.50	NA
Plant Height	M4	PEG	NA	13.77	NA	17.07	NA
Plant Height	M5	PEG	NA	12.40	NA	16.97	NA
Root Length	M1	PEG	NA	5.83	NA	7.43	NA
Root Length	M2	PEG	NA	7.33	NA	10.53	NA
Root Length	M3	PEG	NA	17.30	NA	21.10	NA
Root Length	M4	PEG	NA	7.83	NA	10.83	NA
Root Length	M5	PEG	NA	9.97	NA	13.10	NA
Fresh Weight	M1	PEG	NA	0.082	NA	0.093	NA
Fresh Weight	M2	PEG	NA	0.055	NA	0.125	NA
Fresh Weight	M3	PEG	NA	0.072	NA	0.145	NA
Fresh Weight	M4	PEG	NA	0.062	NA	0.124	NA
Fresh Weight	M5	PEG	NA	0.013	NA	0.016	NA
```

**行数验证**：3指标 × 22材料 = 66行 ✓

---

## 常见错误

1. ❌ 将显著性字母当作SD的一部分
2. ❌ 未正确解析均值±SD格式
3. ❌ 对照匹配错误（如跨组织匹配）
4. ❌ 遗漏了合并单元格的内容
5. ❌ 将SE当作SD使用
6. ❌ 无SD时错误地添加随机数

## 正确做法

1. ✅ 仔细解析±符号，忽略字母
2. ✅ 正确识别均值和SD
3. ✅ 同组织同品种的对照匹配
4. ✅ 识别合并单元格并填充
5. ✅ 检查是否需要SE到SD的转换
6. ✅ 无SD时，SD列标记为NA，不添加随机数
