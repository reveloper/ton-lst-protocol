# Pools

### Deploy Policy <a href="#deploy" id="deploy"></a>

* Pool smart contracts can be deployed by anyone using the TON Network
* Pool smart contracts can be deployed in the Basechain
* The _Pool Jetton minter_ smart contracts are deployed separately
* Pool deploys _Payout minters_ and initiates them.&#x20;
* The address of _Pool Jetton Wallet smart contract_ for deposit payout is calculated on Pool and passed to Deposit Payout in init message.

### **Security Policy**

* The pool smart contract could be stopped(paused) with Halter.
* Pool Smart Contract could be manually restored after it was stopped by Governance via Governor.

### Pool Workflow

The staking pool workflow makes use of several participants that allow the system to work correctly at all times. The sequential ordered system is conducted as follows:

* Interact with Controllers
  1. Lends assets to Сontrollers upon borrow request from **Сontroller** in accordance to _Current Rate_
  2. Receives assets and aggregates profit/loss information from **Сontrollers**
* Interaction with Stakers&#x20;
  3. manages deposits and withdrawals
* Interaction with Interest Manager:
  4. sends aggregate lending round statistics
  5. updates interest upon request from Interest Manager
* Interact with Governor:
  6. sends profits share
  7. updates parameters upon request: deposit params (open?, optimistic?), roles (halter, sudoer, interest\_manager, governor), state (unhalt).

### Pool functions

**Controller part**

Process lending requests from Validator Approvals: send funds if there are enough funds and request fits rate and limits. Saves to _active controller list_ (it is expected that there will be no more than hundreds of those).

Receives debt repayment from Validator Controlers: remove them from the list of _active controllers_

Account for fees: send governance fee to the Governance.

Aggregates profit/loss data for each round, ratio of Pool Jetton_/_TON, sends stats to Interest Manager

**User part**

Keep track of the ratio of Pool Jetton/TON.

Receives deposits from nominators and mints _Deposit_/_Pool Jetton_ for them.

Receives Pool Jetton burns notifications (withdrawal requests) from nominators' wallets and mints _Withdrawal_/_TON_ for them or revert burn.

Keep track of sums of the **current round** payouts (Withdrawals/Deposits).

On aggregation event (lending round end):

* mints pool jetton to _Deposit Payout_ minter for distribution
* sends TONs to _Withdrawal Payout_ minter to fulfill withdrawals

### Message Processing

#### Handlers of incoming messages

* borrow request (only from Сontroller)
* Governance requests (from Governance, Halter, Sudoer)
* debt repayment (only from Controller in _active controller list_)
* deposits (from any user)
* burn notifications (from pool jetton wallets) bounces

#### Outcoming messages:

* deposit to controller (to Controller, insert into _active controller list_)
* aggregated profit notification (to Interest Manager)
* Fees to Governance
* mint pool jetton (to Deposit Payout and nominator)
* TONs (to Withdrawal Payout and nominator)

<figure><img src="../../.gitbook/assets/pool-graphs-Pool Process.drawio.svg" alt=""><figcaption><p>Pool internal message processing(temporary version)</p></figcaption></figure>



### Storage <a href="#storage" id="storage"></a>

* `state`
* `total_balance` - amount of TONs accounted when deposit, withdraw and profit
* `interest_rate` - surplus of credit that should be returned with credit body. Set as integer equal share of credit volume times 2\*\*24
* `optimistic_deposit_withdrawals?` - flag notifies whether optimistic mode is enabled
* `deposits_open?` - flag notifies whether deposits are open
* `current_round_borrowers` - Current _round\_data_
  * `borrowers` - dict of borrowers: `controller_address -> borrowed_amount`
  * `round_id`
  * `active_borrowers` - number of borrowers that didn't return loan yet
  * `borrowed` - amount of borrowed TON (no interest)
  * `expected` - amount of TON expected to be returned (`borrowed + interest`)
  * `returned` - amount of already returned TON
  * `profit` - currently obtained profit (at the end of the round should be equal to `returned-borrowed` and `expected-borrowed`)
* `prev_round_borrowers` - Previous _round\_data_
  * `borrowers` - dict of borrowers: `controller_address -> borrowed_amount`
  * `round_id`
  * `active_borrowers` - number of borrowers that didn't return loan yet
  * `borrowed` - amount of borrowed TON (no interest)
  * `expected` - amount of TON expected to be returned (`borrowed + interest`)
  * `returned` - amount of already returned TON
  * `profit` - currently obtained profit (at the end of the round should be equal to `returned-borrowed` and `expected-borrowed`)
* `min_loan_per_validator` - minimal loan volume per validator
* `max_loan_per_validator` - maximal loan volume per validator
* `governance_fee` - share of profit sent to governance
* **Minters Data**
  * &#x20;Pool Jetton minter address
  * &#x20;Pool Jetton supply
  * Deposit Payout address
  * Deposit Payout supply == number of deposited TON in this round
  * Withdrawal Payout address
  * Withdrawal Payout supply == number of burned Pool Jettons in this round
* **Roles** addresses
  * sudoer
  * governance
  * interest manager
  * halter
  * approver
* **Codes** - a code of child contracts needed either for deployment or for address authorization
  * `controller_code` - needed for controller authorization
  * `payout_code` - needed for Deposit/Withdrawal payouts deployment
  * `pool_jetton_wallet_code` - needed for calculation of address of Deposit Payout wallet

### &#x20;<a href="#deploy" id="deploy"></a>



###
