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

在tiemline字段中显示了支付状态

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