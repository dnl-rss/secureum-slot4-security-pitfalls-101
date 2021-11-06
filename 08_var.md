### 39. Dangerous shadowing:

**WARNING**: Older versions of solidity allow *shadowing of built in variables* (`now`, `assert`, etc) to override behavior

**RESOLUTION**: Newer versions of solidity disallow shadowing of built-in variables

**WARNING**: Local variables, state variables, functions, modifiers, or events with **names that shadow (i.e. override) builtin Solidity symbols** are misleading and may lead to unexpected usages and behavior.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#builtin-symbol-shadowing)

### 40. Dangerous state variable shadowing:

**WARNING**: Older versions of solidity allow *shadowing state variables in derived contracts*.
- this practice is confusing
- may be dangerous for critical variables such as contract `owner`
- e.g. modifiers in base contracts check on base variables but shadowed variables have been set mistakenly

**BEST PRACTICE**: Do not shadow state variables.

**RESOLUTION**: solidity `0.6.0` disallowed shadowing of state variables

> who own contract B?

```solidity
contract A {
  string owner = "me";
}
contract B is A {
  string owner = "you";
}
```

see [here](https://swcregistry.io/docs/SWC-119)

### 41. Pre-declaration usage of local variables:

**WARNING**: Usage of a variable before its declaration (either declared later or in another scope) leads to unexpected behavior in `solc < 0.5.0`

**BEST PRACTICE**: use`solc >= 0.5.0`, which implements **C99-style scoping rules** where variables can only be used after they have been declared and only in the same or nested scopes.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#pre-declaration-usage-of-local-variables)


### 42. Costly operations inside a loop:

**WARNING**: Operations such as state variable updates (using `SSTORE`) inside a loop costs a lot of gas, are expensive and may lead to out-of-gas errors.

**BEST PRACTICE**: Optimizations using local memory variables are preferred.

```solidity
contract CostlyOperationsInLoop{

    uint loop_count = 100;
    uint state_variable=0;

    function bad() external{
        for (uint i=0; i < loop_count; i++){
            state_variable++;
        }
    }

    function good() external{
      uint local_variable = state_variable;
      for (uint i=0; i < loop_count; i++){
        local_variable++;
      }
      state_variable = local_variable;
    }
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#costly-operations-inside-a-loop)


### 43. Calls inside a loop:

**WARNING**: Calls to external contracts inside a loop are dangerous (especially if the loop index can be user-controlled) because it could lead to DoS if one of the calls reverts or execution runs out of gas.

**BEST PRACTICE**: Avoid calls within loops, check that loop index cannot be user-controlled or is bounded.

```solidity
/*
 * @source: https://consensys.github.io/smart-contract-best-practices/known_attacks/#dos-with-unexpected-revert
 * @author: ConsenSys Diligence
 * Modified by Bernhard Mueller
 */

pragma solidity 0.4.24;

contract Refunder {

    address[] private refundAddresses;
    mapping (address => uint) public refunds;

    // ...

    // bad
    function refundAll() public {
        for(uint x; x < refundAddresses.length; x++) { // arbitrary length iteration based on how many addresses participated
            require(refundAddresses[x].send(refunds[refundAddresses[x]])); // doubly bad, now a single failure on send will hold up all funds
        }
    }

}

```

see [here](https://swcregistry.io/docs/SWC-113)


### 44. DoS with block gas limit:

Previously, blocks had a hard gas limit of 15 million gas units. EIP1559 changed this, but blocks still are limited in the amount of gas they can consume.

**WARNING**: Programming patterns such as *looping over arrays of unknown size* may lead to DoS when the *gas cost of execution exceeds the block gas limit*.

**BEST PRACTICE**: Bound loop iterations to a finite number

```solidity
pragma solidity ^0.4.25;

contract DosGas {

    address[] creditorAddresses;
    bool win = false;

    function emptyCreditors() public {
        if(creditorAddresses.length>1500) {
            creditorAddresses = new address[](0);
            win = true;
        }
    }

    function addCreditors() public returns (bool) {
        for(uint i=0;i<350;i++) {
          creditorAddresses.push(msg.sender);
        }
        return true;
    }

    function iWin() public view returns (bool) {
        return win;
    }

    function numberCreditors() public view returns (uint) {
        return creditorAddresses.length;
    }
}
```

see [here](https://swcregistry.io/docs/SWC-128)
