<div align="center">
    <img src="/doc/logo-white.svg" alt="Unique White Label Market">
</div>

[![polkadotjs](https://img.shields.io/badge/polkadot-js-orange?style=flat-square)](https://polkadot.js.org)
[![uniquenetwork](https://img.shields.io/badge/unique-network-blue?style=flat-square)](https://unique.network/)
![GitHub Release Date](https://img.shields.io/github/release-date/uniquenetwork/unique-marketplace-frontend?style=flat-square)
![GitHub](https://img.shields.io/github/v/tag/uniquenetwork/unique-marketplace-frontend?style=flat-square)
![Docker Automated build](https://img.shields.io/docker/cloud/automated/uniquenetwork/marketplace-frontend?style=flat-square)
![language](https://img.shields.io/github/languages/top/uniquenetwork/unique-marketplace-frontend?style=flat-square)
![license](https://img.shields.io/badge/License-Apache%202.0-blue?logo=apache&style=flat-square)



# Marketplace Deployment

Who is this document for:


> * Full stack engineers
> * IT administrators

In this tutorial we will install the marketplace locally on a computer or in a virtual machine with Ubuntu OS. The process of installing it in a production environment is the same plus your IT administrator will need to setup the infrastructure (such as domain name, hosting, firewall, nginx, and SSL certificates) so that the server that hosts the marketplace can be accessed by the users on the Internet, like Unique marketplace: [https://unqnft.io](https://unqnft.io).

## Prerequisites

>  * OS: Ubuntu 18.04 or 20.04
>  * docker CE 20.10 or up
>  * docker-compose 1.25 or up
>  * git
>  * Google Chrome Browser

## Step 1 - Create Escrow Account

An escrow account is a substate address that manages the NFT and Kusama tokens put up for sale.
The easiest way to create an address is to use the extension [https://polkadot.js.org/extension/](https://polkadot.js.org/extension/). During the creation of the address, you will get 12-word mnemonic seed phrase, further called `ESCROW_SEED`. Do not share it with anybody because this phrase is all that’s needed to get access to the money and NFTs that are stored on this account.

## Step X - Start Configuring

From inside the root directory create `.env` and copy the content of the `.env.sample` there. Set `ESCROW_SEED` to the corresponding variable inside `.env`.

`TODO services inside` 

## Step X - Get QTZ

In order to get the marketplace running, your escrow address need some `TODO how much?` QTZ tokens. Now you can buy them on [MEXC Global](https://www.mexc.com/exchange/QTZ_USDT).


## Step X - Deploy Marketplace Smart Contract

There are two ways to put a token up for sale – at a fixed price and through an auction. All fixed price asks go throw special smart contract, which you can explore inside `unique-marketplace-api` project on github - https://github.com/UniqueNetwork/unique-marketplace-api/tree/release/v1.0/blockchain.

We also provide a special utility that is the easiest way to deploy your smart contract. In order to use it run the following script:

```
docker-compose up -d backend
docker exec backend node dist/cli.js playground deploy_contract
```

After a short interval you should get an operational summary output in the terminal:

```
...

SUMMARY:

CONTRACT_ETH_OWNER_SEED: '0x6d853337ab45b20aa5231c33979330e2806465fb4ab...'
CONTRACT_ADDRESS: '0x74C2d83b868f7E7B7C02B7D0b87C3532a06f392c'
```

Set the values above to the corresponding variables of `.env` file.

## Step X - Create Sponsored Collection

You may create collection for your marketplace using [Minter](https://minter-quartz.unique.network). When you create your collection you may find `collection id`

![Minter](./doc/Step6-0.png)

For now, EVM Marketplace can only work with sponsored collections. You may set sponsorship using [polkadot.js.org/apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fquartz.unique.network#/extrinsics) in 3 steps:

### 1. Set Collection Sponsor

- Choose `unique` - `setCollectionSponsor` method
- Set the collectionId parameter to the id of the previously created collection
- Set the escrow address created in step 1 as the new sponsor
- Click `Submit Transaction` and follow the instructions

### 2. Confirm Sponsorship

- Choose `unique` - `confirmSponsorship` method
- Set the escrow address created in step 1 as the transaction sender
- Set the collectionId parameter to the id of the previously created collection
- Click `Submit Transaction` and follow the instructions

### 3. Transfer QTZ to Sponsor

To sponsor EVM calls, you will need to transfer some QTZ to the ethereum mirror of your collection sponsor.

Use a built-in utility to get this address. For the script below, change `<COLLECTION_SPONSOR>` to the escrow address from the Step 1, and run it.
```
docker exec -ti backend node sub_to_eth.js <COLLECTION_SPONSOR>
```

The result will look like this:

```
Substrate address: 5EC3pKTxGj8ciFp37giawUY1B4aWTAU7aRRK8eA1J8SKNRsf
Substrate address balance: 9748981663000000000000
Ethereum mirror: 0x5e125Fd6aA7D06dEEd31475BcE293999a48015B0
Ethereum mirror balance: 0
Substrate mirror of ethereum mirror: 5C9rxShqs4vA3dxvesNUfPHRinWfwSeQAkHmaWbVzki84g1y
Substrate mirror of ethereum mirror balance: 0
```

Copy the `Substrate mirror of ethereum mirror` address and send some QTZ there. Now all ethereum transactions will be sponsored from this address.


## Step X - Check Configuration

Now you can check the configuration to make sure everything is set up.

```
docker-compose up -d backend
docker exec backend node dist/cli.js playground check_configuration
```

If everything is configured correctly, you will see a bunch of green checkboxes in the console, as shown below:

```
Checking CONTRACT_ADDRESS
[v] Contract address valid: 0x3c9931eA16D1048D7e22F3630844EC25eFD6B26f
[v] Contract balance is 40 tokens (40000000000000000000)
[v] Contract self-sponsoring is enabled
[v] Rate limit is zero blocks
[v] Contract owner valid, owner address: 0x3CA7393F1C8Df383c0f35d7BC1a5a938168c7d4b
Contract owner balance is 4 tokens (4492008910681246304)

Checking UNIQUE_COLLECTION_IDS
Collection #3
  [v] Sponsor is confirmed, yGGxcBQUCymdHtjQUdJDiXDTTXuonGv8HyRJiH5YDUcmfUyhr
  [v] Sponsor has 999999999948 tokens (999999999948126753000000000000) on its substrate wallet
  [v] Sponsor has 1000000000 tokens (1000000000000000000000000000) on its ethereum wallet
  [v] Transfer timeout is zero blocks
  [v] Approve timeout is zero blocks
```

Now you're almost done.
## Set Certificate

## Step X - Build and Run

Execute the following command in the terminal and wait for it to complete:

```
docker-compose up -d
```

## Step X - Check Configuration

## Step X - Enjoy

Open [localhost](http://localhost:80) in your Chrome browser. On the first launch you will see the Polkadot{.js}’s request to authorize the website, click “Yes”.

The marketplace will connect to the blockchain and the local backend and will display the empty Market page. It is now ready to rumble.

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
