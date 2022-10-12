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


Chapter 2, Day 3

1.

```
pub fun main() {
  var favoritePeople: [String] = ["Me", "Myself", "Jacob Tucker"]
  log(favoritePeople);
}
```

2.
```
pub fun main() {
  var socialMedia: {String: UInt64} = {"Facebook": 5, "Instagram": 6, "Twitter": 1, "YouTube": 2, "Reddit": 3, "LinkedIn": 4}
  log(socialMedia);
}
```

3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).

The operator ! "unwraps" an optional turning it its basic type, or directly aborting the program.

```
pub fun main(): Int {
    let thing: Int? = 5
    log(thing);
    return thing!
}
```

4. Using this picture below, explain...

What the error message means:

- Program is returning a String optional rather than a String

Why we're getting this error

- We declared that the function should a normal String

How to fix it

- Either change definition of function to return an optional, or "unwrap" any optional with the "!" extension: return thing[0x03]!


Chapter 2, Day 4

CONTRACT:

```
pub contract Authentication {

    pub var wardrobes: {Address: Wardrobe}
    
    pub struct Wardrobe {
        pub let hat: String
        pub let top: String
        pub let bottom: String
        pub let account: Address

        // You have to pass in 4 arguments when creating this Struct.
        init(_hat: String, _top: String, _bottom: String, _account: Address) {
            self.hat = _hat
            self.top = _top
            self.bottom = _bottom
            self.account = _account
        }
    }

    pub fun addWardrobe(hat: String, top: String, bottom: String, account: Address) {
        let newWardrobe = Wardrobe(_hat: hat, _top: top, _bottom: bottom, _account: account)
        self.wardrobes[account] = newWardrobe
    }

    init() {
        self.wardrobes = {}
    }

}
```

TRANSACTION:
```
import Authentication from 0x01

transaction(hat: String, top: String, bottom: String, account: Address) {

    prepare(signer: AuthAccount) {}

    execute {
        Authentication.addWardrobe(hat: hat, top: top, bottom: bottom, account: account)
    }
}
```

SCRIPT:
```
import Authentication from 0x01

pub fun main(account: Address): Authentication.Wardrobe {
    return Authentication.wardrobes[account]!
}
```

Chapter 3, Day 1

1. In words, list 3 reasons why structs are different from resources.

* Unlike structs, resources can't be copied.
* Unlike structs, resources can't be lost, without being intentionally destroyed.
* Unlike structs, resouces must be created with very intentional, specific syntax.

2. Describe a situation where a resource might be better to use than a struct.

A resource is useful when the data used is valuable and needs to be carefully tracked, for example with an expensive NFT.

3. What is the keyword to make a new resource?

"create" resource() 

4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?

No, there must be a function for the creation of a resource.

5. What is the type of the resource below?

The type is listed as a function.

6. pub resource Jacob {

}
Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // there is 1 here
        let myJacob <- create Jacob() // there are 2 here
        return <- myJacob // there is 1 here
    }
}

Chapter 3, Day 2

<pre>
pub contract Quest {

    pub resource Title {
        pub let text: String
        init() {
          self.text = "Placeholder"
        }
    }

    pub var arrayOfTitles: @[Title]

    pub fun addArrayTitle(title: @Title) {
        self.arrayOfTitles.append(<- title)
    }

    pub fun removeArrayTitle(index: Int): @Title {
        return <- self.arrayOfTitles.remove(at: index)
    }

    pub var dictionaryOfTitles: @{String: Title}

    pub fun addDictTitle(title: @Title) {
        let key = title.text
        let oldTitle <- self.dictionaryOfTitles[key] <- title
        destroy oldTitle
    }

    pub fun removeDictTitle(key: String): @Title {
        let title <- self.dictionaryOfTitles.remove(key: key)!
        return <- title
    }

    init() {
        self.arrayOfTitles <- []
        self.dictionaryOfTitles <- {}
    }

}
</pre>

Chapter 3, Day 3

1.
<pre>
pub contract Example {

    pub var dictionaryOfExampleResources: @{String: ExampleResource}

    pub resource ExampleResource {
        pub let text: String
        init(_text: String) {
            self.text = _text
        }
    }

    pub fun getReference(key: String): &ExampleResource? {
        return &self.dictionaryOfExampleResources[key] as &ExampleResource?
    }

    init() {
        self.dictionaryOfExampleResources <- {
                "One": <- create ExampleResource(_text: "A"), 
                "Two": <- create ExampleResource(_text: "B")
            }
    }

}
</pre>

