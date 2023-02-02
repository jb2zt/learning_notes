## Hello Ethernaut
- The password initialized with 'string public password' and then thrown in the constructor. Does that mean the plaintext version was created when I made a new instance when it was deployed, or when the contract itself was deployed, or what? 
It looks like it happens when the contract is created: https://solidity-by-example.org/constructor/
- Where is it encrypted then? In the bytecode for the contract itself or something?
Yes, in the bytecode: If you pass in the value as a constructor parameter, then the value can be seen in the contract creation transaction. Constructor arguments are just appended to the deployment bytecode. With the contract's ABI (not even the source code) it is possible to decode this and find the values.

## Fallback
- Mapping(key => value) looks like it's just a hash/js object? Any differences?
- msg.sender is the eoa/contract that's connecting with the contract
- modifiers change how a function works, for example to add a logic prerequisite
- the contribute() function doesn't take contribution amount as a param, instead it takes msg.value, so I need to 'fund' msg.value somehow?
- I don't get wy I'm not already the owner of the contract if in the constructor owner = msg.sender
    -- Similarly, why would the state variable contributions[] be giving me a different value than getContributions()
- I can't just start by funding the contract via the fallback function because it requires contributions[msg.sender] > 0. Need to make a contribution first, but how to fund that?
    -- So the contribute function is 'public payable' which means I can use simple payment object { to: , from: , value: } as a param when I call it: https://docs.alchemy.com/docs/solidity-payable-functions
- Then I just used the withdraw() function, which passed the level but why isn't there anything in the ether amount over here?: https://goerli.etherscan.io/tx/0xac90827be857d9c09a99ce067aaa7fed95a0c639297b4d594cf9a6d515f55a90

## Fallout

- It looks like the Fal1out() function is the only thing here that reassigns owner, so just send to that? IDK what the allocator() function is for in that case though
    -- I think the extra stuff is just to distract you. In this case you just grab ownership but funding the Fal1out() function

## Coinflip

NOTE: open a PR for this when you get to your other machine: https://github.com/OpenZeppelin/ethernaut/issues/558

- First glance seems obvious enough that I can just calculate blockValue in advance, just need to figure out how the solidity keywords work
- revert() is basically like require; it also returns any unused gas and aborts state changes. It sounds like generally you want to use require unless the logic is a bit more complex, in which case if you really can't simplify it you can use revert.
- While the actual vulnerability is trivial here, the help section says I might need to deploy an attack contract for this level. So why would that be?
