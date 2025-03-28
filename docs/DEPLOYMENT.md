# Raydium CLMM 部署指南

本文档提供了如何将 Raydium CLMM 部署到 Solana 测试网和主网的详细步骤。

## 前置要求

- 已安装 Solana CLI 工具
- 已创建 Solana 钱包（部署者钱包）
- 确保钱包中有足够的 SOL 支付交易费用

## 测试网环境设置与测试

### 本地测试网启动

1. **启动本地测试验证节点**
   ```bash
   solana-test-validator
   ```
   保持此终端窗口运行，验证节点会持续运行。

2. **新开终端，配置连接到本地测试网**
   ```bash
   solana config set --url http://127.0.0.1:8899
   ```

3. **创建测试钱包（如果需要）**
   ```bash
   solana-keygen new -o test-wallet.json
   ```

4. **为测试钱包充值**
   ```bash
   solana airdrop 2 $(solana-keygen pubkey test-wallet.json)
   ```

### 构建和测试

1. **安装项目依赖**
   ```bash
   yarn install
   ```

2. **构建项目**
   ```bash
   anchor build
   ```

3. **运行测试套件**
   ```bash
   anchor test
   ```
   这将执行 `tests/` 目录下的所有测试用例。

4. **检查测试覆盖率**
   ```bash
   anchor test --coverage
   ```

### 测试网交互

1. **创建测试代币**
   ```bash
   spl-token create-token --decimals 6
   ```

2. **创建代币账户**
   ```bash
   spl-token create-account <TOKEN_ADDRESS>
   ```

3. **铸造测试代币**
   ```bash
   spl-token mint <TOKEN_ADDRESS> <AMOUNT> <RECIPIENT_ADDRESS>
   ```

4. **查看代币余额**
   ```bash
   spl-token balance <TOKEN_ADDRESS>
   ```

### 常见测试问题解决

1. **测试网连接问题**
   - 确保本地验证节点正在运行
   - 检查网络配置是否正确
   ```bash
   solana config get
   ```

2. **余额不足**
   - 使用 airdrop 命令获取更多测试 SOL
   ```bash
   solana airdrop 2 <WALLET_ADDRESS>
   ```

3. **交易失败**
   - 检查程序日志
   ```bash
   solana logs
   ```
   - 使用 Solana Explorer 查看详细错误信息

## 测试网部署步骤

1. **配置 Solana CLI 连接到测试网**
   ```bash
   solana config set --url https://api.devnet.solana.com
   ```

2. **确认连接状态**
   ```bash
   solana cluster-version
   ```

3. **部署合约**
   ```bash
   anchor deploy --provider.cluster devnet
   ```

4. **验证部署**
   - 在 Solana Explorer (https://explorer.solana.com/?cluster=devnet) 上查看部署的程序
   - 记录下程序 ID 以供后续使用

## 主网部署步骤

1. **配置 Solana CLI 连接到主网**
   ```bash
   solana config set --url https://api.mainnet-beta.solana.com
   ```

2. **确认连接状态**
   ```bash
   solana cluster-version
   ```

3. **部署合约**
   ```bash
   anchor deploy --provider.cluster mainnet
   ```

4. **验证部署**
   - 在 Solana Explorer (https://explorer.solana.com) 上查看部署的程序
   - 记录下程序 ID 以供后续使用

## 部署后配置

1. **更新程序 ID**
   - 在项目的 `Anchor.toml` 文件中更新程序 ID
   - 在前端代码中更新相应的程序 ID

2. **初始化必要的账户**
   - 部署后需要初始化必要的程序账户
   - 确保所有配置参数正确设置

## 安全注意事项

- 部署前务必进行充分的测试
- 确保部署钱包的私钥安全存储
- 主网部署前建议进行安全审计
- 确保程序的管理权限正确设置

## 常见问题处理

1. **部署失败**
   - 检查钱包余额是否充足
   - 确认网络连接是否稳定
   - 验证合约代码是否正确编译

2. **交易确认问题**
   - 检查网络拥堵情况
   - 适当调整交易费用

## 相关资源

- [Solana 文档](https://docs.solana.com)
- [Anchor 框架文档](https://www.anchor-lang.com)
- [Raydium 官方文档](https://docs.raydium.io)
