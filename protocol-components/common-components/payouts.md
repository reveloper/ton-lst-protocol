# Payouts

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
