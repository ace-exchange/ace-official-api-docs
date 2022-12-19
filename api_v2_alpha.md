
# Version 2 General API Information (still on development, will be released around at 12/15)
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* For GET endpoints, parameters must be sent as url parameters.
* For Post endpoints, parameters format is  x-www-form-urlencoded.
* Parameters may be sent in any order.
* Two kinds of api:\
  a. `Oapi` apis can be used directly without signature authentication.<br>
  b. `Open` apis require `apiKey` and `secretKey`, can apply them in ACE web `https://ace.io/`.

# Signing API Requests
Important Note: Do not reveal your `apiKey` and `secretKey` to anyone. They are as important as your password.

To prevent the request(s) from being tampered with during the process of network transmission, `secretKey` signature authentication via `SHA256` is required to guarantee that you are the source of the request(s). The signature needs to be placed in the url parameter.

* Three parameters are required, including `ACE_SIGN`, `timestamp` and `phone number`, all are involved in signature.
* Follow the rule to combine the parameters : `ACE_SIGN  + Secret Key + parameters values`.
* If  `Secret Key` is `xxxxxx`, then your will get `ACE_SIGNxxxxxx`.
* Then if you have parameters like: `apiKey=AAA&timeStamp=12121212&quoteCurrencyId=1&baseCurrencyId=2`, only get the values with natural order to get `AAA1212121212`.
* Combine `ACE_SIGNxxxxxx` + `AAA1212121212` to get `ACE_SIGNxxxxxxAAA1212121212`
* Signing the obtained string with the `SHA256` algorithm results: `e265a45639d473add583fe58d448fa516550f7cb9428284c0909cfd3380dbed8`. 
* Place this result into `signKey` parameter.



# ENUM definitions
### Kline/Candlestick chart intervals:
* 1: 1 min
* 5: 5 mins
* 10: 10 mins
* 30: 30 mins
* 60: 1 hour
* 120: 2 hours
* 240: 4 hours
* 480: 8 hours
* 720: 12 hours
* 24: 1 day
* 70: 1 week
* 31: 1 month

### Order side:
* BUY  = 1 
* SELL = 2

### Currency ID 
#### Updated list is in Market Pair api.
* TWD	1
* BTC	2
* ETH	4
* LTC	7
* XRP	10
* TRX	13
* USDT	14
* BNB	17
* BTTC	19
* USDC	57
* MOCT	66
* DET	70
* QQQ	72
* HT	74
* UNIC	75
* QTC	76
* FTT	81
* BAAS	83
* OKB	84
* DAI	85
* ACEX	88
* LINK	89
* DEC	90
* HWGC	93
* KNC	94
* COMP	95
* DS	96
* CRO	97
* CREAM	101
* YFI	102
* WNXM	103
* MITH	104
* SGB	106
* ENJ	107
* ANKR	108
* MANA	109
* SXP	110
* CHZ	111
* DOT	112
* CAKE	114
* SHIB	115
* DOGE	116
* MATIC	117
* WOO	119
* SLP	120
* AXS	121
* ADA	122
* QUICK	123
* FTM	124
* YGG	126
* GALA	127
* ILV	128
* DYDX	129
* SOL	130
* SAND	131
* AVAX	132
* LOOKS	133
* APE	135
* GMT	136
* GST	139
* TON	141
* SSV	144
* BUSD	145

# Oapi API - Trade Data
    GET https://ace.io/polarisex/oapi/v2/list/tradePrice
### Response:

Key:
  `<baseCurrencyName>/<quoteCurrencyName>`
 
Value:
| Name | Type | Description |
| ---- | ----  | ---- |
| base_volume | String | 24hr volume |
| last_price | String  | the newest price |
| quote_volume | String  | 24hr turnover, the unit is base on the base currency |

```json=
    {
        "BTC/USDT":{
            "base_volume": "229196.34035399999",
            "last_price": "11881.06",
            "quote_volume": "19.2909"
        }
     }
```

# Oapi API - Market Pair
    GET https://ace.io/polarisex/oapi/v2/list/marketPair
### Response:
| Name | Type  | Description |
| ---- | ---- | ---- |
| symbol | String |      |
| base | String | gmt/usdt, base is btc |
| quote | String |gmt/usdt, quote is gmt |
| basePrecision |String| the precision of base |
| quotePrecision | String| the precision of quote |
| minLimitBaseAmount | String | the minimal amount of base |
| maxLimitBaseAmount | String | the max amount of base |


