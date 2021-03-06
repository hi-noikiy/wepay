---
title: '查询分账结果API'
linkTitle: '查询分账'
date: '2020-04-01'
weight: 3
description: >
  发起分账请求后, 可调用此接口查询分账结果 ;发起分账完结请求后, 可调用此接口查询分账完结的结果
type: 'docs'
---

## 接口说明

**适用对象**: 电商平台\
**请求 URL**: https://api.mch.weixin.qq.com/v3/ecommerce/profitsharing/orders\
**请求方式**: GET\
**接口规则**: https://wechatpay-api.gitbook.io/wechatpay-api-v3

`path` 指该参数需在请求 URL 传参\
`query` 指该参数需在请求 JSON 传参

## 请求参数

| 变量           | 类型       | 必填 | 参数名/描述/示例值                                                                                                                                      |
| -------------- | ---------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sub_mchid      | string(32) | 是   | `二级商户号` `path` 分账出资的电商平台二级商户, 填写微信支付分配的商户号。                                                                              |
|                |            |      | 1900000109                                                                                                                                              |
| transaction_id | string(32) | 是   | `微信订单号` `path` 微信支付订单号。                                                                                                                    |
|                |            |      | 4208450740201411110007820472                                                                                                                            |
| out_order_no   | string(64) | 是   | `商户分账单号` `path` 商户系统内部的分账单号, 在商户系统内部唯一(单次分账、多次分账、完结分账应使用不同的商户分账单号）, 同一分账单号多次请求等同一次。 |
|                |            |      | P20150806125346                                                                                                                                         |

### 请求示例

`URL` https://api.mch.weixin.qq.com/v3/ecommerce/profitsharing/orders?sub_mchid=1900000109&transaction_id=4208450740201411110007820472&out_order_no=P20150806125346

## 返回参数

| 变量               | 类型       | 必填 | 参数名/描述/示例值                                                                                                                               |
| ------------------ | ---------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| sub_mchid          | string(32) | 是   | `二级商户号` 分账出资的电商平台二级商户, 填写微信支付分配的商户号。                                                                              |
|                    |            |      | 1900000109                                                                                                                                       |
| transaction_id     | string(32) | 是   | `微信支付订单号`                                                                                                                                 |
|                    |            |      | 4208450740201411110007820472                                                                                                                     |
| out_order_no       | string(64) | 是   | `商户分账单号` 商户系统内部的分账单号, 在商户系统内部唯一(单次分账、多次分账、完结分账应使用不同的商户分账单号）, 同一分账单号多次请求等同一次。 |
|                    |            |      | P20150806125346                                                                                                                                  |
| order_id           | string(64) | 是   | `微信分账单号`, 微信系统返回的唯一标识                                                                                                           |
|                    |            |      | 008450740201411110007820472                                                                                                                      |
| status             | string(32) | 是   | `分账单状态`, 枚举值:<br>ACCEPTED: 受理成功<br>PROCESSING: 处理中<br>FINISHED: 分账成功<br>CLOSED: 处理失败, 已关单                              |
|                    |            |      | FINISHED                                                                                                                                         |
| receivers          | array      | 是   | `+分账接收方列表` 分账接收方列表。                                                                                                               |
| close_reason       | string(32) | 否   | `关单原因` 描述, 枚举值:<br>NO_AUTH: 分账授权已解除                                                                                              |
|                    |            |      | NO_AUTH                                                                                                                                          |
| finish_amount      | int        | 否   | `分账完结金额` 分账完结的分账金额, 单位为分, 仅当查询分账完结的执行结果时, 存在本字段。                                                          |
|                    |            |      | 100                                                                                                                                              |
| finish_description | string(80) | 否   | `分账完结描述` 分账完结的原因描述, 仅当查询分账完结的执行结果时, 存在本字段。                                                                    |
|                    |            |      | 分账完结                                                                                                                                         |

### 返回示例

正常示例

```json
{
  "sub_mchid": "1900000109",
  "transaction_id": "4208450740201411110007820472",
  "out_order_no": "P20150806125346",
  "order_id": "3008450740201411110007820472",
  "status": "FINISHED",
  "receivers": {
    "receiver_mchid": "1900000110",
    "amount": 100,
    "description": "分给商户 1900000110",
    "result": "SUCCESS",
    "finish_time": "2015-05-20T13:29:35.120+08:00",
    "fail_reason": "ACCOUNT_ABNORMAL"
  },
  "close_reason": "NO_AUTH",
  "finish_amount": 100,
  "finish_description": "分账完结"
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
