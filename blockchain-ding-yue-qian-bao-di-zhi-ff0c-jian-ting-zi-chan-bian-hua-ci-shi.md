## 创建钱包，发起订阅

服务端给用户生产新地址后，通过blockchain接口发起监控请求

```
Post  https://api.blockchain.info/v2/receive/balance_update
content-type: application/json

{
	"key": "blockchain.info api key",
	"addr": "1LWGhsvVyYz3vWRBfDSG7EPjpq2Mu3upWG",
	"callback": "https://4565adb6.ngrok.io",
	"onNotification":"KEEP",
	"confs": 5
}


其中addr为要订阅的地址

```

https://api.blockchain.info/v2/receive/balance_update

* address - The address you would like to monitor
* callback - The callback URL to be notified when a payment is received.
* key - Your blockchain.info receive payments v2 api key. Request an API key.
* onNotification - The request notification behaviour ('KEEP' | 'DELETE).
* confs - Optional (Default 3). The number of confirmations the transaction needs to have before a notification is sent.
* op - Optional (Default 'ALL'). The operation type you would like to receive notifications for ('SPEND' | 'RECEIVE' | 'ALL').



## Step 1 - 新支付单通知

当用户向订阅的地址发起支付后，我们会收到一条推送通知，格式如下：
```
{
    "address":"1LWGhsvVyYz3vWRBfDSG7EPjpq2Mu3upWG",
    "confirmations":0,
    "transaction_hash":"894d51c5e3de4f2f3d8991f10103ad2e4cdbb8ec77cac7cd6493e2d45e6b0773",
    "value":6000
}
```

可以看到，blockchain通过confirmations字段来表示支付状态，0表示pending

value为转账金额，已聪为单位


## Step 2 - 到账通知

发起地址订阅时，我们设定了confs字段为5(默认是3)，表示网络确认5次后，blockchain会发送通知

```
{
    "address":"1LWGhsvVyYz3vWRBfDSG7EPjpq2Mu3upWG",
    "confirmations":5,
    "transaction_hash":"3777ee95f8165ff25fc39a29d2f8303b9eb959fe2dab058702a5e4900dcaab96",
    "value":5000
}
```


## 注意事项

1. 通知发送有一定延迟，会比实际确认时间晚（我单个测试的时间是3分钟左右）
2. 会有重复发送的情况，所以我们要根据transaction_hash确定唯一充值，避免重复



