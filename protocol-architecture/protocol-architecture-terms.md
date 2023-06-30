# Protocol Architecture Terms

## Terms

* **Validator**: Validator nodes are actors that intend to participate in elections via their wallet contract (and if elected, validate new blocks on the network). Validators manage their own private validator key and also partially control their own Сontrollers (giving them the ability to send and receive stakes without the ability to withdraw the entirety of their funds independently).
* **Nominator**: An actor who has TON and wants to lend them on interest. Through the TON liquid staking protocol, they deposit Toncoins with the intention of lending them within a TON Liquid Staking Pool. Nominators communicate with the Liquid Staking Pool through their personal wallet via the wallet’s smart contract.
* **Stakers**: Stakers, also known as Nominators, are TON holders who stake their funds in a pool to be used for validation. In other words, these are users who _nominate_ a pool to exercise their validation rights. In the protocol to avoid confusion all Nominators will be named as Stakers(and sometimes Users).
* **Governance** - actors, that are represented by different roles in the protocol. All roles could be handled from DAO or Multisig Governance node.
* **DAO:** a Decentralized Autonomous Organization that (in this case) manages the platform’s liquid staking protocol by determining key parameters (e.g., setting fees, interest rates, etc.) through governance token (DAO jettons) voting power. The DAO also accumulates protocol service fees which are allocated towards research, development, liquidity incentives, and protocol upgrades that enhance the greater TON Ecosystem.
* **Elector** - is a system smart contract that accepts stakes, contributes to Validator election, decides the ordering of active validator sets, and distributes validation rewards.
* **Validator Сontroller (Controller)** - a specialized smart contract that manages Validator staking.
* **Wallet** - basic smart contracts used for the secure management of funds on TON.
* **Jettons**: scalable tokens on TON Blockchain adhering to TON-specific TEP-74 and TEP-89 token standards. Actor Jetton ownership is defined via the Jetton Wallet Contract managed by the actor's Wallet Smart Contract.
* **NFTs:** non-fungible tokens (NFTs) operating on TON Blockchain adhering to TON-specific TEP-62 and TEP-64 NFT token standards. Actor NFT ownership is defined via the NFT Item Smart Contract managed by the actor's Wallet Smart Contract.
* **Pool**: specialized smart contracts that present themselves as a liquidity pool. Pools securely manage liquid staking for Nominators and Validators and other network participants
* **Poll Jetton**: liquid staking Jettons (receipt tokens) representing native TON token shares that are minted and distributed among liquidity pool participants for measuring the value of staked funds. It implements polls functionality, which allows holders to express their opinion (in particular on network updates) in a decentralized way and then force this decision on validators.
* **Payouts**: Special tokens (as main version - NFTs), that are used for processing delayed payouts: deposits or withdrawals which will be processed at the end of the current round.

