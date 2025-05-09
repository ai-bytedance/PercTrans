# 虚拟货币主力资金入场检测系统

这是一个基于Python的虚拟货币主力资金入场检测系统，可以自动监控虚拟货币市场，检测主力资金入场信号，并通过多种渠道（飞书、Telegram、微信）推送通知。系统集成了高级机器学习模型和强化安全机制。

![20250509-170324](https://github.com/user-attachments/assets/9e60f106-6a08-49f5-ab97-20d922dedf7f)


## 系统概述

- 监控多条区块链上的虚拟货币交易和市场数据
- 分析大额交易、持仓集中度和市场深度等指标
- 使用机器学习模型（LSTM、随机森林、XGBoost）预测主力资金入场
- 检测主力资金入场信号
- 通过飞书、Telegram、微信等渠道推送通知
- 提供安全的API接口访问系统功能

## 功能特点

- **多链支持**: 支持多种区块链（如以太坊、Solana、BNB Chain、Polygon等）
- **多币种支持**: 支持多种虚拟货币（如BTC、ETH、SOL等）
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
  etherscan: "YOUR_ETHERSCAN_API_KEY"  # 以太坊区块链浏览器API
  web3: "YOUR_WEB3_API_KEY"  # 如Infura或Alchemy的API密钥
  ccxt: "YOUR_CCXT_API_KEY"  # 交易所API密钥（如需要）
  covalent: "YOUR_COVALENT_API_KEY"  # Covalent区块链数据API
  dexscreener: "YOUR_DEXSCREENER_API_KEY"  # DEX数据API
  arkham: "YOUR_ARKHAM_API_KEY"  # Arkham情报API（如需要）

  # 通知相关API密钥
  telegram: "YOUR_TELEGRAM_API_KEY"  # Telegram Bot API密钥
  feishu: "YOUR_FEISHU_API_KEY"  # 飞书API密钥
  wechat: "YOUR_WECHAT_API_KEY"  # 微信API密钥

# 监控的虚拟货币列表
crypto_list:
  - BTC
  - ETH
  - SOL
  # 可添加更多...
```

> 注意：您可以根据需要注释掉不需要使用的API密钥。系统会根据可用的API密钥自动选择数据源。

### 通知配置

系统支持多种通知渠道，可在`config/config.yaml`中配置:

```yaml
# 通知配置
notification:
  enabled: true  # 是否启用通知
  channels:
    - "feishu"  # 可选: "feishu", "wechat", "telegram"
  webhook_urls:
    feishu: "https://open.feishu.cn/open-apis/bot/v2/hook/YOUR_FEISHU_WEBHOOK"
    wechat: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=YOUR_WECHAT_WEBHOOK"
    telegram:
      bot_token: "YOUR_TELEGRAM_BOT_TOKEN"
      chat_id: "YOUR_TELEGRAM_CHAT_ID"
```

### 代理设置

如果您需要使用代理访问API，可以配置代理设置:

```yaml
# 代理设置
proxy:
  enabled: false  # 是否启用代理
  type: "http"    # 代理类型：http, https, socks5
  host: "127.0.0.1"  # 代理服务器地址
  port: 7890      # 代理服务器端口
  username: ""    # 代理认证用户名（如果需要）
  password: ""    # 代理认证密码（如果需要）
  # 不使用代理的域名列表
  no_proxy:
    - "localhost"
    - "127.0.0.1"
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
    chain: "ethereum"  # 主链名称，如ethereum, binance, tron等
```

3. 对于没有以太坊合约地址的加密货币，可以使用占位符并指定其主链:

```yaml
NEW_NATIVE_COIN:
  ethereum: "0x1b1a3c7f1a1a1a1a1a1a1a1a1a1a1a1a1a1a1a1a"  # 占位符
  chain: "native_chain_name"  # 例如：bitcoin, cardano, ripple等
```

系统当前支持多种加密货币，包括:
- 以太坊代币 (ETH, SHIB, PEPE等)
- 比特币及其分叉 (BTC, BCH, BSV)
- 其他主流公链代币 (SOL, BNB, ADA, XRP, TRX等)
- 热门代币 (DOGE, ORDI, SUI, OP等)

注意: 
- 对于没有以太坊合约的原生代币（如BTC），系统使用其包装代币（如wBTC）的合约地址进行链上数据监控
- 对于某些链，系统可能仅支持市场数据监控，而不支持完整的链上数据分析
- 添加新代币时，请确保在相应的数据提供商中也有该代币的支持

## 代理配置与诊断

系统支持通过代理访问各种API，特别适合在网络受限环境中使用。

### 代理配置

在`config/config.yaml`文件中配置代理设置:

```yaml
# 代理设置
proxy:
  enabled: true  # 设置为false可禁用代理
  type: "http"    # 代理类型：http, https, socks5
  host: "127.0.0.1"  # 代理服务器地址
  port: 7890      # 代理服务器端口
  username: ""    # 代理认证用户名（如果需要）
  password: ""    # 代理认证密码（如果需要）
  # 不使用代理的域名列表
  no_proxy:
    - "localhost"
    - "127.0.0.1"
```

### 代理诊断

系统启动时会自动检测代理连接状态，并在日志中输出诊断信息。如果遇到API连接问题，请检查:

1. 代理服务器是否正常运行
2. 代理配置是否正确
3. 日志中是否有代理连接错误信息

如果代理连接测试失败，系统会在日志中提示您检查代理配置或考虑禁用代理。

### 常见问题排查

如果遇到数据获取失败的情况:

1. 检查日志文件中的错误信息
2. 验证API密钥是否有效
3. 尝试禁用代理后重新运行系统
4. 确认目标API服务是否可用
5. 检查网络连接状态