```json=
    {
       "symbol":"BTC/USDT",
       "base":"gmt",
       "quote":"usdt",
       "basePrecision":"8",
       "quotePrecision":"5",
       "minLimitBaseAmount":"0.1",
       "maxLimitBaseAmount":"480286"
    }
```
# Open API - Order Books
    GET https://ace.io/polarisex/open/v2/public/getOrderBook

### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| quoteCurrencyId | INT | YES |
| baseCurrencyId | INT | YES |
| depth | INT | NO | size of bids & asks, default: all |

### Response:
    bids are buy orders, asks are sell orders. first value is volume and second value is price

```json=
{
    "attachment": {
        "baseCurrencyId": "2",
        "quoteCurrencyId": "14",
        "baseCurrencyName": "BTC",
        "quoteCurrencyName": "USDT",
        "bids": [
            [
                "0.0009",
                "19993.53"
            ],
            [
                "0.001",
                "19675.33"
            ],
            [
                "0.001",
                "19357.13"
            ]
        ],
        "asks": [
            [
                "0.001",
                "20629.92"
            ],
            [
                "0.001",
                "20948.12"
            ]
        ]
    },
    "message": null,
    "parameters": null,
    "status": 200
}
```
# Open API - Account Balance
    POST https://ace.io/polarisex/open/v2/coin/customerAccount
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| uid | INT | uid of ACE Exchange |
| currencyId | INT |  |
| currencyName | String |  |
| amount | String |  |
| cashAmount | String | available amount |

```json=
    {
      "attachment":[
        {
            "uid": 660,
            "currencyId": 2,
            "currencyName": "BTC",
            "amount": "9999999982.01768559",
            "cashAmount": "9999999982.01768559"
        }
      ],
      "status": 200,
      "message":null,
      "parameters":null
    }
```
# Open API - Kline/Candlestick data
    POST https://ace.io/polarisex/open/v2/kline/getKline
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| duration | INT | NO | please checkout duration enum|
| quoteCurrencyId | INT | YES |
| baseCurrencyId | INT | YES |
| startTime | Timestamp | NO |
| endTime | Timestamp | NO |
| limit | INT | NO | 1~2000, default is 2000 |


### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| current | String | current price |
| volume | String |  trade volume  of this line |
| changeRate | String | change rate of this line|
| highPrice | String |  high price of this line |
| lowPrice | String | low price of this line |
| openPrice | String | open price of this line |
| closePrice | String | close price of this line |
| currentTime | Int | current timestamp, the scale is second |
| createTime | Int | create time, date string format|

```json=
{
    "attachment": [
        {
            "current": "533501.38",
            "volume": "0",
            "changeRate": "0",
            "highPrice": "533501.38",
            "lowPrice": "533501.38",
            "openPrice": "533501.38",
            "closePrice": "533501.38",
            "currentTime": 1670359560000,
            "createTime": "2022-12-07 04:46:00"
        }
    ],
    "message": null,
    "parameters": null,
    "status": 200
}
```
# Open API - New Order
    POST https://ace.io/polarisex/open/v2/order/order

### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| baseCurrencyId | String | YES |
| quoteCurrencyId | String | YES |
| buyOrSell | INT | YES | 1. Buy 2. Sell |
| price | String | No | Only order type 1 need | 
| num | String | No | If order type  2 |
| amount | String | No | Only order type  3 and buy |
| type | INT | YES | 1. limit price order 2. num market order 3. amount market order |
| apiKey | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| orderNo | String | order number of created order |

```json=
{
    "attachment": {
        "orderNo": "16708172598046170050801100274092"
    },
    "message": null,
    "parameters": null,
    "status": 200
}
```
# Open API - Cancel Order
    POST https://ace.io/polarisex/open/v1/order/cancel
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
    if attachment is 200, it's success of  cancelling order.

```json=
    {
        "attachment": 200,
        "message": null,
        "parameters": null,
        "status": 200
    }

OR

    {
        "attachment": null,
        "message": "委託單已成交或已撤銷",
        "parameters": null,
        "status": 2061
    }
```

