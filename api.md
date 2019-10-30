
# General API Information
* The base endpoint is: https://www.ace.io/polarisex/
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* For GET endpoints, parameters must be sent as a query string.
* For POST, PUT, and DELETE endpoints, the parameters may be sent as a query string or in the request body with content type application/x-www-form-urlencoded. 
* You may mix parameters between both the query string and request body if you wish to do so.
* Parameters may be sent in any order.
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
* TWD=1
* BTC=2
* ETH＝4
* LTC＝7
* XRP＝10
* EOS＝11
* XLM＝12
* TRX＝13
* USDT＝14
* GTO=54
* MOT＝58
* BNB＝62
* MOCT=66
* PT=67
* SOX=68
* ERC20-USDT=69
* DET=70
* SOLO=71
* ACEX=88

# Kline/Candlestick data
    GET /quote/getKline

### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| tradeCurrencyId | INT | YES |
| limit | INT | YES |
| baseCurrencyId | INT | YES |
| type | INT | YES | 1 1m; 5 5m; 10 10m; 30 30m; 60 1h; 120 2h; 240 4h; 480 8h; 720 12h; 24 1d; 70 1w; 31 1M |
| endTime | LONG | NO |
| startTime | LONG | NO |

# New Order
    POST /polarisex/open/order/order

### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| baseCurrencyId | INT | YES |
| currencyId | INT | YES |
| buyOrSell | INT | YES | 1 Buy;2 Sell |
| price | INT | YES | 
| num | INT | YES |
| type | INT | YES | 1 limit order; 2 market order |
| apiKey | STRING | YES | 
| securityKey | STRING | YES | 
| uid | STRING | YES | 
| source | INT | YES | 1 web; 2 other |
| fdPassword | - | YES | Null |

### Response:

    {
        "attachment": "15697850529570392100421100482693",
        "message": null,
        "parameters": null,
        "status": 200
    }

# Cancel Order
    POST /polarisex/open/order/cancel
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES | 
| securityKey | STRING | YES | 
| uid | STRING | YES | 
| source | INT | YES | 1 web;2 other |
| fdPassword | - | YES | Null |
### Response:
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

# Order Status
    POST /polarisex/open/order/showOrderStatus
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES | 
| securityKey | STRING | YES | 
| uid | STRING | YES | 

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1=Buy;2=Sell | 
     {
        "attachment": {
            "remainNum": "0.00000000",
            "orderNo": "15681910422154042100431100441305",
            "num": "0.85000000",
            "tradeNum": "0.85000000",
            "baseCurrencyId": 2,
            "baseCurrencyName": "Bitcoin",
            "buyOrSell": 1,
            "orderTime": "2019-09-11 16:37:22.216",
            "currencyName": "Ethereum",
            "price": "0.03096500",
            "averagePrice": "0.03096500",
            "currencyId": 4,
            "status": 2 //  
        },
        "message": null,
        "parameters": null,
        "status": 200
    } 

# Order History
    POST /polarisex/open/order/showOrderHistory
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderId | STRING | YES |
| apiKey | STRING | YES | 
| securityKey | STRING | YES | 
| uid | STRING | YES | 

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1=Buy;2=Sell | 

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
                "baseCurrencyName": "Bitcoin",
                "buyOrSell": 1,
                "orderTime": "2019-09-11 16:37:22.216",
                "currencyName": "Ethereum",
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






