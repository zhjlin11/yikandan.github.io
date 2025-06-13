# 项目说明

本仓库是 **易开单宝** 的示例实现，目标用户为需要快速生成、管理并分享销售单的商家/企业。项目由 Node.js 服务端和微信小程序组成，支持创建销售单、生成公章及查看分享订单等功能。

## 功能概览

- **销售单创建**：在小程序中选择客户和商品填写数量即可生成订单，服务端会自动生成公章和支付二维码。
- **销售单查看与分享**：可在订单列表进入详情页查看订单，并通过微信分享带公章和二维码的销售单给客户。
- **基础数据管理**：服务端提供客户、商品及订单的增删改查接口（小程序暂未提供相应管理页面）。
- **微信支付（示例）**：后端包含预下单和回调接口示例，前端尚未完成支付流程接入。

## 当前进度

- 服务端已实现客户、商品及订单的 CRUD 接口，并在创建订单时生成公章与支付二维码。
- 微信小程序提供登录、下单、列表、详情、编辑及删除等页面，详情页可一键分享。
- 提供基于 Jest 的接口测试示例（需先安装依赖）。
- 微信支付流程仅实现预下单及回调示例，尚未与前端完整打通。

## 公章生成方案调研

常见的公章生成方式通常借助 Canvas 绘图。以下是两种常见场景的方案：

### Node 端
- **canvas (node-canvas)**：Node 环境常用的 Canvas 实现，API 与浏览器 Canvas 接近，可在服务器上生成 PNG 或 JPEG 等格式图像。适合在创建订单后在服务端生成公章图像。

### 小程序端
- **<canvas> 组件**：微信小程序自身提供的 Canvas 组件，可直接在前端绘制图形并导出临时图片，可配合 `wx.canvasToTempFilePath` 导出图像。

两种方式均可绘制圆形边框、文字、星形等元素以实现常见的公章效果。

在服务端创建销售单时，项目会使用 `canvas` 自动生成带订单编号的公章图片，并随订单数据一起返回。小程序的订单详情页提供了“分享”按钮，分享时会携带该公章图片及支付二维码，方便客户直接付款。

## 运行

1. **安装依赖**
   ```bash
   cd server
   npm install
   ```

2. **配置环境变量**
   - `MYSQL_URI`：MySQL 连接地址，例如 `mysql://user:pass@localhost:3306/app_db`
   - `WECHAT_APPID`、`WECHAT_MCHID`、`WECHAT_KEY`：微信商户相关参数
   - `WECHAT_NOTIFY_URL`：微信支付回调地址

3. **启动服务**
   ```bash
   npm start
   ```

4. **运行测试（可选）**
   ```bash
   npm test
   ```
