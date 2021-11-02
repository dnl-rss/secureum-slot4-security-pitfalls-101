### 26. Unexpected Ether and this.balance:

> substack post is unclear

A `contract` can **receive Ether** via:
- payable functions
- selfdestruct()
- coinbase transaction
- pre-sent before creation

**WARNING** Contract logic depending on `this.balance` can therefore be manipulated.

see [here](https://github.com/sigp/solidity-security-blog#3-unexpected-ether-1)
see [here](https://swcregistry.io/docs/SWC-132)

### 27. fallback vs receive():

**BEST PRACTICE**: Check that all precautions and subtleties of fallback/receive functions related to visibility, state mutability and Ether transfers have been considered.  

see [here](https://docs.soliditylang.org/en/latest/contracts.html#fallback-function)
see [here](https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function)

### 28. Dangerous strict equalities:

**BEST PRACTICE**: Use of strict equalities with tokens/Ether can accidentally/maliciously cause unexpected behavior.

Consider using ``>=`` or `<=` instead of `==` for such variables depending on the contract logic.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-strict-equalities)

### 29. Locked Ether:

**WARNING**: Contracts that accept Ether via `payable` functions but without withdrawal mechanisms will lock up that Ether.

**BEST PRACTICE**: Remove `payable` attribute or add a withdraw function.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#contracts-that-lock-ether)
