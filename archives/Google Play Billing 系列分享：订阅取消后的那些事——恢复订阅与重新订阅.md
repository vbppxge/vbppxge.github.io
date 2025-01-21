作者：高寒蕊，Google 开发技术推广工程师

> **Google Play Billing 系列内容**是专门为中文开发者开辟的系列分享，着重讲解中国开发者对 Play Billing 最容易感到疑惑的地方。如果您有任何问题，也欢迎在留言区提出，我们会收集大家的反馈并在后续文章中做出解答。

## 恢复订阅与重新订阅的区别

作为订阅应用的开发者，您可能已经了解订阅业务模式对应用和游戏盈利的重要性。在本文中，我们将探讨用户取消订阅后的两种操作方式——**恢复订阅**和**重新订阅**，并分析它们的区别及注意事项。

### 恢复订阅（Restore）

恢复订阅的重点在于“恢复”，它有以下几个关键条件：
1. 用户已取消订阅；
2. 订阅尚未过期；
3. 只能在 Play 订阅中心操作。

由于是恢复操作，订阅实际上还是原来的，`purchaseToken` 不会发生变化。

### 重新订阅（Resubscribe）

重新订阅本质上是一次新的订阅购买，特点如下：
- 适用于已过期的订阅；
- 用户可以从应用内或 Play 订阅中心发起操作；
- 重新订阅会生成新的 `purchaseToken`。

需要注意的是，如果订阅过期超过一年，用户只能通过您的应用重新购买。

### 两者的对比

以下表格直观展示了恢复订阅与重新订阅的主要区别：

![恢复订阅与重新订阅对比](http://p7.itc.cn/q_70/images03/20200821/78892519fcd646a6b43d6d7f8b563be9.png)

### 特殊场景提醒

当用户取消的订阅尚未到期，又再次购买时：
- 新订阅将替换旧订阅，并在同一到期日期续订；
- 重新订阅会生成新的 `purchaseToken`，而旧的 `purchaseToken` 仍然有效。

开发者需要及时将旧的 `purchaseToken` 标注为失效，以避免同一用户对同一订阅产品（SKU）拥有多个有效的 `purchaseToken`。在这种情况下，您可以通过新的订阅中的 `linkedPurchaseToken` 字段找到旧的 `purchaseToken`。

> **注意**：不要依赖 `linkedPurchaseToken` 字段来判断旧订阅是否仍在有效期内。请始终通过 Google Developer API 的 `expiryTimeMillis` 字段获取订阅的正确到期时间。

## 开发者需要注意的事项

1. **及时确认购买交易**  
   对于重新购买的交易，开发者需要在三天内确认，否则交易将被 Play 取消。

2. **使用 Google Developer API**  
   通过 API 的 `expiryTimeMillis` 字段获取订阅的到期时间，确保数据准确。

3. **参考官方文档**  
   - [销售订阅](https://developer.android.google.cn/google/play/billing/subs)  
   - [实时开发者通知参考指南](https://developer.android.google.cn/google/play/billing/rtdn-reference)  
   - [Google Developer API 的订阅接口](https://developers.google.google.cn/android-publisher/api-ref/rest/v3/purchases.subions/get)

## 总结

恢复订阅和重新订阅是 Play Billing 中两个重要但容易混淆的功能。开发者需要根据用户的具体操作场景，正确处理 `purchaseToken` 和订阅状态。如果您对订阅还有任何疑问，欢迎在评论区留言。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)