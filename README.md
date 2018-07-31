# KAiREX Open API docs (v 0.9)

Kairex provides an open API for our clients to access Kairex platform and use their accounts with their customized software. The base URL is "https://api.kairex.com/{version}".   (All requests have to be of application/json content type. )

This page explains how to use our API and how to set up your software for using our API. 

```text
NOTE: Request limit

The number of requests are limited based on IP for public API or on API key for private API. 
You cannot ask more than 600 requests per 10 minutes. 
If you exceed this limit, Kairex returns http status code 429, meaning "too many requests in a given amount of time". 
```


***

## PUBLIC API 

With the public API, you can get Kairex's market data such as ticker and orderbook. It is open for any clients. 



### - TICKER 
```text
[GET] https://api.kairex.com/v1/market/ticker?quote=BTC&base=KAI

Request : `quote`, `base` 

Response :          
{
  "base":"KAI"
  ,"open":"0.00000072"
  ,"high":"0.00000077"
  ,"low":"0.00000065"
  ,"close":"0.00000077"
  ,"volume":"1470356.57704892"
  ,"total":"1.05939619"
  ,"closed":"0.00000072"
  ,"change":"0.00000005"
  ,"changePercent":"6.9400"
  ,"rankNum":null
}
```

### - ORDERBOOK 
```text
[GET] https://api.kairex.com/v1/market/orderbook?quote=BTC&base=KAI

Request : `quote`, `base` 

response : 
{
  "symbolPair":"KAIBTC"
  ,"orderSeq":27562
  ,"base":"KAI"
  ,"quote":"BTC"
  ,"price":"0.00000078"
  ,"closed":"0.00000072"
  ,"bid":[{"price":"0.00000077","sumAmount":"377482.49931311","changePercent":"6.9400"},
          {"price":"0.00000071","sumAmount":"69433.26760563","changePercent":"-1.3900"} ... ]
  ,"ask":[{"price":"0.00000078","sumAmount":"138346.51989629","changePercent":"8.3300"},
          {"price":"0.0000008","sumAmount":"10000","changePercent":"11.1100"} ... ]
  ,"symbol":"KAI/BTC"
}
```

***

## ACCOUNT API 

To use our account API, it is required to provide your API key, a nonce and a signature in your request header. 

**X-KAIREX-APIKEY** : Your API key value. You can get a pair of an API key and secret key from https://www.kairex.com/mypage/openApi. 

**X-KAIREX-NONCE** : An integer number which must be incremented with your every request. We generally use unix time as a nonce. 

**X-KAIREX-SIGNATURE** : A signature which you make with your secret key, API key and a nonce. This is an HMAC-SHA256 encoded message containing a nonce and your API key. 


For more details, please refer to the following samples. 



JavaScript
``` js 
var hash = CryptoJS.HmacSHA256(apiKey + nonce, secretKey);
var hashInBase64 = CryptoJS.enc.Base64.stringify(hash);
```


Java
``` java 
private String signature(String apiKey, Integer nonce,String secretKey) throws Exception {
  Mac HmacSha256 = Mac.getInstance("HmacSHA256");
  SecretKeySpec secretKeySpec = new SecretKeySpec(secretKey.getBytes("UTF-8"), "HmacSHA256");
  HmacSha256.init(secretKeySpec);
  return Base64.encodeBase64String(HmacSha256.doFinal((apiKey + nonce).getBytes("UTF-8")));
}
```



### - USER BALANCE 
```text
[GET] https://api.kairex.com/v1/balance?currency=KAI

Request : `currency` 

Response : {"currency":"BTC","available":"0.00019925","reserved":"0.00000000"}
```

### - BUY
```text
[POST] https://api.kairex.com/v1/order/buy

Request : `quote`, `base`, `price`, `amount` 
(If the price and amount values exceed 8 decimal places, these are rounded down)

Response : {"code":"SUCCESS","httpStatusCode":200,"httpStatus":"OK"}
```

### - SELL 
```text
[POST] https://api.kairex.com/v1/order/sell

Request : `quote`, `base`, `price`, `amount`
(If the price and amount values exceed 8 decimal places, these are rounded down)

Response : {"code":"SUCCESS","httpStatusCode":200,"httpStatus":"OK"}
```

### - CANCEL 
```text
[POST] https://api.kairex.com/v1/order/cancel

Request : `quote`, `base`, `orderId`

Response : {"code":"SUCCESS","httpStatusCode":200,"httpStatus":"OK"}

           {"code":"ALREADY_ORDERED","httpStatusCode":400,"httpStatus":"BAD_REQUEST"}
```
### - OPEN ORDER HISTORY
```text
[GET] https://api.kairex.com/v1/order/history

Request : `quote`, `base`, `page`, `rows` 

Response : 
{
  "totalPages":1
  ,"currentPage":1
  ,"totalRecords":1
  ,"data: [{
       "orderId":"OD1533035094142_KR00_12142"
      ,"orderType":"SELL"
      ,"orderDateTime":"1533035094"
      ,"price":"1"
      ,"amount":"0.999"
      ,"leftAmount":"0.999"
  }]
}

```
### - TX HISTORY 
```text
[GET] https://api.kairex.com/v1/order/tx/history

Request : `quote`, `base`, `page`, `rows`

Response : 
{
"totalPages":1
,"currentPage":1
,"totalRecords":2
,"data":[ 
      {"orderId":"OD1533034973537_KR00_4122"
      ,"txPrice":"0.00000078"
      ,"txAmount":"0.5"
      ,"fee":"0.0005"
      ,"txDateTime":"1533034974"
      },
      {"orderId":"OD1533034047056_KR00_12342"
      ,"txPrice":"0.00000075"
      ,"txAmount":"1"
      ,"fee":"0.001"
      ,"txDateTime":"1533034047"}
  ]
}
```
