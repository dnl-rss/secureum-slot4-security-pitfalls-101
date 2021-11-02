### 21. Transaction order dependence:

**Race conditions** can be forced on specific Ethereum transactions by **monitoring the mempool**.

**EXAMPLE**: the classic `ERC20` `approve()` change can be **front-run**

**BEST PRACTICE**: Do not make assumptions about transaction order dependence.

see [here](https://swcregistry.io/docs/SWC-114)

### 22. ERC20 approve() race condition:

**BEST PRACTICE**: Use `safeIncreaseAllowance()` and `safeDecreaseAllowance()` from OpenZeppelin’s `SafeERC20` implementation to **prevent race conditions** from manipulating the allowance amounts.

see [here](https://swcregistry.io/docs/SWC-114)

### 23. Signature malleability:

**WARNING**: The `ecrecover` function is susceptible to **signature malleability** which could lead to **replay attacks**.

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
