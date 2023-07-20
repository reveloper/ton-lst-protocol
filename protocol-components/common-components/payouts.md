# Payouts

### Payouts <a href="#payouts" id="payouts"></a>

{% hint style="info" %}
**NFTs:** non-Fungible Tokens on TON Blockchain implemented according to TEP-62 and TEP-64 standards. Ownership of NFTs for actors is defined by an instance of NFT Item Smart Contract which is managed by the actor's Wallet Smart Contract.
{% endhint %}

_Payouts Tokens_ -  a value defined by the ratio of Pool Jetton to TON.  This metric is updated once per validation round. In strict mode, it is assumed that this ratio is not known till the end of the validation round, meaning that actual deposits and withdrawals should be postponed till the end of the corresponding round.&#x20;

Nonetheless, in optimistic mode (when deposits and withdrawals are processed via a projected ratio) withdrawals often cannot often be made if the pool doesn't hold enough TON. Therefore deposits and withdrawals are special contracts that represent deposits and withdrawals being processed. These specialized smart contracts can be implemented as NFTs or Jettons so all wallets are able to interact with them seamlessly.

Deposits and withdrawals that are postponed till the end of a validation round can be represented on-chain in different forms, with the two main forms being NFTs and Jettons.&#x20;

#### Payouts NFT <a href="#payout-nft" id="payout-nft"></a>

{% hint style="info" %}
**In the current implementation, it is the main used mechanism**
{% endhint %}

In this scheme Payout is NFT Collection and "conversion obligation" is an NFT Item. Each round new Payout Collections contracts for Deposit and Withdrawal are created.

When TON is deposited into a liquidity pool, a deposit bill is immediately received. After the current validation round ends and funds are released from the Elector contract, the correct Pool Jetton to TON ratio is defined. This means that the amount of Jetton Pool corresponding to the total deposit value is calculated and sent to the Deposit Collection.

After this process is completed, the Deposit Collection sends a burn request to the last minted NFT which triggers the conversion of that NFT and simultaneously sends the burn request to the prior NFT. Here the idea is that NFTs are linked to lists and used to iterate through the entire collection.

This implementation allows processed deposits and withdrawals to be sent to other users as a single entity, allowing for autoconversion to the desired assets when needed.



#### Payout Jettons <a href="#payout-jettons" id="payout-jettons"></a>

{% hint style="info" %}
This payout scheme is implemented in contract/awaited\_minter/, however, it is not currently used and requires additional tests before using.
{% endhint %}

In this scheme Payout is jetton minter and "conversion obligation" is jettons.&#x20;

When TON is deposited into a liquidity pool, the immediate result is a Deposit jetton.

After the current validation round ends and funds are released from the Elector contract, the correct Pool Jetton to TON ratio is defined. This means that the amount of Pool Jetton corresponding to the total deposit value is calculated and sent to the Deposit minter. After this is carried out, Deposit Jettons can be burned to retrieve Pool Jettons from the minter.

The staker may burn the tokens themselves, however, for convenience, a special _Consigliere_ Role is introduced which has permission to call a potential token burn from any payout jetton wallet. If successful (meaning the distribution has already begun), the user receives their pool jettons and the consigliere initiates reimbursement. That way from the staker's perspective, a few hours after the deposit is initiated, the staker automatically receives their pool jettons.

This implementation allows processed deposits and withdrawals to be split and sent to other users in the correct denominations. However, it requires either a centralized consigliere or an action from the user to convert the payout to an actual asset when desired.

