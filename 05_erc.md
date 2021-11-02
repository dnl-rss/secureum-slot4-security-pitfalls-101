### 21. Transaction order dependence:

**WARNING**: *Race conditions* can be forced on specific Ethereum transactions by *monitoring the mempool*.

**EXAMPLE**: the classic `ERC20` `approve()` change can be *front-run*

**BEST PRACTICE**: Do not make assumptions about transaction order dependence.

> I dont understand this example

```solidity
/*
 * @source: https://github.com/ConsenSys/evm-analyzer-benchmark-suite
 * @author: Suhabe Bugrara
 */

pragma solidity ^0.4.16;

contract EthTxOrderDependenceMinimal {
    address public owner;
    bool public claimed;
    uint public reward;

    function EthTxOrderDependenceMinimal() public {
        owner = msg.sender;
    }

    function setReward() public payable {
        require (!claimed);

        require(msg.sender == owner);
        owner.transfer(reward);
        reward = msg.value;
    }

    function claimReward(uint256 submission) {
        require (!claimed);
        require(submission < 10);

        msg.sender.transfer(reward);
        claimed = true;
    }

```

see [here](https://swcregistry.io/docs/SWC-114)

### 22. ERC20 approve() race condition:

**BEST PRACTICE**: Use `safeIncreaseAllowance()` and `safeDecreaseAllowance()` from OpenZeppelin’s `SafeERC20` implementation to **prevent race conditions** from manipulating the allowance amounts.

```solidity
function safeIncreaseAllowance(
    IERC20 token,
    address spender,
    uint256 value
) internal {
    uint256 newAllowance = token.allowance(address(this), spender) + value;
    _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
}
```

see [here](https://swcregistry.io/docs/SWC-114)

### 23. Signature malleability:

**WARNING**: The `ecrecover` function is susceptible to *signature malleability* which could lead to *replay attacks*.

**BEST PRACTICE**: Consider using OpenZeppelin’s `ECDSA` library.

see [here](https://swcregistry.io/docs/SWC-117)
see [here](https://swcregistry.io/docs/SWC-121)
see [here](https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57)

### 24. ERC20 transfer() does not return boolean:

> substack post is unclear

**BUG SUMMARY**: The `ERC20` function `transfer()` implemented a **faulty interface** early in its development.
    - These faulty `transfer()` implementations do not return a `bool` success condition.
    - Contracts compiled with `solc >= 0.4.22` interacting with these faulty `transfer()` functions will `revert`.

**BEST PRACTICE**: Use OpenZeppelin’s `SafeERC20` wrappers.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc20-interface)
see [here](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca)

### 25. Incorrect return values for ERC721 ownerOf():

> substack post is unclear

Contracts compiled with `solc >= 0.4.22` interacting with `ERC721` `ownerOf()` that returns a `bool` instead of `address` type will `revert`.

**BEST PRACTICE**: Use OpenZeppelin’s `ERC721` contracts.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc721-interface)
