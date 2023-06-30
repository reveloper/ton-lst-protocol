# Governor

### Overview

Governor can be single operator wallet (or better multisignature wallet) at the start which should upon protocol settling be passed to DAO.&#x20;

To protect against the hostile transfer of Governance, such transfers are executed in two steps: first governance needs to declare `governance_update_after` time (it should be after some period of time) and then Governance can be transferred after that.&#x20;



### Security Policy

It may be performed by a wallet, multisignature wallet, or DAO.&#x20;

### Deploy Policy

Governor could be deployed in the Basechain.

### Governance functions

1. set other roles in **Pool**, **Controller**, **Minters**
2. set some parameters (Governance fee) in **Pool**



