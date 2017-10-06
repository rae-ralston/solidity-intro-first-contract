# Deploy your First Contract Instructions
This goes through deploying your first contract web3 version 0.19.0 and the solc compiler.

## Instructions
1. **Prereqs:**
    Install node

    Install testrpc
      `npm install -g ethereumjs-testrpc`

    visit our [installfest doc](https://github.com/rachel-ftw/Basic-Ethereum-Dev-Env-Setup/blob/master/INSTALL.md) for more details on installing these two packages

1. Start testrpc. This operates like your `localhost`, it's your local development version of the ethereum blockchain

    ```bash
    $ testrpc
    ```

1. Open up a split pane. You'll need to make sure you're splitting the same sessionIf you're on iterm your session will split, if you're on bash you'll need to install tmux. Start up `testrpc` in one window & node repl in the other.

1. In a new terminal window, make a project directory and `cd` into it.

1. `$ touch package.json`
    ```js
    {
      "dependencies": {
        "solc": "^0.4.16",
        "web3": "0.19.0"
      }
    }
    ```

1. install the packages into your project
    `$ npm install`

1. run your node console
    `$ node`

1. Add the packages:
    ```js
    const Web3 = require('web3')
    const web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"))
    const solc = require('solc')
    ```

1. Check to make sure web3 is instantiated correctly and connected to localhost. If you get a list of 10 accounts back from `web3.eth.accounts`, then everything is good.
    ```js
    web3.eth.accounts
    let acct1 = web3.eth.accounts[0]
    let acct2 = web3.eth.accounts[1]
    ```

1. create a file called `HelloWorld.sol`
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
    ```

1. Use solc to compile the contract:

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
