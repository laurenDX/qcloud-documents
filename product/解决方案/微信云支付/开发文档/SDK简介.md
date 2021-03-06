## 为什么提供 SDK
- 云支付SDK对支付请求的各种参数进行打包和签名，封装了各种使用上的细节，并包含了自动重试，运行日志上传等功能，服务商使用更方便，且定位故障更快速和准确。

## SDK 提供了哪些接口
- SDK提供了**普通初始化**、**刷卡支付**、**扫码支付**、**查询订单**、**申请退款**、**查询退款**、**门店下载**，**订单批量下载**，**交易统计**这10个接口。
- 另外提供了安全相关的接口，帮助商户在本地存储敏感信息。**安全初始化**、**安全登录**、**安全登录查询**、**身份认证**相关的5个接口。
- 其中刷卡支付和扫码支付为异步方式，接口调用成功只代表支付提交成功，支付结果需要通过查询订单得到；取消订单、申请退款、退款查询、门店上传、门店下载接口为同步调用云支付接口。

## SDK 支持的第三方支付平台
- SDK支持微信支付和支付宝。

## SDK 支持的环境
- 当前只提供了 Windows 环境下的 SDK，后续会提供其他环境。

## 安全性
- 云支付专门提供了安全登录的接口，将商户的敏感信息做了加密和签名，防止黑客偷取或篡改商户信息，并验证登录收银机的人的身份，避免造成商户资金损失。

## 下载地址
- [windows环境运行Demo](https://main.qcloudimg.com/raw/7a89e034fe73cfb8b00ea19d7b953827.zip)，解压后包含两个目录：   
&radic;&nbsp;&nbsp;&nbsp;Demo\_tools目录, 可直接运行，用于验证服务商相关的账户等信息的正确性。    
&radic;&nbsp;&nbsp;&nbsp;Demo\_src目录，为Demo的源码，用于给开发者调用SDK的参考，方便开发者将SDK集成进自己的收银软件中。
- [windows环境运行SecurityDemo](https://main.qcloudimg.com/raw/4e4276190f61e0075678ac98bdc147d2.zip)，安全版SDK，增加了登录权限校验和敏感信息保护功能，目录结构及作用同上。
- [windows环境SDK](https://main.qcloudimg.com/raw/f2897de4f46e1e774fdc45bc31c51e46.zip)，解压后包含两个目录：   
&radic;&nbsp;&nbsp;&nbsp;CloudPayAPI\_SDK\_CPP\_DLL目录，包含编译好的dll库，可直接使用。  
&radic;&nbsp;&nbsp;&nbsp;CloudPayAPI\_SDK\_CPP目录，包含源码，开发者可自行编译。  
&radic;&nbsp;&nbsp;&nbsp;开发者集成云支付时，可参考Demo调用SDK的方式和API说明文档。
