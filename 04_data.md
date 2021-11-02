
### 16. Private on-chain data:

Marking variables `private` does not mean that they cannot be read on-chain.

**BEST PRACTICE**: data intended to be secret should not be stored unencrypted in contract code or state but instead stored encrypted or off-chain.

see [here](https://swcregistry.io/docs/SWC-136)

### 17. Weak PRNG:

**AVOID**: PRNG relying on `block.timestamp`, `now` or `blockhash` can be influenced by miners to some extent and should be avoided.

see [here](https://swcregistry.io/docs/SWC-120)

### 18. Block values as time proxies:

**WARNING**: `block.timestamp` and `block.number` are not good representations for time because of issues with synchronization, miner manipulation and changing block times.

see [here](https://swcregistry.io/docs/SWC-116)

### 20. Integer overflow/underflow:

Not using OpenZeppelin’s `SafeMath` (or similar libraries) that check for overflows/underflows may lead to vulnerabilities or unexpected behavior if user/attacker can control the integer operands of such arithmetic operations.

Solc `v0.8.0` introduced default overflow/underflow checks for all arithmetic operations.

see [here](https://swcregistry.io/docs/SWC-101)
see [here](https://blog.soliditylang.org/2020/10/28/solidity-0.8.x-preview/)

**AVOID**: Division before multiplication can cause precision bugs.

**BEST PRACTICE**: Performing multiplication before division is generally better to avoid loss of precision because Solidity integer division might truncate. (see here)
