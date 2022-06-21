# Dev Note - Web3

## Truffle

### Trouble Shooting

#### Error: Number can only safely store up to 53 bits

Because of BN ETH.

```javascript
// Modify
userPaid = registrantsPaid.toNumber()
// to
userPaid = registrantsPaid
```

#### TypeError: web3.toWei is not a function

```javascript
// Modify
web3.toWei(1, 'ether')
// to
web3.utils.toWei(1, 'ether')
```

## Polygon / Matic Network

### Quick facts

| Property                     | Value                                    |
| :--------------------------- | :--------------------------------------- |
| Matic Mainnet chainId        | 137                                      |
| Matic Mumbai Testnet chainId | 80001                                    |
| Matic Blockchain Explorer    | https://explorer-mainnet.maticvigil.com/ |
| Block time                   | ~3 seconds                               |
| Data refresh latency         | ~6 seconds or 2 Blocks                   |

