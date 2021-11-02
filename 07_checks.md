### 30. Dangerous usage of tx.origin:

**WARNING**: Use of `tx.origin` for authorization may be abused by a Man In The Middle (MITM) malicious contract forwarding calls from the legitimate user who interacts with it.

**BEST PRACTICE**: Use `msg.sender` instead.

see [here](https://swcregistry.io/docs/SWC-115)

### 31. Contract check:

**WARNING**: Checking if a call was made from an Externally Owned Account (EOA) or a contract account is typically done using `extcodesize` check which may be circumvented by a contract during construction when it does not have source code available.

**ALTERNATIVE**: Checking if `tx.origin == msg.sender` is another option to verify if an account is a contract.

**CAVEAT**: Both methods have implications that need to be considered.

see [here](https://consensys.net/blog/blockchain-development/solidity-best-practices-for-smart-contract-security/)

### 32. Deleting a mapping within a struct:

**WARNING**: Deleting a `struct` that contains a `mapping` will not delete the mapping contents which may lead to unintended consequences.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#deletion-on-mapping-containing-a-structure)

### 33. Tautology or contradiction:

Tautologies (always `true`) or contradictions (always `false`) indicate potential flawed logic or redundant checks.

e.g. `x >= 0` which is always `true` if `x` is `uint`.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#tautology-or-contradiction)

### 34. Boolean constant:

Use of `bool` constants in code (e.g. conditionals) is indicative of flawed logic.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#misuse-of-a-boolean-constant)

### 35. Boolean equality:

`bool` variables can be checked within conditionals directly, without the use of equality operators to true/false.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality)

### 36. State-modifying functions:

**NOTE**: Functions that modify state (in assembly or otherwise) but are labeled `constant`/`pure`/`view` will `revert` in `solc >=0.5.0` because of the use of `STATICCALL` opcode.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#constant-functions-using-assembly-code)

**WARNING**: These functions will not revert in prior versions of solidity.

### 37. Return values of low-level calls:

**BEST PRACTICE**: Ensure that return values of low-level calls (`call`/`callcode`/`delegatecall`/`send`/etc.) are checked to avoid unexpected failures.

see [here](https://swcregistry.io/docs/SWC-104)

### 38. Account existence check for low-level calls:

**WARNING**: Low-level calls `call`/`delegatecall`/`staticcall` return `true` even if the account called is **non-existent** (per EVM design).

**BEST PRACTICE**: Account existence must be checked prior to calling if needed.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-calls)
