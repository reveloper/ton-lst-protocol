# Message Processing

{% hint style="info" %}
Please note, bounced messages are omitted for a better understanding of basic processing.
{% endhint %}

### 1. Deposit

#### &#x20;1.1 Optimistic deposit (instant)

The user sends `deposit` op to Pool. Pool asks mint and sends to User pTON(Pool Jettons).

<figure><img src="../.gitbook/assets/1.1-deposit-optimistic.drawio.svg" alt=""><figcaption><p>Optimistic deposit</p></figcaption></figure>

#### 1.2 Pessimistic deposit (delayed)&#x20;



{% hint style="info" %}
&#x20;Payouts are NFTs currently but could be implemented as Jettons. NFT Payouts are represented by two collections Payout NFT deposit and Payout NFT withdrawal.
{% endhint %}

The user sends a `deposit` message to Pool. Pool requests Payout Collection NFT mint for the user a Payout NFT Item (deposit).&#x20;

<figure><img src="../.gitbook/assets/macschemes (2)-deposit_pessimistic_mint.drawio.svg" alt=""><figcaption></figcaption></figure>

The user is an owner of NFT, Pool of Payout NFT Collection.&#x20;

At the end of the round, Pool inited `start distribution`, all Payouts Items burn and the user gets Pool Jettons in exchange.

<figure><img src="../.gitbook/assets/macschemes (2)-deposit_pessimistic_payout_burn.drawio.svg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Optimistic/Pessimistic flag - global Pool config parameter&#x20;
{% endhint %}

### 2 - Withdraw

#### 2.1 - Optimistic withdrawal (Instant)

At any moment users could ask to withdraw their funds from Pool. The user asks to burn his pTON and get back TON amount including the reward.

If the Pool can not pay the requested sum currently pTON will be minted and returned to the User's Jetton Wallet.&#x20;



<figure><img src="../.gitbook/assets/new additional schemes-withdrawal-optimistic.drawio.svg" alt=""><figcaption><p>Optimistic Withdrawal (Kill_or_Fill = true)</p></figcaption></figure>

#### 2.2 Pessimistic withdrawal (Delayed)&#x20;

The user sends a "withdraw" message and gets Payout NFT Item (withdrawal).&#x20;

<figure><img src="../.gitbook/assets/macschemes (2)-withdrawal_pessimistic_payout_mint.drawio (1).svg" alt=""><figcaption></figcaption></figure>

Every round `pool_update()` is launched, which leads to burning Payout NFT Items (users' Payout NFT withdrawals) and converting them to TON or Pool Jettons. TON and pTONs distributed to the appropriate owner of the original NFT Payout.&#x20;

<figure><img src="../.gitbook/assets/macschemes (2)-withdrawal_pessimistic_payout_burn.drawio (1).svg" alt=""><figcaption></figcaption></figure>

If it is Pool Jetton user can ask to immediately withdraw TON in exchange for Pool Jettons he got for previous finalized validation rounds.&#x20;



### 3 Loan

A Controller asks Pool for a loan, with a message `request_loan` that contains desired interest and prepays amount for the loan.  The Controller asks the Pool for a loan. If this request is approved the answer msg `credit` is displayed and the controller marks the borrowing time and updates the borrowed\_amount on-chain.

<figure><img src="../.gitbook/assets/pool-3-request loan.drawio (1).svg" alt=""><figcaption></figcaption></figure>

### 4 Using borrowed amounts for staking&#x20;

A Validator sends a `new_stake` request to a Controller, the Controller sends a `new_stake` request to an Elector with a borrowed amount (where both opcodes are equal).

<figure><img src="../.gitbook/assets/4-Using loan.drawio.svg" alt=""><figcaption><p>Using borrowed sum for stake</p></figcaption></figure>

### 5 Loan Repayment&#x20;

#### 5.1 Regular repayment

When a regular loan repayment is requested, the Elector sends the recover\_stake\_ok request to the Controller. The Controller then sends a pool::loan\_repayment message with the value = to the MIN\_TONS\_FOR\_STORAGE + the borrowed\_amount.

<figure><img src="../.gitbook/assets/pool-3-loan repayment elector.drawio (1).svg" alt=""><figcaption></figcaption></figure>



#### 5.2 - Request by a Validator or an external participant

The sender (any contract) can request the Controller return borrowed sum with `controller::return_unused_loan` op. If it happens after `grace_period` time after the end of the round and the Sender is not a Validator of the corresponding Controller, the Controller will pay a fine to the sender.

<figure><img src="../.gitbook/assets/pool-3-loan repayment validator and other.drawio (1).svg" alt=""><figcaption></figcaption></figure>

#### &#x20;5.3 Forced repayment

When a forced loan repayment is requested, the Governor sends the `return_available_funds` message to the Controller. The Controller then sends the `loan_repayment` message with the correct `value` equals to the `min(borrowed, available_funds)`.&#x20;

If the Pool is able to find the correct borrower and value in its borrowers list, the loan is closed otherwise an error is thrown (so far the protocol has not implemented the logic for cases when the loan is not closed).

<figure><img src="../.gitbook/assets/pool-3-loan repayment governor.drawio (1).svg" alt=""><figcaption></figcaption></figure>
