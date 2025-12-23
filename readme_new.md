## 目录结构

- `test_data/`：测试数据
- `text_analysis_tools/`：功能 API
- `examples.py`：使用样例
- `social_media_text_intelligence.py`：轻量舆情分析工具包（如已加入仓库）


## 功能一览

### A. text_analysis_tools（通用文本分析）

- 文本分类
- 文本聚类
- 文本相似性
- 关键词抽取
- 关键短语抽取
- 情感分析
- 文本纠错
- 文本摘要
- 主题关键词
- 同义词、近义词
- 事件三元组抽取

### B. Social Media Text Intelligence Toolkit（轻量舆情分析）

- 文本清洗：去掉 URL、话题、@、多余空白与标点
- 分词：优先使用 jieba，缺省时采用正则分词
- 关键词 / 主题抽取：简单的 TF-IDF 频率策略
- 情感分析：小型情感词典的符号得分
- 事件触发词：基于关键词的轻量事件标记
- 时间趋势：按天/小时聚合情绪占比
## Social Media Text Intelligence Toolkit

仓库新增了一个轻量级的中文舆情分析工具包，覆盖文本清洗、主题/情感/事件抽取以及时间趋势汇总。

### 功能一览

- 文本清洗：去掉 URL、话题、@、多余空白与标点
- 分词：优先使用 jieba，缺省时采用正则分词
- 关键词 / 主题抽取：简单的 TF-IDF 频率策略
- 情感分析：小型情感词典的符号得分
- 事件触发词：基于关键词的轻量事件标记
- 时间趋势：按天/小时聚合情绪占比

### 快速开始（直接运行内置示例）

```bash
python social_media_text_intelligence.py
```

会打印：
- 主题与情感报告：主题快照、情感分布、事件命中示例
- 时间趋势：按天聚合的情感比例

### 命令行：处理 JSONL 输入

准备一个 `posts.jsonl`，每行形如：

```json
{"text": "新品发布会太精彩了，直播抽奖真的给力！", "timestamp": "2024-05-01 12:00"}
{"text": "客服迟迟不回复，体验很差，考虑投诉。", "timestamp": "2024-05-01 15:00"}
```

运行：

```bash
python social_media_text_intelligence.py --input posts.jsonl --dimension day
```

可选参数：
- `--stopwords path/to/stopwords.txt`：自定义停用词表（每行一个词）
- `--sentiment-positive path/to/pos.txt` / `--sentiment-negative path/to/neg.txt`：自定义情感词典
- `--dimension day|hour`：趋势聚合粒度

### 在代码中使用（API）

```python
from social_media_text_intelligence import PostRecord, SocialMediaTextIntelligenceToolkit

toolkit = SocialMediaTextIntelligenceToolkit()
posts = [
    PostRecord(text="新品发布会太精彩了，直播抽奖真的给力！", timestamp="2024-05-01 12:00"),
    PostRecord(text="客服迟迟不回复，体验很差，考虑投诉。", timestamp="2024-05-01 15:00"),
]

results = toolkit.analyze_posts(posts)
print(results["report"])
print(results["trend"])
```

默认词典覆盖常见停用词、情感词和事件触发词，可按需传入自定义词表以适配行业场景。

### 作为可安装包使用

```bash
pip install .
smti --input posts.jsonl
```

### 下一步可拓展

- 增强主题模型（如 TextRank / KeyBERT）与事件模式匹配
- 增加可视化输出（曲线/柱状图）与告警阈值
- 增加 Kafka/CSV/JSONL 批处理适配器与监控面板
