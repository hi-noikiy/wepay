---
title: '关于申请退款、对账单下载的问题'
weight: 3
---

支付分接口没有单独的申请退款、对账单下载接口, 商户可以调用普通支付的接口来完成这些操作, 接口文档可以查看 API 列表->申请退款、查询退款、退款结果通知、下载对账单, 其中申请退款需要使用【支付成功回调通知】接口回调的 transaction_id 作为条件, transaction_id 等于普通支付接口中的 transaction_id
