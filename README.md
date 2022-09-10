# quest-submission

Chapter 1, Day 1

Explain what the Blockchain is in your own words. You can read this to help you, but you don't have to: https://www.investopedia.com/terms/b/blockchain.asp

* The blockchain is a form of distributed computer with both storage and compute resources that can be read freely and written to at cost. Its operation, performed and stored in a series of blocks, is implemented and secured by nodes and miners around the world.

Explain what a Smart Contract is. You can read this to help you, but you don't have to: https://www.ibm.com/topics/smart-contracts

* A smart contract is computer code that defines operations for data stored on the blockchain.

Explain the difference between a script and a transaction.

* A script can merely read data from a blockchain, while a transaction implies modification or writing to the blockchain. A script is generally free while a transaction costs coinage.

Chapter 1, Day 2

What are the 5 Cadence Programming Language Pillars?

Safety and Security (language encourages secure and safe code); Clarity (language is constructed to make operations clear and apparent); Approachability (language is consistent with other languages, especially in their best practice); Developer Experience (language is designed to produce a quality and enjoyable experience for developers); Resource Oriented Programming (language has a unique model built around scarce resources in computing).

In your opinion, even without knowing anything about the Blockchain or coding, why could the 5 Pillars be useful (you don't have to answer this for #5)?

These pillars give cadence an advantage over alternatives used on other chains, which are often hard to program and read, and which aren't necessarily designed around what has been common usages and demands for the blockchain.


Chapter 2, Day 1

![Image](/Screen Shot 2022-09-09 at 4.40.16 PM.png "image")

```

pub contract JacobTucker {

    pub let is: String

    init() {
        self.is = "The Best!"
    }
}
```


```
import JacobTucker from 0x03

pub fun main(): String {
    return JacobTucker.is
}

```

Please forgive lack of screenshot if that's ok. I wouldn't cheat:
```
17:01:43 
Script 
Result
{"type":"String","value":"The Best!"}
```


Chapter 2, Day 2


Explain why we wouldn't call changeGreeting in a script.

-- Scripts cannot change blockchain data.

What does the AuthAccount mean in the prepare phase of the transaction?

-- It approves a transaction and lets the transaction use data in a given account.

What is the difference between the prepare phase and the execute phase in the transaction?

-- The prepare phase accesses data in an account, whereas the execute phase uses this data or the results of the prepare transaction to call other functions on the blockchain.

This is the hardest quest so far, so if it takes you some time, do not worry! I can help you in the Discord if you have questions.

Add two new things inside your contract:

A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber

```
pub contract HelloWorld {

    pub var greeting: String
    pub var myNumber: Int

    pub fun changeGreeting(newGreeting: String) {
        self.greeting = newGreeting
    }

    pub fun updateMyNumber(newNumber: Int){
        self.myNumber = newNumber
    }

    init() {
        self.greeting = "Hello, World!"
        self.myNumber = 0
    }
}
```

Add a script that reads myNumber from the contract

```
import HelloWorld from 0x01

pub fun main(): Int {
    return HelloWorld.myNumber
}
```

Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

```
import HelloWorld from 0x01

transaction(myNewNumber: Int) {

  prepare(signer: AuthAccount) {}

  execute {
    HelloWorld.updateMyNumber(newNumber: myNewNumber)
  }
}
```
