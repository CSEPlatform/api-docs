> We use JSON RPC to call API. If you don't know it yet, go to this [link](https://github.com/ethereum/wiki/wiki/JSON-RPC#json-rpc-api).

## JSON RPC API
#### contract_run
**Headers**

- authorization - In case your function requires
- socketKey - key to receive result from server

**Parameters**

- Your Contract Address
- Your Method
- Your Params

**Returns**

- Result of your function

_or_
- If you use a function is not viewFuncs, it will return "PROCCESSING" or reason why your function can not apply.

**Example**

```
curl --location --request POST "http://35.247.154.90:8181" \
  --header "Content-Type: application/json" \
  --header "authorization: 633f9446af5028b89a730b6fafd11785371fd3dc948311b9ea1a3dfe61739cfb" \
  --header "socketKey: 4c5c3eb76c7a8368a5ad7bb52f9da0ef7be4552622d6b8e0f59e675af4e77f0d" \
  --data "{
	\"jsonrpc\":\"2.0\",
	\"method\":\"contract_run\",
	\"params\":[\"CSE215AAE36983E03699E5BDE3\", \"buyToken\", 2, 2],
	\"id\":1
}"
```

