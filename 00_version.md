### 1. Solidity versions:

**WARNING**: Using very **old versions** of Solidity **prevents benefits of bug fixes and newer security checks**.

**WARNING**: Using the **latest versions** might make contracts susceptible to **undiscovered compiler bugs**.

**BEST PRACTICE**: Consider using one of these versions: `0.7.5`, `0.7.6` or `0.8.4`.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity)

### 2. Unlocked pragma:

Contracts should be deployed using the **same compiler version/flags with which they have been tested**.

The `^` pragma symbols allows unlocked pragmas within the same major solidity version in `solc ^0.5.10`. 

**BEST PRACTICE**: **Locking the pragma** ensures that contracts **do not accidentally get deployed using an older compiler version with unfixed bugs**.

see [here](https://swcregistry.io/docs/SWC-103)

### 3. Multiple Solidity pragma:

**WARNING**: **Different versions** of solidity contain **different bugs and security checks**.

**BEST PRACTICE**: use **one Solidity compiler version across all contracts**

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used)
