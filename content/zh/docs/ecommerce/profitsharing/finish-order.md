---
title: '完结分账API'
linkTitle: '完结分账'
date: '2019-09-11'
weight: 6
description: >
  不需要进行分账的订单, 可直接调用本接口将订单的金额全部解冻给特约商户。
type: 'docs'
---

{{% alert title="注意:" color="warning"%}}

- 调用分账接口后, 需要解冻剩余资金时, 调用本接口将剩余的分账金额全部解冻给特约商户。

{{% /alert %}}

## 接口说明

**适用对象**: `电商平台`\
**请求 URL**: https://api.mch.weixin.qq.com/v3/ecommerce/profitsharing/finish-order\
**请求方式**: POST\
**接口规则**: https://wechatpay-api.gitbook.io/wechatpay-api-v3

`path` 指该参数需在请求 URL 传参\
`query` 指该参数需在请求 JSON 传参

## 请求参数

| 变量           | 类型       | 必填 | 参数名/描述/示例值                                                                                                                                       |
| -------------- | ---------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sub_mchid      | string(32) | 是   | `二级商户号` `query` 分账出资的电商平台二级商户, 填写微信支付分配的商户号。                                                                              |
|                |            |      | 1900000109                                                                                                                                               |
| transaction_id | string(32) | 是   | `微信订单号` `query` 微信支付订单号。                                                                                                                    |
|                |            |      | 4208450740201411110007820472                                                                                                                             |
| out_order_no   | string(64) | 是   | `商户分帐单号` `query` 商户系统内部的分账单号, 在商户系统内部唯一(单次分账、多次分账、完结分账应使用不同的商户分账单号）, 同一分账单号多次请求等同一次。 |
|                |            |      | P20150806125346                                                                                                                                          |
| description    | string(80) | 是   | `分账描述` `query` 分账的原因描述, 分账账单中需要体现。                                                                                                  |
|                |            |      | 分账完结                                                                                                                                                 |

### 请求示例

```json
{
  "sub_mchid": "1900000109",
  "transaction_id": "4208450740201411110007820472",
  "out_order_no": "P20150806125346",
  "description": "分账完结"
}
```

## 返回参数

| 变量           | 类型       | 必填 | 参数名/描述/示例值                                                                                                                               |
| -------------- | ---------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| sub_mchid      | string(32) | 是   | `二级商户号` 分账出资的电商平台二级商户, 填写微信支付分配的商户号。                                                                              |
|                |            |      | 1900000109                                                                                                                                       |
| transaction_id | string(32) | 是   | `微信订单号` 微信支付订单号。                                                                                                                    |
|                |            |      | 4208450740201411110007820472                                                                                                                     |
| out_order_no   | string(64) | 是   | `商户分帐单号` 商户系统内部的分账单号, 在商户系统内部唯一(单次分账、多次分账、完结分账应使用不同的商户分账单号）, 同一分账单号多次请求等同一次。 |
|                |            |      | P20150806125346                                                                                                                                  |
| order_id       | string(64) | 是   | `微信分帐单号` 微信分账单号, 微信系统返回的唯一标识。                                                                                            |
|                |            |      | 3008450740201411110007820472                                                                                                                     |

### 返回示例

#### 正常示例

```json
{
  "sub_mchid": "1900000109",
  "transaction_id": "4208450740201411110007820472",
  "out_order_no": "P20150806125346",
  "order_id": "3008450740201411110007820472"
}
```

## 错误码<sub>公共错误码</sub>

| 状态码 | 错误码            | 描述           | 解决方案                                                                                                                                                                                                                                                                                                                                                                          |
| ------ | ----------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 500    | SYSTEM_ERROR      | 系统错误       | 系统异常, 请使用相同参数稍后重新调用                                                                                                                                                                                                                                                                                                                                              |
| 400    | PARAM_ERROR       | 参数错误       | 请使用正确的参数重新调用                                                                                                                                                                                                                                                                                                                                                          |
| 400    | INVALID_REQUEST   | 参数错误       | 请使用正确的参数重新调用<br>分账金额超限 分账金额不能大于可分金额或大于最大分账比例金额, 请调整分账金额<br>分账接收方非法 分账接收方在分账之前需要进行添加<br>请求参数不符合参数格式 请求参数错误, 检查原交易号是否存在或发起支付交易接口返回失败<br>订单处理中, 暂时无法分账 订单处理中, 暂时无法分账, 请稍后再试<br>不是分账订单, 无法分账 发起支付交易接口时请用分账的合适参数 |
| 429    | FREQUENCY_LIMITED | 频率限制       | 请降低频率后重试                                                                                                                                                                                                                                                                                                                                                                  |
| 403    | NO_AUTH           | 未开通分账权限 | 请开通商户号分账权限                                                                                                                                                                                                                                                                                                                                                              |
