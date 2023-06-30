# Interest Manager

### Overview

The interest Manager defines the interest rate at which the pool lends funds to the validator. Due to designing it is important to determine rates in such a way that the proposed rate can cover the cost of the validator, taking into account, that often the maintenance of the validator hardware is paid in fiat equivalent.

Due to the Interest Manager working fast with dynamic information Governance fees collects with this component.

Interest Manager is currently represented as a Wallet which is connected with an off-chain service that analyzes blockchain and validation stats related to the liquid pool with its own Wallet.&#x20;

At any moment it could be(and will be) improved to the contract-based on-chain system with the same functionality, that has its central node a Wallet or Multisignature Wallet or DAO.