2.
<pre>
import Example from 0x01

pub fun main() {
  var a = Example.getReference(key: "Two")
  log(a)
}
</pre>

3. References are useful when one would like to change or manipulate a resource without doing the costly and difficult tests associated with moving that resource into a function.

Chapter 3, Day 4:

1. To make sure that functions implement certain elements as requirements, and to gate the accessibility of functions to particular sources.

2. 

<pre>
pub contract Head {

    pub resource interface IGesture {
      pub var movementOne: String
    }

    pub resource Gesture: IGesture {
      pub var movementOne: String
      pub var movementTwo: String

      pub fun changeGestureOne(newGesture: String): String {
        self.movementOne = newGesture;
        return self.movementOne 
      }

      init() {
        self.movementOne = "Shake"
        self.movementTwo = "Nod"
      }
    }

    pub fun accessContent() {
      let gesture: @Gesture <- create Gesture()
      log(gesture.movementOne)

      destroy gesture
    }

    pub fun restrictContent() {
      let gesture: @Gesture{IGesture} <- create Gesture()
      log(gesture.movementTwo)

      destroy gesture
    }
}
</pre>

3. 

pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      
      pub fun changeGreeting(newGreeting: String): String // add to permit function call below 
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String  // ADD line
   
      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "Pineapple"; // add init, not completely need
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}

Chapter 3, Day 5

AREA 1:

Read: a, b, c, d
Write: a, b, c, d 

AREA 2:

Read: a, b, c
Write: a

AREA 3:

Read: a, b, c
Write: a

AREA 4:

Read: a, b, c
Write: a

publicFunc: All scope
 
contractFunc: All scope in contract
 
privateFunc: Only in current and inner scope

Chapter 4, Day 1

1. Explain what lives inside of an account.

An account holds both data storage and contract code, which can operate on data.

2. What is the difference between the /storage/, /public/, and /private/ paths?

Storage actually is located in /storage/, while /public/ and /private/ are paths that the owner can use to give access to data. /public/ is accessible to everyone, while /private/ is only accessible to the owner and other wallets that the owner chooses.

3. What does .save() do? What does .load() do? What does .borrow() do?

Save puts data in storage, while load take a resource from storage. Borrow, however, does not involve a resource and borrows a reference so that the data can be looked at (without being moved).

4. Explain why we couldn't save something to our account storage inside of a script.

Authentication is necessary to add to account storage. This is an obvious architecture decision for the blockchain.

5. Explain why I couldn't save something to your account.

You don't have the necessary authentication to save something, like a resource, to my account.

6. Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:

i. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.

<pre>
import Example from 0x01
transaction() {
  prepare(signer: AuthAccount) {
    let testResource <- Example.createTest()
    
    let accessResource = signer.save(<- testResource, to: /storage/MyTestResource) 

    let loadedResource <- signer.load<@Example.Test>(from: /storage/MyTestResource) 
                 ?? panic("Resource does not exist.")
    log(loadedResource.temple) 

    destroy loadedResource
  }

  execute {

  }
}
</pre>


ii. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

<pre>
import Example from 0x01
transaction() {
  prepare(signer: AuthAccount) {
    let testResource <- Example.createTest()
    let accessResource = signer.save(<- testResource, to: /storage/MyTestResource) 

    let borrowedResource = signer.borrow<&Example.Test>(from: /storage/MyTestResource) 
                 ?? panic("Resource does not exist.")
    log(borrowedResource.temple) 
  }

  execute {

  }
}
</pre>

Chapter 4, Day 2

1. What does .link() do?

The link function takes a resource, or function, in storage and exposes it to a particular path.

2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.

Resource interfaces can refine the accessible functions or elements of an object to make sure that only particular ones are exposed. The schema for the interface will define what is made available.

3. Deploy a contract that contains a resource that implements a resource interface. Then, do the following:

<pre>
pub contract Stuff {

  pub resource interface ITest {
    pub var name: String
  }

  // `Test` now implements `ITest`
  pub resource Test: ITest {
    pub var name: String
    pub var nickname: String

    pub fun changeName(newName: String) {
      self.name = newName
    }

    init() {
      self.name = "Todd"
      self.nickname = "Toad" 
    }
  }

  pub fun createTest(): @Test {
    return <- create Test()
  }

}
</pre>

