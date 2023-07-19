---
description: Circulation of tokens in Pool
---

# Common Components

### Overview

In order to properly conduct equitable token circulation within a TON Liquid Staking liquidity pool, Governance prepares special tokens using minter smart contracts, then these tokens will be distributed and circulated between Pool participants (Governors, Validators, Stakers) according to the defined workflow in Pool. In this way, clear visibility of the distribution of funds and expected rewards will be maintained.

### Optimistic deposits and withdrawals

By default, it is assumed that the Pool Jetton to TON ratio is unpredictable since validators can be slashed and thus it is impossible with 100% probability to say how much the pool balance will change on the round end. If, indeed, the ratio can fluctuate both ways (to increase and to decrease) then we need to postpone all deposits and withdrawals and process them immediately on the round end. Otherwise, if one knows that pool validators will be severely slashed and withdraw funds before that, he will avoid loss by distributing it to other holders.

However, since protocol ensures that the validator has enough funds to pay expected fines and credit interest is agreed on during loan granting, under normal conditions the amount of returned TON on the round end is determined and thus it is possible to calculate the projected pool jetton / TON ratio.&#x20;

Given that it is possible to process deposits and withdrawals in optimistic mode: deposits should be converted to pool jetton based on the projected ratio at the end of the round, and withdrawals should be converted to TON based on the current ratio (like this funds do not work in the round). This optimistic mode should be activated only if there are measures to protect against validator cheating attempts: for instance, the validator fully discloses himself.



#### Fill or Kill and Immediate withdrawals <a href="#fill-or-kill-and-immediate-withdrawals" id="fill-or-kill-and-immediate-withdrawals"></a>

If optimistic mode is activated, withdrawals often cannot be processed immediately if the pool doesn’t hold enough TON. If this does occur, the withdrawal bill should be minted (which is not always optimal for the Staker). At the same time, sometimes the Staker chooses to wait till the end of the validation round to retrieve its profit even if optimistic mode is turned on. To control this behavior two different flag types are used that are related to burn requests, including:



*   `wait_till_round_end` - if `true`&#x20;

    withdrawal bill will be minted regardless of possibility to make immediate withdrawal
* `fill_or_kill` - if `true` and there is not enough TON, burn will be reverted via minting pool jettons back.



\
