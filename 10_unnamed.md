### 48. Dangerous unary expressions:

**WARNING**: Unary expressions such as `x =+ 1` are likely errors where the programmer really meant to use `x += 1`.

**RESOLUTION**: Unary `+` operator was deprecated in `solc v0.5.0`.

see [here](https://swcregistry.io/docs/SWC-129)

### 49. Missing zero address validation:

Setters of `address` type parameters should include a **zero-address check** otherwise contract functionality may become **inaccessible** or **tokens burnt forever**.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation)

### 50. Critical address change:

**Changing critical addresses** in contracts should be a **two-step process** where:
- the **first transaction** (from the old/current address) **registers the new address** (i.e. grants ownership)
- the **second transaction** (from the new address) **replaces the old address** with the new one (i.e. claims ownership).

This gives an opportunity to recover from incorrect addresses mistakenly used in the first step.

If not, contract functionality might become inaccessible.

see [here](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/1488)
see [here](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2369)


### 51. assert()/require() state change:

**BEST PRACTICE**: Invariants in `assert()` and `require()` statements should **not modify the state**.

see [here](https://swcregistry.io/docs/SWC-110)

### 52. require() vs assert():

Used for:
- `require()` should be used for **checking error conditions on inputs and return values**.
- `assert()` should be used for **invariant checking**.

Between `solc 0.4.10` and `0.8.0`:
- `require()` used `REVERT` (`0xfd`) opcode which **refunded remaining gas on failure**  - `assert()` used `INVALID` (`0xfe`) opcode which **consumed all the supplied gas**

see [here](https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions)

### 53. Deprecated keywords:

**AVOID**: Use of deprecated functions/operators such as:
- `block.blockhash()` for `blockhash()`
- `msg.gas` for `gasleft()`
- `throw` for `revert()`
- `sha3()` for `keccak256()`
- `callcode()` for `delegatecall()`
- `suicide()` for `selfdestruct()`,
- `constant` for `view`
- `var` for actual type name

These may cause unintended errors with newer compiler versions.

see [here](https://swcregistry.io/docs/SWC-111)


### 54. Function default visibility:

Functions **without a visibility type** specifier were `public` by default in `solc < 0.5.0`.

This can lead to a vulnerability where a malicious user may make unauthorized state changes.

`solc >= 0.5.0` requires **explicit function visibility** specifiers.

see [here](https://swcregistry.io/docs/SWC-100)


### 55. Incorrect inheritance order:

Contracts inheriting from multiple contracts with identical functions should specify the correct inheritance order
- i.e. more general to more specific to avoid inheriting the incorrect function implementation.

see [here](https://swcregistry.io/docs/SWC-125)


### 56. Missing inheritance:

A `contract` might appear (based on name or functions implemented) to inherit from another `interface` or `abstract contract` without actually doing so.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#missing-inheritance)


### 57. Insufficient gas griefing:

**WARNING** Transaction relayers need to be trusted to provide enough gas for the transaction to succeed.

see [here](https://swcregistry.io/docs/SWC-126)


### 58. Modifying reference type parameters:

Structs/Arrays/Mappings passed as arguments to a function may be by value (`memory`) or reference (`storage`) as specified by the data location (optional before `solc 0.5.0`).

**BEST PRACTICE** Ensure **correct usage of memory and storage** in function parameters and **make all data locations explicit**.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#modifying-storage-array-by-value)


### 59. Arbitrary jump with function type variable:

**BEST PRACTICE**: `function` type variables should be **carefully handled** and **avoided in assembly** manipulations to prevent jumps to arbitrary code locations.

see [here](https://swcregistry.io/docs/SWC-127)


### 60. Hash collisions with multiple variable length arguments:

Using `abi.encodePacked()` with multiple variable length arguments can, in certain situations, lead to a hash collision.

**BEST PRACTICE**: Do not allow users access to parameters used in `abi.encodePacked()`, use fixed length arrays or use `abi.encode()`.

see [here](https://swcregistry.io/docs/SWC-133)
see [here](https://docs.soliditylang.org/en/v0.5.3/abi-spec.html#non-standard-packed-mode)
