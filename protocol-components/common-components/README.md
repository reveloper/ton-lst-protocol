---
description: Circulation of tokens in Pool
---

# Common Components

### Overview

In order to properly conduct equitable token circulation within a TON Liquid Staking liquidity pool, Governance prepares special tokens using minter smart contracts, then these tokens will be distributed and circulated between Pool participants (Governors, Validators, Stakers) according to the defined workflow in Pool. In this way, clear visibility of the distribution of funds and expected rewards will be maintained.

### Optimistic deposits and withdrawals

By default, it is assumed that the ratio pool jetton/TON is unpredictable since validators can be slashed and thus it is impossible with 100% probability to say how much the pool balance will change on the round end. If, indeed, the ratio can fluctuate both ways (to increase and to decrease) then we need to postpone all deposits and withdrawals and process them immediately on the round end. Otherwise, if one knows that pool validators will be severely slashed and withdraw funds before that, he will avoid loss by distributing it to other holders.

However, since protocol ensures that the validator has enough funds to pay expected fines and credit interest is agreed on during loan granting, under normal conditions the amount of returned TON on the round end is determined and thus it is possible to calculate the projected pool jetton / TON ratio.&#x20;

Given that it is possible to process deposits and withdrawals in optimistic mode: deposits should be converted to pool jetton based on the projected ratio at the end of the round, and withdrawals should be converted to TON based on the current ratio (like this funds do not work in the round). This optimistic mode should be activated only if there are measures to protect against validator cheating attempts: for instance, the validator fully disclose himself.



#### Fill or Kill and Immediate withdrawals <a href="#fill-or-kill-and-immediate-withdrawals" id="fill-or-kill-and-immediate-withdrawals"></a>

If optimistic mode is activated, withdrawals often can not be processed immediately if the pool has not enough TONs. In this case Withdrawal bill should be minted which may not be optimal for the staker. At the same time, sometimes the staker wants to wait till the end of the round to get profit of this round even if optimistic mode is on. To control this behavior there are two flags used in conjunction with burn requests. These are:



* `wait_till_round_end` - if `true` Withdrawal bill will be minted regardless of the possibility to make an immediate withdrawal
* `fill_or_kill` - if `true` and there is not enough TON, burn will be reverted via minting pool jettons back.



\
