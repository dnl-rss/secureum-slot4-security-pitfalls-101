### 12. Controlled delegatecall:

`delegatecall()` or `callcode()` to an **address controlled by the user** allows **execution of malicious contracts** in the context of the caller’s state.

**BEST PRACTICE**: Ensure **trusted destination addresses** for `delegatecall()` and `callcode()`.

see [here](https://swcregistry.io/docs/SWC-112)

### 13. Reentrancy vulnerabilities:

**Untrusted external contract calls** could **callback** leading to unexpected results such as **multiple withdrawals** or **out-of-order events**.

**BEST PRACTICE**: Use check-effects-interactions pattern or reentrancy guards.

see [here](https://swcregistry.io/docs/SWC-107)

### 14. ERC777 callbacks and reentrancy:

`ERC777` tokens allow arbitrary callbacks via hooks that are called during token transfers.

**WARNING**: Malicious contract addresses may cause reentrancy on such callbacks if reentrancy guards are not used by `ERC777` hooks.

see [here](https://quantstamp.com/blog/how-the-dforce-hacker-used-reentrancy-to-steal-25-million)

### 15. Avoid transfer()/send() as reentrancy mitigations:

Although `transfer()` and `send()` have been recommended as a security best-practice to prevent reentrancy attacks because they only **forward 2300 gas**, the **gas repricing** of opcodes **may break deployed contracts**.

**BEST PRACTICE**: Use `call()` instead, without hardcoded gas limits. Ensure **reentrancy protection** via **checks-effects-interactions pattern** or **reentrancy guards**.

see [here](https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/)
see [here](https://swcregistry.io/docs/SWC-134)
