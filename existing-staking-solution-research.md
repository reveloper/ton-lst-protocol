# Existing Staking Solution Research

## What are the Purposes of the TON LSt protocol?

* **Liquidity** - it is imperative to have accessibility to a liquid protocol that allows its tooling frameworks to be used for validation while also being able to serve other utilities in parallel.
* **Composability** - Easy for using as a part of other dApps products and protocols.
* **Decentralized** - centralized validation models imply that only validators that create a liquidity pool are able to participate. The TON Liquid Staking Protocol makes it possible to eliminate this dependency by providing a more flexible connection between Nominators (participants with funds) and Validators (participants providing hardware).
* **Small fish friendly** - minimize minimal staking deposits.
* **Empowering** - allows users to have their say in the development of the network via voting power.
* **Safe** - minimize risks of reducing funds as a result of incorrect validation work.
* **Investor diversification** - participants with large amounts of capital are able to more easily diversify their funds.

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

##
