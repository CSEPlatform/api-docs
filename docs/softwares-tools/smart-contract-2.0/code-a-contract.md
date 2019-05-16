
***

> Before coding a contract, you have to know what is Javascript base on ES6?
If you know it, you can pass. If not, you can study it with my suggestions: [`W3School`](https://www.w3schools.com/js/js_es6.asp), [`tutorialspoint`](https://www.tutorialspoint.com/es6/), ... 

***

# Let's start

## Content
* [Step 1: Create](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/#step-1-create)
* [Step 2: Import and Extends](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/_edit#step-1-create)
* [Step 3: Coding](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/#step-3-coding)
* [Step 4: Schema](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/#step-4-schema)
* [Step 5: Write you functions](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/#step-5-write-you-functions)
* [Step 6: package.json](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract#step-6-packagejson)
###  Step 1: Create
Create a file with extend-file is `.js` and a file [`package.json`](https://github.com/CSEPlatform/Smart-Contract-Docs/wiki/How-To-Code-Contract/#step-5-packagejson)

![create file](https://i.imgur.com/q5lxxEk.png)
### Step 2: Import and Extends
You need to 

```
import Contract from 'Contract'
import Payment from 'Payment'
```
With `Payment`, if you don't use it, you can not import them.
And your class name need `extends Contract`
> Example:
```
class TokenMain extends Contract {}
```
### Step 3: Coding
For our contract, we have 3 ways to distinguish:

 1. **`viewFuncs`**

    _viewFuncs_ is array that contains methods are not charged for execution and do not modify the database 
    > Example:
    ```
    static viewFuncs = ['getTokenName', 'getTokenSymbol']
    ```
 2. **`authenticationFuncs`**

    _authenticationFuncs_ is array that contains methods that need to use private key to execute
    > Example:
    ```
    static authenticationFuncs = ['transfer']
    ```
 3. **`publicFuncs`**

    _publicFuncs_ is array that contains methods that user can call and execute
    > Example:
    ```
    static publicFuncs= ['getTokenName', 'getTokenSymbol', 'createAccount','getAccountByAddress', 'transfer']
    ```
**Notice!!!**
> Except for viewFuncs, all the remaining functions are charged for execution

### Step 4: Schema
You need to create a schema like a database of your contract.

### Step 5: Write you functions.
In Constructor, you need to declare
```
constructor(data) {
    super(data)
}
```
Your function
```
getAddresses() {
    return this.accounts
}
```
We support you in building code with functions
* `generateAddress`
   This function will return `address` and `private key`

   ```
   {
        address: `address`,
        privateKey: `privateKey`
   }
   ```
   > Example
   ```
   function createAccount()
   {
       const address = await this.generateAddress()
       const rs = {
             address: address.address,
             balance: 0
       }
       this.accounts.push(rs)
       return address
   }
   ```
* `getLastestBlock`
   This function will return an object

   ```
   {
        "preHash": "08e225dbcfefaf97194f999e4ecb5fc3bf6f5296934ce2ba07b1c9be6d899f5f",
        "hash": "411820972a9b97b6db86cf13abc58d06639a6e33c66d89054009bb433a860bf2",
        "timestamp": "1548054568639",
        "height": 1484
   }
   ```
   > Example
   ```
   function getLastestBlock()
   {
       const block= this.getLastestBlock()
       return block
   }
   ```
* `timestamp`
   This function will return timestamp
   > Example
   ```
   createTimestamp(){
       return this.timestamp()
   }
   ```
* `payment`
   If you want to use this function, you have to import it before.
   > Example
   ```
   import Contract from 'Contract'
   import Payment from 'Payment'
   class TokenMain extends Contract {
      constructor(data) {
          super(data)
          this._payment = new Payment(data)
      }
      function payment(){
          `your code here`
      }
   }
   ```
   
### Step 6: `package.json`

![package.json](https://i.imgur.com/NITn4AJ.png)

