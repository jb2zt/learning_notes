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

- First glance seems obvious enough that I can just calculate blockValue in advance, just need to figure out how the solidity keywords work
- revert() is basically like require; it also returns any unused gas and aborts state changes. It sounds like generally you want to use require unless the logic is a bit more complex, in which case if you really can't simplify it you can use revert.
- While the actual vulnerability is trivial here, the help section says I might need to deploy an attack contract for this level. So why would that be?
    -- Seems like this has to do with block timings and stuff, since you need to execute on the current block hash
- Followed [this guide](https://www.youtube.com/watch?v=VJZuLb1r1nQ&t=983s) because I didn't really understand how to deploy/interact with the level contract with my own contract
- The attack contract is pretty trivial; just duplicate the original contract
- In Remix, change the settings so that Environment is 'injected provider' (set to metamask)
    -- Deploy CoinFlipAttack to the contract address of your ethernaut level instance
    -- Remix now lists the callable functions from that contract, and when you hit flip() it will use your attack contract to flip (I need to draw this out because I don't totally understand the mechanics
    -- Verify that consectutive correct guesses is incrementing on the ethernaut instance
    
## Telephone
- I'm finding it hard to get a good mental picture of the interaction between the contract instance and a contract deployed at that address. I guess it's just that the attack contract imports the contract itself
- I tried just deploying a copy of telephone.sol and calling it with my player address, but that didn't work. I should check if it's getting caught in the conditional or something else is going on.