# Open API - Order List
    POST https://ace.io/polarisex/open/v2/order/getOrderList
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| baseCurrencyId | INT | NO |
| quoteCurrencyId | INT | NO |
| start | INT | NO | Default 1 |
| size | INT | NO | 1-500 Default 10 |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0. unsettled, 1. partial settled, 2. settled, 4. canceled, 5. partial canceled |
| buyOrSell | INT | 1. Buy, 2. Sell |
```json=
{
    "attachment": [
        {
            "uid": 660,
            "orderNo": "16697164905200431470851100284369",
            "orderTime": "2022-11-29 18:08:06",
            "orderTimeStamp": 1669716486980,
            "quoteCurrencyId": 1,
            "quoteCurrencyName": "TWD",
            "baseCurrencyId": 2,
            "baseCurrencyName": "BTC",
            "buyOrSell": "1",
            "num": "1.0000000000000000",
            "price": "490849.7500000000000000",
            "remainNum": "1.0000000000000000",
            "tradeNum": "0.0000000000000000",
            "tradePrice": "0.0000000000000000",
            "tradeAmount": "0.000000000000000000000000000000",
            "tradeRate": "0.00000000000000000000",
            "status": 0,
            "type": 1
        }
    ],
    "message": null,
    "parameters": null,
    "status": 200
}
```
# Open API - Order Status
    POST https://ace.io/polarisex/open/v2/order/showOrderStatus
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0. unsettled, 1. partial settled, 2. settled, 4. canceled, 5. partial canceled |
| buyOrSell | INT | 1. Buy, 2. Sell |
```json=
{
    "attachment": {
        "buyOrSell": 1,
        "averagePrice": "490849.75000000",
        "num": "0.00000000",
        "orderTime": "2022-11-29 18:03:06.318",
        "price": "490849.75000000",
        "status": 4,
        "tradeNum": "0.02697000",
        "remainNum": "0.97303000",
        "baseCurrencyId": 2,
        "baseCurrencyName": "BTC",
        "quoteCurrencyId": 1,
        "quoteCurrencyName": "TWD",
        "orderNo": "16697161898600391472461100244406"
    },
    "message": null,
    "parameters": null,
    "status": 200
}
```
# Open API - Order History
    POST https://ace.io/polarisex/open/v2/order/showOrderHistory
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0. unsettled, 1. partial settled, 2. settled, 4. canceled, 5. partial canceled |
| buyOrSell | INT | 1. Buy, 2. Sell |
```json=
{
    "attachment": {
        "order": {
            "buyOrSell": 1,
            "averagePrice": "491343.74000000",
            "num": "1.00000000",
            "orderTime": "2022-11-29 18:32:22.232",
            "price": "491343.74000000",
            "status": 1,
            "tradeNum": "0.01622200",
            "remainNum": "0.98377800",
            "baseCurrencyId": 2,
            "baseCurrencyName": "BTC",
            "quoteCurrencyId": 1,
            "quoteCurrencyName": "TWD",
            "orderNo": "16697179457740441472471100214402"
        },
        "trades": [
            {
                "price": "491343.74000000",
                "num": "0.01622200",
                "time": "2022-11-29 18:32:25.789",
                "tradeNo": "16697179457897791471461000223437",
                "amount": "7970.57815028"
            }
        ]
    },
    "message": null,
    "parameters": null,
    "status": 200
}
```

# Open API - Trade List
    POST https://ace.io/polarisex/open/v2/order/getTradeList
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| baseCurrencyId | INT | NO |
| quoteCurrencyId | INT | NO |
| buyOrSell | INT | NO | 1. Buy, 2. Sell |
| start | INT | NO | Default 1 |
| size | INT | NO | 1-500 Default 10 |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0. unsettled, 1. partial settled, 2. settled, 4. canceled, 5. partial canceled |
| buyOrSell | INT | 1. Buy, 2. Sell |
```json=
{
    "attachment": [
        {
            "buyOrSell": 1,
            "orderNo": "16708156853695560053601100247906",
            "num": "1",
            "price": "16895",
            "orderAmount": "16895",
            "tradeNum": "0.1",
            "tradePrice": "16895",
            "tradeAmount": "1689.5",
            "fee": "0",
            "feeSave": "0",
            "status": 1,
            "isSelf": false,
            "tradeNo": "16708186395087940051961000274150",
            "tradeTime": "2022-12-12 12:17:19",
            "tradeTimestamp": 1670818639508,
            "quoteCurrencyId": 14,
            "quoteCurrencyName": "USDT",
            "baseCurrencyId": 2,
            "baseCurrencyName": "BTC"
        }
    ],
    "message": null,
    "parameters": null,
    "status": 200
}
```
