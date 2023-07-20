# Controller

## Overview <a href="#validator-controller" id="validator-controller"></a>

_Controller smart contract_ (also _Validator Controller_) used by Validators to participate in Liquid Staking Pool actions during all validation cycles. Validators interact with their Controller contract through the Wallet contract.



### Basics <a href="#basics" id="basics"></a>

Controllers may borrow funds from TON Liquid Staking Pool while validator elections are open.

When borrowing, the Controller agrees to pay a fixed premium on fund usage.

Validator Controller should return funds if not won in the election.

Validator Controllers should always return funds if they are not won in the election.

If the Controller fails to return funds (when possible) during the grace period, anyone (any pool participant including Validators, Stakers, and Governors) can trigger a fund release (from a Validator Controller to the corresponding Pool) to obtain a premium.



### Deployment policy <a href="#deploy" id="deploy"></a>

* Validators deploy Validator Controller smart contracts via their Wallet smart contract.
* The Controller contract MUST be deployed in the Masterchain.
* The Controller address depends on `[validator, pool, governor, halter, approver]`. If any of these addresses changed (rotation of Governor for instance), the validator needs to deploy a new controller (since the old one is no longer able to pass its address authentication).&#x20;
* The validator in most cases has two Controllers (one for even validation rounds and one for odd validation rounds).



### Description

Сontrollers account for validator funds and funds borrowed from the Validation Pool. They are able to process deposits from Validators and Validation Pools (with Pool deposits processed after validator deposits).&#x20;

Upon Validator request, Controllers are able to send some tokens (which make up part of the Validator’s stake) from their balance to an Elector, which is then able to request a withdrawal from the Elector. This is only possible after at least three validator sets are updated

{% hint style="info" %}
[Here](https://github.com/ton-blockchain/nominator-pool/blob/main/func/pool.fc#L566) is the reason it is necessary to ensure the correct stake account.
{% endhint %}



Therefore, Controllers must be able to count validator set updates, which is accomplished by making use of the `update_set_hash` request. Both `withdrawal` and `update_set_hash` requests can be sent by a validator, or after the grace period, by any network participant. In the latter case, the sender receives a bounty from the capital held inside the validator. This functionality protects against non-responding validators.

The Controller specifies a maximum interest rate and minimum and maximum TON credit size via a borrowing request. The Validator can only request such parameters when its balance shows interest plus the recommended fine, while it is only able to request funds if approved by the Approver.



Upon receiving stake from Elector, Сontroller sends borrowed assets plus interest to the Validation pool.



### Message Processing

#### Handlers of incoming messages

* deposit (only from a Pool)
* count validator set update (from Validator or anybody else after the grace period)
* demand to request stake from Elector (from Validator or anybody else after the grace period)
* Validator deposit (only from a Validator)
* Validator withdrawal (only from a Validator)
* demands sending stake to an Elector (only from a Validator)
* stake from the Elector (only from Elector)
* Governance requests (from Governor, Halter, Sudoer)
* approve/disapprove (from Approver) bounces
* bounce stake is sent to Elector (only from Elector)

#### Outcoming messages:

* new\_stake (to an Elector)
* request state (to an Elector)
* Borrowing request (to a Pool)
* Debt repayment (to a Pool)
* Validator withdrawal (to a Validator)

If the Validator Сontroller doesn't have enough assets to repay the debt after stake recovery the Controller is halted. Then typically, Governors will manually decide what the next steps are, for instance, whether they’ll wait till the validator replenishes the Сontroller’s balance or withdraws the balance depending on specific conditions.



### Controller States <a href="#state" id="state"></a>

A Controller can be in the following states:

* Rest
* Sent borrowing request
* Sent stake request
* Staked funds
* Halted (by halter)

<figure><img src="../../.gitbook/assets/pool-graphs-Validator Controller states.drawio.svg" alt=""><figcaption><p>Validator Controller States</p></figcaption></figure>



Additionally, a flag that interacts with Validator Controller states is used to determine whether a credit is active or not active for the current Controller.

A controller may withdraw the validator's funds only from the _Rest State_ if the`borrowed_amount=0`.

A controller is able to send borrowing requests only when it's in _Rest State_.

When a Controller receives a response pertaining to a borrowing request it returns to the _Rest State_. Note: after this process is completed, the `borrowed_amount` and `borrowed_time` may be updated (with the `borrowed_time` only being updated if it was previously at `0`).

The controller can send stake requests only from the Rest State state and only the `now() - borrowed_time < elections_start_before`. In other words, funds can only be borrowed for participation in the closest elections.

When a Controller receives a response on a pending Stake Request it is either passed to the _Fund Staked_ state or returned to the _Rest State_ (depending on the response received).

The Controller can send a request to withdraw stake from the Elector only in the _Fund Staked_ state.

Upon receiving withdrawn stake from the elector, if the controller has enough funds it should automatically repay the debt. If it is unable to pay the debt, it passes the debt to the _Halted state._

### Bounce management <a href="#bounce-management" id="bounce-management"></a>

<figure><img src="../../.gitbook/assets/pool-graphs-Bounce Management.drawio (1).svg" alt=""><figcaption><p>Bounce managemenet</p></figcaption></figure>

The Controller sends messages to Elector, Pool, Validator(wallet), Watchdogs (users noticed Controller’s payment delay)

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
* `state`
* `approval`

### Validator duty <a href="#validator-duty" id="validator-duty"></a>

1. Validators must perform the `update_validator_hash` function when funds are staked inside the node. If validators don't update this parameter during the `GRACE` period, any network participant can update\_validator\_hash and receive the `HASH_UPDATE_FINE`. This rule is defined in the following manner:

> If controller is in Funds Staked state, `validator_set_changes_count < 3`, `borrowed_amount > 0` and `now() - utime_since > GRACE_PERIOD`, meaning any network participant is able to trigger`update_validator_hash` and receive the corresponding reward.

2. Validators must recover funds from Elector after the funds are released (as noted directly above). Otherwise, a participant can `recover_stake` and receive a `STAKE_RECOVER_FINE`, in particular:

> If controller is in Funds Staked state, `validator_set_changes_count >= 2`, `borrowed_amount > 0` and `now() - validator_set_changes_time > stake_held_for + GRACE_PERIOD`, anybody can trigger `recover_stake` and get reward

3. When Validators don’t participate in a validator election round, they must return their unused loan. Otherwise, participants are able to carry out the `return_unused_loan` function and receive the `STAKE_RECOVER_FINE`, in particular:

> If controller is in Rest state, `borrowed_amount > 0` and `utime_since > borrowed_time`, controller has enough funds on balance, anybody can trigger `return_unused_loan` and get reward
