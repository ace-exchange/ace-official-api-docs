
# Version 2 General API Information
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* For GET endpoints, parameters must be sent as a query string.
* You may mix parameters between both the query string and request body if you wish to do so.
* Parameters may be sent in any order.
* Two kinds of api:\
  a. 'Oapi' apis can use directly.\
  b. 'Open' apis require 'apiKey' and 'securityKey', can apply them in ACE web https://ace.io/.

# Signing API Requests
Important Note: Do not reveal your 'apiKey' and 'securityKey' to anyone. They are as important as your password.

To prevent the request(s) from being tempered in the process of network transmission, signature authentication is required for your API Key for the private interface, which guarantees that you are the source of the request(s). A legal ACE signature consists of parameters connected by “&”, and your api_secret, through `MD5` method. The signature needed to be placed in the parameter sign.

* Three parameters are required to be uploaded, including ACE_SIGN, timestamp and phone number, all are involved in signature expect forsign.
* Follow the rule to combine the parameters : `ACE_SIGN + timestamp + phone number`.
* If your `phone number` is `0886123456789`, and the current thirteen-digit `timestamp` is `1234567890000`, then your will get `ACE_SIGN12345678900000886123456789`.
* Signing the obtained string with the MD5 digest algorithm results: `ca2aa2ff46d309ee9abeee2951ae8d27`. Place this result into `signKey` parameter.


# ENUM definitions
### Kline/Candlestick chart intervals:
m -> minutes; h -> hours; d -> days; w -> weeks; M -> months
* 1m
* 5m
* 10m
* 30m
* 1h
* 2h
* 4h
* 8h
* 12h
* 1d
* 1w
* 1M
### Order side:
* BUY
* SELL
### Currency ID
* TWD＝1
* BTC＝2
* ETH＝4
* LTC＝7
* XRP＝10
* EOS＝11
* XLM＝12
* TRX＝13
* USDT＝14
* BNB＝17
* BTT＝19
* HWGC＝22
* GTO＝54
* USDC＝57
* MOT＝58
* UNI＝59
* MOS＝65
* MOCT＝66
* PT＝67
* DET＝70
* SOLO＝71
* QQQ＝72
* APT＝73
* HT＝74
* UNI＝75
* QTC＝76
* MCO＝79
* FTT＝81
* BAAS＝83
* OKB＝84
* DAI＝85
* MCC＝86
* TACEX＝87
* ACEX＝88
* LINK＝89
* DEC＝90
* FANSI＝91
* HWGC＝93
* KNC＝94
* COMP＝95
* DS＝96
* CRO＝97
* CREAM=101
* YFI=102
* WNXM=103
* MITH=104
* DEAC=105
* ENJ=107
* ANKR=108
* MANA=109
* SXP=110
* CHZ=111
* DOT=112
* CAKE=114
* SHIB=115
* DOGE=116
* MATIC=117
* WOO=119
* SLP=120
* AXS=121
* ADA=122
* QUICK=123
* FTM=124
* YGG=126
* GALA=127
* ILV=128
* DYDX=129
* SOL=130
* SAND=131
* AVAX=132
* LOOKS=133
* DEP=134
* APE=135
* GMT=136

# Oapi API - Trade Data
    GET https://ace.io/polarisex/oapi/v2/list/tradePrice
### Response:
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
    GET https://ace.io/polarisex/oapi/list/orderBooks/<baseCurrency_name>/<quoteCurrency_name>
### Response:
```json=
    {
        "market_pair":"BTC/TWD",
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
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |

### Response:
```json=
    {
      "attachment":[
        {
          "currencyId": 4,
          "amount": 6.896,
          "cashAmount": 6.3855,
          "uid": 123,
          "currencyName": "BTC"
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
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |
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
            "current": 490849.75,
            "volume": 0,
            "changeRate": 0,
            "highPrice": 490849.75,
            "lowPrice": 490849.75,
            "openPrice": 490849.75,
            "closePrice": 490849.75,
            "createTime": "2022-11-29 15:26:00",
            "currentTime": 1669706760000
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
| securityKey | STRING | YES |
| uid | STRING | YES |
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
    POST https://ace.io/polarisex/open/v2/order/cancel
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
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
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |
| baseCurrencyId | INT | NO |
| quoteCurrencyId | INT | NO |
| start | INT | NO | Default 1 |
| size | INT | NO | 1-100 Default 10 |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1 Buy; 2 Sell |
```json=
    {
      "attachment": [
        {
          "uid": 0,
          "orderNo": "16113081376560890227301101413941",
          "orderTime": "2021-01-22 17:35:37",
          "orderTimeStamp": 1611308137656,
          "baseCurrencyId": 2,
          "baseCurrencyName": "BTC",
          "quoteCurrencyId": 14,
          "quoteCurrencyName": "USDT",
          "buyOrSell": "1",
          "num": "6.0000000000000000",
          "price": "32.5880000000000000",
          "remainNum": "2.0000000000000000",
          "tradeNum": "4.0000000000000000",
          "tradePrice": "31.19800000000000000000",
          "tradeAmount": "124.7920000000000000",
          "tradeRate": "0.66666666666666666667",
          "status": 1,
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
| orderId | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1 Buy; 2 Sell |
```json=
    {
        "attachment": {
          "remainNum": "0.00000000",
          "orderNo": "15681910422154042100431100441305",
          "num": "0.85000000",
          "tradeNum": "0.85000000",
          "baseCurrencyId": 2,
          "baseCurrencyName": "BTC",
          "quoteCurrencyId": 14,
          "quoteCurrencyName": "USDT",
          "buyOrSell": 1,
          "orderTime": "2019-09-11 16:37:22.216",
          "price": "0.03096500",
          "averagePrice": "0.03096500",
          "status": 2
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
| orderId | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled 1 partial settled 2 settled 4. canceled 5. partial canceled |
| buyOrSell | INT | 1 Buy; 2 Sell |
```json=
    {
        "attachment": {
            "trades": [
                {
                    "amount": 0.0030965,
                    "tradeNo": "15681920522485652100751000417788",
                    "price": "0.03096500",
                    "num": "0.10000000",
                    "bi": 1,
                    "time": "2019-09-11 16:54:12.248"
                },
                {
                    "amount": 0.02322375,
                    "tradeNo": "15682679767395912100751000467937",
                    "price": "0.03096500",
                    "num": "0.75000000",
                    "bi": 1,
                    "time": "2019-09-12 13:59:36.739"
                }
            ],
            "order": {
                "remainNum": "0.00000000",
                "orderNo": "15681910422154042100431100441305",
                "num": "0.85000000",
                "tradeNum": "0.85000000",
                "baseCurrencyId": 2,
                "baseCurrencyName": "BTC",
                "quoteCurrencyId": 14,
                "quoteCurrencyName": "USDT",
                "buyOrSell": 1,
                "orderTime": "2019-09-11 16:37:22.216",
                "price": "0.03096500",
                "averagePrice": "0.03096500",
                "currencyId": 4,
                "status": 2
            }
        },
        "message": null,
        "parameters": null,
        "status": 200
    }
```
