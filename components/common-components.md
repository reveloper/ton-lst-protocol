---
description: Circulation of tokens in Pool
---

# Common Components

### Overview

In order to properly conduct equitable token circulation within a TON Liquid Staking liquidity pool, Governance prepares special tokens using minter smart contracts, then these tokens will be distributed and circulated between Pool participants (Governors, Validator, Nominator) according to the defined workflow in Pool. In this way, clear visibility of the distribution of funds and expected rewards will be maintained.

### Optimistic deposits and withdrawals

By default, it is assumed that the ratio pool jetton/TON is unpredictable since validators can be slashed and thus it is impossible with 100% probability to say how much the pool balance will change on the round end. If, indeed, the ratio can fluctuate both ways (to increase and to decrease) then we need to postpone all deposits and withdrawals and process them immediately on the round end. Otherwise, if one knows that pool validators will be severely slashed and withdraw funds before that, he will avoid loss by distributing it to other holders.

However, since protocol ensures that the validator has enough funds to pay expected fines and credit interest is agreed on during loan granting, under normal conditions the amount of returned TON on the round end is determined and thus it is possible to calculate the projected pool jetton / TON ratio.&#x20;

Given that it is possible to process deposits and withdrawals in optimistic mode: deposits should be converted to pool jetton based on the projected ratio at the end of the round, and withdrawals should be converted to TON based on the current ratio (like this funds do not work in the round). This optimistic mode should be activated only if there are measures to protect against validator cheating attempts: for instance, the validator fully disclose himself.



#### Fill or Kill and Immediate withdrawals <a href="#fill-or-kill-and-immediate-withdrawals" id="fill-or-kill-and-immediate-withdrawals"></a>

If optimistic mode is activated, withdrawals often can not be processed immediately if the pool has not enough TONs. In this case Withdrawal bill should be minted which may not be optimal for the nominator. At the same time, sometimes the nominator wants to wait till the end of the round to get profit of this round even if optimistic mode is on. To control this behavior there are two flags used in conjunction with burn requests. These are:



* `wait_till_round_end` - if `true` Withdrawal bill will be minted regardless of possibility to make immediate withdrawal
* `fill_or_kill` - if `true` iand there is not enough TON, burn will be reverted via minting pool jettons back.

### Pool Jettons

{% hint style="info" %}
Jetton scalable tokens on TON Blockchain implemented according to TEP-74 and TEP-89 standards. Ownership of Jetton for actors is defined by the instance of Jetton Wallet Contract which is managed by the actor's Wallet Smart Contract.
{% endhint %}

Pool Jetton -  is a jetton that is used to manage and track assets lent to the pool. It also has DAO voting capabilities to be used for voting for network config parameters.

Special TON token equivalent for liquid staking that gives users for staked TON in liquid staking pool. User can claim their reward in TON equivalent at any moment. Over time, the price of Pool Jetton grows relative to the cost of native TON, which acts as a motivation for holding tokens for a long time. This behavior(growing of the stTON price) is provided by the borrowing features in the TON LS Pool. The borrowing process is (lending funds from Pool for Election and Validation cycle) defines in Validator Controller smart contract.



### Payouts <a href="#payouts" id="payouts"></a>

{% hint style="info" %}
**NFTs:** non-Fungible Tokens on TON Blockchain implemented according to TEP-62 and TEP-64 standards. Ownership of NFTs for actors is defined by an instance of NFT Item Smart Contract which is managed by the actor's Wallet Smart Contract.
{% endhint %}

_Payouts Tokens_ -  is a value defined by the ratio of Pool Jetton to TON. It is updated once per round. In strict mode, we assume that this ratio is not known till the end of the round and thus actual deposits/withdrawals should be postponed till the end of the round. Besides, even in optimistic mode (when deposits/withdrawals are processed through projected ratio), withdrawals often can not be made if the pool has not enough TON. Thus Deposits/Withdrawals are special contracts that represent deposits/withdrawals in process. Can be implemented as NFTs or Jettons so all wallets will be able to interact with it.

Postponed till the end of the round deposits/withdrawals can be represented on-chain in different forms. Two main approaches are to use Jettons and NFT.

#### Payout NFT <a href="#payout-nft" id="payout-nft"></a>

{% hint style="info" %}
**In the current implementation, it is the main used mechanism**
{% endhint %}

In this Payout scheme is NFT collection and "conversion obligation" is an NFT. Each round new payout collections for deposit and withdrawal are created.

When you deposit TON to the pool you immediately receive a Deposit Bill. Later after the current validation round ends and funds are released from the Elector contract, the correct poolJetton/TON ratio is discovered, amount of Pool Jetton corresponding to the total deposits value is calculated and sent to Deposit Collection. After that the Deposit Collection sends a burn request to the last minted NFT which triggers the conversion of that NFT and simultaneously sends the burn request ton the NFT before that. Here the idea that NFTs are linked lists is used to iterate through the whole collection.

This implementation allows processed deposits/withdrawals to be sent to other users as a whole and allows autoconversion to assets when ready.



#### Payout jettons <a href="#payout-jettons" id="payout-jettons"></a>

{% hint style="info" %}
This payout scheme is implemented in contract/awaited\_minter/, however, it is not currently used and requires additional tests before using.
{% endhint %}

In this scheme Payout is jetton minter and "conversion obligation" is jettons. Each round new payouts for deposit and withdrawal are created.

When you deposit TON to the pool you immediately get Deposit jettons. Later after the current validation round ends and funds are released from Elector, the correct Pool Jetton/TON ratio is discovered, the amount of Pool Jetton corresponding to the total deposits value is calculated and sent to the Deposit minter. After that Deposit jettons can be burned to retrieve Pool Jetton from the minter.&#x20;

The user may burn it himself, however, for convenience a special consigliere role is introduced which has permission to call burn from any payout jetton wallet. If successful (that means if the distribution is already started). The user gets his pool jettons and consigliere reimbursement for fees. That way from the user's perspective, in a few hours after deposit she automatically gets a pool jetton.

This implementation allows processed deposits/withdrawals to be split and sent to other users by parts, however, it requires either centralized consigliere or action from the user to convert payout to an actual asset when ready.

\
