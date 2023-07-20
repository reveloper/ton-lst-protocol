# Pools

### Deployment Policy <a href="#deploy" id="deploy"></a>

* Pool smart contracts can be deployed by anyone using the TON Network
* Pool smart contracts can be deployed in the Basechain
* The _Pool Jetton minter_ smart contracts are deployed separately
* Pool deploys _Payout minters_ and initiates them.&#x20;
* The address of _Pool Jetton wallet_ smart contract for deposit payout is calculated on Pool and passed to Deposit Payout in init message.

### **Security Policy**

* The Pool smart contract could be stopped(paused) with Halter.
* Pool Smart Contract could be manually restored after it was stopped by Governance via Governor.

### Pool Workflow

The staking pool workflow makes use of several participants that allow the system to work correctly at all times. The sequential ordered system is conducted as follows:



#### General workflow

<table><thead><tr><th width="148">Interraction</th><th>Action</th></tr></thead><tbody><tr><td>Controller - Pool </td><td> Assets are lent to a Сontroller upon receiving a borrow request in accordance with the current lending rate</td></tr><tr><td>Controller - Pool  </td><td>Assets are received and the correct profit/loss data is aggregated through a Сontroller</td></tr><tr><td>Staker - Pool </td><td>Manages deposits and withdrawals</td></tr><tr><td>Interest Manager - Pool</td><td>Sends aggregate lending round statistics </td></tr><tr><td>Interest Manager - Pool</td><td>Updates interest upon request from Interest Manager (each round)</td></tr><tr><td>Interest Manager - Pool</td><td>Interest Manager sends round profits share</td></tr><tr><td>Governor - Pool</td><td>Governors update specific parameters when requested:<br>deposit params (open?, optimistic?), roles (halter, sudoer, interest_manager, governor), state (unhalt)</td></tr></tbody></table>



**Validators interaction workflow**

* Processing lending requests from Validator approvals whereby there is enough capital available and the request is commensurate with the desired rate and funding limit. Saves to the _list_ _of_ _active controllers_ (it is expected that there will be no more than hundreds of those).
* Receiving debt repayments from Validator Controllers (which removes them from the _list of active controllers_).
* Account for fees: send governance fee to the Governance.
* Aggregate profit and loss data for each consensus round (denominated using the Pool Jetton/TON ratio) and send the corresponding data to Interest Managers.

**Stakers interaction workflow**

* The pool keeps track of the ratio of Pool Jetton/TON.
* Receiving deposits from Stakers and minting deposits and Pool Jettons on their behalf.
* Receiving Pool Jettons burn notifications (withdrawal requests) from Stakers wallets and minting Stakers wallet withdrawals (of TON) or reverting Pool Jettons burns.
* Keeping track of current round Payout overall sums (for Withdrawals and Deposits).

On aggregation event (lending round end):

* minting Pool Jetton through Pool Jetton minter and pass them to _Deposit Payout_ minter to carry out the distribution
* sending TON to the _Withdrawal Payout_ minter to fulfill withdrawals

### Message Processing

#### Handlers of incoming messages

* borrow request (only from Сontroller)
* Governance requests (from Governors, Halter, Sudoer)
* debt repayment (only from Controller in the _active controller list_)
* deposits (from any user)
* burn notifications (from Pool Jetton wallets) bounces

#### Outcoming messages:

* deposits sent via a Controlle (which are inserted into an active controller list)
* aggregates profit notifications (to Interest Managers)
* fees that are paid to contribute to protocol Governance
* minting Pool Jettons (which are sent to a Deposit Payout and Nominator)
* sending TON (which are sent to a Withdrawal Payout and Nominator)



### Storage <a href="#storage" id="storage"></a>

* `state`
* `total_balance` - The current total balance of TON. It is updated whenever deposits, withdrawals, or profit executions occur.
* `interest_rate` - surplus of credit that should be returned with credit body. Set as integer equal share of credit volume multiplied by 2\*\*24
* `optimistic_deposit_withdrawals?` - flag declares whether optimistic deposit and withdrawal mode is enabled
* `deposits_open?` - flag declares whether deposits are open
* `current_round_borrowers` - current _round\_data_
  * `borrowers` - dict of borrowers: `controller_address -> borrowed_amount`
  * `round_id`
  * `active_borrowers` - number of borrowers that haven’t returned a loan yet
  * `borrowed` - amount of borrowed TON (no interest)
  * `expected` - amount of TON expected to be returned (`borrowed + interest`)
  * `returned` - amount of already returned TON
  * `profit` - currently obtained profit (at the end of the round should be equal to `returned-borrowed` and `expected-borrowed`)
* `prev_round_borrowers` - Previous _round\_data_
  * `borrowers` - dict of borrowers: `controller_address -> borrowed_amount`
  * `round_id`
  * `active_borrowers` - number of borrowers that haven’t returned a loan yet
  * `borrowed` - amount of borrowed TON (no interest)
  * `expected` - amount of TON expected to be returned (`borrowed + interest`)
  * `returned` - amount of TON already returned
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
* **Codes** - a code of child contracts needed for deployment or address authorization
  * `controller_code` - needed for controller authorization
  * `payout_code` - needed for Deposit and Withdrawal Payout deployment
  * `pool_jetton_wallet_code` - needed to calculate the Deposit Payout wallet address

### &#x20;<a href="#deploy" id="deploy"></a>



###
