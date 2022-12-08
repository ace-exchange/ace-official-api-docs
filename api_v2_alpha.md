
# Version 2 General API Information (still on development)
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* For GET endpoints, parameters must be sent as .
* For Post endpoints, parameters format is  x-www-form-urlencoded.
* Parameters may be sent in any order.
* Two kinds of api:\
  a. 'Oapi' apis can use directly.\
  b. 'Open' apis require `apiKey` and `secretKey`, can apply them in ACE web `https://ace.io/`.

# Signing API Requests
Important Note: Do not reveal your `apiKey` and `secretKey` to anyone. They are as important as your password.

To prevent the request(s) from being tempered in the process of network transmission, signature authentication is required for your apiKey for the private interface, which guarantees that you are the source of the request(s).And your secretKey, through `SHA256` method. The signature needed to be placed in the parameter sign.

* Three parameters are required to be uploaded, including ACE_SIGN, timestamp and phone number, all are involved in signature expect forsign.
* Follow the rule to combine the parameters : `ACE_SIGN  + Secret Key + parameters values`.
* If  `Secret Key` is `xxxxxx`, then your will get `ACE_SIGNxxxxxx`.
* Then if you hava parameters like: `apiKey=AAA&timeStamp=12121212&quoteCurrencyId=1&baseCurrencyId=2`, only get the values by definition order to get `AAA1212121212`.
* Combine `ACE_SIGNxxxxxx` + `AAA1212121212` to get `ACE_SIGNxxxxxxAAA1212121212`
* Signing the obtained string with the `SHA256` algorithm results: `e265a45639d473add583fe58d448fa516550f7cb9428284c0909cfd3380dbed8`. 
* Place this result into `signKey` parameter.



# ENUM definitions
### Kline/Candlestick chart intervals:
m -> minutes; h -> hours; d -> days; w -> weeks; M -> months
* 1m

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

```json=
    symbol, format:<baseCurrencyName>/<quoteCurrencyName>

```
 
Value:
| Name | Type | Description |
| ---- | ---- | ---- |
| base_volume | String | 24hr volume |
| last_price | String |  |
| quote_volume | String | turnover |

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
| Name | Type | Description |
| ---- | ---- | ---- |
| symbol | String|  |
| base | String| gmt/usdt, base is btc |
| quote | String| gmt/usdt, quote is gmt |
| basePrecision | String|  |
| quotePrecision | String|  |
| minLimitBaseAmount | String|  |
| maxLimitBaseAmount | String|  |
| minMarketBuyQuoteAmount | String|  |
| maintain | Boolean| under maintainence or not |
| orderBookQuotePrecision | String|  |

```json=
    {
     "symbol":"BTC/USDT","base":"gmt","quote":"usdt","basePrecision":"8",
     "quotePrecision":"5","minLimitBaseAmount":"0.1","maxLimitBaseAmount":"480286",
     "minMarketBuyQuoteAmount":"10","maintain":false,
     "orderBookQuotePrecision":"5"
    }
```
# Oapi API - Order Books
    GET https://ace.io/polarisex/open/v2/public/list/orderBooks/<baseCurrency_name>/<quoteCurrency_name>
### Response:
```json=
    {
        "symbol":"BTC/TWD",
        "quoteCurrencyId": 1,
        "baseCurrencyId":2,
        "orderbook": {
          "asks": [
            [
                "0.449612",
                "1800000"
            ],
            [
                "0.001",
                "1980000"
            ]
          ],
          "bids": [
            [
                "0.017087",
                "1165121.4"
            ],
            [
                "0.01",
                "1165121.2"
            ]
          ]
        }
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
    POST https://ace.io/polarisex/open/v2/kline/getKlineMin
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| quoteCurrencyId | INT | YES |
| baseCurrencyId | INT | YES |
| startTime | Timestamp | NO |
| endTime | Timestamp | NO |
| limit | INT | NO | 1~2000 |

### Response:
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
```json=
    {
        "attachment": "15697850529570392100421100482693",
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
