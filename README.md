# bytetrade-official-api-docs

-Welcome to the bytetrade-official-api-docs wiki!
to [wiki](https://github.com/Bytetrade/bytetrade-official-api-docs/wiki)


## API List
| Content Type | Request Method | Request Type    | Description  | Signature Required |
| ------------ | ----- | ------ | ----- | ----- |
| Symbols&Currency        | [GET /symbols](#get-symbols-get-all-the-trading-symbols-in-bytetradecom)  | GET | Reference information of trading instrument, including base currency, quote precision, etc.  |N |
| Symbols&Currency        | [GET /currencies](#get-currencies-get-currencys-supported-in-bytetradecom)  | GET | The list of currencies available  |N |
| Market Data       | [GET /tickers](#get-tickers)  | GET | Trade summary of a trading day for all symbols or symbol | N |
| Market Data       | [GET /depth](#get-depth-get-market-depth)  | GET | Market Depth of a symbol  | N |
| Market Data       | [GET /klines](#get-klines-get-candlestick-data)   | GET | Kline of a symbol   | N |
| Market Data       | [GET /trades](#get-trades-get-trade-records)  | GET | Trade records of a symbol| N |
|Account	|[GET /balance/](#get-balance-get-balance-in-bytetradecom)|	GET| Get the balance of an account| Y|
|User Order Info	|[GET /orders/all](#get-ordersall-get-open-and-closed-orders-for-an-account)|GET|Get the open and closed orders of an account|N|
|User Order Info	|[GET /orders/open](#get-ordersopen-get-open-orders-for-an-account)|GET|Get the open orders of an account|N|
|User Order Info	|[GET /orders/closed](#get-ordersclosed-get-closed-orders-for-an-account)	|GET|Get the closed orders of an account|N|
|User Order Info	|[GET /orders/{id}](#get-ordersid-get-order-info)	|GET|Get the details of an order of an account	|N|
|User Order Info	|[GET /orders/trades](#get-orderstrades-get-detail-match-results)	|GET|Get detail match results	|N|
|Withdraw/Deposit/Transfer Info	|[GET /depositaddress](#get-depositaddress-get-deposit-address)	|GET|Get deposit address |N|
|Withdraw/Deposit/Transfer Info	|[GET /withdrawals](#get-withdrawals-get-withdrawals-info)	|GET|Get withdrawals info|	N|
|Withdraw/Deposit/Transfer Info	|[GET /deposits](#get-deposits-get-deposit-info)|GET| 	Get deposit info|N|
|Withdraw/Deposit/Transfer Info |[GET /transfers](#get-transfers-get-transfer-info)	|GET|Get transfer info|N|
|Trade	|[POST /transaction/createorder](#post-transactioncreateorder-place-an-order	)|POST|	Place an order	|Y|
|Trade	|[POST /transaction/cancelorder](#post-transactioncancelorder-Cancel-an-order)|POST|	Cancel an order	|Y|
|Withdraw	|[POST /transaction/withdraw](#post-transactionwithdraw-create-a-withdraw-application)	|POST|Create a whitdraw application	|Y|
|Transfer	|[POST /transaction/transfer](#post-transactiontransfer-create-a-transfer-application)	|POST|Create a transfer application|Y|

####  endpoint
* mainchain https://api-v2.byte-trade.com
* testchain https://api-v2-test.byte-trade.com

#### Other instructions
ByteTrade Decentralized Exchange has been [CCXT](https://github.com/ccxt/ccxt) Certified, so you can use the CCXT interface to query and trade on ByteTrade. When initializing the ByteTrade instance of CCXT, the incoming apiKey parameter is the user's username in ByteTrade, and the secret parameter is the user's private key.

####  GET /symbols Get all the trading symbols in byte-trade.com

 Request:
None

Response:
```
symbol list
```

* [bytetrade market structure](https://api-v2.byte-trade.com/symbols)
* [testnet bytetrade market structure](https://api-v2-test.byte-trade.com/symbols)

 Example:
 

```JavaScript
[
    {
      "symbol": "68719476706",     // market symbol, unique
      "name": "ETH/BTC",     // market symbol name, unique
      "base": "2",             // base coin currency
      "quote": "32",           // quote coin currency
      "baseName": "ETH",     // base coin code
      "quoteName": "BTC",    // quote coin code
      "active": true,
      "maker": "0.0008",     // maker fee
      "taker": "0.0008",     // taker fee
      "precision": {         // number of decimal digits "after the dot"
        "amount": 8,        
        "price": 10          
      },
      "limits": {
        "amount": {
          "min": "0.00000001", 
          "max": "-1"        // -1 mean inf, no limit
        },
        "price": {
          "min": "0.0000000001", 
          "max": "-1"            // -1 mean inf, no limit
        }
      }
    }
]
```




####  GET /currencies Get currencys supported in byte-trade.com

 Request:

None

#### Other instructions
1. Order creation, order cancellation, inter-account transfer, will charge 0.0003 BTT per transaction as package fee;
2. After the order is combined, the trade fee rate is 0.08%.
 

 Response:

```
currency list
```

* [bytetrade currency structure](https://api-v2.byte-trade.com/currencies)
* [testnet bytetrade currency structure](https://api-v2-test.byte-trade.com/currencies)

Example:


```JavaScript
[
      {
        "code": "32",         // currency id, unique
        "name": "BTC",      // currency name, unique
        "type": "crypto",
        "fullname": "Bitcoin",
        "active": true,
        "basePrecision": 18,      // in ByteTrade chain, 1 BTC is expressed as 1000000000000000000 integer
        "transferPrecision": 10,  // when transfer in in ByteTrade chain,  the minimum is 0.0000000001
        "externalPrecision": 8,   // in BTC chain, the minimum is 0.00000001
        "fee":"53004",            // withdraw fee, only valid for BTC
        "chainType":"bitcoin",   
        "chainContractAddress": "",
        "limits": {
            "deposit": {
              "min": "0.001",    
              "max": "-1"        // -1 mean inf, no limit
             },
            "withdraw": {
              "min": "0.01",     
              "max": "-1"        // -1 mean inf, no limit
            }
        }
      }
]
```





#### GET /tickers
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| symbol     | false  | string | symbol    |       | 68719476706, 4294967297, ... |

Response:

```
ticker list
```

* [bytetrade ticker structure](https://api-v2.byte-trade.com/tickers)
* [testnet bytetrade ticker structure](https://api-v2-test.byte-trade.com/tickers)

Example:
```JavaScript
[
      {
        "symbol": "68719476706",                 // unique id, unique
        "name": "ETH/BTC",                       // string symbol of the market, unique
        "base": "2",                             // base coin id
        "quote": "32",                           // quote coin id
        "timestamp": 1559124034283,              // int (64-bit Unix Timestamp in milliseconds since Epoch 1 Jan 1970)
        "datetime": "2019-05-29T10:00:34.283Z",  // ISO8601 datetime string with milliseconds
        "high": "0.031526",                        // highest price
        "low": "0.030771",                         // lowest price
        "open": "0.031009",                        // opening price
        "close": "0.031035",                       // price of last trade (closing price for current period)
        "last": "0.031035",                        // same as `close`, duplicated for convenience
        "change": "2.6e-05",                       // absolute change, `last - open`
        "percentage": "0.084",                     // relative change, `(change/open) * 100`
        "baseVolume": "209771.771",                // volume of base currency traded for last 24 hours
        "quoteVolume": "6519.97393184"             // volume of quote currency traded for last 24 hours
      }
]
```



#### GET /depth Get Market Depth
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| symbol     | true  | string | symbol    |       | 68719476706, 4294967297, ... |
| limit      | false  | int | asks and bids size    |        |[1,100]|
| type       | false  | string |Depth data type,step0 mean No market depth aggregation, step1\2\3\4\5 mean aggregation precision is symbol-price-precision*10\100\1000\10000\100000|   step0    | step0,step1,step2,step3,step4,step5 |

Response:
```
depth dict
```
* [bytetrade orderbook structure](https://api-v2.byte-trade.com/depth?symbol=68719476706)
* [testnet bytetrade orderbook structure](https://api-v2-test.byte-trade.com/depth?symbol=47244640236)

Example:
```JavaScript
{
      "bids": [
        [
          "0.031138",   // price
          "0.05"        // amount
        ],
        [
          "0.031137",
          "1.94"
        ],
        [
          "0.031136",
          "0.236"
        ]
       ],
      "asks": [
        [
          "0.031147",
          "14.237"
        ],
        [
          "0.031149",
          "0.033"
        ],
        [
          "0.03115",
          "0.417"
        ],
        [
          "0.031151",
          "0.755"
        ],
    "timestamp": 1559549045008,
    "datetime": "2019-06-03T08:04:05.008Z"
}
```



#### GET /klines Get Candlestick Data
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| symbol     | true  | string | symbol    |       | 68719476706, 4294967297, ... |
| timeframe  | true  | string | kline data type    |        | 1m, 5m,15m,30m,1h,4h,1d,5d,1w,1M |
| since      | false  | int | timestamp in ms to get klines, If since is not sent, the most recent klines will be returned.|      ||
| limit      | false  | int | kline data size    |  100      |[1,500]|

Response:
```
kline list
```
* [bytetrade kline structure](https://api-v2.byte-trade.com/klines?symbol=68719476706&timeframe=1h&limit=500)
* [testnet bytetrade kline structure](https://api-v2-test.byte-trade.com/klines?symbol=47244640236&timeframe=1h&limit=500)

Example:

```JavaScript
[
    [
      1559574540000,      // UTC timestamp in milliseconds, integer
      "0.030753",         // (O)pen price, String
      "0.030778",         // (H)ighest price
      "0.030752",         // (L)owest price
      "0.030778",         // (C)losing price
      "30.716"            // (V)olume (in terms of the base currency)
    ],
    [
      1559574600000,
      "0.030778",
      "0.030789",
      "0.030775",
      "0.030775",
      "86.1294"
    ]
]
```




#### GET /trades Get Trade records
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| symbol     | true  | string | symbol    |       | 68719476706, 4294967297, ... |
| since      | false  | int | timestamp in ms to get trades, If since is not sent, the most recent trades will be returned.|        ||
| limit      | false  | int | trades data size    |  100      |[1,500]|

Response:
```
trade list
```

* [bytetrade trades structure](https://api-v2.byte-trade.com/trades?symbol=68719476706&limit=500)
* [testnet bytetrade trades structure](https://api-v2-test.byte-trade.com/trades?symbol=47244640236&limit=500)

Example:
```JavaScript
[
    {
      "id": "6863a873ed87443bfc8bd759451d5c9ec4a2dafc",          // string trade id
      "txid":"909108605661dfd3e6d85ae2a9faceb524dce733",         // transaction id in bytetrade
      "order": "e0bfe265bab9ac017a867eceff9f5b3464053972",     // sring order id
      "timestamp": 1559633809458,               // Unix timestamp in milliseconds
      "datetime": "2019-06-04T07:36:49.458Z",   // ISO8601 datetime with milliseconds
      "symbol": "68719476706",                    // symbol
      "name": "ETH/BTC",                        // symbol name
      "side": "buy",                            // direction of the trade, "buy" or "sell"
      "price": "0.031118",                      // price in quote currency
      "amount": "0.2379",                       // amount of base currency
      "cost": "0.0074029722"                    // amount of quote currency
    }
]
```


#### GET /balance Get balance in byte-trade.com
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, ... |


Response:
```
balance list
```

* [bytetrade balance structure](https://api-v2.byte-trade.com/balance?userid=bytetrade_token_swaper)
* [testnet bytetrade balance structure](https://api-v2-test.byte-trade.com/balance?userid=transfer_testnet)

Example:
```JavaScript
[
    {
      "code": "32",                              // string coin id
      "name": "BTC",                           // string coin name
      "free": "0.23",                          // money available for trading
      "used": "0.03",                          // money on hold, locked, frozen or pending
      "total": "0.26"                          // total balance (free + used)
    }
]
```


#### GET /orders/all Get open and closed orders for an account
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| symbol     | false  | string | symbol    |       | 68719476706, 4294967297, ... |
| since      | false  | int | timestamp in ms to get open and closed order, If since is not sent, the most recent order will be returned.|        ||
| limit      | false  | int | order data size    |  100      |[1,100]|


Response:
```
open order list
```

* [bytetrade_allorder structure](https://api-v2.byte-trade.com/orders/all?userid=bytetrade_token_swaper)
* [testnet bytetrade allorder structure](https://api-v2-test.byte-trade.com/orders/all?userid=transfer_testnet)

Example:
```JavaScript
[
    {
        "id":                "12345-67890:09876/54321", // string
        "txid":              "42d225b12d97441e709c741e1f99b377ec3a3822"   // tx_id in bytetrade
        "datetime":          "2017-08-17 12:42:48.000", // ISO8601 datetime of "timestamp" with milliseconds
        "timestamp":          1502962946216, // order placing/opening Unix timestamp in milliseconds
        "lastTradeTimestamp": 1502962956216, // Unix timestamp of the most recent trade on this order
        "status":     "open",         // "open"
        "symbol":     "68719476706",  // symbol Id
        "name":       "ETH/BTC",      // symbol name
        "type":       "limit",        // "market", "limit"
        "side":       "buy",          // "buy", "sell"
        "price":       "0.029",       // float price in quote currency
        "average":     "0",
        "amount":      "0.000294",    // ordered amount of base currency
        "filled":      "0",           // filled amount of base currency
        "remaining":   "0.000294",    // remaining amount to fill
        "cost":       "0",            // "filled" * "price" (filling price used where available)
        "fee": {                      // fee info, if available
            "code": "32",             // which currency the fee is (usually quote)
            "name": "BTC",
            "cost": "0",             // the fee amount in that currency
            "rate": "0",             // the fee rate (if available)
        }
]
```


#### GET /orders/open Get open orders for an account
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| symbol     | false  | string | symbol    |       | 68719476706, 4294967297, ... |
| since      | false  | int | timestamp in ms to get open order, If since is not sent, the most recent open order will be returned.|        ||
| limit      | false  | int | openorder data size    |  100      |[1,100]|


Response:
```
open order list
```

* [bytetrade openorder structure](https://api-v2.byte-trade.com/orders/open?userid=bytetrade_token_swaper)
* [testnet bytetrade openorder structure](https://api-v2-test.byte-trade.com/orders/open?userid=transfer_testnet)

Example:
```JavaScript
[
    {
        "id":                "12345-67890:09876/54321", // string
        "txid":              "42d225b12d97441e709c741e1f99b377ec3a3822"   // tx_id in bytetrade
        "datetime":          "2017-08-17 12:42:48.000", // ISO8601 datetime of "timestamp" with milliseconds
        "timestamp":          1502962946216, // order placing/opening Unix timestamp in milliseconds
        "lastTradeTimestamp": 1502962956216, // Unix timestamp of the most recent trade on this order
        "status":     "open",         // "open"
        "symbol":     "68719476706",  // symbol Id
        "name":       "ETH/BTC",      // symbol name
        "type":       "limit",        // "market", "limit"
        "side":       "buy",          // "buy", "sell"
        "price":       "0.029",       // float price in quote currency
        "average":     "0",
        "amount":      "0.000294",    // ordered amount of base currency
        "filled":      "0",           // filled amount of base currency
        "remaining":   "0.000294",    // remaining amount to fill
        "cost":       "0",            // "filled" * "price" (filling price used where available)
        "fee": {                      // fee info, if available
            "code": "32",             // which currency the fee is (usually quote)
            "name": "BTC",
            "cost": "0",             // the fee amount in that currency
            "rate": "0",             // the fee rate (if available)
        }
]
```



#### GET /orders/closed Get closed orders for an account
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| symbol     | false  | string | symbol    |       | 68719476706, 4294967297, ... |
| since      | false  | int | timestamp in ms to get closed order, If since is not sent, the most recent closed order will be returned.|       ||
| limit      | false  | int | closedorder data size    |  100      |[1,100]|


Response:
```
closed order list
```

* [bytetrade closedorder structure](https://api-v2.byte-trade.com/orders/closed?userid=bytetrade_token_swaper)
* [testnet bytetrade closedorder structure](https://api-v2-test.byte-trade.com/orders/closed?userid=transfer_testnet)

Example:
```JavaScript
[
    {
        "id":                "a7922889b77005bae42b48cd15db3849a1e89d4b", // string
        "txid":              "42d225b12d97441e709c741e1f99b377ec3a3822",   // tx_id in bytetrade
        "datetime":          "2017-08-17 12:42:48.000", // ISO8601 datetime of "timestamp" with milliseconds
        "timestamp":          1502962946216, // order placing/opening Unix timestamp in milliseconds
        "lastTradeTimestamp": 1502962956216, // Unix timestamp of the most recent trade on this order
        "status":     "closed",             // "closed"
        "symbol":     "68719476706",          // symbol Id
        "name":       "ETH/BTC",            // symbol name
        "type":       "limit",              // "market", "limit"
        "side":       "buy",                // "buy", "sell"
        "price":       "0.03413801",        // float price in quote currency
        "average":     "0.03413801",        // money/filled
        "amount":      "0.001",             // ordered amount of base currency
        "filled":      "0.001",             // filled amount of base currency
        "remaining":   "0",                 // remaining amount to fill
        "cost":       "0.000034138432",     // "filled" * "price" (filling price used where available)
        "fee": {                            // fee info, if available
            "code": "32",                     // which currency the fee is (usually quote)
            "name": "BTC",
            "cost": "0",                    // the fee amount in that currency
            "rate": "0",                    // the fee rate (if available)
        }
    }
]
```



#### GET /orders/{id} Get Order Info
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| orderid     | true  | string | orderid    |       | a7922889b77005bae42b48cd15db3849a1e89d4b, , ... |



Response:
```
order info dict
```

* [bytetrade orderinfo structure](https://api-v2.byte-trade.com/orders/2a42c54249f1fd27987848fbb5117b8b4b597235)
* [testnet bytetrade orderinfo structure](https://api-v2-test.byte-trade.com/orders/2a6922304a976df6ac49fe2780a87d69ac674266)

Example:
```JavaScript
{
    "id":                "a7922889b77005bae42b48cd15db3849a1e89d4b", // string
    "txid":              "909108605661dfd3e6d85ae2a9faceb524dce733",   // tx_id in bytetrade
    "datetime":          "2017-08-17 12:42:48.000", // ISO8601 datetime of "timestamp" with milliseconds
    "timestamp":          1502962946216, // order placing/opening Unix timestamp in milliseconds
    "lastTradeTimestamp": 1502962956216, // Unix timestamp of the most recent trade on this order
    "status":     "closed",             // "open", "closed",  如果一个订单没有成交,而就被取消掉,通过此接口不会查询到 
    "symbol":     "68719476706",          // symbol Id
    "name":       "ETH/BTC",            // symbol name
    "type":       "limit",              // "market", "limit"
    "side":       "buy",                // "buy", "sell"
    "price":       "0.03413801",        // float price in quote currency
    "amount":      "0.001",             // ordered amount of base currency
    "filled":      "0.001",             // filled amount of base currency
    "remaining":   "0",                 // remaining amount to fill
    "cost":       "0.000034138432",     // "filled" * "price" (filling price used where available)
    "fee": {                            // fee info, if available
        "code": "32",                     // which currency the fee is (usually quote)
        "name": "BTC",
        "cost": "0",                    // the fee amount in that currency
        "rate": "0",                    // the fee rate (if available)
    }
}
```





#### GET /orders/trades Get detail match results
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| symbol     | false  | string | symbol    |       | 68719476706, 4294967297, ... |
| orderid    | false  | string | orderid    |       | a7922889b77005bae42b48cd15db3849a1e89d4b, , ... |
| since      | false  | int | timestamp in ms to get trades info, If since is not sent, the most recent trades info will be returned.|        ||
| limit      | false  | int | trade data size    |  100      |[1,100]|



Response:
```
matchresults info list
```
* [bytetrade history trades detail structure](https://api-v2.byte-trade.com/orders/trades?userid=bytetrade_token_swaper)
* [testnet bytetrade history trades detail structure](https://api-v2-test.byte-trade.com/orders/trades?userid=transfer_testnet)

Example:
```JavaScript
[
    {
        "id":           "3b185509047d385381fa1ec4d975ebb0d41c97c3",  // string trade id
        "txid":         "909108605661dfd3e6d85ae2a9faceb524dce733",  // transaction id in bytetrade
        "timestamp":    1502962946216,                               // Unix timestamp in milliseconds
        "datetime":     "2017-08-17 12:42:48.000",                   // ISO8601 datetime with milliseconds
        "symbol":       "4294967297",                                  // symbol
        "name":         "ETH/BTC",                                   // symbol name
        "order":        "d891f26f8cbc08e27434eb9ab9fb937ee1e7e438",  // string order id or undefined/None/null
        "side":         "sell",                                      // direction of the trade, "buy" or "sell"
        "takerOrMaker": "taker",                                     // string, "taker" or "maker"
        "price":        "0.00007412",                                // float price in quote currency
        "amount":       "27.29041663",                               // amount of base currency
        "cost":         "0.0020227656806156",                        // total cost (including fees), `price * amount`
        "fee":          {                                            // provided by exchange or calculated by ccxt
            "cost":  "0.0000016182125445",                           // float
            "code": "32",                                               // usually base currency for buys, quote currency for sells
            "name": "BTC",
            "rate": 0.002,                                           // the fee rate (if available)
        }
    }
]
```



#### GET /depositaddress Get deposit address
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| code     | false  | string | currency code    |       | 2, 3, ... |


Response:
```
deposit address
```
* [bytetrade depositaddress structure](https://api-v2.byte-trade.com/depositaddress?userid=gogogo)
* [testnet bytetrade depositaddress structure](https://api-v2-test.byte-trade.com/depositaddress?userid=transfer_testnet)

Example:
```JavaScript
[
    {
        "code": "2",                                                 // currency code
        "name": "ETH",
        "chainType":"ethereum",                                    // ethereum/naka/cmt/bitcoin
        "address": "0x10c03cde1395e8e1e7626b890384c9897f7f597b",   // address in terms of requested currency
        "tag": ""                                                  // tag / memo / paymentId for particular currencies (XRP, XMR, ...)
    }
]
```




#### GET /withdrawals Get withdrawals info
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| code     | false  | string | currency code    |       | 2, 3, ... |
| since      | false  | int | timestamp in ms to get withdrawal info, If since is not sent, the most recent withdrawal info will be returned.|        ||
| limit      | false  | int | withdraw data size    |  100      |[1,100]|


Response:
```
withdrawal list
```
* [bytetrade withdraw structure](https://api-v2.byte-trade.com/withdrawals?userid=gogogo)
* [testnet bytetrade withdraw structure](https://api-v2-test.byte-trade.com/withdrawals?userid=harvey1340)

Example:
```JavaScript
[
    {
      "id": "e523e01b778e377c9bbc3a883305409c3774efb1",                             
      "txid": "0xc42f1611d795cb5a9bda63af4a9fd28c7958a18d908bf1750ccfea9ace88d48f",                                                                   // 对应外部链上的转账信息,当前从listWithdraws没有找到这个,但是从区块浏览器中可以看到,需要确认下如何获取
      "timestamp": 1553134089103,                                                  
      "datetime": "2019-03-21T02:08:09.103Z",                                      
      "address": "0x32d74896f05204d1b6ae7b0a3cebd7fc0cd8f9c7",                   
      "tag": "",                                                                  
      "type": "withdrawal",                         
      "amount": 10,                                       
      "code": "3",     
      "name": "KCASH",                                                         
      "status": "succeed",                              
      "updated": 1553134584448,                          
      "fee": {                                   
        "code": "3",    
        "name": "KCASH",                      
        "cost": "0",                            
        "rate": "0"
      }
    }
]
```



#### GET /deposits Get deposit info
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| code     | false  | string | currency code    |       | 2, 3, ... |
| since      | false  | int | timestamp in ms to get deposit info, If since is not sent, the most recent deposit info will be returned.|  0      ||
| limit      | false  | int | deposit data size    |  100      |[1,100]|


Response:
```
deposit list
```
* [bytetrade deposit structure](https://api-v2.byte-trade.com/deposits?userid=gogogo)
* [testnet bytetrade deposit structure](https://api-v2-test.byte-trade.com/deposits?userid=harvey1340)

Example:
```JavaScript
[
    {
      "id": "7b52aeb2d8f624d16ef0473defdd8d39db057c02",                             
      "txid": "0xdfa9724b0f269a2b2eecd952c6b369320febce047938772fff6f129322bf632c",                                                             // 对应外部链上的转账信息,当前从listWithdraws没有找到这个,但是从区块浏览器中可以看到,需要确认下如何获取
      "timestamp": 1553134089103,                                                  
      "datetime": "2019-03-21T02:08:09.103Z",                       
      "address": "0x10c03cde1395e8e1e7626b890384c9897f7f597b",              
      "tag": "",                                                                  
      "type": "deposit",                         
      "amount": "0.01030928",                             
      "code": "2",   
      "name":"ETH",                                                                                               
      "status": "succeed",                              
      "updated": 1553134584448,                        
      "fee": {                                           
        "code": "2",    
        "name": "ETH",                       
        "cost": "0",                            
        "rate": "0"
      }
    }
]
```




#### GET /transfers Get transfer info
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| userid     | true  | string | userid    |       | Aquila, , ... |
| code     | false  | string | currency code    |       | 2, 3, ... |
| since      | false  | int | timestamp in ms to get transfer info, If since is not sent, the most recent transfer info will be returned.  |        ||
| limit      | false  | int | transfer data size    |  100      |[1,100]|


Response:
```
transfer list
```
* [bytetrade transfer structure](https://api-v2.byte-trade.com/transfers?userid=bytetrade_token_swaper)
* [testnet bytetrade transfer structure](https://api-v2-test.byte-trade.com/transfers?userid=transfer_testnet)

Example:
```JavaScript
[
    {
      "id": "8dd700fde3e0f947e4b52d72950a9fb3261c59d1",                    
      "timestamp": 1553134089103,                                           
      "datetime": "2019-03-21T02:08:09.103Z",                        
      "tag": "",                                                                 
      "type": "transfer",                         
      "amount": "-5",                                                             
      "to":"fly2020",                                                           
      "from":"gogogo",                                                            
      "code": "34",                                                         
      "name": "MT",                                           
      "status": "succeed",                                                 
      "updated": 1553134584448                                               
    }
]
```



#### POST /transaction/createorder Place an order
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| trObj     | true  | string | Transaction with serialized signature information |       |  |

| Transaction Param   | required  | data type      | description    | default   | value range    |
| ------  | ----- | ------ | ------  | ----- | -------  |
| fee     | true  | uint128 | The amount of packing fee BTT required to create a transaction, expressed in an integer format on the chain   |  300000000000000     |  |
| creator     | true  | string | username   |       |  |
| side     | true  | uint8 | order direction    |       | 1:sell, 2:buy |
| order_type     | true  | uint8 | order type    |       | 1:limitOrder,2:marketOrder |
| market_name     | true  | string | market name, corresponding to the name field in the symbols interface    |       |  |
| amount     | true  | uint128 | the number of the currency traded, expressed in an integer format on the chain     |       |  |
| price     | true  | uint128 | the price of the currency of the transaction, expressed in an integer format on the chain    |       |  |
| now     | true  | string | current time, format is %Y-%m-%dT%H:%M:%S    |       |  |
| expiration     | true  | string | expiration time, format is %Y-%m-%dT%H:%M:%S    |       |  |
| use_btt_as_fee     | true  | bool | whether to use BTT as a transaction fee    |  false     |  |
| freeze_btt_fee     | false  | uint128 | the number of BTT that need to be frozen when using BTT as a transaction fee    |       |  |
| custom_no_btt_fee_rate     | false  | uint128 | BTT transaction rate when using BTT as transaction fee    |       |  |
| money_id     | true  | uint32 | quote currency, corresponding to the quote field in the symbols interface     |       | 1, 2, 3, ...  |
| stock_id     | true  | uint32 | base currency, corresponding to the base field in the symbols interface     |       | 1, 2, 3, ...   |

#### Other instructions
1. After the order is submitted to the chain, the signature verification will be performed first. After the verification is correct, the message of successful submission is returned (note that this does not represent that the order was successfully created). If the signature verification fails, the verification failure message is returned;
2. Before the order is successfully created, the price accuracy, quantity accuracy, and account balance will be checked. If the check is passed, the creation can be successful.
3. When the order is submitted, the orderid of this order has been determined. The order can be queried through the relevant interface of orders. If the query does not contain this order, the order is not created successfully.

**Example:**

```javascript
var bytetrade_js = require('../utils/bytetrade.min.js');
var test_userid='harvey1340';
var test_privatekey='682503fc0582ff940c7fe97ef48b49d18421e5668af5811c0b3f74cf21f33f54';
var dapp_name = 'Sagittarius';

function create_order() {
    var tr = new bytetrade_js.TransactionBuilder();
    var ob = {
        fee: '300000000000000',   //0.0003 *1000000000000000000  fee
        creator: test_userid,     // creator userid
        side: 2,                  // 1:sell, 2:buy
        order_type: 1,            // 1:limitOrder,2:marketOrder
        market_name: "KCASH/ETH",
        amount: '4310000000000000', // amount, which is represented as an integer on the chain, amount*10^basePrecision;
        price: '83210000000000',    // price, which is represented as an integer on the chain, price*10^basePrecision;
        now: Math.ceil(Date.now() / 1000),
        expiration: Math.ceil(Date.now() / 1000) + 10,
        use_btt_as_fee: false,
        freeze_btt_fee: 0,
        custom_no_btt_fee_rate: 8,
        money_id: 2,   // quote currency
        stock_id: 3    // base currency
    }

    tr.add_type_operation("order_create", ob);
    tr.timestamp = Math.ceil(Date.now() / 1000);
    tr.dapp=dapp_name;
    tr.validate_type = 0;
    tr.add_signer(bytetrade_js.PrivateKey.fromHex(test_privatekey));
    tr.finalize();
    if (!tr.signed) { tr.sign(); }
    var trObj = tr.toObject();
    console.log("id " + tr.id());
    signedTransaction = JSON.stringify(trObj);
    const request = {
        'trObj': signedTransaction,
    };
    // next step is POST request to /transaction/createorder endpoint
}
```

#### POST /transaction/cancelorder Cancel an order
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| trObj     | true  | string | Transaction with serialized signature information  |       |  |

| Transaction Param   | required  | data type      | description    | default   | value range    |
| ------  | ----- | ------ | ------  | ----- | -------  |
| fee     | true  | uint128 | The amount of packing fee BTT required to create a transaction, expressed in an integer format on the chain   |  300000000000000     |  |
| creator     | true  | string | username    |       |  |
| market_name     | true  | string | market name, corresponding to the name field in the symbols interface    |       |  |
| order_id     | true  | uint128 | order id, corresponding to the id field in the orders/open interface   |       |  |
| money_id     | true  | uint32 | quote currency, corresponding to the quote field in the symbols interface   |       | 1, 2, 3, ...  |
| stock_id     | true  | uint32 | base currency, corresponding to the base field in the symbols interface    |       | 1, 2, 3, ...   |

**Example:**

```javascript
var bytetrade_js = require('../utils/bytetrade.min.js');
var test_userid='harvey1340';
var test_privatekey='682503fc0582ff940c7fe97ef48b49d18421e5668af5811c0b3f74cf21f33f54';
var dapp_name = 'Sagittarius';

function cancel_order(order_id) {
    var tr = new bytetrade_js.TransactionBuilder();
    var ob = {
        fee: '300000000000000',   
        creator: test_userid,
        market_name: 4294967297,
        order_id: order_id,
        money_id: 2,
        stock_id: 3
    }

    tr.add_type_operation("order_cancel", ob);

    tr.timestamp = Math.ceil(Date.now() / 1000);
    tr.dapp=dapp_name;
    tr.validate_type = 0;
    tr.add_signer(bytetrade_js.PrivateKey.fromHex(test_privatekey));
    tr.finalize();
    if (!tr.signed) { tr.sign(); }
    var trObj = tr.toObject();
    console.log("id " + tr.id());
    signedTransaction = JSON.stringify(trObj);
    const request = {
        'trObj': signedTransaction,
    };
    // next step is POST request to /transaction/cancelorder endpoint
}
```


#### POST /transaction/withdraw Create a withdraw application
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| chainType     | true  | uint32 | the chain type id of the currency, the chain type name corresponding to the chainType field in the currencies interface    |       |1:ethereum, 2:bitcoin, 3:cmt, 4:naka  |
| toExternalAddress     | true  | string | the user address    |       |  |
| trObj     | true  | string | Transaction with serialized signature information   |       |  |
| chainContractAddress     | true  | string | the address of the currency in the corresponding chain, corresponding to the chainContractAddress field in the currencies interface    |       |  |


| Transaction Param   | required  | data type      | description    | default   | value range    |
| ------  | ----- | ------ | ------  | ----- | -------  |
| fee     | true  | uint128 | The amount of packing fee BTT required to create a transaction, expressed in an integer format on the chain  |  300000000000000     |  |
| from     | true  | string | the username to withdraw    |       |  |
| to_external_address     | true  | string | the pixiu address corresponding to the user, corresponding to the address field in the depositaddress interface|       |  |
| asset_type     | true  | uint32 | currency code     |       |  |
| amount     | true  | uint128 | the number of currency, expressed in an integer format on the chain     |       |  |
| asset_fee     | false  | uint128 | the withdraw fee, valid only when withdraw BTC and USDT, corresponding to the fee field in the currencies interface  |       |  |

**Example:**

```javascript
var bytetrade_js = require('../utils/bytetrade.min.js');
var test_userid='harvey1340';
var test_privatekey='682503fc0582ff940c7fe97ef48b49d18421e5668af5811c0b3f74cf21f33f54';
var pixiu_middle_address = '0x4e52cb4aba1b507ed27ab046791eddee6d471b2d';
var user_real_address = '0x34aF13D89eBCdbc0548FdeB15e5FFF5c2eD93036'
var dapp_name = 'Sagittarius';
var chainType = 1; // ETH


function withdraw() {
    var tr = new bytetrade_js.TransactionBuilder();
    if (chainType !== 2) {  // not btc chain
        var ob = {
            fee: '300000000000000',
            from: test_userid,
            to_external_address: pixiu_middle_address,
            asset_type: 2,
            amount: '20000000000000000'
        };
        tr.add_type_operation("withdraw", ob);
        tr.propose({ fee: '300000000000000',
            proposaler: test_userid,
            expiration_time: Math.ceil(Date.now() / 1000),
            proposed_ops: []
        });
    } else {
        var ob = {
            fee: '300000000000000',
            from: test_userid,
            to_external_address: pixiu_middle_address,
            asset_type: 32,
            amount: '20000000000000000',
            asset_fee: '53004', // btc chain fee
        };
        tr.add_type_operation("withdraw2", ob);
    }

    tr.timestamp = Math.ceil(Date.now() / 1000);
    tr.dapp=dapp_name;
    tr.validate_type = 0;
    tr.add_signer(bytetrade_js.PrivateKey.fromHex(test_privatekey));
    tr.finalize();
    if (!tr.signed) { tr.sign(); }
    var trObj = tr.toObject();
    console.log("id " + tr.id());
    signedTransaction = JSON.stringify(trObj);
    const request = {};
    request['chainType'] = chainType;
    request['toExternalAddress'] = user_real_address;
    request['trObj'] = signedTransaction;
    request['chainContractAddress'] = '0x0000000000000000000000000000000000000000';
    // next step is POST request to /transaction/withdraw endpoint
}
```




#### POST /transaction/transfer Create a transfer application
Request:

| Param   | required  | data type     | description    | default   | value range   |
| ------  | ----- | ------ | ------  | ----- | -------  |
| trObj     | true  | string | Transaction with serialized signature information   |       |  |

| Transaction Param   | required  | data type      | description    | default   | value range    |
| ------  | ----- | ------ | ------  | ----- | -------  |
| fee     | true  | uint128 | The amount of packing fee BTT required to create a transaction, expressed in an integer format on the chain |  300000000000000     |  |
| from     | true  | string | the username who transfer currency   |       |  |
| to     | true  | string | the username who accept currency |       |  |
| asset_type     | true  | uint32 | currency code    |       |  |
| amount     | true  | uint128 | the number of currency, expressed in an integer format on the chain  |       |  |


**Example:**

```javascript
var bytetrade_js = require('../utils/bytetrade.min.js');
var test_userid='harvey1340';
var test_privatekey='682503fc0582ff940c7fe97ef48b49d18421e5668af5811c0b3f74cf21f33f54';

function transfer(to,asset,amount,message) {
    var tr = new bytetrade_js.TransactionBuilder();
    var ob = {
        fee: '300000000000000',
        from: test_userid,
        to: to,
        asset_type: asset,
        amount: amount
    }

    tr.add_type_operation("transfer", ob);

    tr.timestamp = Math.ceil(Date.now() / 1000);
    tr.dapp=dapp_name;
    tr.validate_type = 0;
    tr.add_signer(bytetrade_js.PrivateKey.fromHex(test_privatekey));
    tr.finalize();
    if (!tr.signed) { tr.sign(); }
    var trObj = tr.toObject();
    console.log("id " + tr.id());
    signedTransaction = JSON.stringify(trObj);
    const request = {
        'trObj': signedTransaction,
    };
    // next step is POST request to /transaction/transfer endpoint
}
```

