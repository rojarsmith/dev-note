# Dev Note - Web3

## Concept

### Bytecode instruction

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

## Solidity

### Public vs. External

Basically, public means it can be external or internal, the compiler need additional work for the public function. With the external only, it allows arguments to be read directly from calldata, saving the copying step.

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

## Online Tool

Encoder

(expired) https://toolkit.abdk.consulting/ethereum#rlp
