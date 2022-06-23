# Dev Note - Web3

## Concept

### Bytecode instruction

Every instruction has a length of 1 byte.

contract.json :: bytecode == Ganache :: Transactions :: Contract Creation :: TX DATA

contract.json :: deployedBytecode is the data deployed on blockchain.

```javascript
// Get deployedBytecode
web3.eth.getCode("{contract address}")
```

bytecode is a subcollection of deployedBytecode.

EVM include: 1. storage 2. memory 3. stack

OP CODE can find in the ETH yellow paper.

```assembly
0x15 ISZERO     # Simple not operator.
0x34 CALLVALUE  # Get deposited value by th instuction/transaction responsible for this execution.
0x39 CODECOPY   # Copy code running in current environment to memory.
0x50 POP        # Remove item from stack.
0x52 MSTORE     # Save word to memory.
0x57 JUMP1      # Conditionally alter the program counter.
0x5b JUMPDEST   # Mark a valid destinantion for jumps.
0x60 PUSH1      # Place 1 byte item on stack.
0x80 DUP1       # Duplicate 1st stack item.
0xf3 RETURN     # Halt execution returning output data.
```

program counter pointer to the index of bytecode.

### Gas

Gas Price : How much ETH for each gas unit. Gwei = 0.000 000 001 Eth
Gas Price  20 Gwei : Gas Price = 0.000 000 02 Eth

The maximum fee : Tx Fee = Gas Limit * Gas Price.
Normal transaction's Gas Limit = 21000.

### Keccak-256

ETH hash is Keccak256.

Private key ==[secp256k1]==> Public key ==[Keccak256]==> ETH Address

Example:

```bash
Private key:
bcd3a9cc19cdf4401106d5315e947d5688112f0bec622d15e19d6f0afe8820c6

Public key:
0x0435127b603a3f9f9e38f7683c885bcae69279a1027ccf4693d153e3afc045fc39db1c668a169993f324aa59ddfc44aa91ba178921e31738fd08d335afde244302

Ethereum address:
0x28826f66983d39e0cf67df3a9e20288337d0c584
```

bip39 helper tool cut off Public key, can not generate correct ETH Address。

### Function Signature

setCompleted(uint256) ==[Keccak256]==>

fdacd576 2b86e21561bc37c704505a12d8a5b260c671df7e3ed98e1cbb14f7c5

TX DATA

0xfdacd576 0000000000000000000000000000000000000000000000000000000000000002 (the length of input value 2 is 256)

The begin of start 8 hex is the same.

bytecode can found fdacd576

### nonce 

A scalar value equal to the number of transactions sent from this address or, in the case of accounts with associated code, the number of contract-creations made by this account.

### nonce +1(Position)

Start a transaction, nonce +1,
1.EOA send a transaction.
2.Contract Wallet generate a contract

nonce will not change : Input transaction, call contract

### CSS

Tailwind CSS is the Utility-First CSS make the front-end will not look like some standard template. VSCode support it.

## Solidity

### Public vs. External

Basically, public means it can be external or internal, the compiler need additional work for the public function. With the external only, it allows arguments to be read directly from calldata, saving the copying step.

### pure, view

read-only call, no gas fee

view - it indeicates that the function will not alter the storage state in any way

pure - even more strict, indicating that it won't even read the storage state

## Web3

```javascript
// 
```

## Truffle

### Command

```bash
# Initial project
truffle init

# Connect to Ganache with truffle-config.js development network config
truffle console --network development

# Compile & deploy again
migrate --reset
```

### Web3 at Truffle console

```javascript
// Get wallet's address
accounts

// Get instance of contract
const instance = await Faucet.deployed()

// Read public variable
const funds = await instance.funds()

// BN convert to string type
funds.toString()

// Interact with contract according contract address with web3
const instance = new web3.eth.Contract(Faucet.abi, "0x7e5FD5f569Fe8903baB6607d3b87d45DA591bf8A")

// Read public variable with web3
const funds = await instance.methods.funds().call()

// Make transaction, send from "from" to "to" 10 eth, default unit is wei.
web3.eth.sendTransaction({from: accounts[0], to: "0x6339248a77e255466A209EED18Fd39032C438611", value: "10000000000000000000"})

// Query block with block height
web3.eth.getBlock("9")

const instance = await Faucet.deployed()

instance.addFunds({value: "2000000000000000000"})

// Call function by its signiture

// addFunds() =[Keccak-256]=> 0xa26759cb...
web3.eth.sendTransaction({from: accounts[0], to: '0xC138B35FA662D9601f6b12Efa7138157B57b1059', data: '0xa26759cb', value: '3000000000000000000'})
```

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

## JSON-RPC

```json
{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":"1"}
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

## Online Tool

blockchain data demo

https://andersbrownworth.com/blockchain/

Visualization show blockchain

http://ethviewer.live/

Encoder

(expired) https://toolkit.abdk.consulting/ethereum#rlp