i. In a transaction, save the resource to storage and link it to the public with the restrictive interface.
import Stuff from 0x01

transaction {

  prepare(signer: AuthAccount) {
  signer.save(<- Stuff.createTest(), to: /storage/MyTestResource)
  signer.link<&Stuff.Test{Stuff.ITest}>(/public/MyTestResource, target: /storage/MyTestResource)
  }

  execute {
  }
}

ii. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.

<pre>
import Stuff from 0x01

pub fun main(address: Address): String {
  let publicCapability: Capability<&Stuff.Test{Stuff.ITest}> =
    getAccount(address).getCapability<&Stuff.Test{Stuff.ITest}>(/public/MyTestResource)

  let testResource: &Stuff.Test{Stuff.ITest} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  // This works because `name` is in `&Stuff.Test{Stuff.ITest}`
  //log(testResource.name)
  log(testResource.nickname)
  return testResource.nickname
}
</pre>

iii. Run the script and access something you CAN read from. Return it from the script.

<pre>
import Stuff from 0x01

pub fun main(address: Address): String {
  let publicCapability: Capability<&Stuff.Test{Stuff.ITest}> =
    getAccount(address).getCapability<&Stuff.Test{Stuff.ITest}>(/public/MyTestResource)

  let testResource: &Stuff.Test{Stuff.ITest} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  log(testResource.name)
  return testResource.name
}
</pre>

Chapter 4, Day 3 

1. Why did we add a Collection to this contract? List the two main reasons.

Because otherwise we would have to keep track of storage paths, and they might conflict. A collection also creates a function that would allow other people to give us an NFT. (Additionally, collections may make NFTs easier to organize and keep track of.)

2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")

You must have a function named destory() that can destroy these nested resources.

Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.

We might want to restrict the circumstances in which someone could mint an NFT.

Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

We might have additional public facing function that reveal details about the collection?

Chapter 4, Day 4

<pre>
pub contract CryptoPoops {
  
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that defines the functions of an nft collection 
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

  // A resource for a nft collection
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

    // deposit an nft into the dictionary of nfts with the id of the token
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

    // withdraw an nft token according to the id
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // returns a list with all ids in the collection dictionary
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // returns a reference to the nft with the given id
    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }

    // initialize the collection as empty
    init() {
      self.ownedNFTs <- {}
    }

    // destroy all nfts in the collection
    destroy() {
      destroy self.ownedNFTs
    }
  }

  // returns an empty collection
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  // a new resource that mints nfts
  pub resource Minter {

    // mints an nft with the provided parameters of favourite food and lucky number
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    // returns a resource as the minter
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  // creates a minter resource within the storage of the account that created the contract
  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}

</pre>

Chapter 5, Day 1

1. An event is emitted messaged that can be sent within a function to others on the blockchain.

2.

<pre>
pub contract Example {

  pub event yo(message: String)

  pub fun greeting(_message: String) {
      emit yo(message: _message)
  }
</pre>

<pre>
import Example from 0x01

transaction {

  prepare(acct: AuthAccount) {}

  execute {
    Example.greeting(_message: "hey");
  }
}
</pre>

3. 

<pre>
pub contract Example {

  var lastmessage

  pub event yo(message: String)

  pub fun greeting(_message: String) {
      pre {
        _message != "": "No message"
      }
      emit yo(message: _message)
      post {
        _message == "end": "Program stopped due to end command." 
      }
  }
}
</pre>

4. 

<pre>
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  // YES, length is five!
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  // pre condition passed because string length greater then one, post condition
  // fails because doesn't acknowledge removal of last name
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.
    // Post condition fails, because the before number should less than result
    // after the function is run the first time, the self.number will increment to 1, but the
    // post condition will fail
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
</pre>

1. Explain why standards can be beneficial to the Flow ecosystem.

Standards ensure that certain types of contracts function as expected in a secure way. They strengthen the ecosystem because people can trust contracts, especially common contracts for things like NFTS.

2. What is YOUR favourite food?

Escargot!

Please fix this code (Hint: There are two things wrong):

The contract interface:

<pre>
pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff {
    pub var favouriteActivity: String
  }
}
</pre>
The implementing contract:

<pre>
// specify that it is implementing a standard
pub contract Test: Itest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    // should be newNumber
    self.number = newNumber
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
</pre>

1. What does "force casting" with as! do? Why is it useful in our Collection?

