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
[GET] https://api.kairex.com/v1/market/ticker?quote=BTC&base=KAI

Params : `quote`, `base` 

Response : 


### - ORDERBOOK 
[GET] https://api.kairex.com/v1/market/orderbook?quote=BTC&base=KAI

Params : `quote`, `base` 

response : 



***

## ACCOUNT API 

To use our account API, it is required to provide your API key, a nonce and a signature in your request header. 

**X-KAIREX-APIKEY** : Your API key value. You can get a pair of an API key and secret key from kairex.com. 

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
[GET] https://api.kairex.com/v1/balance?currency=KAI

Params : `currency` 

### - BUY 
[POST] https://api.kairex.com/v1/order/buy

Params : `quote`, `base`, `price`, `amount` 
```text
If the price and amount values exceed 8 decimal places, these are rounded down
```

### - SELL 
[POST] https://api.kairex.com/v1/order/sell

Params : `quote`, `base`, `price`, `amount`
```text
If the price and amount values exceed 8 decimal places, these are rounded down
```

### - CANCEL 
[POST] https://api.kairex.com/v1/order/cancel


### - OPEN ORDER HISTORY
[GET] https://api.kairex.com/v1/order/history

Params : `quote`, `base` 

### - TX HISTORY 
[GET] https://api.kairex.com/v1/order/tx/history

Params : `quote`, `base`, `page`, `rows`
