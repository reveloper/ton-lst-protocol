# GET Method Interfaces

## Pool

`var get_pool_full_data()` - return full Jetton Pool data (note, method depends on network configs and may not work via LS)

* `state` - `uint8` - internal state of the pool. Normal ones is `0`.
* `halted` - `bool` - whether the operation of the pool is stopped. `0` means the pool is working normally
* `total_balance` - `uint` - a sum of all TON accounted by Pool
* `supply` - `uint` - number of issued pool jettons
* `interest_rate` - `uint` - interest rate **per round** encoded as 24-bit number. In other words, after borrowing X TON validator need to return `X*(` 2\*\*24`+ interest_rate) /` 2\*\*24`TON`
* `optimistic_deposit_withdrawals` - `bool` - whether the optimistic (instantaneous) mode of deposit/withdrawals is on or is off
* `deposits_open?` - `bool` - whether deposits are open
* `saved_validator_set_hash` - `uint` - last accounted validator set hash
* `prev_round_borrowers` - `[cell, int, int, int, int, int, int]` - data of previous lending round
* `borrowers_dict` - `dict{controller_address_hash -> (Coins, Coins)}` - list of controllers that borrowed funds, body of credit and interest payment
* `round_id` - `int`
* `active_borrowers` - `int` - number of active borrowers
* `borrowed` - `int` - the total amount of TON borrowed in round
* `expected` - `int` - the total amount of TON expected to be returned in the round end
* `returned` - `int` - the total amount of TON already returned in round
* `profit` - `int` - total profit already received in round
* `current_round_borrowers` - `[cell, int, int, int, int, int, int]` - data of current lending round
* `min_loan_per_validator` - `int` - minimal amount of TON which can be borrowed by controller
* `max_loan_per_validator` - `int` - maximal amount of TON which can be borrowed by controller
* `governance_fee` - `int` - Share of pool profit which is sent to governance encoded as 24-bit number
* `jetton_minter` - `slice` - address of pool jetton root
* `supply` - `int` - the amount of issues pool jettons
* `deposit_payout` - `slice | null` - address of deployed deposit payout of current round (`null` if not deployed)
* `requested_for_deposit` - `int` - amount of TON requested for postponed (till the end of the round) deposit in this round
* `withdrawal_payout` - `slice | null` - address of deployed withdrawal payout of current round (`null` if not deployed)
* `requested_for_withdrawal` - `int` - the amount of pool jettons requested for postponed (till the end of the round) deposit in this round
* `sudoer` - `slice | addr_none` - address of sudoer role (`addr_none` by default)
* `sudoer_set_at` - `int` - timestamp when sudoer was set (quarantine is counted from this date)
* `governor` - `slice` - address of governor role
* `governance_update_after` - `int` - timestamp after which governor can be updated
* `interest_manager` - `slice` - address of interest manager role
* `halter` - `slice` - address of halter role
* `approver` - `slice` - address of approver role
* `controller_code` - `cell` - code of controller
* `pool_jetton_wallet_code` - `cell` - code of pool jetton wallet
* `payout_minter_code` - `cell` - code of payout
* `projected_total_balance` - `int` - the amount of TON expected to be accounted by the Pool at the end of the round
* `projected_supply` - `int` - number of expected issued pool jettons at the end of the round

The current pool jetton/TON ratio is equal to `total_balance/supply`. This ratio is used for immediate withdrawals in _optimistic_ mode.

The projected pool jetton/TON ratio is equal to `projected_total_balance/projected_supply`. This ratio is used for immediate deposits in _optimistic_ mode.

***

`(slice) get_controller_address(int controller_id, slice validator)` - returns address of the validator controller with the given id and validator address. Note controller maybe not yet deployed.

***

`(int, int) get_controller_address_legacy(int controller_id, int wc, int addr_hash)` - the same as previous but accepts parsed validator address and returns parsed controller address

***

`(int, int) get_loan(int controller_id, slice validator_address, int prev?)` - return loan body and load interest for a given controller

***

`(int, int) get_controller_loan_position(int controller_addr_hash, int prev?)` - We order all loans by controller address hash, put them in line and find a position of median of the given controller loan. This data can be used for deterministic voting: if stakers decide to vote in some proportion, we can check that controllers voted in the same proportion. Returns numerator and denominator.

## Controller

`var get_validator_controller_data()` - return full TON data

* `state` - `uint8` - internal state of the controller: `0` - REST, `1` - SENT\_BORROWING\_REQUEST, `2` - SENT\_STAKE\_REQUEST, `3` - FUNDS\_STAKEN, `4` - SENT\_RECOVER\_REQUEST, `5` - INSOLVENT
* `halted` - `bool` - whether the operation of the controller is stopped or not. `0` means the pool is working normally
* `approved` - `bool` - whether controller is approved and can send loan requests
* `stake_amount_sent` - `int` - the volume of last sent stake to the Elector
* `stake_at` - `int` - time of last sent stake to the Elector
* `saved_validator_set_hash` - `int` - latest known validator hash
* `validator_set_changes_count` - `int` - number of validators set updates since stake was sent
* `validator_set_change_time` - `int` - time of the last validator set updates
* `stake_held_for` - `int` - period of stake unlock after the round end
* `borrowed_amount` - `int` - the amount of TON that should be returned to Pool
* `borrowed_time` - `int` - timestamp of when funds were borrowed
* `validator` - `slice` - address of validator wallet
* `pool` - `slice` - address of pool wallet
* `sudoer` - `slice` - address of the sudoer wallet
