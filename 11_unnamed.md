### 61. Malleability risk from dirty high order bits:

Types that **do not occupy the full 32 bytes** might contain “dirty higher order bits” which **does not affect operation on types** but gives **different results with `msg.data`**.

see [here](https://docs.soliditylang.org/en/v0.8.1/security-considerations.html#minor-details)


### 62. Incorrect shift in assembly:

Shift operators (`shl(x, y)`, `shr(x, y)`, `sar(x, y)`) in Solidity assembly apply the shift operation of `x` bits on `y` and not the other way around, which may be confusing.

Check if the values in a shift operation are reversed.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-shift-in-assembly)

### 63. Assembly usage:

Use of EVM assembly is error-prone and should be avoided or double-checked for correctness.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage)

### 64. Right-To-Left-Override control character (U+202E):

Malicious actors can use the Right-To-Left-Override `unicode` character to force RTL text rendering and confuse users as to the real intent of a contract.

`U+202E` character should not appear in the source code of a smart contract.

see [here](https://swcregistry.io/docs/SWC-130)

### 65. Constant state variables:

**BEST PRACTICE** Constant state variables should be declared `constant` to save gas.

see [here]()

### 66. Similar variable names:

**AVOID**: Variables with similar names could be confused for each other.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant)

### 67. Uninitialized state/local variables:

**WARNING**: Uninitialized state/local variables are assigned zero values by the compiler and may cause unintended results e.g. transferring tokens to zero address.

**BEST PRACTICE**: Explicitly initialize all state/local variables.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-state-variables)
see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-local-variables)

### 68. Uninitialized storage pointers:

**WARNING**: Uninitialized local storage variables can point to unexpected storage locations in the contract, which can lead to vulnerabilities.

**RESOLUTION**: `Solc >=0.5.0` disallows such pointers.

see [here](https://swcregistry.io/docs/SWC-109)

### 69. Uninitialized function pointers in constructors:

**WARNING**: Calling uninitialized function pointers in constructors of contracts compiled with solc versions `0.4.5`-`0.4.25` and `0.5.0`-`0.5.7` lead to unexpected behavior because of a compiler bug.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-function-pointers-in-constructors)

### 70. Long number literals:

**BEST PRACTICE**: Number literals with many digits should be carefully checked as they are prone to error.

see [here]()

### 71. Out-of-range enum:

**WARNING**: `Solc < 0.4.5` produced unexpected behavior with out-of-range enums.

**BEST PRACTICE**: Check enum conversion or use a newer compiler.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#too-many-digits)

### 72. Uncalled public functions:

**BEST PRACTICE**: Public functions that are never called from within the contracts should be declared `external` to save gas.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external)

### 73. Dead/Unreachable code:

**WARNING**: Dead code may be indicative of programmer error, missing logic or potential optimization opportunity, which needs to be flagged for removal or addressed appropriately.

see [here](https://en.wikipedia.org/wiki/Dead_code)

### 74. Unused return values:

**WARNING**: **Unused return values** of function calls are **indicative of programmer errors** which may have unexpected behavior.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#unused-return)

### 75. Unused variables:

**WARNING**: **Unused state/local variables** may be **indicative of programmer error**, **missing logic** or **potential optimization opportunity**, which needs to be flagged for removal or addressed appropriately.

see [here](https://swcregistry.io/docs/SWC-131)

### 76. Redundant statements:

**WARNING**: Statements with **no effects** that do not produce code may be indicative of programmer error or missing logic, which needs to be **flagged for removal** or **addressed appropriately**.

see [here](https://swcregistry.io/docs/SWC-135)
