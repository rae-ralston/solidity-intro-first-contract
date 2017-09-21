```js
function clear() {
  process.stdout.write('\u001B[2J\u001B[0;0f');
}
```
adds clear() to the repl


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

1. add the packages
```js
const Web3 = require('web3')
const web3 = new Web3('http://localhost:8545')
const solc = require('solc')
```

1. check to make sure web3 is instantiated correctly and connected to localhost.
```js
let accounts
web3.eth.getAccounts().then(console.log)
web3.eth.getAccounts().then(data => accounts = data)
let acct1 = accounts[0]
let acct2 = accounts[1]
```

1. touch HelloWorld.sol
```js
contract HelloWorld {
  function displayMessage() constant returns (string) {
    return "Hello from a smart contract";
  }
}
```

1. copy to your terminal and paste into a variable called source. This stringifies the contract content.
```js
let source = `contract HelloWorld {
  function displayMessage() constant returns (string) {
    return "Hello from a smart contract";
  }
}`
//> 'contract HelloWorld {\nfunction displayMessage() constant returns (string) {\nreturn "Hello from Smart contract";\n}\n}\n'
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
`let myContract = new web3.eth.Contract(abi, acct1, {gasPrice: '1', gasPrice: '5', from: acct2})`
sending to address (acct1): `myContract.options.address`
from address (acct2): `myContract.options.from`

1. deploy the contract
solidity online compiler
```js
let deployed = myContract.deploy({
  data: compiledContract.bytecode
  }).send({
    from: acct1,
    gas: 1000000,
    gasPrice: '5'
  }).then(contractInstance => console.log(contractInstance.options.address))
    // address of the contract:
    // 0x878dFCd4Bf551bdB5a993B9727A01987b717967b
```

1. check on test rpc that the contract went through and investigate transaction
`web3.eth.getTransaction('your_tx_id').then(console.log)`

1.
