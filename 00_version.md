### 1. Solidity versions:

**WARNING**: Using very *old versions* of Solidity *prevents benefits of bug fixes and newer security checks*.

**WARNING**: Using the *latest versions* might make contracts susceptible to *undiscovered compiler bugs*.

**BEST PRACTICE**: Use one of these versions: `0.7.5`, `0.7.6` or `0.8.4` for optimal tradeoffs between security and functionality.

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity)

### 2. Unlocked pragma:

The unlocked, or floating, pragma (`^` symbol) allows the Solidity file to be compiled within the same major solidity version in `solc ^0.5.10`

**WARNING**: Using the unlocked pragma means that different versions might be used for testing and development

**BEST PRACTICE**: Lock the pragma to ensure that contracts do not accidentally get deployed using an older compiler version, with unfixed bugs.

see [here](https://swcregistry.io/docs/SWC-103)

### 3. Multiple Solidity pragma:

**WARNING**: *Different versions* of solidity contain *different bugs and security checks*.

**BEST PRACTICE**: Use one Solidity compiler version across all contracts

see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used)
