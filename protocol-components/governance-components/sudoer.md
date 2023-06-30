# Sudoer

### Overview

While initially empty, Sudoers gain the ability to send arbitrary messages from various parts of the system.&#x20;

It is a special role that is needed only in emergency situation or for critical updates. This mechanism should be used for fixing bugs and vulnerabilities. This role should be empty most of the time. There is also a quarantine after setting this role to protect against hostile transfer(expected to be 24 hours).

Can significantly upgrade the code or do minor fixes of the following components of  the TON LSt protocol:

* Pool smart contract&#x20;
* Controller smart contract

### Security Policy

Strict secure cold wallet, for example, Ledger.

### Deploy Policy

Sudoer smart contract could be deployed only by the Governance. Sudoer only becomes active if set more than _sudoer\_threshold_ seconds ago (expected to be 24h).&#x20;

Sudoer could be deployed in the Basechain.



