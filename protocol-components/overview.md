# Overview

The main defining feature of the TON Liquid Staking protocol is independent modularity (encapsulation) that separates the functionality of the platform into independent interchangeable modules. This allows engineers to develop, test, and update each module individually as needed. Relatedly, the prevalence of basic independent modules allows for the assembly of the protocol in different ways for various tasks and utilities.

### Participants groups

TON Liquid Staking protocol participants consist of three main types. These include:

<figure><img src="../.gitbook/assets/macschemes-High-level-participants.drawio.svg" alt=""><figcaption></figcaption></figure>

_Governance_ - the most powerful nodes operating on TON Blockchain, Validators are participants that typically host their own hardware to provision their contribution to the network.

_Validators_ - participants with hardware for operation work.

_Nominators_ - participants who desire to share their funds for the pool.

<figure><img src="../.gitbook/assets/pool-3-Contract Managment.drawio (1).svg" alt=""><figcaption></figcaption></figure>

### Governance&#x20;

Governors are network participants that help carry out Validator and Nominator pool management on the protocol. One of their main roles is to track participant token liquidity in the TON Ecosystem using independent analytics based on data from TON Blockchain.

Governors are engaged in dApps, various analytics tools, improving the convenience for everyone else, instructions, improvements, etc.

Governors supports the following components within the Protocol:

Governance Individual components:

* Pool smart contract
* Governor smart contract
* Sudoer smart contract
* Halter smart contract
* Governor Multisignature Wallet or Governor DAO (optional)

{% hint style="info" %}
Governor is currently represented as a Multisignature Wallet but will be implemented as DAO as well.
{% endhint %}

* Interest Manager node

### Validators

Validators are the most powerful nodes operating on the TON Network. Their main utility on the TON Liquid Staking protocol is to provide hardware that allows the protocol and its underlying liquidity pools to function. Validator groups can be made up of one Validator or several Validators that alternate with each cycle. Validators can also provide hardware independently or a combination of hardware and capital. The TON Liquid Staking Protocol is designed to allow for the participation of validators that provide both hardware or capital, meaning the system is designed to be more equitable than other similar renditions (lowering the barrier to entry for Validators).

This group supports with following components of the Protocol:

* _Wallet smart contract_ (basic Ecosystem component) owned by Validator with a Private Key
* _Validator Controller smart contract_
* _Validator hardware and software (mytoncntrl)_

### Nominators

Users of Ecosystem which provides their funds in pools.

This group supports with following components of the Protocol:

* _Wallet_ Smart Contract (basic Ecosystem component) owned by User with a Private Key



### Common Components

* _Pool smart contract_ - central smart contract, which handles the whole TON LSt protocol workflow within other components.
* _Pool Jetton minter smart contract_ -  Pool Jetton master contract owned by Pool that manages minting Jetton Pool amounts for users when it requested by Pool contract.
* _Payouts Collection smart contract_ - Special NFT Collection contract for delayed deposit/withdrawal processing owned by Pool. There are two minters - one for Withdrawals, and one for Deposits.
* _Pool Jetton Wallet smart contract_ -  a liquid staking collateralized receipt token that is fully exchangeable for TON and represented by a Jetton Wallet Smart Contracts&#x20;
* _Payouts_ - Special tokens (as main version - NFT items), that are used for processing delayed payouts: deposits or withdrawals which will be processed at the end of the current round.
