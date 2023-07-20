# Governor

### Overview

Governor can be single operator wallet (or better multisignature wallet) at the start which should upon protocol settling be passed to DAO. Governors DAOs is a jetton-based  DAO that makes use of their own governance jetton (GJ).

To protect against the hostile transfer of Governance, such transfers are executed in two steps: first governance needs to declare `governance_update_after` time (it should be after some period of time) and then Governance can be transferred after that.&#x20;



### Security Policy

Governors generally could be a regular (cold) wallet, multisignature wallet, or DAO.&#x20;

### Deployment Policy

Governor could be deployed in the Basechain.

### Governor functions

1. Responsible for setting set other roles(**Sudoer, Halter, Interest Manager**) in Pool, Controller, and Minters.
2. Responsible for setting various parameters (including governance fees) in Pool**.**



