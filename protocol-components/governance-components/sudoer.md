# Sudoer

### Overview

While initially empty, Sudoers gain the ability to send arbitrary messages from various parts of the system.&#x20;

Sudoers are generally empty by default and are used to send arbitrary messages from arbitrary portions of the system. They are used to significantly upgrade code within the protocol and to perform minor maintenance on the following components:

* Pool smart contracts
* Controller smart contracts

### Security Policy

Strict secure cold wallet, for example, Ledger.

### Deploy Policy

Sudoer smart contracts can be deployed only by Governors. Sudoer only becomes active if set more than _sudoer\_threshold_ seconds ago (expected to be 24h).&#x20;

Sudoer could be deployed in the Basechain.



