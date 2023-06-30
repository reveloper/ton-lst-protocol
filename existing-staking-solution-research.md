# Existing Staking Solution Research

## What are the Purposes of the TON LSt protocol?

* **Liquidity** - it is imperative to have accessibility to a liquid protocol that allows its tooling frameworks to be used for validation while also being able to serve other utilities in parallel.
* **Composability** - easy for using as a part of other dApps products and protocols.
* **Decentralized** - centralized validation models imply that only validators that create a liquidity pool are able to participate. The TON LSt Protocol makes it possible to eliminate this dependency by providing a more flexible connection between Stakers and Validators.
* **Small fish friendly** - minimize minimal staking deposits.
* **Empowering** - allows users to have their say in the development of the network via voting power.
* **Safe** - minimize risks of reducing funds as a result of incorrect validation work.
* **Investor diversification** - Stakers with large amounts of capital are able to more easily diversify their funds.

## Existing TON Staking Solutions

In TON, there are already existing solutions for staking pools, where nominators and validators are combined. At the moment, each of them has parts that are implemented excellently, as well as parts where there is room for improvement. Let's consider the main ones.

#### TON Nominators

**Pros:** supported voting for participants.&#x20;

**Cons:** large minimum deposits, illiquid, centralized validation.

#### TON Whales

**Pros:** Small entry deposits&#x20;

**Cons:** lack of a mechanism for influencing validator voting, illiquid, centralized validation.

#### bemo

{% hint style="info" %}
In active development, information could be outdated.
{% endhint %}

**Pros:** The smallest entry deposits and liquidity&#x20;

**Cons:** lack of a mechanism for influencing the voting of validators, centralized validation.



In the table below we’ll look at the requirements for each TON Liquid Staking participant.

### Comparing TON Nominator Pool, TON Whales nominator pool and bemo

<table><thead><tr><th width="133">Pool</th><th width="115">Voting</th><th width="123">Min deposit</th><th width="149">Liquidity</th><th>Decentralized  validation</th></tr></thead><tbody><tr><td>TON Nominator</td><td>Supported </td><td><strong>10000 TON</strong></td><td><strong>non-Liquidity</strong></td><td><strong>Absent</strong></td></tr><tr><td>TON Whales</td><td><strong>Absent</strong></td><td>50 TON</td><td><strong>non-Liquidity</strong></td><td><strong>Absent</strong></td></tr><tr><td>Bemo</td><td><strong>Absent</strong></td><td>1 TON</td><td>Liquidity supported with special stTON tokens</td><td><strong>Absent</strong></td></tr></tbody></table>

## Empowering Voters

Currently, network updates are voted upon by validators who sign the votes using their secret keys. However, this method doesn't allow for it to be enforced solely on-chain. This process could be changed in the future, in which case the Controller code used in the pool could be updated to better align the pool's voting with the will of the stakers.

Still, the Liquid Staking Pool does have mechanisms in place to incentivize alignment. Specifically, the pool's jetton, which essentially functions as a DAO jetton, has the ability to conduct polls. The poll results for a particular update have a predictable address, enabling validators who have borrowed funds from the pool to automatically ascertain the opinion of stakers on every proposal in the network. Once the stakers' opinions have been ascertained, each validator can determine whether to vote for or against the proposal. This determination is based on an algorithm that ensures the overall voting profile of all validators aligns closely with that of the stakers. Briefly, the algorithm involves:

1. Ordering all loans by controller address hash and arranging them in a line.
2. Dividing this line into two parts, 'FOR' and 'AGAINST', in proportion to the poll results.
3. For every controller, locate the position of the median of the given controller loan.
4. If the median of the loan falls in the 'FOR' part, the validator should vote for it, and vice versa.

The approver should monitor whether validators follow this algorithm and disapprove any that do not.

It's important to note that there's no 'default' voting option. For instance, if only 10% of stakers care enough to vote, with 70% voting for and 30% against a proposal, the pool's validators should vote in a ratio close to 70/30."
