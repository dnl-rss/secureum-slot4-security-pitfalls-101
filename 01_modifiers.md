### 4. Incorrect access control:

**WARNING**: *Missing checks* allow attackers to control *critical logic*.

**BEST PRACTICE**: Contract functions executing *critical logic should have appropriate access control*, enforced via `address` checks **typically in modifiers**.
- e.g. owner, controller, admin checks

see [here](https://docs.openzeppelin.com/contracts/3.x/api/access)
see [here](https://dasp.co/#item-2)

### 5. Unprotected withdraw function:

**WARNING**: *Unprotected* (external/public) function calls *sending Ether/tokens* to user-controlled addresses may allow users to *withdraw unauthorized funds*.

> Integer overflow in the external calls of this contract results in Ether theft:

```solidity
/*
 * @source: https://capturetheether.com/challenges/math/token-sale/
 * @author: Steve Marx
 */

pragma solidity ^0.4.21;

contract TokenSaleChallenge {
    mapping(address => uint256) public balanceOf;
    uint256 constant PRICE_PER_TOKEN = 1 ether;

    function TokenSaleChallenge(address _player) public payable {
        require(msg.value == 1 ether);
    }

    function isComplete() public view returns (bool) {
        return address(this).balance < 1 ether;
    }

    function buy(uint256 numTokens) public payable {
        require(msg.value == numTokens * PRICE_PER_TOKEN);

        balanceOf[msg.sender] += numTokens;
    }

    function sell(uint256 numTokens) public {
        require(balanceOf[msg.sender] >= numTokens);

        balanceOf[msg.sender] -= numTokens;
        msg.sender.transfer(numTokens * PRICE_PER_TOKEN);
    }
}
```

see [here](https://swcregistry.io/docs/SWC-105)

### 6. Unprotected call to selfdestruct:

**WARNING**: A user/attacker can mistakenly/intentionally kill the contract if `selfdestruct` is not protected.

**BEST PRACTICE**: Protect access to `selfdestruct` functions or remove them.

> The parity mutli-sig smart contract wallet was killed when an attacker gained unauthorized ownership and called `kill()` function

```solidity
// kills the contract sending everything to `_to`.
function kill(address _to) onlymanyowners(sha3(msg.data)) external {
  suicide(_to);
}
```

see [here](https://swcregistry.io/docs/SWC-106)

### 7. Modifier side-effects:

**WARNING**: External calls in modifiers will typically **violate** the **checks-effects-interactions pattern**. These side-effects may go unnoticed by developers/auditors because the modifier code is typically far from the function implementation.

**BEST PRACTICE**: Modifiers should only **implement checks** and **not make state changes**.

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

see [here](https://consensys.net/blog/blockchain-development/solidity-best-practices-for-smart-contract-security/)

### 8. Incorrect modifier:

**WARNING**: If a `modifier` does not execute `_` or `revert`, the function using that modifier will *return the default value* causing unexpected behavior.

**BEST PRACTICE**: All paths in a modifier should execute `_` or `revert`

> If the condition in `myModif` is false, the execution of `get()` returns 0. It is better to revert:

```solidity
modidfier myModifier(){
    if( condition ){
       _; // if condition is true, function body executes
    }
    // otherwise, returns default value 0
}

function get() myModifier returns(uint){

}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-modifier)
