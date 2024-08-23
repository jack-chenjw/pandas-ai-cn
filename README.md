# 写在前面
为什么会有Pandas-ai-cn这个项目？
1. PandasAI 没有中文文档；
2. PandasAI 不支持中国地区常用的AI大模型；
3. PandasAI 的提示词和回答中，对中文的支持不太好；
4. PandasAI 对免费模型的支持较弱，不利于测试；
5. PandasAI 提示词不透明；
6. docker-compose 的兼容性不好。

所以Fork了这个项目，希望方便中文用户的使用。

# 

以下是PandasAI 2.2.14版本的readme文档的中文翻译（有删改）：

# ![PandasAI](assets/logo.png)

[![Release](https://img.shields.io/pypi/v/pandasai?label=Release&style=flat-square)](https://pypi.org/project/pandasai/)
[![CI](https://github.com/gventuri/pandas-ai/actions/workflows/ci.yml/badge.svg)](https://github.com/gventuri/pandas-ai/actions/workflows/ci.yml/badge.svg)
[![CD](https://github.com/gventuri/pandas-ai/actions/workflows/cd.yml/badge.svg)](https://github.com/gventuri/pandas-ai/actions/workflows/cd.yml/badge.svg)
[![Downloads](https://static.pepy.tech/badge/pandasai)](https://pepy.tech/project/pandasai) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZnO-njhL7TBOYPZaqvMvGtsjckZKrv2E?usp=sharing)

PandasAI 是一个 Python 平台，使用户可以使用自然语言向数据提问。它帮助非技术用户更自然地与数据互动，也帮助技术用户在处理数据时节省时间和精力。Pandas-ai-cn是PandasAI的中文区适配版本。

# 🚀 部署 PandasAI

PandasAI 可以通过多种方式使用。你可以在 Jupyter notebooks 或 streamlit 应用中轻松使用它，或者将其部署为 REST API，比如使用 FastAPI 或 Flask。

# 🔧 快速入门

你可以在[此处](https://pandas-ai.readthedocs.io/en/latest/)找到 PandasAI 的完整文档。

你可以选择在 Jupyter notebooks、streamlit 应用中使用 PandasAI，或者从代码仓库中使用客户端和服务器架构。

## ☁️ 使用平台

[![PandasAI platform](assets/demo.gif?raw=true)](https://www.youtube.com/watch?v=kh61wEy9GYM)

### 📦 安装

PandasAI 平台使用 Docker 化的客户端-服务器架构。你需要在机器上安装 Docker。

```bash
git clone https://github.com/sinaptik-ai/pandas-ai/
cd pandas-ai
docker-compose build
```
（本项目作者：我在windows上使用docker-compose build没有成功，有成功的请私信告知我一下）

### 🚀 运行平台

构建平台后，可以运行：

```bash
docker-compose up
```

这将启动客户端和服务器，你可以在 `http://localhost:3000` 访问客户端。

## 📚 使用库

### 📦 安装

你可以使用 pip 或 poetry 安装 PandasAI 库。

使用 pip：

```bash
pip install pandasai
```

使用 poetry：

```bash
poetry add pandasai
```

### 🔍 演示

在浏览器中试用 PandasAI 库（需科学上网）：

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZnO-njhL7TBOYPZaqvMvGtsjckZKrv2E?usp=sharing)

### 💻 使用方法

#### 提问

```python
import os
import pandas as pd
from pandasai import Agent

# 示例数据框
sales_by_country = pd.DataFrame({
    "国家": ["美国", "英国", "法国", "德国", "意大利", "西班牙", "加拿大", "澳大利亚", "日本", "中国"],
    "销售额": [5000, 3200, 2900, 4100, 2300, 2100, 2500, 2600, 4500, 7000]
})

# 默认情况下，除非你选择不同的 LLM，否则会使用 BambooLLM。
# 你可以在 https://pandabi.ai 注册获取免费的 API 密钥（也可以在 .env 文件中配置）
os.environ["PANDASAI_API_KEY"] = "这里填入你的API_KEY"

agent = Agent(sales_by_country)
agent.chat('按销售额从高到低排序，列出前5个国家。')
```

```
中国、美国、日本、德国、澳大利亚
```

---

或者你可以提出更复杂的问题：

```python
agent.chat(
    "按销售额从高到低排序，计算前3个国家的总销售额是多少。"
)
```

```
前三个销售国家的总销售额为 16500。
```

#### 可视化图表

你也可以让 PandasAI 为你生成图表：

```python
agent.chat(
    "绘制一个显示每个国家GDP的柱状图。为每个柱子使用不同的颜色。",
)
```

![Chart](assets/histogram-chart.png?raw=true)

#### 多个数据框（dataframe）

你也可以向 PandasAI 传递多个数据框并提出相关问题。

```python
import os
import pandas as pd
from pandasai import Agent

employees_data = {
    'EmployeeID': [1, 2, 3, 4, 5],
    'Name': ['John', 'Emma', 'Liam', 'Olivia', 'William'],
    'Department': ['HR', 'Sales', 'IT', 'Marketing', 'Finance']
}

salaries_data = {
    'EmployeeID': [1, 2, 3, 4, 5],
    '工资': [5000, 6000, 4500, 7000, 5500]
}

employees_df = pd.DataFrame(employees_data)
salaries_df = pd.DataFrame(salaries_data)

# 默认情况下，除非你选择不同的 LLM，否则会使用 BambooLLM。
# 你可以在 https://pandabi.ai 注册获取免费的 API 密钥（也可以在 .env 文件中配置）
os.environ["PANDASAI_API_KEY"] = "这里填入你的API_KEY"

agent = Agent([employees_df, salaries_df])
agent.chat("谁的工资最高?")
```

```
Olivia 的工资最高。
```

你可以在 [examples](examples) 目录中找到更多示例。

## 🔒 隐私与安全

为了生成运行的 Python 代码，我们从数据框（dataframe）中随机取样，并对其进行随机化处理（对敏感数据使用随机生成，对非敏感数据使用随机排序），并仅将随机化后的头部发送给 LLM。

如果你希望进一步加强隐私，可以实例化 PandasAI 时设置 `enforce_privacy = True`，这样只会将列名发送给 LLM，而不是头部数据。

## 📜 许可证

PandasAI 可在 MIT 许可下使用，`pandasai/ee` 目录已删除。

## 资源

- [文档](https://pandas-ai.readthedocs.io/en/latest/) 获取完整文档
- [示例](examples) 获取示例笔记本

## 🤝 贡献

欢迎贡献！请查看未解决的问题，并随时提交拉取请求（pull request）。
欲了解更多信息，请查看[贡献指南](CONTRIBUTING.md)。

### 谢谢！
