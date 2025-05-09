# 虚拟货币主力资金入场检测系统

这是一个基于Python的虚拟货币主力资金入场检测系统，可以自动监控虚拟货币市场，检测主力资金入场信号，并通过多种渠道（飞书、Telegram、微信）推送通知。系统集成了高级机器学习模型和强化安全机制。

![20250509-170324](https://github.com/user-attachments/assets/9e60f106-6a08-49f5-ab97-20d922dedf7f)



## 系统概述

本系统能够:
- 监控多条区块链上的虚拟货币交易和市场数据
- 分析大额交易、持仓集中度和市场深度等指标
- 使用机器学习模型（LSTM、随机森林、XGBoost）预测主力资金入场
- 检测主力资金入场信号
- 通过飞书、Telegram、微信等渠道推送通知
- 提供安全的API接口访问系统功能

## 功能特点

- **多链支持**: 支持多种区块链（如以太坊、Solana、BNB Chain、Polygon等）
- **多币种支持**: 支持多种虚拟货币（如BTC、ETH、SOL、DORA等）
- **多维度分析**: 从链上数据和市场数据两个维度进行分析
- **高级数据提供商**: 支持Glassnode、Nansen、DefiLlama等高级数据源
- **机器学习预测**: 集成LSTM、随机森林、XGBoost等模型进行趋势预测
- **规则引擎**: 基于规则的主力资金入场信号判断
- **多渠道通知**: 支持多种消息推送渠道
- **安全API接口**: 提供JWT认证、IP白名单、速率限制等安全机制
- **API密钥加密**: 提供API密钥的安全加密存储
- **配置灵活**: 支持自定义配置参数

## 安装指南

### 环境要求

- Python 3.8+
- 各API的访问权限（Etherscan、Web3、CCXT等）

### 依赖安装

```bash
pip install -r requirements.txt
```

### 配置文件

编辑`config/config.yaml`文件，填入必要的API密钥和配置信息:

```yaml
api_keys:
  etherscan: "YOUR_ETHERSCAN_API_KEY"
  telegram: "YOUR_TELEGRAM_API_KEY"
  feishu: "YOUR_FEISHU_API_KEY"
  wechat: "YOUR_WECHAT_API_KEY"
  web3: "YOUR_WEB3_API_KEY"
  ccxt: "YOUR_CCXT_API_KEY"
  covalent: "YOUR_COVALENT_API_KEY"
  dexscreener: "YOUR_DEXSCREENER_API_KEY"
  arkham: "YOUR_ARKHAM_API_KEY"
  glassnode: "YOUR_GLASSNODE_API_KEY"
  nansen: "YOUR_NANSEN_API_KEY"

# 监控的区块链网络
blockchain_networks:
  - ethereum
  - solana
  - binance_smart_chain
  - polygon

# 监控的虚拟货币列表
crypto_list:
  - BTC
  - ETH
  - SOL
  - DORA
```

### 环境变量配置

为了安全的API密钥存储，建议设置以下环境变量:

```bash
export CRYPTO_MASTER_KEY="你的主密钥"
export JWT_SECRET_KEY="你的JWT密钥"
```

## 使用方法

### 启动系统

```bash
python main.py
```

### 使用API服务

```bash
python -m api.app
```

API服务将在 http://localhost:8000 启动，可通过 http://localhost:8000/docs 访问API文档。

### 日志查看

日志文件保存在`logs/`目录下，命名格式为`crypto_signals_YYYY-MM-DD.log`。

## 系统架构

系统分为六个主要模块:

1. **数据采集模块**: 负责从多个数据源获取虚拟货币的链上数据和市场数据
   - 支持多链数据采集
   - 支持高级数据提供商
   
2. **特征工程模块**: 对原始数据进行处理和特征提取
   - 时间特征生成
   - 技术指标特征生成
   - 链上数据特征生成
   - 情感分析特征生成

3. **机器学习模块**: 使用多种模型对市场进行预测
   - LSTM进行时间序列预测
   - 随机森林进行分类
   - XGBoost进行高精度预测
   - 模型集成优化预测效果

4. **信号判断模块**: 基于采集到的数据和模型预测，判断是否存在主力资金入场信号

5. **消息推送模块**: 将检测到的信号通过Webhook推送到飞书、Telegram或微信

6. **API安全模块**: 提供安全的API访问接口
   - JWT认证
   - IP白名单
   - 速率限制
   - API密钥加密存储

## 目录结构

```
.
├── main.py                      # 主程序入口
├── config/                      # 配置文件目录
│   ├── config.yaml              # 参数配置（API Key、地址池等）
│   └── api_keys.json            # API密钥存储文件
├── data/
│   ├── fetch_chain.py           # 链上数据采集
│   ├── fetch_market.py          # 行情数据采集
│   ├── multi_chain_provider.py  # 多链数据提供商
│   └── advanced_data_provider.py # 高级数据提供商
├── analysis/
│   ├── rule_engine.py           # 主力判断逻辑
│   ├── feature_engine.py        # 特征提取与数据分析
│   ├── ml_models.py             # 机器学习模型
│   ├── feature_engineering.py   # 高级特征工程
│   └── prediction_service.py    # 预测服务
├── notify/
│   ├── telegram.py              # Telegram Webhook
│   ├── feishu.py                # 飞书 Webhook
│   └── wechat.py                # 微信推送模块
├── api/
│   └── app.py                   # API服务
├── security/
│   └── api_security.py          # API安全模块
└── utils/
    └── logger.py                # 日志与辅助模块
```

## 机器学习模型

系统集成了多种机器学习模型，用于预测主力资金入场:

- **LSTM模型**: 用于时间序列分析，预测价格趋势
- **随机森林模型**: 用于大额交易分类和检测
- **XGBoost模型**: 用于高精度市场走势预测
- **模型集成**: 结合多种模型的预测结果，提高准确率

## API接口

系统提供了RESTful API接口，支持以下功能:

- 用户认证
- 系统状态查询
- 信号查询
- API密钥管理
- 配置管理

API接口采用了多层安全机制:

- JWT令牌认证
- IP白名单
- 速率限制
- 安全审计日志

## 添加新的加密货币

要添加新的加密货币进行监控，请按照以下步骤操作:

1. 在`config/config.yaml`文件中的`crypto_list`部分添加新的加密货币符号:

```yaml
crypto_list:
  - BTC
  - ETH
  # 添加新的加密货币，例如:
  - NEW_COIN
```

2. 在`crypto_contracts`部分添加该加密货币的合约地址:

```yaml
crypto_contracts:
  # 其他加密货币...
  NEW_COIN:
    ethereum: "0x合约地址..."  # 以太坊上的合约地址
    chain: "ethereum"  # 主链名称
```

3. 如果该加密货币没有以太坊上的合约地址，您可以:
   - 使用其他链上的合约地址（需要实现多链支持）
   - 仅监控市场数据（通过CCXT库）

注意: 系统目前主要支持以太坊链上的代币。对于没有以太坊合约的原生代币（如BTC），系统使用其包装代币（如wBTC）的合约地址。
