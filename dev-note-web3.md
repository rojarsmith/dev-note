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