It downcasts the standard interface example to the specific type used in the contract. It is necessary to make sure that only the specific type of NFT intended for our collection will be entered or withdrawn.

2. What does auth do? When do we use it?

auth is needed for references (instead of direct examples of an interface). auth make sure that a reference is an authorized reference before it can be downcast to the specific type used in our NFT contract.

3.

0x01:

<pre>
import NonFungibleToken from 0x02

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
      return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
</pre>

0x02:

<pre>
/**
## The Flow Non-Fungible Token standard
*/

// The main NFT contract interface. Other NFT contracts will
// import and implement this interface
//

pub contract interface NonFungibleToken {

    pub var totalSupply: UInt64

    pub event ContractInitialized()

    pub event Withdraw(id: UInt64, from: Address?)

    pub event Deposit(id: UInt64, to: Address?)

    pub resource interface INFT {
        // The unique ID that each NFT has
        pub let id: UInt64
    }

    pub resource NFT: INFT {
        pub let id: UInt64
    }

    pub resource interface Provider {
        pub fun withdraw(withdrawID: UInt64): @NFT {
            post {
                result.id == withdrawID: "The ID of the withdrawn token must be the same as the requested ID"
            }
        }
    }

    pub resource interface Receiver {
        pub fun deposit(token: @NFT)
    }

    pub resource interface CollectionPublic {
        pub fun deposit(token: @NFT)
        pub fun getIDs(): [UInt64]
        pub fun borrowNFT(id: UInt64): &NFT
    }

    pub resource Collection: Provider, Receiver, CollectionPublic {

        // Dictionary to hold the NFTs in the Collection
        pub var ownedNFTs: @{UInt64: NFT}

        // withdraw removes an NFT from the collection and moves it to the caller
        pub fun withdraw(withdrawID: UInt64): @NFT

        // deposit takes a NFT and adds it to the collections dictionary
        // and adds the ID to the id array
        pub fun deposit(token: @NFT)

        // getIDs returns an array of the IDs that are in the collection
        pub fun getIDs(): [UInt64]

        // Returns a borrowed reference to an NFT in the collection
        // so that the caller can read data and call methods from it
        pub fun borrowNFT(id: UInt64): &NFT {
            pre {
                self.ownedNFTs[id] != nil: "NFT does not exist in the collection!"
            }
        }
    }

    pub fun createEmptyCollection(): @Collection {
        post {
            result.getIDs().length == 0: "The created collection must be empty!"
        }
    }
}
</pre>

Transaction 1: Create Collection

<pre>
import CryptoPoops from 0x01
transaction() {
  prepare(signer: AuthAccount) {
    // Store a `CryptoPoops.Collection` in our account storage.
    signer.save(<- CryptoPoops.createEmptyCollection(), to: /storage/MyCollection)
    
    // Link it to the public.
    signer.link<&CryptoPoops.Collection>(/public/MyCollection, target: /storage/MyCollection)
  }
}
</pre>

Transaction 2: Mint NFT to an Address

<pre>
import CryptoPoops from 0x01

transaction(recipient: Address) {

  // Let's assume the `signer` was the one who deployed the contract, since only they have the `Minter` resource
  prepare(signer: AuthAccount) {
    // Get a reference to the `Minter`
    let minter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
                    ?? panic("This signer is not the one who deployed the contract.")

    // Get a reference to the `recipient`s public Collection
    
    let recipientsCollection = getAccount(recipient).getCapability(/public/MyCollection)
                                  .borrow<&CryptoPoops.Collection>()
                                  ?? panic("The recipient does not have a Collection.")

    // mint the NFT using the reference to the `Minter`
    let nft <- minter.createNFT(name: "bob", favouriteFood: "chicken", luckyNumber: 6)

    // deposit the NFT in the recipient's Collection
    recipientsCollection.deposit(token: <- nft)
    
  }
}
</pre>
        
Script:
        
<pre>
import CryptoPoops from 0x01

pub fun main(recipient: Address, _id: UInt64) {

      let recipientsCollection = getAccount(recipient).getCapability(/public/MyCollection)
         .borrow<&CryptoPoops.Collection>()
         ?? panic("The recipient does not have a Collection.")

      log(recipientsCollection.getIDs());

      let nft = recipientsCollection.borrowAuthNFT(id: _id);

      log(nft.name)
      log(nft.favouriteFood)
      log(nft.luckyNumber);
}
</pre>
