# Deploy your First Contract Instructions
This goes through deploying your first contract on Web3 using their 1.0.0-beta.20 version.

once I'm in node I add a function to clear the console for convience
```js
function clear() {
  process.stdout.write('\u001B[2J\u001B[0;0f');
}
```

[Opcodes list](https://ethereum.stackexchange.com/questions/119/what-opcodes-are-available-for-the-ethereum-evm) for reference.

## Instructions
1. Prereqs:
    install node

    install testrpc
      `npm install -g ethereumjs-testrpc`

    visit our [installfest doc](https://github.com/rachel-ftw/Basic-Ethereum-Dev-Env-Setup/blob/master/INSTALL.md) for more details on installing these two packages

1. Start up testrpc

    install testrpc: `npm install -g ethereumjs-testrpc`

    start command `testrpc`

1. Make a project directory and `cd` into it.

1. `$ touch package.json`
    ```js
    {
      "dependencies": {
        "solc": "^0.4.16",
        "web3": "0.19.0"
      }
    }
    ```

1. `$ npm install`

1. Open up a split pane. If you're on iterm your session will split, if you're on bash you'll need to install tmux. Start up `testrpc` in one window & node repl in the other.

1. Add the packages
    ```js
    const Web3 = require('web3')
    const web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"))
    const solc = require('solc')
    ```

1. Check to make sure web3 is instantiated correctly and connected to localhost.
    ```js
    web3.eth.accounts
    let acct1 = web3.eth.accounts[0]
    let acct2 = web3.eth.accounts[1]
    ```

1. Touch HelloWorld.sol
    ```js
    contract HelloWorld {
      function displayMessage() constant returns (string) {
        return "Whale hello there!";
      }
    }
    ```

1. Copy to your terminal and paste into a variable called source. This stringifies the contract content.
    ```js
    let source = `contract HelloWorld {
      function displayMessage() constant returns (string) {
        return "Whale hello there!"; //REMEMBER SEMICOLON
      }
    }`
    //> 'contract HelloWorld {\nfunction displayMessage() constant returns (string) {\nreturn "Hello from Smart contract";\n}\n}\n'
    ```

1. Compile the contract:

    `let compiled = solc.compile(source)`

1. See the compiled contract & save into a variable:

    `let compiledContract = compiled.contracts[':HelloWorld']`

1. **see the EVM bytecode:** This is what our contracts compile to. When we deoploy to the network, this is what we're actually deploying!

    `compiledContract.bytecode`

1. **see the EVM opcodes:** The opcodes map in a 1 to 1 relationship to the bytecode.

    `compiledContract.opcodes`

1. **see the ABI:** Application Binary Interface. An API (App Programming Interface) works for high level languages at a programming level, an ABI does the same thing but at a binary level.

    `compiledContract.interface`

1. Need the abi to do a contract, save it in JSON format

    `let abi = JSON.parse(compiledContract.interface)`

1. Instantiate the contract with web3

    `let myContract = web3.eth.contract(abi)`

1. Deploy the contract
    ```js
    let contractInstance = myContract.new({
      from: acct1,
      data: compiledContract.bytecode,
      gas: 4700000,
      gasPrice: 5,
    })
    ```

1. Check on test rpc that the contract went through and investigate transaction

    `web3.eth.getTransaction('your_tx_id').then(console.log)`

1. You can see the address of the deployed contract

    `contractInstance.address`

1. you can call the function inside of the contract this way:

    ```js
    contractInstance.displayMessage.call()
    // > 'Whale hello there!!'
    ```
