# 多市场股票与加密货币数据集

本数据集旨在为量化研究提供一个覆盖**中国A股 (CN)、美国 (US)、香港 (HK)、日本 (JP)、韩国 (KR)** 及**加密货币 (Crypto)** 的统一、高质量、版本化的金融数据源。数据通过qlib_duckdb导出，并定期更新。

---

## 🏢 数据覆盖范围与来源

数据按市场和来源进行分类，以确保结构清晰和可追溯。

| 市场 | 主要数据源 | 核心说明与特点 |
| :--- | :--- | :--- |
| **中国A股 (CN)** | **Tushare Pro** (`ts_`)<br>**AKShare** (`ak_`)<br>**Baostock** (`bs_`)<br>**Wind** (`w_`) | 核心市场。数据最为全面，包含日级价格、复权因子、涨跌停、指数成分等。采用多源交叉验证以确保高质量。 |
| **美国 (US)** | **Yahoo Finance** (`yahoo_`) | 通过 Python 库 `yfinance` 获取。提供美股、ETF 的 OHLCV 行情数据。 |
| **香港 (HK)** | **Yahoo Finance** (`yahoo_`)<br>**Tushare** (港股) | 港股行情与基础信息。 |
| **日本 (JP)** | **Yahoo Finance** (`yahoo_`) | 日股市场数据，通过 `yfinance` 获取。 |
| **韩国 (KR)** | **Yahoo Finance** (`yahoo_`) | 韩股市场数据，通过 `yfinance` 获取。 |
| **加密货币 (Crypto)** | **Binance**<br>**Yahoo Finance** (`yahoo_`) | 币安月度K线数据 (`binance_`) 及 Yahoo Finance 的加密货币价格。 |

### 数据表命名约定
在 Dolt 数据库或导出的数据中，表名前缀标识了其原始数据来源，例如 `ts_` 表示来自 Tushare。这种设计便于追溯和理解数据血缘。

---

## 📥 如何获取与使用数据

### 1. 主要仓库 (chenditc/investment_data)
此仓库是项目的核心，存储了经过处理、验证和合并后的最终数据集，尤其在中国A股数据方面最为完整。
或 investment_data 的 release 下载最新打包数据集

*   **下载为 Dolt 数据库（推荐，包含原始及中间表）**：
    ```bash
    dolt clone chenditc/investment_data
    cd investment_data
    # 使用 SQL 查询数据
    dolt sql -q "SELECT * FROM final_a_stock_eod_price LIMIT 5;"
    ```

*   **使用 Qlib 格式的数据（用于量化回测）**：
    仓库的 https://github.com/chenditc/investment_data/releases 页面提供了预打包的 Qlib 格式数据文件，可直接用于 https://github.com/microsoft/qlib 框架。
    ```bash
    # 示例：下载并解压中国A股数据
    wget https://github.com/chenditc/investment_data/releases/download/latest/qlib_bin.tar.gz
    mkdir -p ~/.qlib/qlib_data/cn_data
    tar -zxvf qlib_bin.tar.gz -C ~/.qlib/qlib_data/cn_data
    ```

### 2. 其他市场数据源
对于 US、HK、JP、KR 及部分 Crypto 的原始或增量数据，可以通过以下官方开源库获取：

*   **yfinance**: 用于获取 Yahoo Finance 上的全球股票数据（US, HK, JP, KR 等）。
    *   GitHub: https://github.com/ranaroussi/yfinance
    *   安装: `pip install yfinance`
    *   **重要声明**：yfinance 使用 Yahoo! 的公开 API，并非 Yahoo 的官方产品，仅供研究与教育使用。使用数据时请遵守 https://policies.yahoo.com/ie/en/yahoo/terms/product-atos/apiforydn/index.htm。

*   **Binance Data**: 官方的加密货币历史K线数据。
    *   数据地址: https://data.binance.vision/?prefix=data/spot/monthly/klines/
    *   提供币安现货市场各交易对的月度K线（CSV格式）文件。

---

## 🔧 数据处理流程与质量保证

本数据集的核心优势在于其**数据处理和质量验证流程**：

1.  **站在巨人肩膀上**: 从多个独立项目及数据源（如 Tushare, Yahoo Finance, Baostock 等）获取原始数据。
2.  **合并与导出**: 这些表会被导出为适用于 Qlib 等框架的格式,以提升OLAP的能力。

**核心理念**：我们追求的是**高质量数据 (high quality data)**，而非仅仅是**大量数据 (just data)**。不干净的数据可能导致错误的研究结论。

---

## 🤝 贡献与支持

这是一个社区驱动的项目，欢迎参与。

*   **报告问题**: 在 https://github.com/chenditc/investment_data/issues 中报告任何数据问题或提出新功能建议。
*   **贡献数据或代码**: 欢迎提交 Pull Request 来改进数据获取脚本或添加新的数据源。请先阅读仓库内的贡献指南，并在进行较大改动前发起 Issue 讨论（示例：https://github.com/chenditc/investment_data/issues/11）。
*   **赞助基础设施**: 维护每日更新的 CI/CD 管道需要稳定的服务器资源（需要 30G+ 内存，4核+ CPU 的 VPS）。如果您愿意赞助服务器费用或提供运行环境，请直接联系项目维护者。

---

## 📄 许可与致谢

*   **代码**: 项目代码采用 Apache License 2.0 许可。
*   **数据**: 数据集衍生自各个金融数据提供商。**商业使用时，请务必遵守 Tushare、Yahoo Finance、Binance 等原始数据提供商的服务条款。**
*   **特别感谢**: 感谢 **dmnsn7** 提供 Tushare Token 使得每日更新成为可能，也感谢所有为项目做出贡献的开发者。

有关更详细的数据表结构、更新脚本和开发环境设置，请访问主项目仓库：https://github.com/chenditc/investment_data。

> 文档未提及部分，但基于我所掌握的知识：
> 1.  **Yahoo Finance (yfinance)** 是获取全球多个市场（包括 US, HK, JP, KR, CA 等）免费股票数据的常用且强大的工具，但数据延迟和完整性可能因市场而异。
> 2.  **Binance 官方数据** 是获取加密货币历史行情最权威的免费来源之一，但通常不包含盘口深度（Order Book）数据。
