# 中科健康 AI 旗舰店 Skill

`zk-domestic-store` 是中科国内产品的 Agent Skill。安装后，AI 可按规范查询产品资料、实时价格与折扣、当期活动、抵用券，并在用户明确确认后完成订单预览、建单、支付和订单查询。

> 本仓库只分发纯指令 Skill，不包含商城数据库、管理后台、支付密钥或任何顾客 Token。实际业务由独立 MCP 服务执行。

## 前置条件

- 支持 Agent Skills 的 AI 工具。
- 支持 Streamable HTTP MCP 的客户端。
- 可访问的正式 MCP：`https://open.zkjk.net/mcp`。
- 旗舰店为每位顾客签发的独立 `ZHONGKE_MCP_TOKEN`。

**只安装 Skill 不会自动获得商品或下单数据；还必须配置可访问的 MCP 和有效顾客 Token。**

## 从 GitHub 安装

### 使用 skills CLI

```bash
npx skills add skychen527/zk-domestic-store
```

### OpenClaw

```bash
openclaw skills install git:skychen527/zk-domestic-store
```

### 千问办公、Kimi Code 或兼容 Agent

把下面内容发给智能体：

```text
请从 GitHub 下载安装中科健康 AI 旗舰店 Skill：
https://github.com/skychen527/zk-domestic-store
安装后，请引导我配置 ZHONGKE_MCP_TOKEN。
```

也可以下载 GitHub 自动生成的源码 ZIP，在支持 Skills 的客户端中导入：

```text
https://github.com/skychen527/zk-domestic-store/archive/refs/heads/main.zip
```

## 配置 MCP

```json
{
  "mcpServers": {
    "zk-domestic-store": {
      "type": "streamableHttp",
      "url": "https://open.zkjk.net/mcp",
      "headers": {
        "Authorization": "Bearer ${ZHONGKE_MCP_TOKEN}"
      }
    }
  }
}
```

也可以使用仓库中的 `mcp-config.example.json`。不要把真实 Token 写入本仓库、公开聊天或公开配置。

## 使用示例

产品查询：

```text
用中科健康 AI 旗舰店查询赛若斯孢子油的标准品名、规格、批准文号、实时价格和当期活动。
```

领券与订单预览：

```text
帮我领取满50减10券，买1瓶赛若斯孢子油。先生成订单预览，不要直接下单。
```

确认建单：

```text
确认下单。
```

支付后查询：

```text
我已经完成支付，请查询订单和支付状态。
```

AI 必须先预览订单并取得用户明确确认，才能创建订单。用户“看看”“了解一下”不构成购买授权。

## 工具能力

- `searchProducts`：搜索产品与实时销售信息。
- `getProductDetail`：获取完整产品档案。
- `getCurrentPromotions`：查询当期优惠活动。
- `listVouchers`：查询当前可领抵用券。
- `claimVoucher`：领取指定抵用券。
- `listMyVouchers`：查询本人抵用券。
- `previewOrder`：计算最终订单并获取一次性预览 Token。
- `createOrder`：确认后创建待支付订单和支付参数。
- `queryOrder`：查询本人订单。
- `cancelOrder`：取消待支付订单。
- `queryPayment`：查询支付状态。

## 合规说明

保健功能只使用产品注册或备案口径，不生成疾病治疗、抗癌、替代药物或保证效果等宣传。介绍保健食品时应附上：“保健食品不是药物，不能代替药物治疗疾病”。

## 版本

当前版本：`0.1.0`，见 `manifest.json`。Skill 的正式 MCP 地址为 `https://open.zkjk.net/mcp`；域名、SSL 和正式支付能力以实际部署状态为准。

License：MIT-0，见 `LICENSE`。
