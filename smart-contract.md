
1. Prereqs:
  - install node
  - install testrpc
  - visit our [installfest repo](#) for more details on installing these two packages

1. [opcodes list](https://ethereum.stackexchange.com/questions/119/what-opcodes-are-available-for-the-ethereum-evm)

1. `$ touch package.json`
```js
{
  "dependencies": {
    "solc": "^0.4.16",
    "web3": "^1.0.0-beta.20"
  }
}
```

1. `$ npm install`

1. open up a split pane. If you're on iterm your session will split, if you're on bash you'll need to install tmux. Start up `testrpc` in one window & node repl in the other.

1. ```
const Web3 = require('web3')
const web3 = new Web3('http://localhost:8545')
web3.eth.getAccounts().then(console.log)
const solc = require('solc')
```

1. touch HelloWorld.sol
```
contract HelloWorld {
  function displayMessage() constant returns (string) {
    return "Hello from a smart contract";
  }
}
```

1. copy to your terminal and paste into a variable called source. This stringifies the contract content.
```
let source = `contract HelloWorld {
  function displayMessage() constant returns (string) {
    return "Hello from a smart contract";
  }
}`
> 'contract HelloWorld {\nfunction displayMessage() constant returns (string) {\nreturn "Hello from Smart contract";\n}\n}\n'
```

1. compile the contract
`let compiled = solc.compile(source)`

1. see the compiled contract & save into a variable
`let compiledContract = compiled.contracts[':HelloWorld']`

1. **see the EVM bytecode:** this is what our contracts compile to. When we deoploy to the network, this is what we're actually deploying!
`compiledContract.bytecode`

1. **see the EVM opcodes:** the opcodes map in a 1 to 1 relationship to the bytecode
`compiledContract.opcodes`

1. **see the ABI:** What's an abi?
`compiledContract.interface`

1. need the abi to do a contract, save it in JSON format
`let abi = JSON.parse(compiledContract.interface)`

1. instantiate the contract with web3
`let helloWorldContract = new web3.eth.Contract(abi)`
