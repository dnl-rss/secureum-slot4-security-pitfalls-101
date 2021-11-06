### 21. Transaction order dependence:

**WARNING**: *Race conditions* can be forced on specific Ethereum transactions by *monitoring the mempool*. Transactions my be *front-run*, *back-run*, or *sandwich* attacked.

**EXAMPLE**: the classic `ERC20` `approve()` change can be *front-run*

**BEST PRACTICE**: Do not make assumptions about transaction order dependence.

> ERC20 approve() can be front-run, allowing the attacker (spender) to use first spend the old allowance, have it set by the pending approve() call, and then spend the new allowance.

```solidity
function approve(address spender, uint256 value) public returns (bool) {
  require(spender != address(0));

  _allowed[msg.sender][spender] = value;
  emit Approval(msg.sender, spender, value);
  return true;
}
```

see [here](https://swcregistry.io/docs/SWC-114)

### 22. ERC20 approve() race condition:

**BEST PRACTICE**: Use `safeIncreaseAllowance()` and `safeDecreaseAllowance()` from OpenZeppelin’s `SafeERC20` implementation to *prevent race conditions* from manipulating the allowance amounts.

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

A signature is composed of `(v, r, s)` components. The signature malleability is due to the `s` component, which may be right aligned or left aligned. The `ECDSA` library by OpenZeppelin forces the `s` component to use the lower order bits, mitigating the malleability.

**BEST PRACTICE**: Consider using OpenZeppelin’s `ECDSA` library.

see [here](https://swcregistry.io/docs/SWC-117)
see [here](https://swcregistry.io/docs/SWC-121)
see [here](https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57)

### 24. ERC20 transfer() does not return boolean:

**BUG SUMMARY**: The `ERC20` function `transfer()` implemented a **faulty interface** early in its development.
- `transfer()` should return `bool` success condition, per `ERC20` specs
- faulty `transfer()` implementations do not adhere to the specs, and do not return `bool`
- Contracts compiled with `solc >= 0.4.22` interacting with these faulty `transfer()` functions will `revert`.

**BEST PRACTICE**: Use OpenZeppelin’s `SafeERC20` wrappers.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc20-interface)
see [here](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca)

### 25. Incorrect return values for ERC721 ownerOf():

> substack post is unclear

**BUG SUMMARY**: The `ERC721` function `ownerOf()` may have a **faulty interface**.
- `ownerOf()` should return `address`, per `ERC721` specs
- faulty implementations of `ownerOf()` return `bool` instead of `address`
- contracts compiled with `solc >= 0.4.22` interacting with faulty `ownerOf()` implementations will `revert`.

**BEST PRACTICE**: Use OpenZeppelin’s `ERC721` contracts.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc721-interface)
