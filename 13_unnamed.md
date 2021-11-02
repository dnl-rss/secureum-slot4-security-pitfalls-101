### 95. Unprotected initializers in proxy-based upgradeable contracts:

**WARNING**: **Proxy-based upgradeable contracts** need to use `public` initializer functions, instead of constructors that need to be explicitly called only once.

**BEST PRACTICE**: Preventing multiple invocations of such initializer functions (e.g. via initializer modifier from OpenZeppelin’s Initializable library) is a must.

see [here](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#initializers)
and [here](https://github.com/crytic/slither/wiki/Upgradeability-Checks#initializer-is-not-called)

## 96. Initializing state-variables in proxy-based upgradeable contracts:

**BEST PRACTICE**: This should be done in `initializer` functions and not as part of the state variable declarations, in which case they won’t be set.

see [here](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#avoid-initial-values-in-field-declarations)

## 97. Import upgradeable contracts in proxy-based upgradeable contracts:

Contracts imported from **proxy-based upgradeable contracts** should **also be upgradeable** where such contracts have been modified to use initializers instead of constructors.

see [here](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#use-upgradeable-libraries)

### 98. Avoid selfdestruct or delegatecall in proxy-based upgradeable contracts:

This will cause the logic contract to be destroyed and all contract instances will end up delegating calls to an address without any code.

see [here](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#potentially-unsafe-operations)

### 99. State variables in proxy-based upgradeable contracts:

**BEST PRACTICE**: The declaration order/layout and type/mutability of state variables in such contracts should be preserved exactly while upgrading to prevent critical storage layout mismatch errors.

see [here](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#modifying-your-contracts)

### 100. Function ID collision between proxy/implementation in proxy-based upgradeable contracts:

**WARNING**: Malicious proxy contracts may **exploit function ID collision** to **invoke unintended proxy functions** instead of delegating to implementation functions.

**BEST PRACTICE**: Check for function ID collisions.

see [here](https://github.com/crytic/slither/wiki/Upgradeability-Checks#functions-ids-collisions)
and [here](https://forum.openzeppelin.com/t/beware-of-the-proxy-learn-how-to-exploit-function-clashing/1070)

### 101. Function shadowing between proxy/contract in proxy-based upgradeable contracts:

**WARNING**: Shadow functions in proxy contract prevent functions in logic contract from being invoked.

see [here](https://github.com/crytic/slither/wiki/Upgradeability-Checks#functions-shadowing)
