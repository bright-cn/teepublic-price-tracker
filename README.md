# TeePublic 价格追踪器

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://bright.cn)
[![TeePublic Price Tracker](https://img.shields.io/badge/TeePublic%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://bright.cn/products/insights/price-tracker/teepublic)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://bright.cn/products/insights/price-tracker/teepublic)

实时 TeePublic 价格追踪——一个按需定制印花服饰和商品市场。两种入门方式：**全托管**情报平台，或使用 Bright Data 的 AI Scraper Builder 构建的**自定义 scraper**。

---

## 选项 1：Bright Insights - AI 驱动的价格追踪（推荐）

**[Bright Insights](https://bright.cn/products/insights/price-tracker/teepublic)** 是 Bright Data 的全托管零售情报平台。无需构建 scraper，无需维护基础设施——只需将结构化、可直接分析的价格数据交付到仪表板、数据 feed 或你的 BI 工具中。

**为什么团队选择 Bright Insights：**
- 🚀 **零配置** —— 使用开箱即用的仪表板和数据 feed，几分钟内即可上线
- 🤖 **AI 驱动的推荐** —— 对话式 AI 助手可将数百万数据点即时转化为可执行洞察
- ⚡ **实时监控** —— 从每小时到每天的刷新频率，并提供即时告警（email、Slack、webhook）
- 🌍 **无限扩展** —— 任意网站、任意地区、任意刷新频率
- 🔗 **即插即用集成** —— AWS、GCP、Databricks、Snowflake 等
- 🛡️ **全托管** —— Bright Data 自动处理 schema 变更、站点更新和数据质量问题

**关键使用场景：**
- ✅ **追踪季节性折扣** 和 TeePublic 上的促销活动
- ✅ **监控竞争对手定价** 并识别趋势
- ✅ 当商品价格低于目标价时，**自动触发价格提醒**
- ✅ 监控 MAP 政策合规性并检测价格违规
- ✅ 追踪竞争对手促销及促销动态
- ✅ 将干净、统一的数据直接输入动态定价算法或 AI 模型

> **起价 $250/月 - [获取定制报价 →](https://bright.cn/products/insights/price-tracker/teepublic)**

---

## 选项 2：构建你自己的 TeePublic Scraper

没有预构建的 TeePublic scraper API？没问题。Bright Data 的 **AI Scraper Builder** 只需点击几下即可生成自定义 TeePublic scraper——无需编写代码。

### 几分钟内构建你的 TeePublic scraper

**[打开 TeePublic AI Scraper Builder →](https://bright.cn/products/web-scraper/teepublic)**

选择域名，概述你的数据需求，让我们的 AI scraper builder 自动创建 API。

1. **用自然英语描述数据需求**
2. **AI 即时生成 scraper API**
3. **运行 API 请求并立即获得结果**
4. 如有需要，**在内置 IDE 中编辑代码**

构建完成后，你的 scraper 会获得一个 **Web Scraper ID**（`gd_xxxxxxxxxxxx`）——复制它，用于下面的 Setup 步骤。

### 前置条件

- Python 3.9 或更高版本
- 一个 [Bright Data account](https://bright.cn)（提供免费试用）
- 一个 Bright Data **API token**（[如何获取](https://docs.bright.cn/general/account/account-settings#api-token)）
- 一个用于 TeePublic 的 **Web Scraper ID**（来自上面的构建步骤）

### Setup

1. **克隆此 repository**

   ```bash
   git clone https://github.com/bright-cn/teepublic-price-tracker.git
   cd teepublic-price-tracker
   ```

2. **安装依赖**

   ```bash
   pip install -r requirements.txt
   ```

3. **配置凭据**

   将 `.env.example` 复制为 `.env` 并填入你的值：

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **你的 Web Scraper ID**
   > 将你的 [AI Scraper Builder dashboard](https://bright.cn/products/web-scraper/teepublic) 中的 Web Scraper ID
   > 粘贴到 `BRIGHTDATA_DATASET_ID` 中（格式：`gd_xxxxxxxxxxxx`）。

---

## 用法

一旦你的 TeePublic scraper 构建完成，并且 Web Scraper ID 已在 `.env` 中配置好，Python 接口的使用方式是相同的：

### 1. 通过 URL 追踪特定产品

传入 TeePublic 产品 URL 列表以获取结构化价格数据：

```python
from price_tracker import track_prices

urls = [
    "https://www.teepublic.com/en/products/sample-product-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

或直接运行：

```bash
python price_tracker.py
```

### 2. 通过关键词发现产品

查找与关键词搜索匹配的产品：

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 通过分类 URL 浏览产品

从 TeePublic 分类页面采集所有产品：

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://teepublic.com/category/example",
    limit=100,
)
```

---

## 输出字段

每条结果记录包含以下字段：

| 字段 | 描述 |
|-------|-------------|
| `url` | 产品页面 URL |
| `title` | 产品名称 |
| `brand` | 品牌 |
| `initial_price` | 原价 |
| `final_price` | 促销价 / 当前价格 |
| `currency` | 货币代码 |
| `discount` | 折扣 |
| `in_stock` | 库存状态 |
| `color` | 可选颜色 |
| `size` | 可选尺码 |
| `category` | 产品类别 / 部门 |
| `images` | 产品图片 URL |
| `description` | 产品描述 |
| `timestamp` | 采集时间戳 |

### 示例输出

```json
[
  {
    "url": "https://www.teepublic.com/en/products/sample-product-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://teepublic.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高级选项

`trigger_collection()` 函数接受可选参数来控制数据采集：

| 参数 | 类型 | 默认值 | 描述 |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返回记录的最大数量 |
| `include_errors` | boolean | `true` | 在结果中包含错误报告 |
| `notify` | string (URL) | - | 当快照准备就绪时调用的 webhook URL |
| `format` | string | `json` | 输出格式：`json`、`csv` 或 `ndjson` |

选项示例：

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.teepublic.com/en/products/sample-product-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## 资源

- 🌟 [TeePublic Price Tracker - Bright Insights (Managed)](https://bright.cn/products/insights/price-tracker/teepublic)
- 🏗️ [Build a TeePublic Scraper](https://bright.cn/products/web-scraper/teepublic)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.bright.cn/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://bright.cn/cp/scrapers)
- 🔑 [How to get an API token](https://docs.bright.cn/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://bright.cn)

---

*使用 [Bright Data](https://bright.cn) 构建——行业领先的 web 数据平台。*