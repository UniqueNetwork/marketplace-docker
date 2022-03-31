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

...

## Step X - Build and Run

Execute the following command in the terminal and wait for it to complete:

```
docker-compose up -d
```

## Step X - Enjoy


## License Information

`TODO`