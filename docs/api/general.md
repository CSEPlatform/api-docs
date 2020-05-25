# General API Information


### Endpoints
--- 

`POST` | `http://api-exchange.cse30.io`

By default, you dont need access API Key and API Secret. But, when you use methods: `cse_transfer` & `cse_createWallet`, you must access API Key and API Secret in headers. 

Key | Value
------------ | ------------
apikey | `your API Key`.
apisecret | `your API Secret`.

>Example:

```
curl --location --request POST "http://api-exchange.cse30.io/" \
  --header "Content-Type: application/json" \
  --header "apikey: `your APIKey`" \
  --header "apisecret: `your API Secret`" \
  --data "{
    \"jsonrpc\": \"2.0\",
    \"method\": \"cse_createWallet\",
    \"params\":  [\"\"],
    \"id\": 2
}"
```
### JSON-RPC methods
---
 - [cse_getLatestBlock](#cse_getLatestBlock)
 - [cse_getPrice](#cse_getPrice)
 - [cse_getFee](#cse_getFee)
 - [cse_getTransactionByTxId](#cse_getTransactionByTxId)
 - [cse_transfer](#cse_transfer)
 - [cse_getTransactionByAddress](#cse_getTransactionByAddress)
 - [cse_createWallet](#cse_createWallet)
 - [cse_getBalance](#cse_getBalance)

 ---
# cse_getLatestBlock

Returns the lastest block

**Parameters**

none

**Returns**

``` Object ``` - A block object
 - preHash - ``` String ```
 - hash - ``` String ```
 - timestamp - ``` String ```
 - height - ``` Number ```

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_getLatestBlock",
    "params":[""],
    "id":1
  }
```
  
Result
```
{
    "preHash": "0d8f0c0c49ca5b9c7bb5abb52d8cda8001a0c8564b77e5db6fbecb9f2745b5ac",
    "hash": "952f2ea2345a540b65a33b1ffbb07aa3cb0790ce8cf15a29c21d9f4dad09acc4",
    "timestamp": "1545320263106",
    "height": 1482
  }
```
---

# cse_getPrice

Returns exchange price from CSE to USDT

**Parameters**

none

**Returns**

``` Object ```
- unit - ``` String ```
- price - ``` Number ```

**Example**

Request
```
  { 
    "jsonrpc":"2.0",
    "method":"cse_getPrice",
    "params":[],
    "id":1
  }
```
Result
```
  {
    "unit": "USDT",
    "price": 0.3
  }
```
---

# cse_getFee

Returns fee of CSE

**Parameters**

none

**Returns**

``` Object ```
- unit - ``` String ```
- fee - ``` Number ```

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_getFee",
    "params":[],
    "id":1
  }
```
Result
```
  {
    "unit": "CSE",
    "fee": 0.0001
  }
```
---

# cse_getTransactionByTxId

Return transaction by TxId

**Parameters**
- TxId

**Returns**

``` Object ```
- amount - ``` String ```
- txId - ``` String ```
- blockHash - ``` String ```
- toAddress - ``` String ```
- fromAddress - ``` String ```
- status - ``` String ```

**Example**

Request
```
  {
   "jsonrpc":"2.0",
    "method":"cse_getTransactionByTxId",
    "params":["b3cfe00ede90c2c8f507253a2da1bc9d5809e904ddae3ef978c84828a51f70ad"],
    "id":1
  }
```
Result
```
  {
    "amount": "1.00000",
    "txId": "b3cfe00ede90c2c8f507253a2da1bc9d5809e904ddae3ef978c84828a51f70ad",
    "blockHash": "29b45e8f88d9226a387efb3011f44f941d9075116ae17e237bda9fcec54e4665",
    "toAddress": "CSED3092857E92424EB66B46D7",
    "fromAddress": "CSE52BA77F97FFF100913DC11B",
    "status": "SUCCESS"
  }
```
---

# cse_transfer

Transfer amount to another address

**Parameters**

``` Object ```
- toAddress - ``` String ```
- amount - ``` String ```

**Headers**
- apikey - ``` Your API Key ```
- apisecret - ``` Your API Secret ```
- authorization - ```Private Key of Send Wallet ```

**Returns**

``` Boolean ```
- true || false

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_transfer",
    "params":[{"toAddress":"CSED3092857E92424EB66B46D7","amount": "0.1"}],
    "id":1
  }
```
Result
```
  {
    result: {
      "isSuccess": true,
      "txId": "7548c28ef6baadc7db76aab727bdec813f47e93156fe3836fbd869a67feb43d2"
    }
  }
```
---

# cse_getTransactionByAddress

Return transaction by address

**Parameters**
- address
- type - ``` ALL|RECEIVE|SEND ```
- options {page, limit}

**Returns**

``` Object ```
- amount - ``` String ```
- txId  - ``` String ```
- blockHash - ``` String ```
- toAddress - ``` String ```
- fromAddress - ``` String ```
- status - ``` String ```

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_getTransactionByAddress",
    "params":["CSED3092857E92424EB66B46D7", "ALL", {"page":1,"limit":"10"}],
    "id":1
  }
```
Result
  ```
    [
      {
        "amount": "1.00000",
        "txId": "b3cfe00ede90c2c8f507253a2da1bc9d5809e904ddae3ef978c84828a51f70ad",
        "blockHash": "29b45e8f88d9226a387efb3011f44f941d9075116ae17e237bda9fcec54e4665",
        "toAddress": "CSED3092857E92424EB66B46D7",
        "fromAddress": "CSE52BA77F97FFF100913DC11B",
        "status": "SUCCESS"
      }
  ]
  ```
---

# cse_createWallet

Create a wallet and returns private key and address of wallet

**Headers**
- apikey - ``` Your API Key ```
- apisecret - ``` Your API Secret ```

**Returns** 

``` Object ```

- pk - ``` Object ```
- address - ``` String ```

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_createWallet",
    "params":[""],
    "id":1
  }
```
Result
```
  {
    "pk": 
      {
        "privateKey": "...",
        "iv": "...",
        "salt": "..."
      },
    "address": "..."
  }
```
---

# cse_getBalance

Returns wallet by address

**Parameters**
- address

**Returns**

``` Object ```
- available - ``` String ```
- address - ``` String ```

**Example**

Request
```
  {
    "jsonrpc":"2.0",
    "method":"cse_getBalance",
    "params":["CSED3092857E92424EB66B46D7"],
    "id":1
  }
```
Result
  ```
  {
    "available": "0.00180",
    "address": "CSED3092857E92424EB66B46D7"
  }
```
---

### Webhook Notification Events

CSE will send webhook events whenever you successfully receive a payment.

# `Wallet Deposited Success`

**Event Type:** ``` WALLET_DEPOSITED_SUCCESS ```

**Payload:**
```
{
   "type":"WALLET_DEPOSITED_SUCCESS",
   "data":{
      "address":"CSE7D4F2ECC49A6C17CF230E30",
      "amount":"0.20000",
      "rawTransaction":{
         "amount":"0.20000",
         "toAddress":"CSE7D4F2ECC49A6C17CF230E30",
         "fromAddress":"CSE91F712F8D8B91C79BCB0AF8",
         "txId":"85c48e59add381e0c77da49015bd2f0debe9fb362a7e99f64e444c3a4dfee5f1",
         "blockHash":"ae191887b6348d880888e5c723396e85380159e63539ab30be43d81730725526",
         "status":"SUCCESS",
         "createdAt":"2018-12-21T06:36:05.406Z",
         "fee":0.0001
      }
   }
}
```
---

# `Wallet Withdrawal Success`

**Event Type:** ``` WALLET_WITHDRAWAL_SUCCESS ```

**Payload:**
```
{
   "type":"WALLET_WITHDRAWAL_SUCCESS",
   "data":{
      "address":"CSEE5B2D9D4E716D496E2452BC",
      "amount":"20.00000",
      "rawTransaction":{
         "amount":"20.00000",
         "toAddress":"CSE13A15BC1CEE13FF301D5F3F",
         "fromAddress":"CSEE5B2D9D4E716D496E2452BC",
         "txId":"ad9b31d6a4be04ba6132c925c52e49dfda3b4d1ac9d0737cc6d1db684944188a",
         "blockHash":"15a0a390f93fc462f7ca44982269fb460ea3e9a080ba212d1ac2d1cd772e1739",
         "status":"SUCCESS",
         "createdAt":"2018-12-21T06:24:05.007Z",
         "fee":0.0001
      }
   }
}
```
