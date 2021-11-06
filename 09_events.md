### 45. Missing events:

**BEST PRACTICE**: emit events for critical state changes (e.g. `owner` and other critical parameters) for off-chain tracking

> updateOwner should emit an event

```solidity
contract C {

  modifier onlyAdmin {
    if (msg.sender != owner) throw;
    _;
  }

  function updateOwner(address newOwner) onlyAdmin external {
    owner = newOwner;
  }
}
```

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-access-control)

### 46. Unindexed event parameters:

**WARNING**: Parameters of certain events are expected to be `indexed` (e.g. ERC20 Transfer/Approval events) so that they’re included in the block’s bloom filter for faster access.

**WARNING**: Failure to mark event parameters as `indexed`, when expected, might confuse off-chain tooling looking for such events.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#unindexed-erc20-event-oarameters)

### 47. Incorrect event signature in libraries:

**BUG SUMMARY**: Contract types used in events in libraries cause an incorrect event signature hash

- Instead of using the type `address` in the hashed signature, the actual `contract` name was used, leading to a wrong hash in the logs.
- This is due to a compiler bug introduced in `v0.5.0` and fixed in `v0.5.8`.

```solidity
pragma solidity 0.5.0;
contract A {}

library test {
   event TEST(A e_a);
   function foo(A a) public {
      emit TEST(a);
   }
}
```

see [here](https://docs.soliditylang.org/en/v0.8.1/bugs.html)
