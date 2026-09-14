# 基于Spark的电商网站用户行为统计分析与个性化推荐

## 项目简介

基于Spark对电商用户行为数据进行统计分析与个性化推荐，使用ALS协同过滤算法实现商品推荐，结果写入MySQL并可视化。

## 技术栈

- **数据处理**：PySpark + Spark SQL
- **存储**：HDFS + MySQL
- **机器学习**：Spark MLlib (ALS协同过滤)
- **可视化**：PyEcharts
- **语言**：Python 3.8+

## 项目架构

```
原始CSV数据 → Pandas数据清洗(ETL) → HDFS存储 → Spark SQL多维度统计分析 → MySQL存储
                                                          ↓
                                              Spark MLlib ALS协同过滤推荐
                                                          ↓
                                              PyEcharts数据可视化展示
```

## 目录结构

```
ecommerce-user-behavior-analysis/
├── README.md                    # 项目说明
├── requirements.txt             # 依赖包
├── config/
│   └── config.py                # 配置文件（路径、参数）
├── data/
│   └── README.md                # 数据说明
├── src/
│   ├── 01_data_preprocessing.py # 数据预处理与ETL
│   ├── 02_spark_sql_analysis.py # Spark SQL统计分析
│   ├── 03_als_recommendation.py  # ALS协同过滤推荐
│   └── 04_visualization.py      # PyEcharts可视化
├── output/                       # 输出结果
└── docs/
    └── architecture.md           # 详细架构说明
```

## 数据说明

数据集：电商用户行为数据（UserBehavior）
- 数据量：500万+条
- 字段：用户ID、商品ID、商品类目ID、行为类型、时间戳、地理位置
- 行为类型：浏览(pv)、收藏(fav)、加购(cart)、购买(buy)

## 运行步骤

### 1. 环境准备
```bash
pip install -r requirements.txt
```

### 2. 数据预处理
```bash
python src/01_data_preprocessing.py
```
- 读取500万+条CSV数据
- 统计各字段缺失值与数据质量
- 删除空白值过多的user_geohash字段
- 新增province维度字段
- 清洗后数据上传HDFS

### 3. Spark SQL统计分析
```bash
python src/02_spark_sql_analysis.py
```
- 用户行为统计（浏览/收藏/加购/购买分布）
- 31个省份维度统计分析
- 支持多维度下钻查询
- 结果写入MySQL

### 4. ALS协同过滤推荐
```bash
python src/03_als_recommendation.py
```
- 按8:2划分训练集和测试集
- 训练ALS协同过滤推荐模型
- 模型评估（RMSE约0.56）
- 为10000+用户推荐Top10商品
- 为每个商品推荐Top10用户

### 5. 数据可视化
```bash
python src/04_visualization.py
```
- 用户行为分布饼图
- 用户行为分布柱状图
- 省份统计柱状图

## 项目成果

1. 完成电商用户行为数据从预处理、HDFS存储、Spark SQL分析到ALS协同过滤推荐的完整数据处理流程
2. 实现用户行为多维度统计分析和个性化商品推荐
3. ALS模型RMSE约0.56，推荐效果良好

## 量化指标

| 指标 | 数值 |
|------|------|
| 数据处理量 | 500万+条 |
| 覆盖省份 | 31个 |
| 推荐用户数 | 10000+ |
| 推荐商品数 | Top10/用户 |
| 模型RMSE | 约0.56 |
| 训练集:测试集 | 8:2 |
