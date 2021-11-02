### 1. Solidity versions:

Using very **old versions** of Solidity **prevents benefits of bug fixes and newer security checks**.

Using the **latest versions** might make contracts susceptible to **undiscovered compiler bugs**.

**BEST PRACTICE**: Consider using one of these versions: `0.7.5`, `0.7.6` or `0.8.4`.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity)

### 2. Unlocked pragma:

Contracts should be deployed using the **same compiler version/flags with which they have been tested**.

**BEST PRACTICE**: **Locking the pragma** (for e.g. by not using ^ in pragma solidity 0.5.10) ensures that contracts **do not accidentally get deployed using an older compiler version with unfixed bugs**.

see [here](https://swcregistry.io/docs/SWC-103)

### 3. Multiple Solidity pragma:

**Different versions** of solidity contain **different bugs and security checks**.

**BEST PRACTICE**: It is better to use **one Solidity compiler version across all contracts**

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used)
