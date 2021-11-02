### 4. Incorrect access control:

**BEST PRACTICE**: Contract functions executing **critical logic should have appropriate access control** enforced via address checks (e.g. owner, controller etc.) **typically in modifiers**.

Missing checks allow attackers to control critical logic.

see [here](https://docs.openzeppelin.com/contracts/3.x/api/access)
see [here](https://dasp.co/#item-2)

### 5. Unprotected withdraw function:

**AVOID**: **Unprotected** (external/public) **function calls sending Ether/tokens to user-controlled addresses may allow users to withdraw unauthorized funds**.

see [here](https://swcregistry.io/docs/SWC-105)

### 6. Unprotected call to selfdestruct:

**A user/attacker can mistakenly/intentionally kill the contract**.

**BEST PRACTICE**: Protect access to `selfdestruct` functions.

see [here](https://swcregistry.io/docs/SWC-106)

### 7. Modifier side-effects:

Modifiers should only **implement checks** and **not make state changes**.

**AVOID**: External calls in modifiers will typically **violate** the **checks-effects-interactions pattern**. These side-effects may go unnoticed by developers/auditors because the modifier code is typically far from the function implementation.

see [here](https://consensys.net/blog/blockchain-development/solidity-best-practices-for-smart-contract-security/)

> The modifier `isEligible` of the contracts below makes `Election` susceptible to a reentrancy attack via an external call to `registry`:

```solidity
contract Registry {
    address owner;

    function isVoter(address _addr) external returns(bool) {
        // Code
    }
}

contract Election {
    Registry registry;

    modifier isEligible(address _addr) {
        require(registry.isVoter(_addr));
        _;
    }

    function vote() isEligible(msg.sender) public {
        // Code
    }
}
```
### 8. Incorrect modifier:

**AVOID**: If a `modifier` does not execute `_` or `revert`, the function using that modifier will **return the default value** causing unexpected behavior.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-modifier)

**BEST PRACTICE**: All paths in a modifier should execute `_` or `revert`

> If the condition in `myModif` is false, the execution of `get()` returns 0. It is better to revert:

```solidity
modidfier myModif(){
    if(..){
       _;
    }
}
function get() myModif returns(uint){

}
```
