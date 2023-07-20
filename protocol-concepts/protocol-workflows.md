# Protocol Workflows



### Stakers Workflow

{% hint style="info" %}
Staker is a TON holder who stakes their funds in the Pool to be used for validation. In other words, these are users who _nominate_ a pool to exercise their validation rights.
{% endhint %}

#### Staker Deposit Optimistic

1. The Staker sends `deposit` message to Pool
2. Pool instantly requests Jetton Pool minter to mint new tokens for the Staker.
3. Jetton Pool Minter sends Poll Jettons to Stakers Jetton Wallet

#### Staker Withdrawal Optimistic

1. The Staker burns his Jetton Pool amount at any time during the validation round.
2. Skater's Jetton Wallet sends notification about burn to Pool.
3. The Pool immediately sends back to the Staker's wallet unstaked funds with a reward. If the pool has not have enough funds, Staker will get back Pool Jettons to his Jetton Wallet.



#### Staker Deposit Pessimistic

1. The Staker sent a deposit during the validation round to the pool address.
2. The Staker received a Payout NFT Item (deposit) in return, with the corresponding amount for staking.
3. At the end of the same round, the Pool sends `start_distribution` message, and the Staker's NFT is burned (on behalf of the collection contract).
4. The Staker receives a Pool Jetton in exchange for TON staked at the beginning of the next validation round.

#### Staker Withdraw Pessimistic

1. The Staker sends a burn to their Pool Jetton Wallet during the current round.
2. For the burned Pool Jettons, the Pool contract sends a request to mint Payout NFT (withdrawal) in the Payouts Collection (withdrawal).
3. The Staker received Payout NFT (withdrawal).
4. At the end of the current round, the Pool sends `start_distribution` message, and the Staker's NFT is burned (on behalf of the contract of the Payouts Collection (Withdrawal)).
5. The Staker receives all rewards for the last round and all preceding ones. The next round is launched without the participation of his funds.



### Validator Workflows

#### Validator Request Loan

\
Before the start of elections, Validators can request a loan:

1. The Validator requests from the Controller the ability to take a loan, indicating the acceptable percentage and desired amount.
2. The Controller sends a request to the Pool with the corresponding parameters.
3. If the Pool has an available amount to loan at the specified percentage, the pool returns a confirmation to the controller, along with the issued loan.

After this, the Validator can send an application to participate in the elections through the Controller.

1. The Validator sends a request to the Controller to participate in the elections.
2. If the Controller has a borrowed amount, it is added and redirected to the elector in the application.

#### Validator Loan Repayment

1. After the Validation cycle ends, the Elector returns the stake along with the reward to the Controller.
2. The Controller carries out the loan repayment in a `loan_repayment` message to the Pool.
3. When the last debt in the Pool is repaid, the round\_update() is carried out.&#x20;



### Optional Workflow Types

* Optimistic (instant) deposits/withdrawals vs Pessimistic (delayed) deposits/withdrawals
* NFT bills vs Delayed Jettons Payouts
* Liquidity pool Jetton DAO - governance voting
* Governor Multisignature Wallet vs Governor DAO&#x20;

####
