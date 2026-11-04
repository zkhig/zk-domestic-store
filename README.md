# 中科健康 AI 旗舰店 Skill

`zk-domestic-store` 是中科国内产品的 Agent Skill。安装后，AI 可按规范查询产品资料、实时价格与折扣、当期活动、抵用券，并在用户明确确认后完成订单预览、建单、支付和订单查询。

> 本仓库只分发纯指令 Skill，不包含商城数据库、管理后台、支付密钥或任何顾客 Token。实际业务由独立 MCP 服务执行。

## 前置条件

- 支持 Agent Skills 的 AI 工具。
- 支持 Streamable HTTP MCP 的客户端。
- 可访问的正式 MCP：`https://open.zkjk.net/mcp`。
- 本人可接收短信验证码的手机号。

**只安装 Skill 不会自动获得商品或下单数据；还需要可访问的 MCP。首次涉及领券、订单或支付时，按 AI 提示完成手机号授权即可，无需手工配置 Token。**

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
安装后，请引导我完成手机号授权。
```

也可以下载 GitHub 自动生成的源码 ZIP，在支持 Skills 的客户端中导入：

```text
https://github.com/skychen527/zk-domestic-store/archive/refs/heads/main.zip
```

## 手机号授权与 MCP

正式 MCP 为 `https://open.zkjk.net/mcp`，登录页为 `https://open.zkjk.net/login`。

首次需要身份的操作遇到未授权响应时，Agent 直接询问手机号并调用 `startPhoneLogin` 发送验证码。客户本人在对话中输入验证码后，Agent 调用 `completePhoneLogin` 完成登录，继续领券和下单，无需扫码或复制 Token。支持 OAuth 的平台也可通过授权码和 PKCE 自动完成鉴权。

访问令牌由服务端自动签发和刷新，用户不需要复制、粘贴或保存 Token。示例配置见 `mcp-config.example.json`，不要把真实 Token 写入本仓库、公开聊天或公开配置。

## 统一购物流程

欢迎内容会同时引导客户聊产品或聊日常保健需求：熟悉产品的客户直接查询并购买，无需强行健康问诊。

客户聊日常保健需求时，严格按以下顺序：

1. 客户的 AI Agent 先分析健康问题，给出作息、饮食、运动、压力管理等一般保健建议。
2. 再调用 `getProductDetail` 和 `getProductKnowledge`，结合批准功能、原料工艺、适宜与不适宜人群和实时价格，解释产品推荐理由并给出图文组合。
3. 推荐后调用 `getCurrentPromotions` 展示后台当期活动和抵用券。
4. 最后引导客户完成手机号注册登录、领券、订单预览、明确确认和支付。

健康描述只做日常保健指引，不做疾病诊断；疾病、持续不适或异常症状先建议就医。客户需求与批准功能不匹配时明确不推荐，不为了成交强行组合。

## 使用示例

产品查询：

```text
用中科健康 AI 旗舰店查询赛若斯孢子油的标准品名、规格、批准文号、实时价格和当期活动。
```

健康咨询与推荐：

```text
最近睡眠不太好，请先给我一些日常保健建议，再结合中科产品知识说明哪些产品适合我、为什么，最后查询当期优惠。
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
- `getProductKnowledge`：获取批准说明、原料科普、品质工艺和推荐依据。
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

当前版本：`0.5.1`，见 `manifest.json`。Skill 的正式 MCP 地址为 `https://open.zkjk.net/mcp`；域名、SSL 和正式支付能力以实际部署状态为准。

License：MIT-0，见 `LICENSE`。
