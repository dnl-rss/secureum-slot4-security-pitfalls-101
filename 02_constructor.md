### 9. Constructor names:

**WARNING**: In `solc <0.4.22`, **constructor names** had to be the **same name as the `contract`**  containing it.
- Misnaming the constructor would make it a regular function (not a constructor).

**WARNING**: `solc ^0.4.22` introduced the `constructor` keyword. Contracts could have *both old-style and new-style constructor names*,
- with the first defined constructor would  taking precedence over the second if both existed, which also led to security issues.

**RESOLUTION**: Solc `0.5.0` forced the use of the `constructor` keyword.

> Multiple constructors could be defined for `solc ^0.4.22`, using both the old and new format

```solidity
contract A {
    uint x;
    constructor() public {
        x = 0;
    }
    function A() public {
        x = 1;
    }

    function test() public returns(uint) {
        return x;
    }
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#multiple-constructor-schemes)
see [here](https://swcregistry.io/docs/SWC-118)

### 10. Void constructor:

**WARNING**: Calls to *unimplemented base contract constructors* leads to *misplaced assumptions*.

**BEST PRACTICE**: Check if the *constructor is implemented*, or remove call if not.

> The constructor of `contract B` calls the unimplemented constructor of `A`

```solidity
contract A{}
contract B is A{
    constructor() public A(){}
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#void-constructor)

### 11. Implicit constructor callValue check:

**BUG SUMMARY**: The **creation code** of a `contract` that *does not define a `constructor()` but has a base that does*, did **not `revert`** for calls with non-zero `callValue` when such a constructor was not explicitly `payable`.

This is due to a compiler bug introduced in `v0.4.5` and fixed in `v0.6.8`.

**DESCRIPTION**:
- Starting from Solidity `0.4.5` the **creation code** of contracts without explicit `payable` constructor is supposed to contain a `callvalue` check that results in contract creation **reverting**, if non-zero value is passed.
- However, this **check was missing** if **no explicit constructor was defined** in a contract at all, but the contract has a base that does define a constructor.
- In these cases it is **possible to send value** in a **contract creation transaction** or using **inline assembly** without `revert`, even though the creation code is supposed to be non-payable.

see [here](https://docs.soliditylang.org/en/v0.8.9/bugs.html)
