## 数字货币支付解决方案
-----
### coinbase commerce

[Coinbase commerce](https://commerce.coinbase.com)是coinbase提供的数字货币支付解决方案，商家通过创建支付单，可以让用户支付相应金额的数字货币。

![](/assets/屏幕快照 2018-10-17 下午9.50.01.png)

Coinbase Commerce 同时提供了资产管理界面，方面商家查看订单，管理资产

![](/assets/屏幕快照 2018-10-17 下午9.49.42.png)


### 支付流程

#### 1.创建订单

创建订单API

```
POST https://api.commerce.coinbase.com/charges
```

参数

```
curl https://api.commerce.coinbase.com/charges \
     -X POST \
     -H 'Content-Type: application/json' \
     -H "X-CC-Api-Key: <Your API Key>" \
     -H "X-CC-Version: 2018-03-22"
     -d '{
       "name": "Pivot",
       "description": "Pivot Deposit",
       "pricing_type": "no_price",
       "metadata": {
         "customer_id": "用户ID",
         "customer_name": "用户邮箱"
       }
     }'
```

创建成功后，订单response如下：

```
{
    "data": {
        "addresses": {
            "bitcoincash": "qzjn0aaeljfm2azkwwc6prd4f88re0lrp5xpc9863n",
            "bitcoin": "1MMUsUH5CKmhHKMaMCXhM99T9pSdmjeK9U",
            "ethereum": "0x1b5c4cc377bbad3a7d172314464494de54546dbf",
            "litecoin": "LgHoYEDxaaYxr3zrh8yFRGHDLJPy4H73WN"
        },
        "code": "HH7WJJW2",
        "created_at": "2018-10-17T13:57:20Z",
        "description": "Deposite",
        "expires_at": "2018-10-17T14:57:20Z",
        "hosted_url": "https://commerce.coinbase.com/charges/HH7WJJW2",
        "id": "69deb540-da44-4f19-8281-f924e6ba3872",
        "logo_url": "https://res.cloudinary.com/commerce/image/upload/v1539783006/zwcfpdlib1vkiys31b6k.png",
        "metadata": {
            "customer_id": "id_1005",
            "customer_name": "Satoshi Nakamoto"
        },
        "name": "Pivot",
        "payments": [],
        "pricing_type": "no_price",
        "resource": "charge",
        "timeline": [
            {
                "status": "NEW",
                "time": "2018-10-17T13:57:20Z"
            }
        ]
    }
}
```

* addresses中提供了btc,bch,eth,ltc的支付二维码
* code是支付订单，或者叫转账订单唯一id，可用户查询订单
* expires_at是支付过期时间
* metadata是创建订单时填写的辅助信息


#### 2.查询订单


```
# 查询url
https://api.commerce.coinbase.com/charges/HH7WJJW2


{
    "data": {
        "addresses": {
            "bitcoincash": "qzjn0aaeljfm2azkwwc6prd4f88re0lrp5xpc9863n",
            "ethereum": "0x1b5c4cc377bbad3a7d172314464494de54546dbf",
            "bitcoin": "1MMUsUH5CKmhHKMaMCXhM99T9pSdmjeK9U",
            "litecoin": "LgHoYEDxaaYxr3zrh8yFRGHDLJPy4H73WN"
        },
        "code": "HH7WJJW2",
        "confirmed_at": "2018-10-17T13:59:45Z",
        "created_at": "2018-10-17T13:57:20Z",
        "description": "Deposite",
        "expires_at": "2018-10-17T14:57:20Z",
        "hosted_url": "https://commerce.coinbase.com/charges/HH7WJJW2",
        "id": "69deb540-da44-4f19-8281-f924e6ba3872",
        "logo_url": "https://res.cloudinary.com/commerce/image/upload/v1539783006/zwcfpdlib1vkiys31b6k.png",
        "metadata": {
            "customer_id": "id_1005",
            "customer_name": "Satoshi Nakamoto"
        },
        "name": "Pivot",
        "payments": [
            {
                "network": "ethereum",
                "transaction_id": "0xf339bb22495e52406e22c0ac6cda3992d379d1e6eb673375974add021a4cea84",
                "status": "CONFIRMED",
                "detected_at": "2018-10-17T13:58:28Z",
                "value": {
                    "local": {
                        "amount": "0.20",
                        "currency": "USD"
                    },
                    "crypto": {
                        "amount": "0.001000000",
                        "currency": "ETH"
                    }
                },
                "block": {
                    "height": 6532259,
                    "hash": "0x8feba7a92bd1d32bda17850dae8a3bdbcaf8fa0b5f96da16117f1f5af40dc3ee",
                    "confirmations": 9,
                    "confirmations_required": 8
                }
            }
        ],
        "pricing_type": "no_price",
        "resource": "charge",
        "timeline": [
            {
                "status": "NEW",
                "time": "2018-10-17T13:57:20Z"
            },
            {
                "status": "PENDING",
                "time": "2018-10-17T13:58:28Z",
                "payment": {
                    "network": "ethereum",
                    "transaction_id": "0xf339bb22495e52406e22c0ac6cda3992d379d1e6eb673375974add021a4cea84"
                }
            },
            {
                "status": "COMPLETED",
                "time": "2018-10-17T13:59:45Z",
                "payment": {
                    "network": "ethereum",
                    "transaction_id": "0xf339bb22495e52406e22c0ac6cda3992d379d1e6eb673375974add021a4cea84"
                }
            }
        ]
    },
    "warnings": [
        "Missing X-CC-Version header; serving latest API version (2018-03-22)"
    ]
}

```

* 在payments中是支付信息，重点是value字段

```
"value": {
	"local": {
		"amount": "0.20",
		"currency": "USD"
	},
	"crypto": {
		"amount": "0.001000000",
		"currency": "ETH"
	}
},

```

crypto中显示了用户支付的货币数量和货币单位

在timeline字段中显示了支付状态

```
        "timeline": [
            {
                "status": "NEW",
                "time": "2018-10-17T13:57:20Z"
            },
            {
                "status": "PENDING",
                "time": "2018-10-17T13:58:28Z",
                "payment": {
                    "network": "ethereum",
                    "transaction_id": "0xf339bb22495e52406e22c0ac6cda3992d379d1e6eb673375974add021a4cea84"
                }
            },
            {
                "status": "COMPLETED",
                "time": "2018-10-17T13:59:45Z",
                "payment": {
                    "network": "ethereum",
                    "transaction_id": "0xf339bb22495e52406e22c0ac6cda3992d379d1e6eb673375974add021a4cea84"
                }
            }
        ]
```

一共有下面几种状态：NEW, PENDING, COMPLETED, EXPIRED, UNRESOLVED, RESOLVED, CANCELED



#### 3. 订单列表

coinbase commerce只提供了查询所有订单的功能

```
GET https://api.commerce.coinbase.com/charges
```

我们需要备份订单信息，并写入用户信息方便做查询


#### 4. 取消订单

```
curl https://api.commerce.coinbase.com/charges/66BEOV2A/cancel \
     -X POST \
     -H "X-CC-Api-Key: <Your API Key>" \
     -H "X-CC-Version: 2018-03-22"
```



[Coinbase Commerce API Document](https://commerce.coinbase.com/docs/api/#cancel-a-charge)




#### 5. webhook

coinbase 接口一个api key一分钟只允许使用25次，我们采用创建多个api key解决创建问题，使用webhook来更新订单信息


webhook设置后coinbase会对以下事件发送通知

* charge:created    创建
* charge:confirmed  支付成功
* charge:failed     支付失败
* charge:delayed    支付单过期
* charge:pending    支付确认中


以下是不同事件回调的数据结构

###### charge:created

```
{
    "attempt_number":1,
    "event":{
        "api_version":"2018-03-22",
        "created_at":"2018-10-23T09:12:08Z",
        "data":{
            "addresses":{
                "bitcoin":"1NCwSJbcpCee4MnyQspk19wL6KXaq1TXq",
                "bitcoincash":"qzvnnwndnh5j3tcrsrrhe7s2nc2c5rzzvyysttx7c7",
                "ethereum":"0x7774401f8288e63d26c683390c4049a8dd9c777e",
                "litecoin":"LbpFM99pzzEEWeaTT3gyFqxmkVvUC6VNdW"
            },
            "code":"T9WARCVV",
            "created_at":"2018-10-23T09:12:08Z",
            "description":"Deposite",
            "expires_at":"2018-10-23T10:12:08Z",
            "hosted_url":"https://commerce.coinbase.com/charges/T9WARCVV",
            "id":"a80abc29-4bf2-4b1f-a08b-76ec7242565d",
            "logo_url":"https://res.cloudinary.com/commerce/image/upload/v1539783006/zwcfpdlib1vkiys31b6k.png",
            "metadata":{
                "customer_id":"id_1005",
                "customer_name":"sam"
            },
            "name":"Pivot",
            "payments":[],
            "pricing_type":"no_price",
            "resource":"charge",
            "timeline":[
                {
                    "status":"NEW",
                    "time":"2018-10-23T09:12:08Z"
                }
            ]
        },
        "id":"111c5e80-3142-4800-ac72-d6aefd418b0e",
        "resource":"event",
        "type":"charge:created"
    },
    "id":"0f11f6cd-136f-4560-a8a7-cf5186fbd1d2",
    "scheduled_for":"2018-10-23T09:12:08Z"
}
```


###### charge:pending

```

{
    "attempt_number":1,
    "event":{
        "api_version":"2018-03-22",
        "created_at":"2018-10-23T09:14:36Z",
        "data":{
            "addresses":{
                "bitcoin":"1NCwSJbcpCee4MnyQspk19wL6KXaq1TXq",
                "bitcoincash":"qzvnnwndnh5j3tcrsrrhe7s2nc2c5rzzvyysttx7c7",
                "ethereum":"0x7774401f8288e63d26c683390c4049a8dd9c777e",
                "litecoin":"LbpFM99pzzEEWeaTT3gyFqxmkVvUC6VNdW"
            },
            "code":"T9WARCVV",
            "created_at":"2018-10-23T09:12:08Z",
            "description":"Deposite",
            "expires_at":"2018-10-23T10:12:08Z",
            "hosted_url":"https://commerce.coinbase.com/charges/T9WARCVV",
            "id":"a80abc29-4bf2-4b1f-a08b-76ec7242565d",
            "logo_url":"https://res.cloudinary.com/commerce/image/upload/v1539783006/zwcfpdlib1vkiys31b6k.png",
            "metadata":{
                "customer_id":"id_1005",
                "customer_name":"sam"
            },
            "name":"Pivot",
            "payments":[
                {
                    "block":{
                        "confirmations":0,
                        "confirmations_required":8,
                        "hash":"0x476df935ee06e268b8dfd693fc1f84659ce482bd7558e1b8fba12b9282fe6f18",
                        "height":6567790
                    },
                    "detected_at":"2018-10-23T09:14:35Z",
                    "network":"ethereum",
                    "status":"PENDING",
                    "transaction_id":"0x168d88c295859dba3a4876674bef7ce3e6d5688ccd8b8323e33c9d5da561d03c",
                    "value":{
                        "crypto":{
                            "amount":"0.001000000",
                            "currency":"ETH"
                        },
                        "local":{
                            "amount":"0.20",
                            "currency":"USD"
                        }
                    }
                }
            ],
            "pricing_type":"no_price",
            "resource":"charge",
            "timeline":[
                {
                    "status":"NEW",
                    "time":"2018-10-23T09:12:08Z"
                },
                {
                    "payment":{
                        "network":"ethereum",
                        "transaction_id":"0x168d88c295859dba3a4876674bef7ce3e6d5688ccd8b8323e33c9d5da561d03c"
                    },
                    "status":"PENDING",
                    "time":"2018-10-23T09:14:35Z"
                }
            ]
        },
        "id":"3d8131a0-e6cb-4965-9653-ef1eacb89f78",
        "resource":"event",
        "type":"charge:pending"
    },
    "id":"f6904211-164b-4984-aff3-d4122f460367",
    "scheduled_for":"2018-10-23T09:14:36Z"
}
```

###### charge:confirmed

```
{
    "attempt_number":1,
    "event":{
        "api_version":"2018-03-22",
        "created_at":"2018-10-23T09:17:11Z",
        "data":{
            "addresses":{
                "bitcoin":"1NCwSJbcpCee4MnyQspk19wL6KXaq1TXq",
                "bitcoincash":"qzvnnwndnh5j3tcrsrrhe7s2nc2c5rzzvyysttx7c7",
                "ethereum":"0x7774401f8288e63d26c683390c4049a8dd9c777e",
                "litecoin":"LbpFM99pzzEEWeaTT3gyFqxmkVvUC6VNdW"
            },
            "code":"T9WARCVV",
            "confirmed_at":"2018-10-23T09:17:11Z",
            "created_at":"2018-10-23T09:12:08Z",
            "description":"Deposite",
            "expires_at":"2018-10-23T10:12:08Z",
            "hosted_url":"https://commerce.coinbase.com/charges/T9WARCVV",
            "id":"a80abc29-4bf2-4b1f-a08b-76ec7242565d",
            "logo_url":"https://res.cloudinary.com/commerce/image/upload/v1539783006/zwcfpdlib1vkiys31b6k.png",
            "metadata":{
                "customer_id":"id_1005",
                "customer_name":"sam"
            },
            "name":"Pivot",
            "payments":[
                {
                    "block":{
                        "confirmations":7,
                        "confirmations_required":8,
                        "hash":"0x476df935ee06e268b8dfd693fc1f84659ce482bd7558e1b8fba12b9282fe6f18",
                        "height":6567790
                    },
                    "detected_at":"2018-10-23T09:14:35Z",
                    "network":"ethereum",
                    "status":"CONFIRMED",
                    "transaction_id":"0x168d88c295859dba3a4876674bef7ce3e6d5688ccd8b8323e33c9d5da561d03c",
                    "value":{
                        "crypto":{
                            "amount":"0.001000000",
                            "currency":"ETH"
                        },
                        "local":{
                            "amount":"0.20",
                            "currency":"USD"
                        }
                    }
                }
            ],
            "pricing_type":"no_price",
            "resource":"charge",
            "timeline":[
                {
                    "status":"NEW",
                    "time":"2018-10-23T09:12:08Z"
                },
                {
                    "payment":{
                        "network":"ethereum",
                        "transaction_id":"0x168d88c295859dba3a4876674bef7ce3e6d5688ccd8b8323e33c9d5da561d03c"
                    },
                    "status":"PENDING",
                    "time":"2018-10-23T09:14:35Z"
                },
                {
                    "payment":{
                        "network":"ethereum",
                        "transaction_id":"0x168d88c295859dba3a4876674bef7ce3e6d5688ccd8b8323e33c9d5da561d03c"
                    },
                    "status":"COMPLETED",
                    "time":"2018-10-23T09:17:11Z"
                }
            ]
        },
        "id":"3c1c3659-db1e-4d94-8290-66940cadc762",
        "resource":"event",
        "type":"charge:confirmed"
    },
    "id":"773f244f-a571-4be9-9968-72b5e4859a8e",
    "scheduled_for":"2018-10-23T09:17:11Z"
}
```

###### charge:delayed






###### charge:failed

