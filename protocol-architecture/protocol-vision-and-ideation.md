# Protocol Vision and Ideation



### Workflow

#### User Deposit Optimistic

1. The user sends `deposit` message to Pool
2. Pool instantly requests Jetton Pool minter to mint new tokens for the user.
3. Jetton Pool Minter sends Poll Jettons to users Jetton Wallet

#### User Withdrawal Optimistic

1. The User burns his Jetton Pool.
2. User's Jetton Wallet sends notification about burn to Pool.
3. The pool immediately sends back to the user's wallet unstaked funds with a reward. If pool have no enough funds, user will get back Pool Jettons to his Jetton Wallet.



#### User Deposit Pessimistic

1. The user sent a deposit during round (1) to the pool address.
2. The user received a Payout NFT Item (deposit) in return, with the corresponding amount for staking.
3. At the end of round (1), the Pool sends start\_distribution, the user's NFT is burned (on behalf of the collection contract).
4. The user receives a Pool Jetton for coins staked in the pool during round (2).

#### User Withdraw Pessimistic

1. The user sends a burn to their Pool Jetton Wallet during round (1).
2. For the burned Pool Jettons, the Pool contract sends a request to mint Payout NFT (withdrawal) in the Payouts Collection (withdrawal).
3. The user received Payout NFT (withdrawal).
4. At the end of round (1), the Pool initiates start\_distribution, and the user's NFT is burned (on behalf of the contract of the Payouts Collection (withdrawal)).
5. The user receives all rewards for the round (1) and all preceding ones. Round (2) is launched without the participation of his funds.



#### Validator Request Loan

\
Before the start of elections, Validators can request a loan:

1. The Validator requests from the Controller the ability to take a loan, indicating the acceptable percentage and desired amount.
2. The Controller sends a request to the Pool with the corresponding parameters.
3. If the Pool has an available amount to loan at the specified percentage, the pool returns a confirmation to the controller, along with the issued loan.

After this, the Validator can send an application to participate in the elections through the Controller.

1. The Validator sends a request to the Controller to participate in the elections.
2. If the Controller has a borrowed amount, it is added and redirected to the elector in the application.



#### Loan Repayment

### Workflow protocol scheme&#x20;



### Optional features

* Optimistic deposits/withdrawals vs delayed deposits/withdrawals
* Delayed Jettons vs NFT bills
* Liquidity pool Jetton DAO - governance voting
* Governance Multisignature Wallet vs Governance DAO&#x20;

####
