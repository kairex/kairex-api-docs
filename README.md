# KAiREX Open API docs (v 0.9)

Kairex provides an open API for our clients to access Kairex platform and control their accounts with their customized software. The base URL is "https://api.kairex.com/{version}".   (All requests should be application/json content type. )

This page explains about how to use our API and how to set up your software to use our API. 


**NOTE: Request limit**

We have a request limit based on IP for public API and on API key for private API. You cannot ask more than 600 requests per 10 minutes. 
If you exceed this limit, Kairex returns http status code 429, meaning "too many requests in a given amount of time". 



***

## PUBLIC API 

With the public API, you can get Kairex's market data such as ticker and orderbook. It is open for every clients. 



### - TICKER 
[GET] https://api.kairex.com/v1/market/ticker?quote=BTC&base=KAI

required parameters : quote, base 

response : 


### - ORDERBOOK 
[GET] https://api.kairex.com/v1/market/orderbook?quote=BTC&base=KAI

required parameters : quote, base 

response : 



***

## ACCOUNT API 

To use our account API, it is required to provide your API key, a nonce and a signature in your request header. 

X-KAIREX-APIKEY : It is your API key value and you can get an API key and secret key as a pair from kairex.com(URL 넣어줄 것). 

X-KAIREX-NONCE : It is an integer number which must be with your every request and must be increasing. We generally use unix time as a nonce. 

X-KAIREX-SIGNATURE: You can make a signature with your secret key, API key and a nonce. Signature is a HMAC-SHA256 encoded message containing a nonce and your API key. 


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


### - BUY 
[POST] https://api.kairex.com/v1/order/buy


### - SELL 
[POST] https://api.kairex.com/v1/order/sell


### - CANCEL 
[POST] https://api.kairex.com/v1/order/cancel


### - ORDER HISTORY (미체결된 것) 
[GET] https://api.kairex.com/v1/order/history


### - TX HISTORY 
[GET] https://api.kairex.com/v1/order/tx/history


