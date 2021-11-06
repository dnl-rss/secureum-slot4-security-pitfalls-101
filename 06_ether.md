### 26. Unexpected Ether and this.balance:

A `contract` can **receive Ether** via:
- payable functions
- selfdestruct()
- coinbase transaction
- pre-sent before creation

**WARNING**: Contract logic depending on `this.balance` can therefore be manipulated.

**BEST PRACTICE**: Do not make assumptions regarding `this.balance` for contract logic.

> This contract executes logic based on `this.balance`, which can be manipulated by force sending Ether from a self-destructed contract

```solidity
pragma solidity ^0.4.21;

contract RetirementFundChallenge {
    uint256 startBalance;
    address owner = msg.sender;
    address beneficiary;
    uint256 expiration = now + 10 years;

    function RetirementFundChallenge(address player) public payable {
        require(msg.value == 1 ether);

        beneficiary = player;
        startBalance = msg.value;
    }

    ...

    function withdraw() public {
        require(msg.sender == owner);

        if (now < expiration) {
            // early withdrawal incurs a 10% penalty
            msg.sender.transfer(address(this).balance * 9 / 10);
        } else {
            msg.sender.transfer(address(this).balance);
        }
    }

    function collectPenalty() public {
        require(msg.sender == beneficiary);

        uint256 withdrawn = startBalance - address(this).balance;

        // an early withdrawal occurred
        require(withdrawn > 0);

        // penalty is what's left
        msg.sender.transfer(address(this).balance);
    }
}
```

see [here](https://github.com/sigp/solidity-security-blog#3-unexpected-ether-1)
see [here](https://swcregistry.io/docs/SWC-132)

### 27. fallback vs receive():

**BEST PRACTICE**: Check that all precautions and subtleties of fallback/receive functions related to visibility, state mutability and Ether transfers have been considered.  

see [here](https://docs.soliditylang.org/en/latest/contracts.html#fallback-function)
see [here](https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function)

### 28. Dangerous strict equalities:

**WARNING**: Use of strict equalities with tokens/Ether can accidentally/maliciously cause unexpected behavior.

**BEST PRACTICE**: Consider using ``>=`` or `<=` instead of `==` for such variables depending on the contract logic.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-strict-equalities)

### 29. Locked Ether:

**WARNING**: Contracts that accept Ether via `payable` functions but without withdrawal mechanisms will lock up that Ether.

**BEST PRACTICE**: Remove `payable` attributes, or add a withdraw function.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#contracts-that-lock-ether)
