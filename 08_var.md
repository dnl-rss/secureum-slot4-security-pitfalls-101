### 39. Dangerous shadowing:

> substack post is unclear

**WARNING**: Local variables, state variables, functions, modifiers, or events with **names that shadow (i.e. override) builtin Solidity symbols** are misleading and may lead to unexpected usages and behavior.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#builtin-symbol-shadowing)

### 40. Dangerous state variable shadowing:

Shadowing state variables in derived contracts may be dangerous for critical variables such as contract `owner` and contract incorrectly use base/shadowed variables.
- e.g. modifiers in base contracts check on base variables but shadowed variables have been set mistakenly

**BEST PRACTICE**: Do not shadow state variables.

see [here](https://swcregistry.io/docs/SWC-119)

### 41. Pre-declaration usage of local variables:

**WARNING**: Usage of a variable before its declaration (either declared later or in another scope) leads to unexpected behavior in `solc < 0.5.0`

**BEST PRACTICE**: use`solc >= 0.5.0`, which implements **C99-style scoping rules** where variables can only be used after they have been declared and only in the same or nested scopes.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#pre-declaration-usage-of-local-variables)


### 42. Costly operations inside a loop:

**WARNING**: Operations such as state variable updates (using `SSTORE`) inside a loop cost a lot of gas, are expensive and may lead to out-of-gas errors.

**BEST PRACTICE**: Optimizations using local variables are preferred.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#costly-operations-inside-a-loop)


### 43. Calls inside a loop:

**WARNING**: Calls to external contracts inside a loop are dangerous (especially if the loop index can be user-controlled) because it could lead to DoS if one of the calls reverts or execution runs out of gas.

**BEST PRACTICE**: Avoid calls within loops, check that loop index cannot be user-controlled or is bounded.

see [here](https://swcregistry.io/docs/SWC-113)


### 44. DoS with block gas limit:

**WARNING**: Programming patterns such as **looping over arrays of unknown size** may lead to **DoS when the gas cost of execution exceeds the block gas limit**.

see [here](https://swcregistry.io/docs/SWC-128)
