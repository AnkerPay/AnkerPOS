# AnkerPOS
AnkerPOS APIs allow for simple integration into current and existing payment providers. Add invoices to your website or application to allow users to pay with bitcoin or other crypto currencies.

# Public Rest API for AnkerPOS

## Available coins
BTC
Bitcoin Lightning
ETH
USDC
USDT
TRX

## General API Information
* The base endpoint for **BTC** is: **https://ankerpay.com/api/v1/BTC/pos**
* The base endpoint for **Bitcoin Lightning** is: **https://ankerpay.com/lightning/api/v1/pos**
* The base endpoint for **ETH** is: **https://ankerpay.com/api/v1/ETH/pos**
* The base endpoint for **USDC** is: **https://ankerpay.com/api/v1/USDC/pos**
* The base endpoint for **USDT** is: **https://ankerpay.com/api/v1/USDT/pos**
* The base endpoint for **TRX** is: **https://ankerpay.com/api/v1/TRX/pos**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in **seconds**.

## HTTP Return Codes

* HTTP `4XX` return codes are used for malformed requests;
  the issue is on the sender's side.
* HTTP `403` return code is used when the WAF Limit (Web Application Firewall) has been violated.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  Binance's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.


## Error Codes
* Any endpoint can return an ERROR

Sample Payload below:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

## General Information on Endpoints
* For `GET` endpoints, parameters must be sent as a `query string`.



# Public API Endpoints

## Market Data endpoints

###  Current coin /ZAR rate. 
```
GET /rate
```
This endpoint allows anyone to get a rate of selected coin /ZAR pair

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "price": "181000.00", 
}
```


## POS endpoints
### Fixed Amount Invoice.
```
POST /invoice/new
```
This call allows you to create a new fixed amount invoice. You provide a StoreId and TerminalId keys and the amount in ZAR you want get to it. We return the amount in BTC to deposit and the address to deposit to. 

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | the StoreId
terminalId | STRING | YES | the TerminalId
amount | STRING | YES | amount in ZAR



**Response:**
```javascript
{
  "time": 1596477571, 
  "success": {
    "invoiceId": "AP26YkJo7tb4MCmDt7HhPt",
    "pair": "btc_zar",
    "withdrawal": "Bank Account",
    "withdrawalAmount": "100",
    "deposit": "34qvKdkfSh85Kkct6tmCxLfLjJff73Sim4",
    "depositAmount": "0.00007641",
    "expiration": 1500,
    "apiPubKey": "AcNPAwpEiQmvpbvvyw5DiLHH8UsY3oWcpnSiy",
    "maxLimit": 2.83270872,
    "minerFee": "0.00075",
    "coins": {
      "BTC":  {
          "deposit": "34qvKdkfSh85Kkct6tmCxLfLjJff73Sim4",
          "depositAmount": "0.00007641",
          "invoiceUrl": "bitcoin:3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB?amount=0.00007641", 
          "minerFee": "0.00075",
          "icon": "/api/v1/coins/BTC.png",
        },
    }
  }
}
```


###  Invoice Status.
```
POST /invoice/status
```
This endpoint returns the status of the invoice.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
invoiceId | STRING | YES |  invoice Id to look up.

**Response:**
```javascript
{
  "cryptoCode": "BTC", 
  "invoiceId": "YC2eeMgSijcnf4PL72oPhG", 
  "btcAddress": "3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB", 
  "btcDue": "0.00085195", 
  "expirationSeconds": 379, 
  "status": "invalid", 
  "maxTimeSeconds": 1500, 
  "storeName": "AnkerPay", 
  "orderAmount": "0.00085195", 
  "orderAmountFiat": "R150,00 (ZAR)", 
  "invoiceBitcoinUrl": "bitcoin:3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB?amount=0.00085195", 
  "invoiceBitcoinUrlQR": "bitcoin:3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB?amount=0.00085195", 
  "txCount": 1, 
  "btcPaid": "0.00000000"
}
```


###  Status of deposit to address.
```
GET /status/[address]
```
This endpoint returns the status of the most recent deposit transaction to the address.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES |  is the deposit address to look up.

**Response:**
```javascript
{
  "cryptoCode": "BTC", 
  "invoiceId": "YC2eeMgSijcnf4PL72oPhG", 
  "btcAddress": "3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB", 
  "btcDue": "0.00085195", 
  "expirationSeconds": 379, 
  "status": "invalid", 
  "maxTimeSeconds": 1500, 
  "storeName": "AnkerPay", 
  "orderAmount": "0.00085195", 
  "orderAmountFiat": "R150,00 (ZAR)", 
  "invoiceBitcoinUrl": "bitcoin:3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB?amount=0.00085195", 
  "invoiceBitcoinUrlQR": "bitcoin:3GQpWUCi94opx4dpynd2qJhoRAtepnWvUB?amount=0.00085195", 
  "txCount": 1, 
  "btcPaid": "0.00000000"
}
```

###  Add New terminal.
```
POST /terminal/new
```
This endpoint add new terminal to the Store.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
place | STRING | YES | terminal place.

**Response:**
```javascript
{
  "time": 1596477571, 
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
}
```

###  Terminal status.
```
POST /terminal/status
```
This endpoint returns the status of terminal.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
  "status": "open",
}
```

###  Terminal balance.
```
POST /terminal/balance
```
This endpoint returns the unsettled balance of terminal in ZAR.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "balance": "500.00",
}
```

###  Terminal's invoice list.
```
POST /terminal/invoices
```
This endpoint returns the list of invoices.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
  {
    "orderId": "PETzZsYh8uYxSanS9YsBPy", 
    "pair": "BTC_ZAR", 
    "depositAddress": "3FfwQkufqCEr362Yti69QnVg9Ct19Fb7jC", 
    "depositAmount": "150", 
    "withdrawalAmount": "150.0", 
    "created": "2020-09-09 13:51:46", 
    "status": "no_deposit"
  },
  {
    "orderId": "PETzZsYh8uYxSanS9YsBPy", 
    "pair": "BTC_ZAR", 
    "depositAddress": "3FfwQkufqCEr362Yti69QnVg9Ct19Fb7jC", 
    "depositAmount": "150", 
    "withdrawalAmount": "150.0", 
    "created": "2020-09-09 13:51:46", 
    "status": "no_deposit"
  }
  ]
}
```

###  Store balance.
```
POST /store/balance
```
This endpoint returns the unsettled balance of the store in ZAR.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "balance": "5000.00",
}
```

###  Store's terminal list.
```
POST /store/terminals
```
This endpoint returns the list of invoices.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
  {
  "status": "open",
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
  },
  {
  "status": "open",
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
  }
  ]
}
```

###  QR code image generation.
```
GET /qr?img=[address]
```
This endpoint returns PNG image with the address QR code.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES |  is the deposit address.

**Response:**
```javascript
PNG image
```







