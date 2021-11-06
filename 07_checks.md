### 30. Dangerous usage of tx.origin:

**WARNING**: Use of `tx.origin` for authorization may be abused by a Man In The Middle (MITM) malicious contract forwarding calls from the legitimate user who interacts with it.

**BEST PRACTICE**: Do not use `tx.origin` for authorization. Use `msg.sender` instead.

see [here](https://swcregistry.io/docs/SWC-115)

### 31. Contract check:

**WARNING**: Checking if a call was made from a contract account is typically done using `extcodesize > 0` check, which may be circumvented by a contract during construction when it does not have source code available.

**ALTERNATIVE**: Checking if `tx.origin == msg.sender` is another option to verify if an account is a contract.

**CAVEAT**: Both methods have implications that need to be considered.

see [here](https://consensys.net/blog/blockchain-development/solidity-best-practices-for-smart-contract-security/)

### 32. Deleting a mapping within a struct:

**WARNING**: Deleting a `struct` that contains a `mapping` will not delete the mapping contents, which may lead to unintended consequences.

**BEST PRACTICE**: Use `lock` on the data structure to prevent mistaken usage.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#deletion-on-mapping-containing-a-structure)

### 33. Tautology or contradiction:

- tautologies are always `true`
- contradictions are always `false`
- such statements indicate potential flawed logic or redundant checks.

e.g. `x >= 0` which is always `true` if `x` is `uint`.
```solidity
contract Example {
    uint x;

    function doSomething() {
        if ( x>=0 ) {
            // this check is always true
        }
    }
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#tautology-or-contradiction)

### 34. Boolean constant:

Use of `bool` constants in code (e.g. conditionals) is indicative of flawed logic.

```solidity
contract Example {
    bool constant ALWAYS_TRUE = true;
    bool constant ALWAYS_FALSE = false;

    function doSomething() {
        if ( ALWAYS_TRUE ) {
            // this check is always true. remove check
        }

        if ( ALWAYS_FALSE ) {
            // this check is always false. remove code block
        }
    }
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#misuse-of-a-boolean-constant)

### 35. Boolean equality:

`bool` variables can be checked within conditionals directly, without the use of equality operators to true/false.

```solidity
contract Example {
    bool myBool;

    function doSomething() {
        if ( myBool == true ) {
            // this check syntax is unnecessary
        }

        if ( myBool ) {
            // check the bool value directly instead
        }
    }
}
```
see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality)

### 36. State-modifying functions:

**NOTE**: Functions that modify state (in assembly or otherwise) but are labeled `constant`/`pure`/`view` will `revert` in `solc >=0.5.0` because of the use of `STATICCALL` opcode.

**WARNING**: These functions will not revert in prior versions of solidity.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#constant-functions-using-assembly-code)

### 37. Return values of low-level calls:

**BEST PRACTICE**: Check the *return values* of low-level calls (`call`/`callcode`/`delegatecall`/`send`/etc.) to avoid unexpected failures.

> Check the return value of `call()` to revert of failure

```solidity
(success, data) = externalContract.call{value: 1 ether}("")
require( success ); // revert tx if call fails
```

see [here](https://swcregistry.io/docs/SWC-104)

### 38. Account existence check for low-level calls:

**WARNING**: Low-level calls `call`/`delegatecall`/`staticcall` return `true` even if the account called is **non-existent** (per EVM design).

**BEST PRACTICE**: Account existence must be checked prior to calling if needed.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-calls)
