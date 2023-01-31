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
