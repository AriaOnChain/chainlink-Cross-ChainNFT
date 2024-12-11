# chainlink-Cross-ChainNFT
基于 Chainlink CCIP以及实时价格获取的跨链 NFT 铸造系统
本项目展示了一个基于 Chainlink CCIP（跨链互操作协议）的跨链 NFT 铸造系统，支持在不同区块链网络之间进行通信。系统包含三个智能合约，分别部署在不同网络上：
## 合约说明
1. CrossChainPriceNFT（部署在 Sepolia）
功能：管理 NFT 铸造和动态元数据更新。
特点：
通过 Chainlink 数据预言机获取链上 BTC/USD 的实时价格。
根据价格走势动态生成 NFT 元数据和 SVG 图片。
支持跨链铸造，NFT 元数据包含来源链的信息。
2. CrossDestinationMinter（部署在 Sepolia）
功能：接收跨链消息并调用 CrossChainPriceNFT 的铸造函数。
特点：
通过 Chainlink CCIP 接收跨链消息。
解码消息后调用目标合约执行 NFT 铸造。
3. CrossSourceMinter（部署在 Fuji）
功能：从源链发送跨链消息，触发目标链上的 NFT 铸造。
特点：
集成 Chainlink CCIP 路由器，用于跨链消息发送。
管理 LINK 余额以支付跨链消息费用。
支持 LINK 提现功能。
## 部署与使用
### 部署步骤
在 Sepolia 部署 CrossChainPriceNFT 和 CrossDestinationMinter 合约。
在 Fuji 部署 CrossSourceMinter 合约。
配置 CrossDestinationMinter 指向 CrossChainPriceNFT 合约。
配置 CrossSourceMinter 指向 CrossDestinationMinter 合约。
### 使用方法
向 CrossSourceMinter 转入足够的 LINK 以支付跨链消息费用。
调用 CrossSourceMinter 的 mintOnSepolia() 方法：
源链（Fuji）通过 Chainlink CCIP 向目标链（Sepolia）发送消息。
Sepolia 处理消息并在 CrossChainPriceNFT 合约中铸造 NFT。
