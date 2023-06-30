# Potential Risks

### Overview

The TON Liquid Staking protocol is sometimes susceptible to specific risks during operation, including:

* Protocol logic and implementation issues
* Misbehaving Validator slashing
* Misbehaving Validator voting
* TON Network errors (delayed election issues).

### Handling Bootstrapping and Centralization Risks

Initial protocol bootstrapping, in the particular deployment of new contracts, figuring out the optimal interest rates discovery mechanisms, way of governance token distribution, and desired approvement steps for validators which ensure safety, geographic decentralization, and profitability may be a tricky long-term task.&#x20;

Besides, in a rapidly changing ecosystem landscape, it may be necessary to adjust over time the incentives of external actors to maximize adoption.

To solve this problem, instead of relying on promises of future upgrades, we have adopted the approach of modular structure and role allocation with proper privileges assignment. In particular, in TON LSt we have Governor, Interest Manager, Approver, Halter and Sudoer roles. The last two can be (and should be in the long run) unset.

It is important, that after the protocol is set down is possible to forever switch off setting Sudoer or Halter or transfer Governance.&#x20;

One of the ways is to include corresponding actions to [dao-decisions-filter](https://github.com/EmelyanenkoK/jetton\_dao/blob/embedded-subcontracts/contracts/dao-decisions-filter.func).
