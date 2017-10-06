# Deploy your First Contract Instructions
This goes through deploying your first contract on Web3 using their 1.0.0-beta.20 version.

## Instructions
1. Prereqs:
    - install node
    - install testrpc
      `npm install -g ethereumjs-testrpc`

    visit our [installfest doc](https://github.com/rachel-ftw/Basic-Ethereum-Dev-Env-Setup/blob/master/INSTALL.md) for more details on installing these two packages

1. Make a project directory and `cd` into it.

1. Create a `package.json` file
    ```js
    {
      "dependencies": {
        "solc": "^0.4.16",
        "web3": "^1.0.0-beta.20"
      }
    }
    ```

1. Install your project packages:

    `$ npm install`

1. Start up testrpc:

    `$ testrpc`

1. Open up a split pane. You'll need to make sure you're splitting the same sessionIf you're on iterm your session will split, if you're on bash you'll need to install tmux. Start up `testrpc` in one window & node repl in the other.

1. Add the packages
    ```js
    const Web3 = require('web3')
    const web3 = new Web3('http://localhost:8545')
    const solc = require('solc')
    ```

1. Check to make sure web3 is instantiated correctly and connected to localhost.
    ```js
    let accounts
    web3.eth.getAccounts().then(console.log)
    web3.eth.getAccounts().then(data => accounts = data)
    let acct1 = accounts[0]
    let acct2 = accounts[1]
    ```

1. Touch HelloWorld.sol
    ```js
    pragma solidity ^0.4.17;
    contract HelloWorld {
      function displayMessage() public pure returns (string) {
        return "Whale hallo there!";
      }
    }
    ```

1. Copy to your terminal and paste into a variable called source. This stringifies the contract content.
    ```js
    let source = `pragma solidity ^0.4.17;
    contract HelloWorld {
      function displayMessage() public pure returns (string) {
        return "Whale hallo there!";
      }
    }`
    ```

1. Compile the contract:

    `let compiled = solc.compile(source)`

1. See the compiled contract & save into a variable:

    `compiled = compiled.contracts[':HelloWorld']`

1. **See the EVM bytecode:** this is what our contracts compile to. When we deploy to the network, this is what we're actually deploying!

    `compiled.bytecode`

1. **See the EVM opcodes:** the opcodes map in a 1 to 1 relationship to the bytecode. For reference, here's a [opcodes list](https://ethereum.stackexchange.com/questions/119/what-opcodes-are-available-for-the-ethereum-evm).

    `compiled.opcodes`

1. **See the ABI:** Application Binary Interface. An API (Application Programming Interface) works for high level languages at a programming level, an ABI does the same thing but at a binary level. It's public facing and defines the details of our code.

    `compiled.interface`

1. Save the ABI into a variable.

    `let abi = JSON.parse(compiled.interface)`

1. Instantiate the contract with web3 so that we can deploy it.

    `let myContract = new web3.eth.Contract(abi)`

1. Deploy the contract:
    ```js
    let deployed = myContract.deploy({
      data: compiled.bytecode
    }).send({
      from: acct1,
      gas: 1000000,
      gasPrice: '5'
    }).then(contractInstance => {
      console.log(contractInstance.options)
      return contractInstance
    }).catch(e => console.error(e))
    ```

1. Check `testrpc` to see that that the contract went through. You should see a transaction id & a contract address. This was deployed to its own contract address.  investigate the transaction:

    `web3.eth.getTransaction('paste_in_your_tx_id').then(console.log)`

1. Let's run our function!

    ```js
    deployed.then(CI => {
      CI.methods.displayMessage().call().then(console.log)
    })
    ```
