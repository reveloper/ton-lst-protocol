# Validator Controller

## Overview <a href="#validator-controller" id="validator-controller"></a>

_Validator Controller smart contract_ (also _Controller_ in the following text) used by Validators to participate in Liquid Staking Pool actions during all validation cycles. Validators interact with their Controller through Wallet Smart Contract.



### Basics <a href="#basics" id="basics"></a>

The Controller may borrow funds from TON LS Pool while elections are open.

When borrowing, Controller agrees to pay a fixed premium for fund usage.

Validator Controller should return funds if not won in the election.

Validator Controller should return funds after funds release from the elector smart contract.

If the controller fails to return funds (when possible) during the grace period, anybody(any participant of pool Validator, Nominator, Governance) can trigger funds release (from Validator Controller to Pool) and get a premium.



### Deploy policy <a href="#deploy" id="deploy"></a>

* Validator deploys Validator Controller Smart Contract with its Wallet Smart Contract.
* &#x20;Controller contract must be deployed in masterchain.
* The Controller address depends on `[validator, pool, governor, halter, approver]`. If any of these addresses changed (rotation of Governor for instance), the validator needs to deploy a new controller (since the old one will not pass address-based authentication).
* The validator in most cases has two Validator Controllers (for even and odd validation rounds).

`sudoer` by default is `addr_none`.



Сontroller accounts for funds of the validator and funds borrowed from the Validation Pool. It can process deposits from Validator and from Validation Pool (later Pool). Upon request from the validator it can send a stake from it's balance to Elector. Upon request it can request withdrawal from Elector, but only after at least three updates of validator sets ([here](https://github.com/ton-blockchain/nominator-pool/blob/main/func/pool.fc#L566) is why it is necessary for the correct stake account).&#x20;

Thus Controller needs the ability to count validator set updates, for that purpose there is `update_set_hash` request. Both `withdrawal` and `update_set_hash` requests can be sent by a validator or, after the grace period, by anybody. In the latter case, the sender gets a bounty out of validator funds. This functionality protects against non-responding validators.

The Controller specifies maximal interest rate, and minimal and maximal TON credit size in borrow request. Validator can only request such parameters that it has interest plus recommended fine on it's balance. It can only request funds if is approved by Approver.

Upon receiving stake from Elector, Сontroller sends borrowed assets plus interest to the Validation pool.

Handlers of incoming messages

* deposit (only from Pool)
* count validator set update (from Validator or anybody after grace period)
* demand to request stake from Elector (from Validator or anybody after grace period)
* deposit Validator (only from Validator)
* withdraw Validator (only from Validator)
* demand to send stake to Elector (only from Validator)
* stake from Elector (only from Elector)
* Governance requests (from Governance, Halter, Sudoer)
* approve/disapprove (from Approver) bounces
* bounce of sent stake to Elector (only from Elector)

Outcoming messages:

* new\_stake (to Elector)
* request state (to Elector)
* Borrowing request (to Pool)
* Debt repayment (to Pool)
* Validator withdrawal (to Validator)

[Detailed docs on Validator controler](file:///C:/Users/AlexG/OneDrive/Desktop/TON%20Liquid%20Stake/docs/controller.md)

If Сontroller doesn't have enough assets to repay debt after stake recovery: halt Сontroller, and expect that Governance will "manually" decide what to do, for instance wait till validator replenish Сontroller or withdraw everything depending on conditions.

### &#x20;<a href="#deploy" id="deploy"></a>

### &#x20;<a href="#basics" id="basics"></a>



### State <a href="#state" id="state"></a>

Validator controller can be in the following states:

* Rest
* Sent borrowing request
* Sent stake request
* Staked funds
* Halted (by halter)

<figure><img src="../../.gitbook/assets/pool-graphs-Validator Controller states.drawio.svg" alt=""><figcaption><p>Validator Controller States</p></figcaption></figure>



Additionally, there is a flag on whether a credit is active or not for the current Validator Controller.

The controller may withdraw the validator's funds only from _Rest State_ and if`borrowed_amount=0`.

A controller can send borrowing requests only from the _Rest State_ state.

When Controller receives a response on a borrowing request it returns to _Rest State_. Note, that `borrowed_amount` and `borrowed_time` may be updated. `borrowed_time` only updates if it was `0`.

The controller can send stake requests only from the _Rest State_ state and only if `now() - borrowed_time < elections_start_before`. In other words, funds can be borrowed only to participate in the closest election.

When Controller receives a response on the _Stake Request_ it either passes to _Fund Staken_ or returns to the _Rest_ state (depending on the response).

The controller can send a request to withdraw stake from the elector only in _Fund Staken_ state.

Upon receiving stake withdrawal from the elector, if the controller has enough funds it should automatically repay debt. If not, it passes to _Halted_ state.

### Bounce management <a href="#bounce-management" id="bounce-management"></a>

<figure><img src="../../.gitbook/assets/pool-graphs-Bounce Management.drawio (1).svg" alt=""><figcaption><p>Bounce managemenet</p></figcaption></figure>

Validator Controller sends messages to Elector, Pool, Validator(wallet), Watchdogs

1. Elector

* New stake - bounce should be processed: `SENT_STAKE_REQUEST` -> `REST`
* Recover stake - should be processed

2. Pool

* loan repayment - bounced funds should be added to `borrowed_amount`, `borrowing_time` should be updated too, no change in state
* request loan - bounce should be processed: `SENT_BORROWING_REQUEST` -> `REST`

3. Validator

* withdrawal - sent in non-bouncable mode
* excesses - sent in non-bouncable mode

4. Watchdogs:

* reward - sent in non-bouncable mode

### Storage <a href="#storage" id="storage"></a>

1. Addresses (authorization for different actions)

* validator address
* validation pool address
* halter address
* sudoer address
* approver address

2. Round control (when it is allowed to stake money)

* `saved_validator_set_hash`
* `validator_set_changes_count`
* `validator_set_changes_time`
* `stake_held_for`

3. Funds control

* `borrowed_amount`
* `borrowed_time`
* state
* approval

### Validator duty <a href="#validator-duty" id="validator-duty"></a>

1. Validator must perform `update_validator_hash` when funds are staken. If validators doesn't update it during `GRACE` period, anybody can `update_validator_hash` and get `HASH_UPDATE_FINE`. The rule is as following:

> If controller is in "funds staken" state, `validator_set_changes_count < 3`, `borrowed_amount > 0` and `now() - utime_since > GRACE_PERIOD`, anybody can trigger `update_validator_hash` and get reward

2. Validator must recover funds from elector after it being released. Otherwise, anybody can `recover_stake` and get `STAKE_RECOVER_FINE`, in particular:

> If controller is in "funds staken" state, `validator_set_changes_count >= 2`, `borrowed_amount > 0` and `now() - validator_set_changes_time > stake_held_for + GRACE_PERIOD`, anybody can trigger `recover_stake` and get reward

3. If Validator didn't participated in election, he must return unused load. Otherwise, anybody can `return_unused_loan` and get `STAKE_RECOVER_FINE`, in particular:

> If controller is in "rest" state, `borrowed_amount > 0` and `utime_since > borrowed_time`, controller has enough funds on balance, anybody can trigger `return_unused_loan` and get reward
