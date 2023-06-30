# Description

### Introduction

Pool smart contracts are a central component of asset lending on the TON Liquid Staking protocol.

### Pool&#x20;

#### Definition and function

The _Pool (Pool Central Contract)_ is the main driving architectural framework of the TON LSt protocol and the main arbiter that lends assets to Controllers and aggregates profit/loss information from them.

#### Interaction with Controllers

The Pool Contract caters to Controllers.&#x20;

_Controllers_ (_Validator Controllers_) by lending assets according to the Current Rate when a borrowing request is received.&#x20;

{% hint style="info" %}
#### Validator Election

Validators participate in regular validator elections via a Controller smart contract which manages funds from the liquidity pool while ensuring that lent assets are secure.
{% endhint %}

#### Interaction with Stakers (also Users, Nominators)

_Stakers_ represent regular users - holders of TON funds. The Pool contract is designed to safely manage Stakers' deposits and withdrawals.

#### Interaction with Interest Manager

The Pool is continuously connected with the Interest Manager and helps provision important data for the protocol.&#x20;

The Pool forwards a share of the profits to the Interest Manager and updates deposit parameters and roles upon request.

The _Interest Manager_ dispatches aggregate lending statistics and modifies interest rates upon receiving a request from the Interest Manager.

#### Interaction with Governor

Governor helps provide equitable governance of the TON LSt Protocol. Because of this fact, Governor is in continuous communication with the Pool Central Contract.&#x20;

To provision the equitability of the liquid staking protocol, TONs Decentralized Autonomous Organization (DAO) helps decide key parameters (such as setting protocol fees, interest rates, and more) through governance voting. This framework leverages the voting power of governance token (jetton) holders to make protocol and ecosystem decisions based on voting weight.&#x20;



{% hint style="info" %}
To provision the equitability of the liquid staking protocol, TONs Decentralized Autonomous Organization (DAO) helps decide key parameters (such as setting protocol fees, interest rates, and more) through governance voting. This framework leverages the voting power of governance token (jetton) holders to make protocol and ecosystem decisions based on voting weight.&#x20;

The DAO also accumulates service fees from Stakers which are used to allocate funds towards protocol research, protocol and ecosystem development, liquidity incentives, and protocol upgrades.

Voting with special jettons of LSD.
{% endhint %}

### Pool Jettons

Pool Jettons represent a collateralized asset Stakers receive in exchange for depositing TON into the TON Liquid Staking protocol. Pool Jettons are used to provision the management of user assets lent to specific liquidity pools within the protocol.&#x20;

### Payouts (Bills)

Deposits and withdrawals serve as the lifeblood of asset transactions on the protocol. While employing strict mode we presume that the Pool Jetton to TON ratio is unknown until the consensus round ends, postponing the actual transactions. However, the optimistic mode can be used as long as measures are in place to mitigate misbehaving validators.



### Governance Components (Roles)

#### Halters

Halters act as an emergency safeguard for the TON Liquid Staking protocol and are capable of halting the protocol's operations in case of major system issues.

It is a role, which needs to be a hot wallet (private key stored in memory). It is a special role needed only during initial deployment while audits and practice still didn't prove 100% safety. In the long run, this role should be disembodied.&#x20;

#### Sudoers

While initially empty, Sudoers gain the ability to send arbitrary messages from various parts of the system.&#x20;

It is a special role that is needed only in emergency situation or for critical updates. This mechanism should be used for fixing bugs and vulnerabilities. This role should be empty most of the time. There is also a quarantine after setting this role to protect against hostile transfer(expected to be 24 hours).

#### Approvers

Approvers grant approvals to Controllers when they initiate an in-protocol borrowing request to ensure overall system integrity and compliance.

It decides and enforces whether Pool validators follow decentralization and safety measures set by Pool Governance, and thus can be simple wallet / multisignature in the long run.&#x20;

#### Interest Managers

Interest Managers receive validator round statistics and update interest parameters to enable dynamic interest rate management (changes in interest rates that will be available for loans).

Interest Manager can start as a simple wallet as well. However, provided that this actor receives all necessary information on-chain, in the future it is expedient to pass the Interest manager role to an automatic smart contract.&#x20;

#### Governor

Governor can be single operator wallet (or better multisignature wallet) at the start which should upon protocol settling be passed to DAO. To protect against the hostile transfer of Governance, such transfers are executed in two steps: first governance needs to declare `governance_update_after` time (it should be after some period of time) and then Governance can be transferred after that.&#x20;

####



