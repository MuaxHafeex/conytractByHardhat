# Contract DEployment by Hardhat

This project demonstrates a basic Hardhat use case.

Pre-requisites​

```shell
    npm init --yes
    npm install --save-dev hardhat
    npm install @nomicfoundation/hardhat-toolbox  
    npx hardhat  
```
``` shell
after runnig npx hardhat, you will see

888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.10.1

√ What do you want to do? · Create a JavaScript project
√ Hardhat project root: ·  Project-Directory
√ Do you want to add a .gitignore? (Y/n) · y

Project created
```
``` shell
Create a Smart Contract and place it in contracts folder and rename it with token.sol
    Go to hardhat.config.js
```
``` shell
    require("@nomicfoundation/hardhat-toolbox");

    const { mnemonic } = require('./secrets.json');

    // This is a sample Hardhat task. To learn how to create your own go to
    // https://hardhat.org/guides/create-task.html
    task("accounts", "Prints the list of accounts", async () => {
    const accounts = await ethers.getSigners();

    for (const account of accounts) {
        console.log(account.address);
    }
    });

    // You need to export an object to set up your config
    // Go to https://hardhat.org/config/ to learn more

    /**
    * @type import('hardhat/config').HardhatUserConfig
    */
    module.exports = {
    defaultNetwork: "mainnet",
    networks: {
        localhost: {
        url: "http://127.0.0.1:8545"
        },
        hardhat: {
        },
        testnet: {
        url: "https://data-seed-prebsc-1-s1.binance.org:8545",
        chainId: 97,
        gasPrice: 20000000000,
        accounts: {mnemonic: mnemonic}
        },
        mainnet: {
        url: "https://bsc-dataseed.binance.org/",
        chainId: 56,
        gasPrice: 20000000000,
        accounts: {mnemonic: mnemonic}
        }
    },
    solidity: {
    version: "0.8.9",
    settings: {
        optimizer: {
        enabled: true
        }
    }
    },
    paths: {
        sources: "./contracts",
        tests: "./test",
        cache: "./cache",
        artifacts: "./artifacts"
    },
    mocha: {
        timeout: 20000
    }
    };
```

``` shell
NOTE:
                It requires mnemonic to be passed in for Provider, this is the seed phrase for the account you'd like to deploy from. Create a new `secrets.json` file in root directory and enter your 12 word mnemonic seed phrase to get started. To get the seedwords from metamask wallet you can go to Metamask Settings, then from the menu choose Security and Privacy where you will see a button that says reveal seed words.
```

``` shell
    Sample secrets.json

    {
        "mnemonic": "Your_12_Word_MetaMask_Seed_Phrase"
    }
```
``` shell
    npx hardhat compile
```

``` shell
    Copy and paste the following content into the scripts/deploy.js file.
```

``` shell
    async function main() {
    const [deployer] = await ethers.getSigners();

    console.log("Deploying contracts with the account:", deployer.address);

    console.log("Account balance:", (await deployer.getBalance()).toString());

    const Token = await ethers.getContractFactory("BEP20Token"); //Replace with name of your smart contract
    const token = await Token.deploy();

    console.log("Token address:", token.address);
    }

    main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exit(1);
    });

```
``` shell
    Run this command in root of the project directory:
```
``` shell
    npx hardhat run --network testnet scripts/deploy.js
```
``` shell
    Congratulations! You have successfully deployed BEP20 Smart Contract. Now you can interact with the Smart Contract.
```



