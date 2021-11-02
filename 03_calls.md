### 12. Controlled delegatecall:

`delegatecall()` or `callcode()` to an `address` *controlled by the user* allows **execution of malicious contracts** in the context of the caller’s state.

**BEST PRACTICE**: Ensure *trusted destination* `address` for `delegatecall()` and `callcode()`.

> This proxy pattern is not secure if `callee` is Untrusted

```solidity
pragma solidity ^0.4.24;

contract Proxy {

  address owner;

  constructor() public {
    owner = msg.sender;  
  }

  function forward(address callee, bytes _data) public {
    require(callee.delegatecall(_data));
  }

}
```

see [here](https://swcregistry.io/docs/SWC-112)

### 13. Reentrancy vulnerabilities:

**Untrusted external contract calls** could **call back**, leading to unexpected results such as **multiple withdrawals** or **out-of-order events**.

**BEST PRACTICE**: Use check-effects-interactions pattern or reentrancy guards.

```solidity
/*
 * @source: http://blockchain.unica.it/projects/ethereum-survey/attacks.html#simpledao
 * @author: Atzei N., Bartoletti M., Cimoli T
 * Modified by Josselin Feist
 */
pragma solidity 0.4.24;

contract SimpleDAO {
  mapping (address => uint) public credit;

  function donate(address to) payable public{
    credit[to] += msg.value;
  }

  function withdraw(uint amount) public{
    // THIS CODE IS UNSECURE: REENTRANCY
    // check-interact-effect pattern implemented
    if (credit[msg.sender]>= amount) {
      require(msg.sender.call.value(amount)());
      credit[msg.sender]-=amount;
    }
  }  

  function queryCredit(address to) view public returns(uint){
    return credit[to];
  }
}
```

see [here](https://swcregistry.io/docs/SWC-107)

### 14. ERC777 callbacks and reentrancy:

`ERC777` tokens allow arbitrary callbacks via hooks that are called during token transfers.

**WARNING**: Malicious contract addresses may cause reentrancy on such callbacks if reentrancy guards are not used by `ERC777` hooks.

> The `tokensToSend` hook of `ERC777` is vulnerable to reentrancy

```solidity
interface ERC777TokensSender {
    function tokensToSend(
        address operator,
        address from,
        address to,
        uint256 amount,
        bytes calldata userData,
        bytes calldata operatorData
    ) external;
}
```

see [here](https://quantstamp.com/blog/how-the-dforce-hacker-used-reentrancy-to-steal-25-million)

### 15. Avoid transfer()/send() as reentrancy mitigations:

Although `transfer()` and `send()` have been recommended as a security best-practice to prevent reentrancy attacks because they only **forward 2300 gas**, the **gas repricing** of opcodes **may break deployed contracts**.

**BEST PRACTICE**: Use `call()` to send Ether instead, without hardcoded gas limits. Ensure **reentrancy protection** via **checks-effects-interactions pattern** or **reentrancy guards**.

> One should avoid hardcoded gas limits in calls, because hard forks may change opcode gas costs and break deployed contracts:

```solidity
/*
 * @author: Bernhard Mueller (ConsenSys / MythX)
 */

pragma solidity 0.6.4;

interface ICallable {
    function callMe() external;
}

contract HardcodedNotGood {

    address payable _callable = 0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa;
    ICallable callable = ICallable(_callable);

    constructor() public payable {
    }

    function doTransfer(uint256 amount) public {
        _callable.transfer(amount);
    }

    function doSend(uint256 amount) public {
        _callable.send(amount);
    }

     function callLowLevel() public {
         _callable.call.value(0).gas(10000)("");
     }

     function callWithArgs() public {
         callable.callMe{gas: 10000}();
     }
}
```

see [here](https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/)
see [here](https://swcregistry.io/docs/SWC-134)
