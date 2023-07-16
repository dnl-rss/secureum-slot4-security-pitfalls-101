
### 16. Private on-chain data:

**WARNING**: Marking variables `private` only means that it cannot be read by other smart contracts. It does not mean that the data cannot be read on-chain.

**BEST PRACTICE**: data intended to be secret should not be stored unencrypted in contract code or state but instead stored encrypted or off-chain.

> Storing each player's selected `number` in contract storage allows the second entrant to read the selection of the first, and thus manipulate the outcome

```solidity
/*
 * @source: https://gist.github.com/manojpramesh/336882804402bee8d6b99bea453caadd#file-odd-even-sol
 * @author: https://github.com/manojpramesh
 * Modified by Kaden Zipfel
 */

pragma solidity ^0.5.0;

contract OddEven {
    struct Player {
        address addr;
        uint number;
    }

    Player[2] private players;
    uint count = 0;

    function play(uint number) public payable {
            require(msg.value == 1 ether, 'msg.value must be 1 eth');
            players[count] = Player(msg.sender, number);
            count++;
            if (count == 2) selectWinner();
    }

    function selectWinner() private {
            uint n = players[0].number + players[1].number;
            (bool success, ) = players[n%2].addr.call.value(address(this).balance)("");
            require(success, 'transfer failed');
            delete players;
            count = 0;
    }
}
```

see [here](https://swcregistry.io/docs/SWC-136)

### 17. Weak PRNG:

PRNG = Probabilistic Random Number Generator

**WARNING**: PRNG relying on `block.timestamp`, `now` or `blockhash` can be influenced by miners to some extent and should be avoided.

> The `answer` generated from on-chain randomness is predictable

```solidity

/*
 * @source: https://capturetheether.com/challenges/lotteries/guess-the-random-number/
 * @author: Steve Marx
 */

pragma solidity ^0.4.21;

contract GuessTheRandomNumberChallenge {
    uint8 answer;

    function GuessTheRandomNumberChallenge() public payable {
        require(msg.value == 1 ether);
        answer = uint8(keccak256(block.blockhash(block.number - 1), now));
    }

    function isComplete() public view returns (bool) {
        return address(this).balance == 0;
    }

    function guess(uint8 n) public payable {
        require(msg.value == 1 ether);

        if (n == answer) {
            msg.sender.transfer(2 ether);
        }
    }
}
```

see [here](https://swcregistry.io/docs/SWC-120)

### 18. Block values as time proxies:

**WARNING**: `block.timestamp` and `block.number` are not good representations for time because of issues with synchronization, miner manipulation and changing block times.

> `block.number` is used to represent time in this contract

```solidity
/*
 * @author: Kaden Zipfel
 */

pragma solidity ^0.5.0;

contract TimeLock {
    struct User {
        uint amount; // amount locked (in eth)
        uint unlockBlock; // minimum block to unlock eth
    }

    mapping(address => User) private users;

    // Tokens should be locked for exact time specified
    function lockEth(uint _time, uint _amount) public payable {
        require(msg.value == _amount, 'must send exact amount');
        users[msg.sender].unlockBlock = block.number + (_time / 14);
        users[msg.sender].amount = _amount;
    }

    // Withdraw tokens if lock period is over
    function withdraw() public {
        require(users[msg.sender].amount > 0, 'no amount locked');
        require(block.number >= users[msg.sender].unlockBlock, 'lock period not over');

        uint amount = users[msg.sender].amount;
        users[msg.sender].amount = 0;
        (bool success, ) = msg.sender.call.value(amount)("");
        require(success, 'transfer failed');
    }
}
```

see [here](https://swcregistry.io/docs/SWC-116)

### 19. Integer overflow/underflow:

**WARNING** Not using OpenZeppelin’s `SafeMath` (or similar libraries) that check for overflows/underflows may lead to vulnerabilities or unexpected behavior if user/attacker can control the integer operands of such arithmetic operations.

**RESOLUTION**: ` solc v0.8.0` introduced default overflow/underflow checks for all arithmetic operations.

see [here](https://swcregistry.io/docs/SWC-101)
see [here](https://blog.soliditylang.org/2020/10/28/solidity-0.8.x-preview/)

### 20. Divide after multiply

**WARNING**: Division before multiplication can cause precision bugs.

**BEST PRACTICE**: Division after multiplication is better to avoid loss of precision due to truncation of the quotient.

see [here]()
