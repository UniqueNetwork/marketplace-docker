# Marketplace Deployment

Who is this document for:

  * Full stack engineers
  * IT administrators

In this tutorial we will install the marketplace locally on a computer or in a virtual machine with Ubuntu OS. The process of installing it in a production environment is the same plus your IT administrator will need to setup the infrastructure (such as domain name, hosting, firewall, nginx, and SSL certificates) so that the server that hosts the marketplace can be accessed by the users on the Internet, like Unique marketplace: [https://unqnft.io](https://unqnft.io).

## Prerequisites

  * OS: Ubuntu 18.04 or 20.04
  * docker CE 20.10 or up
  * docker-compose 1.25 or up
  * git
  * Google Chrome Browser

## Step 1 - Install Polkadot{.js} Extension

Visit [https://polkadot.js.org/extension/](https://polkadot.js.org/extension/) and click on the “Download for Chrome” button. Chrome browser will guide you through the rest of the process.

![Install Polkadot{.js} Extension](/doc/step1-1.png)

As a result you should see that little icon in the top right corner:

![Install Polkadot{.js} Extension](/doc/step1-2.png)


## Step 2 - Create Admin Address

Click on the Polkadot{.js} extension icon and select “create new account” in the menu:

<img src="/doc/step2-1.png" width="400">

You should write down the 12-word mnemonic seed on the paper. Do not share it with anybody because this 12-word phrase is all that’s needed to get access to the money and NFTs that are stored on this account.

Follow the Polkadot{.js} instructions to complete the account setup.

## Step 3 - Get Unique

In order to get the marketplace running, you’ll need some Unique coins. For the TestNet 2.0, it is free. You can get it from the faucet bot on Telegram: [@unique2faucetbot]

Copy your account address from Polkadot{.js} extension and send it to the faucet bot:

<img src="/doc/step3-1.png" width="400">

## Step 4 - Deploy Marketplace Smart Contract

1. Download [matcher.wasm](/doc/matcher.wasm) and [metadata.json](/doc/metadata.json) files

2. Open Polkadot Apps UI on the Contracts page: (https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ftestnet2.unique.network#/contracts)[https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ftestnet2.unique.network#/contracts]

![Deploy Marketplace Smart Contract](/doc/step4-1.png)

3. Click on Upload & deploy code button, select metadata.json and then matcher.wasm files you have downloaded previously in the form fields like this and click “Next”:

![Deploy Marketplace Smart Contract](/doc/step4-2.png)

4. Give the contract 100 Unique coins in Endowment so that it can pay for storing its data, click Deploy (and follow signing transaction):

![Deploy Marketplace Smart Contract](/doc/step4-3.png)

5. When the transaction completes, you should see the green notification bar on the right top and the contract will appear in the “contracts” list:

![Deploy Marketplace Smart Contract](/doc/step4-4.png)

6. Expand “Messages” section and find “SetAdmin” method:

![Deploy Marketplace Smart Contract](/doc/step4-5.png)

7. Click on the “exec” button in front of the setAdmin method and select the marketplace admin address both as the caller (“call from account”) and the parameter (“message to send”). Click Execute button and follow with signing this transaction:

![Deploy Marketplace Smart Contract](/doc/step4-6.png)

8. Click on the matcher contract ornament to copy its address for future use:

![Deploy Marketplace Smart Contract](/doc/step4-7.png)

You’re all set with the matcher contract!

## Step 5 - Clone marketplace code from GitHub

Open the terminal and execute the following command:

```
git clone https://github.com/UniqueNetwork/marketplace-docker
cd marketplace-docker
git checkout feature/easy_start
git submodule update --init --recursive --remote
```

## Step 6 - Configure backend (.env file)

1. Create .env file in the root of marketplace-docker project and paste the following content in there:

```
POSTGRES_DB=marketplace_db
POSTGRES_USER=marketplace
POSTGRES_PASSWORD=12345
ADMIN_SEED=
MarketplaceUniqueAddress=
MatcherContractAddress=
UniqueEndpoint=wss://testnet2.uniquenetwork.io
```

2. Edit the .env file:
  * Change ADMIN_SEED to the 12-word admin mnemonic seed phrase that you have saved when you created the admin address in Polkadot{.js} extension
  * Change MarketplaceUniqueAddress value to the address that you have copied from Polkadot{.js} extension:

    <img src="/doc/step6-1.png" width="200">

  * Change MatcherContractAddress value to the Matcher contract address that you have copied from Apps UI after you have deployed it:

  ![Deploy Marketplace Smart Contract](/doc/step6-2.png)

  * Leave the rest of values intact

As a result you should see a similar content to this:

    <img src="/doc/step6-3.png" width="400">

## Step 7 - Configure frontend (.env file)

In this step we will configure the marketplace frontend with your administrator and the matcher contract addresses, specify what NFT collections you’d like your marketplace to handle, and specify the domain name that it’s going to be hosted on (localhost for the purpose of this example).

1. Create an empty .env file in the ui/packages/apps folder and copy the following content in there:

```
CAN_ADD_COLLECTIONS=false
CAN_CREATE_COLLECTION=false
CAN_CREATE_TOKEN=false
CAN_EDIT_COLLECTION=false
CAN_EDIT_TOKEN=false
COMMISSION=10
CONTRACT_ADDRESS=''
DECIMALS=6
ESCROW_ADDRESS=''
FAVICON_PATH='favicons/marketplace'
KUSAMA_DECIMALS=12
MAX_GAS=1000000000000
MIN_PRICE=0.000001
MIN_TED_COLLECTION=1
QUOTE_ID=2
SHOW_MARKET_ACTIONS=true
VALUE=0
VAULT_ADDRESS=""
WALLET_MODE=false
WHITE_LABEL_URL='http://localhost'
UNIQUE_COLLECTION_IDS=23,25
UNIQUE_API='http://localhost:5000'
UNIQUE_SUBSTRATE_API='wss://testnet2.uniquenetwork.io'
```

2. Change the value of CONTRACT_ADDRESS to the address of the smart contract that you copied and saved after it’s been deployed
3. Change the value of ESCROW_ADDRESS to the admin address that you have copied from Polkadot{.js} extension
4. List the collections you would like the marketplace to handle in UNIQUE_COLLECTION_IDS field separated by command (e.g. this example above will configure the marketplace to handle collection 23, Substrapunks, and 25, Chelobricks).

## Step 8 - Build and run

**Optional**: You can pre-pull docker images before you start:

```
docker pull postgres:latest
docker pull node:latest
docker pull ubuntu:18.04
```

Execute the following command in the terminal and wait for it to finish:

```
docker-compose -f docker-compose-local.yml up -d --build
```

## Step 9 - Enjoy!

Open [http://localhost:3000] in your Chrome browser. On the first launch you will see the Polkadot{.js}’s request to authorize the website, click “Yes”:

![Deploy Marketplace Smart Contract](/doc/step9-1.png)

The marketplace will connect to the blockchain and the local backend and will display the empty Market page. It is now ready to play:

![Deploy Marketplace Smart Contract](/doc/step9-2.png)


## License Information

Copyright 2021, Unique Network, Usetech Professional

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
